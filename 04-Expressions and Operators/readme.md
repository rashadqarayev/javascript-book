### Fəsil 4. İfadəlar və Operatorlar (Expressions and Operators)
Bu fəsil, JavaScript-in təməl quruluş daşları olan **ifadələri (expressions)** və onların qurulmasında istifadə olunan **operatorları** əhatə edir.

**İfadə (Expression)** – JavaScript-də qiymətləndirilərək (evaluated) bir **dəyər (value)** yaradan istənilən kod parçasıdır. Sadə bir rəqəm, bir dəyişənin adı və ya daha mürəkkəb riyazi əməliyyatlar – bunların hamısı bir ifadədir. Operatorlar isə, bu sadə ifadələri birləşdirərək daha mürəkkəb ifadələr yaratmaq üçün istifadə olunur. Məsələn, `x` və `y` birer ifadədirsə, `x * y` da onların hasilini yaradan yeni bir ifadədir.

---
### 4.1 İlkin İfadəlar (Primary Expressions)
Ən sadə ifadələr, yəni özündən daha sadə bir ifadədən təşkil olunmayan ifadələrə **ilkin ifadələr (primary expressions)** deyilir. Bunlar JavaScript-in "atoları"dır. Üç əsas növü var:

**1. Literallar (Literals)**
Proqramın içinə birbaşa yazılan sabit dəyərlərdir.
```javascript
1.23         // Rəqəm (Number) literalı
"salam"      // Sətir (String) literalı
/pattern/    // Requlyar İfadə (RegExp) literalı
```

**2. Açar Sözlər (Keywords)**
Bəzi rezerv olunmuş açar sözlər özləri birbaşa bir dəyəri ifadə edir.
```javascript
true         // `true` boolean dəyəri
false        // `false` boolean dəyəri
null         // `null` dəyəri
this         // Hazırkı obyektə istinad (kontekstdən asılı olaraq dəyişir)
```

**3. Dəyişən İstinadları (Variable References)**
Kodda tək başına yazılan bir identifikator (ad), JavaScript tərəfindən bir dəyişən, sabit və ya qlobal obyektin bir xüsusiyyəti kimi qəbul edilir və onun dəyərini tapmağa çalışır.
```javascript
i            // i dəyişəninin dəyəri
sum          // sum dəyişəninin dəyəri
undefined    // qlobal obyektdəki "undefined" xüsusiyyətinin dəyəri
```
Əgər bu adda bir dəyişən tapılmazsa, proqram `ReferenceError` xətası verər.

---
### 4.2 Obyekt və Massiv İnisiatorları (Initializers) 📦
Bunlar, yeni bir obyekt (object) və ya massiv (array) yaradan ifadələrdir. Onlara çox vaxt "obyekt literalı" və ya "massiv literalı" da deyilir.

**Massiv İnisiatorları (Array Initializers)**
Kvadrat mötərizə `[]` içində, vergüllə ayrılmış bir ifadələr siyahısıdır.
```javascript
[]                         // Boş bir massiv
[1 + 2, 3 + 4]             // İki elementli massiv: [3, 7]
let matrix = [[1,2,3], [4,5,6]]; // İç-içə massivlər

// Vergüllər arasında boşluq buraxaraq "boş" (undefined) elementlər yaratmaq olar
let sparse = [1, , , 4]; // 4 elementli massiv: [1, undefined, undefined, 4]
console.log(sparse.length); // ✅ Nəticə: 4
```

**Obyekt İnisiatorları (Object Initializers)**
Fiqurlu mötərizə `{}` içində, `açar: dəyər` formatında cütlüklərdən ibarətdir.
```javascript
{} // Boş bir obyekt

let point = { x: 10, y: 20 }; // İki xüsusiyyətli obyekt

// İç-içə obyektlər
let rectangle = {
  topLeft: { x: 0, y: 0 },
  bottomRight: { x: 100, y: 50 }
};
```
---
### 4.3 Funksiya Təyin Etmə İfadələri (Function Definition Expressions)
Bu, yeni bir funksiya yaradan və nəticə olaraq həmin funksiya obyektini qaytaran bir ifadədir. Bu, "funksiya literalı" kimidir.

**Nümunə:**
```javascript
// Bu ifadənin dəyəri, `x`-in kvadratını hesablayan bir funksiyadır.
// Həmin funksiya `square` adlı dəyişənə mənimsədilir.
let square = function(x) { 
  return x * x; 
};

// İndi `square` dəyişəni vasitəsilə funksiyanı çağıra bilərik
let result = square(5); // ✅ Nəticə: 25
```
Funksiyaları təyin etməyin `function` ifadəsindən başqa, `function` bəyanatı (statement) və ES6 ilə gələn ox funksiyaları (`=>`) kimi yolları da var. Bunları Fəsil 8-də detallı şəkildə araşdıracağıq.

***
### 4.4 Xüsusiyyətə Müraciət İfadələri (Property Access Expressions) `.` `[]`
**Xüsusiyyətə müraciət (property access)** ifadəsi, bir obyektin xüsusiyyətinin və ya bir massivin (array) elementinin dəyərini əldə etmək üçün istifadə olunur. JavaScript-də bunun üçün iki fərqli sintaksis var:

1.  **Nöqtə Sintaksisi (Dot Notation):** `ifade.identifikator`
2.  **Kvadrat Mötərizə Sintaksisi (Bracket Notation):** `ifade[ifade]`

