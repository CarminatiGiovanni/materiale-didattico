# 🧪 Laboratori Git — Quarta Superiore

---

> **Prima di iniziare** — verifica che Git sia installato:
> ```bash
> git --version
> ```
> Se vedi un numero di versione, sei pronto. In caso contrario chiama il docente.

---

## 📋 Programma della giornata

| Lab | Titolo | Durata | Argomenti |
|-----|--------|--------|-----------|
| 1 | Primi passi con Git | ~40 min | Cosa è Git, `init` `add` `commit` `status` `log` |
| 2 | Esplorare e recuperare | ~35 min | `diff` `log` `checkout` (file) `restore` |
| 3 | Branch e merge | ~40 min | `branch` `checkout -b` `merge` |

> ⏱ Lascia 5 minuti alla fine di ogni lab per confrontare i risultati con i compagni.

---

---

# Laboratorio 1 — Primi Passi con Git

**Durata:** ~40 minuti  
**Obiettivo:** Capire cos'è Git, creare un repository e registrare le prime modifiche.

---

## 💡 Teoria in 2 minuti

**Git** è uno strumento che salva la storia completa di un progetto. Ogni volta che "salvi una versione" con Git, crei un **commit**: una fotografia del progetto in quel momento, con data e messaggio.

Tre zone fondamentali da tenere a mente:

```
[ Working Directory ]  →  git add  →  [ Staging Area ]  →  git commit  →  [ Repository ]
   (file sul disco)                    (pronti al salvataggio)              (storia salvata)
```

---

## Parte A — Configurazione (5 min)

Configura il tuo nome e la tua email. Appariranno in ogni commit.

```bash
git config --global user.name "Il Tuo Nome"
git config --global user.email "tua@email.com"
```

---

## Parte B — Creare il repository (10 min)

**1.** Crea una cartella e inizializza Git:

```bash
mkdir lab1-git
cd lab1-git
git init
```

> Git crea una cartella nascosta `.git/` — è il "cervello" del repository. Non toccarla mai.

**2.** Controlla lo stato (sarà vuoto):

```bash
git status
```

**3.** Crea un file `appunti.txt` con questo contenuto:

```
Materia: Informatica
Argomento: Git
Data: oggi
```

**4.** Controlla di nuovo lo stato:

```bash
git status
```

> `appunti.txt` appare come **Untracked**: Git lo vede ma non lo monitora ancora.

---

## Parte C — Il ciclo add → commit (15 min)

**5.** Aggiungi il file all'area di staging:

```bash
git add appunti.txt
```

**6.** Controlla lo stato:

```bash
git status
```

> Ora è sotto **Changes to be committed**: pronto per il salvataggio.

**7.** Registra il primo commit:

```bash
git commit -m "Primo commit: appunti Git"
```

**8.** Aggiungi una riga ad `appunti.txt`:

```
Comandi imparati: init, add, commit, status
```

**9.** Vedi cosa è cambiato:

```bash
git diff
```

> Le righe con `+` sono aggiunte, quelle con `-` sono rimosse.

**10.** Aggiungi e committa:

```bash
git add appunti.txt
git commit -m "Aggiunta lista comandi"
```

---

## Parte D — Esplorare la storia (10 min)

**11.** Crea un secondo file `todo.txt`:

```
[ ] Completare lab 1
[ ] Completare lab 2
[ ] Completare lab 3
```

**12.** Aggiungi e committa:

```bash
git add todo.txt
git commit -m "Aggiunta lista TODO"
```

**13.** Visualizza la storia:

```bash
git log --oneline
```

Dovresti vedere 3 commit, dal più recente al più vecchio.

---

## ✅ Verifica

Prima di andare avanti, assicurati di avere:
- [ ] 3 commit nella history (`git log --oneline`)
- [ ] 2 file nel progetto (`appunti.txt` e `todo.txt`)
- [ ] Capito la differenza tra `git add` e `git commit`

---

## ❓ Domande — Lab 1

Rispondi con parole tue, in 2–4 righe per domanda.

