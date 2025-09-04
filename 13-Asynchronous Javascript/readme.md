### Fəsil 13. Asinxron (Asynchronous) JavaScript

Əksər proqramlar fasiləsiz işləmir. Onlar çox vaxt bir hadisənin baş verməsini gözləyir: istifadəçinin düyməyə klikləməsi, şəbəkədən (network) məlumatın gəlməsi və ya müəyyən bir vaxtın keçməsi.  
**Asinxron proqramlaşdırma**, proqramın bu gözləmə müddətində "donub" qalmaması, əksinə, başqa işləri görməyə davam etməsi üçün bir üsuldur.

Bu fəsildə asinxron əməliyyatları idarə etmək üçün JavaScript-in 3 əsas alətini öyrənəcəyik:

1.  **Promises**: Asinxron bir əməliyyatın gələcəkdəki nəticəsini təmsil edən obyektlər.
2.  **`async/await`**: `Promise`-lərə əsaslanan, lakin kodu sanki sinxron (synchronous) imiş kimi yazmağa imkan verən rahat sintaksis.
3.  **Asinxron Iteratorlar və `for/await`**: Asinxron data axınları (streams) ilə işləmək üçün dövrlər

JavaScript dilinin özündə asinxron bir şey yoxdur. Bu xüsusiyyətləri anlamaq üçün biz əvvəlcə brauzer və Node.js tərəfindən təmin edilən asinxron API-lara (məsələn, zamanlayıcılar, hadisələr) baxmalıyıq.

---

### 13.1 "Callback"-lər ilə Asinxron Proqramlaşdırma

JavaScript-də asinxronluğun ən fundamental səviyyəsi **"callback"** funksiyalarıdır.

**Callback nədir?** 🤔
Bu, yazdığınız və başqa bir funksiyaya arqument kimi ötürdüyünüz bir funksiyadır. Həmin funksiya, müəyyən bir şərt ödəndikdə və ya asinxron bir hadisə baş verdikdə sizin funksiyanızı "geri çağırır".

---

### 13.1.1 Zamanlayıcılar (Timers) ⏳

JavaScript-də asinxronluğun ən sadə forması kodun müəyyən müddətdən sonra və ya müntəzəm aralıqlarla işlədilməsidir. Bu iş üçün iki əsas funksiya var:

- **`setTimeout`** → verilən funksiyanı yalnız _bir dəfə_, gecikmə (millisaniyə ilə) sonra işlədir.
- **`setInterval`** → verilən funksiyanı göstərilən aralıqlarla _davamlı_ olaraq işlədir.

Hər iki funksiya **ID** qaytarır. Bu ID sonradan `clearTimeout` və ya `clearInterval` ilə əməliyyatı dayandırmaq üçün istifadə olunur.

---

#### 🔹 Nümunə 1: `setTimeout` ilə birdəfəlik gecikmə

```javascript
function sayHello() {
  console.log("👋 Salam, 3 saniyə keçdi!");
}

console.log("Zamanlayıcı quruldu...");

// sayHello funksiyasını 3000 ms (3 saniyə) sonra icra etmək üçün planlaşdırırıq
setTimeout(sayHello, 3000);

console.log(
  "Zamanlayıcı qurulandan sonraki kod... (gördüyünüz kimi bu kod gözləmir)"
);
```

**Konsol çıxışı:**

```
Zamanlayıcı quruldu...
Zamanlayıcı qurulandan sonraki kod... (gördüyünüz kimi bu kod gözləmir)
👋 Salam, 3 saniyə keçdi!
```

📌 Gördüyün kimi, sonuncu sətir 3 saniyə gecikmə ilə gəlir.

---

#### 🔹 Nümunə 2: `setInterval` ilə təkrarlanan əməliyyat

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

**Konsol çıxışı:**

```
Periodik yoxlama başladıldı (hər 3 saniyədən bir)...
Bildirişlər yoxlanılır... (dəfə 1)
Bildirişlər yoxlanılır... (dəfə 2)
Bildirişlər yoxlanılır... (dəfə 3)
✅ Periodik yoxlama dayandırıldı.
```

📌 Burada funksiyanın hər 3 saniyədən bir çağırıldığını və 10-cu saniyədə dayandırıldığını görürsən.

---

#### ⚠️ Tipik səhv

```javascript
// Səhv: funksiyanı dərhal çağırır
setTimeout(sayHello(), 3000);

// Doğru: sadəcə istinad veririk (və ya anonim funksiyanı ötürürük)
setTimeout(sayHello, 3000);
setTimeout(() => sayHello(), 3000);
```

---

### 13.1.2 Hadisələr (Events) 🖱️

Brauzerdəki JavaScript proqramları demək olar ki, həmişə **hadisə yönümlü (event-driven)** olur.
Yəni, kod **istifadəçi hərəkətini** (məsələn: klik, klaviatura düyməsi, toxunma) gözləyir və hadisə baş verəndə reaksiya göstərir.

Hadisələri “dinləmək” üçün **`addEventListener`** metodundan istifadə edirik.
Bu metoda biz iki şey deyirik:

1. **Hadisə növü** (məs: `"click"`, `"keydown"`, `"mouseover"` və s.)
2. **Callback funksiyası** – hadisə baş verdikdə çağırılacaq funksiya.

---

#### 🔹 Nümunə: Düyməyə klikləmə hadisəsi

```html
<!-- HTML faylında belə bir düymə olduğunu fərz edək -->
<button id="myButton">Mənə kliklə!</button>
```

```javascript
// 1. HTML elementini seçirik
const button = document.querySelector("#myButton");

// 2. Klik hadisəsi baş verdikdə çağırılacaq callback funksiyası
function onButtonClick() {
  console.log("🎉 Düyməyə klikləndi! İstifadəçiyə reaksiya veririk...");
}

// 3. Düyməyə "click" hadisəsi üçün dinləyici əlavə edirik
button.addEventListener("click", onButtonClick);

console.log(
  "Hadisə dinləyicisi quruldu. İstifadəçinin klikləməsi gözlənilir..."
);
```

---

**Konsol çıxışı:**

```
Hadisə dinləyicisi quruldu. İstifadəçinin klikləməsi gözlənilir...
🎉 Düyməyə klikləndi! İstifadəçiyə reaksiya veririk...   (istifadəçi düyməni klikləyəndə)
```

📌 Burada `onButtonClick` bizim **callback** funksiyamızdır.
Brauzer bu funksiyanı **yadda saxlayır** və yalnız istifadəçi kliklədikdə **geri çağırır**.

---

#### ⚠️ Tipik səhv

```javascript
// Səhv: funksiyanı dərhal çağırırıq, dinləyici isə boş qalır
button.addEventListener("click", onButtonClick());

// Doğru: funksiyanın adını ötürürük (çağırmırıq!)
button.addEventListener("click", onButtonClick);

// Alternativ: anonim funksiyadan istifadə
button.addEventListener("click", () => {
  console.log("Anonim funksiya da işləyə bilər!");
});
```

---

### 13.1.3 Şəbəkə Hadisələri (Network Events) 🌐

