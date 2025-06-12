# Fəsil 9. Siniflər (Classes) 🧑‍💻

Fəsil 6-da JavaScript obyektlərini (objects) fərqli xüsusiyyətlərə (properties) malik unikal varlıqlar kimi öyrəndik. Amma bəzən eyni xüsusiyyətləri (properties) paylaşan obyektlər qrupu – **sinif (class)** yaratmaq daha əlverişlidir. Bu, kodu daha səliqəli və təkrar istifadəyə yararlı edir! ✨

Sinfin (class) hər bir üzvü (member) və ya **instansiyası (instance)** özünəməxsus vəziyyətini (state) təyin edən xüsusi xüsusiyyətlərə (properties) malikdir. Eyni zamanda, onlar sinif (class) tərəfindən təyin olunan və bütün instansiyalar (instances) arasında paylaşılan davranışları (behavior) müəyyən edən **metodlara (methods)** sahibdirlər.

**Sadə bir nümunə:** 🍎 Təsəvvür edin ki, meyvələri təmsil edən bir `Fruit` sinfi (class) var. Hər bir `Fruit` instansiyasının (instance) öz rəngi (color) və çəkisi (weight) (vəziyyəti – state) olacaq. `Fruit` sinfi (class) isə meyvəni təsvir edən (`describe()`) və ya onun dadını (`taste()`) bildirən metodlara (methods) (davranışlarına – behavior) malik olacaq.

```javascript
// Meyvələri təmsil edən "Fruit" sinfi (class).
class Fruit {
    // Konstruktor (constructor) - yeni instansiya (instance) yaradılarkən çağırılır.
    constructor(color, weight) {
        this.color = color;   // Rəng (state)
        this.weight = weight; // Çəki (state)
    }

    // Metod (method) - meyvənin davranışını (behavior) təyin edir.
    describe() {
        return `Bu ${this.color} rəngdə, ${this.weight} qram ağırlığında bir meyvədir.`;
    }

    // Başqa bir metod (method).
    taste() {
        return "Meyvənin dadı var! 😋";
    }
}

// "Fruit" sinfindən (class) yeni instansiyalar (instances) yaradırıq.
let apple = new Fruit("qırmızı", 150);
let banana = new Fruit("sarı", 120);

// Instansiyaların (instances) metodlarını (methods) çağırırıq.
console.log(apple.describe());  // Çıxış: Bu qırmızı rəngdə, 150 qram ağırlığında bir meyvədir.
console.log(banana.taste());    // Çıxış: Meyvənin dadı var! 😋
```
JavaScript-də siniflər (classes) **prototip-əsaslı irsiyyət (prototype-based inheritance)** mexanizmi ilə işləyir. Qısaca desək, əgər iki obyekt (object) eyni **prototipdən (prototype)** metodları (methods) miras alırsa, onlar eyni sinfin (class) instansiyaları (instances) sayılır. 🤝

Eyni prototipdən (prototype) miras alan obyektlər (objects) adətən eyni **konstruktor funksiyası (constructor function)** və ya **fabrikat funksiyası (factory function)** tərəfindən yaradılır.

JavaScript həmişə siniflərin (classes) təyin olunmasına icazə verib. Lakin **ES6** (ECMAScript 2015) ilə `class` açar sözü (keyword) də daxil olmaqla, sinifləri (classes) daha asan yaratmağa imkan verən yeni bir sintaksis (syntax) gəldi. Bu fəsil əvvəlcə siniflərin (classes) pərdə arxasında necə işlədiyini daha aydın göstərmək üçün köhnə üsulu izah edəcək, daha sonra isə yeni sintaksisə (syntax) keçəcəyik. 🔄

Əgər siz Java və ya C++ kimi **obyekt-yönümlü proqramlaşdırma dilləri (object-oriented programming languages)** ilə tanışsınızsa, JavaScript siniflərinin (classes) onlardan fərqli olduğunu görəcəksiniz. JavaScript-in **prototip-əsaslı irsiyyət (prototype-based inheritance)** mexanizmi "klassik" siniflərdən (classes) əhəmiyyətli dərəcədə fərqlənir. Bu fərqi başa düşmək vacibdir! 🤔

---

### 9.1 Siniflər (Classes) və Prototiplər (Prototypes) 💡

JavaScript-də sinif (class) eyni **prototip obyektindən (prototype object)** xüsusiyyətləri (properties) miras alan obyektlər (objects) qrupudur. Deməli, **prototip obyekti (prototype object)** sinfin (class) özəyidir.

`Object.create()` funksiyası (Chapter 6-da izah olunduğu kimi) müəyyən bir prototip obyektindən (prototype object) miras alan yeni bir obyekt (object) yaradır. Yəni, bir prototip obyekti (prototype object) təyin edib, daha sonra `Object.create()` istifadə edərək ondan miras alan obyektlər (objects) yaratmaqla, əslində bir JavaScript sinfi (class) təyin etmiş oluruq.

Sinfin (class) instansiyaları (instances) adətən əlavə ilkinləşdirməyə (initialization) ehtiyac duyur. Buna görə də, yeni obyekti (object) yaradan və ilkinləşdirən (initializes) bir **fabrikat funksiyası (factory function)** təyin etmək geniş yayılmışdır. Nümunə 9-1 bunu göstərir:

**Nümunə 9-1. Sadə bir JavaScript sinfi (class)**

```javascript
// Bu, yeni bir diapazon (range) obyekti (object) qaytaran bir fabrikat funksiyasıdır (factory function).
function range(from, to) {
    // Prototipimiz olan "range.methods"-dən miras alan yeni bir obyekt (object) yaradırıq.
    let r = Object.create(range.methods);

    // Obyektin (object) unikal "state" (vəziyyətini) təyin edən "from" və "to" xüsusiyyətlərini (properties) saxlayın.
    r.from = from;
    r.to = to;

    // Hazır obyekti (object) qaytarın.
    return r;
}

// Bütün "range" obyektləri tərəfindən miras alınan metodları (methods) təyin edən prototip obyektimiz.
range.methods = {
    // "x" dəyərinin diapazonda (range) olub olmadığını yoxlayır.
    includes(x) { return this.from <= x && x <= this.to; },

    // Diapazonu (range) dövrə salınan (iterable) edən generator funksiyası (generator function).
    // Qeyd: Bu, yalnız rəqəmsal (numeric) diapazonlar (ranges) üçündür.
    *[Symbol.iterator]() {
        for (let x = Math.ceil(this.from); x <= this.to; x++)
            yield x;
    },

    // Diapazonun (range) simvolik (string) təmsilini qaytarır.
    toString() { return "(" + this.from + "..." + this.to + ")"; }
};

// 👇 "range" obyektinin (object) necə istifadə edildiyinə baxaq:
let r = range(1, 3);         // Bir diapazon (range) obyekti yaradırıq.
console.log(r.includes(2));  // => true: 2 diapazondadır. ✅
console.log(r.toString());   // => "(1...3)" 📝
console.log([...r]);         // => [1, 2, 3]; iterator (iterator) ilə massivə (array) çeviririk. 🔢
```

Nümunə 9-1-dəki bəzi vacib məqamlar:

* `range()` funksiyası yeni `Range` obyektləri (objects) yaratmaq üçün bir **fabrikat funksiyası (factory function)** rolunu oynayır.
* Bu funksiya sinfi (class) təyin edən prototip obyektini (prototype object) saxlamaq üçün `range.methods` adlı bir yer istifadə olunur.
* Hər bir `Range` obyektinin (object) unikal `state` (vəziyyətini) təyin edən `from` və `to` xüsusiyyətləri (properties) var.
* `range.methods` obyekti (object) **ES6 qısa sintaksisindən (shorthand syntax)** istifadə edərək metodları (methods) təyin edir (buna görə də `function` yazılmır).
* `Symbol.iterator` adlı metod (method) `Range` obyektlərini (objects) dövrə salınan (iterable) edir. `*` işarəsi onun **generator funksiyası (generator function)** olduğunu göstərir. Bu o deməkdir ki, `Range` instansiyaları (instances) `for/of` dövrü (loop) və `...` **spread operatoru (spread operator)** ilə işləyir! 🔄
* Bütün paylaşılan (shared) metodlar (methods) `this` açar sözündən (keyword) istifadə edərək `from` və `to` xüsusiyyətlərinə (properties) müraciət edir. `this` sinif metodlarının (class methods) əsas xüsusiyyətidir. 🎯

---

### 9.2 Siniflər (Classes) və Konstruktorlar (Constructors) 🛠️

Nümunə 9-1 sinif (class) təyin etməyin sadə, lakin adətən istifadə olunmayan bir yolunu göstərirdi, çünki **konstruktor (constructor)** yox idi.

**Konstruktor (constructor)** yeni obyektləri (objects) ilkinləşdirmək üçündür. ⚙️ Onlar `new` açar sözü (keyword) ilə çağırılır. `new` konstruktoru (constructor) çağırdıqda, yeni obyekti (object) avtomatik yaradır, konstruktor (constructor) isə sadəcə onun vəziyyətini (state) qurur.

Əsas məqam odur ki, `new` ilə çağırılan konstruktorun (constructor) **`prototype` xüsusiyyəti (property)** yeni obyektin (object) prototipi (prototype) olur. Yalnız funksiya obyektlərində (function objects) `prototype` xüsusiyyəti (property) olur. Bu deməkdir ki, eyni konstruktorla (constructor) yaradılan bütün obyektlər (objects) eyni sinfin (class) üzvüdür. 🤝

