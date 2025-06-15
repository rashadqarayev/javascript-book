# Chapter 3: Tiplər, Dəyərlər və Dəyişənlər

JavaScript proqramları rəqəmlər (`3.1`), mətnlər (`"Hello, world!"`) və digər məlumatlar üzərində işləyir. Bu məlumatlara **dəyər (value)** deyilir. Dəyərləri yadda saxlamaq, onlara istinad etmək və dəyişmək üçün isə **dəyişənlər (variables)** istifadə olunur.

Dəyərlərin hər biri müəyyən bir **tipə (type)** aiddir. Proqramlaşdırma dillərində tip anlayışı — verilən dəyərin hansı formada olduğunu (məsələn, ədəd, mətn, obyekt və s.) müəyyən edir.

Əgər proqramda bir dəyəri gələcəkdə də istifadə etməyi planlaşdırırıqsa, onu dəyişənə mənimsədirik. Dəyişənin adı olur və həmin adla dəyərə istinad edirik.

---

## 3.1 İcmal və Təriflər

JavaScript-də tiplər 2 əsas kateqoriyaya bölünür:

* **Primitiv tiplər**
* **Obyekt tipləri**

### Primitiv Tiplər

JavaScript-də primitiv tiplərə aşağıdakılar daxildir:

* `number` – rəqəmlər
* `string` – mətnlər
* `boolean` – `true` və ya `false`
* `null`
* `undefined`
* `symbol` (ES6 ilə əlavə olunub)

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

Obyekt – ad (`key`) və dəyər (`value`) cütlüklərindən ibarət olan bir kolleksiyadır.

```js
let person = {
  name: "Ali",
  age: 30
};
```

## Xüsusi Obyekt Tipləri

JavaScript-də adi `object` tipindən başqa, məlumatları fərqli yollarla saxlamaq və işləmək üçün istifadə olunan **xüsusi obyekt tipləri** də mövcuddur. Onlar aşağıdakılardır:

### 1. Array – ardıcıl və nömrələnmiş dəyərlərin toplusu

Array – birdən çox dəyəri bir yerdə saxlamaq üçün istifadə olunur. Bu dəyərlərin **sırası və nömrəsi (index)** var.

```js
let numbers = [1, 2, 3]; // index-lər: 0 → 1, 1 → 2, 2 → 3  
console.log(numbers[0]); // 1  
```

---

### 2. Set – təkrarsız dəyərlərin toplusu

Set – içində **eyni dəyər bir neçə dəfə ola bilməz**. Yəni, təkrar dəyərlər avtomatik çıxarılır.

```js
let unique = new Set([1, 2, 2, 3]);  
console.log(unique); // Set(3) {1, 2, 3}  
```

Faydalıdır: Məsələn, bir massivin içində neçə fərqli dəyər olduğunu tapmaq üçün.

---

### 3. Map – açar-dəyər (key-value) cütlükləri

Map – hər açarın qarşısında bir dəyər saxlanılır. Obyektdən fərqli olaraq:

* Açar hər şey ola bilər (`string`, `number`, `object` və s.)
* Əlavə olunma sırası qorunur

```js
let capitals = new Map();  
capitals.set("Azerbaijan", "Baku");  
capitals.set("Turkey", "Ankara");  

// dəyəri əldə etmək:  
console.log(capitals.get("Turkey")); // "Ankara"  
```

#### Niyə Map istifadə edirik?

* `Object`-dən daha çevikdir
* Açarın tipi daha müxtəlif ola bilər
* `.size` ilə uzunluq asan tapılır

```js
console.log(capitals.size); // 2  
```

---

### 4. Typed Arrays – xüsusi tipli massivlər

Bunlar `Int8Array`, `Float32Array` kimi xüsusi massivlərdir və **yalnız müəyyən tipdə ədədlər** saxlayır. Daha çox **performans** tələb edən hesablama və ya WebGL kimi sahələrdə istifadə olunur.

```js
let intArray = new Int16Array([10, 20, 30]);  
```

---

### 5. RegExp (Regular Expression) – mətnlə işləmək üçün qaydalar

Mətnlərdə müəyyən nümunələr tapmaq və ya dəyişmək üçün istifadə olunur.

```js
let pattern = /hello/i;  
console.log(pattern.test("Hello World")); // true  
```

---

### 6. Date – tarix və saat obyektləri

Tarix və saatla işləmək üçün `Date` obyekti istifadə olunur.

```js
let now = new Date();  
console.log(now.toDateString()); // məsələn: "Tue Apr 23 2025"  
```

---

### 7. Error və alt tipləri – səhvlərlə işləmək üçün

JavaScript-də `Error`, `TypeError`, `SyntaxError` və s. kimi səhv tipləri var. Proqramın gedişində bir problem olduqda istifadə olunur.

```js
try {  
  throw new Error("Nə isə səhv getdi");  
} catch (err) {  
  console.log(err.message); // "Nə isə səhv getdi"  
}  
```

---

## Yekun

| Tip          | Nə üçündür?                       |
| ------------ | --------------------------------- |
| `Array`      | Dəyərləri sıra ilə saxlamaq       |
| `Set`        | Təkrarsız dəyərlər saxlamaq       |
| `Map`        | Açar-dəyər cütləri saxlamaq       |
| `TypedArray` | Performanslı nömrə massivləri     |
| `RegExp`     | Mətnlərdə nümunə tapmaq           |
| `Date`       | Tarix və saatla işləmək           |
| `Error`      | Səhvləri idarə etmək və bildirmək |

---

## Funksiya və Klasslar

JavaScript-də funksiyalar və klasslar da dəyər sayılır və özləri obyektlərin xüsusi növüdür.

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

## Yaddaş İdarəetməsi və Garbage Collection

JavaScript-də yaddaş **avtomatik şəkildə** idarə olunur. Sən bir dəyişən yaradanda, yaddaşda yer ayrılır. Əgər o dəyişənə artıq istinad (link) yoxdursa, **garbage collector** həmin obyektin artıq lazım olmadığını başa düşür və onu yaddaşdan silir.

### Sadə Nümunə:

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

