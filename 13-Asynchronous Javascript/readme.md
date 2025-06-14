### Fəsil 13. Asinxron (Asynchronous) JavaScript ⚡

Əksər proqramlar fasiləsiz işləmir. Onlar çox vaxt xarici bir hadisənin baş verməsini gözləyir: istifadəçinin düyməyə klikləməsi, şəbəkədən (network) məlumatın gəlməsi və ya müəyyən bir vaxtın keçməsi. **Asinxron proqramlaşdırma**, proqramın bu gözləmə müddətində "donub" qalmaması, əksinə, başqa işləri görməyə davam etməsi üçün bir üsuldur.

Bu fəsildə asinxron əməliyyatları idarə etmək üçün JavaScript-in 3 əsas alətini öyrənəcəyik:
1.  **Promises (Vədlər)**: Asinxron bir əməliyyatın gələcəkdəki nəticəsini təmsil edən obyektlər.
2.  **`async/await`**: `Promise`-lərə əsaslanan, lakin kodu sanki sinxron (synchronous) imiş kimi yazmağa imkan verən rahat sintaksis.
3.  **Asinxron Iteratorlar və `for/await`**: Asinxron data axınları (streams) ilə işləmək üçün dövrlər (loops).

İronik olan odur ki, JavaScript dilinin özündə asinxron bir şey yoxdur. Bu xüsusiyyətləri anlamaq üçün biz əvvəlcə brauzer və Node.js tərəfindən təmin edilən asinxron API-lara (məsələn, zamanlayıcılar, hadisələr) baxmalıyıq.

---
### 13.1 "Callback"-lər ilə Asinxron Proqramlaşdırma
JavaScript-də asinxronluğun ən fundamental səviyyəsi **"callback"** funksiyalarıdır.

**Callback nədir?** 🤔
Bu, yazdığınız və başqa bir funksiyaya arqument kimi ötürdüyünüz bir funksiyadır. Həmin funksiya, müəyyən bir şərt ödəndikdə və ya asinxron bir hadisə baş verdikdə sizin funksiyanızı "geri çağırır" (calls you back).

Bunu bir analogiya ilə anlayaq: Dostuna deyirsən ki, "Mənə zəng edib xəbər edərsən" - bu, callback-dir. Sən öz işinə davam edirsən, dostun isə vaxtı gələndə sənə "geri zəng edir".

---
### 13.1.1 Zamanlayıcılar (Timers) ⏳
Asinxronluğun ən sadə forması, bir kodu müəyyən bir müddət sonra icra etməkdir. Bunu `setTimeout` ilə edirik.

**Nümunə 1: `setTimeout` ilə birdəfəlik gecikmə**
```javascript
function sayHello() {
  console.log("👋 Salam, 3 saniyə keçdi!");
}

console.log("Zamanlayıcı quruldu...");

// sayHello funksiyasını 3000 millisaniyə (3 saniyə) sonra icra etmək üçün planlaşdırırıq
setTimeout(sayHello, 3000);

console.log("Zamanlayıcı qurulandan sonraki kod... (gördüyünüz kimi bu kod gözləmir)");
```

**Nümunə 2: `setInterval` ilə təkrarlanan əməliyyat**
Bəzən bir əməliyyatı davamlı olaraq təkrarlamaq lazım gəlir. Məsələn, hər dəqiqə yeni bildirişləri yoxlamaq.
```javascript
let count = 1;
function checkUpdates() {
  console.log(`Bildirişlər yoxlanılır... (dəfə ${count})`);
  count++;
}

console.log("Periodik yoxlama başladıldı (hər 3 saniyədən bir)...");
const intervalId = setInterval(checkUpdates, 3000);

// 10 saniyə sonra periodik yoxlamanı dayandıraq
setTimeout(() => {
  clearInterval(intervalId); // setInterval-ın qaytardığı ID ilə onu dayandırırıq
  console.log("✅ Periodik yoxlama dayandırıldı.");
}, 10000);
```

---
### 13.1.2 Hadisələr (Events) 🖱️
Brauzerdəki JavaScript proqramları demək olar ki, həmişə **hadisə yönümlü (event-driven)** olur. Yəni, onlar kodun icrası üçün istifadəçinin bir hərəkətini (klik, klaviatura düyməsi, toxunma) gözləyir.

Bu hadisələri "dinləmək" üçün `addEventListener` metodundan istifadə edirik. Biz ona hansı hadisəni gözlədiyimizi və həmin hadisə baş verdikdə hansı callback funksiyasını çağırmalı olduğunu deyirik.

**Nümunə: Düyməyə klikləmə hadisəsi**
```javascript
// Təsəvvür edək ki, HTML-də belə bir düyməmiz var:
// <button id="myButton">Mənə kliklə!</button>

// Bu kod brauzerdə işləyir:
// 1. HTML elementini seçirik
// const button = document.querySelector('#myButton');

// 2. Klik hadisəsi baş verdikdə çağırılacaq callback funksiyası
function onButtonClick() {
  console.log("🎉 Düyməyə klikləndi! İstifadəçiyə reaksiya veririk...");
}

// 3. Düyməyə "click" hadisəsi üçün "qulaqcıq" (listener) əlavə edirik
// button.addEventListener('click', onButtonClick);

console.log("Hadisə dinləyicisi quruldu. İstifadəçinin klikləməsi gözlənilir...");
```
Burada `onButtonClick` bizim **callback** funksiyamızdır. Biz onu `addEventListener`-ə veririk və brauzer, istifadəçi düyməyə klikləyənə qədər onu yaddaşda saxlayır. Klik baş verdikdə isə onu "geri çağırır".

***
### 13.1.3 Şəbəkə Hadisələri (Network Events) 🌐
JavaScript-də asinxronluğun ən çox rast gəlinən mənbəyi şəbəkə (network) sorğularıdır. Brauzerdə çalışan bir kodun serverdən məlumat çəkməsi zaman tələb edir və bu proses proqramı "dondurmamalıdır".

Ənənəvi olaraq, bu, `XMLHttpRequest` obyekti və callback funksiyaları ilə edilirdi.

**Əsas Məntiq: "Callback" ilə Nəticəni Gözləmək**
Asinxron bir funksiya (məsələn, serverdən məlumat istəyən) nəticəni dərhal `return` edə bilməz. Bunun əvəzinə, biz ona bir **callback funksiyası** ötürürük. Həmin asinxron funksiya işini bitirdikdə (məlumat gəldikdə və ya xəta baş verdikdə), nəticəni bizim callback funksiyamıza ötürərək onu çağırır.

**"Error-First" Callback Qaydası:** Çox geniş yayılmış bir qaydaya görə, callback funksiyaları adətən iki arqument qəbul edir: `callback(error, result)`.
* Əməliyyat uğurlu olarsa, birinci arqument `null`, ikinci isə nəticə olur: `callback(null, "Məlumatlar")`.
* Xəta baş verərsə, birinci arqument xəta obyekti, ikinci isə `null` olur: `callback("Xəta baş verdi!", null)`.

**Nümunə: `XMLHttpRequest` ilə serverdən versiya nömrəsini almaq**
```javascript
/**
 * Bu funksiya serverə sorğu göndərərək cari versiya nömrəsini asinxron şəkildə alır
 * və nəticəni və ya xətanı `versionCallback` funksiyasına ötürür.
 */
function getCurrentVersionNumber(versionCallback) {
  console.log("🌐 Serverə sorğu göndərilir...");
  
  // 1. HTTP sorğu obyekti yaradırıq
  const request = new XMLHttpRequest();
  
  // 2. Sorğunu konfiqurasiya edirik (GET metodu ilə verilən URL-ə)
  request.open("GET", "http://www.example.com/api/version");
  
  // 3. Sorğunu göndəririk (bu, asinxron başlayır)
  request.send();
  
  // 4. Callback-ləri qeydiyyatdan keçiririk
  
  // 4a. Sorğu uğurla tamamlandıqda bu funksiya çağırılacaq
  request.onload = function() {
    if (request.status === 200) {
      // HTTP statusu 200 OK-dirsə, cavabı emal edirik
      const currentVersion = parseFloat(request.responseText);
      // Nəticəni "error-first" qaydası ilə callback-ə ötürürük
      versionCallback(null, currentVersion);
    } else {
      // Əgər server xətası varsa, onu bildiririk
      versionCallback(`Server xətası: ${request.statusText}`, null);
    }
  };
  
  // 4b. Şəbəkə səviyyəsində xəta baş verərsə (məs. internet kəsilsə)
  request.onerror = request.ontimeout = function(e) {
    versionCallback(`Şəbəkə xətası: ${e.type}`, null);
  };
}

// Funksiyanı belə istifadə edirik:
getCurrentVersionNumber((error, version) => {
  if (error) {
    console.error("❌ Versiya alına bilmədi. Səbəb:", error);
  } else {
    console.log(`✅ Serverdən gələn cari versiya: ${version}`);
  }
});

console.log("Sorğu göndərildi, nəticə gələcəkdə callback vasitəsilə gələcək...");
```

