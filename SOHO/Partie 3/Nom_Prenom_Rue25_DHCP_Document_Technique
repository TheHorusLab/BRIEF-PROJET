# Installation du DHCP  
  
### Étape 1 - Prérequis
  
- Avoir une vm Windows Server 2019 ou 2022 d'installé  
- Sur cette vm, avoir une 2ème carte réseau en mode réseau interne configuré en 10.10.5.10/24  
avec une adresse dns en 10.10.5.10 ou 127.0.0.0  
- Avoir le Firewall désactivé  

### Étape 2 - Installation et configuration des rôles   
  
##### Installation du rôle DHCP  
   
1 - Ouvrir une session avec Administrator  
2 - Dans le Server Manager, aller dans le menu Manage → Add Roles and Features → Next (3 fois)  
3 - Cocher "DHCP Server"  
4 - Cliquer sur "Add Features"  
5 - Cliquer sur Next (3 fois)  
6 - Cliquer sur "Intall"  
7 - Une fois l'installation terminé, cliquer sur "Close"  
8 - Toujours dans le Server Manager, Clic sur l'icone du drapeau en haut à droite (⚠️) → Complete DHCP Configuration  
9 - Cliquer sur "Commit", puis sur "Close"  

##### Configuration du rôle DHCP  

1 - Dans le Server Manager, aller dans le menu Tools → DHCP  
2 - Dans la nouvelle fenêtre qui s'affiche, faites dérouler le menu en cliquant sur l'icône ">" à gauche de votre serveur DHCP  
3 - Faites un clic droit sur "IPv4" puis sélectionner "New Scope..."  
4 - Cliquer sur Next, choisissez un nom, par exemple "Rue25", puis ajouter une description si vous le souhaitez, puis cliquer sur "Next"  
5 - Dans "Start IP address" mettre : 10.10.5.11 , dans "End IP address" mettre : 10.10.5.22 , dans "Length" mettre "24" ou dans "Subnet mask" mettre 255.255.255.0 puis faites "Next" jusqu'à la fin, puis cliquer sur "Finish"  
  
##### Installation du rôle DNS   
  
1 - Dans le Server Manager, aller dans le menu Manage → Add Roles and Features → Next (3 fois)  
2 - Cocher "DNS Server"  
3 - Cliquer sur "Add Features"  
4 - Cliquer sur Next (3 fois)  
5 - Cliquer sur "Intall"  
6 - Une fois l'installation terminé, cliquer sur "Close"  
  
##### Configuration du rôle DNS    
  
Créer une zone de recherche directe  
  
1 - Cliquez sur "Tools"  
2 - sélectionnez "DNS"  
3 – Dans la zone DNS à gauche, faites un clic droit sur le serveur  
4 - Sélectionnez "New Zone"  
5 - Faite "Next"  
6 - Sélectionnez "Primary Zone", puis cliquez sur "Next"  
7 - Sélectionnez "Forward lookup zone" , puis cliquez sur "Next"  
8 - Dans zone name, tapez "rue25.com", puis faites "Next"  
9 – Cocher la case « create a new file with this file name », laisser le nom proposé et cliquer sur « Next »  
10 - Sélectionnez "Do not allow dynamic updates", puis "Next"  
11 - Cliquez sur "Finish"  
  
Créer une zone de recherche inversée  
  
1 – Dans la zone DNS à gauche, faites un clic droit sur le serveur    
2 - Sélectionnez "New Zone"  
3 - Faites "Next"  
4 - Sélectionnez "Primary Zone", puis cliquez sur "Next"  
5 - Sélectionnez "Reverse lookup zone" , puis cliquez sur "Next"  
6 - Sélectionnez "IPv4 Reverse Loockup Zone", puis "Next"  
7 - Sélectionnez "Network ID" puis entrez les 3 premiers octets de l'adresse ip de votre réseau : 10.10.5, puis "Next"  
8 - Sélectionnez "Create a new file with this file name », laisser le nom proposé et cliquer sur « Next »  
9 - sélectionnez "Do not allow dynamic updates", puis "Next"  
10 - Cliquez sur "Finish"  
  
Configurer un forwarder pour les requêtes externes  
->Rester dans le DNS Manager  
1 - Faites un clic droit sur le server, sélectionner « Properties »  
2 – Aller dans l’onglet « Forwarders », puis cliquer sur « Edit »  
3 – Ajouter « 8.8.8.8 »  
  
##### Installation du rôle ADDS  


1 - Dans le Server Manager, aller dans le menu Manage → Add Roles and Features → Next (3 fois)  
2 - Cocher "Active Directory Domain Services"  
4 - Cliquer sur "Add Features"  
5 - Cliquer sur Next (3 fois)  
6 - Cliquer sur "Intall"  
7 - Une fois l'installation terminé, cliquer sur "Close"  


#### Configuration en Contrôleur de domaine  

1 - Toujours dans le Server Manager, Clic sur l'icone du drapeau en haut à droite (⚠️) → Promote this server to a domain controller  
2 - Sélectionner "Add a new forest"  
3 - Dans "Root domain name" mettre :  rue25.com  puis cliquer sur "Next"  
4 - Entrer un mot de passe pour le DSRM, puis cliquer sur "Next" (5 fois)  
5 - Attendre le Prerequisite check completed, puis cliquer sur "Install" et attendre que le serveur redémarre   
6 - Ouvrir la session Administrator  
   
#### Création d'une hiérarchie AD  
   
