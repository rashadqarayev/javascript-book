### Fəsil 15. Veb Brauzerlərdə JavaScript (JavaScript in Web Browsers) 🌐

JavaScript 1994-cü ildə məhz veb brauzerlərdəki sənədlərə dinamik davranış vermək məqsədilə yaradılıb. O vaxtdan bəri həm dilin özü, həm də veb platforması inanılmaz dərəcədə inkişaf edib. Bu gün brauzer sadəcə sənədləri göstərən bir proqram deyil, qrafika, audio, video, şəbəkə (network), yaddaş (storage) və hətta thread-lər kimi xidmətlər təqdim edən tam bir proqram platformasıdır. JavaScript isə bu platformanın xidmətlərindən istifadə etməyə imkan verən dildir.

Bu fəsildə veb proqramları yazmaq üçün lazım olan ən vacib JavaScript API-larını araşdıracağıq:
* 📜 Sənədin məzmununu (**content**) idarə etmək (§15.3) və stilini (**style**) dəyişmək (§15.4)
* 📍 Elementlərin ekrandakı mövqeyini təyin etmək (§15.5)
* 🧩 Təkrar istifadə edilə bilən istifadəçi interfeysi komponentləri (**components**) yaratmaq (§15.6)
* 🎨 Qrafika çəkmək (§15.7 və §15.8)
* 🎵 Səsləri çalmaq və yaratmaq (§15.9)
* 🧭 Brauzerin naviqasiyasını və tarixçəsini (**history**) idarə etmək (§15.10)
* ↔️ Şəbəkə (network) üzərindən data mübadiləsi aparmaq (§15.11)
* 💾 İstifadəçinin kompüterində data saxlamaq (§15.12)
* ⚙️ Thread-lər ilə paralel hesablamalar aparmaq (§15.13)

---
#### QEYD: Client-Side (Frontend) vs Server-Side (Backend)
Bu kitabda və ümumiyyətlə vebdə **"client-side JavaScript"** termini ilə tez-tez qarşılaşacaqsınız. Bu, sadəcə olaraq **veb brauzerdə** işləmək üçün yazılmış JavaScript deməkdir. Onun əksi isə **"server-side JavaScript"**-dir ki, bu da veb serverlərdə (məsələn, Node.js ilə) işləyir.

Bunu bir restoran kimi düşünün:
* **Frontend (Client-Side)** 🛋️: Sizin gördüyünüz, oturduğunuz zal, menyu və ofisiantla ünsiyyətiniz. Bu, istifadəçinin brauzerində baş verən hər şeydir.
* **Backend (Server-Side)** 👨‍🍳: Mətbəxdə yeməklərin bişirildiyi, sifarişlərin idarə olunduğu və anbardakı məhsulların uçotunun aparıldığı, sizin görmədiyiniz hissə. Bu isə serverdə baş verən proseslərdir.

---
#### ❗️ VACİB: Köhnə (Legacy) API-lar haqqında
JavaScript-in 25 ildən artıq olan tarixində brauzerlərə saysız-hesabsız API-lar əlavə edilib. Onların bir çoxu bu gün artıq köhnəlmiş hesab olunur və istifadəsi məsləhət görülmür. Brauzerlər köhnə veb saytları pozmamaq üçün bu API-ları hələ də dəstəkləsə də, müasir proqramlaşdırmada onlardan uzaq durmaq lazımdır. Bu köhnəlmiş API-lar əsasən bunlardır:
1.  **Mülkiyyətçi (Proprietary) API-lar:** Yalnız bir brauzer (əsasən köhnə Internet Explorer) tərəfindən yaradılıb, heç vaxt standartlaşdırılmayanlar.
2.  **Səmərəsiz (Inefficient) API-lar:** `document.write()` kimi, səhifənin performansına kəskin mənfi təsir göstərən metodlar.
3.  **Köhnəlmiş (Outdated) API-lar:** `document.bgColor` kimi, CSS-in gəlişi ilə əhəmiyyətini itirmiş və daha müasir alternativləri olanlar.
4.  **Uğursuz Dizayn Edilmiş API-lar:** İlk DOM API-ları kimi, JavaScript-in təbiətinə uyğun olmayan və istifadəsi çətin olanlar.

**Nəticə:** Bu kitabda və müasir proqramlaşdırmada biz bu köhnə API-ları öyrənməyəcəyik. Fokusumuz müasir, standart və stabil API-lar üzərində olacaq.

Əla, bro! Fəsil 15-in ilk və ən təməl mövzusuna başlayırıq: JavaScript kodu veb səhifəyə necə daxil edilir və necə işləyir?

***
### 15.1 Veb Proqramlaşdırmanın Əsasları (Web Programming Basics)
Bu bölmə, veb üçün yazılan JavaScript proqramlarının necə qurulduğunu, brauzerə necə yükləndiyini, istifadəçidən necə məlumat (input) aldığını, necə nəticə (output) çıxardığını və hadisələrə (events) reaksiya verərək necə asinxron (asynchronous) işlədiyini izah edir.

---
### 15.1.1 HTML `<script>` Teqlərində JavaScript 📜
Brauzerlər HTML sənədlərini göstərir. Əgər brauzerin JavaScript kodunu icra etməsini istəyirsinizsə, həmin kodu HTML sənədinin içinə daxil etməli və ya ona istinad etməlisiniz. Bu işi `<script>` teqi görür.

#### Daxili Skriptlər (Inline Scripts)
JavaScript kodunu birbaşa HTML faylının içində, `<script>` və `</script>` teqləri arasında yaza bilərsiniz.

**Geniş Nümunə: Rəqəmsal Saat ⏰**
Aşağıdakı nümunə, HTML, CSS və JavaScript-in birlikdə işləyərək səhifədə dinamik bir rəqəmsal saat yaratmasını göstərir.
```html
<!DOCTYPE html>
<html>
<head>
  <title>Rəqəmsal Saat</title>
  <style>
    /* CSS ilə saatımızın görünüşünü təyin edirik */
    #clock {
      font: bold 24px sans-serif;
      background: #ddf;
      padding: 15px;
      border: solid black 2px;
      border-radius: 10px;
    }
  </style>
</head>
<body>
  <h1>Rəqəmsal Saat</h1>
  <span id="clock"></span>

  <script>
    // Bütün JavaScript kodumuz buradadır

    function displayTime() {
      // 1. "clock" id-li elementi tapırıq
      const clockElement = document.querySelector("#clock");
      // 2. Hazırkı vaxtı götürürük
      const now = new Date();
      // 3. Vaxtı yerli formata salıb elementin içinə yazırıq
      clockElement.textContent = now.toLocaleTimeString('az-AZ');
    }

    // Səhifə açılan kimi saatı bir dəfə göstəririk
    displayTime();
    
    // Və hər 1000ms-dən (1 saniyə) bir saatı yeniləmək üçün interval qururuq
    setInterval(displayTime, 1000);
  </script>
</body>
</html>
```

#### Xarici Skriptlər (External Scripts)
JavaScript kodunu birbaşa HTML-ə yazmaq əvəzinə, onu ayrı bir `.js` faylında saxlamaq və `<script>` teqinin `src` (source) atributu ilə HTML-ə bağlamaq daha yaxşı təcrübədir (best practice).

Yuxarıdakı saatın kodunu `scripts/digital_clock.js` adlı bir fayla yerləşdirsəydik, HTML-də belə görünərdi:
`<script src="scripts/digital_clock.js"></script>`

**Xarici skriptlərin üstünlükləri:**
* **🧹 Təmiz Kod:** HTML (məzmun) və JavaScript-i (davranış) bir-birindən ayırır.
* **♻️ Təkrar İstifadə:** Eyni JavaScript faylını bir neçə fərqli səhifədə istifadə etmək olar.
* **🚀 Keşləmə (Caching):** Brauzer xarici `.js` faylını bir dəfə yükləyir və sonrakı səhifələrdə onu keşdən (cache) götürür, bu da saytın sürətini artırır.
* **☁️ Kənar Mənbələr:** Başqa serverlərdə yerləşən skriptləri (məsələn, Google Analytics, reklam skriptləri) səhifənizə qoşmağa imkan verir.

#### Modullar (`type="module"`) 📦
Əgər kodunuzu ES6 modulları (`import`/`export`) ilə yazmısınızsa, əsas skript faylınızı HTML-ə `type="module"` atributu ilə daxil etməlisiniz:
`<script type="module" src="main.js"></script>`

#### Skriptin İcra Zamanı: `async` və `defer` ⏳
Standart olaraq, brauzer `<script>` teqinə rast gəldikdə, HTML-i emal etməyi **dayandırır**, skripti yükləyib **icra edir** və yalnız bundan sonra davam edir. Bu, böyük skriptlərdə səhifənin açılışını ləngidə bilər. `async` və `defer` atributları bu davranışı dəyişir.

* **Standart (Bloklayan) 🐢:** `<script src="..."></script>`
    * HTML dayanır, skript yüklənir və icra olunur, sonra HTML davam edir.

* **`defer` (Təxirə Salınmış) 🚶‍♂️...** `<script defer src="..."></script>`
    * Skript HTML ilə paralel olaraq yüklənir.
    * HTML sənədi tam emal olunduqdan **sonra** icra olunur.
    * `defer` skriptləri HTML-dəki ardıcıllıqlarını qoruyur.

* **`async` (Asinxron) 🚀:** `<script async src="..."></script>`
    * Skript HTML ilə paralel olaraq yüklənir.
    * Yüklənən **kimi dərhal** icra olunur (HTML hələ emal olunarkən belə).
    * Hansı birinci yüklənsə, o birinci işə düşür, yəni ardıcıllıq pozula bilər.

**Sadə Alternativ:** Çox vaxt ən sadə həll, `<script>` teqlərini `<body>`-nin ən sonunda, `</html>`-dən əvvəl yerləşdirməkdir. Bu zaman skript işə düşəndə, ondan yuxarıdakı bütün HTML elementləri artıq mövcud olur.

#### Skriptləri Tələbə Görə Yükləmək (Loading Scripts On Demand) ✨
Bəzən bir skriptə səhifə açılan kimi deyil, yalnız istifadəçi bir düyməyə kliklədikdə ehtiyac olur. Bu cür skriptləri tələbə görə yükləmək üçün proqramatik olaraq səhifəyə yeni bir `<script>` elementi əlavə edə bilərik.

**Nümunə: `importScript` Promise-əsaslı funksiya**
```javascript
/**
 * Verilmiş URL-dən skripti asinxron olaraq yükləyir və icra edir.
 * Nəticə olaraq, skript yükləndikdə `fulfilled` olan bir Promise qaytarır.
 */
function importScript(url) {
  return new Promise((resolve, reject) => {
    // 1. Yeni bir <script> elementi yaradırıq
    const s = document.createElement("script");

    // 2. Yüklənmə uğurlu olduqda Promise-i `resolve` edirik
    s.onload = () => { resolve(); };
    // 3. Xəta baş verdikdə `reject` edirik
    s.onerror = (e) => { reject(e); };
    
    // 4. Skriptin mənbəyini (URL) təyin edirik
    s.src = url;
    
    // 5. Və onu sənədin <head> hissəsinə əlavə edərək yüklənməni başladırıq
    document.head.append(s);
  });
}

// İstifadəsi (məsələn, bir düyməyə kliklədikdə):
// myButton.onclick = () => {
//   importScript("/js/heavy-library.js")
//     .then(() => { console.log("Kitabxana uğurla yükləndi!"); })
//     .catch(() => { console.error("Kitabxananı yükləmək mümkün olmadı."); });
// };
```
Əla, bro! Davam edirik. Bu hissə, brauzerdə JavaScript-in "danışdığı" və idarə etdiyi ən vacib obyekt olan **DOM**-u izah edir.

***
### 15.1.2 Sənəd Obyekt Modeli (The Document Object Model - DOM) 🌳
Client-side (brauzer) JavaScript proqramlaşdırmasında ən vacib obyekt, brauzer pəncərəsində göstərilən HTML sənədini təmsil edən **`Document`** obyektidir. HTML sənədləri ilə işləmək üçün istifadə olunan API isə **Sənəd Obyekt Modeli (Document Object Model)**, yəni qısaca **DOM** adlanır.

DOM, HTML sənədinin məntiqi quruluşunu bir obyekt ağacı şəklində təmsil edir.

#### HTML-in Ağac Quruluşu
Gəlin belə sadə bir HTML sənədinə baxaq:
```html
<html>
  <head>
    <title>Nümunə Sənəd</title>
  </head>
  <body>
    <h1>Bir HTML Sənədi</h1>
    <p>Bu, bir <i>sadə</i> sənəddir.</p>
  </body>
</html>
```
Bu sənədin quruluşu bir ağaca bənzəyir: `<html>` teqi kökdür və onun `<body>` və `<head>` adlı "budaqları" var. Bu budaqların da öz daxilində başqa elementləri var.

DOM, bu ağac quruluşunu JavaScript üçün bir obyekt modeli olaraq təqdim edir. Sənəddəki **hər bir HTML teqi** üçün müvafiq bir **`Element` obyekti**, hər bir **mətn parçası** üçün isə bir **`Text` obyekti** mövcuddur. Həm `Element`, həm `Text`, həm də `Document` siniflərinin özləri daha ümumi olan **`Node` (qovşaq)** sinifindən törəyir.

Yuxarıdakı HTML-in DOM ağacı təxminən belə görünür:
```
Document
└── html
    ├── head
    │   ├── #text (boşluq)
    │   ├── title
    │   │   └── #text ("Nümunə Sənəd")
    │   └── #text (boşluq)
    └── body
        ├── #text (boşluq)
        ├── h1
        │   └── #text ("Bir HTML Sənədi")
        ├── #text (boşluq)
        ├── p
        │   ├── #text ("Bu, bir ")
        │   ├── i
        │   │   └── #text ("sadə")
        │   └── #text (" sənəddir.")
        └── #text (boşluq)
```

#### DOM Terminologiyası (DOM Terminology) 👨‍👩‍👧‍👦
Ağac strukturlarını anlamaq üçün "ailə ağacı" terminologiyasından istifadə olunur:
* **Parent (Valideyn):** Bir qovşağın (node) birbaşa yuxarısındakı qovşaq.
* **Children (Övladlar):** Bir qovşağın birbaşa aşağısındakı qovşaqlar.
* **Siblings (Bacı-qardaşlar):** Eyni valideynə sahib olan qovşaqlar.
* **Descendants (Nəsil):** Bir qovşaqdan aşağıda yerləşən bütün qovşaqlar.
* **Ancestors (Əcdadlar):** Bir qovşaqdan yuxarıda yerləşən bütün qovşaqlar (valideyn, nənə-baba və s.).

DOM API-ı, bu ağac üzərində hərəkət etməyə, yeni `Element` və `Text` qovşaqları yaratmağa, onları sənədə əlavə etməyə, yerlərini dəyişməyə və ya silməyə imkan verir. Server tərəfində `console.log()` ilə mətn çıxardığımız halda, brauzer tərəfində formatlı HTML nəticəsini məhz DOM API-ı ilə sənədi manipulyasiya edərək yaradırıq.

#### HTML Teqləri və JavaScript Obyektləri
DOM-da hər bir HTML teq növü üçün xüsusi bir JavaScript sinifi (class) var. Məsələn:
* `<body>` teqi `HTMLBodyElement` sinifinin bir nüsxəsidir.
* `<img>` teqi `HTMLImageElement` sinifinin bir nüsxəsidir.
* `<table>` teqi `HTMLTableElement` sinifinin bir nüsxəsidir.

Bu JavaScript obyektlərinin xüsusiyyətləri (properties), HTML teqlərinin atributları (attributes) ilə birbaşa əlaqəlidir.

**Nümunə: `<img>` teqi və onun `.src` xüsusiyyəti**
```html
<img id="myImage" src="images/cat.jpg" alt="Pişik şəkli">
```
```javascript
// JavaScript-də həmin elementi seçirik
const imgElement = document.querySelector('#myImage');

// Elementin .src propertisi, HTML-dəki src atributunun dəyərini qaytarır
console.log(imgElement.src); // ✅ Nəticə (təxmini): "http://mysite.com/images/cat.jpg"

// İndi isə propertini dəyişək
imgElement.src = "images/dog.jpg";
// BU ANDA BRAUZER `cat.jpg`-i `dog.jpg` ilə əvəz edəcək!
```
Bəzi elementlərin isə `play()` (`<audio>`), `pause()` (`<video>`) kimi özlərinə məxsus xüsusi metodları da olur.

Əla, bro! Bu hissədə brauzerdəki kodlarımızın "yaşadığı" mühitlə – **Qlobal Obyekt** və **Adlar Fəzası** ilə tanış olacağıq. Bu, skriptlərin bir-biri ilə necə əlaqə saxladığını anlamaq üçün vacibdir.

***
### 15.1.3 Veb Brauzerlərdə Qlobal Obyekt (The Global Object) 🌍
Hər bir brauzer pəncərəsi və ya tabı (tab) üçün yalnız **bir ədəd qlobal obyekt (global object)** mövcuddur. Həmin pəncərədə işləyən bütün JavaScript kodları (istisna: worker thread-lər) bu tək qlobal obyekti paylaşır. Bu o deməkdir ki, əgər bir skript bu obyekt üzərində bir xüsusiyyət (property) yaratsa, digər bütün skriptlər də həmin xüsusiyyəti görə bilir.

**Bu qlobal obyektin içində nə var?** 🤔
1.  **JavaScript-in standart kitabxanası:** `parseInt()`, `Math` obyekti, `Date`, `Set`, `Map` sinifləri və s.
2.  **Veb API-ları:** `document` obyekti, `fetch()` funksiyası, `Audio()` konstruktoru və s.
3.  **Pəncərənin öz xüsusiyyətləri:** `history` (brauzerin tarixçəsi), `innerWidth` (pəncərənin eni), `location` (hazırkı URL) və s.

Brauzerlərdə bu qlobal obyektin özünə istinad edən **`window`** adlı bir xüsusiyyəti də var. Yəni, brauzer mühitində **qlobal obyekt elə `window` obyektinin özüdür.**

**Nümunə:**
```javascript
// Bu iki sətir tamamilə eynidir, çünki `innerWidth` qlobal obyektin bir xüsusiyyətidir.
console.log(window.innerWidth); // Pəncərənin daxili enini göstərir
console.log(innerWidth);      // `window.` prefiksini yazmaq məcburi deyil

// `alert` funksiyası da qlobal `window` obyektinin bir metodudur.
window.alert("Bu, window.alert()-dir!");
alert("Bu isə sadəcə alert()-dir!"); // Eyni işi görür
```
Kodun daha aydın olması üçün, pəncərəyə aid xüsusiyyətləri çağırarkən `window.` prefiksini yazmaq yaxşı təcrübə hesab olunur (`window.history`, `window.location` və s.).

---
### 15.1.4 Skriptlərin Ortaq Adlar Fəzası (Scripts Share a Namespace) 🤝
Əgər modullardan (`type="module"`) istifadə etmirsinizsə, bir HTML sənədindəki bütün `<script>` teqləri **eyni qlobal adlar fəzasını (namespace)** paylaşır.

Bunu belə təsəvvür et: eyni sənəddəki bütün `<script>` teqləri eyni otaqdadır. Birinin otağın ortasına qoyduğu bir əşyanı (dəyişən, funksiya), otaqdakı hər kəs görür və istifadə edə bilir. Bu, kiçik proqramlar üçün rahat olsa da, böyük proyektlərdə adların bir-birinə qarışması (`naming conflicts`) kimi ciddi problemlərə yol aça bilər.

#### `var`/`function` və `let`/`const`/`class` arasındakı vacib fərq
Qlobal səviyyədə təyin olunan bütün dəyişənlər və funksiyalar ortaq olsa da, onların qlobal obyektlə əlaqəsi fərqlidir:

* **Köhnə üsul (`var`, `function`):** Qlobal səviyyədə `var` və `function` ilə təyin olunan hər şey birbaşa qlobal `window` obyektinin bir **xüsusiyyətinə (property)** çevrilir.
* **Müasir üsul (`let`, `const`, `class`):** ES6 ilə gələn bu açar sözlərlə qlobal səviyyədə təyin olunanlar ortaq adlar fəzasında mövcud olur, lakin `window` obyektinin bir xüsusiyyətinə çevrilmir.

**Geniş Nümunə: İki fərqli skriptin əlaqəsi**
Təsəvvür edək ki, HTML sənədimizdə aşağıdakı iki skript ardıcıl yerləşdirilib.

**`script1.js`**
```javascript
// Köhnə üsulla təyin olunanlar
var legacyVar = "Mən window-dayam!";
function legacyFunc() {
  console.log("Mən də window-dayam!");
}

// Müasir üsulla təyin olunanlar
let modernLet = "Mən window-da deyiləm.";
const modernConst = "Mən də deyiləm.";
class ModernClass {}
```

**`script2.js`**
```javascript
// İndi isə ikinci skriptdən birincidəkilərə müraciət edək

// `var` və `function` ilə yaradılanlar həm birbaşa, həm də `window` ilə əlçatandır
console.log(legacyVar);         // ✅ "Mən window-dayam!"
console.log(window.legacyVar);  // ✅ "Mən window-dayam!"
legacyFunc();                   // ✅ "Mən də window-dayam!"
window.legacyFunc();            // ✅ "Mən də window-dayam!"

console.log("---");

// `let`, `const`, `class` ilə yaradılanlar birbaşa əlçatandır...
console.log(modernLet);         // ✅ "Mən window-da deyiləm."
const instance = new ModernClass();
console.log(instance);          // ✅ ModernClass {}

// ...amma onlar `window` obyektinin xüsusiyyəti deyil!
console.log(window.modernLet);  // ✅ undefined
console.log(window.ModernClass);// ✅ undefined
```
**Nəticə:** Adların bir-birinə qarışmaması və daha təmiz kod üçün, müasir proyektlərdə həmişə modullardan (`<script type="module">`) istifadə etmək ən yaxşı yoldur.

Olar, bro. Bu hissə brauzerdə yazdığımız kodların ümumi işləmə prinsipini, yəni "oyunun qaydalarını" izah edir. Gəl bu vacib təməl məlumatları da mənimsəyək.

***
### 15.1.5 JavaScript Proqramlarının İcrası (Execution of JavaScript Programs) ▶️

Brauzerdə işləyən bir JavaScript "proqramı", bir HTML sənədinin daxilindəki və ya ona xaricdən qoşulmuş bütün skriptlərin məcmusudur. Bu ayrı-ayrı skriptlər eyni qlobal `Window` obyektini paylaşır və eyni `Document` obyektini idarə edir.

JavaScript proqramının icrasını iki əsas mərhələyə bölmək olar:

#### Mərhələ 1: Yüklənmə və Sinxron İcra 📜
Bu, səhifənin ilk açıldığı andır.
* Brauzer HTML sənədini yuxarıdan aşağıya doğru oxumağa (parse) başlayır.
* `<script>` teqlərinə rast gəldikcə, onları (adətən) dayanıb icra edir. Bu skriptlər ya gələcəkdə istifadə olunacaq funksiyaları təyin edir, ya da səhifənin o anki vəziyyətini manipulyasiya edir.
* Bu mərhələ qısadır və səhifə tam yüklənənə qədər davam edir.

#### Mərhələ 2: Asinxron və Hadisə Yönümlü (Event-Driven) 🔄
Sənəd tam yükləndikdən sonra proqram ikinci mərhələyə keçir.
* Bu mərhələdə JavaScript passiv olaraq "gözləmə" rejiminə keçir.
* O, yalnız müəyyən bir **hadisə (event)** baş verdikdə aktivləşir və müvafiq **hadisə işləyicisini (event handler)**, yəni bizim yazdığımız callback funksiyasını icra edir.
* Bu hadisələr istifadəçi hərəkətləri (klik, klaviatura), şəbəkə cavabları, zamanlayıcılar (`setTimeout`) və s. ola bilər.

Bu iki mərhələ arasındakı keçidi bildirən iki vacib hadisə var:
* **`DOMContentLoaded`**: HTML sənədi tamamilə yüklənib emal olunduqda baş verir. Artıq DOM ağacı ilə işləmək olar.
* **`load`**: Təkcə HTML deyil, həm də bütün xarici resurslar (şəkillər, stillər və s.) tam yüklənib bitdikdə `window` obyekti üzərində baş verir.

---
#### JavaScript-in "Tək-Axınlı" (Single-Threaded) Modeli 🧵
Bu, JavaScript-in ən fundamental xüsusiyyətlərindən biridir.

**Single-threaded nə deməkdir?** Təsəvvür et ki, JavaScript bir restoranda işləyən **tək bir ofisiantdır**. O, eyni anda yalnız bir masaya xidmət edə bilər. Bir masanın sifarişini tam almamış, digərinə keçə bilməz. Yəni, JavaScript eyni anda yalnız bir əməliyyatı yerinə yetirə bilir.

* **Üstünlüyü (+):** Kod yazmaq çox sadələşir. İki fərqli funksiyanın eyni anda işləyib bir-birinə mane olması, "race condition", "deadlock" kimi mürəkkəb problemlər haqqında narahat olmağa ehtiyac qalmır.
* **Mənfi Cəhəti (-):** Əgər həmin tək axın uzun çəkən bir əməliyyatla məşğul olarsa (məsələn, mürəkkəb bir hesablama), bütün proqram, o cümlədən istifadəçi interfeysi (UI) **donur**. Brauzer istifadəçinin kliklərinə və digər hərəkətlərinə reaksiya vermir.
* **Həll Yolu (`Web Worker`-lar):** Bu donma problemini həll etmək üçün arxa planda işləyən "köməkçi ofisiantlar", yəni **Web Worker**-lar mövcuddur. Onlar ağır hesablamaları arxa planda aparır və əsas proqramın donmasının qarşısını alır. Onlar əsas proqramla yalnız mesajlaşma yolu ilə əlaqə saxlayır.

---
#### Səhifə Yüklənməsinin Zaman Xətti (Timeline) ⏰
Brauzer bir səhifəni açarkən baş verən proseslərin ardıcıllığı təxminən belədir:

1.  **Başlanğıc:** Brauzer `Document` obyektini yaradır və HTML-i emal etməyə başlayır. `document.readyState` bu an `"loading"` olur.
2.  **Sinxron Skript:** Brauzer `async` və ya `defer` atributu olmayan bir `<script>`-ə rast gəlir. HTML-in emalını dayandırır, skripti yükləyir və icra edir.
3.  **`async` Skript:** Brauzer `async` atributu olan bir `<script>`-ə rast gəlir. Skripti arxa planda yükləməyə başlayır, amma HTML-in emalını dayandırmır. Skript yüklənən kimi dərhal icra olunur.
4.  **HTML Bitdi:** Sənədin bütün HTML-i emal olunduqda, `document.readyState` `"interactive"` olur.
5.  **`defer` Skriptləri:** İndi brauzer `defer` atributu ilə işarələnmiş bütün skriptləri HTML-dəki ardıcıllıqlarına uyğun olaraq icra edir.
6.  **`DOMContentLoaded` Hadisəsi 🚀:** Bütün sinxron skriptlər və `defer` skriptləri işini bitirdikdən sonra, `Document` obyekti üzərində `DOMContentLoaded` hadisəsi baş verir. Bu an, DOM-la tam işləmək üçün ideal bir vaxtdır.
7.  **`load` Hadisəsi 🏁:** Səhifədəki bütün xarici resurslar (şəkillər, iframe-lər və s.) tam yükləndikdən və bütün `async` skriptlər işini bitirdikdən sonra, `document.readyState` `"complete"` olur və `Window` obyekti üzərində `"load"` hadisəsi baş verir.
8.  **Hadisə Gözləmə:** Bu andan etibarən, proqram istifadəçi və ya digər asinxron hadisələrə reaksiya vermək üçün passiv gözləmə rejiminə keçir.

Bu ardıcıllığı anlamaq, skriptlərinizi niyə və harada yerləşdirməli olduğunuzu bilmək (məsələn, `defer` istifadə etmək və ya `<body>`-nin sonuna qoymaq) və veb səhifənizin sürətini optimallaşdırmaq üçün çox vacibdir.

Əla, bro! Bu, fəslin son nəzəri hissəsidir. Burada proqramımızın xarici aləmlə necə "danışdığını", xətaların necə idarə olunduğunu və brauzerin bizi hansı təhlükələrdən qoruduğunu öyrənəcəyik.

***
### 15.1.6 Proqramın Giriş və Çıxışları (Input and Output) 📥📤
Hər proqram kimi, brauzerdəki JavaScript proqramları da müəyyən giriş (input) məlumatlarını emal edərək nəticə (output) hasil edir.

#### Girişlər (Inputs)
Brauzerdəki bir proqramın əldə edə biləcəyi əsas girişlər bunlardır:
* **📜 Sənədin Məzmunu:** DOM API-ı vasitəsilə sənədin bütün məzmunu (`document`).
* **🖱️ İstifadəçi Hərəkətləri:** Kliklər, klaviatura daxiletmələri, toxunmalar kimi hadisələr (events).
* **🔗 Sənədin URL-i:** `document.URL` ilə hazırkı səhifənin tam URL-ini və ya `location` obyekti ilə onun hissələrini əldə etmək.
* **🍪 "Cookie"-lər:** `document.cookie` ilə brauzerdə saxlanan cookie-ləri oxumaq və yazmaq.
* **🖥️ Brauzer və Sistem Məlumatları:**
    * **`navigator` obyekti:** Brauzer, əməliyyat sistemi və onların imkanları haqqında məlumat verir (`navigator.userAgent`, `navigator.language`, `navigator.hardwareConcurrency` - CPU nüvələrinin sayı).
    * **`screen` obyekti:** İstifadəçinin ekran ölçüləri haqqında məlumat verir (`screen.width`, `screen.height`).

#### Çıxışlar (Outputs)
* **DOM Manipulyasiyası:** Əsas çıxış üsulu, DOM API-ı ilə HTML sənədini dəyişərək istifadəçiyə yeni məzmun və ya dəyişikliklər göstərməkdir.
* **`console.log()`:** Bu metodlarla çıxarılan nəticələr yalnız brauzerin "Developer Console"-unda görünür və əsasən sazlama (debugging) məqsədləri üçün istifadə olunur.

---
### 15.1.7 Proqram Xətaları (Program Errors) 🚨
Brauzerdə işləyən JavaScript proqramları, Node.js proqramları kimi tam mənası ilə "çökmür" (crash). Əgər kodunuzda tutulmayan bir xəta (`uncaught exception`) baş verərsə, brauzer bu xətanı sadəcə "Developer Console"-a yazdırır və proqramın qalan hissəsi (məsələn, digər hadisə dinləyiciləri) işləməyə davam edir.

Bütün bu tutulmayan xətaları idarə etmək üçün iki qlobal hadisə işləyicisi (event handler) var:

* **`window.onerror`**: Tutulmayan **sinxron (synchronous)** xətalar üçün sonuncu müdafiə xəttidir. Bu funksiya 5 arqument qəbul edir (`message`, `source`, `lineno`, `colno`, `error`) və xəta haqqında məlumat verir. Əgər bu funksiya `true` qaytarsa, brauzer standart xəta mesajını konsola yazdırmır.

