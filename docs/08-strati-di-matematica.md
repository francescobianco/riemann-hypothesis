# Sintetizzare strati di matematica dalla prova

Domanda di partenza: se questa dimostrazione fosse accettata, potremmo ricavarne strati di matematica che rendano i passaggi delle dimostrazioni di livello più alto?

Tesi: sì, e in parte quegli strati esistono già nel codice. Non serve aspettare l'accettazione per estrarli, perché la correttezza la garantisce il kernel. L'accettazione darebbe la motivazione a investire per renderli puliti e riusabili.

## 1. Il precedente storico

I grandi teoremi hanno sempre prodotto teoria:

| Problema | Strato di matematica nato o consolidato |
|---|---|
| congetture di Weil | coomologia étale, teoria degli schemi (Grothendieck), Deligne |
| ultimo teorema di Fermat | teoremi di modularità, deformazioni di rappresentazioni di Galois (Wiles, Taylor) |
| congettura di Poincaré | flusso di Ricci con chirurgia (Hamilton, Perelman) |
| teorema dei quattro colori | tecniche di riducibilità verificate al calcolatore; prova formale in Coq (Gonthier) |

Una prova lunga funziona come una miniera. Se ne estraggono concetti e lemmi che diventano i "passi elementari" delle prove successive. Con lo strato giusto, ciò che oggi occupa migliaia di righe si riduce a un'applicazione di un teorema.

## 2. Che cosa c'è già nell'archivio di Alpöge

| Strato | Dove | Dimensione |
|---|---|---|
| teoremi della letteratura formalizzati | 179 enunciati di risultati pubblicati, raccolti attorno a `UpperTheory.LiteratureTheorems` | tutti usati dalla prova finale |
| teoria di base | `GroundTheory`: geometria aritmetica, forme automorfe, gruppo di Brauer, campi di funzioni, altezze e theta, coomologia profinita e di Tate, Hodge p-adica | 8.507 file, 1,1 milioni di righe |
| reciprocità | `ReciprocityTheory` | 3.845 file, 0,56 milioni di righe |
| la cima della prova | `Row ∧ RecTExists ⟹ EveryCharacter ⟹ RH` | poche righe ([04-ultimo-passo.md](04-ultimo-passo.md)) |

Portare questi strati in Mathlib, ripuliti, renderebbe accessibile a tutti un pezzo enorme di teoria dei numeri moderna. È in parte l'obiettivo di [leanprover-community/riemann-hypothesis](https://github.com/leanprover-community/riemann-hypothesis), che vuole il codice "readable [...] aspirationally to Mathlib levels".

## 3. Sintesi automatica: comprimere il corpus

Un corpus di prove formali si può **comprimere**. Si cercano sotto-prove e schemi che si ripetono, e li si trasformano in lemmi e definizioni.

| Tecnica | Idea | Riferimenti |
|---|---|---|
| anti-unificazione | dato un insieme di termini simili, trovare lo schema più specifico che li generalizza | Plotkin (1970), Reynolds (1970) |
| library learning | imparare una libreria di astrazioni che accorcia un insieme di programmi | DreamCoder (Ellis et al., 2021), Stitch (Bowers et al., 2023) |
| estrazione di lemmi da prove | individuare sotto-prove riusabili e promuoverle a lemmi con nome | LEGO-Prover (Wang et al., 2023) |

Uno strato buono è un'ottima compressione, nel senso di [07-substrato.md](07-substrato.md). Non accorcia la prova totale, perché il contenuto resta. Però ne **ammortizza** il costo su molti teoremi.

## 4. Due cautele

- **Un'astrazione utile non è per forza un concetto naturale.** Un lemma estratto solo perché compare spesso può non avere alcun significato matematico. Riconoscere quali strati sono concetti veri, e dar loro un nome, resta un lavoro di **comprensione**. È proprio la parte che oggi manca a questa prova ([05-cosa-e-una-dimostrazione.md](05-cosa-e-una-dimostrazione.md), prova certificata e prova compresa).
- **L'accettazione conta soprattutto sul piano sociale.** La correttezza degli strati la garantisce già il kernel. L'accettazione renderebbe affidabili i lemmi intermedi come mattoni per altri e giustificherebbe l'investimento per pulirli.

## 5. Collegamento con la matematica dinamica

Estrarre strati è un processo per stadi: ogni strato rende "elementari" passi che prima erano lunghi, e il successivo si costruisce sopra. È una forma concreta della matematica dinamica di [06-matematica-dinamica.md](06-matematica-dinamica.md). Con una differenza importante: qui gli stadi **non aggiungono assiomi**. Riorganizzano le costruzioni dentro lo stesso sistema. Si guadagna in altezza, non in forza logica.

## Esperimento proposto: il grafo dei moduli

1. Estrarre il grafo degli `import` dei 175.187 moduli. Si ottiene con un'analisi testuale delle righe `import`, senza compilare.
2. Calcolare per ogni modulo:
   - quante volte è importato (grado entrante);
   - se sta su tutti i cammini verso `Main` (colli di bottiglia, dominatori);
   - la profondità rispetto a Mathlib e rispetto a `Main`.
3. I moduli molto importati e i colli di bottiglia sono i **candidati naturali a diventare strati**.
4. Passo successivo, che richiede Lean: ripetere l'analisi a livello di dichiarazioni, con il cono di dipendenze come in hhh2210, e cercare schemi ricorrenti con l'anti-unificazione.
