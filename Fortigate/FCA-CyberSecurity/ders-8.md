# Configuring the Fortinet Security Fabric

Fortinet **Güvenlik Yapısı (Security Fabric)**, Fortinet ürünlerinin birbiriyle entegre ve koordineli bir şekilde çalışmasını sağlayan bir güvenlik mimarisidir. Bu sayede ağınızdaki tüm güvenlik cihazları (FortiGate, FortiAP, FortiSwitch, FortiClient vb.) birbiriyle iletişim kurarak daha güçlü, daha görünür ve otomatize edilmiş bir savunma hattı oluşturur. Tek tek çalışan cihazlar yerine, bütünsel bir güvenlik ekosistemi yaratılır.

---

## Güvenlik Yapısı Nedir ve Neden Önemlidir?

Geleneksel güvenlik yaklaşımlarında, her güvenlik ürünü bağımsız olarak çalışır ve kendi loglarını, uyarılarını üretir. Bu durum, tehdit algılama ve müdahale süreçlerini karmaşıklaştırır ve geciktirir. Güvenlik Yapısı ise bu dağınıklığı ortadan kaldırır.

* **Entegrasyon:** Farklı Fortinet ürünleri (güvenlik duvarı, kablosuz erişim noktası, anahtar, uç nokta koruma) sorunsuz bir şekilde entegre olur.
* **Otomasyon:** Tehdit algılandığında, entegre ürünler arasında otomatik tepkiler tetiklenebilir. Örneğin, bir uç noktada (FortiClient) kötü amaçlı yazılım tespit edildiğinde, FortiGate otomatik olarak o cihazın ağ erişimini kısıtlayabilir.
* **Merkezi Görünürlük:** Tüm güvenlik ürünlerinden gelen veriler (loglar, tehdit bilgileri) tek bir merkezden (genellikle FortiGate veya FortiManager) izlenebilir. Bu, ağınızdaki güvenlik durumunun kapsamlı bir resmini sunar.
* **Basitleştirilmiş Yönetim:** Birden çok cihazı tek bir konsoldan yönetmek, karmaşıklığı azaltır ve yönetim yükünü hafifletir.
* **Hızlı Tehdit Yanıtı:** Tehdit istihbaratını anında paylaşarak, ağın bir noktasında tespit edilen bir tehdidin diğer noktalara yayılması engellenir.

---

## Güvenlik Yapısının Temel Bileşenleri

* **FortiGate:** Genellikle Güvenlik Yapısının kalbi ve merkezi yönetim noktasıdır. Diğer cihazlarla bağlantıyı kurar ve güvenlik politikalarını uygular.
* **FortiAnalyzer:** Fortinet ürünlerinden gelen tüm logları toplar, analiz eder ve raporlar. Güvenlik Yapısı içindeki görünürlüğü artırır.
* **FortiManager:** Büyük veya karmaşık ağlarda birden çok FortiGate cihazının ve diğer ürünlerin merkezi olarak yönetilmesini sağlar.
* **FortiSwitch:** Güvenli, yönetilebilir ağ anahtarlarıdır. FortiGate ile entegre olarak ağ segmentasyonu ve erişim kontrolü sağlar.
* **FortiAP:** Güvenli kablosuz erişim noktalarıdır. FortiGate üzerinden merkezi olarak yönetilir ve kablosuz ağ güvenliğini sağlar.
* **FortiClient:** Son kullanıcı cihazlarında (bilgisayarlar, mobil cihazlar) çalışan uç nokta güvenlik yazılımıdır. Tehdit tespiti yapar ve FortiGate ile entegre çalışır.
* **FortiSandbox:** Şüpheli dosyaları izole edilmiş bir ortamda (sandbox) analiz ederek bilinmeyen tehditleri (zero-day attacks) tespit eder.

---

## Güvenlik Yapısı Nasıl Yapılandırılır?

Güvenlik Yapısı'nı yapılandırmak genellikle aşağıdaki adımları içerir:

1.  **Fabric Root Seçimi:** Genellikle ağınızdaki ana FortiGate cihazını **Fabric Root** (Yapı Kökü) olarak belirlersiniz. Bu FortiGate, diğer Fortinet cihazlarının bağlanacağı merkez olacaktır.
    * FortiGate arayüzünde **Security Fabric > Fabric Connectors** bölümüne gidin.
    * **Fabric Root**'u buradan etkinleştirin.

2.  **Cihazların Fabriğe Katılması (Connecting Devices):**
    * **FortiSwitch/FortiAP:** FortiGate'e fiziksel olarak bağlandıklarında genellikle otomatik olarak FortiGate tarafından keşfedilir ve yönetilebilir hale gelirler. FortiGate'in **WiFi & Switch Controller** bölümünden bunları yetkilendirip yapılandırabilirsiniz.
    * **FortiClient:** FortiClient yazılımını son kullanıcı cihazlarına kurduktan sonra, FortiClient'ı FortiGate'e bağlamak için FortiGate üzerinde bir **FortiClient EMS (Endpoint Management System)** bağlantısı yapılandırılır. Bu, uç nokta cihazlarından güvenlik bilgilerinin FortiGate'e akmasını sağlar.
    * **FortiAnalyzer/FortiManager:** Bu cihazları Güvenlik Yapısına bağlamak için FortiGate üzerinde **Security Fabric > Fabric Connectors** altında ilgili Connector'ları (FortiAnalyzer/FortiManager) yapılandırırsınız. Bu, logların ve merkezi yönetimin sağlanmasını sağlar.
    * **Diğer FortiGate'ler:** Eğer birden fazla FortiGate'iniz varsa, bunları bir **Fabric Member** (Yapı Üyesi) olarak Fabric Root'a ekleyebilirsiniz. Bu, güvenlik politikalarının merkezi olarak senkronize edilmesine yardımcı olur.

3.  **Otomasyon ve Görünürlük Ayarları:**
    * Cihazlar Fabriğe katıldıktan sonra, **Security Fabric > Automation** bölümünden otomasyon kuralları oluşturabilirsiniz. Örneğin: "Eğer FortiClient bir kötü amaçlı yazılım tespit ederse, FortiGate üzerinden o kullanıcının internet erişimini kısıtla."
    * **Security Fabric > Physical Topology / Logical Topology** ekranlarından tüm bağlı cihazları ve aralarındaki bağlantıları görsel olarak izleyebilirsiniz. **Security Fabric > Audit** bölümünden ise yapılandırma ve uyumluluk denetimlerini yapabilirsiniz.

---

Fortinet Güvenlik Yapısı, modern siber güvenlik tehditlerine karşı kapsamlı, entegre ve otomatize edilmiş bir savunma hattı oluşturmak için vazgeçilmez bir çözümdür.
