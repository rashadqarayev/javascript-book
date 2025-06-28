#  Fəsil 6. Obyektlər

JavaScript-də **obyektlər (objects)** ən vacib anlayışlardan biridir. 💡 Obyektlər JavaScript-in ürəyidir, çünki onlar real dünya məlumatlarını və onların funksionallığını təsvir etmək üçün əsas quruluş daşıyıcısıdır.

##  6.1 Obyektlərə Giriş

###  Obyekt nədir?

Obyekt — bir neçə dəyəri **bir yerdə, açar-dəyər (key-value) cütləri şəklində saxlayan** konteynerdir. Bu dəyərlər **string, number, boolean və ya hətta başqa obyektlər və ya funksiyalar** ola bilər.

Hər bir obyekt içində **xüsusiyyətlər (properties)** saxlayır. Hər bir xüsusiyyət öz növbəsində bir ad (**key**) və ona uyğun bir dəyərdən (**value**) ibarətdir.

Məsələn, bir avtomobili təsvir edən obyekt belə görünə bilər:

```javascript
let car = {
  brand: "Toyota",
  year: 2020,
  color: "qara",
  isElectric: false
};
```

Burada `brand`, `year`, `color` və `isElectric` obyektin xüsusiyyətləridir.


```javascript
let person = {
  firstName: "Əli",
  lastName: "Əliyev",
  age: 30,
  isStudent: false,
  address: { // address xüsusiyyəti özü bir obyektdir
    street: "Neftçilər Prospekti",
    city: "Bakı",
    zipCode: "AZ1000"
  },
  hobbies: ["kitab oxumaq", "idman", "proqramlaşdırma"] // hobbies xüsusiyyəti bir massivdir
};
```

Gördüyünüz kimi, `address` xüsusiyyəti özü bir obyektdir, `hobbies` isə bir massivdir.Obyekt daxilində primitiv dəyərlərdən əlavə, primitiv olmayan dəyərlər də ola bilər.

---


### Xüsusiyyətlər (Properties) necə işləyir?

Təsəvvür edin ki, hər obyekt real həyatdakı bir varlıqdır. Məsələn, bir `kitab` obyekti. Bu kitabı təsvir edən hər bir məlumat onun **xüsusiyyətidir**. Hər xüsusiyyət iki hissədən ibarətdir: **adı (key)** və **dəyəri(value)**.

---

#### Əsas Qaydalar

* Xüsusiyyətin **adı (key)** çox vaxt **string** tipində olur. Bəzən nadir hallarda, xüsusi məqsədlər üçün `Symbol` (Simvol) tipindən də istifadə oluna bilər.
* Xüsusiyyətin **dəyəri (value)** isə istənilən JavaScript tipində (string, number, boolean, null, undefined, funksiya, başqa obyekt və s.) ola bilər.
* Bir obyekt daxilində **eyni adlı iki xüsusiyyət ola bilməz**. Əgər eyni adlı bir xüsusiyyəti təkrar yaratsan, sonuncu dəyər əvvəlki dəyərin üzərinə yazılır (overwrite).

---

####  Praktik Nümunə: Açar Adlarının Yazılışı

Gəlin bir `avtomobil` obyekti yaradaq və açarların fərqli yazılış stillərinə baxaq.

**Qızıl Qayda:** Əgər açarın adı sadədirsə (hərf və rəqəmlərdən ibarətdir, amma rəqəmlə başlamır, boşluq və ya `-` kimi simvolları yoxdur), onu dırnaqsız yaza bilərsiniz. Bu, daha səliqəli görünür. Lakin ad mürəkkəbdirsə, dırnaq işarəsi (`"` və ya `'`) **mütləqdir**.

```javascript
// Gəlin bir avtomobil obyekti yaradaq
let car = {
  //  Sadə adlar - dırnaqsız yazmaq tamamilə normaldır
  brand: "Mercedes",
  model: "S-Class",
  year: 2023,
  isNew: true,

  //  Mürəkkəb adlar - dırnaq işarəsi MÜTLƏQDİR!
  "engine-type": "V8 Biturbo",   // defis (-) olduğu üçün
  "color option": "Obsidian Black" // boşluq olduğu üçün
};

// İndi isə məlumatları əldə edək:

// Sadə adlara nöqtə ilə müraciət edirik
console.log("Avtomobilin markası:", car.brand); // Nəticə: Mercedes

// Mürəkkəb adlara isə kvadrat mötərizə və dırnaqla müraciət edirik
console.log("Mühərrikin növü:", car["engine-type"]); // Nəticə: V8 Biturbo
console.log("Rəng seçimi:", car["color option"]);    // Nəticə: Obsidian Black

// Mürəkkəb ada nöqtə ilə müraciət etmək səhvə səbəb olacaq!
console.log(car.engine-type); // Bu kod error verəcək!
```



####  Dəyərin Üzərinə Yazılması (Overwrite)

Tutaq ki, bir istifadəçinin statusunu qeyd edirik, amma sonra statusu dəyişir.

```javascript
let user = {
  username: "ayan_qarayeva",
  status: "Online", // İlk dəyər
  loginCount: 42,
  
  // Tutaq ki, haradasa səhvən statusu yenidən yazdıq
  status: "Away" // Son dəyər
};

// Gəlin görək, "status" açarının son dəyəri nədir?
console.log(user.status); // Nəticə: "Away"

// JavaScript kodu yuxarıdan aşağı oxuduğu üçün "status" açarına
// təyin edilən ən son dəyəri yadda saxlayır və əvvəlkini unudur.
```

---

###  Obyektlərdə Məlumat Paylaşımı: Dəyər yoxsa İstinad?

JavaScript-də obyektlər **dəyərlə (by value) yox, istinadla (by reference)** ötürülür. Bu, proqramlaşdırmada çox vacib bir anlayışdır və əksər tələbələrin ilk başda çətinlik çəkdiyi mövzulardan biridir. Gəlin fərqə baxaq:

---

**1. Primitiv Tiplər (string, number, boolean, null, undefined, symbol, bigint) — Dəyərlə Ötürülür:**
Primitiv bir dəyəri başqa bir dəyişkənə mənimsətdikdə, orijinal dəyərin bir **kopyası** yeni dəyişənə verilir. Hər hansı birinin dəyişdirilməsi digərinə təsir etmir.

Primitiv tipləri bir vərəq kağıza yazılmış tək bir məlumat kimi düşünün.

1.  Sizin `a` adlı vərəqiniz var və üzərində **"10"** yazılıb.
2.  Dostunuz `b` gəlir və sizdən bu məlumatı istəyir. Siz orijinal vərəqi vermirsiniz. Bunun yerinə, yeni bir boş vərəq götürüb, üzərinə **"10"** yazıb ona verirsiniz. Yəni dəyərin bir **kopyasını** yaradırsınız.
3.  İndi hərənizin əlində üzərində "10" yazılan öz vərəqi var. Onlar bir-birindən tamamilə müstəqildir.
4.  Dostunuz `b` öz vərəqindəki "10"-u pozub yerinə **"20"** yazır. Bu, sizin `a` vərəqinizə heç bir şəkildə təsir etmir.

Sizin kod nümunəniz bu prinsipi mükəmməl şəkildə nümayiş etdirir:

```javascript
let a = 10; // 'a' vərəqində "10" yazılıb.
let b = a;  // 'b' üçün yeni bir vərəq yaradılır və ora da "10" köçürülür.

console.log("Başlanğıcda a:", a); // 10
console.log("Başlanğıcda b:", b); // 10

// İndi 'b' öz vərəqini dəyişir.
b = 20; 

console.log("--- Dəyişiklikdən sonra ---");
console.log("'a' təsirlənmədi:", a); // Hələ də 10 olaraq qalır!
console.log("'b' dəyişdi:", b);      // Artıq 20-dir.
```

Yuxarıdakı misalda gördüyün kimi, `a` primitiv bir dəyər (ədəd) olduğu üçün, `b = a` əməliyyatı `a`-nın dəyərinin bir kopyasını `b`-yə verir. `b`-ni dəyişəndə `a` təsirlənmir.

***

**2. Obyektlər (Objects) — İstinadla Ötürülür:**
Obyektləri kopyaladığımızda, dəyərin özü deyil, obyektin yaddaşdakı yeri (**istinad**) kopyalanır. Bu o deməkdir ki, iki dəyişən eyni obyektə işarə edir. Bir dəyişən vasitəsilə obyektdə edilən hər hansı bir dəyişiklik, digər dəyişən vasitəsilə də görünür.

```javascript
let x = { name: "Rashad" }; // 'x' dəyişəni { name: "Rashad" } obyektinə işarə edir
let y = x;              // 'y' dəyişəni də 'x' ilə eyni obyektə işarə etməyə başlayır (istinad kopyalandı)

console.log(x.name); // "Rashad"
console.log(y.name); // "Rashad"

y.name = "Vəli"; // 'y' vasitəsilə obyektin 'name' xüsusiyyətini dəyişirik

console.log(x.name); // Nəticə: "Vəli" olacaq! Çünki 'x' də 'y' də eyni obyektə baxır.
console.log(y.name); // Nəticə: "Vəli"
```
Yəni `x` və `y` **eyni obyektə baxır**. Birində edilən dəyişiklik o birində də görünür.

---

###  Obyektlər Dinamikdir!

JavaScript-də obyektlər çox dinamikdir. Bu o deməkdir ki, onlar yaradıldıqdan sonra belə, strukturları asanlıqla dəyişdirilə bilər:

* **Yeni xüsusiyyət əlavə edə bilərsən:** Mövcud obyektdə olmayan yeni xüsusiyyətlər təyin edə bilərsən.
* **Xüsusiyyəti silə bilərsən:** `delete` operatorundan istifadə edərək obyektdən istənilən xüsusiyyəti qaldıra bilərsən.
* **Mövcud xüsusiyyətləri dəyişdirə bilərsən:** Bir xüsusiyyətin dəyərini istənilən vaxt yeniləyə bilərsən.

```javascript
let user = {
  name: "Ayan",
  email: "ayan@ay.com"
};
console.log(user); // { name: "Ayan", email: "ayan@ay.com" }

user.age = 22; // Yeni xüsusiyyət əlavə etdik
console.log(user); // { name: "Ayan", email: "ayan@ay.com", age: 22 }

user.email = "ayan.garayev@ay.com"; // Email xüsusiyyətinin dəyərini dəyişdik
console.log(user); // { name: "Ayan", email: "ayan.garayev@ay.com", age: 22 }

delete user.age; // age xüsusiyyətini sildik
console.log(user); // { name: "Ayan", email: "ayan.garayev@ay.com" }
```

---

### 💡 Obyektlərdə ən çox istifadə olunan əməliyyatlar:

* 🔨 **Yaratmaq:** Boş bir obyekt (`let obj = {}`) və ya xüsusiyyətləri ilə birgə (`let person = { name: "Rashad" }`) yarada bilərsən.

* 🧾 **Xüsusiyyət oxumaq (Accessing Properties):** Obyektin xüsusiyyətlərinə iki əsas yolla daxil olmaq olar:
    * **Nöqtə notasiyası (Dot Notation):** `obj.name` - Bu, xüsusiyyət adının bilindiyi və keçərli bir identifikator olduğu hallarda ən çox istifadə olunan yoldur.
    * **Kvadrat mötərizə notasiyası (Bracket Notation):** `obj['name']` - Bu notasiya, xüsusiyyət adı bir dəyişkəndə saxlandığı, yaxud adında boşluq, defis və ya rəqəmlə başlayan kimi xüsusi simvollar olduğu hallarda istifadə olunur.
        ```javascript
        let myKey = "age";
        let person = { name: "Zaur", age: 28, "full-name": "Zaur Əliyev" };

        console.log(person.age);        // 28 (Dot Notation)
        console.log(person[myKey]);     // 28 (Bracket Notation with a variable key)
        console.log(person["full-name"]); // "Zaur Əliyev" (Bracket Notation for special characters)
        ```
* 🖊️ **Xüsusiyyət əlavə etmək/dəyişmək:** `obj.age = 25` və ya `obj['age'] = 25`
* ❌ **Xüsusiyyət silmək:** `delete obj.age`
* ❓ **Xüsusiyyətin mövcudluğunu yoxlamaq:** `"name" in obj` (boolean dəyər qaytarır: `true` və ya `false`).
* 🔄 **Bütün xüsusiyyətləri gəzmək (Iterating):** Ən çox `for...in` dövrü ilə (`for (let key in obj) { ... }`) istifadə olunur.

---

## 🧬 Obyektlər və İrsiyyət (Prototypal Inheritance)  

JavaScript-də hər obyekt, başqa bir obyektin xüsusiyyətlərini və methodlarını **irsən ala bilər**. Bu mexanizmə **prototypal inheritance** deyilir. Bu, obyektlərin bir-biri ilə əlaqə quraraq ortaq funksionallıqları paylaşmasına imkan verir.

Bu irsən alınan obyektə **prototype** (prototip) deyilir. Yəni, əgər sən bir obyektin xüsusiyyətinə və ya metoduna daxil olmaq istəyirsənsə, JavaScript əvvəlcə həmin obyektin özündə bu xüsusiyyəti axtarır. Əgər tapmasa, avtomatik olaraq onun **prototype-ına** baxır. Bu axtarış zəncir boyu davam edir, ta ki xüsusiyyət tapılana və ya zəncirin sonuna çatana qədər.

Demək olar ki, hər bir JavaScript obyekti bir prototype-a malikdir. Bu zəncirin ən yuxarı hissəsində isə **`Object.prototype`** dayanır. Məsələn, `toString()` (obyekti stringə çevirir) və ya `hasOwnProperty()` (obyektin özündə bir xüsusiyyətin olub-olmadığını yoxlayır) kimi bir çox obyekt methodları məhz `Object.prototype`-dan irsən gəlir

---

### 🔗 Bu necə işləyir?

Təsəvvür et:

* Sənin bir **ata** obyektin var.
* Sən də onun **övladı**san.
* Əgər sənin özündə bir xüsusiyyət yoxdursa, JavaScript gedib **atanın üstünə baxır**.
* Əgər atanızda da yoxdursa, onun atasına, və s. bu cür **zəncir** gedir.
* Bu zəncirə **prototype chain** deyilir.

---


## 💻 Frontend Developer və Intern (Prototypal Inheritance)

Təsəvvür et bir şirkətdə:

* 👨‍💻 **FrontendDeveloper** – əsas veb funksiyaları bilir: məsələn, səhifə düzəltmək.
* 🧑‍💻 **Intern** – bu developer-dən **miras alır**, amma özünün də bacarığı var: məsələn, düymə dizayn etmək.

```javascript
const FrontendDeveloper = {
  sehifeYarat: function () {
    console.log("Mən responsiv veb səhifə yaradıram! 📱💻");
  }
};

// Intern frontend developerdən irs alır
const Intern = Object.create(FrontendDeveloper);

// Özünəməxsus bacarığı
Intern.unitTestYaz = function () {
  console.log("Mən unit test yaziram!");
};

// Öz bacarığını işlədir
Intern.unitTestYaz();     // Mən unit test yaziram!

// İrsən aldığı bacarığı işlədir
Intern.sehifeYarat();       // Mən responsiv veb səhifə yaradıram! 📱💻
```

---

### 🧠 İzah :

* "`Object.create()` bir obyektin başqa bir obyektlə əlaqəsini qurur. Bu əlaqə vasitəsilə biri digərindən metod və xüsusiyyət \*\*götürə bilər.\`"
* "`Prototype zənciri` JavaScript-in əsas **irsiyyət modelidir**. Sinif (class) anlayışından fərqli olaraq \*\*obyektlərdən obyektlərə keçid var.\`"

---

### 🧪 "Own Property" nədir?

