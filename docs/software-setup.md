---
layout: default
title: Setup Software 
nav_order: 3
description: "How to setup a ZanzoCam from scratch."
permalink: /software-setup
---

# Setup del Software

Per ottenere una ZanzoCam funzionante, e' necessario configurare sia il Raspberry Pi che il server che ricevera' le foto.


## Server

Il server puo' essere configurato per funzionare in modalita' HTTP o in modalita' FTP. Scegliere una sola delle due modalita'.

### Modalita' HTTP

ATTENZIONE: Questa configurazione richiede PHP5+ sul server.

1. Copiare il contenuto della cartella `remote-server` nella posizione dove si vuole che le foto vengano caricate.

2. Navigare alla URL dove e' stato caricato il contenuto della cartella. Il file dovrebbe ritornare il contenuto del file di configurazione in formato JSON.

3. Navigare alla URL dove e' stato caricato il contenuto della cartella e aggiungere `/control-panel`. Dovrebbe aprirsi l'interfaccia di configurazione di ZANZOCAM. Cambia i parametri nel modo che preferisci e poi premi Salva.

![](/ZanzoCam/assets/images/pannello-remoto.png)

- La sezione Server e' critica al funzionamento di ZanzoCam. Assicurati di selezionare HTTP e, nella URL, copia la URL dove si trova il pannello e rimuovi la parte `control-panel` alla fine. Per esempio, se il tuo pannello di controllo appare su `https://example.com/my-website/zanzocam/control-panel`, nella URL devi scrivere `https://example.com/my-website/zanzocam/`.

- Questa interfaccia e' solo cosmetica: il file di configurazione e' in formato testo puo' essere modificato a mano, purche' contenga sempre e solo JSON valido. Se cio' non accade, la webcam rifiutera' il nuovo file di configurazione e continuera' a usare quella vecchio finche' non riuscira' a scaricare in file valido dal server. Il rifiuto del file di configurazione e' reso evidente nei log, che vengono caricati in ogni caso, quindi controllarli sempre in caso di problemi.

### Modalita' FTP

1. Apri il file `remote-server/control-panel/index.html` con il tuo browser.

2. La pagina ti chiedera' di selezionare un file di configurazione. Seleziona il file `remote-server/configuration/configuration.json`.

![](/ZanzoCam/assets/images/pannello-locale.png)

3. Imposta i parametri che preferisci, e infine premi Salva. Ti verra' chiesto di salvare un file chiamato `configuration.json`: sovrascrivi il file aperto in precedenza.

    - La sezione Server e' critica al funzionamento di ZanzoCam. Assicurati di selezionare FTP, di impostare hostname, username a password correttamente, e se caricherai i files in una sottocartella, specificala nel campo apposito, o ZanzoCam non trovera' i file necessari.

    - Questa interfaccia e' solo cosmetica: il file di configurazione e' in formato testo puo' essere modificato a mano, purche' contenga sempre e solo JSON valido. Se cio' non accade, la webcam rifiutera' il nuovo file di configurazione e continuera' a usare quella vecchio finche' non riuscira' a scaricare in file valido dal server. Il rifiuto del file di configurazione e' reso evidente nei log, che vengono caricati in ogni caso, quindi controllarli sempre in caso di problemi.

4. Copiare il contenuto della cartella `remote-server` nella posizione dove si vuole che le foto vengano caricate.


## Raspberry

1. Scaricare l'immagine `zanzocam_<versione>.zip` da [questo link](https://github.com/ZanSara/zanzocam/releases/latest)

2. Decomprimere l'immagine con `unzip zanzocam_<versione>.zip`. Dovrebbe contenere un file chiamato `zanzocam.img`

3. Formattare l'immagine su una schedina SD. 
    - La formattazione si puo' fare con qualunque software dedicato, oppure da terminale con `dd if=zanzocam.img of=/dev/sdX bs=4M status=progress oflag=sync`, sostituendo `/dev/sdX` con il nome del dispositivo da sovrascrivere (quello senza numero alla fine, come sdb o sdx) e usando `sudo`.
    - Per vedere i dispositivi disponibili dal terminale, esegui `lsblk`.

3a. (**Opzionale**) Se hai a disposizione un Linux (o qualunque altro OS in grado di montare una partizione EXT4) e un software come [GParted](https://gparted.org/), puoi espandere la partizione. Di default ZanzoCam crea una partizione di meno di 4G, in modo da essere compatibile con la maggior parte delle schedine SD, ma e' in genere uno spreco di spazio su schede piu' grandi. Espandi pure la partizione `rootfs` fino a occupare tutto lo spazio disponibile sulla tua SD.

4. Inserire la SD nel Raspberry Pi e aspettare qualche minuto.

5. A un certo punto si attiva una rete WiFi chiamata `zanzocam-setup`. Connettere un dispisitivo con un browser a questa rete.
    - Se la rete e' visibile ma non si riesce a connettersi, e' probabilmente un problema di interferenza WiFi. ZanzoCam genera l'hotspot sul canale 8, per cui assicurati che quel canale sia libero.
    - Se la rete non appare entro dieci minuti, qualcosa e' andato storto durante la formattazione. Riprova dall'inizio per assicurarti che l'immagine che hai scaricato non sia corrotta.

6. Una volta connessi, aprire con il browser la pagina `http://10.0.0.5`. ZanzoCam chiedera' le seguenti credenziali: utente `zanzocam-user`, password `lampone`. Queste password, se possibile, vanno [cambiate in seguito](hardening).

7. Sulla pagina web che si apre, inserire i dati del WiFi e i dati del server dove vuoi che le foto vengano inviate. La URL deve corrispondere alla posizione del file `index.php` caricato in precedenza. Cliccare Configura in entrambi i blocchi per salvare la configurazione. Ignorare il bottone Setup Webcam per ora.

    - Assicurarsi che il file di configurazione sia accessibile e inserire username e password nei rispettivi campi se necessario.

![](/ZanzoCam/assets/images/web-ui.png)

8. Riavviare il Raspberry e collegarsi alla rete WiFi dove dovrebbe collegarsi.

10. Identificare l'IP del Raspberry. 
    - L'IP si puo' trovare visitando il pannello di controllo del tuo router.
    - In alternativa, e' identificabile dal terminale Linux usando `nmap -p 22 192.168.1.0/24`. Fare attenzione al terzo numero dell'IP: esso puo' cambiare a seconda della rete (ovvero invece che 192.168.**1** puo' essere 192.168.**2** oppure 192.168.**0** etc...)
    - Se dopo 5-10 minuti non si trova, assicurarsi che non abbia generato nuovamente una rete di nome `zanzocam-setup`. In quel caso, significa che i dati del WiFi sono sbagliati e non e' riuscito a collegarsi. Ripetere il procedimento dal punto 5.

11. Una volta trovato l'IP del Raspberry, aprire con il browser la pagina `http://IP-DEL-RASPBERRY` (sostituisci con l'IP del Raspberry)

12. Cliccare su Setup Webcam. Si apre una pagina dove, entro mezzo minuto, dovrebbe essere visibile un'anteprima dell'inquadratura della webcam. Una volta aggiustata l'inquadratura, premi "Scatta Foto" e aspetta che i log arrivino alla fine.

13. Entro circa due minuti, il server ricevera' la prima foto e i primi log, e da quel momento in poi il sistema procedera' in maniera autonoma. Da qui in avanti il Raspberry puo' essere controllato tramite il file di configurazione presente sul server.
    - Se cio' non accade, leggi attentamente i log ricevuti sulla pagina Setup Webcam e cerca di capire qual'e' il problema.

