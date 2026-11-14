# Birgir Design System

> **Editorial design system för landningssidor och lead magnets.**
> Lugn, varm, svensk. Som en välskriven essä, inte en amerikansk kursplattform.

**Status:** v2.0, maj 2026.
Konsoliderad från `BRAND.md v0.2` + den interaktiva `colors_and_type.css` + den
publicerade Brand Style Guide v1.1 (PDF). All token-, komponent- och tonregel
i ett dokument. Inga em-dashes.

**Använd så här i nya projekt:**
Dra in `BRAND.md` (denna fil) i prompten till Claude Code, Codex eller
liknande. Den är källkoden för designsystemet. Lägg till en pattern-fil från
`patterns/` om projektet matchar (t.ex. `LANDING-PAGE.md` när du bygger en
landningssida). Bygger du något som inte finns som pattern, följ bara den här
filen och improvisera sektionsstrukturen utifrån projektets behov.

---

## 1. Andan i varumärket

Allt jag bygger ska kännas **lugnt, editoriellt, varmt och svenskt**.
En människa som pratar med en människa, i andra person, med låg volym och
konkreta löften.

### Vibe-orden
1. **Lugn.** Inga popups. Inga skräpmejl. Sidan andas. Animation default
   `0.15s ease`, inga bouncar, inga parallax.
2. **Editoriell.** Source Serif 4 i rubriker, ett kursivt amber-ord som
   accent. Luft mellan stycken, inte fyllnadstext.
3. **Varm.** Crème som sidans bas, aldrig vitt. Skuggor i varma toner.
   Borders i ink, aldrig kallgrå.
4. **Svensk.** Svenska först. Andra person. Ärlig microcopy som beskriver
   gränser, inte hypar dem.

### Inspiration
Stripe Press, Robin Sloan-newslettern, amerikansk indie-publishing från sent
10-tal. *Som att vända blad i en bok, inte som att köpa något på QVC.*

---

## 2. Voice & tone

### Skriv så här
- **Andra person, lågt tonläge:** "Jag mejlar dig", "Du kan avregistrera dig"
- **Korta, raka meningar.** Inga halvkilometers-meningar.
- **Konkret, inte generiskt:** "ett mejl i veckan" slår "regelbundet"
- **Ärlighet om begränsningar:** "Inga skräpmejl", "Du kan när som helst",
  "ingen e-post krävs"
- **Mikrocopy under knappar** (förstärker trygghet, ej hype)
- **Svenska först.** Engelska bara om termen saknar bra svensk översättning
  ("vibe coding", "AI-insikt")
- **Sentence case överallt.** Rubriker, knappar, navigation. Versaler bara i
  eyebrow: "VECKOBREV", "VAD DU FÅR".

### Undvik
- Versaler för betoning ("BÄST PÅ MARKNADEN")
- Utropstecken (max ett per sida, helst inget)
- Emojis i brödtext (sparsamt i rubriker, aldrig som primärt CTA-element)
- Säljklichéer ("revolutionerande", "exklusivt erbjudande", "missa inte")
- "vi" som om det vore ett företag, det är *jag* (Birgir)
- **Em-dash (—) är förbjudet.** Använd alltid antingen komma eller punkt
  + stor bokstav istället. Gäller rubriker, brödtext, mikrocopy, alt-text,
  CSS `content`-strängar, kodkommentarer och allt annat som hamnar i en
  publicerad sida. En-dash (–) för talintervall (t.ex. "7–8 minuter")
  är okej eftersom det inte är em-dash.

### Återkommande copy-mönster
| Plats               | Mönster                                                          |
|---------------------|------------------------------------------------------------------|
| Eyebrow / preheader | Versaler, ord-minimalt: "VECKOBREV", "VAD DU FÅR", "PRENUMERERA" |
| Rubrik-accent       | *Kursivt + amber* för ett betonat ord ("vibe coding", "på riktigt")  |
| Mikrotext under CTA | "Inga skräpmejl. Hanteras enligt GDPR."                          |
| Bekräftelse-rubrik  | "Tack! *Du är med.*"                                             |