Bəzən, obyektin **özündə olan xüsusiyyətləri** ilə **prototipindən irsən gələn xüsusiyyətlər** arasında fərq qoymaq vacib olur.

🔍 **JavaScript-də "own property"** termini, yalnız obyektin birbaşa özünə məxsus (yəni, prototipdən irsən alınmamış) xüsusiyyətlərə deyilir. `hasOwnProperty()` metodu bu fərqi yoxlamaq üçün istifadə olunur:

```javascript
let baseObject = {
  a: 1,
  b: 2
};

let myObject = Object.create(baseObject); // myObject-in prototipi baseObject-dir
myObject.c = 3; // 'c' myObject-in öz xüsusiyyətidir

console.log(myObject.a); // 1 (baseObject-dən irsən gəldi)
console.log(myObject.c); // 3 (myObject-in öz xüsusiyyətidir)

console.log(myObject.hasOwnProperty('a')); // false (çünki 'a' prototipdən gəlir)
console.log(myObject.hasOwnProperty('c')); // true (çünki 'c' myObject-in öz xüsusiyyətidir)
```

---

#  6.2 Obyekt Yaratmaq (Creating Objects)

JavaScript-də **object (obyekt)** yaratmağın bir neçə yolu var:
-  **Object literal** ilə  
- `new` açar sözü ilə  
-  `Object.create()` funksiyası ilə

---
##  6.2.1 Object Literals (Obyekt Literalı)

**Obyekt literalı (Object Literal)** JavaScript-də obyekt yaratmağın **ən sadə, ən qısa və ən çox istifadə olunan** yoludur. Bu üsulda, obyekt birbaşa kodun içində, **curly braces** (fiqurlu mötərizələr) `{}` daxilində, **`açar (key): dəyər (value)`** cütlükləri şəklində təyin olunur. 

###  Sintaksis:

```javascript
let objectName = {
  property1: value1,
  property2: value2,
  // ... daha çox property
};
```

###  Sadə Misallar:

```javascript
let empty = {}; // 🔹 Tamamilə boş bir obyekt yaradır

let point = {
  x: 0,
  y: 0
}; // 🔹 İki ədədi dəyər (x və y koordinatları) saxlayan obyekt

let p2 = {
  x: point.x,
  y: point.y + 1 
  // Mövcud 'point' obyektinin dəyərlərindən istifadə edərək yeni dəyər hesablayır
}; // 🔹 Digər obyektin xüsusiyyətlərinə əsaslanaraq yeni obyekt yaradır
```

---

### 📚 Mürəkkəb obyekt nümunəsi:

Obyekt literalları içərisində başqa obyektlər, massivlər və ya müxtəlif data tipləri saxlaya bilər ki, bu da onları real dünya məlumatlarını modelləşdirmək üçün çox güclü edir:

```javascript
let book = {
  "main title": "JavaScript",          
  // Adında boşluq olan xüsusiyyət – mütləq dırnaq içində yazılmalıdır
  "sub-title": "The Definitive Guide", 
  // Adında defis olan xüsusiyyət – string kimi yazılmalıdır
  for: "all audiences",                
  // ⚠️ `for` JavaScript-in rezerv açar sözüdür. Lakin burada property adı kimi istifadə olunduğu üçün, bu halda problem yaratmır.
  author: {                           
  //  Daxilində başqa bir obyekt saxlayan xüsusiyyət
    firstname: "David",
    surname: "Flanagan"
  },
  pages: 1200,                         // Ədədi dəyər
  isAvailable: true                    // Boolean dəyər
};
```

### 💡 Qeydlər:

* **Xüsusiyyət adı (property key)** JavaScript **identifikatoru** (məs: `name`, `x`) ola bilər və ya **string literal** (`"main title"`, `"sub-title"`). Əgər xüsusiyyət adı JavaScript identifikatoru olmaya biləcək simvollar (məsələn, boşluq, defis, rəqəmlə başlama) -dan ibarətdirsə, onu mütləq dırnaq içində string kimi yazmalısan.
* **Xüsusiyyət dəyəri (property value)** isə istənilən **JavaScript ifadəsi** ola bilər — bu, sadə bir dəyər, başqa bir dəyişkən, obyekt, massiv və ya hətta bir funksiya ola bilər.
* Obyektin **son xüsusiyyətindən sonra vergül (trailing comma)** qoymaq **ən yaxşı təcrübədir (best practice)**:

```javascript
let user = {
  name: "Ayan",
  age: 25, // ✅ Bu vergül sintaktik cəhətdən düzgündür və tövsiyə olunur
};
```

---

### 🔁 Hər dəfə yeni obyekt

**Object literal** bir **ifadə (expression)** olduğu üçün, **hər çağırıldıqda (yəni, hər dəfə icra olunduqda) yeni və fərqli bir obyekt** yaradır. Bu, yaddaşda tamamilə ayrı bir yer tutan yeni bir obyekt instansı deməkdir.

`Date.now()` sənə **hal-hazırkı anın zaman damğasını (timestamp)** qaytarır — bu da **1970-01-01 00:00:00 UTC** tarixindən bəri keçən **millisaniyə sayıdır**.

```javascript
function createUser(name) {
  return {
    username: name,
    createdAt: Date.now() // Funksiyanın hər çağırılışında fərqli zaman olur
  };
}

let u1 = createUser("Rashad");
// Bir neçə milli saniyə gözləyək (və ya başqa kod icra olunsun)
setTimeout(() => {
  let u2 = createUser("Ayan");
  console.log(u1 !== u2); // true – u1 və u2 yaddaşda fərqli obyektlərdir
  console.log(u1.createdAt); // Məsələn: 1718029200000
  console.log(u2.createdAt); // Məsələn: 1718029200050 (bir qədər sonra yaranıb)
}, 50);
```

Burada `createUser()` funksiyası çağırıldıqca **fərqli obyektlər** yaranır və onların `createdAt` dəyərləri də təbii olaraq fərqli olur, çünki `Date.now()` çağırıldığı anın zaman (timestamp) qaytarır. ⏱️

---

# 🏗️ 6.2.2 `new` ilə Obyekt Yaratmaq

JavaScript-də obyekt yaratmağın başqa bir üsulu **`new` operatorundan** istifadə etməkdir. 🔧 Bu operator, adətən **konstruktor funksiyaları (constructor functions)** və ya **siniflərlə (classes)** birlikdə istifadə olunur.

### 🧠 `new` operatoru nə edir?

`new` operatoru çağırıldıqda bir neçə addımı avtomatik olaraq yerinə yetirir:

1.  **Yeni Obyekt Yaradır:** Boş bir JavaScript obyekti yaradır.
2.  **Prototipə Bağlayır:** Yaradılan bu yeni obyektin prototipini (prototype) konstruktor funksiyasının `prototype` xüsusiyyətinə bağlayır.
3.  **Konstruktor Funksiyasını Çağırır:** Konstruktor funksiyasını yeni yaradılmış obyektin kontekstində (`this` açar sözü yeni obyektə işarə edir) çağırır. Bu, obyektin ilkin dəyərlərlə doldurulmasını (initialize) təmin edir. Konstruktor funksiyasının əsas məqsədi budur.
4.  **Obyekti Qaytarır:** Əgər konstruktor funksiyası açıq şəkildə bir obyekt qaytarmasa, `new` operatoru avtomatik olaraq yeni yaradılmış və doldurulmuş (initialized) obyekti qaytarır.

Bu funksiyaya **konstruktor (constructor)** deyilir — yəni obyektin ilk dəyərlərini təyin edən və onun quruluşunu formalaşdıran funksiya. 🏗️

---

### 📚 Misallar – JavaScript-in daxili konstruktorları:

JavaScript-in özünün bəzi daxili (built-in) konstruktorları var ki, onlarla `new` operatoru vasitəsilə müəyyən tipli obyektlər yarada bilərik:

```javascript
let o = new Object(); // ✅ Boş bir obyekt yaradır. Bu, `{}` ilə obyekt yaratmaqla eyni nəticə verir.
console.log(o); // {}

let a = new Array(); // ✅ Boş bir massiv (array) yaradır. Bu, `[]` ilə massiv yaratmaqla eyni nəticə verir.
console.log(a); // []

let d = new Date(); // ✅ Hazırkı tarixi və vaxtı göstərən bir Date obyekt yaradır.
console.log(d); // Məsələn: 2025-06-11T11:02:50.000Z

let r = new RegExp("pattern"); // ✅ Bir müntəzəm ifadə (regular expression) obyekti yaradır.
console.log(r); // /pattern/

let m = new Map(); // ✅ Açar/dəyər (key/value) cütlərini saxlayan bir Map obyekti yaradır.
console.log(m); // Map(0) {}

let s = new Set(); // ✅ Unikal dəyərlər saxlayan bir Set obyekti yaradır.
console.log(s); // Set(0) {}
```

### 🔧 Nəticə:

`new` operatoru ilə istənilən **built-in (daxili)** konstruktorla obyekt yarada bilərsən. Adətən, `Object()` və `Array()` üçün obyekt literalları `{}` və `[]` istifadə etmək daha qısa və oxunaqlı olduğu üçün daha çox üstün tutulur.

Lakin, `Date`, `RegExp`, `Map`, `Set` kimi daha mürəkkəb daxili tiplər üçün `new` operatoru vacibdir.

Ən əsası isə, sən **öz konstruktor funksiyalarını** (və ya ES6 ilə gələn sinifləri (classes)) da yaza bilərsən ki, bu da xüsusi növ obyektlər yaratmağa imkan verir. Bu mövzu daha sonra, **9-cu fəsildə** daha ətraflı izah olunacaq. 📘

---

# 🧬 6.2.3 Prototiplər (Prototypes)

Obyekt yaratmağın üçüncü üsuluna keçməzdən əvvəl, JavaScript-in ən təməl və vacib anlayışlarından biri olan **prototip** konsepsiyasını dərindən başa düşməliyik. Bu, JavaScript-in necə işlədiyini anlamaq üçün açar nöqtələrdən biridir. 🧠

### 🧩 Prototip nədir?

JavaScript-də **demək olar ki, hər obyektin** başqa bir obyektlə gizli bir əlaqəsi var. Bu əlaqəli obyektə **prototype** (prototip) deyilir. Bu, obyektlərin **xüsusiyyətləri (properties)** və **metodları (methods)** başqa obyektlərdən **irsən almasına (inherit)** imkan verən mexanizmdir.

Qısaca desək, bir obyekt bir xüsusiyyətə daxil olmaq istədikdə, əvvəlcə özündə axtarır. Əgər tapmasa, **prototipinə** baxır. Əgər orada da tapmasa, prototipin prototipinə baxır və bu proses zəncir boyunca davam edir.

---

### 🧱 Hansı obyektin prototipi nədir?

Hər bir obyektin prototipi, onun necə yaradılmasından asılı olaraq dəyişə bilər:

| Necə yaradılıb?                  | Əsas prototipi nədir?      | Açıqlama                                                                          |
| :------------------------------- | :------------------------- | :-------------------------------------------------------------------------------- |
| `{}` və ya `new Object()`         | `Object.prototype`         | Boş obyektlər və ya `Object` konstruktoru ilə yaradılan obyektlər `Object.prototype`-dan irsən gəlir. |
| `[]` və ya `new Array()`          | `Array.prototype`          | Massivlər (arrays) `Array.prototype`-dan irsən gəlir. Bu prototip `push()`, `pop()`, `map()` kimi metodları təmin edir. |
| `new Date()`                     | `Date.prototype`           | `Date` obyektləri `Date.prototype`-dan irsən gəlir. Bu prototip `getFullYear()`, `getMonth()` kimi metodları təmin edir. |
| `new Function()` və ya funksiya deklarasiyası | `Function.prototype`       | Funksiyalar `Function.prototype`-dan irsən gəlir. |

🔗 Unutma ki, bütün bu prototiplər (məsələn, `Array.prototype`, `Date.prototype`, `Function.prototype`) sonda yenidən **`Object.prototype`-ə bağlanır**. Bu ardıcıllığa **prototip zənciri (prototype chain)** deyilir. Yəni, bütün obyektlər (bir neçə istisna ilə) nəticədə `Object.prototype`-dən metodları miras ala bilir.

---

### 🔍 Qarışdırıcı məqam: `prototype` xüsusiyyəti və `[[Prototype]]` daxili yuvası

Bu iki termin bir-birinə çox bənzəsə də, JavaScript-də fərqli mənalara malikdir və bəzən çaşqınlıq yaradır. Gəlin fərqləri aydınlaşdıraq:

1.  **`[[Prototype]]` (Daxili yuva / Internal Slot):**
    * Bu, **hər bir obyektin** (funksiyalar da daxil olmaqla) sahib olduğu **gizli, daxili bir referansdır**.
    * Bu referans, həmin obyektin **kimdən irsən gəldiyini** göstərən prototip obyektə işarə edir.
    * Sən `[[Prototype]]` daxili yuvasına birbaşa daxil ola bilməzsən. Onu əldə etmək üçün `Object.getPrototypeOf()` kimi metodlardan və ya bəzi brauzerlərdə `__proto__` (köhnəlmiş) xüsusiyyətindən istifadə olunur.
    * **Məqsədi:** Obyektin prototip zəncirindəki növbəti həlqəsini təyin etmək.

2.  **`prototype` (Açıq Xüsusiyyət / Public Property):**
    * Bu, **yalnız funksiyaların** (xüsusilə konstruktor funksiyalarının) sahib olduğu **adi bir xüsusiyyətdir**.
    * Bu xüsusiyyətin dəyəri, həmin funksiya ilə **`new` operatoru vasitəsilə yaradılan obyektlərin prototipi olacaq** obyektə işarə edir.
    * **Məqsədi:** Yeni obyektlərin hansı prototipə sahib olacağını müəyyənləşdirmək.

Misala baxaq:

```javascript
function User(name) { // 'User' bir konstruktor funksiyasıdır
  this.name = name;
}

let u = new User("Cavid"); // 'u' obyekti 'User' konstruktoru ilə yaradıldı

// 'u' obyektinin [[Prototype]]-i 'User.prototype' obyektidir.
console.log(Object.getPrototypeOf(u) === User.prototype); // ✅ true (u-nun prototipi User.prototype-dir)

// 'User' funksiyasının özünün 'prototype' xüsusiyyəti var.
console.log(typeof User.prototype); // "object"
```

Burada `User.prototype` yeni yaranan `u` obyektinin `[[Prototype]]`-i (yəni, prototipi) rolunu oynayır.

---

### 🧤 `Object.prototype` haqqında xüsusi qeyd

`Object.prototype` – prototip zəncirinin ən yuxarı hissəsində yerləşən **xüsusi bir obyektdir**:
* Bu **yeganə obyektlərdən biridir ki, heç bir prototipi yoxdur**. Yəni, o heç nədən irsən gəlmir — bu, **prototip zəncirinin başlanğıcı və ya sonudur**. 🔚
* `Object.prototype` digər bütün obyektlərin miras ala biləcəyi `toString()`, `hasOwnProperty()`, `isPrototypeOf()` kimi fundamental metodları özündə saxlayır.

---

### 🔗 Prototip Zənciri (Prototype Chain) – Axtarış Mexanizmi

JavaScript-də bir obyektin xüsusiyyətinə və ya metoduna daxil olmaq istədikdə, JavaScript aşağıdakı ardıcıllığı izləyir:

1.  **Əvvəlcə obyektin özündə axtarır.**
2.  Əgər tapmasa, obyektin **`[[Prototype]]`-inə (yəni, prototipinə)** baxır.
3.  Prototipində də tapmasa, prototipin `[[Prototype]]`-inə baxır və bu beləcə **prototip zənciri boyunca** davam edir.
4.  Axtarış `Object.prototype`-ə çatdıqda və orada da tapılmasa, `undefined` qaytarılır.

Misal:

```javascript
let d = new Date(); 
// 'd' obyektinin prototip zənciri: d → Date.prototype → Object.prototype

// d.toString() metodunu çağırırıq
// 1. 'd' obyektinin özündə 'toString()' varmı? Xeyr.
// 2. 'd'-nin prototipinə ('Date.prototype'-ə) baxır. 'Date.prototype'-də 'toString()' metodu var!
// 3. Tapdı və icra etdi.
console.log(d.toString()); 
// Nəticə: "Wed Jun 11 2025 15:05:01 GMT+0400 (Azerbaijan Standard Time)" (tarixə uyğun)

// d.hasOwnProperty('something') metodunu çağırırıq
// 1. 'd' obyektinin özündə 'hasOwnProperty()' varmı? Xeyr.
// 2. 'd'-nin prototipinə ('Date.prototype'-ə) baxır. 'Date.prototype'-də 'hasOwnProperty()' varmı? Xeyr.
// 3. 'Date.prototype'-in prototipinə ('Object.prototype'-ə) baxır. 'Object.prototype'-də 'hasOwnProperty()' metodu var!
// 4. Tapdı və icra etdi.
console.log(d.hasOwnProperty('getDate')); // false (getDate Date.prototype-dədir, 'd'-nin özündə deyil)
console.log(d.hasOwnProperty('toString')); // false (toString Date.prototype-dədir, 'd'-nin özündə deyil)
```
Beləliklə, obyekt həm `Date.prototype`-dən, həm də `Object.prototype`-dən **xüsusiyyətləri və metodları miras ala bilir**. Bu mexanizm, JavaScript-də kodun təkrar istifadəsi (reusability) üçün fundamentaldır.

---

# 🧱 6.2.4 `Object.create()` ilə Obyekt Yaratmaq

`Object.create()` funksiyası — JavaScript-də obyekt yaratmağın **çevik və güclü** üsullarından biridir. ⚙️ Bu metod sənə birbaşa **yeni yaradılacaq obyektin prototipini özün təyin etməyə** imkan verir.

### Sintaksis:

`Object.create(proto, [propertiesObject])`

* `proto`: Yeni obyektin prototipi olacaq obyekt. Bu arqument `null` da ola bilər.
* `propertiesObject` (optional): Yeni obyektə əlavə olunacaq xüsusiyyətləri təyin edən, xüsusiyyət təsvirlərini (property descriptors) ehtiva edən obyekt. Bu, ən çox §14.1-də müzakirə olunacaq xüsusiyyət atributları (`writable`, `enumerable`, `configurable`) ilə bağlıdır.

```javascript
let protoObj = { x: 1, y: 2 };
let o1 = Object.create(protoObj); // 'o1' adlı yeni obyekt yaradılır

console.log(o1);         // {} (boş görünür, çünki x və y öz xüsusiyyəti deyil)
console.log(o1.x);       // Nəticə: 1 (çünki 'x' dəyəri 'o1'-in prototipindən götürülür)
console.log(o1.y);       // Nəticə: 2 (çünki 'y' dəyəri 'o1'-in prototipindən götürülür)
console.log(o1.x + o1.y); // => 3 ✅

// Əmin olmaq üçün prototipə baxaq:
console.log(Object.getPrototypeOf(o1) === protoObj); // true
console.log(o1.hasOwnProperty('x')); // false (xüsusiyyət özündə yox, prototipdədir)

o1.z = 3; // 'z' yeni xüsusiyyət kimi 'o1'-in özünə əlavə olunur
console.log(o1.z);       // 3
console.log(o1.hasOwnProperty('z')); // true
```

Burada `o1` adlı yeni obyekt yaradılır və onun `[[Prototype]]`-i (yəni, prototipi) `protoObj` obyekti (`{ x: 1, y: 2 }`) olur. Yəni `o1.x` və `o1.y`-ə daxil olmaq istədikdə, JavaScript bu dəyərləri **prototipindən götürür**.

---

### 🚫 `null` Prototipi ilə Obyekt Yaratmaq

Əgər `Object.create(null)` istifadə etsən, nəticədə **heç bir prototipi olmayan** bir obyekt yaranır:

```javascript
let o2 = Object.create(null);

console.log(o2); // [Object: null prototype] {} - brauzer konsolunda fərqli görünə bilər
```

❌ Bu obyekt:

* `toString()`, `hasOwnProperty()` kimi `Object.prototype`-dən gələn heç bir metodu **irsən almır**.
* Bu səbəbdən, onun üzərində bu cür metodları çağırsan xəta verəcək: `o2.toString()` // TypeError!
* Bəzi JavaScript operatorları (məsələn, `+` operatoru obyektləri stringə çevirərkən `toString()` metoduna ehtiyac duyarsa) onunla **düzgün işləməyə bilər**.

Bu üsul adətən **tamamilə təmiz obyektlər yaratmaq** üçün istifadə olunur – məsələn, digər obyektlərin daxili metodlarından təsirlənməməsi lazım olan `dictionary` tipli obyektlər və ya `JSON` tipli məlumatları saxlamaq üçün təhlükəsiz bir qab kimi. 🛡️

---

### 🧼 Adi Boş Obyekt Yaratmaq

Əgər sadəcə `{}` və ya `new Object()` ilə eyni nəticəni almaq istəyirsənsə (yəni, `Object.prototype`-dən irsən gələn adi bir boş obyekt), belə yaza bilərsən:

```javascript
let o3 = Object.create(Object.prototype);

console.log(o3); // {}
console.log(o3.toString()); // "[object Object]" - işləyir, çünki Object.prototype-dən gəlir
```

Bu, **adi boş obyekt** kimidir – `toString()` və digər ümumi metodlara malikdir. ✅

---

### ⚡ `Object.create()` niyə bu qədər güclüdür?

`Object.create()` sənə birbaşa prototip zəncirini manipulyasiya etmək imkanı verir. Bu, aşağıdakı üstünlükləri təmin edir:

* ✅ **Özün seçdiyin prototipi olan obyekt yaratmaq:** Bu, klassik irsiyyət modelləri olmadan obyektlər arasında irsiyyət əlaqələri qurmağa imkan verir.
* ✅ **Orijinal (prototip) obyektə toxunmadan irsən gələn xüsusiyyətləri istifadə etmək:** Yeni obyekt prototipdəki xüsusiyyətləri "görür", lakin onların dəyərlərini özündə saxlamır.
* ✅ **Təhlükəsizlik və immutability (dəyişilməzlik) senariləri:** Funksiyalara obyektlərin təhlükəsiz nüsxələrini ötürməyə imkan verir.

---

### 🛡️ Real Dünya Nümunəsi: Təhlükəsiz Obyekt Nüsxəsi (Shallow Copy)

Təsəvvür et ki, hansısa bir kitabxana funksiyasına obyekt ötürürsən, amma onun bu obyektə **təsadüfən dəyişiklik etməsini istəmirsən**. `Object.create()` bu cür ssenarilərdə faydalı ola bilər:

```javascript
let originalData = {
  id: 1,
  name: "Məhsul A",
  price: 100,
  description: "Bu dəyər dəyişilməməlidir."
};

// 'originalData'-nı prototip kimi istifadə edərək yeni bir obyekt yaradırıq.
// Bu yeni obyekt 'originalData'-nın bütün xüsusiyyətlərini irsən alır.
let safeView = Object.create(originalData);

console.log(safeView.description); // "Bu dəyər dəyişilməməlidir." - orijinaldan oxundu

// Təsəvvür et ki, hansısa funksiya 'safeView' üzərində işləyir:
function processProduct(productObj) {
  productObj.description = "Bu dəyişdirildi!"; // productObj-nin özündə yeni bir "description" yaradır
  productObj.price = 120; // price-i dəyişdirməyə çalışır
  console.log("Daxili funksiya: ", productObj.description);
}

processProduct(safeView);

console.log("Orijinal obyektin description-ı:", originalData.description); // Nəticə: "Bu dəyər dəyişilməməlidir."
console.log("Orijinal obyektin price-i:", originalData.price); // Nəticə: 100
console.log("SafeView-in description-ı:", safeView.description); // Nəticə: "Bu dəyişdirildi!"
console.log("SafeView-in price-i:", safeView.price); // Nəticə: 120
```

Bu nümunədə:
* `processProduct` funksiyası `safeView` obyektinin `description` xüsusiyyətini dəyişməyə çalışdıqda, `safeView`-in **özündə** yeni bir `description` xüsusiyyəti yaranır. `originalData`-dakı `description` dəyəri dəyişməz qalır.
* `price` xüsusiyyəti də eyni şəkildə işləyir.
* 📖 Funksiya `originalData`-nın xüsusiyyətlərini **oxuya bilər**.
* ❌ Amma onları dəyişsə də, bu dəyişiklik **əsl `originalData` obyektinə təsir etməz** — dəyişikliklər yalnız yeni `safeView` obyektinin özündə qalır. Bu, **"shadowing"** (kölgələmə) adlanır.

---

### 📌 Qeyd: İkinci Arqument

`Object.create()` funksiyası həmçinin **ikinci əlavə argument** qəbul edə bilir. Bu argument, obyektin **öz xüsusiyyətlərini** (`own properties`) `configurable`, `writable`, `enumerable` kimi **atributları ilə birlikdə** detallı şəkildə təyin etməyə imkan verir. Bu mövzu, xüsusiyyətlərin daha dərin səviyyədə idarə olunması üçün vacibdir və **§14.1**-də ətraflı izah olunacaq. 📘🔍

---

### ⏭️ Növbəti Addım

Prototip zənciri və `Object.create()` ilə obyektlərin necə işlədiyini anladıq. İndi isə obyektlərdə xüsusiyyətlərə necə **daxil olmaq (access)**, **dəyər yazmaq (set)** və onları **silmək (delete)** kimi əsas əməliyyatların mexanizmlərinə keçəcəyik. Bu konseptlər, yuxarıda öyrəndiyimiz prototip zənciri mexanizmlərinin real kodda **nəyə görə və necə işlədiyini** tam olaraq başa düşmək üçün kritik əhəmiyyət kəsb edir. 🧠🔎

---

# 6.3 Xüsusiyyətlərə Daxil Olmaq və Təyin Etmək (Querying and Setting Properties)

JavaScript-də obyektlərin ən fundamental əməliyyatlarından biri onların daxilində saxladıqları xüsusiyyətlərə (properties) daxil olmaq (oxumaq) və ya onların dəyərlərini dəyişdirmək (yazmaq, təyin etmək)dir. Bu əməliyyatlar üçün iki əsas operatordan istifadə edirik: **dot operator (`.`)** və **square bracket operator (`[]`)**. (Bu operatorlar haqqında ilkin məlumatı §4.4-də əldə etmişdin.)

### 🔍 Xüsusiyyətlərin dəyərini almaq (Querying Properties)

Bir obyektin xüsusiyyətinin dəyərini oxumaq üçün hər iki operatordan istifadə edə bilərsən:

* **Dot operator (`.`):**
    Bu operatoru istifadə edərkən, soldakı ifadə obyekt olmalıdır (məsələn: `book`), sağdakı isə **birbaşa xüsusiyyətin adı olan sadə bir identifikator** olmalıdır (məsələn: `author`, `name`). Bu üsul ən çox yayılmış və ən oxunaqlı üsuldur, ancaq xüsusiyyət adının sintaktik cəhətdən keçərli bir identifikator olması tələb olunur (yəni, rəqəmlə başlamamalı, boşluq və ya defis kimi xüsusi simvollar ehtiva etməməlidir).

    ```javascript
    let book = {
      title: "JavaScript Programming",
      author: {
        firstname: "John",
        surname: "Doe"
      }
    };

    let author = book.author;     // book obyektinin "author" xüsusiyyətini alırıq
    let surname = author.surname; // author obyektinin "surname" xüsusiyyətini alırıq

    console.log(surname); // "Doe"
    console.log(book.title); // "JavaScript Programming"
    ```

* **Square bracket operator (`[]`):**
    Bu operatoru istifadə edərkən, mötərizələrin `[]` daxilindəki ifadə ya **string** tipində olmalı, ya da string-ə çevrilə bilən bir dəyər, yaxud **Symbol** (§6.10.3) olmalıdır. Bu üsul, xüsusiyyət adında boşluq, defis, rəqəmlə başlama kimi xüsusi simvollar olduqda və ya xüsusiyyət adının proqram icra olunarkən (runtime) dinamik olaraq təyin edildiyi hallarda çox vacibdir.

    ```javascript
    let book = {
      "main title": "JavaScript: The Good Parts", // Adında boşluq var
      "publication-year": 2008,                   // Adında defis var
      "author": "Douglas Crockford"
    };

    let mainTitle = book["main title"]; // book obyektinin "main title" xüsusiyyətini alırıq
    let pubYear = book["publication-year"]; // book obyektinin "publication-year" xüsusiyyətini alırıq

    console.log(mainTitle); // "JavaScript: The Good Parts"
    console.log(pubYear);   // 2008
    ```

> **Qeyd:** Növbəti fəsildə (§7) `square bracket` operatorunda rəqəmlərin də (massiv indeksləri kimi) çox istifadə olunduğunu görəcəyik. Əslində, massiv indeksləri də JavaScript daxilində avtomatik olaraq string-ə çevrilir. Məsələn, `myArray[0]` əslində `myArray["0"]` kimi işləyir.

---

### ✏️ Xüsusiyyət yaratmaq və ya dəyərini dəyişmək (Setting Properties)

Yeni bir xüsusiyyət əlavə etmək və ya mövcud bir xüsusiyyətin dəyərini yeniləmək (dəyişdirmək) üçün də dot (`.`) və ya square bracket (`[]`) operatorlarından istifadə edirik. Lakin bu dəfə, onlar **assignment əməliyyatının (`=`) sol tərəfində** yer alır:

```javascript
let book = {
  "main title": "JavaScript",
  author: "Douglas"
};

book.edition = 7; // book obyektinə "edition" adlı yeni xüsusiyyət əlavə etdik
console.log(book.edition); // 7

book["main title"] = "ECMAScript"; // mövcud "main title" xüsusiyyətinin dəyərini dəyişdik
console.log(book["main title"]); // "ECMAScript"

book.author = { // Author xüsusiyyətinin dəyərini başqa bir obyektlə dəyişdik
  firstname: "David",
  surname: "Flanagan"
};
console.log(book.author.firstname); // "David"
```

---

**Xülasə: Dot vs. Square Bracket Operatorları**

| Əməliyyat              | Sintaksis (dot operator)  | Sintaksis (square brackets)      | Nə zaman istifadə olunur?                           |
| :--------------------- | :------------------------ | :------------------------------- | :-------------------------------------------------- |
| **Xüsusiyyət oxuma** | `object.property`         | `object["propertyName"]`         | **Dot:** Sabit, sadə identifikatorlar. **Bracket:** Dinamik adlar, boşluqlu/defisli adlar, rəqəmli adlar. |
| **Xüsusiyyət yazma** | `object.property = value` | `object["propertyName"] = value` | **Dot:** Sabit, sadə identifikatorlar. **Bracket:** Dinamik adlar, boşluqlu/defisli adlar, rəqəmli adlar. |

---

# 6.3.1 Obyektlər Assosiativ Massivlər Kimi (Objects As Associative Arrays)

Əvvəlki bölmədə izah etdiyimiz kimi, aşağıdakı iki JavaScript ifadəsi əksər hallarda eyni dəyəri qaytarır:

```javascript
object.property      // Məsələn: book.title
object["property"]   // Məsələn: book["title"]
```

Birinci sintaksis — **dot operator (`.`)** ilə və bir **identifikatorla** istifadə olunur. Bu, C++ və Java kimi **strongly typed (ciddi tipli)** dillərdə struct və ya obyektin **statik sahəsinə (static field)** daxil olmaq kimi oxşardır. Bu dillərdə obyektin sahib olacağı xüsusiyyətlər (sahələr) kodu yazarkən artıq müəyyən edilmiş olur.