**Geniş Nümunə:**
```javascript
const user = {
  "first name": "Ayan", // adda boşluq var
  age: 25,
  address: {
    city: "Bakı"
  }
};

const data = [user, { id: 2 }];

// Nöqtə sintaksisi ilə müraciət
console.log(user.age); // ✅ 25
console.log(user.address.city); // ✅ "Bakı"

// Kvadrat mötərizə sintaksisi ilə müraciət
console.log(user["age"]); // ✅ 25
console.log(data[0]["address"]["city"]); // ✅ "Bakı"

// Nə vaxt kvadrat mötərizə məcburidir?
// 1. Əgər xüsusiyyətin adı düzgün identifikator deyilsə (məsələn, boşluq varsa)
// console.log(user.first name); // ❌ Səhv! Bu işləməyəcək.
console.log(user["first name"]); // ✅ "Ayan"

// 2. Əgər xüsusiyyətin adı bir dəyişəndə saxlanılırsa (dinamik müraciət)
let propName = "age";
console.log(user[propName]); // ✅ 25
```
❗️ **Vacib:** Əgər nöqtə və ya mötərizədən əvvəlki ifadə `null` və ya `undefined` olarsa, proqram dərhal `TypeError` xətası verərək dayanacaq.

---
### 4.4.1 Şərti Müraciət (Conditional Access / Optional Chaining) `?.`
Yuxarıda qeyd etdiyimiz `TypeError` xətasından qaçmaq üçün əvvəllər uzun-uzun `if` yoxlamaları yazmaq lazım gəlirdi. ES2020-də gələn **Optional Chaining** operatoru (`?.`) bu problemi çox zərif bir şəkildə həll edir.

**Necə İşləyir?**
`?.` operatoru, solundakı dəyərin `null` və ya `undefined` olub-olmadığını yoxlayır.
* Əgər dəyər `null` və ya `undefined`-dırsa, ifadənin qalan hissəsini heç icra etmədən dərhal `undefined` qaytarır və xəta baş vermir (**short-circuiting**).
* Əgər dəyər `null` və ya `undefined` deyilsə, müraciət normal şəkildə davam edir.

**Geniş Nümunə: "Əvvəl və Sonra"**
**Problem (Köhnə üsul):**
```javascript
const user = {
  name: "Ayan",
  // profil məlumatları hələ yoxdur
  profile: null 
};

let street;
// Küçə məlumatını almaq üçün iç-içə yoxlama aparmalıyıq ki, xəta olmasın
if (user && user.profile && user.profile.address) {
  street = user.profile.address.street;
} else {
  street = undefined;
}
console.log("Köhnə üsulla küçə:", street); // ✅ undefined
```
**Həll Yolu (Müasir üsul - Optional Chaining):**
```javascript
const user = { name: "Ayan", profile: null };

// Eyni məntiq, tək bir zərif sətirlə!
const street = user.profile?.address?.street;

console.log("Yeni üsulla küçə:", street); // ✅ undefined (HEÇ BİR XƏTA OLMADAN!)
```

---
### 4.5 Funksiya Çağırış İfadələri (Invocation Expressions) `()`
Adi mötərizə `()` cütlüyü, JavaScript-də **funksiya çağırma (invocation)** operatorudur. O, özündən əvvəl gələn ifadənin bir funksiya olduğunu güman edir və onu arqumentlərlə birlikdə icra edir.
```javascript
Math.max(10, 20); // Math.max funksiyasını çağırır
myArray.sort();   // myArray obyektinin sort metodunu çağırır
```
Əgər çağırılan ifadə bir xüsusiyyətə müraciətdirsə (`myArray.sort`), bu, **metod çağırışı (method invocation)** adlanır və funksiyanın daxilindəki `this` açar sözü həmin obyektə (`myArray`-a) işarə edir.

---
### 4.5.1 Şərti Çağırış (Conditional Invocation) `?.()`
Bu da ES2020 ilə gələn bir yenilikdir və **Optional Chaining**-in funksiyalar üçün olan versiyasıdır.

**Problem:** Bəzən bir funksiyaya arqument kimi başqa bir funksiya (callback) ötürülür, amma bu arqument könüllü (optional) ola bilər. Onu çağırmazdan əvvəl mövcud olub-olmadığını yoxlamaq lazımdır.

**Həll Yolu:** `?.()` operatoru, solundakı dəyərin `null` və ya `undefined` olub-olmadığını yoxlayır. Əgər `null` və ya `undefined`-dırsa, heç bir şey etmir və `undefined` qaytarır. Əks halda isə funksiyanı çağırır.

**Geniş Nümunə: "Əvvəl və Sonra"**
**Problem (Köhnə üsul):**
```javascript
function square(x, log) {
  // `log` funksiyasının ötürülüb-ötürülmədiyini yoxlayırıq
  if (log) {
    log(x);
  }
  return x * x;
}
```
**Həll Yolu (Müasir üsul - Conditional Invocation):**
```javascript
function square(x, log) {
  // Əgər `log` mövcuddursa, onu çağır. Yoxdursa, heç nə etmə.
  log?.(x); 
  return x * x;
}
```
**`o.m()`, `o?.m()`, və `o.m?.()` arasındakı fərq:**
Bu üç ifadənin fərqini anlamaq çox vacibdir:
* `o.m()`: `o` mütləq bir obyekt olmalıdır və onun `m` adlı bir funksiya xüsusiyyəti olmalıdır. Əks halda `TypeError`.
* `o?.m()`: Əgər `o` `null` və ya `undefined`-dırsa, bütün ifadə `undefined` olur və dayanır. Əgər `o` bir obyektdirsə, onun mütləq `m` adlı bir funksiya xüsusiyyəti olmalıdır.
* `o.m?.()`: `o` mütləq bir obyekt olmalıdır. Amma onun `m` adlı xüsusiyyəti olmaya da bilər (və ya `null` ola bilər). Əgər `m` bir funksiyadırsa, çağırılır. Əks halda, ifadə `undefined` olur.

