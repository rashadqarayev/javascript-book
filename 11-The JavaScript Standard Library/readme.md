### Fəsil 11: JavaScript-in Standart Kitabxanasına Baxış

Bu fəsildə JavaScript-in daxilində olan, **hər proqramda istifadə oluna bilən hazır siniflər və funksiyalar** təqdim olunur. Yəni əlavə heç nə yükləmədən, **JavaScript-in özündə olan güclü alətlərdən** danışırıq.

### Bu fəsildə nə öyrənəcəyik?

Burada aşağıdakı **standart, hazır alətlərlə** tanış olacağıq:

* **Set və Map** – Unikal dəyərlər və açar-dəyər (key-value) saxlamaq üçün.
* **TypedArray və ArrayBuffer** – İkili data (binary) ilə işləmək üçün.
* **RegExp** – Mətnlə (string) daha ağıllı işləmək üçün nümunələr yaratmaq.
* **Date** – Tarix və zamanla işləmək.
* **Error** – Səhvləri yaratmaq və idarə etmək.
* **JSON** – Data-nı string kimi saxlamaq və oxumaq.
* **Intl** – Lokallaşma (məsələn, fərqli valyuta və tarix formatları).
* **Console** – Debug və log üçün.
* **URL** – URL-ləri parçalayıb işləmək.
* **setTimeout və s.** – Kodun müəyyən vaxtdan sonra işləməsi üçün.

> ⚠️ Qeyd: TypedArray və RegExp hissələri bir az ağır ola bilər, amma digərləri daha yüngül və praktiki hissələrdir.

---

### 11.1.1 `Set` Sinfi (Class)

`Set` — JavaScript-də təkrarsız dəyərləri saxlayan xüsusi bir kolleksiyadır. Massivə bənzəsə də, onun fərqli xüsusiyyətləri var:

* **Təkrarlara icazə vermir.** Yəni, bir dəyəri `Set`-ə iki dəfə əlavə etsən də, sadəcə bir dəfə saxlanır.
* **İndeksli deyil.** `Set`-də `s[0]` kimi indeks vasitəsilə dəyərə birbaşa giriş yoxdur.
* **Əlavə edilən elementlərin daxil olma sırasını saxlayır.** Yəni, iterasiya zamanı elementlər əlavə olunduğu qaydada gəlir.

### Set yaratmaq

Yeni `Set` yaratmaq üçün `new Set()` yazırıq. İstəyə görə `Set`-ə əvvəlcədən elementlər verə bilərik.

```javascript
let s = new Set(); // boş Set

let t = new Set([1, 2, 3]);  // 1, 2, 3 elementləri olan Set
// new Set([1,2,3])

let strSet = new Set("hello"); 
// "h", "e", "l", "o" — stringdəki unikal hərfləri saxlayır
```

* **Nəzərə alın:** `Set` konstruktoru istənilən iterable obyekt qəbul edir. Buna massiv, string, hətta başqa `Set` də daxildir.

```javascript
let s2 = new Set(t);  // t Set-in elementləri ilə yeni Set yaradırıq
```

### **Element əlavə etmək — `add()`**

`Set`-ə element əlavə etmək üçün `add()` metodundan istifadə edirik.

```javascript
let s = new Set();

s.add(1);
s.add(2);
s.add(2);  // təkrar əlavə olunur, amma təsiri yoxdur

console.log(s.size);  // 2
```

* `add()` metodunun qaytarışı `Set`-in özüdür, buna görə zəncirləmə mümkündür:

```javascript
s.add(3).add(4).add(5);
console.log(s.size);  // 5
```

---

### **Element silmək — `delete()` və `clear()`**

* `delete()` metodu verilmiş elementi `Set`-dən silir. Əgər element varsa `true`, yoxdursa `false` qaytarır.

```javascript
s.delete(2);  // true
s.delete(42); // false, belə element yoxdur
```

* `clear()` metodu `Set`-dəki bütün elementləri silir.

```javascript
s.clear();
console.log(s.size);  // 0
```

---


#### Üzvlük Yoxlaması (`has()`) ✅

Set-lər (sets) ilə ən vacib əməliyyat, müəyyən bir dəyərin (value) set-in (set) üzvü (member) olub olmadığını `has()` metodu (method) ilə yoxlamaqdır:

```javascript
let oneDigitPrimes = new Set([2,3,5,7]);
console.log(oneDigitPrimes.has(2));     // => true
console.log(oneDigitPrimes.has(4));     // => false
console.log(oneDigitPrimes.has("5"));   // => false ("5" rəqəm olmadığı üçün)
```
Set-lərin (sets) əsas üstünlüyü, `has()` metodunun (method) üzvlük (membership) yoxlamaları üçün **çox sürətli optimallaşdırılmasıdır**, set-in (set) ölçüsündən (size) asılı olmayaraq. Massivin (array) `includes()` metodu (method) isə massivin (array) ölçüsü (size) ilə mütənasib vaxt aparır, bu da onu böyük toplular (collections) üçün yavaş edir. 🚀

#### Set-ləri İterasiya Etmək (Iterating Sets) 🔄

`Set` sinfi (class) iterable (təkrarlana bilən) olduğundan, `for/of` dövrü (loop) ilə bütün elementlərini (elements) sıralaya (enumerate) bilərsiniz:

```javascript
let sum = 0;
for(let p of oneDigitPrimes) { // Sadə ədədləri dövrə salın
    sum += p;                   // və toplayın
}
console.log(sum);               // => 17
```
`Set` obyektləri (objects) iterable (təkrarlana bilən) olduğu üçün, onları `...` spread operatoru (operator) ilə massivlərə (arrays) və funksiya arqumentlərinə (function arguments) çevirə bilərsiniz:

```javascript
console.log([...oneDigitPrimes]);       // => [2,3,5,7]: Set massivə (array) çevrildi
console.log(Math.max(...oneDigitPrimes));// => 7: Set elementləri funksiya arqumentləri kimi ötürüldü
```
Set-lər (sets) adətən "sırasız toplular" (unordered collections) kimi təsvir edilsə də, JavaScript `Set` sinfi (class) elementlərin (elements) daxil edildiyi qaydanı (order) yadda saxlayır və iterasiya (iterate) zamanı həmişə bu qaydadan (order) istifadə edir.

`Set` sinfi (class) həmçinin massiv (array) metoduna (method) bənzər bir `forEach()` metodu (method) da implementasiya (implement) edir:

```javascript
let product = 1;
oneDigitPrimes.forEach(n => { product *= n; });
console.log(product); // => 210
```
Massivin (array) `forEach()` metodu (method) funksiyaya (function) massiv (array) indekslərini (indexes) ikinci arqument (argument) kimi ötürsə də, `Set` sinfinin (class) `forEach()` metodu (method) sadəcə elementi (element) həm birinci, həm də ikinci arqument (argument) kimi ötürür, çünki set-lərin (sets) indeksləri (indexes) yoxdur.

---

### 11.1.2 `Map` Sinfi (Class) 🗺️

**Map** obyekti (object), **açar (key)** adlanan dəyərlər (values) toplusunu təmsil edir ki, burada hər bir açar (key) ilə başqa bir dəyər (value) əlaqələndirilir ("xəritələnir"). Bir mənada, map massiv (array) kimidir, lakin açarlar (keys) ardıcıl tam ədədlər (sequential integers) əvəzinə istənilən dəyərlər (arbitrary values) ola bilər. Massivlər (arrays) kimi, map-lər (maps) də sürətlidir: açar (key) ilə əlaqəli dəyəri (value) tapmaq, map-in (map) ölçüsündən (size) asılı olmayaraq sürətli olacaq (massivi (array) indeksləmə (indexing) qədər sürətli olmasa da).

#### Map Yaratmaq və İlkinləşdirmək (Creating and Initializing a Map) ✨

Yeni bir map (`Map()`) konstruktoru (constructor) ilə yaradılır:

```javascript
let m = new Map(); // Yeni, boş bir map
let n = new Map([  // Simvol (string) açarları (keys) rəqəmlərə (numbers) xəritələnmiş yeni bir map
    ["one", 1],
    ["two", 2]
]);
```
`Map()` konstruktoruna (constructor) ötürülən opsional arqument (optional argument) iki elementli `[key, value]` massivləri (arrays) qaytaran iterable (təkrarlana bilən) obyekt (object) olmalıdır. Praktikada bu, map-i (map) yaradarkən ilkinləşdirmək (initialize) istəyirsinizsə, adətən istədiyiniz açarları (keys) və əlaqəli dəyərləri (values) massivlər (arrays) massivi (array of arrays) kimi yazacağınız deməkdir. Lakin `Map()` konstruktorunu (constructor) digər map-ləri (maps) kopyalamaq və ya mövcud bir obyektin (object) xüsusiyyət (property) adlarını (names) və dəyərlərini (values) kopyalamaq üçün də istifadə edə bilərsiniz:

```javascript
let copy = new Map(n); // "n" map-i (map) ilə eyni açar (key) və dəyərlərə (values) malik yeni map
let o = { x: 1, y: 2}; // İki xüsusiyyəti (properties) olan bir obyekt (object)
let p = new Map(Object.entries(o)); // Yeni map([["x", 1], ["y", 2]]) ilə eyni
```

#### Elementlərə Daxil Olmaq və Manipulyasiya Etmək (`get()`, `set()`, `has()`, `delete()`, `clear()`, `size`) ⚙️

`Map` obyekti (object) yaratdıqdan sonra, `get()` ilə müəyyən bir açar (key) ilə əlaqəli dəyəri (value) sorğu edə, `set()` ilə yeni bir açar/dəyər (key/value) cütü (pair) əlavə edə bilərsiniz. Unutmayın ki, map açarlar (keys) toplusudur, hər bir açarın (key) əlaqəli dəyəri (value) var. Əgər `set()` metodunu (method) map-də (map) artıq mövcud olan bir açar (key) ilə çağırsanız, yeni bir açar/dəyər (key/value) xəritələməsi (mapping) əlavə etmək əvəzinə, həmin açar (key) ilə əlaqəli dəyəri (value) dəyişəcəksiniz.

`get()` və `set()` metodlarına (methods) əlavə olaraq, `Map` sinfi (class) `Set` metodlarına (methods) bənzər metodlar (methods) da təyin edir:
* `has()`: map-in (map) qeyd olunan açarı (key) ehtiva edib etmədiyini yoxlamaq üçün.
* `delete()`: map-dən (map) bir açarı (key) (və əlaqəli dəyərini (value)) silmək üçün.
* `clear()`: map-dən (map) bütün açar/dəyər (key/value) cütlərini (pairs) silmək üçün.
* `size` xüsusiyyəti (property): map-in (map) nə qədər açar (key) ehtiva etdiyini öyrənmək üçün.

```javascript
let m = new Map(); // Boş bir map ilə başlayın
console.log(m.size);     // => 0

m.set("one", 1);         // "one" açarını (key) 1 dəyərinə (value) xəritələyin
m.set("two", 2);         // "two" açarını (key) 2 dəyərinə (value) xəritələyin
console.log(m.size);     // => 2
console.log(m.get("two"));   // => 2: "two" açarı (key) ilə əlaqəli dəyəri (value) qaytarır
console.log(m.get("three")); // => undefined: bu açar (key) map-də (map) yoxdur

m.set("one", true);      // Mövcud bir açar (key) ilə əlaqəli dəyəri (value) dəyişin
console.log(m.size);     // => 2: ölçü (size) dəyişmir

console.log(m.has("one"));   // => true: map-də (map) "one" açarı (key) var
console.log(m.has(true));    // => false: map-də (map) true açarı (key) yoxdur

console.log(m.delete("one"));// => true: açar (key) mövcud idi və silmə (deletion) uğurlu oldu
console.log(m.size);         // => 1
console.log(m.delete("three"));// => false: mövcud olmayan açarı (key) silmək uğursuz oldu

m.clear();               // Map-dən (map) bütün açar (key) və dəyərləri (values) silin
```
`Set` sinfinin (class) `add()` metodu (method) kimi, `Map`-in `set()` metodu (method) da zəncirlənə (chained) bilər. Bu, map-ləri (maps) massivlər (arrays) massivi (array of arrays) istifadə etmədən ilkinləşdirməyə (initialize) imkan verir:

```javascript
let m = new Map().set("one", 1).set("two", 2).set("three", 3);
console.log(m.size); // => 3
console.log(m.get("two")); // => 2
```

#### Açar (Key) Müqayisəsi (Key Comparison) 🔑

`Set` sinfi (class) kimi, `Map`-də (map) istənilən JavaScript dəyəri (value) açar (key) və ya dəyər (value) kimi istifadə edilə bilər. Buna `null`, `undefined` və `NaN`, həmçinin obyektlər (objects) və massivlər (arrays) kimi referans tipləri (reference types) daxildir. `Set` sinfində (class) olduğu kimi, `Map` da açarları (keys) identikliyə (identity) görə, bərabərliyə (equality) görə deyil, müqayisə edir. Yəni, bir obyekt (object) və ya massiv (array) açar (key) kimi istifadə edirsinizsə, o, hər digər obyekt (object) və massivdən (array) fərqli sayılacaq, hətta tamamilə eyni xüsusiyyətlərə (properties) və ya elementlərə (elements) malik olsalar belə:

```javascript
let m = new Map();       // Boş bir map ilə başlayın.
m.set({}, 1);            // Bir boş obyekti (object) 1-ə xəritələyin.
m.set({}, 2);            // Başqa bir boş obyekti (object) 2-yə xəritələyin.
console.log(m.size);     // => 2: bu map-də (map) iki açar (key) var.
console.log(m.get({}));  // => undefined: bu boş obyekt (object) açar (key) deyil (çünki referans fərqlidir).

m.set(m, undefined);     // Map-in (map) özünü undefined dəyərinə (value) xəritələyin.
console.log(m.has(m));   // => true: m özü-özünün açarıdır.
console.log(m.get(m));   // => undefined: açar (key) olmasa da bu dəyəri (value) alırdıq.
```

#### Map-ləri İterasiya Etmək (Iterating Maps) 🔄

`Map` obyektləri (objects) iterable (təkrarlana bilən) olduğundan, iterasiya olunan hər dəyər (value) ilk elementi (element) açar (key), ikinci elementi (element) isə həmin açar (key) ilə əlaqəli dəyər (value) olan iki elementli massivdir (array). `Map` obyekti (object) ilə spread operatorunu (operator) istifadə etsəniz, `Map()` konstruktoruna (constructor) ötürdüyümüz kimi massivlər (arrays) massivi (array of arrays) əldə edəcəksiniz. `for/of` dövrü (loop) ilə map-i (map) iterasiya (iterate) edərkən, açarı (key) və dəyəri (value) ayrı dəyişənlərə (variables) mənimsətmək üçün strukturlaşdırıcı mənimsətmə (destructuring assignment) istifadə etmək adidir:

```javascript
let m = new Map([["x", 1], ["y", 2]]);
console.log([...m]); // => [["x", 1], ["y", 2]]

for(let [key, value] of m) {
    console.log(`Key: ${key}, Value: ${value}`);
    // İlk iterasiyada key "x" və value 1 olacaq.
    // İkinci iterasiyada key "y" və value 2 olacaq.
}
```
`Set` sinfi (class) kimi, `Map` sinfi (class) də elementləri (elements) daxil etmə (insertion) ardıcıllığı (order) ilə iterasiya (iterate) edir.

Yalnız map-in (map) açarlarını (keys) və ya yalnız əlaqəli dəyərlərini (values) iterasiya (iterate) etmək istəyirsinizsə, `keys()` və `values()` metodlarını (methods) istifadə edin: bunlar daxil etmə (insertion) ardıcıllığı (order) ilə açarları (keys) və dəyərləri (values) iterasiya (iterate) edən iterable obyektlər (objects) qaytarır. (`entries()` metodu (method) açar/dəyər (key/value) cütlərini (pairs) iterasiya (iterate) edən iterable obyekt (object) qaytarır, lakin bu, map-i (map) birbaşa iterasiya (iterate) etməklə eynidir.)

```javascript
console.log([...m.keys()]);   // => ["x", "y"]: yalnız açarlar (keys)
console.log([...m.values()]); // => [1, 2]: yalnız dəyərlər (values)
console.log([...m.entries()]);// => [["x", 1], ["y", 2]]: [...m] ilə eyni
```

`Map` obyektləri (objects) `Array` sinfi (class) tərəfindən ilk dəfə implementasiya (implement) olunmuş `forEach()` metodu (method) istifadə edilərək də iterasiya (iterate) edilə bilər.

```javascript
m.forEach((value, key) => { // Qeyd: value, key (key, value deyil)
    console.log(`Value: ${value}, Key: ${key}`);
    // İlk çağırışda value 1 və key "x" olacaq.
    // İkinci çağırışda value 2 və key "y" olacaq.
});
```
Yuxarıdakı kodda `value` parametrin (parameter) `key` parametrdən (parameter) əvvəl gəlməsi qəribə görünə bilər, çünki `for/of` iterasiyasında (iteration) `key` birinci gəlir. Bu bölmənin əvvəlində qeyd edildiyi kimi, map-i (map) tam ədəd (integer) massiv (array) indekslərinin (indexes) istənilən açar (key) dəyərləri (values) ilə əvəz olunduğu ümumi bir massiv (array) kimi düşünə bilərsiniz. Massivlərin (arrays) `forEach()` metodu (method) massiv (array) elementini (element) birinci, massiv (array) indeksini (index) isə ikinci ötürür, buna görə də, analoji olaraq, map-in (map) `forEach()` metodu (method) map dəyərini (value) birinci, map açarını (key) isə ikinci ötürür.

---

### 11.1.3 `WeakMap` və `WeakSet` ♻️

JavaScript-in `WeakMap` və `WeakSet` kimi "zəif referans" (weak reference) topluları (collections) yaddaş sızmalarının (memory leaks) qarşısını almaq üçün nəzərdə tutulub. Bunlar, obyektlərə (objects) olan referansları (references) "zəif" saxlayır, yəni obyekt başqa heç bir yerdən istifadə olunmursa, zibil (garbage collector) tərəfindən təmizlənə bilər.

#### `WeakMap` Sinfi (Class) 🔑➡️🗑️

`WeakMap`, `Map` sinfinin (class) bir variantıdır, lakin açar (key) dəyərlərinin (values) zibil toplama (garbage collection) prosesinə mane olmur. Bu, obyektlər (objects) proqram tərəfindən artıq "əlçatan" olmadıqda yaddaşlarının (memory) geri alınması prosesidir. Adi `Map` obyektləri (objects) açarlara (keys) "güclü" referanslar (references) saxladığı halda, `WeakMap` "zəif" referanslar (references) saxlayır.

**Əsas Fərqlər və Məhdudiyyətlər:**

* **Açar Tipləri (`Key Types`):** `WeakMap` açarları (keys) **yalnız obyektlər (objects) və ya massivlər (arrays)** ola bilər. Primitiv dəyərlər (primitive values) (string, number, boolean və s.) zibil toplamaya (garbage collection) tabe olmadığı üçün açar (key) kimi istifadə edilə bilməz.
* **Məhdud Metodlar (`Limited Methods`):** `WeakMap` yalnız `get()`, `set()`, `has()`, və `delete()` metodlarını (methods) dəstəkləyir. O, iterable (təkrarlana bilən) deyil və `keys()`, `values()`, `forEach()` və ya `size` xüsusiyyətinə (property) malik deyil. Əgər olsaydı, açarları (keys) əlçatan (reachable) olardı və bu, onun "zəif" xüsusiyyətini (property) pozardı.

**Nümunə: Keşləmə (Caching) ilə Yaddaş Sızmasının Qarşısını Almaq** 💾

Təsəvvür edin ki, bir obyektdə (object) mürəkkəb bir hesablama (computation) aparan bir funksiya (function) yazırsınız və effektivlik üçün nəticəni keşləmək (cache) istəyirsiniz.

