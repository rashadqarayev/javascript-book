# Fəsil 10. Modullar (Modules) 📦

Modul proqramlaşdırma böyük kod bazalarını fərqli mənbələrdən gələn modullarla (modules) qurmaq və global ad sahəsini (global namespace) səliqəli saxlamaq üçündür. Bu, modulların (modules) bir-birinin kodunu təsadüfən dəyişməsinin qarşısını alır. 🛡️

Əvvəllər JavaScript-də daxili modul (module) dəstəyi yox idi. Proqramçılar modulluq (modularity) üçün siniflər (classes), obyektlər (objects) və closure-lardan (closures) istifadə edirdilər. Node.js-də yaranan `require()`-əsaslı modullar (modules) buna nümunədir. Lakin JavaScript dilinin özündə ES6 ilə **`import`** və **`export`** açar sözləri (keywords) təqdim olundu. Bu gün də JavaScript modulluğu (modularity) kod-bağlama (code-bundling) alətlərindən asılıdır.

Bu fəsil aşağıdakıları əhatə edir:
* Siniflər (classes), obyektlər (objects) və closure-larla (closures) modullar (modules).
* `require()` ilə Node modulları (modules).
* `export`, `import` və `import()` ilə ES6 modulları (modules).

---

### 10.1 Siniflər (Classes), Obyektlər (Objects) və Closure-lar (Closures) ilə Modullar (Modules) 💡

Siniflər (classes) metodları (methods) üçün bir növ modul (module) rolunu oynayır. Məsələn, Fəsil 9-dakı `Complex` sinfinin (class) `has()` metodu (method) digər siniflərin (classes) eyni adlı metodları (methods) ilə toqquşmur. Bunun səbəbi, hər sinfin (class) metodlarının (methods) öz müstəqil prototip obyektlərində (prototype objects) təyin olunmasıdır. Obyektlər (objects) kimi siniflər (classes) də modul (modular) davranır: bir obyektə (object) xüsusiyyət (property) əlavə etmək global ad sahəsinə (global namespace) təsir etmir. `Math` obyekti (object) buna yaxşı nümunədir.

Sinif və obyektlərlə (objects) modulluq (modularity) faydalıdır, lakin daxili implementasiya detallarını (implementation details) gizlətməyə imkan vermir. Məsələn, `BitSet` sinfində (class) `_valid()` kimi daxili köməkçi funksiyalar (utility functions) və ya `BitSet.bits` kimi implementasiya detalları (implementation details) gizlədilməlidir. 🙈

**Dərhal Çağırılan Funksiya İfadələri (IIFE)** ilə bu problemi həll edə bilərik. IIFE-lər daxili dəyişənləri (variables) və funksiyaları (functions) öz daxilində gizlədə bilər, modulun (module) ictimai API-sini (public API) isə funksiyanın (function) qaytardığı dəyər (return value) edə bilərik.

**Nümunə (BitSet üçün):**

```javascript
const BitSet = (function() {
    // Özəl detallar burada:
    function isValid(set, n) { /* ... */ }
    const BITS = new Uint8Array([1, 2, 4, 8, ...]); // Digər özəl sabitler
    // ...

    // İctimai hissəni qaytarırıq (məsələn, BitSet sinfi).
    return class BitSet extends AbstractWritableSet {
        // ... implementasiya ...
    };
}());
```

Bir neçə elementi olan modullar (modules) üçün bu yanaşma daha da faydalıdır. Aşağıdakı nümunə `mean()` və `stddev()` funksiyalarını (functions) ixrac edən (exports) kiçik bir statistika modulunu (statistics module) göstərir:

```javascript
const stats = (function() {
    const sum = (x, y) => x + y; // Özəl yardımçı funksiyalar (utility functions)
    const square = x => x * x;

    function mean(data) { /* ... */ } // İctimai funksiya
    function stddev(data) { /* ... */ } // İctimai funksiya

    return { mean, stddev }; // İctimai funksiyaları obyekt kimi ixrac edirik
}());

console.log(stats.mean([1, 3, 5, 7, 9]));    // => 5
console.log(stats.stddev([1, 3, 5, 7, 9]));  // => ~3.16 ✅
```

---

### 10.1.1 Closure-əsaslı Modulluğu (Modularity) Avtomatlaşdırmaq 🤖

JavaScript faylını (file) bu cür modul (module) halına gətirmək mexaniki bir prosesdir. Əsas odur ki, hansı dəyərlərin (values) ixrac (export) olunacağını bilək.

Kod-bağlama (code-bundling) alətləri (məsələn, webpack, Parcel) bu prinsiplə işləyir: faylları (files) IIFE-lərə (dərhal çağırılan funksiya ifadələri) bürüyür, qaytarma dəyərlərini (return values) izləyir və hər şeyi tək bir fayla (file) birləşdirir.

**Nümunə (Bağlama – Bundling):**

```javascript
const modules = {};
function require(moduleName) { return modules[moduleName]; }

modules["sets.js"] = (function() {
    const exports = {};
    // sets.js-in məzmunu: exports.BitSet = class BitSet { ... };
    return exports;
}());

modules["stats.js"] = (function() {
    const exports = {};
    // stats.js-in məzmunu: exports.mean = function(data) { ... }; exports.stddev = function(data) { ... };
    return exports;
}());
```

Bu cür paketlənmiş modullardan (modules) istifadə belə olur:

```javascript
const stats = require("stats.js");
const BitSet = require("sets.js").BitSet;

let s = new BitSet(100);
s.insert(10); s.insert(20); s.insert(30);
let average = stats.mean([...s]); // average = 20 ✅
```
Bu, Node.js-də istifadə olunan `require()` funksiyasına (function) bənzər bir sistemin necə işlədiyini göstərir.

---

### 10.2 Node.js-də Modullar (Modules) 🌳

Node.js proqramlaşdırmasında, kodun fayllara (files) bölünməsi normaldır. Bu fayllar (files) sürətli fayl sistemlərində (filesystems) yerləşir. Veb brauzerlərdən (web browsers) fərqli olaraq, Node.js-də proqramı tək bir JavaScript faylına (file) bağlamağa (bundling) ehtiyac və ya fayda yoxdur, çünki burada şəbəkə (network) yavaşlığı problemi olmur.

Node.js-də hər fayl (file) özəl ad sahəsi (private namespace) olan müstəqil bir moduldur (module). Bir faylda (file) təyin edilmiş sabitələr (constants), dəyişənlər (variables), funksiyalar (functions) və siniflər (classes) yalnız həmin fayla (file) özəldir, **əgər ixrac edilməsələr (exported).** Bir modul (module) tərəfindən ixrac edilən (exported) dəyərlər (values) isə yalnız başqa bir modul (module) onları açıq şəkildə **idxal (import)** etsə görünür.

Node.js modulları (modules) digər modulları (modules) `require()` funksiyası (function) ilə idxal (import) edir və ictimai API-lərini (public APIs) `exports` obyektinin (object) xüsusiyyətlərini (properties) təyin etməklə və ya `module.exports` obyektini (object) tamamilə dəyişdirməklə ixrac (export) edir.

---

### 10.2.1 Node.js İxracları (Exports) 📤

Node.js həmişə təyin olunmuş `exports` adlı global bir obyekt (object) təyin edir. Əgər Node.js modulu (module) bir neçə dəyər (value) ixrac edirsə (exports), onları sadəcə bu obyektin (object) xüsusiyyətlərinə (properties) mənimsədə bilərsiniz:

```javascript
const sum = (x, y) => x + y;
const square = x => x * x;

exports.mean = data => data.reduce(sum) / data.length; // ✅ mean funksiyasını ixrac edir
exports.stddev = function(d) {
    let m = exports.mean(d); // exports.mean istifadə edə bilirik
    return Math.sqrt(d.map(x => x - m).map(square).reduce(sum) / (d.length - 1));
};
```
Lakin, bəzən funksiyalar (functions) və ya siniflər (classes) toplusu əvəzinə yalnız bir funksiya (function) və ya sinif (class) ixrac etmək istəyirsiniz. Bunu etmək üçün ixrac etmək (export) istədiyiniz dəyəri (value) sadəcə `module.exports`-a mənimsədirsiniz:

```javascript
module.exports = class BitSet extends AbstractWritableSet {
    // ... implementasiya qısa olaraq göstərilməyib ...
}; // ✅ Sadəcə BitSet sinfini ixrac edir
```
`module.exports`-un defolt dəyəri (default value) `exports`-un istinad etdiyi eyni obyektdir (object). Yuxarıdakı `stats` modulunda (module) `mean` funksiyasını (function) `exports.mean` əvəzinə `module.exports.mean` olaraq da təyin edə bilərdik. Başqa bir yanaşma isə modulu (module) bitirdikdən sonra fərqli funksiyaları (functions) bir-bir ixrac (export) etmək əvəzinə, tək bir obyekt (object) ixrac etməkdir:

```javascript
// Bütün funksiyaları təyin edin: ictimai (public) və özəl (private).
const sum = (x, y) => x + y;
const square = x => x * x;
const mean = data => data.reduce(sum) / data.length;
const stddev = d => {
    let m = mean(d);
    return Math.sqrt(d.map(x => x - m).map(square).reduce(sum) / (d.length - 1));
};

// Yalnız ictimai olanları ixrac edin.
module.exports = { mean, stddev }; // ✅ mean və stddev-i bir obyekt kimi ixrac edir
```

---

### 10.2.2 Node.js İdxalları (Imports) 📥

Node.js modulu (module) digər modulu (module) `require()` funksiyasını (function) çağıraraq idxal (import) edir. Bu funksiyanın (function) arqumenti (argument) idxal ediləcək modulun (module) adıdır və qaytarma dəyəri (return value) isə həmin modulun (module) ixrac etdiyi dəyərdir (adətən bir funksiya (function), sinif (class) və ya obyekt (object)).

Node.js-ə daxil olan sistem modulunu (module) və ya paket meneceri (package manager) vasitəsilə sisteminizə quraşdırdığınız modulu (module) idxal (import) etmək istəyirsinizsə, sadəcə modulun (module) kvalifikasiyasız adını (unqualified name) istifadə edin, fayl sistemi yolu (filesystem path) göstərməyin:

