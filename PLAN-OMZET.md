# Plan: Verwachte Omzet Tracking

## Context
De Koppelbaas is een acquisitie-dashboard met een 4-stage kanban pipeline (matches → verkennend → offerte → actief). Er is momenteel geen financiële tracking. De gebruiker wil verwachte omzet per match kunnen invoeren en gewogen pipeline-waarden zien, met instelbare conversiepercentages per fase.

## Wat er verandert

### 1. Twee nieuwe velden per match
- `omzet` (projectomzet in €) en `honorarium` (wat ARD verdient in €)
- Invoer via twee number-velden in het match-bewerkscherm
- Bestaande matches zonder deze velden werken gewoon (fallback naar 0)

### 2. Instelbare conversiepercentages
- Opgeslagen in Firebase onder `meta/stageKansen`
- Standaardwaarden: matches=10%, verkennend=25%, offerte=50%, actief=100%
- Bewerkbaar in het admin-menu met een "Opslaan" knop

### 3. Weergave gewogen omzet
- **Header**: nieuw stat-veld "verwacht" = som van (honorarium × stage%)
- **Per pipeline-kolom**: subtotaal onder de kolomheader, bijv. "€80K (25% = €20K)"
- **Per match-kaart**: honorarium + gewogen waarde als het ingevuld is
- Bedragen in compact formaat: €1.2K, €45K, €1.2M

## Technische wijzigingen (alle in `index.html`)

### A. Helpers (~regel 428)
- `stagePct(stageId)` — retourneert conversie% uit Firebase of default
- `fmtEuro(n)` — formatteert bedrag als €45K / €1.2M

### B. State & Firebase
- `STATE.data.meta` toevoegen (~regel 408)
- `'meta'` toevoegen aan `setupListeners()` paths array (~regel 513)

### C. Match edit modal (~regel 1286)
- Twee number-velden (omzet + honorarium) in grid-layout toevoegen
- Gebruikt bestaande `KB.mf()` helper

### D. Match opslaan (~regel 1982)
- `omzet` en `honorarium` toevoegen aan `saveMatch` update-object
- Defaults (0) toevoegen aan `confirmMatch` fbPush

### E. Admin modal (~regel 1507)
- Nieuwe sectie "Conversiepercentages" met 4 number-inputs
- Nieuwe action `saveStageKansen` → `fbUpdate('meta/stageKansen', ...)`
- `showAdmin` modalData uitbreiden met `pct_*` velden

### F. Header stats (~regel 822)
- Gewogen totaal berekenen: `matches.reduce((sum, m) => sum + honorarium * stagePct / 100, 0)`
- Nieuw stat-blok "verwacht" toevoegen

### G. Pipeline kolomheaders (~regel 1061)
- Per kolom: raw som + gewogen waarde tonen als er omzet is

### H. Match-kaarten (~regel 1122)
- Honorarium + gewogen waarde tonen als honorarium > 0

## Volgorde van implementatie
1. Helper functies (geen dependencies)
2. State/Firebase (meta listener)
3. Edit modal + save logica
4. Admin modal (percentages)
5. Display: header, kolommen, kaarten

## Verificatie
- Open app, bewerk een match → vul omzet en honorarium in → opslaan
- Check header stat "verwacht" toont gewogen totaal
- Check kolomheaders tonen subtotalen
- Ga naar admin → pas percentage aan → opslaan → check dat totalen updaten
- Test met lege/nieuwe matches (moeten 0 tonen, geen errors)
