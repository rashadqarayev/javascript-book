### Fəsil 11: JavaScript-in Standart Kitabxanasına Baxış

Bu fəsildə JavaScript-in daxilində olan, **hər proqramda istifadə oluna bilən hazır siniflər və funksiyalar** təqdim olunur. Yəni əlavə heç nə yükləmədən, **JavaScript-in özündə olan alətlərdən** danışırıq.

### Bu fəsildə nə öyrənəcəyik?

Burada aşağıdakı **hazır alətlərlə** tanış olacağıq:

- **Set və Map** – Unikal dəyərlər və açar-dəyər (key-value) saxlamaq üçün.
- **TypedArray və ArrayBuffer** – İkili data (binary) ilə işləmək üçün.
- **RegExp** – Mətnlə (string) daha ağıllı işləmək üçün nümunələr yaratmaq.
- **Date** – Tarix və zamanla işləmək.
- **Error** – Səhvləri yaratmaq və idarə etmək.
- **JSON** – Data-nı string kimi saxlamaq və oxumaq.
- **Intl** – Lokallaşma (məsələn, fərqli valyuta və tarix formatları).
- **Console** – Debug və log üçün.
- **URL** – URL-ləri parçalayıb işləmək.
- **setTimeout və s.** – Kodun müəyyən vaxtdan sonra işləməsi üçün.

---

### 11.1.1 `Set` Sinfi (Class)

`Set` — JavaScript-də təkrarsız dəyərləri saxlayan xüsusi bir kolleksiyadır. Massivə bənzəsə də, onun fərqli xüsusiyyətləri var:

- **Təkrarlara icazə vermir.** Yəni, bir dəyəri `Set`-ə iki dəfə əlavə etsən də, sadəcə bir dəfə saxlanır.
- **İndeksli deyil.** `Set`-də `s[0]` kimi indeks vasitəsilə dəyərə birbaşa giriş yoxdur.
- **Əlavə edilən elementlərin daxil olma sırasını saxlayır.** Yəni, iterasiya zamanı elementlər əlavə olunduğu qaydada gəlir.

### Set yaratmaq

Yeni `Set` yaratmaq üçün `new Set()` yazırıq. İstəyə görə `Set`-ə əvvəlcədən elementlər verə bilərik.

```javascript
let s = new Set(); // boş Set

let t = new Set([1, 2, 3]); // 1, 2, 3 elementləri olan Set
// new Set([1,2,3])

let strSet = new Set("hello");
// "h", "e", "l", "o" — stringdəki unikal hərfləri saxlayır
```

- **Nəzərə alın:** `Set` konstruktoru istənilən iterable obyekt qəbul edir. Buna massiv, string, hətta başqa `Set` də daxildir.

```javascript
let s2 = new Set(t); // t Set-in elementləri ilə yeni Set yaradırıq
```

### **Element əlavə etmək — `add()`**

`Set`-ə element əlavə etmək üçün `add()` metodundan istifadə edirik.

```javascript
let s = new Set();

s.add(1);
s.add(2);
s.add(2); // təkrar əlavə olunur, amma təsiri yoxdur

console.log(s.size); // 2
```

- `add()` metodunun qaytarışı `Set`-in özüdür, buna görə zəncirləmə mümkündür:

```javascript
s.add(3).add(4).add(5);
console.log(s.size); // 5
```

---

### **Element silmək — `delete()` və `clear()`**

- `delete()` metodu verilmiş elementi `Set`-dən silir. Əgər element varsa `true`, yoxdursa `false` qaytarır.

```javascript
s.delete(2); // true
s.delete(42); // false, belə element yoxdur
```

- `clear()` metodu `Set`-dəki bütün elementləri silir.

```javascript
s.clear();
console.log(s.size); // 0
```

---

#### Üzvlük Yoxlaması (`has()`) ✅

Set-lər ilə ən vacib əməliyyat, müəyyən bir dəyərin set-in üzvü olub olmadığını `has()` metodu (method) ilə yoxlamaqdır:

```javascript
let oneDigitPrimes = new Set([2, 3, 5, 7]);
console.log(oneDigitPrimes.has(2)); // => true
console.log(oneDigitPrimes.has(4)); // => false
console.log(oneDigitPrimes.has("5")); // => false ("5" rəqəm olmadığı üçün)
```

#### Set-ləri İterasiya Etmək (Iterating Sets) 🔄

`Set` sinfi iterable (təkrarlana bilən) olduğundan, `for/of` dövrü ilə bütün elementlərini sıralaya bilərsiniz:

```javascript
let sum = 0;
for (let p of oneDigitPrimes) {
  // Sadə ədədləri dövrə salın və toplayın
  sum += p;
}
console.log(sum); // => 17
```

`Set` obyektləri iterable olduğu üçün, onları `...` spread operatoru ilə massivlərə və funksiya arqumentlərinə çevirə bilərsiniz:

```javascript
console.log([...oneDigitPrimes]);
// => [2,3,5,7]: Set massivə (array) çevrildi

console.log(Math.max(...oneDigitPrimes));
// => 7: Set elementləri funksiya arqumentləri kimi ötürüldü
```

`Set`-lər həmçinin `forEach()` metodu ilə işlədilə bilir:

```javascript
let product = 1;
oneDigitPrimes.forEach((n) => {
  product *= n;
});
console.log(product); // => 210
```

---

### 11.1.2 `Map` Sinfi

`Map`, JavaScript-də **açar–dəyər (key–value)** cütlərini saxlayan xüsusi obyekt sinfidir. `Object`-dən fərqli olaraq, `Map` istənilən **dəyəri** (nəinki yalnız `string` və `symbol`) açar kimi istifadə etməyə imkan verir və əlavə funksiyalar təqdim edir.

---

#### Map Yaratmaq

Yeni `Map` yaratmaq üçün `new Map()` konstruktoru istifadə olunur:

```javascript
let m = new Map();
console.log(m.size); // 0
```

İlkin açar/dəyər cütləri ilə yaratmaq:

```javascript
let n = new Map([
  ["one", 1],
  ["two", 2],
]);

console.log(n.get("one")); // 1
console.log(n.size); // 2
```

`Object`-dən `Map` yaratmaq:

```javascript
let o = { x: 1, y: 2 };
let p = new Map(Object.entries(o));

console.log(p.get("x")); // 1
console.log(p.get("y")); // 2

console.log(p);
// new Map([["x", 1 ],["y", 2]])
```

---

#### Əsas Metodlar

| Metod / Xüsusiyyət | Təsviri                                              |
| ------------------ | ---------------------------------------------------- |
| `set(key, value)`  | Açar əlavə edir və ya mövcud açarın dəyərini dəyişir |
| `get(key)`         | Açarın dəyərini qaytarır                             |
| `has(key)`         | Açar mövcuddurmu? `true`/`false`                     |
| `delete(key)`      | Açarı və onun dəyərini silir                         |
| `clear()`          | Bütün cütləri silir                                  |
| `size`             | Neçə cüt olduğunu göstərir                           |

---

#### Nümunə: Əlavə Etmək və Oxumaq

```javascript
let m = new Map();

m.set("a", 1);
m.set("b", 2);

console.log(m.get("a")); // 1
console.log(m.get("b")); // 2
console.log(m.get("c")); // undefined

console.log(m.size); // 2
```

---

#### Mövcud Açarı Yeniləmək

```javascript
m.set("a", 100);
console.log(m.get("a")); // 100
console.log(m.size); // 2 (dəyişmir)
```

---

#### Silmək və Təmizləmək

```javascript
m.delete("b");
console.log(m.has("b")); // false

m.clear();
console.log(m.size); // 0
```

---

#### Açar (Key) Müqayisəsi (Key Comparison)

`Map` obyektində **istənilən JavaScript dəyəri** açar kimi istifadə edilə bilər: `null`, `undefined`, `NaN`, obyektlər, massivlər və s. Lakin burada açarlar **referens əsasında** müqayisə olunur, yəni `===` operatoru ilə.
Bu o deməkdir ki, iki fərqli obyekt və ya massiv **eyni görünüşə malik olsa belə**, `Map` onları **fərqli açar** kimi qəbul edir.

```javascript
let m = new Map(); // Boş bir map ilə başlayın.
m.set({}, 1); // Bir boş obyekti (object) 1-ə xəritələyin.
m.set({}, 2); // Başqa bir boş obyekti (object) 2-yə xəritələyin.

console.log(m.size);
// => 2: bu map-də (map) iki açar (key) var.
console.log(m.get({}));
// => undefined: bu boş obyekt (object) açar (key) deyil (çünki referans fərqlidir).

m.set(m, undefined);

console.log(m.has(m));
// => true: m özü-özünün açarıdır.
console.log(m.get(m));
// => undefined: açar (key) olmasa da bu dəyəri (value) alırdıq.
```

---

#### İterasiya

`Map`-dəki cütləri `for...of` dövrü ilə iterasiya etmək olar:

```javascript
let m = new Map([
  ["x", 10],
  ["y", 20],
]);

for (let [key, value] of m) {
  console.log(key, value);
}
// Output:
// x 10
// y 20
```

---

#### Digər İterasiya Variantları

```javascript
console.log([...m.keys()]);
// => ["x", "y"]: yalnız açarlar (keys)

console.log([...m.values()]);
// => [1, 2]: yalnız dəyərlər (values)

console.log([...m.entries()]);
// => [["x", 1], ["y", 2]]: [...m] ilə eyni
```

---

#### `forEach()` ilə Iterasiya

```javascript
m.forEach((value, key) => {
  console.log(`Key: ${key}, Value: ${value}`);
});
// Output:
// Key: x, Value: 10
// Key: y, Value: 20
```

**Qeyd:** `forEach()` metodunda birinci parametr `value`, ikinci `key` olur (massivlərdə olduğu kimi).

---

#### 🧵 Zəncirləmə (Chaining)

```javascript
let m = new Map().set("a", 1).set("b", 2).set("c", 3);

console.log(m.size); // 3
```

---

---

### 11.1.3 `WeakMap` və `WeakSet`

JavaScript-də `WeakMap` və `WeakSet` **zəif referens** (weak reference) əsasında işləyən kolleksiyalardır. Bu, o deməkdir ki, daxilində saxlanılan obyektlər başqa heç yerdə istifadə olunmursa, **garbage collector** (zibil toplayıcı) onları avtomatik təmizləyə bilər. Bu xüsusiyyət, xüsusilə **yaddaş sızmalarının** qarşısını almaqda faydalıdır.

---

#### `WeakMap` Sinfi

`WeakMap`, `Map`-in variantıdır, lakin burada açarlar yalnız **obyekt** və ya **massiv** ola bilər. Primitiv tiplərə (məsələn, string, number) icazə verilmir.

**Əsas fərqlər:**

- `WeakMap` yalnız `get()`, `set()`, `has()`, `delete()` metodlarını dəstəkləyir.
- `forEach()`, `size`, `keys()` kimi metodlar yoxdur — çünki bu metodlar açarları əlçatan edərdi və zəif referens prinsipi pozulardı.
- **İterable deyil** – yəni `for...of` ilə istifadə oluna bilməz.

**Nümunə**: Obyektə gizli etiket əlavə etmək

```javascript
const userInfo = new WeakMap();

let user = { name: "Ali" };

// `user` obyektinə gizli bir məlumat əlavə edirik
userInfo.set(user, "admin");

console.log(userInfo.get(user)); // => "admin"

user = null; // Artıq istifadə olunmur, GC tərəfindən silinə bilər
```

Burada `user` obyektinə əlavə etdiyimiz `"admin"` məlumatı, `WeakMap` daxilində idi. `user` artıq istifadə olunmadıqda, bu məlumat da **avtomatik silinir**. Əgər `Map` istifadə olunsaydı, bu informasiya yaddaşda qalardı.

---

#### `WeakSet` Sinfi

`WeakSet`, `Set`-in zəif referens saxlayan versiyasıdır və yalnız **obyektləri** üzv kimi qəbul edir.

**Əsas xüsusiyyətlər:**

- Primitiv dəyərlərə icazə verilmir.
- `add()`, `has()`, `delete()` metodları var.
- `forEach()`, `size` və iterable funksiyalar yoxdur.

**Nümunə: Obyektləri İşarələmək**

