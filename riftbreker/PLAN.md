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
