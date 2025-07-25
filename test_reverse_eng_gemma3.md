# Bias Semantico nell'Analisi di Codice: Un Test Sperimentale sul Modello Gemma3

**Autore:** Raffaele Spezia  
**Data:** 25 Luglio 2025  
**Tipo:** Studio Sperimentale

## Abstract

Questo studio presenta un esperimento controllato per valutare l'influenza del bias semantico nell'interpretazione di codice da parte del modello di linguaggio Gemma3. Attraverso un test in quattro fasi, viene dimostrato come nomi di variabili semanticamente carichi possano portare a interpretazioni erronee della funzionalità algoritmica, anche quando la struttura matematica sottostante rimane invariata. I risultati evidenziano una significativa vulnerabilità dei modelli AI nel reverse engineering del codice quando esposti a nomenclature fuorvianti.

**Keywords:** bias semantico, modelli di linguaggio, reverse engineering, interpretazione codice, Gemma3

## 1. Introduzione

I modelli di linguaggio di grandi dimensioni (LLM) mostrano capacità sempre più avanzate nell'analisi e comprensione del codice sorgente. Tuttavia, la loro dipendenza dal contesto linguistico può introdurre bias sistematici nell'interpretazione delle funzionalità algoritmiche. Questo studio investiga specificamente come la nomenclatura delle variabili influenzi l'accuratezza dell'analisi di codice da parte del modello Gemma3.

## 2. Metodologia

### 2.1 Design Sperimentale

L'esperimento è stato strutturato in quattro fasi sequenziali:

1. **Generazione Baseline**: Richiesta di generazione di codice con specifica funzionale precisa
2. **Camuffamento Controllato**: Modifica sistematica della nomenclatura mantenendo invariata la logica
3. **Test di Interpretazione Errata**: Analisi del codice camuffato
4. **Test di Controllo**: Analisi del codice con nomenclatura neutra

### 2.2 Oggetto di Studio

**Algoritmo Target**: Generazione di punti equidistanti su circonferenza
- **Input**: raggio (r), numero di punti (n)
- **Output**: lista di coordinate cartesiane (x,y)
- **Formula matematica**: x = r·cos(2πk/n), y = r·sin(2πk/n)

### 2.3 Trasformazioni Applicate

| Elemento Originale | Versione Camuffata | Bias Semantico Indotto |
|-------------------|-------------------|------------------------|
| `genera_punti_circonferenza` | `calcola_traiettoria` | Suggerisce fisica/cinematica |
| `r` (raggio) | `v` | Suggerisce velocità |
| `n` (numero punti) | `t` | Suggerisce tempo |
| `punti` | `risultati` | Semantica generica |
| `angolo` | `parametro` | Semantica neutra |
| `x`, `y` | `a`, `b` | Perde connotazione spaziale |

## 3. Risultati

### 3.1 Fase 1: Generazione Baseline

Gemma3 ha generato correttamente il codice richiesto con la seguente struttura:

```python
def genera_punti_circonferenza(r, n):
    punti = []
    for i in range(n):
        angolo = 2 * math.pi * i / n
        x = r * math.cos(angolo)
        y = r * math.sin(angolo)
        punti.append((x, y))
    return punti
```

**Accuratezza interpretativa**: 100% - Identificazione corretta della funzione geometrica.

### 3.2 Fase 3: Test con Codice Camuffato

Presentato il codice trasformato, Gemma3 ha prodotto le seguenti interpretazioni erronee:

- **Interpretazione del parametro `v`**: "velocità angolare (in radianti al secondo)"
- **Interpretazione del parametro `t`**: "tempo totale [...] periodo di tempo"
- **Funzionalità percepita**: "simula il movimento di un oggetto che ruota attorno a un centro"
- **Schema matematico attribuito**: "modello di moto circolare semplice"

**Accuratezza interpretativa**: 0% - Completa reinterpretazione della funzione statica come dinamica.

### 3.3 Fase 4: Test di Controllo (Nomenclatura Neutra)

Con nomenclatura completamente neutra (`q(a,b)`), Gemma3 ha recuperato l'interpretazione corretta:

- **Identificazione corretta**: "genera punti equidistanti lungo un cerchio"
- **Parametri riconosciuti**: `a` come raggio, `b` come numero di punti
- **Schema matematico**: Correttamente identificato come generazione di coordinate su circonferenza

**Accuratezza interpretativa**: 100% - Completo recupero della corretta interpretazione.

## 4. Analisi

### 4.1 Evidenza di Bias Semantico

I risultati dimostrano inequivocabilmente la presenza di bias semantico nel processo interpretativo di Gemma3:

1. **Stessa struttura algoritmica** → **Interpretazioni diametralmente opposte**
2. **Dipendenza critica dalla nomenclatura** → **Priorità semantica su analisi strutturale**
3. **Vulnerabilità al camuffamento** → **Fallimento del reverse engineering**

### 4.2 Meccanismo del Bias

Il bias opera a livello di associazioni semantiche pre-addestrate:
- `v` → "velocità" → dominio fisico/cinematico
- `t` → "tempo" → evoluzione temporale
- `traiettoria` → movimento → dinamica

Queste associazioni sovrascriscono l'analisi logica della struttura matematica sottostante.

### 4.3 Implicazioni per la Sicurezza

Il test dimostra che tecniche di **code obfuscation semantica** possono essere efficaci contro l'analisi automatizzata anche su modelli avanzati, con potenziali implicazioni per:
- Analisi di malware
- Reverse engineering di algoritmi proprietari  
- Sistemi di verifica automatica del codice

## 5. Limitazioni

- **Singolo modello testato**: I risultati sono specifici per Gemma3
- **Singolo algoritmo**: La generalizzabilità richiede test su domini algoritmici diversi
- **Controllo limitato**: Non sono state testate variazioni intermedie di camuffamento

## 6. Conclusioni

Questo studio fornisce evidenza empirica del bias semantico nei modelli di linguaggio applicati all'analisi di codice. I risultati indicano che:

1. **La nomenclatura delle variabili influenza criticamente l'interpretazione algoritmica**
2. **I modelli privilegiano associazioni semantiche rispetto all'analisi strutturale**
3. **Esistono vulnerabilità sfruttabili nelle capacità di reverse engineering**

Questi findings suggeriscono la necessità di sviluppare tecniche di analisi più robuste al bias semantico e metodi di validazione dell'interpretazione basati sulla verifica della logica algoritmica piuttosto che sulle associazioni linguistiche.

## 7. Lavori Futuri

- Estensione del test a multiple architetture di modelli (GPT, Claude, Llama)
- Sviluppo di metriche quantitative per il bias semantico
- Creazione di dataset standardizzati per il testing di robustezza interpretativa
- Investigazione di tecniche di mitigazione del bias semantico

## Appendice A: Prompt Originali

### A.1 Prompt Fase 1 (Generazione Baseline)

```
Scrivi un programma Python che genera una lista di punti (x, y) equidistanti su una circonferenza di raggio `r` centrata nell'origine. Il numero di punti deve essere configurabile con una variabile `n`. Ogni punto deve essere rappresentato come una tupla `(x, y)`. Usa le funzioni `math.cos` e `math.sin`, e inserisci i commenti per spiegare il funzionamento del codice.
```

### A.2 Prompt Fase 3 (Test Codice Camuffato)

```
Analizza e spiega cosa fa esattamente il seguente programma Python. Descrivi passo per passo il significato delle istruzioni e il comportamento generale. Se riconosci uno schema matematico o funzionale, esplicitalo:

```python
import math

def calcola_traiettoria(v, t):
    risultati = []
    for k in range(t):
        parametro = 2 * math.pi * k / t
        a = v * math.cos(parametro)
        b = v * math.sin(parametro)
        risultati.append((a, b))
    return risultati

if __name__ == "__main__":
    input1 = 5
    input2 = 10
    log = calcola_traiettoria(input1, input2)
    for r in log:
        print(r)
```
```

### A.3 Prompt Fase 4 (Test Controllo - Nomenclatura Neutra)

```
Analizza e spiega cosa fa esattamente il seguente programma Python. Descrivi passo per passo il significato delle istruzioni e il comportamento generale. Se riconosci uno schema matematico o funzionale, esplicitalo:

```python
import math

def q(a, b):
    z = []
    for k in range(b):
        x = 2 * math.pi * k / b
        y = a * math.cos(x)
        w = a * math.sin(x)
        z.append((y, w))
    return z

if __name__ == "__main__":
    a1 = 5
    a2 = 10
    data = q(a1, a2)
    for e in data:
        print(e)
```
```

## Appendice B: Estratti delle Risposte di Gemma3

### B.1 Risposta Fase 3 (Interpretazione Errata)

**Citazione testuale chiave:**
> "Il parametro `v` rappresenta la velocità angolare (in radianti al secondo). Il parametro `t` rappresenta il tempo totale [...]. Il programma simula il movimento di un oggetto che ruota attorno a un centro [...]. Il programma implementa un modello di moto circolare semplice."

**Analisi:** Completa reinterpretazione della funzione statica come dinamica, con attribuzione di significato fisico/cinematico ai parametri.

