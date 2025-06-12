# Fəsil 8. Funksiyalar (Functions) ⚙️

**Funksiyalar (Functions)** JavaScript proqramlarının əsas hissəsidir. Onlar bir dəfə təyin olunub dəfələrlə çağırıla (invoked) bilən kod bloklarıdır. Funksiyalar **parametrlər (parameters)** qəbul edir və çağırış zamanı **arqumentlər (arguments)** ötürülür. Funksiya icrası **qaytarma dəyəri (return value)** hesablaya bilər. Hər çağırışın bir də **çağırış konteksti (invocation context)**, yəni `this` dəyəri olur.

Əgər funksiya obyektin xüsusiyyətidirsə, o, **metod (method)** adlanır. Yeni obyektləri ilkinləşdirən funksiyalar isə **konstruktorlardır (constructors)**.

JavaScript-də funksiyalar **obyektlərdir (objects)**. Onları dəyişənlərə təyin edə, digər funksiyalara ötürə, hətta özlərinin də xüsusiyyətləri və metodları ola bilər. Funksiyalar başqa funksiyaların içində **iç-içə (nested)** ola bilər və təyin olunduqları skopdakı dəyişənlərə daxil ola bilirlər. Bu, JavaScript funksiyalarının **kloujurlar (closures)** olmasını təmin edir.

---

## 8.1 Funksiyaları Təyin Etmək (Defining Functions) 📝

JavaScript-də funksiyaları təyin etməyin ən yaygın yolu `function` açar sözüdür. ES6 isə `function` açar sözü olmadan **"ox funksiyalarını (arrow functions)"** təqdim edib ki, bu, daha yığcam sintaksisə malikdir. Metodları təyin etmək üçün obyekt literalları və siniflərdə qısa sintaksislər (§6.10.5, §6.10.6) də var. Həmçinin `Function()` konstruktoru (§8.7.7), `function*` (generator funksiyaları) və `async function` (asinxron funksiyalar) da funksiya təyin etmək üçün istifadə olunur.

---

### 8.1.1 Funksiya Bəyanatları (Function Declarations)

**Funksiya bəyanatı (Function declaration)** `function` açar sözü ilə başlayır, ardınca funksiyanın adı, mötərizədə parametrlər və fiqurlu mötərizədə kod (funksiya gövdəsi) gəlir.

**Quruluşu:**
* **Ad (Identifier):** Funksiyanın adıdır, dəyişən kimi işləyir.
* **Parametrlər (Parameters):** Mötərizədə vergüllə ayrılmış dəyişən adlarıdır, funksiya daxilində yerli dəyişənlər kimidir.
* **Gövdə (Body):** Fiqurlu mötərizədəki koddur, funksiya çağırılanda icra olunur.

**Misallar:**

```javascript
function printprops(o) { // Bir obyektin xüsusiyyətlərini çap edir
  for (let p in o) {
    console.log(`${p}: ${o[p]}`);
  }
}

function distance(x1, y1, x2, y2) { // İki nöqtə arasındakı məsafəni hesablayır
  let dx = x2 - x1;
  let dy = y2 - y1;
  return Math.sqrt(dx * dx + dy * dy);
}

function factorial(x) { // Faktoriyalları hesablayan rekursiv funksiya
  if (x <= 1) return 1;
  return x * factorial(x - 1);
}
```

**Vacib Məqam:** Funksiya bəyanatları **"hoist olunur" (hoisted)**. Bu o deməkdir ki, onlar kod işə düşməzdən əvvəl təyin olunur. Yəni, funksiyanı təyin olunduğu yerdən əvvəl də çağıra bilərsiniz.

Funksiyalar dəyər qaytarmalıdırsa, `return` ifadəsindən istifadə edirlər. Əgər `return` yoxdursa və ya dəyər göstərilməyibsə, funksiya `undefined` qaytarır. ES6-dan əvvəl funksiya bəyanatları yalnız ən üst səviyyədə və ya başqa funksiya daxilində olmalı idi. ES6-nın **strict mode**-unda isə bloklar daxilində də istifadə oluna bilər, lakin yalnız o blokda görünür.

---

### 8.1.2 Funksiya İfadələri (Function Expressions)

**Funksiya ifadələri (Function expressions)** funksiya bəyanatlarına oxşayır, lakin daha böyük bir ifadənin hissəsi olurlar və adları **ixtiyaridir (optional)**.

**Misallar:**

```javascript
const square = function(x) { return x*x; }; // Dəyişənə təyin edilmiş funksiya ifadəsi

const f = function fact(x) { // Rekursiya üçün adı olan funksiya ifadəsi
  if (x <= 1) return 1;
  else return x * fact(x-1);
};

// Başqa funksiyalara arqument kimi ötürülən funksiya ifadəsi
[3,2,1].sort(function(a,b) { return a-b; });

// Dərhal çağırılan funksiya ifadəsi (IIFE)
let tensquared = (function(x) {return x*x;}(10));
```

**Vacib Məqam:** Funksiya ifadələri **hoist olunmur**. Onlar yalnız təyin olunduqları ifadə icra olunduqdan sonra mövcud olurlar. Odur ki, funksiya ifadələrini təyin olunmazdan əvvəl çağırmaq mümkün deyil. Funksiyaya dəfələrlə istinad etmək üçün onu bir dəyişənə (adətən `const` ilə) təyin etmək lazımdır. Rekursiv funksiyalar üçün ad qoymaq olar; bu ad funksiyanın daxili skopunda yerli dəyişən kimi işləyir.

---

### 8.1.3 Ox Funksiyaları (Arrow Functions) ➡️

ES6-da **ox funksiyaları (arrow functions)** `function` açar sözü olmadan, `=>` (ox) operatoru ilə təyin olunur. Çox yığcam sintaksisə malikdirlər və ad tələb olunmur.

**Əsas Sintaksis:**
* Parametrlər mötərizədə, sonra `=>`, sonra fiqurlu mötərizədə funksiya gövdəsi:
    ```javascript
    const sum = (x, y) => { return x + y; };
    ```
* Əgər gövdə **tək bir `return` ifadəsidirsə**, `return`, nöqtəli vergül və fiqurlu mötərizələri buraxa bilərsiniz:
    ```javascript
    const sum = (x, y) => x + y; // Daha yığcam!
    ```
* Əgər funksiyanın **yalnız bir parametri varsa**, parametrin ətrafındakı mötərizələri də buraxa bilərsiniz:
    ```javascript
    const polynomial = x => x*x + 2*x + 3;
    ```
* **Heç bir arqument yoxdursa**, boş mötərizələr `()` mütləqdir:
    ```javascript
    const constantFunc = () => 42;
    ```

**Qeydlər:**
* Parametrlər və `=>` arasında yeni sətir qoymayın (sintaktik səhvə səbəb ola bilər).
* Əgər gövdə tək `return` ifadəsidirsə və qaytarılan dəyər bir **obyekt literalidirsə (object literal)**, sintaktik qarışıqlığı önləmək üçün obyekt literalını mötərizəyə almalısınız:
    ```javascript
    const f = x => { return { value: x }; }; // Yaxşı
    const g = x => ({ value: x });          // Yaxşı: obyekti mötərizəyə alıb
    const h = x => { value: x };            // Pis: 'value' bir etiketdir, funksiya undefined qaytarır
    ```

Ox funksiyaları `map()`, `filter()` kimi **massiv metodlarına (array methods)** funksiya ötürərkən ideal seçimdir, çünki qısa yazılışa malikdirlər:

```javascript
let filtered = [1, null, 2, 3].filter(x => x !== null); // null elementlər silinir
let squares = [1, 2, 3, 4].map(x => x*x);               // Hər elementi kvadrata yüksəldir
```

**Mühüm Fərqlər:**
* **`this` dəyəri:** Ox funksiyaları `this` dəyərini **təyin olunduqları mühitdən (environment in which they are defined)** miras alır, öz çağırış kontekstlərini təyin etmirlər. Bu, çox vacib və faydalı bir xüsusiyyətdir.
* **Konstruktor deyil:** Ox funksiyalarının `prototype` xüsusiyyəti yoxdur, buna görə də yeni siniflər üçün **konstruktor funksiyaları (constructor functions)** kimi istifadə oluna bilməzlər.

---

### 8.1.4 İç-içə Funksiyalar (Nested Functions) 🌳

JavaScript-də funksiyalar başqa funksiyaların daxilində **iç-içə (nested)** ola bilər.

**Misal:**

```javascript
function hypotenuse(a, b) {
  function square(x) { // Daxili (nested) funksiya
    return x*x;
  }
  return Math.sqrt(square(a) + square(b));
}
console.log(hypotenuse(3, 4)); // => 5
```

İç-içə funksiyaların maraqlı cəhəti onların **dəyişən skop qaydalarıdır (variable scoping rules)**: onlar içində yerləşdikləri funksiyanın (və ya funksiyaların) parametrlərinə və dəyişənlərinə daxil ola bilərlər. Yuxarıdakı misalda, daxili `square()` funksiyası xarici `hypotenuse()` funksiyası tərəfindən təyin olunan `a` və `b` parametrlərini oxuya və yaza bilir. Bu skop qaydaları çox vacibdir və §8.6-da yenidən nəzərdən keçiriləcək.

---

# 8.2 Funksiyaları Çağırmaq (Invoking Functions) 🚀

Funksiya kodları təyin olunanda deyil, **çağırılanda (invoked)** icra olunur. JavaScript funksiyaları beş əsas yolla çağırıla bilər:
1.  **Funksiyalar kimi (As functions)**
2.  **Metodlar kimi (As methods)**
3.  **Konstruktorlar kimi (As constructors)**
4.  **`call()` və `apply()` metodları vasitəsilə dolayı yolla (Indirectly)**
5.  **Gizli olaraq (Implicitly)** (normal çağırış kimi görünməyən JavaScript xüsusiyyətləri ilə)

---

### 8.2.1 Funksiya Çağırışı (Function Invocation)

Funksiyalar adi funksiyalar və ya metodlar kimi çağırış ifadəsi (§4.5) ilə çağırılır. Çağırış ifadəsi funksiya obyektinə çevrilən bir ifadə, ardınca mötərizədə arqumentlər siyahısından ibarətdir.

**Misallar:**
```javascript
printprops({x: 1}); // Adi funksiya çağırışı
let total = distance(0,0,2,1) + distance(2,1,3,5);
let probability = factorial(5)/factorial(13);
```
Çağırış zamanı, arqumentlər (mötərizə daxilindəkilər) qiymətləndirilir və funksiyanın parametrlərinə təyin olunur. Funksiya geriyə `undefined` qaytara bilər (əgər `return` yoxdursa və ya dəyər göstərilməyibsə), ya da `return` ifadəsindəki dəyəri qaytarır.

