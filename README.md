# LegGo
Appunti disordinati per la realizzazione di un leggio per testi in PDF, usando un vecchio monitor e un raspberry PI

## descrizione ##
- Il leggio è costituito da un monitor ldc a cui è collegato un Raspberry PI (abbreviato in RPI), il tutto inserito in una robusta valigetta che consente di usarlo semplicemente aprendola e accdendendo il tutto. 
- Idealmente dovrebbe funzionare in visualizzazione" senza l'ausilio di tastiera e mouse. Per fare questo è previsto che il PDF col testo possa essere sfogliato con un pedale ~~USB~~ collegato ai due GPIO.
- Potrebbe essere utile prevedere alcuni pulsanti aggiuntivi direttamente collegato ai GPIO di RPI per alcune funzioni utili durante lo show.
- L'uso di tastiera e mouse può essere previsto per la fase di configurazione.
- Almeno una porta USB deve essere accessibile per poter caricare i file PDF.
- Assicuriamoci che il raspberry o lo schermo non vadano mai in risparmio energetico/screensaver ecc
- per essere certi che i pedali e i pulsanti funzionino penso che dobbiamo forzare la finestra del visualizzatore PDF ad essere sempre in primo piano (e forse non basta...) o forse basta che sia a tutto schermo (?)

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
- per mappare GPIO a keystrokes si può usare (RetroGame)[https://learn.adafruit.com/retro-gaming-with-raspberry-pi/adding-controls-software] (gitHub project)[https://github.com/adafruit/Adafruit-Retrogame]
- (lanciare il lettore PDF automaticamente)[https://learn.sparkfun.com/tutorials/how-to-run-a-raspberry-pi-program-on-startup#method-2-autostart]
- (aggiungere icone sul desktop)[https://raspberry-projects.com/pi/pi-operating-systems/raspbian/gui/desktop-shortcuts]
- Raspberry Kiosk how to - (con indicazioni su come evitare che vada in risparmio energetico Standby)[https://pimylifeup.com/raspberry-pi-kiosk/]
- alcune soluzioni per forzare il visualizzatore in primo piano e non perdere il focus [https://www.raspberrypi.org/forums/viewtopic.php?t=236620]
- impostare monitor in verticale/portrait: https://www.raffaelechiatto.com/ruotare-lo-schermo-con-il-raspberry/
- un pulsante per spegnimento e avvio: https://www.stderr.nl/Blog/Hardware/RaspberryPi/PowerButton.html https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=206921#p1280779
- risoluzione monitor https://www.raspberrypi.org/forums/viewtopic.php?p=194596&sid=e74141dc5bfb9394f4ac2d1b7f068d7f#p194596 https://www.raspberrypi.org/documentation/configuration/config-txt/video.md
- GPIO LCD Menu using buttons https://www.raspberrypi.org/forums/viewtopic.php?f=32&t=23375 e una specie di codice finito: https://www.raspberrypi.org/forums/viewtopic.php?p=258411&sid=82730944e5581d970a6af7209e2a21b2#p258411
- How to use gpio buttons in a .sh file? https://www.raspberrypi.org/forums/viewtopic.php?f=45&t=111883&start=25
- intro a xbindkeys (idea è generare keypress con retrogame e generare automazioni con altri programmi o script)
- lunga lista di tools per AutoHokey replicate https://www.autohotkey.com/boards/viewtopic.php?t=9806
- Autokey https://code.google.com/archive/p/autokey/ 
- 

## i pulsanti... ##

1. spegni tutto (magari con longpress sennò è un rischio...)
1. bianco/nero
1. fit to page /larghezza pagina
1. lancia visualizzatore
1. pagina avanti (? duplica il pedale... ? ) 
1. pagina indietro (? duplica il pedale... ? ) 
1. zoom +
1. zoom -
1. open new file (ma poi come scelgo le altre scalette??)

## log della preparazione ##
- create le due cartelle 'testi-accordi' (che contertrà i file singoli delle canzoni) e 'scalette' (che conterrà i file con le canzoni in ordine per i vari show)
- predisposto qpdfview (visualizzatore pdf) per aprirsi sempre con l'ultimo documento aperto. rimosse toolbar. ecc ecc 
- impostato autostart per qpdfview per aprirsi automaticamente al boot
- installato retrogame per inviare keystroke dai GPIO (per abilitare pedali e pulsanti)
- installato wmctrl per inviare il comando che forza il visualizzatore a stare in primo piano ( wmctrl -r qpdfview -b add,above )
- scritto e installato lo script che lancia qpdfview, lo espande a pieno schermo e poi lo fissa in primo piano (soluzione non ottimale perchè può comunque perdere il focus) - potrei risolvere se ogni volta che ricevo un comando dai pedali riporto il focus su qpdfview con il solito magico wmctrl
- installato pdfmixtool
- modificato /boot/config.txt per usare il monito in modalità verticale/portrait (ma la risoluzione è "stirata", va sistemata)