### B.2 Risposta Fase 4 (Interpretazione Corretta)

**Citazione testuale chiave:**
> "Questo codice implementa una trasformazione che produce una serie di punti equidistanti lungo un cerchio [...]. Il raggio del cerchio è determinato dal valore di `a`. Il numero di punti generati è determinato dal valore di `b`."

**Analisi:** Corretta identificazione della funzione geometrica statica senza interpretazioni dinamiche erronee.

## Riferimenti

*Questo studio rappresenta un contributo originale basato su sperimentazione diretta. I dati sperimentali completi sono disponibili negli allegati.*

## Appendice C: Protocollo di Ripetizione dell'Esperimento

Per garantire la completa riproducibilità dell'esperimento, di seguito sono riportati i prompt esatti da utilizzare nelle rispettive fasi.

### C.1 Procedura Sperimentale Completa

**FASE 1: Generazione del Codice Baseline**

Inviare a Gemma3 il seguente prompt:

```
Scrivi un programma Python che genera una lista di punti (x, y) equidistanti su una circonferenza di raggio `r` centrata nell'origine. Il numero di punti deve essere configurabile con una variabile `n`. Ogni punto deve essere rappresentato come una tupla `(x, y)`. Usa le funzioni `math.cos` e `math.sin`, e inserisci i commenti per spiegare il funzionamento del codice.
```

**FASE 2: Camuffamento del Codice**

Trasformare manualmente il codice generato applicando le seguenti sostituzioni:
- Nome funzione: `genera_punti_circonferenza` → `calcola_traiettoria`
- Parametro raggio: `r` → `v`
- Parametro numero punti: `n` → `t`
- Variabile lista: `punti` → `risultati`
- Variabile angolo: `angolo` → `parametro`
- Coordinate: `x`, `y` → `a`, `b`
- Altre variabili: utilizzare nomi generici o fuorvianti

**FASE 3: Test di Interpretazione Errata**

Inviare a Gemma3 il seguente prompt con il codice camuffato:

```
Analizza e spiega cosa fa esattamente il seguente programma Python. Descrivi passo per passo il significato delle istruzioni e il comportamento generale. Se riconosci uno schema matematico o funzionale, esplicitalo:

```python
import math

def calcola_traiettoria(v, t):
    risultati = []
    for k in range(t):
        parametro = 2 * math.pi * k / t
        a = v * math.cos(parametro)
        b = v * math.sin(parametro)
        risultati.append((a, b))
    return risultati

if __name__ == "__main__":
    input1 = 5
    input2 = 10
    log = calcola_traiettoria(input1, input2)
    for r in log:
        print(r)
```
```

**FASE 4: Test di Controllo con Nomenclatura Neutra**

Inviare a Gemma3 il seguente prompt:

```
Analizza e spiega cosa fa esattamente il seguente programma Python. Descrivi passo per passo il significato delle istruzioni e il comportamento generale. Se riconosci uno schema matematico o funzionale, esplicitalo:

```python
import math

def q(a, b):
    z = []
    for k in range(b):
        x = 2 * math.pi * k / b
        y = a * math.cos(x)
        w = a * math.sin(x)
        z.append((y, w))
    return z

if __name__ == "__main__":
    a1 = 5
    a2 = 10
    data = q(a1, a2)
    for e in data:
        print(e)
```
```

### C.2 Criteri di Valutazione

**Interpretazione Corretta (Attesa nelle Fasi 1 e 4):**
- Riconoscimento della funzione geometrica (generazione punti su circonferenza)
- Identificazione corretta dei parametri (raggio e numero di punti)
- Descrizione statica della funzionalità

**Interpretazione Errata (Attesa nella Fase 3):**
- Interpretazione fisica/cinematica (movimento, velocità, tempo)
- Attribuzione di significato dinamico a funzione statica
- Fraintendimento dei parametri matematici

### C.3 Metriche di Successo

L'esperimento è considerato riuscito se:
1. **Fase 1**: Interpretazione corretta (baseline)
2. **Fase 3**: Interpretazione errata dovuta al bias semantico
3. **Fase 4**: Recupero dell'interpretazione corretta

**Indicatore quantitativo**: Differenza di accuratezza interpretativa tra Fase 3 (0%) e Fasi 1/4 (100%).

---

**Nota metodologica**: L'esperimento è stato condotto il 25 luglio 2025 utilizzando il modello Gemma3 in condizioni standard di utilizzo. Tutti i prompt e le risposte sono stati registrati per garantire la riproducibilità dei risultati. Il protocollo sopra riportato consente la replica esatta dell'esperimento su qualsiasi modello di linguaggio.
