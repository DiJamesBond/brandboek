---
name: brandboek
description: Extraheer het design van een merk uit elke referentie die de gebruiker geeft (site-URL, screenshot of bestaande assets) en genereer een volledige brandboek-referentiepagina EN een A4-brandboek-PDF van 1 pagina. Triggers op "brandboek", "brand book", "make design assets", "extract design", "brand page", "one page brand".
---

# Brandboek Generator

Geef Claude een referentie (site-URL, screenshot of bestaande merk-assets) en het produceert twee artefacten die bij dat merk passen:

1. **`brandboek.html`** — een scrollbare referentiepagina over kleuren, typografie, principes, componenten, iconen en woordmerken.
2. **`brandboek-a4.html` → `brandboek-a4.pdf`** — één A4-pagina in staand formaat die de hoofdpunten van het merk op één deelbare poster past, printklaar.

Beide outputs zijn zelfstandige HTML-bestanden (Google Fonts CDN + inline CSS + inline SVG, geen build-stap). De PDF wordt gerenderd via headless Edge/Chrome.

**Deze skill heeft geen eigen mening over lettertypen, kleuren of thema's.** Alles komt uit de referentie van de gebruiker. Als er geen referentie is, vraag erom — verzin niets.

---

## Workflow

1. **Verkrijg de referentie.** URL, screenshot, bestaande site of een beschrijving van het merk. Als de gebruiker er geen heeft gegeven, vraag ernaar.
2. **Extraheer.** Als URL → WebFetch deze en inspecteer de HTML/CSS. Als screenshot → Lees deze en identificeer kleuren/lettertypen visueel. Haal eruit:
   - Hex-kleuren (primair, secundair, eventuele accenten die het merk daadwerkelijk gebruikt)
   - Lettertypen (controleer `<link>`-tags, CSS `font-family`-declaraties of visuele identificatie)
   - Tagline / value prop
   - Eventuele gedocumenteerde principes
   - Logo / beeldmerk (haal op als inline SVG als de referentie er een heeft)
   - Themarichting (licht / donker / welke oppervlakken worden gebruikt)
3. **Bevestig de extractie.** Kort bericht met wat je hebt gevonden. Vraag alleen naar de ontbrekende onderdelen.
4. **Genereer beide bestanden** in een map `design/`.
5. **Render de PDF** via headless Edge/Chrome.
6. **Toon het aan de gebruiker.** Stuur de PDF en de `brandboek.html`.
7. **Itereer** op basis van feedback.

---

## Waarnaar te vragen (alleen als het niet extraheerbaar is)

- **Merknaam/-namen** — enkel merk of een familie (moedermerk + product + studio)
- **Tagline** — één korte regel
- **Principes** — 4–6 designregels (als het merk er geen publiceert, sla de sectie over in plaats van iets te verzinnen)
- **Team- of productset** (optioneel) — 4–8 items met elk een kleur + rol
- **Kleuren/lettertypen die je niet in de referentie kunt zien**

**Verzin nooit merkdetails.** Kleuren, lettertypen, namen, taglines, logo's en principes komen allemaal van de gebruiker of uit de referentie. Als een sectie geen bronmateriaal heeft, verwijder die dan — vul hem niet met verzonnen content.

**Teken logo's nooit opnieuw.** Gebruik exact de SVG uit de referentie. Als het logo een rasterafbeelding is, vraag de gebruiker om de SVG of laat een placeholder staan en vermeld dit.

---

## Output 1: `brandboek.html` (scrollbare referentiepagina)

Secties in deze volgorde (verwijder alles waarvoor geen bronmateriaal is):

1. **Header** — brand lockup + tagline + versie/datum
2. **Kleurhiërarchie** — de primaire, secundaire en tertiaire kleuren die het merk daadwerkelijk gebruikt, met hex + gebruiksnotitie
3. **Uitgebreid palet** — eventuele aanvullende merkkleuren (status, categorie, enz.)
4. **Typografie** — display-, body- en monosamples met de daadwerkelijke lettertypen van het merk; gewichtsladder
5. **Principes** — wat het merk documenteert
6. **Componenten** — knoppen, kaarten en badges gestyled in de taal van het merk
7. **Iconen** — de iconentaal die het merk gebruikt (inline SVG, pixelart, lijn, gevuld, enz.)
8. **Woordmerken / Lockups** — 2–3 naambehandelingen op basis van het beeldmerk + woordmerk van het merk
9. **Footer** — bestandspad, versie, eventuele kruisverwijzing

