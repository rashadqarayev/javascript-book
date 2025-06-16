# Fəsil 5: Statements (Əmrlər)

4-cü fəsildə **ifadələr** JavaScript-də “ifadələr” (phrases) kimi izah olunurdu. Eyni analogiya ilə, **statements** isə JavaScript-in “cümlələri” və ya **əmrləri** kimidir.

İngilis dilində cümlələr nöqtə (`.`) ilə bitdiyi kimi, JavaScript-də də əmrlər **nöqtəli vergül** (`;`) ilə tamamlanır.

**İfadələr** bir **dəyər** qaytarır,
**Əmrlər (statements)** isə **hər hansı bir hadisəni** həyata keçirir.

---

### İfadə Əmrləri (Expression Statements)

Bəzi ifadələr **side effect** (yəni proqramın vəziyyətində dəyişiklik) yaradır. Məsələn:

* dəyişənə qiymət təyin etmək (assignment)
* funksiyanı çağırmaq (function invocation)

Bu cür ifadələr təkbaşına istifadə olunduqda, onlar **ifadə əmri** (expression statement) adlanır.

---

### Təyinat Əmrləri (Declaration Statements)

Bu əmrlər yeni dəyişən və ya funksiya **elan etmək (declare)** üçün istifadə olunur:

* `let`, `const`, `var` — dəyişənlər üçün
* `function` — funksiyalar üçün

---

### JavaScript Proqramları Necə Qurulur?

JavaScript proqramları sadəcə **əmrlər ardıcıllığından** ibarətdir.

1. Proqramdakı əmrlər **yuxarıdan aşağıya doğru**, **sıra ilə** icra olunur.
2. Lakin bu ardıcıllığı dəyişmək də mümkündür — bunun üçün müxtəlif **idarəetmə strukturları (control structures)** mövcuddur.

---

### Əsas İdarəetmə Struktur Tipli Əmrlər:

#### ✅ Şərti Əmrlər (Conditionals)

Bu tip əmrlər ifadənin nəticəsindən asılı olaraq digər əmrləri ya **icra edir**, ya da **ötürür**:

* `if`
* `switch`

#### 🔁 Dövr Əmrləri (Loops)

Əmrləri **dəfələrlə təkrarlamaq** üçün istifadə olunur:

* `while`
* `for`

#### 🔀 Tullanma Əmrləri (Jump Statements)

Proqramın axışını başqa istiqamətə **yönləndirmək** üçün istifadə olunur:

* `break`
* `return`
* `throw`

---

> JavaScript proqramı — sadəcə **əmrlər ardıcıllığıdır**.
> Hər bir əmr **nöqtəli vergül** (`;`) ilə tamamlanır.
> Bu əmrləri öyrəndikdən sonra **öz JavaScript proqramlarını yazmağa** başlaya bilərsən!

---
## 5.1 İfadə Əmrləri (Expression Statements)

JavaScript-də ən sadə əmrlər — **side effect** (yan təsir) yaradan **ifadələr**dir. Bu cür ifadələr haqqında **4-cü fəsildə** də məlumat verilmişdi.

---

### ✅ Təyinat (Assignment) Əmrləri

Ən məşhur ifadə əmrlərindən biri **təyinat (assignment)** əmrləridir. Məsələn:

```js
greeting = "Salam " + name;
i *= 3;
```

---

### ✅ Artırma/Azaltma Operatorları: `++` və `--`

Bu operatorlar da əslində **təyinat əməli** kimi davranır: dəyişənin dəyərini dəyişirlər:

```js
counter++;
```

Bu əmrlər **sadəcə dəyər qaytarmır**, həm də **dəyişəni dəyişdirir** — yəni **yan təsir (side effect)** yaradır.

---

### ✅ `delete` Operatoru

`delete` operatorunun məqsədi bir obyektin xüsusiyyətini **silməkdir**. Buna görə də bu operator **demək olar ki, həmişə ayrıca əmr** kimi istifadə olunur:

```js
delete o.x;
```

---

### ✅ Funksiya Çağırışları (Function Calls)

Funksiya çağırışları da çox vaxt **ifadə əmri** kimi istifadə olunur, çünki onlar:

* ya **host mühitinə təsir edir** (məsələn, konsola çıxış),
* ya da proqramın **daxili vəziyyətini dəyişir**.

```js
console.log(debugMessage);
displaySpinner(); // Tutaq ki, bu funksiya veb tətbiqdə yükləmə ikonunu göstərir
```

Bu funksiyalar bir **dəyər** qaytara bilər, lakin əsas məqsəd — **hər hansı bir hadisənin baş verməsi**, yəni **yan təsir yaratmaqdır**.

Əgər bir funksiyanın heç bir yan təsiri yoxdursa, onu sadəcə çağırmaq **mənasızdır**:

```js
Math.cos(x); // Mənasızdır, çünki nəticə istifadə olunmur
```

Lakin həmin nəticə **saxlanılır və istifadə olunursa**, bu zaman **mənalı** olur:

```js
cx = Math.cos(x); // Dəyəri dəyişəndə saxladıq
```

---

## 5.2 Mürəkkəb və Boş Əmrlər (Compound and Empty Statements)

### 🧱 Mürəkkəb Əmr (Compound Statement)

**Vergül operatoru** bir neçə ifadəni birləşdirib tək ifadəyə çevirdiyi kimi, **əmr blokları** da bir neçə əmri birləşdirərək **tək əmr** kimi istifadə olunmasına imkan verir.

Əmr bloku — sadəcə **süslü mötərizələr** (`{}`) içində yazılmış **əmrlər ardıcıllığıdır**. Məsələn:

```js
{
  x = Math.PI;
  cx = Math.cos(x);
  console.log("cos(π) = " + cx);
}
```

Bu blok — **tək bir əmr** kimi istənilən yerdə istifadə oluna bilər.

---

#### 📌 Nəzərə Alın:

1. **Blok özü nöqtəli vergüllə bitmir.**
   — Blok daxilindəki əmrlər `;` ilə bitir, lakin `block` özü `;` ilə tamamlanmır.

2. **Daxili sətrlər adətən indent (boşluq) ilə yazılır.**
   — Bu məcburi olmasa da, kodun oxunaqlığını artırır.

---

### 🔁 Alt-Əmrlər (Substatements)

Bir çox JavaScript əmrinin daxilində başqa bir əmr olur. Məsələn, `while` dövrü yalnız **bir alt-əmr** tələb edir:

```js
while (i < 10)
  i++;
```

Lakin əgər **bir neçə əmr** yazmaq lazımdırsa, onları `{}` içində birləşdirib **mürəkkəb əmr** halına salmaq lazımdır:

```js
while (i < 10) {
  console.log(i);
  i++;
}
```

---

### ❌ Boş Əmr (Empty Statement)

**Boş əmr** — heç bir əmrin olmadığı, yalnız `;` işarəsindən ibarət bir sətrdir:

```js
;
```

Bu, JavaScript tərəfindən **ötürülür** — yəni heç bir iş görmür. Nadir hallarda, bəzi dövrlərdə bu faydalı ola bilər.

#### Məsələn:

```js
for (let i = 0; i < a.length; a[i++] = 0) ;
```

Bu `for` dövründə bütün iş `a[i++] = 0` ifadəsində görülür. Dövr bədəninə ehtiyac yoxdur. Lakin JavaScript sintaksisində dövr üçün mütləq bir **bədən (statement)** tələb olunduğuna görə, **boş əmr** (`;`) istifadə olunur.

---

### ⚠️ Diqqət: Səhvən Yazılmış `;`

Aşağıdakı kimi **səhvən qoyulmuş nöqtəli vergül** kodun məntiqini poza bilər:

```js
if ((a === 0) || (b === 0)); // Ups! Bu sətr heç nə etmir
  o = null; // Bu isə HƏR ZAMAN icra olunur
```

Yəni `if` heç bir şərti yoxlamır, `o = null` isə istisnasız şəkildə işləyir.

#### ✅ Doğru istifadə üçün belə şərh yazmaq məqsədəuyğundur:

```js
for (let i = 0; i < a.length; a[i++] = 0) /* boş dövr */ ;
```

Belə **şərhlər (comment)** kod oxuyan şəxsə göstərir ki, boş əmr məqsədli şəkildə yazılıb

---

## 5.3 Şərt Əmrləri (Conditionals)

**Şərt əmrləri** (`conditional statements`) — ifadənin nəticəsinə əsasən müəyyən əmrləri **icra edən və ya ötürən** strukturlardır.

Bu cür əmrlər proqramın **qərar vermə nöqtələridir** və bəzən **"budaqlanma (branching)"** əmrləri adlanır.

JavaScript interpretatorunun kod boyunca addım-addım getdiyini təsəvvür etsək, **şərt əmrləri** onun **hansı istiqamətə dönəcəyini** müəyyənləşdirir.

Bu bölmədə aşağıdakılar izah olunur:

* Əsas `if/else` əmri
* Daha mürəkkəb və çoxyollu `switch` əmri

---

## 5.3.1 `if`

`if` — JavaScript-dəki ən sadə və əsas **nəzarət strukturudur**. Onun vasitəsilə **şərtə əsasən əmrlərin icrası** təmin olunur.

---

### `if` Əmri — Sadə Forma:

```js
if (şərt)
  əməl;
```

* Əgər `şərt` **doğru** (`truthy`) dəyər qaytarırsa, `əməl` **icra olunur**.
* Əgər `şərt` **yanlış** (`falsy`) dəyər qaytarırsa, `əməl` **ötürülür**.

#### Nümunə:

```js
if (username == null)
  username = "John Doe";
```

və ya daha qısa formada:

```js
if (!username)
  username = "John Doe";
```

Bu kod `username` dəyişəni boş, `null`, `undefined`, `0`, `""` və ya `NaN` olduqda yeni dəyər verir.

