### Fəsil 17. JavaScript Alətləri və Genişlənmələri 🛠️
Bu nöqtəyə qədər gəlibsənsə, artıq JavaScript dilini, onun brauzerdə və Node.js-də tətbiqini dərindən bilirsən. Bu son fəsil, sənin alətlər qutunu daha da zənginləşdirəcək.

Bu fəsildə tanış olacağımız mövzular bunlardır:
* **ESLint** 🧹: Kodunuzdakı potensial səhvləri və stil problemlərini tapan bir "təmizlikçi".
* **Prettier** 🎨: Kodunuzu standart və səliqəli bir formata salan bir "dizayner".
* **Jest** ✅: JavaScript üçün testlər yazmaq üçün populyar bir alət.
* **npm** 📦: Layihənizin asılı olduğu kitabxanaları idarə edən paket meneceri.
* **Code Bundlers** (webpack, Rollup, Parcel) 🏗️: Modullarınızı brauzer üçün tək bir fayla yığan alətlər.
* **Babel** 🤖: Yeni nəsil JavaScript kodunu köhnə brauzerlərin də başa düşəcəyi formata çevirən bir "tərcüməçi".
* **JSX** və **TypeScript/Flow** ⚛️: JavaScript-ə HTML bənzəri sintaksis və statik tiplər əlavə edən dil genişlənmələri.

Bu fəslin məqsədi bu alətləri tam dərinliyinə qədər öyrətmək deyil, sadəcə onların nə olduğunu, niyə faydalı olduqlarını və nə zaman istifadə etmək lazım olduğunu başa salmaqdır.

---
### 17.1 ESLint ilə Kodun Yoxlanılması (Linting) 🧹
Proqramlaşdırmada **"lint"**, texniki olaraq düzgün işləyən, amma problemli, potensial səhvə yol aça biləcək və ya sadəcə "səliqəsiz" görünən kod parçalarına deyilir. **Linter** isə bu cür kodları tapan bir alətdir.

JavaScript üçün bu gün ən çox istifadə olunan linter **ESLint**-dir. O, kodunuzu analiz edərək, sizi potensial səhvlərdən qoruyur və kod stilinizi standartlaşdırmağa kömək edir.

**Geniş Nümunə:**
**Addım 1: Problemli Kod (`linty.js`)**
Aşağıdakı kod işləyir, amma bir neçə problemi var:
```javascript
var x = 'unused'; // var istifadə olunub, dəyişən isə heç vaxt işlədilmir

export function factorial(x) {
  if (x == 1) { // === yerinə == istifadə olunub
    return 1;
    } else { // səhv boşluq buraxılıb
    return x * factorial(x-1) // sonda ; yoxdur
  }
}
```
**Addım 2: ESLint-in Nəticəsi**
Bu fayl üzərində `eslint linty.js` əmrini işə saldıqda, təxminən belə bir nəticə alırıq:
```bash
$ eslint linty.js

/path/to/linty.js
  1:1   error  Unexpected var, use let or const instead          no-var
  1:5   error  'x' is assigned a value but never used            no-unused-vars
  4:11  error  Expected '===' and instead saw '=='               eqeqeq
  5:1   error  Expected indentation of 8 spaces but found 6      indent
  7:28  error  Missing semicolon                                 semi

✖ 5 problems (5 errors, 0 warnings)
```
**Addım 3: İzah**
Gördüyünüz kimi, ESLint bizə xəbərdarlıq edir:
* `var` yerinə `let` və ya `const` istifadə etməliyik.
* `x` dəyişəni təyin olunub, amma heç vaxt istifadə edilməyib (bu, artıq koddur).
* `==` yerinə, tip yoxlaması da aparan `===` istifadə etməliyik.
* Sətirlərin əvvəlindəki boşluqlar (indentation) və nöqtəli vergül (semicolon) ilə bağlı stil problemləri var.

Bu qaydaların bəziləri xırda görünsə də, `===` və istifadə olunmayan dəyişənlər kimi qaydalar sizi gələcəkdəki ciddi səhvlərdən qoruya bilər.

---
### 17.2 Prettier ilə Kodun Formatlanması (Formatting) 🎨
Bəzi proyektlər ESLint-i təkcə səhvləri tapmaq üçün deyil, həm də bütün komandanın eyni kod stilində yazmasını təmin etmək üçün istifadə edir. Amma bunun daha müasir və avtomatik bir yolu var: **Prettier**.

ESLint "nəyin səhv olduğunu" deyirsə, `Prettier` "necə daha gözəl görünməli olduğunu" həll edir və kodu **avtomatik olaraq** yenidən formatlayır. O, kodun məntiqinə toxunmur, yalnız görünüşünü dəyişir.

**Geniş Nümunə:**
**Addım 1: Səliqəsiz, amma işlək kod (`factorial.js`)**
```javascript
function factorial(x)
{
if(x===1){return 1}
else{return x*factorial(x-1)}
}
```
**Addım 2: Prettier-in Sehrli Toxunuşu**
Terminalda `prettier factorial.js` əmrini işə saldıqda, Prettier bizə bu nəticəni göstərir:
```javascript
function factorial(x) {
  if (x === 1) {
    return 1;
  } else {
    return x * factorial(x - 1);
  }
}
```
Gördüyünüz kimi, Prettier bütün boşluqları, sətir sonlarını və nöqtəli vergülləri avtomatik olaraq əlavə edib, kodu oxunaqlı bir vəziyyətə gətirdi.

**İstifadəsi:**
Ən populyar istifadə üsulu, kod redaktorunuzu (məsələn, VS Code) elə tənzimləməkdir ki, faylı hər yaddaşa verdikdə (`Ctrl+S`/`Cmd+S`) Prettier avtomatik olaraq işə düşsün və kodu sizin üçün formatlasın. Bu, sizi kodun görünüşü haqqında düşünməkdən tamamilə azad edir və bütün komandanın eyni stildə yazmasını təmin edir.


***
### 17.3 Jest ilə Vahid Testləmə (Unit Testing) 🧪

Hər bir ciddi proqramlaşdırma layihəsinin vacib bir hissəsi **testlərin yazılmasıdır**. Testlər, kodunuzun gözlənildiyi kimi işlədiyindən əmin olmağa və gələcəkdə yeni dəyişikliklər etdikdə köhnə funksionallığı pozmadığınızı yoxlamağa kömək edir.