```javascript
// Uzun çəkən bir hesablama funksiyası (function)
function calculateComplexValue(obj) {
    console.log('Mürəkkəb hesablama aparılır...');
    // Burada vaxt aparan bir proses gedir
    return obj.value * 2; // Sadə nümunə üçün
}

// Keş (cache) üçün WeakMap istifadə edirik
const cache = new WeakMap();

function getCalculatedValue(obj) {
    if (cache.has(obj)) {
        console.log('Keşdən (cache) götürülür...');
        return cache.get(obj); // Əgər keşdə (cache) varsa, ordan qaytar
    } else {
        const result = calculateComplexValue(obj); // Yoxdursa, hesabla
        cache.set(obj, result); // Nəticəni keşə (cache) əlavə et
        return result;
    }
}

let myObject = { value: 10 };
console.log(getCalculatedValue(myObject)); // İlk dəfə: "Mürəkkəb hesablama aparılır...", 20
console.log(getCalculatedValue(myObject)); // İkinci dəfə: "Keşdən (cache) götürülür...", 20

// myObject-ə olan referans (reference) silinir.
// myObject indi "əlçatan" deyil və WeakMap onu zibil toplamaya (garbage collection) buraxacaq.
myObject = null;
// Bu nöqtədə WeakMap-də (WeakMap) həmin obyektə (object) olan referans (reference) təmizlənə bilər.
// Əgər Map istifadə etsəydik, myObject yaddaşda qalacaqdı.
```

Bu nümunədə, `myObject` dəyişəni (variable) `null` olaraq təyin edildikdə, ona heç bir güclü referans (strong reference) qalmadığı üçün `WeakMap` onun yaddaşdan (memory) təmizlənməsinə mane olmur. Əgər adi `Map` istifadə etsəydik, `myObject` obyektinə (object) olan referans (reference) map (map) daxilində "güclü" qalacaq və yaddaşdan (memory) təmizlənməyəcəkdi.

#### `WeakSet` Sinfi (Class) 🤝➡️🗑️

`WeakSet` obyekti (object) zibil toplanmasına (garbage collection) mane olmayan obyektlər (objects) toplusunu (set) implementasiya (implement) edir. `WeakSet()` konstruktoru (constructor) `Set()` konstruktoru (constructor) kimi işləyir, lakin `WeakMap` və `Map` arasındakı fərqlərə bənzər fərqləri var:

* **Primitiv Dəyərlərə İcazə Yoxdur (`No Primitive Values`):** `WeakSet` üzv (member) kimi primitiv dəyərlərə (primitive values) icazə vermir (yalnız obyektlər (objects) və massivlər (arrays) ola bilər).
* **Məhdud Metodlar (`Limited Methods`):** `WeakSet` yalnız `add()`, `has()` və `delete()` metodlarını (methods) implementasiya (implement) edir və iterable (təkrarlana bilən) deyil.
* **`size` Xüsusiyyəti Yoxdur (`No size Property`):** `WeakSet`-in `size` xüsusiyyəti (property) yoxdur.

**Nümunə: Obyektləri İşarələmək (Branding Objects)** 🏷️

`WeakSet` adətən obyektləri (objects) müəyyən bir xüsusiyyətə (property) və ya tipə (type) malik olduğunu "işarələmək" (mark) üçün istifadə olunur, lakin bu obyektlərin (objects) yaddaşda (memory) sonsuza qədər qalmasını istəmədiyimiz zaman.

```javascript
// Bir WeakSet yaradırıq ki, xüsusi (special) obyektləri (objects) qeyd edək
const specialObjects = new WeakSet();

// Bir neçə obyekt (object)
let obj1 = { id: 1, name: 'Item A' };
let obj2 = { id: 2, name: 'Item B' };
let obj3 = { id: 3, name: 'Item C' };

// obj1-i xüsusi olaraq işarələyirik (brand)
specialObjects.add(obj1);

console.log(specialObjects.has(obj1)); // => true: obj1 xüsusi obyektlər arasındadır.
console.log(specialObjects.has(obj2)); // => false: obj2 işarələnməyib.

// obj1-ə olan referansı (reference) silirik
obj1 = null;

// Bu nöqtədə, əgər obj1-ə başqa referans (reference) yoxdursa,
// WeakSet onu zibil toplamaya (garbage collection) buraxacaq.
// regular Set istifadə etsəydik, obj1 yaddaşda qalacaqdı.
```
Bu nümunədə, `obj1` `null` olaraq təyin edildikdə, ona heç bir güclü referans (strong reference) qalmadığı üçün `WeakSet` onun yaddaşdan (memory) təmizlənməsinə mane olmur. `WeakSet` sayəsində, işarələdiyimiz (marked) obyektlər (objects) tətbiqdə (application) istifadə olunmağı dayandırdıqda avtomatik olaraq yaddaşdan (memory) silinə bilər.

---

### 11.2 Tipli Massivlər (Typed Arrays) və İkili Data (Binary Data) 💾

Adi JavaScript massivləri (arrays) istənilən tipdə (type) elementlərə (elements) malik ola və dinamik olaraq böyüyüb-kiçilə bilər. Onlar sürətli olsa da, C və Java kimi aşağı səviyyəli dillərin (low-level languages) massiv (array) tiplərindən (types) fərqlənirlər. ES6-da yeni olan **Tipli Massivlər (Typed Arrays)** isə həmin aşağı səviyyəli massivlərə (arrays) daha yaxındır və yaddaş (memory) ilə daha effektiv işləməyə imkan verir.

Tipli massivlər (typed arrays) texniki olaraq əsl massiv (array) deyil ( `Array.isArray()` onlar üçün `false` qaytarır), lakin §7.8-də təsvir olunan bütün massiv (array) metodlarını (methods) və bir neçə əlavə metodu (method) implementasiya (implement) edir. Lakin adi massivlərdən (arrays) bəzi vacib fərqləri var:

* **Rəqəm Elementləri (`Numeric Elements`):** Tipli massivin (typed array) elementləri (elements) yalnız rəqəmlərdir (numbers). Lakin adi JavaScript rəqəmlərindən (numbers) fərqli olaraq, tipli massivlər (typed arrays) saxlanacaq rəqəmlərin (numbers) **tipini** (işarəli (signed) və işarəsiz (unsigned) tam ədədlər (integers), həmçinin IEEE-754 onluq ədədlər (floating point)) və **ölçüsünü** (8 bitdən 64 bitə qədər) dəqiq təyin etməyə imkan verir. Bu, yaddaşın (memory) səmərəli istifadəsi üçün vacibdir.
* **Sabit Uzunluq (`Fixed Length`):** Tipli massivi (typed array) yaratdığınız zaman onun uzunluğunu (length) təyin etməlisiniz və bu uzunluq (length) heç vaxt dəyişə bilməz. Bu, yaddaşın ayrılmasına (memory allocation) kömək edir.
* **Sıfır İlkinləşdirmə (`Zero Initialization`):** Tipli massivin (typed array) elementləri (elements) yaradılarkən həmişə `0`, `0n` və ya `0.0` ilə avtomatik ilkinləşdirilir (initialized).

---

### 11.2.1 Tipli Massiv Növləri (Typed Array Types) 🔢

JavaScript `TypedArray` adlı vahid bir sinif (class) təyin etmir. Bunun əvəzinə, hər biri fərqli element tipinə (element type) və konstruktora (constructor) malik **11 növ tipli massiv (typed array)** mövcuddur. Bu müxtəlif növlər, müxtəlif ikili data (binary data) formatları (formats) ilə işləmək üçün çeviklik təmin edir:

| Konstruktor          | Rəqəm tipi (Numeric type)                             | Təsvir                                                                                                                                                                     |
| :------------------- | :---------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Int8Array()`        | İşarəli (signed) byte-lar (8-bit)                     | -128-dən 127-ə qədər tam ədədlər üçün.                                                                                                                                  |
| `Uint8Array()`       | İşarəsiz (unsigned) byte-lar (8-bit)                  | 0-dan 255-ə qədər tam ədədlər üçün.                                                                                                                                      |
| `Uint8ClampedArray()`| İşarəsiz (unsigned) byte-lar (çevrilmədən - no rollover) | 0-dan 255-ə qədər tam ədədlər üçün, lakin dəyər bu aralığı aşdıqda sıfıra və ya 255-ə "sabitlənir" (clamped). Piksel rəngləri kimi hallarda faydalıdır.                     |
| `Int16Array()`       | İşarəli (signed) 16-bit qısa tam ədədlər (integers)   | -32768-dən 32767-ə qədər.                                                                                                                                                |
| `Uint16Array()`      | İşarəsiz (unsigned) 16-bit qısa tam ədədlər (integers) | 0-dan 65535-ə qədər.                                                                                                                                                     |
| `Int32Array()`       | İşarəli (signed) 32-bit tam ədədlər (integers)        | Geniş diapazonlu tam ədədlər üçün.                                                                                                                                       |
| `Uint32Array()`      | İşarəsiz (unsigned) 32-bit tam ədədlər (integers)     | Geniş diapazonlu müsbət tam ədədlər üçün.                                                                                                                                |
| `BigInt64Array()`    | İşarəli (signed) 64-bit `BigInt` dəyərləri (ES2020)   | Çox böyük tam ədədlər üçün. `BigInt` (bax §3.2.5) istifadə edir.                                                                                                        |
| `BigUint64Array()`   | İşarəsiz (unsigned) 64-bit `BigInt` dəyərləri (ES2020) | Çox böyük müsbət tam ədədlər üçün. `BigInt` istifadə edir.                                                                                                              |
| `Float32Array()`     | 32-bit onluq ədəd (floating-point) dəyəri             | Daha az dəqiqlikli, lakin daha yaddaş səmərəli onluq ədədlər (C/Java-da `float`).                                                                                           |
| `Float64Array()`     | 64-bit onluq ədəd (floating-point) dəyəri             | Adi JavaScript rəqəmləri ilə eyni tipdədir, yüksək dəqiqlikli onluq ədədlər üçün.                                                                                          |

**Bəzi vacib qeydlər:**

* `Int` ilə başlayan tiplər işarəli (müsbət və mənfi) tam ədədlər (integers) saxlayır. `Uint` ilə başlayan tiplər isə yalnız işarəsiz (müsbət) tam ədədlər saxlayır.
* `Float32Array` yaddaşda (memory) `Float64Array`-in yarısı qədər yer tutur, lakin dəqiqliyi (precision) daha azdır.
* Hər bir tipli massiv (typed array) konstruktorunun (constructor) `BYTES_PER_ELEMENT` xüsusiyyəti (property) var ki, bu da tipdən (type) asılı olaraq 1, 2, 4 və ya 8 dəyərini göstərir (bir elementin neçə bayt yer tutduğunu).

---

### 11.2.2 Tipli Massivləri Yaratmaq (Creating Typed Arrays) 🛠️

Tipli massivlər (typed arrays) yaratmağın bir neçə yolu var:

#### 1. Uzunluqla Yaratmaq (Creating with Length)

Ən sadə yol, massivdə (array) istədiyiniz elementlərin (elements) sayını göstərən bir rəqəmsal arqument (numeric argument) ilə müvafiq konstruktoru (constructor) çağırmaqdır. Bütün elementlər (elements) avtomatik olaraq sıfır (0) ilə ilkinləşdirilir (initialized):

```javascript
let bytes = new Uint8Array(1024);     // 1024 baytlıq massiv, hər element 0.
console.log(bytes.length);            // => 1024
console.log(bytes[0]);                // => 0
console.log(bytes[100]);              // => 0

let matrix = new Float64Array(9);    // 9 elementli (məsələn, 3x3 matris üçün) onluq ədəd massivi.
let point = new Int16Array(3);       // 3 elementli 3D fəzada bir nöqtə üçün.
let rgba = new Uint8ClampedArray(4); // 4-baytlı RGBA piksel dəyəri (0-255 aralığında).
let sudoku = new Int8Array(81);      // 9x9 sudoku lövhəsi üçün.
```

#### 2. Mövcud Dəyərlərdən Yaratmaq (Creating from Existing Values)

Əgər tipli massivinizdə (typed array) hansı dəyərləri (values) istədiyinizi bilirsinizsə, onları birbaşa təyin edə bilərsiniz. Hər bir tipli massiv (typed array) konstruktoru (constructor) `Array.from()` və `Array.of()` kimi işləyən statik `from()` və `of()` fabrikat metodlarına (factory methods) malikdir:

```javascript
// `of()` metodu ilə birbaşa dəyərləri təyin edərək yaratmaq:
let whitePixel = Uint8ClampedArray.of(255, 255, 255, 0); // RGBA ağ (qeyri-şəffaf) piksel
console.log(whitePixel);                                  // => Uint8ClampedArray [255, 255, 255, 0]

// `from()` metodu ilə massivəbənzər (array-like) və ya iterable (təkrarlana bilən) obyektdən (object) yaratmaq:
let numbers = [10, 20, 30, 40];
let uint8Numbers = Uint8Array.from(numbers); // Adi massivdən Uint8Array yaratmaq
console.log(uint8Numbers);                   // => Uint8Array [10, 20, 30, 40]

// Mövcud tipli massivi kopyalamaq və ya tipini dəyişmək:
let intsFromWhite = Uint32Array.from(whitePixel); // eyni 4 rəqəm, lakin 32-bit tam ədədlər (ints) kimi
console.log(intsFromWhite);                        // => Uint32Array [255, 255, 255, 0]
```
`from()` metodu (method) massivəbənzər (array-like) və ya iterable (təkrarlana bilən) obyekt (object) qəbul edir. Əhəmiyyətlidir ki, elementləri (elements) rəqəmsal (numeric) olmalıdır (məsələn, simvolları (strings) `from()` metoduna ötürmək mənasızdır).
Dəyərlər (values) massivin (array) tip (type) məhdudiyyətlərinə (constraints) uyğun gəlmək üçün qısaldıla (truncated) bilər, bu zaman heç bir xəbərdarlıq (warning) və ya xəta (error) verilmir:

```javascript
// Onluq ədədlər (Floats) tam ədədlərə (ints) qısaldılır, uzun tam ədədlər (longer ints) 8 bitə qısaldılır.
let truncatedValues = Uint8Array.of(1.23, 2.99, 45000); // 45000 (2^15 + 13928) 255-dən böyük olduğu üçün 45000 % 256 = 200 olur
console.log(truncatedValues);                             // => Uint8Array [1, 2, 200]
```

#### 3. `ArrayBuffer` ilə Yaratmaq (Creating with ArrayBuffer) 📦

Tipli massivlər (typed arrays) yaratmağın başqa bir güclü yolu **`ArrayBuffer`** tipindən (type) keçir. `ArrayBuffer` – yaddaşın (memory) bir hissəsinə (chunk) qeyri-şəffaf (opaque) bir referansdır (reference). Onu konstruktor (constructor) ilə yarada bilərsiniz; sadəcə ayırmaq (allocate) istədiyiniz baytların (bytes) sayını ötürün:

```javascript
let buffer = new ArrayBuffer(1024 * 1024); // 1 meqabayt yaddaş ayırır (1MB = 1024 * 1024 bayt)
console.log(buffer.byteLength);            // => 1048576
```
`ArrayBuffer` sinfi (class) ayırdığınız (allocated) baytlardan (bytes) birbaşa oxumağa (read) və ya yazmağa (write) imkan vermir. Lakin siz bu buferin (buffer) yaddaşını (memory) istifadə edən və həmin yaddaşı (memory) oxumağa (read) və yazmağa (write) imkan verən tipli massivlər (typed arrays) yarada bilərsiniz. Bu, eyni yaddaş sahəsinə (memory area) fərqli şəkildə baxmaq ("view" etmək) üçün idealdır.

Bunu etmək üçün, tipli massiv (typed array) konstruktorunu (constructor) çağırarkən:
* **Birinci arqument (argument) kimi** bir `ArrayBuffer`
* **İkinci arqument (opsional)** kimi array buferin (array buffer) daxilində bayt ofseti (byte offset)
* **Üçüncü arqument (opsional)** kimi massiv (array) uzunluğunu (elementlərlə, baytlarla (bytes) deyil)

verməlisiniz. Əgər ikinci və üçüncü arqumentləri (arguments) atsanız, massiv (array) array buferindəki (array buffer) bütün yaddaşı (memory) istifadə edəcək. Yalnız uzunluq (length) arqumentini (argument) atsanız, massiviniz (array) başlanğıc mövqeyi (start position) ilə buferin (buffer) sonu arasındakı bütün mövcud yaddaşı (memory) istifadə edəcək.
**Yaddaş hizalanması (Memory Alignment) Vacibdir:** Tipli massivlər (typed arrays) yaddaşda (memory) hizalanmış (aligned) olmalıdır, buna görə də bir bayt ofseti (byte offset) təyin edirsinizsə, dəyər (value) tipinizin (type) ölçüsünün (size) misli (multiple) olmalıdır (məsələn, `Int32Array()` üçün 4-ün misli, `Float64Array()` üçün 8-in misli).

**Nümunə (`ArrayBuffer` ilə `Views` yaratmaq):**

```javascript
let buffer = new ArrayBuffer(16); // 16 baytlıq bufer yaradırıq.

// Bu buferə fərqli "görünüşlər" (views) yaradırıq:
let view8 = new Uint8Array(buffer);   // 8-bit işarəsiz tam ədədlər kimi baxış (view)
console.log(view8);                   // => Uint8Array(16) [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

let view32 = new Int32Array(buffer);  // 32-bit işarəli tam ədədlər kimi baxış (view)
console.log(view32);                  // => Int32Array(4) [0, 0, 0, 0] (16 bayt / 4 bayt = 4 element)

// Məsələn, view8 vasitəsilə dəyər yazsaq, view32-də də dəyişikliyi görərik:
view8[0] = 255;
view8[1] = 255;
view8[2] = 255;
view8[3] = 255;
console.log(view32[0]); // => -1 (Uint8Array-də 255-i dörd dəfə yazmaq Int32Array-də -1-ə çevrilir)

// Buferin (buffer) son 4 baytına baxış (view)
let last4Bytes = new Uint8Array(buffer, 12, 4); // Ofset 12-dən başlayaraq 4 bayt
console.log(last4Bytes);                        // => Uint8Array(4) [0, 0, 0, 0]

// Başqa bir görünüş: ofset 4-dən başlayaraq 2 ədəd 32-bit tam ədəd (Int32Array)
// Ofset (4) 32-bit (4 bayt) üçün hizalanmaya (alignment) uyğundur.
let partialInts = new Int32Array(buffer, 4, 2);
console.log(partialInts);                       // => Int32Array(2) [0, 0]
```
Bütün tipli massivlərin (typed arrays) əsasında bir `ArrayBuffer` olduğunu anlamaq vacibdir, hətta siz açıq şəkildə birini təyin etməsəniz belə. Əgər bufer (buffer) obyekti (object) ötürmədən tipli massiv (typed array) konstruktorunu (constructor) çağırsanız, müvafiq ölçüdə (size) bir bufer (buffer) avtomatik olaraq yaradılacaq. `ArrayBuffer` obyektləri (objects) ilə birbaşa işləməyin əsas səbəbi, bəzən tək bir buferin (buffer) bir neçə tipli massiv (typed array) görünüşünə (view) sahib olmaq istəməyinizdir.

---

### 11.2.3 Tipli Massivləri İstifadə Etmək (Using Typed Arrays) 📝

Tipli massiv (typed array) yaratdıqdan sonra, elementlərini (elements) adi massivəbənzər (array-like) obyektlərdə (objects) olduğu kimi, kvadrat mötərizə (square-bracket) sintaksisi (syntax) ilə oxuyub yaza bilərsiniz:

```javascript
let myInts = new Int16Array(5); // 5 elementli, 16-bit tam ədəd massivi
myInts[0] = 10;
myInts[1] = 20;
myInts[2] = -5;
console.log(myInts[0]); // => 10
console.log(myInts[1] + myInts[2]); // => 15
```

**Nümunə: Eratosthenes Qəbirvasitəsi (Sieve of Eratosthenes) ilə Baş Sadə Ədəd Tapmaq** 🚀

Aşağıdakı funksiya (function) təyin etdiyiniz rəqəmdən (number) kiçik olan ən böyük sadə ədədi (prime number) tapır. Kod adi JavaScript massivi (array) ilə eyni olsa da, `Uint8Array()` istifadə etmək onu **dörd dəfədən çox sürətli** edir və **səkkiz dəfə az yaddaş (memory)** istifadə edir:

```javascript
// n-dən kiçik ən böyük sadə (prime) ədədi (number) qaytarır.
function sieve(n) {
    let a = new Uint8Array(n + 1); // a[x] = 1 əgər x mürəkkəbdirsə (composite)
    let max = Math.floor(Math.sqrt(n)); // Bu dərəcədən yuxarı faktorları yoxlamırıq
    let p = 2; // 2 ilk sadə (prime) ədəddir (number)

    while (p <= max) { // max-dan kiçik sadə (prime) ədədlər (numbers) üçün
        for (let i = 2 * p; i <= n; i += p) { // p-nin misillərini mürəkkəb (composite) olaraq işarələyin
            a[i] = 1;
        }
        while (a[++p]) /* boş */; // Növbəti işarəsiz indeks (index) sadə (prime) ədəddir (number)
    }

    while (a[n]) n--; // Sonuncu sadə (prime) ədədi (number) tapmaq üçün geriyə doğru dövrə salın
    return n;          // Və onu qaytarın
}

