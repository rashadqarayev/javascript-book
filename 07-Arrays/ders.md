# Fəsil 7. Massivlər (Arrays) 📦

JavaScript-də **massivlər (arrays)** dəyərlərin **sıralanmış kolleksiyasıdır**. Onlar proqramlaşdırmanın əsas hissələrindən biridir.

---

### Massiv Nədir?

* **Sıralanmış Dəyərlər:** Massivdəki hər bir dəyər bir "element" adlanır.
* **İndeks (Sıra Nömrəsi):** Hər elementin massivdə bir rəqəmsal mövqeyi var, buna "indeks" deyilir.
* **Sıfırdan Başlama:** JavaScript massivlərində ilk elementin indeksi **0 (sıfır)**-dır. Yəni, `massiv[0]` ilk elementi verir.
* **Maksimum Ölçü:** İndekslər 32-bitdir, yəni massivin ən yüksək mümkün indeksi `4294967294` (`2^32 - 2`) ola bilər. Maksimum element sayı isə `4,294,967,295` olaraq düşünülür.

---

### JavaScript Massivlərinin Xüsusiyyətləri

1.  **Tip Yoxdur (Untyped):**
    * Bir massivin elementləri istənilən tipdə ola bilər (məsələn, həm ədəd, həm mətn, həm də obyekt).
    * Hətta eyni massivin fərqli elementləri fərqli tiplərdə ola bilər.
    * Massiv elementləri başqa obyektlər və ya massivlər də ola bilər ki, bu da mürəkkəb strukturlar (obyekt massivləri, massiv massivləri) yaratmağa imkan verir.

2.  **Dinamik Ölçü:**
    * JavaScript massivləri **dinamikdir**. Yəni, onları yaradarkən sabit bir ölçü təyin etmək lazım deyil.
    * Element əlavə etdikcə böyüyürlər, sildikcə kiçilirlər.

3.  **Seyrək Massivlər (Sparse Arrays):**
    * Massiv elementlərinin indeksi ardıcıl olmaya bilər. Yəni, massivin bəzi indekslərində dəyər olmaya bilər (bu "boşluq" yaradır).

4.  **`length` Xüsusiyyəti:**
    * Hər JavaScript massivinin bir **`length` (uzunluq)** xüsusiyyəti var.
    * Normal massivlər üçün `length` massivdəki elementlərin sayını göstərir.
    * Seyrək massivlər üçün `length` massivdəki ən yüksək indeksdən daha böyük ola bilər.

---

### Massivlər və Obyektlər — Əlaqə

* JavaScript massivləri əslində JavaScript obyektlərinin **xüsusi bir formasıdır**.
* Massiv indeksləri (0, 1, 2...) sadəcə tam ədədlərdən ibarət xüsusiyyət adları kimidir.
* Lakin, JavaScript tətbiqləri (brauzerlər, Node.js) massivləri optimallaşdırır, ona görə də rəqəmlə indekslənmiş elementlərə daxil olmaq adi obyekt xüsusiyyətlərinə daxil olmaqdan **xeyli sürətli** olur.

---

# 7.1 Massivləri Yaratmaq (Creating Arrays)

JavaScript-də massivləri yaratmaq üçün bir neçə üsul var. İndi onlara ardıcıllıqla baxaq:

* Massiv Literalları (Array Literals)
* `...` Spread Operatoru
* `Array()` Konstruktoru
* `Array.of()` və `Array.from()` Fabrik Metodları

---

## 7.1.1 Massiv Literalları (Array Literals)

Massiv yaratmağın ən sadə yolu **massiv literallarından** istifadə etməkdir. Bu, sadəcə kvadrat mötərizələr `[]` daxilində vergüllə ayrılmış elementlər siyahısıdır.

**Misallar:**

```javascript
let names = ["Alice", "Bob", "Charlie"];       
// String dəyərlərdən ibarət massiv
let temperatures = [21.5, 19.8, 25.0];         
// Ədədlərdən ibarət massiv
let flags = [true, false, true, false];        
// Boolean dəyərlərdən ibarət massiv

```

* Massiv literalındakı dəyərlər sabit olmaq məcburiyyətində deyil, istənilən **ifadə** (expression) ola bilər:

    ```javascript
    let base = 10;

    let computed = [base,base + 5,base * 2];
    console.log(computed); // [10, 15, 20]
    ```

* Massiv içində **massivlər** və **obyektlər**:

    ```javascript
    let data = [[1, {ad: "Ali"}], [2, {ad: "Veli"}]]; 
    // Massiv daxilində massiv və obyektlər
    ```

* **Seyrək Massivlər (Sparse Arrays):** Əgər massiv literalında iki vergül arasında dəyər yoxdursa, bu, həmin indeksdə elementin olmadığını göstərir və massiv seyrək olur. Belə elementləri soruşanda `undefined` qaytarılır.

    ```javascript
    let say = [1,,3];  
     // İndeks 0 (dəyər 1) və 2 (dəyər 3) var. İndeks 1-də element yoxdur.
    console.log(say.length); // => 3 (Uzunluq boşluqları da sayır)
    console.log(say[1]);     // => undefined

    let bosluqlar = [,,]; 
    // Heç bir elementi olmayan, amma uzunluğu 2 olan massiv
    console.log(bosluqlar.length); // => 2
    console.log(bosluqlar[0]);     // => undefined
    ```
    Qeyd: Massiv literal sintaksisi sonda əlavə bir vergülə icazə verir, ona görə `[,,]` massivinin uzunluğu 2-dir, 3 deyil.

---

## 7.1.2 Spread Operator (`...`)

ES6 (ECMAScript 2015) və sonrakı versiyalarda, bir massivi (və ya digər **iterasiya edilə bilən obyektləri**) başqa bir massiv literalının daxilinə daxil etmək üçün **"spread operatoru" (`...`)** istifadə olunur.

```javascript
let a = [1, 2, 3];
let b = [0, ...a, 4]; 
// 'a' massivinin elementləri 'b' massivinin içinə yayılır
console.log(b);       // => [0, 1, 2, 3, 4]
```
`...a` hissəsi sanki `1, 2, 3` kimi ədədlərin birbaşa oraya yazılması kimidir.

* **Massivin Kopyalanması (Shallow Copy):** Spread operatoru massivləri kopyalamaq üçün çox rahat bir yoldur:

    ```javascript
    let orijinal = [1, 2, 3];
    let kopya = [...orijinal]; 
    // orijinal massivinin kopyasını yaradır
    kopya[0] = 0;             
    // Kopyanı dəyişdirmək orijinalı dəyişdirmir

    console.log(orijinal[0]); // => 1
    console.log(kopya);       // => [0, 2, 3]
    ```

* **İterasiya Edilə Bilən Obyektlər (Iterable Objects):** Spread operatoru yalnız massivlər üzərində deyil, **iterasiya edilə bilən** (yəni `for/of` dövrü ilə gəzilə bilən) istənilən obyekt üzərində işləyir.

    * **Stringlər:** Stringlər iterasiya edilə bilən obyektlərdir, ona görə də onları spread operatoru ilə tək xarakterli string massivlərinə çevirə bilərsiniz:

        ```javascript
        let reqemler = [..."0123456789"];
        console.log(reqemler); 
        // => ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"]
        ```

    * **`Set` Obyektləri:** `Set` obyektləri təkrarlanan elementləri saxlamayan kolleksiyalardır. `Set` obyektləri də iterasiya edilə biləndir. Massivdən təkrarlanan elementləri çıxarmaq üçün massivi `Set`-ə çevirib, sonra `Set`-i spread operatoru ilə yenidən massivə çevirmək asan yoldur:

        ```javascript
        let herfler = [..."hello world"]; 
        // => ["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"]
        let tekrarsizHerfler = [...new Set(herfler)]; 
        // Təkrarlananları çıxarır
        console.log(tekrarsizHerfler); 
        // => ["h", "e", "l", "o", " ", "w", "r", "d"]
        ```

---

## 7.1.3 `Array()` Konstruktoru

Massiv yaratmağın başqa bir yolu `Array()` konstruktorundan istifadə etməkdir. Bu konstruktoru üç fərqli şəkildə çağırmaq olar:

1.  **Arqumentsiz Çağırmaq:**

    ```javascript
    let a = new Array();
    // Və ya daha qısa: let a = Array(); (new açar sözü olmadan da işləyir)
    console.log(a);         // => [] (Boş massiv yaradır)
    console.log(a.length);  // => 0
    ```
    Bu üsul, `[]` massiv literalı ilə boş massiv yaratmaqla eynidir.

2.  **Tək Sayda Rəqəmsal Arqumentlə Çağırmaq (Uzunluq Təyin Etmək):**

    ```javascript
    let a = new Array(10); // Uzunluğu 10 olan, boş elementlərlə massiv yaradır
    console.log(a);         // => [ <10 empty items> ]
    console.log(a.length);  // => 10
    console.log(a[0]);      // => undefined
    ```
    Bu şəkildə massiv yaradarkən, massivdə heç bir dəyər saxlanmır və indeks xüsusiyyətləri ("0", "1" və s.) hətta təyin olunmur. Bu, əvvəlcədən neçə element lazım olacağını bildiyiniz zaman massivə yer ayırmaq üçün istifadə edilə bilər.