---
### 13.1.4 Node.js-də Callback-lər və Hadisələr ⚙️
Node.js server tərəfi (server-side) mühiti olduğu üçün performansın yüksək olması məqsədilə dərin şəkildə asinxrondur. Demək olar ki, bütün fayl və şəbəkə əməliyyatları asinxron işləyir və callback-lərdən geniş istifadə edir.

**Nümunə 1: Faylın məzmununu asinxron oxumaq (`fs.readFile`)**
Node.js-də bir faylı oxumaq üçün istifadə olunan standart funksiya asinxrondur və işini bitirdikdə bir callback çağırır.
```javascript
const fs = require("fs"); // Fayl sistemi (filesystem) modulunu import edirik

console.log("Konfiqurasiya faylı oxunur...");

// `fs.readFile` asinxron olaraq faylı oxuyur və bitirdikdə callback-i çağırır.
// Callback yenə də "error-first" (err, text) prinsipi ilə işləyir.
fs.readFile("config.json", "utf-8", (err, text) => {
  if (err) {
    // Əgər fayl oxunarkən xəta baş verərsə...
    console.error("❌ Konfiqurasiya faylını oxumaq mümkün olmadı:", err.message);
    return;
  }
  
  // Əgər hər şey qaydasındadırsa, faylın məzmununu istifadə edirik
  const options = JSON.parse(text);
  console.log("✅ Fayldan oxunan konfiqurasiya:", options);
  
  // Proqramın qalan hissəsini buradan işə salmaq olar...
  // startProgram(options);
});

console.log("`readFile` çağırıldı, amma proqram gözləmir, öz işinə davam edir...");
```
**Nümunə 2: Node.js ilə HTTP sorğusu (Streaming Events)**
Node.js-də şəbəkə sorğuları daha mürəkkəbdir və hadisələr (events) üzərində qurulub. Böyük bir cavab dərhal deyil, **hissə-hissə (chunks)** gəlir. Buna **axın (stream)** deyilir. Biz bu axını dinləmək üçün hadisələrə "abunə" oluruq.
```javascript
const https = require("https");

function getUrlContent(url, callback) {
  // 1. HTTP GET sorğusunu başladırıq
  const request = https.get(url);
  
  // 2. Hadisə dinləyicilərini (event listeners) qeydiyyatdan keçiririk
  
  // 'response' hadisəsi: serverdən ilk cavab (başlıqlar - headers) gəldikdə baş verir
  request.on("response", response => {
    let httpStatus = response.statusCode;
    if (httpStatus !== 200) {
        callback(`Server xətası: Status ${httpStatus}`, null);
        return;
    }

    let body = ""; // Gələn hissələri (chunks) yığmaq üçün
    response.setEncoding("utf-8");
    
    // 'data' hadisəsi: hər dəfə yeni bir data hissəsi gəldikdə baş verir
    response.on("data", chunk => {
      body += chunk;
    });
    
    // 'end' hadisəsi: bütün hissələr gəlib bitdikdə baş verir
    response.on("end", () => {
      // Yalnız bu anda bütün məlumatı əldə etmiş oluruq
      callback(null, body);
    });
  });
  
  // 'error' hadisəsi: aşağı səviyyəli şəbəkə xətaları üçün
  request.on("error", err => {
    callback(err, null);
  });
}

// Funksiyanı istifadə edirik:
getUrlContent("https://jsonplaceholder.typicode.com/posts/1", (error, content) => {
    if (error) {
        console.error("❌ URL-dən məlumat alına bilmədi:", error);
    } else {
        console.log("✅ Serverdən gələn məlumat:", JSON.parse(content));
    }
});
```

***
### 13.2 "Vəd"lər (Promises) 🤝

**Promise nədir?** 🤔
Təsəvvür et ki, dostundan borc pul istəyirsən. Dostun sənə dərhal pulu vermir, amma söz verir ki, "sabah verəcəm". Bu **söz (vəd)** bir `Promise`-dir.

**Promise (Vəd)** – asinxron bir əməliyyatın **gələcəkdəki nəticəsini** təmsil edən bir obyektdir. Bu nəticə hələ hazır ola da bilər, olmaya da. Promise API-ı bizə birbaşa "nəticə hazırdırmı?" deyə yoxlamaq imkanı vermir. Bunun əvəzinə, biz Promise-ə deyirik: "Nəticə hazır olanda bu funksiyanı çağırarsan".

Ən sadə səviyyədə, Promise-lər callback-lərlə işləməyin fərqli bir yoludur. Amma onların çox mühüm üstünlükləri var.

#### Niyə Callback-lərdən Daha Yaxşıdır?
**1. "Callback Cəhənnəmi"ndən (Callback Hell) Xilas Edir ⛓️**
Callback-lərlə işlədikdə, çox vaxt iç-içə callback-lər yazmalı oluruq. Bu, kodun oxunmasını və idarə edilməsini çətinləşdirir:
```javascript
// Callback Hell nümunəsi:
getData(id, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      getAuthor(comments[0].authorId, (author) => {
        // Kod bir piramida kimi sağa doğru gedir və oxunmaz hala gəlir...
      });
    });
  });
});
```
Promise-lər isə bu iç-içə strukturu daha oxunaqlı olan **xətti bir zəncir (linear chain)** halına gətirməyə imkan verir.

**2. Standartlaşdırılmış Xəta İdarəetməsi (Standardized Error Handling) 🛡️**
Callback-lərdə xətaları idarə etmək çox çətindir. Əgər ən dərindəki bir callback xəta versə, bu xətanın yuxarıya ötürülməsi üçün hər addımda əl ilə yoxlama aparmaq lazımdır. Promise-lərdə isə xəta idarəetməsi standartlaşdırılıb. Zəncirin istənilən yerində baş verən xəta avtomatik olaraq zəncirin sonundakı xəta tutucusuna (error handler) ötürülür.

#### Məhdudiyyətləri (Limitations)
❗️ **Promise-lər yalnız *bir dəfə* yerinə yetirilən əməliyyatların nəticəsini təmsil edir.**

Bu o deməkdir ki, Promise-lər:
* `setTimeout` üçün əla bir alternativdir (çünki bir dəfə işləyir).
* Amma `setInterval` üçün alternativ deyil (çünki o, davamlı olaraq təkrarlanır).
* Bir düymənin `click` hadisəsi üçün uyğun deyil (çünki istifadəçi düyməyə dəfələrlə klikləyə bilər).

Növbəti bölmələrdə bunları daha detallı araşdıracağıq:
* Promise terminologiyası və əsas istifadə qaydaları.
* Promise-ləri necə zəncirvari bağlamaq olar.
* Öz Promise-əsaslı API-larımızı necə yaratmaq olar.

#### VACİB QEYD 🧠
Promise-lər ilk baxışdan sadə görünsə də, dərinliklərinə getdikcə bəzi çaşdırıcı məqamları ortaya çıxır. Onları düzgün və əminliklə istifadə etmək üçün məntiqini dərindən anlamaq lazımdır. Bu səbəbdən, bu fəsli diqqətlə öyrənməyə dəyər, çünki bu, müasir asinxron proqramlaşdırmanın təməlidir.

***
### 13.2.1 Promise-lərin İstifadəsi (Using Promises)

Təsəvvür edək ki, əvvəlki bölmədəki kimi serverdən məlumat çəkən bir funksiyamız var, amma bu dəfə o, callback qəbul etmir, bunun əvəzinə bir **Promise obyekti** qaytarır. Gəlin bu funksiyanı `getJSON()` adlandıraq və onunla necə işlədiyimizə baxaq:

```javascript
// getJSON funksiyası dərhal bir Promise qaytarır
getJSON("/api/user/profile").then(jsonData => {
  // .then() metoduna ötürdüyümüz bu funksiya (callback)...
  // ...yalnız şəbəkə sorğusu uğurla bitib, məlumatlar gəldikdə asinxron olaraq çağırılır.
  console.log("✅ Məlumatlar uğurla alındı:", jsonData);
});
```

**Məntiq necədir?**
1.  `getJSON()` funksiyası çağırılan kimi, o, arxa planda şəbəkə sorğusunu başladır və dərhal bizə bir "vəd" (Promise obyekti) qaytarır. Bu vədin içi hələlik boşdur, o, "gözləmədədir".
2.  Biz bu Promise obyektinin `.then()` metodunu çağıraraq ona deyirik: "Əməliyyat uğurla bitsə, bu funksiyanı çağırarsan".
3.  Serverdən cavab gəlib, məlumatlar uğurla emal edildikdə, Promise həmin məlumatları bizim `.then()`-ə verdiyimiz funksiyaya arqument kimi ötürərək onu çağırır.

Bu sintaksis kodu demək olar ki, bir ingilis cümləsi kimi oxunaqlı edir:
`getJSON(...)` **sonra (then)** `displayUserProfile(...)`
"Məlumatı al, **sonra** istifadəçi profilini göstər."

---
#### Xətaların İdarə Edilməsi (Handling Errors) ❌

Asinxron əməliyyatlar (xüsusilə şəbəkə sorğuları) hər an xəta ilə nəticələnə bilər. Promise-lər bunun üçün çox rahat və standart bir yol təqdim edir.

Əvvəllər `.then()` metoduna ikinci bir funksiya ötürmək mümkün idi: `promise.then(onSuccess, onError)`. Lakin bu, köhnə üsuldur. Daha müasir və güclü üsul `.catch()` metodundan istifadə etməkdir.

