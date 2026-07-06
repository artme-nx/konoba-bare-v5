# DESIGN.md — Konoba Bare, Prvić Šepurine (v5)

register: brand
arhetip: PRESET 1 (rustična konoba) s heritage tonom PRESET 5 — otok bez automobila, obiteljska konoba, "polagano vrijeme"

## Klijent (činjenice)
- Konoba Bare, Prvić Šepurine, otok Prvić (t.u.o. Fortunata)
- Telefon: +385 22 448 094 — jedini rezervacijski/upitni kanal
- Cijelu godinu caffe bar; ljeti konoba i pizzeria (iz priče na stranici)
- RADNO VRIJEME (potvrdio klijent 2026-07-06): caffe bar otvoren cijeli dan, kuhinja 14:00–23:00
- Brod "Bare" za najam — SVI detalji i cijena isključivo NA UPIT (nikad prikazivati cijenu)
- Potvrđena jela (fotke + priča): hladne plate mora, dimljena tuna, pašteta od tune, hobotnica/meso ispod peke (na narudžbu), gradele, pizza. IZBAČENO s jelovnika 2026-07-06 (ne vraćati): carpaccio od tune, marinirani inčuni, "palamida", "Pišk", riječ "smokva" bilo gdje na stranici
- Dolazak na otok: BROD LARA / "brodska linija" — riječ "trajekt" se NE koristi (nalog klijenta 2026-07-06); EN "the Lara boat line", DE "Bootslinie Lara"

## Izvedena paleta (odakle svaki ton)
- `--bg-parchment #F4EEE1` / `--bg-alt #EBE3D2` — vapnenac i suhi kamen Šepurina, topli pergament
- `--terra #B5784A` — terakota krovova i zemlje; akcent za eyebrow/hover
- `--olive #5B6B3A` — masline uz kalete; primarni CTA
- `--sea #3E6374` → sekcija najma broda koristi dublju varijantu `#31505E` (kanal u suton) s pješčanim akcentom `#E9CFA0` (mjesečina/pijesak)
- `--charcoal #2C2C2C` — ugljen s gradela; footer i tekst

## Fontovi
- Display: Cormorant Garamond (varijabilni, self-host) — klasična konoba elegancija, italic za poetske taglinove
  - NAPOMENA (provjereno 2026-07-06): visoko postavljene "lebdeće" kvačice na š/č/ć/ž su DIZAJN ovog pisma
    (konture glifa identične službenim Google datotekama; scaron ymax 730 > cap-height 625) — NIJE slomljeni
    subset, ne "popravljati" i ne trošiti runde na to
- Body: DM Sans (varijabilni, self-host) — tih, čitljiv kontrast serifu
- SELF-HOST OBAVEZNO (GDPR) — nikad runtime fonts.googleapis.com

## Potpisne interakcije (restraint: rustična konoba — suzdržano)
1. NATIVNI SCROLL (4. runda, 2026-07-06, nalog klijenta): Lenis UKLONJEN iz projekta (assets/js/lenis.min.js obrisan).
   Wheel/touchpad, strelice i touch rade izravno; sidreni skokovi preko CSS `html{scroll-behavior:smooth}` +
   `scroll-padding-top:76px` (reduced-motion → auto). NE vraćati Lenis ni bilo koji JS smooth-scroll —
   glatkoća rada ima prednost pred efektom.
2. Hero zoom-out na scroll (scale 1.35→1.0, scrub)
3. Blur+slide t-reveal na tekstu priče; fade+translate reveal sekcija
4. Ken Burns na story slici (scrub)
5. Parallax+zoom na ambijentalnim trakama (zalazak: scale 1→1.08 + ±6 yPercent u JEDNOM tweenu; luka: parallax samo desktop)
6. Word-by-word reveal body teksta (ručni split s NFC normalizacijom, bez SplitText dependencyja; element-djeca poput linkova ostaju atomarna). Letter-by-letter na naslovima UKLONJEN u 3. rundi (odluka klijenta) — ne vraćati.
7. Stat traka ispod heroja: count-up (9→0 automobila, 0→15 minuta) + custom SVG animacije (Homo volans padobranac doskoči → tekst "Dom Fausta Vrančića"; kamena kuća se slaže kamen po kamen → tekst "Zaštićena hrvatska baština"), once:true, reduced-motion = statičan tekst
8. JELOVNIK JE STATIČAN (3. runda, 2026-07-03, izričita odluka klijenta): bez ikakvih reveal/chars animacija na tekstu jelovnika, bez pozadinskih ili pratećih slika (pizza crossfade i coverflow traka UKLONJENI). Jedan blok, dvije kolone (Pizze | Iz konobe + peka istaknuta). NE vraćati animacije ni slike u jelovnik.
9. SCROLL-SCRUB VIDEO SEKCIJA UKLONJENA (5. runda, 2026-07-06, nalog klijenta) — pin/scrub "otkrivanje peke" više NE postoji i NE vraća se; peka-720.mp4 (re-encode za običan loop, crf 25, ~5 MB) sad je jedan od videa u .mom-grid redu. peka-scrub-608.mp4 i peka-720.webm obrisani iz repoa.
10. VIDEO MOMENTI "Četiri kadra" (naslov bez "ognjište" i bez "ispod peke" — nalozi klijenta 2026-07-06): ČETIRI portretna videa nakon galerije, jednako tretirana (bare-vatra, peka-720 "Otkrivanje peke", bare-peka-meso, bare-plata; H.264 720×1280, bez zvuka, preload=none + WebP posteri). NISU scrubovi: tihi loop preko IntersectionObservera (play u kadru, pause izvan); reduced-motion ili bez IO = kontrole bez autoplaya. Desktop 4 kolone, ≤860px 2×2, ≤767px 1 kolona. Izvorni MOV-ovi u ../SLIKE/video, NE commitaju se.
Scrub budžet: desktop 4 (hero, story, zalazak, lara), mobile 3 — unutar FAZA 2D limita; NE dodavati nove scrubove bez gašenja postojećih.
FOTO-OBRADA (3. runda, odluka klijenta): SVE slike u IZVORNIM bojama — bez sepije, bez saturacijskih filtera, bez toplih overlaya/vinjeta; jedina iznimka je hero banner (sepia .05). Band scrimovi (neutralno zatamnjenje za čitljivost teksta) ostaju. NE vraćati filtere.
VIDEO-OBRADA (4. runda): iPhone izvorni MOV-ovi su HLG/BT.2020 — svaki encode MORA kroz "setparams=color_trc=bt2020-10,colorspace=all=bt709" (inače magenta žar); kadrovi s vatrom dodatno "selectivecolor=magentas=0 -0.7 0.4 0:reds=0 -0.2 0.15 0". Ovo NIJE stilski filter nego korekcija HDR→SDR artefakta (izvorne boje se upravo time vraćaju).

