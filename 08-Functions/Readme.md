# Fəsil 8. Funksiyalar (Functions) ⚙️

**Funksiyalar (Functions)** JavaScript proqramlarının əsas hissəsidir. Onlar bir dəfə təyin olunub dəfələrlə çağırıla bilən kod bloklarıdır. Funksiyalar **parametrlər (parameters)** qəbul edir və çağırış zamanı **arqumentlər** ötürülür.

Əgər funksiya obyektin xüsusiyyətidirsə, o, **metod (method)** adlanır.
Funksiya obyektin içindədirsə, ona metod, yeni obyekt yaradırsa **konstruktor** deyilir.

JavaScript-də funksiyalar **obyektlərdir (objects)**. Onları dəyişənlərə təyin edə, digər funksiyalara ötürə, hətta özlərinin də xüsusiyyətləri və metodları ola bilər. Funksiyalar başqa funksiyaların içində **iç-içə (nested)** ola bilər.

---

## 8.1 Funksiyaları Təyin Etmək (Defining Functions) 📝

JavaScript-də funksiyaları təyin etməyin ən geniş istifadə olunan yolu `function` açar sözüdür. ES6 isə `function` açar sözü olmadan **"ox funksiyalarını (arrow functions)"** təqdim edib ki, bu, daha yığcam sintaksisə malikdir.

---

### 8.1.1 Funksiya Bəyanatları (Function Declarations)

`Function` açar sözü ilə başlayır, ardınca funksiyanın adı, mötərizədə parametrlər və fiqurlu mötərizədə kod (funksiya gövdəsi) gəlir.

**Quruluşu:**
* **Ad (Identifier):** Funksiyanın adıdır, dəyişən kimi işləyir.
* **Parametrlər (Parameters):** Mötərizədə vergüllə ayrılmış dəyişən adlarıdır, funksiya daxilində yerli dəyişənlər kimidir.
* **Gövdə (Body):** Fiqurlu mötərizədəki koddur, funksiya çağırılanda icra olunur.

**Misallar:**

```javascript

function distance(x1, y1, x2, y2) { 
// İki nöqtə arasındakı məsafəni hesablayır
  let dx = x2 - x1;
  let dy = y2 - y1;
  return Math.sqrt(dx * dx + dy * dy);
}

function sum(a,b){
  // 2 verilmiş ədədin cəmini hesablayır
  return a + b;
}
```

Funksiyalar dəyər qaytarmalıdırsa, `return` ifadəsindən istifadə edirlər. Əgər `return` yoxdursa və ya dəyər göstərilməyibsə, funksiya `undefined` qaytarır. 

---

### 8.1.2 Funksiya İfadələri (Function Expressions)

Funksiya bəyanatlarına oxşayır, lakin daha böyük bir ifadənin hissəsi olurlar və adları **ixtiyaridir (optional)**.

**Misallar:**

```javascript
const square = function(x) { return x*x; }; 
// Dəyişənə təyin edilmiş funksiya ifadəsi

let tensquared = (function(x) {return x*x;}(10));
// Dərhal çağırılan funksiya ifadəsi (IIFE)
```

**Vacib Məqam:** Funksiya ifadələri **hoist olunmur**. Onlar yalnız təyin olunduqları ifadə icra olunduqdan sonra mövcud olurlar. Odur ki, funksiya ifadələrini təyin olunmazdan əvvəl çağırmaq mümkün deyil. Funksiyaya dəfələrlə istinad etmək üçün onu bir dəyişənə (adətən `const` ilə) təyin etmək lazımdır.

---

### 8.1.3 Ox Funksiyaları (Arrow Functions) ➡️

ES6-da **ox funksiyaları (arrow functions)** `function` açar sözü olmadan, `=>` (ox) operatoru ilə təyin olunur. Çox yığcam sintaksisə malikdirlər və ad tələb olunmur.

**Əsas Sintaksis:**
* Parametrlər mötərizədə, sonra `=>`, sonra fiqurlu mötərizədə funksiya gövdəsi:
    ```javascript
    const sum = (x, y) => { return x + y; };
    ```
* Əgər gövdə **tək bir `return` ifadəsidirsə**, `return`, nöqtəli vergül və fiqurlu mötərizələri qoymaya bilərsiniz:
    ```javascript
    const sum = (x, y) => x + y;
    ```
* Əgər funksiyanın **yalnız bir parametri varsa**, parametrin ətrafındakı mötərizələri də qoymaya bilərsiniz:
    ```javascript
    const polynomial = x => x*x + 2*x + 3;
    ```
* **Heç bir arqument yoxdursa**, boş mötərizələr `()` mütləqdir:
    ```javascript
    const constantFunc = () => 42;
    ```

**Qeydlər:**
* Parametrlər və `=>` arasında yeni sətir qoymayın (sintaktik səhvə səbəb ola bilər).


    ```javascript
    const f = x => { return { value: x }; }; 
    // {value: "5"}
    const g = x => ({ value: x });          
    // {value: "5"}
    const h = x => { value: x };            
    // undefined
    ```

Ox funksiyaları `map()`, `filter()` kimi **massiv metodlarına (array methods)** funksiya ötürərkən ideal seçimdir, çünki qısa yazılışa malikdirlər:

```javascript
let filtered = [1, null, 2, 3].filter(x => x !== null); 
// [1, 2, 3]
// null elementlər silinir
let squares = [1, 2, 3, 4].map(x => x*x);   
// [1, 4, 9, 16]           
// Hər elementi kvadrata yüksəldir
```
---

### 8.1.4 İç-içə Funksiyalar (Nested Functions) 🌳

JavaScript-də funksiyalar başqa funksiyaların daxilində **iç-içə (nested)** ola bilər.

**Misal:**

```javascript
function processArray(arr) {
  function isEven(n) { 
    // Daxili funksiya: ədədin cüt olduğunu yoxlayır
    return n % 2 === 0;
  }

  let evenNumbers = arr.filter(isEven);
  return evenNumbers.reduce((sum, n) => sum + n, 0);
}

console.log(processArray([1, 2, 3, 4, 5, 6])); // => 12 (2+4+6)
```

---

# 8.2 Funksiyaları Çağırmaq (Invoking Functions) 🚀

