### Fəsil 12. Iteratorlar (Iterators) və Generatorlar (Generators) ✨
----

Bu kitabda dəfələrlə qarşılaşdığımız **iterable (iterasiya edilə bilən)** obyektlər və onların **iteratorları** ES6-nın vacib bir xüsusiyyətidir. Massivlər (Arrays), sətirlər (strings), `Set` və `Map` obyektləri iterable-dır. Bu o deməkdir ki, bu data strukturlarının daxilindəki elementlər üzərində dövr etmək (loop) mümkündür.

Bunu ən çox **`for...of`** dövrü ilə görmüşük:
```javascript
let sum = 0;
for (let i of [1, 2, 3]) { // Bu dəyərlərin hər biri üçün bir dəfə dövr edir
  sum += i;
}
console.log(sum); // ✅ Nəticə: 6
```
Iteratorlar həmçinin **spread operatoru (`...`)** ilə bir obyekti massivə (array) "yaymaq" və ya funksiya arqumentlərinə ötürmək üçün istifadə olunur:
```javascript
let chars = [... "abc"]; // ✅ Nəticə: ['a', 'b', 'c']

let numbers = [10, 5, 25];
console.log(Math.max(...numbers)); // ✅ Nəticə: 25
```
**Destructuring assignment (parçalayaraq mənimsətmə)** ilə də işləyirlər:
```javascript
let [x, y] = new Set([10, 20]);
console.log(x); // ✅ Nəticə: 10
```

`Map` obyektləri üzərində iterasiya apardıqda isə, hər element `[açar, dəyər]` cütlüyü şəklində qayıdır ki, bu da `for...of` dövründə destructuring üçün çox əlverişlidir:
```javascript
let myMap = new Map([["bir", 1], ["iki", 2]]);

for (let [key, value] of myMap) {
  console.log(`Açar: ${key}, Dəyər: ${value}`);
}
// ✅ Nəticə:
// Açar: bir, Dəyər: 1
// Açar: iki, Dəyər: 2

// Yalnız açarları və ya dəyərləri istəyirsinizsə:
console.log(...myMap.keys());   // ✅ Nəticə: bir iki
console.log(...myMap.values()); // ✅ Nəticə: 1 2
```

Bu fəsil, iteratorların necə işlədiyini izah edəcək və öz iterable data strukturlarınızı necə yarada biləcəyinizi göstərəcək.

---
### 12.1 Iteratorlar Necə İşləyir? (How Iterators Work) ⚙️
`for...of` və spread operatoru arxa planda bizim üçün çox iş görür. Bəs pərdə arxasında əslində nə baş verir? Bu prosesi anlamaq üçün 3 komponenti bilməliyik:

1.  **Iterable Obyekt (Iterable Object)** 📦: Üzərində dövr edilə bilən obyektlər (Array, String, Set, Map). Əsas xüsusiyyəti odur ki, onun `[Symbol.iterator]` adlı xüsusi bir metodu var.
2.  **Iterator Obyekti (Iterator Object)** 🚶: `[Symbol.iterator]()` metodu çağırıldıqda qaytarılan obyektdir. Onun da əsas xüsusiyyəti `.next()` adlı bir metoda sahib olmasıdır.
3.  **İterasiya Nəticəsi Obyekti (Iteration Result Object)** 📋: `.next()` metodu hər dəfə çağırıldıqda qaytarılan obyektdir. Onun da `{ value, done }` adlı iki xüsusiyyəti var:
    * **`value`**: Dövrün hazırkı addımındakı dəyər.
    * **`done`**: Dövrün bitib-bitmədiyini göstərən bir `boolean` (`true` və ya `false`).