Nümunə 9-2, əvvəlki `Range` sinfini (class) fabrikat funksiyası (factory function) əvəzinə konstruktor funksiyası (constructor function) ilə yenidən qurur. Bu, **ES6 `class` açar sözü (keyword) olmayan versiyalarda sinif (class) yaratmağın ən yaygın (idiomatic) yoludur.** 🎯 Köhnə kodları anlamaq və `class` açar sözünün (keyword) "pərdə arxasında" (under the hood) necə işlədiyini bilmək üçün bunu bilmək vacibdir.

**Nümunə 9-2. Konstruktor (constructor) istifadə edən bir Range sinfi (class)**

```javascript
// Yeni Range obyektlərini (objects) ilkinləşdirən konstruktor funksiyası (constructor function).
// Obyekti (object) yaratmır, sadəcə "this" obyekti ilkinləşdirir.
function Range(from, to) {
    this.from = from; // Başlanğıc nöqtəsi (state)
    this.to = to;     // Son nöqtə (state)
}

// Bütün Range obyektlərinin miras aldığı prototip obyekti (prototype object).
// Xüsusiyyət adı "prototype" olmalıdır!
Range.prototype = {
    includes: function(x) { return this.from <= x && x <= this.to; }, // Diapazonda (range) olub olmadığını yoxlayır.
    [Symbol.iterator]: function*() { // Diapazonu (range) iterable edən generator funksiyası (generator function).
        for(let x = Math.ceil(this.from); x <= this.to; x++)
            yield x;
    },
    toString: function() { return "(" + this.from + "..." + this.to + ")"; } // Simvolik (string) təmsil.
};

// 👇 Nümunə istifadə:
let r = new Range(1,3); // "new" ilə Range obyekti (object) yaradın. 🌟
console.log(r.includes(2));  // => true ✅
console.log(r.toString());   // => "(1...3)" 📝
console.log([...r]);         // => [1, 2, 3] 🔢
```

Nümunə 9-1 və 9-2 arasındakı əsas fərqlər:

1.  **Adlandırma Qaydası (Naming Convention):** Konstruktor funksiyaları (constructor functions) böyük hərflə (`Range()`) başlayır, adi funksiyalar (functions) isə kiçik hərflə (`range()`). Bu, `new`-dən nə zaman istifadə etməli olduğunu göstərir. 📏
2.  **`new` açar sözü (keyword):** Konstruktorlar (constructors) `new` ilə çağırılır, fabrikat funksiyaları (factory functions) isə yox. `new` obyekti (object) avtomatik yaradır və konstruktorun (constructor) sadəcə `this`-i ilkinləşdirməsi kifayətdir.
3.  **Prototip (Prototype) Adı:** Nümunə 9-1-də prototip `range.methods` idi, Nümunə 9-2-də isə mütləq `Range.prototype` olmalıdır. `new` çağırışı avtomatik olaraq konstruktorun `prototype` xüsusiyyətini (property) yeni obyektin (object) prototipi (prototype) kimi istifadə edir. 🔗

Qeyd edək ki, hər iki nümunədə **arrow functions (ox funksiyaları)** istifadə olunmayıb. Niyə? 🚫

* Arrow funksiyalarının (arrow functions) `prototype` xüsusiyyəti (property) yoxdur, bu səbəbdən konstruktor (constructor) kimi istifadə edilə bilməzlər.
* Onlar `this` açar sözünü (keyword) təyin olunduqları kontekstdən (context) miras alırlar, çağırıldıqları obyektə (object) görə yox. Bu, onları metodlar (methods) üçün yararsız edir, çünki metodların (methods) əsas xarakteristikası `this` ilə instansiyaya (instance) müraciət etməsidir. 🙅‍♀️

Xoşbəxtlikdən, yeni **ES6 `class` sintaksisi (syntax)** metodları (methods) arrow funksiyaları (arrow functions) ilə təyin etməyə imkan vermir, beləliklə bu səhvi etmək riski yoxdur. Tezliklə ES6 `class` açar sözünü (keyword) öyrənəcəyik, amma əvvəlcə konstruktorlar (constructors) haqqında daha çox məlumat var. 📚

---

### 9.2.1 Konstruktorlar (Constructors), Sinif Kimliyi (Class Identity) və `instanceof` 🧐

Gördüyümüz kimi, sinfin (class) kimliyi üçün **prototip obyekti (prototype object)** əsasdır: iki obyekt (object) yalnız eyni prototip obyektindən (prototype object) miras alırsa, eyni sinfin (class) instansiyalarıdır (instances). Konstruktor funksiyası (constructor function) vacibdir, lakin sinfin (class) kimliyinin əsası deyil. Bəzən iki fərqli konstruktor (constructor) belə eyni prototipə (prototype) sahib ola bilər. 🤯

Amma konstruktorlar (constructors) sinfin (class) **ictimai siması (public face)** rolunu oynayır. Adətən, konstruktor funksiyasının (constructor function) adı sinfin (class) adı kimi istifadə olunur (məsələn, `Range()` konstruktoru `Range` obyektləri (objects) yaradır). Ən əsası isə, obyektlərin (objects) sinfə (class) aid olub olmadığını yoxlamaq üçün `instanceof` operatorunda istifadə olunurlar.

Əgər `r` adlı bir obyektimiz (object) varsa və onun `Range` obyekti (object) olub olmadığını bilmək istəyiriksə, belə yaza bilərik:

```javascript
r instanceof Range // => true: "r" Range.prototype-dən miras alır. ✅
```

`instanceof` operatoru obyektin (object) (sol tərəf) bir konstruktorun (constructor) (sağ tərəf) prototip zəncirində (prototype chain) olub olmadığını yoxlayır. Yəni, `o instanceof C` ifadəsi, `o` obyekti (object) `C.prototype`-dan miras alırsa, `true` qaytarır. Bu irsiyyət (inheritance) birbaşa olmaya da bilər, yəni prototip zəncirinin (prototype chain) hər hansı bir pilləsində `C.prototype` varsa, nəticə yenə də `true` olacaq. ✨

**Texniki Detal:** `instanceof` operatoru `r` obyektinin (object) həqiqətən də `Range` konstruktoru (constructor) tərəfindən ilkinləşdirilib (initialized) olmadığını yoxlamır. Sadəcə, `r`-in `Range.prototype`-dan miras alıb-almadığını yoxlayır.

**Nümunə:** Əgər biz `Strange()` adlı bir funksiya təyin edib, onun prototipini (prototype) `Range.prototype` ilə eyniləşdirsək, `new Strange()` ilə yaradılan obyektlər (objects) `instanceof` üçün `Range` obyektləri (objects) sayılacaq (baxmayaraq ki, `from` və `to` xüsusiyyətləri (properties) ilkinləşmədiyi üçün əslində `Range` obyekti (object) kimi işləməyəcəklər):

```javascript
function Strange() {}
Strange.prototype = Range.prototype; // Strange-in prototipini Range-in prototipi ilə eyniləşdiririk.
new Strange() instanceof Range // => true 🤯
```
`instanceof` konstruktorun (constructor) istifadə olunmasını yoxlaya bilməsə də, yenə də sağ tərəfində konstruktor funksiyası (constructor function) istifadə edir, çünki konstruktorlar (constructors) sinfin (class) ictimai kimliyidir. 🆔

Əgər bir obyektin (object) prototip zəncirində (prototype chain) müəyyən bir prototipi (prototype) yoxlamaq istəyirsinizsə və konstruktor funksiyasından (constructor function) vasitəçi kimi istifadə etmək istəmirsinizsə, **`isPrototypeOf()` metodundan** istifadə edə bilərsiniz.

Məsələn, Nümunə 9-1-də konstruktor funksiyası (constructor function) olmayan bir sinif (class) təyin etmişdik, buna görə də `instanceof` ilə onu yoxlamaq mümkün deyildi. Bunun əvəzinə, bir obyektin (object) `r` həmin konstruktorsuz (constructor-less) sinfin (class) üzvü olub olmadığını bu kodla yoxlaya bilərdik:

```javascript
range.methods.isPrototypeOf(r); // "range.methods" prototip obyektidir. ✅
```

---

### 9.2.2 `constructor` Xüsusiyyəti (Property) 🏗️

Nümunə 9-2-də biz `Range.prototype`-u sinfimizin (class) metodlarını (methods) ehtiva edən yeni bir obyektə (object) təyin etdik. Bu rahat olsa da, əslində yeni bir obyekt (object) yaratmaq məcburiyyəti yox idi.

Hər bir adi JavaScript funksiyası (arrow, generator və async funksiyalar istisna olmaqla) konstruktor (constructor) kimi istifadə oluna bilər. Konstruktor çağırışları (constructor invocations) isə `prototype` xüsusiyyətinə (property) ehtiyac duyur. Buna görə də, hər adi JavaScript funksiyasının (function) avtomatik olaraq bir `prototype` xüsusiyyəti (property) var. Bu `prototype` xüsusiyyətinin (property) dəyəri (value) tək, sayıla bilməyən (non-enumerable) bir `constructor` xüsusiyyətinə (property) malik obyektdir (object). Bu `constructor` xüsusiyyətinin (property) dəyəri (value) isə funksiyanın (function) özüdür:

```javascript
let F = function() {}; // Bu, bir funksiya obyektidir (function object).
let p = F.prototype;   // F ilə əlaqəli prototip obyektidir (prototype object).
let c = p.constructor; // Prototip (prototype) ilə əlaqəli funksiyadır.
c === F;               // => true: Hər hansı bir F üçün F.prototype.constructor === F olur. ✅
```