Funksiya kodları təyin olunanda deyil, **çağırılanda** icra olunur. JavaScript funksiyaları beş əsas yolla çağırıla bilər:
1.  **Funksiyalar kimi (As functions)**
2.  **Metodlar kimi (As methods)**
3.  **Konstruktorlar kimi (As constructors)**
4.  **`call()` və `apply()` metodları vasitəsilə dolayı yolla (Indirectly)**
5.  **Gizli olaraq (Implicitly)** (normal çağırış kimi görünməyən JavaScript xüsusiyyətləri ilə)

---

Budur **8.2.1 Funksiya Çağırışı** bölməsinin **daha çox example ilə zənginləşdirilmiş, qısa, aydın və axıcı yenilənmiş versiyası**:

---

### 8.2.1 Funksiya Çağırışı (Function Invocation) ⚡️

JavaScript-də funksiyalar **funksiya adı + mötərizə içində arqumentlər** ilə çağırılır:

```javascript
function greet(name) {
  console.log("Salam, " + name + "!");
}

greet("Ayan"); // Salam, Ayan!
```

Arqumentlər **mötərizədə qiymətləndirilir**, parametrlərə ötürülür. Əgər `return` istifadə edilmirsə, funksiya `undefined` qaytarır.

```javascript
function add(a, b) {
  return a + b;
}

let result = add(5, 7); 
console.log(result); // 12
```

---

### Mürəkkəb Misal: Birdən çox funksiyanın çağırılması

```javascript
function square(x) {
  return x * x;
}

function sumOfSquares(a, b) {
  return square(a) + square(b);
}

console.log(sumOfSquares(3, 4)); // 25
```

---

### ❓ Şərti Çağırış (Optional Chaining)

Bəzən funksiya mövcud olmaya bilər. ES2020-dən `?.` istifadə edərək **funksiya varsa çağırmaq**, yoxdursa keçmək mümkündür:

```javascript
let f = null;
f?.(5); // Heç nə etmir, error atmır

let logger = (msg) => console.log(msg);
logger?.("Hello!"); // Hello!
```

---

### 🔹 `this` dəyəri

Funksiya çağırılarkən **`this` konteksti** dəyişə bilər:

* **Adi çağırışda:**

  ```javascript
  function showThis() {
    console.log(this);
  }
  showThis(); 
  // Non-strict: qlobal obyekt, Strict: undefined
  ```

* **Obyekt metodu kimi çağırışda:**

  ```javascript
  const user = {
    name: "Samir",
    greet() {
      console.log("Salam, " + this.name);
    }
  };
  user.greet(); // Salam, Samir
  ```

* **Arrow functions:**

  ```javascript
  const obj = {
    value: 42,
    show: () => console.log(this.value)
  };
  obj.show(); 
  // undefined (arrow this miras alır, obyektə bağlanmır)
  ```

---

### 🌀 Rekursiv Funksiyalar və Call Stack

**Rekursiv funksiya** özünü çağırır. Hər çağırışda `call stack`-a yeni kontekst əlavə edilir.

```javascript
function countdown(n) {
  if (n <= 0) {
    console.log("Hazır!");
  } else {
    console.log(n);
    countdown(n - 1);
  }
}
countdown(5);
// 5
// 4
// 3
// 2
// 1
// Hazır!
```

Çox dərin rekursiya:

```javascript
function forever() {
  return forever();
}
forever(); 
// "Maximum call stack size exceeded" error verir
```

---

### 8.2.2 Metod Çağırışı (Method Invocation) ⚙️

**Metod** obyektin içində saxlanan funksiyadır. Çağırışda:

```javascript
object.method(args);
```

şəklində istifadə olunur və **`this` həmin obyektə işarə edir.**

---

###  Sadə Misal

```javascript
let user = {
  name: "Aylin",
  sayHi() {
    console.log("Salam, " + this.name);
  }
};

user.sayHi(); 
// Salam, Aylin
```

Burada `sayHi()` metodu **`user` obyektinə bağlıdır**, `this` → `user`.

---

### 📌 Parametrlə Çağırış

```javascript
let calculator = {
  add(a, b) {
    console.log(a + b);
  }
};

calculator.add(3, 4); 
// 7
```

---

### 📌 Kvadrat Mötərizə ilə Çağırış

Xüsusiyyət adı dəyişkən olduqda:

```javascript
let methodName = "sayHi";
user[methodName](); 
// Salam, Aylin
```

---

### 📌 Metod Zənciri (Method Chaining)

Əgər metod **obyekti qaytararsa**, ardıcıl metod çağırmaq mümkündür:

```javascript
let counter = {
  value: 0,
  increment() {
    this.value++;
    return this;
  },
  decrement() {
    this.value--;
    return this;
  },
  show() {
    console.log(this.value);
    return this;
  }
};

counter.increment().increment().decrement().show(); 
// 1
```

---

### 8.2.3 Konstruktor Çağırışı (Constructor Invocation) 🏗️

Əgər funksiya **`new` ilə çağırılırsa**, bu **konstruktor çağırışıdır** və **yeni obyekt yaratmaq** üçün istifadə olunur.

---

### 📌 Əsas Sintaksis

```javascript
let obj = new Object();
let arr = new Array(1, 2, 3);
let date = new Date();
```
---

### 📌 Sadə Konstruktor Nümunəsi

```javascript
function User(name, age) {
  this.name = name;
  this.age = age;
}

let user1 = new User("Aylin", 25);
console.log(user1.name); 
// Aylin
console.log(user1.age);  
// 25
```

---

### 🔹 `this` dəyəri

Konstruktor çağırışında `this` həmişə **yeni yaradılan obyektə** işarə edir.

```javascript
function Point(x, y) {
  this.x = x;
  this.y = y;
}

let p = new Point(3, 4);
console.log(p.x, p.y); 
// 3 4
```
---

### 📌 `return` Davranışı

* Konstruktor funksiyasında `return` yazılmasa belə, **yeni obyekt avtomatik qaytarılır.**
* Əgər konstruktor **primitiv dəyər** (number, string, boolean və s.) qaytararsa, yenə də yeni obyekt qaytarılır.

**Misallar:**

```javascript
function A() {
  this.value = 10;
}
console.log(new A().value); 
// 10

function B() {
  return { x: 1 };
}
console.log(new B()); 
// { x: 1 }

function C() {
  return 42; 
  // primitiv dəyər
}
console.log(new C()); 
// C { } --> yeni obyekt, 42 ignore edilir
```

---

```javascript
function Rectangle(width, height) {
  this.width = width;
  this.height = height;
  this.area = function() {
    return this.width * this.height;
  };
}

let rect = new Rectangle(5, 10);
console.log(rect.area()); // 50
```

