# Dossier: la dimostrazione dell'ipotesi di Riemann di Levent Alpöge e Claude

Aggiornato al 25 novembre 2026.

## Sintesi

Il 20 novembre 2026 Levent Alpöge, matematico di Anthropic, ha pubblicato una nota di quattro pagine ([remark.pdf](https://alpo.ge/remark.pdf)). Vi annuncia di aver dimostrato, lavorando con Claude, l'ipotesi di Riemann (RH) e l'ipotesi di Riemann generalizzata (GRH) per le L-funzioni di Dirichlet.

La prova esiste solo come formalizzazione in Lean 4: circa 17,4 milioni di righe in 175.187 file. Non esiste ancora una dimostrazione leggibile da un essere umano.

Stato della verifica al 25 novembre:

- **Verifica formale**: forte. Il codice compila. I cinque teoremi principali usano solo i tre assiomi standard di Lean. Gli enunciati sono quelli di Mathlib. Lean FRO e Mathlib Initiative si dicono pienamente convinti che la prova sia corretta.
- **Comprensione matematica**: assente. Nessuno, autore compreso, ha ancora scritto o assimilato l'argomento completo. Lo dice lo stesso Alpöge.

## Cronologia

| Data | Evento |
|---|---|
| ago 2026 | Anthropic annuncia che un modello Claude non rilasciato ha portato dal 41,6% al 67,2% la quota dimostrata di zeri sulla retta critica. È un risultato parziale, non RH. |
| ott 2026 | Secondo Scientific American, una delle dimostrazioni pubblicate in blocco da OpenAI apre "la prima crepa" nel problema (quasi-RH). |
| 6 ott 2026 | Prima voce della cronologia di Alpöge: il "determinante di Stepanov". Inizia lo sviluppo della prova in Lean. |
| 15 nov 2026 | Arrivano in un solo giorno molti passaggi chiave: theta lift iterati, il seesaw di Rallis, il calcolo del "costo" di ogni lift. |
| 16 nov 2026 | Ultima voce della cronologia: GRH. |
| 20 nov 2026 | Alpöge pubblica `remark.pdf` e l'archivio Lean in 14 parti (`alpo.ge/lean.tar.gz.part01`–`part14`). È il giorno dopo la quotazione in borsa di Anthropic. |
| 21 nov 2026 | Lean FRO e Mathlib Initiative: "We have full confidence that Alpöge's proof of the Riemann Hypothesis is correct". Esce la prima analisi indipendente del codice (hhh2210, v1.0 e v1.1). |
| 22 nov 2026 | Robin Zhang pubblica una nota preliminare sulla prova della GRH (citata da Wikipedia). |
| 23 nov 2026 | Alpöge chiarisce su X che le "250.000 pagine" citate nella nota erano una battuta. |
| 24–25 nov 2026 | La comunità Lean ripulisce il codice in `leanprover-community/riemann-hypothesis`. Ancora nessuna dimostrazione in linguaggio naturale. |

## La nota di Alpöge

La nota non è una dimostrazione. È una dichiarazione con una cronologia commentata. I punti principali:

- **Annuncio**: "I believe I have, working with Claude, discovered a proof of the Riemann Hypothesis". Il metodo si estende alla GRH e, secondo l'autore, con più tempo anche alla Extended RH.
- **Ruolo di Claude**: Alpöge dice di aver guidato la ricerca verso l'impostazione giusta, analoga a un modello giocattolo. Poi Claude lo avrebbe "superato nettamente" sul tratto finale, in particolare sulle forme automorfe di rango alto, che Alpöge ammette di non padroneggiare.
- **Idea chiave dichiarata**: il metodo discende dall'argomento di Stepanov per la RH sui campi di funzioni. Lì la molteplicità di un divisore è essenziale, ma sugli interi è difficile da trovare. Qui la molteplicità viene sostituita da **catene di theta lift**. Per una somma di von Mangoldt di lunghezza X, Claude propone una catena di lunghezza √X.
- **Perché solo Lean**: Alpöge ritiene "inappropriato" pubblicare "some massive unreadable travesty of slop" e sceglie di presentare solo il codice Lean.
- **Tempismo**: Alpöge ammette che il tempismo è "horrible", per la vicinanza alla quotazione in borsa. Scrive anche: "There is an obvious cynical view, to do with the timing, but it is false".
- **Ingredienti citati**: forme CM e ordinarietà, teoria di Iwasawa (con un trucco di Behrend), disuguaglianza di pendenza di Bost, formula di Chowla-Selberg, periodi GGP/Bessel, formula delle tracce relativa, una forma indebolita della congettura di Vandiver, formula di massa di Siegel.

### L'argomento in un paragrafo

Ecco la ricostruzione fatta da [hhh2210/riemann-lean-account](https://github.com/hhh2210/riemann-lean-account) a partire dal sorgente Lean:

1. Una forma "a finestre" del teorema di Landau riduce l'enunciato, per ogni carattere, a una stima unilaterale di una somma lisciata sulle potenze dei primi.
2. Un'identità esatta per un campo CM ausiliario trasforma quella stima in un limite superiore. Il limite riguarda il conteggio pesato delle potenze dei primi il cui Frobenius cade fuori da un tipo CM.
3. Il limite ha la forma x/m + m. L'esponente 1/2 nasce scegliendo m vicino a √x. Qui m è la lunghezza della catena di theta lift tra gruppi unitari definiti.
4. Il limite si ottiene stringendo la pendenza di un reticolo di periodi tra due stime:
   - **dal basso**: divisibilità dei periodi ai primi inerti (gruppo formale di altezza 2 di una curva ellittica CM) più una disuguaglianza di pendenza;
   - **dall'alto**: calcolo esatto della pendenza, che tramite Chowla-Selberg coincide con il termine principale del conteggio.

## La formalizzazione Lean

| Voce | Valore |
|---|---|
| Archivio | `lean.tar.gz`, 239.275.554 byte, SHA-256 `9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc` |
| Dimensione | 175.187 file Lean, 17.374.215 righe |
| Toolchain | Lean 4.33.1, Mathlib al commit `0df444a360eaa60ab8c11dca51a86af692955474` |
| Compilazione | `lake build Main` riuscita: 183.893 job, 13,4 ore su 32 core |
| Assiomi | solo `propext`, `Classical.choice`, `Quot.sound`. Nessun assioma dichiarato nell'archivio. |
| Replay del kernel | 2.695.292 dichiarazioni ricontrollate, 0 errori |
| Dipendenze di `riemannHypothesis` | 1.478.219 dichiarazioni, di cui 1.075.015 teoremi dell'archivio (69%) |
| Letteratura | 179 enunciati di teoremi pubblicati, formalizzati e usati |

Dati da [hhh2210/riemann-lean-account](https://github.com/hhh2210/riemann-lean-account). I dettagli sul codice e su come verificarlo sono in [02-codice-lean.md](02-codice-lean.md).

## Verifica e reazioni

Secondo [Wikipedia](https://en.wikipedia.org/wiki/Alp%C3%B6ge's_proof_of_the_Riemann_hypothesis), la verifica formale poggia su quattro criteri:

1. l'enunciato usa la definizione di Mathlib già verificata;
2. il codice compila in un file di export, cosa che i volontari hanno fatto "dozens of times over";
3. il comparator controlla che l'enunciato di RH non sia stato ridefinito;
4. l'export supera tutti i checker noti.

Andrew Sutherland ha compilato la prova in modo indipendente.

Reazioni riportate:

| Chi | Posizione |
|---|---|
| Lean FRO e Mathlib Initiative | "full confidence" che la prova sia corretta |
| James Maynard | "a historic milestone" |
| Brian Conrey | "the biggest thing I think that's ever happened in mathematics" |
| Martin Bridson (presidente del Clay Mathematics Institute) | "an announcement of historic significance" |
| Terence Tao (lettura preliminare) | la prova sembra usare "the full power of modern developments on the Langlands program and arithmetic geometry" |
| Andrew Granville | critica la chiarezza della nota ("What he released is garbled", secondo resoconti di stampa) e il tempismo rispetto alla quotazione in borsa |
| Maksym Radziwiłł | sottolinea la mancanza di comprensione umana della formalizzazione |
| Steven Strogatz | "A very skilled AI jockey riding a very fast AI horse." |
| Alpöge stesso, su X | "The Lean is full of garbage obviously, but compiles" |

Il Clay Institute non ha ancora deciso sul premio. Le sue regole richiedono una pubblicazione e un periodo di attesa.

## Domande aperte

- **La dimostrazione umana di Stanford**: diversi resoconti parlano di una dimostrazione scritta interamente da esseri umani, uscita lo stesso giorno (Nature, Quanta: "a human proof emerges"). Non sono riuscito a leggere direttamente quelle fonti, perché Nature richiede l'accesso. Autore e dettagli sono da verificare.
- **Sostanza della prova**: quanta parte dei 17 milioni di righe è davvero necessaria? Il 69% dei teoremi è nel cono di dipendenze, ma molti potrebbero essere ridondanti.
- **Semplificazione**: Alpöge prevede argomenti brevi "fairly quickly".

## Fonti

Pagine lette direttamente:

- Alpöge, [Remark](https://alpo.ge/remark.pdf), 20 novembre 2026
- [Wikipedia: Alpöge's proof of the Riemann hypothesis](https://en.wikipedia.org/wiki/Alp%C3%B6ge's_proof_of_the_Riemann_hypothesis)
- Scientific American, [Anthropic mathematician claims to prove Riemann hypothesis](https://www.scientificamerican.com/article/anthropic-mathematician-claims-to-prove-riemann-hypothesis-most-famous-problem-in-all-of-math/), 20–21 novembre 2026
- [leanprover-community/riemann-hypothesis](https://github.com/leanprover-community/riemann-hypothesis), README
- [hhh2210/riemann-lean-account](https://github.com/hhh2210/riemann-lean-account), README (v1.1)
- [tamnd/riemann-lean](https://github.com/tamnd/riemann-lean), copia non modificata, clonata in `lean/riemann-lean`

Viste solo nei risultati di ricerca:

- Nature, [Mathematicians astounded by AI and human-written proofs](https://www.nature.com/articles/d41586-026-03812-7)
- [Phys.org](https://phys.org/news/2026-11-anthropic-claude-math-greatest-riddle.html)
- [IBM Think](https://www.ibm.com/think/news/anthropic-claude-riemann-hypothesis-math-proof)
- [DataCamp: Two Proofs in One Day](https://www.datacamp.com/blog/ai-solved-riemann-hypothesis)
- Scientific American, [No, AI didn't just solve the thorniest problem in math](https://scientificamerican.com/article/no-ai-didnt-just-solve-the-thorniest-problem-in-math) (risultato di agosto)
