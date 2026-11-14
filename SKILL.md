# /mini-course, generera en mini-kurs från innehåll

Tar källinnehåll (transkript, markdown, URL till video) och bygger en polerad,
fristående single-page mini-kurs i Birgir Birgissons brand. Resultatet är en
ren `index.html` + `favicon.svg` redo att deploya till Vercel.

## Hur du kör skillen

Antingen via slash-kommando om skillen är installerad globalt:

```
/mini-course [URL eller filsökväg]
```

Eller, om filen `SKILL.md` ligger i projektroten, säg i klartext:

> "Hej. Läs SKILL.md och bygg en kurs från [URL eller fil]."

Claude läser SKILL.md som en vanlig instruktionsfil och följer den.

**Exempel:**

```
/mini-course https://www.youtube.com/watch?v=ABC123
/mini-course transcripts/min-video.txt
/mini-course "YouTube SEO" från Research/seo-anteckningar.md
```

## Hårda regler (icke-förhandlingsbara)

Det här är vad som gör kurserna konsekventa över tid. Bryt aldrig mot dem.

1. **Fristående landningssida.** Ingen referens till källvideon, transkriptet eller talaren i den renderade kursen. Inga "i videon", "baserad på X", "ursprungsvideo", "26-min video", inga YouTube-länkar, ingen "X sa". Citat från källan blir fristående principer utan attribution. Källan stannar i commit-meddelanden och `transcripts/`-mappen, aldrig i HTML:en.
2. **Inga em-dashar någonstans.** Använd komma eller punkt + stor bokstav istället. Gäller copy, rubriker, mikrocopy, alt-text, CSS `content`-strängar, kodkommentarer. En-dash för talintervall ("7–8 minuter") är okej.
3. **BRAND.md är överordnad designsanning.** Läs `BRAND.md` i projektroten innan du bygger HTML:en. Följ färger, typografi, ton och don'ts exakt. Cream-bakgrund, ink-text, amber CTA, Source Serif 4 + Inter + JetBrains Mono. Inga gradient-bakgrunder, inget mörkt-läge-toggle, inga utropstecken som betoning.
4. **Favicon obligatorisk.** Generera `favicon.svg` enligt `config.favicon`-blocket. Default: K i amber-600 på ink-800. Inkludera `<link rel="icon" type="image/svg+xml" href="favicon.svg" />` direkt efter `<meta name="theme-color">` i `<head>`.
5. **Bra jobbat-avslutning + nyhetsbrevs-CTA.** Sista sektionen efter quizet är "Bra jobbat. Du tog dig igenom hela mini-kursen." följt av ett ljust nyhetsbrevs-kort (cream-50 på mörk ink-bakgrund) som länkar till `config.newsletter.url`. Korten innehåller alltid bullets enligt `config.newsletter.structure` och mikrocopyn enligt `config.newsletter.microcopy`. Knappens text: "Prenumerera på veckobrevet".
6. **Birgir Birgisson finns endast som kurs-författare.** Topbar-prick + footer-copyright. Aldrig som narrator i brödtexten ("Birgir gjorde X", "Birgirs metod"). Använd alltid instruktionsröst ("du gör X", "tumregel") så samma mall fungerar för vilken källa som helst.
7. **Svenska som default.** Kursen är på svenska om inte användaren explicit säger annat.
8. **Signaturen kursiv-amber.** Ett ord per rubrik i `<em class="serif-italic">` (Source Serif 4 italic 500, amber-700). Birgirs visuella signatur, finns på alla rubriker.

## Steg för steg

### Steg 0, kontroll av setup

Läs `config.json` i projektroten. Den borde finnas. Om inte: använd defaulterna nedan, fråga inte. Användaren har redan dragit in starter-kit-filerna.

```json
{
  "brandName": "Birgir Birgisson",
  "outputDir": "./",
  "brandFile": "BRAND.md",
  "language": "sv"
}
```

