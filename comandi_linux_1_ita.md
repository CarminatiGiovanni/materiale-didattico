# 🐧 Laboratori Linux — TPS
### 2 Laboratori • 1 Ora • Ubuntu/Debian • Da zero

---

> **Prima di iniziare** — apri il terminale sulla tua VM Ubuntu.  
> Puoi trovarlo cercando **Terminal** nel menu applicazioni, oppure con la scorciatoia `Ctrl + Alt + T`.
>
> Verifica di essere operativo:
> ```bash
> whoami
> ```
> Il terminale risponde con il tuo nome utente. Sei pronto.

---

## 📋 Programma

| Lab | Titolo | Durata | Argomenti |
|-----|--------|--------|-----------|
| 1 | Esplorare e gestire i file | ~25 min | Filesystem Linux, navigazione, creazione e gestione file/cartelle |
| 2 | Utenti, permessi e processi | ~30 min | Permessi UNIX, utenti, processi attivi |

> ⏱ Lascia 5 minuti alla fine per confrontare i risultati con i compagni.

---

---

# Laboratorio 1 — Esplorare e Gestire i File

**Durata:** ~25 minuti  
**Obiettivo:** Orientarsi nel filesystem Linux e gestire file e cartelle da terminale.

---

## 💡 Teoria in 2 minuti

In Linux **tutto è un file**: documenti, dispositivi, processi. Il filesystem parte da una radice unica chiamata `/` (root), senza lettere di unità come in Windows.

```
/                   ← radice del sistema
├── home/           ← cartelle degli utenti (come "Utenti" in Windows)
│   └── studente/   ← la tua home directory (~)
├── etc/            ← file di configurazione del sistema
├── var/            ← log e dati variabili
└── tmp/            ← file temporanei
```

Il terminale ti mostra sempre **dove sei** con il prompt. Il simbolo `~` significa che sei nella tua home directory.

---

## Parte A — Orientarsi nel filesystem (8 min)

**1.** Scopri dove ti trovi:

```bash
pwd
```

> `pwd` = *Print Working Directory*. Mostra il percorso assoluto della cartella corrente.

**2.** Elenca il contenuto della cartella corrente:

```bash
ls
```

**3.** Versione dettagliata con permessi, dimensione e data:

```bash
ls -l
```

**4.** Mostra anche i file nascosti (quelli che iniziano con `.`):

```bash
ls -la
```

**5.** Spostati nella cartella `/etc` e guardala:

```bash
cd /etc
ls
```

**6.** Torna subito alla tua home in un colpo solo:

```bash
cd ~
pwd
```

**7.** Sali di un livello (cartella padre):

```bash
cd ..
pwd
```

**8.** Rientra nella home:

```bash
cd ~
```

---

## Parte B — Creare e gestire file e cartelle (17 min)

**9.** Crea una cartella di lavoro per questo lab:

```bash
mkdir lab-linux
cd lab-linux
```

**10.** Crea un file di testo vuoto:

```bash
touch appunti.txt
ls
```

**11.** Scrivi del testo nel file:

```bash
echo "Sistema Operativo: Ubuntu" > appunti.txt
echo "Kernel: Linux" >> appunti.txt
echo "Shell: bash" >> appunti.txt
```

> `>` sovrascrive il file. `>>` aggiunge in coda senza cancellare.

**12.** Visualizza il contenuto del file:

```bash
cat appunti.txt
```

**13.** Crea una sottocartella e copia dentro un file:

```bash
mkdir note
cp appunti.txt note/copia-appunti.txt
ls note/
```

**14.** Verifica che l'originale esista ancora:

```bash
ls
```

**15.** Sposta (rinomina) il file originale:

```bash
mv appunti.txt appunti-lab1.txt
ls
```

**16.** Crea altri due file e poi eliminali:

```bash
touch file-da-eliminare.txt altro-file.txt
ls
rm file-da-eliminare.txt
rm altro-file.txt
ls
```

**17.** Elimina la sottocartella con tutto il contenuto:

```bash
rm -r note/
ls
```

> ⚠️ In Linux non c'è cestino da terminale: `rm` è **definitivo**. Usa sempre con attenzione.

---

## ✅ Verifica Lab 1

- [ ] Sai navigare il filesystem con `cd`, `ls`, `pwd`
- [ ] Sai creare file con `touch` ed `echo`
- [ ] Sai copiare (`cp`), spostare (`mv`) ed eliminare (`rm`) file e cartelle
- [ ] Conosci la differenza tra `>` e `>>`

---

## ❓ Domande — Lab 1