**Niyə `.catch()` daha yaxşıdır?** 🤔
Çünki `.catch()` təkcə orijinal asinxron əməliyyatın (`getJSON`) xətasını deyil, həm də `.then()` içindəki kodun özündə baş verən xətaları da tutur!

**Geniş Nümunə: `.then()` və `.catch()` zənciri**
```javascript
function displayData(data) {
  console.log("✅ Uğurlu nəticə:", data);
  // Təsəvvür edək ki, məlumatı emal edərkən burada yeni bir xəta baş verir
  throw new Error("Məlumatı emal etmək mümkün olmadı!");
}

function handleError(error) {
  console.error("❗️ Xəta baş verdi:", error.message);
}

console.log("Sorğu göndərilir...");

// Bu zəncir həm getJSON-dan, həm də displayData-dan gələn xətaları tutacaq
getJSON("/api/user/profile")
  .then(displayData) // Uğur halında bu işə düşür
  .catch(handleError); // İstənilən xəta halında isə bu
```
Bu zəncirvari (chaining) quruluş, xətaların idarə edilməsini çox səliqəli edir.

---
### Promise Terminologiyası 📖

Promise-lərlə işləyərkən 3 əsas vəziyyəti (state) bilmək vacibdir:

* **`Pending` (Gözləmədə) ⏳**: Promise-in ilkin vəziyyəti. Əməliyyat hələ bitməyib, nəticə bəlli deyil.
* **`Fulfilled` (Yerine Yetirildi) ✅**: Əməliyyat uğurla başa çatdı. Promise-in bir "dəyəri" (value) var.
* **`Rejected` (Rədd Edildi) ❌**: Əməliyyat xəta ilə nəticələndi. Promise-in bir "səbəbi" (reason/error) var.

Bir Promise ya `fulfilled`, ya da `rejected` olduqda, onun vəziyyəti **`Settled` (Həll Edildi) 🏁** adlanır. Bir Promise `settled` olduqdan sonra onun vəziyyəti və nəticəsi heç vaxt dəyişmir.

```
            ┌───────────┐
Başlanğıc ──> │  Pending  │ ──┬──> Fulfilled (nəticə ilə) ──┐
            └───────────┘   │                                 ├─> Settled
                            └──> Rejected (xəta ilə)  ───┘
```

***
### 13.2.2 Promise Zəncirləri (Chaining Promises) ⛓️
Promise-lərin ən böyük üstünlüklərindən biri, bir neçə ardıcıl asinxron əməliyyatı iç-içə callback-lər yazmadan, oxunaqlı və **xətti bir zəncir (linear chain)** şəklində ifadə etməyə imkan verməsidir.

Bu zənciri anlamaq üçün gəlin müasir veb sorğuları üçün istifadə olunan, Promise-əsaslı **`fetch` API**-ına baxaq. `fetch` köhnə `XMLHttpRequest`-i əvəz edən müasir bir vasitədir.

**`fetch` necə işləyir?**
1.  Siz `fetch(url)` çağırırsınız, o, dərhal sizə bir `Promise` qaytarır.
2.  Serverdən ilk cavab (başlıqlar - headers) gəldikdə, bu `Promise` bir `Response` obyekti ilə yerinə yetirilir (`fulfilled`).
3.  Lakin bu `Response` obyekti hələ sorğunun tam məzmunu (body) deyil. Məzmunu almaq üçün `response.json()` və ya `response.text()` kimi metodları çağırmalısınız.
4.  Və ən vacib məqam: bu `.json()` və `.text()` metodları da özləri **yeni bir Promise** qaytarır, çünki məzmunun tam yüklənməsi də vaxt apara bilər.

**Problem: İç-içə Promise-lər (Callback Hell-ə bənzər)**
Əgər bu məntiqi səhv başa düşsək, belə bir kod yaza bilərik:
```javascript
// ❌ BU YOL MƏSLƏHƏT GÖRÜLMÜR
fetch("/api/user/profile").then(response => {
  // Bu, köhnə callback hell üslubudur
  response.json().then(profile => { 
    displayUserProfile(profile);
  });
});
```
Bu, bizi yenidən iç-içə quruluşa qaytarır və Promise-lərin əsas məqsədini itirir.

**Həll Yolu: Promise Zənciri (Promise Chain)**
Düzgün və oxunaqlı yol, `.then()` metodlarını bir-birinə zəncir kimi bağlamaqdır:
```javascript
// ✅ DÜZGÜN VƏ OXUNAQLI YOL
fetch("/api/user/profile") // 1. Sorğunu göndər
  .then(response => {
    // 2. Cavab gələndə, məzmunu JSON kimi istə (bu da yeni bir Promise qaytarır)
    return response.json(); 
  })
  .then(profile => {
    // 3. JSON məzmunu tam hazır olanda, onu istifadə et
    displayUserProfile(profile);
  })
  .catch(error => {
    // 4. Bütün zəncir boyunca baş verən istənilən xətanı burada tut
    handleError(error);
  });
```

#### Zəncir Necə İşləyir? 🤯
Bu zəncirvari məntiqin işləməsinin səbəbi budur:
1.  Hər `.then()` çağırışı özündən əvvəlki deyil, **tamamilə yeni bir Promise** qaytarır.
2.  Əgər `.then()`-in içindəki funksiya bir dəyər qaytarırsa (məsələn, `return 10`), növbəti `.then()` həmin dəyəri qəbul edir.
3.  **Ən vacib qayda:** Əgər `.then()` içindəki funksiya başqa bir **Promise qaytarırsa** (bizim nümunədə `response.json()` kimi), zəncir həmin daxili Promise-in həll olunmasını (settle) gözləyir. Daxili Promise uğurlu olarsa, onun nəticəsi növbəti `.then()`-ə ötürülür. Uğursuz olarsa, birbaşa `.catch()` bloku işə düşür.

**Addım-addım Zəncirin Analizi** ➡️
Gəlin `fetch(url).then(callback1).then(callback2)` zəncirinin pərdə arxasında necə işlədiyinə baxaq:  
1️⃣ **`fetch(url)` çağırılır.** O, arxa planda HTTP sorğusunu başladır və dərhal **Promise 1**-i qaytarır.  
2️⃣ Biz **Promise 1**-in `.then()` metodunu çağırırıq və ona `callback1`-i (bizim nümunədə `response => response.json()`) veririk. `.then()` metodu isə bizə dərhal **Promise 2**-ni qaytarır.  
3️⃣ Biz dərhal **Promise 2**-nin `.then()` metodunu çağırırıq və ona `callback2`-ni (bizim nümunədə `profile => displayUserProfile(profile)`) veririk. Bu metod da bizə **Promise 3**-ü qaytarır.  
4️⃣ Bu ilk 3 addım sinxron (dərhal) baş verir. İndi proqram sorğunun cavabının gəlməsini asinxron olaraq gözləyir.  
5️⃣ Serverdən cavab (başlıqlar) gələn kimi, **Promise 1** `Response` obyekti ilə yerinə yetirilir (`fulfilled`).  
6️⃣ **Promise 1** yerinə yetirildiyi üçün, onun nəticəsi (`Response` obyekti) `callback1`-ə ötürülür. `callback1` içindəki `response.json()` metodu çağırılır. Bu metod özü də bir **Promise qaytardığı üçün**, **Promise 2** hələ gözləmədə qalır.  
7️⃣ Nəhayət, sorğunun tam məzmunu (body) yüklənib JSON-a çevrildikdən sonra, `response.json()`-ın qaytardığı daxili Promise yerinə yetirilir. Bu, öz növbəsində **Promise 2**-ni də həmin JSON obyekti ilə yerinə yetirir.  
8️⃣ **Promise 2** yerinə yetirildiyi üçün, onun nəticəsi (JSON obyekti) `callback2`-yə ötürülür və istifadəçi profili ekranda göstərilir.  

***
### 13.2.3 Promise-lərin "Həll Olunması" (Resolving Promises)

Əvvəlki bölmədə belə bir zəncir görmüşdük:
`fetch(url).then(response => response.json()).then(profile => ...)`

Burada bir sual yaranır: `response.json()` metodu özü bir Promise qaytarırsa, necə olur ki, növbəti `.then()` birbaşa həmin Promise-in nəticəsini (`profile` obyektini) qəbul edir? Arada sehr baş vermir. Bu, Promise-lərin "həll olunma" (resolving) mexanizminin işidir.

Gəlin bu prosesi daha aydın görmək üçün kodu addım-addım, açıq şəkildə yazaq:

```javascript
// c1 - birinci .then-in callback-i
function c1(response) {
  console.log("callback 1 (c1) işə düşdü, response obyekti gəldi.");
  // response.json() özü bir Promise qaytarır!
  const p4 = response.json(); 
  console.log("c1, Promise 4-ü (p4) qaytarır.");
  return p4;
}

// c2 - ikinci .then-in callback-i
function c2(profile) {
  console.log("callback 2 (c2) işə düşdü. Nəticə:", profile);
  // displayUserProfile(profile);
}

// Prosesi addım-addım izləyirik
const p1 = fetch("/api/user/profile"); // p1 (Promise 1) yaradıldı
const p2 = p1.then(c1);             // p2 (Promise 2) yaradıldı
const p3 = p2.then(c2);             // p3 (Promise 3) yaradıldı
```
Bu kodda `c1` funksiyası `profile` obyektini deyil, `p4`-ü, yəni başqa bir Promise-i qaytarır. Amma biz `c2`-nin `profile` obyektini almasını gözləyirik. Bu necə olur?

