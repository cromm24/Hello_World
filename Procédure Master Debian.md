![*](https://www.recia.fr/wp-content/uploads/2015/06/debian-logo-horizontal.gif)
___
# AT1C3: VM Master Debian 10 Clean et Classic 

## **Création d'une VM sous VMware workstation.**

2go de Ram / 12Go DD / Bridge  
Installation de Debian 10.7.0  
Nom : unique et pertinant, voué à être changé : changeme  
- Domaine : tssr.lan  
- Mdp : pas de stratégie car l'admin doit etre responsable. dadfba16  
- Utilisateur : login : user // mdp : dadfba16  
- Miroir sur le réseau : Sert à allez en ligne (sur un dépot débian.org) pour installer les différents paquets qui composeront notre serveur linux. Donc  OUI !  (proche de nous de préf donc France)  
- Séléctions des logiciels : décocher tout sauf les utilitaires usuels du système. Pas besoin du reste, on est des POWER USER ! On fait tout nous-même en ligne de commande.
- Grub : Oui il faut l'installer sinon notre linux sera comme les singes de la vertu.   

### **INSTALLATION MINIMALE PROPRE TERMINEE**
---
## **Admin linux en ligne de commande pour avoir une base line propre**  

Préparation du notre noyau linux en ligne de commande :  
* **bash** : bourne again shell // **nano** : not another notpad // **wget** : (windows obtenir : vas chercher sur le net pour le télécharger) // **su -** : permet de passer en super utilisateur mais permets de modifier les variables du path // **dpkg** : debian package // **wbmin** : web administration
* Vérifié le DNS en faisant un `ping 1.1.1.1` et ping google pour vérifier internet : `ping www.google.fr`    
* quelques explictions sur linux :  
`~` : dit qu'on est dans le répertoire maison  
`#` : indique qu'on est en power user  
- `nano .bashrc` : on l'édite et on retire les # avant: export, eval, alias ls, alias ll; alias l  

Mise à jour :  
- `apt update` : contact le dépot officiel et vérifie que le catalogue est à jour.  
- `apt ugrade` : met à jour les paquets.  

On se prépare un arsenal d'utilitaire pour être plus à l'aise :  
- `apt install ssh`

## **Sur le windows hôte :**   
Ouvrir un invite de commande  
Se connecter au serveur ssh de la VM avec la commande : ssh user@192.168.1.20 (en user hein =))   
passage en root : `su -`  

### Installation des applications : 

* ```apt install ``` : permet de vérifier si les services et port sont ouverts.  
* ```apt install zip``` : les formats les plus populaires sur linux sont bz bz2 et gz.  
* ```apt install lynx``` : permet d'avoir un navigateur en html interprété standard.  
* ```apt install curl``` : c'est le programme préféré de l'idôle de Thomas (fallait suivre) permet de tester des url.  
* ```apt install net-tools``` : des outils réseaux.   
* ```apt install dnsutils``` : pour les dns quoi.   
* ```apt install bsdgames``` : une suite de commande en ligne de commande.  
* ```apt install webmin``` : permet d'administrer un serveur linux par un navigateur web (pour les débutants). Allez sur le site de webmin copié le lien du dépot et faire dans le cmd : `wget http://prdownloads.sourceforge.net/webadmin/webmin_1.979_all.deb` . Ensuite on l'installe : `dpkg -i webmin_1.979_all.deb`. Une erreur appparait et c'est NORMAL. dpkg informe apt que 4 lib (blibliotheque) manquent. Donc on force l'installation via ```apt intall -f```. Pour se connecter  à webmin, dans un navigateur on tape notre ip sur le porrt 10000. On se retrouve sur une interface graphique pour gérer de notre serveur : POUR LES NOOBS !!!  
* ```apt install  winbins samba``` : samba permet de faire du partage à la windows // winbind (liaison à la windows) : grace a ce paquet notre serveur pour gérer des paquets à la windows.

### Modifications de fichier :

**`cd /etc :` tout se passe ici pour un admin !** 

`nano resol.conf :`
- domain tssr.lan   
- search tssr.lan  
- nameeserver 1.1.1.1  
nano nsswitch.conf (name service au sens global de résolution de nom; sert à être vu dans le partage windows et avoir accès au nom netbios) :  
- hosts: files dns wins (on ajoute wins à la fin)
cd /etc/network ; on édite le fichier interfaces  

`nano interfaces :` 
- iface ens33 inet static (remplace dhcp par static)
- address 192.168.1.86 (on rajoute cette ligne) ( OUBLIER PAS IL Y A 2 D, C'EST ADDRESSE EN ANGLAIS ! (et pas de "e" non plus))
- netmask 255.255.255.0
- gateway 192.168.1.254


# **_===> ON REBOOT ET C'EST YOUPI !!!_**
# **_MASTER PRET. ON Y TOUCHE PLUS_**
