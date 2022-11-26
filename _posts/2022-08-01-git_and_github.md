---
title: Versiyon Kontrol Sistemleri - Git ve Github
date: 2022-08-01 21:30:30 +/-TTTT
categories: [Tutorial, VCS]
tags: [git, github]
---

### Giriş

<div class='text-justify'>
Yazılım geliştiricilerin ve şirketlerin vazgeçilmez araçlarından biri versiyon kontrol sistemleridir. Versiyon kontrol sistemleri bir dosya ya da dosya kümesindeki değişiklerin takip edilmesine olanak sağlar. Aynı zamanda ekip çalışmasını da mümkün kılar. Versiyon kontrol sistemleri ile çalışırken versiyonlar arası geçiş yapabilir, projeyi farklı dallara (branch diyeceğiz) ayırarak kendimize test alanları oluşturabiliriz.
</div><br>

<div class='text-justify'>
2005 yılında kendi geliştirmelerini yaparken farklı alternatifler olsa da açık kaynak kodlu bir versiyon kontrol sistemi eksikliğini hisseden Linus Torvalds Git'i geliştirmeye başladı. Arkasına açık kaynak kod topluluğunun gücünü de alan Git, sektörün en popüler versiyon kontrol sistemlerinden biri oldu.
Git çalışma mantığı oyunlardaki 'checkpoint' kavramı gibidir. Bir proje geliştirirken tamamlanan bölümleri loglayabilir bu şekilde proje geliştirme safhasını adım adım ele alabiliriz. Ayrıca hangi gün ne gibi değişiklikler yapıldığını görebilir gerektiği takdirde bu değişiklikleri geri alabiliriz.
</div><br>

<div class='text-justify'>
Çoğunlukla git ile karıştırılan github ise basit anlamda git repolarımızı sakladığımız bir portal diyebiliriz. Endüstrinin çok büyük bir bölümünde bu iki sistem kullanılsa da bu iki sistemi iyi bir şekilde öğrendikten sonra diğer versiyon kontrol sistemlerine geçmekte kolay olacaktır.
</div><br>

### Git Temelleri

<div class='text-justify'>
Git ile proje dizinindeki değişiklikler izleneceğinden ilgili proje için yeni bir klasör oluşturulur. Oluşturulan klasördeki değişikliklerin algılanıp takip edilebilmesi için klasörün Git'e tanıtılması gerekmektedir.
</div><br>

<div class='text-justify'>Bu noktada iki önemli komut (status ve init) karşımıza çıkar. Status komutu ile mevcut repodaki durumu (izlenen ve izlenmeyen dosyalar, değişiklikler gibi) çıktı olarak alabiliriz. Ancak klasör bir git reposu olmayabilir. Bu durumda init komutu ile klasörü Git'e tanıtarak bir git reposu haline getirebiliriz. Ancak henüz klasördeki değişiklikler izlenir durumda değildir. </div><br>

```
$ git status
-- klasördeki mevcut durum

$ git init
-- klasörü git reposu haline getirir.
```

<div class='text-justify'>Herhangi bir klasörde init komutu çalıştırmadan önce status komutunu çalıştırarak güncel durumunu kontrol etmemiz gerekir. Git initi tekrar tekrar çalıştırmak karışıklığa sebep olabilir. Git init komutu çalıştığında mevcut klasörde .git isimli gizli bir klasör oluşur. Eğer yanlışlıkla bir klasörde git init komutu çalıştırılırsa bu klasörü silerek git ile bağlantısını kaldırmış oluruz. </div><br>

### Git Branch

<div class='text-justify'>Takım çalışmasını tesis etmek ve yönetimi kolaylaştırmak için proje branch'lere bölünebilir. Yani bir proje tek bir branch'ten oluşabileceği gibi birden fazla branch'ten de oluşabilir. Farklı branchlerdeki değişiklikler optimum şekilde çalışmaya başladığında ana branch ile birleştirilebilir. </div><br>

```
-- projeye yeni branch eklemek
$ git branch branchname
```

### İlk Commit