Cavab `.then()`-in qaytardığı Promise-in davranışındadır.

#### "Resolved" Vəziyyəti Nədir?

`.then(callback)` metodunu çağırdıqda, o, bizə yeni bir Promise (`p2`) qaytarır. Daha sonra, asinxron olaraq `callback` (`c1`) funksiyası çağırılır və bu funksiya bir dəyər (`v`) qaytarır. `callback` öz işini bitirib `v` dəyərini qaytardıqda, `p2` **həll olunur (resolved)**.

Burada iki hal mümkündür:

1.  **Əgər `v` adi bir dəyərdirsə (Promise DEYİLSƏ):** Məsələn, bir rəqəm, sətir və ya obyekt. Bu zaman `p2` dərhal **yerinə yetirilir (fulfilled)** və onun dəyəri `v` olur. ✅

2.  **Əgər `v` özü də bir Promise-dirsə (bizim nümunədəki `p4` kimi):** Bu zaman `p2` **həll olunur (resolved)**, lakin hələ də **gözləmədə (pending)** qalır. Bu vəziyyətdə, `p2` sanki öz taleyini `p4`-ə "bağlayır" və ya ona "kilidlənir". `p2`-nin gələcəyi tamamilə `p4`-ün taleyindən asılı olur.
    * Əgər `p4` uğurla **yerinə yetirilsə (fulfilled)**, `p2` də dərhal eyni dəyərlə `fulfilled` olur.
    * Əgər `p4` xəta ilə **rədd edilsə (rejected)**, `p2` də dərhal eyni xəta ilə `rejected` olur.

**Vizual sxem:**
```
.then(callback) ──> p_yeni (yeni Promise)
     │
     └── callback bir dəyər qaytarır: V
           │
           ├── Əgər V bir Promise DEYİLSƏ ──> p_yeni dərhal Fulfilled olur (dəyəri = V) ✅
           │
           └── Əgər V bir Promise-dirsə (p_daxili) ──> p_yeni Resolved olur (hələ Pending-dir) 🔗
                                                        │
                                                        └── p_daxili Fulfilled/Rejected olan kimi...
                                                              │
                                                              └──> p_yeni də eyni nəticə ilə Fulfilled/Rejected olur.
```

**Nümunəmizə Qayıdaq:**
1.  `c1` funksiyası `p4`-ü (`response.json()`-dan gələn Promise-i) qaytaran kimi, `p2` **həll olunur (resolved)**, amma hələ `pending` vəziyyətində qalır. O, `p4`-ün nəticəsini gözləyir.
2.  Arxa planda serverdən gələn məlumatın tam yüklənməsi bitir və `json()` metodu məzmunu emal edir. Nəticədə, `p4` JSON obyekti ilə **yerinə yetirilir (fulfilled)**.
3.  `p4` yerinə yetirildiyi anda, ona "kilidlənmiş" olan `p2` də avtomatik olaraq eyni JSON obyekti ilə **yerinə yetirilir (fulfilled)**.
4.  Və nəhayət, `p2` yerinə yetirildiyi üçün, onun dəyəri (JSON obyekti) `c2` funksiyasına ötürülür və zəncir davam edir.

***
### 13.2.4 Promise-lər və Xətalar: Dərin Baxış 🛡️

Əvvəlki bölmədə gördük ki, `.then()` metoduna ikinci bir funksiya ötürərək xətaları tutmaq olar. Lakin daha yaxşı və geniş yayılmış üsul, Promise zəncirinin sonuna bir `.catch()` metodu əlavə etməkdir.

**Niyə `.catch()` bu qədər vacibdir?**
Çünki sinxron (synchronous) kodda xəta baş verəndə proqram dərhal dayanır və biz xətanın yerini `stack trace`-dən görürük. Asinxron kodda isə, tutulmayan xətalar çox vaxt "səssizcə yox olur" və problemi tapmaq aylarla çəkə bilər. `.catch()` metodu bu səssiz xətaların qarşısını alır.

#### `.catch()` və `.finally()` Metodları

* **`.catch(callback)`**: Bu, əslində `.then(null, callback)` yazılışının qısa formasıdır. Adı `try...catch` blokundakı `catch`-ə bənzədiyi üçün daha anlaşıqlıdır. Zəncirin istənilən yerində baş verən bir xəta, sanki bir şəlalə kimi, aşağıya doğru axaraq ilk qarşısına çıxan `.catch()` bloku tərəfindən tutulur.

* **`.finally(callback)` (ES2018)**: Bu isə `try...catch...finally` blokundakı `finally`-ə bənzəyir. Promise-in **uğurlu (`fulfilled`) və ya uğursuz (`rejected`)** olmasından asılı olmayaraq, nəticədə **hər zaman** icra olunur. Adətən təmizlik işləri üçün istifadə olunur (məsələn, "loading" indikatorunu gizlətmək, fayl bağlantısını bağlamaq və s.). `.finally()` içindəki funksiya heç bir arqument qəbul etmir, çünki onun məqsədi nəticəni bilmək deyil, sadəcə prosesin bitdiyini təsdiqləməkdir.

#### Həyatdan Gələn Nümunə: Realistik `fetch` Sorğusu
Gəlin `fetch` sorğusunun bütün mümkün xəta hallarını nəzərə alan daha real bir nümunəyə baxaq.
```javascript
fetch("/api/user/profile") // 1. Sorğunu başlat
  .then(response => {
    console.log("Başlıqlar (headers) gəldi, status yoxlanılır...");

    // 2. HTTP statusu uğurlu deyil? (məsələn, 404 Not Found)
    if (!response.ok) {
      // Bu, birbaşa xəta deyil, amma istədiyimiz nəticə də deyil.
      // Zənciri dayandırmadan, sonrakı mərhələyə `null` ötürürük.
      return null;
    }

    // 3. Serverin bizə JSON göndərdiyindən əmin oluruqmu?
    const contentType = response.headers.get("content-type");
    if (!contentType || !contentType.includes("application/json")) {
      // Əgər server səhv formatda cavab göndəribsə, bu, ciddi bir xətadır.
      // Özümüz bir TypeError yaradıb "throw" edirik. Bu, birbaşa .catch() blokunu işə salacaq.
      throw new TypeError(`Gözlənilən format JSON idi, gələn isə: ${contentType}`);
    }

    // Əgər hər şey qaydasındadırsa, cavabın məzmununu JSON kimi emal edən Promise-i qaytarırıq.
    return response.json();
  })
  .then(profile => {
    // 4. Əvvəlki .then()-dən gələn nəticəni (JSON obyektini və ya null-ı) emal edirik.
    if (profile) {
      //displayUserProfile(profile);
      console.log("✅ Profil göstərilir:", profile);
    } else {
      //displayLoggedOutProfilePage();
      console.log("👤 İstifadəçi giriş etməyib və ya tapılmadı.");
    }
  })
  .catch(e => {
    // 5. BÜTÜN zəncir boyunca baş verən istənilən xəta burada tutulur.
    console.error("❗️ Ümumi xəta baş verdi:");
    
    if (e instanceof TypeError) {
      // Bu, bizim yuxarıda "throw" etdiyimiz xətadır.
      console.error("Server tərəfində konfiqurasiya xətası var.", e.message);
    } else if (e.name === 'AbortError') {
      // fetch sorğusu dayandırıldıqda bu baş verir
      console.warn("Sorğu dayandırıldı.");
    } else {
      // Bu isə daha çox şəbəkə problemi (internet kəsintisi və s.) ola bilər.
      console.error("Şəbəkə xətası. İnternet bağlantınızı yoxlayın.", e);
    }
  })
  .finally(() => {
      // 6. İstər uğurlu olsun, istərsə də xəta, bu blok HƏMİŞƏ işə düşür.
      console.log("🧼 Əməliyyat bitdi, təmizlik işləri aparılır (məs. loading indikatoru gizlədilir).");
  });
```

#### Zəncir Ortasında Xətanın Bərpası (Recovery)
`.catch()` həmişə zəncirin sonunda olmalı deyil. Bəzən bir mərhələdə baş verən xətanı tutub, onu bərpa edib, zəncirin davam etməsini təmin etmək olar.

**Nümunə: Xəta baş verdikdə standart bir dəyər qaytarmaq**
```javascript
function riskyOperation() {
  return new Promise((resolve, reject) => {
    if (Math.random() > 0.5) {
      resolve("Orijinal uğurlu nəticə");
    } else {
      reject(new Error("Əməliyyatda təsadüfi bir xəta baş verdi"));
    }
  });
}

riskyOperation()
  .catch(error => {
    console.warn(`⚠️ Xəta tutuldu, lakin proses davam edir. Səbəb: ${error.message}`);
    // Xətanı "uduruq" və zəncirin davam etməsi üçün standart bir dəyər qaytarırıq.
    return "Bərpa edilmiş standart dəyər"; 
  })
  .then(result => {
    // Yuxarıdakı .catch() xətanı tutub normal dəyər qaytardığı üçün bu .then() işə düşür.
    console.log("✅ Zəncir davam etdi. Gələn nəticə:", result);
  });
```

