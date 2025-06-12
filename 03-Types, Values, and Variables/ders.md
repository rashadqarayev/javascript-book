# Chapter 3: Tipler, Dəyərlər və Dəyişənlər

Komputer proqramları rəqəmlər (`3.1`) və ya mətnlər (`"Hello, world!"`) kimi dəyərləri idarə edən dəyişənlər üzərində işləyir. Proqramlaşdırma dilində təmsil oluna və dəyişdirilə bilən dəyərlər *tip* adlanır və proqramlaşdırma dilinin əsas xüsusiyyətlərindən biri onun dəstəklədiyi tiplər toplusudur.

Əgər proqram bu dəyəri gələcəkdə istifadə edəcəksə, bu dəyəri *dəyişənə* mənimsədir. Dəyişənlərin adları olur və biz bu adlardan istifadə edərək həmin dəyərlərə istinad edirik.

---

## 3.1 İcmal və Təriflər

JavaScript-də tiplər 2 əsas kateqoriyaya bölünür:

- **Primitiv tiplər**
- **Obyekt tipləri**

### Primitiv Tiplər

JavaScript-də primitiv tiplərə aşağıdakılar daxildir:

- `number` – rəqəmlər
- `string` – mətnlər
- `boolean` – `true` və ya `false`
- `null`
- `undefined`
- `symbol` (ES6 ilə əlavə olunub)

**Misal:**

```js
let age = 25;           // number
let name = "Ali";       // string
let isAdmin = true;     // boolean
let empty = null;       // null
let notDefined;         // undefined
let uniqueId = Symbol(); // symbol
```

### Obyekt Tipləri

Əgər dəyər `number`, `string`, `boolean`, `null`, `undefined` və ya `symbol` deyilsə, o zaman bu dəyər *obyektdir*.

Obyekt – ad(`key`) və dəyər(`value`) cütlüklərindən ibarət olan bir kolleksiyadır.

```js
let person = {
  name: "Ali",
  age: 30
};
```

## ✅ Xüsusi Obyekt Tipləri

JavaScript-də adi `object` tipindən başqa, məlumatları fərqli yollarla saxlamaq və işləmək üçün istifadə olunan **xüsusi obyekt tipləri** də mövcuddur. Onlar aşağıdakılardır:

### 1. 🔢 **Array** – ardıcıl və nömrələnmiş dəyərlərin toplusu

Array – birdən çox dəyəri bir yerdə saxlamaq üçün istifadə olunur. Bu dəyərlərin **sırası və nömrəsi (index)** var.

```js
let numbers = [1, 2, 3]; // index-lər: 0 → 1, 1 → 2, 2 → 3
console.log(numbers[0]); // 1
```

---

### 2. 🔁 **Set** – təkrarsız dəyərlərin toplusu

Set – içində **eyni dəyər bir neçə dəfə ola bilməz**. Yəni, təkrar dəyərlər avtomatik çıxarılır.

```js
let unique = new Set([1, 2, 2, 3]);
console.log(unique); // Set(3) {1, 2, 3}
```

➡ Faydalıdır: Məsələn, bir massivin içində neçə fərqli dəyər olduğunu tapmaq üçün.

---

### 3. 🗺️ **Map** – açar-dəyər (key-value) cütlükləri

Map – hər açarın qarşısında bir dəyər saxlanılır. Obyektdən fərqli olaraq:
- Açar hər şey ola bilər (`string`, `number`, `object`, və s.)
- Əlavə olunma sırası qorunur

```js
let capitals = new Map();
capitals.set("Azerbaijan", "Baku");
capitals.set("Turkey", "Ankara");

// dəyəri əldə etmək:
console.log(capitals.get("Turkey")); // "Ankara"
```

#### Niyə Map istifadə edirik?
- `Object`-dən daha çevikdir
- Açarın tipi daha müxtəlif ola bilər
- `.size` ilə uzunluq asan tapılır

```js
console.log(capitals.size); // 2
```

---

### 4. 👨‍🔬 **Typed Arrays** – xüsusi tipli massivlər

Bunlar `Int8Array`, `Float32Array` kimi xüsusi massivlərdir və **yalnız müəyyən tipdə ədədlər** saxlayır. Daha çox **performans** tələb edən hesablama və ya WebGL kimi sahələrdə istifadə olunur.

```js
let intArray = new Int16Array([10, 20, 30]);
```

---

### 5. 🔍 **RegExp (Regular Expression)** – mətnlə işləmək üçün qaydalar

Mətnlərdə müəyyən nümunələr tapmaq və ya dəyişmək üçün istifadə olunur.

```js
let pattern = /hello/i;
console.log(pattern.test("Hello World")); // true
```

---

### 6. 📅 **Date** – tarix və saat obyektləri

Tarix və saatla işləmək üçün `Date` obyekti istifadə olunur.

```js
let now = new Date();
console.log(now.toDateString()); // məsələn: "Tue Apr 23 2025"
```

---

### 7. ❗ **Error və alt tipləri** – səhvlərlə işləmək üçün

JavaScript-də `Error`, `TypeError`, `SyntaxError` və s. kimi səhv tipləri var. Proqramın gedişində bir problem olduqda istifadə olunur.

```js
try {
  throw new Error("Nə isə səhv getdi");
} catch (err) {
  console.log(err.message); // "Nə isə səhv getdi"
}
```

---

## 📌 Yekun

| Tip             | Nə üçündür?                                     |
|------------------|-------------------------------------------------|
| `Array`          | Dəyərləri sıra ilə saxlamaq                     |
| `Set`            | Təkrarsız dəyərlər saxlamaq                     |
| `Map`            | Açar-dəyər cütləri saxlamaq                     |
| `TypedArray`     | Performanslı nömrə massivləri                   |
| `RegExp`         | Mətnlərdə nümunə tapmaq                         |
| `Date`           | Tarix və saatla işləmək                         |
| `Error`          | Səhvləri idarə etmək və bildirmək              |

---

## Funksiya və Klasslar

JavaScript-də funksiyalar və klasslar da dəyər sayılır və özləri də obyektlərin xüsusi növüdür.

```js
function greet() {
  console.log("Salam!");
}
```

```js
class Car {
  constructor(brand) {
    this.brand = brand;
  }
}
```
---

## 🧠 Yaddaş İdarəetməsi və Garbage Collection

JavaScript-də yaddaş **avtomatik şəkildə** idarə olunur. Sən bir dəyişən yaradanda, yaddaşda yer ayrılır. Əgər o dəyişənə artıq istinad (link) yoxdursa, **garbage collector** həmin obyektin artıq lazım olmadığını başa düşür və onu yaddaşdan silir.

### ✅ Sadə Nümunə:

```js
function createUser() {
  let user = {
    name: "Rauf",
    age: 25,
  };
  return user;
}

let user1 = createUser(); // user obyektinə istinad var
```

🔹 Burada `user` adlı obyekt yaranır və `user1` dəyişəni ona istinad edir. Bu obyekt **hələki istifadə olunur**, ona görə yaddaşda qalır.

---