console.log(sieve(10));  // => 7 (10-dan kiçik ən böyük sadə ədəd)
console.log(sieve(100)); // => 97
```

Tipli massivlər (typed arrays) həqiqi massiv (array) deyil, lakin əksər massiv (array) metodlarını (methods) yenidən implementasiya (implement) edirlər. Buna görə də, onları adi massivlər (arrays) kimi istifadə edə bilərsiniz:

```javascript
let ints = new Int16Array(10); // 10 qısa tam ədəd (short integers)
ints.fill(3).map(x => x * x).join(""); // => "9999999999"
```
Unutmayın ki, tipli massivlərin (typed arrays) uzunluğu (length) sabitdir, yəni `length` xüsusiyyəti (property) yalnız oxunur (read-only). Massivin (array) uzunluğunu (length) dəyişən metodlar (methods) (məsələn, `push()`, `pop()`, `unshift()`, `shift()`, və `splice()`) tipli massivlər (typed arrays) üçün implementasiya (implement) edilməyib. Lakin massivin (array) məzmununu (contents) uzunluğunu (length) dəyişmədən dəyişdirən metodlar (methods) (məsələn, `sort()`, `reverse()`, və `fill()`) implementasiya (implement) edilib. `map()` və `slice()` kimi yeni massivlər (arrays) qaytaran metodlar (methods) isə çağırıldıqları tipli massiv (typed array) ilə eyni tipdə (type) yeni bir tipli massiv (typed array) qaytarır.

---

### 11.2.4 Tipli Massiv Metodları (Typed Array Methods) və Xüsusiyyətləri (Properties) 📊

Standart massiv (array) metodlarına (methods) əlavə olaraq, tipli massivlər (typed arrays) bəzi öz metodlarını (methods) da implementasiya (implement) edir.

#### `set()` Metodu (Method) ✍️

`set()` metodu (method) adi və ya tipli massivin (typed array) elementlərini (elements) bir tipli massivə (typed array) kopyalayaraq, birdən çox elementi (element) birdən təyin edir:

```javascript
let bytes = new Uint8Array(1024);     // 1K-lıq bufer
let pattern = new Uint8Array([0,1,2,3]); // 4 baytlıq massiv (array)

bytes.set(pattern);                   // Onları başqa bir bayt massivinin (byte array) əvvəlinə kopyalayın
console.log(bytes.slice(0, 4));       // => Uint8Array [0, 1, 2, 3]

bytes.set(pattern, 4);                // Onları fərqli bir ofsetdə (offset) yenidən kopyalayın
console.log(bytes.slice(4, 8));       // => Uint8Array [0, 1, 2, 3]

bytes.set([0,1,2,3], 8);              // Və ya sadəcə adi massivdən (regular array) dəyərləri (values) birbaşa kopyalayın
console.log(bytes.slice(8, 12));      // => Uint8Array [0, 1, 2, 3]

console.log(bytes.slice(0, 12));      // => Uint8Array [0,1,2,3,0,1,2,3,0,1,2,3]
```
`set()` metodu (method) ilk arqument (argument) kimi bir massiv (array) və ya tipli massiv (typed array), opsional (optional) ikinci arqument (argument) kimi isə element ofsetini (element offset) qəbul edir (təyin edilməzsə `0` olur). Əgər dəyərləri (values) bir tipli massivdən (typed array) digərinə kopyalayırsınızsa, əməliyyat çox sürətli olacaq.

#### `subarray()` Metodu (Method) ✂️

Tipli massivlər (typed arrays) həmçinin çağırıldığı massivin (array) bir hissəsini qaytaran `subarray()` metoduna (method) malikdir:

```javascript
let ints = new Int16Array([0,1,2,3,4,5,6,7,8,9]); // 10 qısa tam ədəd (short integers)
let last3 = ints.subarray(ints.length-3, ints.length); // Son 3 element
console.log(last3);                                     // => Int16Array [7, 8, 9]
console.log(last3[0]);                                  // => 7: bu, ints[7] ilə eynidir
```
`subarray()` metodu (method) `slice()` metodu (method) ilə eyni arqumentləri (arguments) qəbul edir və eyni şəkildə işləyir. Lakin vacib bir fərq var:
* `slice()`: Yeni, müstəqil bir tipli massiv (typed array) qaytarır ki, bu da orijinal massivlə (array) yaddaşı (memory) paylaşmır.
* `subarray()`: Heç bir yaddaşı (memory) kopyalamır; sadəcə eyni əsas dəyərlərə (underlying values) yeni bir görünüş (view) qaytarır.

Bu o deməkdir ki, `subarray()` ilə dəyişikliklər orijinal massivə (array) də təsir edir:

```javascript
ints[9] = -1; // Orijinal massivdə (array) bir dəyəri (value) dəyişdirin
console.log(last3[2]); // => -1: o, alt-massivdə (subarray) də dəyişir
```
`subarray()` metodunun (method) mövcud massivin (array) yeni bir görünüşünü (view) qaytarması bizi `ArrayBuffer` mövzusuna geri gətirir. Hər bir tipli massivin (typed array) əsasındakı bufer (buffer) ilə əlaqəli üç xüsusiyyəti (properties) var:

```javascript
console.log(last3.buffer);           // Tipli massiv (typed array) üçün ArrayBuffer obyekti (object)
console.log(last3.buffer === ints.buffer); // => true: hər ikisi eyni buferə (buffer) baxışdır (view)
console.log(last3.byteOffset);       // => 14: bu görünüş (view) buferin (buffer) 14-cü baytından (byte) başlayır (7 * 2 bayt/element)
console.log(last3.byteLength);       // => 6: bu görünüş (view) 6 bayt (3 ədəd 16-bit tam ədəd) uzunluğundadır
console.log(last3.buffer.byteLength);// => 20: lakin əsas buferin (underlying buffer) 20 baytı (bytes) var (10 * 2 bayt/element)
```
`buffer` xüsusiyyəti (property) massivin (array) `ArrayBuffer`-idir. `byteOffset` massivin (array) datanın (data) əsas bufer (underlying buffer) daxilində başlanğıc mövqeyidir (starting position). Və `byteLength` massivin (array) datanın (data) baytlarla (bytes) uzunluğudur. Hər hansı bir tipli massiv (typed array) üçün bu invariant (invariant) həmişə doğru olmalıdır: `a.length * a.BYTES_PER_ELEMENT === a.byteLength // => true`.

`ArrayBuffer`-lər (ArrayBuffers) sadəcə qeyri-şəffaf (opaque) bayt topluluqlarıdır (chunks of bytes). Siz həmin baytlara (bytes) tipli massivlər (typed arrays) ilə daxil ola bilərsiniz, lakin `ArrayBuffer`-in (ArrayBuffer) özü tipli massiv (typed array) deyil. Diqqətli olun: siz `ArrayBuffer`-lərlə (ArrayBuffers) rəqəmsal massiv (array) indeksləməsi (indexing) istifadə edə bilərsiniz, eynilə istənilən JavaScript obyekti (object) kimi. Bunu etmək sizə buferdəki (buffer) baytlara (bytes) daxil olmağa imkan verməz, lakin çaşdırıcı səhvlərə (bugs) səbəb ola bilər:

```javascript
let bytes = new Uint8Array(8);
bytes[0] = 1; // İlk baytı (byte) 1-ə təyin edin
console.log(bytes.buffer[0]); // => undefined: buferin (buffer) 0 indeksli (index) xüsusiyyəti (property) yoxdur (bu, adi JS obyekti (object) xüsusiyyətinə (property) baxışdır)
bytes.buffer[1] = 255; // Buferdə (buffer) bir bayt (byte) səhv təyin etməyə çalışın
console.log(bytes.buffer[1]); // => 255: bu sadəcə adi bir JS xüsusiyyəti (property) təyin edir
console.log(bytes[1]);        // => 0: yuxarıdakı sətir əsl baytı (byte) təyin etmədi
```
Biz əvvəllər `ArrayBuffer()` konstruktoru (constructor) ilə `ArrayBuffer` yaradıb, sonra bu buferi (buffer) istifadə edən tipli massivlər (typed arrays) yarada biləcəyimizi gördük. Başqa bir yanaşma isə ilkin bir tipli massiv (typed array) yaratmaq, sonra həmin massivin (array) buferini (buffer) digər görünüşləri (views) yaratmaq üçün istifadə etməkdir:

```javascript
let bytes = new Uint8Array(1024);   // 1024 bayt
let ints = new Uint32Array(bytes.buffer); // Yuxarıdakı buferə (buffer) 256 tam ədəd (integers) kimi baxış (view)
let floats = new Float64Array(bytes.buffer); // Yuxarıdakı buferə (buffer) 128 ikiqat dəqiqlikli (doubles) onluq ədəd (floats) kimi baxış (view)
```

---

### 11.2.5 DataView və Endianness (Byte Sırası) 🔄

Tipli massivlər (typed arrays) eyni bayt (byte) ardıcıllığını (sequence) 8, 16, 32 və ya 64 bitlik hissələrdə görməyə imkan verir. Bu, **"endianness"**: baytların (bytes) daha uzun sözlərə (words) düzülmə ardıcıllığını (order) ortaya qoyur. Effektivlik üçün, tipli massivlər (typed arrays) əsas aparatın (underlying hardware) yerli endianness-dən (native endianness) istifadə edir.

* **Little-endian** sistemlərdə, bir rəqəmin (number) baytları (bytes) `ArrayBuffer`-də ən az əhəmiyyətlidən ən çox əhəmiyyətliyə doğru düzülür.
* **Big-endian** platformalarda isə baytlar (bytes) ən çox əhəmiyyətlidən ən az əhəmiyyətliyə doğru düzülür.

Əsas platformanın (platform) endianness-ni (endianness) aşağıdakı kodla müəyyən edə bilərsiniz:

```javascript
// Əgər 0x00000001 tam ədədi (integer) yaddaşda (memory) 01 00 00 00 kimi düzülürsə,
// onda biz little-endian platformadayıq. Big-endian platformada isə, 00 00 00 01 baytları (bytes) alardıq.
let littleEndian = new Int8Array(new Int32Array([1]).buffer)[0] === 1;
console.log(`Cari platforma ${littleEndian ? 'Little-endian' : 'Big-endian'}`);
```
Bu gün ən yaygın CPU arxitekturaları **little-endian**-dır. Lakin bir çox şəbəkə protokolları (network protocols) və bəzi ikili fayl (binary file) formatları **big-endian** bayt (byte) ardıcıllığı (ordering) tələb edir. Əgər tipli massivləri (typed arrays) şəbəkədən (network) və ya fayldan (file) gələn data (data) ilə istifadə edirsinizsə, platformanın (platform) endianness-nin (endianness) data (data) bayt (byte) ardıcıllığına (order) uyğun gəldiyini fərz edə bilməzsiniz. Ümumiyyətlə, xarici data (external data) ilə işləyərkən, datanı (data) fərdi baytlar (individual bytes) massivi (array) kimi görmək üçün `Int8Array` və `Uint8Array` istifadə edə bilərsiniz, lakin digər tipli massivləri (typed arrays) çoxbaytlı (multibyte) söz ölçüləri (word sizes) ilə istifadə etməməlisiniz.

Bunun əvəzinə, **`DataView`** sinfini (class) istifadə edə bilərsiniz. Bu sinif (class) `ArrayBuffer`-dən (ArrayBuffer) açıq şəkildə təyin edilmiş bayt (byte) ardıcıllığı (ordering) ilə dəyərləri (values) oxumaq (reading) və yazmaq (writing) üçün metodlar (methods) təyin edir:

```javascript
// Fərz edək ki, emal olunacaq ikili datanın (binary data) baytlarının (bytes) tipli massivi (typed array) var.
let bytes = new Uint8Array([0x00, 0x00, 0x00, 0x01,  // 1 (big-endian)
                            0x00, 0x00, 0x00, 0x02,  // 2 (big-endian)
                            0x01, 0x00, 0x00, 0x00]); // 1 (little-endian)

// Öyrənmək üçün buferə (buffer) yazırıq
let buffer = new ArrayBuffer(12);
let tempBytes = new Uint8Array(buffer);
tempBytes.set(bytes); // datanı (data) buferə (buffer) kopyala.

// İndi DataView obyekti (object) yaradırıq ki, dəyərləri (values) rahat oxuyub yaza bilək.
let view = new DataView(buffer); // Bütün buferi (buffer) əhatə edir

// getInt32(byteOffset, isLittleEndian)
let int1 = view.getInt32(0, false); // Byte 0-dan big-endian işarəli (signed) int oxuyun
console.log(int1);                  // => 1

let int2 = view.getInt32(4, false); // Növbəti int də big-endian-dır
console.log(int2);                  // => 2

let uint3 = view.getUint32(8, true); // Növbəti int little-endian və işarəsizdir (unsigned)
console.log(uint3);                 // => 1

// setUint32(byteOffset, value, isLittleEndian)
view.setUint32(8, 5, false); // 8-ci baytdan başlayaraq 5-i big-endian formatda geri yazın
console.log(new Uint8Array(buffer)); // => Uint8Array [0, 0, 0, 1, 0, 0, 0, 2, 0, 0, 0, 5] (son 4 bayt dəyişdi)
```
`DataView` `Uint8ClampedArray` istisna olmaqla, 10 tipli massiv (typed array) sinfinin (class) hər biri üçün 10 `get` metodu (method) təyin edir. Onların adları `getInt16()`, `getUint32()`, `getBigInt64()` və `getFloat64()` kimidir. İlk arqument (argument) dəyərin (value) başladığı `ArrayBuffer` daxilindəki bayt ofsetidir (byte offset). `getInt8()` və `getUint8()` istisna olmaqla, bu getter metodlarının (getter methods) hamısı ikinci arqument (argument) kimi opsional (optional) boolean (true/false) dəyəri (value) qəbul edir. İkinci arqument (argument) atıldıqda və ya `false` olduqda, big-endian bayt (byte) ardıcıllığı (ordering) istifadə olunur. Əgər ikinci arqument (argument) `true` olarsa, little-endian ardıcıllığı (ordering) istifadə olunur.

`DataView` həmçinin əsas `ArrayBuffer`-ə (ArrayBuffer) dəyərləri (values) yazan 10 müvafiq `Set` metodu (method) təyin edir. İlk arqument (argument) dəyərin (value) başladığı ofsetdir (offset). İkinci arqument (argument) yazılacaq dəyərdir (value). `setInt8()` və `setUint8()` istisna olmaqla, metodların (methods) hər biri opsional (optional) üçüncü arqument (argument) qəbul edir. Arqument (argument) atıldıqda və ya `false` olduqda, dəyər (value) ən əhəmiyyətli bayt (byte) birinci olmaqla big-endian formatda yazılır. Arqument (argument) `true` olduqda isə, dəyər (value) ən az əhəmiyyətli bayt (byte) birinci olmaqla little-endian formatda yazılır.

Tipli massivlər (typed arrays) və `DataView` sinfi (class) ikili datanı (binary data) emal etmək üçün lazım olan bütün alətləri (tools) təmin edir və sizə ZIP fayllarını (files) dekompressiya (decompressing) etmək və ya JPEG fayllarından (files) metadata (metadata) çıxarmaq kimi şeylər edən JavaScript proqramları (programs) yazmağa imkan verir.

---


### **Fəsil 11.3: Requlyar İfadələrlə Nümunə Axtarışı**

**Requlyar ifadə** – mətndə müəyyən bir nümunəni (pattern) təsvir edən bir obyektdir. JavaScript-də `RegExp` sinfi requlyar ifadələri təmsil edir. Həm `String`, həm də `RegExp` sinifləri, mətn üzərində güclü axtarış və əvəzetmə əməliyyatları aparmaq üçün requlyar ifadələrdən istifadə edən metodlara sahibdir.

Bu mövzunu effektiv şəkildə istifadə etmək üçün requlyar ifadələrin qrammatikasını – yəni, bu kiçik proqramlaşdırma dilinin öz qaydalarını öyrənmək lazımdır. Xoşbəxtlikdən, JavaScript-in requlyar ifadə sintaksisi bir çox digər dillərlə oxşardır, buna görə də öyrəndikləriniz gələcəkdə başqa proqramlaşdırma dillərində də sizə faydalı olacaq.

---

#### **11.3.1 Requlyar İfadələrin Təyin Edilməsi**

JavaScript-də requlyar ifadələr iki əsas üsulla yaradılır: **literal sintaksis** və **konstruktor**.

**1. Literal Sintaksis (Ən Çox İstifadə Olunan Üsul)**

Bu üsulda ifadə iki əyri xətt (`/`) arasına yazılır. Bu, ən oxunaqlı və məsləhət görülən yoldur.

```javascript
let pattern = /skript/;
```

Bu nümunə, tərkibində "skript" sözü olan istənilən mətni tapacaq.

**2. `RegExp()` Konstruktoru**

Eyni ifadəni `new RegExp()` konstruktoru ilə də yaratmaq mümkündür. Bu zaman ifadə dırnaq içində bir sətir (string) olaraq yazılır.

```javascript
let pattern = new RegExp("skript");
```

**Fərqləri nədir?** Literal sintaksis daha sürətli və qısadır. Konstruktor isə ifadəni dinamik olaraq, yəni bir dəyişənin dəyərindən yaratmaq lazım olduqda faydalıdır.

##### **Sadə Simvollar və Meta-Simvollar**

Requlyar ifadələrdəki simvolların əksəriyyəti (məsələn, bütün hərflər və rəqəmlər) özünü ifadə edir. Yəni `/a/` ifadəsi sadəcə "a" hərfini axtarır.

Ancaq bəzi simvollar xüsusi məna daşıyır və **meta-simvol** adlanır. Məsələn:
* `$` — Sətrin (string) sonunu bildirir.
* `^` — Sətrin əvvəlini bildirir.

**Nümunə:**
`/s$/` ifadəsi iki simvoldan ibarətdir:
* `s` — "s" hərfini axtarır.
* `$` — həmin "s" hərfinin sətrin sonunda olmasını tələb edir.

Məsələn, "salam" sözü bu nümunəyə uyğun deyil, amma "əlaqələriniz" sözü uyğundur.

##### **Fleqlər (Flags)**

Fleqlər, axtarışın davranışını dəyişən xüsusi "açarlardır". Onlar ikinci əyri xətdən (`/`) sonra yazılır.

* `i` — **Case-Insensitive (Böyük-kiçik hərf fərqi olmadan):** Axtarış zamanı böyük və kiçik hərflər arasında fərq qoyulmur.
* `g` — **Global (Qlobal axtarış):** Yalnız ilk uyğunluğu deyil, bütün uyğun gələn nəticələri tapır.

**Nümunə:**
`/s$/i` — Sətrin "s" və ya "S" hərfi ilə bitib-bitmədiyini yoxlayır.

```javascript
let pattern_case_sensitive = /s$/;
let pattern_case_insensitive = /s$/i;

"salamS".test(pattern_case_sensitive);     // false (böyük S olduğu üçün)
"salamS".test(pattern_case_insensitive);  // true (i fleqi fərqi aradan qaldırır)
```