Bu, əvvəlcədən təyin edilmiş `constructor` xüsusiyyəti (property) olan prototip obyektinin (prototype object) mövcudluğu, obyektlərin (objects) adətən öz konstruktorlarına (constructors) istinad edən bir `constructor` xüsusiyyətini (property) miras alması deməkdir. Konstruktorlar (constructors) sinfin (class) ictimai kimliyi kimi çıxış etdiyindən, bu `constructor` xüsusiyyəti (property) obyektin (object) sinfini (class) bildirir:

```javascript
let o = new F(); // F sinfinin (class) "o" obyektini (object) yaradırıq.
o.constructor === F; // => true: "constructor" xüsusiyyəti (property) sinfi (class) göstərir. 🎯
```



**Şəkil 9-1** bu əlaqəni daha aydın göstərir:

<img width="512" alt="image" src="https://github.com/user-attachments/assets/d31cf59a-baa3-4135-a90b-78127b773854" />


*Şəkil 9-1. Konstruktor funksiyası (constructor function), onun prototipi (prototype) və instansiyaları (instances)*

Şəkildə bizim `Range()` konstruktoru (constructor) nümunə kimi istifadə olunsa da, Nümunə 9-2-dəki `Range` sinfi (class) əvvəlcədən təyin edilmiş `Range.prototype` obyektini (object) öz obyektimizlə yenidən yazır. Bu yeni prototip obyektinin (prototype object) isə `constructor` xüsusiyyəti (property) yoxdur. Bu səbəbdən, bu cür təyin olunmuş `Range` sinfinin (class) instansiyalarının (instances) `constructor` xüsusiyyəti (property) olmur. Bunu düzəltmək üçün prototipə (prototype) açıq şəkildə `constructor` əlavə edə bilərik:

```javascript
Range.prototype = {
    constructor: Range, // Konstruktoru (constructor) açıq şəkildə təyin edirik (geriyə istinad – back-reference).
    /* metod təyinatları (method definitions) buraya daxildir */
};
```

Köhnə JavaScript kodlarında görə biləcəyiniz başqa bir üsul isə, əvvəlcədən təyin edilmiş `constructor` xüsusiyyəti (property) olan prototip obyektindən (prototype object) istifadə edib, metodları (methods) bir-bir ona əlavə etməkdir:

```javascript
// Əvvəlcədən yaradılmış Range.prototype.constructor xüsusiyyətini (property) silməmək üçün
// əvvəlcədən təyin edilmiş Range.prototype obyektini (object) genişləndirin.
Range.prototype.includes = function(x) {
    return this.from <= x && x <= this.to;
};
Range.prototype.toString = function() {
    return "(" + this.from + "..." + this.to + ")";
};
```

Bu şəkildə, `constructor` xüsusiyyətini (property) qoruyursunuz. 🛡️

-----

### 9.3 `class` Açar Sözü (Keyword) ilə Siniflər (Classes) 🚀

JavaScript-də siniflər (classes) həmişə olub, lakin ES6 ilə nəhayət ki, öz sintaksisini (`class` açar sözünü – keyword) qazandılar. Nümunə 9-3, `Range` sinfimizin (class) bu yeni sintaksislə (syntax) necə göründüyünü göstərir.

**Nümunə 9-3. `class` istifadə edərək yenidən yazılmış `Range` sinfi (class)**

```javascript
class Range { // 👈 "class" açar sözü (keyword) ilə sinfi (class) təyin edirik
    constructor(from, to) { // ✨ Konstruktor (constructor) burada təyin olunur
        // Yeni "range" obyektinin (object) vəziyyətini (state) saxlayın.
        this.from = from;
        this.to = to;
    }

    // Metodlar (methods) - funksiya açar sözü (function keyword) yoxdur!
    includes(x) { // Diapazonda (range) olub olmadığını yoxlayır.
        return this.from <= x && x <= this.to;
    }

    *[Symbol.iterator]() { // Diapazonu (range) iterable edən generator funksiyası (generator function).
        for(let x = Math.ceil(this.from); x <= this.to; x++)
            yield x;
    }

    toString() { // Simvolik (string) təmsil.
        return `(${this.from}...${this.to})`;
    }
}

// 👇 Yeni Range sinfinin (class) istifadə nümunələri (examples):
let r = new Range(1,3);      // Range obyekti (object) yaradın.
console.log(r.includes(2));  // => true ✅
console.log(r.toString());   // => "(1...3)" 📝
console.log([...r]);         // => [1, 2, 3] 🔢
```

Unutmayın ki, Nümunə 9-2 və 9-3-də təyin edilmiş siniflər (classes) **tamamilə eyni şəkildə işləyir.** `class` açar sözünün (keyword) gəlişi JavaScript-in prototip-əsaslı siniflərinin (prototype-based classes) əsas təbiətini dəyişmir. Yeni `class` sintaksisi (syntax) sadəcə Nümunə 9-2-dəki daha fundamental sinif (class) təyini mexanizminin "sintaktik şəkəri" (syntactic sugar) kimidir. 🍬

**`class` sintaksisi (syntax) haqqında vacib qeydlər:**

* **Təyinat:** Sinif (class) `class` açar sözü (keyword), sinfin (class) adı və fiqurlu mötərizələrlə (`{}`) təyin olunur.
* **Metodlar (Methods):** Sinif gövdəsindəki (class body) metodlar (methods) obyekt literal qısa metod sintaksisindən (object literal method shorthand) istifadə edir (funksiya açar sözü – function keyword atılır). Amma obyekt literallarından (object literals) fərqli olaraq, metodlar (methods) arasında vergül (comma) olmur.
* **`constructor` açar sözü (keyword):** Bu açar sözü (keyword) sinfin (class) konstruktor funksiyasını (constructor function) təyin etmək üçün istifadə olunur. Əslində funksiyanın (function) adı "constructor" deyil, sadəcə **`Range`** adında yeni bir dəyişən (variable) yaradılır və bu xüsusi konstruktor funksiyası (constructor function) ona mənimsədilir.
* **Boş Konstruktor (Empty Constructor):** Əgər sinfinizin (class) heç bir ilkinləşdirməyə (initialization) ehtiyacı yoxdursa, `constructor` açar sözünü (keyword) və gövdəsini (body) tamamilə ata bilərsiniz; boş bir konstruktor funksiyası (constructor function) sizin üçün avtomatik yaradılacaq. 👻
* **İrsiyyət (`extends`):** Başqa bir sinifdən (class) miras alan (subclass) sinif (class) təyin etmək üçün `extends` açar sözündən (keyword) istifadə olunur:

    ```javascript
    // "Span" "Range" kimidir, lakin başlanğıc (start) və uzunluq (length) ilə ilkinləşdirilir.
    class Span extends Range { // 👈 "Range" sinfindən (class) miras alır
        constructor(start, length) {
            if (length >= 0) {
                super(start, start + length); // Üst sinfin (superclass) konstruktorunu çağırır.
            } else {
                super(start + length, start);
            }
        }
    }
    ```
    Alt-siniflər (subclasses) ayrı bir mövzudur. `extends` və `super` açar sözləri (keywords) haqqında §9.5-də daha ətraflı danışacağıq. 📚

Funksiya təyinatları (function declarations) kimi, sinif təyinatları (class declarations) da bəyanat (statement) və ifadə (expression) formalarına malikdir:

```javascript
let Square = class { // İfadə (expression) forması
    constructor(x) {
        this.area = x * x;
    }
};
new Square(3).area; // => 9 ✅
```
Funksiya ifadələri (function expressions) çox yaygın olsa da, sinif təyinat ifadələri (class definition expressions) o qədər də tez-tez istifadə edilmir, yalnız bir sinfi (class) arqument (argument) kimi alan və alt-sinif (subclass) qaytaran bir funksiya (function) yazırsınızsa.

**`class` açar sözü (keyword) ilə bağlı vacib məqamlar:**

* **Sərt Rejim (`strict mode`):** Sinif təyinatının (class declaration) gövdəsindəki bütün kod avtomatik olaraq **sərt rejimdə (strict mode)** işləyir, hətta `"use strict"` direktivi olmasa belə (§5.6.3). Bu, məsələn, dəyişənləri (variables) elan etmədən istifadə etsəniz səhv (error) ala biləcəyiniz deməkdir. 🚫
* **`Hoisting` yoxdur:** Funksiya təyinatlarından (function declarations) fərqli olaraq, sinif təyinatları (class declarations) **`hoisted` (yuxarı qaldırılmır)** olmur. Yəni, sinfi (class) elan etmədən əvvəl ondan instansiya (instance) yarada bilməzsiniz. ❌

---

### 9.3.1 Statik Metodlar (Static Methods) 🧊

Sinif (class) gövdəsi (body) daxilində **`static`** açar sözünü (keyword) istifadə edərək statik metod (static method) təyin edə bilərsiniz. Statik metodlar (static methods) prototip obyektinin (prototype object) deyil, **konstruktor funksiyasının (constructor function)** xüsusiyyəti (property) kimi təyin olunur.

**Nümunə:** Təsəvvür edək ki, Nümunə 9-3-dəki `Range` sinfinə (class) aşağıdakı kodu əlavə etdik:

```javascript
class Range {
    // ... əvvəlki kodlar ...

    static parse(s) { // 👈 "static" açar sözü (keyword) ilə statik metod (static method) təyin edirik
        let matches = s.match(/^\((\d+)\.\.\.(\d+)\)$/);
        if (!matches) {
            throw new TypeError(`Cannot parse Range from "${s}".`);
        }
        return new Range(parseInt(matches[1]), parseInt(matches[2]));
    }
}
```

Bu kodla təyin olunan metod `Range.parse()`-dır, `Range.prototype.parse()` deyil. Yəni, onu bir instansiya (instance) üzərində yox, birbaşa konstruktor (constructor) (sinfin adı) vasitəsilə çağırmalısınız:

```javascript
let r = Range.parse('(1...10)'); // ✅ Düzgün: Yeni bir Range obyekti (object) qaytarır.
r.parse('(1...10)');             // ❌ Səhv: TypeError: r.parse funksiya deyil.
```

Statik metodlar (static methods) bəzən **sinif metodları (class methods)** da adlanır, çünki onlar sinfin (class) adı / konstruktorun (constructor) adı ilə çağırılır. Bu termin, statik metodları (static methods) instansiyalar (instances) üzərində çağırılan adi instansiya metodlarından (instance methods) fərqləndirmək üçün istifadə olunur. Statik metodlar (static methods) heç bir xüsusi instansiya (instance) üzərində çağırılmadığı üçün, onlarda `this` açar sözünü (keyword) istifadə etmək demək olar ki, heç vaxt məna kəsb etmir. ⚠️

Nümunə 9-4-də statik metodların (static methods) başqa nümunələrini görəcəyik.

---

### 9.3.2 Getters (Alıcılar), Setters (Təyin edicilər) və Digər Metod Formaları ⚙️

Sinif (class) gövdəsi (body) daxilində, obyekt literallarında (object literals) olduğu kimi, **getter (alıcı)** və **setter (təyin edici)** metodları (§6.10.6) təyin edə bilərsiniz. Yeganə fərq odur ki, sinif (class) gövdələrində (bodies) getter (alıcı) və ya setter (təyin edici) metodundan (method) sonra vergül (comma) qoyulmur.

Nümunə 9-4-də sinifdə (class) getter metoduna (getter method) dair praktik bir nümunə var.

Ümumiyyətlə, obyekt literallarında (object literals) icazə verilən bütün qısa metod təyini sintaksisləri (shorthand method definition syntaxes) sinif (class) gövdələrində (bodies) də icazəlidir. Buna **generator metodları (generator methods)** (`*` ilə işarələnir) və adları kvadrat mötərizədəki (square brackets) bir ifadənin (expression) dəyəri (value) olan metodlar (methods) da daxildir. Əslində, siz artıq Nümunə 9-3-də `Range` sinfini (class) iterable edən, hesablanmış ada (computed name) malik bir generator metodu (generator method) görmüsünüz:

```javascript
*[Symbol.iterator]() { // 👈 Generator metodu (generator method) hesablanmış adla (computed name)
    for(let x = Math.ceil(this.from); x <= this.to; x++)
        yield x;
}
```

---

### 9.3.3 Public (İctimai), Private (Şəxsi) və Statik Sahələr (Fields) 🧱

`class` açar sözü (keyword) ilə sinif (class) təyinatlarında əvvəllər yalnız metodları (methods) (getter, setter, generator daxil olmaqla) və statik metodları (static methods) müəyyən edə bilirdik. Sahələri (fields) (obyekt-yönümlü proqramlaşdırmada "xüsusiyyət" (property) deməkdir) birbaşa sinif (class) gövdəsində (body) təyin etmək üçün ES6-da sintaksis (syntax) yox idi.

* **Instansiya sahələri (Instance fields)** üçün konstruktorda (constructor) və ya metodlarda (methods) təyin etməliydiniz.
* **Statik sahələri (Static fields)** isə sinif gövdəsindən (class body) kənarda, sinif (class) təyin edildikdən sonra müəyyən etməli idiniz. Nümunə 9-4 hər ikisinə aid nümunələr ehtiva edir.

Lakin, **yeni standartlaşma prosesi** davam edir! Artıq **ictimai (public)** və **şəxsi (private)**, həm instansiya (instance), həm də statik sahələri (static fields) sinif (class) gövdəsində (body) təyin etməyə imkan verən sintaksislər (syntaxes) mövcuddur. (Qeyd: 2025-ci ilin iyun ayı üçün bu xüsusiyyətlər artıq geniş şəkildə dəstəklənir.)

**Yeni instansiya sahəsi (instance field) sintaksisi:**

Əvvəllər belə yazılırdı:
```javascript
class Buffer {
    constructor() {
        this.size = 0;
        this.capacity = 4096;
        this.buffer = new Uint8Array(this.capacity);
    }
}
```
İndi isə daha təmiz şəkildə belə yaza bilərik:
```javascript
class Buffer {
    size = 0;             // 👈 Public instansiya sahəsi (field)
    capacity = 4096;      // 👈 Public instansiya sahəsi (field)
    buffer = new Uint8Array(this.capacity); // 👈 Public instansiya sahəsi (field)
}
```
Sahələrin (fields) ilkinləşdirilməsi (initialization) konstruktordan (constructor) sinif gövdəsinə (class body) keçib. (Bu kod hələ də konstruktorun (constructor) bir hissəsi kimi işləyir.) `this.` prefiksləri (prefixes) artıq təyinatın (assignment) sol tərəfində yoxdur, amma sağ tərəfdə hələ də `this.` istifadə etməlisiniz. Bu yeni sintaksisin (syntax) üstünlüyü odur ki, sahələri (fields) sinif təyinatının (class definition) yuxarı hissəsində qeyd etməyə imkan verir, bu da oxucu üçün sinfin (class) hansı sahələrinin (fields) vəziyyəti (state) saxladığını aydınlaşdırır. 💡 Sahəni (field) ilkinləşdirici (initializer) olmadan da elan edə bilərsiniz (dəyəri (value) `undefined` olacaq), lakin hər zaman açıq dəyər (explicit value) vermək daha yaxşı üslubdur.

Bu sahə (field) sintaksisi (syntax) sinif (class) gövdələrinin (bodies) obyekt literallarından (object literals) tamamilə fərqli olduğunu aydınlaşdırır (bərabərlik işarələri (equals signs) və nöqtəli vergüllər (semicolons) iki nöqtə (colons) və vergüllər (commas) əvəzinə).

**Şəxsi (Private) instansiya sahələri (fields):**

Eyni təklif **şəxsi (private) instansiya sahələrini (fields)** də təyin edir. Əgər bir sahənin (field) adı `#` (JavaScript identifikatorlarında (identifiers) qanuni simvol deyil) ilə başlayırsa, o sahə (field) sinif (class) gövdəsi (body) daxilində `#` prefiksi (prefix) ilə istifadə edilə bilər, lakin sinif (class) xaricində görünməz və əlçatmaz (və buna görə də dəyişdirilməz) olacaq. 🔐

**Nümunə:** `Buffer` sinfində (class) `size` sahəsinin (field) təsadüfən dəyişdirilməsinin qarşısını almaq üçün şəxsi `#size` sahəsindən (field) istifadə edə bilərik və dəyərə (value) yalnız oxuma (read-only) girişi təmin etmək üçün bir getter funksiyası (getter function) təyin edə bilərik:

```javascript
class Buffer {
    #size = 0;            // 👈 Şəxsi (private) instansiya sahəsi (field)
    get size() { return this.#size; } // Oxumaq üçün getter (alıcı)
}
```
**Qeyd:** Şəxsi sahələr (private fields) istifadə olunmazdan əvvəl bu yeni sintaksis (syntax) ilə elan edilməlidir. Konstruktorda (constructor) sadəcə `this.#size = 0;` yaza bilməzsiniz, əgər sahəni (field) sinif (class) gövdəsində (body) "elan etməsəniz".

**Statik sahələr (Static fields):**

Bir digər təklif **`static`** açar sözünün (keyword) sahələr (fields) üçün standartlaşdırılmasını hədəfləyir. Əgər ictimai (public) və ya şəxsi (private) sahənin (field) elanından (declaration) əvvəl `static` əlavə etsəniz, bu sahələr (fields) instansiyaların (instances) deyil, **konstruktor funksiyasının (constructor function) xüsusiyyəti (property) kimi yaradılacaq.** 📦

Məsələn, əvvəlki `Range.parse()` statik metodumuzda mürəkkəb bir `regex` var idi. Yeni statik sahə (static field) sintaksisi (syntax) ilə onu ayrıca bir statik sahəyə (static field) çıxara bilərik:

```javascript
class Range {
    // ... digər kodlar ...

    static integerRangePattern = /^\((\d+)\.\.\.(\d+)\)$/; // 👈 Statik sahə (static field)

    static parse(s) {
        let matches = s.match(Range.integerRangePattern); // Statik sahəyə (static field) müraciət
        if (!matches) {
            throw new TypeError(`Cannot parse Range from "${s}".`);
        }
        return new Range(parseInt(matches[1]), parseInt(matches[2]));
    }
}
```
Əgər bu statik sahənin (static field) yalnız sinif (class) daxilində əlçatan olmasını istəsəydik, onu `#pattern` kimi bir ad istifadə edərək şəxsi (private) edə bilərdik.

---

### 9.3.4 Nümunə: Kompleks Ədəd Sinifi (Complex Number Class) 🧮

Nümunə 9-4 kompleks ədədləri (complex numbers) təmsil edən bir sinif (class) təyin edir. Bu sinif (class) nisbətən sadədir, lakin **instansiya metodları (instance methods)** (getter-lər (getters) daxil olmaqla), **statik metodlar (static methods)**, **instansiya sahələri (instance fields)** və **statik sahələri (static fields)** ehtiva edir. Həmçinin, sinif (class) gövdəsi (body) daxilində hələ standartlaşmamış instansiya (instance) və statik sahələri (static fields) təyin etmək üçün necə istifadə edə biləcəyimizi göstərən şərhlənmiş kod (commented-out code) da var.

**Nümunə 9-4. Complex.js: Kompleks ədəd sinfi (class)**

