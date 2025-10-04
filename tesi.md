

### **Modello Matematico per il Problema di Flusso a Costo Minimo (MCFP)**

Questo documento presenta in modo schematico il modello matematico per il problema di flusso a costo minimo e le sue condizioni di ottimalità, utilizzando una notazione consistente con la letteratura specialistica (es. Mastroeni & Pappalardo). Vengono inoltre analizzate le formulazioni di problemi noti come casi particolari del MCFP.

---

#### **1. Formulazione del Problema di Flusso di Costo Minimo**

Consideriamo un grafo orientato `D = (V, A)`, dove `V` è l'insieme dei nodi e `A` è l'insieme degli archi.

**1.1. Parametri del Modello**

*   **Matrice di incidenza nodo-arco (`Γ`):** Una matrice `|V| × |A|` i cui elementi `γ_ij` sono definiti come:
    *   `γ_ij = -1` se il nodo `i` è il nodo iniziale dell'arco `j`.
    *   `γ_ij = +1` se il nodo `i` è il nodo finale dell'arco `j`.
    *   `γ_ij = 0` altrimenti.
*   **Bilancio dei nodi (`q`):** Un vettore `q` di dimensione `|V|`. Per ogni nodo `j ∈ V`:
    *   Se `q_j < 0`, il nodo `j` è un'**offerta** (sorgente) e immette `|q_j|` unità di flusso.
    *   Se `q_j > 0`, il nodo `j` è una **domanda** (pozzo) e richiede `q_j` unità di flusso.
    *   Se `q_j = 0`, il nodo `j` è un nodo di **transito**.
    *   Condizione di ammissibilità: `Σ_(j∈V) q_j = 0`.
*   **Costo per unità di flusso (`c`):** Un vettore `c` di dimensione `|A|`. Per ogni arco `(i, j) ∈ A`, `c_ij` è il costo per spedire un'unità di flusso.
*   **Capacità degli archi (`d`):** Un vettore `d` di dimensione `|A|`. Per ogni arco `(i, j) ∈ A`, `d_ij` è la quantità massima di flusso che può transitare.

**1.2. Variabili Decisionali**

*   **Flusso sugli archi (`f`):** Un vettore `f` di dimensione `|A|`, dove `f_ij` rappresenta la quantità di flusso sull'arco `(i, j) ∈ A`.

**1.3. Modello di Programmazione Lineare**

Il modello matematico per il MCFP è:

**Minimizzare:**
`Σ_((i,j)∈A) c_ij * f_ij` (o in forma vettoriale: `c^T f`)

**Sotto i vincoli:**
1.  `Γf = q` (Vincoli di bilancio / conservazione del flusso)
2.  `0 ≤ f ≤ d` (Vincoli di capacità)

---

#### **2. Condizioni di Ottimalità e Problema Duale**

Le condizioni di ottimalità per un problema di programmazione lineare legano la soluzione del problema primale a quella del suo duale tramite le condizioni di scarto complementare.

**2.1. Problema Duale e Variabili Duali**

Associamo le seguenti variabili duali ai vincoli del primale:
*   Un vettore `λ` (chiamati **potenziali di nodo**) per i vincoli di bilancio `Γf = q`.
*   Un vettore `μ` per i vincoli di capacità superiore `f ≤ d`.

Il problema duale associato al MCFP è:

**Massimizzare:**
`q^T λ - d^T μ`

**Sotto i vincoli:**
*   `-Γ^T λ - μ ≤ c`
*   `μ ≥ 0`

**2.2. Costi Ridotti e Condizioni di Ottimalità**

La condizione del duale `-Γ^T λ - μ ≤ c` può essere riscritta per ogni arco `(i, j) ∈ A` come `λ_j - λ_i - μ_ij ≤ c_ij`. Definiamo il **costo ridotto** `c'_ij` dell'arco `(i, j)` rispetto ai potenziali `λ` come:

`c'_ij = c_ij + λ_i - λ_j`

Le condizioni di ottimalità (o di scarto complementare) che legano la soluzione primale `f` e duale (`λ`, `μ`) sono:

1.  `f_ij > 0  =>  c_ij + λ_i - λ_j = μ_ij` (Il vincolo duale è attivo)
2.  `μ_ij > 0  =>  f_ij = d_ij` (Il vincolo primale di capacità è attivo)