***
### 4.6 Obyekt Yaratma İfadələri (Object Creation Expressions) 🏗️
**Obyekt yaratma ifadəsi (object creation expression)**, yeni bir obyekt yaradır və həmin obyektin xüsusiyyətlərini (properties) ilkinləşdirmək üçün bir funksiyanı – yəni **konstruktoru (constructor)** – çağırır.

Bu ifadələr, funksiya çağırışlarına çox bənzəyir, lakin onların qarşısında **`new`** açar sözü olur.

**Nümunə:**
```javascript
// Tutaq ki, belə bir sinifimiz (class) var
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

// `new` ilə bu sinifdən yeni bir obyekt (nüsxə - instance) yaradırıq
const p1 = new Point(10, 20);
const p2 = new Point(5, 5);

console.log(p1); // ✅ Point { x: 10, y: 20 }

// Konstruktora heç bir arqument ötürülmürsə, mötərizələri yazmaya da bilərsiniz
const rightNow = new Date; // `new Date()` ilə eynidir
console.log(rightNow);
```
Obyekt yaratma ifadəsinin dəyəri, yeni yaradılmış obyektin özüdür.

---
### 4.7 Operatorlara Ümumi Baxış (Operator Overview) 🧮
Bu bölmə, JavaScript-dəki bütün operatorların ümumi bir icmalı və onların üstünlük (precedence) və assosiativlik (associativity) kimi vacib xüsusiyyətlərini izah edir.

#### Operatorların Üstünlük Cədvəli (sadələşdirilmiş)
Aşağıda operatorlar, ən yüksək üstünlükdən ən aşağıya doğru qruplaşdırılıb. Eyni qrupdakı operatorlar eyni üstünlüyə malikdir.

1.  **Ən Yüksək: Qruplaşdırma** `()`
2.  **Üzvlük və Çağırış:** `.` (obyekt xüsusiyyəti), `[]` (massiv elementi), `?.` (optional chaining), `()` (funksiya çağırışı), `new`
3.  **Artırma/Azaltma (Postfix):** `++`, `--`
4.  **Unar Operatorlar:** `!` (not), `~` (bitwise not), `+` (rəqəmə çevirmə), `-` (mənfi etmə), `++`/`--` (prefix), `typeof`, `delete`
5.  **Qüvvət:** `**`
6.  **Vurma/Bölmə/Qalıq:** `*`, `/`, `%`
7.  **Toplama/Çıxma:** `+`, `-`
8.  **Bit Sürüşdürmə:** `<<`, `>>`, `>>>`
9.  **Müqayisə:** `<`, `<=`, `>`, `>=`, `instanceof`, `in`
10. **Bərabərlik:** `==`, `!=`, `===`, `!==`
11. **Bit Məntiqi:** `&`, `^`, `|`
12. **Məntiqi Operatorlar:** `&&` (və), `||` (və ya), `??` (nullish coalescing)
13. **Şərt (Ternary) Operatoru:** `... ? ... : ...`
14. **Mənimsətmə (Assignment):** `=`, `+=`, `-=`, `*=`, `**=` və s.
15. **Vergül Operatoru:** `,`

---
#### Operator Anlayışları
**4.7.1 Operandların Sayı (Arity)**
* **Unar (Unary):** Tək operand qəbul edir. Məsələn, `-x` ifadəsində `-` operatoru.
* **Binar (Binary):** İki operand qəbul edir. Əksər operatorlar binar-dır. Məsələn, `x + y`.
* **Ternar (Ternary):** Üç operand qəbul edir. JavaScript-də yeganə ternar operator `?:` şərt operatorudur.

---

**4.7.2 Operand və Nəticə Tipləri**
Əksər operatorlar müəyyən bir tipdə operandlar gözləyir. Əgər fərqli bir tipdə dəyər verilsə, JavaScript onu avtomatik olaraq çevirməyə çalışır.
`"3" * "5"` → `3` və `5` rəqəmlərinə çevrilir və nəticə `15` olur (rəqəm olaraq).

---

**4.7.3 Operatorların Əlavə Təsirləri (Side Effects)**
Bəzi operatorlar, dəyər hesablamaqla yanaşı, proqramın vəziyyətini də dəyişir. Buna **yan təsir (side effect)** deyilir.
* Mənimsətmə operatorları (`=`, `+=`): Dəyişənin dəyərini dəyişir.
* `++` və `--`: Dəyişənin dəyərini artırır və ya azaldır.
* `delete`: Obyektin bir xüsusiyyətini silir.

---
**4.7.4 Operatorların Üstünlüyü (Precedence)**
Bu, mürəkkəb bir ifadədə hansı əməliyyatın birinci yerinə yetiriləcəyini müəyyən edir.
`w = x + y * z;`
Burada, `*` operatorunun üstünlüyü `+`-dan daha yüksək olduğu üçün, əvvəlcə `y` və `z` vurulur. `=` isə ən aşağı üstünlüyə malik olduğu üçün ən sonda icra olunur. Üstünlüyü dəyişmək üçün mötərizələrdən `()` istifadə olunur:
`w = (x + y) * z;`

