# Esercitazione Laboratorio: Algoritmo di Dijkstra
## Percorsi Minimi in una Rete

---

## 📖 Introduzione: Metodi dei Dizionari Python

Prima di iniziare l'esercitazione, è fondamentale comprendere come lavorare con i dizionari in Python, soprattutto i metodi `.keys()`, `.values()` e `.items()`.

### Cosa è un Dizionario?

Un dizionario è una struttura dati che memorizza coppie **chiave-valore**:

```python
# Esempio: costi per raggiungere i nodi
cost = {
    'A': 0,
    'B': 2,
    'C': 9,
    'D': 7
}

# Accesso per chiave
print(cost['A'])  # Output: 0
print(cost['B'])  # Output: 2
```

---

### Metodo 1: `.keys()` - Ottenere tutte le chiavi

```python
cost = {'A': 0, 'B': 2, 'C': 9, 'D': 7}

# Restituisce: dict_keys(['A', 'B', 'C', 'D'])
for k in cost.keys():
    print(k)

# Output:
# A
# B
# C
# D
```

**Cosa fa:** Restituisce un'oggetto che contiene tutte le **chiavi** del dizionario.

**Quando lo usi:** Quando hai bisogno solo delle chiavi (in questo caso, i nomi dei nodi).

**Nota:** Puoi omettere `.keys()` - `for k in cost:` funziona allo stesso modo!

---

### Metodo 2: `.values()` - Ottenere tutti i valori

```python
cost = {'A': 0, 'B': 2, 'C': 9, 'D': 7}

# Restituisce: dict_values([0, 2, 9, 7])
for v in cost.values():
    print(v)

# Output:
# 0
# 2
# 9
# 7
```

**Cosa fa:** Restituisce un'oggetto che contiene tutti i **valori** del dizionario.

**Quando lo usi:** Quando hai bisogno solo dei valori (in questo caso, i costi).

**Utilizzo comune:**
```python
cost_massimo = max(cost.values())  # Trova il valore massimo
somma_costi = sum(cost.values())   # Somma tutti i valori
```

---

### Metodo 3: `.items()` - Ottenere coppie chiave-valore

```python
cost = {'A': 0, 'B': 2, 'C': 9, 'D': 7}

# Restituisce: dict_items([('A', 0), ('B', 2), ('C', 9), ('D', 7)])
for k, v in cost.items():
    print(f"Nodo {k} ha costo {v}")

# Output:
# Nodo A ha costo 0
# Nodo B ha costo 2
# Nodo C ha costo 9
# Nodo D ha costo 7
```

**Cosa fa:** Restituisce coppie **(chiave, valore)** che puoi estrarre contemporaneamente.

**Quando lo usi:** Quando hai bisogno di **ENTRAMBI** la chiave e il valore.

**Nel nostro algoritmo Dijkstra:**
```python
network = {'A': {'B': 2, 'D': 8}, 'B': {'A': 2, 'D': 5}}

node = 'A'
for conn, link in network[node].items():
    # conn = nodo connesso (chiave)
    # link = peso dell'arco (valore)
    print(f"Da {node} a {conn} con peso {link}")

# Output:
# Da A a B con peso 2
# Da A a D con peso 8
```

---

### Tabella Riassuntiva

| Metodo | Restituisce | Uso | Esempio |
|--------|------------|-----|---------|
| `.keys()` | Solo chiavi | `for k in d.keys():` | Ottenere nodi |
| `.values()` | Solo valori | `for v in d.values():` | Ottenere costi |
| `.items()` | Coppie (k, v) | `for k, v in d.items():` | Nodo e costo insieme |

---

### Loop nei Dizionari

#### 1. Iterare solo sulle chiavi
```python
cost = {'A': 0, 'B': 2, 'C': 9}

# Metodo 1: Esplicito
for k in cost.keys():
    print(k)

# Metodo 2: Implicito (più veloce)
for k in cost:
    print(k)

# Entrambi stamano: A, B, C
```

#### 2. Iterare sulle coppie chiave-valore
```python
cost = {'A': 0, 'B': 2, 'C': 9}

for k, v in cost.items():
    print(f"{k}: {v}")

# Output:
# A: 0
# B: 2
# C: 9
```

