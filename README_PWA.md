# MS DMT — Mobilna aplikacija (PWA)

**Autor:** Dr Lorand Sakalaš · KCV Novi Sad
**Verzija:** 1.5

---

## Šta je PWA?

**Progressive Web App** je tehnologija koja omogućava da web aplikacija radi kao native mobilna aplikacija — sa ikonom na home screen-u, bez Safari/Chrome trake, sa offline radom — **ali bez App Store-a i Google Play-a**, bez troškova licenci, bez review procesa.

**Kompatibilno sa:**
- ✅ iPhone / iPad (iOS Safari 11.3+)
- ✅ Android (Chrome, Edge, Samsung Internet, Firefox)
- ✅ Windows / Mac (Chrome, Edge — kao desktop aplikacija)

---

## Dva načina korišćenja

### Način A — Standalone HTML (najjednostavniji)

`MS_DMT_app_Sakalas.html` je samostalan fajl koji možeš:
1. Poslati e-mail-om kao prilog
2. Postaviti na USB / shared folder
3. Otvoriti dvostrukim klikom u bilo kom browser-u

**Ograničenje:** kada se otvori sa file:// (lokalno), service worker (offline keš) ne može se aktivirati. Aplikacija i dalje radi savršeno na mobilnom (responsive UI, sve algoritme), ali nije pravo "instalirana" kao PWA.

**Za mobilnu instalaciju:** otvori fajl → koristi "Add to Home Screen" funkciju iz menija browser-a (uputstvo je ugrađeno u app, klikni "Instaliraj" dugme).

### Način B — Hostovana PWA (puna app-funkcionalnost)

Folder `pwa_hosted_package/` sadrži kompletan paket za pravo hostovanje:
- `index.html` — glavna aplikacija
- `manifest.json` — PWA manifest
- `sw.js` — service worker za offline rad
- `icon-*.png` — ikone svih veličina

Kada se ovaj paket hostuje na **HTTPS** serveru, dobijaš punu PWA funkcionalnost:
- Pravi "Install" prompt na Android Chrome-u
- Splash screen pri pokretanju
- Pravi offline rad (keš)
- Auto-update kada se centralizovano izmeni
- Jedan URL koji distribuiraš — svi imaju najnoviju verziju

---

## Kako hostovati (besplatno, 5 minuta)

### Opcija 1: GitHub Pages (preporučeno)