<div class='text-justify'>Git'e tanıttığımız klasöre yeni dosyalar eklemeye başladığımızda 'git status' komutunun çıktıları değişecektir. Bu dosyalar yeni eklendiği ve sisteme 'izle' komutu verilmediği için untracked olarak kabul edilir. Dosyalardaki değişikleri ya da klasörün tamamındaki değişiklikleri takip etmek için aşağıdaki komutlar çalıştırılır.</div><br>

```
$ git add dosyaadi 
-- verilen dosyayı indeksler

$ git add .                
-- tüm dosyaları indeksler
```

<div class='text-justify'>Bu komutlardan sonra sistem dosyaları izlemeye başladı ancak henüz commit edilmedi, sadece indekslendi. Aşağıdaki komutla değişikliklerimizi commitleyerek değişiklikleri onaylayabiliriz. Commit mesajlarının kısa ve açıklayıcı olmasına özen gösterilmelidir.
Commitlenen dosyalar, commit zamanı, commit işlemini yapan kişiye ait bilgiler loglanır. Bu loglamaya ilerleyen dönemlerde ihtiyaç halinde ulaşılabilir. git log komutu ile commitlere ait loglara erişebiliriz. Eğer tek bir satırda hem git add hem de commit işlemi yapmak istersek commit yaparken -a argümanını kullanabiliriz.</div><br>

```
-- değişikliklerin commitlenmesi
$ git commit -m “commit mesajı”

-- loglara ulaşmak
$ git log

-- tek satırda git add ve git commit komutu
$ git commit -a "commit mesajı"
```

### Gitignore

<div class='text-justify'>Git ile izlenen klasördeki tüm dosyalar indekslenip github'a yüklenebilir. Ancak bazı özel dosyaları bu durumdan muaf tutmak gerekebilir. Bunu sağlamak için gitignore dosyası oluşturulur. Çalışılan klasörde oluşturulacak ve içine özel dosyalar yazılan .gitignore dosyası ile özel bilgi ve belgeler indekslenmeyecek, repoya eklenmeyecektir. Github’da gitignore reposu altında programlama dillerine özel gitignore şablonları vardır. Projeyi internete atmadan önce bu listeler incelenerek faydalanılabilir.
</div><br>

### Head

<div class='text-justify'>Bir repodaki ana branch projenin hazır halidir. Yani tamamlanan değişiklikler ana branchte tutulur. HEAD ise projede aktif olarak hangi branch ve hangi commitle olduğumuzu işaretleyen bir ifadedir. Yeni branchler açıldıkça ve yeni commitler geldikçe HEAD ifadesinin işaretlediği yer de değişir. 
</div><br>

### Merge

<div class='text-justify'>Proje ana branchi master branchtir. Daha sonra test ortamı ya da yeni özellikler eklemek için yeni bir branch açıp oradaki çalışmalar tamamlandıktan sonra bu iki branchi (daha fazla da olabilir) birleştirme ihtiyacı doğabilir. Bu durumda HEAD yeni açılan branchi işaretleyecektir. Tekrar master branche dönmek için switch komutu çalıştırılmalıdır. Bu iki branchte yapılan değişiklikleri birleştirmek için merge komutu kullanılır. Aşağıdaki komut ile master branchin üzerine sonradan açılan branch eklenir ve birleştirme işlemi tamamlanır. git branch komutu çalıştırıldığında güncel branch listesi ekrana basılır.</div><br>

```
-- Merge komutu
$ git merge branchname "commit mesajı"

-- Branch değiştirme
$ git switch branchname

-- Branch listesi
$ git branch
```

### Fast Forward

<div class='text-justify'>Yeni bir branch açarak çalışıldığında değişiklikler aynı noktalarda yapılabilir. Bu durumda merge işlemi aşamasında conflict problemi ile karşılaşabiliriz. Bu durum genellikle hem master hem de yan branchlerde değişiklik yapıldığında karşılaşılabilir. Bunun önüne geçmek için masterda hiç değişiklik yapmadan tüm değişikliklerin yan branchte yapılıp daha sonradan birleştirme yoluna gidilebilir. Bu işleme fast forwarding denir.</div><br>

### Merge Conflict

