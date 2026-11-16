# Viaggio Lanzarote

Sito statico su Cloudflare Workers (solo asset, nessun codice server) per il viaggio
a Lanzarote del 2-9 dicembre 2026. Stessa impostazione del sito `viaggio-edimburgo`.

## Struttura

```
wrangler.jsonc      config Worker: pubblica solo ./public
public/index.html   il sito, file singolo (HTML + CSS + JS inline, mappa Leaflet su OpenStreetMap)
public/lanzarote-2026.ics   calendario del viaggio, scaricabile dalla Checklist
```

## Calendario

Importare un .ics non manda inviti a nessuno, anche se nel file ci sono gli invitati: gli inviti
partono solo dal calendario di chi crea l'evento.

- **Inviti** (in cima alla Checklist): array `CAL`, un link Google Calendar per evento
  (`action=TEMPLATE`, con `add=` per gli ospiti). Le email degli invitati si scrivono nella
  pagina e restano solo nel browser (`lz26-guests`); nel sito non ci sono indirizzi email.
  Chi apre il link salva l'evento e Google manda l'invito.
- `lanzarote-2026.ics`: lo stesso elenco in un file da importare (METHOD:PUBLISH), senza invitati.

`CAL` e il file .ics vengono dalla stessa lista di eventi: se cambiano gli orari nel sito, vanno
aggiornati entrambi.

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

- Il dominio è protetto da Cloudflare Access: codici di prenotazione e nomi dei passeggeri
  si possono inserire. Mai i link di gestione delle prenotazioni (contengono un token che
  permette di modificarle o cancellarle). Se Access viene disattivato, togliere anche i codici.
- L'array `DAYS` è in ordine di data: spostare un giorno significa spostare il blocco.
- Le tratte (`legs`) usano `H` per l'hotel e `<track><n>` per le tappe (es. `blu2`).