#### ❗️ Vacib Qeyd: Ox Funksiyası (`=>`) və `return`
Promise zəncirlərində ən çox edilən səhvlərdən biri, ox funksiyalarında fiqurlu mötərizə `{}` istifadə edərkən `return` açar sözünü unutmaqdır.

```javascript
// DÜZGÜN: fiqurlu mötərizə yoxdur, nəticə avtomatik return olunur.
promise.then(value => anotherPromise(value))

// SƏHV: fiqurlu mötərizə var, amma `return` yoxdur.
// Bu kod `undefined` qaytarır və növbəti .then() heç bir data almır.
promise.then(value => { anotherPromise(value); }) 

// DÜZGÜN (fiqurlu mötərizə ilə):
promise.then(value => { return anotherPromise(value); })
```

***
### 13.2.5 Promise-lərin Paralel İcrası (Promises in Parallel) 🏁

İndiyə qədər gördüyümüz zəncirlər (chains) asinxron əməliyyatları **ardıcıl** şəkildə yerinə yetirmək üçündür. Bəs əgər bir neçə asinxron əməliyyatı **eyni anda** başladaraq hamısının nəticəsini birlikdə gözləmək istəsək? Bunun üçün `Promise` obyektinin xüsusi statik metodları var.

#### `Promise.all()` — "Ya Hamısı, Ya Heç Biri"
`Promise.all()` bir massiv (array) dolusu Promise qəbul edir və nəticədə tək bir yeni Promise qaytarır.

**İşləmə Məntiqi:**
* **Uğur halı ✅:** Yalnız massivdəki **BÜTÜN** Promise-lər uğurla yerinə yetirildikdə (`fulfilled`), `Promise.all()`-un qaytardığı ümumi Promise da yerinə yetirilir. Nəticə olaraq, bütün Promise-lərin nəticələrindən ibarət bir massiv (array) qaytarır (eyni ardıcıllıqla).
* **Xəta halı ❌:** Massivdəki Promise-lərdən **HƏR HANSI BİRİ** rədd edilsə (`rejected`), `Promise.all()` dərhal rədd edilir və digərlərinin nəticəsini gözləmir.

**Geniş Nümunə: Eyni anda bir neçə fərqli məlumatı yükləmək**
Tutaq ki, bir istifadəçinin həm profilini, həm də postlarını eyni anda serverdən çəkmək istəyirik.
```javascript
// Müxtəlif URL-lərə sorğu göndərən Promise-lər yaradırıq
const fetchProfile = fetch('/api/user/profile').then(r => r.json());
const fetchPosts = fetch('/api/user/posts').then(r => r.json());
const fetchSettings = fetch('/api/user/settings').then(r => r.json());

console.log("Üç sorğu da eyni anda göndərildi...");

Promise.all([fetchProfile, fetchPosts, fetchSettings])
  .then(([profile, posts, settings]) => {
    // Bu blok yalnız HƏR ÜÇ sorğu uğurlu olduqda işə düşəcək
    console.log("✅ Bütün məlumatlar uğurla alındı!");
    // Nəticələri destructuring ilə rahatlıqla əldə edirik
    console.log("Profil:", profile);
    console.log("Postlar:", posts);
    console.log("Ayarlar:", settings);
  })
  .catch(error => {
    // Sorğulardan HƏR HANSI BİRİ xəta versə, bu blok işə düşəcək
    console.error("❗️ Sorğulardan birində xəta baş verdi:", error);
  });
```

#### `Promise.allSettled()` — "Hər Kəsin Nəticəsini Gözlə" (ES2020)
`Promise.all()`-dan fərqli olaraq, `Promise.allSettled()` heç vaxt rədd edilmir (`rejected`). O, massivdəki bütün Promise-lərin bitməsini (istər uğurlu, istər uğursuz) gözləyir və hər birinin nəticəsi haqqında detallı məlumat qaytarır. Bu, bəzi əməliyyatlar xəta versə belə, digərlərinin nəticəsini itirməmək üçün idealdır.

**Nümunə: Uğurlu və uğursuz sorğuların nəticələrini birlikdə almaq**
```javascript
const p1 = Promise.resolve("Uğurlu nəticə");
const p2 = new Promise((resolve, reject) => setTimeout(() => reject("Xəta baş verdi!"), 1000));
const p3 = Promise.resolve("Başqa bir uğurlu nəticə");

Promise.allSettled([p1, p2, p3])
  .then(results => {
    console.log("Bütün əməliyyatlar bitdi. Nəticələr:");
    results.forEach(result => {
      if (result.status === 'fulfilled') {
        console.log(`✅ Uğurlu: Dəyər = ${result.value}`);
      } else {
        console.log(`❌ Uğursuz: Səbəb = ${result.reason}`);
      }
    });
  });

// Nəticə:
// Bütün əməliyyatlar bitdi. Nəticələr:
// ✅ Uğurlu: Dəyər = Uğurlu nəticə
// ❌ Uğursuz: Səbəb = Xəta baş verdi!
// ✅ Uğurlu: Dəyər = Başqa bir uğurlu nəticə
```

#### `Promise.race()` — "İlk Çatan Qalibdir" 🏆
`Promise.race()` də bir massiv dolusu Promise qəbul edir, lakin o, hamısının bitməsini gözləmir. Massivdəki Promise-lərdən **hansı birinci həll olunsa (settled)** – istər uğurla (`fulfilled`), istərsə də xəta ilə (`rejected`) – `Promise.race()` dərhal həmin nəticə ilə yekunlaşır.

**Nümunə: Hansı server daha sürətli cavab verəcək?**
Təsəvvür edək ki, eyni məlumatı iki fərqli serverdən almaq imkanımız var və bizə ən sürətli cavab verən lazımdır.
```javascript
const fastServer = new Promise(resolve => setTimeout(() => resolve("Sürətli serverdən cavab"), 1000));
const slowServer = new Promise(resolve => setTimeout(() => resolve("Yavaş serverdən cavab"), 3000));

Promise.race([fastServer, slowServer])
  .then(winnerResult => {
    console.log("Qalib gələn nəticə:", winnerResult);
  })
  .catch(error => {
    console.error("İlk cavab verən xəta qaytardı:", error);
  });

// ✅ Nəticə (1 saniyə sonra): Qalib gələn nəticə: Sürətli serverdən cavab
```
Bu metod həmçinin, bir sorğunun müəyyən bir müddətdən çox çəkəcəyi təqdirdə onu ləğv etmək üçün "timeout" məntiqi qurmaqda da çox istifadə olunur.

***
### 13.2.6 Öz Promise-lərimizi Yaratmaq (Making Promises) 🛠️

İndiyə qədər `fetch()` kimi hazır, Promise qaytaran funksiyalardan istifadə etdik. Bəs özümüz belə funksiyaları necə yarada bilərik? Bunun bir neçə yolu var.

#### Üsul 1: Başqa Promise-lərə Əsaslanaraq 🔗
Əgər əlinizdə artıq bir Promise qaytaran funksiya varsa, onun üzərində `.then()` zənciri quraraq yeni və daha spesifik bir Promise qaytaran funksiya yaratmaq çox asandır.

**Nümunə 1: `fetch`-dən istifadə edərək `getJSON()` yaratmaq**
`fetch` sorğusunun nəticəsini birbaşa JSON-a çevirən bir funksiya yaradaq.
```javascript
function getJSON(url) {
  // fetch() bir Promise qaytarır, biz də onun nəticəsi üzərində .then() çağırırıq
  // response.json() da özü bir Promise qaytardığı üçün, bu zəncir bizə lazım olanı edir.
  return fetch(url).then(response => response.json());
}
```

**Nümunə 2: Yaratdığımız `getJSON()` ilə yeni bir funksiya yaratmaq**
İndi isə, istifadəçinin profilindən onun ən yüksək xalını (high score) gətirən bir funksiya yazaq.
```javascript
function getHighScore() {
  // Öz getJSON funksiyamızdan istifadə edərək zənciri davam etdiririk
  return getJSON("/api/user/profile").then(profile => profile.highScore);
}

// İstifadəsi:
getHighScore()
  .then(score => console.log("Ən yüksək xal:", score))
  .catch(err => console.error("Xal alına bilmədi:", err));
```

---
#### Üsul 2: Sinxron Dəyərlərdən Promise Yaratmaq 🎁
Bəzən bir funksiya API-a görə mütləq Promise qaytarmalıdır, amma nəticə artıq əlimizdədir (sinxron). Bu halda, `Promise.resolve()` və `Promise.reject()` köməyə gəlir.

* **`Promise.resolve(value)`**: Dərhal yerinə yetirilən (`fulfilled`) və verilmiş dəyəri daşıyan bir Promise yaradır.
* **`Promise.reject(error)`**: Dərhal rədd edilən (`rejected`) və verilmiş xətanı daşıyan bir Promise yaradır.