Burada `user` adlı obyekt yaranır və `user1` dəyişəni ona istinad edir. Bu obyekt **hələ istifadə olunur**, ona görə yaddaşda qalır.

---

### Garbage Collection nə vaxt baş verir?

Əgər obyektə **artıq heç bir istinad yoxdursa**, JavaScript onun "lazımsız" olduğunu düşünür və avtomatik silir.

```js
let user2 = {  
  name: "Aysel",  
};  

user2 = null; // artıq obyektə istinad yoxdur  
```

Burada `user2` əvvəlcə bir obyektə işarə edir, sonra `null` veririk. Artıq heç kim o obyektə işarə etmir. JavaScript-in garbage collector mexanizmi bir müddət sonra bu obyekti **yaddaşdan çıxarır**.

---

## Əsas Məqam: JavaScript-də metodlar **obyektlərə bağlıdır**

JavaScript-də bir çox funksiyalar **obyektin içindəki metod** kimi tanınır. `sort` da onlardan biridir və **yalnız Array obyektinə aid olan bir metod**dur.

---

## `sort(a)` – Niyə Səhvdir?

```js
sort(a);  
```

Bu formada yazanda sən deyirsən ki, "adı `sort` olan bir **global funksiyanı** çağır, və `a` array-ni ona ötür."

Amma problem budur ki:
➡️ JavaScript-də `sort` adlı **global (ümumi) funksiya yoxdur!**
➡️ `sort` sadəcə `Array.prototype`-ə məxsus bir metoddur, yəni yalnız array-lərdə mövcuddur.

---

## `a.sort()` – Niyə Doğrudur?

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

## Bunu necə sübut edə bilərik?

```js
console.log(typeof sort);       // undefined (səhv)  
console.log(typeof [].sort);    // 'function' (doğru)  
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

* **Primitiv tiplər**: immutable (dəyişməz)
* **Obyekt tipləri**: mutable (dəyişə bilən)

```js
let num = 5;  
num = 10; // bu yeni dəyərdir, əvvəlki dəyişmir, əvəz olunur  

let arr = [1, 2, 3];  
arr[0] = 99; // array mutable olduğu üçün dəyişə bilər  
```

Qeyd: `string` tipi xarici görünüşcə array kimi olsa da, immutable-dır:

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

* `let` – dəyişən yaratmaq üçün
* `const` – sabit yaratmaq üçün
* `var` – köhnə sintaksis, istifadəsi tövsiyə olunmur

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

### 3.2 JavaScript Rəqəmləri

JavaScript-də rəqəmlər `Number` tipi ilə təmsil olunur. Bu tip həm **tam ədədləri** (məsələn, 3, -7), həm də **onluq kəsr ədədləri** (məsələn, 3.14, -5.6) təmsil edir.

#### Rəqəmlərin Dəqiqliyi

JavaScript, **IEEE 754** standartına əsaslanan bir formatdan istifadə edir ki, bu da rəqəmlərin yaddaşda necə saxlanacağını müəyyən edir. Bu format 64 bitlik bir **floating-point** formatıdır, yəni həm çox **böyük**, həm də çox **kiçik** ədədləri təmsil edə bilir. Məsələn, JavaScript çox böyük ədədləri belə təmsil edə bilir:

* **Böyük ədədlər:** ±1.7976931348623157 × 10^308
* **Kiçik ədədlər:** ±5 × 10^−324

#### Dəqiqlıq Aralığı

JavaScript-də rəqəmlər yalnız müəyyən bir aralıqda dəqiq təmsil oluna bilir. Bu aralıq:

* **Ən kiçik dəqiq rəqəm**: -2^53 (−9,007,199,254,740,992)
* **Ən böyük dəqiq rəqəm**: 2^53 (9,007,199,254,740,992)

Bu aralıqdan daha böyük rəqəmlər istifadə edildikdə, təmsil edilən rəqəmin sonuncu hissələri səhv ola bilər. Məsələn, çox böyük ədədlər istifadə edildikdə, yalnız ilk rəqəmlər dəqiq olur və sonrakılar səhv ola bilər.

#### Rəqəm Literalları

Rəqəm JavaScript proqramında göründüyü zaman buna **numeric literal** (rəqəmli literal) deyilir. JavaScript bir çox növ numeric literal dəstəkləyir. Əlavə olaraq, istənilən numeric literalı mənfi etmək üçün onun qarşısına **-** işarəsi əlavə edə bilərik.

---

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

---

### 3.2.2 Onluq kəsr Ədədləri (Floating-Point Literals)

Onluq kəsr ədədləri (floating-point literals) real ədədlərdir və onlar iki hissədən ibarətdir: bir **tam hissə** və bir **kəsr hissəsi**. Bu ədədlər, eksponensial formatda da yazıla bilər. Məsələn:

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

* `+` — toplamaq
* `-` — çıxmaq
* `*` — vurmaq
* `/` — bölmək
* `%` — qalıq tapmaq

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
Math.log2(1024)     // 10: 1024-ün ikilik loqarifması
Math.log1p(x)       // 1 + x-in təbii logaritması
Math.expm1(x)       // e^x - 1
Math.sign(x)        // x-in işarəsi
Math.imul(2, 3)     // 2 və 3-ün tam sayı ilə hasilini tapır
Math.clz32(0xf)     // 28: 32-bitlik tam ədəddə başdakı sıfırların sayı
Math.trunc(3.9)     // 3: tam hissəsini alır
Math.fround(x)      // 32-bitlik float olaraq yuvarlaqlaşdırır
Math.sinh(x)        // hiperbollu sinus
Math.tanh(x)        // hiperbollu tangens
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

---

### Number Obyektinin Ən Vacib Funksiyaları və Sabitləri

JavaScript-də `Number` obyekti **ədədi dəyərlərlə işləmək üçün xüsusi funksiyalar və sabitlər təqdim edir**. Bunlar istər **parsing** (çevirilmə), istərsə də **doğrulama və yoxlama əməliyyatlarında** istifadə olunur.

---

### `Number.parseInt(x)`

Verilən dəyəri **tam ədədə** çevirməyə çalışır. Əgər mətnin əvvəlində ədəd varsa, onu oxuyur və tam ədəd kimi qaytarır.

**Nümunə:**

```js
Number.parseInt("123")       // 123
Number.parseInt("123.45")    // 123
Number.parseInt("123abc")    // 123
Number.parseInt("abc123")    // NaN
```

**Qeyd:** Ədəddən sonra gələn yazılar nəzərə alınmır, amma ədəd əvvəl gəlməlidir.

---

### `Number.parseFloat(x)`

Verilən dəyəri **onluq kəsr ədədə** çevirməyə çalışır.

**Nümunə:**

```js
Number.parseFloat("123.45")   // 123.45
Number.parseFloat("5.67abc")  // 5.67
Number.parseFloat("abc5.67")  // NaN
```

---

### `Number.isNaN(x)`

`x` dəyərinin **NaN** (Not a Number) olub olmadığını **dəqiq** yoxlayır.

**Nümunə:**

```js
Number.isNaN(NaN)            // true
Number.isNaN("hello")        // false (çünki bu NaN deyil, sadəcə stringdir)
Number.isNaN(parseInt("abc")) // true
```

`isNaN()` ilə `Number.isNaN()` fərqlidir. `Number.isNaN()` daha **dəqiqdir**, yalnız **NaN** olan hallarda `true` verir.

---

### `Number.isFinite(x)`

Dəyərin **sonlu (finite)** ədəd olub olmadığını yoxlayır. `Infinity`, `-Infinity` və `NaN` üçün `false` qaytarır.

**Nümunə:**

```js
Number.isFinite(42)         // true
Number.isFinite(Infinity)   // false
Number.isFinite(NaN)        // false
Number.isFinite("123")      // false (çünki bu stringdir, number deyil)
```

---

### `Number.isInteger(x)`

Dəyərin **tam ədəd** (yəni, onluq hissəsi olmayan) olub olmadığını yoxlayır.

**Nümunə:**

```js
Number.isInteger(10)      // true
Number.isInteger(10.5)    // false
Number.isInteger("10")    // false (çünki stringdir)
```

---

### `Number.isSafeInteger(x)`

**Məqsəd:**
Dəyərin həm **tam ədəd**, həm də **təhlükəsiz ədəd** (JavaScript-də düzgün təmsil oluna bilən aralıqda) olub olmadığını yoxlayır.

Təhlükəsiz tam ədədlər `-(2^53 - 1)` ilə `2^53 - 1` aralığındadır.

**Nümunə:**

```js
Number.isSafeInteger(100)              // true
Number.isSafeInteger(2 ** 53 - 1)      // true
Number.isSafeInteger(2 ** 53)          // false
```

---

### Əlavə Faydalı Sabitlər

```js
Number.MIN_SAFE_INTEGER     // -(2^53 - 1) = -9007199254740991
Number.MAX_SAFE_INTEGER     // 2^53 - 1 = 9007199254740991
```

**Nümunə:**

```js
console.log(Number.EPSILON);  // 2.220446049250313e-16
console.log(0.1 + 0.2 === 0.3); // false!
```

Bəzən onluq kəsr ədədlərin toplanmasında **kiçik fərqlər** olur. `Number.EPSILON` bu fərqləri müqayisə etmək üçün istifadə olunur:

```js
function almostEqual(a, b) {
  return Math.abs(a - b) < Number.EPSILON;
}