3.  **İki və ya Daha Çox Arqumentlə, Yaxud Tək Qeyri-Rəqəmsal Arqumentlə Çağırmaq:**

    ```javascript
    let a = new Array(5, 4, 3, 2, 1, "sınaq, sınaq"); // Arqumentlər massiv elementləri olur
    console.log(a); // => [5, 4, 3, 2, 1, "sınaq, sınaq"]
    console.log(a.length); // => 6
    ```
    Bu formada, konstruktor arqumentləri yeni massivin elementlərinə çevrilir. Lakin, massiv literalı (`[]`) istifadə etmək, bu üsuldan demək olar ki, həmişə daha sadə və oxunaqlıdır.

---

## 7.1.4 `Array.of()` Metodu

`Array()` konstruktorunun tək rəqəmsal arqumentlə çağırılanda uzunluq təyin etməsi, amma birdən çox rəqəmsal arqumentlə çağırılanda elementlər kimi davranması bir çaşqınlıq yarada bilirdi.

**ES6** bu problemi **`Array.of()`** funksiyası ilə həll etdi. Bu, bir "fabrik metodu"dur (factory method). O, arqumentlərinin sayından asılı olmayaraq, həmin arqument dəyərlərini yeni massivin elementləri kimi istifadə edərək bir massiv yaradır və qaytarır:

```javascript
Array.of();       // => [] (Boş massiv)
Array.of(10);    
// => [10] (Tək rəqəmsal elementli massiv yaratmaq mümkündür!)
Array.of(1, 2, 3); // => [1, 2, 3]
```

---

## 7.1.5 `Array.from()` Metodu

`Array.from()` ES6-da təqdim olunan başqa bir massiv fabrik metodudur. Bu metod ilk arqument kimi **iterasiya edilə bilən obyekt** (məsələn, massiv, string, Set) və ya **massivəbənzər obyekt** gözləyir. O, bu obyektin elementlərini ehtiva edən yeni bir massiv qaytarır.

* **İterasiya edilə bilən obyektlərdən massiv yaratmaq:**
    `Array.from(iterable)` istifadəsi `[...iterable]` (spread operatoru) ilə eyni şəkildə işləyir. Bu, bir massivin kopyasını yaratmaq üçün də sadə bir yoldur:

    ```javascript
    let orijinal = [1, 2, 3];
    let kopya = Array.from(orijinal); 
    // orijinal massivinin kopyasını yaradır
    console.log(kopya); // => [1, 2, 3]

    let herfler = Array.from("Salam"); // Stringdən massiv yaratmaq
    console.log(herfler); // => ["S", "a", "l", "a", "m"]
    ```

* **İkinci Arqument (Map Funksiyası):**
    `Array.from()` həmçinin ixtiyari olaraq **ikinci bir arqument** qəbul edir: bir funksiya. Əgər bu funksiyanı ikinci arqument kimi versəniz, yeni massiv qurularkən mənbə obyektindən gələn hər bir element bu funksiyaya ötürüləcək və funksiyanın qaytardığı dəyər massivə orijinal dəyər əvəzinə əlavə olunacaq.
    (Bu, massivin `map()` metodu ilə çox bənzəyir, lakin massiv qurularkən çevirmə aparmaq, massivi qurub sonra başqa bir massivə çevirməkdən daha effektivdir.)

    ```javascript
    let reqemler = Array.from("123", Number); // Hər xarakteri rəqəmə çevirir
    console.log(reqemler); // => [1, 2, 3]

    let tekEdedlerKvadrat = Array.from([1, 2, 3, 4, 5], (num) => {
        if (num % 2 !== 0) return num * num; 
        // Tək ədədlərin kvadratını qaytarır
        return num; 
        // Cüt ədədləri olduğu kimi saxlayır
    });
    console.log(tekEdedlerKvadrat); // => [1, 2, 9, 4, 25]
    ```

---

# 7.2 Massiv Elementlərini Oxumaq və Yazmaq 

Massivin elementlərinə daxil olmaq və ya onların dəyərini dəyişdirmək üçün **kvadrat mötərizə operatoru (`[]`)** istifadə olunur.

* **Sintaksis:** Massivə istinad (`a` kimi bir dəyişən) kvadrat mötərizələrin solunda olmalı, mötərizələrin daxilində isə **sıfırdan böyük və ya sıfıra bərabər olan tam ədəd** dəyəri verən istənilən ifadə (yəni indeks) olmalıdır.
* Bu sintaksis həm massiv elementinin dəyərini **oxumaq**, həm də **yazmaq** üçün istifadə olunur.

**Misallar:**

```javascript
let a = ["dünya"]; 
// Bir elementli massiv ilə başlayırıq: a[0] = "dünya"

let deyer = a[0];     // Massivin 0-cı elementini oxuyuruq
console.log(deyer);   // => "dünya"

a[1] = 3.14;          // Massivin 1-ci elementinə dəyər yazırıq
console.log(a);       // => ["dünya", 3.14]

let i = 2;
a[i] = "üç";          // Massivin 2-ci elementinə dəyər yazırıq
console.log(a);       // => ["dünya", 3.14, "üç"]

a[i + 1] = "salam";   // Massivin 3-cü elementinə dəyər yazırıq (i+1 = 3)
console.log(a);       // => ["dünya", 3.14, "üç", "salam"]
```

---

### Massivlər və `length` Xüsusiyyəti (Avtomatik Yenilənmə) 

Massivlərin ən vacib xüsusiyyəti odur ki, siz `0` ilə `2^32 - 2` arasında **müsbət tam ədəd** olan xüsusiyyət adlarından (yəni, indekslərdən) istifadə etdiyiniz zaman, JavaScript massivin **`length` (uzunluq)** xüsusiyyətini avtomatik olaraq idarə edir.

Yuxarıdakı misalda, biz tək elementli (`a = ["dünya"]`) bir massiv yaratdıq. Sonra indeks 1, 2 və 3-ə dəyərlər təyin etdik. Bu zaman massivin `length` xüsusiyyəti də dəyişdi:

```javascript
let a = ["dünya"]; // length = 1
a[1] = 3.14;       // length = 2
a[2] = "üç";       // length = 3
a[3] = "salam";    // length = 4

console.log(a.length); // => 4
```

---

## Massiv İndeksləri və Xüsusiyyət Adları

JavaScript-də massivlər əslində **obyektdir**. Elementlərə indekslə daxil oluruq, amma bu indekslər **əslində string açarlardır**.

### 1. Massiv indeksləri necə işləyir?

Massivdə `a[1] = "salam"` kimi yazsanız, əslində bu belə işləyir:

```javascript
a["1"] = "salam";
```

Yəni **rəqəm avtomatik stringə çevrilir**. Amma **massivin `length` xüsusiyyəti yalnız düzgün indekslərə** reaksiya verir.

### Düzgün indeks nədir?

* Tam ədəd olmalıdır (məs. `0`, `1`, `42`)
* `0 ≤ indeks < 2^32 - 1`
* Əgər bu şərtlərə uyğundursa, `length` avtomatik artır

---

### 2. Misallar

```javascript
let arr = [];

arr[0] = "first";      // real indeks → arr.length = 1
arr["1"] = "second";   // real indeks → arr.length = 2
arr[-1] = "minus";     // property → arr.length dəyişmir
arr["foo"] = "bar";    // property → arr.length dəyişmir
arr[100] = "big";      // real indeks → arr.length = 101
arr[2.5] = "half";     // property → arr.length dəyişmir
```

###  Yoxlayaq:

```javascript
console.log(arr.length);   // 101
console.log(arr[-1]);      // "minus"
console.log(arr["foo"]);   // "bar"
console.log(arr[2.5]);     // "half"
```

---


### "Out of Bounds" (Sərhəddən Kənar) Xətası Yoxdur

JavaScript-də massivlərdə **sərhəddən kənar indeksə daxil olduqda xəta atmır**. Sadəcə `undefined` qaytarılır.

### Səbəb:

Massivlər əslində obyekt olduğuna görə, **olmayan indekslərə** baxmaq **obyektdə olmayan xüsusiyyətə baxmaq** kimidir.


```javascript
let arr = [true, false];

console.log(arr[0]);   // true
console.log(arr[1]);   // false
console.log(arr[2]);   // undefined (element yoxdur)
console.log(arr[100]); // undefined (element yoxdur)
console.log(arr[-1]);  // undefined (kənar xüsusiyyət, indeks deyil)
```

---

# 7.3 Seyrək Massivlər (Sparse Arrays) 🍂

**Seyrək massiv** elə bir massivdir ki, onun elementləri 0-dan başlayaraq ardıcıl indekslərə malik deyil. Yəni, bəzi indekslərdə elementlər yoxdur, boşluqlar var.

* **`length` Xüsusiyyəti:** Normalda, massivin `length` xüsusiyyəti elementlərin sayını göstərir. Amma seyrək massivdə `length` dəyəri, massivdəki elementlərin sayından **daha böyük** olur.

* **Seyrək Massiv Necə Yaradılır?**
    * `Array()` konstruktoru ilə:

        ```javascript
        let a = new Array(5); 
        // Heç bir element yoxdur, amma a.length 5-dir.
        console.log(a);         // => [ <5 empty items> ]
        console.log(a.length);  // => 5
        ```
    * Mövcud massivin cari uzunluğundan daha böyük bir indeksə dəyər təyin etməklə:

        ```javascript
        let a = [];         
        // Boş massiv, uzunluğu 0.
        a[1000] = 0;        
        // Bir element əlavə edir, amma massivin uzunluğunu 1001-ə qədər artırır.
        console.log(a);     // => [ <1000 empty items>, 0 ]
        console.log(a.length); // => 1001
        ```
