---
# PIVOTING (SIÇRAMA)

Pivoting, bir saldırganın ele geçirdiği bir sistem üzerinden iç ağın diğer sistemlerine erişim sağlamak için kullandığı tekniktir. Bu aksiyon için genelde 

* Proxy
* SSH Tunneling
* VPN
* Port Forwarding

gibi yöntemlerle yapılır.

## SSH Tunelling İle Pivoting

SSH tunneling, bir SSH bağlantısı üzerinden şifreli bir “tünel” açarak başka veri trafiğini bu güvenli kanal içinden geçirme yöntemidir.

### * Adım | 1
İlk adım olarak kurban cihazın ssh servisine erişebildiğimizi varsayıyorum. Ve bu servise bağlantı gerçekleştiriyoruz.

`ssh msfadmin@192.168.50.3`

Ardından şifreyi girerek kurban cihazın ssh servisine erişmiş oluyoruz.

### * Adım | 2
Kendi cihazımızda bir SOCKS Proxy açmamız gerekiyor.

`ssh -D 8090 msfadmin@192.168.50.3`

Böylelikle trafik SSH tüneli içinden geçerek pivot makineden (msfadmin) çıkıyor ve iç ağdaki diğer hedeflere gidiyor. Bu yaptığımız işlemin çalışıp çalışmadığını kontrol etmek için **netstat -ant | grep 8090** komutunu kullanabiliriz.

### * Adım | 3
Açtığımız bu SOCKS proxy eğer başarılıysa artık **proxychains** gibi araçlarla kurban makinada komutlar çalıştırabiliriz.

`proxychains nc -nv 10.10.10.5 80`

Bu komut ile hedef sistem üzerinde **nc -nv 10.10.10.5 80** yani netcat komutu kullanarak kurban cihaz aracılığı ile iç ağdaki 10.10.10.5:80 portuna bağlanmış olduk. Artık iç ağ ile ilgili hangi işlemi yaparsak yapalım **proxychains** uygulamasını kullanmak zorundayız.

### Adım | 4
Artık kurban sistem ile **proxychains** kullanarak SSH tunellemesi yaptık. Örnek olarak iç ağda `10.10.10.4` cihazına bir rdesktop bağlantısı yollayalım.

`proxychains rdesktop 10.10.10.4`

Eğer bağlantı kabul edilirse iç ağdaki bir cihaza uzaktan erişmiş oluruz.
