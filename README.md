# Multilingual Technical Support Dataset (TR / EN)

Çok dilli, çok turlu **teknik destek diyalog veri seti**. Her örnek; bir son kullanıcı/müşteri ile teknik destek arasındaki tam bir sorun çözüm dizisini (issue thread), sorunun başlığını, ilgili ürün/uygulama kategorisini, duygu (sentiment) etiketini ve referans özetini içerir. Veri seti, kurumsal teknik destek senaryolarında **issue classification, title generation, sentiment analysis ve summarization** görevlerinin değerlendirilmesi için tasarlanmıştır.

> Veri yalnızca **değerlendirme (evaluation) ve few-shot prompt tasarımı** için kullanılmıştır; model eğitimi/fine-tuning için kullanılmamıştır.

---

## İçerik

| Dosya | Dil | Örnek sayısı | Marka/Kategori |
|-------|-----|--------------|----------------|
| `tr_data.json` | Türkçe | 106 | 13 |
| `en_data.json` | İngilizce | 235 | 26 |
| **Toplam** | — | **341** | — |

---

## Şema (her kayıt)

Her dosya, aşağıdaki alanlara sahip nesnelerden oluşan bir JSON dizisidir:

| Alan | Tip | Açıklama |
|------|-----|----------|
| `kaynak` | string | Verinin genel kaynağı (ör. Stack Overflow, Reddit, Microsoft, Teknoloji Forumu). Bireysel URL'ler gizlilik/normalizasyon için kaldırılmıştır. |
| `urun` | string | İlgili ürün/uygulama kategorisi → **issue classification** etiketi |
| `baslik` | string | Sorunu özetleyen başlık → **title generation** hedefi |
| `soru` | string | Kullanıcının ilk problem açıklaması |
| `cevaplar` | list | Diyalog turları; her biri `{ "cevap_metni": "..." }`. Çok turlu konuşma akışını temsil eder. |
| `cozum` | string | Kabul edilen / nihai çözüm |
| `sentiment` | string | Müşterinin sorudaki tonu: `negative`, `neutral`, `positive` → **sentiment analysis** etiketi |
| `reference_summary` | string | Sorun + çözümün kısa referans özeti → **summarization** hedefi |
| `language` | string | `tr` veya `en` |

Örnek:

```json
{
  "kaynak": "Stack Overflow",
  "urun": "Docker",
  "baslik": "My Docker image is 1.8GB, how do I make it smaller?",
  "soru": "My Node.js service image is 1.8GB ...",
  "cevaplar": [
    { "cevap_metni": "Three things give you most of the reduction. First, switch the base image ..." },
    { "cevap_metni": "Second, use a multi-stage build ..." }
  ],
  "cozum": "Three things give you most of the reduction. First, switch the base image ...",
  "sentiment": "neutral",
  "reference_summary": "My Docker image is 1.8GB, how do I make it smaller. Resolution: Three things give you most of the reduction.",
  "language": "en"
}
```

---

## Görev kapsamı

| Görev | İlgili alan | Türkçe | İngilizce |
|-------|-------------|--------|-----------|
| Issue Classification | `urun` | ✔ | ✔ |
| Title Generation | `baslik` | ✔ | ✔ |
| Sentiment Labels | `sentiment` | ✔ | ✔ |
| Reference Summaries | `reference_summary` | ✔ | ✔ |

---

## Veri seti dağılımı

| | Türkçe | İngilizce |
|--|--------|-----------|
| Örnek (Samples) | 106 | 235 |
| Marka/Kategori (Brands) | 13 | 26 |
| Dağılım (Distribution) | `10×10 + 3×2` | `9×15 + 15×6 + 2×5` |
| Dil | Türkçe | İngilizce |

### Marka / kategori kırılımı

**Türkçe (13):** Windows 11, iPhone, İnternet Erişimi, Android, Web ve Mağaza Alışverişlerinde Yaşanan Sorunlar, Ekran Kartı, Dizüstü Bilgisayarlar, Oyun Teknik Destek, Sosyal Medya ve Mesajlaşma, Web Hosting - Server, Beyaz Eşya, Yazıcı, Akıllı TV.

**İngilizce (26):** Windows, Linux, macOS, Networking, Command-line, Security, Bash, SSH, Git, Docker, Kubernetes, Python, Node.js, PostgreSQL, MySQL, Nginx, Android, iPhone, AWS, Azure, Excel, Outlook, Chrome, VS Code, Raspberry Pi, WordPress.

### Kaynak dağılımı

- **İngilizce:** Stack Overflow (176), Reddit (30), Microsoft (17), Google Support (6), WordPress Forum (6)
- **Türkçe:** Teknoloji Forumu (104), Müşteri Destek Platformu (2)

### Sentiment dağılımı

| | negative | neutral | positive |
|--|----------|---------|----------|
| İngilizce | 98 (%42) | 130 (%55) | 7 (%3) |
| Türkçe | 35 (%33) | 63 (%59) | 8 (%8) |

Destek verisinin doğası gereği etiketler ağırlıklı olarak `neutral` (sakin nasıl-yapılır soruları) ve `negative` (arıza/hata bildirimleri); `positive` sınıfı (takdir/merak içeren sakin sorular) doğal olarak küçüktür.

---

## Diyalog uzunluğu analizi

Tipik bir çözülmüş vaka **5–14 mesaj** ve **~100–500 kelime** aralığındadır.

| | Kelime (min / medyan / ort. / maks.) | Tur (min / medyan / maks.) |
|--|--------------------------------------|----------------------------|
| İngilizce | 181 / 361 / 841 / 7387 | 2 / 6 / 31 |
| Türkçe | 19 / 116 / 135 / 478 | 2 / 6 / 10 |

- Her iki dilde de **medyan ~6 tur**; bu, hedef "5–14 mesaj" aralığının içindedir.
- İngilizce medyan 361 kelime, Türkçe medyan 116 kelime → hedef "100–500 kelime" bandının içinde.
- Uç değerler: bazı İngilizce kayıtlar (genel forum kaynaklı, kapsamlı tartışmalar) 500 kelimeyi belirgin şekilde aşar; bazı Türkçe kayıtlar kısa, hızlı çözülen vakalardır (100 kelimenin altında). Bu çeşitlilik, gerçek dünyadaki vaka uzunluğu dağılımını yansıtır.

---

## Notlar ve sınırlamalar

- **Gizlilik:** Veri, kişisel/kurumsal tanımlayıcı bilgiler içermeyecek şekilde normalize edilmiştir. Bireysel kaynak URL'leri kaldırılmış, yalnızca genel kaynak etiketi (`kaynak`) bırakılmıştır.
- **Dahili kurumsal yazışmalar** (anonimleştirilmemiş müşteri verisi) bu açık veri setine **dahil edilmemiştir**.
- Veri seti orta ölçeklidir; her örnek tek turlu QA çiftleri yerine **tam çok turlu bir destek vakasını** temsil eder ve daha zengin bağlamsal bilgi içerir.
- Few-shot örnekleri ile skorlanan değerlendirme örnekleri ayrıdır (sızıntıyı önlemek için).

---

## Kullanım

```python
import json

en = json.load(open("en_data.json", encoding="utf-8"))
tr = json.load(open("tr_data.json", encoding="utf-8"))

print(len(en), len(tr))           # 235 106
ornek = en[0]
print(ornek["urun"], ornek["sentiment"])
print(ornek["reference_summary"])
```

---

## Lisans

Bir lisans seçip bu bölüme ekleyin (örn. araştırma/akademik kullanım için **CC BY 4.0** veya **CC BY-NC 4.0**).
