# Gereksinim Görüşmesi Özeti

> P2 — Adım 2. Bu doküman, [`01_paydas_ve_sorular.md`](01_paydas_ve_sorular.md) dosyasında
> tasarlanan 15 soruluk görüşmenin sonuçlarını özetler. Görüşme, vaka çalışması kapsamında
> simülasyon olarak yürütülmüştür.

## Görüşmenin başlangıç noktası

Paydaşın (Satış Operasyonları Müdürü) getirdiği talep tek cümleydi:

> *"İptal-iade işleri artık sistemden yürüsün, elle iş kalmasın."*

Bu cümle bir gereksinim değil, bir semptom ifadesiydi. Görüşmenin amacı, bu cümlenin arkasındaki
gerçek iş kurallarını, kısıtları ve başarı ölçütlerini açığa çıkarmaktı.

## Görüşme notları

Paydaş görüşmesinde **6 gizli iş kuralını** sorularımla çıkardım. Gereksinim özetini **3
iterasyonda** paydaş onayından geçirdim. İlk versiyondaki hatam, duyduğumu dokümante ederken
sadakat kaybıydı; bunu **geri bildirim checklist'i** yöntemiyle çözdüm — her maddeyi paydaşın kendi
ifadesiyle geri okutup teyit alarak.

## Görüşmenin çıktıları nereye gitti

Görüşmede açığa çıkan bilgiler aşağıdaki dokümanlarda formalize edildi:

| Görüşmede ortaya çıkan | Nereye işlendi |
|---|---|
| Gizli iş kuralları | [BRD-lite § 7 — İş kuralları (BR-01…BR-05)](03_brd-lite.md#7-i̇ş-kuralları-business-rules) |
| Kargo görünürlüğü ve hatalı iptal sözü sorunu | [BRD-lite § 6 — Kök sorun 1](03_brd-lite.md#6-as-is-mevcut-durum-özeti) · [As-is diyagramı](../diagrams/as-is-swimlane.png) |
| Hacim ve etki verileri (ayda ~120 talep, 2 iş günü, 3 departman) | [BRD-lite § 1 ve § 6](03_brd-lite.md#1-yönetici-özeti) |
| Hedef ve başarı ölçütleri | [BRD-lite § 3 — Amaç ve hedefler](03_brd-lite.md#3-amaç-ve-hedefler) |
| Kapsam ve kanal kısıtları (yalnızca Türkiye B2C) | [BRD-lite § 4 — Kapsam](03_brd-lite.md#4-kapsam) |
| Görüşmede **netleşmeyen** kararlar | [BRD-lite § 11 — Açık sorular (Q-01…Q-05)](03_brd-lite.md#11-açık-sorular-ve-beklenen-kararlar) |

## Yöntemsel not

Görüşmede cevabı netleşmeyen konuları varsayımla kapatmak yerine sahibi ve hedef tarihiyle **açık
soru** olarak kaydettim (Q-01…Q-05). SLA hedefi ve kısmi iadelerde indirim dağılım mantığı bu
şekilde takibe alındı; ikisi de tasarımı doğrudan etkileyen kararlar olduğu için R-03 riskiyle
ilişkilendirildi.

---

<sub>Bu doküman iş analistliği portfolyosu kapsamında hazırlanmış bir vaka çalışmasıdır. Kişi, kurum
ve süreçler kurgusaldır.</sub>
