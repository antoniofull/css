# Airbnb CSS / Sass Styleguide

*Un ragionevole approccio al CSS e Sass*

## Sommario 

1. [Terminologia](#Terminologia)
    - [Regole](#rule-declaration)
    - [Selettori](#selettori)
    - [Proprietà](#proprieta)
1. [CSS](#css)
    - [Formattazione](#formattazione)
    - [Commenti](#commenti)
    - [OOCSS and BEM](#oocss-and-bem)
    - [Selettori ID](#selettori-id)
    - [Ancore JavaScript](#ancore-javascript)
    - [Bordi](#bordi)
1. [Sass](#sass)
    - [Sintassi](#sintassi)
    - [Ordine di dichiarazione delle proprietà](#ordine-di-dichiarazione-delle-proprietà)
    - [Variabili](#variabili)
    - [Mixins](#mixins)
    - [Direttiva Extend](#direttiva-extend)
    - [selettori annidati](#selettori-annidati)
1. [Traduzioni](#Traduzioni)

## Terminologia

### Regole

Una “regola” è il nome dato a un selettore (o a un gruppo di selettori )con un grouppo di proprietà. Di seguito un esempio:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selettori

In una regola css, "selettori" sono la parte che determina quale elemento del DOM riceve lo stile definito dalla proprietà. I selettori possono riferirsi a elementi HTML, cosí come a un classe di un elemento, ID, o qualsiasi altro attributo. Di seguito alcuni esempi di selettori: 

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Proprietà

Le proprietà sono i valori che danno all'elemento selezionato da una regola il suo stile.
Proprietà sono valori con chiavi-valori, e una regola che può contenere una o più proprietà dichiarate. Le proprietà sono cosi: 

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ back to top](#sommario)**

## CSS

### Formattare

* Usare soft tabs (2 spazi) per indentare il codice css
* Favorire trattino(-) invece che camelCase quando definire i nomi delle classi.
  - Trattino basso e PascalCasing sono okay se tu stai usando BEM (vedi [OOCSS and BEM](#oocss-and-bem) sotto).
* Non usare selettori ID.
* Quando si usano multipli selettori in a regola, dare a ogni selettore la sua linea. 
* Usare uno spazio prima di aprire la parentesi graffa `{` nella regola.
* Nelle proprietà, usa uno spazio dopo, ,ma non prima, il carattere `:`.
* Usare parentesi di chiusura `}` della regola in una nuova linea.
* Usare una linea vuota tra le varie regole.

**Sbagliato**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Corretto**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Commenti

* Favorire commenti in linea (`//` stile sass) rispetto a commenti di block.
* Favorire commenti sulla propria linea, evitare commenti in multikinea.
* Scrivere dettagliati commenti per il codice che non è auto-documentato:
  - Uso di z-index
  - Compatibilità o specifici hacks per browser

### OOCSS and BEM

Noi incoraggiamo una combinazione di OOCSS e BEM per le seguenti ragioni:
  * Aiuta a creare chiare e strette relazioni tra CSS e HTML 
  * Aiuta a creare riusabili componenti
  * Permette di ridurre la nidificazione e tenere la specifity bassa
  * Aiuta a costruire fogli di stile facilmente scalabili

**OOCSS**, oppure “Object Oriented CSS”, è un approccio per scrivere CSS che ti incoraggia a pensare al tuo foglio di stile come una collezione di "oggetti": riusabili, ripetibili snippets di codice che possono essere usati indipendentemente in un sito web.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, sta per “Block-Element-Modifier”, è una _naming convention_ (convenzione per naming delle classi css) per le classi in HTML e CSS. Originalmente sviluppata da Yandex tenendo in conto un largo codice di base e scalabilità, e può essere utilizzata come una solita linea di guida per implementare OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

Noi raccomandiamo una variante di BEM con PascalCased "blocchi", il quale funziona particolarmente bene quando combinato con componenti (ad esempio React.). Trattino basso e trattino sono usati per modificatori (La M in BEM) e figli (la E in BEM).

**Esempio**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

  * `.ListingCard` is the “block” and represents the higher-level component
  * `.ListingCard__title` is an “element” and represents a descendant of `.ListingCard` that helps compose the block as a whole.
  * `.ListingCard--featured` is a “modifier” and represents a different state or variation on the `.ListingCard` block.

### Selettori ID 

Mentre è possibile usare selettori ID in CSS, generalmente è considerato un anti-pattern. I selettori ID introducono un non necessario alto livello di [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) alle tue regole css, and il codice diventa non riutilizzabile.

Per maggiori informazioni sul soggetto, puoi leggere (in inglese) [questo articolo di CSS Wizardry](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) su come usare bene la specificity.

### Ancore JavaScript

Evita di connettere le stesse classi nel tuo CSS e Javascript. Combinare i due spesso porta, come minimo, a tempo perso durante il refactoring quando un developer deve fare referenza a ogni classe che sta cambiando, oppure porta il developer a non eseguire i cambi per paura di fare cambi che possano compromettere il codice base. Noi raccomandiamo di creare delle specifiche classi da usare poi in javascript, con il prefisso `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Bordi

Usa `0` invece di `none` per specificare che uno stile non ha bordi.

**Sbagliato**

```css
.foo {
  border: none;
}
```

**Corretto**

```css
.foo {
  border: 0;
}
```
**[⬆ Torna su](#Sommario)**

## Sass

### Syntassi

* Usa la sintassi `.scss`, mai l'originale sintassi `.sass` 
* Ordina il tuo CSS regolare con la dichiarazione `@include` logicamente (vedi sotto)

### Ordine di dichiarazione delle proprietà

1.  Dichiarazione di proprietà

    Lista tutte le dichiarazioni di proprietà standard, tutto quello che non è un `@include` o un selettore nidificato. 

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` dichiarazione

    Raggruppare tutti gli `@include` alla fine rende facile leggere l'intera regola.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Selettori Nidificati

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

**[⬆ back to top](#table-of-contents)**

## Translation

  This style guide is also available in other languages:

  - ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Bahasa Indonesia**: [mazipan/css-style-guide](https://github.com/mazipan/css-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [ArvinH/css-style-guide](https://github.com/ArvinH/css-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [Zhangjd/css-style-guide](https://github.com/Zhangjd/css-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [mat-u/css-style-guide](https://github.com/mat-u/css-style-guide)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [nao215/css-style-guide](https://github.com/nao215/css-style-guide)
  - ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [CodeMakeBros/css-style-guide](https://github.com/CodeMakeBros/css-style-guide)
  - ![PT-BR](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [felipevolpatto/css-style-guide](https://github.com/felipevolpatto/css-style-guide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Nekorsis/css-style-guide](https://github.com/Nekorsis/css-style-guide)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [ismamz/guia-de-estilo-css](https://github.com/ismamz/guia-de-estilo-css)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [trungk18/css-style-guide](https://github.com/trungk18/css-style-guide)

**[⬆ back to top](#table-of-contents)**