JavaScript dünyasında bir çox test aləti var, lakin **Jest** hər şeyi bir paketdə cəmləyən, populyar və çox rahat bir **test freymvorkudur (testing framework)**.

#### Geniş Nümunə: `getTemperature` Funksiyasını Test Etmək
Gəlin addım-addım bir funksiyanı necə test edəcəyimizə baxaq.

**Addım 1: Test Ediləcək Kod (`getTemperature.js`)**
Bu asinxron (asynchronous) funksiya, şəhər adına görə (saxta) bir veb servisdən temperaturu Selsi ilə alır və onu Farenheytə çevirir. Kodda bilərəkdən bir məntiq səhvi var.
```javascript
const getJSON = require("./getJSON.js"); // Bu, şəbəkə sorğusu göndərən bir moduldur

/**
 * Şəhər adına görə temperaturu Farenheyt ilə qaytaran bir Promise.
 */
module.exports = async function getTemperature(city) {
  // Servisdən temperaturu Selsi ilə alır
  const celsius = await getJSON(
    `https://globaltemps.example.com/api/city/${city.toLowerCase()}`
  );
  
  // Səhv formula: F = (C * 9/5) + 32 olmalıdır
  return (celsius * 5 / 9) + 32;
};
```
**Addım 2: Test Faylı (`getTemperature.test.js`)**
Jest, adətən `.test.js` və ya `.spec.js` ilə bitən faylları avtomatik olaraq test faylı kimi tanıyır.
```javascript
// Test ediləcək funksiyanı import edirik
const getTemperature = require("./getTemperature.js");

// `getJSON` modulunu "mock" edirik. Bu, testimizin real şəbəkə sorğusu
// göndərməsinin qarşısını alır və onu saxta, nəzarət edilə bilən bir versiya ilə əvəz edir.
jest.mock("./getJSON.js");
const getJSON = require("./getJSON.js");

// `describe` bloku, əlaqəli testləri bir qrupda cəmləyir
describe("getTemperature() funksiyası", () => {
  // `test` və ya `it` bloku, tək bir test keysini təyin edir
  test("Düzgün API ünvanını çağırır", async () => {
    const expectedURL = "https://globaltemps.example.com/api/city/baku";
    
    // Test edilən funksiyanı çağırırıq
    await getTemperature("Baku");
    
    // `expect` ilə yoxlama (assertion) edirik:
    // getJSON saxta funksiyasının məhz bu URL ilə çağırıldığını gözləyirik.
    expect(getJSON).toHaveBeenCalledWith(expectedURL);
  });
  
  test("Selsini Farenheytə düzgün çevirir", async () => {
    // Test 1: 0°C -> 32°F olmalıdır
    getJSON.mockResolvedValue(0); // Saxta getJSON-un 0 qaytarmasını təmin edirik
    await expect(getTemperature("any_city")).resolves.toBe(32);

    // Test 2: 100°C -> 212°F olmalıdır
    getJSON.mockResolvedValue(100); // Saxta getJSON-un 100 qaytarmasını təmin edirik
    await expect(getTemperature("any_city")).resolves.toBe(212); // BU TEST XƏTA VERƏCƏK!
  });
});
```
**Addım 3: Testlərin İcra Edilməsi və Xətanın Tapılması**
Terminalda `jest` əmrini işə saldıqda, Jest bu testləri icra edir və bizə belə bir nəticə göstərir:
```bash
$ jest getTemperature

 FAIL  ./getTemperature.test.js
  getTemperature() funksiyası
    ✓ Düzgün API ünvanını çağırır (4ms)
    ✕ Selsini Farenheytə düzgün çevirir (3ms)

  ● getTemperature() funksiyası › Selsini Farenheytə düzgün çevirir

    expect(received).toBe(expected) // Object.is bərabərliyi

    Gözlənilən (Expected): 212
    Alınan (Received):   87.55555555555556

      30 |     // Test 2: 100°C -> 212°F olmalıdır
      31 |     getJSON.mockResolvedValue(100);
    > 32 |     await expect(getTemperature("any_city")).resolves.toBe(212);
         |                                                       ^
      33 |   });