---

### **Əlavə və Praktik Nümunələr**

Nəzəriyyəni daha yaxşı başa düşmək üçün bəzi praktik nümunələrə baxaq. Requlyar ifadələri yoxlamaq üçün ən sadə metod `.test()` metodudur. O, uyğunluq varsa `true`, yoxdursa `false` qaytarır.

**Nümunə 1: Mətnin müəyyən sözlə başlamasının yoxlanılması**
`^` simvolu sətrin başlanğıcını bildirir.

```javascript
// "Salam" sözü ilə başlayan sətirləri axtarır
const pattern = /^Salam/;

console.log( pattern.test("Salam, dünya!") );      // ✅ true
console.log( pattern.test("salam, dünya!") );      // ❌ false (böyük hərf tələb olunur)
console.log( pattern.test("Əvvəlcə Salam, dünya!") ); // ❌ false (sətrin əvvəlində deyil)
```

**Nümunə 2: Yalnız rəqəmlərdən ibarət olmanın yoxlanılması**
`\d` meta-simvolu istənilən rəqəmi (`0-9`) ifadə edir. `+` isə "bir və ya daha çox" deməkdir.

```javascript
// Sətrin başdan sona (^...$) yalnız bir və ya daha çox rəqəmdən (\d+) ibarət olmasını yoxlayır
const pattern = /^\d+$/;

console.log( pattern.test("12345") );      // ✅ true
console.log( pattern.test("123a5") );      // ❌ false ("a" hərfi var)
console.log( pattern.test("123 45") );     // ❌ false (boşluq simvolu var)
console.log( pattern.test("12") );         // ✅ true
```

**Nümunə 3: E-mail formatının sadə yoxlanışı**
Bu, daha real bir nümunədir.

```javascript
// user@domain.com formatını yoxlayır (çox sadələşdirilmiş)
// \S+  -> bir və ya daha çox boşluq olmayan simvol
// @    -> @ simvolu
// \.   -> nöqtə simvolu
const emailPattern = /\S+@\S+\.\S+/;

console.log( emailPattern.test("test@example.com") );  // ✅ true
console.log( emailPattern.test("test.user@example.co.uk") ); // ✅ true
console.log( emailPattern.test("test@example") );      // ❌ false (.com, .net kimi hissə yoxdur)
console.log( emailPattern.test("test example.com") );  // ❌ false (@ simvolu yoxdur)
```

***

### Hərfi Mənada Simvollar (Literal Characters)

Requlyar ifadələrin necə işlədiyini anlamaq üçün ilk növbədə bilməliyik ki, hansı simvollar özünü, hansılar isə xüsusi bir əmri ifadə edir.

#### **1. Adi Simvollar: Hərflər və Rəqəmlər**

Bütün hərflər (`a-z`, `A-Z`) və rəqəmlər (`0-9`) requlyar ifadələrdə özlərini, yəni hərfi mənalarını ifadə edir.

```javascript
// "kod" sözünü axtarır
let pattern = /kod/;

pattern.test("JavaScript kod yazmaq maraqlıdır."); // ✅ true
pattern.test("Bu mətnin mənası fərqlidir.");     // ❌ false
```

#### **2. Xüsusi Mənalı Simvollar (Meta-simvollar)**

Bəzi durğu işarələri və simvollar requlyar ifadələrdə xüsusi məna daşıyır. Onlar hərfi mənada özlərini yox, müəyyən bir qaydanı təmsil edirlər. Bu simvollar bunlardır:

`^ $ . * + ? = ! : | \ / ( ) [ ] { }`

Bu simvolların hər birinin öz rolu var və növbəti fəsillərdə onları detallı öyrənəcəyik.

#### **3. Xüsusi Simvolları Hərfi Mənada Necə İşlətməli?**

Bəs əgər biz mətndə `.` (nöqtə) və ya `+` (üstəgəl) işarəsinin özünü axtarmaq istəsək, nə etməliyik?

**Qayda:** Xüsusi mənalı bir simvolu hərfi mənada (yəni olduğu kimi) axtarmaq üçün onun önünə tərs sleş `\` (backslash) əlavə etmək lazımdır. Bu prosesə "escaping" (sındırma və ya qaçırma) deyilir.

**Praktik Nümunə 1: Domen adında nöqtəni tapmaq**

Tutaq ki, `“sayt.az”` mətnindəki nöqtəni tapmaq istəyirik.

```javascript
let text = "Mənim veb-saytım sayt.az adresidir.";

// YANLIŞ YOL: /sayt.az/
// Bu ifadə "saytXaz" kimi uyğunluqları da tapacaq, çünki "." meta-simvolu
// "istənilən bir simvol" deməkdir.
let wrongPattern = /sayt.az/;
wrongPattern.test("sayt-az"); // ✅ true, amma biz bunu istəmirdik.

// DOĞRU YOL: /sayt\.az/
// Burada \. ifadəsi nöqtənin xüsusi mənasını "sındırır" və onu hərfi mənada axtarır.
let correctPattern = /sayt\.az/;
correctPattern.test(text);         // ✅ true
correctPattern.test("sayt-az");    // ❌ false
```

**Praktik Nümunə 2: "C++" dilini axtarmaq**

```javascript
let text = "Mən C++ öyrənirəm.";

// YANLIŞ YOL: /C++/
// `+` meta-simvolu "bir və ya daha çox" deməkdir. Bu ifadə "ondan əvvəlki C simvolu
// bir və ya daha çox dəfə təkrarlansın" mənasını verir. Yəni "C", "CC", "CCC" kimi...
let wrongPattern = /C++/;
wrongPattern.test("Mən C öyrənirəm."); // ✅ true, halbuki biz "C++" axtarırdıq.

// DOĞRU YOL: /C\+\+/
// Hər bir `+` simvolunun önünə `\` qoyaraq onları xüsusi mənasından azad edirik.
let correctPattern = /C\+\+/;
correctPattern.test(text); // ✅ true
```

**Qızıl qayda:** Əgər bir durğu işarəsinin xüsusi məna daşıyıb-daşımadığından əmin deyilsinizsə, qarşısına `\` qoyaraq özünüzü sığortalaya bilərsiniz. Məsələn, `@` simvolunun xüsusi mənası yoxdur, amma `/e-mail\@example\.com/` yazmaq tamamilə təhlükəsizdir.

#### **4. Nəzarət Simvolları və Tərs Sleş `\`**

Hərfi mənada görünməyən, ancaq mətndə mövcud olan bəzi xüsusi simvollar da (`\n` – yeni sətir, `\t` – tab) tərs sleş vasitəsilə ifadə olunur.

| Simvol | Mənası |
| :--- | :--- |
| `\n` | Yeni sətir |
| `\t` | Tab (boşluq) |
| `\r` | "Carriage return" |
| `\0` | NUL simvolu |
| `\\` | Tərs sleşin (`\`) özü |

Bəs tərs sleşin özünü (`\`) mətndə necə axtara bilərik? Onu da başqa bir tərs sleşlə "sındırmaq" lazımdır: `\\`.

**Nümunə: Windows fayl yolunu tapmaq**

```javascript
let path = "C:\\Users\\admin";

// Tərs sleşi tapmaq üçün \\ istifadə edirik
let pattern = /\\Users\\/;

pattern.test(path); // ✅ true
```

#### **DIQQƏT: `new RegExp()` konstruktoru ilə fərq**

Əgər requlyar ifadəni `new RegExp("...")` ilə yaradırsınızsa, tərs sleşləri **ikiqat** yazmalısınız. Çünki JavaScript sətirləri (`string`) də `\` simvolunu `escape` üçün istifadə edir.

```javascript
// Literal sintaksis:
let literalPattern = /\\/; // Tək bir \ axtarır

// Konstruktor sintaksisi:
// Birinci \ sətir (string) üçündür, ikinci \ isə requlyar ifadə üçün.
let constructorPattern = new RegExp("\\\\"); // Bu da eynilə tək bir \ axtarır.

console.log( literalPattern.test("C:\\") );       // ✅ true
console.log( constructorPattern.test("C:\\") ); // ✅ true
```
***

### Simvol Qrupları (Character Classes)

Tutaq ki, mətndə "a", "b" və ya "c" hərflərindən *hər hansı birini* tapmaq istəyirik. Bunu `/a|b|c/` kimi yaza bilərik, amma daha qısa və oxunaqlı bir yolu var: simvol qrupları.

Simvol qrupu, kvadrat mötərizə `[...]` içərisinə yazılan simvollar toplusudur və həmin topludan **yalnız bir simvolun** uyğun gəlməsini yoxlayır.

#### **1. Əsas Simvol Qrupları: `[...]`**

`/\[abc]/` ifadəsi "a", "b" və ya "c" hərflərindən hər hansı birinə uyğun gəlir.

```javascript
const pattern = /[abc]/;

pattern.test("salam");   // ✅ true ("a" hərfi var)
pattern.test("bina");    // ✅ true ("b" hərfi var)
pattern.test("kod");     // ❌ false (nə "a", nə "b", nə də "c" yoxdur)
pattern.test("abc");     // ✅ true (hər üçü var, ilk "a"-ya görə true qaytarır)
```

**Aralıqlar (Ranges):** Tək-tək bütün simvolları yazmaq əvəzinə, defis (`-`) ilə aralıq təyin edə bilərsiniz.
* `/[a-z]/` — Bütün kiçik latın hərflərindən hər hansı biri.
* `/[A-Z]/` — Bütün böyük latın hərflərindən hər hansı biri.
* `/[0-9]/` — Bütün rəqəmlərdən hər hansı biri.
* `/[a-zA-Z0-9_]/` — İstənilən hərf, rəqəm və ya alt xətt.

#### **2. İnkar Edilmiş Qruplar: `[^...]`**

Əgər mötərizənin içində ilk simvol kimi `^` (caret) işarəsi qoysanız, qrup tam əksinə işləyəcək: mötərizə içindəki simvollar **xaricində** istənilən bir simvolu axtaracaq.

`/[^abc]/` — "a", "b" və "c" hərfləri *istisna olmaqla* istənilən bir simvol.

```javascript
const pattern = /[^0-9]/; // Rəqəm olmayan istənilən simvol

pattern.test("123");  // ❌ false (bütün simvollar rəqəmdir)
pattern.test("12a");  // ✅ true ("a" simvolu rəqəm deyil)
pattern.test("salam"); // ✅ true (bütün simvollar rəqəm deyil)
```
**Diqqət:** Mötərizə daxilindəki `^` ilə requlyar ifadənin əvvəlindəki `^` (sətrin başlanğıcı) fərqli mənalar daşıyır.

#### **3. Hazır Qısayollar (Shorthands)**

Bəzi simvol qrupları o qədər çox istifadə olunur ki, onlar üçün xüsusi qısayollar mövcuddur.

| Qısayol | Açıqlaması | Ekvivalenti |
| :--- | :--- | :--- |
| **`.`** | **İstənilən simvol** (yeni sətir `\n` xaric) | |
| **`\d`** | İstənilən **rəqəm** (digit) | `[0-9]` |
| **`\D`** | Rəqəm **olmayan** istənilən simvol | `[^0-9]` |
| **`\w`** | İstənilən **hərf, rəqəm və ya `_`** (word character) | `[a-zA-Z0-9_]` |
| **`\W`** | `\w`-dəki simvollar **xaricində** qalan hər şey | `[^a-zA-Z0-9_]` |
| **`\s`** | İstənilən **boşluq** simvolu (space, tab, new line) | `[ \t\n\r\f\v]` |
| **`\S`** | Boşluq **olmayan** istənilən simvol | `[^ \t\n\r\f\v]` |

Bu qısayolları birbaşa və ya kvadrat mötərizələrin daxilində istifadə edə bilərsiniz. Məsələn, `/[\s\d]/` ifadəsi "istənilən boşluq simvolu və ya istənilən rəqəm" deməkdir.

**Praktik Nümunə:** Azərbaycan mobil nömrə formatının yoxlanılması.
`+994 (50) 123-45-67`

```javascript
// \d{2} -> 2 ədəd rəqəm deməkdir (növbəti dərsin mövzusu)
const numberPattern = /\+994\s\(\d{2}\)\s\d{3}-\d{2}-\d{2}/;

numberPattern.test("+994 (55) 321-09-87"); // ✅ true
numberPattern.test("+994 55 321-09-87");   // ❌ false (mötərizələr yoxdur)
numberPattern.test("994 (55) 321-09-87");  // ❌ false (+ işarəsi yoxdur)
```

#### **4. İstisnalar və Xüsusi Hallar**

* **Defis (`-`) simvolu:** Əgər qrup daxilində defis simvolunun özünü axtarmaq istəyirsinizsə, onu mütləq **ən sonda** yazın. Məsələn, `/\[a-z-]/` ifadəsi kiçik hərfləri və ya defis simvolunu axtarır.
* **Backspace (`\b`):** `\b` qısayolu normalda "söz sərhədi" (word boundary) deməkdir. Lakin mötərizə içində `[\b]` şəklində yazılarsa, o, hərfi mənada "backspace" simvolunu ifadə edən yeganə yoldur.

---
### **(Bonus) Unicode Simvol Qrupları: `\p{...}`**

Standart qısayollar (`\w`, `\d`) əsasən ASCII (latın əlifbası və ingilis dilinə aid) simvolları tanıyır. Məsələn, `\w` Azərbaycan əlifbasındakı `ə`, `ş`, `ü` kimi hərfləri tanımır.

Bu problemi həll etmək üçün müasir JavaScript (ES2018+) `u` (unicode) fleqi ilə birlikdə `\p{...}` sintaksisini dəstəkləyir.

`\p{...}` müəyyən Unicode xüsusiyyətinə malik bütün simvolları tapmağa imkan verir.

* `/\p{Script=Cyrillic}/u` — Kiril əlifbasına aid istənilən hərfi tapır.
* `/\p{Script=Arabic}/u` — Ərəb əlifbasına aid istənilən hərfi tapır.
* `/\p{Decimal_Number}/u` — Dünyanın istənilən yazısındakı rəqəmləri tapır (`\d`-dən fərqli olaraq).
* `/\p{Alphabetic}/u` — İstənilən əlifbaya aid olan bütün hərfləri tapır (o cümlədən `ə`, `ş`, `ü`).

İnkar forması üçün böyük `\P{...}` istifadə olunur.

```javascript
let pattern = /\p{Alphabetic}/u;

pattern.test("salam"); // ✅ true
pattern.test("şüşə");  // ✅ true (`\w` ilə false olardı)
pattern.test("123");   // ❌ false
```
***

### **Fəsil 11.3.4: Təkrarlayıcılar (Repetition)**

Əvvəlki dərslərdə öyrəndiyimiz sintaksislə iki rəqəmli ədədi `/\d\d/` və ya dörd rəqəmli ədədi `/\d\d\d\d/` kimi ifadə edə bilərdik. Bəs istənilən sayda rəqəmdən ibarət bir ədədi və ya üç hərfdən sonra gələn, ancaq mütləq olmayan (könüllü) bir rəqəmi necə təsvir edə bilərik?

Bu cür daha mürəkkəb nümunələri qurmaq üçün requlyar ifadələrin **təkrarlanma** sintaksisindən istifadə edirik. Bu operatorlar, onlardan əvvəl gələn simvolun, qrupun və ya sinfin neçə dəfə təkrarlana biləcəyini göstərir.

Təkrarlama operatorları həmişə tətbiq olunduğu nümunədən **sonra** gəlir.

#### **Təkrarlama Operatorları**

Aşağıdakı cədvəldə təkrarlama operatorları və onların mənaları verilmişdir.

| Operator | Mənası |
| :--- | :--- |
| **`{n}`** | Əvvəlki elementdən **tam olaraq `n` dəfə** olmalıdır. |
| **`{n,m}`** | Əvvəlki elementdən **minimum `n`**, **maksimum `m` dəfə** olmalıdır. |
| **`{n,}`** | Əvvəlki elementdən **minimum `n` dəfə** (və daha çox) olmalıdır. |
| **`+`** | Əvvəlki elementdən **bir və ya daha çox dəfə** olmalıdır. Ekvivalenti: `{1,}`. |
| **`*`** | Əvvəlki elementdən **sıfır və ya daha çox dəfə** olmalıdır. Ekvivalenti: `{0,}`. |
| **`?`** | Əvvəlki element **könüllüdür** (sıfır və ya bir dəfə). Ekvivalenti: `{0,1}`. |

#### **Praktik Nümunələr**

**1. `{n}` — Dəqiq sayla təkrarlanma**
4 rəqəmli PİN kod axtarışı:

```javascript
let pattern = /^\d{4}$/; // Sətrin başdan sona yalnız 4 rəqəmdən ibarət olmasını yoxlayır

pattern.test("1234");    // ✅ true
pattern.test("123");     // ❌ false (3 rəqəmdir)
pattern.test("12345");   // ❌ false (5 rəqəmdir)
pattern.test("12a4");    // ❌ false (hərf var)
```

**2. `{n,m}` — Aralıqla təkrarlanma**
5-10 simvoldan ibarət istifadəçi adı (username) axtarışı:

```javascript
let pattern = /^\w{5,10}$/; // Yalnız hərf, rəqəm və alt xətdən ibarət, 5-10 simvol uzunluğunda

pattern.test("user123");  // ✅ true (7 simvol)
pattern.test("user");     // ❌ false (4 simvol)
pattern.test("user_long_name"); // ❌ false (14 simvol)
```

**3. `+` — Bir və ya daha çox**
"Java" sözünün əvvəlində və sonunda ən az bir boşluq olan sətirləri tapmaq:

```javascript
let pattern = /\s+java\s+/;

pattern.test("  java  "); // ✅ true
pattern.test(" java ");   // ✅ true
pattern.test("java");     // ❌ false (boşluq yoxdur)
```

**4. `?` — Sıfır və ya bir dəfə (Könüllü)**
Həm "color" (ABŞ), həm də "colour" (BK) sözlərini tapmaq:

```javascript
let pattern = /colou?r/; // "u" hərfi ya heç olmamalı, ya da bir dəfə olmalıdır

