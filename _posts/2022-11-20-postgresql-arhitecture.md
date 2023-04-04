---
title: PostgreSQL Mimarisi
date: 2022-11-20 21:30:30 +/-TTTT
categories: [PostgreSQL, Administration]
tags: [postgresql, mimari,spatial database, architecture,]
---

### <b> 1. Giriş </b>

<div class='text-justify'>
70'li yıllarda önerilen ilişkisel veri tabanı modelinin en güçlü açık kaynak kodlu temsilcisi olan PostgreSQL, kullanıcılarına sağladığı avantajlar sayesinde halen etkin olarak kullanılmaktadır. Güvenlik, hız, mekansal veri desteği gibi avantajlı özellikler sunan PostgreSQL lisans maliyetlerini de ortadan kaldırmıştır. Client-server mimarisinde multi-process hizmet veren bir ilişkisel veri tabanı yönetim sistemidir. Aynı zamanda mekansal veri desteği sunan ilk ilişkisel veri tabanı yönetim sistemlerinden biridir.
</div><br>

<div class='text-justify'>
PostgreSQL mimarisi memory, process ve disk üzerindeki dosyalardan oluşur. Process'ler postgres server, backend, background, replication associated process olmak üzere 4 ana başlıkta incelenebilir.
</div><br>

### <b> 2. PostgreSQL Mimarisi </b>

#### <b> 2.1. Postgres Server Process </b>

<div class='text-justify'>
Önceki sürümlerde postmaster olarak adlandırılan postgres server process <b>pg_ctl</b> yardımcı programının <b>start</b> komutu ile postgres server başlatılır. Postgres server process server çalıştığında ayağa kalkan ilk process'tir ve diğer process'lerin parent process'idir. Sistem başlatıldığında <b>shared memory</b>'den postgresql server'in kullanacağı ram alanı allocate edilir ve background process'ler başlatılır. Varsa replication processler çalıştırılır ve sunucu kullanıcıyı dinlemeye başlar. Kullanıcıdan bir talep geldiği zaman bir backend process başlatılır ve kullanıcıdan gelen tüm talepleri de bu backend process yakalar.
</div><br>

<div class='text-justify'>
Postgres server process default olarak 5432 portu olmak üzere aynı anda yalnızca bir portu dinler. Ancak port numaraları farklı olmak üzere aynı hostta birden fazla PostgreSQL veri tabanı çalıştırmak mümkündür.
</div><br>

#### <b> 2.2. Backend Process </b>
<div class='text-justify'>
Backend process (postgres), postgres server process tarafından başlatılan ve istemciden gelen sorgu ve statement'ları yakalayacan process'tir. Postgres istemciyle tek TCP bağlantısı üzerinden iletişim kurar ve bağlantı kesildiğinde process sonlandırılır. Postgres aynı anda yalnızca bir veri tabanı üzerinde operasyonlar gerçekleştirebildiği için bağlantısı sırasında veri tabanını açıkça belirtmek gereklidir. </div><br>

<div class='text-justify'>
PostgreSQL'in dahili bağlantı havuzu olmadığı için kısa zamanda sık sık bağlanma/bağlantıyı kesme gibi işlemlerde (web sitesi hizmetleri gibi) backend proces oluşturma maliyeti artacaktır. Bu maliyetin veri tabanı üzerindeki olumsuz etkilerini minimize etmek için <b>pgbouncer ve pgpool-II</b> gibi connection pooling yazılımları önerilir.
</div><br>

#### <b> 2.3. Backgorund Process </b>
<div class='text-justify'>
Her bir background process'in kendine özel fonksiyonu ve işleri vardır. Genel olarak sunucuda arka planda çalışan yönetimsel işleri gerçekleştiren process'lerdir. <b>ps -ef</b> komutu ile işletim sistemi üzerinde çalışan processleri, <b>ps -ef | grep postgres</b> komutu ile komutu ile postgresle ilgli çalışan background processleri görüntüleyebiliriz. Background process'ler logger, checkpointer, background writer, walwriter, autovacuum launcher, stats collector olarak sıralanabilir.
</div><br>

<b>Background Writer :</b> Shared buffer pool’da bulunan dirty blokların (değiştirilmiş bloklar) memory'den kalıcı diske yazılması işlemini gerçekleştirir. <br>

<b>Checkpointer :</b> Her checkpoint_timeout periodunda ve max_wal_size parametresi aşıldığında checkpoint işlemini gerçekleştirir.<br>

<b>Autovacuum Launcher :</b> Vacuum process'inin otomatik olarak belirli zamanlarda başlatılmasından sorumlu process'tir.<br>

<b>Wal Writer :</b> Wal bufferda bulunan veriyi wal dosyalarına kalıcı olarak yazmakla sorumludur. Wal dosyaları transaction bilgilerini tutan log dosyalardır. Sistemde ani kapanma durumunda veri kaybı oluşmasını önlemek ve veritabanını recover etmek için kullanılan dosyalardır.<br>

<b>Statistic Collector :</b> Verittabanı istatistiklerinin toplanmasını sağlayan processtir.<br>

<b>Logger :</b> Hata mesajlarını log dosyalarına yazan processtir.<br>

<b>Archiving :</b> Archive.log modundayken WAL dosyasını belirtilen dizine kopyalar.s

### <b> 3. PostgreSQL Memory Yapıları </b>
<div class='text-justify'>
PostgreSQL'de memory yapıları local ve shared memory area olmak üzere ikiye ayrılır.
</div><br>

#### <b> 3.1. Local Memory Area </b>
<div class='text-justify'>
Her backend processinin kendi kendi kullanımı için açılan memory alanıdır. Kullanıcıdan gelen sorgulama işlemleri sırasında kullanmak için memoryden allocate edilen alandır. Work_mem, maintenance_work_mem ve temp_buffer olmak üzere 3 temel yapıdan oluşur.</div><br>

<b>work_mem :</b> Sorgulama işlemleri sırasında join, order by, distinct gibi ifadeler için kullanılan geçici memory alanıdır.

<b>maintenance_work_mem :</b> Reindex, vacuum gibi bakım işlemleri için kullanılan alandır.

<b>temp_buffer :</b> Geçici tabloların tutulduğu bellek alanıdır.

#### <b> 3.2. Shared Memory Area </b>
<div class='text-justify'>
Tüm processler tarafından paylaşımlı olarak kullanılan memory alandır.Server başlatıldığında postmaster tarafından memoryden allocate edilen alandır. Bu alanda shared buffer pool, wal buffer ve commit log (CLOG) gibi 3 bileşenden oluşur.
</div><br>

<b>shared buffer pool :</b>Daha hızlı okuma yazma işlemi yapılması için verinin tutulduğu memory alanıdır. Veri her seferinde diskten okunup yazılmaz. Bu alanda tutulur ve değiştirilir. Daha sonra background writer devreye girerek değişen blokları data filelara yazar.

<b>wal buffer :</b>Veri kaybını önlemek için kullanılan transaction kayıt dosyalarına wal dosyaları denir. Bu kayıtların dosyalara yazılmadan önce memoryde tutulduğu alana denir. 

<b>CLOG</b>Commit log, tutarlılık kontrolü için transactionların durumunun (in progress, committed, aborted) tutulduğu alandır.