JavaScript-də asinxronluğun ən çox rast gəlinən mənbəyi **şəbəkə (network) sorğuları**dır.
Məsələn, brauzerdə çalışan bir kod serverdən məlumat çəkməlidir, amma bu proses **vaxt aparır** və proqramı **dondurmamalıdır**.

---

#### 🔹 Callback ilə nəticəni gözləmək

Asinxron bir funksiya (məsələn, serverdən məlumat istəyən) nəticəni dərhal `return` edə bilməz. Bunun əvəzinə biz ona bir **callback funksiyası** ötürürük.

- Əməliyyat **bitdikdə**, nəticə və ya xəta callback funksiyasına ötürülür.
- Bu üsul “geri çağırma” (callback) modeli adlanır.

---

#### 🔹 "Error-First" Callback Qaydası

Callback-lərin ən geniş yayılmış forması belədir:

```js
callback(error, result);
```

- Əgər **əməliyyat uğurludursa** → `callback(null, result)`
- Əgər **xəta baş veribsə** → `callback(error, null)`

Bu yanaşma nəticələri daha rahat idarə etməyə kömək edir.

---

#### 🔹 Nümunə: `XMLHttpRequest` ilə serverdən versiya nömrəsini almaq

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

  // 3. Sorğunu göndəririk (asinxron başlayır)
  request.send();

  // 4. Callback-ləri qeydiyyatdan keçiririk

  // 4a. Sorğu uğurla tamamlandıqda
  request.onload = function () {
    if (request.status === 200) {
      // HTTP statusu 200 OK
      const currentVersion = parseFloat(request.responseText);
      versionCallback(null, currentVersion); // error=null
    } else {
      versionCallback(`Server xətası: ${request.statusText}`, null);
    }
  };

  // 4b. Şəbəkə xətası (məs: internet kəsilməsi, vaxt aşımı)
  request.onerror = request.ontimeout = function (e) {
    versionCallback(`Şəbəkə xətası: ${e.type}`, null);
  };
}

// Funksiyanı istifadə edirik:
getCurrentVersionNumber((error, version) => {
  if (error) {
    console.error("❌ Versiya alına bilmədi. Səbəb:", error);
  } else {
    console.log(`✅ Serverdən gələn cari versiya: ${version}`);
  }
});

console.log(
  "Sorğu göndərildi, nəticə gələcəkdə callback vasitəsilə gələcək..."
);
```

---

#### 🔹 Konsol çıxışı (mümkün hallar)

1. **Uğurlu cavab (server 200 OK)**

```
🌐 Serverə sorğu göndərilir...
Sorğu göndərildi, nəticə gələcəkdə callback vasitəsilə gələcək...
✅ Serverdən gələn cari versiya: 1.42
```

2. **Server xətası (məs: 500 Internal Server Error)**

```
🌐 Serverə sorğu göndərilir...
Sorğu göndərildi, nəticə gələcəkdə callback vasitəsilə gələcək...
❌ Versiya alına bilmədi. Səbəb: Server xətası: Internal Server Error
```

3. **Şəbəkə xətası (internet kəsilib)**

```
🌐 Serverə sorğu göndərilir...
Sorğu göndərildi, nəticə gələcəkdə callback vasitəsilə gələcək...
❌ Versiya alına bilmədi. Səbəb: Şəbəkə xətası: error
```

---

#### ⚠️ Tipik səhvlər

```javascript
// 1. Yanlış: asinxron funksiya nəticəni dərhal qaytarmır
const version = getCurrentVersionNumber();
console.log(version); // ⚠️ Burada undefined olacaq!

// 2. Doğru: nəticəni callback içində istifadə edirik
getCurrentVersionNumber((error, version) => {
  if (!error) {
    console.log("Doğru nəticə:", version);
  }
});
```

---

### 13.1.4 Node.js-də Callback-lər və Hadisələr ⚙️

Node.js server tərəfi mühitidir və **performansı yüksək saxlamaq üçün** əməliyyatları **asinxron** şəkildə yerinə yetirir.
Demək olar ki, bütün **fayl əməliyyatları** və **şəbəkə sorğuları** asinxrondur və çox vaxt **callback** və **event** mexanizmlərinə əsaslanır.

---

#### 🔹 Nümunə 1: Faylın məzmununu asinxron oxumaq (`fs.readFile`)

Node.js-də bir faylı oxumaq üçün istifadə olunan `fs.readFile` funksiyası **asinxron** işləyir və bitdikdə callback çağırır.

```javascript
const fs = require("fs"); // Fayl sistemi (filesystem) modulunu import edirik

console.log("Konfiqurasiya faylı oxunur...");

// `fs.readFile` asinxron olaraq faylı oxuyur
fs.readFile("config.json", "utf-8", (err, text) => {
  if (err) {
    console.error(
      "❌ Konfiqurasiya faylını oxumaq mümkün olmadı:",
      err.message
    );
    return;
  }

  const options = JSON.parse(text);
  console.log("✅ Fayldan oxunan konfiqurasiya:", options);

  // Proqramın qalan hissəsini buradan işə salmaq olar...
  // startProgram(options);
});

console.log(
  "`readFile` çağırıldı, amma proqram gözləmir, öz işinə davam edir..."
);
```

**Konsol çıxışı (mümkün hallar):**

1. Fayl uğurla oxunarsa:

```
Konfiqurasiya faylı oxunur...
`readFile` çağırıldı, amma proqram gözləmir, öz işinə davam edir...
✅ Fayldan oxunan konfiqurasiya: { "mode": "production", "port": 8080 }
```

2. Fayl mövcud deyilsə:

```
Konfiqurasiya faylı oxunur...
`readFile` çağırıldı, amma proqram gözləmir, öz işinə davam edir...
❌ Konfiqurasiya faylını oxumaq mümkün olmadı: ENOENT: no such file or directory, open 'config.json'
```

---

#### 🔹 Nümunə 2: Node.js ilə HTTP sorğusu (Streaming Events)

Node.js-də HTTP cavabları **hissə-hissə (chunks)** gələ bilər. Buna **axın (stream)** deyilir. Biz bu hissələri toplamaq üçün hadisələrdən istifadə edirik.

```javascript
const https = require("https");

function getUrlContent(url, callback) {
  // 1. HTTP GET sorğusunu başladırıq
  const request = https.get(url);

  // 2. Serverdən cavab gələndə
  request.on("response", (response) => {
    let httpStatus = response.statusCode;
    if (httpStatus !== 200) {
      callback(`Server xətası: Status ${httpStatus}`, null);
      return;
    }

    let body = "";
    response.setEncoding("utf-8");

    // Data hissələri gəldikcə toplamaq
    response.on("data", (chunk) => {
      body += chunk;
    });

    // Bütün məlumat gəldikdə
    response.on("end", () => {
      callback(null, body);
    });
  });

  // Şəbəkə xətası
  request.on("error", (err) => {
    callback(err, null);
  });
}

