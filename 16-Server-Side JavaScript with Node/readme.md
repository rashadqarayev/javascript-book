### Fəsil 16. Node ilə Server-tərəfi JavaScript ⚙️
**Node.js** (və ya qısaca Node), JavaScript-i götürüb, ona əməliyyat sisteminin özü ilə birbaşa işləmək gücü verən bir mühitdir (runtime environment). Node ilə, brauzerin təhlükəsizlik məhdudiyyətləri olmadan, faylları oxuyub-yazan, şəbəkə bağlantıları quran və digər sistem proseslərini idarə edən proqramlar yaza bilərik.

Node.js əsasən bu məqsədlər üçün istifadə olunur:
* **Müasir Shell Skriptləri:** `bash` kimi köhnə shell dillərinin mürəkkəb sintaksisi olmadan, güclü sistem skriptləri yazmaq üçün.
* **Ümumi Məqsədli Proqramlaşdırma:** Tam hüquqlu, etibarlı proqramlar (məsələn, command-line tools) yaratmaq üçün.
* **Veb Serverlər:** Yüksək performanslı, eyni anda minlərlə istifadəçiyə xidmət edə bilən asinxron (asynchronous) veb serverlər yazmaq üçün ən populyar mühitlərdən biridir.

Bu fəsil, Node-un təməl prinsiplərini və ən vacib API-larını öyrədərək, səni bu platformada məhsuldar bir proqramçı etmək üçün möhkəm bir təməl verəcək.

