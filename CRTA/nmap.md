# Nmap: Ağ Keşfi ve Güvenlik Tarayıcısı

Nmap, ağ keşfi ve güvenlik denetimleri için kullanılan ücretsiz ve açık kaynaklı bir yardımcı programdır. Sistem yöneticileri ve güvenlik uzmanları tarafından ağdaki cihazları, açık bağlantı noktalarını, çalışan servisleri, işletim sistemi türlerini ve potansiyel güvenlik açıklarını tespit etmek için yaygın olarak kullanılır. Nmap, esnekliği ve geniş özellik yelpazesi sayesinde ağ güvenliği alanında vazgeçilmez bir araç haline gelmiştir.

---

## Nmap Kullanımı

Nmap'in temel kullanım mantığı oldukça basittir: `nmap [seçenekler] [hedef]`. Hedef, bir IP adresi, bir ana bilgisayar adı, bir IP aralığı veya bir alt ağ olabilir.

**Örnekler:**

* Tek bir IP adresini taramak: `nmap 192.168.1.1`
* Bir ana bilgisayar adını taramak: `nmap example.com`
* Bir IP aralığını taramak: `nmap 192.168.1.1-100`
* Bir alt ağı taramak: `nmap 192.168.1.0/24`

---

## Temel Nmap Parametreleri

Bu bölümde, Nmap'in en sık kullanılan ve genel olarak bilinmesi gereken 10 parametresini bulacaksınız.

* **`-sS`:** En popüler ve varsayılan tarama türüdür. Hedef sistemde TCP 3 yönlü el sıkışmasını tamamlamadan bağlantıyı kapatır, bu da onu daha az fark edilebilir kılar.
* **`-sT`:** Tam TCP bağlantısı kurarak portları tarar. Daha yavaş ve daha kolay tespit edilebilir olabilir ancak bazı durumlarda gereklidir.
* **`-sU`:** UDP portlarını tarar. UDP portları genellikle durum bilgisi sağlamadığından bu tarama yavaş olabilir.
* **`-p <port aralığı>`:** Taranacak belirli portları veya port aralığını belirtir. Örneğin, `-p 80,443` sadece 80 ve 443 numaralı portları tarar, `-p 1-1024` ise 1'den 1024'e kadar olan portları tarar.
* **`-A`:** İşletim sistemi tespiti (`-O`), versiyon tespiti (`-sV`), script taraması (`-sC`) ve traceroute (`--traceroute`) gibi özellikleri birleştirir. Kapsamlı bilgi sağlar ancak daha gürültülüdür.
* **`-O` :** Hedef sistemin işletim sistemi türünü tahmin etmeye çalışır.
* **`-sV`:** Açık portlarda çalışan servislerin versiyon bilgilerini belirlemeye çalışır.
* **`-Pn`:** Hedef ana bilgisayarın aktif olup olmadığını belirlemek için ping göndermez. Güvenlik duvarları tarafından ping engelleniyorsa kullanışlıdır.
* **`-oN <dosya_adı>`:** Tarama sonuçlarını normal formatta belirtilen dosyaya kaydeder.
* **`-oX <dosya_adı>`:** Tarama sonuçlarını XML formatında belirtilen dosyaya kaydeder. Diğer araçlarla entegrasyon için kullanışlıdır.

---

## Güvenlik Sistemlerinden Kaçınma Parametreleri

Güvenlik duvarları, saldırı tespit sistemleri (IDS) ve saldırı önleme sistemleri (IPS) gibi güvenlik mekanizmaları Nmap taramalarını tespit edip engelleyebilir. Aşağıdaki parametreler, bu sistemlerden kaçınmak veya taramayı daha az fark edilebilir hale getirmek için kullanılır.

* **`-f` (Fragment Packets - Paketleri Parçalama):** TCP başlığını küçük parçalara ayırır. Bu, bazı güvenlik duvarlarının veya IDS'lerin paketleri doğru şekilde yeniden birleştirmesini zorlaştırabilir ve taramayı gizleyebilir.
* **`--data-length <sayı>` (Rastgele Veri Ekleme):** Gönderilen paketlere belirtilen miktarda rastgele veri ekler. Bu, bazı imza tabanlı IDS sistemlerini yanıltabilir.
* **`--badsum` (Sahte Kontrol Toplamları):** Paketlerin TCP/UDP/IP kontrol toplamlarını geçersiz olarak ayarlar. Bazı güvenlik duvarları bozuk kontrol toplamlarına sahip paketleri bırakırken, diğerleri bunları işleyebilir ve bu, taramayı tespit etmeyi zorlaştırabilir.
* **`--scan-delay <zaman>` (Tarama Gecikmesi):** Her port taraması arasına belirtilen sürede (saniye, ms) gecikme ekler. Bu, güvenlik sistemlerinin anormal tarama trafiğini tespit etmesini zorlaştırabilir. Örneğin, `--scan-delay 5s` her tarama arasında 5 saniye bekler.
* **`--spoof-mac <MAC adresi/rastgele/üretici>` (MAC Adresi Sahtekarlığı):** Gönderilen paketlerin MAC adresini değiştirir. Bu, yerel ağdaki güvenlik sistemlerinin taramanın kaynağını izlemesini zorlaştırabilir. Örneğin, `--spoof-mac 00:11:22:33:44:55` veya `--spoof-mac random`.