// İstifadə:
getUrlContent(
  "https://jsonplaceholder.typicode.com/posts/1",
  (error, content) => {
    if (error) {
      console.error("❌ URL-dən məlumat alına bilmədi:", error);
    } else {
      console.log("✅ Serverdən gələn məlumat:", JSON.parse(content));
    }
  }
);
```

**Konsol çıxışı (uğurlu cavab):**

```
✅ Serverdən gələn məlumat: {
  userId: 1,
  id: 1,
  title: "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  body: "quia et suscipit..."
}
```

**Konsol çıxışı (xəta):**

```
❌ URL-dən məlumat alına bilmədi: Server xətası: Status 404
```

---

#### ⚠️ Tipik səhvlər

```javascript
// 1. Yanlış: faylı sinxron oxumağa çalışmaq (bloklayıcıdır!)
const data = fs.readFileSync("config.json", "utf-8");
console.log("Bu, böyük fayllarda bütün proqramı dayandıracaq!");

// 2. Yanlış: HTTP cavabını 'end' hadisəsindən əvvəl istifadə etməyə çalışmaq
let result;
getUrlContent(
  "https://jsonplaceholder.typicode.com/posts/1",
  (err, content) => {
    result = content;
  }
);
console.log(result); // ⚠️ Burada hələ undefined olacaq!
```

---

👉 **Yadda saxla:**

- Node.js-də **fayl** və **şəbəkə** əməliyyatları əsasən asinxrondur.
- Fayl oxuma → `fs.readFile` → **error-first callback**.
- HTTP sorğuları → **event-based streaming** (`data`, `end`, `error`).

---

### 13.2 "Vəd"lər (Promises)

## Promise nədir? 🤔

Təsəvvür et ki, dostundan borc pul istəyirsən. Dost dərhal pulu vermir, amma söz verir ki:

> "Sənə sabah verəcəm."

Bu söz, yəni **gələcəkdə icra olunacaq vəd**, JavaScript-də **Promise** adlanır.

**Promise (Vəd)** – asinxron bir əməliyyatın **gələcəkdəki nəticəsini** təmsil edən bir obyektdir.

- Nəticə hələ hazır ola bilər, olmaya da.
- Promise-lər bizə birbaşa “nəticə hazırdırmı?” deməyə imkan vermir. Bunun əvəzinə deyirik:

> “Nəticə hazır olanda bu funksiyanı çağır”.

Promise-lər ən sadə səviyyədə **callback-lərin alternatividir**, amma bəzi mühüm üstünlükləri var.

---

### Niyə Callback-lərdən Daha Yaxşıdır?

### 1️⃣ "Callback Cəhənnəmi"ndən Xilas Edir ⛓️

Callback-lərlə işləyərkən çox vaxt iç-içə callback-lər yazmaq lazımdır. Bu, kodu oxumağı və idarə etməyi çətinləşdirir:

```javascript
// Callback Hell nümunəsi
getData(id, (user) => {
  getPosts(user.id, (posts) => {
    getComments(posts[0].id, (comments) => {
      getAuthor(comments[0].authorId, (author) => {
        console.log("Callback Hell tamamlandı!");
      });
    });
  });
});
```

✅ Promise-lərdə isə bu **xətti zəncir** şəklində yazılır:

```javascript
getData(id)
  .then((user) => getPosts(user.id))
  .then((posts) => getComments(posts[0].id))
  .then((comments) => getAuthor(comments[0].authorId))
  .then((author) => console.log("Promise chain tamamlandı!", author))
  .catch((err) => console.error("Xəta baş verdi:", err));
```

**Konsol çıxışı (müvəffəqiyyətli hal):**

```
Promise chain tamamlandı! { id: 5, name: "Rauf" }
```

---

### 2️⃣ Standartlaşdırılmış Xəta İdarəetməsi

Callback-lərdə xətaları idarə etmək çətindir. Hər səviyyədə əl ilə yoxlama aparmaq lazımdır.
Promise-lərdə isə `.catch()` bütün zəncirdə baş verən xətaları tutur:

```javascript
getData(id)
  .then((user) => getPosts(user.id))
  .then((posts) => getComments(posts[0].id))
  .then((comments) => getAuthor(comments[0].authorId))
  .then((author) => console.log("Author:", author))
  .catch((err) => console.error("Xəta:", err)); // zəncirin hər yerindən gələn xətaları tutur
```

---

## Məhdudiyyətləri (Limitations)

❗ **Promise-lər yalnız bir dəfə yerinə yetirilən əməliyyatları təmsil edir**.

Bu o deməkdir ki:

- ✅ `setTimeout` üçün ideal (bir dəfə işləyir)
- ❌ `setInterval` üçün uyğun deyil (çünki davamlı təkrarlanır)
- ❌ `click` hadisəsi üçün uyğun deyil (çünki istifadəçi düyməyə bir neçə dəfə klikləyə bilər)

---

### 13.2.1 Promise-lərin İstifadəsi (Using Promises) ⚡

Təsəvvür edək ki, serverdən məlumat çəkən bir funksiyamız var, amma bu dəfə **callback yerinə Promise** qaytarır. Gəlin bunu `getJSON()` adlandıraq və istifadəsini göstərək:

```javascript
// getJSON funksiyası dərhal bir Promise qaytarır
getJSON("/api/user/profile").then((jsonData) => {
  console.log("✅ Məlumatlar uğurla alındı:", jsonData);
});

console.log("Promise quruldu, sorğu arxa planda davam edir...");
```

**Konsol çıxışı (təxminən):**

```
Promise quruldu, sorğu arxa planda davam edir...
✅ Məlumatlar uğurla alındı: { id: 1, name: "Leyla", age: 28 }
```

**Məntiq necədir?**

1. `getJSON()` çağırılarkən Promise dərhal geri qaytarılır və **pending** vəziyyətindədir.
2. `.then()` ilə Promise-ə deyirik:

> "Əməliyyat uğurla bitsə, bu funksiyanı çağır."

3. Serverdən cavab gələndə Promise **fulfilled** olur və `.then()` içindəki callback çağırılır.

Bu sintaksis kodu **oxunaqlı və linear** edir:

> `getJSON(...)` **sonra** `displayUserProfile(...)`

---

## ❌ Xətaların İdarə Edilməsi (Handling Errors)

Asinxron əməliyyatlar hər an xəta ilə nəticələnə bilər. Promise-lər bunun üçün **standart yol** təqdim edir: `.catch()`.

```javascript
function displayData(data) {
  console.log("✅ Uğurlu nəticə:", data);
  // Məlumatı emal edərkən xəta baş verə bilər
  throw new Error("Məlumatı emal etmək mümkün olmadı!");
}

function handleError(error) {
  console.error("❗️ Xəta baş verdi:", error.message);
}

console.log("Sorğu göndərilir...");

getJSON("/api/user/profile")
  .then(displayData) // Uğur halında çağırılır
  .catch(handleError); // Hər hansı xəta halında çağırılır
