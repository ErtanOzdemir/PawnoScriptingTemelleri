# Pawno Scripting

## Önsöz
 Pawno, günümüzde halen Half-Life motorunda ve GTA SanAndreas Multiplayer oyununda kullanılan gömülü script dilidir. C dili ile programlandığı için syntax'i (söz dizimi) C programlama diline çok yakındır. Eğer daha önce C dili ile ilgilendiyseniz Pawno'yu kullanmaya çok kısa sürede alışacaksınız. Pawno dilinin çok yaygın olmaması sebebiyle küçük bir geliştirici topluluğuna sahiptir. Hazırladığım rehber SA-MP için eklenti yazmak isteyenlere daha uygun **pawno dili** ile ilgili giriş seviyesi bilgiler içermektedir. Bu rehberde yer alan kodlar şimdi kapanmış olan https://sa-mp.wiki/ adresinden alınarak hazırlanmıştır.

## Giriş
Aşağıda ki kod muhtemelen yazabileceğiniz en temel pawno scriptidir.
```pwn
#include <a_samp>
 
main()
{
	print("Hello World!");
	return 1;
}
```
Çok basit olmasına rağmen bu kodu biraz daha yakından inceleyelim.
<hr>
#### Include
 ````
 #include <a_samp>

Include İngilizce'de içermek, dahil etmek anlamına gelmektedir. Yazılımcılar yazdıkları kodların okunabilirliğini arttırmak ve yazılımların performanslarını arttırmak amacıyla tüm projeyi aynı dosyanın içerisine yazmazlar. Amaçlarına uygun olarak parçalara - dosyalara bölerler ve daha sonra ihtiyaç duyulması halinde istenilen dosyayı projelerine dahil ederek hem kodlarının okunabilirliğini hem de yazdıkları kodların performanslarını arttırmayı amaçlarlar. Yukarıdaki kodu incelediğimiz zaman `pawno/includes/a_samp.inc` adresinde bulunan dosyayı projemize dahil etmiş olduğunu görürüz. 