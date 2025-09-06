### 15.6 Veb Komponentləri (Web Components) 🧩

HTML bizə sənədləri qurmaq üçün `<p>`, `<h1>`, `<input>` kimi bir çox teq verir. Lakin müasir istifadəçi interfeysləri (UI) üçün bu teqlər çox vaxt kifayət etmir.

Məsələn, müasir bir axtarış qutusu (search box) yaratmaq üçün sadəcə bir `<input>` teqi bəs deyil. Bizə adətən bunları birləşdirmək lazım gəlir:

- Elementləri saxlayan bir konteyner `<div>`.
- Mətni daxil etmək üçün bir `<input>`.
- Axtarış və ləğv etmə işarələri üçün iki `<img>` və ya `<span>`.
- Bütün bunların görünüşünü və davranışını idarə etmək üçün xeyli CSS və JavaScript.

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
  <img src="images/search-icon.png" slot="left" />

  <img src="images/cancel-icon.png" slot="right" />
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
const search = document.querySelector("search-box");

// Dəyərini oxumaq
let currentValue = search.value;

// Dəyərini dəyişmək
search.value = "Yeni axtarış";

// Metodunu çağırmaq
search.clear();
```

---

### 15.6.2 HTML Şablonları (Templates) 📄

Veb Komponentlərinin təməlində dayanan texnologiyalardan biri `<template>` teqidir.

**Template nədir?** 🤔
Təsəvvür et ki, peçenye bişirmək üçün bir qəlibin (template) var. Sən bu qəlibdən istifadə edərək yüzlərlə eyni formalı peçenye hazırlaya bilərsən. `<template>` teqi də HTML üçün belə bir "qəlibdir".

O, brauzerdə **görünməyən**, passiv bir HTML strukturu saxlayır. Bu struktur, JavaScript ilə istədiyimiz qədər **kopyalanıb (cloned)** sənədə əlavə edilə bilər. Bu, xüsusilə eyni quruluşa malik elementləri (məsələn, cədvəl sətirləri, siyahı elementləri) təkrar-təkrar yaratmaq lazım gəldikdə çox səmərəlidir, çünki brauzer şablonu yalnız bir dəfə emal (parse) edir.

**API:**

- `<template>` elementi JavaScript-də `HTMLTemplateElement` obyekti ilə təmsil olunur.
- Onun ən vacib xüsusiyyəti **`.content`**-dir.
- `template.content` şablonun içindəki bütün elementləri saxlayan bir **`DocumentFragment`**-dır.
- Bu fraqmenti kopyalamaq üçün `.cloneNode(true)` metodundan istifadə edirik (`true` bütün alt elementlərin də kopyalanmasını təmin edir).

**Geniş Nümunə: Cədvəl sətirlərini şablonla yaratmaq**

```html
<table id="user-table">
  <thead>
    <tr>
      <th>Ad</th>
      <th>Şəhər</th>
    </tr>
  </thead>
  <tbody></tbody>
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
  { name: "Leyla", city: "Sumqayıt" },
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

- **`tagName`**: Fərdi elementin adı (mütləq defisli olmalıdır).
- **`class`**: Həmin elementin davranışını təyin edən və `HTMLElement`-dən törəyən sinif.

#### Həyat Dövrü Metodları (Lifecycle Callbacks)

Brauzer, fərdi elementin həyatının müxtəlif mərhələlərində avtomatik olaraq bəzi xüsusi metodları çağırır:

- **`connectedCallback()`**: Element DOM-a əlavə edildikdə çağırılır. İlkin quraşdırma, stil vermə kimi əməliyyatlar üçün idealdır.
- **`disconnectedCallback()`**: Element DOM-dan silindikdə çağırılır. Təmizlik işləri üçün istifadə oluna bilər.
- **`attributeChangedCallback(name, oldValue, newValue)`**: İzlənilən bir atribut dəyişdikdə çağırılır.

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
customElements.define(
  "inline-circle",
  class InlineCircle extends HTMLElement {
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
      switch (name) {
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
    get diameter() {
      return this.getAttribute("diameter");
    }
    set diameter(value) {
      this.setAttribute("diameter", value);
    }

    get color() {
      return this.getAttribute("color");
    }
    set color(value) {
      this.setAttribute("color", value);
    }
  }
);
```


---

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

- Kölgə DOM-un içində yerləşdirilən `<slot>` teqi, host elementin "işıqlı" (light DOM) övladlarının harada göstəriləcəyini bildirən bir "yer tutucudur".
- Əgər kölgə DOM-da heç bir `<slot>` yoxdursa, host elementin övladları heç vaxt göstərilmir.
- Slotlara `name` atributu verərək, onları adlandırmaq olar. Bu zaman istifadəçi, öz elementinə `slot="ad"` atributu verərək onu müvafiq slota yerləşdirə bilər.

**Geniş Nümunə: Fərdi Kart Komponenti**
Gəlin həm başlığı, həm də məzmunu üçün slotları olan bir `<user-card>` komponenti yaradaq.
**1. Komponentin JavaScript tərifi (onun kölgə DOM-u):**

```javascript
class UserCard extends HTMLElement {
  constructor() {
    super();
    // Komponentə bir kölgə ağacı bağlayırıq
    const shadow = this.attachShadow({ mode: "open" });

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
customElements.define("user-card", UserCard);
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

- Bu metod bir elementə kölgə ağacı bağlayır və həmin ağacı (bir `DocumentFragment` olaraq) qaytarır.
- `mode: 'open'` o deməkdir ki, JavaScript ilə `element.shadowRoot` vasitəsilə kölgə ağacına müraciət etmək olar. `mode: 'closed'` isə bunu qadağan edir və çox nadir hallarda istifadə olunur.

---

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
    this.input.onfocus = () => {
      this.setAttribute("focused", "");
    };
    this.input.onblur = () => {
      this.removeAttribute("focused");
    };

    // Sol ikona (böyüdücü şüşə) kliklədikdə və ya inputda "change" hadisəsi baş verdikdə
    // fərdi "search" hadisəsini göndəririk.
    leftSlot.onclick = this.input.onchange = (event) => {
      event.stopPropagation(); // Klikin yuxarı qabarcıqlanmasının qarşısını alırıq
      if (this.disabled) return;
      this.dispatchEvent(
        new CustomEvent("search", {
          detail: this.input.value,
        })
      );
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
  get value() {
    return this.input.value;
  }
  set value(v) {
    this.input.value = v;
    this.setAttribute("value", v);
  }

  get placeholder() {
    return this.getAttribute("placeholder");
  }
  set placeholder(v) {
    this.setAttribute("placeholder", v);
  }

  get disabled() {
    return this.hasAttribute("disabled");
  }
  set disabled(v) {
    if (v) this.setAttribute("disabled", "");
    else this.removeAttribute("disabled");
  }

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
  const searchBox = document.querySelector("search-box");

  // Fərdi "search" hadisəsini dinləyirik
  searchBox.addEventListener("search", (e) => {
    console.log(`Axtarış başladı! Dəyər: "${e.detail}"`);
  });

  // Fərdi "clear" hadisəsini dinləyirik
  searchBox.addEventListener("clear", () => {
    console.log("Axtarış qutusu təmizləndi!");
  });
</script>
```