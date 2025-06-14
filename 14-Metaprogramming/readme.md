### Fəsil 14. Metaprogramlaşdırma (Metaprogramming) 🧠

Bu fəsil, gündəlik proqramlaşdırmada çox tez-tez istifadə olunmayan, lakin təkrar istifadə edilə bilən kitabxanalar (reusable libraries) yazan proqramçılar üçün çox dəyərli olan bir sıra qabaqcıl JavaScript xüsusiyyətlərini əhatə edir. Bu mövzular, həmçinin JavaScript obyektlərinin davranışının dərinliklərinə baş vurmaq istəyən hər kəs üçün maraqlı olacaq.

Burada təsvir edilən xüsusiyyətlərin bir çoxunu ümumi olaraq **"metaprogramlaşdırma"** adlandırmaq olar. Əgər adi proqramlaşdırma, datanı (data) idarə etmək üçün kod yazmaqdırsa, metaprogramlaşdırma, **başqa bir kodu idarə etmək üçün kod yazmaqdır**. JavaScript kimi dinamik bir dildə proqramlaşdırma ilə metaprogramlaşdırma arasındakı sərhədlər bir az bulanıqdır. Hətta bir obyektin xüsusiyyətləri üzərində `for...in` dövrü ilə iterasiya etmək kimi sadə bir imkan belə, daha statik dillərə öyrəşmiş proqramçılar tərəfindən "meta" hesab edilə bilər.

Bu fəsildə əhatə olunacaq metaprogramlaşdırma mövzuları bunlardır:
* **§14.1**: Obyekt xüsusiyyətlərinin sadalana bilməsini (enumerability), silinə bilməsini (deleteability) və konfiqurasiya edilməsini (configurability) idarə etmək.
* **§14.2**: Obyektlərin genişləndirilməsini (extensibility) idarə etmək və "möhürlənmiş" (sealed) və "dondurulmuş" (frozen) obyektlər yaratmaq.
* **§14.3**: Obyektlərin prototiplərini (prototypes) sorğulamaq və təyin etmək.
* **§14.4**: Tanınmış `Symbol`-lar ilə tiplərinizin davranışını incə tənzimləmək.
* **§14.5**: Şablon teq funksiyaları (template tag functions) ilə DSL-lər (domain-specific languages) yaratmaq.
* **§14.6**: `Reflect` metodları ilə obyektləri yoxlamaq.
* **§14.7**: `Proxy` ilə obyekt davranışını idarə etmək.

---
### 14.1 Obyekt Xüsusiyyətlərinin Atributları (Property Attributes) 🔧
JavaScript-də bir obyektin xüsusiyyətləri (properties) təkcə ad (name) və dəyərdən (value) ibarət deyil. Hər bir xüsusiyyətin, onun davranışını idarə edən 3 əsas **atributu (attribute)** var:

* **`writable` (yazıla bilən)** ✍️: `true` olarsa, xüsusiyyətin dəyərini dəyişmək olar.
* **`enumerable` (sadalana bilən)** 📜: `true` olarsa, xüsusiyyət `for...in` dövründə və `Object.keys()` metodunda görünür.
* **`configurable` (konfiqurasiya edilə bilən)** ⚙️: `true` olarsa, xüsusiyyəti silmək və onun digər atributlarını (məsələn, `writable` və `enumerable`-ı) dəyişmək olar.

Adi yollarla (`{}` və ya `obj.prop = value` ilə) yaradılan xüsusiyyətlərin bütün bu atributları standart olaraq `true` olur. Amma biz bu atributları özümüz idarə edə bilərik.

#### Xüsusiyyət Təsvirçisi (Property Descriptor)
Bu atributları idarə etmək üçün **"property descriptor"** adlanan bir obyektdən istifadə olunur. Bu obyektin iki növü var:
1.  **Data Descriptor:** `value`, `writable`, `enumerable`, `configurable` xüsusiyyətlərinə malikdir.
2.  **Accessor Descriptor:** `get`, `set`, `enumerable`, `configurable` xüsusiyyətlərinə malikdir.

#### `Object.getOwnPropertyDescriptor()` — Atributları Oxumaq
Bir xüsusiyyətin atributlarını (yəni onun descriptor obyektini) oxumaq üçün istifadə olunur.
```javascript
const user = { name: "Ali" };
const descriptor = Object.getOwnPropertyDescriptor(user, 'name');

console.log(descriptor);
// ✅ Nəticə:
// {
//   value: "Ali",
//   writable: true,
//   enumerable: true,
//   configurable: true
// }

const random = {
  get octet() { return Math.floor(Math.random()*256); },
};
console.log(Object.getOwnPropertyDescriptor(random, 'octet'));
// ✅ Nəticə: { get: f, set: undefined, enumerable: true, configurable: true }
```
❗️ Bu metod yalnız obyektin **öz (own)** xüsusiyyətlərini yoxlayır, prototipdən (prototype) gələnləri yox.

#### `Object.defineProperty()` — Atributları Təyin Etmək
Bir xüsusiyyəti sıfırdan, xüsusi atributlarla yaratmaq və ya mövcud bir xüsusiyyətin atributlarını dəyişmək üçün istifadə olunur.

**Geniş Nümunə: Xüsusiyyəti addım-addım idarə etmək**
```javascript
const obj = {};

// 1. Yeni bir 'id' xüsusiyyəti yaradırıq, amma onu sadalana bilən (enumerable) etmirik.
Object.defineProperty(obj, 'id', {
  value: 101,
  writable: true,
  enumerable: false, // for...in və Object.keys()-də görünməyəcək
  configurable: true
});

console.log("obj.id:", obj.id); // ✅ Nəticə: 101
console.log("Object.keys(obj):", Object.keys(obj)); // ✅ Nəticə: [] (çünki enumerable deyil)

// 2. İndi 'id'-ni yalnız-oxunaqlı (read-only) edirik
Object.defineProperty(obj, 'id', { writable: false });

obj.id = 202; // Dəyişməyə cəhd edirik
console.log("Yeni obj.id:", obj.id); // ✅ Nəticə: 101 (dəyişmədi, strict mode-da xəta verər)

// 3. Hələ də `configurable: true` olduğu üçün, onu yenidən dəyişə bilərik
Object.defineProperty(obj, 'id', { value: 303 });
console.log("Yenidən dəyişdirilmiş obj.id:", obj.id); // ✅ Nəticə: 303
```
❗️ Bir xüsusiyyəti `configurable: false` etsəniz, onu bir daha silə bilməzsiniz və əksər atributlarını dəyişə bilməzsiniz. Bu, xüsusiyyəti "dondurmaq" kimidir.