**Prosesin "Çətin Yolu" (The Hard Way)**
Bir `for...of` dövrünün arxa planda əslində necə işlədiyini əl ilə addım-addım göstərək:
```javascript
let meyveler = ["alma", "armud"];

// 1. Iterable obyektdən onun iteratorunu alırıq.
const iterator = meyveler[Symbol.iterator]();

// 2. Növbəti elementi almaq üçün .next() metodunu çağırırıq
let ilkElement = iterator.next();
console.log(ilkElement);
// ✅ Nəticə: { value: "alma", done: false }

// 3. Prosesi davam etdiririk
let ikinciElement = iterator.next();
console.log(ikinciElement);
// ✅ Nəticə: { value: "armud", done: false }

// 4. Artıq element qalmadıqda, `done` xüsusiyyəti `true` olur
let son = iterator.next();
console.log(son);
// ✅ Nəticə: { value: undefined, done: true }
```
Gördüyünüz kimi, `for...of` dövrü bu `while(!result.done)` məntiqini bizim üçün avtomatlaşdırır.

**Iteratorların Özü də Iterable-dır**
JavaScript-in daxili (built-in) iteratorları özləri də iterable-dır. Yəni, bir iteratorun özünün də `[Symbol.iterator]` metodu var və bu metod sadəcə özünü qaytarır. Bu, bəzən iteratorun bir hissəsini istifadə edib, qalanını başqa bir əməliyyata ötürmək lazım olduqda çox faydalıdır.

**Geniş Nümunə: Siyahının başını və quyruğunu ayırmaq**
```javascript
let reqemler = [10, 20, 30, 40, 50];
let iterator = reqemler[Symbol.iterator]();

// 1. İlk elementi .next() ilə götürürük
let ilkReqem = iterator.next().value;
console.log("İlk rəqəm:", ilkReqem); // ✅ Nəticə: 10

// 2. Qalan elementləri spread (...) operatoru ilə yeni bir massivə yığırıq
// Spread operatoru qalan elementlər üçün iteratorun .next() metodunu çağırmağa davam edir
let qalanReqemler = [...iterator];
console.log("Qalan rəqəmlər:", qalanReqemler); // ✅ Nəticə: [20, 30, 40, 50]
```
Bu hissə, sevdiyimiz `for...of` və `...` operatorlarının pərdə arxasında necə bir mexanizmlə işlədiyini bizə göstərdi.

***
### 12.2 Iterable Obyektlərin Yaradılması (Implementing Iterable Objects) 🛠️
ES6-da öz data tiplərimizi **iterable (iterasiya edilə bilən)** etmək çox faydalı bir təcrübədir. Bu, yaratdığımız obyektləri `for...of` dövrü, spread operatoru (`...`) və digər dil xüsusiyyətləri ilə birbaşa istifadə etməyə imkan verir.

Bir sinifi (class) və ya obyekti iterable etmək üçün ona **`[Symbol.iterator]()`** adlı bir metod əlavə etməlisiniz. Bu metod, öz növbəsində, aşağıdakı xüsusiyyətlərə malik bir **iterator obyekti** qaytarmalıdır:
* `.next()` adlı bir metoda sahib olmalıdır.
* `.next()` metodu `{ value, done }` formatında bir **nəticə obyekti** qaytarmalıdır.