almostEqual(0.1 + 0.2, 0.3); // true
```

---

### Yekun Cədvəl (Qısa xülasə)

| Funksiya/Sabit | İstifadə məqsədi |
| :--- | :--- |
| `Number.parseInt(x)` | Tam ədədə çevirmək |
| `Number.parseFloat(x)` | Onluq kəsr ədədə çevirmək |
| `Number.isNaN(x)` | NaN olub olmadığını yoxlamaq |
| `Number.isFinite(x)` | Sonsuz olmayan ədəd olub olmadığını yoxlamaq |
| `Number.isInteger(x)` | Tam ədəd olub olmadığını yoxlamaq |
| `Number.isSafeInteger(x)` | Təhlükəsiz tam ədəd olub olmadığını yoxlamaq |
| `Number.MIN_SAFE_INTEGER` | Ən kiçik təhlükəsiz tam ədəd `-(2^53 - 1)` |
| `Number.MAX_SAFE_INTEGER` | Ən böyük təhlükəsiz tam ədəd `(2^53 - 1)` |
| `Number.EPSILON` | Ən kiçik fərq, `2^-52` |

---

### Mənfi 0

JavaScript-də `-0` və `+0` bərabərdir, lakin onları bölərkən fərqli nəticələr alırıq.

```javascript
let zero = 0;
let negz = -0;

zero === negz           // true: 0 və -0 bərabərdir
1 / zero === 1 / negz   // false: `Infinity` və `-Infinity` fərqli nəticələrdir
```

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

**Qeyd:** Bu bölmə gələcəkdə izah olunacaq.

BigInt JavaScript-də çox böyük tam ədədlərlə işləməyə imkan verir. Standart `Number` tipindən fərqli olaraq, `BigInt` tipində olan ədədlər çox böyük ölçüləri də dəqiq yadda saxlaya bilir.

---

### 3.2.6 Tarix və Zaman

JavaScript tarix və zamanla işləmək üçün `Date` obyektindən istifadə edir. Bu obyekt vasitəsilə istifadəçi hazırki tarixi öyrənə, onu formatlaşdıra və ya zaman fərqlərini hesablaya bilər.

```js
let timestamp = Date.now();         
console.log(timestamp);   // 1750008800000 (Məsələn)
// 1 yanvar 1970 UTC-dən bəri keçən millisaniyəni (Unix timestamp) qaytarır