* **`window.onunhandledrejection`**: Tutulmayan **Promise xətaları (rejections)** üçün nəzərdə tutulub. Bu funksiyaya ötürülən hadisə (event) obyektinin `reason` xüsusiyyəti, `.catch()` blokuna ötürüləcək xətanı saxlayır.

Bu handler-lərin əsas istifadə məqsədi **telemetriyadır**: istifadəçilərin brauzerlərində baş verən gözlənilməz xətaları `fetch()` vasitəsilə öz serverinizə göndərərək, proqramdakı problemlərdən xəbərdar ola bilərsiniz.

---
### 15.1.8 Veb Təhlükəsizlik Modeli (The Web Security Model) 🛡️
Brauzerlər iki rəqabət aparan məqsədi balanslaşdırmağa çalışır:
1.  Güclü veb proqramları yaratmaq üçün zəngin API-lar təqdim etmək.
2.  Zərərli kodların istifadəçinin məlumatlarını oğurlamasının, məxfiliyi pozmasının və sistemə zərər verməsinin qarşısını almaq.

Bu məqsədlə brauzerlər bir neçə fundamental təhlükəsizlik qaydası tətbiq edir.

#### JavaScript Nəyi Edə Bilməz?
Brauzerdəki JavaScript-in imkanları təhlükəsizlik məqsədilə məhdudlaşdırılıb:
* Fayl sisteminə birbaşa giriş yoxdur. JavaScript sizin kompüterinizdən fayl silə və ya ora virus yaza bilməz.
* Ümumi şəbəkə imkanları yoxdur. JavaScript yalnız HTTP (`fetch`) və `WebSocket` kimi standart protokollarla ünsiyyət qura bilər, istənilən portu dinləyən bir server yarada bilməz.

#### Eyni Mənbə Siyasəti (The Same-Origin Policy) 🌍
Bu, vebin ən fundamental təhlükəsizlik qaydalarından biridir.
**Qayda:** Bir mənbədən (origin) gələn skript, başqa bir mənbədən gələn sənədin məzmununa birbaşa müraciət edə bilməz.

**Mənbə (Origin)** nədir? `protokol + host + port` üçlüyü. Məsələn:
* `http://example.com` və `https://example.com` fərqli mənbələrdir (protokol fərqi).
* `http://example.com` və `http://www.example.com` fərqli mənbələrdir (host fərqi).
* `http://example.com:80` və `http://example.com:8080` fərqli mənbələrdir (port fərqi).

Bu siyasət, əsasən `<iframe>`-lər və `fetch()` sorğuları üçün keçərlidir. Yəni, `a.com`-dakı bir skript, `b.com`-dan yüklənmiş bir `<iframe>`-in içindəki sənədi və ya `b.com`-a birbaşa `fetch` sorğusunu icra edə bilməz. Bu qaydanı yumşaltmağın yeganə yolu, digər serverin **CORS (Cross-Origin Resource Sharing)** başlıqları ilə buna icazə verməsidir.

#### Saytlararası Skriptinq (Cross-Site Scripting - XSS) ☠️
XSS, hakerin zərərli kodunu (adətən JavaScript) sizin veb saytınıza yeridərək, saytınıza daxil olan istifadəçilərin brauzerində icra etməsidir.

**Bu necə baş verir?** Saytınız istifadəçidən gələn məlumatı (məsələn, şərh, ad və s.) yoxlamadan, "təmizləmədən" birbaşa səhifənin HTML-inə daxil etdikdə baş verir.

**Sadə Nümunə:** Tutaq ki, saytınız URL-dən adı götürüb istifadəçini salamlayır.
```javascript
// Gözlənilən URL: .../greet.html?name=Ayan
// Zərərli URL: .../greet.html?name=<img src=x onload="alert('HACKED!')">

const name = new URL(document.URL).searchParams.get("name");
// TƏHLÜKƏLİ KOD: .innerHTML istifadəçidən gələn datanı birbaşa HTML-ə yazır
document.querySelector('h1').innerHTML = "Salam, " + name; 
```
Zərərli URL ilə daxil olduqda, səhifəyə `<img>` teqi yeridilir və onun `onload` atributundakı JavaScript kodu icra olunur. Bu, hakerin sizin saytınız adından istifadəçinin cookie-lərini oğurlamasına və ya başqa zərərli fəaliyyətlər etməsinə imkan verə bilər.

**XSS-dən Necə Qorunmalı?**
1.  **Məlumatı Təmizləyin (Sanitize):** İstifadəçidən gələn və HTML-ə yerləşdiriləcək hər hansı bir mətndəki xüsusi HTML simvollarını (`<`, `>`, `&` və s.) onların təhlükəsiz alternativləri ilə (`&lt;`, `&gt;`, `&amp;`) əvəz edin.
2.  **`textContent` istifadə edin:** Əgər HTML render etmək niyyətiniz yoxdursa, `.innerHTML` yerinə **həmişə** `.textContent` istifadə edin. `.textContent` heç vaxt HTML teqlərini icra etmir, onları sadə mətn kimi göstərir. Bu, ən təhlükəsiz yoldur.

Əla, bro! Fəsil 15-in ən fundamental mövzusuna, yəni brauzerdə proqramları interaktiv edən mexanizmə gəldik: **Hadisələr (Events)**.

***
### 15.2 Hadisələr (Events) ⚡
Client-side (brauzer) JavaScript proqramları **hadisə yönümlü (event-driven)** bir modeldə işləyir. Bu nə deməkdir? Proqram, bir aktyor kimi səhnə arxasında dayanıb istifadəçinin hərəkətini gözləyir. İstifadəçi hər hansı bir "maraqlı" hərəkət etdikdə – düyməyə kliklədikdə, klaviaturada bir hərf yazdıqda və ya siçanı tərpətdikdə – brauzer bir **hadisə (event)** yaradır. Bizim kodumuz isə bu hadisəyə reaksiya verərək müvafiq əməliyyatı yerinə yetirir.

Bu modeli anlamaq üçün 5 əsas termini bilməliyik:

#### 1. Hadisənin Növü (Event Type)
Bu, nəyin baş verdiyini bildirən bir sətirdir (string). Hər bir hadisənin özünəməxsus adı var.
* `"click"`: İstifadəçi siçan ilə kliklədikdə.
* `"keydown"`: İstifadəçi klaviaturada bir düyməni basdıqda.
* `"mousemove"`: İstifadəçi siçanı tərpətdikdə.
* `"load"`: Səhifə və ya bir resurs tamamilə yükləndikdə.

#### 2. Hadisənin Hədəfi (Event Target) 🎯
Bu, hadisənin üzərində baş verdiyi obyektdir. Hadisədən danışarkən, həm növünü, həm də hədəfini bildirməliyik.
* `Window` üzərindəki `"load"` hadisəsi.
* `<button>` elementi üzərindəki `"click"` hadisəsi.
* `<input>` elementi üzərindəki `"keydown"` hadisəsi.
`Window`, `Document` və `Element` obyektləri ən çox rast gəlinən hadisə hədəfləridir.

#### 3. Hadisə İşləyicisi (Event Handler / Listener) 👂
Bu, hadisə baş verdikdə çağırılan **callback funksiyasıdır**. Biz bu funksiyanı brauzerə "qeydiyyatdan" keçiririk və deyirik: "Filan hədəfdə, filan növ hadisə baş verəndə, bu funksiyanı çağır".

#### 4. Hadisə Obyekti (Event Object) 📦
Hadisə baş verdikdə, brauzer bizim hadisə işləyicimizə (handler) həmin hadisə haqqında detallı məlumatları saxlayan bir **obyekt** ötürür.
* Bütün hadisə obyektlərində `.type` (hadisənin növü) və `.target` (hadisənin hədəfi) xüsusiyyətləri olur.
* Bundan əlavə, hər növün özünəməxsus məlumatları olur. Məsələn, siçan hadisələrində klikin koordinatları (`clientX`, `clientY`), klaviatura hadisələrində isə basılan düymənin kodu (`key`, `code`) olur.

#### 5. Hadisənin Yayılması (Event Propagation) ➡️
Bəzi hadisələr təkcə hədəf elementdə deyil, həm də onun "əcdadları" boyunca yayılır. Bu prosesin iki əsas mərhələsi var, amma ən vacibi və standart olanı **qabarcıqlanmadır (bubbling)**.

**Qabarcıqlanma (Bubbling):** Təsəvvür et ki, suyun dibinə atılan bir daşdan çıxan qabarcıqlar yuxarıya doğru qalxır. Eynilə, bir element üzərində baş verən hadisə də əvvəlcə həmin elementdə, sonra onun valideynində (parent), sonra onun valideynində və beləcə `Document` və `Window`-a qədər "qalxır".

Bu, bizə imkan verir ki, hər düyməyə ayrı-ayrılıqda "qulaqcıq" (listener) asmaq əvəzinə, onların hamısını əhatə edən bir konteynerə tək bir listener asaq və hadisələri orada tutaq. Bu texnikaya **hadisə deleqasiyası (event delegation)** deyilir.

---
**Hər Şeyi Birləşdirən Nümunə:**
Təsəvvür edək ki, istifadəçi bir veb səhifədə "Yüklə" yazılan yaşıl bir düyməyə (`<button>`) klikləyir.
* **Hadisənin Növü:** `"click"`
* **Hadisənin Hədəfi:** Həmin `<button>` elementi.
* **Hadisə İşləyicisi:** Bizim `button.addEventListener('click', ...)` ilə təyin etdiyimiz funksiya.
* **Hadisə Obyekti:** İşləyiciyə ötürülən və klikin koordinatları, baş vermə zamanı kimi məlumatları saxlayan obyekt.
* **Hadisənin Yayılması:** Klik hadisəsi əvvəlcə `button`-da, sonra onun valideyn `div`-ində, sonra `body`-də və nəhayət `window`-da "hiss edilir" (əgər dayandırılmayıbsa).

**Defolt Davranışlar (Default Actions)**
Bəzi hadisələrin brauzer tərəfindən təyin edilmiş standart bir davranışı var. Məsələn, bir linkə (`<a>` teqi) kliklədikdə, standart davranış brauzerin həmin linkə keçid etməsidir. Biz hadisə işləyicimizin içində bu standart davranışı ləğv edə (`event.preventDefault()`) bilərik.

Düzdü, bro. O qədər çox hadisə növü var ki, hamısını əzbərləmək mümkün deyil. Amma onları kateqoriyalara bölsək, nəyin harada olduğunu anlamaq daha asan olar.

***
### 15.2.1 Hadisə Kateqoriyaları (Event Categories)

Brauzerdəki yüzlərlə hadisə növünü daha yaxşı anlamaq üçün onları bir neçə ümumi kateqoriyaya bölmək olar:

#### 1. Cihazdan Asılı Giriş Hadisələri (Device-Dependent Input Events) 🖱️⌨️
Bu hadisələr birbaşa fiziki bir cihaza (siçan, klaviatura, sensor ekran) bağlıdır və aşağı səviyyəli məlumat verir.
* **Hadisələr:** `mousedown`, `mouseup`, `mousemove`, `keydown`, `keyup`, `keypress`, `touchstart`, `touchmove`, `touchend`.
* **Məsələn:** İstifadəçinin siçanı ekranda hara tərpətdiyini dəqiqliklə izləmək üçün `mousemove` istifadə olunur.

#### 2. Cihazdan Asılı Olmayan Giriş Hadisələri (Device-Independent Input Events) 👆
Bu hadisələr daha ümumi və yüksək səviyyəlidir. Onlar müəyyən bir hərəkəti bildirir, amma bu hərəkətin hansı cihazla edildiyinin fərqi yoxdur.
* **Hadisələr:**
    * **`click`**: Bir elementin aktivləşdirilməsi (bu, siçan klikləməsi, klaviaturadan "Enter" basmaq və ya toxunma ola bilər).
    * **`input`**: Bir daxiletmə sahəsinin (`<input>`) dəyərinin dəyişməsi (bu, həm klaviatura ilə yazmaq, həm də mətni kopyalayıb yapışdırmaq (cut-and-paste) ola bilər).
    * **`pointerdown`, `pointermove`, `pointerup`**: Siçan, toxunma (touch) və qələm (stylus) kimi bütün "işarəçi" cihazlarını birləşdirən müasir hadisələr.
* **Məsələn:** Düymənin necə aktivləşdirildiyindən asılı olmayaraq, sadəcə klikləndiyini bilmək üçün `click` istifadə etmək daha məntiqlidir.

#### 3. İstifadəçi İnterfeysi (UI) Hadisələri 📝
Bunlar, adətən HTML form elementləri ilə əlaqəli yüksək səviyyəli hadisələrdir və istifadəçinin interfeyslə olan əlaqəsini bildirir.
* **Hadisələr:** `focus` (bir input sahəsi aktivləşdikdə), `blur` (fokus itdikdə), `change` (bir `select` və ya `checkbox`-ın dəyəri dəyişdikdə), `submit` (bir form göndərildikdə).
* **Məsələn:** İstifadəçi formadakı məlumatları doldurub "Göndər" düyməsinə basdıqda, məlumatları yoxlamaq üçün `submit` hadisəsini tutmaq.

#### 4. Vəziyyət Dəyişikliyi Hadisələri (State-Change Events) 🔄
Bu hadisələr birbaşa istifadəçi tərəfindən deyil, brauzer və ya şəbəkə tərəfindən, hər hansı bir vəziyyət dəyişikliyini bildirmək üçün yaradılır.
* **Hadisələr:** `load` (bütün resurslar yükləndikdə), `DOMContentLoaded` (HTML emal olunub bitdikdə), `online` və `offline` (internet bağlantısı gəlib-getdikdə), `popstate` (brauzerin "Geri" düyməsi basıldıqda).
* **Məsələn:** Bütün səhifə tam yüklənib hazır olduqda əsas proqram məntiqini işə salmaq üçün `load` hadisəsini gözləmək.

#### 5. API-yə Məxsus Hadisələr (API-Specific Events) ⚙️
Bəzi Veb API-ları (xüsusilə köhnə, `Promise`-əsaslı olmayanlar) özlərinə məxsus hadisələr təyin edir.
* **Hadisələr:**
    * `<audio>` və `<video>` elementləri üçün: `playing`, `pause`, `seeking`, `volumechange`.
    * `IndexedDB` (brauzer databazası) üçün: `success`, `error`.
    * Köhnə `XMLHttpRequest` sorğuları üçün: `load`, `error`, `progress`.
* **Məsələn:** Bir videonun oynamağa başladığını bilmək üçün onun `playing` hadisəsini tutmaq.

Əla, bro! Davam edirik. İndi isə hadisələri "tutmaq" üçün istifadə olunan funksiyaları, yəni **hadisə işləyicilərini (event handlers)** obyektlərə necə bağlaya biləcəyimizi öyrənəcəyik. Bunun bir neçə yolu var.

***
### 15.2.2 Hadisə İşləyicilərinin (Event Handlers) Qeydiyyatı
Hadisə işləyicilərini qeydiyyatdan keçirməyin iki əsas yolu var: köhnə üsul (xüsusiyyət təyin etməklə) və müasir, daha çevik üsul (`addEventListener`).

#### Üsul 1: Xüsusiyyət (Property) Təyin Etməklə 📌
Bu, ən sadə və köhnə üsuldur. Hadisə hədəfinin (event target) üzərində hadisənin adına uyğun gələn bir xüsusiyyətə (property) bir funksiya mənimsədirik. Bu xüsusiyyətlərin adları həmişə **`on`** ilə başlayır (`onclick`, `onload`, `onmouseover` və s.).

**Nümunə:**
```javascript
// Təsəvvür edək ki, HTML-də belə bir düyməmiz var:
// <button id="myButton">Kliklə</button>

const button = document.querySelector('#myButton');

// `onclick` xüsusiyyətinə bir funksiya mənimsədirik
button.onclick = function() {
  console.log("Düyməyə klikləndi!");
};

// ƏGƏR YENİ BİR FUNKSİYA MƏNİMSƏTSƏK, ƏVVƏLKİ SİLİNƏCƏK!
button.onclick = function() {
  console.log("Bu ikinci funksiyadır. Əvvəlki artıq yoxdur.");
};
```
**❗️ Əsas Çatışmazlıq:** Bu üsulun ən böyük mənfi cəhəti odur ki, bir elementin bir hadisə növü üçün **yalnız BİR** işləyicisi ola bilər. Yenisini təyin etdikdə, köhnəsi silinir.

#### HTML Atributları ilə (KÖHNƏ VƏ MƏSLƏHƏT GÖRÜLMƏYƏN ÜSUL) 📜
Hadisə işləyicilərini birbaşa HTML teqinin atributu kimi də yazmaq mümkündür.
`<button onclick="console.log('Klikləndi!');">Kliklə</button>`

**❗️ Niyə istifadə etməməliyik?** Bu üsul müasir veb proqramlaşdırmada artıq **məsləhət görülmür**, çünki o, HTML (struktur) ilə JavaScript-i (davranış) qarışdırır və arxa planda problemli `with` ifadəsini istifadə edərək çaşqınlıq yarada bilir. Sadəcə köhnə kodlarda görə biləcəyiniz üçün burada qeyd olunur.

---
#### Üsul 2: `addEventListener()` (MÜASİR VƏ TÖVSİYƏ OLUNAN ÜSUL) ➕
Bu, hadisələri idarə etməyin müasir, çevik və standart yoludur. O, eyni hadisə üçün birdən çox işləyici təyin etməyə imkan verir.

**Sintaksis:** `target.addEventListener(type, listener, options)`
* **`type`**: Hadisənin adı (sətir şəklində, "on" prefiksi olmadan, məsələn, `"click"`).
* **`listener`**: Hadisə baş verdikdə çağırılacaq callback funksiyası.
* **`options`**: Qeydiyyatın davranışını tənzimləyən bir obyekt (könüllü).

**Geniş Nümunə 1: Birdən çox "listener" əlavə etmək**
```javascript
const button = document.querySelector('#myButton');

button.addEventListener('click', () => {
  console.log("1-ci listener: Salam!");
});

button.addEventListener('click', () => {
  console.log("2-ci listener: Dünya!");
});

// Düyməyə kliklədikdə, hər iki mesaj konsolda görünəcək (ardıcıl olaraq).
```
`addEventListener()` ilə qeydiyyatdan keçirilmiş işləyicilər bir-birini silmir, əksinə, sıraya əlavə olunur.

**`removeEventListener()` ilə "listener"-i silmək**
Bir "listener"-i silmək üçün, onu qeydiyyatdan keçirərkən istifadə etdiyiniz eyni funksiyaya istinad (reference) etməlisiniz. Buna görə də anonim funksiyaları silmək olmur.

**Geniş Nümunə 2: Özünü silən "listener"**
```javascript
const button = document.querySelector('#myButton');

function handleClickOnce() {
  console.log("Bu mesaj yalnız BİR DƏFƏ görünəcək!");
  // İşini bitirdikdən sonra, özünü elementdən silir
  button.removeEventListener('click', handleClickOnce);
}

button.addEventListener('click', handleClickOnce);
```

---
#### `addEventListener` Seçimləri (Options) Obyekti ⚙️
`addEventListener`-in üçüncü arqumenti, hadisənin davranışını tənzimləyən bir obyekt ola bilər. Əsasları bunlardır:
* **`capture`**: `true` olarsa, hadisəni qabarcıqlanma (bubbling) mərhələsində deyil, ələ keçirmə (capturing) mərhələsində tutur. (Bu, növbəti bölmənin mövzusudur).
* **`once`**: `true` olarsa, "listener" ilk dəfə çağırıldıqdan sonra avtomatik olaraq silinir. Yuxarıdakı "özünü silən listener" nümunəsini yazmağın daha asan yoludur:
  `button.addEventListener('click', handleClickOnce, { once: true });`
* **`passive`**: `true` olarsa, brauzerə söz verirsiniz ki, bu "listener" içində `event.preventDefault()`-ı (defolt davranışı ləğv etmək) çağırmayacaqsınız. Bu, xüsusilə mobil cihazlarda `touchmove` kimi sürüşdürmə (scroll) hadisələrinin performansını artırmaq üçün çox vacib bir optimizasiyadır.

Aydındır, bro! Davam edirik. Bu hissədə, hadisə baş verdikdən sonra brauzerin bizim yazdığımız funksiyanı (handler) necə çağırdığını, ona hansı məlumatları ötürdüyünü və biz bu prosesə necə nəzarət edə biləcəyimizi öyrənəcəyik.

***
### 15.2.3 Hadisə İşləyicisinin (Event Handler) Çağırılması
Hadisə işləyicisini (event handler) qeydiyyatdan keçirdikdən sonra, müvafiq hadisə baş verdikdə brauzer onu avtomatik olaraq çağırır. Gəlin bu çağırışın detallarına baxaq.

#### Hadisə İşləyicisinin Arqumenti: `event` Obyekti 📦
Hər bir hadisə işləyicisi çağırıldıqda, ona arqument olaraq hadisə haqqında bütün məlumatları saxlayan bir **`Event` obyekti** ötürülür. Bu obyektin bəzi vacib xüsusiyyətləri (properties) var:

* **`type`**: Hadisənin növünü göstərən sətir (string) (məsələn, `"click"`).
* **`target`**: Hadisənin ilk baş verdiyi, ən dərindəki element (məsələn, kliklənən düymə).
* **`currentTarget`**: Hadisə dinləyicisinin (event listener) qoşulduğu element. Hadisənin yayılması (propagation) zamanı bu, `target`-dən fərqli ola bilər.
* **`timeStamp`**: Hadisənin baş verdiyi anı göstərən zaman damğası (timestamp).
* **`clientX`, `clientY`**: Siçan hadisələri üçün, kursorun pəncərə daxilindəki koordinatları.
* **`key`, `code`**: Klaviatura hadisələri üçün, basılan düymə haqqında məlumat.

**Geniş Nümunə: `event` obyektini araşdırmaq**
Tutaq ki, belə bir HTML strukturumuz var: `<div id="parent"><button id="child">Kliklə</button></div>`
```javascript
const parentDiv = document.querySelector('#parent');
const childButton = document.querySelector('#child');

parentDiv.addEventListener('click', function(event) {
  console.log("--- Valideyn Div-ə çatan hadisə ---");
  console.log("Hadisə növü (type):", event.type); // "click"
  console.log("Əsl hədəf (target):", event.target.id); // "child" (hadisə əslində düymədə başladı)
  console.log("Hazırkı hədəf (currentTarget):", event.currentTarget.id); // "parent" (listener burada qoşulub)
});
```
Düyməyə kliklədikdə, hadisə "qabarcıqlanaraq" (bubble) `div`-ə çatacaq və yuxarıdakı kod işə düşəcək.

#### Hadisə İşləyicisinin Konteksti (`this`)
* **Adi `function` ilə:** İşləyici adi `function` ilə təyin edildikdə, funksiyanın daxilindəki `this` açar sözü birbaşa hadisə dinləyicisinin qoşulduğu elementi (`currentTarget`) göstərir.
* **Ox funksiyası (`=>`) ilə:** Ox funksiyaları isə öz `this` kontekstini yaratmır, onu yazıldığı yerdəki (lexical scope) mühitdən götürür. Bu, adətən qlobal `window` obyekti olur.

**Nümunə:**
```javascript
const button = document.querySelector('#myButton');

button.addEventListener('click', function() {
  // `this` birbaşa düymə elementinin özüdür
  console.log("Adi funksiyada `this`:", this.tagName); // ✅ Nəticə: "BUTTON"
  this.style.color = "red"; // Düymənin rəngini dəyişir
});

button.addEventListener('click', () => {
  // `this` burada `window` obyektidir, düymə deyil!
  console.log("Ox funksiyasında `this`:", this); // ✅ Nəticə: Window {...}
});
```

---
### 15.2.4 Hadisənin Yayılması (Event Propagation) 🌊
Bir element üzərində hadisə baş verdikdə, o, təkcə həmin elementə təsir etmir. Hadisə DOM ağacı boyunca "səyahət" edir. Bu prosesin 3 mərhələsi var.

**Analogy:** Təsəvvür et ki, bir daşı gölə atırsan:
1.  **Ələ Keçirmə (Capturing):** Daş suya dəyməmişdən əvvəl hava qatlarından keçir (yuxarıdan aşağıya).
2.  **Hədəf (Target):** Daş suya dəyir.
3.  **Qabarcıqlanma (Bubbling):** Suya dəydikdən sonra halqalar yaranaraq kənarlara doğru yayılır (aşağıdan yuxarıya).

**Hadisənin Mərhələləri:**
1.  **Capturing Phase (↓):** Hadisə `Window`-dan başlayaraq, `Document`, `<body>` və digər elementlərdən keçərək hədəf elementə doğru "enir". Bu mərhələdə hadisələri tutmaq üçün `addEventListener`-in üçüncü arqumentini `{ capture: true }` etmək lazımdır.
2.  **Target Phase (🎯):** Hadisə birbaşa hədəf elementə çatır.
3.  **Bubbling Phase (↑):** Hadisə hədəf elementdən başlayaraq, valideyn elementlərə doğru "qalxır" və nəhayət `Window`-a çatır. **Bu, standart davranışdır.**

Əksər hadisələr (`click`, `keydown`, `keyup`...) qabarcıqlanır. Bəziləri isə (`focus`, `blur`, `scroll`...) yox.

**Nümunə: Bütün mərhələləri görmək**
HTML: `<div id="div1"><div id="div2"><button id="btn">Kliklə</button></div></div>`
```javascript
const div1 = document.querySelector('#div1');
const div2 = document.querySelector('#div2');
const btn = document.querySelector('#btn');

// Bütün elementlərə həm capturing, həm də bubbling üçün listener əlavə edirik
for (const el of [div1, div2, btn]) {
  el.addEventListener('click', e => console.log(`Capturing: ${e.currentTarget.id}`), { capture: true });
  el.addEventListener('click', e => console.log(`Bubbling:  ${e.currentTarget.id}`));
}
// Düyməyə kliklədikdə konsolda bu ardıcıllığı görəcəksiniz:
// ✅ Capturing: div1
// ✅ Capturing: div2
// ✅ Bubbling:  btn  (həm hədəf, həm də bubbling-in başlanğıcı)
// ✅ Bubbling:  div2
// ✅ Bubbling:  div1
```
---
### 15.2.5 Hadisənin Ləğv Edilməsi (Event Cancellation) 🚫
Bəzən hadisənin standart davranışının və ya onun yayılmasının qarşısını almaq lazım gəlir. Bunun üçün `event` obyektinin metodlarından istifadə edirik.

**`event.preventDefault()` — Defolt Davranışı Dayandırmaq**
Bəzi hadisələrin brauzer tərəfindən standart bir hərəkəti var. Məsələn, bir linkə (`<a>`) kliklədikdə yeni səhifəyə keçmək və ya form (`<form>`) "submit" edildikdə səhifəni yeniləmək. `preventDefault()` bu standart hərəkətin qarşısını alır.

**Nümunə: Formun göndərilməsinin qarşısını almaq**
```javascript
const myForm = document.querySelector('#myForm');
const myInput = document.querySelector('#myInput');

myForm.addEventListener('submit', function(event) {
  // Əgər input boşdursa...
  if (myInput.value.trim() === '') {
    alert("Zəhmət olmasa, sahəni doldurun!");
    // ...formun serverə göndərilməsinin qarşısını alırıq.
    event.preventDefault();
  }
});
```
**`event.stopPropagation()` — Yayılmanı (Propagation) Dayandırmaq**
Bu metod, hadisənin qabarcıqlanma (bubbling) və ya ələ keçirmə (capturing) mərhələlərində yuxarı və ya aşağı hərəkət etməsinin qarşısını alır. Hadisə hazırkı elementdəki bütün işləyiciləri icra edir, amma daha valideyn elementlərə "qalxmır".

**`event.stopImmediatePropagation()`**
Bu, `stopPropagation()`-dan daha sərtdir. O, nəinki hadisənin yayılmasını dayandırır, həm də **eyni element üzərindəki digər hadisə işləyicilərinin** də çağırılmasının qarşısını alır.

Əla, bro! Hadisələr mövzusunun sonuncu, amma çox maraqlı hissəsinə və DOM ilə işləməyə girişə baxaq.

***
### 15.2.6 Fərdi Hadisələrin Göndərilməsi (Dispatching Custom Events) 📢
Brauzerin standart hadisələri (`click`, `load` və s.) ilə məhdudlaşmırıq. Biz özümüzə məxsus, tamamilə yeni hadisələr yaradıb, onları proqram daxilində göndərə (dispatch) bilərik.

**Bu nəyə lazımdır?** 🤔
Bu, proqramın fərqli hissələrini bir-birindən asılı olmayan, "sərbəst" hala gətirmək üçün çox güclü bir yoldur.

**Analogy:** Təsəvvür et ki, proqramın bir hissəsi (məsələn, data yükləyən modul) ağır bir iş görür. Bu modul, interfeysin necə göründüyünü və ya harda bir "loading" spinner-i göstərməli olduğunu bilməməlidir. O, sadəcə bütün proqrama "Mən məşğulam!" deyə bir siqnal (hadisə) göndərə bilər. Başqa bir modul (məsələn, UI modulu) bu siqnalı eşidib, ekranda bir spinner göstərə bilər. İş bitdikdə isə data modulu "İşim bitdi" siqnalını göndərir və UI modulu spinner-i gizlədir. Bu, **komponentlərin bir-birindən xəbərsiz işləməsinə (decoupling)** imkan verir.

**API:**
* **`new CustomEvent(type, options)`**: Fərdi hadisə obyekti yaratmaq üçün istifadə olunur.
    * **`type`**: Hadisəyə verdiyimiz ad (məsələn, `"busy"`).
    * **`options`**: Hadisənin detallarını saxlayan obyekt.
        * **`detail`**: Hadisə ilə birlikdə göndərmək istədiyimiz məlumat (istənilən tipdə ola bilər).
        * **`bubbles: true`**: Hadisənin qabarcıqlanmasını (bubbling) aktivləşdirir.
* **`target.dispatchEvent(event)`**: Yaradılmış hadisəni müəyyən bir hədəf (məsələn, `document`) üzərində "atəşləyir".

**Geniş Nümunə: "Məşğul" (Busy) Vəziyyətini İdarə Etmək**
Aşağıdakı nümunədə iki fərqli modulun bir-biri ilə necə əlaqə qurduğunu görəcəksiniz.