**Nümunə: Funksiya arqumentlərini yoxlamaq**
Asinxron əməliyyata başlamazdan əvvəl arqumentləri yoxlayıb, səhv olduqda dərhal rədd edilmiş (rejected) bir Promise qaytarmaq yaxşı təcrübədir.
```javascript
function getUserData(userId) {
  // Arqumenti sinxron olaraq yoxlayırıq
  if (typeof userId !== 'number' || userId <= 0) {
    // Səhvdirsə, dərhal rədd edilmiş bir Promise qaytarırıq
    return Promise.reject(new Error("Keçərsiz istifadəçi ID-si!"));
  }
  
  // Əgər ID düzgündürsə, əsl asinxron əməliyyatı başladırıq
  return getJSON(`/api/users/${userId}`);
}

getUserData(-5) // Səhv ID göndəririk
  .then(user => console.log(user))
  .catch(error => console.error("❌ Xəta:", error.message)); 
  // ✅ Nəticə: Xəta: Keçərsiz istifadəçi ID-si!
```

---
#### Üsul 3: Sıfırdan Yaratmaq (`Promise` Konstruktoru)
Ən fundamental yol, köhnə callback-əsaslı və ya hadisə-yönümlü (event-based) asinxron API-ları Promise-ə çevirmək üçün `new Promise()` konstruktorundan istifadə etməkdir.

**Sintaksis:** `new Promise((resolve, reject) => { ... })`
Konstruktora bir **"icraçı" (executor)** funksiya ötürülür. Bu funksiya iki arqument qəbul edir:
* **`resolve(value)`**: Asinxron əməliyyat uğurlu olduqda çağırılmalı olan funksiya. Promise-i yerinə yetirir (`fulfills`).
* **`reject(error)`**: Əməliyyatda xəta baş verdikdə çağırılmalı olan funksiya. Promise-i rədd edir (`rejects`).

**Nümunə 1: `setTimeout`-u Promise-ə çevirmək (`wait` funksiyası)**
```javascript
function wait(duration) {
  // Yeni bir Promise yaradıb onu qaytarırıq
  return new Promise((resolve, reject) => {
    // Əvvəlcə arqumenti yoxlayırıq
    if (duration < 0) {
      reject(new Error("Zaman yalnız irəli gedə bilər!"));
    }

    // Əks halda, setTimeout ilə resolve funksiyasını gecikmə ilə çağırırıq
    // setTimeout, verilən müddət bitdikdən sonra resolve() funksiyasını çağıracaq
    // və Promise yerinə yetirilmiş olacaq.
    setTimeout(resolve, duration);
  });
}

console.log("Gözləmə başlayır...");
wait(2000)
  .then(() => {
    console.log("✅ 2 saniyə keçdi!");
  })
  .catch(err => {
    console.error(err.message);
  });
```

**Nümunə 2: Node.js-in callback-əsaslı `http` modulunu Promise-ə çevirmək**
Bu, sıfırdan Promise yaratmağın real həyatdakı gücünü göstərən geniş bir nümunədir.
```javascript
const http = require("http");

function getJSON(url) {
  return new Promise((resolve, reject) => {
    const request = http.get(url, response => {
      if (response.statusCode !== 200) {
        reject(new Error(`HTTP Status: ${response.statusCode}`));
        response.resume(); // Yaddaş sızmasının qarşısını alır
        return;
      }
      
      let body = "";
      response.setEncoding("utf-8");
      response.on("data", chunk => { body += chunk; });
      response.on("end", () => {
        try {
          const parsed = JSON.parse(body);
          resolve(parsed); // Uğurlu halda Promise-i nəticə ilə resolve edirik
        } catch(e) {
          reject(e); // JSON parse xətası olarsa reject edirik
        }
      });
    });

    // Aşağı səviyyəli şəbəkə xətaları üçün
    request.on("error", error => {
      reject(error);
    });
  });
}
```

***
### 13.2.7 Promise-lərin Ardıcıl İcrası (Promises in Sequence) ⛓️
`Promise.all()` bir neçə Promise-i **paralel** olaraq icra etmək üçün əladır. Bəs əgər biz bir massiv (array) dolusu URL-i şəbəkəni yükləməmək üçün **bir-birinin ardınca**, yəni birincisi bitəndən sonra ikincisini çağırmaq istəsək necə?

Bu, bir az daha mürəkkəb bir məsələdir, çünki zənciri əvvəlcədən yox, dinamik olaraq qurmalıyıq. Bunun üçün iki əsas yanaşma var.

#### Üsul 1: Dinamik Zəncir Qurmaq (Domino Effekti  domino️)
Bu yanaşmanın məntiqi, əvvəlcədən bütün zənciri bir-birinə bağlayıb, sonra ilk halqanı işə salmaqdır. Sanki dominoları bir-birinin ardınca düzüb, sonra birincisini itələyirik.

**Nümunə: `fetchSequentially` funksiyası**
```javascript
function fetchSequentially(urls) {
  // Nəticələri (cavabların məzmununu) yığmaq üçün bir massiv
  const bodies = [];

  // Verilmiş tək bir URL üçün sorğu göndərən və nəticəni `bodies`-ə əlavə edən funksiya
  function fetchOne(url) {
    return fetch(url)
      .then(response => response.text())
      .then(body => {
        // Nəticəni massivə əlavə edirik. Buradan bir dəyər return etmirik.
        bodies.push(body);
      });
  }

  // 1. Zənciri başlamaq üçün dərhal yerinə yetirilən bir başlanğıc Promise-i yaradırıq.
  let p = Promise.resolve(undefined);

  // 2. Hər bir URL üçün dövr edərək zəncirimizi dinamik olaraq uzadırıq.
  for (const url of urls) {
    // Hər dövrdə, mövcud zəncirin sonuna yeni bir .then() bloku əlavə edirik.
    // Bu blok, özündən əvvəlki bitdikdən sonra növbəti URL üçün sorğu göndərəcək.
    p = p.then(() => fetchOne(url));
  }

  // 3. Bütün dövr bitdikdən sonra, `p` bizim ən sonuncu Promise-imiz olur.
  // Onun da işi bitdikdən sonra, nəticə olaraq yığılmış `bodies` massivini qaytarırıq.
  return p.then(() => bodies);
}

// İstifadəsi:
const urls = ["/api/url1", "/api/url2", "/api/url3"]; // Təsəvvür edilən URL-lər
fetchSequentially(urls)
  .then(allBodies => {
    console.log("✅ Bütün URL-lər ardıcıl olaraq yükləndi. Nəticələr:", allBodies);
  })
  .catch(error => {
    console.error("❗️ Ardıcıl yükləmə zamanı xəta baş verdi:", error);
  });
```

#### Üsul 2: "Rekursiv" Zəncir (Matryoshka Kuklası Effekti 🪆)
Bu yanaşma daha zərif, lakin anlamaq üçün bir az daha mürəkkəbdir. Burada hər Promise həll olunduqda, öz içindən növbəti addımın Promise-ini qaytarır. Sanki hər Matryoshka kuklasının içindən başqa bir kukla çıxır.

Bu funksiya daha ümumi (generic) olduğu üçün istənilən ardıcıl Promise əməliyyatı üçün istifadə edilə bilər.

**Nümunə: `promiseSequence` funksiyası**
```javascript
/**
 * Bu funksiya bir massiv dolusu "giriş" dəyəri və bir "promiseMaker" funksiyası qəbul edir.
 * Hər bir giriş dəyəri üçün `promiseMaker(input)` funksiyasını çağıraraq yeni bir Promise yaradır
 * və bütün prosesi ardıcıl şəkildə edir.
 */
function promiseSequence(inputs, promiseMaker) {
  // Massivin dəyişdirilə bilən bir kopyasını yaradırıq
  inputs = [...inputs];

  // Bu, bütün sehri edən daxili, "rekursiv" funksiyadır
  function handleNextInput(outputs) {
    // Əgər emal ediləcək başqa bir giriş qalmayıbsa...
    if (inputs.length === 0) {
      // ...nəticə massivini qaytarırıq və bütün zəncir tamamlanır.
      return outputs;
    } else {
      // Əgər hələ də giriş varsa...
      const nextInput = inputs.shift(); // Növbəti girişi massivdən götürürük
      
      // Həmin giriş üçün yeni bir Promise yaradırıq
      const nextPromise = promiseMaker(nextInput);
      
      // Bu Promise həll olunduqda, onun nəticəsini `outputs` massivinə əlavə edirik
      // və prosesi yeni `outputs` massivi ilə yenidən çağırırıq.
      // Bu, əsl rekursiya deyil, çünki hər çağırış asinxron .then() içində baş verir.
      return nextPromise
        .then(output => outputs.concat(output))
        .then(handleNextInput);
    }
  }

  // Prosesi boş bir nəticə massivi ilə başlayırıq
  return Promise.resolve([]).then(handleNextInput);
}

// İstifadəsi:
function fetchBody(url) { 
  console.log(`Sorğu göndərilir: ${url}`);
  return fetch(url).then(r => r.text()); 
}

promiseSequence(urls, fetchBody)
  .then(bodies => {
    console.log("✅ Bütün URL-lər ardıcıl yükləndi (2-ci üsul). Nəticələr:", bodies);
  })
  .catch(console.error);
```

