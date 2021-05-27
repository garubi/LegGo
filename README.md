# leggS
Appunti disordinati per la realizzazione di un leggio per testi in PDF, usando un vecchio monitor e un raspberry PI

## descrizione ##
- Il leggio è costituito da un monitor ldc a cui è collegato un Raspberry PI (abbreviato in RPI), il tutto inserito in una robusta valigetta che consente di usarlo semplicemente aprendola e accdendendo il tutto. 
- Idealmente dovrebbe funzionare in visualizzazione" senza l'ausilio di tastiera e mouse. Per fare questo è previsto che il PDF col testo possa essere sfogliato con un pedale USB.
- Potrebbe essere utile prevedere alcuni pulsanti aggiuntivi direttamente collegato ai GPIO di RPI per alcune funzioni utili durante lo show.
- L'uso di tastiera e mouse può essere previsto per la fase di configurazione.
- Almeno una porta USB deve essere accessibile per poter caricare i file PDF.

## funzionamento ##
- al boot deve aprirsi automaticamente il lettore PDF caricando l'ultimo documento aperto. (in questo modo possiamo accendere e usare LeggS subito, senza bisogno di fare altro).
- se abbiamo bisogno di creare/configurare la scaletta chiudiamo il lettore PDF (come?) e lanciamo PDFmixtools (come?).

- Durante lo show usiamo un file PDF generato ad hoc sulla base della scaletta dello show, indicativamente con una canzone per pagina.
- Per andare avanti e indietro usiamo un due pedali collegati via USB (vedi progetto PushPush) che inviano i due "keypress" necessari per pagina avanti e pagina indietro.
- Il file con i testi show può 
  - essere preparato prima e poi caricato nel raspberry oppure
  - generato direttamente sul raspberry partendo dai  testi delle canzoni salvati in singoli file PDF (un file per ogni canzone) e sempre disponibili nella cartella "originali" sul RPI.
- Per generare il file dello show partendo dai singoli pdf usiamo 'pdfmixtool'.
- Per visualizzare il PDF usiamo o qpdf (preinstallato in RPI) che però non supporta il partire già a schermo pieno o xpdf (che però bisogna vedere se supporta le due pagine affiancate, che potrebbero essere utili).


## links ##
- (PDFmixtools)[https://snapcraft.io/install/pdfmixtool/raspbian]
- per mappare GPIO a keystrokes si può usare (RetroGame)[https://learn.adafruit.com/retro-gaming-with-raspberry-pi/adding-controls-software]
- (lanciare il lettore PDF automaticamente)[https://learn.sparkfun.com/tutorials/how-to-run-a-raspberry-pi-program-on-startup#method-2-autostart]
- (aggiungere icone sul desktop)[https://raspberry-projects.com/pi/pi-operating-systems/raspbian/gui/desktop-shortcuts]

## i pulsanti... ##
- mostra a schermo intero (automatizziamo in qualche modo???)
- pagina avanti (? duplica il pedale... ? ) 
- pagina indietro (? duplica il pedale... ? ) 
- due pagine affianate/una pagina
- fit to page /larghezza pagina
- zoom +
- zoom -
- open new file (ma poi come scelgo le altre scalette??)
