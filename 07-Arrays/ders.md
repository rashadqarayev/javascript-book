# Fəsil 7. Massivlər (Arrays) 📦

JavaScript-də **massivlər (arrays)** dəyərlərin **sıralanmış kolleksiyasıdır**. Onlar proqramlaşdırmanın əsas hissələrindən biridir.

---

### Massiv Nədir? 🧩

* **Sıralanmış Dəyərlər:** Massivdəki hər bir dəyər bir "element" adlanır.
* **İndeks (Sıra Nömrəsi):** Hər elementin massivdə bir rəqəmsal mövqeyi var, buna "indeks" deyilir.
* **Sıfırdan Başlama:** JavaScript massivlərində ilk elementin indeksi **0 (sıfır)**-dır. Yəni, `massiv[0]` ilk elementi verir.
* **Maksimum Ölçü:** İndekslər 32-bitdir, yəni massivin ən yüksək mümkün indeksi `4294967294` (`2^32 - 2`) ola bilər. Maksimum element sayı isə `4,294,967,295` olaraq düşünülür.

---

### JavaScript Massivlərinin Xüsusiyyətləri ✨

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

### Massivlər və Obyektlər — Əlaqə 🤝

* JavaScript massivləri əslində JavaScript obyektlərinin **xüsusi bir formasıdır**.
* Massiv indeksləri (0, 1, 2...) sadəcə tam ədədlərdən ibarət xüsusiyyət adları kimidir.
* Lakin, JavaScript tətbiqləri (brauzerlər, Node.js) massivləri optimallaşdırır, ona görə də rəqəmlə indekslənmiş elementlərə daxil olmaq adi obyekt xüsusiyyətlərinə daxil olmaqdan **xeyli sürətli** olur.

---

# 7.1 Massivləri Yaratmaq (Creating Arrays) 🛠️

JavaScript-də massivləri yaratmaq üçün bir neçə üsul var. İndi onlara ardıcıllıqla baxaq:

* Massiv Literalları (Array Literals)
* `...` Spread Operatoru (Yayma Operatoru)
* `Array()` Konstruktoru
* `Array.of()` və `Array.from()` Fabrik Metodları

---

## 7.1.1 Massiv Literalları (Array Literals) 📝

Massiv yaratmağın ən sadə yolu **massiv literallarından** istifadə etməkdir. Bu, sadəcə kvadrat mötərizələr `[]` daxilində vergüllə ayrılmış elementlər siyahısıdır.

**Misallar:**

```javascript
let bosMassiv = [];             // Heç bir elementi olmayan massiv
let ededler = [2, 3, 5, 7, 11]; // 5 ədəd elementi olan massiv
let qarisik = [1.1, true, "salam", ]; // Fərqli tipli 3 element. Sonda əlavə vergül ola bilər.
```

* Massiv literalındakı dəyərlər sabit olmaq məcburiyyətində deyil, istənilən **ifadə** (expression) ola bilər:

    ```javascript
    let esas = 100;
    let cedvel = [esas, esas + 1, esas + 2]; // => [100, 101, 102]
    ```

* Massiv literalları başqa **obyekt literallarını** və ya **massiv literallarını** da ehtiva edə bilər:

    ```javascript
    let data = [[1, {ad: "Ali"}], [2, {ad: "Veli"}]]; // Massiv daxilində massiv və obyektlər
    ```

* **Seyrək Massivlər (Sparse Arrays):** Əgər massiv literalında iki vergül arasında dəyər yoxdursa, bu, həmin indeksdə elementin olmadığını göstərir və massiv seyrək olur. Belə elementləri soruşanda `undefined` qaytarılır.

    ```javascript
    let say = [1,,3];   // İndeks 0 (dəyər 1) və 2 (dəyər 3) var. İndeks 1-də element yoxdur.
    console.log(say.length); // => 3 (Uzunluq boşluqları da sayır)
    console.log(say[1]);     // => undefined

    let bosluqlar = [,,]; // Heç bir elementi olmayan, amma uzunluğu 2 olan massiv
    console.log(bosluqlar.length); // => 2
    console.log(bosluqlar[0]);     // => undefined
    ```
    Qeyd: Massiv literal sintaksisi sonda əlavə bir vergülə icazə verir, ona görə `[,,]` massivinin uzunluğu 2-dir, 3 deyil.

---

## 7.1.2 Spread Operator (`...`) — Yayma Operatoru 📤

ES6 (ECMAScript 2015) və sonrakı versiyalarda, bir massivi (və ya digər **iterasiya edilə bilən obyektləri**) başqa bir massiv literalının daxilinə daxil etmək üçün **"spread operatoru" (`...`)** istifadə olunur.

```javascript
let a = [1, 2, 3];
let b = [0, ...a, 4]; // 'a' massivinin elementləri 'b' massivinin içinə yayılır
console.log(b);       // => [0, 1, 2, 3, 4]
```
`...a` hissəsi sanki `1, 2, 3` kimi ədədlərin birbaşa oraya yazılması kimidir.

* **Massivin Kopyalanması (Shallow Copy):** Spread operatoru massivləri (sığ bir şəkildə) kopyalamaq üçün çox rahat bir yoldur:

    ```javascript
    let orijinal = [1, 2, 3];
    let kopya = [...orijinal]; // orijinal massivinin kopyasını yaradır
    kopya[0] = 0;             // Kopyanı dəyişdirmək orijinalı dəyişdirmir

    console.log(orijinal[0]); // => 1
    console.log(kopya);       // => [0, 2, 3]
    ```

* **İterasiya Edilə Bilən Obyektlər (Iterable Objects):** Spread operatoru yalnız massivlər üzərində deyil, **iterasiya edilə bilən** (yəni `for/of` dövrü ilə gəzilə bilən) istənilən obyekt üzərində işləyir.

    * **Stringlər:** Stringlər iterasiya edilə bilən obyektlərdir, ona görə də onları spread operatoru ilə tək xarakterli string massivlərinə çevirə bilərsiniz:

        ```javascript
        let reqemler = [..."0123456789"];
        console.log(reqemler); // => ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"]
        ```

    * **`Set` Obyektləri:** `Set` obyektləri təkrarlanan elementləri saxlamayan kolleksiyalardır. `Set` obyektləri də iterasiya edilə biləndir. Massivdən təkrarlanan elementləri çıxarmaq üçün massivi `Set`-ə çevirib, sonra `Set`-i spread operatoru ilə yenidən massivə çevirmək asan yoldur:

        ```javascript
        let herfler = [..."hello world"]; // => ["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d"]
        let tekrarsizHerfler = [...new Set(herfler)]; // Təkrarlananları çıxarır
        console.log(tekrarsizHerfler); // => ["h", "e", "l", "o", " ", "w", "r", "d"]
        ```

---

## 7.1.3 `Array()` Konstruktoru 🏗️

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

## 7.1.4 `Array.of()` Metodu 🆕

`Array()` konstruktorunun tək rəqəmsal arqumentlə çağırılanda uzunluq təyin etməsi, amma birdən çox rəqəmsal arqumentlə çağırılanda elementlər kimi davranması bir çaşqınlıq yarada bilirdi. Məsələn, **tək rəqəmsal elementli bir massiv** yaratmaq `Array()` konstruktoru ilə birbaşa mümkün deyildi (yəni `new Array(10)` `[10]` yox, `[ <10 empty items> ]` yaradırdı).

**ES6** bu problemi **`Array.of()`** funksiyası ilə həll etdi. Bu, bir "fabrik metodu"dur (factory method). O, arqumentlərinin sayından asılı olmayaraq, həmin arqument dəyərlərini yeni massivin elementləri kimi istifadə edərək bir massiv yaradır və qaytarır:

```javascript
Array.of();       // => [] (Boş massiv)
Array.of(10);     // => [10] (Tək rəqəmsal elementli massiv yaratmaq mümkündür!)
Array.of(1, 2, 3); // => [1, 2, 3]
```

---

## 7.1.5 `Array.from()` Metodu 🔄

`Array.from()` ES6-da təqdim olunan başqa bir massiv fabrik metodudur. Bu metod ilk arqument kimi **iterasiya edilə bilən obyekt** (məsələn, massiv, string, Set) və ya **massivəbənzər obyekt** gözləyir. O, bu obyektin elementlərini ehtiva edən yeni bir massiv qaytarır.

* **İterasiya edilə bilən obyektlərdən massiv yaratmaq:**
    `Array.from(iterable)` istifadəsi `[...iterable]` (spread operatoru) ilə eyni şəkildə işləyir. Bu, bir massivin kopyasını yaratmaq üçün də sadə bir yoldur:

    ```javascript
    let orijinal = [1, 2, 3];
    let kopya = Array.from(orijinal); // orijinal massivinin kopyasını yaradır
    console.log(kopya); // => [1, 2, 3]

    let herfler = Array.from("Salam"); // Stringdən massiv yaratmaq
    console.log(herfler); // => ["S", "a", "l", "a", "m"]
    ```

* **Massivəbənzər Obyektlərdən Həqiqi Massiv Yaratmaq:**
    `Array.from()` xüsusilə **"massivəbənzər obyektləri" (array-like objects)** həqiqi JavaScript massivlərinə çevirmək üçün vacibdir. Massivəbənzər obyektlər massiv olmayan obyektlərdir ki, onların **rəqəmsal `length` xüsusiyyəti** və adları tam ədədlər olan dəyərlər saxlayan xüsusiyyətləri var.
    * Məsələn, brauzerdə bəzi DOM metodlarının qaytardığı dəyərlər (HTML elementlərinin kolleksiyası kimi) massivəbənzər olur. Onlarla massiv metodları ilə işləmək üçün əvvəlcə həqiqi massivə çevirmək daha rahatdır:

        ```javascript
        // Fərz edək ki, "arrayLikeObject" bir massivəbənzər obyektdir
        let heqiqiMassiv = Array.from(arrayLikeObject);
        ```

* **İkinci Arqument (Map Funksiyası):**
    `Array.from()` həmçinin ixtiyari olaraq **ikinci bir arqument** qəbul edir: bir funksiya. Əgər bu funksiyanı ikinci arqument kimi versəniz, yeni massiv qurularkən mənbə obyektindən gələn hər bir element bu funksiyaya ötürüləcək və funksiyanın qaytardığı dəyər massivə orijinal dəyər əvəzinə əlavə olunacaq.
    (Bu, massivin `map()` metodu ilə çox bənzəyir, lakin massiv qurularkən çevirmə aparmaq, massivi qurub sonra başqa bir massivə çevirməkdən daha effektivdir.)

    ```javascript
    let reqemler = Array.from("123", Number); // Hər xarakteri rəqəmə çevirir
    console.log(reqemler); // => [1, 2, 3]

    let tekEdedlerKvadrat = Array.from([1, 2, 3, 4, 5], (num) => {
        if (num % 2 !== 0) return num * num; // Tək ədədlərin kvadratını qaytarır
        return num; // Cüt ədədləri olduğu kimi saxlayır
    });
    console.log(tekEdedlerKvadrat); // => [1, 2, 9, 4, 25]
    ```

---

# 7.2 Massiv Elementlərini Oxumaq və Yazmaq 📖✍️

Massivin elementlərinə daxil olmaq və ya onların dəyərini dəyişdirmək üçün **kvadrat mötərizə operatoru (`[]`)** istifadə olunur.

* **Sintaksis:** Massivə istinad (`a` kimi bir dəyişən) kvadrat mötərizələrin solunda olmalı, mötərizələrin daxilində isə **sıfırdan böyük və ya sıfıra bərabər olan tam ədəd** dəyəri verən istənilən ifadə (yəni indeks) olmalıdır.
* Bu sintaksis həm massiv elementinin dəyərini **oxumaq**, həm də **yazmaq** üçün istifadə olunur.

