# LegGo
Appunti disordinati per la realizzazione di un leggio per testi in PDF, usando un vecchio monitor e un raspberry PI

## descrizione e requisiti ##
- Il leggio è costituito da un monitor ldc, orientato in verticale, a cui è collegato un Raspberry PI (abbreviato in RPI), il tutto inserito in una robusta valigetta che consente di usarlo semplicemente aprendola e accdendendo il tutto. 
- Idealmente dovrebbe funzionare "in visualizzazione" senza l'ausilio di tastiera e mouse. Per fare questo è previsto che il PDF col testo possa essere sfogliato con un pedale ~~USB~~ collegato a due GPIO.
- Potrebbe essere utile prevedere alcuni pulsanti aggiuntivi direttamente collegato ai GPIO di RPI per alcune funzioni utili durante lo show.
- L'uso di tastiera e mouse può essere previsto per la fase di configurazione.
- Almeno una porta USB deve essere accessibile per poter caricare i file PDF.


## funzionamento ##
- Durante lo show usiamo un file PDF generato ad hoc sulla base della scaletta dello show, indicativamente con una canzone per pagina.
- al boot deve aprirsi automaticamente il lettore PDF caricando l'ultimo documento aperto. (in questo modo possiamo accendere e usare LegGO subito, senza bisogno di fare altro).
- Per andare avanti e indietro usiamo due pedali collegati ai GPIO che inviano i due "keypress" necessari per pagina avanti e pagina indietro.
- Il file con i testi show può 
  - essere preparato prima e poi caricato nel raspberry oppure
  - generato direttamente sul raspberry partendo dai  testi delle canzoni salvati in singoli file PDF (un file per ogni canzone) e sempre disponibili nella cartella "originali" sul RPI.
- Per generare il file dello show partendo dai singoli pdf usiamo 'pdfmixtool'.
- Per visualizzare il PDF usiamo o qpdf (preinstallato in RPI) che però non supporta il partire già a schermo pieno o xpdf (che però bisogna vedere se supporta le due pagine affiancate, che potrebbero essere utili).
- se abbiamo bisogno di creare/configurare la scaletta, colleghiamo la tastiera e il mouse, chiudiamo il lettore PDF e lanciamo PDFmixtools.
- Assicuriamoci che il raspberry o lo schermo non vadano mai in risparmio energetico/screensaver ecc
- per essere certi che i pedali e i pulsanti funzionino penso che dobbiamo forzare la finestra del visualizzatore PDF ad essere sempre in primo piano (e forse non basta...) o forse basta che sia a tutto schermo (?)


