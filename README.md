# aerokou
This repo is a general repo for aerokou's software development purposes.


Otonom uçuş ve görüntü işleme
Otonom uçuş
Yarışma kuralları gereği otonom uçuş sağlanmalıdır. Peki otonom uçuş nedir? 
Otonom uçuş, aktif olarak bir insanın hava aracını yönlendirmemesi demektir. Bu yönlendirme işlemi direk uçuş kontrolcüsü üzerinden, yardımcı bilgisayarlarla veya yer istasyonu üzerinden otonom bir şekilde yapılabilir. (GUIDED MODE)

Bu üç farklı yöntemin kendine göre avantajları ve dezavantajları vardır.

Uçuş kontrolcüsü üzerinden
Zaten herkesin alışık olduğu bu yöntem şu şekilde işler. Daha önceden koordinatlar, irtifa, hız ve ivme bilgileri gibi uçuş için gerekli olan bilgiler hava aracı otonom uçuş kartına yüklenir. Bu bilgiler ışığında hava aracının daha önceden verilen bu rotayı takip etmesi umut edilir. 

Yardımcı bilgisayarlar yardımı ile
yardımcı bilgisayarlar ile otonom uçuş yapmak aslında daha mantıklı olacaktır çünkü hali hazırda piyasada bulunan otonom uçuş kontrolcülerinde kamera vb üçüncü parti donanım desteği bulunmamaktadır. Bu sistemde işleyiş şöyle olacaktır. Otonom uçuş kontrolcüsü sadece hava aracının temel işlevlerini yapmasını sağlayacak, yardımcı bilgisayar ise bu uçuş kartına ne yapması gerektiğini söyleyecektir. Örneğin uçuş kartı sadece hareketlerin doğru yapıldığından emin olurken sağa sola gitmesi veya alçalıp yükselmesi gerektiği bilgisi yardımcı bilgisayar tarafından sağlanacaktır. Bu yardımcı bilgisayarlara ekstra donanımlar ekleyerek bu otonom uçuş faaliyeti daha verimli hale getirilebilir.

Yer istasyonu üzerinden 
Bu yöntem pek alışılageldik bir yöntem olmamakla beraber, en büyük avantajlarından birisi de hesaplama yapılan bilgisayarın bir yer istasyonu olmasından kaynaklı olarak, hesaplamaların çok daha hızlı yapılması demektir. Ve bu hesaplamalar yapıldıktan sonra bir API veya BOT tarzı bir yazılım ile, telemetri üzerinden hava aracına yapması gereken komutlar gönderilir. Bu yöntemin belkide pek tercih edilmeme sebeplerinden birisi de telemetri verisinin kopması durumunda ciddi sıkıntılar oluşabileceğidir. Telemetri bağlantısı kopmasa dahi, telemetri sinyalindaki kirlilik ve gecikme uçuş verimliliğini kötü etkileyecektir.

Görüntü işleme ve bu veriye göre hava aracını kontrol etme.
Bu işlemin gerçekleşmesi için standart otonom uçuş yöntemi yeterli olmayacaktır. Çünkü direkt olarak piyasadaki otonom uçuş kontrolcülerine kamera bağlanamaması bu durumda büyük bir önem arz etmektedir. Bunu için en çok kullanılan yöntem olan yardımcı bilgisayar ile destekli otonom uçuş tercih edilecektir. Genellikle otonom uçuş kartı olarak Pixhawk yardımcı bilgisayar olarak da Raspberry Pi veya Jetson tarzı yardımcı bilgisayarlar kullanılır. 

Tabiki her şeyin bir bedeli vardır ve bu tarz elektronik donanımlarda genelde bu bedel paradır :)

Şimdiye kadar AeroKou bünyesinde yapılan çalışmalar Raspberry Pi etrafında yoğunlaşmıştır. Fakat bu pek verimli olmamıştır. Kaliteli bir operasyon için, iyi bir yardımcı bilgisayar ve iyi bir kamera olmazsa olmazdır. 

Görüntü işleme yazılımları ile hava aracı arasında mavlink protokolü ile bağlantı sağlanabilir, doğrudan bu protokol üzerinden ardupilot yazılımına komutlar gönderilebilir. Bu komutları harici kütüphaneler yardımı ile (Örn: dronekit) de gönderebilirsiniz. Uygulamalar geliştikçe ve daha kompleks bir hal almaya başladıkça, bu tarz üçüncü parti yazılımlar ve kütüphaneler kullanmak, geliştiriciyi kısıtlayacağı için sonradan bunları terk etmek gerekebilir. Fakat başlangıç ve test amaçları için bu yeterli olacaktır. 

İyi bir görüntü işleme yazılımı
İyi bir görüntü işleme yazılımı için, özellikle bir hava aracını gerçek zamanlı kontrol edecek olan bir görüntü işleme yazılımı için, her parçanın puzzle a tam oturması gerekmektedir. Yani eğer parçalardan birisinde bir sorun varsa diğer parçalar da tam oturmayacaktır.
Biraz daha açıklamam gerekirse;
İyi bir yazılım dili, iyi bir kamera, güçlü bir makine, iyi bir haberleşme sistemi olduğu takdirde gerisi algoritmaların sihrine kalmış olacaktır. 

yazılım dili tercihi olarak mümkün mertebe makine diline yakın bir dil seçilmesi daha iyi sonuçlar almaya yarayacaktır. Fakat burada dikkat edilmesi gereken nokta iyi bir yazılım dili ile yapılamayan bir uygulama yerine kötü bir yazılım dili ile yapılabilecek bir uygulama yapmak daha iyidir.Yani yazdığınız yazılımın teoride iyi olması bir anlam ifade etmez eğer ekipte bu kabiliyet yok ise kabiliyet olan yerden devam edilmesi gerekir.

Daha doğru sonuçlar için makine öğrenmesi kullanılabilir ama yinde yukarıda bahsedilen kabiliyetler doğrultusunda bu olaya yeltenilmelidir.

Yazılımı yazdık nasıl test edeceğiz ?
Bu tarz bir projede yazılımsal yük diğer bütün birimlerden daha fazla olması gerekir. Şimdiye kadar hiç öyle olmasa da. yazılım ekibinin derhal çalışmalara başlaması ve olabildiğince gerçeğe yakın bir simulasyon ortamı kurması gerekir. Bu simulasyon ortamında SITL yardımı ile uçuş kontrolcüsünün tepkileri simule edilerek, yardımcı programlar ile bu uçuş konrolcüsü kontrol edilmeye çalışılır. Daha ileri giderek matematiksel modelleme yapılabilir. 

Simulasyon ortamı kurmak