```javascript
/**
 * Bu Complex sinfinin (class) instansiyaları (instances) kompleks ədədləri (complex numbers) təmsil edir.
 * Xatırladaq ki, kompleks ədəd real (real) və xəyali (imaginary) ədədin cəmidir.
 */
class Complex {
    // #r = 0; // Standardlaşarsa, şəxsi (private) instansiya sahələri (fields) belə olacaq.
    // #i = 0;

    // Konstruktor (constructor) - hər instansiyada (instance) "r" (real) və "i" (imaginary) sahələrini (fields) təyin edir.
    constructor(real, imaginary) {
        this.r = real;      // Ədədin real hissəsi.
        this.i = imaginary; // Ədədin xəyali hissəsi.
    }

    // Instansiya metodları (instance methods) - kompleks ədədlərin toplanması və vurulması.
    plus(that) {
        return new Complex(this.r + that.r, this.i + that.i);
    }
    times(that) {
        return new Complex(this.r * that.r - this.i * that.i,
                           this.r * that.i + this.i * that.r);
    }

    // Statik metodlar (static methods) - kompleks əməliyyatların sinif səviyyəsindəki variantları.
    static sum(c, d) { return c.plus(d); }
    static product(c, d) { return c.times(d); }

    // Getter metodları (getter methods) - sahələr (fields) kimi istifadə olunur.
    get real() { return this.r; }
    get imaginary() { return this.i; }
    get magnitude() { return Math.hypot(this.r, this.i); } // Hipotenusu hesablayır.

    // toString() metodu (method) - siniflər (classes) üçün demək olar ki, həmişə lazımdır.
    toString() { return `{${this.r},${this.i}}`; }

    // İki instansiyanın (instances) eyni dəyəri (value) təmsil edib etmədiyini yoxlayır.
    equals(that) {
        return that instanceof Complex &&
               this.r === that.r &&
               this.i === that.i;
    }

    // static ZERO = new Complex(0,0); // Statik sahələr (static fields) dəstəklənəndə belə olacaq.
}

// Faydalı, əvvəlcədən təyin edilmiş kompleks ədədləri saxlayan sinif sahələri (class fields).
Complex.ZERO = new Complex(0,0);
Complex.ONE = new Complex(1,0);
Complex.I = new Complex(0,1);

// 👇 "Complex" sinfinin (class) konstruktor (constructor), instansiya (instance) və statik sahələri (static fields) / metodları (methods) ilə istifadə nümunələri (examples):
let c = new Complex(2, 3);    // Yeni obyekt (object) yaradın.
let d = new Complex(c.i, c.r); // "c"-nin instansiya sahələrindən (instance fields) istifadə.
console.log(c.plus(d).toString()); // => "{5,5}"; instansiya metodlarından (instance methods) istifadə.
console.log(c.magnitude);          // => Math.hypot(2,3) (təqribən 3.6); getter funksiyasından (function) istifadə.
console.log(Complex.product(c, d).toString()); // => "{0,13}"; statik metoddan (static method) istifadə.
console.log(Complex.ZERO.toString()); // => "{0,0}"; statik xüsusiyyət (static property).
```

---


### 9.4 Mövcud Siniflərə (Classes) Metodlar (Methods) Əlavə Etmək ➕

JavaScript-in **prototip-əsaslı irsiyyət (prototype-based inheritance)** mexanizmi dinamikdir. Bu o deməkdir ki, bir obyekt (object) öz prototipindən (prototype) xüsusiyyətləri (properties) miras alır, hətta prototipin (prototype) xüsusiyyətləri (properties) obyekt (object) yaradıldıqdan sonra dəyişsə belə. ✨ Bu xüsusiyyət sayəsində, biz JavaScript siniflərini (classes) sadəcə onların prototip obyektlərinə (prototype objects) yeni metodlar (methods) əlavə etməklə genişləndirə bilərik.

**Nümunə:** Nümunə 9-4-dəki `Complex` sinfinə (class) kompleks qoşma (complex conjugate) hesablamaq üçün metod (method) əlavə edək:

```javascript
// Bu kompleks ədədin kompleks qoşmasını (complex conjugate) qaytarır.
Complex.prototype.conj = function() {
    return new Complex(this.r, -this.i);
};

// İndi bir nümunəyə baxaq:
let z = new Complex(3, 4); // Z = 3 + 4i
let conj_z = z.conj();    // conj_z = 3 - 4i
console.log(conj_z.toString()); // Çıxış: {3,-4} ✅
```

JavaScript-in daxili (built-in) siniflərinin (classes) prototip obyektləri (prototype objects) də bu şəkildə açıqdır. Bu o deməkdir ki, biz `Number`, `String`, `Array`, `Function` və s. kimi obyektlərə metodlar (methods) əlavə edə bilərik. Bu, dilin köhnə versiyalarında yeni dil xüsusiyyətlərini (features) tətbiq etmək üçün faydalı ola bilər:

```javascript
// Əgər yeni String metodu (method) startsWith() artıq təyin olunmayıbsa...
if (!String.prototype.startsWith) {
    // ...o zaman onu köhnə indexOf() metodu (method) ilə belə təyin edin.
    String.prototype.startsWith = function(s) {
        return this.indexOf(s) === 0;
    };
}

// İndi istifadə edə bilərik:
console.log("Salam dunya".startsWith("Salam")); // => true ✅
```

Başqa bir nümunə:

```javascript
// F funksiyasını (function) "bu" qədər dəfə çağırın, iterasiya nömrəsini ötürərək.
// Məsələn, "hello" 3 dəfə çap etmək üçün:
// let n = 3;
// n.times(i => { console.log(`hello ${i}`); });
Number.prototype.times = function(f, context) {
    let n = this.valueOf(); // Sayı (number) əldə edirik.
    for(let i = 0; i < n; i++) {
        f.call(context, i); // F funksiyasını çağırırıq.
    }
};

// Nümunə istifadəsi:
let num = 3;
num.times(i => console.log(`Salam ${i+1}`)); // Çıxış: Salam 1, Salam 2, Salam 3
```

**Vacib Xəbərdarlıq! ⚠️**
Daxili (built-in) tiplərin (types) prototiplərinə (prototypes) bu cür metodlar (methods) əlavə etmək ümumiyyətlə **pis ideya** hesab olunur. Niyə? Çünki gələcəkdə JavaScript-in yeni bir versiyası eyni adlı bir metod (method) təyin edərsə, bu, qarışıqlığa (confusion) və uyğunsuzluq (compatibility) problemlərinə səbəb ola bilər.

Hətta `Object.prototype`-a da metodlar (methods) əlavə etmək mümkündür ki, bu da onları bütün obyektlər (objects) üçün əlçatan edər. Lakin bu, heç vaxt yaxşı fikir deyil, çünki `Object.prototype`-a əlavə olunan xüsusiyyətlər (properties) `for/in` dövrlərində (loops) görünür (baxmayaraq ki, yeni xüsusiyyəti (property) sayıla bilməyən (non-enumerable) etmək üçün `Object.defineProperty()` [§14.1] istifadə edərək bunun qarşısını ala bilərsiniz). Ümumiyyətlə, **daxili obyektlərin (built-in objects) prototiplərinə dəyişiklik etməkdən çəkinin.** 🚫

---



### 9.5 Alt-siniflər (Subclasses) 🐣➡️🦆

Obyekt-yönümlü proqramlaşdırmada, bir sinif (class) (B) başqa bir sinfi (class) (A) genişləndirə (extend) və ya alt-sinif (subclass) ola bilər. Burada A **üst-sinif (superclass)**, B isə **alt-sinif (subclass)** adlanır. B-nin instansiyaları (instances) A-nın metodlarını (methods) miras alır. B həmçinin öz metodlarını (methods) təyin edə bilər ki, bunlardan bəziləri A-nın eyni adlı metodlarını (methods) **əvəz edə (override)** bilər.

Əgər B-dəki bir metod (method) A-dakı metodu (method) əvəz edirsə, B-dəki metodun (method) tez-tez A-dakı əvəz olunmuş metodu (method) çağırması lazım gəlir. Həmçinin, alt-sinif konstruktoru (subclass constructor) (B()) adətən instansiyaların (instances) tam ilkinləşməsini (initialization) təmin etmək üçün üst-sinif konstruktorunu (superclass constructor) (A()) çağırmalıdır.

Bu bölmə əvvəlcə alt-sinifləri (subclasses) köhnə (ES6-dan əvvəlki) üsulla necə təyin etməyinizi göstərəcək, sonra isə `class`, `extends` açar sözləri (keywords) və `super` açar sözü (keyword) ilə üst-sinif (superclass) konstruktor (constructor) / metod (method) çağırışını nümayiş etdirəcək. Daha sonra alt-siniflərdən (subclasses) qaçınmaq və irsiyyət (inheritance) əvəzinə obyekt kompozisiyasına (object composition) üstünlük vermək haqqında danışılacaq. Bölmə, `Set` siniflərinin (classes) iyerarxiyasını (hierarchy) təyin edən və **abstrakt siniflərin (abstract classes)** interfeysi (interface) implementasiyadan (implementation) necə ayırdığını göstərən geniş bir nümunə ilə bitəcək.

---

### 9.5.1 Alt-siniflər (Subclasses) və Prototiplər (Prototypes) 🔗

Təsəvvür edin ki, Nümunə 9-2-dəki `Range` sinfinin (class) `Span` adlı bir alt-sinfini (subclass) təyin etmək istəyirik. Bu alt-sinif (subclass) `Range` kimi işləyəcək, lakin `start` və `end` əvəzinə `start` və `distance` (və ya `span`) ilə ilkinləşdiriləcək. `Span` instansiyası (instance) həm də `Range` üst-sinfinin (superclass) instansiyası (instance) olacaq.