```javascript
// Node.js-ə daxil olan modullar (modules).
const fs = require("fs");       // Fayl sistemi modulu (module).
const http = require("http");   // HTTP modulu (module).

// Express HTTP server framework üçüncü tərəf moduludur (module).
// Node.js-in bir hissəsi deyil, lakin yerli olaraq quraşdırılıb.
const express = require("express");
```
Öz kodunuzun modulunu (module) idxal (import) etmək istədiyiniz zaman, modulun (module) adı cari modulun (module) faylına (file) nisbətən həmin kodu ehtiva edən faylın (file) yolu (path) olmalıdır. Mütləq yollar (absolute paths) ( `/` ilə başlayan) qanunidir, lakin proqramınızın bir hissəsi olan modulları (modules) idxal (import) edərkən adətən modul (module) adları cari qovluğa (directory) və ya üst qovluğa (parent directory) nisbətən `./` və ya `../` ilə başlayır. Məsələn:

```javascript
const stats = require('./stats.js');             // Cari qovluqdan (directory).
const BitSet = require('./utils/bitset.js');     // utils qovluğundan (directory).
```
(İdxal (import) etdiyiniz faylların (files) `.js` sonluğunu (suffix) atmaq da mümkündür, Node.js yenə də faylları (files) tapacaq, lakin bu fayl uzantılarının (file extensions) açıq şəkildə daxil edilməsi yaygındır.)

Bir modul (module) yalnız bir funksiya (function) və ya sinif (class) ixrac (export) etdikdə, sadəcə onu `require` etməlisiniz. Bir modul (module) bir neçə xüsusiyyəti (properties) olan bir obyekt (object) ixrac (export) etdikdə isə seçiminiz var: ya bütün obyekti (object) idxal (import) edə bilərsiniz, ya da yalnız istifadə etməyi planlaşdırdığınız xüsusi xüsusiyyətləri (properties) (destructuring assignment - strukturlaşdırıcı mənimsətmə ilə) idxal (import) edə bilərsiniz. Bu iki yanaşmanı müqayisə edin:

```javascript
// Bütün "stats" obyektini (object) bütün funksiyaları (functions) ilə birlikdə idxal (import) edin.
const stats = require('./stats.js');
// Ehtiyacımızdan daha çox funksiyamız (functions) var, lakin onlar səliqəli şəkildə
// rahat "stats" ad sahəsində (namespace) təşkil olunub.
let average = stats.mean(data);

// Alternativ olaraq, dəqiq istədiyimiz funksiyaları (functions) birbaşa yerli ad sahəsinə (namespace) idxal (import) etmək üçün
// idiomatik strukturlaşdırıcı mənimsətmədən (destructuring assignment) istifadə edə bilərik:
const { stddev } = require('./stats.js'); // ✅ Yalnız stddev funksiyasını idxal edir
// Bu gözəl və qısadır, baxmayaraq ki, stddev() funksiyası (function) üçün "stats" prefiksi (prefix)
// olmayan bir ad sahəsi (namespace) olduğu üçün bir az kontekst (context) itiririk.
let sd = stddev(data);
```

---

### 10.2.3 Vebdə (Web) Node.js-tərzli Modullar (Modules) 🌐

`Exports` obyekti (object) və `require()` funksiyası (function) olan modullar (modules) Node.js-ə daxildir. Lakin webpack kimi bir bağlama (bundling) aləti (tool) ilə kodunuzu emal etməyə hazırsınızsa, bu modul (module) stilini veb brauzerlərdə (web browsers) işləmək üçün nəzərdə tutulmuş kod üçün də istifadə etmək mümkündür. Son vaxtlara qədər bu, çox yaygın bir şey idi və siz hələ də çoxlu veb-əsaslı kodda bunu görə bilərsiniz.

Lakin, JavaScript-in artıq öz standart modul (module) sintaksisi (syntax) olduğu üçün, bağlayıcılardan (bundlers) istifadə edən proqramçılar `import` və `export` ifadələri (statements) ilə rəsmi JavaScript modullarından (modules) istifadə etməyə daha çox meyl edirlər. 🚀

---

### 10.3 ES6 Modulları (Modules) 🚀

ES6 (ECMAScript 2015) JavaScript dilinə **`import`** və **`export`** açar sözlərini (keywords) əlavə edərək nəhayət ki, əsas dil xüsusiyyəti kimi həqiqi modulluq (modularity) dəstəyini gətirdi. Konseptual olaraq, ES6 modulları (modules) Node.js modulları (modules) ilə eynidir: hər fayl (file) öz müstəqil moduludur (module). Bir fayl (file) daxilində təyin edilmiş sabitələr (constants), dəyişənlər (variables), funksiyalar (functions) və siniflər (classes) yalnız həmin modula (module) özəldir, **əgər açıq şəkildə ixrac (export) edilməyibsə.** Bir moduldan (module) ixrac edilən (exported) dəyərlər (values) isə yalnız onları açıq şəkildə idxal (import) edən digər modullarda (modules) istifadə edilə bilər. ES6 modulları (modules) Node.js modullarından (modules) idxal (import) və ixrac (export) üçün istifadə olunan sintaksis (syntax) ilə, həm də veb brauzerlərdə (web browsers) modulların (modules) necə təyin olunması ilə fərqlənir.

**ES6 Modullarının Adi JavaScript "Script"-lərindən Fərqləri:**