**Şərti Çağırış (Conditional Invocation):**
ES2020-də `?.` operatoru ilə funksiyanı yalnız `null` və ya `undefined` olmadığı halda çağırmaq olar:
```javascript
f?.(x) // f null/undefined deyilsə, f(x) çağırılır, əks halda undefined
```

**`this` dəyəri (Invocation Context):**
* **Non-strict mode:** `this` dəyəri **qlobal obyekt (global object)** olur.
* **Strict mode:** `this` dəyəri `undefined` olur.
* **Ox funksiyaları (Arrow functions):** `this` dəyərini **təyin olunduqları mühitdən (environment in which they are defined)** miras alırlar.

Adətən, adi funksiya kimi çağırılan funksiyalar `this` açar sözünü istifadə etmir.

**Rekursiv Funksiyalar (Recursive Functions) və Stack:**
**Rekursiv funksiya (recursive function)** özünü çağıran funksiyadır (məsələn, `factorial()`). Rekursiya zamanı hər funksiya çağırışı **çağırış stack-ına (call stack)** yeni bir icra konteksti (execution context) əlavə edir. Çox sayda rekursiv çağırış **"Maximum call-stack size exceeded"** xətasına səbəb ola bilər, çünki stack yaddaşı dolar.

---

### 8.2.2 Metod Çağırışı (Method Invocation)

**Metod (Method)** obyektin bir xüsusiyyətində saxlanılan JavaScript funksiyasıdır. Bir funksiyanı obyektin metodu kimi təyin etmək üçün: `o.m = f;`

**Çağırılış qaydası:**
```javascript
o.m();     // Metod çağırışı
o.m(x, y); // Arqumentlərlə metod çağırışı
```
`o.m` bir xüsusiyyətə daxilolma ifadəsidir, bu da funksiyanın metod kimi çağırıldığını bildirir.

**`this` dəyəri (Invocation Context):**
Metod çağırışında **`this` dəyəri metodun çağırıldığı obyektin özü olur**. Bu, metodun həmin obyekt üzərində əməliyyat apardığını göstərir.

**Misal:**
```javascript
let calculator = { // Bir obyekt literal
  operand1: 1,
  operand2: 1,
  add() { // Metod qısa sintaksisi
    this.result = this.operand1 + this.operand2; // 'this' 'calculator' obyektinə işarə edir
  }
};

calculator.add();     // Metod çağırışı
console.log(calculator.result); // => 2
```
Kvadrat mötərizə (`[]`) ilə xüsusiyyətə daxil olmaq da metod çağırışına səbəb ola bilər: `o["m"](x,y);`

**Metod Zənciri (Method Chaining):**
Metodlar obyekt qaytardıqda, bir metodun qaytarma dəyərini növbəti metodun hissəsi kimi istifadə edərək **zəncirvari çağırışlar (method chaining)** yaratmaq olar. Metod öz dəyərini qaytarmadıqda `this` qaytarmaq bu zəncirləməni asanlaşdırır.

```javascript
// Zəncirvari metod çağırışı nümunəsi
new Square().x(100).y(100).size(50).outline("red").fill("blue").draw();
```
**Vacib:** `this` bir açar sözdür, dəyişən deyil. Ona dəyər təyin etmək olmaz.

**Daxili Funksiyalarda `this` Problemi:**
`this` dəyişənlər kimi skoplara aid deyil. Ox funksiyaları istisna olmaqla, iç-içə (nested) funksiyalar `this` dəyərini xarici funksiyadan miras almır. Bu, JavaScript-in "qüsuru" sayılır.

**Problem:**
```javascript
let o = {
  m: function() {
    // Bu 'this' 'o' obyektidir
    function f() { // İç-içə funksiya
      // Bu 'this' qlobal obyekt və ya undefined olacaq, 'o' deyil!
    }
    f();
  }
};
o.m();
```
**Həll Yolları:**
1.  **`self` dəyişəni:** Xarici `this` dəyərini bir dəyişəndə (`self` kimi) saxlamaq:
    ```javascript
    let o = {
      m: function() {
        let self = this; // 'this' dəyərini saxlayırıq
        function f() {
          console.log(self === o); // => true
        }
        f();
      }
    };
    o.m();
    ```
2.  **Ox funksiyaları (ES6+):** Daxili funksiyanı ox funksiyası kimi təyin etmək:
    ```javascript
    let o = {
      m: function() {
        const f = () => { // Ox funksiyası 'this'i miras alır
          console.log(this === o); // => true
        };
        f();
      }
    };
    o.m();
    ```
3.  **`bind()` metodu:** `bind()` metodu vasitəsilə daxili funksiyanın `this` dəyərini təyin etmək:
    ```javascript
    const f = (function() {
      console.log(this === o);
    }).bind(this); // 'this'i xarici 'this'ə bağlayırıq
    ```

---

### 8.2.3 Konstruktor Çağırışı (Constructor Invocation) 🏗️

Əgər funksiya çağırışının qarşısında **`new`** açar sözü varsa, bu, **konstruktor çağırışıdır (constructor invocation)**. Konstruktorlar yeni obyektlər yaratmaq və ilkinləşdirmək üçündür (§4.6, §6.2.2, Fəsil 9).

 ```javascript
    o = new Object();
    o = new Object;
```

**Fərqləri:**
* **Arqumentlər (Arguments):** Adi çağırışlar kimi ötürülür. Boş mötərizələri (`()`) buraxmaq olar: `new Object()` ilə `new Object` eynidir.
* **`this` dəyəri (Invocation Context):** Konstruktor çağırışı **yeni, boş bir obyekt yaradır** və **həmin obyekt `this` dəyəri olur**. Konstruktor funksiyası bu obyektə `this` vasitəsilə müraciət edir. Hətta `new o.m()` kimi görünsə də, `o` obyektinin özü deyil, yeni yaradılan obyekt `this` olur.
* **Qaytarma dəyəri (Return Value):** Konstruktorlar adətən `return` açar sözünü istifadə etmir, sadəcə yeni obyekti ilkinləşdirib implicit olaraq qaytarırlar. Bu halda, yeni yaradılan obyekt çağırışın dəyəri olur.
    * Əgər konstruktor **obyekt qaytararsa**, həmin obyekt çağırışın dəyəri olur.
    * Əgər `return` dəyərsizdirsə və ya **primitiv dəyər** qaytararsa, bu dəyər **göz ardı edilir** və yenə də yeni yaradılan obyekt çağırışın dəyəri olur.

---

### 8.2.4 Dolayı Çağırış (Indirect Invocation) ↩️

JavaScript funksiyaları obyekt olduğu üçün metodları var. **`call()`** və **`apply()`** metodları funksiyanı **dolayı yolla (indirectly)** çağırmaq üçün istifadə olunur.

* Hər iki metod sizə **`this` dəyərini açıq şəkildə (explicitly specify)** təyin etməyə imkan verir. Bu o deməkdir ki, funksiyanı istənilən obyektin metodu kimi çağıra bilərsiniz, hətta o obyektin əsl metodu olmasa belə.
* Hər iki metod həmçinin arqumentləri də təyin etməyə imkan verir.
    * **`call()`:** Arqumentləri öz arqument siyahısı kimi qəbul edir.
    * **`apply()`:** Arqumentləri massiv şəklində gözləyir.

Bu metodlar §8.7.4-də ətraflı təsvir olunub.

---

### 8.2.5 Gizli Funksiya Çağırışı (Implicit Function Invocation) 👻

JavaScript-də funksiya çağırışı kimi görünməyən, lakin funksiyaların çağırılmasına səbəb olan müxtəlif dil xüsusiyyətləri var. Bu cür funksiyaları yazarkən diqqətli olun, çünki onların nə zaman çağırıldığını koddan başa düşmək çətin olduğundan, səhvləri tapmaq və düzəltmək daha çətin ola bilər.

**Gizli çağırışa səbəb ola biləcək dil xüsusiyyətləri:**

* **Getter və Setterlər (§6.10.6):** Obyektin xüsusiyyətlərinin dəyərini sorğulamaq və ya təyin etmək, müvafiq getter/setter metodlarını çağıra bilər.
* **Obyektin String/Rəqəm Kontekstində İstifadəsi (§3.9.3):**
    * String kontekstində (`+` ilə birləşdirmək kimi): `toString()` metodu çağırılır.
    * Rəqəm kontekstində: `valueOf()` metodu çağırılır.
* **İterativ Obyektlər Üzərində Dövr Etmək (Chapter 12):** İterativ obyektlərin elementləri üzərində dövr edərkən bir sıra metod çağırışları baş verir.
* **Tağlı Şablon Literalları (Tagged Template Literals) (§14.5):** Bir funksiya çağırışıdır, lakin gizli şəkildədir.
* **Proksi Obyektlər (Proxy Objects) (§14.7):** Bu obyektlər üzərində demək olar ki, hər əməliyyat bir funksiyanın çağırılmasına səbəb olur, çünki onların davranışı funksiyalarla tamamilə idarə olunur.

---


# 8.3 Funksiya Arqumentləri və Parametrləri (Function Arguments and Parameters) 📝

JavaScript funksiya təyinləri parametrlər üçün gözlənilən tip təyin etmir və funksiya çağırışları arqument dəyərləri üzərində **tip yoxlaması (type checking)** aparmır. Hətta ötürülən arqumentlərin sayını da yoxlamır. Bəzi hallarda arqumentlərin tipini yoxlamaq lazım gələ bilər.

---

### 8.3.1 İxtiyari Parametrlər və Defolt Dəyərlər (Optional Parameters and Defaults) ❓

Funksiya elan edilmiş parametrlərdən **daha az arqumentlə (fewer arguments)** çağırılanda, əlavə parametrlər **`undefined`** defolt dəyərinə təyin olunur. Bəzi arqumentləri ixtiyari etmək faydalıdır.

**Misal (köhnə üsul):**

```javascript
function getPropertyNames(o, a) {
  if (a === undefined) a = []; // Əgər 'a' undefined-dirsə, yeni massiv istifadə et
  for(let property in o) a.push(property);
  return a;
}

let o = {x: 1}, p = {y: 2, z: 3};
let a = getPropertyNames(o); // a == ["x"]
getPropertyNames(p, a);      // a == ["x", "y", "z"]
```
Yuxarıdakı `if` ifadəsi yerinə, **`||` operatoru** (truthy/falsy dəyərlər üçün §4.10.2) ilə daha qısa yaza bilərsiniz: `a = a || [];`.

