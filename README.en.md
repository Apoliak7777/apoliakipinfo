<div align="center">

[![Slovencina](https://img.shields.io/badge/SK-Sloven%C4%8Dina-30363d?style=for-the-badge)](README.md) [![English](https://img.shields.io/badge/EN-English-2ea043?style=for-the-badge)](README.en.md)

</div>

<div align="center">

# 🌐 IP Infos

**A single-file static website that shows your public IP address and looks up geolocation data for any IPv4 address or domain.**

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?style=flat-square&logo=githubpages&logoColor=white)
![Zero Build](https://img.shields.io/badge/build-none-success?style=flat-square)

[Live demo](https://ipinfo.apoliak.online) - [Quick start](#-quick-start) - [Known limitations](#️-known-limitations)

</div>

---

## 📑 Table of contents

- [🔎 Overview](#-overview)
- [✨ Features](#-features)
- [🚀 Quick start](#-quick-start)
- [📁 Project structure](#-project-structure)
- [🔌 APIs used](#-apis-used)
- [🛠️ Tech stack](#️-tech-stack)
- [🌍 Deploying to GitHub Pages](#-deploying-to-github-pages)
- [⚠️ Known limitations](#️-known-limitations)
- [📄 License](#-license)

---

## 🔎 Overview

**IP Infos** is a network utility page that automatically detects your public IP address on load and lets you look up geolocation data for any IPv4 address or domain - country, region, city, provider, time zone and a map link based on the coordinates.

The entire application lives in a single `index.html` file: markup, CSS and JavaScript are all inline. There is no backend, no build step, no package manager and no dependency that would need installing. The data comes straight from the browser via two public APIs.

The user interface is entirely in Slovak (`<html lang="sk">`) and the project is deployed as a GitHub Pages site on a custom domain.

---

## ✨ Features

- 🛰️ **Automatic detection of your own IP** - right after the page loads, `api.ipify.org` is called and the returned public IP is displayed in large type along with status badges.
- 🌍 **IP or domain lookup** - type an address into the field and press `Hľadať info` or `Enter`; the data is fetched from `ipapi.co`.
- ✅ **Client-side validation** - the `detectKind()` function checks the input against a strict IPv4 regex or a domain regex before any network call is made.
- 🧹 **Input normalization** - `cleanInput()` trims whitespace, strips `http://` / `https://` and a trailing slash, so pasting a full URL works too.
- 📊 **Two result cards** - `Výsledok` shows the query, IP, country, region, city and provider; `Detaily` shows the input type, timezone, country code, postal code and continent.
- 🗺️ **Google Maps link** - if the API returns both latitude and longitude, an `Otvoriť mapu` link is added that opens in a new window. The numeric coordinates themselves are never printed anywhere, they are only used to build the URL.
- 📋 **Copy IP** - the `Kopírovať moju IP` button uses the Clipboard API; if it fails, the IP is inserted directly into the input field, selected, and a warning message is shown.
- ⚡ **Use my IP** - fills in your own IP with a single click and immediately runs the lookup.
- 🌓 **Dark and light theme toggle** - toggles the `data-theme` attribute on `<html>`; the initial theme is taken from the operating system setting via `prefers-color-scheme`.
- 💬 **Color-coded status messages** - success / error / warning messages, plus a disabled button while a request is in flight.
- 📱 **Fully responsive layout** - single column on mobile, a two-column grid above 760px, `viewport-fit=cover` for devices with a notch.

---

## 🚀 Quick start

There is nothing to install and nothing to build. The repository contains no `package.json`, `requirements.txt`, `Makefile` or any build script.

### Simplest - open the file directly

Windows (`cmd.exe` or PowerShell):

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

### Recommended - serve it over a local HTTP server

```bash
python -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

Alternatively, if you have Node.js:

```bash
npx serve .
```

> **Tip:** When opened over `file://`, `navigator.clipboard` may be unavailable or fail in some browsers. In that case the `Kopírovať moju IP` button falls back - it inserts the IP into the input field and shows a warning message. A local HTTP server on `localhost` solves this problem.

> **Warning:** The application always needs an active internet connection, even locally. All displayed data is pulled live from external APIs.

---

## 📁 Project structure

```text
apoliakipinfo/
├── index.html    # the whole application - markup + inline <style> + inline <script>
├── CNAME         # GitHub Pages custom domain binding (ipinfo.apoliak.online)
├── LICENSE       # MIT - but only a fragment, see the License section
├── README.md     # Slovak version
└── README.en.md  # this file - English version
```

Internal breakdown of `index.html` (303 lines):

| Lines   | Contents                                                                                                        |
| ------- | -------------------------------------------------------------------------------------------------------------- |
| 10-112  | Inline `<style>` - CSS variables for the dark and light theme, cards, grid layout, media queries                 |
| 114-185 | Markup - topbar with the logo and theme toggle, hero card, own-IP card, controls, two result cards               |
| 187-301 | Inline `<script>` - the `state` object, the `el` element cache, functions and event listener bindings            |

Key functions in the script:

| Function                                    | What it does                                         |
| ------------------------------------------- | ---------------------------------------------------- |
| `setTheme()` / `detectSystemTheme()`        | Switches the theme and reads the OS preference        |
| `cleanInput()`                              | Normalizes the input (trim, strips protocol and slash) |
| `detectKind()`                              | Recognizes an IPv4 or a domain, otherwise returns an empty string |
| `loadMyIp()`                                | Fetches your own public IP from ipify                 |
| `lookup()`                                  | Performs the geolocation query against ipapi.co       |
| `renderResult()`                            | Renders both result cards and the optional map link   |
| `copyMyIp()` / `useMyIp()` / `clearResults()` | Handle the remaining buttons                        |

The bootstrap is the last two lines of the script: `setTheme(detectSystemTheme())` and `loadMyIp()`.

---

## 🔌 APIs used

Both URLs are hardcoded directly in `index.html`. Neither one requires an API key or registration.

| Purpose                       | Endpoint                                | Method | Called in    |
| ----------------------------- | --------------------------------------- | ------ | ------------ |
| Detecting your own public IP  | `https://api.ipify.org?format=json`     | GET    | `loadMyIp()` |
| Geolocation of an IP or domain | `https://ipapi.co/{query}/json/`        | GET    | `lookup()`   |

The fields from the ipapi.co response that the page processes: `ip`, `country_name`, `region`, `city`, `org`, `timezone`, `country_code`, `postal`, `continent_code`, `latitude`, `longitude`. The first nine are printed as rows in the result cards, while `latitude` and `longitude` are used solely to build the Google Maps link.

---

## 🛠️ Tech stack

| Layer               | Solution                                                                        |
| ------------------- | ------------------------------------------------------------------------------- |
| Markup              | HTML5, a single file, no framework                                               |
| Styles              | Inline CSS, custom properties, Grid + Flexbox, media queries                     |
| Logic               | Vanilla JavaScript (ES2017+: `async` / `await`, `fetch`, template literals, Clipboard API) |
| Typography          | Inter from Google Fonts                                                          |
| Hosting             | GitHub Pages with a custom domain via `CNAME`                                    |
| Build / dependencies | None - zero package managers, zero lockfiles, zero vendored libraries           |

---

## 🌍 Deploying to GitHub Pages

1. Push the repository to GitHub.
2. In the repository settings, enable GitHub Pages with the source set to the root of the default branch.
3. The `CNAME` file already contains `ipinfo.apoliak.online`, so Pages will bind the custom domain.
4. Point the domain's DNS record at GitHub Pages.

Changing the domain means editing the `CNAME` file and the corresponding DNS record. No server runtime, database, environment variables or API keys are needed.

---

## ⚠️ Known limitations

- 🚫 **IPv6 is not supported in lookups.** The regex in `detectKind()` only matches an IPv4 dotted quad, so an IPv6 address is rejected before the request is even sent.
- 🏷️ **The `IPv4` badge is hardcoded.** `loadMyIp()` renders a badge reading `IPv4` regardless of what ipify actually returned - it is a static string in the template, not a value derived from the response. With the currently used `api.ipify.org` endpoint this is harmless, but switching to the dual-stack `api64.ipify.org` would turn the badge into a misleading label.
- 🧨 **No escaping when rendering.** `renderResult()` and `item()` assemble HTML via template literals and assign it through `innerHTML` without sanitization. Input validation limits what can get through from the query, but a hostile or compromised API response would be rendered as live markup.
- 🌐 **Hard dependency on two third-party services.** If `api.ipify.org` or `ipapi.co` is unavailable, blocked by an ad blocker or a DNS filter, or you exceed the free tier rate limit, the page only shows a generic error message. The code cannot tell a rate limit apart from an invalid query - both end up with the `Lookup zlyhal` message.
- 🔐 **Privacy.** Every visitor's IP goes out to `api.ipify.org`, every query goes to `ipapi.co`, and the Inter font is downloaded from Google's CDN. The page contains no privacy notice.
- 🧭 **Domain queries are not guaranteed.** The documented ipapi.co endpoint is meant for IP addresses; the code sends domain names to that same path. In practice it usually works, but it is not a guaranteed contract.
- 💾 **The theme is not persisted.** Nothing is ever written to `localStorage`, so after a reload the toggle falls back to the operating system preference.
- 🎨 **Unfinished styling of the map link.** `renderResult()` generates `<a class="map-link">`, but no `.map-link` rule exists in the stylesheet, and the link is inserted directly into the `.list` container instead of into an `.item`, so it renders unstyled and misaligned with the other rows.
- 🧪 **No tests, no linting, no CI.** Quality rests entirely on manual checks in the browser.
- 🗣️ **Slovak only.** The whole interface, error messages included, is in Slovak, and no i18n mechanism is in place.

---

## 📄 License

The repository contains a `LICENSE` file, but it is **incomplete**. It holds only three lines:

```text
MIT License

Copyright (c) 2026
```

The actual grant of rights, the conditions, the warranty disclaimer and the name of the copyright holder are all missing. The intent is clearly MIT, but as currently worded the file grants no rights to anyone. Before using the project in any way, the full text of the MIT license and the author's name need to be added to `LICENSE`.

---

<div align="center">

Built by **Alex Poliak** - [GitHub](https://github.com/Apoliak7777) - [alexpoliak21@gmail.com](mailto:alexpoliak21@gmail.com)

</div>