* **Modulluq (`Modularity`):** Ən əsas fərq modulluğun (modularity) özündədir. Adi script-lərdə (scripts), top-level (ən yüksək səviyyəli) dəyişən (variable), funksiya (function) və sinif (class) elanları (declarations) bütün script-lər (scripts) tərəfindən paylaşılan tək bir global kontekstə (context) daxil olur. Modullarda (modules) isə hər faylın (file) öz özəl konteksti (private context) var və onlar `import` və `export` ifadələrini (statements) istifadə edə bilirlər. Bu, kodun təmiz və toqquşmasız qalmasını təmin edir.
* **Sərt Rejim (`Strict Mode`):** ES6 modulu daxilindəki kod (istənilən ES6 sinif təyinində (class definition) olduğu kimi) avtomatik olaraq **sərt rejimdə (strict mode)** işləyir (bax §5.6.3). Bu o deməkdir ki, ` "use strict" ` direktivini yazmağa ehtiyac qalmır. Həmçinin, modul (module) daxilində `with` ifadəsi (statement), `arguments` obyekti (object) və ya elan olunmamış dəyişənlər (undeclared variables) istifadə edilə bilməz.
* **`this` dəyəri (`this value`):** Modullarda (modules) `this` dəyəri (value) hətta top-level (ən yüksək səviyyəli) kodda belə `undefined` olur. Bu, adi script-lərdən (scripts) fərqlidir, çünki veb brauzerlər (web browsers) və Node.js-də script-lər (scripts) `this`-i global obyektə (global object) təyin edir.

---

**ES6 Modulları (Modules) Vebdə (Web) və Node.js-də** 🌐

ES6 modulları (modules) illərdir vebdə (web) webpack kimi kod bağlayıcıları (code bundlers) vasitəsilə geniş şəkildə istifadə olunur. Bu bağlayıcılar (bundlers) müstəqil JavaScript modullarını (modules) veb səhifələrə (web pages) daxil edilə bilən böyük, qeyri-modul (non-modular) paketlərə (bundles) birləşdirir. Hazırda (2025-ci ilin iyun ayı etibarilə) ES6 modulları (modules) Internet Explorer istisna olmaqla, bütün müasir veb brauzerlər (web browsers) tərəfindən yerli (natively) olaraq dəstəklənir. Vebdə (web) yerli olaraq istifadə edildikdə, ES6 modulları (modules) HTML səhifələrə `<script type="module">` xüsusi teqi (tag) ilə əlavə olunur (daha sonra bu fəsildə ətraflı izah olunacaq).

JavaScript modulluğunun (modularity) qabaqcılı olan Node.js isə özünü iki tam uyğun olmayan modul (module) sistemini dəstəkləmək kimi bir vəziyyətdə tapdı. Node.js 13-dən etibarən ES6 modulları (modules) dəstəklənir, lakin hələlik Node.js proqramlarının (programs) böyük əksəriyyəti köhnə Node.js modullarını (modules) (CommonJS) istifadə etməyə davam edir.

---

### 10.3.1 ES6 İxracları (Exports) 📤

Bir sabit (constant), dəyişən (variable), funksiya (function) və ya sinfi (class) ES6 modulundan (module) ixrac (export) etmək üçün müxtəlif yollar var:

1.  **Birbaşa Elanla İxrac (`Named Exports`):** Sadəcə elanın (declaration) qarşısına **`export`** açar sözünü (keyword) əlavə edin. Bu, hər bir ixracı (export) öz adı ilə təmin edir:
    ```javascript
    export const PI = Math.PI; // ✅ PI sabitini ixrac edir
    export function degreesToRadians(d) { return d * PI / 180; } // ✅ Funksiyanı ixrac edir
    export class Circle { // ✅ Sinifi ixrac edir
        constructor(r) { this.r = r; }
        area() { return PI * this.r * this.r; }
    }
    ```

2.  **Toplu İxrac (`Export List`):** Modulunuzda (module) `export` açar sözlərini (keywords) səpmək əvəzinə, sabitlərinizi (constants), dəyişənlərinizi (variables), funksiyalarınızı (functions) və siniflərinizi (classes) normal şəkildə təyin edə bilərsiniz. Daha sonra (adətən modulun (module) sonunda) bir `export` ifadəsi (statement) ilə dəqiq nəyin ixrac (export) edildiyini bir yerdə elan edə bilərsiniz:
    ```javascript
    // Yuxarıdakı üç ayrı ixrac (export) əvəzinə:
    export { Circle, degreesToRadians, PI }; // ✅ Hər üç elementi bir yerdə adları ilə ixrac edir
    ```
    Bu sintaksis (syntax) fiqurlu mötərizələr (curly braces) içində vergüllə (comma) ayrılmış identifikatorların (identifiers) siyahısını tələb edir və obyekt literalı (object literal) yaratmır.

3.  **Varsayılan İxrac (`Default Export` - `export default`):** Modullar (modules) adətən yalnız bir dəyər (value) (çox vaxt bir funksiya (function) və ya sinif (class)) ixrac (export) edir. Bu hallarda **`export default`** açar sözünü (keyword) istifadə etmək idxal (import) prosesini asanlaşdırır:
    ```javascript
    export default class BitSet { // ✅ Bu sinfi (class) modulun (module) varsayılan (default) ixracı (export) kimi təyin edir
        // ... implementasiya qısa olaraq göstərilməyib ...
    }
    ```
    `export default` adi `export`-dan fərqli olaraq, adı olmayan ifadələri (expressions) (məsələn, anonim funksiya ifadələri (anonymous function expressions) və anonim sinif ifadələri (anonymous class expressions)) ixrac (export) edə bilər. Əgər `export default` sonrası fiqurlu mötərizələr (curly braces) görsəniz, bu, ixrac (export) edilən bir obyekt literalidir (object literal). Bir modulun (module) yalnız bir varsayılan (default) ixracı (export) ola bilər.