<div class='text-justify'>Farklı branchlerde yapılan ve birbirini etkileyen işlemler merge conflict hatasına sebep olabilir. Bu durumlarda merge işlemi otomatik olarak gerçekleştirilmez. Meydana gelen çakışmaların çözülmesi ve ondan sonra merge işleminin yapılması gereklidir. Kalacak ve gidecek dosyalar tespit edilir ve daha sonra merge ve commit işlemi tekrar yapılarak bu sorun da çözülmüş olur.</div><br>

### Stash Kavramı

<div class='text-justify'>Bir branchte yapılan değişikliklerin commitlenememesi ve branchin değişmesi durumunda farklı problemler ortaya çıkabilir. Proje, restore komutu ile son commit durumuna geri döndürülebilir ancak bu da veri kaybı demektir. Proje commitlemeye müsaitse commit edilmeli aksi durumda restore ve stash kavramları değerlendirilmelidir..</div><br>

```
-- son commite dönme
$ git restore filename
```

<div class='text-justify'>Stash yaparak değişiklikleri commitlemeden de saklayıp daha sonradan dönebiliriz. Değişiklik yapıldıktan sonra commitleyecek durumda değilsek stash komutu ile yapılan değişiklikler saklanır. Değişiklikleri sakladığımız yerden almak için stash pop komutu çalıştırılır. Gerekli düzenlemeler tamamlandıktan sonra commitlenebilir.</div><br>

<div class='text-justify'>git stash list komutu ile güncel stashleri listeleriz. Birden fazla stash varsa git stash apply stash@{0} komutu ile stash listesinden istediğimiz stashi geri getirebiliriz. Bu şekilde istediğimiz stashi istediğimiz yere commitleyebiliriz. Git stash clear komutu ile stash listesini temizleyebiliriz. git stash apply komutu ile de mevcut stash’in tuttuğu değişiklikler geri getirilir ama stash listesinde de kalmaya devam eder. Pop ile geri getirilen değişiklikler stash listesinden çıkarılır.</div><br>

### Geçmişe Dönme

<div class='text-justify'>Commit atmadığımız değişiklikler için git restore komutu ile değişikliklerden vazgeçebiliriz. Ama commitledikten sonra tekrar geri dönmek gerekirse checkout komutu kullanılır..</div><br>

```
$ git checkout commitID
```

<div class='text-justify'>Ancak bu noktada bir problem ortaya çıkar. HEAD kısmı güncel commiti gösterir ama master son commiti gördüğü için HEAD’i görmez. Bu hatadan kaçınmak için git switch master ile master branche geri dönebiliriz. Ama bu bizi geri döndürür yani değişiklik yapmamış oluruz.</div><br>

<div class='text-justify'>Peki aynı branchten devam etmek için ne yapmalıyız? git reset commitID ile verilen commit ID’den sonraki commitler silinir ancak o commitlerin değişiklikleri kalır. git reset --hard commitID komutu ile verilen commitID’den sonraki commitleri ve yapılan değişiklerin hepsini siler. Ancak dikkatli olunmalıdır, özellikle birden fazla kişi ile çalışırken conflicte sebep olabilir. Son iki commiti sileyim ama yeni bir commit oluşturayım bir de masterdan devam edeyim dersek o zaman revert komutunu kullanacağız. git revert commitID şekliyle yaralanıyoruz. Ancak bunları yaparken dikkat etmeliyiz çünkü conflict meydana gelebilir. </div><br>

### Git Diff

<div class='text-justify'>Git diff komutu ile belli başlı yerler arasındaki farkları gösterir. Bunlar dosyalar ve kodlar arasındaki farklar olabilir. Yapılan değişiklikleri, silinenleri ve eklenenleri gösterir. Git diff tek başına çalıştırıldığında sadece güncel dosya içerisinde yapılan değişikleri gösterir. İndekslenen dosyadaki değişiklikleri göstermez. Ama biz herhangi bir şey için de değişiklikleri de görebiliriz. Mesela git diff HEAD çalıştırıp son commite göre neleri değiştirdiğimizi de görebiliriz (Bunun sebebi HEAD’in son commite işaret etmesi). Commitler arasındaki farkı görmek için ise git diff commit1 commit2 yazarsak iki commit arasındaki farkı gösterir.
İki commit arasındaki boşluk açısından bir problem olursa iki nokta koyabiliriz. Aynı şekilde iki branch arasındaki değişiklikleri de görebiliriz. </div><br>

