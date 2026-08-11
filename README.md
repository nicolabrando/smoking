# Conta Sigarette (PWA)

App per contare sigarette e Nicorette, con timer "dall'ultima", intervallo
massimo di oggi, record assoluto, analitica e storico per giorno.
I dati restano **solo sul dispositivo** (localStorage): non vengono inviati da nessuna parte.

## Importante quando modifichi i file

Il service worker serve i file dalla cache. **A ogni modifica cambia
`CACHE_VERSION` in `sw.js`** (es. `2026-08-12-1`), altrimenti i dispositivi che
hanno già installato l'app continueranno a usare la versione vecchia.

## Struttura

- `index.html` – tutta l'app (interfaccia + logica, senza dipendenze esterne)
- `sw.js` – service worker: cache versionata, stale-while-revalidate
- `manifest.json` – dati di installazione PWA
- `icons/` – icone PNG (180 per iOS, 192/512 per Android, 512 maskable)
- `esempio-import.csv` – esempio di file importabile (vedi sotto)

Nessuna dipendenza da CDN esterni: l'app funziona anche completamente offline.
Le istruzioni per installarla sulla schermata Home (iPhone/Android) sono
dentro l'app, in **Impostazioni**.

## Backup / formato CSV

Da **Impostazioni** si esporta e si importa un CSV. Formato esportato: una riga
per evento, colonne `data,ora,tipo`:

```csv
data,ora,tipo
2026-08-10,04:21:00,sigaretta
2026-08-10,07:46:00,sigaretta
2026-08-11,09:30:00,nicorette
```

L'importazione è tollerante: separatore `,` o `;`, date `AAAA-MM-GG` o
`GG/MM/AAAA`, orari con o senza secondi, tipo abbreviabile (`s`/`n`). È accettato
anche il formato "largo" (una riga per data e gli orari nelle colonne successive:
in quel caso sono tutte sigarette). All'importazione si sceglie **Unisci**
(aggiunge solo gli eventi nuovi, i duplicati vengono ignorati) o **Sostituisci**.

`esempio-import.csv` contiene le sigarette del 10 agosto 2026 visibili nella
vecchia app (le prime 5 di 8): puoi completarlo/correggerlo con un editor di
testo e importarlo per ripartire con lo storico.

L'app ricorda di esportare i dati se l'ultimo backup risale a più di 7 giorni fa.

## Impostazioni utili

- **Obiettivo giornaliero**: massimo di sigarette al giorno mostrato in Casa (es. `2 / 10`).
- **Record assoluto**: parte dal valore di base `813:53:02` (il record della
  vecchia app) e si aggiorna da solo se lo superi; è modificabile o azzerabile.