```javascript
const activeUsers = new WeakSet();
let user1 = { name: "Aysel" };
let user2 = { name: "Murad" };

activeUsers.add(user1); // Aysel aktivdir

console.log(activeUsers.has(user1)); // true
console.log(activeUsers.has(user2)); // false

user1 = null; // Artıq istifadə olunmur
// `WeakSet` içindəki referens silinə bilər
```

Bu nümunədə, `user1` artıq istifadə olunmur deyə, onunla bağlı məlumat yaddaşdan **təmizlənə bilər**. `Set` istifadə olunsaydı, bu mümkün olmayacaqdı.

---

## 11.2 Typed Arrays və Binary Data

JavaScript-də adi `Array`-lər çevik və istənilən tipdə dəyərlər saxlaya bilər. Lakin bu massivlər aşağı səviyyəli dillərdəki kimi sabit ölçülü və tipli deyillər. Yaddaş baxımından optimallaşdırılmış strukturlar lazım olduqda **Typed Arrays** istifadə olunur.

**Typed Array** — vahid tipli ədədlərdən ibarət sabit uzunluqlu massivlərdir. ES6 ilə gəlmişdir və **ikili məlumatların** (binary data) işlənməsi üçün nəzərdə tutulmuşdur.

---

### Əsas Xüsusiyyətlər

1. **Yalnız ədədlər saxlayır** — Məzmun yalnız `number` və ya `BigInt` ola bilər.
2. **Element tipi və ölçüsü sabitdir** — Hər tip, dəyərləri müəyyən bit ölçüsündə (8, 16, 32, 64) saxlayır.
3. **Sabit uzunluqludur** — Yaradıldıqda uzunluğu təyin olunur və dəyişmir.
4. **Avtomatik sıfırlama** — Yeni massivdə bütün dəyərlər `0` ilə başlanır.
5. **Adi `Array` deyil** — `Array.isArray()` nəticəsi `false` olur.

---

### 11 Növ Typed Array və Onların Təyinatı

| Constructor         | Saxlanılan Dəyər         | Qısa İzah                                                          |
| ------------------- | ------------------------ | ------------------------------------------------------------------ |
| `Int8Array`         | 8-bit signed tam ədəd    | -128 → 127                                                         |
| `Uint8Array`        | 8-bit unsigned tam ədəd  | 0 → 255                                                            |
| `Uint8ClampedArray` | 8-bit unsigned (clamped) | 0 → 255, kənar dəyərlər 0 və 255-ə sabitlənir (rəng məlumatı üçün) |
| `Int16Array`        | 16-bit signed            | -32768 → 32767                                                     |
| `Uint16Array`       | 16-bit unsigned          | 0 → 65535                                                          |
| `Int32Array`        | 32-bit signed            | ±2 milyardlıq aralıq                                               |
| `Uint32Array`       | 32-bit unsigned          | 0 → 4 milyarddan çox                                               |
| `Float32Array`      | 32-bit onluq ədəd        | Daha az dəqiqlik, daha az yaddaş (C-də `float`)                    |
| `Float64Array`      | 64-bit onluq ədəd        | JavaScript-dəki adi `Number` tipi                                  |
| `BigInt64Array`     | 64-bit signed `BigInt`   | Çox böyük tam ədədlər üçün                                         |
| `BigUint64Array`    | 64-bit unsigned `BigInt` | Yalnız müsbət `BigInt` üçün                                        |

---

### Texniki Qeyd

- Hər konstruktorun `.BYTES_PER_ELEMENT` property-si var:

  ```js
  Float64Array.BYTES_PER_ELEMENT; // 8
  Int16Array.BYTES_PER_ELEMENT; // 2
  ```

  Bu, bir elementin neçə bayt tutduğunu bildirir.

---

### Niyə Typed Array?

- Fayllarla, şəkillərlə, video/audio ilə işləyərkən (binary data)
- WebGL və oyun mühərriklərində performans üçün
- Xarici API və ya sistemlərlə (məs. sensorlar, socket-lər) əlaqədə ikili məlumat formatı tələb olunduqda

---

### `Signed` və `Unsigned` Nədir?

Tipli massivlərdə ədədlər iki əsas kateqoriyaya ayrılır:

| Tip        | Açıqlama                                                        | Ədəd Aralığı                                | Nümunə Konstruktorlar                                                             |
| ---------- | --------------------------------------------------------------- | ------------------------------------------- | --------------------------------------------------------------------------------- |
| `Signed`   | Həm **müsbət**, həm də **mənfi** ədədlər saxlaya bilər.         | Simmetrik aralıq: məsələn, `-128` → `127`   | `Int8Array`, `Int16Array`, `Int32Array`, `BigInt64Array`                          |
| `Unsigned` | Yalnız **müsbət** ədədlər saxlaya bilər. Mənfi dəyərlər yoxdur. | 0-dan maksimum dəyərə: məsələn, `0` → `255` | `Uint8Array`, `Uint16Array`, `Uint32Array`, `BigUint64Array`, `Uint8ClampedArray` |

#### Məsələn:

- `Int8Array`: 8 bitlik massivdir və -128-dən +127-ə qədər tam ədədlər saxlaya bilər (signed).
- `Uint8Array`: Eyni ölçülü massivdir (8 bit), lakin yalnız 0-dan 255-ə qədər müsbət ədədlər saxlaya bilər (unsigned).

---

## 11.2.2 Tipli Massivləri Yaratmaq (Creating Typed Arrays)

JavaScript-də tipli massivlər (**typed arrays**) yaddaşda optimallaşdırılmış, sabit ölçülü və sabit tipli massivlərdir. Onlar əsasən binary verilənlərlə işləmək, məsələn, şəkillər, səslər və fayllar üçün istifadə olunur.

Typed array-ləri yaratmaq üçün üç əsas üsul var:

## 1. Uzunluqla Yaratmaq (Creating with Length)

`TypedArray` konstruktora istədiyiniz element sayını verirsiniz. Bütün elementlər avtomatik `0` ilə doldurulur.

```js
let bytes = new Uint8Array(5);
console.log(bytes); // Uint8Array(5) [0, 0, 0, 0, 0]
console.log(bytes.length); // 5
console.log(bytes[0]); // 0
```

Digər nümunələr:

```js
let matrix = new Float64Array(4); // 4 elementli 64-bit float massivi
console.log(matrix); // Float64Array(4) [0, 0, 0, 0]

let point = new Int16Array(3); // 3 elementli 16-bit işarəli tam ədəd massivi
console.log(point); // Int16Array(3) [0, 0, 0]
```

---

## 2. Mövcud Dəyərlərdən Yaratmaq (Creating from Existing Values)

### a) `of()` metodu ilə

Birbaşa elementləri verərək yaradılır:

```js
let pixel = Uint8ClampedArray.of(255, 128, 64, 0);
console.log(pixel); // Uint8ClampedArray(4) [255, 128, 64, 0]
```

### b) `from()` metodu ilə

Mövcud massiv və ya iterable obyektlərdən yaradır:

```js
let numbers = [10, 20, 30];
let uint8Numbers = Uint8Array.from(numbers);
console.log(uint8Numbers); // Uint8Array(3) [10, 20, 30]
```

---

## 3. `ArrayBuffer` ilə Yaratmaq (Creating with `ArrayBuffer`)

```javascript
let buffer = new ArrayBuffer(16);
// 16 baytlıq bufer yaradırıq.

// Bu buferə fərqli "görünüşlər" (views) yaradırıq:
let view8 = new Uint8Array(buffer);
// 8-bit işarəsiz tam ədədlər kimi baxış (view)
console.log(view8);
// => Uint8Array(16) [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

let view32 = new Int32Array(buffer);
// 32-bit işarəli tam ədədlər kimi baxış (view)
console.log(view32);
// => Int32Array(4) [0, 0, 0, 0] (16 bayt / 4 bayt = 4 element)

// Məsələn, view8 vasitəsilə dəyər yazsaq, view32-də də dəyişikliyi görərik:
view8[0] = 255;
view8[1] = 255;
view8[2] = 255;
view8[3] = 255;
console.log(view32[0]);
// => -1 (Uint8Array-də 255-i dörd dəfə yazmaq Int32Array-də -1-ə çevrilir)

// Buferin (buffer) son 4 baytına baxış (view)
let last4Bytes = new Uint8Array(buffer, 12, 4);
// Ofset 12-dən başlayaraq 4 bayt
console.log(last4Bytes);
// => Uint8Array(4) [0, 0, 0, 0]

// Başqa bir görünüş: ofset 4-dən başlayaraq 2 ədəd 32-bit tam ədəd (Int32Array)
// Ofset (4) 32-bit (4 bayt) üçün hizalanmaya (alignment) uyğundur.
let partialInts = new Int32Array(buffer, 4, 2);
console.log(partialInts);
// => Int32Array(2) [0, 0]
```

---

# 11.2.3 Tipli Massivləri İstifadə Etmək (Using Typed Arrays)

## Elementlərə giriş və dəyişiklik

Typed array elementləri kvadrat mötərizə `[index]` ilə oxunur və yazılır. Məsələn:

```js
let temperatures = new Float32Array(4);
temperatures[0] = 36.6;
temperatures[1] = 37.2;
temperatures[2] = 35.9;
temperatures[3] = 36.8;

console.log(temperatures[1]); // 37.2
console.log(temperatures[0] + temperatures[3]); // 73.4
```

---

### Nümunə: 3D koordinatlarda məsafə hesabı

3 ölçülü nöqtələrin məsafəsini hesablamaq üçün Int16Array istifadə edək:

```js
function distance3D(p1, p2) {
  let dx = p2[0] - p1[0];
  let dy = p2[1] - p1[1];
  let dz = p2[2] - p1[2];
  return Math.sqrt(dx * dx + dy * dy + dz * dz);
}

let pointA = new Int16Array([1, 2, 3]);
let pointB = new Int16Array([4, 6, 9]);

console.log(distance3D(pointA, pointB)); // 8.366600265340756
```

---

### Tipli massiv metodları ilə işləmək

Typed array-lər bəzi massiv metodlarını dəstəkləyir. Məsələn, `fill()`, `map()`, `slice()`.

```js
let arr = new Uint8Array(5);
arr.fill(7);

let doubled = arr.map((x) => x * 2);
console.log(doubled); // Uint8Array(5) [14, 14, 14, 14, 14]

let part = doubled.slice(1, 4);
console.log(part); // Uint8Array(3) [14, 14, 14]
```

---

### Metodlarla dəyişiklik nümunəsi: səsləri səs artırmaq (volume boost)

```js
let soundSamples = new Int16Array([100, -100, 200, -200, 0]);

// Hər sample-i 50% artırırıq, amma maksimum 32767-dən çox olmamalıdır
let boosted = soundSamples.map((x) => {
  let boostedVal = x * 1.5;
  if (boostedVal > 32767) return 32767;
  if (boostedVal < -32768) return -32768;
  return boostedVal;
});

console.log(boosted); // Int16Array(5) [150, -150, 300, -300, 0]
```

---

## Vacib xatırlatma

- Typed array-lərin uzunluğu sabitdir, ona görə `push()`, `pop()` kimi metodlar yoxdur.
- Amma `fill()`, `map()`, `slice()` kimi metodlar tam dəstəklənir və nəticədə eyni tipdə yeni typed array qaytarılır.

---

# 11.2.4 Tipli Massiv Metodları (Typed Array Methods) və Xüsusiyyətləri (Properties)

Typed array-lər adi massivlərə oxşayır, amma özlərinə məxsus sürətli metodları və yaddaşla bağlı xüsusiyyətləri var.

---

### 1. `set()` Metodu (Copying values) ✍️

`set()` metodu bir tipli massivə başqa massivdən birdən çox elementi **kopyalamağa** imkan verir.

```js
let bytes = new Uint8Array(12);
let pattern = new Uint8Array([10, 20, 30, 40]);

bytes.set(pattern);
console.log(bytes.slice(0, 4)); // Uint8Array [10, 20, 30, 40]

bytes.set(pattern, 4);
console.log(bytes.slice(4, 8)); // Uint8Array [10, 20, 30, 40]

bytes.set([1, 2, 3, 4], 8);
console.log(bytes.slice(8, 12)); // Uint8Array [1, 2, 3, 4]

console.log(bytes);
// Uint8Array(12) [10, 20, 30, 40, 10, 20, 30, 40, 1, 2, 3, 4]
```