---

## 3. Färgpalett

```css
:root {
  /* Cream, varma bakgrunder */
  --cream-50:  #FBF8F1;   /* sidans softaste yta, formulär, kort */
  --cream-100: #F5EFE2;   /* primär bakgrund */
  --cream-200: #ECE3CE;   /* alternerande sektion */
  --cream-300: #DED0AE;   /* sällan, accent */

  /* Ink, varm svart/gråskala */
  --ink-900: #1A1713;     /* primär text och dark sektioner */
  --ink-800: #2A2520;
  --ink-700: #3D362E;     /* brödtext, lead */
  --ink-500: #6B5F52;     /* dämpad text på cream */
  --ink-400: #8A7D6E;     /* mikrotext, footer-rader */
  --ink-300: #B8AD9E;     /* dämpad text på dark bakgrund */
  --ink-200: #D9D0BE;     /* borders */

  /* Amber, Claude-orange, primär accent (saffran) */
  --amber-700: #A25B2A;   /* hover på CTA, eyebrow-text, kursiv accent */
  --amber-600: #C66A2E;   /* primär CTA-bakgrund */
  --amber-500: #D97742;
  --amber-400: #E69068;
  --amber-100: #F5D7BF;   /* mjuk highlight, badge */
  --amber-50:  #FBEEE1;   /* felmeddelande-fyllning */

  /* Plum, sekundär accent, sparsamt */
  --plum-700: #3B1D1A;    /* en gång per sida, max */
}
```

### Användningsregler
- **Bakgrund:** alltid `cream-100` som standard, alternera med `cream-200`
  för sektionsbyte. Aldrig vitt.
- **Mörka sektioner** (footer, sticky CTA, slut-CTA): `ink-900` med
  `cream-100` text. Kommer in en gång per sida, vanligen sent.
- **Primär CTA:** `amber-600` bakgrund + vit text. Hover: `amber-700`.
  En primär CTA per sida, max.
- **Sekundär CTA på cream:** `ink-900` bakgrund + vit text (svart knapp).
- **Eyebrow / länkar:** `amber-700`.
- **Brödtext:** `ink-900` på cream, `ink-700` för lead, `ink-500` för
  dämpad sekundärtext.
- **Plum:** sekundär accent, en viskning. Aldrig gradient, aldrig wash.

### Semantiska tokens
```css
:root {
  --bg:               var(--cream-100);
  --bg-alt:           var(--cream-200);
  --bg-soft:          var(--cream-50);
  --bg-dark:          var(--ink-900);
  --fg:               var(--ink-900);
  --fg-muted:         var(--ink-500);
  --fg-subtle:        var(--ink-400);
  --fg-on-dark:       var(--cream-100);
  --fg-on-dark-muted: var(--ink-300);
  --border:           var(--ink-200);
  --link:             var(--amber-700);
  --accent:           var(--amber-700);
  --cta:              var(--amber-600);
  --cta-hover:        var(--amber-700);
}
```

---

## 4. Typografi

```
Display (rubriker):   Source Serif 4 (400, 500, 600, italic)
Body (brödtext, UI):  Inter (400, 500, 600, 700)
Mono (numrering):     JetBrains Mono (400, 500)
```

Ladda från Google Fonts. I Next.js: `next/font/google`.

```css
@import url("https://fonts.googleapis.com/css2?family=Source+Serif+4:ital,opsz,wght@0,8..60,400..700;1,8..60,400..700&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap");

:root {
  --serif: "Source Serif 4", Georgia, "Times New Roman", serif;
  --sans:  "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --mono:  "JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, monospace;
}
```