**Qeyd:** `export` açar sözü (keyword) yalnız JavaScript kodunuzun top-level (ən yüksək səviyyəsində) görünə bilər. Sinif (class), funksiya (function), dövr (loop) və ya şərt (conditional) daxilindən dəyər (value) ixrac (export) edə bilməzsiniz. Bu, ES6 modul (module) sisteminin vacib bir xüsusiyyətidir və statik analizə (static analysis) imkan verir.

---

### 10.3.2 ES6 İdxalları (Imports) 📥

Digər modullar (modules) tərəfindən ixrac (export) edilmiş dəyərləri (values) **`import`** açar sözü (keyword) ilə daxil edirsiniz:

1.  **Varsayılan İdxal (`Default Import`):** `export default` ilə ixrac (export) edilmiş dəyəri (value) idxal (import) etmək üçün:
    ```javascript
    import BitSet from './bitset.js'; // ✅ './bitset.js' modulundan varsayılan (default) ixracı (export) BitSet adı ilə idxal edir
    ```
    İdxal edilən dəyər (value) `const` kimi sabitdir (constant) və modulun (module) istənilən yerində əlçatandır (hoisting sayəsində).

2.  **Adlandırılmış İdxallar (`Named Imports`):** Bir neçə adlandırılmış (named) ixracı (export) olan moduldan (module) dəyərləri (values) idxal (import) etmək:
    ```javascript
    import { mean, stddev } from "./stats.js"; // ✅ "./stats.js" modulundan mean və stddev funksiyalarını adları ilə idxal edir
    ```
    Buradakı fiqurlu mötərizələr (curly braces) strukturlaşdırıcı mənimsətməyə (destructuring assignment) bənzəyir və ixrac edilmiş dəyərlərin (exported values) adlarını göstərir.

3.  **Bütün İxracları İdxal (`Namespace Import`):** Modulun (module) bütün adlandırılmış (named) ixraclarını (exports) bir obyekt (object) kimi idxal (import) etmək və bir ad sahəsi (namespace) altında birləşdirmək:
    ```javascript
    import * as stats from "./stats.js"; // ✅ Bütün ixracları "stats" obyekti (object) olaraq idxal edir
    // İstifadəsi: stats.mean(data); stats.stddev(data);
    ```

4.  **Həm Varsayılan, Həm Adlandırılmış İdxal:** Həm `export default`, həm də adlandırılmış (named) ixracları (exports) olan modullardan (modules) idxal (import) etmək:
    ```javascript
    import Histogram, { mean, stddev } from "./histogram-stats.js"; // ✅ Histogramı varsayılan (default), mean və stddev-i adlandırılmış (named) olaraq idxal edir
    ```

5.  **İxracsız Modul İdxalı (`Side-Effect Import`):** Heç bir dəyər (value) ixrac (export) etməyən, lakin kodun işlənməsi ilə yan təsir (side effect) göstərən modulu (module) daxil etmək:
    ```javascript
    import "./analytics.js"; // ✅ Yalnız modulun (module) kodunu işə salır (məsələn, hadisə dinləyicilərini (event listeners) qeydiyyatdan keçirir)
    ```
    Bu modul (module) ilk dəfə idxal (import) edildikdə işə düşür (sonrakı idxallar (imports) heç nə etmir).

**Ümumi Qaydalar:**
* İdxallar (imports) yalnız modulun (module) top-level (ən yüksək səviyyəsində) yerləşə bilər.
* Modulun (module) yolu (path) tək və ya ikiqat dırnaqda (quotes) sabit simvol literal (string literal) olmalıdır (`./util.js` kimi). Dəyişən (variable) və ya template literal (şablon literal) istifadə edilə bilməz.
* Modul spesifikasiya simvolu (module specifier string) mütləq (`/` ilə başlayan), nisbi (`./` və ya `../` ilə başlayan) yol, yaxud tam URL (`protocol` və `hostname` ilə) olmalıdır. Kvalifikasiyasız (unqualified) adlara ("util.js" kimi) icazə verilmir (baxmayaraq ki, bəzi bağlayıcılar (bundlers) bunu dəstəkləyir).

---

### 10.3.3 Adlandırma (Renaming) ilə İdxallar (Imports) və İxraclar (Exports) 🔄

Modullar (modules) arasında ad toqquşmalarının (name conflicts) qarşısını almaq, yaxud adları daha aydın etmək üçün **`as` açar sözünü (keyword)** istifadə edərək idxal (import) və ya ixrac (export) zamanı dəyərləri (values) yenidən adlandıra (rename) bilərsiniz:

1.  **İdxalda Yenidən Adlandırma (`Renaming on Import`):**
    ```javascript
    import { render as renderImage } from "./imageutils.js"; // ✅ render funksiyasını renderImage adı ilə idxal edir
    import { render as renderUI } from "./ui.js";             // ✅ render funksiyasını renderUI adı ilə idxal edir
    ```
    *Qeyd:* Varsayılan (default) ixracı (export) adlandırmaq üçün də bu sintaksis (syntax) istifadə oluna bilər:
    ```javascript
    import { default as Histogram } from "./histogram.js"; // ✅ Varsayılan (default) ixracı (export) Histogram adı ilə idxal edir
    ```