### 🗑️ Garbage Collection nə vaxt baş verir?

Əgər obyektə **artıq heç bir istinad yoxdursa**, JavaScript onun "lazımsız" olduğunu düşünür və avtomatik silir.

```js
let user2 = {
  name: "Aysel",
};

user2 = null; // artıq obyektə istinad yoxdur
```

🔹 Burada `user2` əvvəlcə bir obyektə işarə edir, sonra `null` veririk. Artıq heç kim o obyektə işarə etmir. JavaScript-in garbage collector mexanizmi bir müddət sonra bu obyekti **yaddaşdan çıxarır**.

---

## 🎯 Əsas Məqam: JavaScript-də metodlar **obyektlərə bağlıdır**

JavaScript-də bir çox funksiyalar **obyektin içindəki metod** kimi tanınır. `sort` da onlardan biridir və **yalnız Array obyektinə aid olan bir metod**dur.

---

## ❌ `sort(a)` – Niyə Səhvdir?

```js
sort(a);
```

Bu formada yazanda sən deyirsən ki, "adı `sort` olan bir **global funksiyanı** çağır, və `a` array-ni ona ötür."

Amma problem budur ki:  
➡️ JavaScript-də `sort` adlı **global (ümumi) funksiya yoxdur!**  
➡️ `sort` sadəcə `Array.prototype`-ə məxsus bir metoddur, yəni yalnız array-lərdə mövcuddur.

---

## ✅ `a.sort()` – Niyə Doğrudur?

```js
a.sort();
```

Bu isə deməkdir ki:  
➡️ `a` adlı array obyektinin `sort()` adlı metodunu çağır.

Bu doğrudur, çünki `sort()` metodu **Array** tipinə aiddir:

```js
let a = [3, 1, 2];
a.sort(); // [1, 2, 3]
```

Burada `a` array-dir və `.sort()` metodu `Array.prototype.sort` funksiyası ilə işləyir.

---

## 🔬 Bunu necə sübut edə bilərik?

```js
console.log(typeof sort); // undefined (səhv)
console.log(typeof [].sort); // 'function' (doğru)
```

JavaScript-də `sort()`, `map()`, `filter()`, `push()` kimi bir çox funksiyalar **obyektə bağlı metodlardır**. Onlara **obyektin adından sonra nöqtə qoyaraq** müraciət etməliyik.

Yalnız obyektlər deyil, `string`, `number`, `boolean`, `symbol` kimi primitiv tiplərin də öz metodları olur:

```js
let text = "salam";
console.log(text.toUpperCase()); // "SALAM"
```

Yeganə istisna: `null` və `undefined`. Bu tiplər metodlara malik deyil və onlarla birbaşa metod istifadə etmək mümkün deyil.

---

## Mutable və Immutable Tiplər

- **Primitiv tiplər**: immutable (dəyişməz)
- **Obyekt tipləri**: mutable (dəyişə bilən)

```js
let num = 5;
num = 10; // bu yeni dəyərdir, əvvəlki dəyişmir, əvəz olunur

let arr = [1, 2, 3];
arr[0] = 99; // array mutable olduğu üçün dəyişə bilər
```

**Qeyd:** `string` tipi xarici görünüşcə array kimi olsa da, immutable-dır:

```js
let word = "salam";
word[0] = "S"; // bu dəyişiklik işləmir
```

---

## Tip Dəyişməsi (Type Coercion)

JavaScript dəyərləri avtomatik olaraq bir tipdən digərinə çevirə bilər.

```js
let x = "5" + 1; // "51" (number → string)
let y = "5" - 1; // 4   (string → number)
```

---

## Dəyişənlər və Sabitlər

Dəyərlərə istinad etmək üçün dəyişənlər və sabitlərdən istifadə edirik.

- `let` – dəyişən yaratmaq üçün
- `const` – sabit yaratmaq üçün
- `var` – köhnə sintaksis, istifadəsi tövsiyə olunmur

```js
let a = 10;
const b = 20;
```

JavaScript-də dəyişənlər tipsizdir – hansı tipdə dəyər alacağını əvvəlcədən müəyyən etmir:

```js
let value = 5;
value = "now it's a string";
```

---

## 3.2 JavaScript Rəqəmləri

JavaScript-də rəqəmlər `Number` tipi ilə təmsil olunur. Bu tip həm **tam ədədləri** (məsələn, 3, -7), həm də **ondalıklı ədədləri** (məsələn, 3.14, -5.6) təmsil edir.

### Rəqəmlərin Dəqiqliyi

JavaScript, **IEEE 754** standartına əsaslanan bir formatdan istifadə edir ki, bu da rəqəmlərin yaddaşda necə saxlanacağını müəyyən edir. Bu format 64 bitlik bir **floating-point** formatıdır, yəni həm çox **böyük**, həm də çox **kiçik** ədədləri təmsil edə bilir. Məsələn, JavaScript çox böyük ədədləri belə təmsil edə bilir:

- **Böyük ədədlər:** ±1.7976931348623157 × 10^308
- **Kiçik ədədlər:** ±5 × 10^−324

### Dəqiqlıq Aralığı

JavaScript-də rəqəmlər yalnız müəyyən bir aralıqda dəqiq təmsil oluna bilir. Bu aralıq:

- **Ən kiçik dəqiq rəqəm**: -2^53 (−9,007,199,254,740,992)
- **Ən böyük dəqiq rəqəm**: 2^53 (9,007,199,254,740,992)

Bu aralıqdan daha böyük rəqəmlər istifadə edildikdə, təmsil edilən rəqəmin sonuncu hissələri səhv ola bilər. Məsələn, çox böyük ədədlər istifadə edildikdə, yalnız ilk rəqəmlər dəqiq olur və sonrakılar səhv ola bilər.

### Rəqəm Literalları

Rəqəm JavaScript proqramında göründüyü zaman buna **numeric literal** (rəqəmli literal) deyilir. JavaScript bir çox növ numeric literal dəstəkləyir. Əlavə olaraq, istənilən numeric literalı mənfi etmək üçün onun qarşısına **-** işarəsi əlavə edə bilərik.

### 3.2.1 Tam Ədədlər (Integer Literals)

JavaScript-də **tam ədədlər** decimal (base-10) sistemində yazılır. Məsələn:

```js
let num1 = 0;          // 0
let num2 = 3;          // 3
let num3 = 10000000;   // 10000000
```

Bundan əlavə, JavaScript **heksadesimal** (base-16) ədədləri də dəstəkləyir. Heksadesimal ədəd `0x` ilə başlayır və 0-9 və ya a-f (və ya A-F) arasında olan simvollarla ifadə edilir. Məsələn:

```js
let hex1 = 0xff;       // 255: (15*16 + 15)
let hex2 = 0xBADCAFE;  // 195939070
```

ES6 ilə birlikdə, biz **binary** (ikilik - base 2) və **oktal** (base 8) ədədlərini də yazma imkanı əldə etdik. Bunlar sırasıyla `0b` və `0o` ilə başlayır. Məsələn:

```js
let bin = 0b10101;     // 21: (1*16 + 0*8 + 1*4 + 0*2 + 1*1)
let oct = 0o377;       // 255: (3*64 + 7*8 + 7*1)
```

### 3.2.2 Ondalıklı Ədədlər (Floating-Point Literals)

Ondalıklı ədədlər (floating-point literals) real ədədlərdir və onlar iki hissədən ibarətdir: bir **tam hissə** və bir **kesir hissəsi**. Bu ədədlər, eksponensial formatda da yazıla bilər. Məsələn:

```js
let float1 = 3.14;
let float2 = 2345.6789;
let float3 = .333333333333333333;
let float4 = 6.02e23;      // 6.02 × 10^23
let float5 = 1.4738223E-32; // 1.4738223 × 10^−32
```

Daha oxunaqlı olması üçün, biz `underscore (_)` işarəsini də istifadə edə bilərik. Bu işarə rəqəmləri qruplaşdırmaq və onları daha aydın göstərmək üçün istifadə olunur:

```js
let billion = 1_000_000_000;
let bytes = 0x89_AB_CD_EF;
let bits = 0b0001_1101_0111;
let fraction = 0.123_456_789;
```

---

## 3.2.3 Arifmetika JavaScript-də

JavaScript proqramlarında rəqəmlərlə işləyərkən, müxtəlif arifmetik operatorlardan istifadə edilir. Bu operatorlara aşağıdakılar daxildir:

- `+` — toplamaq
- `-` — çıxmaq
- `*` — vurmaq
- `/` — bölmək
- `%` — qalıq tapmaq

Bu barədə daha ətraflı məlumat **Chapter 4**-də veriləcək.

Bununla yanaşı, JavaScript daha kompleks riyazi əməliyyatları yerinə yetirmək üçün `Math` obyektinin konstantları və funksiyalarından istifadə edir.

### Math Obyekti

JavaScript-də `Math` obyektinin bir çox faydalı funksiyası mövcuddur:

```javascript
Math.pow(2, 53)    // 2^53
Math.round(0.6)    // 1: ən yaxın tam ədədə yuvarlaqlaşdırma
Math.ceil(0.6)     // 1: yuxarıya yuvarlaqlaşdırma
Math.floor(0.6)    // 0: aşağıya yuvarlaqlaşdırma
Math.abs(-5)       // 5: müsbət dəyərini almaq
Math.max(x, y, z)  // x, y, z arasında ən böyük ədəd
Math.min(x, y, z)  // x, y, z arasında ən kiçik ədəd
Math.random()      // 0 ilə 1 arasında təsadüfi ədəd
Math.PI            // π: dairənin çevrəsi
Math.E             // e: təbii logaritmanın əsas ədədi
Math.sqrt(3)       // 3-ün kvadrat kökü
Math.pow(3, 1/3)   // 3-ün kub kökü
Math.sin(0)        // sin(0)
Math.atan(x)       // x-in tərs sinusunu tapmaq
Math.log(10)       // 10-un təbii logaritması
```

### ES6-da əlavə funksiyalar

ES6 ilə `Math` obyektinə daha çox yeni funksiyalar əlavə olunub:

```javascript
Math.cbrt(27)       // 3: 27-nin kub kökü
Math.hypot(3, 4)    // 5: kvadratların cəminin kvadrat kökü
Math.log10(100)     // 2: 100-un onluqlu logaritması
Math.log2(1024)     // 10: 1024-ün iki cəmlü logaritması
Math.log1p(x)       // 1 + x-in təbii logaritması
Math.expm1(x)       // e^x - 1
Math.sign(x)        // x-in işarəsi
Math.imul(2, 3)     // 2 və 3-ün tam sayı ilə hasilini tapır
Math.clz32(0xf)     // 28: 32-bitlik tam ədəddə başdakı sıfırların sayı
Math.trunc(3.9)     // 3: tam hissəsini alır
Math.fround(x)      // 32-bitlik float olaraq yuvarlaqlaşdırır
Math.sinh(x)        // hiperbollu sinus
Math.tanh(x)        // hiperbollu tərs sinus
Math.asinh(x)       // hiperbollu sinus funksiyasının tərsi
Math.atanh(x)       // hiperbollu tərs tangens
```

### 0 ilə Bölmə

JavaScript-də sıfıra bölmə zamanı və ya rəqəmlərlə qarşılaşdıqda xüsusi bir problem yaranmır. Bu hallarda nəticə `Infinity` və ya `-Infinity` kimi xüsusi dəyərlər olur.

```javascript
1 / 0                // Infinity
Number.MAX_VALUE * 2 // Infinity
-1 / 0               // -Infinity
```

Amma, maraqlı bir vəziyyət var ki, `0 / 0` ifadəsi heç bir xüsusi dəyər qaytarmır və nəticə `NaN` (Not a Number) olur.

```javascript
0 / 0     // NaN
Infinity / Infinity // NaN
```

### Infinity və NaN

JavaScript-də `Infinity` və `NaN` (Not a Number) `Number` obyektinin xüsusi xüsusiyyətləri kimi qəbul edilir.

#### Infinity

`Infinity` müsbət sonsuzluğu təmsil edir, `-Infinity` isə mənfi sonsuzluğu təmsil edir.

```javascript
Number.POSITIVE_INFINITY // Infinity
1 / 0                     // Infinity
Number.MAX_VALUE * 2     // Infinity
```

#### NaN

`NaN` qeyri-rəqəm dəyərini təmsil edir və əgər bir əməliyyat qeyri-rəqəmli və ya rəqəmə çevrilə bilməyən dəyərlə edilsə, nəticə `NaN` olacaq.

```javascript
Number.NaN  // NaN
0 / 0       // NaN
```

### NaN ilə yoxlama

JavaScript-də bir dəyərin `NaN` olduğunu yoxlamaq adi bir şəkildə, məsələn, `x === NaN` ilə mümkün deyil. Bunun əvəzinə aşağıdakı yollarla yoxlama aparmaq lazımdır:

```javascript
x !== x            // true: NaN özünü bərabər deyil
Number.isNaN(x)    // true: NaN olub olmadığını yoxlayır
```

### Number Obyektinin Funksiyaları

JavaScript-də `Number` obyektinin bəzi funksiyaları var:

```javascript
Number.parseInt(x)    // Tam ədəd olaraq çevrilən dəyəri verir
Number.parseFloat(x)  // Ondalıklı ədəd olaraq çevrilən dəyəri verir
Number.isNaN(x)       // NaN olub olmadığını yoxlayır
Number.isFinite(x)    // `Infinity` və `-Infinity` xaricindəki ədədlər üçün true qaytarır
Number.isInteger(x)   // Tam ədəd olub olmadığını yoxlayır
Number.isSafeInteger(x) // x-in təhlükəsiz tam ədəd olub olmadığını yoxlayır
Number.MIN_SAFE_INTEGER // => -(2**53 - 1) 
Number.MAX_SAFE_INTEGER // => 2**53 - 1
Number.EPSILON // => 2**-52: rəqəmlər arasındakı ən kiçik fərq
```

### Mənfi 0