```
Jest bizə dəqiq göstərir ki, ikinci testimiz xəta verdi, çünki 212 gözlədiyi halda, 87.55... alıb. Bu, formulamızın səhv olduğunu dərhal üzə çıxarır.

**Addım 4: Düzəliş və Uğurlu Nəticə**
`getTemperature.js`-dəki formulanı `(celsius * 9 / 5) + 32` olaraq düzəldib, testi yenidən işə salsaq, bütün testlərin uğurla keçdiyini görəcəyik. ✅

---
### 17.4 npm ilə Paket İdarəetməsi (Package Management) 📦
Müasir proqramlaşdırma, təkərin yenidən ixtirasına qarşıdır. Biz tez-tez başqaları tərəfindən yazılmış və yoxlanılmış hazır kod kitabxanalarından (libraries/packages) istifadə edirik.

**Paket meneceri (package manager)**, bu üçüncü tərəf kitabxanaları asanlıqla tapmaq, layihəyə yükləmək, versiyalarını idarə etmək və yeniləmək üçün olan bir alətdir. JavaScript dünyasının "supermarketidir". Node.js ilə birlikdə gələn standart paket meneceri **`npm`**-dir.

**Əsas `npm` Əmrləri:**
* **`package.json`**: Hər bir Node layihəsinin "pasportu"dur. Layihə haqqında məlumatları və onun asılı olduğu bütün paketlərin siyahısını saxlayır.
* **`npm init`**: Yeni bir layihəyə başlayarkən, `package.json` faylını yaratmaq üçün interaktiv bir köməkçidir.
* **`npm install <paket_adı>`**: Yeni bir paket yükləmək və onu layihənin asılılıqları siyahısına (`dependencies`) əlavə etmək üçün.
  `$ npm install express`
* **`npm install --save-dev <paket_adı>`**: Yalnız development (inkişaf) mərhələsində lazım olan alətləri (məsələn, Jest, Prettier) yükləmək və `devDependencies` siyahısına əlavə etmək üçün.
  `$ npm install --save-dev jest`
* **`npm install`**: `package.json` faylındakı bütün asılılıqları avtomatik olaraq `node_modules` qovluğuna yükləmək üçün. Başqasının layihəsini öz kompüterinizə köçürdükdən sonra ilk işə salınan əmr adətən budur.
* **`npm install -g <paket_adı>`**: Bir aləti lokal layihəyə deyil, bütün sistemə qlobal olaraq quraşdırmaq üçün.
  `$ npm install -g eslint`

**`npx` - Lokal Alətləri Rahat İşə Salmaq**
Bir aləti (`jest` kimi) lokal olaraq quraşdırdıqda, onu birbaşa terminaldan çağırmaq olmur. Tam yolunu (`./node_modules/.bin/jest`) yazmaq lazımdır. **`npx`**, bu işi asanlaşdıran bir vasitədir. O, lokal olaraq quraşdırılmış aləti adı ilə çağırmağa imkan verir:
`$ npx jest`

***
### 17.5 Kodun Birləşdirilməsi (Code Bundling) 📦
Əgər veb brauzerlər üçün böyük bir JavaScript proqramı yazırsınızsa, çox güman ki, bir **kod birləşdirici (code bundler)** alətindən istifadə edəcəksiniz.

**Problem nədir?** 🤔
Müasir proqramlar onlarla, hətta yüzlərlə kiçik modul fayllarından (`import`/`export` istifadə edərək) ibarət olur. Brauzerin bu faylların hər birini ayrı-ayrılıqda yükləməsi həm yavaş, həm də səmərəsizdir.

**Həll Yolu: Birləşdirmək (Bundling)**
**Analogy:** Təsəvvür et ki, səyahətə çıxırsan. Bütün əşyalarını ayrı-ayrı kiçik çantalarda daşımaq əvəzinə, hamısını bir böyük **çemodana (bundle)** yığırsan. Bu, həm daşımaq üçün daha rahatdır, həm də daha səmərəlidir.

**Kod Birləşdirici (Code Bundler)** də, layihəndəki bütün JavaScript modul fayllarını götürüb, onların asılılıqlarını izləyərək, nəticədə brauzer üçün optimallaşdırılmış tək (və ya bir neçə) bir fayla yığan bir alətdir.

**Populyar Birləşdiricilər:**
* **Webpack:** Çox güclü, çox konfiqurasiya edilə bilən, geniş plaguin (plugin) ekosisteminə malik, sənaye standartı sayılan bir alət. Amma konfiqurasiyası bir az mürəkkəb ola bilər.
* **Rollup:** Əsasən kitabxanalar (libraries) yaratmaq üçün optimallaşdırılıb.
* **Parcel:** Heç bir konfiqurasiya tələb etməyən (`zero-configuration`), istifadəsi çox asan bir alternativ.

#### Birləşdiricilərin Əsas Xüsusiyyətləri
* **🌳 Tree-Shaking (Ağac Silkələmə):** Layihənizdə import etdiyiniz bir kitabxananın yalnız istifadə etdiyiniz hissələrini yekun fayla daxil edir. İstifadə olunmayan kodları "ağacdan tökərək" nəticə faylının həcmini kəskin şəkildə kiçildir.
* **🧩 Kod Bölmə (Code Splitting):** Proqramın başlanğıcda yalnız lazım olan hissəsini yükləməyə imkan verir. Qalan hissələr isə dinamik `import()` ilə tələb olunduqda (məsələn, istifadəçi başqa bir səhifəyə keçdikdə) yüklənir. Bu, ilkin yüklənmə sürətini çox artırır.
* **🗺️ Mənbə Xəritələri (Source Maps):** Birləşdirilmiş və kiçildilmiş (minified) kodda bir xəta baş verdikdə, brauzerin developer konsolunda xətanın sizin orijinal, oxunaqlı kodunuzun hansı sətrində olduğunu göstərməsinə imkan verir. Bu, sazlama (debugging) üçün həyati əhəmiyyət daşıyır.
* **🔥 Development Server & HMR:** Əksər birləşdiricilər, siz kodda dəyişiklik etdikdə avtomatik olaraq kodu yenidən yığan və brauzeri yeniləyən bir development server ilə gəlir. **Hot Module Replacement (HMR)** isə səhifəni tam yeniləmədən, yalnız dəyişən modulu anında dəyişdirərək daha sürətli bir development təcrübəsi təmin edir.

---
### 17.6 Babel ilə Transpilyasiya (Transpilation) 🤖
**Babel**, müasir JavaScript kodunu köhnə JavaScript koduna "tərcümə edən" bir alətdir. O, bir **transpilyatordur (transpiler)**, yəni bir dildəki mənbə kodunu eyni dilin başqa bir versiyasına çevirir.

**Niyə lazımdır?** 🤔
**Analogy:** Təsəvvür et ki, sən ən müasir dildə bir şeir yazırsan, amma nənən yalnız köhnə ləhcəni başa düşür. **Babel** sənin müasir şeirini götürüb, nənənin başa düşəcəyi köhnə ləhcəyə **tərcümə edən** bir "tərcüməçidir".

Eynilə, Babel də yeni nəsil JavaScript xüsusiyyətlərini (ES2020, ESNext) istifadə edərək yazdığınız kodu, hələ bu xüsusiyyətləri dəstəkləməyən köhnə brauzerlərin başa düşdüyü köhnə versiyaya (adətən ES5) çevirir.

**Geniş Nümunə: "Əvvəl və Sonra"**
**Müasir Kod (ES6+):**
```javascript
// Arrow function, default parameter, exponentiation operator, class
const myFunc = (a, b = 10) => a ** b;

class MyClass {
  constructor(name) {
    this.name = name;
  }
}
```
**Babel-in Nəticəsi (Təxmini ES5 ekvivalenti):**
```javascript
"use strict";

// class -> function constructor
var MyClass = function MyClass(name) {
  this.name = name;
};

