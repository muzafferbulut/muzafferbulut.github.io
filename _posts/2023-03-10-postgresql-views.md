---
title: PostgreSQL View Yapıları
date: 2023-03-10 09:30:30 +/-TTTT
categories: [PostgreSQL, SQL]
tags: [postgresql, views, gis]
---

### <b> 1. Giriş </b>

<div class='text-justify'>
SQL ile veri tabanını sorgulamak, veri filtrelemek ya da raporlama yapmak gerektiğinde her zaman basit SQL cümleleri yazmayız. Karmaşık sorgular yazmak, raporlar hazırlamak gerekebilir. Aynı SQL sorgusu farklı zamanlarda ve aynı uygulamanın farklı kısımlarında kullanılıyor da olabilir. Bu gibi durumlarda aynı komplex sorguyu defalarca yazmamak, gerektiğinde kolaylıkla çağırabilmek adına view yapıları kullanılır. Bir view oluşturulduğunda temel olarak karmaşık bir SQL sorgusunda isim atamış oluruz, bu nedenle arka planda bir veri değil SQL cümlesi tutulur. 
</div><br>

### <b> 2. PostgreSQL'de View Yapılarını Yönetmek </b>

<div class='text-justify'>
View yapıları arka planda veri değil bir SQL sorgusu saklar. Bu sorgu çoğu zaman birden fazla tablonun join yapılara birleştirilmiş oldukça kapsamlı ve karmaşık bir sorgudur. View olarak tanımlanmış bu sorgunun sonucuna temel anlamda mantıksal bir tablo gibi <b>SELECT</b> deyimi ile erişilebilir. View'lar için basit anlamda create ve select yapıları aşağıdaki gibidir.
</div><br>

```sql
-- create view deyimi "or replace" deyimi ile birlikte kullanılırsa daha
-- sonradan view tanımı değiştirilebilir.
CREATE VIEW OR REPLACE view_name AS
    SELECT a.id, a.col1, a.col2, b.id, b.col1
    FROM table_a a
    INNER JOIN table_b b ON b.id = a.b_id;
    -- burada view olarak oluşurulacak view_name ile çağrılacak 
    -- sorgunun tamamı yer alacaktır.

-- select view
SELECT * FROM view_name;
```

<div class='text-justify'>
Bir view'in tanımını değiştirmek için alter deyimi bir view'ı silmek için ise drop deyimi kullanılır. Örneğin bir view'ın ismini değiştirmek için aşağıdaki komut kullanılabilir.
</div><br>

```sql
-- view adını değiştirmek
ALTER VIEW table_a RENAME TO table_a_rename;

-- var olan bir view'ı silmek
DROP VIEW IF EXISTS table_a_rename;
```

<div class='text-justify'>
Temel olarak view yapıları bir SQL sorgusuna isim atanarak karmaşık sorguların basitleştirilmesine, kullanıcıların görme yetkilerine sahip olduğu alanların view olarak tanımlanması yoluyla ise yetkilendirme gibi konularda fayda sağlar.
</div><br>

<div class='text-justify'>
View yapıları temel anlamda bir SQL sorgusu tuttuğu için insert, update, delete gibi işlemler yapılamaz. Ancak bazı özel kouşllar altında bu komutları sisteme gönderme imkanı vardır. Ama öyle bir durumda da yine işlem view'ın kullandığı tabloda yapılacağından verinin bulunduğu yerde bu işlemi yapmak daha sağlıklıdır.
</div><br>

### <b> 3. Materialized Views </b>

<div class='text-justify'>
Basit bir view kullanımında select deyimi ile view çağrıldığında arka planda aslında view'ın tuttuğu SQL sorgusu çalıştırılır. Bu işlem büyük veri tabanlarında her seferinde yeniden çalışacak bir karmaşık SQL sorgusu demektir. Dolayısıyla performans kaybı kaçınılmaz olur. Bu gibi durumlarda zaman ve performans avantajı sağlamak için PostgreSQL view kavramını verilerin fiziksel olarak depolanabildiği Materialized View'a genişletir.
</div><br>

<div class='text-justify'>
Materialized view yapılarında karmaşık sorgu sonucu önbelleğe alınır ve periyodik aralıklarla güncelleme yapışabilir. Dolayısıyla hem performans avantajı sağlanır hem de veri güncel tutulur. Sağladığı avantaj dolayısıyla genellikle performans gerektiren veri ambarı ve iş zekası uygulamalarında kullanılır. Aşağıdaki şekilde bir materialized view tanımlanabilir.
</div><br>

```sql
-- materialized view oluşturma
CREATE MATERIALIZED VIEW view_name as
SELECT * FROM table_a
WITH [NO] DATA;
```

<div class='text-justify'>
With deyimi materialized view'lar oluşturulurken içine veri eklemek için kullanılır. View oluşturulurken içine veri eklenmezse boş bir view olacağı için veri eklenene kadar okunma işlemi yapılamaz.
</div><br>

<div class='text-justify'>
Bir materialized view oluşturduk, güncelleme işlemlerini nasıl yapacağız? Materialized view yapıları önbellekte veri tuttuğu için güncelleme işlemleri atlanmamalıdır. Yetersiz güncelleme işlemi güncel veriye daha geç ulaşmamıza sebep olabilir.
</div><br>

```sql
-- verileri yükleme
REFRESH MATERIALIZED VIEW view_name;
```

<div class='text-justify'>
Ancak bu kullanım sırasında tablo kilitleneceği için sorgu atılamaz. Bu sebeple aşağıdaki kullanım önerilir.
</div><br>

```sql
REFRESH MATERIALIZED VIEW CONCURRENTLY view_name;
```

<div class='text-justify'>
Bu işlemde view'ın geçici olarak güncellenmiş bir sürümü oluşturulur ve aradaki farklar kontrol edilir. 
</div><br>

### <b> 4. Recursive View </b>

<div class='text-justify'>
PostgreSQL 9.3 sürümü ile birlike bir standart haline gelen recursive (özyinelemeli view) kendi kendini çağıran view'ların oluşturulmasını ifade eder. Aşağıdaki şekilde oluşturulur.
</div><br>

```sql
-- view oluşturmak
CREATE RECURSIVE VIEW view_name(columns) as
SELECT columns

-- ya da
CREATE VIEW view_name 
AS
  WITH RECURSIVE cte_name (columns) AS (
    SELECT ...)
  SELECT columns FROM cte_name;
```