**Misallar:**

```javascript
let a = ["dünya"]; // Bir elementli massiv ilə başlayırıq: a[0] = "dünya"

let deyer = a[0];     // Massivin 0-cı elementini oxuyuruq
console.log(deyer);   // => "dünya"

a[1] = 3.14;          // Massivin 1-ci elementinə dəyər yazırıq
console.log(a);       // => ["dünya", 3.14]

let i = 2;
a[i] = "üç";          // Massivin 2-ci elementinə dəyər yazırıq
console.log(a);       // => ["dünya", 3.14, "üç"]

a[i + 1] = "salam";   // Massivin 3-cü elementinə dəyər yazırıq (i+1 = 3)
console.log(a);       // => ["dünya", 3.14, "üç", "salam"]

a[a[i]] = a[0];       // Mürəkkəb əməliyyat:
                      // Öncə a[i] oxunur (yəni a[2] = "üç")
                      // Sonra a["üç"] kimi dəyər təyin olunur.
                      // Bu, rəqəmsal olmayan indeks olduğuna görə, massiv xüsusiyyəti kimi yaranır.
                      // a[0] = "dünya" olduğuna görə a["üç"] = "dünya" olur.
                      // Massivlər obyekt olduğu üçün belə də xüsusiyyət əlavə etmək olar.
console.log(a);       // => ["dünya", 3.14, "üç", "salam", "üç": "dünya"] - burada "üç" string kimi property adıdır.
```

---

### Massivlər və `length` Xüsusiyyəti (Avtomatik Yenilənmə) 📏

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

### Massiv İndeksləri vs. Obyekt Xüsusiyyət Adları 🤔

Yadda saxlayın ki, massivlər əslində obyektlərin xüsusi bir növüdür. Massiv elementlərinə daxil olmaq üçün istifadə olunan kvadrat mötərizələr, obyekt xüsusiyyətlərinə daxil olmaq üçün istifadə olunan kvadrat mötərizələr kimi işləyir.

* JavaScript sizin təyin etdiyiniz rəqəmsal massiv indeksini avtomatik olaraq **stringə** çevirir. Məsələn, indeks `1` "1" stringinə çevrilir, sonra bu string xüsusiyyət adı kimi istifadə olunur.
* Bu, yalnız massivlərə aid bir xüsusiyyət deyil; adi obyektlərdə də bu cür indeksləmə edə bilərsiniz:

    ```javascript
    let o = {};      // Boş bir obyekt yaradırıq
    o[1] = "bir";    // Ona tam ədədlə indeksləmə edirik
    console.log(o);  // => { '1': 'bir' }
    console.log(o["1"]); // => "bir" (Rəqəmsal və string xüsusiyyət adları eynidir!)
    ```

Aydın olmaq üçün, massiv indeksini obyekt xüsusiyyət adından fərqləndirmək faydalıdır:
* **Bütün indekslər xüsusiyyət adlarıdır.**
* Lakin, yalnız `0` ilə `2^32 - 2` arasında olan **tam ədədlər** (bu aralıqda olan stringlər də) **indeks** hesab olunur.
* Bütün massivlər obyektlərdir və siz onlara istənilən adla xüsusiyyətlər əlavə edə bilərsiniz. Amma əgər istifadə etdiyiniz xüsusiyyət adı massiv indeksi qaydalarına uyğundursa, massiv **`length` xüsusiyyətini avtomatik yeniləmək** kimi xüsusi bir davranış sərgiləyir.

---

### Mənfi İndekslər və Tam Olmayan İndekslər ⚠️

* Massivi mənfi ədədlərlə və ya tam olmayan ədədlərlə indeksləyə bilərsiniz. Bu zaman rəqəm stringə çevrilir və bu string xüsusiyyət adı kimi istifadə olunur.
* Belə hallar **massiv indeksi** kimi yox, adi **obyekt xüsusiyyəti** kimi qəbul edilir və massivin `length` xüsusiyyətini dəyişdirmir.
* Əgər massivi tam ədədə çevrilə bilən bir stringlə indeksləsəniz (məsələn, "1000"), bu da massiv indeksi kimi davranacaq.
* Eyni şey, tam ədəd olan onluq ədədlər üçün də doğrudur (məsələn, `1.000` əslində `1` deməkdir):

    ```javascript
    let a = [1, 2, 3];

    a[-1.23] = true;  // Bu, "-1.23" adlı bir xüsusiyyət yaradır. Massivin length-i dəyişmir.
    console.log(a);     // => [1, 2, 3, '-1.23': true]
    console.log(a.length); // => 3

    a["1000"] = 0;    // Bu, massivin 1001-ci elementi kimi qəbul edilir (indeks 1000). Length 1001 olur.
    console.log(a.length); // => 1001

    a[1.000] = "yeni"; // Bu, indeks 1-dir. a[1] = "yeni" ilə eynidir.
    console.log(a);     // => [1, "yeni", 3, '-1.23': true, '1000': 0]
    ```

---

### "Out of Bounds" Xətası Yoxdur ✅

Massiv indekslərinin sadəcə xüsusi bir növ obyekt xüsusiyyət adı olması o deməkdir ki, JavaScript massivlərində "sərhəddən kənar" (out of bounds) xətası yoxdur.
Hər hansı bir obyektin mövcud olmayan xüsusiyyətini soruşduğunuzda, xəta almırsınız; sadəcə `undefined` dəyəri alırsınız. Bu, obyektlər üçün olduğu kimi massivlər üçün də doğrudur:

```javascript
let a = [true, false]; // Bu massivin 0 və 1 indekslərində elementləri var

console.log(a[2]);   // => undefined; bu indeksdə element yoxdur
console.log(a[-1]);  // => undefined; bu adda xüsusiyyət yoxdur (massiv indeksi deyil)
console.log(a[100]); // => undefined; bu indeksdə element yoxdur
```
Yəni, bir massivin mövcud olmayan bir indeksinə daxil olmaq xəta vermir, sadəcə `undefined` dəyərini qaytarır.

---

# 7.3 Seyrək Massivlər (Sparse Arrays) 🍂

**Seyrək massiv** elə bir massivdir ki, onun elementləri 0-dan başlayaraq ardıcıl indekslərə malik deyil. Yəni, bəzi indekslərdə elementlər yoxdur, boşluqlar var.

* **`length` Xüsusiyyəti:** Normalda, massivin `length` xüsusiyyəti elementlərin sayını göstərir. Amma seyrək massivdə `length` dəyəri, massivdəki elementlərin sayından **daha böyük** olur.

* **Seyrək Massiv Necə Yaradılır?**
    * `Array()` konstruktoru ilə:

        ```javascript
        let a = new Array(5); // Heç bir element yoxdur, amma a.length 5-dir.
        console.log(a);         // => [ <5 empty items> ]
        console.log(a.length);  // => 5
        ```
    * Mövcud massivin cari uzunluğundan daha böyük bir indeksə dəyər təyin etməklə:

        ```javascript
        let a = [];         // Boş massiv, uzunluğu 0.
        a[1000] = 0;        // Bir element əlavə edir, amma massivin uzunluğunu 1001-ə qədər artırır.
        console.log(a);     // => [ <1000 empty items>, 0 ]
        console.log(a.length); // => 1001
        ```
    * Gələcəkdə öyrənəcəyimiz `delete` operatoru ilə də massivi seyrək etmək olar.

* **Performans:** Çox seyrək olan massivlər adətən daha yavaş, lakin yaddaşa daha qənaətcil şəkildə həyata keçirilir. Belə massivlərdə element axtarmaq adi obyekt xüsusiyyətlərini axtarmaq qədər vaxt apara bilər.

* **Massiv Literallarında Seyrəklik:**
    Massiv literalında vergüllər arasında dəyər buraxdığınız zaman (məsələn, `[1,,3]` kimi), yaranan massiv seyrək olur və buraxılan elementlər sadəcə mövcud olmur:

    ```javascript
    let a1 = [,];         // Bu massivdə element yoxdur, uzunluğu 1-dir.
    console.log(0 in a1); // => false (a1-də 0 indeksli element yoxdur)

    let a2 = [undefined]; // Bu massivdə undefined dəyərli bir element var.
    console.log(0 in a2); // => true (a2-də 0 indeksli element var və dəyəri undefined-dir)
    ```
    Yəni, `[,,]` ilə `[undefined, undefined]` fərqlidir. Birincidə elementlər yoxdur, ikincidə isə `undefined` dəyəri olan elementlər var.

* **Nəticə:** Seyrək massivləri başa düşmək JavaScript massivlərinin əsl təbiətini anlamaq üçün vacibdir. Ancaq praktikada əksər JavaScript massivləri seyrək olmur. Əgər seyrək massivlərlə işləməli olsanız, kodunuz yəqin ki, onları `undefined` elementləri olan qeyri-seyrək massivlər kimi qəbul edəcək.

---

