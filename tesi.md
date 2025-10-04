### **Introduzione ai Problemi di Flusso di Costo Minimo e Loro Varianti**

Il presente lavoro si propone di analizzare in modo rigoroso e dettagliato i problemi di flusso di costo minimo, partendo dalla loro formalizzazione matematica fino a esplorare alcune importanti variazioni. L'obiettivo è fornire un quadro completo che spazia dalla definizione del modello di base all'inquadramento di problemi specifici come casi particolari, gettando le basi per la comprensione di modelli più complessi e dei relativi metodi risolutivi.

---

#### **1. Il Problema di Flusso di Costo Minimo: Modello Matematico e Condizioni di Ottimalità**

Il problema di flusso di costo minimo (Minimum Cost Flow Problem - MCFP) è un problema di ottimizzazione fondamentale che consiste nel trovare il modo meno costoso per inviare un certo quantitativo di flusso attraverso una rete.

**1.1. Formulazione Matematica**

Per definire formalmente il problema, consideriamo un grafo orientato `D = (V, A)`, dove `V` è l'insieme dei nodi e `A` è l'insieme degli archi. A ogni elemento della rete sono associati i seguenti parametri:

*   **Costo per unità di flusso:** Per ogni arco `(i, j) ∈ A`, è definito un costo `c_ij` per spedire un'unità di flusso.
*   **Capacità dell'arco:** Ogni arco `(i, j) ∈ A` ha una capacità massima `u_ij` che rappresenta la quantità massima di flusso che può transitare.
*   **Bilancio dei nodi:** A ogni nodo `i ∈ V` è associato un valore `b_i` che rappresenta il bilancio di flusso.
    *   Se `b_i > 0`, il nodo `i` è un'**offerta** (sorgente) e immette `b_i` unità di flusso nella rete.
    *   Se `b_i < 0`, il nodo `i` è una **domanda** (pozzo) e richiede `|b_i|` unità di flusso dalla rete.
    *   Se `b_i = 0`, il nodo `i` è un nodo di **transito**.

Una condizione necessaria per l'esistenza di un flusso ammissibile è che la somma totale delle offerte sia uguale alla somma totale delle domande, ovvero `Σ b_i = 0` per ogni `i` in `V`.

Le variabili decisionali del problema sono `x_ij`, che rappresentano la quantità di flusso sull'arco `(i, j) ∈ A`.

Il modello di programmazione lineare per il problema di flusso di costo minimo è il seguente:

**Minimizzare:**
`Σ_((i,j)∈A) c_ij * x_ij` (Funzione di costo totale)

**Sotto i vincoli:**
1.  `Σ_(j:(i,j)∈A) x_ij - Σ_(k:(k,i)∈A) x_ki = b_i` per ogni `k ∈ V` (Vincoli di bilancio o di conservazione del flusso)
2.  `0 ≤ x_ij ≤ u_ij` per ogni `(i, j) ∈ A` (Vincoli di capacità)

I vincoli di bilancio assicurano che per ogni nodo, il flusso totale uscente meno il flusso totale entrante sia uguale al suo bilancio. I vincoli di capacità garantiscono che il flusso su ogni arco non superi la sua capacità massima.

**1.2. Condizioni di Ottimalità e Derivazione dalle Condizioni di Karush-Kuhn-Tucker (KKT)**

Essendo il problema di flusso di costo minimo un problema di programmazione lineare, le sue condizioni di ottimalità possono essere derivate dalle condizioni di Karush-Kuhn-Tucker (KKT). Le condizioni KKT sono condizioni necessarie per l'ottimalità di una soluzione in problemi di programmazione non lineare con vincoli di disuguaglianza. Per i problemi di programmazione lineare, queste condizioni sono anche sufficienti.

Associamo una variabile duale (moltiplicatore) `π_i` a ogni vincolo di bilancio e una variabile duale `μ_ij` a ogni vincolo di capacità superiore. Il problema duale associato è:

**Massimizzare:**
`Σ_(i∈V) b_i * π_i - Σ_((i,j)∈A) u_ij * μ_ij`

**Sotto i vincoli:**
*   `π_j - π_i - μ_ij ≤ c_ij` per ogni `(i,j) ∈ A`
*   `μ_ij ≥ 0` per ogni `(i,j) ∈ A`

Le condizioni di ottimalità di KKT, che mettono in relazione la soluzione primale `x` e la soluzione duale `(π, μ)`, sono:

1.  **Ammissibilità primale:** I vincoli del problema primale devono essere soddisfatti.
2.  **Ammissibilità duale:** I vincoli del problema duale devono essere soddisfatti.
3.  **Condizioni di complementarietà (o scarto complementare):**
    *   `x_ij * (c_ij - π_j + π_i + μ_ij) = 0` per ogni `(i,j) ∈ A`
    *   `μ_ij * (u_ij - x_ij) = 0` per ogni `(i,j) ∈ A`

Queste condizioni forniscono un criterio per determinare se una data soluzione di flusso è ottima. In particolare, definendo il **costo ridotto** `c'_ij = c_ij - π_i + π_j`, le condizioni di complementarietà implicano che per un flusso `x` e dei potenziali `π` ottimali:

*   Se `0 < x_ij < u_ij`, allora `c'_ij = 0`.
*   Se `x_ij = 0`, allora `c'_ij ≥ 0`.
*   Se `x_ij = u_ij`, allora `c'_ij ≤ 0`.

---

#### **2. Variazioni dei Problemi di Flusso di Costo Minimo**

Molti problemi di ottimizzazione su grafi possono essere formulati come casi particolari del problema di flusso di costo minimo. Di seguito ne analizziamo tre di fondamentale importanza.

**2.1. Problema del Cammino Minimo (Shortest Path Problem)**

Il problema del cammino minimo consiste nel trovare il percorso di costo (o lunghezza) minimo tra un nodo sorgente `s` e un nodo destinazione `t` in un grafo orientato.

Questo problema può essere modellato come un problema di flusso di costo minimo nel seguente modo:

*   Si imposta un'offerta di 1 unità al nodo sorgente `s` (`b_s = 1`).
*   Si imposta una domanda di 1 unità al nodo destinazione `t` (`b_t = -1`).
*   Tutti gli altri nodi hanno un bilancio nullo (`b_i = 0` per `i ≠ s, t`).
*   La capacità di ogni arco è impostata a 1.
*   Il costo `c_ij` di ogni arco `(i, j)` corrisponde alla sua lunghezza o costo nel problema originale.

Risolvendo questo problema di flusso di costo minimo, si otterrà un flusso unitario lungo gli archi che costituiscono il cammino minimo da `s` a `t`.

**2.2. Problema dell'Albero Ricoprente di Costo Minimo (Minimum Spanning Tree Problem)**

Dato un grafo non orientato e connesso con pesi sugli archi, il problema dell'albero ricoprente di costo minimo consiste nel trovare un sottoinsieme di archi che connetta tutti i nodi senza formare cicli e la cui somma dei pesi sia minima.

La formulazione del problema dell'albero di copertura di costo minimo come un problema di flusso di costo minimo è meno diretta rispetto al cammino minimo. In generale, non è considerato un'istanza diretta del modello di flusso standard, ma le sue condizioni di ottimalità, come la proprietà del taglio e la proprietà del ciclo, sono strettamente correlate ai principi duali presenti nei problemi di flusso.

**2.3. Problema di Assegnamento (Assignment Problem)**

Il problema di assegnamento riguarda l'assegnazione ottimale di un insieme di agenti a un insieme di compiti, dove a ogni possibile assegnazione è associato un costo. L'obiettivo è trovare un'assegnazione uno-a-uno che minimizzi il costo totale.

Questo problema può essere modellato su un grafo bipartito `G = (U ∪ V, A)`, dove `U` rappresenta l'insieme degli agenti e `V` l'insieme dei compiti. Un arco `(u, v)` con `u ∈ U` e `v ∈ V` rappresenta la possibile assegnazione dell'agente `u` al compito `v`, con un costo associato `c_uv`.

La formulazione come problema di flusso di costo minimo è la seguente:

*   Si crea un nodo sorgente `s` e un nodo pozzo `t`.
*   Per ogni agente `u ∈ U`, si aggiunge un arco `(s, u)` con capacità 1 e costo 0. Ogni nodo in `U` ha un'offerta di 1.
*   Per ogni compito `v ∈ V`, si aggiunge un arco `(v, t)` con capacità 1 e costo 0. Ogni nodo in `V` ha una domanda di 1.
*   Gli archi originali `(u, v)` del grafo bipartito mantengono i loro costi `c_uv` e hanno una capacità di 1.

Risolvendo questo problema di flusso, si ottiene un flusso intero che corrisponde all'assegnazione ottimale. Il fatto che il problema di assegnamento sia un caso particolare del flusso di costo minimo ne garantisce la risolvibilità in tempo polinomiale.