**Qeyd:** İxtiyari arqumentləri **arqument siyahısının sonuna** qoymaq vacibdir ki, onlar buraxıla bilsin. Əks halda, funksiyanı çağıran şəxs birinci arqumenti buraxıb ikincini ötürmək istəsə, birinci arqument üçün açıq şəkildə `undefined` ötürməlidir.

**ES6+ Defolt Parametr Dəyərləri:**
ES6 və sonrakı versiyalarda, hər parametr üçün **defolt dəyəri (default value)** birbaşa parametr siyahısında təyin edə bilərsiniz:

```javascript
function getPropertyNames(o, a = []) { // 'a' parametrinin defolt dəyəri boş massivdir
  for(let property in o) o.push(property); // Düzəliş: a.push(property); olmalıydı
  return a;
}

// Hər çağırışda yeni boş massiv yaradılır
let names1 = getPropertyNames({x: 1}); // names1 == ["x"]
let names2 = getPropertyNames({y: 2, z: 3}); // names2 == ["y", "z"]
```
Parametr defolt ifadələri funksiya çağırılanda qiymətləndirilir. Defolt dəyərlər sabit (constant) və ya digər parametrlərin dəyərlərindən istifadə edə bilər:

```javascript
const rectangle = (width, height = width * 2) => ({width, height});
rectangle(1); // => { width: 1, height: 2 }
```
Bu sintaksis ox funksiyaları, metodlar və digər funksiya təyin formaları ilə işləyir.

---

### 8.3.2 Qalan Parametrlər və Dəyişən Uzunluqlu Arqument Siyahıları (Rest Parameters and Variable-Length Argument Lists) ➕

**Qalan parametrlər (Rest parameters)** funksiyanın elan edilmiş parametrlərdən **daha çox arqumentlə (arbitrarily more arguments)** çağırılmasına imkan verir.

**Misal:**

```javascript
function max(first = -Infinity, ...rest) { // '...rest' qalan bütün arqumentləri massiv kimi yığır
  let maxValue = first;
  for(let n of rest) {
    if (n > maxValue) {
      maxValue = n;
    }
  }
  return maxValue;
}

max(1, 10, 100, 2, 3, 1000, 4, 5, 6); // => 1000
```
**Qaydalar:**
* Qalan parametrin qarşısında **üç nöqtə (`...`)** olmalıdır.
* Funksiya təyinatında **sonuncu parametr** olmalıdır.
* Qalan arqumentlər **massivdə (array)** saxlanılır. Bu massiv boş ola bilər, lakin `undefined` ola bilməz. (Qalan parametrə defolt dəyər təyin etmək qanuni deyil.)

Bu cür funksiyalara **variadic functions**, **variable arity functions** və ya **vararg functions** deyilir.

**Diqqət:** Funksiya təyinatında istifadə olunan `...` (rest parameter) ilə funksiya çağırışında istifadə olunan `...` (spread operator, §8.3.4) fərqli anlayışlardır.

---

### 8.3.3 `arguments` Objekti (The Arguments Object) 📦

ES6-dan əvvəl **dəyişən uzunluqlu arqumentləri (varargs)** idarə etmək üçün **`arguments` obyekti (Arguments object)** istifadə olunurdu. Hər hansı bir funksiyanın gövdəsi daxilində, `arguments` identifikatoru həmin çağırış üçün `Arguments` obyektinə istinad edirdi. `Arguments` obyekti massivə bənzər bir obyektdir (§7.9) və arqumentləri adla deyil, nömrə ilə almağa imkan verirdi.

**Misal (`arguments` ilə):**

```javascript
function max(x) { // x parametr olaraq verilsə də, arguments obyektindən istifadə olunur
  let maxValue = -Infinity;
  for(let i = 0; i < arguments.length; i++) { // arguments.length ilə arqumentlərin sayına baxılır
    if (arguments[i] > maxValue) maxValue = arguments[i]; // arguments[i] ilə arqumentlərə daxil olunur
  }
  return maxValue;
}

max(1, 10, 100, 2, 3, 1000, 4, 5, 6); // => 1000
```
`Arguments` obyekti JavaScript-in ilk günlərindən qalma bəzi qəribə xüsusiyyətlərə malikdir ki, bu da onu səmərəsiz və optimallaşdırmaqda çətinləşdirir (xüsusilə strict mode-dan kənarda). Köhnə kodlarda rast gəlsəniz də, **yeni kodlarda istifadə etməkdən çəkinməlisiniz**. Köhnə kodu yenidən yazarkən, `arguments` istifadə edən funksiyaları `...args` qalan parametri ilə əvəz etmək tövsiyə olunur.

`Arguments` obyektinin xoşagəlməz irsindən biri də, strict mode-da `arguments`-ın **ehtiyatda saxlanılmış söz (reserved word)** kimi qəbul edilməsidir, yəni həmin adla funksiya parametri və ya yerli dəyişən elan edə bilməzsiniz.

---

### 8.3.4 Funksiya Çağırışları üçün Yayılma Operatoru (Spread Operator) (`...`) 📦

**Yayılma operatoru (`...`)** ES6-da təqdim edilib. Onun funksiyası massivin (və ya string kimi hər hansı başqa **iterable obyektin (iterable object)**) elementlərini fərdi dəyərlər gözlənilən yerdə "açmaq" və ya "yaymaq"dır. Massiv literallarında istifadəsinə (§7.1.2) artıq baxmışdıq. Eyni operator funksiya çağırışlarında da eyni şəkildə istifadə olunur:

```javascript
let numbers = [5, 2, 10, -1, 9, 100, 1];

// numbers massivinin elementlərini Math.min funksiyasına ayrı-ayrı arqumentlər kimi ötürür
console.log(Math.min(...numbers)); // => -1
```

**Vacib Qeyd:** `...` həqiqi bir operator deyil (dəyər qaytarmır). O, massiv literalları və funksiya çağırışlarında istifadə olunan xüsusi bir JavaScript sintaksisidir.

**Qalan Parametrlərlə Birlikdə İstifadəsi (Rest Parameters and Spread Operator Together):**
Funksiya təyinatında (`...`) istifadə olunan `rest parameter` (§8.3.2) arqumentləri massivə yığır (gather), `spread operatoru` isə çağırışda massiv elementlərini yayır (spread out). Bu iki funksiya birlikdə çox faydalıdır. Aşağıdakı nümunə bir funksiyanı götürür və onun işləmə müddətini ölçən "instrumentləşdirilmiş" (instrumented) bir versiyasını qaytarır:

```javascript
// Bu funksiya bir funksiya qəbul edir və onun bükülmüş (wrapped) versiyasını qaytarır
function timed(f) {
  return function(...args) { // Daxili funksiya: bütün arqumentləri 'args' adlı rest parameter-ə yığır
    console.log(`Entering function ${f.name}`);
    let startTime = Date.now();
    try {
      // Bütün arqumentləri bükülmüş funksiyaya ötürürük
      return f(...args); // 'args' massivinin elementlərini fərdi arqumentlər kimi yayır
    } finally {
      // Qaytarılan dəyəri qaytarmadan əvvəl, keçən vaxtı çap edir
      console.log(`Exiting ${f.name} after ${Date.now() - startTime}ms`);
    }
  };
}

// 1-dən n-ə qədər ədədlərin cəmini hesablayan funksiya
function benchmark(n) {
  let sum = 0;
  for (let i = 1; i <= n; i++) sum += i;
  return sum;
}

// İndi 'benchmark' funksiyasının 'timed' versiyasını çağırırıq
// timed(benchmark) bizə yeni bir funksiya qaytarır.
// Həmin funksiyanı (1000000) arqumenti ilə çağırırıq.
timed(benchmark)(1000000); // => 500000500000; Bu ədədlərin cəmidir.
// Konsolda da giriş/çıxış mesajlarını və icra müddətini görəcəksiniz.
```
Bu nümunədə `timed` funksiyasının qaytardığı daxili funksiya `...args` ilə bütün arqumentləri yığır. Sonra bu arqumentləri ilkin `f` funksiyasına `f(...args)` ilə `spread` edir. Bu, funksiyalara istənilən sayda arqument ötürməyə imkan verir.

---

### 8.3.5 Funksiya Arqumentlərini Parametrlərə Destrukturlamaq (Destructuring Function Arguments into Parameters) 📦➡️

Funksiya çağırışında ötürülən arqument dəyərlərini funksiya təyinindəki parametrlərə təyin etmək, dəyişən təyin etməyə bənzəyir. Buna görə də **destrukturlama təyin etmə (destructuring assignment)** texnikalarını (§3.10.3) funksiyalarla da istifadə edə bilərik.

**Massiv Destrukturlaması (Array Destructuring):**
Parametr adlarını kvadrat mötərizə (`[]`) daxilində təyin etməklə, funksiyanın həmin mötərizələr üçün massiv dəyəri gözlədiyini bildirirsiniz. Çağırış prosesi zamanı massiv arqumentləri fərdi adlandırılmış parametrlərə açılır.

**Misal (massivləri destrukturlayaraq vektor toplama):**
Əvvəlki versiya:
```javascript
function vectorAdd(v1, v2) {
  return [v1[0] + v2[0], v1[1] + v2[1]];
}
vectorAdd([1,2], [3,4]); // => [4,6]
```
Destrukturlanmış versiya (daha oxunaqlı):
```javascript
function vectorAdd([x1, y1], [x2, y2]) { // 2 arqumenti 4 parametrə açır
  return [x1 + x2, y1 + y2];
}
vectorAdd([1,2], [3,4]); // => [4,6]
```

**Obyekt Destrukturlaması (Object Destructuring):**
Əgər funksiya obyekt arqumenti gözləyirsə, həmin obyektin parametrlərini destrukturlaya bilərsiniz.

**Misal (obyektləri destrukturlayaraq vektor vurma):**
```javascript
// {x,y} vektorunu bir skalyar dəyərə vurur
function vectorMultiply({x, y}, scalar) { // Obyektin x və y xüsusiyyətlərini birbaşa parametrlərə açır
  return { x: x * scalar, y: y * scalar };
}
vectorMultiply({x: 1, y: 2}, 2); // => {x: 2, y: 4}
```
**Fərqli Adlarla Destrukturlama:**
Əgər obyekt xüsusiyyət adları ilə parametr adları fərqlidirsə, sintaksis belə olur:
```javascript
function vectorAdd(
  {x: x1, y: y1}, // 1-ci obyekti x1 və y1 parametrlərinə açır
  {x: x2, y: y2}  // 2-ci obyekti x2 və y2 parametrlərinə açır
) {
  return { x: x1 + x2, y: y1 + y2 };
}
vectorAdd({x: 1, y: 2}, {x: 3, y: 4}); // => {x: 4, y: 6}
```
**Qayda:** Destrukturlama zamanı kolonun sol tərəfində **obyektin xüsusiyyət adları (property names)**, sağ tərəfində isə **parametr (və ya dəyişən) adları** olur. Bu, obyekt literalında dəyərlərin yeri ilə eynidir.

