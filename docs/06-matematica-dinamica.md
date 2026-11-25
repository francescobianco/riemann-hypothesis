# Matematica dinamica e Gödel

Domanda di partenza: se macchine che imparano producono matematica "a passi", in cui ogni stadio contiene il precedente, il teorema di Gödel viene superato?

Tesi di questa nota:

- La matematica dinamica è **compatibile** con Gödel, non lo smentisce. Il processo per stadi l'ha previsto Gödel stesso, e Turing e Feferman l'hanno studiato a fondo.
- Il processo supera ogni sistema fissato solo a una di due condizioni. O le sue scelte **non sono calcolabili**, o rinuncia alla **certezza** (apprendimento al limite).
- La questione aperta e interessante è a quale dei due casi appartengono le reti che imparano.

## 1. La tesi di partenza, formulata con cura

> Gödel ragiona su un sistema *statico*: la matematica al passo 1 non è completa. Ma la matematica al passo 2 contiene quella al passo 1 e dimostra ciò che al passo 1 era indimostrabile. Un sistema che si muove da sé (un "automotore") rende la matematica un processo dinamico. In questo campo delle costruzioni non esistono enunciati indimostrabili: ognuno diventa dimostrabile a qualche passo.

C'è un nucleo vero: ogni singolo enunciato indecidibile viene superato da uno stadio successivo. Il punto da esaminare è il passaggio da "ogni enunciato a qualche passo" a "nessun limite per il processo".

## 2. Che cosa dice davvero Gödel

**Primo teorema (1931).** Prendiamo un sistema formale T con tre proprietà:

- è **effettivamente assiomatizzato**: un algoritmo elenca i suoi assiomi;
- è **coerente**;
- contiene un minimo di aritmetica.

Allora esiste un enunciato aritmetico G_T vero ma non dimostrabile in T.

**Secondo teorema.** T non dimostra la propria coerenza, Con(T).

Gödel non dice che esistono verità indimostrabili *in assoluto*. Dice che **nessun sistema fissato, generato da un algoritmo**, le dimostra tutte. L'indimostrabilità è sempre relativa a un sistema.

## 3. Il passo dinamico: progressioni di teorie

L'idea del "passo due" è classica:

```
T₀ = PA
T₁ = T₀ + Con(T₀)
T₂ = T₁ + Con(T₁)
…
T_ω = unione dei Tₙ
T_{ω+1} = T_ω + Con(T_ω)
…   (si prosegue lungo gli ordinali)
```

Ogni stadio dimostra ciò che il precedente non dimostrava, la sua coerenza.

| Risultato | Che cosa mostra |
|---|---|
| Turing, *Systems of Logic Based on Ordinals* (1939) | esiste un cammino della progressione per coerenza che dimostra **tutti** gli enunciati veri della forma "per ogni n, P(n)" con P calcolabile (Π⁰₁) |
| Feferman, *Transfinite recursive progressions of axiomatic theories* (1962) | con il principio di riflessione uniforme, un cammino opportuno dimostra **tutta** l'aritmetica vera |

Fin qui la tua intuizione è confermata: esiste un processo per stadi che arriva a tutto.

## 4. Perché, se il processo è un algoritmo, ricade sotto Gödel

Il prezzo sta in una clausola dei teoremi di Turing e Feferman: *un cammino opportuno*. Gli ordinali si raggiungono tramite notazioni, e la scelta della notazione "giusta" a ogni stadio non è calcolabile. Scegliere un cammino completo ha la complessità di un problema Π¹₁-completo, ben oltre qualsiasi algoritmo.

Supponiamo invece che sia una macchina, cioè un programma, a generare gli stadi T₀, T₁, T₂, … In quel caso l'unione di tutti gli stadi è a sua volta un sistema effettivamente assiomatizzato. Il suo elenco di assiomi è il programma stesso. Gödel si applica a quell'unione: esiste un enunciato vero che **nessuno stadio** del processo dimostra.

Visto dall'esterno, un processo dinamico generato da un algoritmo è un sistema statico più grande. Il dinamismo non basta. Serve che le scelte del processo vengano da fuori dell'algoritmo.

## 5. Il problema della scelta: quale estensione è vera?

A ogni stadio ci sono più estensioni coerenti. Se G è indecidibile in T, allora sia T + G sia T + ¬G sono coerenti. Il caso più netto è questo:

- T + Con(T) è coerente e **corretto**;
- T + ¬Con(T) è anch'esso coerente, per il secondo teorema, ma **dimostra un falso**: afferma che esiste una prova di contraddizione, che non esiste.

Una macchina che estende i propri assiomi non può distinguere in generale un'estensione corretta da una solo coerente. Già la coerenza non è decidibile. Il **criterio di verità** che guida il processo deve venire da qualche altra parte: intuizione, interazione con il mondo, dati. Altrimenti il processo resta coerente ma può derivare verso il falso.

## 6. La versione che regge: apprendimento al limite

Esiste un modello di "matematica dinamica" che supera davvero i sistemi statici, a un prezzo preciso.

**Apprendimento al limite** (Gold, *Limiting recursion*, 1965; Putnam, *Trial and error predicates*, 1965). Una macchina riceve una domanda e produce una successione di risposte provvisorie, che può correggere. Decide la domanda se, da un certo momento in poi, la risposta non cambia più ed è giusta. Non è richiesto che sappia *quando* è arrivata.

| | Sistema statico | Apprendimento al limite |
|---|---|---|
| cosa decide | insiemi calcolabili | insiemi Δ⁰₂: calcolabili con l'oracolo dell'arresto (lemma del limite di Shoenfield, 1959) |
| problema dell'arresto | indecidibile | decidibile al limite |
| certezza | ogni risposta è definitiva | non si sa mai quando la risposta è definitiva |
| natura | dimostrazione | congettura rivedibile |