**D1.** Qual è la differenza tra `git add` e `git commit`? Perché Git separa questi due passaggi invece di farne uno solo?

**D2.** Cosa succederebbe se modificassi `appunti.txt` e poi eseguissi subito `git commit` senza fare prima `git add`? Prova a immaginarlo prima di testarlo.

**D3.** Perchè è fondamentale la flag `-m` quando si fa un commit?

---

## 📌 Comandi del Lab 1

| Comando | Cosa fa |
|---------|---------|
| `git init` | Crea un nuovo repository |
| `git status` | Mostra lo stato dei file |
| `git add <file>` | Porta un file in staging |
| `git add .` | Porta **tutti** i file modificati in staging |
| `git commit -m "..."` | Salva uno snapshot con messaggio |
| `git diff` | Mostra modifiche non ancora in staging |
| `git log --oneline` | Storia compatta dei commit |

---

---

# Laboratorio 2 — Esplorare e Recuperare

**Durata:** ~35 minuti  
**Prerequisito:** Lab 1 completato (almeno 3 commit)  
**Obiettivo:** Confrontare versioni diverse e recuperare un file com'era in passato.

---

## 💡 Teoria in 1 minuto

Ogni commit ha un **hash** — un codice univoco (es. `a3f9c21`). Puoi usarlo per ispezionare o recuperare qualsiasi versione passata. Git non cancella mai nulla: ogni commit è permanente nella history.

---

## Parte A — Ispezionare la storia (10 min)

**1.** Continua nel repository del Lab 1, oppure crea una storia veloce:

```bash
mkdir lab2-storia && cd lab2-storia
git init
echo "riga 1" > file.txt && git add . && git commit -m "v1"
echo "riga 2" >> file.txt && git add . && git commit -m "v2"
echo "riga 3 - sbagliata" >> file.txt && git add . && git commit -m "v3 con errore"
echo "riga 4" >> file.txt && git add . && git commit -m "v4"
```

**2.** Visualizza la history con dettagli:

```bash
git log --oneline
```