Saknas `BRAND.md`: säg till användaren och avbryt, för design utan BRAND.md går mot punkt 3 ovan.

### Steg 1, hämta källinnehåll

**Om YouTube-URL:**
- Kör `./get-transcript.sh "<URL>" transcripts/<slug>.txt` (skapa `transcripts/`-mappen om den inte finns).
- Om scriptet failar (rate limit, captions saknas): be användaren klistra in transkriptet manuellt eller ge en fil.

**Om filsökväg:** läs hela filen.

**Om mappsökväg:** läs alla `.md` och `.txt`-filer.

**Om bara ämnesnamn:** fråga användaren vilken källa de vill använda.

### Steg 2, analys och struktur

Läs allt källinnehåll noggrant. Sen:

1. Identifiera kärnämnet och vad kursen ska heta. Kort, konkret titel.
2. Plocka ut **4 till 8 naturliga moduler**. Gruppera relaterade koncept.
3. För varje modul, plocka ut:
   - Principer och regler
   - Tekniker steg för steg
   - Exempel, mallar, formler
   - Vanliga misstag eller "vad du inte ska göra"
   - Checklistor där det är relevant
4. Identifiera nyckelcitat eller exempel värda en callout.
5. Identifiera ramverk eller numrerade system värda numrerade kort.

**Viktigt:** klipp inte ner och sammanfatta inte källan i förväg. Värdet ligger i detaljen. Ta med allt, organiserat tydligt.

### Steg 3, bekräfta strukturen

Innan du bygger, presentera:
- Föreslagen kurstitel
- Modullista med korta beskrivningar (1 mening per modul)
- Antal moduler

Fråga: "Ser strukturen bra ut, eller vill du justera något?" Vänta på bekräftelse.

### Steg 4, bygg HTML-sidan

Skapa `index.html` i projektroten enligt `config.outputDir`.

#### Designsystem

Mappa BRAND.md till CSS-variabler:

```css
:root {
  --cream-50: #FBF8F1;
  --cream-100: #F5EFE2;  /* primär bakgrund */
  --cream-200: #ECE3CE;  /* alternerande sektion */
  --cream-300: #DED0AE;
  --ink-900: #1A1713;    /* primär text + dark sektioner */
  --ink-800: #2A2520;
  --ink-700: #3D362E;
  --ink-500: #6B5F52;    /* dämpad text */
  --ink-400: #8A7D6E;
  --ink-300: #B8AD9E;
  --ink-200: #D9D0BE;    /* borders */
  --amber-700: #A25B2A;  /* eyebrow, länkar */
  --amber-600: #C66A2E;  /* primär CTA */
  --amber-500: #D97742;
  --amber-400: #E69068;
  --amber-100: #F5D7BF;
  --amber-50:  #FBEEE1;
  --plum-700:  #3B1D1A;

  --serif: "Source Serif 4", Georgia, serif;
  --sans:  "Inter", -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
  --mono:  "JetBrains Mono", ui-monospace, "SF Mono", monospace;
}
```

Ladda fonter från Google Fonts:
```
Source Serif 4: weight 400, 500, 600, 700, italic 500, italic 600
Inter: weight 400, 500, 600, 700
JetBrains Mono: weight 400, 500, 600
```

#### Obligatoriska sidkomponenter

**Progress bar.** Fixed top, gradient amber-600 → amber-700, växer med scroll.

**Topbar.** Sticky, cream-100 med blur, vänster: prick + "Birgir Birgisson". Höger: modul-nav (`01 ...`, `02 ...`, `Quiz`). Active state highlights aktuell modul via Intersection Observer.

**Hero.**
- Badge: "Mini-kurs på svenska" eller liknande, **utan datum eller källreferens**
- Rubrik: clamp(40px, 5.6vw, 68px), Source Serif 4 600, ett ord i `<em class="serif-italic">` (signaturen)
- Subhead: 19px, ink-500, max-width 640px, **utan källattribution**
- Meta-rad: t.ex. "7 moduler · ~30 min · FRI"
- Modul-tags: klickbara pills som scrollar till varje modul
- Hero-frame: dekorativ panel med "browser-style" dotrow + en illustrativ accent
- Accent-bar: 120px gradient