#### `Object.defineProperties()` — Birdən Çox Xüsusiyyəti Təyin Etmək
Eyni anda bir neçə xüsusiyyəti atributları ilə birlikdə təyin etməyə imkan verir.
```javascript
const point = {};

Object.defineProperties(point, {
  x: { value: 10, writable: true, enumerable: true },
  y: { value: 20, writable: true, enumerable: true },
  r: {
    get() { return Math.sqrt(this.x * this.x + this.y * this.y); },
    enumerable: true
  }
});

console.log(point.r); // ✅ Nəticə: 22.36...
```
---
#### `Object.assign()` Problemi və Həlli
`Object.assign()` obyektləri kopyalayarkən yalnız xüsusiyyətlərin **dəyərlərini** kopyalayır, onların atributlarını (məsələn, `get`/`set` metodlarını) yox.

**Problem Nümunəsi:**
```javascript
const counter = {
  c: 0,
  get count() { return this.c++; } // Hər çağırıldıqda `c`-ni artırır
};

console.log(counter.count); // 0
console.log(counter.count); // 1

// `assign` ilə kopyalamağa çalışaq
const copiedCounter = Object.assign({}, counter);

// `count` xüsusiyyətinin `get` metodu deyil, onun o anki dəyəri (2) kopyalandı
console.log(copiedCounter.count); // 2
console.log(copiedCounter.count); // 2 (artmır, çünki artıq sadə bir data xüsusiyyətidir)
```

**Həll Yolu:** Atributları da kopyalayan öz funksiyası.
```javascript
function assignDescriptors(target, ...sources) {
  for (const source of sources) {
    // Obyektin öz (own) xüsusiyyətlərinin adlarını götürürük
    for (const name of Object.getOwnPropertyNames(source)) {
      // Hər xüsusiyyətin descriptor-unu alırıq
      const descriptor = Object.getOwnPropertyDescriptor(source, name);
      // Və həmin descriptor ilə hədəf obyektdə yeni xüsusiyyət yaradırıq
      Object.defineProperty(target, name, descriptor);
    }
  }
  return target;
}

const correctlyCopied = assignDescriptors({}, counter);

console.log(correctlyCopied.count); // 3 (kopyalanarkən bir dəfə də işlədi)
console.log(correctlyCopied.count); // 4 (artır, çünki getter metodu kopyalandı!)
```
Bu, metaprogramlaşdırmanın gücünü göstərən əla bir nümunədir: dilin standart davranışını öz ehtiyaclarımıza uyğun genişləndirmək.

***
### 14.2 Obyektin Genişləndirilməsi (Object Extensibility) 🔒
Bir obyektin **genişləndirilə bilən (extensible)** atributu, ona yeni xüsusiyyətlər (properties) əlavə edilib-edilməyəcəyini müəyyən edir. Standart olaraq, bütün obyektlər genişləndirilə biləndir. Amma biz bunu dəyişə bilərik.

Bu, obyektləri arzuolunmaz dəyişikliklərdən qorumaq və onların strukturunu "kilidləmək" üçün faydalıdır.

**`Object.preventExtensions()` — Yeni Xüsusiyyətləri Qadağan Etmək**
Bu funksiya, obyekti qeyri-genişləndirilən (non-extensible) edir. Bundan sonra obyektə yeni xüsusiyyət əlavə etmək cəhdi uğursuz olacaq (`strict mode`-da `TypeError` verəcək).
* **Yoxlamaq üçün:** `Object.isExtensible()`
* **❗️ Vacib:** Bu prosesi geri qaytarmaq mümkün deyil.

**Nümunə:**
```javascript
const user = { name: "Ayan" };

console.log("Genişləndirilə biləndir?", Object.isExtensible(user)); // ✅ true
user.age = 25; // Yeni xüsusiyyət əlavə edirik
console.log("Yaş əlavə olundu:", user); // ✅ { name: 'Ayan', age: 25 }

console.log("--- Genişlənmənin qarşısı alınır... ---");
Object.preventExtensions(user);

console.log("İndi genişləndirilə biləndir?", Object.isExtensible(user)); // ✅ false

user.city = "Bakı"; // Yeni xüsusiyyət əlavə etməyə cəhd edirik
console.log("Şəhər əlavə olundu?", user); // ✅ { name: 'Ayan', age: 25 } (əlavə olunmadı)
```

#### `Object.seal()` — Möhürləmək
Bu metod daha sərtdir. O, obyekti həm **qeyri-genişləndirilən** edir, həm də onun bütün mövcud xüsusiyyətlərini **qeyri-konfiqurasiyalı (`configurable: false`)** edir.
* **Nəticə:** Yeni xüsusiyyət əlavə etmək və ya mövcud olanı silmək olmaz. Amma mövcud xüsusiyyətlərin dəyərini (əgər `writable: true` isə) dəyişmək olar.
* **Yoxlamaq üçün:** `Object.isSealed()`

**Nümunə:**
```javascript
const car = { brand: "Mercedes", year: 2022 };
Object.seal(car);

console.log("Möhürlənib?", Object.isSealed(car)); // ✅ true

// 1. Yeni xüsusiyyət əlavə etmək cəhdi (uğursuz)
car.color = "black";
// 2. Mövcud xüsusiyyəti silmək cəhdi (uğursuz)
delete car.year;
// 3. Mövcud xüsusiyyətin dəyərini dəyişmək cəhdi (uğurlu!)
car.brand = "BMW";

console.log("Nəticə:", car); // ✅ { brand: "BMW", year: 2022 }
```
#### `Object.freeze()` — Dondurmaq 🧊
Bu, ən sərt kilidləmə metodudur. Obyekti həm **möhürləyir (seals)**, həm də bütün data xüsusiyyətlərini **yalnız-oxunaqlı (`writable: false`)** edir.
* **Nəticə:** Obyektə nəyisə əlavə etmək, silmək və ya dəyişmək mümkün deyil.
* **Yoxlamaq üçün:** `Object.isFrozen()`

**Nümunə:**
```javascript
const settings = { theme: "dark", version: "1.0.0" };
Object.freeze(settings);

console.log("Dondurulub?", Object.isFrozen(settings)); // ✅ true

// Dəyəri dəyişməyə cəhd edirik
settings.theme = "light";

console.log("Nəticə:", settings); // ✅ { theme: "dark", version: "1.0.0" } (dəyişmədi)
```
❗️ Bu üç metodun hamısı yalnız obyektin özünə təsir edir, onun prototipinə (prototype) yox.

---
### 14.3 Prototip (prototype) Atributu 🔗
Bir obyektin **prototip atributu**, onun hansı obyektdən xüsusiyyətləri miras aldığını müəyyən edir. Bu, JavaScript-in irsiyyət (inheritance) mexanizminin təməlidir.

#### Prototipi Əldə Etmək (Getting the Prototype)

* **`Object.getPrototypeOf(obj)`**: Bir obyektin prototipini qaytaran standart və müasir üsuldur.
* **`p.isPrototypeOf(o)`**: `p` obyektinin `o` obyektinin prototip zəncirində olub-olmadığını yoxlayır (`true`/`false`).

