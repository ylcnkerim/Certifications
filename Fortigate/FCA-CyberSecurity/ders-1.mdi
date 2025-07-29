# Configuring Interfaces and Routing

## 1. Interfaces
- FortiGate’in ağdaki bağlantı noktalarıdır.
- Her interface şu bileşenlerden oluşur:
    - **Name (İsim):** Örn. wan1, lan, dmz
    - **IP Address (IP Adresi):** Interface’e atanır (örn. 192.168.1.1)
    - **Role (Rol):** LAN, WAN, DMZ gibi
    - **Administrative Access:** HTTPS, SSH, PING vb. erişim izinleri

### Örnek:
- **wan1:** 192.168.1.1 (Internet/WAN tarafı)
- **lan:** 10.0.0.1 (İç ağ tarafı)

---

## 2. Routing
- Routing, FortiGate’in paketleri doğru yöne göndermesini sağlar.
- **Default Route (Varsayılan Rota):**
    - Hedef: 0.0.0.0/0
    - Gateway: ISP yönlendiricisi (ör. 192.168.1.254)
    - Interface: WAN
- **Static Routes:**
    - Belirli bir ağ için manuel yönlendirme eklenir (ör. 10.10.0.0/24 → DMZ).

---

## 3. Best Practices
- Interface isimlerini anlamlı ver (örn. “OfficeLAN”, “ISP-WAN”).
- Yönetim erişimlerini (HTTPS, SSH) yalnızca güvenli ağlardan aç.
- Default route’u eklemeyi unutma, aksi halde FortiGate internete çıkamaz.
- Gereksiz açık portları kapat (örn. Telnet).

---

## 4. Mini Lab (Uygulama)
- **Adım 1:** wan1 interface’ine ISP’den aldığı IP’yi ata (örn. 192.168.1.1/24).
- **Adım 2:** lan interface’ine iç ağ IP’sini ata (örn. 10.0.0.1/24).
- **Adım 3:** Default route ekle (0.0.0.0/0 → 192.168.1.254 via wan1).
- **Adım 4:** PC’den FortiGate’e ping atarak bağlantıyı test et.
