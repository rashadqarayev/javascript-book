# 9. Siniflər (Classes) 

Fəsil 6-da biz JavaScript-də obyektləri (objects) öyrəndik — yəni, unikal xüsusiyyətlərə malik varlıqları. Amma çox vaxt eyni xüsusiyyətlərə və davranışlara sahib obyektlər qrupu yaratmaq lazım gəlir. Belə hallarda **siniflər (classes)** istifadə olunur. Siniflər kodunuzu daha səliqəli, oxunaqlı və təkrar istifadəyə yararlı edir.

---

### Meyvə sinfi

Tutaq ki, meyvələri təsvir edən `Fruit` adlı bir sinifimiz var. Hər meyvənin öz rəngi və çəkisi var. Həmçinin, meyvəni təsvir edən və onun dadını bildirən metodları da mövcuddur.

```javascript
class Fruit {
    constructor(color, weight) {
        this.color = color;     // Rəng (state)
        this.weight = weight;   // Çəki (state)
    }

    describe() {
        return `Bu ${this.color} rəngdə, ${this.weight} qram ağırlığında bir meyvədir.`;
    }

    taste() {
        return "Meyvənin dadı var!";
    }
}

let apple = new Fruit("qırmızı", 150);
let banana = new Fruit("sarı", 120);

console.log(apple.describe());  
// Bu qırmızı rəngdə, 150 qram ağırlığında bir meyvədir.

console.log(banana.taste());    
// Meyvənin dadı var!
```

---

### Siniflər və Prototiplər

JavaScript-də siniflər **prototip-əsaslı irsiyyət (prototype-based inheritance)** mexanizmi ilə işləyir. Bu o deməkdir ki:

* Sinifdən yaradılan bütün instansiyalar (obyektlər) eyni prototipdən metodları miras alırlar.
* Beləliklə, onlar eyni sinifə aid sayılır.
* Prototip və konstruktor funksiyası vasitəsilə bu mexanizm həyata keçirilir.

JavaScript əvvəlcə sinifləri sintaksis olaraq dəstəkləmirdi, amma **ES6**-da (ECMAScript 2015) sinifləri yaratmaq üçün yeni və daha oxunaqlı `class` açar sözü əlavə olundu.

---

### 9.1 Siniflər (Classes) və Prototiplər (Prototypes) 💡

JavaScript-də sinif (class) eyni **prototip obyektindən (prototype object)** xüsusiyyətləri (properties) miras alan obyektlər qrupudur. Yəni, **prototip obyekti (prototype object)** sinfin (class) əsasını təşkil edir.

`Object.create()` funksiyası müəyyən bir prototip obyektindən miras alan yeni obyekt yaradır. Beləcə, biz `Object.create()`-dən istifadə edərək prototipə əsaslanan sinif yarada bilərik.

---

#### Sadə nümunə: İnsan (Person) sinfi

```javascript
function Person(name, age) {
  let obj = Object.create(Person.methods);
  obj.name = name;
  obj.age = age;
  return obj;
}

Person.methods = {
  greet() {
    return `Salam, mənim adım ${this.name} və mən ${this.age} yaşındayam.`;
  }
};

let p = Person("Rəşad", 20);
console.log(p.greet()); 
// Salam, mənim adım Rəşad və mən 20 yaşındayam.
```

> Burada `Person` fabrikat funksiyası yeni insan obyekti yaradır. Metodlar `Person.methods` prototipində saxlanılır.

---

####  Bank Hesabı (BankAccount)

```javascript
function BankAccount(owner, balance = 0) {
  let account = Object.create(BankAccount.methods);
  account.owner = owner;
  account.balance = balance;
  return account;
}

BankAccount.methods = {
  deposit(amount) {
    this.balance += amount;
    return `Hesaba ${amount} əlavə olundu. Yeni balans: ${this.balance}`;
  },
  withdraw(amount) {
    if(amount > this.balance) return "Kifayət qədər vəsait yoxdur!";
    this.balance -= amount;
    return `Hesabdan ${amount} çıxarıldı. Yeni balans: ${this.balance}`;
  },
  info() {
    return `${this.owner} sahibinin balansı: ${this.balance}`;
  }
};

let acc = BankAccount("Ayan", 1000);
console.log(acc.deposit(500));   
// Hesaba 500 əlavə olundu. Yeni balans: 1500
console.log(acc.withdraw(2000)); 
// Kifayət qədər vəsait yoxdur!
console.log(acc.info());          
// Ayan sahibinin balansı: 1500
```
---