---

**4.7.5 Operatorların Assosiativliyi (Associativity)**
Eyni üstünlüyə malik operatorlar bir ifadədə ardıcıl gəldikdə, onların hansı istiqamətdə icra olunacağını bildirir.
* **Soldan-sağa (Left-to-Right):** Əksər operatorlar belədir. Məsələn:
  `w = x - y - z;` eynidir `w = ((x - y) - z);`
* **Sağdan-sola (Right-to-Left):** Mənimsətmə (`=`), qüvvət (`**`) və şərt (`?:`) operatorları belədir. Məsələn:
  `w = x = y = z;` eynidir `w = (x = (y = z));`

---

**4.7.6 Qiymətləndirmə Ardıcıllığı (Order of Evaluation)**
Bu, ən vacib qaydalardan biridir. Operatorların üstünlüyündən və assosiativliyindən asılı olmayaraq, JavaScript ifadələri **həmişə ciddi şəkildə soldan-sağa** qiymətləndirir.

`w = x + y * z;` ifadəsində:
1. Əvvəlcə `w` dəyişəni tapılır.
2. Sonra `x` dəyişəni tapılır.
3. Sonra `y` dəyişəni tapılır.
4. Sonra `z` dəyişəni tapılır.
5. Yalnız bundan sonra, üstünlüyə görə, `y * z` əməliyyatı aparılır.
6. Sonra toplama, sonda isə mənimsətmə icra olunur.

***
### 4.8 Riyazi (Arithmetic) İfadəlar 🧮
Bu bölmə, operandlar üzərində riyazi və ya rəqəmsal əməliyyatlar aparan operatorları əhatə edir.

Əsas riyazi operatorlar bunlardır: `**` (qüvvətə yüksəltmə), `*` (vurma), `/` (bölmə), `%` (qalıq-modulo), `+` (toplama) və `-` (çıxma).

Bu operatorlar (toplama istisna olmaqla), operandları rəqəm deyilsə, onları rəqəmə çevirməyə çalışır. Əgər çevirmək mümkün deyilsə, nəticə `NaN` (Not-a-Number) olur.

```javascript
3 ** 4; // 3-ün 4-cü qüvvəti => 81
10 * 5; // => 50
10 / 3; // ✅ 3.333... (JavaScript-də bölmə həmişə həqiqi rəqəm qaytarır)
10 % 3; // 10-u 3-ə böldükdə qalan qalıq => 1
```
❗️ **Vacib Qeydlər:**
* Qüvvət (`**`) operatoru sağdan-sola işləyir: `2 ** 2 ** 3` ifadəsi `2 ** 8` (256) kimi hesablanır.
* `-3 ** 2` kimi bir ifadə yazmaq sintaksis xətasıdır. Brauzer çaşqınlığın qarşısını almaq üçün sizdən mötərizə istifadə etməyi tələb edir: `(-3) ** 2` (9) və ya `-(3 ** 2)` (-9).

---
#### 4.8.1 Toplama Operatoru (+): "Toplama yoxsa Birləşdirmə?"
`+` operatorunun iki fərqli rolu var: rəqəmləri toplayır və sətirləri (strings) birləşdirir (concatenation).

**Qızıl Qayda:** Əgər operandlardan **heç olmasa biri sətirdirsə**, həmişə **birləşdirmə (concatenation)** baş verir. Yalnız hər iki tərəf rəqəmə çevrilə biləndə **toplama (addition)** olur.

**Geniş Nümunə Cədvəli:**
```javascript
// Rəqəm + Rəqəm = Toplama
1 + 2;            // ✅ 3

// Sətir + Sətir = Birləşdirmə
"salam" + " " + "dünya"; // ✅ "salam dünya"
"1" + "2";        // ✅ "12"

// Sətir + Rəqəm = Birləşdirmə (rəqəm sətirə çevrilir)
"1" + 2;            // ✅ "12"
1 + "2";            // ✅ "12"

// Obyekt/Boolean/Null ilə
true + true;        // ✅ 2        (true 1-ə çevrilir)
2 + null;           // ✅ 2        (null 0-a çevrilir)
2 + undefined;      // ✅ NaN      (undefined rəqəmə çevrilmir)
1 + {};             // ✅ "1[object Object]" (obyekt sətirə çevrilir)
```
**Assosiativlik (Associativity) Nümunəsi:**
`+` operatoru soldan-sağa işlədiyi üçün, mötərizələr nəticəni tamamilə dəyişə bilər.
```javascript
1 + 2 + " kor siçan"; // Əvvəlcə 1+2 hesablanır -> 3, sonra birləşir -> "3 kor siçan"
1 + (2 + " kor siçan"); // Əvvəlcə 2 " kor siçan"-la birləşir -> "2 kor siçan", sonra 1 birləşir -> "12 kor siçan"
```

---
#### 4.8.2 Unar Riyazi Operatorlar (`+`, `-`, `++`, `--`)
Bu operatorlar tək bir operand üzərində işləyir.
* **Unar Plus `(+)`**: Operandını rəqəmə çevirməyə çalışır. Məsələn, `+"3"` ifadəsi `3` rəqəmini qaytarır.
* **Unar Minus `(-)`**: Operandını rəqəmə çevirib, işarəsini əksinə dəyişir.
* **İnkrement `(++)`**: Operandının dəyərini bir vahid artırır.
* **Dekrement `(--)`**: Operandının dəyərini bir vahid azaldır.