**L'esempio di RH.** RH equivale a un enunciato Π⁰₁. Una macchina banale la "decide" al limite: risponde "vera" e controlla un caso dopo l'altro. Se trova un controesempio cambia risposta per sempre. Se RH è vera, la macchina ha avuto ragione fin dall'inizio, ma non avrebbe mai potuto saperlo. Questo è esattamente il divario tra *credere al limite* e *dimostrare*.

**Anche qui c'è un limite.** L'aritmetica vera nel suo complesso non è Δ⁰₂: si estende su tutta la gerarchia aritmetica. Un solo livello di revisione non basta. Ci vogliono revisioni annidate, cioè ancora più rinuncia alla certezza.

## 7. Le reti che imparano sono procedure fissate?

È il punto in cui la tua domanda diventa davvero aperta.

- **Rete addestrata e congelata.** È una funzione calcolabile. Insieme al verificatore Lean costituisce un sistema effettivo, e Gödel si applica.
- **Addestramento come processo.** I pesi cambiano in base ai dati. Se i dati vengono da un programma, l'intero sistema è ancora un algoritmo. Se vengono dal mondo (esperimenti, matematici umani, altre macchine), il sistema è **aperto**. Gödel parla di sistemi chiusi, e non si applica direttamente a sistemi aperti. Resta discusso se un sistema aperto possa fare qualcosa che un algoritmo con un input non può.
- **Il bivio di Gödel** (Gibbs Lecture, 1951). O la mente umana supera ogni macchina finita, o esistono problemi aritmetici assolutamente indecidibili per la mente umana. Gödel considerava aperta la scelta. Gli argomenti di Lucas (1961) e Penrose (1989, 1994) sostengono il primo corno. La critica standard è che nemmeno un essere umano può conoscere con certezza la propria coerenza, e quindi l'argomento colpisce anche noi.

Una rete che continua a imparare dal mondo riapre questo bivio dalla parte della macchina. Non è una smentita di Gödel. È la domanda se "macchina" debba voler dire "macchina di Turing chiusa".

## 8. Il collegamento con la prova di Alpöge

La prova di Alpöge e Claude **non** esce dal quadro di Gödel. È una prova in un sistema fissato: Lean, Mathlib e tre assiomi. Inoltre RH, essendo Π⁰₁, non era un enunciato "gödeliano" per costruzione. Il suo stato era semplicemente sconosciuto.

Ciò che è cambiato è un'altra cosa. La distinzione classica di Reichenbach separa il **contesto della scoperta** dal **contesto della giustificazione**:

| | Scoperta | Giustificazione |
|---|---|---|
| chi la fa | Alpöge + Claude, per tentativi, in sei settimane | il kernel di Lean |
| natura | dinamica, rivedibile, guidata da analogie (Stepanov, campi di funzioni) | statica, meccanica, definitiva |
| cosa cambia con le macchine | molto: velocità, ampiezza, 17 milioni di righe | quasi nulla: è lo stesso controllo dei tipi |

La "matematica dinamica" che hai in mente è propriamente la **teoria del processo di scoperta**. È lo studio delle costruzioni in quanto *processi*, non in quanto *oggetti* (vedi [05-cosa-e-una-dimostrazione.md](05-cosa-e-una-dimostrazione.md)). Non sostituisce la dimostrazione: la produce.

## 9. Tesi riformulata

1. Ogni enunciato vero è dimostrabile in qualche stadio di un processo di estensione. È vero, ma in modo banale se le scelte sono libere.
2. Se il processo è un algoritmo chiuso, esiste un enunciato vero che nessuno stadio dimostra. Questo è Gödel, e resta valido.
3. Un processo dinamico supera ogni sistema fissato solo in due modi. O è guidato da una fonte non calcolabile, che può essere il mondo, l'esperienza, l'interazione. Oppure accetta di essere fallibile, come l'apprendimento al limite.
4. Le macchine che imparano dal mondo sono il primo candidato concreto per il caso 3. Se lo siano davvero è una domanda aperta, e non di sola logica.

## Domande aperte

- Un sistema di addestramento con dati dal mondo è formalmente un algoritmo con un input (quindi coperto dalla teoria della calcolabilità relativa) o qualcosa di diverso?
- Si può definire una nozione di "dimostrazione dinamica" che conservi abbastanza certezza da essere utile? Per esempio una prova al limite con controllo statico a ogni stadio.
- Gli automotori come oggetto matematico: quali strutture descrivono un processo che costruisce costruzioni? Candidati: teoria dei giochi, semantica operazionale, coalgebre. Da approfondire.

## Riferimenti

- K. Gödel, *Über formal unentscheidbare Sätze der Principia Mathematica und verwandter Systeme I*, 1931
- A. M. Turing, *Systems of Logic Based on Ordinals*, Proc. London Math. Soc., 1939
- S. Feferman, *Transfinite recursive progressions of axiomatic theories*, J. Symbolic Logic, 1962
- J. R. Shoenfield, *On degrees of unsolvability*, Annals of Math., 1959 (lemma del limite)
- E. M. Gold, *Limiting recursion*, J. Symbolic Logic, 1965
- H. Putnam, *Trial and error predicates and the solution to a problem of Mostowski*, J. Symbolic Logic, 1965
- K. Gödel, *Some basic theorems on the foundations of mathematics and their implications* (Gibbs Lecture, 1951), in *Collected Works III*
- J. R. Lucas, *Minds, Machines and Gödel*, Philosophy, 1961
- R. Penrose, *The Emperor's New Mind* (1989), *Shadows of the Mind* (1994)
- H. Reichenbach, *Experience and Prediction*, 1938