**D1.** Il filesystem Linux parte da `/` mentre Windows usa lettere come `C:\`. Quale dei due approcci ti sembra più logico e perché?

**D2.** Qual è la differenza tra `>` e `>>`? Scrivi un esempio concreto in cui usare `>>` è necessario mentre `>` causerebbe un problema.

**D3.** Il comando `mv` serve sia a spostare che a rinominare un file. Come è possibile che un solo comando faccia due cose? Cosa hanno in comune le due operazioni?

**D4.** `rm` non usa il cestino e cancella in modo definitivo. Perché secondo te Linux funziona così invece di usare un cestino come i sistemi desktop? In quali situazioni questo comportamento è un vantaggio? (Pensa per cosa è nato linux)

---

## 📌 Comandi del Lab 1

| Comando | Cosa fa |
|---------|---------|
| `pwd` | Mostra la cartella corrente |
| `ls` | Elenca il contenuto |
| `ls -la` | Elenco dettagliato con file nascosti |
| `cd <percorso>` | Cambia cartella |
| `cd ~` | Torna alla home |
| `cd ..` | Sale alla cartella padre |
| `mkdir <nome>` | Crea una cartella |
| `touch <file>` | Crea un file vuoto |
| `echo "testo" > file` | Scrive testo in un file (sovrascrive) |
| `echo "testo" >> file` | Aggiunge testo in fondo al file |
| `cat <file>` | Visualizza il contenuto di un file |
| `cp <orig> <dest>` | Copia un file |
| `mv <orig> <dest>` | Sposta o rinomina un file |
| `rm <file>` | Elimina un file (definitivo!) |
| `rm -r <cartella>` | Elimina una cartella e il suo contenuto |

---

---

# 🔑 Approfondimento — `sudo` e `apt`: installare e gestire il sistema

> Questa sezione si legge prima di iniziare il Lab 2. Non richiede operazioni obbligatorie, ma contiene esercizi pratici opzionali contrassegnati con 🔧.

---

## Chi è `root` e cos'è `sudo`

In Linux esiste un utente speciale chiamato **root** — il super-amministratore del sistema. Root può fare qualsiasi cosa: leggere ogni file, modificare la configurazione, installare programmi, cancellare l'intero sistema operativo.

Per sicurezza, il tuo utente normale **non è root** e non può eseguire operazioni rischiose. Il comando `sudo` (*Super User DO*) ti permette di eseguire un singolo comando con i privilegi di root, dopo aver inserito la tua password.

```
utente normale  →  sudo comando  →  sistema chiede la password  →  comando eseguito come root
```

**Esempio concreto:** aggiornare la lista dei pacchetti disponibili richiede permessi di root perché modifica file di sistema:

```bash
# Senza sudo → errore: Permission denied
apt update

# Con sudo → funziona
sudo apt update
```

> 💡 `sudo` non ti trasforma in root in modo permanente: vale solo per quel singolo comando. Se vuoi una sessione root completa (sconsigliato), si usa `sudo -i` — ma in questo corso non ne avrai bisogno.

### Perché non usare sempre root?

Il principio è chiamato **least privilege** (privilegio minimo): ogni utente e processo dovrebbe avere solo i permessi strettamente necessari. Lavorare sempre come root è pericoloso perché:

- Un errore di battitura (`rm -r /` invece di `rm -r ./`) può distruggere il sistema
- Un programma malevolo in esecuzione avrebbe accesso illimitato
- Non c'è modo di distinguere le azioni dell'amministratore da quelle normali nei log

---

## `apt` — il gestore di pacchetti

Su Ubuntu e Debian, i programmi si installano tramite **pacchetti**: archivi che contengono il software, le sue dipendenze e le istruzioni di installazione. Il gestore di pacchetti `apt` (*Advanced Package Tool*) si occupa di scaricare, installare, aggiornare e rimuovere i pacchetti.

I pacchetti vengono scaricati da **repository**: server ufficiali mantenuti da Ubuntu/Debian che contengono migliaia di programmi verificati e sicuri.

```
Repository ufficiale Ubuntu  →  Internet  →  apt  →  il tuo sistema
```

### I comandi fondamentali di `apt`

| Comando | Cosa fa |
|---------|---------|
| `sudo apt update` | Aggiorna la lista dei pacchetti disponibili (non installa nulla) |
| `sudo apt upgrade` | Installa gli aggiornamenti di tutti i pacchetti già presenti |
| `sudo apt install <nome>` | Installa un nuovo pacchetto |
| `sudo apt remove <nome>` | Rimuove un pacchetto (lascia i file di configurazione) |
| `sudo apt purge <nome>` | Rimuove un pacchetto **e** i suoi file di configurazione |
| `apt search <parola>` | Cerca pacchetti per nome o descrizione (non serve sudo) |
| `apt show <nome>` | Mostra informazioni dettagliate su un pacchetto |

> ⚠️ `sudo apt update` e `sudo apt install` sono comandi **distinti e separati**. `update` non installa nulla: scarica solo la lista aggiornata di cosa è disponibile. Eseguirlo prima di installare è buona pratica perché evita di installare versioni vecchie.

### Il flusso tipico di installazione

```bash
# 1. Aggiorna la lista dei pacchetti disponibili
sudo apt update