**Geniş Nümunə: Iterable `Range` Sinifi**
Gəlin müəyyən bir aralıqdakı bütün tam rəqəmlər üzərində dövr edən bir `Range` sinifi yaradaq.
```javascript
/**
 * Range obyekti müəyyən bir aralığı təmsil edir {x: from <= x <= to}.
 * Bu sinif iterable-dır və aralıqdakı bütün tam rəqəmləri bir-bir qaytarır.
 */
class Range {
  constructor(from, to) {
    this.from = from;
    this.to = to;
  }

  // Bu aralığa hər hansı bir x rəqəminin daxil olub-olmadığını yoxlayır
  has(x) {
    return typeof x === "number" && this.from <= x && x <= this.to;
  }

  // Obyekti { x | 1 ≤ x ≤ 10 } kimi bir sətirə (string) çevirir
  toString() {
    return `{ x | ${this.from} ≤ x ≤ ${this.to} }`;
  }

  // ❗️ ƏSAS HİSSƏ: Bu metod sinifi iterable edir.
  [Symbol.iterator]() {
    // Hər bir iteratorun öz vəziyyəti (state) olmalıdır ki, bir-birinə mane olmasınlar.
    // Dövrə başlamaq üçün 'from'-dan böyük və ya bərabər olan ilk tam rəqəmi götürürük.
    let nextValue = Math.ceil(this.from);
    let lastValue = this.to;

    // Bu, bizim iterator obyektimizdir.
    return {
      // .next() metodu bu obyekti iterator edir.
      next() {
        // Əgər hələ son dəyərə çatmamışıqsa...
        if (nextValue <= lastValue) {
          // ...hazırkı dəyəri qaytarırıq və növbəti dəyər üçün bir vahid artırırıq.
          return { value: nextValue++, done: false };
        } else {
          // ...əks halda, iterasiyanın bitdiyini bildiririk.
          return { done: true };
        }
      },

      // Əlavə rahatlıq üçün iteratorun özünü də iterable edirik.
      // Bu, yarımçıq qalmış iterasiyaya davam etməyə imkan verir.
      [Symbol.iterator]() { return this; }
    };
  }
}

// İndi yaratdığımız Range sinifini `for...of` ilə istifadə edə bilərik!
console.log("1-dən 5-ə qədər olan rəqəmlər:");
for (let x of new Range(1, 5)) {
  console.log(x);
}

// Və ya spread operatoru (...) ilə!
const arr = [...new Range(-2, 2)];
console.log("Aralıqdan yaranan massiv (array):", arr); // ✅ Nəticə: [-2, -1, 0, 1, 2]
```

**Iterable Qaytaran Funksiyalar**
Bu protokoldan istifadə edərək, massivlərin (arrays) `.map()` və `.filter()` metodlarına bənzər, lakin istənilən iterable obyektlə işləyən və nəticə olaraq yeni bir iterable qaytaran funksiyalar yaza bilərik.

**Nümunə: `map` və `filter` funksiyaları**
```javascript
// Verilmiş iterable üzərində 'f' funksiyasını tətbiq edən yeni bir iterable qaytarır
function map(iterable, f) {
  let iterator = iterable[Symbol.iterator]();
  return {
    [Symbol.iterator]() { return this; },
    next() {
      let v = iterator.next();
      if (v.done) {
        return v;
      } else {
        // Dəyəri funksiyadan keçirib yeni `value` olaraq qaytarırıq
        return { value: f(v.value), done: false };
      }
    }
  };
}

// Verilmiş iterable-ı `predicate` funksiyasına görə filterləyən yeni bir iterable qaytarır
function filter(iterable, predicate) {
  let iterator = iterable[Symbol.iterator]();
  return {
    [Symbol.iterator]() { return this; },
    next() {
      for (;;) { // Şərt ödənilənə qədər dövr edir
        let v = iterator.next();
        if (v.done || predicate(v.value)) {
          return v;
        }
      }
    }
  };
}

// Yaratdığımız funksiyaları bir-birinə bağlayaraq istifadə edək:
let range = new Range(1, 10);
// 1. 1-10 aralığındakı cüt rəqəmləri filterləyirik
let evenNumbers = filter(range, x => x % 2 === 0); // iterable: 2, 4, 6, 8, 10
// 2. Həmin cüt rəqəmləri kvadrata yüksəldirik
let squaredEvens = map(evenNumbers, x => x * x);   // iterable: 4, 16, 36, 64, 100

// 3. Və nəticəni massivə (array) çeviririk
console.log([...squaredEvens]); // ✅ Nəticə: [4, 16, 36, 64, 100]
```

---
### 12.2.1 Iteratoru "Bağlamaq" (Closing): The `return()` Metodu 🚪
Bəzən `for...of` dövrü sona çatmamış dayandırıla bilər (məsələn, `break` və ya `return` ilə). Bəs əgər iteratorumuz arxa planda bir fayl açıbsa və ya başqa bir resurs istifadə edirsə? Dövr yarımçıq qalsa, həmin resurs açıq qalacaq.

