# 8. Konu: Haberleşme Protokolleri (UART, I2C, SPI)

Bu doküman; gömülü sistemlerde donanımlar ve çipler arası veri aktarımında kullanılan üç temel seri haberleşme protokolünü, donanımsal kablolama kurallarını, çalışma prensiplerini ve interaktif soru-cevap kayıtlarını içerir.

---

## 8.1 Seri Haberleşmeye Giriş ve UART Protokolü

- **Seri Haberleşme:** Bitlerin tek bir hat üzerinden arka arkaya (sırayla) aktığı iletişim yöntemidir.
- **UART (Universal Asynchronous Receiver-Transmitter):**
  - **Hatlar:**
    - `TX` (Transmit): Veri gönderen pin.
    - `RX` (Receive): Veri dinleyen pin.
    - `GND` (Toprak): Voltaj potansiyel referansını eşitlemek için **ortak bağlanması zorunludur.**
  - **Çapraz Bağlantı:** Cihaz 1 `TX` $\rightarrow$ Cihaz 2 `RX`, Cihaz 1 `RX` $\leftarrow$ Cihaz 2 `TX`.
  - **Asenkron Yapı ve Baud Rate:** Ayrı bir saat (Clock) hattı yoktur. Her iki cihazın da saniyedeki bit aktarım hızını (**Baud Rate**, örn: 9600, 115200 bps) önceden **birebir aynı** ayarlaması şarttır. Uyuşmazlık durumunda zamanlama kayar ve veri tamamen bozuk (çöp veri) okunur.
  - **Paket (Frame) Akışı:** Hat boştayken HIGH ($1$).
    - `Start Biti (0)` $\rightarrow$ `8 Bit Veri (D0-D7)` $\rightarrow$ `Stop Biti (1)`.

### Soru - Cevap
> **Soru:**  
> 1) İki MCU'yu UART ile bağlarken 1. MCU'nun `TX` pini nereye bağlanır? Neden `GND` pinleri de bağlanmak zorundadır?  
> 2) Gönderici $115200$ Baud iken alıcı yanlışlıkla $9600$ Baud yapılırsa alıcı veriyi doğru okuyabilir mi? Neden?  
> **Öğrenci Cevabı:**  
> 1) `TX` ucunu `RX` ucuna bağlamamız gerekir. `GND`'ye bağlama sebebimiz aradaki voltaj değerini birbirine göre doğru okuyabilmeleridir.  
> 2) Doğru okuyamaz; çünkü UART'ta 2 cihaz arasında clock olmadığı için alıcının da vericinin de Baud Rate değeri aynı olmalıdır.  
> **Değerlendirme:** Tamamen doğru.

---

## 8.2 I2C (Inter-Integrated Circuit) Protokolü

- **Genel Yapı:** Philips (NXP) tarafından kart üzerindeki sensör, RTC ve ekranları minimum bacakla bağlamak için geliştirilmiş **2 hatlı senkron** protokoldür.
- **Hatlar:**
  - `SDA` (Serial Data): Verinin çift yönlü (Half-Duplex) aktığı tek hat.
  - `SCL` (Serial Clock): İletişimin ritmini belirleyen saat hattı.
- **Master - Slave ve Senkron Yapı:**
  - Master (MCU) saat sinyalini (`SCL`) üretir. Saat vurdukça `SDA` üzerinden bitler okunur; bu nedenle Baud Rate ayarı gerekmez.
- **Adresleme Mantığı (Bus / Veri Yolu):**
  - Aynı `SDA` ve `SCL` hatlarına onlarca farklı sensör paralel bağlanabilir.
  - Her Slave cihazın benzersiz bir **7-bitlik donanım adresi** vardır (Örn: `0x68`, `0x3C`). Master hatta önce konuşacağı cihazın adresini gönderir.
- **Open-Drain ve Pull-Up Şartı:** Hatların kısa devre olmaması için pinler Open-Drain modunda çalışır ve hatlara harici **Pull-up dirençleri** bağlanması zorunludur.

### Soru - Cevap
> **Soru:**  
> 1) I2C'de UART'ın aksine neden önceden bir Baud Rate anlaşması şart değildir? Ritmi kim belirler?  
> 2) Aynı SDA/SCL hattına aynı anda hem sıcaklık sensörü hem OLED ekran bağlandığında MCU bu iki cihazla karışıklık yaşamadan nasıl iletişim kurar?  
> **Öğrenci Cevabı:**  
> 1) Çünkü I2C'de `SCL` denen clock hattı vardır ve bu iletişim hızını kontrol eder.  
> 2) Her modülün kendi kodu (adresi) vardır ve bu sayede mikroişlemci o kod ile ilgili sensörle haberleşir.  
> **Değerlendirme:** Tamamen doğru.