---

### 8.2.4 Dolayı Çağırış (Indirect Invocation)

JavaScript-də funksiyalar obyekt olduğu üçün onların `call()` və `apply()` metodları mövcuddur. Bu metodlar funksiyanı dolayı yolla çağırmağa və `this` dəyərini açıq şəkildə təyin etməyə imkan verir.

Bu üsulla funksiyanı başqa obyekt üzərində metod kimi işlətmək olur, həmin obyektin özündə həmin metod olmasa belə.

---

#### `call()` və `apply()` arasındakı fərq

* `call()` arqumentləri bir bir qəbul edir.
* `apply()` arqumentləri massiv (array) şəklində qəbul edir.

Hər ikisinin ilk arqumenti `this` dəyərinin hansı obyekt olacağını göstərir.

---

#### Nümunə: `call()`

```javascript
function greet() {
  console.log("Hello, " + this.name);
}

let user = { name: "Alice" };
greet.call(user); // Hello, Alice
```

Burada `greet` funksiyası `user` obyektinin `name` xüsusiyyətini oxuyur, çünki `call()` vasitəsilə `this` həmin obyektə bağlanır.


#### Nümunə: `apply()`

```javascript
introduce.apply(person, ["London", 30]);
// My name is John, I am 30 years old, living in London.
```

---    

# 8.3 Funksiya Arqumentləri və Parametrləri (Function Arguments and Parameters)

JavaScript-də funksiya parametrləri üçün tip təyin edilmir və funksiyalar çağırıldıqda ötürülən arqumentlərin nə tipi, nə də sayı yoxlanılır. Bu, proqramçıya çeviklik versə də, bəzi hallarda arqumentlərin tipi və sayı üzərində nəzarət vacib ola bilər.

---

### 8.3.1 İxtiyari Parametrlər və Standart Dəyərlər (Optional Parameters and Defaults)

JavaScript funksiyaları bəzi arqumentlər ötürülmədikdə `undefined` qəbul edir. Bu vəziyyətlərdə parametrlər üçün standart dəyər təyin edilə bilər.

**Köhnə üsul (standart dəyər yoxdursa):**

```javascript
function multiply(a, b) {
  if (b === undefined) b = 1;
  return a * b;
}

console.log(multiply(4));    // 4
console.log(multiply(4, 2)); // 8
```

**ES6 ilə standart dəyər təyini:**

```javascript
function greet(name, greeting = "Hello") {
  console.log(greeting + ", " + name);
}

greet("Aysel");           // Hello, Aysel
greet("Rəşad", "Salam");  // Salam, Rəşad
```

**`null` ötürüldükdə standart dəyər işləmir:**

```javascript
greet("Tunar", null); // null, Tunar
```

**Standart dəyər başqa parametrdən asılı ola bilər:**

```javascript
function area(width, height = width * 2) {
  return width * height;
}

console.log(area(3));    // 18
console.log(area(3, 4)); // 12
```

**Standart dəyər funksiyanın nəticəsi ola bilər:**

```javascript
function random() {
  return 5;
}

function getNumber(n = random()) {
  return n;
}

console.log(getNumber());   // 5
console.log(getNumber(10)); // 10
```

---

### 8.3.2 Rest parametrlər və dəyişən uzunluqlu arqument siyahıları (Rest Parameters and Variable-Length Argument Lists)

Funksiyaya neçə arqument ötürüləcəyini əvvəlcədən bilmədiyimiz hallarda **rest parameter** (`...`) sintaksisindən istifadə edilir. Bu sintaksis funksiyaya ötürülən artıq arqumentləri **massiv** kimi toplamağa imkan verir.

---

#### Rest parameter sintaksisi

```javascript
function sum(...args) {
  let total = 0;
  for (let n of args) {
    total += n;
  }
  return total;
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(10, 20, 30, 40)); // 100
```

> Burada `args` bir `rest parameter`-dir və funksiyaya ötürülən bütün arqumentləri massiv kimi saxlayır.

---

#### Rest parameter digər parametrlə birlikdə

```javascript
function log(type, ...messages) {
  console.log(type + ": " + messages.join(", "));
}

log("Error", "Disk full", "Low memory");
// Error: Disk full, Low memory
```

---

#### Qaydalar

* `rest parameter` `...` simvolu ilə yazılır
* Funksiya təyinatında **yalnız bir rest parameter** ola bilər
* **Həmişə sonuncu parametr** olmalıdır
* Rest parametrlər həmişə **array** tipində olur (boş ola bilər, `undefined` ola bilməz)

---


| Termin                | İzah                                                                                          |
| --------------------- | --------------------------------------------------------------------------------------------- |
| **Rest parameter**    | Funksiya təyinatında `...` ilə başlayan və bütün artıq arqumentləri massivə toplayan parametr |
| **Variadic function** | İstənilən sayda arqument qəbul edən funksiyalar (məsələn: `console.log()`)                    |
| **Arity**             | Funksiyanın qəbul etdiyi arqumentlərin sayı                                                   |

> Rest parameter ilə yazılmış funksiyalar `variadic functions` adlanır.

---

### 8.3.3 `arguments` Objekti (The Arguments Object)

ES6-dan əvvəl funksiyaya ötürülən arqumentlərə daxil olmaq üçün `arguments` adlı xüsusi obyekt istifadə olunurdu. Bu obyekt massivə bənzərdir və bütün arqumentləri indekslə təqdim edir: `arguments[0]`, `arguments[1]`, və s.

---

**Nümunə:**

```javascript
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

console.log(sum(1, 2, 3, 4)); // 10
console.log(sum(10, 20));     // 30
```

---

**Qeydlər:**

* `arguments` **array deyil**, amma `length` və indekslər var
* `ES6`-dan sonra `rest parameter` (`...args`) onu əvəz edir

---

### 8.3.4  Spread Operatoru (`...`)

**Spread operatoru (`...`)** ES6 ilə gəlib. Massivin və ya iterable obyektin elementlərini funksiya çağırışında ayrıca arqument kimi açır.

```javascript
const letters = ['a', 'b', 'c'];
console.log(...letters); // a b c
```

---

### Rest parametrlə birlikdə istifadə

Funksiya təyinatında `rest parameter` (`...args`) arqumentləri massiv kimi yığır, spread operator isə çağırışda həmin massiv elementlərini açır.

