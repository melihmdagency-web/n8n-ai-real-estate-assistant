# KURULUM – AI Real Estate Assistant (n8n)

Toplam süre: ~20 dk. n8n: http://localhost:5678

## 1. CSV'leri Google Sheets'e yükle
1. https://sheets.new aç → dosyaya ad ver: `Real Estate Demo`.
2. Alttaki sekmeyi **Listings** olarak yeniden adlandır (sağ tık → Yeniden adlandır).
3. **Dosya → İçe aktar → Yükle** → `listings.csv` → "Geçerli sayfanın yerine koy" → İçe aktar.
4. Alttaki **+** ile yeni sekme aç, adı **Leads**. Aynı şekilde `leads.csv` içe aktar (sadece başlık satırı gelir).
5. Tarayıcı adres çubuğundan Sheet ID'yi kopyala: `docs.google.com/spreadsheets/d/` **BU_KISIM** `/edit`.

## 2. Workflow'u içe aktar
1. n8n → sol üst **Create workflow** (veya Workflows → Add).
2. Sağ üstte **⋯** menüsü → **Import from file** → `workflow.json` seç.
3. Kanvasta 8 node görmelisin (Chat Trigger, Agent, Groq, Memory, 4 tool).

## 3. Groq kimlik bilgisi
1. https://console.groq.com/keys → **Create API Key** → kopyala. (Dosyaya yazma, sadece n8n'e yapıştır.)
2. Workflow'da **Groq Chat Model** node'una çift tıkla → Credential kutusu → **Create new credential**.
3. API Key alanına anahtarı yapıştır → **Save**. Yeşil "Connection tested successfully" çıkmalı.
4. Model: `openai/gpt-oss-120b`. Hata verirse listeden güncel bir model seç.

## 4. Google Sheets bağlantısı (localhost OAuth)
Bir kerelik Google Cloud ayarı:
1. https://console.cloud.google.com → yeni proje (`n8n-demo`).
2. **APIs & Services → Library** → "Google Sheets API" → **Enable**. (Drive API'yi de aç.)
3. **OAuth consent screen** → External → uygulama adı, e-postan → Kaydet. **Test users** kısmına kendi Gmail'ini ekle.
4. **Credentials → Create credentials → OAuth client ID** → Web application.
5. **Authorized redirect URIs** → ekle: `http://localhost:5678/rest/oauth2-credential/callback`
6. **Create** → Client ID ve Client Secret'i kopyala.

n8n'de:
1. **Search Listings** node'una çift tıkla → Credential → **Create new** → Client ID + Secret yapıştır → **Sign in with Google** → hesabını seç → "Devam" (uyarı çıkarsa Advanced → Go to app).
2. Aynı node'da **Document** → **By ID** → Sheet ID'ni yapıştır (`PASTE_YOUR_SHEET_HERE` yerine). Sheet: **Listings**.
3. **Save Lead** ve **Confirm Viewing** node'larında aynısını yap: credential'ı listeden seç, Document ID yapıştır, Sheet = **Leads**.
4. Sağ üst **Save**.

## 5. Chat'i test et
1. Canvas altında **Open chat** (veya **Test workflow**) → sohbet penceresi açılır.
2. "Hi" yaz. Asistan tek soru sormalı.
3. Her testten sonra Sheets'te **Leads** sekmesine bak.

## 6. Üç test konuşması

**A) Uygun alıcı** (beklenen: puan ≥ 60, 3 saat önerilir, seçilen saat Leads'e yazılır)
> I want to buy a 3-room apartment in Leipzig. → Budget 300000 → within 2 months → pre-approved mortgage → Anna Beispiel, anna.beispiel@example.com → "Wed 14:30"

**B) Bütçesi düşük alıcı** (beklenen: puan < 60, saat önerilmez, "ekip e-posta ile dönecek")
> I'd like to buy a house in Leipzig with 5 rooms. → Budget 80000 → no rush, maybe next year → no financing yet → Ben Muster, ben.muster@example.com

**C) Almanca konuşan alıcı** (beklenen: tüm cevaplar Almanca)
> Hallo, ich suche eine Mietwohnung in Leipzig mit 2 Zimmern. → Budget 1000 Euro → ab sofort → Clara Probe, clara.probe@example.com

## Sorun giderme
- **Boş cevap / tool hatası:** Node'daki Document ID yapıştırıldı mı? Sekme adları tam `Listings` / `Leads` mı?
- **Google "redirect_uri_mismatch":** 4. adım 5'teki URI birebir aynı olmalı.
- **Groq 429:** Ücretsiz limit; 1 dk bekle.