* **Massiv Literallarında Seyrəklik:**
    Massiv literalında vergüllər arasında dəyər buraxdığınız zaman (məsələn, `[1,,3]` kimi), yaranan massiv seyrək olur və buraxılan elementlər sadəcə mövcud olmur:

    ```javascript
  let a = [1, , 3];
  console.log(1 in a); // false — 1-ci indeksdə element yoxdur
  console.log(a[1]);   // undefined — çünki element mövcud deyil
  ```

   Amma bu isə fərqlidir:

    ```javascript
     let a = [1, undefined, 3];
     console.log(1 in a); 
     // true — element var, sadəcə dəyəri undefined-dir
    ```

---

# 7.4 Massivin Uzunluğu (Array Length)

Hər massivin bir **`length` (uzunluq)** xüsusiyyəti var və məhz bu xüsusiyyət massivləri adi JavaScript obyektlərindən fərqləndirir.

* **Sıx (Dense) Massivlər üçün `length`:**
    Sıx (seyrək olmayan) massivlər üçün `length` xüsusiyyəti massivdəki **elementlərin sayını** göstərir. Onun dəyəri massivdəki ən yüksək indeksdən bir vahid böyükdür.

    ```javascript
    [].length;              // => 0: massivdə element yoxdur
    ["a", "b", "c"].length; // => 3: ən yüksək indeks 2-dir (0, 1, 2), uzunluq 3-dür.
    ```

* **Seyrək Massivlər üçün `length`:**
    Seyrək massivdə `length` xüsusiyyəti elementlərin sayından **daha böyükdür**. Bu halda, deyə biləcəyimiz tək şey budur ki, `length` massivdəki **hər bir elementin indeksindən böyükdür**. Yaxud başqa sözlə, bir massivin (seyrək və ya yox) indeksi `length`-dən böyük və ya ona bərabər olan bir elementi heç vaxt olmaz.

* **`length` Dəyişməzliyini Qorumaq (Invariant):**
    Bu qaydanı qorumaq üçün massivlərdə iki xüsusi davranış var:

    1.  **İndeksin `length`-i Artırması:**
        Əgər massivə cari `length` dəyərindən böyük və ya ona bərabər olan bir indeksə (məsələn, `a[i]`) dəyər təyin etsəniz, `length` xüsusiyyətinin dəyəri avtomatik olaraq `i + 1`-ə dəyişir.

        ```javascript
        let arr = [1, 2]; // arr.length 2-dir
        arr[5] = 10;      // İndeks 5 cari uzunluqdan (2) böyükdür.
        console.log(arr.length); // => 6 (5 + 1)
        console.log(arr);        // => [1, 2, <3 empty items>, 10]
        ```

    2.  **`length` Dəyərini Azaltmaqla Elementləri Silmək:**
        Əgər `length` xüsusiyyətinin dəyərini cari dəyərindən kiçik, lakin mənfi olmayan bir tam ədədə `n` təyin etsəniz, indeksi `n`-dən böyük və ya ona bərabər olan **bütün massiv elementləri massivdən silinir**.

        ```javascript
        let a = [1, 2, 3, 4, 5]; // Başlanğıcda 5 elementli massiv.
        a.length = 3;           
        // length-i 3-ə qoyduq. İndeksi 3 və 4 olan elementlər silindi.
        console.log(a);         // => [1, 2, 3]

        a.length = 0;           // length-i 0-a qoyduq. Bütün elementlər silindi.
        console.log(a);         // => [] (Boş massiv)

        a.length = 5;           // length-i 5-ə qoyduq.
        console.log(a);         
        // => [ <5 empty items> ] (Uzunluq 5-dir, amma element yoxdur,
        // sanki 'new Array(5)' ilə yaradılıb)
        ```
---

# 7.5 Massiv Elementlərini Əlavə Etmək və Silmək

Massivlərə element əlavə etmək və ya silmək üçün bir neçə üsul var.

### Element Əlavə Etmək (Adding Elements)

1.  **Yeni İndeksə Dəyər Təyin Etməklə (Ən Sadə Yol):**
    Massivə element əlavə etməyin ən sadə yolu sadəcə mövcud uzunluğundan daha böyük bir indeksə dəyər təyin etməkdir:

    ```javascript
    let fruits = [];           // Yeni boş massiv yaradırıq
    fruits[2] = "banana";      
    // 2-ci indeksə dəyər əlavə edirik (0 və 1 boş qalır)
    fruits[0] = "apple";       // 0-cı indeksə dəyər əlavə edirik
    fruits[1] = "orange";      // 1-ci indeksə dəyər əlavə edirik
    console.log(fruits);       // => ["apple", "orange", "banana"]
    ```

2.  **`push()` Metodu ilə (Sona Əlavə Etmək):**
    `push()` metodu massivin sonuna bir və ya daha çox dəyər əlavə edir. Bu metod massivin yeni uzunluğunu qaytarır.

    ```javascript
    let a = [];                       // Yeni boş massiv yaradırıq
    a.push("zero");                  
    // Massivin sonuna "zero" əlavə olunur
    console.log(a);                  // => ["zero"]
    a.push("one", "two");            
    // İki yeni element əlavə olunur: "one" və "two"
    console.log(a);                  // => ["zero", "one", "two"]
    ```
3.  **`unshift()` Metodu ilə (Əvvələ Əlavə Etmək):**
    `unshift()` metodu massivin əvvəlinə bir dəyər əlavə edir, mövcud elementləri isə daha yüksək indekslərə sürüşdürür. Bu metod haqqında daha ətraflı §7.8-də danışacağıq.
    ```javascript
    let arr = [2, 3];
    arr.unshift(1);  
    console.log(arr); // => [1, 2, 3]
    ```

## Element Silmək (Deleting Elements)

### 1. `delete` Operatoru ilə

`delete` operatoru massivdə elementin özünü silir, amma massivdə boşluq (hole) yaranır və uzunluq dəyişmir.

```javascript
let a = [1, 2, 3];
delete a[2];                // 2-ci indeksdəki elementi silir
console.log(a);            // => [1, 2, <1 empty item>]
console.log(2 in a);       // => false (element yoxdur)
console.log(a.length);     // => 3 (uzunluq dəyişmir)
```

---

### 2. `pop()` Metodu ilə (Sondan Silmək)

`pop()` metodu massivin son elementini silir və onu qaytarır. Uzunluğu 1 azaldır.

```javascript
let a = [1, 2, 3];
let last = a.pop();         // Son elementi silir və qaytarır
console.log(last);          // => 3
console.log(a);             // => [1, 2]
console.log(a.length);      // => 2
```

---

### 3. `shift()` Metodu ilə (Əvvəldən Silmək)

`shift()` metodu massivin ilk elementini silir, onu qaytarır və digər elementləri bir indeks aşağı sürüşdürür.

```javascript
let a = [1, 2, 3];
let first = a.shift();      // İlk elementi silir və qaytarır
console.log(first);         // => 1
console.log(a);             // => [2, 3]
console.log(a.length);      // => 2
```

---

### 4. `length` Xüsusiyyətini Dəyişdirməklə

`length`-i azaldaraq massivdən son elementləri asanlıqla silə bilərsiniz.

```javascript
let a = [1, 2, 3, 4, 5];
a.length = 3;               
// Massivin uzunluğunu 3-ə endirir, sonrakı elementlər silinir
console.log(a);             // => [1, 2, 3]
```

---

### 5. `splice()` Metodu ilə (Ümumi Məqsədli Silmə/Əlavə Etmə)

`splice()` ilə istənilən yerdən elementləri silmək və ya əlavə etmək mümkündür.

```javascript
let a = [1, 2, 3, 4, 5];
let removed = a.splice(1, 2); 
// İndeks 1-dən başlayaraq 2 element silir
console.log(removed);          // => [2, 3]
console.log(a);                // => [1, 4, 5]
```

Bu methodların daha çox nümunə ilə izahı növbəti bölmədə olacaqdır.

---

## 7.6 Massivlərdə Dövr Etmək (Iterating Arrays)

Massivin hər bir elementi üzərində dövr etmək üçün bir neçə üsul var. Bu üsullar, işinizin tələbinə uyğun olaraq müxtəlif ssenarilərdə faydalı ola bilər.

### 1\. `for/of` Dövrü (ES6+)

ES6-dan etibarən, massivlərin (və ya istənilən **iterasiya edilə bilən obyektin**) elementləri arasında dövr etməyin ən asan və ən oxunaqlı yolu **`for/of`** dövrüdür. Bu, daha əvvəl §5.4.4-də ətraflı izah edilmişdi. O, birbaşa elementlərin üzərindən keçir və kodu daha təmiz edir.

```javascript
let fruits = ["apple", "pear", "pomegranate", "quince"]; 
// Meyvələr massivi yaradırıq
console.log("My favorite fruits:");
for (let fruit of fruits) { 
  // Hər meyvəni tək-tək gəzirik
  console.log(fruit);
}
// Nəticə:
// My favorite fruits:
// apple
// pear
// pomegranate
// quince
```