```javascript
function wrap(f) {
  return function(...args) {
    console.log('Function started');
    let result = f(...args);
    console.log('Function ended');
    return result;
  }
}

function multiply(a, b) {
  return a * b;
}

const wrappedMultiply = wrap(multiply);
wrappedMultiply(4, 5); // 20
```

---

### Əlavə nümunə

Massivləri birləşdirmək üçün:

```javascript
const arr1 = [10, 20];
const arr2 = [30, 40];
const combined = [...arr1, ...arr2];
console.log(combined); 
// [10, 20, 30, 40]
```

---

**Qeyd:**

* `rest parameter` arqumentləri massiv kimi toplayır.
* Spread operator massiv elementlərini ayrıca arqument kimi açır.

---


### 8.3.5 Funksiya Arqumentlərini Parametrlərdə Açmaq (Destructuring Arguments into Parameters)

Funksiyaya arqument kimi massiv və ya obyekt göndərərkən, onları funksiyanın içində ayrıca dəyişənlərə bölmək olar. Bu üsula **destrukturlama** deyilir.

Məsələn, əgər funksiya iki elementli massiv gözləyirsə, həmin elementləri ayrıca dəyişənlər kimi parametrdə götürmək mümkündür.

#### Massiv arqumentini parametrlərdə açmaq

```javascript
function sum([a, b]) {
  return a + b;
}

sum([3, 5]); // => 8
```

Burada `[a, b]` yazmaqla, funksiya `sum` massivdəki birinci elementi `a`, ikinci elementi `b` kimi qəbul edir.

---

#### Obyekt arqumentini parametrlərdə açmaq

Eyni şeyi obyektlərlə də edə bilərik:

```javascript
function area({width, height}) {
  return width * height;
}

area({width: 5, height: 10}); // => 50
```

Burada `{width, height}` funksiyanın parametri olub, obyektin `width` və `height` xüsusiyyətlərini ayrıca dəyişənlər kimi qəbul edir.

---

#### Parametr adlarını dəyişmək

Əgər funksiyanın içində dəyişən adları fərqli olmalıdırsa, belə yazılır:

```javascript
function area({width: w, height: h}) {
  return w * h;
}

area({width: 5, height: 10}); // => 50
```

Burada `width` obyektin açarı, `w` isə funksiyanın parametri adıdır.

----

#### Standart dəyərlər vermək

Əgər arqumentdə bəzi açarlar yoxdursa, onlara standart dəyər təyin etmək olar:

```javascript
function area({width = 1, height = 1}) {
  return width * height;
}

area({width: 5}); // => 5 (height default olaraq 1 götürülür)
```

#### Çoxlu ixtiyari arqumentləri obyektlə göndərmək

Əgər funksiyaya çoxlu arqument ötürmək istəyirsinizsə, amma çoxlu parametrlər yaratmaq istəmirsinizsə, bir obyekt istifadə edin:

```javascript
function copy({source, target, count = 1}) {
  for(let i = 0; i < count; i++) {
    target.push(source[i]);
  }
  return target;
}

let a = [1, 2, 3], b = [];
copy({source: a, target: b, count: 2}); // => [1, 2]
```

---

#### Massiv içində qalan elementləri toplamaq (Rest operator)

Massiv arqumentini destrukturlayarkən, qalan elementləri toplamaq üçün `...` istifadə olunur:

```javascript
function test([first, second, ...rest]) {
  return rest;
}

test([1, 2, 3, 4, 5]); // => [3, 4, 5]
```
---

#### Obyekt içində qalan açarları toplamaq (ES2018+)

Eyni qayda obyekt üçün də keçərlidir:

```javascript
function func({a, b, ...others}) {
  return others;
}

func({a:1, b:2, c:3, d:4}); // => {c:3, d:4}
```
---

### 8.3.6 Arqument Tipləri (Argument Types) 🛠️

JavaScript-də funksiya parametrlərinin **təyin olunmuş tipləri yoxdur (no declared types)** və funksiyaya ötürülən arqumentlərə **tip yoxlaması (type checking)** avtomatik edilmir. Buna görə, kodun aydın və səhvsiz işləməsi üçün arqumentlərə mənalı adlar vermək və lazım gəldikdə şərhlərlə izah etmək vacibdir.

JavaScript **lazım olanda tip çevirmələri (type coercion)** edir (§3.9). Məsələn, string gözləyən funksiyaya başqa tipdən dəyər versəniz, funksiya onu avtomatik stringə çevirə bilər. Bu, ümumiyyətlə səhvsiz işləyir, çünki primitivlərin hamısı `toString()` metoduna malikdir.

---

**Tip Yoxlaması Misalı:**

```javascript
// 'a' adlı iterable obyektin elementlərinin cəmini hesablayır.
// Bütün elementlər rəqəm olmalıdır.
function sum(a) {
  let total = 0;
  for (let element of a) {
    if (typeof element !== "number") {
      throw new TypeError("sum(): elements must be numbers");
    }
    total += element;
  }
  return total;
}

console.log(sum([1, 2, 3])); 
// => 6

// Səhv nümunələr:
// sum(1, 2, 3);         // TypeError: 1 is not iterable
// sum([1, 2, "3"]);     // TypeError: element 2 is not a number
```

Bu nümunədə `sum` funksiyası arqumentin iterable olub olmadığını yoxlayır (for...of ilə) və hər elementin tipinin `number` olduğunu yoxlayır. Əgər tip uyğun deyilsə, `TypeError` atır.

---

### 8.4 Funksiyalar Dəyərlər Kimi (Functions as Values)

JavaScript-də funksiyalar yalnız əməliyyat aparmaq üçün deyillər – onlar həm də **dəyər (value)** kimi istifadə oluna bilirlər. Bu, onları daha çevik və güclü edir.

###  Funksiyaları necə dəyər kimi istifadə edə bilərik?

* Dəyişənlərə təyin edə bilərik.
* Obyekt və massivlərdə saxlaya bilərik.
* Başqa funksiyalara arqument kimi ötürə bilərik.
* Və funksiyalardan geri qaytara da bilərik.

---

### Funksiya dəyişən və obyekt içində

```javascript
function square(x) { return x * x; }

let s = square;             // Funksiyanı dəyişənə təyin edirik
console.log(s(4));          // => 16 (eyni funksiyanı çağırır)

let o = {
  square: function(x) {
    return x * x;
  }
};

console.log(o.square(16));  // => 256 (obyekt içində metod kimi)
```

---

### Massivdə Funksiya

```javascript
let a = [x => x * x, 20];
console.log(a[0](a[1])); // => 400
```

