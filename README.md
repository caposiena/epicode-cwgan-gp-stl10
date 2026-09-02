# Conditional WGAN-GP su STL-10

## Descrizione

Progetto realizzato per il modulo di Generative AI del Master Epicode.

L'obiettivo del progetto è implementare una Conditional Wasserstein GAN con Gradient Penalty (cWGAN-GP) in grado di generare immagini appartenenti alle 10 classi del dataset STL-10.

A differenza di una GAN tradizionale, il modello utilizza anche l'etichetta della classe desiderata, permettendo di controllare la categoria dell'immagine generata.

## Dataset

È stato utilizzato il dataset STL-10 nella porzione di training.

Le immagini sono state:

- ridimensionate a 64x64 pixel;
- convertite in tensori;
- normalizzate nell'intervallo [-1, 1].

Le 10 classi utilizzate sono: aereo, uccello, auto, gatto, cervo, cane, cavallo, scimmia, nave e camion.

## Generatore

Il Generatore riceve in input un vettore casuale di rumore e l'etichetta della classe da generare.

La classe viene trasformata attraverso un Embedding Layer e combinata con il vettore di rumore. La rete utilizza livelli Dense e blocchi ConvTranspose2d con Batch Normalization e ReLU. Il livello finale utilizza una funzione Tanh per produrre immagini normalizzate nell'intervallo [-1, 1].

## Critico

Il Critico riceve una coppia composta da immagine e classe associata.

L'informazione della classe viene trasformata e concatenata all'immagine. La rete utilizza convoluzioni con stride e Layer Normalization. A differenza di un discriminatore GAN tradizionale, non viene utilizzata una funzione Sigmoid finale: l'output è un singolo valore scalare non vincolato.

## Gradient Penalty

Per rendere più stabile l'addestramento è stato utilizzato il Gradient Penalty previsto dalla WGAN-GP, calcolato su immagini interpolate tra campioni reali e immagini generate.

Il coefficiente utilizzato è `lambda_gp = 10`.

## Addestramento

Il modello è stato addestrato utilizzando un custom training loop. Per ogni aggiornamento del Generatore, il Critico viene aggiornato 5 volte.

Principali iperparametri utilizzati:

- learning rate: 0.0001
- dimensione embedding: 50
- Gradient Penalty: 10
- aggiornamenti Critico: 5
- numero di epoche: 50

## Risultati

Durante l'addestramento sono state generate gallerie delle 10 classi per osservare l'evoluzione delle immagini sintetiche.

All'inizio le immagini sono costituite principalmente da rumore. Con il procedere dell'addestramento iniziano a comparire strutture, colori e caratteristiche differenti tra le varie classi.

Dopo 50 epoche le immagini non risultano ancora perfettamente definite, ma mostrano un'evoluzione evidente rispetto alla fase iniziale.

## Loss del Critico

È stato analizzato l'andamento della loss del Critico durante le 50 epoche. La loss mostra una variazione iniziale molto rapida seguita da una progressiva stabilizzazione nelle ultime epoche.

## Considerazioni finali

Il progetto ha permesso di implementare una GAN condizionata utilizzando il metodo Wasserstein con Gradient Penalty.

La parte più delicata è stata mantenere stabile il training e bilanciare l'apprendimento del Generatore e del Critico.

Dopo 50 epoche il modello mostra un miglioramento rispetto alle immagini generate inizialmente, anche se un numero maggiore di epoche potrebbe permettere di ottenere immagini più definite.

L'obiettivo principale del progetto, cioè implementare una cWGAN-GP condizionata sulle classi di STL-10, è stato raggiunto.
