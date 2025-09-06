### 15.3.6 Geniş Nümunə: Mündəricatın Yaradılması 📚

**Məqsəd:** Səhifədəki bütün `<h2>`, `<h3>` və s. başlıq teqlərini avtomatik olaraq tapıb, nömrələyib, səhifənin əvvəlində onlara kliklənə bilən bir linklər siyahısı (mündəricat) yaratmaq.


```css
#TOC {
  border: solid black 1px;
  margin: 10px;
  padding: 10px;
  background: #f0f0f0;
}
.TOCEntry {
  margin: 5px 0px;
}
.TOCEntry a {
  text-decoration: none;
}
.TOCLevel1 {
  font-size: 16pt;
  font-weight: bold;
}
.TOCLevel2 {
  font-size: 14pt;
  margin-left: 15px;
}
.TOCLevel3 {
  font-size: 12pt;
  margin-left: 30px;
}
.TOCSectNum {
  color: #555;
}
.TOCSectNum::after {
  content: ": ";
}
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


---

### 15.4.4 Stil Cədvəllərinin (Stylesheets) Skriptləşdirilməsi 🎨

JavaScript ilə təkcə tək bir elementin stilini deyil, bütöv bir stil cədvəlini (**stylesheet**) manipulyasiya etmək olar. Səhifəyə qoşulmuş `<link rel="stylesheet">` və ya daxili `<style>` teqləri də adi DOM elementləridir və onları seçib idarə edə bilərik.

**Nümunə 1: Stil cədvəlini aktiv/deaktiv etmək (Mövzu dəyişdirici - Theme Toggle)**
Tutaq ki, HTML-də iki fərqli mövzu (tema) üçün stil faylımız var:

```html
<link id="light-theme" rel="stylesheet" href="light.css" />
<link id="dark-theme" rel="stylesheet" href="dark.css" disabled />
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

### 15.5.3 Verilmiş Nöqtədəki Elementi Təyin Etmək

`.getBoundingClientRect()`-in əksi olan bir əməliyyatdır: verilmiş `(x, y)` koordinatında hansı elementin olduğunu tapmaq. Bunun üçün `document.elementFromPoint(x, y)` metodundan istifadə edirik.

**Nümunə: Kliklənən elementi tapmaq**

```javascript
document.addEventListener("click", (event) => {
  // Klikin viewport koordinatlarını götürürük
  const x = event.clientX;
  const y = event.clientY;

  // Həmin nöqtədəki ən üst səviyyəli elementi tapırıq
  const elementUnderCursor = document.elementFromPoint(x, y);

  if (elementUnderCursor) {
    console.log(
      `Siz bir <${elementUnderCursor.tagName}> elementinə kliklədiniz!`
    );
  }
});
```

---


### Geniş Nümunə: Rəqəm Tapma Oyunu 🎮

Bu nümunə, yuxarıda izah edilən bütün məntiqi birləşdirərək tam işlək bir tək səhifəli proqram (SPA) yaradır. İstifadəçi hər təxmin etdikdə, oyunun vəziyyəti tarixçəyə yazılır və "Geri" düyməsi ilə əvvəlki təxminlərə qayıtmaq mümkün olur.

**1. HTML və CSS Quruluşu**

```html
<html>
  <head>
    <title>Mən bir rəqəm tutmuşam...</title>
    <style>
      body {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-evenly;
        font-family: sans-serif;
      }
      #heading {
        font: bold 36px sans-serif;
      }
      #container {
        border: solid black 1px;
        width: 80%;
        background-color: #f0f0f0;
      }
      #range {
        background-color: #4caf50;
        height: 1.5em;
        transition: all 0.3s ease;
      }
      #input {
        font-size: 24px;
        width: 60%;
        padding: 5px;
        margin-top: 20px;
        text-align: center;
      }
      #playagain {
        font-size: 24px;
        padding: 10px;
        border-radius: 5px;
        margin-top: 20px;
        cursor: pointer;
      }
    </style>
  </head>
  <body>
    <h1 id="heading">Mən bir rəqəm tutmuşam...</h1>
    <div id="container"><div id="range"></div></div>
    <input id="input" type="text" autofocus />
    <button id="playagain" hidden onclick="location.search='';">
      Yenidən Oyna
    </button>

    <script>
      /* ... JavaScript kodu aşağıdadır ... */
    </script>
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
    s.low = 0;
    s.high = 101; // Təxmin aralığı
    s.numGuesses = 0; // Təxminlərin sayı
    s.guess = null; // Son təxmin
    return s;
  }

  // `popstate` hadisəsindən gələn sadə obyektdən GameState nüsxəsini bərpa edir
  static fromStateObject(stateObject) {
    let s = new GameState();
    for (let key of Object.keys(stateObject)) {
      s[key] = stateObject[key];
    }
    return s;
  }

  // Oyunun hazırkı vəziyyətini URL-ə çevirir (bookmark üçün)
  toURL() {
    let url = new URL(window.location);
    url.searchParams.set("l", this.low);
    url.searchParams.set("h", this.high);
    url.searchParams.set("n", this.numGuesses);
    if (this.guess) url.searchParams.set("g", this.guess);
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

    heading.textContent =
      document.title = `Mən ${this.low} və ${this.high} arası bir rəqəm tutmuşam...`;
    range.style.marginLeft = `${this.low}%`;
    range.style.width = `${this.high - this.low}%`;
    input.value = "";
    input.focus();

    if (this.guess === null) {
      input.placeholder = "Təxmininizi daxil edin";
    } else if (this.guess < this.secret) {
      input.placeholder = `${this.guess} çox aşağıdır. Yenidən cəhd edin!`;
    } else if (this.guess > this.secret) {
      input.placeholder = `${this.guess} çox yuxarıdır. Yenidən cəhd edin!`;
    } else {
      heading.textContent =
        document.title = `${this.guess} doğrudur! ${this.numGuesses} cəhddə tapdınız!`;
      input.hidden = true;
      playagain.hidden = false;
    }
  }

  // İstifadəçinin təxmininə görə vəziyyəti yeniləyir
  updateForGuess(guess) {
    if (guess > this.low && guess < this.high) {
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