**`++` və `--` üçün Pre- və Post- fərqi:**
* **Pre-inkrement (`++i`):** Əvvəlcə dəyəri artırır, sonra **yeni dəyəri** qaytarır.
* **Post-inkrement (`i++`):** Əvvəlcə **köhnə dəyəri** qaytarır, sonra dəyəri artırır.

**Geniş Nümunə:**
```javascript
let pre_i = 10;
let pre_j = ++pre_i; // pre_i olur 11, pre_j-ə 11 mənimsədilir
console.log(`Pre-increment: i=${pre_i}, j=${pre_j}`); // ✅ i=11, j=11

let post_i = 10;
let post_j = post_i++; // post_j-ə köhnə dəyər olan 10 mənimsədilir, sonra post_i 11 olur
console.log(`Post-increment: i=${post_i}, j=${post_j}`); // ✅ i=11, j=10
```
---
#### 4.8.3 Bitlər Üzərində Əməliyyat Operatorları (Bitwise Operators) 👾
Bu, aşağı səviyyəli (low-level) bir mövzudur və əksər veb proqramçılar bunlardan demək olar ki, heç vaxt istifadə etmir. Bu operatorlar rəqəmlərin özləri ilə deyil, onların ikilik (binary) say sistemindəki bitləri ilə işləyir.

Əsas bitwise operatorları bunlardır:
* **`&` (Bitwise AND):** Hər iki operandda eyni mövqedəki bit `1` olarsa, nəticədə də həmin bit `1` olur.
* **`|` (Bitwise OR):** Operandlardan birində müvafiq bit `1` olarsa, nəticədə də `1` olur.
* **`^` (Bitwise XOR):** Yalnız operandlardan birində (hər ikisində eyni anda yox) müvafiq bit `1` olarsa, nəticədə `1` olur.
* **`~` (Bitwise NOT):** Bütün bitləri tərsinə çevirir (`0`-ları `1`, `1`-ləri `0`).
* **`<<` (Left Shift)**, **`>>` (Sign-propagating Right Shift)**, **`>>>` (Zero-fill Right Shift)**: Bitləri sola və ya sağa sürüşdürür.

Bunlar adətən icazələr (permissions) kimi bayraq (flag) sistemləri qurmaqda və ya aşağı səviyyəli qrafika və ya cihaz proqramlaşdırmasında istifadə olunur.


***
### 4.9 Nisbət Operatorları (Relational Expressions)
Bu bölmədə, iki dəyər arasında bir əlaqəni (bərabərlik, kiçiklik, mənsubiyyət) yoxlayan və nəticə olaraq `true` və ya `false` qaytaran operatorları araşdıracağıq. Bu ifadələr, proqramın axışını idarə edən `if`, `while` kimi strukturların təməlini təşkil edir.

---
#### 4.9.1 Bərabərlik və Bərabərsizlik Operatorları (`==`, `===`, `!=`, `!==`)
JavaScript-də bərabərliyi yoxlamaq üçün iki fərqli operator var. Onların fərqini bilmək, proqramlaşdırmada ən çox edilən səhvlərdən birinin qarşısını alır.

❗️ **Qızıl Qayda:** Peşəkar JavaScript proqramlaşdırmasında **demək olar ki, həmişə `===` (ciddi bərabərlik) və `!==`** operatorlarından istifadə olunur. `==` və `!=` operatorları gözlənilməz nəticələrə və çətin tapılan xətalara səbəb ola bilən mürəkkəb tip çevrilmələri (type coercion) apardığı üçün onların istifadəsi **məsləhət görülmür**.

* **`===` (Ciddi Bərabərlik / Strict Equality):** İki dəyərin həm **tipini**, həm də **dəyərini** müqayisə edir. Heç bir tip çevrilməsi aparmır. Əgər tiplər fərqlidirsə, dərhal `false` qaytarır.
* **`==` (Mücərrəd Bərabərlik / Loose Equality):** Dəyərləri müqayisə etməzdən əvvəl, onların tiplərini eyniləşdirmək üçün arxa planda **tip çevrilmələri** aparır. Bu, çox vaxt gözlənilməz nəticələr verir.

**Geniş Nümunə: `==` tələləri**
Aşağıdakı cədvəl, `==` operatorunun nə qədər çaşdırıcı ola biləcəyini göstərir.

| İfadə (Expression)  | `==` Nəticəsi | `===` Nəticəsi | İzah                                             |
| ------------------- | :-----------: | :------------: | ------------------------------------------------ |
| `5 == "5"`          | ✅ `true`     | ❌ `false`     | `==` sətiri rəqəmə çevirir                       |
| `0 == false`        | ✅ `true`     | ❌ `false`     | `==` boolean-ı rəqəmə çevirir (false -> 0)       |
| `null == undefined` | ✅ `true`     | ❌ `false`     | `==` üçün xüsusi bir istisnadır                  |
| `[] == 0`           | ✅ `true`     | ❌ `false`     | Boş massiv primitivə çevrildikdə 0 olur          |
| `"" == 0`           | ✅ `true`     | ❌ `false`     | Boş sətir rəqəm 0-a çevrilir                     |
| `" \t\r\n" == 0`    | ✅ `true`     | ❌ `false`     | Yalnız boşluqlardan ibarət sətir də 0-a çevrilir |

**Obyektlərin Müqayisəsi:** `==` və `===` operatorlarının hər ikisi obyektləri **istinadla (by reference)** müqayisə edir, dəyərlə yox. Yəni, iki fərqli obyektin daxilindəki xüsusiyyətlər eyni olsa belə, onlar bərabər sayılmır.

