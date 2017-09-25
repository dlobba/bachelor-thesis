# Discorso presentazione relazione finale

## Slide Attività svolte

PyGFA è una libreria che opera su file rappresentanti 
concetti biologici e descritti da apposite specifiche.

Si sono studiate le specifiche e i concetti
di DNA e di assemblaggio, si è implementata l'effettiva libreria
verificandone funzionalità e qualità mediante strumenti appositi
ed infine si sono effettuati dei benchmark per verificare
il comportamento del sistema in presenza di grandi quantità
di dati, confrontandolo con una libreria esistente che rappresenta
l'attuale stato dell'arte.

## Slide Cos'è il DNA
Il DNA è il materiale che racchiude le informazioni necessarie
per descrivere un organismo. E' composto da nucleotidi
cui è possibile riferirsi mediante la singola base azotata che contengono:
Adenina, Guanina, Timina, Citosina (A, C, G, T rispettivamente).
E' possibile quindi dire che il DNA è una stringa!

La stringa che codifica il DNA non è il risultato diretto del sequenziamento
di un genoma, ma la ricombinazione di sequenze molto piccole prodotte
da metodi di nuova generazione (NGS) che permettono di sequenziare
una parte di DNA a prezzi accessibili.

I metodi NGS producono un'alta mole di informazioni; sequenze
e collegamenti presenti fra esse, che descrivono sovrapposizioni
e spazi, oltre a tutta una serie di informazioni che è utile
salvare prodotta degli step intermedi al sequenziamento.

Per esempio è possibile avere sequenze che formano dei percorsi oppure
sequenze che dipartono in un punto
per poi ricongiungersi successivamente. Per questo motivo è necessario
un formato standard che descriva genericamente questi dati.

## Slide I file GFA

I file GFA nascono dalla necessità di creare un nuovo formato standard per
la rappresentazione dei grafi di assemblaggio di un genoma (o più contemporaneamente).

GFA comprende 2 specifiche, la prima specifica GFA1 è appositamente pensata per i grafi
di assemblaggio, permette infatti di descrivere le sequenze, le modalità in cui sono sovrapposte
in modo da formare un continuum oppure un contenimento. Non permette però
di descrivere informazioni più generiche!

Per questo vi è la seconda specifica, GFA2, meno restrittiva e più generica che permette
di descrivere le informazioni con il livello di dettaglio richiesto dall'utilizzatore (quindi
sia di descrivere le informazioni con meno dettagli, sia con più dettagli).

Questi file sono composti da linee aventi una formattazione predefinita; campi separati
da tabulazioni, ogni campo ha un significato e deve essere esplicitamente indicato.
Ogni linea descrive un concetto specifico del grafo di assemblaggio (una sequenza,
un insieme di sequenze, una sovrapposizione, un contenimento...).
E' possibile estendere la specifica introducendo nuove linee.
 
## Slide PyGFA

PyGFA è una libreria python per gestire file GFA. Python è uno dei linguaggi più
utilizzati in svariati ambiti, tra cui la bioinformatica. Per questo si è voluto
fornire una libreria, implementata in questo linguaggio, che fosse in grado di operare con
i file GFA.

PyGFA sfrutta la classe Multigrafo offerta dalla libreria NetworkX, una libreria
specifica nella rappresentazione di grafi e alberi, per contenere le informazioni
dei file gfa, attribuendo loro i ruoli di nodi e archi e permettendo così di
riuscire ad applicare algoritmi pensati ai grafi al contesto dell'assemblaggio del
genoma. 

## Slide Gestione delle Informazioni

Le informazioni provenienti dai file GFA subiscono 3 trasformazioni nel
passaggio da file a grafo:
1. nella prima, ciascuna linea viene rappresentata con una classe
2. nella seconda, ad ogni classe viene attribuito il ruolo che ricoprirà
	nel grafo; le linee segment saranno i nodi, le linee di path e di group
	saranno dei sottografi e le linee rimanenti saranno archi.
3. nella terza, le classi rappresentanti gli elementi del grafo vengono
	convertite in dizionari python, per una maggiore integrazione
	e facilità di utilizzo della struttura offerta da networkx.
	
Gli archi che rappresentano un continuum fra due sequenze vengono
marcati in modo da poter eseguire delle operazioni utili ai fini della
fase di asseblaggio del genoma.

In questo modo si ha una doppia visione del grafo gfa,
una in cui le informazioni costituiscono nodi e archi generici
di un grafo mentre l'altra (in cui vengono considerati solo
archi di dovetail) rappresentante i modi in cui due sequenze
possono susseguirsi.

## Slide Funzionalità

Grazie all'utilizzo di Networkx come libreria di supporto
è stato possibile implementare con grande facilità le operazioni
sul grafo generico, mentre le operazioni che operano sul grafo
ottenuto considerando solo gli archi di dovetail sono state implementate
partendo dal codice sorgente offerto dalla libreria ed integrandolo
con un iteratore personalizzato.

Tra le funzionalità offerte ci sono:
- il calcolo delle componenti connesse
- il calcolo di tutti i percorsi semplici compresi tra due nodi
- salvataggio del grafo in una delle due specifiche
- ricerca degli elementi utilizzando un
	comparatore definito dall'utilizzatore

## Slide Benchmark

Sono stati condotti dei test per verificare le performance di pygfa.
Nella prima serie di test si è analizzato il solo comportamento della libreria,
analizzando il modo in cui scalava con l'aumentare delle informazioni.

Nella seconda serie di test, si è confrontato il comportamento di pygfa rispetto
una libreria concorrente, gfapy, che rappresenta l'attuale stato dell'arte in questo
contesto.

I risultati della prima serie di test indicano che pygfa ha un andamento lineare
sia in termini di tempo di esecuzione che di spazio occupato in memoria.
Mentre, messa a confronto con la libreria concorrente, è possibile notare
come il tempo di esecuzione del calcolo delle componenti connesse
sia nettamente più veloce in pygfa rispetto a gfapy, mantenendo
un occupazione in memoria molto simile.

## Slide Metodo di sviluppo

Per lo sviluppo di PyGFA è stata adottata una metodologia extreme programming,
con tempi di analisi brevi che hanno permesso di concentrarsi subito sull'implementazione
delle funzionalità cruciali del sistema, per poi estendere le funzionalità nelle fasi successive.
In parallelo alla fase di sviluppo si è svolta
la fase di test per verificare il corretto funzionamento delle parti di sistema che venivano
implementate.

Queste attività si sono ripetute ciclicamente per tutto il periodo di sviluppo,
integrando al termine di ciascuna iterazione una fase di refactoring del codice.

## Slide Strumenti di aiuto nello sviluppo

Nello sviluppo è fatto uso di strumenti di supporto per
la verifica dei casi di test (Coverage.py) e per misurare la qualità globale
del codice del sistema (Pylint), raggiungendo la copertura del 96% del
codice.

Oltre a questi strumenti, si è utilizzato uno strumento per la generazione
automatica della documentazione (Sphinx) rendendola fruibile mediante
sito-web e mettendola in hosting su una piattaforma online specifica
(Read the Docs).


## Slide Conclusioni

PyGFA ha raggiunto una versione alpha stabile.
Rimane da fare una fase di refactoring del codice più accurata.
Tuttavia la libreria offre una piattaforma piuttosto solida che è possibile
estendere facilmente con nuove funzionalità.