**Modul-sektioner.**
- `.module` med border-top och cream-100 bakgrund
- `.module.alt` med cream-200 (alternera mellan moduler)
- Section-head: liten label "MODUL 01" i mono uppercase amber-700
- Stora h2 med signaturen kursiv-amber på ett ord
- Brödtext i container-narrow (760px max-width)

**Komponenter att använda:**

1. **Bullet-listor** med SVG checkmark-cirklar (amber-100 fyllning, amber-700 ✓).
2. **Callouts** med border-left amber-600, ljus amber/plum gradient bakgrund. Mörka callouts (ink-900 bakgrund) för "nyckelinsikter".
3. **Numrerade kort** (`.qcard`) för frågor, framework-steg, listor.
4. **Steg-listor** med mono-numrering "01 / 02" som badge.
5. **Accordions** (`details.acc`) för fördjupningar och long-form moduler.
6. **Jämförelsetabell** (`table.compare`) för export-format, alternativ, etc.
7. **Before/after-paneler** (`.ba`) för bra-vs-dåligt-exempel.
8. **Theme swatches** för färgvarianter om relevant.
9. **Quiz** på slutet av kursen, alltid 3 frågor som testar de viktigaste principerna. Visar score efter alla tre besvarade.

**Animationer.**
- `reveal`-klass + Intersection Observer för fade-up vid scroll.
- Inga gradient-bakgrunder. Inga 3D-effekter.

**Responsivt.**
- Single column under 720px
- Modul-nav döljs under 1000px (eller blir burger om nödvändigt)
- Container-padding 32px desktop, 22px mobil

#### Favicon

Generera `favicon.svg` enligt `config.favicon.spec`. Default-mall:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64" width="64" height="64">
  <rect width="64" height="64" rx="12" fill="#2A2520"/>
  <text x="32" y="46"
        font-family="'Source Serif 4', Georgia, 'Times New Roman', serif"
        font-weight="700" font-size="44"
        fill="#C66A2E" text-anchor="middle">K</text>
