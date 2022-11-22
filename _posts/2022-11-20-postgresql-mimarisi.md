---
title: PostgreSQL Mimarisi
date: 2022-11-20 21:30:30 +/-TTTT
categories: [Veritabanı, PostgreSQL]
tags: [postgresql, mimari,spatial database, architecture,]
---

### 1. Giriş

<div class='text-justify'>
70'li yıllarda önerilen ilişkisel veri tabanı modelinin en güçlü açık kaynak kodlu temsilcisi olan PostgreSQL, kullanıcılarına sağladığı avantajlar sayesinde halen etkin olarak kullanılmaktadır. Güvenlik, hız, mekansal veri desteği gibi avantajlı özellikler sunan PostgreSQL lisans maliyetlerini de ortadan kaldırmıştır. Client-server mimarisinde multi-process hizmet veren bir ilişkisel veri tabanı yönetim sistemidir. Aynı zamanda mekansal veri desteği sunan ilk ilişkisel veri tabanı yönetim sistemlerinden biridir.
</div><br>

<div class='text-justify'>
PostgreSQL mimarisi memory, process ve disk üzerindeki dosyalardan oluşur. Process'ler postgres server, backend, background, replication associated process olmak üzere 4 ana başlıkta incelenebilir.
</div><br>

#### 1.1. Postgres Server Process
<div class='text-justify'>
Postgresql server üzerinde çalışan, server çalıştığında ilk ayağa kalkan ve diğer process'lerin bağlı olduğu process'tir. Sistem başlatıldığında shared memory'den postgresql server'in kullanacağı ram alanı allocate edilir ve background process'ler başlatılır. Varsa replication processler çalıştırılır ve sunucu kullanıcıyı dinlemeye başlar. Kullanıcıdan bir talep geldiği zaman bir backend process başlatılır ve kullanıcıdan gelen tüm talepleri de bu backend process yakalar.
</div><br>

#### 1.2. Backend Process
<div class='text-justify'>
Postgres olarakta adlandırılan backend process client tarafından gönderilen sorgu ve statement'ları yakalayan process'tir. Kullanıcı bir SQL çalıştırmak için sisteme bağlandığında postgres server process (postmaster) tarafından yeni bir backend process (postgres) fork edilir ve kullanıcıdan gelen talepler dinlenir. Kullanıcı bağlantısını kestiği zaman postgres terminate edilir. Eğer kısa zamanda sık sık kullanıcı bağlanıp bağlantıyı kesiyorsa (web sitesi hizmetleri gibi)  bağlantı isteklerine karşılık vermek için başlatılan backend process maliyetleri artıracaktır. Bunun için Postgresql orta katmanında pg_browser gibi uygulamalar kullanılarak bu işlemler gerçekleştirilir. Kullanıcı gelir bağlanır işi bitince disconnect olur ve connection havuzuna bırakır. Bu sebeple backend process sürekli aç kapa olmayacağı için sunucu rahatlar.
</div><br>

#### 1.3. Backgorund Process
<div class='text-justify'>
Background process'ler sunucuda arka planda çalışan yönetimsel işleri gerçekleştiren process'lerdir. ps -ef komutu ile işletim sistemi üzerinde çalışan processleri, ps -ef | grep postgres komutu ile postgresle ilgli çalışan background processleri görüntüleyebiliriz. Background process'ler logger, checkpointer, background writer, walwriter, autovacuum launcher, stats collector olarak sıralanabilir.
</div><br>

<b>Background Writer :</b> Shared buffer pool’da bulunan dirty blokların (değiştirilmiş bloklar) memory'den diske yazılması işlemini gerçekleştirir. <br>

<b>Checkpointer :</b> Checkpoint işleminin gerçekleşmesinden sorumlu process'tir.<br>

<b>Autovacuum Launcher :</b> Vacuum process'inin otomatik olarak belirli zamanlarda başlatılmasından sorumlu process'tir.<br>

<b>Wal Writer :</b> Wal bufferda bulunan veriyi wal dosyalarına kalıcı olarak yazmakla sorumludur. Wal dosyaları transaction bilgilerini tutan log dosyalardır. Sistemde ani kapanma durumunda veri kaybı oluşmasını önlemek ve veritabanını recover etmek için kullanılan dosyalardır.<br>

<b>Statistic Collector :</b> Verittabanı istatistiklerinin toplanmasını sağlayan processtir.<br>

<b>Logger :</b> Error mesajlarını log dosyalarına yazan processtir.<br>

<b>Archiving :</b> Arşivleme işlemini gerçekleştiren processtir.

#### 1.4. Replication Associated Process 
<div class='text-justify'>
Streming işleminden sorumlu processtir. Replikasyon işlemi sırasında veri transfrer işlemini sağlayan processtir.
</div><br>

### 2. PostgreSQL Memory Yapıları
<div class='text-justify'>
PostgreSQL'de memory yapıları local ve shared memory area olmak üzere ikiye ayrılır.
</div><br>

#### 2.1. Local Memory Area
<div class='text-justify'>
Her backend processinin kendi kendi kullanımı için açılan memory alanıdır. Kullanıcıdan gelen sorgulama işlemleri sırasında kullanmak için memoryden allocate edilen alandır. Work_mem, maintenance_work_mem ve temp_buffer olmak üzere 3 temel yapıdan oluşur.</div><br>

<b>work_mem :</b> sorgulama işlemleri sırasında join, order by, distinct gibi ifadeler için kullanılan geçici memory alanıdır.

<b>maintenance_work_mem :</b> reindex, vacuum gibi bakım işlemleri için kullanılan alandır.

<b>temp_buffer :</b> geçici tabloların tutulduğu bellek alanıdır.

* #### 2.2. Shared Memory Area
<div class='text-justify'>
Tüm processler tarafından paylaşımlı olarak kullanılan memory alandır. PostgreSQL server başlatıldığında memoryden allocate edilen alandır. Postgres server process tarafından allocate edilir. Bu alanda shared buffer pool, wal buffer ve commit log (CLOG) gibi 3 bileşenden oluşur.
</div><br>

<b>shared buffer pool :</b>Daha hızlı okuma yazma işlemi yapılması için verinin tutulduğu memory alanıdır. Veri her seferinde diskten okunup yazılmaz. Bu alanda tutulur ve değiştirilir. Daha sonra background writer devreye girerek değişen blokları data filelara yazar.

<b>wal buffer :</b>Veri kaybını önlemek için kullanılan transaction kayıt dosyalarına wal dosyaları denir. Bu kayıtların dosyalara yazılmadan önce memoryde tutulduğu alana denir. 

<b>CLOG</b>Commit log, tutarlılık kontrolü için transactionların durumunun tutulduğu alandır. Transactionlar inprogress, commit ya da abort durumda olabilir. 