#### 3. Iterare solo sui valori
```python
cost = {'A': 0, 'B': 2, 'C': 9}

for v in cost.values():
    print(v)

# Output:
# 0
# 2
# 9
```

#### 4. Enumerare (aggiungere un indice)
```python
cost = {'A': 0, 'B': 2, 'C': 9}

for i, (k, v) in enumerate(cost.items()):
    print(f"Posizione {i}: {k} = {v}")

# Output:
# Posizione 0: A = 0
# Posizione 1: B = 2
# Posizione 2: C = 9
```

---

## 🎯 Obiettivo dell'Esercitazione

Implementare **l'algoritmo di Dijkstra** per trovare i percorsi di costo minimo da un nodo sorgente a tutti gli altri nodi in una rete di comunicazione. L'esercizio è suddiviso in **5 parti da svolgere progressivamente** in **1 ora e 30 minuti**.

**Competenze che svilupperai:**
- Uso dei dizionari Python e dei loro metodi
- Algoritmi su grafi
- Programmazione iterativa e logica di loop
- Strutturazione di codice complesso

---

## Cronogramma suggerito

| Parte | Argomento | Tempo |
|-------|-----------|-------|
| Introduzione | Metodi dizionari | 10 min |
| Parte 1 | Inizializzazione | 10 min |
| Parte 2 | Trovare il minimo | 10 min |
| Parte 3 | Aggiornare costi | 10 min |
| Parte 4 | Ciclo principale | 15 min |
| Parte 5 | Visualizzare risultati | 10 min |
| **Test e debug** | Esecuzione e verifica | 15 min |
| **Bonus** | Rete alternativa | 10 min |
| **TOTALE** | | **90 min** |

---

## 1️⃣ Rete Principale: Comunicazione tra Nodi

### Visualizzazione della Rete

| Nodo | Connessioni |
|------|-----------|
| A | → B (2), → D (8) |
| B | → A (2), → D (5), → E (6) |
| C | → E (9), → F (3) |
| D | → A (8), → B (5), → E (3), → F (2) |
| E | → B (6), → D (3), → F (1), → C (9) |
| F | → D (2), → E (1), → C (3) |

### Rappresentazione Grafica

```
    2       5
A -------- B
|    \      \
8     \5     6
|      \      \
D --------E ---- F
 \  3    / \  1 /
  \     /   \  /
   2   /     3
    \ /       \
     F --------C
         3
```

### Pseudo-codice dell'Algoritmo

```
ALGORITMO Dijkstra(grafo, nodo_sorgente):
    
    INIZIALIZZAZIONE:
    1. N = []  (nodi visitati)
    2. NN = [tutti i nodi]  (nodi non visitati)
    3. cost = {tutti i nodi: INF}  (costo infinito)
    4. cost[nodo_sorgente] = 0  (costo 0 per partenza)
    5. nexthop = {tutti i nodi: ''}  (prossimi hop sconosciuti)
    
    CICLO PRINCIPALE:
    6. MENTRE NN non è vuota:
        
        a. Trova il nodo in NN con costo MINIMO
        b. Rimuovi il nodo da NN
        c. Aggiungi il nodo a N
        
        d. PER OGNI connessione del nodo corrente:
            - SE (costo_nodo_corrente + peso_arco) < costo_destinazione:
                * Aggiorna cost[destinazione]
                * Aggiorna nexthop[destinazione]
    
    RETURN cost, nexthop
```

---

## 📋 PARTE 1: Inizializzazione (10 minuti)

### Obiettivo
Preparare tutte le strutture dati necessarie per l'algoritmo.

### Dati Forniti

```python
network = {
    'A': {'B': 2, 'D': 8},
    'B': {'A': 2, 'D': 5, 'E': 6},
    'C': {'E': 9, 'F': 3},
    'D': {'A': 8, 'B': 5, 'E': 3, 'F': 2},
    'E': {'B': 6, 'D': 3, 'F': 1, 'C': 9},
    'F': {'D': 2, 'E': 1, 'C': 3}
}

INF = float('inf')
```

