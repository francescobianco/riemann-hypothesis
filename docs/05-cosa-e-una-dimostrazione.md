# Che cos'è una dimostrazione

Domanda di partenza: una dimostrazione è solo una costruzione, cioè una sequenza di passi? Se sì, che cosa la distingue dalle altre costruzioni? E come vanno lette le dimostrazioni per assurdo?

Tesi di questa nota: in Lean, e più in generale secondo la corrispondenza di Curry-Howard, **ogni dimostrazione è una costruzione**. Ciò che la rende *dimostrazione* non è il modo in cui è fatta, ma **l'uso che se ne fa**: di una dimostrazione conta solo che esista, non quale sia.

## 1. Dimostrazioni come costruzioni (Curry-Howard)

| Logica | Calcolo |
|---|---|
| enunciato A | tipo A |
| dimostrazione di A | termine (programma) di tipo A |
| A → B | funzione che trasforma prove di A in prove di B |
| A ∧ B | coppia ⟨prova di A, prova di B⟩ |
| ∀ x, P x | funzione che a ogni x associa una prova di P x |
| ∃ x, P x | coppia ⟨testimone, prova⟩ |
| verificare una prova | controllare il tipo (il kernel) |

`riemannHypothesis` è letteralmente un termine, cioè un oggetto costruito, il cui tipo è `RiemannHypothesis`. I 17 milioni di righe sono il programma che lo costruisce.

## 2. Quando una costruzione è una dimostrazione

Lean separa due mondi:

- **`Type`, costruzioni come dati.** Conta *quale* oggetto si costruisce: due numeri naturali diversi sono diversi.
- **`Prop`, costruzioni come dimostrazioni.** Vale la *proof irrelevance*: due prove dello stesso enunciato sono per definizione uguali.

```lean
example (p : Prop) (h₁ h₂ : p) : h₁ = h₂ := rfl   -- tutte le prove di p coincidono
```

Ecco la risposta alla domanda "quando un processo si riconosce come dimostrazione e non come costruzione". Succede quando dimentichiamo il processo e teniamo solo il fatto che il tipo è abitato. Una dimostrazione è una costruzione vista *a meno del suo contenuto*.

Ne seguono due fatti:

- **Ogni costruzione è una dimostrazione, ma spesso banale.** Un termine `t : A` prova almeno che "A è abitato". Questo vale anche per `3 : ℕ`. Diventa interessante solo quando il tipo è un enunciato che qualcuno ha posto.
- **Il contenuto può contare lo stesso.** Una prova costruttiva di `∃ x, P x` contiene un testimone. Lean lo sa e vieta di estrarlo da un `Prop`: il testimone c'è, ma non è accessibile come dato. Per usarlo bisogna formulare l'enunciato in `Type`, come `{x // P x}`.

## 3. Dimostrazioni privilegiate

Tra le prove dello stesso enunciato, alcune valgono più di altre anche se Lean le considera uguali. I criteri principali:

| Criterio | Idea | Riferimento |
|---|---|---|
| **analiticità** | usa solo concetti presenti nell'enunciato (proprietà della sottoformula) | Gentzen, eliminazione del taglio |
| **normalità** | nessuna deviazione: nessun lemma introdotto e subito eliminato | Prawitz |
| **spiegazione** | mostra *perché* è vero, non solo *che* lo è | Steiner; Thurston, *On proof and progress* |
| **brevità / sorveglianza** | una mente può percorrerla per intero | Wittgenstein, la prova "perspicua" |

Il caso di Alpöge è l'estremo opposto dell'analiticità. Per un enunciato su ζ usa Langlands, teoria di Iwasawa, geometria di Arakelov: è un **taglio** enorme. Il teorema di Gentzen dice che in linea di principio i tagli si possono eliminare, ma la prova analitica risultante sarebbe di dimensioni astronomiche. Quindi i lemmi intermedi (`Row`, `RecTExists`) non sono un ornamento. Sono ciò che rende la prova finita e praticabile.