**3.** Mostra tutti i dettagli di un commit specifico (usa l'hash del secondo commit):

```bash
git show <hash-v2>
```

**4.** Confronta due commit tra loro:

```bash
git diff <hash-v1> <hash-v3>
```

> Sostituisci gli hash con quelli reali che vedi nel tuo `git log`.

---

## Parte B — Recuperare un file da un commit passato (15 min)

Immagina che la "riga 3 sbagliata" sia un bug. Vuoi riportare `file.txt` com'era alla v2, **senza perdere i commit v3 e v4**.

**5.** Recupera il file da un commit precedente:

```bash
git checkout <hash-v2> -- file.txt
```

> Il `--` separa l'hash dal nome file. I commit v3 e v4 restano intatti nella history.

**6.** Controlla il contenuto e lo stato:

```bash
cat file.txt
git status
```

> Il file è già in staging, pronto per il commit.

**7.** Committa il ripristino:

```bash
git commit -m "Ripristino file.txt alla versione v2"
```

**8.** Verifica la history finale:

```bash
git log --oneline
```

> Ora hai 5 commit: v1, v2, v3, v4 e il commit di ripristino. Nulla è andato perso.

---

## Parte C — Annullare modifiche non committate (10 min)

A volte modifichi un file per sbaglio e vuoi tornare indietro **prima** di fare commit.

**9.** Modifica `file.txt` in modo errato:

```bash
echo "modifica sbagliata" >> file.txt
cat file.txt
```

**10.** Annulla la modifica (torna all'ultimo commit):

```bash
git restore file.txt
cat file.txt
```

> La modifica è sparita. Attenzione: `git restore` è irreversibile — la modifica non è mai stata committata, quindi non c'è modo di recuperarla.

**11.** Prova il caso "ho fatto `add` per errore":

```bash
echo "altra modifica" >> file.txt
git add file.txt
git status
git restore --staged file.txt   # toglie dallo staging
git restore file.txt             # annulla la modifica
git status                       # tutto pulito
```

---

## ✅ Verifica

- [ ] Sai trovare l'hash di un commit con `git log --oneline`
- [ ] Sai recuperare un file da un commit passato con `git checkout <hash> -- <file>`
- [ ] Sai annullare modifiche non committate con `git restore`

---

## ❓ Domande — Lab 2

**D4.** Dopo aver eseguito `git checkout <hash-v2> -- file.txt`, il commit v3 (quello con l'errore) esiste ancora nella history? Spiega perché questo comportamento è utile rispetto a cancellarlo direttamente.

**D5.** Qual è la differenza tra `git restore file.txt` e `git restore --staged file.txt`? In quale situazione useresti l'uno e in quale l'altro?

**D6.** Se cancelli un file con `rm file.txt` (senza usare Git) e poi esegui `git restore file.txt`, cosa succede? Perché è possibile recuperarlo?

**D7.** L'output di `git diff <hash1> <hash2>` mostra righe con `+` e righe con `-`. Cosa indica ciascun simbolo? Se inverto l'ordine degli hash (`git diff <hash2> <hash1>`), cosa cambia nell'output?

---

## 📌 Comandi del Lab 2

| Comando | Cosa fa |
|---------|---------|
| `git log --oneline` | History compatta con hash |
| `git show <hash>` | Dettagli completi di un commit |
| `git diff <h1> <h2>` | Confronta due commit |
| `git checkout <hash> -- <file>` | Recupera un file da un commit passato |
| `git restore <file>` | Annulla modifiche non committate |
| `git restore --staged <file>` | Rimuove un file dallo staging |

---

---

# Laboratorio 3 — Branch e Merge

**Durata:** ~40 minuti  
**Prerequisito:** Lab 1 e 2  
**Obiettivo:** Creare branch separati per lavorare in parallelo e unirli con merge.

---

## 💡 Teoria in 2 minuti

Un **branch** è una linea di sviluppo indipendente. L'idea è semplice:

```
main:         A --- B --- C ----------- M   (merge commit)
                          \           /
nuova-funzione:            D --- E ---
```

Sul branch `main` hai la versione stabile. Sviluppi la nuova funzionalità su un branch separato, senza rischiare di rompere nulla. Quando sei soddisfatto, fai **merge** per unire il lavoro.

---

## Parte A — Creare e usare un branch (15 min)

**1.** Crea il repository base:

```bash
mkdir lab3-branch && cd lab3-branch
git init
echo "# Progetto Classe" > README.md
echo "Versione: 1.0" >> README.md
git add . && git commit -m "Setup iniziale"
```

**2.** Vedi i branch esistenti:

```bash
git branch
```

> Il `*` indica il branch corrente (`main` o `master`).

**3.** Crea un branch e spostati subito:

```bash
git checkout -b aggiunte
```

**4.** Sei su `aggiunte`. Modifica il file:

```bash
echo "Autore: Classe 4X" >> README.md
echo "Anno: 2024/25" >> README.md
git add . && git commit -m "Aggiunto autore e anno"
```

**5.** Aggiungi un secondo commit sul branch:

```bash
echo "Materia: Informatica" >> README.md
git add . && git commit -m "Aggiunta materia"
```

**6.** Torna su `main` e guarda il file — le modifiche **non ci sono**:

```bash
git checkout main
cat README.md
```

**7.** Visualizza entrambi i branch:

```bash
git log --oneline --graph --all
```

---

## Parte B — Merge senza conflitti (10 min)

**8.** Sei su `main`. Unisci il branch `aggiunte`:

```bash
git merge aggiunte
```

**9.** Verifica il risultato:

```bash
cat README.md
git log --oneline --graph --all
```

**10.** Il branch ha esaurito il suo scopo — eliminalo:

```bash
git branch -d aggiunte
```

---

## Parte C — Gestire un conflitto (15 min)

Un **conflitto** avviene quando due branch modificano la stessa riga. Git si ferma e ti chiede di scegliere. Non è un errore: è Git che non vuole decidere al posto tuo.

**11.** Crea la situazione di conflitto:

```bash
# Modifica su main
echo "Versione: 2.0" > README.md   # sovrascrive tutto con riga singola
git add . && git commit -m "Main: versione 2.0"

# Crea branch con modifica alla stessa riga
git checkout -b fix-versione
echo "Versione: 1.5-fix" > README.md
git add . && git commit -m "Fix: versione 1.5"

# Torna su main
git checkout main
```

**12.** Fai merge — si genera il conflitto:

```bash
git merge fix-versione
```

> Git stampa: `CONFLICT (content): Merge conflict in README.md`

**13.** Apri `README.md` e guarda i marcatori:

```bash
cat README.md
```

Vedrai:

```
<<<<<<< HEAD
Versione: 2.0
=======
Versione: 1.5-fix
>>>>>>> fix-versione
```

| Marcatore | Significato |
|-----------|-------------|
| `<<<<<<< HEAD` | Versione del branch corrente (main) |
| `=======` | Separatore |
| `>>>>>>> fix-versione` | Versione del branch in arrivo |

**14.** Risolvi il conflitto: modifica il file fino a che non contiene solo il testo corretto (senza marcatori). Scegli tu quale versione tenere, ad esempio:

```
Versione: 2.0
```

**15.** Segnala a Git che hai risolto e finalizza:

```bash
git add README.md
git commit -m "Merge: risolto conflitto versione"
```

**16.** Visualizza la history finale:

```bash
git log --oneline --graph --all
```

---

## ✅ Verifica finale (tutti e 3 i lab)

- [ ] Sai creare un repository da zero e fare commit
- [ ] Sai leggere `git log` e recuperare un file da un commit passato
- [ ] Sai creare un branch, svilupparci e fare merge
- [ ] Sai leggere i marcatori di conflitto e risolverli

---

## ❓ Domande — Lab 3

**D8.** Dopo aver fatto `git checkout main` al passo 6, le modifiche del branch `aggiunte` non sono visibili in `README.md`. Dove si trovano in quel momento? Sono andate perse?

> _Risposta:_ _______________________________________________

**D9.** Perché è buona pratica sviluppare una nuova funzionalità su un branch separato invece di lavorare direttamente su `main`? Fai un esempio concreto tratto da un progetto reale (anche scolastico).

> _Risposta:_ _______________________________________________

**D10.** Durante la risoluzione di un conflitto, Git non sceglie automaticamente quale versione tenere. Secondo te, perché Git non può farlo da solo? Esistono casi in cui potrebbe farlo senza rischi?

> _Risposta:_ _______________________________________________

---

## 📌 Comandi del Lab 3

| Comando | Cosa fa |
|---------|---------|
| `git branch` | Elenca i branch (`*` = corrente) |
| `git checkout -b <nome>` | Crea un branch e si sposta |
| `git checkout <nome>` | Cambia branch |
| `git merge <nome>` | Unisce il branch nel branch corrente |
| `git branch -d <nome>` | Elimina un branch già mergiato |
| `git merge --abort` | Annulla un merge in corso |

---

---

## 📚 Tutti i Comandi — Riferimento Rapido

| Comando | Categoria |
|---------|-----------|
| `git init` | Setup |
| `git config --global user.name/email` | Setup |
| `git status` | Info |
| `git add <file>` / `git add .` | Staging |
| `git commit -m "..."` | Commit |
| `git log --oneline` | Storia |
| `git log --oneline --graph --all` | Storia |
| `git diff` | Confronto |
| `git diff <h1> <h2>` | Confronto |
| `git show <hash>` | Confronto |
| `git checkout <hash> -- <file>` | Ripristino |
| `git restore <file>` | Ripristino |
| `git restore --staged <file>` | Ripristino |
| `git branch` | Branch |
| `git checkout -b <nome>` | Branch |
| `git checkout <nome>` | Branch |
| `git merge <nome>` | Merge |
| `git branch -d <nome>` | Branch |
| `git merge --abort` | Merge |

---

*Laboratori Git — Quarta Superiore • v2.0 • Marzo 2026 • 3 lab / 2 ore*
