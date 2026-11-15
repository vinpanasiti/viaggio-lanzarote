# Viaggio a Lanzarote, 2-9 dicembre 2026

Mappa e programma giorno per giorno, stessa impostazione dei siti di Edimburgo e della Norvegia.
Il sito è un file unico: `public/index.html`.

Deploy: Cloudflare Workers (static assets) collegato a questa repo.
Build command vuoto, deploy command `npx wrangler deploy`, root directory `/`.
Ogni push su `main` aggiorna il sito.