-----

### 2\. `for/of` ilə İndeks və Dəyəri Birlikdə Əldə Etmək (`entries()`)

Əgər `for/of` dövrü istifadə edərkən həm elementin özünə, həm də onun **indeksinə** ehtiyacınız varsa, massivin **`entries()`** metodundan və **destructuring assignment**-dən (strukturu pozaraq mənimsətmə) istifadə edin. Bu üsul, elementlərin sıra nömrəsi ilə birlikdə işləməli olduğunuz hallar üçün idealdır.

```javascript
let cities = ["Baku", "Ganja", "Sumgayit", "Lankaran"];
console.log("Major cities of Azerbaijan:");
for (let [index, city] of cities.entries()) { 
  // Hər indeks və şəhəri birlikdə alırıq
  console.log(`${index + 1}. ${city}`); 
  // Sıra nömrəsi ilə çap edirik
}
// Nəticə:
// Major cities of Azerbaijan:
// 1. Baku
// 2. Ganja
// 3. Sumgayit
// 4. Lankaran
```

-----

### 3\. `forEach()` Metodu (Funksional Yanaşma)

`forEach()` massivləri iterasiya etmək üçün funksional bir yanaşma təklif edən bir massiv metodudur, bu, `for` dövrünün yeni bir forması deyil. Massivin `forEach()` metoduna bir funksiya ötürürsünüz və `forEach()` massivin hər bir elementi üzərində sizin funksiyanızı bir dəfə çağırır. Bu metod, hər element üzərində müəyyən bir əməliyyat aparmaq istədiyiniz zaman çox faydalıdır.

```javascript
let students = ["Ayşe", "Elvin", "Leyla", "Murad"];
let greetings = [];

students.forEach(student => { 
  // Hər tələbə üçün salamlama mətni hazırlayırıq
  greetings.push(`Hello, ${student}!`);
});
console.log(greetings);
// Nəticə:
// [ 'Hello, Ayşe!', 'Hello, Elvin!', 'Hello, Leyla!', 'Hello, Murad!' ]
```

-----

### 4\. Ənənəvi `for` Dövrü (Old School)

Massiv elementləri arasında dövr etmək üçün köhnə, lakin etibarlı `for` dövründən də istifadə edə bilərsiniz (§5.4.3). Bu dövr növü massivin **indeksləri** üzərində işləyir və sizə hər bir elementə indeks vasitəsilə müraciət etməyə imkan verir. Nəzarətin tamamilə sizin əlinizdə olduğu ssenarilərdə, məsələn, massivin bir hissəsini tərsinə gəzmək və ya yalnız müəyyən şərtləri ödəyən elementlərə baxmaq lazım gəldikdə, bu dövr növü hələ də faydalıdır.

```javascript
let scores = [12, 5, 20, 8, 15]; // Tələbə qiymətləri
let passingScores = [];
for (let i = 0; i < scores.length; i++) { 
  // Massivdəki hər indeks üçün
  let score = scores[i];                  
  // Həmin indeksdəki elementi alırıq
  if (score >= 10) {                         
    // Əgər qiymət keçid balından yuxarıdırsa
    passingScores.push(score);              
    // Onu keçən qiymətlər massivinə əlavə edirik
  }
}
console.log("Passing scores:", passingScores); 
// => Passing scores: [ 12, 20, 15 ]
```

-----

Tamamilə haqlısan\! Bu nümunə həqiqətən də bir az uzundur və yeni başlayan üçün birbaşa "çoxölçülü massiv" konsepsiyasını anlamaq üçün ən sadə yanaşma deyil. Vurma cədvəli əla bir tətbiq olsa da, massivin necə yaradıldığını və doldurulduğunu izah edən kod hissəsi diqqəti əsas mövzudan yayındıra bilər.

Məqsəd, "massivlərin massivləri" ideyasını sadə və dərhal anlaşıqlı bir şəkildə göstərməkdir. Bu hissəni aşağıdakı kimi yenidən quraq. Əsas diqqəti qısa və məqsədəuyğun nümunələrə yönəldəcəm:

-----

## 7.7 Çoxölçülü Massivlər (Multidimensional Arrays)

JavaScript birbaşa olaraq "həqiqi çoxölçülü massivləri" dəstəkləmir. Lakin siz **massivlərin massivlərini** (yəni bir massivin elementləri başqa massivlər olan) yaratmaqla bunu təxmin edə bilərsiniz. Bu üsulla, iki, üç və ya daha çox ölçülü massivlər kimi davranan strukturlar qura bilərsiniz.

Təsəvvür edin ki, bir cədvəl və ya matris kimi məlumat saxlamaq istəyirsiniz. Hər sıra özü bir massiv, bu sıralar isə əsas massivin elementləri olacaq.

  * **Dəyərə Daxil Olmaq:** Bir massivlər massivindəki bir dəyərə daxil olmaq üçün sadəcə **`[]` operatorunu ardıcıl olaraq iki dəfə** istifadə edirsiniz. Məsələn, əgər `matrix` dəyişəni ədədlər massivlərinin massividirsə, `matrix[x]` bu massivdəki hər hansı bir sıranı (özü də bir massiv olan) təmsil edir. Bu sıradakı müəyyən bir ədədə daxil olmaq üçün `matrix[x][y]` yazarsınız.

-----

### Sadə Nümunə: Şəhər və Ölkə Məlumatları 

Gəlin, iki ölçülü bir massivi şəhər və onun aid olduğu ölkə məlumatlarını saxlamaq üçün necə istifadə edəcəyimizə baxaq. Bu, çoxölçülü massiv anlayışını sadə şəkildə göstərir.

```javascript
// İki ölçülü massiv yaradırıq: hər daxili massiv [ölkə, paytaxt] formatındadır
let countriesAndCapitals = [
  ["Azerbaijan", "Baku"],
  ["Turkey", "Ankara"],
  ["Germany", "Berlin"]
];

console.log(countriesAndCapitals[0]);     // => [ 'Azerbaijan', 'Baku' ] (Birinci sıra)
console.log(countriesAndCapitals[0][0]);  // => Azerbaijan (Birinci sıranın birinci elementi - ölkə)
console.log(countriesAndCapitals[1][1]);  // => Ankara (İkinci sıranın ikinci elementi - paytaxt)

// Yeni məlumat əlavə etmək
countriesAndCapitals.push(["France", "Paris"]);
console.log(countriesAndCapitals[3][1]);  // => Paris
```

-----

### Nümunə: Sadə Matris Əməliyyatları (Oyun Taxtası kimi) 🎮

Çoxölçülü massivlər tez-tez oyunlarda və ya cədvəl tipli məlumat strukturlarında istifadə olunur. Məsələn, bir oyun taxtasını təmsil edə bilər:

```javascript
// 3x3 ölçülü bir oyun taxtası yaradırıq (məsələn, xaç-sıfır oyunu üçün)
// Boş xanaları "" ilə işarələyirik
let gameBoard = [
  ["X", "", "O"],
  ["", "O", ""],
  ["X", "X", ""]
];

console.log(gameBoard[0][0]); // => X (Yuxarı sol küncdəki dəyər)
console.log(gameBoard[1][1]); // => O (Mərkəzdəki dəyər)

// Bir xananın dəyərini dəyişmək
gameBoard[2][2] = "O"; // Aşağı sağ küncə 'O' yerləşdiririk
console.log(gameBoard);
/*
Nəticə:
[
  [ 'X', '', 'O' ],
  [ '', 'O', '' ],
  [ 'X', 'X', 'O' ]
]
*/

```

-----

# 7.8 Massiv Metodları

Əvvəlki hissələr massivlərlə işləmək üçün əsas JavaScript sintaksisinə fokuslanırdı. Ümumiyyətlə, ən güclü olanlar **`Array` sinfi tərəfindən təyin olunan metodlardır**. Bu metodları oxuyarkən, bəzilərinin çağırıldığı massivi dəyişdirdiyini (modify), bəzilərinin isə dəyişdirmədiyini yadda saxlamaq vacibdir.


---

## 7.8.1 Massiv İterator Metodları

Bu hissədəki metodlar, massiv elementlərini ardıcıl olaraq sizin təqdim etdiyiniz bir funksiyaya ötürərək massivlər üzərində effektiv şəkildə dövr etməyə imkan verir. Onlar massivləri gəzmək, **xəritələmək (`map`)**, **filtrləmək (`filter`)**, **test etmək (`test`)** və **yığmaq (`reduce`)** üçün güclü vasitələrdir.

**Ümumi Xüsusiyyətlər:**

* Bu metodlar ilk arqument olaraq bir **funksiya** qəbul edir və massivin hər bir elementi üçün bu funksiyanı çağırır.
* Seyrək massivlərdə (sparse arrays) **mövcud olmayan elementlər üçün funksiya çağırılmır**.
* Sizin funksiyanız adətən üç arqumentlə çağırılır: **elementin dəyəri**, **indeksi** və **massivin özü**. Çox vaxt sizə yalnız ilk arqument (dəyər) lazım olur.
* Əksər hallarda, bu metodlar çağırıldığı massivi **dəyişdirmir**; onlar ya yeni bir massiv qaytarır, ya da müəyyən bir nəticə verir.
* Bu funksiyalar tez-tez metod çağırışının bir hissəsi olaraq **yerində (inline)** təyin olunur. **Arrow function** sintaksisi (§8.1.3) bu metodlarla işləmək üçün ideal seçimdir və biz nümunələrdə ondan istifadə edəcəyik.

