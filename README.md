# Aksjeradar

Arbeidstittel for en statisk nettside for daglige aksjenyheter, gratis nyhetsbrev-påmelding, og en betalt Pro-tier med Discord-tilgang.

## Modellen

- **Gratis nyhetsbrev** — alle kan melde seg på med e-post
- **Pro-tier (betalt)** — betalende lesere får:
  - Ukentlig selskaps-dypdykk i e-post
  - Månedlig portefølje-rapport
  - Tilgang til en lukket Discord-server for spørsmål, ticker-idéer og diskusjon
  - Mulighet til å poste egne analyser og få tilbakemelding fra andre
- **Betalingsløsning:** Substack (anbefalt for v1)
- **Discord-server:** driftes av deg selv (gratis), invitasjons-lenke går til betalende medlemmer

Du trenger ikke å bygge innlogging, betalingssystem eller forum på siden selv. Substack håndterer betaling og gir deg betalende-medlemmer-listen. Discord håndterer samfunnet. Siden din er bare en markedsplass og en tydelig skillelinje mellom gratis og Pro.

## Stack

- Ren HTML + CSS. Ingen byggesteg, ingen rammeverk.
- Design: **Terminal** (svart bakgrunn, neon-grønn, monospace). De to andre variantene som ble vurdert ligger i [DESIGN.md](./DESIGN.md).
- Hosting: Cloudflare Pages (gratis, knyttet til GitHub).
- Nyhetsbrev-leverandør: ikke valgt ennå. Skjemaet er for øyeblikket en placeholder som peker til `#`.

## Filstruktur

```
aksjeradar/
├── index.html             selve siden
├── styles-base.css        felles typografi, skjemaer, layout-primitiver
├── styles-terminal.css    designvarianten som er valgt
├── styles-editorial.css   alternativ 1 (ikke valgt)
├── styles-bloomberg.css   alternativ 2 (ikke valgt)
├── DESIGN.md              beskrivelse av de to ikke-valgte variantene
├── README.md              denne fila
└── images/                nedlastede stock-bilder (brukes ikke i v1, men ligger her)
```

## Kjøre lokalt

```bash
cd ~/projects/aksjeradar
python3 -m http.server 8765
```

Åpne http://localhost:8765.

Serveren stopper med Ctrl+C. Hvis port 8765 er opptatt, sjekk med `lsof -i :8765 -P -n` og drep den gamle prosessen.

## Legge inn nyhetsbrev-skjemaet skikkelig

Når du har valgt leverandør (Mailchimp, Substack, Buttondown, Brevo, osv.):

1. Logg inn hos leverandøren og opprett en ny liste/nyhetsbrev.
2. Be om "embed-kode for skjema" eller "form action URL".
3. Åpne `index.html`, finn blokken som starter med `<form action="#" method="post">` (ca midt i fila), og erstatt den med leverandørens snippet.
4. Hvis leverandøren krever hidden inputs, legg dem inn i skjemaet.

Eksempel for Mailchimp (ettersender du den faktiske koden fra dem):

```html
<form action="https://YOURNAME.usX.list-manage.com/subscribe/post?u=XXXXX&id=XXXXX" method="post" target="_blank" novalidate>
    <input type="email" name="EMAIL" placeholder="epost@domene.no" required>
    <button type="submit" class="btn btn--primary">subscribe</button>
</form>
```

## Sette opp Discord-serveren

1. Last ned Discord-appen (gratis, macOS/iOS/Android).
2. Klikk "+" på venstre side for å opprette server. Gi den navn (f.eks. "Aksjeradar Pro").
3. Lag noen kanaler: f.eks. `#generelt`, `#ticker-idéer`, `#spørsmål`, `#kunngjøringer`.
4. Sett `#generelt`, `#ticker-idéer` og `#spørsmål` som tilgjengelige for alle (gratis-medlemmer kan ikke se andre kanaler).
   Hvis du vil ha en helt lukket server (kun Pro): hopp over dette og sett hele serveren som kun-tilgjengelig-via-invitasjon.