# 7.4 Massivin Uzunluğu (Array Length) 📏

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
        a.length = 3;           // length-i 3-ə qoyduq. İndeksi 3 və 4 olan elementlər silindi.
        console.log(a);         // => [1, 2, 3]

        a.length = 0;           // length-i 0-a qoyduq. Bütün elementlər silindi.
        console.log(a);         // => [] (Boş massiv)

        a.length = 5;           // length-i 5-ə qoyduq.
        console.log(a);         // => [ <5 empty items> ] (Uzunluq 5-dir, amma element yoxdur,
                                //     sanki 'new Array(5)' ilə yaradılıb)
        ```

* **`length` Dəyərini Artırmaq:**
    Massivin `length` xüsusiyyətinin dəyərini cari dəyərindən daha böyük bir dəyərə təyin edə bilərsiniz. Bunu etmək massivə yeni elementlər əlavə etmir; sadəcə massivin sonunda seyrək bir sahə (boşluq) yaradır.

---

# 7.5 Massiv Elementlərini Əlavə Etmək və Silmək ➕➖

Massivlərə element əlavə etmək və ya silmək üçün bir neçə üsul var.

### Element Əlavə Etmək (Adding Elements)

1.  **Yeni İndeksə Dəyər Təyin Etməklə (Ən Sadə Yol):**
    Massivə element əlavə etməyin ən sadə yolu sadəcə mövcud uzunluğundan daha böyük bir indeksə dəyər təyin etməkdir:

    ```javascript
    let a = [];          // Boş massiv ilə başlayırıq
    a[0] = "sıfır";      // 0-cı indeksə dəyər əlavə edirik
    a[1] = "bir";        // 1-ci indeksə dəyər əlavə edirik
    console.log(a);      // => ["sıfır", "bir"]
    ```

2.  **`push()` Metodu ilə (Sona Əlavə Etmək):**
    `push()` metodu massivin sonuna bir və ya daha çox dəyər əlavə edir. Bu metod massivin yeni uzunluğunu qaytarır.

    ```javascript
    let a = [];
    a.push("sıfır");         // Sona "sıfır" əlavə edir. a = ["sıfır"]
    console.log(a);          // => ["sıfır"]

    a.push("bir", "iki");    // Sona "bir" və "iki" əlavə edir. a = ["sıfır", "bir", "iki"]
    console.log(a);          // => ["sıfır", "bir", "iki"]
    ```
    `push()` ilə bir dəyər əlavə etmək, `a[a.length] = dəyər` yazmaqla eynidir.

3.  **`unshift()` Metodu ilə (Əvvələ Əlavə Etmək):**
    `unshift()` metodu massivin əvvəlinə bir dəyər əlavə edir, mövcud elementləri isə daha yüksək indekslərə sürüşdürür. Bu metod haqqında daha ətraflı §7.8-də danışacağıq.

### Element Silmək (Deleting Elements)

1.  **`delete` Operatoru ilə:**
    Siz obyekt xüsusiyyətlərini sildiyiniz kimi, `delete` operatoru ilə massiv elementlərini də silə bilərsiniz:

    ```javascript
    let a = [1, 2, 3];
    delete a[2]; // İndeks 2-dəki elementi silir
    console.log(a);      // => [1, 2, <1 empty item>] (element 3 silindi, yerində boşluq var)
    console.log(2 in a); // => false: 2 indeksində artıq element təyin olunmayıb
    console.log(a.length); // => 3: 'delete' massivin uzunluğunu dəyişmir!
    ```
    Bir massiv elementini `delete` ilə silmək, həmin elementə `undefined` təyin etməkdən fərqlidir. `delete` istifadə edildikdə, massiv **seyrək** olur, yəni silinən yer boş qalır və ondan sonrakı elementlər aşağı sürüşmür.

2.  **`pop()` Metodu ilə (Sondan Silmək):**
    `pop()` metodu massivin son elementini silir və onu qaytarır. Bu, massivin uzunluğunu 1 vahid azaldır. Bu barədə daha ətraflı §7.8-də danışacağıq.

3.  **`shift()` Metodu ilə (Əvvəldən Silmək):**
    `shift()` metodu massivin ilk elementini silir və onu qaytarır. Bu da massivin uzunluğunu 1 vahid azaldır və bütün digər elementləri bir indeks aşağı sürüşdürür. Bu barədə daha ətraflı §7.8-də danışacağıq.

4.  **`length` Xüsusiyyətini Dəyişdirməklə:**
    Massivin sonunda elementləri silməyin başqa bir yolu, `length` xüsusiyyətinin dəyərini istədiyiniz yeni uzunluğa təyin etməkdir (buna §7.4-də toxunmuşduq).

    ```javascript
    let b = [1, 2, 3, 4, 5];
    b.length = 2; // Massivin uzunluğunu 2-yə qoyur, 3, 4, 5 silinir.
    console.log(b); // => [1, 2]
    ```

5.  **`splice()` Metodu ilə (Ümumi Məqsədli Silmə/Əlavə Etmə):**
    `splice()` metodu massiv elementlərini yerləşdirmək, silmək və ya əvəz etmək üçün ümumi təyinatlı metoddur. O, `length` xüsusiyyətini dəyişdirir və lazım gəldikdə massiv elementlərini yüksək və ya aşağı indekslərə sürüşdürür. Ətraflı məlumat üçün §7.8-ə baxın.

---

# 7.6 Massivlərdə Dövr Etmək (Iterating Arrays) 🔄

Massivin hər bir elementi üzərində dövr etmək üçün bir neçə üsul var.

### 1. `for/of` Dövrü (ES6+) ✨

ES6-dan etibarən, massivlərin (və ya istənilən **iterasiya edilə bilən obyektin**) elementləri arasında dövr etməyin ən asan yolu **`for/of`** dövrüdür. Bu, daha əvvəl §5.4.4-də ətraflı izah edilmişdi.

```javascript
let herfler = [..."Salam dünya"]; // Hərflər massivi yaradırıq
let metn = "";
for (let herf of herfler) { // Hər hərfi tək-tək gəzirik
  metn += herf;
}
console.log(metn); // => "Salam dünya" (original mətni yenidən qurduq)
```
`for/of` dövrünün istifadə etdiyi daxili massiv iteratoru elementləri artan sırada qaytarır. Seyrək massivlər üçün xüsusi bir davranışı yoxdur və mövcud olmayan elementlər üçün sadəcə `undefined` qaytarır.

### 2. `for/of` ilə İndeks və Dəyəri Birlikdə Əldə Etmək (`entries()`) 📊

Əgər `for/of` dövrü istifadə edərkən həm elementin özünə, həm də onun **indeksinə** ehtiyacınız varsa, massivin **`entries()`** metodundan və **destructuring assignment**-dən istifadə edin:

```javascript
let herfler = [..."Salam"];
let herIkinci = "";
for (let [index, herf] of herfler.entries()) { // Hər indeks və hərfi birlikdə alırıq
  if (index % 2 === 0) { // Cüt indeksli hərfləri götürürük (0, 2, 4...)
    herIkinci += herf;
  }
}
console.log(herIkinci); // => "Sla"
```

### 3. `forEach()` Metodu (Funksional Yanaşma) 🚀

`forEach()` massivləri iterasiya etmək üçün funksional bir yanaşma təklif edən bir massiv metodudur, bu, `for` dövrünün yeni bir forması deyil. Massivin `forEach()` metoduna bir funksiya ötürürsünüz və `forEach()` massivin hər bir elementi üzərində sizin funksiyanızı bir dəfə çağırır:

```javascript
let boyukHerfler = "";
herfler.forEach(herf => { // Hər hərfi böyük hərflərlə boyukHerfler stringinə əlavə edir
  boyukHerfler += herf.toUpperCase();
});
console.log(boyukHerfler); // => "SALAM"
```
`forEach()` massivi ardıcıl şəkildə iterasiya edir və funksiyanıza ikinci arqument olaraq massiv indeksini də ötürür (bu bəzən faydalı ola bilər). `for/of` dövründən fərqli olaraq, `forEach()` seyrək massivlərdən xəbərdardır və **mövcud olmayan elementlər üçün funksiyanızı çağırmır.**

`forEach()` metodu haqqında daha ətraflı §7.8.1-də danışılacaq. Həmin bölmədə `map()` və `filter()` kimi xüsusi massiv iterasiyası edən metodlar da əhatə olunacaq.

### 4. Ənənəvi `for` Dövrü (Old School) 👴

Massiv elementləri arasında dövr etmək üçün köhnə, lakin etibarlı `for` dövründən də istifadə edə bilərsiniz (§5.4.3):

```javascript
let herfler = [..."JavaScript"];
let saitler = "";
for (let i = 0; i < herfler.length; i++) { // Massivdəki hər indeks üçün
  let herf = herfler[i];                     // Həmin indeksdəki elementi alırıq
  if (/[aeiouəiöü]/.test(herf.toLowerCase())) { // Sait olub olmadığını yoxlayırıq (kiçik hərflərlə müqayisə üçün)
    saitler += herf;                         // Əgər saitdirsə, onu yadda saxlayırıq
  }
}
console.log(saitler); // => "aai"
```

* **Performans İpucları (Nadir Hallarda):**
    İç-içə dövrlər kimi performansın kritik olduğu yerlərdə, bəzən massivin uzunluğu hər iterasiyada deyil, yalnız bir dəfə alınaraq yerli bir dəyişkəndə saxlanılır. Müasir JavaScript interpretatorları ilə bunun performans fərqi yaratdığı aydın deyil, lakin bu yanaşmalar da var:

    ```javascript
    // Massivin uzunluğunu yerli dəyişəndə saxlayın
    for (let i = 0, len = herfler.length; i < len; i++) {
      // dövrün gövdəsi
    }

    // Massivi sondan əvvələ doğru iterasiya edin
    for (let i = herfler.length - 1; i >= 0; i--) {
      // dövrün gövdəsi
    }
    ```

* **Seyrək Massivlərdə Nəzərə Alınmalı:**
    Yuxarıdakı `for` dövrü nümunələri massivin sıx olduğunu və bütün elementlərin etibarlı məlumat ehtiva etdiyini fərz edir. Əgər belə deyilsə (massiv seyrəkdirsə və ya `undefined` elementlər varsa), elementləri istifadə etməzdən əvvəl onları yoxlamalısınız. `undefined` və mövcud olmayan elementləri ötürmək istəyirsinizsə, belə yaza bilərsiniz:

    ```javascript
    let a = [1,,3, undefined, 5]; // Seyrək və undefined elementli massiv
    for (let i = 0; i < a.length; i++) {
      if (!(i in a)) continue; // Mövcud olmayan elementləri ötürür (sparse)
      if (a[i] === undefined) continue; // undefined dəyərli elementləri ötürür
      // dövrün gövdəsi burada
      console.log(`İndeks ${i}: ${a[i]}`);
    }
    // Nəticə:
    // İndeks 0: 1
    // İndeks 2: 3
    // İndeks 4: 5
    ```

---

# 7.7 Çoxölçülü Massivlər (Multidimensional Arrays) 📊

JavaScript birbaşa olaraq "həqiqi çoxölçülü massivləri" dəstəkləmir. Lakin siz **massivlərin massivlərini** (yəni bir massivin elementləri başqa massivlər olan) yaratmaqla bunu təxmin edə bilərsiniz. Bu üsulla, iki, üç və ya daha çox ölçülü massivlər kimi davranan strukturlar qura bilərsiniz.

* **Dəyərə Daxil Olmaq:** Bir massivlər massivindəki bir dəyərə daxil olmaq üçün sadəcə **`[]` operatorunu ardıcıl olaraq iki dəfə** istifadə edirsiniz. Məsələn, əgər `matrix` dəyişəni ədədlər massivlərinin massividirsə, `matrix[x]` bu massivdəki hər hansı bir sıranı (özü də bir massiv olan) təmsil edir. Bu sıradakı müəyyən bir ədədə daxil olmaq üçün `matrix[x][y]` yazarsınız.

---

### Konkret Nümunə: Vurma Cədvəli ✖️

Gəlin, iki ölçülü bir massivi vurma cədvəli kimi necə istifadə edəcəyimizə dair bir nümunəyə baxaq:

```javascript
// Çoxölçülü massiv yaradırıq
let table = new Array(10); // 10 sıradan ibarət massiv yaradırıq (hələlik boş sətirlər)
// console.log(table); // => [ <10 empty items> ]

// Hər bir sıraya 10 sütundan ibarət bir massiv təyin edirik
for (let i = 0; i < table.length; i++) {
  table[i] = new Array(10); // Hər sıra üçün 10 sütunlu massiv (boş sütunlar)
}
// console.log(table);
/*
 => [
   [ <10 empty items> ],
   [ <10 empty items> ],
   ...
 ]
*/

// Massivi dəyərlərlə doldururuq (vurma cədvəli)
for (let row = 0; row < table.length; row++) {      // Hər sıranı gəzirik (0-dan 9-a qədər)
  for (let col = 0; col < table[row].length; col++) { // Hər sıradakı hər sütunu gəzirik (0-dan 9-a qədər)
    table[row][col] = row * col; // Həmin indeksdəki dəyərə sıra * sütun hasilini yazırıq
  }
}
// console.log(table);
/*
 => [
   [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
   [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
   [0, 2, 4, 6, 8, 10, 12, 14, 16, 18],
   ...
 ]
*/

