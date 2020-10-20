# challenge1
Machine learning by neural network


# Il problema

il problema consiste nel predire un tag, tra quattro valori possibili (*00*,*10*,*01*,*11*),  partendo  da un vettore di dati di input (S) composto da alcune decine di elementi. 

La struttura dati del vettore S è fissa e ogni elemento del vettore rappresenta una feature esprimibile in alternativa in uno dei seguenti due modi:

- un numero compreso tra 0 e 100
- una stringa di lunghezza compresa tra  arbitraria tra 0 e 255 caratteri.

Es:

```
feature 1: string
feature 2: string
feature 3: number
feature 4: number
...
feature n: number
```

La distanza tra due vettori può essere calcolata sapendo che:

- la distanza tra due elementi numerici è la differenza in valore assoluto.
- la distanza tra due stringhe è 0 se le due stringhe sono identiche o 100 se sono diverse.

Si suppone esista un dataset di training composto da circa 100000 vettori S già taggati.

Esiste un vettore di pesi assegnati ai singoli elementi di S di cui l’algoritmo dovrebbe tenere conto.

Si conosce apriori la la distribuzione statistica attesa dei tag:

- 00 presente con una frequenza circa del 94%
- 11 presente con una frequenza circa del 2%
- 01 presente con una frequenza circa del 1%
- 10 presente con una frequenza circa del 3%

## Risultato atteso

si richiede la disponibilità di due comandi:

- **c1model** per la costruzione di un modello di M.L
- **c1predictor** in grado di utilizzare il modello prodotto al passo precedente per prevedere il valore del tag associato ad un vettore di featute S

### c1model

un comando che dato un set di dati di training  in input in formato csv, costruisce su file un modello di machine learning.

Il field-expert avrà la possibilità di guidare l’algoritmo della ricerca dei pesi ottimali, definendo opzionalmente  il range minimo-massimo dello spazio di ricerca. 

Usage:

```
c1model --sample=<sample_file> --output=<modelfile> [--weights=<weights_file>] [eventuali hyper parametri]
```

Esempio <sample_file> format:

```
feature 1,feature 2,feature 3,...,feature n,tag
"ABCD","DAEBAA",0,100,...,0, "10"
"ABCD",,90,0,...,100, “00”
...
```

Esempio <weights_file> format:

```
MIN-values, MAX-values
feature1, <min(feature1)>, <max(feature1)>
feature2,min(feature2), max(feature2)
…
featuren,min(featureN), max(featureN)
```


Il field-expert, grazie a questo file, ha un doppio controllo:
definizione dei range dello spazio di ricerca per il peso di ciascuna feature
definizione del peso che una feature deve avere: facendo coincidere minimo e massimo peso (per una data feature) allo stesso valore, l’algoritmo di ottimizzazione non lo modificherà.

qualora il parametro --weights  non fosse fornito come input, l’algoritmo cercherà i pesi inizializzando lo spazio di ricerca con un’euristica invece che inizializzando la ricerca con tale conoscenza a-priori.

c1model  ritorna 0 in caso di successo e un valore diverso da 0 in caso di errore. Eventuali messaggi di errori sono visualizzati su standard error


### c1predictor

un comando che dato in input un  modello e uno stream di  vettori, ritorni per ciascun vettore di input la predizione del del tag e (possibilmente) uno stimatore della qualità della predizione (i.e., la probabilità a priori che il la predizione sia corretta).

usage:

```
c1predictor --model=<modello> [--input_file=<path file di input>] [--output=<path file di output>] [eventuali hyper parametri] 
```

Se i parametri --input_file e/o --output non sono specificati si assume rispettivamente lo standard input e lo standard output.

Il comando ritorna 0 in caso di successo e un valore diverso da 0 in caso di errore. Eventuali messaggi di errori sono visualizzati su standard error

Il formato del file di input è identico a quello utilizzato per i sample passati a c1model  senza l'ultimo campo:

```
feature 1,feature 2,feature 3,...,feature n
"ABCD","DAEBAA",0,100,...,0
"ABCD",,90,0,...,100
...
```

Il formato del file di input è identico a quello utilizzato per i sample passati a c1model con un campo aggiuntivo che è la stima di confidenza della previsione sul tag:

```
feature 1,feature 2,feature 3,...,feature n,tag,confidence
"ABCD","DAEBAA",0,100,...,0, "10", 0.91
"ABCD",,90,0,...,100, “00”, 0.99
...
```


## Riferimenti a tool già esistenti con cui fare benchmark

- AMAZON AWS Comprehend

## Vincoli e gradi di liberà

- non sono richiesti particolari requisiti prestazionali sia per la costruzione del modello sia per il calcolo delle predizioni
- il linguaggio di programmazione è liberò purchè i comandi siano disponibili ( o facilmente resi disponibili) tramite container docker