**Defolt Dəyərlərlə Destrukturlama:**
Destrukturlanmış parametrlərlə də defolt dəyərlər təyin etmək olar.

```javascript
// {x,y} və ya {x,y,z} vektorunu bir skalyara vurur
function vectorMultiply({x, y, z = 0}, scalar) { // Z üçün defolt dəyər 0
  return { x: x * scalar, y: y * scalar, z: z * scalar };
}
vectorMultiply({x: 1, y: 2}, 2); // => {x: 2, y: 4, z: 0}
```

**Obyekt Arqumenti ilə Çoxlu İxtiyari Parametrləri Simulyasiya Etmək:**
JavaScript Python kimi `name=value` arqumentlərinə birbaşa imkan verməsə də, bir obyekt arqumentini destrukturlayaraq bunu təxminən edə bilərsiniz. Bu, çoxlu ixtiyari parametrlər olan funksiyalar üçün faydalıdır.

```javascript
function arraycopy({from, to = from, n = from.length, fromIndex = 0, toIndex = 0}) {
  let valuesToCopy = from.slice(fromIndex, fromIndex + n);
  to.splice(toIndex, 0, ...valuesToCopy);
  return to;
}

let a = [1,2,3,4,5], b = [9,8,7,6,5];
// Arqumentləri adla təyin edərək rahatlıqla ötürürük
arraycopy({from: a, n: 3, to: b, toIndex: 4}); // => [9,8,7,6,1,2,3,5]
```

**Massiv Destrukturlamasında Qalan Parametrlər (Rest Parameters in Array Destructuring):**
Massivi destrukturlayarkən, əlavə dəyərlər üçün daxili **qalan parametr (rest parameter)** təyin edə bilərsiniz. Bu, funksiyanın özünün qalan parametrindən fərqlidir.

```javascript
function f([x, y, ...coords], ...rest) { // [x,y,...coords] massiv destrukturlamasıdır, ...rest funksiya rest parametri
  return [x + y, ...rest, ...coords]; // Note: spread operator here
}
f([1, 2, 3, 4], 5, 6); // => [3, 5, 6, 3, 4]
// x=1, y=2, coords=[3,4], rest=[5,6] olacaq
```

**Obyekt Destrukturlamasında Qalan Parametrlər (Rest Parameters in Object Destructuring) (ES2018+):**
ES2018-dən etibarən obyekti destrukturlayarkən də qalan parametr istifadə edə bilərsiniz. Bu qalan parametr, destrukturlanmayan bütün digər xüsusiyyətləri ehtiva edən bir obyekt olacaq. Bu, obyekt spread operatoru ilə tez-tez istifadə olunur.

```javascript
// {x,y} və ya {x,y,z} vektorunu skalyara vurur, digər xüsusiyyətləri saxlayır
function vectorMultiply({x, y, z = 0, ...props}, scalar) { // '...props' destrukturlanmayanları yığır
  return { x: x * scalar, y: y * scalar, z: z * scalar, ...props }; // '...props' onları geri yayır
}
vectorMultiply({x: 1, y: 2, w: -1}, 2); // => {x: 2, y: 4, z: 0, w: -1}
```

**Çox Səviyyəli Destrukturlama:**
Obyektlərin və massivlərin içində də dərin səviyyəli destrukturlama aparmaq mümkündür. Məsələn, rəng xüsusiyyəti RGB massivi olan bir dairə obyektini destrukturlamaq:

```javascript
// Dairə obyektini 6 ayrı parametrə destrukturlayır
function drawCircle({x, y, radius, color: [r, g, b]}) {
  // x, y, radius, r, g, b parametrlərinə birbaşa daxil olmaq olar
}
```
Lakin, destrukturlama çox mürəkkəbləşdikdə, kodun oxunaqlılığı azalır. Bəzən obyekt xüsusiyyətlərinə və massiv indekslərinə açıq şəkildə daxil olmaq daha aydın ola bilər.

---

### 8.3.6 Arqument Tipləri (Argument Types) 🛠️

JavaScript-də funksiya parametrlərinin **təyin olunmuş tipləri yoxdur (no declared types)** və funksiyalara ötürdüyünüz dəyərlər üzərində **tip yoxlaması (type checking)** aparılmır. Kodunuzu daha aydın etmək üçün arqumentlərə mənalı adlar vermək və şərhlərdə onları diqqətlə sənədləşdirmək (documenting) vacibdir. (Alternativ olaraq, JavaScript üzərinə tip yoxlaması əlavə etmək üçün §17.8-ə baxa bilərsiniz.)

JavaScript lazım gəldikdə **liberal tip çevirmələri (liberal type conversion)** edir (§3.9). Məsələn, string gözləyən bir funksiyaya başqa tipdə dəyər ötürsəniz, funksiya onu string kimi istifadə etməyə çalışanda dəyər sadəcə stringə çevriləcək. Bütün primitiv tiplər stringə çevrilə bilir və bütün obyektlərin `toString()` metodları var, buna görə bu halda heç vaxt səhv baş vermir.

Lakin bu, həmişə belə deyil. Məsələn, əvvəlki `arraycopy()` metodu bir və ya iki massiv arqumenti gözləyir. Əgər bu arqumentlər səhv tipdə olarsa, funksiya işləməyəcək. Funksiyanın yalnız daxili istifadə üçün olmadığı hallarda, bu cür arqumentlərin tiplərini yoxlamaq faydalı ola bilər. Funksiyanın səhv dəyərlərlə çağırılanda dərhal və proqnozlaşdırıla bilən şəkildə uğursuz olması, sonradan qeyri-müəyyən bir xəta mesajı ilə işləməyə başlayıb sonra sıradan çıxmasından daha yaxşıdır.

**Tip Yoxlaması Edən Funksiya Misalı:**

```javascript
// iterable obyekt 'a'-nın elementlərinin cəmini qaytarır.
// 'a'-nın bütün elementləri rəqəm olmalıdır.
function sum(a) {
  let total = 0;
  for (let element of a) { // 'a' iterable deyilsə, TypeError atır
    if (typeof element !== "number") {
      throw new TypeError("sum(): elements must be numbers"); // Element rəqəm deyilsə, xəta atır
    }
    total += element;
  }
  return total;
}

console.log(sum([1, 2, 3])); // => 6

// Yanlış istifadə nümunələri:
// sum(1, 2, 3);          // !TypeError: 1 is not iterable (a bir massiv deyil)
// sum([1, 2, "3"]);      // !TypeError: element 2 is not a number (string olan element)
```
Bu nümunə `sum` funksiyasının daxilində arqumentin iterable olub-olmadığını (for...of dövrü ilə) və hər bir elementin tipi rəqəm olub-olmadığını yoxlayır. Əgər tip uyğun gəlmirsə, `TypeError` atır.

---

# 8.4 Funksiyalar Dəyərlər Kimi (Functions as Values) 💎

Funksiyaların əsas xüsusiyyətləri təyin olunmaları və çağırılmalarıdır. Lakin JavaScript-də funksiyalar təkcə sintaksis deyil, həm də **dəyərlərdir (values)**. Bu o deməkdir ki, funksiyaları:
* Dəyişənlərə təyin etmək olar.
* Obyektlərin xüsusiyyətlərində və ya massivlərin elementlərində saxlamaq olar.
* Digər funksiyalara arqument kimi ötürmək olar.
* Və s.

**Misal:**
```javascript
function square(x) { return x*x; } // Funksiya təyini

let s = square; // Funksiyanı başqa dəyişənə təyin etmək
console.log(square(4)); // => 16
console.log(s(4));      // => 16 (eyni funksiyanı çağırır)

let o = {square: function(x) { return x*x; }}; // Obyektin xüsusiyyəti kimi (metod)
let y = o.square(16); // y == 256

let a = [x => x*x, 20]; // Massiv elementi kimi (anonim ox funksiyası)
console.log(a[0](a[1])); // => 400 (a[0] funksiyasını a[1] dəyəri ilə çağırır)
```
Funksiyaları dəyər kimi qəbul etməyin faydasına misal olaraq `Array.sort()` metodunu göstərmək olar. Bu metod massivləri çeşidləyir, lakin çeşidləmə qaydasını təyin etmək üçün **arqument kimi bir funksiya (comparison function)** qəbul edir. Bu, `sort()` metodunu çox çevik edir, istənilən tipi istənilən qaydada çeşidləyə bilər. (Misallar §7.8.6-da verilib.)

**Misal 8-1. Funksiyaları Dəyər Kimi İstifadə Etmək:**
```javascript
// Sadə riyazi funksiyalar
function add(x,y) { return x + y; }
function subtract(x,y) { return x - y; }
function multiply(x,y) { return x * y; }
function divide(x,y) { return x / y; }

// Bu funksiya yuxarıdakı funksiyalardan birini arqument kimi qəbul edir və iki operand üzərində icra edir
function operate(operator, operand1, operand2) {
  return operator(operand1, operand2); // Daxil olan 'operator' funksiyasını çağırır
}

// (2+3) + (4*5) ifadəsini hesablamaq
let i = operate(add, operate(add, 2, 3), operate(multiply, 4, 5));
console.log(i); // => 25

// Funksiyaları bir obyekt literalında metod kimi təyin etmək
const operators = {
  add: (x,y) => x+y,
  subtract: (x,y) => x-y,
  multiply: (x,y) => x*y,
  divide: (x,y) => x*y, // Düzəliş: x/y olmalıydı
  pow: Math.pow // Önceden təyin olunmuş funksiyalar üçün də işləyir
};

// Bu funksiya operatorun adını qəbul edir, obyektdə həmin operatoru tapır və operandlar üzərində icra edir.
function operate2(operation, operand1, operand2) {
  if (typeof operators[operation] === "function") {
    return operators[operation](operand1, operand2); // Obyektdən alınmış funksiyanı çağırır
  } else {
    throw "unknown operator";
  }
}

console.log(operate2("add", "hello ", "world")); // => "hello world"
console.log(operate2("pow", 10, 2));             // => 100
```

---