---
#### 4.9.2 Müqayisə Operatorları (`<`, `>`, `<=`, `>=`)
Bu operatorlar iki dəyərin sıralamasını (ədədi və ya əlifba sırası) yoxlayır.

**Əsas İşləmə Məntiqi:**
* Əgər hər iki operand sətirdirsə, müqayisə əlifba sırası ilə (Unicode kodlarına görə) aparılır.
* Əks halda, hər iki operand rəqəmə çevrilir və rəqəmsal müqayisə aparılır.

**Geniş Nümunə Cədvəli:**
| İfadə        | Nəticə     | İzah                                                       |
| ------------ | :---------: | ---------------------------------------------------------- |
| `10 < 3`     | ❌ `false`  | Rəqəm müqayisəsi                                           |
| `"10" < "3"` | ✅ `true`   | Sətir müqayisəsi ("1" simvolu "3"-dən əvvəl gəlir)          |
| `"2" < 10`   | ✅ `true`   | Rəqəm müqayisəsi ("2" sətiri rəqəm 2-yə çevrilir)         |
| `"Z" < "a"`  | ✅ `true`   | Sətir müqayisəsi (böyük hərflərin Unicode kodu kiçiklərdən əvvəl gəlir) |

**Doğru sıralama üçün** `String.prototype.localeCompare()` və ya `Intl.Collator` istifadə etmək daha düzgündür.

---
#### 4.9.3 `in` Operatoru
`in` operatoru, bir obyektin (və ya onun prototip zəncirinin) verilmiş adda bir xüsusiyyətə sahib olub-olmadığını yoxlayır.

**Nümunə:**
```javascript
const user = { name: "Ayan", age: 25 };
const numbers = [10, 20, 30];

// Obyekt üçün
console.log("name" in user);   // ✅ true
console.log("city" in user);   // ❌ false
console.log("toString" in user); // ✅ true (çünki Object.prototype-dan miras alır)

// Massiv üçün (indekslər xüsusiyyət adı kimi yoxlanılır)
console.log("1" in numbers);     // ✅ true (indeks 1 var)
console.log(3 in numbers);       // ❌ false (indeks 3 yoxdur)
```

---
#### 4.9.4 `instanceof` Operatoru
`instanceof` operatoru, bir obyektin müəyyən bir sinifin (class) nüsxəsi (instance) olub-olmadığını yoxlayır.

**Necə İşləyir?**
`o instanceof C` ifadəsi, arxa planda `C.prototype` obyektinin `o` obyektinin prototip zəncirində mövcud olub-olmadığını yoxlayır.

**Nümunə:**
```javascript
const d = new Date();
const a = [];

console.log(d instanceof Date);    // ✅ true
console.log(d instanceof Object);  // ✅ true (bütün obyektlər Object-dən törəyir)
console.log(d instanceof Array);   // ❌ false

console.log(a instanceof Array);   // ✅ true
console.log(a instanceof Object);  // ✅ true (bütün massivlər obyektdir)
```
***
### 4.10 Məntiqi İfadəlar (Logical Expressions)
Bu bölmədə, adətən müqayisə operatorları ilə birlikdə istifadə edilən `&&` (VƏ), `||` (VƏ YA), və `!` (DEYİL) məntiqi operatorlarını araşdıracağıq. Bu operatorları tam anlamaq üçün, JavaScript-dəki **"doğru-bənzər" (truthy)** və **"yalan-bənzər" (falsy)** anlayışlarını xatırlamaq vacibdir. (Xatırlatma: `false`, `null`, `undefined`, `0`, `NaN`, və `""` falsy-dir, qalan hər şey isə truthy).

---
#### 4.10.1 Məntiqi VƏ (Logical AND `&&`)
`&&` operatoru, sol tərəfdəki ifadəni qiymətləndirir:
* Əgər sol tərəf **falsy**-dirsə, o, heç sağ tərəfə baxmadan, dərhal **sol tərəfin dəyərini** qaytarır.
* Əgər sol tərəf **truthy**-dirsə, o, sağ tərəfdəki ifadəni qiymətləndirir və **sağ tərəfin dəyərini** qaytarır.

Bu davranışa **"qısa qapanma" (short-circuiting)** deyilir.

**Geniş Nümunə: `TypeError`-dan qaçmaq**
Bu, `&&`-nin ən çox istifadə olunduğu yerlərdən biridir: bir obyektin `null` və ya `undefined` olmadığını yoxlayıb, yalnız bundan sonra onun bir xüsusiyyətinə müraciət etmək.
```javascript
// Köhnə üsul (if ilə):
function printUsername(user) {
  if (user) {
    console.log(user.name);
  }
}

// Müasir üsul (`&&` ilə):
function printUsernameModern(user) {
  // Əgər `user` truthy-dirsə (yəni null deyil), o zaman `user.name`-i icra et (və qaytar).
  user && console.log(user.name); 
}

const user1 = { name: "Ayan" };
const user2 = null;

printUsernameModern(user1); // ✅ "Ayan"
printUsernameModern(user2); // ✅ Heç bir xəta baş vermir, çünki `user2` falsy olduğu üçün ikinci hissə heç vaxt icra olunmur.
```
---
#### 4.10.2 Məntiqi VƏ YA (Logical OR `||`)
`||` operatoru da "qısa qapanma" prinsipi ilə işləyir, amma tərsinə:
* Əgər sol tərəf **truthy**-dirsə, o, heç sağ tərəfə baxmadan, dərhal **sol tərəfin dəyərini** qaytarır.
* Əgər sol tərəf **falsy**-dirsə, o, sağ tərəfdəki ifadəni qiymətləndirir və **sağ tərəfin dəyərini** qaytarır.