let now = new Date();               
console.log(now) // Sun Jun 15 2025 14:33:20 GMT+0400 (Azerbaijan Standard Time)
// Hal-hazırdakı tarixi və saatı `Date` obyekti kimi verir
```

### 📘 Əlavə Faydalı Metodlar:

#### Tarixin elementlərini almaq:

```js
let day = now.getDate();         // → 15
let month = now.getMonth();      // → 5 (İyun ayı, 0-dan başlayır)
let year = now.getFullYear();    // → 2025
let hour = now.getHours();       // → 14
let minute = now.getMinutes();   // → 33
let second = now.getSeconds();   // → 20
```

> **Qeyd:** JavaScript-də aylar 0-dan başlayır:
> `0 → Yanvar`, `1 → Fevral`, ..., `11 → Dekabr`


🕒 Timestamp – bu, **1970-01-01T00:00:00Z** tarixindən indiyə qədər keçən **millisaniyələrin** sayıdır.


---


## 3.3 Mətn(Strings)

JavaScript-də mətnləri təmsil edən tip **string** adlanır. String — dəyişməyən (immutable), sıralanmış **16-bit** dəyərlərin ardıcıllığıdır və hər biri bir **Unicode** simvolunu ifadə edir.

> **Qeyd:** JavaScript-də ayrıca `char` (tək simvol) tipi yoxdur. Tək simvol saxlamaq üçün uzunluğu 1 olan string istifadə olunur.

JavaScript **UTF-16** Unicode kodlamasından istifadə edir. Bu səbəbdən bəzi simvollar 1 ədəd 16-bit dəyərlə, bəziləri isə 2 ədəd 16-bit dəyər (surrogate pair) ilə təmsil olunur.

```js
let euro = "€";
let love = "💙"; // Bu, mavi ürək emojisidir

console.log(euro.length); // 1 — bu simvol 1 ədəd 16-bit elementdən ibarətdir
console.log(love.length); // 2 — “💙” surroqat cütü (surrogate pair) olaraq 2 elementlə kodlanır: "\ud83d\udc99"
```

---

### 3.3.1 String literal-ları

JavaScript-də string yaratmaq üçün mətn sadəcə **tək (‘ ’)**, **ikiqat (“ ”)** və ya **backtick (` `)** dırnaqlar içində yazılır.

```js
""                           // Boş string: uzunluğu 0
"testing"                   // Sadə mətn
"3.14"                      // Ədədi string şəklində
"Wouldn't you prefer O'Reilly's book?" // İkiqat dırnaqla yaradılan string daxilində tək dırnaq problemsiz işləyir
"τ is the ratio of a circle's circumference to its radius"

`"She said 'hi'", he said.`  // Backtick stringləri

// İki sətri bir sətrdə ifadə etmək:
'two\nlines'

// Tək sətir, kodda üç sətirdə yazılmış:
"one\
 long\
 line"

// İki sətrli string, sətir sonu (newline) daxil edilir:
`the newline character at the end of this line
is included literally in this string`

// HTML-də tək və ikiqat dırnaqlar birlikdə:
<button onclick="alert('Thank you')">Click Me</button>
```

---

### 3.3.2 String literal-larında qaçış ardıcıllığı

String literal-larında xüsusi simvolları (məsələn, dırnaq, yeni sətir və s.) təmsil etmək üçün **escape sequence** (qaçış ardıcıllığı) istifadə olunur. Bu `\` simvolu ilə başlayır.

```js
'You\'re right, it can\'t be a quote' // \ ilə tək dırnaq qaçırılır
"You're right, it can't be a quote"   // Burada isə ehtiyac yoxdur
```

Ən çox istifadə olunan qaçış ardıcıllıqları:

| Qaçış simvolu | Mənası |
| :--- | :--- |
| `\'` | Tək dırnaq |
| `\"` | İkiqat dırnaq |
| `\\` | Tərs slash (`\`) |
| `\n` | Yeni sətr |
| `\t` | Tab boşluğu |
| `\uXXXX` | Unicode simvolu |

---

## 3.3.3 Stringlərlə İş

### Stringlərin Birləşdirilməsi (Concatenation)

JavaScript-də stringləri `+` operatoru ilə asanlıqla birləşdirə bilərik:

```js
let msg = "Hello, " + "world";   // Nəticə: "Hello, world"

let name = "Rəşad";
let greeting = "Welcome to my blog, " + name; // Nəticə: "Welcome to my blog, Rəşad"
```

---

### Stringlərin Müqayisəsi

Stringləri `===`, `!==`, `<`, `>`, `<=`, `>=` operatorları ilə müqayisə etmək mümkündür. Müqayisə UTF-16 kodları əsasında aparılır:

```js
console.log("abc" === "abc");   // true
console.log("abc" < "abd");     // true (çünki 'c' < 'd')
console.log("a" > "A");         // true (çünki kiçik hərfin UTF-16 kodu böyük hərfdən böyükdür)
```

---

### String Uzunluğu

String-in uzunluğunu `.length` xüsusiyyəti ilə öyrənə bilərik:

```js
let s = "Hello";
console.log(s.length);  // 5
```

---

### Stringdən Hissə Almaq

Stringdən müəyyən hissələri almaq üçün müxtəlif metodlardan istifadə edə bilərik:

```js
let s = "Hello, world";

console.log(s.substring(1, 4)); // "ell" — 1-dən 4-ə (4 daxil deyil)
console.log(s.slice(1, 4));     // "ell" — eyni nəticə verir
console.log(s.slice(-3));       // "rld" — sondan 3 simvolu götürür
console.log(s.split(", "));     // ["Hello", "world"] — vergülə görə ayırır
```

---

### Stringdə Axtarış

String daxilində simvolların və ya alt-stringlərin mövqeyini tapmaq üçün:

```js
console.log(s.indexOf("l"));      // 2 — ilk "l"-in yeri
console.log(s.indexOf("l", 3));   // 3 — 3-cü indeksdən sonra axtarır
console.log(s.indexOf("zz"));     // -1 — tapılmadı
console.log(s.lastIndexOf("l"));  // 10 — sondan axtarış

console.log(s.startsWith("Hell"));  // true
console.log(s.endsWith("!"));       // false
console.log(s.includes("or"));      // true
```

---

### Stringin Dəyişdirilməsi

Stringdə müəyyən hissəni dəyişmək və ya formatlamaq üçün:

```js
console.log(s.replace("llo", "ya"));  // "Heya, world"
console.log(s.toLowerCase());          // "hello, world"
console.log(s.toUpperCase());          // "HELLO, WORLD"
console.log(s.normalize());            // Unicode normalizasiyası üçün
```

---

### Simvollarla İşləmək

Stringdə tək-tək simvollarla işləmək üçün:

```js
console.log(s.charAt(0));           // "H" — ilk simvol
console.log(s.charAt(s.length-1)); // "d" — son simvol
console.log(s.charCodeAt(0));       // 72 — ilk simvolun UTF-16 kodu
console.log(s.codePointAt(0));      // 72 — ilk simvolun Unicode nöqtəsi
```

---

### String Padding (Doldurma) — ES2017

Stringləri sağdan və ya soldan müəyyən simvolla doldurmaq üçün:

```js
console.log("x".padStart(3));          // "  x" — əvvəlinə boşluqla doldurur
console.log("x".padEnd(3));            // "x  " — sonuna boşluq əlavə edir
console.log("x".padStart(3, "*"));     // "**x" — əvvəlinə ulduzlarla doldurur
console.log("x".padEnd(3, "-"));       // "x--" — sonuna tire əlavə edir
```

---

### Boşluqların Təmizlənməsi

Stringin əvvəlində və ya sonunda olan boşluqları təmizləmək üçün:

```js
console.log(" test ".trim());         // "test"
console.log(" test ".trimStart());    // "test "
console.log(" test ".trimEnd());      // " test"
```

---

### Əlavə Metodlar

```js
console.log(s.concat("!"));           // "Hello, world!"
console.log("es".repeat(5));          // "eseseseses"
```

---

### Stringlər Dəyişməzdir (Immutable)

String metodları yeni string qaytarır, mövcud stringi dəyişmir:

```js
let text = "hello";
let upperText = text.toUpperCase();

console.log(text);       // "hello" — dəyişməyib
console.log(upperText);  // "HELLO" — yeni string
```

---

### Stringləri Massiv Kimi İstifadə Etmək

String-dəki simvollara indekslərlə müraciət edə bilərik:

```js
let s = "hello";
console.log(s[0]);          // "h"
console.log(s[s.length-1]); // "o"
```

---

## 3.3.4 Template Literalları

ES6-dan etibarən JavaScript-də stringləri `backtick` (`` ` ``) işarəsi ilə yarada bilərik:

```js
let s = `hello world`;
```

### Template literal-ların yaranma səbəbi
String içərisində dəyişənləri və ifadələri asanlıqla yerləşdirmək üçün istifadə olunur:

```js
let name = "Bill";
let greeting = `Hello ${name}.`; // greeting = "Hello Bill."
```

> `${}` içərisində istənilən JavaScript ifadəsi (`expression`) yaza bilərsiniz.
> `{} — curly braces` (fiqurlu mötərizə) adlanır.
> `${}` xaricindəki hər şey sadə string literal mətnidir.

---

## 3.3.5 Model Uyğunluğu (Regular Expressions - RegExp)

JavaScript mətn sətirlərində nümunələri tapmaq üçün **regular expression** (`RegExp`) adlı xüsusi data tipi təqdim edir.

### RegExp nümunələri
```js
/^HTML/;             // "HTML" ilə başlayan string
/[1-9][0-9]*/;       // 0 olmayan bir rəqəm + istənilən sayda rəqəm
/\bjavascript\b/i;   // "javascript" sözünü tap, böyük-kiçik fərq etmədən
```

---

### RegExp ilə işləyən metodlar

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

## 3.4 Boolean Dəyərlər

### Boolean nədir?
Boolean dəyər yalnız **iki mümkün nəticədən birini** ifadə edə bilər:
- `true` (doğru)
- `false` (səhv)

Bu dəyərlər əsasən **müqayisə nəticəsində** və **şərt bloklarında** istifadə olunur.

### Müqayisə nümunəsi
```js
let a = 4;
console.log(a === 4); // true
```

### Boolean ilə if/else istifadəsi
```js
let a = 4;
if (a === 4) {
  console.log("Şərt doğrudur");
} else {
  console.log("Şərt səhvdir");
}
```

---

### JavaScript-də avtomatik boolean çevrilməsi

JavaScript-də bəzi dəyərlər şərtlərdə yoxlanılarkən **avtomatik olaraq `boolean`** tipinə çevrilir.

#### Falsy (yalançı) dəyərlər:
Aşağıdakı dəyərlər `false` kimi qəbul olunur:
- `undefined`
- `null`
- `0`
- `-0`
- `NaN`
- `""` (boş string)

#### Truthy (doğru) dəyərlər:
"Falsy" dəyərlər istisna olmaqla bütün digər dəyərlər, o cümlədən:
- string-lər (`"hello"`)
- array-lər (`[]`)
- obyektlər (`{}`)
`true` kimi qiymətləndirilir.

---

### Boolean ilə `.toString()` metodu
```js
let a = true;
a.toString(); // "true"
```

---

## Müqayisə operatorları və məntiqi əməliyyatlar

### `&&` — AND operatoru
Hər iki tərəf də `true` olarsa, nəticə `true` olur:
```js
4 > 3 && 5 > 4 // true && true => true
```

### `||` — OR operatoru
Tərəflərdən **ən az biri** `true` olarsa, nəticə `true` olur:
```js
4 > 3 || 4 > 5 // true || false => true
```

### `!` — NOT operatoru
Dəyərin **əksini** qaytarır:
```js
!(4 > 6) // !(false) => true
```

---

## 3.5 Null və Undefined

JavaScript-də **dəyərin olmamasını və ya boşluğunu** ifadə etmək üçün iki əsas dəyər var:
**`null`** və **`undefined`**.

---

### `null` – Məlum şəkildə boşluq
- **`null`** bir dəyərin **qəsdən boş olduğunu** göstərmək üçün proqramçı tərəfindən istifadə olunur.
- Sanki “burada dəyər olmalı idi, amma mən qəsdən boş buraxıram” deməkdir.
- `typeof null` nəticəsi `object` olur (bu, JavaScript-in ilk versiyalarından qalma bir xəta hesab olunur).

```js
let a = null;
console.log(typeof a); // "object"
```

Digər dillərdə ekvivalentləri:
- Python: `None`
- SQL: `NULL`
- Ruby: `nil`

