# Fortinet Control Web Access Using Web Filtering

Fortinet güvenlik duvarları (FortiGate), **Web Filtreleme** özelliği sayesinde kullanıcıların internet üzerindeki web sitelerine erişimini yönetmenizi sağlar. Bu özellik, zararlı siteleri engellemek, üretkenliği artırmak ve bant genişliğini verimli kullanmak için kritik öneme sahiptir.

---

## Web Filtreleme Nedir?

Web filtreleme, internet trafiğini analiz ederek belirli web sitelerinin veya site kategorilerinin (örneğin sosyal medya, kumar, yetişkin içerik) erişimini **engelleme**, **uyarma** veya **izin verme** işlemidir. FortiGate cihazları bu filtrelemeyi aşağıdaki yollarla yapar:

* **Kategori Bazlı Filtreleme:** FortiGuard web filtreleme servisleri, milyonlarca web sitesini kategorilere ayırır. Siz de bu kategorilere göre politika belirleyebilirsiniz. Örneğin, "Sosyal Medya" kategorisindeki tüm siteleri engelleyebilirsiniz.
* **URL Bazlı Filtreleme:** Belirli bir URL'yi (örneğin, `www.kotusite.com`) doğrudan engelleyebilir veya izin verebilirsiniz.
* **Puanlama (Rating) Bazlı Filtreleme:** Bazı siteler FortiGuard tarafından risk puanlarına göre sınıflandırılır. Bu puanlara göre erişim kuralları belirleyebilirsiniz.

---

## Neden Web Filtreleme Kullanılır?

* **Güvenlik:** Kötü amaçlı yazılım (malware), kimlik avı (phishing) siteleri ve diğer siber tehditler içeren sitelere erişimi engelleyerek ağınızı korur.
* **Üretkenlik:** Çalışanların iş saatleri içinde sosyal medya, oyun siteleri veya diğer dikkat dağıtıcı sitelere erişimini kısıtlayarak üretkenliği artırır.
* **Yasal Uyumluluk:** Belirli içeriklerin erişimini kısıtlayarak yasal düzenlemelere veya şirket politikalarına uyum sağlamanıza yardımcı olur.
* **Bant Genişliği Yönetimi:** Eğlence veya gereksiz içerikli sitelere erişimi sınırlayarak ağdaki bant genişliğinin daha verimli kullanılmasını sağlar.

---

## Nasıl Çalışır?

FortiGate cihazınız, ağınızdan geçen tüm web trafiğini FortiGuard'ın bulut tabanlı veri tabanına başvurarak inceler.

1.  Bir kullanıcı bir web sitesine erişmeye çalıştığında, FortiGate isteği yakalar.
2.  İlgili web sitesinin kategorisi, URL'si ve puanı hakkında bilgi almak için FortiGuard'a sorgu gönderir.
3.  FortiGate, sizin tanımladığınız web filtreleme politikalarına göre bu bilgileri değerlendirir.
4.  Politikaya uygun olarak erişime **izin verir**, **engeller** veya kullanıcıya bir **uyarı** gösterir.

---

## Basit Bir Örnek: Sosyal Medya Erişimi

Diyelim ki şirketinizde iş saatleri içinde sosyal medya kullanımını kısıtlamak istiyorsunuz:

1.  FortiGate arayüzünde **Security Profiles > Web Filter** bölümüne gidersiniz.
2.  Yeni bir web filtre profili oluşturursunuz (örneğin, "Ofis Web Filtresi").
3.  **FortiGuard Category Based Filter** altında "Social Networking" (Sosyal Ağlar) kategorisini bulursunuz.
4.  Bu kategori için aksiyonu **Block (Engelle)** olarak ayarlarsınız.
5.  Son olarak, bu web filtre profilini ilgili güvenlik politikanıza (Security Policy) atarsınız.

Artık, bu politikadan geçen tüm kullanıcılar FortiGuard tarafından "Sosyal Ağlar" kategorisinde listelenen sitelere erişemeyecektir.

---

Fortinet Web Filtreleme, ağ güvenliğinizi ve verimliliğinizi artırmak için güçlü ve esnek bir araçtır.
