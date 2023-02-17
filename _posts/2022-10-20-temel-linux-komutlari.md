---
title: Temel Linux Komutları
date: 2022-10-20 21:30:30 +/-TTTT
categories: [Not Defteri, Linux]
tags: [linux, os, operating system]
---

<div class='text-justify'>
Bilgisayarlarımız çalışmak için işletim sistemlerine ihtiyaç duyar. Günümüzde en çok kullanılan işletim sistemleri Windows, MacOS, Linux ve Android işletim sistemleridir. Bu işletim sistemlerinden Linux'un masaüstü bilgisayarlarda pazar payı %2.5 olsa da bulut ve hosting hizmetlerinde bu oran %90 üzerine çıkmaktadır. Dolayısıyla temel linux komutlarını bilmek önem arz etmektedir.<br><br>

Linux 1991 yılında Linus Torvalds tarafından geliştirilen, en çok bilinen ve en çok kullanılan açık kaynak kodlu işletim sistemidir. Linux, açık kaynak kodlu olduğu için, kullanıcılar özgürce kaynak kodunu indirip değiştirme, dağıtma ve paylaşma gibi aktiviteler yapabilirler. Bu nedenle, Linux, dünya çapında birçok kullanıcının ve geliştiricinin kullanması ve katkıda bulunmasıyla hızla gelişir ve güncellenir. Linux, birçok farklı cihazda kullanılabilecek şekilde özelleştirilebilir ve ölçeklenebilir bir işletim sistemi olarak kabul edilir. </div>

### <b> cat : </b>

<div class='text-justify'>
cat (concatenate) komutu temel olarak bir dosyasının içeriğini görüntülemek için kullanılır. Ayrıca dosya içeriğini filtrelemek ve yeniden yönlendirmek için de kullanılabilir.<br>

```
-- dosyanın içeriğini görüntülemek
$ cat file.txt

-- dosyaları birleştirmek
$ cat file1.txt file2.txt > new_file.txt

```
</div>

### <b> cd : </b>

<div class='text-justify'>
cd (change directory) komutu linux ve unix tabanlı işletim sistemlerinde farklı dizinler arasında geçiş yapmaya olanak tanıyan komuttur. <br>

```
-- dizin değiştirme
$ cd hedefdizin

-- bir üst dizine gitme
$ cd ..

-- ana dizine dönme
$ cd

```
</div>

### <b> chmod : </b>

<div class='text-justify'>
chmod (change mod) komutu dosya veya dizinlerin erişim haklarını değiştirmeye olanak tanıyan komuttur. Dosyalar ve dizinler, üç farklı kullanıcı sınıfı tarafından erişilebilir: kullanıcı (owner), grup ve diğer kullanıcılar. "chmod" komutu, bu kullanıcı sınıflarının erişim haklarını değiştirir. Erişim hakları, okuma (r), yazma (w) ve çalıştırma (x) şeklinde belirtilir. chmod komutu, dosya veya dizinlerin erişim haklarını belirlemek için 3 farklı sayı kullanır: 4, 2 ve 1. Bu sayılar, sırasıyla okuma (r), yazma (w) ve çalıştırma (x) haklarını temsil eder. Bu sayılar farklı kombinasyonlar halinde kullanılarak dosyaların ve dizinlerin erişim hakları belirlenir. Örneğin, 7 (rwx) tüm erişim haklarını temsil ederken, 0 (---) tüm erişim haklarını kapatır. <br>

```
-- Erişim haklarını belirli bir sayısal değere değiştirmek
$ chmod 755 file.txt

```
</div>

### <b> chown : </b>

<div class='text-justify'>
chown (change owner) komutu dosya ve dizinlerin sahiplerini değiştirmeye olanak tanıyan komuttur. Linux tabanlı işletim sistemlerinde bir dosyanın sahibi o dosyayı oluşturan kullanıcıdır. chown komutu dosya ve dizinlerin sahiplik bilgilerini deiştirmek için yaygın olarak kulanılan komuttur. <br>

```
-- dosya veya dizinin sahibini değiştirmek
$ chown new_user file.txt

-- tüm alt dizinler ve dosyaların sahiplik bilgisini değiştirmek
$ chown -R new_user:new_group_folder/
```
</div>

