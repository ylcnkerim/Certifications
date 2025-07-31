# Kerberoasting

Kerberoasting, özellikle Active Directory ortamlarında kullanılan ve saldırganların servis hesaplarının düz metin parolalarını elde etmek için Kerberos kimlik doğrulama protokolünün belirli bir zayıflığından faydalandığı bir saldırı tekniğidir.

## Mantığı

Önce kontrol ettiğimiz Domain User makinasından SPN'leri olan hesaplar aranır ardından bu hesaplardan bulduğu her bir SPN için bir Servis Biletleri (Ticket Granting Service - TGS) bileti talep eder. Saldırgan, elde ettiği bu şifrelenmiş TGS biletlerini ağ dışına, kendi kontrolündeki bir bilgisayara çıkarır. Bu biletler, ilgili servis hesabının parola karması (hash) ile şifrelendiğinden, saldırgan bu karmayı kırmak için çevrimdışı parola kırma araçları (örneğin John the Ripper, Hashcat) kullanabilir. Parola başarıyla kırıldığında, saldırgan artık servis hesabının düz metin parolasına sahiptir. Bu servis hesapları genellikle ağ üzerinde yetki yükseltebilir.
&nbsp;

---
&nbsp; 

## Adım | 1: SPN Numaralandırma

Bu adımda saldırgan, ele geçirdiği alan kullanıcısı hesabını kullanarak Active Directory'de SPN'leri olan servis hesaplarını arar.

```
../PowerView_dev.ps1
Get-NetUser -SPN -Verbose
```

Komutu, PowerShell'in Active Directory (AD) ortamlarında kullanıcı hesaplarını sorgulamak için kullanılan bir parçasıdır.

```
setspn -T <domain_ismi> -Q */*
```

Bu komut ise Windows'un yerleşik setspn.exe yardımcı programıdır ve Active Directory'deki domainin SPN'leri sorgulamak veya yönetmek için kullanılır. Bulunan SPN: "HTTP/portal.cyberwarfare.corp" dir.

## Adım | 2: TGS Talep Etmek

```
Add-Type -AssemblyNmae System.IndentityModel
New-Object System.IndentityModel.Tokens.KerberRequestorSecurityToken -ArgumentList "HTTP/portal.cyberwarfare.corp
```

* `Add-Type` : Bu PowerShell cmdlet'i, belirtilen .NET derlemesini (assembly) veya C# kaynak kodunu mevcut PowerShell oturumunuza dinamik olarak yükler.

* `AssemblyName System.IdentityModel` : Bu kısım, yüklenmek istenen .NET derlemesinin adıdır. System.IdentityModel derlemesi, .NET uygulamalarının kimlik yönetimi, güvenlik token'ları ve Kerberos gibi kimlik doğrulama protokolleriyle etkileşim kurması için gerekli sınıfları içerir.

* `New-Object` : Bu cmdlet, belirtilen .NET sınıfından yeni bir nesne (instance) oluşturur.

* `System.IdentityModel.Tokens.KerberosRequestorSecurityToken` : Bu, System.IdentityModel derlemesinde bulunan ve belirli bir Hizmet Asıl Adı (SPN) için bir Kerberos hizmet bileti talep etmek için kullanılan bir .NET sınıfıdır. Bu sınıf, Kerberos protokolünün alt seviye ayrıntılarını soyutlar ve belirli bir hizmet için bir bilet alınmasını kolaylaştırır.

* `ArgumentList "HTTP/portal.cyberwarfare.corp"` : Bu parametre, KerberosRequestorSecurityToken sınıfının yapıcı metoduna iletilen argümanları belirtir. Buradaki "HTTP/portal.cyberwarfare.corp" ifadesi, talep edilmek istenen servis biletinin SPN'sidir.

Bu komutlar ile sistemin kerberos belleğine bu TGS'leri işledik.

```
klist
```

Komutu ile kullanıcının sistemdeki Kerberos biletlerini (tickets) görüntülemek ve yönetmek için kullanılır.

## Adım | 3: Biletleri Kopyalama

Önceki adımlarda, `Add-Type` ve `New-Object` ile belirli bir SPN için Kerberos servis bileti talep ettik ve bu bilet yerel sistemin Kerberos önbelleğine kaydedildi. Şimdi sıra bu şifrelenmiş bileti sistemden alıp çevrimdışı kırılabilir bir formata dönüştürmekte. İşte bu noktada Mimikatz devreye girer.

```
../Invoke-Mimikatz.ps1
Invoke-Mimikatz -Command '"kerberos::list /export"' -verbose
```

Komutları, etkin Mimikatz işlevselliğini kullanarak sistemin Kerberos önbelleğini tarar. Burada bulunan tüm Kerberos biletleri (özellikle daha önce New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken ile talep ettiğimiz şifrelenmiş servis bileti dahil), diskte belirlenen bir konuma (genellikle Invoke-Mimikatz betiğinin çalıştığı dizine veya geçici dizine) .kirbi uzantılı dosyalar olarak dışa aktarılır. `ls` komutu ile bu dosyaları görüntüleyebiliriz.

## Adım | 4: Hash Brute Force

Bu ".kirbi" dosyalarını asıl saldırgan cihazı (kali linux) a kopyaladıktan sonra;

```
python tgsrepcrack.py 10k-worst-pass.txt example.kirbi
```

* `python`: Bu, Python yorumlayıcısını çağırır. Yani, ardından gelen dosyanın bir Python betiği olduğunu ve Python ortamında çalıştırılması gerektiğini belirtir.

* `tgsrepcrack.py`: Bu, Kerberos TGS (Ticket Granting Service) biletlerini kırmak için özel olarak yazılmış bir Python betiğidir. Bu betik, genellikle Impacket gibi popüler siber güvenlik araç kitlerinin bir parçası olarak veya bağımsız olarak bulunur. Ana görevi, ele geçirilen .kirbi dosyasının içeriğini okumak, şifrelenmiş kısmı çıkarmak ve sağlanan parola listesine karşı denemeler yaparak parolanın doğru olup olmadığını kontrol etmektir.

* `10k-worst-pass.txt`: Bu, betiğin parola kırma işlemi sırasında deneyeceği sözlük dosyasıdır (wordlist). "10k-worst-pass.txt" gibi bir isim, genellikle en yaygın kullanılan veya kolay tahmin edilebilen 10.000 (veya başka bir sayı) parolayı içeren bir listeye işaret eder. Saldırganlar, bu tür sözlükleri kullanarak ilk ve en hızlı parola kırma denemelerini yaparlar, çünkü birçok servis hesabı maalesef zayıf veya yaygın parolalar kullanır.

* `example.kirbi`: Bu, önceki adımda Mimikatz (kerberos::list /export) kullanılarak kurban sistemden dışa aktarılan şifrelenmiş Kerberos servis biletini içeren dosyanın adıdır. Bu dosya, servis hesabının parolasıyla şifrelenmiş bir veri bloğu içerir.

Artık şifreleri görmüş oluyoruz.

---

**NOT: Bir sonraki ders **silver-ticket-attacks.md** dosyasıdır. Bundan önce ise **lateral-movement.md** dosyasını inceleyiniz.