-----

### `forEach()` Metodu

`forEach()` metodu massiv üzərində dövr edir və massivin hər bir elementi üçün sizin təyin etdiyiniz funksiyanı çağırır. Bu funksiya çağırılarkən avtomatik olaraq üç arqument alır: **elementin dəyəri**, **indeksi** və **massivin özü**. Sizə lazım olan arqumentləri funksiyanızda istifadə edə bilərsiniz. Məsələn, əgər yalnız elementin dəyəri lazımdırsa, funksiyanızı bir parametrli yaza bilərsiniz.

```javascript
let numbers = [1, 2, 3, 4, 5];
let sum = 0;

// Massiv elementlərinin cəmini hesablayırıq
// Burada yalnız `number` (elementin dəyəri) arqumentindən istifadə edirik
numbers.forEach(number => {
  sum += number;
});
console.log(sum); // => 15

let students = ["Ali", "Sara", "Emin", "Aygun"];

// Nümunə 1: Hər tələbənin adını çap edirik
// Yalnız `student` (elementin dəyəri) arqumentindən istifadə edirik
console.log("Students List:");
students.forEach(student => {
  console.log(student);
});
/*
Nəticə:
Students List:
Ali
Sara
Emin
Aygun
*/
```

-----

**Vacib Qeyd:** `forEach()` dövrü `for` dövründəki `break` və ya `continue` kimi ifadələrlə **dayandırıla bilməz** və ya **ötürülə bilməz**. Yəni, bir dəfə başladıqdan sonra, massivin bütün elementləri üçün funksiyanız çağırılacaq. Əgər iterasiyanı erkən dayandırmaq lazımdırsa, `for` dövründən və ya `some()`, `every()` kimi digər massiv metodlarından istifadə etməlisiniz.

-----

### `map()` Metodu

`map()` metodu çağırıldığı massivin hər bir elementini sizin təyin etdiyiniz funksiyaya ötürür və **həmin funksiyanın qaytardığı dəyərləri ehtiva edən yeni bir massiv** qaytarır.

```javascript
let a = [1, 2, 3];
let kvadratlar = a.map(x => x * x); // Hər x-i götürüb x*x qaytarır
console.log(kvadratlar);          // => [1, 4, 9]
console.log(a);                   // => [1, 2, 3] (Original massiv dəyişməyib!)
```
`map()` metoduna ötürdüyünüz funksiya `forEach()`-də olduğu kimi çağırılır, lakin o, mütləq bir dəyər qaytarmalıdır. `map()` **həmişə yeni bir massiv qaytarır** və çağırıldığı massivi dəyişdirmir.

---

### `filter()` Metodu 

`filter()` metodu çağırıldığı massivin elementlərindən şərtə uyğun gələnləri seçərək yeni bir massiv qaytarır. Bu metoda ötürdüyünüz funksiya bir **predikat** olmalıdır: yəni, `true` (doğru) və ya `false` (yalan) qaytaran bir funksiya. Predikat hər element üçün çağırılır. Əgər predikat `true` qaytararsa, həmin element yeni massivə əlavə olunur.

```javascript
let ages = [18, 12, 25, 6, 30, 15];

// Nümunə 1: Yalnız 18 və daha yuxarı yaşda olanları seçirik
let adults = ages.filter(age => age >= 18);
console.log(adults); // => [18, 25, 30]

// Nümunə 2: İndeksi cüt olan və yaşı 20-dən böyük olanları seçirik
let selectedPeople = ages.filter((age, index) => index % 2 === 0 && age > 20);
console.log(selectedPeople); // => [25, 30] (ages[2]=25, ages[4]=30)
```

-----

**Vacib Qeyd:** `filter()` metodu seyrək massivlərdəki boş elementləri (`empty items`) ötürür və onun qaytardığı massiv **həmişə sıx (dense)** olur. Bu o deməkdir ki, yeni massivdə heç bir "boş yer" olmayacaq.

  * **Seyrək massivdəki boşluqları aradan qaldırmaq üçün:**

    ```javascript
    let sparseArray = [1, , 3, , 5]; // İkinci və dördüncü elementlər boşdur
    let denseArray = sparseArray.filter(() => true); // Bütün mövcud elementləri götürür
    console.log(denseArray); // => [1, 3, 5]
    ```

  * **Boşluqları, `undefined` və `null` elementləri silmək üçün:**

    ```javascript
    let mixedArray = [1, undefined, 2, null, 3, , 4];
    // `x` dəyəri `undefined` və ya `null` deyilsə, onu seç
    let cleanedArray = mixedArray.filter(x => x !== undefined && x !== null);
    console.log(cleanedArray); // => [1, 2, 3, 4]
    ```

-----

### `find()` və `findIndex()` Metodları

`findIndex()` və `find()` metodları massivdə müəyyən bir şərti ödəyən elementi axtarmaq üçün istifadə olunur. Hər iki metod da `filter()` kimi bir **predikat funksiya** qəbul edir. Predikat `true` qaytaran ilk element tapılan kimi axtarışı dayandırırlar.

  * **`findIndex()`:** Şərti ödəyən **ilk elementin indeksini** qaytarır. Əgər belə bir element tapılmazsa, `-1` qaytarır.
  * **`find()`:** Şərti ödəyən **ilk elementin dəyərini** qaytarır. Əgər belə bir element tapılmazsa, `undefined` qaytarır.

```javascript
let students = [
  { name: "Ali", grade: 88, passed: true },
  { name: "Leyla", grade: 95, passed: true },
  { name: "Murad", grade: 45, passed: false },
  { name: "Nigar", grade: 78, passed: true }
];

// 🔍 1. İlk "keçməyən" tələbəni tapırıq
let failedStudent = students.find(student => student.passed === false);
console.log("Failed student:", failedStudent);
// => Failed student: { name: 'Murad', grade: 45, passed: false }

// 🔢 2. Balı 90-dan yuxarı olan ilk tələbənin indeksini tapırıq
let topStudentIndex = students.findIndex(student => student.grade > 90);
console.log("Index of top student:", topStudentIndex);
// => Index of top student: 1 (Leyla)

// ❌ 3. Adı "Rashad" olan tələbəni tapmağa çalışırıq (yoxdur)
let rashad = students.find(student => student.name === "Rashad");
console.log("Rashad:", rashad);
// => Rashad: undefined

// ❓ 4. Balı 50-dən aşağı olan ilk tələbənin indeksini tapırıq
let lowGradeIndex = students.findIndex(student => student.grade < 50);
console.log("Index of low grade student:", lowGradeIndex);
// => Index of low grade student: 2 (Murad)
```
---

### `every()` və `some()` Metodları

`every()` və `some()` metodları massivlərdə şərtləri yoxlamaq üçün istifadə olunan güclü predikat metodlardır. Onlar sizin təyin etdiyiniz funksiyanı massiv elementlərinə tətbiq edir və nəticədə `true` (doğru) və ya `false` (yalan) qaytarırlar.

-----

#### `every()` Metodu (Hər kəs üçün)

Bu metod riyaziyyatdakı "hər şey üçün" (`∀`) kəmiyyətçisinə bənzəyir. **Yalnız və yalnız** massivdəki **bütün elementlər** üçün sizin predikat funksiyanız `true` qaytararsa, `every()` metodu `true` qaytarır. Əgər bircə element belə şərti ödəmirsə, dərhal `false` qaytarır.

```javascript
let studentScores = [85, 92, 78, 95, 88]; // Tələbə balları

// Nümunə 1: Bütün tələbələr keçid balını (70) keçibmi?
let allPassed = studentScores.every(score => score >= 70);
console.log("Did all students pass the exam?", allPassed); 
// => Did all students pass the exam? true

// Nümunə 2: Bütün məhsulların stokda olması şərti
let productsInStock = [
  { name: "Laptop", inStock: true },
  { name: "Mouse", inStock: true },
  { name: "Keyboard", inStock: false }
];
let areAllProductsAvailable = productsInStock.every(product => product.inStock === true);
console.log("Are all products available?", areAllProductsAvailable); // => Are all products available? false
```

-----

#### `some()` Metodu (Bəziləri üçün) ❔

Bu metod riyaziyyatdakı "mövcuddur" (`∃`) kəmiyyətçisinə bənzəyir. Əgər massivdə predikatın `true` qaytardığı **ən azı bir element** mövcuddursa, `some()` metodu `true` qaytarır. Yalnız və yalnız predikat massivin bütün elementləri üçün `false` qaytararsa, `false` qaytarır.

```javascript
let userStatuses = ["active", "inactive", "pending", "active"]; 
// İstifadəçi statusları

// Nümunə 1: Aktiv istifadəçi varmı?
let isAnyUserActive = userStatuses.some(status => status === "active");
console.log("Is there any active user?", isAnyUserActive); 
// => Is there any active user? true

// Nümunə 2: Massivdə mənfi ədəd varmı?
let temperatureReadings = [15, 10, -5, 20, 0];
let hasNegativeTemperature = temperatureReadings.some(temp => temp < 0);
console.log("Is there any negative temperature reading?", hasNegativeTemperature); 
// => Is there any negative temperature reading? true

// Nümunə 3: Massivdə "error" statusu varmı?
let systemLogs = ["info", "warning", "debug"];
let hasError = systemLogs.some(log => log === "error");
console.log("Is there any 'error' in logs?", hasError); 
// => Is there any 'error' in logs? false
```

