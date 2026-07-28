# BRD-Lite — İptal ve İade Süreçleri Otomasyon Projesi

> Bu doküman, iş analistliği portfolyosu kapsamında hazırlanmış bir vaka çalışmasıdır. İçerdiği
> şirket, kişi ve süreçler kurgusaldır; herhangi bir gerçek kurum veya kişiyle ilişkisi yoktur.
> Doküman, BRD yazımı, gereksinim analizi ve süreç modelleme yeteneklerini göstermek amacıyla
> hazırlanmıştır.

> **Not:** Bu dosya, [`03_brd-lite.docx`](03_brd-lite.docx) dosyasının tarayıcıda okunabilir
> Markdown sürümüdür. İçerik birebir aynıdır.

---

## 1. Yönetici özeti

**Problem.** Mevcut B2C iptal ve iade süreci; kargo durumunun sistemde görülememesi, 2 iş gününü
bulan işlem süreleri ve sistem dışı Excel takipleri nedeniyle operasyonel verimsizliğe, hatalı
stok bilgisine ve müşteri memnuniyetsizliğine yol açmaktadır.

**Amaç ve beklenen fayda.**

| Alan | Beklenen fayda |
|---|---|
| İşlem süreleri | Dakikalar seviyesine indirilmesi hedeflenmektedir; kesin değer sponsor onayı sonrası netleştirilecektir. |
| Stok doğruluğu | Otomatik güncellemelerle %100 güvence altına alınacaktır. |
| Sistem dışı takip | Excel kullanımı tamamen ortadan kaldırılacaktır (deprecate edilecek). |
| Müşteri deneyimi | Anlık kargo entegrasyonu ve otomatik SMS/e-posta bildirimleriyle kısır döngü aramaları engellenecektir. |

**Kapsam.** Proje yalnızca Türkiye B2C operasyonunu kapsamaktadır.

## 2. Proje bilgileri

| Meta veri | Detay bilgisi |
|---|---|
| Proje adı | B2C İptal ve İade Süreçleri Otomasyonu |
| İş sponsoru | Satış Operasyonları Müdürü |
| Hazırlayan | Senanur Yıldırım (İş Analisti) |
| Tarih | 25 Temmuz 2026 |
| Doküman versiyonu | v1.0 |
| Doküman durumu | Taslak / Onay bekliyor |

## 3. Amaç ve hedefler

Projenin ana amacı, iptal ve iade süreçlerini tek bir platformda (Oracle ERP) toplayarak
operasyonel mükemmelliği sağlamak ve müşteri deneyimini iyileştirmektir.

| Hedef maddesi | Hedeflenen değer | Ölçüm yöntemi |
|---|---|---|
| İşlem süresi (SLA) | TBD — sponsor ile netleştirilecek (Q-03) | ERP işlem zaman damgası |
| Sistem dışı takip oranı | %0 (Excel takibi tamamen kapatılacak) | Depo ve muhasebe denetim raporları |
| Mükerrer satış / stok hatası | %0 | Hatalı stok / eksi stok vaka raporları |
| Müşteri iletişim döngüsü | "Param nerede" aramalarında %80 azalma | Müşteri hizmetleri çağrı kategorizasyonu |
| Kargo görünürlük doğruluğu | %100 canlı API doğruluğu | Kargo entegrasyon logları |

## 4. Kapsam

### 4.1. Kapsam içi (in-scope)

- **Organizasyonel / coğrafi kapsam:** yalnızca Türkiye B2C operasyonu ve B2C e-ticaret/satış
  kanalları.
- **Süreç kapsamı:** müşteri talebinin alınması, 14 günlük yasal süre kontrolü, anlık kargo durumu
  sorgulama, kargolanmamış sipariş iptali, kargolanmış/teslim edilmiş ürün iadesi, depo fiziksel
  kabul ve kalite kontrolü, stok güncelleme, ödeme iadesi (kart/IBAN) ve müşteri otomatik
  bilgilendirme adımları.

### 4.2. Kapsam dışı (out-of-scope)

- Kurumsal / B2B (toptan) satış kanallarının iade ve iptal süreçleri.
- Müşterilerin web sitesi üzerinden kendi kendine (self-servis) iade talebi oluşturması
  (Faz 2 değerlendirmesi).
- ERP dışındaki yeni bir ödeme altyapısı / POS sağlayıcı entegrasyonu.

### 4.3. Etkilenen sistemler

