---
layout: default
title: Home
nav_order: 1
description: "ZanzoCam transforms a Raspberry Pi into a remote webcam."
permalink: /
---

# ZanzoCam - Webcam remota per Raspberry Pi
{: .fs-9 }

ZanzoCam scatta foto a intervalli regolabili e le invia al tuo server. E' pensato per funzionare in maniera autonoma per lunghi periodi.
{: .fs-6 .fw-300 }

[Scarica immagine](https://github.com/ZanSara/zanzocam/releases/latest/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [Vai su GitHub](https://github.com/ZanSara/zanzocam){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

-----

## Setup

Per ottenere una ZanzoCam funzionante, e' necessario avere [l'hardware necessario](hardware-setup), un [server](software-setup#server), e il [software](software-setup#raspberry).

## Caratteristiche

ZanzoCam e' pensata per il monotoraggio a lungo termine di aree remote, per cui e' pensata per essere autonoma e affidabile anche in zone di difficile accesso.

ZanzoCam supporta diverse configurazioni hardware, che puoi vedere in [questa pagina](hardware-setup). Alcune sono piu' semplici, per aree disponibilita' di corrente e a internet, mentre altre sono piu' estrememe possono funzionare a pannelli solari e con una connessione 3G.

### Richiede:
- Un Rapsberry Pi connesso a Internet.
- Una cartella FTP **oppure** un server con IP pubblico o dominio, dove caricare le foto e i logs di esecuzione.

### NON richiede:
- IP pubblico/statico per il Raspberry Pi.
- VPN.
- Nessun vero requisito per il server (una cartella FTP e' sufficiente).

### Supporta:
- Semplici modifiche alla foto (aggiunta testo e/o loghi, aggiunta data e ora, risoluzione, compressione, ...)
- Connessione Ethernet o WiFi o 3G
- Qualunque scheda che supporta Raspberry Pi OS e webcam compatibile con `picamera` (anche se cio' puo' richiedere una ricalibrazione).
- Intervalli orari configurabili.
- Pausa notturna nello scatto delle foto.
- Hot-swap del server.

### NON supporta:
- Video
- Accesso diretto al Raspberry Pi da remoto (la configurazione remota e' possibile, ma asincrona)
- Strategie di upload diverse da HTTP o FTP (niente Dropbox o Google Drive).