Burada `a[0]` bir funksiyadır, `a[1]` isə ədəddir. Funksiyanı həmin ədədlə çağırırıq.

---

### Real istifadəyə nümunə: `Array.sort()`

Massivi çeşidləyərkən istədiyimiz qaydanı biz təyin edə bilərik:

```javascript
let nums = [10, 2, 5];
nums.sort((a, b) => a - b); 
// Artan sıra
```

Burada `sort()` funksiyası başqa bir funksiyanı (comparison function) qəbul edir.

---

### Funksiyaları Arqument Kimi Ötürmək

```javascript
function add(x, y) { return x + y; }
function multiply(x, y) { return x * y; }

function operate(operator, a, b) {
  return operator(a, b); 
  // Funksiya arqument kimi ötürülür
}

let result = operate(add, operate(add, 2, 3), operate(multiply, 4, 5));
console.log(result); 
// => 25
```

---

### Funksiyalar Obyekt Daxilində

```javascript
const operators = {
  add: (x, y) => x + y,
  subtract: (x, y) => x - y,
  multiply: (x, y) => x * y,
  divide: (x, y) => x / y,
  pow: Math.pow
};

function operate2(name, x, y) {
  let func = operators[name];
  if (typeof func === "function") {
    return func(x, y);
  } else {
    throw "unknown operator";
  }
}

console.log(operate2("add", "hello ", "world")); // => "hello world"
console.log(operate2("pow", 10, 2));             // => 100
```

##  8.4.1 – Funksiyaların Öz Xüsusiyyətləri

JavaScript-də funksiyalar həm də **obyekt olduğuna** görə, onlara adətən obyektə etdiyimiz kimi **xüsusiyyət (property)** əlavə edə bilərik. Bu xüsusiyyətlər, funksiyanın xarici dəyişənlərə ehtiyac duymadan **daxili məlumat (internal state)** saxlamasına imkan verir.

---

### Call Counter

```javascript
function greet(name) {
  greet.count++; // hər çağırışda say artır
  return `Salam, ${name}!`;
}

greet.count = 0; // xüsusiyyəti təyin edirik

greet("Rəşad");
greet("Turan");
greet("Leyla");

console.log(`Greet funksiyası ${greet.count} dəfə çağırılıb.`); 
// => Greet funksiyası 3 dəfə çağırılıb.
```

 Burada `greet.count` funksiyanın **özünə əlavə edilmiş bir sayğacdır**. Bu cür daxili "state" saxlamaq üçün faydalıdır.

---

###  Keşləmə ilə Sadə Kvadrat Funksiyası

```javascript
function cachedSquare(n) {
  if (!(n in cachedSquare.cache)) {
    cachedSquare.cache[n] = n * n;
  }
  return cachedSquare.cache[n];
}

cachedSquare.cache = {}; // Xüsusiyyət kimi boş obyekt

console.log(cachedSquare(4)); // 16 (hesablayır və yadda saxlayır)
console.log(cachedSquare(4)); // 16 (birbaşa keşdən götürür)
console.log(cachedSquare.cache); // { "4": 16 }
```

Bu nümunədə `cachedSquare.cache` funksiyanın **öz yaddaşı** kimi işləyir. Bu texnika **memoization** adlanır – təkrar hesablamalardan qaçmaq üçün faydalıdır.

---

### 8.5 Funksiyalar Ad Məkanları Kimi (Functions as Namespaces)

JavaScript-də funksiyalar öz daxilində **yerli (local)** dəyişənlər yaradır. Bu dəyişənlər yalnız həmin funksiyanın daxilində görünür və funksiyanın xaricindən onlara birbaşa giriş mümkün deyil.

Bu xüsusiyyət sayəsində funksiyalar **ad məkanı (namespace)** kimi istifadə edilə bilər. Yəni, çoxlu dəyişənləri qlobal sahədə saxlamaq əvəzinə, onları bir funksiyanın daxilində qruplaşdırırıq. Bu, proqramın digər hissələrində eyni adlı dəyişənlərin qarışmasının qarşısını alır.

### Nümunə

Tutaq ki, aşağıdakı kimi iki funksiya var:

```javascript
function first() {
  let message = "Salam";
  console.log(message);
}

function second() {
  let message = "Hello";
  console.log(message);
}

first();  // Konsola "Salam" yazır
second(); // Konsola "Hello" yazır
```

Hər iki funksiyada `message` adlı dəyişən var, amma onlar fərqli namespace - də yerləşdiyindən bir-birinə müdaxilə etmirlər.

---

### İmmediately Invoked Function Expression (IIFE)

Bəzən kodu sadəcə namespace yaratmaq üçün funksiyanın içində yazıb dərhal icra etmək istəyirik. Bunun üçün anonim funksiya ifadəsindən istifadə olunur və bu funksiya dərhal çağırılır.

```javascript
(function() {
  let count = 0; // Lokal dəyişən
  console.log("İcra olunur, say: " + count);
})();
```

Yuxarıdakı anonim funksiyanın dərhal çağırılması **IIFE** adlanır. Sintaksisin başa düşülməsi üçün `function` sözündən əvvəl mötərizə qoymaq vacibdir. Bu, JavaScript-ə bildirir ki, bu, funksiya bəyanatı deyil, ifadədir.

---

### 8.6 Kloujurlar (Closures)

JavaScript-də funksiyalar **leksik skoplaşma (lexical scoping)** prinsipi ilə işləyir. Bu o deməkdir ki, funksiyanın daxilində istifadə olunan dəyişənlər onun **harada təyin olunduğuna** görə müəyyənləşir, çağırıldığı yerə görə yox.

Buna görə, funksiyanın təyin olunduğu yerdəki skopdakı dəyişənlərə funksiya çağırılanda da daxil olmaq mümkündür. Bu xüsusiyyət kompüter elmlərində **kloujur (closure)** adlanır.

---

### Kloujurlar nədir?

Kloujur – funksiya ilə onu əhatə edən skopun birgə saxlanmasıdır. Başqa sözlə, funksiya təyin edildiyi yerdəki dəyişənləri yadda saxlayır və həmin dəyişənlərə funksiyanın başqa yerdən çağırılması zamanı da daxil ola bilir.

---

### Leksik Skopun İşi:

```javascript
let scope = "global";

function checkScope() {
  let scope = "local";
  function inner() {
    return scope;
  }
  return inner();
}

console.log(checkScope()); // "local"
```