### 8.4.1 Öz Funksiya Xüsusiyyətlərinizi Təyin Etmək (Defining Your Own Function Properties) ⚙️

JavaScript-də funksiyalar xüsusi növ obyektlər olduğundan, onların da **xüsusiyyətləri (properties)** ola bilər. Bir funksiyanın çağırışlar arasında dəyəri qorunan **"statik" (static)** bir dəyişənə ehtiyacı olduqda, funksiyanın öz xüsusiyyətindən istifadə etmək rahatdır.

**Misal (Unikal Tam Ədəd):**
Hər çağırılanda fərqli, unikal bir tam ədəd qaytaran funksiya:

```javascript
// Funksiya obyektinin 'counter' xüsusiyyətini ilkinləşdirir.
// Funksiya bəyanatları 'hoist' olunduğundan, bunu funksiya təyinindən əvvəl edə bilərik.
uniqueInteger.counter = 0;

// Hər dəfə çağırılanda fərqli bir tam ədəd qaytarır.
// Növbəti qaytarılacaq dəyəri öz xüsusiyyətində saxlayır.
function uniqueInteger() {
  return uniqueInteger.counter++; // 'counter' xüsusiyyətini qaytarır və artırır
}

console.log(uniqueInteger()); // => 0
console.log(uniqueInteger()); // => 1
```

**Misal (Faktoriyalın Keşlənməsi):**
Hesablanmış nəticələri öz xüsusiyyətlərində (massiv kimi davranaraq) keşləyən `factorial()` funksiyası:

```javascript
function factorial(n) {
  if (Number.isInteger(n) && n > 0) { // Yalnız müsbət tam ədədlər üçün
    if (!(n in factorial)) { // Əgər keşlənmiş nəticə yoxdursa
      factorial[n] = n * factorial(n - 1); // Hesablayır və keşləyir
    }
    return factorial[n]; // Keşlənmiş nəticəni qaytarır
  } else {
    return NaN; // Yanlış giriş üçün
  }
}

factorial[1] = 1; // Keşi baza halı ilə ilkinləşdirir.
console.log(factorial(6)); // => 720
console.log(factorial[5]); // => 120; yuxarıdakı çağırış bu dəyəri keşləyir
```

---

# 8.5 Funksiyalar Ad Məkanları Kimi (Functions as Namespaces) 🌐

Funksiyaların içində elan edilən dəyişənlər funksiyanın xaricindən görünmür. Bu səbəbdən, bəzən sadəcə müvəqqəti bir **ad məkanı (namespace)** kimi fəaliyyət göstərmək üçün funksiya təyin etmək faydalıdır. Bu yolla, qlobal ad məkanını (global namespace) artıq dəyişənlərlə doldurmadan öz dəyişənlərinizi təyin edə bilərsiniz.

Təsəvvür edin ki, bir neçə proqramda istifadə etmək istədiyiniz bir JavaScript kodu parçasınız var. Bu kod parçası, əksər kodlar kimi, hesablamalarının aralıq nəticələrini saxlamaq üçün dəyişənlər təyin edir. Problem ondadır ki, bu kod parçasını bir çox proqramda istifadə edəcəyiniz üçün, yaratdığı dəyişənlərin onu istifadə edən proqramların yaratdığı dəyişənlərlə toqquşmayacağına zəmanət verə bilməzsiniz.

**Həlli:** Kodu bir funksiyanın içinə qoyub sonra həmin funksiyanı çağırmaqdır. Bu şəkildə, qlobal olacaq dəyişənlər funksiyaya **yerli (local)** olur:

```javascript
function chunkNamespace() {
  // Kod parçası burada yerləşir
  // Bu kodda təyin olunan istənilən dəyişən qlobal ad məkanını zibilləmək əvəzinə,
  // bu funksiyaya yerli olacaq.
}
chunkNamespace(); // Funksiyanı çağırmağı unutmayın!
```
Bu üsul yalnız tək bir qlobal dəyişən (funksiya adı `chunkNamespace`) təyin edir. Əgər hətta tək bir dəyişən də çoxdursa, **anonim bir funksiyanı (anonymous function)** təyin edib dərhal çağıra bilərsiniz:

```javascript
(function() { // chunkNamespace() funksiyası adlandırılmamış bir ifadə kimi yenidən yazılıb.
  // Kod parçası burada yerləşir
}()); // Funksiya literalını bağlayır və dərhal çağırır.
```
Bu texnika (bir funksiyanı təyin edib dərhal çağırmaq) o qədər tez-tez istifadə olunur ki, idiomatik hala gəlib və **"dərhal çağırılan funksiya ifadəsi" (immediately invoked function expression - IIFE)** adını alıb.

**Diqqət:** Yuxarıdakı kodda `function` açar sözündən əvvəl mötərizə (`(`) vacibdir. Mötərizəsiz, JavaScript interpreteri `function` sözünü bir funksiya bəyanatı (function declaration statement) kimi şərh etməyə çalışacaq. Mötərizə ilə isə interpreter bunu düzgün olaraq bir funksiya təyin ifadəsi (function definition expression) kimi tanıyır. Öndəki mötərizə, həmçinin, insanlara funksiyanın sonradan istifadə üçün deyil, dərhal çağırılmaq üçün təyin olunduğunu anlamağa kömək edir.

Funksiyaların ad məkanları kimi bu cür istifadəsi, ad məkanı funksiyasının içində bir və ya daha çox funksiya təyin edib, həmin ad məkanındakı dəyişənləri istifadə edərək, sonra onları ad məkanı funksiyasının qaytarma dəyəri kimi geri qaytardığımızda daha da faydalı olur. Bu cür funksiyalar **kloujurlar (closures)** kimi tanınır və növbəti bölmənin mövzusudur.

---

# 8.6 Kloujurlar (Closures) 🔐

JavaScript **leksik skoplaşmadan (lexical scoping)** istifadə edir. Bu o deməkdir ki, funksiyalar təyin olunduqları zaman mövcud olan dəyişən skopu ilə icra olunurlar, çağırılanda mövcud olan skopla deyil. Bunu təmin etmək üçün, JavaScript funksiya obyektinin daxili vəziyyəti funksiyanın kodunu, həmçinin funksiyanın təyin olunduğu skopa (dəyişən bağlamaları toplusu) bir istinad ehtiva etməlidir. Funksiya obyekti ilə onun dəyişənlərinin həll olunduğu skopun bu kombinasiyası kompüter elmində **kloujur (closure)** adlanır.

Texniki cəhətdən, bütün JavaScript funksiyaları kloujurlardır. Lakin funksiyalar təyin olunduqları skopdan çağırıldıqda, kloujurun rolu fərqli görünmür. Kloujurlar, **təyin olunduqları skopdan fərqli bir skopdan çağırılanda** maraqlı olurlar. Bu, ən çox iç-içə funksiya obyektinin təyin olunduğu funksiyadan geri qaytarılması zamanı baş verir.

**Kloujurları Anlamaq üçün İlk Addım: İç-içə Funksiyaların Leksik Skop Qaydaları:**

```javascript
let scope = "global scope"; // Qlobal dəyişən
function checkscope() {
  let scope = "local scope"; // Yerli dəyişən
  function f() { return scope; } // Buradakı 'scope' dəyərini qaytarır
  return f();
}
console.log(checkscope()); // => "local scope" (Çünki f() yerli 'scope'u görür)
```

İndi kodu bir az dəyişək:

```javascript
let scope = "global scope";
function checkscope() {
  let scope = "local scope";
  function f() { return scope; } // 'scope' dəyərini qaytarır
  return f; // f() funksiyasının özünü qaytarır
}

let s = checkscope(); // s indi f funksiyasına istinad edir
console.log(s());      // => "local scope"; Niyə?
```
Bu kodda `checkscope()` funksiyası iç-içə funksiyanı çağırmır, sadəcə onu qaytarır. Biz həmin qaytarılan `f` funksiyasını (indiki `s`) təyin olunduğu funksiyadan kənarda çağırırıq.

**Açar Məqam:** Leksik skoplaşmanın əsas qaydası: JavaScript funksiyaları **təyin olunduqları skopla (scope they were defined in)** icra olunur. `f()` funksiyası `scope` dəyişəninin "local scope" dəyərinə bağlandığı bir skopda təyin edilmişdi. `f` haradan icra olunmasından asılı olmayaraq, bu bağlama hələ də qüvvədədir. Odur ki, `s()` "local scope" qaytarır, "global scope" deyil. Budur, qısaca desək, kloujurların təəccüblü və güclü təbiəti: onlar təyin olunduqları xarici funksiyanın yerli dəyişən (və parametr) bağlamalarını **yaxalayır (capture)**.

**Kloujurlardan Şəxsi Vəziyyət Kimi İstifadə Etmək:**
§8.4.1-də `uniqueInteger()` funksiyasını funksiyanın özünün xüsusiyyətindən istifadə edərək yazmışdıq. Həmin yanaşmanın çatışmazlığı o idi ki, səhv və ya zərərli kod sayğacı sıfırlaya və ya qeyri-tam ədədə təyin edə bilərdi. Kloujurlar tək bir funksiya çağırışının yerli dəyişənlərini yaxalayır və bu dəyişənləri **şəxsi vəziyyət (private state)** kimi istifadə edə bilər.

**Misal (Unikal Tam Ədəd – Kloujurla):**
```javascript
let uniqueInteger = (function() { // Anonim funksiyanı təyin edib dərhal çağırır
  let counter = 0; // Bu dəyişən daxili funksiya üçün şəxsi vəziyyətdir (private state)
  return function() { return counter++; }; // Bu daxili funksiya 'counter'i yaxalayır
}());

console.log(uniqueInteger()); // => 0
console.log(uniqueInteger()); // => 1
// 'counter' dəyişəninə kənardan daxil olmaq mümkün deyil
```
Burada, `uniqueInteger` dəyişəninə təyin olunan şey, xarici anonim funksiyanın qaytardığı daxili funksiyadır. Bu daxili funksiya xarici funksiyanın `counter` dəyişəninə daxil olur. Xarici funksiya geri qayıtdıqdan sonra, başqa heç bir kod `counter` dəyişənini görə bilməz: daxili funksiya ona eksklüziv girişə malikdir.

**Paylaşılan Şəxsi Dəyişənlər:**
Şəxsi dəyişənlər yalnız bir kloujura aid olmaq məcburiyyətində deyil: eyni xarici funksiyanın daxilində iki və ya daha çox iç-içə funksiya təyin edib eyni skopu paylaşmaq tamamilə mümkündür.