İkinci sintaksis — **square bracket operator (`[]`)** ilə və içində **string (və ya stringə çevrilə bilən ifadə)** olan bir dəyərlə istifadə olunur. Bu, sanki bir **string indeksli massivə (array)** daxil olmaq kimidir. Proqramlaşdırma dünyasında belə massivlər **associative array** (digər adları: hash, map, dictionary) adlanır. Onlar dəyərləri indekslərin əvəzinə açarlar (keys) vasitəsilə saxlamağa imkan verir.

**JavaScript-də obyektlər təməlində assosiativ massivlərdir!** Bu bölmə bunun niyə bu qədər vacib və güclü bir xüsusiyyət olduğunu izah edir.

---

### 🤔 Strongly Typed vs. Loosely Typed (Dinamik Tipli) – Fərq Nədir?

* **Strongly typed (Ciddi tipli) dillər (məsələn: C, C++, Java):** Bu dillərdə obyektlərin yalnız öncədən təyin olunmuş və sayca məhdud olan xüsusiyyətləri (properties) ola bilər. Obyektin tipini təyin edərkən bütün xüsusiyyətlər bəyan edilir və proqram işləyərkən yeni xüsusiyyətlər əlavə etmək mümkün deyil.
* **JavaScript isə loosely typed (dinamik tipli) dildir:** Bu, obyektlərə **istənilən vaxt, istənilən sayda yeni xüsusiyyət əlavə etməyin** və ya mövcudlarını silməyin mümkün olduğu deməkdir. Bu dinamiklik JavaScript-i çox çevik edir.

---

### 🔸 Dot operator (`.`) – Sabit (Static) Xüsusiyyət Adları

Dot operatoru (`.`) ilə istifadə olunan property adı **identifier** kimi yazılır, yəni kodu yazarkən **sabitdir (hardcoded)** və proqram işləyərkən (runtime) bir dəyişkən vasitəsilə dəyişdirilə bilməz. Əgər sən bir xüsusiyyətə daxil olmaq üçün dəyişkəndən istifadə etmək istəyirsənsə, dot operatoru işləməyəcək.

```javascript
let myProperty = "name";
let person = { name: "Aysel", age: 25 };

// console.log(person.myProperty); 
// undefined olacaq! Çünki "person" obyektində "myProperty" adlı bir xüsusiyyət yoxdur.
// JavaScript `myProperty` sözünü bir identifikator kimi qəbul edir, dəyişkən kimi yox.
```

---

### 🔹 Square bracket operator (`[]`) – Dinamik (Dynamic) Xüsusiyyət Adları

Square bracket operatoru (`[]`) ilə property adı bir **string ifadəsi** kimi verilir. Bu o deməkdir ki, proqram icra olunarkən (runtime) bu string dəyəri **dinamik şəkildə yaradıla və ya dəyişdirilə bilər**.

Məsələn:

```javascript
let customer = {
  address0: "Nizami küçəsi 1",
  address1: "Səməd Vurğun küçəsi 5",
  address2: "Əhməd Cavad küçəsi 10",
  address3: "28 May küçəsi 20"
};

let addr = "";
for(let i = 0; i < 4; i++) {
  // `address${i}` ilə dinamik olaraq "address0", "address1" və s. stringlər yaradırıq.
  addr += customer[`address${i}`] + "\n"; // Square bracket operatoru dinamik stringlərlə işləyir
}
console.log(addr);
// Nəticə:
// Nizami küçəsi 1
// Səməd Vurğun küçəsi 5
// Əhməd Cavad küçəsi 10
// 28 May küçəsi 20
```

Bu kod `customer` obyektinin `address0`, `address1`, `address2`, `address3` xüsusiyyətlərinin dəyərlərini oxuyur və birləşdirir. Gördüyün kimi, xüsusiyyət adları loop (dövr) daxilində dinamik olaraq formalaşdırılır.

---

### ⚙️ Niyə Assosiativ Massivlər Belə Faydalıdır? (Praktik Nümunə)

Tutaq ki, sən istifadəçi portfelində (portfolio) saxladığı səhmlərin adlarını və sayını izləyən bir proqram yazırsan. Bu portfel `portfolio` adlı obyektlə təmsil olunur və hər bir səhmin adı **property adı (key)**, səhmin sayı isə həmin property'sinin **dəyəridir (value)**.

İstifadəçi səhmin adını proqramın işləmə vaxtında (runtime) daxil edir, ona görə də property adları əvvəlcədən məlum deyil. Bu səbəbdən, dot operatoru ilə birbaşa istifadə etmək mümkün deyil. Məhz burada square bracket operatoru imdadımıza çatır:

```javascript
// Yeni səhm əlavə edən funksiya
function addStock(portfolio, stockName, shares) {
  portfolio[stockName] = shares; // stockName dəyəri dinamikdir, ona görə square bracket istifadə edirik
  console.log(`${stockName} səhmi portfelə əlavə edildi: ${shares} ədəd.`);
}

let myPortfolio = {}; // Boş portfel obyekti

addStock(myPortfolio, "AAPL", 10); // Apple səhmi
addStock(myPortfolio, "GOOGL", 5); // Google səhmi
addStock(myPortfolio, "MSFT", 12); // Microsoft səhmi

console.log(myPortfolio); // { AAPL: 10, GOOGL: 5, MSFT: 12 }
```

Burada, `stockName` dəyişəni runtime zamanı dinamik təyin olunur, ona görə də yalnız square bracket operatoru ilə property oxumaq/yazmaq mümkündür.

---

### 🔄 `for...in` dövrü və Assosiativ Massivlər

Portfeldəki bütün səhmlərin ümumi dəyərini hesablamaq kimi hallarda `for...in` dövründən istifadə etmək çox faydalıdır. Bu dövr, obyektin bütün **nömrələnə bilən (enumerable)** xüsusiyyətlərinin adlarını ardıcıllıqla gəzməyə imkan verir:

```javascript
// Səhmlərin hazırkı qiymətini qaytaran fiktiv funksiya
function getQuote(stockSymbol) {
  const prices = {
    "AAPL": 170.00,
    "GOOGL": 180.00,
    "MSFT": 420.00
  };
  return prices[stockSymbol] || 0; // Qiymət tapılmasa 0 qaytar
}

function computePortfolioValue(portfolio) {
  let totalValue = 0.0;
  for(let stockSymbol in portfolio) {
    // `stockSymbol` dəyişəni hər iterasiyada property adını (string olaraq) saxlayır
    let shares = portfolio[stockSymbol];  // Bu səhmin sayı
    let price = getQuote(stockSymbol);    // Səhmin cari qiyməti
    totalValue += shares * price;         // Ümumi dəyərə əlavə et
  }
  return totalValue; // Portfelin ümumi dəyəri
}

let myPortfolio = { "AAPL": 10, "GOOGL": 5, "MSFT": 12 };
let portfolioWorth = computePortfolioValue(myPortfolio);
console.log(`Portfelin ümumi dəyəri: $${portfolioWorth.toFixed(2)}`);
// Nəticə: $8310.00 (10*170 + 5*180 + 12*420)
```

`for...in` dövrü həmçinin prototip zəncirindən gələn `enumerable` xüsusiyyətləri də gəzir. Bunu nəzərə almaq lazımdır. Yalnız obyektin "öz" xüsusiyyətlərini gəzmək istəyirsənsə, `hasOwnProperty()` metodundan istifadə etməyin tövsiyə olunur:

```javascript
for(let stockSymbol in portfolio) {
  if (portfolio.hasOwnProperty(stockSymbol)) { // Yalnız obyektin öz xüsusiyyətlərini nəzərə alırıq
    let shares = portfolio[stockSymbol];
    // ...
  }
}
```

---

### 🚩 ES6 və sonrakı versiyalarda: `Map` class-ı

Sadə obyektlər assosiativ massiv kimi işləməyinə baxmayaraq, **ES6 (ECMAScript 2015)** və sonrakı versiyalarda təqdim olunan **`Map` class-ı (§11.1.2)** daha əlverişli və effektiv assosiativ massiv (key-value kolleksiyası) kimi istifadə olunur.

`Map` obyektlərinə nisbətən bəzi üstünlüklərə malikdir:

* **İstənilən tipdə açar (key) istifadə edə bilmək:** `Map` obyektlərdən fərqli olaraq, açar kimi stringlərlə yanaşı, rəqəmlər, boolean dəyərlər, hətta digər obyektlər və funksiyalar da qəbul edə bilər.
* **Açarların sırası qorunur:** `Map` obyektlərində əlavə olunan açarların daxilolma sırası qorunur, bu isə `for...in` kimi dövrlərdə açarların hansı ardıcıllıqla gələcəyini proqnozlaşdırmağa imkan verir.
* **Performans:** Böyük həcmli data üçün `Map` obyektlər, xüsusilə tez-tez əlavə etmə/silmə əməliyyatları edildikdə, sadə obyektlərə nisbətən daha yaxşı performans göstərə bilər.
* **`size` xüsusiyyəti:** Birbaşa kolleksiyadakı elementlərin sayını verir.


---

# 6.3.2 İrsiyyət (Inheritance) 🧬🧩

JavaScript obyektləri həm **özünəməxsus xüsusiyyətlərə (own properties)**, həm də **prototip obyektindən miras aldıqları (inherited properties)** xüsusiyyətlərə sahib ola bilirlər. Bu səbəbdən, bir xüsusiyyətə daxil olarkən JavaScript-in necə davrandığını və bu irsiyyət mexanizminin necə işlədiyini yaxşı başa düşmək vacibdir.

---

### 🔗 Prototip Zənciri (Prototype Chain) – Xüsusiyyətlərin Axtarışı

Bildiyimiz kimi, JavaScript-də demək olar ki, hər obyektin bir **prototipi (prototype)** var (yeganə istisna `Object.prototype`-dir, onun prototipi `null`-dır). Bir obyektin xüsusiyyətinə (məsələn, `o.x`) daxil olmağa cəhd etdiyin zaman JavaScript aşağıdakı ardıcıllıqla axtarış aparır:

1.  **Obyektin özündə axtarır:** Əvvəlcə, JavaScript həmin xüsusiyyətin obyektin **özünə məxsus (own property)** olub-olmadığını yoxlayır.
2.  **Prototip zəncirini izləyir:** Əgər axtarılan xüsusiyyət obyektin özündə tapılmazsa, JavaScript dərhal onun **`[[Prototype]]`-inə (yəni, prototip obyektinə)** baxır.
3.  **Zəncir boyu davam edir:** Əgər prototipdə də tapılmazsa, prototipin `[[Prototype]]`-inə (zəncirdəki növbəti obyektə) baxılır və bu proses **`null`-a çatana qədər** davam edir.
4.  **`undefined` qaytarılır:** Əgər zəncirin sonuna qədər axtarılan xüsusiyyət tapılmazsa, nəticə olaraq `undefined` dəyəri qaytarılır.


```javascript
let o = {};           
// 'o' → Object.prototype (Object.prototype-in prototipi null-dır)
o.x = 1;              
// 'o'-nun öz xüsusiyyəti (own property): x = 1

let p = Object.create(o); 
// 'p'-nin prototipi 'o' obyektidir (p → o → Object.prototype)
p.y = 2;              
// 'p'-nin öz xüsusiyyəti: y = 2

let q = Object.create(p);  
// 'q'-nun prototipi 'p' obyektidir (q → p → o → Object.prototype)
q.z = 3;              
// 'q'-nun öz xüsusiyyəti: z = 3

console.log(q.x); 
// Nəticə: 1. Q.x-i axtararkən: q-da yoxdur → p-də yoxdur → o-da var (x=1) → tapdı!
console.log(q.y); 
// Nəticə: 2. Q.y-i axtararkən: q-da yoxdur → p-də var (y=2) → tapdı!
console.log(q.z); 
// Nəticə: 3. Q.z-i axtararkən: q-da var (z=3) → tapdı!

let f = q.toString(); 
// Nəticə: "[object Object]". toString() metodunu axtararkən: q-da yoxdur → p-də yoxdur → o-da yoxdur → Object.prototype-də var → tapdı!

console.log(f);
console.log(q.x + q.y); 
// Nəticə: 3 (miras alınan x və y dəyərləri istifadə olundu)
```

---

## 6.3.3 Xüsusiyyətə Daxil Olarkən Yaranan Xətalar (Property Access Errors) ⚠️❌

JavaScript-də obyekt xüsusiyyətlərinə daxil olmaq və onları təyin etmək prosesi adətən sadə olsa da, bəzi hallarda gözlənilməz xətalarla qarşılaşa bilərik. Bu bölmədə bu zaman baş verə biləcək **səhvlər və xəta növləri** izah olunur.

---

### ❓ Mövcud Olmayan Xüsusiyyətlər (Non-existent Properties)

* Bir obyektin **özünə və ya prototip zəncirinə** aid olmayan bir xüsusiyyəti soruşmaq **sintaktik səhv hesab edilmir** — nəticə sadəcə `undefined` dəyəri olur.

  ```javascript
  let book = { title: "JavaScript", author: "Rashad" };
  console.log(book.subtitle); 
  // Nəticə: undefined, çünki "subtitle" xüsusiyyəti mövcud deyil.
  console.log(book.year);     
  // Nəticə: undefined
  console.log(book.author)
  // Rashad
  ```

* `null` və ya `undefined` dəyərlərin heç bir xüsusiyyəti ola bilməz (primitiv dəyərlərdir). Ona görə də, bu dəyərlərdən birinin xüsusiyyətinə daxil olmağa cəhd etsən, JavaScript **TypeError** xətası atır. Bu, ən çox rast gəlinən JavaScript xətalarından biridir.

  ```javascript
  let len = book.subtitle.length; 
  // book.subtitle 'undefined' olduğu üçün:
  // TypeError: Cannot read properties of undefined (reading 'length')
  ```

---

### 🛡️ `null` və `undefined`-dən Qorunmaq (Safe Navigation)

Obyektlərdə xüsusiyyətlərə daxil olarkən `null` və ya `undefined` ilə qarşılaşma ehtimalını nəzərə almaq və buna qarşı müdafiə mexanizmləri qurmaq vacibdir, yoxsa `TypeError` ilə tez-tez rastlaşacayıq.

Bu cür halların öhdəsindən gəlmək üçün ən çox istifadə olunan üsullar:

* **Uzun (Verbose) Üsul:** Hər bir addımda varlığı yoxlamaq. Bu, kodun oxunaqlığını azalda bilər.

    ```javascript
    let surname = undefined;
    if (book) { // book obyektinin mövcudluğunu yoxla
      if (book.author) { // book.author obyektinin mövcudluğunu yoxla
        surname = book.author.surname; // Yalnız sonra surname-i əldə et
      }
    }
    console.log(surname); // undefined (əgər book.author yoxdursa)
    ```

* **Qısa  Üsul (`&&` logical AND operatoru ilə):** Bu üsul JavaScript-də çox məşhurdur və "short-circuiting" prinsipindən istifadə edir.

    ```javascript
    // book, book.author, book.author.surname ardıcıllığını yoxlayır.
    // Əgər bunlardan hər hansı biri 'falsy' (null, undefined, 0, "", false) olarsa,
    // dəyəri qaytarır və sonrakı yoxlamanı dayandırır (short-circuiting).
    let surname = book && book.author && book.author.surname;
    console.log(surname); // undefined (əgər book.author yoxdursa)
    ```
    Burada `&&` operatoru əgər `book` və ya `book.author` `undefined` (və ya `null`) olarsa, növbəti ifadəyə keçmir və `undefined` qaytarır. Bu, `TypeError`-dən qorunmağın effektiv yoludur.

---

### 🚀 ES2020: Optional Chaining Operatoru `?.`