// const -> var
// arrow function -> regular function
var myFunc = function myFunc(a, b) {
  // default parameter
  if (b === void 0) { b = 10; }
  // exponentiation -> Math.pow
  return Math.pow(a, b);
};
```
**Müasir İstifadəsi:**
Bu gün əksər brauzerlər ES6-nı tam dəstəkləsə də, Babel hələ də çox vacibdir:
1.  **Ən yeni dil xüsusiyyətlərini** (hələ brauzerlərə tam gəlməmiş) istifadə etmək üçün.
2.  **Dil genişlənmələrini** (language extensions) istifadə etmək üçün. Ən populyarları **JSX** (React ilə istifadə olunur) və **TypeScript**-dir. Babel bu xüsusi sintaksisləri adi JavaScript-ə çevirə bilir.

Adətən Babel tək başına deyil, kod birləşdiricilərin (məsələn, webpack-in `babel-loader`-i) bir hissəsi kimi istifadə olunur və bütün prosesi avtomatlaşdırır.

***
### 17.7 JSX: JavaScript daxilində İşarələmə (Markup) İfadələri ⚛️
**JSX (JavaScript XML)**, JavaScript-in sintaksis genişlənməsidir və bizə JavaScript kodunun içində birbaşa HTML-ə bənzər kod yazmağa imkan verir. JSX, ən çox **React** freymvorku ilə assosiasiya olunur və istifadəçi interfeyslərini (UI) deklarativ şəkildə təsvir etmək üçün istifadə olunur.

React istifadə etməsəniz belə, onun populyarlığı səbəbindən JSX sintaksisi ilə tez-tez qarşılaşacaqsınız, buna görə də onun nə olduğunu anlamaq vacibdir.

**Analogy:** JSX, sanki JavaScript-ə HTML "ləhcəsində" danışmağı öyrədir. 🗣️📄

**Ən Vacib Qayda:** JSX, brauzerlər tərəfindən birbaşa başa düşülmür. O, işləməzdən əvvəl **Babel** kimi bir transpilyator (transpiler) vasitəsilə standart JavaScript koduna **çevrilməlidir**.

#### JSX-in JavaScript-ə Çevrilməsi (Transformation)
JSX-in bütün sehri, onun arxa planda sadə bir funksiya çağırışına çevrilməsidir: **`React.createElement()`**.
`React.createElement(type, props, ...children)`
* **`type`**: Elementin növü (məsələn, `"div"`, `"h1"` və ya bir komponent).
* **`props`**: Elementin atributlarını (`id`, `className` və s.) saxlayan bir obyekt.
* **`...children`**: Elementin daxilindəki uşaq elementlər və ya mətnlər.

Gəlin bu çevrilməni nümunələrlə görək.

**Nümunə 1: Sadə teq və atributlar**
**JSX kodu (bizim yazdığımız):**
```jsx
const greeting = <h1 className="title" id="main-title">Salam, Dünya!</h1>;
```
**Babel-in nəticəsi (arxa planda alınan JavaScript):**
```javascript
const greeting = React.createElement(
  "h1", // type
  { className: "title", id: "main-title" }, // props obyekti
  "Salam, Dünya!" // children
);
```

**Nümunə 2: İç-içə elementlər**
**JSX kodu:**
```jsx
const card = (
  <div className="card">
    <h2>Başlıq</h2>
    <p>Məzmun...</p>
  </div>
);
```
**Babel-in nəticəsi:**
```javascript
const card = React.createElement(
  "div",
  { className: "card" },
  React.createElement("h2", null, "Başlıq"),
  React.createElement("p", null, "Məzmun...")
);
```

**Nümunə 3: JSX daxilində JavaScript ifadələri (`{...}`)**
JSX-in ən güclü tərəfi, onun içinə birbaşa `{}` işarələri ilə JavaScript ifadələri yerləşdirə bilməkdir.
**JSX kodu:**
```jsx
function UserProfile({ user, showDetails }) {
  return (
    <div className={`profile ${user.theme}`}>
      <h1>{user.name.toUpperCase()}</h1>
      {/* && operatoru ilə şərti renderinq */}
      {showDetails && <p>Yaş: {user.age}</p>}
    </div>
  );
}
```
**Babel-in nəticəsi:**
```javascript
function UserProfile({ user, showDetails }) {
  return React.createElement(
    "div",
    { className: `profile ${user.theme}` },
    React.createElement("h1", null, user.name.toUpperCase()),
    showDetails && React.createElement("p", null, "Yaş: ", user.age)
  );
}
```

**Nümunə 4: Massivi (Array) render etmək**
`.map()` kimi massiv metodları, JSX-də dinamik siyahılar yaratmaq üçün çox geniş istifadə olunur.
**JSX kodu:**
```jsx
function NavLinks({ items }) {
  return (
    <ul>
      {items.map(item => <li key={item.id}><a href={item.url}>{item.text}</a></li>)}
    </ul>
  );
}
```
**Babel-in nəticəsi:**
```javascript
function NavLinks({ items }) {
  return React.createElement(
    "ul",
    null,
    items.map(item => 
      React.createElement(
        "li",
        { key: item.id },
        React.createElement("a", { href: item.url }, item.text)
      )
    )
  );
}
```
React, `children` olaraq ötürülən bu massivi avtomatik olaraq açıb, elementləri bir-bir render edir.

---
#### Komponentlər vs. HTML Teqləri: Böyük və Kiçik Hərf Fərqi 💡
Bu, JSX-in ən fundamental qaydasıdır:
* **Kiçik hərflə başlayan teqlər (`div`, `p`, `img`)**: HTML teqləri kimi qəbul edilir və `React.createElement`-ə `"div"` kimi bir **sətir (string)** olaraq ötürülür.
* **BÖYÜK hərflə başlayan teqlər (`UserProfile`, `NavLinks`)**: Fərdi **komponentlər** kimi qəbul edilir və `React.createElement`-ə `UserProfile` kimi birbaşa **funksiya və ya sinif referansı** olaraq ötürülür.

Bu, bizə öz təkrar istifadə edilə bilən UI bloklarımızı yaratmağa imkan verir.

**Nümunə:**
```jsx
// 1. Böyük hərflə başlayan bir funksiya yaradırıq - bu, bir React komponentidir.
// O, `props` adlı bir obyekt qəbul edir.
function WelcomeMessage(props) {
  return <h1>Salam, {props.name}!</h1>;
}

// 2. İndi onu HTML teqi kimi başqa bir JSX ifadəsinin içində istifadə edə bilərik.
const app = <WelcomeMessage name="Ayan" />;