### Rebase

<div class='text-justify'>Git’in en çok korkulan konularından biridir çünkü yanlış kullanılırsa sorunlara sebep olabilir. Ana proje master branchinde tutuluyor olsun. Başka bir branch ile çalışmalarımıza devam edip tamamladıktan sonra merge edebiliriz. Ve bu durum devam ederse bir çok merge commit oluşur. git switch feat ve ardından git rebase master  komutları ile rebase yapılabilir ve bu şekilde commitler sıralanarak loglanabilir. 
Bu şekilde bir uygulamada tarih değiştiği için özellikle takım çalışmalarında problem yaratabilir. Bireysel çalışmalarda kullanmaya daha uygundur.
Rebase ile merge commitleri temizleyip önce master commitleri sonra yeni branch commitleri sıralanır ve loglar temizlenmiş olur.</div><br>

### Github

<div class='text-justify'>Git popüler bir versiyon kontrol sistemidir ve faydaları saymakla bitmez. Git’in yol arkadaşı local repolarımızı online ortama taşıyabildiğimiz ücretli ve ücretsiz versiyonları bulunan Github’dır. Github ile projelerimizi public ya da private olarak saklayabilir, takım çalışmalarına katılabilir ve başka projelere katkıda bulunabiliriz. Bu işlemleri yapmak için de clone ve fork kavramlarını bilmek gereklidir. </div><br>

### İlk Repository

<div class='text-justify'>Github internet sitesinde yeni bir repo oluşturulur. Oluşturulan repoya ait bilgiler açılış ekranında verilir. </div><br>
  
```
-- remote repo ekleme
$ git remote add origin repoUrl

-- uzak repoya değişiklikleri gönderme
$ git push -u origin main 

-- uzak repodaki branchleri görüntüleme
$ git branch -r

-- uzak repodaki branche geçme
$ git checkout origin/main
```

<div class='text-justify'>Özellikle farklı bilgisayarlar ile çalışırken repoların senkronize olmama durumu ile karşılaşabiliriz. Bu durumda repoları senkronize etmek için fetch ve pull komutları kullanılır. </div><br>
  
```
-- origin linkindeki master branchı getirme
$ git fetch origin master
```
<div class='text-justify'>Fetch komutu dosyaları bize getirir ama bizim dosyalarımızı değiştirmez. Bize değişiklikleri inceleme imkanı sunar. İncelemeden sonra bir problem olmadığı anlaşılırsa pull komutu ile dosyalardaki değişiklikleri onaylayabiliriz.</div><br>
  
```
-- origin linkindeki değişiklikleri onaylama
$ git pull origin master
```

### Clone ve Fork

<div class='text-justify'> Github ile paylaşılmış bir repoyu siteden indirebileceğimiz gibi, git ile de bir repo dosyası olarak indirebiliriz.</div><br>
  
```
-- Clone
$ git clone repoUrl
```
  
<div class='text-justify'> Bu komut verilen adresteki dosyaları mevcut dizine indirecektir. Bu dosyalardan çalışmaya devam edebiliriz. Ancak uzaktaki bir projeye katkı sağlama düşüncesi varsa yani bu dosyalardaki değişiklikleri proje sahibine göndererek çalışmaya katkı vermeyi amaçlıyorsak o zaman projeyi forklamak gereklidir. Projeyi forkladıktan sonra local repo oluşturarak klonlayıp kendi değişikliklerimizi yapabilir incelemesi için tekrar proje sahibine (pull requests açmak) gönderebiliriz. </div><br>

#### NOT:

<div class='text-justify'>Bu yazının hazırlanmasında Atıl Samancıoğlu hocamın BTK Akademi platformundaki Versiyon Kontrol Sistemleri kursundan yararlanılmıştır. Kendisine teşekkür ederim. </div><br>