ECMAScript 2020 (ES2020) ilə gələn ən faydalı sintaksislərdən biri **optional chaining operatoru (`?.`)**-dur. Bu operator, mürəkkəb obyekt strukturlarında iç-içə xüsusiyyətlərə daxil olarkən kodun daha qısa, daha oxunaqlı və xətalara qarşı daha davamlı olmasını təmin edir:

```javascript
let book = {
  title: "JavaScript",
  author: {
    firstname: "John",
    // surname: "Doe" //surname xüsusiyyəti yoxdur
  }
};

let authorSurname = book?.author?.surname; 
// book?.author?.surname - author və ya surname yoxdursa undefined qaytar
console.log(authorSurname); // Nəticə: undefined

// Massivlər və funksiyalarla da işləyir:
let users = [{ name: "Rashad" }, { name: "Ayan" }];
let firstUserName = users?.[0]?.name; 
// Massivin 0-cı elementi və onun adı
console.log(firstUserName); // "Rashad"

let greet = {
  sayHello: () => "Hello!"
};
let message = greet?.sayHello?.(); // sayHello metodu varsa çağır
console.log(message); // "Hello!"

let noGreet;
let noMessage = noGreet?.sayHello?.(); // noGreet yoxdursa çağırılmaz
console.log(noMessage); // undefined
```

---

### ❌ Xüsusiyyət Təyin Edərkən Yaranan Xətalar (Setting Properties Errors)

Property təyin etməyə çalışarkən də bəzi xətalarla qarşılaşa bilərik:

* **`null` və ya `undefined` dəyərinə property təyin etmək:** Təbii ki, `null` və `undefined` primitiv dəyərlər olduğu üçün onlara yeni xüsusiyyət əlavə etmək və ya mövcudunu dəyişmək mümkün deyil. Buna cəhd etsən, **TypeError** alınır.

    ```javascript
    let obj = null;
    obj.x = 10; 
    // TypeError: Cannot set properties of null (setting 'x')
    ```

* **Read-only (yalnız oxuna bilən) xüsusiyyətlər:** Obyektin bəzi xüsusiyyətləri (`writable` atributu `false` olaraq təyin edildiyi üçün) dəyişdirilə bilməz. Belə bir xüsusiyyətə dəyər təyin etməyə çalışsan, **strict mode**-da **TypeError** atılır. Non-strict mode-da isə dəyişiklik sadəcə **uğursuz olur** (silent failure) və xəta atılmır.

    ```javascript
    let constantObject = {};
    Object.defineProperty(constantObject, 'PI', { value: 3.14, writable: false });
    // constantObject.PI = 3.14159; 
    // Strict mode-da TypeError, Non-strict mode-da dəyişmir (silent failure)
    ```

## Non-extensible (genişlənməyən) obyektlər:

JavaScript-də bəzi obyektlərə yeni xüsusiyyətlər əlavə etmək qadağan edilə bilər. Bu, obyektin strukturunu qorumaq və gözlənilməz dəyişikliklərin qarşısını almaq üçün faydalıdır. JavaScript bizə obyektlərin genişlənmə qabiliyyətini idarə etmək üçün üç əsas metod təklif edir: `Object.preventExtensions()`, `Object.seal()`, və `Object.freeze()`

---

#### `Object.preventExtensions()`

Bu metod obyektə yeni xüsusiyyətlər əlavə edilməsinin qarşısını alır. Lakin, mövcud xüsusiyyətləri dəyişdirmək və ya silmək hələ də mümkündür.

  * **Yeni xüsusiyyətlər əlavə etmək:** Qadağandır.
  * **Mövcud xüsusiyyətləri dəyişdirmək:** İcazəlidir.
  * **Mövcud xüsusiyyətləri silmək:** İcazəlidir.

**Nümunə:**

```javascript
let myObject = { a: 1 };

Object.preventExtensions(myObject); 
// myObject-a yeni xüsusiyyət əlavə etməyi qadağan edir

console.log(Object.isExtensible(myObject)); 
// Output: false (genişlənməyən)

myObject.b = 2; // Yeni xüsusiyyət əlavə etməyə cəhd
console.log(myObject); // Output: { a: 1 } (b əlavə olunmadı)

myObject.a = 10; // Mövcud xüsusiyyəti dəyişdirmək
console.log(myObject); // Output: { a: 10 } (dəyişiklik uğurlu oldu)

delete myObject.a; // Mövcud xüsusiyyəti silmək
console.log(myObject); // Output: {} (silinmə uğurlu oldu)

// Strict mode-da yeni xüsusiyyət əlavə etməyə cəhd TypeError atar:
// "use strict";
let strictObj = {};
Object.preventExtensions(strictObj);
strictObj.newProp = "value"; 
// TypeError: Cannot add property newProp, object is not extensible
```

-----

#### `Object.seal()`

`Object.seal()` metodu `Object.preventExtensions()` funksionallığını özündə birləşdirir və əlavə olaraq obyektin mövcud xüsusiyyətlərinin silinməsinin qarşısını alır. Lakin, mövcud xüsusiyyətlərin dəyərlərini dəyişdirmək hələ də mümkündür (əgər onlar `writable` deyillərsə).

  * **Yeni xüsusiyyətlər əlavə etmək:** Qadağandır.
  * **Mövcud xüsusiyyətləri dəyişdirmək:** İcazəlidir (xüsusiyyət `writable` olduğu müddətcə).
  * **Mövcud xüsusiyyətləri silmək:** Qadağandır.

**Nümunə:**

```javascript
let sealedObject = { name: "JavaScript", version: "ES6" };

Object.seal(sealedObject); 
// Obyektə yeni xüsusiyyətlər əlavə etməyi qadağan edir və mövcudları silməyə icazə vermir.

console.log(Object.isExtensible(sealedObject)); // Output: false
console.log(Object.isSealed(sealedObject));   // Output: true

sealedObject.author = "Rashad Garayev"; 
// Yeni xüsusiyyət əlavə etməyə cəhd
console.log(sealedObject); 
// Output: { name: "JavaScript", version: "ES6" } (author əlavə olunmadı)

sealedObject.version = "ES2024"; 
// Mövcud xüsusiyyəti dəyişdirmək
console.log(sealedObject); 
// Output: { name: "JavaScript", version: "ES2024" } (dəyişiklik uğurlu oldu)

delete sealedObject.name; 
// Mövcud xüsusiyyəti silməyə cəhd
console.log(sealedObject); 
// Output: { name: "JavaScript", version: "ES2024" } (silinmə uğursuz oldu)

// Strict mode-da yeni xüsusiyyət əlavə etməyə cəhd TypeError atar:
"use strict";
let strictSealedObj = {};
Object.seal(strictSealedObj);
strictSealedObj.newProp = "value"; // TypeError
```

-----

#### `Object.freeze()`

`Object.freeze()` ən sərt metodur. Obyekti `seal` edir və əlavə olaraq bütün mövcud xüsusiyyətləri dondurur, yəni onların dəyərlərini dəyişdirmək və ya atributlarını konfiqurasiya etmək qadağan olur. Dondurulmuş obyektlər tamamilə dəyişməzdir (shallow freeze).

  * **Yeni xüsusiyyətlər əlavə etmək:** Qadağandır.
  * **Mövcud xüsusiyyətləri dəyişdirmək:** Qadağandır.
  * **Mövcud xüsusiyyətləri silmək:** Qadağandır.

**Nümunə:**

```javascript
let frozenObject = { constant: 100, message: "Hello" };

Object.freeze(frozenObject); // Obyekti tamamilə dondurur

console.log(Object.isExtensible(frozenObject)); // Output: false
console.log(Object.isSealed(frozenObject));   // Output: true
console.log(Object.isFrozen(frozenObject));   // Output: true

frozenObject.newProp = "error"; 
// Yeni xüsusiyyət əlavə etməyə cəhd
console.log(frozenObject); 
// Output: { constant: 100, message: "Hello" }

frozenObject.constant = 200; 
// Mövcud xüsusiyyəti dəyişdirməyə cəhd
console.log(frozenObject); 
// Output: { constant: 100, message: "Hello" }

delete frozenObject.message; 
// Mövcud xüsusiyyəti silməyə cəhd
console.log(frozenObject); 
// Output: { constant: 100, message: "Hello" }

// Strict mode-da yuxarıdakı dəyişiklik cəhdləri (yeni əlavə etmək, mövcudu dəyişmək/silmək) TypeError atar:
"use strict";
let strictFrozenObj = { x: 1 };
Object.freeze(strictFrozenObj);
strictFrozenObj.x = 2; // TypeError: Cannot assign to read only property 'x'
strictFrozenObj.y = 3; // TypeError: Cannot add property y, object is not extensible
```


**Qeyd:** Yuxarıdakı bütün hallarda, `strict mode`-da yuxarıda qeyd olunan qadağalara əməl edilmədikdə **`TypeError`** atılır. `Non-strict mode`-da isə bu cəhdlər sadəcə **uğursuz olur** (silent failure), yəni heç bir səhv mesajı verilmədən dəyişiklik baş vermir. Bu səbəbdən, kodunuzda gözlənilməz davranışların qarşısını almaq üçün həmişə **`strict mode`** istifadə etmək tövsiyə olunur.

---


# 6.4 Xüsusiyyətləri Silmək (Deleting Properties) 

Obyektin xüsusiyyətlərini silmək üçün **`delete` operatorundan** istifadə olunur. Burada vacib məqam budur ki, `delete` operatoru xüsusiyyətin **dəyərinə deyil, xüsusiyyətin özünə** (yəni, açar-dəyər cütünə) təsir edir və onu obyektdən tamamilə qaldırır.

```javascript
let book = {
  title: "JavaScript",
  author: "Rashad Garayev",
  year: 2025
};

console.log(book);         
// { title: "JavaScript", author: "Rashad Garayev", year: 2025 }
delete book.author;        
// book obyektinin "author" xüsusiyyəti silinir
console.log(book);         
// { title: "JavaScript", year: 2025 }

delete book["year"];       
// "year" xüsusiyyəti də silinir (bracket notation ilə də işləyir)
console.log(book);         
// { title: "JavaScript" }
```

---

### ️🧩 `delete` operatoru və İrsi Xüsusiyyətlər (Inherited Properties)

`delete` operatoru yalnız obyektin **özünəməxsus xüsusiyyətlərini (own properties)** silir. O, **irsi (inherited) xüsusiyyətləri silmir**.

Əgər sən bir obyekt vasitəsilə miras alınan bir xüsusiyyəti silməyə cəhd etsən, `delete` operatoru həmin əməliyyatı yerinə yetirməyəcək, çünki xüsusiyyət obyektin özündə deyil, onun prototip zəncirində yerləşir. İrsi xüsusiyyətin silinməsi üçün onu prototip obyektindən silmək lazımdır. Bu isə həmin prototipdən miras alan bütün obyektlərə təsir edəcək.

```javascript
let protoObj = { x: 1 };
let myObj = Object.create(protoObj);
myObj.y = 2;

console.log(myObj.x); // 1 (irsən gəlir)
console.log(myObj.y); // 2 (own property)

delete myObj.x; // false və ya true qaytara bilər, lakin myObj.x dəyəri dəyişməz qalır
// Xüsusiyyət silinmədi, çünki o, myObj-nin öz xüsusiyyəti deyil, prototipdədir.
console.log(myObj.x); // Hələ də 1

delete myObj.y; // true (own property silindi)
console.log(myObj.y); // undefined
```

---

### ✔️ `delete` operatorunun nəticəsi (Return Value)

`delete` ifadəsi həmişə bir boolean dəyər qaytarır:

* Əgər silmək mümkün olduqda (və ya silinəcək property mövcud deyilsə), **`true`** qaytarır.
* Əgər silmə əməliyyatı qadağan olunarsa (məsələn, `non-configurable` xüsusiyyət silinməyə çalışılırsa) və strict mode-da deyilsə, **`false`** qaytarır. Strict mode-da isə bu hallarda xəta atılır.

Misal:

```javascript
let o = { x: 1, y: 2 };

console.log(delete o.x);      // Nəticə: true (o.x silindi)
console.log(o);               // { y: 2 }

console.log(delete o.x);      
// Nəticə: true (o.x artıq yoxdur, amma yenə də true qaytarır)

console.log(delete o.toString); 
// Nəticə: true (toString irsi xüsusiyyətdir, silmir, amma true qaytarır.

console.log(delete 1);        
// Nəticə: true (mənasız olsa da, sintaktik cəhətdən keçərlidir)
```

---

### ❌ `delete` və `non-configurable` Xüsusiyyətlər

`delete` operatoru **`non-configurable` (silinməz)** olaraq təyin edilmiş xüsusiyyətləri silə bilməz.

* **Built-in (daxili) obyektlərin bəzi xüsusiyyətləri** standart olaraq `non-configurable` olur (məsələn, `Object.prototype`-dakı metodlar).
* Brauzerlərdə və ya Node.js-də qlobal obyektin (`globalThis` və ya `window`/`global`) birbaşa elan edilmiş **`var` açar sözü ilə yaradılmış qlobal dəyişənlər** və **funksiya deklarasiyaları** da adətən `non-configurable` olurlar.

**Strict mode**-da belə bir `non-configurable` xüsusiyyəti silməyə cəhd **`TypeError`** ilə nəticələnir. Non-strict mode-da isə `delete` sadəcə **`false`** qaytarır və heç bir xəta atmır (silent failure).

```javascript
// Strict mode-da:
"use strict";
delete Object.prototype; 
// TypeError: Cannot delete property 'prototype' of function Object()
var x = 1;
delete x; 
// SyntaxError: Delete of an unqualified identifier in strict mode.
// Bu səhv 'delete globalThis.x;' yazmağın vacibliyinə işarə edir.

// Non-strict mode-da:
console.log(delete Object.prototype);    
// false
var x = 1;
console.log(delete x); 
// false (browserdə və ya Node.js-də qlobal skopda `var` ilə elan olunduğu üçün)
function f() {}
console.log(delete f); 
// false (qlobal skopda funksiya deklarasiyası olduğu üçün)
```

---

### ⚠️ Qlobal Obyekt Property-lərini Silmək və Strict Mode Fərqi

Non-strict mode-da qlobal obyektin konfiqurasiya edilə bilən xüsusiyyətlərini sadəcə `delete myGlobalVar;` kimi silmək mümkün ola bilər.

Lakin **Strict mode**-da, ixtisaslaşdırılmamış identifikatorların (`unqualified identifiers`) `delete` operatoru ilə silinməsi **`SyntaxError`** verir. Bu o deməkdir ki, qlobal dəyişəni silmək istəyirsənsə, ona qlobal obyekt (məsələn, `window` brauzerdə, `global` Node.js-də, və ya `globalThis` hər ikisində) vasitəsilə daxil olmalısan:

```javascript
"use strict";

var myGlobalVar = "Hello";
delete myGlobalVar; 
// SyntaxError: Delete of an unqualified identifier in strict mode.

delete globalThis.myGlobalVar; 
// ✅ Strict mode-da düzgün yazılışdır.
console.log(myGlobalVar); // undefined

// const və let ilə elan edilmiş qlobal dəyişənlər heç vaxt silinə bilməz.
delete globalThis.myConst; 
// Həmişə false və ya TypeError (qlobal obyektdə property kimi yoxdur).
```

---
# 6.5 Xüsusiyyətləri Yoxlamaq (Testing Properties)

Obyektin müəyyən bir **xüsusiyyətə (property)** malik olub-olmadığını yoxlamaq çox vacibdir. Bu, kodumuzun daha etibarlı işləməsinə kömək edir. Bunun üçün bir neçə üsul var.

---

### 1️⃣ `in` operatoru ❓

`in` operatoru bir xüsusiyyətin obyektin **özündə (own property)** və ya onun **prototip zəncirində (inherited property)** mövcud olub-olmadığını yoxlayır.

