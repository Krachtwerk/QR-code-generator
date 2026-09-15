# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Krachtwerk QR-code generator: één bestand, `index.html`. Geen build, geen server, geen externe verzoeken, geen dependencies. UI-tekst, variabelen en functienamen zijn Nederlands, houd dat zo.

## Draaien en testen

- Draaien: open `index.html` in een browser (dubbelklik, of GitHub Pages vanaf `main` / root).
- Zelfcheck: open `index.html?selftest` en lees de console. Dertien asserts over vCard/wifi-escaping, contrast en SVG-opbouw. Er is geen testrunner, geen npm; nieuwe logica krijgt een `check(...)`-regel in het zelftest-blok onderaan [index.html](index.html#L712).
- Scannen wordt niet getest. Wijzig je de modulegeometrie in `renderSVG`, verifieer dan handmatig met een telefoon.

## Architectuur

Drie blokken in `index.html`, in volgorde: `<style>` met huisstijl-CSS-variabelen, de inline qrcode-generator-library (geminificeerd, MIT, niet aanraken), en het eigen script vanaf regel 398.

Dataflow: `payloads[staat.type]()` levert de tekst → `renderSVG()` maakt de SVG → `teken()` zet hem in de DOM. `staat` (type, voorgrond `vg`, achtergrond `ag`, logo, size) is de enige globale toestand; elke input roept `straks()` aan, dat `teken()` 120 ms debounced.

- De library levert alleen de modulematrix via `qr.isDark()`. Stippen, afgeronde zoekpatronen en de logo-uitsparing worden in `renderSVG()` zelf naar SVG-strings geschreven. Foutcorrectie staat vast op `"H"`.
- De SVG is de bron van waarheid (`huidigeSVG`). PNG en klembord worden daaruit afgeleid via een canvas in `naarPNG()`.
- Het beeldmerk is één `<path d>` in de DOM (`#beeldmerk`), uitgelezen naar `BEELDMERK` en herschaald in de SVG; het neemt de gekozen voorgrondkleur over.
- Contrast onder 3:1 toont een waarschuwing maar blokkeert het genereren niet. Te lange invoer gooit in de library en wordt in `teken()` gevangen als foutmelding.
- Nieuw QR-type toevoegen: payload-functie in `payloads`, lege-tekstmelding in `leegTekst`, veldenblok in de HTML, knop met `data-type`.
- Escaping loopt via `vEsc` (vCard, RFC 6350) en `wEsc` (wifi, ZXing-conventie). Wijzig die niet zonder de bijbehorende asserts aan te passen.

Typografie is bewust de systeemfontstack: krachtwerk.nl stuurt geen CORS-header voor Tide Sans en Open Sans.