```javascript
// --- UI Modulu (ui.js) ---
// Bu modul yalnız "busy" hadisəsini dinləyir və spinner-i göstərir/gizlədir.
// O, işin nə olduğunu və nə vaxt başladığını bilmir.

document.addEventListener("busy", (event) => {
  const isBusy = event.detail; // `detail` xüsusiyyətindən məlumatı alırıq
  const spinner = document.querySelector("#spinner"); // Təsəvvür edək ki, belə bir elementimiz var

  if (isBusy) {
    console.log("🌀 Spinner göstərilir... Proqram məşğuldur.");
    // spinner.style.display = 'block';
  } else {
    console.log("✅ Spinner gizlədilir. Proqram hazırdır.");
    // spinner.style.display = 'none';
  }
});


// --- Data Modulu (data.js) ---
// Bu modul, UI haqqında heç nə bilmir. O, sadəcə öz vəziyyəti haqqında məlumat verir.
function fetchData() {
  console.log("Məlumatların yüklənməsi başlayır...");
  
  // 1. İşə başlamazdan əvvəl "məşğulam" siqnalı göndəririk.
  document.dispatchEvent(new CustomEvent("busy", { detail: true }));

  // 2. Asinxron əməliyyatı simulyasiya edirik (setTimeout)
  setTimeout(() => {
    console.log("Məlumatlar yükləndi!");
    
    // 3. İş bitdikdən sonra "artıq məşğul deyiləm" siqnalı göndəririk.
    document.dispatchEvent(new CustomEvent("busy", { detail: false }));
  }, 3000);
}

// Gəlin prosesi başladaq!
fetchData();
```

---
### 15.3 Sənədlərin Skriptləşdirilməsi (Scripting Documents) ✍️
Client-side (brauzer) JavaScript-in əsas məqsədi statik HTML sənədlərini interaktiv veb proqramlara çevirməkdir. Bu səbəbdən, veb səhifələrinin məzmununu skriptləşdirmək JavaScript-in mərkəzi funksiyasıdır.

Hər `Window` obyektinin, pəncərədəki sənədi təmsil edən bir `document` xüsusiyyəti var. Bu `Document` obyekti, DOM API-nın mərkəzi obyektidir. DOM-un nə olduğunu §15.1.2-də öyrəndik. Bu bölmədə isə onun API-ını daha detallı araşdıracağıq.

Bu bölmədə aşağıdakı mövzuları əhatə edəcəyik:
* **🔍 Elementləri Seçmək və Sorğulamaq:** Sənəddən istədiyimiz elementləri necə tapmaq olar.
* **↔️ Sənəd Üzərində Hərəkət Etmək (Traversal):** Bir elementdən onun valideyninə (parent), övladlarına (children) və ya bacı-qardaşlarına (siblings) necə keçmək olar.
* **🏷️ Element Atributlarını Oxumaq və Dəyişmək:** Elementlərin `id`, `class`, `src` kimi atributlarını necə idarə etmək olar.
* **📝 Sənədin Məzmununu İdarə Etmək:** Elementlərin daxilindəki mətn və ya HTML-i necə dəyişmək olar.
* **🧱 Sənədin Strukturunu Dəyişmək:** Yeni elementlər yaratmaq, onları sənədə əlavə etmək və ya silmək.

Əla, bro! Artıq DOM-un nə olduğunu bilirik. İndi isə onunla real iş görməyə başlayırıq: sənədin içindən istədiyimiz HTML elementlərini tapıb, onları JavaScript obyektləri kimi "ələ keçirməyə".

***
### 15.3.1 Sənəd Elementlərinin Seçilməsi (Selecting Document Elements) 🎯

Bir elementi manipulyasiya etmək üçün, ilk növbədə, onu seçməliyik. Bütün DOM əməliyyatlarının başlanğıc nöqtəsi qlobal `document` obyektidir. Bu obyekt, səhifədəki elementləri tapmaq üçün bizə güclü metodlar təqdim edir.

#### Müasir Üsul: CSS Selektorları ilə Seçim 🔍
Elementləri seçməyin ən güclü və müasir yolu CSS selektorlarından istifadə etməkdir. Əgər CSS bilirsinizsə, bu sizə çox tanış gələcək. Bilmirsinizsə də, əsasları çox sadədir:

* `div` — Bütün `<div>` teqlərini seçir.
* `#nav` — `id`-si "nav" olan elementi seçir.
* `.warning` — `class`-ı "warning" olan bütün elementləri seçir.
* `p[lang="az"]` — `lang` atributu "az" olan bütün `<p>` teqlərini seçir.
* `#log span` — `id`-si "log" olan elementin içindəki bütün `<span>` elementlərini seçir (nəvə, nəticə və s.).
* `ul > li` — `<ul>` elementinin birbaşa övladı olan bütün `<li>` elementlərini seçir.
* `h1, h2, h3` — Bütün `<h1>`, `<h2>` və `<h3>` teqlərini seçir.

Bu selektorları istifadə edən iki əsas metod var:

**1. `document.querySelector(selector)`**
Verilmiş selektora uyğun gələn **ilk elementi** tapıb qaytarır. Heç bir element tapılmazsa, `null` qaytarır.

**2. `document.querySelectorAll(selector)`**
Verilmiş selektora uyğun gələn **bütün elementləri** bir `NodeList` obyekti şəklində qaytarır.
* **`NodeList` nədir?** Bu, massivə (array) çox bənzəyən bir kolleksiyadır. Onun da `.length` xüsusiyyəti var və elementlərinə indekslə müraciət etmək olar. O, həm də **iterable**-dır, yəni `for...of` dövrü ilə rahatlıqla istifadə edilə bilər. Əsl massivə (array) çevirmək üçün `Array.from()` istifadə edə bilərsiniz.

**Geniş Nümunə:**
Gəlin aşağıdakı HTML üzərində bu metodları yoxlayaq.
```html
<div id="main-content" class="container">
  <h1 class="title main">Başlıq</h1>
  <p>Birinci paraqraf.</p>
  <ul id="list">
    <li>Element 1</li>
    <li class="special item">Element 2</li>
    <li>Element 3</li>
  </ul>
</div>
```
```javascript
// querySelector ilə tək element seçmək
const mainDiv = document.querySelector("#main-content");
const mainTitle = mainDiv.querySelector(".title.main"); // Elementin içində də axtarış edə bilərik
const specialItem = document.querySelector("ul > li.special");

console.log(mainTitle.textContent); // ✅ Nəticə: "Başlıq"
console.log(specialItem.textContent); // ✅ Nəticə: "Element 2"

// querySelectorAll ilə çox element seçmək
const allListItems = document.querySelectorAll("#list li");
console.log(`Siyahıda ${allListItems.length} element var.`); // ✅ Nəticə: 3

// NodeList üzərində dövr etmək
for (const item of allListItems) {
  console.log("Siyahı elementi:", item.textContent);
}
```

#### Digər Faydalı Selektor Metodları (`closest` və `matches`)
* **`element.closest(selector)`**: Bu metod, elementin özündən başlayaraq DOM ağacında **yuxarıya doğru** hərəkət edir və verilmiş selektora uyğun gələn ilk "əcdadı" (ancestor) tapır. Bu, xüsusilə hadisə işləyicilərində (event handlers) çox faydalıdır.
* **`element.matches(selector)`**: Bu metod, elementin özünün verilmiş selektora uyğun gəlib-gəlmədiyini yoxlayır və `true` ya da `false` qaytarır.

**Nümunə:**
```javascript
// specialItem dəyişənini yuxarıdakı nümunədən istifadə edirik
const li2 = document.querySelector('.special');

// Bu elementin ən yaxın `.container` class-lı valideynini tapırıq
const container = li2.closest(".container");
console.log("Ən yaxın konteynerin ID-si:", container.id); // ✅ Nəticə: "main-content"

// `li2` elementinin "item" class-ına sahib olub-olmadığını yoxlayırıq
console.log("`li2`-nin 'item' class-ı var?", li2.matches(".item")); // ✅ Nəticə: true
```

---
#### Köhnə (Legacy) Seçim Metodları 📜
`querySelector` ailəsindən əvvəl, elementləri seçmək üçün başqa metodlar var idi. Onları köhnə kodlarda görə bilərsiniz.
* **`document.getElementById(id)`**: Verilmiş `id`-yə sahib olan tək elementi qaytarır. Çox sürətli işləyir və hələ də geniş istifadə olunur. (Diqqət: `"#"` işarəsi olmadan yazılır).
* **`document.getElementsByTagName(tagName)`**: Verilmiş teq adına sahib bütün elementləri qaytarır.
* **`document.getElementsByClassName(className)`**: Verilmiş `class`-a sahib bütün elementləri qaytarır.

**❗️ Vacib Fərq:** Bu köhnə metodlar (`getElementById` xaric), statik bir `NodeList` deyil, **canlı (live)** bir `HTMLCollection` qaytarır. Bu o deməkdir ki, əgər siz DOM-a yeni bir element əlavə etsəniz, bu kolleksiya avtomatik olaraq yenilənəcək. `querySelectorAll` isə statik bir "snapshot" qaytarır və yenilənmir.

Əla, bro! Sənədin içindən istədiyimiz elementi seçməyi artıq bacarırıq. İndi isə seçdiyimiz bir elementdən başlayaraq DOM ağacı boyunca "səyahət etməyi" – yəni onun valideynini, övladlarını və qonşu elementlərini tapmağı öyrənəcəyik.

***
### 15.3.2 Sənəd Quruluşu və Onun Üzərində Hərəkət (Traversal) 🌳
Bir `Element` obyektini əldə etdikdən sonra, onun xüsusiyyətlərindən (properties) istifadə edərək DOM ağacı boyunca yuxarı, aşağı və yanlara doğru hərəkət edə bilərik. Bunun üçün iki fərqli yanaşma var.

#### Üsul 1: Yalnız Elementlər Üzərində Hərəkət (Element Traversal) 👨‍👩‍👧‍👦
Bu, ən çox istifadə olunan və ən rahat üsuldur. Bu yanaşma, teqlər arasındakı boşluqları (whitespace), mətnləri və kommentləri nəzərə almadan, **yalnız `Element` qovşaqları (nodes)** arasında hərəkət etməyə imkan verir.

**Əsas Xüsusiyyətlər:**
* **`parentElement`**: Elementin valideyn (parent) elementini qaytarır.
* **`children`**: Elementin birbaşa övladı olan **yalnız elementlərdən** ibarət bir `HTMLCollection` qaytarır.
* **`childElementCount`**: Övlad elementlərin sayını qaytarır (`children.length` ilə eynidir).
* **`firstElementChild`**: İlk övlad elementi qaytarır.
* **`lastElementChild`**: Sonuncu övlad elementi qaytarır.
* **`nextElementSibling`**: Növbəti "qardaş" elementi qaytarır.
* **`previousElementSibling`**: Əvvəlki "qardaş" elementi qaytarır.

**Geniş Nümunə:**
Gəlin aşağıdakı HTML üzərində bu xüsusiyyətləri yoxlayaq.
```html
<div id="parent-div">
  <h2>Məqalə Başlığı</h2>
  <p id="p1">Bu, birinci paraqrafdır.</p>
  <p id="p2" class="highlight">Bu, ikinci paraqrafdır.</p>
  <p id="p3">Bu, üçüncü paraqrafdır.</p>
</div>
```
```javascript
// Ortadakı paraqrafı seçirik
const p2 = document.querySelector('#p2');

// İndi isə onun "ailə üzvlərini" tapaq
console.log("Valideyni (parentElement):", p2.parentElement.id); // ✅ "parent-div"
console.log("Növbəti qardaşı (nextElementSibling):", p2.nextElementSibling.id); // ✅ "p3"
console.log("Əvvəlki qardaşı (previousElementSibling):", p2.previousElementSibling.id); //✅ "p1"

// İndi isə valideyn elementin övladlarına baxaq
const parentDiv = document.querySelector('#parent-div');
console.log("Valideynin övladlarının sayı (childElementCount):", parentDiv.childElementCount); // ✅ 4
console.log("İlk övladı (firstElementChild):", parentDiv.firstElementChild.tagName); // ✅ "H2"
console.log("Son övladı (lastElementChild):", parentDiv.lastElementChild.tagName); // ✅ "P"

// Bütün övladları dövrə salaq
for (const child of parentDiv.children) {
  console.log(`- Övlad element: ${child.tagName} (id: ${child.id})`);
}
```
---
#### Üsul 2: Bütün Qovşaqlar (Nodes) Üzərində Hərəkət 🔬
Bu yanaşma daha detallıdır və təkcə elementləri deyil, həm də **`Text`** (mətn və boşluqlar) və **`Comment`** (kommentlər) kimi bütün növ qovşaqları (nodes) nəzərə alır.

**Əsas Xüsusiyyətlər:**
* **`parentNode`**: İstənilən növ valideyn qovşağı qaytarır.
* **`childNodes`**: Bütün növ övlad qovşaqlardan ibarət bir `NodeList` qaytarır.
* **`firstChild`**, **`lastChild`**: İlk və sonuncu övlad qovşağı qaytarır.
* **`nextSibling`**, **`previousSibling`**: Növbəti və əvvəlki qonşu qovşağı qaytarır.
* **`nodeType`**: Qovşağın növünü bildirən bir rəqəm (1: Element, 3: Text, 8: Comment).
* **`nodeName`**: Elementlər üçün teq adını, digərləri üçün isə xüsusi adları (`#text`, `#comment`) qaytarır.

❗️ **Ən Vacib Məqam:** Bu yanaşma HTML-dəki **boşluqlara (whitespace)** qarşı çox həssasdır. Teqlər arasında yazdığınız bir sətir boşluğu (newline) belə, bir `#text` qovşağı yaradır. Bu, `firstChild` və ya `nextSibling` kimi xüsusiyyətlərin gözləmədiyiniz bir boş mətn qovşağını qaytarmasına səbəb ola bilər.

**Nümunə: `...Sibling` və `...ElementSibling` fərqi**
Yenə eyni HTML-i istifadə edək.
```javascript
const p2 = document.querySelector('#p2');

// p2-dən sonrakı qardaş, əslində HTML-dəki boşluqdan yaranan bir #text qovşağıdır.
console.log("Növbəti qardaş qovşaq (nextSibling):", p2.nextSibling);

// Əsl <p id="p3"> elementini tapmaq üçün iki dəfə irəli getməliyik: boşluq -> komment -> boşluq -> p3
let nextNode = p2.nextSibling;
while (nextNode && nextNode.nodeType !== 1) { // Yalnız Element axtarırıq (nodeType === 1)
  nextNode = nextNode.nextSibling;
}
console.log("Əsl növbəti element qovşağı:", nextNode.id); // ✅ "p3"

// Halbuki...
console.log("ElementSibling ilə birbaşa tapmaq:", p2.nextElementSibling.id); // ✅ "p3"
```
**Nəticə:** Gündəlik işlər üçün adətən `...ElementSibling`, `children` kimi metodlar daha rahat və proqnozlaşdırılandır. Bütün qovşaqlara (nodes), o cümlədən mətn və kommentlərə ehtiyacınız olduqda isə `...Sibling`, `childNodes` kimi metodlardan istifadə edə bilərsiniz.

Davam edirik, bro! Elementləri seçməyi və onlar arasında hərəkət etməyi öyrəndik. İndi isə həmin elementlərin **atributlarını** (attributes) – yəni onların `id`, `class`, `src` kimi xüsusiyyətlərini JavaScript ilə necə idarə edəcəyimizi öyrənəcəyik.

***
### 15.3.3 Atributlar (Attributes) 🏷️
HTML elementləri bir teq adı və **atributlar** adlanan `ad=dəyər` cütlüklərindən ibarətdir. Məsələn, bir `<a>` teqinin `href` atributu, linkin hara aparacağını müəyyən edir.

Elementin atributlarını idarə etmək üçün `getAttribute()` və `setAttribute()` kimi ümumi metodlar olsa da, əksər hallarda bu atributlara birbaşa JavaScript obyektinin **xüsusiyyətləri (properties)** kimi müraciət etmək daha asan və rahatdır.

#### HTML Atributları Element Xüsusiyyəti (Property) Kimi
DOM-da hər bir HTML elementi üçün yaradılan JavaScript obyektinin, həmin elementin standart atributlarına uyğun gələn xüsusiyyətləri olur.

**Sadə Nümunə:**
```html
<img id="main-logo" src="/images/logo.png" title="Bizim Loqo">
```
```javascript
const logo = document.querySelector('#main-logo');

// Atributları birbaşa properti kimi oxuyuruq
console.log("Şəklin mənbəyi (src):", logo.src);
console.log("Elementin ID-si (id):", logo.id);

// Propertini dəyişmək, HTML atributunu da avtomatik dəyişir!
logo.title = "Yeni Başlıq";
logo.src = "/images/new-logo.png"; // Bu sətir brauzerin yeni şəkli yükləməsinə səbəb olur
```

**❗️ Adlandırma Qaydaları və İstisnalar:**
* HTML atribut adları böyük-kiçik hərfə həssas deyil, amma JavaScript xüsusiyyət adları həssasdır.
* Çoxsözlü atribut adları JavaScript-də **camelCase** formatına çevrilir. Məsələn, `tabindex` → `tabIndex`.
* JavaScript-də rezerv olunmuş sözlər fərqli adlanır. Ən məşhurları:
    * `for` ( `<label>` teqində) → `htmlFor` olur.
    * `class` → `className` olur.
* Bəzi atributlar (məsələn, `maxlength`) sətir (string) yerinə birbaşa rəqəm (number), bəziləri isə (`checked`) `boolean` (true/false) dəyəri qaytarır.

---
#### `class` Atributu: `className` və `classList` 🎨
`class` atributu bir elementə bir və ya bir neçə CSS klassı vermək üçün istifadə olunur və çox vacibdir.

* **`.className`**: Bu xüsusiyyət, `class` atributundakı bütün klassları **tək bir sətir (string)** kimi qaytarır və dəyişir. Bu, tək bir klassı əlavə etmək və ya silmək üçün çox narahatdır.
* **`.classList`**: Bu, `class` atributunu idarə etmək üçün **müasir və tövsiyə olunan** yoldur. O, `Set`-ə bənzər bir obyekt qaytarır və çox faydalı metodlara malikdir: `add()`, `remove()`, `toggle()`, `contains()`.

**Geniş Nümunə: `classList` ilə işləmək**
```html
<div id="alert-box" class="alert hidden">Bu bir mesajdır.</div>
```
```javascript
const box = document.querySelector('#alert-box');

// 1. Yeni bir klass əlavə etmək
box.classList.add('alert-success');
console.log("`add` sonrası className:", box.className); // "alert hidden alert-success"

// 2. Mövcud bir klassı silmək
box.classList.remove('hidden');
console.log("`remove` sonrası className:", box.className); // "alert alert-success"

// 3. Müəyyən bir klassın olub-olmadığını yoxlamaq
const hasAlertClass = box.classList.contains('alert');
console.log("`alert` klassı var?:", hasAlertClass); // ✅ true

// 4. Bir klassı "açıb-bağlamaq" (toggle)
// Əgər klass varsa, onu silir. Yoxdursa, əlavə edir.
box.classList.toggle('active'); // `active` klassı əlavə olundu
console.log("İlk toggle sonrası:", box.className);

box.classList.toggle('active'); // `active` klassı silindi
console.log("İkinci toggle sonrası:", box.className);
```

---
#### `data-*` Atributları və `dataset` 📦
Bəzən sırf JavaScript-də istifadə etmək üçün elementlərə xüsusi, qeyri-standart məlumatlar əlavə etmək lazım gəlir. Bunun üçün HTML-də **`data-`** prefiksi ilə başlayan atributlar yaratmaq standart bir yoldur. Bu atributlar səhifənin görünüşünə təsir etmir, ancaq kodumuz üçün faydalı məlumatlar saxlaya bilir.

JavaScript-də bu atributları oxumaq üçün elementin **`.dataset`** adlı xüsusi bir obyekti var.

**Adlandırma Qaydası:** `data-` prefiksi atılır və defislə ayrılmış sözlər camelCase formatına çevrilir:
`data-user-id` → `dataset.userId`
`data-user-role` → `dataset.userRole`

**Geniş Nümunə:**
```html
<div id="user-card" data-user-id="12345" data-user-role="admin" data-is-active="true">
  İstifadəçi Adı
</div>
```
```javascript
const card = document.querySelector('#user-card');

// `dataset` obyekti vasitəsilə məlumatları oxuyuruq
const userId = card.dataset.userId; // "12345"
const userRole = card.dataset.userRole; // "admin"

console.log(`İstifadəçi ID: ${userId}, Rolü: ${userRole}`);

// Dəyəri dəyişmək də çox asandır
card.dataset.userRole = 'editor';
console.log("Yeni rol:", card.dataset.userRole); // ✅ "editor"

// Yeni bir data atributu əlavə etmək
card.dataset.lastLogin = new Date().toISOString();
// Bu, HTML-də <div ... data-last-login="..."> atributunu yaradacaq.
console.log(card.dataset.lastLogin);
```

Əla, bro! DOM ilə işləməyə davam edirik. İndi elementlərin içindəki məzmunu (content) oxumağı və dəyişməyi, həmçinin sıfırdan yeni elementlər yaradıb səhifəyə əlavə etməyi öyrənəcəyik.

***
### 15.3.4 Elementin Məzmunu (Element Content) 📝
Bir elementin məzmununa iki fərqli şəkildə baxa bilərik: HTML teqləri ilə birlikdə və ya sadəcə saf mətn şəklində. DOM bizə hər ikisi ilə işləmək üçün imkanlar verir.

#### Məzmunla HTML Kimi İşləmək
* **`.innerHTML`**: Bir elementin daxilindəki bütün HTML məzmununu bir sətir (string) kimi alır və ya dəyişir. Bu xüsusiyyətə yeni bir HTML sətri mənimsətdikdə, brauzer həmin sətri emal (parse) edərək onu real HTML elementlərinə çevirir.
* **`.outerHTML`**: `innerHTML`-ə bənzəyir, lakin nəticəyə elementin öz teqlərini də daxil edir. Dəyişdirildikdə isə elementi özünü tamamilə əvəz edir.
* **`.insertAdjacentHTML(position, html)`**: Verilmiş HTML sətrini elementin dörd fərqli mövqeyindən birinə əlavə etməyə imkan verir: `"beforebegin"`, `"afterbegin"`, `"beforeend"`, `"afterend"`.

**Nümunə: `innerHTML` və `insertAdjacentHTML`**
```html
<div id="test-div">Bu, bir <span>testdir</span>.</div>
```
```javascript
const div = document.querySelector('#test-div');

// Məzmunu HTML kimi oxumaq
console.log("innerHTML:", div.innerHTML); // ✅ "Bu, bir <span>testdir</span>."

// Məzmunu HTML kimi tamamilə dəyişmək
div.innerHTML = "<strong>Bu, yeni və vacib bir mətndir!</strong>";

// Mövcud məzmunun sonuna yeni HTML əlavə etmək
div.insertAdjacentHTML("beforeend", " <a href='#'>daha ətraflı</a>");
```
❗️ **Təhlükəsizlik Xəbərdarlığı:** `.innerHTML`-ə istifadəçidən və ya etibar etmədiyiniz mənbədən gələn məlumatı birbaşa mənimsətməyin! Bu, XSS (Cross-Site Scripting) hücumlarına yol aça bilər.

#### Məzmunla Sadə Mətn (Plain Text) Kimi İşləmək
* **`.textContent`**: Bu, məzmunla işləməyin **standart və təhlükəsiz** yoludur. O, bir elementin və onun bütün alt elementlərinin daxilindəki **yalnız mətnləri** birləşdirərək qaytarır və bütün HTML teqlərini nəzərə almır. Bu xüsusiyyətə yeni bir dəyər mənimsətdikdə isə, bütün xüsusi simvollar (`<`, `>` və s.) HTML teqi kimi yox, sadə mətn kimi qəbul edilir.

**Geniş Nümunə: `innerHTML` vs `textContent` fərqi**
```html
<div id="demo-box">Bu <strong>qalın</strong> mətndir.</div>
```
```javascript
const box = document.querySelector('#demo-box');

// Oxuyarkən fərq
console.log("innerHTML:", box.innerHTML);     // ✅ "Bu <strong>qalın</strong> mətndir."
console.log("textContent:", box.textContent); // ✅ "Bu qalın mətndir."

// Dəyişərkən fərq
box.textContent = "Bu <strong>isə qalın deyil</strong>.";
// Nəticə olaraq brauzerdə "<strong>" teqləri mətn kimi görünəcək, qalın olmayacaq.
console.log("Dəyişdirilmiş HTML:", box.innerHTML);
// ✅ "Bu &lt;strong&gt;isə qalın deyil&lt;/strong&gt;." (Brauzer təhlükəsizlik üçün dəyişir)
```
**Bonus:** `<script>` teqinin `.text` xüsusiyyəti ilə, `type`-ı fərqli təyin edərək (məsələn, `type="text/plain"`) HTML faylınızın içində proqramınız üçün data saxlaya bilərsiniz.

---
### 15.3.5 Qovşaqların (Nodes) Yaradılması, Əlavə Edilməsi və Silinməsi 🧱
DOM bizə təkcə mövcud elementləri dəyişməyə deyil, həm də sıfırdan yeni elementlər yaradıb səhifənin istənilən yerinə əlavə etməyə və ya silməyə imkan verir.

**Müasir Metodlar:**
* **`document.createElement(tagName)`**: Verilmiş teq adına uyğun yeni bir boş `Element` yaradır.
* **`element.append(...nodes)`**: Elementin övladlarının **sonuna** yeni elementlər və ya mətnlər əlavə edir.
* **`element.prepend(...nodes)`**: Elementin övladlarının **əvvəlinə** əlavə edir.
* **`element.before(...nodes)`**: Elementin özündən **əvvələ**, onunla eyni səviyyəyə ("qardaş" kimi) əlavə edir.
* **`element.after(...nodes)`**: Elementin özündən **sonraya** əlavə edir.
* **`element.remove()`**: Elementi sənəddən tamamilə silir.
* **`element.replaceWith(...nodes)`**: Elementi yeni elementlərlə əvəz edir.

**Geniş Nümunə: Siyahını dinamik olaraq yaratmaq**
```html
<div id="list-container">
  <h1>Meyvələr</h1>
  <ul id="fruit-list">
    <li>Alma</li>
  </ul>
</div>
```
```javascript
const list = document.querySelector('#fruit-list');
const title = document.querySelector('h1');

// 1. Yeni bir element yaradırıq
const newLi = document.createElement('li');
newLi.textContent = 'Portağal';

// 2. append() ilə siyahının sonuna əlavə edirik
list.append(newLi);

// 3. prepend() ilə siyahının əvvəlinə əlavə edirik
const firstLi = document.createElement('li');
firstLi.textContent = 'Armud';
list.prepend(firstLi);

// 4. after() ilə siyahıdan sonra bir xətt əlavə edirik
list.after(document.createElement('hr'));

// 5. Bir elementi silirik
const alma = list.children[1]; // İndi almanın indeksi 1-dir
alma.remove();

// 6. Başlığı başqa bir elementlə əvəz edirik
const newTitle = document.createElement('h2');
newTitle.textContent = "Sevimli Meyvələrim";
title.replaceWith(newTitle);
```

**Elementlərin Kopyalanması (`cloneNode`)**
Bir elementi sənədin başqa bir yerinə `append` etsəniz, o, köhnə yerindən silinib yeni yerinə **köçürülür (moved)**. Onu kopyalamaq üçün `cloneNode(true)` metodundan istifadə etməlisiniz. `true` arqumenti, elementin bütün daxili məzmunu ilə birlikdə kopyalanmasını təmin edir.

```javascript
const original = document.querySelector('#p1');
const copyContainer = document.querySelector('#copy-container');

// Orijinalı KOPYALAYIB əlavə edirik
const aCopy = original.cloneNode(true);
copyContainer.append(aCopy);
```

Əla, bro! İndi isə öyrəndiyimiz bütün DOM manipulyasiya biliklərini birləşdirib, real bir layihəcik hazırlayaq: istənilən HTML sənədi üçün avtomatik olaraq bir **mündəricat (Table of Contents - TOC)** yaradan skript!

Bu, indiyə qədər öyrəndiyimiz demək olar ki, hər şeyi – element seçmə, yaratma, məzmun dəyişmə, atribut təyin etmə və DOM-a əlavə etmə – özündə birləşdirən mükəmməl bir nümunədir.

***
### 15.3.6 Geniş Nümunə: Mündəricatın Yaradılması 📚

**Məqsəd:** Səhifədəki bütün `<h2>`, `<h3>` və s. başlıq teqlərini avtomatik olaraq tapıb, nömrələyib, səhifənin əvvəlində onlara kliklənə bilən bir linklər siyahısı (mündəricat) yaratmaq.

#### Təxmini Nəticə və Stil (Styling)

Skript işə düşdükdən sonra, səhifədəki başlıqlar "1. Başlıq", "1.1. Alt Başlıq" kimi nömrələnəcək və yuxarıda həmin başlıqlara aparan bir menyu yaranacaq. Həmin mündəricatı aşağıdakı kimi CSS ilə dizayn edə bilərsiniz:
```css
#TOC { 
  border: solid black 1px; 
  margin: 10px; 
  padding: 10px; 
  background: #f0f0f0;
}
.TOCEntry { margin: 5px 0px; }
.TOCEntry a { text-decoration: none; }
.TOCLevel1 { font-size: 16pt; font-weight: bold; }
.TOCLevel2 { font-size: 14pt; margin-left: 15px; }
.TOCLevel3 { font-size: 12pt; margin-left: 30px; }
.TOCSectNum { color: #555; }
.TOCSectNum::after { content: ": "; }
```

