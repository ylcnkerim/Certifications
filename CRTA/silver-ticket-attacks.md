# Silver Ticket Attack

Silver Ticket Attack, bir saldırganın Kerberos kimlik doğrulama protokolündeki bir zayıflıktan yararlanarak, belirli bir hizmet için sahte bir Kerberos hizmet bileti (ST Service Ticket veya TGS Ticket) oluşturduğu bir siber saldırı tekniğidir

## Nasıl Çalışır?

**Gerekli Olan Bilgiler:** 

* Hedef hizmetin çalıştığı sunucunun FQDN'si
* Hedef hizmetin SPN'si
* Hizmetin çalıştığı etki alanı (domain) adı ve SID'si (Security Identifier)
* Hizmetin çalıştığı servis hesabının veya makine hesabının NT hash'i.

&nbsp;

## Adım 1 | NTLM Hash Çekmek

Bu adım için Domain Controller üzerinde yetkilendirilmiş bir kullanıcı olmamız gerekiyor. biz şuan "DS-01" kullanıcısındayız. Yani yetkiliyiz bir önceki derste parolasını bulduğumuz makine.

```
Invoke-Mimikatz -Command '"lsadump::dcsync /user:cyberwarfare\dc-01"' -verbose
```

Bu komut ile "dc-01" kullanıcısının parola hash'ini (NTLM hash'ini) bir Domain Controller'dan çekmeye çalışır.

## Adım 2 | Golden Ticket Oluşturmak

NTLM hash'ini ele geçirdiğinizi varsayarak, şimdi "Golden Ticket" (Altın Bilet) saldırısının nasıl gerçekleştirildiğini ve sonuçlarını inceleyebiliriz;

```
../Invoke-Mimikatz.ps1
Invoke-Mimikatz -Command '"kerberos::golden /user:administrator /domain:cyberwarfare.corp /sid:<SID> /target:DC-01.cyberwarfare.corp /service:cifs /rc4:<NTLM_hash> /id:500 /groups:512 /star:offset:0 /endin:600 /renewmax:10080 /ptt"' -verbose
```

Bu, Mimikatz'ın kerberos::golden komutunu çalıştırır. Bu komut, bir Golden Ticket oluşturmak için kullanılır.

* `/user:administrator` : Bu, sahte Golden Ticket'ın hangi kullanıcı adına ait olacağını belirtir. Saldırganın bu durumda administrator adlı, genellikle etki alanı yöneticisi olan bir kullanıcının kimliğine bürünmek istediğini gösterir. Bu kullanıcı gerçekte var olmak zorunda değildir.

* `/domain:cyberwarfare.corp` : Biletin hangi etki alanı için oluşturulacağını belirtir.

* `/sid:<domain>`: Etki alanının kendi SID'sidir, Etki alanının SID'si genellikle S-1-5-21-XXX-XXX-XXX formatındadır. Bu, Kerberos'un bileti doğrulaması için gereklidir.

* `/target:DC-01.cyberwarfare.corp` : Bu parametre, Golden Ticket'ın hangi Domain Controller tarafından verilmiş gibi görüneceğini belirtir. Saldırganın ağdaki bir DC'nin FQDN'sini girmesi gerekir.

* `/service:cifs` : Bu parametre, sahte TGT'nin (Golden Ticket) genellikle herhangi bir hizmete erişim sağlamak için kullanılacağını gösterir. Ancak cifs parametresi daha çok Silver Ticket bağlamında belirli bir hizmeti hedeflemek için kullanılır. Golden Ticket, temel olarak bir TGT olduğu için, herhangi bir hizmete erişmek için kullanılabilir ve genellikle spesifik bir /service parametresi gerektirmez. Bu parametrenin burada bulunması, komutun yanlış yapılandırıldığına veya bir "Silver Ticket" komutunun Golden Ticket senaryosunda yanlış kullanıldığına işaret edebilir. Bir Golden Ticket ile amaç, her türlü hizmete erişebilecek genel bir TGT oluşturmaktır.

* `/rc4:<NTLM_hash>` : Bu parametre, sahte bileti şifrelemek için kullanılacak olan krbtgt hesabının NTLM hash'ini (veya AES anahtarını) belirtir. Bu hash, lsadump::dcsync /user:krbtgt komutuyla elde edilen kritik bilgidir. Mimikatz, bu hash'i kullanarak Domain Controller'ın yapacağı gibi bir TGT'yi şifreler.

* `/id:500` : Bu parametre, oluşturulacak sahte biletin içine gömülecek olan kullanıcı ID'sini (RID - Relative ID) belirtir. 500 değeri, yerleşik yönetici (Administrator) hesabının RID'sidir. Bu, saldırganın bilet aracılığıyla yönetici yetkileri elde etmek istediğini gösterir.

* `/groups:512` : Bu parametre, biletin içine dahil edilecek güvenlik grubunun RID'sini belirtir. 512 değeri, Etki Alanı Yöneticileri (Domain Admins) grubunun RID'sidir. Bu, saldırganın Domain Admin yetkileriyle hareket etmek istediğini gösterir.

* `/startoffset:0` : Biletin geçerli olmaya başlayacağı sürenin başlangıç kayması (saat cinsinden). 0 genellikle anında geçerli olmasını ifade eder.

* `/endin:600` : Biletin ne kadar süre sonra sona ereceğini belirtir (dakika cinsinden). 600 dakika (10 saat) geçerli olacağını gösterir.

* `/renewmax`: 10080: Biletin maksimum yenilenebilir süresini belirtir (dakika cinsinden). 10080 dakika (7 gün) boyunca yenilenebilir olacağını gösterir. Bu, saldırganın uzun süreli kalıcılık elde etmesini sağlar.

* `/ptt`: Pass-the-Ticket anlamına gelir. Bu kritik parametre, oluşturulan sahte Golden Ticket'ın doğrudan mevcut PowerShell oturumunun Kerberos önbelleğine enjekte edilmesini sağlar. Bu sayede, saldırganın oluşturduğu bilet anında kullanılabilir hale gelir.

Artık administrator kullanıcısı olarak konsolu kullanabiliriz.

---

**NOT: Bir sonraki ders "persistence-exfiltrate.md" dosyasındadır ayrıca bu dersten önce "kerberoasting.md" dosyasına göz atın.**