```

**Konsol çıxışı (xəta halı):**

```
Sorğu göndərilir...
✅ Uğurlu nəticə: { id: 1, name: "Leyla", age: 28 }
❗️ Xəta baş verdi: Məlumatı emal etmək mümkün olmadı!
```

> `.catch()` həm `getJSON`-dan gələn xətanı, həm də `.then()` içində baş verən xətanı tutur. Bu, **zəncirvari və səliqəli error handling** təmin edir.

---

## 📖 Promise Terminologiyası

Promise-lərlə işləyərkən 3 əsas vəziyyət var:

| Vəziyyət           | Təsviri                                                                |
| ------------------ | ---------------------------------------------------------------------- |
| **Pending (⏳)**   | Əməliyyat hələ başa çatmayıb, nəticə məlum deyil                       |
| **Fulfilled (✅)** | Əməliyyat uğurla başa çatdı, Promise-in **value**-si mövcuddur         |
| **Rejected (❌)**  | Əməliyyat xəta ilə başa çatdı, Promise-in **reason/error**-u mövcuddur |

Bir Promise **ya fulfilled, ya da rejected** olduqda onun vəziyyəti **Settled (🏁)** adlanır. Settled olduqdan sonra nəticəsi dəyişməzdir.

```
            ┌───────────┐
Başlanğıc ──> │  Pending  │ ──┬──> Fulfilled (nəticə ilə) ──┐
            └───────────┘   │                                 ├─> Settled
                            └──> Rejected (xəta ilə)  ───┘
```

---

### ✅ Nəticə

- Promise = gələcəkdə tamamlanacaq əməliyyatın vədi
- `.then()` = uğurlu nəticə üçün callback
- `.catch()` = xəta üçün callback
- `.finally()` (sonrakı bölmədə) = həmişə işə düşən təmizləmə callback
- Promise-lər callback hell-dən xilas edir və xətaların idarəsini standartlaşdırır.

---

### 13.2.2 Promise Zəncirləri (Chaining Promises) ⛓️

Promise-lərin ən böyük üstünlüklərindən biri, **ardıcıl asinxron əməliyyatları iç-içə callback-lər olmadan, oxunaqlı bir xətti zəncir** şəklində ifadə etməkdir.

---

## **Müasir vebdə Promise: `fetch` API** 🌐

`fetch` köhnə `XMLHttpRequest`-i əvəz edən müasir Promise-əsaslı vasitədir.

### **`fetch` necə işləyir?**

1. `fetch(url)` çağırırsınız və dərhal **Promise** qaytarır.
2. Serverdən ilk cavab (headers) gələndə, Promise **fulfilled** olur və `Response` obyekti ilə tamamlanır.
3. `Response` obyekti hələ sorğunun tam məzmunu (body) deyil. Məzmunu almaq üçün `response.json()` və ya `response.text()` çağırmalısınız.
4. `.json()` və `.text()` metodları da özləri **yeni bir Promise** qaytarır, çünki məzmunun tam yüklənməsi vaxt tələb edə bilər.

---

### **Yanlış yol: İç-içə Promise-lər** ❌

```javascript
fetch("/api/user/profile").then((response) => {
  response.json().then((profile) => {
    displayUserProfile(profile);
  });
});
```

Bu yanaşma bizi yenidən **callback hell-ə bənzər iç-içə quruluşa** qaytarır.

---

### **Düzgün yol: Promise Zənciri (Chaining)** ✅

```javascript
fetch("/api/user/profile") // 1️⃣ Sorğu göndər
  .then((response) => response.json()) // 2️⃣ JSON-ı al (yeni Promise)
  .then((profile) => displayUserProfile(profile)) // 3️⃣ JSON hazır olanda istifadə et
  .catch((error) => handleError(error)); // 4️⃣ Hər hansı xəta burda tutulur
```

**Konsol çıxışı (uğurlu halda):**

```
Serverdən cavab alındı...
Profil məlumatı göstərildi: { id: 1, name: "Leyla", age: 28 }
```

**Konsol çıxışı (xəta halı):**

```
❗️ Xəta baş verdi: NetworkError when attempting to fetch resource.
```

---

### **Zəncirin İşləmə Mexanizmi** 🤯

1. `fetch(url)` çağırılır → **Promise 1** qaytarılır (pending).
2. `.then(callback1)` çağırılır → dərhal **Promise 2** qaytarılır.
3. `.then(callback2)` çağırılır → dərhal **Promise 3** qaytarılır.
4. Server cavabı gələndə, **Promise 1 fulfilled** olur və nəticəsi `callback1`-ə ötürülür.
5. `callback1` içində `response.json()` çağırılır → bu da **yeni Promise** qaytarır.
6. JSON tam yükləndikdə, daxili Promise fulfilled olur və **Promise 2** yerinə yetirilir.
7. Nəticə `callback2`-yə ötürülür və istifadəçi profili ekranda göstərilir.
8. Əgər zəncirin hər hansı mərhələsində xəta baş verərsə, `.catch()` işə düşür.

---

### **Vacib Qeyd**

- Hər `.then()` **özündən əvvəlki Promise-in nəticəsini** alır və ya **daxili Promise-in tamamlanmasını** gözləyir.
- `.catch()` zəncirin istənilən yerində baş verən xətaları tutur.
- Promise chaining kodu həm **oxunaqlı**, həm də **səliqəli** edir, callback hell-dən tamamilə qurtarır.

---

### 13.2.3 Promise-lərin "Həll Olunması" (Resolving Promises) 🔗

Əvvəlki bölmədə belə bir zəncir görmüşdük:

```javascript
fetch(url)
  .then((response) => response.json())
  .then((profile) => displayUserProfile(profile));
```

Burada sual yaranır:
`response.json()` özü **Promise** qaytarır, amma növbəti `.then()` birbaşa həmin Promise-in nəticəsini (`profile`) alır. Arada sehr baş vermir, amma mexanizm belə işləyir.

---

## **Addım-addım nümunə**

```javascript
// c1 - birinci .then callback-i
function c1(response) {
  console.log("callback 1 (c1) işə düşdü, response obyekti gəldi.");
  const p4 = response.json(); // response.json() özü Promise qaytarır
  console.log("c1, Promise 4-ü (p4) qaytarır.");
  return p4;
}

// c2 - ikinci .then callback-i
function c2(profile) {
  console.log("callback 2 (c2) işə düşdü. Nəticə:", profile);
  // displayUserProfile(profile);
}

// Zənciri qururuq
const p1 = fetch("/api/user/profile"); // p1 (Promise 1)
const p2 = p1.then(c1); // p2 (Promise 2)
const p3 = p2.then(c2); // p3 (Promise 3)
```

**Konsol çıxışı (təxmini):**

```
callback 1 (c1) işə düşdü, response obyekti gəldi.
c1, Promise 4-ü (p4) qaytarır.
callback 2 (c2) işə düşdü. Nəticə: { id: 1, name: "Leyla", age: 28 }
```

---

## **"Resolved" Vəziyyəti Nədir?**

`.then(callback)` yeni bir Promise (`p_yeni`) qaytarır. Callback `v` dəyərini qaytardıqda, `p_yeni` **həll olunur (resolved)**.

### İki hal mümkündür:

1️⃣ **Əgər `v` adi dəyərdirsə (Promise DEYİL):**

- `p_yeni` dərhal fulfilled olur.
- Dəyəri `v`-dir. ✅

2️⃣ **Əgər `v` özü Promise-dirsə (`p_daxili`):**

- `p_yeni` həll olunur, amma hələ **pending** qalır.
- `p_yeni` **p_daxili**-yə "kilidlənir".
- `p_daxili` fulfilled/ rejected olanda, `p_yeni` də eyni nəticə ilə fulfilled/rejected olur. 🔗

**Vizual sxem:**

```
.then(callback) ──> p_yeni (yeni Promise)
     │
     └── callback bir dəyər qaytarır: V
           │
           ├── Əgər V adi dəyərdirsə ──> p_yeni dərhal Fulfilled ✅
           │
           └── Əgər V Promise-dirsə (p_daxili) ──> p_yeni Pending 🔗
                                                        │
                                                        └── p_daxili Fulfilled/Rejected olanda...
                                                              │
                                                              └── p_yeni də eyni nəticə ilə Fulfilled/Rejected olur