</svg>
```

Referera den i `<head>`:

```html
<link rel="icon" type="image/svg+xml" href="favicon.svg" />
```

#### Slutsektionen efter quizet (obligatorisk)

Mörk `.end`-sektion (ink-900 bakgrund) med:

1. Eyebrow "Klar"
2. Rubrik: "Bra jobbat. Du tog dig igenom hela <em class='serif-italic'>mini-kursen</em>."
3. Lead som mjukt knyter till kursens ämne (en mening, ingen hård sälj)
4. **Nyhetsbrevs-kort** (cream-50, amber-knapp, ink-900 text):
   - Eyebrow: "Veckobrev"
   - Rubrik: t.ex. "Vill du tänka mer kring [ämne från kursen], AI och Claude?"
   - Beskrivning som matchar `config.newsletter.cadence` ("Kort, lågmält, på svenska")
   - Bullets enligt `config.newsletter.structure` (med checkmark-cirklar)
   - Knapp: "Prenumerera på veckobrevet" → `config.newsletter.url`
   - Mikrocopy under knappen: `config.newsletter.microcopy`

Ingen sekundär CTA till källvideo. Ingen "Open Claude"-knapp. Bara nyhetsbrevet.

#### Footer

Enkel, en rad: `© <år> Birgir Birgisson`. Cream-200 bakgrund, ink-500 text. Inga länkar till källor.

### Steg 5, output

1. Spara `index.html` i projektroten
2. Spara `favicon.svg` i projektroten
3. Spara råtranskriptet i `transcripts/<slug>.txt` om en sådan hämtades
4. Skapa minimal `vercel.json`:
   ```json
   { "$schema": "https://openapi.vercel.sh/vercel.json", "cleanUrls": true, "trailingSlash": false }
   ```
5. Skapa `.gitignore` med `.DS_Store`, `node_modules/`, `.env`, `.vercel`

Fråga sedan användaren om de vill att du:
- Skapar git-repo och första commit
- Skapar ett GitHub-repo med `gh repo create digitalutveckling101/<slug> --public --source=. --push`
- Eller om de vill ta hand om det själva

Berätta vad du skapade: antal moduler, antal sektioner, om favicon är genererad, om transkript ligger i `transcripts/`.

### Steg 6, OG-bild (obligatorisk, sker efter deploy)

När kursen är pushad och Steg 5 är klart, **stanna och prompta användaren med exakt det här meddelandet** (anpassat till slug och faktisk URL där det är relevant):

> **Kursen är pushad. Nu kör du dessa steg själv:**
>
> 1. Gå till Vercel, importera repot, klicka Deploy
> 2. Koppla din custom domain om du vill (t.ex. `<projektnamn>.birgir.se`)
> 3. Öppna live-URL:en i webbläsaren
> 4. Ta en skärmbild av hero-sektionen (toppen av sidan)
> 5. Öppna Canva, skapa en design **1200×630 px**, använd skärmbilden som utgångspunkt
> 6. Ladda ner från Canva som **JPG** (inte PNG, JPG ger ~10× mindre fil)
> 7. Lägg filen i den här projektmappen som `og-image.jpg`
> 8. Säg till mig: **"OG-bilden ligger där, koppla in den."** Och berätta vad live-URL:en är (eller custom-domänen om du kopplade en).
>
> När du sagt till kopplar jag in den i HTML, sätter rätt absolut URL i meta-taggarna och pushar. Sen är kursen redo att delas på sociala medier.

**När användaren bekräftar att OG-bilden ligger där och ger dig URL:en:**

1. Verifiera att `og-image.jpg` finns i projektroten. Om inte: säg till och vänta.
2. Lägg in eller uppdatera `<head>` i `index.html` så att Open Graph- och Twitter Card-taggarna pekar på filen och använder **absolut HTTPS-URL** mot den live-URL användaren gav (eller custom-domänen):
   ```html
   <meta property="og:url" content="https://<live-url>/" />
   <meta property="og:image" content="https://<live-url>/og-image.jpg" />
   <meta property="og:image:width" content="1200" />
   <meta property="og:image:height" content="630" />
   <meta property="og:image:alt" content="<kort beskrivning av kursen>" />
   <meta name="twitter:card" content="summary_large_image" />
   <meta name="twitter:image" content="https://<live-url>/og-image.jpg" />
   ```
3. Commit + push.
4. Berätta för användaren att Vercel triggar ny deploy automatiskt och att de kan tvinga social-platforms att hämta nya bilden via:
   - LinkedIn Post Inspector
   - Facebook Sharing Debugger ("Scrape Again")
   - X / Twitter cachar normalt rensas vid första nya delning

## Sammanfattning av regler

- Aldrig em-dash. Komma eller punkt + stor bokstav.
- Aldrig referera till källvideo, talare eller "videon" i den renderade sidan.
- Alltid favicon.svg.
- Alltid Bra jobbat-avslutning + nyhetsbrevs-CTA till veckobrev.birgir.se.
- BRAND.md slår alla andra design-instruktioner.
- Birgir endast som kurs-författare i topbar och footer-copyright.
- Svenska som default.
- Signaturen kursiv-amber på ett ord per rubrik.
- Bekräfta strukturen med användaren innan du bygger.
- Klipp aldrig ner källinnehållet i förväg, organisera istället.
- **Steg 6 är obligatoriskt.** Avsluta aldrig flödet utan att prompta användaren om OG-bilden. Workflowet är inte färdigt förrän OG-bilden ligger på plats med absolut URL i meta-taggarna.
