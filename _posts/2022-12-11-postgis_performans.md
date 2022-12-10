---
title: PostGIS Performans 'Tricks'
date: 2022-12-11 21:30:30 +/-TTTT
categories: [Veritabanı, PostGIS]
tags: [postgresql, postgis,spatial database, spatial query performance]
---

### 1. Giriş

<div class='text-justify'>
İlişkisel veri tabanı yönetim sistemlerinin en güçlü açık kaynak kodlu temsilcisi olan PostgreSQL, mekansal veri yönetimi açısından da sektörün en güçlü temsilcilerindendir. Arkasına açık kaynak kod topluluğunun gücünü alan PostgreSQL, 13 Ekim’de 15.1. sürümünün duyurulmasıyla gelişmeye ve geliştirilmeye devam etmektedir.
</div><br>

<div class='text-justify'>
PostGIS eklentisinin kurulması ile birlikte koordinat dönüşümü, geometri işleme, analiz etme, sorgulama, sınama, topolojik ilişkileri kontrol etme gibi işlevleri yerine getiren oldukça geniş bir fonksiyon havuzu kolaylıkla kullanılabilir duruma gelmektedir.
</div><br>

### 2. Geometri Sorguları Performans Artırma Teknikleri

<div class='text-justify'>
PostGIS vektör, raster, topoloji gibi eklentileri ile binden fazla fonksiyonu bünyesinde barındıran en verimli açık kaynak kodlu mekansal veri yönetimi aracıdır. Kurulumu ve kullanımı oldukça kolaydır. Yaygın olarak kullanılan GIS yazılımları ile kolaylıkla entegre edilebildiği için konum tabanlı uygulama geliştiricileri tarafından yoğun ilgi görmektedir. PostgreSQL veri tabanına basit bir prosedür izlenerek dahil edilir ve PostgreSQL’in sağladığı temel veri türlerini genişletir.
</div><br>

<div class='text-justify'>
PostGIS kendine has bir geometri tanımına sahiptir. Geometride artan kırık noktalar tanımın uzamasına ve dolayısıyla performans sorunlarının ortaya çıkmasına sebep olmaktadır. Veri hacmi büyük olan konumsal uygulamalarda performans önemli bir parametredir ve sorgu tasarımlarının performans kriteri göz önüne alınarak yapılması gerekmektedir. Mekansal fonksiyonların performansını önemli ölçüde artıran farklı yaklaşımlar mevcuttur.
</div><br>

#### 2.1. Mekansal İndeksleme

<div class='text-justify'>
Mekansal verilerin tutulduğu tabloda temel bir st_intersects fonksiyonu çalıştırdığımızı düşünelim. PostgreSQL Explain Analyze aracı ile sorgunun nasıl çalıştığı incelenirse, tabloda temel bir arama yapıldığı görülür ki dikey olarak büyüyen verilerde bu durum maliyetin artmasına sebep olacaktır. Bu durumun önüne geçmek için uygulayabileceğimiz en kolay yöntemlerden biri mekansal indekslemedir. Tabloya eklenecek mekansal indeks ile sorgu süresi %97 oranında azalmaktadır. Mekansal indeks tüm tablo kayıtlarını taramak yerine ilgili boundary box alanında kalan kayıtların taraması yapılır. Boundary box geometri verisine nispeten çok basit yapıda olduğundan önemli fark yaratabilir, hatta bazı durumlarda yeterli olabilir.
</div><br>

##### 2.1.1. GIST İndeks

<div class='text-justify'>
GIST (Generalized Search Tree) geometrik veri türlerini indekslemek için kullanılan bir ‘full text search’ aracıdır. BTree indeks genellikle karşılaştırma yapıları için kullanılırken GIST ağaç yapısında veri tutmasına karşın daha çok geometri ve text dokümanları gibi operatörler için kullanılmaktadır.
</div><br>

#### 2.2. İndeks Sorguları

<div class='text-justify'>
Mekansal indeks eklenmiş bir tablodan veriyi hızlı bir şekilde sorgulamanın en hızlı yolu sadece indeks sorgularıdır. && operatörü ile işlemleri boundary box üzerinden yapabiliriz. Ancak geniş alanlarda boundary box alanları da büyüyeceğinden potansiyel sonuç kümesi de büyür. Bir nehir geometrisi ele alınırsa, boundary box alanı nehrin kapladığı alan yanında oldukça büyük kalacaktır. Bu da potansiyel sonuçların sayısını artıracaktır. Yalnızca indeks sorguları yaparken gerçekleşen hız artışının gelen yanlış veri yükünü karşılayıp karşılamayacağının analizi yapılmalıdır.
</div><br>

#### 2.3. Veriyi Alt Alanlara Bölme

<div class='text-justify'>
Yine bir nehir geometrisi düşünelim. Bir nehir geometrisi çok fazla kırık nokta içerir ve uzun bir çizgi ile ifade edilir. Dolayısıyla boundary box alanı gerçek alandan çok büyük olur. Mekansal indeksler işlemleri boundary box alanı üzerinden yaptığından PostGIS ilk aşamada nehirden çok uzakta olan geometrileri de dikkate alacak, ancak detaylı hesaplamaları yaptıktan sonra gerçek sonuca ulaşacaktır. Dikey olarak büyük tablolarda bu bir performans problemi yaratacaktır. Bundan kaçınmak için, nehri ifade eden boundary box alanını daha küçük parçalara bölerek bu parçaları farklı bir tabloda tutmak gerekir. Daha küçük ve daha basit olan bu alan parçaları muhtemel yanlış cevap sayılarını önemli ölçüde azaltacak, dönüş süresini de pozitif olarak etkileyecektir. Ancak bu çözümün de veritabanına insert ve depolanacak veride artış gibi bir maliyeti olacaktır.
</div><br>

#### 2.4. Geometri Basitleştirme

<div class='text-justify'>
Karmaşık geometrilerde yer alan kırık noktalar sorgu sonuçları açısından her zaman önem arz etmeyebilir. Ancak bu geometri verileri sorgularda kullanılırken tüm detaylar dikkate alındığında sorgu maliyeti artar. Bu gibi durumlarda geometri verisi (örneğin st_simplify fonksiyonu ile) gerçeğe en yakın olacak şekilde basitleştirilir. Yani gereksiz detaylar geometriden çıkarılır. Bu - daha basit - geometriden yapılacak sorgu ya da analiz daha hızlı çalışacaktır.
</div><br>

### 3. Sonuç

<div class='text-justify'>
PostGIS tablolarında tutulan geometri verilerinin sorgulanması ve analizi esnasında performans açısından önemli fark yaratabilecek tekniklerin bazılarına değinmiş olduk. Elbette bu teknikler artırılabilir. Her durumda veri, proje ve sorgu tasarımına uygun tekniğin seçilmesi, gerekli durumlarda ar-ge çalışmalarının yapılması ve sonrasında uygulamaya geçilmesi maliyetleri azaltma açısından önemli olacaktır.
</div><br>