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
