---
title: Veri Tabanı Saldırıları ve Veri Tabanı Güvenliği
date: 2023-01-15 21:30:30 +/-TTTT
categories: [Not Defteri, Güvenlik]
tags: [sql, brute force, sql injection, veritabanı güvenliği, database security, sql server]
---

### <b> 1. Giriş </b>

<div class='text-justify'>
Bilişim teknolojilerinde meydana gelen gelişim hizmetlerin çeşitlenmesine olanak tanımıştır. Bu süreçte birçok iş ve uygulama online ortama taşınmış, dijital dönüşüm kapsamında da taşınmaya devam etmektedir. Bu durum insan hayatı için çok önemli kolaylıklar sağlıyor olsa da yeni tehditlerin de ortaya çıkmasına sebep olmaktadır.  Olası mağduriyetleri engellemek için verilerin etkin bir şekilde depolanması ve korunması gereklidir.
</div><br>

<div class='text-justify'>
Gelişen dünyada hizmetlerden ve teknolojiden yararlanabilmek için çoğu zaman kişisel bilgilerimizi kullanıyoruz. Dolayısıyla bu durum verilerin etkin bir şekilde korunmasını zorunlu kılmaktadır.
</div><br>


### <b> 2. Brute Force Saldırıları </b>

<div class='text-justify'>
Brute Force kaba kuvvet anlamına gelmektedir. Temel prensibi sisteme erişerel çok sayıda giriş denemesi yapıp parolayı ele geçirmektir. Brute Force ile parola elde ediltikten sonra verileri dışarı çıkarma ve hatta format atmaya kadar ileri işlemler yapılabilir. En yaygın kullanılan saldırı yöntemlerinden biridir.
</div><br>

<div class='text-justify'>
Brute Force saldırılarının başarılı olmasında başta gelen sebeplerden biri veri tabanı default kullanıcılarını disable hale getirmemektir. Şifre denemeleri sırasında online ortamda kolaylıkla bulunabilen şifre sözlükleri kullanılmakta, default kullanıcılar ile denenmektedir ve bu bir güvenlik açığına sebep olmaktadır. Bu durumun önüne geçmek için default ayarları değiştirip, default kullanıcıları disable hale getirmenin yanında sistem belli bir yanlış şifre denemesinden sonra aynı kullanıcı için deneme yapmaya izin vermemelidir.
</div><br>

<div class='text-justify'>
Brute Force saldırılarında yerel ağda yapılan saldırılar çok önemlidir. İnternet üzerinden yapılan saldırılarda saniyede 15-20 tane deneme yapılabiliyorken yerel ağdan yapılan saldırılarda farklı yöntemlerle deneme sayısı 5 bin seviyelerine çıkmaktadır.<br>

Bu aşamada sisteme başarılı ve başarısız girişleri log'layan Login Auditing menüsünden başarısız girişler loglanabilir. Bu şekilde sık sık başarısız giriş yapmaya çalışan kaynaklar tespit edilerek engellenebilir. <br>

Sıklıkla başarısız şifre denemesi yapan IP kara listeye alınarak şifre doğru olsa bile veri tabanına girmesine izin vermeyecek bir server trigger yazılarakta brute force saldırılarına karşı önlem alınabilir.
</div><br>

### <b> 3. SQL Injection Saldırıları </b>

<div class='text-justify'>
SQL Injection zararlı olmayan SQL cümlelerinin arasına zararlı olacak kodların enjekte edilmesi işlemidir. Brute Force saldırısının aksine burada amaç şifre kırmak değildir. Bu saldırının yapılabilmesi için zaten veri tabanına bir şekilde bağlı olmak gerekmektedir. <br><br>

Örneğin hazırlanan bir programda bulunan bir açık veri tabanına sql cümlesi göndermeye olanak tanıyorsa kötü niyetli kişiler bu açıktan faydalanarak sadece SQL cümleleri kullanarak diske format atabilir. Farklı olarak yine SQL kullanırak zararlı bir yazılım sunucuya yüklenebilir. <br><br>

Bir SQL Injection saldırısında genellikle yapılan ilk şey bir kullanıcı eklemektir. Daha sonra bu kullanıcı aracılığıyla sisteme erişip zararlı faaliyetleri gerçekleştirmektir. Bu işlemlerin önüne geçmek için her yeni kullanıcı oluşuturulduğunda ilgili bilgileri mail atan bir server trigger yazılabilir. <br>
</div><br>

<b>NOT: </b> Notlar Sayın [Ömer Çolakoğlu](https://www.linkedin.com/in/omercolakoglu/) hocamızın BTK Akademi platformundaki [Veri Tabanı Saldırıları ve Veri Tabanı Güvenliği](https://www.btkakademi.gov.tr/portal/course/veri-tabani-saldirilari-ve-veri-tabani-guvenligi-6569) kursu esnasında alınmış notlardır. Kendisine teşekkür ederim.