# Cestovka pro Eleventy

*Cvičný projekt pro kurz Staň se kóderkou od Czechitas.*

Projekt *Cestovka* už znáš. Pracovala jsi na něm v předchozích cvičeních. Nyní Cestovku upravíme tak, abychom ji jako web mohli publikovat pomocí generátoru statických webů Eleventy. Vytvoříme si základní šablonu s hlavičkou a patičkou stránky, abychom pomocí ní mohli na webu vytvořit i další stránky.

![Ukázka výsledku](ukazka-vysledku.jpg)

## Postup

Cílem projektu je procvičit si založení projektu v Eleventy a vytvoření základní šablony:

### Založení projektu

- udělej si fork tohoto repozitáře
- v repozitáři je pouze tento návod, takže pokud nepotřebuješ, aby byl návod součástí projektu, můžeš si klidně na GitHubu založit úplně nový (prázdný) repozitář a vyzkoušet si tak, jak bys postupovala například u svého vlastního projektu
- repozitář si naklonuj k sobě na počítač
- pokud si vytvářila svůj vlastní repozitář (a ne kopii repozitáře s tímto zadáním), vytvoř si v projektu soubor `.gitignore` a napiš do něj minimálně následující dva řádky:
  ```
  node_modules/
  _site/
  ```

### Instalace Eleventy

- inicializuj `package.json` pomocí následujícího příkazu na příkazové řádce:
  ```shell
  npm init -y
  ```
- nainstaluj balíček systému Eleventy, opět na příkazové řádce:
  ```shell
  npm install --save-dev @11ty/eleventy
  ```
- v souboru `package.json` uprav sekci `scripts` tak, aby obsahovala skripty `serve` a `build` pro snazší spouštění webu. Tvůj `package.json` by měl vypadat nějak takto (může se lišit verze Eleventy, název projektu, apod.):
  ```json
  {
    "name": "projekt-cestovka-eleventy",
    "version": "1.0.0",
    "scripts": {
      "serve": "npx @11ty/eleventy --serve",
      "build": "npx @11ty/eleventy"
    },
    "devDependencies": {
      "@11ty/eleventy": "^3.1.0"
    }
  }
  ```
  Nepotřebné řádky, jako `author`, `license`, apod. můžeš klidně odstranit, ale nic se nestane, když je v `package.json` necháš.

### Konfigurace Eleventy

- v kořenové složce projektu si založ nový soubor s názvem `eleventy.config.js`
- v tomto souboru si můžeme Eleventy nastavit podle svých potřeb
- potřebujeme nastavit zejména:
  - kopírování složky `images` do výsledného webu (složku `images` přidáme později)
  - kopírování složky `css` do výsledného webu
  - nastavení šablonovacího jazyka, který budeme používat - my si nastavíme systém Nunjucks (čti jako "nunčaks")
- kompletní obsah souboru `eleventy.config.js` by měl vypadat následovně:
  ```js
  export default async function(eleventyConfig) {
    // Výchozí výstupní složka je: _site
    // Zkopírovat složku images/ do _site/images
    eleventyConfig.addPassthroughCopy("images");
    // Zkopírovat složku css/ to _site/css/
    eleventyConfig.addPassthroughCopy("css");
  }

  export const config = {
    // jako šablonu nebo soubor s obsahem
    // můžeme použít následující formáty
    templateFormats: ["njk", "html", "md"],

    // jako šablonovací jazyk zvolíme pro všechny
    // formáty výše jazyk Nunjucks
    markdownTemplateEngine: "njk",
    htmlTemplateEngine: "njk",
    dataTemplateEngine: "njk",
  };
  ```
- Eleventy bychom měli mít nainstalované a připravené.

### Nakopírování obsahu Cestovky