***
### 13.3 `async` və `await` 🪄

ES2017-də təqdim edilən `async` və `await` açar sözləri, asinxron proqramlaşdırmada bir inqilab etdi. Bu iki açar söz, Promise-lərə əsaslanan kodu elə bir hala gətirir ki, sanki proqram şəbəkə cavabını və ya başqa bir hadisəni gözləyərkən "dayanır".

Əslində arxa planda hələ də Promise-lər işləyir, lakin `async/await` onların mürəkkəbliyini gizlədərək bizə çox təmiz və oxunaqlı kod yazmaq imkanı verir.

* Bir Promise-in uğurlu nəticəsi (`fulfilled value`), sinxron funksiyanın `return` dəyərinə bənzəyir.
* Bir Promise-in xətası (`rejected reason`), sinxron funksiyanın `throw` etdiyi xətaya bənzəyir.

`async/await` bu bənzərliyi götürüb, onu birbaşa sintaksisə çevirir.

---
### 13.3.1 `await` İfadəsi (Expression)

`await` açar sözü bir Promise-i götürür və onu ya bir **qaytarılan dəyərə**, ya da bir **xətaya** çevirir.
`await promise` ifadəsi, `promise` həll olunana (settled) qədər funksiyanın icrasını **dayandırır (pause)**.

* Əgər `promise` uğurla yerinə yetirilərsə (`fulfilled`), `await promise` ifadəsinin nəticəsi həmin uğurlu dəyər olur.
* Əgər `promise` rədd edilərsə (`rejected`), `await promise` ifadəsi həmin xətanı "atır" (`throws`).

**Nümunə:**
```javascript
// Bu kod ancaq bir `async` funksiyanın daxilində işləyir!
console.log("Sorğu göndərilir...");

// 1. `await` fetch-in qaytardığı Promise-in həll olmasını gözləyir
const response = await fetch("/api/user/profile");

// 2. `await` response.json()-dan gələn Promise-in həll olmasını gözləyir
const profile = await response.json();

console.log("✅ Nəticə alındı:", profile);
```
**❗️ Vacib Məqam:** `await` bütün proqramı "dondurmur". O, yalnız içində olduğu `async` funksiyanı dayandırır. Bu müddətdə JavaScript arxa planda başqa işləri görməyə davam edə bilər.

---
### 13.3.2 `async` Funksiyaları (Functions) ✨

`await`-in ən vacib qaydası budur:

❗️ **Qızıl Qayda:** `await` açar sözü **yalnız `async` ilə işarələnmiş funksiyaların daxilində** istifadə edilə bilər.

`async` açar sözü bir funksiyanın qarşısında yazıldıqda iki əsas şeyi edir:
1.  Həmin funksiyanın **həmişə bir Promise qaytarmasını** təmin edir.
2.  Funksiyanın daxilində `await`-dən istifadə etməyə imkan verir.

**`async` funksiyası necə Promise qaytarır?**
* Əgər `async` funksiya bir dəyər `return` edərsə, funksiyanın qaytardığı Promise həmin dəyərlə **yerinə yetirilir (fulfilled)**.
* Əgər `async` funksiya bir xəta `throw` edərsə, funksiyanın qaytardığı Promise həmin xəta ilə **rədd edilir (rejected)**.

**Nümunə 1: `async/await` ilə `getHighScore` funksiyası**
Gəlin əvvəlki bölmələrdəki `getHighScore` funksiyasını `async/await` ilə yenidən yazaq.
```javascript
async function getHighScore() {
  try {
    console.log("Profil məlumatları çəkilir...");
    const response = await fetch("/api/user/profile");
    
    // Xəta yoxlaması
    if (!response.ok) {
      throw new Error(`HTTP xətası! Status: ${response.status}`);
    }

    const profile = await response.json();
    console.log("Profil uğurla alındı!");
    
    return profile.highScore; // Bu dəyər, funksiyanın qaytardığı Promise-in nəticəsi olacaq
  } catch (error) {
    console.error("Ən yüksək xalı almaq mümkün olmadı:", error);
    throw error; // Xətanı yenidən "throw" edirik ki, funksiyanı çağıran kod onu tuta bilsin
  }
}

// İstifadəsi:
// `getHighScore` async olduğu üçün, onun nəticəsini ya .then() ilə, ya da başqa bir async funksiya içində await ilə gözləməliyik.
getHighScore()
  .then(score => console.log(`🏆 Ən yüksək xal: ${score}`))
  .catch(err => console.log("Xəta ilə nəticələndi."));
```
Gördüyünüz kimi, `try...catch` bloku adi sinxron kodda olduğu kimi işləyir! Bu, `.catch()` zəncirindən daha oxunaqlıdır.

---
### 13.3.3 Birdən Çox Promise-i Gözləmək
Əgər bir neçə asinxron əməliyyatı bir-birinin ardınca `await` ilə çağırsanız, onlar ardıcıl icra olunacaq və bu, səmərəli deyil.

```javascript
// ❌ SƏHV YANAŞMA (yavaş işləyir)
console.log("Məlumatlar ardıcıl çəkilir...");
const user = await getJSON(userUrl); // Bu bitənə qədər alt sətir başlamır
const posts = await getJSON(postsUrl); // Bu da yuxarıdakını gözləyir
console.log("Ardıcıl çəkmə bitdi.");
```

Əgər bu sorğular bir-birindən asılı deyilsə, onları paralel icra etmək üçün `Promise.all()` istifadə etməliyik. `await` bunu da çox asanlaşdırır.

```javascript
// ✅ DÜZGÜN YANAŞMA (sürətli işləyir)
console.log("Məlumatlar paralel çəkilir...");
const [user, posts] = await Promise.all([
  getJSON(userUrl),
  getJSON(postsUrl)
]);
console.log("Paralel çəkmə bitdi.");
```
Burada hər iki `getJSON` sorğusu eyni anda başlayır və `await` onların hər ikisinin bitməsini gözləyir.

---
### 13.3.4 Pərdə Arxası (Implementation Details) ⚙️
`async/await`-in necə işlədiyini anlamaq üçün onun arxa planda nə etdiyini bilmək faydalıdır. Qısacası:
* **`async function`**: Yazdığınız funksiyanın gövdəsini bir `Promise` içinə "bükür".
* **`await`**: Funksiyanın gövdəsini hissələrə ayırır. Hər `await`-dən sonrakı hissə, əslində `await`-in gözlədiyi Promise-in `.then()` metoduna verilən bir callback funksiyasıdır.

Yəni `async/await` Promise-ləri yox etmir, sadəcə onları istifadə etmək üçün çox daha rahat və oxunaqlı bir sintaksis ("sintaktik şəkər") təqdim edir. Bu, asinxron JavaScript kodunu yazmağın müasir standartıdır.

***
### 13.4 Asinxron İterasiya (Asynchronous Iteration) 🔁
Əvvəlki bölmələrdə öyrəndik ki, `Promise`-lər **tək bir** asinxron əməliyyatın gələcək nəticəsi üçündür. Bəs əgər bizə bir dəfəyə deyil, zamanla, hissə-hissə gələn bir data axını (stream of data) ilə işləmək lazımdırsa? Məsələn, böyük bir faylı oxumaq, real-time bildirişləri qəbul etmək və ya bir veb-soketdən (WebSocket) gələn mesajları dinləmək kimi.

Bu cür təkrarlanan asinxron hadisələr üçün nə sadə Promise-lər, nə də `async/await` birbaşa işləmir.

**Həll yolu:** ES2018-də təqdim edilən **Asinxron İteratorlar (Asynchronous Iterators)** və onlarla işləmək üçün yaradılmış yeni **`for/await...of`** dövrü.

---
### 13.4.1 `for/await` Dövrü (The `for/await` Loop) ⏳

`for/await...of` dövrü, adi `for...of` dövrünün asinxron versiyasıdır. O, üzərində dövr etdiyi mənbədən hər bir elementi götürür, onun həll olunmasını (`await` ilə) gözləyir və sonra dövrün gövdəsini icra edir.

❗️ **Qızıl Qayda:** Adi `await` kimi, `for await` da yalnız `async` ilə işarələnmiş funksiyaların daxilində işləyir.

**Nümunə 1: Node.js ilə böyük bir faylı hissə-hissə oxumaq**
Təsəvvür edək ki, çox böyük bir log faylımız var və onu tam yaddaşa yükləmədən, sətir-sətir (və ya hissə-hissə) emal etmək istəyirik.
```javascript
const fs = require("fs");

async function processFile(filename) {
  // Faylı oxumaq üçün bir "axın" (stream) yaradırıq
  const stream = fs.createReadStream(filename, { encoding: "utf-8" });

  console.log("Fayl oxunmağa başlayır...");

  // for/await dövrü, stream-dən hər yeni bir "hissə" (chunk) gəldikcə onu gözləyir
  for await (const chunk of stream) {
    // parseChunk(chunk); // Hər gələn hissəni emal edirik
    console.log(`--- YENİ HİSSƏ GƏLDİ (uzunluğu: ${chunk.length}) ---`);
  }
  
  console.log("✅ Faylın oxunması tamamilə bitdi.");
}
```