// 3. Babel bunu bu koda çevirir:
const app = React.createElement(WelcomeMessage, { name: "Ayan" });
// Göründüyü kimi, birinci arqument artıq "h1" kimi sətir deyil, 
// birbaşa WelcomeMessage funksiyasının özüdür.
```
Bu mexanizm, React-də komponentlərin bir-birinin içində istifadə edilərək mürəkkəb interfeyslər yaradılmasının təməlini təşkil edir.

***
### 17.8 Flow ilə Tiplərin Yoxlanılması (Type Checking) ✅
JavaScript dinamik bir dildir, yəni dəyişənlərin tipi proqram işləyərkən müəyyən edilir (`let x = 5; x = "salam";`). Bu, çeviklik versə də, bəzən gözlənilməz xətalara yol açır (məsələn, rəqəm gözlədiyin yerə sətir gəlməsi).

**Statik tip yoxlaması (static type checking)** isə, bu cür xətaları kodu heç işə salmamışdan əvvəl, yazı zamanı tapmağa kömək edən bir prosesdir. JavaScript üçün bu işi görən iki populyar alət var: **Flow** və **TypeScript**.

**TypeScript vs. Flow**
Hər ikisi də eyni məqsədə xidmət etsə də, aralarında fərq var. Qısacası:
* **TypeScript (TS):** JavaScript-in üstündə qurulmuş yeni bir dildir. Öz kompilyatoru (`tsc`) var və JavaScript-ə tiplərdən əlavə, `enum`, `interface` kimi yeni xüsusiyyətlər də gətirir. Bu gün daha geniş yayılıb.
* **Flow:** Yalnız tip yoxlaması üçün nəzərdə tutulmuş bir alətdir. O, yeni bir dil deyil, JavaScript koduna əlavə olunan **annotasiyalardır**. Babel, bu annotasiyaları koddan təmizləyərək onu standart JavaScript-ə çevirir.

Bu kitab JavaScript haqqında olduğu üçün, biz burada dili daha az dəyişən `Flow`-ya nümunə olaraq baxacağıq, amma burada öyrənəcəyiniz bütün prinsiplər TypeScript üçün də tamamilə keçərlidir.

---
### 17.8.1 Flow-u Quraşdırmaq və İşə Salmaq
Flow-u layihəyə `npm install --save-dev flow-bin` ilə əlavə etmək, `npx flow --init` ilə `.flowconfig` faylı yaratmaq olar. Bir faylın Flow tərəfindən yoxlanılması üçün, həmin faylın ən başına **`// @flow`** kommentini əlavə etmək kifayətdir.

**Flow-un "Ağıllı" Tərəfi: Tip Çıxarma (Type Inference)**
Flow-un ən güclü tərəflərindən biri odur ki, siz heç bir tip annotasiyası yazmasanız belə, o, kodunuzu analiz edərək dəyişənlərin tipini "təxmin etməyə" (to infer) çalışır və məntiqsizlikləri tapır.

**Nümunə 1: Dəyişənin tipinin dəyişməsi**
```javascript
// @flow
let i = { r: 0 }; // Flow bilir ki, 'i' bir obyektdir.
for(i = 0; i < 10; i++) { // SƏHV: 'i'-ni burada rəqəmlə əvəzləyirik!
  console.log(i);
}
i.r = 1; // Flow burada xəta verir: "r" xüsusiyyəti rəqəmdə mövcud deyil!
```

**Nümunə 2: Funksiyaya səhv tipdə arqument ötürmək**
```javascript
// @flow
function size(x) {
  return x.length; // Flow başa düşür ki, 'x' .length xüsusiyyəti olan bir şey olmalıdır.
}
let s = size(1000); // SƏHV: 1000 rəqəminin .length xüsusiyyəti yoxdur! Flow xəta verəcək.
```
---
### 17.8.2 Tip Annotasiyalarından İstifadə 🏷️
Flow-un əsl gücü, ona özümüzün **tip annotasiyaları (type annotations)** ilə kömək etdiyimiz zaman ortaya çıxır. Bu, dəyişənin və ya funksiya parametrinin adından sonra iki nöqtə (`:`) və tipin adını yazmaqla edilir.

**Nümunə: Dəyişənlərə və funksiyalara tip vermək**
```javascript
// @flow

// Dəyişənlər üçün annotasiyalar
let name: string = "Ayan";
let age: number = 25;
let isActive: boolean = true;

// Funksiyalar üçün annotasiyalar
// `s` parametrinin string, qaytarılan dəyərin isə number olacağını bildiririk
function size(s: string): number {
  return s.length;
}

const result: number = size("salam");
```

#### `null` və `undefined` Problemi (`Maybe` Tipləri)
Flow, standart olaraq, təhlükəsizliyi təmin etmək üçün `null` və `undefined` dəyərlərini heç bir tipə aid etmir. Yəni, `string` olaraq təyin etdiyiniz bir dəyişənə `null` mənimsədə bilməzsiniz.

Əgər bir dəyişənin `null` və ya `undefined` ola biləcəyini bildirmək istəyirsinizsə, tipin əvvəlinə sual işarəsi (`?`) qoymalısınız. Buna **"maybe type"** deyilir. Məsələn, `?string` "ya bir sətir, ya null, ya da undefined" deməkdir.

**Geniş Nümunə: `null` yoxlamasının məcburiyyəti**
Bu, statik tip yoxlamasının nə qədər faydalı olduğunu göstərən ən yaxşı nümunədir.

**Addım 1: Funksiyanı `?string` ilə təyin edirik**
```javascript
// @flow
function safeSize(s: ?string): number {
  // SƏHV: Flow burada xəta verəcək!
  // Çünki 's' null ola bilər və null-ın .length xüsusiyyəti yoxdur.
  return s.length;
}
```
Flow dərhal bizi xəbərdar edir ki, bu kod təhlükəlidir və `null` dəyəri üçün proqram "çökə" bilər.

**Addım 2: Düzgün və təhlükəsiz kod**
Flow, biz `s`-in `null` olmadığını yoxlayana qədər bizə icazə verməyəcək.
```javascript
// @flow
function safeSize(s: ?string): number {
  // `s`-in null və ya undefined ola biləcəyini yoxlayırıq
  if (s === null || s === undefined) {
    // Bu blokda Flow bilir ki, s null və ya undefined-dır.
    return 0;
  } else {
    // BU BLOKDA İSƏ, Flow 100% əmindir ki, 's' bir string-dir.
    // Buna görə də .length istifadə etməyə icazə verir.
    return s.length;
  }
}

console.log(safeSize("salam")); // 5
console.log(safeSize(null));   // 0
console.log(safeSize());       // 0
```
Bu, statik tip yoxlamasının əsas məqsədidir: potensial run-time xətalarını hələ yazılma mərhələsində tutaraq, daha etibarlı və möhkəm kod yazmağımıza kömək etmək.

