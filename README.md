# De Koppelbaas

Acquisitie-dashboard waarmee ontwikkelaars en kansen gekoppeld worden tot matches die door een pipeline bewegen.

## Features

- Kanban-pipeline met 4 stages (potentieel, verkennend, offerte, lopend werk)
- Drag & drop (desktop + touch/mobiel)
- Realtime sync via Firebase
- Dark mode
- Zoeken en filteren
- Stage gates bij fase-wisseling
- Alarmsysteem bij onvoldoende koppelingen
- Coalities (groepen ontwikkelaars)
- Database backup & import
- Print-optimalisatie (landscape)

## Setup

### 1. Firebase project

1. Maak een project aan in de [Firebase Console](https://console.firebase.google.com/)
2. Activeer **Realtime Database** (regio: `europe-west1`)
3. Kopieer de Firebase config en vul deze in `index.html` bij `firebaseConfig`

### 2. Database rules

Ga naar **Realtime Database > Rules** en plak:

```json
{
  "rules": {
    "meta": { ".read": true, ".write": true },
    "devTypes": { ".read": true, ".write": true },
    "kansCats": { ".read": true, ".write": true },
    "devs": { ".read": true, ".write": true },
    "kansen": { ".read": true, ".write": true },
    "matches": { ".read": true, ".write": true },
    "users": { ".read": true, ".write": true }
  }
}
```

### 3. Wachtwoord

Bij de eerste keer openen detecteert de app dat er geen wachtwoord is en toont een setup-scherm. Het wachtwoord wordt als SHA-256 hash opgeslagen in Firebase. Wijzigen kan via het Admin-menu (tandwiel in de header).

### 4. Hosting

Push naar GitHub en activeer **GitHub Pages** (Settings > Pages > branch `main`, root `/`).

### 5. API key beveiligen (optioneel)

In Google Cloud Console > APIs & Services > Credentials:
- **Application restrictions**: Websites, voeg je domein toe
- **API restrictions**: alleen Firebase Realtime Database

## Technologie

- Vanilla JS (geen frameworks, geen build tools)
- Firebase Realtime Database via CDN
- Enkele `index.html` met inline CSS en JS