#### JavaScript Kodu və İzahı ⚙️
Aşağıdakı kod, bütün bu işi görən tam bir skriptdir. Kodun içindəki şərhlər hər bir addımın nə etdiyini detallı şəkildə izah edir.
```javascript
// Bütün kodumuzu "DOMContentLoaded" hadisəsinin içinə yazırıq.
// Bu, skriptin yalnız bütün HTML sənədi yüklənib hazır olduqdan sonra işə düşməsini təmin edir.
document.addEventListener("DOMContentLoaded", () => {
    
  // 1. Mündəricat üçün konteyner elementi axtarırıq.
  let toc = document.querySelector("#TOC");

  // Əgər HTML-də id="TOC" olan bir element yoxdursa, özümüz birini yaradırıq.
  if (!toc) {
    toc = document.createElement("div");
    toc.id = "TOC";
    // Və onu <body>-nin ən əvvəlinə əlavə edirik.
    document.body.prepend(toc);
  }

  // 2. Sənəddəki bütün bölmə başlıqlarını (h2-dən h6-ya qədər) seçirik.
  const headings = document.querySelectorAll("h2,h3,h4,h5,h6");

  // Bölmə nömrələrini yadda saxlamaq üçün bir massiv (array) yaradırıq.
  // Məsələn, [1, 2, 1] -> 1.2.1 demək olacaq.
  const sectionNumbers = [0, 0, 0, 0, 0];

  // 3. Tapdığımız hər bir başlıq üçün dövr edirik.
  for (const heading of headings) {
    // Əgər başlıq mündəricatın öz içindədirsə, onu nəzərə almırıq.
    if (heading.parentNode === toc) {
      continue;
    }

    // Başlığın səviyyəsini təyin edirik (h2 -> 1, h3 -> 2, ...)
    const level = parseInt(heading.tagName.charAt(1)) - 1;

    // Hazırkı səviyyə üçün nömrəni bir vahid artırırıq.
    sectionNumbers[level - 1]++;
    // Ondan daha aşağı səviyyədəki bütün nömrələri isə sıfırlayırıq.
    for (let i = level; i < sectionNumbers.length; i++) {
      sectionNumbers[i] = 0;
    }

    // İndi isə tam bölmə nömrəsini yaradırıq (məs. "2.3.1")
    const sectionNumber = sectionNumbers.slice(0, level).join(".");
    
    // 4. Bölmə nömrəsini başlıq elementinin əvvəlinə əlavə edirik.
    // Bunu bir <span> içinə qoyuruq ki, CSS ilə dizayn edə bilək.
    const span = document.createElement("span");
    span.className = "TOCSectNum";
    span.textContent = sectionNumber;
    heading.prepend(span);

    // 5. Başlığa birbaşa link verə bilmək üçün bir "lövbər" (anchor) yaradırıq.
    const anchor = document.createElement("a");
    const fragmentName = `TOC${sectionNumber}`;
    anchor.name = fragmentName; // <a name="TOC2.3.1"></a>
    
    // Lövbəri başlıqdan əvvələ yerləşdiririk...
    heading.before(anchor);
    // ...və başlığı lövbərin içinə köçürürük.
    anchor.append(heading);

    // 6. İndi isə mündəricat (TOC) üçün bu bölməyə aparan bir link yaradırıq.
    const link = document.createElement("a");
    link.href = `#${fragmentName}`; // Linkin hədəfi yaratdığımız lövbərdir.

    // Başlığın məzmununu kopyalayıb linkin içinə yazırıq.
    // Bu, təhlükəsizdir, çünki məzmunu öz sənədimizdən götürürük.
    link.innerHTML = heading.innerHTML;
    
    // 7. Linki, səviyyəsinə uyğun class-a malik bir div-ə yerləşdiririk.
    const entry = document.createElement("div");
    entry.classList.add("TOCEntry", `TOCLevel${level}`);
    entry.append(link);

    // 8. Və nəhayət, bu hazır mündəricat sətrini əsas TOC konteynerinə əlavə edirik.
    toc.append(entry);
  }
});
```
Gördüyün kimi, bu skript indiyə qədər öyrəndiyimiz demək olar ki, hər şeyi – element seçmə, yaratma, məzmun dəyişmə, atribut təyin etmə və DOM-a əlavə etmə – özündə birləşdirir. Bu, DOM manipulyasiyasının gücünü göstərən əla bir nümunədir!

Əla, bro! İndi isə JavaScript ilə səhifəmizin görünüşünü, yəni CSS-i necə idarə edə biləcəyimizi öyrənəcəyik. Bu, veb proqramları dinamik və interaktiv etməyin ən vacib yollarından biridir.

***
### 15.4 CSS-in Skriptləşdirilməsi (Scripting CSS) 🎨
JavaScript təkcə sənədin məzmununu (HTML) deyil, həm də onun vizual görünüşünü və düzülüşünü (CSS) idarə edə bilir. Bunun bir neçə fərqli texnikası var.

JavaScript ilə ən çox idarə olunan bəzi CSS xüsusiyyətləri bunlardır:
* **`display`**: Elementi gizlətmək (`"none"`) və ya göstərmək (`"block"`, `"flex"` və s.) üçün.
* **`position`, `top`, `left`**: Elementləri səhifədə dinamik olaraq mövqeləndirmək üçün (məsələn, xəbərdarlıq pəncərələri, tooltip-lər).
* **`transform`**: Elementləri çevirmək, ölçüsünü dəyişmək və ya fırlatmaq üçün (`translate`, `scale`, `rotate`).
* **`transition`**: CSS dəyişikliklərini animasiyalı etmək üçün. JavaScript animasiyanı başladır, brauzer isə onu avtomatik idarə edir.

---
### 15.4.1 CSS Sinifləri (Classes) 🏷️
Bir elementin stilini dəyişməyin **ən sadə, ən təmiz və ən çox tövsiyə olunan** yolu, JavaScript ilə onun `class` atributuna CSS sinifləri (classes) əlavə etmək və ya silməkdir. Bu işi, əvvəlki bölmədə öyrəndiyimiz **`element.classList`** obyekti ilə çox rahat etmək olar.

**Geniş Nümunə: Mesaj qutusunun stilini dəyişmək**
Tutaq ki, belə bir CSS siniflərimiz var:
```css
.hidden {
  display: none;
}
.highlight {
  background-color: yellow;
  border: 1px solid red;
  font-weight: bold;
}
```
Və belə bir HTML elementimiz: `<p id="my-message">Bu bir test mesajıdır.</p>`

İndi isə JavaScript ilə bu elementin siniflərini idarə edək:
```javascript
const message = document.querySelector("#my-message");

// Elementi gizlətmək üçün "hidden" sinifini əlavə edirik
// message.classList.add("hidden");

// Elementi vurğulamaq üçün "highlight" sinifini əlavə edirik
message.classList.add("highlight");
console.log("Mesajın klassları:", message.className); // ✅ "highlight"

// Bir müddət sonra vurğulamağı ləğv etmək
setTimeout(() => {
  message.classList.remove("highlight");
  console.log("Vurğulama silindi. Klasslar:", message.className); // ✅ ""
}, 2000);

// toggle metodu isə sinifi "açıb-bağlayır"
// setInterval(() => message.classList.toggle('highlight'), 500);
```

---
### 15.4.2 Sətirdaxili (Inline) Stillər 🖌️
Bəzən bir stil təkcə bir elementə aiddir və dinamik olaraq hesablanır (məsələn, siçanın kursorunun dəqiq koordinatları). Belə hallar üçün hər ehtimala qarşı bir CSS sinifi yaratmaq mümkün deyil. Bu zaman biz birbaşa elementin `style` atributunu, yəni sətirdaxili (inline) stilləri JavaScript ilə idarə edirik.

Hər bir HTML elementinin, onun `style` atributunu təmsil edən bir **`.style`** xüsusiyyəti (property) var. Bu xüsusiyyət sətir (string) deyil, **`CSSStyleDeclaration`** adlı bir obyektdir. Bu obyektin xüsusiyyətlərini dəyişərək elementin inline stillərini təyin edə bilərik.

**🚨 Adlandırma Qaydası:** CSS-də defislə (`-`) yazılan xüsusiyyətlər, JavaScript-də **camelCase** formatında yazılır:
* `font-size` → `style.fontSize`
* `background-color` → `style.backgroundColor`
* `border-left-width` → `style.borderLeftWidth`

**🚨 Dəyərlər Həmişə Sətir (String) Olmalıdır:** `.style` obyektinin xüsusiyyətlərinə dəyər mənimsədərkən, dəyər həmişə sətir (string) olmalıdır və lazım gəldikdə "px", "%", "em" kimi ölçü vahidləri də daxil edilməlidir.
`el.style.width = 200;` ❌ (SƏHV)
`el.style.width = "200px";` ✅ (DÜZGÜN)

**Geniş Nümunə: Elementi dinamik olaraq mövqeləndirmək**
```javascript
// Tutaq ki, HTML-də belə bir tooltip elementimiz var
// <div id="tooltip" class="hidden"></div>

const tooltip = document.querySelector("#tooltip");

// Bu funksiya, tooltip-i verilmiş koordinatlarda göstərir
function showTooltip(text, x, y) {
  tooltip.textContent = text;
  
  // Stil xüsusiyyətlərini təyin edirik
  tooltip.style.left = x + "px"; // "150px" kimi bir dəyər olur
  tooltip.style.top = y + "px";  // "200px" kimi bir dəyər olur
  
  // Və onu görünən edirik
  tooltip.style.display = "block";
}

// İstifadəsi (məsələn, bir siçan hərəkəti zamanı)
// showTooltip("Bu bir tooltip-dir!", 150, 200);

// Gizlətmək üçün isə:
// tooltip.style.display = "none";
```

**`.style` Obyektinin Məhdudiyyəti:**
`.style` xüsusiyyəti yalnız elementin `style="..."` atributu ilə təyin edilmiş **inline stilləri** oxuya və dəyişə bilir. O, xarici CSS faylından və ya `<style>` teqindən gələn stilləri **görmür**. Bir elementin bütün stillərini (həm inline, həm CSS faylından gələn) bilmək üçün, növbəti bölmədə öyrənəcəyimiz "hesablanmış stillər"dən (computed styles) istifadə etmək lazımdır.

Əla, bro! Gəldik CSS-i JavaScript ilə idarə etməyə. Bu, veb səhifələrimizə təkcə məzmun deyil, həm də vizual dinamiklik qatmaq üçün çox vacib bir bacarıqdır.

***
### 15.4.3 Hesablanmış Stillər (Computed Styles) 📐
Bir elementin `.style` xüsusiyyəti yalnız onun `style="..."` atributu ilə verilmiş **sətirdaxili (inline)** stillərini göstərir. Bəs elementin xarici CSS faylından və ya `<style>` teqindən gələn stillərini necə bilmək olar?

Cavab: **Hesablanmış stillər (computed styles)**. Bu, brauzerin bir elementi ekranda göstərmək üçün istifadə etdiyi **bütün stil qaydalarının yekun nəticəsidir.**

Bu yekun nəticəni əldə etmək üçün `window.getComputedStyle(element)` funksiyasından istifadə edirik.

**`element.style` ilə `getComputedStyle()` arasındakı fərqlər:**
1.  **Yalnız-Oxunaqlıdır (Read-Only):** Hesablanmış stilləri dəyişmək olmaz, onlara yalnız baxmaq olar.
2.  **Dəyərlər Mütləqdir (Absolute):** Bütün nisbi ölçü vahidləri (`%`, `em`, `pt`) piksellərə (`px`) çevrilir. Rənglər isə `rgb()` və ya `rgba()` formatında göstərilir.
3.  **Qısayollar (Shortcuts) Yoxdur:** `margin` və ya `border` kimi qısayol xüsusiyyətlərini sorğulamaq olmur. Onların `marginLeft`, `borderTopWidth` kimi tərkib hissələrini bir-bir sorğulamaq lazımdır.

**Geniş Nümunə:**
```css
/* style.css faylı */
.my-box {
  width: 50%; /* Nisbi ölçü */
  font-size: 1.2em;
  border: 1px solid black;
  color: blue; /* CSS-dən gələn rəng */
}
```
```html
<div id="box1" class="my-box" style="color: red; padding: 20px;">Mətn</div>
```
```javascript
const box = document.querySelector("#box1");
const computedStyles = window.getComputedStyle(box);

// 1. .style ilə yalnız inline stilləri görə bilirik
console.log(".style.color:", box.style.color); // ✅ "red"
console.log(".style.width:", box.style.width); // ✅ "" (boş, çünki inline deyil)

console.log("--- Hesablanmış Stillər ---");

// 2. getComputedStyle ilə isə yekun nəticəni görürük
console.log("Hesablanmış color:", computedStyles.color); // ✅ "rgb(255, 0, 0)" (inline stil daha güclüdür)
console.log("Hesablanmış width:", computedStyles.width); // ✅ "512px" (təxmini, brauzer %-i px-ə çevirdi)
console.log("Hesablanmış font-size:", computedStyles.fontSize); // ✅ "19.2px" (təxmini, em-i px-ə çevirdi)
console.log("Hesablanmış padding-left:", computedStyles.paddingLeft); // ✅ "20px" (inline stilin içindəki padding)
```

---
### 15.4.4 Stil Cədvəllərinin (Stylesheets) Skriptləşdirilməsi 🎨
JavaScript ilə təkcə tək bir elementin stilini deyil, bütöv bir stil cədvəlini (**stylesheet**) manipulyasiya etmək olar. Səhifəyə qoşulmuş `<link rel="stylesheet">` və ya daxili `<style>` teqləri də adi DOM elementləridir və onları seçib idarə edə bilərik.

**Nümunə 1: Stil cədvəlini aktiv/deaktiv etmək (Mövzu dəyişdirici - Theme Toggle)**
Tutaq ki, HTML-də iki fərqli mövzu (tema) üçün stil faylımız var:
```html
<link id="light-theme" rel="stylesheet" href="light.css">
<link id="dark-theme" rel="stylesheet" href="dark.css" disabled>
```
```javascript
function toggleTheme() {
  const lightTheme = document.querySelector("#light-theme");
  const darkTheme = document.querySelector("#dark-theme");

  // `disabled` xüsusiyyəti `true` olarsa, həmin stylesheet deaktiv olur
  if (darkTheme.disabled) {
    // Hal-hazırda işıqlı mövzudayıq, qaranlığa keçirik
    lightTheme.disabled = true;
    darkTheme.disabled = false;
    console.log("Mövzu dəyişdirildi: Qaranlıq 🌒");
  } else {
    // Qaranlıq mövzudayıq, işıqlıya keçirik
    lightTheme.disabled = false;
    darkTheme.disabled = true;
    console.log("Mövzu dəyişdirildi: İşıqlı ☀️");
  }
}
// toggleTheme() funksiyasını bir düymənin klikinə bağlaya bilərsiniz.
```

**Nümunə 2: Səhifəyə dinamik olaraq yeni stil əlavə etmək**
```javascript
// Bu "əyləncəli" kod, səhifədəki hər şeyi 180 dərəcə fırladır :)
document.head.insertAdjacentHTML(
  "beforeend",
  "<style>body { transform: rotate(180deg); transition: transform 1s; }</style>"
);
```
---
### 15.4.5 CSS Animasiyaları və Hadisələri 🎬
JavaScript-in ən güclü tərəflərindən biri, CSS ilə təyin olunmuş animasiyaları **tətikləmək (trigger)** və onların gedişatını **izləməkdir**.

**Məntiq:** Animasiyanı CSS edir, JavaScript isə sadəcə "başla" əmrini verir.

Bu, adətən bir elementə xüsusi bir CSS sinifi (class) əlavə etməklə edilir.

**Geniş Nümunə: Elementi yox etmək (Fade Out) və hadisəsini tutmaq**
CSS-də belə siniflərimiz olsun:
```css
.fadeable {
  transition: opacity 0.5s ease; /* Opacity dəyişəndə 0.5s-yə animasiya et */
}
.fade-out {
  opacity: 0; /* Tamamilə şəffaf et */
}
```
HTML-də isə belə bir qutu: `<div id="my-box" class="fadeable">Salam!</div>`

İndi JavaScript ilə animasiyanı başladaq və bitməsini gözləyək:
```javascript
const box = document.querySelector("#my-box");
const fadeButton = document.querySelector("#fade-button"); // Təsəvvüri düymə

// Düyməyə kliklədikdə animasiyanı başladaq
fadeButton.onclick = () => {
  console.log("Animasiya başlayır...");
  box.classList.add("fade-out"); // Bu sətir animasiyanı tətikləyir!
};

// `transitionend` hadisəsi, CSS transition animasiyası bitdikdə işə düşür
box.addEventListener("transitionend", () => {
  console.log("✅ Animasiya tamamilə bitdi!");
  // Animasiya bitdikdən sonra elementi tamamilə DOM-dan silə bilərik
  // box.remove(); 
});
```
Eyni məntiq daha mürəkkəb `@keyframes` ilə qurulan **CSS Animations** üçün də keçərlidir. Onların da `animationstart`, `animationend` kimi öz hadisələri var.


Olar, bro! Keçirik veb proqramlaşdırmanın ən praktik mövzularından birinə: elementlərin səhifədəki ölçüsünü, mövqeyini və sürüşdürmə (scroll) vəziyyətini JavaScript ilə necə idarə edəcəyimizə.

Bro, tamamilə haqlısan, üzrlü say. Tələsiklikdən bəzi ingiliscə terminləri əlavə etməyi unutmuşam. Düzəliş üçün sağ ol! Gəl həmin fəsli düzgün, terminlərlə birlikdə yenidən göndərim.

***
### 15.5 Sənəd Həndəsəsi (Geometry) və Sürüşdürmə (Scrolling) 📐

İndiyə qədər sənədə bir element ağacı kimi baxdıq. Amma brauzer bu sənədi ekranda göstərəndə (render), hər bir elementin bir **ölçüsü** və **mövqeyi** olur. Bəzən, xüsusilə dinamik interfeyslər (məsələn, bir elementin yanında tooltip göstərmək) yaratmaq üçün bu həndəsi məlumatları bilməliyik.

---
### 15.5.1 Sənəd və Görünüş Sahəsi Koordinatları (Document and Viewport Coordinates)
Bir elementin mövqeyini ölçərkən iki fərqli koordinat sistemindən istifadə edirik:

* **Görünüş Sahəsi Koordinatları (Viewport Coordinates):** Bu, elementin pəncərənin o an **görünən hissəsinin** yuxarı-sol küncünə nəzərən mövqeyidir. Bu, ən çox istifadə olunan sistemdir.
* **Sənəd Koordinatları (Document Coordinates):** Bu, elementin bütün HTML sənədinin (sürüşdürmə nəzərə alınmadan) ən yuxarı-sol küncünə nəzərən mövqeyidir.

**Analogy:** Təsəvvür et ki, uzun bir papirus tumarını (sənəd) oxuyursan.
* Sənin o an **gözünlə gördüyün hissə** `viewport`-dur. Bir şəklin bu hissəyə görə mövqeyi **viewport koordinatıdır**.
* Şəklin tumarın **ən başından** olan məsafəsi isə **sənəd koordinatıdır**.
Sən tumarı aşağı sürüşdürdükcə, şəklin viewport-dakı mövqeyi dəyişir, amma sənəddəki mövqeyi sabit qalır.

Bu iki sistem arasındakı əlaqəni **sürüşdürmə məsafəsi (scroll offset)** ilə qurmaq olar: `viewportY = documentY - window.scrollY`.

Ancaq iç-içə sürüşən elementlər səbəbindən sənəd koordinatları ilə işləmək çətinləşir. Buna görə də, demək olar ki, bütün müasir JavaScript API-ları **viewport koordinatları** ilə işləyir.

---
### 15.5.2 Elementin Həndəsəsini (Geometry) Sorğulamaq 📏
Bir elementin ölçüsünü və viewport-dakı mövqeyini öyrənmək üçün ən yaxşı yol onun `.getBoundingClientRect()` metodunu çağırmaqdır.

Bu metod, aşağıdakı xüsusiyyətlərə malik bir obyekt qaytarır:
* `left`, `top`: Elementin yuxarı-sol küncünün viewport-a nəzərən x və y koordinatları.
* `right`, `bottom`: Elementin aşağı-sağ küncünün koordinatları.
* `width`, `height`: Elementin eni və hündürlüyü (padding və border daxil).

**Geniş Nümunə:**
```html
<div id="box" style="width: 200px; height: 100px; margin: 50px; border: 5px solid black;"></div>
```
```javascript
const box = document.querySelector("#box");
const rect = box.getBoundingClientRect();

console.log("Elementin həndəsəsi:", rect);
// Nəticə (təxmini):
// {
//   x: 50, y: 50, 
//   width: 210, height: 110,  // (200px en + 2*5px border)
//   top: 50, right: 260, bottom: 160, left: 50
// }

console.log(`Elementin yuxarı-sol küncü (${rect.left}, ${rect.top}) mövqeyindədir.`);
```
---
### 15.5.3 Verilmiş Nöqtədəki Elementi Təyin Etmək
`.getBoundingClientRect()`-in əksi olan bir əməliyyatdır: verilmiş `(x, y)` koordinatında hansı elementin olduğunu tapmaq. Bunun üçün `document.elementFromPoint(x, y)` metodundan istifadə edirik.

**Nümunə: Kliklənən elementi tapmaq**
```javascript
document.addEventListener('click', (event) => {
  // Klikin viewport koordinatlarını götürürük
  const x = event.clientX;
  const y = event.clientY;

  // Həmin nöqtədəki ən üst səviyyəli elementi tapırıq
  const elementUnderCursor = document.elementFromPoint(x, y);

  if (elementUnderCursor) {
    console.log(`Siz bir <${elementUnderCursor.tagName}> elementinə kliklədiniz!`);
  }
});
```

---
### 15.5.4 Sürüşdürmə (Scrolling) 📜
JavaScript ilə həm bütün pəncərəni, həm də ayrı-ayrı elementləri sürüşdürmək mümkündür.

* **`window.scrollTo(x, y)`**: Pəncərəni mütləq sənəd koordinatlarına sürüşdürür.
* **`window.scrollBy(x, y)`**: Pəncərəni hazırkı mövqeyindən nisbi olaraq sürüşdürür.
* **`element.scrollIntoView()`**: Səhifəni həmin element görünənə qədər sürüşdürür.

Bu metodların hamısı, **yumşaq sürüşdürmə (smooth scrolling)** üçün `{ behavior: 'smooth' }` opsiyasını qəbul edir.

**Geniş Nümunə: Səhifə içi naviqasiya**
```html
<a href="#footer" id="go-to-bottom">Səhifənin sonuna get</a>
<footer id="page-footer">Bu səhifənin sonudur.</footer>
```
```javascript
const link = document.querySelector("#go-to-bottom");
const footer = document.querySelector("#page-footer");

link.addEventListener('click', (event) => {
  // Linkin standart davranışının (səhifəni qəfil atmasının) qarşısını alırıq
  event.preventDefault(); 
  
  console.log("Səhifənin sonuna yumşaq sürüşdürmə başlayır...");
  
  // `footer` elementini görünüşə gətiririk
  footer.scrollIntoView({
    behavior: 'smooth', // Yumşaq animasiya ilə
    block: 'start'      // Elementin yuxarı kənarını viewport-un yuxarısına hizala
  });
});
```
---
### 15.5.5 Ölçülər və Sürüşdürmə Vəziyyəti
Bəzən viewport-un, bütün sənədin və ya bir elementin daxili məzmununun ölçülərini bilmək lazım gəlir.

**Pəncərə üçün:**
* `window.innerWidth`, `window.innerHeight`: Viewport-un eni və hündürlüyü.
* `window.scrollX`, `window.scrollY`: Üfüqi və şaquli sürüşdürmə məsafəsi (scroll offset).

**Element üçün (ən vacibləri):**
Bir elementin ölçüləri üçün 3 fərqli xüsusiyyət ailəsi var. Bunu bir şəkil çərçivəsi kimi təsəvvür edək:
* **`offsetWidth`, `offsetHeight`**: Elementin tam xarici ölçüsü (məzmun + padding + border). _(Çərçivənin tam ölçüsü)_
* **`clientWidth`, `clientHeight`**: Elementin daxili ölçüsü (məzmun + padding, amma border **xaric**). _(Çərçivənin şüşəli hissəsi)_
* **`scrollWidth`, `scrollHeight`**: Elementin bütün (həm görünən, həm də sürüşdürülərək görünə bilən) məzmununun tam ölçüsü. _(Çərçivəyə sığmayıb bükülmüş şəklin tam ölçüsü)_

**Elementin sürüşdürülməsini idarə etmək:**
* **`scrollTop`, `scrollLeft`**: Elementin daxili məzmununun nə qədər sürüşdürüldüyünü göstərir. Bu xüsusiyyətlər həm də **yazıla biləndir (writable)**!

**Nümunə: Div-i proqramatik sürüşdürmək**
```html
<div id="scrollable-div" style="height: 100px; overflow-y: scroll; border: 1px solid;">
  <p>Çox uzun mətn...</p> </div>
<button id="scroll-btn">50px aşağı sürüşdür</button>
```
```javascript
const scrollableDiv = document.querySelector("#scrollable-div");
const scrollBtn = document.querySelector("#scroll-btn");

console.log("Div-in görünən hündürlüyü (clientHeight):", scrollableDiv.clientHeight); // ~100
console.log("Div-in tam məzmun hündürlüyü (scrollHeight):", scrollableDiv.scrollHeight); // >100

scrollBtn.addEventListener('click', () => {
  // `scrollTop` xüsusiyyətini dəyişərək div-i aşağı sürüşdürürük
  scrollableDiv.scrollTop += 50;
});
```Bro, tamamilə haqlısan, üzrlü say. Tələsiklikdən bəzi ingiliscə terminləri əlavə etməyi unutmuşam. Düzəliş üçün sağ ol! Gəl həmin fəsli düzgün, terminlərlə birlikdə yenidən göndərim.

***
### 15.5 Sənəd Həndəsəsi (Geometry) və Sürüşdürmə (Scrolling) 📐

İndiyə qədər sənədə bir element ağacı kimi baxdıq. Amma brauzer bu sənədi ekranda göstərəndə (render), hər bir elementin bir **ölçüsü** və **mövqeyi** olur. Bəzən, xüsusilə dinamik interfeyslər (məsələn, bir elementin yanında tooltip göstərmək) yaratmaq üçün bu həndəsi məlumatları bilməliyik.

---
### 15.5.1 Sənəd və Görünüş Sahəsi Koordinatları (Document and Viewport Coordinates)
Bir elementin mövqeyini ölçərkən iki fərqli koordinat sistemindən istifadə edirik:

* **Görünüş Sahəsi Koordinatları (Viewport Coordinates):** Bu, elementin pəncərənin o an **görünən hissəsinin** yuxarı-sol küncünə nəzərən mövqeyidir. Bu, ən çox istifadə olunan sistemdir.
* **Sənəd Koordinatları (Document Coordinates):** Bu, elementin bütün HTML sənədinin (sürüşdürmə nəzərə alınmadan) ən yuxarı-sol küncünə nəzərən mövqeyidir.

**Analogy:** Təsəvvür et ki, uzun bir papirus tumarını (sənəd) oxuyursan.
* Sənin o an **gözünlə gördüyün hissə** `viewport`-dur. Bir şəklin bu hissəyə görə mövqeyi **viewport koordinatıdır**.
* Şəklin tumarın **ən başından** olan məsafəsi isə **sənəd koordinatıdır**.
Sən tumarı aşağı sürüşdürdükcə, şəklin viewport-dakı mövqeyi dəyişir, amma sənəddəki mövqeyi sabit qalır.

Bu iki sistem arasındakı əlaqəni **sürüşdürmə məsafəsi (scroll offset)** ilə qurmaq olar: `viewportY = documentY - window.scrollY`.

Ancaq iç-içə sürüşən elementlər səbəbindən sənəd koordinatları ilə işləmək çətinləşir. Buna görə də, demək olar ki, bütün müasir JavaScript API-ları **viewport koordinatları** ilə işləyir.

---
### 15.5.2 Elementin Həndəsəsini (Geometry) Sorğulamaq 📏
Bir elementin ölçüsünü və viewport-dakı mövqeyini öyrənmək üçün ən yaxşı yol onun `.getBoundingClientRect()` metodunu çağırmaqdır.

Bu metod, aşağıdakı xüsusiyyətlərə malik bir obyekt qaytarır:
* `left`, `top`: Elementin yuxarı-sol küncünün viewport-a nəzərən x və y koordinatları.
* `right`, `bottom`: Elementin aşağı-sağ küncünün koordinatları.
* `width`, `height`: Elementin eni və hündürlüyü (padding və border daxil).

**Geniş Nümunə:**
```html
<div id="box" style="width: 200px; height: 100px; margin: 50px; border: 5px solid black;"></div>
```
```javascript
const box = document.querySelector("#box");
const rect = box.getBoundingClientRect();

console.log("Elementin həndəsəsi:", rect);
// Nəticə (təxmini):
// {
//   x: 50, y: 50, 
//   width: 210, height: 110,  // (200px en + 2*5px border)
//   top: 50, right: 260, bottom: 160, left: 50
// }

console.log(`Elementin yuxarı-sol küncü (${rect.left}, ${rect.top}) mövqeyindədir.`);
```
---
### 15.5.3 Verilmiş Nöqtədəki Elementi Təyin Etmək
`.getBoundingClientRect()`-in əksi olan bir əməliyyatdır: verilmiş `(x, y)` koordinatında hansı elementin olduğunu tapmaq. Bunun üçün `document.elementFromPoint(x, y)` metodundan istifadə edirik.

**Nümunə: Kliklənən elementi tapmaq**
```javascript
document.addEventListener('click', (event) => {
  // Klikin viewport koordinatlarını götürürük
  const x = event.clientX;
  const y = event.clientY;

  // Həmin nöqtədəki ən üst səviyyəli elementi tapırıq
  const elementUnderCursor = document.elementFromPoint(x, y);

  if (elementUnderCursor) {
    console.log(`Siz bir <${elementUnderCursor.tagName}> elementinə kliklədiniz!`);
  }
});
```

---
### 15.5.4 Sürüşdürmə (Scrolling) 📜
JavaScript ilə həm bütün pəncərəni, həm də ayrı-ayrı elementləri sürüşdürmək mümkündür.

* **`window.scrollTo(x, y)`**: Pəncərəni mütləq sənəd koordinatlarına sürüşdürür.
* **`window.scrollBy(x, y)`**: Pəncərəni hazırkı mövqeyindən nisbi olaraq sürüşdürür.
* **`element.scrollIntoView()`**: Səhifəni həmin element görünənə qədər sürüşdürür.

Bu metodların hamısı, **yumşaq sürüşdürmə (smooth scrolling)** üçün `{ behavior: 'smooth' }` opsiyasını qəbul edir.

**Geniş Nümunə: Səhifə içi naviqasiya**
```html
<a href="#footer" id="go-to-bottom">Səhifənin sonuna get</a>
<footer id="page-footer">Bu səhifənin sonudur.</footer>
```
```javascript
const link = document.querySelector("#go-to-bottom");
const footer = document.querySelector("#page-footer");

link.addEventListener('click', (event) => {
  // Linkin standart davranışının (səhifəni qəfil atmasının) qarşısını alırıq
  event.preventDefault(); 
  
  console.log("Səhifənin sonuna yumşaq sürüşdürmə başlayır...");
  
  // `footer` elementini görünüşə gətiririk
  footer.scrollIntoView({
    behavior: 'smooth', // Yumşaq animasiya ilə
    block: 'start'      // Elementin yuxarı kənarını viewport-un yuxarısına hizala
  });
});
```
---
### 15.5.5 Ölçülər və Sürüşdürmə Vəziyyəti
Bəzən viewport-un, bütün sənədin və ya bir elementin daxili məzmununun ölçülərini bilmək lazım gəlir.

**Pəncərə üçün:**
* `window.innerWidth`, `window.innerHeight`: Viewport-un eni və hündürlüyü.
* `window.scrollX`, `window.scrollY`: Üfüqi və şaquli sürüşdürmə məsafəsi (scroll offset).

**Element üçün (ən vacibləri):**
Bir elementin ölçüləri üçün 3 fərqli xüsusiyyət ailəsi var. Bunu bir şəkil çərçivəsi kimi təsəvvür edək:
* **`offsetWidth`, `offsetHeight`**: Elementin tam xarici ölçüsü (məzmun + padding + border). _(Çərçivənin tam ölçüsü)_
* **`clientWidth`, `clientHeight`**: Elementin daxili ölçüsü (məzmun + padding, amma border **xaric**). _(Çərçivənin şüşəli hissəsi)_
* **`scrollWidth`, `scrollHeight`**: Elementin bütün (həm görünən, həm də sürüşdürülərək görünə bilən) məzmununun tam ölçüsü. _(Çərçivəyə sığmayıb bükülmüş şəklin tam ölçüsü)_

**Elementin sürüşdürülməsini idarə etmək:**
* **`scrollTop`, `scrollLeft`**: Elementin daxili məzmununun nə qədər sürüşdürüldüyünü göstərir. Bu xüsusiyyətlər həm də **yazıla biləndir (writable)**!

**Nümunə: Div-i proqramatik sürüşdürmək**
```html
<div id="scrollable-div" style="height: 100px; overflow-y: scroll; border: 1px solid;">
  <p>Çox uzun mətn...</p> </div>