### Skala
| Roll                | Font                                | Storlek                           | Övrigt |
|---------------------|-------------------------------------|-----------------------------------|--------|
| Hero h1             | Source Serif 4 600                  | `clamp(40px, 5.4vw, 72px)`        | line-height 1.05, letter-spacing -0.02em |
| h2                  | Source Serif 4 600                  | `clamp(28px, 3.4vw, 40px)`        | line-height 1.15 |
| h3                  | Source Serif 4 600                  | `clamp(18px, 1.8vw, 22px)`        | line-height 1.25 |
| Brödtext            | Inter 400                           | 17px                              | line-height 1.6 |
| Lead                | Inter 400 eller Source Serif 4 400  | 19px                              | line-height 1.55, ink-700 |
| Eyebrow / preheader | Inter 600                           | 12.5px                            | uppercase, letter-spacing 0.17em, amber-700 |
| Mikrotext           | Inter 400                           | 12.5px                            | ink-400 |
| Knapp               | Inter 600                           | 15px (lg: 16px)                   | line-height 1 |
| Mono numrering      | JetBrains Mono 500                  | 12px                              | letter-spacing 0.02em, amber-700 |

### Den signaturmässiga rubrik-accenten
Ett ord eller två i en rubrik sätts i **kursiv + amber-700**, med
Source Serif 4 italic 500. Det är min visuella signatur. **Max ett
amber-italic-ord per rubrik.** Två gör meningen till reklam.

```html
<h1>Veckobrev om AI, etik och <em class="serif-italic">vibe coding</em></h1>
```

```css
.serif-italic {
  font-family: var(--serif);
  font-style: italic;
  font-weight: 500;
  color: var(--amber-700);
}
```

---

## 5. Spacing, radie, skuggor

```css
:root {
  /* Radii */
  --r-sm: 6px;    /* små badges */
  --r-md: 10px;   /* knappar, input */
  --r-lg: 16px;   /* kort, formulär */
  --r-xl: 24px;   /* stora kort, modaler */

  /* Layout */
  --container:        1180px;
  --container-narrow: 760px;       /* för text-tunga sidor */
  --pad-x:            32px;
  --pad-x-mobile:     22px;
  --section-y:        96px;        /* desktop */
  --section-y-mobile: 64px;

  /* Skuggor, alltid varma, aldrig grå */
  --shadow-sm:  0 1px 2px rgba(26,23,19,0.06), 0 1px 1px rgba(26,23,19,0.04);
  --shadow-md:  0 4px 16px rgba(26,23,19,0.08), 0 2px 4px rgba(26,23,19,0.04);
  --shadow-lg:  0 20px 60px rgba(26,23,19,0.12), 0 8px 20px rgba(26,23,19,0.06);
  --shadow-cta: 0 10px 30px -8px rgba(198,106,46,0.55);

  /* Transitions */
  --t-fast: 0.15s ease;
  --t-base: 0.2s ease;
}
```

---

## 6. Komponenter

### 6.1 Knappsystem
Tre roller: amber för det enda primära löftet, ink för en sekundär
bekräftelse, ghost för "läs mer". **Aldrig två amber-knappar i samma vyport.**

```css
.btn {
  display: inline-flex; align-items: center; justify-content: center; gap: 10px;
  font-family: var(--sans); font-weight: 600; font-size: 15px;
  padding: 14px 22px; border-radius: var(--r-md);
  border: 1px solid transparent; cursor: pointer; line-height: 1;
  transition: background-color var(--t-fast), color var(--t-fast),
              box-shadow var(--t-fast), border-color var(--t-fast),
              transform var(--t-fast);
}
.btn:active        { transform: translateY(1px); }
.btn-amber         { background: var(--amber-600); color: #fff; box-shadow: var(--shadow-cta); }
.btn-amber:hover   { background: var(--amber-700); }
.btn-primary       { background: var(--ink-900);   color: #fff; }
.btn-primary:hover { background: var(--ink-800); }
.btn-ghost         { background: transparent; color: var(--ink-900); border-color: var(--ink-200); }
.btn-ghost:hover   { background: var(--cream-50); border-color: var(--ink-300); }
.btn-lg            { font-size: 16px; padding: 18px 28px; }
```