### <b> cp : </b>

<div class='text-justify'>
cp (copy) komutu dosya ve dizinlerin kopyalanmasında kullanılır. <br>

```
-- bir dosyayı başka bir dosyaya kopyalamak
$ cp file1.txt file2.txt

-- bir dosyayı başka bir dizine kopyalamak
$ cp file.text directory/

-- tüm dosya ve dizinleri bir dizine koyalamak
$ cp -R source/ target/
```
</div>

### <b> df: </b>

<div class='text-justify'>
df (disk free) komutu dosya sisteminin durumu hakkında (özellikle disk kullanımı hakkında) bilgi verir. Komut sayesinde mevcut disk alanının kullanılan ve boş olan kısmını hangi dosya sistemlerinin daha fazla kaynak kullandığı görüntülenebilir. <br>

```
-- okunabilir çıktı için
$ df -h
```
</div>

### <b> diff : </b>

<div class='text-justify'>
diff (difference) komutu iki dosya ve dizin arasındaki farklılıkları gösteren komuttur. Bu komut ile değişen dosya ve satırlar ayıklanarak başka bir dosyaya yazılabilir. <br>

```
-- iki dosya arasındaki fark
$ diff file1.txt file2.txt

-- iki dizin arasındaki fark
$ diff -r dir1/ dir2/

-- farkları bir dosyaya kaydetmek
$ diff file1.txt file2.txt > difference.txt
```
</div>

### <b> du : </b>

<div class='text-justify'>
du (disk usage) komutu parametre olarak aldığı dosya sisteminin disk kullanımı hakkında bilgi verir. Parametre olarak bir dizin almazsa mevcut dizinin disk kullanımı hakknında bilgi verir. Bu bilgiler disk kullanımını etkin bir şekilde yönetmek için yardımcı olur.<br>

```
-- belirli bir dizinin disk kullanımını görüntülemek
$ du /home/user/directory

--okunaklı çıktı almak
$ du -h
```
</div>

### <b> echo : </b>

<div class='text-justify'>
echo komutu terminal ekranına değişken veya değerleri yazdırmak için kullanılan komuttur. <br>

```
-- bir metin yazdırmak
$ echo "Hello world!"

-- bir değişken değeri yazdırmak
$ echo $KULLANICI_ADI
```
</div>

### <b> find : </b>

<div class='text-justify'>
find komutu belirli bir dizin veya alt dizinlerindeki dosya ve dizinleri aramak için kullanılır. Eğer dosya yolu belirtilmezse işlemler mevcut dizinde gerçekleştirilir. <br>

```
-- belirli bir dizinde belirli bir dosya adı ile arama yapmak
$ find /home/user/directory -name filename

-- boyut kriterli arama yapmak
$ find /home/user/directory size +1M

-- tarih kriterli arama yapmak
$ find /home/user/directory -mtime -7
```
</div>

### <b> grep : </b>

<div class='text-justify'>
grep komutu bir metin dosyası ve çıktı akışındaki belirli bir desene göre eşleşen satırları bulmak için kullanılır. <br>

```
-- belirli bir dosyada belirli bir kelimeyi aramak
$ grep kelime dosya

-- belirli bir dosyada belirli bir desen için düzenli ifade kullanmak
$ grep 'desen' dosya

-- bir dizindeki tüm dosyalarda belirli bir kelimeyi aramak
$ grep -r kelime directory/
```
</div>

### <b> head : </b>

<div class='text-justify'>
head komutu bir dosyanın ilk birkaç satırını yazdırmak için kullanılır. <br>

```
-- belirli bir dosyanın ilk 10 satırını yazdırmak
$ head file.txt

-- belirli bir dosyanın ilk N satırını yazdırmak
$ head -n N file.txt
```
</div>

### <b> history : </b>

<div class='text-justify'>
history komutu ile sisteme daha önce girilen komutlar listelenebilir. Daha önce girilen bir komutu tekrar çalıştırmak için ! işareti kullanmak yeterlidir. <br>

```
-- daha önce girilen son 100 komutu görüntülemek için
$ history 100

-- geçmişte girilen komutların listesini bir dosyaya kaydetmek için 
$history > dosya_adı

-- geçmişte girilen komutlarda belirli bir kelimeyi aramak için
$ history | grep kelime
```
</div>

