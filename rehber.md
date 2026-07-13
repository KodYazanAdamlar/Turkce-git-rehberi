# Türkçe Git Kullanım Rehberi

## 1. Giriş

Her şeyden önce "Git nedir?" sorusu ile başlayalım. Burada biraz tarih dersine giriyoruz. Git; ilk olarak 2005 yılında [Linus Torvalds](https://tr.wikipedia.org/wiki/Linus_Torvalds) tarafından geliştirilen, Linux çekirdeğinin geliştirme sürecini hızlı ve güvenli bir hale getirmek için ortaya çıkarılmış bir versiyon kontrol sistemiydi. Daha sonra projenin ana bakım ve geliştirme sorumluluğunu [Junio C. Hamano](https://github.com/gitster) üstlenmiştir.

### 1.1 Versiyon Kontrol Sistemi

Peki nedir bu versiyon kontrol sistemi? Versiyon Kontrol Sistemi (VCS), kısaca bir proje içindeki dosyaların **zaman içinde nasıl değiştiğini kaydeden ve gerektiğinde eski hallerine döndürmeyi sağlayan sistemdir**.

### 1.2 Başlangıç

Bazı şeyleri gördüğümüze göre fazla sıkmamak adına artık başlayabiliriz.

Rehbere giriş yapmadan önce test aşamasındaki yardımları için @the_91455'e teşekkür ederim.

***Önemli Not:*** **Öncelikle bu rehber Git'in tüm özelliklerini barındırmaz; içeriğinde en çok karşılaşılan senaryolar ve komutlar vardır. Git'in resmi sayfasında bulunan [Cheatsheet](https://git-scm.com/cheat-sheet) temel alınarak yazılmıştır.**

Bu rehberi kullanmak için mantıken önce Git'in yüklü olması gerekir. Bunun için forumda daha önce açılmış bu konuya bakabilirsiniz:

https://btt.community/t/linux-icin-git-kurulumu-ve-temel-konfigurasyon-rehberi/9334?u=altsysrq

Bu rehberi uyguladıktan sonra Git'i kurmuş ve kullanıma hazır olacaksınız.

## 2. Rehber

Aşamalara geçmeden önce bahsetmek gereken bazı şeyler var. Bu aşamaları anlamak için Git'in bazı yapı özelliklerini anlamamız gerekiyor. Bunlar üç ana bölümden oluşur: çalışma dizini (working directory), staging area ve repository.

Çalışma dizini, projenin bilgisayarındaki hali yani üzerinde direkt çalıştığın klasördür. Dosyaları burada oluşturur, düzenler ve silersin. Ancak bu aşamadaki değişiklikler Git tarafından henüz kayıt altına alınmış sayılmaz.

Staging area, yapılan değişikliklerin bir sonraki commit’e hazırlanmak için bekletildiği alandır. `git add` komutu ile dosyalar buraya alınır. Bu alan sayesinde hangi değişikliklerin kaydedileceğini seçebilirsin. Yani her şeyi değil, sadece istediğini commit’e dahil etme imkânı sağlar.

Repository ise commit’lerin kalıcı olarak saklandığı yerdir. `git commit` komutu çalıştırıldığında staging area’daki veriler alınır ve proje geçmişine kaydedilir. Artık bu değişiklikler bir versiyon haline gelmiş olur ve gerektiğinde geri dönülebilir.

Git’in iç yapısında veriler dosya bazlı değil, obje tabanlı saklanır. Blob, dosyanın içeriğini temsil eder. Tree, klasör yapısını ve dosya ilişkilerini tutar. Commit ise belirli bir anda projenin tamamının snapshot’ını temsil eder ve önceki commit’lere bağlıdır.

HEAD, Git’in şu anda hangi commit üzerinde olduğunu gösteren işaretçidir. Branch ise aslında bir commit’i gösteren basit bir işaretçidir ve farklı geliştirme yollarını paralel şekilde yönetmeyi sağlar.

Genel akış şu şekildedir: dosya değişir, working directory’de bulunur, git add ile staging area’ya alınır, git commit ile repository’ye kaydedilir ve git push ile uzak depoya gönderilir.

Bu yapı sayesinde Git, projelerde hem düzenli bir geçmiş tutar hem de karmaşık geliştirme süreçlerini yönetilebilir hale getirir.

Bunlara değindiğimize göre aşamalara geçebiliriz.

### 2.1 Repo Oluşturma

Bir repo oluşturmak demek, temelde Git versiyon kontrol sistemini bazı dosyaları izlemek ve versiyon kontrolü yapmak için ayarlamak demektir. Bunun için repoya çevirmek istediğiniz dizine gitmeli ve bazı komutları çalıştırmalısınız.

```
cd /gideceğiniz/dizin/
git init
```

### 2.2 Dosya İşlemleri

Git’te dosya işlemleri; değişikliklerin commit’e dahil edilmesi için yapılan ekleme, çıkarma ve taşıma gibi temel işlemleri kapsar. Bu işlemler aynı zamanda dosyaların staging alanına (stage area) alınıp alınmamasını da belirler. Yani Git’te dosya işlemleri, hangi değişikliklerin takip edileceğini ve commit’e dahil edileceğini kontrol etmemizi sağlar.

Temellerden bahsettiysek artık komutlara geçebiliriz.

---

Bu komutu kullanarak dosya ismi girerek dosyaları Git'in takip etmesini, yani stage area'ya almasını sağlayabilirsiniz.

```
git add <dosya>
```

---

Bu komutu kullanarak tek tek dosya girmek yerine hepsini toplu olarak ekleyebilir, yani hepsinin takip edilmesini sağlayabilirsiniz.

```
git add .
```

---

Bu komutu kullanarak dosyaların o anki durumlarını görebilirsiniz:

```
git status
```

---

Bu komutu kullanarak dosyaları silebilirsiniz:

```
git rm <dosya>
```
Bu komutu kullanarak dosyaları Git takibinden çıkarabilir ancak bilgisayarınızda kalmasını sağlayabilirsiniz:

```
git rm --cached <dosya>
```

---

[quote="BilgeGates, post:11, topic:25863"]
Bu komut ile bir dosyayı yeniden adlandırabilir veya başka bir konuma taşıyabilirsiniz:

```
git mv <eski_konum_veya_isim> <yeni_konum_veya_isim>
```

Şimdi biraz daha ileri düzey bir komutumuz var. Bu komut o an stage içerisine alınmamış dosyaların tamamı yerine, belirli kısımlarının seçilerek stage edilmesini sağlar:

```
git add -p
```

Bu komutu yazınca konsolda değişen kod kısımları (hunk) gösterilir ve alt kısımda şöyle bir soru çıkar:

```
(1/1) Bu parça hazırlansın mı [y,n,q,a,d,s,e,?]? 
```

Seçeneklerin açıklamaları şu şekildedir:

* **y** → Bu kısmı stage et
* **n** → Etme
* **q** → Çık
* **a** → Kalanların hepsini stage et
* **d** → Kalanların hiçbirini stage etme
* **s** → Bu parçayı daha küçük parçalara böl *(Sadece parça bölünebilir durumdaysa bu seçenek görünür)*
* **e** → Patch’i elle düzenle
* **?** → Yardımı göster
[/quote]


---

Bu komut her şeyi stage'den çıkarır.

```
git reset
```

---

Bu komut ile dosyayı stage etmeden Git'in dosyayı takip etmesini, yani "Git'e bu dosyayı eklemeyi planlıyorum" demesini sağlayabilirsiniz.

```
git add -N <dosya>
```
veya
```
git add --intent-to-add <dosya>
```

### 2.3 Commit Atmak

Bu aşamaya geldiğimize göre artık stage area'dan çıkıp repository kısmına geçiş yapabiliriz. Çünkü buradaki komutların genel işlevi, stage area'daki değişiklikleri ana repoya aktarmaktır.

---

İlk olarak en basit ve düz komut var. Bu komut ile commit atabilirsiniz ancak commit mesajı için size bir metin editörü açar ve mesajı oradan yazabilirsiniz.

```
git commit
```

---

Bu komut ile metin editörü açmadan commit mesajınızı komut satırında yazabilir ve commit atabilirsiniz.

```
git commit -m 'mesaj'
```

---

Bu komutu kullanarak daha önceden takip edilen dosyalardaki değişiklikleri tekrar `git add` komutunu kullanmadan otomatik olarak stage'e ekleyip commit atabilirsiniz.

```
git commit -am 'message'
```

### 2.4 Git'te Dallar (Branch)

Git üzerinde bir repo için tek bir geliştirme hattı olması zorunlu değildir. Kendinize yeni ekleyeceğiniz özellikler veya yeni sürümler için farklı hatlar, yani dallar oluşturabilirsiniz.

Bunları adı üstünde, bir ağacın dalları gibi düşünebilirsiniz. Ağacın ana bir gövdesi olur, burası sizin main dalınızdır. Ana versiyon yani projenin stabil versiyonu genelde bu gövdede durur. Dallar ise yine ağaçlarda olduğu gibi ana gövdeye bağlı ancak kendi şekline ve farklı özelliklerine sahiptir. Bunu aşağıdaki görsel üzerinden açıklayabiliriz.

![Branch açıklama|595x164](upload://hgOgkMpW4DVpYuqOZVSVEYcHkyQ.png)

Görselde de görüldüğü üzere main dalından “banana” adlı bir dal oluşturulmuş ve geliştirmeye bağımsız şekilde devam edilmiştir.

Mantığı özetlediğimize göre komutlara geçebiliriz.

---

Bu komutu kullanarak Git reponuz üzerinde yeni bir dal açabilirsiniz.

```
git branch <dal_ismi>
```

---

Bu komutları kullanarak reponuz üzerinde var olan dallar arasında geçiş yapabilirsiniz.

```
git switch <isim>
```
veya
```
git checkout <isim>
```

---

Var olan dalları listelemek için:

```
git branch
```

---

Silmek istediğiniz dalı ismini yazarak seçip silebilirsiniz:

```
git branch -d <isim>
```

---

Bir dalı hataları vb. önemsemeden zorla silmek için:

```
git branch -D <isim>
```

---

Bir dal oluşturup anında ona geçmek için:

```
git switch -c <isim>
```
veya
```
git checkout -b <isim>
```

---

Üzerinde olduğunuz dalı yeniden adlandırmak için:
```
git branch -m yeni-isim
```
İsmini yazarak seçtiğiniz dalı yeniden adlandırmak için:

```
git branch -m eski-isim yeni-isim
```
---

Bu komut biraz daha istatistik çıkarmaya yöneliktir; bu komutu kullanarak en çok commit atılan dalı bulabilirsiniz.

```
git branch --sort=-committerdate
```

#### 2.4.1 Dalları Birleştirmek

Git dal mantığını çözdüğümüze göre artık bu dalların birleşimi, birbirine eklenmesi gibi işlemleri görebiliriz.

**Burada değinmek istediğim bir şey var. Şu ana kadar benzer işlemleri yapan iki komut gördük: `rebase` ve `merge`. Bu komutların ikisi de aynı işi yapıyor gibi gözükse de aslında yapma biçimleri farklıdır. `merge` ortak yeni bir commit oluştururken, `rebase` commit'leri birleştirmeden uca ekler. Bunları aşağıdaki komutlarda da göreceksiniz.**

Bu komutları kullanarak diğer dalınızı sanki hep oradaymış gibi ana dala ekleyebilirsiniz.

```
git switch banana # Ekleyeceğiniz dala geçin
git rebase main # Ve ekleyin
```

**Dikkat:** `rebase` commit geçmişini yeniden yazar. Eğer üzerinde çalıştığınız dal başkaları tarafından da kullanılıyorsa (uzak repoya push edilmişse) `rebase` kullanmaktan kaçının. Geçmiş değiştiği için takım arkadaşlarının repolarıyla çakışmaya yol açar. Kural olarak: **sadece kendinize ait ve henüz paylaşılmamış branch'lerde rebase kullanın.**

![Branch açıklama|595x164](upload://hgOgkMpW4DVpYuqOZVSVEYcHkyQ.png)

Eklenmiş hali:

![rebase|585x260](upload://iDdTR7XqwWzfhilVqa3WzRjwtyY.png)

---

Bu komutları kullanarak diğer daldaki commit'leri ana dalda tek bir commit haline getirip ekleyebilirsiniz. Bu işlem tek commit atsa da geçmişi korur.

```
git switch main
git merge banana
```

Öncesi:

![Branch açıklama|595x164](upload://hgOgkMpW4DVpYuqOZVSVEYcHkyQ.png)

Sonrası:

![squash|595x164](upload://2amwrxHMFaelKz8b75wP8zrBQaC.png)

Ancak burada istisnai bir durum vardır. Bu durum fast-forward olarak adlandırılır. Bu, ana dala diğer daldan commit'ler atıldıktan sonra bir şeyler eklenmediğinde iki dalı hızlıca birleştirmede yaşanır. Aslında yaptığı şey sadece işaretçiyi (pointer) ileri taşımaktır.

```
git switch main
git merge banana
```

Öncesi:

![fast forward 1|644x164](upload://wDB3O9jWpoGu2N1IWyEkqiF2CfD.png)

Sonrası:

![fast forward 2|659x164](upload://8sfkM3pRZhLPweof0pwJp5I9nLj.png)

---

Bu komutu kullanarak birden fazla commit'i tek commit haline getirebilirsiniz. Ancak bu geçmişi tutmaz. Basitleştirirsek "ayrı ayrı göstermemek" denebilir.

```
git switch main
git merge --squash banana
git commit
```

Öncesi:

![Branch açıklama|595x164](upload://hgOgkMpW4DVpYuqOZVSVEYcHkyQ.png)

Sonrası:

![merge|595x179](upload://3mjYTzNOUOQ7IOGo8dIpzlEotuj.png)

---

Bu komutu kullanarak diğer daldan sadece istediğiniz commit'i seçip ana dala ekleyebilirsiniz.

```
git cherry-pick <commit>
```

[quote="BilgeGates, post:11, topic:25863"]
### 2. Yeni Bölüm: 2.4.2 Birleşme Çakışmalarını (Merge Conflict) Çözmek

**2.4.2 Birleşme Çakışmalarını (Merge Conflict) Çözmek**
`merge` veya `rebase` sırasında iki dal aynı dosyanın aynı satırını farklı şekilde değiştirdiyse, Git bunları tek başına birleştiremez ve bir conflict oluşur. Bu durumda Git işlemi durdurur.

Conflictli dosyaları görmek için:

```
git status

```

Conflictli dosyayı açtığınızda Git, problemli bölgeyi şu şekilde işaretler:

```
<<<<<<< HEAD
senin dalındaki hali
=======
diğer daldan gelen hali
>>>>>>> banana

```

Dosyayı elle düzenleyip kodun son ve doğru hâlini bırakırsınız. Ardından Git’in eklediği bu üç işaretçi satırını (`<<<<<<<`, `=======`, `>>>>>>>`) silersiniz.

Sonra sorunu çözdüğünüzü Git’e bildirmek için dosyayı tekrar stage edersiniz:

```
git add <dosya_adi>

```

Ve işlemi türüne göre tamamlarsınız:

```
git merge --continue    # merge yapıyorduysanız
git rebase --continue   # rebase yapıyorduysanız

```

Eğer işlerin karıştığını düşünür ve işlemden tamamen vazgeçmek isterseniz:

```
git merge --abort
git rebase --abort
```
[/quote]



### 2.5 Repodaki Farklılıkları Görmek

Bu işlemler, başlıkta da görüldüğü üzere repo üzerine yeni eklenen değişiklikler ile öncekiler arasındaki farkları gösterir. Bu işlemler genellikle `diff` komutlarıyla yapılır. "Diff", İngilizce *difference* yani *farklılık* kelimesinin kısaltmasıdır. Bu işlemlerin komutları aşağıdaki gibidir:

Bu komut son commit'ten beri repo üzerinde yapılmış, stage edilmiş ve edilmemiş değişikliklerin tümünü listeler.

```
git diff HEAD
```

Bu komut son commit'ten beri repo üzerinde yapılmış stage edilmiş değişiklikleri listeler.

```
git diff --staged
```

Bu komut son commit'ten beri repo üzerinde yapılmış stage edilmemiş değişiklikleri listeler.

```
git diff
```

**Bu komutlardan sonra commit'ler arasındaki farklara geçiyoruz.**

Bu komutu kullanarak belirttiğiniz commit'te yapılan değişiklikleri listeleyebilirsiniz.

```
git show <commit_hash>
```

Bu komut; belirttiğiniz commit'te sadece değişen dosya sayısı, satır sayısı gibi istatistikleri verir.

```
git show <commit_hash> --stat
```

Bu komut, iki commit arasındaki değişiklikleri karşılaştırmanızı sağlar.

```
git diff <1.commit_hash> <2.commit_hash>
```

Bu komut yukarıdaki komutu, daha önce bahsettiğimiz stat özelliği ile yapar.

```
git diff <1.commit_hash> <2.commit_hash> --stat
```

Bu komut belirttiğiniz commit'te, belirttiğiniz dosyada yapılmış olan değişiklikleri verir.

```
git diff <commit_hash> <dosya>
```

Git’te `<commit_hash>` isteyen yere sadece hash değil, farklı şeyler de yazabilirsiniz. Bunlar hash yerine kullanılan referans olarak geçer:

* `main` gibi branch isimleri → o branch’in son commit’i
* `v0.1` gibi tag’ler → o sürüme ait commit
* `3e887ab` gibi hash → direkt commit'in kendisi
* `origin/main` → uzak repodaki branch
* `HEAD` → şu anki commit
* `HEAD~3` veya `HEAD^^^` → 3 commit geriye gider

### 2.6 Değişiklikleri Geri Almak

Başlangıçta **1.1 Versiyon Kontrol Sistemi** bölümünde bu özellikten, versiyon kontrol sistemlerinin görevleri kısmında bahsetmiştik. Şimdi ise bu geri alma işlemlerini pratikte nasıl yaptığımıza bakacağız.

---

`git reset` komutu, yapılan değişiklikleri geri almak veya Git’in takip ettiği durumu önceki bir noktaya döndürmek için kullanılır. Bu komutun etkisi kullanılan moda göre değişir.

```
git reset
```

Bu komut, staging area (index) içinde bulunan tüm değişiklikleri geri alır. Yani dosyalar çalışma dizininde kalır ancak `git add` ile eklenmiş durumdan çıkarılır.

```
git reset <dosya>
```

Bu komut yalnızca belirtilen dosyayı staging area’dan çıkarır. Dosya üzerindeki değişiklikler silinmez, sadece commit’e hazırlanmış durumdan geri alınır.

```
git reset --soft HEAD~1
```

Son commit’i geri alır ancak yapılan değişiklikleri staging area içinde bırakır. Yani değişiklikler commit edilmemiş ama "hazır" durumda kalır.

```
git reset --mixed HEAD~1
```

Son commit’i geri alır ve değişiklikleri staging area’dan da çıkarır. Dosyalar çalışma dizininde kalır ama tekrar `git add` yapılması gerekir.

```
git reset --hard HEAD~1
```

Son commit’i tamamen siler ve tüm değişiklikleri geri döndürür. Çalışma dizinindeki dosyalar da dahil olmak üzere her şey belirtilen commit durumuna geri gider.

**Bu mod, geri alınması zor değişiklikler yapabilir, dikkatli kullanılmalıdır.**

`git reset` komutu ve modlarını bitirdiğimize göre diğer komutlar ve özelliklerine geçebiliriz.

---

Bu komutları kullanarak dosyayı son commit'teki haline çevirebilir ve stage edilmemiş değişiklikleri silebilirsiniz. Bu komut stage edilmiş değişiklikleri silmez.

```
git restore <dosya> # Modern komuttur.
```
veya
```
git checkout <dosya> # Eski halidir.
```

---

`git add` komutu ile takip edilmeyen, stage edilmemiş dosyaları silmek için:

```
git clean -f
```

**Dikkat:** `git clean` ile silinen dosyalar çöp kutusuna gitmez, doğrudan silinir ve **geri alınamaz**. Çalıştırmadan önce `git clean -n` ile hangi dosyaların etkileneceğini mutlaka kontrol edin. Sadece untracked dosyaları değil, dizinleri de silmek istiyorsanız `-fd`, Git tarafından yoksayılan (`.gitignore`'daki) dosyaları da dahil etmek istiyorsanız `-fx` kullanılır.

---

Bu komutları kullanarak seçilen dosyayı çalışma dizini üzerinde de tamamen son commit'te bulunan haline çevirebilirsiniz.

```
git restore --staged --worktree <dosya> # Modern Yöntem
```
veya
```
git checkout HEAD <dosya> # Eski Yöntem
```

---

Bu komut ile çalışma dizini ve staging area üzerindeki tüm değişiklikler geçici olarak saklanır ve çalışma dizini son commit'teki (HEAD) haline geri döner. Bu işlem commit oluşturmaz, sadece değişiklikleri stash alanına taşır.

```
git stash
```

Sonradan geri almak için:

```
git stash apply
```
Değişiklikleri geri getirir (stash kalır).

```
git stash pop
```
Değişiklikleri geri getirir ve stash'i siler.

[quote="BilgeGates, post:11, topic:25863"]
Saklanan tüm stash’leri listelemek için:

```
git stash list

```

En üstteki stash’i çalışma dizinine geri getirmeden tamamen silmek için:

```
git stash drop

```

Belirli bir stash’i (listeden sırasına bakarak) seçip geri getirmek için:

```
git stash apply stash@{2}
```
[/quote]



### 2.7 Commit Geçmişini Düzenlemek

Bu komutlar ile commit'leri geri alma, ileri alma gibi işlemler yapabilirsiniz.

---

Bu komut son commit’i geri alır ama değişiklikleri silmez; sadece commit’i kaldırıp dosyaları çalışma alanında bırakır.

```
git reset HEAD^
```

---

Bu komut, son **N commit’i interaktif rebase ile düzenlemek** için kullanılır:

```
git rebase -i HEAD~N
```

Buradaki `N`, geriye doğru kaç commit ile çalışmak istediğinizi belirtir (örneğin son 3, 5, 10 commit).

Bu komut çalıştırıldığında Git, seçilen commit’leri bir editör içinde listeler. Her satırın başında genelde `pick` ifadesi bulunur.

Eğer commit’leri birleştirmek (squash etmek) istiyorsan:

* `pick` → commit’i olduğu gibi kullanır
* `squash` → bu commit’i bir önceki commit ile birleştirir ve commit mesajlarını düzenleme imkânı verir
* `fixup` → commit’i birleştirir ama **sadece ilk commit mesajını korur**, diğerlerini siler

---

Bu komutu kullanarak seçtiğiniz daldaki tüm HEAD hareketlerinin değişimini gösterir.

```
git reflog <dalın_adı>
```

---

Bu komutu kullanarak `git reflog` komutundan aldığınız hash ile belirttiğiniz commit'e geri dönebilirsiniz.

```
git reset --hard <hash>
```

---

Bu komutu kullanarak son commit üzerinde düzenleme yapabilirsiniz. Örneğin unuttuğunuz dosyayı dahil edebilir veya mesajını değiştirebilirsiniz.

```
git commit --amend
```

### 2.8 Uzak Konum

Git'i kullanmanın temel amacı; birden fazla geliştiricinin düzenli ve kayıtlı bir şekilde işlem yaparak daha verimli bir işbirliği sağlamaktır. Mantıken birden fazla kişinin tek bilgisayarda çalışması pek mümkün değildir. Bu yüzden Git üzerinde uzak bir sunucuda, uzak konumda çalışabileceğiniz, dosyaları yükleyip indirebileceğiniz komutlar bulunur.

Bu işlemleri yapmak için kullanabileceğiniz komutlar şu şekildedir:

Bu komutu kullanarak uzak konumdaki bir repoyu klonlayabilirsiniz.

```
cd /klonlayacağınız/dizin/
git clone <url>
```

Burada ayrıca bahsetmek istediğim bir nokta var: Eğer repo genelde olduğu gibi GitHub kullanıyorsa, daha kolay klonlayabileceğiniz bir araç mevcuttur. Bu araç GitHub CLI yani GitHub'ın komut satırı aracıdır. Bunu kullanmak için:

```
gh auth login # Burada size verdiği adımlar ile giriş yapacaksınız.
gh repo clone kullanıcı/repo
```

Bu komutlar ile kolayca kopyalayabilirsiniz. **Ancak bu araç ayrı bir araçtır, komutları farklıdır.**

Bu aracın size getirdiği bir ekstra katkı daha vardır. Eğer projelerinizde Github kullanıyorsanız ve giriş için token veya SSH key gibi yolları kullanıyorsanız. Bunları manuel ayarlamak yerine Github CLI (gh) ile `gh auth login` komutunu kullanarak giriş yapabilirsiniz. Bu sayede Git işlemlerinizde gerekli kimlik doğrulaması otomatik olarak yönetilir ve token veya SSH key ile yaptığınız işlemleri yapmanızı sağlar. **(Bu araç Git aracından farklı bir araçtır. Bu yüzden bu rehberde bu araçtan detaylıca bahsetmeyeceğiz.)**

#### 2.8.1 Uzak Konuma Veri Göndermek

Bu komutla yerel deponuza uzak konumu bağlayabilirsiniz.

```
git remote add <isim> <url>
```

---

Uzaktaki konumda seçtiğiniz dala değişiklikleri pushlamak (göndermek) için:

```
git push origin <dal>
```

---

Üzerinde bulunduğunuz dalı pushlamak için:

```
git push
```

---

Daha önce hiç pushlamadığınız dalı pushlamak için:

```
git push -u origin <isim>
```

---

Bazı hatalar oluştuğu vb. durumlarda zorla pushlamak için:

```
git push --force-with-lease
```

---

Etiketler ile birlikte pushlamak için kullanılır:

```
git push --tags
```
Etiketleri sonraki konuda göreceğiz.

---

#### 2.8.2 Uzak Konumdan Veri Çekmek

Uzak konumdaki değişiklikleri indirir ancak sizdeki dosyaları değiştirmez.

```
git fetch origin main
```

---

Bu komutu kullanarak uzaktaki değişiklikleri dosyalara yazabilirsiniz.

```
git pull origin main
```
Aslında şunun kısayoludur:

```
git fetch origin main
git merge origin/main
```

---

Bu komutu kullanarak istediğiniz dalı seçip indirebilirsiniz.

```
git pull origin <isim>
```

---

### 2.9 Git'i Konfigüre Etmek

Bu komutları kullanarak isim ayarlama, e-posta ayarlama gibi çeşitli konfigürasyonlar yapabilirsiniz.

---

Bu komutu kullanarak depo üzerinde ekleme yaparken kullanılacak adınızı belirleyebilirsiniz:

```
git config user.name "Adınız"
```
Bu komutu kullanarak aynı işlemi depo yerine sistem genelinde ayarlayabilirsiniz:

```
git config --global user.name "Adınız"
```

---

Bu komutu kullanarak depo üzerinde kullanılacak e-postanızı ayarlayabilirsiniz:

```
git config user.email "mailin@example.com"
```
Bu komutu kullanarak depo yerine sistem genelinde ayarlayabilirsiniz:

```
git config --global user.email "mailin@example.com"
```

---

Bu komutu kullanarak alias ekleyebilirsiniz:

```
git config alias.<aliasınız> <atayacağınız_komut>
```

---

Bu komutu kullanarak tüm konfigürasyon komutlarını görebilirsiniz:

```
man git-config
```

---

### 2.10 Kod Yapısını İncelemek

Bu komutları kullanarak dalların geçmişini inceleyebilirsiniz.

```
git log main # Commit geçmişini düz yazı halinde görebilirsiniz.
```
```
git log --graph main # Bu komutu kullanarak grafikleştirilmiş haliyle görebilirsiniz.
```
```
git log --oneline # Bu komutu kullanarak satır satır görebilirsiniz.
```

---

Bu komutu kullanarak bir dosya üzerinde değişim yapılan tüm commit'leri görebilirsiniz.

```
git log <dosya>
```

---

Dosyanın ismi değişse bile, geçmişteki tüm commit’leri takip ederek komple tarihçesini gösterir.

```
git log --follow <dosya>
```

---

Bu komutu kullanarak commit'lerde seçtiğiniz kelime değişmiş mi değişmemiş mi diye bakabilirsiniz.

```
git log -G <kelime>
```

---

Dosyada değişen satırları kimin değiştirdiğini gösterir.

```
git blame <file>
```

[quote="BilgeGates, post:11, topic:25863"]
Bu komutu kullanarak tüm dalları, aralarındaki bağlantıları ve geçmişi tek bir grafik üzerinde görebilirsiniz:

```
git log --oneline --graph --all
```

Bu komutu kullanarak sadece son birkaç (örneğin 5) commit’i kısa ve özet halinde görebilirsiniz:

```
git log --oneline -5
```

Bu komutu kullanarak sadece belirli bir yazara ait commit’leri filtreleyebilirsiniz:

```
git log --author="İsim"
```
[/quote]



### 2.11 Önemli Dosyalar

* `.git/config`: Sadece o repo’ya özel Git ayarlarının bulunduğu dosya.
* `~/.gitconfig`: Kullanıcının tüm sistem Git ayarları burada durur.
* `gitignore`: Git’in hangi dosyaları görmezden geleceğini söyler.

## 3. Senaryolar

### Senaryo 1 — Sıfırdan proje açıp GitHub’a gönderme

```
mkdir web-proje
cd web-proje
git init
```

Burada yeni bir proje klasörü oluşturabilir ve bunu Git projesi haline getirebilirsiniz. Artık bu klasördeki değişiklikler takip edilebilir hale gelir.

```
echo "# Web Projesi" > README.md
git add README.md
git commit -m "ilk proje dosyası eklendi"
```

Burada bir dosya oluşturabilir ve Git’e bu dosyayı kaydetmesini söyleyebilirsiniz. Sonra ilk versiyonu oluşturmuş olursunuz.

```
git remote add origin https://github.com/kullanici/web-proje.git
git branch -M main
git push -u origin main
```

Burada yerel projeyi GitHub’a bağlayabilir, projeyi internet üzerine gönderebilirsiniz. Artık başka yerden de görülebilir hale gelir.

---

### Senaryo 2 — Ekip projesine girip özellik ekleme

```
git clone https://github.com/kullanici/ekip-proje.git
cd ekip-proje
```

Burada başka bir projenin kopyasını bilgisayarınıza indirebilirsiniz. Artık ekip ile aynı kod üzerinde çalışabilirsiniz.

```
git checkout -b login-sistemi
```

Burada ana projeyi bozmadan yeni bir çalışma alanı açabilirsiniz. Böylece sadece login özelliği üzerinde çalışırsınız.

```
echo "login ekranı hazır" > login.txt
git add login.txt
git commit -m "login sistemi eklendi"
git push origin login-sistemi
```

Burada bir özellik ekleyebilir, bunu kaydedebilir ve GitHub’a gönderebilirsiniz. Sonra ekip bunu görebilir ve isterse ana projeye ekleyebilir.

[quote="BilgeGates, post:11, topic:25863"]
Not: Bu özellik kaydedilip GitHub’a gönderildikten sonra, değişiklik genellikle GitHub üzerinde bir **Pull Request (PR)** açılarak ana projeye dahil edilir.
[/quote]


---

### Senaryo 3 — Hata düzeltme ve geri dönme işlemi

```
git checkout main
git pull origin main
git checkout -b hata-duzeltme
```

Burada önce ana projeye geri dönebilir, en güncel hali çekebilir ve ardından hata düzeltmek için yeni bir dal açabilirsiniz. Böylece ana kodu bozmadan çalışabilirsiniz.

```
echo "bug fix yapıldı" > fix.txt
git add fix.txt
git commit -m "kritik hata düzeltildi"
git push origin hata-duzeltme
```

Burada bir hatayı düzeltebilir, değişiklikleri kaydedebilir ve GitHub’a gönderebilirsiniz. Ekip bu değişikliği inceleyip uygun görürse ana projeye ekleyebilir.

```
git checkout main
git merge hata-duzeltme
```

Burada yapılan düzeltmeyi ana projeye dahil edebilirsiniz. Böylece hata resmi olarak çözülmüş olur.

[quote="BilgeGates, post:11, topic:25863"]
Not: Hata düzeltilip `hata-duzeltme` dalı uzak sunucuya pushlandıktan sonra, bu değişiklik genellikle GitHub üzerinde bir **Pull Request (PR)** açılarak ana brancha (main/master veya her neyse) dahil edilir. Ekip kodu inceler ve onaylarsa birleştirme (merge) işlemi tamamlanır.
[/quote]



## 4. Kaynakça

Bu rehber için yukarıda bahsettiğim gibi en çok Git Cheatsheet ve Git Dokümentasyonu'ndan yararlandım. Linkleri aşağıdadır:

* [Git Cheatsheet](https://git-scm.com/cheat-sheet)
* [Git Docs](https://git-scm.com/docs)
* https://tr.wikipedia.org/wiki/Git_(yaz%C4%B1l%C4%B1m)
* https://tr.wikipedia.org/wiki/Linus_Torvalds
* https://cli.github.com/manual/

Görseller için yararlandığım kaynak ise yine Git Cheatsheet üzerinde bulunan görsellerdir.

- [Git Cheatsheet](https://git-scm.com/cheat-sheet)