Bu, adətən bir neçə variant arasından ilk "doğru" olanı seçmək və ya standart (default) dəyərlər təyin etmək üçün istifadə olunur.

**Geniş Nümunə: Standart dəyər təyin etmək**
```javascript
function welcome(username) {
  // Əgər `username` ötürülübsə (truthy), onu istifadə et.
  // Əks halda (falsy: undefined, null, ""), "Qonaq" dəyərini götür.
  const nameToDisplay = username || "Qonaq";
  console.log(`Xoş gəldin, ${nameToDisplay}!`);
}

welcome("Ayan"); // ✅ Xoş gəldin, Ayan!
welcome("");     // ✅ Xoş gəldin, Qonaq! (çünki "" falsy-dir)
welcome(null);   // ✅ Xoş gəldin, Qonaq!
```
❗️ **Qeyd:** Bu üsul, `0` rəqəmi keçərli bir dəyər olduqda problem yarada bilər, çünki `0` da `falsy`-dir. Müasir JavaScript-də bu problemi həll etmək üçün **nullish coalescing operator (`??`)** istifadə olunur.

---
#### 4.10.3 Məntiqi DEYİL (Logical NOT `!`)
`!` unar bir operatordur və operandının boolean dəyərini **tərsinə çevirir**.
* Əgər operand **truthy**-dirsə, `!` `false` qaytarır.
* Əgər operand **falsy**-dirsə, `!` `true` qaytarır.

`&&` və `||`-dan fərqli olaraq, `!` həmişə `true` və ya `false` qaytarır.

**Nümunə: `!!` ilə boolean-a çevirmək**
İkiqat NOT (`!!`) istənilən bir dəyəri onun həqiqi boolean ekvivalentinə çevirmək üçün istifadə olunan bir "trick"-dir.
```javascript
console.log(!!"Salam");   // ✅ true (sətir truthy-dir)
console.log(!!0);         // ✅ false (0 falsy-dir)
console.log(!!{});        // ✅ true (boş obyekt truthy-dir)
console.log(!!undefined); // ✅ false (undefined falsy-dir)
```

---
### 4.11 Mənimsətmə İfadələri (Assignment Expressions) `=`
JavaScript-də `=` operatoru bir dəyişənə və ya xüsusiyyətə dəyər mənimsətmək üçün istifadə olunur.

**Zəncirvari Mənimsətmə:**
`=` operatoru sağdan-sola assosiativliyə malikdir. Bu, bir neçə dəyişənə eyni anda eyni dəyəri mənimsətməyə imkan verir:
```javascript
let x, y, z;
x = y = z = 10; // Əvvəlcə z=10, sonra y=z, sonra x=y olur.

console.log(x, y, z); // ✅ 10 10 10
```
#### 4.11.1 Əməliyyatla Birlikdə Mənimsətmə (`+=`, `-=`, `*=`, ...)
Bu operatorlar, bir riyazi əməliyyatı və mənimsətməni birləşdirən qısayollardır.
`a += b;` ifadəsi, `a = a + b;` ifadəsinə demək olar ki, ekvivalentdir.

**Nümunələr:**
```javascript
let total = 100;
const tax = 18;
total += tax; // total = total + tax;
console.log(total); // ✅ 118

let message = "Salam";
message += ", Dünya!"; // message = message + ", Dünya!";
console.log(message); // ✅ "Salam, Dünya!"

let score = 10;
score *= 2; // score = score * 2;
console.log(score); // ✅ 20
```

***
### 4.12 Qiymətləndirmə İfadələri (Evaluation Expressions)
JavaScript-in ən güclü, lakin eyni zamanda ən təhlükəli xüsusiyyətlərindən biri, sətir (string) şəklində olan JavaScript kodunu icra etmək bacarığıdır. Bu, qlobal **`eval()`** funksiyası ilə edilir.

❗️ **ÇOX VACİB TƏHLÜKƏSİZLİK XƏBƏRDARLIĞI** ☢️
`eval()` çox güclü, amma bir o qədər də **təhlükəlidir**.
* **Təhlükəsizlik Boşluğu:** İstifadəçidən və ya hər hansı bir kənar mənbədən gələn mətni heç vaxt `eval()`-a ötürməyin. Bu, hakerin sizin saytınızda istənilən kodu işə salmasına imkan verən böyük bir təhlükəsizlik boşluğudur.
* **Performans:** JavaScript mühərrikləri (engines), içində `eval()` olan funksiyaları optimallaşdıra bilmir, bu da kodun sürətini kəskin şəkildə aşağı salır.
**Nəticə:** Peşəkar kodda demək olar ki, **heç vaxt** istifadə edilmir. Ondan uzaq durun!

#### `eval()`-ın növləri: Lokal və Qlobal
`eval()`-ın davranışı, onun necə çağırılmasından asılıdır:
* **Direkt `eval()` (Direct Eval):** `eval(...)` şəklində birbaşa çağırıldıqda, o, **hazırkı skopda (local scope)** işləyir. Yəni, olduğu funksiyanın daxili dəyişənlərini oxuya və dəyişə bilər.
* **Qeyri-direkt `eval()` (Indirect Eval):** Əgər `eval` başqa bir adla (məsələn, `let myEval = eval; myEval(...)`) çağırılarsa, o, **qlobal skopda (global scope)** işləyir və lokal dəyişənlərə toxuna bilmir.

