La virtualisation lourde
========================

## Pré-requis lexical

Afin de correctement comprendre l'ensemble de ce texte, voici une brève définition des différents termes techniques qui y seront utilisé:

* **Noyau**: le noyau est le coeur du système d'exploitation. Il gère les ressources matérielles de la machine sur laquelle il est installé et assure la communication des différentes programmes et du matériel.

* **Machine hôte**: la machine hôte est la machine physique sur laquelle se trouve les différentes machines virtuelles. Son système d'exploitation sera désigné comme système hôte.

* **Machine virtuelles**: Une machine virtuelle est une machines qui n'existe que de manière logique grâce à des programme comme un émulateur ou un hyperviseur. Son système d'exploitation sera désigné comme système invité.

* **Hyperviseur**: un hyperviseur est un programme qui à la responsabilité de gérer les machines virtuelles et de réceptionner les appels systèmes d'un système d'exploitation invité, afin de les modifier pour qu'ils soit compréhensible par le système hôte et inversement dans le cadre d'une virtualisation lourde, ou bien directement par le matériel dans le cadre de la para-virtualisation.

* **Orchestration**: de manière général, l'orchestration est un mécanisme logique qui alloue la puissance de calcul à chaque processus en ayant besoin de manière équilibré en fonction de ces besoins et de la priorité du processus.  Dans le cadre d'une virtualisation lourdes, l'hyperviseur fait office d'orchestrateur des différentes machines virtuelles dont il a la charge en faisant en sorte d'équilibré équitablement l'attrivution des ressources du système hôte à chaqu'une d'elles.  Dans le cadre de la para-virtualisation, nottament réseau où les machines virtuelles, le ou les hyperviseurs et l'espace de stockage se trouve sur des machines physique différentes, l'orchestrateur est un programmes qui và faire office de lien entre les hyperviseur, les machines virtuelles et l'espace de stockage afin d'éviter tout conflit et de justment distribué les ressources matérielles à ces différents services.

## Rappel des différents types de virtualisation

La virtualisation du point de vue machine, est le fait de faire fonctionner sur une machine physique, une ou plusieurs machines virtuelle.

Ces machines peuvent être virtualisé à différents niveau:

* virtualisation d'un ordinateur complet avec tout son matériel (processeur, RAM, espace de stockage, etc...).  Cette solution est la plus compatible si l'on souhaite utiliser un système d'exploitation qui n'a pas été prévu spécifiquement pour cette utilisation car faisant abstraction du matériel physique de la machine hôte.  Cependant, elle est aussi la plus lente et gourmande en terme de ressources de par l'imitation d'un matériel quasi-complet via un hyperviseur de type 2, d'où sa dénomination de virtualisation "lourde".