**Nümunə:**
```javascript
// Boş obyektin prototipi Object.prototype-dır
console.log(Object.getPrototypeOf({})); // [Object: null prototype] {} ...

// Massivin (array) prototipi Array.prototype-dır
console.log(Object.getPrototypeOf([])); // Array.prototype

// Bir prototip yaradırıq
const parent = {
  speak() { console.log("Hello from parent!"); }
};
// Həmin prototiplə yeni bir obyekt yaradırıq
const child = Object.create(parent);

console.log(parent.isPrototypeOf(child)); // ✅ true
console.log(Object.prototype.isPrototypeOf(parent)); // ✅ true
```
#### Prototipi Dəyişmək (Setting the Prototype)
`Object.setPrototypeOf(obj, proto)` metodu, bir obyektin prototipini yaradıldıqdan sonra dəyişməyə imkan verir.

**❗️ ÇOX VACİB PERFORMANS XƏBƏRDARLIĞI:** Bu metoddan istifadə etmək **qətiyyən məsləhət görülmür!** JavaScript mühərrikləri (engines) kodun sürətli işləməsi üçün obyektlərin prototiplərinin sabit qaldığını fərz edərək optimizasiyalar aparır. `setPrototypeOf` çağırmaq bu optimizasiyaları pozur və kodunuzun kəskin şəkildə yavaşlamasına səbəb ola bilər.

#### Köhnə (Legacy) `__proto__` Xüsusiyyəti
Köhnə brauzerlərdə prototipə müraciət etmək üçün qeyri-standart `__proto__` (iki altdan xətt) xüsusiyyətindən istifadə olunurdu. Bu gün də brauzerlərdə dəstəklənir, amma istifadəsi məsləhət deyil.

Onun yeganə maraqlı tətbiqi, obyekt literalı daxilində prototipi birbaşa təyin etməkdir:
```javascript
const animalProto = {
  breathe() { console.log("Nəfəs alıram..."); }
};

const dog = {
  bark() { console.log("Hav-hav!"); },
  __proto__: animalProto // Prototipi birbaşa təyin edirik
};

dog.breathe(); // ✅ Nəticə: Nəfəs alıram...
```

***
### 14.4 Tanınmış Simvollar (Well-Known Symbols) 🏷️

ES6-da `Symbol` tipi təqdim olunarkən əsas məqsədlərdən biri, mövcud kodu pozmadan dilə yeni funksionallıqlar əlavə etmək idi. Bunun üçün **"tanınmış simvollar (well-known symbols)"** yaradıldı.

Bunlar, `Symbol` obyektinin özünün statik xüsusiyyətləri olan xüsusi simvollardır (məsələn, `Symbol.iterator`). Biz bu simvolları öz siniflərimizdə (classes) və obyektlərimizdə metod adı kimi istifadə edərək, JavaScript-in `for...of`, `instanceof` kimi daxili mexanizmlərinin davranışını öz ehtiyaclarımıza uyğun dəyişə bilərik.

---
#### 14.4.1 `Symbol.iterator` və `Symbol.asyncIterator`
Bu iki simvol, obyektləri **iterable (iterasiya edilə bilən)** və ya **asinxron iterable** etmək üçündür. Onları Fəsil 12 və 13-də detallı şəkildə araşdırdığımız üçün burada sadəcə xatırlatmaqla kifayətlənirik.

---
#### 14.4.2 `Symbol.hasInstance` — `instanceof` Operatorunu Fərdiləşdirmək 🔍
Normalda `o instanceof C` ifadəsi, `C.prototype`-ın `o` obyektinin prototip zəncirində olub-olmadığını yoxlayır. Lakin `Symbol.hasInstance` bu davranışı tamamilə dəyişməyə imkan verir.

Əgər bir obyektin `[Symbol.hasInstance]` adlı metodu varsa, `instanceof` operatoru həmin metodu çağırır və nəticə olaraq onun qaytardığı `true`/`false` dəyərini istifadə edir.

**Nümunə: `uint8` (0-255 arası tam rəqəm) tipini yoxlamaq**
Təsəvvür edək ki, bir dəyərin 8-bitlik işarəsiz tam rəqəm olub-olmadığını yoxlayan bir "tip" yaratmaq istəyirik.
```javascript
const uint8 = {
  [Symbol.hasInstance](value) {
    // Bu metod, 'value instanceof uint8' çağırıldıqda işə düşür
    console.log(`Yoxlanılan dəyər: ${value}`);
    return Number.isInteger(value) && value >= 0 && value <= 255;
  }
};

console.log(128 instanceof uint8); // Yoxlanılan dəyər: 128 -> ✅ Nəticə: true
console.log(300 instanceof uint8); // Yoxlanılan dəyər: 300 -> ❌ Nəticə: false
console.log(Math.PI instanceof uint8); // Yoxlanılan dəyər: 3.14... -> ❌ Nəticə: false
```
❗️ **Qeyd:** Bu, texniki cəhətdən maraqlı olsa da, kodu oxumağı çətinləşdirə bilər. Çox vaxt `isUint8(value)` kimi sadə bir funksiya yazmaq daha anlaşıqlı olur.

---
#### 14.4.3 `Symbol.toStringTag` — Obyektin "Teq"ini Dəyişmək
Adi `typeof` operatoru bütün obyektlər üçün `"object"` qaytarır və onların daxili tipini ayırd edə bilmir. Lakin `Object.prototype.toString.call()` metodu daha detallı məlumat verir.

**Nümunə: `classof` köməkçi funksiyası**
Bu funksiya, istənilən dəyərin daxili sinif (class) adını qaytarır.
```javascript
function classof(o) {
  return Object.prototype.toString.call(o).slice(8, -1);
}

console.log(classof([]));      // ✅ "Array"
console.log(classof(new Date())); // ✅ "Date"
console.log(classof(/./));     // ✅ "RegExp"
console.log(classof({}));      // ✅ "Object"
```
Problem o idi ki, ES6-dan əvvəl bizim yaratdığımız siniflər üçün bu metod həmişə `"Object"` qaytarırdı. İndi isə `Symbol.toStringTag` sayəsində biz öz siniflərimiz üçün xüsusi "teq" təyin edə bilərik.

**Geniş Nümunə: `User` sinifi üçün xüsusi teq yaratmaq**
```javascript
// Əvvəlcə, simvol olmadan bir sinif yaradırıq
class User {
  constructor(name) {
    this.name = name;
  }
}

const userWithoutTag = new User("Ali");
console.log("Simvolsuz sinifin teqi:", classof(userWithoutTag)); // ❌ Nəticə: "Object"

// İndi isə eyni sinifə Symbol.toStringTag əlavə edirik
class BetterUser {
  constructor(name) {
    this.name = name;
  }

  // Bu getter, Object.prototype.toString.call() çağırıldıqda istifadə olunacaq
  get [Symbol.toStringTag]() {
    return "User"; // Xüsusi teqimizi təyin edirik
  }
}

const userWithTag = new BetterUser("Ayan");
console.log("Simvollu sinifin teqi:", classof(userWithTag)); // ✅ Nəticə: "User"
console.log(Object.prototype.toString.call(userWithTag)); // ✅ Nəticə: "[object User]"
```
Bu, yazdığımız siniflərin və kitabxanaların JavaScript-in daxili mexanizmləri ilə daha yaxşı inteqrasiya olmasına kömək edir.

