# Aksjeradar — designvarianter

Arbeidstittel for prosjektet. Du valgte **Terminal** (svart + neon-grønt, monospace, scrollende ticker, trading-station-stil).

De to andre variantene som ble vurdert ligger her, slik at vi kan bytte stil senere uten å gjette fra bunnen av. Hver variant er bare en CSS-fil — for å bytte, kopier den til `styles.css` (overskriver terminal-versjonen) og oppdater `<link>`-taggen i `index.html`.

---

## 1. Editorial (ikke valgt)

- **Følelse:** rolig, papir-aktig, som en finansavis på søndag morgen
- **Bakgrunn:** varm krem (`#f6f1e8`)
- **Aksent:** dyp skoggrønn (`#143a2b`)
- **Type:** Fraunces (display-sans med serif-følelse) for overskrifter, system-sans for brødtekst
- **Layout:** stor hero, 3-kol news-grid med featured først, bredt utstrakt
- **Passer når:** du vil at siden skal føles som etablert og troverdig, ikke som en handelsplattform
- **Filer:** `styles-editorial.css`

**Når det er riktig bytte:** hvis du vil at nyhetsbrevet skal føles "noe du leser på kvelden med kaffe," eller hvis du etter hvert vil drive en redaksjonell aktør med reportasjer, ikke bare daglig handel.

## 2. Terminal (valgt)

- **Følelse:** rask, presis, trading-station — du ser data først, pynt sist
- **Bakgrunn:** sort (`#0a0e0a`)
- **Aksent:** neon-grønn (`#00ff88`) + signal-oransje (`#ffb000`) for premium
- **Type:** JetBrains Mono overalt, store bokstaver, ASCII-stil («// subscribe», `## latest feed`, `> ` prompt)
- **Layout:** scrollende ticker-bar i hero, tett news-grid med featured først, smal premium-seksjon
- **Passer når:** du er komfortabel med at leseren din er litt nerdy, og du vil signalisere at innholdet er datadrevet
- **Filer:** `styles.css` (denne er aktiv nå)

**Premium-modell:** betalte lesere får Discord-tilgang via Substack. To knapper i Pro-seksjonen — "bli pro" går til Substack-betaling, "discord ↗" går til invitasjons-lenken. Du trenger ikke bygge innlogging selv.

**Hvorfor denne kan slå feil:** "wall street på 3 minutter" forutsetter at du faktisk leverer det. Hvis du ender opp med lange analyser, vil leseren forvente det visuelle tempoet og føle seg lurt.

## 3. Bloomberg-ish (ikke valgt)

- **Følelse:** seriøs finansredaksjon, som FT, WSJ eller Bloomberg desktop
- **Bakgrunn:** mørk navy (`#0d1b2a`)
- **Aksent:** signal-oransje (`#ff6b1a`) + himmelblå (`#4a9eff`) for sekundær
- **Type:** system-sans for titler, JetBrains Mono for data og ticker
- **Layout:** indeks-strip med 4 indeks-celler (SPX/NDX/DJI/VIX) i hero, "FREE DAILY"-tag på nyhetsbrev, tett kort-design
- **Passer når:** du vil bygge et troverdig finansmerke uten å gå full terminal-nerd
- **Filer:** `styles-bloomberg.css`

**Når det er riktig bytte:** hvis du vil at tittelen skal tas på alvor av tradere, journalister og finansfolk, men uten å skremme bort vanlige investorer.

---

## Praktisk bytte

For å bytte fra Terminal til f.eks. Bloomberg-ish:

1. Kopier den valgte CSS-filen til `styles.css` (overskriver den gamle)
2. Eller bare oppdater `<link rel="stylesheet" href="styles-terminal.css">` i `index.html` til den nye filen
3. Behold `styles-base.css` — den er felles for alle varianter

HTML-en i `index.html` bruker klassenavn som matcher alle tre variantene (`variant-terminal`, `variant-editorial`, `variant-bloomberg`). For å bytte, endre `class` på `<main>` til ønsket variant og bytt CSS-fil. Innholdet forblir likt.

---

## Ting vi ikke har bestemt ennå

- **Navn:** "Aksjeradar" er arbeidstittel. Når du finner et ordentlig navn, endrer du det i `<title>`, i logoen (to steder i `index.html`), og i footer.
- **Bilder:** varianten bruker ikke bilder ennå (ren typografi). Hvis du vil ha en hero-bilde, gi beskjed så legger vi det inn.
- **Nyhetsbrev-leverandør:** skjemaet peker på `#` (placeholder). Du bestemmer selv om det skal være Mailchimp, Substack, Buttondown eller Brevo. Når du velger, får du en embed-kode fra dem som vi limer inn i `index.html` (erstatter hele `<form>`-blokken).
- **Disclaimer-tekst:** "Ikke investeringsrådgivning" er på plass. Hvis du vil at advokat-tekst eller autorisasjons-info skal stå, send det og vi oppdaterer.