2.  **İxracda Yenidən Adlandırma (`Renaming on Export`):** Yalnız toplu (list) ixrac (export) sintaksisində (syntax) mümkündür.
    ```javascript
    export { // ✅ layout-u calculateLayout, render-i renderLayout olaraq ixrac edir
        layout as calculateLayout,
        render as renderLayout
    };
    ```
    Unutmayın ki, `as` açar sözündən (keyword) əvvəl yalnız bir identifikator (identifier) olmalıdır, ifadə (expression) deyil. Məsələn, `export { Math.sin as sin };` sintaksis səhvinə (SyntaxError) səbəb olar.

---

### 10.3.4 Yenidən İxraclar (Re-Exports) 🔄

Bəzən funksiyaları (functions) və ya sinifləri (classes) ayrı-ayrı modullarda (modules) saxlayıb, lakin onların hamısını bir yerdən idxal etməyə imkan verən ümumi bir "paket" modulu (module) təklif etmək istəyirik.

**Nümunə:** Təsəvvür edin ki, `mean()` funksiyası (function) `"./stats/mean.js"`-də, `stddev()` isə `"./stats/stddev.js"`-dədir. Hər ikisini `"./stats.js"` modulundan (module) əlçatan etmək üçün əvvəlcə idxal (import) edib, sonra yenidən ixrac (export) edə bilərik:

```javascript
import { mean } from "./stats/mean.js";
import { stddev } from "./stats/stddev.js";
export { mean, stddev }; // Mean və Stddev-i yenidən ixrac edir
```

ES6 modulları (modules) bu ssenari üçün xüsusi bir sintaksis (syntax) təqdim edir: **"yenidən ixrac" (re-export)**. Bu, idxal (import) və ixrac (export) addımlarını (steps) tək bir ifadədə (statement) birləşdirir:

```javascript
export { mean } from "./stats/mean.js";   // ✅ mean-i birbaşa yenidən ixrac edir
export { stddev } from "./stats/stddev.js"; // ✅ stddev-i birbaşa yenidən ixrac edir
```
Burada `mean` və `stddev` adları (names) birbaşa istifadə olunmur, sadəcə yenidən ixrac edilir.

**Bütün adlandırılmış (named) ixracları (exports) yenidən ixrac etmək:**
Seçici olmaq istəmiriksə, bir moduldan (module) bütün adlandırılmış (named) ixracları (exports) wildcard (`*`) ilə yenidən ixrac (re-export) edə bilərik:

```javascript
export * from "./stats/mean.js";   // ✅ mean.js-dən bütün adlandırılmış (named) ixracları yenidən ixrac edir
export * from "./stats/stddev.js"; // ✅ stddev.js-dən bütün adlandırılmış (named) ixracları yenidən ixrac edir
```

**Yenidən adlandırma (`Renaming`) ilə Yenidən İxrac:**
Yenidən ixrac (re-export) sintaksisi (syntax) adi `import` və `export` ifadələrində (statements) olduğu kimi `as` açar sözü (keyword) ilə yenidən adlandırmaya (renaming) imkan verir. Məsələn, `mean()` funksiyasını (function) yenidən ixrac (re-export) edib ona `average()` adlı başqa bir ad da vermək:

```javascript
export { mean, mean as average } from "./stats/mean.js";
export { stddev } from "./stats/stddev.js";
```

**Varsayılan (Default) İxracları (Exports) Yenidən İxrac Etmək:**
Əgər modullar (modules) `export default` ilə funksiyalarını (functions) ixrac edibsə (bu, tək ixracı (export) olan modullar (modules) üçün tez-tez rast gəlinən haldır), o zaman yenidən ixrac (re-export) sintaksisi (syntax) adsız varsayılan (default) ixraclara (exports) ad təyin etməyi tələb edir:

```javascript
export { default as mean } from "./stats/mean.js";   // ✅ Varsayılan (default) ixracı (export) mean adı ilə yenidən ixrac edir
export { default as stddev } from "./stats/stddev.js"; // ✅ Varsayılan (default) ixracı (export) stddev adı ilə yenidən ixrac edir
```

**Adlandırılmış Simvolu (Symbol) Varsayılan (Default) İxrac Kimi Yenidən İxrac Etmək:**
Başqa bir moduldan (module) adlandırılmış (named) bir simvolu (symbol) öz modulunuzun (module) varsayılan (default) ixracı (export) kimi yenidən ixrac (re-export) edə bilərsiniz:

```javascript
export { mean as default } from "./stats.js"; // ✅ mean funksiyasını bu modulun (module) varsayılan (default) ixracı (export) edir
```
Həmçinin, başqa bir modulun (module) varsayılan (default) ixracını (export) öz modulunuzun (module) varsayılan (default) ixracı (export) kimi yenidən ixrac (re-export) etmək də mümkündür:

```javascript
export { default } from "./stats/mean.js"; // ✅ './stats/mean.js' modulunun varsayılan (default) ixracını (export) yenidən ixrac edir
```

---

### 10.3.5 Vebdə (Web) JavaScript Modulları (Modules) 🌐

Əvvəlki bölmələrdə ES6 modulları (modules) abstrakt şəkildə izah edildi. İndi onların veb brauzerlərində (web browsers) necə işlədiyini müzakirə edəcəyik. (Əgər veb proqramlaşdırma təcrübəniz yoxdursa, Fəsil 15-i oxuduqdan sonra daha yaxşı başa düşə bilərsiniz.)