### <b> hostname : </b>

<div class='text-justify'>
hostname komutu sistemdeki bilgisayar adını (host adını) görüntülemek ve değiştirmek için kullanılır. hostname komutu kullanıldığında, sistemdeki bilgisayar adı (host adı) ekrana yazdırılır. Varsayılan olarak, bu ad, bilgisayarın ilk kurulum sırasında belirlenen isimdir. <br>
 
```
-- sistemdeki bilgisayar adını görüntülemek için
$ hostname

-- sistemdeki bilgisayar adını değiştirmek için 
$ hostname yeni_bilgisayar_adı
```
</div>

### <b> jobs : </b>

<div class='text-justify'>
jobs komutu arka planda çalışan işlemleri görüntülemek ve yönetmek için kullanılır. Jobs komutu ile sistemde çalışan işlemler ve işlemlere ilişkin bilgiler ekranda listelenir. <br>

```
-- arka planda çalışan işlemleri listelemek için
$ jobs

-- bir işlemi durdurmak için 
$ kill %<işlem numarası>

--bir işlemi devam ettirmek için: 
$ fg %<işlem numarası>

-- bir işlemi arka planda devam ettirmek için 
$ bg %<işlem numarası>
 
```
</div>

### <b> kill : </b>

<div class='text-justify'>
kill komutu belirli bir işlemi sonlandırmak veya sinyal göndermek için kullanılır. kill komutu ile bir işlem PID (Process ID) numarası veya işlem adı kullanılarak sonlandırılabilir. Bu komut, özellikle hata veren veya donanım hatası olan işlemleri sonlandırmak için kullanışlıdır. 

```
$ kill [options] PID
```
</div>

### <b> locate : </b>

<div class='text-justify'>
locate komutu belirli bir dosya veya dizinin konumunu hızlıca bulmak için kullanılır. <br>

```
$ locate [options] filename
```
Sıklıkla kullanılan seçenekleri şunlardır:<br>

 * "-i" :  Büyük-küçük harf duyarlılığını kapatır.
 * "-l" : Arama sonuçlarının sayısını sınırlandırır.
 * "-n" : Arama yapmak için kullanılacak veritabanını belirler.

</div>

### <b> ls : </b>

<div class='text-justify'>
ls komutu bulunduğunuz dizindeki dosya ve dizinleri listelemek için kullanılır. Kullanımı aşağıdaki gibidir. Burada "options", komut için seçeneklerdir ve isteğe bağlıdır. "file" ise listelemek istediğiniz dosya veya dizinin adıdır. <br>

```
-- ls komutu temel kullanımı
$ ls [options] [file]

-- tüm dosya ve dizinleri listeleme
$ ls

-- tüm dosya ve dizinleri ayrıntılı listeleme
$ ls -l

-- gizli dosyaları da listeler
$ ls -a
```
</div>

### <b> man : </b>

<div class='text-justify'>
man (manual) komutu kullanıcılara işletim sistemi hakkında detaylı bilgi verir. Kullanımı aşağıdaki gibidir. <br>

```
$ man [section] command
```

Burada "command", öğrenmek istediğiniz komut veya konunun adıdır. "section" ise, komut veya konunun belgelendirme bölümünü belirtir. Örneğin, "section 1" komutlar için, "section 5" dosya biçimleri için, "section 8" sistem yönetimi için kullanılır. "Section" belirtilmezse, varsayılan olarak "section 1" kullanılır. Örneğin, "man ls" komutu "ls" komutu hakkında ayrıntılı belgelendirme sağlar. Ayrıca, "man 5 passwd" komutu, "passwd" dosya biçimi hakkında belgelendirme sağlar.
</div>

### <b> mkdir : </b>

<div class='text-justify'>
mkdir (make directory) komutu kullanıcıların dizin veya klasör oluşturmasına olanak tanır. 

```
-- doc dizini oluşturma
$ mkdir doc

-- doc dizini altına project dizini oluşturma
$ mkdir doc/project

-- iki işlemi tek adımda yapma
$ mkdir -p doc/project
```
</div>

### <b> mv : </b>

