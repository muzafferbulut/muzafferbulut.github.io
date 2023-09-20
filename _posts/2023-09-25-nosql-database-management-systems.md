---
title: NoSQL Veri Tabanı Yönetim Sistemleri
date: 2023-09-25 09:30:30 +/-TTTT
categories: [MongoDB, NoSQL]
tags: [mongodb, nosql, document based, collection, database]
---

### <b> 1. Giriş </b>

<div class='text-justify'>
Veri tabanı yönetim sistemlerinin dönüşümü ve gelişimini 3 aşamada incelemek mümkündür. İlk aşamada veriler ilkel bilgisayarlarda sıralı bantlar halinde saklanırken veriye erişim ve yönetim için gelişmiş kodlama bilgisi gerekiyordu. Bilgisayar teknolojilerinin gelişimi henüz olgunlaşmamışken veri operasyonları, sorgulama ve raporlama işlemleri için efor harcamak gerekiyordu.
</div><br>

<div class='text-justify'>
1970'li yıllara gelindiğinde E.F. Codd tarafından yayımlanan bir makale ile günümüzde de halen yoğun olarak kullanılan ilişkisel veri tabanı modelinin temelleri atıldı. Verileri tablolar halinde ele alan, tablolar arası kurulan ilişkiler ile çapraz sorgulama olanağı tanıyan ilişkisel veri tabanı modeli günümüzde verinin hayati önem taşıdığı uygulamalarda tercih edilmektedir. ACID (Atomicity, Consistency, Isolation, Durability) ilkeleri ile veri tutarlılığı açısından avantaj sağlayan bu model SQL ile de veri erişimi, yönetimi ve raporlama gibi işlemlerde de gelişmiş kodlama bilgisi gerekliliğini ortadan kaldırmıştır.
</div><br>

<div class='text-justify'>
Gelişen teknoloji veri kaynaklarının çeşitlenmesine, veri boyutunun ve depolama maliyetlerinin daha hızlı yükselmesine sebep oldu. Farklı kaynaklardan toplanan, yapısal olmayan, yatay ve düşey olarak hızlı büyüyen veri yığını olarak adlandırılan bu yapıya büyük veri kümesi adı verilir. İlişkisel veri tabanı yönetim sistemleri yapısal veri kümelerini yönetmek ve sorgulamak için tasarlandığından büyük veri için optimmum çözüm değildir. Sorgulama, raporlama, ölçeklenebilirlik ve performans problemlerinin yanında depolama maliyetleri de hızla artmaktadır. Bu probleme çözüm olarak NoSQL veri tabanı yönetim sistemleri önerilmiştir.
</div><br>

<div class='text-justify'>
NoSQL veri tabanı yönetim sistemleri yapısal olmayan, farklı kaynaklardan toplanan, yatay ve dikey olarak hızlı büyüyen veri setlerini optimum maliyetle depolamak ve bu veri setlerinden faydalı bilgi çıkarmak için bir çözüm sunar. ACID ilkelerine karşılık olarak NoSQL veri tabanı yönetim sistemlerinde BASE (Basically Avaliable, Soft State, Eventually Consistent) ilkeleri benimsenmiştir. Bu ilkeler sayesinde NoSQL veri tabanı yönetim sistemleri esnek, kolay ölçeklenebilir ve yüksek performanslı bir yapıya sahip olmuştur.
</div><br>

### <b> 2. NoSQL Veri Tabanı Yönetim Sistemi Tipleri </b>

<div class='text-justify'>
NoSQL sistemler veri saklama ve okuma yöntemleri açısından çeşitlilik gösterir ve temel anlamda 4 bölümde incelenebilir.
</div><br>

#### <b> 2.1. Anahtar - Değer (Key-Value) Modeli </b>

<div class='text-justify'>
Anahtar-değer veri modeli en basit ve en verimli yapılardan biridir. Hash tablo kullanılması fikri üzerine inşa edilmiş olan bu veri tabanı modeli benzersiz anahtarlar ve bu anahtarlar ile ilişkilendirilmiş verilerden oluşur. Dolayısıyla bu sistem bir anahtar-deper deposu olarak düşünülebilir. Anahtarlar indeks kullandığı için ilişkisel veri tabanı modeline göre daha hızlı olmasına karşın veri boyutu arttıkça verimlilik düşer. Tutarlılık yerine yüksek ölçeklenebilirliği benimseyen bu sistemde belli sorgu senaryolarında yüksek performans potansiyeli olmasına rağmen karmaşık sorgulamalar yapma yeteneği kısıtlıdır. Forumlar, alışveriş siteleri, uygulamalar için önbellekleme gibi uygulama alanlarıda kullanılmaktadır. Örnek olarak Redist, Hazelcaast ve DynamoDB verilebilir.
</div><br>

#### <b> 2.2. Sütun Bazlı (Wide Column) Modeli </b>

<div class='text-justify'>
Büyük hacimde verileri dağıtık olarak depolayabilmek için geliştirilmiş veri modelidir. Yapı olarak ilişkisel ve anahtar-değer veri tabanı modelleri arasında yer alır. İlişkisel veri tabanı modelinden farklı olarak veriler birbiri ile ilişkilendirimiş sütunlarda tutulur. Veri depolamada yüksek ölçeklenebilirlik sunar ancak maliyet ve performans problemleri vardır. Mesajlaşma sistemleri, veri madenciliği ve analitik uygulamalar için kullanılabilir. Örnek olarak Cassandra ve HBASE verilebilir.
</div><br>

#### <b> 2.3. Belge Tabanlı (Document Based) Modeli </b>

<div class='text-justify'>
Belge tabalı modelde veriler versiyonlanmış belgeler halinde tutulur. Eğer kullanıcı tarafından belirtilmediyse her belgeye bir eşsiz değer atanır. Bir veri tabanındaki belgeler farklı alanlara sahip olabileceği gibi iç içe geçmiş belgeler de oluşturulabilir. Belgelerin içeriği ilişkisel veri tabanlarına benzese de veriler belli bir şemaya zorlanmadığı için yata ölçeklenebilirlik ve esneklik gibi avantajları vardır. İçerik yönetim sistemi, blog yazımı, sosyal medya uygulamaları, loglama işlemleri, sensör verilerinin depolanması gibi alanlarda kullanılabilir. Örnek olarak MongoDB ve CouchDB ortamları verilebilir.
</div><br>

#### <b> 2.4. Graf (Graph Based) Modeli </b>

<div class='text-justify'>
Diğer veri tabanı türlerine nazaran daha az kullanılan graf tabanı veri tabanı türleri birbirine bağlı olan verileri yönetme yeteneği öne çıkar. Graf tabanlı veri tabanı sistemleri düğümler ve düğümler birbirine bağlayan kenarlardan oluşur. Düğümler nesneleri, kenarlar ise nesneler arasındaki ilişkileri temsil eder. ACID ilkelerine uyumludur ve hızlı ve kolay ölçeklenebilir yapıdadır. Sosyal medya uygulamaları, en yakın rotayı bulma, içerik yönetimi, güvenlik ve erişim kontrolü gibi uygulama alanlarında kullanılır. Örnek olarak Neo4J, InfoGrid ve InfoGraph verilebilir.
</div><br>