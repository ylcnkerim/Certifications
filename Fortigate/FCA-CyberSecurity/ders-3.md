# Authenticating Network Users

## 1. Kullanıcı Doğrulama (Authentication) Nedir?
- Ağ kaynaklarına erişmeden önce kullanıcının kimliğinin doğrulanması sürecidir.
- Amaç: Yetkisiz erişimleri engellemek ve güvenliği artırmak.

---

## 2. FortiGate’te Authentication Yöntemleri
- **Local User Database:** FortiGate üzerinde oluşturulan kullanıcılar.
- **Remote Authentication Servers:**  
  - **RADIUS**  
  - **LDAP / Active Directory**  
  - **TACACS+**  
- **Single Sign-On (SSO):** Kullanıcının bir kez doğrulanması sonrası otomatik erişim.

---

## 3. Authentication Tipleri
- **Firewall Policy Authentication:** Kullanıcıların belirli firewall politikaları üzerinden erişim için doğrulanması.
- **SSL VPN Authentication:** VPN bağlantısı sırasında kullanıcı doğrulaması.
- **Two-Factor Authentication (2FA):** Kullanıcı adı + şifre + ek doğrulama (SMS, token).

---

## 4. Authentication Süreci
1. Kullanıcı erişim talebinde bulunur.  
2. FortiGate veya ilgili sunucu kullanıcı kimlik bilgilerini doğrular.  
3. Doğrulama başarılı ise kullanıcıya erişim izni verilir.  

---

## 5. Örnek: LDAP ile Authentication
- FortiGate, Active Directory’ye bağlanır.  
- Kullanıcılar, AD’deki kullanıcı bilgileriyle doğrulanır.  
- Merkezi kullanıcı yönetimi sağlar.

---

## 6. Önemli
- Mümkünse **Remote Authentication** kullan (merkezi yönetim için).  
- **2FA** ile güvenliği artır.  
- Kullanıcı rolleri ve izinlerini net tanımla.  
- Authentication loglarını düzenli kontrol et.

---

## 7. Mini Lab (Uygulama) -- Reponun İçine Görselleri Koyacağım
1. FortiGate üzerinde local user oluştur.  
2. Basit bir firewall policy’ye user authentication ekle.  
3. LDAP sunucusu kurulumu ve FortiGate’e bağlanma.  
4. Test PC ile kullanıcı girişi yaparak doğrulamayı test et. 