---

## 8.3 SPI (Serial Peripheral Interface) Protokolü

- **Genel Yapı:** Motorola tarafından geliştirilen, çok yüksek veri hızlarına ($10\text{ MHz} - 50\text{ MHz}+$) çıkabilen **senkron ve Full-Duplex** protokoldür.
- **4 Hatlı Mimari:**
  - `SCK` (Serial Clock): Master tarafından üretilen saat hattı.
  - `MOSI` (Master Out Slave In): Master'dan Slave'e veri hattı.
  - `MISO` (Master In Slave Out): Slave'den Master'a veri hattı.
  - `CS` / `SS` (Chip Select / Slave Select): Hedef çipi seçen hat (Genellikle **Active-LOW**).
- **Full-Duplex Üstünlüğü:** `MOSI` ve `MISO` ayrı hatlar olduğu için Master veri gönderirken **aynı anda** Slave'den veri alabilir (I2C ise Half-Duplex'tir; gönderme ve alma sırayladır).
- **Adresleme Farkı (Fiziksel Seçim):**
  - SPI'da yazılımsal 7-bit adres **yoktur**.
  - `SCK`, `MOSI`, `MISO` hatları tüm cihazlara paralel gider; ancak **her yeni Slave için MCU'dan ayrı bir `CS` pini** çekilir.
  - Çok fazla sensör bağlanırsa MCU'nun GPIO bacakları hızla tükenir.

### Soru - Cevap
> **Soru:**  
> 1) SPI hattına 3 ayrı sensör eklemek istersen MCU'dan o sensörlere fazladan hangi pinden kaç tane çekmek zorunda kalırsın?  
> 2) SPI'ın I2C'ye göre en büyük iki teknik üstünlüğü nedir?  
> **Öğrenci Yanıtı:**  
> 2) I2C'de Half-Duplex vardır, aynı anda çift yönlü veri iletimi yapılmaz. SPI'da ise Full-Duplex olup aynı anda çift yönlü yapılabilir ve SPI çok daha hızlıdır.  
> 1) Rehberlik istendi $\rightarrow$ Her sensörün bağımsız CS pini gerektirdiği açıklandı.  
> **Öğrenci Düzeltmesi:** Fazladan 2 pin (toplam 3 adet CS pini) bağlanmalıdır.  
> **Değerlendirme:** Tamamen doğru.

---

## 8.4 Protokollerin Karşılaştırması ve Seçim Kriterleri

| Kriter | UART | I2C | SPI |
|---|---|---|---|
| **Pin / Kablo** | 2 (TX, RX) + GND | 2 (SDA, SCL) + GND | 4+ (MOSI, MISO, SCK, CS) + GND |
| **Senkron / Asenkron** | Asenkron (Clock yok) | Senkron (SCL) | Senkron (SCK) |
| **İletişim Yönü** | Full-Duplex | Half-Duplex | Full-Duplex |
| **Hız Seviyesi** | Düşük/Orta ($9.6 - 115.2\text{ kbps}$) | Orta ($100 - 400\text{ kHz}$) | **Çok Yüksek** ($10 - 50\text{ MHz}+$) |
| **Çoklu Cihaz** | Yalnızca 2 cihaz arası | **Mükemmel** (Adresleme ile onlarca cihaz) | İyi (Her cihaza ayrı CS pini gerekir) |

### Soru - Cevap (Seçim Senaryosu)
> **Soru:** Bir Akıllı Saat projesinde:  
> 1) Yüksek çözünürlüklü renkli TFT ekran,  
> 2) Nabız ve ivmeölçer sensörü,  
> 3) Bluetooth haberleşme modülü  
> için sırasıyla hangi protokoller seçilmelidir?  
> **Öğrenci Cevabı:**  
> 1) **SPI** (Yüksek hız ihtiyacı için).  
> 2) **I2C** (Düşük veri, az pin tasarrufu için).  
> 3) **UART** (Bluetooth paket iletişimi için).  
> **Değerlendirme:** Mühendislik seçimi %100 doğrudur.
