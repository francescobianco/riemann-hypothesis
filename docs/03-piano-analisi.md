# Piano di analisi

Obiettivo: capire che cosa dimostra davvero il codice e come è organizzato, senza poterlo compilare per intero in locale.

## 1. Gli enunciati

- [ ] Leggere `Statements.lean` e confrontarlo con le definizioni di Mathlib (`riemannZeta`, `DirichletCharacter.LFunction`).
- [ ] Controllare che le ipotesi escludano esattamente gli zeri banali e il polo, né più né meno.
- [ ] Confrontare con `src/Challenge.lean` di leanprover-community.

## 2. Mappa della prova dall'alto

- [ ] Partire da `Supplements/Kernels/MainTheoremsAll` e seguire `rhOfAll`, `allChi`, `claimAll` e `critAll` per alcuni livelli.
- [ ] Associare ogni passo ai capitoli della ricostruzione di hhh2210: Landau a finestre, identità CM, stima x/m + m, catena di theta lift, pendenza del reticolo.

## 3. Analisi statica (fattibile su questa macchina)

- [ ] Grafo degli `import` tra i 175.187 moduli: componenti, profondità, moduli più importati.
- [ ] Distribuzione di file e righe per sotto-cartella di `Development` (70% del codice).
- [ ] Ricerca dei duplicati: nomi di teoremi dichiarati in più moduli (hhh2210 ne conta circa 40.000).
- [ ] Elenco dei 179 teoremi della letteratura formalizzati e dove sono usati.
- [ ] Opzioni insolite: `set_option maxHeartbeats`, `unsafe`, `partial`, `implemented_by`, `extern`, `macro` e `elab` personalizzati.

## 4. Verifica

- [ ] Riscaricare la release da `alpo.ge` e confrontare gli hash (`scripts/verify-source.sh`).
- [ ] Installare `elan` e aprire il progetto con la cache precompilata, così da poter navigare con l'LSP.
- [ ] Se c'è una macchina grande a disposizione: `lake comparator` in sandbox.

## Domande per iniziare

- Quanto del codice è davvero necessario? Il 69% dei teoremi dell'archivio è nel cono di dipendenze.
- Dove sta il passaggio "√X theta lift" della nota nel codice?
- Quali parti sono infrastruttura generica, cioè teoria che manca in Mathlib, e quali sono specifiche della prova?
