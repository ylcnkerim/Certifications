# Configuring the FortiGate Intrusion Prevention System

FortiGate güvenlik duvarlarındaki **Saldırı Önleme Sistemi (Intrusion Prevention System - IPS)**, ağınızı kötü niyetli saldırılardan korumak için tasarlanmış güçlü bir özelliktir. IPS, bilinen saldırı imzalarını (patterns) tanıyarak veya anormal davranışları tespit ederek saldırıları **daha ağınıza ulaşmadan engeller**.

---

## IPS Nedir ve Neden Önemlidir?

IPS, ağ trafiğini sürekli olarak analiz eden ve potansiyel tehditleri tespit ettiğinde hemen harekete geçen bir güvenlik katmanıdır. Güvenlik duvarları (firewall) trafiği port ve protokol bazında filtrelerken, IPS içeriğe daha derinlemesine bakar.

* **Saldırı Algılama:** Bilinen saldırı imzaları, protokol anormallikleri veya davranışsal anormallikler gibi yöntemlerle saldırıları tespit eder.
* **Saldırı Önleme:** Tespit ettiği saldırıları engeller, oturumu sıfırlar veya zararlı trafiği bırakır. Bu sayede saldırının hedefe ulaşmasını engeller.

**Neden Önemlidir?**

* **Zero-Day Saldırılara Karşı Koruma:** Kısmen de olsa, henüz imzası olmayan yeni saldırı türlerine karşı davranışsal analizle koruma sağlayabilir.
* **Bilinen Zafiyetlere Karşı Koruma:** Uygulamalardaki veya işletim sistemlerindeki bilinen zafiyetleri hedef alan saldırıları durdurur.
* **Ağ Stabilitesi:** DDoS gibi ağ hizmetini kesintiye uğratmayı amaçlayan saldırıları önleyerek ağınızın çalışır durumda kalmasını sağlar.

---

## FortiGate'de IPS Nasıl Çalışır?

FortiGate üzerindeki IPS, FortiGuard Labs tarafından sürekli güncellenen geniş bir saldırı imzası veri tabanını kullanır.

1.  **Trafik Akışı:** Ağ trafiği FortiGate üzerinden geçerken, IPS motoru trafiği detaylıca inceler.
2.  **İmza Eşleştirme:** Gelen trafik, FortiGuard tarafından sağlanan binlerce IPS imzasıyla karşılaştırılır. Bu imzalar belirli saldırı kalıplarını, kötü amaçlı kodları veya şüpheli protokol davranışlarını temsil eder.
3.  **Anormal Davranış Tespiti (Heuristics):** Bazı durumlarda, imza olmasa bile trafiğin normalden sapıp sapmadığına dair davranışsal analizler yapılır.
4.  **Aksiyon Alma:** Bir saldırı tespit edildiğinde, IPS profilinizde tanımlı olan aksiyonu gerçekleştirir:
    * **Block (Engelle):** Saldırgan trafiği tamamen engeller.
    * **Reset (Sıfırla):** Bağlantıyı sıfırlar.
    * **Monitor (İzle):** Saldırıyı kaydeder ancak engellemez (daha çok test aşamasında kullanılır).

---

## Basit IPS Yapılandırması

FortiGate'de IPS'i yapılandırmak oldukça basittir:

1.  **IPS Profili Oluşturma/Düzenleme:**
    * FortiGate arayüzünde **Security Profiles > Intrusion Prevention** bölümüne gidin.
    * Mevcut bir profili düzenleyebilir veya **Create New** (Yeni Oluştur) diyerek kendi profilinizi oluşturabilirsiniz.
    * Burada, FortiGuard kategorilerine göre hangi saldırı tiplerine karşı nasıl bir aksiyon alınacağını (blok, izle vb.) belirlersiniz. Örneğin, "Botnet.C2" (Botnet Komuta Kontrol) kategorisindeki tüm imzaları engellemek isteyebilirsiniz.
    * Daha spesifik olmak isterseniz, belirli **IPS Signatures** (IPS İmzaları) arayabilir ve aksiyonlarını tek tek yapılandırabilirsiniz. Örneğin, belirli bir CVE açığını hedefleyen saldırıyı engellemek isteyebilirsiniz.

2.  **Güvenlik Politikasına Atama:**
    * Oluşturduğunuz veya düzenlediğiniz IPS profilini, korumak istediğiniz trafik akışını temsil eden **Policy & Objects > Firewall Policy** altındaki güvenlik politikalarınıza atamanız gerekir.
    * İlgili güvenlik politikasını düzenleyin ve **Security Profiles** bölümünde oluşturduğunuz **Intrusion Prevention** profilini etkinleştirin.

---

IPS, FortiGate'inizin güvenlik yeteneklerini önemli ölçüde artıran kritik bir bileşendir. Doğru yapılandırma ile ağınızı birçok yaygın ve karmaşık saldırıdan koruyabilirsiniz.