### 9.2 Konstruktorlar və Klassik Sinif Yaratma

JavaScript-də obyekt yaratmağın klassik yolu **konstruktor funksiyası** ilə `new` açar sözündən istifadə etməkdir.

#### Sıfırdan obyekt yaratmaq:

```js
let obj = new Constructor(...args);
```

---

### Nümunə 1: Sadə sinif (`Car`)

```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

Car.prototype.info = function() {
  return `${this.year} ${this.make} ${this.model}`;
};

let car = new Car("Toyota", "Camry", 2010);
console.log(car.info()); // 2010 Toyota Camry
```

---

### 9.2.1 `instanceof` və Sinif Kimliyi

`instanceof` bir obyektin müəyyən konstruktorla yaradılıb-yaradılmadığını **prototip zəncirinə** baxaraq yoxlayır:

```js
car instanceof Car // true
```

Bu, sadəcə `car.__proto__` zəncirində `Car.prototype` var, deməkdir. Hətta başqa bir konstruktor belə həmin prototipi paylaşsa:

```js
function Strange() {}
Strange.prototype = Car.prototype;

let weird = new Strange();
console.log(weird instanceof Car); // true
```

`instanceof` – obyektin hansı sinfə aid olduğunu tapmaq üçün ideal üsuldur.  
---

### Alternativ: `isPrototypeOf()`

Əgər konstruktor yoxdur və ya istifadə edilməyibsə:

```js
Car.prototype.isPrototypeOf(car) // true
```

---

### 9.2.2 `constructor` Xüsusiyyəti

Bütün funksiyalar avtomatik bir `prototype` obyektinə sahib olur və bu obyektin içində `constructor` olur:

```js
function A() {}
console.log(A.prototype.constructor === A); // true
```

Bu sayədə yaradılan obyektlərdən orijinal funksiyanı tapmaq mümkündür:

```js
let o = new A();
console.log(o.constructor === A); // true
```

---

### Problem: `constructor` yox olur?

Əgər `prototype`-u tamamilə yenidən təyin etsək, `constructor` silinir:

```js
function Range(from, to) {
  this.from = from;
  this.to = to;
}

// Burada constructor silinir:
Range.prototype = {
  includes(x) {
    return this.from <= x && x <= this.to;
  }
};

console.log(new Range(1, 3).constructor === Range); // false
```

**Həll:** `constructor`-u əllə bərpa et:

```js
Range.prototype = {
  constructor: Range,
  includes(x) {
    return this.from <= x && x <= this.to;
  }
};
```

---

### Alternativ Yanaşma (Constructor qorunur)

```js
Range.prototype.includes = function(x) {
  return this.from <= x && x <= this.to;
};
Range.prototype.toString = function() {
  return `(${this.from}...${this.to})`;
};
```

Bu üsul `constructor`-u silmir, çünki `prototype`-u yenidən yazmırıq, sadəcə genişləndiririk.

---


### 9.3 `class` Açar Sözü ilə Sinif Yaratmaq

JavaScript-də sinif anlayışı əvvəldən olsa da, ES6 ilə birlikdə bu anlayış üçün ayrıca `class` sintaksisi təqdim olundu. Bu sintaksis funksional cəhətdən əvvəlki `constructor` və `prototype` üsulu ilə eynidir, sadəcə daha oxunaqlı və strukturlaşdırılmış formadadır.

---

### Sinifin Təyini və İstifadəsi

```js
class Car {
  constructor(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
  }

  info() {
    return `${this.year} ${this.make} ${this.model}`;
  }

  age(currentYear) {
    return currentYear - this.year;
  }
}

let myCar = new Car("Toyota", "Camry", 2010);
console.log(myCar.info());      // 2010 Toyota Camry
console.log(myCar.age(2025));   // 15
```

---

### Əsas Xüsusiyyətlər

* `class` ilə sinif təyin edirik.
* `constructor` metodu yeni obyekt yaradılarkən çağırılır və `this`-ə dəyərlər mənimsədilir.
* Funksiya açar sözü (`function`) yazılmır.


---

### Alt Sinif Yaratmaq (`extends` və `super`)