📌 **Qeyd:** `if` şərtindəki `()` mötərizələri **məcburidir**.

---

### `if` — Bloklu Forma:

Bir neçə əmri birlikdə `if` şərti daxilində icra etmək istəyirsənsə, əmrləri `{}` içində yazmalısan:

```js
if (!address) {
  address = "";
  message = "Zəhmət olmasa ünvanı qeyd edin.";
}
```

---

### `if/else` Əmri — İkiyollu Forma:

```js
if (şərt)
  əməl1;
else
  əməl2;
```

* Əgər `şərt` doğrudursa → `əməl1` icra olunur.
* Əks halda → `əməl2` icra olunur.

#### Nümunə:

```js
if (n === 1)
  console.log("1 yeni mesajınız var.");
else
  console.log(`${n} yeni mesajınız var.`);
```


---

### ⚠️ `else` ilə iç-içə `if` — Başqa bir nümunə

```js
let user = "admin";
let isLoggedIn = false;

if (user === "admin")
  if (isLoggedIn)
    console.log("Panelə giriş icazəsi verildi");
  else
    console.log("Zəhmət olmasa daxil olun"); // Problem haradadır?
```

Bu kodda `else` yalnız `isLoggedIn` şərtinə aiddir. Yəni `user === "admin"` olsa da, `isLoggedIn` false-dursa, "Zəhmət olmasa daxil olun" mesajı görünəcək.
Amma sənin niyyətin belə ola bilərdi ki: **əgər user "admin" deyilsə**, bu mesaj yazılsın.

---

### ✅ Qarışıqlığın qarşısını alan düzgün forma:

```js
if (user === "admin") {
  if (isLoggedIn) {
    console.log("Panelə giriş icazəsi verildi");
  }
} else {
  console.log("Zəhmət olmasa daxil olun");
}
```

İndi isə `else` artıq `user === "admin"` şərtinə aiddir, və bu daha aydındır.

---

Əksər proqramçılar **həmişə** `if`, `else`, `while` və digər strukturlarda əmrləri **blok şəklində** (`{}` içində) yazır, hətta **tək sətrlik** olsa belə:

```js
if (x > 0) {
  console.log("Müsbət ədəddir.");
}
```

Bu vərdiş:

* Qarışıqlığın qarşısını alır
* Kodun oxunuşunu və dəyişdirilməsini asanlaşdırır

---

## 🔍 Əlavə Nümunələr

#### 1. Sadə `if/else`:

```js
let age = 20;

if (age >= 18) {
  console.log("Siz yetkinsiniz.");
} else {
  console.log("Siz hələ uşaqsınız.");
}
```

#### 2. İki `if` ard-arda:

```js
if (score > 90) {
  console.log("Əla!");
}

if (score < 50) {
  console.log("Zəif nəticə.");
}
```

#### 3. `if` ilə `return`:

```js
function checkPassword(pw) {
  if (pw.length < 6) {
    return "Şifrə çox qısadır!";
  }

  return "Qəbul edildi.";
}
```

---

## 5.3.2 `else if`

Əvvəlki `if/else` strukturunda — **yalnız iki vəziyyətdən** birini icra etmək mümkün idi:
→ şərt doğrudursa `if`,
→ əks halda `else`.

Bəs **bir neçə fərqli şərtə** görə **fərqli əmrlər** icra etmək lazım olanda nə etməli?

---

### ✅ Həll yolu: `else if`

Əslində `else if` — JavaScript-də ayrıca bir əməl **deyil**,
sadəcə **tez-tez istifadə olunan bir yazı nümunəsidir (idiom)**.
Bu, birdən çox `if/else` blokunun **ardıcıl və səliqəli yazılmasıdır**.

---

### 🎯 Nümunə: İstifadəçinin Yaşına görə Kateqoriya

```js
let age = 25;

if (age < 13) {
  console.log("Uşaq");
} else if (age >= 13 && age < 18) {
  console.log("Yeniyetmə");
} else if (age >= 18 && age < 65) {
  console.log("Böyüklər");
} else {
  console.log("Pensiya yaşında");
}
```

**İzah:**

* `age < 13` olduqda: "Uşaq"
* `13–17` yaş aralığında: "Yeniyetmə"
* `18–64` yaş aralığında: "Böyüklər"
* `65+` olduqda: "Pensiya yaşında"


---

### 💡 Bərbad nümunə (bunu etmə!):

Aşağıdakı kod **tam sintaktik olaraq doğrudur**, amma **oxumaq çətindir**:

```js
if (user.role === "admin") {
  // Admin üçün panel
} else {
  if (user.role === "editor") {
    // Editor üçün alətlər
  } else {
    if (user.role === "viewer") {
      // Yalnız baxış rejimi
    } else {
      // Naməlum istifadəçi rolu
    }
  }
}

```

Bu, **tam iç-içə (`nested`)** formada yazılıb. Amma `else if` ilə bu eyni kod **daha aydın** şəkildə ifadə olunur.

---

### 🔁 Xülasə:

* `else if` → bir çox şərtə görə fərqli bloklar işlətməyə imkan verir.
* Daha **modul**, **oxunaqlı** və **sadə** yazılışdır.
* **İç-içə `else { if { ... } }` yazmaq yerinə `else if` istifadə et!**

---

## 5.3.3 `switch` – çoxyönlü şərtləri daha rahat yazmaq üçün

Bir `if` ifadəsi proqramın axışında budaqlanma (yəni şaxələnmə) yaradır və `else if` istifadə etməklə çoxyönlü budaqlanmalar etmək mümkündür.

Ancaq belə bir çoxyönlü `if` blokları hər dəfə **eyni ifadəni** yoxlayırsa (məsələn: `age === 24`, `age === 25`, və s.), bu zaman həmin ifadənin **dəfələrlə təkrar yoxlanılması** artıq olur. Həm oxunaqlılığı azaldır, həm də performans baxımından səmərəsiz olur.

---

### Nə vaxt `switch` istifadə etməliyik?

Əgər **bütün şərtlər eyni ifadəyə (deyək ki, `n`) əsaslanırsa**, `switch` daha münasibdir. Çünki `switch` həmin ifadəni **yalnız bir dəfə** hesablayır və sonra onun nəticəsini `case`-lərlə müqayisə edir.

---

##  `switch` ifadəsinin sintaksisi:
```js
switch (ifadə) {
  case dəyər1:
    // buradakı kodlar ifadə === dəyər1 olduqda icra edilir
    break;

  case dəyər2:
    // ifadə === dəyər2 olduqda icra edilir
    break;

  default:
    // heç biri uyğun gəlmirsə, bu kod icra edilir
    break;
}
```

---

##  Əvvəlki `if/else` nümunəsi ilə müqayisə:

Əgər `if/else` belədirsə:

```js
let age = 25;

if (age < 13) {
  console.log("Uşaq");
} else if (age >= 13 && age < 18) {
  console.log("Yeniyetmə");
} else if (age >= 18 && age < 65) {
  console.log("Böyüklər");
} else {
  console.log("Pensiya yaşında");
}
```

Bu `switch` ilə belə yazılır:

```js
let age = 25;

switch (age) {
  case 5:
    console.log("5 yaşındasınız");
    break;
  case 10:
    console.log("10 yaşındasınız");
    break;
  case 18:
    console.log("18 yaşınız tamamdır, səs vermək hüququnuz var");
    break;
  case 25:
    console.log("25 yaşınız var, artıq gənc yetkinlikdə sayılırsınız");
    break;
  default:
    console.log("Yaşınız xüsusi kateqoriyaya aid deyil");
}
```
---

## ⚠️ `break` sözü niyə vacibdir?

`break` əmri `switch` blokundan çıxmağa kömək edir. Əgər `break` yazılmazsa, uyğun `case` tapıldıqdan sonra **növbəti bütün `case`-lər də icra olunur**. Buna **fall-through** deyilir.

Yəni:

```js
switch(n) {
  case 1:
    console.log("Bir");
  case 2:
    console.log("İki");
}
```

Əgər `n === 1` olarsa, həm `"Bir"`, həm də `"İki"` çap olunur — çünki `break` yoxdur!

Amma belə yazılsa:

```js
switch(n) {
  case 1:
    console.log("Bir");
    break;
  case 2:
    console.log("İki");
    break;
}
```

Yalnız uyğun `case` icra olunur və sonra `switch`-dən çıxılır.

> 🔸 Qeyd: Əgər `switch` bir funksiyanın içindədirsə, `break` əvəzinə `return` da istifadə oluna bilər. Hər ikisi `switch`-dən çıxmağı təmin edir.

---


## 💡 Əlavə qeydlər:

* Ən çox hallarda `case`-dən sonra **sabit dəyərlər** yazılır (`1`, `"string"`, və s.).
* Amma ECMAScript standartına görə, `case`-dən sonra **istənilən ifadə** (expression) yaza bilərsən.
* Uyğunluq **`===` ilə yoxlanılır**, `==` yox! Yəni **tip uyğunluğu da olmalıdır**..

---


# 5.4 Dövrlər (Loops)

Şərt ifadələrini başa düşmək üçün biz JavaScript tərcüməçisinin (interpreter) sənin kodunu oxuyarkən bir budaqlanma yolunu izlədiyini təsəvvür etmişdik. Dövrü (looping) ifadələr isə bu yolun öz-özünə qayıdıb kodun müəyyən hissəsini təkrar-təkrar işlətməsini təmin edir.

JavaScript-də beş növ dövr var:

* `while`
* `do/while`
* `for`
* `for/of` (və onun `for/await` variantı)
* `for/in`


---

## 5.4.1 `while`

`while` ifadəsi JavaScript-in ən əsas dövr operatorudur, elə `if` ifadəsi kimi.