JavaScript-də `-0` və `+0` bərabərdir, lakin onları bölərkən fərqli nəticələr alırıq.

```javascript
let zero = 0;
let negz = -0;

zero === negz           // true: 0 və -0 bərabərdir
1 / zero === 1 / negz   // false: `Infinity` və `-Infinity` fərqli nəticələrdir
```

Əla gedir, bro! Bu hissəni də tam markdown formatında aşağıda sənə təqdim edirəm:

---

### 3.2.4 Yuvarlaqlaşdırma xətaları

```js
let x = .3 - .2;
let y = .2 - .1;
x === y; // false!
x === .1; // false: .3 - .2 !== 0.1
y === .1; // true: .2 - .1 === 0.1
```

`.3 - .2` və `.2 - .1` nəticələri eyni olmalı kimi görünsə də, `x === y` ifadəsi `false` qaytarır. Bunun səbəbi kompüterlərin ədədləri ikili sistemdə təxmini yadda saxlamasıdır.

Bu problem yalnız JavaScript-ə xas deyil – C, Java, Python kimi digər proqramlaşdırma dillərində də oxşar yuvarlaqlaşdırma xətaları baş verir.

---

### 3.2.5 BigInt ilə ixtiyari dəqiq tam ədədlər

> ⚠️ **Bu bölmə gələcəkdə izah olunacaq.**

BigInt JavaScript-də çox böyük tam ədədlərlə işləməyə imkan verir. Standart `Number` tipindən fərqli olaraq, `BigInt` tipində olan ədədlər çox böyük ölçüləri də dəqiq yadda saxlaya bilir.

---

### 3.2.6 Tarix və zaman

JavaScript tarix və zamanla işləmək üçün `Date` obyektindən istifadə edir. Bu obyekt tarixi və zamanı idarə etmək üçün çoxsaylı metodlara malikdir.

```js
let timestamp = Date.now();         // Hazırki zaman millisaniyə formatında (timestamp).
let now = new Date();               // Hazırki zamanı Date obyekti kimi alırıq.
let ms = now.getTime();             // Date obyektini millisaniyə timestamp-ına çevirir.
let iso = now.toISOString();        // ISO formatında string olaraq tarixi verir.
```

🕒 Timestamp – bu, **1970-01-01T00:00:00Z** tarixindən indiyə qədər keçən **millisaniyələrin** sayıdır.

Super, sən demişkən, 3.3 bölməsini də tam, gözəl formatda yazdım. ChatGPT olaraq mənim də əlavəm 3.3.2-dədir 😎

---

## 3.3 Mətn

JavaScript-də mətnləri təmsil edən tiplər **string** adlanır. String — dəyişməyən, sıralanmış **16-bit** dəyərlərin ardıcıllığıdır və hər biri bir **Unicode** simvolunu təmsil edir.

String-in uzunluğu onun tərkibindəki 16-bit-lik dəyərlərin sayıdır. JavaScript string-ləri sıfırdan başlayan indekslə işləyir. Boş string-in (`""`) uzunluğu 0 olur.

JavaScript-də string-i təmsil edən ayrıca bir “char” (tək simvol) tipi yoxdur. Əgər yalnız bir simvol saxlamaq istəyiriksə, uzunluğu 1 olan string istifadə olunur.

JavaScript **UTF-16 Unicode** şrift cədvəlindən istifadə edir. Bu səbəblə bəzi simvollar bir dənə 16-bit-lik dəyərlə, digərləri isə iki ədəd 16-bit-lik kodla ifadə oluna bilər.

```js
let euro = "€";
let love = "❤";

euro.length      // => 1: bu simvol yalnız bir 16-bit elementlə təmsil olunur
love.length      // => 2: UTF-16 ilə ❤ belə kodlanır: "\ud83d\udc99"
```

---

### 3.3.1 String literal-ları

JavaScript-də string yaratmaq üçün mətn sadəcə **tək (‘ ’)**, **iki (“ ”)** və ya **backtick (` `)** içində yazılır.

```js
""                           // Boş string: uzunluğu 0
"testing"                   // Sadə mətn
"3.14"                      // Ədədi string şəklində
'name="myform"'            // HTML atributu
"Wouldn't you prefer O'Reilly's book?" // Tək dırnaq ikiqat dırnaq içində problemsizdir
"τ is the ratio of a circle's circumference to its radius"

`"She said 'hi'", he said.`  // Backtick stringləri

// İki sətri bir sətrdə ifadə etmək:
'two\nlines'

// Tək sətir, kodda üç sətirdə yazılmış:
"one\
 long\
 line"

// İki sətrli string, literal kimi newline daxil edilir:
`the newline character at the end of this line
is included literally in this string`

// HTML-də tək və ikiqat dırnaqlar birlikdə:
<button onclick="alert('Thank you')">Click Me</button>
```

---

### 3.3.2 String literal-larında qaçış ardıcıllığı