// Çoxölçülü massivdən istifadə edərək 5 * 7 hasilini tapırıq
// table[5][7] massivin 5-ci sırasının (row) 7-ci sütunundakı (col) dəyəri deməkdir.
console.log(table[5][7]); // => 35 (5 * 7-nin nəticəsi)
```

Bu nümunə göstərir ki, massivlərin massivlərindən istifadə edərək, mürəkkəb cədvəlvari və ya matrisvari məlumatları JavaScript-də necə effektiv şəkildə saxlamaq və onlara daxil olmaq olar.

---

# 7.8 Massiv Metodları ⚙️

Əvvəlki hissələr massivlərlə işləmək üçün əsas JavaScript sintaksisinə fokuslanırdı. Ümumiyyətlə, ən güclü olanlar **`Array` sinfi tərəfindən təyin olunan metodlardır**. Bu metodları oxuyarkən, bəzilərinin çağırıldığı massivi dəyişdirdiyini (modify), bəzilərinin isə dəyişdirmədiyini yadda saxlamaq vacibdir.

* Bəzi metodlar yeni bir massiv qaytarır və orijinal massiv dəyişməz qalır.
* Digər metodlar isə massivi yerində dəyişdirir (`in place`) və dəyişdirilmiş massivə istinad qaytarır.

Aşağıdakı bölmələrdə əlaqəli massiv metod qruplarına baxacağıq:

* **İterator metodları:** Massiv elementləri üzərində dövr edir.
* **Stak və Növbə metodları:** Massivə başlanğıcdan və ya sondan element əlavə edir və ya silir.
* **Alt-massiv metodları:** Massivin ardıcıl hissələrini çıxarmaq, silmək, yerləşdirmək, doldurmaq və kopyalamaq üçündür.
* **Axtarış və Sıralama metodları:** Massiv daxilində elementləri tapmaq və ya massiv elementlərini sıralamaq üçündür.
* Həmçinin `Array` sinfinin statik metodlarına və massivləri birləşdirmək, stringə çevirmək üçün bir neçə digər metoda da toxunacağıq.

---

## 7.8.1 Massiv İterator Metodları 🚶‍♂️🚶‍♀️

Bu hissədəki metodlar massiv elementlərini ardıcıl olaraq sizin təqdim etdiyiniz bir funksiyaya ötürərək massivlər üzərində dövr edir. Onlar massivləri gəzmək, xəritələmək (`map`), filtrləmək (`filter`), test etmək (`test`) və yığmaq (`reduce`) üçün rahat yollar təqdim edir.

**Ümumi Xüsusiyyətlər:**

* Bütün bu metodlar ilk arqument olaraq bir **funksiya** qəbul edir və massivin hər bir elementi (və ya bəzi elementləri) üçün bu funksiyanı çağırır.
* Əgər massiv seyrəkdirsə (sparse), təqdim etdiyiniz funksiya **mövcud olmayan elementlər üçün çağırılmır**.
* Əksər hallarda, sizin funksiyanız **üç arqumentlə** çağırılır:
    1.  Massiv elementinin **dəyəri**.
    2.  Massiv elementinin **indeksi**.
    3.  Massivin **özü** (bütün massiv).
    * Çox vaxt yalnız ilk arqument (dəyər) sizə lazım olur.
* Bu iterator metodlarının əksəriyyəti **ixtiyari ikinci arqument** qəbul edir. Əgər bu arqument təyin olunarsa, funksiya bu ikinci arqumentin bir metodu kimi çağırılır. Yəni, bu ikinci arqument funksiyanın daxilindəki `this` açar sözünün dəyərinə çevrilir.
* Sizin funksiyanızın qaytardığı dəyər adətən vacibdir, lakin fərqli metodlar bu dəyəri fərqli şəkildə idarə edir.
* Burada təsvir olunan metodların heç biri çağırıldığı massivi **dəyişdirmir** (baxmayaraq ki, sizin ötürdüyünüz funksiya massivi dəyişdirə bilər).
* Bu funksiyalar çox vaxt metod çağırışının bir hissəsi olaraq **inline (yerində)** təyin olunur. **Arrow function** sintaksisi (ox funksiyaları, §8.1.3) bu metodlarla xüsusilə yaxşı işləyir və biz misallarda ondan istifadə edəcəyik.

---

### `forEach()` Metodu ✅

`forEach()` metodu massiv üzərində dövr edir və hər bir element üçün təyin etdiyiniz funksiyanı çağırır. Funksiya elementin dəyəri, indeksi və massivin özü ilə çağırılır. Əgər sizə yalnız dəyər lazımdırsa, bir parametrli funksiya yaza bilərsiniz.

```javascript
let data = [1, 2, 3, 4, 5];
let cem = 0;

// Massiv elementlərinin cəmini hesablayırıq
data.forEach(value => { // Yalnız dəyər lazımdır, digər arqumentlər gözardı edilir
  cem += value;
});
console.log(cem); // => 15

// Hər massiv elementini 1 vahid artırırıq
data.forEach(function(v, i, a) { // Dəyər (v), İndeks (i), Massiv (a)
  a[i] = v + 1; // Massivi dəyişdiririk
});
console.log(data); // => [2, 3, 4, 5, 6]
```
**Vacib Qeyd:** `forEach()` dövrü `for` dövründəki `break` statementi kimi iterasiyanı vaxtından əvvəl dayandırmağa imkan vermir. Bütün elementlər funksiyaya ötürüləcək.

---

### `map()` Metodu 🗺️

`map()` metodu çağırıldığı massivin hər bir elementini sizin təyin etdiyiniz funksiyaya ötürür və **həmin funksiyanın qaytardığı dəyərləri ehtiva edən yeni bir massiv** qaytarır.

```javascript
let a = [1, 2, 3];
let kvadratlar = a.map(x => x * x); // Hər x-i götürüb x*x qaytarır
console.log(kvadratlar);          // => [1, 4, 9]
console.log(a);                   // => [1, 2, 3] (Original massiv dəyişməyib!)
```
`map()` metoduna ötürdüyünüz funksiya `forEach()`-də olduğu kimi çağırılır, lakin o, mütləq bir dəyər qaytarmalıdır. `map()` **həmişə yeni bir massiv qaytarır** və çağırıldığı massivi dəyişdirmir. Əgər original massiv seyrəkdirsə, qaytarılan massiv də eyni seyrəkliyə sahib olacaq (eyni uzunluğa və eyni boş elementlərə malik olacaq).

---

### `filter()` Metodu 🧪

`filter()` metodu çağırıldığı massivin elementlərinin bir **alt çoxluğunu** (subset) ehtiva edən yeni bir massiv qaytarır. Bu metoda ötürdüyünüz funksiya bir **predikat** olmalıdır: `true` və ya `false` qaytaran bir funksiya. Predikat `forEach()` və `map()`-də olduğu kimi çağırılır. Əgər predikat `true` qaytararsa (və ya `true`-ya çevrilə bilən bir dəyər), həmin element alt çoxluğa əlavə olunur və qaytarılan massivin bir hissəsi olur.

```javascript
let a = [5, 4, 3, 2, 1];
let ucdenKicikler = a.filter(x => x < 3); // Dəyəri 3-dən kiçik olanları seçir
console.log(ucdenKicikler);            // => [2, 1]

let cutIndeksler = a.filter((x, i) => i % 2 === 0); // İndeksi cüt olanları seçir
console.log(cutIndeksler);             // => [5, 3, 1] (indeks 0, 2, 4)
```
**Vacib Qeyd:** `filter()` seyrək massivlərdəki boş elementləri ötürür və onun qaytardığı massiv **həmişə sıx (dense)** olur.

* Seyrək massivdəki boşluqları bağlamaq üçün:
    ```javascript
    let seyrekmassiv = [1,,3,,5];
    let sixmassiv = seyrekmassiv.filter(() => true); // Bütün mövcud elementləri götürür
    console.log(sixmassiv); // => [1, 3, 5]
    ```
* Boşluqları, `undefined` və `null` elementləri bağlamaq və silmək üçün:
    ```javascript
    let b = [1, undefined, 2, null, 3, , 4];
    b = b.filter(x => x !== undefined && x !== null);
    console.log(b); // => [1, 2, 3, 4]
    ```

---

### `find()` və `findIndex()` Metodları 🔍

`find()` və `findIndex()` metodları `filter()`-ə bənzəyir, çünki onlar massivdə sizin predikat funksiyanızın `true` qaytardığı elementləri axtarır. Lakin `filter()`-dən fərqli olaraq, bu iki metod predikat ilk dəfə uyğun bir element tapdıqda dövr etməyi dayandırır.

* **`find()`:** Uyğun gələn elementi qaytarır. Əgər heç bir element tapılmazsa, `undefined` qaytarır.
* **`findIndex()`:** Uyğun gələn elementin **indeksini** qaytarır. Əgər heç bir element tapılmazsa, `-1` qaytarır.

```javascript
let a = [1, 2, 3, 4, 5];

let ucunIndeksi = a.findIndex(x => x === 3); // Dəyəri 3 olan elementin indeksini tapır
console.log(ucunIndeksi);                  // => 2

let menfiEdedIndeksi = a.findIndex(x => x < 0); // Massivdə mənfi ədəd yoxdur
console.log(menfiEdedIndeksi);             // => -1

let besinMiskili = a.find(x => x % 5 === 0); // 5-ə bölünən ilk ədədi tapır
console.log(besinMiskili);                 // => 5

let yeddininMiskili = a.find(x => x % 7 === 0); // Massivdə 7-yə bölünən yoxdur
console.log(yeddininMiskili);              // => undefined
```

---

### `every()` və `some()` Metodları ✅❌

`every()` və `some()` metodları massiv predikatlarıdır: onlar sizin təyin etdiyiniz predikat funksiyasını massiv elementlərinə tətbiq edir, sonra `true` və ya `false` qaytarır.

* **`every()`:** Riyazi "hər şey üçün" (`∀`) kəmiyyətçisi kimidir: yalnız və yalnız sizin predikat funksiyanız massivdəki **bütün elementlər üçün `true` qaytararsa**, `true` qaytarır.

    ```javascript
    let a = [1, 2, 3, 4, 5];
    let ondanKicikdir = a.every(x => x < 10);      // Bütün dəyərlər 10-dan kiçikdirmi?
    console.log(ondanKicikdir);                    // => true

    let cutEdedler = a.every(x => x % 2 === 0);    // Bütün dəyərlər cütdürmü?
    console.log(cutEdedler);                       // => false (1, 3, 5 təkdir)
    ```

* **`some()`:** Riyazi "mövcuddur" (`∃`) kəmiyyətçisi kimidir: əgər massivdə predikatın `true` qaytardığı **ən azı bir element mövcuddursa**, `true` qaytarır. Yalnız və yalnız predikat massivin bütün elementləri üçün `false` qaytararsa, `false` qaytarır.

    ```javascript
    let a = [1, 2, 3, 4, 5];
    let beziCutEdedler = a.some(x => x % 2 === 0); // Massivdə bəzi cüt ədədlər varmı?
    console.log(beziCutEdedler);                   // => true (2 və 4 var)

    let reqemDeyil = a.some(isNaN);                // Massivdə rəqəm olmayan dəyər varmı? (isNaN = is Not a Number)
    console.log(reqemDeyil);                       // => false (hamısı rəqəmdir)
    ```
* **Dövrü Dayandırma Xüsusiyyəti:** Həm `every()`, həm də `some()` qaytaracaqları dəyəri bildikləri anda massiv elementləri üzərində dövr etməyi dayandırırlar.
    * `some()` sizin predikatınız ilk dəfə `true` qaytaran kimi `true` qaytarır və yalnız predikatınız həmişə `false` qaytararsa, bütün massivi gəzir.
    * `every()` isə bunun əksidir: sizin predikatınız ilk dəfə `false` qaytaran kimi `false` qaytarır və yalnız predikatınız həmişə `true` qaytararsa, bütün elementləri gəzir.
* **Boş Massivlər:** Riyazi konvensiyaya görə, `every()` boş massivdə çağırılanda `true`, `some()` isə `false` qaytarır.

---

### `reduce()` və `reduceRight()` Metodları 📉📈

`reduce()` və `reduceRight()` metodları massivin elementlərini, sizin təyin etdiyiniz bir funksiyadan istifadə edərək, **tək bir dəyər** əldə etmək üçün birləşdirir. Bu, funksional proqramlaşdırmada tez-tez rast gəlinən bir əməliyyatdır və "inject" və ya "fold" kimi də tanınır.

**Misallar:**

```javascript
let a = [1, 2, 3, 4, 5];

// Elementlərin cəmi
let cem = a.reduce((akkumulyator, cariDeyer) => akkumulyator + cariDeyer, 0); // Başlanğıc dəyəri 0
console.log(cem); // => 15