-----

**Dövrü Dayandırma Xüsusiyyəti:** Həm `every()`, həm də `some()` qaytaracaqları dəyəri bildikləri anda massiv elementləri üzərində dövr etməyi dayandırırlar.

  * **`some()`** sizin predikatınız ilk dəfə `true` qaytaran kimi dərhal `true` qaytarır və axtarışı dayandırır. Yalnız predikatınız həmişə `false` qaytararsa, bütün massivi gəzir.
  * **`every()`** isə bunun əksidir: sizin predikatınız ilk dəfə `false` qaytaran kimi dərhal `false` qaytarır və axtarışı dayandırır. Yalnız predikatınız həmişə `true` qaytararsa, bütün elementləri gəzir.

-----

**Boş Massivlər:** Riyazi konvensiyaya görə, `every()` boş massivdə çağırılanda `true` (çünki `false` qaytaran heç bir element yoxdur), `some()` isə `false` qaytarır (çünki `true` qaytaran heç bir element yoxdur).

```javascript
let emptyArray = [];
console.log("every() on empty array:", emptyArray.every(x => x > 0)); // => true
console.log("some() on empty array:", emptyArray.some(x => x > 0));   // => false
```

-----


Bu bölmə artıq çox əhatəlidir və yaxşı yazılıb! Amma istədiyin kimi daha *kitab üslubuna uyğun*, **nümunələrlə zəngin** və **qısa anlayış blokları** şəklində təqdim edə bilərəm. Gəlin əvvəlcə analiz edib sonra son versiyanı təqdim edim:

---

## `reduce()` və `reduceRight()` Metodları
`reduce()` metodu massivin bütün elementlərini bir funksiya vasitəsilə **tək bir nəticəyə** çevirir.
Əməliyyat **soldan-sağa** aparılır. Əgər sağdan başlamaq istəyirsənsə, `reduceRight()` istifadə olunur.

---

### 🧠 Sintaksis

```javascript
array.reduce((accumulator, currentValue, index, array) => { /* ... */ }, initialValue);
```

* **`accumulator`** – yığılmış nəticə.
* **`currentValue`** – cari massiv elementi.
* **`initialValue`** – ilkin başlanğıc dəyəri. (Əgər göstərilməzsə, massivdəki ilk element götürülür.)

---

###  Nümunələr

```javascript
let nums = [1, 2, 3, 4];

let sum = nums.reduce((acc, val) => acc + val, 0);
console.log(sum); // => 10

let product = nums.reduce((acc, val) => acc * val, 1);
console.log(product); // => 24

let words = ["JS", "is", "fun"];

let sentence = words.reduce((acc, val) => acc + " " + val);
console.log(sentence); // => "JS is fun"

let cart = [
  { product: "Laptop", price: 1200 },
  { product: "Book", price: 30 },
  { product: "Pen", price: 5 }
];

let total = cart.reduce((sum, item) => sum + item.price, 0);
console.log(total); // => 1235

let entries = [["name", "Ali"], ["age", 25]];

let obj = entries.reduce((acc, [key, val]) => {
  acc[key] = val;
  return acc;
}, {});

console.log(obj); // => { name: "Ali", age: 25 }
```

---

###  `reduceRight()` — Eyni funksionallıq, amma sağdan başlayır

```javascript
let letters = ["a", "b", "c", "d"];

// Sağdan-sola birləşdirərək stringi tərsinə çeviririk
let reversed = letters.reduceRight((acc, char) => acc + char, "");

console.log(reversed); // => "dcba"
```

---

### Diqqət: Boş Massivdə İlkin Dəyər Vacibdir

```javascript
[].reduce((a, b) => a + b);
// ❌ TypeError: Reduce of empty array with no initial value
```
---

## 7.8.2 Massivləri Düzləndirmək: `flat()` və `flatMap()` Metodları

Bəzən massiv daxilində başqa massivlər ola bilər — bu cür iç-içə strukturlar kodun işlənməsini çətinləşdirə bilər. Bu hallarda **`flat()`** və **`flatMap()`** metodları iç-içə massivləri daha **sadə və oxunaqlı** massivlərə çevirmək üçün istifadə olunur.

---

### `flat()` Metodu

`flat()` metodu, bir massiv içindəki **massivləri "düzləndirərək"** tək bir səviyyəli massiv yaradır. Bu metod massivə **toxunmur**, əvəzində **yeni bir düz massiv qaytarır**.

#### Əsas istifadə:

```javascript
[1, [2, 3]].flat();    
// => [1, 2, 3]
```

#### Sadəcə bir səviyyə düzləndirilir:

```javascript
[1, [2, [3]]].flat();  
// => [1, 2, [3]]
```

---

#### Dərinlik səviyyəsi (depth) necə idarə olunur?

Əgər birdən çox səviyyəli iç-içə massivlər varsa, `flat()` metoduna bir rəqəm arqument kimi verərək neçə səviyyə düzləndiriləcəyini təyin edə bilərsiniz:

```javascript
let a = [1, [2, [3, [4]]]];

a.flat(1); // => [1, 2, [3, [4]]]
a.flat(2); // => [1, 2, 3, [4]]
a.flat(3); // => [1, 2, 3, 4]
a.flat(99); // => [1, 2, 3, 4] 
// Dərinlikdən asılı olmayaraq tam düzləndirir
```

---

### `flatMap()` Metodu

`flatMap()` həm `map()` funksiyasını, həm də `flat(1)` funksiyasını **birdəfəlik və effektiv şəkildə** yerinə yetirir.

#### Nümunə: Cümlələri sözlərə parçalamaq

```javascript
let sentences = ["hello world", "js guide"];

let words = sentences.flatMap(sentence => sentence.split(" "));
console.log(words); 
// => ["hello", "world", "js", "guide"]
```

#### Qeyd:

`sentences.map(...).flat()` yazmaqdansa `flatMap()` daha **qısa və performanslıdır**.

---

### İstisna və filtr funksiyası kimi istifadə:

`flatMap()`-in gözəl xüsusiyyətlərindən biri də **bəzən elementləri filterləməyə** də imkan verməsidir. Sadəcə, həmin elementləri **boş massivə (`[]`)** xəritələyin:

```javascript
["apple", "", "banana", null, "cherry", undefined]
  .flatMap(fruit => fruit ? fruit : []);
// => ["apple", "banana", "cherry"] (boş və null dəyərlər çıxarılır)
```

#### Alternativ `map()` ilə edilərsə:

```javascript
["apple", "", "banana", null, "cherry", undefined]
  .map(fruit => fruit ? fruit : [])
  .flat();
// => ["apple", [], "banana", [], "cherry", []] (istədiyimiz kimi deyil!)
```

---


## 7.8.3 Massivləri Birləşdirmək: `concat()` Metodu ilə 🔗

`concat()` metodu bir və ya bir neçə massivi və/və ya elementləri mövcud massivə **əlavə edərək** yeni bir massiv yaradır. Bu metod **orijinal massivi dəyişdirmir**, sadəcə **onun yeni bir nüsxəsini qaytarır**.

### İstifadə Sintaksisi:

```javascript
let newArray = array.concat(value1, value2, ..., valueN);
```

---

### Nümunələr 

#### 1. Sadə elementlərin əlavə olunması:

```javascript
let a = [1, 2, 3];
let b = a.concat(4, 5);
console.log(b); // => [1, 2, 3, 4, 5]
```

#### 2. Massivlərin birləşdirilməsi:

```javascript
let a = [1, 2];
let b = [3, 4];
let c = a.concat(b);
console.log(c); // => [1, 2, 3, 4]
```

#### 3. Birdən çox massiv və element birləşdirilir:

```javascript
let a = [1];
let b = a.concat([2, 3], 4, [5, 6]);
console.log(b); // => [1, 2, 3, 4, 5, 6]
```

#### 4. İç-içə massivlər (nested arrays) düzləşdirilmir:

```javascript
let a = [1, 2];
let b = a.concat([3, [4, 5]]);
console.log(b); // => [1, 2, 3, [4, 5]]
```

#### 5. Orijinal massiv dəyişmir:

```javascript
let a = [10, 20];
a.concat(30);
console.log(a); // => [10, 20]
```

---

## 7.8.4 **Staklar (Stacks) və Növbələr (Queues)**

`push()`, `pop()`, `shift()` və `unshift()` metodları ilə

Massivlər, **stak (stack)** və **növbə (queue)** kimi tanınan iki məşhur məlumat strukturunu izah etmək üçün istifadə oluna bilər.

---

### Stack (Stak) — LIFO Prinsipi

**Last-In, First-Out (LIFO)**: Ən son daxil edilən element **birinci çıxır**. Kitabların üst-üstə yığılması kimi düşünün.

#### `push()` – sonuna əlavə et

Massivin **sonuna** bir və ya bir neçə element əlavə edir. `concat()`-dan fərqli olaraq, bu dəyişiklik **yerində baş verir**.