Sintaksisi belədir:

```js
while (ifadə)
  ifadə_bloku
```

**İzahı:**

* Tərcüməçi (interpreter) əvvəlcə `ifadə`-ni qiymətləndirir (yəni yoxlayır).
* Əgər `ifadə` **false** (yalan) dəyər verirsə, o zaman dövrün bədəni (`ifadə_bloku`) atlanır və proqram növbəti əmri icra edir.
* Əgər `ifadə` **true** (doğru) dəyər verirsə, dövr bədəni icra olunur və sonra yenidən `ifadə` yoxlanılır.
* Bu proses `ifadə` yalan dəyər alana qədər davam edir.

---

### Dövrdə dəyişənlərin rolu

Adətən, hər dövrün sonunda bir və ya bir neçə dəyişən dəyişir. Bu dəyişənlər dövrün şərtində (`ifadə` hissəsində) iştirak edirsə, dövrün şərtinin nəticəsi hər dəfə dəyişə bilər və nəticədə dövr sona çata bilər.

Əgər dövr şərti həmişə doğru qalarsa, dövr heç vaxt bitməz.

---

### Misal: 0-dan 9-a qədər saydırmaq

```js
let count = 0;

while (count < 10) {
  console.log(count); // 0 1 2 3 4 5 7 8 9 
  count++;
}
```

**İzah:**

* `count` dəyişəni əvvəlcə 0-a bərabərdir.
* Dövrün şərti `count < 10` olduğu üçün `count` 10-dan kiçik olduğu müddətcə dövr işləyir.
* Dövr bədənində `count` çap olunur və sonra bir vahid artırılır.
* Nəhayət `count` 10 olanda, `count < 10` şərti yalana çevrilir və dövr bitir.

---

### Dövr dəyişənlərinin adlandırılması

* `count` kimi dəyişənlər dövr sayğacı (counter) adlanır.
* Tez-tez `i`, `j`, `k` kimi qısa adlar istifadə olunur.
* Lakin kodun anlaşılan olması üçün dəyişən adları daha təsviri olmalıdır.

---

## 5.4.2 `do/while`

`do/while` dövrü `while`-a oxşayır, amma fərqi odur ki, şərt dövrün **sonunda** yoxlanılır, yəni dövr bədəni ən azı bir dəfə icra olunur.

Sintaksisi:

```js
do
  ifadə_bloku
while (ifadə);
```

---

### İstifadəsi

* `do/while` dövrü `while`-dan daha az istifadə olunur, çünki çox vaxt dövrün ən azı bir dəfə icra olunacağı dəqiq deyil.
* Amma əgər sən mütləq dövr bədənini bir dəfə icra etmək istəyirsənsə, onda `do/while` yaxşı seçimdir.

---

Nümunə:

```js
let number;

do {
  number = prompt("0-dan böyük bir rəqəm daxil edin:");
} while (number <= 0);

console.log("Daxil etdiyiniz rəqəm:", number);
```

---

**İzah:**

* `do` bloku ən azı bir dəfə işə düşür, istifadəçidən rəqəm soruşur.
* `while` şərti yoxlayır: rəqəm 0-dan kiçik və ya bərabərdirsə, sorğu yenidən verilir.
* Beləliklə, düzgün rəqəm daxil edilənə qədər dövr təkrarlanır.

---

### Sintaksis fərqləri

* `do/while` dövründə həm `do` açar sözü, həm də `while` açar sözü olmalıdır.
* `do/while` dövrü **nöqtəli vergül (;)** ilə bitir.
* `while` dövründə isə bədən süslü mötərizələrdədirsə, nöqtəli vergül tələb olunmur.

---
## 5.4.3 `for` dövrü (loop)

`for` statement — dövr yaratmaq üçün istifadə olunur və çox vaxt `while`-dan daha rahatdır. Çünki burada loop-un əsas elementləri — **initialization**, **condition**, və **increment** — bir yerdə yazılır, ona görə də oxumaq və anlamaq daha asandır.

### Sintaksis:

```js
for (initialization; condition; increment) {
  // dövrün bədəni (loop body)
}
```

* **initialization** — dövr dəyişəninin (məsələn, sayğacın) başlanğıc dəyəri təyin edilir.
* **condition** — hər iterasiyadan əvvəl yoxlanılır, doğru (true) olduğu müddətcə dövr davam edir.
* **increment** — hər dövrün sonunda dəyişənin dəyəri artırılır və ya yenilənir.

### `for` dövrünün iş prinsipi:

1. **Initialization** bir dəfə icra olunur — dövr başlamazdan əvvəl.
2. **Condition** yoxlanılır.
3. Əgər **condition** doğru (true) olarsa, dövrün bədəni (statements) icra olunur.
4. Sonra **increment** icra olunur.
5. Yenidən **condition** yoxlanılır və proses təkrar olunur.

---

### `for` və `while` dövrlərinin bərabərliyi (equivalent):

```js
// for loop
for (let i = 0; i < 10; i++) {
  console.log(i); // 0 1 2 3 4 5 6 7 8 9
}

// eyni while loop
let i = 0;
while (i < 10) {
  console.log(i);
  i++;
}
```

---

### Birdən çox dəyişənlə `for` loop:

```js
let i, j, sum = 0;
for (i = 0, j = 10; i < 10; i++, j--) {
  sum += i * j;
}
console.log(sum);
```

---

### `for` loop-da istəyə bağlı ifadələr (optional expressions)

`for`-un üç hissəsindən (initialization, condition, increment) istənilən birini boş saxlamaq olar, amma `;` yazmaq mütləqdir.

* Məsələn, sonsuz dövr yaratmaq üçün:

```js
for (;;) {
  // infinite loop
}
```

---

### Nümunə: əlaqəli siyahını (`linked list`) gəzmək

```js
function tail(o) {
  for (; o.next; o = o.next) {
    // boş dövr bədəni
  }
  return o;
}
```

Burada **initialization** yoxdur, sadəcə **condition** və **increment** var.

---

## 5.4.4 for/of

**for/of** — ES6-da əlavə olunmuş yeni dövr (loop) operatorudur. Bu, `for` açar sözü ilə başlayır, amma adi `for` dövründən tam fərqlidir. Həmçinin, əvvəlki `for/in` dövründən də fərqlidir.

---

### Iterable Objects (İterasiya oluna bilən obyektlər)

`for/of` dövrü **iterable** obyektlərlə işləyir. İterable obyektlər barədə daha geniş izahı 12-ci fəsildə verəcəyik, amma sadəcə bilmək kifayətdir ki, **arrays (massivlər), stringlər, sets və maps** iterable hesab olunur. Bu obyektlər elementlər ardıcıllığını təmsil edir və `for/of` ilə üzərində addım-addım hərəkət etmək mümkündür.

### `for/of` nümunəsi:

```js
let data = [1, 2, 3, 4, 5, 6, 7, 8, 9], sum = 0;
for(let element of data) {
  sum += element;
}
console.log(sum); // 45
```

Burada `for/of` array-in hər bir elementini `element` dəyişəninə verir və dövr boyu bu elementləri toplamaq mümkündür.

### Sintaksis

`for(of)` dövrü belə yazılır:

```js
for (let variable of iterableObject) {
  // body
}
```

* `variable` — hər iterasiyada iterable obyektin növbəti elementi ilə doldurulur.
* `iterableObject` — array, string və ya iterable obyekt.

### Arrays canlı iterasiya olunur

* Dövr zamanı massivə edilən dəyişikliklər dövrün nəticəsinə təsir göstərə bilər.
* Məsələn, dövrün daxilində massivə yeni element əlavə etsən, bu sonsuz dövr yarada bilər.

---

## For/of İlə obyektlər

* **Object-lər iterable deyil**, ona görə `for/of`-u adi object-lə istifadə etmək **TypeError** verir.

```js
let o = { x: 1, y: 2, z: 3 };
for(let element of o) { // TypeError!
  console.log(element);
}
```

### Object property-lərini iterasiya etmək üçün:

* `for/in` istifadə et (bölmə §5.4.5-də).
* Yoxsa, `for/of` ilə `Object.keys()` və ya `Object.values()` metodlarından istifadə et.


```js
let obj = { a: 10, b: 20 };

// Açarları almaq:
for (let key of Object.keys(obj)) {
  console.log(key); // a, b
}

// Dəyərləri almaq:
for (let value of Object.values(obj)) {
  console.log(value); // 10, 20
}

// Hər açar və dəyəri birlikdə almaq:
for (let [key, value] of Object.entries(obj)) {
  console.log(key, value); // a 10, b 20
}
```

---

## FOR/OF İlə STRINGS

* String-lər ES6-da hər bir simvol üzrə iterasiya olunur.

```js
let word = "hello";
for (let char of word) {
  console.log(char);
}
// h, e, l, l, o

let count = 0;
for (let letter of "apple") {
  if (letter === "p") {
    count++;
  }
}
console.log(`'p' hərfi sayı: ${count}`); // 'p' hərfi sayı: 2


```

* Qeyd: String-lər UTF-16 simvoluna deyil, Unicode codepoint-lərə görə iterasiya olunur.

---

## FOR/OF İlə SET və MAP

* **Set** və **Map** sinifləri də iterable-dir.

```js
let colors = new Set(["red", "green", "blue"]);

for (let color of colors) {
  console.log(color);
}
// red, green, blue
```

* **Map** iterasiya ediləndə hər iterasiyada açar/dəyər cütü qaytarılır:

```js
let capitals = new Map([
  ["Azerbaijan", "Baku"],
  ["Turkey", "Ankara"]
]);

for (let [country, capital] of capitals) {
  console.log(country, capital);
}
// Azerbaijan Baku
// Turkey Ankara
```

## ASYNCHRONOUS ITERATION WITH FOR/AWAIT

