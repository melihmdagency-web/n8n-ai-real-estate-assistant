# KURULUM – Data Tables (OAuth yok)

n8n: http://localhost:5678 · Süre ~10 dk

## 1. Tabloları oluştur
n8n sol menü → **Overview** → **Data tables** sekmesi → **Create data table**.

**Tablo 1: `Listings`** (adı birebir böyle)
| Sütun | Tip |
|---|---|
| listing_id, city, address, type, intent, highlight | String |
| rooms, size_sqm, price | Number |

**Tablo 2: `Leads`**
| Sütun | Tip |
|---|---|
| name, email, timeline, financing, intent, viewing_slot | String |
| budget, score | Number |

(`id`, `createdAt`, `updatedAt` otomatik gelir, ekleme.)

## 2. listings.csv'yi içe aktar
- Tablo sayfasında **Import CSV** (veya ⋯ menüsü) varsa: `listings.csv` seç → sütunları eşle.
- Yoksa: tabloyu açıp **Add row** ile 12 satırı elle yaz/yapıştır. Hızlı yol: 12 satırı CSV'den kopyala, 3-4 ilanla başla (test için yeter; hepsi şart değil).

## 3. Workflow'u içe aktar
Yeni workflow → sağ üst **⋯ → Import from file** → `workflow.json`. (Eski Google Sheets sürümünü kullandıysan sil.)

## 4. Node'larda tabloları seç
**Search Listings**, **Save Lead**, **Confirm Viewing** node'larına çift tıkla → **Data table** alanı → listeden `Listings` / `Leads` seç (ilk ikisi zaten adla gelir; kırmızıysa yeniden seç). **Save**.

## 5. Groq + test
Groq Chat Model → Create new credential → API anahtarını yapıştır (console.groq.com/keys). **Open chat** → `KURULUM.md`'deki 3 test konuşmasını dene. Sonuçlar **Leads** tablosunda görünür.

**Sorun:** Tool hata verirse sütun adlarını (küçük harf, alt çizgi) kontrol et.