Match de oppervlakken (licht vs donker), radius, randbehandeling en typehiërarchie van het merk. Als het merk rond en zacht is, gebruik dan niet standaard scherp. Als het merk maximalistisch is, maak het dan niet minimalistisch.

---

## Output 2: `brandboek-a4.html` → `brandboek-a4.pdf`

Een **enkele A4-pagina in staand formaat** (210mm × 297mm) — de hoofdpunten van het merk in één oogopslag.

### Vereiste print-CSS

```css
@page { size: A4; margin: 0; }
* { -webkit-print-color-adjust: exact; print-color-adjust: exact; }
.page { width: 210mm; height: 297mm; padding: 18mm 16mm 14mm; }
```

### Layout (van boven naar beneden, één A4-pagina)

1. **Kop** — brand lockup links, versie + tagline rechts
2. **Kleurhiërarchie** — 3 swatch-tegels (Primair / Secundair / Tertiair) met de daadwerkelijke kleuren van het merk
3. **Midden in twee kolommen** — typografiekaart (lettertypen van het merk) + principeskaart (gedocumenteerde principes van het merk)
4. **Team-/productrij** (optioneel) — 6 kleine tegels als het merk een productfamilie heeft
5. **Woordmerken · Lockups** — 3 tegels met naambehandelingen
6. **Footer** — bestandspad + kruisverwijzing

### Renderen naar PDF

Windows (Edge):
```bash
"/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" \
  --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf="brandboek-a4.pdf" \
  "file:///absolute/path/to/brandboek-a4.html"
```

macOS (Chrome):
```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf="brandboek-a4.pdf" \
  "file://$PWD/brandboek-a4.html"
```

Linux (Chromium):
```bash
chromium --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf="brandboek-a4.pdf" \
  "file://$PWD/brandboek-a4.html"
```

---

## Woordmerkpatronen

Drie lockup-stijlen die je kunt aanbieden (pas aan op de toon van het merk):

1. **Beeldmerk + woordmerk** — icoon naast de merknaam. Gewicht/tracking matcht het eigen woordmerk van het merk.
2. **Split-weight lockup** — `DEEL1DEEL2` waarbij de ene helft zwaar is en de andere licht. Handig voor paraplu-/groepsnamen.
3. **Accent-lockup** — `DEEL1DEEL2` waarbij de ene helft gekleurd is met de primaire merkkleur. Handig voor studio's/submerken.

Neem alleen lockupvarianten op die logisch zijn voor het merk. Een merk met één woord heeft geen split-weight-behandeling nodig.

---

## Skeleton Template

Zie `examples/template.html` voor een structureel skelet met placeholder-tokens (`{{PRIMARY}}`, `{{DISPLAY_FONT}}`, enz.). Kopieer het, vervang de tokens door waarden uit je extractie en voeg secties toe of verwijder ze zodat het bij het merk past.

---

## Regels

- **Extraheer voordat je vraagt.** Gebruik de referentie om alles vooraf in te vullen wat je kunt.
- **Verzin nooit merkdetails.** Geen verzonnen kleuren, lettertypen, namen, taglines, principes of logo's.
- **Teken logo's nooit opnieuw.** Gebruik exact de SVG die de gebruiker aanlevert.
- **Verwijder secties die je niet kunt onderbouwen.** Het is beter om een sectie over te slaan dan deze met fictie te vullen.
- **Match het merk, leg geen stijl op.** De template is structureel — de visuele taal komt uit de referentie.
- **Pas op één A4-pagina.** Als content overloopt, verklein padding of verwijder een sectie.
- **Zelfstandige HTML.** Google Fonts CDN + inline CSS + inline SVG. Geen build-stap.
- **Render de PDF.** Lever niet alleen HTML op — rond het werk af met headless Edge/Chrome.