#### `pop()` – sondan sil

Massivin **sonundakı** elementi silir və **qaytarır**.



```javascript
let stack = [];
stack.push(10);      // stack = [10]
stack.push(20);      // stack = [10, 20]
console.log(stack.pop()); // => 20, stack = [10]
console.log(stack.pop()); // => 10, stack = []
```

> **Qeyd:** Əgər `stack.pop()`-u boş massivdə çağırsanız, `undefined` qaytaracaq, xəta yox.

```javascript
let stack = [];
stack.push([1, 2]); // Bütün massiv bir element kimi əlavə olunur
console.log(stack); // => [[1, 2]]

let s = [0];
s.push(...[1, 2]); // array parçalanır
console.log(s); // => [0, 1, 2]
```

---

### 🚦 Queue (Növbə) — FIFO Prinsipi

**First-In, First-Out (FIFO):** Ən əvvəl daxil edilən element **birinci çıxır**. Supermarket növbəsi kimi düşünün.

#### `push()` – sonuna əlavə et

Yeni gələn növbənin sonuna durur.

#### `shift()` – əvvəldən sil

Növbədə ilk duran xidmət edilir və çıxarılır.

```javascript
let q = [];
q.push(1);            // q = [1]
q.push(2);            // q = [1, 2]
console.log(q.shift()); // => 1, q = [2]
console.log(q.shift()); // => 2, q = []
```

---

### Alternativ: `unshift()` və `shift()`

Əgər növbənin **əvvəlinə əlavə edib**, **əvvəlindən silmək** istəyirsinizsə:

#### `unshift()` – əvvələ əlavə et

#### `shift()` – əvvəldən sil


```javascript
let a = [];
a.unshift(1);      // a = [1]
a.unshift(2);      // a = [2, 1]
console.log(a.shift()); // => 2
console.log(a);        // => [1]
```
---

## 7.8.5 Alt-massivlər (Subarrays) ilə işləmək: `slice()`, `splice()`, `fill()` və `copyWithin()`

JavaScript-də massivlərin müəyyən hissələri ilə işləmək üçün bir neçə metod mövcuddur. Bu metodlar alt-massiv çıxarmaq, dəyişmək, doldurmaq və ya kopyalamaq üçün istifadə olunur.

---

## `slice()` metodu

Massivin müəyyən bir hissəsini çıxarır, **orijinal massiv dəyişmir**.

### Sintaksis:

```javascript
array.slice(start, end)
```

* `start` — başlanğıc indeks (daxildir)
* `end` — son indeks (daxil deyil, optional)

### Nümunələr:

```javascript
let arr = [10, 20, 30, 40, 50];

console.log(arr.slice(1, 4));  // [20, 30, 40]
console.log(arr.slice(2));     // [30, 40, 50]
console.log(arr.slice(-3, -1));// [30, 40]
console.log(arr.slice());      // [10, 20, 30, 40, 50] (tam surət)
console.log(arr);              // [10, 20, 30, 40, 50] (dəyişməyib)
```

---

## `splice()` metodu

Massivi **dəyişdirir**: elementləri silmək, əlavə etmək və ya əvəz etmək üçün istifadə olunur.

### Sintaksis:

```javascript
array.splice(start, deleteCount, item1, item2, ...)
```

* `start` — əməliyyatın başlanğıc indeksi
* `deleteCount` — neçə element silinəcək
* `item1, item2, ...` — əlavə olunacaq elementlər (optional)

### Nümunələr:

```javascript
let arr = [1, 2, 3, 4, 5];

// 2 element sil, indeks 1 və 2-də olanlar (2 və 3)
console.log(arr.splice(1, 2)); // [2, 3]
console.log(arr);              // [1, 4, 5]

// İndeks 1-də heç nə silmədən "a", "b" əlavə et
arr.splice(1, 0, 'a', 'b');
console.log(arr);              // [1, "a", "b", 4, 5]

// İndeks 3-də 1 element sil və 100 əlavə et
arr.splice(3, 1, 100);
console.log(arr);              // [1, "a", "b", 100, 5]

// Bütün elementləri sil (indeks 0-dan sonuna qədər)
console.log(arr.splice(0));    // [1, "a", "b", 100, 5]
console.log(arr);              // []
```

---

## `fill()` metodu

Massivin elementlərini müəyyən dəyər ilə doldurur, **orijinal massiv dəyişir**.

### Sintaksis:

```javascript
array.fill(value, start, end)
```

* `value` — doldurulacaq dəyər
* `start` — başlanğıc indeks (optional)
* `end` — bitmə indeksi (optional, daxil deyil)

### Nümunələr:

```javascript
let arr = new Array(5);      // [ <5 empty items> ]

arr.fill(0);
console.log(arr);            // [0, 0, 0, 0, 0]

arr.fill(5, 2);
console.log(arr);            // [0, 0, 5, 5, 5]

arr.fill(9, 1, 4);
console.log(arr);            // [0, 9, 9, 9, 5]

arr.fill(7, -3, -1);
console.log(arr);            // [0, 9, 7, 7, 5]
```

---

## `copyWithin()` metodu

Massivin bir hissəsini başqa yerə **yerində dəyişərək** kopyalayır. Massivin uzunluğunu dəyişdirmir.

### Sintaksis:

```javascript
array.copyWithin(target, start, end)
```

* `target` — kopyalanacaq hissənin başlanacağı indeks
* `start` — kopyalanacaq hissənin başlanğıc indeksi (optional)
* `end` — kopyalanacaq hissənin bitmə indeksi (optional, daxil deyil)

### Nümunələr:

```javascript
let arr = [1, 2, 3, 4, 5];

// indeks 1-dən başlayaraq, 0-dan sonuna qədər elementləri kopyala
arr.copyWithin(1);
console.log(arr);            // [1, 1, 2, 3, 4]

let arr2 = [10, 20, 30, 40, 50];

// indeks 2-yə, 3-dən 5-ə qədər elementləri kopyala (yəni 40 və 50)
arr2.copyWithin(2, 3, 5);
console.log(arr2);           // [10, 20, 40, 50, 50]

let arr3 = [5, 6, 7, 8, 9];

// mənfi indekslərlə: 0-a sondan 2-ci elementdən başlayaraq kopyala
arr3.copyWithin(0, -2);
console.log(arr3);           // [8, 9, 7, 8, 9]
```

---

# 7.8.6 Massiv Axtarış və Sıralama Metodları (Array Searching and Sorting Methods)

JavaScript-də massivlərə eynilə stringlərdə olduğu kimi `indexOf()`, `lastIndexOf()` və `includes()` metodları tətbiq olunur. Bunlar massivdə müəyyən bir dəyərin olub-olmadığını və harada yerləşdiyini tapmaq üçün istifadə olunur. Massivdə elementlərin sırasını dəyişdirmək üçün isə `sort()` və `reverse()` metodları var.

---

## `indexOf()` və `lastIndexOf()`

* **`indexOf(value, fromIndex)`** — Massivi əvvəldən sona doğru axtarır və `value` dəyərinin ilk tapıldığı indeksini qaytarır.
* **`lastIndexOf(value, fromIndex)`** — Massivi sondan əvvələ doğru axtarır və `value` dəyərinin ilk tapıldığı (sondan başlayan) indeksini qaytarır.
* Tapılmasa, nəticə `-1` olur.

> **Qeyd:** İkinci arqument axtarışa başlanğıc indeksidir və mənfi dəyərlər də qəbul olunur (massivin sonundan geri saymaq kimi).

**Nümunə:**

```javascript
let a = [0, 1, 2, 1, 0];

console.log(a.indexOf(1));     // 1 (ilk 1-in indeksi)
console.log(a.lastIndexOf(1)); // 3 (sondan ilk 1-in indeksi)
console.log(a.indexOf(3));     // -1 (tapılmadı)
```

---

## `findall()` funksiyası nümunəsi

Massivdə müəyyən dəyərin bütün indekslərini tapmaq üçün `indexOf()`-un ikinci arqumentindən istifadə edərək belə funksiyanı yarada bilərik:

```javascript
function findall(a, x) {
  let results = [], pos = 0;
  while (pos < a.length) {
    pos = a.indexOf(x, pos);
    if (pos === -1) break;
    results.push(pos);
    pos++;
  }
  return results;
}

let numbers = [10, 20, 10, 30, 10];
console.log(findall(numbers, 10)); // [0, 2, 4]
```

---

## `includes()`

* `includes(value)` — Massivdə `value`-nun olub-olmadığını `true`/`false` şəklində qaytarır.
* `indexOf()`-dan fərqli olaraq, `includes()` **`NaN` dəyərini də aşkar edə bilir**.

**Nümunə:**

```javascript
let a = [1, true, 3, NaN];

console.log(a.includes(true));  // true
console.log(a.includes(2));     // false
console.log(a.includes(NaN));   // true
console.log(a.indexOf(NaN));    // -1 (indexOf NaN-ı tapa bilməz)
```

---

## `sort()`

* `sort()` massiv elementlərini **yerində** (in place) sıralayır və sıralanmış massivi qaytarır.
* Standart halda, elementləri **əlifba sırası ilə** sıralayır (rəqəmləri də string kimi).
* Ədədi sıralama üçün xüsusi müqayisə funksiyası vermək lazımdır.

**Nümunələr:**