**Nümunə 2: Bir massiv (array) dolusu Promise üzərində dövr etmək**
`for/await` həmçinin, daxilində Promise-lər olan adi, sinxron bir massiv (array) üzərində də işləyə bilir.

Tutaq ki, bir neçə URL-ə eyni anda sorğu göndərib, cavabları gəldikcə emal etmək istəyirik.
```javascript
const urls = ["/api/url1", "/api/url2", "/api/url3"];
// Hər URL üçün bir fetch Promise-i yaradıb, yeni bir massivə yığırıq
const promises = urls.map(url => fetch(url));

// ADİ for...of ilə: await-i dövrün içində yazmalıyıq
async function normalForLoop() {
  for (const promise of promises) {
    const response = await promise; // Hər bir promise-in həll olmasını gözləyirik
    // handle(response);
  }
}

// for/await...of ilə: await dövrün özündədir, kod daha səliqəli görünür
async function forAwaitLoop() {
  for await (const response of promises) {
    // Bu dövr hər bir promise-in nəticəsini avtomatik olaraq "await" edir
    // handle(response);
  }
}
```
Bu konkret nümunədə hər iki dövr eyni işi görür. Amma `for/await`-in əsl gücü, əsl asinxron iteratorlarla işlədikdə ortaya çıxır.

---
### 13.4.2 Asinxron Iteratorlar (Asynchronous Iterators) 📜

Gəlin Fəsil 12-dəki iteratorları xatırlayaq. Bir obyektin iterable olması üçün onun `[Symbol.iterator]` metodu olmalı idi. Həmin metod isə `.next()` metodu olan bir iterator obyekti qaytarmalı idi.

Asinxron iteratorlar da çox bənzərdir, amma **iki fundamental fərqi var:**

1.  **Symbol ➡️**: Asinxron iterable obyekt `[Symbol.iterator]` deyil, **`[Symbol.asyncIterator]`** adlı bir metoda sahib olmalıdır.
2.  **`.next()` Metodunun Qaytardığı Dəyər ➡️**: Adi iteratorun `.next()` metodu birbaşa `{ value, done }` obyektini qaytarırdı. Asinxron iteratorun `.next()` metodu isə, bu obyekti bir **Promise-in içində** qaytarır: `Promise< { value, done } >`.

**Vizual Fərq:**
* **Sinxron Iterator:** `.next()  ──>  { value: 'A', done: false }`
* **Asinxron Iterator:** `.next()  ──>  Promise  ──(həll olunur / fulfilled)──>  { value: 'A', done: false }`

Bu o deməkdir ki, əsl asinxron iteratorlarda təkcə dəyərin özü deyil, hətta iterasiyanın bitib-bitməməsi (`done` xüsusiyyəti) belə asinxron olaraq müəyyən edilə bilər. `for/await` dövrü məhz bu `Promise< { value, done } >` strukturunu anlamaq və onunla işləmək üçün yaradılıb.


***
### 13.4.3 Asinxron Generatorlar (Asynchronous Generators) 🪄
Əvvəlki fəsildə gördüyümüz kimi, iterator yaratmağın ən asan yolu generatorlardan istifadə etməkdir. Eyni məntiq asinxron iteratorlar üçün də keçərlidir. Onları **asinxron generatorlar (asynchronous generators)** ilə yarada bilərik.

Asinxron generator, həm `async` funksiyanın, həm də `generator` funksiyasının xüsusiyyətlərini özündə birləşdirir:
* **Sintaksis:** `async function*`
* **İmkanlar:** Daxilində həm `await` (gözləmək üçün), həm də `yield` (dəyər ötürmək üçün) istifadə edə bilərsiniz.

`yield` ilə ötürdüyünüz dəyərlər, asinxron iterator protokoluna uyğun olması üçün avtomatik olaraq Promise-ə "bükülür".

**Geniş Nümunə: `setInterval` əvəzinə Asinxron Generator ilə Saat**
`setInterval` və callback yerinə, gəlin `for await` dövrü ilə işləyən bir saniyə sayğacı düzəldək.
```javascript
// 1. setTimeout-u Promise-ə çevirən bir köməkçi (helper) funksiya
function wait(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// 2. Hər `interval` millisaniyədən bir sayğacı artıran və `yield` edən asinxron generator
async function* createClock(interval, maxTicks = Infinity) {
  for (let tick = 1; tick <= maxTicks; tick++) {
    // Növbəti addıma keçməzdən əvvəl verilən interval qədər gözləyirik
    await wait(interval);
    // Sayğacın hazırkı dəyərini "ötürürük"
    yield tick;
  }
}

// 3. Asinxron generatorumuzu `for await` ilə istifadə edən test funksiyası
async function runClock() {
  console.log("Saat başladıldı... 🕔");
  
  // `createClock` generatoru hər saniyə bir "tick" dəyəri verəcək
  // və `for await` dövrü hər dəyəri gəldikcə götürəcək.
  // 5 dəfə işləyib dayanacaq (maxTicks=5).
  for await (const tick of createClock(1000, 5)) {
    console.log(`Tick: ${tick}`);
  }
  
  console.log("Saat dayandı. 🏁");
}

runClock();
```

---
### 13.4.4 Asinxron Iteratorların Yaradılması (Implementing Asynchronous Iterators) ⚙️
Asinxron iteratorları generatorsuz, "əl ilə" də yaratmaq mümkündür. Bu, daha çox nəzarət imkanı verir, amma daha mürəkkəbdir. Bunun üçün `[Symbol.asyncIterator]` metodu olan bir obyekt yaratmalıyıq. Həmin obyektin `next()` metodu isə `async` olmalı və `Promise< {value, done} >` qaytarmalıdır.

**Geniş Nümunə: `AsyncQueue` Sinifi (Class)**
Ən çətin, amma ən güclü nümunələrdən biri, istənilən hadisə mənbəyini (event source) asinxron bir axına (stream) çevirə bilən ümumi (generic) bir "növbə" (queue) sinifi yaratmaqdır. Bu sinif arxa planda bütün mürəkkəb məntiqi həll edir.

Aşağıdakı `AsyncQueue` sinifi, `dequeue()` (növbədən çıxarma) metodu çağırıldıqda bir Promise qaytarır. Bu o deməkdir ki, dəyərlər növbəyə əlavə olunmamışdan əvvəl belə onları "tələb etmək" olar.

```javascript
// Bu sinifin detallı izahı çox uzundur, ancaq əsas məqsədi
// asinxron gələn dəyərləri bir növbəyə yığıb, for/await ilə istifadə etməyə imkan verməkdir.
class AsyncQueue {
  constructor() {
    this.values = [];     // Gələn dəyərlər üçün növbə
    this.resolvers = [];  // Dəyər gəlməmiş tələb olunan Promise-lərin resolve funksiyaları
    this.closed = false;
  }
  enqueue(value) {
    if (this.closed) throw new Error("AsyncQueue closed");
    if (this.resolvers.length > 0) this.resolvers.shift()(value);
    else this.values.push(value);
  }
  dequeue() {
    if (this.values.length > 0) return Promise.resolve(this.values.shift());
    if (this.closed) return Promise.resolve(AsyncQueue.EOS);
    return new Promise(resolve => { this.resolvers.push(resolve); });
  }
  close() {
    while (this.resolvers.length > 0) this.resolvers.shift()(AsyncQueue.EOS);
    this.closed = true;
  }
  [Symbol.asyncIterator]() { return this; }
  next() {
    return this.dequeue().then(value => (value === AsyncQueue.EOS)
      ? { value: undefined, done: true }
      : { value, done: false });
  }
}
AsyncQueue.EOS = Symbol("end-of-stream"); // Növbənin bitdiyini göstərən xüsusi işarə
```

**Bu mürəkkəb sinifdən necə sadə istifadə etmək olar?** 🤔
İndi bu sinifin köməyi ilə brauzerdəki istənilən klik hadisəsini asan bir `for/await` dövrünə çevirək.

**Nümunə: Klik hadisələrini axın kimi emal etmək**
```javascript
// Verilmiş elementin üzərindəki hadisələri AsyncQueue-ya əlavə edən köməkçi funksiya
function eventStream(element, eventType) {
  const q = new AsyncQueue(); // Növbəmizi yaradırıq
  // Hadisə baş verdikcə, hadisə obyektini növbəyə əlavə edirik
  element.addEventListener(eventType, e => q.enqueue(e));
  return q;
}

// İndi isə klikləri asinxron bir dövr ilə tutaq
async function handleClicks() {
  console.log("🖱️ Klikləri gözləyirəm... (çıxmaq üçün səhifədə 5 dəfə klikləyin)");
  
  let clickCount = 0;
  // `document` üzərindəki hər bir klik üçün dövr edirik
  for await (const event of eventStream(document, "click")) {
    clickCount++;
    console.log(`Klik nömrə ${clickCount}! Koordinatlar: X=${event.clientX}, Y=${event.clientY}`);
    if (clickCount === 5) {
      console.log("5 klik edildi, dövr dayandırılır.");
      break; // Dövrü dayandırırıq
    }
  }
}

// handleClicks(); // Bu funksiyanı brauzer konsolunda işə salsanız, hər klikdə mesaj görəcəksiniz.
```