<button id="scroll-btn">50px aşağı sürüşdür</button>
```
```javascript
const scrollableDiv = document.querySelector("#scrollable-div");
const scrollBtn = document.querySelector("#scroll-btn");

console.log("Div-in görünən hündürlüyü (clientHeight):", scrollableDiv.clientHeight); // ~100
console.log("Div-in tam məzmun hündürlüyü (scrollHeight):", scrollableDiv.scrollHeight); // >100

scrollBtn.addEventListener('click', () => {
  // `scrollTop` xüsusiyyətini dəyişərək div-i aşağı sürüşdürürük
  scrollableDiv.scrollTop += 50;
});
```
Hə, bro! İndi isə müasir frontend proqramlaşdırmanın ən maraqlı və vacib mövzularından birinə başlayırıq: **Veb Komponentləri (Web Components)**. Bu, öz fərdi HTML teqlərimizi yaratmaq üçün brauzerin bizə verdiyi bir super gücdür!

***
### 15.6 Veb Komponentləri (Web Components) 🧩
HTML bizə sənədləri qurmaq üçün `<p>`, `<h1>`, `<input>` kimi bir çox teq verir. Lakin müasir istifadəçi interfeysləri (UI) üçün bu teqlər çox vaxt kifayət etmir.

Məsələn, müasir bir axtarış qutusu (search box) yaratmaq üçün sadəcə bir `<input>` teqi bəs deyil. Bizə adətən bunları birləşdirmək lazım gəlir:
* Elementləri saxlayan bir konteyner `<div>`.
* Mətni daxil etmək üçün bir `<input>`.
* Axtarış və ləğv etmə işarələri üçün iki `<img>` və ya `<span>`.
* Bütün bunların görünüşünü və davranışını idarə etmək üçün xeyli CSS və JavaScript.

Hər dəfə axtarış qutusu lazım olanda bütün bunları sıfırdan yazmaq əvəzinə, tək bir `<search-box>` teqi kimi istifadə edə biləcəyimiz hazır bir komponent yaratmaq daha yaxşı olmazdımı?

Böyük JavaScript freymvorkları (React, Angular, Vue) məhz bu problemi həll etmək üçün komponent modelini təqdim edir. **Veb Komponentləri (Web Components)** isə, bu işi heç bir freymvork olmadan, birbaşa brauzerin öz daxili standartları ilə görməyə imkan verən bir texnologiyalar toplusudur.

---
### 15.6.1 Veb Komponentlərinin İstifadəsi (Using Web Components) 🧱
Başqa bir developer tərəfindən yaradılmış və ya öz yaratdığınız bir veb komponentini səhifənizdə istifadə etmək çox asandır.

#### Addım 1: Skripti Qoşmaq (Including the Script)
Veb komponentləri JavaScript ilə təyin olunduğu üçün, ilk növbədə onun `.js` faylını HTML sənədinizə qoşmalısınız. Onlar adətən müasir modul (module) formatında yazılır.
```html
<script type="module" src="components/search-box.js"></script>
```

#### Addım 2: Fərdi Teqi İşlətmək (Using the Custom Tag)
Skripti qoşduqdan sonra, komponentin təyin etdiyi xüsusi HTML teqini adi bir teq kimi istifadə edə bilərsiniz.
```html
<search-box placeholder="Axtar..."></search-box>
```
❗️ **Qızıl Qayda:** Fərdi element adlarında **mütləq bir defis (`-`)** olmalıdır (`search-box`, `my-button`, `user-profile`). Bu, gələcəkdəki standart HTML teqləri ilə adların qarışmasının qarşısını alır. `searchbox` adı keçərli deyil, amma `search-box` keçərlidir.

#### Addım 3: "Slot"-lar ilə Məzmun Yerləşdirmək
Bəzi komponentlər, onların daxilinə öz elementlərinizi yerləşdirmək üçün xüsusi "yuvalar" – yəni **slot-lar** təqdim edir. Hansı elementin hansı yuvaya gedəcəyini bildirmək üçün uşaq elementin üzərində `slot` atributundan istifadə olunur.

**Geniş Nümunə: Axtarış qutusuna ikonlar əlavə etmək**
Tutaq ki, `<search-box>` komponentimiz solda və sağda ikonlar üçün `left` və `right` adlı slotlar təqdim edir.
```html
<search-box>
  <img src="images/search-icon.png" slot="left">
  
  <img src="images/cancel-icon.png" slot="right">
</search-box>
```
#### Komponentin Yüklənmə Prosesi və CSS "Tricki"
Brauzer, `<search-box>` teqini görəndə, onun davranışını təyin edən JavaScript faylı hələ yüklənməmiş ola bilər. Bu normaldır. Brauzer əvvəlcə onu adi bir element kimi yaradır, JavaScript faylı yüklənib icra olunduqdan sonra isə həmin elementi "təkmilləşdirir" (upgrades) və ona bütün funksionallığını verir.

Bu təkmilləşmə anına qədər komponentin düzgün görünməməsi və səhifədə "atılıb-düşmə" yaratmaması üçün belə bir CSS-dən istifadə etmək olar:
```css
/* :not(:defined) - hələ təyin olunmamış (yəni JS-i yüklənməmiş)
   bütün search-box elementlərini seçir. */
search-box:not(:defined) {
  /* Təkmilləşənə qədər elementi şəffaf et */
  opacity: 0;
  
  /* Amma yerini tutması üçün ölçülərini ver */
  display: inline-block;
  width: 300px;
}
```

#### JavaScript ilə İdarə Etmək
Veb komponentləri adi HTML elementləri kimidir. Onları `document.querySelector()` ilə seçib, xüsusiyyətlərini dəyişə və metodlarını çağıra bilərsiniz.
```javascript
// Tutaq ki, komponentin 'value' adlı bir xüsusiyyəti və 'clear()' adlı bir metodu var
const search = document.querySelector('search-box');

// Dəyərini oxumaq
let currentValue = search.value;

// Dəyərini dəyişmək
search.value = "Yeni axtarış";

// Metodunu çağırmaq
search.clear(); 
```
İndi isə gəlin görək, bu veb komponentlərini yaradan üç əsas texnologiya nədir.

Düzdü, bro. Veb Komponentləri yaradan üç əsas texnologiyadan ilk ikisi ilə tanış olaq. Bunlar bir-birini tamamlayan çox güclü vasitələrdir.

***
### 15.6.2 HTML Şablonları (Templates) 📄
Veb Komponentlərinin təməlində dayanan texnologiyalardan biri `<template>` teqidir.

**Template nədir?** 🤔
Təsəvvür et ki, peçenye bişirmək üçün bir qəlibin (template) var. Sən bu qəlibdən istifadə edərək yüzlərlə eyni formalı peçenye hazırlaya bilərsən. `<template>` teqi də HTML üçün belə bir "qəlibdir".

O, brauzerdə **görünməyən**, passiv bir HTML strukturu saxlayır. Bu struktur, JavaScript ilə istədiyimiz qədər **kopyalanıb (cloned)** sənədə əlavə edilə bilər. Bu, xüsusilə eyni quruluşa malik elementləri (məsələn, cədvəl sətirləri, siyahı elementləri) təkrar-təkrar yaratmaq lazım gəldikdə çox səmərəlidir, çünki brauzer şablonu yalnız bir dəfə emal (parse) edir.

**API:**
* `<template>` elementi JavaScript-də `HTMLTemplateElement` obyekti ilə təmsil olunur.
* Onun ən vacib xüsusiyyəti **`.content`**-dir.
* `template.content` şablonun içindəki bütün elementləri saxlayan bir **`DocumentFragment`**-dır.
* Bu fraqmenti kopyalamaq üçün `.cloneNode(true)` metodundan istifadə edirik (`true` bütün alt elementlərin də kopyalanmasını təmin edir).

**Geniş Nümunə: Cədvəl sətirlərini şablonla yaratmaq**
```html
<table id="user-table">
  <thead>
    <tr><th>Ad</th><th>Şəhər</th></tr>
  </thead>
  <tbody>
    </tbody>
</table>

<template id="user-row-template">
  <tr>
    <td class="name"></td>
    <td class="city"></td>
  </tr>
</template>
```
```javascript
const users = [
  { name: "Ayan", city: "Bakı" },
  { name: "Tural", city: "Gəncə" },
  { name: "Leyla", city: "Sumqayıt" }
];

const tableBody = document.querySelector("#user-table tbody");
const template = document.querySelector("#user-row-template");

for (const user of users) {
  // 1. Şablonun məzmununu dərin şəkildə kopyalayırıq
  const clone = template.content.cloneNode(true);
  
  // 2. Klonun içindəki elementləri tapıb, məzmununu doldururuq
  clone.querySelector(".name").textContent = user.name;
  clone.querySelector(".city").textContent = user.city;
  
  // 3. Hazır klonu cədvəlin `tbody`-sinə əlavə edirik
  tableBody.append(clone);
}
```

---
### 15.6.3 Fərdi Elementlər (Custom Elements) 🏗️
Bu, Veb Komponentlərin ürəyidir. Fərdi elementlər, bizə öz xüsusi HTML teqlərimizi (məsələn, `<search-box>`) yaratmağa və onların davranışını bir JavaScript sinifi (class) ilə idarə etməyə imkan verir.

**API:** `customElements.define(tagName, class)`
Bu qlobal metod, bir teq adını müvafiq bir siniflə "qeydiyyatdan" keçirir.
* **`tagName`**: Fərdi elementin adı (mütləq defisli olmalıdır).
* **`class`**: Həmin elementin davranışını təyin edən və `HTMLElement`-dən törəyən sinif.

#### Həyat Dövrü Metodları (Lifecycle Callbacks)
Brauzer, fərdi elementin həyatının müxtəlif mərhələlərində avtomatik olaraq bəzi xüsusi metodları çağırır:
* **`connectedCallback()`**: Element DOM-a əlavə edildikdə çağırılır. İlkin quraşdırma, stil vermə kimi əməliyyatlar üçün idealdır.
* **`disconnectedCallback()`**: Element DOM-dan silindikdə çağırılır. Təmizlik işləri üçün istifadə oluna bilər.
* **`attributeChangedCallback(name, oldValue, newValue)`**: İzlənilən bir atribut dəyişdikdə çağırılır.

Bir atributun dəyişikliyini izləmək üçün, sinifdə `static get observedAttributes()` metodu ilə həmin atributların adlarından ibarət bir massiv (array) qaytarmaq lazımdır.

**Geniş Nümunə: `<inline-circle>` Fərdi Elementi**
Mətnin içində rəngli dairələr göstərən sadə bir fərdi element yaradaq.
**İstifadəsi (HTML-də):**
```html
<p>
  Bu mətndə bir neçə dairə var:
  <inline-circle color="red"></inline-circle>
  <inline-circle color="blue" diameter="1.5em"></inline-circle>
  və bir də standart
  <inline-circle></inline-circle>.
</p>
```
**Təyinatı (JavaScript-də):**
```javascript
// "inline-circle" teqini InlineCircle sinifi ilə əlaqələndiririk
customElements.define("inline-circle", class InlineCircle extends HTMLElement {

  // Hansı atributların dəyişikliyini izləmək istədiyimizi bildiririk
  static get observedAttributes() {
    return ["color", "diameter"];
  }

  // Element DOM-a ilk dəfə daxil olduqda bu metod çağırılır
  connectedCallback() {
    console.log("inline-circle DOM-a əlavə edildi!");
    // İlkin stilləri veririk
    this.style.display = "inline-block";
    this.style.borderRadius = "50%";
    this.style.border = "solid black 1px";
    this.style.transform = "translateY(10%)";
    
    // Əgər ölçü verilməyibsə, standart bir ölçü təyin edirik
    if (!this.style.width) {
      this.style.width = "0.8em";
      this.style.height = "0.8em";
    }
  }

  // `observedAttributes`-dəki bir atribut dəyişdikdə bu metod çağırılır
  attributeChangedCallback(name, oldValue, newValue) {
    console.log(`Atribut dəyişdi: ${name}, yeni dəyər: ${newValue}`);
    switch(name) {
      case "diameter":
        // `diameter` atributu dəyişəndə, ölçü stillərini yeniləyirik
        this.style.width = newValue;
        this.style.height = newValue;
        break;
      case "color":
        // `color` atributu dəyişəndə, arxa fon rəngini yeniləyirik
        this.style.backgroundColor = newValue;
        break;
    }
  }
  
  // Rahatlıq üçün atributları JavaScript xüsusiyyətləri (properties) ilə əlaqələndiririk
  get diameter() { return this.getAttribute("diameter"); }
  set diameter(value) { this.setAttribute("diameter", value); }

  get color() { return this.getAttribute("color"); }
  set color(value) { this.setAttribute("color", value); }
});
```

Əla, bro! Gəldik Veb Komponentləri "əsl" komponent edən sonuncu və ən güclü texnologiyaya: **Kölgə DOM (Shadow DOM)**. Bu, komponentlərimiz üçün əsl "şəxsi dünya" yaratmağa imkan verir.

***
### 15.6.4 Kölgə (Shadow) DOM 👻
Əvvəlki nümunədəki `<inline-circle>` elementi tam qapsulyasiyalı (encapsulated) deyil. Çünki o, öz stilini dəyişmək üçün birbaşa öz `style` atributunu manipulyasiya edir. Bu, adi HTML elementlərinin davranışı deyil. Komponentimizi əsl "qara qutuya" çevirmək üçün **Shadow DOM** adlı bu güclü mexanizmdən istifadə etməliyik.

**Shadow DOM nədir?** 🤔
Təsəvvür et ki, bir Veb Komponent bir evdir. **Shadow DOM** isə o evin **daxili dünyasıdır**. Küçədən baxanlar (əsas sənəd) evin içindəki mebelin yerini və ya divarların rəngini görə bilməz və dəyişə bilməz. Evin sahibi (komponentin özü) isə öz daxili dünyasını tamamilə idarə edir.

Texniki olaraq, Shadow DOM, bir elementə (buna **kölgə sahibi - shadow host** deyilir) bağlanmış, ondan "cMərtəbəən" ikinci, daha özəl bir DOM ağacıdır. Daxili `<video>` və ya `<audio>` elementlərinin play/pause düymələri məhz bu texnologiya ilə yaradılıb – onlar sənədin bir hissəsidir, amma biz onları birbaşa DOM-da görə və manipulyasiya edə bilmirik.

#### Kölgə DOM-un "Mühafizəsi" (Encapsulation) 🛡️
Shadow DOM-un əsas xüsusiyyəti onun təmin etdiyi **qapsulyasiyadır (encapsulation)**. Bu, 3 istiqamətdə özünü göstərir:

1.  **DOM Qapsulyasiyası:** Kölgə ağacının (shadow tree) içindəki elementlər əsas sənəddən tamamilə gizlədilir. Onları `document.querySelector()` ilə tapmaq olmur və onlar host elementin `.children` siyahısında görünmür.
2.  **CSS Qapsulyasiyası:** Bu, bəlkə də ən vacib xüsusiyyətdir. Kölgə ağacının daxilində yazdığınız CSS stilləri **kənarı təsir etmir** və eyni zamanda, kənardakı (əsas sənəddəki) CSS stilləri də kölgə ağacının **daxilinə təsir etmir**. Bu, CSS-də adların toqquşması problemini tamamilə aradan qaldırır!
3.  **Hadisə (Event) Qapsulyasiyası:** Kölgə ağacının içindən başlayıb yuxarı "qabarcıqlanan" (bubble) hadisələrin hədəfi (`event.target`) avtomatik olaraq host elementin özü ilə əvəz olunur. Bu, komponentin daxili strukturunu kənardan gizlədir.

#### İşıqlı (Light) DOM və Kölgə (Shadow) DOM-un Birgə İstifadəsi: `<slot>` 🧩
Bəs əgər bir elementin həm öz daxili kölgə DOM-u, həm də istifadəçi tərəfindən verilən övladları (`<search-box>` nümunəsindəki `<img>`-lər kimi) varsa, onlar necə göstərilir?

Cavab **`<slot>`** teqidir.
* Kölgə DOM-un içində yerləşdirilən `<slot>` teqi, host elementin "işıqlı" (light DOM) övladlarının harada göstəriləcəyini bildirən bir "yer tutucudur".
* Əgər kölgə DOM-da heç bir `<slot>` yoxdursa, host elementin övladları heç vaxt göstərilmir.
* Slotlara `name` atributu verərək, onları adlandırmaq olar. Bu zaman istifadəçi, öz elementinə `slot="ad"` atributu verərək onu müvafiq slota yerləşdirə bilər.

**Geniş Nümunə: Fərdi Kart Komponenti**
Gəlin həm başlığı, həm də məzmunu üçün slotları olan bir `<user-card>` komponenti yaradaq.
**1. Komponentin JavaScript tərifi (onun kölgə DOM-u):**
```javascript
class UserCard extends HTMLElement {
  constructor() {
    super();
    // Komponentə bir kölgə ağacı bağlayırıq
    const shadow = this.attachShadow({ mode: 'open' });
    
    // Kölgə ağacının daxili HTML-ini və stillərini təyin edirik
    shadow.innerHTML = `
      <style>
        .card { border: 1px solid #ccc; border-radius: 8px; padding: 16px; }
        header { font-weight: bold; border-bottom: 1px solid #ccc; padding-bottom: 8px; }
      </style>
      <div class="card">
        <header>
          <slot name="card-header">Standart Başlıq</slot>
        </header>
        <div class="card-body">
          <slot>Standart Məzmun</slot>
        </div>
      </div>
    `;
  }
}
customElements.define('user-card', UserCard);
```

**2. Komponentin HTML-də istifadəsi:**
```html
<user-card>
  <h1 slot="card-header">Ayan Məmmədova</h1>

  <p>Software Engineer</p>
  <a href="#">Profili göstər</a>
</user-card>
```
Bu iki kod birləşdikdə, ekranda səliqəli, tam qapsulyasiyalı bir kart komponenti yaranır.

#### Shadow DOM API-ı
Bu qədər güclü bir texnologiya olmasına baxmayaraq, onun API-ı çox sadədir. Əsas metod **`element.attachShadow({ mode: 'open' })`**-dur.
* Bu metod bir elementə kölgə ağacı bağlayır və həmin ağacı (bir `DocumentFragment` olaraq) qaytarır.
* `mode: 'open'` o deməkdir ki, JavaScript ilə `element.shadowRoot` vasitəsilə kölgə ağacına müraciət etmək olar. `mode: 'closed'` isə bunu qadağan edir və çox nadir hallarda istifadə olunur.

Və budur, bro, fəslin sonu! İndi öyrəndiyimiz hər üç texnologiyanı – **HTML Şablonları**, **Fərdi Elementlər** və **Kölgə DOM**-u birləşdirərək, tam işlək, qapsulyasiyalı və təkrar istifadə edilə bilən bir `<search-box>` komponenti yaradacağıq. Bu, bütün hissələrin bir-birinə necə bağlandığını göstərən əla bir yekun nümunədir.

***
### 15.6.5 Geniş Nümunə: `<search-box>` Veb Komponenti 🏁

Bu nümunə, şəkildə gördüyümüz axtarış qutusunu tamamilə sıfırdan yaradır.

#### Addım 1: Komponentin Quruluşu və Stili (`<template>`)
İlk növbədə, komponentimizin daxili HTML quruluşunu və CSS stillərini bir `<template>` içində təyin edirik. Bu, hər bir `<search-box>` nüsxəsi üçün təkrar-təkrar istifadə edəcəyimiz "qəlibdir".

```javascript
// Bu kodu sinifimizi (class) təyin etməzdən əvvəl və ya sonra yaza bilərik.
// Adətən sinifin statik bir xüsusiyyəti kimi təyin olunur.

// 1. Boş bir template elementi yaradırıq
SearchBox.template = document.createElement("template");

// 2. Template-in daxili HTML-ini və CSS-ini təyin edirik
SearchBox.template.innerHTML = `
  <style>
    /* :host seçicisi, <search-box> elementinin özünə stil verir */
    :host {
      display: inline-block; /* Komponentin özü sətirdə bir blok kimi görünsün */
      border: 1px solid #ccc;
      border-radius: 5px;
      padding: 4px 8px;
    }
    :host([hidden]) { display: none; } /* Əgər hidden atributu varsa, gizlət */
    :host([disabled]) { opacity: 0.5; } /* Əgər disabled atributu varsa, solğunlaşdır */
    :host([focused]) { 
      /* "focused" atributu əlavə etdikdə, saxta fokus halqası göstər */
      box-shadow: 0 0 2px 2px #6AE;
    }

    /* Daxili elementlər üçün stillər (bunlar kənara təsir etmir!) */
    input {
      border: none;
      outline: none; /* Fokus halqasını ləğv et */
      padding: 0;
      background: transparent;
      font: inherit; /* Valideynin fontunu götürsün */
    }
    slot {
      cursor: default; /* İkonların üzərində standart kursor olsun */
      user-select: none; /* İkonların mətnini seçmək olmasın */
    }
  </style>
  
  <div> <slot name="left">\u{1f50d}</slot> <input type="text" id="input" />     <slot name="right">\u{2573}</slot>  </div>
`;
```

#### Addım 2: Komponentin Məntiqi (`SearchBox` sinifi)
İndi isə komponentimizin bütün davranışını idarə edən JavaScript sinifini (class) yaradırıq.

```javascript
class SearchBox extends HTMLElement {
  constructor() {
    super(); // Mütləq ilk olaraq çağırılmalıdır!

    // 1. Kölgə DOM (Shadow DOM) yaradırıq və onu elementə bağlayırıq
    this.attachShadow({ mode: "open" });

    // 2. Yuxarıda yaratdığımız şablonu kopyalayıb, kölgənin içinə əlavə edirik
    this.shadowRoot.append(SearchBox.template.content.cloneNode(true));
    
    // 3. Kölgənin içindəki vacib elementlərə rahat çatmaq üçün istinadlar (references) yaradırıq
    this.input = this.shadowRoot.querySelector("#input");
    const leftSlot = this.shadowRoot.querySelector('slot[name="left"]');
    const rightSlot = this.shadowRoot.querySelector('slot[name="right"]');

    // 4. Hadisə dinləyicilərini (event listeners) təyin edirik
    // Daxili input fokus alıb-itirdikdə, host elementə "focused" atributunu əlavə edib-silirik
    this.input.onfocus = () => { this.setAttribute("focused", ""); };
    this.input.onblur = () => { this.removeAttribute("focused"); };
    
    // Sol ikona (böyüdücü şüşə) kliklədikdə və ya inputda "change" hadisəsi baş verdikdə
    // fərdi "search" hadisəsini göndəririk.
    leftSlot.onclick = this.input.onchange = (event) => {
      event.stopPropagation(); // Klikin yuxarı qabarcıqlanmasının qarşısını alırıq
      if (this.disabled) return;
      this.dispatchEvent(new CustomEvent("search", {
        detail: this.input.value
      }));
    };
    
    // Sağ ikona (X) kliklədikdə, fərdi "clear" hadisəsi göndəririk
    rightSlot.onclick = (event) => {
      event.stopPropagation();
      if (this.disabled) return;
      const e = new CustomEvent("clear", { cancelable: true });
      this.dispatchEvent(e);
      // Əgər kimsə bu hadisənin standart davranışını ləğv etməyibsə (`preventDefault`), inputu təmizləyirik.
      if (!e.defaultPrevented) {
        this.input.value = "";
      }
    };
  }
  
  // İzlədiyimiz atributlar dəyişdikdə bu metod avtomatik çağırılır
  attributeChangedCallback(name, oldValue, newValue) {
    // Host elementin atributlarını daxili inputun xüsusiyyətləri ilə sinxronlaşdırırıq
    if (name === "disabled") {
      this.input.disabled = newValue !== null;
    } else if (name === "placeholder") {
      this.input.placeholder = newValue;
    } else if (name === "size") {
      this.input.size = newValue;
    } else if (name === "value") {
      this.input.value = newValue;
    }
  }

  // Atributları rahat istifadə üçün JavaScript xüsusiyyətləri (properties) kimi təyin edirik
  get value() { return this.input.value; }
  set value(v) { this.input.value = v; this.setAttribute("value", v); }
  
  get placeholder() { return this.getAttribute("placeholder"); }
  set placeholder(v) { this.setAttribute("placeholder", v); }
  
  get disabled() { return this.hasAttribute("disabled"); }
  set disabled(v) { if (v) this.setAttribute("disabled", ""); else this.removeAttribute("disabled"); }
  
  // Bu statik xüsusiyyət, hansı atributların dəyişikliyini izləyəcəyimizi bildirir
  static get observedAttributes() {
    return ["disabled", "placeholder", "size", "value"];
  }
}

// 3. Nəhayət, fərdi elementimizi brauzerdə qeydiyyatdan keçiririk
customElements.define("search-box", SearchBox);
```
#### Addım 3: İstifadəsi (Usage)
Artıq `<search-box>` tam hazırdır və onu HTML-də istifadə edib, fərdi hadisələrini dinləyə bilərik.
```html
<h3>Komponentimizi Test Edək</h3>
<search-box placeholder="Axtarış üçün yazın..."></search-box>

<script>
  const searchBox = document.querySelector('search-box');
  
  // Fərdi "search" hadisəsini dinləyirik
  searchBox.addEventListener('search', e => {
    console.log(`Axtarış başladı! Dəyər: "${e.detail}"`);
  });
  
  // Fərdi "clear" hadisəsini dinləyirik
  searchBox.addEventListener('clear', () => {
    console.log("Axtarış qutusu təmizləndi!");
  });
</script>
```
Bununla da Veb Komponentlər mövzusunu tamamilə bitirdik, bro! Artıq öz təkrar istifadə edilə bilən, qapsulyasiyalı HTML elementlərini yaratmağın bütün sirlərini bilirsən!

Əla, bro! İndi isə veb səhifələrdə təkcə mətn və qutularla deyil, həm də qrafika ilə işləməyə başlayırıq. Mövzumuz: **SVG** – brauzerdə qrafika çəkməyin ən güclü yollarından biri.

***
### 15.7 SVG: Miqyaslana Bilən Vektor Qrafikası (Scalable Vector Graphics) 🎨

**SVG (Scalable Vector Graphics)** bir şəkil formatıdır. Amma o, bildiyimiz PNG və ya JPEG kimi deyil.

**Fərq nədədir?** 🤔
Təsəvvür et ki, PNG və JPEG şəkilləri mozaika kimi kiçik rəngli daşlardan (piksellərdən) yığılır. Onu böyütdükdə keyfiyyət itir, piksellər görünməyə başlayır. **SVG** isə bir rəsmin çəkilməsi üçün verilən **həndəsi təlimatdır**: "bir dairə çək, sonra qırmızı rəngdə bir xətt çək...". Bu təlimat istənilən ölçüdə eyni keyfiyyətlə icra oluna bildiyi üçün, ona **"miqyaslana bilən" (scalable)** deyilir.

SVG şəkilləri, HTML-ə çox bənzəyən XML adlı bir dildə təsvir olunur. Brauzerlərdə SVG-dən 3 fərqli yolla istifadə etmək olar:
1.  Adi `<img>` teqi ilə (`<img src="my-image.svg">`).
2.  SVG teqlərini birbaşa HTML sənədinin içinə yazmaqla.
3.  JavaScript və DOM API-ı ilə dinamik olaraq SVG elementləri yaratmaqla.

Bu bölmədə biz ikinci və üçüncü yollara, yəni SVG-ni HTML-ə daxil edib onu JavaScript ilə idarə etməyə baxacağıq.

---
### 15.7.1 SVG-nin HTML-də İstifadəsi
SVG teqlərini birbaşa HTML faylının içinə yazdığınız zaman, onlara eynilə HTML elementləri kimi CSS ilə stil verə bilərsiniz.

**Geniş Nümunə: Statik Analoq Saatın Siferblatı**
Aşağıdakı kod, HTML və SVG teqlərindən istifadə edərək statik bir saat siferblatı yaradır. Saatın əqrəbləri hələlik 12:00-ı göstərir.

```html
<html>
<head>
<title>Analoq Saat</title>
<style>
  /* Bu CSS stilləri aşağıdakı SVG elementlərinə tətbiq olunur */
  #clock {
    stroke: black;         /* Bütün xətlərin rəngi */
    stroke-linecap: round; /* Xətlərin ucları yumru olsun */
    fill: #ffe;            /* Daxili rəng (ağ-sarı) */
  }
  #clock .face { stroke-width: 3; }    /* Siferblatın xətt qalınlığı */
  #clock .ticks { stroke-width: 2; }   /* Saat bölgülərinin xətt qalınlığı */
  #clock .hands { stroke-width: 3; }   /* Əqrəblərin xətt qalınlığı */
  #clock .numbers {                    /* Rəqəmlərin stili */
    font-family: sans-serif;
    font-size: 10;
    font-weight: bold;
    text-anchor: middle; /* Mətn mərkəzə görə hizalansın */
    stroke: none;        /* Rəqəmlərin kənar xətti olmasın */
    fill: black;         /* Rəqəmlərin rəngi */
  }
</style>
</head>
<body>
  <svg id="clock" viewBox="0 0 100 100" width="250" height="250">
    
    <circle class="face" cx="50" cy="50" r="45"/>
    
    <g class="ticks">
      <line x1='50' y1='5' x2='50' y2='10' />
      <line x1='72.5' y1='11.03' x2='70' y2='15.36' />
      </g>
    
    <g class="numbers">
      <text x="50" y="18">12</text> <text x="85" y="53">3</text>
      <text x="50" y="88">6</text> <text x="15" y="53">9</text>
    </g>
    
    <g class="hands">
      <line class="hourhand" x1="50" y1="50" x2="50" y2="25"/>
      <line class="minutehand" x1="50" y1="50" x2="50" y2="20"/>
    </g>
  </svg>

  <script src="clock.js"></script>