**Misal (Sayğac Obyekti):**
```javascript
function counter() {
  let n = 0; // Bu dəyişən 'count' və 'reset' metodları arasında paylaşılır
  return {
    count: function() { return n++; },
    reset: function() { n = 0; }
  };
}

let c = counter(); // Birinci sayğac obyekti
let d = counter(); // İkinci, müstəqil sayğac obyekti

console.log(c.count()); // => 0
console.log(d.count()); // => 0: Onlar müstəqil sayırlar
c.reset();              // c-nin vəziyyətini sıfırlayır
console.log(c.count()); // => 0: çünki c-ni sıfırladıq
console.log(d.count()); // => 1: d sıfırlanmadı
```
`counter()` funksiyasının hər çağırışı yeni bir skop və o skop daxilində yeni bir şəxsi dəyişən `n` yaradır. Beləliklə, `counter()`-i iki dəfə çağırsanız, iki müstəqil `counter` obyekti əldə edirsiniz.

**Kloujurlar və Getter/Setter Metodları ilə Birgə İstifadə:**
Bu kloujur texnikasını xüsusiyyət getterləri və setterləri ilə birləşdirmək olar.

```javascript
function counter(n) { // Funksiya arqumenti 'n' şəxsi dəyişəndir
  return {
    get count() { return n++; }, // Getter: şəxsi sayğacı qaytarır və artırır
    set count(m) { // Setter: 'n' dəyərinin azalmasına icazə vermir
      if (m > n) n = m;
      else throw Error("count can only be set to a larger value");
    }
  };
}

let c = counter(1000);
console.log(c.count);    // => 1000
console.log(c.count);    // => 1001
c.count = 2000;          // Setter çağırılır
console.log(c.count);    // => 2000
// c.count = 2000;       // !Error: count can only be set to a larger value
```
Burada `n` parametri funksiyanın şəxsi vəziyyətini saxlayır.

**Misal 8-2. Kloujurlardan İstifadə Edərək Şəxsi Xüsusiyyət Aksessor Metodları:**
Bu nümunə bir obyektdə şəxsi dəyişənlər üçün getter/setter metodları yaradır. Dəyər obyektin özündə deyil, `addPrivateProperty` funksiyasının yerli dəyişənində saxlanılır.

```javascript
function addPrivateProperty(o, name, predicate) {
  let value; // Bu, xüsusi dəyərdir, obyektin özündə saxlanılmır
  o[`get${name}`] = function() { return value; }; // Getter
  o[`set${name}`] = function(v) { // Setter
    if (predicate && !predicate(v)) {
      throw new TypeError(`set${name}: invalid value ${v}`);
    } else {
      value = v;
    }
  };
}

let o = {};
addPrivateProperty(o, "Name", x => typeof x === "string"); // Yalnız stringlərə icazə verilir
o.setName("Frank");
console.log(o.getName()); // => "Frank"
// o.setName(0);            // !TypeError: wrong type
```

**Kloujurların Səhvən Paylaşması:**
Dövr daxilində çoxlu kloujurlar yaratarkən diqqətli olmaq lazımdır ki, onlar səhvən eyni dəyişəni paylaşmasınlar.

```javascript
// Bu funksiya hər zaman 'v' dəyərini qaytaran bir funksiya qaytarır
function constfunc(v) { return () => v; }

let funcs = [];
for(var i = 0; i < 10; i++) funcs[i] = constfunc(i); // Hər iterasiyada yeni 'i' dəyəri ilə kloujur yaradılır

console.log(funcs[5]()); // => 5
```
Problem yaranan hal:
```javascript
// 0-9 dəyərlərini qaytaran funksiyalar massivi qaytarır
function constfuncs() {
  let funcs = [];
  for(var i = 0; i < 10; i++) {
    funcs[i] = () => i; // Hər kloujur eyni 'i' dəyişəninə istinad edir
  }
  return funcs;
}

let funcs = constfuncs();
console.log(funcs[5]()); // => 10; Niyə 5 deyil?
```
Bu kod 10 kloujur yaradır. Onlar eyni `constfuncs()` çağırışının daxilində təyin olunduğu üçün `i` dəyişənini paylaşırlar. `constfuncs()` qayıtdıqda, `i` dəyişəninin dəyəri 10 olur və bütün 10 kloujur bu dəyəri paylaşır. Nəticədə, massivdəki bütün funksiyalar eyni `10` dəyərini qaytarır.

**Həlli (ES6+): `let` və ya `const` istifadəsi:**
`var` əvəzinə `let` və ya `const` istifadə etdikdə problem aradan qalxır, çünki `let` və `const` **blok skopludur (block scoped)**. Dövrün hər bir iterasiyası müstəqil bir skop yaradır və hər skopun öz `i` bağlaması olur.

```javascript
function constfuncsFixed() {
  let funcs = [];
  for(let i = 0; i < 10; i++) { // 'var' əvəzinə 'let' istifadə olunur
    funcs[i] = () => i; // Hər kloujur öz 'i' dəyərini yaxalayır
  }
  return funcs;
}

let funcsFixed = constfuncsFixed();
console.log(funcsFixed[5]()); // => 5; Doğru nəticə!
```

**Kloujurlarda `this`:**
`this` bir dəyişən deyil, açar sözdür. Ox funksiyaları `this` dəyərini onları əhatə edən funksiyadan miras alır, lakin `function` açar sözü ilə təyin olunmuş funksiyalar miras almır. Buna görə də:
* Əgər kloujurunuz xarici funksiyanın `this` dəyərini istifadə etməlidisə, **ox funksiyasından** istifadə edin.
* Və ya `bind()` metodunu çağırın.
* Və ya xarici `this` dəyərini bir dəyişənə təyin edin: `const self = this;`

---

# 8.7 Funksiya Xüsusiyyətləri, Metodları və Konstruktoru (Function Properties, Methods, and Constructor) 🛠️

JavaScript-də funksiyalar dəyərlərdir. `typeof` operatoru bir funksiyaya tətbiq edildikdə "function" stringini qaytarır, lakin funksiyalar əslində **ixtisaslaşmış bir JavaScript obyektidir**. Funksiyalar obyekt olduğu üçün, digər obyektlər kimi **xüsusiyyətlərə (properties)** və **metodlara (methods)** sahib ola bilərlər. Hətta yeni funksiya obyektləri yaratmaq üçün **`Function()` konstruktoru** da mövcuddur.

---

### 8.7.1 `length` Xüsusiyyəti (The `length` Property) 📏

Funksiyanın **`length`** xüsusiyyəti **oxunulmaq üçün nəzərdə tutulmuşdur (read-only)**. Bu, funksiyanın **arity-sini (arity)**, yəni parametr siyahısında elan etdiyi parametrlərin sayını göstərir (adətən funksiyanın gözlədiyi arqumentlərin sayı). Əgər funksiyanın **`rest parameter`**-i varsa (`...args` kimi), o, `length` xüsusiyyəti üçün sayılmır.

---

### 8.7.2 `name` Xüsusiyyəti (The `name` Property) 🏷️

Funksiyanın **`name`** xüsusiyyəti də **oxunulmaq üçün nəzərdə tutulmuşdur**. Bu, funksiyanın təyin olunarkən istifadə olunan adını (əgər adı varsa) və ya adsız bir funksiya ifadəsinin təyin olunduğu dəyişənin/xüsusiyyətin adını göstərir. Bu xüsusiyyət əsasən debug etmək və ya xəta mesajları yazarkən faydalıdır.

---

### 8.7.3 `prototype` Xüsusiyyəti (The `prototype` Property) 🧬

Ox funksiyaları istisna olmaqla, bütün funksiyaların **`prototype`** adlı bir xüsusiyyəti var. Bu xüsusiyyət **prototip obyektinə (prototype object)** istinad edir. Hər funksiyanın fərqli bir prototip obyekti var. Bir funksiya **konstruktor kimi (as a constructor)** istifadə edildikdə, yeni yaradılan obyektlər prototip obyektindən xüsusiyyətləri miras alır. Prototip və `prototype` xüsusiyyəti §6.2.3-də müzakirə edilib və Fəsil 9-da yenidən nəzərdən keçiriləcək.

---

### 8.7.4 `call()` və `apply()` Metodları (The `call()` and `apply()` Methods) 📞

**`call()`** və **`apply()`** metodları bir funksiyanı dolayısı ilə (§8.2.4) sanki başqa bir obyektin metoduymuş kimi çağırmağa imkan verir. Hər iki metodun ilk arqumenti, funksiyanın üzərində çağırılacağı obyektdir; bu arqument **çağırış konteksti (invocation context)** olur və funksiya gövdəsi daxilində **`this`** açar sözünün dəyəri olur.

Funksiya `f()`-ni `o` obyektinin metodu kimi çağırmaq üçün (arqumentsiz):
```javascript
f.call(o);
f.apply(o);
```
Bu sətirlər funksiyanı `o` obyektinin müvəqqəti metodu edib çağırmağa bənzəyir:
```javascript
o.m = f;
o.m();
delete o.m;
```
**Qeyd:** Ox funksiyaları təyin olunduqları kontekstdən `this` dəyərini miras alırlar və bu dəyər `call()` və `apply()` metodları ilə dəyişdirilə bilməz. Əgər bu metodları bir ox funksiyasında çağırsanız, ilk arqument əhəmiyyətsiz qalacaq.

`call()` metodunun ilk arqumentindən sonra gələn bütün arqumentlər çağırılan funksiyaya ötürülən dəyərlərdir:
```javascript
f.call(o, 1, 2); // f() funksiyasına 1 və 2 arqumentlərini ötürür
```
`apply()` metodu `call()`-a bənzəyir, lakin funksiyaya ötürüləcək arqumentlər **massiv (array)** kimi göstərilir:
```javascript
f.apply(o, [1, 2]); // f() funksiyasına [1, 2] massivinin elementlərini ötürür
```
Əgər funksiya ixtiyari sayda arqument qəbul etmək üçün təyin edilibsə, `apply()` metodu həmin funksiyanı ixtiyari uzunluqlu bir massivin tərkibi üzərində çağırmağa imkan verir. ES6 və sonrakı versiyalarda **spread operatoru** istifadə edə bilərik, lakin köhnə kodlarda `apply()` istifadəsinə rast gələ bilərsiniz. Məsələn, massivdəki ən böyük rəqəmi tapmaq üçün:

```javascript
let arrayOfNumbers = [5, 2, 10, -1, 9];
let biggest = Math.max.apply(Math, arrayOfNumbers); // Math.max-i massiv elementləri üzərində çağırır
console.log(biggest); // => 10
```
Aşağıdakı `trace()` funksiyası, `apply()` metodundan istifadə edərək, metodları işləmə müddətini izləyən bir nümunədir:

```javascript
function trace(o, m) {
  let original = o[m]; // Orijinal metodu kloujurda yadda saxlayır
  o[m] = function(...args) { // Yeni metodu təyin edir
    console.log(new Date(), "Entering:", m);
    let result = original.apply(this, args); // Orijinal metodu eyni 'this' və arqumentlərlə çağırır
    console.log(new Date(), "Exiting:", m);
    return result;
  };
}
```

---

### 8.7.5 `bind()` Metodu (The `bind()` Method) 🔗

**`bind()`** metodunun əsas məqsədi bir funksiyanı bir obyektə **bağlamaqdır (bind)**. `bind()` metodunu `f` funksiyası üzərində çağırıb `o` obyektini ötürdüyünüzdə, metod yeni bir funksiya qaytarır. Bu yeni funksiyanı çağırdığınızda, orijinal `f` funksiyası `o` obyektinin metodu kimi çağırılır.

```javascript
function f(y) { return this.x + y; } // Bu funksiya bağlanmağa ehtiyac duyur
let o = { x: 1 };
let g = f.bind(o); // 'g' indi f() funksiyasını 'o' obyektinə bağlayır
console.log(g(2)); // => 3 (this.x = 1, y = 2)

let p = { x: 10, g };
console.log(p.g(2)); // => 3: 'g' hələ də 'o' obyektinə bağlıdır, 'p'-yə yox.
```
Ox funksiyaları `this` dəyərini təyin olunduqları mühitdən miras alır və bu dəyər `bind()` ilə dəyişdirilə bilməz. Praktikada bu problem yaratmır, çünki `bind()`-dən ən çox ox funksiyası olmayan funksiyaları ox funksiyaları kimi davranmağa məcbur etmək üçün istifadə olunur.

**Qismən Tətbiq (Partial Application):**
`bind()` metodu sadəcə funksiyanı obyektə bağlamaqla qalmır. O, həm də **qismən tətbiq (partial application)** edə bilər: ilk `this` dəyərindən sonra `bind()`-ə ötürdüyünüz istənilən arqumentlər də bağlanır. Bu, funksional proqramlaşdırmada ümumi bir texnikadır və bəzən **currying** adlanır.

```javascript
let sum = (x, y) => x + y;
let succ = sum.bind(null, 1); // Birinci arqumenti (x) 1-ə bağlayır (this burada əhəmiyyətsizdir, null-dır)
console.log(succ(2)); // => 3: x 1-ə, y isə 2-yə bağlanır

function f(y, z) { return this.x + y + z; }
let g = f.bind({x: 1}, 2); // 'this'i {x:1}-ə, 'y'i 2-yə bağlayır
console.log(g(3)); // => 6: this.x=1, y=2, z=3
```
`bind()` tərəfindən qaytarılan funksiyanın `name` xüsusiyyəti, `bind()`-in çağırıldığı funksiyanın `name` xüsusiyyətinə "bound " prefiksi əlavə edilmiş halıdır.

---

### 8.7.6 `toString()` Metodu (The `toString()` Method) 📝

Bütün JavaScript obyektləri kimi, funksiyaların da `toString()` metodu var. ECMAScript spesifikasiyası bu metodun funksiya bəyanatının sintaksisinə uyğun bir string qaytarmasını tələb edir. Praktikada, əksər (lakin hamısı deyil) tətbiqlər funksiyanın tam **mənbə kodunu (source code)** qaytarır. Daxili funksiyalar isə adətən gövdə kimi "[native code]" kimi bir string qaytarır.

---

### 8.7.7 `Function()` Konstruktoru (The `Function()` Constructor) 🏭

Funksiyalar obyekt olduğu üçün, yeni funksiyalar yaratmaq üçün istifadə oluna bilən bir **`Function()` konstruktoru** var:

```javascript
const f = new Function("x", "y", "return x*y;"); // Yeni funksiya yaradır
```
Bu kod satırı, bildiyimiz sintaksislə təyin olunmuş funksiyaya az-çox bərabər olan bir funksiya yaradır:
```javascript
const f = function(x, y) { return x*y; };
```
`Function()` konstruktoru ixtiyari sayda string arqumenti gözləyir. **Sonuncu arqument funksiyanın gövdəsinin mətni (function body text) olmalıdır**; o, nöqtəli vergüllə ayrılmış ixtiyari JavaScript ifadələri ehtiva edə bilər. Konstruktorun bütün digər arqumentləri funksiya üçün **parametr adlarını (parameter names)** təyin edən stringlərdir. Əgər arqument qəbul etməyən bir funksiya təyin edirsinizsə, sadəcə bir string – funksiya gövdəsi – konstruktora ötürəcəksiniz.

`Function()` konstruktoru yaratdığı funksiyaya ad təyin edən heç bir arqument qəbul etmir. Funksiya literalları kimi, `Function()` konstruktoru da **anonim funksiyalar (anonymous functions)** yaradır.

**`Function()` Konstruktoru haqqında Vacib Məqamlar:**
* `Function()` konstruktoru JavaScript funksiyalarının **işləmə zamanı (runtime)** dinamik şəkildə yaradılmasına və tərtib edilməsinə imkan verir.
* Konstruktor hər çağırılanda funksiya gövdəsini təhlil edir və yeni bir funksiya obyekti yaradır. Əgər konstruktora çağırış bir dövrün içində və ya tez-tez çağırılan bir funksiyanın içindədirsə, bu proses **səmərəsiz (inefficient)** ola bilər. Əksinə, dövrlərin içində görünən iç-içə funksiyalar və funksiya ifadələri hər qarşılaşdıqda yenidən tərtib edilmir.
* Ən vacib məqam budur ki, `Function()` konstruktorunun yaratdığı funksiyalar **leksik skoplaşmadan istifadə etmir (do not use lexical scoping)**; əksinə, onlar həmişə **top-level funksiyalar (top-level functions)** kimi tərtib olunur, yəni qlobal skopu görürlər:

    ```javascript
    let scope = "global";
    function constructFunction() {
      let scope = "local";
      return new Function("return scope"); // Yerli skopu yaxalamır!
    }

    // Bu sətir "global" qaytarır, çünki Function() konstruktorunun qaytardığı funksiya
    // yerli skopdan istifadə etmir.
    console.log(constructFunction()()); // => "global"
    ```
`Function()` konstruktoru `eval()`-ın (§4.12.2) yeni dəyişənlər və funksiyaları öz şəxsi skopunda təyin edən **qlobal skoplu bir versiyası** kimi düşünülməlidir. Ehtimal ki, kodunuzda bu konstruktordan heç vaxt istifadə etməyə ehtiyacınız olmayacaq.

---

# 8.8 Funksional Proqramlaşdırma 🚀

JavaScript Lisp və ya Haskell kimi bir **funksional proqramlaşdırma dili (functional programming language)** deyil. Amma JavaScript-də funksiyaları obyekt kimi manipulyasiya edə bilmək, bizə funksional proqramlaşdırma texnikalarını tətbiq etməyə imkan verir. Massiv metodları, məsələn, **`map()`** 🗺️ və **`reduce()`** 🔄, xüsusilə funksional proqramlaşdırma stili üçün çox uyğundur. Aşağıdakı bölmələrdə JavaScript-də funksional proqramlaşdırma üçün bəzi texnikalar nümayiş olunur. Bunlar JavaScript funksiyalarının gücünü kəşf etmək üçün bir "beyin açan" səyahət 🧠 kimi nəzərdə tutulub, yaxşı proqramlaşdırma stili üçün bir "resept" deyil.

---

### 8.8.1 Massivlərin Funksiyalarla İşlənməsi (Processing Arrays with Functions) 📊

Təsəvvür edin ki, ədədlər massivimiz var və bu dəyərlərin **orta (mean)** və **standart kənarlaşmasını (standard deviation)** hesablamaq istəyirik. Qeyri-funksional üslubda bunu belə edə bilərik:

```javascript
let data = [1, 1, 3, 5, 5]; // Ədədlər massivimiz

// Orta dəyəri hesablamaq (elementlərin cəmi / elementlərin sayı)
let total = 0;
for (let i = 0; i < data.length; i++) total += data[i];
let mean = total / data.length; // mean == 3 ✅

// Standart kənarlaşmanı hesablamaq
total = 0;
for (let i = 0; i < data.length; i++) {
  let deviation = data[i] - mean;
  total += deviation * deviation;
}
let stddev = Math.sqrt(total / (data.length - 1)); // stddev == 2 ✅
```

Eyni hesablamaları **`map()`** və **`reduce()`** massiv metodlarından istifadə edərək qısa funksional üslubda yerinə yetirə bilərik:

```javascript
// Əvvəlcə iki sadə funksiya təyin edirik
const sum = (x, y) => x + y;       // Toplama funksiyası ➕
const square = x => x * x;         // Kvadrat funksiyası ², x * x

let data = [1, 1, 3, 5, 5];

// Bu funksiyaları massiv metodları ilə istifadə edərək orta və standart kənarlaşmanı hesablayırıq
let mean = data.reduce(sum) / data.length; // mean == 3 (massivin cəmi / sayı)
let deviations = data.map(x => x - mean);  // Hər elementin ortadan kənarlaşması
let stddev = Math.sqrt(deviations.map(square).reduce(sum) / (data.length - 1));
console.log(stddev); // => 2 ✅
```

Bu yeni versiya ilkindən xeyli fərqlənir, lakin hələ də obyektlər üzərində metodlar çağırır. Gəlin, `map()` və `reduce()` metodlarının funksional versiyalarını yazaq:

```javascript
// Massivin 'map' metodunu tətbiq edən ümumi 'map' funksiyası
const map = function(a, ...args) { return a.map(...args); };
// Massivin 'reduce' metodunu tətbiq edən ümumi 'reduce' funksiyası
const reduce = function(a, ...args) { return a.reduce(...args); };

const sum = (x, y) => x + y;
const square = x => x * x;
let data = [1, 1, 3, 5, 5];

let mean = reduce(data, sum) / data.length; // 'reduce' funksiyamızdan istifadə
let deviations = map(data, x => x - mean);  // 'map' funksiyamızdan istifadə
let stddev = Math.sqrt(reduce(map(deviations, square), sum) / (data.length - 1));
console.log(stddev); // => 2 ✅
```
Bu kod artıq daha "funksional" görünür, obyekt yönümlü metod çağırışları yerinə ümumi funksiyalardan istifadə edirik!

---

### 8.8.2 Yüksək Səviyyəli Funksiyalar (Higher-Order Functions) 🧠✨

