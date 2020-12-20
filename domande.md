# Domande slide Grafica

### Che cos'è la radianza?
Quantità di luce che attraversa una superficia unitaria e diretta lungo un
angolo solido unitario.

### Che cosa si intende per BRDF?
Funzione che lega la luce che colpisce un punto lungo una direzione A con la
luce che viene riflessa lungo una direzione B

### Cosa si intende per soluzione globale/locale dell'equazione del rendering?
La soluzione globale tiene conto di tutte le fonti luminose e produce un
rendering fotorealistico e accurato ma è computazionalmente più pesante. La
soluzione locale usa svariati trucchi e ottimizazioni per rendere il rendering
meno complesso per un computer ma produce un risultato poco realistico.

### Come si comportano i material opachi e lucidi?

La luce che colpisce i materiali opachi penetra sotto la superficie, rimbalza
(subsurface scattering) per poi riemergere. Si parla di luce diffusa. Il colore
della luce che riemerge dalla superficie è stato alterato dall'oggetto colpito.
Nei materiali lucidi la luce rimbalza dalla superficie senza penetrare. Si parla
di luce speculare. Il colore non viene alterato.

### Cosa si intende per "modello di illuminazione"?

Funzione che permette di modellare matematicamente il comportamento della luce e
la sua interazione con le superfici. Descrive la relazione che c'è tra la luce
che colpisce un oggetto e quella che viene riflessa. `f: Sorgente x Materiale x
Normale -> Colore`

### Quale termine del modello di Phong simula l'effetto di illuminazione globale dovuto alle inter-riflessioni?

E' un termine (chiamato termine ambientale) aggiunto all'espressione del modello
di Phong. E' funzione di I_a (indice dell'intensità dell'illuminazione della
scena), K_a (indice della riflessività del materiale che si sta renderizzando).

###  Cosa si intende per Physically Based Rendering

Un modello di illuminazione più avanzato rispetto a Phong o Gourad che tenta di
approssimare meglio il comportamento della luce in natura. Descrive i materiali
secondo le loro proprietà fisiche.

###  Quali sono le tecniche che si usano per accelerare il calcolo delle intersezioni raggi/oggetti?

+ Volume di contenimento: i solidi vengono racchiusi in volumi. Il test delle
  intersezioni prosegue considerando questi volumi e non più i solidi. Le
  prestazioni aumentano perchè i volumi utilizzati sono geometricamente più
  semplici rispetto ai solidi.
+ Partizionamento spaziale: suddivide la scena in celle
+ Volume di contenimento gerarchico: suddivide la scena in celle organizzate
  secondo un albero
+ Octree: suddivide la scena in celle organizzate secondo un albero dove ogni
  nodo ha un numero costante di figli
+ KDTree: suddivide la scena in celle organizzate secondo un albero dove si
  cerca di assegnare le mesh ai figli di ogni nodo in numero uniforme

### Che cos'è la rasterizzazione?

Processo che produce un immagine raster a partire da un qualche altro tipo di
immagine (vettoriale, scena 3D, etc..)

### Che cosa si intende per pipeline grafica?

Processo che implementa la rasterizzazione.

// TODO: Aggiungere immagine

### Cos'è il volume di vista canonico?

E' il tronco di piramide compreso tra due piani paralleli mappato su un
parallelepipedo retto

// TODO: Aggiungere immagine

### Cosa sono e a cosa servono le operazioni di clipping e backface culling?

Il clipping rimuove (eventualmente tagliando) gli oggetti che non sono
all'interno del frustum.

Il backface culling rimuove le facce la cui normale non è concorde con quella
della telecamera, cioè le facce di cui vediamo il retro e non il fronte.

### Si descriva l'algoritmo Z-buffer

Per ogni raggio uscente dalla telecamera vogliamo trovare l'oggetto più vicino.
Per farlo, controlliamo uno ad uno gli oggetti che intersecano il raggio e
controlliamo la distanza dell'intersezione. Mano a mano che facciamo questo,
usiamo una matrice per considerare sempre e solo l'oggetto più vicino e scartare
quelli più lontani.

### Si descrivano brevemente le operazioni di scan conversion. Qual è il vantaggio dell’algoritmo di Bresenham per rasterizzare i segmenti?

Scan conversion è sinonimo di rasterizzazione. Alcuni algoritmi sono:

+ Point Sampling
+ Midpoint (Bresenham)
+ Pineda (Pixel Walk)