- İlk arqument: kopyalanacaq massiv
- İkinci arqument: başlanğıc indeksi (default 0)

---

### 2. `subarray()` Metodu (Creating views without copying)

`subarray()` massivdən hissə götürür, amma yeni yaddaş ayırmır, sadəcə **mövcud yaddaşa yeni görünüş** yaradır.

```js
let ints = new Int16Array([5, 10, 15, 20, 25, 30]);
let sub = ints.subarray(2, 5); // indeks 2-dən 4-ə kimi hissə
console.log(sub); // Int16Array [15, 20, 25]
console.log(sub[0]); // 15 (ints[2] ilə eynidir)
```

### `subarray()` dəyişiklikləri orijinal massivə təsir edir:

```js
ints[3] = 99;
console.log(sub[1]); // 99
```

---

### 3. `slice()` ilə müqayisə

`slice()` də massivdən hissə götürür, amma **yeni müstəqil massiv yaradır** (yaddaş paylaşımı yoxdur).

```js
let slice = ints.slice(2, 5);
console.log(slice); // Int16Array [15, 99, 25]

ints[3] = 20;
console.log(slice[1]); // 99 (dəyişmir)
console.log(sub[1]); // 20 (dəyişir)
```

---

### 4. Yaddaş və Ofset Xüsusiyyətləri

```js
console.log(sub.buffer); // ArrayBuffer obyektidir
console.log(sub.buffer === ints.buffer); // true (eyni yaddaş)

console.log(sub.byteOffset); // 4 bayt ofset (2 element * 2 bayt)
console.log(sub.byteLength); // 6 bayt (3 element * 2 bayt)
console.log(ints.buffer.byteLength); // 12 bayt (6 element * 2 bayt)
```

---

### 5. `ArrayBuffer`-in indeksləndirilməsi ilə bağlı diqqət

`ArrayBuffer` özü massiv deyil, ona indekslərlə daxil olmaq faydasızdır:

```js
let buffer = new ArrayBuffer(4);
console.log(buffer[0]); // undefined

buffer[0] = 100;
console.log(buffer[0]); // hələ də undefined
```

`ArrayBuffer`-də verilənlərə baxmaq üçün mütləq typed array yaradılmalıdır.

---

### 6. Birdən çox görünüş yaratmaq

Eyni `ArrayBuffer`-ə fərqli tip və uzunluqda görünüş yarada bilərik:

```js
let bytes = new Uint8Array(16); // 16 bayt yaddaş
let ints = new Uint32Array(bytes.buffer); // 4 ədəd 32-bit tam ədəd
let floats = new Float64Array(bytes.buffer, 0, 2); // 2 ədəd 64-bit onluq ədəd

console.log(bytes.length); // 16
console.log(ints.length); // 4
console.log(floats.length); // 2
```

---

| Xüsusiyyət / Metod    | Təsvir                                               |
| --------------------- | ---------------------------------------------------- |
| `set(array, offset)`  | Massivdən birdən çox elementi kopyalayır             |
| `subarray(start,end)` | Yaddaşı paylaşan, yeni görünüş yaradır               |
| `slice(start,end)`    | Yeni müstəqil tipli massiv yaradır                   |
| `buffer`              | Massivin əsas `ArrayBuffer` obyektinə istinad        |
| `byteOffset`          | Görünüşün `ArrayBuffer` içində başladığı bayt ofseti |
| `byteLength`          | Görünüşün baytla uzunluğu                            |

---

### **Fəsil 11.3: Requlyar İfadələrlə Nümunə Axtarışı**

**Requlyar ifadə** – mətndə müəyyən bir nümunəni (pattern) təsvir edən bir obyektdir. JavaScript-də `RegExp` sinfi requlyar ifadələri təmsil edir. Həm `String`, həm də `RegExp` sinifləri, mətn üzərində güclü axtarış və əvəzetmə əməliyyatları aparmaq üçün requlyar ifadələrdən istifadə edən metodlara sahibdir.

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

---

### **Sadə Simvollar və Meta-Simvollar**

Requlyar ifadələrdəki simvolların əksəriyyəti (məsələn, bütün hərflər və rəqəmlər) özünü ifadə edir. Yəni `/a/` ifadəsi sadəcə "a" hərfini axtarır.

Ancaq bəzi simvollar xüsusi məna daşıyır və **meta-simvol** adlanır. Məsələn:

- `$` — Sətrin (string) sonunu bildirir.
- `^` — Sətrin əvvəlini bildirir.

**Nümunə:**
`/s$/` ifadəsi iki simvoldan ibarətdir:

- `s` — "s" hərfini axtarır.
- `$` — həmin "s" hərfinin sətrin sonunda olmasını tələb edir.

---

#### **Fleqlər (Flags)**

Fleqlər, axtarışın davranışını dəyişən xüsusi "açarlardır". Onlar ikinci əyri xətdən (`/`) sonra yazılır.

- `i` — **Case-Insensitive (Böyük-kiçik hərf fərqi olmadan):** Axtarış zamanı böyük və kiçik hərflər arasında fərq qoyulmur.
- `g` — **Global (Qlobal axtarış):** Yalnız ilk uyğunluğu deyil, bütün uyğun gələn nəticələri tapır.

**Nümunə:**
`/s$/i` — Sətrin "s" və ya "S" hərfi ilə bitib-bitmədiyini yoxlayır.

```javascript
let pattern_case_sensitive = /s$/;
let pattern_case_insensitive = /s$/i;

"salamS".test(pattern_case_sensitive); // false (böyük S olduğu üçün)
"salamS".test(pattern_case_insensitive); // true (i fleqi fərqi aradan qaldırır)
```

---

### **Əlavə və Praktik Nümunələr**

Requlyar ifadələri yoxlamaq üçün ən sadə metod `.test()` metodudur. O, uyğunluq varsa `true`, yoxdursa `false` qaytarır.

**Nümunə 1: Mətnin müəyyən sözlə başlamasının yoxlanılması**
`^` simvolu sətrin başlanğıcını bildirir.

```javascript
// "Salam" sözü ilə başlayan sətirləri axtarır
const pattern = /^Salam/;

console.log(pattern.test("Salam, dünya!"));
// ✅ true
console.log(pattern.test("salam, dünya!"));
// ❌ false (böyük hərf tələb olunur)
console.log(pattern.test("Əvvəlcə Salam, dünya!"));
// ❌ false (sətrin əvvəlində deyil)
```

**Nümunə 2: Yalnız rəqəmlərdən ibarət olmanın yoxlanılması**
`\d` meta-simvolu istənilən rəqəmi (`0-9`) ifadə edir. `+` isə "bir və ya daha çox" deməkdir.

```javascript
// Sətrin başdan sona (^...$) yalnız bir və ya daha çox rəqəmdən (\d+) ibarət olmasını yoxlayır
const pattern = /^\d+$/;

console.log(pattern.test("12345")); // ✅ true
console.log(pattern.test("123a5")); // ❌ false ("a" hərfi var)
console.log(pattern.test("123 45")); // ❌ false (boşluq simvolu var)
console.log(pattern.test("12")); // ✅ true
```

**Nümunə 3: E-mail formatının sadə yoxlanışı**

```javascript
// user@domain.com formatını yoxlayır (çox sadələşdirilmiş)
// \S+  -> bir və ya daha çox boşluq olmayan simvol
// @    -> @ simvolu
// \.   -> nöqtə simvolu
const emailPattern = /\S+@\S+\.\S+/;

console.log(emailPattern.test("test@example.com"));
// ✅ true
console.log(emailPattern.test("test.user@example.co.uk"));
// ✅ true
console.log(emailPattern.test("test@example"));
// ❌ false (.com, .net kimi hissə yoxdur)
console.log(emailPattern.test("test example.com"));
// ❌ false (@ simvolu yoxdur)
```

---

### Hərfi Mənada Simvollar (Literal Characters)

#### **1. Adi Simvollar: Hərflər və Rəqəmlər**

Bütün hərflər (`a-z`, `A-Z`) və rəqəmlər (`0-9`) requlyar ifadələrdə özlərini, yəni hərfi mənalarını ifadə edir.

```javascript
// "kod" sözünü axtarır
let pattern = /kod/;

pattern.test("JavaScript kod yazmaq maraqlıdır."); // ✅ true
pattern.test("Bu mətnin mənası fərqlidir."); // ❌ false
```

#### **2. Xüsusi Mənalı Simvollar (Meta-simvollar)**

Bəzi durğu işarələri və simvollar requlyar ifadələrdə xüsusi məna daşıyır. Onlar hərfi mənada özlərini yox, müəyyən bir qaydanı təmsil edirlər. Bu simvollar bunlardır:

`^ $ . * + ? = ! : | \ / ( ) [ ] { }`

Bu simvolların hər birinin öz rolu var və növbəti fəsillərdə onları detallı öyrənəcəyik.

---

#### **3. Xüsusi Simvolları Hərfi Mənada Necə İşlətməli?**

Bəs əgər biz mətndə `.` (nöqtə) və ya `+` (üstəgəl) işarəsinin özünü axtarmaq istəsək, nə etməliyik?

**Qayda:** Xüsusi mənalı bir simvolu hərfi mənada axtarmaq üçün onun önünə `\` (backslash) əlavə etmək lazımdır.

**Praktik Nümunə 1: Domen adında nöqtəni tapmaq**

Tutaq ki, `“sayt.az”` mətnindəki nöqtəni tapmaq istəyirik.

```javascript
let text = "Mənim veb-saytım sayt.az adresidir.";

// YANLIŞ YOL: /sayt.az/
// Bu ifadə "saytXaz" kimi uyğunluqları da tapacaq, çünki "." meta-simvolu
// "istənilən bir simvol" deməkdir.
let wrongPattern = /sayt.az/;
wrongPattern.test("sayt-az");
// ✅ true, amma biz bunu istəmirdik.

// DOĞRU YOL: /sayt\.az/
// Burada \. ifadəsi nöqtənin xüsusi mənasını "sındırır" və onu hərfi mənada axtarır.
let correctPattern = /sayt\.az/;
correctPattern.test(text);
// ✅ true
correctPattern.test("sayt-az");
// ❌ false
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

#### **4. Nəzarət Simvolları və Tərs Sleş `\`**

Hərfi mənada görünməyən, ancaq mətndə mövcud olan bəzi xüsusi simvollar da (`\n` – yeni sətir, `\t` – tab) tərs sleş vasitəsilə ifadə olunur.