## Dekorativni potpis (individualiziran)
- SVG line-art divideri: riblja kost/mreža motiv (postojeći, jedinstven za Bare)
- Tanki ivory inset okvir (1px, inset 9px) na story i brod fotografijama — "uokvirena razglednica"
- Zrnati overlay (feTurbulence, 5%) preko cijele stranice — stari papir/kamen
- Ambijentalne full-bleed trake — zalazak nad kanalom (natpis samo "Prvić Šepurine", 3. runda) i BROD LARA u zalasku (#band-lara, 4. runda: nova fotka IMG_7150, Jadrolinijin brod + svjetionik; citat "Večer na Prviću..." ostaje); dva zalaska su svjesna odluka klijenta
- Jelovnik (4. runda, 2026-07-06, nalog klijenta): naslov samo "Jelovnik"; jedan statični blok u 2 kolone — LIJEVO "Pizze" (14 naziva, "Pišk" IZBAČEN), DESNO gore "Iz konobe": Hladna plata, Dimljena tuna, Slane srdele, Kuhana sabljarka (bez "palamida"), Pršut i sir, Pašteta od tune (preimenovano iz "Marin kruhić s ribljom paštetom"), Salate; DESNO dolje istaknuti blok "Ispod peke" (— Meso / — Hobotnica, na narudžbu dan ranije). IZBAČENI (ne vraćati): Carpaccio od tune, Marinirani inčuni. Bez cijena, bez animacija, bez slika. Mobitel: pizze → ostala jela → peka.
- Galerija: raspored po ručnoj skici klijenta (4×3: vertikale na lijevom/desnom rubu preko 2 reda, široke na r1 c3-4 i r2 c2-3), kompaktna (1000px, redovi 164px), hover scale 1.08 iznad susjeda (samo hover:hover + pointer:fine)

## Tvrda pravila (Impeccable NE SMIJE pregaziti)
- Mapa: keyless iframe embed, VIDLJIVA ODMAH bez klik-facade (nalog klijenta 2026-07-06; loading="lazy" ostaje) — i dalje NIKAD Maps JS API / key= parametar
- Bez API ključeva, tokena, tajni u kodu (S1)
- Fontovi self-host (S3); GSAP/ScrollTrigger self-host u assets/js (Lenis uklonjen — vidjeti Potpisne interakcije #1)
- VIŠEJEZIČNOST (4. runda): /en/ i /de/ su POTPUNE statične kopije s prevedenim sadržajem (nazivi jela ostaju hrvatski s kratkim prijevodom u <small>/zagradi), hreflang hr/en/de/x-default na SVE TRI verzije, jezični switcher HR|EN|DE u headeru i mobilnom izborniku. Svaka izmjena HR sadržaja MORA se preslikati u en/ i de/ (generator: tools/gen_lang.py — puca glasno ako HR string više ne postoji).
- Najam broda: cijena se NIGDJE ne prikazuje — CTA "Upit za najam" na telefon
- ANTI-SLOP §1: jela/satnica/godine/kapacitet — samo potvrđeno ili TODO[klijent]; vlasnička imena (Zrinka Ivić Škara i Ivo Škara) OBJAVLJENA u footer impressumu po nalogu klijenta 2026-07-06 — drugdje ih ne isticati. IMPRESSUM: OIB i točna adresa NEDOSTAJU (TODO[klijent], OIB OBAVEZAN prije predaje klijentu) — ne izmišljati.
- JEZIČNI SWITCHER: linkovi na EKSPLICITNI index.html (en/index.html, ../index.html…) — direktorijski linkovi ("en/") na file:// i nekim serverima daju listing/prazan međukorak; JS ponese #hash na drugu verziju. Header CTA "Rezerviraj stol · 022 448 094" je tel: link (nalog klijenta 2026-07-06).
- Ambijentalne fotke (otok/zalazak) NIKAD u galeriji hrane; samo kao trake/akcenti
- JSON-LD Restaurant schema + og tagovi ostaju
