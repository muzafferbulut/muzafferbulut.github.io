---
title: Ubuntu Üzerine PostgreSQL Kurulumu
date: 2023-02-04 21:30:30 +/-TTTT
categories: [PostgreSQL, Kurulum]
tags: [postgresql, ubuntu,linux, kurulum]
---

<div class='text-justify'>
PostgreSQL veri tabanı dünyasının en hızlı ve güvenli açık kaynak kodlu veri tabanı yönetim sistemlerinden biridir. Arkasına aldığı açık kaynak kod topluluğunun
desteğiyle gelişmeye devam etmektedir. Artan lisans maliyetlerinden ötürü de bir çok kurum veri tabanı yönetim sistemi olarak PostgreSQL tercih etmektedir.
</div><br>

<div class='text-justify'>
Ubuntu linux tabanlı özgür ve ücretsiz bir işletim sistemidir. PostgreSQL varsayılan olarak tüm ubuntu sürümlerinde mevcuttur ve postgresin alternatif sürümlerine de
postgresql apt deposundan erişilebilir.
</div><br>

<div class='text-justify'>
PostgreSQL'i en güncel haliyle kurmak için öncelikle kullanılan ubuntu depolarının güncellenmiş olması gerekmektedir. Terminalde update komutu çalıştırıp depoları 
güncelleyerek kurulum başlatılabilir.
</div><br>

```
$ sudo apt-get update
```

<div class='text-justify'>
Depoların güncellenmesi tamamlandıktan sonra Postgresql kuruluman başlanır. Aşağıdaki komut kullanılarak PostgreSQL paketleri indirilir.
</div><br>

```
$ sudo apt-get install postgresql postgresql-contrib
```

<div class='text-justify'>
Komutun çalışması tamamlandğında postgresql kurulmuş olacaktır. Spesifik bir sürüm kurulmak isteniyorsa postgresql parametresi sonuna (örneğin postgresql-12) eklenebilir. postgresql-contrib paketi temel özellikler için gerekli değildir ancak veri tabanı sistemi için ek araç ve eklentiler içermektedir. Örneğin bu paket veri tabanı yönetimi ve veri işleme gibi uygulamalar için eklentiler ve veri tabanı güvenliğini artırmak için araçlar içerebilir. PostgreSQL'in kurulduğu dizin incelenecek olursa veri tabanı için gerekli olan konfigürasyon dosyaları görülür.
</div><br>

```
$ ls /etc/postgresql/14/main

-- conf.d,  environment, pg_ctl.conf, pg_hba.conf, pg_ident.conf, start.conf
```

<div class='text-justify'>
<b>conf.d</b> dizini PostgreSQL veri tabanı yapılandırması için kullanılan eklenti dosyalarınıns saklandığı dizindir. Bu dosyalar veri tabanı yapılandırmasının bazı özelliklerini değiştirmek için kullanılabilir. <br> <b>Environment</b> dizini veri tabanının çalıştığı esnada kullanılacak ortam değişkenlerini saklamak için kullanılır.<br> <b>pg_ctl.conf</b> PostgreSQL veri tabanının kontrol edilmesi için kullanılan bir yapılandırma dosyasıdır. Veri tabanı servisinin başlatılması, durdurulması gibi işlemlerin kontrol edilmesinde kullanılır. Bu süreçlerde kullanılmak üzere belirli parametre ve ayarlar pg_ctl.conf dosyası aracılığıyla yapılır.<br> <b>pg_hba.conf (Host Based Authentication Conf)</b> dosyası ile veri tabanının erişim denetimi yapılır. Bu konfigürasyon dosyası veri tabanına bağlantı isteği atan kullanıcıların hangi IP adresleri ve makine adları üzerinden ve hangi yetkilerle erişime izin verileceğini tanımlar. Veri tabanı gizliliği ve güvenliği konusunda önemli rol oynar.<br> <b>pg_ident.conf (PostgreSQL Identification Mapping Conf)</b> dosyası PostgreSQL veri tabanında kullanıcı tanımlama işlemleri için kullanılan bir konfigürasyon dosyasıdır.<br> Son olarak <b>start.conf</b> dosyası veri tabanının nasıl başlatılacağının tanımlandığı konfigürasyon dosyasıdır. Veritabanının başlatılması sırasında kullanması gereken belirli parametreleri ve ayarları tanımlar. Örneğin, veritabanının bellek kullanımı, dosya sistemi gibi özelliklerinin nasıl yapılandırılacağı bu dosya üzerinden belirlenebilir. Bu dosya, veritabanının performansını ve çalışmasını optimize etmek için önemli bir rol oynar.
</div><br>


<div class='text-justify'>
Veri tabanı servisinin durumunu öğrenmek için service postgresql komutu kullanılır. Bu komut veri tabanı hizmetinin durumunu görmemize, veri tabanı hizmetini başlatmamıza ve durdurmamıza olanak tanır.
</div><br>

```
-- veri tabanı servisinin durumunu öğrenmek için
$ service postgresql status

-- veri tabanı servisini durdurmak için
$ service postgresql stop

-- veri tabanı servisini başlatmak için
$ service postgresql start

-- veri tabanı servisini yeniden başlatmak için
$ service postgresql restart
```

<div class='text-justify'>
PostgreSQL kurulumu tamamnlandıktan sonra postgres adında bir sistem hesabı tanımlanır. Root kullanıcısından postgres kullanıcısına geçmek için <bold>sudo su postgres</bold> komutu kullanılaibilir. Ardından <bold>psql</bold> komutu vererek psql shell ekranına geçiş sağlanır.
</div><br>