Solitamente si usa il Midpoint (Bresenham) perchè più performante perchè non
utilizza arrotondamenti e prodotti.

### Qual è la differenza tra flat e Gouraud shading?

Il flat shading usa per ogni pixel di una certa faccia la stessa normale.
Gouraud invece interpola le normali dei vertici della faccia sui pixel. Il flat
shading è molto meno performante rispetto a Gouraud a parità di qualità visiva.

### Cosa si intende per compositing?

E' la tecnica utilizzata per renderizzare oggetti traslucidi che si
sovrappongono sulla piano di vista.

### Cosa sono gli shader?

Programmi inseriti nella pipeline di rasterizzazione per alterare i processi che
avvengono in questa e quindi l'output

### A cosa serve il texture mapping

Tecnica usata per proiettare informazione, codificata sotto forma di immagine
raster (in questo caso prendono il nome 'texture'), su un modello 3D.

### Quali tipi di informazione possono contenere le texture?

Possono contenere valori scalari come trasparenza, bumpiness, roughness,
metalness oppure valori vettoriali come colore, normale.

### In che modo la variazione di distanza degli oggetti può influire nell'effetto del texture mapping?

Durante il rendering il texture mapping crea una relazione tra i pixel
dell'immagine renderizzata e i pixel della texture (texel). Questa relazione
spesso non è 1:1. Ci sono due casi:

+ Magnification: se l'oggetto è vicino alla telecamera più pixel del viewport
  corrispondono allo stesso texel
+ Minification: se l'oggetto è lontano dalla telecamera più texel corrispondono
  allo stesso pixel del viewport

