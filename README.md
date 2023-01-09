# Analysis-of-series-tv
“Predizione del genere per ciascuna serie tv presa in esame”
Features:
- Series Title: nome della serie tv
- Year Released: anno in cui è stata messa in onda la serie tv
- Content Rating: classificazione dei contenuti valuta l'idoneità di trasmissioni TV, film, fumetti o videogiochi per il proprio pubblico
- IMDB Rating: rating delle serie rilasciato da IMDB1 (1-10)
- R Rating: rating delle serie rilasciato da Rotten Tomato2 (0-100)
- Description: trama della serie tv
- No of Seasons: numero stagioni della serie tv
- Streaming Platform: piattaforma streaming in cui è presente la serie tv
Target:
- Genre: genere della serie tv

*Esplorazione del dataset*
Per prima cosa sono andata a visualizzare il dataset e i valori presenti nelle varie colonne.
A primo impatto non sembrano esserci “N/A” o valori inseriti erroneamente quindi decido di visualizzare il valore massimo e il valore minimo rispettivamente per le feature “IMDB Rating” e “R Rating”.
Si può notare come il valore massimo della feature “IMDB Rating” è un “N/A” mentre il valore minimo per la feature “R Rating” è “-1” perciò mi son domandata se fosse una semplice osservazione inserita erroneamente o meno.
Andando a visualizzare in senso ascendente la feature “R Rating” si pone in luce come non sia una semplice osservazione inserita erroneamente bensì siano molteplici i casi in cui è presente il valore “-1” e non solo in questa colonna!
Sono arrivata alla conclusione che il valore “-1” sia stato utilizzato per evidenziare l’assenza dell’informazione per quella determinata osservazione perciò decido di rimuovere i valori trasformandoli in “None” ed eliminarli.
Decido di visualizzare il numero di valori “None” presenti in ciascuna feature e noto come in “Content Rating” sono presenti più del 40% di valori “None”, perciò, decido di eliminare direttamente la colonna poiché non in grado di apportarmi una sufficiente informazione.
Il prossimo passo è quello di rinominare le colonne, eliminare le stringhe “Season” o “Seasons” presenti nella feature “NOSeasons” in modo da poter tenere esclusivamente il numero delle stagioni come tipo integer.
1 IMDB, gli utenti registrati su IMDB possono esprimere un voto (da 1 a 10) su ogni titolo pubblicato nel database. I voti individuali vengono quindi aggregati e riepilogati in un'unica valutazione IMDB visibile nella pagina principale del titolo
2 Rotten Tomatoes è un sito web che si occupa di raccogliere recensioni, informazioni e notizie sul mondo del cinema e delle serie TV.
Ad una seconda osservazione del dataset ci si rende conto di come sia presente nella feature “Genre” l’anno in cui è stata rilasciata la serie tv (es. Serie tv: “The Office”) perciò decido di rimuovere valori numerici dalla feature e in “Streaming Platform”.
Visualizzando i grafici di dispersione per le variabili “Year”, “NOSeasons”, “IMDB Rating” e “R Rating” decido di tenere le serie tv che hanno meno di 25 stagioni e che sono state emesse successivamente al 1950.
Successivamente sono passata a fare la tf-idf della feature “Description”.
Il passo successivo è stato quello di eliminare le variabili non necessarie e indicizzare la variabile “StreamingPlatform” per poter successivamente creare un vettore delle features assemblate.
Per poter predire la variabile target, quest’ultima è stata convertita prima in una singola colonna per ciascuna label e, successivamente, in un vettore unico di 0,1 in base alla presenza delle labels nel genere identificato per ciascuna serie.
Per quanto riguarda il modello utilizzato, la logica è stata quella di utilizzare il modulo LogisticRegressionWithLBFGS di pyspark.mllib.classification che addestra un modello di regressione logistica dato un RDD di coppie (label, features).
Questo modello utilizza di default la Regolarizzazione L2.
Successivamente è stato calcolato il tasso di errata classificazione per ciascuna label in base ai risultati ottenuti dalla predizione rispetto al risultato del test. Si può notare come tendenzialmente l’accuracy ha valori al di sopra dell’80.