| Sistem | Rolü |
|---|---|
| **Oracle ERP** (birincil sistem) | Tüm iptal/iade kayıtları, stok güncellemeleri ve ödeme onaylarının yönetileceği ana platform. |
| **Kargo sistemi** (entegrasyon) | Canlı kargo/teslimat durumunun sorgulanacağı harici API entegrasyonu. |
| **Muhasebe Excel dosyaları** (deprecate) | Süreç devreye alındığında tamamen kapatılacak ve kullanımı yasaklanacak sistem dışı dosya. |

## 5. Paydaşlar ve RACI matrisi

| Paydaş | Rol | Sorumluluk | RACI |
|---|---|---|---|
| İş sponsoru | Satış Ops. Müdürü | İş sponsoru, gereksinim ve doküman onayı | **A** |
| Senanur Yıldırım | İş Analisti | Gereksinim tespiti, analiz, dokümantasyon ve süreç modelleme | **R** |
| Finans yetkilisi | Finans / Muhasebe | Ödeme kuralları, mali onaylar ve muhasebe açık soruları | **C** |
| Depo şefi (TBD) | Depo / Lojistik | Fiziksel kontrol, kalite onay akışı ve kabul kriterleri | **C** |
| Müşteri temsilcileri | Satış / Destek | Operasyonel süreç kullanıcıları, talep girişleri | **I** |
| IT / Oracle danışmanı | IT / Yazılım | Sistem mimarisi, API entegrasyonu ve ERP geliştirmeleri | **R** |
| E-ticaret ekibi | Dijital kanal | Web kanalı etkileşimi ve sipariş verisi akışı | **C** |

**RACI tanımları:** Responsible (yapan), Accountable (onaylayan), Consulted (danışılan),
Informed (bilgilendirilen).

## 6. As-is (mevcut durum) özeti

Mevcut iptal/iade süreci standart bir kurala bağlı kalmaksızın e-posta, telefon ve kişisel Excel
dosyaları üzerinden yürütülmektedir. Bu durum süreçte kontrolsüzlüğe, operasyonel gecikmelere ve
maddi kayıplara yol açmaktadır. *(Detaylı süreç akışı için bkz. Ek A: As-Is Swimlane Diyagramı.)*

### Mevcut durumdaki 5 temel kök sorun

1. **Görünürlük eksikliği.** Müşteri temsilcisi kargo durumunu sistemde göremediği için tahmini
   iptal sözü vermekte; kargodaki ürünler için hatalı iptal sözü verilmektedir (ayda ~2 vaka riski).
2. **Uzun işlem süresi.** Sipariş yönetiminde manuel ters kayıt işlemleri nedeniyle süreçte
   +2 iş günü gecikme yaşanmaktadır.
3. **Standart dışı karar ve hatalı stok.** Kısmi iadelerde kişiye göre farklı yöntemler
   uygulanmakta; depoda elle stok güncellemesi yapıldığı için mükerrer satış riski oluşmaktadır.
4. **Sistem dışı Excel takibi.** Muhasebe süreci Oracle dışında bağımsız bir Excel dosyasında
   yürütmekte, ay sonu ERP ile manuel mutabakat eziyeti yaşanmaktadır.
5. **Bildirim eksikliği.** Müşteriye sürecin tamamlandığına dair otomatik bildirim gitmediği için
   müşteri "Param nerede?" aramalarıyla akışı en başa döndüren bir kısır döngü yaratmaktadır.

## 7. İş kuralları (business rules)

| ID | Kural |
|---|---|
| **BR-01** *(kargo durumu ayrımı)* | Sipariş sorgulamasında kargo statüsü "Kargolanmadı" ise sistem anlık iptal akışını; "Kargoda" veya "Teslim Edildi" ise iade akışını tetiklemelidir. Kargo statüsü okunamıyorsa sistem temsilciye uyarı vererek körü körüne iptal işlemi yapılmasını engellemelidir. |
| **BR-02** *(depo onayı şartı)* | Muhasebe ekranlarında iade ödeme onayının açılması için Oracle üzerinde Depo/Lojistik rolü tarafından ürünün fiziksel kontrolünün yapılıp "İadeye Uygun" onayının verilmiş olması zorunludur. |
| **BR-03** *(ödeme tipine göre iade)* | İade ödemeleri, müşterinin siparişi oluşturduğu orijinal ödeme yöntemine göre otomatik belirlenir. Kredi kartı ile yapılan ödemeler karta iade, havale/EFT ile yapılan ödemeler IBAN'a havale olarak işleme alınır. |
| **BR-04** *(yasal süre sınırı)* | Cayma hakkı kapsamındaki iade taleplerinin teslimat tarihinden itibaren 14 takvim günü içinde başlatılması gerekir. Ayıplı/hasarlı ürün durumları bu süre sınırından muaftır. |
| **BR-05** *(kısmi iade kuralı)* | Çoklu ürün içeren siparişlerde kalem bazlı iade mümkündür. İade edilen kalemin tutarı siparişten düşülür. İndirim/kampanya kuponlarının oransal dağılım kuralı Q-04 açık sorusunun kapanmasıyla kesinleşecektir. |

