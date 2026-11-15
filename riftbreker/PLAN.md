# Riftbreker: plan

Een mobiele **Vampire Survivors × Diablo**-hybride in één HTML-bestand (Three.js r128, geen build-stap).
Speel je in een Rift, dan kies je skills zoals in Vampire Survivors. In de stad bouw je je personage op zoals in Diablo: gear, sets, Paragon en Grootse Rifts.

## Kernlus (de dopamine-motor)

```
Stad ──► Rift (2–4 min) ──► Riftbewaker ──► buitregen + sleutel ──► Stad
  ▲          │ kills → XP → level-ups (kies 1 van 3 skills/passieven)        │
  │          │ kills → loot met lichtbundels, schrijnen, elites              │
  └── uitrusten · betoveren · ontmantelen · Paragon-punten ◄────────────────┘
                     ▼
         Grootse Rift N (5:00 timer) → sneller = +1/+2/+3 niveaus → hogere iLvl → betere gear
```

Elke laag geeft een eigen beloning op een eigen tempo:

| Tempo | Beloning |
|---|---|
| Seconden | Schadegetallen, kritieke treffers, explosies, munten |
| ~20 s | Level-up met een keuze uit 3 kaarten |
| ~1 min | Elite-pakket, gegarandeerd een zeldzaam item; schrijn-buff |
| Per rift | Riftbewaker, een regen van legendarische items en een GR-sleutel |
| Per sessie | Paragon-levels, een nieuw GR-niveau, een set compleet, een Oud/Oeroud item |
| Oneindig | Paragon heeft geen plafond; GR-niveaus schalen exponentieel |

## Systemen (allemaal geïmplementeerd)

- **Rifts**: een normale Rift op Kwelling 1–40 levert een GR-sleutel op. Een Grootse Rift heeft een timer van 5:00. Ben je snel klaar, dan stijg je 1–3 niveaus. Elke rift krijgt een willekeurig thema (Helse Diepten, Bevroren Crypte, …).
- **Monsters**: zwak voetvolk, snelle rennende vijanden, zware brutes, elites (×7 HP, gouden ring) en een Riftbewaker (een kogelring, een aanval met waarschuwingscirkel en volgelingen die hij oproept).
- **Skills in de rift** (VS-stijl, max. niveau 5): Magische schicht, Arcane bollen, Kettingbliksem, Vorstnova, Meteoor en Vuuraura. Daarnaast 8 passieven.
- **Gear**: 7 slots en de rangen Normaal, Magisch, Zeldzaam, Legendarisch en Set, met daarbovenop **Oud** (+30%) en **Oeroud** (perfecte rolls, rode bundel).
  - 15 affix-types. Platte waarden schalen met iLvl (`1.14^iLvl`); percentages hebben een vaste range.
  - 13 legendarische krachten die je build veranderen (splitsende schichten, ontploffende lijken, ...).
  - 3 sets (Vuurvogel, Stormheer, Wachter) met bonussen bij 2 en 4 stuks.
  - Vergelijk Schade/Taaiheid in % met wat je gedragen hebt, en een ▲-indicator in je tas.
- **Smid**: ontmantelen levert 💎 op. Betoveren rolt een item opnieuw (kost 💎 en 💰). Auto-ontmantelen kun je per rang instellen.
- **Paragon**: oneindig, met 4 bomen (Kracht, Precisie, Vitaliteit, Snelheid). De XP per kill schaalt met `1.12^L`, de benodigde XP met `1.055^lvl`. Wie hogere niveaus pusht, gaat dus steeds sneller.
- **Schrijnen**: Kracht (2× schade), Razernij (+50% snelheid) en Geleiding (een bliksemstorm).

## Graphics: procedurele gear (v2)

- De held wordt opgebouwd uit losse onderdelen. Elk slot voegt een laag toe die afhangt van de rang en het iLvl:
  - Normaal: alleen kleding.
  - Magisch: gewaad met kap.
  - Zeldzaam: leer met kleine schouderstukken.
  - Legendarisch: plaatpantser met cape, gloeiende randen, punten en hoorns.
  - Set: een eigen kleur per set en een kroon van punten.
  - Oud: extra punten.
  - Oeroud: rood pantser met gloeiende ogen.