String literal-larında xüsusi simvolları (məsələn, dırnaq, newline və s.) təmsil etmək üçün **escape sequence** (qaçış ardıcıllığı) istifadə olunur. Bu `\` simvolu ilə başlayır.

```js
'You\'re right, it can\'t be a quote' // \ ilə tək dırnaq qaçırılır
"You're right, it can't be a quote"   // Burada isə ehtiyac yoxdur
```

Ən çox istifadə olunan qaçış ardıcıllıqları:

| Qaçış simvolu | Mənası                |
|---------------|------------------------|
| `\'`          | Tək dırnaq             |
| `\"`          | İkiqat dırnaq          |
| `\\`          | Tərs slash (`\`)       |
| `\n`          | Yeni sətr              |
| `\t`          | Tab boşluğu            |
| `\uXXXX`      | Unicode simvolu        |

---
Əla, gəlin bu notları daha səliqəli və aydın şəkildə təqdim edək, həm də davam etdirək. Burada sən JavaScript-də stringlərlə işləməyin əsaslarını öyrənirsən və çox vacib bir hissəni əhatə etmisən. İndi isə bu notları daha strukturlu şəkildə təqdim edirəm və davamını əlavə edirəm:

---

## **3.3.3 Stringlərlə İş**

### 🧩 **Stringlərin birləşdirilməsi**
JavaScript-də stringləri `+` operatoru ilə birləşdirə bilərik:
```js
let msg = "Hello, " + "world"; // "Hello, world"
let greeting = "Welcome to my blog, " + name;
```

### 🧪 **Stringlərin müqayisəsi**
Stringlər `===`, `!==`, `<`, `<=`, `>`, `>=` operatorları ilə müqayisə oluna bilər. Bu müqayisələr UTF-16 kodlarına əsaslanır.

---

### 📏 **String uzunluğu**
Stringin uzunluğunu `length` xüsusiyyəti ilə öyrənə bilərik:
```js
let s = "Hello";
console.log(s.length); // 5
```

---

### ✂️ **Stringdən hissə almaq**
```js
let s = "Hello, world";

s.substring(1, 4); // "ell"
s.slice(1, 4);     // "ell"
s.slice(-3);       // "rld"
s.split(", ");     // ["Hello", "world"]
```

---

### 🔍 **Stringdə axtarış**
```js
s.indexOf("l");       // 2
s.indexOf("l", 3);    // 3
s.indexOf("zz");      // -1
s.lastIndexOf("l");   // 10

s.startsWith("Hell"); // true
s.endsWith("!");      // false
s.includes("or");     // true
```

---

### ✏️ **Stringin dəyişdirilməsi**
```js
s.replace("llo", "ya");    // "Heya, world"
s.toLowerCase();           // "hello, world"
s.toUpperCase();           // "HELLO, WORLD"
s.normalize();             // Unicode normalizasiya
```

---

### 🔠 **Simvollarla işləmək**
```js
s.charAt(0);           // "H"
s.charAt(s.length-1);  // "d"
s.charCodeAt(0);       // 72
s.codePointAt(0);      // 72
```

---

### 🧱 **String padding (ES2017)**
```js
"x".padStart(3);         // "  x"
"x".padEnd(3);           // "x  "
"x".padStart(3, "*");    // "**x"
"x".padEnd(3, "-");      // "x--"
```

---

### 🧼 **Boşluqların təmizlənməsi**
```js
" test ".trim();        // "test"
" test ".trimStart();   // "test "
" test ".trimEnd();     // " test"
```

---

### 🧬 **Əlavə metodlar**
```js
s.concat("!");          // "Hello, world!"
"<>".repeat(5);         // "<><><><><>"
```

---

### 🔒 **Stringlər immutable-dir**
`replace()` və `toUpperCase()` kimi metodlar yeni string qaytarır, köhnəni dəyişmir.

---

### 📚 **Stringləri massiv kimi istifadə**
```js
let s = "hello, world";
s[0];               // "h"
s[s.length - 1];    // "d"
```

Çox gözəl! Aşağıdakı kimi bu hissəni daha oxunaqlı və təmiz bir formatda təqdim edirəm. Həmçinin bəzi əlavə şərhlər və başlıqlar da əlavə edirəm ki, öyrənmə daha rahat olsun:

---

## **3.3.4 📌 Template Literalları**

ES6-dan etibarən JavaScript-də stringləri `backtick` (`` ` ``) işarəsi ilə yarada bilərik:

```js
let s = `hello world`;
```

### 💡 **Template literal-ların yaranma səbəbi**
String içərisində dəyişənləri və ifadələri asanlıqla yerləşdirmək üçün istifadə olunur:

```js
let name = "Bill";
let greeting = `Hello ${name}.`; // greeting = "Hello Bill."
```

> `${}` içərisində istənilən JavaScript ifadəsi (`expression`) yaza bilərsən.  
> `{} — curly braces` (fiqurlu mötərizə) adlanır.  
> `${}` xaricindəki hər şey sadə string literal mətnidir.

---

## **3.3.5 🧪 Model Uyğunluğu (Regular Expressions - RegExp)**

JavaScript mətn sətirlərində nümunələri tapmaq üçün **regular expression** (`RegExp`) adlı xüsusi data tipi təqdim edir.

### 🔣 **RegExp nümunələri**
```js
/^HTML/;             // "HTML" ilə başlayan string
/[1-9][0-9]*/;       // 0 olmayan bir rəqəm + istənilən sayda rəqəm
/\bjavascript\b/i;   // "javascript" sözünü tap, böyük-kiçik fərq etmədən
```

---

### 🧰 **RegExp ilə işləyən metodlar**

```js
let text = "testing: 1, 2, 3";
let pattern = /\d+/g; // bir və ya daha çox rəqəmi tap (global axtarış)

pattern.test(text);        // true: uyğun gələn hissə var
text.search(pattern);      // 9: ilk uyğunluğun başlanğıc indeksi
text.match(pattern);       // ["1", "2", "3"]: bütün uyğunluqları qaytarır
text.replace(pattern, "#"); // "testing: #, #, #"
text.split(/\D+/);         // ["", "1", "2", "3"]: rəqəm olmayan simvollar üzrə parçala
```

> `\d` — rəqəmlər  
> `\D` — rəqəm olmayanlar  
> `\b` — söz sərhədi  
> `i` — case-insensitive (böyük/kiçik fərq etməz)  
> `g` — global axtarış (bir dəfədən çox tapmaq üçün)

---

## **3.4 🔁 Boolean Dəyərlər**

### ✅ **Boolean nədir?**
Boolean dəyər yalnız **iki mümkün nəticədən birini** ifadə edə bilər:  
- `true` (doğru)  
- `false` (səhv)  

Bu dəyərlər əsasən **müqayisə nəticəsində** və **şərt bloklarında** istifadə olunur.

### 🔍 **Müqayisə nümunəsi**
```js
let a = 4;
console.log(a === 4); // true
```

### 🧠 **Boolean ilə if/else istifadə**
```js
if (a === 4) {
  console.log("SALAM TRUE");
} else {
  console.log("SALAM FALSE");
}
```

---

### 🛠️ **JavaScript-də avtomatik boolean çevrilməsi**

JavaScript-də bəzi dəyərlər **avtomatik olaraq `boolean`** tipinə çevrilir.  

#### ❌ **Falsy (yalançı) dəyərlər:**
Aşağıdakı dəyərlər `false` kimi qəbul olunur:
- `undefined`
- `null`
- `0`
- `-0`
- `NaN`
- `""` (boş string)

#### ✅ **Truthy (doğru) dəyərlər:**
Bütün digər dəyərlər, o cümlədən:
- string-lər (`"hello"`)
- array-lar (`[]`)
- obyektlər (`{}`)
`true` kimi qiymətləndirilir.

---

### 🔁 **Boolean ilə `.toString()` metodu**
```js
let a = true;
a.toString(); // "true"
```

---

## **🔗 Müqayisə operatorları və məntiqi əməliyyatlar**

### `&&` — **AND** operatoru
İki tərəf də `true` olarsa nəticə `true` olur:
```js
4 > 3 && 5 > 4 // true && true => true
```

### `||` — **OR** operatoru
Tərəflərdən **ən az biri** `true` olarsa nəticə `true` olur:
```js
4 > 3 || 4 > 5 // true || false => true
```

### `!` — **NOT** operatoru
Dəyərin **əksini** qaytarır:
```js
!(4 > 6) // !(false) => true
```

---

Bu mövzu JavaScript-də çox vacibdir, çünki demək olar ki, **bütün şərt blokları və dövrlər** `boolean` dəyərlərlə işləyir.
---

## **3.5 🌀 Null və Undefined**

JavaScript-də **dəyərin olmamasını və ya boşluğunu** ifadə etmək üçün iki əsas dəyər var:  
**`null`** və **`undefined`**

---

### 🕳️ **`null` – Məlum şəkildə boşluq**
- **`null`** bir dəyərin **qəsdən boş olduğunu** göstərmək üçün istifadə olunur.
- Sanki “burada dəyər olmalı idi, amma mən qəsdən boş buraxıram” deməkdir.
- `typeof null` nəticəsi `object` olur (JavaScript-in tarixi bir səhvidir).

