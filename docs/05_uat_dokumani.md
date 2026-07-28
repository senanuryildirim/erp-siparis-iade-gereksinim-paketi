# UAT Dokümanı — İptal ve İade Süreçleri Otomasyonu

> **Not:** Bu dosya, [`05_uat_dokumani.xlsx`](05_uat_dokumani.xlsx) dosyasının tarayıcıda okunabilir
> Markdown sürümüdür. İçerik birebir aynıdır; Excel'deki 5 sayfa aşağıda 5 bölüm olarak yer alır.

**İş Analisti:** Senanur Yıldırım · **Proje:** Oracle ERP İptal ve İade Süreçleri Otomasyonu ·
**Doküman versiyonu:** v1.0

| Bölüm | İçerik |
|---|---|
| [1](#1-uat-kapsam-matrisi-traceability) | UAT kapsam matrisi (izlenebilirlik) |
| [2](#2-uat-test-veri-seti) | Test veri seti (9 kayıt) |
| [3](#3-uat-senaryoları-12-senaryo) | UAT senaryoları (12 senaryo) |
| [4](#4-uat-06--detay-vitrin-senaryosu) | UAT-06 detay vitrin senaryosu |
| [5](#5-uat-onay-bloğu) | UAT onay bloğu |

---

## 1. UAT kapsam matrisi (traceability)

| User Story | Kapsamı | İlgili UAT senaryoları | Öncelik |
|---|---|---|---|
| US-01 | Güncel kargo durumu görüntüleme | UAT-01, UAT-02 | Yüksek |
| US-02 | Kargolanmamış siparişin anlık iptali + neden kaydı | UAT-03 | Yüksek |
| US-03 | İade akışı yönlendirme + iade kargo kodu | UAT-04 | Yüksek |
| US-04 | 14 günlük yasal süre denetimi + istisnalar | UAT-05, UAT-06, UAT-07 | Yüksek |
| US-05 | Kalem bazlı kısmi iptal ve iade | UAT-08 | Orta |
| US-06 | Depo fiziksel iade kabul / red | UAT-09, UAT-10 | Yüksek |
| US-07 | Ödeme yöntemine göre iade | UAT-12 | Yüksek |
| US-08 | Depo onayı olmayan taleplerde ödeme kısıtı | UAT-11 | Yüksek |
| US-09 | Otomatik müşteri bildirimleri | UAT-12 (birleşik test) | Yüksek |
| US-10 | Otomatik stok güncelleme (kategori bazlı) | UAT-09, UAT-10 (birleşik test) | Yüksek |
| US-11 | Audit log (denetim izi) | Kapsam dışı — teknik/IT testi | Düşük |

> **Toplam:** 12 UAT senaryosu · 10 user story kapsandı · US-11 audit log kapsam dışı (IT ekibi
> tarafından teknik test edilecektir).

## 2. UAT test veri seti

Aşağıdaki test siparişleri UAT senaryolarında referans olarak kullanılacaktır.

| Sipariş No | Statü | Ödeme tipi | Teslim tarihi | İçerik / detay |
|---|---|---|---|---|
| TEST-1001 | Kargoda | Kredi Kartı | — | Tek kalem ürün (A ürünü) |
| TEST-1002 | Kargolanmadı | Kredi Kartı | — | Çoklu kalem (A ürünü + B ürünü) |
| TEST-1003 | Teslim Edildi | Havale / EFT | 20.07.2026 (5 gün önce) | Tek kalem ürün — cayma hakkı testi |
| TEST-1004 | Teslim Edildi | Kredi Kartı | 01.07.2026 (24 gün önce) | Ayıplı/kusurlu ürün iddiası |
| TEST-1005 | Teslim Edildi | Kredi Kartı | 01.07.2026 (24 gün önce) | Sebepsiz iade talebi |
| TEST-1006 | Teslim Edildi | Kredi Kartı | 18.07.2026 (7 gün önce) | Çoklu kalem (X ürünü + Y ürünü) |
| TEST-1007 | Teslim Edildi | Kredi Kartı | 22.07.2026 (3 gün önce) | İade incelemesindeki depo ürünü |
| TEST-1008 | Teslim Edildi | Kredi Kartı | 22.07.2026 (3 gün önce) | Hasarlı/kullanılmış iade ürünü |
| TEST-1009 | Teslim Edildi | Kredi Kartı | 23.07.2026 (2 gün önce) | Onaylanmış depo iadesi (muhasebe kuyruğu) |

## 3. UAT senaryoları (12 senaryo)

Her senaryo mutlu yol, alternatif yol veya negatif test olarak sınıflandırılmıştır. UAT-06 için
detaylı vitrin senaryosu [Bölüm 4](#4-uat-06--detay-vitrin-senaryosu)'te yer alır.

---

### UAT-01 — Kargolanmış siparişte kargo durumu görüntüleme

`US-01 / FR-01` · `Test verisi: TEST-1001` · **Mutlu yol**

**Test adımları**
1. Sipariş sorgulama ekranında TEST-1001 sorgula.
2. "Kargo Statüsü" alanını incele.
3. İptal ve iade seçeneklerinin durumunu kontrol et.

**Beklenen sonuç.** Kargo statüsü "Kargoda" olarak görünür, son güncelleme zamanı ekrandadır.
Anlık iptal seçeneği pasif, iade seçeneği aktif olmalıdır.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-02 — Kargo verisi okunamadığında iptal butonu pasif

`US-01 / FR-01, FR-11` · `Test verisi: TEST-1001 (kargo veri kaynağı yok)` · **Negatif test**

**Test adımları**
1. Test ortamında kargo veri kaynağını devre dışı bırak.
2. TEST-1001 sorgula.
3. Ekrandaki uyarı ve buton durumunu kontrol et.

**Beklenen sonuç.** "Kargo Durumu Belirsiz — Manuel Sorgulama Yapınız" uyarısı gösterilir. Anlık
iptal seçeneği pasif hale gelir; hatalı işlem yapılması engellenir.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-03 — Kargolanmamış siparişin anlık iptali + iptal nedeni seçimi

`US-02 / FR-02, FR-13` · `Test verisi: TEST-1002` · **Mutlu yol**

**Test adımları**
1. TEST-1002 sorgula.
2. "Anlık İptal" işlemini başlat.
3. İptal nedeni listesinden "Müşteri Vazgeçti" seç.
4. Onayla.

**Beklenen sonuç.** Sipariş statüsü anında "İptal Edildi" olarak değişir. İptal nedeni sipariş
detayına kaydedilir. Talep otomatik olarak muhasebe iş kuyruğuna düşer.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-04 — Kargolanmış sipariş için iade akışı + iade kargo kodu üretimi

`US-03 / FR-03, FR-12` · `Test verisi: TEST-1003` · **Mutlu yol**

**Test adımları**
1. TEST-1003 sorgula.
2. "İade Talebi Oluştur" işlemini başlat.
3. İade nedeni "Cayma Hakkı" seç.
4. Onayla.
5. Ekranda görüntülenen iade kargo kodunu not et.

**Beklenen sonuç.** İade talebi "İade Talebi Oluşturuldu" statüsüyle başlatılır. Benzersiz iade
kargo kodu üretilir ve temsilci ekranında görüntülenir. Müşteriye otomatik bildirim gider.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-05 — 14 gün içi cayma hakkı ile iade talebi kabulü

`US-04 / FR-05, BR-04` · `Test verisi: TEST-1003 (5 gün önce teslim)` · **Mutlu yol**

**Test adımları**
1. TEST-1003 sorgula.
2. "İade Talebi Oluştur" işlemini başlat.
3. İade nedeni "Cayma Hakkı" seç.
4. Onayla.

**Beklenen sonuç.** Sistem 14 günlük süreyi otomatik hesaplar. Süre içinde olduğu için talep
onaylanır ve depo kabul aşamasına aktarılır. Süre kontrolü manuel değil, sistem tarafındandır.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-06 — Ayıplı ürün iadesi (14 gün sonrası, belge zorunlu)

`US-04 / FR-05, FR-15` · `Test verisi: TEST-1004 (24 gün önce teslim)` · **Alternatif yol**

**Test adımları**
1. TEST-1004 sorgula.
2. İade nedeni "Cayma Hakkı" seç → sistem reddeder.
3. İade nedeni "Ayıplı / Kusurlu Ürün" seç.
4. Açıklama ve belge yükle.
5. Onayla.

*(Detaylı vitrin senaryosu için [Bölüm 4](#4-uat-06--detay-vitrin-senaryosu).)*

**Beklenen sonuç.** "Cayma Hakkı" 14 gün sonrası reddedilir. "Ayıplı Ürün" seçiminde 14 gün engeli
kaldırılır ancak açıklama + belge zorunlu tutulur. Zorunlu alanlar dolunca talep oluşur.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-07 — 14 gün geçmiş sebepsiz iade reddi

`US-04 / FR-05, BR-04` · `Test verisi: TEST-1005 (24 gün önce teslim)` · **Negatif test**

**Test adımları**
1. TEST-1005 sorgula.
2. "İade Talebi Oluştur" işlemini başlat.
3. İade nedeni "Cayma Hakkı" seç.
4. Onayla.

**Beklenen sonuç.** Sistem "Yasal 14 günlük iade süresi dolmuştur, işlem başlatılamaz" uyarısı
verir. Talep oluşturulmaz, sistemde kayıt açılmaz.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-08 — Çoklu kalem siparişte kalem bazlı kısmi iade

`US-05 / FR-06, BR-05` · `Test verisi: TEST-1006 (X + Y ürünü)` · **Mutlu yol**

**Test adımları**
1. TEST-1006 sorgula.
2. Kısmi iade ekranını aç.
3. Sadece X ürününü seç, Y ürününü seçme.
4. Onayla.

**Beklenen sonuç.** Sadece X ürünü için iade akışı başlatılır. Y ürünü sipariş içinde aktif kalır.
Ana sipariş dokümanı bozulmadan sadece seçilen kalemin statüsü değişir.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-09 — Depo onayı + otomatik satılabilir stok güncellemesi

`US-06 + US-10 / FR-07, FR-10` · `Test verisi: TEST-1007` · **Mutlu yol**

**Test adımları**
1. Depo sorumlusu rolüyle Oracle'a giriş yap.
2. TEST-1007'nin iade talebini aç.
3. Fiziksel kontrol sonrası "İadeye Uygun — Kabul" onayı gir.
4. Ürünün stok kartını kontrol et.

**Beklenen sonuç.** İade talebi "Depo Onaylı" durumuna geçer ve muhasebe kuyruğuna düşer. İlgili
ürünün satılabilir stok miktarı anlık olarak artar. Manuel stok girişine gerek kalmaz.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-10 — Hasarlı ürün reddi + gerekçe zorunluluğu + hasarlı stok kategorisi

`US-06 + US-10 / FR-07, FR-14` · `Test verisi: TEST-1008` · **Alternatif yol**

**Test adımları**
1. Depo rolüyle TEST-1008'i aç.
2. "İadeye Uygun Değil — Red" seç.
3. Gerekçe boşken kaydetmeye çalış → sistem engellemeli.
4. Gerekçe gir ve kaydet.
5. Stok kategorilerini kontrol et.

**Beklenen sonuç.** Gerekçe olmadan red kaydı engellenir. Gerekçeyle red kaydı başarıyla
tamamlanır. Ürün "Satılabilir Stok"a **eklenmez**, otomatik olarak "İnceleme / Hasarlı Stok"
kategorisine geçer. Müşteriye red gerekçesi ile birlikte otomatik bildirim gider.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-11 — Depo onayı olmadan muhasebe ödeme yapamıyor

`US-08 / FR-07, NFR-04` · `Test verisi: TEST-1007 (depo onayı henüz yok)` · **Negatif test**

**Test adımları**
1. Muhasebe rolüyle Oracle'a giriş yap.
2. TEST-1007'nin iade kaydını aç.
3. Ödeme onay adımını başlatmayı dene.

**Beklenen sonuç.** Ödeme onay butonları/adımı kilitli görünür. "Depo Onayı Bekleniyor" uyarısı
görüntülenir. Herhangi bir şekilde bypass edilmeye çalışılırsa sistem işlemi reddeder.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

### UAT-12 — Kredi kartı iadesi + otomatik müşteri bildirimi

`US-07 + US-09 / FR-04, FR-09` · `Test verisi: TEST-1009` · **Mutlu yol**

**Test adımları**
1. Muhasebe rolüyle TEST-1009'u aç (depo onaylı, muhasebe kuyruğunda).
2. Ödeme tipini kontrol et (otomatik "Kredi Kartı" gelmeli).
3. Ödeme onayını ver.
4. Müşterinin SMS/e-posta kanallarını kontrol et.

**Beklenen sonuç.** Ödeme tipi orijinal siparişten otomatik "Kredi Kartı — Aynı Karta İade" olarak
belirlenir. Onay ile birlikte iade karta yansıtılır ve iade statüsü "Tamamlandı" olur. Müşteriye
"İadeniz Tamamlandı" bildirimi otomatik gönderilir.

**Statü:** ☐ Pass ☐ Fail ☐ Blocked

---

## 4. UAT-06 — Detay vitrin senaryosu

Bu senaryo, karmaşık bir vakayı adım adım gösteren "vitrin" formatındadır. Diğer UAT senaryoları
kompakt formatta [Bölüm 3](#3-uat-senaryoları-12-senaryo)'te yer alır.

| Alan | Değer |
|---|---|
| **Senaryo ID** | UAT-06 |
| **İlişkili user story** | US-04 (14 günlük yasal süre denetimi) |
| **İlişkili FR** | FR-05, FR-15 |
| **İlişkili iş kuralı** | BR-04 (yasal süre sınırı) |
| **Test verisi** | TEST-1004 (Teslim Edildi, 24 gün önce, ayıplı iddia) |

**Ön koşullar**

1. Test kullanıcısı "Müşteri Temsilcisi" rolüyle Oracle ERP'de oturum açmış olmalı.
2. TEST-1004 numaralı sipariş: statü = "Teslim Edildi", teslim tarihi = 24 gün önce,
   ödeme tipi = kredi kartı.
3. Sistemde iade nedeni listesi tanımlı ("Cayma Hakkı", "Ayıplı / Kusurlu Ürün" seçenekleri mevcut).
4. Belge yükleme test dosyası hazır (örn. `urun-hasari.jpg`, ~500 KB).

**Adımlar**

| # | Aksiyon | Beklenen sonuç |
|---|---|---|
| 1 | "Sipariş Sorgulama" ekranını aç, "Sipariş No" alanına TEST-1004 gir, sorgula işlemini tetikle | Sipariş detay ekranı açılır; kargo statüsü "Teslim Edildi", teslim tarihi 24 gün önce olarak görüntülenir |
| 2 | "İade Talebi Oluştur" işlemini başlat | İade talebi formu açılır; "İade Nedeni" alanı zorunlu olarak işaretlidir |
| 3 | "İade Nedeni" listesinden "Cayma Hakkı" seç ve "İade Talebi Oluştur" işlemini onayla | Sistem "Yasal 14 günlük iade süresi dolmuştur, işlem başlatılamaz" uyarısını gösterir; işlem kilitli kalır, kaydedilmez |
| 4 | "İade Nedeni" seçimini "Ayıplı / Kusurlu Ürün" olarak değiştir | Ekranda "Açıklama" ve "Destekleyici Belge/Görsel" alanları görünür ve her ikisi de zorunlu olarak işaretlenir |
| 5 | "Açıklama" alanını boş bırak, belge yüklemeden "İade Talebi Oluştur" işlemini onayla | Sistem "Ayıplı ürün iadelerinde açıklama ve destekleyici belge zorunludur" uyarısı verir; kayıt oluşmaz |
| 6 | "Açıklama" alanına "Ürün ambalajı hasarlı geldi, cihaz açılmıyor" yaz; "Belge Yükle" işlemiyle `urun-hasari.jpg` dosyasını ekle | Açıklama metni ve belge önizlemesi ekranda görüntülenir |
| 7 | "İade Talebi Oluştur" işlemini onayla | Talep başarıyla oluşur; 14 gün engeli devre dışı kalır; iade statüsü "İade Talebi Oluşturuldu — Depo Kabul Bekleniyor" olarak atanır; benzersiz iade kargo kodu üretilir |
| 8 | Müşterinin kayıtlı iletişim kanallarını (SMS/e-posta) kontrol et | Müşteriye "Talebiniz alındı" bildirimi ve iade kargo kodu otomatik gönderilir |

**Genel beklenen sonuç.** 14 günlük yasal cayma süresi dolmuş bir siparişte "Cayma Hakkı" ile iade
başlatma girişimi sistem tarafından reddedilir. Aynı sipariş için "Ayıplı / Kusurlu Ürün" nedeni
seçildiğinde 14 gün engeli kaldırılır ancak açıklama ve destekleyici belge girilmesi zorunlu
tutulur. Zorunlu alanlar dolduğunda talep oluşur, benzersiz iade kargo kodu üretilir ve müşteri
otomatik bilgilendirilir.

**Test sonucu ve imza**

| Alan | Değer |
|---|---|
| Genel statü | ☐ Pass ☐ Fail ☐ Blocked |
| Test tarihi | |
| Test eden | |
| Notlar / bulgular | |

## 5. UAT onay bloğu

UAT sonucunda projenin canlıya alınıp alınamayacağı bu blokta onaylanır.

**UAT genel kararı**

☐ Kabul Edildi (Go-Live)  ☐ Şartlı Kabul (kritik olmayan hatalar giderilecek)  ☐ Reddedildi

**İmzalar**

| Rol / unvan | İsim / soyisim | İmza / karar | Tarih |
|---|---|---|---|
| İş sponsoru (Satış Operasyonları Müdürü) | [İsim] | | __ / __ / 2026 |
| İş analisti | Senanur Yıldırım | | __ / __ / 2026 |
| Test / QA koordinatörü | [İsim] | | __ / __ / 2026 |
| Yazılım takım lideri (IT) | [İsim] | | __ / __ / 2026 |

---

<sub>Bu doküman iş analistliği portfolyosu kapsamında hazırlanmış bir vaka çalışmasıdır. Kişi, kurum
ve süreçler kurgusaldır.</sub>
