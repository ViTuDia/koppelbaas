# De Koppelbaas — Project Status

## Wat het is
Acquisitie-dashboard voor architectenbureau ARD/AtelierRuimDenkers. Koppelt ontwikkelaars aan kansen (vastgoedprojecten) tot matches die door een kanban-pipeline bewegen.

## Technische stack
- Single-file vanilla JS app: `index.html` (~2500+ regels)
- Firebase Realtime Database (project: atelierruimdenkers-d47d6)
- GitHub Pages hosting: https://vitudia.github.io/koppelbaas/
- SHA-256 wachtwoord auth, dark mode, CSS custom properties
- Touch drag polyfill voor iOS/iPadOS

## Firebase config
- API key: AIzaSyCuOvghMPD0d_yKnhAhNHXbN72NosDRdKQ
- Database URL: https://atelierruimdenkers-d47d6-default-rtdb.europe-west1.firebasedatabase.app
- Rules: meta, devTypes, kansCats, devs, kansen, matches, users allemaal .read/.write true
- API key restriction: HTTP referrers (https://vitudia.github.io/*), API: Firebase Realtime Database only

## Git
- Repo: vitudia/koppelbaas (private)
- Branch: claude/review-instructions-plan-PRq6N
- GitHub MCP tools geven 403/404 sinds repo private is → push direct naar main

## Voltooide features
- Complete kanban board (4 stages: matches, verkennend, offerte, actief)
- Ontwikkelaars-paneel met type-groepen, badges, highlight/selectie
- Kansen-paneel met categorie-groepen, in/uitklappen
- Match-kaarten met coalities, stage gates, alarmsysteem
- Drag & drop: desktop (HTML5) + touch (iOS polyfill met 200ms hold)
- Drag reorder: matches in pipeline, devTypes, kansCats, devs, kansen tussen categorieën
- Resizable divider tussen panelen en pipeline
- Cat-group visuele hiërarchie (achtergrond containers voor categorieën)
- Dark mode met 30+ kleurvariabelen
- Admin menu: wachtwoord wijzigen, backup/import, uitloggen
- Login flow: auto-detect setup als geen hash, 4s timeout fallback
- Mobiel: tab-navigatie, stage-tabs, move-knoppen, geen selectie-highlight
- 10 UX features: toast, zoekbalk, escape, dubbelklik, swipe tabs, haptic, verbindingsindicator, lege-kolom berichten, dirty-modal warning, tab-tellers
- Print: landscape, geen marges
- seed.html voor testdata

## Bekende aandachtspunten
- Print preview kan nog beter (was eerder leeg, nu gefixed maar niet grondig getest)
- Sortering devs/kansen binnen categorie via drag werkt, maar ▲/▼ knoppen ontbreken (spec: optioneel)
- Firebase API key is publiek (by design), beveiligd via HTTP referrer restriction

## Bestanden
- `/home/user/koppelbaas/index.html` — de volledige app
- `/home/user/koppelbaas/seed.html` — testdata loader
- `/home/user/koppelbaas/database.rules.json` — Firebase rules
- `/home/user/koppelbaas/firebase.json` — hosting config
- `/home/user/koppelbaas/INSTRUCTIES/` — specificatie + React prototype
