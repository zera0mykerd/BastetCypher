<img src="https://i.ibb.co/T3xmCfF/Screen.png" width="100%">

# вαѕтєт¢уρнєя 𓃭 — BastetCipher

<div align="center">

![BastetCipher Banner](https://img.shields.io/badge/BastetCipher-v1.0.0-c9a84c?style=for-the-badge&labelColor=1a1208)
![Licenza MIT](https://img.shields.io/badge/Licenza-MIT-f0c040?style=for-the-badge&labelColor=1a1208)
![HTML5](https://img.shields.io/badge/HTML5-Single--File-e34c26?style=for-the-badge&logo=html5&logoColor=white&labelColor=1a1208)
![Nessuna dipendenza](https://img.shields.io/badge/Dipendenze-Nessuna-00c896?style=for-the-badge&labelColor=1a1208)
![Web Crypto API](https://img.shields.io/badge/Web_Crypto_API-Native-4af?style=for-the-badge&labelColor=1a1208)
![Zero trasmissioni](https://img.shields.io/badge/Rete-Zero_Trasmissioni-ff6a00?style=for-the-badge&labelColor=1a1208)

<br/>

> *«Custodito da Bastet, forgiato nell'entropia.»*

**BastetCipher** è un generatore crittografico deterministico di password/cifrari ad alta entropia  
e un sistema completo di gestione archivi crittografati,  
completamente client-side, costruito come applicazione HTML5 a file singolo e privo di qualsiasi dipendenza esterna.  
Ogni calcolo avviene interamente nel browser dell'utente — nessun dato viene mai trasmesso, registrato o condiviso.

</div>

---

## 📑 Indice

- [Panoramica](#-panoramica)
- [Caratteristiche principali](#-caratteristiche-principali)
- [Demo](#-demo)
- [Come funziona — Architettura crittografica](#-come-funziona--architettura-crittografica)
  - [Passo 1 — Sale deterministico](#passo-1--sale-deterministico)
  - [Passo 2 — Hash di base (SHA-256, SHA-384, SHA-512)](#passo-2--hash-di-base-sha-256-sha-384-sha-512)
  - [Passo 3 — Seed di trasformazione](#passo-3--seed-di-trasformazione)
  - [Passo 4 — Trasformazione proprietaria a 4 stadi](#passo-4--trasformazione-proprietaria-a-4-stadi)
  - [Passo 5 — Concatenazione degli hash trasformati](#passo-5--concatenazione-degli-hash-trasformati)
  - [Passo 6 — Derivazione chiave PBKDF2-HMAC-SHA512](#passo-6--derivazione-chiave-pbkdf2-hmac-sha512)
  - [Passo 7 — Inserimento deterministico di caratteri speciali](#passo-7--inserimento-deterministico-di-caratteri-speciali)
  - [Passo 7b — Capitalizzazione mista deterministica](#passo-7b--capitalizzazione-mista-deterministica)
  - [Passo 8 — Cifrario finale](#passo-8--cifrario-finale)
- [Il PIM — Personal Iterations Multiplier](#-il-pim--personal-iterations-multiplier)
- [Vault Sacro — Gestore Archivi Crittografati](#-vault-sacro--gestore-archivi-crittografati)
  - [Il formato .bca](#il-formato-bca)
  - [Struttura binaria del file .bca](#struttura-binaria-del-file-bca)
  - [Tab 1 — Crea Archivio .bca](#tab-1--crea-archivio-bca)
  - [Tab 2 — Estrai Archivio .bca](#tab-2--estrai-archivio-bca)
  - [Tab 3 — Apri Archivio .bca — Caveau Virtuale](#tab-3--apri-archivio-bca--caveau-virtuale)
  - [Viewer integrato per tipo di file](#viewer-integrato-per-tipo-di-file)
  - [Ciclo di vita dei dati in memoria](#ciclo-di-vita-dei-dati-in-memoria)
- [Best practice — Sicurezza Operativa](#-best-practice--sicurezza-operativa)
- [Sicurezza e protezioni implementate](#-sicurezza-e-protezioni-implementate)
- [Interfaccia utente e animazioni](#-interfaccia-utente-e-animazioni)
- [Struttura del file](#-struttura-del-file)
- [Utilizzo](#-utilizzo)
- [Requisiti](#-requisiti)
- [Installazione e deploy](#-installazione-e-deploy)
- [Proprietà del cifrario generato](#-proprietà-del-cifrario-generato)
- [Avvertenze e limitazioni](#-avvertenze-e-limitazioni)
- [Licenza](#-licenza)

---

## 🔭 Panoramica

BastetCipher nasce dall'esigenza di avere uno strumento crittografico **completamente autonomo**, **riproducibile** e **offline**, capace di generare cifrari ad alta entropia a partire da una frase segreta e un numero personale (PIM), senza mai affidarsi a server remoti, database o connessioni di rete.

A questo si aggiunge il **Vault Sacro**: un sistema completo per la creazione, l'estrazione e la visualizzazione sicura di archivi crittografati nel formato proprietario `.bca`. Gli archivi sono protetti da una doppia cifratura a cascata (AES-256-GCM + AES-256-CBC) con derivazione delle chiavi PBKDF2-HMAC-SHA512 a 200.000 iterazioni. Il **Caveau Virtuale** consente di aprire e navigare l'archivio direttamente nel browser, interamente in RAM, senza che nessun byte decrittografato tocchi mai il disco.

Il nome è ispirato alla dea egizia **Bastet** — protettrice, guardiana e custode dei segreti — e si riflette nell'intera estetica dell'interfaccia, che ricrea fedelmente l'atmosfera di una camera sacra dell'antico Egitto.

L'applicazione è distribuita come **singolo file `.html` autosufficiente**: basta aprirlo in qualsiasi browser moderno per utilizzarlo, senza installazioni, senza account, senza connessione a Internet.

---

## ✨ Caratteristiche principali

| Caratteristica | Descrizione |
|---|---|
| 🔐 **Crittografia nativa** | Utilizza esclusivamente la [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) nativa del browser — nessuna libreria crittografica di terze parti |
| 🧂 **Sale deterministico** | Il sale viene derivato deterministicamente dall'input, garantendo riproducibilità assoluta a parità di frase e PIM |
| 🔑 **PBKDF2-HMAC-SHA512** | Derivazione della chiave con numero di iterazioni variabile e dipendente dal PIM, nell'intervallo [50.000 – 600.000+] per il cifrario; 200.000 iterazioni fisse per gli archivi .bca |
| 🎲 **Trasformazione proprietaria** | Quattro stadi di trasformazione deterministica (rotazione, scambio, rimappatura hex, inversione a sezioni) applicati prima di PBKDF2 |
| 🔢 **PIM personale** | Un moltiplicatore numerico (1–32 cifre) che altera radicalmente le iterazioni e quindi il cifrario prodotto |
| 🌿 **Caratteri speciali deterministici** | Inserimento di 8–15 caratteri speciali in posizioni pseudocasuali ma completamente riproducibili |
| 🔡 **Capitalizzazione mista** | Esattamente il 50% dei caratteri alfabetici è maiuscolo e il 50% minuscolo, scelti in modo deterministico |
| 🗄️ **Archivi .bca** | Creazione di archivi crittografati nel formato proprietario `.bca` con doppia cifratura AES-256-GCM + AES-256-CBC, compressione deflate-raw, salt e IV casuali |
| 🔍 **Caveau Virtuale** | Apertura e navigazione degli archivi .bca interamente in RAM nel browser, senza alcuna scrittura su disco, con viewer dedicato per ogni tipo di file |
| 🛡️ **Content Security Policy** | CSP rigorosa embedded nell'HTML che blocca script remoti, connessioni esterne, frame e worker |
| 🚫 **Zero trasmissioni di rete** | `connect-src 'none'` nella CSP — nessun dato lascia mai il dispositivo dell'utente |
| 🧱 **Protezione XSS** | Funzione `escapeHTML()` dedicata; costruzione del DOM esclusivamente tramite `textContent` e `createElement` — mai `innerHTML` con dati utente |
| 📄 **File singolo** | Un unico file `.html` autosufficiente, zero dipendenze, zero build step, funziona offline |
| 🎨 **UI immersiva egizio-sacra** | Animazioni CSS, SVG animati, particelle di sabbia su canvas, torce con fiamme procedurali, Bastet animata |

---

## 🎬 Demo

Per eseguire BastetCipher è sufficiente:

```bash
# Clona il repository
git clone https://github.com/tuo-username/BastetCipher.git

# Apri direttamente nel browser
open BastetCipher_ITA.html
# oppure su Windows:
start BastetCipher_ITA.html
```

Nessun server, nessun `npm install`, nessuna dipendenza. **Zero.**

---

## 🔬 Come funziona — Architettura crittografica

BastetCipher implementa una **pipeline crittografica a 8 passi** che combina più primitive crittografiche standard con trasformazioni deterministiche proprietarie, al fine di produrre un cifrario finale di lunghezza elevata, alta entropia e complessità caratteriale uniforme.

```
FRASE SEGRETA + PIM
        │
        ▼
  ┌─────────────┐
  │  PASSO 1    │  SHA-256 → Sale deterministico
  └──────┬──────┘
         │
         ▼
  ┌─────────────┐
  │  PASSO 2    │  SHA-256 + SHA-384 + SHA-512 → tre hash di base (h1, h2, h3)
  └──────┬──────┘
         │
         ▼
  ┌─────────────┐
  │  PASSO 3    │  SHA-256 → Seed di trasformazione
  └──────┬──────┘
         │
         ▼
  ┌─────────────┐
  │  PASSO 4    │  Trasformazione proprietaria a 4 stadi su h1, h2, h3
  └──────┬──────┘
         │
         ▼
  ┌─────────────┐
  │  PASSO 5    │  Concatenazione: ".," + t1 + t2 + t3 + ",."
  └──────┬──────┘
         │
         ▼
  ┌─────────────┐
  │  PASSO 6    │  PBKDF2-HMAC-SHA512 (50.000–600.000+ iterazioni, guidate dal PIM)
  └──────┬──────┘
         │
         ▼
  ┌─────────────┐
  │  PASSO 7    │  Inserimento deterministico di 8–15 caratteri speciali (LCG PRNG)
  └──────┬──────┘
         │
         ▼
  ┌─────────────┐
  │  PASSO 7b   │  Capitalizzazione mista deterministica (50%↑ / 50%↓)
  └──────┬──────┘
         │
         ▼
  ┌─────────────┐
  │  PASSO 8    │  Wrapper finale: ".," + output + ",."
  └──────┬──────┘
         │
         ▼
   CIFRARIO FINALE
```

---

### Passo 1 — Sale deterministico

```javascript
const salt = await sha256('BastetCipher' + input + pim + PEPPER + 'SacredSalt');
```

Il **sale** non è casuale ma **deterministico**: viene calcolato a partire dalla frase segreta, dal PIM, da un prefisso fisso (`'BastetCipher'`), da una costante interna denominata **PEPPER** e da un suffisso di dominio (`'SacredSalt'`).

**Perché un sale deterministico?**  
A differenza degli schemi di hashing delle password (dove il sale casuale è fondamentale per prevenire attacchi con rainbow table), BastetCipher è un generatore di cifrari *riproducibili*: dati gli stessi input, deve sempre produrre lo stesso output. Il sale deterministico garantisce questa proprietà senza sacrificare la diversificazione del dominio.

Il **PEPPER** è una costante Unicode hardcoded nel codice sorgente che include un carattere geroglifico (`𓃠`, U+13060) — funge da "segreto condiviso a livello applicativo" che differenzia BastetCipher da qualsiasi altra implementazione che potesse usare gli stessi algoritmi.

---

### Passo 2 — Hash di base (SHA-256, SHA-384, SHA-512)

```javascript
const h1 = await sha256(input + salt + pim + PEPPER);          // 64 caratteri hex
const h2 = await sha384(salt + input + pim + PEPPER);          // 96 caratteri hex
const h3 = await sha512(input + ':' + salt + ':' + pim + ':' + PEPPER); // 128 caratteri hex
```

Vengono calcolati **tre hash** con funzioni di lunghezza diversa, in ordine diverso degli argomenti. L'utilizzo di SHA-256, SHA-384 e SHA-512 in parallelo serve a:

- **Aumentare la quantità di materiale grezzo** disponibile per i passi successivi (totale: 288 caratteri hex)
- **Diversificare la struttura** del materiale prima della trasformazione proprietaria
- **Garantire che una compromissione parziale** di una delle funzioni hash non comprometta l'intero sistema

---

### Passo 3 — Seed di trasformazione

```javascript
const seed = await sha256(input + pim + PEPPER);
```

Un ulteriore SHA-256 — calcolato con argomenti in ordine diverso dai precedenti — produce il **seed di trasformazione**: un valore hex a 64 caratteri che governa deterministicamente tutti i parametri della trasformazione proprietaria applicata nel passo successivo.

---

### Passo 4 — Trasformazione proprietaria a 4 stadi

La funzione `transformHash(hash, seed)` applica quattro trasformazioni sequenziali e **completamente deterministiche** a ciascuno dei tre hash di base:

#### Stadio 1 — Rotazione ciclica sinistra

```
rotAmt = seedNum % len
output = hash[rotAmt:] + hash[:rotAmt]
```

I caratteri vengono ruotati verso sinistra di un numero di posizioni derivato dal seed. L'ammontare della rotazione è unico per ogni combinazione frase+PIM.

#### Stadio 2 — Scambio a passo variabile

```
swapStep = (seedNum % 7) + 2   // valore tra 2 e 8
```

Per ogni coppia di posizioni distanziate di `swapStep`, i caratteri vengono scambiati. Anche il passo di scambio è derivato dal seed, rendendo il pattern di scambio dipendente dall'input.

#### Stadio 3 — Rimappatura hex tramite tabella di sostituzione

Viene costruita una **permutazione a 16 elementi** dell'alfabeto esadecimale `[0..15]` tramite l'algoritmo Fisher-Yates con un generatore LCG (Linear Congruential Generator) inizializzato dal seed:

```javascript
// LCG: rng = (rng * 1664525 + 1013904223) >>> 0
```

Ogni cifra esadecimale dell'hash viene rimappata secondo questa tabella di sostituzione — in sostanza, una S-box deterministica dipendente dal seed.

#### Stadio 4 — Inversione a sezioni

```
secLen = (seedNum % 12) + 4   // lunghezza sezione: 4..15 caratteri
```

La stringa viene suddivisa in blocchi di lunghezza `secLen` e ogni blocco a indice dispari viene invertito. La lunghezza delle sezioni è ancora una volta derivata dal seed.

---

### Passo 5 — Concatenazione degli hash trasformati

```javascript
const combined = '.,' + t1 + t2 + t3 + ',.';
```

I tre hash trasformati (rispettivamente 64, 96 e 128 caratteri hex) vengono concatenati in una stringa unica di **290 caratteri**, preceduta e seguita dai marcatori `.,` e `,.`. Questo materiale combinato costituisce il "testo in chiaro" che verrà elaborato da PBKDF2.

---

### Passo 6 — Derivazione chiave PBKDF2-HMAC-SHA512

Questo è il passo computazionalmente più costoso e il cuore della resistenza agli attacchi a forza bruta.

#### Calcolo del numero di iterazioni

Il numero di iterazioni **non è fisso** ma dipende matematicamente dal PIM attraverso un meccanismo a due componenti:

```javascript
// Componente 1: mappatura non lineare basata su hash
const pimHash  = await sha256(pim + PEPPER + 'IterSeed');
const hashInt  = parseInt(pimHash.substring(0, 6), 16); // intero a 24 bit: 0..16777215
const baseIter = 50000 + Math.floor((hashInt / 16777215) * 550000); // 50.000..600.000

// Componente 2: offset unico del PIM (mod 65537 × 7)
const twist    = (pimNum % 65537) * 7;

// Iterazioni finali
const iters    = baseIter + twist;
```

| Componente | Descrizione |
|---|---|
| `baseIter` | Derivato dall'hash SHA-256 del PIM: valore distribuito in modo pseudo-uniforme nell'intervallo [50.000, 600.000]. Due PIM diversi producono quasi certamente `baseIter` diversi. |
| `twist` | Offset aggiuntivo calcolato direttamente dal valore numerico del PIM via `(pimNum % 65537) * 7`. Garantisce che PIM simili (es. 1000 e 1001) producano iterazioni significativamente diverse. |

In pratica il numero totale di iterazioni può superare abbondantemente **600.000**, rendendo ogni tentativo di brute-force computazionalmente proibitivo senza conoscere il PIM esatto.

#### Parametri PBKDF2

```javascript
await crypto.subtle.deriveBits({
  name:       'PBKDF2',
  salt:       encode(pbkdf2Salt),   // SHA-256(prefisso + input + pim + PEPPER)
  iterations: iters,                // 50.000 – 600.000+
  hash:       'SHA-512'
}, keyMaterial, 64 * 8);            // 512 bit = 64 byte = 128 caratteri hex
```

L'output è una **stringa hex da 128 caratteri** (512 bit di chiave derivata) che costituisce il materiale grezzo dei passi successivi.

---

### Passo 7 — Inserimento deterministico di caratteri speciali

```javascript
function insertSpecialChars(str, seedHex) {
  const SPECIAL = '!@#$%^&*_-+=~?';
  const rng = createPRNG(seedHex);
  const insertCount = 8 + Math.floor(rng() * 8); // 8..15 inserimenti
  // ...
}
```

Vengono inseriti **8–15 caratteri speciali** in posizioni deterministicamente calcolate tramite un generatore LCG inizializzato dal seed di trasformazione. Il PRNG usato qui è un flusso **indipendente** rispetto a quello dei passi successivi (sono usati seed diversi per i diversi PRNG).

I caratteri speciali sono scelti dal set `!@#$%^&*_-+=~?` — sufficientemente variegato da soddisfare la maggior parte dei requisiti di complessità delle password.

---

### Passo 7b — Capitalizzazione mista deterministica

```javascript
function applyMixedCase(str, seedHex) {
  const rng = createPRNG(seedHex.split('').reverse().join('')); // seed invertito → flusso indipendente
  // Raccoglie tutti gli indici alfabetici
  // Shuffle Fisher-Yates deterministico
  // Esattamente ceil(n/2) caratteri → MAIUSCOLO, il resto → minuscolo
}
```

Su tutti i caratteri alfabetici presenti nella stringa viene applicata una capitalizzazione **deterministicamente bilanciata al 50%**: il seed invertito garantisce un flusso PRNG indipendente dal Passo 7, evitando correlazioni tra inserimento di speciali e maiuscole.

---

### Passo 8 — Cifrario finale

```javascript
const finalCipher = '.,' + withCase + ',.';
```

Il cifrario finale viene racchiuso tra i marcatori `.,` e `,.` — una firma riconoscibile di BastetCipher che facilita il riconoscimento visivo dell'output. La lunghezza totale del cifrario finale è tipicamente compresa tra **150 e 160 caratteri**.

---

## 🔢 Il PIM — Personal Iterations Multiplier

Il **PIM** (Personal Iterations Multiplier) è il secondo fattore di personalizzazione del cifrario, distinto dalla frase segreta. È un numero intero compreso tra 1 e 32 cifre decimali.

### Perché il PIM è critico

- **Un PIM diverso produce un cifrario completamente diverso**, anche con la stessa frase segreta — non esiste alcuna relazione matematica osservabile tra i cifrari prodotti da PIM diversi
- **Il PIM altera il numero di iterazioni PBKDF2** in modo non lineare e dipendente dall'hash, rendendo impossibile la costruzione di rainbow table anche per chi conosce l'algoritmo
- **Il PIM non è memorizzato da nessuna parte** — è responsabilità dell'utente memorizzarlo o conservarlo in modo sicuro
- **PIM più grandi non sono necessariamente più sicuri** di PIM piccoli: un PIM di 6 cifre scelto in modo non ovvio offre già una sicurezza pratica eccellente

### Validazione del PIM

Il campo PIM accetta esclusivamente sequenze numeriche di lunghezza 1–32 e applica due sanitizzazioni in tempo reale:
1. Troncamento automatico a 32 cifre se la lunghezza viene superata
2. Rimozione degli zeri iniziali non significativi

### Amplificatore entropico
Questo è semplicissimo, inserisci la cifra, più è alta e più lunga sarà la tua password deterministica generata, serve se l'utente non si accontenta mai e vuole ancora più sicurezza nel caso sia estremamente paranoico!

---

## 🗄️ Vault Sacro — Gestore Archivi Crittografati

Il **Vault Sacro** è la seconda macro-funzionalità di BastetCipher, accessibile nella sezione inferiore dell'interfaccia. Permette di creare, estrarre e navigare archivi crittografati nel formato proprietario `.bca` — interamente nel browser, senza alcuna dipendenza esterna e senza trasmissione di dati in rete.

Il Vault è organizzato in **tre tab**:

| Tab | Funzione |
|---|---|
| 𓃠 Crea Archivio .bca | Cifra uno o più file in un singolo archivio .bca protetto da password |
| 𓂀 Estrai Archivio .bca | Decifra un archivio .bca e scarica il contenuto come .zip |
| 𓇯 Apri Archivio .bca | Apre l'archivio in un Caveau Virtuale in-memory, senza estrarre nulla su disco |

---

### Il formato .bca

`.bca` (BastetCipher Archive) è un formato binario proprietario progettato per massimizzare la sicurezza e la portabilità. Ogni archivio è un singolo file autocontenuto che racchiude un numero arbitrario di file (fino a 1024) compressi e cifrati con doppio strato crittografico.

#### Algoritmi utilizzati

| Livello | Algoritmo | Parametri |
|---|---|---|
| Compressione | deflate-raw | Tramite `CompressionStream` nativa del browser |
| Derivazione chiavi | PBKDF2-HMAC-SHA512 | 200.000 iterazioni, salt casuale 256 bit |
| Cifratura Strato 1 | AES-256-GCM | IV casuale 96 bit, autenticazione integrata (tag 128 bit) |
| Cifratura Strato 2 | AES-256-CBC | IV casuale 128 bit, applicato sopra l'output GCM |
| Integrità file | CRC-32 | Calcolato sui dati originali prima della compressione |

#### Doppia cifratura a cascata

La scelta di combinare GCM e CBC è deliberata e ragionata:

- **AES-256-GCM** (Strato 1 — interno): fornisce cifratura autenticata. Il tag di autenticazione GCM permette di rilevare immediatamente qualsiasi manomissione del ciphertext prima di procedere. Una password errata o un file corrotto vengono rifiutati a questo livello.
- **AES-256-CBC** (Strato 2 — esterno): aggiunge un secondo strato di cifratura con chiave derivata indipendente. Anche se per ipotesi la chiave GCM venisse compromessa, l'attaccante si troverebbe ancora davanti al layer CBC cifrato.

Le due chiavi (k1 per GCM, k2 per CBC) sono derivate separatamente dagli stessi 512 bit di output PBKDF2:

```javascript
k1 = bits[0..31]   // primi 32 byte → chiave AES-256-GCM
k2 = bits[32..63]  // secondi 32 byte → chiave AES-256-CBC
```

---

### Struttura binaria del file .bca

```
┌────────────────────────────────────────────────────────────────┐
│  MAGIC       4 byte   Identificatore formato: 0x42 0x43 0x41 0x21 ("BCA!")
│  VERSION     1 byte   Versione del formato (attualmente: 1)
│  SALT       32 byte   Salt casuale per PBKDF2 (generato con crypto.getRandomValues)
│  ITERATIONS  4 byte   Numero di iterazioni PBKDF2 (little-endian uint32)
│  IV1        12 byte   IV casuale per AES-256-GCM (96 bit)
│  IV2        16 byte   IV casuale per AES-256-CBC (128 bit)
├────────────────────────────────────────────────────────────────┤
│  CIPHERTEXT  N byte   Output AES-CBC( AES-GCM( PAYLOAD ) )
│                       dove PAYLOAD è strutturato come:
│                       ┌─ FILE_COUNT  2 byte  (uint16 little-endian)
│                       │  Per ogni file:
│                       │  ┌─ NAME_LEN   2 byte  (uint16 little-endian)
│                       │  │  NAME       N byte  (UTF-8)
│                       │  │  CRC32      4 byte  (uint32 little-endian)
│                       │  │  ORIG_SIZE  4 byte  (uint32 little-endian)
│                       │  │  COMP_SIZE  4 byte  (uint32 little-endian)
│                       │  └─ DATA       N byte  (deflate-raw compresso)
└────────────────────────────────────────────────────────────────┘
```

Ogni campo del payload è presente solo nel plaintext interno, mai visibile nel file .bca senza la password corretta. Il magic number e il version byte sono gli unici dati in chiaro, insieme ai parametri crittografici (salt, IV) che per definizione non richiedono segretezza.

---

### Tab 1 — Crea Archivio .bca

La tab di creazione permette di selezionare fino a **1024 file** di qualsiasi formato (tramite drag & drop o selettore file) e cifrarli in un unico archivio `.bca`.

#### Procedura interna step-by-step

1. **Selezione file**: i file vengono letti con `FileReader.readAsArrayBuffer()` e tenuti in memoria come `Uint8Array`. Non vengono mai scritti su disco né inviati altrove.
2. **Inserimento password**: l'utente inserisce la password di cifratura. Il campo è di tipo `password`, con `autocomplete="new-password"` e `spellcheck="false"`.
3. **Generazione parametri casuali**: tramite `crypto.getRandomValues()` vengono generati salt (32 byte), IV1 (12 byte) e IV2 (16 byte) — valori unici per ogni archivio creato, rendendo impossibile il riutilizzo di ciphertext.
4. **Derivazione chiavi**: PBKDF2-HMAC-SHA512 con 200.000 iterazioni produce 512 bit da cui vengono estratte k1 (per GCM) e k2 (per CBC). Il buffer dei bit grezzi viene azzerato con `.fill(0)` subito dopo l'estrazione.
5. **Compressione**: ogni file viene compresso individualmente con `CompressionStream('deflate-raw')`. Viene calcolato il CRC-32 sui dati originali prima della compressione.
6. **Costruzione payload**: i file compressi vengono assemblati nel payload binario con le rispettive intestazioni (nome, CRC, dimensioni).
7. **Doppia cifratura**: il payload viene cifrato prima con AES-256-GCM (k1, IV1), poi il risultato con AES-256-CBC (k2, IV2).
8. **Assemblaggio .bca**: header + ciphertext vengono concatenati e scaricati come file `.bca` tramite un Blob URL temporaneo, revocato subito dopo il click.

La barra di avanzamento mostra lo stato in tempo reale con messaggi descrittivi per ogni fase.

---

### Tab 2 — Estrai Archivio .bca

La tab di estrazione permette di decriptare un archivio `.bca` e scaricare il contenuto originale come file `.zip` standard.

#### Procedura interna step-by-step

1. **Caricamento**: il file `.bca` viene caricato tramite drag & drop o selettore e letto interamente in `ArrayBuffer`.
2. **Validazione header**: vengono verificati magic number (4 byte) e versione. Un file non riconosciuto viene rifiutato immediatamente.
3. **Lettura parametri**: salt, numero di iterazioni, IV1 e IV2 vengono estratti dall'header.
4. **Derivazione chiavi**: identica al processo di creazione, con la stessa password e lo stesso salt letto dall'header.
5. **Decifratura strato 2 (CBC)**: se la password è errata, la decifratura CBC produce output casuale che farà fallire la verifica GCM al passo successivo.
6. **Decifratura strato 1 (GCM)**: l'autenticazione GCM verifica integrità e autenticità. Password errata o file manomesso → errore immediato e rifiuto.
7. **Parsing payload**: il plaintext viene analizzato per ricostruire la lista file con nomi, dimensioni e dati compressi.
8. **Decompressione e verifica CRC**: ogni file viene decompresso con `DecompressionStream('deflate-raw')` e il CRC-32 viene verificato rispetto al valore registrato nell'archivio.
9. **Costruzione ZIP**: i file decompressi vengono assemblati in un archivio `.zip` standard (formato PKZip con directory centrale e EOCD) interamente in memoria.
10. **Download**: lo zip viene scaricato tramite Blob URL temporaneo, revocato dopo 1,5 secondi.

---

### Tab 3 — Apri Archivio .bca — Caveau Virtuale

Il **Caveau Virtuale** è la funzionalità più avanzata del Vault. Permette di aprire un archivio `.bca` e sfogliarne il contenuto direttamente nel browser, **senza estrarre mai nulla su disco** e senza che i dati decrittografati lascino la RAM del browser.

Il paradigma di sicurezza è analogo a quello di un volume crittografato montato con VeraCrypt: i file diventano accessibili finché il caveau è aperto, e scompaiono completamente alla chiusura.

#### Apertura del Caveau

1. Il file `.bca` viene caricato e mantenuto in `ArrayBuffer` in memoria.
2. L'utente inserisce la password — che viene **azzerata dall'input UI immediatamente** dopo la derivazione delle chiavi, prima ancora di restituire il controllo all'utente.
3. `parseBCA()` decifra l'archivio e restituisce la lista delle entry con i rispettivi dati compressi in RAM.
4. Viene mostrata la lista dei file navigabile con nome, dimensione originale e pulsante di apertura.

#### Apertura di un singolo file

Quando l'utente clicca su un file dalla lista:

1. I dati compressi vengono decompresso con `DecompressionStream('deflate-raw')` — interamente in memoria.
2. Viene costruito un documento HTML completo (`srcdoc`) contenente il file embedded come **data URL** (`data:<mime>;base64,...`).
3. Il documento `srcdoc` viene iniettato nell'iframe del viewer — **senza alcun URL**, senza blob URL, senza richieste di rete.
4. Il buffer decompresso viene azzerato con `.fill(0)` subito dopo la costruzione dello srcdoc.

**Eccezione — PDF**: il viewer nativo di Chrome/Firefox per i PDF richiede internamente iframe con URL `chrome-extension://...` che non possono essere ospitati dentro uno srcdoc. Per i PDF viene quindi creato un Blob URL (`URL.createObjectURL()`), caricato nell'iframe tramite `frame.src`, e **revocato subito dopo il caricamento** (`frame.onload`). Il contenuto rimane in memoria nel renderer del browser ma il riferimento URL non esiste più.

#### Chiusura del Caveau

Alla chiusura (pulsante dedicato o chiusura del viewer):

1. Il Blob URL eventualmente aperto viene revocato con `URL.revokeObjectURL()`.
2. L'iframe viene svuotato: `frame.removeAttribute('srcdoc')` + `frame.removeAttribute('src')` + `frame.src = 'about:blank'`. Questo forza il browser a distruggere il documento corrente dal rendering engine.
3. Ogni entry dell'archivio in memoria ha i propri dati compressi azzerati con `.fill(0)`.
4. Il buffer del file `.bca` originale viene azzerato con `.fill(0)`.
5. L'interfaccia viene ripristinata allo stato iniziale.

---

### Viewer integrato per tipo di file

Il Caveau Virtuale include un viewer dedicato per ogni categoria di file. Tutto il contenuto viene renderizzato tramite `srcdoc` con un documento HTML costruito dinamicamente in JavaScript, che include una **Content Security Policy interna** (`<meta http-equiv="Content-Security-Policy">`) con `default-src 'none'` — nessuna risorsa esterna può essere caricata dal viewer, nemmeno per errore.

| Tipo | Estensioni | Metodo di visualizzazione |
|---|---|---|
| **Immagini** | jpg, jpeg, png, gif, webp, svg, bmp | `<img src="data:image/...;base64,...">` su sfondo scuro |
| **Audio** | mp3, ogg, wav, flac, aac | `<audio controls src="data:audio/...;base64,...">` con player nativo |
| **Video** | mp4, webm, mov, avi, mkv | `<video controls src="data:video/...;base64,...">` con player nativo |
| **PDF** | pdf | Blob URL + `<embed>` nel viewer nativo del browser (iframe senza sandbox) |
| **Testo / Codice** | txt, md, json, xml, csv, js, ts, py, sh, css | `<pre>` con testo HTML-escaped, font monospace per codice |
| **HTML** | html, htm | srcdoc con `sandbox="allow-scripts"` — opaque origin, isolamento completo |
| **Fallback** | qualsiasi altro formato | Schermata informativa con tipo MIME e dimensione |

#### Sicurezza del sandbox per HTML

I file HTML sono l'unica categoria che potrebbe contenere JavaScript potenzialmente malevolo. Per questo vengono aperti con `sandbox="allow-scripts"` senza `allow-same-origin`: l'iframe opera su un'**origine opaca** — i suoi script non possono accedere a cookie, localStorage, né interagire con la pagina madre. È il massimo isolamento ottenibile nel browser senza WebAssembly.

Per tutti gli altri tipi di file (non HTML), il sandbox viene rimosso per permettere al browser di usare i propri viewer nativi (PDF viewer, player audio/video), ma la CSP interna allo srcdoc garantisce che nessuna risorsa esterna venga mai caricata.

---

### Ciclo di vita dei dati in memoria

```
File originali
      │
      ▼ read as ArrayBuffer
RAM ──┤ buffer .bca (ArrayBuffer)
      │
      ▼ parseBCA() → decrypt → decompress
RAM ──┤ entries[] con .compressed (Uint8Array per file)
      │
      ▼ vaultOpenFile() → DecompressionStream
RAM ──┤ data (Uint8Array decompresso) → srcdoc string → data.fill(0)
      │
      ▼ frame.srcdoc = ...  (o Blob per PDF)
RAM ──┤ documento nel renderer iframe
      │
      ▼ vaultCloseViewer()
RAM ──┤ frame.src = 'about:blank' → renderer distrugge documento
      │
      ▼ vaultCloseVault()
RAM ──┤ entry.compressed.fill(0) per ogni file
      │  buffer .bca fill(0)
      ▼
    (tutto azzerato, GC può raccogliere)
```

**Garanzie offerte:**
- ✅ Zero scritture su disco volontarie in qualsiasi fase
- ✅ Blob URL revocato dopo il caricamento (PDF) o mai creato (altri tipi)
- ✅ Buffer azzerati esplicitamente alla chiusura
- ✅ Password azzerata dall'input UI subito dopo la derivazione
- ✅ Nessuna connessione di rete (`connect-src 'none'` nella CSP)
- ✅ Nessun accesso a storage persistente (localStorage, IndexedDB, cookie)

**Limitazioni inevitabili (comuni a qualsiasi software):**
- ⚠️ La RAM volatile è fuori dal controllo del JavaScript: il browser/OS può mantenere copie interne per GC, JIT compiler, ecc.
- ⚠️ Se il sistema va in ibernazione con il Caveau aperto, i dati potrebbero finire su disco (vedi sezione Best Practice)
- ⚠️ Se la RAM è sotto pressione, il sistema operativo può fare swap su disco senza che il browser possa impedirlo

---

## 🔒 Best practice — Sicurezza Operativa

Il Vault integra nell'interfaccia un pannello di **avvisi operativi** che ricorda all'utente le misure di sistema necessarie per raggiungere il massimo livello di sicurezza. Di seguito il dettaglio completo.

### 🔴 Disabilita l'ibernazione

Quando il PC va in ibernazione, l'**intera RAM viene scritta su disco** (file `hiberfil.sys` su Windows, partizione swap su Linux). Qualsiasi dato aperto nel Caveau in quel momento finisce nel file di ibernazione in chiaro.

```bash
# Windows
powercfg /h off

# macOS
sudo pmset -a hibernatemode 0

# Linux
# Rimuovi la riga resume= dalla configurazione GRUB
# e disabilita la partizione di swap dall'ibernazione nel file /etc/fstab
```

### 🔴 Disabilita la Swap / Memoria virtuale

Se la RAM fisica è satura, il sistema operativo sposta pagine di memoria su disco (swap/pagefile). Questo può accadere silenziosamente in background e può coinvolgere dati decrittografati anche senza ibernazione.

```bash
# Windows
# Sistema → Impostazioni di sistema avanzate → Prestazioni
# → Opzioni avanzate → Memoria virtuale → Nessun file di paging

# macOS
# La swap è gestita automaticamente dal sistema.
# Attiva FileVault per cifrare il volume di sistema, così la swap è cifrata.

# Linux (disabilita temporaneamente)
sudo swapoff -a

# Linux (permanente: rimuovi o commenta le righe swap in /etc/fstab)
# Es: commenta la riga che contiene "swap" o "none swap sw 0 0"
```

### 🟡 Cifra il disco (alternativa se la swap non è disattivabile)

Se non è possibile disattivare la swap, la crittografia dell'intero disco garantisce che i dati finiti nella swap rimangano inaccessibili senza la chiave di avvio.

| Sistema | Strumento consigliato | Note |
|---|---|---|
| Windows | **VeraCrypt** | ⚠️ BitLocker **non è consigliato**: è closed source, per impostazione predefinita carica le chiavi di ripristino sull'account Microsoft (server di terze parti fuori dal controllo dell'utente), ed è soggetto alla legislazione statunitense (FISA, NSL) che può imporre la consegna di dati con obbligo di segretezza verso l'utente finale. **VeraCrypt** è open source, sottoposto ad audit indipendenti pubblici, senza account, senza cloud e senza alcuna chiave su server esterni. |
| macOS | FileVault 2 | Integrato nel sistema, cifra l'intero volume inclusa la swap |
| Linux | LUKS con dm-crypt | Standard de facto, integrato nel kernel, audit pubblici disponibili |

### 🟡 Cancella i file originali in modo sicuro

Dopo aver creato l'archivio `.bca`, i file originali non crittografati devono essere eliminati con una **cancellazione sicura a più passate** per renderli irrecuperabili con strumenti forensi, anche da spazio non allocato.

| Sistema | Strumento | Algoritmo |
|---|---|---|
| Windows | **Eraser** | Gutmann (35 passate) o DoD 5220.22-M (7 passate) |
| macOS | `rm -P file` oppure **Permanent Eraser** | Sovrascrittura multipla |
| Linux | `shred -vuz -n 35 file` | Gutmann (35 passate) |
| Linux | `wipe -rf directory/` | Sovrascrittura ricorsiva |

> ⚠️ **Nota importante per SSD e NVMe**: su unità a stato solido la cancellazione sicura per sovrascrittura è **meno affidabile** a causa del **wear leveling** — il controller dell'SSD può scrivere i dati in settori fisici diversi da quelli logici indirizzati dal sistema operativo, lasciando copie residue in settori non mappati. Su SSD, la **crittografia del disco** (LUKS, FileVault, VeraCrypt) è la protezione primaria più affidabile: se il disco è cifrato fin dall'inizio, i dati "non cancellati" sono comunque inaccessibili senza la chiave.

---

## 🛡️ Sicurezza e protezioni implementate

### Content Security Policy (CSP)

BastetCipher incorpora una **CSP rigorosa** direttamente nel tag `<meta>`:

```
default-src  'none'
script-src   'self' 'unsafe-inline'   ← solo script inline dello stesso file
style-src    'self' 'unsafe-inline' data:
img-src      'self' data:             ← solo il favicon in data-URI
font-src     'self' data:
media-src    data:                    ← audio/video solo come data URL (Caveau Virtuale)
object-src   data:                    ← embed PDF solo come data URL (Caveau Virtuale)
connect-src  'none'                   ← ZERO connessioni di rete
frame-src    blob:                    ← solo blob URL per PDF nel viewer nativo
worker-src   'none'
base-uri     'none'
form-action  'none'
```

`connect-src 'none'` è la direttiva più importante: rende **tecnicamente impossibile** qualsiasi trasmissione di dati verso server esterni, anche in caso di exploit nel codice JavaScript.

`frame-src blob:` è limitato all'unico caso necessario (PDF nel viewer nativo) — tutti gli altri tipi di file vengono aperti tramite `srcdoc` che non richiede alcuna direttiva `frame-src`.

`media-src data:` e `object-src data:` permettono rispettivamente la riproduzione di audio/video e la visualizzazione di PDF embedded come data URL nel Caveau Virtuale — esclusivamente dati già in memoria, nessuna risorsa di rete.

### CSP interna agli srcdoc del Caveau Virtuale

Ogni documento generato per il viewer del Caveau include una propria CSP interna:

```html
<meta http-equiv="Content-Security-Policy"
  content="default-src 'none';
           style-src 'unsafe-inline';
           script-src 'none';
           img-src data:;
           media-src data:;
           object-src data:;">
```

Questa CSP è **additiva** rispetto a quella della pagina madre (che viene ereditata dagli srcdoc senza sandbox): può solo aggiungere restrizioni, mai allargarle. Il risultato è che ogni documento nel viewer è completamente isolato dalla rete e da qualsiasi risorsa esterna.

### Protezione XSS

L'unica sezione dell'interfaccia che visualizza dati computati dal cifrario (il pannello statistiche) è costruita **esclusivamente tramite DOM API sicure**:

```javascript
// ✅ Sicuro — costruzione DOM esplicita
const badge  = document.createElement('span');
badge.appendChild(document.createTextNode(label + ' '));
const strong = document.createElement('strong');
strong.textContent = value;   // textContent — mai interpretato come HTML

// ❌ Mai usato con dati utente
// element.innerHTML = userInput;  // VIETATO
```

La funzione `escapeHTML()` è disponibile come guardia aggiuntiva per eventuali futuri utilizzi, ma il codice attuale non si affida a essa per la sicurezza strutturale — preferisce la costruzione DOM esplicita.

Anche i nomi dei file visualizzati nel Caveau Virtuale vengono sanitizzati alla lettura (`replace(/[\x00-\x1f<>"'\`&/\\]/g, '')`) prima di essere inseriti nel DOM o usati nelle stringhe srcdoc.

### Isolamento dell'esecuzione

- **Nessuna comunicazione tra schede o origini**: assenza di `postMessage`, `BroadcastChannel` o `SharedWorker`
- **Nessun accesso a storage persistente**: nessun utilizzo di `localStorage`, `sessionStorage`, `IndexedDB` o cookie
- **Nessuna libreria esterna**: zero CDN, zero `<script src="...">` — l'intera applicazione è autocontenuta
- **Azzeramento esplicito dei buffer sensibili**: tutti i `Uint8Array` contenenti dati crittografici vengono azzerati con `.fill(0)` subito dopo l'uso

### Web Crypto API — Sicurezza algoritmica

Tutte le operazioni crittografiche delegano alla **Web Crypto API** nativa del browser, che:
- Utilizza implementazioni crittografiche validate e certificate a livello di sistema operativo
- Esegue le operazioni sensibili in un contesto privilegiato, isolato dal JavaScript applicativo
- Non espone il materiale crittografico grezzo al garbage collector JavaScript

---

## 🎨 Interfaccia utente e animazioni

BastetCipher implementa un'interfaccia tematica ispirata all'antico Egitto, costruita interamente in HTML, CSS e SVG — senza canvas 2D per l'UI (escluse le particelle) e senza WebGL.

### Componenti visivi principali

| Componente | Implementazione | Descrizione |
|---|---|---|
| **Statua di Bastet** | SVG inline (120×140 viewBox) | Scultura felina stilizzata con collare, gioielli, ankh, occhi animati via JS con pulsazione cromatica ciclica |
| **Grande Bastet** | SVG inline (800×900 viewBox) | Illustrazione dettagliata con pelo multistrato, collare con gemme (turchese, lapislazzuli, rubino), animazione ammiccamento, coda oscillante, rispiro del petto, alone solare |
| **Piramide di sfondo** | SVG poligonale + animazione CSS | Tre livelli di poligoni concentrici con Occhio di Ra; animazione `pyramidPulse` con drop-shadow dorato; modalità attiva durante la generazione |
| **Torce** | SVG + CSS keyframes | Quattro torce angolari con fiamme a quattro strati (core, mid, tip, inner) animate indipendentemente; braci (`ember`) fluttuanti su variabili CSS custom |
| **Glifi murali** | SVG laterali | Due colonne di geroglifici egizi (Unicode range `𓃠`–`𓋹`) in posizione `fixed`, specchiati tramite `scaleX(-1)` |
| **Particelle di sabbia** | Canvas 2D | 80 particelle in floating con ciclo di vita, colori oro e sabbia, animazione RAF (requestAnimationFrame) |
| **Ruota del cifrario** | SVG + `wheelSpin` | Disco con tacche e geroglifici in rotazione continua (20s); accelera a 1s durante la generazione |
| **Indicatori occhi** | CSS + JS | Due orb ellittici che si attivano con `eyePulse` durante la generazione; icona lucchetto con animazione `lockBounce` |
| **Esplosioni di rune** | DOM injection + CSS | Al click sul pulsante genera 14 elementi `div.rune-burst` con traiettorie radiali calcolate via variabili CSS `--tx`, `--ty` |
| **Effetto macchina da scrivere** | `setInterval` JS | Output del cifrario rivelato a blocchi di 8 caratteri ogni 18ms con cursore a blocco lampeggiante |
| **Barra di avanzamento** | DOM + JS | Barra con gradient animato che si aggiorna a ogni passo della pipeline; testo di stato sovrapposto con `mix-blend-mode: difference` |
| **Vault Sacro** | HTML + CSS tematico verde-smeraldo | Sezione separata con palette verde (#00c896) e bordi animati in contrasto con la palette oro del generatore |
| **Viewer del Caveau** | Overlay full-screen + iframe | Finestra modale con header sicuro, iframe per il contenuto, barra di sicurezza in calce con status in tempo reale |
| **Pannello Best Practice** | CSS grid 2×2 | Quattro card informative con bordo rosso-cremisi e accenti dorati per trasmettere urgenza visiva |

### Animazioni CSS principali

```css
/* Selezione rappresentativa delle keyframe */
@keyframes torchFlicker    /* oscillazione opacità/scala fiamme — 2.5s */
@keyframes flameDance      /* deformazione fiamma principale — 0.9s alternate */
@keyframes pyramidPulse    /* pulsazione drop-shadow piramide — 6s */
@keyframes titleShimmer    /* variazione filter sull'h1 — 4s */
@keyframes altarGlow       /* box-shadow sul pannello altare — 8s */
@keyframes runeBurstAnim   /* esplosione rune radiale — 1.2s forwards */
@keyframes emberFloat      /* braci che salgono con variabili CSS — 1–1.8s */
@keyframes eyePulse        /* pulsazione box-shadow occhi attivi — 1s */
@keyframes bandScroll      /* scorrimento infinito bande decorative — 12s */
@keyframes vaultGlow       /* pulsazione bordo smeraldo del Vault — 8s */
@keyframes noticePulse     /* pulsazione sottile pannello best practice — 6s */
```

---

## 📁 Struttura del file

BastetCipher è distribuito come **file unico autocontenuto**. La struttura interna è organizzata nelle seguenti sezioni:

```
BastetCipher_ITA.html
│
├── <head>
│   ├── Content Security Policy (meta http-equiv)
│   ├── Viewport meta
│   ├── Favicon SVG inline (data-URI)
│   └── <style> — ~900 righe CSS
│       ├── Variabili CSS (:root)
│       ├── Camera di sfondo + texture muro
│       ├── Canvas particelle
│       ├── Glifi murali
│       ├── Torce e fiamme
│       ├── Piramide di sfondo
│       ├── Layout principale (#app)
│       ├── Header e titolo
│       ├── Pannello altare (#altar)
│       ├── Ruota cifrario + indicatori
│       ├── Elementi form (input, label)
│       ├── Pulsante genera
│       ├── Barra di avanzamento
│       ├── Pannello output
│       ├── Pulsante copia
│       ├── Statistiche cifrario
│       ├── Piè di pagina
│       ├── Animazione esplosione rune
│       ├── Stato errore
│       ├── Vault Sacro (#archive-vault) — stile smeraldo
│       ├── Dropzone, file list, input, pulsanti vault
│       ├── Vault Browse — lista file navigabile
│       ├── Viewer overlay (#vault-viewer-overlay)
│       ├── Pannello best practice (.vault-security-notice)
│       └── Media query responsive
│
├── <body>
│   ├── #chamber — overlay sfondo
│   ├── #particles-canvas — canvas particelle
│   ├── #bg-pyramid — SVG piramide
│   ├── .wall-glyphs.left/.right — SVG glifi murali
│   ├── .torch.tl/.tr — torce superiori (con SVG fiamme)
│   ├── #app — contenitore principale
│   │   ├── <header #header>
│   │   │   ├── #bastet-svg — SVG statua Bastet piccola
│   │   │   ├── .app-title
│   │   │   ├── .app-tagline
│   │   │   └── .divider-runes
│   │   ├── <main #altar>
│   │   │   ├── .altar-band (top)
│   │   │   ├── .altar-top-row
│   │   │   │   ├── #cipher-wheel-wrap — SVG ruota
│   │   │   │   ├── #bastet-eyes-wrap — indicatori stato
│   │   │   │   └── SVG chiave antica
│   │   │   ├── #input-phrase — campo frase segreta
│   │   │   ├── #input-pim — campo PIM
│   │   │   ├── #error-msg — messaggio errore
│   │   │   ├── #btn-generate — pulsante genera
│   │   │   ├── #status-bar — barra avanzamento
│   │   │   ├── #output-section — sezione output
│   │   │   │   ├── #output-panel
│   │   │   │   │   ├── #swirl-overlay
│   │   │   │   │   └── #output-text
│   │   │   │   └── #cipher-stats
│   │   │   └── .altar-band.bottom
│   │   ├── <section #archive-vault>
│   │   │   ├── .vault-band (top)
│   │   │   ├── .vault-title-row
│   │   │   ├── .vault-tabs
│   │   │   │   ├── #vault-tab-create — tab Crea
│   │   │   │   ├── #vault-tab-extract — tab Estrai
│   │   │   │   └── #vault-tab-browse — tab Apri (Caveau Virtuale)
│   │   │   ├── #vault-panel-create — pannello creazione
│   │   │   │   ├── #vault-create-dz — dropzone file
│   │   │   │   ├── #vault-create-filelist — lista file selezionati
│   │   │   │   ├── #vault-create-pw — campo password
│   │   │   │   ├── #vault-create-btn — pulsante crea
│   │   │   │   ├── #vault-status-create — barra avanzamento
│   │   │   │   └── .vault-algo-row — pill algoritmi
│   │   │   ├── #vault-panel-extract — pannello estrazione
│   │   │   │   ├── #vault-extract-dz — dropzone .bca
│   │   │   │   ├── #vault-extract-pw — campo password
│   │   │   │   ├── #vault-extract-btn — pulsante estrai
│   │   │   │   └── #vault-status-extract — barra avanzamento
│   │   │   ├── #vault-panel-browse — pannello Caveau Virtuale
│   │   │   │   ├── #vault-browse-dz — dropzone .bca
│   │   │   │   ├── #vault-browse-pw — campo password
│   │   │   │   ├── #vault-browse-btn — pulsante apri caveau
│   │   │   │   ├── #vault-browse-filelist — lista navigabile
│   │   │   │   └── #vault-browse-footer — pulsante chiudi caveau
│   │   │   ├── .vault-security-notice — pannello best practice
│   │   │   │   ├── card Disabilita ibernazione
│   │   │   │   ├── card Disabilita swap
│   │   │   │   ├── card Cifra il disco (+ nota BitLocker/VeraCrypt)
│   │   │   │   └── card Cancella file originali in modo sicuro
│   │   │   └── .vault-band.bottom
│   │   ├── <footer #footer>
│   │   └── #MILU — SVG grande Bastet decorativa
│   │
│   ├── #vault-viewer-overlay — overlay viewer (display:none di default)
│   │   └── #vault-viewer-box
│   │       ├── #vault-viewer-header (icona, nome file, info, pulsante chiudi)
│   │       ├── #vault-viewer-body
│   │       │   └── #vault-viewer-frame — iframe per il contenuto
│   │       └── #vault-viewer-security-bar — barra di stato sicurezza
│   │
│   └── <script>
│       ├── escapeHTML() — utilità XSS
│       ├── initParticles() — IIFE canvas particelle
│       ├── spawnRuneBursts() — effetto esplosione
│       ├── startRuneAnimation() / stopRuneAnimation()
│       ├── encode() / bufToHex()
│       ├── sha256() / sha384() / sha512()
│       ├── pbkdf2()
│       ├── transformHash() — trasformazione proprietaria
│       ├── createPRNG() — LCG seed
│       ├── insertSpecialChars()
│       ├── applyMixedCase()
│       ├── runCipherPipeline() — pipeline principale
│       ├── generateCipher() — handler UI
│       ├── copyCipher() — copia negli appunti
│       ├── animateBastetEyes() — IIFE animazione occhi
│       ├── keydown listener (Enter → generateCipher)
│       ├── input listener PIM (validazione lunghezza)
│       ├── [VAULT]
│       ├── BCA_MAGIC / BCA_VERSION / BCA_ITERS — costanti formato
│       ├── vConcat() / vU16() / vU32() — utility binarie
│       ├── initCRC() / crc32V() — CRC-32
│       ├── vCompress() — deflate-raw via CompressionStream
│       ├── deriveVaultKeys() — PBKDF2 per archivi
│       ├── vFmtSize() — formattazione dimensioni
│       ├── buildBCA() — costruzione archivio cifrato
│       ├── parseBCA() — parsing e decifratura archivio
│       ├── buildZipFromEntries() — costruzione ZIP output
│       ├── vaultDownload() — download tramite Blob URL temporaneo
│       ├── vaultState — stato UI create/extract
│       ├── vaultSwitchTab() — cambio tab
│       ├── vaultAddFiles() / vaultRenderFileList() — gestione file
│       ├── vaultShowMsg() / vaultSetProg() / vaultEndProg()
│       ├── vaultCreateArchive() — handler creazione
│       ├── vaultExtractArchive() — handler estrazione
│       ├── initVaultUI() — IIFE inizializzazione event listener
│       ├── vaultLoadBCA() — caricamento .bca per estrazione
│       ├── vaultLoadBrowseBCA() — caricamento .bca per navigazione
│       ├── vaultBrowseState — stato Caveau Virtuale
│       ├── vaultBrowseSetProg() / vaultBrowseShowMsg()
│       ├── vaultOpenVault() — apertura e decifratura caveau
│       ├── vaultBrowseRenderList() — rendering lista file
│       ├── vaultFileIcon() — icona per tipo file
│       ├── vaultMime() — tipo MIME per estensione
│       ├── vaultDecompress() — decompressione singolo file
│       ├── uint8ToBase64() — conversione chunked Uint8→base64
│       ├── buildSrcdoc() — costruzione documento viewer per tipo
│       ├── vaultOpenFile() — apertura singolo file nel viewer
│       ├── vaultCloseViewer() — chiusura viewer e pulizia memoria
│       └── vaultCloseVault() — chiusura caveau e azzeramento RAM
```

---

## 🚀 Utilizzo

### Generatore di cifrari

1. Apri `BastetCipher_ITA.html` in un browser moderno
2. Inserisci la tua **frase segreta** nel campo apposito
3. Inserisci il tuo **PIM** (valore numerico da 1 a 32 cifre)
4. Premi il pulsante **Genera Cifrario** oppure premi `Invio`
5. Attendi il completamento della pipeline (indicata dalla barra di avanzamento)
6. Il cifrario appare nel pannello output con effetto macchina da scrivere
7. Usa il pulsante **Copia negli Appunti** per copiare il risultato

### Crea un archivio .bca

1. Scorri fino alla sezione **Vault Sacro**
2. Seleziona la tab **𓃠 Crea Archivio .bca**
3. Trascina i file nella dropzone oppure cliccala per selezionarli (max 1024 file)
4. Inserisci la password di cifratura
5. Premi **Crea e Cifra Archivio .bca**
6. Il file `.bca` viene scaricato automaticamente al termine

### Estrai un archivio .bca

1. Seleziona la tab **𓂀 Estrai Archivio .bca**
2. Trascina il file `.bca` nella dropzone
3. Inserisci la password
4. Premi **Decifra ed Estrai → .zip**
5. Il file `.zip` con il contenuto originale viene scaricato automaticamente

### Aprire il Caveau Virtuale (senza estrarre su disco)

1. Seleziona la tab **𓇯 Apri Archivio .bca**
2. Trascina il file `.bca` nella dropzone
3. Inserisci la password
4. Premi **Apri Caveau Virtuale — Solo In Memoria**
5. La lista dei file appare: clicca **𓂀 Apri** su qualsiasi file per visualizzarlo nel viewer integrato
6. Al termine, clicca **𓃠 Chiudi Caveau e Distruggi Dati dalla RAM** per azzerare tutto

### Riproducibilità del cifrario

> **Proprietà fondamentale:** BastetCipher è completamente **deterministico**.  
> La stessa frase segreta + lo stesso PIM produrranno **sempre** e **solo** lo stesso cifrario, indipendentemente da quando, dove o su quale dispositivo viene eseguito.

Questo rende BastetCipher utilizzabile come generatore di password **senza necessità di archiviare le password generate**: è sufficiente ricordare la frase e il PIM per rigenerare il cifrario in qualsiasi momento.

### Esempio di output

```
.,aB#7xK2mQ!dF9nR+vC4wL8pT1sJ6uH_eI3oG5yM0zA~bE?cN*,
.,(continua per ~150-160 caratteri totali),.
```

---

## 💻 Requisiti

| Requisito | Minimo | Consigliato |
|---|---|---|
| **Browser** | Chrome 60+, Firefox 57+, Safari 11+, Edge 18+ | Chrome/Firefox versione corrente |
| **Web Crypto API** | Obbligatoria (nativa in tutti i browser moderni) | — |
| **CompressionStream API** | Necessaria per il Vault (Chrome 80+, Firefox 113+, Safari 16.4+) | Chrome/Firefox versione corrente |
| **JavaScript** | Abilitato | — |
| **Connessione Internet** | Non necessaria | Non necessaria |
| **Sistema operativo** | Qualsiasi | Qualsiasi |
| **Installazione** | Nessuna | Nessuna |

> ⚠️ **Nota:** La Web Crypto API richiede un **contesto sicuro** (HTTPS o `localhost`). L'apertura diretta del file tramite protocollo `file://` funziona in tutti i browser desktop principali, ma potrebbe non funzionare in alcuni ambienti mobili limitati.

---

## 📦 Installazione e deploy

### Utilizzo locale (raccomandato)

```bash
git clone https://github.com/tuo-username/BastetCipher.git
cd BastetCipher
# Apri direttamente il file HTML nel browser
```

### Deploy su GitHub Pages

```bash
# Il file index può essere rinominato per GitHub Pages
cp BastetCipher_ITA.html index.html
git add index.html
git commit -m "deploy: BastetCipher su GitHub Pages"
git push origin main
# Attiva GitHub Pages nelle impostazioni del repository (branch: main, root: /)
```

### Deploy su qualsiasi hosting statico

BastetCipher è un singolo file HTML statico: funziona su qualsiasi hosting che serva file statici (Netlify, Vercel, Cloudflare Pages, AWS S3, ecc.) senza alcuna configurazione server.

> **Nota:** il Vault e il Caveau Virtuale funzionano correttamente anche in locale via `file://` su browser desktop moderni. Per ambienti mobile o browser con policy restrittive su `file://`, si raccomanda di servire il file via HTTP locale (es. `python3 -m http.server`).

---

## 📊 Proprietà del cifrario generato

| Proprietà | Valore |
|---|---|
| **Lunghezza tipica** | 150–160 caratteri |
| **Set di caratteri** | Esadecimale rimappato + speciali (`!@#$%^&*_-+=~?`) + maiuscole/minuscole + marcatori `.,` |
| **Caratteri speciali** | 8–15 (posizioni deterministiche) |
| **Capitalizzazione** | 50% maiuscolo / 50% minuscolo (bilanciato deterministicamente) |
| **Marcatori** | Prefisso `.,` e suffisso `,.` |
| **Entropia stimata** | > 200 bit (input di media complessità) |
| **Riproducibilità** | 100% deterministica |
| **Algoritmo core** | PBKDF2-HMAC-SHA512 |
| **Iterazioni** | 50.000 – 600.000+ (dipendenti dal PIM) |

## 📊 Proprietà degli archivi .bca

| Proprietà | Valore |
|---|---|
| **Cifratura** | AES-256-GCM (strato 1) + AES-256-CBC (strato 2) |
| **Derivazione chiavi** | PBKDF2-HMAC-SHA512, 200.000 iterazioni |
| **Salt** | 256 bit casuali (crypto.getRandomValues) — unico per archivio |
| **IV GCM** | 96 bit casuali — unico per archivio |
| **IV CBC** | 128 bit casuali — unico per archivio |
| **Compressione** | deflate-raw (CompressionStream nativa) |
| **Integrità per file** | CRC-32 verificato alla decompressione |
| **File massimi per archivio** | 1024 |
| **Autenticazione** | GCM tag 128 bit — rileva manomissioni e password errate |
| **Formato output estrazione** | .zip standard (PKZip con central directory) |
| **Scritture su disco** | Zero durante creazione, estrazione e navigazione |

---

## ⚠️ Avvertenze e limitazioni

1. **Responsabilità dell'utente:** BastetCipher non memorizza né gestisce le tue credenziali. È responsabilità esclusiva dell'utente conservare in modo sicuro la frase segreta, il PIM e le password degli archivi.

2. **Non è un password manager:** BastetCipher è un generatore deterministico, non un vault con metadati. Non gestisce rotazione delle password, policy di scadenza o associazioni sito/credenziale.

3. **Sicurezza della frase segreta:** La robustezza del cifrario è proporzionale alla complessità della frase segreta. Frasi corte o prevedibili riducono significativamente la sicurezza.

4. **RAM e sistema operativo:** Come descritto nella sezione Best Practice, BastetCipher non può controllare la swap del sistema operativo o l'ibernazione. L'uso del Caveau Virtuale con dati molto sensibili richiede la disabilitazione di queste funzionalità a livello di sistema.

5. **Contesto di esecuzione:** Sebbene il codice sia open source e ispezionabile, si raccomanda di eseguire BastetCipher su dispositivi fidati e privi di malware. Un keylogger attivo sul sistema può catturare la frase segreta o la password dell'archivio prima che raggiungano l'applicazione.

6. **Revisione crittografica:** La trasformazione proprietaria al Passo 4 non è stata sottoposta a revisione crittografica formale. È opportuno considerarla un layer di oscuramento deterministico aggiuntivo, non un sostituto delle primitive crittografiche standard utilizzate nei passi 2 e 6. La sicurezza degli archivi .bca si basa esclusivamente su primitive standard (AES-256-GCM, AES-256-CBC, PBKDF2-HMAC-SHA512) ampiamente analizzate.

7. **Compatibilità `file://`:** In alcuni ambienti mobili e browser con configurazioni restrittive, la Web Crypto API e la CompressionStream API potrebbero non essere disponibili tramite protocollo `file://`. In tal caso, servire il file tramite un server HTTP locale o HTTPS.

8. **SSD e cancellazione sicura:** Su unità a stato solido, la cancellazione per sovrascrittura non è affidabile a causa del wear leveling. La cifratura del disco fin dall'inizio è la protezione più efficace su SSD.

---

## 📜 Licenza

```
MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

<div align="center">

**𓃠 𓂀 𓆣 𓇯 𓋹 𓊹 𓁹 𓃠 𓂀 𓆣**

*BastetCipher · Camera di Crittografia Sacra · PBKDF2-HMAC-SHA512*

*Tutti i calcoli vengono eseguiti localmente · Nessun dato trasmesso*

</div>