***
### 14.4.4 `Symbol.species` — Obyektlərin "Növünü" Təyin Etmək 🧬
ES6-dan əvvəl `Array` kimi daxili (built-in) siniflərdən alt-siniflər (subclasses) yaratmaq çox çətin idi. İndi isə `class` və `extends` ilə bu çox asandır. Bəs belə bir alt-sinif yaratdıqda nə baş verir?

**Problem:** Tutaq ki, `Array`-dən törəyən `EZArray` adlı bir sinifimiz var. `Array`-dən miras aldığı `.map()`, `.filter()` kimi metodlar yeni bir massiv (array) qaytardıqda, bu yeni massiv `Array` tipində olmalıdır, yoxsa `EZArray`?

```javascript
class EZArray extends Array {
  // İlk və son elementləri rahatlıqla əldə etmək üçün getter-lər əlavə edirik
  get first() { return this[0]; }
  get last() { return this[this.length - 1]; }
}

let e = new EZArray(1, 2, 3);
console.log(e.last); // ✅ Nəticə: 3

// `e` üzərində .map() çağırırıq
let f = e.map(x => x * x);
console.log(f); // ✅ Nəticə: EZArray(3) [ 1, 4, 9 ]

// `f` də bir EZArray-dir!
console.log(f instanceof EZArray); // ✅ Nəticə: true
console.log(f.last); // ✅ Nəticə: 9
```
Gördüyünüz kimi, standart davranış olaraq, metodlar alt-sinifin (`EZArray`) bir nüsxəsini qaytarır.

**Həll Yolu: `Symbol.species`**
Bu davranışa **`Symbol.species`** adlı statik (static) bir getter nəzarət edir. Sadə dillə desək, `.map()` kimi metodlar yeni bir massiv yaradarkən, "hansı tipdə massiv yaradım?" sualını `Symbol.species`-ə verir. Standart olaraq, cavab "elə öz tipimdə" (`this`, yəni `EZArray`) olur.

Əgər biz `.map()` metodunun adi bir `Array` qaytarmasını istəyiriksə, bu davranışı öz sinifimizdə dəyişə bilərik.

**Nümunə: `Symbol.species`-i dəyişdirmək**
```javascript
class NormalArrayReturningEZArray extends Array {
  // `species`-i dəyişərək, bütün metodların adi Array qaytarmasını təmin edirik
  static get [Symbol.species]() { 
    return Array; 
  }
  
  get first() { return this[0]; }
  get last() { return this[this.length - 1]; }
}

let e = new NormalArrayReturningEZArray(1, 2, 3);
let f = e.map(x => x * x);

console.log(f instanceof NormalArrayReturningEZArray); // ✅ Nəticə: false
console.log(f instanceof Array); // ✅ Nəticə: true
console.log(f.last); // ✅ Nəticə: undefined (çünki `f` artıq `EZArray` deyil və bu getter-ə sahib deyil)
```
Bu simvol, həmçinin `TypedArray`, `ArrayBuffer` və `Promise` kimi digər daxili siniflərdə də oxşar məntiqlə işləyir.

---
### 14.4.5 `Symbol.isConcatSpreadable` — `.concat()` Davranışını Dəyişmək ↔️
`.concat()` metodu, arqumentlərini birləşdirərkən fərqli davranır:
* Əgər arqument bir massivdirsə, onun elementlərini "yayıb" (spread/flatten) yeni massivə əlavə edir.
* Əgər arqument massiv deyilsə, onu bir bütün kimi yeni massivə əlavə edir.

`[1, 2].concat([3, 4])` → `[1, 2, 3, 4]` (yayılır)
`[1, 2].concat(5)` → `[1, 2, 5]` (birbaşa əlavə olunur)

ES6-dan əvvəl bu qərar `Array.isArray()` ilə verilirdi. İndi isə **`Symbol.isConcatSpreadable`** bizə bu davranış üzərində tam nəzarət imkanı verir. Bir obyektin bu adlı `true` xüsusiyyəti varsa, `.concat()` onu massiv kimi "yaymağa" çalışacaq.

**Nümunə 1: Massivəbənzər (Array-like) Obyekti "Yaymaq"**
Tutaq ki, massiv olmayan, amma massiv kimi görünən bir obyektimiz var.
```javascript
const arrayLike = {
  length: 2,
  0: "a",
  1: "b"
};

// Simvol olmadan, obyekt bir bütün kimi əlavə olunur
console.log([0].concat(arrayLike)); // ✅ Nəticə: [0, { length: 2, 0: 'a', 1: 'b' }]

// İndi isə onu "yayıla bilən" edək
arrayLike[Symbol.isConcatSpreadable] = true;

console.log([0].concat(arrayLike)); // ✅ Nəticə: [0, "a", "b"]
```

**Nümunə 2: `Array` Alt-sinifinin "Yayılmasını" Ləğv Etmək**
Standart olaraq, `Array`-dən törəyən siniflər də "yayıla biləndir". Biz bunun qarşısını ala bilərik.
```javascript
class NonSpreadableArray extends Array {
  // `concat`-a deyirik ki, bu obyektin elementlərini "yaymasın"
  get [Symbol.isConcatSpreadable]() {
    return false;
  }
}

let a = new NonSpreadableArray("x", "y");
let b = [1, 2].concat(a);

console.log(b); // ✅ Nəticə: [1, 2, NonSpreadableArray(2) [ 'x', 'y' ]]
console.log(b.length); // ✅ Nəticə: 3 (əgər yayılsaydı, 4 olardı)
```
***
### 14.4.6 Nümunə Axtarışı Simvolları (Pattern-Matching Symbols) 🔍

Biz bilirik ki, `String.prototype.search()` və `.match()` kimi metodlar adətən `RegExp` (Requlyar İfadə) obyekti ilə işləyir. Lakin ES6-dan sonra bu metodlar daha da güclənib. Artıq biz öz "nümunə axtarışı" məntiqimizi quran siniflər (classes) yarada bilərik.

**Bu necə işləyir?**
Çox sadə. Əgər `string.search(myPattern)` kimi bir metod çağırsanız və `myPattern` obyekti `RegExp` deyilsə, JavaScript yoxlayır ki, onun `[Symbol.search]` adlı bir metodu var ya yox. Əgər varsa, JavaScript əslində bu kodu işə salır: `myPattern[Symbol.search](string)`.

Bu, aşağıdakı 5 metod üçün keçərlidir:
* `Symbol.match`
* `Symbol.matchAll`
* `Symbol.search`
* `Symbol.replace`
* `Symbol.split`