C'è poi una distanza tra **prova certificata** e **prova compresa**. Il kernel di Lean accetta questa prova. La comunità matematica non l'ha ancora riconosciuta come *sua*, perché manca la spiegazione. Sono due sensi di "dimostrazione" che qui per la prima volta si separano in modo netto.

## 4. Le dimostrazioni per assurdo

Bisogna distinguere due cose diverse:

1. **Provare ¬A** supponendo A e arrivando a una contraddizione. È pienamente costruttivo: ¬A *è per definizione* `A → False`, quindi si costruisce una funzione.
2. **Provare A** supponendo ¬A e arrivando a una contraddizione. Serve ¬¬A → A, cioè il terzo escluso. Questo non è costruttivo: non si esibisce nulla di A.

In Lean il secondo tipo passa per `Classical.choice`, che implica il terzo escluso (teorema di Diaconescu). Lo si vede anche nell'ultimo passo della prova di Alpöge:

```lean
-- Formalism/GammaFactorNonvanishing/CritLine.lean
theorem Formalism.VW.critLine ... : s.re = 1 / 2 := by
  ...
  by_contra hne                          -- assurdo di tipo (2)
  rcases lt_or_gt_of_ne hne with hlt | hgt
```

Hai ragione però a dire che "in un certo senso" anche queste sono costruttive. In tre sensi precisi:

- **Traduzione di Gödel-Gentzen.** Ogni prova classica di A si trasforma meccanicamente in una prova *costruttiva* di una versione indebolita di A (A con doppie negazioni). L'assurdo non sparisce: si sposta nell'enunciato.
- **Enunciati di forma Π⁰₁.** Sono enunciati del tipo "per ogni n vale P(n)", con P verificabile da un algoritmo. Per questi, la prova classica e quella costruttiva si equivalgono: se P è decidibile, ¬¬P(n) implica P(n). **RH è equivalente a un enunciato Π⁰₁** (Davis, Matiyasevich, Robinson; e anche la disuguaglianza di Lagarias, che si verifica con un calcolo finito per ogni n).
- **Risultati di conservatività** (Friedman e altri). Per enunciati aritmetici di questa forma, i sistemi classici non dimostrano nulla in più rispetto a quelli costruttivi. Nemmeno l'assioma di scelta aggiunge nulla per questi enunciati (assolutezza di Shoenfield).

**Per RH, quindi, l'uso del terzo escluso e della scelta è in linea di principio eliminabile.** Esiste una prova costruttiva, che però nessuno ha scritto. Dalla prova classica la si estrae con un procedimento meccanico.

Una cautela: questi risultati valgono per l'aritmetica e la teoria degli insiemi. Il trasferimento alla teoria dei tipi di Lean con `Classical.choice` è plausibile ma andrebbe verificato.

## 5. Sintesi: dimostrazione come categoria di processi

| | Costruzione | Dimostrazione |
|---|---|---|
| oggetto | termine `t : A`, con `A : Type` | termine `p : A`, con `A : Prop` |
| identità | conta quale termine | tutti i termini sono uguali (irrilevanza) |
| cosa trasmette | il dato | la verità dell'enunciato |
| verifica | controllo di tipo | controllo di tipo, la stessa operazione |
| ricerca | non decidibile in generale | non decidibile in generale |

Una possibile formulazione della tua idea è questa. Una dimostrazione è un processo costruttivo *riconosciuto* da un verificatore (il kernel, o una comunità) relativamente a un enunciato. Dopo il riconoscimento resta solo la verità dell'enunciato, e il processo si dimentica. Tra i processi equivalenti, alcuni sono privilegiati per analiticità, normalità o capacità di spiegare. Questi criteri stanno *fuori* dalla logica formale, che non li distingue.

## Domande aperte per il nostro lavoro

- Dove usa davvero la prova di Alpöge `Classical.choice` e il terzo escluso? È solo comodità di Mathlib o è essenziale in qualche punto?
- Le tre ipotesi in cima (`Row`, `RecTExists`, `VWin`) sono "tagli". Quanto sarebbe più lunga la prova senza di essi?
- Si può misurare quanto la prova sia "analitica", per esempio contando quali concetti compaiono nel cono di dipendenze rispetto all'enunciato?
