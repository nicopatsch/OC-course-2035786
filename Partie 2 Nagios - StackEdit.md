## Partie 2 Installez votre outil de supervision

### Chapitre 2.1. Installez Nagios Core sur votre serveur Debian

Maintenant que vous avez connaissance des problématiques de supervision, des différents acteurs sur le marché ainsi que des avantages et inconvénients de la solution Nagios, je vous retrouve donc dans ce chapitre pour installer Nagios Core sur un système GNU/Linux depuis les sources. Ce processus peut se détailler en quatre grandes étapes :

- la **préparation du serveur**, qui va héberger le service Nagios ;

- le **téléchargement et la compilation des sources** Nagios ;

- l’installation de **l’arborescence** Nagios ;

- la vérification de la connexion à **l’interface d’administration** de Nagios.

#### Préparez votre serveur Debian à recevoir Nagios

La machine cible est une **Debian Stretch 9.6.0** en version minimale, disponible au téléchargement directement sur le site de Debian.Voici, par exemple, pour une architecture amd64 [disponible ici](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.6.0-amd64-netinst.iso).

**[SCREENSHOT : 2.1-1_ImageMinimaleDebian]**

Pendant le processus d’installation, je vous propose dans un premier temps de ne sélectionner que le minimum requis, à savoir :

- le **serveur SSH**, pour se connecter via un terminal distant ;

- les **composants usuels** du système, qui contiennent les librairies C les plus communément employées.

**[SCREENSHOT : 2.1-2_InstallMinimaleDebian ]**

Quelques packages supplémentaires sont nécessaires afin de profiter pleinement de toutes les capacités de Nagios.

Vous allez donc installer quelques dépendances via diverses commandes que je vous présenterai par la suite.

##### Installez le serveur Apache

Pour accéder à l’interface Web de gestion de Nagios, vous avez besoin d’un serveur Apache et de l’interpréteur PHP.

```shell

apt-get install apache2 php php-gd php-imap php-curl php-mcrypt

```
Cette commande va installer les packages suivants : //TODO ajouter logs 1

##### Installez les librairies Perl
Pour exploiter au maximum les possibilités offertes par les plugins de Nagios, vous avez besoin de quelques librairies Perl supplémentaires :

```shell
apt-get install libxml-libxml-perl libnet-snmp-perl libperl-dev libnumber-format-perl libconfig-inifiles-perl libdatetime-perl libnet-dns-perl
```
Vous pourrez noter par exemple les librairies perl SNMP. En effet sans l'inclusion de ces librairies, le plugin SNMP de Nagios ne pourra pas fonctionner !

Cette commande va installer les packages suivants : //TODO ajouter Logs 2

##### Installez les librairies graphiques
Nagios nécessite également quelques librairies graphiques :

```shell
apt-get install libpng-dev libjpeg-dev libgd-dev
```

Cette commande va installer les packages suivants : //TODO ajouter logs 3

##### Installez les outils de compilation standards
Enfin pour installer Nagios et ses plugins vous aurez besoin des outils de compilation standards et du package unzip :

En effet, Nagios est un programme compilé en C, donc le compilateur GCC et ses outils associés MAKE et AUTOCONF sont indispensables, et les sources sont téléchargées souvent dans un format compressé, donc UNZIP est nécessaire également.

```shell
apt-get install gcc make autoconf libc6 unzip
```

Cette commande va installer les packages suivants : //TODO ajouter logs 4


##### Créez l'environnement Nagios

Il vous reste juste quelques ajouts d’ordre administratif à effectuer… et notamment, la **création de l’environnement Nagios**, avec son utilisateur, son groupe et son répertoire de travail.
En effet, Nagios est un programme qui n'a pas besoin de tourner sous root.
Pour ajouter l’utilisateur **`nagios`** sur le système, saisissez la commande suivante :

```shell
useradd -m -p $(openssl passwd nagios) nagios
```