`Span` instansiyası (instance) `Span.prototype`-dan özelleşdirilmiş `toString()` metodunu (method) miras alacaq, lakin `Range` sinfinin (class) alt-sinfi (subclass) olması üçün `Range.prototype`-dan da metodları (methods) (məsələn, `includes()`) miras almalıdır.

**Nümunə 9-5. Span.js: `Range`-in sadə alt-sinfi (subclass)**

```javascript
// Alt-sinifimizin (subclass) konstruktor funksiyası (constructor function).
function Span(start, span) {
    if (span >= 0) {
        this.from = start;
        this.to = start + span;
    } else {
        this.to = start;
        this.from = start + span;
    }
}

// Span.prototype-ın Range.prototype-dan miras almasını təmin edin.
Span.prototype = Object.create(Range.prototype);

// Range.prototype.constructor-u miras almaq istəmirik, öz konstruktor (constructor) xüsusiyyətimizi (property) təyin edirik.
Span.prototype.constructor = Span;

// Öz toString() metodunu (method) təyin edərək, Span Range-dən miras alacağı toString() metodunu (method) əvəz edir (override).
Span.prototype.toString = function() {
    return `(${this.from}... +${this.to - this.from})`;
};
```

`Span`-ı `Range`-in alt-sinfi (subclass) etmək üçün `Span.prototype`-ın `Range.prototype`-dan miras almasını təmin etməliyik. Yuxarıdakı kodda əsas sətir budur: `Span.prototype = Object.create(Range.prototype);`. Əgər bu sətir sizin üçün məna kəsb edirsə, JavaScript-də alt-siniflərin (subclasses) necə işlədiyini başa düşürsünüz. 🧠

`Span()` konstruktoru (constructor) ilə yaradılan obyektlər (objects) `Span.prototype` obyektindən (object) miras alacaq. Amma biz həmin obyekti (object) `Range.prototype`-dan miras alması üçün yaratdıq, beləliklə `Span` obyektləri (objects) həm `Span.prototype`-dan, həm də `Range.prototype`-dan miras alacaq.

Fərq edə bilərsiniz ki, `Span()` konstruktoru (constructor) `Range()` konstruktorunun (constructor) etdiyi eyni `from` və `to` xüsusiyyətlərini (properties) təyin edir, buna görə də yeni obyekti (object) ilkinləşdirmək (initialize) üçün `Range()` konstruktorunu (constructor) çağırmağa ehtiyac qalmır. Eynilə, `Span`-ın `toString()` metodu (method) `Range`-in `toString()` versiyasını (version) çağırmadan simvolik (string) çevirməni tamamilə yenidən icra edir. Bu, `Span`-ı xüsusi bir hal edir və biz ancaq üst-sinfin (superclass) implementasiya (implementation) detallarını bildiyimiz üçün bu cür alt-sinifləməyə (subclassing) nail ola bilirik. Sağlam bir alt-sinifləmə (subclassing) mexanizmi siniflərə (classes) öz üst-sinifinin (superclass) metodlarını (methods) və konstruktorunu (constructor) çağırmağa imkan verməlidir, lakin ES6-dan əvvəl JavaScript-də bunları etmək üçün sadə bir yol yox idi.

Xoşbəxtlikdən, ES6 bu problemləri `class` sintaksisinin (syntax) bir hissəsi olaraq **`super` açar sözü (keyword)** ilə həll edir. Növbəti bölmə bunun necə işlədiyini göstərir.

---

### 9.5.2 `extends` və `super` ilə Alt-siniflər (Subclasses) 🦸‍♀️

ES6 və daha sonra, bir sinif (class) təyinatına `extends` bəndi (clause) əlavə etməklə asanlıqla bir alt-sinif (subclass) yarada bilərsiniz. Bunu hətta daxili (built-in) siniflər (classes) üçün də edə bilərsiniz:

```javascript
// İlk və son elementlər üçün getter-lər (getters) əlavə edən sadə bir Array alt-sinfi (subclass).
class EZArray extends Array { // 👈 Array sinfindən (class) miras alır
    get first() { return this[0]; }
    get last() { return this[this.length-1]; }
}

let a = new EZArray();
a instanceof EZArray; // => true: "a" alt-sinif (subclass) instansiyasıdır (instance).
a instanceof Array;   // => true: "a" həm də üst-sinif (superclass) instansiyasıdır (instance).
a.push(1,2,3,4);      // a.length == 4; miras alınan metodları (methods) istifadə edə bilirik.
a.pop();              // => 4: Başqa bir miras alınan metod (method).
a.first;              // => 1: Alt-sinif (subclass) tərəfindən təyin edilmiş ilk getter (alıcı).
a.last;               // => 3: Alt-sinif (subclass) tərəfindən təyin edilmiş son getter (alıcı).
a[1];                 // => 2: Adi massiv (array) girişi sintaksisi (syntax) hələ də işləyir.
Array.isArray(a);     // => true: Alt-sinif (subclass) instansiyası (instance) həqiqətən də massivdir (array).
EZArray.isArray(a);   // => true: Alt-sinif (subclass) statik metodları (static methods) da miras alır! 🤩
```

Bu `EZArray` alt-sinfi (subclass) iki sadə getter metodu (getter methods) təyin edir. `EZArray` instansiyaları (instances) adi massivlər (arrays) kimi davranır və biz `push()`, `pop()` və `length` kimi miras alınan metodları (methods) və xüsusiyyətləri (properties) istifadə edə bilirik. Həmçinin alt-sinifdə (subclass) təyin edilmiş `first` və `last` getterlərini (getters) də istifadə edə bilirik. Təkcə `pop()` kimi instansiya metodları (instance methods) deyil, `Array.isArray` kimi statik metodlar (static methods) da miras alınır. Bu, ES6 `class` sintaksisinin (syntax) yeni bir xüsusiyyətidir: `EZArray()` bir funksiyadır, lakin `Array()`-dən miras alır:

```javascript
// EZArray instansiya metodlarını (instance methods) miras alır, çünki EZArray.prototype
// Array.prototype-dan miras alır.
Array.prototype.isPrototypeOf(EZArray.prototype); // => true ✅

// EZArray statik metodları (static methods) və xüsusiyyətləri (properties) miras alır, çünki
// EZArray Array-dən miras alır. Bu, extends açar sözünün (keyword) xüsusi bir xüsusiyyətidir
// və ES6-dan əvvəl mümkün deyildi.
Array.isPrototypeOf(EZArray); // => true ✅
```

`TypedMap` nümunəsi daha dolğun bir nümunədir. O, daxili `Map` sinfinin (class) `TypedMap` adlı bir alt-sinfini (subclass) təyin edir ki, bu da map-in açarlarının (keys) və dəyərlərinin (values) təyin olunmuş tiplərdə (types) olmasını təmin etmək üçün tip yoxlaması (type checking) əlavə edir. Ən əsası, bu nümunə **`super` açar sözünün (keyword)** üst-sinif (superclass) konstruktorunu (constructor) və metodlarını (methods) çağırmaq üçün necə istifadə edildiyini göstərir.

**Nümunə 9-6. TypedMap.js: Açar (Key) və Dəyər (Value) Tiplərini (Types) Yoxlayan Map alt-sinfi (subclass)**

```javascript
class TypedMap extends Map {
    constructor(keyType, valueType, entries) {
        // Əgər girişlər (entries) qeyd olunubsa, onların tiplərini (types) yoxlayın.
        if (entries) {
            for(let [k, v] of entries) {
                if (typeof k !== keyType || typeof v !== valueType) {
                    throw new TypeError(`Wrong type for entry [${k}, ${v}]`);
                }
            }
        }
        // Üst-sinfi (superclass) (tipi yoxlanılmış) ilkin girişlərlə (initial entries) ilkinləşdirin.
        super(entries); // 👈 Üst-sinif (superclass) konstruktorunu (constructor) çağırırıq!

        // Alt-sinfin (subclass) öz vəziyyətini (state) ilkinləşdirin.
        this.keyType = keyType;
        this.valueType = valueType;
    }

    // set() metodunu (method) xəritəyə (map) əlavə olunan yeni girişlər (entries) üçün tip yoxlaması (type checking) əlavə etmək üçün yenidən təyin edin.
    set(key, value) {
        // Açar (key) və ya dəyər (value) səhv tiplidirsə (type) xəta (error) atın.
        if (this.keyType && typeof key !== this.keyType) {
            throw new TypeError(`${key} is not of type ${this.keyType}`);
        }
        if (this.valueType && typeof value !== this.valueType) {
            throw new TypeError(`${value} is not of type ${this.valueType}`);
        }

        // Tiplər (types) düzgündürsə, superclass-ın set() metodunu (method) çağırın.
        return super.set(key, value); // 👈 Üst-sinif (superclass) metodunu (method) çağırırıq!
    }
}
```

`TypedMap()` konstruktorunun (constructor) ilk iki arqumenti (arguments) istənilən açar (key) və dəyər (value) tipləridir (types). Üçüncü arqument (argument) isə map-dəki ilkin girişləri (initial entries) təyin edən `[key,value]` massivləri (arrays) (və ya hər hansı iterable obyekt (object)) ola bilər. Konstruktor (constructor) əvvəlcə onların tiplərini (types) yoxlayır, sonra `super(entries)` istifadə edərək üst-sinif (superclass) konstruktorunu (constructor) çağırır. `super(entries)` çağırışından sonra `TypedMap()` konstruktoru (constructor) `this.keyType` və `this.valueType` dəyərlərini (values) təyin edərək öz alt-sinif (subclass) vəziyyətini (state) ilkinləşdirir.