| Roll      | Token         | Användning                                              |
|-----------|---------------|---------------------------------------------------------|
| Amber     | `.btn-amber`  | Primär CTA, en per sida ("Prenumerera", "Köp")          |
| Primary   | `.btn-primary`| Sekundär bekräftelse. På ink-band: cream-bg + ink-text. |
| Ghost     | `.btn-ghost`  | "Läs mer", tertiär. Aldrig ensam på sidan.              |
| Storlek lg| `.btn-lg`     | Hero-CTA. Påslagsbar på amber och primary.              |

**Microcopy under primär CTA:** Inter 400, 12.5px, ink-400, en rad,
en mening. "Inga skräpmejl. Hanteras enligt GDPR."

### 6.2 Input-fält
```css
.input {
  width: 100%; padding: 14px 16px;
  border-radius: var(--r-md);
  border: 1px solid var(--ink-200);
  background: #fff;
  font: inherit; color: var(--fg);
  transition: border-color var(--t-fast), box-shadow var(--t-fast);
}
.input::placeholder { color: var(--ink-400); }
.input:focus {
  outline: none;
  border-color: var(--amber-600);
  box-shadow: 0 0 0 3px rgba(198,106,46,0.15);
}
.input.is-error {
  border-color: var(--amber-600);
  background: var(--amber-50);
}
```

Fokus är **alltid amber, aldrig blått eller grönt**. Felmeddelanden visas
inline under fältet i amber-700 text på amber-50 fyllning. Aldrig toast.

### 6.3 Eyebrow
```css
.eyebrow {
  font-family: var(--sans); font-weight: 600;
  font-size: 12.5px; letter-spacing: 0.17em;
  text-transform: uppercase; color: var(--amber-700);
  margin: 0 0 12px;
}
```

```html
<p class="eyebrow">Veckobrev</p>
<h2>Häng med i <em class="serif-italic">veckobrevet</em>.</h2>
```

### 6.4 Kort med 01/02/03 numrering
Cream-50 bakgrund, ink-200 border, 28px padding, 16px radie, varm
shadow-sm. Mono-numrering i amber-700 ovanför titeln. **Aldrig ikoner,
aldrig emoji som markörer.** Använd alltid **tre kort** i en sektion.
Behöver du fler punkter, gör om sektionen till en numrerad lista i ett
textstycke.

```css
.card {
  background: var(--cream-50);
  border: 1px solid var(--ink-200);
  border-radius: var(--r-lg);
  padding: 28px;
  box-shadow: var(--shadow-sm);
}
.card .num {
  font-family: var(--mono); font-weight: 500;
  font-size: 12px; letter-spacing: 0.06em;
  color: var(--amber-700);
}
```

```html
<article class="card">
  <span class="num">01</span>
  <h4>Du får ett mejl i veckan</h4>
  <p>7 till 8 minuters läsning. En tanke om AI, en hands-on idé.</p>
</article>
```

### 6.5 Formulär
- Eyebrow + rubrik + lead ovanför formuläret
- Inputs i `cream-50` eller vit, varma `ink-200`-borders
- Felmeddelanden inline i kortet, amber-varning, ingen toast
- GDPR-checkbox där personuppgifter samlas in
- Mikrotext under primär knapp som beskriver gränser, lovar inget

### 6.6 Hero
Eyebrow → serif-rubrik med kursivt amber-ord → lead 19px → primär CTA
+ ghost → microcopy. Aldrig en gradient, aldrig en stockbild.

### 6.7 Footer
Alltid `ink-900`, cream-text, fyra kolumner: wordmark + tagline,
navigation, projekt/kontakt, juridik. Tunn linje (1px solid
`rgba(217,208,190,0.18)`) delar topp och bottenrad.

