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
6. Letter-by-letter reveal na svim H2 naslovima sekcija + word-by-word reveal body teksta (izričit zahtjev klijenta; ručni split s NFC normalizacijom, bez SplitText dependencyja; element-djeca poput linkova ostaju atomarna)
7. Stat traka ispod heroja: count-up (9→0 automobila, 0→15 minuta) + custom SVG animacije (Homo volans padobranac doskoči → tekst "Dom Fausta Vrančića"; kamena kuća se slaže kamen po kamen → tekst "Zaštićena hrvatska baština"), once:true, reduced-motion = statičan tekst
8. JELOVNIK JE STATIČAN (3. runda, 2026-07-03, izričita odluka klijenta): bez ikakvih reveal/chars animacija na tekstu jelovnika, bez pozadinskih ili pratećih slika (pizza crossfade i coverflow traka UKLONJENI). Jedan blok, dvije kolone (Pizze | Iz konobe + peka istaknuta). NE vraćati animacije ni slike u jelovnik.
9. Scroll-scrub video "otkrivanje peke" (14 s, 720×1280 H.264+VP9, pin 220%, currentTime scrub preko blob URL-a radi pouzdanog seekanja; mobile = tihi autoplay loop; poster prvi frame). Original MOV se NE commita.
Scrub budžet: desktop 5 (hero, story, zalazak, riva, video), mobile 3 — unutar FAZA 2D limita; NE dodavati nove scrubove bez gašenja postojećih.
FOTO-OBRADA (3. runda, odluka klijenta): SVE slike u IZVORNIM bojama — bez sepije, bez saturacijskih filtera, bez toplih overlaya/vinjeta; jedina iznimka je hero banner (sepia .05). Band scrimovi (neutralno zatamnjenje za čitljivost teksta) ostaju. NE vraćati filtere.

## Dekorativni potpis (individualiziran)
- SVG line-art divideri: riblja kost/mreža motiv (postojeći, jedinstven za Bare)
- Tanki ivory inset okvir (1px, inset 9px) na story i brod fotografijama — "uokvirena razglednica"
- Zrnati overlay (feTurbulence, 5%) preko cijele stranice — stari papir/kamen
- Ambijentalne full-bleed trake — zalazak nad kanalom (natpis samo "Prvić Šepurine", 3. runda) i zalazak s rive s galebom (citat "Večer na Prviću..."); dva zalaska su svjesna odluka klijenta
- Jelovnik (3. runda): naslov samo "Jelovnik"; jedan statični blok u 2 kolone — LIJEVO "Pizze" (15 naziva), DESNO "Iz konobe": Hladna plata, Carpaccio od tune, Dimljena tuna, Slane srdele, Marinirani inčuni, Kuhana sabljarka/palamida, Pršut i sir, Marin kruhić s ribljom paštetom, Salate + istaknuti blok "Ispod peke" (— Meso / — Hobotnica, na narudžbu dan ranije); bez cijena, bez animacija, bez slika
- Galerija: raspored po ručnoj skici klijenta (4×3: vertikale na lijevom/desnom rubu preko 2 reda, široke na r1 c3-4 i r2 c2-3), kompaktna (1000px, redovi 164px), hover scale 1.08 iznad susjeda (samo hover:hover + pointer:fine)

## Tvrda pravila (Impeccable NE SMIJE pregaziti)
- Mapa: keyless iframe embed + klik-facade (GDPR) — nikad Maps JS API / key= parametar
- Bez API ključeva, tokena, tajni u kodu (S1)
- Fontovi self-host (S3); GSAP/Lenis self-host u assets/js
- Najam broda: cijena se NIGDJE ne prikazuje — CTA "Upit za najam" na telefon
- ANTI-SLOP §1: jela/satnica/godine/kapacitet — samo potvrđeno ili TODO[klijent]; vlasnička imena se ne objavljuju bez potvrde
- Ambijentalne fotke (otok/zalazak) NIKAD u galeriji hrane; samo kao trake/akcenti
- JSON-LD Restaurant schema + og tagovi ostaju