Combinando queste condizioni, otteniamo un criterio pratico per verificare l'ottimalità di un flusso ammissibile `f` dato un insieme di potenziali `λ`:

*   **Se `c'_ij > 0`**: Il costo per mandare flusso è positivo. Per minimizzare i costi, non si invia flusso. Quindi `f_ij = 0`.
*   **Se `c'_ij < 0`**: Il costo ridotto è negativo, quindi conviene inviare quanto più flusso possibile. Quindi `f_ij = d_ij`.
*   **Se `c'_ij = 0`**: Il flusso non altera il costo ridotto, quindi può assumere qualsiasi valore ammissibile. `0 ≤ f_ij ≤ d_ij`.

Riepilogando, una soluzione `f` è ottima se e solo se esistono dei potenziali di nodo `λ` tali che per ogni arco `(i, j)`:
*   Se `0 < f_ij < d_ij`, allora `c'_ij = 0`.
*   Se `f_ij = 0`, allora `c'_ij ≥ 0`.
*   Se `f_ij = d_ij`, allora `c'_ij ≤ 0`.

---

#### **3. Variazioni dei Problemi di Flusso di Costo Minimo**

**3.1. Problema del Cammino Minimo (Shortest Path Problem)**

Dati un nodo sorgente `s` e un nodo destinazione `t`.
*   **Bilancio dei nodi:**
    *   `q_s = -1` (offerta di 1 unità).
    *   `q_t = 1` (domanda di 1 unità).
    *   `q_j = 0` per ogni altro nodo `j`.
*   **Capacità:** `d_ij = 1` per ogni arco `(i, j) ∈ A`.
*   **Costi:** `c_ij` corrisponde alla lunghezza/costo dell'arco.
La soluzione `f` avrà valore 1 sugli archi del cammino minimo e 0 altrove.

**3.2. Problema dell'Albero Ricoprente di Costo Minimo (Minimum Spanning Tree Problem)**

Questo problema, definito su grafi non orientati, non è generalmente formulato come un'istanza diretta del MCFP standard. Tuttavia, le sue condizioni di ottimalità (proprietà del taglio e del ciclo) sono concettualmente legate ai principi di dualità dei problemi di flusso.

**3.3. Problema di Assegnamento (Assignment Problem)**

Dato un grafo bipartito `G = (U ∪ V, A)`, dove `U` sono agenti e `V` sono compiti.
*   **Bilancio dei nodi:**
    *   Per ogni agente `u ∈ U`, `q_u = -1` (offerta di 1 unità).
    *   Per ogni compito `v ∈ V`, `q_v = 1` (domanda di 1 unità).
    *   Si assume `|U| = |V|`.
*   **Capacità:** `d_uv = 1` per ogni arco `(u, v) ∈ A`.
*   **Costi:** `c_uv` è il costo di assegnare l'agente `u` al compito `v`.
La soluzione intera `f` indica le assegnazioni ottimali (se `f_uv = 1`, `u` è assegnato a `v`).
### **4. Generalizzazione tramite Inequazioni Variazionali (VI)**

Il modello di flusso a costo minimo (MCFP) può essere generalizzato per includere situazioni più complesse, come ad esempio i costi che dipendono dalla congestione della rete (cioè, il costo su un arco dipende dal flusso che vi transita). Questo tipo di problemi non è più lineare e viene elegantemente formulato attraverso il formalismo delle **inequazioni variazionali**.

#### **4.1. Il Problema dell'Inequazione Variazionale per Flussi su Rete**

Il modello generalizzato non cerca di minimizzare una funzione di costo totale, ma piuttosto di trovare uno stato di **equilibrio**. Un flusso `f*` è in equilibrio se nessun utente ha incentivo a cambiare unilateralmente il proprio percorso, dato il flusso degli altri. Questa condizione può essere espressa matematicamente come un'inequazione variazionale.

**4.1.1. Modifiche al Modello**

*   **Funzione di costo (`c(f)`):** Il costo non è più un vettore costante `c`, ma una funzione `c(f)` che mappa un vettore di flusso `f` a un vettore di costi. La componente `c_ij(f)` rappresenta il costo sull'arco `(i, j)` quando il flusso sull'intera rete è `f`. Si assume `c(f) ≥ 0`.
*   **Insieme ammissibile (`K_f`):** L'insieme dei flussi ammissibili rimane definito dagli stessi vincoli lineari del problema classico:
    `K_f := {f ∈ R^|A| : Γf = q, 0 ≤ f ≤ d}`