```javascript
let a = ["banana", "cherry", "apple"];
a.sort();
console.log(a); // ["apple", "banana", "cherry"]

let nums = [33, 4, 1111, 222];
nums.sort();
console.log(nums); // [1111, 222, 33, 4] — əlifba sırası ilə (səhv!)

nums.sort((a, b) => a - b);
console.log(nums); // [4, 33, 222, 1111] — ədədi sıralama

nums.sort((a, b) => b - a);
console.log(nums); // [1111, 222, 33, 4] — tərs ədədi sıralama
```

---

### Hərf böyük-kiçik fərqinə həssas olmayan sıralama

```javascript
let a = ["ant", "Bug", "cat", "Dog"];

a.sort();
console.log(a); // ["Bug", "Dog", "ant", "cat"] (böyük-kiçik hərflərə həssas)

a.sort((s, t) => {
  s = s.toLowerCase();
  t = t.toLowerCase();
  if (s < t) return -1;
  if (s > t) return 1;
  return 0;
});
console.log(a); // ["ant", "Bug", "cat", "Dog"] (həssas deyil)
```

---

## `reverse()`

* Massivin elementlərinin sırasını tərs çevirir.
* Bu da **yerində** edilir, yəni yeni massiv yaratmır.

**Nümunə:**

```javascript
let a = [1, 2, 3];
a.reverse();
console.log(a); // [3, 2, 1]
```

---

# 7.8.7 Massivi Stringə Çevirmə (Array to String Conversions) ✍️

JavaScript-də massivləri stringlərə çevirmək üçün 3 əsas metod var. Bunlar əsasən məlumatları ekrana, loglara və ya xəta mesajlarına yazmaq üçün istifadə olunur.

---

## `join()` Metodu

* Massivin bütün elementlərini stringə çevirir və onları müəyyən edilmiş ayırıcı ilə birləşdirir.
* Əgər ayırıcı göstərilməzsə, **defolt olaraq vergül (",")** istifadə olunur.

**Nümunələr:**

```javascript
let a = [1, 2, 3];

console.log(a.join());     // "1,2,3"
console.log(a.join(" "));  // "1 2 3"
console.log(a.join(""));   // "123"

let b = new Array(10);     // Uzunluğu 10, amma elementləri boş massiv
console.log(b.join("-"));  // "---------"
```

> **Qeyd:** `join()` stringi hissələrə bölən `String.split()` metodunun əksidir.

---

## `toString()` Metodu

* Massivlərin standart `toString()` metodu çağırıldığında, o, `join()` metodunun arqumentsiz versiyası kimi işləyir.
* Yəni elementləri vergüllə birləşdirərək string qaytarır.

**Nümunələr:**

```javascript
console.log([1, 2, 3].toString());       // "1,2,3"
console.log(["a", "b", "c"].toString()); // "a,b,c"
console.log([1, [2, "c"]].toString());   // "1,2,c"  (daxili massiv də stringə çevrilir)
```

> **Qeyd:** `toString()` nəticəsində kvadrat mötərizələr və ya digər ayırıcılar çıxmır.

---

## `toLocaleString()` Metodu

* `toLocaleString()` metodunun işi `toString()` kimi olsa da, fərqli olaraq **hər bir elementin öz `toLocaleString()` metodunu çağırır**.
* Bu, lokalizə edilmiş (məsələn, rəqəmlərdə və tarixlərdə regional format) stringlər almağa imkan verir.
* Nəticədə elementlər lokal-spesifik ayırıcı ilə birləşdirilir.

**Misal:**

```javascript
let numbers = [123456.789, new Date(Date.UTC(2020, 0, 1))];

console.log(numbers.toString());
// "123456.789,Wed Jan 01 2020 03:00:00 GMT+0300 (GMT+03:00)"

console.log(numbers.toLocaleString('de-DE'));
// "123.456,789; 01.01.2020"
```

Burada `toLocaleString()` metodu sayları və tarixləri Almaniya regional qaydasına uyğun formatlayır.

---


# 7.8.8 Statik Massiv Funksiyaları (Static Array Functions)

JavaScript-də `Array` sinfi, massiv üzərində əməliyyat aparan metodlardan başqa, **statik funksiyalar** da təqdim edir. Bu funksiyalar `Array` konstruktoru vasitəsilə çağırılır və massivlərin yaradılması və yoxlanması üçün istifadə olunur.

---

### `Array.of()` və `Array.from()`

* Bu iki metod **fabrik metodlarıdır (factory methods)** və yeni massiv yaratmaq üçün istifadə olunur.
* Onların iş prinsipi və istifadəsi barədə daha əvvəl (§7.1.4 və §7.1.5) ətraflı sənədləşdirilib.

---

### `Array.isArray()`

* `Array.isArray(value)` funksiyası daxil edilən `value` dəyərinin massiv olub-olmadığını müəyyən edir.
* Əgər massivdirsə `true`, deyilsə `false` qaytarır.

**Nümunələr:**

```javascript
console.log(Array.isArray([]));    // true (boş massiv)
console.log(Array.isArray([1,2,3])); // true (elementləri olan massiv)
console.log(Array.isArray({}));    // false (obyekt, massiv deyil)
console.log(Array.isArray("foo")); // false (string, massiv deyil)
console.log(Array.isArray(null));  // false
```

---

# 7.9 Massivə Bənzər Obyektlər (Array-Like Objects) 🎭

JavaScript massivləri bəzi özəl xüsusiyyətlərə malikdir, məsələn:

* `length` xüsusiyyəti avtomatik yenilənir,
* `length`-i kiçiltmək massivdə elementləri kəsir,
* `Array.prototype`-dan metodları miras alırlar,
* `Array.isArray()` onları aşkar edir.

Lakin, **ədədi `length` xüsusiyyətinə** və uyğun **0-dan başlayaraq tam ədəd indekslərə** sahib istənilən obyekt **massivə bənzər obyekt (array-like object)** sayılır.

---

### Misal: adi obyektə massivə bənzər xüsusiyyətlər əlavə etmək

```javascript
let a = {};    // boş obyekt

for (let i = 0; i < 10; i++) {
  a[i] = i * i;   // indeks kimi string, dəyər kimi kvadrat
}
a.length = 10;    // length əlavə edirik

let total = 0;
for (let j = 0; j < a.length; j++) {
  total += a[j];
}
console.log(total); // 285 (0² + 1² + ... + 9²)
```

---

### Massivə bənzər obyektləri yoxlamaq üçün funksiya

```javascript
function isArrayLike(o) {
  return o && typeof o === "object" &&
         Number.isFinite(o.length) &&
         o.length >= 0 &&
         Number.isInteger(o.length) &&
         o.length < 4294967295;
}

console.log(isArrayLike([]));                     // true
console.log(isArrayLike({0: 'a', 1: 'b', length: 2})); // true
console.log(isArrayLike("salam"));                // false (string)
console.log(isArrayLike(function(){}));           // false (funksiya)
```

---

### Massiv metodlarını massivə bənzər obyektlərdə istifadə etmək

`Array.prototype` metodları yalnız həqiqi massivlərdə var, ona görə massivə bənzər obyektlərdə onları birbaşa çağıra bilmərik. Amma `call` ilə dolayı istifadə edə bilərik:

```javascript
let a = {0: "a", 1: "b", 2: "c", length: 3};

console.log(Array.prototype.join.call(a, "+"));        
// "a+b+c"
console.log(Array.prototype.map.call(a, x => x.toUpperCase())); 
// ["A", "B", "C"]
console.log(Array.prototype.slice.call(a, 0));          
// ["a", "b", "c"]

// Daha sadə üsul (ES6+)
console.log(Array.from(a));                              
// ["a", "b", "c"]
```

---

# 7.10 Stringlər Massiv Kimi (Strings as Arrays) 📜

JavaScript-də stringlər UTF-16 kodlu simvolların **yalnız oxumaq üçün nəzərdə tutulmuş massivləri (read-only arrays)** kimidir. Onların elementlərinə `charAt()` metodu ilə yanaşı, kvadrat mötərizələr (`[]`) vasitəsilə də daxil olmaq olar:

```javascript
let s = "test";
console.log(s.charAt(0)); // "t"
console.log(s[1]);        // "e"
```

* `typeof s` hələ də `"string"` qaytarır.
* `Array.isArray(s)` isə **`false`** döndərir — string massiv deyil.

---

### Stringlərin massiv kimi davranmasının faydası

* Daha qısa və oxunaqlı indeksləmə (kvadrat mötərizələrlə).
* Stringlərə massiv metodlarını tətbiq etmək mümkün olur (dolayı yolla).

Məsələn, stringin hər bir simvolunu aralarına boşluq qoyaraq birləşdirmək:

```javascript
console.log(Array.prototype.join.call("JavaScript", " ")); 
// "J a v a S c r i p t"
```

---

### Vacib məqamlar

* Stringlər **dəyişməzdir (immutable)** — yəni onların içindəki simvolları dəyişmək olmaz.
* Massiv metodları kimi `push()`, `sort()`, `reverse()`, `splice()` və s. stringlərdə **işləmir** (yerində dəyişiklik tələb edənlər).
* Belə metodları stringə tətbiq etməyə cəhd etsəniz, **xəta vermir, amma heç bir dəyişiklik də etmir (fails silently)**.

---