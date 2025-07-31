# Credential Dumping

Credential Dumping, saldırganların bir sistemde saklanan kimlik bilgilerini (kullanıcı adları, parolalar, hash’ler, Kerberos biletleri vb.) ele geçirmek için kullandığı bir saldırı tekniğidir.

## MimiKatz

Mimikatz, windows’ta parolaları, hash’leri, Kerberos ticket’larını vb. bellekten çıkaran (credential dumping) ünlü bir post-exploitation aracıdır. Domain server'da yetki yükselttikten sonra artık bilgileri dump etmeye başlayabiliriz. 

### Adım 1 | Mimikatz Giriş

```
../Invoke-Mimikatz.ps1
```

Bu komut, "bir üst klasördeki Invoke-Mimikatz.ps1 dosyasını çalıştır” anlamındadır. Antivirüslerden kaçmak için bu yol izlenir ve Mimikatz scriptlerini çalıştırma imkanı sağlar.

### Adım 2 | Oturum Bilgilerini Çekmek

```
Invoke-Mimikatz -DumpCreds -Verbose
```

Komutu, **PowerShell** üzerinden **Mimikatz** kullanarak sistemdeki oturum bilgilerini (parola, hash, Kerberos ticket) ayrıntılı şekilde görüntülemek için çalıştırılır.

### Adım 3 | Hash İle Manipülasyon

Azönceki çektiğimiz oturum bilgilerinde parolalar direkt olarak değilde hash olarak verilir. Bu hash'leri çözmek yerine direkt bu hashleri kullanarak sistemde bu hash sahibi olan kullanıcı gibi davranabiliriz.

```
Invoke-Mimikatz -Command '"sekurlsa::pth /user:user /domain:cyberwarfare.corp /rc4 <hash> /run:powershell.exe"'
```

* `Invoke-Mimikatz` → PowerShell içinden Mimikatz fonksiyonunu çalıştırır.

* `sekurlsa::pth` → Pass-the-Hash modülü. Parolayı bilmeden sadece NTLM hash ile oturum açma.

* `/user:user` → Taklit edilecek kullanıcının adı (user).

* `/domain:cyberwarfare.corp` → Kullanıcının bağlı olduğu domain.

* `/rc4 <hash>` → Kullanıcının NTLM hash’i (RC4 burada NTLM hash türünü ifade ediyor).

* `/run:powershell.exe` → Bu hash ile açılacak yeni işlem. Yani bu hash kimliğiyle yeni bir PowerShell oturumu başlatır.

### Adım 4 | Lateral Movement

```
cd C:\Users\employee\Downloads
..\Find-WMILocalAdminAccess.ps1
Find-WMILocalAdminAccess -Verbose
```

Bu komutlar, elde edilen kimlik bilgileriyle ağda **lateral movement (yana doğru hareket)** için keşif yapıyor.Geçerli oturumdaki kullanıcı (bu durumda Pass-the-Hash ile taklit edilen "user") hangi bilgisayarlara WMI üzerinden Local Admin olarak erişebilir, bunu tespit eder.

---

**NOT: Bu dersin ardından lateral-movement.md dosyasını okumalısınız**