### Istruzioni

Scrivere il codice per inizializzare:

1. **Lista `N`** - Nodi già visitati (inizialmente vuota)
2. **Lista `NN`** - Nodi ancora da visitare (tutti i nodi)
3. **Dizionario `cost`** - Costi iniziali:
   - Nodo 'A' ha costo 0 (è il nodo sorgente)
   - Tutti gli altri nodi hanno costo INF (infinito)
4. **Dizionario `nexthop`** - Next hop iniziale:
   - Tutti i valori sono stringhe vuote ''

### Input Atteso
```python
# Variabili da inizializzare
N = ?
NN = ?
cost = ?
nexthop = ?
```

### Output Atteso
```python
N = []
NN = ['A', 'B', 'C', 'D', 'E', 'F']
cost = {'A': 0, 'B': inf, 'C': inf, 'D': inf, 'E': inf, 'F': inf}
nexthop = {'A': '', 'B': '', 'C': '', 'D': '', 'E': '', 'F': ''}
```

### Suggerimenti

- Usa `INF` per i valori infiniti
- La lista `NN` contiene esattamente i nomi dei nodi
- Il dizionario `cost` deve avere una chiave per ogni nodo
- Puoi usare un dizionario vuoto iniziale e popolare con un loop

---

## 📋 PARTE 2: Trovare il Nodo con Costo Minimo (10 minuti)

### Obiettivo
Scrivere una funzione (o inline nel ciclo) che individui quale nodo elaborare.

### Istruzioni

Implementare una funzione **oppure** inserire il codice direttamente nel ciclo principale che:

1. Prende in input:
   - `NN`: lista dei nodi non visitati
   - `cost`: dizionario con i costi attuali

2. Trova il nodo in `NN` che ha il **costo MINORE**

3. Restituisce quel nodo

### Input Atteso
```python
NN = ['B', 'C', 'D', 'E', 'F']  # A è già stato visitato
cost = {'A': 0, 'B': 2, 'C': inf, 'D': 8, 'E': inf, 'F': inf}
```

### Output Atteso
```python
node = 'B'  # Ha il costo minore (2)
```

### Pseudo-codice
```
FUNZIONE trova_nodo_minimo(NN, cost):
    node_minimo = NN[0]  (inizializza con il primo nodo)
    
    PER OGNI nodo n in NN:
        SE cost[n] < cost[node_minimo]:
            node_minimo = n
    
    RETURN node_minimo
```

### Suggerimenti
- Inizia con il primo nodo della lista `NN[0]`
- Confronta il suo costo con gli altri
- Aggiorna `node_minimo` quando trovi un costo più piccolo
- Potrebbe essere una funzione separata o inline nel while

---

## 📋 PARTE 3: Aggiornare i Costi (10 minuti)

### Obiettivo
Implementare il rilassamento degli archi (edge relaxation).

### Istruzioni

Dato un nodo corrente, per ogni suo vicino:

1. Accedi alle connessioni del nodo dalla `network`
2. Calcola il costo totale per raggiungere il vicino
3. Se è migliore del costo attuale, aggiorna sia il costo che il next hop

### Input Atteso

Supponiamo di aver appena elaborato il nodo 'A':

```python
node = 'A'
cost = {'A': 0, 'B': inf, 'C': inf, 'D': inf, 'E': inf, 'F': inf}
nexthop = {'A': '', 'B': '', 'C': '', 'D': '', 'E': '', 'F': ''}

# network[node] contiene le connessioni di A:
# {'B': 2, 'D': 8}
```

### Output Atteso

Dopo l'elaborazione di 'A':

```python
cost = {'A': 0, 'B': 2, 'C': inf, 'D': 8, 'E': inf, 'F': inf}
nexthop = {'A': '', 'B': 'A', 'C': '', 'D': 'A', 'E': '', 'F': ''}

# Spiegazione:
# - B: cost[A] + 2 = 0 + 2 = 2 (aggiornato da inf)
# - D: cost[A] + 8 = 0 + 8 = 8 (aggiornato da inf)
```

### Pseudo-codice