Bu problemin həlli üçün iterator obyektinə `.return()` adlı bir metod əlavə etmək olar. Əgər iterasiya yarımçıq dayandırılsa, JavaScript mühərriki avtomatik olaraq bu metodu çağırır və iteratora "təmizlik işləri" aparmaq üçün şans verir.

**Nümunə: `.return()` metodunun işləmə prinsipi**
```javascript
const myIterable = {
  [Symbol.iterator]() {
    let count = 0;
    return {
      next() {
        if (count < 5) {
          console.log(`(next) Dəyər verilir: ${count}`);
          return { value: count++, done: false };
        }
        console.log("(next) İterasiya normal bitdi");
        return { done: true };
      },
      // Bu metod yalnız iterasiya yarımçıq qaldıqda çağırılacaq
      return() {
        console.log("❗️ İterasiya yarımçıq dayandırıldı! Təmizləmə işləri aparılır...");
        return { done: true }; // Nəticə obyekti qaytarmalıdır
      }
    };
  }
};

console.log("Dövr başlayır...");
for (const value of myIterable) {
  if (value > 2) {
    break; // Dövrü 3-cü addımda yarımçıq saxlayırıq
  }
}
console.log("Dövr bitdi.");

// Nəticə:
// Dövr başlayır...
// (next) Dəyər verilir: 0
// (next) Dəyər verilir: 1
// (next) Dəyər verilir: 2
// (next) Dəyər verilir: 3
// ❗️ İterasiya yarımçıq dayandırıldı! Təmizləmə işləri aparılır...
// Dövr bitdi.
```
Gördüyünüz kimi, `break` işə düşən kimi `.return()` metodu avtomatik çağırıldı.

***
### 12.3 Generatorlar (Generators) 🪄
**Generator (generator)** – xüsusi sintaksislə yaradılan bir iterator növüdür. O, bir funksiya kimidir, ancaq icrasını istənilən an **dayandırıb (pause)**, daha sonra qaldığı yerdən **davam etdirə (resume)** bilər.

Generator yaratmaq üçün **generator funksiyası (generator function)** təyin etməliyik. Bu, adi funksiya kimidir, lakin `function` açar sözündən sonra bir ulduz (`*`) işarəsi ilə yazılır: `function*`.

Bir generator funksiyasını çağırdıqda, o, dərhal icra olunmur. Bunun əvəzinə, bizə bir **generator obyekti (generator object)** qaytarır. Bu obyekt özü bir **iteratordur**. Onun `.next()` metodunu çağırdıqda, funksiyanın gövdəsi ilk `yield` ifadəsinə qədər işləyir.

**`yield`** açar sözü ES6 ilə gələn bir yenilikdir və `return`-ə bənzəyir, amma fərqi odur ki, funksiyanı sonlandırmır, sadəcə dayandırır. `yield` edilən dəyər, `.next()` metodunun qaytardığı nəticə obyektinin `value` xüsusiyyəti olur.

**Nümunə 1: Sadə generator**
Gəlin birrəqəmli sadə ədədləri qaytaran bir generator yaradaq.
```javascript
// Generator funksiyası `function*` ilə təyin olunur
function* oneDigitPrimes() {
  console.log("Generator başladı...");
  yield 2;
  console.log("İkinci dəyərə keçdi...");
  yield 3;
  console.log("Üçüncü dəyərə keçdi...");
  yield 5;
  console.log("Dördüncü dəyərə keçdi...");
  yield 7;
  console.log("Generator bitdi.");
}

// 1. Funksiyanı çağırdıqda o, icra olunmur, sadəcə bir generator obyekti qaytarır.
let primes = oneDigitPrimes();

// 2. Kodun icrası yalnız .next() çağırıldıqda başlayır.
console.log(primes.next()); // "Generator başladı..." -> { value: 2, done: false }
console.log(primes.next()); // "İkinci dəyərə keçdi..." -> { value: 3, done: false }
console.log(primes.next()); // "Üçüncü dəyərə keçdi..." -> { value: 5, done: false }
console.log(primes.next()); // "Dördüncü dəyərə keçdi..." -> { value: 7, done: false }
console.log(primes.next()); // "Generator bitdi." -> { value: undefined, done: true }

// Generatorlar iterable olduğu üçün onları birbaşa `for...of` və `...` ilə işlədə bilərik.
console.log([...oneDigitPrimes()]); // ✅ Nəticə: [2, 3, 5, 7]
```

