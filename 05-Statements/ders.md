# Fəsil 5: Statements (Əmrlər)

### Giriş

4-cü fəsildə **ifadələr** JavaScript-də "ifadələr" (phrases) kimi izah olunurdu. Eyni analoji ilə, **statements** isə JavaScript-in "cümlələri" və ya **əmrləri** kimidir.

İngilis dilində cümlələr nöqtə (`.`) ilə bitdiyi kimi, JavaScript-də də əmrlər **nöqtəli vergül** (`;`) ilə tamamlanır (bax: §2.6).

> 🔸 **İfadələr** bir **dəyər** qaytarır,
> 🔸 **Əmrlər (statements)** isə **nə isə baş verir** deyə icra olunur.

---

### İfadə Əmrləri (Expression Statements)

İfadələr bəzən **side effect** (yəni nəticə olaraq proqram vəziyyətində dəyişiklik) yaradır. Məsələn:

* dəyişənə qiymət təyin etmək (assignment)
* funksiyanı çağırmaq (function invocation)

Bu tip ifadələr təkbaşına da istifadə oluna bilər və onlar bu zaman **expression statement** adlanır.

---

### Təyinat Əmrləri (Declaration Statements)

Bunlar yeni dəyişən və ya funksiya **təqdim etmək (declare)** üçün istifadə olunur:

* `let`, `const`, `var` — dəyişənlər üçün
* `function` — funksiyalar üçün

---

### JavaScript Proqramları Necə Qurulur?

JavaScript proqramları sadəcə **əmrlər ardıcıllığıdır**.

1. Proqramda əmrlər **yuxarıdan aşağıya**, **sıra ilə** icra olunur.
2. Amma bu sıranı dəyişmək də mümkündür — bunun üçün müxtəlif **idarəetmə strukturları (control structures)** mövcuddur:

---

### Əsas İdarəetmə Struktur Tipi Əmrlər:

#### ✅ Şərti Əmrlər (Conditionals)

Belə əmrlər ifadənin qiymətindən asılı olaraq digər əmrləri **icra edir** və ya **ötürür**:

* `if`
* `switch`

#### 🔁 Dövr Əmrləri (Loops)

Digər əmrləri **dəfələrlə təkrarlamaq** üçün istifadə olunur:

* `while`
* `for`

#### 🔀 Tullanma Əmrləri (Jumps)

Proqramın axarını başqa yerə **tullanmaq** üçün istifadə olunur:

* `break`
* `return`
* `throw`

---

### Nəticə

Bu fəsildə JavaScript-in müxtəlif əmrləri və onların sintaksisi öyrədiləcək. Fəsilin sonunda yerləşən **Cədvəl 5-1** bu əmrlərin sintaksisini ümumiləşdirəcək.

> JavaScript proqramı — sadəcə **əmrlər ardıcıllığıdır**.
> Hər bir əmr **nöqtəli vergül** (`;`) ilə ayrılır.
> Bu əmrləri öyrəndikdən sonra **öz JavaScript proqramlarını yazmağa** başlaya bilərsən!

---
## 5.1 İfadə Əmrləri (Expression Statements)

JavaScript-də ən sadə əmrlər — **side effect** yaradan **ifadələr**dir. Bu cür əmrlər haqqında **4-cü fəsildə** də məlumat verilmişdi.

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

Bu əmrlər **sadəcə qiymət qaytarmır**, həm də **dəyişəni dəyişir** — yəni **side effect** yaradır.

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
displaySpinner(); // Tutaq ki, bu funksiya web tətbiqdə spinner göstərir
```

Bu funksiyalar bir **dəyər** qaytara bilər, amma əsas məqsəd **nə isə baş verməsi** — side effect yaratmaqdır.

Əgər bir funksiyanın heç bir yan təsiri yoxdursa, onu sadəcə çağırmaq **mənasızdır**:

```js
Math.cos(x); // Mənasız, çünki nəticə istifadə olunmur
```

Amma həmin nəticəni **saxlayıb istifadə edəcəksənsə**, bu zaman mənalı olur:

```js
cx = Math.cos(x); // Dəyəri dəyişəndə saxladıq
```

---

### ℹ️ Qeyd

Bütün bu nümunələrdəki kod sətrləri **nöqtəli vergül** (`;`) ilə bitir. JavaScript-də bu — ifadənin bitdiyini göstərir və çox vacibdir.

---

## 5.2 Mürəkkəb və Boş Əmrlər (Compound and Empty Statements)

### 🧱 Mürəkkəb Əmr (Compound Statement)

**Virgül operatoru** (§4.13.7) bir neçə ifadəni birləşdirib tək ifadə etdiyinə bənzər şəkildə, **əmr blokları** bir neçə əmri birləşdirərək **tək əmr** kimi istifadə olunmasına imkan verir.

Əmr bloku sadəcə **sağ və sol qıvrım mötərizələr** (`{}`) arasında yazılmış **əmr ardıcıllığıdır**. Məsələn:

```js
{
  x = Math.PI;
  cx = Math.cos(x);
  console.log("cos(π) = " + cx);
}
```

Bu blok — tək bir əmr kimi istənilən yerdə istifadə oluna bilər.

---

#### 📌 Nəzərə alın:

1. **Blok özü nöqtəli vergüllə bitmir.**
   — Blokdakı daxili əmrlər `;` ilə bitir, amma `block` özü `;` ilə bitmir.

2. **Daxili sətrlər adətən indent (boşluq) ilə yazılır.**
   — Bu məcburi deyil, amma kodun oxunaqlığını artırır.

---

### 🔁 Alt-Əmrlər (Substatements)

Bir çox JavaScript əmrinin **daxilində başqa bir əmr** olur. Məsələn, `while` dövrü yalnız **bir alt-əmr** tələb edir:

```js
while (i < 10)
  i++;
```

Amma əgər biz **bir neçə əmr** yazmaq istəyiriksə, onları `{}` içində birləşdirərək **mürəkkəb əmr** halına salırıq:

```js
while (i < 10) {
  console.log(i);
  i++;
}
```

---

### ❌ Boş Əmr (Empty Statement)

**Boş əmr** — heç bir əmrin olmadığı, sadəcə `;` olan bir sətrdir:

```js
;
```

Bu, JavaScript tərəfindən **sadəcə ötülür** — heç bir iş görülmür. Nadir hallarda, bəzi dövrlərdə faydalı ola bilər.

#### Məsələn:

```js
for (let i = 0; i < a.length; a[i++] = 0) ;
```

Bu `for` dövründə bütün iş `a[i++] = 0` ifadəsində görülür, dövr bədəninə ehtiyac yoxdur. Amma JavaScript `for` sintaksisində mütləq bir bədən (statement) tələb etdiyi üçün, **boş əmr** (`;`) istifadə olunur.

---

### ⚠️ Diqqət: Səhvən Yazılmış `;`

Aşağıdakı kimi **səhvən qoyulmuş nöqtəli vergül** kodun işləmə məntiqini poza bilər:

```js
if ((a === 0) || (b === 0)); // Ups! Bu sətr heç nə etmir
  o = null; // bu sətr isə HƏR ZAMAN icra olunur