* ES2018 ilə **asynchronous iterator** və `for/await` dövrü gəldi.
* `for/await` **async iterator** ilə işləyir, yəni asinxron məlumat axını üçün istifadə olunur.

```js
async function demo() {
  const arr = ["salam", "necesen", "yaxşıyam"];

  for await (let word of arr) {
    console.log(word);
  }
}

demo();

// salam, necesen , yaxşıyam
```

---

### 5.4.5 for/in

`for/in` döngüsü `for/of` döngüsünə çox bənzəyir, sadəcə `of` açarı `in` ilə əvəz olunur. 
`for/of` döngüsündə `of`-dan sonra iterasiya edilə bilən obyekt tələb olunur, amma `for/in` döngüsü `in`-dən sonra istənilən obyektlə işləyir.  
`for/of` ES6 ilə gəlib, amma `for/in` JavaScript-in əvvəldən olan xüsusiyyətidir, ona görə də sintaksisi daha təbii səslənir.

`for/in` ifadəsi müəyyən obyektin **property (xassə) adları** üzərində dövr edir. Sintaksisi belədir:

```js
for (variable in object)
  statement
```

* `variable` adətən dəyişənin adı olur, amma dəyişən elan edə, ya da assignment-in sol tərəfi üçün uyğun hər hansı ifadə ola bilər.
* `object` ifadəsi obyekt kimi qiymətləndirilir.
* `statement` isə döngünün gövdəsini təşkil edən ifadə və ya blokdur.

Misal üçün belə istifadə olunur:

```js
let adam = {
  ad: "Rəşad",
  yas: 25,
  seher: "Bakı"
};

for (let açar in adam) {
  console.log(açar + ":", adam[açar]);
}
// ad: Rəşad
// yas: 25
// seher: Bakı
```


Qeyd etmək lazımdır ki, `for/in`-də `variable` istənilən sol tərəf ifadəsi ola bilər, yəni hər iterasiyada fərqli qiymət ala bilər. Məsələn, bütün property adlarını array-a kopyalamaq üçün belə istifadə edilə bilər:


```js
let car = {
  brand: "BMW",
  model: "X5",
  year: 2020
};

let keys = [];

for (let key in car) {
  keys.push(key);
}

console.log(keys); // ["brand", "model", "year"]
```

---

### 💬 İzah:

* `car` adlı obyektimiz var.
* `for...in` ilə `car` obyektinin **açar adlarını** (property adlarını) bir-bir əldə edirik (`"brand"`, `"model"`, `"year"`).
* Hər açarı `keys` adlı massivə `push` edirik.

---


JavaScript-də massivlər sadəcə xüsusi obyektlərdir və onların indeksləri obyektin property-ləri sayılır, buna görə `for/in` ilə massiv indekslərini də dövr etmək olar. Məsələn:

```js
let a = ["alma", "armud", "banan"];

for (let i in a) {
  console.log(i);
}

// 0 , 1 , 2
```

bu, massiv `a`-nın indekslərini (0, 1, 2 və s.) çap edir.

---

**5.5 Tullanma Operatorları (Jump Statements)**

JavaScript-də **tullanma operatorları** kodunuzun icra axınını dəyişməyə imkan verən əmrlərdir. Onlar proqramın bir hissəsindən digərinə keçməyə kömək edir.

Əsas tullanma operatorları bunlardır:

* **`break`**: Bir dövrü (loop) və ya `switch` operatorunu dərhal bitirir və ondan sonrakı koda keçir.
* **`continue`**: Bir dövrün cari addımının (iterasiyasının) qalan hissəsini atlayır və dövrün növbəti addımına başlayır.
* **`return`**: Bir funksiyadan çıxır və lazım gələrsə, bir dəyər qaytarır.
* **`throw`**: Bir xəta (exception) yaradır və proqramın ən yaxın xəta idarəedicisinə keçməsinə səbəb olur.

---

**5.5.1 Etiketli Operatorlar (Labeled Statements)**

JavaScript-də istənilən operatora bir ad, yəni **etiket** verə bilərsiniz. Etiket vermək üçün operatorun əvvəlinə bir ad (identifikator) və iki nöqtə (:) qoyulur:

```javascript
etiketAdı: operator
```

Etiketlər əsasən **`break`** və **`continue`** operatorları ilə birlikdə, daxili dövrlərdən və ya çətin keçidlərdən çıxmaq üçün istifadə olunur.

**Nümunə:**

Bu nümunədə, `mainloop` adlı bir `while` dövrü var. `continue mainloop` əmri `mainloop` dövrünün cari addımını atlayıb növbəti addıma keçir:

```javascript
mainLoop: while (true) {
  let token = getNextToken();

  if (token === null) {
    break mainLoop; // mainLoop adlı dövrü dayandırırıq
  }

  if (token === "skip") {
    continue mainLoop; // mainLoop dövrünün növbəti iterasiyasına keçir
  }

  console.log(token); // Token çap olunur, əgər "skip" deyilsə
}
```

**Qaydalar:**

* Etiket adı qanuni bir JavaScript identifikatoru olmalıdır.
* Etiket adları ayrılmış sözlər (`if`, `while` və s.) ola bilməz.
* Bir operatorun birdən çox etiketi ola bilər.

---

### 5.5.2 `break` operatoru

`break` operatoru dövrləri (`for`, `while`, `do/while`) və ya `switch` operatorlarını dərhal dayandırmaq üçün istifadə olunur.

**Sadə istifadəsi:**

```javascript
break;
```

Bu formada `break` yalnız bir dövrün və ya `switch` operatorunun içərisində istifadə edilə bilər.

**Nümunə:** Bir `array`də dəyər axtarışı. Dəyər tapılsa, dövr dərhal dayanır:

```javascript
let numbers = [3, 7, 10, 15, 20];
let limit = 12;

for (let num of numbers) {
  if (num > limit) {
    break; // 12-dən böyük ədədi tapanda dövrü dayandırır
  }
  console.log(num); // 12-dən kiçik və ya bərabər ədədləri çap edir
}
```


---

## **Labeled break (break labelname;)

JavaScript-də `break` əmri ilə bir **etiket (label)** istifadə edə bilərsiniz. Etiket — bu, operatora verilən ad (label), məsələn: `labelname:`. `break labelname;` əmri həmin etiketlə işarələnmiş blokdan çıxır.

Bu xüsusiyyət ən çox **iç-içə dövrlərdən (nested loops)** və ya başqa etiketlənmiş bloklardan dərhal çıxmaq üçün faydalıdır.

---

### Vacib Qaydalar

* `break` ilə `labelname` arasında **yeni sətir (newline)** qoymaq olmaz, yoxsa JavaScript onu adi `break;` kimi qəbul edər.
* `break` funksiyanın sərhədlərini keçə bilməz. Yəni funksiyanın içindən xarici bir etiketə `break` etmək mümkün deyil.

---

### Sadə Nümunə: İki iç-içə dövrdən etiketlə çıxmaq

```js
outerLoop: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      break outerLoop; // Hər iki dövrdən dərhal çıxır
    }
    console.log(i, j);
  }
}

console.log("Dövr bitdi");
```

**İzah:**
`i === 1 && j === 1` olduqda `break outerLoop;` yazılır və həm `j` dövrü, həm də `i` dövrü dərhal dayandırılır. Əks halda, bütün `(i, j)` cütlükləri çap olunur.

---

### Alternativ nümunə: `if` bloku üçün etiketlə `break`

```js
checkBlock: {
  console.log("Başladı");

  if (true) {
    break checkBlock; // Bu blokdan dərhal çıxır
  }

  console.log("Bu heç vaxt çap olunmayacaq");
}

console.log("Sonra davam edir");
```

**İzah:**
`break checkBlock;` əmri `checkBlock` adlı etiketlə işarələnmiş blokdan çıxır. Ona görə `"Bu heç vaxt çap olunmayacaq"` konsola yazılmır.

---

## 5.5.3 `continue` operatoru

`continue` operatoru dövrün (`loop`) cari iterasiyasını dayandırıb, **növbəti iterasiyaya keçmək** üçündür. Yəni, `break` kimi dövrü tam dayandırmır, sadəcə cari addımı atlayır.

---

### Sadə istifadə:

```js
continue;
```

> **Qeyd:** `continue` yalnız dövr daxilində işləyir, başqa yerdə istifadə olunarsa, `syntax error` verir.

---

### `Labeled continue`

`continue` operatorunu `label` ilə də istifadə edə bilərik:

```js
continue labelName;
```

Bu, iç-içə dövrlərdə istənilən xarici dövrə keçid üçün faydalıdır.

---

### `continue` necə işləyir dövr növünə görə?

* **`for` dövrü:** `continue` cari iterasiyanı dayandırır, sonra `increment` (məsələn, `i++`) və şərt yoxlanılır, növbəti iterasiyaya keçilir.
* **`while` dövrü:** `continue` şərti yenidən yoxlayır, əgər `true`dirsə, dövr bədəni yenidən başdan icra olunur.
* **`do/while` dövrü:** `continue` əvvəlcə şərti yoxlayır, sonra dövr bədəni yenidən icra olunur.
* **`for/of` və `for/in`:** `continue` növbəti elementə və ya property-ə keçir.

---

### Dövr növlərinə görə nümunələr

---

#### 1. `for` dövründə

```js
for (let i = 0; i < 5; i++) {
  if (i === 2) continue;  // i=2 olduqda növbəti iterasiyaya keçir
  console.log(i);         // 0,1,3,4 çap olunur, 2 atlandı
}
```

---

#### 2. `while` dövründə

```js
let i = 0;
while (i < 5) {
  i++;
  if (i === 3) continue; // i=3 olduqda növbəti iterasiyaya keçir
  console.log(i);        // 1,2,4,5 çap olunur, 3 atlandı
}
```

