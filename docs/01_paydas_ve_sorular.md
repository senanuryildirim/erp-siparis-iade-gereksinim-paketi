# Paydaş Listesi ve Gereksinim Soruları

> P2 — Adım 1. Bu doküman, gereksinim görüşmesi (simülasyon) öncesinde hazırlanmıştır.

## 1. Paydaş Listesi

| Paydaş | Rolü | Süreçteki İlgisi | Etki / İlgi Düzeyi |
|---|---|---|---|
| Satış Operasyon Müdürü | Talep sahibi (sponsor) | Talebin sahibi; ekibinin manuel iş yükünden ve müşteri şikayetlerinden doğrudan sorumlu. Yeni akışın kapsamına ve önceliklerine karar verecek kişi. | Yüksek / Yüksek |
| Satış Uzmanları | Son kullanıcı | Sipariş girişlerini yapan ve süreci başlatan ekip. Yeni akışın getireceği operasyonel kolaylıktan (manuel iş yükünün azalması, hız ve hatasızlık) doğrudan etkilenecek ana gruptur.| Orta / Yüksek |
| Depo / Lojistik Ekibi | Son kullanıcı | İade akışındaki fiziksel ürün kontrolünü, kargo kabulünü ve stok güncelleme adımlarını sahada doğrudan yönetecek taraf. | Orta / Yüksek |
| Muhasebe | Son kullanıcı | Faturalandırma, cari takibi ve ödeme süreçlerinden sorumlu ekip. Sipariş bilgilerinin ERP'ye hatasız aktarılması, fatura uyuşmazlıklarını ve manuel düzeltme süreçlerini azaltacaktır | Orta / Yüksek |
| Oracle ERP Yöneticisi / IT | Teknik paydaş | Yeni iş akışının teknik olarak tasarlanması, entegrasyonların yapılması ve sistemin stabil çalıştırılmasından sorumlu. Geliştirme ve bakım süreçlerinin mimarıdır. | Yüksek / Yüksek |
| Müşteri | Dolaylı paydaş | Siparişinin hızlı ve hatasız teslim edilmesini bekleyen nihai alıcı. Sürecin iyileşmesi, müşteri memnuniyeti ve teslimat sürelerinin kısalması olarak doğrudan müşteriye yansır. | Düşük / Yüksek |

## 2. Gereksinim Görüşmesi Soruları (15 soru)

Sorular kategorilere ayrılmıştır. Görüşmede muğlak talep şuydu:
*"İptal-iade işleri artık sistemden yürüsün, elle iş kalmasın."*

### Mevcut süreç (as-is)
1. _Bugün bir iptal talebi geldiğinde adım adım ne oluyor? Kim, hangi sistemde, ne yapıyor?_
2. _Müşteriden iptal/iade talebi telefon veya e-posta ile geldiği andan itibaren, sisteme girilene kadar arada hangi manuel adımlar atılıyor?_
3. _Kargoya verilmiş bir siparişe yanlışlıkla iptal sözü verildiği bir durum yaşandı mı? En son yaşandığında sonrasında ne oldu — kim fark etti, nasıl çözüldü?_

### İş kuralları
4. _Müşteri arayıp 'siparişimi iptal etmek istiyorum' dediğinde, hangi aşamadaki siparişleri anında iptal edebiliyorsunuz, hangilerini edemiyorsunuz?_
5. _Bir iade sürecinde stok güncellemesi ve müşteriye ödeme yapılabilmesi için sistemde kimin, neyi onaylaması gerekiyor — muhasebenin ödeme yapabilmesinin ön koşulları neler?_
6. _Bu süreci tasarlarken uymak zorunda olduğumuz yasal düzenlemeler veya sözleşmesel kısıtlar var mı — örneğin iade taleplerine dair süre sınırları gibi, neler?_

### Hacim ve etki
7. _Ayda gelen 120 talebin yaklaşık yüzde kaçı ürün kargoya verilmeden önceki anlık iptallerden, yüzde kaçı kargo ulaştıktan sonraki iadelere dönüşüyor?_
8. _2 iş günü süren bu manuel süreçte, geciken tek bir iptal/iade talebi ortalama kaç kez müşteri şikayeti veya mükerrer telefon araması olarak Satış ekibine geri dönüyor?_

### İstisnalar ve sınır durumları
9. _Depoya geri gelen ürün hasarlı, eksik veya kullanılmış çıkarsa süreç nasıl ilerliyor?_
10. _Bir siparişin içinde birden fazla ürün varsa ve müşteri kısmi iptal gerçekleştirmek istiyorsa, Oracle üzerinde mevcut sipariş güncelleniyor mu yoksa eski sipariş kapatılıp yeni bir sipariş mi açılıyor?_
11. _Müşteri siparişi taksitle veya hediye çeki/kupon kullanarak aldıysa, muhasebe tarafında iade tutarı ve yöntemi nasıl hesaplanıyor?_

### Hedef ve başarı ölçütü
12. _Yeni sistem devreye girdiğinde, şu an 2 iş günü süren iptal işleminin ideal olarak kaç dakikaya veya saate düşmesini hedefliyorsunuz?_
13. _Sistem canlıya alındıktan altı ay sonra dönüp baktığınızda, neye bakarak 'bu iş başarılı oldu' diyeceksiniz?_

### Kapsam ve kısıtlar
14. _Bu yeni yapacağımız sistem sadece e-ticaret ya da perakende gibi belirli bir satış kanalı için mi geçerli olacak, yoksa şirketin tüm toptan ve kurumsal satışlarındaki iptal/iade süreçlerini de kapsayacak mı?_
15. _Mevcut sistemimiz Oracle ERP olduğuna göre, yeni geliştireceğimiz ekranlar ve akışlar tamamen Oracle'ın içinde mi çözülmeli, yoksa dışarıya farklı bir yazılım/arayüz entegre etme şansımız var mı?_

## 3. Görüşme Notları

Görüşmenin sonuçları, çıktıların hangi dokümana işlendiğini gösteren eşleme tablosuyla birlikte
ayrı bir dosyada toplanmıştır: **[02_gorusme_ozeti.md](02_gorusme_ozeti.md)**

