# Gym Tracker

Pagina web **in un unico file** (`index.html`, nessuna dipendenza esterna) per tenere traccia delle schede di allenamento in palestra. Funziona offline, su telefono e su GitHub Pages. Nessun database: lo storico vive nel `localStorage` del browser e nel JSON che esporti.

## Uso

1. Apri `index.html` nel browser (o pubblicalo con GitHub Pages: *Settings → Pages → Deploy from branch → main / root*).
2. **Allenamento**: scegli la scheda, inserisci i kg per ogni serie (le macchine `M1` e i manubri `M2`), spunta gli esercizi a corpo libero, aggiungi note e premi *Salva sessione*. I campi sono precompilati con i kg dell'ultima sessione (o con i valori di esempio della scheda). Quello che scrivi viene conservato come bozza anche se chiudi la pagina a metà allenamento.
   - **Tempo Run**: seleziona il blocco del circuito in cui ti sei fermato, i metri percorsi in quel blocco e il tempo totale del workout.
   - Per registrare una giornata passata basta cambiare la data prima di salvare.
3. **Storico**: elenco delle sessioni filtrabile per scheda, con dettaglio, *Modifica* ed *Elimina*.
4. **Grafici**: andamento per esercizio (kg max e media per sessione) oppure, per Tempo Run, tempo totale e punto raggiunto nel circuito (metri percorsi su 6000). Finestra scorrevole a 7 giorni, estendibile a 1 mese, 3 mesi, 6 mesi, 1 anno.
5. **Dati**: esporta il JSON (scarica o copia), importa un JSON (unione per id, senza duplicati, oppure sostituzione completa), cancella tutto.

## Persistenza

- Dopo ogni allenamento esporta il JSON e conservalo (ad esempio committandolo in questo repository).
- Su un nuovo dispositivo o browser importa l'ultimo JSON: ritrovi tutto lo storico. Importare lo stesso file più volte non crea duplicati.

## Modificare le schede

Le schede sono definite nella costante `SCHEDE` in cima allo script di `index.html`: ogni esercizio ha `kind` (`M1` macchina, `M2` manubri, `''` corpo libero), `reps` (una voce per serie), opzionalmente `defaults` (kg di esempio), `note` e `scheme` (testo mostrato al posto di "4x10").

## Formato del JSON

```json
{
  "app": "gym-tracker",
  "version": 1,
  "exportedAt": "2026-10-15T20:00:00.000Z",
  "sessions": [
    {
      "id": "…", "scheda": "gambe", "date": "2026-10-15T18:30",
      "createdAt": "…", "updatedAt": "…", "notes": "",
      "exercises": { "leg": { "kg": [50, 55, 60, 62.5] }, "pressa": { "kg": [90, 90, 90, 90] } }
    },
    {
      "id": "…", "scheda": "tempo_run", "date": "2026-10-14T19:00",
      "createdAt": "…", "updatedAt": "…", "notes": "",
      "run": { "block": 9, "meters": 560, "minutes": 43 }
    }
  ]
}
```

`block` è l'indice (da 0) del blocco del circuito raggiunto.
