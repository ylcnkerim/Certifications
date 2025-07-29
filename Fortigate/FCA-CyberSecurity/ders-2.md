# Firewall Policies

## 1. Firewall Policy Nedir?
- FortiGate üzerinde trafiğin **nasıl yönetileceğini** belirleyen kurallardır.
- Temel işlevi: **Kaynak (Source) → Hedef (Destination) → Servis (Service)** kombinasyonuna göre trafiğe **izin vermek** veya **engellemek**.
- Politikalar **sıralı** çalışır: En üstteki kural önce işlenir (Top-Down).

---

## 2. Firewall Policy Temel Bileşenleri
- **Name:** Politikaya anlamlı bir isim verin.
- **Incoming Interface:** Trafiğin geldiği arayüz.
- **Outgoing Interface:** Trafiğin gideceği arayüz.
- **Source:** Trafiğin geldiği adres veya adres grubu.
- **Destination:** Trafiğin hedef adresi.
- **Service:** İzin verilen protokol veya port.
- **Action:** Trafiğin sonucunu belirler.
- **NAT:** Giden trafiğin adres çevirme ayarı.
- **Security Profiles:** IPS, Web Filter, Antivirus, Application Control gibi ek güvenlik katmanları.

---

## 3. Politikaların Çalsışma Mantığı
- **Top-Down**: FortiGate kuralları yukarıdan aşağıya doğru okur.
- **First Match**: İlk eşleşen kural uygulanır, diğerleri kontrol edilmez.
- **Implicit Deny**: Hiçbir kurala uymayan trafik varsayılan olarak reddedilir.

---

## 4. Örnek Senaryo
**Amaç:** LAN’daki cihazların internete çıkabilmesi.  

- **Incoming Interface:** LAN  
- **Outgoing Interface:** WAN  
- **Source:** 10.0.0.0/24  
- **Destination:** All  
- **Service:** ALL  
- **Action:** Allow  
- **NAT:** Enabled  

> Bu kural ile LAN’daki cihazlar NAT kullanarak WAN üzerinden internete erişebilir.

---

## 5. Önemli
- Politikaları **önem sırasına** göre yerleştir (en kısıtlayıcı üstte, en genel altta).
- **Açıklayıcı isimler** kullan.
- Gereksiz **“Any-Any-Allow”** kurallarından kaçın.
- **Loglama** aktif ederek trafik kaydı tut.
- Gerektiğinde **Security Profiles** ekle (IPS, AV, Web Filter).

---
