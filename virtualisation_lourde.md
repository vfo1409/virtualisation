La virtualisation lourde
========================

## Rappel sur la virtualisation

La virtualisation du point de vue machine, est le fait de faire fonctionner sur une machine physique, une ou plusieurs machines virtuelle.

Ces machines peuvent être virtualisé à différents niveau:

* virtualisation d'un ordinateur complet avec tout son matériel (processeur, RAM, espace de stockage, etc...).  Cette solution est la plus compatible si l'on souhaite utiliser un système d'exploitation qui n'a pas été prévu spécifiquement pour cette utilisation car faisant abstraction du matériel physique de la machine hôte.  Cependant, elle est aussi la plus lente et gourmande en terme de ressources de par l'émulation d'un matériel quasi-complet via un hyperviseur de type 2, d'où sa dénomination de virtualisation "lourde".

* virtualisation d'un système d'exploitation prévu à cet effet qui utilise directement les ressources matérielles offertes par la machine hôte à l'aide d'un hyperviseur de type 1.
Cette solution est la plus performante car les systèmes d'exploitation sont optimisé pour cette utilisation,  mais en contrapartie aussi la plus contraignants et onéreuse, particulièrement si l'on utilise des logicielle propriétaire.
On désigne cette virtualisation de para-virtualisation.

* virtualisation d'un environnement système (un noyeau) directement dans l'espace de l'utilisateur comme n'importe quel autre logiciel.
Ce nouvel environnement dispose ensuite de son propre espace utilisateur et gère ses applications comme s'il était le noyau légitime de la machine.
Cette solution est la moins performante de par la présence de deux noyau sur la même machine, le manque d'isolation entre eux et par conséquent la dépendance du noyau émulé par rapport au noyau hôte.

* virtualisation d'un ensemble d'application via un isolateur.
Bien que l'on ne parle pas de virtualisation d'un système dans ce dernier cas, cette solution propre à Linux permet de faire fonctionner un lots d'applications, voir même plusieurs instance de la même application, sans avoir à se préoccuper du matériel de la machine hôte.
Celà est très performant mais l'isolation n'étant pas complète, cette solution est totalement dépendante du noyau hôte.

Ainsi dans le cadre de la virtualisation lourde, une machine vituelle est un ordinateur quasi-complet disposant d'un processeur, d'une RAM, un ou plusieurs disques durs, une ou plusieurs interface réseau et plus ou moins de périphériques externes (lecteur disquette, CD-ROM, contrôleur USB, etc...).
Cette ordinateur est émulé par un hyperviseur de type 2 qui se charge de faire le lien entre la machine virtuelle et la machine hôte.  Lors de la création de la machine virtuelle, l'utilisateur se retrouve tout simplement avec une machine dont le disque dur est vide. Il lui imcombe d'installer le système d'exploitation de son choix, comme s'il venait réellemment de se procurer un ordinateur non-formaté.