Cette commande crée l’utilisateur `nagios` sur le système et initie son mot de passe à `nagios`. Bien entendu, il vous faudra renforcer ce mot de passe sur une machine de production.

>**:information_source:** Par ailleurs, il est également nécessaire de partager un groupe commun entre l’utilisateur `nagios` et l’utilisateur `www-data` (qui fait tourner le **serveur Web apache2**) afin, notamment, de permettre l’administration de Nagios depuis l’interface Web. Je reviendrai concrètement sur cette notion un peu plus loin dans le cours.
Pour l’instant, saisissez les commandes suivantes :

```shell
groupadd nagcmd
usermod -a -G nagcmd nagios
usermod -a -G nagcmd www-data
```

1. La première commande crée un groupe `nagcmd`, qui servira à **exécuter des commandes Nagios depuis l’interface Web**.
2. La seconde commande ajoute l’utilisateur `nagios` dans le groupe `nagcmd`.
3. La troisième commande ajoute l’utilisateur `www-data`, sous lequel tournent les processus du serveur Web Apache2 dans le groupe `nagcmd`.
Dernière commande à passer, la **création d’un répertoire de stockage pour tous les téléchargements** :

```shell
mkdir /home/nagios/downloads
```
Voilà, le serveur est maintenant prêt à recevoir Nagios et ses composants.

#### Téléchargez et compilez les sources de Nagios

Pour télécharger les sources de Nagios Core, il faut se rendre directement sur le site de Nagios : http://www.nagios.org
Dès la page d’accueil, vous trouverez un lien vers le téléchargement de **Nagios Core**.

**[SCREENSHOT : 2.1-3_NagiosCoreDownloadLink ]**
Un clic sur ce lien vous amène sur une page vous proposant de choisir votre système d’exploitation. En ce qui vous concerne, ce sera bien entendu Linux et son fidèle TUX !

**[SCREENSHOT : 2.1-4_NagiosCoreSystemSelection ]**

