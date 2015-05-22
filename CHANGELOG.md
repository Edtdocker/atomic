# Changelog

## DIA 1-5 [22/04/2015 - 29/05/2015]
- Hem llegit la documentació sobre el projecte Atomic i hem estudiat els diferents conceptes relacionats.	
- Hem preparat el l’entorn de treball de GitLab enllaçant-lo amb cada equip.		
- A cada equip de treball hem creat una maquina virtual amb una imatge de Fedora21 Cloud precarregada, i hem configurat l’accès local i remot de l’usuari fedora.		

## DIA 6 [04/05/2015]
- Hem afegit més espai de disc per als Dockers en tots els equips.		
- Hem començat a configurar el Kubernetes, deixant un dels pcs com a master del cluster i 2 com a minions.		

## DIA 7 [05/05/2015]
- Configurem el Flanneld, que s’encarrega de gestionar la xarxa entre Dockers, en totes les maquines.		
- Comencem a provar les eines que ofereix el Kubernetes.		

## DIA 8 [06/05/2015]
- Introducció als Dockers en Fedora21. Diversos errors al implementar-los.		

## DIA 9 [07/05/2015]
- Hem fet funcionar una imatge Docker amb un server httpd. Falta resoldre els problemes al compartir arxius entre la màquina host i el Docker.		
- Intentem crear un registre local a la màquina master per a pujar les imatges Docker i poder compartirles entre els minions, però trobem problemes amb el trafic https.		

## DIA 10 [08/05/2015]
- Deixem aparcat el tema del registre local i comecem a configurar diversos Dockers que ofereixin serveis en els 2 minions. Per el moment disposem de Dockers amb servei httpd, postgresql i un Docker que executa un programa de python amb l’objectiu d’aconseguir crear un cluster de Dockers que ofereixin aquests serveis controlats pel Kubernetes.		

## DIA 11 [11/05/2015]
- Estem configurant la imatge de server postgresql amb l’objectiu de disposar de diversos dockers amb serveis de postgresql que comparteixin les dades d’una base de dades. Ens estem troban molts problemes de permisos entre els hosts i els dockers.		
- Hem aconseguit que tots els dockers consultin la mateixa base de dades, encara que no s’actualitza en “temps real”, cal reiniciar el docker per veure els canvis que ha fet una altra màquina.		
- Una màquina virtual ha “mort” i cal configurar-ho tot de nou en aquest minion.		

## DIA 12 [12/05/2015]
- La imatge per crear dockers amb el servei de postgres ja funciona amb els volums compartits, tots els dockers d’un mateix host poden accedir a la mateixa base de dades.		
- Comencem la configuració del Kubernetes per a la gestió de dockers entre diversos hosts automàticament. Estudiem el format dels fitxers .json i .yaml que serveixen per configurar els dockers i replicationcontrollers.		
- Ens trobem alguns problemes per sincronitzar el Atomic master amb el minion.		

## DIA 13 [13/05/2015]
- Hem aconseguit que funcioni la sincronització entre l’Atomic master i els minions, on, el master controla 2 minions amb els serveis web i postgresql.  Aquest s’encarrega que existeixi un Docker d’aquests serveis sempre actiu.		
- Intentem monitoritzar el funcionament del Kubernetes mitjançant el cockpit. Però ens trobem amb problemas a l’hora de poder monitoritzar més d’un host.		

## DIA 14 [14/05/2015]
- Hem arreglat la màquina virtual que donava problemes i, per tant, ja disposem d’un master i dos minions.		
- Hem solucionat els problemes de permisos i ja podem monitoritzar via cockpit els 3 atomic hosts.		
- Hem intentat solucionar el repositori local sense éxit, per tant, de moment, els minions solament poden executar imatges que tinguin localment o es pugin descarregar del repositori públic.		

## DIA 15 [15/05/2015]
- Hem configurat un repositori public, que, mitjançant tags adequats en les imatges que hem creat, podem pujar-les o baixar-les còmodament, i, per tant, els minions poden executar Dockers manats pel master encara que no els tinguin localment.		
- Estem intentant modificar el Dockerfile del servei postgresql amb l’objectiu d’automatitzar-ho tot i que es pugui executar en un host nou de manera independent. Estem tenint problemes amb els volums compartits.		
- Hem vist que via Dockerfile no es poden automatitzar la creació de volums compartits, per tant, intentarem crear un volum compartit en la màquina màster i els minions accediran a aquest.		

## DIA 16 [18/05/2015]
- Mirant la documentació comprovem que no és possible compartir volums remotament ni fer el lligam entre volums compartits via Dockerfile. Per tant, fem el lligam abans del Dockerfile i creem un de nou per a que el servei postgresql funcioni correctament.		
- Comencem a crear la documentació referent al Dockers.		

## DIA 17 [19/05/2015]
- Creem els serveis corresponents de Kubernetes per a donar accès desde l’exterior a la base de dades postgres i al server web.		
- Intentem afegir alguns Dockers simples més amb diversos sistemes operatius Linux de proves, però mitjançant el Kubernetes, els Dockers es tanquen sols i no es poden implementar correctament.		

## DIA 18 [20/05/2015]
- Continuem amb la documentació		
- El servei de postgresql ha deixat de funcionar i estam intentant arreglar-lo.		

## DIA 19 [21/05/2015]
- Hem arreglat el servei de postgres.		
- Ha aparegut un bug en el kernel del minion Atomic02 que, encara que s’eliminessin totes les imatges i containers, la data i la metadata dels dockers no varia, i no deixa fer res.		
- Hem aconseguit solucionar el problema.		
- Continuem amb la documentació.		

## DIA 20 [22/05/2015]
- Continuem amb la documentació.		
- Comprovem que tot funciona correctament.		
- Ens assegurem que tots els documents del projecte es troben al repositori de GitHub.		
- Preparem el document de la presentació mitjançant Pandoc		