```

---

### **Nümunəyə qayıdaq**

1. `c1` `p4`-ü (`response.json()` Promise-i) qaytarır → `p2` həll olunur, amma pending qalır.
2. Serverdən məlumat tam yüklənir və `p4` fulfilled olur (JSON obyekt).
3. `p2` avtomatik olaraq `p4`-ün nəticəsi ilə fulfilled olur.
4. Nəticə `c2`-yə ötürülür və zəncir davam edir.

---

### 13.2.4 Promise-lər və Xətalar: Dərin Baxış 🛡️

Əvvəlki bölmədə `.then()` metoduna ikinci funksiya ötürərək xətaları tutmaq mümkün olduğunu gördük. Lakin müasir və daha geniş yayılmış üsul, **Promise zəncirinin sonunda `.catch()`** istifadə etməkdir.

---

## **Niyə `.catch()` vacibdir?**

- Synchronous kodda xəta baş verəndə proqram dərhal dayanır və `stack trace`-dən xətanın yerini görürük.
- Asynchronous kodda isə tutulmayan xətalar çox vaxt səssizcə yox olur və problemi tapmaq çətinləşir.
- `.catch()` metoduyla bu səssiz xətaların qarşısını alırıq.

---

## **`.catch()` və `.finally()`**

- **`.catch(callback)`** – `.then(null, callback)` qısa formasıdır.

  - Zəncir boyunca baş verən bütün xətalar **ilk `.catch()`** tərəfindən tutulur.
  - Sanki xətalar bir şəlalə kimi aşağı axır.

- **`.finally(callback)` (ES2018)**

  - Promise **uğurlu (`fulfilled`)** və ya **uğursuz (`rejected`)** olmasından asılı olmayaraq icra olunur.
  - Adətən təmizlik işləri üçün istifadə olunur (məsələn, loading indikatorunu gizlətmək).
  - `.finally()` içində arqument yoxdur, məqsəd sadəcə prosesi yekunlaşdırmaqdır.

---

## **`fetch` nümunəsi**

```javascript
fetch("/api/user/profile")
  .then((response) => {
    console.log("Başlıqlar gəldi, status yoxlanılır...");
    if (!response.ok) return null;

    const contentType = response.headers.get("content-type");
    if (!contentType || !contentType.includes("application/json")) {
      throw new TypeError(
        `Gözlənilən format JSON idi, gələn isə: ${contentType}`
      );
    }

    return response.json();
  })
  .then((profile) => {
    if (profile) {
      console.log("✅ Profil göstərilir:", profile);
    } else {
      console.log("👤 İstifadəçi tapılmadı.");
    }
  })
  .catch((e) => {
    console.error("❗️ Ümumi xəta baş verdi:");
    if (e instanceof TypeError) {
      console.error("Server format xətası:", e.message);
    } else if (e.name === "AbortError") {
      console.warn("Sorğu dayandırıldı.");
    } else {
      console.error("Şəbəkə xətası. İnternet bağlantınızı yoxlayın.", e);
    }
  })
  .finally(() => {
    console.log("🧼 Əməliyyat bitdi, təmizlik işləri aparılır.");
  });
```

### Mümkün **console çıxışları**:

#### 1️⃣ Uğurlu sorğu:

```
Başlıqlar gəldi, status yoxlanılır...
✅ Profil göstərilir: { id: 1, name: "Test User", email: "test@example.com" }
🧼 Əməliyyat bitdi, təmizlik işləri aparılır.
```

#### 2️⃣ Server JSON göndərmirsə:

```
Başlıqlar gəldi, status yoxlanılır...
❗️ Ümumi xəta baş verdi:
Server format xətası: Gözlənilən format JSON idi, gələn isə: text/html
🧼 Əməliyyat bitdi, təmizlik işləri aparılır.
```

#### 3️⃣ Şəbəkə problemi:

```
Başlıqlar gəldi, status yoxlanılır...
❗️ Ümumi xəta baş verdi:
Şəbəkə xətası. İnternet bağlantınızı yoxlayın. TypeError: Failed to fetch
🧼 Əməliyyat bitdi, təmizlik işləri aparılır.
```

#### 4️⃣ İstifadəçi tapılmadı (404 və ya null):

```
Başlıqlar gəldi, status yoxlanılır...
👤 İstifadəçi tapılmadı.
🧼 Əməliyyat bitdi, təmizlik işləri aparılır.
```

---

## **Zəncir ortasında xətanın bərpası (Recovery)**

`.catch()` həmişə zəncirin sonunda olmalı deyil. Bəzən xətanı tutub, onu bərpa edib, zəncirin davam etməsini təmin edə bilərik.

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
  .catch((error) => {
    console.warn(
      `⚠️ Xəta tutuldu, lakin proses davam edir. Səbəb: ${error.message}`
    );
    return "Bərpa edilmiş standart dəyər";
  })
  .then((result) => {
    console.log("✅ Zəncir davam etdi. Gələn nəticə:", result);
  });
```

### Mümkün console çıxışları:

#### 1️⃣ Uğurlu əməliyyat (xəta olmadan):

```
✅ Zəncir davam etdi. Gələn nəticə: Orijinal uğurlu nəticə
```

#### 2️⃣ Xəta baş verdi və bərpa edildi:

```
⚠️ Xəta tutuldu, lakin proses davam edir. Səbəb: Əməliyyatda təsadüfi bir xəta baş verdi
✅ Zəncir davam etdi. Gələn nəticə: Bərpa edilmiş standart dəyər
```

Burada görürsən ki, `.catch()` zəncirin davam etməsinə imkan verir və `.then()` həmişə işləyir, hətta əvvəlki mərhələdə xəta baş versə belə.

## **Vacib Qeyd: `=>` və `return`**

Promise zəncirlərində ən çox rast gəlinən səhvlərdən biri:

```javascript
// ✅ Avtomatik return, fiqurlu mötərizə yoxdur
promise.then((value) => anotherPromise(value));

// ❌ Fiqurlu mötərizə, amma return yoxdur -> undefined qaytarır
promise.then((value) => {
  anotherPromise(value);
});

// ✅ Fiqurlu mötərizə ilə return
promise.then((value) => {
  return anotherPromise(value);
});
```

---

### 13.2.5 Promise-lərin Paralel İcrası (Promises in Parallel) 🏁