**Generator Sintaksis Növləri**
Generatorları fərqli formalarda təyin etmək olar:
```javascript
// Function Expression
const seq = function*(from, to) {
  for(let i = from; i <= to; i++) yield i;
};
console.log([...seq(3, 5)]); // ✅ Nəticə: [3, 4, 5]

// Object Method
let obj = {
  *myGenerator() {
    yield 'a';
    yield 'b';
  }
};
console.log([...obj.myGenerator()]); // ✅ Nəticə: ['a', 'b']
```
❗️ Unutma: Ox funksiyası (`=>`) sintaksisi ilə generator yaratmaq mümkün deyil.

**Iterable Sinifləri Sadələşdirmək**
Əvvəlki bölmədəki `Range` sinifini xatırlayırsınız? Orada `[Symbol.iterator]` üçün nə qədər kod yazmışdıq. İndi isə həmin metodu bir generator ilə nə qədər asan yaza biləcəyimizə baxın:

```javascript
class Range {
  constructor(from, to) {
    this.from = from;
    this.to = to;
  }
  
  // Əvvəlki mürəkkəb [Symbol.iterator] metodunun əvəzinə:
  *[Symbol.iterator]() {
    for(let x = Math.ceil(this.from); x <= this.to; x++) {
      yield x;
    }
  }
}

console.log([...new Range(1, 5)]); // ✅ Nəticə: [1, 2, 3, 4, 5]
```
Gördüyünüz kimi, generatorlar iterator yaratmaq prosesini inanılmaz dərəcədə sadələşdirir!

---
### 12.3.1 Generator Nümunələri ✨
Generatorların əsl gücü, dəyərləri hesablama yolu ilə yaratdıqda ortaya çıxır.

**Nümunə 1: Sonsuz Fibonacci Generatoru**
Bu generator, heç vaxt dayanmadan Fibonacci ardıcıllığını yaradır. `yield` sayəsində funksiya sonsuz dövrdə " ilişib" qalmır.
```javascript
function* fibonacciSequence() {
  let x = 0, y = 1;
  for(;;) { // Sonsuz dövr
    yield y;
    [x, y] = [y, x + y]; // Dəyərləri yeniləyirik
  }
}

// Sonsuz bir generatordan necə istifadə etməli? Dəyərləri məhdudlaşdırmaqla!
// Məsələn, 20-ci Fibonacci rəqəmini tapaq:
function fibonacci(n) {
  for (let f of fibonacciSequence()) {
    if (n-- <= 0) return f;
  }
}
console.log("20-ci Fibonacci rəqəmi:", fibonacci(20)); // ✅ Nəticə: 6765
```

**Nümunə 2: `take(n, iterable)` - "N dənə götür" Generatoru**
Bu, başqa bir iterable obyektdən (və ya generatordan) yalnız ilk `n` elementi götürən bir "köməkçi" generatordur.
```javascript
function* take(n, iterable) {
  let it = iterable[Symbol.iterator](); // İteratoru alırıq
  while (n-- > 0) {
    let next = it.next();
    if (next.done) return; // Əgər elementlər bitibsə, dayanırıq
    else yield next.value; // Əks halda, dəyəri ötürürük
  }
}

// İndi Fibonacci ardıcıllığının ilk 5 elementini asanlıqla götürə bilərik:
console.log(
  "İlk 5 Fibonacci rəqəmi:",
  [...take(5, fibonacciSequence())]
);
// ✅ Nəticə: [1, 1, 2, 3, 5]
```

