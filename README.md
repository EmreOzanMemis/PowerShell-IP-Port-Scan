# PowerShell IP Port Scan
```
$port = (enter port value)
$network = “enter network value”
$range = 1..254
$ErrorActionPreference= ‘silentlycontinue’
$(Foreach ($add in $range)
{ $ip = “{0}.{1}” –F $network,$add
Write-Progress “Scanning Network” $ip -PercentComplete (($add/$range.Count)*100)
If(Test-Connection –BufferSize 32 –Count 1 –quiet –ComputerName $ip)
{ $socket = new-object System.Net.Sockets.TcpClient($ip, $port)
If($socket.Connected) { “$ip port $port open”
$socket.Close() }
else { “$ip port $port not open ” }
}
}) | Out-File C:\reports\portscan.csv
```
Bu PowerShell scripti, belirlenen bir ağdaki IP adreslerinin belirli bir portunun açık olup olmadığını kontrol eder ve sonuçları bir CSV dosyasına kaydeder. Ağ güvenliği ve yönetimi açısından önemli bir araç olan bu script, özellikle port taraması yaparak hangi servislerin ve portların aktif olduğunu belirlemek için kullanılır. İşte scriptin detaylı bir açıklaması ve kullanım amacı:

Scriptin Bileşenleri
Değişken Tanımlamaları:
```
$port = (enter port value)
$network = “enter network value”
$range = 1..254
$ErrorActionPreference= ‘silentlycontinue’
```
  $port: Taranacak port numarası.
  $network: Tarama yapılacak ağın başlangıç adresi (Örneğin, "192.168.1").
  $range: Tarama yapılacak IP adresi aralığı (1'den 254'e).
  $ErrorActionPreference: Hata durumunda scriptin nasıl davranacağını belirler. 'silentlycontinue' seçeneği, hataları göz ardı edip scriptin çalışmaya devam etmesini sağlar.

Ağ Tarama Döngüsü:
```
Foreach ($add in $range)
{ $ip = “{0}.{1}” –F $network,$add
```
Bu döngü, belirtilen ağdaki her IP adresi için çalışır. IP adresleri, $network ve $add değişkenlerinin kombinasyonuyla dinamik olarak oluşturulur.

İlerleme Göstergesi:
```
Write-Progress “Scanning Network” $ip -PercentComplete (($add/$range.Count)*100)
```
Write-Progress: Scriptin ilerlemesini gösteren bir ilerleme çubuğu gösterir. Bu, uzun süren taramalar için kullanıcıya sürecin ne kadar tamamlandığını gösterir.

Bağlantı Testi ve Port Kontrolü:
```
If(Test-Connection –BufferSize 32 –Count 1 –quiet –ComputerName $ip)
{ $socket = new-object System.Net.Sockets.TcpClient($ip, $port)
```
  Test-Connection: Belirtilen IP adresine ping atar. Eğer cevap alınırsa (yani cihaz ağda aktifse), belirtilen portta bir TCP bağlantısı kurmaya çalışır.
  System.Net.Sockets.TcpClient: TCP bağlantısı için bir nesne oluşturur ve belirtilen port üzerinden bağlantı kurmaya çalışır.

Sonuçların Kaydedilmesi:
```
If($socket.Connected) { “$ip port $port open” }
else { “$ip port $port not open ” }
$socket.Close()
}) | Out-File C:\reports\portscan.csv
```
  Bağlantı başarılı ise IP adresi ve port bilgisi "açık" olarak, değilse "açık değil" olarak kaydedilir.
  Sonuçlar, bir CSV dosyasına yazılır. Bu dosya, tarama sonuçlarını incelemek için kullanılır.

Neden ve Ne Amaçla Kullanılır?

  Güvenlik Denetimi: Ağdaki açık portları tespit ederek potansiyel güvenlik açıklarını belirler.
  Ağ Yönetimi: Hangi servislerin hangi portlarda çalıştığını ve ağdaki hangi cihazların aktif olduğunu gösterir.
  Uyumluluk ve Denetim: Ağın uyumluluk ve güvenlik politikalarına uygunluğunu denetler.

Bu script, sistem ve ağ yöneticileri tarafından, özellikle kurumsal ve geniş ağ yapılarına sahip ortamlarda, ağın güvenliğini ve performansını artırmak amacıyla düzenli olarak kullanılır.



  
