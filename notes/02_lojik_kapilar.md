# 2. Konu: Lojik Kapılar

Bu doküman, mikrodenetleyicilerin işlemci (CPU) ve Aritmetik Mantık Biriminin (ALU) temel yapı taşları olan lojik kapıları, doğruluk tablolarını, elektriksel aktiflik seviyelerini ve soru-cevap kayıtlarını içerir.

---

## 2.1 Lojik Kapı Kavramı ve AND (VE) Kapısı

- Lojik kapılar, transistörlerin bir araya gelmesiyle mantıksal kararlar üreten dijital devre elemanlarıdır.
- **AND Kapısı:** Mantıksal çarpma işlemidir ($Y = A \cdot B$).
  - Çıkışın HIGH ($1$) olabilmesi için **tüm girişlerin HIGH ($1$) olması şarttır**.

| Giriş A | Giriş B | Çıkış Y |
|:---:|:---:|:---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | **1** |

- **Donanım Örneği:** Seri bağlı iki mekanik anahtar veya CNC tezgâhında "Güvenlik Kapağı Kapalı" (1) VE "Başlat Butonuna Basıldı" (1) koşulu sağlandığında motoru çalıştıran mantık.

### Soru - Cevap
> **Soru:** 3 girişli ($A, B, C$) bir AND kapısının doğruluk tablosunda kaç durum olur? Çıkışın $1$ olduğu kaç durum vardır ve girişler ne olmalıdır?  
> **Öğrenci Cevabı:** $8$ farklı durum olur ve HIGH olan sadece $1$ durum olur, o da `1, 1, 1`.  
> **Değerlendirme:** Doğru ($2^3 = 8$).

---

## 2.2 OR (VEYA) Kapısı

- Mantıksal toplama işlemidir ($Y = A + B$).
- Girişlerden **en az biri HIGH ($1$)** olduğunda çıkış HIGH ($1$) olur.
- Çıkışın LOW ($0$) olabilmesi için tüm girişlerin aynı anda LOW ($0$) olması gerekir.

| Giriş A | Giriş B | Çıkış Y |
|:---:|:---:|:---:|
| 0 | 0 | **0** |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

- **Donanım Örneği:** Paralel bağlı anahtarlar; Yangın Sensörü ($1$) VEYA Manuel Yangın Butonu ($1$) tetiklendiğinde alarm sirenini ($1$) devreye sokan güvenlik devresi.

### Soru - Cevap
> **Soru:** 3 girişli bir OR kapısı için: 1) Çıkışın $0$ olduğu giriş kombinasyonu nedir? 2) Çıkışın $1$ olduğu toplam kaç durum vardır?  
> **Öğrenci Cevabı:** 1) `000`, 2) 7 durumun hepsi 1.  
> **Değerlendirme:** Doğru.

---

## 2.3 NOT (DEĞİL / Evirici) Kapısı ve Active-LOW Mantığı

- Tek girişli ve tek çıkışlıdır ($Y = \overline{A}$).
- Girişi tersler: $0 \rightarrow 1$, $1 \rightarrow 0$.

| Giriş A | Çıkış Y |
|:---:|:---:|
| 0 | **1** |
| 1 | **0** |

- **Gömülü Sistemlerde "Active-LOW" Mantığı:**
  Bir pinin üzerinde çizgi varsa ($\overline{\text{RESET}}$, $\overline{\text{ENABLE}}$, $\overline{\text{CS}}$) veya `_N` eki taşıyorsa bu sinyal **LOW ($0\text{V}$)** seviyesinde aktiftir.

### Soru - Cevap
> **Soru:** Datasheet'inde $\overline{\text{ENABLE}}$ (Active-LOW) pini bulunan bir motor sürücüyü çalıştırmak için MCU'dan bu pine ne gönderilmelidir? Eğer normal `ENABLE` (Active-HIGH) olsaydı ne gönderilirdi?  
> **Öğrenci Cevabı:** 1) Active-LOW olduğu için LOW gönderildiğinde çalışır. 2) Active-HIGH olsaydı HIGH olduğunda çalışırdı.  
> **Değerlendirme:** Doğru.

---

## 2.4 XOR (Özel VEYA / Exclusive-OR) Kapısı

- Girişler birbirinden **farklıysa $1$**, **aynıysa $0$** üretir ($Y = A \oplus B$).

| Giriş A | Giriş B | Çıkış Y |
|:---:|:---:|:---:|
| 0 | 0 | 0 |
| 0 | 1 | **1** |
| 1 | 0 | **1** |
| 1 | 1 | 0 |

- **Donanımsal Uygulamaları:**
  1. **Toplayıcı Devreler (Half Adder):** $1 + 1 = 10_2$ işleminde toplam biti doğrudan XOR kapısıyla elde edilir.
  2. **Parite ve Hata Denetimi (CRC):** UART vb. haberleşme hatlarında bit kontrolü için kullanılır.
  3. **Durum Değiştirme (NOT / Buffer Dönüşümü):** Bir girişi sabit tutulduğunda sinyali kontrol eder.

### Soru - Cevap
> **Soru:** 2 girişli bir XOR kapısının $B$ girişine sürekli HIGH ($1$) bağlanırsa:  
> 1) $A=0$ iken çıkış ne olur?  
> 2) $A=1$ iken çıkış ne olur?  
> 3) Bu kapı hangi temel kapı gibi davranmış olur?  
> **Öğrenci Yanıtı:** 1) 1 olur, 2) 0 olur, 3) AND gibi davranmış olur.  
> **Rehberlik / Hata Analizi:** 1 ve 2 doğruydu ($0 \rightarrow 1$ ve $1 \rightarrow 0$). Ancak giriş değerinin tam tersine döndüğü hatırlatıldı. 0'ı 1, 1'i 0 yapan kapı soruldu.  
> **Öğrenci Düzeltmesi:** **NOT kapısı gibi davranır.**  
> **Değerlendirme:** Doğru. (Bir bacağı $1$'e bağlanırsa NOT / Evirici; $0$'a bağlanırsa Buffer / Aynısını geçiren kapı olur).
