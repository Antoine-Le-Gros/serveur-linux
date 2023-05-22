# Enzo ROLIN <code>roli0007</code> et Antoine LE GROS <code>le-g0067</code>

## Configurations Réalisées

### Gestion des services : systemd

Tout d'abord, nous avons lu le manuel de la commande systemclt avec la commande:  
<code>man systemctl</code>
![images](Images/systemd/1.man.png)

Nous affichons la liste des services démarrés avec la commande <code>systemctl </code> :
![images](Images/systemd/2.commande_systemctl_rendu.png)

Nous installons le paquet sshd avec la commande   
<code>sudo apt install openssh-server</code>
![images](Images/systemd/3.apt_install_openssh-server.png)

On verifie ensuite que le sshd est démarré en tapant la commande <code>ssh 10.31.11.146</code>
![images](Images/systemd/4.ssh_ip.png)

Nous tapons la commande <code>exit</code>
![images](Images/systemd/5.exitssh.png)

Nous stoppons le service sshd et tentons de nous reconnecter en tapant ces deux commandes <code>systemctl stop sshd  - ssh 10.31.11.146</code>
![images](Images/systemd/6.systemctlstop.png)
Nous constatons donc qu'apres avoir stoppé le service sshd nous ne pouvons pas nous reconnecter.

On redemarre ensuite le service sshd en tapant la commande <code>systemctl restart sshd</code>
![images](Images/systemd/7.restartsshd.png) 
Nous constatons que nous pouvons desormais nous reconnecter apres avoir redemarré le service sshd.

### 2. Serveur Web apache2

Nous installons le paquet apache2  
<code>sudo apt instal apache2</code>
![images](Images/apache2/1.aptinstallapache2.png)

Nous demarrons le service apache2 a l'aide de la comande <code>systemctl start apache2</code>

Cela fonctionne car nous avons accès a cette page web dont l'url est http://10.31.11.146 :  
![images](Images/apache2/3.testnavigateur.png)

Explorons le contenu du repertoire /etc/apache2 grace a la commande <code>ls /etc/apache2</code>
![images](Images/apache2/4.lsetcapache.png)

Nous lisons le fonctionnement du fichier apache2.conf :
![images](Images/apache2/5.nanoapache2.png)

Nous lisons le manuel de a2enmod : 
![images](Images/apache2/6.man_a2enmod.png)

Nous activons les pages d'acceuil des utilisateurs et nous redemarrons le service apache2 pour recharger la configuration du serveur WEB a l'aide des commandes <code>a2enmod userdir</code> et <code>systemctl restart apache2</code>:
![images](Images/apache2/7-8.userdir-restart.png)

Nous créons le repertoire public_html dans le repertoire d'acceuil <code>sudo mkdir public_html</code>

On donne les droits d’accès à l’utilisateur du serveur Web www-data pour lui permettre l'accès à votre répertoire d’accueil ainsi qu'au répertoire public_html a l'aide des commandes :
![images](Images/apache2/10.setfaclm.png)

Nous affectons les droits par défaut correspondant au répertoire public_html a l'aide de la commande  
<code>sudo setfacl -b public_html</code>
![images](Images/apache2/11.setfacldefaut.png)

Nous verifions le bon fonctionnement 
![images](Images/apache2/12.bonfonctionnement.png)

Nous allons desactiver le systeme de listage dans le fichier userdir.conf en supprimant l'option Indexes en utilisant la commande <code>sudo nano userdir.conf</code>  
![images](Images/apache2/13.indexes.png)

En effet nous n'avons plus accès au listage : 
![images](Images/apache2/14.fonctionneplus.png)

Nous créons un fichier bienvenue.html dans ce répertoire, contenant la phrase "Bienvenue sur votre site perso" avec la commande <code>sudo nano bienvenue.html</code>
![images](Images/apache2/15.bienvenuehtml.png)

Nous verifions le bon fonctionnement de votre serveur en accédant à la page http://10.31.11.146/~iut/bienvenue.html 
et cela fonctionne car on peut voir le bienvenue.html:
![images](Images/apache2/16.testbienvenue.png)

### Configuration d'un serveur Web virtuel

Tout d'abord, nous tapons la commande   
<code>nslookup 10.31.11.146</code>
![images](Images/apache2/17.nslookup.png)

Nous creons ensuite un repertoire mon_serveur dans le repertoire d'acceuil
grace a la commande <code>sudo mkdir mon_serveur</code>

Nous donnons les droits d'accés a ce repertoire pour l'utilisateur www-data
![images](Images/apache2/19.autorisationmonserveur.png)

Nous creons ensuite un lien symbolique avec la commande :  
![images](Images/apache2/20.liensymbolique.png)