***
### 17.8.3 Sinif (Class) Tipləri 🏛️
Flow, primitiv tiplərlə yanaşı, həm JavaScript-in daxili (built-in) siniflərini (`Date`, `RegExp`, `Map`, `Set`), həm də bizim öz yaratdığımız sinifləri bir tip kimi tanıyır.

**Nümunə: Daxili sinifləri tip kimi istifadə etmək**
```javascript
// @flow

// Bu funksiya, verilmiş tarixin ISO formatının,
// verilmiş requlyar ifadəyə uyğun gəlib-gəlmədiyini yoxlayır.
// Annotasiyalar sayəsində, bura səhvən başqa bir tipdə dəyər ötürmək mümkün deyil.
function dateMatches(d: Date, p: RegExp): boolean {
  return p.test(d.toISOString());
}

const isTodayChristmas = dateMatches(new Date(), /^\d{4}-12-25T/);
```

**Fərdi Siniflərlə (Custom Classes) İşləmək**
Öz yaratdığınız bir sinifi tip kimi istifadə etmək üçün, Flow bir qaydaya əməl etməyinizi tələb edir: **sinifin bütün xüsusiyyətlərinin (properties) tipləri əvvəlcədən bəyan edilməlidir.**

**Geniş Nümunə: `Complex` (Kompleks Rəqəm) sinifi**
```javascript
// @flow

class Complex {
  // 1. Sinifin xüsusiyyətlərini və onların tiplərini əvvəlcədən bəyan edirik
  r: number;
  i: number;
  static i: Complex; // Hətta statik xüsusiyyətləri də

  // 2. Konstruktorun parametrlərinin də tiplərini veririk
  constructor(r: number, i: number) {
    this.r = r;
    this.i = i;
  }
  
  // 3. Metodların parametrlərinin və qaytarılan dəyərin tiplərini veririk
  add(that: Complex): Complex {
    return new Complex(this.r + that.r, this.i + that.i);
  }
}

// Bu mənimsətmə, yuxarıda `static i: Complex` annotasiyası olmasaydı,
// Flow tərəfindən xəta olaraq qəbul edilərdi.
Complex.i = new Complex(0, 1);
```

---
### 17.8.4 Obyekt Tipləri (Object Types) 📦
Bir funksiyanın müəyyən bir "formaya" (`shape`) malik obyekt qəbul etdiyini bildirmək üçün, obyekt tiplərindən istifadə edirik. Onların sintaksisi obyekt literallarına çox bənzəyir, lakin dəyərlərin yerinə tiplər yazılır.

**Geniş Nümunə: Müxtəlif obyekt tipləri**
```javascript
// @flow

// 1. Sadə Obyekt Tipi: `Point` adlı obyektin `x` və `y` adlı `number` xüsusiyyətləri olmalıdır.
function distance(point: {x: number, y: number}): number {
  return Math.hypot(point.x, point.y);
}

// 2. Könüllü (Optional) Xüsusiyyət: `z` xüsusiyyətinin olması məcburi deyil (adın yanındakı `?`-yə diqqət et).
type Point3D = { x: number, y: number, z?: number };
const p1: Point3D = { x: 1, y: 2 }; // ✅ Düzgündür
const p2: Point3D = { x: 1, y: 2, z: 3 }; // ✅ Düzgündür

// 3. Dəqiq (Exact) Obyekt Tipi: `{| ... |}` sintaksisi, obyektin yalnız göstərilən xüsusiyyətlərə malik olmasına icazə verir.
type ExactPoint = {| x: number, y: number |};
// const p3: ExactPoint = { x: 1, y: 2, z: 3 }; // ❌ SƏHV! `z` xüsusiyyəti artıqdır.

// 4. İndekslənə bilən (Indexable) Tip / Lüğət (Dictionary)
// Bu, açarları `string`, dəyərləri isə `number` olan bir obyekt deməkdir.
type ScoreSheet = { [string]: number };
const scores: ScoreSheet = { "Ayan": 95, "Tural": 88 }; // ✅ Düzgündür
// const wrongScores: ScoreSheet = { "Leyla": "əla" }; // ❌ SƏHV! Dəyər `number` olmalıdır.
```
---
### 17.8.5 Tip Ləqəbləri (Type Aliases) ✍️
Uzun və mürəkkəb tip təriflərini (xüsusilə obyekt tiplərini) hər dəfə təkrar-təkrar yazmaq həm yorucu, həm də səhvə yol aça biləndir. **`type`** açar sözü ilə biz mürəkkəb bir tipə ad (ləqəb - alias) verib, onu təkrar istifadə edə bilərik.

**Nümunə: `Point` tipi yaratmaq**
Əvvəlki `distance` funksiyasını `type` ilə daha oxunaqlı edək.
```javascript
// @flow

// 1. `Point` adlı yeni bir tip ləqəbi yaradırıq
export type Point = {
  x: number,
  y: number
};

// 2. İndi funksiyamızda uzun `{x: number, y: number}` yerinə, sadəcə `Point` yazırıq
function distance(point: Point): number {
  return Math.hypot(point.x, point.y);
}

// 3. Bu tipi başqa fayllarda da istifadə etmək üçün export edə bilərik
//    və `import type { Point } from './distance.js'` ilə import edə bilərik.
```
`type` ləqəbləri, kodu daha təmiz, oxunaqlı və idarə edilə bilən etdiyi üçün böyük layihələrdə çox vacibdir.

---

### 17.8.6 Massiv (Array) və Kortec (Tuple) Tipləri
Flow-da massivlərin tipini təyin etmək üçün, həm də massivin daxilindəki elementlərin tipini bildirmək lazımdır.

**Bircinsli (Homogeneous) Massivlər**
Bütün elementləri eyni tipdə olan massivlər üçün iki fərqli sintaksis var:
1.  **`Array<T>`**: `Array<number>`, `Array<string>`
2.  **`T[]`**: `number[]`, `string[]` (eyni mənanı verir, daha qısa yazılışdır)