---

#### 3. `do/while` dövründə

```js
let i = 0;
do {
  i++;
  if (i === 4) continue; // i=4 olduqda növbəti iterasiyaya keçir
  console.log(i);        // 1,2,3,5 çap olunur, 4 atlandı
} while (i < 5);
```

---

#### 4. `for/of` dövründə

```js
let arr = ["alma", "armud", "heyva"];

for (let meyve of arr) {
  if (meyve === "armud") continue; // armud atlandı
  console.log(meyve);              // alma, heyva çap olunur
}
```

---

#### 5. `Labeled continue` — iç-içə dövrlərdə

```js
outer: for (let i = 0; i < 3; i++) {
  inner: for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) continue outer;  // `inner` yox, `outer` dövrünün növbəti iterasiyasına keçir
    console.log(i, j);
  }
}
```

---

### Vacib qeyd:

`break` və `continue` operatorlarında `labelName` ilə `break`/`continue` arasında **yeni sətir (newline) qoymaq olmaz**, yoxsa JavaScript onu sadəcə `break;` və ya `continue;` kimi qəbul edir.

---

Bu `return` operatorunun izahı artıq çox aydın və yaxşı yazılıb, amma onu daha sadə, az texniki terminlərlə, müəllim dili ilə, həmçinin nümunələri sadələşdirib, əsas məqamları önə çıxarmaq olar. Üstəlik, bəzi vacib qaydaları yanına ingiliscə terminlərlə qeyd etmək istifadəçi üçün faydalı olar.

---

## 5.5.4 `return` operatoru

Funksiyalar bir dəyər qaytara bilər. `return` operatoru funksiyanın işini bitirib, həmin dəyəri çağıran yerə göndərmək üçün istifadə olunur.

---

### Sintaksis:

```js
return expression;
```

* `expression` — funksiyanın qaytaracağı dəyərdir.
* `return` yalnız **funksiya içində** işləyir, başqa yerdə yazsan səhv verir (`syntax error`).

---

### Necə işləyir?

`return`-ə çatanda funksiya dərhal dayanır və `expression`-in dəyərini çağıran koda verir.

---

### Nümunə:

```js
function kvadrat(x) {
  return x * x; // x-in kvadratını qaytarır
}

console.log(kvadrat(3)); // 9
```

---

### Əgər `return` yoxdursa?

Funksiya sonunda `return` yazmasan, funksiya `undefined` dəyərini qaytarır.

```js

function salam() {
  console.log("Salam!");
}

let netice = salam(); 
console.log(netice); // undefined
```

---

### `return` olmadan da istifadə etmək olar

Sadəcə funksiyanı dayandırmaq üçün, heç bir dəyər qaytarmadan:

```js
function yoxla(n) {
  if (n < 0) return;  // n mənfi olsa, funksiyanı dayandır və undefined qaytar
  console.log("Ədədi qəbul etdim:", n);
}
```

---

### Vacib Qaydalar (Important Rules):

* `return` və ondan sonra gələn ifadə (`expression`) arasına **yeni sətir (line break)** qoymaq olmaz!
  Javascript bunu avtomatik nöqtəli vergül kimi qəbul edir (`automatic semicolon insertion`).

**Pis nümunə (syntax error ola bilər):**

```js
return
5 + 5;  // Burada 5 + 5 ifadəsi return-dan ayrı qalır, undefined qaytarılır.
```

**Doğru nümunə:**

```js
return 5 + 5;
```

---

### 5.5.5 `yield` operatoru

`yield` operatoru `return`a bənzəyir, amma yalnız **generator funksiyalarda** (ES6 ilə gəlib) istifadə olunur. `yield` funksiya icrasını müvəqqəti dayandırır və növbəti dəyəri (`value`) verir, amma funksiyadan tam çıxmır — yəni icra sonra yenidən davam edə bilər.

**Sadə nümunə:**

```js
function* sayNumbers() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = sayNumbers();

console.log(generator.next().value); // 1
console.log(generator.next().value); // 2
console.log(generator.next().value); // 3
```

Burada `sayNumbers` generatorudur. Hər `yield` növbəti dəyəri verir və icranı saxlayır. `next()` çağıranda isə növbəti `yield`-ə qədər davam edir.

---

**Qeyd:** `yield` və generator funksiyalar daha dərin mövzudur, onlara **Fəsil 12**-də geniş baxılacaq. Burada isə `yield`-in `return`dan fərqli olaraq funksiya icrasını saxlayıb, növbəti dəyəri vermək üçün istifadə edildiyini göstərdik.

---

### 5.5.6 `throw` operatoru

**Exception** (istisna) proqramda gözlənilməz və ya səhv vəziyyəti göstərir. İstisna "atmaq" (`throw`) — bu vəziyyəti proqramda bildirmək deməkdir. "Tutmaq" (`catch`) isə atılan istisnanı qəbul edib ona görə hərəkət etməkdir.

JavaScript-də proqram icrası zamanı problem yaranarsa və ya biz özümüz `throw` operatoru ilə istisna atsaq, proqram dayana və ya həmin istisnanı idarə etmək lazım olur. Bu idarə etmə üsulu `try/catch` blokunda verilir (sonrakı bölmələrdə izah olunacaq).

`throw` operatorunun sintaksisi belədir:

```js
throw expression;
```

Burada `expression` istənilən dəyər ola bilər — string, rəqəm, obyekt və s. Ən çox `Error` obyektləri atılır ki, onlar xətanın növü və mesajını daşıyır.

---

**Sadə nümunə:**

```js
function divide(a, b) {
  if (b === 0) throw "Sıfıra bölmək olmaz!"; // İstisna atırıq

  return a / b;
}

console.log(divide(10, 2)); // 5

// divide(5, 0); // Burada xəta atılır və proqram dayanar
```

Burada `divide` funksiyası sıfıra bölmə halında `throw` ilə xəta mesajı atır. Bu, proqramda səhv olduğunu göstərir və kodun növbəti sətirlərinin icrasını dayandırır.


**Qeyd:** İstisnalar `try/catch` blokları ilə tutulmadığı halda, proqram xəta verərək dayana bilər. Ona görə istisna atarkən onun tutulmasını da planlaşdırmaq lazımdır.

---

## 5.5.7 `try/catch/finally`

`try/catch/finally` JavaScript-də **istisnaların (exceptions)** idarə olunması üçün istifadə olunan quruluşdur. Bu quruluş proqramın gözlənilməz xətalar üzündən qəfil dayanmaması və xətaların təhlükəsiz şəkildə idarə olunması üçün vacibdir.

---

### Blokların funksiyaları

* **`try` bloku:**
  Bu blokda, xəta baş verə biləcək kod yazılır. Burada yazılan kod normalda problemsiz icra olunur. Amma əgər problem yaransa, həmin xəta `catch` blokuna ötürülür.

* **`catch` bloku:**
  Əgər `try` blokunda xəta yaranarsa, bu blok icra olunur. Burada yaranan xəta bir dəyişən (məsələn, `e` və ya `error`) kimi tutulur. Bu dəyişən vasitəsilə xətanın nə olduğu öyrənilir və ona uyğun tədbir görülür.

* **`finally` bloku:**
  Bu blok `try`-`catch` prosesindən asılı olmayaraq **həmişə** icra olunur. Burada əsasən açılmış resursları (fayllar, şəbəkə bağlantıları və s.) bağlamaq, yaddaşı təmizləmək və digər təmizlik işləri görülür.

---

### Sintaksis

```javascript
try {
  // Xəta yarana biləcək kod
}
catch (error) {
  // 'try' blokunda xəta yaranarsa, burada icra olunur
  // 'error' dəyişəni xətanın detallarını saxlayır
}
finally {
  // Bu blok hər zaman icra olunur
}
```

---

### İstisna atmaq (`throw`)

`try` blokunda, özümüz də istisna ata bilərik. Bu zaman proqramın müəyyən yeri problemli olduqda, biz bir `Error` obyekti və ya istənilən başqa dəyəri `throw` əmri ilə atırıq. Bu, həmin xəta mesajını `catch` blokuna ötürür.

---

### Nümunə

Aşağıdakı nümunədə fayl oxuma funksiyası yazılıb. Əgər funksiya səhv arqumentlə çağırılarsa, xəta atır və `catch` bloku xətanı tutub idarə edir. `finally` bloku isə hər halda işləyir.

```javascript
function readFile(filename) {
  if (typeof filename !== 'string') {
    throw new Error("Filename should be a string");
  }
  if (filename === "") {
    throw new Error("Filename cannot be empty");
  }
  // Simulyasiya üçün fayl məzmunu qaytarılır
  return "Fayl məzmunu: Lorem ipsum...";
}

try {
  // Burada səhv arqument veririk - boş string
  let content = readFile("");
  console.log(content);
} 
catch (error) {
  // Xəta tutulur və mesajı göstərilir
  console.error("Xəta baş verdi: " + error.message);
}
finally {
  // Həmişə işləyir, təmizlik işləri üçün
  console.log("Fayl oxuma əməliyyatı tamamlandı.");
}
```

**İcra nəticəsi:**

```
Xəta baş verdi: Filename cannot be empty  
Fayl oxuma əməliyyatı tamamlandı.
```

---

### `finally` bloku niyə lazımdır?

* Əgər proqramınızda açılmış fayllar, verilənlər bazası bağlantıları, yaddaşda ayrılmış obyektlər varsa, onların mütləq bağlanması, azad edilməsi vacibdir.
* `finally` bloku bunun üçün ideal yerdir, çünki o, xəta olsa da, olmasa da hər zaman işləyir.
* Məsələn, faylı açırsınızsa, iş bitəndən sonra onun mütləq bağlı olduğuna əmin olmaq lazımdır. Bunu `finally` blokunda edə bilərsiniz.