```js
let a = null;
console.log(typeof a); // "object"
```

> ✅ Digər dillərdə ekvivalentləri:
- Python: `None`
- SQL: `NULL`
- Ruby: `nil`

---

### 🕳️ **`undefined` – Naməlum və ya təyin olunmamış**
- **`undefined`** isə o deməkdir ki, dəyişən yaradılıb, amma **heç bir dəyər təyin edilməyib**.
- JavaScript özü avtomatik bu dəyəri verir.

```js
let b;
console.log(b); // undefined
console.log(typeof b); // "undefined"
```

---

### 🔍 **Fərqləri və ortaq cəhətləri**

| Özəllik           | `null`                 | `undefined`            |
|------------------|------------------------|------------------------|
| Təyin edən       | Proqramçı              | JavaScript özü         |
| Tip (`typeof`)   | `"object"`             | `"undefined"`          |
| Məna             | Qəsdən boş             | Təyin edilməyib        |
| Boolean kimi     | `false`                | `false`                |
| Metod/funksiyası | Yoxdur                 | Yoxdur                 |

---

### ❗ **== və === fərqi**
```js
null == undefined // true (dəyərləri oxşardır)
null === undefined // false (tipləri fərqlidir)
```

---

### 🧠 **Tövsiyə və şəxsi yanaşma**
- **`undefined`**: sistem səviyyəsində, yəni dəyişən hələ dəyər almamışsa.
- **`null`**: proqram səviyyəsində, yəni "bu dəyər bilərəkdən boşdur".

> 💡 Əgər dəyişkənə və ya funksiyaya mütləq bir boşluq ötürmək lazımdırsa, adətən `null` istifadə etmək **daha aydın və məqsədli** görünür.

---

## **3.6 🧩 Symbols (Simvollar)**

### ✨ **Symbol nədir?**
`Symbol` — **ES6 ilə gəlmiş**, unikal və dəyişməz bir **data tipi**dir. String olmayan property name-lər yaratmaq və **unikallıq təmin etmək** üçün istifadə olunur.

```js
let strname = "string name"; // Adi string property
let symname = Symbol("propname"); // Simvol olaraq property adı
```

| İfadə             | Nəticə        |
|------------------|---------------|
| `typeof strname` | `"string"`    |
| `typeof symname` | `"symbol"`    |

---

### 🧠 **Simvolların əsas xüsusiyyətləri:**
- `Symbol()` **həmişə unikal bir dəyər** qaytarır, **eyni arqumentlə belə**.
- `Symbol` **literal sintaksisə malik deyil**, sadəcə `Symbol()` funksiyası ilə yaradılır.
- Simvollar **obyektlərdə propery adı** kimi istifadə oluna bilər.

```js
let o = {};
o[strname] = 1;
o[symname] = 2;

console.log(o[strname]);  // 1
console.log(o[symname]);  // 2
```

---

### 🧪 **toString() metodu**
```js
let s = Symbol("sym_x");
console.log(s.toString()); // "Symbol(sym_x)"
```
> ✅ Bu metod simvolun adına baxmağa imkan verir (debug məqsədilə faydalıdır).

---

### 🌍 **Symbol.for() və Symbol.keyFor()**
`Symbol.for()` — simvolu **global registrdə** saxlayır. Əgər eyni açarla yenidən çağırsan, **eyni simvolu qaytarır**.

```js
let s = Symbol.for("shared");
let t = Symbol.for("shared");

console.log(s === t); // true
console.log(s.toString()); // "Symbol(shared)"
console.log(Symbol.keyFor(t)); // "shared"
```

#### Fərq:
| Metod          | Xüsusiyyət                             |
|----------------|----------------------------------------|
| `Symbol()`     | **Həmişə yeni və unikal** dəyər yaradır |
| `Symbol.for()` | **Global və paylaşılan** dəyər yaradır  |

---

### 🧭 **Simvollar niyə faydalıdır?**
- **Adların toqquşmasının qarşısını almaq** üçün.
- Obyektdə **gizli və ya xüsusi property-lər** yaratmaq üçün.
- Məsələn: framework və ya kitabxanalarda daxili istifadə üçün.

---

## **3.7 🌐 Qlobal Obyektlər (Global Objects)**

### 🧾 **Qlobal obyekt nədir?**
Qlobal obyekt — JavaScript mühərriki işə düşən kimi avtomatik **yaradılan** və proqram boyu **mövcud olan** obyektlər toplusudur.

Bu obyektin xassələri və metodları **hər yerdə mövcuddur**, çünki onlar **global scope**-a aiddir.

---

### 🪄 **Qlobal Sabitlər (Global Constants)**

| Ad         | Açıqlama                          |
|------------|-----------------------------------|
| `undefined`| Təyin olunmamış dəyər             |
| `Infinity` | Sonsuz dəyər                      |
| `NaN`      | "Not a Number" – Ədədi olmayan    |

---

### ⚙️ **Qlobal Funksiyalar (Global Functions)**

| Funksiya       | Açıqlama                                     |
|----------------|----------------------------------------------|
| `isNaN(x)`     | Dəyərin NaN olub olmadığını yoxlayır         |
| `parseInt(str)`| String-i tam ədədə çevirir                   |
| `eval(str)`    | String ifadəni icra edir (riskli ola bilər)  |

---

### 🏗️ **Qurucu (Constructor) Funksiyalar**

Qlobal obyekt tərəfindən təmin edilən `new` açarı ilə istifadə olunan sinifvari funksiyalar:

```js
new Date();      // Tarix və saat obyektləri
new RegExp();    // Regular expression
new String();    // String obyekti
new Object();    // Ümumi obyekt
new Array();     // Array (massiv)
```

---

### 📦 **Qlobal obyektlər (Global objects)**

- **`Math`** — Riyazi funksiyalar və sabitlər üçün.
- **`JSON`** — JSON məlumatları ilə işləmək üçün (`stringify`, `parse`).

---

### 🪟 **Brauzerdə qlobal obyekt: `window`**

- Web brauzerdə `window` obyekti **bütün qlobal dəyişənləri və funksiyaları özündə saxlayır**.
- Misal:
  ```js
  var x = 10;
  console.log(window.x); // 10
  ```

> 📌 Qeyd: Node.js mühitində isə bu obyektin adı `global` olur, `window` deyil.

### 🧠 Nəticə
Qlobal obyektlər JavaScript-in **əsas dayaqlarıdır**. Onlar bizə **ilk gündən** müxtəlif funksiyalar, sabitlər və obyektlər təqdim edir.

---

### **3.8 İmmutabl Primitiv Dəyərlər və Mutable Obyekt Referansları**

JavaScript-də **primitiv dəyərlər** və **obyektlər** arasında çox əhəmiyyətli fərqlər mövcuddur. 

#### **Primitivlər (İmmutabl)**

Primitiv dəyərlər (undefined, null, boolean, number, string) **değişdirilə bilməz**. Yəni, bu tiplə bağlı olan dəyişənlər yaradıldıqdan sonra onların dəyəri dəyişdirilə bilməz.

