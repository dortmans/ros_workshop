# Workshop "ROS voor Engineers"

Docent: Eric Dortmans (e.dortmans@fontys.nl)

## Leerdoelen

1. Basisconcepten van ROS kennen en begrijpen
2. Praktisch met diverse ROS tools kunnen omgaan
3. Zelf een toepassing kunnen bouwen met bestaande ROS nodes
4. Robots kunnen modelleren, visualiseren en simuleren
5. Een mobiel robot platform autonoom kunnen laten navigeren
6. Een traject voor een robot arm kunnen plannen

Voor het behalen van deze leerdoelen zijn een aantal sessies nodig.

## Voorkennis

Als voorkennis is nodig:

- Enige vaardigheid in het omgaan met Linux (met name Ubuntu), zowel met de muis als via commando's in een terminal window
- Enige kennis van sensoren (camera, laserscanner, etc.), en actuatoren (motoren etc.)
- Enige kennis van bewegingsleer (kinematica)
- Enige kennis van interfaces (USB, RS232, Ethernet/EtherCat)

Programmeerervaring is niet nodig, maar wel handig.

## Benodigdheden

Je moet zelf een laptop meebrengen met daarop geinstalleerd Ubuntu 14.04 plus ROS Indigo. Je kunt deze software op 2 manieren installeren:

1. als virtuele machine onder Windows (of Mac OS/X)
2. native, bijvoorbeeld als multiboot optie naast Windows

### Ubuntu plus ROS als Virtuele Machine

ROS als Virtuele Machine heeft als voordeel dat je zelf geen Ubuntu en ROS hoeft te installeren. Nadeel is dat de performance minder is dan een native installatie. Voor het draaien van 3D simulaties is een native installatie aan te bevelen.

Het enige wat je hoeft te doen is 2 files downloaden:

- een Ubuntu plus ROS image
- een programma zoals VirtualBox om dat image te executeren

Op de [Nootrix site](http://nootrix.com/) staan een aantal [kant en klare ROS Indigo images](http://nootrix.com/2014/09/ros-indigo-virtual-machine/). Het 32 bits image (RosIndigo32Bits.ova) is goed genoeg. Je kunt het via je webbrowser [hier](http://www.fhict.nl/docent/downloads/TI/MinorES/RosIndigo32Bits.ova) downloaden. Je kunt het ook [via Bittorrent downloaden](http://nootrix.com/00download/download.html?fileId=rosIndigo32BitsVMTorrent).

Het programma VirtualBox voor Windows (of Mac OS/X)kun je [hier](https://www.virtualbox.org/wiki/Downloads) downloaden. Daarna moet je het installeren net als ieder ander programma.
In plaats van VirtualBox kun je overigens ook VMware gebruiken. De VMware Player kun je [hier](http://www.filehippo.com/download_vmware_player/) downloaden.

Met VirtualBox (of VMware Player) kun je nu de RosIndigo32Bits.ova file openen en de betreffende virtuele machine opstarten.

### Ubuntu plus ROS native

Voor een native installatie moet je 2 dingen doen:

1. Ubuntu 14.04 installeren
2. ROS Indigo installeren in Ubuntu

Ubuntu Desktop 14.04 kun je [hier downloaden](http://www.ubuntu.com/download).

Hoe je ROS Indigo (Desktop - full install) moet installeren kun je [hier](http://wiki.ros.org/indigo/Installation/Ubuntu) lezen.

## Linux commando vaardigheid

Om met ROS te kunnen werken moet je enige vaardigheid hebben in het omgaan met Linux commando's. Ik kan de volgende turorials aanbevelen:

- UNIX Tutorial for Beginners: http://info.ee.surrey.ac.uk/Teaching/Unix/ 
- Linux Fun: http://linux-training.be/downloads/