Əvvəlki bölmələrdə gördüyümüz **zəncirlər (chains)** asinxron əməliyyatları **ardıcıl** şəkildə icra edirdi. Bəs əgər bir neçə asinxron əməliyyatı **eyni anda** başladıb, hamısının nəticəsini gözləmək istəsək? Bunun üçün JavaScript `Promise` obyektinin xüsusi statik metodları var.

---

#### `Promise.all()` — "Ya Hamısı, Ya Heç Biri"

`Promise.all()` bir massiv (`array`) dolusu Promise qəbul edir və nəticədə **tək bir yeni Promise** qaytarır.

**İşləmə Məntiqi:**

- ✅ **Uğur halı:** Yalnız bütün Promise-lər `fulfilled` olduqda, ümumi Promise də uğurla bitir və nəticələri massiv şəklində qaytarır.
- ❌ **Xəta halı:** Massivdəki Promise-lərdən **hər hansı biri** `rejected` olsa, dərhal rədd edilir və digər nəticələr gözlənilmir.

**Nümunə: Bir neçə məlumatı eyni anda yükləmək**

```javascript
const fetchProfile = fetch("/api/user/profile").then((r) => r.json());
const fetchPosts = fetch("/api/user/posts").then((r) => r.json());
const fetchSettings = fetch("/api/user/settings").then((r) => r.json());

console.log("Üç sorğu da eyni anda göndərildi...");

Promise.all([fetchProfile, fetchPosts, fetchSettings])
  .then(([profile, posts, settings]) => {
    console.log("✅ Bütün məlumatlar uğurla alındı!");
    console.log("Profil:", profile);
    console.log("Postlar:", posts);
    console.log("Ayarlar:", settings);
  })
  .catch((error) => {
    console.error("❗️ Sorğulardan birində xəta baş verdi:", error);
  });
```

**Console çıxışı (hamısı uğurlu olsa):**

```
Üç sorğu da eyni anda göndərildi...
✅ Bütün məlumatlar uğurla alındı!
Profil: {id: 1, name: "Test User", ...}
Postlar: [{id: 101, title: "Post 1"}, {id: 102, title: "Post 2"}, ...]
Ayarlar: {theme: "dark", notifications: true, ...}
```

---

#### `Promise.allSettled()` — "Hər Kəsin Nəticəsini Gözlə" (ES2020)

`Promise.allSettled()` heç vaxt rədd edilmir. O, **bütün Promise-lərin bitməsini** gözləyir və hər birinin nəticəsini statusla birlikdə qaytarır.

**Nümunə: Uğurlu və uğursuz əməliyyatları birlikdə işlətmək**

```javascript
const p1 = Promise.resolve("Uğurlu nəticə");
const p2 = new Promise((resolve, reject) =>
  setTimeout(() => reject("Xəta baş verdi!"), 1000)
);
const p3 = Promise.resolve("Başqa bir uğurlu nəticə");

Promise.allSettled([p1, p2, p3]).then((results) => {
  console.log("Bütün əməliyyatlar bitdi. Nəticələr:");
  results.forEach((result) => {
    if (result.status === "fulfilled") {
      console.log(`✅ Uğurlu: Dəyər = ${result.value}`);
    } else {
      console.log(`❌ Uğursuz: Səbəb = ${result.reason}`);
    }
  });
});
```

**Console çıxışı:**

```
Bütün əməliyyatlar bitdi. Nəticələr:
✅ Uğurlu: Dəyər = Uğurlu nəticə
❌ Uğursuz: Səbəb = Xəta baş verdi!
✅ Uğurlu: Dəyər = Başqa bir uğurlu nəticə
```

---

#### `Promise.race()` — "İlk Çatan Qalibdir" 🏆

`Promise.race()` hamının bitməsini gözləmir. Massivdəki Promise-lərdən **hansı birinci həll olunarsa** (istər `fulfilled`, istər `rejected`) dərhal həmin nəticə ilə yekunlaşır.

**Nümunə: Hansı server daha sürətli cavab verəcək?**

```javascript
const fastServer = new Promise((resolve) =>
  setTimeout(() => resolve("Sürətli serverdən cavab"), 1000)
);
const slowServer = new Promise((resolve) =>
  setTimeout(() => resolve("Yavaş serverdən cavab"), 3000)
);

Promise.race([fastServer, slowServer])
  .then((winnerResult) => {
    console.log("Qalib gələn nəticə:", winnerResult);
  })
  .catch((error) => {
    console.error("İlk cavab verən xəta qaytardı:", error);
  });
```

**Console çıxışı (1 saniyə sonra):**

```
Qalib gələn nəticə: Sürətli serverdən cavab
```

> Bu metod, həmçinin, müəyyən müddətdən çox çəkən sorğuları ləğv etmək üçün **timeout** məntiqində də istifadə olunur.

---

### 13.2.6 Öz Promise-lərimizi Yaratmaq (Making Promises) 🛠️

Əvvəllər `fetch()` kimi hazır Promise qaytaran funksiyalardan istifadə etdik. Bəs **özümüz necə Promise yarada bilərik**? Bunun üç əsas yolu var.

---

#### 1️⃣ Başqa Promise-lərə Əsaslanaraq 🔗

Əgər artıq Promise qaytaran funksiya varsa, onun üzərində `.then()` zənciri quraraq **yeni və daha spesifik Promise** yarada bilərik.

**Nümunə 1: `getJSON()` yaratmaq**

```javascript
function getJSON(url) {
  return fetch(url).then((response) => response.json());
}
```

**Nümunə 2: İstifadəçinin ən yüksək xalını gətirən funksiya**

```javascript
function getHighScore() {
  return getJSON("/api/user/profile").then((profile) => profile.highScore);
}

getHighScore()
  .then((score) => console.log("Ən yüksək xal:", score))
  .catch((err) => console.error("Xal alına bilmədi:", err));
```

**Console çıxışı (uğurlu sorğu):**

```
Ən yüksək xal: 1200
```

---

#### 2️⃣ Sinxron Dəyərlərdən Promise Yaratmaq 🎁

Bəzən nəticə **artıq əlindədir**, amma funksiya Promise qaytarmalıdır. Bunun üçün:

- `Promise.resolve(value)` → dərhal fulfilled Promise yaradır
- `Promise.reject(error)` → dərhal rejected Promise yaradır

**Nümunə: Arqumentləri yoxlamaq**

```javascript
function getUserData(userId) {
  if (typeof userId !== "number" || userId <= 0) {
    return Promise.reject(new Error("Keçərsiz istifadəçi ID-si!"));
  }
  return getJSON(`/api/users/${userId}`);
}

getUserData(-5)
  .then((user) => console.log(user))
  .catch((error) => console.error("❌ Xəta:", error.message));
```

**Console çıxışı:**

```
❌ Xəta: Keçərsiz istifadəçi ID-si!
```

---

#### 3️⃣ Sıfırdan Yaratmaq (`Promise` Konstruktoru) 🛠️

Ən fundamental yol **`new Promise()`** ilə sıfırdan Promise yaratmaqdır. Bu, callback-əsaslı və ya event-based asinxron API-ları Promise-ə çevirmək üçün ideal üsuldur.

