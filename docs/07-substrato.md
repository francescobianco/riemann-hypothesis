# Si può cambiare il substrato?

Domanda di partenza: la struttura della matematica è troppo ricca per essere fissata in modo ottimale in Lean. Perché non cambiare substrato per renderne la rappresentazione meno complessa?

Tesi: il substrato si può cambiare, ma la complessità **si sposta** invece di sparire. Tra sistemi di pari forza il guadagno è limitato. Con sistemi più forti il guadagno può essere enorme, ma si paga in assiomi.

## 1. Le alternative esistono

| Substrato | Esempi | Caratteristica |
|---|---|---|
| teoria dei tipi dipendenti | Lean, Coq/Rocq, Agda | tipi ricchi, prove come termini |
| teoria degli insiemi | Mizar, Metamath (set.mm), Isabelle/ZF | vicina alla pratica classica, tipi deboli |
| logica di ordine superiore | Isabelle/HOL, HOL Light, HOL4 | più semplice, niente tipi dipendenti, automazione forte |
| fondazioni univalenti | UniMath, Agda cubica, HoTT | strutture isomorfe sono *uguali*; trasporto automatico |
| fondazioni categoriali | ETCS, teorie dei topos | la matematica descritta per morfismi |

## 2. Tra sistemi di pari forza il guadagno è limitato

I sistemi di pari forza si traducono l'uno nell'altro con un costo al più polinomiale. Nella teoria della complessità delle prove questo si chiama simulazione tra sistemi di prova (Cook e Reckhow).

È l'analogo del **teorema di invarianza** della complessità di Kolmogorov: due linguaggi universali descrivono lo stesso oggetto con lunghezze che differiscono al più per una costante, il costo del traduttore.

Ne segue che nessun substrato è ottimale per tutto, ma nessuno è nemmeno drasticamente migliore degli altri su tutto.

## 3. Con un sistema più forte il guadagno può essere enorme

**Teorema di accelerazione di Gödel** (1936): per ogni sistema ce n'è uno più forte, per esempio aggiungendo la coerenza del primo. In quello più forte certi teoremi hanno prove più corte di qualsiasi funzione calcolabile fissata.

Un sistema più forte, però, ha **più assiomi**. È l'idea di [04-ultimo-passo.md](04-ultimo-passo.md) vista dall'altro lato. Al limite estremo si prende RH come assioma e la prova diventa una riga. La complessità non sparisce: si trasferisce nella base. Per questo conta che gli assiomi siano "non arbitrari".

## 4. Da dove vengono i 17 milioni di righe

| Fonte | Dipende dal substrato? | Note |
|---|---|---|
| teoria che manca in Mathlib | no | forme automorfe, Langlands, Hodge p-adica: l'archivio le costruisce in `GroundTheory` (1,1 milioni di righe) |
| ridondanza della generazione automatica | no | circa 40.000 nomi di teoremi dichiarati in più moduli; per Alpöge "The Lean is full of garbage obviously, but compiles" |
| raccordo tra presentazioni equivalenti | **sì** | coercioni, `cast`, trasporti lungo equivalenze, lemmi di compatibilità: l'"abuso di notazione" dei matematici, reso esplicito |

Solo il terzo punto dipende davvero dal substrato. È esattamente ciò che l'**univalenza** vuole eliminare, rendendo automatico il trasporto tra strutture equivalenti. Il prezzo è un calcolo più complicato (serve la teoria dei tipi cubica per farlo calcolare) e un'automazione meno matura.

## 5. Il substrato si sceglie anche per altri motivi

- **Fiducia:** un kernel piccolo, con più verificatori indipendenti (per Lean: il kernel ufficiale, nanoda, lean4lean).
- **Automazione:** tattiche, `simp`, ricerca di lemmi.
- **Libreria:** Mathlib è il frutto di anni di lavoro di centinaia di persone. Cambiare substrato vorrebbe dire riscriverla.

## 6. Sintesi

La ricchezza della matematica sta nelle **molte presentazioni equivalenti** della stessa struttura. Un sistema formale deve sceglierne una, e quindi pagare il raccordo con le altre. Il costo si può ridurre, per esempio con l'univalenza, ma un sistema ottimale per tutto non esiste. Il motivo profondo è lo stesso dell'invarianza di Kolmogorov e dell'accelerazione di Gödel.

## Esperimento proposto

Misurare il **costo del substrato** nell'archivio: quanta parte del cono di dipendenze di `riemannHypothesis` è raccordo e quanta è matematica. Indicatori grezzi:

- occorrenze di `cast`, `▸`, `Eq.mpr`, `congrArg`, `Equiv`/`≃` usati come trasporto;
- lemmi il cui nome contiene `_coe`, `_cast`, `_equiv`, `_congr`, `ofEquiv`, `transport`;
- dimensione dei moduli che contengono solo istanze e coercioni.

È un'analisi statica, che non richiede di compilare.
