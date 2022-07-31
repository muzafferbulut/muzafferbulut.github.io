---
title: Versiyon Kontrol Sistemleri - Git ve Github
date: 2022-02-10 21:30:30 +/-TTTT
categories: [Programlama, VCS]
tags: [git, github]
---

### Giriş

<div class='text-justify'>
Yazılım geliştiricilerin ve şirketlerin vazgeçilmez araçlarından biri versiyon kontrol sistemleridir. Versiyon kontrol sistemleri bir dosya ya da dosya kümesindeki değişiklerin takip edilmesine olanak sağlar. Aynı zamanda ekip çalışmasını da mümkün kılar. Versiyon kontrol sistemleri ile çalışırken versiyonlar arası geçiş yapabilir, projeyi farklı dallara (branch diyeceğiz) ayırarak kendimize test alanları oluşturabiliriz.
</div><br>

<div class='text-justify'>
2005 yılında kendi geliştirmelerini yaparken farklı alternatifler olsa da açık kaynak kodlu bir versiyon kontrol sistemi eksikliğini hisseden Linus Torvalds Git'i geliştirmeye başlıyor. Arkasına açık kaynak kod topluluğunun gücünü de alan Git sektörün en popüler versiyon kontrol sistemlerinden biri oluyor.

Git çalışma mantığı oyunlardaki checkpoint kavramı gibidir. Bir proje geliştirirken tamamlanan bölümleri loglayabilir bu şekilde proje geliştirme safhasını adım adım ele alabiliriz. Ayırca hangi gün ne gibi değişiklikler yapıldığını görebilir gerektiği takdirde bu değişiklikleri geri alabiliriz.
</div><br>

<div class='text-justify'>
Github ise basit anlamda Git repolarımızı sakladığımız bir portal diyebiliriz. Endüstrinin çok büyük bir bölümünde bu iki sistem kullanılsa da bu iki sistemi iyi bir şekilde öğrendikten sonra diğer versiyon kontrol sistemlerine geçmekte kolay olacaktır.
</div><br>

### Git Temelleri

<div class='text-justify'>
Git ile çalışmaya başlamadan önce yeni repomuz için mkdir komutunu kullanarak yeni bir klasör oluşturalım. Bu klasör kodlarımızın ya da çalışma dosyalarımızın yer alacağı herhangi bir klasör de olabilir. Dosyalarımızdaki değişiklikleri izlemek için klasörü git’e tanıtmamız gerekecek. Peki bu işlem nasıl yapılır?</div><br>

<div class='text-justify'>Çalışmaya başlamadan önce önemli bir adım var. cd komutu ile oluşturulan klasör içine gidildikten sonra git status komutu çalıştırılır.</div><br>

<div class='text-justify'>Bu komut klasördeki güncel git durumunu gösterir. Yani halihazırda eklenen, izlenmeyen dosyalar, daha önceden izlenen dosyalara ait değişiklikler gibi çeşitli bilgileri bu komut yardımıyla edinebiliriz. Bu komut sonucunda eğer klasör bir git reposu ile bağlı değilse git init komutu ile klasörü git ile bağlayabiliriz. Böylelikle yapılan değişiklikler git tarafından izlenebilir hale gelecek. Ancak henüz izleme başlamış değil. </div><br>

<div class='text-justify'>Git init yardımıyla içinde bulunduğumuz dizin git ile bağlanır, yani bu klasörde git başlatılır. Ana branch (main ya da master) oluşturulur. Ana branch, bizim projemizdeki içimize sinen tüm değişiklikleri commitlediğimiz, test ortamlarında yazılan kodları birleştirdiğimiz branchtir. Artık bu klasör bizim repomuz oldu. Buradaki dosyalar izlenecek ve değişiklikler git aracılığıyla kodlanacak.</div><br>

<div class='text-justify'>Herhangi bir klasörde git init çalıştırmadan önce git status komutunu çalıştırarak güncel durumunu kontrol etmemiz gerekir. Git initi tekrar tekar çalıştırmak karışıklığa sebep olabilir.
Git init komutu çalıştığında mevcut klasörde .git isimli gizli bir klasör oluşur. Eğer yanlışlık bir klasörde git init komutu çalıştırılırsa bu klasörü silerek git ile bağlantısını kaldırmış oluruz.</div><br>