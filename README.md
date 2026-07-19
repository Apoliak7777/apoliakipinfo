<div align="center">

[![Slovencina](https://img.shields.io/badge/SK-Sloven%C4%8Dina-2ea043?style=for-the-badge)](README.md) [![English](https://img.shields.io/badge/EN-English-30363d?style=for-the-badge)](README.en.md)

</div>

<div align="center">

# 🌐 IP Infos

**Jednosúborová statická web stránka, ktorá zobrazí tvoju verejnú IP adresu a vyhľadá geolokačné údaje k ľubovoľnej IPv4 adrese alebo doméne.**

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?style=flat-square&logo=githubpages&logoColor=white)
![Zero Build](https://img.shields.io/badge/build-none-success?style=flat-square)

[Živé demo](https://ipinfo.apoliak.online) - [Rýchly štart](#-rýchly-štart) - [Známe obmedzenia](#️-známe-obmedzenia)

</div>

---

## 📑 Obsah

- [🔎 Prehľad](#-prehľad)
- [✨ Funkcie](#-funkcie)
- [🚀 Rýchly štart](#-rýchly-štart)
- [📁 Štruktúra projektu](#-štruktúra-projektu)
- [🔌 Použité API](#-použité-api)
- [🛠️ Technológie](#️-technológie)
- [🌍 Nasadenie na GitHub Pages](#-nasadenie-na-github-pages)
- [⚠️ Známe obmedzenia](#️-známe-obmedzenia)
- [📄 Licencia](#-licencia)

---

## 🔎 Prehľad

**IP Infos** je network utility stránka, ktorá po načítaní automaticky zistí tvoju verejnú IP adresu a umožní ti vyhľadať geolokačné údaje k akejkoľvek IPv4 adrese alebo doméne - krajinu, región, mesto, providera, časové pásmo a odkaz na mapu podľa súradníc.

Celá aplikácia žije v jedinom súbore `index.html`: markup, CSS aj JavaScript sú inline. Nie je tu žiadny backend, žiadny build step, žiadny package manager a žiadna závislosť, ktorú by bolo treba inštalovať. Dáta prichádzajú priamo z prehliadača z dvoch verejných API.

Používateľské rozhranie je kompletne v slovenčine (`<html lang="sk">`) a projekt je nasadený ako GitHub Pages stránka na vlastnej doméne.

---

## ✨ Funkcie

- 🛰️ **Automatická detekcia vlastnej IP** - hneď po načítaní stránky sa zavolá `api.ipify.org` a vrátená verejná IP sa zobrazí veľkým písmom vrátane stavových badge-ov.
- 🌍 **Lookup IP alebo domény** - zadaj adresu do poľa a stlač `Hľadať info` alebo `Enter`; údaje sa načítajú z `ipapi.co`.
- ✅ **Validácia na strane klienta** - funkcia `detectKind()` overí vstup prísnym IPv4 regexom alebo doménovým regexom ešte pred akýmkoľvek sieťovým volaním.
- 🧹 **Normalizácia vstupu** - `cleanInput()` oreže medzery, odstráni `http://` / `https://` a koncové lomítko, takže funguje aj vloženie celej URL.
- 📊 **Dve výsledkové karty** - `Výsledok` zobrazí dotaz, IP, krajinu, región, mesto a providera; `Detaily` typ vstupu, timezone, country code, PSČ a kontinent.
- 🗺️ **Odkaz na Google Maps** - ak API vráti latitude aj longitude, pribudne link `Otvoriť mapu` otvárajúci sa v novom okne. Samotné číselné súradnice sa nikde nevypisujú, použijú sa iba na zostavenie URL.
- 📋 **Kopírovanie IP** - tlačidlo `Kopírovať moju IP` používa Clipboard API; ak zlyhá, IP sa vloží priamo do vstupného poľa, označí sa a zobrazí sa warning hláška.
- ⚡ **Použiť moju IP** - jedným klikom vyplní vlastnú IP a okamžite spustí lookup.
- 🌓 **Prepínač tmavej a svetlej témy** - prepína atribút `data-theme` na `<html>`; počiatočná téma sa preberá z nastavenia operačného systému cez `prefers-color-scheme`.
- 💬 **Farebné stavové hlášky** - správy typu success / error / warning, plus deaktivované tlačidlo počas prebiehajúceho requestu.
- 📱 **Plne responzívny layout** - jednostĺpcový na mobile, dvojstĺpcová mriežka nad 760px, `viewport-fit=cover` pre zariadenia s výrezom.

---

## 🚀 Rýchly štart

Nie je čo inštalovať a nie je čo buildovať. V repozitári nie je `package.json`, `requirements.txt`, `Makefile` ani žiadny build skript.

### Najjednoduchšie - otvor súbor priamo

Windows (`cmd.exe` alebo PowerShell):

```powershell
start index.html
```

macOS:

```bash
open index.html
```

Linux:

```bash
xdg-open index.html
```

### Odporúčané - servuj cez lokálny HTTP server

```bash
python -m http.server 8000
```

Potom otvor `http://localhost:8000` v prehliadači.

Alternatívne, ak máš Node.js:

```bash
npx serve .
```

> **Tip:** Pri otvorení cez `file://` môže byť `navigator.clipboard` v niektorých prehliadačoch nedostupný alebo zlyhať. Tlačidlo `Kopírovať moju IP` v takom prípade prepne na fallback - vloží IP do vstupného poľa a zobrazí warning hlášku. Lokálny HTTP server na `localhost` tento problém rieši.

> **Pozor:** Aplikácia potrebuje aktívne internetové pripojenie vždy, aj lokálne. Všetky zobrazené dáta sa ťahajú naživo z externých API.

---

## 📁 Štruktúra projektu

```text
apoliakipinfo/
├── index.html    # celá aplikácia - markup + inline <style> + inline <script>
├── CNAME         # GitHub Pages custom domain binding (ipinfo.apoliak.online)
├── LICENSE       # MIT - ale iba torzo, pozri sekciu Licencia
├── README.md     # tento súbor - slovenská verzia
└── README.en.md  # anglická verzia
```

Vnútorné členenie `index.html` (303 riadkov):

| Riadky  | Obsah                                                                                                          |
| ------- | -------------------------------------------------------------------------------------------------------------- |
| 10-112  | Inline `<style>` - CSS premenné pre tmavú a svetlú tému, karty, grid layout, media queries                       |
| 114-185 | Markup - topbar s logom a prepínačom témy, hero karta, karta s vlastnou IP, ovládacie prvky, dve výsledkové karty |
| 187-301 | Inline `<script>` - objekt `state`, cache elementov `el`, funkcie a naviazanie event listenerov                  |

Kľúčové funkcie v skripte:

| Funkcia                                     | Čo robí                                              |
| ------------------------------------------- | ---------------------------------------------------- |
| `setTheme()` / `detectSystemTheme()`        | Prepnutie témy a načítanie preferencie z OS           |
| `cleanInput()`                              | Normalizácia vstupu (trim, odstránenie protokolu a lomítka) |
| `detectKind()`                              | Rozpozná IPv4 alebo doménu, inak vráti prázdny reťazec |
| `loadMyIp()`                                | Načíta vlastnú verejnú IP z ipify                     |
| `lookup()`                                  | Vykoná geolokačný dotaz na ipapi.co                   |
| `renderResult()`                            | Vykreslí obe výsledkové karty a prípadný odkaz na mapu |
| `copyMyIp()` / `useMyIp()` / `clearResults()` | Obsluha ostatných tlačidiel                         |

Bootstrap sú posledné dva riadky skriptu: `setTheme(detectSystemTheme())` a `loadMyIp()`.

---

## 🔌 Použité API

Obidve URL sú natvrdo zapísané priamo v `index.html`. Ani jedno nevyžaduje API kľúč ani registráciu.

| Účel                          | Endpoint                                | Metóda | Volané v     |
| ----------------------------- | --------------------------------------- | ------ | ------------ |
| Zistenie vlastnej verejnej IP | `https://api.ipify.org?format=json`     | GET    | `loadMyIp()` |
| Geolokácia IP alebo domény    | `https://ipapi.co/{query}/json/`        | GET    | `lookup()`   |

Polia z odpovede ipapi.co, ktoré stránka spracúva: `ip`, `country_name`, `region`, `city`, `org`, `timezone`, `country_code`, `postal`, `continent_code`, `latitude`, `longitude`. Prvých deväť sa vypisuje ako riadky výsledkových kariet, `latitude` a `longitude` slúžia výlučne na zostavenie odkazu na Google Maps.

---

## 🛠️ Technológie

| Vrstva              | Riešenie                                                                        |
| ------------------- | ------------------------------------------------------------------------------- |
| Markup              | HTML5, jeden súbor, bez frameworku                                               |
| Štýly               | Inline CSS, custom properties, Grid + Flexbox, media queries                     |
| Logika              | Vanilla JavaScript (ES2017+: `async` / `await`, `fetch`, template literals, Clipboard API) |
| Typografia          | Inter z Google Fonts                                                             |
| Hosting             | GitHub Pages s vlastnou doménou cez `CNAME`                                      |
| Build / závislosti  | Žiadne - nula package managerov, nula lockfilov, nula vendorovaných knižníc      |

---

## 🌍 Nasadenie na GitHub Pages

1. Pushni repozitár na GitHub.
2. V nastaveniach repozitára zapni GitHub Pages so zdrojom z rootu default branchu.
3. Súbor `CNAME` už obsahuje `ipinfo.apoliak.online`, takže Pages naviaže vlastnú doménu.
4. Nasmeruj DNS záznam domény na GitHub Pages.

Zmena domény znamená úpravu súboru `CNAME` a príslušného DNS záznamu. Žiadny server runtime, databáza, environment premenné ani API kľúče nie sú potrebné.

---

## ⚠️ Známe obmedzenia

- 🚫 **IPv6 nie je podporované pri lookupe.** Regex v `detectKind()` matchuje iba IPv4 dotted-quad, IPv6 adresa je odmietnutá ešte pred odoslaním requestu.
- 🏷️ **Badge `IPv4` je hardcodovaný.** `loadMyIp()` vykreslí badge s textom `IPv4` bez ohľadu na to, čo ipify vrátilo - je to statický reťazec v šablóne, nie hodnota odvodená z odpovede. Pri aktuálne používanom endpointe `api.ipify.org` to nevadí, ale prípadný prechod na dual-stack `api64.ipify.org` by z tohto badge-u urobil klamlivý údaj.
- 🧨 **Chýba escapovanie pri renderovaní.** `renderResult()` a `item()` skladajú HTML cez template literals a priraďujú ho cez `innerHTML` bez sanitizácie. Validácia vstupu obmedzuje, čo môže prejsť z dotazu, ale nepriateľská alebo kompromitovaná odpoveď z API by sa vykreslila ako živý markup.
- 🌐 **Tvrdá závislosť na dvoch cudzích službách.** Ak je `api.ipify.org` alebo `ipapi.co` nedostupné, blokované ad-blockerom či DNS filtrom, alebo prekročíš rate limit free tieru, stránka zobrazí iba všeobecnú chybovú hlášku. Kód nevie odlíšiť rate limit od neplatného dotazu - oboje skončí hláškou `Lookup zlyhal`.
- 🔐 **Súkromie.** IP každého návštevníka odchádza na `api.ipify.org`, každý dotaz na `ipapi.co` a font Inter sa sťahuje z CDN Googlu. Stránka neobsahuje žiadne privacy oznámenie.
- 🧭 **Doménové dotazy nie sú garantované.** Dokumentovaný endpoint ipapi.co je určený pre IP adresy; kód posiela doménové mená na tú istú cestu. V praxi to zvyčajne funguje, ale nejde o garantovaný kontrakt.
- 💾 **Téma sa neukladá.** Nikde nie je zápis do `localStorage`, takže po reloade sa prepínač vráti na preferenciu operačného systému.
- 🎨 **Nedokončený štýl odkazu na mapu.** `renderResult()` generuje `<a class="map-link">`, ale pravidlo `.map-link` v stylesheete neexistuje a odkaz sa vkladá priamo do kontajnera `.list` namiesto do `.item`, takže sa vykreslí neostýlovaný a nezarovnaný s ostatnými riadkami.
- 🧪 **Žiadne testy, linting ani CI.** Kvalita stojí výlučne na manuálnej kontrole v prehliadači.
- 🗣️ **Iba slovenčina.** Celé rozhranie vrátane chybových hlášok je slovenské, i18n mechanizmus nie je pripravený.

---

## 📄 Licencia

V repozitári je súbor `LICENSE`, ale je **neúplný**. Obsahuje iba tri riadky:

```text
MIT License

Copyright (c) 2026
```

Chýba samotné udelenie práv, podmienky, vylúčenie záruk aj meno držiteľa autorských práv. Zámer je zjavne MIT, no v aktuálnom znení súbor nikomu žiadne práva neudeľuje. Pred akýmkoľvek použitím projektu treba do `LICENSE` doplniť plné znenie MIT licencie a meno autora.

---

<div align="center">

Vytvoril **Alex Poliak** - [GitHub](https://github.com/Apoliak7777) - [alexpoliak21@gmail.com](mailto:alexpoliak21@gmail.com)

</div>