Nous creons ensuite index.html dans le repertoire mon_serveur avec la commande :  
<code>sudo nano mon_serveur/index.html</code>

Nous nous deplacons dans le répertoire /etc/apache2/sites-available avec la commande :  
<code>cd /etc/apache2/sites-available</code>

Nous copions le fichier 000-default.conf en 2A4V2-31UVM146.conf avec la commande :  
<code>sudo cp 000-default.conf 2A4V2-31UVM146.conf</code>

Nous editons le fichier 2A4V2-31UVM146.conf de facon demandé : 
![images](Images/apache2/24.nano000-default.png)

Nous activons le site et redemarons la configuration apache avec ces deux commandes  
<code>sudo a2ensite 2A4V2-31UVM146 -  
systemctl reload apache2</code>


Nous verifions l'accès à l'URL http://2A4V2-31UVM146.ad-urca.univ-reims.fr :
![images](Images/apache2/27.resultatbon.png)

### 3. Serveur Web sécurisé https

Nous verifions  la présence des paquets nécessaires : libssl, openssl et ssl-cert
avec la commande <code>dpkg -l | grep ssl</code>
![images](Images/serveurweb/1.dpkggrep.png)

Nous creons le certificat en tapant les commandes <code>mkdir /etc/apache2/ssl   et  
/usr/sbin/make-ssl-cert /usr/share/ssl-cert/ssleay.cnf /etc/apache2/ssl/apache.pem</code>
![images](Images/serveurweb/2.certif.png)
![images](Images/serveurweb/2.certif2.png)

Nous créons le fichier de configuration du VirtualHost de votre serveur ssl en faisant une copie du fichier default-ssl.conf du répertoire /etc/apache2/sites-available en tapant la commande :
<code>cp default-ssl.conf mon-serveur-ssl.conf</code>
![images](Images/serveurweb/3.copiedefaultssl.png)


Nous editons ce fichier pour ajouter ou modifier les lignes demandées grace a la commande <code>sudo nano mon-serveur-ssl.conf</code>:
![images](Images/serveurweb/4.editionmonserveurssl.png)

Nous tapons ces trois commandes a la suite afion d'activer le module ssl, d'activer le site et redemarrer le service apache  
<code>sudo a2enmod ssl</code>  
<code>sudo a2ensite mon-serveur-ssl.conf</code>  
<code>systemctl restart apache2</code>

Nous testons voir si le site fonctionne avec l'URL : https://2a4f1-31upca226.ad-urca.univ-reims.fr
![images](Images/serveurweb/8.sitefonctionne.png)

### 4. Langage de programmation PHP

Nous installons les paquets php et libapache2-mod-php avec les commandes : 
<code>sudo apt install php - sudo apt install libapache2-mod-php</code>

Nous editons le fichier /etc/apache2/mods-enabled/php7.4.conf et commentons les 5 dernières lignes avec la commande  
<code>sudo nano /etc/apache2/mods-enabled/php7.4.conf</code>
![images](Images/php/2.commenterphp.png)

Nous redemarrons le service apache 2 avec la commande <code>systemctl restart apache2</code>
![images](Images/php/3.restartapache.png)

Nous créons un fichier index.php dans notre répertoire public_html contenant la ligne suivante : <?php phpinfo();
<code>cd public_html – sudo nano index.php</code>
![images](Images/php/4.indexphp.png)

Nous verifions le bon fonctionnement de la page dans le navigateur
![images](Images/php/5.resultatphp.png)

### 5. Serveur de base de données MySQL

On installe le paquet mysql-server avec la commande <code>sudo apt install mysql-server</code>
![images](Images/mysql/1.installmysql.png)

Nous configurons la sécurité de  la manière suivante :
![images](Images/mysql/2.mysqlinstallation.png)
![images](Images/mysql/2.2.alldone.png)
![images](Images/mysql/2.3.mysqlroot.png)
(voir Problèmes rencontrés)

Nous nous connectons en tant que root :
<code>sudo mysql</code>

Nous creons un utilisateur avec un mot de passe qui est 'Azerty01' avec la commande :  
<code>CREATE USER 'admin'@'localhost' IDENTIFIED BY ‘Azerty01’;</code>
![images](Images/mysql/4.creationadmin.png)

Nous donnons les privilèges a cet utilisateur avec la commande <code>GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;</code>
![images](Images/mysql/5.privileges.png)

On quitte mysql avec la commande : 
<code>exit</code>

Nous redemarrons le service mysql avec la commande <code>systemctl restart mysql</code>
![images](Images/mysql/7.restartsystemctl.png)

Nous testons le serveur avec la commande   
<code>mysqlshow -u admin -p</code>  
![images](Images/mysql/8.testserv.png)

### 6. Outil d'administration de bases de données phpMyAdmin