</body>
</html>
```
Gördüyünüz kimi, `<svg>` teqinin içində `<circle>`, `<line>`, `<text>` kimi HTML-ə bənzər, amma həndəsi fiqurlar çəkmək üçün olan xüsusi teqlər var. CSS-də isə `stroke` (xətt rəngi) və `fill` (iç rəngi) kimi SVG-yə məxsus xüsusiyyətlər istifadə olunur.

---
### 15.7.2 SVG-nin Skriptləşdirilməsi (Scripting SVG) ⏰
SVG-ni birbaşa HTML-ə daxil etməyin əsas üstünlüyü, onu JavaScript ilə manipulyasiya edə bilməkdir. `<svg>` və onun içindəki bütün elementlər DOM-un bir hissəsidir və biz onları `querySelector` ilə seçib dəyişə bilərik.

**Geniş Nümunə: Analoq Saatı Canlandırmaq**
İndi isə yuxarıdakı statik saatı götürüb, ona qoşduğumuz `clock.js` faylının içindəki kodla real vaxtı göstərən bir saata çevirək.

```javascript
// Bu kod bir IIFE (Immediately Invoked Function Expression) içindədir,
// yəni özü-özünü dərhal çağırır və işə düşür.
(function updateClock() {
  const now = new Date(); // Hazırkı vaxtı götürürük
  const sec = now.getSeconds();
  const min = now.getMinutes() + sec / 60; // Dəqiqəni kəsrli alırıq ki, əqrəb səlis hərəkət etsin
  const hour = (now.getHours() % 12) + min / 60; // Saatı 12 saatlıq formatda və kəsrli alırıq
  
  // Dəqiqə və saat üçün çevrilmə bucaqlarını hesablayırıq
  // Dəqiqə əqrəbi 60 dəqiqəyə 360 dərəcə fırlanır (hər dəqiqə 6 dərəcə)
  const minangle = min * 6; 
  // Saat əqrəbi 12 saata 360 dərəcə fırlanır (hər saat 30 dərəcə)
  const hourangle = hour * 30; 
  
  // DOM-dan saat və dəqiqə əqrəblərini (yəni <line> elementlərini) seçirik
  const minhand = document.querySelector("#clock .minutehand");
  const hourhand = document.querySelector("#clock .hourhand");
  
  // `setAttribute` ilə onların "transform" atributunu dəyişərək onları fırladırıq
  // `rotate(bucaq, mərkəz_x, mərkəz_y)`
  minhand.setAttribute("transform", `rotate(${minangle}, 50, 50)`);
  hourhand.setAttribute("transform", `rotate(${hourangle}, 50, 50)`);
  
  // Bu funksiyanı təxminən 10 saniyə sonra yenidən çağırmaq üçün zamanlayıcı qururuq
  // (Daha dəqiq olması üçün hər saniyə də çağırmaq olar)
  setTimeout(updateClock, 10000); 
}()); 
```
Beləliklə, statik bir SVG təsvirini götürüb, JavaScript ilə onu tam işlək, dinamik bir analoq saata çevirdik! Bu, SVG və DOM API-larının birlikdə nə qədər güclü olduğunu göstərən əla bir nümunədir.

Olar, bro! SVG mövzusunun son hissəsinə keçirik. Bu dəfə təkcə mövcud SVG-ni dəyişməyəcəyik, sıfırdan, tamamilə JavaScript kodu ilə dinamik bir qrafik – pay diaqramı (pie chart) yaradacağıq!

***
### 15.7.3 JavaScript ilə SVG Şəkilləri Yaratmaq 📊
SVG-ni birbaşa HTML-ə daxil etməklə yanaşı, biz onu tamamilə JavaScript ilə də yarada bilərik. Bu, xüsusilə serverdən gələn dinamik datanı vizuallaşdırmaq lazım gəldikdə çox faydalıdır. Məsələn, bir sorğunun nəticələrinə əsasən bir diaqram qurmaq kimi.

#### ❗️ Vacib Fərq: `createElementNS()`
SVG teqləri (`<svg>`, `<circle>`, `<path>`) texniki olaraq HTML deyil, başqa bir XML ləhcəsi olduğu üçün, onları adi `document.createElement()` metodu ilə yaratmaq olmaz.

Bunun əvəzinə, biz **`document.createElementNS(namespace, tagName)`** metodundan istifadə etməliyik.
* **`namespace`**: SVG elementləri üçün bu dəyər həmişə `"http://www.w3.org/2000/svg"` sətiridir.
* **`tagName`**: Yaratmaq istədiyimiz teqin adı (məsələn, `"svg"`, `"path"`).

---
#### Geniş Nümunə: Pay Diaqramı (Pie Chart) Yaratmaq
Aşağıdakı `pieChart` funksiyası, ona verilən dataya əsasən tam bir SVG pay diaqramı yaradır və nəticə olaraq bir `<svg>` elementi qaytarır. Kodun içindəki şərhlər hər addımın nə etdiyini detallı şəkildə izah edir.

```javascript
/**
 * Verilmiş dataya əsasən bir SVG pay diaqramı (pie chart) yaradır və
 * həmin <svg> elementini qaytarır.
 * @param {object} options - Diaqramın parametrləri:
 * width, height: SVG qrafikinin ölçüsü (piksel ilə).
 * cx, cy, r: Diaqram dairəsinin mərkəzi və radiusu.
 * lx, ly: Leqandanın (izahlı siyahı) yuxarı-sol küncü.
 * data: { "ad1": dəyər1, "ad2": dəyər2, ... } formatında olan data.
 */
function pieChart(options) {
  const { width, height, cx, cy, r, lx, ly, data } = options;

  // SVG elementləri üçün XML namespace
  const svgNS = "http://www.w3.org/2000/svg";

  // 1. Əsas <svg> elementini yaradırıq
  const chart = document.createElementNS(svgNS, "svg");
  chart.setAttribute("width", width);
  chart.setAttribute("height", height);
  chart.setAttribute("viewBox", `0 0 ${width} ${height}`);
  chart.setAttribute("font-family", "sans-serif");
  chart.setAttribute("font-size", "14");

  // 2. Datanı emal edirik
  const labels = Object.keys(data);
  const values = Object.values(data);
  const total = values.reduce((sum, value) => sum + value, 0);

  // Hər dilimin bucağını hesablamaq üçün
  let angles = [0];
  for(let i = 0; i < values.length; i++) {
    angles.push(angles[i] + (values[i] / total) * 2 * Math.PI);
  }

  // 3. Hər bir data nöqtəsi üçün bir diaqram dilimi və leqanda yaradırıq
  values.forEach((value, i) => {
    // ---- Diaqram Dilimini Çəkirik ----
    
    // Dilimin başlanğıc və son nöqtələrinin koordinatlarını hesablayırıq
    const x1 = cx + r * Math.sin(angles[i]);
    const y1 = cy - r * Math.cos(angles[i]);
    const x2 = cx + r * Math.sin(angles[i+1]);
    const y2 = cy - r * Math.cos(angles[i+1]);

    // Əgər dilim yarım-dairədən böyükdürsə, SVG qövsü üçün xüsusi bir fleq təyin edirik
    const bigArcFlag = (angles[i+1] - angles[i] > Math.PI) ? 1 : 0;

    // SVG <path> elementi üçün "təlimat" sətrini yaradırıq.
    // Bu, "rəssama" nəyi harada çəkəcəyini deyir:
    // M cx,cy -> Mərkəzə get (Move to)
    // L x1,y1 -> Birinci nöqtəyə xətt çək (Line to)
    // A r,r ... x2,y2 -> Oradan ikinci nöqtəyə qövs çək (Arc)
    // Z -> Yolu bağla (mərkəzə qayıt)
    const pathData = `M${cx},${cy} L${x1},${y1} A${r},${r} 0 ${bigArcFlag} 1 ${x2},${y2} Z`;
    
    // Hər dilim üçün fərqli bir rəng yaradırıq
    const sliceColor = `hsl(${(i * 45) % 360}, 70%, 50%)`;

    // <path> elementini yaradırıq
    const slice = document.createElementNS(svgNS, "path");
    slice.setAttribute("d", pathData);
    slice.setAttribute("fill", sliceColor);
    slice.setAttribute("stroke", "white");
    slice.setAttribute("stroke-width", "2");
    chart.append(slice); // Dilimi diaqrama əlavə edirik

    // ---- Leqandanı (İzahlı Siyahı) Çəkirik ----
    
    // Leqanda üçün rəngli kvadrat yaradırıq
    const icon = document.createElementNS(svgNS, "rect");
    icon.setAttribute("x", lx);
    icon.setAttribute("y", ly + 30 * i);
    icon.setAttribute("width", 20);
    icon.setAttribute("height", 20);
    icon.setAttribute("fill", sliceColor);
    chart.append(icon);

    // Leqanda üçün mətni yaradırıq
    const label = document.createElementNS(svgNS, "text");
    label.setAttribute("x", lx + 30);
    label.setAttribute("y", ly + 30 * i + 16);
    // Mətnin faizini də hesablayıb əlavə edirik
    const percentage = ((value / total) * 100).toFixed(1);
    label.textContent = `${labels[i]}: ${percentage}%`;
    chart.append(label);
  });

  return chart; // Hazır <svg> elementini qaytarırıq
}
```
#### Funksiyanın İstifadəsi
İndi isə bu funksiyanı çağırıb, onun qaytardığı SVG elementini səhifəmizə əlavə edək.
```html
<div id="chart-container" style="text-align: center;">
  <h2>Ən Populyar Proqramlaşdırma Texnologiyaları (2018)</h2>
</div>
```
```javascript
const chartContainer = document.querySelector("#chart-container");

const data = {
  "JavaScript": 71.5, "Java": 45.4, "Python": 37.9, 
  "C#": 35.3, "PHP": 31.4, "C++": 24.6
};

const chart = pieChart({
  width: 600, height: 400, // SVG-nin ümumi ölçüsü
  cx: 200, cy: 200, r: 180,  // Dairənin mərkəzi və radiusu
  lx: 420, ly: 50,           // Leqandanın başlanğıc mövqeyi
  data: data                 // İstifadə olunacaq data
});

// Yaratdığımız diaqramı konteynerin içinə əlavə edirik
chartContainer.append(chart);
```
Bu nümunə, DOM API-nın nə qədər güclü olduğunu və JavaScript ilə təkcə sənəd məzmununu deyil, həm də mürəkkəb, dinamik vizualizasiyalar yaratmağın mümkün olduğunu göstərir.

Əla, bro! İndi isə brauzerdə qrafika yaratmağın başqa bir güclü yolu ilə tanış oluruq: **`<canvas>` elementi**. SVG-dən fərqli bir yanaşma təqdim edir və xüsusilə dinamik animasiyalar və oyunlar üçün çox faydalıdır.

***
### 15.8 `<canvas>` ilə Qrafika 🖌️
`<canvas>` elementi özü-özlüyündə görünüşə malik olmayan, sadəcə səhifədə bir "rəsm kətanı" yaradan bir HTML teqidir. Bütün sehr isə, JavaScript ilə bu kətan üzərində rəsm çəkməyə imkan verən **Drawing API**-da baş verir.

#### SVG vs. Canvas: Əsas Fərq Nədir?
Təsəvvür et ki, rəsm çəkməyin iki yolu var:

1.  **SVG (Konstruktor  Lego 🧱):** Sən rəssama təlimat verirsən: "bir qırmızı dairə, bir yaşıl kvadrat çək". Rəssam bu fiqurları ayrı-ayrı obyektlər kimi yaradır və onları lövhəyə qoyur. Sən istənilən an "dairəni mavi rəngə boya" və ya "kvadratı başqa yerə çək" deyə bilərsən. Obyektlər özlüyündə "sağdır" və manipulyasiya edilə bilər. Bu, **deklarativ (retained mode)** yanaşmadır.

2.  **Canvas (Rəssam və Kətan 🎨):** Sən rəssama bir kətan (canvas) verirsən və əmrlər verirsən: "fırçanı qırmızıya batır, filan koordinatda dairəvi hərəkət et". Rəng kətana çəkildikdən sonra, o, artıq sadəcə bir **piksel toplusudur**. Onu dəyişmək üçün, ya üstündən başqa rəng çəkməli, ya da bütün kətanı silib şəkli yenidən çəkməlisən. Obyektlər yaddaşda qalmır. Bu, **prosedural (immediate mode)** yanaşmadır.

#### Rəsm Konteksti (Drawing Context)
`<canvas>` ilə rəsm çəkmək üçün, əvvəlcə onun **"rəsm kontekstini" (drawing context)** əldə etməliyik. Bunu `.getContext("2d")` metodu ilə edirik. Bu metod, 2D qrafika çəkmək üçün lazım olan bütün metodları və xüsusiyyətləri saxlayan bir obyekt qaytarır.

**Sadə Nümunə: Kvadrat və Dairə**
```html
<p>Bu bir qırmızı kvadratdır: <canvas id="square" width="12" height="12"></canvas>.</p>
<p>Bu isə bir göy dairədir: <canvas id="circle" width="12" height="12"></canvas>.</p>

<script>
  // Kvadratı çəkirik
  let squareCanvas = document.querySelector("#square");
  let sqContext = squareCanvas.getContext("2d");
  sqContext.fillStyle = "#f00"; // İç rəngini qırmızı təyin edirik
  sqContext.fillRect(0, 0, 12, 12); // Verilmiş koordinatda içi dolu düzbucaqlı çəkirik
  
  // Dairəni çəkirik
  let circleCanvas = document.querySelector("#circle");
  let cContext = circleCanvas.getContext("2d");
  cContext.beginPath(); // Yeni bir "yol" (path) başladırıq
  // Dairə (qövs) əlavə edirik: mərkəz(6,6), radius(6), başlanğıc və son bucaq
  cContext.arc(6, 6, 6, 0, 2 * Math.PI); 
  cContext.fillStyle = "#00f"; // İç rəngini göy təyin edirik
  cContext.fill(); // Yolu (path) təyin olunmuş rənglə doldururuq
</script>
```

---
### 15.8.1 Yollar (Paths) və Çoxbucaqlılar (Polygons) 📐
Mürəkkəb fiqurlar (xətlər, üçbucaqlar, çoxbucaqlılar) çəkmək üçün, əvvəlcə virtual "fırçamızın" gedəcəyi bir **yol (path)** təyin etməliyik. Bu, bir neçə metodun ardıcıl çağırılması ilə edilir.

**Əsas Path Metodları:**
* **`beginPath()`**: Yeni bir yol başlayır, köhnə yolu təmizləyir. **Bu çox vacibdir!** Hər yeni fiqur üçün bunu çağırmaq lazımdır.
* **`moveTo(x, y)`**: Fırçanı rəsm çəkmədən verilmiş `(x, y)` koordinatına aparır. Adətən yolun başlanğıc nöqtəsini təyin edir.
* **`lineTo(x, y)`**: Fırçanın hazırkı mövqeyindən verilmiş `(x, y)` koordinatına bir xətt çəkir.
* **`closePath()`**: Yolun hazırkı nöqtəsini onun başlanğıc nöqtəsi ilə birləşdirən bir xətt çəkir.
* **`stroke()`**: Təyin olunmuş yolu (path) xətlərlə çəkir (konturlarını).
* **`fill()`**: Təyin olunmuş yolun daxilini rənglə doldurur.

**Geniş Nümunə: Çoxbucaqlılar çəkmək**
Aşağıdakı `polygon` funksiyası, verilmiş mərkəz və radiusa görə n-bucaqlı bir fiqurun yolunu (path) yaradır.
```javascript
/**
 * Verilmiş kontekstdə n-tərəfli bir çoxbucaqlı çəkir.
 * c: 2D context, n: tərəf sayı, x,y: mərkəz, r: radius
 */
function polygon(c, n, x, y, r) {
  const angle = (2 * Math.PI) / n; // Təpə nöqtələri arasındakı bucaq
  c.moveTo(x + r, y); // İlk təpə nöqtəsinə keçirik

  for (let i = 1; i < n; i++) {
    const currentAngle = i * angle;
    // Növbəti təpə nöqtəsinə xətt çəkirik
    c.lineTo(x + r * Math.cos(currentAngle), y + r * Math.sin(currentAngle));
  }
  
  c.closePath(); // Yolu bağlayaraq fiquru tamamlayırıq
}

// Tutaq ki, HTML-də <canvas id="main-canvas" width="500" height="150"></canvas> var
const canvas = document.querySelector("#main-canvas");
const ctx = canvas.getContext("2d");

// 1. Yeni bir yol başladırıq (bütün fiqurlar bu yola daxil olacaq)
ctx.beginPath();

// 2. Müxtəlif çoxbucaqlıların yollarını yaradırıq
polygon(ctx, 3, 75, 75, 50);   // Üçbucaq
polygon(ctx, 4, 200, 75, 50);  // Kvadrat
polygon(ctx, 5, 325, 75, 50);  // Beşbucaqlı
polygon(ctx, 6, 450, 75, 50);  // Altıbucaqlı

// 3. Fiqurların necə görünəcəyini təyin edirik
ctx.fillStyle = "#ccc";   // Daxili rəng - boz
ctx.strokeStyle = "#008"; // Kənar xətt rəngi - tünd göy
ctx.lineWidth = 5;        // Xətt qalınlığı

// 4. İndi isə bütün təyin olunmuş yolları çəkirik
ctx.fill();   // Bütün fiqurların içini doldurur
ctx.stroke(); // Bütün fiqurların kənar xətlərini çəkir
```
`fill()` metodu açıq yolları avtomatik bağlayaraq doldurur, ancaq `stroke()` yalnız təyin olunmuş xətləri çəkir. Tam bir kontur üçün `closePath()` istifadə etmək vacibdir.

Əla, bro! Kətan (canvas) üzərində rəsm çəkməyin daha dərin detallarına baxırıq. Bu hissədə kətanın ölçülərini, koordinat sistemini və rəsm çəkərkən istifadə etdiyimiz "fırça", "rəng" kimi atributları necə idarə edəcəyimizi öyrənəcəyik.

***
### 15.8.2 Kətanın (Canvas) Ölçüləri və Koordinatları 📏
`<canvas>` elementinin `width` və `height` atributları onun üzərində rəsm çəkilə bilən səthin, yəni **buferin (buffer)** ölçüsünü piksel ilə müəyyən edir. Koordinat sistemi standartdır: **`(0,0)` yuxarı-sol küncdür**, `x` sağa, `y` isə aşağıya doğru artır.

❗️ **Vacib Qeyd:** `canvas.width` və ya `canvas.height` xüsusiyyətlərini JavaScript ilə dəyişmək, hətta mövcud dəyərinə yenidən mənimsətmək belə, **kətanı tamamilə təmizləyir** və bütün qrafik vəziyyətini (rənglər, stillər, transformasıyalar) sıfırlayır.

#### Yüksək Keyfiyyətli (High-DPI / Retina) Ekranlar üçün Optimallaşdırma
Yüksək piskel sıxlığı olan ekranlarda (məsələn, müasir telefonlar, MacBook-lar) `<canvas width="300" height="150">` təyin etsəniz, şəkliniz bir az bulanıq görünə bilər. Çünki brauzer 300x150 piksellik bir kətanı, əslində 600x300 fiziki pikseli əhatə edən bir sahəyə "dartır".

Bunun qarşısını almaq və kəskin (crisp) qrafiklər əldə etmək üçün **ən yaxşı yol** budur:
1.  Kətanın ekranda görünəcək ölçüsünü **CSS ilə** verin.
2.  JavaScript-də isə, onun real `width` və `height`-ini ekranın piksel nisbətinə (`devicePixelRatio`) vurun.

**Geniş Nümunə:**
```html
<canvas id="myCanvas" style="width: 300px; height: 150px; border: 1px solid black;"></canvas>
```
```javascript
const canvas = document.querySelector('#myCanvas');
const ctx = canvas.getContext('2d');

// Cihazın piksel nisbətini götürürük (adətən 1 və ya 2 olur)
const dpr = window.devicePixelRatio || 1;

// CSS-dəki ölçüləri alırıq
const rect = canvas.getBoundingClientRect();

// Kətanın bufer ölçüsünü fiziki piksellərə uyğunlaşdırırıq
canvas.width = rect.width * dpr;
canvas.height = rect.height * dpr;

// Bütün rəsm əməliyyatlarını bu nisbətə görə miqyaslayırıq (böyüdürük)
ctx.scale(dpr, dpr);

// İndi çəkəcəyimiz hər şey kəskin görünəcək!
ctx.font = "24px sans-serif";
ctx.fillText("Salam, Dünya!", 10, 50);
```

---
### 15.8.3 Qrafik Atributları (Graphics Attributes) 🎨
`fillRect()` və ya `stroke()` kimi metodları çağırmazdan əvvəl, kontekst (`ctx`) obyektinin müxtəlif xüsusiyyətlərini təyin edərək rəsmin necə görünəcəyini idarə edirik.

**Analogy:** Kontekst (`ctx`) bir rəssamdır və bu atributlar onun palitrası, fırçalarının qalınlığı və növü kimidir. Rəsm çəkməzdən əvvəl bu alətləri sazlayırsan.

#### Xətt Stilləri (Line Styles) 〰️
* **`lineWidth`**: Xəttin qalınlığı (piksel ilə). Standart: `1`.
* **`lineCap`**: Açıq xətlərin uclarının forması: `"butt"` (düz), `"round"` (yumru), `"square"` (kvadrat).
* **`lineJoin`**: İki xəttin birləşdiyi yerdəki küncün forması: `"miter"` (iti), `"round"` (yumru), `"bevel"` (kəsik).
* **`setLineDash([dash, space, ...])`**: Qırıq-qırıq xətlər çəkmək üçün bir metod. Massivdəki rəqəmlər ardıcıl olaraq "çək" və "boşluq burax" əmrlərini verir.

**Nümunə:**
```javascript
ctx.lineWidth = 10;

// lineCap nümunəsi
ctx.beginPath();
ctx.lineCap = "butt"; ctx.moveTo(20, 20); ctx.lineTo(100, 20); ctx.stroke();
ctx.beginPath();
ctx.lineCap = "round"; ctx.moveTo(20, 40); ctx.lineTo(100, 40); ctx.stroke();
ctx.beginPath();
ctx.lineCap = "square"; ctx.moveTo(20, 60); ctx.lineTo(100, 60); ctx.stroke();

// setLineDash nümunəsi
ctx.beginPath();
ctx.setLineDash([15, 5, 2, 5]); // 15px xətt, 5px boşluq, 2px nöqtə, 5px boşluq...
ctx.moveTo(0, 100);
ctx.lineTo(300, 100);
ctx.stroke();
```

#### Rənglər, Naxışlar və Qradientlər 🌈
* **`fillStyle`**: `fill()` metodu üçün rəng, qradient və ya naxış.
* **`strokeStyle`**: `stroke()` metodu üçün rəng, qradient və ya naxış.

**Nümunə: Qradient (gradient) ilə doldurmaq**
```javascript
// Soldan sağa xətti bir qradient yaradırıq
const gradient = ctx.createLinearGradient(0, 0, canvas.width / dpr, 0);

// Rəng dayanacaqları əlavə edirik (0.0 -> başlanğıc, 1.0 -> son)
gradient.addColorStop(0.0, "blue");
gradient.addColorStop(0.5, "green");
gradient.addColorStop(1.0, "red");

// fillStyle-a rəng əvəzinə, yaratdığımız qradienti veririk
ctx.fillStyle = gradient;
ctx.fillRect(0, 0, canvas.width / dpr, canvas.height / dpr);
```
#### Mətn Stilləri (Text Styles) ✍️
* **`font`**: Mətnin stilini CSS `font` sintaksisi ilə təyin edir (məsələn, `"bold 24px sans-serif"`).
* **`textAlign`**: Mətnin üfüqi hizalanması (`"left"`, `"center"`, `"right"`).
* **`textBaseline`**: Mətnin şaquli hizalanması (`"top"`, `"middle"`, `"bottom"`, `"alphabetic"`).

#### Kölgələr (Shadows) 👤
* **`shadowColor`**: Kölgənin rəngi.
* **`shadowOffsetX`, `shadowOffsetY`**: Kölgənin obyektdən nə qədər sağa və aşağıda düşəcəyi.
* **`shadowBlur`**: Kölgənin bulanıqlıq dərəcəsi.

**Nümunə:**
```javascript
ctx.shadowColor = "rgba(0,0,0,0.5)";
ctx.shadowOffsetX = 4;
ctx.shadowOffsetY = 4;
ctx.shadowBlur = 5;

ctx.font = "bold 50px sans-serif";
ctx.fillStyle = "red";
ctx.fillText("KÖLGƏLİ MƏTN", 20, 100);
```
#### Vəziyyəti Yadda Saxlamaq və Bərpa Etmək (`save`/`restore`) 💾
Bəzən müvəqqəti olaraq rəngi və ya xətt qalınlığını dəyişib, sonra əvvəlki vəziyyətə qayıtmaq lazım gəlir. Hər dəfə bütün xüsusiyyətləri əl ilə sıfırlamaq əvəzinə, `save()` və `restore()` metodlarından istifadə edirik.
* **`save()`**: Hazırkı bütün qrafik vəziyyətini (rənglər, stillər, transformasıyalar) bir stekə (stack) "itələyir".
* **`restore()`**: Stekdən sonuncu yadda saxlanılmış vəziyyəti götürüb bərpa edir.

**Nümunə:**
```javascript
ctx.fillStyle = 'blue';
ctx.fillRect(10, 10, 100, 100);

ctx.save(); // 1. Mavi rəng vəziyyətini yadda saxlayırıq

ctx.fillStyle = 'red'; // 2. Rəngi qırmızıya dəyişirik
ctx.fillRect(150, 10, 100, 100);

ctx.restore(); // 3. Əvvəlki vəziyyəti (mavi rəng) bərpa edirik

ctx.fillRect(300, 10, 100, 100); // Bu kvadrat mavi rəngdə olacaq!
```
❗️ `save()`/`restore()` metodları hazırkı **yolu (path)** yadda saxlamır, yalnız qrafik atributları saxlayır.

---
Canvas hissesini sonra davam ederik shekiller coxdu burda umumen 15.8 i yeniden yazmag lazimdir.

--- 


Əla, bro! Davam edirik. İndi isə veb səhifələrimizə səs effektləri və musiqi əlavə etməyin yollarını öyrənəcəyik.

***
### 15.9 Səs (Audio) API-ları 🎵

Veb səhifələrinə səs və video yerləşdirmək üçün standart HTML `<audio>` və `<video>` teqləri var. Onların `.play()`, `.pause()`, `.volume` kimi rahat API-ları mövcuddur.

Lakin bu bölmədə biz sırf JavaScript ilə səs effektləri əlavə etməyin iki fərqli və daha proqramatik yoluna baxacağıq.

---
### 15.9.1 `Audio()` Konstruktoru (Constructor) 🔊
Səhifənizə səs effekti əlavə etmək üçün mütləq HTML-ə `<audio>` teqi yazmaq məcburiyyətində deyilsiniz. JavaScript ilə dinamik olaraq bir `<audio>` elementi yarada bilərsiniz. Bunun ən asan yolu `Audio()` konstruktorundan istifadə etməkdir.

**Məntiq:** `new Audio(faylin_url)` ilə yaddaşda bir audio pleyer yaradırsınız və onu sənədə əlavə etmədən belə, istədiyiniz an `.play()` metodu ilə səsləndirə bilərsiniz.

**Geniş Nümunə: Klik Səs Effekti**
Bu nümunə, hər dəfə düyməyə kliklədikdə bir səs effektini necə səsləndirəcəyimizi göstərir.
```javascript
// Tutaq ki, səsimiz `sounds/click-effect.mp3` faylındadır.
// Səsi əvvəlcədən yaddaşa yükləyirik ki, ilk klikdə gecikmə olmasın.
const clickSound = new Audio("sounds/click-effect.mp3");

// HTML-dəki bir düyməni təsəvvür edək: <button id="myButton">Kliklə!</button>
document.getElementById('myButton').addEventListener('click', () => {
  // Hər klikdə səsin bir nüsxəsini (klonunu) yaradıb, onu çalırıq.
  // Bu, istifadəçi sürətli kliklədikdə səslərin bir-birinin üstünə düşərək
  // ahəngdar şəkildə çalmasına imkan verir.
  clickSound.cloneNode().play();
});
```
Burada `.cloneNode()`-dan istifadə etmək vacibdir. Əks halda, səs hələ bitməmiş ikinci dəfə klikləsəniz, birinci səs kəsiləcək və yenidən başlayacaq. Klonlama isə hər dəfə yeni, müstəqil bir pleyer yaradır.

---
### 15.9.2 WebAudio API-ı 🎹
Bu, sadəcə hazır səs fayllarını səsləndirmək üçün deyil, sıfırdan **səs yaratmaq (synthesize)** və onu dərin şəkildə **emal etmək (process)** üçün nəzərdə tutulmuş çox güclü bir API-dır.

**Analogy:** WebAudio API-ı ilə işləmək, köhnə elektron sintezatorları "kabellərlə" (patch cords) bir-birinə qoşmağa bənzəyir. Siz, səs mənbələri (`OscillatorNode`), səs effektləri (`GainNode` - səs gücü) və səs çıxışı (`destination` - dinamiklər) kimi ayrı-ayrı "qutuları" (`AudioNode`) bir-birinə `.connect()` metodu ilə bağlayaraq bir səs sxemi qurursunuz.

Bu API bir az mürəkkəbdir və səs mühəndisliyi anlayışları tələb edə bilər, amma əsaslarını anlamaq üçün aşağıdakı nümunəyə baxaq.

**Geniş Nümunə: Sadə bir akkordun sintez edilməsi**
Bu kod, üç fərqli notadan ibarət bir akkord yaradır və onu bir saniyə ərzində yavaş-yavaş söndürür (fade-out).
```javascript
// 1. Audio Kontekstini yaradırıq - bu, bizim virtual səs studiyamızdır.
// Köhnə Safari brauzerləri üçün `webkitAudioContext` yoxlanılır.
const audioContext = new (window.AudioContext || window.webkitAudioContext)();

// 2. Səs mənbələrini yaradırıq: 3 fərqli not üçün 3 Oscillator Node.
// Oscillator-lar təmiz bir sinusoidal səs dalğası yaradır.
const notes = [293.7, 370.0, 440.0]; // D major akkordunun notaları (D, F#, A)
const oscillators = notes.map(frequency => {
  const oscillator = audioContext.createOscillator();
  oscillator.frequency.value = frequency; // Hər birinin tezliyini (notasini) təyin edirik
  return oscillator;
});

// 3. Səs səviyyəsini idarə etmək üçün bir Gain Node yaradırıq (fade-out effekti üçün).
const volumeControl = audioContext.createGain();
// Səs gücünü 0.02 saniyəyə tam səviyyəyə qaldır...
volumeControl.gain.setTargetAtTime(1, 0.0, 0.02);
// ...sonra isə 0.1 saniyədən başlayaraq 0.2 saniyə ərzində yavaş-yavaş sıfıra endir.
volumeControl.gain.setTargetAtTime(0, 0.1, 0.2);

// 4. Bütün qovşaqları bir-birinə bağlayırıq (sxemi qururuq).
// 📤 Oscillatorlar -> 🎛️ Səs Kontrolu -> 🔊 Dinamiklər
const speakers = audioContext.destination; // Çıxış nöqtəsi (dinamiklər)
oscillators.forEach(oscillator => oscillator.connect(volumeControl));
volumeControl.connect(speakers);

// 5. Nəhayət, səslənməni başladırıq və müəyyən müddət sonra dayandırırıq.
const startTime = audioContext.currentTime;
const stopTime = startTime + 1.25;