- Hoe hoger het iLvl, hoe zwaarder en breder het pantser wordt (`bulk`).
- Wapens: staf, toverstok, zwaard, bijl en scepter, met gloed per rang.
- Een grondaura groeit met het aantal legendarische en oude items.
- Een draaiende 3D-voorvertoning in het uitrustingsscherm.
- Monsters zijn modellen die uit meerdere delen zijn samengevoegd (ghoul, imp, ogre), met een waggelende loopanimatie, schaduw en flits bij een treffer.
- Projectielen gloeien, en er is een kort hit-stop-moment bij elites, legendarische drops en de bewaker.

## v3: kerkers, elites, bosses en diepe gear

- **Lineaire kerker per rift**: ongeveer 22 kamers met gangen die steeds verder het level in lopen, plus zijkamers met een schrijn of een schatgoblin. Er zijn muren, vuurkorven en kristallen, en een minimap die zich vult terwijl je verkent. Groepen monsters wachten tot je dichtbij komt; af en toe valt een horde aan.
- **Elites**:
  - Blauwe Kampioenen: een groep van 3–4.
  - Gele Zeldzame elites: 1 leider met dienaren.
  - Ze hebben een naam, een levensbalk en 1–4 affixen (meer op hogere Kwelling): Snel, Extra Gezond, Afschermend, Gesmolten, Bevroren, Mortier, Teleporteur, Vampirisch, Geëlektrificeerd, Arcaan en Pestilent.
  - Gevaarlijke aanvallen worden vooraf aangekondigd met cirkels op de grond, zodat je ze kunt ontwijken.
- **Schatgoblin**: rent weg en verdwijnt na 14 seconden. Versla je hem, dan krijg je een regen van goud, items en edelstenen.
- **4 bosses**, elk met 3 aanvallen. Onder 50% leven worden ze razend en vallen ze sneller aan.
  - Gromgor: een lijn van inslagen, een stormloop en een stamp.
  - Vexara: een kogelspiraal, jonge spinnen en webben die je vertragen.
  - Ignar: een meteorenregen, ringen van vuurballen en een vuuradem in een kegel.
  - Morvath: teleporteren met een ijsring, draaiende ijsstralen en bevriezende inslagen.
- **Kwelling 1–50**:
  - Monsterniveau `(T−1)·3`.
  - Beloning: XP ×(1+0,25·(T−1)), goud ×(1+0,2·(T−1)) en +10% kans op legendarische items per niveau.
  - Een inschatting laat zien hoe zwaar het wordt (Makkelijk/Uitdagend/Zwaar/Dodelijk), met hoe snel je een monster doodt en hoeveel klappen je overleeft.
  - Een Kwelling voltooien ontgrendelt de volgende.
- **Gear-diepte**:
  - Itemkracht per item en gemiddeld over je uitrusting.
  - Rollbereik `[min–max]` per bonus, kwaliteit in procent en ✦ bij een perfecte roll.
  - Elementschade (Vuur/Kou/Bliksem/Arcaan) en schade per skill, voor buildkeuzes.
  - Schadereductie en leven per kill.
  - **Versterken** van +1 tot +15: tot +5 lukt het altijd; vanaf +8 kun je bij falen een niveau terugvallen.
  - **Sockets en edelstenen**: Robijn, Smaragd, Topaas en Amethist. Drie gelijke combineer je tot één van een hoger niveau. In een wapen geven ze een andere bonus dan in pantser.
  - Een vergelijking per statistiek en een detailscherm met de hele schade-opbouw.

## v4: klassen, heldenniveau en doelen

- **4 eigen klassen**:
  - Strijder (melee, extra leven en pantser).
  - Jager (pijlen, sneller aanvallen en lopen).
  - Magiër (elementen, groter effectgebied).
  - Zieneres (heilig licht dat schade doet en geneest).
