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

**[Devam Edecek]**
## Too many shallow microservices
We can apply the above scale-agnostic principle to microservices architecture as well. Too many shallow microservices won't do any good - the industry is heading towards somewhat "macroservices", i.e., services that aren't that shallow. One of the worst and hardest to fix phenomena is so-called distributed monolith, which is often the result of this overly granular shallow separation.

I once consulted a startup where a team of three developers introduced 17(!) microservices. They were 10 months behind schedule and appeared nowhere close to the public release. Every new requirement led to changes in 4+ microservices. Diagnostic difficulty in integration space skyrocketed. Both time to market and cognitive load were unacceptably high. `🤯`  

Is this the right way to approach the uncertainty of a new system? It's enormously difficult to elicit the right logical boundaries in the beginning, and by introducing too many microservices we make things worse. The team's only justification was: "The F(M)AANG companies proved microservices architecture to be effective". *Hello, you got to stop dreaming big.*

Checkout the [Tanenbaum–Torvalds debate](https://en.wikipedia.org/wiki/Tanenbaum%E2%80%93Torvalds_debate). The claim suggested that Linux's monolithic design was flawed and obsolete, and that a microkernel architecture should be used instead. Indeed, the microkernel design seemed to be superior "from a theoretical and aesthetical" point of view. Three decades on, microkernel-based GNU Hurd is still in development, and monolithic Linux is everywhere - this page is powered by Linux, your smart teapot is powered by Linux.  

A well-crafted monolith with truly isolated modules is often much more flexible than a bunch of microservices. It also requires far less cognitive effort to maintain. It's only when the need for separate deployments becomes crucial (e.g. development team scaling) that you should consider adding a network layer between the modules (future microservices).

## Feature-rich languages
We feel excited when new features got released in our favourite language. We spend some time learning these features, we build code upon them.

If there are lots of features, we may spend half an hour playing with a few lines of code, to use one or another feature. And it's kind of a waste of time. But what's worse, **when you come back later, you would have to recreate that thought process!** `🤯`

**You not only have to understand this complicated program, you have to understand why a programmer decided this was the way to approach a problem from the features that are available.**  

These statements are made by none other than Rob Pike.

> Reduce cognitive load by limiting the number of choices.  

Language features are OK, as long as they are orthogonal to each other.

<details>
  <summary><b>Thoughts from an engineer with 20+ years of C++ experience ⭐️</b></summary>
  <br>
  I was looking at my RSS reader the other day and noticed that I have somewhat three hundred unread articles under the "C++" tag. I haven't read a single article about the language since last summer, and I feel great!<br><br>
  I've been using C++ for 20 years for now, that's almost two-thirds of my life. Most of my experience lies in dealing with the darkest corners of the language (such as undefined behaviours of all sorts). It's not a reusable experience, and it's kind of creepy to throw it all away now.<br><br>
  Like, can you imagine, the token <code>||</code> has a different meaning in <code>requires ((!P&lt;T&gt; || !Q&lt;T&gt;))</code> and in <code>requires (!(P&lt;T&gt; || Q&lt;T&gt;))</code>. The first is the constraint disjunction, the second is the good-old logical OR operator, and they behave differently.<br><br>
  You can't allocate space for a trivial type and just <code>memcpy</code> a set of bytes there without extra effort - that won't start the lifetime of an object. This was the case before C++20. It was fixed in C++20, but the cognitive load of the language has only increased.<br><br>
  Cognitive load is constantly growing, even though things got fixed. I should know what was fixed, when it was fixed, and what it was like before. I am a professional after all. Sure, C++ is good at legacy support, which also means that you <b>will face</b> that legacy. For example, last month a colleague of mine asked me about some behaviour in C++03. <code>🤯</code><br><br>
  There were 20 ways of initialization. Uniform initialization syntax has been added. Now we have 21 ways of initialization. By the way, does anyone remember the rules for selecting constructors from the initializer list? Something about implicit conversion with the least loss of information, <i>but if</i> the value is known statically, then... <code>🤯</code><br><br>
  <b>This increased cognitive load is not caused by a business task at hand. It is not an intrinsic complexity of the domain. It is just there due to historical reasons</b> (<i>extraneous cognitive load</i>).<br><br>
  I had to come up with some rules. Like, if that line of code is not as obvious and I have to remember the standard, I better not write it that way. The standard is somewhat 1500 pages long, by the way.<br><br>
  <b>By no means I am trying to blame C++.</b> I love the language. It's just that I am tired now.
</details>


## Business logic and HTTP status codes
On the backend we return:  
`401` for expired jwt token  
`403` for not enough access  
`418` for banned users  

The guys on the frontend use backend API to implement login functionality. They would have to temporarily  create the following cognitive load in their brains:  
`401` is for expired jwt token // `🧠+`, ok just temporary remember it  
`403` is for not enough access // `🧠++`  
`418` is for banned users // `🧠+++`  

Frontend developers would (hopefully) introduce some kind `numeric status -> meaning` dictionary on their side, so that subsequent generations of contributors wouldn't have to recreate this mapping in their brains.

Then QA people come into play:
"Hey, I got `403` status, is that expired token or not enough access?"
**QA people can't jump straight to testing, because first they have to recreate the cognitive load that the guys on the backend once created.**

Why hold this custom mapping in our working memory? It's better to abstract away your business details from the HTTP transfer protocol, and return self-descriptive codes directly in the response body:
```json
{
    "code": "jwt_has_expired"
}
```

Cognitive load on the frontend side: `🧠` (fresh, no facts are held in mind)  
Cognitive load on the QA side: `🧠`

The same rule applies to all sorts of numeric statuses (in database or wherever) - prefer self-describing strings. We are not in the era of 640K computers to optimise for memory.  

> People spend time arguing between `401` and `403`, making choices based on their level of understanding. But in the end it just doesn't make any sense. We can separate errors into either user-related or server-related, but apart from that, things are kind of blurry. As for following this mystical "RESTful API" and using all sorts of HTTP verbs and statuses, the standard simply doesn't exist. The only valid document on the matter is a paper published by Roy Fielding, dated back in 2000, and it says nothing about verbs and statuses. People get along with just a few basic HTTP statuses and POSTs only, and they are doing just fine.

P.S. It's often mentally taxing to distinguish between "authentication" and "authorization". We can use simpler terms like ["login" and "permissions"](https://ntietz.com/blog/lets-say-instead-of-auth/) to reduce the cognitive load.

## Abusing DRY principle

Do not repeat yourself - that is one of the first principles you are taught as a software engineer. It is so deeply embedded in ourselves that we can not stand the fact of a few extra lines of code. Although in general a good and fundamental rule, when overused it leads to the cognitive load we can not handle.

Nowadays, everyone builds software based on logically separated components. Often those are distributed among multiple codebases representing separate services. When you strive to eliminate any repetition, you might end up creating tight coupling between unrelated components. As a result changes in one part may have unintended consequences in other seemingly unrelated areas. It can also hinder the ability to replace or modify individual components without impacting the entire system. `🤯`  

In fact, the same problem arises even within a single module. You might extract common functionality too early, based on perceived similarities that might not actually exist in the long run. This can result in unnecessary abstractions that are difficult to modify or extend.  

Rob Pike once said:

> A little copying is better than a little dependency.  

We are tempted to not reinvent the wheel so strong that we are ready to import large, heavy libraries to use a small function that we could easily write by ourselves. It introduces unnecessary dependencies and bloated code. Make informed decisions about when to import external libraries and when it is more appropriate to write concise, self-contained code snippets to accomplish smaller tasks.

Abuse of this principle could lead to indirect coupling (or just unnecessary coupling), premature abstractions and large, generic solutions, maintenance complexity, high cognitive load.

## Tight coupling with a framework
Frameworks evolve at their own pace, which in most cases doesn't match the lifecycle of our project.

By relying too heavily on a framework, we force all upcoming developers to learn that framework first (or its particular version). Even though frameworks enable us to launch MVPs in a matter of days, in the long run they tend to add unnecessary complexity and cognitive load.

Worse yet, at some point frameworks can become a significant constraint when faced with a new requirement that just doesn't fit the architecture. From here onwards people end up forking a framework and maintaining their own custom version. Imagine the amount of cognitive load a newcomer would have to build (i.e. learn this custom framework) in order to deliver any value. `🤯`

**By no means do we advocate to invent everything from scratch!**

We can write code in a somewhat framework-agnostic way. The business logic should not reside within a framework; rather, it should use the framework's components. Put a framework outside of your core logic. Use the framework in a library-like fashion. This would allow new contributors to add value from day one, without the need of going through debris of framework-related complexity first.

## Hexagonal/Onion architecture
There is a certain engineering excitement about all this stuff.

I myself was a passionate advocate of Onion Architecture for years. I used it here and there and encouraged other teams to do so. The complexity of our projects went up, the sheer number of files alone had doubled. It felt like we were writing a lot of glue code. On ever changing requirements we had to make changes across multiple layers of abstractions, it all became tedious. `🤯`

Jumping from call to call to read along and figure out what goes wrong and what is missing is a vital requirement to quickly solve problem. With this architecture’s layer uncoupling it requires an exponential factor of extra, often disjointed, traces to get to the point where the failure occurs. Every such trace takes space in our limited working memory. `🤯`  

This architecture was something that made intuitive sense at first, but every time we tried applying it to projects it made a lot more harm than good. In the end, we gave it all up in favour of the good old dependency inversion principle. **No port/adapter terms to learn, no unnecessary layers of horizontal abstractions, no extraneous cognitive load.**

> Do not add layers of abstractions for the sake of an architecture. Add them whenever you need an extension point that is justified for practical reasons. **[Layers of abstraction aren't free of charge](https://blog.jooq.org/why-you-should-not-implement-layered-architecture), they are to be held in our working memory**.  

Even though these layered architectures have accelerated an important shift from traditional database-centric applications to a somewhat infrastructure-independent approach, where the core business logic is independent of anything external, the idea is by no means novel.  

These architectures are not fundamental, they are just subjective, biased consequences of more fundamental principles. Why rely on those subjective interpretations? Follow the fundamentals instead: dependency inversion principle, isolation, single source of truth, true invariant, complexity, cognitive load and information hiding.

[Discussion](https://github.com/zakirullin/cognitive-load/discussions/24)

## DDD
Domain-driven design has some great points, although it is often misinterpreted. People say "We write code in DDD", which is a bit strange, because DDD is about problem space, not about solution space.

Ubiquitous language, domain, bounded context, aggregate, event storming are all about problem space. They are meant to help us learn the insights about the domain and extract the boundaries. DDD enables developers, domain experts and business people to communicate effectively using a single, unified language. Rather than focusing on these problem space aspects of DDD, we tend to emphasise particular folder structures, services, repositories, and other solution space techniques. 

Chances are that the way we interpret DDD is likely to be unique and subjective. And if we build code upon this understanding, i.e., if we create a lot of extraneous cognitive load - future developers are doomed. `🤯`

## Learning from the Giants
Take a look at the overarching design principles of one of the biggest tech companies:  
`Clarity`: The code’s purpose and rationale is clear to the reader.  
`Simplicity`: The code accomplishes its goal in the simplest way possible.  
`Concision`: The code is easy to discern the relevant details, and the naming and structure guide the reader through these details.  
`Maintainability`: The code is easy for a future programmer to modify correctly.  
`Consistency`: The code is consistent with the broader codebase.  

Does the new fancy buzzword comply with these principles? Or all it does is creating extraneous cognitive load?

<details>
  <summary><b>Here's a fun picture</b></summary>
  <img src="img/complexity.png"><br>
  Code Complexity vs. Experience from <a href="https://twitter.com/flaviocopes">@flaviocopes</a>
</details>

> Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.  
> **Brian Kernighan**

## Conclusion
*The intricate and multifaceted nature of cognitive load within the realm of comprehension and problem-solving necessitates a diligent and strategic approach in order to navigate the complexities and optimize mental capacity allocation.* `🤯`  

Do you feel it? The above statement is difficult to understand. We have just created an unnecessary cognitive load in your head. **Do not do this to your colleagues.**

![Smart Author](/img/smartauthorv5.png)

We should reduce any cognitive load above and beyond what is intrinsic to the work we do. 

---
Follow on [Twitter](https://twitter.com/zakirullin), [GitHub](https://github.com/zakirullin) or connect on [LinkedIn](https://www.linkedin.com/in/zakirullin/)
