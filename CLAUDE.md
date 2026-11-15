# Viaggio Lanzarote

Sito statico su Cloudflare Workers (solo asset, nessun codice server) per il viaggio
a Lanzarote del 2-9 dicembre 2026. Stessa impostazione del sito `viaggio-edimburgo`.

## Struttura

```
wrangler.jsonc      config Worker: pubblica solo ./public
public/index.html   il sito, file singolo (HTML + CSS + JS inline, mappa Leaflet su OpenStreetMap)
```

## Viste

- **Mappa per giorno**: array `DAYS`, in ordine di data. Ogni giorno ha `note`, `transport`,
  `food`, `eat`, `flex`, `sun` (alba/tramonto), `sched`, `legs` (tratte) e `pts` (tappe).
  Ogni tappa può avere `q` (ricerca per Google Maps), `kids` (riquadro per le bambine) e
  `arch` (approfondimento architettonico).
- **Tabella completa**: generata da `DAYS`.
- **Gourmet**: contenuto statico in `buildGourmet()`.
- **Checklist**: array `CHECK`; le spunte sono salvate nel browser di chi le mette.

## Colori delle tappe

`blu` programma, `arancio` opzionale, `verde` alternativa, `hotel` alloggio.

## Foto

Non ci sono file immagine. Ogni tappa ha `wiki: ['Titolo_pagina', ...]` con titoli di
Wikipedia in inglese: il sito prende la foto al volo, provando i titoli in ordine.

## Deploy

Ogni push su `main` ripubblica il sito tramite Cloudflare Workers Builds.

## Regole per le modifiche

- Non inserire nel sito codici di prenotazione o link di gestione delle prenotazioni:
  il sito è raggiungibile da chiunque abbia l'URL.
- L'array `DAYS` è in ordine di data: spostare un giorno significa spostare il blocco.
- Le tratte (`legs`) usano `H` per l'hotel e `<track><n>` per le tappe (es. `blu2`).