##### **Məsələn:**
```js
let s = "hello"; // String yaradılır
s.toUpperCase(); // "HELLO" qaytarır, amma `s` dəyişməz qalır
s                // "hello": original string dəyişməyib
```

- **Stringlər**: Stringlərin xarakterlərdən ibarət olduğunu düşünsək də, **onları dəyişmək mümkün deyil**. String metodları yalnız **yeni string qaytarır**.

##### **Primitivlər müqayisə olunur:**

İki primitiv dəyər yalnız **eyni dəyəri saxladıqları halda** bərabər sayılır.

```js
const friend = "Rashad"
const father = "Rashad"
friend === father  // true: çünki eyni dəyəri saxlayırlar
```

#### **Obyektlər (Mutable)**

Obyektlər (array-lər və funksiyalar) **mutable** (değişdirilə bilən) olub, onların **xassələri** dəyişdirilə bilər.

##### **Məsələn:**
```js
let o = {x: 1};   // Obyekt yaradılır
o.x = 2;          // Obyekti dəyişdiririk
o.y = 3;          // Yeni xassə əlavə edirik

let a = [1, 2, 3]; // Array yaradılır
a[0] = 0;          // Array-in bir elementini dəyişirik
a[3] = 4;          // Yeni element əlavə edirik
```

#### **Obyektlərin müqayisəsi**

İki obyekt **eyni dəyəri** saxlasalar belə, onlar **bərabər sayılmır**. Çünki obyektlər referans tipləridir və onları müqayisə edərkən **referanslar** yoxlanılır.

```js
let o = {x: 1};
let p = {x: 1};
o === p // false: Çünki onlar iki fərqli obyektlərdir

let a = [], b = [];  // İki boş array
a === b // false: Çünki onlar fərqli array-lardır
```

#### **Referans Tipi**

Obyektlər **referans tipləridir** və müqayisə zamanı **onların eyni obyektə istinad edib-etmədiyi** yoxlanılır.

```js
let a = [];  // Array yaradılır
let b = a;   // `b` `a` ilə eyni array-a istinad edir
b[0] = 1;    // `b` vasitəsilə array-ı dəyişirik
a[0]         // 1: `a` da dəyişdi, çünki `a` və `b` eyni obyektə istinad edir
a === b      // true: `a` və `b` eyni obyektə istinad edir
```

#### **Yeni Kopya Yaratmaq (Copying Arrays)**

Obyektin (və ya array-in) yeni kopyasını yaratmaq üçün onun bütün elementlərini eyni ilə kopyalamaq lazımdır. Aşağıda array-lərin necə kopyalanması göstərilib:

```js
let a = ["a", "b", "c"];  // Orijinal array
let b = [];  // Yeni array yaradılır

// Array-i dövr edərək kopyalayırıq
for (let i = 0; i < a.length; i++) {
    b[i] = a[i]; // Elementi b-yə kopyalayırıq
}

let c = Array.from(b);  // ES6-da Array.from() ilə kopyalama
```

#### **Array-ləri müqayisə etmək (Array Comparison)**

İki array-i müqayisə etmək üçün aşağıdakı funksiyanı istifadə edə bilərik:

```js
function equalArrays(a, b) {
    if (a === b) return true; // Eyni array-lər bərabərdir
    if (a.length !== b.length) return false; // Uzunluqları fərqli olan array-lər bərabər deyil
    for (let i = 0; i < a.length; i++) {
        if (a[i] !== b[i]) return false; // Hər hansı bir element fərqlidirsə, array-lər bərabər deyil
    }
    return true; // Əks halda, array-lər bərabərdir
}
```

#### **Nəticə**

- **Primitiv dəyərlər** dəyişdirilə bilməz və **dəyər əsaslı** müqayisə edilir.
- **Obyektlər** dəyişdirilə bilər və **referans əsaslı** müqayisə edilir.

Bu xüsusiyyətləri başa düşmək, JavaScript-də yaddaş idarəsi və dəyişənlər üzərində işləməkdə çox faydalıdır.

Çox gözəl, bu bölmə 3.9 Type Conversions JavaScript-in ən vacib və praktik hissələrindən biridir. Mən sənə bu notları bir az daha strukturlaşdırılmış və aydın şəkildə **Azerbaycanca** aşağıda təqdim edirəm. İstəsən, Markdown və ya PDF faylı kimi də verə bilərəm.

---

## 📘 3.9 Tip Çevrilmələri (Type Conversions)

JavaScript-də tip çevrilməsi çox geniş yayılmışdır. Bəzi dəyərlər `truthy` və `falsy` olaraq `boolean` tipinə çevrilə bilər.

---

### 🔁 Avtomatik Tip Çevrilmələri

```js
10 + " objects"   // "10 objects": ədəd `string`-ə çevrilir
"7" * "4"         // 28: hər iki string `number`-a çevrilir
let n = 1 - "x";  // NaN: "x" sayı deyil
n + " objects"    // "NaN objects": NaN stringə çevrilir
```

---

### 🔢 Dəyərlərin Tipə Çevrilməsi

| Dəyər          | `String`-ə        | `Number`-a | `Boolean`-a |
| -------------- | ----------------- | ---------- | ----------- |
| `undefined`    | `"undefined"`     | `NaN`      | `false`     |
| `null`         | `"null"`          | `0`        | `false`     |
| `true`         | `"true"`          | `1`        | `true`      |
| `false`        | `"false"`         | `0`        | `false`     |
| `""`           | `""`              | `0`        | `false`     |
| `"1.2"`        | `"1.2"`           | `1.2`      | `true`      |
| `"one"`        | `"one"`           | `NaN`      | `true`      |
| `0`            | `"0"`             | `0`        | `false`     |
| `-0`           | `"0"`             | `-0`       | `false`     |
| `Infinity`     | `"Infinity"`      | `Infinity` | `true`      |
| `NaN`          | `"NaN"`           | `NaN`      | `false`     |
| `1`, `-1`      | `"1"`, `"-1"`     | `1`, `-1`  | `true`      |
| `[]`           | `""`              | `0`        | `true`      |
| `[9]`          | `"9"`             | `9`        | `true`      |
| `['a']`        | `"a"`             | `NaN`      | `true`      |
| `{}`           | `[object Object]` | `NaN`      | `true`      |
| `function(){}` | `function...`     | `NaN`      | `true`      |

---

### 🧮 3.9.1 Bərabərlik və Tip Çevrilməsi

`==` operatoru **tip çevrilməsi** edir, `===` isə etmir.

```js
null == undefined    // true
"0" == 0             // true
0 == false           // true
"0" == false         // true
```

---

### ✍️ 3.9.2 Açıq (Explicit) Çevrilmələr

```js
Number("3")        // 3
String(false)      // "false"
Boolean([])        // true

x + ""             // String(x)
+x                 // Number(x)
x - 0              // Number(x)
!!x                // Boolean(x)
```

### 🔢 Ədədləri fərqli əsaslara çevirmək

```js
let n = 17;
n.toString(2);   // "10001"
n.toString(8);   // "21"
n.toString(16);  // "11"
```