1. Idi na [github.com](https://github.com) i napravi besplatan nalog
2. Klikni **"New repository"**, daj ime npr. `ms-dmt-app`, izaberi **Public**
3. Klikni **"uploading an existing file"** i prebaci sva 4 fajla iz `pwa_hosted_package/`
4. Klikni **Settings → Pages** u tom repo-u
5. Pod "Source" izaberi **main branch** i klikni **Save**
6. Posle 1-2 minuta dobiješ URL: `https://[username].github.io/ms-dmt-app/`
7. Otvori taj URL na telefonu → "Add to Home Screen" / "Install app"

**Prednosti:** besplatno zauvek, automatski HTTPS, neograničeno deljenje URL-a kolegama.

### Opcija 2: Netlify Drop (najbrža)

1. Idi na [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag-and-drop ceo `pwa_hosted_package/` folder na stranicu
3. Posle 30 sekundi dobiješ URL: `https://nešto-random.netlify.app`
4. (Opciono: registruj besplatan nalog za stalan URL i custom domen)

**Prednosti:** najbrži start, ne treba nalog za prvi deploy.

### Opcija 3: Vercel

1. Idi na [vercel.com](https://vercel.com), registracija kroz GitHub
2. **Import** repository → izaberi `pwa_hosted_package` folder
3. Auto-deploy → URL spreman za 1 minut

### Opcija 4: Lokalna mreža klinike

Ako klinika ima intranet server, kopiraj `pwa_hosted_package/` u web folder. Pristup preko intra-IP adrese (npr. `https://192.168.x.x/ms-dmt/`). Bitno: mora biti **HTTPS** za pun PWA rad.

---

## Kako instalirati na telefonu (za korisnike)

### iPhone / iPad

1. Otvori URL u **Safari**-ju (NE Chrome — iOS Chrome ne podržava PWA install na isti način)
2. Tap na **Share** dugme (ikona kvadrata sa strelicom navlie) u dnu ekrana
3. Skroluj i izaberi **"Add to Home Screen"** / **"Dodaj na početni ekran"**
4. Tap **"Add"** u gornjem desnom uglu
5. ✅ Ikona MS DMT pojavila se na home screen-u — otvara se kao prava aplikacija

### Android

1. Otvori URL u **Chrome**-u
2. Tap na **menu (⋮)** u gornjem desnom uglu
3. Izaberi **"Install app"** ili **"Add to Home screen"**
4. Potvrdi
5. ✅ Aplikacija se pojavljuje u app drawer-u i na home screen-u

(Aplikacija je dovoljno pametna da automatski prikaže instalacioni banner kada otkrije da si na mobilnom uređaju.)

### Desktop (Chrome / Edge)

1. Otvori URL u Chrome ili Edge
2. U adresnoj traci, klikni ikonu **install** (kompjuter sa strelicom dole, desno od adrese)
3. Klikni **"Install"** u dijalog
4. ✅ Aplikacija je pokrenuta u svom prozoru, sa ikonom u Start Menu / Dock

---

## Brzi test posle hostovanja

Otvori URL na telefonu. Ako vidiš:
- Banner pri vrhu ekrana koji nudi instalaciju → ✅ PWA radi
- "Add to Home Screen" iz menija pravilno radi → ✅ Manifest je ispravan
- Pošto instaliraš i isključiš WiFi/data → app se i dalje otvara → ✅ Service worker keš radi

---

## Sigurnost i privatnost

- **Nema kolačića, analitike, ili tracking-a.** Sve radi lokalno na uređaju.
- **Nema slanja podataka** — niti jedna informacija o pacijentu ne ide na server.
- **Sav keš je lokalni** — službba health record je netaknuta.
- **HIPAA/GDPR**: pošto ne unosiš identifikaciju pacijenta, alat je sigurnosno neutralan.

---

## Update-ovanje

**Standalone HTML:** kada izađe nova verzija, šalješ novi fajl. Stari je nepotreban.

**Hostovana PWA:** zameniš fajlove na hosting-u → svi korisnici dobijaju update automatski pri sledećem otvaranju (service worker povlači nove fajlove). Nema potrebe da reinstaliraju.

---

## Troubleshooting

**"Add to Home Screen" se ne pojavljuje na iPhone-u:**
→ Mora biti Safari (ne Chrome ili Firefox). iOS Chrome ne podržava direktnu PWA instalaciju.

**Android Chrome ne nudi "Install":**
→ Proveri da li je sajt na HTTPS. PWA install zahteva HTTPS (ili localhost). file:// neće raditi za pun install.

**Aplikacija se otvara, ali nakon zatvaranja podaci nestaju:**
→ Ovo je očekivano ponašanje — aplikacija ne čuva stanje pacijenata između sesija (bezbednosna feature). Svaki pacijent počinje sveže.

**Ikona izgleda obrisana ili "izrezana" na Android-u:**
→ `icon-maskable-512.png` ima safe-zone podršku za "adaptive icons". Manifest već ima `purpose: "maskable"` postavljeno.

---

## Bonus: ako želiš pravi App Store / Play Store

PWA može se pretvoriti u native aplikaciju za App Store / Play Store korišćenjem:
- **PWABuilder** (Microsoft, besplatan): [pwabuilder.com](https://www.pwabuilder.com/) — ubaci URL hostovane PWA, dobiješ Android APK i iOS Xcode projekat
- **Capacitor** (Ionic): wrapper koji konvertuje PWA u native app
- **Bubblewrap**: Android-only, generiše TWA (Trusted Web Activity)

Trošak: Apple Developer Program $99/god + Google Play one-time $25. Vredi razmotriti samo ako se planira distribucija preko zvaničnih radnji.

---

## Sažetak: koji format izabrati?

| Tvoj scenario | Preporuka |
|---|---|
| Lično koristim na svom telefonu | Otvori HTML → Add to Home Screen |
| Slanje 5-10 kolega na odeljenju | Standalone HTML preko e-mail-a |
| Distribucija celoj klinici | Hostuj na GitHub Pages → deli URL |
| Konferencija / kongres / edukacija | Hostuj + napravi QR kod od URL-a |
| App Store / Play Store | PWABuilder (samo ako je vredno truda) |

---

## Šta sve sadrži paket

```
pwa_hosted_package/
├── index.html              ← Glavna aplikacija (PWA-enabled)
├── manifest.json           ← PWA manifest
├── sw.js                   ← Service worker za offline rad
├── icon-192.png            ← Android ikona 192px
├── icon-512.png            ← Android ikona 512px
├── icon-maskable-512.png   ← Adaptive icon za Android
└── apple-touch-icon.png    ← iOS ikona 180px
```

Ukupno ~155 KB (jako malo, brz download i instalacija).