```

Yəni `if` heç bir şərt yerinə yetirmir, `o = null` isə həmişə işləyir.

#### ✅ Doğru istifadə üçün belə şərh əlavə etmək yaxşıdır:

```js
for (let i = 0; i < a.length; a[i++] = 0) /* boş dövr */ ;
```

Bu cür **şərh (comment)**, məqsədli olaraq boş əmr yazdığını göstərir.

---

## 5.3 Şərt Əmrləri (Conditionals)

Şərt əmrləri (`conditional statements`) — bir ifadənin dəyərinə əsasən digər əmrləri **icra edir və ya ötürür**.
Bu əmrlər **kodun qərar nöqtələridir** və bəzən **"budaqlanma (branching)"** əmrləri də adlanır.

JavaScript interpretatorunun kod boyunca getdiyini təsəvvür etsək, **şərt əmrləri** onun hansı yolla gedəcəyini müəyyən edir.

Bu bölmədə aşağıdakılar izah edilir:

* Əsas `if/else` əmri
* Daha mürəkkəb və çoxyollu `switch` əmri

---

## 5.3.1 `if`

`if` — JavaScript-də ən əsas **nəzarət əmri**dir. Onun köməyi ilə **şərtə əsasən əmrlərin icrası** həyata keçirilir.

### `if` Əmri – 1-ci Forma:

```js
if (şərt)
  əməl
```

* Əgər `şərt` doğru (`truthy`) dəyər qaytarırsa, **əməl icra olunur**.
* Əgər `şərt` yanlış (`falsy`) dəyər qaytarırsa, **əməl ötürülür**.

📌 **Truthiness və Falsiness** haqqında ətraflı məlumat üçün bax: §3.4

#### Nümunə:

```js
if (username == null)
  username = "John Doe";
```

və ya daha qısa şəkildə:

```js
if (!username)
  username = "John Doe";
```

Bu, `username` dəyişəni `null`, `undefined`, `false`, `0`, `""`, və ya `NaN` olduqda yeni dəyər təyin edir.

📌 **Qeyd**: `if` şərtindəki mötərizə **məcburidir**.

---

### `if` Bloklu Forma:

Əgər `if`-dən sonra **bir neçə əmri birlikdə** yazmaq istəyirsənsə, `{}` istifadə et:

```js
if (!address) {
  address = "";
  message = "Zəhmət olmasa ünvanı qeyd edin.";
}
```

---

### `if/else` Əmri – 2-ci Forma:

```js
if (şərt)
  əməl1
else
  əməl2
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

## ❗ `else` ilə iç-içə (`nested if`) diqqətli olun

Aşağıdakı kodda `else` hansı `if`-ə aiddir? Diqqətlə baxın:

```js
i = j = 1;
k = 2;

if (i === j)
  if (j === k)
    console.log("i = k");
  else
    console.log("i j-yə bərabər deyil"); // PROBLEM!
```

Burada `else`, **ən yaxın `if`**-ə aiddir (`j === k`). Amma indent (boşluq) bizə başqa cür göstərə bilər.

JavaScript bunu belə başa düşür:

```js
if (i === j) {
  if (j === k)
    console.log("i = k");
  else
    console.log("i j-yə bərabər deyil"); // Həqiqətdə bu işləyir!
}
```

---

### ✅ Düzgün yazılış (qarışıqlığın qarşısını almaq üçün):

```js
if (i === j) {
  if (j === k) {
    console.log("i = k");
  }
} else {
  console.log("i j-yə bərabər deyil");
}
```

💡 **Qayda**: JavaScript-də `else` **həmişə ən yaxın `if`** ilə bağlı olur.

---

## 💡 TÖVSİYƏ

Bir çox proqramçı **həmişə** `if`, `else`, `while` və digər strukturlarda əmrləri `{}` ilə yazır, **tək sətr olsa belə**:

```js
if (x > 0) {
  console.log("müsbət");
}
```

Bu vərdiş:

* Qarışıqlığın qarşısını alır
* Kodu oxumağı və dəyişməyi asanlaşdırır

📌 Qeyd: Bu kitabda kodlar daha yığcam görünsün deyə bu qayda bəzən pozulur.

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

### `else if` Sintaksisi:

```js
if (n === 1) {
  // Kod bloku #1
} else if (n === 2) {
  // Kod bloku #2
} else if (n === 3) {
  // Kod bloku #3
} else {
  // Əgər yuxarıdakılar heç biri ödənmirsə — Kod bloku #4
}
```

Bu yazılışda:

* Əgər `n === 1` → blok #1 işləyir
* Əgər `n === 2` → blok #2
* Əgər `n === 3` → blok #3
* Əks halda → blok #4 (default vəziyyət)

---

### Niyə `else if` istifadə etməliyik?

Bu yazılış:

* Daha **səliqəli** görünür
* Daha **oxunaqlıdır**
* Yazmaq və anlamaq **daha asandır**

---

### 💡 Bərbad nümunə (bunu etmə!):

Aşağıdakı kod **tam sintaktik olaraq doğrudur**, amma **oxumaq çətindir**:

```js
if (n === 1) {
  // Kod #1
} else {
  if (n === 2) {
    // Kod #2
  } else {
    if (n === 3) {
      // Kod #3
    } else {
      // Kod #4
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

## 5.3.3 `switch` – çoxyönlü şərtləri daha təmiz yazmaq üçün

Bir `if` ifadəsi proqramın axışında budaqlanma (yəni şaxələnmə) yaradır və `else if` istifadə etməklə çoxyönlü budaqlanmalar etmək mümkündür.

Ancaq belə bir çoxyönlü `if` blokları hər dəfə **eyni ifadəni** yoxlayırsa (məsələn: `n === 1`, `n === 2`, və s.), bu zaman həmin ifadənin **dəfələrlə təkrar yoxlanılması** artıq olur. Həm oxunaqlılığı azaldır, həm də performans baxımından səmərəsiz olur.

---

### 🧠 Nə vaxt `switch` istifadə etməliyik?

Əgər **bütün şərtlər eyni ifadəyə (deyək ki, `n`) əsaslanırsa**, `switch` daha münasibdir. Çünki `switch` həmin ifadəni **yalnız bir dəfə** hesablayır və sonra onun nəticəsini `case`-lərlə müqayisə edir.

---

## 📘 `switch` ifadəsinin sintaksisi:

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

## 🧪 Əvvəlki `if/else` nümunəsi ilə müqayisə:

Əgər `if/else` belədirsə:

```js
if (n === 1) {
  // kod bloku #1
} else if (n === 2) {
  // kod bloku #2
} else if (n === 3) {
  // kod bloku #3
} else {
  // kod bloku #4 (default)
}
```

Bu `switch` ilə belə yazılır:

```js
switch (n) {
  case 1:
    // kod bloku #1
    break;
  case 2:
    // kod bloku #2
    break;
  case 3:
    // kod bloku #3
    break;
  default:
    // kod bloku #4
    break;
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

## 👨‍💻 Daha realist nümunə:

```js
function convert(x) {
  switch (typeof x) {
    case "number":
      // Əgər x rəqəmdir, onu 16-lıq formada qaytar
      return x.toString(16);

    case "string":
      // Əgər x sətirdirsə, onu dırnaqlara al
      return '"' + x + '"';

    default:
      // Digər hallarda String() ilə çevrilir
      return String(x);
  }
}
```

Bu funksiyada `switch` dəyişənin tipinə (`typeof x`) əsaslanır və müxtəlif çevirmələr edir:

* Əgər ədəddirsə → 16-lıq formada (hex)
* Əgər sətrdirsə → qoşa dırnaqla əhatələnir
* Əgər başqa tipdirsə → `String()` funksiyası ilə çevrilir

---

## 💡 Əlavə qeydlər:

* Ən çox hallarda `case`-dən sonra **sabit dəyərlər** yazılır (`1`, `"string"`, və s.).
* Amma ECMAScript standartına görə, `case`-dən sonra **istənilən ifadə** (expression) yaza bilərsən.
* Uyğunluq **`===` ilə yoxlanılır**, `==` yox! Yəni **tip uyğunluğu da olmalıdır**.
* Hər `switch` işlədildikdə **bütün `case` ifadələri hesablanmır**. Ona görə də, `case` ifadələrində **funksiya çağırışı və ya yan təsirli (side-effect) kodlar** yazmaq tövsiyə edilmir.

---

## 🎯 Nəticə:

* `switch` – çoxsaylı şərtlər üçün **daha təmiz və performanslı** yanaşmadır.
* `break` və `default` düzgün istifadə olunmalıdır.
* `case` uyğunluğu `===` ilə aparılır, ona görə də **dəqiq uyğunluq** vacibdir.
* Yan təsiri olmayan sabit `case` ifadələri daha təhlükəsizdir.

---


# 5.4 Dövrlər (Loops)

Şərt ifadələrini başa düşmək üçün biz JavaScript tərcüməçisinin (interpreter) sənin kodunu oxuyarkən bir budaqlanma yolunu izlədiyini təsəvvür etmişdik. Dövrü (looping) ifadələr isə bu yolun öz-özünə qayıdıb kodun müəyyən hissəsini təkrar-təkrar işlətməsini təmin edir.

JavaScript-də beş növ dövr var:

* `while`
* `do/while`
* `for`
* `for/of` (və onun `for/await` variantı)
* `for/in`

Aşağıdakı alt-bölmələrdə hər birinin izahı veriləcək.

> Dövrlərin ən çox istifadə olunduğu yerlərdən biri massivlərin elementləri üzərində təkrarlama (iteration) aparmaqdır. Bu mövzu §7.6-da ətraflı şəkildə izah olunub və Array sinfinin xüsusi dövr metodları da orada təsvir edilir.

---

## 5.4.1 `while`

`while` ifadəsi JavaScript-in ən əsas dövr operatorudur, elə `if` ifadəsi kimi.

Sintaksisi belədir:

```js
while (ifadə)
  ifadə_bloqu
```

**İzahı:**

* Tərcüməçi (interpreter) əvvəlcə `ifadə`-ni qiymətləndirir (yəni yoxlayır).
* Əgər `ifadə` **false** (yalan) dəyər verirsə, o zaman dövrün bədəni (`ifadə_bloqu`) atlanır və proqram növbəti əmri icra edir.
* Əgər `ifadə` **true** (doğru) dəyər verirsə, dövr bədəni icra olunur və sonra yenidən `ifadə` yoxlanılır.
* Bu proses `ifadə` yalan dəyər alana qədər davam edir.

---

### Sonsuz dövr

Məsələn, `while(true)` sonsuz dövr yaradır, yəni proqram o anda dayanmır.

---

### Dövrdə dəyişənlərin rolu

Adətən, hər dövrün sonunda bir və ya bir neçə dəyişən dəyişir. Bu dəyişənlər dövrün şərtində (`ifadə` hissəsində) iştirak edirsə, dövrün şərtinin nəticəsi hər dəfə dəyişə bilər və nəticədə dövr sona çata bilər.

Əgər dövr şərti həmişə doğru qalarsa, dövr heç vaxt bitməz.

---

### Misal: 0-dan 9-a qədər saydırmaq

```js
let count = 0;

while (count < 10) {
  console.log(count);
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
  ifadə_bloqu
while (ifadə);
```

---

### İstifadəsi

* `do/while` dövrü `while`-dan daha az istifadə olunur, çünki çox vaxt dövrün ən azı bir dəfə icra olunacağı dəqiq deyil.
* Amma əgər sən mütləq dövr bədənini bir dəfə icra etmək istəyirsənsə, onda `do/while` yaxşı seçimdir.

---

### Misal: massiv elementlərini çap etmək

```js
function printArray(a) {
  let len = a.length, i = 0;
  
  if (len === 0) {
    console.log("Empty Array");
  } else {
    do {
      console.log(a[i]);
    } while (++i < len);
  }
}
```

**İzah:**

* Əgər massiv boşdursa (`len === 0`), "Empty Array" mesajı çap olunur.
* Əks halda, `do` bloku ən azı bir dəfə işə düşür və massiv elementləri tək-tək çap olunur.
* Dövr `i` dəyişəni massiv uzunluğundan kiçik olduğu müddətcə davam edir.

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
  console.log(i);
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

**for/of** — ES6-da əlavə olunmuş yeni dövr (loop) operatorudur. Bu, `for` açar sözü ilə başlayır, amma adi `for` dövründən tam fərqlidir. Həmçinin, əvvəlki `for/in` dövründən də fərqlidir (onun izahı §5.4.5-dədir).

### Iterable Objects (İterasiya oluna bilən obyektlər)

`for/of` dövrü **iterable** obyektlərlə işləyir. İterable obyektlər barədə daha geniş izahı 12-ci fəsildə verəcəyik, amma sadəcə bilmək kifayətdir ki, **arrays (massivlər), strings (sətirlər), sets və maps** iterable hesab olunur. Bu obyektlər elementlər ardıcıllığını təmsil edir və `for/of` ilə üzərində addım-addım hərəkət etmək mümkündür.

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

## FOR/OF İlə OBJECT-lər

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
let o = { x: 1, y: 2, z: 3 };
for(let k of Object.keys(o)) {
  console.log(k); // x, y, z
}

let sum = 0;
for(let v of Object.values(o)) {
  sum += v;
}
console.log(sum); // 6
```

* `Object.entries()` isə həm açarları, həm də dəyərləri almağa imkan verir, destructuring ilə rahat işləyir:

```js
for(let [k, v] of Object.entries(o)) {
  console.log(k, v); // x 1, y 2, z 3
}
```

---

## FOR/OF İlə STRINGS

* String-lər ES6-da hər bir simvol üzrə iterasiya olunur.

```js
let frequency = {};
for(let letter of "mississippi") {
  frequency[letter] = (frequency[letter] || 0) + 1;
}
console.log(frequency); // {m:1, i:4, s:4, p:2}
```

* Qeyd: String-lər UTF-16 simvoluna deyil, Unicode codepoint-lərə görə iterasiya olunur.

---

## FOR/OF İlə SET və MAP

* **Set** və **Map** sinifləri də iterable-dir.

```js
let wordSet = new Set("Na na na na na na na na Batman!".split(" "));
for(let word of wordSet) {
  console.log(word);
}
```

* **Map** iterasiya ediləndə hər iterasiyada açar/dəyər cütü qaytarılır:

```js
let m = new Map([[1, "one"]]);
for(let [key, value] of m) {
  console.log(key, value); // 1 "one"
}
```

---

## ASYNCHRONOUS ITERATION WITH FOR/AWAIT

* ES2018 ilə **asynchronous iterator** və `for/await` dövrü gəldi.
* `for/await` **async iterator** ilə işləyir, yəni asinxron məlumat axını üçün istifadə olunur.

```js
async function printStream(stream) {
  for await (let chunk of stream) {
    console.log(chunk);
  }
}
```

---

### 5.4.5 for/in

`for/in` döngüsü `for/of` döngüsünə çox bənzəyir, sadəcə `of` açarı `in` ilə əvəz olunur. `for/of` döngüsündə `of`-dan sonra iterasiya edilə bilən obyekt tələb olunur, amma `for/in` döngüsü `in`-dən sonra istənilən obyektlə işləyir. `for/of` ES6 ilə gəlib, amma `for/in` JavaScript-in əvvəldən olan xüsusiyyətidir, ona görə də sintaksisi daha təbii səslənir.

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
for(let p in o) {
  // p dəyişəninə o obyektinin property adları verilir
  console.log(o[p]); // Hər property-nin dəyərini çap edir
}
```

`for/in` işlədilərkən, əvvəlcə `object` ifadəsi qiymətləndirilir. Əgər nəticə `null` və ya `undefined` olsa, döngü keçir və növbəti ifadəyə keçir. Daha sonra obyektin bütün **enumerable** (sayğacda olan) property-ləri üçün döngü gövdəsi bir dəfə işləyir. Hər iterasiyadan əvvəl, `variable` dəyişəninə həmin property-nin adı (string) təyin edilir.

Qeyd etmək lazımdır ki, `for/in`-də `variable` istənilən sol tərəf ifadəsi ola bilər, yəni hər iterasiyada fərqli qiymət ala bilər. Məsələn, bütün property adlarını array-a kopyalamaq üçün belə istifadə edilə bilər:

```js
let o = { x: 1, y: 2, z: 3 };
let a = [], i = 0;
for(a[i++] in o) /* boş */;
```

JavaScript-də massivlər sadəcə xüsusi obyektlərdir və onların indeksləri obyektin property-ləri sayılır, buna görə `for/in` ilə massiv indekslərini də dövr etmək olar. Məsələn:

```js
for(let i in a) console.log(i);
```

bu, massiv `a`-nın indekslərini (0, 1, 2 və s.) çap edir.

Mənim öz kodumda tez-tez səhvən massivlərdə `for/in` istifadə etdiyim üçün səhvlər baş verir, əslində massivlərlə işləyərkən demək olar ki, həmişə `for/of` istifadə etmək lazımdır.

`for/in` döngüsü obyektin bütün property-lərini deyil, yalnız enumerable (sayğacda olan) string adlarını dövr edir. Symbol tipli property-ləri dövr etmir. Məsələn, `toString()` metodu bütün obyektlərdə mövcuddur, amma bu metod `for/in`-də siyahıya düşmür, çünki o enumerable deyil. Sənin kodunda yaradılan bütün property-lər default olaraq enumerable olur (yəni siyahıya düşür), amma sən onları qeyri-enumerable də edə bilərsən.

`for/in` həmçinin **inherit edilmiş enumerable** property-ləri də dövr edir. Bu o deməkdir ki, əgər sən kodunda bütün obyektlərə miras qalan property-lər əlavə etmisənsə, döngü gözlənilməz nəticələr verə bilər. Buna görə çox proqramçılar `for/in` yerinə `for/of` ilə `Object.keys()` istifadə etməyi üstün tuturlar.

Əgər döngünün içində hələ siyahıya düşməmiş bir property silinərsə, o property dövr olunmur. Əgər yeni property əlavə edilərsə, onlar bəzən siyahıya düşə bilər, bəzən yox. Daha ətraflı məlumat üçün JavaScript-ın property enumerasiyası qaydalarına baxmaq lazımdır.

---

**5.5 Tullanma Operatorları (Jump Statements)**

JavaScript-də **tullanma operatorları** kodunuzun icra axınını dəyişməyə imkan verən əmrlərdir. Onlar proqramın bir hissəsindən digərinə keçməyə kömək edir.

Əsas tullanma operatorları bunlardır:

* **`break`**: Bir dövrü (loop) və ya `switch` operatorunu dərhal bitirir və ondan sonrakı koda keçir.
* **`continue`**: Bir dövrün cari addımının (iterasiyasının) qalan hissəsini atlayır və dövrün növbəti addımına başlayır.
* **`return`**: Bir funksiyadan çıxır və lazım gələrsə, bir dəyər qaytarır.
* **`throw`**: Bir xəta (exception) yaradır və proqramın ən yaxın xəta idarəedicisinə keçməsinə səbəb olur.

Bu operatorlar haqqında daha ətraflı məlumat növbəti bölmələrdə veriləcək.

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
mainloop: while(token !== null) {
  // Bəzi kodlar...

  if (token === "skip") {
    continue mainloop; // mainloop dövrünün növbəti addımına keçir
  }

  // Daha çox kod...
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
for(let i = 0; i < a.length; i++) {
  if (a[i] === target) break; // `target` tapıldıqda dövrü dayandırır
}
// Əgər dövr buradan çıxıbsa, ya `target` tapılıb, ya da `array`in sonuna çatılıb.
```

**Labeled `break` (`break labelname;`)**

`break` operatorunu bir `label`lə də istifadə edə bilərsiniz:

```javascript
break labelname;
```

**Labeled `break`** etiketlənmiş `statement`in sonuna tullanır və onu dayandırır. Bu, əsasən iç-içə dövrlərdən (`nested loops`) və ya digər `labeled` bloklardan çıxmaq üçün faydalıdır. `Labeled statement` dövr və ya `switch` olmaya da bilər, hər hansı bir `labeled statement` ola bilər.

**Vacib qeyd:** `break` sözü ilə `labelname` arasına yeni sətir (`newline`) qoymaq olmaz. Əks halda JavaScript onu sadə `break;` kimi qəbul edə bilər.

**Nümunə:** Matrisdə (2D `array`) cəmləmə zamanı xəta olarsa, dərhal `computeSum` blokundan çıxmaq:

```javascript
let matrix = getData(); // İki ölçülü `array` əldə et
let sum = 0, success = false;

computeSum: if (matrix) { // `computeSum` adlı labeled blok
  for(let x = 0; x < matrix.length; x++) {
    let row = matrix[x];
    if (!row) break computeSum; // Sətr boşdursa, `computeSum` blokundan çıx

    for(let y = 0; y < row.length; y++) {
      let cell = row[y];
      if (isNaN(cell)) break computeSum; // Dəyər ədəd deyilsə, `computeSum` blokundan çıx
      sum += cell;
    }
  }
  success = true; // Hər şey uğurla tamamlandı
}

// Bura ya bütün cəmləmə bitəndə, ya da `break computeSum` işə düşəndə gəlinir.
// `success == false` olarsa, matrisdə problem var idi.
// Əks halda, `sum` matrisdəki bütün ədədlərin cəmini saxlayır.
```

**Mühüm məhdudiyyət:** `break` operatoru, `labeled` olsun ya olmasın, funksiya sərhədlərini aşmır. Yəni, bir funksiya təyinini `label`ləyib, funksiyanın içərisindən o `label`i istifadə edərək "tullana" bilməzsiniz.

---

### 5.5.3 `continue` operatoru

`continue` operatoru `break` operatoruna bənzəyir, lakin dövrü dayandırmaq əvəzinə, dövrü növbəti `iteration`dan yenidən başladır.

**Sadə istifadəsi:**

```javascript
continue;
```

`continue` operatoru **yalnız** dövrün (`loop`) daxilində istifadə edilə bilər. Başqa yerdə istifadə olunarsa, `syntax error` verər.

**Labeled `continue` (`continue labelname;`)**

`continue` operatoru bir `label`lə də istifadə edilə bilər:

```javascript
continue labelname;
```

**`continue` işləyəndə:**

`continue` operatoru icra edildikdə, dövrün cari `iteration`u bitirilir və növbəti `iteration` başlayır. Bu, dövrün növündən asılı olaraq bir qədər fərqli işləyir:

* **`while` dövründə:** Dövrün əvvəlindəki şərt (`expression`) yenidən yoxlanılır və `true` olarsa, dövrün `body`si yuxarıdan başlayaraq icra edilir.
* **`do/while` dövründə:** İcra dövrün altına keçir, burada dövrün şərti yenidən yoxlanılır və sonra dövr yuxarıdan yenidən başlayır.
* **`for` dövründə:** Əvvəlcə `increment expression` (artırma ifadəsi) qiymətləndirilir, sonra test `expression` yenidən yoxlanılır ki, növbəti `iteration` olub-olmayacağı müəyyənləşsin.
* **`for/of` və ya `for/in` dövründə:** Dövr növbəti `iterated value` və ya növbəti `property name` təyin edilərək yenidən başlayır.

**Nümunə:** Bu nümunədə, `data[i]` `undefined` olarsa, cari `iteration`ın qalan hissəsi atlanır:

```javascript
for(let i = 0; i < data.length; i++) {
  if (!data[i]) continue; // `data[i]` undefined isə, cari `iteration`ı atla
  total += data[i]; // Yalnız `data[i]` undefined olmayanda icra olunur
}
```

**`Labeled continue`** `nested loops` daxilində istifadə edilə bilər ki, yenidən başlanmalı olan dövr dərhal onu əhatə edən dövr olmasın.

**Vacib qeyd:** `break` operatorunda olduğu kimi, `continue` sözü ilə `labelname` arasına yeni sətir (`newline`) qoymaq olmaz.

---

### 5.5.4 `return` operatoru

Xatırlayın ki, funksiya çağırışları (`function invocations`) `expression`lardır və bütün `expression`ların dəyəri (`value`) var. `return` operatoru bir funksiyanın daxilində istifadə olunur və bu funksiya çağırışının dəyərini müəyyən edir.

**Sintaksisi:**

```javascript
return expression;
```

`return` operatoru yalnız funksiyanın `body`si daxilində istifadə edilə bilər. Başqa yerdə istifadə olunması `syntax error`a səbəb olar.

`return` operatoru icra edildikdə, onu ehtiva edən funksiya, `expression`ın dəyərini onu çağıran yerə (`caller`) qaytarır.

**Nümunə:**

```javascript
function square(x) {
  return x * x; // `x` dəyərinin kvadratını qaytarır
}

square(2) // => 4 (funksiya 4 dəyərini qaytarır)
```

Əgər bir funksiyada `return` operatoru olmazsa, funksiya bütün `statement`ləri ardıcıl olaraq icra edir və sonunda çağıran yerə `undefined` dəyərini qaytarır.

`return` operatoru çox vaxt funksiyanın son `statement`i olur, lakin bu mütləq deyil. `return statement` icra olunduğu an funksiya çağırana geri dönür, hətta funksiyanın `body`sində başqa `statement`lər qalsa belə.

`return` operatoru `expression` olmadan da istifadə edilə bilər. Bu halda, funksiya çağıran yerə `undefined` qaytarır.

**Nümunə:**

```javascript
function displayObject(o) {
  // Əgər `o` null və ya undefined olarsa, dərhal geri dön.
  if (!o) return; // undefined qaytarır

  // Funksiyanın qalan kodu burada yerləşir...
}
```

**Vacib qeyd:** JavaScript-in avtomatik nöqtəli vergül əlavə etmə (`automatic semicolon insertion`) xüsusiyyətinə görə, `return` `keyword`i ilə ondan sonra gələn `expression` arasında yeni sətir (`line break`) qoymaq olmaz.

---

### 5.5.5 `yield` operatoru

`yield` operatoru `return` operatoruna bənzəyir, lakin yalnız **generator functions** (ES6-da təqdim olunub) daxilində istifadə olunur. `yield` funksiyanın icrasını dayandırır və `generated sequence`də növbəti `value`nu verir, lakin funksiyadan tamamilə çıxmır.

**Nümunə:**

```javascript
// Integer aralığını qaytaran bir generator function
function* range(from, to) {
  for(let i = from; i <= to; i++) {
    yield i; // Hər iterationda `i` dəyərini verir
  }
}
```

**Qeyd:** `yield` operatorunu tam başa düşmək üçün **iterators** və **generators** mövzularını bilmək lazımdır. Bu mövzular **Fəsil 12**-də daha ətraflı izah olunacaq. Burada sadəcə `jump statement`ləri tamamlamaq üçün qeyd edilmişdir. (`yield` texniki olaraq bir `statement` deyil, bir `operator`dur, Fəsil 12.4.2-də izah olunacaq.)

---

### 5.5.6 `throw` operatoru

**Exception** (`istisna`) — proqramda qeyri-adi bir vəziyyətin və ya xətanın baş verdiyini bildirən bir siqnaldır. Bir `exception` "atmaq" (`to throw`) belə bir xəta və ya qeyri-adi vəziyyəti bildirmək deməkdir. Bir `exception` "tutmaq" (`to catch`) isə onu idarə etmək, yəni xətadan bərpa olmaq üçün lazımi addımları atmaqdır.

JavaScript-də `runtime` zamanı xəta baş verdikdə və ya proqram açıq şəkildə `throw` operatorundan istifadə edərək bir `exception` atdıqda `exception`lər yaranır. `Exception`lər `try/catch/finally statement`i ilə tutulur, bu, növbəti bölmədə izah ediləcək.

`throw` operatorunun sintaksisi belədir:

```javascript
throw expression;
```

`expression` istənilən tipdə bir dəyər ola bilər. Siz xəta kodunu bildirən bir rəqəm və ya insan tərəfindən oxuna bilən bir xəta mesajı olan bir `string` ata bilərsiniz. JavaScript `interpreter`inin özü bir xəta atdıqda `Error class`ı və onun alt `class`ları istifadə olunur və siz də onları istifadə edə bilərsiniz. Bir `Error object`inin `name` `property`si xətanın tipini, `message` `property`si isə `constructor function`a ötürülən `string`i saxlayır.

**Nümunə:** Bu nümunədə, `invalid argument` ilə çağırıldıqda `Error object` atan bir funksiya göstərilir:

```javascript
function factorial(x) {
  // Əgər input argument invalid isə, bir exception at!
  if (x < 0) throw new Error("x must not be negative");

  // Əks halda, dəyəri hesabla və normal qaytar
  let f;
  for(f = 1; x > 1; f *= x, x--) /* empty */ ;
  return f;
}

factorial(4) // => 24
// factorial(-1) // Bu, 'Error: x must not be negative' exception atar
```

Bir `exception` atıldıqda, JavaScript `interpreter`i normal proqram icrasını dərhal dayandırır və ən yaxın `exception handler`ə tullanır. `Exception handler`lər `try/catch/finally statement`inin `catch clause`u ilə yazılır (növbəti bölmədə izah olunacaq).

Əgər `exception`ın atıldığı `code block`un əlaqəli bir `catch clause`u yoxdursa, `interpreter` növbəti yuxarı `code block`u yoxlayır. Bu, bir `handler` tapılana qədər davam edir. Əgər funksiyanın özündə `try/catch/finally statement` yoxdursa, `exception` funksiyanı çağıran koda `propagate` edir (ötürülür). Bu şəkildə `exception`lər JavaScript metodlarının `lexical structure`u və `call stack` üzrə yuxarı doğru yayılır. Əgər heç bir `exception handler` tapılmazsa, `exception` bir xəta kimi qəbul edilir və istifadəçiyə bildirilir.

---

### 5.5.7 `try/catch/finally`

`try/catch/finally` `statement`i JavaScript-in `exception handling` mexanizmidir. Bu, xətaların proqramın işini dayandırmadan idarə olunmasına kömək edir.

* **`try` bloku:** Xəta yarana biləcək kodu ehtiva edir. Normalda, bu kod heç bir problem olmadan icra olunur.
* **`catch` bloku:** Əgər `try` blokunda bir `exception` yaranarsa, bu blokdakı `statement`lər icra olunur. `catch` açar sözündən sonra mötərizədə bir `identifier` (məsələn, `e` və ya `ex`) gəlir. Bu `identifier` atılan `exception` dəyərini (adətən bir `Error object`i) saxlayır. Bu `identifier` yalnız `catch` blokunun daxilində görünür (`block scope`).
* **`finally` bloku:** Bu blokdakı kod, `try` blokunda nə baş verməsindən asılı olmayaraq, **həmişə** icra olunur. Adətən `cleanup` (təmizləmə) işləri üçün istifadə olunur.

Həm `catch`, həm də `finally` blokları `optional`dır, lakin bir `try` bloku bunlardan ən azı biri ilə müşayiət olunmalıdır. Bütün `try`, `catch`, və `finally` blokları fiqurlu mötərizələrlə ({ }) başlayıb bitir və bu mötərizələr məcburidir.

**Ümumi Sintaksis və Məqsəd:**

```javascript
try {
  // Normalda problemsiz işləyən kod.
  // Lakin bəzən (məsələn, 'throw' ilə) exception yarada bilər.
}
catch(e) {
  // Bu blok, yalnız 'try' blokunda exception yaranarsa icra olunur.
  // 'e' yerli dəyişəni atılan 'Error object'ini və ya digər dəyəri bildirir.
  // Exception'ı idarə edə, görməməzlikdən gələ və ya yenidən ata bilərsiniz ('rethrow').
}
finally {
  // Bu blokdakı statement'lər həmişə icra olunur.
  // 'try' bloku normal bitsə də, 'break', 'continue' və ya 'return' ilə bitsə də,
  // ya da 'catch' tərəfindən idarə olunan və ya idarə olunmayan bir exception ilə bitsə də.
}
```

**Real Həyat Nümunəsi:**

Aşağıdakı nümunədə, `factorial()` metodu istifadəçi inputunu yoxlayır. Əgər `input` `invalid` olarsa, `exception` atılır və `catch` bloku xətanı idarə edir.

```javascript
try {
  // İstifadəçidən müsbət tam ədəd daxil etməsini istəyin
  let n = Number(prompt("Please enter a positive integer", ""));

  // Input valid hesab olunaraq, ədədin factorial'ini hesablayın
  let f = factorial(n); // Əvvəlki bölmədəki factorial() funksiyası

  // Nəticəni göstərin
  alert(n + "! = " + f);
}
catch(ex) { // Əgər istifadəçinin inputu valid deyildisə, bura gəlirik
  alert(ex); // İstifadəçiyə xətanın nə olduğunu deyin
}
// Bu nümunədə finally clause yoxdur.
```

**`finally` blokunun davranışı:**

`finally` bloku, `try` blokundakı kodun necə bitməsindən asılı olmayaraq (normal bitmə, `return`, `continue`, `break` ilə çıxma, `exception` ilə) həmişə icra olunur. O, adətən resursları bağlamaq və ya digər `cleanup` əməliyyatları üçün istifadə olunur.

* Normal halda, `try` bloku bitdikdən sonra `finally` bloku icra olunur.
* Əgər `try` bloku `return`, `continue`, `break` `statement`i ilə tərk edilərsə, `finally` bloku yeni təyinat yerinə tullanmadan əvvəl icra edilir.
* Əgər `try` blokunda `exception` yaranarsa və onu idarə edən bir `catch` bloku varsa, əvvəlcə `catch` bloku, sonra isə `finally` bloku icra olunur.
* Əgər yerli `catch` bloku yoxdursa, `interpreter` əvvəlcə `finally` blokunu icra edir, sonra ən yaxın `catch clause`a tullanır.

**Vacib qeyd:** Əgər `finally` blokunun özü bir `jump`a (`return`, `continue`, `break`, `throw` və ya `exception` atan bir metodun çağırılması ilə) səbəb olarsa, `interpreter` gözləyən hər hansı bir `jump`ı tərk edir və yeni `jump`ı yerinə yetirir. Məsələn, `finally clause` bir `exception` atarsa, bu `exception` atılmaqda olan hər hansı digər `exception`ı əvəz edir.

`try` və `finally` `catch clause` olmadan da birlikdə istifadə edilə bilər. Bu halda, `finally` bloku sadəcə `cleanup code`dur ki, `try` blokunda nə baş verməsindən asılı olmayaraq icra olunması təmin edilir.

---

### 5.6 Müxtəlif `Statements`lər

Bu bölmə JavaScript-də qalan üç `statement`i — `with`, `debugger` və `"use strict"`i izah edir.

---

### 5.6.1 `with`

`with` `statement`i, təyin edilmiş bir `object`in `properties`lərinin, həmin `code block`u üçün `scope`da `variable`lər kimi qəbul edildiyi şəkildə kodu icra edir.

**Sintaksis:**

```javascript
with (object)
  statement
```

Bu `statement`, `object`in `properties`lərini `variable` kimi istifadə edərək müvəqqəti bir `scope` yaradır və sonra həmin `scope` daxilində `statement`i icra edir.

**Vacib qeyd: `with` `statement`i istifadə etməyin!**

* `with` `strict mode`da (`5.6.3-cü bölməyə baxın`) qadağandır.
* `non-strict mode`da belə, `deprecated` (istifadəsi tövsiyə edilmir) hesab olunur. Mümkün olduqca istifadə etməkdən çəkinin.
* `with` istifadə edən JavaScript kodu optimallaşdırılması çətindir və `with` `statement`i olmadan yazılmış ekvivalent koddan xeyli yavaş işləyə bilər.

**Niyə istifadə olunurdu? (Və niyə indi yox?)**

`with` `statement`i əvvəllər daxili-daxilinə (`deeply nested`) `object hierarchy`ləri ilə işləməyi asanlaşdırmaq üçün istifadə olunurdu. Məsələn, `HTML form` elementlərinə daxil olmaq üçün:

```javascript
document.forms[0].address.value
```

Əgər bu cür ifadələri dəfələrlə yazmaq lazım gəlirdisə, `with` `statement`i `form object`inin `properties`lərini `variable` kimi işləməyə kömək edirdi:

```javascript
with(document.forms[0]) {
  // Burada form elementlərinə birbaşa daxil olun:
  name.value = "";
  address.value = "";
  email.value = "";
}
```

Bu, daha az yazmağa imkan verirdi. Lakin, `with` `statement`i olmadan eyni kodu daha yaxşı yazmağın asan bir yolu var:

```javascript
let f = document.forms[0];
f.name.value = "";
f.address.value = "";
f.email.value = "";
```

Bu sonuncu üsul, `with`dən daha aydın, daha sürətli və daha asan başa düşüləndir.

**Qeyd:** Əgər `with` `statement`inin `body`si daxilində `const`, `let` və ya `var` ilə bir `variable` və ya `constant` elan etsəniz, bu, adi bir `variable` yaradır və təyin edilmiş `object` daxilində yeni bir `property` təyin etmir.

---

### 5.6.2 `debugger` operatoru

`debugger` `statement`i normalda heç nə etmir. Lakin, əgər bir `debugger program` mövcuddursa və işləyirsə, bu `statement` bir `debugging action` yerinə yetirə bilər (məcburi deyil).

Praktikada, bu `statement` bir `breakpoint` kimi işləyir: JavaScript kodunun icrası dayanır və siz `debugger`dən `variable`lərin dəyərlərini çap etmək, `call stack`i araşdırmaq və s. üçün istifadə edə bilərsiniz.

**Nümunə:** Fərz edək ki, `f()` funksiyanız `undefined argument` ilə çağırıldığı üçün bir `exception` alırsınız və bu çağırışın haradan gəldiyini tapa bilmirsiniz. Bu problemi `debug` etmək üçün `f()` funksiyasını belə dəyişə bilərsiniz:

```javascript
function f(o) {
  if (o === undefined) debugger; // Debugging məqsədləri üçün müvəqqəti sətir
  // Funksiyanın qalan kodu burada yerləşir.
}
```

İndi, `f()` funksiyası `argument`siz çağırıldıqda, icra dayanacaq və siz `debugger`dən `call stack`i yoxlayaraq bu səhv çağırışın haradan gəldiyini tapa bilərsiniz.

**Qeyd:** Bir `debugger`in mövcud olması kifayət deyil: `debugger statement` sizin üçün `debugger`i başlatmayacaq. Lakin, bir `web browser` istifadə edirsinizsə və `developer tools console` açıqdırsa, bu `statement` bir `breakpoint` yaratacaq.

---

### 5.6.3 `"use strict"`

`"use strict"` `directive`i ES5-də təqdim edilmişdir. `Directives` `statement` deyil, lakin onlara çox yaxındır (`"use strict"` bu səbəbdən burada sənədləşdirilir). `"use strict"` `directive`i ilə adi `statement`lər arasında iki əsas fərq var:

* Heç bir dil `keyword`i ehtiva etmir: `directive` sadəcə xüsusi bir `string literal`dan (tək və ya cüt dırnaqlarda) ibarət olan bir `expression statement`dir.
* Yalnız bir `script`in əvvəlində və ya bir funksiyanın `body`sinin əvvəlində, hər hansı bir əsl `statement`dən əvvəl görünə bilər.

`"use strict"` `directive`inin məqsədi, ondan sonra gələn kodun (`script`də və ya funksiyada) `strict code` olduğunu bildirməkdir.

* Bir `script`in `top-level` (funksiya olmayan) kodu, əgər `script`də `"use strict"` `directive`i varsa, `strict code`dur.
* Bir funksiyanın `body`si, əgər `strict code` daxilində təyin edilibsə və ya özündə `"use strict"` `directive`i varsa, `strict code`dur.
* `eval()` metoduna ötürülən kod, əgər `eval()` `strict code`dan çağırılıbsa və ya kod `string`i `"use strict"` `directive`i ehtiva edirsə, `strict code`dur.
* Açıq şəkildə `strict` olaraq elan edilən koddan əlavə, bir `class body`sində (`Fəsil 9`) və ya bir ES6 `module`da (`§10.3`) olan hər hansı bir kod avtomatik olaraq `strict code`dur. Bu o deməkdir ki, əgər bütün JavaScript kodunuz `module`lar kimi yazılıbsa, o avtomatik olaraq `strict`dir və sizə heç vaxt açıq `"use strict"` `directive`i istifadə etməyə ehtiyac qalmayacaq.

`Strict code` **strict mode** da icra olunur. `Strict mode`, dilin bəzi əhəmiyyətli çatışmazlıqlarını aradan qaldıran, daha güclü xəta yoxlaması və artan təhlükəsizlik təmin edən dilin məhdud bir alt hissəsidir. `Strict mode` standart olmadığı üçün, dilin köhnə, çatışmaz xüsusiyyətlərini istifadə edən köhnə JavaScript kodu hələ də düzgün işləməyə davam edəcək.

**Strict mode və non-strict mode arasındakı əsas fərqlər (ilk üçü xüsusilə vacibdir):**

1.  `with statement` `strict mode`da icazəli deyil.
2.  `Strict mode`da bütün `variable`lər `declare` edilməlidir: əgər `declare` edilməmiş bir `variable`, `function`, `function parameter`, `catch clause parameter` və ya `global object`in `property`sinə bir dəyər təyin etsəniz, bir `ReferenceError` atılır. (`Non-strict mode`da bu, `global variable`i `implicitly declare` edir.)
3.  `Strict mode`da, funksiyalar (metodlar kimi deyil, funksiya kimi) çağırıldıqda `this` dəyəri `undefined` olur. (`Non-strict mode`da, funksiyalar funksiya kimi çağırıldıqda `global object` həmişə onların `this` dəyəri kimi ötürülür.) Həmçinin, `strict mode`da bir funksiya `call()` və ya `apply()` ilə çağırıldıqda (`§8.7.4`), `this` dəyəri `call()` və ya `apply()`yə ilk `argument` kimi ötürülən dəyərin özü olur. (`Non-strict mode`da `null` və `undefined` dəyərləri `global object` ilə əvəz olunur və `non-object` dəyərlər `object`lərə çevrilir.)
4.  `Strict mode`da, `non-writable property`lərə təyinatlar və `non-extensible object`lər üzərində yeni `property` yaratma cəhdləri bir `TypeError` atır. (`Non-strict mode`da bu cəhdlər səssizcə uğursuz olur.)
5.  `Strict mode`da, `eval()` metoduna ötürülən kod `caller`in `scope`unda `variable`lər `declare` edə və ya funksiyalar təyin edə bilməz, bu, `non-strict mode`da mümkündür. Bunun əvəzinə, `variable` və `function definition`ları `eval()` üçün yaradılan yeni bir `scope`da yaşayır. Bu `scope` `eval()` geri döndükdə ləğv edilir.
6.  `Strict mode`da, bir funksiyadakı `Arguments object` (`§8.3.3`) funksiyaya ötürülən dəyərlərin `static copy`sini saxlayır. `Non-strict mode`da, `Arguments object` `array` elementləri və adlandırılmış `function parameter`lərin hər ikisinin eyni dəyərə istinad etdiyi "sehrli" bir davranışa malikdir.
7.  `Strict mode`da, əgər `delete operator`dan sonra bir `unqualified identifier` (məsələn, bir `variable`, `function` və ya `function parameter`) gələrsə, bir `SyntaxError` atılır. (`Non-strict mode`da belə bir `delete expression` heç nə etmir və `false` dəyərləndirilir.)
8.  `Strict mode`da, bir `non-configurable property`ni silməyə cəhd edildikdə bir `TypeError` atılır. (`Non-strict mode`da cəhd uğursuz olur və `delete expression` `false` dəyərləndirilir.)
9.  `Strict mode`da, bir `object literal`in eyni adla iki və ya daha çox `property` təyin etməsi bir `syntax error`dur. (`Non-strict mode`da heç bir xəta baş vermir.)
10. `Strict mode`da, bir `function declaration`ın eyni adla iki və ya daha çox `parameter`ə sahib olması bir `syntax error`dur. (`Non-strict mode`da heç bir xəta baş vermir.)
11. `Strict mode`da, `octal integer literals` (sıfırla başlayan, lakin ardınca `x` gəlməyən) icazəli deyil. (`Non-strict mode`da bəzi `implementation`lar `octal literals`a icazə verir.)
12. `Strict mode`da, `eval` və `arguments` `identifier`ləri `keyword`lər kimi qəbul edilir və dəyərlərini dəyişməyə icazə verilmir. Bu `identifier`lərə dəyər təyin edə, onları `variable` kimi `declare` edə, `function name` kimi istifadə edə, `function parameter name` kimi istifadə edə və ya bir `catch block`un `identifier`i kimi istifadə edə bilməzsiniz.
13. `Strict mode`da, `call stack`i araşdırma qabiliyyəti məhduddur. `arguments.caller` və `arguments.callee` hər ikisi `strict mode` funksiyası daxilində bir `TypeError` atır. `Strict mode` funksiyaları həmçinin oxunduqda `TypeError` atan `caller` və `arguments` `properties`lərinə malikdir.

---

### 5.7 `Declarations`

`const`, `let`, `var`, `function`, `class`, `import`, və `export` `keyword`ləri texniki olaraq `statement` deyil. Onlar `declaration` adlanır.

`Statements` "nəsə baş verməsinə" səbəb olur. `Declarations` isə yeni dəyərləri təyin edir və onlara adlar verir ki, biz həmin dəyərlərə istinad edə bilək. Onlar proqramın işə düşmədən əvvəl emal olunan hissələri kimidir, proqramın strukturunu müəyyən edirlər.

JavaScript `declaration`ları `constant`ları, `variable`ləri, `function`ları və `class`ları təyin etmək, həmçinin `module`lar arasında dəyərləri `import` və `export` etmək üçün istifadə olunur. Növbəti hissələrdə bu `declaration`lara nümunələr veriləcək. Onların hər biri kitabın başqa yerlərində daha ətraflı izah olunacaq.

---

### 5.7.1 `const`, `let`, və `var`

`const`, `let`, və `var` `declaration`ları `§3.10`-da daha ətraflı izah olunur.

* ES6 və sonrakı versiyalarda:
    * `const` **`constant`lar**ı (`dəyişməyən dəyərlər`) `declare` edir.
    * `let` **`variable`ləri** (`dəyişə bilən dəyərlər`) `declare` edir.
* ES6-dan əvvəl:
    * `var` `variable`ləri `declare` etmək üçün yeganə yol idi və `constant`ları `declare` etmək üçün bir yol yox idi.

**Qeyd:** `var` ilə `declare` edilən `variable`lər `containing block` əvəzinə `containing function`a `scope`ludur. Bu, `bug`lara səbəb ola bilər və müasir JavaScript-də `let` əvəzinə `var` istifadə etmək üçün heç bir səbəb yoxdur.

**Nümunə:**

```javascript
const TAU = 2 * Math.PI; // TAU dəyəri dəyişməz qalacaq
let radius = 3;          // radius dəyəri dəyişə bilər
var circumference = TAU * radius; // Əvvəlki versiyalarda istifadə olunurdu, lakin indi let tövsiyə olunur
```
---
### 5.7.2 `function`

`function declaration` `function`ları təyin etmək üçün istifadə olunur. `Function`lar `Fəsil 8`-də ətraflı izah edilir. (`§4.3`-də də `function expression` hissəsi kimi `function` görmüşük.)

Bir `function declaration` belə görünür:

```javascript
function area(radius) {
  return Math.PI * radius * radius;
}
```

Bir `function declaration` bir `function object` yaradır və onu göstərilən ada (`area` bu nümunədə) təyin edir. Proqramımızın başqa yerində, bu addan istifadə edərək `function`a istinad edə və içindəki kodu işlədə bilərik.

JavaScript kodunun hər hansı bir `block`undakı `function declaration`ları həmin kod işə düşməzdən əvvəl emal olunur və `function name`ləri bütün `block` boyu `function object`lərinə bağlanır. Biz deyirik ki, `function declaration`ları **"hoisted"** edilir, çünki sanki onlar təyin olunduqları `scope`un ən yuxarı hissəsinə köçürülmüş kimi davranırlar. Nəticədə, bir `function`u çağıran kod, `function`u `declare` edən koddandan əvvəl də proqramınızda mövcud ola bilər.

**Əlavə funksiya növləri:**

* **Generator funksiyaları:** `§12.3`-də təsvir edilən xüsusi bir `function` növüdür. `Generator declaration`ları `function keyword`ini istifadə edir, lakin ondan sonra bir ulduz (*) işarəsi gəlir.
* **Asynchronous funksiyalar:** `§13.3`-də təsvir edilən `function`lardır. Onlar da `function keyword`ini istifadə edərək `declare` edilir, lakin əvvəlinə `async` `keyword`i əlavə olunur.

---


### 5.7.3 `class`

ES6 və sonrakı versiyalarda, `class declaration` yeni bir `class` yaradır və ona istinad edə biləcəyimiz bir ad verir. `Class`lar `Fəsil 9`-da ətraflı təsvir edilmişdir.

Sadə bir `class declaration` belə görünə bilər:

```javascript
class Circle {
  constructor(radius) {
    this.r = radius;
  }
  area() {
    return Math.PI * this.r * this.r;
  }
  circumference() {
    return 2 * Math.PI * this.r;
  }
}
```

**Vacib qeyd:** Funksiyalardan fərqli olaraq, `class declaration`ları `hoisted` deyil. Yəni, bir `class`ı `declare` etdiyiniz koddandan əvvəl onu istifadə edə bilməzsiniz.

---

### 5.7.4 `import` və `export`

`import` və `export` `declaration`ları birlikdə istifadə olunur ki, JavaScript kodunun bir `module`unda təyin olunmuş dəyərlər başqa bir `module`da istifadə edilə bilsin.

* Bir `module` öz `global namespace`inə malik, digər `module`lardan tamamilə asılı olmayan bir JavaScript kodu `file`ıdır.
* Bir `module`da təyin edilmiş bir dəyərin (`function` və ya `class` kimi) başqa bir `module`da istifadə edilməsinin yeganə yolu, əgər təyin edən `module` onu `export` ilə `export` edirsə və istifadə edən `module` onu `import` ilə `import` edirsə mümkündür.

`Module`lar `Fəsil 10`-un mövzusudur və `import` və `export` `§10.3`-də ətraflı izah olunur.

**`import` `directive`ləri:**

`import` `directive`ləri JavaScript kodunun başqa bir `file`ından bir və ya daha çox dəyəri `import` etmək və cari `module` daxilində onlara adlar vermək üçün istifadə olunur. `import` `directive`lərinin bir neçə fərqli forması var.

**Bəzi nümunələr:**

```javascript
import Circle from './geometry/circle.js'; // `Circle`ı tam olaraq import edir
import { PI, TAU } from './geometry/constants.js'; // `PI` və `TAU`u spesifik olaraq import edir
import { magnitude as hypotenuse } from './vectors/utils.js'; // `magnitude`u `hypotenuse` adı ilə import edir
```

**`export` `directive`ləri:**

Bir JavaScript `module`u daxilindəki dəyərlər `private`dir və açıq şəkildə `export` edilmədikcə digər `module`lara `import` edilə bilməz. `export` `directive`i bunu edir: o, cari `module`da təyin edilmiş bir və ya daha çox dəyərin `exported` olduğunu və buna görə də digər `module`lar tərəfindən `import` üçün mövcud olduğunu `declare` edir.

**Nümunə:**

```javascript
// geometry/constants.js faylında
const PI = Math.PI;
const TAU = 2 * PI;
export { PI, TAU }; // PI və TAU'nu digər modullar üçün export edir
```

`export` `keyword`i bəzən digər `declaration`ların üzərində `modifier` kimi istifadə olunur və bu, bir `constant`, `variable`, `function` və ya `class`ı təyin edən və eyni zamanda `export` edən bir növ `compound declaration` ilə nəticələnir.

Əgər bir `module` yalnız bir dəyəri `export` edirsə, bu adətən xüsusi `export default` forması ilə edilir:

```javascript
export const TAU = 2 * Math.PI; // TAU konstantını təyin edir və export edir
export function magnitude(x,y) { return Math.sqrt(x*x + y*y); } // funksiyanı təyin edir və export edir
export default class Circle { /* class definition omitted here */ } // Circle class'ını default olaraq export edir
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