**Nümunə 3: `zip(...iterables)` - Birləşdirici Generator**
Bu generator bir neçə iterable obyekti arqument kimi götürür və onların elementlərini növbə ilə, bir-birinin ardınca qaytarır.
```javascript
function* zip(...iterables) {
  let iterators = iterables.map(i => i[Symbol.iterator]());
  let index = 0;
  while (iterators.length > 0) {
    if (index >= iterators.length) {
      index = 0; // Sona çatdıqda başa qayıdırıq
    }
    let item = iterators[index].next();
    if (item.done) {
      iterators.splice(index, 1); // Bütün elementləri bitən iteratoru siyahıdan silirik
    } else {
      yield item.value;
      index++;
    }
  }
}

// 3 fərqli iterable-ı birləşdirək:
const zipped = [...zip(oneDigitPrimes(), "abc", [9, 8])];
console.log("Birləşdirilmiş nəticə:", zipped);
// ✅ Nəticə: [2, "a", 9, 3, "b", 8, 5, "c", 7]
```

Bununla da generatorların sehrli dünyasına səyahətimizi başa vurduq. Gördüyün kimi, onlar mürəkkəb iterasiya məntiqini çox sadə və oxunaqlı bir şəkildə yazmağa imkan verir!

Və budur, generatorların sonuncu, amma çox güclü bir xüsusiyyəti, bro! Bu hissə ilə iterator və generator mövzusunu tam möhürləyirik.

***
### 12.3.2 `yield*` və Rekursiv Generatorlar (Recursive Generators) 🔁

Təsəvvür et ki, bir neçə fərqli iterable obyekti (məsələn, massiv (array), sətir (string), generator) birləşdirib, onların elementlərini bir-birinin ardınca, ardıcıl şəkildə qaytaran bir generator yazmaq istəyirik. Bunu iç-içə `for...of` dövrü ilə belə yaza bilərik:

```javascript
function* sequence(...iterables) {
  for (const iterable of iterables) {
    for (const item of iterable) {
      yield item;
    }
  }
}

const numbers = [1, 2];
const letters = "ab";
console.log([...sequence(numbers, letters)]); // ✅ Nəticə: [1, 2, "a", "b"]
```
Bu, işlək bir koddur, amma daha sadə bir yolu var.

#### `yield*` İfadəsi

Başqa bir iterable-ın bütün elementlərini tək-tək `yield` etmək o qədər yayğın bir əməliyyatdır ki, ES6 bunun üçün **`yield*`** adlı xüsusi bir sintaksis təqdim edir. Bu, "iterasiyanı başqa bir iterable-a ötür (delegate)" deməkdir.

`yield*` ilə yuxarıdakı `sequence` funksiyası daha qısa və oxunaqlı olur:

```javascript
function* sequenceWithYieldStar(...iterables) {
  for (const iterable of iterables) {
    yield* iterable; // Bu sətir, iç-içə olan bütün for...of dövrünü əvəz edir!
  }
}

const numbers = [1, 2];
const letters = "ab";
console.log([...sequenceWithYieldStar(numbers, letters)]); // ✅ Nəticə: [1, 2, "a", "b"]
```

**❗️ Vacib Qeyd: `yield*` harada işləmir?**
`yield` və `yield*` ifadələri yalnız birbaşa generator funksiyasının (`function*`) daxilində işləyir. Onları `forEach` kimi adi funksiyaların daxilində istifadə etmək olmaz.

```javascript
function* sequenceWrong(...iterables) {
  // BU KOD İŞLƏMİR! ❌
  // Çünki forEach-ə ötürülən ox funksiyası (=>) adi bir funksiyadır, generator deyil.
  iterables.forEach(iterable => yield* iterable); // SyntaxError: yield is only valid in generator functions.
}
```