## 8. Yüksek seviye gereksinimler

### 8.1. Fonksiyonel gereksinimler

| ID | Gereksinim | İlişkili iş kuralı |
|---|---|---|
| **FR-01** | Sistem, sipariş sorgulama ekranında kargo firması, canlı kargo/teslimat durumunu göstermelidir. | BR-01 |
| **FR-02** | Sistem, kargoya verilmemiş siparişler için müşteri temsilcisine tek adımlı ve anlık iptal işlemi imkânı sağlamalıdır. | BR-01 |
| **FR-03** | Sistem, kargoya verilmiş veya teslim edilmiş siparişleri iptal akışı yerine otomatik olarak iade akışına yönlendirmelidir. | BR-01 |
| **FR-04** | Sistem, iade tutarı hesaplamasında ve ödeme adımında orijinal ödeme tipini (kredi kartı / havale) sipariş verisinden otomatik olarak tanımlamalıdır. | BR-03 |
| **FR-05** | Sistem, iade taleplerinde teslim tarihinden itibaren 14 günlük cayma süresini kontrol etmeli; süre dolmuşsa temsilciyi uyarmalıdır. Ayıplı ürün seçiminde bu kontrol devre dışı kalmalıdır. | BR-04 |
| **FR-06** | Sistem, kısmi iade taleplerinde ana siparişi bozmadan kalem bazında (satır bazlı) iade işlemine izin vermelidir. | BR-05 |
| **FR-07** | Sistem, Depo/Lojistik onayı bulunmayan iade talepleri için muhasebe ekranında ödeme onay yetkisini vermemelidir. | BR-02 |
| **FR-08** | Sistem, iade sürecinin finansal takibini (iade durumu, onay, ödeme, mutabakat) Oracle üzerinde uçtan uca sağlamalıdır; ERP dışı takip ihtiyacı ortadan kalkmalıdır. | Genel |
| **FR-09** | Sistem, muhasebe ödeme onayının tamamlandığı anda müşteriye otomatik bilgilendirme (SMS ve e-posta) göndermelidir. | Genel |
| **FR-10** | Sistem, depodan verilen iade kabul onayı ile eşzamanlı olarak Oracle üzerindeki satılabilir stok miktarını otomatik güncellemelidir. | BR-02 |
| **FR-11** | Sistem, kargo statüsünün okunamadığı durumlarda temsilciye uyarı göstermeli ve iptal işleminin başlatılmasını, statü teyit edilene kadar bloke etmelidir. | BR-01 |
| **FR-12** | Sistem, iade talebi oluşturulduğunda müşterinin depoya gönderim yapabileceği benzersiz bir "İade Kargo Kodu / Referans Numarası" üretmeli ve temsilci ekranında görüntülemelidir. | BR-01 |
| **FR-13** | Sistem, iptal işlemi sırasında önceden tanımlı bir iptal nedeni listesi sunmalı ve seçilen gerekçeyi sipariş detay kaydına işlemelidir. | BR-01 |
| **FR-14** | Sistem, "Hasarlı / Yeniden Satışa Uygun Değil" olarak değerlendirilen iade ürünlerini satılabilir stoğa eklememeli; otomatik olarak "İnceleme / Hasarlı Stok" kategorisine aktarmalıdır. | BR-02 |
| **FR-15** | Sistem, "Ayıplı / Kusurlu Ürün" nedeniyle açılan iade taleplerinde açıklama ve destekleyici belge/görsel girilmesini zorunlu tutmalıdır. | BR-04 |

### 8.2. Fonksiyonel olmayan gereksinimler

| ID | Kategori | Gereksinim |
|---|---|---|
| **NFR-01** | Performans | Oracle ERP üzerindeki iptal/iade sorgulama ve tetikleme işlemleri sistem yükü altında en fazla 5 saniye içinde tamamlanmalıdır. |
| **NFR-02** | Kullanılabilirlik | Müşteri temsilcisi ekranındaki iptal veya iade başlatma akışı en fazla 3 tıklama ile gerçekleştirilebilmelidir. |
| **NFR-03** | İzlenebilirlik / audit trail | Tüm iptal/iade adımları için işlem yapan kullanıcı ID'si, işlem zamanı ve aşama bilgisini içeren denetim izi Oracle üzerinde tutulmalıdır. |
| **NFR-04** | Yetkilendirme / güvenlik | Depo onay yetkisi sadece "Depo/Lojistik" rolüne; ödeme iade onay yetkisi ise sadece "Muhasebe/Finans" rolüne tanımlanmalıdır. |
| **NFR-05** | Uyumluluk | Sistem, Mesafeli Satış Sözleşmesi Yönetmeliği ve KVKK standartlarına uygun şekilde kişisel veri ve işlem geçmişi saklamalıdır. |