// Elementlərin hasili
let hasil = a.reduce((akkumulyator, cariDeyer) => akkumulyator * cariDeyer, 1); // Başlanğıc dəyəri 1
console.log(hasil); // => 120

// Ən böyük dəyər
let enBoyuk = a.reduce((akkumulyator, cariDeyer) => (akkumulyator > cariDeyer) ? akkumulyator : cariDeyer); // Başlanğıc dəyəri yoxdur
console.log(enBoyuk); // => 5
```

* **`reduce()` Arqumentləri:**
    `reduce()` iki arqument qəbul edir:
    1.  **Reduksiya funksiyası (callback):** Bu funksiya iki dəyəri (əvvəlki nəticəni və cari elementi) birləşdirərək tək bir dəyər qaytarır.
    2.  **İlkin dəyər (initial value, ixtiyari):** Bu, reduksiya funksiyasına ilk çağırışda birinci arqument kimi ötürülən dəyərdir. Əgər verilməzsə, massivin **ilk elementi** ilkin dəyər kimi istifadə olunur və reduksiya funksiyası massivin ikinci elementi ilə başlayır.

* **Reduksiya Funksiyasının Çağırılması:**
    `reduce()` ilə istifadə olunan funksiyalar `forEach()` və `map()` ilə istifadə olunanlardan fərqlidir.
    * Birinci arqument: **Akkumulyasiya olunmuş nəticə** (reduction so far).
    * İkinci arqument: Massiv elementinin **dəyəri**.
    * Üçüncü arqument: Massiv elementinin **indeksi**.
    * Dördüncü arqument: Massivin **özü**.

    Yuxarıdakı cəmləmə nümunəsində (`a.reduce((x,y) => x+y, 0)`):
    * İlk çağırış: `x = 0` (ilkin dəyər), `y = 1` (massivin ilk elementi). Qaytarır `1`.
    * İkinci çağırış: `x = 1` (əvvəlki nəticə), `y = 2` (massivin ikinci elementi). Qaytarır `3`.
    * Daha sonra `x = 3, y = 3` qaytarır `6`, sonra `x = 6, y = 4` qaytarır `10`, nəhayət `x = 10, y = 5` qaytarır `15`. Bu son dəyər `reduce()`-un qaytardığı dəyər olur.

* **İlkin Dəyər Olmadan `reduce()`:**
    Əgər `reduce()`-u ilkin dəyər arqumenti olmadan çağırırsınızsa, massivin **ilk elementi** ilkin dəyər kimi istifadə olunur. Bu halda, reduksiya funksiyasına ilk çağırışda massivin birinci və ikinci elementləri ilk və ikinci arqumentlər kimi ötürülür.

    ```javascript
    let b = [10, 20, 5];
    let max = b.reduce((a, b) => (a > b) ? a : b); // İlk dəyər 10 olur, sonra 20 ilə müqayisə olunur, sonra 5 ilə.
    console.log(max); // => 20
    ```
    **Boş massivdə ilkin dəyər olmadan `reduce()` çağırmaq `TypeError` səhvinə səbəb olur.**
    Əgər massivdə bir element varsa və ya massiv boşdursa, lakin ilkin dəyər təyin olunubsa, `reduce()` reduksiya funksiyasını heç vaxt çağırmadan həmin bir dəyəri qaytarır.

* **`reduceRight()`:**
    `reduceRight()` `reduce()` ilə tamamilə eyni işləyir, lakin massivi ən yüksək indeksdən ən aşağıya (sağdan-sola) emal edir. Bu, reduksiya əməliyyatının sağdan-sola assosiativliyi olduğu hallarda (məsələn, üstləmə kimi) faydalı ola bilər:

    ```javascript
    // 2^(3^4) hesablayırıq. Üstləmə sağdan-sola prioritetə malikdir.
    let a = [2, 3, 4];
    // reduceRight: (4) alınır, sonra 3^(4), sonra 2^(3^4)
    let netice = a.reduceRight((akkumulyator, cariDeyer) => Math.pow(cariDeyer, akkumulyator));
    console.log(netice); // => 2.4178516392292583e+24 (yəni 2^(3^4) = 2^81 = bu böyük rəqəm)
    ```
* **Qeyd:** Nə `reduce()`, nə də `reduceRight()` reduksiya funksiyasının hansı `this` dəyəri ilə çağırılacağını təyin edən ixtiyari bir arqument qəbul etmir. Onun yerini ilkin dəyər arqumenti tutur. Əgər reduksiya funksiyanızın müəyyən bir obyektin metodu kimi çağırılmasına ehtiyacınız varsa, `Function.bind()` metodundan (§8.7.5) istifadə edə bilərsiniz.

`reduce()` və `reduceRight()` yalnız riyazi hesablamalar üçün nəzərdə tutulmayıb. İki dəyəri (məsələn, iki obyekti) eyni tipdə bir dəyərə birləşdirə bilən istənilən funksiya reduksiya funksiyası kimi istifadə edilə bilər. Lakin, massiv reduksiyalarından istifadə edərək ifadə edilən alqoritmlər tez bir zamanda mürəkkəbləşə və başa düşülməsi çətinləşə bilər, buna görə də bəzən massivləri emal etmək üçün adi dövr konstruksiyalarından istifadə etmək kodunuzu oxumaq, yazmaq və başa düşmək üçün daha asan ola bilər.

---

# 7.8.2 Massivləri Düzəltmək (Flattening Arrays) `flat()` və `flatMap()` Metodları ilə

ES2019-da təqdim olunan bu metodlar mürəkkəb (iç-içə) massivləri daha sadə, tək səviyyəli massivlərə çevirmək üçün istifadə olunur.

### `flat()` Metodu

`flat()` metodu çağırıldığı massivdəki elementləri ehtiva edən **yeni bir massiv (new array)** yaradır və qaytarır. Lakin, original massivdəki **massiv olan elementlər** (nested arrays) qaytarılan massivin daxilinə "düzəldilir" (flattened).

**Misallar:**

```javascript
[1, [2, 3]].flat();    // => [1, 2, 3]
[1, [2, [3]]].flat(); // => [1, 2, [3]] (yalnız bir səviyyə düzəldir)
```

* **Dərinlik (Depth):** Arqumentsiz çağırılanda, `flat()` **bir səviyyə (one level)** iç-içəlikləri düzəldir. Orijinal massivin özü massiv olan elementləri düzəldilir, lakin həmin massivlərin daxilindəki massivlər düzəldilmir. Əgər daha çox səviyyəni düzəltmək istəyirsinizsə, `flat()` metoduna bir rəqəm (dərinlik dərəcəsi) ötürün:

    ```javascript
    let a = [1, [2, [3, [4]]]];

    a.flat(1); // => [1, 2, [3, [4]]] (1 səviyyə düzəldildi)
    a.flat(2); // => [1, 2, 3, [4]]   (2 səviyyə düzəldildi)
    a.flat(3); // => [1, 2, 3, 4]     (3 səviyyə düzəldildi)
    a.flat(4); // => [1, 2, 3, 4]     (4 səviyyə düzəldildi, artıq düzəldiləcək bir şey yoxdur)
    ```

### `flatMap()` Metodu

`flatMap()` metodu `map()` metodu kimi işləyir (bax "7.8.1 Array Iterator Methods" hissəsinə), lakin əlavə olaraq qaytarılan massiv avtomatik olaraq `flat()` metoduna ötürüldüyü kimi "düzəldilir" (flattened). Yəni, `a.flatMap(f)` çağırmaq `a.map(f).flat()` ilə eynidir, lakin daha **effektivdir (more efficient)**.

```javascript
let phrases = ["salam dünya", "JavaScript bələdçisi"];
let words = phrases.flatMap(phrase => phrase.split(" ")); // Hər cümləni boşluqlara görə ayırır və nəticəni düzəldir
console.log(words); // => ["salam", "dünya", "JavaScript", "bələdçisi"]
```

`flatMap()`-i, giriş massivinin hər bir elementinin çıxış massivinin istənilən sayda elementinə **xəritələməyə (map)** imkan verən `map()` metodunun ümumiləşdirilmiş (generalization) forması kimi düşünə bilərsiniz. Xüsusilə, `flatMap()` giriş elementlərini **boş bir massivə (empty array)** xəritələməyə imkan verir ki, bu da çıxış massivində heç bir şeyə düzəldilir:

```javascript
// Mənfi olmayan ədədləri onların kvadrat köklərinə çevirir (map non-negative numbers to their square roots)
[-2, -1, 1, 2, 3].flatMap(x => x < 0 ? [] : Math.sqrt(x)); // => [1, 1.4142135623730951, 1.73205081037989]
// -2 və -1 üçün [] (boş massiv) qaytarıldığından, bunlar son nəticədə yer almır.
```

---

# 7.8.3 Massivləri Birləşdirmək (Adding Arrays) `concat()` Metodu ilə

`concat()` metodu, çağırıldığı orijinal massivin elementlərini və ardınca `concat()`-a verilən hər bir arqumenti ehtiva edən **yeni bir massiv (new array)** yaradır və qaytarır. Əgər bu arqumentlərdən hər hansı biri özü bir massivdirsə, o zaman massivin özü deyil, **massiv elementləri** birləşdirilir (concatenated).

**Qeyd:** `concat()` massivlərin massivlərini rekursiv olaraq düzəltmir (does not recursively flatten). `concat()` çağırıldığı massivi dəyişdirmir (does not modify the array on which it is invoked).

**Misallar:**

```javascript
let a = [1, 2, 3];

let yeni1 = a.concat(4, 5);      // => [1, 2, 3, 4, 5]
console.log(yeni1);

let yeni2 = a.concat([4, 5], [6, 7]); // Massivlər düzəldilir (flattened)
console.log(yeni2);             // => [1, 2, 3, 4, 5, 6, 7]

let yeni3 = a.concat(4, [5, [6, 7]]); // Lakin iç-içə massivlər düzəldilmir (not nested arrays)
console.log(yeni3);             // => [1, 2, 3, 4, 5, [6, 7]]

console.log(a);                 // => [1, 2, 3] (orijinal massiv dəyişdirilməyib!)
```

**Vacib Qeyd:** `concat()` çağırıldığı massivin **yeni bir kopyasını (new copy)** yaradır. Bir çox hallarda bu doğru davranışdır, lakin bu, **baha başa gələn bir əməliyyatdır (expensive operation)**. Əgər kodunuzda `a = a.concat(x)` kimi bir şey yazırsınızsa, o zaman yeni bir massiv yaratmaq əvəzinə, massivinizi **yerində dəyişdirmək (modify in place)** üçün `push()` (bax "7.5 Adding and Deleting Array Elements" hissəsinə) və ya `splice()` (bax "7.8.5 Subarray Methods" hissəsinə) metodlarından istifadə etməyi düşünməlisiniz.

---

# 7.8.4 Staklar (Stacks) və Növbələr (Queues) `push()`, `pop()`, `shift()` və `unshift()` Metodları ilə 📦 FIFO / LIFO

`push()` və `pop()` metodları sizə massivlərlə **stak (stack)** kimi işləməyə imkan verir. Stak "Last-In, First-Out" (LIFO) prinsipi ilə işləyən bir məlumat strukturudur.

* **`push()` metodu:** Bir və ya daha çox yeni elementi massivin sonuna əlavə edir (appends) və massivin yeni uzunluğunu (new length) qaytarır.
    * `concat()`-dan fərqli olaraq, `push()` massiv arqumentlərini düzəltmir (does not flatten array arguments). Yəni, bir massivi `push()` etsəniz, həmin massiv tək bir element kimi daxil edilir.

* **`pop()` metodu:** Əksini edir: massivin son elementini silir (deletes), massivin uzunluğunu bir vahid azaldır (decrements length) və sildiyi dəyəri qaytarır (returns the value).

Hər iki metod massivi yerində dəyişdirir (modify the array in place). `push()` və `pop()` kombinasiyası sizə JavaScript massivi ilə bir **"Last-In, First-Out" (LIFO) stak** tətbiq etməyə imkan verir.

**Misal (Stak):**

```javascript
let stack = [];     // stack == []
stack.push(1, 2);   // stack == [1, 2];
console.log(stack.pop());    // stack == [1]; qaytarır 2
console.log(stack); // => [1]

