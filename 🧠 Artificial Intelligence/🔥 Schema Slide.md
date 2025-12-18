---
aliases:
tags:
  - schema
dg-publish: true
---
# 1 Agents

> È qualunque cosa che può interagire

Possiede:

- **Sensori** → per percepire l'ambiente
- **Attuatori** → per effettuare azioni nell'ambiente

$f:P^{*}\to A$

Dove:

- $P^{*}$ → è la storia delle percezioni
- $A$ → è l'insieme delle possibili azioni di un agent 

![[allegati/Image.png|711x394]]

+ *The First Agent*
    semplice senza valutare nessun tipo di valutazione, completa l'obbiettivo senza considerare il tempo o i cicli. Non considera i cambiamenti intermedi

```cpp
function Reflex-Vacuum-Agent( [location,status]) returns an action
  if status = Dirty then return Suck
  else if location = A then return Right
  else if location = B then return Left
```

**Rational Agents** → è un agente che sceglie le proprie azioni allo scopo di massimizzare le metriche i performance, dato lo storico delle percezioni ^RationalAgent

- Non è onnisciente
- Non prevede il futuro
- Non ha sempre successo

*AMBIENTE STATICO* → ossia completamente osservabile e prevedibile (azioni deterministiche con regole chiare) incoraggiando ad imparare comportamenti fissi.

- La soluzione ottima non cambia, stesso obbiettivo
- Soluzione fragile
    - *Esempio Vespa*: che come c'è un cambiamento riparte dall'azione che scaturisce la percezione quindi spreco di cicli inutile

## 1.1 PEAS

***P***erformance metric

***E***nviroment

***A***ctuators

***S***ensors

Più è ristretto l'ambiente → più l'agente sarà semplice da costruire

- Agent Architecture = $A + S$
- Agent = Architecture + Function
+ Example Taxi

Performance metric: safety, destination, profits, legality, comfort, ....
Environment: streets/freeways, traffic, pedestrians, weather, ...
Actuators: steering, accelerator, brake, horn, speaker/display, …
Sensors: camera, accelerometers, gauges, engine sensors, keyboard, GPS…

## 1.2 Enviroments - Ambienti

![[allegati/Image (2).png]]

> **Single-Agent** → ossia che nell'ambiente agisce un solo agente e non deve competere con altri

> Real Agent → è come il taxi non comprende nessuna delle caratteristiche, il che lo rende molto complicato da realizzare

- **Table-Driven Agent** → ossia prende decisioni consultando una **tabella di lookup** in cui, per ogni possibile percezione (o sequenza di percezioni), è già memorizzata l’azione da compiere.
    - Complessità alta per la creazione della tabella
- **Reflex Agent** → insieme di regole condizionali dirette “condizione → azione” ^ReflexAgent

![[Image (3).png|755x455]]

***Effectiveness-Efficiency Trade-off***

| **TA**                                     | **SRA**                                                            |
| ------------------------------------------ | ------------------------------------------------------------------ |
| Alti costi per gestire e creare la tabella | Riduce i costi creando solo delle condizioni di attivazione        |
| Scala esponenzialmente il tempo → $O(n^2)$ | Scala linearmente il tempo → $O(n)$                                |
|                                            | Semplifica un approccio già base e stupido → osservabile e piccolo |

## 1.3 Tipi di Agenti

### 1.3.1 Model-Based Agent

^5938be

![[Image (4).png|748x383]]