## 9. Varsayımlar ve kısıtlar

### 9.1. Varsayımlar

- Oracle ERP mevcut lisans ve altyapısının, ek bir modül satın alımına gerek kalmadan bu
  geliştirmeleri desteklediği varsayılmıştır.
- Entegre olunacak kargo firmasının canlı veri sağlayan ve kararlı çalışan bir API servisinin
  bulunduğu varsayılmıştır.
- Muhasebe ve depo departmanlarının sistem dışı Excel dosyalarını bırakarak %100 ERP kullanımına
  geçişi kabul ettiği varsayılmıştır.
- Projenin Faz 1 aşamasında yalnızca B2C operasyonunun kapsandığı, B2B kanalının etkilenmeyeceği
  varsayılmıştır.

### 9.2. Kısıtlar

- Mevcut Oracle ERP mimarisi değiştirilmeyecek, geliştirmeler mevcut altyapı üzerinde yapılacaktır.
- Proje kapsamına yeni bir ödeme yöntemi veya harici sanal POS altyapısı eklenmeyecektir.
- Bütçe ve zaman planı: TBD (sponsor onayı sonrası netleştirilecektir).
- Faz 1 canlıya alımı yalnızca Türkiye operasyonunu kapsamak zorundadır.

## 10. Riskler ve azaltma önlemleri

| Risk ID | Risk açıklaması | Olasılık | Etki | Azaltma önlemi |
|---|---|---|---|---|
| **R-01** | Kargo firması API servisindeki gecikme veya kesintiler nedeniyle kargo statüsünün okunamaması. | Yüksek | Yüksek | Faz 1 için sisteme manuel kargo takip numarası ile sorgulama yapma "fallback" butonu eklenmesi. |
| **R-02** | Muhasebe ekibinin Excel takibini bırakmakta direnç göstermesi ve adaptasyon sorunu. | Orta | Yüksek | Muhasebe ekibinin erken UAT aşamasına dahil edilmesi ve 2 haftalık paralel çalışma dönemi tanımlanması. |
| **R-03** | Kısmi iadelerde indirim/kampanya dağılım kuralının netleşmemesi nedeniyle yazılımda gecikme yaşanması. | Orta | Orta | Finans ile analiz aşaması bitmeden öncelikli karar toplantısı yapılması (Q-04 takibi). |
| **R-04** | Ayıplı/hasarlı ürün iade akışının tasarım aşamasında belirsizlikler çıkarması. | Orta | Orta | Ayıplı ürün akışının Faz 1'de temel mantıkla tutulması, karmaşık vakaların Faz 1.5 olarak ayrı kurgulanması. |

## 11. Açık sorular ve beklenen kararlar

| ID | Açık soru / karar konusu | Sorumlu paydaş | Hedef tarih | Durum |
|---|---|---|---|---|
| **Q-01** | İptal/iade edilen siparişlerde kullanılan kupon ve hediye çeklerinin müşteriye iade kuralı ne olacak? | Finans / Muhasebe | TBD | Açık |
| **Q-02** | Depo kontrolünde "hasarlı/kullanılmış" çıkan ürünler için uygulanacak prosedür ve müşteri iletişim kuralı nedir? | Finans + Depo | TBD | Açık |
| **Q-03** | Hedeflenen işlem süresi (SLA) KPI değerlerinin kesinleştirilmesi. | İş sponsoru | TBD | Açık |
| **Q-04** | Kısmi iadelerde sepet indirimlerinin ürünlere dağılım mantığı nasıl hesaplanacak? | Finans / Muhasebe | TBD | Açık |
| **Q-05** | IBAN ile iadelerde IBAN alma sorumluluğu kimde olacak (müşteri temsilcisi mi, muhasebe mi)? | İş sponsoru | TBD | Açık |

## 12. Ekler

| Ek | İçerik |
|---|---|
| **Ek A** | [As-is swimlane süreç diyagramı](../diagrams/as-is-swimlane.png) — 5 kulvarlı mevcut durum şeması |
| **Ek B** | [To-be swimlane süreç diyagramı](../diagrams/to-be-swimlane.png) — gelecek durum süreç akış şeması |
| **Ek C** | [Paydaş görüşme notları özeti](02_gorusme_ozeti.md) |