# 2. Installa il programma desiderato
sudo apt install <nome-pacchetto>
```

Durante l'installazione, `apt` mostra quanto spazio occuperà e chiede conferma. Rispondi `Y` (o premi Invio se `Y` è già l'opzione predefinita).

---

## 🔧 Esercizi pratici (opzionali)

**Es. 1 — Cerca e installa `tree`** (un comando che mostra le cartelle come albero):

```bash
apt search tree          # cerca pacchetti che contengono "tree"
apt show tree            # leggi la descrizione
sudo apt update
sudo apt install tree
tree ~/lab-linux         # prova subito il nuovo comando
```

**Es. 2 — Installa e prova `htop`** (una versione migliorata di `top`):

```bash
sudo apt install htop
htop                     # premi q per uscire
```

**Es. 3 — Rimuovi un pacchetto che hai appena installato:**

```bash
sudo apt remove tree
tree                     # ora il comando non esiste più
```

---

## ❓ Domande — sudo e apt

**D-S1.** Qual è la differenza tra `apt update` e `apt upgrade`? Perché è importante eseguirli nell'ordine corretto?

**D-S2.** Perché `sudo apt update` richiede `sudo`, mentre `apt search <parola>` no? Cosa distingue le due operazioni a livello di permessi?

---

---

# Laboratorio 2 — Utenti, Permessi e Processi

**Durata:** ~30 minuti  
**Prerequisito:** Lab 1 completato  
**Obiettivo:** Capire il sistema di permessi UNIX, gestire utenti e ispezionare i processi attivi.

---

## 💡 Teoria in 2 minuti

### Permessi UNIX

Ogni file in Linux ha tre gruppi di permessi:

```
-rwxr-xr--   studente   gruppo   file.txt
 ^^^              → permessi del proprietario  (rwx = leggi, scrivi, esegui)
    ^^^           → permessi del gruppo        (r-x = leggi, esegui)
       ^^^        → permessi degli altri       (r-- = solo leggi)