- Elke klasse heeft een eigen niveau, skilltree en uitrusting. Tas, goud, edelstenen en Paragon zijn gedeeld.
- **Heldenniveau 1–60**: per niveau 1 skillpunt, +3% schade en +8 leven. Na niveau 60 gaat XP naar Paragon (oneindig).
- **Skilltree**: 6 skills per klasse. Ze komen vrij op niveau 1, 5, 10, 20, 30 en 45, en elke skill kun je tot niveau 10 opwaarderen. Herverdelen kost goud.
  - Elke skill hoort bij een type (Projectiel, Baan, Ketting, Nova, Inslag, Aura of Slag) en heeft een element.
  - Legendarische krachten, sets en bonussen werken per type, zodat ze bij elke klasse passen.
- **Level-ups in de rift** geven tijdelijk +1 op je geleerde skills, of een passieve bonus.
- **Doelen**: 12 categorieën met steeds hogere stappen, zoals niveau, Torment, GR, elites, bosses, legendarische items, goblins, versterken, edelstenen, itemkracht, Paragon en klassen. Elke stap claim je voor goud, scherven, sleutels en edelstenen.

## v5: Druïde, metgezellen en zichtbare skillgroei

- **Druïde**: een wolf die met je meegroeit.
  - Op niveau 1 is het één grijze wolf. Vanaf niveau 4 zijn het er twee, met een donkere vacht en gloeiende ogen. Vanaf niveau 7 worden het zilveren geestwolven met een aura, en op niveau 8 komt er een derde bij, een alfawolf met een ring. Het formaat groeit +9% per niveau.
  - Op niveau 30 komt er een Grizzly bij.
  - De set Wilde Jacht en de legendarische kracht Roedelhart geven extra wolven en schade.
- **Magiër · Energieschild** (niveau 15): een zichtbare bol die 12–50% schade opvangt. Hij wordt groter en kleurt van blauw naar violet. Vanaf niveau 4 draait er een tweede ring; vanaf niveau 5 schokt hij vijanden.
- **Alle skills groeien zichtbaar per niveau**: grotere projectielen en bollen, dikkere bliksem, extra nova-ringen (niveau 5 en 8), grotere meteoren, meer deeltjes, sterkere aura's en slagen met vonken.

## Balans in één formule

- Monster-HP/schade: `MS(L) = 1.16^L`. Gear: `SC(iLvl) = 1.14^iLvl`, waarbij drops iLvl = L+2 (normaal) of L+3 (GR) krijgen.
- Het verschil van ~1,7% per niveau moet je dichten met affixes, legendarische krachten, sets en Paragon. Daardoor is er altijd een volgende upgrade om naar te jagen.

## Mobiele performance

- Instanced meshes voor monsters, projectielen, pickups en deeltjes: ongeveer 12 draw calls.
- Geen schaduwen; pixelratio ≤ 1.75; maximaal 150 monsters; een ruimtelijk grid voor botsingen.
- Schadegetallen en bliksem worden getekend op een 2D-canvas-overlay, niet als DOM-nodes.
- Zwevende joystick (raak het scherm ergens aan). Op desktop werkt WASD.
- De save gaat naar `localStorage`.

## Roadmap (volgende stappen)

1. **Legendarische edelstenen**: socket in een ring of amulet; upgrade ze in de GR in plaats van niveaus omhoog te gaan.
2. **Kanai-kubus**: extraheer een legendarische kracht en draag er 3 tegelijk.
3. **Meer klassen** (Barbaar-melee, Necromancer-minions), elk met eigen skills en sets.
4. **Seizoenen/leaderboard** voor het hoogste GR-niveau (vergt een kleine backend).
5. **PWA** (manifest + service worker), zodat je het spel offline als app op je homescreen kunt zetten.
6. **Audio**: echte samples voor de drop van een legendarisch item (het "ting"-moment) en muziek per thema.
7. **Balans-sim**: een headless script dat 1000 rifts simuleert om de drop-rates te tunen.