---

### `try` - `finally` tək başına

`catch` bloku olmadan da `try`-`finally` yazmaq mümkündür:

```javascript
try {
  console.log("Əməliyyat başlayır");
  // Burada xəta da baş verə bilər
}
finally {
  console.log("Əməliyyat bitdi (mütləq icra olunur)");
}
```

Bu vəziyyətdə, əgər `try` blokunda xəta olsa da, `finally` işləyəcək. Amma xəta tutulmayacaq və proqram həmin xətanı yuxarıya ötürəcək.

---

### `return`, `break`, `continue` ilə `finally` blokunun işləməsi

* Əgər `try` blokunda `return`, `break` və ya `continue` varsa, əvvəlcə `finally` blokunun kodu icra olunur, sonra isə `return` və ya `break` əməliyyatı yerinə yetirilir.
* Yəni `finally` bloku bu cür vəziyyətlərdə də qaçırılmır.

Məsələn:

```javascript
function test() {
  try {
    console.log("try");
    return 1;
  }
  finally {
    console.log("finally");
  }
}

console.log(test());
```

**Nəticə:**

```
try  
finally  
1
```

---

### Vacib nüans: `finally` blokundakı `return` və ya `throw`

Əgər `finally` blokunda da `return` və ya `throw` istifadə olunarsa, bu `try` və ya `catch` blokundakı `return` və ya `throw` əmrlərini **ələ keçirir** (override edir).

Məsələn:

```javascript
function test() {
  try {
    return 1;
  }
  finally {
    return 2;
  }
}

console.log(test());  // Nəticə: 2
```

Bu halda, `try` blokundakı `return 1` gözardı edilir və `finally` blokundakı `return 2` icra olunur.

---

## Nəticə

* `try` — potensial xəta verən kodu yazırıq.
* `catch` — xətanı tutub emal edirik.
* `finally` — həmişə işləyən blok, burada təmizlik işləri aparılır.

Bu quruluş proqramlarımızı daha etibarlı, oxunaqlı və səhvlərə dayanıqlı edir.

---

### 5.6 Müxtəlif `Statements`lər

Bu bölmə JavaScript-də qalan üç `statement`i — `with`, `debugger` və `"use strict"`i izah edir.

---

## 5.6.1 `with` statementi

JavaScript-də `with` statementi müəyyən bir obyektin **property**-lərini (xüsusiyyətlərini) müvəqqəti olaraq lokal dəyişənlər kimi istifadə etməyə imkan verən bir sintaksis quruluşudur.

---

### `with` nə edir?

`with` statementi, verilmiş obyektin içindəki bütün property-ləri icra olunan kod bloku üçün **lokal dəyişən** kimi qəbul edir. Beləliklə, həmin blokda hər hansı property adını yazanda, o, həmin obyektin müvafiq xüsusiyyətinə istinad edir.

---

### Sintaksis

```javascript
with (object)
  statement
```

* `object`: Məlumatları saxlayan obyekt (məsələn, `document.forms[0]`)
* `statement`: Obyektin property-ləri konteksində icra olunacaq kod bloku (adətən mötərizə içində bir neçə sətirdən ibarət olur)

---

### Nümunə

```javascript
with (document.forms[0]) {
  name.value = "";
  address.value = "";
  email.value = "";
}
```

Bu kodda `name`, `address`, `email` birbaşa `document.forms[0]` obyektinin property-ləri kimi qəbul olunur.

---

### Niyə `with` istifadə olunurdu?

Əvvəllər kodda çox uzun və təkrar olunan obyekt yolu yazmaq əvəzinə, bu yolları qısaltmaq üçün `with` statementindən istifadə olunurdu. Bu, kodu daha qısa və oxunaqlı etmək məqsədi güdürdü.

Məsələn, belə:

```javascript
document.forms[0].name.value = "";
document.forms[0].address.value = "";
document.forms[0].email.value = "";
```

yerinə yuxarıdakı `with` bloku ilə yazılırdı.

---

### Niyə `with` istifadə etməməliyik?

1. **Strict Mode qadağandır:**
   ES5 ilə gələn `strict mode` rejimində `with` istifadə etmək qadağandır. Bu da yeni, təhlükəsiz və optimallaşdırılmış JavaScript yazmaq üçün vacibdir.

2. **Kodun oxunmasını çətinləşdirir:**
   `with` bloku içində hansı dəyişənin obyekt property-si, hansının isə lokal dəyişən olduğu aydın deyil. Bu, xüsusən böyük və mürəkkəb kodlarda səhvlərə səbəb olur.

3. **Performans problemi:**
   `with` istifadə olunan kod JavaScript mühərriki tərəfindən optimallaşdırmaq çətindir. Bu da tətbiqin yavaş işləməsinə gətirib çıxara bilər.

4. **Əvəzlənməsi asandır:**
   `with` istifadə etmədən eyni işi daha aydın və sadə kodla yazmaq mümkündür. Məsələn:

   ```javascript
   let form = document.forms[0];
   form.name.value = "";
   form.address.value = "";
   form.email.value = "";
   ```

   Bu üsul həm daha oxunaqlı, həm də performans baxımından daha sərfəlidir.

---

### Nəticə

* `with` statementi əvvəllər kodu qısaltmaq üçün istifadə olunurdu, amma bu gün artıq **tövsiyə edilmir** və `strict mode`da qadağandır.
* Modern JavaScript-də onun yerinə dəyişənlərə referans verməklə (məsələn, `let obj = ...`) işləmək məsləhətdir.
* `with` istifadə etməkdən qaçınmaq, kodun təhlükəsizliyini, performansını və oxunaqlığını artırır.

---

### 5.6.2 `debugger` operatoru

`debugger` `statement`i JavaScript-də xüsusi bir əmrlə icranı dayandırmaq üçün istifadə olunur. Əgər `debugger` mövcuddursa (məsələn, browser-də developer tools açıqdırsa), icra dayandırılır və siz həmin yerdə proqramın vəziyyətini yoxlaya, dəyişənlərin dəyərlərini, çağırış zəncirini (call stack) görə bilərsiniz.

Bu, proqramda səhvləri tapmaq və necə işlədiyini addım-addım izləmək üçün çox faydalıdır.

---

### Sintaksis

```javascript
debugger;
```

---

### Tam nümunə:

```javascript
function checkNumber(num) {
  // Əgər daxil olan ədəd mənfidirsə, debugger işə düşəcək və icra dayandırılacaq
  if (num < 0) debugger;  

  console.log("Ədəd:", num);
}

checkNumber(5);    // Normal halda konsola "Ədəd: 5" yazacaq
checkNumber(-10);  // Burada debugger işə düşəcək, icra dayanacaq
```

---

### İzah:

1. `checkNumber` funksiyası bir ədəd qəbul edir.
2. Əgər bu ədəd mənfi olarsa, `debugger` əmri ilə icra dayandırılır.
3. Bu zaman, siz browser-in Developer Tools (F12 ilə açılır) konsolunda həmin anda proqramın vəziyyətini araşdıra bilərsiniz — məsələn, `num` dəyişəninin dəyərini, çağırış zəncirini və digər lokal dəyişənləri görə bilərsiniz.
4. Bu, proqramdakı səhvləri tapmaq və onların səbəbini anlamaq üçün çox effektiv üsuldur.
5. Əgər `debugger` komandası yoxdursa və ya Developer Tools açıq deyilsə, bu sətr heç bir təsir etmədən keçəcək.

---

### Əlavə qeydlər

* `debugger` istifadə edərkən həmişə koddakı həmin sətri izləmək və səbəbi anlamaq üçün `Developer Tools` konsolunu açmalısınız.
* `debugger` kodunuzu `production`da saxlamayın, yalnız inkişaf zamanı istifadə olunur.
* `debugger` ilə proqramın axarını dayandırıb dəyişənlərin dəyərini yoxlamaq, mərhələli təhlil üçün vacibdir.

---

# 5.6.3 `"use strict"` (Sərt rejim)

### `"use strict"` nədir?

`"use strict"` — JavaScript-də **sərt rejimi (strict mode)** aktivləşdirmək üçün istifadə olunan xüsusi bir direktivdir.

* Bu direktiv kodunuzu daha təhlükəsiz və səhvlərə qarşı daha dözümlü edir.
* Köhnə səhvlərin qarşısını alır, səhv kod yazma ehtimalını azaldır.
* Kodun əvvəlində və ya funksiya daxilində yazılır.
* Strict mode-da bəzi köhnə davranışlar qadağan olunur və ya səhv kimi qiymətləndirilir.

---

### Necə yazılır?

```javascript
"use strict";
```

Yaxud funksiya daxilində:

```javascript
function test() {
  "use strict";
  // bu funksiya sərt rejimdə işləyəcək
}
```

---

### Niyə istifadə etməliyik?

1. **Səhvləri aşkar edir:** Məsələn, dəyişəni əvvəlcədən elan etmədən istifadə etsəniz, səhv verir.
2. **Kodun təhlükəsizliyi artır:** Məsələn, bəzi təhlükəli davranışlar qadağan edilir.
3. **Kodun performansı yaxşılaşa bilər:** Çünki optimizatorlar daha yaxşı işləyə bilər.
4. **Gələcəkdəki ES6 və daha yeni standartlarla uyumluluq təmin edir.**

---

### Strict mode və non-strict mode arasındakı əsas fərqlər