#### Rekursiv (Recursive) Generatorlar

`yield*` ifadəsinin ən güclü tərəflərindən biri, onu başqa generatorlarla birlikdə istifadə edərək **rekursiv generatorlar** yaratmağa imkan verməsidir. Bu, ağac (tree) kimi mürəkkəb, iç-içə data strukturlarını sadə bir şəkildə iterasiya etmək üçün əla bir yoldur.

**Geniş Nümunə: İç-içə kateqoriyaları sadə bir siyahıya çevirmək**
Tutaq ki, belə bir kateqoriya strukturumuz var:
```javascript
const categories = {
  name: "Elektronika",
  subCategories: [
    { name: "Telefonlar", subCategories: [] },
    { 
      name: "Kompüterlər", 
      subCategories: [
        { name: "Notbuklar", subCategories: [] },
        { name: "Masaüstü", subCategories: [] }
      ]
    }
  ]
};
```
Bu mürəkkəb strukturu düz bir siyahı halına gətirən rekursiv bir generator yazaq:
```javascript
function* iterateCategories(category) {
  // 1. Hazırkı kateqoriyanın adını qaytarırıq (yield)
  yield category.name;

  // 2. Əgər alt-kateqoriyaları varsa...
  if (category.subCategories.length > 0) {
    for (const subCategory of category.subCategories) {
      // 3. ...hər bir alt-kateqoriya üçün iterasiyanı rekursiv olaraq bu funksiyanın özünə ötürürük (yield*)
      yield* iterateCategories(subCategory);
    }
  }
}

// İndi bütün kateqoriyaları sadə bir massiv kimi əldə edə bilərik:
const allCategoryNames = [...iterateCategories(categories)];

console.log(allCategoryNames);
// ✅ Nəticə: ["Elektronika", "Telefonlar", "Kompüterlər", "Notbuklar", "Masaüstü"]
```

---

### 12.4 Generatorların Qabaqcıl Xüsusiyyətləri (Advanced Generator Features) 🚀

Generatorların əsas istifadə məqsədi iterator yaratmaq olsa da, onların təməl xüsusiyyəti bir hesablama prosesini **dayandırmaq (pause)**, aralıq nəticəni **ötürmək (yield)** və daha sonra qaldığı yerdən **davam etmək (resume)** imkanıdır. Bu, iteratorlardan daha geniş imkanlar deməkdir.

---
### 12.4.1 Generator Funksiyasının Qaytarılan Dəyəri (Return Value) 🎁

Adi funksiyalar kimi, generatorlar da `return` ilə bir dəyər qaytara bilər. Bəs bu dəyərə nə olur?

`return` ilə qaytarılan dəyər, `.next()` metodunun **sonuncu** çağırışında, `done: true` olan nəticə obyektinin `value` xüsusiyyəti olur.

❗️ **Çox Vacib Qeyd:** `for...of` dövrü və spread operatoru (`...`) `done: true` olan nəticənin `value`-sunu, yəni generatorun `return` etdiyi dəyəri **nəzərə almır!** Bu dəyəri yalnız `.next()` metodunu əl ilə çağırdıqda əldə etmək mümkündür.

**Nümunə: `return` dəyərini görmək**
```javascript
function* oneAndDone() {
  yield 1;
  yield 2;
  return "Generator bitdi!";
}

// 1. `for...of` və `...` ilə istifadə etdikdə `return` dəyəri görünmür:
const normalIteration = [...oneAndDone()];
console.log("Normal iterasiya:", normalIteration); // ✅ Nəticə: [1, 2]

// 2. `.next()` metodunu əl ilə çağırdıqda isə görünür:
const generator = oneAndDone();

console.log(generator.next()); // { value: 1, done: false }
console.log(generator.next()); // { value: 2, done: false }
console.log(generator.next()); // { value: "Generator bitdi!", done: true } ⬅️ Budur!
console.log(generator.next()); // { value: undefined, done: true } (Artıq bitib)
```

