# Il codice Lean in questo repository

## Dove si trova

`lean/riemann-lean/` è un clone superficiale (`--depth 1`) di [tamnd/riemann-lean](https://github.com/tamnd/riemann-lean). È la release di Alpöge del 20 novembre 2026, byte per byte. Occupa 1,8 GB e ha un repository git proprio, escluso dal nostro tramite `.gitignore`.

| Percorso | Contenuto |
|---|---|
| `lean/riemann-lean/lean/` | la release originale: `lakefile.toml`, `lean-toolchain`, `src/` |
| `lean/riemann-lean/lean/src/Main.lean` | i cinque teoremi finali |
| `lean/riemann-lean/lean/src/Statements.lean` | i quattro enunciati sulle L-funzioni di Dirichlet (importa solo Mathlib) |
| `lean/riemann-lean/manifests/` | SHA-256 delle 14 parti, dell'archivio e di ogni file |
| `lean/riemann-lean/docs/` | `STRUCTURE.md`, `BUILD.md`, `SCAN.md` del mirror |

## Verifiche fatte qui (25 novembre 2026)

| Controllo | Esito |
|---|---|
| `sha256sum -c manifests/files.sha256` (175.190 voci) | tutti i file coincidono |
| Ricerca testuale di `sorry`, `admit`, `axiom`, `native_decide` in `lean/src` | nessun risultato |
| Compilazione | non fatta: Lean non è installato e la macchina ha 12 core e 31 GB di RAM |

Il manifest viene dal mirror stesso. Per un confronto davvero indipendente con `alpo.ge` serve `scripts/verify-source.sh`, che riscarica 239 MB.

## I teoremi finali

```lean
-- lean/src/Main.lean
import Statements
import Supplements.Kernels.MainTheoremsAll

theorem riemannHypothesis : RiemannHypothesis := rhOfAll
theorem riemannZeta_eq_zero_re : RiemannZetaZerosOnLine := rhOfAll
theorem LFunction_ne_zero_of_half_lt_re : DirichletLNonvanishingRightOfHalf := allChi
theorem LFunction_eq_zero_re : DirichletLZerosOnLine := claimAll.2.1
theorem LFunction_eq_zero_re_of_isPrimitive : PrimitiveDirichletLZerosOnLine := critAll
```

`RiemannHypothesis` è la definizione di Mathlib (`Mathlib.NumberTheory.LSeries.RiemannZeta`). Le altre quattro sono in `Statements.lean` e vanno lette con attenzione, perché sono il punto in cui un enunciato sbagliato potrebbe rendere la prova banale.

## Struttura per libreria

| Libreria | File | Righe | Quota |
|---|---:|---:|---:|
| `Development` | 141.928 | 12.232.663 | 70,4% |
| `Formalism` | 18.254 | 3.200.748 | 18,4% |
| `GroundTheory` | 8.507 | 1.119.138 | 6,4% |
| `ReciprocityTheory` | 3.845 | 563.016 | 3,2% |
| `CompanionTheory` | 2.027 | 194.218 | 1,1% |
| `PositivityTheory` | 449 | 55.130 | 0,3% |
| `Lemmata` | 167 | 9.232 | 0,1% |
| `Supplements` | 3 | 22 | — |
| `BuildAid` | 5 | 20 | — |

`Supplements/Kernels/MainTheoremsAll` è il punto di ingresso da cui partire per scendere nella prova.

## Toolchain

- Lean `v4.33.1` (`lean/lean-toolchain`)
- Mathlib al commit `0df444a360eaa60ab8c11dca51a86af692955474`
- `maxHeartbeats = 4000000`, `--tstack=400000`, `autoImplicit = false`

## Come compilarlo

Una compilazione completa richiede circa 13 ore su 32 core, 128 GB di RAM e 150 GB di disco (dati di hhh2210). Su questa macchina non è realistica. Le alternative:

1. **Senza compilare**: [homeowmorphism/riemann-hypothesis-original](https://github.com/homeowmorphism/riemann-hypothesis-original) offre una cache precompilata della release originale. Così si apre il progetto nell'editor e si naviga con l'LSP, ma Lean non ricontrolla le prove.
2. **Versione ripulita**: [leanprover-community/riemann-hypothesis](https://github.com/leanprover-community/riemann-hypothesis) (`lake exe cache get`). Il branch `original` contiene la release originale.
3. **Verifica completa**: `lake comparator` in sandbox, su una macchina grande. Vedi il README della comunità Lean.

## Altri repository utili

| Repository | A cosa serve |
|---|---|
| [leanprover-community/riemann-hypothesis](https://github.com/leanprover-community/riemann-hypothesis) | pulizia ufficiale della comunità, CI, comparator |
| [hhh2210/riemann-lean-account](https://github.com/hhh2210/riemann-lean-account) | esposizione dell'argomento ricavata dal codice, cono di dipendenze, replay del kernel |
| [tamnd/riemann](https://github.com/tamnd/riemann) e [riemann.tamnd.com](https://riemann.tamnd.com) | 12 capitoli in prosa sulla parte alta della prova, report di fiducia |
| [elcapo/levent-alpoge-lean-rh-formalization](https://github.com/elcapo/levent-alpoge-lean-rh-formalization) | copia strutturata per l'analisi |