```js
class Square extends Car {
  constructor(length) {
    super("Square", "Model", 2020); 
    // Yuxarı sinifin konstruktorunu çağırırıq
    this.area = length * length;
  }
}

let sq = new Square(4);
console.log(sq.area);  // 16
```

* `extends` vasitəsilə başqa sinifdən miras alırıq.
* `super()` yuxarı sinfin `constructor` metodunu çağırır.

---

### Sinif İfadəsi (Class Expression)

Siniflər birbaşa dəyişən kimi də təyin oluna bilər:

```js
let Rectangle = class {
  constructor(w, h) {
    this.w = w;
    this.h = h;
  }

  area() {
    return this.w * this.h;
  }
};

let r = new Rectangle(4, 5);
console.log(r.area());  // 20
```

---

### Diqqət Ediləcək Məqamlar

* Bütün `class`-ların içi avtomatik olaraq `strict mode` rejimində işləyir.
* `class` təyin olunmadan əvvəl istifadə edilə bilməz (`hoisting` yoxdur).

---
Çox yaxşı gedirik. Aşağıdakı kimi yenidən yazdım — sadə dil, qısa izah, daha çox nümunə, emojisiz və oxucuya fokuslanmış:

---

### 9.3.1 Statik Metodlar (`static`)

Sinifin içində `static` açar sözü ilə yazılan metodlar **instansiyalara yox**, birbaşa **sinfin özünə** aiddir. Onlar `prototype`-da yox, konstruktor funksiyasının özündə yerləşir.

#### Nümunə: `parse` adlı statik metod

```js
class Range {
  constructor(from, to) {
    this.from = from;
    this.to = to;
  }

  static parse(str) {
    let match = str.match(/^\((\d+)\.\.\.(\d+)\)$/);
    if (!match) throw new Error("Format səhvdir");
    return new Range(Number(match[1]), Number(match[2]));
  }

  toString() {
    return `(${this.from}...${this.to})`;
  }
}

let r1 = Range.parse("(5...10)");
console.log(r1.toString());     // (5...10)
```

Statik metod **instansiya üzərində çağırıla bilməz**:

```js
let r2 = new Range(1, 3);
r2.parse("(1...3)");    // Xəta: parse funksiyası instansiyada yoxdur
```

Bu metodlar adətən köməkçi funksiyalar üçün istifadə olunur, məsələn, string-dən obyekt yaratmaq, formatlama, müqayisə və s.

---

### 9.3.2 Getter və Setter metodları

Sinif daxilində `get` və `set` sözləri ilə **xüsusi oxuma və yazma metodları** təyin edə bilərik.

#### Nümunə: `User` sinfi ilə

```js
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name.toUpperCase();
  }

  set name(value) {
    if (value.length < 2) throw new Error("Ad çox qısadır");
    this._name = value;
  }
}

let u = new User("Ali");
console.log(u.name);      // ALI
u.name = "Mehdi";
console.log(u.name);      // MEHDI
```

Getter/Setter adları **xüsusiyyət kimi** istifadə olunur (`user.name`), metod kimi deyil (`user.name()` deyil).

---

### Digər Metod Formaları

#### 1. **Generator metodlar (`*`)**

Generatorlar `yield` ifadəsi ilə bir neçə dəyər qaytarmaq üçün istifadə olunur:

```js
class Counter {
  constructor(limit) {
    this.limit = limit;
  }

  *[Symbol.iterator]() {
    for (let i = 1; i <= this.limit; i++) {
      yield i;
    }
  }
}

let c = new Counter(3);
console.log([...c]);    // [1, 2, 3]
```

#### 2. **Hesablanmış metod adları (Computed Method Names)**

Metod adını dəyişən və ya ifadə ilə təyin edə bilərik:

```js
let methodName = "sayHello";

class Greeter {
  [methodName]() {
    return "Salam!";
  }
}

let g = new Greeter();
console.log(g.sayHello());  // Salam!
```

---

## 9.3.3 Public, Private və Statik Sahələr

JavaScript-də sinif içində `field` (xüsusiyyət) təyin etmək üçün əvvəllər yalnız konstruktor istifadə olunurdu. Yeni sintaksis isə bu sahələri sinifin daxilində birbaşa yazmağa imkan verir.

### Public (İctimai) sahələr

Public sahə sinif obyektinə məxsus olur və `constructor` xaricində təyin edilir:

```js
class Counter {
  count = 0;

  increase() {
    this.count++;
  }
}

const c = new Counter();
c.increase();
console.log(c.count); // 1
```

### Private (Şəxsi) sahələr

Private sahələr `#` işarəsi ilə başlayır və sinifdən kənarda istifadə oluna bilməz:

```js
class BankAccount {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const acc = new BankAccount();
acc.deposit(100);
console.log(acc.getBalance()); // 100
console.log(acc.#balance); // ❌ SyntaxError
```

### Statik sahələr

Statik sahələr sinifin özünə aiddir, instansiyalara deyil:

```js
class MathUtil {
  static PI = 3.14;

  static square(x) {
    return x * x;
  }
}

console.log(MathUtil.PI); // 3.14
console.log(MathUtil.square(4)); // 16
```

---

## 9.3.4 Nümunə: Complex Sinfi

Aşağıda **kompleks ədədləri** təmsil edən sinif nümunəsi verilib. Bu sinifdə həm public sahələr, həm instansiya metodları, həm də statik metodlar istifadə olunub.

```js
class Complex {
  constructor(real, imag) {
    this.real = real;
    this.imag = imag;
  }

  add(other) {
    return new Complex(this.real + other.real, this.imag + other.imag);
  }

  multiply(other) {
    const r = this.real * other.real - this.imag * other.imag;
    const i = this.real * other.imag + this.imag * other.real;
    return new Complex(r, i);
  }

  get magnitude() {
    return Math.sqrt(this.real ** 2 + this.imag ** 2);
  }

  toString() {
    return `${this.real} + ${this.imag}i`;
  }

  equals(other) {
    return other instanceof Complex &&
           this.real === other.real &&
           this.imag === other.imag;
  }

  static ZERO = new Complex(0, 0);
  static ONE = new Complex(1, 0);
  static I = new Complex(0, 1);
}
```

İstifadə nümunəsi:

```js
const x = new Complex(2, 3);
const y = new Complex(1, -1);

const sum = x.add(y);
console.log(sum.toString()); // 3 + 2i

const prod = x.multiply(y);
console.log(prod.toString()); // 5 + 1i

console.log(Complex.ZERO.toString()); // 0 + 0i
```

---


### 9.4 Mövcud Siniflərə Metodlar Əlavə Etmək

JavaScript-in prototip-əsaslı irsiyyət mexanizmi dinamikdir. Obyektlər yaradıldıqdan sonra belə, onların prototiplərinə yeni metodlar əlavə etmək mümkündür.

**Məsələn,** aşağıdakı kod `Complex` sinfinə yeni metod əlavə edir:

```javascript
Complex.prototype.conj = function() {
    return new Complex(this.r, -this.i);
};

let z = new Complex(3, 4);
console.log(z.conj().toString()); // => {3,-4}
```

Daxili siniflərin prototipləri də açıqdır. Buna görə istəsəniz, `String`, `Number` və digər tiplərə metod əlavə edə bilərsiniz.

Məsələn, `startsWith` metodu mövcud deyilsə, onu əlavə etmək olar:

```javascript
if (!String.prototype.startsWith) {
    String.prototype.startsWith = function(s) {
        return this.indexOf(s) === 0;
    };
}

console.log("Salam".startsWith("Sa")); // => true
```

Başqa bir nümunə — `Number` tipinə `times` metodu əlavə etmək:

```javascript
Number.prototype.times = function(f) {
    let n = this.valueOf();
    for (let i = 0; i < n; i++) {
        f(i);
    }
};

(3).times(i => console.log(`Salam ${i + 1}`)); 
// Salam 1, Salam 2, Salam 3
```

**Diqqət!** Daxili tiplərin prototiplərinə müdaxilə etmək ümumiyyətlə tövsiyə edilmir. Gələcəkdə dilin özündə eyni adlı metodlar əlavə edilərsə, bu problemlərə səbəb ola bilər. Xüsusilə `Object.prototype`-a dəyişiklik etmək təhlükəlidir və qarşısı alınmalıdır.

---

### 9.5 Alt-siniflər (Subclasses)

Bir sinif (B) başqa sinfi (A) genişləndirərək alt-sinif ola bilər. B, A-nın metodlarını miras alır, həmçinin öz metodlarını əlavə edib A-nın metodlarını əvəz edə bilər.

Alt-sinif konstruktoru adətən üst-sinif konstruktorunu çağırır.

