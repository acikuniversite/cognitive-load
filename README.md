# Bilişsel Yük Önemlidir

*Bu bir yaşayan dokümandır, son güncelleme: **Haziran 2024***

## Giriş
Birçok moda kelime ve en iyi uygulama var, ama daha temel bir şeye odaklanalım. Önemli olan, geliştiricilerin kodu incelerken hissettikleri kafa karışıklığıdır.

Kafa karışıklığı zaman ve paraya mal olur. Kafa karışıklığı, yüksek *bilişsel yük* nedeniyle oluşur. Bu, soyut bir kavram değil, **temel bir insan sınırlamasıdır**.

Kod yazmaktan çok daha fazla zaman harcadığımız için, sürekli olarak kodumuza aşırı bilişsel yük ekleyip eklemediğimizi sormalıyız.

## Bilişsel Yük
> Bilişsel yük, bir geliştiricinin bir görevi tamamlamak için düşünmesi gereken miktardır.

Kod okurken değişken değerlerini, kontrol akış mantığını ve çağrı dizilerini kafanızda tutarsınız. Ortalama bir kişi, işleyen hafızasında yaklaşık [dört şey](https://github.com/zakirullin/cognitive-load/issues/16) tutabilir. Bilişsel yük bu eşiğe ulaştığında, anlamak için önemli bir çaba gerektirir.

*Diyelim ki tamamen yabancı bir projeye bazı düzeltmeler yapmamız istendi. Bize gerçekten zeki bir geliştiricinin katkıda bulunduğu söylendi. Birçok havalı mimari, süslü kütüphaneler ve trend teknolojiler kullanılmış. Başka bir deyişle, **önceki yazar bize yüksek bilişsel yük yaratmış.***

![Cognitive Load](/img/cognitiveloadv4.png)

Projelerimizdeki bilişsel yükü mümkün olduğunca azaltmalıyız.

Zor olan kısım, önceki yazarın projeye aşina olması nedeniyle yüksek bilişsel yük yaşamamış olabilir.

<details>
  <summary><b>Aşinalık vs Basitlik</b></summary>
  <br>
  Sorun şu ki <b>aşinalık basitlikle aynı şey değildir</b>. Aynı hissi verirler — fazla zihinsel çaba harcamadan bir alanda hareket etme kolaylığı — ama çok farklı nedenlerle. Kullandığınız her "akıllı" (okuyun: "kendine dönük") ve gayri-idiomatik numara, herkes için bir öğrenme cezası oluşturur. Bu öğrenmeyi tamamladıklarında, kodla çalışmak daha az zor olacaktır. Yani, zaten aşina olduğunuz kodu nasıl basitleştireceğinizi anlamak zor olabilir. Bu yüzden, kodu çok fazla içselleştirmeden önce yeni gelen kişinin kodu eleştirmesini sağlamaya çalışıyorum!<br><br>
  Muhtemelen önceki yazar(lar) bu büyük karmaşayı bir kerede değil, küçük artışlarla oluşturdu. Bu yüzden, hepsini bir arada anlamaya çalışan ilk kişi sizsiniz.<br><br>
  Sınıfımda bir gün yüzlerce satır koşullu ifadeler içeren geniş bir SQL saklı prosedürü incelerken biri sordu: "Bunun bu kadar kötü hale gelmesine nasıl izin verildi?" Dedim ki: “Sadece 2 veya 3 koşullu ifade olduğunda, başka bir tane eklemek fark yaratmaz. 20 veya 30 koşullu ifade olduğunda, başka bir tane eklemek fark yaratmaz!”<br><br>
  Kodu basitleştiren bir kuvvet yoktur, sadece yaptığınız bilinçli tercihler vardır. Basitleştirmek çaba gerektirir ve insanlar çoğu zaman acele ederler.<br><br>
  <i>Yukarıdaki yorumu için <a href="https://dannorth.net">Dan North</a>'a teşekkürler.</i><br><br>
</details>

Projeye yeni insanları dahil ettiğinizde, yaşadıkları kafa karışıklığını ölçmeye çalışın (çift programlama yardımcı olabilir). Eğer 40 dakikadan fazla kafa karışıklığı yaşıyorlarsa, kodunuzda geliştirilmesi gereken şeyler var demektir.

<details>
  <summary><b>Bilişsel yük ve kesintiler</b></summary>
  <img src="img/interruption.jpeg"><br>
</details>

## Bilişsel Yük Türleri
**İçsel** - bir görevin doğasından kaynaklanan zorluk. Azaltılamaz, yazılım geliştirmenin tam kalbindedir.

**Dışsal** - bilginin sunulma şekliyle oluşturulan. Görevle doğrudan ilgili olmayan faktörlerden kaynaklanır, örneğin zeki yazarın kaprisleri. Büyük ölçüde azaltılabilir. Bu tür bilişsel yüke odaklanacağız.

![Intrinsic vs Extraneous](/img/smartauthorv5.png)

Şimdi dışsal bilişsel yükün somut pratik örneklerine geçelim.

*Not: Katkılar memnuniyetle karşılanır!*

---

Bilişsel yük seviyesini şu şekilde ifade edeceğiz:  
`🧠`: taze işleyen hafıza, sıfır bilişsel yük  
`🧠++`: işleyen hafızamızda iki gerçek, bilişsel yük arttı  
`🤯`: işleyen hafıza aşımı, 4'ten fazla gerçek

## Karmaşık Koşullar
```go
if val > someConstant // 🧠+
    && (condition2 || condition3) // 🧠+++, önceki koşul doğru olmalı, c2 veya c3 doğru olmalı
    && (condition4 && !condition5) { // 🤯, burada kafamız karıştı
    ...
}
```

Anlamlı isimlerle ara değişkenler tanıtın:
```go
isValid = var > someConstant
isAllowed = condition2 || condition3
isSecure = condition4 && !condition5 
// 🧠, koşulları hatırlamak zorunda değiliz, açıklayıcı değişkenler var
if isValid && isAllowed && isSecure {
    ...
}
```

## İç İf Blokları
```go
if isValid { // 🧠+, tamam iç içe kod sadece geçerli giriş için geçerli
    if isSecure { // 🧠++, sadece geçerli ve güvenli giriş için işler yapıyoruz
        stuff // 🧠+++
    }
} 
```

Erken dönüşlerle karşılaştırın:
```go
if !isValid
    return
 
if !isSecure
    return

// 🧠, önceki dönüşleri gerçekten umursamıyoruz, buradaysak her şey yolunda demektir

stuff // 🧠+
```

Sadece mutlu yola odaklanabiliriz, böylece işleyen hafızamızı her türlü ön koşuldan kurtarabiliriz.

## Kalıtım Kabusu
Bize yönetici kullanıcılarımız için birkaç şey değiştirilmesi söylendi: `🧠`

`AdminController, UserController'dan türetilir, GuestController, BaseController'dan türetilir`

Ohh, işlevselliğin bir kısmı BaseController içinde, bakalım: 🧠+
Temel rol mekanikleri GuestController içinde tanıtılmış: 🧠++
Şeyler kısmen UserController içinde değiştirilmiş: 🧠+++
Sonunda buradayız, AdminController, kod yazalım! 🧠++++

Oh, bekleyin, SuperuserController var, bu AdminControllerdan türetilmiş. AdminControllerı değiştirerek, türetilmiş sınıfta şeyleri bozabiliriz, bu yüzden önce SuperuserControllera bakalım: 🤯

Kalıtımdan çok kompozisyonu tercih edin. Ayrıntılara girmeyeceğiz - orada [bolca malzeme](https://www.youtube.com/watch?v=hxGOiiR9ZKg) var.

## Çok Fazla Küçük Yöntem, Sınıf veya Modül
> Bu bağlamda yöntem, sınıf ve modül birbirinin yerine kullanılabilir
 
"Yöntemler 15 satırdan kısa olmalı" veya "sınıflar küçük olmalı" gibi mantralar biraz yanlış çıktı.

**Derin modül** - basit arayüz, karmaşık işlevsellik
**Sığ modül**- sağladığı küçük işlevsellik için nispeten karmaşık arayüz

![Deep module](/img/deepmodulev5.png)

Çok fazla sığ modüle sahip olmak, projeyi anlamayı zorlaştırabilir. **Sadece her modülün sorumluluklarını akılda tutmakla kalmıyoruz, aynı zamanda tüm etkileşimlerini de akılda tutmamız gerekiyor.** Sığ bir modülün amacını anlamak için, önce ilgili tüm modüllerin işlevselliğine bakmamız gerekir. 🤯

> Bilgi gizleme çok önemlidir ve sığ modüllerde o kadar çok karmaşıklığı gizlemiyoruz.

İki evcil hayvan projem var, her ikisi de biraz 5K kod satırından oluşuyor. Birincisinde 80 sığ sınıf varken, ikincisinde yalnızca 7 derin sınıf var. Bir buçuk yıldır bu projelerin hiçbirini yürütmüyorum.

Geri döndüğümde, ilk projedeki 80 sınıf arasındaki tüm etkileşimleri çözmenin son derece zor olduğunu fark ettim. Kodlamaya başlamadan önce muazzam miktarda bilişsel yükü yeniden oluşturmam gerekecekti. Öte yandan, ikinci projeyi hızlı bir şekilde kavrayabildim çünkü basit bir arayüze sahip sadece birkaç derin sınıf vardı.

> En iyi bileşenler, güçlü işlevsellik sağlayan ancak basit bir arayüze sahip olanlardır.
> **John K. Ousterhout**

UNIX I/O'nun arayüzü çok basittir. Sadece beş temel çağrıya sahiptir:
```python
open(path, flags, permissions)
read(fd, buffer, count)
write(fd, buffer, count)
lseek(fd, offset, referencePosition)
close(fd)
```

Bu arayüzün modern bir uygulaması **yüz binlerce satır kod** içerir. Çok fazla karmaşıklık perdenin altında gizlidir. Yine de basit arayüzü sayesinde kullanımı kolaydır.

> Bu derin modül örneği John K. Ousterhout'un [Yazılım Tasarımının Felsefesi](https://web.stanford.edu/~ouster/cgi-bin/book.php) adlı kitabından alınmıştır. Bu kitap yalnızca yazılım geliştirmedeki karmaşıklığın özünü ele almakla kalmaz, aynı zamanda Parnas'ın etkili makalesinin [Sistemleri Modüllere Ayırmada Kullanılacak Kriterler Üzerine](https://www.win.tue.nl/~wstomv/edu/2ip30/references/criteria_for_modularization.pdf) en iyi yorumunu da içerir. Her ikisi de olmazsa olmaz okumalardır. Diğer ilgili okumalar: [Muhtemelen Temiz Kod'u önermeyi bırakmanın zamanı geldi](https://qntm.org/clean), [Küçük Fonksiyonlar Zararlı Olarak Kabul Ediliyor](https://copyconstruct.medium.com/small-functions-considered-harmful-91035d316c29), [Doğrusal kod daha okunabilir](https://blog.separateconcerns.com/2023-09-11-linear-code.html).
Eğer bizim çok fazla sorumluluğu olan şişkin objelerini desteklediğimizi düşünüyorsanız yanılıyorsunuz.

## Çok sayıda yüzeysel mikroservis
Yukarıdaki ölçek-agnostik ilkeyi mikroservis mimarisi için de uygulayabiliriz. Çok sayıda yüzeysel mikroservisin pek faydası olmayacak - endüstri bir nebze "makroservislere" doğru ilerliyor, yani çok derin olmayan servisler. Bu aşırı granüler yüzeysel ayrımın sonucu olan en kötü ve en zor düzeltilen olgulardan biri ise dağıtık monolit olarak adlandırılıyor.

Bir startupta danışmanlık yaptığım bir zaman, üç geliştiriciden oluşan bir ekip 17(!) mikroservis tanıttı. Planlanan sürenin 10 ay gerisindeydiler ve halka sunulmaya hiç yaklaşamamışlardı. Her yeni gereksinim 4'ten fazla mikroserviste değişiklik yapılmasına yol açıyordu. Entegrasyon alanında teşhis zorluğu doruk noktaya ulaştı. Hem pazara çıkış süresi hem de bilişsel yük kabul edilemez derecede yüksekti. 🤯

Yeni bir sistemde belirsizliğe bu şekilde mi yaklaşmalıyız? Başlangıçta doğru mantıksal sınırları belirlemek olağanüstü derecede zordur ve çok sayıda mikroservis tanıtarak işleri daha da kötüleştiririz. Ekip sadece şunu savunuyordu: "F(M)AANG şirketleri mikroservis mimarisinin etkili olduğunu kanıtladı". Merhaba, hayal kurmayı bırakmalısınız.

Tanenbaum-Torvalds tartışmasına bir göz atın. Linux'un monolitik tasarımının kusurlu ve çağdışı olduğu iddia edildi ve bunun yerine mikroçekirdek mimarisi kullanılmalıydı. Gerçekten de, mikroçekirdek tasarımı "teorik ve estetik" açıdan üstün görünüyordu. Üç on yıl sonra, mikroçekirdek tabanlı GNU Hurd hâlâ geliştirme aşamasında ve monolitik Linux her yerde - bu sayfa Linux tarafından desteklenmektedir, akıllı çaydanlık Linux tarafından çalıştırılmaktadır.

Gerçekten izole edilmiş modüllerle iyi bir şekilde oluşturulmuş bir monolit, genellikle bir dizi mikroservisten çok daha esnektir. Ayrıca, bakımı için çok daha az bilişsel çaba gerektirir. Modüller arasına bir ağ katmanı eklemeyi düşünmelisiniz (gelecekteki mikroservisler gibi) sadece ayrı dağıtımların gerekliliği belirleyici hale geldiğinde.

## Özelliklerle dolu diller
## Özelliklerle dolu diller
Favori dilimizde yeni özellikler çıktığında heyecanlanırız. Bu özellikleri öğrenmek için zaman harcarız, üzerine kodlar oluştururuz.

Eğer çok fazla özellik varsa, bazen birkaç satır kodla oynamak için yarım saat harcayabiliriz, bir özellik kullanmak için diğerine geçmek için. Bu zaman kaybı gibi görünüyor. Ama daha da kötüsü, **daha sonra geri döndüğünüzde, o düşünce sürecini yeniden oluşturmanız gerekebilir!** `🤯`

**Bu ifadeler, Rob Pike tarafından yapılmıştır.**

> Seçeneklerin sayısını sınırlayarak bilişsel yükü azaltın.

Dil özellikleri, birbirine dik olduğu sürece sorun değil.

<details>
  <summary><b>20+ yıllık C++ deneyimine sahip bir mühendisin düşünceleri ⭐️</b></summary>
  <br>
  Geçenlerde RSS okuyucuma baktım ve "C++" etiketi altında yaklaşık üç yüz okunmamış makale olduğunu fark ettim. Geçen yazdan beri dil hakkında hiçbir makale okumadım ve harika hissediyorum!<br><br>
  Şu anda 20 yıldır C++ kullanıyorum, bu hayatımın neredeyse üçte ikisi demek. Deneyimimin çoğu dilin en karanlık köşeleriyle uğraşmakla geçiyor (tanımsız davranışlar gibi). Bu tekrar kullanılabilir bir deneyim değil ve şimdi hepsini atmak biraz ürkütücü.<br><br>
  Mesela, `||` sembolünün biri `requires ((!P<T> || !Q<T>))` ve diğeri `requires (!(P<T> || Q<T>))` içinde farklı anlamları olduğunu hayal edebilir misiniz? İlkisi kısıtlama bağlacı, ikincisi ise bildiğimiz mantıksal VEYA operatörü, ve farklı davranışlar sergiliyorlar.<br><br>
  Bir trivial tür için bellek ayıramaz ve sadece bir set baytı `memcpy` ile oraya kopyalayamazsınız - bu bir nesnenin yaşamını başlatmazdı. Bu durum C++20'den önce böyleydi. C++20'de düzeltildi ama dilin bilişsel yükü sadece arttı.<br><br>
  Şey, şeyler düzeltilse de bilişsel yük sürekli artıyor. Ne zaman neyin düzeltildiğini, ne zaman nasıl olduğunu bilmeliyim. Sonuçta profesyonelim. C++ mirasıyla uğraşmayı seven bir dil. Mesela geçen ay bir meslektaşım bana C++03'te bazı davranışlar hakkında sordu. `🤯`<br><br>
  20 farklı başlatma yöntemi vardı. Birleşik başlatma sözdizimi eklendi. Şimdi 21 başlatma yöntemimiz var. Bu arada, başlatma listesinden yapıcı seçme kurallarını hatırlayan var mı? Bir şeyler, dolaylı dönüşümle en az kayıpla bilgi kaybı yaşamadan seçimle ilgili, <i>ama eğer</i> değer statik olarak biliniyorsa... `🤯`<br><br>
  <b>Bu artan bilişsel yük, eldeki iş görevinden kaynaklanmıyor. Alanın özgül karmaşıklığı değil. Tarihsel nedenlerden dolayı orada</b> (<i>fazladan bilişsel yük</i>).<br><br>
  Bazı kurallar koymak zorunda kaldım. Mesela, eğer o kod satırı açık değilse ve standartı hatırlamam gerekiyorsa, o şekilde yazmam daha iyi olabilir. Bu arada, standart yaklaşık olarak 1500 sayfa uzunluğunda.<br><br>
  <b>Kesinlikle C++'ı suçlamaya çalışmıyorum.</b> Dil seviyorum. Sadece artık yoruldum.
</details>


## İş mantığı ve HTTP durum kodları
Arka planda şunları döndürüyoruz:
- `401` süresi dolmuş JWT token için
- `403` yeterli erişim olmadığı için
- `418` yasaklanmış kullanıcılar için

Ön taraftaki ekip, giriş işlevselliğini uygulamak için backend API'sini kullanıyor. Geçici olarak aşağıdaki bilişsel yükü oluşturmalılar:
- `401`, süresi dolmuş JWT token için // `🧠+`, tamam sadece geçici olarak hatırlayın
- `403`, yeterli erişim için // `🧠++`
- `418`, yasaklanmış kullanıcılar için // `🧠+++`

Ön taraftaki geliştiriciler (umuyorum ki) sayısal durum -> anlam sözlüğü gibi bir şey oluşturacaklar, böylece sonraki katkı sağlayıcılarının bu eşleştirmeyi yeniden oluşturması gerekmez.

Sonra test ekibi devreye giriyor:
"Hey, `403` durumu aldım, bu süresi dolmuş token mı yoksa yeterli erişim mi?"

**Test ekibi, test etmeye doğrudan geçemiyor, çünkü önce backend tarafında yapılan bilişsel yükü yeniden oluşturmak zorundalar.**

Neden bu özel eşlemeyi çalışma belleğimizde tutalım? İş detaylarınızı HTTP iletim protokolünden soyutlamak ve yanıt gövdesinde doğrudan kendini açıklayan kodları döndürmek daha iyidir:
```json
{
    "code": "jwt_has_expired"
}
```
Ön taraftaki bilişsel yük: 🧠 (taze, zihin boş)
Test ekibindeki bilişsel yük: 🧠

Veritabanında veya başka yerlerdeki sayısal durumlar için aynı kural geçerlidir - kendini açıklayan dizeleri tercih edin. Bellek optimizasyonu için 640K bilgisayarlar çağında değiliz.

>İnsanlar 401 ve 403 arasında tartışma zamanı harcıyor, anlama düzeylerine göre seçimler yapıyorlar. Ama sonunda hiçbir anlam ifade etmiyor. Hataları kullanıcıyla ilgili veya sunucuyla ilgili olarak ayırabiliriz, ama bundan başka her şey biraz belirsiz. Bu gizemli "RESTful API"yi takip etmek ve çeşitli HTTP fiillerini ve durumlarını kullanmak gibi bir standardın gerçekte var olmadığını söylemek gerekir. Konu hakkında tek geçerli belge, 2000 yılına dayanan Roy Fielding'in yayınladığı bir makaledir ve fiiller ve durumlar hakkında hiçbir şey söylemez. İnsanlar sadece birkaç temel HTTP durumu ve sadece POST işlemleri ile de idare ediyorlar ve işleri gayet yolunda gidiyor.

P.S. "Kimlik doğrulama" ve "yetkilendirme" arasındaki ayrımı yapmak genellikle zihinsel olarak yorucu olabilir. Bilişsel yükü azaltmak için "giriş" ve "izinler" gibi daha basit terimler kullanabiliriz.

## DRY prensibini kötüye kullanma

"Kendini tekrar etme" - yazılım mühendisi olarak öğretilen ilk prensiplerden biridir. Bu prensip, genelde iyi ve temel bir kural olmasına rağmen, aşırı kullanıldığında bizleri kaldıramayacağımız bilişsel yüklerle karşı karşıya bırakabilir.

Günümüzde, herkes mantıksal olarak ayrılmış bileşenlere dayalı yazılımlar inşa ediyor. Bu bileşenler genellikle ayrı hizmetleri temsil eden birden fazla kod tabanına dağılmış durumda. Her türlü tekrarı ortadan kaldırmaya çalışırken, ilgisiz bileşenler arasında sıkı bağlar oluşturma riski taşırsınız. Bir bölümde yapılan değişiklikler, görünüşte ilgisiz diğer alanlarda istenmeyen sonuçlara yol açabilir. Ayrıca, bireysel bileşenleri değiştirmeyi veya değiştirmeyi mümkün kılmadan tüm sistemi etkilemeye başlayabilir. `🤯`

Aslında, aynı sorun tek bir modül içinde bile ortaya çıkabilir. Algılanan benzerliklere dayanarak çok erken ortak işlevsellik çıkarabilirsiniz, ancak uzun vadede gerçekte var olmayabilirler. Bu gereksiz soyutlamaların, değiştirmesi veya genişletmesi zor olan gereksiz soyutlamaların ortaya çıkmasına neden olabilir.

Rob Pike bir zamanlar şöyle demişti:

> Biraz kopyalama, biraz bağımlılıktan daha iyidir.

Bu prensibin kötüye kullanımı, dolaylı bağlantılar (veya gereksiz bağlantılar), erken soyutlamalar ve büyük, genel çözümler, bakım karmaşıklığı ve yüksek bilişsel yük ile sonuçlanabilir.

## Bir çerçeve ile sıkı bağlantı
Çerçeveler (örneğin, **Spring**, **Django** gibi) kendi hızlarında evrilir, ki çoğu durumda bu bizim projemizin yaşam döngüsü ile uyuşmaz.

Çerçeveye aşırı güvenmekle, tüm gelecek geliştiricilerin ilk önce o çerçeveyi (veya özel sürümünü) öğrenmesini zorunlu kılıyoruz. Çerçeveler bize MVP'leri (Minimum Viable Product - En Az Viable Ürün) günler içinde başlatma imkanı verse de, uzun vadede gereksiz karmaşıklık ve bilişsel yük eklemeye eğilimlidirler.

Dahası, bir noktada çerçeveler, mimariye uymayan yeni bir gereksinimle karşılaşıldığında önemli bir kısıtlama haline gelebilir. Bundan sonra insanlar, çerçeveyi çatallayarak kendi özel sürümlerini sürdürmeye başlarlar. Yeni bir katılımcının herhangi bir değer sunabilmesi için (yani bu özel çerçeveyi öğrenmesi için) oluşturması gereken bilişsel yük miktarını düşünün. `🤯`

**Kesinlikle her şeyi sıfırdan icat etmemizi savunmuyoruz!**

Kodumuzu nispeten çerçeve-bağımsız bir şekilde yazabiliriz. İş mantığı çerçevede değil, onun bileşenlerini kullanmalıdır. Çerçeveyi temel mantığınızın dışına yerleştirin. Çerçeveyi kitaplık gibi kullanın. Bu, yeni katılımcıların çerçeveyle ilgili karmaşıklık kalıntılarını gözden geçirmeden ilk günden itibaren katkı sağlamasına olanak tanır.

## Hexagonal/Onion mimarisi
Bununla ilgili bazı mühendislik heyecanları var.

Ben yıllar boyunca **Onion Mimarisi**'nin tutkulu bir savunucusuydum. Bunu burada ve şurada kullandım ve diğer ekipleri de teşvik ettim. Projelerimizin karmaşıklığı arttı

## DDD
Alan odaklı tasarım (Domain-driven design - DDD), bazı harika noktalara sahiptir, ancak genellikle yanlış yorumlanır. İnsanlar "Biz DDD'de kod yazıyoruz" derler, bu biraz garip çünkü DDD, çözüm alanı değil, sorun alanıyla ilgilidir.

Evrensel dil, alan, sınırlı bağlam, birleşik, olay fırtınası hepsi sorun alanıyla ilgilidir. Bu unsurların amacı, alan hakkında içgörüler kazanmamıza ve sınırları çıkarmamıza yardımcı olmaktır. DDD, geliştiricilerin, alan uzmanlarının ve iş insanlarının tek bir birleşik dil kullanarak etkili iletişim kurmalarını sağlar. Ancak DDD'nin bu sorun alanı yönlerine odaklanmak yerine, genellikle belirli klasör yapıları, servisler, depolar ve diğer çözüm alanı tekniklerini vurgularız.

Muhtemelen DDD'yi nasıl yorumladığımız benzersiz ve öznel olma eğilimindedir. Ve eğer bu anlayış üzerine kod inşa edersek, yani gereksiz bilişsel yük oluşturursak - gelecekteki geliştiriciler mahvolur. `🤯`

## Devlerden Öğrenmek
En büyük teknoloji şirketlerinden birinin genel tasarım prensiplerine bir göz atın:
- **Netlik**: Kodun amacı ve mantığı okuyucu için açıkça anlaşılır.
- **Basitlik**: Kod, hedefini en basit şekilde gerçekleştirir.
- **Özlülük**: Kod, ilgili detayları ayırt etmek kolaydır ve isimlendirme ile yapı, okuyucuyu bu detaylar boyunca yönlendirir.
- **Bakım Kolaylığı**: Kodun gelecekteki bir programcı tarafından doğru bir şekilde değiştirilmesi kolaydır.
- **Tutarlılık**: Kod, geniş kod tabanıyla tutarlıdır.

Yeni moda kelime bu prensiplere uyuyor mu? Yoksa sadece gereksiz bilişsel yük oluşturuyor mu?

<details>
  <summary><b>Burada eğlenceli bir resim var</b></summary>
  <img src="img/complexity.png"><br>
  Kod Karmaşıklığı vs. Deneyim, <a href="https://twitter.com/flaviocopes">@flaviocopes</a> tarafından
</details>

> Hata ayıklama, ilk olarak kodu yazmaktan iki kat daha zordur. Dolayısıyla, kodu mümkün olduğunca akıllıca yazarsanız, tanım gereği, onu hata ayıklamak için yeterince akıllı değilsinizdir.
> **Brian Kernighan**

## Conclusion
*Zihinsel yükün anlama ve problem çözme alanındaki karmaşık ve çok yönlü doğası, karmaşıklıkları yönetmek ve zihinsel kapasite tahsisini optimize etmek için dikkatli ve stratejik bir yaklaşım gerektirir.* `🤯`

Hissettiniz mi? Yukarıdaki ifadeyi anlamak zor, değil mi? Kafanızda gereksiz bir bilişsel yük oluşturduk. **İş arkadaşlarınıza bunu yapmayın.**

![Smart Author](/img/smartauthorv5.png)

Yaptığımız işin içsel doğasından kaynaklanan bilişsel yükü mümkün olan en aza indirmeliyiz.

---

Orjinal Yazı Sahibi Takip et: [Twitter](https://twitter.com/zakirullin), [GitHub](https://github.com/zakirullin) veya bağlantı kur: [LinkedIn](https://www.linkedin.com/in/zakirullin/)