---

### `undefined` – Naməlum və ya təyin olunmamış
- **`undefined`** isə o deməkdir ki, dəyişən yaradılıb, amma ona **hələ heç bir dəyər təyin edilməyib**.
- JavaScript mühərriki özü bu dəyəri avtomatik verir.

```js
let b;
console.log(b); // undefined
console.log(typeof b); // "undefined"
```

---

### Fərqləri və ortaq cəhətləri

| Özəllik | `null` | `undefined` |
| :--- | :--- | :--- |
| Təyin edən | Proqramçı | JavaScript mühərriki |
| Tip (`typeof`) | `"object"` | `"undefined"` |
| Boolean kimi | `false` | `false` |
| Metod/funksiyası | Yoxdur | Yoxdur |

---

### `==` və `===` fərqi
```js
null == undefined // true (dəyərləri boş olduğu üçün oxşar qəbul edilir)
null === undefined // false (tipləri fərqlidir)
```

---

### Tövsiyə və yanaşma
- **`undefined`**: sistem səviyyəsində, yəni dəyişənin hələ dəyər almadığını bildirir.
- **`null`**: proqram məntiqi səviyyəsində, yəni "bu dəyər bilərəkdən boşdur" mənasında.

Əgər bir dəyişənə və ya funksiyaya mütləq bir "boş" dəyər ötürmək lazımdırsa, adətən `null` istifadə etmək **daha aydın və məqsədli** bir yanaşmadır.

---

## 3.6 Symbols (Simvollar)

### Symbol nədir?
`Symbol` — **ES6 ilə gəlmiş**, unikal və dəyişməz bir **primitiv data tipi**dir. String olmayan property adları yaratmaq və **unikallıq təmin etmək** üçün istifadə olunur.

```js
let strname = "string name"; // Adi string property
let symname = Symbol("propname"); // Simvol olaraq property adı
```

| İfadə | Nəticə |
| :--- | :--- |
| `typeof strname` | `"string"` |
| `typeof symname` | `"symbol"` |

---

### Simvolların əsas xüsusiyyətləri:
- `Symbol()` funksiyası **həmişə unikal bir dəyər** qaytarır, hətta arqumentləri eyni olsa belə.
- `Symbol` **literal sintaksisə malik deyil**, yalnız `Symbol()` funksiyası ilə yaradılır.
- Simvollar **obyektlərdə property adı** kimi istifadə oluna bilər.

```js
let o = {};
o[strname] = 1;
o[symname] = 2;

console.log(o[strname]);  // 1
console.log(o[symname]);  // 2
```

---

### `toString()` metodu
```js
let s = Symbol("sym_x");
console.log(s.toString()); // "Symbol(sym_x)"
```
Bu metod simvolun adına baxmağa imkan verir (əsasən sazlama (debug) məqsədilə faydalıdır).

---

### `Symbol.for()` və `Symbol.keyFor()`
`Symbol.for()` — simvolu **qlobal simvol registrində** axtarır və ya yaradır. Əgər eyni açar sözlə yenidən çağırılsa, **eyni simvolu qaytarır**.

```js
let s = Symbol.for("shared");
let t = Symbol.for("shared");

console.log(s === t); // true
console.log(s.toString()); // "Symbol(shared)"
console.log(Symbol.keyFor(t)); // "shared"
```

#### Fərq:
| Metod | Xüsusiyyət |
| :--- | :--- |
| `Symbol()` | **Həmişə yeni və unikal** dəyər yaradır |
| `Symbol.for()` | **Qlobal və paylaşılan** dəyər yaradır və ya qaytarır |

---

### Simvollar niyə faydalıdır?
- Obyekt property-lərində **adların toqquşmasının qarşısını almaq** üçün.
- Obyektdə **gizli və ya xüsusi daxili property-lər** yaratmaq üçün.
- Məsələn, framework və ya kitabxanalarda daxili istifadə üçün ideal vasitədir.

---

## 3.7 Qlobal Obyektlər (Global Objects)

### Qlobal obyekt nədir?
Qlobal obyekt — JavaScript mühərriki işə düşən kimi avtomatik **yaradılan** və proqram boyu **hər yerdən əlçatan olan** obyektlər və dəyişənlər toplusudur. Bu obyektin xassələri və metodları **global scope**-a aiddir.

---

### Qlobal Sabitlər (Global Constants)

| Ad | Açıqlama |
| :--- | :--- |
| `undefined`| Təyin olunmamış dəyər |
| `Infinity` | Sonsuz dəyər |
| `NaN` | "Not a Number" – Ədəd olmayan dəyər |

---

### Qlobal Funksiyalar (Global Functions)

| Funksiya | Açıqlama |
| :--- | :--- |
| `isNaN(x)` | Dəyərin NaN olub olmadığını yoxlayır |
| `parseInt(str)`| String-i tam ədədə çevirir |
| `eval(str)` | String ifadəni icra edir (istifadəsi riskli sayılır) |

---

### Qurucu (Constructor) Funksiyalar

Qlobal obyekt tərəfindən təmin edilən və `new` açar sözü ilə istifadə olunan əsas siniflər:

```js
new Date();      // Tarix və saat obyektləri
new RegExp();    // Regular expression
new String();    // String obyekti
new Object();    // Ümumi obyekt
new Array();     // Array (massiv)
```

---

### Qlobal obyektlər (Global objects)

- **`Math`** — Riyazi funksiyalar və sabitlər üçün.
- **`JSON`** — JSON formatlı məlumatlarla işləmək üçün (`stringify`, `parse`).

---

### Brauzerdə qlobal obyekt: `window`

- Web brauzerdə `window` obyekti **bütün qlobal dəyişənləri və funksiyaları özündə saxlayır**.
- Misal:
  ```js
  var x = 10;
  console.log(window.x); // 10
  ```

**Qeyd:** Node.js mühitində isə bu obyektin adı `global` olur, `window` deyil.

### Nəticə
Qlobal obyektlər JavaScript-in **əsas dayaqlarıdır**. Onlar bizə ilk andan etibarən müxtəlif funksiyalar, sabitlər və obyektlər təqdim edir.