`inner()` funksiyası `scope` dəyişənini axtaranda onu **öz funksiyasından yuxarıdakı** skoplarda tapır. Burada `inner()`-in daxil olduğu skopda `scope` "local" olduğu üçün o qaytarılır.

---

### Kloujur nümunəsi: Funksiyanı geri qaytarmaq

```javascript
let scope = "global";

function checkScope() {
  let scope = "local";
  function inner() {
    return scope;
  }
  return inner; // funksiyanı çağırmır, sadəcə geri qaytarır
}

let func = checkScope();
console.log(func()); // "local"
```

Bu misalda `inner` funksiyası `checkScope`-un daxilində təyin olunub, amma sonra `checkScope`-dan çıxaraq `func` dəyişəninə təyin olunur. `func()` çağırışında belə, `inner` funksiyası öz təyin olunduğu `scope` dəyişənini yadda saxlayır və onu qaytarır.

---

### Kloujurlardan şəxsi vəziyyət kimi istifadə

Kloujurlar, funksiya daxilində olan dəyişənləri xaricdən gizlətmək, ancaq funksiyaya daxil olmaqla bu dəyişənləri oxumaq və dəyişmək imkanı vermək üçün çox yararlıdır.

```javascript
let uniqueInteger = (function() {
  let counter = 0;
  return function() {
    return counter++;
  };
}());

console.log(uniqueInteger()); // 0
console.log(uniqueInteger()); // 1
// 'counter' dəyişəninə xaricdən daxil olmaq mümkün deyil
```

Burada `counter` dəyişəni `uniqueInteger` funksiyasının daxilində **şəxsi vəziyyət** kimi saxlanılır və başqa koddan əlçatmazdır.

---

### Bir neçə funksiyanın eyni şəxsi vəziyyəti paylaşması

```javascript
function counter() {
  let n = 0;
  return {
    count: function() { return n++; },
    reset: function() { n = 0; }
  };
}

let c1 = counter();
let c2 = counter();

console.log(c1.count()); // 0
console.log(c2.count()); // 0
c1.reset();
console.log(c1.count()); // 0
console.log(c2.count()); // 1
```

Hər `counter()` çağırışı öz `n` dəyişənini yaradır və nəticədə fərqli şəxsi vəziyyətə malik obyektlər əmələ gəlir.

---

### Kloujurlar və getter/setter metodları

```javascript
function counter(n) {
  return {
    get count() { return n++; },
    set count(value) {
      if (value > n) n = value;
      else throw new Error("Count yalnız artırıla bilər");
    }
  };
}

let c = counter(1000);
console.log(c.count); // 1000
console.log(c.count); // 1001
c.count = 2000;
console.log(c.count); // 2000
// c.count = 1000;  // Xəta: Count yalnız artırıla bilər
```

`n` burada şəxsi dəyişəndir, ona birbaşa daxil olmaq mümkün deyil, amma getter və setter vasitəsilə idarə olunur.

---

### Dövrdə kloujurlar və dəyişənlərin paylaşılması

`var` ilə yazılan dövrdə dəyişənlər blok skopunda deyil, funksional skopda təyin olunduğu üçün kloujurlar gözlənilməz davranış göstərə bilər.

```javascript
function constFuncs() {
  let funcs = [];
  for(var i = 0; i < 10; i++) {
    funcs[i] = function() { return i; };
  }
  return funcs;
}

let fs = constFuncs();
console.log(fs[5]()); // 10
```

Burada bütün funksiyalar eyni `i` dəyişənini paylaşır və `i` dövr bitdikdən sonra `10` olur.

---

### Həlli: `let` ilə blok skop yaratmaq

```javascript
function constFuncs() {
  let funcs = [];
  for(let i = 0; i < 10; i++) {
    funcs[i] = function() { return i; };
  }
  return funcs;
}

let fs = constFuncs();
console.log(fs[5]()); // 5
```

`let` hər dövr iterasiyasında yeni `i` yaratdığı üçün hər funksiya öz `i`-sini yadda saxlayır.

---

### 8.7 Funksiya Xüsusiyyətləri, Metodları və Konstruktoru

JavaScript-də funksiyalar təkcə kod blokları deyil, həm də xüsusi obyektlərdir. `typeof` operatoru funksiyaya tətbiq olunanda "function" dəyərini qaytarır. Amma əslində, funksiya JavaScript obyektinin xüsusi növüdür. Bu səbəbdən funksiyalar da obyektlər kimi müxtəlif xüsusiyyətlərə və metodlara sahib ola bilirlər. Bundan əlavə, JavaScript-də yeni funksiya yaratmaq üçün `Function()` adlı konstruktor mövcuddur.

---

### 8.7.1 `length` Xüsusiyyəti

Hər funksiyanın `length` adlı oxuna bilən xüsusiyyəti var. Bu, funksiyanın təyin etdiyi parametr sayını göstərir. Qısaca desək, `length` funksiyanın gözlədiyi arqumentlərin sayını bildirir.

Misal üçün:

```javascript
function f(a, b, c) {}
console.log(f.length); // 3

function g(a, b, ...rest) {}
console.log(g.length); // 2 (rest parametri sayılmır)
```

Burada `rest` parametri (`...rest`) funksiyanın gözlədiyi arqument sayına daxil edilmir.

---

### 8.7.2 `name` Xüsusiyyəti

Funksiyanın `name` xüsusiyyəti funksiyanın adını saxlayır. Bu, funksiyanın təyin olunarkən verdiyiniz adı və ya adlandırılmış funksiya ifadəsinin adını göstərir. Bu xüsusiyyət adətən səhv mesajlarında və debugging zamanı istifadə olunur.

```javascript
function myFunc() {}
console.log(myFunc.name); // "myFunc"

const anon = function() {};
console.log(anon.name); // "anon" (əgər dəyişən adı varsa)

const arrow = () => {};
console.log(arrow.name); // "arrow"
```

---

### 8.7.3 `prototype` Xüsusiyyəti

Bütün normal funksiyaların (amma ox funksiyalarının yox) `prototype` adlı bir xüsusiyyəti olur. Bu `prototype` obyektinə istinad edir. JavaScript-də konstruktor funksiyaları bu prototip obyektindən yeni yaradılan obyektlərə xüsusiyyətləri miras verirlər.

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  console.log("Salam, mən " + this.name);
};

