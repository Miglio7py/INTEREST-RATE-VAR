# INTEREST-RATE-VAR
Modello VAR econometrico, per la predizione delle decisioni sui tassi di interesse della FED.
# Introduzione
Negli ultimi 6 trimestri, i tassi di interesse hanno subito un importante aumento per fronteggiare la forte inflazione, passando dallo 0,25% al 5,5%. Gli operatori si chiedono quando questo vertiginoso rialzo dei tassi possa finire, e quando addirittura la FED potrebbe decidere di tagliare i tassi di interesse adottando una politica dovish. In questa analisi verrà implementato un modello Vector Autoregressive Model (VAR) che consenta di descrivere le decisioni sui tassi di interesse, e prevedere le future decisioni (Q4-2023, Q1-2024, Q2-2024, Q3-2024). Per descrivere La variazione dei tassi di interesse, vengono utilizzati 10 variabili macroeconomiche quali: il totale della bilancia dei pagamenti per gli importi US (TBIMTOT), il totale delle esportazioni US (Export), l’indice dei prezzi al consumo anno per anno (CPI YoY), il tasso di disoccupazione totale nella forza lavoro aggiustato per la stagionalità (USURTOT Index), il prodotto interno lordo US trimestre per trimestre (GDP QoQ), l’indice dei prezzi degli immobili residenziali US (Corelogic case-shiller), il prezzo S&P 500 (S&P500). Le serie storiche sono di dati trimestrali, e vanno dal Q2-1993 al Q3-2023.
# Stazionarietà Serie Storiche (ADF)
L’analisi inizia con un Augmented Dickey-Fuller (ADF) Test volto a verificare la presenza di trend stocastici all’interno delle variabili selezionate. Tale test serve per verificare se la serie è stazionaria o no. 
I risultati del test mostrano come solo 3 serie storiche su 11 sono stazionarie.
Per rendere una serie stazionaria viene applicata una differenziazione prima. Questo permette di eliminare la tendenza e la ciclicità di una serie temporale. Applicare una differenziazione prima su una serie storica y significa sottrarre il valore al tempo t al valore al tempo t-1.
# Causalità di Granger
Per selezionare quali variabili tenere all’interno del modello, si effettua un Granger Causality test. Questo test ha come ipotesi nulla che la variabile X non causa la variabile Y. Quando la variabile x non aumenta la capacità predittiva della variabile y si dice che x non granger causa y. 
# Correlazione e Multicollinearità
Selezionate le variabili da tenere all’interno del modello, si costruisce la matrice delle correlazioni tra le variabili. 
Successivamente si è controllato il Variance Inflation Factor (VIF), un indice utilizzato per valutare la presenza di multicollinearità tra le variabili. Valori elevati di VIF indicano forte correlazione, e questo può portare distorsione all’interno del modello, facendo perdere significatività ad alcuni predittori. 
# Addestramento Modello
Prima di stimare il modello utilizzando tutte le osservazioni, si stima il modello sulle prime 116 osservazioni e si crea un dataframe di test contenente le ultime 4 osservazioni non considerate, in modo da poter confrontare le predizioni con dati osservati.
Come criterio di selezione dei lag p e q del modello VARMA(p,q), si utilizza il criterio d’informazione di Akaike (AIC). Vengono confrontati p e q da 0 a 3, e la combinazione con valore AIC più basso è la VARMA(2,0) ovvero un VAR(2). 
# Diagnostica del modello
Si osserva la distribuzione dei residui. Con un Jarque Bera Test si testa sotto ipotesi nulla che i residui siano distribuiti come una normale.
Successivamente si effettua l’Augmented Dickey-Fuller Test (ADF) sui residui, per vedere se essi sono stazionari.
Successivamente alla stazionarietà si valuta l’autocorrelazione dei residui. Per farlo viene utilizzato il test Ljung-Box. Tale test ha come ipotesi nulla che non vi siano autocorrelazioni tra le serie.
# Previsioni
Una volta effettuati i vari test sul modello, si prevedono i 4 periodi successivi al dataframe train.
Le previsioni sembrano seguire bene l’andamento  anche se non sono molto precise. Bisogna anche tenere in considerazione che una variazione dei tassi dell’1,25% in un trimestre è un dato abbastanza anomalo e per questo motivo il nostro modello potrebbe non stimarlo bene.
# Valutazione Bontà delle Previsioni
Come misura per valutare la bontà delle previsioni si utilizza il Mean Squared Error (MSE) e il Mean Absolute Percentage Error (MAPE). 
# Modello Finale
Dopo aver testato il modello train, si stima il modello finale considerando l’intero campione di osservazioni. Secondo il criterio di selezione AIC, anche in questo caso il modello ottimale è un VAR(2,0).
Una volta stimato il modello si effettuano gli stessi test diagnostici che sono stati effettuati precedentemente sul modello train.
# Conclusione
Grazie a tale modello abbiamo ottenuto le previsioni della differenza prima dei tassi di interesse della FED. Le previsioni sono tutte vicine allo zero, il che è coerente con la fine del periodo hawkish della Federal Reserve. Infatti, togliendo la differenziazione, si è visto come i tassi attesi nei prossimi 4 trimestri passino dal 5,5% odierno fino al 5,56% del Q3-2024.