stack.push(3);      // stack == [1, 3]
console.log(stack.pop());    // stack == [1]; qaytarır 3
console.log(stack); // => [1]

stack.push([4, 5]); // stack == [1, [4, 5]] (massiv düzəldilmir!)
console.log(stack.pop());    // stack == [1]; qaytarır [4, 5]
console.log(stack); // => [1]

console.log(stack.pop());    // stack == []; qaytarır 1
console.log(stack); // => []
```

`push()` metodu ona ötürdüyünüz massivi düzəltmir, lakin əgər bir massivdən bütün elementləri başqa bir massivə əlavə etmək istəyirsinizsə, bunu açıq şəkildə düzəltmək üçün **spread operatorundan (`...`)** istifadə edə bilərsiniz (§8.3.4):

```javascript
let a = [1, 2];
let values = [3, 4, 5];
a.push(...values); // 'values' massivinin elementlərini 'a' massivinin sonuna əlavə edir.
console.log(a);   // => [1, 2, 3, 4, 5]
```

---

### `unshift()` və `shift()` Metodları

`unshift()` və `shift()` metodları `push()` və `pop()` kimi davranır, lakin onlar elementləri massivin sonundan deyil, **başlanğıcından (beginning)** əlavə edir və silir.

* **`unshift()`:** Massivin əvvəlinə bir və ya daha çox element əlavə edir, mövcud massiv elementlərini yer açmaq üçün daha yüksək indekslərə sürüşdürür (shifts up) və massivin yeni uzunluğunu qaytarır.
* **`shift()`:** Massivin ilk elementini silir (removes) və qaytarır. Bütün sonrakı elementləri massivin əvvəlindəki yeni boş yerə sürüşdürür (shifts down).

Siz `unshift()` və `shift()` metodlarından bir stak tətbiq etmək üçün istifadə edə bilərsiniz, lakin bu, `push()` və `pop()` istifadə etməkdən daha az effektiv (less efficient) olardı. Çünki massivə element əlavə edildikdə və ya silindikdə elementlər hər dəfə yuxarı və ya aşağı sürüşdürülməlidir.

Bunun əvəzinə, siz **növbə (queue) məlumat strukturunu** massivin sonuna elementlər əlavə etmək üçün `push()` və başlanğıcından elementləri silmək üçün `shift()` istifadə edərək tətbiq edə bilərsiniz. Bu, "First-In, First-Out" (FIFO) prinsipidir.

**Misal (Növbə):**

```javascript
let q = [];     // q == []
q.push(1, 2);   // q == [1, 2] (növbənin sonuna əlavə edilir)
console.log(q.shift());     // q == [2]; qaytarır 1 (növbənin əvvəlindən silinir)
console.log(q); // => [2]

q.push(3);      // q == [2, 3]
console.log(q.shift());     // q == [3]; qaytarır 2
console.log(q); // => [3]

console.log(q.shift());     // q == []; qaytarır 3
console.log(q); // => []
```

`unshift()` metodunun diqqətə layiq bir xüsusiyyəti var ki, sizi təəccübləndirə bilər: Birdən çox arqumenti `unshift()`-ə ötürəndə, onlar eyni anda daxil edilir. Bu o deməkdir ki, onlar massivdə tək-tək daxil edildikləri zaman olduğundan fərqli qaydada yer alırlar:

```javascript
let a = [];    // a == []
a.unshift(1);  // a == [1]
a.unshift(2);  // a == [2, 1] (yeni element həmişə ən əvvələ gəlir)
console.log(a); // => [2, 1]

let b = [];    // b == []
b.unshift(1, 2); // a == [1, 2] (1 və 2 birlikdə daxil edilir, orijinal qaydada)
console.log(b);  // => [1, 2]
```

---

# 7.8.5 Alt-massivlər (Subarrays) `slice()`, `splice()`, `fill()` və `copyWithin()` Metodları ilə ✂️📝

Massivlər, ardıcıl bölgələrdə, yəni massivin alt-massivləri (subarrays) və ya "dilimləri" (slices) üzərində işləyən bir sıra metodlar təyin edir. Aşağıdakı bölmələrdə dilimləri çıxarmaq (extracting), əvəz etmək (replacing), doldurmaq (filling) və kopyalamaq (copying) üçün metodlar təsvir edilir.

---

### `slice()` Metodu

`slice()` metodu təyin olunmuş massivin bir dilimini (slice) və ya alt-massivini (subarray) qaytarır. Onun iki arqumenti qaytarılacaq dilimin başlanğıcını (start) və sonunu (end) təyin edir.

* Qaytarılan massiv birinci arqumentlə təyin olunan elementi və ikinci arqumentlə təyin olunan elementə qədər (lakin daxil olmadan) bütün sonrakı elementləri ehtiva edir.
* Əgər yalnız bir arqument təyin olunarsa, qaytarılan massiv başlanğıc mövqeyindən massivin sonuna qədər bütün elementləri ehtiva edir.
* Əgər arqumentlərdən hər hansı biri mənfidirsə (negative), bu, massivin uzunluğuna nisbətən bir massiv elementini təyin edir. Məsələn, `-1` arqumenti massivdəki son elementi, `-2` arqumenti isə ondan əvvəlki elementi təyin edir.

**Qeyd:** `slice()` çağırıldığı massivi dəyişdirmir (does not modify the array).

**Misallar:**

```javascript
let a = [1, 2, 3, 4, 5];

a.slice(0, 3);    // => [1, 2, 3] (indeks 0-dan 3-ə qədər, 3 daxil deyil)
a.slice(3);       // => [4, 5] (indeks 3-dən massivin sonuna qədər)
a.slice(1, -1);   // => [2, 3, 4] (indeks 1-dən sondan birinciyə qədər, sonuncu daxil deyil)
a.slice(-3, -2);  // => [3] (sondan 3-cü elementdən sondan 2-ci elementə qədər, 2-ci daxil deyil)
```

---

### `splice()` Metodu

`splice()` massivdən elementləri daxil etmək (inserting) və ya silmək (removing) üçün ümumi təyinatlı metoddur. `slice()` və `concat()`-dan fərqli olaraq, `splice()` çağırıldığı massivi **dəyişdirir (modifies the array)**. `splice()` və `slice()` adları çox oxşardır, lakin əsaslı şəkildə fərqli əməliyyatlar yerinə yetirirlər.

`splice()` massivdən elementləri silə, yeni elementlər daxil edə və ya hər iki əməliyyatı eyni anda yerinə yetirə bilər. Daxiletmə və ya silinmə nöqtəsindən sonra gələn massiv elementlərinin indeksləri lazım olduqda artırılır və ya azaldılır ki, massivin qalan hissəsi ilə ardıcıl qalsınlar (remain contiguous).

* `splice()`-ın **ilk arqumenti (start index)** daxiletmə və/və ya silinmənin başlayacağı massiv mövqeyini təyin edir.
* **İkinci arqument (delete count)** massivdən neçə elementin silinəcəyini (spliced out of) təyin edir. (Qeyd: bu, `slice()`-dan fərqli bir nöqtədir. `slice()`-ın ikinci arqumenti bitmə mövqeyidir. `splice()`-ın ikinci arqumenti uzunluqdur.) Əgər bu ikinci arqument buraxılarsa, başlanğıc elementdən massivin sonuna qədər bütün massiv elementləri silinir.
* `splice()` silinmiş elementlərin massivini qaytarır (returns an array of the deleted elements) və ya heç bir element silinməyibsə, boş bir massiv qaytarır.

**Silmə Misalları:**

```javascript
let a = [1, 2, 3, 4, 5, 6, 7, 8];

a.splice(4);    // => [5, 6, 7, 8]; (indeks 4-dən sonuna qədər silir)
console.log(a); // a is now [1, 2, 3, 4]

a.splice(1, 2); // => [2, 3]; (indeks 1-dən başlayaraq 2 element silir)
console.log(a); // a is now [1, 4]

a.splice(1, 1); // => [4]; (indeks 1-dən başlayaraq 1 element silir)
console.log(a); // a is now [1]
```

`splice()`-ın ilk iki arqumenti hansı massiv elementlərinin silinəcəyini təyin edir. Bu arqumentləri, ilk arqumentlə təyin olunmuş mövqedən başlayaraq, massivə daxil ediləcək elementləri təyin edən **istənilən sayda əlavə arqumentlər** (additional arguments) izləyə bilər.

**Daxiletmə və Əvəz Etmə Misalları:**

```javascript
let a = [1, 2, 3, 4, 5];

// İndeks 2-dən başlayaraq 0 element silir, "a" və "b" daxil edir
a.splice(2, 0, "a", "b"); // => [] (heç nə silinmədi)
console.log(a);         // a is now [1, 2, "a", "b", 3, 4, 5]

// İndeks 2-dən başlayaraq 2 element silir ("a", "b"), sonra [1,2] və 3 daxil edir
a.splice(2, 2, [1, 2], 3); // => ["a", "b"] (silinən elementlər)
console.log(a);         // a is now [1, 2, [1, 2], 3, 3, 4, 5]
```
**Qeyd:** `concat()`-dan fərqli olaraq, `splice()` massivlərin **özlərini (arrays themselves)** daxil edir, həmin massivlərin elementlərini deyil.

---

### `fill()` Metodu

`fill()` metodu massivin elementlərini və ya massivin bir dilimini (slice) təyin olunmuş bir dəyərə təyin edir. O, çağırıldığı massivi **mutasiya edir (mutates the array)** və həmçinin dəyişdirilmiş massivi qaytarır.

* `fill()`-ın **ilk arqumenti** massiv elementlərini təyin ediləcək dəyərdir.
* **İxtiyari ikinci arqument (start index)** başlanğıc indeksini təyin edir. Buraxılarsa, doldurma indeks 0-dan başlayır.
* **İxtiyari üçüncü arqument (end index)** bitmə indeksini təyin edir — bu indeksə qədər (lakin daxil olmadan) massiv elementləri doldurulacaq. Bu arqument buraxılarsa, massiv başlanğıc indeksdən sona qədər doldurulur. `slice()`-da olduğu kimi mənfi rəqəmlər (negative numbers) ötürərək indeksləri massivin sonuna nisbətən təyin edə bilərsiniz.

**Misallar:**

```javascript
let a = new Array(5); // Heç bir elementi olmayan, uzunluğu 5 olan massivlə başlayır
console.log(a);       // => [ <5 empty items> ]

a.fill(0);            // => [0, 0, 0, 0, 0]; massivi sıfırlarla doldurur
console.log(a);

a.fill(9, 1);         // => [0, 9, 9, 9, 9]; indeks 1-dən başlayaraq 9-larla doldurur
console.log(a);

