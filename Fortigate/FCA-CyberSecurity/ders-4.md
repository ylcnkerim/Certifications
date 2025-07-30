# Fortigate Inspect SSL Traffic

## SSL Inspection Nedir?
- **SSL (Secure Sockets Layer)** ve **TLS (Transport Layer Security)**, internet trafiğini şifreleyerek güvenli hale getirir.  
- Ancak, şifreli trafik **güvenlik cihazları** tarafından doğrudan incelenemez.  
- **SSL Inspection**, FortiGate'in bu şifreli trafiği çözerek içerideki zararlı faaliyetleri tespit etmesine olanak tanır.

---

## Neden Önemli?
- Şifreli trafiğin artmasıyla birlikte saldırılar da bu trafiğin içine gizlenmeye başladı.  
- **Antivirüs**, **Web Filtering** ve **Intrusion Prevention** gibi güvenlik özellikleri, trafiği görebilmek için **SSL Inspection**'a ihtiyaç duyar.  

---

## SSL Inspection TÜrleri
1. **Full SSL Inspection**  
   - FortiGate, istemci ile sunucu arasındaki trafiği tamamen çözer.  
   - Yeni bir **CA sertifikası** yüklenmesi gerekir (istemcilerde güven uyarısı olmaması için).  
   - Daha kapsamlıdır ama **daha fazla işlem gücü** gerektirir.  

2. **SSL Certificate Inspection**  
   - Trafiğin sadece **sertifika bilgilerini** kontrol eder.  
   - Daha hızlıdır ve istemcilere ek sertifika yükleme gerektirmez.  
   - Ancak içerik taraması yapmaz.  

---

## Nasıl Çalışır?
1. **İstemci (Client)** HTTPS isteği gönderir.  
2. **FortiGate**, bu isteği yakalar ve kendisini "ara sunucu (proxy)" gibi konumlandırır.  
3. **FortiGate**, kendi sertifikasını kullanarak istemciye yanıt verir.  
4. Orijinal sunucuya da kendi bağlantısını kurar.  
5. Trafik çözüldükten sonra güvenlik kontrolleri yapılır.  

---

## Konfigürasyon (Basit)
1. **CA Sertifikasını FortiGate’e yükle** (ve istemcilere dağıt).  
2. **Policy & Objects** → **SSL/SSH Inspection** bölümünden profil oluştur.  
3. İlgili güvenlik politikasına bu SSL Inspection profilini uygula.  

---

## Pros & Cons
**Avantajlar:**  
- Şifreli trafiğin içeriği görülebilir.  
- Zararlı yazılımlar, oltalama siteleri ve diğer tehditler daha iyi tespit edilir.  

**Dezavantajlar:**  
- Performansa ek yük getirir.  
- Sertifika yönetimi karmaşık olabilir.  

---

## Hangisi Ne Zaman Kullanılmalı?
- **Full Inspection:**  
  - Güvenlik önceliği yüksekse, özellikle şirket içi cihazlarda.  
- **Certificate Inspection:**  
  - Performansın kritik olduğu ve içerik taramasına gerek olmayan durumlarda.