### 📐 Say formatlama metodları

```js
let n = 123456.789;
n.toFixed(2)        // "123456.79"
n.toExponential(3)  // "1.235e+5"
n.toPrecision(4)    // "1.235e+5"
```

---

### 🧾 `parseInt()` və `parseFloat()`

```js
parseInt("3 blind mice")    // 3
parseFloat(" 3.14 meters")  // 3.14
parseInt("-12.34")          // -12
parseInt("0xFF")            // 255
parseInt("077", 8)          // 63
parseInt("077", 10)         // 77
parseInt("11", 2)           // 3
parseInt("ff", 16)          // 255
parseInt("zz", 36)          // 1295
```

> **Qeyd**: `parseInt()` ikinci arqument kimi **say sistemini** (radix) qəbul edir.

---

### 🧠 3.9.3 Object-dən Primitive-ə Çevrilmə

> Bu hissə gələcəkdə əlavə olunacaq. (`valueOf()` və `toString()` metodları istifadə olunur.)

---

### **3.10 Dəyişənlərin Təyini və Qiymət Verilməsi**

Proqramlaşdırmada ən əsas anlayışlardan biri adlardan (yəni **identifikatorlardan**) istifadə edərək dəyərləri təmsil etməkdir. Bir ada dəyər təyin etdikdə, bu ada **dəyişən** deyirik. Dəyişən o deməkdir ki, bu dəyər proqramın işləmə gedişində dəyişə bilər. Əgər bir ada sabit (dəyişməyən) dəyər təyin etsək, buna **sabit (constant)** deyirik.

---

### **3.10.1 `let` və `const` ilə Təyin**

#### ✅ `let` ilə dəyişən təyini:

```js
let i;
let sum;
let i = 0, j = 0, k = 0;
let x = 2, y = x * x;
```

* **Yaxşı təcrübə:** Dəyişəni yaradanda ona ilkin dəyər vermək.
* Əgər `let` ilə dəyişən yaradılıbsa amma dəyər verilməyibsə, onun dəyəri `undefined` olacaq.

#### ✅ `const` ilə sabit təyini:

```js
const H0 = 74; // Hubble sabiti
const C = 299792.458; // İşıq sürəti
const AU = 1.496E8; // Astronomik vahid (km)
```

* `const` ilə təyin olunan dəyərlər dəyişdirilə bilməz.

---

### **Dəyişən və Sabitlərin Göstəriş Sahəsi (Scope)**

* `let` və `const` **blok səviyyəli skop**-a malikdir. Yəni yalnız təyin edildiyi blok daxilində keçərlidir.

---

### **Təkrar Təyinlər və Səhvlər**

* Eyni dəyişəni `let` və ya `const` ilə birdən çox dəfə təyin etmək **sintaksis xətasıdır**:

```js
const x = 1;
if (x === 1) {
    let x = 2; // Bu blok daxilində fərqli x-dir
    console.log(x); // 2
}
console.log(x); // 1
let x = 3; // Xəta! x artıq təyin olunub
```

---

### **Təyinlər və Tiplər**

* C və Java kimi statik tiplərdə dəyişənin tipi əvvəlcədən yazılır.
* Amma JavaScript dinamik tiplidir:

```js
let i = 10;
i = "Rashad"; // tip dəyişir, bu normaldır
```

---

### **3.10.2 `var` ilə Dəyişən Təyini (köhnə üsul)**

* **ES6-dan əvvəl** dəyişənləri yalnız `var` ilə təyin etmək olurdu:

```js
var x;
var data = [], count = data.length;
for(var i = 0; i < count; i++) console.log(data[i]);
```

#### 🔍 `var` ilə `let` arasındakı əsas fərqlər:

1. `var` **blok skop** deyil, **funksiya skop**-dur.
2. `var` ilə təyin olunan dəyişənlər qlobal obyektin (`globalThis`) bir hissəsi olur və silinə bilməz.
3. `var` ilə eyni adda dəyişəni təkrar təyin etmək mümkündür.
4. `var` təyin olunan dəyişənlər **hoisting** edilir – yəni, kodun yuxarısına "qaldırılır". Bu zaman dəyəri `undefined` olur və xəta çıxmır:

```js
console.log(x); // undefined
var x = 5;
```

Amma `let` və `const` ilə belə hal xəta verir.

---

### **3.10.3 Parçalanmış Mənimsətmə (Destructuring Assignment)**

**Destructuring**, yəni **parçalayaraq mənimsətmə**, massiv və obyektlərdən müəyyən dəyərləri rahatlıqla çıxarmağa imkan verir.

#### **Array-lərlə istifadə**

```js
let [x, y] = [1, 2]; // x = 1, y = 2
[x, y] = [x + 1, y + 1]; // x = 2, y = 3
[x, y] = [y, x]; // x = 3, y = 2 (dəyərləri dəyişdilər)
```

#### **Funksiya ilə istifadə**

```js
function toPolar(x, y) {
  return [Math.sqrt(x*x + y*y), Math.atan2(y, x)];
}
function toCartesian(r, theta) {
  return [r * Math.cos(theta), r * Math.sin(theta)];
}
let [r, theta] = toPolar(1, 1);
let [x, y] = toCartesian(r, theta);
```

#### **Obyektlərdə istifadə**

```js
let o = { x: 1, y: 2 };
for (const [name, value] of Object.entries(o)) {
  console.log(name, value); // "x 1", "y 2"
}
```

#### **Əlavə Misallar**

```js
let [x, y] = [1]; // x = 1, y = undefined
[x, y] = [1, 2, 3]; // x = 1, y = 2
[, x, , y] = [1, 2, 3, 4]; // x = 2, y = 4
let [x, ...y] = [1, 2, 3, 4]; // x = 1, y = [2, 3, 4]
```

#### **İç-içə strukturlar**

```js
let [a, [b, c]] = [1, [2, 2.5]]; // a = 1, b = 2, c = 2.5
```

#### **String-lə istifadə (iterable)**

```js
let [first, ...rest] = "Hello"; // first = "H", rest = ["e", "l", "l", "o"]
```

#### **Obyekt Parçalanması**

```js
let transparent = { r: 0.0, g: 0.0, b: 0.0, a: 1.0 };
let { r, g, b } = transparent; // r = 0.0, g = 0.0, b = 0.0
```

#### **Alias (ad dəyişmə)**

```js
const { cos: cosine, tan: tangent } = Math; // cosine = Math.cos, tangent = Math.tan
```

#### **İç-içə obyektlərdən/parçalanma**

```js
let points = [{ x: 1, y: 2 }, { x: 3, y: 4 }];
let [{ x: x1, y: y1 }, { x: x2, y: y2 }] = points;
// x1 = 1, y1 = 2, x2 = 3, y2 = 4
```

#### **Object of Array parçalama (tövsiyə olunur)**

```js
let points = { p1: [1, 2], p2: [3, 4] };
let { p1: [x1, y1], p2: [x2, y2] } = points;
// x1 = 1, y1 = 2, x2 = 3, y2 = 4
```