---

## 3.8 Dəyişməz Primitiv Dəyərlər və Dəyişən Obyekt Referansları

JavaScript-də **primitiv dəyərlər** və **obyektlər** arasında çox əhəmiyyətli bir fərq var.

#### Primitivlər (Immutable - Dəyişməz)

Primitiv dəyərlər (undefined, null, boolean, number, string, symbol) **dəyişdirilə bilməz**. Yəni, bu tiplə bağlı olan dəyişənlər yaradıldıqdan sonra onların dəyəri birbaşa dəyişdirilə bilməz.

##### Məsələn:
```js
let s = "hello"; // String yaradılır
s.toUpperCase(); // "HELLO" qaytarır, amma `s` dəyişməz qalır
console.log(s);  // "hello": orijinal string dəyişməyib
```

- **Stringlər**: Stringlərin simvollardan ibarət olduğunu düşünsək də, onların hər hansı bir simvolunu dəyişmək mümkün deyil. String metodları həmişə **yeni bir string qaytarır**.

##### Primitivlərin müqayisəsi:

İki primitiv dəyər yalnız **eyni dəyəri saxladıqları halda** bərabər sayılır.

```js
const text1 = "test"
const text2 = "test"
text1 === text2  // true: çünki eyni dəyəri saxlayırlar
```

#### Obyektlər (Mutable - Dəyişdirilə bilən)

Obyektlər (array-lər və funksiyalar da daxil olmaqla) **mutable** (dəyişdirilə bilən) sayılır, çünki onların **xassələri** sonradan dəyişdirilə bilər.

##### Məsələn:
```js
let o = {x: 1};   // Obyekt yaradılır
o.x = 2;          // Obyektin xassəsini dəyişdiririk
o.y = 3;          // Yeni xassə əlavə edirik

let a = [1, 2, 3]; // Array yaradılır
a[0] = 0;          // Array-in bir elementini dəyişirik
a[3] = 4;          // Yeni element əlavə edirik
```

#### Obyektlərin müqayisəsi

İki fərqli obyekt, xassələri **eyni olsa belə**, bir-birinə bərabər sayılmır. Çünki obyektlər referans tipləridir və onları müqayisə edərkən yaddaşdakı **referanslar** yoxlanılır.

```js
let o = {x: 1};
let p = {x: 1};
o === p // false: Çünki onlar yaddaşda iki fərqli obyektdir

let a = [], b = [];  // İki boş array
a === b // false: Çünki onlar fərqli array-lardır
```

#### Referans Tipi

Obyektlər dəyişənlərə dəyər olaraq deyil, **referans olaraq** mənimsədilir. Müqayisə zamanı **onların eyni obyektə istinad edib-etmədiyi** yoxlanılır.

```js
let a = [];  // Array yaradılır
let b = a;   // `b` dəyişəni `a` ilə eyni array-a istinad edir
b[0] = 1;    // `b` vasitəsilə array-ı dəyişirik
console.log(a[0]); // 1: `a` da dəyişdi, çünki `a` və `b` eyni obyektdir
a === b      // true: `a` və `b` eyni obyektə istinad edir
```

#### Yeni Kopya Yaratmaq (Copying Arrays)

Obyektin (və ya array-in) yeni kopyasını yaratmaq üçün onun bütün elementlərini bir-bir yeni obyektə köçürmək lazımdır.

```js
let a = ["a", "b", "c"];  // Orijinal array
let b = [];  // Yeni boş array yaradılır

// Array-i dövr edərək kopyalayırıq
for (let i = 0; i < a.length; i++) {
    b[i] = a[i]; // Elementi b-yə kopyalayırıq
}

let c = Array.from(b);  // ES6-da Array.from() ilə kopyalama
```

#### Array-ləri müqayisə etmək (Array Comparison)

İki array-in elementlərinin eyni olub-olmadığını yoxlamaq üçün aşağıdakı kimi bir funksiya yazmaq olar:

```js
function equalArrays(a, b) {
    if (a === b) return true; // Eyni array-lərdirsə, bərabərdir
    if (a.length !== b.length) return false; // Uzunluqları fərqlidirsə, bərabər deyil
    for (let i = 0; i < a.length; i++) {
        if (a[i] !== b[i]) return false; // Hər hansı bir element fərqlidirsə, bərabər deyil
    }
    return true; // Bütün yoxlamalardan keçdisə, deməli bərabərdir
}
```

#### Nəticə

- **Primitiv dəyərlər** dəyişdirilə bilməz və **dəyərlərinə görə** müqayisə edilir.
- **Obyektlər** dəyişdirilə bilər və **referanslarına görə** müqayisə edilir.

---

## 3.9 Tip Çevrilmələri (Type Conversions)

JavaScript-də tip çevrilməsi tez-tez baş verir. Məsələn, bəzi dəyərlər `truthy` və `falsy` olaraq `boolean` tipinə çevrilə bilər.

---

### Avtomatik Tip Çevrilmələri

```js
10 + " objects"   // "10 objects": ədəd `string`-ə çevrilir
"7" * "4"         // 28: hər iki string `number`-a çevrilir
let n = 1 - "x";  // NaN: "x" ədədə çevrilə bilmir
n + " objects"    // "NaN objects": NaN string-ə çevrilir
```

---

### Dəyərlərin Tipə Çevrilmə Cədvəli

| Dəyər | `String`-ə | `Number`-a | `Boolean`-a |
| :--- | :--- | :--- | :--- |
| `undefined` | `"undefined"` | `NaN` | `false` |
| `null` | `"null"` | `0` | `false` |
| `true` | `"true"` | `1` | `true` |
| `false` | `"false"` | `0` | `false` |
| `""` | `""` | `0` | `false` |
| `"1.2"` | `"1.2"` | `1.2` | `true` |
| `"one"` | `"one"` | `NaN` | `true` |
| `0` | `"0"` | `0` | `false` |
| `-0` | `"0"` | `-0` | `false` |
| `Infinity` | `"Infinity"` | `Infinity` | `true` |
| `NaN` | `"NaN"` | `NaN` | `false` |
| `1`, `-1` | `"1"`, `"-1"` | `1`, `-1` | `true` |
| `[]` | `""` | `0` | `true` |
| `[9]` | `"9"` | `9` | `true` |
| `['a']` | `"a"` | `NaN` | `true` |
| `{}` | `"[object Object]"`| `NaN` | `true` |
| `function(){}` | `"function..."` | `NaN` | `true` |