![https://fr.wikipedia.org/wiki/Virtualisation#Hyperviseur_de_type_2](https://raw.githubusercontent.com/vfo1409/virtualisation/master/Diagramme_ArchiEmulateur.png "Schéma de la virtualisation lourde. Par Primalmotion — Travail personnel, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=4587919")

* virtualisation d'un système d'exploitation prévu à cet effet qui utilise directement les ressources matérielles offertes par la machine hôte à l'aide d'un hyperviseur de type 1.
Cette solution est la plus performante car les systèmes d'exploitation sont optimisé pour cette utilisation,  mais en contrapartie aussi la plus contraignants et onéreuse, particulièrement si l'on utilise des logicielle propriétaire payant.
On désigne cette virtualisation de para-virtualisation.

![https://fr.wikipedia.org/wiki/Virtualisation#Hyperviseur_de_type_1](https://raw.githubusercontent.com/vfo1409/virtualisation/master/Diagramme_ArchiHyperviseur.png "Schéma de la para-virtualisation. Par Primalmotion — Travail personnel, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=4587933")

* virtualisation d'un environnement système (un noyeau) directement dans l'espace de l'utilisateur comme n'importe quel autre logiciel.
Ce nouvel environnement dispose ensuite de son propre espace utilisateur et gère ses applications comme s'il était le noyau légitime de la machine.
Cette solution est la moins performante de par la présence de deux noyau sur la même machine, le manque d'isolation entre eux et par conséquent la dépendance du noyau émulé par rapport au noyau hôte.

![https://fr.wikipedia.org/wiki/Virtualisation#Noyau_en_espace_utilisateur](https://raw.githubusercontent.com/vfo1409/virtualisation/master/Diagramme_ArchiKernelUserSpace.png "Schéma de la virtualisation noyeau en espace utilisateur. Par Primalmotion — Travail personnel, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=4587946")

* virtualisation d'un ensemble d'application via un isolateur.
Bien que l'on ne parle pas de virtualisation d'un système dans ce dernier cas, cette solution propre à Linux permet de faire fonctionner un lots d'applications, voir même plusieurs instance de la même application, sans avoir à se préoccuper du matériel de la machine hôte.
Celà est très performant mais l'isolation n'étant pas complète, cette solution est totalement dépendante du noyau hôte.

![https://fr.wikipedia.org/wiki/Virtualisation#Isolateur](https://raw.githubusercontent.com/vfo1409/virtualisation/master/Diagramme_ArchiIsolateur.png "Schéma de la virtualisation par isolateur. Par Primalmotion — Travail personnel, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=4587937")

Ainsi dans le cadre de la virtualisation lourde, une machine vituelle est un ordinateur quasi-complet disposant d'un processeur, d'une RAM, un ou plusieurs disques durs, une ou plusieurs interface réseau et plus ou moins de périphériques externes (lecteur disquette, CD-ROM, contrôleur USB, etc...).
Cette ordinateur est géré par un hyperviseur de type 2 qui se charge de faire le lien entre la machine virtuelle et la machine hôte.  Lors de la création de la machine virtuelle, l'utilisateur se retrouve tout simplement avec une machine dont le disque dur est vide. Il lui imcombe d'installer le système d'exploitation de son choix, comme s'il venait réellemment de se procurer un ordinateur non-formaté.

Il est à noter que le système d'exploitation de la machines virtuelle doit être prévu pour la même architecture de processeur que celle de la machine hôte car les logiciel de virtualisation se contente de passer aux processeur les instruction basique émanent du système invité.
En effet, si le système invité n'est pas prévu pour l'architecture du processeur de la machine hôte, il en résultera des crash puisque le processeur recevra des instructions qui ne font pas partie de son jeu d'instructions et ne saura comment les traiter.
La solution à ce problème s'appelle l'émulation qui elle va réinterpreté les instruction du système invité pour qu'elle soit compréhensible par le processeur de la machine hôte.

## Evolution technologique de la virtualisation

### Un peu d'histoire

Le concept de virtualisation remonte au années 60 où fut mené une grande part des travaux la concernant au centre scientifique de Cambridge d'IBM avec le MIT. Ces recherches donnèrent un système expérimental qui devint une gamme de produit vendu par IBM à partir de 1964, alors désigné comme Hyperviseur, aujourd'hui appellé mainframe: la série System/360 puis 370.
Le principe de cette n'était autre que de couvrir tout les besoins informatiques scientifique et de gestion de l'époque, tout en étant compatible avec l'ensemble des autres modèles de la gamme.
Cette virtualisation état d'ordre mécanique, c'est-à-dire que l'ensemble des machines de cette gamme utilisait toutes la même architecture, prémissant le concept du compatible-PC.
La génération suivante de supercalculateur, la Série Z devenue System Z en avril 2006, prolonge cette philosophie en rendant aisé le changement d'une machine à une autre au sein de la série. De plus, cette série est justement dédié majoritairement à la paravirtualisation.

Dans les années 80, certains micro-ordinateurs de l'époque furent expérimenté des technique de virtualisation, de manière logiciel ou matériel par l'ajout de composant.
On pourra nottamment cité l'exemple de l'ADAM pour la virtualisation matérielle. Distribué en 1983, soit en tant que module d'extension pour la console CBS Colecovision la transformant en une suite bureautique pour les standarts de l'époque comportant une imprimante à marguerite, un clavier professionnel, un lecteur de cassettes et trois programmes (un traitement de texte en ROM, un compilateur BASIC et le jeu Super Buck Rogers sur cassette). Soit directement sous la forme d'un micro-ordinateur, le circuit imprimé de la console étant intégré dans le corp de la machine.

Mais la machine ayant véritablement tirée son jeu du lot dans ce domaine à ce moment n'est autre que l'Amiga vendu par Commodore International de 1985 à 1994 sur lequel il était possible de lancer en multitâche d'autre système d'exploitation comme Windows, linux ou encore Macintosh à partir de AmigaOs, le système d'exploitation natif.

A partir de la seconde moitié des années 90, des passionnées et nostalgique se lance dans des projets d'émulation de machines de la décennies précédentes comme le C64 ou la NES.
Enfin, à l'orée des années 2000, la société VMware lance et popularise un système de virtualisation lourde propriétaire et payante pour les architecture de type x86, architecture standart des compatibles-PC. Devant le succès de ce procédé, d'autres projets du même accabie voit le jour comme le logiciel libre Virtual Box maintenant détenu par Oracle, ou encore VirtualPC spécialisé pour une utilisation des différentes versions de Windows de Connectix, racheté en octobre 2003 par Microsoft.

### Soucis technique

Historiquement l'architecture standart des compatible-PC est celle du x86, basé sur le jeu d'instruction du processeur 8086 d'Intel crée en 1978.
Au niveau des systèmes d'exploitation 32 bits, cette architecture mets à disposition des anneaux de protection des données en mémoires au nombre de quatre, le noyau 0 étant généralement utilisé par le noyau du système afin d'accéder au matériel et le 3 pour les programmes. L'objectif est de cloisonné les données des différents programmes afin que jamais ils n'accèdent directement à celles se trouvant dans d'autres anneaux. C'est justement le rôle du systèmes d'exploitation que de faire office de pont entre les anneaux lorsque celà est nécessaire.

Remarquant que les anneaux 1 et 2 était rarement utilisé, les principaux fabriquant de processeur, Intel et AMD, les ont simplement supprimé lors de la mise au point de l'architecture x64.

![https://www.antoinebenkemoun.fr/wp-content/uploads/2009/08/rings.jpg](https://raw.githubusercontent.com/vfo1409/virtualisation/master/rings.jpg "Schéma des anneaux de protection sur une architecture x86 pour une para-virtualisation")

Ce qui posea un problème pour les logiciels de virtualisation qui placeait justement leurs hyperviseur dans l'un de ces deux noyau par mesures de sécurité. Les éditeurs de ces logiciels trouvèrent comme solutions de placer l'hyperviseur dans l'anneau 0 et le noyeau du sytème dans le même anneau que les applications (inversement dans le cadre d'une virtualisation lourde), tout en faisant en sorte de le cloisonné au maximum pour que les applications n'y est pas directement accès.

![https://www.antoinebenkemoun.fr/wp-content/uploads/2009/08/Image-1.png](https://raw.githubusercontent.com/vfo1409/virtualisation/master/Image-1.png "Schéma des anneaux de protection sur une architecture x64 pour une para-virtualisation")

Face à l'importance de plus en plus croissantes de la virtualisation, nottament dans le domaine professionnel, Intel et AMD modifière leurs processeurs pour étentre leurs jeu d'instruction en y ajoutant des fonctionnalités de virtualisation matériellement assistée, parmis celle-ci on trouve:

* l'ajout d'un anneau -1 pour y placer l'hyperviseur ou le noyeau afin que le noyeau du système soit à nouveau correctement isolé seul dans un anneau.

* de données à l'hyperviseur un accès directes au ressources matériels afin de mieux les redistribuées en fonction des besoins de chaque machines virtuels

* et ainsi de permettre au machines virtuelles de ne plus êtres totalement dépendante du noyeau du système hôte puisque l'hyperviseur dispose d'un accès directe au processeur

![https://www.antoinebenkemoun.fr/wp-content/uploads/2009/08/Image-2.png](https://raw.githubusercontent.com/vfo1409/virtualisation/master/Image-2.png "Schéma des anneaux de protection sur une architecture x64 pour une para-virtualisation matériellement assistée")

## Exemple par la pratique

Dans cet exemple, nous allons créer et configurer par ligne de commande une machine virtuelle par virtualisation lourde à l'aide du logiciel VirtualBox d'Oracle via son utilitaire intégré VBoxManage. Afin de prouver que le système invité n'est pas dépendant du système hôte, nous y installerons Windows XP 32bits.

La procédure d'instalation d'un programme étant propre au système d'exploitation que vous utilisez et à vos préférence lorsque plusieurs procédures sont possible, nous n'aborderons pas cette étape ici.

Nous précisons par ailleurs que cet exemple est effectué sous Ubuntu 16.04 LTS 64bits avec la version 5.0.24_Ubuntu r108355 de VirtualBox sur une machine ayant une architecture x64 avec un Intel Core I5 avec prise en charge de la virtualisation matériellement assitée et munie de 8Go de mémoire vive.

Tout d'abord, il faut crée le fichier de configuration de la machine virtuelle que nous nommerons winXP (vous l'aurez deviné, nous y installerons par la suite Windows XP 32bits) et que nous enregistrerons directement à la console de VirtualBox avec `--register`:

    VBoxManage createvm --name winXP --register

Ce fichier ce trouvera par défaut dans le dossier `VirtualBox VMs/winXP` dans votre home.

Vous pouvez crée ce fichier sans l'enregistrer à la console de VirtualBox mais il faudra alors taper la commande suivante pour l'enregistrer:

    VBoxManage registervm '/votre/home/VirtualBox VMs/winXP/winXP.vbox'

Nous allons ensuite crée le fichier qui représente le disque dur virtuel de la machine. Ceci n'étant qu'un exemple nous lui attribuons 5GB.
Ce fichier se trouvera dans le dossier courant au moment de l'exécution de la commande:

    VBoxManage createhd --filename winXP --size 5000

Configuration en ligne de commande de notre machine virtuelle

Maintenant il faut définir le type de système d'exploitation que nous souhaitons utilisé:

    VBoxManage modifyvm winXP --ostype WindowsXP

Pour une liste de l'ensemble des type de système d'exploitation pris en charge:

    VBoxManage list ostypes

Nous lui allourons 100MB de mémoire vive:

    VBoxManage modifyvm winXP --memory 100

Passons au contrôleur de stockage. Ici il s'appellera IDE pour un contrôleur de stockage de type ide, avec un chipset émulé PIIX4 pour y connecté ensuite un disque dur et un lecteur DVDROM tout en étant amorçable, c'est-à-dire qu'il sera lancé par le BIOS au démarrage de la machine:

    VBoxManage storagectl winXP --name IDE --add ide --controller PIIX4 --portcount 2 --bootable on

**Remarque:** vous pouvez très bien chosir un contrôleur de type sata et précisé le nombre de port dont celui-ci dispose avec `--portcount <1-30>`. Ici nous choisissons ide car notre version de l'installeur de Windows XP ne dispose pas de pilote pour le sata et crash donc dès le démarrage.

Nous allons y attacher tout d'abord un disque dur qui sera représenté par le fichier de disque dur virtuel que nous avons crée plus tôt:

    VBoxManage storageattach winXP --storagectl IDE --port 0 --device 0 --type hdd --medium "/chemin/absolu/winXP.vdi"

Puis y attacher un lecteur DVDROM avec l'image disque du CDROM d'installation de Windows XP:

    VBoxManage storageattach winXP --storagectl IDE --port 1 --device 0 --type dvddrive --medium "/chemin/absolu/image/disque/xppro.iso"

--medium peut prendre la valeur emptydrive si vous ne voulez pas y mettre d'image disque, host:/chemin/lecteur/physique pour que la machine virtuelle utilise un lecteur de la machine hôte ou encore addition pour installé les addons invités permettant le partage de fichiers entre le système hôte et l'invité ou encore une meilleur gestion de capture des entrée entre les deux système.

Nous allons ensuite configuraté l'affiche et le son, nottament l'accélération 3D (si vous en disposez), 100 Mo de mémoire vidéo (Vram), le pilote audio et son codec:

    VBoxManage modifyvm winXP --vram 100 --accelerate3d on --audio alsa --audiocontroller ac97

Enfin, la machine virtuelle disposera d'une seul carte réseau de type PCnet-FAST III, qui sera relié au réseau et ce par la méthode NAT qui est la plus simple d'utilisation:

    VBoxManage modifyvm winXP --nic1 nat --nictype1 Am79C973 --cableconnected1 on

La configuration physique étant terminé, nous pouvons finalement allumer la machine virtuelle:

    VBoxManage startvm winXP

Le reste ce passe exactement de la même manière que si vous utilisiez une vraie machine physique: installations du système d'exploitation, des programmes puis utilisation de ceux-ci.
L'avantage étant que si vous "cassez" le système invité, celà n'impact nullment le système hôte !
Il vous suffira de réinstaller le système invitée sur la machine virtuelle.

![Internet Explore sous Windows XP invitée via une machine virtuelle VirtualBox sous Ubuntu 16.04 LTS](https://raw.githubusercontent.com/vfo1409/virtualisation/master/Capture_du_2016-11-05_18-30-16.png "Internet Explore sous Windows XP invité via une machine virtuelle VirtualBox sous Ubuntu 16.04 LTS. Par Valentin Faria Oliveira - Travail personnel")

## Sources

Généralités informatiques:  
[https://fr.wikipedia.org/wiki/Noyau_de_syst%C3%A8me_d%27exploitation](https://fr.wikipedia.org/wiki/Noyau_de_syst%C3%A8me_d%27exploitation)  
[https://en.wikipedia.org/wiki/Orchestration_(computing)](https://en.wikipedia.org/wiki/Orchestration_(computing))

La virtualisation, ses différents types, et la virtualisation lourde:  
[https://fr.wikipedia.org/wiki/Virtualisation](https://fr.wikipedia.org/wiki/Virtualisation)    
[https://doc.ubuntu-fr.org/virtualisation](https://doc.ubuntu-fr.org/virtualisation)    
[http://www.culture-informatique.net/cest-quoi-la-virtualisation/](http://www.culture-informatique.net/cest-quoi-la-virtualisation/)    
[https://www.antoinebenkemoun.fr/2009/10/classification-des-types-de-virtualisation-mise-a-jour/](https://www.antoinebenkemoun.fr/2009/10/classification-des-types-de-virtualisation-mise-a-jour/)    
[https://www.antoinebenkemoun.fr/2009/07/les-differents-types-de-virtualisation-la-virtualisation-totale/](https://www.antoinebenkemoun.fr/2009/07/les-differents-types-de-virtualisation-la-virtualisation-totale/)

Les hyperviseur:  
[https://www.antoinebenkemoun.fr/2009/10/de-la-differenciation-hyperviseur-type-1-type-2/](https://https://www.antoinebenkemoun.fr/2009/10/de-la-differenciation-hyperviseur-type-1-type-2/)

La virtualisation matériellement assitée:  
[https://www.antoinebenkemoun.fr/2009/08/les-anneaux-de-protection/](https://www.antoinebenkemoun.fr/2009/08/les-anneaux-de-protection/)  
[https://www.antoinebenkemoun.fr/2009/08/les-anneaux-de-protection-systeme-dans-le-cas-du-64-bit/](https://www.antoinebenkemoun.fr/2009/08/les-anneaux-de-protection-systeme-dans-le-cas-du-64-bit/)  
[https://www.antoinebenkemoun.fr/2009/07/la-virtualisation-materiel-assistee/](https://www.antoinebenkemoun.fr/2009/07/la-virtualisation-materiel-assistee/)

Différence entre virtualisation et émulation:  
[http://carvounas.net/blog/2010/05/17/virtualisation-vs-emulation/](http://carvounas.net/blog/2010/05/17/virtualisation-vs-emulation/)  
[http://www.tomshardware.fr/articles/virtualisation-Intel-AMD,2-353-4.html](http://www.tomshardware.fr/articles/virtualisation-Intel-AMD,2-353-4.html)

La Colecovision, l'ADAM et l'Amiga:  
[http://www.grospixels.com/site/adam.php](http://www.grospixels.com/site/adam.php)  
[http://gamopat.com/article-1972191.html](http://gamopat.com/article-1972191.html)  
[https://en.wikipedia.org/wiki/Coleco_Adam](https://en.wikipedia.org/wiki/Coleco_Adam)  
[https://fr.wikipedia.org/wiki/Amiga](https://fr.wikipedia.org/wiki/Amiga)

L'IBM System/360, 370 et z:  
[https://fr.wikipedia.org/wiki/IBM_360_et_370](https://fr.wikipedia.org/wiki/IBM_360_et_370)  
[https://fr.wikipedia.org/wiki/ZSeries](https://fr.wikipedia.org/wiki/ZSeries)  
[https://fr.wikipedia.org/wiki/System_z](https://fr.wikipedia.org/wiki/System_z)

VirtualBox:  
[https://www.virtualbox.org/manual/](https://www.virtualbox.org/manual/ "particulièrement le chapitre 8 sur VBoxManage")
