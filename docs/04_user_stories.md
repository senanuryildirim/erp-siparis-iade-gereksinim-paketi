# User Stories — İptal ve İade Süreçleri Otomasyonu

> **Not:** Bu dosya, [`04_user_stories.docx`](04_user_stories.docx) dosyasının tarayıcıda
> okunabilir Markdown sürümüdür. İçerik birebir aynıdır.

**9 epic · 11 user story.** Her story Given/When/Then kabul kriterleriyle ve üç senaryo katmanında
yazılmıştır: mutlu yol, alternatif yol, hata durumu.

| Epic | Story |
|---|---|
| [E1](#epic-e1--kargo--statü-görünürlüğü) · Kargo & statü görünürlüğü | US-01 |
| [E2](#epic-e2--sipariş-iptal-süreci) · Sipariş iptal süreci | US-02 |
| [E3](#epic-e3--i̇ade-süreci-yönlendirme--yasal-süre-kontrolü) · İade yönlendirme & yasal süre | US-03, US-04 |
| [E4](#epic-e4--kısmi-işlemler) · Kısmi işlemler | US-05 |
| [E5](#epic-e5--depo-kontrol-ve-onay) · Depo kontrol ve onay | US-06 |
| [E6](#epic-e6--ödeme-iadesi--muhasebe) · Ödeme iadesi & muhasebe | US-07, US-08 |
| [E7](#epic-e7--müşteri-iletişimi--bildirim) · Müşteri iletişimi & bildirim | US-09 |
| [E8](#epic-e8--stok-yönetimi) · Stok yönetimi | US-10 |
| [E9](#epic-e9--i̇zlenebilirlik-ve-audit) · İzlenebilirlik ve audit | US-11 |

---

## Epic E1 — Kargo & statü görünürlüğü

### US-01 — Güncel kargo durumu görüntüleme

`İlişkili FR: FR-01` · `İlişkili BR: BR-01` · `Öncelik: Must Have`

> **Bir müşteri temsilcisi olarak**, sipariş sorgulama ekranında ilgili siparişin güncel kargo
> durumunu görmek **istiyorum**, **böylece** kargodaki bir siparişe yanlışlıkla iptal sözü vermemiş
> ve süreci doğru operasyonel akışa yönlendirebilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (görünür kargo durumu)**
- **Given** müşteri temsilcisi bir sipariş sorguladığında,
- **When** sistemde geçerli kargo verisi mevcut olduğunda,
- **Then** sipariş ekranında "Kargo Statüsü" alanında güncel durum (Kargolanmadı / Kargoda /
  Teslim Edildi) ve kargo durumunun son güncellenme zamanı görüntülenmelidir.

**Senaryo 2 — Alternatif yol (henüz kargolanmamış sipariş)**
- **Given** müşteri temsilcisi depodan henüz çıkmamış bir siparişi sorguladığında,
- **When** sipariş için kargo kaydı henüz oluşmamışsa,
- **Then** "Kargo Statüsü" alanı "Kargolanmadı" olarak görünmeli ve iptal işlemi aktif olmalıdır.

**Senaryo 3 — Hata durumu (kargo verisi alınamıyor)**
- **Given** müşteri temsilcisi bir sipariş sorguladığında,
- **When** kargo verisi okunamıyor veya taze bilgiye ulaşılamıyorsa,
- **Then** ekranda "Kargo Durumu Belirsiz — Manuel Sorgulama Yapınız" uyarısı çıkmalı ve hatalı
  işlem yapılmaması için anlık iptal seçeneği pasifleşmelidir.

**Bağımlılıklar:** kargo firması veri entegrasyonu (R-01 riski).

---

## Epic E2 — Sipariş iptal süreci

### US-02 — Kargolanmamış siparişin anlık iptali ve neden kaydı

`İlişkili FR: FR-02, FR-13` · `İlişkili BR: BR-01` · `Öncelik: Must Have`

> **Bir müşteri temsilcisi olarak**, kargoya verilmemiş siparişler için anlık iptal işlemi
> başlatabilmek ve iptal nedenini seçebilmek **istiyorum**, **böylece** 2 iş günü süren manuel ters
> kayıt bekleme süresini ortadan kaldırır ve kök neden analizleri için veri oluşturabilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (anlık iptal işlemi)**
- **Given** sipariş kargo statüsü "Kargolanmadı" durumunda iken,
- **When** müşteri temsilcisi iptal işlemini onayladığında,
- **Then** sipariş statüsü anında "İptal Edildi" olarak değişmeli ve talep muhasebe iş kuyruğuna
  aktarılmalıdır.

**Senaryo 2 — Alternatif yol (iptal nedeni seçimi)**
- **Given** müşteri temsilcisi anlık iptal işlemini başlattığında,
- **When** sistem önceden tanımlı iptal nedeni listesini sunduğunda ve temsilci uygun nedeni
  seçtiğinde,
- **Then** seçilen iptal gerekçesi sipariş detay kaydına işlenerek işlem tamamlanmalıdır.

**Senaryo 3 — Hata durumu (eşzamanlı durum değişimi)**
- **Given** müşteri temsilcisi iptal onayı verdiğinde,
- **When** sipariş bu esnada durum değiştirip "Kargoda" statüsüne geçmişse,
- **Then** sistem "Sipariş bu esnada kargoya verildi — iade akışına yönlendiriliyor" uyarısı
  göstermeli ve süreci otomatik olarak iade akışına aktarmalıdır.

**Bağımlılıklar:** US-01.

---

## Epic E3 — İade süreci yönlendirme & yasal süre kontrolü

### US-03 — Kargolanmış ve teslim edilmiş siparişlerin iadeye yönlendirilmesi ve kargo kodu üretimi

`İlişkili FR: FR-03, FR-12` · `İlişkili BR: BR-01` · `Öncelik: Must Have`

> **Bir müşteri temsilcisi olarak**, kargodaki veya teslim edilmiş siparişler için sistemin
> doğrudan iade akışı başlatmasını ve iade kargo kodu üretmesini **istiyorum**, **böylece** iptal
> edilemeyecek siparişlerde hatalı işlem yapılmasını engeller ve müşteriye fiziken iade yapabileceği
> referansı anında sunabilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (iade akışının başlatılması)**
- **Given** kargo statüsü "Kargoda" veya "Teslim Edildi" olan bir sipariş sorgulandığında,
- **When** müşteri temsilcisi iade talebi oluşturduğunda,
- **Then** sistem doğrudan iade akışını başlatmalı ve talebe "İade Talebi Oluşturuldu" statüsü
  atanmalıdır.

**Senaryo 2 — Alternatif yol (müşteri iade kargo kodu üretimi)**
- **Given** iade talebi başarılı bir şekilde oluşturulduğunda,
- **When** sistem işlemi kaydettiğinde,
- **Then** ekran üzerinde müşterinin depoya gönderim yapabileceği benzersiz "İade Kargo Kodu /
  Referans Numarası" üretilmeli ve temsilci ekranında görüntülenmelidir.

**Senaryo 3 — Hata durumu (mükerrer iade talebi)**
- **Given** zaten devam eden bir iade talebi bulunan sipariş sorgulandığında,
- **When** müşteri temsilcisi tekrar iade başlatmaya çalıştığında,
- **Then** sistem "Bu sipariş için aktif bir iade süreci zaten mevcuttur" uyarısı vererek ikinci
  talebi engellemelidir.

**Bağımlılıklar:** US-01.

### US-04 — İade süreçlerinde 14 günlük yasal süre denetimi ve istisnalar

`İlişkili FR: FR-05, FR-15` · `İlişkili BR: BR-04` · `Öncelik: Must Have`

> **Bir müşteri temsilcisi olarak**, iade taleplerinde teslimat tarihinden itibaren 14 günlük yasal
> cayma süresinin sistem tarafından denetlenmesini **istiyorum**, **böylece** manuel tarih hesabı
> yapmaktan kurtulur ve süresi geçmiş haksız iadeleri kurala uygun şekilde süzebilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (yasal süre içinde iade)**
- **Given** teslimat tarihinden itibaren 14 takvim gününden az süre geçmiş bir sipariş için,
- **When** müşteri temsilcisi "Cayma Hakkı" nedeni ile iade talebi girdiğinde,
- **Then** sistem iade talebini onaylamalı ve depo kabul aşamasına aktarmalıdır.

**Senaryo 2 — Alternatif yol (ayıplı ürün muafiyeti ve belge zorunluluğu)**
- **Given** teslimat tarihinden itibaren 14 günden fazla süre geçmiş bir sipariş için,
- **When** müşteri temsilcisi iade nedenini "Ayıplı / Kusurlu Ürün" olarak seçtiğinde,
- **Then** sistem 14 gün engelini kaldırmalı, ancak açıklama veya destekleyici belge/görsel
  girilmesini zorunlu kılarak iade talebini kabul etmelidir.

**Senaryo 3 — Hata durumu (süre aşımı engeli)**
- **Given** teslimat tarihinden itibaren 15 gün veya daha fazla süre geçmiş bir sipariş için,
- **When** iade nedeni "Cayma Hakkı" seçildiğinde,
- **Then** sistem "Yasal 14 günlük iade süresi dolmuştur, işlem başlatılamaz" uyarısı vermeli ve
  işlemi kilitledikten sonra kaydetmemelidir.

**Bağımlılıklar:** US-03.

---

## Epic E4 — Kısmi işlemler

### US-05 — Kalem bazlı kısmi iptal ve iade yönetimi

`İlişkili FR: FR-06` · `İlişkili BR: BR-05` · `Öncelik: High`

> **Bir müşteri temsilcisi olarak**, çoklu ürün içeren siparişlerde sadece seçilen ürün kalemleri
> için kısmi iptal veya iade işlemi başlatabilmek **istiyorum**, **böylece** tüm siparişi bozmadan
> müşterinin parça taleplerini doğru tutarlarla karşılayabilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (seçili kalem iadesi/iptali)**
- **Given** içinde birden fazla ürün bulunan bir sipariş ekranında,
- **When** müşteri temsilcisi sadece belirli ürün kalemlerini seçip işlemi başlattığında,
- **Then** sistem sadece seçilen kalemlerin statüsünü değiştirmeli, seçilmeyen diğer ürünlerin
  sipariş durumunu aynen korumalıdır.

**Senaryo 2 — Alternatif yol (kampanyalı/indirimli sepette kısmi iade)**
- **Given** sepet indirimi veya kampanya uygulanmış çoklu bir siparişte kısmi iade/iptal
  yapıldığında,
- **When** işlem başlatıldığında,
- **Then** sistem kalan ürünlerin kampanya şartlarını koruyarak geri ödenecek net tutarı otomatik
  hesaplamalı ve ekranda vermelidir.

> *Not: kesin davranış Q-04 açık sorusunun kapanmasıyla netleşecektir; bu senaryo mevcut varsayıma
> dayanmaktadır.*

**Senaryo 3 — Hata durumu (ürün seçilmeden işlem yapılması)**
- **Given** kısmi işlem ekranında,
- **When** müşteri temsilcisi hiçbir ürün satırını işaretlemeden onay butonuna bastığında,
- **Then** "Lütfen işlem yapılacak en az bir ürün kalemi seçiniz" uyarısı gösterilmeli ve
  ilerlemeye izin verilmemelidir.

**Bağımlılıklar:** US-02, US-03, Q-04 açık sorusu.

> *Not: sprint tahmininde iptal ve iade kolları ayrı alt görevler/task'lar olarak modellenmelidir.*

---

## Epic E5 — Depo kontrol ve onay

### US-06 — Depo fiziksel iade kabul ve kalite değerlendirmesi

`İlişkili FR: FR-07` · `İlişkili BR: BR-02` · `Öncelik: Must Have`

> **Bir depo sorumlusu olarak**, depoya fiziken ulaşan iade ürünlerin kalite kontrol sonucunu
> sisteme girebilmek **istiyorum**, **böylece** muhasebe ödeme adımına güvenli girdi sağlayabilir ve
> sistem dışı haberleşme ihtiyacını ortadan kaldırabilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (ürün iadeye uygun kabul edildi)**
- **Given** depoya fiziken ulaşmış bir iade ürünü incelendiğinde,
- **When** depo sorumlusu "İadeye Uygun — Kabul" onayını verdiğinde,
- **Then** iade talebi sistemde "Depo Onaylı" durumuna geçmeli ve muhasebe onay kuyruğuna
  yansımalıdır.

**Senaryo 2 — Alternatif yol (kullanılmış veya hasarlı ürün reddi)**
- **Given** depoya ulaşan ürün fiziken hasarlı veya yeniden satışa uygunsuz olduğunda,
- **When** depo sorumlusu "İadeye Uygun Değil — Red" seçeneğini işaretleyip gerekçe açıklamasını
  girdiğinde,
- **Then** iade talebi reddedilmeli ve red gerekçesi müşteri temsilcisi ekranına düşmelidir.

**Senaryo 3 — Hata durumu (gerekçesiz red verme denemesi)**
- **Given** depo sorumlusu iade ürününü reddetmek istediğinde,
- **When** gerekçe/açıklama alanını doldurmadan işlemi kaydetmeye çalıştığında,
- **Then** sistem "Red işlemlerinde açıklama girilmesi zorunludur" uyarısı vererek kaydı
  engellemelidir.

**Bağımlılıklar:** US-03.

---

## Epic E6 — Ödeme iadesi & muhasebe

### US-07 — Orijinal ödeme yöntemine göre iade işlemi

`İlişkili FR: FR-04, FR-08` · `İlişkili BR: BR-03` · `Öncelik: Must Have`

> **Bir muhasebe uzmanı olarak**, depo onayı tamamlanmış taleplerin ödemesini orijinal ödeme
> yöntemine (kart/IBAN) uygun olarak sistem üzerinden onaylamak **istiyorum**, **böylece** Excel
> takibine ihtiyaç duymadan mali süreçleri doğrudan sistem içerisinden hatasız tamamlayabilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (kredi kartına iade onayı)**
- **Given** kredi kartı ile ödenmiş ve depo onayı almış bir iade muhasebe ekranına düştüğünde,
- **When** muhasebe uzmanı ödemeyi onayladığında,
- **Then** sistem ödemeyi kartın yapıldığı kanala geri yansıtmalı ve iade statüsünü "Tamamlandı"
  olarak güncellemelidir.

**Senaryo 2 — Alternatif yol (havale / EFT iadesi)**
- **Given** havale/EFT ile yapılmış bir ödemenin iadesi onaylandığında,
- **When** muhasebe uzmanı ödeme iade ekranını açtığında,
- **Then** sistem siparişle eşleşen IBAN bilgisini getirmeli ve EFT onayının tek adımla verilmesini
  sağlamalıdır.

**Senaryo 3 — Hata durumu (IBAN bilgisi eksikliği)**
- **Given** havale/EFT ödemeli bir iade işleminde,
- **When** sistemde geçerli bir IBAN bilgisi tanımlı değilse,
- **Then** sistem "IBAN bilgisi eksik — müşteri temsilcisinden bilgi bekleniyor" uyarısı vererek
  ödeme butonunu pasif tutmalıdır.

> *Not: kesin davranış Q-05 açık sorusunun kapanmasıyla netleşecektir; bu senaryo mevcut varsayıma
> dayanmaktadır.*

**Bağımlılıklar:** US-06, Q-05 açık sorusu.

### US-08 — Depo onayı olmayan taleplerde ödeme kısıtı ve yetkilendirme

`İlişkili FR: FR-07` · `İlişkili BR: BR-02` · `İlişkili NFR: NFR-04` · `Öncelik: Must Have`

> **Bir muhasebe uzmanı olarak**, depo tarafından henüz fiziken incelenip onaylanmamış iadelerde
> ödeme adımı yapamamayı **istiyorum**, **böylece** depoya gelmeyen veya reddedilen ürünler için
> hatalı ödeme yapma riskini sıfırlayabilir ve rol yetkilerine uygun hareket edebilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (kilitli ödeme alanı)**
- **Given** depo kabul kontrolü henüz yapılmamış bir iade talebi sorgulandığında,
- **When** muhasebe uzmanı iade detay ekranını açtığında,
- **Then** ödeme onay butonları kilitli görünmeli ve "Depo Onayı Bekleniyor" uyarısı
  görüntülenmelidir.

**Senaryo 2 — Alternatif yol (depo reddi durumunda ödeme kapatma)**
- **Given** depo tarafından "Red" verilmiş bir iade talebi olduğunda,
- **When** talep muhasebe ekranına düştüğünde,
- **Then** sistem ödeme imkânını tamamen kapatmalı ve iade durumunu "Ödeme Yapılamaz —
  Reddedildi" şeklinde işaretlemelidir.

**Senaryo 3 — Hata durumu (doğrudan erişim engeli)**
- **Given** depo onayı bulunmayan bir talep için,
- **When** ödeme onay adımı yetkisiz şekilde tetiklenmeye çalışılırsa,
- **Then** sistem kaydı reddetmeli ve "Depo onayı olmadan ödeme işlemi gerçekleştirilemez" uyarısı
  basmalıdır.

**Bağımlılıklar:** US-06.

---

## Epic E7 — Müşteri iletişimi & bildirim

### US-09 — Süreç adımlarında otomatik müşteri bilgilendirmesi

`İlişkili FR: FR-09` · `İlişkili BR: Genel` · `Öncelik: Must Have`

> **Bir müşteri olarak**, iptal ve iade sürecimin önemli aşamalarında (talep alındı, iade
> onaylandı, ödeme yapıldı) tarafıma otomatik bilgilendirme yapılmasını **istiyorum**, **böylece**
> sürecin durumu hakkında bilgi sahibi olur ve destek hattını tekrar aramak zorunda kalmam.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (iade / iptal tamamlandı bildirimi)**
- **Given** bir iptal veya iade süreci başarıyla tamamlandığında ve ödeme onayı verildiğinde,
- **When** işlem statüsü "Tamamlandı" olarak güncellendiği an,
- **Then** müşterinin kayıtlı iletişim kanallarına (SMS/e-posta) işlemin sonuçlandığını bildiren
  mesaj otomatik iletilmelidir.

**Senaryo 2 — Alternatif yol (iade kabul / red bildirimi)**
- **Given** depo sorumlusu iade ürün incelemesini tamamladığında,
- **When** kabul veya red kararı sisteme girildiği an,
- **Then** müşteriye durum bilgilendirmesi (kabul edildi / reddedildi gerekçesi) mesajı otomatik
  tetiklenmelidir.

**Senaryo 3 — Hata durumu (eksik iletişim bilgisi)**
- **Given** otomatik bildirim gönderileceği esnada,
- **When** müşterinin kayıtlı iletişim bilgisi eksik veya hatalı ise,
- **Then** sistem işlem akışını durdurmamalı, bildirim durumunu "Gönderilemedi" olarak işaretleyip
  loglamalıdır.

**Bağımlılıklar:** US-02, US-07.

---

## Epic E8 — Stok yönetimi

### US-10 — Kabul edilen iadelerde kategori bazlı otomatik stok güncellemesi

`İlişkili FR: FR-10, FR-14` · `İlişkili BR: BR-02` · `Öncelik: Must Have`

> **Bir depo sorumlusu olarak**, iadesini onayladığım ürünlerin stok miktarının kabul durumuna göre
> sistemde otomatik güncellenmesini **istiyorum**, **böylece** elle stok girerken yapılan hataları
> ve aynı ürünün tekrar satılamaması veya mükerrer satılması riskini engellerim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (satılabilir stok artışı)**
- **Given** depo sorumlusu bir ürünün iadesini "Uygun" olarak onayladığında,
- **When** iade kabul kaydı tamamlandığı an,
- **Then** sistem ilgili ürünün "Satılabilir Stok" miktarını onaylanan adet kadar anında
  artırmalıdır.

**Senaryo 2 — Alternatif yol (hasarlı ürün — ayrılmış stok kategorisi)**
- **Given** iade gelen ürün "Hasarlı / Yeniden Satışa Uygun Değil" olarak değerlendirildiğinde,
- **When** depo kaydı kapatıldığında,
- **Then** ürün satılabilir stok alanına eklenmemeli, otomatik olarak "İnceleme / Hasarlı Stoğu"
  kategorisine aktarılmalıdır.

> *Not: kesin kategori yapısı Q-02 açık sorusunun kapanmasıyla netleşecektir; bu senaryo mevcut
> varsayıma dayanmaktadır.*

**Senaryo 3 — Hata durumu (stok güncelleme aksaması)**
- **Given** iade kabulü yapıldığı anda,
- **When** sistemsel bir aksama nedeniyle stok miktarı güncellenemezse,
- **Then** sistem iade onayını askıya almalı ve ekranda "Stok güncellenemedi, lütfen işlemi
  tekrarlayınız" hatası göstermelidir.

**Bağımlılıklar:** US-06, Q-02 açık sorusu.

---

## Epic E9 — İzlenebilirlik ve audit

### US-11 — İptal ve iade süreçlerinde denetim izi (audit log) takibi

`İlişkili NFR: NFR-03` · `İlişkili BR: Genel` · `Öncelik: High`

> **Bir sistem yöneticisi olarak**, iptal ve iade süreçlerinde gerçekleşen tüm adımların kullanıcı,
> zaman ve durum bazlı kayıt altına alınmasını **istiyorum**, **böylece** olası uyuşmazlıklarda veya
> iç denetim süreçlerinde geçmişe dönük işlemleri şeffaflıkla izleyebilirim.

**Kabul kriterleri**

**Senaryo 1 — Mutlu yol (kullanıcı işlem kaydı)**
- **Given** herhangi bir kullanıcı (temsilci/depo/muhasebe) bir iptal veya iade adımı
  gerçekleştirdiğinde,
- **When** işlem onaylandığında,
- **Then** sistem yapılan işlemi, işlemi yapan kullanıcı bilgisini, zaman damgasını ve eski/yeni
  durum bilgisini tarihçeye kaydetmelidir.

**Senaryo 2 — Alternatif yol (işlem tarihçesi inceleme)**
- **Given** yetkili bir kullanıcı sipariş detay ekranına girdiğinde,
- **When** "İşlem Tarihçesi" sekmesini seçtiğinde,
- **Then** siparişe ait tüm iptal/iade adımları kronolojik sırayla ekranda listelenmelidir.

**Senaryo 3 — Hata durumu (tarihçe kaydı alınamaması)**
- **Given** bir operasyonel işlem yürütülürken,
- **When** sistem denetim kaydını teknik bir aksama nedeniyle oluşturamıyorsa,
- **Then** sistem denetim kaydını yeniden denemeli; başarısızlık devam ederse yönetici uyarısı
  üretmeli, ancak iş akışının kesilmemesi için ana işlemi tamamlamalıdır.

**Bağımlılıklar:** US-02, US-06, US-07.

---

<sub>Bu doküman iş analistliği portfolyosu kapsamında hazırlanmış bir vaka çalışmasıdır. Kişi, kurum
ve süreçler kurgusaldır.</sub>
