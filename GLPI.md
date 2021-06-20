![*](https://glpi-project.org/wp-content/uploads/2017/03/logo-glpi-bleu-1.png "GLPI ")


_CLONE DU MASTER DEBIAN_

**Sur le windows hôte :**   
Ouvrir un invite de commande  
Se connecter au serveur ssh de la VM avec la commande : ssh user@192.168.1.86 (en user hein =))   
passage en root : `su -`  

1ère chose à faire : changer de nom  

> `nano /etc/hostname`  
> remplacer changeme par GLPI  

On ne peut pas faire `apt install glpi`, ca serait trop beau donc go le [site officiel](https://glpi-project.org/fr/) !  
Un peu de culture: existe depuis 2003, écrit en PHP, cela fait principalement de l'inventaire informatique, de la gestion de ticket, du helpdesk.    

---


# Installation de GLPI sous Linux

_Info + : Pour plus d’informations sur GLPI, consulter l’article Introduction au logiciel GLPI._  

1. On va installer les applications nécessaires, à savoir **apache2** pour les services web, **mariadb** pour la base de données et **php** pour le langage de programmation (notre serveur devient donc un serveur « **LAMP** »).
>`apt install apache2 php libapache2-mod-php mariadb-server -y`  

2. Ensuite, nous allons installer toutes les dépendances donc pourrait avoir besoin GLPI un jour ou l'autre.    
> `apt install php-mysqli php-mbstring php-curl php-gd php-simplexml php-intl php-ldap php-apcu php-xmlrpc php-cas php-zip php-bz2 php-ldap php-imap -y`

3. Voilà qui est fait. Nous allons maintenant sécuriser l’accès au service de base de données. Lancez la commande suivante :
> `mysql_secure_installation`

* Le mot de passe de l’utilisateur root est demandé. Il ne s’agit pas ici du mot de passe de l’utilisateur root sur la machine elle-même mais de l’utilisateur SQL (base de données). A ce stade, aucun mot de passe ne lui a été configuré, c’est donc ce que nous allons faire.   

![](https://neptunet.fr/wp-content/uploads/2020/11/glpi54.png)  
* Appuyer simplement sur **Entrée**.  
* A la question suivante, on vous demande si vous voulez attribuer un mot de passe au compte root. Tapez la lettre **Y** pour répondre Yes et appuyez sur **Entrée**.  
* Saisisser 2 fois le mot de passe que vous voulez donner au compte SQL root.  
* Répondre **Yes** à toutes les autres questions posées.  

4. Maintenant que l’accès aux bases de données est sécurisé, se connecter avec le compte root et le mot de passe que nous venons de lui définir :
> `mysql -u root -p`

5. Créer la base de données qui sera utilisée par GLPI et un utilisateur de base de données qui aura les pleins pouvoirs sur celle-ci. Voici les 3 commandes à saisir pour cela (**les ; sont nécessaires**) :

> `create database db_glpi;`  
* créer une base de données appelée « db_glpi » (on peut lui donner le nom que l'on veut)  
> `grant all privileges on db_glpi.* to admindb_glpi@localhost identified by "MDP";`  
* créer un utilisateur ici nommé « admindb_glpi », lui attribuer le mot de passe « MDP » et lui donner tous les privilèges (une sorte de « contrôle total » sur la base de données « db_glpi »). Une fois encore, à vous de définir les noms que vous souhaitez.  
> `exit`    

6. Avant se de lancer dans l’installation même de GLPI, une dernière manipulation facultative mais utile : sécuriser l’accès au répertoire qui va convenir GLPI sur la machine. On va en fait refuser l’indexation des fichiers de configuration de GLPI dans un navigateur web.  

* Pour cela, modifier le fichier de configuration du site web par défaut d’apache :  
> `nano /etc/apache2/sites-available/000-default.conf`  

* Sous la ligne « DocumentRoot », ajouter les lignes suivantes en respectant l’indentation :  
> <Directory /var/www/html>
> Options Indexes FollowSymLinks
> AllowOverride All
> Require all granted
> </Directory>

Vous devriez avoir un fichier qui ressemblera à ceci :  

![*](https://neptunet.fr/wp-content/uploads/2020/11/glpi58.png)  


_Info + : Le paramètre « Directory » correspond au futur emplacement de stockage de GLPI sur ma machine, à adapter à votre configuration bien entendu. Il n’est pas rare de placer GLPI dans un dossier à part entière plutôt qu’à la racine du service web, dans ce cas il sera dans /var/www/html/glpi, il faudra donc mettre ce fichier à jour. L’URL du site deviendra http://adresse_ip/glpi_

7. Pour appliquer toutes les modifications, il reste à redémarrer le service apache :
> `service apache2 restart`

## **Nous allons ENFIN pouvoir installer GLPI !**

8. Placer vous dans un répertoire temporaire et téléchargez la dernière version disponible de GLPI sur Github :  
> `cd /tmp`  
> `wget https://github.com/glpi-project/glpi/releases/download/9.5.5/glpi-9.5.5.tgz`  

9. Décompresser l’archive de GLPI :  
> `tar -xvzf glpi-9.5.5.tgz`  

10. Copier le contenu du dossier décompressé nommé « glpi » dans `/var/www/html`.  

* copier des fichiers dits “cachés” dont le nom commence par un “.”  
> `shopt -s dotglob` 

* supprimer le fichier index.html qui est la page d’accueil par défaut d’Apache  
> `rm /var/www/html/index.html`  

* copier le tout dans la destination  
> `cp -r glpi/* /var/www/html/`  

11. Rendre l’utilisateur des services web (nommé www-data) propriétaire de ces nouveaux fichiers :
> `chown -R www-data /var/www/html`  

## Les fichiers pour GLPI sont prêts   

Poursuivre l'installation via une interface web. Si votre machine hôte possède une interface graphique avec un navigateur internet, rendez-vous à l’URL suivante :  

> `http://localhost`  

Ou via son adresse ip  

> `http://ip_de_votre_machine_glpi`  

_Info + : Si vous tombez sur une page web d’Apache par défaut, vous pouvez supprimer le fichier index.html avec la commande suivante et rafraichir la page, vous sera alors bien sur le setup d’installation de GLPI : `rm /var/www/html/index.html`(ou alors mettre un beau chat en ascii dedans, ca pourrait faire plaisir à quelqu'un ...)_

**Page du setup de GLPI.**    

* Sélectionner le Français dans la liste déroulante et cliquez sur OK. 

![*](https://neptunet.fr/wp-content/uploads/2020/11/glpi61.png)  

* Accepter les conditions d’utilisation.  

* Cliquer sur le bouton **Installer** pour lancer le setup.  

* Une série de test sera lancée par le setup pour s’assurer que tous les prérequis nécessaires au bon fonctionnement de GLPI sont remplis. Si vous avez correctement suivi ce tuto, il ne devrait y avoir que des coches vertes. Cliquer sur **Continuer**.  

![*](https://neptunet.fr/wp-content/uploads/2020/11/glpi64.png)  


* Saisir les informations sur la base de données destinées à GLPI que nous avons précédemment créée. Saisir **localhost** pour spécifier que la machine actuelle héberge à la fois le site web de GLPI et la base de données (si la base de données est stockée sur une autre machine, saisissez son adresse IP). Rentrer ensuite le **nom de l’utilisateur** qui a tous les privilèges sur cette base de données et son **mot de passe**.  

![*](https://neptunet.fr/wp-content/uploads/2020/11/glpi65.png)  

* Sélectionner ensuite la base de données créée spécialement pour **GLPI**.  

![*](https://neptunet.fr/wp-content/uploads/2020/11/glpi66.png)  

* Le setup va contacter la base de données pour s’assurer que tout est OK. Cliquer sur **continuer**.  

* Choisir d’envoyer ou pas des statistiques sur votre utilisation de GLPI à l’équipe qui gère le projet et cliquer sur **continuer**.  

* Une version commerciale de GLPI avec un service support dédié existe. Faire un don est possible. Cliquer sur **continuer**.  

## **L’installation est terminée.** 

### Encore quelques réglages

* Cliquer sur **Utiliser GLPI**.  
* Se connecter avec les identifiants par défaut d’un compte administrateur.  
* Un message d’avertissement vous informe que par **sécurité** il faudra changer les mots de passe par défaut des 4 utilisateurs créés automatiquement et supprimer le fichier « install.php ».  

![*](https://neptunet.fr/wp-content/uploads/2020/11/glpi73.png)  

* Cliquer sur le nom d'un utilisateur. Attribuer lui un nouveau mot de passe. **Répêter l'opération pour tout les utilisateurs**.  
* Supprimer le fichier install.php :  
> `rm /var/www/html/install/install.php`  

_Info + : La localisation du fichier install.php dépend de l’emplacement où se trouvent les fichiers de GLPI sur la machine._  

# ___TOUT EST FINIS, GL & HF !___

_Source : [https://neptunet.fr/install-glpi/](https://neptunet.fr/install-glpi/)_  