oscillators.forEach(o => {
  o.start(startTime);   // Bütün notaları eyni anda başlat
  o.stop(stopTime);     // Və 1.25 saniyə sonra hamısını dayandır
});
```
Bu, WebAudio API-nın sadəcə kiçik bir nümunəsidir. Əgər bu mövzu sizə maraqlı gəldisə, onlayn olaraq daha çox məlumat tapa bilərsiniz.

Əla, bro! Bu dəfəki mövzumuz, JavaScript ilə brauzerin özünü idarə etməyin təməl yollarıdır: səhifənin ünvanını (URL) oxumaq, onu dəyişərək yeni səhifələrə keçmək və istifadəçinin "Geri"/"İrəli" düymələri ilə işləyən naviqasiya tarixçəsini (history) idarə etmək.

***
### 15.10 Məkan (Location), Naviqasiya (Navigation) və Tarixçə (History) 🧭
Həm `Window`, həm də `Document` obyektlərinin **`location`** adlı bir xüsusiyyəti (property) var. Bu xüsusiyyət, pəncərədə göstərilən sənədin hazırkı URL-ini təmsil edən və yeni sənədlər yükləmək üçün API təqdim edən bir `Location` obyektinə istinad edir.

Bu `Location` obyekti, daha əvvəl öyrəndiyimiz `URL` obyektinə çox bənzəyir. Onun `protocol`, `hostname`, `pathname` kimi xüsusiyyətləri ilə hazırkı URL-in hissələrini asanlıqla əldə etmək olar.

**Nümunə: `window.location` obyektini araşdırmaq**
Tutaq ki, brauzerin ünvan sətrində bu URL var: `https://www.example.com/search?q=javascript#results`
```javascript
console.log("Tam URL (href):", window.location.href);
// ✅ "https://www.example.com/search?q=javascript#results"

console.log("Protokol:", window.location.protocol); // ✅ "https:"
console.log("Host adı:", window.location.hostname); // ✅ "www.example.com"
console.log("Yol:", window.location.pathname);       // ✅ "/search"
console.log("Axtarış hissəsi (search):", window.location.search);   // ✅ "?q=javascript"
console.log("Lövbər (hash):", window.location.hash);       // ✅ "#results"
```
❗️ **Qeyd:** `location` obyektinin özünün `.searchParams` xüsusiyyəti yoxdur. URL-in axtarış parametrlərini (`?q=...&page=2`) asanlıqla emal etmək üçün, ondan yeni bir `URL` obyekti yaratmaq lazımdır:
`const params = new URL(window.location).searchParams;`
`const query = params.get("q");`

---
### 15.10.1 Yeni Sənədlərin Yüklənməsi
JavaScript ilə brauzeri proqramatik olaraq yeni bir səhifəyə yönləndirmək çox asandır.

* **`location.href = "..."` və ya `location = "..."`**: Brauzeri yeni bir URL-ə yönləndirməyin ən sadə yoludur.
* **`location.assign("...")`**: `location.href`-ə yeni bir dəyər mənimsətməklə eyni işi görür.
* **`location.replace("...")`**: Yeni səhifəni yükləyir, lakin fərqi odur ki, brauzerin tarixçəsində (history) **hazırkı səhifəni yenisi ilə əvəz edir**. Bu o deməkdir ki, istifadəçi "Geri" düyməsinə kliklədikdə, `replace` edilən səhifəyə deyil, ondan əvvəlki səhifəyə qayıdacaq.
* **`location.reload()`**: Hazırkı səhifəni yeniləyir (F5 düyməsi kimi).

**`assign()` vs. `replace()` Analogy:**
* **`assign()`** 🖼️: Sanki səyahət albomunuza yeni bir şəkil əlavə edirsiniz. Geri qayıtmaq mümkündür.
* **`replace()`** 🗑️: Sanki albomdakı hazırkı şəkli cırıb yerinə yenisini yapışdırırsınız. Köhnə şəkil artıq yoxdur.

**Geniş Nümunə: `replace()` ilə yönləndirmə**
Bu, istifadəçi brauzeri köhnə olduqda və ya lazım olan funksionallığı dəstəkləmədikdə onu başqa bir "statik" səhifəyə yönləndirmək üçün ideal bir yoldur.
```javascript
function isBrowserSupported() {
  // Tutaq ki, brauzerin köhnə olduğunu yoxlayan bir məntiqdir
  return "Promise" in window; // Promise-ləri dəstəkləyirsə, "dəstəklənir" sayırıq
}

if (!isBrowserSupported()) {
  // `replace` istifadə edirik ki, istifadəçi "Geri" düyməsinə basdıqda,
  // yenidən bu səhifəyə qayıdıb sonsuz yönləndirməyə düşməsin.
  window.location.replace("static-page.html");
}
```

---
### 15.10.2 Brauzer Tarixçəsi (Browse History) 📜
`window.history` obyekti, pəncərənin naviqasiya tarixçəsi ilə işləmək üçün metodlar təqdim edir. Təhlükəsizlik səbəbindən, siz bu tarixçədəki URL-lərin siyahısını görə bilməzsiniz, ancaq tarixçə boyunca irəli-geri hərəkət edə bilərsiniz.

* **`history.back()`**: Brauzerin "Geri" düyməsini sıxmaqla eynidir.
* **`history.forward()`**: "İrəli" düyməsini sıxmaqla eynidir.
* **`history.go(n)`**: Tarixçədə `n` addım irəli (`n > 0`) və ya geri (`n < 0`) gedir. `history.go(0)` səhifəni yeniləyir.

Müasir **Tək Səhifəli Proqramlar (Single-Page Applications - SPAs)**, səhifəni tam yeniləmədən məzmunu dinamik olaraq dəyişir. Bu səbəbdən, standart "Geri" və "İrəli" düymələrinin düzgün işləməsi üçün tarixçəni özümüz idarə etməliyik.

---
### 15.10.3 `hashchange` Hadisəsi ilə Tarixçəni İdarə Etmək
Bu, SPA-larda tarixçəni idarə etməyin klassik üsullarından biridir. Məntiqi **URL "hash"-i (`#`)** üzərində qurulub.

**"Hash Tricki" Necə İşləyir:**
1.  URL-in `location.hash` hissəsini (`#`-dən sonrakı hissə) JavaScript ilə dəyişdikdə, səhifə **yenilənmir**.
2.  Amma `location.hash` dəyişdirildikdə, bu, brauzerin tarixçəsinə **yeni bir qeyd** əlavə edir.
3.  Və ən əsası, `location.hash` hər dəfə dəyişdikdə (istər kodla, istərsə də istifadəçi "Geri"/"İrəli" düymələrini basdıqda), `window` obyekti üzərində **`"hashchange"` hadisəsi** baş verir.

**Geniş Nümunə: "Hash" ilə tab-lı interfeys yaratmaq**
```html
<nav>
  <a href="#home">Ana Səhifə</a> | <a href="#about">Haqqımızda</a> | <a href="#contact">Əlaqə</a>
</nav>
<div id="content" style="padding: 10px; border: 1px solid #ccc;"></div>
```
```javascript
const contentDiv = document.querySelector('#content');

// Bu funksiya, hash dəyərinə uyğun olaraq səhifənin məzmununu yeniləyir
function updateContentForHash() {
  const hash = window.location.hash || '#home'; // Əgər hash yoxdursa, #home-u standart götür
  
  console.log(`Hash dəyişdi: ${hash}`);

  switch (hash) {
    case '#home':
      contentDiv.textContent = '🏠 Ana Səhifəyə xoş gəlmisiniz!';
      break;
    case '#about':
      contentDiv.textContent = '🏢 Bizim şirkət haqqında məlumat.';
      break;
    case '#contact':
      contentDiv.textContent = '📞 Əlaqə məlumatları: +994...';
      break;
    default:
      contentDiv.textContent = '❌ Səhifə tapılmadı.';
  }
}

// "hashchange" hadisəsini dinləyirik. Hər dəfə URL-in hash-i dəyişəndə bu funksiya işə düşür.
window.addEventListener('hashchange', updateContentForHash);

// Səhifə ilk dəfə yüklənəndə də məzmunu göstərmək üçün funksiyanı bir dəfə çağırırıq
updateContentForHash();
```
İndi linklərə kliklədikdə və ya brauzerin "Geri"/"İrəli" düymələrini basdıqda, məzmunun səhifə yenilənmədən dəyişdiyini görəcəksiniz.

Aydındır, bro! `hashchange` klassik üsul olsa da, müasir Tək Səhifəli Proqramlarda (Single-Page Applications - SPA) tarixçəni idarə etməyin daha güclü və zərif bir yolu var: **`pushState()` metodu**.

***
### 15.10.4 `pushState()` ilə Tarixçəni İdarə Etmək 💾
Bu texnika, `hashchange` "trick"-indən daha mürəkkəb, lakin daha təmiz və güclüdür. O, URL-in `hash` hissəsini yox, bütün URL-i dəyişməyə (səhifəni yeniləmədən!) və tarixçəyə sadə bir sətir deyil, tam bir **vəziyyət obyekti (state object)** əlavə etməyə imkan verir.

**Əsas Məntiq və Dövrə:**
1.  **Vəziyyəti Saxlamaq:** Proqram yeni bir vəziyyətə keçdikdə (məsələn, istifadəçi başqa bir səhifəyə kliklədikdə), biz bu vəziyyəti təmsil edən bir JavaScript obyektini `history.pushState(state, title, url)` metodu ilə brauzerin tarixçəsinə "əlavə edirik". Bu, həm də brauzerin ünvan sətrindəki URL-i səhifəni yeniləmədən dəyişir.
2.  **Geri Qayıtmaq:** İstifadəçi "Geri" (və ya "İrəli") düyməsini basdıqda, brauzer **`"popstate"`** adlı bir hadisə (event) "atəşləyir".
3.  **Vəziyyəti Bərpa Etmək:** Bizim yazdığımız `popstate` hadisə işləyicisi (handler), hadisə obyekti ilə birlikdə gələn və əvvəlcədən saxladığımız həmin `state` obyektini qəbul edir. Həmin obyektə əsasən, biz interfeysi əvvəlki vəziyyətinə uyğun olaraq yenidən qururuq (re-render).

#### `history.pushState()` Metodunun Arqumentləri
`history.pushState(state, title, url)`
* **`state`**: Tarixçədə saxlamaq istədiyimiz, proqramın hazırkı vəziyyətini təmsil edən JavaScript obyekti. Bu obyekt **Structured Clone Algorithm** ilə kopyalanır, yəni `JSON.stringify`-dan daha güclüdür və `Map`, `Set`, `Date` kimi tipləri də saxlaya bilir.
* **`title`**: Bu arqument əksər brauzerlər tərəfindən istifadə olunmur. Adətən boş sətir `""` ötürülür.
* **`url`**: Ünvan sətrində göstəriləcək yeni URL. Bu, istifadəçilərin proqramın daxili vəziyyətlərini əlfəcin (bookmark) etməsinə və ya paylaşmasına imkan verir.

**`history.replaceState()`**: `pushState` kimidir, lakin tarixçəyə yeni bir qeyd əlavə etmək əvəzinə, hazırkı qeydi əvəz edir. Adətən səhifə ilk dəfə yüklənəndə ilkin vəziyyəti təyin etmək üçün istifadə olunur.

---
### Geniş Nümunə: Rəqəm Tapma Oyunu 🎮
Bu nümunə, yuxarıda izah edilən bütün məntiqi birləşdirərək tam işlək bir tək səhifəli proqram (SPA) yaradır. İstifadəçi hər təxmin etdikdə, oyunun vəziyyəti tarixçəyə yazılır və "Geri" düyməsi ilə əvvəlki təxminlərə qayıtmaq mümkün olur.

**1. HTML və CSS Quruluşu**
```html
<html>
<head>
  <title>Mən bir rəqəm tutmuşam...</title>
  <style>
    body { display: flex; flex-direction: column; align-items: center; justify-content: space-evenly; font-family: sans-serif; }
    #heading { font: bold 36px sans-serif; }
    #container { border: solid black 1px; width: 80%; background-color: #f0f0f0; }
    #range { background-color: #4CAF50; height: 1.5em; transition: all 0.3s ease; }
    #input { font-size: 24px; width: 60%; padding: 5px; margin-top: 20px; text-align: center; }
    #playagain { font-size: 24px; padding: 10px; border-radius: 5px; margin-top: 20px; cursor: pointer; }
  </style>
</head>
<body>
  <h1 id="heading">Mən bir rəqəm tutmuşam...</h1>
  <div id="container"><div id="range"></div></div>
  <input id="input" type="text" autofocus>
  <button id="playagain" hidden onclick="location.search='';">Yenidən Oyna</button>

  <script> /* ... JavaScript kodu aşağıdadır ... */ </script>
</body>
</html>
```

**2. JavaScript Kodu və İzahı**
```javascript
/**
 * GameState sinifi, oyunumuzun bütün daxili vəziyyətini idarə edir.
 */
class GameState {
  // Yeni bir oyun başlatmaq üçün statik (static) "factory" metodu
  static newGame() {
    let s = new GameState();
    s.secret = Math.floor(Math.random() * 100) + 1; // 1-100 arası gizli rəqəm
    s.low = 0; s.high = 101; // Təxmin aralığı
    s.numGuesses = 0; // Təxminlərin sayı
    s.guess = null;   // Son təxmin
    return s;
  }

  // `popstate` hadisəsindən gələn sadə obyektdən GameState nüsxəsini bərpa edir
  static fromStateObject(stateObject) {
    let s = new GameState();
    for(let key of Object.keys(stateObject)) { s[key] = stateObject[key]; }
    return s;
  }
  
  // Oyunun hazırkı vəziyyətini URL-ə çevirir (bookmark üçün)
  toURL() {
    let url = new URL(window.location);
    url.searchParams.set("l", this.low);
    url.searchParams.set("h", this.high);
    url.searchParams.set("n", this.numGuesses);
    if(this.guess) url.searchParams.set("g", this.guess);
    return url.href;
  }

  // URL-dən oyun vəziyyətini bərpa edir
  static fromURL(url) {
    let s = new GameState();
    let params = new URL(url).searchParams;
    s.low = parseInt(params.get("l"));
    s.high = parseInt(params.get("h"));
    s.numGuesses = parseInt(params.get("n"));
    s.guess = parseInt(params.get("g"));
    
    // Əgər URL-də məlumatlar tam deyilsə və ya səhvdirsə, null qaytarırıq
    if (isNaN(s.low) || isNaN(s.high) || isNaN(s.numGuesses)) return null;
    
    // URL-dən qayıdanda təhlükəsizlik üçün yeni bir gizli rəqəm seçirik
    s.secret = Math.floor(Math.random() * (s.high - s.low - 1)) + s.low + 1;
    return s;
  }

  // DOM-u hazırkı oyun vəziyyətinə uyğun olaraq yeniləyir (render)
  render() {
    const heading = document.querySelector("#heading");
    const range = document.querySelector("#range");
    const input = document.querySelector("#input");
    const playagain = document.querySelector("#playagain");

    heading.textContent = document.title = `Mən ${this.low} və ${this.high} arası bir rəqəm tutmuşam...`;
    range.style.marginLeft = `${this.low}%`;
    range.style.width = `${(this.high-this.low)}%`;
    input.value = "";
    input.focus();

    if (this.guess === null) {
      input.placeholder = "Təxmininizi daxil edin";
    } else if (this.guess < this.secret) {
      input.placeholder = `${this.guess} çox aşağıdır. Yenidən cəhd edin!`;
    } else if (this.guess > this.secret) {
      input.placeholder = `${this.guess} çox yuxarıdır. Yenidən cəhd edin!`;
    } else {
      heading.textContent = document.title = `${this.guess} doğrudur! ${this.numGuesses} cəhddə tapdınız!`;
      input.hidden = true;
      playagain.hidden = false;
    }
  }

  // İstifadəçinin təxmininə görə vəziyyəti yeniləyir
  updateForGuess(guess) {
    if ((guess > this.low) && (guess < this.high)) {
      if (guess < this.secret) this.low = guess;
      else if (guess > this.secret) this.high = guess;
      this.guess = guess;
      this.numGuesses++;
      return true; // Vəziyyət yeniləndi
    }
    return false; // Keçərsiz təxmin
  }
}

// ---- ƏSAS MƏNTİQ: Hadisələrin bir-birinə bağlanması ----

// 1. Səhifə yüklənəndə, ya URL-dən vəziyyəti bərpa edirik, ya da yeni oyun başlayırıq.
let gamestate = GameState.fromURL(window.location) || GameState.newGame();

// 2. Bu ilkin vəziyyəti tarixçədə `replaceState` ilə saxlayırıq.
history.replaceState(gamestate, "", gamestate.toURL());

// 3. İlkin vəziyyəti render edirik.
gamestate.render();

// 4. İstifadəçi input-a bir dəyər yazıb "Enter" basdıqda ("change" hadisəsi).
document.querySelector("#input").onchange = (event) => {
  if (gamestate.updateForGuess(parseInt(event.target.value))) {
    // Əgər təxmin keçərlidirsə, yeni vəziyyəti `pushState` ilə tarixçəyə yazırıq.
    history.pushState(gamestate, "", gamestate.toURL());
  }
  // Və hər təxmindən sonra interfeysi yeniləyirik.
  gamestate.render();
};

// 5. İstifadəçi brauzerin "Geri" və ya "İrəli" düyməsini basdıqda.
window.onpopstate = (event) => {
  // `event.state`-dən saxladığımız obyekti götürüb, vəziyyəti bərpa edirik.
  console.log("popstate hadisəsi baş verdi. Vəziyyət bərpa olunur.");
  gamestate = GameState.fromStateObject(event.state);
  // Və həmin vəziyyətə uyğun interfeysi yenidən render edirik.
  gamestate.render();
};
```

Əla, bro! Bu fəslin ən vacib və ən çox istifadə edəcəyiniz hissələrindən birinə gəldik: **Şəbəkə Əməliyyatları**. Müasir veb proqramları demək olar ki, həmişə serverlərlə "danışır". Gəlin görək bunu necə edəcəyik.

***
### 15.11 Şəbəkə Əməliyyatları (Networking) 🌐
Veb səhifəniz hər dəfə yüklənəndə, brauzer arxa planda onsuz da bir çox şəbəkə sorğusu edir (HTML, CSS, şəkil, skript faylları üçün). Lakin JavaScript, bizə proqramın içindən, istədiyimiz zaman serverə sorğu göndərmək üçün xüsusi API-lar təqdim edir.

Bu bölmədə ən vacib olan **`fetch()` API**-ını detallı şəkildə araşdıracağıq.

---
### 15.11.1 `fetch()` API-ı ilə Sorğular
`fetch()` API-ı, şəbəkə sorğuları etmək üçün müasir, güclü və `Promise`-əsaslı bir vasitədir. O, köhnə və mürəkkəb `XMLHttpRequest` obyektini tamamilə əvəz edir.

Sadə bir `GET` sorğusu üçün `fetch()` ilə işləmək adətən 3 addımlı bir prosesdir:
1.  `fetch(url)` funksiyasını çağırırsınız və o, sizə dərhal bir `Promise` qaytarır.
2.  Həmin Promise yerinə yetirildikdə (`fulfilled`), siz bir `Response` obyekti alırsınız. Bu obyektin `.json()` və ya `.text()` kimi metodlarını çağıraraq, cavabın məzmununu (body) istəyirsiniz. Bu metodlar da özləri **yeni bir Promise** qaytarır.
3.  İkinci Promise də yerinə yetirildikdə, nəhayət, sizə lazım olan son data (JSON obyekti və ya mətn) gəlir və siz onu emal edirsiniz.

Bu iki asinxron addım səbəbindən, `fetch` ilə işləyərkən adətən iki `.then()` və ya iki `await` ifadəsi görürük.

**Nümunə 1: `.then()` zənciri ilə JSON almaq**
```javascript
fetch('/api/users/current') // 1. GET sorğusu göndər
  .then(response => response.json()) // 2. Cavabın məzmununu JSON kimi emal et
  .then(currentUser => {             // 3. Emal olunmuş JSON ilə işlə
    console.log("İstifadəçi:", currentUser);
    // displayUserInfo(currentUser);
  })
  .catch(error => console.error("Xəta baş verdi:", error));
```

**Nümunə 2: `async/await` ilə mətn almaq (Daha müasir və oxunaqlı)**
```javascript
async function isServiceReady() {
  // 1. Sorğunun cavabının gəlməsini gözləyirik
  const response = await fetch('/api/service/status');
  // 2. Cavabın məzmununun mətn kimi oxunmasını gözləyirik
  const bodyText = await fetch(response.text());
  // 3. Nəticəni istifadə edirik
  return bodyText === "ready";
}
```

#### HTTP Statusları, Başlıqlar (Headers) və Xətaların İdarə Edilməsi 🛡️
`fetch()`-in ən vacib və çox vaxt çaşqınlıq yaradan xüsusiyyəti budur:

❗️ **"fetch gotcha":** `fetch()` sorğusu, server `404 (Not Found)` və ya `500 (Server Error)` kimi bir xəta statusu qaytarsa belə, **rədd edilmir (rejected)**! O, yalnız şəbəkə problemi (məsələn, internet kəsintisi) olduqda rədd edilir. Buna görə də, cavabın statusunu **həmişə əl ilə yoxlamaq lazımdır!**

Bunun üçün `Response` obyektinin `ok` xüsusiyyətindən istifadə edə bilərsiniz. `response.ok` yalnız HTTP statusu `200-299` aralığında olduqda `true` olur.

**Geniş və Realistik Nümunə:**
```javascript
async function getCurrentUser() {
  try {
    const response = await fetch("/api/user/profile");

    // 1. Statusu yoxlayırıq
    if (!response.ok) {
      throw new Error(`HTTP xətası! Status: ${response.status}`);
    }

    // 2. Cavabın tipini yoxlayırıq (həqiqətən JSON gəlirmi?)
    const contentType = response.headers.get("content-type");
    if (!contentType || !contentType.includes("application/json")) {
      throw new TypeError("Gözlənilən format JSON deyildi!");
    }
    
    // 3. Hər şey qaydasındadırsa, məzmunu qaytarırıq
    const user = await response.json();
    return user;

  } catch (error) {
    console.error("İstifadəçini almaq mümkün olmadı:", error);
    // Xətanı yuxarıya ötürürük ki, çağıran kod da xəbərdar olsun
    throw error;
  }
}
```

#### Sorğuya Parametr və Başlıq (Header) Əlavə Etmək
`fetch()` funksiyası ikinci bir arqument olaraq, sorğunu detallı konfiqurasiya etmək üçün bir `options` obyekti qəbul edir.

* **`method`**: Sorğunun metodu (`"GET"`, `"POST"`, `"PUT"`, `"DELETE"`).
* **`headers`**: Sorğuya əlavə ediləcək HTTP başlıqları (headers). Məsələn, autentikasiya üçün.
* **`body`**: `POST` və ya `PUT` sorğuları ilə serverə göndəriləcək məlumat.

**Geniş Nümunə: Serverə `POST` sorğusu ilə yeni data göndərmək**
```javascript
async function createNewUser(userData) {
  try {
    const response = await fetch("/api/users", {
      method: "POST", // Metodu POST olaraq təyin edirik
      headers: {
        // Serverə JSON formatında data göndərdiyimizi bildiririk
        "Content-Type": "application/json",
        // Lazım gələrsə, autentikasiya başlığını əlavə edirik
        "Authorization": "Bearer YOUR_API_TOKEN"
      },
      // Göndərəcəyimiz JavaScript obyektini JSON sətrinə çeviririk
      body: JSON.stringify(userData)
    });

    if (!response.ok) {
      throw new Error(`Server xətası: ${response.status}`);
    }

    const createdUser = await response.json();
    console.log("✅ Yeni istifadəçi yaradıldı:", createdUser);
    return createdUser;

  } catch (error) {
    console.error("❌ İstifadəçi yaradıla bilmədi:", error);
  }
}

// İstifadəsi:
// createNewUser({ name: "Ayan", job: "Developer" });
```
#### Sorğunun Ləğv Edilməsi (Aborting a Request) ⏱️
Bəzən uzun çəkən bir sorğunu ləğv etmək lazım gəlir (məsələn, istifadəçi "Ləğv et" düyməsinə basdıqda). Bunu `AbortController` ilə etmək olar.

**Nümunə: 5 saniyəlik "timeout" ilə `fetch`**
```javascript
function fetchWithTimeout(url, timeout = 5000) {
  const controller = new AbortController();
  // `setTimeout` ilə ləğv etmə əməliyyatını planlaşdırırıq
  const timeoutId = setTimeout(() => controller.abort(), timeout);

  return fetch(url, {
    signal: controller.signal // Abort siqnalını sorğuya bağlayırıq
  }).then(response => {
    // Əgər sorğu uğurlu olubsa, timeout-u ləğv edirik ki, boş yerə işləməsin
    clearTimeout(timeoutId);
    return response;
  }).catch(error => {
    // Əgər xəta "AbortError"dirsə, deməli timeout baş verib
    if (error.name === 'AbortError') {
      throw new Error('Sorğu zaman aşımına uğradı!');
    }
    throw error;
  });
}

// İstifadəsi:
// fetchWithTimeout("/api/slow-data")
//   .then(response => response.json())
//   .then(data => console.log(data))
//   .catch(error => console.error(error.message));
```

Əla, bro! `fetch` ilə sorğu göndərməyi öyrəndik. Amma bu, həmişə "client-server" dialoqudur. Bəs əgər server bizə özü istədiyi vaxt, durduq yerə "salam" göndərmək istəsə necə? Bunun üçün **Server-Sent Events (SSE)** texnologiyası var.

***
### 15.11.2 Server-tərəfli Hadisələr (Server-Sent Events - SSE) 📡

Normalda vebdə ünsiyyət "sorğu-cavab" kimidir: sən (client) sual verirsən, o (server) cavab verir. **SSE** isə sanki bir **radio yayımıdır**. Sən bir dəfə radiostansiyaya (`EventSource`) qoşulursan və o, sənə istədiyi vaxt, fasiləsiz olaraq yeni mahnılar (məlumatlar) göndərməyə davam edir.

Bu texnikanın arxasında **uzunmüddətli sorğu (long polling)** dayanır:
1.  Client serverə bir sorğu göndərir.
2.  Server bu bağlantını **bağlamır**, açıq saxlayır.
3.  Serverin client-ə göndərməli yeni bir məlumatı olduqda, həmin açıq bağlantı vasitəsilə datanı yazır.
4.  Bağlantı qırılarsa, client avtomatik olaraq yenidən qoşulmağa cəhd edir.

Brauzerdə bu mexanizmi asanlıqla istifadə etmək üçün **`EventSource`** API-ı var.

**API:** `new EventSource(url)`
Bu, verilmiş URL-ə davamlı bir bağlantı yaradır. Serverdən hər dəfə yeni bir məlumat gəldikdə, bu obyekt bir hadisə (event) "atəşləyir" və biz də ona `addEventListener` ilə "qulaq asa" bilərik.

* Gələn məlumat, hadisə obyektinin **`.data`** xüsusiyyətində olur.
* Server, hadisələrə xüsusi adlar verə bilər (məsələn, `"chat"`). Əgər ad verməzsə, standart hadisə adı `"message"` olur.

---
### Geniş Nümunə: Real-Time Çat Tətbiqi (Real-Time Chat Application) 💬
Gəlin bu texnologiya ilə tam işlək, çox sadə bir çat proqramı yaradaq. Bu nümunə iki hissədən ibarətdir: client (brauzer) və server (Node.js).

#### Addım 1: Client Tərəfi (Brauzer Kodu - `chatClient.html`)
Bu, istifadəçinin brauzerində görəcəyi HTML faylıdır. O, həm mesajları almaq üçün `EventSource` yaradır, həm də mesaj göndərmək üçün `fetch` istifadə edir.
```html
<html>
<head><title>SSE Çat</title></head>
<body>
  <input id="input" style="width:100%; padding:10px; border:solid black 2px"/>

  <script>
    // İstifadəçidən ləqəbini alırıq
    const nick = prompt("Ləqəbinizi daxil edin:");
    const input = document.getElementById("input");
    input.focus(); // Səhifə açılan kimi yazı sahəsi aktiv olsun

    // 1. Mesajları ALMAQ üçün EventSource yaradırıq
    console.log("Çata qoşulunur...");
    const chat = new EventSource("/chat"); // Serverdəki /chat ünvanına qoşuluruq

    // Serverdən "chat" növlü bir hadisə gəldikdə...
    chat.addEventListener("chat", event => {
      const div = document.createElement("div"); // Yeni bir div yaradırıq
      div.textContent = event.data; // Mesajın məzmununu div-ə yazırıq
      input.before(div); // Və onu input sahəsindən əvvələ əlavə edirik
      input.scrollIntoView(); // Həmişə son mesajın görünməsini təmin edirik
    });

    // 2. Mesaj GÖNDƏRMƏK üçün input-un "change" hadisəsini dinləyirik
    input.addEventListener("change", () => {
      // İstifadəçi yazıb Enter-ə basdıqda bu işə düşür
      fetch("/chat", {
        method: "POST", // POST sorğusu göndəririk
        body: nick + ": " + input.value // Gövdəyə ləqəb və mesajı yazırıq
      })
      .catch(e => console.error("Mesaj göndərilə bilmədi:", e));
      
      input.value = ""; // Mesaj göndərildikdən sonra input-u təmizləyirik
    });
  </script>
</body>
</html>
```

#### Addım 2: Server Tərəfi (Node.js Kodu - `chatServer.js`)
Bu kod, yuxarıdakı client-ə xidmət edən sadə bir Node.js serveridir.
```javascript
// Bu kod Node.js ilə işə salınmalıdır

const http = require("http");
const fs = require("fs");
const url = require("url");

// Client üçün HTML faylını yaddaşa oxuyuruq
const clientHTML = fs.readFileSync("chatClient.html");

// Aktiv client-lərin `response` obyektlərini saxlamaq üçün bir massiv
let clients = [];

// Serveri yaradıb, 8080 portunda dinləməyə başlayırıq
const server = new http.Server();
server.listen(8080);

// Hər yeni sorğu üçün bu funksiya işə düşür
server.on("request", (request, response) => {
  const pathname = url.parse(request.url).pathname;

  if (pathname === "/") { // Əgər sorğu ana səhifəyədirsə...
    // Client-ə HTML faylımızı göndəririk
    response.writeHead(200, {"Content-Type": "text/html"}).end(clientHTML);

  } else if (pathname === "/chat" && request.method === "GET") { // Əgər GET sorğusudursa...
    // Client SSE bağlantısı qurur. Onu qəbul edirik.
    acceptNewClient(request, response);

  } else if (pathname === "/chat" && request.method === "POST") { // Əgər POST sorğusudursa...
    // Client yeni bir mesaj göndərir. Onu hamıya yayımlayırıq.
    broadcastNewMessage(request, response);

  } else { // Başqa bütün hallar üçün 404 xətası qaytarırıq
    response.writeHead(404).end();
  }
});

function acceptNewClient(request, response) {
  // Yeni client-in `response` obyektini `clients` massivinə əlavə edirik
  clients.push(response);
  
  // Əgər client bağlantını kəsərsə, onu siyahıdan silirik
  request.connection.on("end", () => {
    clients.splice(clients.indexOf(response), 1);
    response.end();
  });
  
  // SSE üçün vacib olan başlıqları (headers) təyin edirik
  response.writeHead(200, {
    "Content-Type": "text/event-stream",
    "Connection": "keep-alive",
    "Cache-Control": "no-cache",
  });
  
  // Qoşulan client-ə "Xoş gəldin" mesajı göndəririk
  response.write("event: chat\ndata: Qoşuldunuz!\n\n");
  
  // `response.end()` çağırmırıq! Bağlantını açıq saxlayırıq.
}

async function broadcastNewMessage(request, response) {
  // POST sorğusunun gövdəsini (body) oxuyuruq
  let body = "";
  for await (const chunk of request) { body += chunk; }
  
  // Client-ə boş bir 200 OK cavabı göndərib bağlantını bağlayırıq
  response.writeHead(200).end();
  
  // Gələn mesajı SSE formatına salırıq
  const message = "data: " + body.replace(/\n/g, "\ndata: ");
  const event = `event: chat\n${message}\n\n`;
  
  // Və bu hadisəni qoşulu olan BÜTÜN client-lərə yazırıq
  clients.forEach(client => client.write(event));
}

console.log("Çat serveri http://localhost:8080 ünvanında işləyir...");
```
**Necə İşə Salmalı?**
1.  Bu iki kodu `chatClient.html` və `chatServer.js` adları ilə eyni qovluqda saxlayın.
2.  Terminalda `node chatServer.js` əmrini icra edin.
3.  Brauzerdə `http://localhost:8080` ünvanını açın və bir neçə fərqli tabda açaraq özünüzlə söhbət edin!