a.fill(8, 2, -1);     // => [0, 9, 8, 8, 9]; indeks 2-dən başlayaraq sondan 1-ciyə qədər (yəni indeks 2 və 3) 8-lərlə doldurur
console.log(a);
```

---

### `copyWithin()` Metodu

`copyWithin()` massivin bir dilimini (slice) massivin daxilindəki yeni bir mövqeyə kopyalayır. O, massivi **yerində dəyişdirir (modifies the array in place)** və dəyişdirilmiş massivi qaytarır, lakin massivin uzunluğunu dəyişdirmir.

* **İlk arqument (target index)** ilk elementin kopyalanacağı təyinat indeksini (destination index) təyin edir.
* **İkinci arqument (start index)** kopyalanacaq ilk elementin indeksini təyin edir. Əgər buraxılarsa, 0 istifadə olunur.
* **Üçüncü arqument (end index)** kopyalanacaq elementlər diliminin sonunu təyin edir. Əgər buraxılarsa, massivin uzunluğu istifadə olunur. Başlanğıc indeksdən bitmə indeksinə qədər (lakin daxil olmadan) elementlər kopyalanacaq. `slice()`-da olduğu kimi mənfi rəqəmlər ötürərək indeksləri massivin sonuna nisbətən təyin edə bilərsiniz.

**Misallar:**

```javascript
let a = [1, 2, 3, 4, 5];

a.copyWithin(1);        // => [1, 1, 2, 3, 4]; massiv elementlərini bir vahid yuxarı kopyalayır (target 1, start 0, end length)
console.log(a);         // 1-ci indeksdən başlayaraq 0-dan sonrakı elementlər kopyalanır.

let b = [1, 2, 3, 4, 5]; // Yenidən təyin edək ki, misal aydın olsun
b.copyWithin(2, 3, 5);  // => [1, 2, 4, 5, 5]; son 2 elementi (indeks 3 və 4) indeks 2-yə kopyalayır
console.log(b);         // Element 3 (dəyəri 4) indeks 2-yə, element 4 (dəyəri 5) indeks 3-ə kopyalanır.

let c = [1, 2, 3, 4, 5]; // Yenidən təyin edək
c.copyWithin(0, -2);    // => [4, 5, 3, 4, 5]; mənfi offsetlər də işləyir (target 0, start sondan 2-ci (3), end length)
console.log(c);         // 3-cü indeksdən sonrakı elementlər (4, 5) 0-cı indeksdən kopyalanır.
```

`copyWithin()` xüsusilə **tipli massivlər (typed arrays)** (§11.2) üçün faydalı olan yüksək performanslı (high-performance) bir metod olaraq nəzərdə tutulub. O, C standart kitabxanasındakı `memmove()` funksiyasından modelləşdirilmişdir. Qeyd edək ki, mənbə (source) və təyinat (destination) bölgələri arasında üst-üstə düşmə olsa belə, kopyalama düzgün işləyəcək.

---

# 7.8.6 Massiv Axtarış və Sıralama Metodları (Array Searching and Sorting Methods) 🔎🔄

Massivlər, `indexOf()`, `lastIndexOf()` və `includes()` kimi, stringlərin eyni adlı metodlarına bənzər metodları tətbiq edir. Həmçinin massiv elementlərinin sırasını dəyişdirmək üçün `sort()` və `reverse()` metodları da mövcuddur.

### `indexOf()` və `lastIndexOf()` Metodları

`indexOf()` və `lastIndexOf()` massivdə **təyin olunmuş bir dəyərə (specified value)** malik elementi axtarır və tapılan ilk belə elementin **indeksini (index)** qaytarır. Əgər heç nə tapılmazsa, `-1` qaytarır.

* `indexOf()` massivi **başlanğıcdan sona qədər (beginning to end)** axtarır.
* `lastIndexOf()` massivi **sondan başlanğıca qədər (end to beginning)** axtarır.

**Misallar:**

```javascript
let a = [0, 1, 2, 1, 0];

a.indexOf(1);       // => 1: a[1] 1-dir (ilk tapılan)
a.lastIndexOf(1);   // => 3: a[3] 1-dir (sondan tapılan ilk)
a.indexOf(3);       // => -1: heç bir elementin dəyəri 3 deyil
```

`indexOf()` və `lastIndexOf()` arqumentlərini massiv elementləri ilə `===` operatorunun ekvivalenti olan alqoritmdən istifadə edərək müqayisə edir. Əgər massiviniz primitiv dəyərlər (primitive values) yerinə obyektləri (objects) ehtiva edirsə, bu metodlar iki istinadın (references) eyni obyekti göstərib-göstərmədiyini yoxlayır. Əgər siz bir obyektin məzmununa (content) əsasən axtarış etmək istəyirsinizsə, bunun yerinə öz **fərdi predikat funksiyanız (custom predicate function)** ilə `find()` metodundan istifadə edin.

`indexOf()` və `lastIndexOf()` axtarışa başlamaq üçün ixtiyari bir **ikinci arqument (optional second argument)** qəbul edir. Bu arqument buraxılarsa, `indexOf()` başlanğıcdan, `lastIndexOf()` isə sondan başlayır. İkinci arqument üçün mənfi dəyərlərə (negative values) icazə verilir və `slice()` metodu üçün olduğu kimi, massivin sonundan bir ofset (offset) kimi qəbul edilir: məsələn, `-1` dəyəri massivdəki son elementi təyin edir.

Aşağıdakı funksiya massivdə təyin olunmuş bir dəyəri axtarır və uyğun gələn bütün indekslərin (all matching indexes) massivini qaytarır. Bu, `indexOf()`-un ikinci arqumentinin ilk uyğunluqdan sonrakıları tapmaq üçün necə istifadə edilə biləcəyini göstərir:

```javascript
// 'a' massivində 'x' dəyərinin bütün halları tapır və uyğun indekslərin massivini qaytarır.
function findall(a, x) {
  let results = [], // Qaytaracağımız indekslər massivi
    len = a.length,   // Axtarılacaq massivin uzunluğu
    pos = 0;          // Axtarışın başlanğıc mövqeyi

  while (pos < len) { // Axtarılacaq daha çox element olduğu müddətcə...
    pos = a.indexOf(x, pos); // 'pos' mövqeyindən başlayaraq axtarır
    if (pos === -1) break;   // Əgər heç nə tapılmasa, bitiririk.
    results.push(pos);       // Əks halda, indeksi massivdə saxlayırıq
    pos = pos + 1;           // Və növbəti axtarışı növbəti elementdən başlayırıq
  }
  return results; // İndekslər massivini qaytarır
}

let numbers = [10, 20, 10, 30, 10];
console.log(findall(numbers, 10)); // => [0, 2, 4]
```
Qeyd edək ki, stringlərdə də `indexOf()` və `lastIndexOf()` metodları var, lakin onlarda mənfi ikinci arqument sıfır kimi qəbul edilir.

### `includes()` Metodu

ES2016-da təqdim olunan `includes()` metodu tək bir arqument (single argument) qəbul edir və massivin həmin dəyəri ehtiva edib-etmədiyini göstərən `true` (ehtiva edirsə) və ya `false` (ehtiva etmirsə) qaytarır. O, dəyərin indeksini bildirmir, yalnız onun mövcud olub-olmadığını bildirir. `includes()` metodu massivlər üçün effektiv bir **set üzvlüyü testi (set membership test)** kimidir. Lakin, massivlər setlər üçün səmərəli bir təmsil (efficient representation) deyil və əgər bir neçə elementdən çox elementlə işləyirsinizsə, əsl `Set` obyektindən (§11.1.1) istifadə etməlisiniz.

`includes()` metodu `indexOf()` metodundan bir vacib cəhətdən bir qədər fərqlidir. `indexOf()` bərabərliyi `===` operatorunun etdiyi eyni alqoritmdən istifadə edərək yoxlayır, bu alqoritm isə `NaN` (Not-a-Number) dəyərini özü də daxil olmaqla hər hansı başqa dəyərdən fərqli hesab edir. `includes()` isə bir qədər fərqli bir bərabərlik versiyasından istifadə edir ki, bu da **`NaN` dəyərini özü ilə bərabər hesab edir**. Bu o deməkdir ki, `indexOf()` bir massivdə `NaN` dəyərini aşkar etməyəcək, lakin `includes()` aşkar edəcək:

```javascript
let a = [1, true, 3, NaN];

a.includes(true);  // => true
a.includes(2);     // => false
a.includes(NaN);   // => true
a.indexOf(NaN);    // => -1; indexOf NaN-ı tapa bilməz
```

### `sort()` Metodu

`sort()` massivin elementlərini **yerində (in place)** sıralayır (sorts) və sıralanmış massivi qaytarır. Arqumentsiz çağırılanda, `sort()` massiv elementlərini **əlifba sırası ilə (alphabetical order)** sıralayır (lazım gələrsə müqayisəni yerinə yetirmək üçün onları müvəqqəti olaraq stringlərə çevirir):

```javascript
let a = ["banana", "cherry", "apple"];
a.sort(); // a == ["apple", "banana", "cherry"]
console.log(a);
```
Əgər massiv `undefined` elementləri ehtiva edirsə, onlar massivin sonuna sıralanır.

Massivi əlifba sırasından fərqli bir ardıcıllıqla sıralamaq üçün `sort()`-a arqument olaraq bir **müqayisə funksiyası (comparison function)** ötürməlisiniz. Bu funksiya onun iki arqumentindən hansının sıralanmış massivdə birinci gəlməli olduğuna qərar verir.

* Əgər birinci arqument ikincidən əvvəl gəlməlidirsə, müqayisə funksiyası **sıfırdan kiçik (less than zero)** bir rəqəm qaytarmalıdır.
* Əgər birinci arqument sıralanmış massivdə ikincidən sonra gəlməlidirsə, funksiya **sıfırdan böyük (greater than zero)** bir rəqəm qaytarmalıdır.
* Və əgər iki dəyər bərabərdirsə (yəni, onların sırası əhəmiyyətsizdirsə), müqayisə funksiyası **`0`** qaytarmalıdır.

Məsələn, massiv elementlərini əlifba sırası yerinə **ədədi qaydada (numerical order)** sıralamaq üçün aşağıdakını edə bilərsiniz:

```javascript
let a = [33, 4, 1111, 222];
a.sort(); // a == [1111, 222, 33, 4]; (əlifba sırası ilə)
console.log(a);

a.sort(function(a, b) { // Bir müqayisə funksiyası ötürürük
  return a - b;          // Sıraya görə < 0, 0, və ya > 0 qaytarır
}); // a == [4, 33, 222, 1111]; (ədədi qayda)
console.log(a);

a.sort((a, b) => b - a); // a == [1111, 222, 33, 4]; (ədədi qaydada tərs sıralama)
console.log(a);
```

Massiv elementlərini sıralamağın başqa bir nümunəsi olaraq, string massivini **hərf böyük-kiçik fərqinə həssas olmayan (case-insensitive)** əlifba sırası ilə sıralaya bilərsiniz. Bunun üçün hər iki arqumenti müqayisə etməzdən əvvəl kiçik hərflərə (toLowerCase() metodu ilə) çevirən bir müqayisə funksiyası ötürülür:

```javascript
let a = ["ant", "Bug", "cat", "Dog"];
a.sort(); // a == ["Bug", "Dog", "ant", "cat"]; (hərf böyük-kiçik fərqinə həssas sıralama)
console.log(a);

