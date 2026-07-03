# DESIGN.md — Konoba Bare, Prvić Šepurine (v5)

register: brand
arhetip: PRESET 1 (rustična konoba) s heritage tonom PRESET 5 — otok bez automobila, obiteljska konoba, "polagano vrijeme"

## Klijent (činjenice)
- Konoba Bare, Prvić Šepurine, otok Prvić (t.u.o. Fortunata)
- Telefon: +385 22 448 094 — jedini rezervacijski/upitni kanal
- Cijelu godinu caffe bar; ljeti konoba i pizzeria (iz priče na stranici)
- Brod "Bare" za najam — SVI detalji i cijena isključivo NA UPIT (nikad prikazivati cijenu)
- Potvrđena jela (fotke + priča): hladne plate mora, carpaccio od tune, hobotnica/meso ispod peke (na narudžbu), gradele, pizza

## Izvedena paleta (odakle svaki ton)
- `--bg-parchment #F4EEE1` / `--bg-alt #EBE3D2` — vapnenac i suhi kamen Šepurina, topli pergament
- `--terra #B5784A` — terakota krovova i zemlje; akcent za eyebrow/hover
- `--olive #5B6B3A` — masline uz kalete; primarni CTA
- `--sea #3E6374` → sekcija najma broda koristi dublju varijantu `#31505E` (kanal u suton) s pješčanim akcentom `#E9CFA0` (mjesečina/pijesak)
- `--charcoal #2C2C2C` — ugljen s gradela; footer i tekst

## Fontovi
- Display: Cormorant Garamond (varijabilni, self-host) — klasična konoba elegancija, italic za poetske taglinove
- Body: DM Sans (varijabilni, self-host) — tih, čitljiv kontrast serifu
- SELF-HOST OBAVEZNO (GDPR) — nikad runtime fonts.googleapis.com

## Potpisne interakcije (restraint: rustična konoba — suzdržano)
1. Lenis smooth scroll (lerp 0.2 — NE dirati, štekavost riješena ranije; uz to NIKAD ne vraćati
   CSS `html{scroll-behavior:smooth}` — tuče se s Lenisom pa touchpad/wheel šteka ili stane)
2. Hero zoom-out na scroll (scale 1.35→1.0, scrub)
3. Blur+slide t-reveal na tekstu priče; fade+translate reveal sekcija
4. Ken Burns na story slici (scrub)
5. Parallax+zoom na ambijentalnim trakama (zalazak: scale 1→1.08 + ±6 yPercent u JEDNOM tweenu; luka: parallax samo desktop)
6. Letter-by-letter reveal na svim H2 naslovima sekcija (izričit zahtjev klijenta, 2026-07-03; ručni char-split s NFC normalizacijom, bez SplitText dependencyja)
7. Stat traka ispod heroja: count-up (9→0 automobila, 0→15 minuta) + stagger reveal, once:true
8. 2× horizontalna traka slika u jelovniku (pin + x scrub) — IZRIČIT zahtjev klijenta (2026-07-03), svjesno odstupanje od restraint pravila za rustičnu konobu; na <768px efekt ugašen → običan swipe red (slike max-width:80vw da sljedeća proviruje)
Scrub budžet: desktop 6, mobile 3 (gornji FAZA 2D limit) — iznimka odobrena zahtjevom klijenta za horizontalne trake; NE dodavati nove scrubove bez gašenja postojećih.

## Dekorativni potpis (individualiziran)
- SVG line-art divideri: riblja kost/mreža motiv (postojeći, jedinstven za Bare)
- Tanki ivory inset okvir (1px, inset 9px) na story i brod fotografijama — "uokvirena razglednica"
- Zrnati overlay (feTurbulence, 5%) preko cijele stranice — stari papir/kamen
- Ambijentalne full-bleed trake sa citatom u dnu — zalazak i luka u predvečerje (vlastite fotke; namjerno NE dva zalaska, band-mjesec zamijenjen lukom 2026-07-03)
- Jelovnik (2026-07-03, sa stvarnog jelovnika): DIO 1 "Pizze" samo nazivi (15 pizza, bez cijena i sastojaka), DIO 2 "Iz konobe" samo nazivi (bez cijena); Manifest sekcija uklonjena, zamijenjena stat trakom (0 automobila / 15 min od Vodica / Zaštićena hrvatska baština / Dom Fausta Vrančića)

## Tvrda pravila (Impeccable NE SMIJE pregaziti)
- Mapa: keyless iframe embed + klik-facade (GDPR) — nikad Maps JS API / key= parametar
- Bez API ključeva, tokena, tajni u kodu (S1)
- Fontovi self-host (S3); GSAP/Lenis self-host u assets/js
- Najam broda: cijena se NIGDJE ne prikazuje — CTA "Upit za najam" na telefon
- ANTI-SLOP §1: jela/satnica/godine/kapacitet — samo potvrđeno ili TODO[klijent]; vlasnička imena se ne objavljuju bez potvrde
- Ambijentalne fotke (otok/zalazak) NIKAD u galeriji hrane; samo kao trake/akcenti
- JSON-LD Restaurant schema + og tagovi ostaju