---

## Güvenlik Sistemlerine Yakalanması Neredeyse İmkansız Nmap Komutu


nmap -sS -Pn -p 1-65535 --scan-delay 10s --badsum --data-length 100 --spoof-mac random -f --source-port 53 --max-retries 0 --host-timeout 300s -T paranoid <hedef_IP>

## Nmap Scripting Engine (NSE) ile Komut Dosyası Çalıştırma

Nmap, yalnızca port taraması ve işletim sistemi tespiti gibi temel işlevlerle sınırlı değildir. **Nmap Scripting Engine (NSE)** sayesinde, Nmap'in yeteneklerini büyük ölçüde genişleten çeşitli komut dosyalarını (script'leri) çalıştırabilirsiniz. Bu komut dosyaları, zafiyet tespiti, kötü amaçlı yazılım analizi, servis keşfi ve daha birçok gelişmiş ağ keşfi görevi için kullanılabilir.

---

## NSE Nedir ve Neden Önemlidir?

NSE, Nmap'e entegre edilmiş güçlü bir komut dosyası çalıştırma motorudur.
* **Gelişmiş Bilgi Toplama:** Standart Nmap taramalarının ötesinde, hedef sistemler hakkında çok daha detaylı bilgiler edinmenizi sağlar.
* **Zafiyet Tespiti:** Bilinen zafiyetleri tarayarak potansiyel güvenlik açıklarını tespit edebilir.
* **Servis Enumarasyonu:** Çalışan servisler hakkında spesifik bilgilere (örneğin, HTTP başlıkları, SMB paylaşımları) ulaşabilir.
* **Güvenlik Denetimleri:** Ağ cihazlarının güvenlik yapılandırmalarını kontrol edebilir.
* **Otomasyon:** Tekrarlayan görevleri otomatikleştirebilir ve karmaşık analizleri hızlandırabilir.

---

## Nmap Komut Dosyalarını Çalıştırma

Nmap ile komut dosyalarını çalıştırmak için temel olarak **`-sC`** veya **`--script`** parametresi kullanılır.

* **`-sC` (Varsayılan Script'leri Çalıştırma):** Bu parametre, Nmap'in en sık kullanılan ve genel amaçlı olan varsayılan komut dosyalarını çalıştırır.

* **`--script <script_adı_veya_kategori>` (Belirli Script'leri Çalıştırma):** Bu parametre ile belirli bir komut dosyasını, komut dosyası kategorisini veya birden fazla komut dosyasını çalıştırabilirsiniz.
    ```bash
    nmap --script http-title <hedef_IP>

    nmap --script vuln <hedef_IP>

    nmap --script http-enum,smb-enum-shares <hedef_IP>

    nmap --script "http-*" <hedef_IP>

---
## Belirli Bir Servis Ve Sürümü İçin Nmap Scriptlerini Çalıştırma.

Diyelim ki bir hedef sistemde Apache 2.x.x web sunucusu çalıştığını varsayalım ve bu sunucuyla ilgili Nmap Scripting Engine (NSE) komut dosyalarını çalıştırmak istiyoruz.
Nmap'in yüklü olduğu sistemde, Apache ile ilgili komut dosyalarını bulmak için genellikle `/usr/share/nmap/scripts/` dizinine bakmamız gerekir.

Apache ile ilgili scriptleri bulmak için `grep` komutunu kullanabiliriz.
##` Nmap script lerinin bulunduğu dizine git
`cd /usr/share/nmap/scripts/`

## "apache" kelimesini içeren tüm scriptleri listeleyin ve bir dosyaya kaydedin
`grep -l apache * > apache_scripts.txt`

## "http-apache" ile başlayan scriptleri bulun
`grep -l "http-apache" * >> apache_scripts.txt`

## Metin belgesini çalıştır.
`nmap -p 80,443 --script $(cat apache_scripts.txt) <hedef_IP>`