**Kolumnrubriker:** mono 8pt, letter-spacing 0.16em, amber-500.
**Länkar:** Inter 10pt, cream-100. **Microtext:** ink-300.

**Aldrig:** "Built with ♥", sociala ikoner som radband, nyhetsbrevsruta i
footern (den hör till sin egen sektion).

---

## 7. Sektionsmallar

En landningssida växlar mellan `cream-100` och `cream-200`. Ett
ink-band kan komma in en gång, sent på sidan, för att hålla ihop en
slut-CTA. Padding `--section-y` (96px desktop, 64px mobil).

| Mall                   | Bakgrund    | Notering                                  |
|------------------------|-------------|-------------------------------------------|
| Hero                   | cream-100   | Eyebrow + serif h1 + lead + CTA-par       |
| Vad du får (3 kort)    | cream-200   | 01 / 02 / 03, mono-numrering              |
| Editorial textblock    | cream-100   | 760px container, Source Serif 4 lead      |
| Citat / pullquote      | cream-200   | Centrerat, kursiv serif, amber-citattecken|
| Final CTA-band         | ink-900     | Slut på sidan, en gång                    |
| Formulär-sektion       | cream-100   | cream-50 kort, centrerat                  |

---

## 8. MailerLite-integration

Standard-stack för all mejlfångst:

- **Endpoint i projektet:** `POST /api/subscribe` (Next.js route handler)
- **Bearer token** från env-var `MAILERLITE_API_KEY`
- **MailerLite endpoint:** `https://connect.mailerlite.com/api/subscribers`
- **Payload:**
  ```json
  {
    "email": "...",
    "fields": { "name": "..." },
    "groups": ["<group_id_per_projekt>"],
    "status": "active"
  }
  ```
- **Validering serverside:** namn 1 till 80 tecken, e-post regex,
  GDPR-checkbox måste vara true
- **Felmeddelanden inline** i formulärkortet
- **Efter success:** redirect till `/tack` eller motsvarande

### MailerLite-grupper per projekt
| Projekt                       | Group ID              |
|-------------------------------|-----------------------|
| nyhetsbrev.birgir.se          | `185528538957350071`  |
| forsta-webbapp.birgir.se      | `185432089867322593`  |

### GDPR-checkbox
> "Jag godkänner att Birgir Birgisson lagrar mitt namn och min e-postadress
> för att skicka [veckobrevet/guiden/kursinformationen]. Jag kan när som helst
> avregistrera mig eller begära att mina uppgifter raderas."

### Mikrotext under knapp
> "Inga skräpmejl. Hanteras enligt GDPR."

---

## 9. Bilder, ikoner, logotyp

### Wordmark
"Birgir." i Source Serif 4 600, ink-900 bokstäver, amber-600 punkt.
Tre exporterade varianter:

| Fil                                  | Användning                                    |
|--------------------------------------|-----------------------------------------------|
| `birgir-wordmark-transparent.png`    | Mot cream eller ljusa bakgrunder              |
| `birgir-wordmark-cream.png`          | Med inbäddad cream-100 bakgrund (1200×400)    |
| `birgir-wordmark-white.png`          | Vita bokstäver + amber-prick mot mörk bakgrund|

### Porträtt
Kvadratiskt porträtt, beskuret rund. Min 400×400, helst 600×600.
Lägg i `public/images/birgir.jpg`. I "Om mig"-sektion: 200px diameter
desktop, 160px mobil.

### Favicon
Delad SVG på mörk bakgrund med gold gradient. Kopiera till `app/icon.svg`
i nya Next.js-projekt.

### OG-bild
Generera dynamiskt med Next.js `opengraph-image.tsx`. Layout: cream
bakgrund, eyebrow uppe, stor serif-rubrik (med italic accent),
"birgir.se" nere till vänster.

---

## 10. Tekniskt standardstack