---

### 9.5.1 Alt-siniflər (Subclasses) və Prototiplər (Prototypes) 🔗

Tutaq ki, `Animal` adlı sinifimiz var və biz onun alt-sinfi `Dog` yaratmaq istəyirik. `Dog` həm `Animal`-ın metodlarını miras alacaq, həm də özünə xas metodu olacaq.

```javascript
// Üst-sinif: Animal
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    return `${this.name} səslənir.`;
};

// Alt-sinif: Dog
function Dog(name, breed) {
    Animal.call(this, name); // Animal konstruktorunu çağırırıq
    this.breed = breed;
}

// Dog prototipini Animal prototipindən miras aldırırıq
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

// Dog öz speak metodunu təyin edir (üst-sinifin metodunu əvəz edir)
Dog.prototype.speak = function() {
    return `${this.name} (${this.breed}) havlayır.`;
};

// İstifadə nümunəsi
let myDog = new Dog('Çakır', 'Kangal');
console.log(myDog.speak());  // Çakır (Kangal) havlayır.
```

---


### 9.5.2 `extends` və `super` ilə Alt-siniflər (Subclasses)

ES6 və sonrakı versiyalarda `extends` açar sözü ilə sinifdən alt-sinif yaratmaq çox asanlaşdı. Bu üsulla həm öz siniflərinizi, həm də daxili sinifləri (məsələn, `Array`, `Map`) genişləndirə bilərsiniz.

Aşağıda sadə `EZArray` alt-sinifi nümunəsi var. Bu sinif `Array`-dən miras alır və massivə iki yeni getter (`first` və `last`) əlavə edir:

```javascript
// Array-dən miras alan alt-sinif
class EZArray extends Array {
  get first() { return this[0]; }
  get last() { return this[this.length - 1]; }
}

let a = new EZArray();
a.push(1, 2, 3, 4);

console.log(a instanceof EZArray);  // true
console.log(a instanceof Array);    // true
console.log(a.pop());               // 4
console.log(a.first);               // 1
console.log(a.last);                // 3
console.log(a[1]);                  // 2
console.log(Array.isArray(a));      // true
console.log(EZArray.isArray(a));    // true
```

`EZArray`-ın instansiyası həm `EZArray`, həm də `Array` instansiyasıdır. İndi isə `super` açar sözünün necə istifadə olunmasına baxaq.

---

`super` sinifin alt-sinifində üst-sinifin konstruktorunu və metodlarını çağırmaq üçün istifadə olunur. Aşağıdakı nümunədə `Person` sinifindən miras alan `Employee` sinifi göstərilir:

```javascript
class Person {
    constructor(name) {
        this.name = name;
    }

    greet() {
        return `Salam, mənim adım ${this.name}`;
    }
}

class Employee extends Person {
    constructor(name, jobTitle) {
        super(name);  // Üst-sinif konstruktorunu çağırırıq
        this.jobTitle = jobTitle;
    }

    greet() {
        // Üst-sinif metodunu çağırıb əlavə məlumat əlavə edirik
        return `${super.greet()}. Mən ${this.jobTitle} vəzifəsindəyəm.`;
    }
}

let emp = new Employee("Rəşad", "Proqramçı");
console.log(emp.greet());
// Nəticə: Salam, mənim adım Rəşad. Mən Proqramçı vəzifəsindəyəm.
```

**Qaydalar:**

* Alt-sinif konstruktorunda `super()` çağırılmalıdır, əks halda `this` istifadə etmək olmaz.
* `super` həm konstruktor daxilində üst-sinif konstruktorunu, həm də metodlarda üst-sinif metodlarını çağırmaq üçün istifadə olunur.
* Əgər konstruktor təyin etməsəniz, alt-sinif avtomatik olaraq bütün arqumentləri `super()`-a ötürür.

---


### 9.5.3 İrsiyyət (Inheritance) əvəzinə Delegasiya (Delegation)

`extends` açar sözü ilə bir sinfi (class) irsən götürüb, ondan alt-sinif (subclass) yaratmaq mümkündür. Lakin bu, hər zaman düzgün yanaşma deyil. Bəzən bir sinfin funksionallığını təkrar istifadə etmək istəyirsinizsə, onu irsən götürmək yerinə, sadəcə bir instansiya yaradıb onun metodlarından **delegasiya** vasitəsilə istifadə etmək daha effektivdir.

