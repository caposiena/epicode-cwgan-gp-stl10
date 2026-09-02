# Conditional WGAN-GP su STL-10

Progetto realizzato per il modulo di Generative AI del Master Epicode.

L'obiettivo è realizzare una Conditional Wasserstein GAN con Gradient Penalty in grado di generare immagini appartenenti alle 10 classi del dataset STL-10.

## Preparazione dei dati

Ho utilizzato la parte train di STL-10. Le immagini vengono ridimensionate a 64x64 pixel, trasformate in tensori e normalizzate nell'intervallo [-1, 1].

Le classi sono: aereo, uccello, auto, gatto, cervo, cane, cavallo, scimmia, nave e camion.

## Modello

Il Generatore riceve un vettore di rumore e la classe dell'immagine da generare. La classe viene trasformata tramite un embedding e unita al vettore di rumore.

La rete utilizza un layer Dense seguito da ConvTranspose2d, BatchNormalization e ReLU. Nell'ultimo layer viene utilizzata Tanh.

Il Critico riceve invece sia l'immagine sia la classe. In questo caso ho utilizzato LayerNormalization e non è presente una Sigmoid finale, perché il Critico deve restituire un valore scalare non vincolato.

## Training

L'addestramento è stato realizzato con un ciclo manuale e Gradient Penalty calcolato su campioni interpolati.

Ho utilizzato questi parametri:

- learning rate: 0.0001
- embedding: 50
- coefficiente Gradient Penalty: 10
- 5 aggiornamenti del Critico per ogni aggiornamento del Generatore
- 50 epoche

Durante il training ho generato periodicamente una galleria delle 10 classi per controllare l'evoluzione delle immagini.

## Risultati

Nelle prime epoche le immagini sono principalmente rumore. Durante l'addestramento iniziano a comparire colori e strutture differenti tra le varie classi.

Alla cinquantesima epoca le immagini non sono ancora molto definite, ma il miglioramento rispetto all'inizio è visibile.

Ho inoltre riportato il grafico della loss del Critico, che dopo la variazione iniziale tende progressivamente a stabilizzarsi.

## Considerazioni

La parte più delicata è stata ottenere un training sufficientemente stabile e mantenere un equilibrio tra Generatore e Critico.

Con 50 epoche il risultato rimane abbastanza semplice, ma è possibile osservare l'evoluzione della generazione e il funzionamento della cWGAN-GP condizionata sulle classi di STL-10.