**Konstruktorlarda (Constructors) `super()` istifadəsi ilə bağlı vacib qaydalar:**

* **`extends` ilə sinif (class) təyin edirsinizsə, konstruktorunuz (constructor) üst-sinif konstruktorunu (superclass constructor) çağırmaq üçün `super()` istifadə etməlidir.** ⚠️
* Əgər alt-sinifinizdə (subclass) konstruktor (constructor) təyin etmirsinizsə, bir konstruktor (constructor) avtomatik yaradılacaq və o, sadəcə aldığı bütün dəyərləri (values) `super()`-a ötürəcək.
* **Konstruktorda (constructor) `this` açar sözünü (keyword) `super()` ilə üst-sinif konstruktorunu (superclass constructor) çağırmadan əvvəl istifadə edə bilməzsiniz.** Bu qayda üst-siniflərin (superclasses) alt-siniflərdən (subclasses) əvvəl ilkinləşməsini təmin edir. 🔒
* `new.target` xüsusi ifadəsi (expression) konstruktor funksiyalarında (constructor functions) çağırılan konstruktora (constructor) istinaddır. Alt-sinif konstruktoru (subclass constructor) `super()` istifadə edərək üst-sinif konstruktorunu (superclass constructor) çağırdıqda, həmin üst-sinif konstruktoru (superclass constructor) `new.target` dəyəri (value) kimi alt-sinif konstruktorunu (subclass constructor) görəcək.

`TypedMap` sinfindəki (class) `set()` metodu (method) `Map` üst-sinfinin (superclass) `set()` metodunu (method) əvəz edir (override). Bu `set()` metodu (method) əvvəlcə açar (key) və dəyərin (value) tiplərini (types) yoxlayır, sonra isə `super.set(key, value)` istifadə edərək üst-sinfin (superclass) `set()` metodunu (method) çağırır. Burada `super` açar sözü (keyword) `this` kimi işləyir: cari obyektə (object) istinad edir, lakin üst-sinifdə (superclass) əvəz olunmuş metodlara (overridden methods) daxil olmağa imkan verir.

**Qayda:** Konstruktorlarda (constructors) `super()`-u `this`-ə daxil olub obyekti (object) ilkinləşdirməzdən əvvəl çağırmalısınız. Lakin metodları (methods) əvəz edərkən (override) belə qaydalar yoxdur. Bir metod (method) üst-sinif (superclass) metodunu (method) istədiyi yerdə (başda, ortada, sonda) çağıra bilər.

`TypedMap` sinfi (class) şəxsi sahələrin (private fields) istifadəsi üçün ideal bir namizəddir. Hazırda, istifadəçi `keyType` və ya `valueType` xüsusiyyətlərini (properties) dəyişərək tip yoxlamasını (type checking) poza bilər. Şəxsi sahələr (private fields) dəstəklənəndə (artıq dəstəklənir), biz bu xüsusiyyətləri (properties) `#keyType` və `#valueType` olaraq dəyişə bilərik ki, kənardan dəyişdirilməsinlər.

---

### 9.5.3 İrsiyyət (Inheritance) əvəzinə Delegasiya (Delegation) 🤝

`extends` açar sözü (keyword) alt-sinifləri (subclasses) asanlıqla yaratmağa imkan verir. Lakin bu o demək deyil ki, çoxlu alt-siniflər (subclasses) yaratmalısınız. Bəzən başqa bir sinfin (class) davranışını (behavior) paylaşmaq istəyirsinizsə, alt-sinif (subclass) yaratmaq əvəzinə, həmin sinifdən (class) bir instansiya (instance) yaradıb, lazım olduqda ona **delegasiya (delegation)** etmək daha asan və çevik ola bilər.

Yəni, yeni bir sinif (class) alt-sinifləmə (subclassing) ilə deyil, digər sinifləri (classes) "bürüyərək" (wrapping) və ya "birləşdirərək" (composing) yaradırsınız. Bu yanaşmaya tez-tez **"kompozisiya" (composition)** deyilir və obyekt-yönümlü proqramlaşdırmanın tez-tez sitat gətirilən bir prinsipidir: **"irsiyyət (inheritance) əvəzinə kompozisiyaya (composition) üstünlük verin."** (`favor composition over inheritance`). 🌿

**Nümunə:** Təsəvvür edin ki, JavaScript-in `Set` sinfinə (class) bənzər, lakin dəyərin (value) neçə dəfə əlavə olunduğunun sayını (count) saxlayan bir `Histogram` sinfi (class) istəyirik. `Histogram` sinfinin (class) API-si `Set`-ə bənzəsə də, onu `Map` istifadə edərək implementasiya (implement) etmək daha məntiqli olar, çünki dəyərlər (values) və onların sayı (count) arasında əlaqə (mapping) saxlamaq lazımdır. Beləliklə, `Set`-i alt-sinifləmə (subclassing) əvəzinə, `Set`-ə bənzər bir API təyin edən, lakin həmin metodları (methods) daxili bir `Map` obyektinə (object) delegasiya (delegation) edən bir sinif (class) yarada bilərik. Nümunə 9-7 bunu necə edəcəyimizi göstərir.

**Nümunə 9-7. Histogram.js: Delegasiya (Delegation) ilə implementasiya olunmuş Set-ə bənzər sinif (class)**

```javascript
/**
 * Dəyərin (value) neçə dəfə əlavə olunduğunu izləyən Set-ə bənzər sinif (class).
 * add() və remove() metodlarını (methods) Set kimi çağırın.
 * count() metodunu (method) isə dəyərin (value) neçə dəfə əlavə olunduğunu tapmaq üçün istifadə edin.
 * Varsayılan iterator (default iterator) ən azı bir dəfə əlavə olunmuş dəyərləri (values) qaytarır.
 * [value, count] cütlərini (pairs) iterasiya (iterate) etmək istəyirsinizsə, entries() istifadə edin.
 */
class Histogram {
    // İlkinləşdirmək (initialize) üçün sadəcə delegasiya (delegation) edəcəyimiz bir Map obyekti (object) yaradırıq.
    constructor() { this.map = new Map(); }

    // Hər hansı bir açar (key) üçün say (count) Map-dəki dəyər (value) və ya Map-də yoxdursa sıfırdır.
    count(key) { return this.map.get(key) || 0; }

    // Set-ə bənzər has() metodu (method) say (count) sıfır deyilsə true qaytarır.
    has(key) { return this.count(key) > 0; }

    // Histogram-ın ölçüsü (size) sadəcə Map-dəki girişlərin (entries) sayıdır.
    get size() { return this.map.size; }

    // Bir açar (key) əlavə etmək üçün, sadəcə Map-dəki sayını (count) artırın.
    add(key) { this.map.set(key, this.count(key) + 1); }

    // Bir açarı (key) silmək bir az çətindir, çünki say (count) sıfıra düşərsə, Map-dən açarı (key) silməliyik.
    delete(key) {
        let count = this.count(key);
        if (count === 1) {
            this.map.delete(key);
        } else if (count > 1) {
            this.map.set(key, count - 1);
        }
    }

    // Histogramı (Histogram) iterasiya (iterate) etmək sadəcə içində saxlanılan açarları (keys) qaytarır.
    [Symbol.iterator]() { return this.map.keys(); }

    // Bu digər iterator metodları (methods) sadəcə Map obyektinə (object) delegasiya (delegation) edir.
    keys() { return this.map.keys(); }
    values() { return this.map.values(); }
    entries() { return this.map.entries(); }
}
```

Nümunə 9-7-dəki `Histogram()` konstruktoru (constructor) yalnız bir `Map` obyekti (object) yaradır. Metodların (methods) əksəriyyəti isə sadəcə map-in bir metoduna (method) delegasiya (delegation) edən bir sətirlik kodlardır, bu da implementasiyanı (implementation) olduqca sadə edir. Delegasiyadan (delegation) irsiyyət (inheritance) əvəzinə istifadə etdiyimiz üçün, bir `Histogram` obyekti (object) `Set` və ya `Map` instansiyası (instance) deyil. Lakin `Histogram` bir sıra geniş istifadə olunan `Set` metodlarını (methods) implementasiya (implement) edir və JavaScript kimi tiplənməmiş (untyped) bir dildə bu, çox vaxt kifayətdir: formal irsiyyət (inheritance) əlaqəsi bəzən yaxşıdır, lakin tez-tez seçimə bağlıdır.

---

### 9.5.4 Sinif İyerarxiyaları (Class Hierarchies) və Abstrakt Siniflər (Abstract Classes) 🌳

Nümunə 9-6 `Map`-i necə alt-sinifləyəcəyimizi (subclass) göstərdi. Nümunə 9-7 isə heç bir şey alt-sinifləmədən (subclass) `Map` obyektinə (object) necə delegasiya (delegation) edə biləcəyimizi göstərdi. JavaScript siniflərini (classes) datanı (data) qablaşdırmaq (encapsulate) və kodunuzu modullaşdırmaq (modularize) üçün istifadə etmək tez-tez əla bir texnikadır və siz `class` açar sözünü (keyword) tez-tez istifadə etdiyinizi görə bilərsiniz. Lakin irsiyyət (inheritance) əvəzinə kompozisiyaya (composition) üstünlük verdiyinizi və `extends` (kitabxana (library) və ya framework istifadə etdiyiniz hallar istisna olmaqla) açar sözündən (keyword) nadir hallarda istifadə etdiyinizi görə bilərsiniz.

Lakin, bəzi hallarda çoxsaylı alt-sinifləmə (subclassing) səviyyələri uyğundur. Bu fəsli müxtəlif növ setləri (sets) təmsil edən siniflər (classes) iyerarxiyasını (hierarchy) göstərən geniş bir nümunə ilə bitirəcəyik. (Nümunə 9-8-də təyin olunmuş set sinifləri (set classes) JavaScript-in daxili `Set` sinfi (class) ilə oxşar, lakin tamamilə uyğun deyil.)

