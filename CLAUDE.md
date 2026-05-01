# Brandboek Toolkit

Geef Claude een merk- of sitereferentie → krijg een volledige brandboek-referentiepagina EN een A4-brandboek-PDF van 1 pagina terug.

## Wat deze toolkit doet

1. Extraheert kleuren, lettertypen en principes uit elke referentie die je geeft (URL, screenshot, bestaande site of beschrijving)
2. Genereert `design/brandboek.html` — een scrollbare referentiepagina
3. Genereert `design/brandboek-a4.html` — één A4-pagina in staand formaat
4. Rendert `design/brandboek-a4.pdf` via headless Edge/Chrome

De skill staat op `.claude/skills/brandboek/SKILL.md`. Lees die eerst en volg daarna de workflow daar.

## Regels

- **Extraheer eerst uit de referentie.** Vraag de gebruiker niet om iets dat je zelf kunt zien.
- **Verzin nooit merkdetails.** Kleuren, lettertypen, namen en taglines komen van de gebruiker of uit de referentie.
- **Teken logo's nooit opnieuw.** Gebruik exact de SVG die de gebruiker aanlevert.
- **De template buigt mee met het merk.** Donker is de standaard, maar match de referentie als die afwijkt.
- **Pas op één A4-pagina.** Als content overloopt, maak het compacter of verwijder een sectie.
- **Render de PDF.** Lever geen HTML op zonder de gerenderde output.
