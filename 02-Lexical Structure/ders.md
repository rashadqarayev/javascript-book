# Chapter 2. Leksikal Struktur

Proqramlaşdırma dilinin **leksikal strukturu** — sənin proqramı həmin dildə necə yazmalı olduğunu izah edən qaydalar toplusudur. Bu dilin ən aşağı səviyyəli sintaksisidir.  
Yəni dəyişənlərin adlandırılması, kommentlərin necə yazılması, sətrlərin bir-birindən necə ayrılması və s. kimi texniki detallar buraya daxildir.

Bu bölmədə aşağıdakı hissələri öyrənəcəyik:

- Boşluqlar, dəyişən adları və sətir boşluqları  
- Kommentlər  
- Literallar  
- Təyinedicilər və rezerv olunmuş sözlər  
- Unicode  
- Semikolonlar  

---

## 2.1 JavaScript proqramının mətni

JavaScript **case-sensitive** dildir. Yəni `myVariable`, `MyVariable`, və `MYVARIABLE` — fərqli adlardır. Buna görə də dəyişən, funksiya və digər adlandırmalarda hərf ölçüsünə diqqət etmək çox vacibdir.

```js
let onLine, OnLine, Online, ONLINE;
// Bunlar dörd fərqli dəyişəndir
```

JavaScript yazılış zamanı **boşluqlara və sətir keçidlərinə çox da əhəmiyyət vermir**. Bu isə bizə kodu istədiyimiz kimi formatlamaq imkanı verir.

---

## 2.2 Kommentlər

Kodda şərh yazmaq üçün **kommentlər** istifadə olunur. Bunlar iki cürdür:

```js
// Bu tək sətirlik kommentdir

/*
 * Bu isə çoxsətirli kommentdir
 * Belə yazılır
 */

let x = 5; // bu da sətirin sonunda yazılmış kommentdir
```

Kommentlər kodun işləməsinə təsir etmir, amma **oxunaqlılığı və başa düşülməni artırmaq üçün** çox vacibdir.

---

## 2.3 Literallar

**Literal** — proqram daxilində birbaşa yazılmış sabit dəyərdir.

```js
42            // tam ədəd
3.14          // onluq ədəd
"Hello"       // string
'World'       // başqa string
true, false   // boolean dəyərlər
null          // obyektin olmaması
```

Literallar dəyişkənlərə mənalı məlumat təyin etmək üçün istifadə olunur.

---

## 2.4 Təyinedicilər və rezerv olunmuş sözlər

**Təyinedici (identifier)** sadəcə bir addır. Bu ad dəyişənə, funksiyaya, class-a və s. verilir.

Təyinedici aşağıdakı qaydalara uyğun yazılmalıdır:

- Rəqəmlə başlaya bilməz
- `$`, `_` və ya hərf ilə başlamalıdır
- Unicode simvolları da istifadə oluna bilər (məsələn: `π` və ya `sí`)

```js
let i, my_variable, v13, _dummy, $value;
```

### Rezerv olunmuş sözlər

JavaScript-də bəzi sözlər var ki, onlar artıq müəyyən məqsədlərlə istifadə olunur və **təkrar təyinedici kimi istifadə edilə bilməz**.

Misal üçün:

```
as, await, break, case, catch, class, const, continue, debugger, default,
delete, do, else, export, extends, false, finally, for, from, function,
if, import, in, instanceof, let, new, null, return, static, super, switch,
this, throw, true, try, typeof, var, void, while, with, yield
```

**Gələcəkdə istifadə üçün rezerv edilmiş sözlər**:

```
enum, implements, interface, package, private, protected, public
```

> `arguments` və `eval` kimi sözlər də təyinedici kimi istifadəyə qadağandır.

Bəzən bu sözlər obyekt açarları kimi istifadə oluna bilər:

```js
const obj = {
  return: "qayıt",
  class: "sinif"
};
```

---

## 2.5 Unicode

JavaScript Unicode istifadə etdiyi üçün istənilən dilin hərfləri ilə yazıla bilər:

```js
const π = 3.1415;
const привет = "salam";
const 变量 = "dəyişən";
```

Unicode sayəsində dəyişən adlarında və string-lərdə müxtəlif dillərdən və simvollardan istifadə mümkündür. Ancaq **oxunaqlılığı qorumaq** üçün buna ehtiyatla yanaşmaq lazımdır.

---

## 2.6 Semikolonlar

Semikolon (`;`) ifadələrin bir-birindən **dəqiq ayrılması üçün** istifadə olunur.

```js
let x = 10;
let y = 20;
console.log(x + y);
```

Əgər biz semikolon qoymasaq, bəzi hallarda JavaScript **avtomatik olaraq semikolon əlavə edir**. Amma bu həmişə düzgün nəticə vermir.

Məsələn:

```js
let a
a
=
3
console.log(a)
```

JavaScript bunu belə oxuyacaq:

```js
let a; a = 3; console.log(a);
```

---

### Problemli nümunə:

```js
let y = x + f
(a + b).toString()
```

Burada JavaScript `f(a + b).toString()` hissəsini `f` funksiyasının çağırışı kimi anlayacaq. Halbuki bizim niyyətimiz bu deyildi.

Doğru yazılış:

```js
let y = x + f;
(a + b).toString();
```

---

### Avtomatik semikolon əlavə olunması

Aşağıdakı sözlər sətirin əvvəlində olarsa və dərhal sonra yeni sətir gəlirsə, JavaScript **avtomatik semikolon** əlavə edir:

- `return`
- `throw`
- `yield`
- `break`
- `continue`

Yanlış:

```js
return
true;
```

Doğru:

```js
return true;
```

---

### `++` və `--` operatorları

Bu operatorlar **prefix** və ya **postfix** formada yazılır:

```js
let i = 0;

i++; // postfix
++i; // prefix
```

Əgər bu operatoru ayrı sətirdə yazsaq və semikolon unutsaq, JavaScript səhv başa düşə bilər.

---

### Arrow funksiyalarda sətir yazılışı

Səhv yazım:

```js
const sum = ()
=> {}
```

Doğru yazım:

```js
const sum = () => {};
```

---

## 2.7 Yekun

Bu fəsildə JavaScript-in leksikal strukturunun əsaslarını öyrəndik:

- Kodda hansı simvollardan istifadə edilə biləcəyini
- Kommentlərin yazılış qaydalarını
- Literallar və təyinedici adlarının tələblərini
- Unicode dəstəyini
- Semikolonların vacibliyini

Növbəti fəslimizdə **primitiv tiplər və onların xüsusiyyətləri** ilə daha yaxından tanış olacağıq.