**Geniş Nümunə: `Glob` Nümunələri Yaratmaq**
Gəlin fayl sistemlərində tez-tez istifadə olunan, `*` (istənilən sayda simvol) və `?` (bir simvol) ilə işləyən öz `Glob` pattern sinifimizi yaradaq.
```javascript
class Glob {
  constructor(glob) {
    this.glob = glob;
    
    // Glob pattern-ini arxa planda işləməsi üçün RegExp-ə çeviririk.
    // ? -> bir simvol deməkdir, amma / xaric.
    // * -> istənilən sayda simvol deməkdir, amma / xaric.
    const regexpText = glob.replace("?", "([^/])").replace("*", "([^/]*)");
    this.regexp = new RegExp(`^${regexpText}$`, "u");
  }

  toString() {
    return this.glob;
  }
  
  // İndi isə string metodlarının axtardığı simvolları təyin edirik:
  
  [Symbol.search](s) {
    console.log(`Glob[Symbol.search] metodu "${s}" üzərində çağırıldı.`);
    return s.search(this.regexp);
  }
  
  [Symbol.match](s) {
    console.log(`Glob[Symbol.match] metodu "${s}" üzərində çağırıldı.`);
    return s.match(this.regexp);
  }
  
  [Symbol.replace](s, replacement) {
    console.log(`Glob[Symbol.replace] metodu "${s}" üzərində çağırıldı.`);
    return s.replace(this.regexp, replacement);
  }
}

const pattern = new Glob("docs/*.txt");

// İndi `pattern`-i adi string metodları ilə istifadə edə bilərik!
"docs/file.txt".search(pattern);   // ✅ Nəticə: 0 (uyğunluq var)
"docs/file.html".search(pattern); // ✅ Nəticə: -1 (uyğunluq yoxdur)

const match = "docs/file.txt".match(pattern);
console.log("Qrup:", match[1]); // ✅ Nəticə: "file" (çünki * qrupun içindədir)

const replaced = "docs/file.txt".replace(pattern, "images/$1.jpg");
console.log("Əvəzlənmiş:", replaced); // ✅ Nəticə: "images/file.jpg"
```

---
### 14.4.7 `Symbol.toPrimitive` — Obyekti Primitivə Çevirmək 🔢
JavaScript bir obyekti primitiv bir dəyərə (sətir, rəqəm) çevirmək lazım gəldikdə, adətən onun `.toString()` və ya `.valueOf()` metodlarını çağırır. Hansını birinci çağıracağı isə kontekstdən asılıdır.

`Symbol.toPrimitive` bu proses üzərində tam nəzarət etməyə imkan verən müasir bir yoldur. Obyektin bu adda bir metodu varsa, JavaScript çevirmə əməliyyatı üçün birbaşa onu çağırır və ona bir **"göstəriş" (hint)** ötürür.

`hint`-in 3 mümkün dəyəri var:
* `"string"`: Obyektin sətirə (string) çevrilməsi gözlənilir (məsələn, template literal içində ` `${myObj}` `).
* `"number"`: Obyektin rəqəmə çevrilməsi gözlənilir (məsələn, `myObj * 2`).
* `"default"`: Hansı tipin lazım olduğu dəqiq bilinmir (`myObj + myObj` kimi).

**Geniş Nümunə: `Money` Sinifi**
```javascript
class Money {
  constructor(amount, currency) {
    this.amount = amount;
    this.currency = currency;
  }

  [Symbol.toPrimitive](hint) {
    console.log(`💡 Çevrilmə üçün göstəriş (hint): "${hint}"`);
    
    switch (hint) {
      case 'number':
        return this.amount;
      case 'string':
        return `${this.amount} ${this.currency}`;
      case 'default':
      default:
        return `${this.amount} ${this.currency}`;
    }
  }
}

const price = new Money(99.95, "AZN");

// String konteksti
console.log(`Məhsulun qiyməti: ${price}`);
// 💡 Çevrilmə üçün göstəriş (hint): "string"
// ✅ Nəticə: Məhsulun qiyməti: 99.95 AZN

// Number konteksti
console.log(`Qiymətin iki misli: ${price * 2}`);
// 💡 Çevrilmə üçün göstəriş (hint): "number"
// ✅ Nəticə: Qiymətin iki misli: 199.9

// Default kontekst
console.log("Qiyməti sətirlə birləşdirmək: " + price);
// 💡 Çevrilmə üçün göstəriş (hint): "default"
// ✅ Nəticə: Qiyməti sətirlə birləşdirmək: 99.95 AZN
```

---
### 14.4.8 `Symbol.unscopables` — Köhnə Kodla Uyğunluq (Compatibility) 🛡️
Bu, çox nadir və texniki bir simvoldur. Onun yeganə məqsədi, köhnə və artıq istifadəsi **məsləhət görülməyən** `with` ifadəsi ilə yaranan uyğunluq (compatibility) problemlərini həll etməkdir.

**Problem nə idi?** `with(myArray)` kimi bir kod yazıldıqda, `myArray`-in bütün xüsusiyyətləri sanki birbaşa dəyişən kimi əlçatan olurdu. `Array.prototype`-a yeni metodlar (məsələn, `.values()`) əlavə edildikdə, bu, köhnə saytlardakı, eyni adda (`values`) dəyişəni olan kodları pozurdu.

**Həll Yolu:** `Symbol.unscopables` adlı bir obyekt, `with` ifadəsinə deyir ki, "bu siyahıdakı xüsusiyyətləri görməzdən gəl". `Array.prototype`-da belə bir obyekt var və o, `.keys()`, `.values()` kimi yeni metodları `with`-dən gizlədir.

Bizim üçün maraqlı olan yeganə tərəfi, bu simvol vasitəsilə `Array`-ə hansı yeni metodların əlavə olunduğunu görməkdir:
```javascript
console.log(Object.keys(Array.prototype[Symbol.unscopables]));
// ✅ Nəticə (təxmini): ["at", "copyWithin", "entries", "fill", "find", "findIndex", "flat", "flatMap", "includes", "keys", "values"]
```
Bu simvolu öz kodunuzda istifadə etməyə demək olar ki, heç vaxt ehtiyacınız olmayacaq.

***
### 14.5 Şablon Teqləri (Template Tags) 🏷️

Biz artıq **şablon literalları (template literals)** ilə tanışıq: `` `...` ``. Bəs bu ` `` `-dən dərhal əvvəl bir funksiya adı yazsaq nə olar? Bu zaman o, **teqlənmiş şablon (tagged template)** olur və bu, adi bir sətir deyil, xüsusi bir funksiya çağırışına çevrilir.

Bu xüsusiyyət, adətən, JavaScript daxilində xüsusi "mini dillər" – **DSL-lər (Domain-Specific Languages)** yaratmaq üçün istifadə olunur. Məsələn, bəzi məşhur kitabxanalar bundan istifadə edir:
* **Styled Components:** `css`\` color: red; \`` ilə birbaşa CSS yazmaq üçün.
* **GraphQL:** `gql`\` query { user { id } } \`` ilə GraphQL sorğuları yazmaq üçün.

Gəlin görək bu sehr necə işləyir və öz teq funksiyalarımızı necə yarada bilərik.

#### Teq Funksiyası Necə İşləyir?
Bir funksiyanı teq kimi istifadə etdikdə, JavaScript onu xüsusi arqumentlərlä çağırır:
* **Birinci arqument:** Şablonun dəyişənlərə (`${...}`) bölünmüş statik sətir hissələrindən ibarət bir massiv (array).
* **Sonrakı arqumentlər:** Şablonun içindəki `${...}` dəyişənlərinin dəyərləri (ardıcıllıqla).