Aggiunge → *STATE* al [[#^ReflexAgent]

- Stima lo stato attuale dell'ambiente basandosi sullo stato interno dell'agente
    - Lo stato interno dipende dallo storico delle percezioni
- Lo stato interno serve per catturare il modello delle transizioni dell'ambiente
    - Conseguenze dell'azione dell'agente
    - Dinamiche dell'ambiente
- *Sensor Model* → come i sensori dell'agente rappresentano il mondo - rumore e limitazioni
    - Approssima l'intero stato → efficace in ambianti parzialmente osservabili

### 1.3.2 Goal-Based Agent

![[Image (5).png|702x430]]

❓Massimizzare la ricompensa immediata vs massimizzare la ricompensa a lungo termine

- **Search** → come è meglio navigare tra le possibili soluzioni? Decision Tree
- **Planning** → sfruttare la conoscenza del mondo per eseguire le migliori azioni

> I modelli goal-based rappresentano il loro obbiettivo (*goal*) e possono modificarlo

### 1.3.3 Utility-Based Agent

![[Image (6).png|727x448]]

> Invece di utilizzare un solo stato, assegna un punteggio ad ogni stato interno che indica quanto è buono

- Permette di analizzare varie traiettorie e quindi tenere in considerazione non solo se arrivo al traguardo ma anche come ci arrivo → permette di selezionare la soluzione migliore tra quelle esistenti risparmiando sulle mosse da fare per esempio

È fondamentale quando:

- ci sono stati finali **non tutti equivalenti** (es. vincere con 10 mosse vs con 100 mosse),
- l’ambiente è **stocastico** (es. backgammon),
- ci sono più obiettivi in conflitto (es. “arrivare a destinazione **velocemente ma in sicurezza**”)

### 1.3.4 Learning-Based Agent

![[Image (7).png|782x329]]

Tutti i modelli di agenti possono essere learning-based o no.

L'apprendimento può modificare qualche componente.

- **Elementi prestazionali** → i precedenti agenti ricevevano il contesto e sceglievano l'azione da effettuare
- **Critic** → guida l'apprendimento tramite la valutazione del comportamento attuale con la metrica delle performance esterne
- **Problem** → allontanarsi dal comportamento greedy provando nuove azioni (greedy = sceglie l'opzione migliore senza considerare altre soluzioni) 

## 1.4 Summary

- **Agenti** interagiscono con **l'ambiente** tramite **attuatori** e **sensori**
- La **funzione** degli agenti →  [Agent = Architecture + Function](craftdocs://open?blockId=4F5E2847-6565-43CC-A12A-EB81130E4DFF&spaceId=4b28628f-7dfd-54ce-27f9-28712867f81f) → descrive cosa fa l'agente in tutte le circostanze
- La **misurazione delle prestazioni** valuta la sequenza dell'ambiente
- Un [[#^RationalAgent]]
- [PEAS](./🔥 Schema Slide.md#peas) → descrive gli ambienti di attività
- [Enviroments - Ambienti](./🔥 Schema Slide.md#enviroments--ambienti) → Osservabile, Deterministico, Episodico, Statico, Discreto
- Molti tipi di Agenti
    - [[#^ReflexAgent]]
    - [[#1.3.1 Model-Based Agent]]
    - [[#1.3.2 Goal-Based Agent]]]
    - [[#1.3.2 Goal-Based Agent]]

---

# 2 Search

> Search Alorithm → dato un problema, ritorna una sequenza di azioni, che formano un path verso lo stato *goal* o *fail*

**Open-Loop** - *offline* → un agente cerca la sequenza ottimale di azioni e successivamente la esegue
- *Completamente osservabile*
- *Deterministico* → risposte dell'ambiente conosciute

**Closed-Loop** - *online* → l'agente valuta continuamente i feedback provenienti dall'ambiente
- *parzialmente osservabile*
- *Stocastico* → le risposte variano
	- Devo controllare il mondo per sapere dove sono, in quale stato

![[Image (8).png|752x448]]

> **Scelte di Design → Guida veloce**

- **Deterministica e completamente osservabile** → problema a stato singolo
    - l'agente sa esattamente in quel stato si troverà → la soluzione è una sequenza di stati o azioni
- > **Non osservabile** → problema conforme senza sensore
    - L'agente non percepisce lo stato → soluzione è una sequenza di azioni
- *Non deterministico e parzialmente osservabile** → problema di contingenza
    - le percezioni prendono nuove informazioni riguardo allo stato
    - la soluzione è un piano contingente o una policy
    - spesso ricerca interlacciata
- **Spazio degli Stati Sconosciuto** → problema di esplorazione (online - closed-loop)
    - l'agente deve scoprire lo spazio degli stati

![[Image (9).png|741x278]]

![[Image (10).png|738x361]]

## 2.1 Tree Search

> Inizia dagli stati già visitati e gli espande → genera gli stati successivi

- > **Frontiera** → insieme di nodi non espansi che separano i nodi espansi da quelli non raggiunti
- > **Nodi Raggiunti** → *frontiera* + *nodi espansi*
- > Ricerca **Uniforme**

```cpp
function Tree-Search(problem, strategy) return a solution or failure
  inizializza con initial state del problema
  loop do
    if non ci sono candidati return failure
    scegli un nodo da espandere in base alla strategia
    if nodo contine goal state return soluzione
    else espandi il nodo e aggiungi il nodo alla ricerca ad albero
  end
```

![[Image (11).png]]

![[Image (12).png]]

- **State** → uno stato è una rappresentazione di una configurazione fisica
- **Nodo** → è una struttura dati che costituisce una parte della ricerca ad albero
    - include genitori, figli. profondità e costo dei path (cammini)
- **La funzione di Espansione**
    - Crea nuovi nodi
    - Riempie i vari campi
    - Usa la funzione *Successore* del problema per creare gli stati corrispondenti

> La ricerca ad albero non corrisponde all'intero spazio degli stati → un insieme di satti finito può portare ad una ricerca infinita (loops, path ridondanti)

> Nodi nella ricerca ad albero = stati dello spazio

Gli stati sono tutti diversi nello spazio

I nodi non sono unici in un albero di ricerca → se abbiamo un nodo che ritorna ad un altro già esplorato devo comunque selezionarlo

### 2.1.1 Scegliere una Strategia di Ricerca

> Una strategia di ricerca è definita **in base all'ordine con cui i nodi vengono espansi**

Una **strategia** è **valutata** tramite queste dimensioni:

- **Completezza** → trova sempre una soluzione
- **Complessità di Tempo** → numero di nodi generati o espansi
- **Complessità dello Spazio** → numero massimo di nodi in memoria
- **Ottimale** → trova sempre la soluzione con meno costo

Complessità di Tempo e Spazio sono misurati in:

- $b$ → fattore massimo di ramificazione dell'albero di ricerca (quanti nodi espando)
- $d$ → profondità della soluzione migliore (minor costo)
- $m$ → massima profondità dello spazio degli stati (può essere $\infty$)

## 2.2 Uninformed Tree Search - Non Informata

> Una ricerca non informata usa solo le informazioni disponibili quando definiamo il problema, non sappiamo quanto siamo lontani dal goal (obbiettivo)

- Breadth-first search
- Uniform-cost search
- Depth-first search
- Depth-limited search
- Iterative deepening search

### 2.2.1 Breadth-First Search
- Frontiera → è una coda FIFO
- Espande prima in larghezza poi in profondità

![[Image (13).png]]

![[Image (14).png]]

| **Completo**    | **Time**                                         | **Space**                      | **Optimal**                                       |
| --------------- | ------------------------------------------------ | ------------------------------ | ------------------------------------------------- |
| **Si**          | $\mathbf{O}(b^d)$                                | $\mathbf{O}(b^d)$              | **Si**                                            |
| Se $b$ è finito | Dato che fa tutti i nodi ad una certa profondità | Prende tutti i nodi in memoria | Se il costo per step è $1$ non ottimo in generale |

#### Dijkstra's Algorithm → ricerca costo uniforme

> Le azioni hanno un costo differente → espandere i nodi con il cammino di costo minimo

> La frontiera è una coda pesata → i path con costo minore vanno per primi

| **Completo**                                          | **Time**                                                                                                                | **Space**                | **Optimal**                                                |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | ------------------------ | ---------------------------------------------------------- |
| **Si**                                                | $\mathbf{O}(b^{1+[\frac{C^*}{\eta}]})$                                                                                  | $\mathbf{O}(b^d)$        | **Si**                                                     |
| Se il costo minimo delle azioni è positivo ossia $>0$ | Dove $C^*$ è il costo del path ottimale → può eplorare percorsi a basso costo che non servono a niente può essere $b^d$ | **Stesso della memoria** | I nodi sono espansi in ordine in base al costo del cammino |

### 2.2.2 Depth-First Search

> Espande prima i nodi in profondità

> Coda → LIFO → mette i nodi in cima (Ultimo ad entrare primo ad uscire Last In - First Out)

![[Image (15).png]]

![[Image (16).png]]

| **Completo**                 | **Time**                                                                       | **Space**        | **Optimal** |
| ---------------------------- | ------------------------------------------------------------------------------ | ---------------- | ----------- |
| **Si**                       | $\mathbf{O}(b^m)$                                                              | $\mathbf{O}(bm)$ | No          |
| Se $b$ è finito e senza loop | Dato che vado prima in profondità esploro prima tutti i fligli fino al massimo | **Lineare**      |             |

> DFS può essere implementato in vari modi:

- > **Depth-Limited search** → con profondità di esplorazione fissata
- > **Iterative Deeping Search** → iterazioni con valore di profondità che aumenta
- > **Bidirectional Search** → iniziamo la ricerca dallo stato iniziale e dallo stato obbiettivo verso lo stato iniziale

#### Come Riconoscere uno Stato Ripetuto?

- **Ricordare gli stati raggiunti**
    - Posso riconoscere e conservare i path di costo minore
    - Se ho spazio
- Non controllo se il problema non produce path ridondanti
- **Controlla solo i loop**
    - Conserva i genitori per ogni nodo
    - Segue la catena dei parenti per vedere se ricade in un loop
    - Posso seguire la catena fino al root → puntatori al padre
    - **Non c'è bisogno di memoria in più** solo **più tempo per controllare la catena**

### 2.2.3 Sommario → Uninformed Search

![[Image (17).png]]

## 2.3 Heauristic - Informed Search

> Espandere i nodi più promettenti

> Promettenti è una valutazione euristica → propone una stima del minor costo da uno stato al nodo $n$ fino al goal

> Frontiera → è una coda pesata e ordinata in base al valore euristico

Best-First Search Algorithm:

- Greedy Search
- A* Search

### 2.3.1 Greedy Search

> Seleziono ad ogni nodo quello con il costo euristico minore

- Non ha meccanismi per trovare il cammino migliore guardando i successivi
- Potrebbe portare a cammini di costo maggiore anche se il primo passo è più conveniente

| **Completo**                                                                                                | **Time**                                       | **Space**                     | **Optimal** |
| ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | ----------------------------- | ----------- |
| No                                                                                                          | $\mathbf{O}(b^m)$                              | $\mathbf{O}(b^m)$             | No          |
| Può rimanere incastrato in un loop.<br/>Completo in uno spazio finito con il controllo degli stati ripetuti | Una buona euristca può portare a miglioramenti | Tiene tutti i nodi in memoria |             |

### 2.3.2 A* Search

> Idea → evitare di espandere i cammini già costosi

> Controllo se un cammino è più costoso di un altro cammino che ho già esplorato se si allora continuo il cammino con il costo minore e così via

> In questo modo tengo in considerazione il cammino di costo minimo generale, in modo da poter scegliere quello minore analizzando anche cammini alternativi

**Funzione di Valutazione** → $f(n)=g(n)+h(n)$

- $g(n)$ = costo fino a questo momento per raggiungere $n$ 
- $h(n)$ = costo stimato al goal da $n$ → euristica greedy
- $f(n)$ = costo stimato del path migliore passando attraverso $n$ per raggiungere il goal
- Questa funzione valuta ad ogni nodo il costo decidendo come proseguire

**Complessità di Spazio** → la complessità scala con il numero di nodi espanso

- $A*$ espande tutti i nodi con $f(n)<C^*$
- $A*$ espande qualche nodo con $f(n)=C^*$
- $A*$ non espande nodi con $f(n)>C^*$
- Con $C^*$ costo del path ottimale 

**Complessità di Tempo** → è esponenziale nell'errore di $h$

A* utilizza un euristica *ammissibile*

- $h(n)\le h^*(n)$ dove $h^*(n)$ è il vero costo da $n$
- $h(n)\ge 0$ tale che $h(G)=0$ per ogni goal $G$
- Un euristica ammissibile non sovrastima mai la distanza dal goal

***A** è completa**

***A$^*$* è ottima se *h* è ammissibile**

> **Euristica Consistente** → $h(n)\le c(n,a,n \prime)+h(n \prime),\ \forall n\prime$

- Più forte dell'ammissibilità → il costo dei cammini è monotona crescente
    - Ossia il valore di $f(n)$ non scende mai lungo il cammino
- Quando raggiungi uno stato è già sulla la strada ottimale per arrivare ad $n$
    - Se l'euristica fosse solo ammissibile invece potrei estrarre un nodo con un cammino sub-ottimale e doverlo riconsiderare in seguito

![[Image (18).png]]

### 2.3.3 Problema
> A* visita troppi nodi → richiede che vengano esplorati ed espansi molti nodi
> → **A* Pesato**: sub-optimal ma abbastanza buono
> $f(n)=g(n)+W\ h(n),\ \ W>1$
> Trova una soluzione con costo tra $C^*$ e $W\ C^*$
> -> Spesso vicina a $C^*$

#### Contorni con A*

![[Image (19).png|714x431]]

![[Image (20).png]]

![[Image (21).png]]

#### Beam Search
La **beam search** è un algoritmo di ricerca basato su **euristiche**  che esplora un grafo espandendo il nodo più promettente in un insieme limitato di nodi.
La beam search  simile a [[#2.2.1 Breadth-First Search]] limitata. che mira a ridurre i requisiti di memoria. Nella beam search è tenuto in considerazione solo un numero predefinito di migliori soluzioni parziali. Di conseguenza, è considerato un algoritmo greedy.
- L’euristica non è necessariamente ammissibile né ottimale
- **Obiettivo tipico**: generare sequenze plausibili in problemi di decoding
- Mantiene i **B migliori cammini parziali** in un albero di ricerca.
- Ad ogni passo: espande ognuno, valuta i successori, sceglie i B migliori, scarta gli altri.
- Non garantisce la soluzione ottimale

---
# 3 Local Search
Cosa vogliamo:
- Veloce
- Economico
- Abbastanza buono
- Ricerca non sistematica
	- Non ci interessa come ci arriviamo all'obbiettivo ma vogliamo arrivarci

> Local search restituisce una soluzione non un path
> - Non salva gli stati visitati
> - Non esplora l'intero spazio degli stati

## 3.1 Hill Climbing
> Hill-climbing può rimanere bloccato
	- Ottimo locale
	- Greedy Local Search


> [!NOTE] Gradient Descent
> Se uso il gradiente per valutare e guidare la funzione di ricerca otteniamo la discesa del gradiente.

- si blocca su i massimi locali
- Si blocca su parti pari (flat)
### 3.1.1 Soluzioni
**Random Restart**
- Riesce a scapapre dall'ottimo locale
- Completo ma molto lento
**Sideway Move** -> *movimento laterale*
- Riesce ad uscire dalle parti flat se non sono il massimo
- Fisso -> Si sposta di massimo $k$ step
![[image-1.png|525x302]]

```c title="Hill Climbing"
function Hill-Climbing(problem) returns a state that is a local maximum
    inputs: problem, a problem definition
    local variables: current, a node, neighbor, a node
    
    current ← Make-Node(Initial-State(problem))
    loop do
        neighbor ← a highest-valued successor of current
        if Value(neighbor) ≤ Value(current)
        then return State(current)
        current ← neighbor
end
```

## 3.2 Simulated Annealing
> Uscire dall'ottimo locale facendo un passo random
> - un passo random non rimane mai incastrato -> ma è lento


> [!important] Simulated Annealing
> Corrisponde a -> *hill climbing $+$ random moves*
> - I movimenti random diventano meno frequenti nel tempo
> - I movimenti casuali migliorano con il tempo
> - **Temperature** -> è un parametro che controlla il trade-off tra hill-climbing e movimenti random
> 	- La temperatura cambia con il tempo permettendo all'inizio di fare molti passi casuali e mano a mano che vado avanti i passi casuali diminuiscono
> 	- Alla fine $T=0$ -> nessun passo casuale


### 3.2.1 Temperature Scheduling
$$
e^{\frac{x}{T}},\quad T>0,\quad x>0
$$
- $x$ Grande -> è una mossa molto sbagliata, c'è una bassa probabilità di accettare lo spostamento
- Aumentando $T$ -> aumento la probabilità di accettare spostamenti
Se $T\to 0$ allora anneiling diventa deterministico -> non effettua mosse casuali
- Accetta solo spostamenti buoni $e^{\frac{x}{T}}\to 0$ 
**Anneiling Scheduling** -> inizia con T grande e lo diminuisce con il passare del tempo
- Se T viene diminuite abbastanza lentamente l'algoritmo converge al *massimo globale* 
	- Se invece è troppo lento visito tutti gli stati -> lentissimo
- *Diminuzione Esponenziale* -> $T_t = k\ T_{t-1},\quad 0<k<1$
- *Power-law Decrese* -> $T_t=\frac{T_0}{t^a}$ es: $a=1$
### 3.2.2 Local Beam Search
> Invece di prendere solo la prima soluzione prendo le prime $k$
> Espando tutto e seleziono i top-$k$ stati
> - Ha senso quando la funzione non converge sicuramente


> [!important] Funzionamento
> È la versione stocastica della [[#Beam Search]]
> Gli stati interagiscono con gli altri -> abbandono i path con meno importanza, concentrandomi su quelli più promettenti
> - Non è una ricerca parallela su $k$ path
> - *Local beam -> parte da k stati, tiene i migliori k successori globalmente.*
> - **Non garantisce ottimalità**

#### Beam search decoder in NLP

NLP -> Natural Language Processing
Tutto ciò che concerne l'elaborazione del linguaggio naturale -> nell'esempio la traduzione da inglese ad italiano tramite encoder (trasforma la frase in vettori-embedding) e deconder con vari livelli di linar model e softmax per avere l'output
*Esempio:*
- *Input*: how are you?
- *Output*: come stai?
![[image-2.png]]
##### 3.3 Beam Search vs Greedy Search in NLP
**Greedy Search** -> semplicemente prende la parola più simile per ogni posizione
- Ogni parola è considerata indipendente
- Funziona ma può essere migliorato
**Beam Search**
- Trova il path migliore e tiene in considerazione cosa ha già selezionato
- Prende $N$ migliori candidati per ogni posizione
- Espande gli attuali $N$ migliori candidati e prende i migliori $N$
- Nell'ultima posizione prende il miglior risultato tra gli $N$ candidati finali
##### Beam Search Decoder in NLP
Bisogna eseguire il decoder $N$ volte per ogni posizione:
- Costa tanto
- Ma il risultato spesso è il migliore
![[image-3.png]]
## 3.3 Ambiente Non Deterministico

> Alcune azione hanno un risultato non deterministico

Belief State (Stato di Fede) -> un insieme di stati futuri per ogni stato corrente e coppia di azioni
- Non sai in anticipo dove ti porta l'azione
Conditional Plan -> la solzuione è un albero (if-then-else chain) non una sequenza
- Puoi eseguire la soluzione e in base al risultato dell'ambiente scegliere l'azione corretta
- **AND-OR Search Trees** -> è la soluzione per il conditional plan
	- **AND node** -> Fornito dall'ambiente insieme al set di tutti i possibili stati successivi
	- **OR node** -> Fornito dall'agente insieme all'azione da eseguire
### 3.3.1 Esempio Erratic Vacuum World
Non deterministic action
- Se lo stato corrente è sporco, "suck" pulisce il quadrato corrispondente ma avvolte pulisce anche il quadrato adiacente
- Se lo stato corrente è pulito, "suck" può depositare la sporcizia

"Left" e "Right" sono deterministiche

**Belief state**
- $Result(s,a)=?$
**Conditional plan**
- «suck»; `if 5 do [«right», «suck»]`
- Starting from state 1
- $Result(1, «suck»)= \{5, 7\}$
> In questo modo se "suck" pulisce entrambi io avrò che `if 5` non sarà rispettato quindi faccio un altra azione
### 3.3.2 AND-OR Search Tree
- Necessario per evitare loop
- L'algoritmo è leggermente più coprente ma l'albero si espande ancora
- Esiste una versione di $A^*$ per l'albero AND-OR

**Solzuione**
Un sotto-albero dove:
-  ogni foglia è un goal
- Ha un azione per ogni nodo OR
- Include un ramo per tutti i risultati dei nodi AND
![[image-4.png|465x412]]

## 3.4 Searching Senza Osservazioni
- Non ci sono percezioni
- Senza sensori -> *Conformant Problem*

> L'agente deve stimale lo stato
> **Soluzione** -> è una sequenza di azioni
> - Non può essere un conditional plan se non puoi ricevere un feedback dall'ambiente (non posso fare `if`)

**Belief Space** -> lo spazio delle rappresentazioni interne di ciò che credo sia vero sul mondo (non certe)

Le azioni possono portare informazioni su possibili stati futuri
- *belief state* $= \{1,2,3,4,5,6,7,8\}$, *action*$=«right»$, *next belief state* $= ?$
- $\{2, 4, 6, 8\}$
#### Search in Conformant Problem
> Idea -> cercare nel belief space (completamente osservabile) invece che nello spazio degli stati (non osservabile)

- Belief state space = powerset of states -> 2𝑛 possible beliefs instead of N
- Initial belief state = all the N states
- Le azioni *Actions(B)* che posso fare sono limitate dall'unione e l'intersezione di tutte le azioni *Actions(S)* pr ogni $S$ in $B$
- Transition model (for deterministic actions): Quando esegui un’azione, non vai in un singolo stato, ma in un **nuovo belief state**.
- **Goal Test**
	- **Possibly achieve** il goal: se almeno uno degli stati nel belief è un goal.
	- **Necessarily achieve** il goal: se _tutti_ gli stati del belief sono goal.
- **Action cost**: Se l’azione costa in modo diverso nei vari stati, allora devi trovare un criterio
	- Questo serve per decidere quanto “vale” un’azione in un belief state.

##### Check Visited States
###### Come controllare gli stati visitati?
- Esempio:
	- 𝐵1 = {5, 7}
	- 𝐵2 = {1, 3, 5, 7}
- Puoi **potare** (cioè scartare) i rami di 𝐵2, perché qualsiasi soluzione valida per $\{1, 3, 5, 7\}$ sarà anche una soluzione per $\{5, 7\}$.
👉 In generale: puoi scartare i rami che appartengono a **superset** di belief state già visitati.
###### Ragionamento inverso
- Se hai già risolto il problema per un certo belief state, allora lo hai risolto anche per qualsiasi **sottoinsieme** di esso.
###### Complessità
- Nonostante queste ottimizzazioni, la ricerca nei problemi conformant (cioè senza osservazioni, dove l’agente non sa esattamente dove si trova) rimane molto costosa, perché lo spazio dei belief state è enorme.
###### Ricerca incrementale nei belief state
1. Trova una soluzione che funziona per il **primo stato** in B.
2. Controlla se quella soluzione funziona anche per gli altri stati in B.
    - Se sì → fermati.
    - Se no → riprova.
3. **Fallire velocemente** (cioè accorgersi presto se una soluzione non funziona) può migliorare la velocità di convergenza
# 4 CSP - Problema di Soddisfazione del Vincolo
> **C**onstraint **S**atisfaction  **P**roblem

Introduciamo la rappresentazione degli stati come rappresentazione fattoriale
- Stato -> $X=\{X_1,\ X_2,\ ...,\ X_n\}$ con $n$ variabili
- Dominio -> $X_i\in D_i$ 
- La **conoscenza del dominio** è espressa con un set di **vincoli C**
	- Es. $X_1\le 0,\ \ X_2\in [1,3], ...$ 


> [!info] $CSP(X, D, C)$
> **Goal Test** -> Assegno valori alle variabili per soddisfare i vincoli
> - **Completo vs Assegnazione Parziale** -> tutte o solo una parte delle variabili hanno un valore assegnato
> - **Assegnamento Consistente** -> Assegnare variabili soddisfa le condizioni
> - **Soluzione Parziale** -> un assegnamento parziale che è consistente
> 
> Obbiettivo CSP è trovare una soluzione completa e consistente


> [!important]  Factored Representation
> State Rappresentation -> Qui rappresenti **ogni stato possibile come un’unica entità indivisibile** (un “punto” nello spazio degli stati).
>  
> Factored Representation -> Qui invece **uno stato non è rappresentato come un singolo nodo “piatto”**, ma come **una tupla di valori di variabili**:
> $$\text{Stato }S=(X_1​=v_1​,\ X_2​=v_2​,\ …,\ X_n​=v_n​)$$
> Quindi:
> - Invece di rappresentare **esplicitamente tutti gli stati**,
> - **rappresenti le variabili e i vincoli** che definiscono implicitamente lo spazio degli stati.


### 4.1.1 Esempio - Map Coloring
*Obbiettivo* -> colorare gli stati adiacenti con un colore diverso
![[image-9.png|274x226]]
Quindi abbiamo:
- $X = \{WA,\ NT,\ SA,\ Q,\ NSW,\ V,\ T\}$
- $D ={r,\ g,\ b}\ \ \forall𝑋_𝑖$
- $C =$
	- $WA ≠ 𝑁𝑇$
	- $𝑊𝐴,\ 𝑁𝑇 ∈ { 𝑟,\ 𝑔 ,\ 𝑟,\ 𝑏 ,\ 𝑔,\ 𝑟 ,\ 𝑔,\ 𝑏,\ … }$
	- Entrambi -> ripetere per ogni coppia di stati

![[image-10.png|274x242]]
> Le variabili sono i nodi e gli archi sono i vincoli
> Il grafico aiuta la ricerca, come possiamo vedere dal nodo "Tasmania" che non ha vincoli quindi possiamo assegnarli un colore casuale

| **Dominio**                    | **Vincoli**    | **Risolvibilità**                                         |
| ------------------------------ | -------------- | --------------------------------------------------------- |
| Discreto finito                | Qualsiasi      | Decidibile (es. backtracking, CSP classici)               |
| Discreto infinito              | Lineari o meno | Difficile → servono metodi speciali o bounds              |
| Discreto + vincoli lineari     | Lineari        | Solvibile (anche se NP-hard)                              |
| Discreto + vincoli non lineari | Non lineari    | **Indecidibile in generale**                              |
| Continuo + vincoli lineari     | Lineari        | **Solvibile in modo efficiente** (programmazione lineare) |
| Continuo + vincoli non lineari | Non lineari    | Più complesso; decidibilità dipende dal tipo di vincolo   |
## 4.2 Tipi di Vincoli
- **Unitario**
	- Una variabile coinvolta -> $A\ne 1$
- **Binario**
	- Due variabili coinvolte -> $A\le B$
- **Globale** -> vincoli con $n$ variabili coinvolte
	- Non necessariamente tutte le variabili
	- Alldif$(A,\ B,\ C,\ D)$
- **Vincoli di Preferenza**
	- Vincoli leggeri che rappresentano le preferenze
	- Modellato con un costo associato a ciascun incarico -> problema di ottimizzazione vincolato
	- Es. Linear Model

### 4.2.1 Vincoli Hyper-Graph
Non posso rappresentare i vincoli come archi dato che per definizione gli archi connettono solo due nodi
![[image-11.png]]
- **Nodi:** I cerchi (variabili lettera: F,T,U,W,R,O) e i quadrati (variabili di riporto: C1​,C2​,C3​).
- **Archi:** Le linee che collegano i nodi rappresentano le relazioni o i vincoli tra le variabili definite dalle equazioni.

> Per vincoli con dominio finito, puoi sempre ridurre un hyper-graph in un grafico normale
> - vincoli globali possono essere divisi in un set di vincoli binari

Qualche vincolo popolare (es. Alldiff) gode di algoritmi studiati appositamente
- Non serve ridurre il grafo a vincoli binari
- Sono più facili da leggere da una prospettiva umana

## 4.3 Come Cercare in CSP
**Gli stati sono definiti dai valori assegnati finora**
*Initial State - stato iniziale:*
- Assegnazione vuota -> $\{\}$
- Fattore di ramificazione alla radice -> $=nd$

*Azioni:*
- Assegna un valore alle variabili non assegnate che non causa un conflitto con l'assegnamento corrente
- Fattore di ramificazione al secondo livello -> $=(n-1)d$

*Ogni soluzione completa appare alla profondità $n$:*
- Problema -> $n!$ o $d^n$ foglie ([[#^6f1c56|Riduce Complessità]])
- Risoluzione -> Sfrutta la struttura del problema 

*Assegnamenti illegali* -> ritornano fallimento - non risolvibile
*Goal Test* -> controlla se l'assegnamento attuale è completo

### 4.3.1 Backtracking Search

> Fissa un ordine tra gli stati -> rimuove la complessità $n!$
> Ossia -> Non c’è bisogno di considerare tutti gli ordini delle variabili: possiamo **fissare a priori un unico ordine**, e assegnare le variabili sempre in quell’ordine.


> [!attention] Assegnazione Stati vs Singolo Nodo
> Se tu considerassi **ogni stato come una possibile sequenza parziale di assegnamenti senza un ordine fisso tra le variabili**, allora:
> - Devi esplorare **tutte le possibili permutazioni delle variabili**
> - Ci sono $n!$ possibili ordini diversi in cui potresti assegnarle.
> 
> Invece di avere **ogni nodo come “una configurazione intera” di n variabili**, ogni nodo nella ricerca corrisponde a **“assegnare un valore a una singola variabile”**, secondo l’ordine prefissato. ^6f1c56

**Ogni nodo è considerato un singolo assegnamento** -> non l'intero stato
- solamente $d^n$ foglie 
- Costruisco un albero con ogni nodo che è un assegnazione di una singola variabile non di un assegnazione di più variabili

Algoritmo non informato per CSP
- Depth-first co assegnamento a variabile singola
- La soluzione è ancora un cammino nell'albero


> [!seealso] Logica Algoritmo
> Sceglie ripetutamente una variabile non assegnata, e poi prova tutti i valori nel dominio di quella variabile a turno, cercando di estendere ciascuno in una soluzione tramite una chiamata ricorsiva. Se la chiamata ha successo, la soluzione viene restituita e, se fallisce, l'assegnazione viene ripristinata allo stato precedente e proviamo il valore successivo. Se nessun valore funziona, restituiamo il fallimento.

![[image-12.png|536x438]]
- Con stati atomici, gli algoritmi non informati non sfruttano l'euristica
- Nella rappresentazione fattorizzata esistono euristiche indipendenti dal dominio
	- Possono migliorare la velocità di ricerca

## 4.4 Miglioramenti 
- [[#4.4.2 Miglioramenti Tramite Inferenza|4.4.2 Miglioramenti Tramite Inferenza]]
### 4.4.1 Miglioramenti di Ricerca
- [[#MEVs - Minimum Remaining Values|MEVs - Minimum Remaining Values]]
- [[#Degree Heuristic - Gradi di Euristica|Degree Heuristic - Gradi di Euristica]]
- [[#Least-Constraint Values|Least-Constraint Values]]

#### MEVs - Minimum Remaining Values
> Scegli la variabile con meno valori legali -> utilizza quella per avere un approccio "**prima fallimenti**" così da fare una specie di **pruning** sui nodi con meno speranze così da eliminarli subito

#### Degree Heuristic - Gradi di Euristica
>La **degree heuristic** sceglie, tra le variabili non ancora assegnate, **quella che è coinvolta nel maggior numero di vincoli con altre variabili non assegnate**, così da massimizzare la riduzione futura del branching.  
>Serve spesso come **criterio di spareggio** quando più variabili hanno lo stesso numero di valori legali (dopo la MRV).

#### Least-Constraint Values
> Questa è la descrizione dell’**euristica LCV (Least Constraining Value)** 👨‍🏫
> - **Idea**: quando scegli il valore per una variabile, assegna **quello che elimina il minor numero di valori possibili per le altre variabili ancora non assegnate**, così da **lasciare più libertà futura**.
> - **Differenza con MRV**:
> 	- **MRV (Minimum Remaining Values)** sceglie _quale variabile_ assegnare → cerca di fallire presto per potare l’albero.
> 	- **LCV (Least Constraining Value)** sceglie _quale valore_ assegnare → cerca di **non restringere troppo** il resto del problema, per non imboccare precocemente un vicolo cieco.
> 
> 👉 Entrambe accelerano la ricerca, ma **da prospettive opposte**: MRV anticipa i fallimenti, LCV mantiene aperte le possibilità.

![[image-13.png]]

### 4.4.2 Miglioramenti Tramite Inferenza
- [[#Forward checking|Forward checking]]
- [[#Arc Consistency|Arc Consistency]]

#### Forward checking
Assegna un valore a $X$  
Per ogni vincolo tra $X$ e $Y$:  
→ **elimina** dal dominio di $Y$ tutti i valori che violano il vincolo con $X$.  
Se il dominio di Y rimane vuoto → **backtrack!**
- Significa che l’assegnamento precedente era errato.
- Esistono molte strategie di backtracking

👉 Questo è un esempio di **inferenza nei CSP**:
- Non si fa ricerca, si **riduce lo spazio delle possibili assegnazioni**.
- L’inferenza può essere eseguita **prima** e **durante** la ricerca.
	- Ad esempio, **prima della ricerca** si può imporre la **coerenza di stato** eliminando i valori di dominio che violano vincoli **unari**.

![[image-14.png]]

#### Arc Consistency
**coerenza d’arco**
- Una **variabile X è arc-consistent** rispetto a un’altra variabile $Y$ se:  
$$\forall x \in \text{dom}(X), \ \exists y \in \text{dom}(Y) \ \text{tale che} \ (x,y) \ \text{rispetta il vincolo tra X e Y}.$$

👉 In altre parole: **ogni valore nel dominio di X deve avere almeno un valore compatibile nel dominio di Y**.   
- Un **grafo è arc-consistent** se **tutte le variabili sono arc-consistent rispetto a tutte le altre** (cioè per ogni arco X→Y vale la condizione sopra).

> [!tip] Grafo Arc-Consistency
> - **Per ogni valore possibile di $X$**, deve esserci **almeno un valore compatibile in $Y$**.
> - Se esiste un valore $x$ che non ha nessun $y$ compatibile, allora quel $x$ va eliminato dal dominio di $X$.
> 
> Quando elimini un valore xx dal **dominio di una variabile X** tramite AC-3 o altra inferenza
> - Stai **riducendo lo spazio di ricerca**, perché quell’assegnamento **non verrà mai esplorato** durante il backtracking.
> - Questo **non elimina variabili**, elimina solo possibili valori per ciascuna variabile.

##### ⚙️ Come rendere un grafo arc-consistent: algoritmo AC-3
L’algoritmo **AC-3** è un procedimento di inferenza che **rende il CSP arc-consistente**, oppure segnala fallimento se qualche dominio si svuota.

**Passi principali:**
1. **Inizializza una coda** con tutti gli archi (X → Y) del CSP.
2. **Ripeti finché ci sono cambiamenti**:
    - Estrai un arco X → Y dalla coda.
    - **Rendi X arc-consistent rispetto a Y**:
        - Elimina dal dominio di X tutti i valori x per cui **non esiste nessun y nel dominio di Y compatibile**con x.
    - Se il dominio di X è stato modificato
        - Aggiungi alla coda **tutti gli archi K → X**, cioè quelli che puntano verso X, perché il cambiamento potrebbe renderli incoerenti.
    - Se il dominio di una variabile si svuota → **fallimento** (il CSP non ha soluzione).
##### ⏱️ Complessità
Per un CSP con:
-  **c** = numero di archi binari
- **d** = dimensione massima dei domini
👉 La complessità è **O(c · d³)**

- **Ogni arco** può essere controllato in **O(d²)** (perché bisogna confrontare ogni valore di X con ogni valore di Y).
- Ogni arco può essere reinserito nella coda al massimo **d volte**, perché ogni variabile può perdere al massimo d valori → da qui **O(c · d³)**.

##### 📝 Riassunto finale
- **Arc-consistency** = ogni valore di ogni variabile ha un supporto compatibile nei vicini.
- **AC-3** = algoritmo di inferenza che:
    - non fa ricerca,
    - riduce i domini eliminando valori impossibili,
    - può essere eseguito prima o durante la ricerca per potare fortemente lo spazio
- Se dopo AC-3 qualche dominio è vuoto → **il CSP non è risolvibile**.

#### Maintaining Arc Consistency
MAC (Maintaining Arc-Consistency) **estende il Forward Checking** chiamando AC-3 dopo ogni assegnamento durante la ricerca.
- Dato un assegnamento a ($X$), si chiama AC-3 con una coda contenente tutti gli archi ($Y \to X$), per tutte le variabili ($Y$) ancora non assegnate.
- Se AC-3 fallisce → **backtrack**.

MAC **potatura più aggressiva rispetto al Forward Checking**, perché verifica ricorsivamente eventuali incoerenze.

## 4.5 Local Search for CSP
Formulazione a **stato completo**: ogni stato assegna un valore a **tutte le variabili**.
- La ricerca cambia il valore di **una variabile alla volta**.
    - Quindi, lo stato iniziale è un **assegnamento casuale di tutte le variabili**.

**Euristica Min-Conflicts**: scegli il valore che **violerebbe il minor numero di vincoli**, così da avvicinarti alla soluzione.
- È possibile **pesare diversamente i vincoli** per dare priorità a quelli più importanti,
    - perché la soluzione finale potrebbe non essere ottimale rispetto a tutti i vincoli.
### 4.5.1 Min-Conflict Heuristic
**Euristica Min-Conflicts**: scegli il valore che **violerebbe il minor numero di vincoli**.
- Funziona molto bene, **tranne in una regione critica**.
    - Ad esempio, risolve il problema delle **N-queens** in un numero di passi quasi costante, indipendentemente da $N$ (circa 50 passi).
### 4.5.2 Sfruttiamo la Struttura del Problema
Anche avendo **un arsenale di milioni di trucchi** (e ce ne sono molti altri!), i **CSP rimangono comunque molto difficili in generale**.
- Caso peggiore: ( $d^n$ ) foglie nell’albero di ricerca (come abbiamo visto).
#### Scomposizione in sottoproblemi indipendenti
- Alcuni problemi possono essere **divisi in sottoproblemi indipendenti**, ad esempio “T” e “mainland”.
- Gli **algoritmi sui grafi** possono identificare queste **sottostrutture nel grafo dei vincoli**, come le **componenti connesse**.
- Le **componenti connesse** sono indipendenti tra loro, quindi la **soluzione complessiva** del CSP è semplicemente **l’unione delle soluzioni di ciascuna componente**.
#### ⚡ Vantaggio computazionale

- Se una componente ha $c < n$ variabili, allora la ricerca su quella componente scala come $d^c$ , invece di $d^n$.
- In altre parole, **scomporre il problema in sottoproblemi riduce drasticamente la complessità**, sfruttando l’indipendenza tra le variabili.

### 4.5.3 Tree-structured CSPs
Se il **grafo dei vincoli è un albero**, risolvere il CSP costa $O(n d^2)$
- Lineare in $n!$

Dato un **grafo dei vincoli senza cicli** (un albero), l’algoritmo di ricerca procede così:
1. **Ordinamento topologico**: scegli una qualsiasi variabile come radice e ordina le variabili in modo che ciascuna sia figlia della propria variabile genitore.
2. **Rendi l’albero arc-consistente**; se fallisce → termina con fallimento.
    - Ci sono ( n-1 ) archi e ( d^2 ) combinazioni di valori da controllare per ciascun arco.
3. **Assegna a ogni nodo** un qualsiasi valore consistente con quello del genitore; se non esiste → fallimento

> In questo caso **non serve mai fare backtracking**.

![[image-15.png]]

### 4.5.4 🌳 “Piantare alberi dove non ce ne sono”

- Possiamo risolvere **rapidamente CSP strutturati ad albero**.
- Ma cosa succede se il CSP **non è un albero**? Possiamo provare a **forzarlo a diventarlo**.
#### Procedura per trasformare un CSP in albero
1. **Assegna un valore a una variabile** (scegli un nodo).
2. **Rendi gli archi coerenti** (arc-consistency).
3. **Rimuovi la variabile assegnata**

- Se il grafo risultante **può essere trasformato in un albero**, allora puoi **verificare rapidamente se il CSP è risolvibile**.
- Altrimenti, puoi provare **un altro valore per la variabile** o **scegliere un altro nodo da assegnare**.
#### ⚡ Cutset Conditioning

- Supponiamo di dover provare ( c ) variabili (il **cutset**), cioè quelle che “rompono i cicli” del grafo
- Complessità approssimativa:  
$$
    O(d^c (n-c) d^2)  
$$
- Qui:
    - $d^c$ = tutte le combinazioni di valori per le variabili nel cutset
    - $n-c$ = numero di variabili rimanenti che ora formano un albero
    - $d^2$ = costo per verificare la coerenza sugli archi rimanenti

- In pratica, riduce drasticamente il problema, perché **la parte ciclica viene gestita separatamente**, mentre il resto è un albero → risolvibile in tempo lineare.
![[image-16.png]]
## 4.6 Riassunto

### 4.6.1 I. Fondamenti dei Constraint Satisfaction Problems (CSP)

#### 5.1.1 Definizione e Struttura dello Stato

- **Stato Fattorizzato:** Lo stato $X$ è rappresentato da un insieme di $n$ variabili ${X_1, X_2, \dots, X_n}$.
- **Dominio ($D_i$):** Ogni variabile $X_i$ assume valori da un dominio $D_i$.
- **Vincoli ($C$):** La conoscenza del dominio è espressa da un insieme di vincoli che devono essere soddisfatti (es. $X_1 \le 0$, $X_2 \in {1, 3}$).
- **Obiettivo del CSP:** Trovare una **soluzione completa e consistente**.
    - **Assegnazione Completa:** Tutte le variabili hanno un valore assegnato.
    - **Assegnazione Parziale:** Solo alcune variabili hanno un valore assegnato.
    - **Assegnazione Consistente:** Le variabili assegnate soddisfano i vincoli.
    - **Soluzione Parziale:** Un'assegnazione parziale che è consistente.

#### 4.5.2 Rappresentazione dei Vincoli

- **Grafo dei Vincoli:** Le variabili sono rappresentate come **nodi** e i vincoli sono rappresentati come **archi**. La struttura del grafo può aiutare la ricerca.
- **Vincoli Comuni:**
    - **Vincoli Unari:** Coinvolgono una sola variabile (es. $A \ne 1$).
    - **Vincoli Binari:** Coinvolgono due variabili (es. $A \le B$).
    - **Vincoli Globali:** Coinvolgono un numero qualsiasi di variabili (non necessariamente tutte), es. $Alldiff(A, B, C, D)$.
- **Iper-Grafo dei Vincoli:** Necessario quando ci sono vincoli globali, poiché un arco per definizione connette solo due nodi.
    - **Riduzione:** Per i vincoli con dominio finito, un iper-grafo può sempre essere ridotto a un grafo normale suddividendo i vincoli globali in un insieme di vincoli binari. Questo è utile se si vogliono usare algoritmi per soli vincoli binari.
- **Vincoli di Preferenza (Soft Constraints):** Rappresentano una preferenza (es. se possibile, assegna questo rispetto a quello). Sono modellati tramite un costo associato a ciascuna assegnazione (diventando un problema di ottimizzazione vincolata).

### 4.6.2 II. Tecniche di Ricerca di Soluzioni
- [[#A. Framework di Ricerca (Stato Fattorizzato)|A. Framework di Ricerca (Stato Fattorizzato)]]
- [[#B. Ricerca Non Informata: Backtracking Search|B. Ricerca Non Informata: Backtracking Search]]
- [[#C. Miglioramenti tramite Euristiche (Domain-Independent)|C. Miglioramenti tramite Euristiche (Domain-Independent)]]
- [[#D. Ricerca Locale (Local Search)|D. Ricerca Locale (Local Search)]]
#### A. Framework di Ricerca (Stato Fattorizzato)

- **Stati:** Definiti dai valori assegnati finora.
- **Stato Iniziale:** L'assegnazione vuota ${\ }$.
- **Azioni:** Assegnare un valore a una variabile non assegnata che non è in conflitto con l'assegnazione corrente.
- **Profondità della Soluzione:** Ogni soluzione completa appare alla profondità $n$.
- **Problema di Complessità:** Il numero di foglie (senza ottimizzazioni) è $n! d^n$.

#### B. Ricerca Non Informata: Backtracking Search

- **Algoritmo:** Backtracking Search è essenzialmente una ricerca in profondità (Depth-First Search) con assegnazione a variabile singola.
- **Fissare l'Ordine:** Fissando un ordine per le variabili si elimina la complessità $n!$, riducendo il numero di foglie a $d^n$.
- **Processo:** Sceglie ripetutamente una variabile non assegnata, prova tutti i valori nel suo dominio, ed estende ricorsivamente l'assegnazione. Se la chiamata fallisce, l'assegnazione viene ripristinata e si prova il valore successivo. Se nessun valore funziona, restituisce fallimento.

#### C. Miglioramenti tramite Euristiche (Domain-Independent)

Nello stato fattorizzato esistono euristiche indipendenti dal dominio che velocizzano la ricerca.

- **1. Minimum Remaining Values (MRVs):**
    
    - **Scelta:** Scegliere la variabile con il **minor numero di valori legali** (possibili).
    - **Logica:** Cerca il fallimento il prima possibile (_Failure-First_) in modo che la potatura (_pruning_) avvenga prima.
- **2. Degree Heuristic:**
    
    - **Scelta:** Scegliere la variabile con il **maggior numero di vincoli** tra le variabili ancora da assegnare.
    - **Uso:** Serve come _tie-breaker_ per variabili con lo stesso numero di mosse legali (stesso MRV).
- **3. Least-Constraining Values (LCVs):**
    
    - **Scelta:** Scegliere un valore che escluda il **minor numero di valori** nelle altre variabili ancora da assegnare.
    - **Logica:** Mantiene aperte le opzioni, cercando di evitare percorsi stretti che potrebbero essere sbagliati.

#### D. Ricerca Locale (Local Search)

- **Formulazione a Stato Completo:** Ogni stato assegna un valore a _ogni_ variabile.
- **Inizio:** Lo stato iniziale è un'assegnazione casuale di tutte le variabili.
- **Azione:** La ricerca cambia il valore di una sola variabile alla volta.
- **Heuristica Min-Conflicts:**
    - **Scelta:** Scegliere il valore che **rompe il minor numero di vincoli**.
    - **Obiettivo:** Avvicina lo stato alla soluzione.
    - **Efficacia:** Funziona molto bene; ad esempio, risolve il problema delle N-regine in un numero costante di passi, indipendentemente da $N$.

### 4.6.3 III. Tecniche di Inferenza e Propagazione dei Vincoli

L'inferenza non è una ricerca, ma riduce lo spazio di possibili assegnazioni. Può essere eseguita prima o durante la ricerca.

#### Forward Checking (FC)

- **Funzionamento:** Dopo aver assegnato un valore a una variabile $X$, per ogni vincolo $X-Y$, si **eliminano i valori dal dominio di $Y$** che violano il vincolo.
- **Azione in caso di fallimento:** Se non rimangono valori in un dominio, si esegue il _backtrack_, poiché l'assegnazione precedente era sbagliata.

#### Arc Consistency (AC)

- **Definizione:** Una variabile $X$ è **arc-consistent** se per ogni arco $X \to Y$, per ogni valore $x$ nel dominio di $X$, esiste almeno un valore $y$ nel dominio di $Y$ che è permesso.
- **Grafo Arc-Consistent:** Un grafo è arc-consistent se tutte le variabili sono arc-consistent.

#### Algoritmo AC-3

- **Scopo:** Rende un grafo arc-consistent o restituisce fallimento.
- **Processo:**
    1. Creare una coda con tutti gli archi.
    2. Ripetere finché non ci sono più cambiamenti:
        - Selezionare un arco casuale $X \to Y$ e rendere $X$ arc-consistent, **riducendo il dominio di $X$**.
        - Se viene rimosso un valore da $X$, **aggiungere tutti gli archi $K \to X$ alla coda** (perché $K$ potrebbe non essere più consistente rispetto al nuovo $X$).
        - Se una variabile rimane senza assegnazioni possibili, fallire.
- **Complessità:** $O(cd^3)$, dove $c$ è il numero di archi binari e $d$ è la dimensione del dominio.

#### Maintaining Arc Consistency (MAC)

- **Funzionamento:** MAC aumenta il _forward checking_ chiamando AC-3 dopo ogni assegnazione durante la ricerca.
- **Invocazione:** Data un'assegnazione a $X$, MAC chiama AC-3 con una coda di archi $Y \to X$ (per tutte le $Y$ non ancora assegnate).
- **Potatura:** MAC pota più del _forward checking_ perché verifica ricorsivamente le incoerenze.

### 4.6.4 IV. Sfruttare la Struttura del Problema (Semplificazione)

Anche con molte euristiche, i CSP rimangono difficili in generale (worst-case $d^n$). Sfruttare la struttura può portare a grandi semplificazioni.

#### Componenti Connesse Indipendenti

- **Identificazione:** Gli algoritmi sui grafi possono identificare sottostrutture (componenti connesse) nel grafo dei vincoli.
- **Vantaggio:** I componenti connesse sono sottoproblemi indipendenti.
- **Soluzione:** La soluzione è l'unione delle soluzioni dei componenti.
- **Scalabilità:** La complessità scala come $d^c$, dove $c < n$ sono le variabili coinvolte nel sottoproblema.

#### CSPs a Struttura ad Albero (Tree-Structured CSPs)

- **Definizione:** Se il grafo dei vincoli è un albero (senza cicli), il CSP è notevolmente più facile.
- **Costo:** La risoluzione costa $O(nd^2)$, che è **lineare** in $n$ (numero di variabili).
- **Algoritmo (Nessun Backtrack):**
    1. **Ordinamento Topologico:** Scegliere una variabile come radice e ordinare le variabili in modo che ciascuna sia figlia del suo genitore.
    2. **Arc-Consistency:** Rendere l'albero arc-consistent (o fallire).
    3. **Assegnazione:** Assegnare a ogni nodo un valore consistente con il suo genitore (o fallire).

#### Cutset Conditioning (Piantare alberi dove non ci sono)

- **Scopo:** Forzare un CSP a diventare strutturato ad albero.
- **Processo:** Si assegna un valore a un sottoinsieme di variabili, chiamato **cutset** ($c$ variabili).
- **Vantaggio:** Se il grafo risultante (dopo aver rimosso e assegnato il cutset) può essere trasformato in un albero, si può verificare rapidamente se è risolvibile.
- **Complessità:** $O(d^c (n-c) d^2)$. L'efficacia dipende dalla dimensione $c$ del _cutset_.

# 5 Games
> **Competitive Game** -> Ricerca contraddittoria (*adversarial search*)
> - Giocatori fanno a turno
> - I giocatori hanno goal opposti, in contrasto
> - Somma zero -> uno vince l'altro perde
> - **Perfettamente informato -> fully observable**

- Stato iniziale -> $S_0$
- $\text{TO-MOVE}(s)$ -> Mossa per spostarsi nello stato $s$
- $\text{ACTIONS}(s)$ -> mosse legali per lo stato $s$
- $\text{RESULT}(s,\ a)$ -> dopo lo spostamento (transition model) il risultato è il nuovo stato
- $\text{IS-TERMINAL}(s)$ -> il gioco è finito o no
- $\text{UTILITY}(s;\ p)$ -> Funzione di utilità - payoff ottenuta su uno stato terminale $s$ dal giocatore $p$ -> ossia è lo stato finale

![[image-17.png|559x174]]

## 5.1 Game Tree -> Min-Max Problem
> Stesse proprietà della ricerca ad albero 
> - Può essere infinita se lo spazio degli stati è infinito
> - Può essere infinito se lo spazio degli stati è finito ma permette ripetizioni degli stati (posizioni)

Il grafico dello spazio degli stati ha i nodi che rappresentano gli stati e gli archi che rappresentano le possibili azioni
- Lo stesso stato può essere raggiunto da più direzioni - *path*
- The game tree è *sovrapposto* al grafico -> nel gioco la radice è la posizione di partenza

Non c'è speranza di costruire l'intero game tree per qualsiasi gioco anche di dimensione ragionevole
-  $< 9!$ states for tictactoe $≈ 360 000$ states -> insostenibile
![[image-18.png|299x242]]

### 5.1.1 MIN - MAX
> La strategia per ogni giocatore è un *piano condizionato* (**conditional plan**)
> - *Si può applicare a qualsiasi gioco deterministico con informazione perfetta*
> - Cosa fa MAX se MIN fa qualcosa?


Se il gioco ha solo come risultato win/lose
- Allora abbiamo una ricerca in un ambiente non deterministico
- Le mosse dell'avversario sono modellate come una distribuzione di probabilità
- Win=goal, lose = not goal
- **AND-OR albero di ricerca**

Se il risultato ha più valori -> **minmax**
- È perfetto per giochi deterministici e con informazioni perfette

![[image-21.png|551x150]]
![[image-19.png|551x306]]

#### Minmax Value
Una volta ottenuto il valore minimo massimo per ogni nodo, puoi recuperare la strategia ottimale

Nelle foglie (nodi terminali) -> $\text{MINIMAX}(s) = \text{UTILITY}(s;\ p)$
- Il valore di utility -> ossia valore del risultato finale

Un nodo intermedio, non terminale -> $\text{MINIMAX}(s) =$ il valore dell'utility **per MAX**
- Ossia il valore **MINIMAX(s)** rappresenta il **valore di utilità garantito per MAX** se entrambi i giocatori (MAX e MIN) giocano in modo ottimale da quel nodo in poi.
- È il miglior risultato che MAX può ottenere partendo da $s$, considerando che MIN cercherà di minimizzare il punteggio di MAX.

$$
\text{MINIMAX}(s)=\max_{𝑎∈\text{𝐴𝐶𝑇𝐼𝑂𝑁𝑆}(𝑠)}\ \text{MINIMAX}(\text{𝑅𝐸𝑆𝑈𝐿𝑇}(𝑠,\ 𝑎 )) ,\ \text{TO-MOVE}(s) = \text{MAX}
$$
$$
\text{MINIMAX}(s)=\max_{𝑎∈\text{𝐴𝐶𝑇𝐼𝑂𝑁𝑆}(𝑠)}\ \text{MINIMAX}(\text{𝑅𝐸𝑆𝑈𝐿𝑇}(𝑠,\ 𝑎 )) ,\ \text{TO-MOVE}(s) = \text{MIN}
$$
- MAX sceglie l'opzione che massimizza MINIMAX
	- Il punteggio di MAX dipende dalle mosse di min infatti come vediamo nella figura nel turno di MAX (root) il valore è 3 perchè considera che min faccia la scelta migliore e quindi MAX deve scegliere tra 3 e 2 che sono i valori minimi quando tocca a MIN
- MIN sceglie l'opzione che minimizza MINIMAX
	- Dato che ha l'obbiettivo opposto

> Utilizzando l'algoritmo Minimax:
> 1. **Esegui una ricerca in profondità (depth-first search) dalla radice** per trovare tutti gli stati terminali (foglie)
> 2. **Ottieni l'utilità delle foglie** (i valori numerici associati ai nodi terminali).
> 3. **Ripercorri all'indietro (backtrack) per calcolare il valore Minimax di tutti i nodi intermedi**:
    - Se il nodo genitore è un nodo **MIN**, prendi il valore **minimo** tra i valori Minimax dei figli.
    - Se il nodo genitore è un nodo **MAX**, prendi il valore **massimo** tra i valori Minimax dei figli.

ES:
Immagina un gioco dove MAX e MIN alternano mosse:
- MAX parte dalla radice e sceglie tra $A_1$​, $A_2$​, $A_3$​.
- Se sceglie $A_1$​, vede 3, 12, 8, ma sa che MIN minimizzerà a 3.
- Sceglie $A_1$ perché 3 è il massimo tra 3, 2, e 2 (valori di $A_1$​, $A_2$​, $A_3$​)


> [!important] MinMax Search
> - **Completa** -> **SI** con stati finiti
> - **Ottimale** -> **SI**, contro un giocatore perfetto
> - **Complessità in Tempo** -> $O(b^m)$
> - **Complessità Spazio** -> $O(bm)$ = Depth-First

#### Giocare contro un Avversario Sub-Ottimale
> Minimax ha dei leggeri inconvenienti

A seconda del giocatore può fare mosse perfette oppure come nella maggior parte dei casi sbagliare e quindi non avere un gioco perfetto.
Se io gioco contro di lui e se la posizione è un pareggio con un gioco perfetto, potresti voler fare una mossa complicata, ma non ottimale per provare a prendere la vittoria:
- Se rispondo con l'unica mossa corretta nella posizione: perdi (p=1%)
- Se rispondo con alcune delle mosse sbagliate: vinci (p=85%)
- Se rispondo con il resto delle mosse (non ottime, ne pessime): è un pareggio (p=14%)

Questa è la differenza tra le cosiddette "mosse del computer" e le "mosse umane"
- Ci sono programmi per computer che cercano di imitare il modo in cui gli esseri umani si comportano contro gli avversari non ottimali

##### Complessità
- Branching factor in chess = 35
- Average game length = 40 moves = 80 ply
- Given the complexity of DFS, you get 3580 states to visit
> Per ridurlo abbiamo bisogno del **PRUNING**

### 5.1.2 $\alpha$ - $\beta$ pruning
> Pota i rami che non portano a nessuna mossa migliore né per MAX né per MIN

$\alpha$ = valore MINIMAX più grande finora
$\beta$ = valore MINIMAX più piccolo finora
$[\alpha,\ \beta]$
Iniziale = $[-\infty,\ +\infty]$

![[image-22.png]]

**Regola generale**: considera un nodo con MINIMAX = V. 
- Se un nodo della stessa profondità (MINIMAX=m') o un nodo più alto (MINIMAX=m) ha un valore MINIMAX migliore, non raggiungerai mai il nodo con MINIMAX=V: **puoi potarlo!**


> [!question] Perchè Pruning?
> Non ha effetto sulla soluzione
> Con il migliore pruning possiamo ridurre la complessità a $O(b^{m/2})$
> - Buono ma non sufficiente per problemi con grandezza ragionevole ($\sqrt b$ fattore su cui ha effetto)
> - Il miglior pruning dipende **dall'ordine delle mosse**
> - La generazione di mosse che consentono di potare prima l'albero porta alla migliore potatura -> faccio mosso che permettono di potare più rami

La ricerca del raggio può essere utilizzata solo per considerare le mosse "top" k ogni volta, in base ad alcune funzioni di valutazione
- Rischi di potare la mossa migliore, ovviamente.

#### Strategie diverse da DFS
- **Type A** strategy
	- Esplorare completamente lo spazio della ricerca fino ad una profondità limitata
	- Usare un euristica per valutare utility per i nodi alla profondità finale
- **Type B** strategy
	- Non esplorare le mosse che sembrano pessime basandosi su una data euristica
	- Segui le mosse più promettenti dove possibile -> possibilmente fino alla fine del gioco

Type A funziona se il fattore di brenching (è il numero massimo di figli che un nodo può avere in un albero) non è molto largo, Type B è utile quando il fattore di brenching è molto alto.

Le euristiche vengono **apprese** o sfruttate come una combinazione ponderata di caratteristiche di gioco
**Effetto orizzonte**: l'euristica valuta la posizione come buona, ma esplorando un livello di profondità in più potrebbe cambiare completamente la valutazione.

### 5.1.3 MCTS - Monte-Carlo Tree Search
> Utile quando il fattore di brenching è enorme, Anche con la potatura, saresti limitato a piccole profondità
> - Utile quando non hai una funzione di valutazione buona -> anche se puoi sempre aggiungerla se ce l'hai

**Rollout - Playout** (lanci)-> da un determinato stato, riproduci un intero episodio fino alla fine e ottieni l'utilità.
In MCTS, il valore di uno stato è l'**utilità finale media** su un certo numero di lanci da quello stato

**Playout Policy** -> funzione che decide quqle mossa prendere ad ogni step
- Questa è la parte dove il reinforcement learning aiuta molto
- La politica di lancio non è tutta la storia

**Pure Monte-Carlo Search** -> dato uno stato, calcolare il suo valore effettuando $N$ rollouts
- Nessun search tree necessario

#### Passaggi di MCTS (Monte Carlo Tree Search)

- **Registra la simulazione in un albero di ricerca**  
	- All'inizio, l'albero di ricerca contiene solo lo stato iniziale.

- Dato un albero di ricerca con uno stato come radice, ripeti:  
	- **Selezione**: Dalla radice, usa la politica di selezione per scegliere un percorso verso una foglia.  
	- **Espansione**: Usa la politica di simulazione per generare un nuovo nodo a partire dalla foglia corrente.  
	- **Simulazione**: Esegui una rollout (simulazione completa) dal nodo appena espanso usando la politica di simulazione. L'albero di ricerca non viene aggiornato.  
	- **Back-propagation (non quella!)**: Aggiorna il valore dei nodi nell'albero di ricerca dal nodo appena aggiunto fino alla radice, seguendo il percorso di selezione.  
	- **Ritorno**: Scegli la mossa con il numero più alto di simulazioni (vedi più avanti).  
	- **Esecuzione della mossa**: Esegui la mossa, ottieni il nuovo stato e ripeti i passaggi sopra.

#### Politica di selezione
- **Definizione**: Una funzione che determina quale mossa seguire in un dato albero di ricerca.  
- **Bilanciamento**: Concilia **sfruttamento** (simulazioni da stati buoni e ben esplorati) e **esplorazione** (simulazioni da stati meno frequentati).  

MCTS combina esplorazione casuale e strategia per migliorare le decisioni in giochi complessi, aggiornando l'albero solo con i risultati delle simulazioni.

![[image-23.png]]

#### Tempo di Esplorazione
$$
\sqrt{\frac{log_N(\text{Parent}(n))}{N(n)}}
$$
- $N(\text{Parent}(n))\ge N(n)$
	- Come ogni volta che visiti n visiti Parent(n)
- $log_N(\text{Parent}(n))\le N(n)$
	- Da un certo numero di playouts in avanti

Supponiamo che Parent$(n)$ riceve 10 playouts più di $n$
Il temine di esplorazione va a 0 quando il numero di playouts aumenta
- non vuoi esplorare per sempre

![[image-24.png|468x359]]

![[image-25.png]]

#### Deterministic Games
- MCTS non richiede un euristica
	- Puoi usarlo su tutti i giochi basta conoscere le regole 
	- Puoi usare un euristica se ne hai una (ad esempio per migliorare la politica)
- MCTS si basa sull'esplorazione, quindi potrebbe volerci del tempo per determinare il valore di una mossa ovviamente buona/cattiva.
	- L'euristica può aiutare qui
- MCTS con politica/euristica appresa raggiunge costantemente prestazioni sovrumane
	- Chess

Nota: gli approcci moderni combinano ricerca e apprendimento e altro ancora!
#### Stochastic Games
Nodo casuale che rappresenta tutti i possibili risultati (ad esempio, il risultato di un lancio di dadi)
![[image-26.png|461x352]]

#### Minimax Previsto
Lo stesso di minimax, ma devi gestire anche i nodi casuali
Il valore di un nodo casuale è il valore atteso di tutti i possibili risultati del nodo

$$
\text{ExpectedMINIMAX}(s) =\sum_r P(r) \text{𝐸𝑥𝑝𝑒𝑐𝑡𝑒𝑑𝑀𝐼𝑁𝐼𝑀𝐴𝑋}(\text{𝑅𝑒𝑠𝑢𝑙𝑡}(𝑠,\ 𝑟))
$$
-Puoi applicare un ragionamento simile a giochi con informazioni parziali, sia deterministici (ad esempio, scacchi ciechi) che stocastici (ad esempio, poker). 
- **Sono preferibili aggiustamenti ad hoc**, ma non entreremo nel dettaglio.
---
# 6 Ottimizzazioni
#slide8
## 6.1 Artificial Life
La vita artificiale cerca di capire le proprietà essenziali dei sistemi viventi sintetizzando un comportamento realistico in software, hardware e prodotti biochimici.
- **Soft Artificial Life** -> simulazioni digitali processi e organismi viventi
- Hard Artificial life -> implementazione hardware (soft robot). evoluzione dei controller e della morfologia ossia la struttura fisica
- Wet Artificial life -> sintetizza sistemi viventi delle sostanze biochimiche, ossia cellule artificiali
- **Impegno ingegneristico non necessario** -> non vogliamo necessariamente risolvere un problema con un certo margine di errore
- Vogliamo capire come le cose funzionano e scoprire nuove cose, se qualcosa diventa anche utile meglio.

### 6.1.1 Prioneria dell'Artificial Life
> La vita artificiale slega il vincolo materiale al carbonio permettendo di studiare i principi generali senza essere legati al caso specifico (carbonio) ma analizzando in generale il fenomeno

#### Origini
La vita è un processo iterativo guidato dall'evoluzione -> avere infiniti loop è potente
- condizione iniziale -> come la vita è nata
- Evoluzione Biologica -> algoritmo
	- Come la vita progredisce e diventa varia
	- Come gli organismi moderni sono i discendenti dei loro antenati
#### Natural Selection
> Condizioni **necessarie e sufficienti** per l'evoluzione tramite natural selection

- Tasso di riproduzione diverso
	- Proprietà dell'ambiente che favoriscono un un certo tratto
	- Sopravvivono gli individue che combaciano di più all'ambiente
- Ereditarietà -> tratti che vengono passati  alla nuova generazione
- Variazioni -> tratti differenti in una popolazione

> Tratti vantaggiosi sopravvivono in una popolazione mentre gli altri spariscono -> **convergono verso una popolazione omogenea** fino alla successiva mutazione e a quelle già presenti

Punti chiave dell'evoluzione:
- Adattamento -> una caratteristica è favorita dalla selezione naturale diventa sempre più importante per un dato compito
- Esitazione -> trovare un uso diverso per un tratto prima utilizzato per un diverso uso.
- Diversità:
	- Mutazioni casuali -> crea nuovi geni
	- Ricombinazione -> cross-over, mischiare i geni
	- Selezione naturale -> cambiata in base al fitness
		- Fitness -> rappresenta quanto è buono ad essere trasmesso un genotipo comparato agli altri
		- Selezione sessuale e artificiale
	- Processi divergenti -> non collassa, ma crea novità
#### Condizioni Iniziali e Algoritmo di evoluzione
Bisogna approssimare le condizioni iniziali di ambiente e di popolazione composta da molti individui.
Un sistema caotico e complesso: ogni approssimazione porta a grandi differenze negli stati futuri.

**Problema di Ottimizzazione** -> partiamo già da un processo ben definito: evoluzione tramite selezione naturale.
Possiamo simulare le condizioni necessarie e sufficienti:
- variazione
- Riproduzione differenze
- Ereditarietà

### 6.1.2 Skeleton of EAs
- Inizializzazione della **popolazione** $P_0$ di **individui** con $N$ caratteristiche (features)
	- Le caratteristiche sono espresse in un dominio specifico di linguaggio (bit-string o numeri reali)
- **Valutazione** della **fitness** della popolazione $P_0$
- Per ogni step $t=1,1,...$
	- Selezione un genitore di $P_0$ in base alla fitness
	- **Riproduzione e ricombinazione** delle caratteristiche del **figlio**
	- Applica una mutazione delle caratteristiche (random o rate di mutazione di adattamento)
	- **Valuta** la fitness della nuova popolazione
	- Selezione un sottoinsieme di $K$ individui sopravvissuti prendendo $P_t$ (esempi casuali pesati dalla fitness)

#### EAs -> Dettagli e Variazioni
- Basandosi sulle regioni dove la fitness è alta -> la sfruttiamo
	- Hill-climbing nello spazio della fitness
- Seleziona i genitori basandosi solo sul rank relativo della fitness
- Il numero di figli è proporzionale alla differenza tra la media e la fitness individuale -> *ossia se ha una fitness alta avrà più figli*
- **Rate di mutazioni diversi** per ogni caratteristica individuale
- La ricombinazione può mediare le caratteristiche o prendere il massimo/minimo
	- La ricombinazione cross-over inserisce un punto di taglio e cambia le caratteristiche

### 6.1.3 Diversi Tipi di ALGORITMI Evolutivi
**Strategie di Evoluzione -> Basate sulla distribuzione**
- Mutazioni random + valutazione + ricombinazione
- Programmazione Evolutiva -> mutazione random + valutazione

**Algoritmi Genetici -> Basati sulla popolazione**
- Come input abbiamo i genotipi codificati come stringhe di bit
- Le mutazioni sono rare e la ricombinazione è il fattore chiave
- La fitness è rilevante anche per selzionare i genitori

**Programmazione Genetica**
- l'individuo è un set di istruzioni -> albero
- Algoritmi genetici per codice genetico

### 6.1.4 Genetic Programming
Fenomeni tipici:
1. Fixing Sorting -> l'AI invece di fare un ordinamento complesso sceglie la strada semplice ossia cancellare tutti gli elementi così la lista è sicuramente ordinata, questo però rende il codice inutile -> controllo: deve contenere gli stessi elementi di input
2. L'evoluzione può portare a cheat ossia eliminare il file target, modificando l'ambiente 
3. **Bloating** -> Le mutazioni sono casuali e distruttive. Se hai un codice perfetto di 3 righe, una mutazione probabilmente lo romperà.
	L'evoluzione tende ad aggiungere enormi quantità di codice inutile (definito "introns" o junk code) come cicli che non fanno nulla.
	- *FUNZIONE:* perchè aumento il codice che le mutazioni vanno ad intaccare riducendo la probabilità che vengano intaccate le parti vitali

### 6.1.5 Black-box Ottimizzazioni
1. Algoritmo di ricerca da massimizzare $f:S\to R$
	1. $S$ è lo spazio di ricerca
	2. Di solito: $S⊂  R^n$
2. $f$ può essere qualsiasi funzione -> più utile se $f$ non è convessa
3. L'unica informazione è la valutazione della funzione $f$ in un punto
	1. I punti sono presi dallo spazio di ricerca, deve essere in grado di campionare
	2. Non ha bisogno di altre informazioni, **no derivate** 
4. **Metriche di Performance**
	1. Richiede una valutazione per convergere
	2. Valore della soluzione finale
	3. Note -> assumiamo che il costo della valutazione è indipendente dal numero degli esempi

### 6.1.6 Strategie di Evoluzione
1. Ottimizzazione black-box **stocastica**
2. $f$ è una funzione **continua**
3. Inizia con un insieme di individui
	1. Valuta gli individui
	2. Muta e ricombina individui
4. Individui all'inizio della simulazione $x^0\in S ⊂ R^n$
	1. fenotipo, oggetto o **parametri di decisione**
5. Un set di **parametri strategici** $\sigma^o$ per ogni individuo
	1. Tasso di mutazione dei parametri decisionali
	2. Parametri che possono evolversi

> Mutazione = **aggiungere rumore casuale** (Gaussiano)
> - Isotropic Gaussian (1 parametro -varianza) = $N(m,\sigma^2\ l)$)
> - 1 parametro per ogni dimensione = $N(m,\ D^2)$ con $D$ diagonale

Esplora uniformemente lo spazio di ricerca ossia è come fare l'assunzione minima sulla struttura di $f$
**Mixing number  $\rho$** -> indica quanti genitori generano figli
- I genitori sono scelti casualmente
- $\rho=1$ -> clone
- Selezione -> (𝜇, 𝜆)-ES e (𝜇 + 𝜆)-ES
	- Ossia come vedremo dopo la selezione nel primo caso tra solo i figli e nel secondo la selezione dall'insieme combinato di figli e genitori

Se vuoi generare $\lambda$ figli, devi scegliere $\lambda$ gruppi di $\rho$ genitori
**Ricombinazione** -> molte possibilità
- Ad esempio -> selezionare delle features $j$ dai genitori $i$ (random), media (multi-recombination), media pesata

## 6.2 Self-adaptive ES
> Adapting strategy parameters
> - *ES = Evolution Strategies*

### 6.2.1 (𝜇, 𝜆)-ES
- Popolazione $X^g\in R^{\micro ,n}$, step size $\sigma^g\in R^\micro$ -> strategy parameter
	- Uguali per tutte le features do un dato individuo -> isotopic gaussian
- Genera $\lambda$ figli ($k=1,...,\lambda$)
	- Seleziona parametri a caso $P^g\in R^{\rho ,n}$
	- $\sigma^{g+1}_k =recombine(\sigma^g |P^g)e^{\epsilon_k\sim N(0,1)}$
	- $x^{g+1}_k =recombine(P^g)+N(0,\sigma^{g+1}_k)$
	- Valutare la fitness
- Teniamo i meglio figli $\micro$ tra tutti i figli $\lambda$
	- $\lambda > \micro$ iperparametro -> non viene cambiato durante l'evoluzione
- I genitori vengono **sempre scartati**

Come candidati possiamo prendere:
- gli individui **migliori**
- La **media** degli individui
- La **media pesata** degli individui
![[image-27.png|414x386]]

### 6.2.2 (𝜇 + 𝜆)-ES
> **I figli competono anche con i genitori**
> - è meglio per spazi finiti grandi o problemi combinatori

- ($1+1$)-ES fu il primo ES
	- Seleziona un individuo tra due possibilità il genitore o il figlio
- Preserva il migliore fino ad ora
- Avvolte può mischiare i numeri
	- $(\micro /\rho + \lambda)$-ES
	- $(\micro /\rho , \lambda)$-ES

#### Descrizione Algoritmo
Questo pseudocodice descrive il funzionamento generale di una **Strategia Evolutiva** (Evolution Strategy - ES), mostra le due alternative viste prima $(\mu/\rho \stackrel{+}{,} \lambda)$.

Ecco cosa fa passo dopo passo in parole semplici:
1. Creazione dei figli -> Da un gruppo di genitori ($\mu$), l'algoritmo genera un numero maggiore di figli ($\lambda$). Per creare ogni figlio:
    - **Marriage & Recombination:** Seleziona alcuni genitori ($\rho$) e mescola le loro informazioni (sia il codice genetico $y$ che i parametri strategici $s$).
    - **Mutation:** Applica mutazioni casuali. Nota importante: prima muta i parametri di strategia ($s$, che controllano _quanto_ mutare) e poi usa quelli per mutare il codice genetico vero e proprio ($y$).
    - **Fitness:** Calcola il punteggio ($F$) del nuovo figlio.
2. Selezione dei sopravvissuti (Line 14-17) -> Alla fine del ciclo, deve decidere chi passa alla generazione successiva. Il codice mostra due modalità alternative:
    - **$(\mu, \lambda)$ - Selezione a virgola:** I genitori vengono scartati a priori. I nuovi sopravvissuti sono scelti **solo tra i figli**. (Utile per evitare di rimanere bloccati in ottimi locali).
    - **$(\mu + \lambda)$ - Selezione col più:** I nuovi sopravvissuti sono i migliori scelti dall'insieme unito di **genitori E figli**. (Molto elitario, preserva sempre la soluzione migliore trovata finora).

In sintesi: **Genera una prole numerosa rimescolando i genitori, mutala, e poi tieni solo i migliori (o rimpiazzando i vecchi o facendoli competere).**

#### Perchè Dovremmo Usare (𝜇, 𝜆)-ES se non è Elitaria
> *ELITARIA* -> significa che la soluzione ottima trovata fino ad ora sopravvive sempre, quindi NON-ELITARIA significa che le soluzione vecchie vengono sempre scartate anche se erano le migliori

Per evitare l'overfitting verso l'oggetto
- `,`-selection -> è un esploratore migliore dello spazio di ricerca
- `+`-selection -> è uno sfruttatore migliore dello spazio di ricerca

- **Il Paradosso:** A volte, per risolvere un problema, devi **smettere di cercare direttamente l'obiettivo**.
- **L'Esempio del Labirinto:**
    - _La Metrica:_ Immagina che il punteggio sia la "distanza in linea d'aria dall'uscita".
    - _L'Inganno:_ Se ti trovi vicinissimo all'uscita ma separato da un muro, il tuo punteggio è alto.
    - _La Necessità:_ Per vincere devi allontanarti dal muro (peggiorare il punteggio momentaneamente) per aggirare l'ostacolo.
- **Il Paesaggio di Ricerca (Landscape):**
    - È pieno di **ottimi locali**: punti che sembrano soluzioni (punteggio alto) ma sono in realtà vicoli ciechi (come il punto davanti al muro).
- **La Soluzione:**
    - Poiché è difficile capire quando un problema è "ingannevole", la strategia migliore è **aumentare l'esplorazione** (fare cose diverse) invece di seguire ciecamente la metrica di ottimizzazione, accettiamo di fare un passo peggiorativo per permetterci di migliorare in seguito.

#### Comparazione `,` e `+` -selection
Cosa succede se $\micro = \lambda$, ossia il numero di figli è uguale al numero dei nodi che devo prendere, nel caso di (𝜇, 𝜆)-ES?
- Tutti i figli sono presi
- Non c'è selezione
- Passi casuali nello spazio di ricerca
Cosa succede invece in *(𝜇 + 𝜆)-ES*?
- Non ci sono problemi la selezione continua a funzionare
- Tieni i meglio $\micro = \lambda$ individui predi dal set formato dai figli e genitori $\micro + \lambda$
- Qui non cambia niente dato che l'insieme dove applico la selezione è più grande dei soli figli dato che comprende anche i genitori

### 6.2.3 CMA-ES - Covariance Matrix Adaptation
> Le mutazioni seguono un matrice di covarianza **adattiva** che si aggiusta basandosi sulla forma locale del paesaggio della funzione 
> - Come il momentum -> velocizza quando c'è una pianure e rallenta quando ci sono colline

- Popolazione campionata dalla gaussiana multivariata -> $N(m^g, C^g)$
	- $C^g$ simmetrica
	- $\frac{n^2+n}{2}$ gradi di libertà
- Il vettore medio è preso come soluzione candidata
- Inizializza $C^0=I$, $m^g\in R^n$ - random

#### 1 Generazione della Prole -> Sampling
- **L'Azione:** Creiamo nuovi candidati (figli) partendo dalla distribuzione attuale. 
- La Formula:$$x_k^{g+1} = m_g + \sigma \cdot N(0, C_g)$$
	- **$m_g$:** Il punto centrale attuale (la media della popolazione "buona").
	- **$\sigma$ (Sigma):** L'ampiezza del passo (step-size), quanto lontano osiamo andare.    
    - **$N(0, C_g)$:** Il "rumore" casuale modellato dalla matrice di covarianza $C_g$ (che determina la _forma_ della ricerca, es. un cerchio o un ellisse).
- **In sintesi:** Prendiamo il centro attuale e "spariamo" $\lambda$ figli casuali attorno ad esso.
#### 2 Valutazione e Ordinamento -> Selection
- **Ordinamento:** Una volta generati i figli, calcoliamo la loro fitness (punteggio).
- **Ranking:** Li mettiamo in fila dal migliore al peggiore ($x_{1:\lambda}$ è il migliore, $x_{\lambda:\lambda}$ è il peggiore).

#### 3 Aggiornamento della Media -> Recombination
- **L'Obiettivo:** Spostare il centro della ricerca ($m_{g+1}$) verso la zona promettente trovata dai figli migliori. 
- La Formula:$$m_{g+1} = \sum_{i=1}^{\mu} w_i \cdot x_i^{g+1}$$
    - Si calcola la **media pesata** solo dei migliori $\mu$ figli.
    - **$w_i$ (Pesi):** Danno più importanza ai figli migliori ($w_1 > w_2 > \dots$). La somma dei pesi è 1.

#### 4 Il Concetto Chiave: Selezione Troncata -> Truncated Selection
- **La Domanda:** Perché usiamo solo $\mu$ genitori se ne abbiamo generati $\lambda$ ($\mu < \lambda$)?
- **Il Motivo:** È questo che crea la **pressione evolutiva**.
    - Se usassimo _tutti_ i figli ($\mu = \lambda$) per calcolare la nuova media, il centro rimarrebbe praticamente fermo (la media del rumore casuale è zero).
    - Scartando i peggiori e tenendo solo i migliori (Troncamento), forziamo la media a spostarsi fisicamente verso la "salita", guidando l'evoluzione.

Ecco il seguito dello schema, strutturato per collegarsi direttamente ai punti precedenti (Sampling, Selection, Mean Update). Qui entriamo nel cuore matematico del CMA-ES.

#### 5 Effective Sample Size ($\mu_{eff}$) - La Qualità dell'Informazione
- **Il Concetto:** Misura quanta informazione stiamo realmente utilizzando per aggiornare la distribuzione, basandosi su come sono distribuiti i pesi $w_i$.
    - La Formula:$$\mu_{eff} = \frac{1}{\sum_{i=1}^{\mu} w_i^2}$$
    - **Il Range ($1 \le \mu_{eff} \le \mu$):**
	- Se tutti i pesi sono uguali ($w_i = 1/\mu$): $\mu_{eff} = \mu$. Significa che "ascoltiamo" tutti i genitori allo stesso modo (massima informazione, ma bassa pressione selettiva).
    - Se un peso è 1 e gli altri 0: $\mu_{eff} = 1$. Significa che tutto dipende dal singolo figlio migliore (massima selezione, rischio instabilità). 
- **Regole Pratiche (Heuristics):**
    - Solitamente si sceglie $\mu \approx \lambda / 2$ (si tengono metà dei figli).
    - L'obiettivo è avere $\mu_{eff} \approx \lambda / 4$.
    - I pesi decrescono linearmente: $w_i \propto \mu - i + 1$.

#### 6 Aggiornamento della Matrice di Covarianza ($C$) - Parte I

Questo è il passaggio cruciale che permette all'algoritmo di "imparare" la forma del problema (es. capire se c'è una valle stretta e allungare l'ellisse di ricerca in quella direzione).
- L'Intuizione:
    Vogliamo che la nuova matrice $C_{g+1}$ aumenti la probabilità di generare vettori simili a quelli che hanno avuto successo nella generazione corrente ($x_{g+1}$). Se andare a "Nord-Est" ha pagato, deformiamo la ricerca verso "Nord-Est".
- Riscrivere l'aggiornamento della Media:
    Possiamo vedere la nuova media non come una posizione assoluta, ma come la vecchia media più uno spostamento pesato: $$\mathbf{m}_{g+1} = \mathbf{m}_g + \sum_{i=1}^{\mu} w_i (\mathbf{x}_i^{g+1} - \mathbf{m}_g)$$
    (Stiamo sommando i vettori differenza "successo - vecchia media")

- Stima della Covarianza ($C_{\lambda}^{g+1}$):
    Prima di aggiornare la matrice vera e propria, stimiamo la forma della distribuzione attuale basandoci sui campioni appena estratti: $$C_{\lambda}^{g+1} = \frac{1}{\lambda} \sum_{i=1}^{\lambda} (\mathbf{x}_i^{g+1} - \mathbf{m}_g)(\mathbf{x}_i^{g+1} - \mathbf{m}_g)^T$$
    - **Concetto chiave:** Questo è uno **stimatore corretto (unbiased estimator)** di $C_g$. Significa che $E[C_{\lambda}^{g+1}] = C_g$. In parole povere: se avessimo infiniti campioni, questa formula ci ridarebbe esattamente la forma della matrice originale.
#### 7 Stima Migliorata della Covarianza ($C_{\mu}$)
- **L'Idea:** Invece di stimare la forma usando _tutti_ i figli (che darebbe una stima neutra/unbiased), usiamo solo i **migliori $\mu$ figli**. 
- La Formula:$$C_{\mu}^{g+1} = \sum_{i=1}^{\mu} w_i \frac{(x_i^{g+1} - m_g)}{\sigma} \frac{(x_i^{g+1} - m_g)^T}{\sigma}$$
- **Il Significato:**
    - La somma ora arriva fino a $\mu$ (non $\lambda$): stiamo ignorando i figli scarsi.
    - **Effetto:** Campionare da questa matrice $C_{\mu}$ tenderà a riprodurre i passi che hanno avuto successo in questa generazione.

#### 8 Rank-$\mu$ Update (L'Aggiornamento Robusto)
- **Il Problema:** Non possiamo buttare via la vecchia matrice $C_g$ e sostituirla con $C_{\mu}$ ogni volta (sarebbe instabile e dimenticherebbe tutto ciò che ha imparato prima).
- **La Soluzione (Exponential Smoothing):** Uniamo la vecchia conoscenza con la nuova stima.
- La Formula:$$C_{g+1} = (1 - c_{\mu}) C_g + c_{\mu} \frac{1}{\sigma^2} C_{\mu}^{g+1}$$
    - **$C_g$:** La "memoria" storica delle generazioni passate.
    - **$C_{\mu}^{g+1}$:** La "novità" imparata dai migliori figli di oggi.
    - **$c_{\mu}$ (Learning Rate):** Un valore tra 0 e 1. Determina quanto velocemente l'algoritmo impara (o dimentica).
        - $c_{\mu}$ basso $\rightarrow$ Memoria lunga (stabile).
        - $c_{\mu}$ alto $\rightarrow$ Adattamento rapido (ma rischioso)

#### 8 Rank-One Update (L'Aggiornamento Aggressivo)
- **L'Intuizione:** E se invece di fare la media pesata di $\mu$ vettori, ci fidassimo ciecamente **solo del primo della classe** (il figlio migliore assoluto $x_1$)?
- La Formula: $$C_{g+1} = (1 - c_1) C_g + c_1 \frac{(x_1^{g+1} - m_g)}{\sigma} \frac{(x_1^{g+1} - m_g)^T}{\sigma}$$
- **L'Effetto:**
    - Aumenta drasticamente la probabilità di generare vettori lungo la linea dell'unico passo migliore appena fatto.
    - È molto efficiente quando c'è una direzione chiara e dominante da seguire.
#### 9 Evolution Path (Percorso Evolutivo)
- Per aggiornare la matrice di covarianza, si considera una **sequenza di passi successivi** avvenuti nel corso di più generazioni (non solo l'ultima).
#### 10 Il "Vero" Aggiornamento CMA-ES
- L'algoritmo completo si ottiene combinando due tecniche:
    - **Rank-one update:** applicato agli _evolution paths_.
    - **Rank-$\mu$ update:** (quello classico basato sulla media dei migliori).

#### 11 Adattamento di $\sigma$ (Step Size)
- La grandezza del passo $\sigma$ **non è fissa**.
- Esiste una regola di aggiornamento che permette la **self-adaptation** (l'algoritmo regola da solo quanto grandi devono essere i passi).
### 6.2.4 Rissunto
#### Struttura degli Algoritmi Evolutivi (EAs)
- **Ciclo di Base (Scheletro):**
       1. Inizializzazione della popolazione.
    2. Valutazione (Fitness).
    3. Selezione dei genitori.
    4. Variazione (Crossover e Mutazione).
    5. Sostituzione (creazione della nuova generazione) 5.
- **Tipologie Principali:**
    - **Genetic Algorithms:** Usano stringhe di bit; la ricombinazione è il fattore principale.
    - **Genetic Programming:** L'individuo è un programma (es. albero di sintassi); usato per generare codice.
    - **Evolution Strategies (ES):** Ottimizzazione a valori reali, basata su mutazione stocastica (es. rumore Gaussiano.
- **Problemi Comuni:** L'evoluzione può "barare" (Cheating) trovando scorciatoie tecniche che soddisfano la fitness senza risolvere il problema reale (es. cancellare il file target invece di eguagliarlo) o soffrire di "Bloating" (codice spazzatura.

#### Evolution Strategies (ES)
- **Obiettivo:** Ottimizzazione "Black-box" di funzioni continue dove non si conoscono derivate.
- **Self-Adaptation:** È fondamentale evolvere non solo la soluzione ($x$), ma anche i parametri di strategia ($\sigma$, step size) per evitare di rimanere bloccati quando la fitness migliora.
- **Meccanismi di Selezione:**
    - **Selezione a Virgola $(\mu, \lambda)$:** I genitori vengono scartati. Non è elitaria, favorisce l'esplorazione e l'adattamento dinamico del passo (step size). È generalmente preferita per evitare ottimi locali.
    - **Selezione col Più $(\mu + \lambda)$:** I genitori competono con i figli. È elitaria (il migliore sopravvive sempre), ottima per sfruttare una zona (exploitation) ma rischia stagnazione.

#### CMA-ES (Covariance Matrix Adaptation)
- **Funzionamento:** È lo stato dell'arte per l'ottimizzazione continua. Adatta una matrice di covarianza ($C$) per modellare la forma del paesaggio di ricerca (es. trasformando la ricerca da circolare a ellittica).
- **Aggiornamento della Media:** La nuova media è una media pesata dei migliori $\mu$ figli (Selection), spingendo la ricerca verso le zone promettenti.
- **Aggiornamento della Covarianza ($C$):** Combina due metodi:
    - **Rank-$\mu$ update:** Usa le informazioni della popolazione (stabilità).
    - **Rank-one update:** Usa il percorso evolutivo (evolution path) per sfruttare il "momento" e la direzione dei passi precedenti.
- **Controllo del Passo ($\sigma$):** La dimensione del passo si adatta confrontando la lunghezza del cammino percorso con quella di una camminata casuale (Path Length Control).

# 7 Artificial Life
> Evolution of Complexity -> da cellule base a organismi più complessi

**Tierra** -> Programmi software (creature) che competono per il tempo della CPU e vivono nello spazio RAM
**Avida** -> Il software ha risorse dedicate, può acquisirne di più eseguendo attività (ad esempio la moltiplicazione binaria)

> Fitness -> vivere abbastanza a lungo

- **Sensibilità alle condizioni iniziali:** Una variazione infinitesimale all'inizio porta a risultati finali completamente diversi (Caos).
- **Determinismo vs Prevedibilità:** Il sistema segue regole fisse, ma è **imprevedibile** perché è impossibile conoscere lo stato iniziale con precisione perfetta.
- **Conclusione:** Non possiamo calcolare il futuro con una formula, l'unico modo è **eseguire la simulazione** passo dopo passo.

### 7.1.1 Cellular Automata
> Modello matematico semplice di **self-replication** (riproduzione autonoma)

- La descrizione del sistema viene utilizzata per replicare il sistema e per costruirlo
	- Questo è stato concepito prima che il meccanismo del DNA fosse pienamente compreso / scoperto
- **Spazio Discreto** -> multi-dimensional vettore di celle e **tempo discreto**
- 1 variabile discreta per cella
- Regole di Aggiornamento dettano le dinamiche
	- Aggiornamenti sincroni basati su ogni celle dei vicini
- Gestione dei Confine
	- Celle possono rimanere costanti e lo spazio può essere sovrapposto -> toroidal structure
	- Anche il quartiere può essere definito in modo diverso

#### Funzionamento base di un **Automa Cellulare Elementare (1D)** - ECA
- **Struttura:** Una fila di celle che possono essere solo **0** o **1** (bianco o nero).
- **Regola di Aggiornamento:** Lo stato futuro di una cella dipende solo da **se stessa e dai suoi due vicini** (sinistra e destra) al tempo precedente (sj−1,sj,sj+1).
- **La "Regola":** I disegni in basso (111 → 0, 110 → 1, ecc.) definiscono la "legge fisica" di quel mondo. Leggendo i risultati in basso come un numero binario (es. 01011010), si ottiene il numero della Regola (es. Rule 30, Rule 90) che determina se il sistema genererà ordine, caos o strutture frattali.

Assunzioni:
- Uno stato iniziale 0-state deve essere mappato a 0 -> L'ultima cifra deve essere 0
- Riflessione Simmetrica -> isotropia: stesso comportamento che riguarda la direzione
	- 110 -> 011, 100 -> 001
- Regole ammissibili del modulo $𝑏_1𝑏_2𝑏_3𝑏_4𝑏_2𝑏_5𝑏_40$ → 32 possible 

![[image-28.png|488x251]]
![[image-29.png|493x245]]

**Evoluzione**
- Rumore bianco come stato iniziale
	- I valori delle celle non sono correlati tra loro
	- Un parametro p descrive l'intero stato
- Sistema ordinato di conseguenza
	- Più parametri per descriverlo
- Proprietà di auto-organizzazione
	- Un altro segno distintivo di sistemi non lineari complessi
- 300 fasi temporali di evoluzione con regola 126
- Stato iniziale casuale
	- $P(x=1) = 0,5$
- È anche possibile lo studio delle proprietà globali
	- Teoria classica dei sistemi dinamici

##### Regole in ECA
1. Type I
	- Evoluzione verso gli stati statici
2. Type II
	- Evoluzione alla struttura periodica
3. Type III
	- L'evoluzione a schemi caotici -> porta alla (pseudo) casualità
4. Type IV
	- Evoluzione a modelli complessi -> limite del caos

##### CA con k Stati per Cella
> Formazione di membrane protettive che proteggono il loro contenuto dagli effetti esterni
> Quando due membrane si scontrano possono distruggersi a vicenda

![[image-30.png|326x278]]

## 7.2 Universal Computer - Calcolo Universale
#rileggere

Questo capitolo spiega come gli Automi Cellulari (CA) non siano solo generatori di pattern visivi, ma possano fungere da veri e propri sistemi di calcolo.
- **Il Caso dell'ECA 110:** La "Regola 110" degli Automi Cellulari Elementari (identificata da Stephen Wolfram) è un esempio di sistema capace di **computazione universale**.
- **Definizione:** Essere un computer universale significa che, modificando solo l'input iniziale, il sistema è in grado di calcolare qualsiasi funzione computabile. È teoricamente equivalente a una **Macchina di Turing universale**.
- **Nota sulla complessità:** La configurazione necessaria per ottenere la computazione universale non è necessariamente la più semplice possibile4.
**Varianti del modello CA - Variazioni**
Oltre al modello classico, esistono molte varianti che ne estendono le capacità:
- **Non-uniformi:** Celle diverse possono seguire regole di aggiornamento diverse.
- **Non-locali (Learning rules):** La definizione di "vicinato" cambia, ad esempio seguendo la struttura di un grafo (automata network) invece di una griglia fissa.
- **Asincroni:** Le celle non si aggiornano tutte contemporaneamente nello stesso istante (Asynchronous updates).
- **Continui:** Si abbandona la discretizzazione per avere spazio o tempo continui.
### 7.2.1 Extending GoL (Estensione del Game of Life)

Il concetto classico di _Game of Life_ viene generalizzato per superare i limiti discreti.
- **Dalla Discretizzazione alla Continuità:** Si passa da griglie fisse e stati binari (0/1) a modelli che ricordano le _Cellular Neural Networks_ (calcolo analogico/continuo)
- **Definizione Formale:** Un Automa Cellulare (CA) è definito dalla tupla $(L, T, S, N, \phi)$, dove $L$ è il reticolo (spazio), $T$ la linea temporale, $S$ l'insieme degli stati, $N$ il vicinato e $\phi$ la regola locale
- **Obiettivo:** Creare comportamenti emergenti complessi in spazi e tempi non necessariamente discreti

### 7.2.2 Lenia
Lenia è una generalizzazione "continua" degli Automi Cellulari.
- **Continuità Totale:** A differenza del CA classico, in Lenia lo spazio, il tempo e gli stati sono continui (i valori tendono a zero invece di essere discreti: $1/R, 1/T, 1/P \rightarrow 0$)
- **Ingredienti:** La regola di aggiornamento si basa su tre componenti chiave: la distribuzione del Potenziale, la distribuzione della Crescita e il Kernel

### 7.2.3 Potential Distribution - Distribuzione del Potenziale
Rappresenta la "percezione" che una cella ha del suo vicinato.
- **Calcolo:** Si calcola tramite una **convoluzione** tra lo stato attuale della griglia ($A^t$) e un _Kernel_ ($K$): $U^t(x) = K * A^t(x)$
- **Il Kernel ($K$):** Definisce l'area di influenza. Spesso ha la forma di anelli concentrici (shell) definiti da picchi $\beta$ e raggi specifici. Usa la distanza polare dal centro

![[image-31.png|532x207]]

### 7.2.4 Growth Distribution - Distribuzione della Crescita
Determina come cambia lo stato della cella in base al potenziale percepito.
- **Mapping:** È una funzione $G$ che mappa il valore del potenziale $U$ in un valore di crescita nell'intervallo $[-1, 1]$
- **Forma:** Solitamente è una funzione unimodale (una campana Gaussiana) definita da un centro $\mu$ e una larghezza $\sigma$.
- Aggiornamento: Il nuovo stato è dato dallo stato precedente più la crescita moltiplicata per un piccolo intervallo di tempo $\Delta t$, il tutto "clippato" tra 0 e 1:$$A^{t+\Delta t} = \text{clip}_{[0,1]}(A^t + \Delta t \cdot G(U^t))$$
- **Efficienza:** Le convoluzioni vengono calcolate efficientemente nel dominio delle frequenze usando la FFT (Fast Fourier Transform)
![[image-32.png|467x233]]

### 7.2.5 Extended Lenia
È un'espansione ulteriore di Lenia che introduce maggiore complessità e stabilità.
- **Multidimensionale:** Funziona in 3 o più dimensioni, permettendo forme più stabili
- **Kernel Multipli:** Usa diversi kernel, ognuno con il proprio raggio e la propria funzione di crescita, aumentando il comportamento caotico
- **Canali Multipli:** Introduce diversi "canali" (come mondi paralleli o tipi di sostanze) che interagiscono tra loro, permettendo la specializzazione dei ruoli
- **Risultati:** A differenza della Lenia base, la versione estesa mostra fenomeni di **auto-replicazione**, solitoni (strutture che si auto-organizzano) e forme complesse come "serpenti 3D"
### 7.2.6 (Extended) Lenia results + Flow Lenia
- **Risultati Chiave:** A differenza della versione base, la **Extended Lenia** mostra capacità di **auto-replicazione**: i "solitoni" (strutture stabili auto-organizzanti) possono dividersi in due parti che si respingono a vicenda.
- **Strutture complesse:** Emergono forme tridimensionali (es. "3D Snake") capaci di mangiare e crescere, risultato della coordinazione tra canali multipli con dinamiche diverse
- **Flow-Lenia:** Una variante che introduce la **conservazione della massa** nel sistema per facilitare la ricerca evolutiva

### 7.2.7 Biomaker CA – Plant-based lifeforms
Un modello di vita artificiale ispirato alle piante.
- **Elementi dell'Ecosistema:** Include Terra, Aria, Agenti (Semi), celle Immobili e Sole
- **Metabolismo:** Gli agenti acquisiscono energia dai nutrienti della terra (tramite le radici) e dell'aria (tramite le foglie); il Sole rigenera i nutrienti dell'aria, le celle immobili quelli della terra
- **Ciclo Vitale:**
    - **Riproduzione:** Avviene tramite spargimento di semi soggetti a mutazione
    - **Competizione:** La crescita consuma spazio e nutrienti (che sono finiti); le piante morte restituiscono nutrienti al terreno
- Earth (1), Air (2), Seed Agent (3), Immovable (4), Sun (5)
- Ogni cellula percepisce un quartiere locale
![[image-33.png]]

### 7.2.8 Biomaker CA - Meccaniche Generali
- **Azioni:** Le cellule eseguono azioni specifiche (nascere, specializzarsi, muoversi) in base al loro tipo
- **Meta-Evoluzione:** Il sistema usa due livelli di ottimizzazione:
    - Un _Outer loop_ che genera diversi ambienti.
    - Un _Inner loop_ (ottimizzazione ES) che adatta gli agenti a quegli ambienti        
    - **Nota:** Questo processo è computazionalmente molto costoso

### 7.2.9 Particle Swarm ALife
Sistemi basati su agenti puntiformi invece che su griglie.
- **Definizione:** Ogni particella è un pixel con posizione, velocità e colore (che ne identifica la "famiglia")
- **Dinamica:** Il comportamento emerge dalle forze di **attrazione e repulsione** tra le diverse famiglie (es. "il rosso attrae il rosso", "il giallo respinge il rosso")
- **Risultato:** Si creano comportamenti auto-organizzanti come il _flocking_ (stormi) o strutture stabili, gestendo anche le collisioni

### 7.2.10 CA Applications - Applicazioni degli Automi Cellulari
Oltre alla simulazione della vita, i CA hanno usi pratici in ingegneria e informatica:
- **Hardware (VLSI):** Progettazione di circuiti e generatori di numeri pseudo-casuali per l'auto-test dei chip
- **Crittografia:** Sfruttando la generazione di sequenze complesse
- **Image Processing:**
    - Rilevamento dei bordi e riduzione del rumore
    - **Segmentazione:** Segmentare immagini ad alta risoluzione (sfida difficile anche per le reti neurali moderne) usando pochissimi parametri
- **Compressione:** Di testo e immagini
## 7.3 Neural Cellular Automata (NCA)

È un modello che fonde gli Automi Cellulari con il Deep Learning per studiare la **morfogenesi** (come un organismo sviluppa la sua forma) e l'auto-organizzazione.
- **Obiettivo:** Creare sistemi capaci di "coltivare" una forma specifica (es. un'immagine) partendo da un singolo seme e di mantenerla stabile nonostante le perturbazioni
### 7.3.1 Struttura della Cella
Ogni cella non è un semplice valore binario, ma un **vettore di numeri reali** (solitamente 16 canali):
- **Primi 3 canali:** RGB (colore visibile)
- **Quarto canale:** Alpha (trasparenza/vitalità). Se $\alpha > 0.1$ la cella è "viva", altrimenti è "morta" o in crescita 
- **Canali rimanenti:** Stati nascosti (hidden state) per la comunicazione locale.

### 7.3.2 Funzionamento (Architettura)
L'aggiornamento avviene in due fasi cicliche:
- **Percezione:** Ogni cella "vede" il suo vicinato $3 \times 3$ attraverso filtri convoluzionali fissi (es. filtri di Sobel per rilevare gradienti $x$ e $y$)
- **Aggiornamento (Update Rule):**
    - Un **piccolo network neurale** (rete densa/MLP) riceve il vettore di percezione e calcola la variazione di stato $\Delta s$ 
    - La stessa rete è condivisa da **tutte** le celle (come in un CA classico, le regole sono locali e uniformi)
    - **Aggiornamento Stocastico:** Per simulare l'asincronia naturale, ogni cella si aggiorna con una probabilità del 50% a ogni passo

### 7.3.3 Stabilità e Rigenerazione
- **Training:** Si parte da una singola cella viva e si addestra la rete affinché, dopo $N$ passi, la griglia assomigli all'immagine target
- **Pool di Stati:** Per rendere il pattern stabile (un "attrattore"), si usa un pool di stati passati durante l'addestramento. Il sistema impara a correggere errori partendo non solo dall'inizio, ma anche da stati intermedi o degradati 
- **Rigenerazione:** Grazie a questo addestramento robusto, se una parte dell'immagine viene cancellata ("trauma"), le celle rimanenti comunicano per ricostruire la parte mancante

### 7.3.4 Applicazione: Self-classifying MNIST
- Un esempio di **classificazione distribuita**: invece di un classificatore centrale, ogni cella di un'immagine (es. una cifra MNIST) decide autonomamente quale numero rappresenta.
- Le celle devono raggiungere un "accordo" (consenso) tramite interazioni locali. Gli ultimi 10 canali del vettore di stato rappresentano le probabilità delle classi (0-9).

![[image-34.png]]

### 7.3.5 Adding Stability (Aggiungere Stabilità)

Per evitare che l'immagine generata si degradi o cambi una volta completata, bisogna rendere il pattern target un **attrattore** stabile.
- **Il Problema:** Se addestriamo solo partendo dal seme iniziale, la rete non sa cosa fare se l'immagine devia leggermente una volta finita.
- **La Soluzione (Sample Pool):** Si utilizza una "pool" (riserva) di stati passati. Invece di ricominciare sempre da zero, la rete viene addestrata a riprendere da questi stati intermedi o finiti salvati nella pool, imparando a correggerli e mantenerli stabili nel tempo

### 7.3.6 Regenerate after Trauma - Rigenerazione dopo un Trauma
L'NCA è capace di autoripararsi se viene danneggiato
- **Metodo di Training:** Per insegnare questa capacità, durante l'addestramento si applicano danni intenzionali agli stati prelevati dalla pool.
- **Il Danno:** Solitamente si azzera (cancella) una regione circolare casuale dell'immagine. La rete è così costretta a imparare come ricostruire le informazioni mancanti basandosi sulle celle sopravvissute nel vicinato

### 7.3.7 Self-classifying MNIST
In questa applicazione, l'NCA non viene usato per generare immagini, ma per **classificarle** (riconoscere cifre scritte a mano).
- **Consenso Distribuito:** Non c'è un decisore centrale; ogni singola cella vede solo il suo vicinato e propone una predizione (0-9). La classificazione finale emerge dall'**accordo** (consenso) della maggioranza delle celle.

![[image-35.png|546x280]]

Ecco un riassunto strutturato dei concetti chiave del documento "Logical Agents", organizzato per essere inserito direttamente nei tuoi schemi di studio.

---
# 8 LOGICAL AGENTS
#rileggere
## 8.1 Agenti Basati su Conoscenza - Knowledge-Based Agents
Questi agenti mantengono uno **stato interno** (conoscenza) per operare in ambienti parzialmente osservabili.
- **Knowledge Base (KB):** Un insieme di "frasi" (fatti/regole) espresse in un linguaggio formale che rappresentano ciò che l'agente sa1.
- **Approccio Dichiarativo vs Procedurale:**
    - _Dichiarativo:_ Diciamo all'agente _cosa_ deve sapere (TELL) e chiediamo _cosa_ fare (ASK).
    - _Procedurale:_ Codifichiamo direttamente _come_ eseguire le azioni.
- **Ciclo di vita:** L'agente percepisce $\rightarrow$ Aggiorna la KB (TELL) $\rightarrow$ Interroga la KB per decidere l'azione (ASK) $\rightarrow$ Esegue l'azione $\rightarrow$ Aggiorna la KB con l'azione eseguita (TELL).
![[image-36.png|200x295]]

```c
function KB-Agent(percept) returns an action
	static: KB, aknowledge base
			t, a counter, initially 0, indicating time
	Tell(KB, Make-Percept-Sentence(percept, t))
	action <- Ask(KB, Make-Action-Query(t))
	Tell(KB, Make-Action-Sentence(action, t))
	t <- t+1
	return action
```

#### Wupus World
#### 1. Definizione PEAS (Performance, Environment, Actuators, Sensors)
- **Performance:** +1000 per l'oro, -1000 se muori (Wumpus o Pozzo), -1 per ogni passo (incentiva l'efficienza), -10 per scoccare la freccia.
- **Environment:** Una griglia 4x4 di stanze.
    - C'è un **Wumpus** (mostro) che mangia l'agente se entra nella sua stanza.
    - Ci sono dei **Pozzi (Pits)** in cui si cade (morte).
    - C'è dell'**Oro**.
- **Actuators:** Muoversi (Avanti, Gira Dx, Gira Sx), Afferrare (Grab), Tirare (Shoot).
- **Sensors (Cruciale!):** L'agente percepisce solo la casella attuale.
    - **Puzza (Stench):** Se il Wumpus è in una casella adiacente (non diagonale).
    - **Brezza (Breeze):** Se un Pozzo è in una casella adiacente.
    - **Luccichio (Glitter):** Se l'Oro è nella casella attuale.
    - **Urlo (Scream):** Se il Wumpus viene ucciso (si sente ovunque).
    - **Bump:** Se sbatti contro un muro.

#### 2. Perché serve la Logica?
Un agente a riflessi (che agisce solo sull'istante) fallirebbe.
- _Esempio:_ Se sento "Brezza", non so _quale_ casella vicina ha il pozzo. Se mi muovo a caso, muoio.
- **L'Agente Logico** usa la **Knowledge Base (KB)** per accumulare indizi nel tempo.
    - _Passo 1:_ Sono in [1,1], niente puzza/brezza → deduco che [1,2] e [2,1] sono **sicure (OK)**.
    - _Passo 2:_ Vado in [2,1], sento Brezza → deduco che c'è un pozzo in [2,2] O in [3,1].
    - _Passo 3:_ Torno indietro e vado in [1,2], sento Puzza → deduco Wumpus in [2,2] O in [1,3]
    - _Inferenza:_ Incrociando i dati, l'agente può dedurre posizioni sicure che non ha ancora visitato.

### 8.1.2 Logica: Sintassi e Semantica
Per ragionare, serve un linguaggio formale.
- **Sintassi:** Le regole che definiscono quali frasi sono "ben formate" (grammatica).
- **Semantica:** Definisce la "verità" delle frasi rispetto a un **Modello** (un "mondo possibile"). Esempio: "A è vero" significa che nel modello $m$, la proposizione $A$ vale _true_.
- **Entailment (Conseguenza Logica) $KB \models \alpha$:**
    - La frase $\alpha$ è conseguenza logica di $KB$ se e solo se **in tutti** i modelli in cui $KB$ è vera, anche $\alpha$ è vera
    - Formalmente: $M(KB) \subseteq M(\alpha)$ -> ossia l'insieme dei mondi dove KB è vera è contenuto nell'insieme dei mondi dove $\alpha$ è vera
![[image-37.png]]
- Dove $M(\beta)=M(KB)$

**Esempio Wupus**
![[image-38.png|524x242]]
Ora utilizziamo le conoscenze del mondo del wupus e riduciamo le possibili alternative -> passiamo da 8 a 3 possibili
![[image-39.png|524x430]]
### 8.1.3 Inferenza Logica
Il processo per derivare nuove frasi vere da quelle esistenti.
- **Model Checking:** Enumerare tutti i modelli possibili per verificare se $KB \models \alpha$ come ad esempio le Tabelle di Verità.
	- È robusto ma costoso ($O(2^n)$)
	- Consiste nel guardare se l'insieme $KB$ è sottoinsieme della sentenza $\alpha$
- **Soundness (Correttezza):** Un algoritmo è _sound_ se deriva **solo** frasi che sono effettivamente conseguenze logiche (non inventa falsità)
- **Completeness (Completezza):** Un algoritmo è _complete_ se è in grado di derivare **tutte** le frasi vere possibili

### 8.1.4 Logica Proposizionale (PL)
La forma più semplice di logica, basata su simboli atomici ($P, Q$) e connettivi ($\neg, \wedge, \vee, \Rightarrow, \Leftrightarrow$).
- **Validità (Tautologia):** Una frase vera in _tutti_ i modelli (es. $P \vee \neg P$).
- **Soddisfacibilità (SAT):** Una frase vera in _almeno_ un modello.
- **Teorema di Deduzione:** $\alpha \models \beta$ se e solo se $(\alpha \Rightarrow \beta)$ è valida (tautologia)
- **Dimostrazione per Assurdo (Refutazione):** $\alpha \models \beta$ se e solo se $(\alpha \wedge \neg \beta)$ è **insoddisfacibile** (contraddizione). _Questo è il concetto base della Risoluzione._

### 8.1.5 Algoritmi per la PL
Ci sono due approcci principali per decidere l'entailment:
#### A. Model Checking (Controllo sui Modelli)
Un approccio basato sull'enumerazione dei possibili mondi per verificare l'entailment logico (KB⊨α).
1. **Truth Table Enumeration (Enumerazione della Tabella di Verità):**
    - L'algoritmo costruisce la tabella di verità completa per le variabili proposizionali coinvolte.
    - Verifica che in **tutti** i modelli (righe) in cui la _Knowledge Base_ è vera, anche la frase α sia vera.
    - **Proprietà:** È un algoritmo **Sound** (corretto) e **Complete** (completo), ma la complessità temporale è **esponenziale** ($O(2n)$), rendendolo impraticabile per un numero elevato di simboli.
2. **Local Search (Ricerca Locale):**
    - Le slide menzionano che i metodi di ricerca locale (es. WalkSAT, Simulated Annealing) possono essere utilizzati per trovare un modello soddisfabile.

#### B. Theorem Proving (Deduzione / Inferenza)
L'applicazione di regole di inferenza per derivare nuove frasi (teoremi) da quelle esistenti nella Knowledge Base, senza dover enumerare i modelli.
- **Inference Rules (Regole di Inferenza):** Si basano su pattern standard (es. _Modus Ponens_, _And-Elimination_) per generare nuove frasi in modo deduttivo.
- **Monotonicity (Monotonia):** Una proprietà fondamentale della logica: l'insieme delle frasi implicate dalla KB può solo crescere (o restare uguale) aggiungendo nuove informazioni. Aggiungere conoscenza non invalida mai le conclusioni precedenti.
- **Resolution (Risoluzione):**
    - È una regola di inferenza singola che, se usata correttamente, fornisce un algoritmo di inferenza **Sound e Complete** (nota: le slide sottolineano che abbiamo algoritmi completi, e la risoluzione è il principale per la logica proposizionale generale).
    - **CNF (Conjunctive Normal Form):** Per usare la risoluzione, le frasi devono essere convertite in forma congiuntiva normale (congiunzione di disgiunzioni di letterali).
    - **Refutation (Dimostrazione per Contraddizione):** L'algoritmo tipico aggiunge la negazione della tesi (¬α) alla KB e cerca di derivare la **clausola vuota** (contraddizione). Se la si ottiene, allora α deve essere vera.

### 8.1.6 Horn Clauses e Chaining
Un sottoinsieme della logica proposizionale molto efficiente.
- **Clausola di Horn:** Una disgiunzione con **al massimo un** letterale positivo (es. ¬A∨¬B∨C, che equivale a A∧B⇒C).
- **Vantaggio:** L'inferenza con clausole di Horn è lineare $O(N)$ rispetto alla dimensione della KB.

**Algoritmi di Inferenza:**
- **Forward Chaining (Data-Driven):** Parte dai fatti noti per derivare nuove conclusioni ("in avanti"). L'idea è attivare ogni regola le cui premesse sono soddisfatte, aggiungendo la conclusione alla KB.
    - _Implementazione:_ Mantiene un **conteggio** delle premesse mancanti per ogni regola. Quando un simbolo viene inferito, decrementa i contatori delle regole che lo contengono; se un contatore arriva a zero, la regola si attiva.
    - _Pro/Contro:_ È completo, ma può generare molti fatti inutili se non servono all'obiettivo.
    - Partendo da $A$ e $B$ l'unico punto che posso derivare è L dato che gli altri mi ritornano tutti 1 (falso) dato che non sono ancora disponibili.
    - Continua ad esplorare tutto fino a quanto non raggiungo la query $Q$
![[image-40.png|462x288]]
- **Backward Chaining (Goal-Driven):** Lavora all'indietro partendo dalla query q. Se q non è noto, cerca le regole che hanno q come conclusione e tenta di dimostrare le loro premesse (sotto-obiettivi) ricorsivamente.
    - _Struttura:_ Forma un albero di ricerca AND-OR (AND = tutte le premesse di una regola devono essere vere; OR = basta una regola funzionante per provare un fatto).
    - _Pro/Contro:_ Spesso più efficiente in pratica perché evita inferenze irrilevanti (base di Prolog).
    - Quando arrivo a questa situazione ho che $A$ e $B$ sono fatti quindi posso prendere definitivamente anche $L$ e di conseguenza risalgo fino a $Q$
![[image-41.png|487x325]]
    
---
# 9 First-Order Logic - FOL Inference
#rileggere
la Logica Proposizionale (PL) alla Logica del Primo Ordine (FOL) è dettato dalla necessità di maggiore espressività e composizionalità.
- **Ontological Commitment (Cosa esiste):** In FOL il mondo è costituito da **Fatti**, **Oggetti** e **Relazioni** (mentre in PL esistono solo Fatti).
- **Epistemological Commitment (Credenze dell'agente):** Vero/Falso/Sconosciuto.

### 9.1.1 Sintassi e Semantica
- **Simboli:** 
	- Costanti (es. $Bacciu$, $Juventus$)
	- Predicati ($BrotherOf$, $>$)
	- Funzioni ($sqrt$)
	- Variabili ($x, y$)
	- Connettivi ($\land, \lor, \neg, \Rightarrow$)
	- Uguaglianza ($=$)
	- Quantificatori ($\forall, \exists$).
- **Modello:** Contiene uno o più elementi del dominio (oggetti) e le loro relazioni.
- **Interpretazione:** Associa un referente concreto a ogni costante, predicato e funzione.
- **Verità:** Una frase $p(t_1, \dots, t_n)$ è vera _se e sose_ gli oggetti riferiti dai termini $t_i$ sono nella relazione $p$definita dall'interpretazione.

### 9.1.2 Database Semantics & Closed-World Assumption
Per semplificare la rappresentazione della conoscenza (KB), si adottano spesso assunzioni da database:
1. **Closed-World Assumption (CWA):** Le frasi atomiche che non sono note come vere, sono considerate false (niente informazioni "nascoste").
2. **Unique Names Assumption:** Ogni costante, predicato o funzione ha un unico referente univoco.

## 9.2 Instanziazione Universale ed Esistenziale
Queste regole permettono di manipolare i quantificatori per inferire nuove frasi.
### 9.2.1 Universal Instantiation - UI
**Definizione:** Ogni frase ottenuta sostituendo una variabile quantificata universalmente con un _ground term_ (termine senza variabili) è un'inferenza valida.
Regola di Inferenza:
$$\frac{\forall v ~ \alpha}{\text{Subst}(\{v/g\}, \alpha)}$$
dove $v$ è la variabile, $g$ è un ground term.
**Es:** Da $∀𝑥\ 𝐴𝑡𝑡𝑒𝑛𝑑 (𝑥, 𝐴𝐼𝐹)$ possiamo inferire $Attend(Pietro, AIF), Attend(Paola, AIF), …$

### 9.2.2 Existential Instantiation - EI
**Definizione:** Possiamo sostituire una variabile quantificata esistenzialmente con una nuova costante (mai usata prima nel KB).
Regola di Inferenza:
$$\frac{\exists v ~ \alpha}{\text{Subst}(\{v/k\}, \alpha)}$$
dove $k$ è una costante Skolem.
**Dettagli Tecnici:**
- **Skolem Constant:** La costante $k$ "porta in esistenza" un oggetto specifico.
- La nuova KB ottenuta con EI **non è logicamente equivalente** alla KB originale, ma è _soddisfacibile_ se e solo se la KB originale lo era.

## 9.3 Riduzione alla Logica Proposizionale - Propositionalization
**Concetto:** È possibile trasformare una $KB_{FOL}$ in una $KB_{PL}$ eliminando i quantificatori tramite UI ed EI, preservando l'entailment per frasi _ground_.
Teorema di Herbrand (1930):
Se $KB_{FOL} \models \alpha$, allora esiste una $KB_{PL}$ di dimensione finita tale che $KB_{PL} \models \alpha$
**Algoritmo (Idea intuitiva):**
1. Creare $KB_{PL}$ istanziando termini fino a profondità $n=0, 1, 2...$
2. Fermarsi quando la KB implica $\alpha$.

**Problema:** I simboli di funzione inducono infiniti termini ground ($Father(Father(John))...$). Questo rende il processo inefficiente e genera molte frasi inutili. L'entailment in FOL è **semidecidibile** (Turing/Church, 1936).

## 9.4 Unificazione (Unification)
**Definizione:** L'unificazione è il processo che trova una sostituzione $\theta$ che rende identiche due diverse espressioni logiche.
Regola:
$$\text{Unify}(\alpha, \beta) = \theta \iff \alpha\theta = \beta\theta$$
Standardizing Apart:
Se due frasi usano la stessa variabile (es. $x$) ma con significati diversi (ambiti diversi), è necessario rinominare una delle variabili per evitare conflitti prima di unificare.
- Esempio: $\text{Unify}(Knows(John, x), Knows(y, OJ)) = \{x/OJ, y/John\}$

## 9.5 Generalized Modus Ponens (GMP)
**Definizione:** È una versione "lifted" (elevata) del Modus Ponens che lavora direttamente con variabili e quantificatori tramite unificazione, applicata a **Definite Clauses** (clausole con esattamente un letterale positivo).
Regola di Inferenza:
$$\frac{p'_1, p'_2, \dots, p'_n, (p_1 \land p_2 \land \dots \land p_n \Rightarrow q)}{q\theta}$$
dove $\text{Unify}(p'_i, p_i) = \theta$ per ogni $i$.
**Proprietà:**
- **Soundness:** Il GMP è corretto (sound).
- **Efficienza:** Evita la generazione di infinite frasi proposizionali irrelevanti.

## 9.6 Forward & Backward Chaining
Algoritmi applicabili a KB composte solo da **Definite Clauses** (es. Horn Clauses). In questo sottoinsieme della FOL, l'inferenza è ancora semidecidibile.
### 9.6.1 Forward Chaining (FC)
**Funzionamento:** Approccio _data-driven_. Si parte dai fatti noti, si attivano tutte le regole le cui premesse sono soddisfatte e si aggiungono le conclusioni alla KB. Si ripete finché non si deriva la query o non ci sono più fatti nuovi.
- **Matching:** È la parte costosa (NP-hard nel caso generale) perché bisogna confrontare ogni premessa con ogni fatto.
- **Proprietà:** È **Sound** e **Complete** per le clausole definite.

### 9.6.2 Backward Chaining (BC)
**Funzionamento:** Approccio _goal-driven_. Si parte dalla query $q$ e si cercano regole che hanno $q$ come conclusione. Si generano nuove sottoproposizioni (premesse) ricorsivamente.
- **Struttura:** Ricerca Depth-First (DFS), AND-OR search.
- **Proprietà:** È **Sound**, ma **Not Complete** (può finire in loop infiniti o spazi infiniti se non controllata).
- **Utilizzo:** Base per la _Logic Programming_.

### 9.6.3 Esempio "Crime" (Slide 27-31)
_Problema:_ Provare `Criminal(West)`.
- **FC:** Parte da `Missile(M1)`, `Owns(Nono, M1)`, `American(West)` etc., e sale fino a derivare `Criminal(West)`.
- **BC:** Parte da `Criminal(West)`, cerca una regola che implica `Criminal(x)`, unifica $\{x/West\}$, e procede a verificare le premesse (`American(West)`, `Weapon(y)`, etc.).

## 9.7 Logic Programming (Prolog)
**Definizione:** La computazione viene vista come inferenza logica su una KB.
- I programmi sono insiemi di clausole nella forma: `head :- literal1, ..., literaln`.

**Funzionamento (Prolog):**
- Implementa **Backward Chaining** su clausole di Horn.
- Usa ottimizzazioni e può gestire aritmetica.
- **Closed-World Assumption:** `alive(X) :- notDead(X)` (Negation as failure).

**Limiti:**
- Prolog è **incompleto** per clausole definite a causa della strategia di ricerca DFS (può bloccarsi in rami infiniti dove una BFS troverebbe la soluzione).

## 9.8 Resolution (Risoluzione)
**Definizione:** Un algoritmo di inferenza **completo** per tutta la FOL (non solo clausole definite), basato sulla prova per contraddizione.
Regola di Inferenza (Lifted Resolution):
$$\frac{p_1 \lor \dots \lor p_k, \quad m_1 \lor \dots \lor m_n}{(p_1 \lor \dots \lor p_{i-1} \lor p_{i+1} \lor \dots \lor m_1 \lor \dots \lor m_{j-1} \lor m_{j+1} \lor \dots)\theta}$$
dove $\text{Unify}(p_i, \neg m_j) = \theta$.

### 9.8.1 Algoritmo: Proof by Contradiction
Per provare che $KB \models \alpha$, si aggiunge $\neg \alpha$ alla KB e si cerca di derivare la clausola vuota (contraddizione) convertendo tutto in **CNF (Conjunctive Normal Form)**.
### 9.8.2 Conversione in CNF (Passaggi Dettagliati)
1. **Eliminare Implicazioni:** $A \Rightarrow B$ diventa $\neg A \lor B$.
2. **Spostare la Negazione all'interno:** Usare le leggi di De Morgan e $\neg \forall x p \equiv \exists x \neg p$.
3. **Standardizzare le Variabili:** Rinominare le variabili affinché ogni quantificatore usi un nome diverso.
4. **Skolemization:**
    - Sostituire variabili quantificate esistenzialmente ($\exists$) con Costanti di Skolem o **Funzioni di Skolem**.
    - Se l'esistenziale è dentro un universale ($\forall x \exists y$), $y$ diventa $F(x)$ (funzione dell'universale che lo precede).
5. **Rimuovere Quantificatori Universali:** Assumere implicitamente che tutte le variabili rimaste siano $\forall$.
6. **Distribuire $\land$ su $\lor$:** Per ottenere una congiunzione di disgiunzioni.

### 9.8.3 Proprietà della Risoluzione
- **Completezza:** Se $KB \models \alpha$, la risoluzione derivarà una contraddizione (Teorema di completezza di Gödel/Robinson).
- **Semidecidibilità:** Se la frase _non_ è vera, la risoluzione potrebbe non fermarsi mai (loop infinito). Non possiamo sapere in tempo finito se "non esiste prova".
## 9.9 Ontologie e Semantic Web

**Definizione:** Rappresentazione formale della conoscenza di un dominio specifico.
- **Struttura:** Categorie (Classi), Sottocategorie, Proprietà degli oggetti (Relazioni), Istanze (Individui).
- **Vantaggi:** Controllo automatico della validità, derivazione di nuove proprietà, standardizzazione.
- **Linguaggio:** FOL è il linguaggio base delle ontologie.

**Strumenti citati:**
- **Protégé:** Editor open-source per ontologie (Stanford).
- **SPARQL:** Linguaggio di query per ontologie.
- **Esempi:** Wikidata (DB strutturato per Wikipedia), ConceptNet (Semantic network per common sense).
---
# 10 Planning (Classical & Extensions)

## 10.1 Classical Planning e PDDL

### 10.1.1 Definizione di Classical Planning
Il compito di trovare una sequenza di azioni per raggiungere un obiettivo in un ambiente discreto, deterministico, statico e completamente osservabile1.
A differenza degli agenti logici puri o basati su ricerca generica, il Classical Planning utilizza linguaggi ad-hoc e sfrutta sia l'inferenza logica che la ricerca.

### 10.1.2 Planning Domain Definition Language (PDDL)
Linguaggio standard (1998) basato su Lisp
- **Semantica:** "Database semantics" con **Closed-world assumption** (ciò che non è specificato è falso) e **Unique-names assumption** (nomi diversi indicano oggetti diversi)
- **Struttura:** Ogni programma PDDL distingue tra:
    - **Dominio:** Conoscenza comune a tutti i problemi (schema delle azioni)
    - **Problema:** Conoscenza specifica dell'istanza (stato iniziale e goal)

### 10.1.3 Stati in PDDL
**Definizione:** Uno stato è una congiunzione di **ground atomic fluents**
- **Ground:** Nessuna variabile (es. $At(Truck, Pisa)$ è ground, $At(Truck, x)$ no)
- **Atomic:** Singolo predicato senza connettivi logici complessi
- **Fluents:** Aspetti del mondo che cambiano nel tempo

### 10.1.4 Azioni in PDDL
Le azioni sono definite tramite un **Action Schema** che include:
1. **Action name** e lista di variabili.
2. **Precondition:** Congiunzione di letterali (positivi o negati) che devono essere veri affinché l'azione sia applicabile.
3. **Effect:** Congiunzione di letterali che descrive come cambia lo stato.

**Esempio (Schema):**
Snippet di codice
```
Action(Fly(p, from, to),
    PRECOND: At(p, from) \land Plane(p) \land Airport(from) \land Airport(to)
    EFFECT: \neg At(p, from) \land At(p, to))
```

## 10.2 Definizione del Problema ed Esecuzione
### 10.2.1 Specifica del Problema
Un problema richiede, oltre al dominio, la definizione di:
- **Initial state:** Congiunzione di fluenti ground
- **Goal state:** Congiunzione di letterali (può contenere negazioni)

### 10.2.2 Applicabilità ed Esecuzione
Regola di Applicabilità:
Data un'azione con precondizione $P$, l'azione è applicabile in uno stato $s$ sse:
$$S \models P$$
Ogni letterale positivo in $P$ deve valere in $s$ e ogni letterale negato in $P$ non deve essere presente in $s$
Risultato dell'Azione:
Il nuovo stato $s'$ è calcolato come:
$$s' = Result(s, a) = (s - DEL(a)) \cup ADD(a)$$

- **DEL(a):** Fluenti che appaiono negati negli effetti di $a$ (vengono rimossi).
- **ADD(a):** Fluenti che appaiono positivi negli effetti di $a$ (vengono aggiunti)

## 10.3 Algoritmi di Planning (Search)
### 10.3.1 Forward State-Space Search
Ricerca nello spazio degli stati partendo dallo stato iniziale (Progression search):
1. Inizia allo stato iniziale.
2. Unifica lo stato corrente con le precondizioni di ogni schema di azione per trovare le azioni applicabili.
3. Applica la sostituzione risultante per ottenere un'azione ground.
4. Applica l'azione e genera il nuovo stato

### 10.3.2 Backward Search
Ricerca partendo dal goal verso lo stato iniziale (Regression search):
1. Considera le **azioni rilevanti**: un'azione è rilevante se un suo effetto unifica con uno dei letterali del goal e nessuno dei suoi effetti nega parti del goal.
2. Applica le azioni a ritroso fino a raggiungere lo stato iniziale.
    **Vantaggio:** Il _branching factor_ è ridotto rispetto alla forward search perché si considerano solo percorsi rilevanti per il goal

## 10.4 Euristiche e Pruning
Per migliorare l'efficienza della ricerca si usano euristiche basate su **problemi rilassati**
### 10.4.1 Heuristics
1. **Ignore Precondition Heuristic:**
    - Ogni azione è applicabile (si ignorano le precondizioni)
    - Si cerca il numero minimo di azioni i cui effetti uniti soddisfano il goal. 
    - **Proprietà:** È un'euristica **ammissibile** (sottostima sempre la lunghezza della soluzione)
2. **Ignore Delete List Heuristic:**
    - Rimuove i letterali negativi dagli effetti delle azioni
    - Le azioni non possono annullare passi precedenti (progresso monotonico).
    - Spesso usata con Hill-Climbing, ma non garantisce la soluzione ottima (approssimazione)

### 10.4.2 Pruning (Potatura)
1. **Symmetry Reduction:** Elimina i rami simmetrici dell'albero di ricerca, mantenendone solo uno (es. l'ordine di oggetti indistinguibili non conta)
2. **Forward Pruning:** Taglia rami basandosi su una "preferred action" derivata da un piano rilassato
    - **Nota:** Si rischia di potare la soluzione ottima

### 10.4.3 Riduzione dello Spazio degli Stati
- **State Abstraction:** Mapping _many-to-one_ dagli stati a una rappresentazione astratta (rimuovendo alcuni fluenti) per ottenere soluzioni approssimate
- **Decomposition:** Divide il problema in parti (sotto-goal). Si basa sulla **Subgoal independence assumption**: il costo totale è approssimato dalla somma dei costi dei sotto-goal risolti indipendentemente

## 10.5 Hierarchical Planning
Per mitigare l'esplosione delle azioni, si usa l'astrazione gerarchica.
### 10.5.1 Concetti Chiave
- **High Level Action (HLA):** Un'azione astratta (es. $Go(Home, Rome)$) che ammette uno o più **refinements**(raffinamenti) in sequenze di azioni
- **Refinement:** Implementazione di un HLA tramite azioni primitive (o altre HLA).
- **High Level Plan (HLP):** Una sequenza di HLA

### 10.5.2 Hierarchical Forward Planning
Algoritmo che parte da un HLP contenente una singola HLA "Act". Usa una ricerca **Breadth-First** per trovare i possibili raffinamenti di ogni HLA nel piano corrente fino a raggiungere azioni primitive

## 10.6 Estensioni: Sensorless e Online Planning
### 10.6.1 Sensorless Planning (Incertezza)
- **Belief State:** L'agente mantiene un insieme di stati possibili in cui potrebbe trovarsi
- **Open-world assumption:** Se un fluente non appare, il suo valore è **sconosciuto** (differenza cruciale rispetto al PDDL classico)
- **Conditional Effect:** Effetti nella forma "when _condition_: _effect_".
    - L'effetto si applica solo se la _condition_ (formula logica) è verificata nel belief state corrente

### 10.6.2 Online Planning (Execution Monitoring)
Utile in ambienti non deterministici. Richiede monitoraggio e replanning.
Tre approcci di monitoraggio:
1. **Action monitoring:** Verifica che le precondizioni valgano prima di eseguire l'azione
2. **Plan monitoring:** Verifica che il piano rimanente possa ancora avere successo
3. **Goal monitoring:** Controlla se esiste un set di obiettivi migliore da perseguire

## 10.7 Tempo, Schedulazione e Risorse
Il classical planning decide _cosa_ fare; lo scheduling decide _quando_. Approccio: "Plan first, schedule later".
### 10.7.1 Rappresentazione e Vincoli
- **Risorse:** Possono essere consumabili o riutilizzabili. Si usa l'aggregazione numerica (es. $Inspectors(2)$) per ridurre la complessità.
- **Partial-Order Plan:** Un piano è un grafo diretto che permette azioni in parallelo

### 10.7.2 Scheduling (CPM - Critical Path Method)
- **Critical Path:** Il percorso nel grafo con la durata totale più lunga. Determina la durata dell'intero piano
- **Slack (Margine di scorrimento):** Finestra temporale in cui un'azione fuori dal percorso critico può essere eseguita.
    - Definito da $ES$ (Earliest Start) e $LS$ (Latest Start).
    - Le azioni sul percorso critico hanno **Slack = 0**
- **Euristiche:** Minimum slack heuristic (schedula prima le azioni con meno slack)