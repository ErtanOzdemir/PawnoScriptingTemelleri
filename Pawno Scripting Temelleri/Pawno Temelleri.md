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
 ```
 #include <a_samp>
 ```
Include İngilizce'de içermek, dahil etmek anlamına gelmektedir. Yazılımcılar yazdıkları kodların okunabilirliğini arttırmak ve yazılımların performanslarını arttırmak amacıyla tüm projeyi aynı dosyanın içerisine yazmazlar. Amaçlarına uygun olarak parçalara - dosyalara bölerler ve daha sonra ihtiyaç duyulması halinde istenilen dosyayı projelerine dahil ederek hem kodlarının okunabilirliğini hem de yazdıkları kodların performanslarını arttırmayı amaçlarlar. Yukarıdaki kodu incelediğimiz zaman `pawno/includes/a_samp.inc` adresinde bulunan `<a_samp>` dosyasını projemize dahil edilmiş olduğunu görürüz. Böylelikle `<a_samp>` dosyasının içerinde ki komutları projemizde çalıştırabileceğiz.

```
#include <core>
#include <float>
#include <string>
#include <file>
#include <time>
#include <datagram>
#include <a_players>
#include <a_vehicles>
#include <a_objects>
#include <a_sampdb>
```
Yukarıda görüdüğükleriniz projenize dahil edebileceğiniz diğer kütüphaneler.
 ### Calls
 Bir sonraki adımda iki tane fonksiyon görüyoruz. Programlama dillerinde fonksiyonlar, lisede anlatılan fonksiyonlar konusundan hiçbir farkı yoktur. Matematikte fonksiyonlar genellikle f(x) olarak ifade edilir. Örneğin f(x) = x + 1 fonksiyonun görevi, x yerine verilen değeri 1 ile toplamaktır. Pawno'daki print(string[]) ifadesinin görevi parantezler içerisine yazılan değeri ekrana bastırmaktır. Şimdilik `string[]` ifadesine takılmayın. İlerleyen bölümlerde ondan da bahsedeceğim.  

 Bir diğer fonksiyon ise `main()` fonksiyonudur. Bu fonksiyonun görevi ise içerisine yazılan kodları çalıştırmasıdır. Yani print fonksiyonu tek başına ekrana bir şey basmaz. Basmasını istiyorsak çalıştırmamız lazım.

 ````
 return 1;
 ````
Return komutu ne olduğunu anlatmak amacıyla main'e bir değer döndürür. 1 ifadesi işlemin başarılı olduğunu, 0 ifadesi ise başarısız olduğunu temsil eder. return komutu hakkında daha detaylı bilgi için [burayı]([https://link](https://medium.com/royto/return-komutu-ne-i%CC%87%C5%9Fe-yarar-83a90b9e475b)) okumanızı öneririm.

### Statements
<hr>
 Basit script'imizde return ve print fonksiyonunun noktali virgül ( ; ) ile bitirildiğini görüyoruz. Noktalı virgül statement'ın (ifadenin) bitirildiğini belirtmek için kullanılır. Yani print fonksiyonu yazdım ve işim bitti diyebilmek için kullanılır.

 ````
 main() { print("Hello World!"); return 1; }
 ````
Ayrıca yukarıda ki kodu incelediğimizde bir küme parantezlerini görüyoruz ({}). Küme parantezleri ise içerisine yazılan kodların bir arada çalıştırılmasını sağlar. Örneğin aşağıda ki kodda küme parantezlerini kullanmayalım.

````main() print("Hello World!"); return 1;````

Print fonksiyonundan sonra noktalı virgül kullanmamız ifadenin sonlandırıldığını belirtiyor bu yüzden print'ten sonra return'ün okunup okunmayacağı muğlak olur. Bu yüzden derleyici programını derlerken aynı karmaşıklığın içerisine kendi de düşeceği için hata verecektir. Eğer halen küme parantezi kullanmak istemiyorsanız bunu yapmanın bir yolu ifadenin print fonksiyonunda son bulmadığını belirtmektir. Peki bunu nasıl yapabilirsiniz?

````main() print("Hello World!"), return 1;````

Print'ten sonra virgül kullanmak daha ifadenin bitmediği devamının olduğunu belirtmek için kullanılır. Böylelik derleyici kodunuzu okurken print() fonksiyonun sonunda durmaz, virgülü görünce devamının olduğunu anlar ve okumaya devam eder.

## Fonksiyonlar
<hr>

Aslında fonksiyonun ne olduğunu yukarıda çokça bahsettim. print fonksiyonu buna örnekti. Kütüphanelerin içerisinde ki hazır fonksiyonları kullanabileceğiniz gibi aynı zaman kendi fonksiyonlarınızı tanımlamanızda mümkün. Aşağıda ki örneğe bakalım;

````
#include <a_samp>
 
main()
{
	return MyFunction();
}
 
MyFunction()
{
	print("Hello World!");
	return 1;
}
````

Main ifadesinin dışarısında `MyFunction` isimli bir fonksiyon görüyorsunuz. Bu fonksiyonu kendimiz main'in dışarısında tanımladık. Fonksiyon adını (MyFunction kısmını) istediğiniz gibi değiştirebilirsiniz.