const p = new Person("Rəşad");
p.greet(); // Salam, mən Rəşad
```

---

### 8.7.4 `call()` və `apply()` Metodları

JavaScript-də `call()` və `apply()` metodları bir funksiyanı başqa bir obyektin metodu kimi çağırmağa imkan verir. Bu metodlar funksiyanın çağırış kontekstini (yəni `this` dəyərini) təyin edir.

* `call()` metodunda funksiyaya ötürülən arqumentlər vergüllə ayrılmış formada verilir.
* `apply()` metodunda isə arqumentlər massiv şəklində ötürülür.

Misal:

```javascript
function f(a, b) {
  console.log(this, a, b);
}

const o = {name: "obyekt"};

f.call(o, 1, 2);     // this = o, arqumentlər 1 və 2
f.apply(o, [1, 2]);  // this = o, arqumentlər [1, 2] massivindən götürülür
```

Bu üsullar `this` dəyərini dəyişdirərək funksiyanı istənilən obyekt üzərində işlətməyə imkan verir.

Qeyd etmək lazımdır ki, ox funksiyaları `this` dəyərini öz təyin olunduğu mühitdən alır və `call()` və `apply()` ilə `this`-i dəyişmək mümkün deyil.

`apply()` metodu xüsusilə ixtiyari sayda arqument qəbul edən funksiyalarda faydalıdır. Məsələn, massivdəki ən böyük rəqəmi tapmaq üçün:

```javascript
let arr = [5, 2, 10, -1, 9];
let max = Math.max.apply(Math, arr);
console.log(max); // 10
```

---

### 8.7.5 `bind()` Metodu

`bind()` metodu funksiyanı müəyyən obyektə bağlayır və yeni funksiya yaradır. Bu yeni funksiyanı çağırdıqda, orijinal funksiya həmişə bağlandığı obyekt üzərində işləyir.

```javascript
function f(y) {
  return this.x + y;
}

let o = { x: 1 };
let g = f.bind(o); // f funksiyasını 'o' obyektinə bağlayır

console.log(g(2)); // 3 (this.x = 1, y = 2)
```

`bind()` funksiyanı bağlamaqla yanaşı, arqumentlərin qismən tətbiqinə də imkan verir. Yəni, funksiyanın əvvəlki arqumentlərini əvvəlcədən təyin etmək mümkündür.

```javascript
let sum = (x, y) => x + y;

let succ = sum.bind(null, 1); 
// x = 1 bağlanıb, y çağırış zamanı verilir
console.log(succ(2)); // 3
```

Bu üsul funksional proqramlaşdırmada geniş istifadə olunur və **partial application** (qismən tətbiq) adlanır.

Arrow funksiyaları `this` dəyərini bağlamaq üçün `bind()` metodundan istifadə edilə bilməz, çünki onlar `this`-i təyin olunduqları mühitdən götürürlər.

---

### 8.7.6 `toString()` Metodu

Bütün JavaScript obyektləri kimi, funksiyaların da `toString()` adlı metodu var. Bu metod funksiyanın mətn şəklində təsvirini qaytarır. ECMAScript spesifikasiyası `toString()` metodunun funksiyanın sintaksisinə uyğun mətn qaytarmasını tələb edir.

Praktikada, əksər müasir brauzerlər və mühitlər funksiyanın tam mənbə kodunu qaytarır. Məsələn:

```javascript
function sum(a, b) {
  return a + b;
}

console.log(sum.toString());
```

Nəticədə belə bir mətn görünə bilər:

```javascript
"function sum(a, b) {
  return a + b;
}"
```

Lakin, daxili funksiyalar (məsələn, `Math.max`) üçün bu metod, funksiyanın tərifi əvəzinə, adətən belə bir yazı qaytarır:

```javascript
"function max() { [native code] }"
```

Bu o deməkdir ki, həmin funksiya JavaScript dili ilə deyil, mühitin özündə natamam (native) şəkildə təmin olunub.

---

### 8.7.7 `Function()` Konstruktoru

JavaScript-də yeni funksiyalar yaratmaq üçün `Function()` adlı konstruktor mövcuddur. Bu konstruktorun arqumentləri funksiyanın parametr adları və funksiyanın gövdəsinin mətni şəklində verilir.

Misal:

```javascript
const f = new Function("x", "y", "return x * y;");
console.log(f(2, 3)); // 6
```

Yuxarıdakı kod aşağıdakı funksiya ilə eyni işi görür:

```javascript
const f = function(x, y) {
  return x * y;
};
```

`Function()` konstruktoruna ötürülən sonuncu arqument funksiyanın gövdəsidir, digər arqumentlər isə parametr adlarıdır. Əgər parametr yoxdursa, sadəcə gövdə mətni verilir.

Konstruktor dinamik funksiyalar yaratmağa imkan verir, lakin bəzi vacib xüsusiyyətləri var:

* `Function()` ilə yaradılan funksiyalar **leksik skopda** deyil, **qlobal skopda** işləyir. Bu o deməkdir ki, onlar tərtib olunduqları yerdəki dəyişənləri görə bilmirlər.

Misal:

```javascript
let scope = "qlobal";

function makeFunction() {
  let scope = "yerli";
  return new Function("return scope;");
}

console.log(makeFunction()()); // "qlobal"
```

* `Function()` konstruktordan istifadə zamanı funksiyanın mətninin təhlili (parsing) hər dəfə təzədən aparılır, bu isə performansa mənfi təsir göstərə bilər.

* Əksər hallarda bu konstruktorun istifadəsinə ehtiyac olmur, çünki funksiyaları adi sintaksis ilə yaratmaq daha sərfəlidir və təhlükəsizdir.

---

# 8.8 Funksional Proqramlaşdırma

JavaScript rəsmi olaraq Lisp, Haskell və ya OCaml kimi tam funksional proqramlaşdırma dili deyil. Buna baxmayaraq, funksiyaların obyekt kimi istifadə olunması və yüksək səviyyəli massiv metodlarının mövcudluğu JavaScript-də funksional proqramlaşdırma texnikalarını tətbiq etməyə imkan yaradır.

Funksional proqramlaşdırma, əsasən, funksiyaların birinci dərəcəli obyekt olması, yəni funksiyaların dəyişənlər kimi ötürülməsi, başqa funksiyalara arqument kimi verilməsi və ya nəticə kimi qaytarılması prinsipinə əsaslanır. Bu yanaşma proqram kodunun daha təmiz, oxunaqlı və səhvsiz olmasına kömək edir.

---

### 8.8.1 Massivlərin Funksiyalarla İşlənməsi (Processing Arrays with Functions)

Təsəvvür edin ki, ədədlər massivimiz var və biz bu dəyərlərin **cümi** və **kvadratlarının ortalamasını** hesablamaq istəyirik. Ənənəvi (imperativ) üsulla bunu belə edə bilərik:

```javascript
let data = [2, 4, 6, 8, 10];