2020-ci ilin əvvəlləri etibarilə, ES6 modulları (modules) üçün istehsal (production) kodu adətən webpack kimi alətlərlə (tools) paketlənirdi (bundled). Bu, ümumilikdə daha yaxşı performans (performance) verir, lakin gələcəkdə şəbəkə sürətləri artdıqca və brauzerlər (browsers) modulları (modules) optimallaşdırdıqca dəyişə bilər.

İnkişaf (development) zamanı artıq paketləmə (bundling) alətləri (tools) məcburi deyil, çünki müasir brauzerlərin (browsers) hamısı JavaScript modulları (modules) üçün yerli (native) dəstək verir. Modullar (modules) defolt olaraq sərt rejimdə (strict mode) işləyir, `this` global obyektə (global object) istinad etmir və top-level (ən yüksək səviyyəli) elanlar (declarations) global olaraq paylaşılmır. Modulları (modules) adi koddan (code) fərqli icra etmək lazım olduğundan, HTML-də də dəyişikliklər tələb olunur. Veb brauzerində (web browser) yerli (natively) `import` direktivlərini (directives) istifadə etmək üçün kodunuzun bir modul (module) olduğunu `<script type="module">` teqi (tag) ilə bildirməlisiniz.

```html
<script type="module">
    import "./main.js";
</script>
```
ES6 modullarının (modules) bir üstünlüyü, hər modulun (module) statik (static) idxallar (imports) dəstinə malik olmasıdır. Bu, brauzerə (browser) proqramı (program) tam yükləmək üçün lazım olan bütün əlaqəli modulları (modules) ardıcıl yükləməyə imkan verir. `<script type="module">` teqi (tag) modulyar (modular) proqramın (program) başlanğıc nöqtəsini qeyd edir. Onun idxal etdiyi (imported) modullar (modules) `<script>` teqlərində (tags) olmur, əvəzinə tələb olunduqda adi JavaScript faylları (files) kimi yüklənir və sərt rejimdə (strict mode) icra edilir.

Inline `<script type="module">` daxilindəki kod da bir ES6 moduludur (module) və `export` istifadə edə bilər. Lakin bunun bir mənası yoxdur, çünki inline modulları (modules) adlandırmaq üçün HTML sintaksisi (syntax) yoxdur, buna görə də başqa modul (module) onu idxal (import) edə bilməz.

`type="module"` atributu (attribute) olan script-lər (scripts) `defer` atributu (attribute) olan script-lər (scripts) kimi yüklənir və icra edilir: kodun yüklənməsi HTML ayrıştırması (parsing) zamanı başlasa da, icra HTML ayrıştırması (parsing) tamamlanana qədər gözləyir. Sonra script-lər (həm modulyar (modular), həm də qeyri-modulyar (non-modular)) HTML sənədində (document) göründükləri ardıcıllıqla icra edilir.

Modulların (modules) icra vaxtını (execution time) `async` atributu (attribute) ilə dəyişə bilərsiniz. `async` modulu (module) kod yüklənən kimi icra olunur, hətta HTML ayrıştırması (parsing) tamamlanmasa belə.

**Brauzer Uyğunluğu (`Browser Compatibility`):**
`<script type="module">` dəstəkləyən brauzerlər (browsers) `<script nomodule>`-u da dəstəkləməlidir. Modul-bilikli (module-aware) brauzerlər (browsers) `nomodule` atributu (attribute) olan script-ləri (scripts) nəzərə almır, lakin modulları (modules) dəstəkləməyən brauzerlər (browsers) onu nəzərə alıb script-i (script) işə salır. Bu, köhnə brauzerlər (browsers) üçün fallback (geri düşmə) təmin etmək üçün güclü bir texnikadır. Məsələn, IE11 üçün Babel və webpack ilə kodu (code) qeyri-modulyar (non-modular) ES5 koduna (code) çevirib `<script nomodule>` ilə yükləyə bilərsiniz.

Adi script-lər (scripts) və modul script-lər (module scripts) arasındakı başqa bir fərq **cross-origin yüklənmə (cross-origin loading)** ilə bağlıdır. Adi `<script>` teqi (tag) istənilən serverdən (server) kod yükləyə bilsə də, `<script type="module">` təhlükəsizliyi artırır: modullar (modules) yalnız eyni mənşədən (origin) yüklənə bilər və ya düzgün CORS başlıqları (headers) olmalıdır. Bu, inkişaf (development) zamanı `file:` URL-ləri ilə test etməyi çətinləşdirir; statik veb serveri (web server) qurmağınız lazım gələ bilər.

Bəzi proqramçılar (programmers) modul (modular) fayllarını (files) adi `.js` fayllarından (files) fərqləndirmək üçün `.mjs` fayl uzantısını (filename extension) istifadə edirlər. Veb brauzerlər (web browsers) üçün fayl uzantısı (file extension) əhəmiyyətli deyil (MIME tipi (type) vacibdir), lakin Node.js fayl uzantısından (file extension) modul (module) sistemini fərqləndirmək üçün bir işarə (hint) kimi istifadə edir. Beləliklə, Node.js ilə istifadə olunacaq ES6 modulları (modules) yazırsınızsa, `.mjs` konvensiyasını (convention) qəbul etmək faydalı ola bilər.

---

### 10.3.6 Dinamik İdxallar (`Dynamic Imports`) `import()` ilə ⚡