![](https://i.imgur.com/dc1msHT.png)

### In quali modi si possono costruire le mappature delle immagini sui modelli?

+ Color mapping: applica una texture sulla superficie del modello
+ Bump mapping: altera la normale. di conseguenza l'aspetto del modello sarà
  diverso senza intaccarne la geometria
+ Enviroment mapping: spiegato nella domanda successiva
+ Image-based lighting: tramite un cube map (utilizzato anche dall'environment
  mapping) codifica la luce ambientale proveniente da ciascuna direzione verso
  un oggetto. elimina la componente ricorsiva del modello di illuminazione
  perchè sappiamo già la luce entrante da ciascuna direzione.
+ Light map: codifica la luminosità in ogni punto della superficie di un
  modello. non c'è bisogno di alcun ray tracing perchè la luce in ogni punto è
  già nota perchè calcolata a priori

![](https://i.imgur.com/rA5D9G6.png) ![](https://i.imgur.com/ADgjv1i.png)

### Cosa è l’environment mapping?

Codifica la luce proveniente da ciascuna direzione verso un oggetto. Al
contrario della Image-based lightning non fornisce un'informazione
successivamente utilizzata nel calcolo del colore ma direttamente il colore da
applicare sull'oggetto.

### Quali sono le proprietà delle ombre proiettate?

+ Se la sorgente è **puntiforme** le ombre che proietta presentano bordi netti
+ In caso di scene statiche le ombre non cambiano e non dipendono dal punto di
  vista
+ Se la sorgente luminosa corrisponde con la telecamera, non vi è nessuna ombra
+ Per ottenere l'ombra che un oggetto A manifesta su un oggetto B, si proietta A
  su B lungo la direzione della luce

### Come si può implementare con la pipeline grafica la tecnica dello shadow volume?

Abbiamo a disposizione i volumi in ombra della scena. Per ogni pixel del
viewport tracciamo il raggio uscente e cerchiamo il primo oggetto visibile.
Dobbiamo capire se il punto colpito è in ombra o meno. Per farlo contiamo quante
volte il raggio entra ed esce dai volumi in ombra rispettivamente aumentando e
decrementando un contatore. Alla fine il valore ottenuto indicherà se il punto
intersecato dal raggio è in ombra (contatore > 0) oppure no (contatore = 0).

### Quali sono i limiti delle varie tecniche di generazione delle ombre?

+ Projective shadows
  + L'ombra potrebbe compenetrarsi con una superficie provocando z-fighting
  + Contorni netti
  + Risultato visivo non bello in presenza di texture complesse
  + Poligoni multipli portano a ombre piu scure
  + Le ombre vanno ridisegnate anche se l’oggetto non si muove

+ Shadow maps
  + Si puo avere un problema di discretizzazione, infatti a seconda della
    prospettiva il pixel della shadow-map puo coprire molti pixel dell’immagine
    creando artefatti a blocchi (pixellatura delle ombre).

+ Shadow volume
  + Problemi con tanti poligoni, computazionalmente pesante
  + Se il punto di vista è già in ombra il contatore dovrebbe partire con un
    valore diverso da 0

### Quali sono i limiti della keyframe animation?

+ E' una tenica meccanica e dispendiosa in termini di tempo
+ E' difficile produrre animazioni fluide e naturali o comunque richiede tanti
  keyframe

### Quali tecniche si usano per animare corpi umani e facce?

+ Blend shape animating: interpolazione lineare dei vertici
+ Skinning: deformazione delle mesh attraverso uno scheletro

### Cosa si intende per skinning e rigging?

Lo skinning è una tecnica che permette di associare uno scheletro ad una mesh e
di deformarla traslando, ruotando, scalando le ossa.

Il rigging è una tenica che permette di controllare uno scheletro ad alto
livello. Si utilizza uno schema gerarchico pesato.

### Qual è la differenza tra cinematica diretta ed inversa?

Nella cinematica diretta controlliamo il movimento dei singoli ossi. (ad esempio
muovo singolarmente ogni singolo osso della gamba)

Nella cinematica diretta si specificano alcuni vincoli e si lascia a degli
algoritmi il compito di produrre un movimento realistico delle ossa. (ad esempio
muovo solo il piede e un algoritmo produrrà i movimenti delle ossa della gamba)

### Di cosa si occupano visualizzazione scientifica e visualizzazione dell'informazione?

Visualizzazione scientifica: permette di visualizzare dati ottenuti con sensori
o simulazioni

Visualizzazione dell'informazione: rappresentazinoe con mezzi grafici di dati
statistici (ad esempio: istogrammi, infografiche, etc..)

### Cos’è il colore?

// TODO: Copiare dalla domanda della prima slide

### Cosa significa stimolo preattentivo?

Reazione involontaria che attira la nostra attenzione su una certo particolare

### Quali sono i passi di un’applicazione di visualizzazione scientifica interattiva

+ Inserimento dati
+ Filtro
+ Mappatura e arricchimento del dataset
+ Codifica in una struttura dati
+ Visualizzazione/Renderizzazione

### Quali tipi di strutture dati si utilizzano nella visualizzazione scientifica?

![](https://i.imgur.com/xvwhi8y.png)

### Come si estraggono le isosuperfici dai volumi?

Con l'algoritmo dei marching cubes. Per ogni cella del dataset controlliamo
quali vertici sono sopra a una data soglia e quali sotto. La configurazione di
mappa poi ad una superficie. Rimane da collegare insieme le diverse superfici

### Quali problemi ha l’algoritmo che si usa a tale scopo?

+ Alcune configurazioni sono superfluee
+ Alcune configurazioni producono artefatti nelle superfici

### Cos’è il rendering volumetrico diretto?

Tecnica di rendering in cui non ci si ferma al primo punto intersecato ma in cui
il colore visualizzato si ottiene dalla funzione di trasferimento a cui abbiamo
dato in input un valore che è funzione di tutti i punti contenuti nel raggio.

### Cosa si intende per funzione di trasferimento?

Funzione che mappa i valori del dataset in un colore da visualizzare

### Cosa si usa al posto della normale alla superficie nello shading volumetrico per creare gli effetti di illuminazione?

Si usa il gradiente del dataset vettoriale. Come visto in Analisi II il
gradiente di una funzione è sempre perpendicolare alla curva di valore costante.

### Cosa sono i glifi?

Elementi grafici come icone, freccie, cilindri, etc.. che permettono di
visualizzare i valori contenuti nel dataset in vari punti

### Perché è critico il campionamento della posizione delle frecce per la visualizzazione dei campi vettoriali?

Perchè altrimenti si ottiene un risultato con troppa occlusione ed è difficile
interpretare la visualizzazione

### Cosa sono le streamlines? Quando corrispondono alla traiettoria delle particelle nel campo?

Sono linee che rappresentano un dataset in cui ciascun valore è un vettore. Si
ottiene integrando il dataset a partire da una serie di punti campionati.

Corrispondono alle traiettorie delle particelle quando integriamo in entrambe le
direzioni fino ad uscire dal volume del dataset. Cioè visualizziamo le
traiettorie per intero e non solo alcuni tratti.