| Simvol | Mənası                |
| :----- | :-------------------- |
| `\n`   | Yeni sətir            |
| `\t`   | Tab (boşluq)          |
| `\r`   | "Carriage return"     |
| `\0`   | NUL simvolu           |
| `\\`   | Tərs sleşin (`\`) özü |

Bəs tərs sleşin özünü (`\`) mətndə necə axtara bilərik? Onu: `\\`.

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

console.log(literalPattern.test("C:\\")); // ✅ true
console.log(constructorPattern.test("C:\\")); // ✅ true
```

---

---

## BURA KIMI BAXMISAN

### Simvol Qrupları (Character Classes)

Tutaq ki, mətndə "a", "b" və ya "c" hərflərindən _hər hansı birini_ tapmaq istəyirik. Bunu `/a|b|c/` kimi yaza bilərik, amma daha qısa və oxunaqlı bir yolu var: simvol qrupları.

Simvol qrupu, kvadrat mötərizə `[...]` içərisinə yazılan simvollar toplusudur və həmin topludan **yalnız bir simvolun** uyğun gəlməsini yoxlayır.

#### **1. Əsas Simvol Qrupları: `[...]`**

`/\[abc]/` ifadəsi "a", "b" və ya "c" hərflərindən hər hansı birinə uyğun gəlir.

```javascript
const pattern = /[abc]/;

pattern.test("salam"); // ✅ true ("a" hərfi var)
pattern.test("bina"); // ✅ true ("b" hərfi var)
pattern.test("kod"); // ❌ false (nə "a", nə "b", nə də "c" yoxdur)
pattern.test("abc"); // ✅ true (hər üçü var, ilk "a"-ya görə true qaytarır)
```

**Aralıqlar (Ranges):** Tək-tək bütün simvolları yazmaq əvəzinə, defis (`-`) ilə aralıq təyin edə bilərsiniz.

- `/[a-z]/` — Bütün kiçik latın hərflərindən hər hansı biri.
- `/[A-Z]/` — Bütün böyük latın hərflərindən hər hansı biri.
- `/[0-9]/` — Bütün rəqəmlərdən hər hansı biri.
- `/[a-zA-Z0-9_]/` — İstənilən hərf, rəqəm və ya alt xətt.

#### **2. İnkar Edilmiş Qruplar: `[^...]`**

Əgər mötərizənin içində ilk simvol kimi `^` (caret) işarəsi qoysanız, qrup tam əksinə işləyəcək: mötərizə içindəki simvollar **xaricində** istənilən bir simvolu axtaracaq.

`/[^abc]/` — "a", "b" və "c" hərfləri _istisna olmaqla_ istənilən bir simvol.

```javascript
const pattern = /[^0-9]/; // Rəqəm olmayan istənilən simvol

pattern.test("123"); // ❌ false (bütün simvollar rəqəmdir)
pattern.test("12a"); // ✅ true ("a" simvolu rəqəm deyil)
pattern.test("salam"); // ✅ true (bütün simvollar rəqəm deyil)
```

**Diqqət:** Mötərizə daxilindəki `^` ilə requlyar ifadənin əvvəlindəki `^` (sətrin başlanğıcı) fərqli mənalar daşıyır.

#### **3. Hazır Qısayollar (Shorthands)**

Bəzi simvol qrupları o qədər çox istifadə olunur ki, onlar üçün xüsusi qısayollar mövcuddur.

| Qısayol  | Açıqlaması                                           | Ekvivalenti      |
| :------- | :--------------------------------------------------- | :--------------- |
| **`.`**  | **İstənilən simvol** (yeni sətir `\n` xaric)         |                  |
| **`\d`** | İstənilən **rəqəm** (digit)                          | `[0-9]`          |
| **`\D`** | Rəqəm **olmayan** istənilən simvol                   | `[^0-9]`         |
| **`\w`** | İstənilən **hərf, rəqəm və ya `_`** (word character) | `[a-zA-Z0-9_]`   |
| **`\W`** | `\w`-dəki simvollar **xaricində** qalan hər şey      | `[^a-zA-Z0-9_]`  |
| **`\s`** | İstənilən **boşluq** simvolu (space, tab, new line)  | `[ \t\n\r\f\v]`  |
| **`\S`** | Boşluq **olmayan** istənilən simvol                  | `[^ \t\n\r\f\v]` |

Bu qısayolları birbaşa və ya kvadrat mötərizələrin daxilində istifadə edə bilərsiniz. Məsələn, `/[\s\d]/` ifadəsi "istənilən boşluq simvolu və ya istənilən rəqəm" deməkdir.

**Praktik Nümunə:** Azərbaycan mobil nömrə formatının yoxlanılması.
`+994 (50) 123-45-67`

```javascript
// \d{2} -> 2 ədəd rəqəm deməkdir (növbəti dərsin mövzusu)
const numberPattern = /\+994\s\(\d{2}\)\s\d{3}-\d{2}-\d{2}/;

numberPattern.test("+994 (55) 321-09-87"); // ✅ true
numberPattern.test("+994 55 321-09-87"); // ❌ false (mötərizələr yoxdur)
numberPattern.test("994 (55) 321-09-87"); // ❌ false (+ işarəsi yoxdur)
```

#### **4. İstisnalar və Xüsusi Hallar**

- **Defis (`-`) simvolu:** Əgər qrup daxilində defis simvolunun özünü axtarmaq istəyirsinizsə, onu mütləq **ən sonda** yazın. Məsələn, `/\[a-z-]/` ifadəsi kiçik hərfləri və ya defis simvolunu axtarır.
- **Backspace (`\b`):** `\b` qısayolu normalda "söz sərhədi" (word boundary) deməkdir. Lakin mötərizə içində `[\b]` şəklində yazılarsa, o, hərfi mənada "backspace" simvolunu ifadə edən yeganə yoldur.

---

### **(Bonus) Unicode Simvol Qrupları: `\p{...}`**

Standart qısayollar (`\w`, `\d`) əsasən ASCII (latın əlifbası və ingilis dilinə aid) simvolları tanıyır. Məsələn, `\w` Azərbaycan əlifbasındakı `ə`, `ş`, `ü` kimi hərfləri tanımır.

Bu problemi həll etmək üçün müasir JavaScript (ES2018+) `u` (unicode) fleqi ilə birlikdə `\p{...}` sintaksisini dəstəkləyir.

`\p{...}` müəyyən Unicode xüsusiyyətinə malik bütün simvolları tapmağa imkan verir.

- `/\p{Script=Cyrillic}/u` — Kiril əlifbasına aid istənilən hərfi tapır.
- `/\p{Script=Arabic}/u` — Ərəb əlifbasına aid istənilən hərfi tapır.
- `/\p{Decimal_Number}/u` — Dünyanın istənilən yazısındakı rəqəmləri tapır (`\d`-dən fərqli olaraq).
- `/\p{Alphabetic}/u` — İstənilən əlifbaya aid olan bütün hərfləri tapır (o cümlədən `ə`, `ş`, `ü`).

İnkar forması üçün böyük `\P{...}` istifadə olunur.

```javascript
let pattern = /\p{Alphabetic}/u;

pattern.test("salam"); // ✅ true
pattern.test("şüşə"); // ✅ true (`\w` ilə false olardı)
pattern.test("123"); // ❌ false
```

---

### **Fəsil 11.3.4: Təkrarlayıcılar (Repetition)**

Əvvəlki dərslərdə öyrəndiyimiz sintaksislə iki rəqəmli ədədi `/\d\d/` və ya dörd rəqəmli ədədi `/\d\d\d\d/` kimi ifadə edə bilərdik. Bəs istənilən sayda rəqəmdən ibarət bir ədədi və ya üç hərfdən sonra gələn, ancaq mütləq olmayan (könüllü) bir rəqəmi necə təsvir edə bilərik?

Bu cür daha mürəkkəb nümunələri qurmaq üçün requlyar ifadələrin **təkrarlanma** sintaksisindən istifadə edirik. Bu operatorlar, onlardan əvvəl gələn simvolun, qrupun və ya sinfin neçə dəfə təkrarlana biləcəyini göstərir.

Təkrarlama operatorları həmişə tətbiq olunduğu nümunədən **sonra** gəlir.

#### **Təkrarlama Operatorları**

Aşağıdakı cədvəldə təkrarlama operatorları və onların mənaları verilmişdir.

| Operator    | Mənası                                                                           |
| :---------- | :------------------------------------------------------------------------------- |
| **`{n}`**   | Əvvəlki elementdən **tam olaraq `n` dəfə** olmalıdır.                            |
| **`{n,m}`** | Əvvəlki elementdən **minimum `n`**, **maksimum `m` dəfə** olmalıdır.             |
| **`{n,}`**  | Əvvəlki elementdən **minimum `n` dəfə** (və daha çox) olmalıdır.                 |
| **`+`**     | Əvvəlki elementdən **bir və ya daha çox dəfə** olmalıdır. Ekvivalenti: `{1,}`.   |
| **`*`**     | Əvvəlki elementdən **sıfır və ya daha çox dəfə** olmalıdır. Ekvivalenti: `{0,}`. |
| **`?`**     | Əvvəlki element **könüllüdür** (sıfır və ya bir dəfə). Ekvivalenti: `{0,1}`.     |

#### **Praktik Nümunələr**

**1. `{n}` — Dəqiq sayla təkrarlanma**
4 rəqəmli PİN kod axtarışı:

```javascript
let pattern = /^\d{4}$/; // Sətrin başdan sona yalnız 4 rəqəmdən ibarət olmasını yoxlayır

pattern.test("1234"); // ✅ true
pattern.test("123"); // ❌ false (3 rəqəmdir)
pattern.test("12345"); // ❌ false (5 rəqəmdir)
pattern.test("12a4"); // ❌ false (hərf var)
```

**2. `{n,m}` — Aralıqla təkrarlanma**
5-10 simvoldan ibarət istifadəçi adı (username) axtarışı:

```javascript
let pattern = /^\w{5,10}$/; // Yalnız hərf, rəqəm və alt xətdən ibarət, 5-10 simvol uzunluğunda

pattern.test("user123"); // ✅ true (7 simvol)
pattern.test("user"); // ❌ false (4 simvol)
pattern.test("user_long_name"); // ❌ false (14 simvol)
```

**3. `+` — Bir və ya daha çox**
"Java" sözünün əvvəlində və sonunda ən az bir boşluq olan sətirləri tapmaq:

```javascript
let pattern = /\s+java\s+/;

pattern.test("  java  "); // ✅ true
pattern.test(" java "); // ✅ true
pattern.test("java"); // ❌ false (boşluq yoxdur)
```

**4. `?` — Sıfır və ya bir dəfə (Könüllü)**
Həm "color" (ABŞ), həm də "colour" (BK) sözlərini tapmaq:

```javascript
let pattern = /colou?r/; // "u" hərfi ya heç olmamalı, ya da bir dəfə olmalıdır

pattern.test("color"); // ✅ true
pattern.test("colour"); // ✅ true
pattern.test("colouur"); // ❌ false ("u" iki dəfədir)
```

**5. `*` — Sıfır və ya daha çox**
Açıq mötərizə `(` olmayan istənilən sayda simvolu tapmaq:

```javascript
let pattern = /[^(]*/;

pattern.test("salam necesen?"); // ✅ true
pattern.test("bu (mesaj) gizlidir"); // `(` simvoluna qədər olan "bu " hissəsini tapacaq
```

---

### **Diqqət Edilməli Vacib Məqamlar**

**1. Təkrarlayıcı Nəyə Tətbiq Olunur?**
Təkrarlayıcılar yalnız onlardan **dərhal əvvəl gələn tək bir elementə** (simvol, simvol qrupu və ya qısayol) təsir edir.

- `/\d{3}/` — Üç ədəd rəqəm deməkdir (`\d\d\d`).
- `/ab{3}/` — "a" hərfi və ondan sonra gələn üç ədəd "b" hərfi deməkdir (`abbb`). Bu ifadə "ababab" demək DEYİL.

Əgər bir neçə simvoldan ibarət bir qrupu təkrarlamaq istəyirsinizsə, onları mötərizə `()` içinə almalısınız. Bu, növbəti dərsin mövzusudur. Məsələn, `/(ab){3}/` ifadəsi "ababab" deməkdir.

**2. `*` və `?` Təhlükəsi: Heç Nəyə Uyğunlaşma**
`*` (sıfır və ya daha çox) və `?` (sıfır və ya bir) operatorları **heç nəyə** də uyğun gələ bilər. Bu, bəzən gözlənilməz nəticələrə yol aça bilər.

Məsələn, `/a*/` ifadəsini götürək. Bu ifadə "sıfır və ya daha çox 'a' hərfi" deməkdir.

```javascript
let pattern = /a*/;

pattern.test("aaaa"); // ✅ true (4 'a' var)
pattern.test("b"); // ✅ true (Çünki "b" sətrinin əvvəlində SIFIR sayda 'a' var)
```

Bu ikinci nəticə çaşdırıcı görünə bilər. Requlyar ifadə mühərriki sətrin əvvəlinə baxır və soruşur: "Burada sıfır və ya daha çox 'a' varmı?". Cavab "Bəli, burada sıfır 'a' var" olduğu üçün nəticə `true` olur. Buna görə də bu operatorları istifadə edərkən diqqətli olmaq lazımdır.

---

### **Fəsil 11.3.5: Acgöz Olmayan (Tənbəl) Təkrarlayıcılar**

Əvvəlki fəsildə öyrəndiyimiz təkrarlayıcılar (`*`, `+`, `{n,m}`) standart olaraq **"acgöz" (greedy)** rejimdə işləyir. Bu o deməkdir ki, onlar requlyar ifadənin qalan hissəsinin də uyğun gəlməsinə imkan verəcək şəkildə **mümkün olan ən uzun** simvol ardıcıllığını ələ keçirməyə çalışırlar.

Lakin bəzən bizə bunun əksi lazımdır: mümkün olan **ən qısa** uyğunluğu tapmaq. Bu davranış **"acgöz olmayan"** və ya **"tənbəl" (lazy/non-greedy)** rejim adlanır.

Təkrarlayıcını "tənbəl" etmək çox asandır: sadəcə ondan sonra bir sual işarəsi `?` əlavə edin:

- `*` (acgöz) → `*?` (tənbəl)
- `+` (acgöz) → `+?` (tənbəl)
- `??` (acgöz) → `??` (tənbəl)
- `{n,m}` (acgöz) → `{n,m}?` (tənbəl)

#### **Acgöz və Tənbəl Rejimin Fərqi**

Bu fərqi anlamaq üçün ən yaxşı nümunə HTML teqləri arasındakı mətni tapmaqdır. Tutaq ki, belə bir sətrimiz var:

`let text = "<b>JavaScript</b> və <b>CSS</b>";`

Bizim məqsədimiz `<b>` teqləri arasındakı məzmunu tapmaqdır.

**1. Acgöz `.*` ilə Yanaşma (Yanlış Nəticə)**

```javascript
let greedyPattern = /<b>.*<\/b>/;
console.log(text.match(greedyPattern)[0]);
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

console.log(filePattern.test("script.js")); // ✅ true
console.log(filePattern.test("style.css")); // ✅ true
console.log(filePattern.test("archive.zip")); // ❌ false
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
console.log(pattern.test("Java")); // ✅ true
```

**2. Nümunəni Yadda Saxlamaq (Capturing):** Mötərizələr, uyğun gəldikləri mətni "yadda saxlayır" və bu hissələri sonradan əldə etmək mümkün olur.

```javascript
// Məqsəd: istifadəçinin adını və domenini ayrı-ayrı əldə etmək
const email = "user@example.com";
const pattern = /(\w+)@(\w+\.\w+)/;
const match = email.match(pattern);

if (match) {
  console.log("Tam uyğunluq:", match[0]); // user@example.com
  console.log("Birinci qrup (ad):", match[1]); // user
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
console.log(quotePattern.test(`'səhv"`)); // ❌ false (dırnaqlar fərqlidir)
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
  console.log("Birinci qrup (ay):", match[1]); // 06 (il yadda saxlanmadığı üçün bu \1 oldu)
  console.log("İkinci qrup (gün):", match[2]); // 12
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
  console.log(match.groups.city); // "Bakı"
  console.log(match.groups.country); // "Azərbaycan"
}
```

Adlandırılmış qruplara istinad `\k<ad>` ilə edilir:

```javascript
const quotePattern = /(?<quote>['"])(.*?)\k<quote>/;

console.log(quotePattern.test(`'düzgündür'`)); // ✅ true
console.log(quotePattern.test(`"səhvdir'`)); // ❌ false
```

---

### (SPECIFYING MATCH POSITION) Uyğunluğun Mövqeyini Bildirən Operatorlar

Bəzi operatorlar simvolları yox, onların arasındakı **mövqeləri** yoxlayır. Bunlara "lövbər" (anchor) deyilir, çünki nümunəni sətrin (string) müəyyən bir yerinə "bağlayırlar".

#### 1. Lövbərlər (Anchors): `^`, `$`, `\b`, `\B`

- **`^`** — Sətrin (string) **əvvəlini** bildirir.
- **`$`** — Sətrin (string) **sonunu** bildirir.
- **`\b`** — **Söz sərhədini (word boundary)** bildirir. Söz simvolu (`\w`) ilə qeyri-söz simvolu (`\W`) arasındakı mövqedir.
- **`\B`** — Söz sərhədi (word boundary) **olmayan** mövqeni bildirir.

```javascript
// Yalnız "salam" sözündən ibarət sətrə uyğun gəlir
console.log(/^salam$/.test("salam")); // ✅ true
console.log(/^salam$/.test("çox salam")); // ❌ false

// "kod" sözünü ayrıca bir söz kimi tapır
const wordPattern = /\bkod\b/;
console.log(wordPattern.test("kod yazmaq")); // ✅ true
console.log(wordPattern.test("dekodlaşdırmaq")); // ❌ false ('kod' sözün içindədir)

// "az" hissəsinin sözün içində olmasını tələb edir
const innerPattern = /\Baz\B/;
console.log(innerPattern.test("bazar")); // ✅ true
console.log(innerPattern.test("azad")); // ❌ false ('az' sözün başındadır)
```

#### 2. İrəli Baxış (Lookahead): `(?=...)` və `(?!...)`

Bunlar mövqeni yoxlayır, amma yoxladıqları simvolları uyğunluğun nəticəsinə daxil etmir ("baxır, amma götürmür").

- **`(?=p)` (Positive Lookahead):** Əgər `p` nümunəsi gəlirsə, uyğunluğu tap.
- **(?!p) (Negative Lookahead):** Əgər `p` nümunəsi gəlmirsə, uyğunluğu tap.

```javascript
// Yalnız "$" işarəsindən əvvəl gələn rəqəmləri tapır
const pricePattern = /\d+(?=\$)/;
console.log("Qiymət: 50$".match(pricePattern)); // ✅ Nəticə: ["50"] ('$' daxil deyil)

// "Script" sözü ilə davam etməyən "Java" sözünü tapır
const javaPattern = /Java(?!Script)/;
console.log(javaPattern.test("JavaBeans")); // ✅ true
console.log(javaPattern.test("JavaScript")); // ❌ false
```

#### 3. Geri Baxış (Lookbehind): `(?<=...)` və `(?<!...)`

İrəli baxış (lookahead) kimidir, ancaq mövqenin **əvvəlində** nə olduğunu yoxlayır. (ES2018+).

- **`(?<=p)` (Positive Lookbehind):** Əgər mövqedən əvvəl `p` varsa, uyğunluğu tap.
- **`(?<!p)` (Negative Lookbehind):** Əgər mövqedən əvvəl `p` yoxdursa, uyğunluğu tap.

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

- `g` (global) — Yalnız ilkini deyil, **bütün uyğunluqları** tapır.
- `i` (case-insensitive) — Böyük-kiçik hərf fərqini aradan qaldırır.
- `m` (multiline) — `^` və `$` həm də hər sətrin əvvəli və sonu üçün işləyir.
- `s` (dotall) — `.` simvolunun yeni sətir (`\n`) simvolunu da əhatə etməsinə imkan verir.
- `u` (unicode) — Emoji kimi çoxbaytlı simvolların düzgün işlənməsini təmin edir.
- `y` (sticky) — Yalnız son uyğunluğun bitdiyi mövqedən axtarışa başlayır.

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

---

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
  console.log("Tam uyğunluq:", result[0]); // ftp://files.example.com/docs/report.pdf
  console.log("Protokol (qrup 1):", result[1]); // ftp
  console.log("Host (qrup 2):", result[2]); // files.example.com
  console.log("Yol (qrup 3):", result[3]); // docs/report.pdf
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
  console.log(
    `Açar: ${match.groups.key}, Dəyər: ${match.groups.value}, İndeks: ${match.index}`
  );
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

---

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
    console.log(
      `'${searchTerm}' sözü ${matches.length} dəfə tapıldı:`,
      matches
    );
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
  console.log(
    `Tapıldı: ${match[0]}, Rəng kodu: ${match[1]}, İndeks: ${match.index}`
  );
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
  console.log(
    `'a' tapıldı, indeks: ${match.index}. Növbəti axtarış ${pattern.lastIndex}-dən başlayacaq.`
  );
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

- **`.source`**: Requlyar ifadənin `/.../` arasındakı mətnini qaytarır.
- **`.flags`**: Fleqləri bir sətir (string) olaraq qaytarır.
- **`.global`**, **`.ignoreCase`**, **`.multiline`** və s.: Müvafiq fleqin təyin edilib-edilmədiyini göstərən `true`/`false` dəyərlər.

```javascript
const myPattern = /\w+/gi;

console.log("Mənbə (source):", myPattern.source); // ✅ Nəticə: \w+
console.log("Fleqlər (flags):", myPattern.flags); // ✅ Nəticə: gi
console.log("Qlobaldırmı? (global):", myPattern.global); // ✅ Nəticə: true
console.log("Hərfə həssasdırmı? (ignoreCase):", myPattern.ignoreCase); // ✅ Nəticə: true
```

---

### 11.4 Tarix (Date) və Zaman (Time)

JavaScript-də tarix və zamanla işləmək üçün `Date` sinifi (class) istifadə olunur. `Date` obyekti `new Date()` konstruktoru (constructor) ilə yaradılır.

**Obyekt Yaratma Nümunələri:**

**1. Arqumentsiz - Hazırkı vaxt**

```javascript
const now = new Date();
console.log("Hazırkı vaxt:", now.toString());
// Fri Jun 13 2025 19:26:51 GMT+0400 (Azerbaijan Standard Time)
```

**2. Rəqəmlərlə - Detallı tarix**  
Konstruktora 2 və ya daha çox rəqəm verdikdə, onlar yerli (local) vaxt qurşağına görə İL, AY, GÜN, SAAT... kimi qəbul edilir.

```javascript
// ❗️ Diqqət: Aylar 0-dan başlayır (0 = Yanvar, 5 = İyun)!
// Günlər isə 1-dən başlayır.

// 13 İyun 2025, saat 19:30 (Bakı vaxtı ilə)
const someDay = new Date(2025, 5, 13, 19, 30, 0);
console.log("Müəyyən edilmiş tarix:", someDay.toLocaleString("az-AZ"));
// ✅ Nəticə: 13.06.2025, 19:30:00
```

**3. string ilə**  
`new Date()` standart formatda olan stringləri də emal edə bilir. Ən çox istifadə olunan format ISO 8601 formatıdır.

```javascript
// 'Z' hərfi vaxtın UTC olduğunu bildirir
const isoDate = new Date("2025-01-20T10:00:00Z");
console.log(
  "ISO formatlı tarix (Bakı vaxtı ilə):",
  isoDate.toLocaleString("az-AZ", { timeZone: "Asia/Baku" })
);
// ISO formatlı tarix (Bakı vaxtı ilə): 2025-01-20 14:00:00
```

---

### 11.4.1 Zaman Damğası (Timestamp) ⏱

Hər bir `Date` obyekti arxa planda sadəcə bir rəqəm saxlayır: 1 Yanvar 1970-ci il saat 00:00:00-dan (UTC vaxtı ilə) keçən millisaniyələrin sayı. Buna **zaman damğası (timestamp)** deyilir.

- `.getTime()`: Obyektin zaman damğasını (timestamp) qaytarır.
- `.setTime()`: Obyektin zaman damğasını (timestamp) dəyişir.
- `Date.now()`: Hal-hazırkı zaman damğasını (timestamp) birbaşa qaytarır.

**Nümunə: Kodun işləmə müddətini ölçmək** 🚀

```javascript
console.log("Əməliyyat başlayır...");

// Əməliyyat başlayır...
const startTime = Date.now();
const endTime = Date.now();
const duration = endTime - startTime;

console.log(`✅ Əməliyyat ${duration} ms (millisaniyə) çəkdi.`);
// ✅ Əməliyyat 51 ms (millisaniyə) çəkdi.
```

---

### 11.4.2 Tarix üzərində Hesablamalar (Date Arithmetic)

`Date` obyektləri ilə riyazi əməliyyatlar aparmaq mümkündür.

**Tarix Hissələrini Almaq və Dəyişmək (Getters & Setters)**  
Hesablama aparmaq üçün əvvəlcə tarixin hissələrini almalı (`get`) və ya dəyişməliyik (`set`).

- `getFullYear()` / `setFullYear()`
- `getMonth()` / `setMonth()`
- `getDate()` / `setDate()` (Ayın gününü verir)
- `getDay()` (Həftənin gününü verir: 0 = Bazar, 1 = Bazar ertəsi...)

**Nümunə 1: İki tarix arasındakı fərqi tapmaq**

```javascript
const newYear = new Date("2026-01-01");
const today = new Date("2025-06-13");

const diffMilliseconds = newYear - today;
// Fərqi günə çeviririk (1000ms * 60s * 60m * 24h)

const diffDays = Math.floor(diffMilliseconds / (1000 * 60 * 60 * 24));

console.log(`💡 Yeni ilə ${diffDays} gün qalıb.`);
// ✅ Nəticə: Yeni ilə 202 gün qalıb.
```

**Nümunə 2: Tarixə vaxt əlavə etmək (overflow nümunəsi)**
`set` metodları "daşma" (overflow) problemini avtomatik həll edir. Məsələn, aya 12-dən böyük dəyər verdikdə, il avtomatik artır.

```javascript
const d = new Date(2025, 10, 20); // 20 Noyabr 2025
console.log("Başlanğıc tarixi:", d.toLocaleDateString("az-AZ"));
// Başlanğıc tarixi: 2025-11-20

// 20 Noyabrın üstünə 2 ay gəlirik. Nəticədə 20 Yanvar 2026 alınır.
d.setMonth(d.getMonth() + 2);

console.log("Nəticə:", d.toLocaleDateString("az-AZ"));
// ✅ Nəticə: "20.01.2026"
```

---

### 11.4.3 Tarixin Formatlanması və Emalı (Formatting and Parsing) 

`Date` obyektini istifadəçiyə göstərmək üçün onu sətirə (string) çevirmək lazımdır.

**Nümunə: Müxtəlif formatlama metodları**

```javascript
const d = new Date("2025-10-26T20:30:00");

console.log("toString():", d.toString()); 
// toString(): Sun Oct 26 2025 20:30:00 GMT+0400 (Azerbaijan Standard Time)

console.log("toUTCString():", d.toUTCString()); 
// toUTCString(): Sun, 26 Oct 2025 16:30:00 GMT

console.log("toISOString():", d.toISOString()); 
// toISOString(): 2025-10-26T16:30:00.000Z

console.log("toLocaleDateString('az-AZ'):", d.toLocaleDateString("az-AZ")); 
// toLocaleDateString('az-AZ'): 26.10.2025

console.log("toLocaleTimeString('az-AZ'):", d.toLocaleTimeString("az-AZ")); 
// toLocaleTimeString('az-AZ'): 20:30:00

console.log("toDateString():", d.toDateString()); 
// toDateString(): Sun Oct 26 2025

```

**`Date.parse()`:** String formatında olan tarixi analiz edərək onun (timestamp) qaytarır. Bu, formatlamanın əksidir.

```javascript
const dateString = "2025-01-01T00:00:00Z";
const timestamp = Date.parse(dateString);

console.log("Alınan timestamp:", timestamp); // ✅ Nəticə: 1735689600000

// Həmin timestamp-dən yeni Date obyekti yaratmaq
const newDate = new Date(timestamp);
console.log(newDate.toUTCString()); // ✅ Nəticə: "Wed, 01 Jan 2025 00:00:00 GMT"
```

---

### 11.5 Xəta Sinifləri (Error Classes)

JavaScript-də `throw` və `catch` istənilən dəyəri xəta kimi istifadə edə bilsə də, ən yaxşı təcrübə (best practice) `Error` sinifindən (class) və ya onun alt-siniflərindən (subclasses) istifadə etməkdir.

`Error` obyektinin 3 əsas xüsusiyyəti  var:

- **`.name`**: Xətanın növü (məsələn, "Error", "TypeError").
- **`.message`**: Konstruktora (constructor) ötürülən xəta mesajı.
- **`.stack`**: Xətanın baş verdiyi yerə qədər olan bütün funksiya çağırışlarını göstərən uzun sətir (string).

**Nümunə: `try...catch` bloku ilə xətanı tutmaq**

```javascript
function checkAge(age) {
  if (typeof age !== "number" || age < 18) {
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

// Output:
/*
❗️ Bir xəta baş verdi!
Xətanın adı (name):Error
Xətanın mesajı (message):
İstifadəçi 18 yaşından kiçikdir və ya yaşı rəqəm deyil.

-------------------

Stek izi (stack trace):

Error: İstifadəçi 18 yaşından kiçikdir və ya yaşı rəqəm deyil. at checkAge (<anonymous>:10:11) at <anonymous>:16:17 at mn (<anonymous>:16:5455) at Object.playcodeUpdateWindow (<anonymous>:25:438) at <anonymous>:25:1688 at <anonymous>:12:3313 at Array.forEach (<anonymous>) at e._handleSignal (<anonymous>:12:3286) at e.dispatch (<anonymous>:12:1650) at t (<anonymous>:15:1493)
*/
```

----
##### JavaScript-in Standart Xəta Növləri

JavaScript proqramlaşdırma dilində bəzi **hazır (built-in)** xəta sinifləri mövcuddur. Bu siniflər `Error` sinifindən törəyir və proqramda müəyyən növ səhvlər baş verdikdə avtomatik yaradılır.

### 1. `TypeError` – Tip xətası

Dəyər üzərində onun tipinə uyğun olmayan bir əməliyyat aparıldıqda baş verir.

**Nümunə:**

```js
(10).toUpperCase(); 
// TypeError: (10).toUpperCase is not a function
```

`toUpperCase()` metodunu yalnız `string` tiplər üçün istifadə etmək olar.

---

### 2. `ReferenceError` – İstinad xətası

Təyin olunmamış bir dəyişənə müraciət edildikdə baş verir.

**Nümunə:**

```js
console.log(x); 
// ReferenceError: x is not defined
```

---

### 3. `SyntaxError` – Sintaksis xətası

JavaScript kodunun düzgün sintaksisə uyğun yazılmadığı hallarda yaranır.

**Nümunə:**

```js
let x = ;
// SyntaxError: Unexpected token ';'
```

---

### 4. `RangeError` – Aralıq xətası

Bir funksiyaya və ya əməliyyata icazə verilən aralıqdan kənar dəyər verildikdə yaranır.

**Nümunə:**

```js
let arr = new Array(-5);
// RangeError: Invalid array length
```

---

### 5. `URIError` – URI kodlaşdırma xətası

`decodeURI` və `encodeURI` funksiyalarında yanlış simvollar istifadə olunduqda baş verir.

**Nümunə:**

```js
decodeURI('%');
// URIError: URI malformed
```
---

Bu standart xətalar JavaScript tərəfindən avtomatik yaradılır. Əgər proqramın məntiqi tələb edirsə, biz öz **xüsusi (fərdi) xəta siniflərimizi** də yarada bilərik.

----
**Fərdi Xəta Sinifləri Yaratmaq (Custom Error Classes)**
Bəzən proqramın məntiqinə uyğun xüsusi xəta sinifləri yaratmaq daha faydalı olur. Məsələn, istifadəçi sistemi qeyri-qanuni şəkildə istifadə edərsə, biz `UnauthorizedError` adında bir xəta yarada bilərik. Bu, istənilən `Error` deyil, **giriş icazəsi ilə bağlı konkret xəta** olduğunu açıq şəkildə göstərir.

**Nümunə: `UnauthorizedError` sinifi**

```javascript
// Öz fərdi xəta sinifimizi yaradırıq
class UnauthorizedError extends Error {
  constructor(username) {
    super(`İstifadəçi icazəsiz giriş etməyə çalışdı: ${username}`);
    this.name = "UnauthorizedError";
    this.username = username;
    this.time = new Date().toISOString();
  }
}

function login(username, password) {
  const validUsers = {
    admin: "1234",
    user: "abcd",
  };

  if (!(username in validUsers) || validUsers[username] !== password) {
    throw new UnauthorizedError(username);
  }

  return `Xoş gəlmisiniz, ${username}!`;
}

try {
  const message = login("hacker", "wrongpass");
  console.log(message);
} catch (error) {
  console.error("🚨 Xəta baş verdi:", error.message);

  if (error instanceof UnauthorizedError) {
    console.error(`Xəta növü: ${error.name}`);
    console.error(`İstifadəçi adı: ${error.username}`);
    console.error(`Vaxt: ${error.time}`);
  } else {
    console.error("Naməlum xəta!");
  }
}
/* 
🚨 Xəta baş verdi: İstifadəçi icazəsiz giriş etməyə çalışdı: hacker
Xəta növü: UnauthorizedError
İstifadəçi adı: hacker
Vaxt: 2025-08-01T09:47:00.000Z 
*/
```

---

### 11.6 JSON Seriləşdirmə (Serialization) və Emal (Parsing) 🔄

**Seriləşdirmə (Serialization)** – proqramdakı obyekt, massiv (array) kimi data strukturlarını saxlamaq və ya şəbəkə (network) üzərindən ötürmək üçün sətirə (string) çevirmə prosesidir.

JavaScript-də bu proses üçün **JSON (JavaScript Object Notation)** formatı və iki əsas funksiya istifadə olunur:

- `JSON.stringify()`: Obyekti JSON formatlı string-ə çevirir.
- `JSON.parse()`: JSON formatlı string-i yenidən obyektə çevirir.

JSON əksər primitiv tipləri (string, number, boolean, null), massivləri (arrays) və obyektləri (objects) dəstəkləyir, ancaq `Date`, `Map`, `Set`, `RegExp` kimi mürəkkəb tipləri birbaşa dəstəkləmir.

**Nümunə 1: Əsas istifadə və formatlı çıxış**

```javascript
const user = {
  id: 101,
  username: "johndoe",
  isAdmin: false,
  roles: ["editor", "viewer"],
  profile: null,
};

// 1. Obyekti JSON stringinə çeviririk
const jsonString = JSON.stringify(user);
console.log("Normal JSON:", jsonString);
// {"id":101,"username":"johndoe","isAdmin":false,"roles":["editor","viewer"],"profile":null}

// 2. İnsanların oxuması üçün formatlı JSON yaradırıq
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


## 11.6.1 Fərdi Dəyişikliklər (JSON Customizations)

JavaScript-də obyektləri JSON formatına çevirən və əksinə çevirən metodlar – `JSON.stringify` və `JSON.parse` – öz davranışlarını dəyişmək üçün **əlavə imkanlar** təqdim edir.

Bu imkanlardan istifadə edərək:

* Bəzi dəyərləri gizlədə,
* Yalnız seçilmiş xüsusiyyətləri saxlaya,
* Tarixləri `Date` obyektinə geri çevirə bilərik.

---

### 1. `toJSON()` metodu ilə fərdi çevirmə

Əgər bir obyektin daxilində `toJSON()` metodu varsa, `JSON.stringify()` həmin obyektin yerinə bu metodun qaytardığı dəyəri seriləşdirir.

**Məsələn: `Date` obyektləri**

```js
const d = new Date("2025-06-13T15:28:51.520Z");
console.log(d.toJSON()); 
// "2025-06-13T15:28:51.520Z"
```

---

### 2. `JSON.parse()` ilə reviver funksiyası

`JSON.parse()` metodunun ikinci arqumenti **reviver** adlanır. Bu funksiya hər bir `key:value` cütü üçün çağırılır və dəyərləri istədiyimiz formada **dəyişməyə və ya canlandırmağa** imkan verir.

#### Nümunə: `Date` obyektini seriləşdirib geri qaytarmaq

```js
const event = {
  title: "JavaScript Konfransı",
  date: new Date()
};

// JSON formatına çeviririk (stringify)
const eventString = JSON.stringify(event);
console.log("Seriləşdirilmiş:", eventString);
// {"title":"JavaScript Konfransı","date":"2025-06-13T15:28:51.520Z"}

// JSON-u geri obyektə çeviririk (parse) və tarix dəyərini canlandırırıq
const revived = JSON.parse(eventString, (key, value) => {
  if (key === "date" && typeof value === "string") {
    return new Date(value); // String olan tarixi yenidən Date obyektinə çevir
  }
  return value;
});

console.log("Canlandırılmış obyekt:", revived);
/* Canlandırılmış obyekt: 
{
    "title": "JavaScript Konfransı",
    "date": "2025-08-01T10:04:59.377Z"
}
*/
console.log("Tarix tipi:", revived.date instanceof Date); // true
```

---

### 3. `JSON.stringify()` ilə replacer funksiyası

`JSON.stringify()` metodunun ikinci arqumenti **replacer** funksiyası və ya **massiv** ola bilər.

#### a) Replacer kimi massiv – yalnız seçilmiş xüsusiyyətlər:

```js
const product = {
  id: 123,
  name: "Laptop",
  price: 2500,
  internalCode: "XYZ-987-A",
  specs: { cpu: "i7", ram: 16 },
};

const publicView = JSON.stringify(product, ["name", "price", "specs"], 2);
console.log("Yalnız seçilən xüsusiyyətlər:\n", publicView);
```

**Nəticə:**

```json
{
  "name": "Laptop",
  "price": 2500,
  "specs": {
    "cpu": "i7",
    "ram": 16
  }
}
```

---

#### b) Replacer kimi funksiya – istədiyin sahəni gizlət:

```js
const filtered = JSON.stringify(product, (key, value) => {
  if (key === "internalCode") return undefined;
  return value;
}, 2);

console.log("Gizlədilmiş versiya:\n", filtered);
```

---

### 11.7 Beynəlxalqlaşdırma API-ı (The Internationalization API)

JavaScript-in beynəlxalqlaşdırma (internationalization) API-ı, proqramları fərqli dillər, regionlar və mədəniyyətlər üçün uyğunlaşdırmağa kömək edən bir alətlər toplusudur. Bu API 3 əsas sinifdən  ibarətdir:

- **`Intl.NumberFormat`**: Rəqəmləri, valyutaları və faizləri formatlamaq üçün.
- **`Intl.DateTimeFormat`**: Tarix və zamanı formatlamaq üçün.
- **`Intl.Collator`**: Sətirləri (strings) yerli əlifba qaydalarına görə müqayisə etmək və sıralamaq (sort) üçün.

---

### 11.7.1 Rəqəmlərin Formatlanması (Formatting Numbers)

Fərqli ölkələrdə insanlar rəqəmləri fərqli yazır. Məsələn, bəziləri kəsr hissəni ayırmaq üçün nöqtə (`.`), bəziləri isə vergül (`,`) istifadə edir. Minlik ayırıcıları da fərqli ola bilir. `Intl.NumberFormat` sinifi bu məsələni bizim üçün həll edir.

Onu `new Intl.NumberFormat(locale, options)` konstruktoru ilə yaradırıq:

- **`locale`**: Formatlamanın hansı dil və region üçün ediləcəyini göstərən sətir (string). Məsələn, `"az-AZ"` (Azərbaycan), `"en-US"` (ABŞ İngiliscəsi), `"fr-FR"` (Fransa Fransızcası). Boş buraxılarsa, sistemin lokalını götürür.
- **`options`**: Formatlamanın detallarını bildirən bir obyekt (object).

**Nümunə 1: Sadə rəqəm formatlanması (fərqli lokallar ilə)**
Görək eyni rəqəm fərqli ölkələrdə necə görünür.

```javascript
const number = 1234567.89;

// Azərbaycan lokalı (az-AZ)
const azFormatter = new Intl.NumberFormat("az-AZ");
console.log("Azərbaycan formatı:", azFormatter.format(number));
// ✅ Nəticə: 1 234 567,89

// ABŞ lokalı (en-US)
const usFormatter = new Intl.NumberFormat("en-US");
console.log("ABŞ formatı:", usFormatter.format(number));
// ✅ Nəticə: 1,234,567.89

// Almaniya lokalı (de-DE)
const deFormatter = new Intl.NumberFormat("de-DE");
console.log("Almaniya formatı:", deFormatter.format(number));
// ✅ Nəticə: 1.234.567,89
```

**Nümunə 2: Valyuta formatlanması (currency)**
Bu, ən çox istifadə olunan xüsusiyyətlərdən biridir. `style: 'currency'` və `currency: 'KOD'` opsiyalarını tələb edir.

```javascript
const amount = 1550.75;

// Azərbaycan Manatı (AZN)
const aznFormatter = new Intl.NumberFormat("az-AZ", {
  style: "currency",
  currency: "AZN",
});
console.log("Manatla:", aznFormatter.format(amount)); 
// ✅ Nəticə: 1.550,75 ₼

// ABŞ Dolları (USD)
const usdFormatter = new Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
});
console.log("Dollarla:", usdFormatter.format(amount)); 
// ✅ Nəticə: $1,550.75

// Avro (EUR) - valyutanın adını tam göstərmək
const eurFormatter = new Intl.NumberFormat("de-DE", {
  style: "currency",
  currency: "EUR",
  currencyDisplay: "name",
});
console.log("Avro ilə:", eurFormatter.format(amount)); 
// ✅ Nəticə: 1.550,75 Euro
```

**Nümunə 3: Faiz formatlanması**

```javascript
const value = 0.895;

const percentFormatter = new Intl.NumberFormat("az-AZ", {
  style: "percent",
  minimumFractionDigits: 1, // Kəsr hissədə minimum 1 rəqəm göstər
});

console.log(percentFormatter.format(value)); 
// ✅ Nəticə: 89,5 %
```

**Nümunə 4: Rəqəm hissələrinin idarə edilməsi**
Bəzən qiymətləri göstərərkən hər zaman iki kəsr rəqəmi olmasını istəyirik (məsələn, 15.00).

```javascript
const price = 1299;
const precisePrice = 49.956;
const simplePrice = 78.5;

const priceFormatter = new Intl.NumberFormat("az-AZ", {
  style: "currency",
  currency: "AZN",
  minimumFractionDigits: 2, 
  // Həmişə minimum 2 kəsr rəqəmi göstər
  maximumFractionDigits: 2, 
  // Maksimum 2 kəsr rəqəmi göstər (yuvarlaqlaşdırır)
});

console.log(priceFormatter.format(price)); 
// ✅ Nəticə: 1.299,00 ₼
console.log(priceFormatter.format(precisePrice)); 
// ✅ Nəticə: 49,96 ₼ (yuvarlaqlaşdırdı)
console.log(priceFormatter.format(simplePrice)); 
// ✅ Nəticə: 78,50 ₼ (sonuna sıfır əlavə etdi)
```

**Nümunə 5: Format metodunu birbaşa istifadə etmək**
Formatter obyektinin `.format` metodunu birbaşa bir dəyişənə mənimsədib, onu bir funksiya kimi istifadə edə bilərsiniz.

```javascript
const data = [100, 2500, 98765];

// Formatter yaradıb, onun format metodunu birbaşa bir dəyişənə veririk
const formatWithGrouping = new Intl.NumberFormat("az-AZ").format;

const formattedData = data.map(formatWithGrouping);
console.log(formattedData); 
// ✅ Nəticə: ["100", "2.500", "98.765"]
```

---

### 11.7.2 Tarix və Zamanın Formatlanması (Formatting Dates and Times) 

`Date` obyektinin `.toLocaleDateString()` kimi sadə metodları olsa da, onlar formatlama üzərində tam nəzarət imkanı vermir.

Bütün bu detallı nəzarəti **`Intl.DateTimeFormat`** sinfi təmin edir. O da `Intl.NumberFormat` kimi `new Intl.DateTimeFormat(locale, options)` konstruktoru ilə yaradılır və `.format()` metodu ilə istifadə olunur.

- **`locale`**: Formatlamanın hansı dil və region üçün ediləcəyini göstərir (məsələn, `"az-AZ"`).
- **`options`**: Tarix və zamanın hansı hissələrinin və necə göstəriləcəyini təyin edən bir obyekt (object).

**Əsas `options` Xüsusiyyətləri:**

- **`year`, `month`, `day`, `weekday`**: Tarix hissələri. Dəyərləri `"numeric"`, `"2-digit"`, `"long"` (uzun, məs. "İyun"), `"short"` (qısa, məs. "İyn") ola bilər.
- **`hour`, `minute`, `second`**: Zaman hissələri. Dəyərləri `"numeric"`, `"2-digit"` ola bilər.
- **`timeZone`**: Tarixi fərqli bir zaman qurşağı (timezone) üçün göstərməyə imkan verir (məsələn, `"America/New_York"`).
- **`hour12`**: `true` və ya `false` dəyəri ilə 12 saatlıq və ya 24 saatlıq formatı seçir.

---

**Nümunələr**

**Nümunə 1: Sadə və detallı formatların müqayisəsi**
Hazırkı vaxtı (`new Date()`) götürərək onu fərqli detallarla formatlayaq.

```javascript
const today = new Date(); 
// Hazırkı vaxt: 13 İyun 2025, 19:33

// 1. Standart, qısa format (Azərbaycan lokalı ilə)
const simpleFormatter = new Intl.DateTimeFormat("az-AZ");
console.log("Sadə format:", simpleFormatter.format(today));
// ✅ Nəticə: 13.06.2025

// 2. Bütün detalları özündə cəmləyən geniş format
const fullOptions = {
  weekday: "long",
  year: "numeric",
  month: "long",
  day: "numeric",
  hour: "2-digit",
  minute: "2-digit",
  second: "2-digit",
};
const fullFormatter = new Intl.DateTimeFormat("az-AZ", fullOptions);
console.log("Geniş format:", fullFormatter.format(today));
// ✅ Nəticə: Cümə, 13 iyun 2025, 19:33:03
```

**Nümunə 2: Eyni formatın fərqli lokallarda (locales) göstərilməsi**
Yuxarıdakı `fullOptions` obyektini fərqli dillər üçün istifadə edək.

```javascript
const date = new Date("2025-06-13T19:30:00");
const options = {
  weekday: "long",
  month: "long",
  day: "numeric",
  year: "numeric",
};

const az_format = new Intl.DateTimeFormat("az-AZ", options).format(date);
const en_format = new Intl.DateTimeFormat("en-US", options).format(date);
const ja_format = new Intl.DateTimeFormat("ja-JP", options).format(date);

console.log("🇦🇿 Azərbaycan:", az_format); 
// ✅ Nəticə: Cümə, 13 iyun 2025
console.log("🇺🇸 ABŞ:", en_format); 
// ✅ Nəticə: Friday, June 13, 2025
console.log("🇯🇵 Yaponiya:", ja_format); 
// ✅ Nəticə: 2025年6月13日金曜日
```

---
### 11.7.3 Stringlərin Müqayisəsi və Sıralanması (Comparing and Sorting Strings) 

Stringləri  əlifba sırasına görə sıralamaq (sort) düşündüyümüzdən daha mürəkkəb bir məsələdir. Məsələn, JavaScript-in standart `.sort()` metodu simvolları onların Unicode kodlarına görə sıralayır. Bu, bir çox dil üçün, o cümlədən Azərbaycan dili üçün düzgün nəticə vermir.

**Problem Nədir?** 🤔
Standart `.sort()` metodu Azərbaycan dilinin xüsusi hərflərini (`ç`, `ş`, `ğ`, `ə`, `ö`, `ü`) və onların əlifbadakı yerini tanımır.

```javascript
const words = ["şüşə", "səbət", "çanta", "can"];

// Standart sort metodu səhv nəticə verir
words.sort();
console.log("Səhv sıralama:", words);
// ❌ Nəticə : ["can", "səbət", "çanta", "şüşə"] 
// (çünki 's' hərfi 'ç'-dən əvvəl gəlir)
```

**Həll Yolu: `Intl.Collator`** ✅
`Intl.Collator` sinifi (class) stringləri müxtəlif dillərin və regionların qaydalarına uyğun olaraq müqayisə etmək və sıralamaq üçün yaradılıb. O, `.compare` adlı bir metod təqdim edir ki, bu metodu birbaşa `.sort()` metoduna ötürmək olar.

`new Intl.Collator(locale, options)` konstruktoru ilə yaradılır:

- **`locale`**: Müqayisənin hansı dilin qaydalarına görə aparılacağını bildirir (`"az-AZ"`).
- **`options`**: Müqayisənin davranışını tənzimləyən bir obyekt (object).

**Əsas `options` Xüsusiyyətləri:**

- **`usage`**: İstifadə məqsədi. `"sort"` (sıralama) və ya `"search"` (axtarış) ola bilər.
- **`sensitivity`**: Həssaslıq dərəcəsi.
  - `"base"`: Həm hərflərin böyük-kiçikliyinə, həm də aksentlərə məhəl qoymur (məsələn, `e` == `ə` == `E`).
  - `"accent"`: Aksentləri nəzərə alır, amma böyük-kiçikliyi yox (`e` != `ə`, amma `ə` == `Ə`).
  - `"case"`: Böyük-kiçikliyi nəzərə alır, amma aksentləri yox (`e` != `E`, amma `e` == `ə`).
  - `"variant"`: Hər şeyi nəzərə alır (tam həssaslıq). Bu, `"sort"` üçün standart dəyərdir.
- **`numeric`**: `true` olarsa, sətirlərin içindəki rəqəmləri ədədi dəyərlərinə görə sıralayır (məsələn, "Fəsil 2" "Fəsil 10"-dan əvvəl gəlir).
- **`ignorePunctuation`**: `true` olarsa, durğu işarələri və boşluqlar nəzərə alınmır.

---

**Geniş Nümunələr**

**Nümunə 1: Azərbaycan dili üçün düzgün əlifba sırası**
Yuxarıdakı səhv sıralama problemini `Intl.Collator` ilə həll edək.

```javascript
const words = ["şüşə", "səbət", "çanta", "can"];

// Azərbaycan lokalı üçün bir müqayisəçi (collator) yaradırıq
const azCollator = new Intl.Collator("az-AZ").compare;

// .sort() metoduna öz müqayisəçimizi ötürürük
words.sort(azCollator);

console.log("Düzgün sıralama:", words);
// ✅ Nəticə: ["can", "çanta", "səbət", "şüşə"]
```

**Nümunə 2: Rəqəmlərin nəzərə alınması (Numeric Sorting)** 🔢
Fayl adları və ya başlıqlar kimi rəqəmli sətirləri sıralamaq üçün `numeric: true` əvəzsizdir.

```javascript
const files = ["report-10.pdf", "report-2.pdf", "report-1.pdf"];

files.sort();
console.log("Səhv rəqəm sıralaması:", files);
// ["report-1.pdf", "report-10.pdf", "report-2.pdf"]

// Düzgün sıralama üçün `numeric: true` istifadə edirik
const numericCollator = new Intl.Collator(undefined, { numeric: true }).compare;
files.sort(numericCollator);
console.log("Düzgün rəqəm sıralaması:", files);
// ✅ Nəticə: ["report-1.pdf", "report-2.pdf", "report-10.pdf"]
```

---
### 11.8 Konsol (Console) API-ı 💻

`console` obyekti, JavaScript kodunu yazarkən və sazlayarkən (debug) ən yaxın dostumuzdur. O, sadəcə `console.log()`-dan ibarət deyil və bir çox faydalı metoda malikdir. Bu API standart olmasa da, bütün müasir brauzerlər və Node.js tərəfindən tam dəstəklənir.

---

**Log Səviyyələri (Log Levels): `log`, `info`, `warn`, `error`**
Bu metodlar mesajları fərqli vaciblik səviyyələrinə görə konsola çıxarmaq üçündür. Brauzerlər adətən hər birini fərqli ikon və rənglə göstərir.

```javascript
console.log("Bu, sadə bir log mesajıdır.");
console.info("💡 Bu, informativ bir mesajdır."); 
console.warn("⚠️ Bu, bir xəbərdarlıqdır."); 
console.error("❌ Bu isə bir xətadır."); 

// Bu, sadə bir log mesajıdır.
// 💡 Bu, informativ bir mesajdır.
// ⚠️ Bu, bir xəbərdarlıqdır.
// ❌ Bu isə bir xətadır.

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

**Cədvəl Formatı (Table Formatting): `table()`**
Massivləri (arrays of objects) və ya obyektləri oxunaqlı cədvəl formatında göstərmək üçün super bir vasitədir.

```javascript
const users = [
  { ad: "Elvin", soyad: "Əliyev", yas: 28 },
  { ad: "Ayan", soyad: "Məmmədova", yas: 25 },
  { ad: "Tural", soyad: "Hüseynov", yas: 32 },
];

console.log("İstifadəçilər cədvəl formatında:");
console.table(users);

/* İstifadəçilər cədvəl formatında:
┌─────────┬─────────┬──────────────┬─────┐
│ (index) │   ad    │    soyad     │ yas │
├─────────┼─────────┼──────────────┼─────┤
│    0    │ 'Elvin' │  'Əliyev'    │ 28  │
│    1    │ 'Ayan'  │ 'Məmmədova'  │ 25  │
│    2    │ 'Tural' │ 'Hüseynov'   │ 32  │
└─────────┴─────────┴──────────────┴─────┘
*/

console.log("Yalnız 'ad' və 'yas' sütunları ilə cədvəl:");
console.table(users, ["ad", "yas"]);

/*
Yalnız 'ad' və 'yas' sütunları ilə cədvəl:
┌─────────┬─────────┬─────┐
│ (index) │   ad    │ yas │
├─────────┼─────────┼─────┤
│    0    │ 'Elvin' │ 28  │
│    1    │ 'Ayan'  │ 25  │
│    2    │ 'Tural' │ 32  │
└─────────┴─────────┴─────┘
*/
```

---

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

/*
▼ İstifadəçi Məlumatları
   Ad: Elvin Əliyev
   Yaş: 28
   ▼ 📍 Ünvan
      Şəhər: Bakı
      Küçə: Nizami
*/

/*
▶ İstifadəçi Məlumatları
*/
```

---

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
//
// Cüt rəqəm: 1
// Tək rəqəm: 1
// Cüt rəqəm: 2
// Tək rəqəm: 2
// Cüt rəqəm: 3

console.countReset("Cüt rəqəm"); // "Cüt rəqəm" sayğacını sıfırlayır
console.count("Cüt rəqəm"); // ✅ Nəticə: Cüt rəqəm: 1
```

---

**Zamanlayıcı (Timer): `time()`, `timeLog()`, `timeEnd()`**
Müəyyən bir əməliyyatın nə qədər vaxt apardığını ölçmək üçün `Date.now()`-dan daha rahat bir üsuldur.

```javascript
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

### 11.8.1 Konsol ilə Formatlı Çıxış (Formatted Output)

`console.log` və bənzəri metodların ilk arqumenti C proqramlaşdırma dilindəki kimi formatlaşdırma təyin edə bilər. `%s`, `%d` kimi xüsusi işarələr sonrakı arqumentlərin dəyərləri ilə əvəz olunur.

- **`%s`**: Sətir (String)
- **`%d`** və **`%i`**: Tam rəqəm (Integer)
- **`%f`**: Həqiqi rəqəm (Floating-point)
- **`%o`** və **`%O`**: Obyekt (Object)
- **`%c`**: CSS stili (yalnız brauzerdə işləyir)

**Nümunə 1: Müxtəlif tiplərin formatlanması**

```javascript
const name = "Ayan";
const age = 25;
const balance = 152.75;
const user = { name: "Ayan", age: 25 };

console.log("İstifadəçi: %s, Yaş: %d, Balans: %f AZN", name, age, balance);
// İstifadəçi: Ayan, Yaş: 25, Balans: 152.75 AZN

console.log("Obyektin detallı görünüşü: %O", user);
/* 
{
    "name": "Ayan",
    "age": 25
}
*/
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
![[Pasted image 20250802171002.png]]

---

### 11.9 URL API-ları (URL APIs)

Veb proqramlaşdırmada URL-lərlə işləmək çox yayğın bir tələbatdır. JavaScript-də URL-ləri emal etmək, hissələrə ayırmaq (parse), dəyişdirmək və düzgün formatlamaq üçün müasir **`URL`** sinifi (class) mövcuddur.

`URL` sinifi standart ECMAScript-in bir hissəsi olmasa da, bütün müasir brauzerlər və Node.js tərəfindən tam dəstəklənir.

#### `URL` Obyektinin Yaradılması və Xüsusiyyətləri 

`new URL()` konstruktoru ilə bir URL obyekti yaradılır. Bu obyekt URL-i avtomatik olaraq hissələrə ayırır və bu hissələrə xüsusiyyətlər (properties) vasitəsilə asanlıqla müraciət etmək olur.

**Nümunə 1: URL-in hissələrə ayrılması**

```javascript
const myUrl =
  "https://www.example.com:8080/axtarish?q=JavaScript&lang=az#netice-1";
const url = new URL(myUrl);

console.log("Bütün URL (href):", url.href);
// "https://www.example.com:8080/axtarish?q=JavaScript&lang=az#netice-1"
console.log("Protokol (protocol):", url.protocol); // "https:"
console.log("Host (host):", url.host); // "www.example.com:8080"
console.log("Hostname (hostname):", url.hostname); // "www.example.com"
console.log("Port (port):", url.port); // "8080"
console.log("Yol (pathname):", url.pathname); // "/axtarish"
console.log("Axtarış hissəsi (search):", url.search); // "?q=JavaScript&lang=az"
console.log("Lövbər (hash):", url.hash); // "#netice-1"
console.log("Mənbə (origin):", url.origin); // "https://www.example.com:8080"
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
// https://mysite.com/m%C9%99hsullar/yeni%20m%C9%99hsul?filter=yeni%23
// Gördüyünüz kimi, 'ə', ' ' (boşluq) və '#' simvolları düzgün kodlaşdırıldı.
```

---
#### Axtarış Parametrləri (Search Params): `URLSearchParams`

URL-in `?`-dən sonrakı hissəsini (`q=test&cat=tech` kimi) idarə etmək üçün `search` xüsusiyyəti əvəzinə, daha güclü olan `searchParams`-dan istifadə etmək daha rahatdır. Bu, `URLSearchParams` adlı xüsusi bir obyekt qaytarır.

**`URLSearchParams` Nümunəsi:**

```javascript
const url = new URL("https://example.com/search");
console.log("Başlanğıc URL:", url.href);
// Başlanğıc URL: https://example.com/search

// 1. Parametrlər əlavə edirik: .append()
url.searchParams.append("q", "JavaScript");
url.searchParams.append("cat", "proqramlaşdırma");
console.log("Parametr əlavə edildi:", url.href); 
// Parametr əlavə edildi: 
// https://example.com/search?q=JavaScript&cat=proqramla%C5%9Fd%C4%B1rma

// 2. Parametrin dəyərini dəyişirik: .set()
url.searchParams.set("q", "ECMAScript"); 
// 'q'-nin köhnə dəyərini silib yenisini yazır
console.log("Parametr dəyişdirildi:", url.href); 
// Parametr dəyişdirildi: 
// https://example.com/search?q=ECMAScript&cat=proqramla%C5%9Fd%C4%B1rma

// 3. Eyni adlı ikinci bir parametr əlavə edirik
url.searchParams.append("cat", "veb");
console.log("Eyni adlı parametr əlavə edildi:", url.href); 
// Eyni adlı parametr əlavə edildi:
// https://example.com/search?q=ECMAScript&cat=proqramla%C5%9Fd%C4%B1rma&cat=veb

// 4. Parametrləri alırıq: .get() və .getAll()
console.log("`q` parametrinin dəyəri:", url.searchParams.get("q")); 
// ✅ "ECMAScript"
console.log(
  "`cat` parametrinin bütün dəyərləri:",
  url.searchParams.getAll("cat")
); // ✅ ["proqramlaşdırma", "veb"]

// 5. Parametri silirik: .delete()
url.searchParams.delete("cat");
console.log("Parametr silindi:", url.href); 
// ...?q=ECMAScript

// 6. Bütün parametrlər üzərində dövr etmək (loop)
for (const [key, value] of url.searchParams) {
  console.log(`Açar: ${key}, Dəyər: ${value}`);
}
// Açar: q, Dəyər: ECMAScript
// Açar: cat, Dəyər: proqramlaşdırma
```

---

### 11.10 Zamanlayıcılar (Timers) 

JavaScript-in ilk günlərindən bəri brauzerlər və Node.js, proqramlara müəyyən bir müddət sonra və ya periodik olaraq funksiya çağırmaq imkanı verən iki əsas funksiya təqdim edir: **`setTimeout()`** və **`setInterval()`**. Bu funksiyalar kodun dərhal yox, gələcəkdə icra olunmasını təmin edir və asinxron (asynchronous) proqramlaşdırmanın təməlini təşkil edir.

---

#### `setTimeout()` — Birdəfəlik Gecikmə

Bu funksiya, verilmiş bir funksiyanı (callback) müəyyən bir gecikmədən  sonra **yalnız bir dəfə** icra etmək üçün istifadə olunur.
`setTimeout(funksiya, gecikmə_ms)`

- **`funksiya`**: Gecikmədən sonra icra olunacaq funksiya.
- **`gecikmə_ms`**: Gecikmə müddəti (millisaniyə ilə). `1000ms = 1 saniyə`.

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

// 😌 Bomba zərərsizləşdirildi, partlayış olmayacaq.
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
  const timeString = now.toLocaleTimeString("az-AZ"); // Vaxtı Azərbaycan formatında götürürük
  console.log(`🕒 ${timeString}`);
}, 1000);

// 10 saniyə sonra `clearInterval` ilə saatı dayandırırıq
setTimeout(() => {
  clearInterval(clockIntervalId);
  console.log("Saat dayandırıldı.");
}, 10000);

// console.log("Rəqəmsal saat başladıldı... (10 saniyə sonra dayanacaq)");
// 🕒 17:30:53
// Saat dayandırıldı.
```