1 - Aller dans Server manager → Tools → Active Directory Users and Computers  
2 - Dans le menu View, sélectionner Advanced Features  
    (Le fait d'activer les Advanced Features ajoute des possibilités de paramétrage dans les différentes fenêtres de configuration AD.)  
3 - Dérouler l'arborescence sous le domaine rue25.com  
4 - Créer 3 OU sur la racine du domaine en faisant un clic-droit, puis "New" > "Organizational Unit"  
    - Rue_Utilisateurs  
    - Rue_Ordinateurs  
    - Rue_Securite  
5 - Dans l'OU "Rue_Utilisateurs", créer 4 OU en faisant un clic-droit, puis "New" > "Organizational Unit"  
    - Direction  
    - Immobilier  
    - Commercial  
    - Comptabilité  
Faire la même chose pour l'OU "Rue_Ordinateurs"  

#### Création des utilisateurs    
  
1 - Dans l’OU Rue_Utilisateurs > Direction, faites un clic-droit dans la fenêtre centrale, puis "New" > "User"   
2 - Dans "First Name" taper : Sylvie  ;  dans "Last name" taper : Bien ; dans "User logon name" taper : sylvie.bien  , puis cliquer sur "Next"  
3 - Dans "Password" Taper un mot de passe ; puis saisissez-le à nouveau dans "Confirm"  
4 - Décocher "User must change password at next logon"  et cocher "user cannot change password" et "Password never expires" , puis cliquer sur "Next", puis "Finish".  
5 - Faites la même chose pour "Lisa RAZOU"  
6 - Dans l’OU Rue_Utilisateurs > Immobilier , faites les mêmes manipulations pour ajouter les utilisateurs "Alain FIRMERIE" et "Mehdi TEZ"  
7 - Dans l’OU Rue_Utilisateurs > Commercial , faites les mêmes manipulations pour ajouter les utilisateurs "Jonathan LONGTEMPS" et "Paul DUNOR"  
8 - Dans l’OU Rue_Utilisateurs > Comptabilité , faites les mêmes manipulations pour ajouter les utilisateurs "Vincent TYME" et "Cyrène DEMER"  
9 - Dans l’OU Rue_Utilisateurs > Direction, faites un clic-droit dans la fenêtre centrale, puis "New" > "User"    
10 - Dans "First Name" taper : Samira  ;  dans "Last name" taper : Bien ; dans "User logon name" taper : samira.bien  , puis cliquer sur "Next"  
11 - Dans "Password" Taper un mot de passe ; puis saisissez-le à nouveau dans "Confirm"  
12 - Décocher "User must change password at next logon"  et cocher "user cannot change password" et "Password never expires" , puis cliquer sur "Next", puis "Finish".  
  
#### Création des groupes de sécurité  

1 - Dans l’OU Rue_Utilisateurs > Direction, faites un clic-droit dans la fenêtre centrale, puis "New" > "Group"    
2 - Dans "Group name" mettre "grp_direction"  
3 - Dans "Group scope", sélectionner "Domain local"  
4 - Dans "Group type", sélectionner "Security", puis valider avec "OK"  
5 - Double cliquer sur le groupe que vous venez de créer  
6 - Aller dans l'onglet "Members", puis cliquer sur "Add"  
7 - Dans "Enter the object name to select" entrer les personnes qui font parties de ce groupe, vérifier à chaque fois les noms en cliquant sur "Check Names" puis faites "OK" pour ajouter, ensuite cliquer sur "Apply"  
8 - Recommencer les étapes 1 à 7 pour les OU Immobilier, Commercial et Comptabilité  
9 - Dans l’OU Rue_Utilisateurs > Direction, faites un clic-droit dans la fenêtre centrale, puis "New" > "Group"   
10 - Dans "Group name" mettre "directrice"  
11 - Dans "Group scope", sélectionner "Domain local"  
12 - Dans "Group type", sélectionner "Security", puis valider avec "OK"  
13 - Double cliquer sur le groupe que vous venez de créer  
14 - Aller dans l'onglet "Members", puis cliquer sur "Add"  
15 - Dans "Enter the object name to select" entrer "Samira" vérifier le nom en cliquant sur "Check Names" puis faites "OK" pour ajouter, ensuite cliquer sur "Apply"  

#### Création des dossiers partagés  
 
1 - Aller à la racine du lecteur C:  
2 - Faites un clic-droit puis, sélectionner "New", puis "Folder", nommer ce dossier "Direction"  
3 - Faites un clic-droit sur ce dossier, puis sélectionner "Properties"  
4 - Aller sur l'onglet "Sharing"  
5 - Cliquer sur "Advanced sharing..."  
6 - Cocher la case " Share this folder"  
7 - Cliquer sur "Remove" pour enlever "Everyone"  
7 - Cliquer sur "Permissions"  
8 - Cliquer sur "Add"  
9 - Dans "Enter the object name to select" entrer le nom du groupe "grp_direction", vérifier le nom en cliquant sur "Check Names" puis faites "OK" pour ajouter 
10 - Cocher la case "Change" dans la colonne "Allow", puis cliquer sur "Apply" puis "OK"  
11 - Valider les modifications du partage avancée (Advanced Sharing") avec "Apply, puis "OK", puis fermer avec "Close"  
12 - Refaire les mêmes étapes en créant un dossier "Immobilier"  et rajouter en plus dans les permissions le groupe "Directrice" mais en ne sélectionnant que "Read" dans la colonne "Allow"  
13 - Refaire les mêmes étapes en créant un dossier "Commercial"  et rajouter en plus dans les permissions le groupe "Directrice" et "Comptabilité" mais en ne sélectionnant que "Read" dans la colonne "Allow"  
14 - Refaire les mêmes étapes en créant un dossier "Comptabilité"  et rajouter en plus dans les permissions le groupe "Directrice" mais en ne sélectionnant que "Read" dans la colonne "Allow"  
  