ES6 `import` və `export` direktivləri (directives) statikdir; onlar modul (module) yüklənməzdən əvvəl əlaqələri təyin etməyə imkan verir. Bu o deməkdir ki, statik (statically) idxal (import) edilmiş dəyərlər (values) kodunuz işə düşməzdən əvvəl hazır olur.

Vebdə (web), kod şəbəkə (network) üzərindən yüklənir və tez-tez mobil cihazlarda (mobile devices) icra edilir. Bu cür mühitdə, bütün proqramın (program) işə düşməzdən əvvəl yüklənməsini tələb edən statik (static) modul (module) idxalları (imports) həmişə səmərəli olmur. Veb tətbiqləri (web applications) adətən əvvəlcə yalnız ilk səhifəni göstərmək üçün lazım olan kodu (code) yükləyir, sonra istifadəçi digər məzmunla (content) qarşılıqlı əlaqədə (interact) olduqca daha böyük kodları (codes) dinamik (dynamically) yükləyir.

ES2020-də **`import()`**-un tətbiqi ilə bu dəyişdi (2020-ci ilin əvvəlləri etibarilə, dinamik idxal (dynamic import) ES6 modullarını (modules) dəstəkləyən bütün brauzerlər (browsers) tərəfindən dəstəklənir). `import()`-a bir modul (module) spesifikatoru (specifier) ötürürsünüz və o, modulun (module) yüklənməsi və işə düşməsinin (running) asinxron (asynchronous) prosesini (process) təmsil edən bir **Promise** obyekti (object) qaytarır. Dinamik idxal (dynamic import) tamamlandıqda, Promise "yerinə yetirilir" (fulfilled) (bax Fəsil 13) və statik `import * as` forması ilə əldə edəcəyinizə bənzər bir obyekt (object) yaradır.

**Nümunə:** `"./stats.js"` modulunu (module) statik (statically) əvəzinə dinamik (dynamically) idxal (import) edib istifadə etmək:

```javascript
import("./stats.js").then(stats => { // ✅ Modulu (module) asinxron (asynchronous) yükləyir
    let average = stats.mean(data);
});
```
Və ya `async` funksiyasında (function) (`async/await` haqqında Fəsil 13-də ətraflı var):

```javascript
async function analyzeData(data) {
    let stats = await import("./stats.js"); // ✅ Modulu (module) asinxron (asynchronous) yükləyir və Promise-in tamamlanmasını gözləyir
    return {
        average: stats.mean(data),
        stddev: stats.stddev(data)
    };
}
```
`import()`-a ötürülən arqument (argument) statik `import` direktivində (directive) istifadə edəcəyiniz kimi bir modul (module) spesifikatoru (specifier) olmalıdır. Lakin `import()` ilə sabit simvol literal (constant string literal) istifadə etmək məhdudiyyəti yoxdur; düzgün formada simvola (string) çevrilən hər hansı bir ifadə (expression) kifayət edəcək.

Dinamik `import()` bir funksiya çağırışına (function invocation) bənzəsə də, əslində funksiya (function) deyil, bir operatordur (operator). Parentheses (mörtərizələr) operator (operator) sintaksisinin (syntax) məcburi hissəsidir.

Dinamik `import()` yalnız veb brauzerlər (web browsers) üçün deyil, webpack kimi kod-paketləmə (code-packaging) alətləri (tools) tərəfindən də yaxşı istifadə edilə bilər. `import()` çağırışlarını (calls) strategik şəkildə istifadə edərək, böyük bir monolitik (monolithic) paketi (bundle) tələb olunduqda yüklənə bilən kiçik paketlərə (bundles) bölə bilərsiniz.

---

### 10.3.7 `import.meta.url` ℹ️

ES6 modul (module) sisteminin son bir xüsusiyyəti **`import.meta`**-dır. ES6 modulu (module) daxilində (lakin adi `<script>` və ya `require()` ilə yüklənmiş Node.js modulunda (module) deyil), `import.meta` hazırda icra olunan modul (module) haqqında metadata (metadata) ehtiva edən bir obyektə (object) istinad edir. Bu obyektin (object) `url` xüsusiyyəti (property) modulun (module) yükləndiyi URL-dir (Node.js-də bu, `file://` URL-i olacaq).

`import.meta.url`-in əsas istifadə sahəsi, modulun (module) olduğu qovluqda (directory) (və ya ona nisbətən) saxlanılan şəkillərə (images), data fayllarına (data files) və ya digər resurslara (resources) müraciət edə bilməkdir. `URL()` konstruktoru (constructor) nisbi URL-i (relative URL) `import.meta.url` kimi mütləq URL-ə (absolute URL) nisbətən asanlıqla həll etməyə imkan verir.

**Nümunə:** strings-i (simvolları) lokallaşdırmağa (localize) ehtiyacı olan bir modul (module) yazmısınız və lokallaşdırma (localization) faylları (files) modulla (module) eyni qovluqda (directory) olan `l10n/` qovluğunda (directory) saxlanılır. Modulunuz (module) öz strings-lərini (simvolları) belə bir funksiya (function) ilə yaradılmış URL (URL) istifadə edərək yükləyə bilər:

```javascript
function localStringsURL(locale) {
    return new URL(`l10n/${locale}.json`, import.meta.url); // ✅ Modulun (module) yerləşdiyi yerə nisbətən URL yaradır
}
```