* **Sintaksis:** `"xüsusiyyət_adı" in obyekt`
* **Nəticə:** Mövcuddursa `true`, yoxdursa `false` qaytarır.

```javascript
let o = { x: 1 }; // 'o' obyektinin öz xüsusiyyəti 'x'

"x" in o;         // => true  ('x' 'o'-nun öz xüsusiyyətidir)
"y" in o;         // => false ('y' 'o'-da yoxdur)
"toString" in o;  // => true  ('toString' 'Object.prototype'-dən miras alınıb)
```

---

### 2️⃣ `hasOwnProperty()` metodu 🔑

Bu metod obyektin **yalnız özünə aid (own property)** xüsusiyyətləri yoxlayır. Miras alınan xüsusiyyətlər üçün `false` qaytarır.

* **Sintaksis:** `obyekt.hasOwnProperty("xüsusiyyət_adı")`
* **Nəticə:** Öz xüsusiyyətidirsə `true`, yoxdursa və ya miras alınıbsa `false` qaytarır.

```javascript
let o = { x: 1 };

o.hasOwnProperty("x");        // => true  ('x' 'o'-nun öz xüsusiyyətidir)
o.hasOwnProperty("y");        // => false ('y' 'o'-da yoxdur)
o.hasOwnProperty("toString"); 
// => false ('toString' miras alınıb, 'o'-nun öz xüsusiyyəti deyil)
```
Bu metod `for/in` dövrü ilə birlikdə yalnız obyektin öz xüsusiyyətlərini gəzmək üçün çox faydalıdır:

```javascript
const user = {
  name: "Rəşad",
  age: 25
};

// Prototipə əlavə olunur
Object.prototype.extra = "Bu mirasdır";

for (let key in user) {
  if (user.hasOwnProperty(key)) {
    console.log(`${key} → ${user[key]}`);
  }
}
/* 
name → Rəşad
age → 25
*/
```

Əgər `hasOwnProperty()` istifadə etməsəydik, bu da çıxacaqdı:

```
extra → Bu mirasdır
```

---

### 3️⃣ `propertyIsEnumerable()` metodu 📝

Bu metod yoxlayır ki, bir xüsusiyyət:

1. Obyektin **özünə aiddirmi** (yəni miras alınıb yoxsa yox)?
2. Və **gəzilə biləndirmi** (`enumerable: true`)?

**Sadə dillə:** Əgər xüsusiyyət **öz obyektindədirsə** və **gizlənməyibsə**, bu metod `true` qaytarır.

---

### Sintaksis:

```javascript
obyekt.propertyIsEnumerable("xüsusiyyət_adı")
```

---

### Praktik və Aydın Nümunə:

Təsəvvür et ki, çantan var. İçində 2 görünən, 1 gizli əşya var:

```javascript
const bag = {
  pen: "🖊️",
  notebook: "📓"
};

// Gizli məktub əlavə edirik
Object.defineProperty(bag, "secretLetter", {
  value: "📜",
  enumerable: false // gizli et!
});
```

İndi yoxlayaq hansı görünür:

```javascript
console.log(Object.keys(bag)); 
// 👉 ["pen", "notebook"] → `secretLetter` görünmür!

console.log(bag.propertyIsEnumerable("pen")); 
// ✅ true → çantada görünür

console.log(bag.propertyIsEnumerable("secretLetter")); 
// ❌ false → çantada gizlənib, görünmür
```

---

### 4️⃣ Xüsusiyyət dəyərini yoxlama (`!== undefined`) 🧐

Bu üsul, xüsusiyyətin dəyərinin `undefined` olub-olmadığını yoxlayır. **Lakin bu, xüsusiyyətin mövcudluğunu yoxlamaq üçün hər zaman etibarlı deyil**, çünki bir xüsusiyyətin dəyəri elə `undefined` ola bilər.

* **Sintaksis:** `obyekt.xüsusiyyət_adı !== undefined`
* **Nəticə:** Dəyəri `undefined`-dan fərqlidirsə `true`, `undefined` və ya xüsusiyyət yoxdursa `false` qaytarır.

```javascript
let o = { x: undefined, y: 2 }; 
// 'x' property'si var, amma dəyəri undefined

o.x !== undefined;       
// => false (xüsusiyyət var, amma dəyəri undefined-dır)
o.y !== undefined;       
// => true  (xüsusiyyət var və dəyəri undefined deyil)
o.z !== undefined;       
s// => false (xüsusiyyət mövcud deyil)

// Müqayisə üçün 'in' operatoru ilə:
"x" in o;                // => true  ('x' xüsusiyyəti mövcuddur)
"z" in o;                // => false ('z' xüsusiyyəti mövcud deyil)
```
Bu səbəbdən, xüsusiyyətin **mövcudluğunu** yoxlamaq üçün `in` operatoru və ya `hasOwnProperty()` metodları daha dəqiqdir.

---

### 5️⃣ `Object.keys()`, `Object.getOwnPropertyNames()`, `Object.getOwnPropertySymbols()` 📋

Bu metodlar obyektin **özünəməxsus xüsusiyyətlərinin adlarını** (yaxud Symbol-larını) massiv şəklində qaytarır. Sonra bu massivdə axtardığın xüsusiyyət adının olub-olmadığını yoxlaya bilərsən.

* **`Object.keys(o)`**: Obyektin **özünə aid olan və enumerable** xüsusiyyətlərinin adlarını qaytarır.
    ```javascript
    let o = { a: 1, b: 2 };
    Object.keys(o); // => ["a", "b"]
    ```
* **`Object.getOwnPropertyNames(o)`**: Obyektin **özünə aid olan** (enumerable olub-olmadığından asılı olmayaraq) xüsusiyyətlərinin adlarını qaytarır.
    ```javascript
    const user = { name: "Rəşad" };

    // Yeni xüsusiyyət əlavə edirik: 'password'
    Object.defineProperty(user, "password", {
    value: "123456",
    enumerable: false // gizli saxla!
    });

    console.log(Object.keys(user));
    //  ["name"] → 'password' gizlidir, görünmür

    console.log(Object.getOwnPropertyNames(user));
    //  ["name", "password"] → hər ikisi öz xüsusiyyətidir, görünür

    console.log(user.propertyIsEnumerable("password"));
    //  false → gizlidir, for...in və Object.keys ilə gəlməyəcək
    ```
    
* **`Object.getOwnPropertySymbols(o)`**: Obyektin **özünə aid olan Symbol** xüsusiyyətlərini qaytarır.
    ```javascript
    const mySymbol = Symbol("id");
    let o = { [mySymbol]: 1, name: "Test" };
    Object.getOwnPropertySymbols(o); // => [Symbol(id)]
    ```
Bu metodlarla qaytarılan massivdə `includes()` metodunu istifadə edərək xüsusiyyətin mövcudluğunu yoxlaya bilərsən:
```javascript
const o = { name: "Alice", age: 30 };
Object.keys(o).includes("name"); // => true
Object.keys(o).includes("email"); // => false
```

---

### 🚩 Qeyd

* **`in` operatoru** xüsusiyyətin mövcudluğunu dəyərindən asılı olmayaraq yoxlamaq üçün ən doğru seçimdir (öz və ya miras alınmış).
* **`hasOwnProperty()`** yalnız obyektin öz xüsusiyyətlərini nəzərə almaq lazım gəldikdə vacibdir.

---

# 6.6 Xüsusiyyətləri Siyahıya Almaq (Enumerating Properties)

JavaScript-də bəzən obyektin **tək bir xüsusiyyətini yox, bütün xüsusiyyətlərini (properties)** siyahı şəklində almaq və ya onlar üzərində dövr etmək (iterate) lazım olur. Bunun üçün müxtəlif üsullar mövcuddur.

---

### 1️⃣ `for/in` dövrü 🔄

`for/in` dövrü obyektin **bütün `enumerable` xüsusiyyətləri** (həm öz, həm də miras alınan) üzərində dövr edir.

* **`Enumerable` xüsusiyyətlər:** JavaScript tərəfindən normal kodla yaradılan xüsusiyyətlər (məsələn, `obj.x = 1`) **default olaraq `enumerable`** olurlar.
* **`Non-enumerable` xüsusiyyətlər:** Daxili (built-in) metodlar (məsələn, `toString()`) və ya `Object.defineProperty()` ilə xüsusi olaraq `enumerable: false` təyin edilmiş xüsusiyyətlər `non-enumerable` olurlar və `for/in` tərəfindən siyahıya alınmırlar.

```javascript
let o = { a: 1, b: 2, c: 3 };

// toString metodu Object.prototype-dədir və non-enumerable-dir:
console.log(o.propertyIsEnumerable("toString")); // => false

for (let p in o) {
  console.log(p); 
  // Output: a, b, c (toString kimi miras alınan non-enumerable-lər göstərilməz)
}
```

---

### 2️⃣ `for/in` ilə miras alınan xüsusiyyətlərdən qorunmaq 🛡️

`for/in` dövrü həm obyektin öz xüsusiyyətlərini, həm də miras aldığı xüsusiyyətləri siyahıya alır. Bəzən bizə yalnız obyektin **öz xüsusiyyətləri** lazım olur. Bu halda `hasOwnProperty()` metodundan istifadə edirik:

```javascript
let proto = { p: 1 };
let o = Object.create(proto);
o.x = 2; // 'o'-nun öz xüsusiyyəti
o.y = 3; // 'o'-nun öz xüsusiyyəti

for (let prop in o) {
  if (!o.hasOwnProperty(prop)) continue;  // Miras alınan xüsusiyyətləri ötür
  console.log(prop); // Output: x, y (p göstərilməz)
}
```

---

### 3️⃣ Alternativ Yanaşma: `Object.keys()` + `for/of` 🎯

`for...in` dövrünün **təhlükəsiz alternativi**, obyektin öz xüsusiyyətlərini əvvəlcə massiv kimi almaq, sonra üzərindən `for...of` ilə keçməkdir.

---

###  1. `Object.keys(obj)` → `enumerable` öz property-ləri (string)

```javascript
const user = { name: "Ayan", age: 22 };

for (let key of Object.keys(user)) {
  console.log(`${key}: ${user[key]}`);
}
// ✅ name: Ayan
// ✅ age: 22
```

---

###  2. `Object.getOwnPropertyNames(obj)` → **bütün string adlar** (görünən + gizli)

```javascript
const user = {};
Object.defineProperty(user, "hidden", {
  value: "gizli",
  enumerable: false
});

console.log(Object.getOwnPropertyNames(user));
// ✅ ["hidden"]
```

---

### 3. `Object.getOwnPropertySymbols(obj)` → `Symbol` property-ləri

```javascript
const id = Symbol("id");
const user = { [id]: 42 };

console.log(Object.getOwnPropertySymbols(user));
// ✅ [ Symbol(id) ]
```

---

###  4. `Reflect.ownKeys(obj)` → **hər şeyi qaytarır** (string + symbol, gizli və görünən)

```javascript
const id = Symbol("id");
const user = { name: "Leyla", [id]: 99 };

Object.defineProperty(user, "secret", {
  value: "🙊",
  enumerable: false
});

console.log(Reflect.ownKeys(user));
// ✅ ["name", "secret", Symbol(id)]
```

---

##  6.6.1 Xüsusiyyətlərin Siyahıya Alınma Qaydası (Enumeration Order)

### 📋 JavaScript-də obyektin property-ləri necə sıralanır?

ES6 standartına görə 3 mərhələli sıralama var:

---

### ✅ 1. **Rəqəm kimi görünən stringlər** → Kiçikdən böyüyə

```javascript
const obj1 = {
  "2": "iki",
  "10": "on",
  "1": "bir",
};

console.log(Object.keys(obj1)); 
// ✅ ["1", "2", "10"]
```

> Rəqəm kimi görünən stringlər (`"1"`, `"2"`, `"10"`) əvvəlcə **rəqəm kimi** sıralanır.

---

### ✅ 2. **Adi string adlar** → Əlavə olunma sırasına görə

```javascript
const obj2 = {
  banana: "🍌",
  apple: "🍎",
  cherry: "🍒"
};

console.log(Object.keys(obj2)); 
// ✅ ["banana", "apple", "cherry"]
```

> Rəqəm olmayan string adlar **əlavə olunma sırasına** görə gedir.

---

### ✅ 3. **Symbol xüsusiyyətlər** → Əlavə olunma sırasına görə, ən sonda

```javascript
const sym1 = Symbol("a");
const sym2 = Symbol("b");

const obj3 = {
  x: 1,
  [sym1]: "💡",
  y: 2,
  [sym2]: "🔐"
};

console.log(Reflect.ownKeys(obj3)); 
// ✅ ["x", "y", Symbol(a), Symbol(b)]
```

> `Symbol` adları **ən sonda** gəlir — həm `Object.getOwnPropertySymbols()` ilə, həm də `Reflect.ownKeys()` ilə görünür.

---

### ⚠️ `for...in` Dövründə Qaydalar

```javascript
const base = { inherited: "🧬" };
const obj = Object.create(base);
obj["3"] = "üç";
obj["1"] = "bir";
obj["z"] = "z hərfi";

for (let key in obj) {
  console.log(key);
}
// "1", "3", "z", "inherited"
```

---

# 6.7 Obyektləri Genişləndirmək (Extending Objects)

JavaScript-də tez-tez bir obyektin xüsusiyyətlərini (properties) başqa bir obyektə **kopyalamaq** lazım gəlir. Bu, obyektləri birləşdirmək və ya default dəyərləri təyin etmək üçün istifadə olunur.

---

### 1️⃣ `Object.assign()` → Obyektləri birləşdirmək üçün ⚙️

`Object.assign()` ES6 ilə gəldi və bir neçə obyektin **xüsusiyyətlərini birləşdirmək/kopyalamaq** üçün istifadə olunur.

---

### 📌 Sintaksis:

```javascript
Object.assign(target, ...sources)
```

* `target`: Nəticə bu obyektə yazılır (birbaşa dəyişir).
* `sources`: Kopyalanacaq obyektlər. Əvvəlkiləri sonrakılar "əvəz edə" bilər.

---

### 🔍 Nümunə:

```javascript
const target = { x: 1 };
const source1 = { y: 2, z: 3 };
const source2 = { z: 4, a: 5 };

Object.assign(target, source1, source2);
console.log(target); 
// ✅ { x: 1, y: 2, z: 4, a: 5 }
```

---

### 🧠 Nə baş verdi?

* `source1` → `y` və `z` əlavə olundu.
* `source2` → `z`-ni yenidən **əvəz etdi**, `a` əlavə olundu.
* `target` dəyişdirildi və nəticə **özünə** yazıldı.

---

### ⚠️ Diqqət:

* Dərin kopyalama (deep copy) **etmir**. Yalnız **səthi (shallow)** kopyalayır.

```javascript
const obj1 = { nested: { val: 1 } };
const obj2 = Object.assign({}, obj1);

obj2.nested.val = 99;
console.log(obj1.nested.val); 
// ❗️ 99 → hər ikisi eyni "nested" obyektə baxır!
```


---

### 2️⃣ Default Dəyərlər və Üstələmə (Overrides) 🎯

Bəzən istifadəçidən gələn ayarlar var (`userSettings`), bəzən isə proqramın öz təyin etdiyi **default dəyərlər** (`defaults`). Biz istəyirik ki:

✅ `userSettings` varsa → onu götürsün
✅ Yoxdursa → `defaults`-dan istifadə olunsun

---

### ❌ Səhv Yanaşma:

```javascript
Object.assign(userSettings, defaults); 
// defaults → userSettings-in üstündən yazır!
```

---

### ✅ Düzgün Yanaşma:

```javascript
const defaults = { color: "red", size: "medium" };
const userSettings = { size: "large", font: "Arial" };

// Əvvəl defaults → boş obyektə kopyalanır,
// sonra userSettings → onun üstünə yazılır
const final = Object.assign({}, defaults, userSettings);

console.log(final); 
// ✅ { color: "red", size: "large", font: "Arial" }
```

---

### İzah:

```text
{}                        // Boş obyekt (müdaxilə etməmək üçün)
↓
+ defaults               // { color: "red", size: "medium" }
↓
+ userSettings           // { size: "large", font: "Arial" }
↓
= final result           // { color: "red", size: "large", font: "Arial" }
```

---


### 4️⃣ Spread Operatoru (`...`) ilə Oxşar Əməliyyat 

**ES6 (ES2015)** ilə gələn **spread operatoru (`...`)** obyektləri birləşdirmək üçün daha qısa və oxunaqlı bir sintaksis təklif edir. Bu, `Object.assign()` ilə eyni şəkildə işləyir:

```javascript
let defaults = { color: "red", size: "medium" };
let userSettings = { size: "large", font: "Arial" };

// Spread operatoru ilə:
let finalSettings = { ...defaults, ...userSettings };
console.log(finalSettings); 
// => { color: "red", size: "large", font: "Arial" }
```

---

### 5️⃣ Öz `merge()` Funksiyamız – Yalnız Olmayanları Əlavə Et 🛠️

**Məqsəd:** `Object.assign()` kimi *üstələməsin*. Əksinə, **target-də olmayanları əlavə etsin**, olanlara **toxunmasın**.

---

### 🧠 Niyə Lazımdır?

```javascript
Object.assign(target, source);
// source target-dəki dəyərləri üstələyir (overwrite)
```

Amma bəzən:

* İstəyirik `target` dəyişməsin.
* Yalnız `source`-dakı **yeni açarlar** əlavə olunsun.

---

### ✅ Öz `merge()` funksiyamız:

```javascript
function merge(target, ...sources) {
  for (const source of sources) {
    for (const key of Object.keys(source)) {
      if (!(key in target)) {
        target[key] = source[key];
      }
    }
  }
  return target;
}
```

---

### 🎯 Nümunə:

```javascript
let defaults = { theme: "light", fontSize: 14 };
let user1 = { fontSize: 18 }; // yalnız font dəyişib
let user2 = { layout: "grid" }; // əlavə ayar

merge(user1, defaults, user2);

console.log(user1);
// ✅ { fontSize: 18, theme: "light", layout: "grid" }
// Qeyd: 'fontSize' dəyişməyib, çünki artıq var idi
```

---

###  Fərq nədir?

| Funksiya            | Davranış                                                |
| ------------------- | ------------------------------------------------------- |
| `Object.assign()`   | Hər şeyi üstələyir (mövcud açarlar da dəyişir)          |
| `merge()` | Mövcud olanları saxlayır, yalnız olmayanları əlavə edir |

---

### Qısa Yekun 📝

* Obyektdən obyektə xüsusiyyətləri kopyalamaq üçün **`Object.assign()`** ən standart yoldur.
* **Spread operatoru (`...`)** daha müasir və oxunaqlı bir alternativ təqdim edir.
* Əgər mövcud dəyərləri qoruyaraq yalnız əskik xüsusiyyətləri əlavə etmək istəyirsənsə, öz **`merge()`** funksiyasını yaza bilərsən.
---

# 6.8 Obyektləri Serializasiya Etmək (Serializing Objects) 📦➡️📝

### Serializasiya Nədir?

**Serializasiya** obyektin (və ya başqa bir məlumatın) strukturunu və dəyərlərini, onu **mətndən ibarət bir formaya (string)** çevirmək prosesidir. Bunun məqsədi məlumatı saxlamaq (faylda, verilənlər bazasında) və ya şəbəkə üzərindən başqa bir sistemə göndərməkdir. Sonra bu string yenidən orijinal obyektə çevrilə bilər.

JavaScript-də bu məqsədlə iki əsas funksiya istifadə olunur:

* **`JSON.stringify()`**: JavaScript obyektini **JSON formatlı stringə** çevirir.
* **`JSON.parse()`**: JSON formatlı stringi yenidən **JavaScript obyektinə** çevirir.

---

### JSON Nədir? 📜

**JSON (JavaScript Object Notation)**, yəni "JavaScript Obyekt Notasiyası", yüngül bir məlumat mübadiləsi formatıdır. Adından da göründüyü kimi, JavaScript obyekt və massiv (array) sintaksisinə çox oxşardır, lakin daha sadə və Universaldır (bir çox proqramlaşdırma dili tərəfindən dəstəklənir).

**Misal:**

```javascript
let user = { name: "Ayan", age: 25 };

// 1️⃣ Obyekti JSON stringə çevir:
let jsonString = JSON.stringify(user);
console.log(jsonString);
// 👉 Nəticə: '{"name":"Ayan","age":25}'

 // 2️⃣ JSON stringi yenidən obyektə çevir:
let parsedUser = JSON.parse(jsonString);
console.log(parsedUser);
// 👉 Nəticə: { name: "Ayan", age: 25 }
```

---

### JSON-in Dəstəklədiyi Dəyərlər ✔️❌

JSON formatı yalnız müəyyən tipli dəyərləri dəstəkləyir:

* **Dəstəklənir:**
    * Obyektlər (JavaScript obyektləri)
    * Massivlər (Arrays)
    * Stringlər (Mətnlər)
    * Ədədlər (Numbers: tam ədədlər və onluqlar, amma `NaN`, `Infinity` istisna)
    * `true` (boolean)
    * `false` (boolean)
    * `null`

* **Xüsusi Hallar:**
    * `NaN` (Not a Number), `Infinity`, `-Infinity` dəyərləri serializasiya olunarkən JSON-da **`null`** kimi göstərilir.
    * **`Date` obyektləri** (tarix obyektləri) `JSON.stringify()` tərəfindən **ISO formatlı stringə** çevrilir (məsələn: `"2023-10-27T10:00:00.000Z"`). Lakin `JSON.parse()` həmin stringi **yenidən `Date` obyektinə çevirmir**, onu sadəcə bir string olaraq saxlayır. Tarix obyektini bərpa etmək üçün əlavə kod lazım olur.

* **Tam Dəstəklənməyənlər (Serializasiya Olunmayanlar):**
    * Funksiyalar (`function`)
    * `RegExp` (müntəzəm ifadələr)
    * `Error` obyektləri
    * `undefined`
    * `Symbol` dəyərləri
    * Dövrvari referanslar (circular references – obyektin özünə və ya özünün bir hissəsinə istinad etməsi)

    Bu tipli dəyərlərə malik xüsusiyyətlər `JSON.stringify()` tərəfindən **sadəcə stringdən çıxarılır** (silinir) və JSON stringinə daxil edilmir.

```javascript
JSON.stringify({ a: 5, b: undefined });
// 👉 Nəticə: '{"a":5}'  — `b` silinir

JSON.stringify({ a: NaN });
// 👉 Nəticə: '{"a":null}'

JSON.stringify({ a: () => 42 });
// 👉 Nəticə: '{}' — funksiyalar çıxarılır

JSON.stringify({ d: new Date() });
// 👉 Nəticə: '{"d":"2025-06-28T10:00:00.000Z"}'

JSON.stringify({ sym: Symbol("id") });
// 👉 Nəticə: '{}' — Symbol silinir
```        

---


###  İkinci Arqument — Serializasiyanı və Parsing-i Fərdiləşdirmək

Hem `JSON.stringify()` hem də `JSON.parse()` funksiyaları üçün ikinci arqument fərdiləşdirmə üçündür:

---

### ✅ `JSON.stringify(value, replacer, space)`

#### 1. `replacer` — Hansı sahələrin saxlanacağını və necə görünəcəyini təyin edir.

🔹 **Massiv kimi (`string[]`)**: Yalnız bu sahələr daxil edilir:

```javascript
let obj = { a: 1, b: 2, c: 3 };
let json = JSON.stringify(obj, ["a", "c"]);
console.log(json); // 👉 '{"a":1,"c":3}'
```

🔹 **Funksiya kimi**: Hər sahənin dəyərini dəyişmək mümkündür:

```javascript
let obj = { a: 1, b: 2 };
let json = JSON.stringify(obj, (key, value) => {
  return typeof value === "number" ? value * 10 : value;
});
console.log(json); // 👉 '{"a":10,"b":20}'
```

#### 2. `space` — JSON stringini gözəlləşdirir (indentasiya)

```javascript
let obj = { a: 1, b: { c: 2 } };
let json = JSON.stringify(obj, null, 2); // 2 boşluqla indent
console.log(json);
/*
{
  "a": 1,
  "b": {
    "c": 2
  }
}
*/
```

---

### ✅ `JSON.parse(text, reviver)`

#### `reviver` — JSON parse edilərkən sahələrin dəyərini dəyişmək imkanı verir.

Tutaq ki, `number` tipində olan dəyərləri **ikiqat artırmaq** istəyirik:

```javascript
let json = '{"a": 1, "b": 2, "c": "salam"}';

let obj = JSON.parse(json, (key, value) => {
  if (typeof value === "number") {
    return value * 2; // bütün rəqəmləri 2 ilə vur
  }
  return value;
});

console.log(obj); // 👉 { a: 2, b: 4, c: "salam" }

```


---

# 6.9 Obyekt Metodları (Object Methods) 🔧

JavaScript obyektlərində iki növ faydalı metod var: **universal metodlar** (obyektin prototipindən miras alınanlar) və **statik metodlar** (`Object` konstruktorunun özünə aid olanlar).

---

### Universal Metodlar (Prototype Metodları) 🌍

Bütün JavaScript obyektləri (yəni, `Object.prototype`-dən gələn prototip zənciri olanlar) avtomatik olaraq `Object.prototype`-də təyin edilmiş metodları miras alırlar. Bu metodlar hər bir obyekt üzərində istifadə edilə bilər.

**Misal:**

* **`hasOwnProperty()`**: Bu metod obyektin **özünə aid (own property)** müəyyən bir xüsusiyyətin olub-olmadığını yoxlayır. Miras alınan xüsusiyyətləri saymır.
* **`propertyIsEnumerable()`**: Bu metod isə xüsusiyyətin obyektin özünə aid olub-olmadığını və `for/in` dövrü kimi yerlərdə **siyahıya alına bilən (enumerable)** olub-olmadığını yoxlayır.

---

### Statik Metodlar (Object Konstruktorunun Metodları) 📚

`Object` konstruktorunun özündə də çoxlu faydalı metodlar var. Bu metodlar birbaşa `Object` üzərindən çağırılır və obyektin özünə aid deyil.

**Misal:**

* **`Object.create()`**: Yeni bir obyekt yaradır və onun prototipini təyin edir.
* **`Object.keys()`**: Bir obyektin **özünə aid olan və `enumerable`** xüsusiyyətlərinin adlarını massiv olaraq qaytarır.
* **`Object.assign()`**: Bir və ya bir neçə mənbə obyektindən xüsusiyyətləri hədəf obyektə kopyalayır.

---

## 6.9.1 `toString()` Metodu 🧾➡️🖋️

### `toString()` Nədir? 

`toString()` metodu JavaScript obyektlərinin ən əsas universal metodlarından biridir. O, **heç bir arqument qəbul etmir** və çağırıldığı obyektin **string (mətn) təmsilçiliyini** qaytarır. JavaScript çox vaxt obyektləri stringə çevirmək lazım gələndə bu metodu avtomatik olaraq çağırır.

**Misal üçün, belə hallar:**

* Bir stringlə obyekt birləşdiriləndə: `"Dəyər: " + myObject`
* String gözləyən bir funksiya və ya parametrə obyekt göndərəndə.

---

### Default `toString()` Necədir?

`Object.prototype`-dəki default `toString()` versiyası çox məlumat verici deyil. O, adətən obyektin "klass" adını bildirən bir string qaytarır:

```javascript
let o = { x: 1, y: 1 };
let s = o.toString();
console.log(s); 
// Nəticə: "[object Object]"
// Array üçün: [1,2,3].toString() => "1,2,3"
// Function üçün: (function(){}).toString() => "function(){}"
```

---

### Öz `toString()` Metodunu Yaratmaq (Override Etmək)

Çox vaxt default `toString()` metodu bizim üçün kifayət qədər faydalı olmur. Daha mənalı və oxunaqlı bir string təmsilçiliyi əldə etmək üçün **`toString()` metodunu obyektin özündə yenidən yaza (override edə) bilərik**.

**Misal:** Bir nöqtə obyektini `(x, y)` formatında göstərmək üçün:

```javascript
let point = {
  x: 1,
  y: 2,
  // toString metodunu override edirik
  toString: function() { 
    return `(${this.x}, ${this.y})`; 
    // Bu obyektin x və y dəyərlərini istifadə edirik
  }
};

console.log(String(point)); 
// "(1, 2)" 
// (String() funksiyası point.toString() metodunu çağırır)
console.log("Nöqtə: " + point); 
// => "Nöqtə: (1, 2)" 
// (String birləşməsi toString() metodunu çağırır)
```

---

### 6.9.2 `toLocaleString()` Metodu — Lokal Formatda String Çevirmə 🌍

#### `toLocaleString()` nədir?

`toLocaleString()` metodu JavaScript obyektlərinin dəyərlərini, istifadəçinin yerli (lokal) dil və region parametrlərinə uyğun olaraq, **oxunaqlı formatda stringə** çevirmək üçün istifadə olunur.

Bu metod `Object.prototype`-dən miras alınır, yəni demək olar ki, bütün obyektlərdə mövcuddur. Lakin əksər daxili obyektlər bu metodu özünəməxsus şəkildə yenidən təyin edirlər ki, nəticə istifadəçiyə uyğun, lokalizasiya edilmiş formada olsun.

---

#### `toLocaleString()`-in əsas xüsusiyyətləri:

* **Default davranış:** `toLocaleString()` standart `toString()` metodunu çağırır, yəni heç bir lokalizasiya etməz.
* **`Date` obyektlərində:** Tarixi və vaxtı istifadəçinin saat qurşağına, dilinə və yerli tarix formatına uyğun göstərir.
* **`Number` obyektlərində:** Rəqəmləri minlik ayırıcıları, onluq nöqtəsi və valyuta işarələri kimi yerli standartlara uyğun göstərir.
* **`Array` obyektlərində:** Hər element üçün `toLocaleString()` çağırır və nəticələri vergüllə birləşdirir.

---

#### Nümunələr

```javascript
// Number obyektində
let number = 1234567.89;
console.log(number.toLocaleString()); 
// "1,234,567.89" (ingilis formatında)
// Azərbaycan dilində və lokalda isə fərqli ola bilər.

// Tarix obyektində
let date = new Date('2024-06-28T14:30:00Z');
console.log(date.toLocaleString('az-AZ')); 
// "28.06.2024 18:30:00" (Azərbaycan lokalına uyğun)

// Array obyektində
let arr = [1000, 2000, 3000];
console.log(arr.toLocaleString()); 
// "1,000,2,000,3,000" (hər ədəd öz lokal formatında)

// Öz obyektimizdə
let point = {
  x: 1000,
  y: 2000,
  toString: function() {
    return `(${this.x}, ${this.y})`; // Sadə string təmsilçiliyi
  },
  toLocaleString: function() { 
    // Rəqəmlərin lokal formatda göstərilməsi
    return `(${this.x.toLocaleString()}, ${this.y.toLocaleString()})`;
  }
};

console.log(point.toString());       // => "(1000, 2000)"
console.log(point.toLocaleString()); // => "(1,000, 2,000)" (lokal formatda)
```

---

#### İstifadə məqsədi

`toLocaleString()` metodu xüsusilə **istifadəçi interfeysində (UI)** istifadəçinin dilinə və region parametrlərinə uyğun göstərmək üçün faydalıdır. Məsələn, pul məbləğləri, tarix və vaxt formatları ölkəyə görə fərqli olduğundan, bu metod rahatlıqla həmin fərqləri idarə etməyə imkan verir.

