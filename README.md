# Evidencija pića — PWA

Standalone web aplikacija za evidenciju prodanih pića.
Može se instalirati na mobitel (Android i iOS) kao prava aplikacija.

## Struktura datoteka

```
/
├── index.html          ← glavna aplikacija
├── manifest.json       ← PWA manifest (naziv, ikone, boje)
├── sw.js               ← service worker (offline podrška)
├── icons/
│   ├── icon-72x72.png
│   ├── icon-96x96.png
│   ├── icon-128x128.png
│   ├── icon-144x144.png
│   ├── icon-152x152.png
│   ├── icon-192x192.png
│   ├── icon-384x384.png
│   ├── icon-512x512.png
│   ├── apple-touch-icon.png
│   ├── favicon-16x16.png
│   └── favicon-32x32.png
└── README.md
```

## Kako objaviti (hosting)

### Opcija A — Netlify (preporučeno, besplatno)
1. Idite na https://netlify.com i kreirajte besplatni račun
2. Kliknite "Add new site" → "Deploy manually"
3. Prevucite (drag & drop) cijelu mapu na stranicu
4. Gotovo! Dobivate URL poput: https://vas-naziv.netlify.app

### Opcija B — GitHub Pages (besplatno)
1. Kreirajte GitHub račun i novi repozitorij
2. Uploadajte sve datoteke
3. Settings → Pages → Source: main branch
4. URL: https://korisnik.github.io/naziv-repozitorija

### Opcija C — Vlastiti webserver
- Kopirajte sve datoteke u public_html / htdocs / www mapu
- Mora biti HTTPS (obavezno za PWA i service worker)

## Instalacija na mobitel

### Android (Chrome)
- Otvorite stranicu u Chromeu
- Pojavit će se banner "Instaliraj aplikaciju" — kliknite
- Ili: izbornik (⋮) → "Dodaj na početni zaslon"

### iOS (Safari)
- Otvorite stranicu u Safariju
- Kliknite gumb "Dijeli" (kvadrat sa strelicom gore)
- "Dodaj na početni zaslon"

## Napomene

- Podaci se čuvaju lokalno na uređaju (localStorage)
- Aplikacija radi offline nakon prvog učitavanja
- Za sinkronizaciju između uređaja potreban je zajednički backend
