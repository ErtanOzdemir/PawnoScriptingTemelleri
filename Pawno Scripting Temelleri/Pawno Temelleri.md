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

#### Include
 ```pwn
 #include <a_samp>
 ```
Include İngilizce'de içermek, dahil etmek anlamına gelmektedir. Yazılımcılar yazdıkları kodların okunabilirliğini arttırmak ve yazılımların performanslarını arttırmak amacıyla tüm projeyi aynı dosyanın içerisine yazmazlar. Amaçlarına uygun olarak parçalara - dosyalara bölerler ve daha sonra ihtiyaç duyulması halinde istenilen dosyayı projelerine dahil ederek hem kodlarının okunabilirliğini hem de yazdıkları kodların performanslarını arttırmayı amaçlarlar. Yukarıdaki kodu incelediğimiz zaman `pawno/includes/a_samp.inc` adresinde bulunan `<a_samp>` dosyasını projemize dahil edilmiş olduğunu görürüz. Böylelikle `<a_samp>` dosyasının içerinde ki komutları projemizde çalıştırabileceğiz.

```pwn
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

 ````pwn
 return 1;
 ````
Return komutu ne olduğunu anlatmak amacıyla main'e bir değer döndürür. 1 ifadesi işlemin başarılı olduğunu, 0 ifadesi ise başarısız olduğunu temsil eder. return komutu hakkında daha detaylı bilgi için [burayı]([https://link](https://medium.com/royto/return-komutu-ne-i%CC%87%C5%9Fe-yarar-83a90b9e475b)) okumanızı öneririm.

### Statements

 Basit script'imizde return ve print fonksiyonunun noktali virgül ( ; ) ile bitirildiğini görüyoruz. Noktalı virgül statement'ın (ifadenin) bitirildiğini belirtmek için kullanılır. Yani print fonksiyonu yazdım ve işim bitti diyebilmek için kullanılır.

 ````pwn
 main() { print("Hello World!"); return 1; }
 ````
Ayrıca yukarıda ki kodu incelediğimizde bir küme parantezlerini görüyoruz ({}). Küme parantezleri ise içerisine yazılan kodların bir arada çalıştırılmasını sağlar. Örneğin aşağıda ki kodda küme parantezlerini kullanmayalım.

````pwn 
main() print("Hello World!"); return 1;
````

Print fonksiyonundan sonra noktalı virgül kullanmamız ifadenin sonlandırıldığını belirtiyor bu yüzden print'ten sonra return'ün okunup okunmayacağı muğlak olur. Bu yüzden derleyici programını derlerken aynı karmaşıklığın içerisine kendi de düşeceği için hata verecektir. Eğer halen küme parantezi kullanmak istemiyorsanız bunu yapmanın bir yolu ifadenin print fonksiyonunda son bulmadığını belirtmektir. Peki bunu nasıl yapabilirsiniz?

````pwn
main() print("Hello World!"), return 1;
````

Print'ten sonra virgül kullanmak daha ifadenin bitmediği devamının olduğunu belirtmek için kullanılır. Böylelik derleyici kodunuzu okurken print() fonksiyonun sonunda durmaz, virgülü görünce devamının olduğunu anlar ve okumaya devam eder.

## Fonksiyonlar


Aslında fonksiyonun ne olduğunu yukarıda çokça bahsettim. print fonksiyonu buna örnekti. Kütüphanelerin içerisinde ki hazır fonksiyonları kullanabileceğiniz gibi aynı zaman kendi fonksiyonlarınızı tanımlamanızda mümkün. Aşağıda ki örneğe bakalım;

````pwn
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
````pwn
#include <a_samp>
 
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

````pwn
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


Bir değişken basitçe hafızanın bir parçasıdır. Hafıza ise verilerin depolandığı, gerektiğinde okunup değiştirilebildiği yerdir. Değişkenler bir veya birden fazla hücreden oluşabilir. Bir hücre 32 bit'ten oluşur bu da 4 byte'a eşittir. Eğer bit ve byte'ın ne olduğunu bilmiyorsanız şöyle açıklayabiliriz. Bilgisayarlar 1 ve 0'lar ile çalışır. Biz buna binary (ikili) sayı sistemi diyoruz. Siz kodlarınızı yazıp derlediğiniz (compile) zaman kodlarınız, bilgisayarınızın anladığı dile yani binary'e dönüştürülür. Örneğin siz 123 sayısını bilgisayar üzerinde yazdığınız zaman, bilgisayarınız bu sayıyı binary yani (1111011) olarak görür. Her 1 ve 0 bir bit olarak sayılır. Yani 123 toplam 7 bitlik bir sayıdır. Eğer 8 bit olsaydı, o zaman 1 byte'lık bir sayı diyebilirdik. Günlük yaşantımızda ikilik sayı sistemi yerine 10'lu sayı sistemini kullandığımızı unutmayalım. Konumuzdan çok fazla uzaklaşmadan değişkenlere geri dönelim.

### Deklarasyon

Bir değişkeni yaratmak için onu deklare etmemiz gerekir.
````pwn
new myVariable;

````

Bu sisteme myVariable isimli yeni bir değişken yarattığımızı haber verir ve ilk değerini atamazsanız otomatik olarak sizin yerinize 0 değeri atanır.

### Değer Atama

Eğer değişken değerinin sıfır yerine farklı bir sayı olmasını istiyorsanız o halde böyle yapmanız gerekir;

````pwn
new myVariable = 7;
````

Böylelikle myVariable isimli değişkenin değerini 7 yapmış olduk.

Bu değişkeni ekrana bastırmak için ne yapmalıyız?
````pwn
new myVariable = 7; 
printf("Merhaba %d", myVariable);
````
printf() fonksiyonu kullandığımıza dikkat edin. print() fonksiyonuna göre farkı, string ifadelere müdahale edebilmemizdir. `%d` burada format tanımlayıcısıdır. Integerlar (tam sayılar) için %d kullanılır. Yani "Merhaba" yazısının yanına tamsayı bir değer geleceğini bildirir ve ekrana `Merhaba 7` yazdırır.

````pwn
new myVariable = 7;
printf("%d", myVariable);
myVariable = 8;
printf("%d", myVariable);

````

Yukarıdaki kod, ilk olarak ekrana 7'yi bastırır. İlk printf() fonksiyonundan sonra myVariable değişkeninin değeri 8 olarak değiştirilmiş. Bu sebeple ikinci printf() ekrana 8'i bastırır.

Değişkenler üzerinde oynayabilirsiniz. Bunun örnekleri aşağıdadır.

````pwn
myVariable = myVariable + 4;
````
Değişkenin değerini 4 arttırır.

````pwn
myVariable += 4;
````
Bu da aynı şekilde değişkenin değerini 4 arttırır.

```pwn
myVariable -= 4;
```
4 azaltır.

```pwn
myVariable *= 4;
```
4 ile çarpar.
```pwn
myVariable /= 4;
```
4'e böler.

## Diziler (Arrays)

Diziler, birden fazla veriyi tek seferde saklayabileceğiniz ve dinamik olarak erişebileceğiniz değişkenlerdir. Bir dizi derleme zamanında (compile time) deklare edilir bu yüzden kaç tane veri saklayacağınızı daha önceden bilmeniz gerekir.

```pwn
new myArray[5];
```
Yukarıdaki kod sizin için 5 slot büyüklüğünde yer ayırır böylelikle tek seferde 5 tane veri saklamış olursunuz.

````pwn
new myVariable = 5,
myArray[myVariable];
````
Bu kod, myVariable'da depolanan sayı kadar büyüklükte bir dizi oluşturacak gibi görünüyor (burada 5, ancak herhangi bir şey olabilir), ancak bunu yapamazsınız. PAWN'de, kodunuzu derlediğinizde değişkenler için bellek atanır, bu, dizilerin her zaman tek boyutlu olduğu anlamına gelir, boyutu istediğiniz zaman istediğiniz
### Erişim Sağlamak
Yeni bir dizi oluşturduğunuz zaman, içerisinde bulunan veriler varsayılan olarak sıfırdır.

```pwn
new myArray[5];
```
```
0 0 0 0 0
```
Yukarıdaki sıfırlardan herhangi bir tanesini değiştirmek isterseniz `dizi_ismi[değiştimek istediğiniz elemanın sıra sayısı]` formatını kullanmanız gerekir. Örneğin;
```pwn
new myArray[5];
myArray[2] = 7;

