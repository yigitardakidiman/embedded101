# Embedded 101 - Gömülü Sistemler ve Yazılım Temelleri

Bu depo, sıfırdan elektrik-elektronik ve gömülü yazılım temellerini öğrenmek, mikrodenetleyici mimarilerini derinlemesine kavramak ve sektörel mühendislik pratikleri kazanmak amacıyla oluşturulmuştur.

Öğrenme süreci ve eğitmen çalışma ilkeleri [agent.md](agent.md) dosyasında tanımlanan pedagojik standartlara göre yürütülmektedir.

---

## 🎯 Müfredat ve Yol Haritası

- [x] **1. Bit, Byte ve Sayı Sistemleri**
  - Binary, Decimal, Hexadecimal tabanlar ve dönüşümler
  - Bit manipülasyonu (`AND`, `OR`, `XOR`, `NOT`, bit kaydırma `<<`, `>>`)
  - Register seviyesinde bit set/clear/toggle uygulamaları
- [x] **2. Lojik Kapılar**
  - AND, OR, NOT, XOR kapıları ve doğruluk tabloları
  - Aktif-Düşük (Active-LOW) ve donanımsal mantık
- [ ] **3. Elektriksel Sinyaller** *(Devam Ediyor)*
  - HIGH/LOW seviyeleri, floating durumları ve gerilim eşikleri
  - Yükselen (Rising) ve düşen (Falling) kenar tetikleme
  - Kare dalga, sinüs, periyot ($T$) ve frekans ($f$)
  - PWM (Pulse Width Modulation), Duty Cycle ve ortalama gerilim
  - STEP sinyalleri ve motor kontrol temelleri
- [ ] **4. Programlama Temelleri**
  - Değişkenler, veri tipleri, değişken isimlendirme, atama işlemleri
  - Kontrol yapıları (`if-else`, `switch-case`)
  - Döngüler (`for`, `while`)
  - Fonksiyonlar ve diziler
- [ ] **5. Algoritma Kurma**
  - Problem analizi, adım adım çözüm geliştirme
  - Akış diyagramları ve sözde kod (pseudocode) oluşturma
- [ ] **6. Geliştirme Ortamları ve İşletim Sistemleri**
  - IDE, VS Code, derleyici zinciri (toolchain)
  - Linux, Ubuntu ve gömülü geliştirme ortamları
- [ ] **7. Mikrodenetleyici Mimarisi**
  - MCU, CPU, RAM, Flash bellek organizasyonu
  - GPIO yapısı, donanımsal yazmaçlar (registers) ve firmware kavramı
- [ ] **8. Haberleşme Protokolleri**
  - UART, I2C ve SPI haberleşmelerinin donanımsal mimarisi, farkları ve paket yapıları
- [ ] **9. Pointer ve Struct**
  - Bellek adresleri, işaretçi mantığı (`*`, `&`), struct yapıları ve bellek haritalama
- [ ] **10. Interrupt (Kesme)**
  - Kesme mantığı, Polling ile farkı, Kesme Servis Rutinleri (ISR) ve önceliklendirme
- [ ] **11. Timer ve Zamanlama**
  - Donanımsal sayaçlar (Timers), bloklayan (`delay`) ve bloklamayan (`millis`/zamanlayıcı tabanlı) kod mimarisi
- [ ] **12. Debug ve Datasheet Okuma**
  - Datasheet ve referans kılavuzu okuma, register haritaları, Serial debug ve breakpoint kullanımı