**Quraşdırma:** Node.js-i rəsmi saytı olan [https://nodejs.org](https://nodejs.org) ünvanından asanlıqla yükləyib quraşdıra bilərsiniz. Quraşdırma zamanı, Node ilə birlikdə **`npm` (Node Package Manager)** adlı paket meneceri də avtomatik olaraq quraşdırılır.

---
### 16.1 Node Proqramlaşdırmanın Əsasları
Gəlin Node proqramlarının necə qurulduğuna və işlədiyinə baxaq.

#### 16.1.1 Konsol Çıxışı (Console Output)
Brauzerdən fərqli olaraq, Node-da `console.log()` təkcə sazlama (debug) üçün deyil, proqramın istifadəçiyə nəticə göstərməsi üçün əsas vasitədir.
* **`console.log()`**: Məlumatı standart çıxışa (**stdout**) yazır.
* **`console.error()`**: Məlumatı standart xəta çıxışına (**stderr**) yazır.

Bu fərq, proqramın çıxışını bir fayla yönləndirdikdə vacib olur. Məsələn, terminalda `node myapp.js > output.log` yazsanız, yalnız `console.log()` ilə yazılanlar `output.log` faylına düşəcək, `console.error()` ilə yazılan xətalar isə hələ də terminalda görünəcək.

#### 16.1.2 Əmr Sətri Arqumentləri və Mühit Dəyişənləri
Node proqramları, xaricdən məlumatı iki əsas yolla qəbul edir:

* **`process.argv`**: Proqramı işə salarkən verilən əmr sətri (command-line) arqumentlərini saxlayan bir massivdir (array).
    * `[0]`: Həmişə Node proqramının özünə gedən yol.
    * `[1]`: Həmişə icra olunan JavaScript faylına gedən yol.
    * `[2]` və sonrası: Bizim ötürdüyümüz əsl arqumentlər.

**Nümunə (`args.js`):**
```javascript
// İlk iki elementi kəsərək, yalnız bizim verdiyimiz arqumentləri götürürük
const myArgs = process.argv.slice(2);
console.log("Mənim arqumentlərim:", myArgs);
```
**Terminalda icra etsək:**
`$ node args.js istifadechi --level=admin --status active`
**Çıxış:**
`Mənim arqumentlərim: [ 'istifadechi', '--level=admin', '--status', 'active' ]`

* **`process.env`**: Sistemin **mühit dəyişənlərini (environment variables)** saxlayan bir obyektdir. Bu, parollar, API açarları kimi həssas məlumatları kodun içinə yazmamaq üçün çox faydalıdır.

**Nümunə (`env.js`):**
```javascript
// `USER` mühit dəyişənini oxuyuruq, əgər yoxdursa standart dəyər olaraq "Qonaq" istifadə edirik
const username = process.env.USER || 'Qonaq';
console.log(`Salam, ${username}!`);
```
**Terminalda icra etsək:**
`$ node env.js` -> `Salam, (sizin kompüter adınız)!`
`$ USER=Ayan node env.js` -> `Salam, Ayan!`

#### 16.1.3 Proqramın Həyat Dövrü (Life Cycle)
Node proqramı, skript faylındakı bütün sinxron kodu icra etdikdən sonra dərhal dayanmır. O, arxa planda hələ də davam edən asinxron əməliyyatlar (məsələn, şəbəkə sorğusu, `setTimeout`) və ya hadisə dinləyiciləri olduğu müddətcə işləməyə davam edir. Bütün işlər bitdikdə, proqram avtomatik olaraq sonlanır.
* **`process.exit()`**: Proqramı məcburi sonlandırmaq üçün istifadə olunur.
* **Tutulmayan xətalar:** Əgər proqramda tutulmayan bir xəta (`uncaught exception`) və ya `Promise` rədd edilməsi (`unhandled rejection`) baş verərsə, Node standart olaraq xətanı göstərir və proqramı dayandırır. Bunun qarşısını almaq üçün qlobal handler-lər təyin etmək olar: `process.on('uncaughtException', ...)` və `process.on('unhandledRejection', ...)`.

#### 16.1.4 Node Modulları (Node Modules) 📦
JavaScript-ə rəsmi modul sistemi (ES6 Modules) gəlməmişdən çox əvvəl, Node öz modul sistemini yaratmışdı: **CommonJS**.
* **CommonJS Modulları:** `require()` funksiyası ilə başqa modulları import edir və `module.exports` obyekti ilə öz dəyərlərini export edir. `.js` və `.cjs` fayl uzantıları ilə işləyir.
* **ES6 Modulları:** Müasir `import` və `export` sintaksisini istifadə edir. Node-da bu modulları istifadə etmək üçün ya fayl uzantısı `.mjs` olmalı, ya da layihənin `package.json` faylında `"type": "module"` təyin olunmalıdır.

**Nümunə (CommonJS):**
**`math.js` faylı:**
```javascript
function add(a, b) { return a + b; }
const PI = 3.14159;

// Bu fayldan `add` funksiyasını və `PI` sabitini export edirik
module.exports = { add, PI };
```
**`app.js` faylı:**
```javascript
// `math.js` modulunu `require` ilə import edirik
const myMath = require('./math.js');

const sum = myMath.add(5, 10);
console.log(`Cəm: ${sum}, Pi: ${myMath.PI}`); // ✅ Nəticə: Cəm: 15, Pi: 3.14159
```
#### 16.1.5 Node Paket Meneceri (NPM)
`npm`, Node.js ilə birlikdə gələn və dünyanın ən böyük proqram təminatı reyestrinə giriş imkanı verən bir paket meneceridir. O, sizə başqaları tərəfindən yazılmış minlərlə hazır kitabxananı (packages) asanlıqla layihənizə əlavə etməyə imkan verir.
* **`package.json`**: Hər bir Node layihəsinin "pasportu"dur. Layihə haqqında məlumatları və onun asılı olduğu bütün paketlərin siyahısını saxlayır.
* **`npm init`**: Yeni bir `package.json` faylı yaratmaq üçün istifadə olunan əmr.
* **`npm install <paket_adı>`**: Yeni bir paket yükləmək və onu `package.json`-a əlavə etmək üçün. Məsələn: `npm install express`.
* **`npm install`**: `package.json`-da siyahısı olan bütün asılılıqları avtomatik olaraq yükləmək üçün.

***
### 16.2 Node-un Əsas Prinsipi: Standart Olaraq Asinxronluq (Asynchronous by Default) ⚡
Node.js, eyni anda minlərlə istifadəçiyə xidmət edə bilən yüksək performanslı veb serverlər kimi, **G/Ç intensiv (I/O intensive)** proqramlar üçün dizayn edilib və optimallaşdırılıb.

Node, bu yüksək performansa və eynivaxtlılığa (concurrency) çox-axınlılıq (multithreading) ilə deyil, brauzerlərdə olduğu kimi, **tək-axınlı (single-threaded)** və **hadisə-yönümlü (event-driven)** bir model ilə nail olur.

**"Single-Threaded Server" Necə Yüksək Performanslı Olur?** 🤔
**Analogy:** Təsəvvür et ki, bir restoranda (server) 100 müştəri (client) var. 100 ofisiant (thread) tutmaq həm baha başa gələr, həm də qarışıqlıq yaradar. Node-un modeli isə **super-sürətli tək bir ofisiantdır**.
O, bir müştərinin sifarişini alıb dərhal mətbəxə ötürür (**asinxron əməliyyatı başladır**) və həmin yeməyin bişməsini gözləmədən, dərhal növbəti müştərinin sifarişini almağa gedir (**başqa bir sorğunu emal edir**). Mətbəxdən yemək hazır olduqda, ofisianta siqnal verilir (**event**) və o, yeməyi müvafiq masaya aparır (**callback-i icra edir**).

Bu model, ofisiantın (əsas axının) heç vaxt boş dayanmamasını və daima məşğul olmasını təmin edir.

#### Bloklamayan (Non-blocking) API
Node bu yanaşmanı o qədər ciddiyə alır ki, təkcə şəbəkə sorğuları deyil, hətta lokal fayl sistemindən fayl oxumaq (`fs.readFile`) kimi əməliyyatlar belə standart olaraq **asinxrondur**. Çünki müasir sistemlərdə "lokal" fayl belə, əslində şəbəkə üzərindəki bir diskdə yerləşə bilər və bu da gecikmələrə səbəb olur. Node, bu kiçik gecikmələrin belə proqramı "dondurmasının" qarşısını alır.

#### Callback-lər və "Error-First" Qaydası
Node, `Promise`-lərdən əvvəl yaradıldığı üçün, onun əsas asinxron API-ları **callback** funksiyaları üzərində qurulub. Bu callback-lər, demək olar ki, həmişə **"error-first" (öncə xəta)** prinsipi ilə işləyir.

Yəni, callback funksiyası adətən iki arqument qəbul edir: `function(err, data)`.
* **Qızıl Qayda:** **Həmişə** birinci `err` arqumentini yoxlayın! Əgər o, `null` deyilsə, deməli xəta baş verib.
* Əgər `err` `null`-dırsa, deməli əməliyyat uğurludur və nəticə `data` arqumentindədir.

---
### Geniş Nümunə: Konfiqurasiya Faylının Oxunmasının 4 Yolu
Gəlin eyni məsələnin – bir konfiqurasiya faylını oxuyub JSON kimi emal etmənin – Node-da fərqli asinxron yanaşmalarla necə həll olunduğuna baxaq.

**1. Callback Üsulu (Klassik Node)**
```javascript
const fs = require("fs");

function readConfigWithCallback(path, callback) {
  fs.readFile(path, "utf-8", (err, text) => {
    if (err) { // Əvvəlcə həmişə xətanı yoxlayırıq!
      console.error("Fayl oxunarkən xəta baş verdi:", err);
      callback(null);
      return;
    }
    
    try {
      const data = JSON.parse(text);
      callback(data);
    } catch (e) {
      console.error("JSON parse xətası:", e);
      callback(null);
    }
  });
}
```

**2. `Promise` Üsulu (`util.promisify` ilə)**
Node-un `util` modulu, köhnə "error-first" callback funksiyalarını avtomatik olaraq `Promise` qaytaran funksiyalara çevirən `promisify` adlı bir köməkçi təqdim edir.
```javascript
const fs = require("fs");
const util = require("util");

const readFilePromise = util.promisify(fs.readFile); // fs.readFile-ı "promisify" edirik

function readConfigWithPromise(path) {
  return readFilePromise(path, "utf-8")
    .then(text => JSON.parse(text));
}
```

**3. `async/await` Üsulu (Müasir Standart)**
`Promise`-lərimiz olduqdan sonra, ən təmiz və oxunaqlı yol `async/await`-dir.
```javascript
const fs = require("fs").promises; // Node 10+ versiyalarında .promises ilə hazır Promise versiyaları gəlir

async function readConfigWithAsync(path) {
  const text = await fs.readFile(path, "utf-8");
  return JSON.parse(text);
}
```

**4. Sinxron (Synchronous) Üsul (`...Sync`)**
Node, rahatlıq üçün bəzi funksiyaların bloklayan, yəni sinxron versiyalarını da təqdim edir. Onların adları adətən **`Sync`** ilə bitir (`readFileSync`, `writeFileSync`). Bu funksiyalar proqramın icrasını dayandırır və nəticəni birbaşa `return` edir.

❗️ **İstifadə yeri:** Bu funksiyalardan yalnız proqram ilk dəfə işə düşərkən, konfiqurasiya fayllarını oxumaq kimi birdəfəlik əməliyyatlarda istifadə etmək olar. Server sorğuları emal etməyə başladıqdan sonra onlardan istifadə etmək, performansı kəskin şəkildə aşağı salır.

```javascript
const fs = require("fs");

function readConfigWithSync(path) {
  const text = fs.readFileSync(path, "utf-8");
  return JSON.parse(text);
}
```
Bu dörd nümunə, Node.js-də asinxronluğun təkamülünü və fərqli yanaşmalarını mükəmməl şəkildə göstərir.

***
### 16.3 Buferlər (Buffers) 📦
Brauzerdə daha çox mətnlərlə (strings) işləsək də, Node.js-də faylları oxuyarkən, şəbəkədən data qəbul edərkən və ya ümumiyyətlə, binary (ikilik) data ilə işləyərkən tez-tez **`Buffer`** sinifi (class) ilə qarşılaşacağıq.

**Buffer nədir?** 🤔
* **Analogy:** Əgər sətir (string) oxuduğumuz bir **kitabdırsa**, `Buffer` həmin kitabın çap olunduğu **kağız və mürəkkəbdir**. O, "A" hərfi və ya "Salam" sözü kimi xarakterləri yox, onları təşkil edən xam **baytları (bytes)** saxlayır.
* **Texniki olaraq:** `Buffer` sinifi, müasir JavaScript-dəki `Uint8Array`-in bir alt-sinifidir. Yəni o, dəyişdirilə bilən (mutable) bir bayt massividir.
* **Əsas Fərqi:** `Buffer`-i `Uint8Array`-dən fərqləndirən əsas cəhət, onun sətirlərlə (strings) çox rahat işləmək üçün xüsusi metodlara sahib olmasıdır. O, sətirləri baytlara **kodlaşdıra (encode)** və baytları yenidən sətirə **dekodlaşdıra (decode)** bilir.

#### Simvol Kodlaşdırmaları (Character Encodings)
Sətir və baytlar arasında çevirmə aparmaq üçün, hansı qaydaya əsasən bir xarakterin hansı bayt ardıcıllığına uyğun gəldiyini bilmək lazımdır. Bu qaydaya **kodlaşdırma (encoding)** deyilir. Node-un `Buffer`-i aşağıdakı əsas kodlaşdırmaları dəstəkləyir:
* **`"utf8"`**: Standart (default) və ən çox istifadə olunan Unicode kodlaşdırması. Azərbaycan hərfləri daxil, bütün dilləri dəstəkləyir.
* **`"ascii"`**: Yalnız 7-bitlik ingilis əlifbası simvollarını saxlayır.
* **`"hex"`**: Hər bir baytı iki simvollu onaltılıq (hexadecimal) bir sətirə çevirir.
* **`"base64"`**: Binary datanı təhlükəsiz şəkildə mətn formatında ötürmək üçün istifadə olunan bir kodlaşdırma.

---
### `Buffer` ilə İşləmək: Geniş Nümunə
Aşağıdakı nümunə, `Buffer` ilə edilə bilən əsas əməliyyatları göstərir.

```javascript
// --- 1. Buffer Yaratmaq ---

// a) Sətirdən (string) buffer yaratmaq
// "Salam" sözünü UTF-8 kodlaşdırması ilə baytlara çeviririk
const bufFromString = Buffer.from("Salam", "utf8");
console.log("Sətirdən buffer:", bufFromString);
// ✅ Nəticə: <Buffer 53 61 6c 61 6d> (hər rəqəm bir hərfin kodudur)

// b) Bayt massivindən (array of bytes) buffer yaratmaq
// "JS" sözünü ASCII/UTF8 kodları ilə yaradırıq (J=74, S=83)
const bufFromBytes = Buffer.from([74, 83]);
console.log("Baytlardan buffer:", bufFromBytes);
// ✅ Nəticə: <Buffer 4a 53> (hex formatında)

// c) Boş və ya doldurulmuş buffer yaratmaq (Buffer.alloc)
const emptyBuf = Buffer.alloc(10); // 10 baytlıq, içi tamamilə sıfırlarla dolu buffer
const filledBuf = Buffer.alloc(5, "A"); // 5 baytlıq, hamısı "A" hərfi ilə dolu buffer
console.log("Boş buffer:", emptyBuf); // ✅ <Buffer 00 00 00 00 00 00 00 00 00 00>
console.log("Doldurulmuş buffer:", filledBuf); // ✅ <Buffer 41 41 41 41 41>


// --- 2. Buffer-i Oxumaq ---

// a) Buffer-i yenidən sətirə çevirmək (.toString)
console.log("bufFromBytes-dan sətir:", bufFromBytes.toString("utf8")); // ✅ "JS"
// b) Buffer-i fərqli formatda göstərmək
console.log("Salam buffer-i 'hex' formatında:", bufFromString.toString("hex")); // ✅ "53616c616d"


// --- 3. Buffer-i Dəyişmək (Mutability) ---
// Buffer-lər dəyişdirilə biləndir, yəni içindəki baytları bir-bir dəyişə bilərik
const myBuffer = Buffer.from("TEST");
console.log("Orijinal buffer:", myBuffer.toString()); // ✅ "TEST"
myBuffer[0] = 66; // T(84) hərfinin kodunu B(66) ilə əvəz edirik
myBuffer[1] = 69; // E
myBuffer[2] = 83; // S
myBuffer[3] = 84; // T
console.log("Dəyişdirilmiş buffer:", myBuffer.toString()); // ✅ "BEST"


// --- 4. Mürəkkəb Oxuma Əməliyyatları ---
// Buffer-dən tək bir bayt deyil, bütöv bir rəqəm (məsələn, 32-bitlik) də oxumaq olar.
const complexBuf = Buffer.from([0xDE, 0xAD, 0xBE, 0xEF, 0xCA, 0xFE]);
// İlk 4 baytı 32-bitlik rəqəm kimi oxumaq (Big-Endian formatında)
const num = complexBuf.readUInt32BE(0);
console.log("Oxunan rəqəm (hex):", num.toString(16).toUpperCase()); // ✅ "DEADBEEF"
```
**Praktikada:** Çox vaxt Node-un öz API-ları (məsələn, fayl oxuyarkən) buferlərlə işi arxa planda özü həll edir. Faylı oxuyarkən sadəcə `"utf8"` kimi bir kodlaşdırma təyin etməyiniz kifayətdir ki, Node sizə birbaşa sətir (string) qaytarsın. Ancaq xam (raw) binary data ilə işləmək lazım gəldikdə, `Buffer` sizin əsas alətiniz olacaq.

***
### 16.4 Hadisələr (Events) və `EventEmitter` 📢
Node.js-də, bir əməliyyatın birdən çox mərhələsi olduqda və ya zamanla təkrar-təkrar bir nəticə qaytardıqda (məsələn, bir serverin yeni bağlantıları qəbul etməsi), sadə bir callback funksiyası kifayət etmir.

Belə hallar üçün Node.js **hadisə-yönümlü (event-based)** bir API təqdim edir. Bu modeldə, müəyyən bir hadisə baş verdikdə "siqnal" göndərən obyektlər var. Bu obyektlərin hamısı **`EventEmitter`** sinifindən (class) törəyir. `net.Server`, `fs.ReadStream`, `http.Request` kimi bir çox daxili (built-in) Node sinifləri bir `EventEmitter`-dir.

#### `EventEmitter` ilə İşləmək
Bu siniflə işləmək üçün əsasən 3 metoddan istifadə edirik:
* **`.on(eventName, listener)`**: Bir hadisəni "dinləmək" üçün istifadə olunur. `eventName` baş verdikdə, `listener` funksiyası (callback) çağırılır.
* **`.once(eventName, listener)`**: `.on` kimidir, lakin `listener` yalnız **bir dəfə** çağırıldıqdan sonra avtomatik olaraq sıradan çıxarılır.
* **`.emit(eventName, ...args)`**: Bir hadisəni "atəşləmək", yəni siqnalı göndərmək üçün istifadə olunur. `...args` ilə `listener` funksiyalarına istənilən sayda arqument ötürmək olar.

---
### Geniş Nümunə: Fərdi `Downloader` Sinifi Yaratmaq 📥
Gəlin `EventEmitter`-dən törəyən öz sinifimizi yaradaq. Bu `Downloader` sinifi, bir faylın yüklənməsini simulyasiya edəcək və prosesin fərqli mərhələlərində (`start`, `progress`, `end`) hadisələr göndərəcək.

**`downloader.js` faylı:**
```javascript
// Əvvəlcə, `events` modulundan EventEmitter sinifini import edirik
const EventEmitter = require('events');

// Öz sinifimizi EventEmitter-dən törədirik
class Downloader extends EventEmitter {
  constructor(url) {
    super(); // `super()` çağırmaq mütləqdir!
    this.url = url;
  }
  
  // Yükləmə prosesini başladan metod
  download() {
    console.log(`"${this.url}" üçün yükləmə prosesi başlayır...`);
    // 1. "start" hadisəsini göndəririk (emit edirik) və ona URL-i arqument kimi ötürürük
    this.emit('start', this.url);
    
    let progress = 0;
    const interval = setInterval(() => {
      progress += 20;
      if (progress < 100) {
        // 2. Hər 500ms-dən bir, irəliləyiş haqqında "progress" hadisəsi göndəririk
        this.emit('progress', progress);
      } else {
        // 3. Yükləmə 100% olduqda, intervalı dayandırıb "end" hadisəsini göndəririk
        clearInterval(interval);
        this.emit('end', "Fayl uğurla yükləndi!");
      }
    }, 500);
  }
}

// Sinifimizi başqa faylların istifadə etməsi üçün export edirik
module.exports = Downloader;
```

**`app.js` faylı (istifadəsi):**
```javascript
const Downloader = require('./downloader.js');

// Downloader-in yeni bir nüsxəsini (instance) yaradırıq
const myDownloader = new Downloader("file.zip");

// İndi isə onun göndərəcəyi hadisələrə "qulaq asırıq" (.on metodu ilə)

myDownloader.on('start', (url) => {
  console.log(`➡️ Proses başladı: ${url}`);
});

myDownloader.on('progress', (percent) => {
  // Gələn faiz dəyərini istifadə edərək bir proqres çubuğu göstərə bilərik
  console.log(`... ${percent}% yükləndi`);
});

myDownloader.on('end', (message) => {
  console.log(`✅ Proses bitdi: ${message}`);
});

// Nəhayət, yükləmə prosesini başladaq
myDownloader.download();
```

---
#### ❗️ Vacib Qeydlər və "Gotcha"-lar
**1. Hadisələr Sinxron (Synchronous) Çağırılır**
`.emit()` metodu, qeydiyyatdan keçmiş bütün `listener`-ləri **dərhal və sinxron** olaraq çağırır. O, növbəyə qoymur. Bu o deməkdir ki, əgər sizin hadisə işləyiciniz (listener) uzun çəkən bir əməliyyat aparsa, o, bütün **event loop**-u bloklayacaq. Buna görə də, `listener`-ləri həmişə sürətli və qısa saxlamaq lazımdır.

**2. Xüsusi `"error"` Hadisəsi**
`EventEmitter`-də `"error"` adlı hadisənin xüsusi bir rolu var.
**Qızıl Qayda:** Əgər bir `EventEmitter` `"error"` hadisəsi göndərərsə və həmin anda bu hadisəyə "qulaq asan" heç bir `.on('error', ...)` listener-i yoxdursa, Node.js bunu tutulmamış bir xəta (uncaught exception) kimi qəbul edir və **proqramı dərhal çökdürür (crashes)**.

Buna görə də, xəta göndərmə ehtimalı olan hər bir `EventEmitter` üçün **həmişə** bir xəta işləyicisi təyin etmək çox vacibdir.

**Nümunə:**
```javascript
const myEmitter = new EventEmitter();

// BU SƏTRİ KOMMENTƏ ALIB KODU İŞƏ SALSANIZ, PROQRAM ÇÖKƏCƏK!
myEmitter.on('error', (err) => {
  console.error("AMAN, BİR XƏTA TUTULDU:", err.message);
});

// İndi isə xəta hadisəsini göndəririk
myEmitter.emit('error', new Error("Bumm! Gözlənilməz bir problem."));
```
Bu qayda, proqramda potensial xətaların nəzərdən qaçmasının qarşısını almaq üçün nəzərdə tutulub.

***
### 16.5 Axınlar (Streams) 🌊
Bir alqoritm yazarkən, adətən ən asan yol bütün datanı yaddaşa oxumaq, onu emal etmək və sonra nəticəni yazdırmaqdır. Məsələn, böyük bir faylı köçürmək üçün `fs.readFile()` ilə onu tam oxuyub, sonra `fs.writeFile()` ilə yaza bilərik. Lakin bu yanaşmanın böyük bir problemi var: əgər fayl 1GB-dırsa, sizin proqramınız da 1GB RAM istifadə etməli olacaq! Bu, çox səmərəsizdir və böyük fayllarda proqramın "çökməsinə" səbəb ola bilər.

**Axınlar (Streams)** bu problemi həll edir.
**Analogy:** Təsəvvür et ki, böyük bir su çənindən (fayl) otağındakı qaba su daşımaq istəyirsən.
* **Köhnə üsul (`readFile`):** Bütün çəni bir dəfəyə qaldırıb daşımaq. Bu, çox ağırdır və mümkünsüz ola bilər.
* **Axın (Stream) üsulu:** Bu prosesi bir **şlanq** vasitəsilə etmək. Su, şlanqdan hissə-hissə (**chunks**), davamlı bir axınla gəlir, sən də onu gəldikcə emal edirsən. Bütün suyu eyni anda yaddaşda saxlamağa ehtiyac qalmır.

Node.js-də şəbəkə və fayl sistemi API-larının əksəriyyəti axınlar üzərində qurulub. Axınların 4 əsas növü var:

* **`Readable` (Oxunaqlı) 📥**: Datanın gəldiyi mənbədir. Məsələn, `fs.createReadStream()` (fayldan oxumaq üçün) və ya `process.stdin` (klaviaturadan daxil edilən məlumat).
* **`Writable` (Yazıla bilən) 📤**: Datanın göndərildiyi hədəfdir. Məsələn, `fs.createWriteStream()` (fayla yazmaq üçün) və ya `process.stdout` (konsola çıxış).
* **`Duplex` (İki tərəfli) ↔️**: Həm oxunaqlı, həm də yazıla biləndir. Ən yaxşı nümunə, şəbəkə `Socket`-idir.
* **`Transform` (Çevirici) 🔄**: Xüsusi bir `Duplex` növüdür. Ona yazılan datanı emal edib (məsələn, sıxışdırıb və ya şifrələyib), çevrilmiş halda oxumaq üçün təqdim edir. `zlib.createGzip()` (datanı sıxışdırmaq üçün) buna bir nümunədir.

Axınlarla işləməyin əsas çətinliyi, datanın gəlmə sürəti ilə emal olunma sürətini sinxronlaşdırmaqdır. Buna **"əks təzyiq" (backpressure)** deyilir. Xoşbəxtlikdən, Node.js-in `.pipe()` metodu bu mürəkkəb işi bizim üçün avtomatik həll edir.

---
### 16.5.1 "Boru"lar (Pipes) ቧ
`.pipe()` metodu, bir `Readable` axının çıxışını bir `Writable` axının girişinə "bağlamağın" ən asan yoludur. O, datanın bir axından digərinə səmərəli şəkildə ötürülməsini, əks təzyiqin (backpressure) və xətaların idarə olunmasını tamamilə öz üzərinə götürür.

**Geniş Nümunə 1: Faylın səmərəli kopyalanması**
`readFile`/`writeFile` əvəzinə, eyni işi axınlarla belə edərik:
```javascript
const fs = require('fs');

console.log("Fayl kopyalanır...");

// Oxumaq üçün bir Readable stream yaradırıq
const readableStream = fs.createReadStream('boyuk-fayl.txt');
// Yazmaq üçün bir Writable stream yaradırıq
const writableStream = fs.createWriteStream('kopyalanmish-fayl.txt');

// İndi isə sadəcə onları bir-birinə "bağlayırıq"
readableStream.pipe(writableStream);

// Prosesin bitdiyini bilmək üçün hadisəni dinləyirik
writableStream.on('finish', () => {
  console.log('✅ Kopyalama uğurla bitdi!');
});

readableStream.on('error', (err) => console.error("Oxuma xətası:", err));
writableStream.on('error', (err) => console.error("Yazma xətası:", err));
```

**Geniş Nümunə 2: Boru Kəməri (Pipeline) və Transform Axınları**
Siz bir neçə axını zəncirvari şəkildə bir-birinə bağlayaraq bir "boru kəməri" (pipeline) yarada bilərsiniz. Məsələn, bir faylı oxuyub, onu `gzip` ilə sıxışdırıb, sonra başqa bir fayla yazmaq.

`Fayl (Readable) ➡️ Gzip (Transform) ➡️ Yeni Fayl (Writable)`
```javascript
const fs = require('fs');
const zlib = require('zlib'); // Node-un daxili sıxışdırma modulu

const source = fs.createReadStream('boyuk-fayl.txt');
const gzipper = zlib.createGzip();
const destination = fs.createWriteStream('boyuk-fayl.txt.gz');

console.log("Fayl sıxışdırılır...");

// Axınları bir-birinə bağlayırıq
source.pipe(gzipper).pipe(destination);

destination.on('finish', () => console.log('✅ Fayl uğurla sıxışdırıldı!'));
```
**Geniş Nümunə 3: Fərdi Transform Axını Yaratmaq (`GrepStream`)**
Bu, axınların əsl gücünü göstərən mürəkkəb bir nümunədir. Unix-dəki `grep` əmri kimi işləyən, gələn mətn axınından yalnız müəyyən bir requlyar ifadəyə uyğun gələn sətirləri buraxan bir `Transform` axını yaradacağıq.

```javascript
const { Transform } = require('stream'); // `stream` modulundan Transform sinifini götürürük

class GrepStream extends Transform {
  constructor(pattern) {
    super({ decodeStrings: false }); // Daxil olan datanı avtomatik string-ə çevirməməsini deyirik
    this.pattern = pattern;
    this.incompleteLine = ""; // Hissə-hissə gələn natamam sətirləri yadda saxlamaq üçün
  }
  
  // Bu metod, hər dəfə yeni bir data "hissəsi" (chunk) gəldikdə çağırılır
  _transform(chunk, encoding, callback) {
    const lines = (this.incompleteLine + chunk).split('\n');
    this.incompleteLine = lines.pop(); // Sonuncu, natamam ola biləcək sətri saxlayırıq

    const output = lines
      .filter(line => this.pattern.test(line)) // Yalnız pattern-ə uyğun gələnləri filterləyirik
      .join('\n');
    
    // Nəticəni növbəti axına ötürürük
    callback(null, output ? output + '\n' : '');
  }

  // Bütün data gəlib bitdikdən sonra bu metod çağırılır
  _flush(callback) {
    // Ən sonda qalan natamam sətri də yoxlayırıq
    if (this.pattern.test(this.incompleteLine)) {
      callback(null, this.incompleteLine + '\n');
    } else {
      callback(); // Heç nə yoxdursa, boş callback çağırırıq
    }
  }
}

// ---- İSTİFADƏSİ ----
// Bu skripti `grep.js` adı ilə saxlayın.
// Terminaldan belə istifadə edin: node grep.js "aranan_soz" < input.txt > output.txt

const pattern = new RegExp(process.argv[2]); // Axtarılacaq sözü command-line-dan götürürük

// Standart girişi (stdin) oxuyuruq, onu Grep axınından keçiririk və nəticəni standart çıxışa (stdout) yazırıq
process.stdin
  .setEncoding('utf8')
  .pipe(new GrepStream(pattern))
  .pipe(process.stdout);
```

***
### 16.5.2 Asinxron İterasiya (Asynchronous Iteration) ilə Axınları Oxumaq 🔁
Əvvəlki bölmədə axınları birləşdirmək üçün `.pipe()` metodunu gördük. Amma bəzən bizə axından gələn datanı hissə-hissə (chunk by chunk) özümüzün emal etməsi lazım gəlir.

Bunun üçün **ən müasir və tövsiyə olunan** yol, Node.js 12+ versiyalarında gələn **asinxron iterasiya (asynchronous iteration)** və **`for await...of`** dövründən istifadə etməkdir. Bu yanaşma, mürəkkəb hadisə (event) məntiqini gizlədərək, asinxron data axınını oxumağı adi bir sinxron dövr kimi sadə göstərir.

**Geniş Nümunə: `grep` funksiyasını `for await` ilə yenidən yazmaq**
Əvvəlki bölmədəki mürəkkəb `GrepStream` sinifini xatırlayırsan? İndi eyni məntiqi `for await` ilə nə qədər sadə yaza biləcəyimizə baxaq.
```javascript
/**
 * Bu asinxron funksiya, `source` axınından sətirləri oxuyur və yalnız `pattern`-ə
 * uyğun gələnləri `destination` axınına yazır.
 */
async function grep(source, destination, pattern, encoding = "utf8") {
  // Oxuma axınını mətn rejimində işləməsi üçün konfiqurasiya edirik
  source.setEncoding(encoding);

  let incompleteLine = ""; // Hissə-hissə gələn natamam sətirləri yadda saxlamaq üçün

  // 1. `for await` dövrü, `source`-dan hər yeni bir data hissəsi (chunk)
  //    gəldikcə onu gözləyir və `chunk` dəyişəninə mənimsədir.
  for await (const chunk of source) {
    // Əvvəldən qalan natamam sətrə yeni gələn hissəni əlavə edib, sətirlərə bölürük
    let lines = (incompleteLine + chunk).split("\n");
    // Sonuncu sətrin natamam olma ehtimalı var, onu sonrakı hissə üçün saxlayırıq
    incompleteLine = lines.pop();

    // Tamamlanmış sətirlər üzərində dövr edirik
    for (const line of lines) {
      // Əgər sətir bizim axtardığımız pattern-ə uyğun gəlirsə...
      if (pattern.test(line)) {
        // ...onu hədəf axına yazırıq
        destination.write(line + "\n", encoding);
      }
    }
  }

  // Dövr bitdikdən sonra, ən sonda qalan natamam sətri də yoxlayırıq
  if (pattern.test(incompleteLine)) {
    destination.write(incompleteLine + "\n", encoding);
  }
}

// ---- İstifadəsi ----
const pattern = new RegExp(process.argv[2]); // Axtarılacaq sözü command-line-dan götürürük

console.log(`--- "${pattern.source}" sözü üçün axtarış başlayır ---`);

// Standart girişi (stdin) oxuyuruq və nəticəni standart çıxışa (stdout) yazırıq
grep(process.stdin, process.stdout, pattern)
  .catch(err => {
    console.error(err);
    process.exit();
  });
```

---
### 16.5.3 Axınlara Yazmaq və Əks Təzyiqi (Backpressure) İdarə Etmək ✍️🚦
Bir `Writable` (yazıla bilən) axına data yazmaq üçün `.write()` metodundan istifadə edirik. Lakin burada çox vacib bir məqam var: **əks təzyiq (backpressure)**.

**Problem:** Təsəvvür et ki, siz məlumatı axına çox sürətlə yazırsınız, amma axının digər ucu (məsələn, fayla yazma və ya şəbəkəyə göndərmə) o sürətlə işləyə bilmir. Bu zaman, göndərilən data axının daxili buferində (internal buffer) yığılmağa başlayır və yaddaş problemi yaradır.

**Həll Yolu:** `.write()` metodu bu problemi bizə bildirmək üçün bir siqnal qaytarır.
* **`stream.write(chunk)` -> `true`**: "Davam et, buferdə hələ yer var."
* **`stream.write(chunk)` -> `false`**: "Dayan, bufer doludur! Bir az gözlə."

Əgər `.write()` `false` qaytararsa, yazmağı dayandırıb, axının `"drain"` hadisəsini göndərməsini gözləməliyik. `"drain"` hadisəsi, buferin boşaldığını və yenidən data qəbul etməyə hazır olduğunu bildirir.

**Geniş Nümunə: `for await` ilə əks təzyiqi düzgün idarə etmək**
Aşağıdakı `copy` funksiyası, bir axından digərinə datanı köçürərkən əks təzyiqi düzgün idarə edir. Bu, demək olar ki, `.pipe()`-ın əl ilə yazılmış versiyasıdır.
```javascript
// Bu köməkçi funksiya, axına bir hissə (chunk) yazır və
// yazmağa davam etməyin təhlükəsiz olub-olmadığını bildirən bir Promise qaytarır.
function write(stream, chunk) {
  const hasMoreRoom = stream.write(chunk);
  
  if (hasMoreRoom) {
    // Əgər yer varsa, dərhal həll olunan bir Promise qaytarırıq
    return Promise.resolve();
  } else {
    // Əgər yer yoxdursa, "drain" hadisəsi baş verənə qədər gözləyəcək bir Promise qaytarırıq
    return new Promise(resolve => {
      stream.once('drain', resolve);
    });
  }
}

// Bu funksiya, `source`-dan `destination`-a datanı əks təzyiqi nəzərə alaraq köçürür
async function copy(source, destination) {
  for await (const chunk of source) {
    // `write` funksiyasını `await` edirik. Əgər bufer doludursa,
    // bu sətir "drain" hadisəsi baş verənə qədər gözləyəcək.
    await write(destination, chunk);
  }
}

// copy(process.stdin, process.stdout);
```
---
### 16.5.4 Hadisələrlə (Events) Axınları Oxumaq (Köhnə Üsullar)
`for await` və `.pipe()` olmadıqda və ya uyğun olmadıqda, axınları oxumaq üçün iki klassik hadisə-yönümlü rejim var.

#### 1. Axan Rejim (Flowing Mode) ▶️
Bu rejimdə, axın datanı əldə etdiyi anda, onu avtomatik olaraq sizə doğru "itələyir". Siz sadəcə `"data"` hadisəsini dinləməlisiniz.
* **`stream.on('data', (chunk) => { ... })`**: Hər yeni data hissəsi gəldikdə çağırılır.
* **`stream.pause()`**: Data axınını müvəqqəti dayandırır (əks təzyiq üçün).
* **`stream.resume()`**: Dayandırılmış axını yenidən davam etdirir.
* **`stream.on('end', () => { ... })`**: Bütün data oxunub bitdikdə çağırılır.

**Nümunə: `copyFile` funksiyasını "flowing mode" ilə yazmaq**
```javascript
const fs = require("fs");

function copyFileFlowing(source, dest, callback) {
  const input = fs.createReadStream(source);
  const output = fs.createWriteStream(dest);

  input.on("data", (chunk) => {
    // Yeni data gəldikdə, onu çıxışa yazırıq
    const hasRoom = output.write(chunk);
    // Əgər çıxış buferi dolubsa, giriş axınını dayandırırıq
    if (!hasRoom) {
      input.pause();
    }
  });
  
  // Çıxış buferi boşaldıqda ("drain" hadisəsi), giriş axınını yenidən davam etdiririk
  output.on("drain", () => {
    input.resume();
  });
  
  // Giriş axını bitdikdə, çıxış axınını da bağlayırıq
  input.on("end", () => {
    output.end();
  });
  
  // Proses tam bitdikdə callback çağırılır
  output.on("finish", () => {
    callback(null);
  });
  
  // Xətaları idarə edirik
  input.on("error", callback);
  output.on("error", callback);
}
```

#### 2. Fasiləli Rejim (Paused Mode) ⏸️
Bu, axınların standart (default) rejimidir. Data avtomatik gəlmir. Siz datanı özünüz "dartıb" çıxarmalısınız.
* **`stream.on('readable', () => { ... })`**: Axının daxili buferində oxunacaq yeni data olduqda çağırılır.
* **`stream.read()`**: Buferdəki datanı oxuyur. Oxunacaq data yoxdursa `null` qaytarır.
❗️ **Vacib:** `"readable"` hadisəsi gəldikdən sonra, bufer tam boşalana, yəni `.read()` `null` qaytarana qədər onu dövr içində çağırmaq lazımdır. Əks halda, yeni `"readable"` hadisəsi heç vaxt baş verməyəcək.

**Nümunə: Faylın SHA256 hash-ini hesablamaq**
```javascript
const fs = require("fs");
const crypto = require("crypto");

function sha256(filename, callback) {
  const input = fs.createReadStream(filename);
  const hasher = crypto.createHash("sha256");

  // Oxunacaq data olduqda bu hadisə işə düşür
  input.on("readable", () => {
    let chunk;
    // Buferi tam boşaldana qədər oxuyuruq
    while((chunk = input.read()) !== null) {
      hasher.update(chunk); // Hər hissəni hasher-ə ötürürük
    }
  });
  
  // Axın bitdikdə yekun hash-i hesablayırıq
  input.on("end", () => {
    const hash = hasher.digest("hex");
    callback(null, hash);
  });

  input.on("error", callback);
}
```

***
### 16.6 Proses (Process), CPU və Əməliyyat Sistemi Məlumatları ℹ️
Node.js, işlədiyi mühit haqqında detallı məlumat almaq üçün bizə iki əsas vasitə təqdim edir: qlobal **`process` obyekti** və daxili **`os` modulu**.

#### `process` Obyekti - Hazırkı Prosesə Nəzarət ⚙️
`process` obyekti qlobaldır, yəni onu `require` etməyə ehtiyac yoxdur. O, hal-hazırda işləyən Node.js proqramı haqqında məlumatları və onu idarə etmək üçün metodları saxlayır.

**Geniş Nümunə: Proses haqqında hesabat**
Aşağıdakı kod, `process` obyektinin ən çox istifadə olunan xüsusiyyətlərindən bəzilərini göstərərək proqram haqqında bir hesabat hazırlayır.
```javascript
console.log("--- Proses Hesabatı ---");
console.log(`Node.js versiyası (version): ${process.version}`);
console.log(`Platforma (platform): ${process.platform}`); // 'darwin' (macOS), 'win32' (Windows), 'linux'
console.log(`Proses ID (pid): ${process.pid}`);
console.log(`İşləmə müddəti (uptime): ${(process.uptime()).toFixed(2)} saniyə`);
console.log(`Hazırkı qovluq (cwd): ${process.cwd()}`);

const memoryUsage = process.memoryUsage();
console.log(`Yaddaş istifadəsi (memoryUsage):`);
console.log(`  - RSS: ${(memoryUsage.rss / 1024 / 1024).toFixed(2)} MB`);
console.log(`  - Heap Total: ${(memoryUsage.heapTotal / 1024 / 1024).toFixed(2)} MB`);
console.log(`  - Heap Used: ${(memoryUsage.heapUsed / 1024 / 1024).toFixed(2)} MB`);

// Proqramı 1 saniyə sonra uğurlu bir kodla bitiririk
setTimeout(() => {
  console.log("\nProqram sonlandırılır...");
  process.exit(0); // 0 - uğurlu çıxış deməkdir
}, 1000);
```

**Digər Faydalı `process` Xüsusiyyətləri:**
* **`process.argv`**: Əmr sətri (command-line) arqumentlərini saxlayan massiv (array).
* **`process.env`**: Sistemin mühit dəyişənlərini (environment variables) saxlayan obyekt.
* **`process.execPath`**: Node.js proqramının özünün fayl sistemindəki tam yolu.
* **`process.getuid()` / `.setuid()`**: Unix sistemlərində istifadəçi ID-sini almaq və ya dəyişmək üçün.
* **`process.nextTick(callback)`**: `setTimeout(callback, 0)`-a bənzəyir, amma event loop-da daha tez işə düşür.

---
#### `os` Modulu - Əməliyyat Sistemi və Hardware 🖥️
Bu modul, proqramın işlədiyi kompüterin özü – yəni əməliyyat sistemi və hardware haqqında məlumatları əldə etmək üçündür. `process`-dən fərqli olaraq, onu `require` etmək lazımdır.

**Geniş Nümunə: Kompüter haqqında hesabat**
```javascript
// `os` modulunu import edirik
const os = require('os');

console.log("--- Kompüter Hesabatı ---");
console.log(`Əməliyyat sistemi (type): ${os.type()}`); // "Darwin" (macOS), "Windows_NT", "Linux"
console.log(`ƏS relizi (release): ${os.release()}`);
console.log(`Kompüterin adı (hostname): ${os.hostname()}`);
console.log(`Sistemin işləmə müddəti: ${(os.uptime() / 3600).toFixed(1)} saat`);

// Yaddaş (RAM) məlumatları
const totalMemGB = (os.totalmem() / 1024 / 1024 / 1024).toFixed(2);
const freeMemGB = (os.freemem() / 1024 / 1024 / 1024).toFixed(2);
console.log(`Ümumi Yaddaş (RAM): ${totalMemGB} GB`);
console.log(`Azad Yaddaş (RAM): ${freeMemGB} GB`);

// CPU haqqında məlumat
console.log("CPU Nüvələri (cores):");
os.cpus().forEach((cpu, i) => {
  console.log(`  - Nüvə ${i + 1}: ${cpu.model} @ ${cpu.speed}MHz`);
});

// Hazırkı istifadəçi haqqında məlumat
const userInfo = os.userInfo();
console.log(`Hazırkı istifadəçi: ${userInfo.username}`);
console.log(`Ev qovluğu (Home Directory): ${userInfo.homedir}`);
```
**Digər Faydalı `os` Metodları:**
* **`os.arch()`**: CPU arxitekturasını qaytarır ("x64", "arm").
* **`os.networkInterfaces()`**: Mövcud şəbəkə bağlantıları haqqında detallı məlumatları qaytarır.
* **`os.tmpdir()`**: Əməliyyat sisteminin müvəqqəti fayllar üçün standart qovluğunu qaytarır.
* **`os.EOL`**: Əməliyyat sisteminə uyğun sətir sonu işarəsini qaytarır ("\n" və ya "\r\n").

***
### 16.7 Fayllarla İşləmək 📁
Node.js-in daxili **`fs` (file system)** modulu, fayllar və qovluqlarla işləmək üçün çox geniş bir API təqdim edir. Bu modul, faylları oxumaq, yazmaq, silmək, adını dəyişmək və s. kimi bütün əməliyyatları əhatə edir.

`fs` modulundakı əksər funksiyaların bir neçə fərqli versiyası var:
1.  **Asinxron (Callback-based):** `fs.readFile()` kimi, "error-first" callback ilə işləyən standart Node.js üsulu.
2.  **Sinxron (Blocking):** `fs.readFileSync()` kimi, adı `Sync` ilə bitən və proqramı bloklayan, amma istifadəsi daha sadə olan versiyalar.
3.  **Promise-əsaslı:** `fs.promises.readFile()` kimi, müasir `async/await` ilə işləmək üçün ideal olan versiyalar.

Biz nümunələrdə əsasən müasir **Promise-əsaslı** və lazım gəldikdə **Sinxron** versiyalara fokuslanacağıq.

---
### 16.7.1 Yollar (Paths), Fayl Deskriptorları (File Descriptors) və FileHandles 🛣️
Bir faylla işləməzdən əvvəl, ona gedən yolu (path) düzgün təyin etməliyik. Fərqli əməliyyat sistemləri (Windows `\`, Linux/macOS `/`) fərqli yol ayırıcılarından istifadə etdiyi üçün, yolları əl ilə birləşdirmək səhvlərə yol aça bilər.

Bunun üçün Node.js-in daxili **`path`** modulu var. O, platformadan asılı olmayan, təhlükəsiz yol əməliyyatları üçün köməkçi funksiyalar təqdim edir.

**Geniş Nümunə: `path` modulu ilə işləmək**
```javascript
const path = require('path');

const myFilePath = '/users/test/project/index.js';

console.log("Platformanın ayırıcısı (sep):", path.sep); // Linux/macOS-da '/', Windows-da '\'

// Yolu hissələrə ayırmaq
console.log("Faylın adı (basename):", path.basename(myFilePath)); // ✅ "index.js"
console.log("Faylın genişlənməsi (extname):", path.extname(myFilePath)); // ✅ ".js"
console.log("Qovluğun yolu (dirname):", path.dirname(myFilePath)); // ✅ "/users/test/project"

// Yolları birləşdirmək (platformaya uyğun ayırıcı ilə)
const assetsPath = path.join(__dirname, 'assets', 'images', 'logo.png');
console.log("Birləşdirilmiş yol (join):", assetsPath);

// Nisbi yolu mütləq yola çevirmək
const absolutePath = path.resolve('style.css');
console.log("Mütləq yol (resolve):", absolutePath);
```
**Fayl Deskriptorları (File Descriptors):** Bunlar, açıq bir fayla istinad edən aşağı-səviyyəli rəqəmlərdir. Onlar yalnız `fs.read()` və `fs.write()` kimi, faylın müəyyən bir hissəsinə birbaşa müraciət tələb edən mürəkkəb əməliyyatlar üçün lazımdır. Gündəlik işlərdə adətən onlara ehtiyac olmur.

---
### 16.7.2 Faylların Oxunması 📖
Node.js-də bir faylı oxumağın 3 əsas yolu var. Hər birinin öz yeri və məqsədi var.

#### Üsul 1: Faylı Tamamilə Yaddaşa Oxumaq (All at Once)
Bu, kiçik və orta ölçülü fayllar (məsələn, konfiqurasiya faylları) üçün ən sadə və rahat yoldur. Amma böyük fayllar üçün yaddaş problemi yarada bilər.

**Geniş Nümunə (`async/await` ilə):**
```javascript
const fs = require('fs').promises; // Promise-əsaslı versiyanı import edirik

async function readConfigFile(filePath) {
  try {
    // Faylı "utf8" kodlaşdırması ilə oxuyuruq ki, nəticə birbaşa sətir (string) olsun
    const content = await fs.readFile(filePath, 'utf8');
    const config = JSON.parse(content);
    console.log("✅ Oxunan konfiqurasiya:", config);
    return config;
  } catch (error) {
    console.error("❌ Faylı oxumaq və ya emal etmək mümkün olmadı:", error);
  }
}

// readConfigFile('config.json');

// Sinxron versiya isə belədir (adətən proqram başlayarkən istifadə olunur):
// const configText = fs.readFileSync('config.json', 'utf8');
```

#### Üsul 2: Axınla (Stream) Oxumaq
Bu, böyük fayllarla işləmək üçün **ən səmərəli və tövsiyə olunan** yoldur. Faylın məzmunu yaddaşa tam yüklənmir, hissə-hissə (chunk by chunk) oxunur.

**Geniş Nümunə: Böyük bir faylın məzmununu konsola yazdırmaq**
```javascript
const fs = require('fs');

function printFile(filename) {
  console.log(`--- ${filename} faylının məzmunu ---`);
  // Oxumaq üçün bir axın (Readable Stream) yaradırıq
  fs.createReadStream(filename, 'utf8')
    // Və onu birbaşa standart çıxışa (yəni konsola) "bağlayırıq"
    .pipe(process.stdout)
    .on('error', (err) => console.error("Xəta:", err));
}

// printFile('my-large-log-file.log');
```

#### Üsul 3: Aşağı Səviyyəli (Low-Level) Oxuma
Bu, faylın yalnız müəyyən bir hissəsini, müəyyən bir bayt aralığını oxumaq lazım gəldikdə istifadə olunan qabaqcıl bir üsuldur. Bu, fayl deskriptorları (file descriptors) tələb edir.

**Nümunə: Faylın yalnız müəyyən baytlarını oxumaq**
```javascript
const fs = require('fs');

function readSpecificBytes(filename) {
  let fd; // Fayl deskriptoru üçün dəyişən
  try {
    // Faylı yalnız oxumaq üçün sinxron olaraq açırıq və deskriptoru alırıq
    fd = fs.openSync(filename, 'r');
    
    const buffer = Buffer.alloc(15); // Nəticəni saxlamaq üçün 15 baytlıq bir bufer
    
    // Fayldan oxuma əməliyyatı:
    // `readSync(fayl_deskriptoru, bufer, buferdə_başlanğıc, oxunacaq_uzunluq, faylda_başlanğıc)`
    fs.readSync(fd, buffer, 0, 15, 5); // 5-ci baytdan başlayaraq 15 bayt oxu
    
    console.log("Oxunan baytlar:", buffer);
    console.log("Sətir kimi:", buffer.toString('utf8'));
  } finally {
    // Əməliyyat bitdikdən sonra faylı HƏMİŞƏ bağlamaq vacibdir!
    if (fd !== undefined) {
      fs.closeSync(fd);
    }
  }
}
// readSpecificBytes('my-file.txt');
```

***
### 16.7.3 Fayllara Yazmaq (Writing Files) ✍️
Fayllara məlumat yazmaq da, onları oxumaq kimi üç əsas üsulla həyata keçirilə bilər. Vacib bir məqam: əgər yazdığınız fayl mövcud deyilsə, Node.js onu avtomatik olaraq yaradacaq.

#### Üsul 1: Faylı Bir Dəfəyə Yazmaq
Əgər yazmaq istədiyiniz bütün məzmun yaddaşda, bir sətir (string) və ya buferdə (buffer) hazırdırsa, bu ən sadə yoldur.
* **`fs.promises.writeFile(path, data, options)`**: Məlumatı asinxron olaraq fayla yazır. Əgər fayl varsa, onun üzərinə yazır (overwrite).
* **`fs.promises.appendFile(path, data, options)`**: Faylın üzərinə yazmır, məlumatı faylın sonuna əlavə edir.

**Geniş Nümunə: Konfiqurasiya faylını yaddaşa vermək**
```javascript
const fs = require('fs').promises;

const settings = {
  theme: "dark",
  user: "ayan99",
  notifications: false
};

async function saveSettings(filePath, data) {
  try {
    // Obyekti JSON sətrinə çevirib fayla yazırıq
    await fs.writeFile(filePath, JSON.stringify(data, null, 2), 'utf8');
    console.log(`✅ Ayarlar "${filePath}" faylına uğurla yazıldı.`);
  } catch(err) {
    console.error("❌ Ayarları yazmaq mümkün olmadı:", err);
  }
}

async function addLog(logPath, logMessage) {
    try {
        const logEntry = `${new Date().toISOString()}: ${logMessage}\n`;
        // `appendFile` ilə hər dəfə yeni logu faylın sonuna əlavə edirik
        await fs.promises.appendFile(logPath, logEntry, 'utf8');
        console.log("Log əlavə olundu.");
    } catch(err) {
        console.error("Log yazıla bilmədi:", err);
    }
}

// saveSettings('settings.json', settings);
// addLog('app.log', 'İstifadəçi sistemə daxil oldu.');
```

#### Üsul 2: Axınla (Stream) Yazmaq
Böyük həcmli datanı hissə-hissə (chunk by chunk) yazdırmaq üçün ən səmərəli yoldur. Bunun üçün `fs.createWriteStream()` ilə bir **yazıla bilən axın (Writable Stream)** yaradılır.

**Nümunə: Fayla 0-dan 99-a qədər rəqəmləri yazmaq**
```javascript
const fs = require('fs');

const outputStream = fs.createWriteStream("numbers.txt");

console.log("Rəqəmlər fayla yazılır...");

for(let i = 0; i < 100; i++) {
  // Hər rəqəmi axına yazırıq
  outputStream.write(`${i}\n`);
}

// ÇOX VACİB: Bütün yazma əməliyyatları bitdikdən sonra, axını mütləq bağlamaq lazımdır!
outputStream.end();

outputStream.on('finish', () => {
  console.log("✅ Fayla yazma əməliyyatı tamamlandı.");
});
```
#### Fayl Rejimi Fleqləri (File Mode Flags) 🚩
Faylları yazı üçün açarkən, onunla necə davranacağımızı bildirən xüsusi fleqlər (flags) təyin edə bilərik. Bunlar `writeFile` və `createWriteStream`-in `options` obyektində verilir. Ən vacibləri:
* **`"w"`** (write): Faylı yazmaq üçün açır. Əgər fayl varsa, məzmununu tamamilə silir. Standart (default) rejim budur.
* **`"a"`** (append): Faylı sonuna əlavə etmək üçün açır. Mövcud məzmuna toxunmur.
* **`"wx"`** (write, exclusive): Yalnız fayl mövcud deyilsə, onu yaradıb yazmaq üçün açır. Əgər fayl artıq mövcuddursa, xəta verir. Bu, təsadüfən bir faylın üzərinə yazmağın qarşısını almaq üçün çox faydalıdır.

**Nümunə:**
`fs.promises.writeFile("log.txt", "Yeni log...", { flag: "a" });`

---
### 16.7.4 Digər Fayl Əməliyyatları
Yazmaq və oxumaqdan başqa, fayllarla digər əməliyyatlar da apara bilərik.

**Faylı Kopyalamaq (`copyFile`)** ➡️
```javascript
const fs = require('fs').promises;

async function backupFile(filePath) {
  const backupPath = filePath + '.bak';
  try {
    // `COPYFILE_EXCL` fleqi, əgər ehtiyat nüsxə faylı artıq varsa,
    // onun üzərinə yazmağın qarşısını alır və xəta verir.
    await fs.copyFile(filePath, backupPath, fs.constants.COPYFILE_EXCL);
    console.log(`✅ Ehtiyat nüsxə yaradıldı: ${backupPath}`);
  } catch(err) {
    if (err.code === 'EEXIST') {
      console.warn(`⚠️ Ehtiyat nüsxə artıq mövcuddur: ${backupPath}`);
    } else {
      console.error(`❌ Ehtiyat nüsxə yaratmaq mümkün olmadı:`, err);
    }
  }
}
// backupFile('database.db');
```
**Faylın Adını/Yerini Dəyişmək (`rename`)** ➡️
Bu funksiya, faylın həm adını dəyişmək, həm də onu başqa bir qovluğa köçürmək (eyni disk bölümü daxilində) üçün istifadə olunur.
```javascript
async function moveFile(oldPath, newPath) {
  try {
    await fs.promises.rename(oldPath, newPath);
    console.log(`✅ Fayl "${oldPath}" -> "${newPath}" köçürüldü.`);
  } catch(err) {
    console.error("❌ Faylı köçürmək mümkün olmadı:", err);
  }
}
// moveFile('./temp.txt', './archive/temp.txt');
```
**Faylı Silmək (`unlink`)** 🗑️
Faylı fayl sistemindən tamamilə silir. (Adı Unix sistemlərindəki "linki qırmaq" məntiqindən gəlir).
```javascript
async function deleteFile(filePath) {
  try {
    await fs.promises.unlink(filePath);
    console.log(`✅ Fayl uğurla silindi: ${filePath}`);
  } catch(err) {
    console.error("❌ Faylı silmək mümkün olmadı:", err);
  }
}
// deleteFile('./temp.bak');
```
***
### 16.7.5 Fayl Metadatası (File Metadata) ℹ️
Bəzən bizə faylın içindəki məzmun deyil, faylın özü haqqında məlumatlar lazım olur: ölçüsü, yaradılma tarixi, icazələri və s. Bu məlumatlara **metadata** deyilir. Node.js-də bu məlumatları almaq üçün `fs.stat()` və onun variantlarından istifadə olunur.

**Metadata Almaq (`stat`)**
`fs.promises.stat(path)` metodu, fayl haqqında məlumatları saxlayan bir `Stats` obyekti ilə nəticələnən bir `Promise` qaytarır.

**Geniş Nümunə: Fayl haqqında hesabat**
```javascript
const fs = require('fs').promises;

async function getFileStats(filename) {
  try {
    // `stat` metodunu çağırırıq və nəticəsini gözləyirik
    const stats = await fs.stat(filename);

    console.log(`--- "${filename}" üçün metadata ---`);
    console.log(`Fayldırmı? (isFile):`, stats.isFile());
    console.log(`Qovluqdurmu? (isDirectory):`, stats.isDirectory());
    console.log(`Ölçüsü (size):`, stats.size, "bayt");
    console.log(`Son dəyişiklik tarixi (mtime):`, stats.mtime.toLocaleString('az-AZ'));
    console.log(`Yaradılma tarixi (birthtime):`, stats.birthtime.toLocaleString('az-AZ'));
    // İcazələri (permissions) 8-lik say sistemində göstəririk
    console.log(`İcazələr (mode):`, '0o' + stats.mode.toString(8));
  } catch (err) {
    console.error(`Metadata alına bilmədi:`, err.message);
  }
}

// getFileStats('package.json');
```
**Metadata Dəyişmək**
`fs` modulu həmçinin faylın icazələrini (`chmod`) və sahibini (`chown`) dəyişməyə imkan verir. Məsələn, bir faylı yalnız-oxunaqlı (read-only) etmək üçün:
`await fs.promises.chmod('config.json', 0o444);`

---
### 16.7.6 Qovluqlarla (Directories) İşləmək 📂
`fs` modulu, qovluqları yaratmaq, silmək və içindəkiləri siyahılamaq üçün də tam bir API təqdim edir.

#### Qovluq Yaratmaq və Silmək (`mkdir`, `rmdir`)
* **`fs.promises.mkdir(path, options)`**: Yeni bir qovluq yaradır. Ən faydalı opsiyası `{ recursive: true }`-dur. Bu, lazım gələrsə, bütün valideyn qovluqları da avtomatik olaraq yaradır (`mkdir -p` kimi).
* **`fs.promises.rmdir(path)`**: Boş bir qovluğu silir.

**Nümunə:**
```javascript
const fs = require('fs').promises;

async function setupProject() {
  try {
    // İç-içə qovluqları bir əmrlə yaradırıq
    await fs.mkdir('my-project/assets/images', { recursive: true });
    console.log("✅ Qovluq strukturu yaradıldı.");
  } catch (err) {
    console.error("❌ Qovluq yaratmaq mümkün olmadı:", err);
  }
}
// setupProject();
```
#### Qovluğun Məzmununu Siyahılamaq
Bunun üçün iki əsas üsul var:

**1. `readdir()` - Bütün Məzmunu Birdən Oxumaq**
Bu metod, qovluğun içindəki bütün fayl və qovluqların adlarından ibarət bir massiv (array) qaytarır. `{ withFileTypes: true }` opsiyası ilə hər bir elementin növünü (fayl və ya qovluq) də öyrənmək olar.

**Nümunə:** Hazırkı qovluqdakı alt-qovluqları tapmaq.
```javascript
const fs = require('fs').promises;

async function findSubdirectories(path = '.') {
  try {
    // `withFileTypes: true` bizə Dirent obyektləri verir
    const entries = await fs.readdir(path, { withFileTypes: true });
    
    const subdirs = entries
      .filter(entry => entry.isDirectory()) // Yalnız qovluqları filterləyirik
      .map(entry => entry.name); // Və onların adlarını götürürük
      
    console.log(`Alt-qovluqlar:`, subdirs);
  } catch (err) {
    console.error("Xəta:", err);
  }
}
// findSubdirectories();
```

**2. `opendir()` - Axınla (Stream) Oxumaq**
Çox böyük (minlərlə faylı olan) qovluqlarla işləyərkən, bütün siyahını yaddaşa yükləmək səmərəli deyil. `fs.promises.opendir()` bir **asinxron iterator (asynchronous iterator)** qaytarır və `for await...of` dövrü ilə qovluğun məzmununu hissə-hissə oxumağa imkan verir.

**Geniş Nümunə: Qovluğun məzmununu detallı siyahılamaq**
```javascript
const fs = require('fs').promises;
const path = require('path');

async function listDirectoryDetails(dirpath) {
  try {
    console.log(`--- "${dirpath}" qovluğunun məzmunu ---`);
    // 1. Qovluğu asinxron iterator kimi açırıq
    const dir = await fs.opendir(dirpath);
    
    // 2. `for await` ilə hər bir elementi bir-bir oxuyuruq
    for await (const entry of dir) {
      const fullPath = path.join(dirpath, entry.name);
      // Hər element üçün əlavə məlumat (metadata) alırıq
      const stats = await fs.stat(fullPath);
      const type = entry.isDirectory() ? "📂 Qovluq" : "📄 Fayl";
      
      console.log(`${type.padEnd(10)} | ${String(stats.size).padStart(8)} bayt | ${entry.name}`);
    }
  } catch (err) {
    console.error("Xəta:", err);
  }
}

// listDirectoryDetails('.');
```

***
### 16.8 HTTP Client-lər və Serverlər 🌐
Node.js-in daxili **`http`** və **`https`** modulları, HTTP protokolunun aşağı-səviyyəli (low-level), lakin tam funksiyalı bir tətbiqini təqdim edir. Bu API-lar bir az mürəkkəb olsa da, onların necə işlədiyini anlamaq, Express kimi freymvorkların arxa planda nə etdiyini başa düşmək üçün çox vacibdir.

---
#### HTTP Client: Serverə Sorğu Göndərmək 📤
Node.js ilə başqa bir serverə HTTP sorğusu göndərmək üçün `http.get()` (sadə GET sorğuları üçün) və ya `http.request()` (daha mürəkkəb POST, PUT kimi sorğular üçün) funksiyalarından istifadə edirik.

Bu API-lar köhnə, hadisə-yönümlü (event-based) olduğu üçün, onları müasir kodda rahat istifadə etmək məqsədilə adətən bir `Promise` içinə "bükürük".

**Geniş Nümunə: `postJSON` funksiyası**
Aşağıdakı funksiya, verilmiş bir obyektin `JSON` formatına salıb, `HTTPS POST` sorğusu ilə başqa bir serverə göndərir və serverin cavabını bir `Promise` olaraq qaytarır.
```javascript
const https = require("https");

/**
 * Bir obyekti JSON-a çevirib, göstərilən ünvana HTTPS POST sorğusu ilə göndərir.
 * Serverin JSON cavabını emal edib, nəticəni Promise olaraq qaytarır.
 */
function postJSON(host, endpoint, body) {
  return new Promise((resolve, reject) => {
    // 1. Göndəriləcək obyektin JSON sətirinə çevrilməsi
    const bodyText = JSON.stringify(body);

    // 2. HTTPS sorğusunun seçimlərini (options) təyin edirik
    const requestOptions = {
      method: "POST",
      host: host,
      path: endpoint,
      headers: {
        "Content-Type": "application/json",
        "Content-Length": Buffer.byteLength(bodyText) // Məzmunun ölçüsü (vacibdir!)
      }
    };

    // 3. Sorğunu yaradırıq
    const request = https.request(requestOptions);
    
    // 4. Məlumatı sorğunun gövdəsinə (body) yazırıq və sorğunu tamamlayırıq
    request.write(bodyText);
    request.end();

    // 5. Hadisələri (events) dinləyirik
    // Aşağı səviyyəli şəbəkə xətası üçün (məs. internet yoxdur)
    request.on("error", (e) => reject(e));

    // Serverdən ilk cavab gəldikdə bu hadisə baş verir
    request.on("response", (response) => {
      // Status kodu 200 deyilsə, xəta ilə reject edirik
      if (response.statusCode !== 200) {
        reject(new Error(`HTTP status: ${response.statusCode}`));
        response.resume(); // Yaddaş sızmasının qarşısını alır
        return;
      }
      
      response.setEncoding("utf8");
      
      // Cavabın gövdəsini hissə-hissə yığmaq üçün
      let responseBody = "";
      response.on("data", chunk => { responseBody += chunk; });
      
      // Bütün cavab gəlib bitdikdə
      response.on("end", () => {
        try {
          const parsed = JSON.parse(responseBody);
          resolve(parsed); // Uğurlu halda, JSON obyektini resolve edirik
        } catch (e) {
          reject(e); // JSON parse xətası olarsa, reject edirik
        }
      });
    });
  });
}

// ---- İstifadəsi ----
// async function testPost() {
//   const newUser = { name: "Ayan", job: "Developer" };
//   try {
//     const result = await postJSON("reqres.in", "/api/users", newUser);
//     console.log("Serverdən gələn cavab:", result);
//   } catch (error) {
//     console.error("postJSON xətası:", error);
//   }
// }
// testPost();
```
---
#### HTTP Server: Sorğuları Qəbul Etmək 📥
Node.js-in əsas gücü, asanlıqla yüksək performanslı HTTP serverlər yaratmaqdır.
**Əsas Məntiq:**
1.  `http.Server` obyekti yaradılır.
2.  Server `.listen(port)` metodu ilə müəyyən bir portda "qulaq asmağa" başlayır.
3.  Server, hər gələn sorğu üçün `"request"` hadisəsi göndərir. Biz də bu hadisəni dinləyən bir funksiya yazırıq. Bu funksiya iki obyekt qəbul edir: `request` (gələn sorğu) və `response` (göndərəcəyimiz cavab).

**Geniş Nümunə: Statik Faylları Göndərən Sadə Veb Server**
Bu server, göstərilən qovluqdakı faylları (HTML, CSS, JS) brauzerə göndərir.
```javascript
const http = require("http");
const url = require("url");
const path = require("path");
const fs = require("fs");

/**
 * Verilmiş qovluqdakı faylları, göstərilən portda xidmət edən bir server yaradır.
 * @param {string} rootDirectory - Faylların olduğu qovluğun yolu.
 * @param {number} port - Serverin qulaq asacağı port.
 */
function serve(rootDirectory, port) {
  const server = new http.Server();
  server.listen(port);
  console.log(`Server http://localhost:${port} ünvanında qulaq asır...`);

  // Hər yeni sorğu üçün bu funksiya işə düşür
  server.on("request", (request, response) => {
    // 1. Sorğunun URL-ini emal edirik
    const endpoint = url.parse(request.url).pathname;
    let filename = path.join(rootDirectory, endpoint.substring(1));
    
    // Təhlükəsizlik: istifadəçinin kök qovluqdan kənara çıxmasının qarşısını alırıq
    if (!filename.startsWith(rootDirectory)) {
        response.writeHead(403).end("Forbidden");
        return;
    }

    // 2. Faylın məzmun tipini (content type) təxmin edirik
    const type = {
        ".html": "text/html", ".css": "text/css", ".js": "text/javascript",
        ".png": "image/png", ".jpeg": "image/jpeg", ".txt": "text/plain"
    }[path.extname(filename)] || "application/octet-stream";

    // 3. Faylı oxumaq üçün bir Readable Stream yaradırıq
    const stream = fs.createReadStream(filename);

    // 4. Axın (stream) hadisələrini dinləyirik
    stream.once("readable", () => {
      // Fayl oxuna biləndirsə, uğurlu başlıqları (headers) göndəririk
      response.setHeader("Content-Type", type);
      response.writeHead(200); // 200 OK
      // Və faylın məzmununu birbaşa `response` obyektinə "bağlayırıq" (pipe)
      // Bu, ən səmərəli yoldur!
      stream.pipe(response);
    });

    stream.on("error", (err) => {
      // Əgər fayl tapılmasa və ya başqa bir xəta olsa...
      response.setHeader("Content-Type", "text/plain; charset=UTF-8");
      response.writeHead(404); // 404 Not Found
      response.end(err.message);
    });
  });
}

// Serveri işə salırıq. İkinci arqument qovluq, üçüncü isə portdur.
// Terminalda belə işə salın: node server.js ./public 8000
// serve(process.argv[2] || ".", parseInt(process.argv[3]) || 8000);
```
Bu sadə kod ilə, artıq sizin lokal fayllarınızı brauzerə göndərən bir veb serveriniz var!

***
### 16.9 Qeyri-HTTP Şəbəkə Serverləri və Client-ləri 🔌
HTTP vebin təməl protokolu olsa da, şəbəkə proqramlaşdırması təkcə ondan ibarət deyil. Node.js-in daxili **`net`** modulu, client və server arasında birbaşa, davamlı bir əlaqə quran TCP soketləri (sockets) ilə işləmək üçün tam bir API təqdim edir.

**Analogy:** Əgər HTTP bir poçt xidmətidirsə (məktubların müəyyən formatı və qaydaları var), **TCP** birbaşa **telefon xəttidir** 📞. Qaydaları sən qoyursan, istədiyin formatda "danışa" bilərsən.

Bu cür serverlər real-time oyunlar, çat proqramları və ya fərdi protokollarla işləyən sistemlər üçün istifadə olunur.

**Əsas Məntiq:** `net` modulunda bir **`Socket`** obyekti, həm oxumaq, həm də yazmaq üçün olan bir **`Duplex` axınıdır (stream)**.

* **Server Yaratmaq:** `net.createServer()` ilə bir server obyekti yaradılır, `.listen(port)` ilə müəyyən bir portda "qulaq asmağa" başlayır və hər yeni client qoşulduqda `"connection"` hadisəsi göndərir.
* **Client Yaratmaq:** `net.createConnection(port, host)` ilə serverə qoşulan bir `Socket` obyekti yaradılır.

---
### Geniş Nümunə: "Tak-Tak" Zarafatı Serveri (Knock-Knock Joke Server) 😂
Bu nümunə, qoşulan hər bir client-ə interaktiv olaraq "Tak-tak" zarafatı edən bir TCP serveri yaradır. Bu, `net`, `readline` və `async/await` kimi bir neçə Node.js xüsusiyyətini birləşdirən əla bir nümunədir.

**`joke-server.js` faylı:**
```javascript
const net = require("net");
const readline = require("readline"); // Sətir-sətir oxumaq üçün

// 1. Server obyektini yaradırıq
const server = net.createServer();

// 2. Serveri 6789 portunda dinləməyə başlayırıq
server.listen(6789, () => {
  console.log("😂 Zarafat serveri 6789 portunda işləyir...");
});

// 3. Hər yeni client qoşulduqda "connection" hadisəsi baş verir
server.on("connection", socket => {
  console.log("Yeni bir client qoşuldu!");
  
  // Həmin client üçün zarafat prosesini başladırıq
  tellJoke(socket)
    .catch(err => console.error("Zarafat zamanı xəta:", err))
    .finally(() => {
      // Zarafat bitdikdə və ya xəta olduqda bağlantını bağlayırıq
      console.log("Client ilə bağlantı kəsildi.");
      socket.end();
    });
});

// Bildiyimiz zarafatlar
const jokes = {
  "Boo": "Don't cry... it's only a joke!",
  "Lettuce": "Let us in! It's freezing out here!",
  "A little old lady": "Wow, I didn't know you could yodel!"
};

// Bu asinxron funksiya, verilmiş soket üzərində zarafatı interaktiv şəkildə edir
async function tellJoke(socket) {
  // Təsadüfi bir zarafat seçirik
  const who = Object.keys(jokes)[Math.floor(Math.random() * Object.keys(jokes).length)];
  const punchline = jokes[who];

  // Client-dən gələn datanı sətir-sətir oxumaq üçün readline interfeysi yaradırıq
  const lineReader = readline.createInterface({ input: socket, output: socket });
  
  // Client-ə mətn göndərmək üçün köməkçi funksiya
  const output = (text) => socket.write(`${text}\r\n>> `);
  
  let stage = 0; // Zarafatın hansı mərhələdə olduğunu izləyirik

  output("Knock knock!"); // Zarafatı başlayırıq

  // `for await` ilə client-dən gələn hər sətri asinxron olaraq oxuyuruq
  for await (const inputLine of lineReader) {
    if (stage === 0) { // İlk mərhələ: "Who's there?" gözləyirik
      if (inputLine.trim().toLowerCase() === "who's there?") {
        output(who); // Düzgün cavab gəldikdə, zarafatın ilk hissəsini deyirik
        stage = 1; // Və növbəti mərhələyə keçirik
      } else {
        output('Zəhmət olmasa, "Who\'s there?" yazın.');
      }
    } else if (stage === 1) { // İkinci mərhələ: "{who} who?" gözləyirik
      if (inputLine.trim().toLowerCase() === `${who.toLowerCase()} who?`) {
        socket.write(`${punchline}\r\n`); // Düzgün cavab gəldikdə, sonluğu deyirik
        return; // Və funksiyanı bitiririk
      } else {
        output(`Zəhmət olmasa, "${who} who?" yazın.`);
      }
    }
  }
}
```

#### Client Tərəfi (Client-Side)
Bu cür sadə bir serverə qoşulmaq üçün xüsusi bir proqram yazmağa ehtiyac yoxdur. Əksər əməliyyat sistemlərində olan `nc` (netcat) utiliti ilə qoşula bilərsiniz.
**Terminalda:**
`$ nc localhost 6789`

Amma Node.js ilə client yazmaq da çox asandır. Aşağıdakı kod, serverə qoşulur və terminaldakı yazdıqlarınızı serverə, serverdən gələnləri isə terminala ötürür.

**`joke-client.js` faylı:**
```javascript
const net = require("net");

// Command-line-dan serverin host adını götürürük (əgər verilməyibsə localhost)
const host = process.argv[2] || 'localhost';

// Serverə qoşuluruq
const socket = net.createConnection(6789, host);

// Serverdən gələn hər şeyi birbaşa konsola (stdout) "bağlayırıq"
socket.pipe(process.stdout);

// Konsola yazdığımız hər şeyi isə birbaşa serverə (socket) "bağlayırıq"
process.stdin.pipe(socket);

// Bağlantı kəsildikdə proqramı bitiririk
socket.on('close', () => process.exit());
socket.on('error', () => process.exit());
```
Bu `.pipe()` istifadəsi, Node.js axınlarının (streams) nə qədər güclü və zərif olduğunu göstərən mükəmməl bir nümunədir.

***
### 16.10 Uşaq Proseslərlə (Child Processes) İşləmək ⚙️
Node.js-in ən güclü tərəflərindən biri, öz proqramının içindən başqa sistem proqramlarını və ya skriptləri işə salmaq imkanıdır. Bu, **`child_process`** modulu vasitəsilə həyata keçirilir. Bu modul, fərqli ehtiyaclar üçün 4 əsas yanaşma təqdim edir.

---
#### 1. `execSync()` — Sinxron və Sadə İcra
Bu, bir əmri icra etməyin ən asan yoludur. O, proqramı **bloklayır** – yəni, çağırılan proqram işini bitirənə qədər sizin Node proqramınız gözləyir. Sadə skriptlər və birdəfəlik əməliyyatlar üçün idealdır.

* **Necə işləyir?** Verilən əmri sistemin **shell**-ində (məsələn, bash, cmd) icra edir. Bu, `|`, `*`, `&&` kimi shell xüsusiyyətlərindən istifadə etməyə imkan verir.
* **Nəticə:** Uğurlu olarsa, proqramın standart çıxışını (stdout) birbaşa qaytarır. Xəta olarsa, exception atır.

**Geniş Nümunə: Hazırkı `git` statusunu yoxlamaq**
```javascript
const { execSync } = require('child_process');

try {
  console.log("Git statusu yoxlanılır...");
  // `git status` əmrini işə salır və nəticəsini birbaşa string olaraq alır
  const gitStatus = execSync('git status', { encoding: 'utf8' });
  
  if (gitStatus.includes("nothing to commit, working tree clean")) {
    console.log("✅ Repozitoriya təmizdir.");
  } else {
    console.log("⚠️ Repozitoriyada dəyişikliklər var:\n", gitStatus);
  }
} catch (error) {
  // Əgər qovluq bir git repo deyilsə və ya `git` quraşdırılmayıbsa, xəta baş verəcək
  console.error("❌ Bu bir git repozitoriyası deyil və ya 'git' tapılmadı.");
}
```

---
#### 2. `exec()` — Asinxron və Buferlənmiş İcra
Bu, `execSync`-in asinxron (asynchronous) versiyasıdır. Proqramı bloklamır, nəticə hazır olduqda isə bir `Promise` (və ya callback) vasitəsilə xəbər verir.

* **Nə zaman istifadə etməli?** Qısa müddətdə işləyib bitən və çıxışı çox böyük olmayan proqramlar üçün idealdır. Çünki o, bütün çıxışı yaddaşda **buferləşdirir** və yalnız sonda təqdim edir.

**Geniş Nümunə: Sistemdə müəyyən proqramların quraşdırıldığını paralel yoxlamaq**
```javascript
const util = require('util');
const { exec } = require('child_process');
const execPromise = util.promisify(exec); // exec-i promisify edirik

async function checkCommands(commands) {
  console.log("Proqramlar yoxlanılır:", commands.join(', '));

  // Hər bir proqram üçün bir yoxlama Promise-i yaradırıq
  const checkPromises = commands.map(cmd => {
    // Unix-də 'which', Windows-da 'where' əmri proqramın mövcudluğunu yoxlayır
    return execPromise(`which ${cmd}`).then(() => `${cmd}: ✅ Mövcuddur`);
  });

  // Bütün yoxlamaların nəticəsini gözləyirik
  const results = await Promise.allSettled(checkPromises);

  results.forEach(result => {
    if (result.status === 'fulfilled') {
      console.log(result.value);
    } else {
      // execPromise xəta qaytardıqda, deməli proqram tapılmayıb
      const cmd = result.reason.cmd.split(' ')[1];
      console.log(`${cmd}: ❌ Mövcud deyil`);
    }
  });
}

// checkCommands(['node', 'npm', 'git', 'docker']);
```

---
#### 3. `spawn()` — Asinxron və Axınla (Stream) İcra 📜
Bu, uşaq proseslərlə işləməyin ən güclü və çevik yoludur. `exec`-dən fərqli olaraq, çıxışı buferləşdirmir. Bunun əvəzinə, uşaq prosesin **standart giriş (`stdin`), çıxış (`stdout`) və xəta (`stderr`) axınlarına (streams)** birbaşa giriş imkanı verir.

* **Nə zaman istifadə etməli?** Uzun müddət işləyən və ya davamlı olaraq çıxış yaradan proqramlarla (məsələn, bir log faylını izləmək, video emalı) real-time interaksiya üçün idealdır.

**Geniş Nümunə: Bir log faylını canlı izləmək (`tail -f`)**
```javascript
const { spawn } = require('child_process');

const logFilePath = 'server.log';

// `tail -f` əmri, faylın sonuna əlavə olunan yeni sətirləri davamlı olaraq çıxışa verir
const tailProcess = spawn('tail', ['-f', logFilePath]);

console.log(`"${logFilePath}" faylı canlı izlənilir... (Dayandırmaq üçün Ctrl+C)`);

// Uşağın standart çıxışını (stdout) dinləyirik
tailProcess.stdout.on('data', (data) => {
  // Hər yeni sətir gəldikcə onu formatlayıb konsola yazdırırıq
  process.stdout.write(`[LOG]: ${data.toString()}`);
});

tailProcess.on('error', (err) => {
  console.error(`"'tail'" əmrini işə salmaq mümkün olmadı. Fayl mövcuddurmu?`, err);
});
```
---
#### 4. `fork()` — Node Proseslərini Çağırmaq 💬
`fork()` metodu, `spawn()`-un xüsusi bir növüdür və **yalnız başqa bir Node.js skriptini** yeni bir proses kimi işə salmaq üçün nəzərdə tutulub.

* **Əsas Üstünlüyü:** `fork()` avtomatik olaraq valideyn (parent) və uşaq (child) proses arasında bir **ünsiyyət kanalı (IPC - Inter-Process Communication)** yaradır. Bu, onlara `.send()` metodu ilə bir-birinə asanlıqla mesaj (obyekt, massiv) göndərməyə və `.on('message', ...)` ilə bu mesajları qəbul etməyə imkan verir.

**Geniş Nümunə: Bloklayan hesablamanı uşaq prosesə həvalə etmək**
Ağır bir hesablamanı worker-ə ötürərək, əsas prosesin "donmasının" qarşısını alırıq.

**`calculator.js` (Uşaq Proses)**
```javascript
// Valideyn prosesdən mesaj gözləyirik
process.on('message', (message) => {
  if (message.command === 'sum') {
    let sum = 0;
    console.log(`Uşaq proses hesablamaya başladı... (bu bir neçə saniyə çəkəcək)`);
    // Ağır, bloklayan bir əməliyyat
    for (let i = 0; i < message.number; i++) {
      sum += i;
    }
    // Nəticəni valideynə geri göndəririk
    process.send({ result: sum });
  }
});
```
**`main.js` (Valideyn Proses)**
```javascript
const { fork } = require('child_process');

console.log("Valideyn proses başladı.");

// `calculator.js` skriptini yeni bir proses kimi işə salırıq
const child = fork(`${__dirname}/calculator.js`);

// Uşaq prosesdən gələn cavabı gözləyirik
child.on('message', (message) => {
  console.log(`✅ Uşaq prosesdən cavab gəldi: ${message.result}`);
  // İş bitdiyi üçün bağlantını kəsirik
  child.disconnect();
});

// Uşaq prosesə tapşırığı göndəririk
child.send({ command: 'sum', number: 5e9 }); // 5 milyarda qədər cəmləmə

console.log("Valideyn uşağın cavabını gözləyərkən öz işinə davam edir və donmur!");
```
Bu dörd üsul, Node.js-də sistem səviyyəsində müxtəlif ehtiyaclar üçün fərqli həllər təqdim edir.

***
### 16.11 İşçi Axınları (Worker Threads) 🧵
Node.js-in əsas işləmə modeli tək-axınlı və hadisə-yönümlüdür. Bu, proqram yazmağı çox sadələşdirir. Lakin ağır bir hesablama (CPU-intensive task) əsas axını "dondura" və serverin cavab vermə qabiliyyətini aşağı sala bilər.

**Worker Threads** bu problemi həll edir. Onlar, əsas proqramla paralel olaraq arxa planda (background) işləyən həqiqi thread-lərdir.

❗️ **Ən Vacib Məqam (Təhlükəsizlik):** Ənənəvi thread proqramlaşdırmasından fərqli olaraq, Node worker-ləri standart olaraq **yaddaşı paylaşmır**. Onlar bir-biri ilə yalnız **mesajlaşma (message passing)** yolu ilə "danışır". Bu, multithreading-in ənənəvi başağrılarının (`race conditions`, `deadlocks`) qarşısını alır və onu daha təhlükəsiz edir.

Worker-lərdən 3 əsas halda istifadə etmək olar:
1.  **Əsl Paralellik:** Çoxnüvəli (multi-core) prosessorlarda ağır hesablamaları (məsələn, şəkil emalı, elmi hesablamalar) müxtəlif nüvələrə paylayaraq sürəti artırmaq üçün.
2.  **Cavabdehliyi (Responsiveness) Qorumaq:** Əsas axının həmişə yeni gələn sorğulara cavab verməyə hazır olmasını təmin etmək üçün, uzun çəkən hesablamaları arxa plana keçirmək.
3.  **Sinxron Kodu Asinxrona Çevirmək:** Əlinizdə köhnə, bloklayan (blocking) bir funksiya varsa, onu bir worker içində işə salaraq, əsas proqramınız üçün qeyri-bloklayan (non-blocking) bir əməliyyata çevirə bilərsiniz.

---
### 16.11.1 Worker Yaratmaq və Mesajlaşmaq 📨
Worker-lərlə işləmək üçün **`worker_threads`** modulundan istifadə edirik.

**Əsas API:**
* **Əsas kodda (Main Thread):**
    * `new Worker(path, options)`: Yeni bir worker yaradır.
    * `worker.postMessage(data)`: Worker-ə məlumat göndərir.
    * `worker.on('message', ...)`: Worker-dən gələn mesajları qəbul edir.
* **Worker kodunda (Worker Thread):**
    * `parentPort`: Əsas proqramla (valideynlə) danışmaq üçün olan obyektdir.
    * `parentPort.postMessage(data)`: Valideynə mesaj göndərir.
    * `parentPort.on('message', ...)`: Valideyndən gələn mesajı qəbul edir.

**Geniş Nümunə: Əsas və Worker kodunu eyni faylda saxlamaq**
Bu nümunə, `isMainThread` yoxlaması ilə həm əsas proqramın, həm də worker-in məntiqini eyni faylda saxlamağın zərif bir yolunu göstərir. Əsas proqram, ağır bir işi worker-ə ötürür və nəticəni bir `Promise` olaraq qaytarır.
```javascript
// my-worker.js faylı

const threads = require('worker_threads');
const { parentPort, Worker } = threads;

// isMainThread yoxlaması, kodun hansı mühitdə işlədiyini bildirir
if (threads.isMainThread) {
  // --- 1. ƏSAS PROQRAM MƏNTİQİ (MAIN THREAD) ---
  console.log("Əsas proqram işə düşdü.");

  // Bu funksiya, ağır bir işi worker-ə ötürür və nəticə üçün bir Promise qaytarır
  function runHeavyTask(data) {
    return new Promise((resolve, reject) => {
      // Worker-i elə bu faylın özündən yaradırıq (`__filename`)
      const worker = new Worker(__filename);
      
      // Worker-dən mesaj gəldikdə, Promise-i `resolve` edirik
      worker.on('message', resolve);
      // Worker-də xəta baş verərsə, Promise-i `reject` edirik
      worker.on('error', reject);
      
      // Tapşırığı (datanı) worker-ə göndəririk
      worker.postMessage(data);
    });
  }
  
  // Funksiyanı test edirik
  runHeavyTask({ number: 42 })
    .then(result => console.log("✅ Əsas proqram worker-dən nəticəni aldı:", result))
    .catch(err => console.error("❌ Əsas proqram xəta tutdu:", err));

} else {
  // --- 2. İŞÇİ MƏNTİQİ (WORKER THREAD) ---
  // Bu hissə yalnız worker olaraq işə salındıqda icra olunur
  
  // Valideyndən gələn tək bir mesajı dinləmək üçün `.once` istifadə edirik
  // Bu, worker-in işini bitirdikdən sonra avtomatik sonlanmasına imkan verir
  parentPort.once('message', (taskData) => {
    console.log("Worker tapşırığı qəbul etdi:", taskData);
    
    // Tutaq ki, bu çox ağır bir hesablamadır
    const result = taskData.number * taskData.number;
    
    // Nəticəni valideynə geri göndəririk
    parentPort.postMessage({ result });
  });
}
```
**Mesajların ötürülməsi:** `postMessage` ilə göndərilən obyektlər **Structured Clone Algorithm** ilə kopyalanır. Bu, `JSON.stringify`-dan daha güclüdür və `Map`, `Set`, `Date` kimi tipləri də dəstəkləyir.

---

### 16.11.2 Worker-in İcra Mühiti (Execution Environment) ⚙️
Əsas proqram (main thread) və worker eyni JavaScript kodunu işlətsə də, onların "yaşadığı" mühit arasında bəzi vacib fərqlər var. Bu fərqləri bilmək, worker-ləri daha effektiv idarə etməyə kömək edir.

#### `workerData` - Başlanğıc Məlumatını Ötürmək
Worker-i yaradarkən, ona `postMessage` gözləmədən, birbaşa işə başlaması üçün ilkin məlumat ötürmək olar. Bu, `Worker` konstruktorunun ikinci arqumentindəki `workerData` xüsusiyyəti ilə edilir. Bu, `postMessage`-dən daha səmərəli bir yoldur.

**Geniş Nümunə:**
**`main.js` (Əsas kod)**
```javascript
const { Worker } = require('worker_threads');

const worker = new Worker(__filename, {
  // workerData ilə worker-ə başlanğıc məlumatı ötürürük
  workerData: {
    message: "Salam, worker!",
    initialValue: 100
  }
});

worker.on('message', msg => console.log(msg));
```
**`my-worker.js` (Eyni faylın `else` bloku)**
```javascript
// ...
} else {
  // --- İŞÇİ MƏNTİQİ (WORKER THREAD) ---
  const { workerData, parentPort } = require('worker_threads');

  // workerData-nı birbaşa, mesaj gözləmədən əldə edirik
  console.log("Worker-ə ötürülən ilkin data:", workerData); // { message: 'Salam, worker!', initialValue: 100 }

  const result = workerData.initialValue * 2;
  parentPort.postMessage(`Nəticə: ${result}`);
}
```

#### Digər Fərqlər:
* **`isMainThread` və `parentPort`**: Artıq bildiyimiz kimi, `isMainThread` worker-də `false` olur və worker-in əsas proqramla danışması üçün `parentPort` obyekti olur.
* **`process.env`**: Standart olaraq, worker əsas proqramın mühit dəyişənlərinin (environment variables) bir **kopyasını** alır. Onlar arasında yaddaş paylaşılmır.
* **`stdin`, `stdout`, `stderr`**: Standart olaraq, worker-in `console.log()` və `console.error()` çıxışları birbaşa əsas proqramın konsoluna ötürülür (piped). Worker-in `process.stdin`-i isə standart olaraq boşdur. Bu davranışları da konstruktorun `options` obyektində dəyişmək olar.
* **Prosesə Təsir:** Worker-in içindən `process.exit()` çağırmaq bütün proqramı deyil, **yalnız həmin worker thread-ini** sonlandırır. `process.chdir()` kimi bütün prosesə təsir edən metodları isə worker içindən çağırmaq olmaz.

---
### 16.11.3 Ünsiyyət Kanalları (Communication Channels) və `MessagePort`-lar ↔️
Əsas proqram və worker arasında standart bir mesajlaşma kanalı (`worker` obyekti ↔ `parentPort`) olsa da, bəzən daha mürəkkəb ssenarilər üçün xüsusi, birbaşa kanallar yaratmaq lazım gəlir. Məsələn:
* Valideyn ilə worker arasında fərqli məqsədlər üçün bir neçə kanal yaratmaq (biri normal, biri "təcili" mesajlar üçün).
* İki fərqli worker-in bir-biri ilə birbaşa, valideyni narahat etmədən "danışmasını" təmin etmək.

Bunun üçün **`MessageChannel`** API-ı istifadə olunur.

**Analogy:** Əgər standart `postMessage` bir ümumi radio kanalıdırsa, `MessageChannel` iki nəfər arasında qurulmuş xüsusi bir **telsiz rabitəsidir** 📞.

**API:**
`new MessageChannel()`-ı çağırdıqda, o, iki xüsusiyyəti olan bir obyekt qaytarır: `port1` və `port2`. Bunlar bir-birinə bağlanmış iki `MessagePort` obyektidir. Bir porta `.postMessage()` ilə göndərilən mesaj, digər portda `"message"` hadisəsi ilə qəbul edilir.

**Sadə Nümunə (hələlik yalnız əsas proqram daxilində):**
```javascript
const { MessageChannel } = require('worker_threads');

// 1. Yeni bir rabitə kanalı yaradırıq
const channel = new MessageChannel();

// 2. Hər iki portu dəyişənlərə mənimsədirik
const port1 = channel.port1;
const port2 = channel.port2;

// 3. Port 2-dən gələn mesajları dinləyirik
port2.on('message', (message) => {
  console.log('📥 Port 2-də mesaj qəbul edildi:', message);
  port2.close(); // İşi bitirdikdən sonra portu bağlayırıq
  port1.close();
});

// 4. Port 1 vasitəsilə mesaj göndəririk
console.log('📤 Port 1-dən mesaj göndərilir...');
port1.postMessage({ data: 'Bu, özəl bir kanaldır!' });
```
Bu portlardan birini `postMessage`-in ikinci arqumenti kimi bir worker-ə **ötürməklə (transfer)**, əsas proqram və worker arasında xüsusi bir kanal yaratmış oluruq.

---

### 16.11.4 `MessagePort` və `TypedArray`-lərin Ötürülməsi (Transferring) 📦➡️
Biz bilirik ki, `postMessage()` məlumatı bir thread-dən digərinə **kopyalayaraq** göndərir. Lakin bəzi obyektlər üçün kopyalamaq əvəzinə, onları birbaşa **ötürmək (transfer)** mümkündür.

**Analogy:** Kopyalamaq (`structured clone`), sanki bir sənədin surətini çıxarıb dostunuza vermək kimidir. Hər ikinizdə bir nüsxə olur. **Ötürmək (transfer)** isə, sənədin orijinalını birbaşa dostunuza vermək kimidir. Artıq o sənəd sizdə yox, yalnız dostunuzdadır. Bu, daha sürətlidir, amma obyekt orijinal yerdə istifadəsiz hala gəlir.

Bu, `postMessage()`-in ikinci, könüllü arqumenti olan **`transferList`** massivi (array) ilə edilir.

#### `MessagePort`-un Ötürülməsi
`MessageChannel` ilə yaratdığımız xüsusi rabitə kanallarını worker-lərlə istifadə etmək üçün, portlardan birini onlara ötürməliyik.

**Nümunə: Worker-ə xüsusi bir rabitə kanalı vermək**
**`main.js` (Valideyn Proqram)**
```javascript
const { Worker, MessageChannel } = require('worker_threads');

const worker = new Worker('./my-worker.js');
// 1. Yeni bir rabitə kanalı yaradırıq
const channel = new MessageChannel();

// 2. port2-ni worker-ə ötürürük. Bunun üçün onu həm mesajın içinə,
//    həm də transfer siyahısına (`transferList`) əlavə edirik.
worker.postMessage({ command: 'connect', port: channel.port2 }, [channel.port2]);

// 3. İndi worker ilə `port1` vasitəsilə danışa bilərik
channel.port1.on('message', (msg) => {
  console.log("✅ Worker-dən özəl kanalla cavab gəldi:", msg);
});

channel.port1.postMessage("Bu mesaj özəl kanalla gedir!");
```
**`my-worker.js` (Uşaq Proses)**
```javascript
const { parentPort } = require('worker_threads');

// Valideyndən gələn ilk mesaja qulaq asırıq
parentPort.once('message', (message) => {
  if (message.command === 'connect') {
    const privatePort = message.port;
    console.log("Worker özəl kanalı qəbul etdi.");
    
    // Özəl kanaldan gələn mesajları dinləyirik
    privatePort.on('message', (msg) => {
      console.log("Worker özəl kanalla mesaj aldı:", msg);
      // Cavabı da eyni kanalla geri göndəririk
      privatePort.postMessage("Eşidirəm!");
    });
  }
});
```
#### `TypedArray`-lərin Ötürülməsi (Performans üçün)
Böyük həcmli binary data (şəkil, səs, video) ilə işləyərkən, `TypedArray`-ləri (və onların arxasındakı `ArrayBuffer`-i) kopyalamaq çox baha başa gəlir. Onları **ötürmək (transfer)** isə demək olar ki, ani baş verir (sıfır-kopyalama, zero-copy).

Bunun üçün `postMessage`-in ikinci arqumentinə `TypedArray`-in özünü deyil, onun `.buffer`-ini ötürmək lazımdır.

**Nümunə:**
```javascript
const pixels = new Uint8Array(1024 * 1024); // 1MB data

console.log("Ötürülmədən əvvəl buffer-in ölçüsü:", pixels.buffer.byteLength); // ✅ 1048576

// `pixels.buffer`-i transfer siyahısına əlavə edirik
worker.postMessage(pixels, [pixels.buffer]);

// Ötürüldükdən sonra, buffer əsas proqramda boşalır və istifadəsiz hala gəlir!
console.log("Ötürüldükdən sonra buffer-in ölçüsü:", pixels.buffer.byteLength); // ✅ 0
```
---
### 16.11.5 `TypedArray`-lərin Thread-lər Arasında Paylaşılması (Sharing) ⚠️
Bu, çox qabaqcıl və potensial olaraq **təhlükəli** bir xüsusiyyətdir.
Standart olaraq, worker-lər yaddaşı paylaşmır və bu, onları təhlükəsiz edir. **`SharedArrayBuffer`** isə bu qaydanı pozaraq, iki thread-in eyni yaddaş sahəsinə eyni anda yazmasına imkan verir. Bu, gözlənilməz və çətin tapılan xətalara – **yarış vəziyyətlərinə (race conditions)** – gətirib çıxarır.

**Problem: Race Condition**
Aşağıdakı nümunədə, həm əsas, həm də worker thread eyni ortaq dəyişəni 10 milyon dəfə artırır. Nəticə 20 milyon olmalı olsa da, heç vaxt o rəqəmi almırıq, çünki thread-lər bir-birinin artırma əməliyyatının üstünə yazır.
```javascript
// ... (SharedArrayBuffer ilə yazılmış səhv kod nümunəsi) ...
// Nəticə (təxmini): 11843567 (20,000,000 deyil!)
```

**Həll Yolu: `Atomics` API-ı**
`Atomics` obyekti, `SharedArrayBuffer` üzərində **atomik (bölünməz)** əməliyyatlar aparmağa imkan verir. Yəni, bir əməliyyat (məsələn, dəyəri oxumaq, artırmaq və geri yazmaq) başqa heç bir thread tərəfindən kəsilə bilməz.

**Nümunə: `Atomics.add` ilə düzgün artırma**
```javascript
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');
const { SharedArrayBuffer, Int32Array, Atomics } = global;

if (isMainThread) {
    const sharedBuffer = new SharedArrayBuffer(4); // 4 baytlıq ortaq yaddaş
    const sharedArray = new Int32Array(sharedBuffer);
    
    new Worker(__filename, { workerData: sharedArray });

    for (let i = 0; i < 10_000_000; i++) {
      // Atomik olaraq 1 əlavə edir
      Atomics.add(sharedArray, 0, 1);
    }
    
    // ... worker-dən mesaj gözləmə məntiqi ...
} else {
    const sharedArray = workerData;
    for (let i = 0; i < 10_000_000; i++) {
      Atomics.add(sharedArray, 0, 1);
    }
    parentPort.postMessage('done');
}
// Bu kod düzgün olaraq 20,000,000 nəticəsini verəcək.
```
**Nəticə:** `SharedArrayBuffer` və `Atomics` çox spesifik, yüksək performans tələb edən hallar üçündür. Əksər hallarda, yaddaşı paylaşmayan standart worker-lər və `postMessage` ilə mesajlaşma daha təhlükəsiz və sadə yoldur.