| Fərq                              | Non-strict mode (Adətən)                                                           | Strict mode (Sərt rejim)                                                                        |
| --------------------------------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| **1. Dəyişənlərin elan edilməsi** | İstənilən yerdə dəyişəni istifadə etmək olar (globalə düşür)                       | Dəyişəni əvvəlcədən `var`, `let`, `const` ilə elan etməlisiniz, əks halda `ReferenceError` olur |
| **2. `this` dəyəri funksiyada**   | Funksiya daxilində `this` global obyektdir (`window` və ya `global`)               | Funksiya daxilində `this` `undefined` olur, yəni boşdur                                         |
| **3. `with` statement**           | İstifadə oluna bilər                                                               | Tamamilə qadağandır                                                                             |
| **4. Eyni adda iki property**     | Problem olmur                                                                      | Syntax error olur                                                                               |
| **5. `eval()` davranışı**         | `eval` daxilində dəyişənlər və funksiyalar çağıran səviyyəyə əlavə olunur          | `eval` öz yerli `scope`unda işləyir, dəyişənlər orada qalır                                     |
| **6. `delete` operator**          | Dəyişənləri və ya funksiyaları silmək mümkündür (amma adətən heç bir təsir yoxdur) | Sadə identifikatorları (məsələn, `delete x`) silmək olmaz, syntax error verir                   |

---

### Sadə nümunələrlə izah

#### 1. Dəyişəni elan etmədən istifadə

```javascript
"use strict";
x = 10; // ReferenceError: x is not defined
```

```javascript
// non-strict mode-da belə yazmaq olar və x avtomatik globalə əlavə olunur (bu pis təcrübədir)
x = 10; // heç bir xəta olmur, amma pisdir!
```

#### 2. `this` dəyərinin fərqi

```javascript
// Non-strict mode
function f() {
  console.log(this);  // window (browser-də)
}
f();
```

```javascript
// Strict mode
"use strict";
function f() {
  console.log(this);  // undefined
}
f();
```

#### 3. `with` qadağası

```javascript
"use strict";
with (Math) {   // SyntaxError: Strict mode code may not include a with statement
  console.log(sin(0));
}
```

#### 4. Eyni adda iki property

```javascript
"use strict";
let obj = {
  x: 1,
  x: 2  // SyntaxError: Duplicate data property in object literal not allowed in strict mode
};
```

#### 5. `delete` ilə dəyişəni silmək olmaz

```javascript
"use strict";
var y = 10;
delete y;  // SyntaxError: Delete of an unqualified identifier in strict mode.
```

---

### Qısa yekun

* `"use strict"` yazmaq kodu daha sərt yoxlayır.
* Daha çox səhvləri əvvəlcədən tapır.
* Köhnə, səhv və təhlükəli praktikaları qadağan edir.
* Yeni kod yazanda mütləq istifadə etmək məsləhətdir.

---

### 5.7 `Declarations` (Təyinatlar)

JavaScript-də bəzi `keyword`lər var ki, onlar texniki olaraq adi `statement` (ifadə olunan əmrlər) deyil, **`declaration`** adlanır. Məsələn:
`const`, `let`, `var`, `function`, `class`, `import`, `export`

---

### `Declarations` nədir?

* `Statements` — proqramda bir əməliyyatın, hərəkətin baş verməsini təmin edir.
* `Declarations` — isə proqramda **yeni dəyərlər, funksiyalar, dəyişənlər və s. üçün adlar yaradır**, yəni struktur müəyyənləşdirir.

Sadəcə `declaration` yazdıqda, biz proqramda yeni "simvol" (ad) yaradırıq və ona bir dəyər, funksiya və ya sinif (class) təyin edirik. Bu adlar daha sonra proqramda istifadə olunur.

---

### `Declarations` necə işləyir?

* `Declarations` kodun icrasından əvvəl işlənir (hoisting mexanizmi ilə).
* Onlar proqramın strukturunu müəyyən edir: hansı dəyişənlər, funksiyalar, siniflər mövcuddur.
* `Declarations` həmçinin `module`larda `import` və `export` üçün istifadə olunur — yəni fayllar arasında dəyərləri paylaşmaq üçün.

---

### Nümunələr

```javascript
// Variable declaration
let age = 25;
const name = "Rəşad";
var isActive = true;

// Function declaration
function greet() {
  console.log("Salam!");
}

// Class declaration
class Person {
  constructor(name) {
    this.name = name;
  }
}

// Import declaration (module-lar üçün)
import { readFile } from "fs";

// Export declaration (module-lar üçün)
export const PI = 3.14;
```

---

### 5.7.1 `const`, `let`, və `var`

JavaScript-də dəyişənləri elan etmək üçün üç əsas `declaration` tipi var: `const`, `let` və `var`. Onların hər birinin öz xüsusiyyətləri və istifadə qaydaları var.

---

#### `const` — Dəyişməyən dəyərlər (Constant)

* `const` ilə elan edilmiş dəyişənlərin dəyəri **dəyişdirilə bilməz**.
* Yəni, bir dəfə dəyər təyin edildikdən sonra, onu yenidən təyin etmək olmaz.
* Lakin, əgər `const` ilə bir obyekt və ya massiv elan edilərsə, onun **içindəki elementlər dəyişdirilə bilər** (yalnız dəyişənin özü yenidən təyin edilə bilməz).

```javascript
const PI = 3.14;
PI = 3.1415;  // Xəta! Dəyər dəyişdirilə bilməz.

const obj = {a: 1};
obj.a = 2;    // Bu doğrudur, çünki obyektin özü deyil, onun içindəki dəyər dəyişir.
```

---

#### `let` — Dəyişkən dəyərlər (Variable)

* `let` ilə elan edilmiş dəyişənlər **dəyişdirilə bilər**.
* `let` ES6 ilə gəlib və daha düzgün `scope` idarəsi təmin edir (`block scope`).
* `let` ilə elan edilmiş dəyişən yalnız **öz blokunda** (`{ ... }`) mövcuddur.

```javascript
let count = 1;
count = 2; // Doğrudur

if (true) {
  let insideBlock = 5;
}
// console.log(insideBlock); // Xəta! insideBlock yalnız if bloku içində mövcuddur.
```

---

#### `var` — Köhnə tip dəyişən (Function-scoped variable)

* `var` ES5 və əvvəlki versiyalarda istifadə olunurdu.
* `var` ilə elan edilmiş dəyişənlər `function scope`-ludur, yəni `function` daxilində tanınır, amma `block` daxilində deyil.
* Bu səbəbdən `var` ilə yazılan kodlarda bəzən gözlənilməz nəticələr yaranır (məsələn, `for` dövrəsində dəyişənin `block` xaricində də mövcud olması).
* Müasir JavaScript-də `let` və `const` üstünlük təşkil edir və `var`dan qaçmaq tövsiyə olunur.

```javascript
if (true) {
  var x = 10;
}
console.log(x); // 10, çünki var block scope deyil, function scope-dur
```

---

### Ümumi fərqlər cədvəl şəklində

| Xüsusiyyət         | `const`                                                               | `let`          | `var`                                          |
| ------------------ | --------------------------------------------------------------------- | -------------- | ---------------------------------------------- |
| Yenidən təyin olma | Xeyr                                                                  | Bəli           | Bəli                                           |
| Scope              | Block (`{}`)                                                          | Block (`{}`)   | Function (və ya global)                        |
| Hoisting           | Bəli, amma temporal dead zone (TDZ) var (əvvəl istifadə mümkün deyil) | Eynilə `const` | Bəli, əvvəlcədən mövcuddur və `undefined` olur |
| Yenidən elan etmə  | Xeyr                                                                  | Xeyr           | Bəli                                           |


---
### 5.7.2 `function`

`function declaration` — yəni funksiya elanları — proqramda funksiyalar yaratmaq üçün istifadə olunur. Funksiya içində yazdığımız kodu bir adla yadda saxlayırıq və lazım olanda həmin adı çağırıb funksiyanı işə salırıq.

Məsələn, belə bir funksiya yarada bilərik:

```javascript
function area(radius) {
  return Math.PI * radius * radius;
}

area(5) // 25 * 3.1434422
```

Bu nümunədə `area` funksiyası `radius` adlı bir dəyər alır və dairənin sahəsini hesablayıb qaytarır.

---

**Əhəmiyyətli məqam:**
JavaScript-də funksiyalar “hoisted” olunur, yəni sanki onlar kodun əvvəlində yaradılır. Buna görə funksiyanı kodda əvvəlcə yazmasaq da, sonra rahat çağıra bilərik:

```javascript
console.log(square(4)); // 16

function square(n) {
  return n * n;
}
```

Yuxarıdakı nümunədə funksiyanı çağırış yazılandan sonra yazmışıq, amma problem olmur.

---

**Başqa növ funksiyalar:**

* **Generator funksiyaları:**
  Bunlar xüsusi funksiyalardır və `function*` şəklində yazılır. Gələcəkdə `yield` ilə dəyərləri ardıcıl olaraq qaytara bilirlər.

* **Asinxron funksiyalar (async functions):**
  Bu funksiyalar `async function` kimi elan edilir və içində `await` istifadə etməyə imkan verir. Asinxron əməliyyatları rahat idarə etmək üçün çox faydalıdır.

---


### 5.7.3 `class`

ES6 və sonrakı versiyalarda, `class declaration` yeni bir `class` (sinif) yaradır və ona istinad edə biləcəyimiz bir ad verir. `Class`lar obyekt yaratmaq üçün bir şablondur (template) — yəni, eyni tipli çoxsaylı obyektlərin necə görünəcəyini və nə edə biləcəyini təyin edir. Bu mövzu `Fəsil 9`-da daha ətraflı təsvir ediləcək.

Sadə bir `class declaration` nümunəsi:

```javascript
class Circle {
  constructor(radius) {
    // 'this' burada yaradılan obyektə işarə edir
    this.r = radius;
  }
  area() {
    // Sahəni hesablayan metod
    return Math.PI * this.r * this.r;
  }
  circumference() {
    // Çevrə uzunluğunu hesablayan metod
    return 2 * Math.PI * this.r;
  }
}
````

`class`lardan obyekt yaratmaq üçün `new` operatorundan istifadə olunur:

```javascript
const c = new Circle(5);
console.log(c.area()); // 78.53981633974483
```

**Vacib:** `class declaration`ları **hoisted deyil**. Yəni, `class`ı yaratmadan əvvəl onu istifadə etməyə çalışsanız, `ReferenceError` alacaqsınız:

```javascript
const c = new Circle(5); // ReferenceError: Cannot access 'Circle' before initialization

class Circle {
  constructor(radius) {
    this.r = radius;
  }
}
```

Başqa bir nümunə:

```javascript
class Car {
  constructor(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
  }

  start() {
    console.log(`${this.make} ${this.model} işə düşdü.`);
  }

  info() {
    return `${this.year} model ${this.make} ${this.model}`;
  }
}

const myCar = new Car("Toyota", "Corolla", 2020);

myCar.start();                // Toyota Corolla işə düşdü.
console.log(myCar.info());    // 2020 model Toyota Corolla
```

---

**Qısa əlavə:** `class`lar həmçinin digər `class`lardan miras ala (inherit) bilər. Bu, obyekt yönümlü proqramlaşdırmanın əsas xüsusiyyətidir və `extends` açar sözü ilə edilir. Məsələn:

```javascript
class ElectricCar extends Car {
  constructor(make, model, year, batteryLife) {
    super(make, model, year); // Əsas classın constructorunu çağırırıq
    this.batteryLife = batteryLife;
  }

  batteryStatus() {
    console.log(`Batareya səviyyəsi: ${this.batteryLife}%`);
  }
}

const tesla = new ElectricCar("Tesla", "Model S", 2023, 90);
tesla.start();              // Tesla Model S işə düşdü.
tesla.batteryStatus();      // Batareya səviyyəsi: 90%
```

---
### 5.7.4 `import` və `export`

Müasir JavaScript proqramları böyüdükcə kodu ayrı-ayrı `file`larda (fayllarda) saxlamaq və onları `module`lar (modullar) şəklində idarə etmək zərurəti yaranır. `import` və `export` `declaration`ları (bəyannamələri) məhz bu məqsədə xidmət edir: bir JavaScript `module`unda yaradılan dəyərləri (`function`, `class` (sinif), `variable` (dəyişən) və s.) digər `module`larda istifadə etməyə imkan verir.

Bir **`module` (modul)** — özünün qlobal `namespace`inə (adlar fəzasına) malik, digər `module`lardan asılı olmayan müstəqil bir JavaScript `file`ıdır. Bu o deməkdir ki, bir `module` daxilində təyin edilən hər şey, standart olaraq **`private`dır (qapalıdır)** və yalnız həmin modul daxilində əlçatandır.

Bir dəyərin modul xaricinə çıxarılmasının və başqa bir `module`da istifadə edilməsinin yeganə yolu var:
1.  Dəyəri yaradan `module` onu `export` `keyword`ü (açar sözü) ilə **ixrac (export)** etməlidir.
2.  Dəyəri istifadə edən `module` isə onu `import` `keyword`ü ilə **idxal (import)** etməlidir.

> ℹ️ `Module`lar haqqında daha ətraflı məlumat `Fəsil 10`-da, `import` və `export` sintaksisinin bütün incəlikləri isə `§10.3`-də izah olunacaq.

***

#### **`export`: Moduldan Dəyərləri Paylaşmaq**

`export` bəyannaməsi cari `module`da təyin edilmiş bir və ya daha çox dəyərin digər `module`lar tərəfindən idxal edilə biləcəyini bildirir.

Dəyərləri ixrac etməyin iki əsas üsulu var:

**1. Adlandırılmış İxrac (Named Export):**
Müəyyən adlarla bir neçə dəyəri ixrac etmək üçün istifadə olunur.

```javascript
// Fayl: geometry/constants.js

// İxrac etmək istədiyimiz dəyişənləri təyin edirik
const PI = Math.PI;
const TAU = 2 * PI;

// Onları adları ilə birlikdə ixrac edirik
export { PI, TAU };
```

**2. Standart İxrac (Default Export):**
Bir `module`un əsasən təmsil etdiyi tək bir dəyəri ixrac etmək üçün istifadə olunur. Hər `module`da yalnız **bir** `export default` ola bilər.

```javascript
// Fayl: geometry/circle.js

// Bu modulun "əsas" məqsədi olan Circle sinfini yaradırıq
export default class Circle {
  // ... sinfin tərkibi
}
```

Həmçinin `export` `keyword`ünü birbaşa `declaration`ın önündə `modifier` (modifikator) kimi yazaraq təyin etmə və ixrac prosesini birləşdirmək də mümkündür:

```javascript
// TAU dəyişənini təyin edir VƏ eyni anda ixrac edir
export const TAU = 2 * Math.PI;

// magnitude funksiyasını təyin edir VƏ eyni anda ixrac edir
export function magnitude(x, y) {
  return Math.sqrt(x*x + y*y);
}
```

---

### **`import`: Başqa Modullardan Dəyərləri Almaq**

`import` bizə imkan verir ki, JavaScript kodumuzu kiçik modullara bölək və bir modulda yazdığımız funksiyalar, siniflər və ya dəyişənləri başqa modullarda istifadə edək.

**Başqa sözlə:**

* Bir faylda (modulda) nə isə `export` edirik.
* Başqa faylda isə `import` ilə onu çağırırıq, yəni "alırıq" və istifadə edirik.

---

### `import`-un əsas 3 növü:

---

#### 1. **Default (Standart) ixracın idxalı**

* Əgər modulda bir dəyər `export default` ilə ixrac olunubsa, onu idxal edərkən sadəcə adı yazırıq, mötərizə olmadan.

**Misal:**

```javascript
// circle.js
export default class Circle {
  constructor(radius) {
    this.radius = radius;
  }
  area() {
    return Math.PI * this.radius * this.radius;
  }
}
```

```javascript
// app.js
import Circle from './circle.js';  // Default ixracı idxal edirik

const c = new Circle(5);
console.log(c.area());
```

---

#### 2. **Adlandırılmış (Named) ixracların idxalı**

* Modulda bir neçə dəyər varsa və onlar `export` ilə ayrıca ixrac olunubsa, idxal edərkən adlarını mötərizə `{}` içində yazırıq.

**Misal:**

```javascript
// constants.js
export const PI = 3.14159;
export const TAU = 2 * PI;
```

```javascript
// app.js
import { PI, TAU } from './constants.js';

console.log(PI);  // 3.14159
console.log(TAU); // 6.28318
```

---

#### 3. **Adın dəyişdirilməsi (Alias) idxal zamanı**

* İdxal edərkən, istədiyimiz adla istifadə etmək üçün dəyəri yeni adla gətirə bilərik.

**Misal:**

```javascript
// utils.js
export function magnitude(x, y) {
  return Math.sqrt(x * x + y * y);
}
```

```javascript
// app.js
import { magnitude as hypotenuse } from './utils.js';

console.log(hypotenuse(3, 4)); // 5
```

---

### 5.8 JavaScript `Statements`lərinin Xülasəsi

| `Statement`             | Təsvir                                                                          |
| :---------------------- | :------------------------------------------------------------------------------ |
| `break`                 | Ən daxili `loop`dan və ya `switch`dən, ya da `named enclosing statement`dən çıxır. |
| `case`                  | Bir `switch` daxilindəki `statement`i `label`ləyir.                               |
| `class`                 | Bir `class` `declare` edir.                                                       |
| `const`                 | Bir və ya daha çox `constant`ı `declare` və `initialize` edir.                     |
| `continue`              | Ən daxili `loop`un və ya `named loop`un növbəti `iteration`unu başlayır.            |
| `debugger`              | `Debugger breakpoint`ı.                                                         |
| `default`               | Bir `switch` daxilindəki `default statement`i `label`ləyir.                        |
| `do/while`              | `while loop`a alternativ.                                                          |
| `export`                | Digər `module`lara `import` edilə bilən dəyərləri `declare` edir.                 |
| `for`                   | İstifadəsi asan bir `loop`.                                                         |
| `for/await`             | Bir `async iterator`un dəyərlərini `asynchronously iterate` edir.                 |
| `for/in`                | Bir `object`in `property name`lərini `enumerate` edir.                              |
| `for/of`                | Bir `iterable object`in (`array` kimi) dəyərlərini `enumerate` edir.               |
| `function`              | Bir `function` `declare` edir.                                                    |
| `if/else`               | Bir `condition`dan asılı olaraq bir və ya digər `statement`i icra edir.             |
| `import`                | Digər `module`larda təyin edilmiş dəyərlər üçün adlar `declare` edir.             |
| `label`                 | `break` və `continue` ilə istifadə üçün `statement`ə ad verir.                  |
| `let`                   | Bir və ya daha çox `block-scoped variable`ı `declare` və `initialize` edir (yeni sintaksis). |
| `return`                | Bir `function`dan dəyər qaytarır.                                                  |
| `switch`                | `case` və ya `default` `label`lərə çoxşaxəli keçid.                                |
| `throw`                 | Bir `exception` atır.                                                            |
| `try/catch/finally`     | `Exception`ları idarə edir və kodu təmizləyir (`cleanup`).                        |
| `"use strict"`          | `Script`ə və ya `function`a `strict mode restriction`larını tətbiq edir.             |
| `var`                   | Bir və ya daha çox `variable`ı `declare` və `initialize` edir (köhnə sintaksis).   |
| `while`                 | Əsas bir `loop construct`.                                                         |
| `with`                  | `Scope chain`i genişləndirir (`deprecated` və `strict mode`da qadağandır).           |
| `yield`                 | `Iterate` edilmək üçün bir dəyər təmin edir; yalnız `generator function`larda istifadə olunur. |