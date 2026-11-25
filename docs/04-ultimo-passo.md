# L'ultimo passo della dimostrazione

Obiettivo: isolare in alto la catena che porta all'ipotesi di Riemann (RH) e leggere il resto della prova come una riduzione di ipotesi intermedie ai soli assiomi di Lean.

## L'idea di lavoro

Una dimostrazione in Lean è un termine. Il suo tipo è l'enunciato, e le sue foglie sono assiomi. Per `riemannHypothesis` le foglie sono solo tre: `propext`, `Quot.sound` e `Classical.choice`.

Se si scrivesse `axiom rh : RiemannHypothesis`, tutto ciò che sta sopra RH resterebbe valido, ma su un assioma arbitrario. I 17 milioni di righe servono a eliminare quell'assioma, sostituendolo con una costruzione che arriva fino ai tre assiomi standard. In questo senso la prova è un grande "ricamo" di riduzioni.

Due precisazioni:

- **Le definizioni da sole non bastano.** Svolgere una definizione non prova nulla di nuovo, perché le definizioni sono estensioni conservative. Il contenuto sta nei termini di prova: lemmi, induzioni, uso della scelta. In questo senso "unire per definizioni" significa che ogni nome si svolge fino al nucleo. Il lavoro vero è costruire i termini.
- **Non tutto è riducibile.** Per Gödel, alcuni enunciati veri non sono dimostrabili da un dato sistema di assiomi. Che RH lo sia non era garantito a priori. Questa prova, se corretta, mostra che RH si riduce ai tre assiomi.

## La catena in alto

```
riemannHypothesis                                   Main.lean
 └ rhOfAll := Formalism.VW.rh_of_every allChi        Supplements/Kernels/MainTheorems.lean
     │  passo finale: da "nessuno zero per Re s > 1/2" a RH,
     │  usando l'equazione funzionale e il carattere banale mod 1   Formalism/GammaFactorNonvanishing/CritLine.lean
     └ allChi : UpperTheory.EveryCharacter
         := FinalTheorems.everyCharacter                              CharacterBoxes/MainConclusion.lean
         := everyCharacter_of_row galoisLocalGlobalStrong_recParameter
              └ every_character_of_literature_of_vWin
                  ( O  : OpenLiteratureStatements := ⟨h, recTExists, h⟩ ,
                    hV : VWin := vwin_of_row h )                      .../ThirdPlaceSeries/EveryCharacterOfLiteratureOfVWin.lean
```

### Passo 1: da `EveryCharacter` a RH (classico, circa 25 righe)

`EveryCharacter` dice che per ogni modulo N, ogni carattere di Dirichlet χ e ogni s con Re s > 1/2, si ha L(χ, s) ≠ 0, tranne il polo del carattere banale in s = 1.

Da qui si ricava RH. Si prende χ = 1 mod 1, che dà ζ. Se ζ(s) = 0 con Re s < 1/2, la funzione ζ completata e l'equazione funzionale producono uno zero in 1 − s, che ha Re > 1/2: contraddizione. Gli zeri della funzione Γ corrispondono esattamente agli zeri banali −2, −4, …

Questo passo è matematica da manuale. Tutta la difficoltà sta in `EveryCharacter`.

### Passo 2: `EveryCharacter` ridotto a tre ipotesi nominate

Il penultimo nodo prende due argomenti:

| Ipotesi | Tipo Lean | Contenuto (dal nome) |
|---|---|---|
| `caraianiLocalGlobalDual` | `UpperTheory.One.M.Row` | compatibilità locale-globale per rappresentazioni di Galois (Caraiani e altri) |
| `localLanglandsExists` | `UpperTheory.Zero.LLC.RecTExists` | esistenza della corrispondenza di Langlands locale |
| `galoisRepOfPolarizedPairLit` | `UpperTheory.One.M.Row` | rappresentazioni di Galois di coppie polarizzate. Ha lo **stesso tipo** della prima. |
| `hV` | `UpperTheory.Top.VWin` | `∀ ψ : PChar, VWinAt ψ 401`. Viene ricavata da `Row` con `vwin_of_row`. |

In cima, la prova di RH diventa quindi questa catena:

```
Row  ∧  RecTExists   ⟹   EveryCharacter   ⟹   RH
```

`Row` è fornito da `UpperTheory.Gal.Int.galoisLocalGlobalStrong_recParameter`, `RecTExists` da `UpperTheory.Zero.LLC.recTExists`.

Questa è proprio la forma dell'idea di lavoro: in alto, enunciati della letteratura usati come ipotesi; sotto, il codice che li dimostra fino agli assiomi.

## Prossimi passi

- [ ] Leggere `Row0b` (`Foundations/GaloisRepresentations/TypeConditions/WeightsConjBis.lean:69`) e `RecTExists`, e scriverli in linguaggio matematico.
- [ ] Misurare il cono di dipendenze di ciascuna delle due prove. Quale porta la maggior parte dei 17 milioni di righe?
- [ ] Capire dove entra la catena di √X theta lift. Deve stare sotto `every_character_of_theta_integrals_local_matching`.
- [ ] Esperimento con gli assiomi, quando Lean sarà installato: in un file separato dichiarare `axiom row : UpperTheory.One.M.Row` e `axiom rec : UpperTheory.Zero.LLC.RecTExists`, derivare RH, e lanciare `#print axioms`. Si ottiene la "dimostrazione corta modulo letteratura", che serve solo la compilazione dei moduli sopra quel livello.