---

### 3.9.1 Bərabərlik və Tip Çevrilməsi

`==` operatoru müqayisə etməzdən əvvəl **tip çevrilməsi** edir, `===` isə etmir və həm dəyəri, həm də tipi yoxlayır.

```js
null == undefined    // true
"0" == 0             // true
0 == false           // true
"0" == false         // true
```

---

### 3.9.2 Açıq (Explicit) Çevrilmələr

Bəzən tipləri özümüzün birbaşa çevirməsi lazım gəlir.

```js
Number("3")        // 3
String(false)      // "false"
Boolean([])        // true

// Qısayollar
let x = "5";
x + ""             // String(x) ilə eyni
+x                 // Number(x) ilə eyni
!!x                // Boolean(x) ilə eyni
```

### Ədədləri fərqli say sistemlərinə çevirmək

```js
let n = 17;
n.toString(2);   // "10001" (ikilik)
n.toString(8);   // "21" (səkkizlik)
n.toString(16);  // "11" (onaltılıq)
```

### Say formatlama metodları

```js
let n = 123456.789;
n.toFixed(2)        // "123456.79" (kəsr hissəni 2 rəqəmə qədər saxlayır)
n.toExponential(3)  // "1.235e+5" (eksponensial formada göstərir)
n.toPrecision(7)    // "123456.8" (ümumi 7 rəqəm dəqiqliklə göstərir)
```

---

### `parseInt()` və `parseFloat()`

Bu funksiyalar string-in başlanğıcındakı rəqəmləri oxuyur və onları ədədə çevirir.

```js
parseInt("3 blind mice")    // 3
parseFloat(" 3.14 meters")  // 3.14
parseInt("-12.34")          // -12
parseInt("0xFF")            // 255 (onaltılıq)
parseInt("077", 8)          // 63 (səkkizlik)
parseInt("077", 10)         // 77 (onluq)
parseInt("11", 2)           // 3 (ikilik)
```

**Qeyd**: `parseInt()` ikinci arqument kimi **say sistemini** (radix) qəbul edə bilir.

---

### 3.9.3 Obyektdən Primitivə Çevrilmə

**Qeyd:** Bu hissə gələcəkdə əlavə olunacaq. (Prosesdə `valueOf()` və `toString()` metodları istifadə olunur.)

---

## 3.10 Dəyişənlərin Təyini və Qiymət Verilməsi

Proqramlaşdırmada ən əsas anlayışlardan biri adlardan (yəni **identifikatorlardan**) istifadə edərək dəyərləri yadda saxlamaqdır. Bir ada dəyər təyin etdikdə, bu ada **dəyişən** deyirik, çünki bu dəyər proqramın işləmə gedişində dəyişə bilər. Əgər bir ada sabit (dəyişməyən) dəyər təyin etsək, buna **sabit (constant)** deyirik.

---

### 3.10.1 `let` və `const` ilə Təyin

#### `let` ilə dəyişən təyini:

```js
let i;
let sum;
let i = 0, j = 0, k = 0;
let x = 2, y = x * x;
```

- **Yaxşı təcrübə:** Dəyişəni yaradanda ona ilkin dəyər vermək.
- Əgər `let` ilə dəyişən yaradılıbsa, amma ona dəyər verilməyibsə, ilkin dəyəri `undefined` olacaq.

#### `const` ilə sabit təyini:

```js
const H0 = 74; // Hubble sabiti
const C = 299792.458; // İşıq sürəti
const AU = 1.496E8; // Astronomik vahid (km)
```

- `const` ilə təyin olunan dəyərlər sonradan dəyişdirilə bilməz.

---

### Dəyişən və Sabitlərin Göstəriş Sahəsi (Scope)

- `let` və `const` **blok səviyyəli göstəriş sahəsinə** malikdir. Yəni, yalnız təyin edildiyi `{}` bloku daxilində keçərlidir.

---

### Təkrar Təyinlər və Səhvlər

- Eyni skop daxilində eyni dəyişəni `let` və ya `const` ilə birdən çox dəfə təyin etmək **sintaksis xətasıdır**:

```js
const x = 1;
if (x === 1) {
    let x = 2; // Bu, blok daxilində fərqli bir x dəyişənidir
    console.log(x); // 2
}
console.log(x); // 1
let x = 3; // Xəta! x artıq bu skopda təyin olunub
```

---

### Təyinlər və Tiplər

- C və Java kimi statik tipli dillərdən fərqli olaraq, JavaScript dinamik tiplidir, yəni dəyişənin tipi sonradan dəyişə bilər:

```js
let i = 10;
i = "ten"; // Tip dəyişir, bu normaldır
```

---

### 3.10.2 `var` ilə Dəyişən Təyini (köhnə üsul)

- **ES6-dan (2015-ci ildən) əvvəl** dəyişənlər yalnız `var` ilə təyin edilirdi:

```js
var x;
var data = [], count = data.length;
for(var i = 0; i < count; i++) console.log(data[i]);
```

#### `var` ilə `let` arasındakı əsas fərqlər:

1.  `var` **blok skopuna** deyil, **funksiya skopuna** malikdir.
2.  Qlobal skopda `var` ilə təyin olunan dəyişənlər qlobal obyektin (`window`) bir hissəsi olur.
3.  `var` ilə eyni adda dəyişəni təkrar təyin etmək mümkündür və bu, xəta vermir.
4.  `var` ilə təyin olunan dəyişənlər **hoisting** edilir – yəni, skopun yuxarısına "qaldırılır". Bu zaman dəyəri təyin olunana qədər `undefined` olur:

```js
console.log(x); // undefined (xəta vermir)
var x = 5;
```
`let` və `const` ilə bu cür istifadə isə xətaya səbəb olur. Müasir JavaScript-də `var` istifadə etmək tövsiyə edilmir.-