[Ce document](https://assets.nagios.com/downloads/nagiosxi/docs/Installing-Nagios-XI-Manually-on-Linux.pdf) permet de téléchargerles sources avec une documentation d’installation. Il concerne principalement la version XI de Nagios, que je vous ai déjà présentée dans la première partie de ce cours. J’en reparlerai dans le chapitre 4 de cette partie.

**[SCREENSHOT : 2.1-5_NagiosXIDocumentationLink]**

Pour obtenir de la documentation sur **Nagios Core** et notamment sur le processus d’installation pour Debian, je vous invite à vous rendre sur [cette page](https://support.nagios.com/kb/article/nagios-core-installing-nagios-core-from-source-96.html#Debian).

**[SCREENSHOT : 2.1-6_NagiosCoreInstallationDebianDocumentationLink]**

Vous pouvez maintenant continuer le processus de téléchargement des sources et cliquer sur le bouton « Download ». Vous arrivez alors sur un *pop-in*, qui vous propose de donner vos coordonnées si vous souhaitez être informé des évolutions des produits Nagios.

**[SCREENSHOT : 2.1-7_NagiosMailingList]**

Cliquer sur « Skip » vous évite de donner votre adresse de messagerie, si vous ne le souhaitez pas.
Finalement, tout ceci vous amène enfin à la page de téléchargement des sources de Nagios Core :

**[SCREENSHOT : 2.1-8_DownloadNagiosCore]**

Effectuez un clic droit sur le lien `nagios-X.X.X.tar.gz` pour copier le lien dans le presse-papier, puis saisissez la commande suivante dans votre terminal depuis le répertoire `/home/nagios/downloads`. En l’occurrence ici, la version téléchargée est la 4.4.2.

```shell
cd /home/nagios/downloads
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.2.tar.gz
```

Le résultat de cette commande doit ressembler à :

```console
root@NagiosDebian:/home/nagios/downloads# wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.2.tar.gz
--2018-11-23 10:16:53--  https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.2.tar.gz
Résolution de assets.nagios.com (assets.nagios.com)…2600:3c00::f03c:91ff:fedf:b821, 72.14.181.71
Connexion à assets.nagios.com (assets.nagios.com)|2600:3c00::f03c:91ff:fedf:b821|:443… connecté.
requête HTTP transmise, en attente de la réponse… 200 OK
Taille : 11301454 (11M) [application/x-gzip]
Sauvegarde en : `nagios-4.4.2.tar.gz`
nagios-4.4.2.tar.gz  100%[==============================================================>]  10,78M 956KB/s in 13s
2018-11-23 10:17:06 (880 KB/s) — `nagios-4.4.2.tar.gz` sauvegardé [11301454/11301454]
```

Décompressez l’archive des sources de Nagios avec la commande suivante :

```shell
tar -zxvf nagios-4.4.2.tar.gz
```  

Cette commande crée un répertoire `nagios-4.4.2` dans le répertoire courant et y décompresse toutes les sources.
Positionnez-vous dans ce répertoire avec la commande suivante :

```shell
cd nagios-4.4.2
```

La compilation des sources Nagios passe par un script `configure`. Celui-ci permet, entre autres, de s’assurer que les éléments nécessaires sont présents sur le système, et de passer quelques paramètres au processus de compilation.
Dans votre cas, vous allez indiquer deux choses : où se situe le répertoire par défaut de configuration des sites Web, et où se situe le groupe `nagcmd` que vous souhaitez configurer… le tout, avec la commande suivante (**sur la même ligne** !) :

```shell
./configure --with-httpd-conf=/etc/apache2/sites-enabled --with-command-group=nagcmd
```

Si tout se passe bien, le résultat de cette commande doit ressembler à :

```console

*** Configuration summary for nagios 4.4.2 2018-08-16 ***:
General Options:
-------------------------
        Nagios executable:  nagios
        Nagios user/group:  nagios,nagios
       Command user/group:  nagios,nagcmd
             Event Broker:  yes
        Install ${prefix}:  /usr/local/nagios
    Install ${includedir}:  /usr/local/nagios/include/nagios
                Lock file:  /run/nagios.lock
   Check result directory:  /usr/local/nagios/var/spool/checkresults
           Init directory:  /lib/systemd/system
  Apache conf.d directory:  /etc/apache2/sites-enabled
             Mail program:  /bin/mail
                  Host OS:  linux-gnu
          IOBroker Method:  epoll
Web Interface Options:
------------------------
                HTML URL:  http://localhost/nagios/
                 CGI URL:  http://localhost/nagios/cgi-bin/
Traceroute (used by WAP):  /usr/sbin/traceroute
Review the options above for accuracy. If they look okay,
type 'make all' to compile the main program and CGIs.
```
Pour lancer la compilation, exécutez la commande suivante :
```shell
make all
```

Encore une fois, si tout se passe bien, la sortie de cette commande doit ressembler à :

```console
*** Support Notes *******************************************
If you have questions about configuring or running Nagios,
please make sure that you:
- Look at the sample config files
- Read the documentation on the Nagios Library at:
https://library.nagios.com
before you post a question to one of the mailing lists.
Also make sure to include pertinent information that could
help others help you.  This might include:
- What version of Nagios you are using
- What version of the plugins you are using
- Relevant snippets from your config files
- Relevant error messages from the Nagios log file
For more information on obtaining support for Nagios, visit:
https://support.nagios.com
*************************************************************
Enjoy.
```

Bravo ! Il vous reste quelques commandes à passer pour finaliser l’installation.

#### Installez l’arborescence et les fichiers Nagios sur votre serveur

##### Créez l'arborescence Nagios

Afin de créer l’arborescence Nagios et d’y installer les fichiers binaires, lancez la commande suivante :
```console
make install
```

Vous pouvez vérifier que l’arborescence Nagios est créée avec la commande suivante :

```shell
ls -lrtha /usr/local/nagios
```

Cette commande affiche la liste des premiers répertoires de Nagios :

```console
drwxrwsr-x 11 root staff  4,0K nov.  23 10:37..
drwxrwsr-x  2 nagios nagios 4,0K nov.  23 10:37 bin
drwxrwsr-x  2 nagios nagios 4,0K nov.  23 10:37 sbin
drwxrwsr-x 14 nagios nagios 4,0K nov.  23 10:37 share
drwxrwsr-x  2 nagios nagios 4,0K nov.  23 10:37 libexec
drwxr-sr-x  7 root staff  4,0K nov.  23 10:37 .
drwxrwsr-x  4 nagios nagios 4,0K nov.  23 10:37 var
```

##### Installez le service Nagios

Ensuite, vous allez installer le service Nagios, c’est-à-dire les composants nécessaires au démarrage de Nagios avec la machine. Lancez la commande suivante :

```shell
make install-daemoninit
```

Cette commande vous retournera :

```shell
/usr/bin/install -c -m 755 -d -o root -g root /lib/systemd/system
/usr/bin/install -c -m 755 -o root -g root startup/default-service /lib/systemd/system/nagios.service
Created symlink /etc/systemd/system/multi-user.target.wants/nagios.service → /lib/systemd/system/nagios.service.
*** Init script installed ***
```

Vous pouvez ici constater la création du fichier de service unit `nagios.service` dans l’arborescence de `systemd`. Ainsi que la création du lien symbolique dans la « target multi-user », afin de démarrer le service automatiquement avec le système.

##### Installez le pipe de Nagios

Etape suivante : installer le **pipe** de Nagios. Lancez la commande suivante :

```shell
make install-commandmode
```

Observez le résultat de cette commande :

```console
/usr/bin/install -c -m 775 -o nagios -g nagcmd -d /usr/local/nagios/var/rw
chmod g+s /usr/local/nagios/var/rw
*** External command directory configured ***
```

Vous pouvez notamment constater qu’un répertoire `**rw**` a été créé dans l’arborescence Nagios. Les droits de lecture et d’écriture sont attribués à l’utilisateur `nagios` et au groupe `nagcmd` (que vous avez indiqué lors de l’exécution du script `configure`).
Par ailleurs la présence du **sticky bit** (via la commande `chmod g+s`) permet de distribuer l’ID du groupe `nagcmd` à tous les fichiers et répertoires qui seraient créés dans ce répertoire `rw`. Vous verrez un peu plus loin dans le cours que, lorsque Nagios se lance, ce dernier crée justement son « **pipe** » de commandes dans ce répertoire. Je vous donne rendez-vous dans le chapitre 4 de cette partie pour illustrer complètement le fonctionnement de ce « **pipe** ».

##### Installez les fichiers de configuration de Nagios

Pour installer les fichiers de configuration de base de Nagios, lancez la commande suivante :

```shell
make install-config
```

Le résultat de cette commande indique la liste des fichiers de configuration déposée dans l’arborescence, comme ci-dessous :

```console
/usr/bin/install -c -m 775 -o nagios -g nagios -d /usr/local/nagios/etc
/usr/bin/install -c -m 775 -o nagios -g nagios -d /usr/local/nagios/etc/objects
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/nagios.cfg /usr/local/nagios/etc/nagios.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/cgi.cfg /usr/local/nagios/etc/cgi.cfg
/usr/bin/install -c -b -m 660 -o nagios -g nagios sample-config/resource.cfg /usr/local/nagios/etc/resource.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/templates.cfg /usr/local/nagios/etc/objects/templates.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/commands.cfg /usr/local/nagios/etc/objects/commands.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/contacts.cfg /usr/local/nagios/etc/objects/contacts.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/timeperiods.cfg /usr/local/nagios/etc/objects/timeperiods.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/localhost.cfg /usr/local/nagios/etc/objects/localhost.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/windows.cfg /usr/local/nagios/etc/objects/windows.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/printer.cfg /usr/local/nagios/etc/objects/printer.cfg
/usr/bin/install -c -b -m 664 -o nagios -g nagios sample-config/template-object/switch.cfg /usr/local/nagios/etc/objects/switch.cfg
```

##### Installez l'interface Webd'administration

Pour installer l’interface Web d’administration de Nagios, lancez la commande suivante :

```shell
make install-webconf
```

Cette commande dépose le fichier `nagios.conf` dans l’arborescence Apache (`/etc/apache2/sites-enabled`). Pour fonctionner correctement, l’interface d’administration de Nagios nécessite les modules `rewrite` et `cgi` d’Apache. Pour les activer, lancez les commandes suivantes :
```console
a2enmod rewrite
a2enmod cgi
```
Ces commandes vous demandent de redémarrer le service Apache afin d’être prises en compte. Mais vous allez retarder cette instruction, car vous n’en avez pas encore terminé avec la configuration de Nagios.

##### Configurez l'accès Apache

Pour accéder à l’interface d’administration de Nagios, il est nécessaire de configurer un accès Apache **htaccess**. Lancez la commande suivante :

```shell
htpasswd -cb /usr/local/nagios/etc/htpasswd.users nagiosadmin pass
```

Cette commande crée le fichier `htaccess` dans l’arborescence du site d’administration (`/usr/local/nagios/etc/htpasswd.users`) et configure un premier utilisateur comme ci-dessous :
- **login : nagiosadmin** ;
- **password : pass** (encore une fois, ce mot de passe est à renforcer dans un contexte de production).

##### Configurez les droits pour la configuration
La configuration Nagios s’effectuera directement avec le compte **nagios**, **pas besoin d’être root** pour cela. Il faut tout de même attribuer les droits sur l’arborescence Nagios à l’utilisateur **nagios**. Pour ce faire, lancez la commande suivante :
```shell
root@NagiosDebian:~# chown -R nagios:nagcmd /usr/local/nagios
```
Cette commande attribue, de manière récursive, la propriété à l’utilisateur `nagios` et au groupe `nagcmd` de l’arborescence Nagios (`/usr/local/nagios`).

##### Redémarrez Apache
Dernières opérations : il faut redémarrer le service Apache et démarrer le service Nagios. Pour cela, lancez les commandes suivantes :
```shell
systemctl restart apache2
systemctl start nagios
```

Si tout se passe bien, ces deux commandes ne renvoient rien en sortie standard.
**Bravo ! Vous venez d’installer Nagios Core sur une machine GNU/Linux Debian.**
Pour vérifier que Nagios tourne, lancez la commande suivante :
```shell
ps -edf | grep nagios
```

Parmi les lignes résultat, vous devriez trouver celles-ci :

```console
nagios 29558 1  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios -d /usr/local/nagios/etc/nagios.cfg
nagios 29559 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios --worker /usr/local/nagios/var/rw/nagios.qh
nagios 29560 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios --worker /usr/local/nagios/var/rw/nagios.qh
nagios 29561 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios --worker /usr/local/nagios/var/rw/nagios.qh
nagios 29562 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios --worker /usr/local/nagios/var/rw/nagios.qh
nagios 29563 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios -d /usr/local/nagios/etc/nagios.cfg
```
Debian 9 étant sous **systemd**, vous pouvez aussi lancer la commande suivante :
```shell
systemctl status nagios
```
Le retour de cette commande est complet, car il permet non seulement de s’assurer que le service a bien pour statut `**active**`, mais aussi de consulter les dernières lignes des fichiers de traces du service Nagios. En l’occurrence, vous devriez obtenir quelque chose comme :
```console
wproc: early_timeout=0; exited_ok=1; wait_status=32512; error_code=0;
wproc: stderr line 01: /bin/sh: 1: /bin/mail: not found
wproc: stderr line 02: /usr/bin/printf: erreur d'écriture: Relais brisé (pip
SERVICE ALERT: localhost;Swap Usage;CRITICAL;HARD;1;(No output on stdout) stde
SERVICE ALERT: localhost;Total Processes;CRITICAL;HARD;1;(No output on stdout)
```
Diantre ! Il semblerait que Nagios envoie déjà quelques alertes de supervision !

#### Connectez-vous une première fois à l’interface d’administration de Nagios
Allez immédiatement vérifier cet état de fait en consultant l’interface d’administration de Nagios. Pour cela, ouvrez un navigateur et connectez-vous sur l’adresse IP de votre serveur Nagios via une URL du type :
http://@ipServeurNagios/nagios (par exemple http://172.16.10.10/nagios).
Le site d’administration vous propose alors de vous connecter en renseignant votre login et votre password. Souvenez-vous que dans le processus d’installation, vous avez indiqué les accès suivants :
- login : nagiosadmin ;
- password : pass.
**[SCREENSHOT : 2.1-9_NagiosHTaccess]**
Une fois ces identifiants saisis, vous arrivez normalement sur la page d’accueil de l’interface Web d’administration de Nagios :
**[SCREENSHOT : 2.1-10_NagiosWebHomePage]**
Profitez de ce premier accès à l’interface Nagios pour cliquer sur le « Alerts » dans le menu « Reports », sur la gauche de la page.
**[SCREENSHOT : 2.1-11_NagiosAlerts]**
Aïe ! Effectivement, Nagios relève déjà quelques soucis. Mais en observant un peu mieux la description de ces lignes, vous pourrez observer qu’elles ont toutes une cause commune :
**[SCREENSHOT : 2.1-12_NoSuchFileOrDirectory ]**
Et la cause commune de ces erreurs est simple : Nagios cherche des fichiers qui n’existent pas dans son arborescence. C’est normal, ces fichiers sont les **plugins** standards de Nagios, que nous n'avons pas encore installés.

#### En résumé
Dans ce chapitre, nous avons vu ensemble la première étape de l'installation de Nagios Core, c’est-à-dire :
- la préparation de son serveur d'hébergement ; 
- la compilation de ses sources ;
- la création de son arborescence ;
- la configuration de son interface Webd'administration.
La seconde étape consiste justement à **compiler et installer les plugins**. Ces actions viendront corriger d'elles-mêmes les erreurs affichées sur l'interface. C'est ce que je vous propose de réaliser dans le prochain chapitre !

<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJtUk5ibld0QW44S2R4SWdBIjp7In
RleHQiOiJQb3VyIGV4cGxvaXRlciBhdSBtYXhpbXVtIGxlcyBw
b3NzaWJpbGl0w6lzIG9mZmVydGVzIHBhciBsZXMgcGx1Z2lucy
BkZSBOYWdpb3Ms4oCmIiwic3RhcnQiOjIwMzIsImVuZCI6MjE3
M30sIm1EUWVudDZnT3NiWHdSb2wiOnsidGV4dCI6IkVuZmluIH
BvdXIgaW5zdGFsbGVyIE5hZ2lvcyBldCBzZXMgcGx1Z2lucyB2
b3VzIGF1cmV6IGJlc29pbiBkZXMgb3V0aWxzIGRlIGNvbXDigK
YiLCJzdGFydCI6Mjg1NSwiZW5kIjoyOTczfSwiQTZRYW9RbVJ5
QmdvYlBKdyI6eyJ0ZXh0IjoidXRpbGlzYXRldXIiLCJzdGFydC
I6MzUyMiwiZW5kIjozNTMzfX0sImNvbW1lbnRzIjp7IlROc3ZF
MEFmZFF5aTRIVlMiOnsiZGlzY3Vzc2lvbklkIjoibVJOYm5XdE
FuOEtkeElnQSIsInN1YiI6ImdvOjEwMjEyMTAyMDI5NTY5OTEz
NTMzOCIsInRleHQiOiJFc3QpY2UgcXVlIHR1IHBlbnNlcyBxdW
Ugw6dhIHZhdWRyYWl0IGxlIGNvw7t0IGRlIHLDqXN1bWVyIHJh
cGlkZW1lbnQgw6AgcXVvaSBzZXJ2ZW50IGNlcyBsaWJyYWlyaW
VzID8gU2FucyBlbnRyZXIgZGFucyBsZXMgZMOpdGFpbHMsIG1h
aXMgcGFyIGV4ZW1wbGUgcG9pbnRlciB1bmUgZm9uY3Rpb25uYW
xpdMOpIGVuIHBhcnRpY3VsaWVyIHF1aSBsZXMgbsOpY2Vzc2l0
ZS4gRXN0LWNlIHF1ZSB0dSB2YXMgbCd1dGlsaXNlciBkYW5zIG
NlIGNvdXJzID9cblBlbnNlcy10dSBxdWUgY2Ugc29pdCBwZXJ0
aW5lbnQgZGUgcHLDqWNpc2VyIMOnYSA/IiwiY3JlYXRlZCI6MT
U0NDU0NTUxNDAzMH0sIklWNHBENkpQb1E2blBBaFkiOnsiZGlz
Y3Vzc2lvbklkIjoibURRZW50NmdPc2JYd1JvbCIsInN1YiI6Im
dvOjEwMjEyMTAyMDI5NTY5OTEzNTMzOCIsInRleHQiOiJFbmNv
cmUgdW5lIGZvaXMsIGlsIG1lIHNlbWJsZSBxdWUgw6dhIHZhdW
RyYWl0IGxlIGNvdXAgZCdleHBsaXF1ZXIgZW4gdW5lIHBocmFz
ZSDDoCBxdW9pIMOnYSBzZXJ2aXJhLCBwb3VyIMOpdml0ZXIgcX
VlIGwnw6l0dWRpYW50IG4naW5zdGFsbGUgZGVzIHBhY2thZ2Vz
IMOgIGwnYXZldWdsZS4gUXUnZW4gcGVuc2VzLXR1ID8iLCJjcm
VhdGVkIjoxNTQ0NTQ1NjE0MjY3fSwiTUJob0dMR0NISmNzQ2ZV
NyI6eyJkaXNjdXNzaW9uSWQiOiJBNlFhb1FtUnlCZ29iUEp3Ii
wic3ViIjoiZ286MTAyMTIxMDIwMjk1Njk5MTM1MzM4IiwidGV4
dCI6IkRlIGNlIHF1ZSBqZSB2b2lzLCBsZSBjb25jZXB0IGTigJ
l1dGlsaXNhdGV1ciBpY2kgZXN0IGFzc2V6IHBhcnRpY3VsaWVy
LiBBIHF1b2kgc2VydC1pbCA/IFF1ZSByZXByw6lzZW50ZS10LW
lsID8gSWwgbWUgc2VtYmxlIHF14oCZaWwgZmF1ZHJhaXQgYmll
biBkw6lmaW5pciBjZSBjb25jZXB0IGF2YW50IGRlIGNvbnRpbn
Vlci4gUXXigJllbiBwZW5zZXMtdHUgPyIsImNyZWF0ZWQiOjE1
NDQ1NDYxNzYwOTV9fSwiaGlzdG9yeSI6WzE0NTY1MjE0OTQsLT
IwMTAzMzE5NzMsLTExNzczNjc5NTAsNzcwOTgzNTAyLDk4MjY0
MzY0NiwzMzQxOTk0NzAsMTQ5Nzk1MzI4LC0yMTA3ODA1Nzg3LC
0xMjUxNjA1NTY4LDIwMjgxMzgzODYsMTM4NjIxNDYyNyw0NDU2
MTUyMzAsLTk2NDEwMDYwMSwxMzQwMDEzMTExLC0xMDAzNTMzOT
E2LDgzODU1NzI4NSwxMDg3MTM4MDY3XX0=
-->