**Titolo:** Test TIGM-01 e TIGM-02: Analisi della Comprensione nei Modelli LLM attraverso la Manipolazione Semantica

**Autore:** Raffaele Spezia

---

**Obiettivo del documento**

Verificare il grado di comprensione effettiva di un modello LLM (Gemma3) mediante tre test successivi:

1. Generazione autonoma di un programma geometrico.
2. Presentazione dello stesso programma con nomi fuorvianti.
3. Presentazione del medesimo programma con nomi neutri.

Questo approccio permette di distinguere tra pattern matching semantico e riconoscimento strutturale reale.

---

### **1. Generazione del programma originale**

**Prompt dato a Gemma3:**

> Scrivi un programma Python che genera una lista di punti (x, y) equidistanti su una circonferenza di raggio `r` centrata nell'origine. Il numero di punti deve essere configurabile con una variabile `n`. Ogni punto deve essere rappresentato come una tupla `(x, y)`. Usa le funzioni `math.cos` e `math.sin`, e inserisci i commenti per spiegare il funzionamento del codice.

**Codice generato:**

```python
import math

def genera_punti_circonferenza(r, n):
  punti = []
  for i in range(n):
    angolo = 2 * math.pi * i / n
    x = r * math.cos(angolo)
    y = r * math.sin(angolo)
    punti.append((x, y))
  return punti

if __name__ == "__main__":
  raggio = 5
  numero_punti = 10
  punti_circonferenza = genera_punti_circonferenza(raggio, numero_punti)
  for punto in punti_circonferenza:
    print(punto)
```

**Commento del modello:** Gemma3 ha fornito una descrizione dettagliata del funzionamento del programma, riconoscendone la struttura geometrica: generazione di punti equidistanti su una circonferenza. Tutti i concetti sono stati spiegati correttamente.

---

### **2. Camuffamento semantico del programma (TIGM-01)**

**Codice camuffato (nomi fuorvianti):**

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

**Prompt per il test:**

> Analizza e spiega cosa fa esattamente il seguente programma Python. Descrivi passo per passo il significato delle istruzioni e il comportamento generale. Se riconosci uno schema matematico o funzionale, esplicitalo:

**Risposta del modello:** Gemma3 ha interpretato `v` come "velocità angolare", `t` come "tempo", `parametro` come "omega·t". Ha associato la funzione al moto circolare uniforme nel tempo. Ha costruito una narrazione coerente, ma **totalmente falsa** rispetto alla struttura reale del codice.

**Conclusione del test:** Gemma3 ha proiettato una spiegazione cinematica basata **esclusivamente sui nomi delle variabili**. Il comportamento dedotto era errato. Il modello non ha riconosciuto che `v` era il raggio, e `t` il numero di punti.

---

### **3. Ripresentazione con variabili neutre (TIGM-02)**

**Codice con nomi neutri:**

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

**Prompt per il test:**

> Analizza e spiega cosa fa esattamente questo programma Python. Descrivi il significato delle istruzioni e il comportamento generale. Se riconosci una struttura funzionale o geometrica, spiega in cosa consiste:

**Risposta del modello:** Gemma3 ha riconosciuto correttamente che il programma genera \*\*punti equidistanti su una circonferenza di raggio \*\*\`\`. Ha descritto correttamente l'uso di `cos` e `sin`, la distribuzione degli angoli, e il significato geometrico della lista `z`.

**Conclusione del test:** In assenza di etichette semantiche fuorvianti, il modello ha dedotto correttamente la funzione geometrica sottostante. Questo suggerisce che **l'errore nel test 2 non era dovuto all'ignoranza, ma a una dipendenza semantica dai nomi delle variabili**.

---

### **Conclusione generale**

I test TIGM-01 e TIGM-02 dimostrano che:

- I modelli LLM possono generare codice corretto.
- Possono essere sviati completamente da nomi semantici fuorvianti.
- In assenza di semantica, possono riconoscere correttamente la struttura.

Questa è una prova forte del fatto che molti LLM non comprendono realmente il codice: **lo associano a strutture preesistenti viste nei dati di addestramento**, basandosi pesantemente su pattern linguistici e non su deduzione logico-matematica.

Un test utile per valutare la "comprensione funzionale" reale nei modelli di linguaggio.

**Prossimi passi:** formalizzazione del protocollo TIGM in un benchmark replicabile tra modelli. Valutazione comparata su Phi-3, LLaMA, Mistral, GPT-4, ecc.