**Nümunənin Sökülməsi (Deconstruction):**
Tutaq ki, belə bir kodumuz var:
```javascript
const name = "Ayan";
const age = 25;

myTag`Salam, ${name}! Sənin ${age} yaşın var.`;
```
JavaScript arxa planda bunu bu funksiya çağırışına çevirir:
```javascript
myTag(
  ['Salam, ', '! Sənin ', ' yaşın var.'], // 1. Statik sətirlər massivi
  name,                                    // 2. Birinci dəyişən
  age                                      // 3. İkinci dəyişən
);
```

Teq funksiyasının qaytardığı dəyər isə teqlənmiş şablonun son nəticəsi olur. Bu, sətir (string) olmaya da bilər!

---
**Geniş Nümunələr**

**Nümunə 1: `html` Teqi - Təhlükəsiz HTML Yaratmaq 🛡️**
Problem: Dəyişənləri birbaşa HTML içinə yerləşdirmək təhlükəlidir, çünki bu, XSS (Cross-Site Scripting) hücumlarına yol aça bilər. Gəlin elə bir `html` teqi yaradaq ki, bütün dəyişənləri HTML üçün təhlükəsiz hala gətirsin (escape etsin).

```javascript
function html(strings, ...values) {
  console.log("Statik hissələr:", strings);
  console.log("Dinamik dəyərlər:", values);

  // 1. Hər bir dəyişəni təhlükəsiz formata salırıq
  const escapedValues = values.map(value => 
    String(value)
      .replace(/&/g, "&amp;")
      .replace(/</g, "&lt;")
      .replace(/>/g, "&gt;")
      .replace(/"/g, "&quot;")
      .replace(/'/g, "&#39;")
  );

  // 2. Statik hissələri və təhlükəsiz dəyişənləri birləşdiririk
  let result = strings[0];
  for (let i = 0; i < escapedValues.length; i++) {
    result += escapedValues[i] + strings[i+1];
  }
  
  return result;
}

const userInput = "<script>alert('HACKED!')</script>";
const safeHtml = html`<h2>Salam, ${userInput}</h2>`;

console.log("Təhlükəsiz HTML:", safeHtml);
// ✅ Nəticə: <h2>Salam, &lt;script&gt;alert(&#39;HACKED!&#39;)&lt;/script&gt;</h2>
// Gördüyünüz kimi, təhlükəli teqlər sadə mətnə çevrildi.
```