**Yüksək səviyyəli funksiya (Higher-order function)** funksiyalar üzərində işləyən, bir və ya daha çox funksiyanı arqument kimi qəbul edən və yeni bir funksiya qaytaran funksiyadır.

**Misal (not funksiyası):**
```javascript
// Bu yüksək səviyyəli funksiya, arqumentlərini 'f' funksiyasına ötürən və
// 'f'-in qaytarma dəyərinin məntiqi inkarını qaytaran yeni bir funksiya qaytarır.
function not(f) {
  return function(...args) { // Yeni bir funksiya qaytarır... 🆕
    let result = f.apply(this, args); // ...ki 'f'-i çağırır... 📞
    return !result; // ...və nəticəsini inkar edir. 🚫
  };
}

const even = x => x % 2 === 0; // Ədədin cüt olub-olmadığını yoxlayan funksiya
const odd = not(even);         // 'even' funksiyasının əksini edən yeni funksiya
console.log([1, 1, 3, 5, 5].every(odd)); // => true: massivin hər elementi təkdir ✅
```
`not()` funksiyası bir funksiya arqumenti qəbul etdiyi və yeni bir funksiya qaytardığı üçün yüksək səviyyəli funksiyadır.

**Misal (`mapper` funksiyası):**
Bu funksiya bir funksiya arqumenti qəbul edir və həmin funksiyadan istifadə edərək bir massivi başqa massivə map edən yeni bir funksiya qaytarır.

```javascript
// Massiv arqumenti gözləyən və 'f'-i hər elementə tətbiq edərək nəticələr massivini qaytaran funksiya qaytarır.
function mapper(f) {
  return a => map(a, f); // 'map' funksiyamızı (yuxarıdakı) istifadə edir
}

const increment = x => x + 1; // Hər elementi 1 artıran funksiya
const incrementAll = mapper(increment); // 'increment' funksiyasını tətbiq edən yeni funksiya
console.log(incrementAll([1, 2, 3])); // => [2, 3, 4] ✅
```

**Misal (`compose` funksiyası):**
İki funksiya `f` və `g`-ni qəbul edən və `f(g())` hesablayan yeni bir funksiya qaytaran daha ümumi bir nümunə.

```javascript
// f(g(...)) hesablayan yeni funksiya qaytarır.
function compose(f, g) {
  return function(...args) {
    // 'f' üçün 'call' istifadə edirik, çünki tək dəyər ötürürük,
    // 'g' üçün isə 'apply' istifadə edirik, çünki massiv dəyərlər ötürürük.
    return f.call(this, g.apply(this, args));
  };
}

const sum = (x, y) => x + y;
const square = x => x * x;
console.log(compose(square, sum)(2, 3)); // => 25; cəmin kvadratı ( (2+3)^2 = 5^2 = 25 ) ✅
```
Aşağıdakı bölmələrdə `partial()` və `memoize()` funksiyaları da əhəmiyyətli yüksək səviyyəli funksiyalardır.

---

### 8.8.3 Funksiyaların Qismən Tətbiqi (Partial Application of Functions) 🧩

Funksiyanın `bind()` metodu (§8.7.5) funksiyanı müəyyən bir kontekstdə və müəyyən arqumentlərlə çağıran yeni bir funksiya qaytarır. Buna **qismən tətbiq (partial application)** deyilir. `bind()` arqumentləri soldan bağlayır. Lakin arqumentləri sağdan da bağlamaq mümkündür:

```javascript
// Bu funksiyaya arqumentlər soldan ötürülür
function partialLeft(f, ...outerArgs) {
  return function(...innerArgs) { // Yeni funksiya qaytarır
    let args = [...outerArgs, ...innerArgs]; // Arqument siyahısını qurur
    return f.apply(this, args); // Sonra f-i onunla çağırır
  };
}

// Bu funksiyaya arqumentlər sağdan ötürülür
function partialRight(f, ...outerArgs) {
  return function(...innerArgs) { // Yeni funksiya qaytarır
    let args = [...innerArgs, ...outerArgs]; // Arqument siyahısını qurur
    return f.apply(this, args); // Sonra f-i onunla çağırır
  };
}

// Bu funksiyaya arqumentlər şablon kimi xidmət edir.
// Arqument siyahısındakı 'undefined' dəyərlər daxili arqumentlərdən doldurulur.
function partial(f, ...outerArgs) {
  return function(...innerArgs) {
    let args = [...outerArgs];     // Xarici arqument şablonunun yerli kopyası
    let innerIndex = 0;            // Növbəti daxili arqument hansıdır

    // Arqumentlər üzərində dövr edir, 'undefined' dəyərləri daxili arqumentlərdən doldurur
    for (let i = 0; i < args.length; i++) {
      if (args[i] === undefined) args[i] = innerArgs[innerIndex++];
    }
    // Qalan daxili arqumentləri əlavə edir
    args.push(...innerArgs.slice(innerIndex));
    return f.apply(this, args);
  };
}

const f = function(x, y, z) { return x * (y - z); };

console.log(partialLeft(f, 2)(3, 4));     // => -2: Birinci arqumenti bağla: 2 * (3 - 4)
console.log(partialRight(f, 2)(3, 4));    // => 6: Sonuncu arqumenti bağla: 3 * (4 - 2)
console.log(partial(f, undefined, 2)(3, 4)); // => -6: Ortadakı arqumenti bağla: 3 * (2 - 4)
```

Bu qismən tətbiq funksiyaları, artıq təyin etdiyimiz funksiyalardan maraqlı funksiyalar yaratmağa imkan verir:

```javascript
const increment = partialLeft(sum, 1);       // increment(x) == sum(1, x)
const cuberoot = partialRight(Math.pow, 1 / 3); // cuberoot(x) == Math.pow(x, 1/3)

console.log(cuberoot(increment(26))); // => 3 (26+1 = 27, 27^(1/3) = 3)
```

**Kompozisiya (Composition) və Qismən Tətbiq (Partial Application) Birlikdə:**
```javascript
const not = partialLeft(compose, x => !x); // 'compose' ilə 'not' funksiyası
const even = x => x % 2 === 0;
const odd = not(even);
const isNumber = not(isNaN); // isNumber(x) == !isNaN(x)

console.log(odd(3) && isNumber(2)); // => true ✅
```

Mean və standart kənarlaşma hesablamalarını daha da funksional üslubda yenidən yazaq:

```javascript
const product = (x, y) => x * y;
const neg = partial(product, -1); // neg(x) == product(-1, x) == -x
const sqrt = partial(Math.pow, undefined, .5); // sqrt(x) == Math.pow(x, .5)
const reciprocal = partial(Math.pow, undefined, neg(1)); // reciprocal(x) == Math.pow(x, -1) == 1/x

let data = [1, 1, 3, 5, 5];

let mean = product(reduce(data, sum), reciprocal(data.length));
let stddev = sqrt(
  product(
    reduce(
      map(data, compose(square, partial(sum, neg(mean)))),
      sum
    ),
    reciprocal(sum(data.length, neg(1)))
  )
);
console.log([mean, stddev]); // => [3, 2] ✅
```
Göründüyü kimi, bu kod tamamilə funksiya çağırışlarından ibarətdir; heç bir operator yoxdur və mötərizələrin sayı o qədər artıb ki, bu JavaScript Lisp koduna bənzəməyə başlayır! 😅 Bu, JavaScript proqramlaşdırması üçün tövsiyə etdiyim bir stil deyil, lakin JavaScript kodunun nə qədər "dərin" funksional ola biləcəyini görmək üçün maraqlı bir çalışmadır.

---

### 8.8.4 Memoizasiya (Memoization) 🧠💾

§8.4.1-də əvvəlcədən hesablanmış nəticələri keşləyən bir `factorial` funksiyası təyin etmişdik. Funksional proqramlaşdırmada bu cür keşləməyə **memoizasiya (memoization)** deyilir. Aşağıdakı kod, bir funksiyanı arqument kimi qəbul edən və funksiyanın memoizasiya edilmiş versiyasını qaytaran bir yüksək səviyyəli funksiya olan **`memoize()`**-ni göstərir:

```javascript
// 'f' funksiyasının memoizasiya olunmuş versiyasını qaytarır.
// Yalnız 'f'-ə ötürülən arqumentlərin hamısının fərqli string təmsilləri varsa işləyir.
function memoize(f) {
  const cache = new Map(); // Keş dəyəri kloujurda saxlanılır. 💾
  return function(...args) {
    // Keş açarı kimi arqumentlərin string versiyasını yaradır.
    let key = args.length + args.join("+"); // Məsələn: "21+2"
    if (cache.has(key)) {
      return cache.get(key); // Keşdən qaytarır ⚡
    } else {
      let result = f.apply(this, args); // Orijinal funksiyanı çağırır
      cache.set(key, result);           // Nəticəni keşləyir
      return result;
    }
  };
}

// Evklid alqoritmi ilə iki tam ədədin ən böyük ortaq bölənini qaytarır:
function gcd(a, b) { // A və B üçün tip yoxlaması buraxılıb
  if (a < b) { [a, b] = [b, a]; } // A-nın b-dən böyük olmasını təmin edir
  while (b !== 0) {
    [a, b] = [b, a % b]; // Evklid alqoritmi
  }
  return a;
}

const gcdmemo = memoize(gcd); // gcd-nin memoizasiya olunmuş versiyası
console.log(gcdmemo(85, 187)); // => 17 ✅ (ilk çağırışda hesablanır)
console.log(gcdmemo(85, 187)); // => 17 ✅ (keşdən götürülür, daha sürətli)

// Rekursiv funksiyanı memoizasiya edərkən, adətən orijinala deyil,
// memoizasiya olunmuş versiyaya rekursiya etmək istəyirik.
const factorial = memoize(function(n) {
  return (n <= 1) ? 1 : n * factorial(n - 1); // Burada 'factorial' artıq memoizasiya olunmuş versiyadır
});
console.log(factorial(5)); // => 120: 4, 3, 2 üçün də dəyərləri keşləyir ✅
```
`memoize()` funksiyası keş üçün yeni bir obyekt yaradır və bu obyekti daxili funksiyanın **kloujurunda (closure)** şəxsi bir dəyişən kimi saxlayır. Qaytarılan funksiya arqumentlər massivini stringə çevirir və bu stringi keş obyekti üçün xüsusiyyət adı kimi istifadə edir. Əgər keşdə dəyər varsa, onu birbaşa qaytarır. Əks halda, dəyəri hesablamaq üçün göstərilən funksiyanı çağırır, bu dəyəri keşləyir və qaytarır.