```
Bu kod size 2. sırdaki sıfırın yeni değerini ayarlamanızı sağlar. Böylelikle dizi elemanları artık şu şekilde olur;
```
0 0 7 0 0
```
**Önemli Not:** Dizileri soldan saymaya 1'den değil, 0'dan başlamanız gerekiyor. Bu sebeple yedi sayısı 3. sırada gözükmesine rağmen aslında 2. sırada.

```
0. 1. 2. 3. 4. //Sıra sayısı
0  0  7  0  0
```
Eğer dizinin dahilinde olmayan örneğin 5 elemanlı bir dizide 6. elemanı değiştirmeye çalışırsanız hata alırsınız.


## Strings (Karakter Dizileri)
Yukarıda daha sonra anlatacağımı söylediğim önemli bir konudayız. Şu ana kadar değişkenlerimize genellikle tam sayılar atadık. Şimdi ise değişkenlerimize nasıl karakterler (a,b,c gibi) atayabiliriz onu inceleyeceğiz. Stringler, içerisinde birden çok karakter veya insanların okuyabileceği kelimeleri ve cümleleri barındırabilen bir dizi çeşididr. Karakterler hafızada 1 byte'lık yer kaplar. Bunu nasıl hesaplıyoruz? Örneğin "A" harfine, ASCII tablosu dediğimiz tabloda onluk tabana göre karşılık gelen sayı 65'tir. 65 sayısını 2'li sayı sistemine (binary) dönüştürdüğümüz zaman (1000001) 7 bitlik yer kaplar fakat şu an değinmeyeceğimiz başka bir sbepten dolayı başına bir sıfır daha eklenir (01000001). Karakter dizilerinde her bir harf dizideki bir hücreyi tutar. Birazdan örneklerle inceleyeceğiz.

PAWN "NULL terminated" (NULL: İngilizce'de boş gibi bir anlam taşımaktadır fakat kafanız karışmasın NULL'da aslında bir değerdir. NULL terminated - NULL ile sonlandırılmış  ) bir dildir. Yani bunun anlamı 0'a ulaşıldığında string ifadenin sonuna ulaşılmış olur. 20 hücreli bir dizide 19 karakter ve bir tane de NULL sonlandırma karakterine sahip olabilirsiniz. Aşağıda ki örneği inceleyelim.
```pwn
new myString[16] = "Hello World";
```
Yukarıda ki kod parçasında köşeli parantez içerisinde bulunan sayı yandaki tırnak içerisinde bulunan 15 harflik karakter dizisi için bellekte açılacak hücre sayısını belirtiyor. Tırnak işaretini kullanmamızın amacı atama operatörünün (=) sağında bulunan değerin string olduğunu bilgisayara anlatabilmek. Şimdi stringlerin dizilerle olan yakınlığını ortaya çıkaralım. Bellekte toplam 16 hücre açtık. Bu hücrelere tek bir harf atanır. Ayrıca boşlukta bir karakterdir. Dolayısıyla 16 hücrenin sadece 11 tanesi karakterlerle doldurulmuş olur. Ve tabiki dizinin bitirildiğini anlamak için 12. hücre de 0 (NULL) ile doldurularak bellekte toplam 16 hücreden 12'si kullanılmış olur. Geri kalan 4 hücrenin akıbeti önemli değildir. 

```
072 101 108 108 111 032 087 111 114 108 100 0 x x x x 
```
Yukarıda her bir hücreye denk gelen karakterlerin ASCII değerlerini, NULL terminated ifadesini ve bu ifadeden sonra önemli olmayan ayrılmış hücreleri (x) görüyorsunuz.

Stringleri, aynı diziler gibi manipüle edebilirsiniz

```pwn
new myString[16] = "Hello World!";
myString[1] = 97;
```
Yukarıdaki kod **myString** değişkeninin 1. harfini ASCII tablosunda 97'ye kaşılık gelen harf (a) ile değiştirir. Yani "Hello" değilde "H**a**llo" olur. Okunaklığı arttırmak amacıyla 97 yerine böyle de yapabiliriz;
```pwn
myString[1] = 'a';
```
"a" harfinin çevresinde ki tek tırnak işareti onu bir karakter olduğunu belirtir. Karakterlerin NULL terminated olması gerekmez. Bu yüzden sadece bir hücrelik yer kaplarlar.
```pwn
new myString[16] = "Hello World!";
myString[1] = '\0';
```
'\0' 2 karakterdir ancak \ karakteri bir sonraki karakteri etkileyen özel bir karaterdir. \0 NULL anlamına gelir. Aşağıdaki kod parçası da aynı şeyi yapar.
```pwn
new myString[16] = "Hello World!";
myString[1] = 0;

```
Yukarıdaki kodun artık birinci harfi null olduğu için sonraki karakterleri ekrana bastırmaz.
```
H
```
Fakat burada karıştırılmaması gereken önemli bir husus vardır;
```pwn
new myString[16] = "Hello World!";
myString[1] = '0';
```
Yukarıdaki sıfır tek tırnak içerisinde olduğu için bir NULL ifade değil, karakterdir ve kodun çıktısı aşağıdaki gibi olur.
```
h0llo
```
## Escape character