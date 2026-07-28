<div align="center">

# Oracle ERP — Sipariş İptal ve İade Süreçlerinin Otomasyonu

**İş analistliği vaka çalışması** · Paydaş görüşmesinden UAT'ye uçtan uca gereksinim analizi paketi

![Rol](https://img.shields.io/badge/Rol-İş_Analisti-0A66C2?style=flat-square)
![Alan](https://img.shields.io/badge/Alan-E--ticaret_%2F_Oracle_ERP-F80000?style=flat-square)
![Modelleme](https://img.shields.io/badge/Modelleme-draw.io-F08705?style=flat-square)
![Çıktılar](https://img.shields.io/badge/Çıktılar-BRD_·_11_User_Story_·_12_UAT-2B579A?style=flat-square)

`Türkçe` · [`English`](README.en.md)

</div>

---

E-ticaret satışı yapan bir teknoloji ürünleri firmasında siparişler Oracle ERP üzerinde
yönetiliyor, ancak **iptal ve iade talepleri e-posta ve telefonla geliyor ve tamamen manuel
işleniyor.** Bu paket, sürecin uçtan uca ERP'ye taşınması için hazırlanmış gereksinim analizi
çalışmasının tamamıdır.

Başlangıçtaki talep tek cümleydi: *"İptal-iade işleri artık sistemden yürüsün, elle iş
kalmasın."* Bu paket, o muğlak cümlenin nasıl ölçülebilir gereksinimlere, iş kurallarına ve test
edilebilir kabul kriterlerine dönüştüğünü gösteriyor.

## Bir bakışta

| | |
|---|---|
| **Rol** | İş Analisti (tek kişilik analiz sorumluluğu) |
| **Kapsam** | Türkiye B2C iptal ve iade süreci, Oracle ERP |
| **Paydaş** | 7 paydaş, 5 departman |
| **Yöntem** | Paydaş analizi → gereksinim görüşmesi → as-is/to-be modelleme → BRD → user story → UAT |
| **Araçlar** | draw.io, Word, Excel, Git/GitHub |
| **Üretilen çıktılar** | Paydaş matrisi · 15 soruluk görüşme tasarımı · 2 swimlane diyagramı · BRD-lite · 11 user story (Given/When/Then) · 12 UAT senaryosu · izlenebilirlik matrisi |

## Problem

Süreç Oracle ERP'nin dışında yürüdüğü için ölçülemiyor, denetlenemiyor ve tekrar ediyor.
Analiz sırasında **5 kök sorun** tespit ettim:

| # | Kök sorun | Operasyonel etkisi |
|---|---|---|
| 1 | **Görünürlük eksikliği** — müşteri temsilcisi kargo durumunu sistemde göremiyor | Kargoya verilmiş siparişe iptal sözü veriliyor; ayda ~2 vaka riski |
| 2 | **Uzun işlem süresi** — iptaller manuel ters kayıtla işleniyor | Bir iptalin tamamlanması ortalama **2 iş günü** |
| 3 | **Standart dışı karar ve hatalı stok** — kısmi iadede kişiye göre farklı yöntem, stok elle güncelleniyor | Satılabilir stok yanlış görünüyor, mükerrer satış riski |
| 4 | **Sistem dışı Excel takibi** — muhasebe iadeleri ERP dışında ayrı dosyada tutuyor | Ay sonu ERP mutabakatı manuel yapılıyor |
| 5 | **Bildirim eksikliği** — müşteriye otomatik bilgilendirme gitmiyor | "Param nerede?" aramaları süreci başa döndüren kısır döngü yaratıyor |

**Hacim:** ayda yaklaşık **120 iptal/iade talebi**, süreç **3 departmanı** doğrudan etkiliyor.

## Hedefler ve başarı ölçütleri

| Hedef | Hedeflenen değer | Ölçüm yöntemi |
|---|---|---|
| İşlem süresi (SLA) | Gün → dakika seviyesi *(kesin değer sponsor onayı bekliyor — Q-03)* | ERP işlem zaman damgası |
| Sistem dışı takip oranı | %0 (Excel takibi tamamen kapatılacak) | Depo ve muhasebe denetim raporları |
| Mükerrer satış / stok hatası | %0 | Hatalı stok ve eksi stok vaka raporları |
| Müşteri iletişim döngüsü | "Param nerede" aramalarında **%80 azalma** | Müşteri hizmetleri çağrı kategorizasyonu |
| Kargo görünürlük doğruluğu | %100 canlı API doğruluğu | Kargo entegrasyon logları |

## Kapsam

<table>
<tr><th width="50%">✅ Kapsam içi</th><th width="50%">⛔ Kapsam dışı</th></tr>
<tr valign="top">
<td>

- Türkiye B2C operasyonu ve B2C e-ticaret/satış kanalları
- Müşteri talebinin alınması
- 14 günlük yasal cayma süresi kontrolü
- Anlık kargo durumu sorgulama
- Kargolanmamış sipariş iptali
- Kargolanmış / teslim edilmiş ürün iadesi
- Depo fiziksel kabul ve kalite kontrolü
- Otomatik stok güncelleme
- Ödeme iadesi (kart / IBAN)
- Otomatik müşteri bilgilendirme

</td>
<td>

- Kurumsal / B2B (toptan) satış kanallarının iptal ve iade süreçleri
- Müşterinin web sitesinden self-servis iade talebi oluşturması *(Faz 2 değerlendirmesi)*
- ERP dışında yeni bir ödeme altyapısı / POS sağlayıcı entegrasyonu
- Audit log teknik testi *(IT ekibi tarafından yürütülecek)*

</td>
</tr>
</table>

**Etkilenen sistemler:** Oracle ERP (birincil), kargo firması API'si (entegrasyon), muhasebe
Excel dosyaları (devreye alımda tamamen kapatılacak).

## Süreç modelleme — as-is ve to-be

<table>
<tr>
<th width="50%">As-Is — mevcut durum</th>
<th width="50%">To-Be — hedeflenen durum</th>
</tr>
<tr valign="top">
<td><a href="diagrams/as-is-swimlane.png"><img src="diagrams/as-is-swimlane.png" alt="As-is swimlane diyagramı"></a></td>
<td><a href="diagrams/to-be-swimlane.png"><img src="diagrams/to-be-swimlane.png" alt="To-be swimlane diyagramı"></a></td>
</tr>
<tr valign="top">
<td>

**5 kulvar.** Adımların büyük kısmı `[Sistem Dışı]` etiketli: kafadan 14 gün hesabı, körü körüne
iptal sözü, elle stok güncelleme, mail ile iade onayı, Excel iade kaydı, elle mutabakat.
Kırmızı etiketler kök sorunların akıştaki tam yerini işaretliyor.

</td>
<td>

**4 kulvar.** Manuel ters kayıt yapan **"Sipariş Yönetimi" kulvarı tamamen ortadan kalkıyor** —
işin ERP'ye taşınmasıyla o kulvarın varlık nedeni kayboluyor. Kargo statüsü akışı en başta
üçe ayırıyor, yasal süre kontrolü sistemde, bildirimler otomatik.

</td>
</tr>
</table>

> Diyagramlar tam boyutta görmek için tıklanabilir. Kaynak dosyalar
> [`diagrams/`](diagrams/) klasöründedir.

## İş kuralları

| ID | Kural |
|---|---|
| **BR-01** | Kargo statüsü "Kargolanmadı" ise **iptal**, "Kargoda"/"Teslim Edildi" ise **iade** akışı tetiklenir. Statü okunamıyorsa sistem uyarı verir ve körü körüne iptali engeller. |
| **BR-02** | Muhasebe ödeme onayının açılması için Depo/Lojistik rolünün "İadeye Uygun" onayı **zorunludur**. |
| **BR-03** | İade, müşterinin orijinal ödeme yöntemine göre otomatik belirlenir: kart ödemesi → karta iade, havale/EFT → IBAN'a havale. |
| **BR-04** | Cayma hakkı iadeleri teslimden itibaren **14 takvim günü** içinde başlatılmalıdır. Ayıplı/hasarlı ürün bu sınırdan muaftır. |
| **BR-05** | Çoklu ürünlü siparişlerde kalem bazlı kısmi iade mümkündür; iade edilen kalemin tutarı siparişten düşülür. |

## Gereksinimler

**11 fonksiyonel gereksinim (FR)** ve **5 fonksiyonel olmayan gereksinim (NFR)** tanımlandı; her
FR ilişkili iş kuralına bağlandı. Öne çıkanlar:

<details>
<summary><b>Fonksiyonel gereksinimler (11) — açmak için tıklayın</b></summary>

| ID | Gereksinim | İlgili BR |
|---|---|---|
| FR-01 | Sipariş sorgulama ekranında kargo firması ve canlı kargo/teslimat durumu gösterilmelidir. | BR-01 |
| FR-02 | Kargoya verilmemiş siparişler için tek adımlı, anlık iptal imkânı sağlanmalıdır. | BR-01 |
| FR-03 | Kargolanmış veya teslim edilmiş siparişler otomatik olarak iade akışına yönlendirilmelidir. | BR-01 |
| FR-04 | İade tutarı ve ödeme adımında orijinal ödeme tipi sipariş verisinden otomatik tanımlanmalıdır. | BR-03 |
| FR-05 | 14 günlük cayma süresi kontrol edilmeli; süre dolmuşsa uyarı verilmeli, ayıplı üründe kontrol devre dışı kalmalıdır. | BR-04 |
| FR-06 | Kısmi iadelerde ana sipariş bozulmadan kalem bazlı işlem yapılabilmelidir. | BR-05 |
| FR-07 | Depo onayı bulunmayan iadelerde muhasebe ekranında ödeme onay yetkisi verilmemelidir. | BR-02 |
| FR-08 | İade sürecinin finansal takibi uçtan uca Oracle üzerinde yapılmalı; ERP dışı takip ihtiyacı kalmamalıdır. | Genel |
| FR-09 | Muhasebe ödeme onayı tamamlandığı anda müşteriye otomatik SMS ve e-posta gönderilmelidir. | Genel |
| FR-10 | Depo iade kabul onayı ile eşzamanlı olarak satılabilir stok otomatik güncellenmelidir. | BR-02 |
| FR-11 | Kargo statüsü okunamadığında temsilciye uyarı gösterilmeli ve iptal, statü teyit edilene kadar bloke edilmelidir. | BR-01 |

</details>

<details>
<summary><b>Fonksiyonel olmayan gereksinimler (5) — açmak için tıklayın</b></summary>

| ID | Kategori | Gereksinim |
|---|---|---|
| NFR-01 | Performans | İptal/iade sorgulama ve tetikleme işlemleri yük altında en fazla **5 saniye** içinde tamamlanmalıdır. |
| NFR-02 | Kullanılabilirlik | İptal veya iade başlatma akışı en fazla **3 tıklama** ile gerçekleştirilebilmelidir. |
| NFR-03 | İzlenebilirlik | Tüm adımlar için kullanıcı ID'si, işlem zamanı ve aşama bilgisini içeren audit log tutulmalıdır. |
| NFR-04 | Güvenlik / yetki | Depo onay yetkisi yalnızca "Depo/Lojistik", ödeme iade onayı yalnızca "Muhasebe/Finans" rolüne tanımlanmalıdır. |
| NFR-05 | Uyumluluk | Sistem, Mesafeli Satış Sözleşmesi Yönetmeliği ve KVKK'ya uygun veri saklamalıdır. |

</details>

## User story'ler

**9 epic altında 11 user story**, her biri Given/When/Then kabul kriterleriyle ve üç senaryo
katmanıyla yazıldı: **mutlu yol**, **alternatif yol** ve **hata durumu**. Bağımlılıklar ve
öncelikler (Must Have / High) her story'de belirtildi.

| Epic | User story |
|---|---|
| E1 · Kargo & statü görünürlüğü | US-01 Güncel kargo durumu görüntüleme |
| E2 · Sipariş iptal süreci | US-02 Kargolanmamış siparişin anlık iptali ve neden kaydı |
| E3 · İade yönlendirme & yasal süre | US-03 İade akışı yönlendirme + kargo kodu üretimi · US-04 14 günlük yasal süre denetimi ve istisnalar |
| E4 · Kısmi işlemler | US-05 Kalem bazlı kısmi iptal ve iade |
| E5 · Depo kontrol ve onay | US-06 Depo fiziksel iade kabul ve kalite değerlendirmesi |
| E6 · Ödeme iadesi & muhasebe | US-07 Orijinal ödeme yöntemine göre iade · US-08 Depo onayı olmayan taleplerde ödeme kısıtı |
| E7 · Müşteri iletişimi | US-09 Süreç adımlarında otomatik müşteri bilgilendirmesi |
| E8 · Stok yönetimi | US-10 Kabul edilen iadelerde kategori bazlı otomatik stok güncellemesi |
| E9 · İzlenebilirlik ve audit | US-11 Denetim izi (audit log) takibi |

<details>
<summary><b>Örnek: US-04 — 14 günlük yasal süre denetimi</b></summary>

> **Bir müşteri temsilcisi olarak**, iade taleplerinde teslimat tarihinden itibaren 14 günlük
> yasal cayma süresinin sistem tarafından denetlenmesini **istiyorum**, **böylece** manuel tarih
> hesabı yapmaktan kurtulur ve süresi geçmiş haksız iadeleri kurala uygun şekilde süzebilirim.

**Senaryo 2 — Alternatif yol (ayıplı ürün muafiyeti ve belge zorunluluğu)**

- **Given** teslimat tarihinden itibaren 14 günden fazla süre geçmiş bir sipariş için,
- **When** müşteri temsilcisi iade nedenini "Ayıplı / Kusurlu Ürün" olarak seçtiğinde,
- **Then** sistem 14 gün engelini kaldırmalı, ancak açıklama veya destekleyici belge/görsel
  girilmesini zorunlu kılarak iade talebini kabul etmelidir.

*İlişkili FR: FR-05 · İlişkili BR: BR-04 · Öncelik: Must Have · Bağımlılık: US-03*

</details>

## UAT ve izlenebilirlik

**12 UAT senaryosu** ve **9 kayıtlık test veri seti** (TEST-1001…TEST-1009) hazırlandı. Senaryolar
mutlu yol, alternatif yol ve negatif test olarak sınıflandırıldı; UAT-06 karmaşık bir vaka olduğu
için adım adım "vitrin" formatında ayrıca detaylandırıldı.

Her user story'nin hangi UAT senaryosuyla doğrulandığını gösteren izlenebilirlik matrisi:

| User Story | Kapsamı | İlgili UAT | Öncelik |
|---|---|---|---|
| US-01 | Güncel kargo durumu görüntüleme | UAT-01, UAT-02 | Yüksek |
| US-02 | Kargolanmamış siparişin anlık iptali + neden kaydı | UAT-03 | Yüksek |
| US-03 | İade akışı yönlendirme + iade kargo kodu | UAT-04 | Yüksek |
| US-04 | 14 günlük yasal süre denetimi + istisnalar | UAT-05, UAT-06, UAT-07 | Yüksek |
| US-05 | Kalem bazlı kısmi iptal ve iade | UAT-08 | Orta |
| US-06 | Depo fiziksel iade kabul / red | UAT-09, UAT-10 | Yüksek |
| US-07 | Ödeme yöntemine göre iade | UAT-12 | Yüksek |
| US-08 | Depo onayı olmayan taleplerde ödeme kısıtı | UAT-11 | Yüksek |
| US-09 | Otomatik müşteri bildirimleri | UAT-12 *(birleşik test)* | Yüksek |
| US-10 | Otomatik stok güncelleme (kategori bazlı) | UAT-09, UAT-10 *(birleşik test)* | Yüksek |
| US-11 | Audit log (denetim izi) | Kapsam dışı — IT teknik testi | Düşük |

**Sonuç:** 12 UAT senaryosu ile 10 user story kapsandı. US-11 (audit log) bilinçli olarak
kapsam dışı bırakıldı; teknik bir doğrulama olduğu için IT ekibi tarafından test edilecek.

## Riskler ve açık sorular

Analizi "her şey netmiş gibi" kapatmak yerine, belirsizlikleri açıkça kayıt altına aldım.

<details>
<summary><b>Riskler ve azaltma önlemleri (4)</b></summary>

| ID | Risk | Olasılık / Etki | Azaltma önlemi |
|---|---|---|---|
| R-01 | Kargo API'sindeki gecikme veya kesinti nedeniyle statünün okunamaması | Yüksek / Yüksek | Faz 1 için manuel kargo takip numarasıyla sorgulama yapan "fallback" butonu |
| R-02 | Muhasebe ekibinin Excel takibini bırakmakta direnç göstermesi | Orta / Yüksek | Muhasebenin erken UAT'ye dahil edilmesi ve 2 haftalık paralel çalışma dönemi |
| R-03 | Kısmi iadelerde indirim dağılım kuralının netleşmemesi | Orta / Orta | Analiz bitmeden öncelikli karar toplantısı (Q-04 takibi) |
| R-04 | Ayıplı ürün iade akışının tasarımda belirsizlik çıkarması | Orta / Orta | Faz 1'de temel mantıkla tutulması, karmaşık vakaların Faz 1.5'e ayrılması |

</details>

<details>
<summary><b>Açık sorular ve beklenen kararlar (5)</b></summary>

| ID | Açık soru | Sorumlu paydaş |
|---|---|---|
| Q-01 | İptal/iade edilen siparişlerde kupon ve hediye çeki iade kuralı ne olacak? | Finans / Muhasebe |
| Q-02 | Depoda "hasarlı/kullanılmış" çıkan ürünler için prosedür ve müşteri iletişim kuralı nedir? | Finans + Depo |
| Q-03 | Hedeflenen işlem süresi (SLA) KPI değeri nedir? | İş sponsoru |
| Q-04 | Kısmi iadelerde sepet indirimlerinin ürünlere dağılım mantığı nasıl hesaplanacak? | Finans / Muhasebe |
| Q-05 | IBAN ile iadelerde IBAN alma sorumluluğu kimde (temsilci mi, muhasebe mi)? | İş sponsoru |

</details>

## Çıktı dosyaları

Tüm dokümanlar hem **tarayıcıda okunabilir Markdown** hem de **orijinal Office** formatında:

| # | Çıktı | Tarayıcıda oku | İndir |
|---|---|---|---|
| 01 | Paydaş listesi ve gereksinim soruları (15 soru) | [`01_paydas_ve_sorular.md`](docs/01_paydas_ve_sorular.md) | — |
| 02 | Gereksinim görüşmesi özeti | [`02_gorusme_ozeti.md`](docs/02_gorusme_ozeti.md) | — |
| 03 | BRD-lite (iş gereksinim dokümanı) | [`03_brd-lite.md`](docs/03_brd-lite.md) | [`.docx`](docs/03_brd-lite.docx) |
| 04 | User story'ler (11 story, 9 epic) | [`04_user_stories.md`](docs/04_user_stories.md) | [`.docx`](docs/04_user_stories.docx) |
| 05 | UAT dokümanı (12 senaryo + izlenebilirlik) | [`05_uat_dokumani.md`](docs/05_uat_dokumani.md) | [`.xlsx`](docs/05_uat_dokumani.xlsx) |
| — | As-is / to-be swimlane diyagramları | [`diagrams/`](diagrams/) | — |

## Yöntem ve araçlar

- **Paydaş analizi:** etki/ilgi düzeyi matrisi ve RACI ile 7 paydaşın rol ayrımı
- **Gereksinim toplama:** 6 kategoride 15 soruluk görüşme tasarımı (as-is, iş kuralları, hacim ve
  etki, istisnalar, hedef ve başarı ölçütü, kapsam ve kısıtlar) — görüşme simülasyon olarak yürütüldü
- **Süreç modelleme:** draw.io ile as-is / to-be swimlane akışları, kök neden etiketleme
- **Dokümantasyon:** BRD-lite, Given/When/Then user story'ler, UAT senaryoları
- **Sürüm yönetimi:** Git / GitHub

## Öğrendiklerim

**Paydaşın söylediği ile ihtiyacı olan aynı şey değil.** Görüşmeye "elle iş kalmasın" gibi tek
cümlelik bir taleple girdim; sorularımla **6 gizli iş kuralını** açığa çıkardım. Bunların hiçbiri
ilk talebin içinde yoktu — 14 günlük yasal süre, depo onayı olmadan ödeme yapılamaması, ödeme
tipine göre iade yöntemi gibi kurallar ancak doğru soru sorulduğunda ortaya çıktı.

**Dokümantasyonun en zor kısmı sadakat.** Gereksinim özetini paydaş onayından **3 iterasyonda**
geçirdim. İlk versiyondaki hatam, duyduğumu yazıya geçirirken sadakat kaybıydı: paydaşın
söylediğini kendi yorumumla sıkıştırıp anlamı kaydırıyordum. Bunu bir **geri bildirim
checklist'i** yöntemiyle çözdüm — her maddeyi paydaşın kendi ifadesiyle geri okutup teyit alarak.

**Belirsizliği gizlemek yerine kayıt altına almak.** SLA hedefi ve indirim dağılım mantığı gibi
kararları uyduracak yerde açık soru (Q-01…Q-05) olarak sahibi ve etkisiyle listeledim. Bir
gereksinim dokümanının olgunluğu, neyi bildiğinden çok **neyi bilmediğini ne kadar net
söylediğiyle** ölçülüyor.

## Kurgusallık notu

Bu proje eğitim ve portfolyo amacıyla kurgulanmış bir vaka çalışmasıdır. İçindeki şirket, kişi ve
süreçler kurgusaldır; gerçek şirket adı, gerçek müşteri verisi veya gerçek kurumsal ekran
görüntüsü içermez. Amaç, BRD yazımı, gereksinim analizi ve süreç modelleme yetkinliğini somut
çıktılarla göstermektir.

---

<div align="center">

**Senanur Yıldırım** · İş Analisti
[LinkedIn](https://www.linkedin.com/in/senanur-yildirim) · [senanur.yildirim.ba@gmail.com](mailto:senanur.yildirim.ba@gmail.com)

</div>