**Sintaksis:**

```javascript
new Promise((resolve, reject) => {
  // Uğur → resolve(value)
  // Xəta → reject(error)
});
```

**Nümunə 1: `wait()` funksiyası**

```javascript
function wait(duration) {
  return new Promise((resolve, reject) => {
    if (duration < 0) {
      reject(new Error("Zaman yalnız irəli gedə bilər!"));
    }
    setTimeout(resolve, duration);
  });
}

console.log("Gözləmə başlayır...");
wait(2000)
  .then(() => console.log("✅ 2 saniyə keçdi!"))
  .catch((err) => console.error(err.message));
```

**Console çıxışı (2 saniyə sonra):**

```
Gözləmə başlayır...
✅ 2 saniyə keçdi!
```

**Nümunə 2: Node.js `http` modulunu Promise-ə çevirmək**

```javascript
const http = require("http");

function getJSON(url) {
  return new Promise((resolve, reject) => {
    const request = http.get(url, (response) => {
      if (response.statusCode !== 200) {
        reject(new Error(`HTTP Status: ${response.statusCode}`));
        response.resume();
        return;
      }

      let body = "";
      response.setEncoding("utf-8");
      response.on("data", (chunk) => {
        body += chunk;
      });
      response.on("end", () => {
        try {
          resolve(JSON.parse(body));
        } catch (e) {
          reject(e);
        }
      });
    });

    request.on("error", (error) => reject(error));
  });
}
```

**Console çıxışı (uğurlu sorğu):**

```
{ id: 1, name: "Test User", email: "user@test.com" }
```

---

### 13.2.7 Promise-lərin Ardıcıl İcrası (Promises in Sequence) ⛓️

`Promise.all()` bir neçə Promise-i **paralel** icra etmək üçün idealdır. Amma bəzən istədiyimiz odur ki, bir massiv (`array`) dolusu URL **ardıcıl olaraq**, yəni birincisi bitdikdən sonra ikincisi işləsin.

Bu daha mürəkkəbdir, çünki **zənciri dinamik qurmaq** lazımdır. İki əsas yanaşma var: **dinamik zəncir** və **rekursiv zəncir**.

---

#### 1️⃣ Dinamik Zəncir Qurmaq (Domino Effekti 🁢)

İdeya: Əvvəlcədən bütün zənciri qurub, sonra ilk Promise-i işə salmaq. Sanki dominoları bir-birinin ardınca düzüb, sonra birini itələyirsən.

**Nümunə: `fetchSequentially` funksiyası**

```javascript
function fetchSequentially(urls) {
  const bodies = [];

  function fetchOne(url) {
    return fetch(url)
      .then((response) => response.text())
      .then((body) => bodies.push(body));
  }

  let p = Promise.resolve(undefined);

  for (const url of urls) {
    p = p.then(() => fetchOne(url));
  }

  return p.then(() => bodies);
}

// İstifadəsi:
const urls = ["/api/url1", "/api/url2", "/api/url3"];
fetchSequentially(urls)
  .then((allBodies) => {
    console.log(
      "✅ Bütün URL-lər ardıcıl olaraq yükləndi. Nəticələr:",
      allBodies
    );
  })
  .catch((error) => {
    console.error("❗️ Ardıcıl yükləmə zamanı xəta baş verdi:", error);
  });
```

**Console çıxışı (təsəvvür edilən URL-lər üçün):**

```
✅ Bütün URL-lər ardıcıl olaraq yükləndi. Nəticələr: ["body1", "body2", "body3"]
```

---

#### 2️⃣ Rekursiv Zəncir (Matryoshka Kuklası Effekti 🪆)

İdeya: Hər Promise həll olunduqda, öz içindən növbəti Promise-i qaytarır. Sanki hər Matryoshka kuklasının içindən başqa bir kukla çıxır.

**Nümunə: `promiseSequence` funksiyası**

```javascript
function promiseSequence(inputs, promiseMaker) {
  inputs = [...inputs];

  function handleNextInput(outputs) {
    if (inputs.length === 0) {
      return outputs;
    } else {
      const nextInput = inputs.shift();
      return promiseMaker(nextInput)
        .then((output) => outputs.concat(output))
        .then(handleNextInput);
    }
  }

  return Promise.resolve([]).then(handleNextInput);
}

// İstifadəsi:
function fetchBody(url) {
  console.log(`Sorğu göndərilir: ${url}`);
  return fetch(url).then((r) => r.text());
}

promiseSequence(urls, fetchBody)
  .then((bodies) => {
    console.log(
      "✅ Bütün URL-lər ardıcıl yükləndi (2-ci üsul). Nəticələr:",
      bodies
    );
  })
  .catch(console.error);
```

**Console çıxışı:**

```
Sorğu göndərilir: /api/url1
Sorğu göndərilir: /api/url2
Sorğu göndərilir: /api/url3
✅ Bütün URL-lər ardıcıl yükləndi (2-ci üsul). Nəticələr: ["body1", "body2", "body3"]
```

---

### 13.3 `async` və `await` 🪄

ES2017-də təqdim edilən **`async` və `await`** açar sözləri asinxron proqramlaşdırmada inqilab etdi. Bu iki açar söz Promise-lərə əsaslanan kodu **sanki sinxron** kimi yazmağa imkan verir.

> Arxa planda hələ də Promise işləyir, amma kod çox daha təmiz və oxunaqlıdır.

- Promise uğurla tamamlanarsa → `return` dəyərinə bənzəyir
- Promise rədd edilərsə → `throw`-a bənzəyir

---

### 13.3.1 `await` İfadəsi (Expression)

`await` bir Promise-i götürür və onun **bitməsini gözləyir**. Nəticə:

- Uğur → dəyər qaytarır (`fulfilled value`)
- Xəta → "throw" edir (`rejected reason`)

**Nümunə:**

```javascript
console.log("Sorğu göndərilir...");

const response = await fetch("/api/user/profile");
const profile = await response.json();

console.log("✅ Nəticə alındı:", profile);
```

**Vacib Məqam:** `await` bütün proqramı dondurmur, yalnız daxil olduğu `async` funksiyanı dayandırır.

---

### 13.3.2 `async` Funksiyaları (Functions) ✨

`async` funksiyası:

1. Həmişə **Promise qaytarır**
2. Daxildə `await` istifadə etməyə imkan verir

**Qayda:**

- `return value` → Promise fulfilled
- `throw error` → Promise rejected

**Nümunə 1: `getHighScore` funksiyası**

```javascript
async function getHighScore() {
  try {
    console.log("Profil məlumatları çəkilir...");
    const response = await fetch("/api/user/profile");

    if (!response.ok) {
      throw new Error(`HTTP xətası! Status: ${response.status}`);
    }

    const profile = await response.json();
    console.log("Profil uğurla alındı!");

    return profile.highScore;
  } catch (error) {
    console.error("Ən yüksək xalı almaq mümkün olmadı:", error);
    throw error;
  }
}

// İstifadəsi:
getHighScore()
  .then((score) => console.log(`🏆 Ən yüksək xal: ${score}`))
  .catch((err) => console.log("Xəta ilə nəticələndi."));
```