**Nümunə 2: `glob` Teqi - Fərdi Obyekt Yaratmaq 🧱**
Bir teq funksiyası mütləq sətir (string) qaytarmalı deyil. O, bir obyekt, massiv (array) və ya istənilən başqa bir dəyər qaytara bilər. Əvvəlki bölmədəki `Glob` sinifimizi xatırlayaq və onun üçün bir teq yaradaq.
```javascript
// Glob sinifimizi yenidən təyin edirik (əvvəlki bölmədən)
class Glob {
  constructor(glob) { this.glob = glob; /* ...davamı... */ }
  // ...davamı...
}

function glob(strings, ...values) {
  // Bütün hissələri birləşdirib tək bir sətirə çeviririk
  let s = strings[0];
  for (let i = 0; i < values.length; i++) {
    s += values[i] + strings[i+1];
  }
  // Və bu sətirdən yeni bir Glob obyekti yaradıb qaytarırıq
  return new Glob(s);
}

const rootDir = "/usr/files";
const filePattern = glob`${rootDir}/*.js`;

console.log(filePattern instanceof Glob); // ✅ Nəticə: true
console.log(filePattern.glob); // ✅ Nəticə: "/usr/files/*.js"
```

**Bonus: `strings.raw` Xüsusiyyəti**
Teq funksiyasına ötürülən ilk arqumentin (`strings` massivinin) `.raw` adlı xüsusi bir xüsusiyyəti də var. Bu, eyni sətir hissələrini saxlayır, lakin daxilindəki `\n`, `\t` kimi escape ardıcıllıqlarını emal etmir, onları olduğu kimi saxlayır.

```javascript
function demo(strings) {
  console.log("Normal string:", strings[0]);
  console.log("Raw string:", strings.raw[0]);
}

demo`salam\ndünya`;
// ✅ Nəticə:
// Normal string: salam
// dünya
// Raw string: salam\ndünya
```
Bu, öz sintaksisində `\` işarəsindən istifadə edən DSL-lər (məsələn, Windows fayl yolları) yaratmaq üçün faydalıdır.

***
### 14.6 `Reflect` API-ı  зеркало

`Reflect` bir sinif (class) deyil, `Math` obyekti kimi, içində bir neçə statik metod saxlayan bir obyektdir. Bu metodlar ES6-da təqdim edilib və JavaScript-in daxili, fundamental əməliyyatlarını proqramatik olaraq çağırmaq üçün bir "API" təqdim edir.

**Bəs `Reflect` nəyə lazımdır?** 🤔
Təsəvvür et ki, JavaScript-də `delete obj.prop`, `prop in obj` kimi operatorlar və `Object.defineProperty()` kimi metodlar var. `Reflect` bütün bu fərqli sintaksisləri vahid, funksional bir "adlar fəzasında" (namespace) birləşdirir.

Onun iki əsas üstünlüyü var:
1.  **Daha Yaxşı Geri Dəyər:** `Object.defineProperty()` kimi bəzi metodlar xəta baş verdikdə `TypeError` atır. `Reflect`-in müvafiq metodları isə xəta baş verdikdə sadəcə `false`, uğurlu olduqda isə `true` qaytarır. Bu, `try/catch` olmadan sadə bir `if` ilə yoxlama aparmağı asanlaşdırır.
2.  **`Proxy` ilə Uyğunluq:** `Reflect` API-ındakı metodların siyahısı, növbəti bölmədə öyrənəcəyimiz `Proxy` obyektinin "tələlərinin" (traps) siyahısı ilə eynidir. Bu, `Proxy` yaradarkən standart davranışı çağırmağı çox asanlaşdırır.

#### `Reflect` Metodları və Nümunələr

Aşağıda ən çox istifadə olunan `Reflect` metodları və onların sadə nümunələri verilmişdir.

**1. Xüsusiyyətlərlə İşləmək (`get`, `set`, `has`)**
Bu metodlar, obyektin xüsusiyyətlərini oxumaq, yazmaq və yoxlamaq üçündür.
```javascript
const user = {
  name: "Ayan",
  _age: 25,
  get age() { return this._age; }
};

// Reflect.get() -> obj.prop kimidir
console.log("Ad:", Reflect.get(user, 'name')); // ✅ "Ayan"

// Reflect.set() -> obj.prop = value kimidir. Uğurlu olarsa true qaytarır.
Reflect.set(user, 'name', 'Elvin');
console.log("Yeni ad:", user.name); // ✅ "Elvin"

// Reflect.has() -> 'prop' in obj kimidir
console.log("`age` xüsusiyyəti varmı?:", Reflect.has(user, 'age')); // ✅ true
console.log("`city` xüsusiyyəti varmı?:", Reflect.has(user, 'city')); // ✅ false
```

**2. Xüsusiyyətləri Təyin Etmək və Silmək (`defineProperty`, `deleteProperty`)**
Bunlar, `Object.defineProperty()` və `delete` operatorunun funksional alternativləridir.
```javascript
const car = {};

// `Object.defineProperty` xəta atdığı halda, `Reflect.defineProperty` sadəcə `false` qaytarır
const success = Reflect.defineProperty(car, 'model', {
  value: 'Mercedes',
  writable: false,
  enumerable: true
});

console.log("Əməliyyat uğurludurmu?:", success); // ✅ true
console.log("Model:", car.model); // ✅ "Mercedes"

// `delete` operatoru kimi işləyir
Reflect.deleteProperty(car, 'model');
console.log("Model silindi?", !Reflect.has(car, 'model')); // ✅ true
```

**3. Funksiya və Konstruktorları Çağırmaq (`apply`, `construct`)**
Bu metodlar funksiyaları və sinifləri (classes) proqramatik olaraq çağırmağa imkan verir.
```javascript
function greet(prefix, suffix) {
  console.log(`${prefix} ${this.name} ${suffix}`);
}

const person = { name: "Leyla" };

// greet funksiyasını `person` obyekti `this` olaraq çağırır
Reflect.apply(greet, person, ["Salam,", "!"]); // ✅ Nəticə: Salam, Leyla !

class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
    console.log(`${name} (${age} yaş) adlı istifadəçi yaradıldı.`);
  }
}

// `new User("Tural", 30)` ilə eynidir
const newUser = Reflect.construct(User, ["Tural", 30]);
```
**4. Prototip və digər metodlar (`getPrototypeOf`, `ownKeys`, ...)**
Bu metodlar da `Object` üzərindəki analoqları kimi işləyir, lakin daha ardıcıl davranış (adətən `true`/`false` qaytarmaq) sərgiləyir.
```javascript
const myObj = { [Symbol("id")]: 123, key: "value" };

// `Object.keys()`-dən fərqli olaraq, həm string, həm də Symbol açarları qaytarır
console.log("Bütün açarlar (ownKeys):", Reflect.ownKeys(myObj)); // ✅ ['key', Symbol(id)]

// Prototipi qaytarır
console.log("Prototip:", Reflect.getPrototypeOf(myObj) === Object.prototype); // ✅ true
```
**`Object` vs `Reflect` - Əsas Fərq**
Əsas fərq, xəta baş verdikdə metodların davranışıdır:
* **`Object.defineProperty()`**: Uğursuz olarsa `TypeError` **atır (throws)**. Uğurlu olarsa, obyekti qaytarır.
* **`Reflect.defineProperty()`**: Uğursuz olarsa `false` **qaytarır**. Uğurlu olarsa, `true` qaytarır.

Bu, `Reflect`-i `try/catch` blokları olmadan, sadə `if` şərtləri ilə işləmək üçün daha əlverişli edir.

Gördüyün kimi, `Reflect` API-ı obyektlərlə apardığımız standart əməliyyatları daha proqramatik və funksional bir şəkildə etməyə imkan verir. Onun əsl gücü isə növbəti və sonuncu bölməmiz olan **`Proxy`-lərlə** birlikdə ortaya çıxacaq!

***
### 14.7 Proksi (Proxy) Obyektləri 🎭

`Proxy` sinifi (class) ilə, başqa bir obyektin (hədəf obyekt) davranışını idarə edən yeni bir obyekt (proksi obyekti) yaradırıq.

**Sintaksis:** `new Proxy(target, handlers)`
* **`target`**: Proksinin "bükdüyü" orijinal obyekt. Heç bir qayda təyin edilmədikdə, bütün əməliyyatlar bu obyektə ötürülür.
* **`handlers`**: "Tələlər" (traps) adlanan metodları saxlayan bir obyekt. Hər bir tələ, müəyyən bir fundamental əməliyyatı (məsələn, xüsusiyyəti oxumaq, yazmaq) ələ keçirir.

**Əsas İşləmə Prinsipi:**
Bir proksi üzərində hər hansı bir əməliyyat apardıqda (məsələn, `proxy.name = "Ali"`), JavaScript əvvəlcə `handlers` obyektində müvafiq bir tələnin (bizim nümunədə `set` tələsinin) olub-olmadığını yoxlayır.
* **Əgər tələ varsa**, həmin funksiya çağırılır və əməliyyatın necə icra olunacağına o qərar verir.
* **Əgər tələ yoxdursa**, əməliyyat birbaşa `target` obyekti üzərində icra olunur.

#### Sadə Nümunələr
**Nümunə 1: Şəffaf Proksi (Transparent Proxy)**
Əgər `handlers` obyekti boşdursa, proksi bütün əməliyyatları birbaşa `target`-ə ötürür və tamamilə "şəffaf" olur.
```javascript
const target = { id: 10, name: "Məhsul" };
const handlers = {}; // Boş handler-lar

const proxy = new Proxy(target, handlers);

console.log(proxy.name); // ✅ Nəticə: "Məhsul" (target-dən oxundu)

proxy.price = 99.9;
console.log(target.price); // ✅ Nəticə: 99.9 (target-ə yazıldı)
```

**Nümunə 2: Ləğv Edilə Bilən Proksi (Revocable Proxy)**
Bəzən bir obyektə müvəqqəti giriş imkanı verib, sonra bu girişi bağlamaq lazım gəlir. `Proxy.revocable()` məhz bunun üçündür. O, `{ proxy, revoke }` formatında bir obyekt qaytarır.
```javascript
const sensitiveData = { secret: "12345" };
const { proxy, revoke } = Proxy.revocable(sensitiveData, {});

// Proksi vasitəsilə dataya çata bilirik
console.log(proxy.secret); // ✅ Nəticə: "12345"

// İndi isə girişi bağlayırıq!
revoke();
console.log("Proksi ləğv edildi!");

try {
  // Artıq proksiyə müraciət etmək TypeError verəcək
  console.log(proxy.secret);
} catch (e) {
  console.error("❌ Xəta:", e.message); // ✅ Nəticə: Xəta: Cannot perform 'get' on a proxy that has been revoked
}
```

---
#### Geniş Nümunələr
`handlers` obyektinə tələlər əlavə etdikdə, proksinin əsl gücü ortaya çıxır.

**Nümunə 1: Yalnız-Oxunaqlı (Read-Only) Proksi 🛡️**
Bir obyekti dəyişikliklərdən qorumaq üçün onu yalnız-oxunaqlı bir proksi ilə "bükə" bilərik.
```javascript
function readOnlyProxy(target) {
  const readOnlyHandlers = {
    // `set` tələsi, bir xüsusiyyətə dəyər yazmaq cəhdini ələ keçirir
    set(obj, prop, value) {
      throw new TypeError(`"${prop}" xüsusiyyətini dəyişmək olmaz, obyekt yalnız-oxunaqlıdır!`);
    },
    // `deleteProperty` tələsi, xüsusiyyəti silmək cəhdini ələ keçirir
    deleteProperty(obj, prop) {
      throw new TypeError(`"${prop}" xüsusiyyətini silmək olmaz.`);
    }
  };

  return new Proxy(target, readOnlyHandlers);
}

const user = { name: "Ayan" };
const readOnlyUser = readOnlyProxy(user);

console.log(readOnlyUser.name); // ✅ Oxumaq mümkündür: "Ayan"

try {
  readOnlyUser.name = "Elvin"; // Yazmağa cəhd edirik
} catch (e) {
  console.error("❌", e.message); // ✅ Nəticə: "name" xüsusiyyətini dəyişmək olmaz...
}
```
**Nümunə 2: Bütün Əməliyyatları Loglayan Proksi 🔍**
Bu, metaprogramlaşdırmanın nə qədər güclü olduğunu göstərən klassik bir nümunədir. Bu proksi, üzərində aparılan HƏR BİR əməliyyatı konsola yazdırır. Bu, başqa bir kodun və ya kitabxananın sizin obyektinizlə arxa planda necə davrandığını anlamaq üçün əla bir vasitədir.

Burada `Reflect` API-ından geniş istifadə edirik, çünki o, standart davranışı çağırmaq üçün ideal bir yoldur.
```javascript
function loggingProxy(target, targetName) {
  const handlers = {};
  
  // Bütün mümkün Reflect metodları üzərində dövr edərək, hər biri üçün bir handler yaradırıq
  for (const key of Reflect.ownKeys(Reflect)) {
    if (typeof Reflect[key] === 'function') {
      handlers[key] = function (...args) {
        // Əməliyyatı konsola yazdırırıq
        const [targetObj, propName, ...rest] = args;
        console.log(`➡️ Handler: ${key}, Hədəf: ${targetName}, Açar: ${String(propName || "")}`);
        
        // Standart əməliyyatı Reflect ilə çağırırıq və nəticəsini qaytarırıq
        return Reflect[key](...args);
      };
    }
  }
  
  return new Proxy(target, handlers);
}

// İndi bunu test edək!
const data = [10, 20];
const proxyData = loggingProxy(data, "myArray");

// `.map()` metodunun arxada necə işlədiyinə baxaq:
console.log("--- .map() metodu yoxlanılır ---");
proxyData.map(x => x * x);
```
Bu kodu işə saldıqda konsolda `.map()`-in arxa planda obyektin `.length`-ni, `.constructor`-unu, hər bir elementin mövcudluğunu (`has`) və dəyərini (`get`) necə yoxladığını addım-addım görəcəksiniz. Bu, əsl metaprogramlaşdırmadır!

***
### 14.7.1 Proksi İnvariantları (Proxy Invariants) ⚖️

`Proxy` bizə obyektlərin davranışını idarə etmək üçün inanılmaz bir güc verir. Amma bu güc sərhədsiz deyil. Biz proksilərdən istifadə edərək JavaScript-in daxili, fundamental qaydalarını pozan "qeyri-mümkün" və ya "aldadıcı" obyektlər yarada bilmərik.

Bu fundamental qaydalara **invariantlar (invariants)** deyilir.

**Problem nədir?** 🤔
Təsəvvür et ki, `Object.preventExtensions()` ilə genişləndirilməsi qadağan edilmiş bir `target` obyektimiz var. Amma biz onun üçün elə bir proksi yaradırıq ki, `isExtensible` tələsi (trap) həmişə `true` qaytarır. Bu, bir yalandır və JavaScript məntiqinə ziddir.

**Həll Yolu: Proksinin Öz Nəzarəti**
Bu cür ziddiyyətlərin qarşısını almaq üçün, `Proxy` mexanizminin özü bir "müfəttiş" rolunu oynayır. Bir tələ (trap) öz işini bitirib bir nəticə qaytardıqdan sonra, proksi həmin nəticənin `target` obyektin vəziyyəti ilə uyğun olub-olmadığını yoxlayır. Əgər bir invariant pozulursa, proksi tələnin nəticəsini qəbul etmir və dərhal bir `TypeError` atır.

Qısacası, proksi ilə **`target` obyektin dəyişməz xüsusiyyətləri haqqında yalan danışa bilməzsiniz.**

#### İnvariant Pozulması Nümunələri

**Nümunə 1: Genişləndirilmə (Extensibility) İnvariantı**
Bir proksi, hədəfi (target) qeyri-genişləndirilən (non-extensible) olduğu halda, özünü genişləndirilən kimi göstərə bilməz.
```javascript
// 1. Genişləndirilməsi dayandırılmış bir hədəf (target) yaradırıq
const target = Object.preventExtensions({});

// 2. Onun üçün "yalan danışan" bir proksi yaradırıq
const proxy = new Proxy(target, {
  isExtensible() {
    return true; // Yalan: "Mən genişləndirilə bilənəm"
  }
});

try {
  // 3. Proksinin genişləndirilə bilən olub-olmadığını yoxlayırıq
  Reflect.isExtensible(proxy);
} catch (e) {
  console.error("❌ Xəta:", e.message);
  // ✅ Nəticə: 'isExtensible' on a proxy: trap result does not reflect extensibility of proxy target.
  // Çünki proksi, hədəfin vəziyyətinə zidd olan bir cavab qaytardı.
}
```

**Nümunə 2: Dəyişməz Xüsusiyyət (Immutable Property) İnvariantı**
Əgər bir xüsusiyyət `configurable: false` və `writable: false` isə (məsələn, `Object.freeze()` ilə dondurulubsa), proksinin `get` tələsi həmin xüsusiyyət üçün fərqli bir dəyər qaytara bilməz.
```javascript
// 1. Dondurulmuş bir hədəf (target) yaradırıq
const target = Object.freeze({ greeting: "Salam" });

// 2. `get` tələsi ilə fərqli bir dəyər qaytarmağa çalışan bir proksi yaradırıq
const proxy = new Proxy(target, {
  get(obj, prop) {
    if (prop === 'greeting') {
      return "Hello"; // Yalan: Orijinal dəyər "Salam"-dır
    }
    return Reflect.get(...arguments);
  }
});

try {
  // 3. Proksi vasitəsilə dondurulmuş xüsusiyyətə müraciət edirik
  console.log(proxy.greeting);
} catch (e) {
  console.error("❌ Xəta:", e.message);
  // ✅ Nəticə: 'get' on a proxy: property 'greeting' is a read-only and non-configurable...
  // ...data property on the proxy target but the proxy did not return its actual value
}
```

**Son Nəticə:**
`Proxy` çox güclü olsa da, JavaScript-in təməl qaydalarını pozaraq tamamilə "saxta" və sistemin məntiqini alt-üst edən obyektlər yaratmağa imkan vermir. Bu invariantlar, sistemin stabil və proqnozlaşdırıla bilən qalmasını təmin edən vacib bir müdafiə mexanizmidir. 🛡️