Burada ki fonksiyonun temel görevini anlamak için küme parantezlerinin içine bakalım. Küme parantezinin içerisinde `print("Hello World")` yazıyor. Yani ekrana "Hello World" yazdırıyor. Fakat siz bu fonksiyonu main fonksiyonun içerisine yazmazsanız kodunu çalışmaz. Çünkü en başta da değindiğim gibi kodlarınızı çalıştırmak için main'in içerisine yazamalısınız.

İsterseniz MyFunction'ı return değeri olarak kullanmayabilirsiniz;
````#include <a_samp>
 
main()
{
	MyFunction();
	return 1;
}
 
MyFunction()
{
	print("Hello World!");
	return 1;
}
````

Bu şekilde de kodunuz düzgün bir şekilde çalışacaktır.

### Parametreler

Fonksiyonunuzun dışarıyla iletişime geçebilmesi için parametrelere ihtiyacı vardır. 

````
#include <a_samp>
 
main()
{
	return MyFunction("Hello World!");
}
 
MyFunction(string[])
{
	print(string);
	return 1;
}
````

MyFunction'ın parantezlerinin içerisinde görebildiğiniz üzere `string[]` isimli bir değişken var. Bu değişken aynı zamanda fonksiyonumuzun parametresidir. Buradaki parametre der ki; bu fonksiyon içerisine string türünde bir değişken değeri gerekli. Dolayısıyla siz buraya string türünde bir değişken değeri yazdığınız zaman, değişken adının yazdığı yere yani MyFunction'da ki print'in içerisine taşınır. Böylelikle main'in içerisinde ki `MyFunction("Hello Word!")` fonskiyonu ekrana `Hello World!` bastırır. Eğer string istenilen yere bir karakter değilde sayı (integer) değer yazdırsaydık hata alırdık. Yukarıda köşeli paranteze sahip olan ifadeyi ilerleyen bölümlerde anlatacağım.

## Değişkenler (Variables)
<hr>

Bir değişken basitçe hafızanın bir parçasıdır. Hafıza ise verilerin depolandığı, gerektiğinde okunup değiştirilebildiği yerdir. Değişkenler bir veya birden fazla hücreden oluşabilir. Bir hücre 32 bit'ten oluşur bu da 4 byte'a eşittir. Eğer bit ve byte'ın ne olduğunu bilmiyorsanız şöyle açıklayabiliriz. Bilgisayarlar 1 ve 0'lar ile çalışır. Biz buna binary (ikili) sayı sistemi diyoruz. Siz kodlarınızı yazıp derlediğiniz (compile) zaman kodlarınız, bilgisayarınızın anladığı dile yani binary'e dönüştürülür. Örneğin siz 123 sayısını bilgisayar üzerinde yazdığınız zaman, bilgisayarınız bu sayıyı binary yani (1111011) olarak görür. Her 1 ve 0 bir bit olarak sayılır. Yani 123 toplam 7 bitlik bir sayıdır. Eğer 8 bit olsaydı, o zaman 1 byte'lık bir sayı diyebilirdik. Günlük yaşantımızda ikilik sayı sistemi yerine 10'lu sayı sistemini kullandığımızı unutmayalım. Konumuzdan çok fazla uzaklaşmadan değişkenlere geri dönelim.

### Deklarasyon

Bir değişkeni yaratmak için onu deklare etmemiz gerekir.
````
new myVariable;

````

Bu sisteme myVariable isimli yeni bir değişken yarattığımızı haber verir ve ilk değerini atamazsanız otomatik olarak sizin yerinize 0 değeri atanır.

#### Değer Atama

Eğer değişken değerinin sıfır yerine farklı bir sayı olmasını istiyorsanız o halde böyle yapmanız gerekir;

````
new myVariable = 7;
````

Böylelikle myVariable isimli değişkenin değerini 7 yapmış olduk.

Bu değişkeni ekrana bastırmak için ne yapmalıyız?
````
new myVariable = 7; 
printf("Merhaba %d", myVariable);
````
printf() fonksiyonu kullandığımıza dikkat edin. print() fonksiyonuna göre farkı, string ifadelere müdahale edebilmemizdir. `%d` burada format tanımlayıcısıdır. Integerlar (tam sayılar) için %d kullanılır. Yani "Merhaba" yazısının yanına tamsayı bir değer geleceğini bildirir ve ekrana `Merhaba 7` yazdırır.

````
new myVariable = 7;
printf("%d", myVariable);
myVariable = 8;
printf("%d", myVariable);

````

Yukarıdaki kod, ilk olarak ekrana 7'yi bastırır. İlk printf() fonksiyonundan sonra myVariable değişkeninin değeri 8 olarak değiştirilmiş. Bu sebeple ikinci printf() ekrana 8'i bastırır.

Değişkenler üzerinde oynayabilirsiniz. Bunun örnekleri aşağıdadır.

````
myVariable = myVariable + 4;
````
Değişkenin değerini 4 arttırır.

````
myVariable += 4;
````
Bu da aynı şekilde değişkenin değerini 4 arttırır.

```
myVariable -= 4;
```
4 azaltır.

```
myVariable *= 4;
```
4 ile çarpar.
```
myVariable /= 4;
```
4'e böler.

#### Diziler (Arrays)