a.sort(function(s, t) {
  let a_lower = s.toLowerCase();
  let b_lower = t.toLowerCase();
  if (a_lower < b_lower) return -1;
  if (a_lower > b_lower) return 1;
  return 0;
}); // a == ["ant", "Bug", "cat", "Dog"]; (hərf böyük-kiçik fərqinə həssas olmayan sıralama)
console.log(a);
```

### `reverse()` Metodu

`reverse()` metodu massivin elementlərinin sırasını tərs çevirir (reverses the order) və tərs çevrilmiş massivi qaytarır. O, bunu **yerində (in place)** edir; başqa sözlə, elementləri yenidən sıralanmış yeni bir massiv yaratmır, əksinə onları mövcud massivdə yenidən düzəldir:

```javascript
let a = [1, 2, 3];
a.reverse(); // a == [3, 2, 1]
console.log(a);
```

---

# 7.8.7 Massivi Stringə Çevirmə (Array to String Conversions) ✍️

`Array` sinfi massivləri stringlərə çevirə bilən üç metod təyin edir ki, bu da adətən log və xəta mesajları yaradarkən istifadə olunur.

### `join()` Metodu

`join()` metodu massivin bütün elementlərini stringlərə çevirir (converts to strings) və onları birləşdirir (concatenates), nəticə olaraq bir string qaytarır. Nəticə stringdə elementləri ayıran ixtiyari bir ayırıcı string (separator string) təyin edə bilərsiniz. Əgər ayırıcı string təyin olunmazsa, vergül (comma) istifadə olunur:

```javascript
let a = [1, 2, 3];
a.join();      // => "1,2,3" (defolt olaraq vergül ilə)
a.join(" ");   // => "1 2 3" (boşluq ilə)
a.join("");    // => "123" (ayırıcı olmadan)

let b = new Array(10); // Elementləri olmayan, uzunluğu 10 olan massiv
b.join("-");           // => "---------" (9 tire stringi)
```

`join()` metodu, bir stringi hissələrə ayıraraq massiv yaradan `String.split()` metodunun əksidir (inverse).

### `toString()` Metodu

Massivlər, bütün JavaScript obyektləri kimi, bir `toString()` metoduna malikdir. Bir massiv üçün bu metod, arqumentsiz `join()` metodu kimi işləyir:

```javascript
[1, 2, 3].toString();       // => "1,2,3"
["a", "b", "c"].toString(); // => "a,b,c"
[1, [2, "c"]].toString();   // => "1,2,c" (daxili massiv də stringə çevrilir)
```
Qeyd edək ki, çıxışda kvadrat mötərizələr və ya massiv dəyəri ətrafında hər hansı digər ayırıcı yoxdur.

### `toLocaleString()` Metodu

`toLocaleString()` `toString()` metodunun **lokallaşdırılmış (localized)** versiyasıdır. O, hər bir massiv elementini elementin `toLocaleString()` metodunu çağıraraq stringə çevirir, sonra isə nəticə stringləri **lokal-spesifik (locale-specific)** (və tətbiq tərəfindən təyin olunmuş) bir ayırıcı stringdən istifadə edərək birləşdirir.

---

# 7.8.8 Statik Massiv Funksiyaları (Static Array Functions) 🏭

Daha əvvəl sənədləşdirdiyimiz massiv metodlarına əlavə olaraq, `Array` sinfi, massivlər üzərində deyil, `Array` konstruktoru vasitəsilə çağıra biləcəyiniz üç **statik funksiya (static functions)** da təyin edir.

* `Array.of()` və `Array.from()` yeni massivlər yaratmaq üçün **fabrik metodlarıdır (factory methods)**. Onlar §7.1.4 və §7.1.5-də sənədləşdirilib.

* Digər statik massiv funksiyası isə **`Array.isArray()`**-dır ki, naməlum bir dəyərin massiv olub-olmadığını müəyyən etmək üçün faydalıdır:

    ```javascript
    Array.isArray([]);  // => true (boş massiv massivdir)
    Array.isArray({});  // => false (obyekt massiv deyil)
    ```

---

# 7.9 Massivə Bənzər Obyektlər (Array-Like Objects) 🎭

Gördüyümüz kimi, JavaScript massivlərinin digər obyektlərdə olmayan bəzi xüsusi xüsusiyyətləri (special features) var:
* `length` xüsusiyyəti (property) siyahıya yeni elementlər əlavə edildikcə avtomatik olaraq yenilənir (automatically updated).
* `length` dəyərini daha kiçik bir dəyərə təyin etmək massivi kəsir (truncates the array).
* Massivlər `Array.prototype`-dan faydalı metodları miras alır (inherit useful methods).
* `Array.isArray()` massivlər üçün `true` qaytarır.

Bunlar JavaScript massivlərini adi obyektlərdən fərqləndirən xüsusiyyətlərdir. Lakin onlar bir massivi təyin edən əsas xüsusiyyətlər deyil. Çox vaxt **ədədi `length` xüsusiyyətinə (numeric length property)** və buna uyğun **mənfi olmayan tam ədəd xüsusiyyətlərinə (non-negative integer properties)** malik istənilən obyekti bir növ massiv kimi qəbul etmək tamamilə məqbuldur.

Bu **"massivə bənzər" obyektlər (array-like objects)** bəzən praktikada ortaya çıxır və siz birbaşa massiv metodlarını onlara tətbiq edə bilməsəniz və ya `length` xüsusiyyətindən xüsusi davranış gözləyə bilməsəniz də, əsl massiv üçün istifadə edəcəyiniz eyni kodla onlar üzərində yenə də dövr edə bilərsiniz (iterate through them). Məlum olur ki, bir çox massiv alqoritmləri (array algorithms) əsl massivlərlə olduğu kimi massivə bənzər obyektlərlə də eyni dərəcədə yaxşı işləyir. Bu, xüsusilə alqoritmləriniz massivi yalnız oxumaq üçün (read-only) qəbul etdiyi və ya ən azı massiv uzunluğunu dəyişməz qoyduğu hallarda doğrudur.

Aşağıdakı kod adi bir obyekti götürür, onu massivə bənzər bir obyekt etmək üçün xüsusiyyətlər əlavə edir və sonra yaranan psevdo-massivin "elementləri" üzərində dövr edir:

```javascript
let a = {}; // Adi boş bir obyektlə başlayırıq

// Onu "massivə bənzər" etmək üçün xüsusiyyətlər əlavə edirik
let i = 0;
while (i < 10) {
  a[i] = i * i;
  i++;
}
a.length = i; // 'length' xüsusiyyətini əlavə edirik

// İndi onu əsl massiv kimi gəzirik
let total = 0;
for (let j = 0; j < a.length; j++) {
  total += a[j];
}
console.log(total); // => 285 (0*0 + 1*1 + ... + 9*9)
```

Frontend (client-side) JavaScript-də, HTML sənədləri ilə işləmək üçün bir sıra metodlar (məsələn, `document.querySelectorAll()`) massivə bənzər obyektlər qaytarır. Budur, massivlər kimi işləyən obyektləri test etmək üçün istifadə edə biləcəyiniz bir funksiya:

```javascript
// 'o' obyektinin massivə bənzər olub olmadığını müəyyən edir.
// Stringlər və funksiyalar ədədi 'length' xüsusiyyətlərinə malikdir, lakin
// 'typeof' testi ilə istisna edilir. Client-side JavaScript-də DOM mətn
// node-ları (text nodes) ədədi 'length' xüsusiyyətinə malikdir və əlavə
// 'o.nodeType !== 3' testi ilə istisna edilməsi lazım gələ bilər.
function isArrayLike(o) {
  if (o &&                         // 'o' null, undefined və s. deyil.
      typeof o === "object" &&     // 'o' bir obyektdir
      Number.isFinite(o.length) && // 'o.length' sonlu bir ədəddir
      o.length >= 0 &&             // 'o.length' mənfi deyil
      Number.isInteger(o.length) && // 'o.length' bir tam ədəddir
      o.length < 4294967295) {     // 'o.length' < 2^32 - 1 (massiv uzunluğunun maksimum dəyəri)
    return true;                   // Onda 'o' massivə bənzərdir.
  } else {
    return false;                  // Əks halda, deyil.
  }
}

console.log(isArrayLike([]));               // => true (əsl massiv)
console.log(isArrayLike({0: 'a', 1: 'b', length: 2})); // => true (massivə bənzər obyekt)
console.log(isArrayLike("salam"));          // => false (stringlər adətən massiv kimi işlənmir)
console.log(isArrayLike(function(){}));    // => false (funksiyalar da)
```

Daha sonrakı bir hissədə stringlərin massivlər kimi davrandığını görəcəyik. Bununla belə, massivə bənzər obyektlər üçün bu kimi testlər adətən stringlər üçün `false` qaytarır — onlar adətən massivlər kimi deyil, stringlər kimi ən yaxşı şəkildə işlənir.

Əksər JavaScript massiv metodları, həqiqi massivlərə əlavə olaraq **massivə bənzər obyektlərə tətbiq edildikdə də düzgün işləməsi üçün qəsdən ümumi (generic)** təyin edilmişdir. Massivə bənzər obyektlər `Array.prototype`-dan miras almadığı üçün, massiv metodlarını birbaşa onlar üzərində çağıra bilməzsiniz. Lakin, siz onları `Function.call` metodundan istifadə edərək **dolayı yolla (indirectly)** çağıra bilərsiniz (ətraflı məlumat üçün §8.7.4-ə baxın):

```javascript
let a = {"0": "a", "1": "b", "2": "c", length: 3}; // Massivə bənzər obyekt

// join() metodunu 'a' obyekti üzərində çağırırıq
console.log(Array.prototype.join.call(a, "+")); // => "a+b+c"

// map() metodunu 'a' obyekti üzərində çağırırıq
console.log(Array.prototype.map.call(a, x => x.toUpperCase())); // => ["A", "B", "C"]

// slice() metodunu 'a' obyekti üzərində çağırırıq (əsl massiv kopyası)
console.log(Array.prototype.slice.call(a, 0)); // => ["a", "b", "c"]: əsl massiv kopyası

// Daha asan bir üsul: Array.from() (ES6+)
console.log(Array.from(a)); // => ["a", "b", "c"]: daha asan massiv kopyası
```
Bu kodun sondan ikinci sətri, massivə bənzər obyektin elementlərini əsl massiv obyektinə kopyalamaq üçün `Array.slice()` metodunu çağırır. Bu, bir çox köhnə kodda (legacy code) mövcud olan bir idomatik fənddir (idiomatic trick), lakin indi `Array.from()` ilə bunu etmək çox daha asandır.

---

# 7.10 Stringlər Massiv Kimi (Strings as Arrays) 📜

JavaScript stringləri (strings) **UTF-16 Unicode simvollarının yalnız oxumaq üçün (read-only) massivləri** kimi davranır. Fərdi simvollara `charAt()` metodu ilə daxil olmaq əvəzinə, kvadrat mötərizələrdən (`[]`) istifadə edə bilərsiniz:

```javascript
let s = "test";
console.log(s.charAt(0)); // => "t"
console.log(s[1]);        // => "e"
```

`typeof` operatoru stringlər üçün hələ də "string" qaytarır, əlbəttə ki, və `Array.isArray()` metoduna bir string ötürsəniz, `false` qaytarır.

İndekslənə bilən stringlərin (indexable strings) əsas faydası sadəcə `charAt()` çağırışlarını daha qısa (more concise) və oxunaqlı (readable) olan, potensial olaraq daha effektiv (more efficient) kvadrat mötərizələrlə əvəz edə bilməyimizdir. Stringlərin massivlər kimi davranması, həmçinin onlara ümumi massiv metodlarını tətbiq edə biləcəyimiz deməkdir. Məsələn:

```javascript
// JavaScript stringini boşluqlarla birləşdirir
console.log(Array.prototype.join.call("JavaScript", " ")); // => "J a v a S c r i p t"
```
Unutmayın ki, stringlər **dəyişməz dəyərlərdir (immutable values)**, buna görə də onlar massivlər kimi qəbul edildikdə, onlar **yalnız oxumaq üçün massivlərdir (read-only arrays)**. `push()`, `sort()`, `reverse()` və `splice()` kimi massiv metodları bir massivi yerində dəyişdirir və stringlər üzərində işləmir. Bir stringi massiv metodu ilə dəyişdirməyə cəhd etmək, lakin, bir xətaya (error) səbəb olmur: o, sadəcə **səssizcə uğursuz olur (fails silently)**.

---