**Nümunə:**
```javascript
const geval = eval; // `eval`-a başqa ad veririk (qeyri-direkt çağırış üçün)

let x = "qlobal", y = "qlobal";

function localEval() {
  let x = "lokal"; // Lokal `x`
  eval("x += ' (dəyişdirildi)';"); // Direkt `eval` lokal `x`-i dəyişir
  return x;
}

function globalEval() {
  let y = "lokal"; // Lokal `y`
  geval("y += ' (dəyişdirildi)';"); // Qeyri-direkt `eval` qlobal `y`-i dəyişir
  return y; // Lokal `y` dəyişməz qalır
}

console.log("Lokal eval nəticəsi:", localEval(), " | Qlobal x:", x);
// ✅ Nəticə: Lokal eval nəticəsi: lokal (dəyişdirildi)  | Qlobal x: qlobal

console.log("Qlobal eval nəticəsi:", globalEval(), "| Qlobal y:", y);
// ✅ Nəticə: Qlobal eval nəticəsi: lokal | Qlobal y: qlobal (dəyişdirildi)
```

---
### 4.13 Müxtəlif Operatorlar (Miscellaneous Operators)
Bu bölmədə, digər kateqoriyalara tam sığmayan, amma çox faydalı olan bir neçə operatorla tanış olacağıq.

#### 4.13.1 Şərt Operatoru (Conditional Operator `?:`)
Bu, JavaScript-dəki yeganə **ternar (üç operandlı)** operatordur. `if/else` ifadəsinin qısa bir alternativdir.
**Sintaksis:** `şərt ? dəyər_eğer_doğrudursa : dəyər_eğer_yanlışdırsa`

```javascript
const age = 20;
const status = (age >= 18) ? "Yetkin" : "Yeniyetmə";
console.log(status); // ✅ "Yetkin"
```

#### 4.13.2 "Nullish Coalescing" Operatoru (`??`)
Bu ES2020 ilə gələn çox faydalı bir operatordur. `||` (OR) operatoruna bənzəyir, amma ondan daha ağıllıdır.
* `||` sol tərəfdəki dəyər **falsy** (`0`, `""`, `false`, `null`, `undefined`) olduqda sağ tərəfi qaytarır.
* `??` isə yalnız sol tərəfdəki dəyər **`null` və ya `undefined`** olduqda sağ tərəfi qaytarır.

Bu, `0` və ya boş sətir `""` kimi dəyərlərin keçərli olduğu hallarda çox vacibdir.

**Nümunə:**
```javascript
let userSettings = { volume: 0, theme: "" };

// `||` ilə səhv nəticə
const volume_or = userSettings.volume || 50; // volume 0 (falsy) olduğu üçün 50 götürülür
console.log("`||` ilə səs səviyyəsi:", volume_or); // ❌ 50

// `??` ilə düzgün nəticə
const volume_qq = userSettings.volume ?? 50; // volume 0, null və ya undefined olmadığı üçün elə 0 götürülür
console.log("`??` ilə səs səviyyəsi:", volume_qq); // ✅ 0
```

#### 4.13.3 `typeof` Operatoru
Bir dəyərin tipini sətir (string) olaraq qaytaran unar bir operatordur.
```javascript
console.log(typeof 42);          // ✅ "number"
console.log(typeof "salam");     // ✅ "string"
console.log(typeof true);        // ✅ "boolean"
console.log(typeof {});          // ✅ "object"
console.log(typeof []);          // ✅ "object" (massivlər də obyektdir)
console.log(typeof function(){}); // ✅ "function"
console.log(typeof undefined);   // ✅ "undefined"
console.log(typeof null);        // ❗️ "object" (bu, JavaScript-in tarixi bir səhvidir)
```
#### 4.13.4 `delete` Operatoru
Bir obyektin xüsusiyyətini və ya bir massivin elementini silir.
```javascript
const person = { name: "Ayan", city: "Bakı" };
delete person.city;
console.log(person); // ✅ { name: "Ayan" }

const numbers = [10, 20, 30];
delete numbers[1];
console.log(numbers); // ✅ [10, <1 empty item>, 30] (elementi silir, amma massivin uzunluğu dəyişmir)
```
#### 4.13.5 `await` Operatoru
Bu operator, `Promise`-lərlə işləmək üçün nəzərdə tutulub və asinxron funksiyaların (`async function`) daxilində istifadə olunur. Fəsil 13-də detallı şəkildə araşdırdıq.

#### 4.13.6 `void` Operatoru
Nadir hallarda istifadə olunur. Öz operandını qiymətləndirir, amma nəticəsini nəzərə almadan həmişə `undefined` qaytarır. `void 0` yazılışı, `undefined`-ın dəyişdirilmə ehtimalı olan köhnə mühitlərdə, 100% `undefined` dəyəri almaq üçün istifadə olunurdu.

#### 4.13.7 Vergül Operatoru (`,`)
Vergül operatoru, sol tərəfindəki ifadəni qiymətləndirir, nəticəsini atır, sonra sağ tərəfdəki ifadəni qiymətləndirir və onun nəticəsini qaytarır. Yeganə praktik istifadə yeri, `for` dövründə birdən çox dəyişəni eyni anda idarə etməkdir.
```javascript
for (let i = 0, j = 10; i <= 10; i++, j--) {
  console.log(`i=${i}, j=${j}`);
}
```