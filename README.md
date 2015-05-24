# Atomic
## Introducció al projecte Atomic
El projecte Atomic proporciona una plataforma que permet la instal·lació i la gestió de contenidors en diferents tipus de servidors (físics, màquines virtuals, cloud…). Els hosts estan orientats a proporcionar els serveis dels containers de la manera més eficient possible i per tant, són hosts molt simples, únicament amb el software necessari.
Aquests hosts estan dissenyats a partir de l’OStree, quw és un sistema d'actualització en fase Beta per a sistemes operatius Linux que proporciona diversos objectes de sistemes de fitxers en lloc dels paquets RPM habituals. Aquests objectes son immutables i permeten que el sistema es pugui actualiztar facilment i, en cas de ser necessari, fer un rollback(És a dir, tornar a la ultima configuració que funcionaba perfectament).

La execució dels containers es fa per mitjà de Dockers i la gestió dels clusters de dockers es fa via Kubernetes.
L'objectiu principal es de disposar d'una màquina master, que controla diverses màquines slaves(anomenades minions) que cada una té diferents Dockers amb un servei. Aquests serveis seran accessibles desde l'exterior de la xarxa Atomic una vegada configurat el Kubernetes correctament. Finalment, mitjançant la eina cockpit, es pot monitoritzar i gestionar
de manera còmoda tots els hosts Atomic.
	
## Requisits
Per a la implantació del projecte Atomic és necessari:
	
* Una imatge de màquina virtual de Fedora21 o CentOS.	
	* Imatge Fedora21 per containers: [https://getfedora.org/cloud/download/](https://getfedora.org/cloud/download/)
	* Imatge CentOS : [http://buildlogs.centos.org/rolling/7/isos/x86_64/](http://buildlogs.centos.org/rolling/7/isos/x86_64/)
* Un client de virtualització (virt-manager o VirtualBox).

## Instal·lació		
Actualment, es pot dur a terme mitjançant imatges de disc pre-generades per a màquines virtuals,via Anaconda i, fins i tot, en un entorn bare metal.
El procediment principal d’instal·lació és importar la imatge al client de virtualització proporcionant les dades generals i d’usuari. Utilitzant virt-manager en Fedora21:
	
1. Descarregar i descomprimir la imatge de Fedora21
2. Inicialitzar el virt-manager
3. Crear una nova VM a partir d’importar una imatge de disk existent
4. Sel·leccionar la imatge descarregada de tipus Linux Fedora20(or later)
5. Ajustar les característiques(RAM,CPU…) de la VM.

Com la imatge de disk es minimal, necesitarem crear una imatge ISO amb la metadata(informació general de la màquina) i amb user-data(informació per a l’accès d’usuari).

	#vim meta-data
	instance-id: id1 (id qualsevol)
	local-hostname: Atomic2.escoladeltreball.org (hostname de la màquina)
	
	#vim user-data
	#cloud-config (comentari obligatori)
	password: jupiter (password per a l’usuari principal)
	ssh_pwauth: True
	chpasswd: { expire: False } (temps expiracio de la password)
	ssh_authorized_keys: (clau publica per connexió remota desatesa)
	ssh-rsa (...AAAAB3NzaC1yc2....) isx46989504@i14.informatica.escoladeltreball.org

Un cop creats els 2 fitxers, generem la imatge ISO amb aquestes dades, que posteriorment afegirem a la nostre màquina virtual.

	#genisoimage -output init.iso -volid cidata -joliet -rock user-data meta-data

L’usuari libvirt ha de tenir permisos de lectura sobre la imatge generada init.iso.
	
Una vegada tenim la imatge, obrim el virt-manager, cliquem a Open i seguidament veure els detalls. A continuació sel·leccionarem:
	
1. Add hardware > Storage
2. Select managed or other storage > Busquem la imatge init.iso recentment creada
3. Especifiquem el Device Type com a CDROM device
A partir d'ara ja podem posar en marxa la màquina virtual i accedirem amb l'usuari "fedora" i la password especificada al fitxer user-data.

Un cop arrencat el sistema per primera vegada es recomanable actualitzar el software del sistema. Per fer-ho utilitzarem el OSTree
	
	#sudo atomic upgrade
