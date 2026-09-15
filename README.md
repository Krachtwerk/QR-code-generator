# Krachtwerk QR-code generator

Eén bestand, geen installatie. Dubbelklik `index.html` en je kunt aan de slag.

## Voor collega's

1. Kies wat er in de code moet: een link, contactgegevens, wifi, e-mail, telefoon of SMS.
2. Kies een kleur uit de huisstijl, of een eigen kleur.
3. Download als PNG, download als SVG, of kopieer de code en plak hem in PowerPoint, Word of Teams.

**PNG** voor presentaties, social en e-mail. **SVG** voor drukwerk: die blijft scherp op elk formaat, van visitekaartje tot beursbanner.

De code krijgt altijd de hoogste foutcorrectie, dus hij blijft leesbaar met het beeldmerk erin, na drukken en na vouwen.

**Let op de oranje waarschuwing.** Die verschijnt als de twee kleuren te weinig verschillen. De code wordt dan nog steeds gemaakt, maar scant mogelijk niet. Test hem in dat geval even met je telefoon voordat je hem naar de drukker stuurt.

## Delen met het team

Twee manieren:

- Stuur `index.html` als bijlage. Werkt offline, ook zonder netwerk op een beursstand.
- Zet het bestand op GitHub Pages: repo-instellingen → Pages → bron `main` / root. De generator staat dan op `https://<org>.github.io/QR-code-generator/`.

## Technisch

- `index.html` is het hele project. Geen build, geen server, geen externe verzoeken.
- QR-encoding door [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) van Kazuhiko Arase (MIT), inline meegeleverd. De library levert alleen de modulematrix; de stippen, afgeronde zoekpatronen en de logo-uitsparing worden hier zelf naar SVG geschreven.
- De SVG is de bron. PNG en klembord worden daaruit afgeleid via een canvas.
- Huisstijlkleuren staan als CSS-variabelen bovenaan het bestand, gemeten uit de officiële logobestanden. Het beeldmerk is uit datzelfde bestand getraceerd naar één SVG-pad, dus het schaalt mee en neemt de gekozen kleur over.
- Typografie is de systeemfontstack. Tide Sans en Open Sans staan wel op krachtwerk.nl, maar die server stuurt geen CORS-header, dus een browser weigert ze van buitenaf te laden. Wil je ze toch: zet de `.woff2`-bestanden naast `index.html` en voeg een `@font-face` met een relatief pad toe.

### Zelfcheck

Open `index.html?selftest` en kijk in de console. Dertien asserts op de escaping van vCard en wifi, de contrastberekening en de opbouw van de SVG.

Wat de zelfcheck niet dekt is het scannen zelf. Dat is één keer handmatig geverifieerd door de gerenderde afbeelding pixel voor pixel terug te lezen en elk modulecentrum te vergelijken met de matrix van de library: nul afwijkingen op codes van 29, 37 en 69 modules.