```
PER OGNI connessione (vicino, peso_arco) in network[node]:
    nuovo_costo = cost[node] + peso_arco
    
    SE nuovo_costo < cost[vicino]:
        cost[vicino] = nuovo_costo
        nexthop[vicino] = node
```

### Suggerimenti
- Usa `.items()` per iterare su chiave-valore della rete
- La chiave è il nodo connesso, il valore è il peso dell'arco
- Confronta prima di aggiornare
- Non aggiornare se il nuovo costo non è migliore

---

## 📋 PARTE 4: Ciclo Principale (15 minuti)

### Obiettivo
Assemblare tutte le parti per far funzionare l'algoritmo completo.

### Istruzioni

Implementare il ciclo principale che:

1. **Mentre `NN` non è vuota:**
   - Trova il nodo con costo minimo (Parte 2)
   - Rimuovilo da `NN` con `.remove()`
   - Aggiungilo a `N` con `.append()`
   - Aggiorna i costi dei vicini (Parte 3)

### Input Atteso

Iniziale:
```python
N = []
NN = ['A', 'B', 'C', 'D', 'E', 'F']
cost = {'A': 0, 'B': inf, 'C': inf, 'D': inf, 'E': inf, 'F': inf}
nexthop = {'A': '', 'B': '', 'C': '', 'D': '', 'E': '', 'F': ''}
```

### Output Atteso

Dopo il ciclo completo:
```python
N = ['A', 'B', 'D', 'E', 'F', 'C']  # Tutti visitati
NN = []  # Vuota

cost = {
    'A': 0,
    'B': 2,
    'C': 12,
    'D': 7,
    'E': 8,
    'F': 9
}

nexthop = {
    'A': '',
    'B': 'A',
    'C': 'F',
    'D': 'B',
    'E': 'B',
    'F': 'D'
}
```

### Pseudo-codice

```
MENTRE NN non è vuota:
    
    1. node = trova_nodo_minimo(NN, cost)
    
    2. NN.remove(node)
    
    3. N.append(node)
    
    4. PER OGNI (conn, link) in network[node].items():
           SE cost[node] + link < cost[conn]:
               cost[conn] = cost[node] + link
               nexthop[conn] = node
```

### Suggerimenti
- Usa un ciclo `while` che continua finché `NN` non è vuota
- La condizione `while NN:` è vera se la lista non è vuota
- `.remove()` rimuove la prima occorrenza
- `.append()` aggiunge alla fine della lista
- Utilizza `.items()` per scorrere la rete

---

## 📋 PARTE 5: Visualizzare la Routing Table (10 minuti)

### Obiettivo
Stampare i risultati in modo professionale e leggibile.

### Istruzioni

Stampare una tabella ben formattata che mostri per ogni nodo:
- Il nodo destinazione
- Il costo per raggiungerlo da A
- Il prossimo hop

### Input Atteso

```python
cost = {'A': 0, 'B': 2, 'C': 12, 'D': 7, 'E': 8, 'F': 9}
nexthop = {'A': '', 'B': 'A', 'C': 'F', 'D': 'B', 'E': 'B', 'F': 'D'}
```

### Output Atteso

```
==================================================
ROUTING TABLE (da nodo A)
==================================================
Destinazione    Costo           Next Hop        
--------------------------------------------------
A               0                               
B               2               A               
C               12              F               
D               7               B               
E               8               B               
F               9               D               
==================================================
```

### Pseudo-codice

```
Stampa "=" * 50
Stampa "ROUTING TABLE (da nodo A)"
Stampa "=" * 50
Stampa intestazione: "Destinazione | Costo | Next Hop"
Stampa "-" * 50

PER OGNI nodo k in cost.keys():
    Stampa formato: nodo, costo[nodo], nexthop[nodo]

Stampa "=" * 50
```

### Suggerimenti
- Usa `print()` con formattazione f-string
- Usa `.keys()` per iterare sulle chiavi
- Usa stringhe con larghezza fissa per allineamento: `f"{k:<15}"`
- `<` allinea a sinistra, `>` a destra
- Puoi usare `cost[k]` per accedere al valore dalla chiave

---

## ✨ PARTE BONUS: Rete Alternativa (10 minuti)