- k dispozici máš [řešení projektu Cestovka](https://github.com/Czechitas-Koderka-podklady/reseni-projekt-cestovka)
- v řešení je HTML a CSS úvodní stránky
- naklonuj si repozitář s řešením k sobě na disk - můžeš to brát tak, jako že sis Cestovku takto sama dopředu nakódovala jen pomocí HTML a CSS
- z řešení si do projektu, kde jsme nastavili Eleventy nakopíruj:
  - soubor `index.html` a případné další HTML soubory
  - složku `css`
  - složku `images`
- pokud spustíš Eleventy pomocí připraveného scriptu `serve` ve VS Code, nebo na příkazové řádce napíšeš `npx eleventy --serve`, měla bys vidět úvodní stránku Cestovky
- teď jsme připraveni udělat ze stránky šablonu

### Vytvoření šablony

- v kořenové složce projektu vytvoříme složku `_includes`
- ve složce `_includes` vytvoříme soubor pro šablonu např. `sablona.njk`
- do šablony přesuneme vrchní část HTML stránky (z našeho `index.html`), včetně hlavičky webu, kde je logo a menu
- do šablony přesuneme i spodní část HTML stránky, tj. patičku stránky a vše, co za ní následuje (konec `</body>` a `</html`)
- mezí horní a spodní část stránky v šabloně vložíme značku, díky které se nám do šablony doplní obsah:
  ```njk
  {{ content | safe }}
  ```
- v sekci `<head>` v šabloně můžeme ve značce `<title>` vyměnit obsah za `{{title}}` - díky tomu se do šablony doplní vždy titulek stránky, která se v šabloně zrovna bude zobrazovat
- šablonu máme připravenou a můžeme se věnovat stránce s obsahem

### Stránka s obsahem a použití šablony

- ze souboru `index.html` jsme přesunuli vrchní a spodní část stránky do šablony, v souboru by nám měl zbýt pouze prostředek stránky (obsah s mřízkou výletů, vlastnosti s ikonkami, novinky)
- když teď spustíme Eleventy tak uvidíme, že se nám zobrazuje pouze obsah bez hlavičky a patičky i bez CSS stylu
- do stránky potřebujeme přidat tzv. **front matter**, kde nastavíme
  - titulek stránky
  - jakou šablonu má stránka použít
- front matter musí být vždy úplně na začátku souboru a začíná a končí řádkem s třemi znaky mínus `---`
- pozor, že před začátkem front matter nesmí být žádná mezera
- na začátek `index.html` vložme:
  ```yaml
  ---
  layout: sablona.njk
  title: Cestovka
  ---
  ```
- pokud spustíme Eleventy a podíváme se na stránku nyní, měli bychom vidět, že už obsahuje i hlavičku a patičku stránky a mezi ně je dosazený obsah

### Když nefunguje CSS ani obrázky

- na úvodní stránce `index.html` bude pravděpodobně vše fungovat správně
- pokud bychom ale měli na webu další stránky, je pravděpodobné, že se nám na nich přestanou zobrazovat obrázky a přestane fungovat i CSS
- na výsledném webu totiž Eleventy přesune HTML soubory dalších stránek do složek a cesty k obrázkům a CSS souborům tak přestanou fungovat
- řešení je jednoduché - upravíme cesty k souborům tak, aby vždy začínaly lomítkem `/`, které symbolizuje kořenovou složku webu
- v šabloně, v souborech s obsahem, a raději i v CSS (pokud máme v CSS obrázky nastavené na pozadí) přidejme k cestám lomítko na začátek
- z `<link rel="stylesheet" href="css/style.css">` se stane:
  ```html
  <link rel="stylesheet" href="/css/style.css">
  ```
- odkazy na obrázky, např. `<img class="trip__image" src="images/palmy.jpg" alt="Palmy">` upravme na:
  ```html
  <img class="trip__image" src="/images/palmy.jpg" alt="Palmy">
  ```

### Vystavení stránky na Netlify

- udělej `commit` všech změn a `push` na GitHub
- přihlaš se (pomocí GitHub účtu) na Netlify
- na záložce **Project** přidej nový projekt tlačítkem **Add new project** a vyber **Import an existing project**
- vyber GitHub a vyhledej svůj repozitář s projektem
- pokud toto děláš poprvé, GitHub tě ještě vyzve, abys Netlify povolila přístup ke svým repozitářům
- jakmile vybereš projekt, na následující obrazovce by mělo být vše už nastavené a stačí jen odkliknout
- Netlify bude chvilku trvat, než web sestaví a vystaví na internet
- jakmile bude vše hotové, web je vystavený na adrese, kterou vidíš v horní části stránky, a můžeš se na něj podívat

🎉 **Gratulujeme, umíš tvořit weby pomocí šablon generátoru statických webů Eleventy!**