pattern.test("color");   // ✅ true
pattern.test("colour");  // ✅ true
pattern.test("colouur"); // ❌ false ("u" iki dəfədir)
```

**5. `*` — Sıfır və ya daha çox**
Açıq mötərizə `(` olmayan istənilən sayda simvolu tapmaq:

```javascript
let pattern = /[^(]*/;

pattern.test("salam necesen?");  // ✅ true
pattern.test("bu (mesaj) gizlidir"); // `(` simvoluna qədər olan "bu " hissəsini tapacaq
```

---

### **Diqqət Edilməli Vacib Məqamlar**

**1. Təkrarlayıcı Nəyə Tətbiq Olunur?**
Təkrarlayıcılar yalnız onlardan **dərhal əvvəl gələn tək bir elementə** (simvol, simvol qrupu və ya qısayol) təsir edir.

* `/\d{3}/` — Üç ədəd rəqəm deməkdir (`\d\d\d`).
* `/ab{3}/` — "a" hərfi və ondan sonra gələn üç ədəd "b" hərfi deməkdir (`abbb`). Bu ifadə "ababab" demək DEYİL.

Əgər bir neçə simvoldan ibarət bir qrupu təkrarlamaq istəyirsinizsə, onları mötərizə `()` içinə almalısınız. Bu, növbəti dərsin mövzusudur. Məsələn, `/(ab){3}/` ifadəsi "ababab" deməkdir.

**2. `*` və `?` Təhlükəsi: Heç Nəyə Uyğunlaşma**
`*` (sıfır və ya daha çox) və `?` (sıfır və ya bir) operatorları **heç nəyə** də uyğun gələ bilər. Bu, bəzən gözlənilməz nəticələrə yol aça bilər.

Məsələn, `/a*/` ifadəsini götürək. Bu ifadə "sıfır və ya daha çox 'a' hərfi" deməkdir.

```javascript
let pattern = /a*/;

pattern.test("aaaa"); // ✅ true (4 'a' var)
pattern.test("b");    // ✅ true (Çünki "b" sətrinin əvvəlində SIFIR sayda 'a' var)
```
Bu ikinci nəticə çaşdırıcı görünə bilər. Requlyar ifadə mühərriki sətrin əvvəlinə baxır və soruşur: "Burada sıfır və ya daha çox 'a' varmı?". Cavab "Bəli, burada sıfır 'a' var" olduğu üçün nəticə `true` olur. Buna görə də bu operatorları istifadə edərkən diqqətli olmaq lazımdır.

***

### **Fəsil 11.3.5: Acgöz Olmayan (Tənbəl) Təkrarlayıcılar**

Əvvəlki fəsildə öyrəndiyimiz təkrarlayıcılar (`*`, `+`, `{n,m}`) standart olaraq **"acgöz" (greedy)** rejimdə işləyir. Bu o deməkdir ki, onlar requlyar ifadənin qalan hissəsinin də uyğun gəlməsinə imkan verəcək şəkildə **mümkün olan ən uzun** simvol ardıcıllığını ələ keçirməyə çalışırlar.

Lakin bəzən bizə bunun əksi lazımdır: mümkün olan **ən qısa** uyğunluğu tapmaq. Bu davranış **"acgöz olmayan"** və ya **"tənbəl" (lazy/non-greedy)** rejim adlanır.

Təkrarlayıcını "tənbəl" etmək çox asandır: sadəcə ondan sonra bir sual işarəsi `?` əlavə edin:
* `*` (acgöz) → `*?` (tənbəl)
* `+` (acgöz) → `+?` (tənbəl)
* `??` (acgöz) → `??` (tənbəl)
* `{n,m}` (acgöz) → `{n,m}?` (tənbəl)

#### **Acgöz və Tənbəl Rejimin Fərqi**

Bu fərqi anlamaq üçün ən yaxşı nümunə HTML teqləri arasındakı mətni tapmaqdır. Tutaq ki, belə bir sətrimiz var:

`let text = "<b>JavaScript</b> və <b>CSS</b>";`

Bizim məqsədimiz `<b>` teqləri arasındakı məzmunu tapmaqdır.

**1. Acgöz `.*` ilə Yanaşma (Yanlış Nəticə)**

```javascript
let greedyPattern = /<b>.*<\/b>/;
console.log( text.match(greedyPattern)[0] );
// Nəticə: "<b>JavaScript</b> və <b>CSS</b>"
```
**Niyə belə oldu?** `.*` ifadəsi "istənilən simvoldan sıfır və ya daha çox" deməkdir və acgöz olduğu üçün ilk `<b>`-dən sonra dayana biləcəyi ən son `</b>`-ə qədər hər şeyi (o cümlədən birinci `</b>` və `<b>` teqlərini) ələ keçirdi.

**2. Tənbəl `*?` ilə Yanaşma (Düzgün Nəticə)**

```javascript
// matchAll metodu bütün uyğunluqları tapır
let lazyPattern = /<b>.*?<\/b>/g; // g fleqi qlobal axtarış üçündür
let matches = text.matchAll(lazyPattern);

for (const match of matches) {
  console.log(match[0]);
}
// Nəticə:
// "<b>JavaScript</b>"
// "<b>CSS</b>"
```
**Burada nə baş verdi?** `*?` tənbəl olduğu üçün, `<b>`-dən sonra gələn `</b>` teqini tapdığı **ilk anda** dayanır və uyğunluğu tamamlayır. Sonra axtarışa davam edərək növbəti uyğunluğu tapır.


---

### Növbələşmə (Alternation): `|`

`|` simvolu "və ya" deməkdir. Soldakı və ya sağdakı ifadədən biri ilə uyğunluq axtarır.

```javascript
// .js, .css, və ya .html ilə bitən faylları axtarır
const filePattern = /\.(js|css|html)$/;

console.log(filePattern.test("script.js"));   // ✅ true
console.log(filePattern.test("style.css"));    // ✅ true
console.log(filePattern.test("archive.zip"));  // ❌ false
```

**Diqqət:** `|` soldan sağa işləyir. Soldakı variant uyğun gələn kimi, sağdakı yoxlanılmır.

```javascript
// "ab" sətrində "a" tapılan kimi axtarış dayanır.
console.log("ab".match(/a|ab/)); // Nəticə: ["a"]
```

---

### Qruplaşdırma və İstinadlar: `()`

Mötərizələrin bir neçə məqsədi var:

**1. Əməliyyatları Birləşdirmək:** Təkrarlayıcıları (`?`, `*`, `+`) tək bir simvola yox, bütöv bir qrupa tətbiq etmək üçün istifadə olunur.

```javascript
// "JavaScript" və ya "Java" sözlərini tapır. "?" bütün "Script" sözünə aiddir.
const pattern = /Java(Script)?/;

console.log(pattern.test("JavaScript")); // ✅ true
console.log(pattern.test("Java"));       // ✅ true
```

**2. Nümunəni Yadda Saxlamaq (Capturing):** Mötərizələr, uyğun gəldikləri mətni "yadda saxlayır" və bu hissələri sonradan əldə etmək mümkün olur.

```javascript
// Məqsəd: istifadəçinin adını və domenini ayrı-ayrı əldə etmək
const email = "user@example.com";
const pattern = /(\w+)@(\w+\.\w+)/;
const match = email.match(pattern);

if (match) {
  console.log("Tam uyğunluq:", match[0]);   // user@example.com
  console.log("Birinci qrup (ad):", match[1]);  // user
  console.log("İkinci qrup (domen):", match[2]); // example.com
}
```

**3. Geri İstinadlar (Back-references: `\1`, `\2`):** Bir qrupun yadda saxladığı mətni eyni requlyar ifadənin içində yenidən istifadə etməyə imkan verir. `\1` birinci qrupa, `\2` ikinci qrupa istinad edir.

Ən yaxşı nümunə, açılış və bağlanış dırnaq işarələrinin eyni olmasını təmin etməkdir:

```javascript
// Bura diqqət: \1 simvolu, birinci mötərizənin (['"]) yadda saxladığı simvolun
// eynisini tələb edir.
const quotePattern = /(['"])(.*?)\1/;

console.log(quotePattern.test(`'salam'`)); // ✅ true (hər iki tərəf tək dırnaqdır)
console.log(quotePattern.test(`"dünya"`)); // ✅ true (hər iki tərəf cüt dırnaqdır)
console.log(quotePattern.test(`'səhv"`));  // ❌ false (dırnaqlar fərqlidir)
```

---

### Yadda Saxlamayan Qruplar: `(?:...)`

Bəzən sadəcə qruplaşdırma etmək lazımdır, amma həmin hissəni yadda saxlamağa və ya ona nömrə verilməsinə ehtiyac yoxdur. Bu zaman `(?:...)` istifadə olunur. Bu, həm requlyar ifadəni optimallaşdırır, həm də nömrələməni qarışdırmır.

```javascript
const date = "2025-06-12";
// Burada ili yadda saxlamaq istəmirik, yalnız ay və günü
const pattern = /(?:\d{4})-(\d{2})-(\d{2})/;
const match = date.match(pattern);

if (match) {
  console.log("Tam uyğunluq:", match[0]); // 2025-06-12
  console.log("Birinci qrup (ay):", match[1]);  // 06 (il yadda saxlanmadığı üçün bu \1 oldu)
  console.log("İkinci qrup (gün):", match[2]);  // 12
}
```

---

### Adlandırılmış Qruplar (Named Groups): `(?<ad>...)`

Bu müasir (ES2018) xüsusiyyət, qrupları nömrələrlə (`\1`, `\2`) deyil, adlarla çağırmağa imkan verir. Bu, ifadəni daha oxunaqlı edir.

```javascript
const text = "Şəhər: Bakı, Ölkə: Azərbaycan";
const pattern = /Şəhər: (?<city>\w+), Ölkə: (?<country>\w+)/;
const match = text.match(pattern);

if (match) {
  console.log(match.groups.city);    // "Bakı"
  console.log(match.groups.country); // "Azərbaycan"
}
```

Adlandırılmış qruplara istinad `\k<ad>` ilə edilir:

```javascript
const quotePattern = /(?<quote>['"])(.*?)\k<quote>/;

console.log(quotePattern.test(`'düzgündür'`)); // ✅ true
console.log(quotePattern.test(`"səhvdir'`));   // ❌ false
```
***

### (SPECIFYING MATCH POSITION) Uyğunluğun Mövqeyini Bildirən Operatorlar

Bəzi operatorlar simvolları yox, onların arasındakı **mövqeləri** yoxlayır. Bunlara "lövbər" (anchor) deyilir, çünki nümunəni sətrin (string) müəyyən bir yerinə "bağlayırlar".

#### 1. Lövbərlər (Anchors): `^`, `$`, `\b`, `\B`

* **`^`** — Sətrin (string) **əvvəlini** bildirir.
* **`$`** — Sətrin (string) **sonunu** bildirir.
* **`\b`** — **Söz sərhədini (word boundary)** bildirir. Söz simvolu (`\w`) ilə qeyri-söz simvolu (`\W`) arasındakı mövqedir.
* **`\B`** — Söz sərhədi (word boundary) **olmayan** mövqeni bildirir.

```javascript
// Yalnız "salam" sözündən ibarət sətrə uyğun gəlir
console.log( /^salam$/.test("salam") ); // ✅ true
console.log( /^salam$/.test("çox salam") ); // ❌ false

// "kod" sözünü ayrıca bir söz kimi tapır
const wordPattern = /\bkod\b/;
console.log( wordPattern.test("kod yazmaq") );   // ✅ true
console.log( wordPattern.test("dekodlaşdırmaq") ); // ❌ false ('kod' sözün içindədir)

// "az" hissəsinin sözün içində olmasını tələb edir
const innerPattern = /\Baz\B/;
console.log( innerPattern.test("bazar") ); // ✅ true
console.log( innerPattern.test("azad") );  // ❌ false ('az' sözün başındadır)
```

#### 2. İrəli Baxış (Lookahead): `(?=...)` və `(?!...)`

Bunlar mövqeni yoxlayır, amma yoxladıqları simvolları uyğunluğun nəticəsinə daxil etmir ("baxır, amma götürmür").

* **`(?=p)` (Positive Lookahead):** Əgər `p` nümunəsi gəlirsə, uyğunluğu tap.
* **(?!p) (Negative Lookahead):** Əgər `p` nümunəsi gəlmirsə, uyğunluğu tap.

```javascript
// Yalnız "$" işarəsindən əvvəl gələn rəqəmləri tapır
const pricePattern = /\d+(?=\$)/;
console.log("Qiymət: 50$".match(pricePattern)); // ✅ Nəticə: ["50"] ('$' daxil deyil)

// "Script" sözü ilə davam etməyən "Java" sözünü tapır
const javaPattern = /Java(?!Script)/;
console.log(javaPattern.test("JavaBeans"));   // ✅ true
console.log(javaPattern.test("JavaScript"));  // ❌ false
```

#### 3. Geri Baxış (Lookbehind): `(?<=...)` və `(?<!...)`

İrəli baxış (lookahead) kimidir, ancaq mövqenin **əvvəlində** nə olduğunu yoxlayır. (ES2018+).

* **`(?<=p)` (Positive Lookbehind):** Əgər mövqedən əvvəl `p` varsa, uyğunluğu tap.
* **`(?<!p)` (Negative Lookbehind):** Əgər mövqedən əvvəl `p` yoxdursa, uyğunluğu tap.

```javascript
// Yalnız "AZN " mətnindən sonra gələn rəqəmləri tapır
const aznPattern = /(?<=AZN\s)\d+/;
console.log("Məbləğ: AZN 150".match(aznPattern)); // ✅ Nəticə: ["150"]

// Yalnız rəqəm olmayan simvoldan sonra gələn rəqəmi tapır
const notDigitPattern = /(?<!\d)1/;
console.log("a1 b2 11".match(notDigitPattern)); // ✅ Nəticə: ["1"] ('a'dan sonraki '1')
```

---

### Fleqlər (Flags)

Fleqlər (flags), requlyar ifadənin ümumi davranışını dəyişən modifikatorlardır və `/.../`-dən sonra yazılır.

* `g` (global) — Yalnız ilkini deyil, **bütün uyğunluqları** tapır.
* `i` (case-insensitive) — Böyük-kiçik hərf fərqini aradan qaldırır.
* `m` (multiline) — `^` və `$` həm də hər sətrin əvvəli və sonu üçün işləyir.
* `s` (dotall) — `.` simvolunun yeni sətir (`\n`) simvolunu da əhatə etməsinə imkan verir.
* `u` (unicode) — Emoji kimi çoxbaytlı simvolların düzgün işlənməsini təmin edir.
* `y` (sticky) — Yalnız son uyğunluğun bitdiyi mövqedən axtarışa başlayır.

```javascript
// g - global
console.log("test test".match(/test/g)); // ✅ Nəticə: ["test", "test"]

// i - case-insensitive
console.log("Salam".match(/salam/i)); // ✅ Nəticə: ["Salam"]

// m - multiline
const text = "bir\niki\nüç";
console.log(text.match(/^\w+/gm)); // ✅ Nəticə: ["bir", "iki", "üç"]

// s - dotall
console.log(/./s.test("\n")); // ✅ true

// u - unicode
console.log(/^.$/u.test("😊")); // ✅ true
console.log(/^.$/.test("😊")); // ❌ false ( 'u' fleqi olmadan emoji 2 simvol sayılır)
```

***

### Sətir (String) Metodları ilə Nümunə Axtarışı

Requlyar ifadələri (Regular Expressions) JavaScript kodunda istifadə etmək üçün `String` obyektinin 4 əsas metodu var.

---

#### 1. `search()` Metodu

Sətrin (string) daxilində requlyar ifadəyə uyğun ilk alt-sətrin başlanğıc indeksini (index) qaytarır. Uyğunluq tapılmazsa, `-1` qaytarır.

**Nümunə 1: Sadə axtarış**
```javascript
let text = "JavaScript öyrənmək üçün əla dildir.";

// /üçün/ requlyar ifadəsi ilə axtarış
let index = text.search(/üçün/);

console.log(`'üçün' sözü ${index} indeksindən başlayır.`); // ✅ Nəticə: 'üçün' sözü 19 indeksindən başlayır.
```

**Nümunə 2: Uğursuz axtarış**
```javascript
let text = "JavaScript öyrənmək üçün əla dildir.";
let index = text.search(/Python/);

if (index === -1) {
  console.log("Axtarılan söz tapılmadı."); // ✅ Nəticə: Axtarılan söz tapılmadı.
}
```

**Nümunə 3: Sətir (String) arqumenti ilə axtarış**
`search()` metodu arqument olaraq sətir (string) də qəbul edə bilir və onu avtomatik requlyar ifadəyə çevirir.
```javascript
let text = "JavaScript öyrənmək üçün əla dildir.";

// 'dildir' sözünü birbaşa sətir kimi axtarmaq
let index = text.search("dildir");

console.log(index); // ✅ Nəticə: 33
```
---

#### 2. `replace()` Metodu

Sətrin daxilində tapılan nümunəni yeni bir sətirlə əvəz edir.

**Nümunə 1: Sadə qlobal (global) əvəzetmə**
```javascript
let text = "Status: aktiv, İstifadəçi: aktiv, Admin: aktiv";

// Bütün "aktiv" sözlərini "passiv" ilə əvəz edir
let newText = text.replace(/aktiv/g, "passiv");

console.log(newText); // ✅ Nəticə: "Status: passiv, İstifadəçi: passiv, Admin: passiv"
```

**Nümunə 2: Qrup istinadları (`$1`, `$2`) ilə format dəyişdirmə**
Tutaq ki, `YYYY-MM-DD` formatında olan tarixi `DD.MM.YYYY` formatına çevirmək istəyirik.
```javascript
let date = "Tarix: 2025-06-12";
// (\d{4}) -> $1 (il), (\d{2}) -> $2 (ay), (\d{2}) -> $3 (gün)
let newDate = date.replace(/(\d{4})-(\d{2})-(\d{2})/, "$3.$2.$1");

console.log(newDate); // ✅ Nəticə: "Tarix: 12.06.2025"
```

**Nümunə 3: Funksiya (function) ilə dinamik əvəzetmə**
Mətndəki bütün sözləri böyük hərflərə çevirək.
```javascript
let text = "salam dünya, necəsən?";

let upperCaseText = text.replace(/\w+/g, (word) => {
  // `replace` hər tapdığı söz üçün bu funksiyanı çağırır
  return word.toUpperCase();
});

console.log(upperCaseText); // ✅ Nəticə: "SALAM DÜNYA, NECƏSƏN?"
```

**Nümunə 4: Adlandırılmış qruplar (`$<name>`) ilə əvəzetmə**
```javascript
let user = "Ad: John, Soyad: Doe";

// `key` və `value` adında qruplar yaradırıq
let pattern = /(?<key>\w+): (?<value>\w+)/g;
let reordered = user.replace(pattern, "$<value> ($<key>)");

console.log(reordered); // ✅ Nəticə: "John (Ad), Doe (Soyad)"
```
---

#### 3. `match()` Metodu

Sətrin daxilindəki uyğunluqları bir massiv (array) şəklində qaytarır.

**Nümunə 1: `g` fleqi ilə bütün uyğunluqları tapmaq**
Mətndəki bütün hashtag-ləri tapaq.
```javascript
let post = "Mənim sevimli dillərim #JavaScript və #HTML, həmçinin #CSS.";
let hashtags = post.match(/#\w+/g);

console.log(hashtags); // ✅ Nəticə: ["#JavaScript", "#HTML", "#CSS"]
```

**Nümunə 2: `g` fleqi olmadan detallı məlumat əldə etmək**
`match()` `g` fleqi olmadan istifadə edildikdə, tapdığı ilk uyğunluq haqqında bütün detalları qaytarır.
```javascript
let url = "ftp://files.example.com/docs/report.pdf";
let pattern = /(\w+):\/\/([\w.-]+)\/(.*)/; // 3 ədəd yadda saxlayan qrup var
let result = url.match(pattern);

if (result) {
  console.log("Tam uyğunluq:", result[0]);   // ftp://files.example.com/docs/report.pdf
  console.log("Protokol (qrup 1):", result[1]); // ftp
  console.log("Host (qrup 2):", result[2]);     // files.example.com
  console.log("Yol (qrup 3):", result[3]);      // docs/report.pdf
  console.log("Başlanğıc indeksi:", result.index); // 0
}
```

**Nümunə 3: Uğursuz axtarış**
Uyğunluq tapılmadıqda `match()` metodu `null` qaytarır.
```javascript
let text = "salam";
let result = text.match(/\d+/); // Rəqəm axtarır

console.log(result); // ✅ Nəticə: null
```

---

#### 4. `matchAll()` Metodu

Bu metod (ES2020), `g` fleqi ilə istifadə edilərək bütün uyğunluqlar haqqında detallı məlumat verən bir **iterator** qaytarır. Bu, hər bir uyğunluğun həm dəyərini, həm də detallarını (indeks, qruplar) əldə etmək üçün idealdır.

**Nümunə: `key=value` cütlərini emal etmək**
```javascript
const query = "id=123&user=admin&token=xyz";
const pattern = /(?<key>\w+)=(?<value>\w+)/g; // Adlandırılmış qruplardan istifadə edirik

const matches = query.matchAll(pattern);

for (const match of matches) {
  console.log(`Açar: ${match.groups.key}, Dəyər: ${match.groups.value}, İndeks: ${match.index}`);
}
// Nəticə:
// Açar: id, Dəyər: 123, İndeks: 0
// Açar: user, Dəyər: admin, İndeks: 8
// Açar: token, Dəyər: xyz, İndeks: 20
```

---

#### 5. `split()` Metodu

Sətri (string) verilmiş ayırıcıya (separator) görə bölərək alt-sətirlərdən ibarət bir massiv (array) yaradır.

**Nümunə 1: Kompleks ayırıcı ilə bölmə**
Sətri həm vergül, həm nöqtə, həm də nida işarəsinə görə sözlərə ayırmaq.
```javascript
let sentence = "Salam, dünya! Necəsən?";
let words = sentence.split(/[,\s!.?]+/); // \s+ boşluqları da təmizləyir

console.log(words); // ✅ Nəticə: ["Salam", "dünya", "Necəsən", ""]
```

**Nümunə 2: Yadda saxlayan qruplarla (capturing groups) bölmə**
Əgər ayırıcıda yadda saxlayan qrup varsa, həmin qrupun məzmunu da nəticə massivinə (array) daxil edilir.
```javascript
const text = "1-ci addım, 2-ci addım, 3-cü addım";
const pattern = /(, )/; // Ayırıcını yadda saxlayırıq

const parts = text.split(pattern);

console.log(parts); // ✅ Nəticə: ["1-ci addım", ", ", "2-ci addım", ", ", "3-cü addım"]
```

***

### `RegExp` Sinifi (Class)

Bu fəsildə `RegExp` obyektinin özünü, onun konstruktorunu (constructor), xüsusiyyətlərini (properties) və metodlarını (methods) araşdıracağıq.

---

#### `RegExp()` Konstruktoru (Constructor)

Requlyar ifadələri təkcə literal `/.../` sintaksisi ilə deyil, həm də `new RegExp()` konstruktoru ilə yaratmaq mümkündür. Bu, xüsusilə requlyar ifadəni dinamik olaraq, məsələn, istifadəçidən gələn bir dəyərə görə yaratmaq lazım olduqda faydalıdır.

**Arqumentlər:**
1.  **Nümunə (pattern):** Requlyar ifadənin gövdəsini təmsil edən bir sətir (string).
2.  **Fleqlər (flags):** İkinci, könüllü (optional) arqument. Fleqləri təmsil edən sətir (`"g"`, `"i"`, `"gi"` və s.).

**Diqqət:** Sətir (string) içində `\` xüsusi məna daşıdığı üçün, requlyar ifadədəki hər bir `\` simvolu üçün ikiqat `\\` yazılmalıdır.

**Nümunə 1: Dinamik axtarış nümunəsi yaratmaq**
İstifadəçinin daxil etdiyi sözü böyük-kiçik hərf fərqi olmadan bütün mətndə axtaran bir ifadə yaradaq.
```javascript
function searchInText(searchTerm, text) {
  // İstifadəçinin daxil etdiyi dəyərdən dinamik RegExp yaradırıq
  // 'i' fleqi - case-insensitive, 'g' fleqi - global
  const pattern = new RegExp(searchTerm, "ig");
  
  const matches = text.match(pattern);
  
  if (matches) {
    console.log(`'${searchTerm}' sözü ${matches.length} dəfə tapıldı:`, matches);
  } else {
    console.log("Uyğunluq tapılmadı.");
  }
}

searchInText("kod", "Kod yazmaq hər zaman maraqlıdır. Bu kod çox sadədir.");
// ✅ Nəticə: 'kod' sözü 2 dəfə tapıldı: ["Kod", "kod"]
```

**Nümunə 2: `\` simvolunun istifadəsi**
Beş rəqəmli poçt kodunu (`\d{5}`) axtaran ifadə yaradaq.
```javascript
// \d yazmaq üçün sətir daxilində \\d yazmalıyıq.
const zipcodePattern = new RegExp("\\d{5}", "g");

console.log("10001 10002 10003".match(zipcodePattern)); // ✅ Nəticə: ["10001", "10002", "10003"]
```

**Nümunə 3: Requlyar ifadəni kopyalayıb fleqlərini dəyişmək**
```javascript
const originalPattern = /JavaScript/g;
// Orijinal ifadəni kopyalayırıq, amma ona 'i' fleqini əlavə edirik
const caseInsensitivePattern = new RegExp(originalPattern, "i");

console.log(caseInsensitivePattern.test("javascript")); // ✅ Nəticə: true
console.log(caseInsensitivePattern.flags); // ✅ Nəticə: "i" (g fleqi silindi)
```

---

#### `RegExp` Metodları: `test()` və `exec()`

`RegExp` obyektinin özünün də iki əsas metodu var.

##### `test()` Metodu
Ən sadə metoddur. Verilmiş sətrin (string) requlyar ifadəyə uyğun gəlib-gəlmədiyini yoxlayır və `true` və ya `false` qaytarır.

```javascript
const pattern = /^\d+$/; // Yalnız rəqəmlərdən ibarət olub-olmadığını yoxlayır

console.log(pattern.test("12345")); // ✅ Nəticə: true
console.log(pattern.test("12a45")); // ❌ Nəticə: false
```

##### `exec()` Metodu
Ən güclü metoddur. `match()` metodunun qlobal (global) olmayan halı kimi, hər zaman tapdığı **ilk uyğunluq haqqında detallı** bir massiv (array) qaytarır. Uyğunluq tapmazsa `null` qaytarır. `g` və ya `y` fleqləri ilə istifadə edildikdə, hər dəfə çağırıldıqda növbəti uyğunluğu axtarır.

**Nümunə: `exec()` ilə bütün uyğunluqlar üzərində dövr etmək (loop)**
```javascript
const text = "Rəng: #FFFFFF, Fon: #000000, Çərçivə: #FF9900";
const hexPattern = /#([A-F0-9]{6})/gi;
let match;

// exec() hər dəfə bir uyğunluq tapır. Tapacaq heç nə qalmayanda null qaytarır və dövr dayanır.
while ((match = hexPattern.exec(text)) !== null) {
  console.log(`Tapıldı: ${match[0]}, Rəng kodu: ${match[1]}, İndeks: ${match.index}`);
}
// Nəticə:
// Tapıldı: #FFFFFF, Rəng kodu: FFFFFF, İndeks: 7
// Tapıldı: #000000, Rəng kodu: 000000, İndeks: 24
// Tapıldı: #FF9900, Rəng kodu: FF9900, İndeks: 43
```

---

#### `lastIndex` Xüsusiyyəti (Property) və Onun Yaratdığı Problemlər

`g` və ya `y` fleqləri ilə işləyən `exec()` və `test()` metodları `lastIndex` adlanan bir xüsusiyyətdən (property) asılıdır. Bu xüsusiyyət növbəti axtarışın haradan başlayacağını bildirir və hər uğurlu axtarışdan sonra avtomatik yenilənir. Bu, bəzən gözlənilməz problemlərə yol aça bilər.

**Problem 1: Sonsuz dövr (Infinite Loop)**
Əgər `RegExp` literalı `while` dövrünün şərtinin daxilində yazılarsa, hər dövrdə yeni bir obyekt yaranır və `lastIndex` sıfırlanır. Bu da sonsuz dövrə səbəb olur.

```javascript
// SƏHV KOD! Sonsuz dövrə girəcək.
// while ((match = /a/g.exec("ab a")) !== null) { ... }

// DÜZGÜN KOD!
const pattern = /a/g;
let match;
while ((match = pattern.exec("ab a")) !== null) {
  console.log(`'a' tapıldı, indeks: ${match.index}. Növbəti axtarış ${pattern.lastIndex}-dən başlayacaq.`);
}
// Nəticə:
// 'a' tapıldı, indeks: 0. Növbəti axtarış 1-dən başlayacaq.
// 'a' tapıldı, indeks: 3. Növbəti axtarış 4-dən başlayacaq.
```

**Problem 2: Buraxılmış uyğunluqlar**
Eyni qlobal (global) requlyar ifadə obyekti birdən çox sətir (string) üzərində istifadə edildikdə, `lastIndex` sıfırlanmadığı üçün bəzi uyğunluqlar tapılmaya bilər.

```javascript
const wordList = ["book", "apple", "coffee"];
const doubleLetterPattern = /(\w)\1/g; // Qoşa hərf axtarır

// 'book' sözündə 'oo' tapılır və lastIndex yenilənir.
// 'apple' sözündə axtarış sıfırdan başlamadığı üçün 'pp' buraxıla bilər.
for (const word of wordList) {
  doubleLetterPattern.lastIndex = 0; // HƏLL YOLU: Hər yeni sözdən əvvəl lastIndex-i sıfırlamaq
  if (doubleLetterPattern.test(word)) {
    console.log(`'${word}' sözündə qoşa hərf var.`);
  }
}
// Nəticə:
// 'book' sözündə qoşa hərf var.
// 'apple' sözündə qoşa hərf var.
// 'coffee' sözündə qoşa hərf var.
```

---

#### Digər `RegExp` Xüsusiyyətləri (Properties)

Hər `RegExp` obyektinin, onun haqqında məlumat verən, yalnız oxuna bilən (read-only) xüsusiyyətləri var:
* **`.source`**: Requlyar ifadənin `/.../` arasındakı mətnini qaytarır.
* **`.flags`**: Fleqləri bir sətir (string) olaraq qaytarır.
* **`.global`**, **`.ignoreCase`**, **`.multiline`** və s.: Müvafiq fleqin təyin edilib-edilmədiyini göstərən `true`/`false` dəyərlər.

```javascript
const myPattern = /\w+/gi;

console.log("Mənbə (source):", myPattern.source);     // ✅ Nəticə: \w+
console.log("Fleqlər (flags):", myPattern.flags);       // ✅ Nəticə: gi
console.log("Qlobaldırmı? (global):", myPattern.global); // ✅ Nəticə: true
console.log("Hərfə həssasdırmı? (ignoreCase):", myPattern.ignoreCase); // ✅ Nəticə: true
```
---
### 11.4 Tarix (Date) və Zaman (Time) 📅

JavaScript-də tarix və zamanla işləmək üçün `Date` sinifi (class) istifadə olunur. `Date` obyekti `new Date()` konstruktoru (constructor) ilə yaradılır.

**Obyekt Yaratma Nümunələri:**

**1. Arqumentsiz - Hazırkı vaxt** 🕰️
```javascript
const now = new Date();
console.log("Hazırkı vaxt:", now.toString());
// ✅ Nəticə (təxmini): Fri Jun 13 2025 19:26:51 GMT+0400 (Azerbaijan Standard Time)
```

**2. Rəqəmlərlə - Detallı tarix**
Konstruktora 2 və ya daha çox rəqəm verdikdə, onlar yerli (local) vaxt qurşağına görə İL, AY, GÜN, SAAT... kimi qəbul edilir.
```javascript
// ❗️ Diqqət: Aylar 0-dan başlayır (0 = Yanvar, 5 = İyun)!
// Günlər isə 1-dən başlayır.

// 13 İyun 2025, saat 19:30 (Bakı vaxtı ilə)
const someDay = new Date(2025, 5, 13, 19, 30, 0); 
console.log("Müəyyən edilmiş tarix:", someDay.toLocaleString('az-AZ'));
// ✅ Nəticə: 13.06.2025, 19:30:00
```

**3. Sətir (string) ilə**
`new Date()` standart formatda olan sətirləri (strings) də emal edə bilir. Ən çox istifadə olunan format ISO 8601 formatıdır.
```javascript
// 'Z' hərfi vaxtın UTC olduğunu bildirir
const isoDate = new Date("2025-01-20T10:00:00Z");
console.log("ISO formatlı tarix (Bakı vaxtı ilə):", isoDate.toLocaleString('az-AZ', { timeZone: 'Asia/Baku' }));
// ✅ Nəticə: 20.1.2025, 14:00:00
```
---
### 11.4.1 Zaman Damğası (Timestamp) ⏱️

Hər bir `Date` obyekti arxa planda sadəcə bir rəqəm saxlayır: 1 Yanvar 1970-ci il saat 00:00:00-dan (UTC vaxtı ilə) keçən millisaniyələrin sayı. Buna **zaman damğası (timestamp)** deyilir.

* `.getTime()`: Obyektin zaman damğasını (timestamp) qaytarır.
* `.setTime()`: Obyektin zaman damğasını (timestamp) dəyişir.
* `Date.now()`: Hal-hazırkı zaman damğasını (timestamp) birbaşa qaytarır.

**Nümunə: Kodun işləmə müddətini ölçmək** 🚀
```javascript
console.log("Əməliyyat başlayır...");
const startTime = Date.now();

// Bir qədər vaxt aparan bir əməliyyat simulyasiyası
for (let i = 0; i < 100000000; i++) { /* Boş dövr */ }

const endTime = Date.now();
const duration = endTime - startTime;

console.log(`✅ Əməliyyat ${duration} ms (millisaniyə) çəkdi.`);
```
---
### 11.4.2 Tarix üzərində Hesablamalar (Date Arithmetic) 셈️

`Date` obyektləri ilə riyazi əməliyyatlar aparmaq mümkündür.

**Tarix Hissələrini Almaq və Dəyişmək (Getters & Setters)**
Hesablama aparmaq üçün əvvəlcə tarixin hissələrini almalı (`get`) və ya dəyişməliyik (`set`).
* `getFullYear()` / `setFullYear()`
* `getMonth()` / `setMonth()`
* `getDate()` / `setDate()` (❗️ Ayın gününü verir)
* `getDay()` (❗️ Həftənin gününü verir: 0 = Bazar, 1 = Bazar ertəsi...)

**Nümunə 1: İki tarix arasındakı fərqi tapmaq**
```javascript
const newYear = new Date("2026-01-01");
const today = new Date("2025-06-13");

const diffMilliseconds = newYear - today;
// Fərqi günə çeviririk (1000ms * 60s * 60m * 24h)
const diffDays = Math.floor(diffMilliseconds / (1000 * 60 * 60 * 24));

console.log(`💡 Yeni ilə ${diffDays} gün qalıb.`); // ✅ Nəticə: Yeni ilə 202 gün qalıb.
```

**Nümunə 2: Tarixə vaxt əlavə etmək (overflow nümunəsi)**
`set` metodları "daşma" (overflow) problemini avtomatik həll edir. Məsələn, aya 12-dən böyük dəyər verdikdə, il avtomatik artır.
```javascript
const d = new Date(2025, 10, 20); // 20 Noyabr 2025
console.log("Başlanğıc tarixi:", d.toLocaleDateString('az-AZ'));

// 20 Noyabrın üstünə 2 ay gəlirik. Nəticədə 20 Yanvar 2026 alınır.
d.setMonth(d.getMonth() + 2); 

console.log("Nəticə:", d.toLocaleDateString('az-AZ')); // ✅ Nəticə: "20.01.2026"
```
---
### 11.4.3 Tarixin Formatlanması və Emalı (Formatting and Parsing) 📜

`Date` obyektini istifadəçiyə göstərmək üçün onu sətirə (string) çevirmək lazımdır.

**Nümunə: Müxtəlif formatlama metodları**
```javascript
const d = new Date("2025-10-26T20:30:00");

console.log("toString():", d.toString());
console.log("toUTCString():", d.toUTCString());
console.log("toISOString():", d.toISOString()); // ❗️ API-lar üçün standart format
console.log("toLocaleDateString('az-AZ'):", d.toLocaleDateString('az-AZ')); // ✅ "26.10.2025"
console.log("toLocaleTimeString('az-AZ'):", d.toLocaleTimeString('az-AZ')); // ✅ "20:30:00"
console.log("toDateString():", d.toDateString());
```

**`Date.parse()`:** Sətir (string) formatında olan tarixi analiz edərək onun zaman damğasını (timestamp) qaytarır. Bu, formatlamanın əksidir.
```javascript
const dateString = "2025-01-01T00:00:00Z";
const timestamp = Date.parse(dateString);

console.log("Alınan timestamp:", timestamp); // ✅ Nəticə: 1735689600000

// Həmin timestamp-dən yeni Date obyekti yaratmaq
const newDate = new Date(timestamp);
console.log(newDate.toUTCString()); // ✅ Nəticə: "Wed, 01 Jan 2025 00:00:00 GMT"
```


***
### 11.5 Xəta Sinifləri (Error Classes)
JavaScript-də `throw` və `catch` istənilən dəyəri xəta kimi istifadə edə bilsə də, ən yaxşı təcrübə (best practice) `Error` sinifindən (class) və ya onun alt-siniflərindən (subclasses) istifadə etməkdir.

**Niyə `Error` obyekti istifadə etməliyik?** 🤔
Çünki `Error` obyekti yaradıldıqda, o, kodun hansı ardıcıllıqla çağırıldığını göstərən **yaddaş stekini (stack trace)** özündə saxlayır. Bu, xətanın mənbəyini tapmaq (debug) üçün çox vacibdir.

`Error` obyektinin 3 əsas xüsusiyyəti (property) var:
* **`.name`**: Xətanın növü (məsələn, "Error", "TypeError").
* **`.message`**: Konstruktora (constructor) ötürülən xəta mesajı.
* **`.stack`**: Xətanın baş verdiyi yerə qədər olan bütün funksiya çağırışlarını göstərən uzun sətir (string).

**Nümunə: `try...catch` bloku ilə xətanı tutmaq**
```javascript
function checkAge(age) {
  if (typeof age !== 'number' || age < 18) {
    // Xəta yaradırıq və onu "throw" edirik
    throw new Error("İstifadəçi 18 yaşından kiçikdir və ya yaşı rəqəm deyil.");
  }
  return "İstifadəçi qeydiyyatdan keçə bilər.";
}

try {
  let message = checkAge("iyirmi"); // Səhv tipdə dəyər ötürürük
  console.log(message);
} catch (error) {
  console.log("❗️ Bir xəta baş verdi!");
  console.log("Xətanın adı (name):", error.name);
  console.log("Xətanın mesajı (message):", error.message);
  console.log("-------------------");
  console.log("Stek izi (stack trace):", error.stack);
}
```
**JavaScript-in Standart Xəta Növləri**
JavaScript-in özünün də standart xətalar üçün alt-sinifləri var:
* **`TypeError`**: Dəyər üzərində səhv tipdə əməliyyat aparıldıqda. Məsələn, `(10).toUpperCase()`.
* **`ReferenceError`**: Mövcud olmayan bir dəyişənə müraciət edildikdə. Məsələn, `console.log(x)`.
* **`SyntaxError`**: Kodun sintaksisi səhv olduqda. Məsələn, `let x = ;`.
* `RangeError`, `URIError`, `EvalError`...

**Fərdi Xəta Sinifləri Yaratmaq (Custom Error Classes)**
Bəzən proqramın məntiqinə uyğun xüsusi xəta sinifləri yaratmaq daha faydalı olur. Məsələn, şəbəkə (network) sorğuları üçün `HTTPError` yaradaq.

**Nümunə: `HTTPError` sinifi**
```javascript
// Error sinifini genişləndirərək öz xəta sinifimizi yaradırıq
class HTTPError extends Error {
  constructor(status, statusText, url) {
    // `super()` ilə ana Error sinifinin konstruktoruna mesaj göndəririk
    super(`${status} ${statusText}: ${url}`);
    
    // Öz xüsusiyyətlərimizi əlavə edirik
    this.name = "HTTPError";
    this.status = status;
    this.statusText = statusText;
    this.url = url;
  }
}

function fetchSomeData(url) {
  // Tutaq ki, sorğu uğursuz oldu və 404 statusu qayıtdı
  throw new HTTPError(404, "Not Found", url);
}

try {
  fetchSomeData("http://example.com/non-existent-page");
} catch (error) {
  console.error("Xəta baş verdi:", error.message);
  
  // Xətanın bizim yaratdığımız HTTPError olub-olmadığını yoxlayırıq
  if (error instanceof HTTPError) {
    console.error(`HTTP Status: ${error.status}`);
    console.error(`Sorğu edilən URL: ${error.url}`);
  }
}
```
---
### 11.6 JSON Seriləşdirmə (Serialization) və Emal (Parsing) 🔄
**Seriləşdirmə (Serialization)** – proqramdakı obyekt, massiv (array) kimi data strukturlarını saxlamaq və ya şəbəkə (network) üzərindən ötürmək üçün sətirə (string) çevirmə prosesidir.

JavaScript-də bu proses üçün **JSON (JavaScript Object Notation)** formatı və iki əsas funksiya istifadə olunur:
* `JSON.stringify()`: Obyekti JSON formatlı sətirə (string) çevirir.
* `JSON.parse()`: JSON formatlı sətri (string) yenidən obyektə çevirir.

JSON əksər primitiv tipləri (string, number, boolean, null), massivləri (arrays) və obyektləri (objects) dəstəkləyir, ancaq `Date`, `Map`, `Set`, `RegExp` kimi mürəkkəb tipləri birbaşa dəstəkləmir.

**Nümunə 1: Əsas istifadə və formatlı çıxış**
```javascript
const user = {
  id: 101,
  username: "johndoe",
  isAdmin: false,
  roles: ["editor", "viewer"],
  profile: null
};

// 1. Obyekti JSON sətirinə çeviririk
const jsonString = JSON.stringify(user);
console.log("Normal JSON:", jsonString);
// ✅ Nəticə: {"id":101,"username":"johndoe","isAdmin":false,"roles":["editor","viewer"],"profile":null}

// 2. İnsanların oxuması üçün formatlı (pretty-print) JSON yaradırıq
// 3-cü arqument boşluqların sayını bildirir
const prettyJsonString = JSON.stringify(user, null, 2);
console.log("Formatlı JSON:\n", prettyJsonString);
/* ✅ Nəticə:
{
  "id": 101,
  "username": "johndoe",
  "isAdmin": false,
  "roles": [
    "editor",
    "viewer"
  ],
  "profile": null
}
*/

// 3. Sətri yenidən obyektə çeviririk
const userCopy = JSON.parse(jsonString);
console.log("Kopyalanmış obyekt:", userCopy);
```

---
### 11.6.1 Fərdi Dəyişikliklər (JSON Customizations) 🛠️
`JSON.stringify` və `JSON.parse` metodlarının davranışını xüsusi ehtiyaclara görə dəyişmək mümkündür.

**`toJSON()` Metodu**
Əgər bir obyektin `.toJSON()` adlı metodu varsa, `JSON.stringify` həmin obyekti deyil, bu metodun qaytardığı dəyəri sətirə çevirəcək. `Date` obyektləri bu metoddan istifadə edərək özlərini ISO formatlı sətirə çevirir.

**`JSON.parse()` ilə "Canlandırıcı" (Reviver) Funksiyası**
`JSON.parse` metodunun ikinci arqumenti **reviver** funksiyasıdır. Bu funksiya emal edilən hər bir `key:value` cütü üçün çağırılır və dəyəri çevirməyə (transform) imkan verir. Bu, `Date` sətirlərini yenidən `Date` obyektinə çevirmək üçün idealdır.

**Geniş Nümunə: `Date` obyektini seriləşdirib yenidən canlandırmaq**
```javascript
const event = {
  title: "JavaScript Konfransı",
  date: new Date() // Hazırkı vaxt
};

// 1. stringify edirik. `date` xüsusiyyəti toJSON() metodu sayəsində ISO sətirinə çevriləcək.
const eventString = JSON.stringify(event);
console.log("Seriləşdirilmiş:", eventString); 
// ✅ Nəticə: {"title":"JavaScript Konfransı","date":"2025-06-13T15:28:51.520Z"}

// 2. parse edərkən reviver funksiyası ilə `date`-i yenidən Date obyektinə çeviririk
const revivedEvent = JSON.parse(eventString, (key, value) => {
  // Əgər dəyər tarix formatlı bir sətirdirsə...
  if (key === 'date' && typeof value === 'string') {
    return new Date(value); // ...onu Date obyektinə çevir
  }
  return value; // Əks halda dəyəri olduğu kimi saxla
});

console.log("Canlandırılmış:", revivedEvent);
console.log("Tarixin tipi:", revivedEvent.date instanceof Date); // ✅ Nəticə: true
```

**`JSON.stringify()` ilə "Əvəzləyici" (Replacer) Funksiyası**
`JSON.stringify`-ın ikinci arqumenti həm də **replacer** funksiyası və ya massiv (array) ola bilər.
* **Massiv (Array):** Yalnız massivdə adları olan xüsusiyyətləri seriləşdirir.
* **Funksiya (Function):** Hər bir dəyəri seriləşdirmədən əvvəl dəyişməyə imkan verir. `undefined` qaytardıqda, həmin xüsusiyyət nəticəyə daxil edilmir.

**Nümunə: `replacer` ilə seriləşdirməni idarə etmək**
```javascript
const product = {
  id: 123,
  name: "Laptop",
  price: 2500,
  internalCode: "XYZ-987-A", // Bu xüsusiyyəti istifadəçiyə göstərmək istəmirik
  specs: { cpu: 'i7', ram: 16 }
};

// 1. Yalnız müəyyən xüsusiyyətləri seçmək (massiv ilə)
const publicData = JSON.stringify(product, ["name", "price", "specs"], 2);
console.log("İctimai data:\n", publicData);
/* ✅ Nəticə:
{
  "name": "Laptop",
  "price": 2500,
  "specs": {
    "cpu": "i7",
    "ram": 16
  }
}
*/

// 2. Müəyyən xüsusiyyətləri gizlətmək (funksiya ilə)
const privateData = JSON.stringify(product, (key, value) => {
  if (key === "internalCode") {
    return undefined; // Bu xüsusiyyəti nəticədən çıxar
  }
  return value;
}, 2);
console.log("Daxili kodu gizlədilmiş data:\n", privateData);
```
***
### 11.7 Beynəlxalqlaşdırma API-ı (The Internationalization API) 🌍
JavaScript-in beynəlxalqlaşdırma (internationalization) API-ı, proqramları fərqli dillər, regionlar və mədəniyyətlər üçün uyğunlaşdırmağa kömək edən bir alətlər toplusudur. Bu API 3 əsas sinifdən (class) ibarətdir:
* **`Intl.NumberFormat`**: Rəqəmləri, valyutaları və faizləri formatlamaq üçün.
* **`Intl.DateTimeFormat`**: Tarix və zamanı formatlamaq üçün.
* **`Intl.Collator`**: Sətirləri (strings) yerli əlifba qaydalarına görə müqayisə etmək və sıralamaq (sort) üçün.

Bu fəsildə bu siniflərin hər birini ayrı-ayrılıqda araşdıracağıq.

---
### 11.7.1 Rəqəmlərin Formatlanması (Formatting Numbers) 💵
Fərqli ölkələrdə insanlar rəqəmləri fərqli yazır. Məsələn, bəziləri kəsr hissəni ayırmaq üçün nöqtə (`.`), bəziləri isə vergül (`,`) istifadə edir. Minlik ayırıcıları da fərqli ola bilir. `Intl.NumberFormat` sinifi bu məsələni bizim üçün həll edir.

Onu `new Intl.NumberFormat(locale, options)` konstruktoru ilə yaradırıq:
* **`locale`**: Formatlamanın hansı dil və region üçün ediləcəyini göstərən sətir (string). Məsələn, `"az-AZ"` (Azərbaycan), `"en-US"` (ABŞ İngiliscəsi), `"fr-FR"` (Fransa Fransızcası). Boş buraxılarsa, sistemin lokalını götürür.
* **`options`**: Formatlamanın detallarını bildirən bir obyekt (object).

**Nümunə 1: Sadə rəqəm formatlanması (fərqli lokallar ilə)**
Görək eyni rəqəm fərqli ölkələrdə necə görünür.
```javascript
const number = 1234567.89;

// Azərbaycan lokalı (az-AZ)
const azFormatter = new Intl.NumberFormat('az-AZ');
console.log("Azərbaycan formatı:", azFormatter.format(number));
// ✅ Nəticə: 1 234 567,89

// ABŞ lokalı (en-US)
const usFormatter = new Intl.NumberFormat('en-US');
console.log("ABŞ formatı:", usFormatter.format(number));
// ✅ Nəticə: 1,234,567.89

// Almaniya lokalı (de-DE)
const deFormatter = new Intl.NumberFormat('de-DE');
console.log("Almaniya formatı:", deFormatter.format(number));
// ✅ Nəticə: 1.234.567,89
```

**Nümunə 2: Valyuta formatlanması (currency)** 💰
Bu, ən çox istifadə olunan xüsusiyyətlərdən biridir. `style: 'currency'` və `currency: 'KOD'` opsiyalarını tələb edir.
```javascript
const amount = 1550.75;

// Azərbaycan Manatı (AZN)
const aznFormatter = new Intl.NumberFormat('az-AZ', {
  style: 'currency',
  currency: 'AZN'
});
console.log("Manatla:", aznFormatter.format(amount)); // ✅ Nəticə: 1.550,75 ₼

// ABŞ Dolları (USD)
const usdFormatter = new Intl.NumberFormat('en-US', {
  style: 'currency',
  currency: 'USD'
});
console.log("Dollarla:", usdFormatter.format(amount)); // ✅ Nəticə: $1,550.75

// Avro (EUR) - valyutanın adını tam göstərmək
const eurFormatter = new Intl.NumberFormat('de-DE', {
  style: 'currency',
  currency: 'EUR',
  currencyDisplay: 'name' // 'symbol' (default), 'code', 'name' ola bilər
});
console.log("Avro ilə:", eurFormatter.format(amount)); // ✅ Nəticə: 1.550,75 Euro
```

**Nümunə 3: Faiz formatlanması (percent)**
```javascript
const value = 0.895;

const percentFormatter = new Intl.NumberFormat('az-AZ', {
  style: 'percent',
  minimumFractionDigits: 1 // Kəsr hissədə minimum 1 rəqəm göstər
});

console.log(percentFormatter.format(value)); // ✅ Nəticə: 89,5 %
```

**Nümunə 4: Rəqəm hissələrinin idarə edilməsi**
Bəzən qiymətləri göstərərkən hər zaman iki kəsr rəqəmi olmasını istəyirik (məsələn, 15.00).
```javascript
const price = 1299;
const precisePrice = 49.956;
const simplePrice = 78.5;

const priceFormatter = new Intl.NumberFormat('az-AZ', {
  style: 'currency',
  currency: 'AZN',
  minimumFractionDigits: 2, // Həmişə minimum 2 kəsr rəqəmi göstər
  maximumFractionDigits: 2  // Maksimum 2 kəsr rəqəmi göstər (yuvarlaqlaşdırır)
});

console.log(priceFormatter.format(price));        // ✅ Nəticə: 1.299,00 ₼
console.log(priceFormatter.format(precisePrice)); // ✅ Nəticə: 49,96 ₼ (yuvarlaqlaşdırdı)
console.log(priceFormatter.format(simplePrice));  // ✅ Nəticə: 78,50 ₼ (sonuna sıfır əlavə etdi)
```
**Nümunə 5: Format metodunu birbaşa istifadə etmək**
Formatter obyektinin `.format` metodunu birbaşa bir dəyişənə mənimsədib, onu bir funksiya kimi istifadə edə bilərsiniz.
```javascript
const data = [100, 2500, 98765];

// Formatter yaradıb, onun format metodunu birbaşa bir dəyişənə veririk
const formatWithGrouping = new Intl.NumberFormat('az-AZ').format;

const formattedData = data.map(formatWithGrouping);
console.log(formattedData); // ✅ Nəticə: ["100", "2.500", "98.765"]
```


---
### 11.7.2 Tarix və Zamanın Formatlanması (Formatting Dates and Times) 🗓️

`Date` obyektinin `.toLocaleDateString()` kimi sadə metodları olsa da, onlar formatlama üzərində tam nəzarət imkanı vermir. Məsələn, bəlkə də ilin göstərilməsini istəmir, amma həftənin gününün əlavə olunmasını istəyirsiniz? Ayın adı tam yazılsın, yoxsa qısaldılmış?

Bütün bu detallı nəzarəti **`Intl.DateTimeFormat`** sinifi təmin edir. O da `Intl.NumberFormat` kimi `new Intl.DateTimeFormat(locale, options)` konstruktoru ilə yaradılır və `.format()` metodu ilə istifadə olunur.

* **`locale`**: Formatlamanın hansı dil və region üçün ediləcəyini göstərir (məsələn, `"az-AZ"`).
* **`options`**: Tarix və zamanın hansı hissələrinin və necə göstəriləcəyini təyin edən bir obyekt (object).

**Əsas `options` Xüsusiyyətləri:**
* **`year`, `month`, `day`, `weekday`**: Tarix hissələri. Dəyərləri `"numeric"`, `"2-digit"`, `"long"` (uzun, məs. "İyun"), `"short"` (qısa, məs. "İyn") ola bilər.
* **`hour`, `minute`, `second`**: Zaman hissələri. Dəyərləri `"numeric"`, `"2-digit"` ola bilər.
* **`timeZone`**: Tarixi fərqli bir zaman qurşağı (timezone) üçün göstərməyə imkan verir (məsələn, `"America/New_York"`).
* **`hour12`**: `true` və ya `false` dəyəri ilə 12 saatlıq və ya 24 saatlıq formatı seçir.

---
**Geniş Nümunələr**

**Nümunə 1: Sadə və detallı formatların müqayisəsi**
Hazırkı vaxtı (`new Date()`) götürərək onu fərqli detallarla formatlayaq.
```javascript
const today = new Date(); // Hazırkı vaxt: 13 İyun 2025, 19:33

// 1. Standart, qısa format (Azərbaycan lokalı ilə)
const simpleFormatter = new Intl.DateTimeFormat('az-AZ');
console.log("Sadə format:", simpleFormatter.format(today));
// ✅ Nəticə: 13.06.2025

// 2. Bütün detalları özündə cəmləyən geniş format
const fullOptions = {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric',
  hour: '2-digit',
  minute: '2-digit',
  second: '2-digit'
};
const fullFormatter = new Intl.DateTimeFormat('az-AZ', fullOptions);
console.log("Geniş format:", fullFormatter.format(today));
// ✅ Nəticə: Cümə, 13 iyun 2025, 19:33:03
```

**Nümunə 2: Eyni formatın fərqli lokallarda (locales) göstərilməsi**
Yuxarıdakı `fullOptions` obyektini fərqli dillər üçün istifadə edək.
```javascript
const date = new Date('2025-06-13T19:30:00');
const options = { weekday: 'long', month: 'long', day: 'numeric', year: 'numeric' };

const az_format = new Intl.DateTimeFormat('az-AZ', options).format(date);
const en_format = new Intl.DateTimeFormat('en-US', options).format(date);
const ja_format = new Intl.DateTimeFormat('ja-JP', options).format(date);

console.log("🇦🇿 Azərbaycan:", az_format); // ✅ Nəticə: Cümə, 13 iyun 2025
console.log("🇺🇸 ABŞ:", en_format);       // ✅ Nəticə: Friday, June 13, 2025
console.log("🇯🇵 Yaponiya:", ja_format);   // ✅ Nəticə: 2025年6月13日金曜日
```
Gördüyünüz kimi, eyni qaydalar hər dilin öz qrammatikasına uyğun tətbiq olunur.

**Nümunə 3: Zaman qurşağının (Timezone) dəyişdirilməsi** ✈️
Hazırda Bakıda saatın neçə olduğunu bilirik. Görəsən eyni anda Nyu-Yorkda saat neçədir?
```javascript
const now = new Date(); // Hazırkı vaxt

const timeOptions = {
  hour: '2-digit',
  minute: '2-digit',
  second: '2-digit',
  timeZoneName: 'short'
};

// Bakı vaxtı
const bakuTime = new Intl.DateTimeFormat('az-AZ', { ...timeOptions, timeZone: 'Asia/Baku' });
console.log("Bakı vaxtı:", bakuTime.format(now));
// ✅ Nəticə: 19:33:03 GMT+4

// Nyu-York vaxtı
const newYorkTime = new Intl.DateTimeFormat('en-US', { ...timeOptions, timeZone: 'America/New_York' });
console.log("Nyu-York vaxtı:", newYorkTime.format(now));
// ✅ Nəticə: 11:33:03 AM EDT
```

**Nümunə 4: Müxtəlif format stilləri**
Fərqli məqsədlər üçün fərqli formatlar yarada bilərik.
```javascript
const eventDate = new Date('2025-12-25T20:00:00');

// Stil 1: Yalnız ay və gün
const monthDay = new Intl.DateTimeFormat('az-AZ', { month: 'long', day: 'numeric' }).format(eventDate);
console.log("🗓️ Tədbir tarixi:", monthDay); // ✅ Nəticə: 25 dekabr

// Stil 2: Qısa tarix və 12-saatlıq zaman formatı
const shortWithAmPm = new Intl.DateTimeFormat('en-US', {
  year: '2-digit',
  month: 'numeric',
  day: 'numeric',
  hour: 'numeric',
  minute: 'numeric',
  hour12: true
}).format(eventDate);
console.log("⏰ ABŞ formatı:", shortWithAmPm); // ✅ Nəticə: 12/25/25, 8:00 PM
```
**Nümunə 5 (Bonus): Fərqli təqvimlər**
Tarixi başqa təqvim sistemlərinə görə də formatlamaq olar.
```javascript
const d = new Date("2025-06-13");
const opts = { year: 'numeric', era: 'short' };

console.log("İslami Təqvim:", new Intl.DateTimeFormat('ar-SA-u-ca-islamic', opts).format(d));
// ✅ Nəticə: ١٤٤٦ هـ
console.log("İbrani Təqvimi:", new Intl.DateTimeFormat('he-IL-u-ca-hebrew', opts).format(d));
// ✅ Nəticə: ה'תשפ"ה
```

---

### 11.7.3 Sətirlərin Müqayisəsi və Sıralanması (Comparing and Sorting Strings) 🔤
Sətirləri (strings) əlifba sırasına görə sıralamaq (sort) düşündüyümüzdən daha mürəkkəb bir məsələdir. Məsələn, JavaScript-in standart `.sort()` metodu simvolları onların Unicode kodlarına görə sıralayır. Bu, bir çox dil üçün, o cümlədən Azərbaycan dili üçün düzgün nəticə vermir.

**Problem Nədir?** 🤔
Standart `.sort()` metodu Azərbaycan dilinin xüsusi hərflərini (`ç`, `ş`, `ğ`, `ə`, `ö`, `ü`) və onların əlifbadakı yerini tanımır.

```javascript
const words = ['şüşə', 'səbət', 'çanta', 'can'];

// Standart sort metodu səhv nəticə verir
words.sort();
console.log("Səhv sıralama:", words);
// ❌ Nəticə (təxmini): ["can", "səbət", "çanta", "şüşə"] (çünki 's' hərfi 'ç'-dən əvvəl gəlir)
```

**Həll Yolu: `Intl.Collator`** ✅
`Intl.Collator` sinifi (class) sətirləri müxtəlif dillərin və regionların qaydalarına uyğun olaraq müqayisə etmək və sıralamaq üçün yaradılıb. O, `.compare` adlı bir metod təqdim edir ki, bu metodu birbaşa `.sort()` metoduna ötürmək olar.

`new Intl.Collator(locale, options)` konstruktoru ilə yaradılır:
* **`locale`**: Müqayisənin hansı dilin qaydalarına görə aparılacağını bildirir (`"az-AZ"`).
* **`options`**: Müqayisənin davranışını tənzimləyən bir obyekt (object).

**Əsas `options` Xüsusiyyətləri:**
* **`usage`**: İstifadə məqsədi. `"sort"` (sıralama) və ya `"search"` (axtarış) ola bilər.
* **`sensitivity`**: Həssaslıq dərəcəsi.
    * `"base"`: Həm hərflərin böyük-kiçikliyinə, həm də aksentlərə məhəl qoymur (məsələn, `e` == `ə` == `E`).
    * `"accent"`: Aksentləri nəzərə alır, amma böyük-kiçikliyi yox (`e` != `ə`, amma `ə` == `Ə`).
    * `"case"`: Böyük-kiçikliyi nəzərə alır, amma aksentləri yox (`e` != `E`, amma `e` == `ə`).
    * `"variant"`: Hər şeyi nəzərə alır (tam həssaslıq). Bu, `"sort"` üçün standart dəyərdir.
* **`numeric`**: `true` olarsa, sətirlərin içindəki rəqəmləri ədədi dəyərlərinə görə sıralayır (məsələn, "Fəsil 2" "Fəsil 10"-dan əvvəl gəlir).
* **`ignorePunctuation`**: `true` olarsa, durğu işarələri və boşluqlar nəzərə alınmır.

---
**Geniş Nümunələr**

**Nümunə 1: Azərbaycan dili üçün düzgün əlifba sırası**
Yuxarıdakı səhv sıralama problemini `Intl.Collator` ilə həll edək.
```javascript
const words = ['şüşə', 'səbət', 'çanta', 'can'];

// Azərbaycan lokalı üçün bir müqayisəçi (collator) yaradırıq
const azCollator = new Intl.Collator('az-AZ').compare;

// .sort() metoduna öz müqayisəçimizi ötürürük
words.sort(azCollator);

console.log("Düzgün sıralama:", words);
// ✅ Nəticə: ["can", "çanta", "səbət", "şüşə"]
```

**Nümunə 2: Rəqəmlərin nəzərə alınması (Numeric Sorting)** 🔢
Fayl adları və ya başlıqlar kimi rəqəmli sətirləri sıralamaq üçün `numeric: true` əvəzsizdir.
```javascript
const files = ['report-10.pdf', 'report-2.pdf', 'report-1.pdf'];

// Standart sort səhv nəticə verəcək: ["report-1.pdf", "report-10.pdf", "report-2.pdf"]
files.sort();
console.log("Səhv rəqəm sıralaması:", files);

// Düzgün sıralama üçün `numeric: true` istifadə edirik
const numericCollator = new Intl.Collator(undefined, { numeric: true }).compare;
files.sort(numericCollator);
console.log("Düzgün rəqəm sıralaması:", files);
// ✅ Nəticə: ["report-1.pdf", "report-2.pdf", "report-10.pdf"]
```

**Nümunə 3: Həssaslığın (Sensitivity) idarə edilməsi**
Böyük-kiçik hərf və ya aksent fərqlərini nəzərə alıb-almamağı seçə bilərik.
```javascript
const names = ['Emin', 'əmin', 'emin', 'Əmin'];

// `sensitivity: 'base'` - nə böyük-kiçikliyə, nə də aksentə fikir verir
const baseCollator = new Intl.Collator('az-AZ', { sensitivity: 'base' }).compare;
names.sort(baseCollator);
console.log("Əsas hərfə görə sıralama:", names);
// ✅ Nəticə (təxmini): ["Emin", "emin", "əmin", "Əmin"] (eyni baza hərflər qruplaşdırılır)

// `sensitivity: 'variant'` (standart) - hər şeyi nəzərə alır
const variantCollator = new Intl.Collator('az-AZ', { sensitivity: 'variant' }).compare;
names.sort(variantCollator);
console.log("Tam həssas sıralama:", names);
// ✅ Nəticə: ["Emin", "emin", "Əmin", "əmin"] (düzgün əlifba sırası)
```

**Nümunə 4: "Yumşaq" axtarış (Fuzzy Search)** 🔍
`usage: 'search'` və `sensitivity: 'base'` ilə aksent və böyük-kiçik hərf fərqi olmadan axtarış etmək olar.
```javascript
const cityList = ['Gəncə', 'Şəki', 'Gence', 'gəncə'];
const searchTerm = 'gence';

const fuzzyMatcher = new Intl.Collator('az-AZ', { 
  usage: 'search', 
  sensitivity: 'base' 
}).compare;

// `searchTerm`-ə bərabər olan ilk elementi tapırıq
const found = cityList.find(city => fuzzyMatcher(city, searchTerm) === 0);

console.log(`'${searchTerm}' üçün tapılan ilk uyğunluq: ${found}`);
// ✅ Nəticə: 'gence' üçün tapılan ilk uyğunluq: Gəncə
```

---

### 11.8 Konsol (Console) API-ı 💻
`console` obyekti, JavaScript kodunu yazarkən və sazlayarkən (debug) ən yaxın dostumuzdur. O, sadəcə `console.log()`-dan ibarət deyil və bir çox faydalı metoda malikdir. Bu API standart olmasa da, bütün müasir brauzerlər və Node.js tərəfindən tam dəstəklənir.

**Log Səviyyələri (Log Levels): `log`, `info`, `warn`, `error`**
Bu metodlar mesajları fərqli vaciblik səviyyələrinə görə konsola çıxarmaq üçündür. Brauzerlər adətən hər birini fərqli ikon və rənglə göstərir.
```javascript
console.log("Bu, sadə bir log mesajıdır.");
console.info("💡 Bu, informativ bir mesajdır."); // Adətən yanında (i) ikonu olur.
console.warn("⚠️ Bu, bir xəbərdarlıqdır."); // Adətən sarı rəngdə və xəbərdarlıq ikonu ilə.
console.error("❌ Bu isə bir xətadır.");    // Adətən qırmızı rəngdə və xəta ikonu ilə.
```

**Yoxlama (Assertion): `assert()`**
Birinci arqumenti `false` olarsa, qalan arqumentləri bir xəta mesajı kimi konsola çıxarır. Bu, kodun müəyyən bir şərtə cavab verib-vermədiyini yoxlamaq üçün faydalıdır.
```javascript
const x = 5;
const y = 10;

// Şərt doğru olduğu üçün heç nə çıxmır.
console.assert(x < y, "X y-dən kiçik olmalıdır.");

// Şərt səhv olduğu üçün xəta mesajı çıxır.
console.assert(x > y, "X y-dən böyük deyil!", { x_dəyəri: x, y_dəyəri: y });
// ❌ Nəticə: Assertion failed: X y-dən böyük deyil! {x_dəyəri: 5, y_dəyəri: 10}
```

**Cədvəl Formatı (Table Formatting): `table()`** 📊
Massivləri (arrays of objects) və ya obyektləri oxunaqlı cədvəl formatında göstərmək üçün super bir vasitədir.
```javascript
const users = [
  { ad: "Elvin", soyad: "Əliyev", yas: 28 },
  { ad: "Ayan", soyad: "Məmmədova", yas: 25 },
  { ad: "Tural", soyad: "Hüseynov", yas: 32 }
];

console.log("İstifadəçilər cədvəl formatında:");
console.table(users);

console.log("Yalnız 'ad' və 'yas' sütunları ilə cədvəl:");
console.table(users, ["ad", "yas"]);
```

**Qruplaşdırma (Grouping): `group()`, `groupEnd()`**
Əlaqəli konsol mesajlarını bir başlıq altında, iç-içə və səliqəli şəkildə qruplaşdırmağa imkan verir.
```javascript
console.group("İstifadəçi Məlumatları");
  console.log("Ad: Elvin Əliyev");
  console.log("Yaş: 28");
  console.group("📍 Ünvan"); // İç-içə qrup
    console.log("Şəhər: Bakı");
    console.log("Küçə: Nizami");
  console.groupEnd(); // Daxili qrupu bağlayır
console.groupEnd(); // Əsas qrupu bağlayır
```

**Sayğac (Counter): `count()`, `countReset()`**
Müəyyən bir hadisənin neçə dəfə baş verdiyini izləmək üçün idealdır.
```javascript
for (let i = 0; i < 5; i++) {
  if (i % 2 === 0) {
    console.count("Cüt rəqəm");
  } else {
    console.count("Tək rəqəm");
  }
}
// Nəticə:
// Cüt rəqəm: 1
// Tək rəqəm: 1
// Cüt rəqəm: 2
// Tək rəqəm: 2
// Cüt rəqəm: 3

console.countReset("Cüt rəqəm"); // "Cüt rəqəm" sayğacını sıfırlayır
console.count("Cüt rəqəm"); // ✅ Nəticə: Cüt rəqəm: 1
```

**Zamanlayıcı (Timer): `time()`, `timeLog()`, `timeEnd()`** ⏱️
Müəyyən bir əməliyyatın nə qədər vaxt apardığını ölçmək üçün `Date.now()`-dan daha rahat bir üsuldur.
```javascript
// "Məlumat yükləmə" adlı zamanlayıcını başladırıq
console.time("Məlumat yükləmə");

// Müəyyən bir müddət sonra...
setTimeout(() => {
  console.timeLog("Məlumat yükləmə", "İlkin datalar yükləndi.");
}, 500);

// Daha bir müddət sonra...
setTimeout(() => {
  // Zamanlayıcını dayandırırıq və ümumi keçən vaxtı göstəririk
  console.timeEnd("Məlumat yükləmə");
}, 1200);

// Nəticə:
// Məlumat yükləmə: 505.12ms İlkin datalar yükləndi.
// Məlumat yükləmə: 1201.34ms - timer ended
```

---
### 11.8.1 Konsol ilə Formatlı Çıxış (Formatted Output) 🎨

`console.log` və bənzəri metodların ilk arqumenti C proqramlaşdırma dilindəki kimi formatlaşdırma təyin edə bilər. `%s`, `%d` kimi xüsusi işarələr sonrakı arqumentlərin dəyərləri ilə əvəz olunur.

* **`%s`**: Sətir (String)
* **`%d`** və **`%i`**: Tam rəqəm (Integer)
* **`%f`**: Həqiqi rəqəm (Floating-point)
* **`%o`** və **`%O`**: Obyekt (Object)
* **`%c`**: CSS stili (yalnız brauzerdə işləyir)

**Nümunə 1: Müxtəlif tiplərin formatlanması**
```javascript
const name = "Ayan";
const age = 25;
const balance = 152.75;
const user = { name: "Ayan", age: 25 };

console.log(
  "İstifadəçi: %s, Yaş: %d, Balans: %f AZN",
  name,
  age,
  balance
);

console.log("Obyektin detallı görünüşü: %O", user);
```

**Nümunə 2: CSS ilə rəngli çıxış (Brauzer Konsolu üçün)**
Bu, konsolda mesajları daha diqqətəçarpan etmək üçün əla bir yoldur.
```javascript
console.log(
  "Bu normal mətndir. %cBu hissə qırmızı və qalındır!%c Bu isə yenidən normaldır.",
  "color: red; font-weight: bold; font-size: 16px;", // Birinci %c-nin stili
  "color: black;" // İkinci %c stili sıfırlayır
);
```
---


### 11.9 URL API-ları (URL APIs) 🔗
Veb proqramlaşdırmada URL-lərlə işləmək çox yayğın bir tələbatdır. JavaScript-də URL-ləri emal etmək, hissələrə ayırmaq (parse), dəyişdirmək və düzgün formatlamaq üçün müasir **`URL`** sinifi (class) mövcuddur.

`URL` sinifi standart ECMAScript-in bir hissəsi olmasa da, bütün müasir brauzerlər və Node.js tərəfindən tam dəstəklənir.

#### `URL` Obyektinin Yaradılması və Xüsusiyyətləri (Properties)
`new URL()` konstruktoru ilə bir URL obyekti yaradılır. Bu obyekt URL-i avtomatik olaraq hissələrə ayırır və bu hissələrə xüsusiyyətlər (properties) vasitəsilə asanlıqla müraciət etmək olur.

**Nümunə 1: URL-in hissələrə ayrılması**
```javascript
const myUrl = "https://www.example.com:8080/axtarish?q=JavaScript&lang=az#netice-1";
const url = new URL(myUrl);

console.log("Bütün URL (href):", url.href);
console.log("Protokol (protocol):", url.protocol); // "https:"
console.log("Host (host):", url.host);             // "www.example.com:8080"
console.log("Hostname (hostname):", url.hostname); // "www.example.com"
console.log("Port (port):", url.port);             // "8080"
console.log("Yol (pathname):", url.pathname);       // "/axtarish"
console.log("Axtarış hissəsi (search):", url.search);   // "?q=JavaScript&lang=az"
console.log("Lövbər (hash):", url.hash);             // "#netice-1"
console.log("Mənbə (origin):", url.origin);         // "https://www.example.com:8080"
```

**Nümunə 2: URL hissələrini dəyişdirmək və avtomatik formatlama**
`URL` sinifinin ən böyük üstünlüklərindən biri, xüsusi simvolları avtomatik olaraq düzgün formatlamasıdır (escaping).
```javascript
const url = new URL("https://mysite.com");

// Pathname-ə boşluq və Azərbaycan hərfləri olan bir yol əlavə edirik
url.pathname = "/məhsullar/yeni məhsul";

// search hissəsinə # kimi xüsusi simvol əlavə edirik
url.search = "filter=yeni#";

console.log("Avtomatik formatlanmış URL:", url.href);
// ✅ Nəticə: https://mysite.com/m%C9%99hsullar/yeni%20m%C9%99hsul?filter=yeni%23
// Gördüyünüz kimi, 'ə', ' ' (boşluq) və '#' simvolları düzgün kodlaşdırıldı.
```

#### Axtarış Parametrləri (Search Params): `URLSearchParams`
URL-in `?`-dən sonrakı hissəsini (`q=test&cat=tech` kimi) idarə etmək üçün `search` xüsusiyyəti əvəzinə, daha güclü olan `searchParams`-dan istifadə etmək daha rahatdır. Bu, `URLSearchParams` adlı xüsusi bir obyekt qaytarır.

**Geniş `URLSearchParams` Nümunəsi:**
```javascript
const url = new URL("https://example.com/search");
console.log("Başlanğıc URL:", url.href);

// 1. Parametrlər əlavə edirik: .append()
url.searchParams.append('q', 'JavaScript');
url.searchParams.append('cat', 'proqramlaşdırma');
console.log("Parametr əlavə edildi:", url.href); // ...?q=JavaScript&cat=proqramlaşdırma

// 2. Parametrin dəyərini dəyişirik: .set()
url.searchParams.set('q', 'ECMAScript'); // 'q'-nin köhnə dəyərini silib yenisini yazır
console.log("Parametr dəyişdirildi:", url.href); // ...?q=ECMAScript&cat=proqramlaşdırma

// 3. Eyni adlı ikinci bir parametr əlavə edirik
url.searchParams.append('cat', 'veb');
console.log("Eyni adlı parametr əlavə edildi:", url.href); // ...&cat=proqramlaşdırma&cat=veb

// 4. Parametrləri alırıq: .get() və .getAll()
console.log("`q` parametrinin dəyəri:", url.searchParams.get('q')); // ✅ "ECMAScript"
console.log("`cat` parametrinin bütün dəyərləri:", url.searchParams.getAll('cat')); // ✅ ["proqramlaşdırma", "veb"]

// 5. Parametri silirik: .delete()
url.searchParams.delete('cat');
console.log("Parametr silindi:", url.href); // ...?q=ECMAScript

// 6. Bütün parametrlər üzərində dövr etmək (loop)
for (const [key, value] of url.searchParams) {
  console.log(`Açar: ${key}, Dəyər: ${value}`);
}
```

---
### 11.9.1 Köhnə (Legacy) URL Funksiyaları 📜
`URL` sinifindən əvvəl, JavaScript-də URL kodlaşdırması üçün qlobal funksiyalar mövcud idi. Onları bilmək faydalıdır, amma istifadələri məsləhət görülmür.

* **`escape()`/`unescape()`**: Ən köhnə və **istifadədən qalxmış (deprecated)** funksiyalar. **Heç vaxt istifadə etməyin.**
* **`encodeURI()`/`decodeURI()`**: Bütöv bir URL-i kodlaşdırmaq üçündür. URL ayırıcılarını (`:`, `/`, `?`, `#`, `&`) kodlaşdırmır.
* **`encodeURIComponent()`/`decodeURIComponent()`**: URL-in tək bir hissəsini (məsələn, axtarış dəyərini) kodlaşdırmaq üçündür. Bütün xüsusi simvolları, o cümlədən ayırıcıları kodlaşdırır.

**`encodeURI` və `encodeURIComponent` arasındakı fərq:**
```javascript
const fullUrl = "http://example.com/search?q=c++ kod";
const urlComponent = "c++ kod";

// encodeURI bütün URL üçün nəzərdə tutulub, / : ? & = # simvollarını dəyişmir.
console.log("encodeURI:", encodeURI(fullUrl));
// ✅ Nəticə: http://example.com/search?q=c++%20kod

// encodeURIComponent isə bir hissə üçün nəzərdə tutulub, bütün xüsusi simvolları dəyişir.
console.log("encodeURIComponent:", encodeURIComponent(urlComponent));
// ✅ Nəticə: c%2B%2B%20kod
```
**Son Nəticə:** Bu köhnə funksiyalarla başınızı ağrıtmaqdansa, URL ilə bağlı bütün əməliyyatlar üçün **hər zaman müasir `URL` sinifindən istifadə edin.** O, həm daha güclü, həm də daha təhlükəsizdir.

---

### 11.10 Zamanlayıcılar (Timers) ⏳
JavaScript-in ilk günlərindən bəri brauzerlər və Node.js, proqramlara müəyyən bir müddət sonra və ya periodik olaraq funksiya çağırmaq imkanı verən iki əsas funksiya təqdim edir: **`setTimeout()`** və **`setInterval()`**. Bu funksiyalar kodun dərhal yox, gələcəkdə icra olunmasını təmin edir və asinxron (asynchronous) proqramlaşdırmanın təməlini təşkil edir.

---
#### `setTimeout()` — Birdəfəlik Gecikmə
Bu funksiya, verilmiş bir funksiyanı (callback) müəyyən bir gecikmədən (delay) sonra **yalnız bir dəfə** icra etmək üçün istifadə olunur.
`setTimeout(funksiya, gecikmə_ms)`

* **`funksiya`**: Gecikmədən sonra icra olunacaq funksiya.
* **`gecikmə_ms`**: Gecikmə müddəti (millisaniyə ilə). `1000ms = 1 saniyə`.

**❗️ Vacib Məqam:** `setTimeout` asinxron işləyir. Yəni, o, funksiyanı icra üçün "növbəyə" qoyur və dərhal sonrakı kodun icrasına davam edir, gecikmənin bitməsini gözləmir.

**Nümunə 1: Asinxron davranış**
```javascript
console.log("Birinci mesaj (dərhal icra olunur)");

setTimeout(() => {
  console.log("İkinci mesaj (2 saniyə sonra icra olunur)");
}, 2000);

console.log("Üçüncü mesaj (dərhal icra olunur)");

// Konsolun çıxışı:
// ✅ Birinci mesaj (dərhal icra olunur)
// ✅ Üçüncü mesaj (dərhal icra olunur)
// ...2 saniyə sonra...
// ✅ İkinci mesaj (2 saniyə sonra icra olunur)
```

**Nümunə 2: Zamanlayıcını ləğv etmək (`clearTimeout`)**
`setTimeout` çağırıldıqda bir "ID" dəyəri qaytarır. Bu ID-ni `clearTimeout()` funksiyasına ötürərək, planlaşdırılmış əməliyyatı ləğv etmək olar.
```javascript
// 5 saniyə sonra "Partlayış!" mesajı çıxmalı olan bir "bomba" quraşdırırıq
const bombTimerId = setTimeout(() => {
  console.log("💣 Partlayış!");
}, 5000);

// Ancaq 3 saniyə sonra fikrimizi dəyişirik və bombanı zərərsizləşdiririk
setTimeout(() => {
  clearTimeout(bombTimerId);
  console.log("😌 Bomba zərərsizləşdirildi, partlayış olmayacaq.");
}, 3000);
```

---
#### `setInterval()` — Təkrarlanan Gecikmə
Bu funksiya, verilmiş bir funksiyanı müəyyən bir interval (interval) ilə **davamlı olaraq, təkrar-təkrar** icra edir.
`setInterval(funksiya, interval_ms)`

**`clearInterval()`** isə `setInterval` ilə başladılmış təkrarlanan prosesi dayandırmaq üçün istifadə olunur.

**Geniş Nümunə: Rəqəmsal Saat ⏰**
Gəlin `setInterval` ilə hər saniyə yenilənən bir rəqəmsal saat düzəldək və 10 saniyə sonra onu dayandıraq.
```javascript
console.log("Rəqəmsal saat başladıldı... (10 saniyə sonra dayanacaq)");

// Hər 1000ms-dən (1 saniyə) bir konsolu təmizləyib, hazırkı vaxtı yazdırırıq
const clockIntervalId = setInterval(() => {
  console.clear(); // Konsolu təmizləyir
  const now = new Date();
  const timeString = now.toLocaleTimeString('az-AZ'); // Vaxtı Azərbaycan formatında götürürük
  console.log(`🕒 ${timeString}`);
}, 1000);

// 10 saniyə sonra `clearInterval` ilə saatı dayandırırıq
setTimeout(() => {
  clearInterval(clockIntervalId);
  console.log("Saat dayandırıldı.");
}, 10000);
```