Applica l'algoritmo a una seconda rete per consolidare l'apprendimento.

### Rete di Trasporto Stradale

Calcola i percorsi minimi da **Milano (M)** a tutte le altre città:

| Città | Connessioni |
|-------|-----------|
| M (Milano) | → B (Bologna): 200 km |
| B (Bologna) | → M: 200 km, → F (Firenze): 150 km, → V (Venezia): 350 km |
| F (Firenze) | → B: 150 km, → R (Roma): 250 km, → V: 400 km |
| V (Venezia) | → B: 350 km, → F: 400 km |
| R (Roma) | → F: 250 km, → N (Napoli): 240 km |
| N (Napoli) | → R: 240 km |

### Grafo in Python

```python
network_trasporto = {
    'M': {'B': 200},
    'B': {'M': 200, 'F': 150, 'V': 350},
    'F': {'B': 150, 'R': 250, 'V': 400},
    'V': {'B': 350, 'F': 400},
    'R': {'F': 250, 'N': 240},
    'N': {'R': 240}
}
```

### Istruzioni

1. Copia il tuo codice completo
2. Sostituisci `network` con `network_trasporto`
3. Modifica la lista `NN` con i nomi delle città
4. Modifica la visualizzazione della routing table
5. Esegui e osserva i risultati

### Output Atteso

```
==================================================
ROUTING TABLE (da nodo M)
==================================================
Destinazione    Costo           Next Hop        
--------------------------------------------------
M               0                               
B               200             M               
F               350             B               
V               550             B               
R               600             F               
N               840             R               
==================================================
```

### Domande di Controllo

1. Qual è la distanza minima da Milano a Roma?
2. Per raggiungere Napoli da Milano, quale città devi attraversare per prima?
3. Se il collegamento B-V fosse 300 km invece di 350 km, cambierebbe il percorso per Venezia?

---

## 📊 Verifica dei Risultati

### Tabella di Confronto Rete Principale

| Destinazione | Costo Atteso | Next Hop Atteso |
|---|---|---|
| A | 0 | (sorgente) |
| B | 2 | A |
| C | 12 | F |
| D | 7 | B |
| E | 8 | B |
| F | 9 | D |

### Test di Validazione

Prima di considare terminato il lavoro, verifica:

- [ ] Tutti i nodi sono visitati (`N` contiene 6 elementi)
- [ ] `NN` è vuota al termine del ciclo
- [ ] I costi corrispondono alla tabella di confronto
- [ ] I next hop permettono di ricostruire i percorsi minimi
- [ ] Il programma stampa la routing table correttamente

---

## 📚 Estensioni Avanzate

Dopo aver completato l'esercitazione:

1. **Modificare la sorgente:**
   - Calcola da nodo C anziché A
   - Cosa cambia?

2. **Tracciare il percorso completo:**
   - Non solo il next hop, ma l'intero percorso
   - Es: A → B → D → F → C per raggiungere C

3. **Grafi disconnessi:**
   - Cosa succede se non tutti i nodi sono raggiungibili?
   - Come rappresentare "non raggiungibile"?

4. **Ottimizzazione con heap:**
   - Usa `heapq` per rendere più efficiente la ricerca del minimo
   - Confronta con il metodo brute-force

---

## 📝 Note Importanti

### Formato Dei Dati

- I nomi dei nodi sono **stringhe** (con virgolette)
- I pesi sono **numeri interi** o **float**
- I costi infiniti si rappresentano con `float('inf')`

### Confronti con l'Infinito

```python
INF = float('inf')

print(5 < INF)      # True
print(INF < INF)    # False
print(INF == INF)   # True
```

### Stringhe Vuote vs None

```python
nexthop = {'A': ''}  # Stringa vuota
# Non usare None o una lista vuota []
```

### Iterazione su Dizionari

```python
# Corretti:
for k in cost.keys():        # esplicito
for k in cost:               # implicito (veloce)
for v in cost.values():      # solo valori
for k, v in cost.items():    # coppie

# Sbagliato:
for i in cost:               # i è una chiave, non un indice
```
Buon lavoro! 🚀

