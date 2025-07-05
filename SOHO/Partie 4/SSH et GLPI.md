### Installation SSH  
  
commande : apt-get install ssh  
  
### Vérifier l'état du service SSH  
  
commande : systemctl status ssh  
  
### modification du fichier de configuration sshd    
  
commande : nano /etc/ssh/sshd_config  
  
Mettre les valeurs suivantes (en supprimant le symbole # au début des lignes pour activer la valeur):  
  
-> Port 22  
-> PermitRootlogin no  
-> PubkeyAuthentication yes  
-> PasswordAuthentication yes (pour un lab on peut laisser sur "yes" pour des test mais dans les bonnes pratiques on mettrais la valeur "no" pour interdire la connexion par mot de passe)  
-> X11Forwarding yes (permet une connexion sur la Debian à partir d'une interface graphique d'une autre machine en GUI, dans les bonnes pratiques elle serait à désactiver, donc à mettre en "no" )  
-> AllowUsers wilder25 (permet que seul l'utilisateur wilder25 puisse se connecter en SSH)  
  
### Limitation du nombre de connexion avec le compte wilder25    
1 - nano /etc/security/limits.conf  
2 - ajouter : wilder hard maxlogins 1  
3 - systemctl restart sshd  
  
#### Installation des dépendances de GLPI  
   
Installation du serveur web apache2  
   
1 - Taper la commande : apt-get install apache2  (à la question : Do you want to continue? valider avec "Y")  
2 - Vérifier l'état du serveur web en tapant la commande : systemctl status apache2  (Active doit être : active (running) en vert)  
3 - Taper la commande : apt-get install curl  (permet de récupérer le contenu de la page web pour savoir si elle existe vraiment)    
4 - Taper la commande : curl http://adresse_ip_de_la_debian (pour connaître l'adresse ip de votre debian, taper la commande : ip a)  
  
Installation des dépendances pour PHP  
  
1 - Taper la commande : apt-get install ca-certificates apt-transport-https software-properties-common wget curl lsb-release (à la question : Do you want to continue? valider avec "Y")  
2 - Ajouter le dépôt pour PHP8.3 avec la commande : curl -sSL https://packages.sury.org/php/README.txt | bash -x  
3 - Actualiser les paquets disponibles avec la commande : apt-get update  
  
Installation de PHP  
  
1 - Taper la commande : apt-get install php8.3  
2 - Taper la commande : apt-get install libapache2-mod-php8.3  
3 - Redémarrer Apache : systemctl restart apache2  
4 - Vérifier que php est bien installé avec la commande : php -v  
5 - Installer les extensions pour glpi en tapant la commande :  
apt-get install php8.3-common php8.3-curl php8.3-bcmath php8.3-intl php8.3-mbstring php8.3-xmlrpc php8.3-mcrypt php8.3-mysql php8.3-gd php8.3-xml php8.3-cli php8.3-zip php8.3-ldap php8.3-Phar php8.3-zip php8.3-bz2  
6 - Activer le module php avec la commande : a2enmod php8.3  
7 - Redémarrer le serveur web avec la commande : systemctl restart apache2  
  
Installer la base de donnée MariaDB  
  
1 - Taper la commande : apt-get install mariadb-server -y  
2 - Lancer le processus d'initialisation de la base de données avec la commande : mysql_secure_installation  

A la question : Switch to unix_socket authentication : Répondre "n"  
A la question : Change the root password : Répondre "y" et mettre un mot de passe et le confirmer en le mettant de nouveau (Attention : retenez bien ce mot de passe car il vous sera demandé plus tard dans l'installation  )   
A la question : Remove anonymous users : Répondre "n"  
A la question : Disable root login remotely : Répondre "n"  
A la question : Remove test database and access to it : Répondre "y"  
A la question : Reload privilege tables now : Répondre "y"  

Connexion à la base de donnée  
  
1 - Tape la commande : mysql -u root -p  
2 - Entrer le mot de passe que vous avez fourni précédemment   
3 - On va configurer ceci :  
  
Un nom de base de données : glpidb  
un compte avec des droits d'accès élevé : glpi (il faudra choisir un mot de passe)  
Cela va se faire avec les commandes ci-dessous :  

CREATE DATABASE glpidb CHARACTER SET utf8 COLLATE utf8_bin;  
grant all privileges on glpidb.* to glpi@localhost identified by 'entrerunmotdepasse';  
flush privileges;  
quit  
  
Récupération des sources Glpi  
  
1 - Taper la commande : wget https://github.com/glpi-project/glpi/releases/download/10.0.18/glpi-10.0.18.tgz  

2 - Décompresser le fichier et l'amener à côté du fichier html avec la commande : tar -xzvf glpi-10.0.18.tgz -C /var/www/  
3 - Modifier la configuration d'apache pour qu'il pointe vers le bon fichier avec la commande : nano /etc/apache2/sites-enabled/000-default.conf  
Dans l'édition, modifier le "documentroot" avec : /var/www  
Enregistrer avec Ctrl+O, puis quitter avec Ctrl+X  
4 - Recharger Apache2 pour qu'il prenne en compte les modifications avec : systemctl reload apache2  
  
Configuration de PHP  
  
1 - Taper la commande : nano /etc/php/8.3/cli/php.ini  
2 - Modifier les valeurs suivantes:  
    memory_limit = 128M  
    max_execution_time = 600  
3 - changer les droits du fichier glpi avec la commande : chown -R www-data:www-data /var/www/glpi/  
  
### Installer GLPI  
  
1 - Sur un navigateur web, taper la commande : http://adresse IP de la machine glpi/glpi  
2 - Vous arrivez sur le menu GLPI SETUP, sélectionner la langue "Français", puis cliquer sur "OK"  
3 - Cliquer sur "Continuer"  
4 - Cliquer sur "Installer"  
5 - Cliquer sur "Continuer"  
6 - A l'étape 1:  
    dans "Serveur SQL" , taper "localhost"  
    dans "Utilisateur SQL", taper "glpi"  
    dans "Mot de passe SQL", taper votre mot de passe que vous avez défini lors de l'installation de la base de donné MariaDB.  Vous savez le moment où je vous ai dit de retenir votre mot de passe ca ril vous sera demandé un moment...et bien ce moment c'est maintenant ^^  Enfin cliquer sur "Continuer".  
7 - A l'étape 2:  
    Sélectionner votre base de donnée "glpidb"  et cliquier sur "Continuer"  
8 - A l'étape 3:  
    Lorsque la base a bien été initialisée, cliquer sur "Continuer"  
9 - A l'étape 4:  
    Décocher la case : Envoyer "statistiques d'usage"  pûis cliquer sur "Continuer"  
10 - A l'étape 5: 
    Cliquer sur "Continuer"  
11 - A l'étape 6:  
    L'installation est terminé, les idenfitiants et mots de passe par défaut vous sont donnés pour vous connecté suivant vos besoins. Cliquer sur "Utiliser GLPI"  
12 - Dans "Identifiant", taper "glpi"  
    Dans "Mot de passe", taper "glpi" , puis cliquer sur "Se connecter"  
13 - Vous arriver sur le tableau de bord et un message s'affiche pour vous dire de changer le mot de passe pour plus de sécurité (ce qui est fortement conseillé)  