5. Gå til server-innstillinger → Invitaser folk → Lag en invitasjon. Kopier lenken (ser ut som `https://discord.gg/abc123xyz`).
6. Åpne `index.html` og erstatt `https://discord.gg/DIN-INVITASJON` med din faktiske lenke.

## Sette opp Substack-betaling

1. Opprett en Substack-konto (gratis).
2. Lag et nyhetsbrev med samme navn som Aksjeradar.
3. Slå på "Paid subscription" og sett en pris (f.eks. 99 kr/mnd).
4. Substack gir deg en lenke til betalingssiden — kopier den.
5. Åpne `index.html`, finn `<a href="#" class="btn btn--primary">bli pro</a>` og erstatt `#` med din Substack-URL.
6. I Substack-innstillingene: legg inn en "velkomst-e-post" der du forteller nye betalende om Discord-lenken.

## Deploy til Cloudflare Pages

1. Last opp prosjektet til GitHub (du bruker GitHub Desktop — trykk "Publish repository").
2. Gå til https://dash.cloudflare.com → Pages → Connect to Git → velg repoet.
3. Build command: la stå tom. Build directory: `/` (rot).
4. Klikk "Save and Deploy". Du får en `https://aksjeradar.pages.dev`-adresse etter ~30 sekunder.

For custom-domene (f.eks. `aksjeradar.no`): i Pages-prosjektet → Custom domains → legg til domenet. Hvis domenet ligger hos Domeneshop/One.com/GoDaddy, pek A- og CNAME-recordene derfra (ikke flytt domenet).

## Innhold du må fylle inn før launch

- [ ] Velg et ordentlig navn (erstatter "Aksjeradar" i `<title>`, logo og footer)
- [ ] Velg nyhetsbrev-leverandør og bytt ut skjemaet
- [ ] Sett opp Substack-betaling: lag en "paid subscription" og bytt ut `href="#"` på "bli pro"-knappen med Substack-URL-en din
- [ ] Sett opp Discord-server: opprett server, lag en "Pro"-kanal (skjult for ikke-medlemmer), generer en invitasjons-lenke og erstatt `https://discord.gg/DIN-INVITASJON` i `index.html`
- [ ] Bestem Pro-pris (vanlig: 79-149 kr/mnd for trading-samfunn)
- [ ] **Handelshistorikk-seksjon i Pro-tier** — vurder å vise konkrete handler (kjøp/salg-dato, avkastning) for å vise at det lønner seg. Viktig: vis både vinnere OG tapere for å se troverdig ut, og legg ved disclaimer om at historisk avkastning ikke garanterer fremtidig. Tre alternativer: (1) Én enkelt handel med kontekst, (2) Tabell med flere handler (vinnere + tapere), (3) Lenke til åpen, verifiserbar portefølje (f.eks. Sharesight, Tradervue, eller en åpen konto). Anbefaler versjon 2 for v1.
- [ ] Ekte disclaimer-tekst fra en advokat hvis du vil ha full trygghet
- [ ] Org.nr. i footer hvis du driver dette som foretak

## Ting du kan endre når som helst

- Nyhetene i `<section id="news">` — bare bytt ut kortene
- Ticker-symbolene i hero — endre `<span class="ticker-item">` i `.ticker-track`
- Fargepaletten — endre CSS-variablene øverst i `styles-terminal.css`
- Tekst i `// pro tier`-seksjonen — bytt ut punktene hva Premium skal inneholde

## Personvern og juridisk

Siden samler kun det brukeren skriver inn i nyhetsbrev-skjemaet (én e-postadresse). Den bruker ingen cookies, ingen tracking, ingen tredjepartsanalyse. Du trenger kun en personvernerklæring hvis du begynner å kjøre annonser eller spore brukere.

## Support

Innhold sendes på e-post (norsk eller engelsk). Jeg svarer så fort jeg kan. Hvis noe er ødelagt på siden, send en lenke + et skjermbilde + hva du forventet skulle skje.