| Lager        | Val                                                  |
|--------------|------------------------------------------------------|
| Framework    | Next.js (App Router) + TypeScript                    |
| Styling      | Tailwind CSS v4 + tokens.css för designsystemet      |
| Hosting      | Vercel                                               |
| Domän        | `<projektnamn>.birgir.se`                            |
| Analytics    | Vercel Analytics (`@vercel/analytics/next`)          |
| Email        | MailerLite via serverside API-route                  |
| SEO          | `metadata` API + `robots.ts` + `sitemap.ts` + `opengraph-image.tsx` |
| Repo         | GitHub (org: `digitalutveckling101`)                 |

---

## 11. Do och Don't

### Do
- ✓ Skriv i andra person ("Jag mejlar dig en gång i veckan")
- ✓ Ett kursivt amber-ord per rubrik (saffranregeln)
- ✓ Crème som sidans bas, alternera cream-100 och cream-200
- ✓ Microcopy som beskriver gränser, inte hypar
- ✓ Numrera i mono, 01 / 02 / 03, i amber-700
- ✓ Sentence case överallt, versaler bara i eyebrow
- ✓ En primär CTA per sida (amber)
- ✓ Tre kort per sektion, inte fyra eller fem

### Don't
- ✗ Em-dash (—) i copy. Använd komma eller punkt + stor bokstav.
- ✗ Gradienter, brus, isometriska bilder, 3D-blobbar
- ✗ Två CTA-färger på samma sida
- ✗ "Vi" som om det vore ett företag, det är jag (Birgir)
- ✗ Versaler för betoning (GRATIS, MISSA INTE)
- ✗ Popups, autoöppnande nyhetsbrev, cookie-väggar
- ✗ Stockfoton av "team i kostym"
- ✗ Generiska SaaS-illustrationer
- ✗ Mörkt läge-toggle (cream är hela poängen)
- ✗ Auto-popup för newsletter
- ✗ Cookie-banner som tar halva sidan
- ✗ "Hej, jag heter ChatGPT", **Claude** är assistenten på mina sajter
- ✗ "Built with ♥" eller sociala ikoner som radband i footern

---

## 12. Hur du använder den här filen i nytt Claude/Codex-projekt

I första prompten:

> Bygg [vad du bygger] för [projekt].
>
> Designsystemet ligger i `BRAND.md` (denna fil), klistra in eller länka till
> [URL till repo]. Följ den exakt: färger, typsnitt, komponenter, tonalitet,
> do/don't.
>
> Tekniskt: Next.js App Router + TypeScript + Tailwind v4 + Vercel.
> Email: MailerLite via `/api/subscribe`.
>
> [Om det är en sidtyp som har en pattern-fil:]
> Utöver BRAND.md, här är `patterns/LANDING-PAGE.md` (eller `SALES-PAGE.md`)
> som beskriver vilka sektioner sidan ska ha.
>
> [Om det inte finns en pattern:]
> Sektionsstrukturen sätter vi tillsammans baserat på projektets behov.
> Använd BRAND.md som DNA, men improvisera uppbyggnaden.
>
> Innehåll: [klistra in copy och struktur här]
>
> Referenssajter att titta på live:
> - https://nyhetsbrev.birgir.se
> - https://forsta-webbapp.birgir.se
> - https://vibe-coding-startkit.birgir.se

---

## Versionshistoria

- **v2.0** (maj 2026): Konsoliderad version. Slog ihop `BRAND.md v0.2`,
  den interaktiva `colors_and_type.css` och den publicerade Brand Style
  Guide v1.1. Lade till semantiska tokens, hero-mall, dedikerad
  sektionsmallstabell, ren do/don't-lista, wordmark-varianttabell,
  utbyggd typografiskala. Allt em-dash-fritt.
- **v0.2** (2026-04-23): Refaktor. Flyttade ut sektionsstrukturer för
  landing och sales till `patterns/`. Ren designsystem-fil.
- **v0.1** (2026-04-23): Första utkastet. Destillerat från
  `vibe-coding-startkit`, `forsta-webbapp` och nyhetsbrev-bygget.
