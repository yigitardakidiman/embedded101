# 6. Konu: Geliştirme Ortamları ve İşletim Sistemleri

Bu doküman; gömülü yazılım geliştirme sürecinde kullanılan metin editörleri, derleyiciler, IDE'ler ile Linux çekirdeği, dağıtımlar ve Gömülü Linux kullanım alanlarını içerir.

---

## 6.1 Metin Editörü, Derleyici (Compiler) ve IDE Kavramları

Mikrodenetleyiciler sadece $0$ ve $1$'lerden oluşan **Makine Kodunu (Binary)** çalıştırır. C kodunu bu seviyeye taşımak için üç temel araç kullanılır:

1. **Metin Editörü (Text Editor):** Sadece kod yazılan metin düzenleyicidir (Saf VS Code, Sublime Text). Kodu derleyemez veya donanıma yükleyemez.
2. **Derleyici (Compiler):** C kodunu işlemcinin makine diline çeviren programdır.
   - **Native Compiler:** Kendi bilgisayarında çalışıp yine kendi bilgisayarın için kod üreten derleyicidir (`gcc`).
   - **Cross-Compiler (Çapraz Derleyici):** Kendi bilgisayarında (x86) çalışıp, hedef mikrodenetleyici (ARM Cortex-M, AVR vb.) için makine kodu üreten derleyicidir (`arm-none-eabi-gcc`). Gömülü sistemlerde kural olarak çapraz derleyiciler kullanılır.
3. **IDE (Tümleşik Geliştirme Ortamı):** Editör, derleyici, hata ayıklayıcı (debugger) ve kod yükleme araçlarını tek bir arayüzde toplayan yazılımdır (STM32CubeIDE, Keil MDK, MPLAB X).

### Soru - Cevap
> **Soru:**  
> 1) Bilgisayarında yazılan kodu ARM mikrodenetleyicinin makine koduna çeviren derleyici türü nedir?  
> 2) Saf VS Code kurup içine C kodu yazarsan harici bir derleyici olmadan bunu MCU'da çalıştırabilir misin?  
> **Öğrenci Cevabı:** 1) Cross compiler. 2) Çalıştıramam çünkü saf VS Code bir metin editörüdür, makine diline çevirmek için compiler gereklidir.  
> **Değerlendirme:** Tamamen doğru.

---

## 6.2 Çekirdek (Kernel), Linux ve Ubuntu Arasındaki Fark

- **Çekirdek (Kernel):** Donanım (CPU, RAM, depolama, çevre birimleri) ile üst seviye yazılımlar arasında köprü olan en alt seviye işletim sistemi bileşenidir.
- **Linux:** 1991'de Linus Torvalds tarafından yazılan bir **Çekirdektir (Kernel)**.
- **Dağıtım (Distribution):** Linux çekirdeğinin etrafına paket yöneticisi, masaüstü arayüzü ve sistem araçları eklenerek oluşturulan tam işletim sistemidir (Örn: Ubuntu, Debian, Fedora).
- **Gömülü Linux (Embedded Linux):** Yüksek işlem gücü, ağ desteği (Ethernet, WiFi), dosya sistemleri ve grafik arayüz gerektiren güçlü işlemcilerde (Raspberry Pi, akıllı TV, araç eğlence sistemleri) tercih edilir.
- **Determinizm ve Gerçek Zamanlılık (Real-Time):** Can güvenliği ve anlık tepki gerektiren sistemlerde (ABS fren, hava yastığı, motor kontrol) Linux'un zamanlama belirsizliklerinden kaçınmak için doğrudan **Bare-Metal (çıplak C)** veya **RTOS** çalıştıran mikrodenetleyiciler kullanılır.

### Soru - Cevap
> **Soru:**  
> 1) Linux ile Ubuntu arasındaki teknik ilişki nedir?  
> 2) Bir otomobilde ABS Fren Sistemi ile Navigasyon ekranında sırasıyla Gömülü Linux mu yoksa Bare-Metal MCU mu kullanılır? Neden?  
> **Öğrenci Cevabı:** 1) Linux bir kernel'dir (çekirdektir). Ubuntu ise Linux tabanlı bir dağıtımdır. 2) Navigasyonda Gömülü Linux, ABS'de Bare-Metal MCU kullanılır.  
> **Değerlendirme:** Doğru. ABS'de gecikmeye (jitter) tahammül yoktur; gerçek zamanlı (real-time) kesin determinizm şarttır.