**4.1.2. Formulazione come Inequazione Variazionale (VI)**

Il problema di equilibrio del flusso di rete consiste nel trovare un flusso `f* ∈ K_f` tale che:

`(c(f*), f - f*) ≥ 0`, per ogni `f ∈ K_f`

dove `(a, b)` denota il prodotto scalare tra i vettori `a` e `b`.

**Interpretazione:** Questa condizione significa che, allo stato di equilibrio `f*`, qualsiasi deviazione infinitesimale verso un altro flusso ammissibile `f` non può portare a una diminuzione del costo. `f*` è un punto stazionario.

#### **4.2. Relazione con il MCFP Classico**

Il modello MCFP standard è un caso particolare del modello VI.
Se la funzione di costo `c(f)` è indipendente dal flusso, cioè `c(f) = c` (un vettore di costanti), allora l'inequazione variazionale diventa:

`(c, f - f*) ≥ 0`  =>  `c^T f - c^T f* ≥ 0`  =>  `c^T f ≥ c^T f*`

Questa è esattamente la condizione di ottimalità per il problema di programmazione lineare:
`min { c^T f | f ∈ K_f }`
Il flusso `f*` è la soluzione che minimizza la funzione di costo totale `c^T f` sull'insieme ammissibile `K_f`.

#### **4.3. Condizioni di Ottimalità per l'Inequazione Variazionale**

Similmente al caso lineare, la soluzione di un'inequazione variazionale su un poliedro può essere caratterizzata da condizioni di tipo KKT. Queste condizioni generalizzano quelle viste per il MCFP.

Un flusso `f*` è una soluzione dell'inequazione variazionale se e solo se esistono dei vettori duali `λ*` (potenziali di nodo) e `μ*` (moltiplicatori per le capacità) tali che la terna `(f*, λ*, μ*)` soddisfa il seguente sistema (cfr. Proposizione 3.1 in Mastroeni & Pappalardo):

1.  **Ammissibilità primale:**
    *   `Γf* = q`
    *   `0 ≤ f* ≤ d`
2.  **Ammissibilità duale:**
    *   `μ* ≥ 0`
3.  **Condizioni di equilibrio/stazionarietà (generalizzazione della complementarietà):**
    *   `c(f*) - Γ^T λ* + μ* ≥ 0`  (Nel paper, la notazione `λΓ` è equivalente a `Γ^T λ`)
    *   `(c(f*) - Γ^T λ* + μ*, f*) = 0`
    *   `(f* - d, μ*) = 0`

**4.3.1. Interpretazione tramite Costi Ridotti Generalizzati**

Definiamo il **costo ridotto generalizzato** `c'_ij(f)` per un arco `(i, j)` come:

`c'_ij(f) = c_ij(f) + λ_i - λ_j`

Le condizioni di ottimalità per il modello VI possono essere riespresse in modo analogo al caso lineare, fornendo un criterio di equilibrio locale per ogni arco:

Per un flusso di equilibrio `f*` e i relativi potenziali `λ*`:
*   **Se `0 < f*_ij < d_ij`**: Il flusso è intermedio. Per l'equilibrio, il costo ridotto deve essere nullo.
    => `c'_ij(f*) = 0`
*   **Se `f*_ij = 0`**: Non c'è flusso. Questo è ottimale solo se il costo ridotto per introdurre flusso non è negativo.
    => `c'_ij(f*) ≥ 0`
*   **Se `f*_ij = d_ij`**: L'arco è saturo. Questo è ottimale solo se il costo ridotto è non positivo, indicando che si vorrebbe inviare ancora più flusso se la capacità lo permettesse.
    => `c'_ij(f*) ≤ 0`

Queste condizioni mostrano come i principi fondamentali di ottimalità del MCFP classico (basati sui costi ridotti) si estendano naturalmente a problemi di equilibrio molto più generali. I problemi specifici come il cammino minimo e l'assegnamento, essendo casi particolari del MCFP classico, sono di conseguenza anche casi particolari del modello di flusso basato su inequazione variazionale (con funzione di costo costante).