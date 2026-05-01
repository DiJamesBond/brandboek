# Brandboek Toolkit

Geef Claude een merkreferentie en krijg in één keer een complete brandboek-pagina en een printklare A4-brandboek.

## Wat je krijgt

1. **`brandboek.html`** — een scrollbare referentiepagina over kleuren, typografie, principes, componenten, iconen en woordmerken.
2. **`brandboek-a4.html` + `brandboek-a4.pdf`** — één A4-pagina in staand formaat die de hoofdpunten van het merk op één deelbare poster past.

Beide outputs zijn zelfstandige HTML-bestanden (Google Fonts CDN + inline CSS + inline SVG). Geen build-stap. De PDF wordt gerenderd via headless Edge/Chrome.

**De toolkit heeft geen eigen mening over lettertypen, kleuren of thema's.** Alles komt uit de referentie die je aan Claude geeft — het geëxtraheerde merk is wat je terugkrijgt.

## Snelle start

```bash
git clone <link invullen>
```

Voeg dit daarna in Claude Code toe als werkmap (of plaats de map `.claude/skills/brandboek/` in de `.claude/skills/` van je project). Vraag Claude:

> Maak een brandboek van [site-URL / screenshot / beschrijving]

Claude extraheert wat mogelijk is, vraagt naar ontbrekende informatie, genereert beide bestanden en rendert de PDF.

## Wat is inbegrepen

```
brandboek/
├── .claude/
│   └── skills/
│       └── brandboek/
│           ├── SKILL.md                       # De skill-definitie
│           └── examples/
│               └── template.html              # Structureel skelet met {{TOKENS}} om in te vullen
├── CLAUDE.md
└── README.md
```

## Hoe het werkt

1. Je geeft Claude een referentie (URL, screenshot, bestaande site of merkbeschrijving).
2. Claude extraheert kleuren, lettertypen, tagline, principes en logo-SVG.
3. Claude bevestigt wat er is geëxtraheerd en vraagt alleen naar de ontbrekende onderdelen.
4. Claude schrijft beide HTML-bestanden naar een map `design/`.
5. Claude rendert `brandboek-a4.pdf` via headless Edge/Chrome.
6. Je itereert — restylen, lege ruimte vullen, woordmerk-behandelingen wisselen.

## Voorbeeldprompts

```
Maak een brandboek voor linear.app
```

```
Hier is een screenshot van mijn product. Extraheer het merk en bouw het brandboek + A4-brandboek.
```

```
Mijn merk is "Acme" — primair #ff4444, secundair #666, lettertypen Inter + JetBrains Mono, tagline "Ship faster, break less". Bouw de assets.
```

## De PDF renderen

De skill begeleidt Claude bij het renderen via een headless browser. Handmatig commando (Windows):

```bash
"/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" \
  --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf="brandboek-a4.pdf" \
  "file:///absolute/path/to/brandboek-a4.html"
```

macOS/Linux-equivalenten staan in `SKILL.md`.

## Licentie

MIT