---

**Qeyd:**
`toLocaleString()` metodu opsional olaraq **lokallaşdırma parametrlərini** də ala bilər (məsələn, dil kodu, format opsiyaları), bu isə əlavə tənzimləmələrə imkan yaradır.

```javascript
console.log(number.toLocaleString('de-DE', { style: 'currency', currency: 'EUR' }));
// "1.234.567,89 €" (Almaniya lokalında valyuta formatı)
```

---

### 6.9.3 `valueOf()` Metodu — Obyektin Primitiv Dəyərə Çevrilməsi 🔢

---

#### `valueOf()` nədir?

`valueOf()` metodu obyekt JavaScript-də rəqəmsal və ya başqa primitiv kontekstdə istifadə olunanda avtomatik çağırılır və obyektin primitiv (əsasən rəqəm) dəyərini qaytarır.

---

#### Default davranış

* Əksər obyektlərin `valueOf()` metodu sadəcə **obyektin özünü qaytarır**, yəni xüsusi çevirmə etmir.
* Lakin bəzi daxili siniflər (məsələn, `Date`) `valueOf()` metodunu özlərinə uyğun yenidən təyin ediblər.

---

#### `Date` obyektlərində `valueOf()`

`Date` obyektində `valueOf()` tarixi **millisaniyə (Unix timestamp)** şəklində ədədi dəyərə çevirir.

```javascript
let d = new Date("2024-06-28T00:00:00Z");
console.log(d.valueOf()); // Məsələn: 1719628800000 (Unix timestamp millisaniyədə)
```

Bu sayədə tarixləri asanlıqla müqayisə etmək mümkündür:

```javascript
let d1 = new Date("2024-06-28");
let d2 = new Date("2023-01-01");

console.log(d1 > d2); // true, çünki d1-in millisaniyə dəyəri böyükdür
```

---

#### Öz `valueOf()` nümunəsi: Məsafəni hesablamaq üçün nöqtə (point)

```javascript
let account = {
  owner: "Rəşad",
  balance: 1200,
  valueOf() {
    // Account obyektini rəqəmsal kontekstdə balans kimi qəbul edirik
    return this.balance;
  }
};

console.log(account + 300);   // 1500  (1200 + 300)
console.log(account > 1000);  // true  (1200 > 1000)
console.log(Number(account)); // 1200 (Number() çağıranda valueOf işləyir)
```

---

#### Nəticə

* `valueOf()` obyektin primitiv dəyərə çevrilməsini idarə edir.
* Rəqəmsal və müqayisə əməliyyatlarında obyektin necə davranacağını müəyyən etmək üçün faydalıdır.

---

## 6.9.4 `toJSON()` Metodu 🗃️➡️📝

### `toJSON()` nədir?

`JSON.stringify()` funksiyası obyektləri JSON formatına çevirəndə, əgər həmin obyektin **özünə aid `toJSON()` metodu** varsa, o metod çağırılır və nəticəsi JSON-a çevrilir.

Yəni, `toJSON()` metodu **obyektin JSON üçün "özəl təmsilçisi"** rolunu oynayır.

---

### Nümunə: `Date` obyektində `toJSON()`

```javascript
let d = new Date("2024-06-28T12:00:00Z");

console.log(JSON.stringify(d));
// Nəticə: "\"2024-06-28T12:00:00.000Z\""
// Date obyektinin `toJSON()` metodu onu ISO stringə çevirir
```

---

### Öz `toJSON()` metodu nümunəsi: `point` obyekti

```javascript
let point = {
  x: 3,
  y: 7,
  toString() {
    return `(${this.x}, ${this.y})`;
  },
  toJSON() {
    // JSON.stringify çağırılanda burası işləyir
    return this.toString();  // Obyekti JSON üçün "(3, 7)" kimi təmsil edirik
  }
};

console.log(JSON.stringify(point)); // => "\"(3, 7)\""

console.log(JSON.stringify([point])); // => '["(3, 7)"]'
// Massiv içində də `point`-in `toJSON()` metodu çağırılır
```

---

### Nəticə

* `toJSON()` metodu JSON formatına çevrilmə zamanı obyektin özünü necə göstərəcəyini idarə edir.
* Bu metodu yazmaqla JSON nəticəsini istədiyimiz formada düzəldə bilərik.

---

## 6.10 Genişlənmiş Obyekt Literal Sintaksisi (Extended Object Literal Syntax) 🧩✨

ES6 (ECMAScript 2015) və sonrakı JavaScript versiyaları **obyekt literalını (object literal)** yazmaq üçün bir neçə yeni və faydalı üsul gətirib. Bu yeniliklər kodumuzu daha qısa, oxunaqlı və dinamik edir.

## 6.10.1 Qısa Yazılış Xüsusiyyəti (Shorthand Properties) ⚡

Əgər bir obyekt xüsusiyyətinin adı ilə ona təyin edilən dəyəri verən dəyişənin adı eynidirsə, artıq hər ikisini yazmağa ehtiyac qalmır.

**Əvvəl:**

```javascript
let x = 1, y = 2;
let o = {
  x: x, // Xüsusiyyət adı 'x', dəyər 'x' dəyişənindən
  y: y  // Xüsusiyyət adı 'y', dəyər 'y' dəyişənindən
};
console.log(o.x + o.y); // => 3
```

**İndi (ES6 ilə):**

```javascript
let x = 1, y = 2;
let o = { x, y }; // Əgər adlar eynidirsə, sadəcə dəyişənin adını yazmaq kifayətdir
console.log(o.x + o.y); // => 3
```
---

## 6.10.2 Hesablanmış (Dinamik) Xüsusiyyət Adları (Computed Property Names) 🧮

### Nədir?

Bəzən obyektin xüsusiyyətinin adı əvvəlcədən məlum olmur, dinamik olaraq bir dəyişəndən və ya funksiyanın nəticəsindən alınır. ES6 ilə bu, obyekt literalını yaratarkən birbaşa mümkün oldu.

---

### Əvvəlki üsul (obyekti yaratdıqdan sonra əlavə etmək):

```javascript
const key1 = "name";
function getKey2() { return "age"; }

let obj = {};
obj[key1] = "Rashad";       // Sonradan əlavə edilir
obj[getKey2()] = 25;        // Sonradan əlavə edilir

console.log(obj.name);      // "Rashad"
console.log(obj.age);       // 25
```

---

### İndi ES6 ilə (obyekt literalında birbaşa):

```javascript
const key1 = "name";
function getKey2() { return "age"; }

let obj = {
  [key1]: "Rashad",        // Dinamik açar: "name"
  [getKey2()]: 25          // Dinamik açar: "age"
};

console.log(obj.name);     // "Rashad"
console.log(obj.age);      // 25
```

---

### Vacib!

Kvadrat mötərizələr `[ ... ]` içində istənilən ifadə yaza bilərsən — dəyişən, funksiyanın nəticəsi və ya hətta hesablama:

```javascript
let i = 1;
let obj = {
  ["prop_" + i]: "value"  // Açarı "prop_1" olur
};

console.log(obj.prop_1);  // "value"
```

---

## 6.10.3 Symbol-lardan Xüsusiyyət Adı Kimi İstifadə (Symbols as Property Names) 🛡️

ES6 ilə obyekt xüsusiyyətlərinin adları artıq yalnız string (mətn) deyil, həm də **`Symbol`** ola bilər. Symbol-lar JavaScript-də unikal və "şəffaf olmayan" (opaque) dəyərlərdir.

```javascript
const extension = Symbol("my extension symbol"); 
// Unikal bir Symbol yaradırıq
let o = {
  [extension]: { /* genişlənmə məlumatları burada saxlanır */ } 
  // Symbol-u property adı kimi istifadə edirik
};

o[extension].x = 0; // Bu Symbol ilə obyektə məlumat əlavə edirik
console.log(o[extension].x); // => 0
```

* `Symbol()` funksiyası ilə hər dəfə unikal bir `Symbol` yaradılır. Ona verdiyiniz string arqumenti yalnız debugging (kodda səhvləri tapmaq) məqsədi üçündür, Symbol-un öz dəyərini dəyişmir.
* **Faydası:** Symbol-lar obyektlərə unikal və başqa kodla **toqquşmayan (collision-free)** xüsusiyyət adları əlavə etməyə imkan verir. Bu, xüsusilə üçüncü tərəfin kodundan aldığınız obyektə özəl məlumatlar əlavə etmək istədiyinizdə faydalıdır, çünki adların eyniliyinə görə bir-birinin üzərinə yazma (overwrite) riski yox olur.
* **Qeyd:** Symbol-lar təhlükəsizlik (security) üçün deyil, yalnız ad toqquşmalarının qarşısını almaq üçün nəzərdə tutulub.

---

## 6.10.4 Spread Operator (`...`) — Obyektə Xüsusiyyətlərin Yayılması (Kopyalanması)

ES2018-dən başlayaraq, **spread operatoru (`...`)** mövcud bir obyektin bütün özünə məxsus xüsusiyyətlərini yeni bir obyektin içinə asanlıqla kopyalamaq üçün istifadə olunur.

```javascript
let position = { x: 0, y: 0 };
let dimensions = { width: 100, height: 75 };

let rect = { ...position, ...dimensions }; 
// position və dimensions-ın xüsusiyyətlərini rect-ə kopyalayır

console.log(rect); // => { x: 0, y: 0, width: 100, height: 75 }
console.log(rect.x + rect.y + rect.width + rect.height); // => 175
```

* `...` operatoru yalnız obyektin **özünə məxsus (own)** və **`enumerable`** xüsusiyyətlərini kopyalayır.
* **Miras alınan (inherited)** xüsusiyyətlər kopyalanmır.

**Misal:**

```javascript
let o = Object.create({ x: 1 }); // 'x' miras alınan property-dir
let p = { ...o };               // 'o'-dan 'x' kopyalanmır

console.log(p.x); 
// => undefined, çünki 'x' 'p'-nin özünə məxsus xüsusiyyəti olmadı
```

**Vacib Qeyd:**

* Əgər iki kopyalanan obyektin **eyni adlı xüsusiyyətləri** varsa, **sonuncu obyektin dəyəri üstünlük qazanır** (overwrite edir):

```javascript
let o = { x: 1 };
let p = { x: 0, ...o }; 
// 'o'-dakı 'x:1' 'x:0'-ın üzərinə yazır
console.log(p.x); // => 1

let q = { ...o, x: 2 }; 
// 'o'-dakı 'x:1'-dən sonra 'x:2' yazıldığı üçün bu üstün gəlir
console.log(q.x); // => 2
```

* Spread operatorunun performansı obyektin xüsusiyyətlərinin sayına görə **O(n)** mürəkkəbliyindədir (yəni xüsusiyyət sayı artdıqca icra vaxtı da xətti olaraq artır). Çox böyük obyektlərin spread edilməsi performansa təsir edə bilər.

---

## 6.10.5 Qısa Yazılışla Metod Yaratmaq (Shorthand Methods)

ES6-da obyekt metodlarını təyin etmək üçün daha qısa və aydın bir sintaksis gətirildi.

**ES5-də metod yazılışı:**

```javascript
let square = {
  side: 10,
  area: function() { // 'function' açar sözü və iki nöqtə (:) var
    return this.side * this.side;
  }
};
console.log(square.area()); // => 100
```

**ES6 ilə qısa yazılış:**

```javascript
let square = {
  side: 10,
  area() { // 'function' açar sözü və iki nöqtə (:) yoxdur
    return this.side * this.side;
  }
};
console.log(square.area()); // => 100
```

* Bu yeni sintaksis **`function` açar sözünü və iki nöqtəni (`:`)** yazmaq ehtiyacını aradan qaldırır.
* Bu, obyektin daxilindəki elementin bir **metod** olduğunu daha aydın göstərir.

---

## 6.10.6 Xüsusiyyətlərin Oxuyucuları və Yazıcıları (Property Getters and Setters) 📝

JavaScript-də obyekt xüsusiyyətləri iki cür ola bilər:

1.  **Data Property**: Bu, bildiyimiz adi xüsusiyyətdir, sadəcə bir adın bir dəyəri saxladığı yerdir (məsələn, `ad: "Əli"`).
2.  **Accessor Property**: Bu, birbaşa dəyər saxlamaq əvəzinə, dəyəri oxuyanda və ya yazanda çağırılan xüsusi **getter** və **setter** metodlarına malikdir. Bu metodlar bir növ "qapıçı" kimidir, xüsusiyyətə daxil olan və çıxan dəyərləri idarə edir.

* **Getter**: Bir xüsusiyyətin dəyəri oxunanda (məsələn, `obyekt.xususiyyet` yazanda) çağırılır və dəyəri qaytarır.
* **Setter**: Bir xüsusiyyətə dəyər təyin ediləndə (məsələn, `obyekt.xususiyyet = deyer` yazanda) çağırılır və təyin edilən dəyəri parametr kimi qəbul edir.

---

### Sintaksis:

```javascript
let mehsul = {
  // Bu adi data property-dir:
  qiymet: 10,

  // Bu isə accessor property-dir (getter və setter-i var):
  get vergiliQiymet() {
    // vergiliQiymet oxunanda bu işləyir
    return this.qiymet * 1.18; 
    // Qiymətə 18% ƏDV əlavə edirik
  },

  set vergiliQiymet(yeniQiymet) {
    // vergiliQiymet-ə dəyər veriləndə bu işləyir
    this.qiymet = yeniQiymet / 1.18; 
    // Vergisiz qiyməti hesablayıb saxlayırıq
  }
};

console.log(mehsul.qiymet);          // => 10
console.log(mehsul.vergiliQiymet);   
// vergiliQiymet oxunur, getter işləyir => 11.8 (10 * 1.18)

mehsul.vergiliQiymet = 23.6;         
// vergiliQiymet-ə dəyər verilir, setter işləyir
console.log(mehsul.qiymet);          // => 20 (23.6 / 1.18)
console.log(mehsul.vergiliQiymet);   // => 23.6
```

Bu nümunədə **`vergiliQiymet`** birbaşa dəyər saxlamır. Onun dəyəri **`qiymet`** data property-si üzərindən hesablanır və idarə olunur. Bu, daxili məlumatın (qiymətin) necə istifadə olunduğunu gizlətməyə və daha çevik riyazi hesablamalar etməyə imkan verir.

---

### Nümunə: Tam Adı Yoxlama (Virtual Property) 🧍

Gəlin bir istifadəçi obyektinə baxaq. Biz istifadəçinin `ad` və `soyad`ını saxlayırıq, amma eyni zamanda `tamAd` adlı bir "virtual" xüsusiyyətə də ehtiyacımız var.

```javascript
let user = {
  ad: "Kamran",
  soyad: "Əliyev",

  get tamAd() {
    // tamAd-ı oxuyanda ad və soyadı birləşdirir
    return `${this.ad} ${this.soyad}`;
  },

  set tamAd(value) {
    // tamAd-a dəyər veriləndə, dəyəri boşluğa görə ayırıb ad və soyadı təyin edir
    let parts = value.split(" "); // "Ayxan Muradov" => ["Ayxan", "Muradov"]
    this.ad = parts[0];
    this.soyad = parts[1];
  }
};

console.log(user.tamAd); // => "Kamran Əliyev" (getter işləyir)

user.tamAd = "Ayxan Muradov"; // Setter işləyir
console.log(user.ad);      // => "Ayxan"
console.log(user.soyad);   // => "Muradov"
console.log(user.tamAd);   // => "Ayxan Muradov"
```
Burada `tamAd` adlı bir data property yoxdur, amma biz ona adi bir property kimi daxil ola və ya dəyər təyin edə bilirik. Bu, kodumuzu daha səliqəli və məntiqi edir.

---