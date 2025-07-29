# Privilege Escalation Via vsftpd Backdoor (FTP Exploitation)

## 1. FTP Üzerinden Sisteme Giriş Yapılmış
Sisteme FTP zafiyetlerini kullanarak hedef sunucuya backdoor veya reverse-shell vb. dosyaları yüklebilme veya bu tür işlemleri gerçekleştirdikten sonra. Bu yöntem, vsftpd 2.3.4 sürümünde bırakılmış bir arka kapı (backdoor) zafiyetinden faydalanıyor. 
Normalde FTP servisi, kullanıcı adı ve parola ile giriş yapılmasını bekler; ama bu sürümde geliştirici tarafından (bilerek ya da yanlışlıkla) bırakılmış özel bir arka kapı kodu var.

## 2. Metasploit Konsolunu Başlatma
Metasploit Framework’ü kullanarak zafiyeti sömürmek için terminalde şu komutu çalıştır:
`msfconsole`
Metasploit, bilinen exploit’leri barındıran güçlü bir sızma testi aracıdır.

## 3. Exploit Arama
Saldırgan, vsftpd için mevcut exploit’leri aramak amacıyla şu komutu çalıştırır:
`search vsftpd`
Bu komut, Metasploit içindeki vsftpd ile ilgili modülleri listeler.

## 4. Exploit Seçimi
Çıkan sonuçlar arasından backdoor içeren exploit’i seçer:
`use <exploit_ismi>`
Bu komutla saldırgan exploit modülünü aktif hâle getirir.

## 5. Hedef Sistemi Tanımlama
Ardından hedef sistemin IP adresini tanımlamak için:
`set RHOSTS <hedef_IP>`
komutunu çalıştırır.

## 6. Verbose Modu Açma
İşlem detaylarını görebilmek için:
`set verbose true`
komutunu kullanır.

## 7. Exploit’i Çalıştırma
Son olarak exploit’i çalıştırır:
`run`
Bu işlem sonucunda saldırgan, exploit başarılı olursa shell erişimi kazanır ve doğrudan root yetkileriyle sisteme giriş yapabilir.

# Bunun Mantığı
Bu saldırının mantığı aslında oldukça basit: Hedefte çalışan vsftpd 2.3.4 sürümünde bir arka kapı bulunuyor. Bu arka kapı, belirli şekilde bağlantı kurulduğunda saldırgana otomatik olarak root yetkisine sahip bir shell açıyor. 
Yani saldırgan klasik yöntemlerle kullanıcı adı/parola almak zorunda kalmadan doğrudan sistemin en yetkili kullanıcısı oluyor.
