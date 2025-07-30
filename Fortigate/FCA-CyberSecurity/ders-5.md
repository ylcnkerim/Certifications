# Fortigate Blocking Malware

## Malware Nedir?
- **Malware (Kötü Amaçlı Yazılım)**, bilgisayarlara ve ağlara zarar vermek için tasarlanmış yazılımlardır.  
- Türleri:  
  - **Virüsler**  
  - **Trojan (Truva Atı)**  
  - **Ransomware**  
  - **Spyware**  
  - **Worms (Solucanlar)**  

---

## Malware Blocking Neden Önemli?
- Zararlı yazılımlar **veri hırsızlığı**, **sistem çökmesi**, **şifreleme (ransomware)** gibi ciddi tehditler oluşturur.  
- **Erken tespit** ve **otomatik engelleme** ağı korur.  
- FortiGate, farklı güvenlik servisleriyle bu tehditleri proaktif olarak durdurur.  

---

## FortiGate Kötü Amaçlı Yazılımları Nasıl Engeller?
FortiGate, zararlı yazılımları engellemek için birden fazla güvenlik katmanını birlikte kullanır:

1. **Antivirus (AV) Scanning**  
   - Dosya ve trafiği gerçek zamanlı tarar.  
   - Şüpheli dosyalar için **karantina** uygular.  

2. **Web Filtering**  
   - Zararlı veya bilinen kötü amaçlı web sitelerine erişimi engeller.  

3. **IPS (Intrusion Prevention System)**  
   - Saldırı imzalarını tespit ederek kötü amaçlı trafiği durdurur.  

4. **FortiSandbox (Opsiyonel)**  
   - Şüpheli dosyaları izole bir ortamda çalıştırarak davranışlarını analiz eder.  

---

## Konfigürasyon (Temel)
1. **Security Profiles** → **Antivirus** bölümünden yeni bir profil oluştur.  
2. **Scan Options** kısmında:  
   - **HTTP, HTTPS, FTP, IMAP, POP3, SMTP** trafiği için taramayı aktif et.  
3. **Web Filter** profilinde:  
   - **Malicious Websites** kategorisini engelle.  
4. **Policy & Objects** → ilgili güvenlik politikasına bu profilleri uygula.  

---

## Pros & Cons
**Avantajlar:**  
- Çok katmanlı koruma sağlar.  
- Güncel tehdit veritabanı ile sürekli savunma.  

**Dezavantajlar:**  
- Çok yoğun trafikte performans etkisi olabilir.  
- SSL trafiğini taramak için ek sertifika yönetimi gerekir.

---