## links ##
### applicazioni principali e configurazione generale ###
- (PDFmixtools)[https://snapcraft.io/install/pdfmixtool/raspbian]
- (lanciare il lettore PDF automaticamente)[https://learn.sparkfun.com/tutorials/how-to-run-a-raspberry-pi-program-on-startup#method-2-autostart]
- alcune soluzioni per forzare il visualizzatore in primo piano e non perdere il focus [https://www.raspberrypi.org/forums/viewtopic.php?t=236620]
- (aggiungere icone sul desktop)[https://raspberry-projects.com/pi/pi-operating-systems/raspbian/gui/desktop-shortcuts]
- Raspberry Kiosk how to - (con indicazioni su come evitare che vada in risparmio energetico Standby)[https://pimylifeup.com/raspberry-pi-kiosk/]

#### update marzo 2022 ####
- https://opensource.com/article/19/2/manipulating-pdfs-linux PDF shuffle sembra meglio di PDFmixtools perchè permette di aggiungere e spostare file che già sono in scaletta
- https://itsfoss.com/pdfarranger-app/ PDFArranger dovrebbe esserne il successore... temo sia molto pesante...

### Monitor ###
- impostare monitor in verticale/portrait: https://www.raffaelechiatto.com/ruotare-lo-schermo-con-il-raspberry/
- risoluzione monitor https://www.raspberrypi.org/forums/viewtopic.php?p=194596&sid=e74141dc5bfb9394f4ac2d1b7f068d7f#p194596 https://www.raspberrypi.org/documentation/configuration/config-txt/video.md

### GPIO ###
- per mappare GPIO a keystrokes si può usare (RetroGame)[https://learn.adafruit.com/retro-gaming-with-raspberry-pi/adding-controls-software] (gitHub project)[https://github.com/adafruit/Adafruit-Retrogame]

- un pulsante per spegnimento e avvio: https://www.stderr.nl/Blog/Hardware/RaspberryPi/PowerButton.html https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=206921#p1280779 (qui allungare il tempo in cui va tenuto premuto: https://www.raspberrypi.org/forums/viewtopic.php?p=1548142#p1548142) (See '/boot/overlay/README.*' for more information.)
- GPIO LCD Menu using buttons https://www.raspberrypi.org/forums/viewtopic.php?f=32&t=23375 e una specie di codice finito: https://www.raspberrypi.org/forums/viewtopic.php?p=258411&sid=82730944e5581d970a6af7209e2a21b2#p258411
- https://www.raspberrypi.org/forums/viewtopic.php?p=1455499#p1455499
- How to use gpio buttons in a .sh file? https://www.raspberrypi.org/forums/viewtopic.php?f=45&t=111883&start=25
- intro a xbindkeys (idea è generare keypress con retrogame e generare automazioni con altri programmi o script) [tutorial/esempio](https://www.linux.com/news/start-programs-pro-xbindkeys/) [altro](https://mauriziosiagri.wordpress.com/tag/xbindkeys-config/) [oppure qui](http://xahlee.info/linux/linux_xbindkeys_tutorial.html) [col suo compare XVKBD](http://xahlee.info/linux/linux_xvkbd_tutorial.html)
- oppure [triggerhappy](https://github.com/wertarbyte/triggerhappy) ([esempio per il software Volumio](https://community.volumio.org/t/keyboard-shortcuts-with-triggerhappy/4826/15)) [man page](http://manpages.ubuntu.com/manpages/bionic/man1/thd.1.html) - questo consente anche di avere pulsanti che cambiano il "mode" o il "bank": https://github.com/wertarbyte/triggerhappy/issues/4
- lunga lista di tools per AutoHokey replicate https://www.autohotkey.com/boards/viewtopic.php?t=9806
- Autokey https://code.google.com/archive/p/autokey/ 
- 

## i pulsanti... ##

1. spegni tutto (magari con longpress sennò è un rischio...)
1. bianco/nero
1. fit to page /larghezza pagina
1. lancia visualizzatore & open new file (ma poi come scelgo le altre scalette??)
1. pagina avanti (? duplica il pedale... ? ) 
1. pagina indietro (? duplica il pedale... ? ) 
1. zoom +
1. zoom -

## log della preparazione ##
- create le due cartelle 'testi-accordi' (che contertrà i file singoli delle canzoni) e 'scalette' (che conterrà i file con le canzoni in ordine per i vari show)
- predisposto qpdfview (visualizzatore pdf) per aprirsi sempre con l'ultimo documento aperto. rimosse toolbar. ecc ecc 
- impostato autostart per qpdfview per aprirsi automaticamente al boot
- installato retrogame per inviare keystroke dai GPIO (per abilitare pedali e pulsanti)
- installato wmctrl per inviare il comando che forza il visualizzatore a stare in primo piano ( wmctrl -r qpdfview -b add,above )
- scritto e installato lo script che lancia qpdfview, lo espande a pieno schermo e poi lo fissa in primo piano (soluzione non ottimale perchè può comunque perdere il focus) - potrei risolvere se ogni volta che ricevo un comando dai pedali riporto il focus su qpdfview con il solito magico wmctrl
- installato pdfmixtool
- modificato /boot/config.txt per usare il monito in modalità verticale/portrait (ma la risoluzione è "stirata", va sistemata)
- modificato /boot/config.txt per abilitare GPIO3 con pressione di 5 secondi a fare lo shutdown. un nuovo press riavvia il PI
- configurato RetroGame per pagina avanti e indietro con i due GPIO che useremo con i pedali e con un GPIO per alternare bianco/nero nel lettore PDF
- impostato `config_hdmi_boost` in /boot/config.txt a 7 per cercare di evitare lo spegnimento casuale di qualche secondo del monitor che capitava su certi palchi. (per ora sembra funzionare)