Bu üsula **kompozisiya** (composition) deyilir və obyekt-yönlü proqramlaşdırmada tez-tez tövsiyə olunur:

> **“Irsiyyət əvəzinə kompozisiyaya üstünlük verin.”** (`Favor composition over inheritance`)

---

#### Nümunə: Histogram sinfi

Tutaq ki, `Set` kimi işləyən, amma əlavə olunan dəyərlərin neçə dəfə gəldiyini də izləyən bir `Histogram` sinfi yaratmaq istəyirik. Bu halda, `Set`-dən irs almaqdansa, **daxildə `Map` istifadə edib onun metodlarına delegasiya etmək** daha uyğundur.

```javascript
class Histogram {
    constructor() {
        this.map = new Map(); // Daxili xəritə
    }

    count(key) {
        return this.map.get(key) || 0;
    }

    has(key) {
        return this.count(key) > 0;
    }

    get size() {
        return this.map.size;
    }

    add(key) {
        this.map.set(key, this.count(key) + 1);
    }

    delete(key) {
        const count = this.count(key);
        if (count === 1) this.map.delete(key);
        else if (count > 1) this.map.set(key, count - 1);
    }

    [Symbol.iterator]() {
        return this.map.keys(); // Varsayılan iterator
    }

    keys() {
        return this.map.keys();
    }

    values() {
        return this.map.values();
    }

    entries() {
        return this.map.entries();
    }
}
```
---

## 9.5.4 Sinif İyerarxiyaları və Abstrakt Siniflər — Sadə və Real Nümunə

### Problem: Mesaj sistemində fərqli mesaj tiplərini saxlamaq istəyirik

Tutaq ki, bizim tətbiqimizdə fərqli mesaj növləri var:

* `TextMessage` — adi mətn mesajı
* `ImageMessage` — şəkil göndərmək üçün
* `VideoMessage` — video göndərmək üçün

Biz istəyirik ki, bütün bu mesaj tipləri müəyyən ortaq davranışlara sahib olsun, məsələn:

* `.send()`
* `.toString()`

---

### 🔧 Addım 1: Abstrakt sinif yaradırıq

```javascript
class AbstractMessage {
  send() {
    throw new Error("send() metodu abstraktdır və implementasiya olunmalıdır.");
  }

  toString() {
    throw new Error("toString() metodu abstraktdır və implementasiya olunmalıdır.");
  }
}
```

Bu sinif **birbaşa istifadə edilə bilməz**. Onun məqsədi: **ortaq metodların strukturunu müəyyən etməkdir**.

---

### Addım 2: Alt siniflər yaradılır

```javascript
class TextMessage extends AbstractMessage {
  constructor(content) {
    super();
    this.content = content;
  }

  send() {
    console.log(`Mətn göndərildi: ${this.content}`);
  }

  toString() {
    return `[Text] ${this.content}`;
  }
}

class ImageMessage extends AbstractMessage {
  constructor(imageUrl) {
    super();
    this.imageUrl = imageUrl;
  }

  send() {
    console.log(`Şəkil göndərildi: ${this.imageUrl}`);
  }

  toString() {
    return `[Image] ${this.imageUrl}`;
  }
}
```

---

### İstifadə

```javascript
const msg1 = new TextMessage("Salam, necəsən?");
const msg2 = new ImageMessage("https://example.com/image.jpg");

msg1.send();     // Mətn göndərildi: Salam, necəsən?
msg2.send();     // Şəkil göndərildi: https://example.com/image.jpg

console.log(msg1.toString());  // [Text] Salam, necəsən?
console.log(msg2.toString());  // [Image] https://example.com/image.jpg
```

---

### Yalnız abstrakt sinifi istifadə etmək olmaz

```javascript
const wrong = new AbstractMessage();
wrong.send(); // Xəta: send() metodu abstraktdır...
```

---

### 📌 Qısa Yekun:

| Term                          | İzah                                                                        |
| ----------------------------- | --------------------------------------------------------------------------- |
| `AbstractMessage`             | Abstrakt sinifdir, ümumi struktur verir                                     |
| `TextMessage`, `ImageMessage` | Konkret siniflərdir, `send()` və `toString()` metodlarını implement edirlər |
| `super()`                     | Abstrakt sinifin constructor-un çağırılmasıdır                              |

---