Nümunə 9-8 çoxlu alt-sinif (subclass) təyin edir, lakin həmçinin **abstrakt sinifləri (abstract classes)** – tam implementasiyası (implementation) olmayan sinifləri (classes) – bir qrup əlaqəli alt-sinif (subclass) üçün ümumi bir üst-sinif (superclass) kimi necə təyin edə biləcəyinizi göstərir. Abstrakt üst-sinif (abstract superclass) bütün alt-siniflərin (subclasses) miras alacağı və paylaşacağı qismən bir implementasiya (implementation) təyin edə bilər. Alt-siniflər (subclasses) isə yalnız üst-sinif (superclass) tərəfindən təyin edilmiş, lakin implementasiya (implement) edilməmiş **abstrakt metodları (abstract methods)** implementasiya (implement) etməklə öz unikal davranışlarını (behavior) təyin etməlidirlər. Qeyd edək ki, JavaScript-də abstrakt metodların (abstract methods) və ya abstrakt siniflərin (abstract classes) formal tərifi yoxdur; mən sadəcə burada implementasiya olunmamış metodlar (unimplemented methods) və qismən implementasiya olunmuş siniflər (incompletely implemented classes) üçün bu adı istifadə edirəm. 👻

Nümunə 9-8 yaxşı şərh olunub və öz-özlüyündə aydınlaşdırıcıdır. Sizi siniflər (classes) haqqında bu fəslin son nümunəsi kimi onu oxumağa təşviq edirəm. Nümunə 9-8-dəki son sinif (class) `&`, `|` və `~` operatorları ilə çoxlu bit manipulyasiyası (bit manipulation) aparır, bunları §4.8.3-də nəzərdən keçirə bilərsiniz.

**Nümunə 9-8. Sets.js: Abstrakt (Abstract) və Konkret (Concrete) Set Siniflərinin (Set Classes) İyerarxiyası (Hierarchy)**

```javascript
/**
 * AbstractSet sinfi (class) tək bir abstrakt metod (abstract method), has() metodunu (method) təyin edir.
 */
class AbstractSet {
    // Alt-siniflərin (subclasses) öz işləyən versiyalarını təyin etməsi üçün burada xəta (error) atır.
    has(x) { throw new Error("Abstract method"); }
}

/**
 * NotSet, AbstractSet-in konkret (concrete) alt-sinfidir (subclass).
 * Bu set-in üzvləri (members) başqa bir set-in üzvü (member) olmayan bütün dəyərlərdir (values).
 * Başqa bir set-ə (set) əsaslandığı üçün yazıla bilən (writable) deyil.
 * Sonsuz üzvlərə (members) malik olduğu üçün sayıla bilən (enumerable) deyil.
 */
class NotSet extends AbstractSet {
    constructor(set) {
        super();
        this.set = set;
    }
    // Miras aldığımız abstrakt metodun (abstract method) implementasiyası.
    has(x) { return !this.set.has(x); }
    // Object metodunu (method) əvəz edirik (override).
    toString() { return `{ x| x ∉ ${this.set.toString()} }`; }
}

/**
 * RangeSet, AbstractSet-in konkret (concrete) alt-sinfidir (subclass).
 * Üzvləri (members) 'from' və 'to' hədləri (bounds) arasında olan bütün dəyərlərdir (values).
 * Üzvləri (members) onluq ədədlər (floating point numbers) ola biləcəyi üçün sayıla bilən (enumerable) deyil.
 */
class RangeSet extends AbstractSet {
    constructor(from, to) {
        super();
        this.from = from;
        this.to = to;
    }
    has(x) { return x >= this.from && x <= this.to; }
    toString() { return `{ x| ${this.from} ≤ x ≤ ${this.to} }`; }
}

/*
 * AbstractEnumerableSet, AbstractSet-in abstrakt (abstract) alt-sinfidir (subclass).
 * Set-in ölçüsünü (size) qaytaran abstrakt getter (abstract getter) və abstrakt iterator (abstract iterator) təyin edir.
 * Sonra onların üzərində konkret isEmpty(), toString() və equals() metodlarını (methods) implementasiya (implement) edir.
 * Iteratoru (iterator), size getter-i (getter) və has() metodunu (method) implementasiya (implement) edən alt-siniflər (subclasses)
 * bu konkret metodları (methods) pulsuz əldə edir.
 */
class AbstractEnumerableSet extends AbstractSet {
    get size() { throw new Error("Abstract method"); }
    [Symbol.iterator]() { throw new Error("Abstract method"); }
    isEmpty() { return this.size === 0; }
    toString() { return `{${Array.from(this).join(",")}}`; }
    equals(set) {
        if (!(set instanceof AbstractEnumerableSet)) return false;
        if (this.size !== set.size) return false;
        for(let element of this) {
            if (!set.has(element)) return false;
        }
        return true;
    }
}

/*
 * SingletonSet, AbstractEnumerableSet-in konkret (concrete) alt-sinfidir (subclass).
 * Tək üzvü (member) olan yalnız oxuma (read-only) set-dir.
 */
class SingletonSet extends AbstractEnumerableSet {
    constructor(member) {
        super();
        this.member = member;
    }
    // Bu üç metodu (methods) implementasiya (implement) edirik, isEmpty, equals()
    // və toString() implementasiyalarını isə bu metodlara əsasən miras alırıq.
    has(x) { return x === this.member; }
    get size() { return 1; }
    *[Symbol.iterator]() { yield this.member; }
}

/*
 * AbstractWritableSet, AbstractEnumerableSet-in abstrakt (abstract) alt-sinfidir (subclass).
 * Set-ə fərdi elementləri (elements) əlavə edən və silən abstrakt insert() və remove() metodlarını (methods) təyin edir.
 * Sonra onların üzərində konkret add(), subtract() və intersect() metodlarını (methods) implementasiya (implement) edir.
 * Qeyd edək ki, API-miz burada standart JavaScript Set sinfindən (class) fərqlənir.
 */
class AbstractWritableSet extends AbstractEnumerableSet {
    insert(x) { throw new Error("Abstract method"); }
    remove(x) { throw new Error("Abstract method"); }
    add(set) {
        for(let element of set) {
            this.insert(element);
        }
    }
    subtract(set) {
        for(let element of set) {
            this.remove(element);
        }
    }
    intersect(set) {
        for(let element of this) {
            if (!set.has(element)) {
                this.remove(element);
            }
        }
    }
}

/**
 * BitSet, AbstractWritableSet-in konkret (concrete) alt-sinfidir (subclass).
 * Elementləri (elements) müəyyən bir maksimum ölçüdən (maximum size) kiçik olan
 * mənfi olmayan tam ədədlər (non-negative integers) üçün çox effektiv,
 * sabit ölçülü (fixed-size) set implementasiyasıdır (implementation).
 */
class BitSet extends AbstractWritableSet {
    constructor(max) {
        super();
        this.max = max; // Saxlaya biləcəyimiz maksimum tam ədəd (integer).
        this.n = 0;     // Set-də neçə tam ədəd (integer) var.
        this.numBytes = Math.floor(max / 8) + 1; // Neçə byte-a (byte) ehtiyacımız var.
        this.data = new Uint8Array(this.numBytes); // Byte-lar (bytes).
    }

    // Daxili metod (internal method) - dəyərin (value) bu set-in (set) qanuni üzvü (member) olub olmadığını yoxlayır.
    _valid(x) { return Number.isInteger(x) && x >= 0 && x <= this.max; }

    // Müəyyən edilmiş byte-ın (byte) müəyyən edilmiş bitinin (bit) qurulub-qurulmadığını yoxlayır.
    _has(byte, bit) { return (this.data[byte] & BitSet.bits[bit]) !== 0; }

    // "x" dəyəri (value) bu BitSet-də (BitSet) var mı?
    has(x) {
        if (this._valid(x)) {
            let byte = Math.floor(x / 8);
            let bit = x % 8;
            return this._has(byte, bit);
        } else {
            return false;
        }
    }

    // "x" dəyərini (value) BitSet-ə (BitSet) daxil edin.
    insert(x) {
        if (this._valid(x)) {
            let byte = Math.floor(x / 8);
            let bit = x % 8;
            if (!this._has(byte, bit)) {
                this.data[byte] |= BitSet.bits[bit];
                this.n++;
            }
        } else {
            throw new TypeError("Invalid set element: " + x );
        }
    }

    remove(x) {
        if (this._valid(x)) {
            let byte = Math.floor(x / 8);
            let bit = x % 8;
            if (this._has(byte, bit)) {
                this.data[byte] &= BitSet.masks[bit];
                this.n--;
            }
        } else {
            throw new TypeError("Invalid set element: " + x );
        }
    }

    // Set-in (set) ölçüsünü (size) qaytaran getter (alıcı).
    get size() { return this.n; }

    // Hər biti (bit) yoxlayaraq set-i (set) iterasiya (iterate) edin.
    *[Symbol.iterator]() {
        for(let i = 0; i <= this.max; i++) {
            if (this.has(i)) {
                yield i;
            }
        }
    }
}
// has(), insert() və remove() metodları (methods) tərəfindən istifadə olunan bəzi əvvəlcədən hesablanmış dəyərlər (values).
BitSet.bits = new Uint8Array([1, 2, 4, 8, 16, 32, 64, 128]);
BitSet.masks = new Uint8Array([~1, ~2, ~4, ~8, ~16, ~32, ~64, ~128]);
```