**Console çıxışı (uğurlu sorğu üçün):**

```
Profil məlumatları çəkilir...
Profil uğurla alındı!
🏆 Ən yüksək xal: 1200
```

**Console çıxışı (HTTP xətası üçün):**

```
Profil məlumatları çəkilir...
Ən yüksək xalı almaq mümkün olmadı: Error: HTTP xətası! Status: 500
Xəta ilə nəticələndi.
```

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
const [user, posts] = await Promise.all([getJSON(userUrl), getJSON(postsUrl)]);
console.log("Paralel çəkmə bitdi.");
```

Burada hər iki `getJSON` sorğusu eyni anda başlayır və `await` onların hər ikisinin bitməsini gözləyir.

---

### 13.4 Asinxron İterasiya (Asynchronous Iteration) 🔁

Əvvəlki bölmələrdə öyrəndik ki, **Promise-lər tək bir asinxron əməliyyatın nəticəsi** üçündür.
Amma bəzən data **hissə-hissə**, zamanla gəlir — məsələn:

- Böyük faylları oxumaq
- Real-time bildirişləri qəbul etmək
- WebSocket-dən gələn mesajlar

Sadə Promise və `async/await` bu cür axınlar üçün kifayət etmir.

**Həll:** ES2018 ilə təqdim olunan **Asinxron İteratorlar** və **`for/await...of`** dövrü.

---

### 13.4.1 `for/await` Dövrü (The `for/await` Loop) ⏳

`for/await...of` dövrü, adi `for...of`-un asinxron versiyasıdır:

- Hər elementi götürür
- `await` ilə onun həllini gözləyir
- Sonra dövrün gövdəsini icra edir

❗️ Qızıl Qayda: `for await` yalnız `async` funksiyanın daxilində işləyir.

---

#### Nümunə 1: Node.js ilə böyük faylı oxumaq

```javascript
const fs = require("fs");

async function processFile(filename) {
  const stream = fs.createReadStream(filename, { encoding: "utf-8" });
  console.log("Fayl oxunmağa başlayır...");

  for await (const chunk of stream) {
    console.log(`--- YENİ HİSSƏ GƏLDİ (uzunluğu: ${chunk.length}) ---`);
  }

  console.log("✅ Faylın oxunması tamamilə bitdi.");
}

// İstifadəsi:
// processFile("large-log.txt");
```

**Console çıxışı (təxmini):**

```
Fayl oxunmağa başlayır...
--- YENİ HİSSƏ GƏLDİ (uzunluğu: 1024) ---
--- YENİ HİSSƏ GƏLDİ (uzunluğu: 1024) ---
...
✅ Faylın oxunması tamamilə bitdi.
```

---

#### Nümunə 2: Promise massivinə `for/await` tətbiqi

```javascript
const urls = ["/api/url1", "/api/url2", "/api/url3"];
const promises = urls.map((url) => fetch(url));

// Adi for...of ilə:
async function normalForLoop() {
  for (const promise of promises) {
    const response = await promise;
    console.log("Cavab alındı:", response.url);
  }
}

// for/await...of ilə:
async function forAwaitLoop() {
  for await (const response of promises) {
    console.log("Cavab alındı (for/await):", response.url);
  }
}

// İstifadəsi:
// await normalForLoop();
// await forAwaitLoop();
```

**Console çıxışı (təsəvvür edilən URL-lər üçün):**

```
Cavab alındı: /api/url1
Cavab alındı: /api/url2
Cavab alındı: /api/url3

Cavab alındı (for/await): /api/url1
Cavab alındı (for/await): /api/url2
Cavab alındı (for/await): /api/url3
```

> `for/await`-in əsl gücü, **əsl asinxron iteratorlarla**, məsələn `ReadableStream` və ya WebSocket axınları ilə işləyərkən ortaya çıxır.

---

### 13.4.2 Asinxron Iteratorlar (Asynchronous Iterators) 📜

- Adi iterator: `[Symbol.iterator]` metodu + `.next()` → `{ value, done }`
- Asinxron iterator: `[Symbol.asyncIterator]` metodu + `.next()` → `Promise<{ value, done }>`

**Vizual Fərq:**

```
Sinxron:   iterator.next()  ──> { value: 'A', done: false }
Asinxron:  asyncIterator.next() ──> Promise ──(fulfilled)──> { value: 'A', done: false }
```

> `for/await...of` məhz bu Promise-lə bükülmüş `.next()` obyektini idarə etmək üçün var.

---

### 13.4.3 Asinxron Generatorlar 🪄

Asinxron generatorlar həm `async`, həm də `generator` xüsusiyyətinə malikdir:

```javascript
async function* createClock(interval, maxTicks = Infinity) {
  for (let tick = 1; tick <= maxTicks; tick++) {
    await wait(interval); // gözləyir
    yield tick; // dəyəri ötürür
  }
}

async function wait(ms) {
  return new Promise((r) => setTimeout(r, ms));
}

async function runClock() {
  console.log("Saat başladı... 🕔");
  for await (const tick of createClock(1000, 5)) {
    console.log(`Tick: ${tick}`);
  }
  console.log("Saat dayandı. 🏁");
}

// runClock();  // Console çıxışı:
```

**Console çıxışı:**

```
Saat başladı... 🕔
Tick: 1
Tick: 2
Tick: 3
Tick: 4
Tick: 5
Saat dayandı. 🏁
```

---

### 13.4.4 Asinxron Iteratorları Əl ilə Yaratmaq ⚙️

```javascript
class AsyncQueue {
  constructor() {
    this.values = [];
    this.resolvers = [];
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
    return new Promise((resolve) => this.resolvers.push(resolve));
  }

  close() {
    while (this.resolvers.length > 0) this.resolvers.shift()(AsyncQueue.EOS);
    this.closed = true;
  }

  [Symbol.asyncIterator]() {
    return this;
  }
  next() {
    return this.dequeue().then((v) =>
      v === AsyncQueue.EOS
        ? { value: undefined, done: true }
        : { value: v, done: false }
    );
  }
}
AsyncQueue.EOS = Symbol("end-of-stream");
```

---

#### Nümunə: Klik hadisələrini axın kimi izləmək

```javascript
function eventStream(element, eventType) {
  const q = new AsyncQueue();
  element.addEventListener(eventType, (e) => q.enqueue(e));
  return q;
}

async function handleClicks() {
  console.log("🖱️ Klikləri gözləyirəm (5 klik)...");

  let count = 0;
  for await (const event of eventStream(document, "click")) {
    count++;
    console.log(`Klik ${count}: X=${event.clientX}, Y=${event.clientY}`);
    if (count === 5) break;
  }

  console.log("Dövr dayandı.");
}

// handleClicks(); // Browser-də işə salın
```

**Console çıxışı (browser):**

```
🖱️ Klikləri gözləyirəm (5 klik)...
Klik 1: X=120, Y=250
Klik 2: X=90, Y=300
Klik 3: X=450, Y=220
Klik 4: X=330, Y=400
Klik 5: X=150, Y=120
Dövr dayandı.
```

---