```javascript
// @flow
// Bu funksiya, yalnız rəqəmlərdən ibarət bir massiv qəbul edir
function average(data: Array<number>): number {
  const sum = data.reduce((a, b) => a + b, 0);
  return sum / data.length;
}

average([1, 2, 3, 4, 5]);      // ✅ Düzgündür
// average([1, 2, "üç"]); // ❌ SƏHV! Flow xəta verəcək, çünki "üç" bir rəqəm deyil.
```
**Korteclər (Tuples)**
Uzunluğu sabit olan və hər bir elementinin tipi fərqli ola bilən massivlərə **kortec (tuple)** deyilir. Onların tipi kvadrat mötərizə `[]` içində təyin olunur.

```javascript
// @flow
// Bu funksiya, birinci elementi rəqəm, ikinci elementi isə sətir olan
// iki elementli bir kortec qaytarır.
function getStatus(): [number, string] {
  return [200, "OK"];
}

// Kortec-lərlə işləməyin ən yaxşı yolu destructuring-dir
const [statusCode, statusMessage]: [number, string] = getStatus();
```

**Müxtəlif Tipli Massivlər (`mixed` və `any`)**
Elementlərinin tipi fərqli-fərqli olan bir massiv təyin etmək üçün **`Array<mixed>`** istifadə olunur. `mixed`, Flow-a deyir ki, "bu massivdə hər şey ola bilər". Lakin `mixed`-in gözəlliyi odur ki, siz massivdən bir elementi götürüb onunla işləmək istədikdə, Flow sizdən həmin elementin tipini yoxlamağı tələb edəcək. Bu, təhlükəsizliyi təmin edir. **`any`** tipi isə `mixed`-ə bənzəsə də, o, bütün tip yoxlamalarını deaktiv edir və istifadəsi məsləhət görülmür.

---
### 17.8.7 Parametrli Tiplər (Generics) `<T>`
`Array<number>` sintaksisində `number`, `Array` tipi üçün bir **tip parametri (type parameter)** adlanır. Bu cür tiplərə **parametrli tiplər** və ya daha geniş mənada **generics** deyilir.

**Analogy:** Təsəvvür et ki, bir **qutu (`Array`)** var. Amma bu sadəcə qutu deyil, **"alma qutusu" (`Array<alma>`)** və ya **"kitab qutusu" (`Array<kitab>`)** ola bilər. `<...>` içindəki tip parametri, bu "ümumi" konteynerin içində nəyin olacağını dəqiq bildirməyə imkan verir.

`Set` və `Map` kimi daxili siniflər də generikdir:
* `let mySet: Set<string> = new Set();`
* `let myMap: Map<number, User> = new Map();` (açar `number`, dəyər `User` tipində)

**Fərdi Generics Yaratmaq**
Biz öz siniflərimizi və funksiyalarımızı da generik edə bilərik.

**Nümunə 1: Generik `Result` Sinifi**
Bu sinif, ya bir xəta (`E` tipi), ya da uğurlu bir nəticə (`V` tipi) saxlaya bilər. `E` və `V` burada istənilən tipin yerini tutan "placeholder"-lardır.
```javascript
// @flow
class Result<E, V> {
  error: ?E;
  value: ?V;
  constructor(error: ?E, value: ?V) { this.error = error; this.value = value; }
  // ...davamı...
}

// İstifadəsi:
let successResult: Result<Error, string> = new Result(null, "Uğurlu!");
let errorResult: Result<Error, string> = new Result(new Error("Xəta!"), null);
```

**Nümunə 2: Generik `zip` Funksiyası**
Bu funksiya, iki fərqli tipdə massiv götürüb, onları cütlərdən ibarət yeni bir massivə çevirir.
```javascript
// @flow
function zip<A, B>(a: Array<A>, b: Array<B>): Array<[A, B]> {
  // ... funksiyanın məntiqi ...
}
```
---
### 17.8.8 Yalnız-Oxunaqlı (Read-Only) Tiplər 🛡️
Bəzən bir funksiyanın ona ötürülən obyekti və ya massivi dəyişməyəcəyinə "söz verməsini" istəyirik. Flow, bu "sözü" yoxlamaq üçün xüsusi **utility tiplər** təqdim edir:
* **`$ReadOnly<T>`**: Obyektlər üçün.
* **`$ReadOnlyArray<T>`**: Massivlər üçün.

Bu tiplər, datanı run-time-da "dondurmur" (`Object.freeze` kimi). Onlar yalnız development zamanı, əgər siz səhvən dəyişiklik etməyə cəhd etsəniz, Flow-un sizə xəbərdarlıq etməsini təmin edir.

**Nümunə:**
```javascript
// @flow
type Point = { x: number, y: number };

// Bu funksiya, 'p' obyektini dəyişməyəcəyinə söz verir.
function distance(p: $ReadOnly<Point>): number {
  // p.x = 100; // ❌ SƏHV! Flow burada xəta verəcək: "Cannot assign to p.x because it is read-only"
  return Math.hypot(p.x, p.y);
}

// Bu funksiya, `data` massivini dəyişməyəcəyinə söz verir.
function sum(data: $ReadOnlyArray<number>): number {
  // data.push(100); // ❌ SƏHV! `.push` massivi dəyişdiyi üçün Flow xəta verəcək.
  return data.reduce((a, b) => a + b, 0);
}
```
Bu, funksiyalarınızın daha təhlükəsiz və proqnozlaşdırıla bilən olmasına kömək edən çox güclü bir vasitədir.

***
### 17.8.9 Funksiya Tipləri (Function Types) `( ) => { }`
Biz funksiyaların parametrlərinin və qaytardığı dəyərin tipini necə təyin edəcəyimizi gördük. Bəs əgər bir funksiyanın parametri elə başqa bir funksiyadırsa (yəni **callback**)? Bu zaman həmin callback funksiyasının öz "formasını" – yəni hansı tipdə arqumentlər qəbul edib, hansı tipdə dəyər qaytardığını – təsvir etməliyik.

Bunun üçün sintaksis ox funksiyalarına (`=>`) çox bənzəyir:
`(arg1: Tip1, arg2: Tip2) => QaytarılanTip`