On installe le paquet phpmyadmin avec la commande : <code>sudo apt install phpmyadmin</code>  
![images](Images/phpmyadmin/installphpmyadmin.png)
Puis, nous répondons aux questions.

Nous sélectionnons apache2 et nous selectionnons **"Oui"** pour l'utilisation de dbconfig-common.
![images](Images/phpmyadmin/ouiutilisationdbconfig-common.png)

On selectionne comme mot de passe **Azerty01** pour l'utilisateur admin

On active le module de gestion des chaînes de caractères multi-octets de php avec la commande : <code>sudo phpenmod mbstring</code>
![images](Images/phpmyadmin/multi-octet.png)

On redémarre ensuite le service apache2.  
![images](Images/phpmyadmin/redemapache2.png)

Nous testons phpmyadmin en nous connectant à http://10.31.11.226/phpmyadmin.  
![images](Images/phpmyadmin/connexionphpmyadmin.png)  
![images](Images/phpmyadmin/connexionreussiphpmyadmin.png)

Nous créons un utilisateur mysqltest en cochant l'option "Créer une base portant son nom" et donnez à cet utilisateur tous les privilèges sur cette base.  
![images](Images/phpmyadmin/cr%C3%A9ationutilisateurmysqltest.PNG)  
![images](Images/phpmyadmin/privilegesbdmysqltest.PNG)

Nous testons la connexion à phpMyMdmin avec cet utilisateur.  
![images](Images/phpmyadmin/connexionmysqltest.png)

### 7. Installation du CMS WordPress

On utilise la commande wget pour télécharger l’archive WordPress dans le répertoire personnel de l'utilisateur iut :
<code>wget https://fr.wordpress.org/latest-fr_FR.zip</code>  
![images](Images/CMS/installwordpress.png)
![images](Images/CMS/installationwordpress.png)

On installe le CMS dans le répertoire /home/iut/mon_serveur tout en suivant la procédure détaillée sur https://fr.wordpress.org/support/article/how-to-install-wordpress/.  
![images](Images/CMS/envoieduzipdansmonserveur.png)  
![images](Images/CMS/dezipage.png)  
![images](Images/CMS/dossierwordpressmonserveur.png)

On termine l'installation.  
Tout d'abord, on créer une base de données et un utilisateur.  
![images](Images/CMS/creationbd.png)  
![images](Images/CMS/creationutilisateur.PNG)

Ensuite, on configure le fichier wp-config.php. 
Pour cela, nous allons sur le site https://2a4f1-31upca226.ad-urca.univ-reims.fr/wordpress/wp-admin/setup-config.php  
![images](Images/CMS/passwordusernameetc.png)  
Où l'on remplie les informations avec celles sur phpmyadmin.
Après, on obtient un code à reproduire
![images](Images/CMS/reglewpconfig.png)  
On renomme le fichier wp-config-sample.php en wp-config.php et on l'édite la configuration
![images](Images/CMS/aremplacer.png)  
![images](Images/CMS/areproduire.png)

A la fin, nous validons l'installation et on remplit les champs du formulaire.
![images](Images/CMS/titredusiteidentifiant.png)  
Et juste après, nous arrivons sur le site https://2a4f1-31upca226.ad-urca.univ-reims.fr/wordpress/wp-admin.
![images](Images/CMS/bienvenuesurwordpress.png)

Nous vérifions le bon fonctionnement en accédant à l'URL http://2a4f1-31upca226.ad-urca.univ-reims.fr/.
![images](Images/CMS/bonfonctionnementfin.png)

## Problèmes rencontrés

>Nous avons rencontré peu de problème au cours de cette SAÉ mais nous allons vous citer les principaux :
### Création d'une nouvelle machine virtuelle : 

- Au cours de l'étape 2 (Serveur Web apache2) dans la partie "Configuration d'un serveur Web virtuel", nous avons crée une nouvelle machine virtuelle car le DNS ne voulait plus fonctionner apres un redemarrage. Suite a la création de la nouvelle et avoir retapé toutes les commandes précédentes, la machine virtuelle fonctionnait parfaitement et le DNS aussi.

### Erreur au niveau du <code>sudo mysql_secure_installation</code> :
- En effet, après avoir tapé la commande ci dessus, le terminal provoqué une erreur au niveau du mot de passe, il refusait peu importe le mot de passe entré et il etait pour nous impossible de sortir de l'interface ou il fallait tapé le mot de passe. Après plusieurs recherche nous avons trouvé la commande :  
<code>ALTER USER ‘root’@’localhost’ IDENTIFIED WITH mysql_native_password BY ‘Azerty01’; </code>
Suite a cette commande, nous avons retapé la commande :  
<code>sudo mysql_secure_installation</code>  
Et cela a fonctionné car il n'y avait plus l'erreur.