---
### 12.4.2 `yield` İfadəsinin Dəyəri (Value of `yield`) 🤝
`yield` təkcə dəyər ötürən bir operator deyil, həm də özü bir dəyər qəbul edə bilən bir **ifadədir (expression)**. Bu, generatorla onu çağıran kod arasında **iki tərəfli kommunikasiya** qurmağa imkan verir:
* **Generator `yield` ilə xaricə dəyər göndərir 📤.**
* **Xarici kod `.next(value)` ilə içəri, yəni generatora dəyər göndərir 📥.**

`.next()`-ə ötürülən dəyər, dayandırılmış `yield` ifadəsinin dəyəri olur.

**Geniş Nümunə: İki tərəfli rabitə**
```javascript
function* twoWayCommunication() {
  console.log("Generator başladı.");
  
  let firstInput = yield "İlk sual: Adın nədir?"; // 1. Dayandı, "İlk sual..." göndərdi
  console.log(`📥 Generatora gələn ilk cavab: ${firstInput}`);
  
  let secondInput = yield "İkinci sual: Yaşın neçədir?"; // 2. Dayandı, "İkinci sual..." göndərdi
  console.log(`📥 Generatora gələn ikinci cavab: ${secondInput}`);
  
  return "Dialoq bitdi.";
}

const g = twoWayCommunication();

// Birinci next() həmişə arqumentsiz olmalıdır, çünki hələ cavab gözləyən `yield` yoxdur.
let question1 = g.next(); // "Generator başladı."
console.log("📤 Generatordan gələn ilk sual:", question1.value);

// İndi generator ilk `yield`-də dayanıb və cavab gözləyir. Ona cavabı .next() ilə göndəririk.
let question2 = g.next("Elvin"); // "Elvin" dəyəri `firstInput`-a mənimsədilir
console.log("📤 Generatordan gələn ikinci sual:", question2.value);

// Prosesi təkrarlayırıq
let finalResult = g.next(28); // "28" dəyəri `secondInput`-a mənimsədilir
console.log("📤 Generatordan gələn son nəticə:", finalResult.value);
```
---
### 12.4.3 Generatorun `return()` və `throw()` Metodları ⚡
Generatoru təkcə `.next()` ilə deyil, həm də xaricdən `.return()` və `.throw()` metodları ilə idarə etmək olar.

* **`generator.return()`**: Generatoru sanki bir `return` ifadəsinə rast gəlmiş kimi davranmağa məcbur edir. Bu, generatorun `try...finally` bloku varsa, təmizlik işlərinin aparılması üçün çox vacibdir.
* **`generator.throw()`**: Generatorun daxilinə sanki bir `throw` ifadəsi icra olunmuş kimi bir xəta göndərir. Generator bu xətanı `try...catch` ilə tutub emal edə bilər.

**Nümunə: `.return()` və `.throw()` ilə generatoru idarə etmək**
```javascript
function* managedGenerator() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch(e) {
    console.log("❗️ Generator daxilində xəta tutuldu:", e.message);
  } finally {
    console.log("🧼 Təmizlik işləri aparılır (finally bloku).");
  }
}

// 1. .return() nümunəsi
const gen1 = managedGenerator();
console.log(gen1.next()); // { value: 1, done: false }
// Dövrü yarımçıq dayandırırıq
console.log(gen1.return("Bitdi")); // "🧼 Təmizlik işləri..." -> { value: "Bitdi", done: true }

console.log("\n-------------------\n");

// 2. .throw() nümunəsi
const gen2 = managedGenerator();
console.log(gen2.next()); // { value: 1, done: false }
// Generatorun içinə xəta göndəririk
console.log(gen2.throw(new Error("Xarici müdaxilə")));
// "❗️ Generator daxilində xəta tutuldu..."
// "🧼 Təmizlik işləri..."
// { value: undefined, done: true }
```
---
