---
title: PostgreSQL Trigger Yapıları
date: 2023-04-01 09:30:30 +/-TTTT
categories: [PostgreSQL, SQL]
tags: [postgresql, views, SQL, triggers, gis]
---

### <b> 1. Giriş </b>

<div class='text-justify'>
PostgreSQL Trigger bir tabloyla ilişkili insert, update, delete ve truncate işlemleri meydana geldiğinde çağrılan kullanıcı tanımlı bir fonksiyondur. Bu özellik veri tabanı işlemerinin daha esnek hale getirilmesine ve özelleştirilmesine olanak sağlar. Bir fonksiyon oluşturulur ve ilgili tablo ile bağlanır. Tabloya bağlanırken hangi değişikliklerde bu fonksiyonun tetikleneceği de belirtilir. İlgili tabloda belirtilen değişiklikler gerçekleştiğinde fonksiyon tetiklenerek işlevini yerine getirir.
</div><br>

<div class='text-justify'>
Diğer kullanıcı tanımlı fonksiyonlar ile farkı ise belirtilen işlem gerçekleştiğinde kendiliğinden tetiklenmesidir. Diğer kullanıcı tanımlı fonksiyonlarda kullanıcı yazdığı SQL scripti içinde fonksiyonu kendisi çağırarak kullanır. Trigger ise uygun şartlar oluştuğunda fonksiyon çalışarak işlevini yerine getirecektir.
</div><br>

<div class='text-justify'>
PostgreSQL satır ve ifade düzeyi olmak üzere 2 tip trigger tipi sağlar. Satır düzeyindeki trigger fonksiyonları değişen satır kadar tetiklenirken ifade düzeyinde triggger yapıları değiştiren ifade (UPDATE, INSERT, DELETE TRUNCATE) kadar tetiklenir. 
</div><br>

<div class='text-justify'>
Triggerlar, veri tabanı işlemlerini denetlemek ve özelleştirmek için kullanılabilir. Örneğin, bir trigger, belirli bir tablodaki verileri otomatik olarak değiştirebilir veya silebilir. Ayrıca, triggerlar, veri tabanı güvenliğini artırmak için de kullanılabilir. Örneğin, bir trigger, belirli bir tablodan veri silinirken bir kayıt tutabilir ve bu kaydı daha sonra denetim amaçlı kullanabilir.
</div><br>

### <b> 2. Database Trigger </b>

<div class='text-justify'>
Kullanıcı tanımlı fonksiyonlara benzeyen trigger, herhangi bir değişken almaz. Bir trigger şu şekilde tanımlanır.
</div><br>

```sql
CREATE TRIGGER trigger_name
{ BEFORE | AFTER } { event [ OR ... ] }
ON table_name
[ FROM referenced_table_name ]
[ NOT DEFERRABLE | [ DEFERRABLE ] [ INITIALLY IMMEDIATE | INITIALLY DEFERRED ] ]
[ FOR [ EACH ] { ROW | STATEMENT } ]
[ WHEN ( condition ) ]
EXECUTE FUNCTION function_name ( arguments );
```
<div class='text-justify'>
Burada trigger'ın hangi şartlarda tetikleneceği event kısmında belirtilir. Table name kısmı trigger'ın bağlı olduğu tablonun adıdır. Execute Function kısmında trigger'ın çalıştırmasını istediğimiz fonksiyon yer alır. Örneğin worker_status tablosunda bir değişiklik olduğunda bildirim göndermek için şöyle bir trigger yazılabilir.
</div><br>

```sql
CREATE OR REPLACE FUNCTION send_notification()
RETURNS trigger AS $$
BEGIN
  -- bildirim gönder
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER notify_changes
AFTER INSERT OR UPDATE OR DELETE
ON worker_status
FOR EACH ROW
EXECUTE FUNCTION send_notification();
```

<div class='text-justify'>
Bir tablodan trigger silmek için drop deyimi ile birlikte aşağıdaki söz dizimi kullanılır.
</div><br>

```sql
DROP TRIGGER [IF EXISTS] trigger_name 
ON table_name [ CASCADE | RESTRICT ];
```

<div class='text-justify'>
Alter deyimi ile daha önceden oluşturulan trigger yeniden adlandırılabilir.
</div><br>

```sql
ALTER TRIGGER trigger_name
ON table_name 
RENAME TO new_trigger_name;
```

<div class='text-justify'>
Bir trigger alter table disable trigger deyimi kullanılarak devre dışı bırakılır.
</div><br>

```sql
ALTER TABLE table_name
DISABLE TRIGGER trigger_name | ALL
```

<div class='text-justify'>
Bir trigger veya bir tabloyla ilişkili tüm trigger'ları etkinleştirmek için ALTER TABLE ENABLE TRIGGER deyimini kullanılır.
</div><br>

```sql
ALTER TABLE table_name
ENABLE TRIGGER trigger_name |  ALL;
```

### <b> 3. Server Trigger </b>

<div class='text-justify'>
Server triggerlar, veritabanı yönetim sistemleri tarafından desteklenen özel bir nesnedir. Bu triggerlar, belirli bir veritabanı sunucusunda gerçekleşen olaylar (örneğin, bağlantı açma, bağlantı kesme, sorgu çalıştırma vb.) tetiklendiğinde otomatik olarak çalışan özel işlevlerdir.
</div><br>

<div class='text-justify'>
Server triggerlar, veritabanı yönetim sistemleri tarafından desteklenen birçok olay için kullanılabilir. Örneğin, bir sunucuya yeni bir bağlantı açıldığında otomatik olarak belirli bir sorgu çalıştırılabilir veya bir bağlantı kesildiğinde otomatik olarak belirli bir işlem gerçekleştirilebilir.
</div><br>

<div class='text-justify'>
Server triggerlar, özellikle veritabanı yönetimi için otomasyon sağlamak için çok kullanışlıdır. Örneğin, bir server trigger, belirli bir veritabanı sunucusunda bir kullanıcının belirli bir tabloya erişmesini engelleyebilir veya belirli bir sorgu çalıştırıldığında otomatik olarak bir rapor oluşturabilir. Şifre denemelerini denetleyerek tehlikeli bir durumda ip'yi kara listeye alabilir.
</div><br>

<div class='text-justify'>

</div><br>