```

| Simbolo | Significato | Valore numerico |
|---------|-------------|-----------------|
| `r` | read — lettura | 4 |
| `w` | write — scrittura | 2 |
| `x` | execute — esecuzione | 1 |
| `-` | permesso assente | 0 |

I permessi si scrivono anche in **notazione ottale**: `rwx` = 4+2+1 = **7**, `r-x` = 4+0+1 = **5**, `r--` = 4+0+0 = **4**.

Quindi `rwxr-xr--` = **754**.

---

## Parte A — Leggere e modificare i permessi (12 min)

**1.** Vai nella cartella del Lab 1 e crea un file di prova:

```bash
cd ~/lab-linux
echo "file di prova permessi" > prova.txt
```

**2.** Visualizza i permessi del file:

```bash
ls -l prova.txt
```

> Leggi la prima colonna: tipo + permessi proprietario + permessi gruppo + permessi altri.

**3.** Rimuovi tutti i permessi (nessuno può fare nulla):

```bash
chmod 000 prova.txt
ls -l prova.txt
```

**4.** Prova a leggere il file — otterrai un errore:

```bash
cat prova.txt
```

**5.** Ripristina i permessi: proprietario legge e scrive, gli altri solo leggono:

```bash
chmod 644 prova.txt
ls -l prova.txt
cat prova.txt
```

**6.** Rendi il file eseguibile per il proprietario:

```bash
chmod 744 prova.txt
ls -l prova.txt
```

> Nota il cambiamento nella prima colonna: compare la `x`.

**7.** Crea uno script bash minimale e rendilo eseguibile:

```bash
echo '#!/bin/bash' > saluta.sh
echo 'echo "Ciao da Linux!"' >> saluta.sh
chmod +x saluta.sh
./saluta.sh
```

---

## Parte B — Utenti e gruppi (8 min)

**8.** Scopri chi sei e a quali gruppi appartieni:

```bash
whoami
id
groups
```

**9.** Visualizza la lista degli utenti del sistema:

```bash
cat /etc/passwd
```

> Ogni riga ha il formato: `nomeutente:x:UID:GID:info:home:shell`  
> Gli utenti di sistema (UID < 1000) gestiscono i servizi — non sono persone reali.

**10.** Filtra solo gli utenti "veri" (UID ≥ 1000):

```bash
awk -F: '$3 >= 1000' /etc/passwd
```

**11.** Visualizza i gruppi del sistema:

```bash
cat /etc/group
```

**12.** Mostra informazioni sul tuo utente:

```bash
id
```

> `uid` = user ID, `gid` = group ID principale, `groups` = tutti i gruppi a cui appartieni.

---

## Parte C — Processi (10 min)

**13.** Visualizza i processi attivi dell'utente corrente:

```bash
ps
```

**14.** Visualizza **tutti** i processi del sistema in formato esteso:

```bash
ps aux
```

> Colonne principali: `USER` (proprietario), `PID` (ID processo), `%CPU`, `%MEM`, `COMMAND`.

**15.** Vista dinamica in tempo reale (come Task Manager):

```bash
top
```

> Premi `q` per uscire. Premi `k` per terminare un processo inserendo il suo PID.

**16.** Cerca un processo specifico per nome:

```bash
ps aux | grep bash
```

> Il `|` (pipe) passa l'output di `ps aux` come input a `grep`, che filtra le righe contenenti "bash".

**17.** Lancia un processo in background e poi trovalo:

```bash
sleep 60 &
ps aux | grep sleep
```

**18.** Termina il processo trovando il suo PID (sostituisci `<PID>` con il numero che hai visto):

```bash
kill <PID>
ps aux | grep sleep
```

---

## ✅ Verifica Lab 2

- [ ] Sai leggere i permessi di un file con `ls -l`
- [ ] Sai modificare i permessi con `chmod` in notazione ottale
- [ ] Conosci la struttura di `/etc/passwd`
- [ ] Sai visualizzare i processi con `ps aux` e `top`
- [ ] Sai terminare un processo con `kill`

---

## ❓ Domande — Lab 2

**D5.** Converti in notazione ottale i seguenti set di permessi. Mostra i calcoli:
- `rwxrwxrwx` → ___
- `rw-r--r--` → ___
- `rwx------` → ___

**D6.** Un file ha permessi `chmod 000`. Anche il proprietario non riesce a leggerlo. Come può risolvere il problema? Sarebbe possibile per un altro utente normale leggere comunque quel file? E per root?

**D7.** Cosa rappresenta il PID di un processo? Perché ogni processo ha un numero univoco invece di usare direttamente il suo nome?

**D8.** Spiega cosa fa questo comando nel dettaglio, pezzo per pezzo:
```bash
ps aux | grep bash
```
Cosa fa `ps aux`? Cosa fa `|`? Cosa fa `grep bash`? Qual è il risultato finale?

---

## 📌 Comandi del Lab 2

| Comando | Cosa fa |
|---------|---------|
| `ls -l` | Mostra permessi, proprietario e dimensione |
| `chmod <ottale> <file>` | Modifica i permessi in notazione ottale |
| `chmod +x <file>` | Aggiunge il permesso di esecuzione |
| `whoami` | Mostra l'utente corrente |
| `id` | Mostra UID, GID e gruppi dell'utente |
| `groups` | Elenca i gruppi dell'utente |
| `cat /etc/passwd` | Lista utenti del sistema |
| `cat /etc/group` | Lista gruppi del sistema |
| `ps` | Processi dell'utente corrente |
| `ps aux` | Tutti i processi del sistema |
| `top` | Monitor processi in tempo reale |
| `kill <PID>` | Termina un processo per ID |
| `comando &` | Lancia un processo in background |
| `cmd1 \| cmd2` | Pipe: passa output di cmd1 a cmd2 |

---

---

## 📚 Tutti i Comandi — Riferimento Rapido

| Comando | Categoria |
|---------|-----------|
| `pwd` | Navigazione |
| `cd <percorso>` / `cd ~` / `cd ..` | Navigazione |
| `ls` / `ls -l` / `ls -la` | Navigazione |
| `mkdir` / `touch` | Creazione |
| `echo "..." > file` / `>>` | Scrittura file |
| `cat <file>` | Lettura file |
| `cp` / `mv` / `rm` / `rm -r` | Gestione file |
| `chmod` | Permessi |
| `whoami` / `id` / `groups` | Utenti |
| `cat /etc/passwd` / `/etc/group` | Utenti |
| `ps` / `ps aux` / `top` | Processi |
| `kill <PID>` | Processi |
| `comando &` | Processi background |
| `cmd1 \| cmd2` | Pipe |

---

*Laboratori Linux TPS — Quarta Superiore • v1.0 • Marzo 2026 • 2 lab / 1 ora*