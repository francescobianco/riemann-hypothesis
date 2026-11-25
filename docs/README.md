# Documentazione

Dossier e materiale di lavoro sulla dimostrazione dell'ipotesi di Riemann annunciata da Levent Alpöge con Claude il 20 novembre 2026.

| File | Contenuto |
|---|---|
| [01-dossier.md](01-dossier.md) | cosa è stato annunciato, la cronologia, la nota, lo stato della verifica, le reazioni, le fonti |
| [02-codice-lean.md](02-codice-lean.md) | il sorgente Lean in `lean/riemann-lean`, la sua struttura e come verificarlo |
| [03-piano-analisi.md](03-piano-analisi.md) | il piano di lavoro per l'analisi |
| [04-ultimo-passo.md](04-ultimo-passo.md) | la catena finale verso RH e la lettura della prova come riduzione ad assiomi |
| [05-cosa-e-una-dimostrazione.md](05-cosa-e-una-dimostrazione.md) | dimostrazione e costruzione, prove privilegiate, prove per assurdo |
| [06-matematica-dinamica.md](06-matematica-dinamica.md) | processi per stadi, Gödel, apprendimento al limite, macchine che imparano |
| [07-substrato.md](07-substrato.md) | si può cambiare il substrato formale? costo di rappresentazione e assiomi |
| [08-strati-di-matematica.md](08-strati-di-matematica.md) | estrarre strati riusabili dalla prova; compressione e grafo dei moduli |

## Il codice Lean

Il sorgente (1,8 GB) non è nel repository: è escluso da `.gitignore`. Su un altro computer si scarica con:

```bash
git clone --depth 1 https://github.com/tamnd/riemann-lean.git lean/riemann-lean
```