Olar, bro! Şəbəkə əməliyyatlarının sonuncu və ən güclü növü ilə tanış olaq: **WebSockets**. Bu texnologiya, client ilə server arasında real-time, iki tərəfli bir "dəhliz" açır.

***
### 15.11.3 WebSockets ↔️

Server-Sent Events (SSE) serverdən client-ə doğru **tək tərəfli** bir məlumat axını idisə, WebSockets client və server arasında **iki tərəfli (bidirectional)** bir ünsiyyət kanalı yaradır.

**Analogy:** Əgər SSE bir radio yayımı idisə (tək tərəfli), **WebSocket** bir **telefon danışığıdır** 📞. Hər iki tərəf istədiyi vaxt danışa və bir-birinə məlumat (həm mətn, həm də fayl/binary data) göndərə bilər.

Bu texnologiya, real-time çat proqramları, onlayn oyunlar və birja məlumatlarını canlı izləmə kimi tətbiqlər üçün idealdır.

**Necə İşləyir?**
WebSocket bağlantısı, xüsusi `ws://` (təhlükəsiz olmayan) və ya `wss://` (təhlükəsiz) protokolları ilə işləyən URL-lərə edilir. Bağlantı qurularkən, brauzer əvvəlcə serverə bir HTTP sorğusu göndərir və `Upgrade: websocket` başlığı (header) ilə bağlantının statusunu HTTP-dən WebSocket protokoluna yüksəltməyi xahiş edir. Bu o deməkdir ki, WebSocket istifadə etmək üçün server tərəfinin də bu protokolu dəstəkləməsi mütləqdir.

#### `WebSocket` API-ı ilə İşləmək

JavaScript-də WebSocket ilə işləmək üçün `WebSocket` obyektindən istifadə edirik. Bütün proses hadisələr (events) üzərində qurulub.

**Bağlantının Vəziyyətləri (`readyState`)**
Bir `WebSocket` obyekti 4 fərqli vəziyyətdə ola bilər:
* `WebSocket.CONNECTING` (0): Qoşulma prosesi gedir.
* `WebSocket.OPEN` (1): Bağlantı uğurla qurulub və data mübadiləsinə hazırdır.
* `WebSocket.CLOSING` (2): Bağlantı bağlanma prosesindədir.
* `WebSocket.CLOSED` (3): Bağlantı tamamilə bağlanıb və ya qurula bilməyib.

---
### Geniş Nümunə: WebSocket ilə Canlı Əlaqə Qurmaq 💬
Bu nümunə, bir WebSocket serverinə qoşulmağın, mesaj göndərib almağın və bağlantını idarə etməyin bütün mərhələlərini göstərir. Test üçün, bizə göndərdiyimiz hər mesajı əks-səda kimi geri qaytaran ictimai bir test serverindən (`wss://echo.websocket.events`) istifadə edəcəyik.

```javascript
// 1. WebSocket serverinə qoşuluruq
console.log("WebSocket serverinə qoşulmağa cəhd edilir...");
// wss:// təhlükəsiz bağlantı deməkdir
const socket = new WebSocket("wss://echo.websocket.events");


// --- 2. Hadisə Dinləyicilərini (Event Listeners) Təyin Edirik ---

// `onopen` hadisəsi: Bağlantı uğurla qurulduqda işə düşür
socket.onopen = (event) => {
  console.log("✅ Bağlantı uğurla quruldu!");
  
  // Bağlantı qurulan kimi serverə bir mesaj göndəririk
  console.log("📤 Serverə ilk mesaj göndərilir: 'Salam!'");
  socket.send("Salam, WebSocket serveri!");
};


// `onmessage` hadisəsi: Serverdən yeni bir mesaj gəldikdə işə düşür
socket.onmessage = (event) => {
  // Gələn məlumat `event.data`-nın içində olur
  console.log(`📥 Serverdən gələn mesaj (əks-səda): "${event.data}"`);
};


// `onclose` hadisəsi: Bağlantı bağlandıqda işə düşür
socket.onclose = (event) => {
  if (event.wasClean) {
    // Əgər bağlantı normal şəkildə (məsələn, socket.close() ilə) bağlanıbsa
    console.log(`🔌 Bağlantı normal şəkildə bağlandı (kod=${event.code}, səbəb=${event.reason || 'yoxdur'})`);
  } else {
    // Məsələn, server prosesi dayandırıldıqda və ya şəbəkə problemi olduqda
    console.error("❗️ Bağlantı qəfil kəsildi!");
  }
};


// `onerror` hadisəsi: Xəta baş verdikdə
socket.onerror = (error) => {
  // Adətən `onerror`-dən sonra `onclose` da baş verir
  console.error(`❌ WebSocket xətası baş verdi.`);
};


// --- 3. Proqramatik olaraq mesaj göndərmək və bağlantını bağlamaq ---
setTimeout(() => {
    // Mesaj göndərməzdən əvvəl bağlantının AÇIQ olduğunu yoxlamaq yaxşı təcrübədir
    if (socket.readyState === WebSocket.OPEN) {
        console.log("📤 İkinci mesaj göndərilir...");
        socket.send("Bu da ikinci mesajdır.");
    }
}, 3000);

// 5 saniyə sonra bağlantını özümüz bağlayırıq
setTimeout(() => {
    if (socket.readyState === WebSocket.OPEN) {
        console.log("Bağlantı bağlanılır...");
        // .close() metodu ilə bağlantını normal şəkildə bağlayırıq
        socket.close(1000, "İşimi bitirdim");
    }
}, 5000);
```
Bu kod, WebSocket-in bütün həyat dövrünü (lifecycle) – yaranma, mesajlaşma və bağlanma – əhatə edir.

Olar, bro! Veb proqramlarımızı daha "ağıllı" etməyin vaxtı gəldi. Bu bölmədə, brauzerin istifadəçinin kompüterində məlumatları necə saxladığını və bu yaddaşla necə işlədiyimizi öyrənəcəyik.

***
### 15.12 Yaddaş (Storage) 💾
Veb proqramları, istifadəçi təcrübəsini yaxşılaşdırmaq üçün brauzer API-larından istifadə edərək məlumatları birbaşa istifadəçinin cihazında saxlaya bilər. Bu, brauzerə bir növ "yaddaş" qazandırır. Məsələn, istifadəçinin seçimlərini (mövzu: qaranlıq/işıqlı), dil seçimini və ya hətta yarımçıq qalmış bir formadakı məlumatları saxlaya bilərik ki, o, sayta yenidən daxil olanda hər şeyi qaldığı yerdən davam etdirsin.

Bu yaddaş, təhlükəsizlik üçün **mənbəyə (origin)** görə təcrid olunub, yəni `a.com` saytı `b.com` saytının saxladığı məlumatları oxuya bilməz.

Client-side yaddaşın bir neçə növü var, biz bu bölmədə ən çox istifadə olunan **Web Storage API**-ına baxacağıq.

---
### 15.12.1 Web Storage: `localStorage` və `sessionStorage`
Web Storage API-ı, məlumatları `açar:dəyər` (key:value) formatında saxlamaq üçün iki obyekt təqdim edir. Onlar adi JavaScript obyektləri kimi istifadə olunur, lakin iki vacib qaydası var:
1.  Saxlanılan dəyərlər (values) **mütləq sətir (string)** olmalıdır.
2.  Saxlanılan məlumatlar səhifə yeniləndikdən sonra belə **qalıcıdır (persistent)**.

#### Əsas API Əməliyyatları (Basic API Operations) 📦
`localStorage` və `sessionStorage` eyni metodlara malikdir. Nümunələri `localStorage` ilə göstərəcəyik.

**Geniş Nümunə:**
```javascript
// --- 1. Dəyər Yazmaq (Setting a Value) ---
// İki fərqli üsulla yazmaq olar:
localStorage.setItem('username', 'ayan_m');
localStorage.theme = 'dark'; // Property kimi də yazmaq olar

console.log("Yaddaşa yazıldı!");

// --- 2. Dəyəri Oxumaq (Getting a Value) ---
const username = localStorage.getItem('username');
const theme = localStorage.theme;

console.log(`Xoş gəldin, ${username}! Seçdiyin mövzu: ${theme}.`);
// ✅ Nəticə: Xoş gəldin, ayan_m! Seçdiyin mövzu: dark.

// --- 3. Dəyəri Silmək (Removing a Value) ---
localStorage.removeItem('theme');
console.log("`theme` açarı yaddaşdan silindi.");

// --- 4. Bütün yaddaşı təmizləmək ---
// localStorage.clear();
// console.log("Bütün lokal yaddaş təmizləndi.");
```

#### String Olmayan Datanın Saxlanması (`JSON` ilə)
`localStorage` yalnız sətirləri (strings) saxlaya bildiyi üçün, obyekt və ya massiv (array) kimi mürəkkəb dataları saxlamaq üçün onları əvvəlcə sətirə çevirmək lazımdır. Bunun üçün ən yaxşı yol `JSON`-dır.

**Nümunə:**
```javascript
const userSettings = {
  language: "az",
  notifications: true,
  volume: 80
};

// 1. Obyekti JSON sətrinə çevirib yaddaşa yazırıq
localStorage.setItem('settings', JSON.stringify(userSettings));
console.log("Ayarlar yaddaşa JSON formatında yazıldı.");

// 2. Yaddaşdan oxuyarkən JSON sətrini yenidən obyektə çeviririk
const savedSettingsString = localStorage.getItem('settings');
const savedSettingsObject = JSON.parse(savedSettingsString);

console.log("Yaddaşdan oxunan obyekt:", savedSettingsObject);
console.log("Bildirişlər aktivdirmi?", savedSettingsObject.notifications); // ✅ true
```
---
#### `localStorage` vs. `sessionStorage`: Həyat Müddəti və Əhatə Dairəsi (Lifetime and Scope) 🚪
Bu iki obyektin API-ları eyni olsa da, onların davranışı tamamilə fərqlidir.

**`localStorage` (Daimi Yaddaş)**
* **Həyat Müddəti ⏳:** Daimidir. Brauzer bağlanıb-açılsa belə, məlumatlar silinmir. Yalnız JavaScript kodu (`clear()` və ya `removeItem()`) və ya istifadəçi özü brauzer ayarlarından təmizlədikdə silinir.
* **Əhatə Dairəsi 🌍:** Mənbə (origin) üzrə qlobaldır. Eyni mənbədən (`https://example.com`) açılmış **bütün tablar və pəncərələr** eyni `localStorage`-ı paylaşır. Bir tabda edilən dəyişiklik digər tabda dərhal görünür.

**`sessionStorage` (Sessiya Yaddaşı)**
* **Həyat Müddəti ⏳:** Yalnız bir **sessiya** (session) qədərdir. Sessiya, bir brauzer **tabının** ömrüdür. Tab bağlanan kimi, həmin tabın `sessionStorage`-ı tamamilə silinir.
* **Əhatə Dairəsi 🏠:** Hər bir tab üçün **ayrıdır**. Eyni mənbədən açılmış iki fərqli tab, bir-birinin `sessionStorage`-ını görə və ya dəyişə bilməz.

---
#### "storage" Hadisəsi - Tablar Arası Ünsiyyət 🔄
`localStorage` eyni mənbədən açılmış tablar arasında bir "rabitə kanalı" kimi də istifadə edilə bilər. Bir tabda `localStorage` dəyişdirildikdə, **digər bütün tablarda** `"storage"` hadisəsi baş verir.

❗️ **Vacib:** Bu hadisə dəyişikliyi edən tabın özündə yox, digər tablarda baş verir.

**Nümunə: `storage` hadisəsini tutmaq**
Bu kodu test etmək üçün, eyni saytı iki fərqli brauzer tabında açın.

**Birinci Tabda bu kodu konsola yazın:**
```javascript
window.addEventListener('storage', (event) => {
  console.log("Başqa bir tabda yaddaş dəyişdi!");
  console.log(`Açar (key): ${event.key}`);
  console.log(`Köhnə dəyər (oldValue): ${event.oldValue}`);
  console.log(`Yeni dəyər (newValue): ${event.newValue}`);
});
console.log("Yaddaş dəyişiklikləri üçün dinləməyə başladım...");
```

**İndi isə, İkinci Tabda bu kodu konsola yazın:**
```javascript
// Bu kodu işə saldıqda, birinci tabın konsolunda hadisə məlumatları görünəcək
localStorage.setItem('broadcast-message', 'Salam, qonşu tab! ' + Math.random());
```
Bu mexanizm, bir tabda edilən dəyişikliyin (məsələn, istifadəçinin sistemdən çıxması) digər bütün açıq tablarda dərhal əks olunması üçün istifadə edilə bilər.

Olar, bro! Veb yaddaşı mövzusunun digər iki növü ilə tanış olaq: köhnə, amma hələ də bəzən rastlaşacağımız **"Cookie"-lər** və çox güclü, amma bir o qədər də mürəkkəb olan **`IndexedDB`**.

***
### 15.12.2 "Cookie"-lər 🍪
**Cookie (kuki)**, veb brauzer tərəfindən saxlanılan və müəyyən bir veb saytla əlaqələndirilən kiçik həcmli, adlandırılmış bir datadır. Onlar əsasən server tərəfi (server-side) proqramlaşdırması üçün nəzərdə tutulub. Cookie-lərin əsas xüsusiyyəti odur ki, onlar hər bir HTTP sorğusu ilə avtomatik olaraq serverə göndərilir. Bu, serverin istifadəçi sessiyalarını (sessions) izləməsi üçün faydalıdır.

Client tərəfində isə, cookie-lərlə işləmək üçün API çox köhnə və "kriptikdir". Bütün əməliyyatlar `document.cookie` adlı tək bir xüsusiyyəti (property) oxuyub-yazmaqla həyata keçirilir.

**Cookie-ləri Oxumaq**
`document.cookie`-ni oxuduqda, o, sizə `ad1=dəyər1; ad2=dəyər2; ...` formatında tək bir sətir (string) qaytarır. Bu sətri emal etmək üçün onu özümüz parçalamalıyıq (parse).

**Cookie-ləri Yazmaq və İdarə Etmək**
Yeni bir cookie yazmaq, dəyişmək və ya silmək üçün `document.cookie`-yə xüsusi formatda bir sətir mənimsətməliyik: `ad=dəyər; max-age=saniyə; path=/; secure`.
* **`max-age`**: Cookie-nin neçə saniyə yaşayacağını bildirir. Təyin edilməzsə, brauzer bağlananda silinir.
* **`path`**: Cookie-nin saytın hansı yollarında (paths) əlçatan olacağını bildirir.
* **`secure`**: Təyin edildikdə, cookie yalnız təhlükəsiz `https://` bağlantısı ilə göndərilir.
* **Cookie-ni silmək üçün**, onun `max-age` atributunu `0` olaraq yenidən təyin etmək kifayətdir.

**Geniş Nümunə: `setCookie` və `getCookies` köməkçi funksiyaları**
Aşağıdakı iki funksiya, cookie-lərlə işləməyi bir qədər asanlaşdırır.
```javascript
/**
 * Yeni bir cookie təyin edir.
 * @param {string} name - Cookie-nin adı.
 * @param {string} value - Cookie-nin dəyəri.
 * @param {number} daysToLive - Cookie-nin neçə gün yaşayacağı.
 */
function setCookie(name, value, daysToLive = null) {
  let cookie = `${name}=${encodeURIComponent(value)}`;
  if (daysToLive !== null) {
    // max-age saniyə ilə işlədiyi üçün günləri saniyəyə çeviririk
    cookie += `; max-age=${daysToLive * 60 * 60 * 24}`;
  }
  document.cookie = cookie;
  console.log(`'${name}' adlı cookie təyin edildi.`);
}

/**
 * Bütün cookie-ləri bir Map obyekti kimi qaytarır.
 */
function getCookies() {
  const cookies = new Map();
  const all = document.cookie;
  if (all === "") return cookies;

  const list = all.split("; ");
  for (const cookie of list) {
    const p = cookie.indexOf("=");
    if (p === -1) continue;
    const name = cookie.substring(0, p);
    let value = cookie.substring(p + 1);
    value = decodeURIComponent(value);
    cookies.set(name, value);
  }
  return cookies;
}

// ---- İstifadəsi ----
console.log("Cookie-ləri təyin edirik...");
setCookie("username", "ayan99", 15); // 15 günlük cookie
setCookie("language", "az");       // Sessiya cookie-si

console.log("\nBütün cookie-ləri oxuyuruq:");
const allCookies = getCookies();
console.log("İstifadəçi adı:", allCookies.get("username")); // ✅ "ayan99"

// Cookie-ni silmək üçün max-age=0 veririk
// setCookie("language", "", 0);
```
❗️ **Nəticə:** `localStorage`-dan fərqli olaraq, cookie-lər kiçik həcmlidir (~4KB) və hər HTTP sorğusuna əlavə yük yaradır. Buna görə də, sırf client-side yaddaş üçün `localStorage` daha yaxşı seçimdir.

---
### 15.12.3 IndexedDB: Brauzer Databazası 🗄️
**IndexedDB**, brauzerdə işləyən tamhüquqlu, asinxron (asynchronous), obyekt-yönümlü bir **databazadır**. O, `localStorage`-dan qat-qat güclüdür və böyük həcmli strukturlaşdırılmış datanı saxlamaq, indeksləmək və sorğulamaq üçün nəzərdə tutulub.

**Analogy:** Əgər `localStorage` sadə bir qeyd dəftəridirsə, `IndexedDB` içində axtarış, indeksləmə və çeşidləmə imkanları olan böyük bir **kartoteka şkafıdır** 🗂️.

**Əsas Anlayışlar:**
* **Database (Databaza):** Hər mənbənin (origin) özünəməxsus adları olan bir neçə databazası ola bilər.
* **Object Store (Obyekt Anbarı):** SQL-dəki cədvələ (table) bənzəyir. Obyektləri saxlayır.
* **Key (Açar):** Hər obyektin onu unikal edən bir əsas açarı olmalıdır.
* **Index (İndeks):** Obyektləri əsas açardan başqa, digər xüsusiyyətlərinə görə sürətli axtarmaq üçün istifadə olunur.
* **Transaction (Tranzaksiya):** Bütün databaza əməliyyatları (oxuma, yazma) bir tranzaksiya içində aparılır. Bu, datanın bütövlüyünü təmin edir (ya bütün əməliyyatlar uğurlu olur, ya da heç biri).

❗️ **Mürəkkəb API:** `IndexedDB`-nin API-ı köhnədir və `Promise` və ya `async/await` əvəzinə **hadisələr (events)** üzərində qurulub. Bu, onunla işləməyi bir az "əziyyətli" edir.

**Geniş Nümunə: ABŞ Poçt Kodları (Zip Code) Databazası**
Bu mürəkkəb nümunə, `IndexedDB` ilə databaza yaratmağın, onu doldurmağın və sorğulamağın bütün addımlarını göstərir.

```javascript
// Bu köməkçi funksiya, databaza bağlantısını açır və hazır olduqda callback-i çağırır.
function withDB(callback) {
  // 1. Databazanı adı və versiyası ilə açmağa cəhd edirik
  const request = indexedDB.open("zipcodes", 1);
  request.onerror = console.error; // Xətaları konsola yazdırırıq
  request.onsuccess = () => { // Uğurlu olduqda...
    const db = request.result; // ...databaza obyektini götürürük
    callback(db);             // ...və əsas funksiyamızı çağırırıq
  };
  
  // 2. Əgər databaza mövcud deyilsə və ya versiyası köhnədirsə, bu hadisə işə düşür.
  // Databaza strukturu (object store, index) YALNIZ burada yaradıla bilər.
  request.onupgradeneeded = () => {
    initdb(request.result);
  };
}

// Databazanı ilk dəfə quran funksiya
function initdb(db) {
  // "zipcodes" adlı bir obyekt anbarı (object store) yaradırıq
  const store = db.createObjectStore("zipcodes", { keyPath: "zipcode" });
  // Obyektləri şəhər adına görə də axtarmaq üçün bir indeks (index) yaradırıq
  store.createIndex("cities", "city");
}

// Verilmiş poçt koduna görə şəhəri tapan funksiya
function lookupCity(zip, callback) {
  withDB(db => {
    // 1. Yalnız-oxumaqlı (read-only) bir tranzaksiya başladırıq
    const transaction = db.transaction(["zipcodes"]);
    const store = transaction.objectStore("zipcodes");
    // 2. Verilmiş açara (zip) görə obyekti istəyirik (bu asinxron əməliyyatdır)
    const request = store.get(zip);
    request.onerror = console.error;
    request.onsuccess = () => { // Sorğu bitdikdə...
      const record = request.result; // ...nəticəni götürürük
      if (record) callback(`${record.city}, ${record.state}`);
      else callback(null);
    };
  });
}

// Şəhər adına görə bütün poçt kodlarını tapan funksiya
function lookupZipcodes(city, callback) {
  withDB(db => {
    const transaction = db.transaction(["zipcodes"]);
    const store = transaction.objectStore("zipcodes");
    const index = store.index("cities"); // Bu dəfə indeksdən istifadə edirik
    
    // İndeks üzərindən bütün uyğun gələn qeydləri istəyirik
    const request = index.getAll(city);
    request.onerror = console.error;
    request.onsuccess = () => { callback(request.result); };
  });
}
```
Gördüyünüz kimi, `IndexedDB` çox güclüdür, amma hər bir addım üçün hadisə dinləyiciləri yazmaq API-ı mürəkkəbləşdirir. Xoşbəxtlikdən, bu cür API-ları daha rahat istifadə etmək üçün müasir kitabxanalar (libraries) mövcuddur.

Bro, gəldik Fəsil 15-in son, amma ən vacib və müasir mövzularından birinə. JavaScript-in "tək-axınlı" (single-threaded) olmasının yaratdığı "donma" problemini həll edən super güc: **Veb İşçiləri (Worker Threads)**.

***
### 15.13 Veb İşçiləri (Worker Threads) və Mesajlaşma 🧵
JavaScript-in təməl xüsusiyyəti onun tək-axınlı olmasıdır. Bu o deməkdir ki, o, eyni anda yalnız bir iş görə bilir. Əgər bir funksiya ağır bir hesablama apararsa (məsələn, böyük bir şəkli emal etmək), bütün veb səhifə, o cümlədən istifadəçi interfeysi (UI) həmin əməliyyat bitənə qədər **donur**.

**Web Worker**-lar bu problemi həll etmək üçün yaradılıb. Onlar, əsas proqramdan tamamilə təcrid olunmuş şəkildə **arxa planda (background)** işləyən xüsusi skriptlərdir.

* **Təcrid olunma (Isolation):** Worker-lərin öz qlobal obyektləri var. Onlar `window` və ya `document` obyektlərinə birbaşa müraciət edə **bilməzlər**. Yəni, bir worker birbaşa DOM-u manipulyasiya edə bilməz.
* **Ünsiyyət (Communication):** Onlar əsas proqramla və ya digər worker-lərlə yalnız **asinxron mesajlaşma (asynchronous message passing)** yolu ilə "danışa" bilərlər.

Bu, ağır hesablamaları arxa plana keçirərək, istifadəçi interfeysinin həmişə aktiv və cavabdeh qalmasını təmin edir.

---
### 15.13.1 Worker Obyektləri və 15.13.2 Worker-in Qlobal Obyekti
Worker ilə işləməyin iki tərəfi var:
1.  **Xaricdən:** Əsas skriptimizdən worker-i necə yaratmaq və onunla necə "danışmaq". Bu, `Worker` obyekti ilə edilir.
2.  **Daxildən:** Worker-in öz kodunun içində nə baş verir və o, əsas skriptə necə cavab verir. Bu isə `WorkerGlobalScope` adlı xüsusi bir qlobal mühitdə baş verir.

**Əsas API:**
* **Əsas skriptdə:**
    * `new Worker("worker.js")`: Yeni bir worker yaradır.
    * `worker.postMessage(data)`: Worker-ə məlumat göndərir.
    * `worker.onmessage = (event) => { ... }`: Worker-dən gələn mesajları qəbul edir.
    * `worker.terminate()`: Worker-i məcburi dayandırır.
* **Worker skriptində (`worker.js`):**
    * `self`: Worker-in qlobal obyektinə istinaddır (`window` əvəzinə).
    * `self.postMessage(data)`: Əsas skriptə məlumat göndərir.
    * `self.onmessage = (event) => { ... }`: Əsas skriptdən gələn mesajları qəbul edir.

**Geniş Nümunə: Ağır Hesablamanı Worker-ə Ötürmək**
Bu nümunədə, biz sadə ədədləri tapan ağır bir funksiyanı worker-ə ötürəcəyik ki, UI donmasın.

**`main.js` (Əsas Proqram Kodu)**
```javascript
console.log("Əsas proqram başladı.");

// 1. Worker faylımızın yolunu göstərərək yeni bir worker yaradırıq.
const primeWorker = new Worker('prime-worker.js');

// 2. Worker-dən mesaj gəldikdə bu funksiya işə düşəcək.
primeWorker.onmessage = (event) => {
  const primes = event.data;
  console.log(`✅ Worker işini bitirdi. Tapılan sadə ədədlərin sayı: ${primes.length}`);
  // Nəticəni ekranda göstərə bilərik.
  // document.querySelector('#result').textContent = `Nəticə: ${primes.length} ədəd tapıldı.`;
};

// 3. Worker-ə tapşırığı göndəririk.
console.log("Worker-ə tapşırıq göndərilir: 10,000,000-a qədər sadə ədədləri tap.");
// Bu əməliyyat normalda brauzeri bir neçə saniyə dondurardı.
primeWorker.postMessage(10000000); 

console.log("Worker-ə tapşırıq göndərildi. Əsas proqram öz işinə davam edir və donmur!");
```

**`prime-worker.js` (Worker Faylı)**
```javascript
console.log("Worker skripti yükləndi və işə hazırır.");

// Sadə ədədləri tapan bir funksiya
function findPrimes(max) {
    const primes = [];
    for (let i = 2; i <= max; i++) {
        let isPrime = true;
        for (let j = 2; j <= Math.sqrt(i); j++) {
            if (i % j === 0) {
                isPrime = false;
                break;
            }
        }
        if (isPrime) {
            primes.push(i);
        }
    }
    return primes;
}

// 4. Əsas proqramdan mesaj gəldikdə bu funksiya işə düşür.
self.onmessage = (event) => {
  const maxNumber = event.data;
  console.log(`Worker tapşırığı qəbul etdi: ${maxNumber}`);

  // 5. Ağır hesablamanı edirik.
  const result = findPrimes(maxNumber);
  
  // 6. Nəticəni əsas proqrama geri göndəririk.
  console.log("Hesablama bitdi, nəticə geri göndərilir.");
  self.postMessage(result);
  
  // İş bitdiyi üçün worker-i bağlayırıq.
  self.close();
};
```
---
### 15.13.3 Worker-ə Kod Import Etmək (`importScripts()`)
Worker-lər HTML sənədindən təcrid olunduğu üçün, `<script>` teqlərini görə bilmir. Onların özünəməxsus, **sinxron (synchronous)** işləyən bir kod import etmə mexanizmi var: `importScripts()`.

```javascript
// util.js faylı
// function multiply(a, b) { return a * b; }

// worker.js faylı
importScripts('util.js'); // util.js faylının içindəki bütün kodu import edir

console.log(multiply(5, 10)); // ✅ Nəticə: 50
```
**Qeyd:** Müasir worker-lər artıq ES6 modullarını (`import`/`export`) da dəstəkləyir. Bunun üçün worker yaradarkən `new Worker("worker.js", { type: "module" })` yazmaq lazımdır. Bu, `importScripts`-dən daha müasir bir yoldur.

---
### 15.13.4 Worker-in İcra Modeli və Xətaları 🚨
* **İcra:** Worker öz skriptini yuxarıdan aşağıya icra edir, sonra isə eynilə əsas proqram kimi hadisə gözləmə rejiminə keçir (`onmessage`). Əgər gözləyəcəyi heç bir hadisə və ya timer qalmayıbsa, avtomatik olaraq sonlanır. Worker, həmçinin `self.close()` ilə özünü sonlandıra bilər.
* **Xətalar:** Worker-in içində tutulmayan bir xəta baş verərsə, bu, əsas proqramı "çökdürmür". Bunun əvəzinə, həm worker-in özündə (`self.onerror`), həm də əsas proqramdakı worker obyektində (`worker.onerror`) bir `"error"` hadisəsi baş verir. Bu hadisələri dinləyərək xətaları tutmaq mümkündür.

---
### 15.13.5/6 `postMessage` ilə Qabaqcıl Mesajlaşma 📨
`postMessage` metodu təkcə worker ilə əsas proqram arasında deyil, həm də pəncərələr, `<iframe>`-lər və hətta worker-lər arasında birbaşa əlaqə qurmaq üçün də istifadə olunur.

* **`MessageChannel`**: İki tərəf arasında birbaşa, özəl bir "rabitə kanalı" yaratmağa imkan verir. Bu, xüsusilə bir neçə worker-in bir-biri ilə danışması üçün faydalıdır.
* **`<iframe>`-lər arası Mesajlaşma**: Təhlükəsizlik səbəbindən, fərqli mənbələrdən (origins) olan `<iframe>`-lər bir-birinin məzmununa baxa bilməz. Amma `window.postMessage(message, targetOrigin)` metodu onlara bir-birinə təhlükəsiz şəkildə mesaj göndərməyə imkan verir. `targetOrigin` arqumenti, mesajın yalnız gözlənilən mənbəyə göndərilməsini təmin edir.
* **Transferable Objects**: `postMessage` ilə böyük dataları (xüsusilə `ArrayBuffer`) göndərərkən, onu kopyalamaq əvəzinə **ötürmək (transfer)** olar. Bu, yaddaşa və zamana böyük qənaət edir. Ötürülən obyekt orijinal mühitdə artıq istifadə edilə bilməz hala gəlir.

---

15.14 ve 15.5 sonradan izah olunacaq

---