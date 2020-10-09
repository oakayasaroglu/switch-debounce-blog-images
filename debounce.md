# Switch Bounce Nedir 

Bir switchin durumunu değiştirdiğiniz zaman meydana gelen mekanik titreme sonucunda anahtar birkaç defa açık ve kapalı pozisyonlar arasında gidip gelir. 

Bu olay mikrosaniyeler düzeyinde gerçekleşir ve __“Switch Bounce”__ olarak adlandırılır.

![Image 1](https://raw.githubusercontent.com/osmanalperenkayasaroglu/switch-debounce-blog-images/main/1.png "Image 1")

_Switch bounce osiloskopta bu şekilde görünür._

# Debounce Nedir 

Switch bounce olayı devrelerimizde kısa süreli noise etkisi oluşturduğu için mux, mcu vs. kullandığımız devrelerde problem oluşturması yüksek bir olasılıktır. Bounce etkisini önlemeye __“Debounce”__ denir. En yaygın 3 debounce uygulaması şunlardır:

1.	Entegre kullanmak
2.	RC Devresiyle Debouncing
3.	SR latch kullanarak debouncing
4.	Yazılım ile debouncing

## Entegre kullanmak

Bir debounce entegresini datasheetine uygun bir şekilde kendi devrenizde kullanabilirsiniz ancak maliyetli bir çözümdür ve yaygın bir yöntem değildir.

## RC Devresiyle Debouncing

Basit bir RC filtresiyle debounce yapmak **en yaygın** ve **en hesaplı** donanım çözümüdür. Tek yapmamız gereken bir *pull up/pull down direncine* gereken **low pass filtresini** modifiye etmektir. Bu filtrenin low pass filtresi olmasının sebebi *bounce olayının yüksek frekansta gerçekleşmesi* ve bu gürültüyü önlemek istememizdir. 

![Image 2](https://raw.githubusercontent.com/osmanalperenkayasaroglu/switch-debounce-blog-images/main/2.png "Image 2")

Şekildeki devre kurabileceğimiz **en hesaplı** debounce devresidir ancak **optimum yöntem değildir.** GPIOmuzun eşik değeri seviyesindeki herhangi bir bounce devrede kararsızlık oluşturabilir. Bu sebepten ötürü *RC değerini biraz daha yüksek tutmak* mantıklıdır. 

![Image 3](https://raw.githubusercontent.com/osmanalperenkayasaroglu/switch-debounce-blog-images/main/3.png "Image 3")

Şekildeki devre ise bir öncekinden daha pahalı ancak **daha garanti** bir devredir. 74HC14 çeviricisi *gecikme* sağladığı için gürültü işlemciye ulaşmaz. Bu devreyi kurduğumuz zaman herhangi bir yazılımsal destek olmadan positive logic çalışabilirsiniz.

Bu tip devrelerde **Schmitt-Trigger** çeviricileri diğer çeviricilere tercih etmeniz daha doğru bir seçim olur çünkü Schmitt-Trigger çeviriciler **histerezis**(gecikme) sağladığı için sinyal, bounce bittikten sonra devrenizin çıkışına iletilecektir.

![Image 4](https://raw.githubusercontent.com/osmanalperenkayasaroglu/switch-debounce-blog-images/main/4.png "Image 4")

Eğer kapasitörünüzün daha *hızlı şarj* olmasını istiyorsanız şekilde devrede görüldüğü üzere R2’yi bir diyotla *bypass* ederek bunu yapabilirsiniz.

## SR Latch Kullanarak Debouncing

Bu yöntem debounce yöntemleri içerisinde **en garantili** olanıdır. 

![Image 5](https://raw.githubusercontent.com/osmanalperenkayasaroglu/switch-debounce-blog-images/main/5.png "Image 5")

SR latch’in çalışma prensibine göre 2 girişin 0 olduğu durumda çıkışın en son durumunu kaydeder. SPDT (Single Pole Double Throw) anahtarlarda bounce durumunda ne 1 numaralı giriş (SW) ne de 3 numaralı giriş (SW) anahtarlandığı için *floating*(bouncing esnasında oluşan) durumunda SR Latch *hafıza durumunda* olacaktır. En son hangi girişe temas edildiyse o girişin sinyalini **stabil** bir şekilde tutar. 

SR Latch ile yapılan debouncing’in **dezavantajı** ise bu yöntemin *SPDT anahtarlara* uygulanabilmesi ve SPDT anahtarların **maliyetinin fazlalığıdır.**

## Yazılım ile Debouncing

Bu yöntemde en çok kullanılan 2 çeşit algoritma vardır:

1.	Sayma Algoritması
2.	Shift Register Yöntemi

Sayma algoritmasına göre işlemci sinyalin değiştiğini algıladıktan sonra belirli bir süre sayı saymaya başlar ve hedef sayıya ulaştığında gelen sinyal neyse onu kabul eder.

Shift register yönteminde ise sayma algoritmasını seçilen bir shift register üzerinden hexadecimal sayım yapılarak kullanılır.

İkisinin de mantığında bouncing süresinin bitmesini bekledikten sonra gelen sinyali kabul etmek vardır.

------------------------------------------------------------------------------------------------------------------------------------------------

Debouncing hakkındaki yazım bu kadardı. Güzel günleriniz olsun.



Bu Yazıyı Yazarken Kullandığım Kaynaklar:

https://www.pololu.com/docs/0J16/4

https://electrosome.com/switch-debouncing/ 

https://www.embedded.com/my-favorite-software-debouncers/