// Cəmi hesablamaq
let total = 0;
for (let i = 0; i < data.length; i++) {
  total += data[i];
}
console.log(total); // => 30

// Kvadratların cəmini hesablamaq
let totalSquares = 0;
for (let i = 0; i < data.length; i++) {
  totalSquares += data[i] * data[i];
}
let meanSquares = totalSquares / data.length;
console.log(meanSquares); // => 44
```

İndi eyni hesablamaları **`map()`** və **`reduce()`** massiv metodları ilə funksional üslubda yerinə yetirək:

```javascript
const sum = (x, y) => x + y;
const square = x => x * x;

let data = [2, 4, 6, 8, 10];

// Cəmi hesablamaq
let total = data.reduce(sum);
console.log(total); // => 30

// Kvadratların ortalaması
let meanSquares = data.map(square).reduce(sum) / data.length;
console.log(meanSquares); // => 44
```

Daha ümumi funksional üsulla, `map` və `reduce` funksiyalarını ayrıca yazıb istifadə edə bilərik:

```javascript
const map = (a, fn) => a.map(fn);
const reduce = (a, fn) => a.reduce(fn);

const sum = (x, y) => x + y;
const square = x => x * x;

let data = [2, 4, 6, 8, 10];

let total = reduce(data, sum);
let meanSquares = reduce(map(data, square), sum) / data.length;

console.log(total);       // => 30
console.log(meanSquares); // => 44
```

Bu nümunələr funksional proqramlaşdırmanın əsas prinsiplərini — funksiyaların tətbiqi və transformasiyasını — aydın göstərir. Funksional metodlardan istifadə kodu daha qısa, oxunaqlı və təkrar istifadəyə yararlı edir.

---

### 8.8.2 Yüksək Səviyyəli Funksiyalar (Higher-Order Functions)

**Yüksək səviyyəli funksiya** — funksiyaları arqument kimi qəbul edən və/və ya funksiya qaytaran funksiyadır. Yəni, belə funksiyalar digər funksiyalar üzərində işləyir.

---

**Misal: `not()` funksiyası**

`not` funksiyası arqument olaraq funksiya alır və həmin funksiyanın nəticəsini tərsinə çevirən yeni funksiya qaytarır.

```javascript
function not(f) {
  return function(...args) {
    let result = f.apply(this, args);
    return !result;
  };
}

const even = x => x % 2 === 0;  // Ədədin cüt olub-olmadığını yoxlayır
const odd = not(even);          // 'even'in əksini verən funksiya

console.log([1, 1, 3, 5, 5].every(odd)); // => true; bütün ədədlər təkdir
```

---

**Misal: `mapper()` funksiyası**

`mapper` funksiyası bir funksiya arqumenti kimi qəbul edir və onu massivə tətbiq edən yeni funksiya qaytarır.

```javascript
function mapper(f) {
  return a => map(a, f); // yuxarıdakı 'map' funksiyasından istifadə
}

const increment = x => x + 1;         // Hər elementi 1 artırır
const incrementAll = mapper(increment);

console.log(incrementAll([1, 2, 3])); // => [2, 3, 4]
```

---

**Misal: `compose()` funksiyası**

İki funksiyanı qəbul edir və onları ardıcıl şəkildə tətbiq edən yeni funksiya qaytarır: `f(g(...))`.

```javascript
function compose(f, g) {
  return function(...args) {
    return f.call(this, g.apply(this, args));
  };
}

const sum = (x, y) => x + y;
const square = x => x * x;

console.log(compose(square, sum)(2, 3)); // => 25; (2+3)^2 = 25
```

---

### 8.8.4 Memoizasiya (Memoization)

Memoizasiya — bir funksiyanın əvvəlcə hesablanmış nəticələrini yadda saxlamaq və eyni arqumentlərlə növbəti çağırışlarda həmin yaddaşdan sürətlə nəticəni qaytarmaq üsuludur. Bu, xüsusilə **təkrarlanan və mürəkkəb hesablamalar** zamanı proqramın sürətini artırmaq üçün faydalıdır.

---

#### Sadə izah:

Təsəvvür edin ki, bir funksiya çox mürəkkəb riyazi əməliyyat aparır və eyni arqumentlərlə çoxsaylı çağırılır. Hər dəfə hesablamanı yenidən etmək yerinə, əvvəlki nəticəni yadda saxlayıb, növbəti çağırışda sadəcə həmin nəticəni vermək daha sürətlidir.

---

#### Sadə nümunə: memoizasiya funksiyası

```javascript
function memoize(f) {
  const cache = new Map(); // Keş saxlamaq üçün obyekt

  return function(...args) {
    let key = args.join(","); // Arqumentləri stringə çeviririk

    if (cache.has(key)) {
      // Əgər nəticə yaddaşda varsa, onu qaytar
      return cache.get(key);
    }

    // Nəticə yoxdursa, funksiyanı çağır və nəticəni yaddaşa yaz
    let result = f.apply(this, args);
    cache.set(key, result);

    return result;
  };
}
```

---

#### Misal: ən böyük ortaq bölən (GCD)

```javascript
function gcd(a, b) {
  while (b !== 0) {
    [a, b] = [b, a % b];
  }
  return a;
}

const gcdMemo = memoize(gcd);

console.log(gcdMemo(85, 187)); // => 17 (hesablanır)
console.log(gcdMemo(85, 187)); // => 17 (keşdən qaytarılır, sürətli)
```

Burada `gcdMemo` funksiyası `gcd` funksiyasının yaddaşlı versiyasıdır. İkinci dəfə eyni arqumentlə çağırıldıqda, nəticə artıq hesablanmadığından daha sürətli alınır.

---

#### Misal: rekursiv faktoriyal funksiyası

Rekursiv funksiyaların performansını artırmaq üçün memoizasiya çox faydalıdır.

```javascript
const factorial = memoize(function f(n) {
  return n <= 1 ? 1 : n * f(n - 1);
});

console.log(factorial(5)); // => 120
```

Burada `factorial` özü özünü çağırır, amma hər dəyər bir dəfə hesablanıb yaddaşa yazılır. Növbəti çağırışlarda isə yaddaşdan alınır.

---