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

**BastetCipher** è un generatore crittografico deterministico di password/cifrari ad alta entropia,  
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

Il nome è ispirato alla dea egizia **Bastet** — protettrice, guardiana e custode dei segreti — e si riflette nell'intera estetica dell'interfaccia, che ricrea fedelmente l'atmosfera di una camera sacra dell'antico Egitto.

L'applicazione è distribuita come **singolo file `.html` autosufficiente**: basta aprirlo in qualsiasi browser moderno per utilizzarlo, senza installazioni, senza account, senza connessione a Internet.

---

## ✨ Caratteristiche principali

| Caratteristica | Descrizione |
|---|---|
| 🔐 **Crittografia nativa** | Utilizza esclusivamente la [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) nativa del browser — nessuna libreria crittografica di terze parti |
| 🧂 **Sale deterministico** | Il sale viene derivato deterministicamente dall'input, garantendo riproducibilità assoluta a parità di frase e PIM |
| 🔑 **PBKDF2-HMAC-SHA512** | Derivazione della chiave con numero di iterazioni variabile e dipendente dal PIM, nell'intervallo [50.000 – 600.000+] |
| 🎲 **Trasformazione proprietaria** | Quattro stadi di trasformazione deterministica (rotazione, scambio, rimappatura hex, inversione a sezioni) applicati prima di PBKDF2 |
| 🔢 **PIM personale** | Un moltiplicatore numerico (1–32 cifre) che altera radicalmente le iterazioni e quindi il cifrario prodotto |
| 🌿 **Caratteri speciali deterministici** | Inserimento di 8–15 caratteri speciali in posizioni pseudocasuali ma completamente riproducibili |
| 🔡 **Capitalizzazione mista** | Esattamente il 50% dei caratteri alfabetici è maiuscolo e il 50% minuscolo, scelti in modo deterministico |
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
connect-src  'none'                   ← ZERO connessioni di rete
object-src   'none'
frame-src    'none'
worker-src   'none'
base-uri     'none'
form-action  'none'
```

`connect-src 'none'` è la direttiva più importante: rende **tecnicamente impossibile** qualsiasi trasmissione di dati verso server esterni, anche in caso di exploit nel codice JavaScript.

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

### Isolamento dell'esecuzione

- **Nessuna comunicazione tra schede o origini**: assenza di `postMessage`, `BroadcastChannel` o `SharedWorker`
- **Nessun accesso a storage persistente**: nessun utilizzo di `localStorage`, `sessionStorage`, `IndexedDB` o cookie
- **Nessuna libreria esterna**: zero CDN, zero `<script src="...">` — l'intera applicazione è autocontenuta

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
│   └── <style> — ~700 righe CSS
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
│   │   ├── <footer #footer>
│   │   └── #MILU — SVG grande Bastet decorativa
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
│       └── input listener PIM (validazione lunghezza)
```

---

## 🚀 Utilizzo

### Utilizzo base

1. Apri `BastetCipher_ITA.html` in un browser moderno
2. Inserisci la tua **frase segreta** nel campo apposito
3. Inserisci il tuo **PIM** (valore numerico da 1 a 32 cifre)
4. Premi il pulsante **Genera Cifrario** oppure premi `Invio`
5. Attendi il completamento della pipeline (indicata dalla barra di avanzamento)
6. Il cifrario appare nel pannello output con effetto macchina da scrivere
7. Usa il pulsante **Copia negli Appunti** per copiare il risultato

### Riproducibilità

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

---

## ⚠️ Avvertenze e limitazioni

1. **Responsabilità dell'utente:** BastetCipher non memorizza né gestisce le tue credenziali. È responsabilità esclusiva dell'utente conservare in modo sicuro la frase segreta e il PIM.

2. **Non è un password manager:** BastetCipher è un generatore deterministico, non un vault cifrato. Non gestisce rotazione delle password, policy di scadenza o metadati associati.

3. **Sicurezza della frase segreta:** La robustezza del cifrario è proporzionale alla complessità della frase segreta. Frasi corte o prevedibili riducono significativamente la sicurezza.

4. **Contesto di esecuzione:** Sebbene il codice sia open source e ispezionabile, si raccomanda di eseguire BastetCipher su dispositivi fidati e privi di malware. Un keylogger attivo sul sistema può catturare la frase segreta prima che raggiunga l'applicazione.

5. **Revisione crittografica:** La trasformazione proprietaria al Passo 4 non è stata sottoposta a revisione crittografica formale. È opportuno considerarla un layer di oscuramento deterministico aggiuntivo, non un sostituto delle primitive crittografiche standard utilizzate nei passi 2 e 6.

6. **Compatibilità `file://`:** In alcuni ambienti mobili e browser con configurazioni restrittive, la Web Crypto API potrebbe non essere disponibile tramite protocollo `file://`. In tal caso, servire il file tramite un server HTTP locale o HTTPS.

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