**Geniş Nümunə: Callback funksiyası üçün tip yaratmaq**
Aşağıdakı nümunədə, `fetchText` funksiyası bir callback qəbul edir və biz həmin callback üçün `FetchTextCallback` adlı bir tip ləqəbi (type alias) yaradırıq.
```javascript
// @flow

// 1. Callback funksiyamızın tipini təyin edirik
// Bu funksiya 3 arqument qəbul edir: Error (ola da bilər, olmaya da),
// number (ola da bilər, olmaya da) və string (ola da bilər, olmaya da).
// Heç nə qaytarmadığı üçün `void` yazırıq.
export type FetchTextCallback = (?Error, ?number, ?string) => void;

// 2. İndi isə əsas funksiyamızda həmin tipi istifadə edirik
function fetchText(url: string, callback: FetchTextCallback) {
  let status = null;
  fetch(url)
    .then(response => {
      status = response.status;
      return response.text();
    })
    .then(body => {
      // Uğurlu halda, xəta `null`, nəticə isə `body` olur
      callback(null, status, body);
    })
    .catch(error => {
      // Xəta halında, xətanı birinci, nəticəni isə `null` ötürürük
      callback(error, status, null);
    });
}
```
---
### 17.8.10 Birləşmə Tipləri (Union Types) `|`
Bəzən bir dəyişən və ya funksiya parametri təkcə bir tipdə deyil, bir neçə fərqli tipdən biri ola bilər. Məsələn, bir funksiya həm `Array`, həm `Set`, həm də `Map` qəbul edə bilsin. Bu cür hallar üçün **birləşmə (union)** tiplərindən istifadə edirik.

Sintaksisi çox sadədir: tipləri bir-birindən şaquli xətt (`|`) ilə ayırırıq. Bu işarə "və ya" kimi oxunur.

**Geniş Nümunə: Müxtəlif kolleksiyaların ölçüsünü tapan `size` funksiyası**
```javascript
// @flow

// Bu funksiya, 3 fərqli tipdən birini qəbul edə bilir
function size(collection: Array<mixed> | Set<mixed> | Map<mixed, mixed>): number {
  // `collection`-ın hansı tipdə olduğunu yoxlamalıyıq (type refinement)
  if (Array.isArray(collection)) {
    // Bu blokda Flow bilir ki, `collection` bir massivdir (Array)
    return collection.length;
  } else {
    // Bu blokda isə Flow bilir ki, `collection` ya Set, ya da Map-dir
    // və hər ikisinin də `.size` xüsusiyyəti var.
    return collection.size;
  }
}

const arr = [1, 2, 3];
const set = new Set(["a", "b"]);

console.log(`Massivin ölçüsü: ${size(arr)}`);   // ✅ 3
console.log(`Set-in ölçüsü: ${size(set)}`);       // ✅ 2
// size("salam"); // ❌ SƏHV! Flow xəta verəcək, çünki string bu tiplərdən biri deyil.
```
**Qeyd:** Əvvəl gördüyümüz `?string` əslində `string | null | void` yazılışının qısa formasıdır.

---
### 17.8.11 Sadalanan (Enum) və Fərqləndirilən (Discriminated) Birləşmələr 🚦
Flow, tipləri daha da dəqiqləşdirmək üçün çox güclü iki konsept təqdim edir.

**1. Sadalanan Tiplər (Enumerated Types)**
Bir tipin dəyərinin yalnız müəyyən bir neçə sabit dəyərdən biri ola biləcəyini təyin edə bilərik. Bu, sətir (string) və ya rəqəm literallarının birləşməsi (union) ilə yaradılır.
```javascript
// @flow
type Suit = "Clubs" | "Diamonds" | "Hearts" | "Spades";
type Status = "pending" | "success" | "error";

let cardSuit: Suit = "Hearts"; // ✅ Düzgündür
// let wrongSuit: Suit = "Spade"; // ❌ SƏHV! Flow xəta verəcək, çünki "Spade" bu tipdə yoxdur.
```
**2. Fərqləndirilən Birləşmələr (Discriminated Unions)**
Bu, union tiplərinin ən güclü tətbiqidir. Bir neçə fərqli **obyekt tipini** bir `union`-da birləşdiririk. Amma bir şərtlə: hər bir obyekt tipinin içində eyni adlı, lakin fərqli sabit dəyərə malik bir "fərqləndirici" (discriminant) xüsusiyyət olmalıdır.

**Geniş Nümunə: Worker-dən gələn fərqli mesajları idarə etmək**
Təsəvvür edək ki, bir worker bizə 3 fərqli tipdə mesaj göndərə bilər: nəticə, xəta və ya statistika.
```javascript
// @flow

// Hər mesaj tipinin özünəməxsus xüsusiyyətləri var, amma hamısında
// `messageType` adlı fərqləndirici bir açar var.

type ResultMessage = { messageType: "result", result: Array<any> };
type ErrorMessage = { messageType: "error", error: Error };
type StatsMessage = { messageType: "stats", splinesPerSecond: number };

// Ümumi mesaj tipi, yuxarıdakı üç tipin birləşməsidir
type WorkerMessage = ResultMessage | ErrorMessage | StatsMessage;

// Bu funksiya, gələn mesajı təhlükəsiz şəkildə emal edir
function handleMessage(message: WorkerMessage) {
  
  // Fərqləndirici açara görə yoxlama aparırıq
  if (message.messageType === "result") {
    // Bu `if` blokunun içində, Flow 100% əmindir ki, `message` bir `ResultMessage`-dir.
    // Buna görə də `message.result` xüsusiyyətini istifadə etmək tamamilə təhlükəsizdir.
    console.log("Nəticə gəldi:", message.result);

  } else if (message.messageType === "error") {
    // Bu blokda isə, Flow bilir ki, bu bir `ErrorMessage`-dir.
    console.error("Worker-də xəta baş verdi:", message.error);
    
  } else { // messageType === "stats"
    // Və nəhayət, bu blokda `message`-in `StatsMessage` olduğunu bilir.
    console.info("Statistika:", message.splinesPerSecond);
  }
}
```
Bu yanaşma, `switch` və ya `if/else` blokları ilə işləyərkən, hər bir halda düzgün obyektlə işlədiyimizdən əmin olaraq, kodu daha etibarlı və səhvsiz etməyə kömək edir.