<div class='text-justify'>
mv (move) komutu dosyaları veya dizinleri taşımak veya yeniden adlandırmak için kullanılır. <br>

```
-- bir dosyayı başka bir dizine taşıma
$ mv file.txt Documents/
```
</div>

### <b> ping : </b>

<div class='text-justify'>
ping komutu bilgisayar ağları üzerindeki diğer cihazların bağlantısını test etmek için kullanılan bir araçtır. Bu komut, bir IP adresi veya bir ana bilgisayar adı belirtilerek kullanılır ve ağdaki bir cihaza bir paket gönderir ve o cihazdan bir yanıt alınmasını bekler. <br>

```
$ ping [options] host
```
</div>

### <b> pwd : </b>

<div class='text-justify'>
pwd (print working directory) komutu, çalışılan dizinin tam yolunu ekrana yazdırmak için kullanılan bir komuttur. Özellikle uzun ve karmaşık bir dizin yapısı içinde çalışırken, mevcut dizinin tam yolunu hızlı bir şekilde görüntülemek için kullanışlı bir araçtır.
</div>

### <b> rm : </b>

<div class='text-justify'>
rm (remove) komutu, dosya veya dizinleri silmek için kullanılır. <br>

```
-- bir dosya silmek
$ rm myFile.txt

-- bir dizin silmek
$ rm -r directory
```
</div>

### <b> rmdir : </b>

<div class='text-justify'>
rmdir (remove directory) komutu dizinleri silmek için kullanılır. Bu komut sadece boş dizinleri silebilir. Eğer dizin içinde dosya veya alt dizin varsa, "rmdir" komutu çalışmayacaktır.<br>

```
$ rmdir directory
```
</div>

### <b> tail : </b>

<div class='text-justify'>
tail komutu bir dosyanın son kısmını görüntülemek için kullanılan bir komuttur. Bu komutla, bir dosyanın en son satırları veya belirli bir sayıda son satırı görüntülenebilir. Genelde büyük dosyaların sonunda meydana gelen hataları veya değişiklikleri izlemek için kullanılır. <br>

```
$ tail [options] filename
```
</div>

### <b> tar : </b>

<div class='text-justify'>
tar (Tape Archive) komutu bir arşivleme aracıdır. Bu komut, birden çok dosya veya dizini tek bir arşiv dosyasında toplamanıza ve sıkıştırmanıza olanak tanır. Bazı yaygın "tar" komutu kullanımları şunlardır:<br>

```
-- Arşiv dosyası oluşturma
$ tar -cvf archive.tar file1 file2 dir1

-- Arşiv dosyasından dosyaların geri çekilmesi
$ tar -xvf archive.tar

-- Arşiv dosyasının sıkıştırılması (gzip)
$ tar -czvf archive.tar.gz file1 file2 dir1

-- Sıkıştırılmış bir arşiv dosyasından dosyaların geri çekilmesi
$ tar -xzvf archive.tar.gz
```
</div>

### <b> top : </b>

<div class='text-justify'>
top komutu sistem performansı ve kaynak kullanımı hakkında gerçek zamanlı bilgi sağlayan bir araçtır. Bu komut, işlemci kullanımı, bellek kullanımı, disk girdi/çıktısı kullanımı, işlem öncelikleri, CPU sürelerini ve diğer birçok sistem ölçütlerini izleyebilir. Ayrıca sistem kaynaklarının kullanımını öncelik sırasına göre sıralayarak hangi işlemlerin sistemde en fazla kaynak kullandığını da görüntüleyebilir.
</div>

### <b> touch : </b>

<div class='text-justify'>
touch komutu var olan dosyaların erişim veya değiştirilme zamanlarını güncellemek veya var olmayan dosyaları oluşturmak için kullanılan bir komuttur.

```
$ touch [option(s)] file_name(s)
```
</div>

### <b> unzip : </b>

<div class='text-justify'>
unzip komutu sıkıştırılmış ZIP dosyalarını açmak için kullanılan bir komuttur.

```
$ unzip -q data.zip
```
</div>

### <b> zip : </b>

<div class='text-justify'>
zip komutu bir veya daha fazla dosyayı sıkıştırmak veya arşivlemek için kullanılan bir Linux komutudur. 

```
$ zip [options] archive file(s)...
```

</div>