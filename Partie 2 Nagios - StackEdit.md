## Partie 2 Installez votre outil de supervision
### Chapitre 2.1. Installez la solution Nagios CORE

>Maintenant que vous avez connaissance des problématiques de supervision, 
>des différents acteurs sur le marché ainsi que des avantages et inconvénients de fairele choix de la solution Nagios, je vous retrouve donc dans ce chapitre avec l’objectif d’installer Nagios Core depuis les sources sur un système GNU/Linux. Ce processus peut se détailler en 4 grandes étapes :

>-   La **préparation du serveur** à héberger le service Nagios ;
>-   Le **téléchargement et la compilation des sources** Nagios ;
>-   L’installation de **l’arborescence** Nagios ;
>-   Le vérification de la connexion à **l’interface d’administration** de Nagios.

#### A. Préparez un serveur Debian à recevoir Nagios

La machine cible de l’installation de Nagios est une **Debian Stretch 9.6.0** en version minimale disponible au téléchargement directement depuis le site de Debian. Par exemple, pour une architecture amd64 : https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.6.0-amd64-netinst.iso

**[SCREENSHOT : 2.1-1_ImageMinimaleDebian ]**

Pendant le processus d’installation, je vous propose dans un premier temps de ne sélectionner que les composants minimums au système, à savoir :

-   Le **serveur SSH** pour se connecter via un terminal distant ;
-   Les **composants usuels** du systèmes qui contiennent les librairies C les plus communément employées.

**[SCREENSHOT : 2.1-2_InstallMinimaleDebian ]**

Nagios nécessite quelques packages supplémentaires sur cette installation minimale afin de profiter pleinement de toutes ses capacités.

Vous allez donc installer quelques dépendances via diverses commandes que je vous présente dans la suite.

#### B. Installez le serveur Apache
Pour accéder à l’interface web de gestion de Nagios, vous avez besoin d’un serveur apache et de l’interpréteur PHP.

```shell 
apt-get install apache2 php php-gd php-imap php-curl php-mcrypt
```

Cette commande va installer les packages suivants :

```console 
apache2-bin apache2-data apache2-utils fontconfig-config fonts-dejavu-core libapache2-mod-php7.0 libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libc-client2007e libcurl3 libfontconfig1 libgd3 libjbig0 libjpeg62-turbo libltdl7 liblua5.2-0 libmcrypt4 libtiff5

libwebp6 libxpm4 mlock php-common php7.0 php7.0-cli php7.0-common php7.0-curl php7.0-gd php7.0-imap php7.0-json php7.0-mcrypt php7.0-opcache php7.0-readline psmisc ssl-cert

Paquets suggérés :

www-browser apache2-doc apache2-suexec-pristine | apache2-suexec-custom php-pear uw-mailutils libgd-tools libmcrypt-dev mcrypt openssl-blacklist

Les NOUVEAUX paquets suivants seront installés :

apache2 apache2-bin apache2-data apache2-utils fontconfig-config fonts-dejavu-core libapache2-mod-php7.0 libapr1 libaprutil1 libaprutil1-dbd-sqlite3 libaprutil1-ldap libc-client2007e libcurl3 libfontconfig1 libgd3 libjbig0 libjpeg62-turbo libltdl7 liblua5.2-0 libmcrypt4

libtiff5 libwebp6 libxpm4 mlock php php-common php-curl php-gd php-imap php-mcrypt php7.0 php7.0-cli php7.0-common php7.0-curl php7.0-gd php7.0-imap php7.0-json php7.0-mcrypt php7.0-opcache php7.0-readline psmisc ssl-cert
```
#### C. Installez les librairies Perl
Pour exploiter au maximum les possibilités offertes par les plugins de Nagios, vous avez besoin de quelques librairies Perl supplémentaires :

```shell
apt-get install libxml-libxml-perl libnet-snmp-perl libperl-dev libnumber-format-perl libconfig-inifiles-perl libdatetime-perl libnet-dns-perl
```
Cette commande va installer les packages suivants :

```console
libalgorithm-c3-perl libauthen-sasl-perl libb-hooks-endofscope-perl libc-dev-bin libc6-dev libclass-c3-perl libclass-c3-xs-perl libclass-data-inheritable-perl libclass-method-modifiers-perl libclass-singleton-perl libdata-optlist-perl libdatetime-locale-perl
libdatetime-timezone-perl libdevel-caller-perl libdevel-lexalias-perl libdevel-stacktrace-perl libdigest-hmac-perl libencode-locale-perl libeval-closure-perl libexception-class-perl libfile-listing-perl libfont-afm-perl libhtml-form-perl libhtml-format-perl
libhtml-parser-perl libhtml-tagset-perl libhtml-tree-perl libhttp-cookies-perl libhttp-daemon-perl libhttp-date-perl libhttp-message-perl libhttp-negotiate-perl libio-html-perl libio-socket-inet6-perl libio-socket-ssl-perl liblwp-mediatypes-perl liblwp-protocol-https-perl
libmailtools-perl libmodule-implementation-perl libmodule-runtime-perl libmro-compat-perl libnamespace-autoclean-perl libnamespace-clean-perl libnet-http-perl libnet-ip-perl libnet-smtp-ssl-perl libnet-ssleay-perl libpackage-stash-perl libpackage-stash-xs-perl
libpadwalker-perl libparams-classify-perl libparams-util-perl libparams-validationcompiler-perl librole-tiny-perl libscalar-list-utils-perl libsocket6-perl libspecio-perl libsub-exporter-perl libsub-exporter-progressive-perl libsub-identify-perl libsub-install-perl
libsub-name-perl libtest-fatal-perl libtimedate-perl libtry-tiny-perl liburi-perl libvariable-magic-perl libwww-perl libwww-robotrules-perl libxml-namespacesupport-perl libxml-parser-perl libxml-sax-base-perl libxml-sax-expat-perl libxml-sax-perl linux-libc-dev manpages-dev
perl-openssl-defaults

Paquets suggérés :

libgssapi-perl glibc-doc libdata-dump-perl libcrypt-ssleay-perl libcrypt-des-perl libscalar-number-perl libauthen-ntlm-perl

Les NOUVEAUX paquets suivants seront installés :

libalgorithm-c3-perl libauthen-sasl-perl libb-hooks-endofscope-perl libc-dev-bin libc6-dev libclass-c3-perl libclass-c3-xs-perl libclass-data-inheritable-perl libclass-method-modifiers-perl libclass-singleton-perl libconfig-inifiles-perl libdata-optlist-perl
libdatetime-locale-perl libdatetime-perl libdatetime-timezone-perl libdevel-caller-perl libdevel-lexalias-perl libdevel-stacktrace-perl libdigest-hmac-perl libencode-locale-perl libeval-closure-perl libexception-class-perl libfile-listing-perl libfont-afm-perl
libhtml-form-perl libhtml-format-perl libhtml-parser-perl libhtml-tagset-perl libhtml-tree-perl libhttp-cookies-perl libhttp-daemon-perl libhttp-date-perl libhttp-message-perl libhttp-negotiate-perl libio-html-perl libio-socket-inet6-perl libio-socket-ssl-perl
liblwp-mediatypes-perl liblwp-protocol-https-perl libmailtools-perl libmodule-implementation-perl libmodule-runtime-perl libmro-compat-perl libnamespace-autoclean-perl libnamespace-clean-perl libnet-dns-perl libnet-http-perl libnet-ip-perl libnet-smtp-ssl-perl
libnet-snmp-perl libnet-ssleay-perl libnumber-format-perl libpackage-stash-perl libpackage-stash-xs-perl libpadwalker-perl libparams-classify-perl libparams-util-perl libparams-validationcompiler-perl libperl-dev librole-tiny-perl libscalar-list-utils-perl libsocket6-perl
libspecio-perl libsub-exporter-perl libsub-exporter-progressive-perl libsub-identify-perl libsub-install-perl libsub-name-perl libtest-fatal-perl libtimedate-perl libtry-tiny-perl liburi-perl libvariable-magic-perl libwww-perl libwww-robotrules-perl libxml-libxml-perl
libxml-namespacesupport-perl libxml-parser-perl libxml-sax-base-perl libxml-sax-expat-perl libxml-sax-perl linux-libc-dev manpages-dev perl-openssl-defaults autoconf automake autotools-dev libsigsegv2 m4
```

#### D. Installez les librairies graphiques
Nagios nécessite également quelques librairies graphiques :

```shell
apt-get install libpng-dev libjpeg-dev libgd-dev
```

Cette commande va installer les packages suivants :

```console
libdpkg-perl libexpat1-dev libfile-fcntllock-perl libfontconfig1-dev libfreetype6-dev libice-dev libice6 libjbig-dev
libjpeg62-turbo-dev liblzma-dev libpng-tools libpthread-stubs0-dev libsm-dev libsm6 libtiff5-dev libtiffxx5 libvpx-dev libvpx4
libx11-dev libx11-doc libxau-dev libxcb1-dev libxdmcp-dev libxpm-dev libxt-dev libxt6 pkg-config x11-common x11proto-core-dev
x11proto-input-dev x11proto-kb-dev xorg-sgml-doctools xtrans-dev zlib1g-dev

Paquets suggérés :

debian-keyring patch libice-doc liblzma-doc libsm-doc libxcb-doc libxt-doc

Les NOUVEAUX paquets suivants seront installés :

libdpkg-perl libexpat1-dev libfile-fcntllock-perl libfontconfig1-dev libfreetype6-dev libgd-dev libice-dev libice6 libjbig-dev
libjpeg-dev libjpeg62-turbo-dev liblzma-dev libpng-dev libpng-tools libpthread-stubs0-dev libsm-dev libsm6 libtiff5-dev libtiffxx5
libvpx-dev libvpx4 libx11-dev libx11-doc libxau-dev libxcb1-dev libxdmcp-dev libxpm-dev libxt-dev libxt6 pkg-config x11-common
x11proto-core-dev x11proto-input-dev x11proto-kb-dev xorg-sgml-doctools xtrans-dev zlib1g-dev
```

#### E. Installez les outils de compilation standards
Enfin pour installer Nagios et ses plugins vous aurez besoin des outils de compilation standards et du package unzip :

```shell
apt-get install gcc make autoconf libc6 unzip
```

Cette commande va installer les packages suivants :

```console
binutils cpp cpp-6 gcc-6 libasan3 libatomic1 libcc1-0 libcilkrts5 libgcc-6-dev libgomp1 libisl15 libitm1 liblsan0 libmpc3 libmpfr4 libmpx2 libquadmath0 libtsan0 libubsan0

Paquets suggérés :

binutils-doc cpp-doc gcc-6-locales gcc-multilib autoconf automake libtool flex bison gdb gcc-doc gcc-6-multilib gcc-6-doc libgcc1-dbg libgomp1-dbg libitm1-dbg libatomic1-dbg libasan3-dbg liblsan0-dbg libtsan0-dbg libubsan0-dbg libcilkrts5-dbg libmpx2-dbg libquadmath0-dbg

make-doc

Les NOUVEAUX paquets suivants seront installés :

binutils cpp cpp-6 gcc gcc-6 libasan3 libatomic1 libcc1-0 libcilkrts5 libgcc-6-dev libgomp1 libisl15 libitm1 liblsan0 libmpc3 libmpfr4 libmpx2 libquadmath0 libtsan0 libubsan0 make unzip
```
#### F. Créez l'environnement Nagios
Il vous reste juste quelques ajouts d’ordre administratif à effectuer. Et notamment la **création de l’environnement Nagios**, avec son utilisateur, son groupe et son répertoire de travail.

Pour ajouter l’utilisateur **`nagios`** sur le système, saisissez la commande suivante :

```shell
useradd -m -p $(openssl passwd nagios) nagios
```

Cette commande créé l’utilisateur `nagios` sur le système et initie son mot de passe à `nagios`. Bien entendu, il vous sera nécessaire de renforcer ce mot de passe sur une machine de production.

>Par ailleurs, il est également nécessaire de partager un groupe commun entre l’utilisateur `nagios` et l’utilisateur `www-data` (qui fait tourner le **serveur Web apache2**) afin notamment de rendre possible l’administration de Nagios depuis l’interface web. Je reviendrai concrètement sur cette notion un peu plus loin dans le cours.

Pour l’instant, saisissez les commandes suivantes :

```shell
groupadd nagcmd
usermod -a -G nagcmd nagios
usermod -a -G nagcmd www-data
```

1. La première commande crée un groupe `nagcmd` qui servira à **exécuter des commandes Nagios depuis l’interface web** ;
2. La seconde commande ajoute l’utilisateur `nagios` dans le groupe `nagcmd` ;
3. La troisième commande ajouter l’utilisateur `www-data` sous lequel tournent les processus du serveur web Apache2 dans le groupe `nagcmd`.

Dernière commande à passer, la **création d’un répertoire de stockage pour tous les téléchargements** :

```shell
mkdir /home/nagios/downloads
```

Voilà, le serveur est maintenant prêt à recevoir Nagios et ses composants.

#### G. Téléchargez et compilez les sources de Nagios

Pour télécharger les sources de Nagios Core, il faut se rendre directement sur le site de Nagios : http://www.nagios.org

Dès la page d’accueil, vous trouverez un lien vers le téléchargement de **Nagios Core**.

**[SCREENSHOT : 2.1-3_NagiosCoreDownloadLink ]**

Un click sur ce lien vous amène sur une page vous proposant de choisir votre système d’exploitation. En ce qui vous concerne, ce sera bien entendu Linux et son fidèle TUX !

**[SCREENSHOT : 2.1-4_NagiosCoreSystemSelection ]**

[Ce document](https://assets.nagios.com/downloads/nagiosxi/docs/Installing-Nagios-XI-Manually-on-Linux.pdf) permet également le téléchargement des sources accompagnées cependant d’une documentation d’installation . Ce document concerne principalement la version XI de Nagios, je vous ai déjà présenté cette version dans la première partie de ce cours, j’en reparlerai dans le chapitre 4 de cette partie.

**[SCREENSHOT : 2.1-5_NagiosXIDocumentationLink ]**

Pour obtenir de la documentation sur la **version Core de Nagios** et notamment sur le processus d’installation pour Debian je vous invite à vous rendre sur [cette page](https://support.nagios.com/kb/article/nagios-core-installing-nagios-core-from-source-96.html#Debian)

**[SCREENSHOT : 2.1-6_NagiosCoreInstallationDebianDocumentationLink ]**

Vous pouvez maintenant continuer le processus de téléchargement des sources et cliquer sur le bouton `Download`. Vous arrivez désormais sur un pop-in vous invitant de manière facultative à laisser vos coordonnées à Nagios afin de vous tenir au courant des évolutions de leurs produits via une liste de diffusion.

**[SCREENSHOT : 2.1-7_NagiosMailingList ]**

Cliquer sur le lien `Skip` vous permet de ne pas laisser votre adresse de messagerie si vous ne le souhaitez pas.

Finalement, tout ceci vous amène enfin à la page de téléchargement des sources de Nagios Core :

**[SCREENSHOT : 2.1-8_DownloadNagiosCore ]**

Effectuez un clic droit sur le lien `nagios-X.X.X.tar.gz` pour copier le lien dans le presse papier et saisissez ensuite la commande suivante dans votre terminal dans le répertoire `/home/nagios/downloads`. En l’occurence ici, la version téléchargée est la 4.4.2.

```shell
cd /home/nagios/downloads
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.2.tar.gz
```
Le résultat de cette commande doit ressembler à quelque chose comme :

```console
root@NagiosDebian:/home/nagios/downloads# wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.2.tar.gz
--2018-11-23 10:16:53--  https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.2.tar.gz
Résolution de assets.nagios.com (assets.nagios.com)… 2600:3c00::f03c:91ff:fedf:b821, 72.14.181.71

Connexion à assets.nagios.com (assets.nagios.com)|2600:3c00::f03c:91ff:fedf:b821|:443… connecté.
requête HTTP transmise, en attente de la réponse… 200 OK
Taille : 11301454 (11M) [application/x-gzip]
Sauvegarde en : « nagios-4.4.2.tar.gz »

nagios-4.4.2.tar.gz  100%[==============================================================>]  10,78M 956KB/s  in 13s

2018-11-23 10:17:06 (880 KB/s) — « nagios-4.4.2.tar.gz » sauvegardé [11301454/11301454]
```

Décompressez l’archive des sources de Nagios avec la commande suivante  :

```shell
tar -zxvf nagios-4.4.2.tar.gz
```  

Cette commande crée un répertoire `nagios-4.4.2` dans le répertoire courant et y décompresse toutes les sources.

Positionnez-vous dans ce répertoire avec la commande suivante :

```shell
cd nagios-4.4.2
```

La compilation des sources Nagios passe par un script `configure` qui permet notamment de vérifier que les éléments nécessaires sont présents sur le système mais également de passer quelques paramètres au processus de compilation.

Dans votre cas, vous allez indiquer où se situent le répertoire par défaut de la configuration des sites web ainsi que le groupe `nagcmd` que vous souhaitez configurer, avec la commande suivante (**sur la même ligne** ! ) :

```shell
./configure --with-httpd-conf=/etc/apache2/sites-enabled --with-command-group=nagcmd
```

Si tout se passe bien, le résultat de cette commande doit produire quelque chose comme :

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

Review the options above for accuracy.  If they look okay,
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

#### H. Installez l’arborescence et les fichiers Nagios sur le serveur

##### Créez l'arborescence Nagios
Afin de créer l’arborescence Nagios et d’y installer les fichiers binaires lancez la commande suivante :

```console
make install
```
Vous pouvez vérifier que l’arborscence Nagios est créée avec la commande suivante :

```shell
ls -lrtha /usr/local/nagios
```

Cette commande affiche la liste des premiers répertoires de Nagios :

```console
drwxrwsr-x 11 root staff  4,0K nov.  23 10:37 ..
drwxrwsr-x  2 nagios nagios 4,0K nov.  23 10:37 bin
drwxrwsr-x  2 nagios nagios 4,0K nov.  23 10:37 sbin
drwxrwsr-x 14 nagios nagios 4,0K nov.  23 10:37 share
drwxrwsr-x  2 nagios nagios 4,0K nov.  23 10:37 libexec
drwxr-sr-x  7 root staff  4,0K nov.  23 10:37 .
drwxrwsr-x  4 nagios nagios 4,0K nov.  23 10:37 var
```

##### Installez le service Nagios
Ensuite, vous allez installer le service Nagios, c’est à dire les composants nécessaires à faire démarrer Nagios avec la machine. Lancez la commande suivante :

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

Vous pouvez ici constater la création du fichier de service unit `nagios.service` dans l’arborescence de `systemd`. Ainsi que la création du lien symbolique dans la `target multi-user` afin de démarrer le service automatiquement avec le système.

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

Vous pouvez notamment constater la création d’un répertoire « **rw** » dans l’arborescence Nagios à qui est attribué les droits de lecture et d’écriture pour l’utilisateur `nagios` et le groupe `nagcmd` (que vous avez indiqué lors de l’exécution du script `configure`).

Par ailleurs la présence du **sticky bit** (via la commande `chmod g+s`) permet de distribuer l’ID du groupe `nagcmd` à tous les fichiers et répertoires qui seraient créés dans ce répertoire `rw`. Vous verrez un peu plus loin dans le cours qu’au lancement de Nagios, ce dernier créé justement son « **pipe** » de commandes dans ce répertoire. Je vous donne rendez vous dans le chapitre 4 de cette partie pour illustrer complètement le fonctionnement de ce « **pipe** ».

##### Installez les fichiers de configuration de Nagios
Pour installer les fichiers de configuration de base de Nagios, lancez la commande suivante :

```shell
make install-config
```

Le résultat de cette commande indique la liste des fichiers de configuration déposés dans l’arborescence telle que :

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

##### Installez l'interface d'administration Web
Pour installer l’interface d’administration Web de Nagios, lancez la commande suivante :

```shell
make install-webconf
```

Cette commande dépose le fichier `nagios.conf` dans l’arborescence Apache (`/etc/apache2/sites-enabled`). Pour fonctionner correctement, l’interface d’administration de Nagios nécessite les modules `rewrite` et `cgi` de Apache. Pour les activer, lancez les commandes suivantes :

```console
a2enmod rewrite
a2enmod cgi
```

Ces commandes vous indiquent de redémarrer le service Apache pour être prises en compte. Mais vous allez retarder cette instruction, car vous n’en avez pas encore terminé avec la configuration de Nagios.

##### Configurez l'accès Apache
Pour accéder à l’interface d’administration de Nagios, il est nécessaire de configurer un accès Apache **htaccess**. Lancez la commande suivante :

```shell
htpasswd -cb /usr/local/nagios/etc/htpasswd.users nagiosadmin pass
```

Cette commande créé le fichier `htaccess` dans l’arborescence du site d’administration (`/usr/local/nagios/etc/htpasswd.users`) et configure un premier utilisateur tel que :

- **login : nagiosadmin** ;
- **password : pass** (encore une fois, ce mot de passe est à renforcer dans un contexte de production).

##### Configurez les droits pour la configuration
La configuration Nagios s’effectuera directement avec le compte **nagios**, **pas besoin d’être root** pour cela, mais il faut tout de même attribuer les droits sur l’arborescence Nagios à l’utilisateur **nagios**, pour cela, lancez la commande suivante :

```shell
root@NagiosDebian:~# chown -R nagios:nagcmd /usr/local/nagios
```

Cette commande attribue de manière récursive, la propriété à l’utilisateur `nagios` et au groupe `nagcmd` de l’arborescence Nagios (`/usr/local/nagios`).

##### Redémarrez Apache
Dernières opérations, il faut redémarrer le service Apache et démarrer le service Nagios. Pour cela, lancez les commandes suivantes :

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

Parmi les lignes résultats, vous devriez trouver celles-ci :

```console
nagios 29558 1  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios -d /usr/local/nagios/etc/nagios.cfg
nagios 29559 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios --worker /usr/local/nagios/var/rw/nagios.qh
nagios 29560 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios --worker /usr/local/nagios/var/rw/nagios.qh
nagios 29561 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios --worker /usr/local/nagios/var/rw/nagios.qh
nagios 29562 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios --worker /usr/local/nagios/var/rw/nagios.qh
nagios 29563 29558  0 11:04 ?  00:00:00 /usr/local/nagios/bin/nagios -d /usr/local/nagios/etc/nagios.cfg
```

Debian 9 étant sous **systemd**, vous pouvez également lancer la commande suivante :

```shell
systemctl status nagios
```

Le retour de cette commande est bien complet, car il permet de vérifier que le service est bien au statut « **active** », mais également de consulter les dernières lignes des fichiers de trace du service Nagios, en l’occurence, vous devriez obtenir quelque chose comme :

```console
wproc: early_timeout=0; exited_ok=1; wait_status=32512; error_code=0;
wproc: stderr line 01: /bin/sh: 1: /bin/mail: not found
wproc: stderr line 02: /usr/bin/printf: erreur d'écriture: Relais brisé (pip
SERVICE ALERT: localhost;Swap Usage;CRITICAL;HARD;1;(No output on stdout) stde
SERVICE ALERT: localhost;Total Processes;CRITICAL;HARD;1;(No output on stdout)
```

Diantre ! Il semblerait que Nagios envoie déjà quelques alertes de supervision !

#### I. Connectez vous une première fois à l’interface d’administration de Nagios

Allez immédiatement vérifier cet état de fait en consultant l’interface d’administration de Nagios. Pour cela, ouvrez un navigateur et connectez vous sur l’adresse IP de votre serveur Nagios via une URL du type :
http://@ipServeurNagios/nagios (par exemple http://172.16.10.10/nagios).

Le site d’administration vous propose alors de vous connecter via une mire login–password. Souvenez vous que dans le processus d’installation, vous avez indiqué les accès suivants :
- login : nagiosadmin ;
- password : pass.

**[SCREENSHOT : 2.1-9_NagiosHTaccess ]**

Une fois ces identifiants saisis, vous arrivez normalement sur la page d’accueil de l’interface d’administration Web de Nagios :

**[SCREENSHOT : 2.1-10_NagiosWebHomePage ]**

Profitez de ce premier accès sur l’interface Nagios pour cliquer sur le `Alerts` dans le menu `Reports` sur la gauche de la page.

**[SCREENSHOT : 2.1-11_NagiosAlerts ]**

Aïe ! Effectivement, Nagios relève déjà quelques soucis. Mais en observant un peu mieux la description de ces lignes, vous pourrez observer qu’elles ont toutes une cause commune :

**[SCREENSHOT : 2.1-12_NoSuchFileOrDirectory ]**

>Et la cause commune de ces erreurs est simple : Nagios cherche des fichiers qui n’existent pas dans son arborescence. C’est normal, ces fichiers sont les **plugins** standards de Nagios. Car, vous venez ici de compléter uniquement la première étape de l'installation de la solution Nagios Core, c'est à dire, la préparation de son serveur d'hébergement, la compilation de ses sources, la création de son arborescence et la configuration de son interface d'administration Web. La seconde étape consiste justement à compiler et installer ces plugins. Ces actions viendront corriger d'elles mêmes les erreurs affichées sur l'interface. C'est ce que je vous propose de réaliser dans le prochaine chapitre !
<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJNSnhMdGtBV0E4N1UyckMyIjp7In
RleHQiOiJgbmFnaW9zYCIsInN0YXJ0IjoyNzg1MywiZW5kIjoy
Nzg1M30sIlpPbUdyY3N2UTExbFBWZDMiOnsidGV4dCI6IltDZS
Bkb2N1bWVudF0oaHR0cHM6Ly9hc3NldHMubmFnaW9zLmNvbS9k
b3dubG9hZHMvbmFnaW9zeGkvZG9jcy9JbnN0YWxsaW5nLU5hZ2
nigKYiLCJzdGFydCI6MCwiZW5kIjowfSwiVVFsUWFVd09yVms0
RXNRbSI6eyJ0ZXh0IjoiPioqOmluZm9ybWF0aW9uX3NvdXJjZT
oqKiIsInN0YXJ0IjoyNzg1MywiZW5kIjoyNzg1M30sIlFsRHFq
Qm9uTHBLSHN0bXMiOnsidGV4dCI6IiMjIyMgUHLDqXBhcmV6IH
VuIHNlcnZldXIgRGViaWFuIMOgIHJlY2V2b2lyIE5hZ2lvcyIs
InN0YXJ0IjoyNzg1MywiZW5kIjoyNzg1M30sIk56OEU1SkxlV1
R0cHNnRzYiOnsidGV4dCI6ImBgYHNoZWxsIiwic3RhcnQiOjI3
ODUzLCJlbmQiOjI3ODUzfSwibVJOYm5XdEFuOEtkeElnQSI6ey
J0ZXh0IjoiUG91ciBleHBsb2l0ZXIgYXUgbWF4aW11bSBsZXMg
cG9zc2liaWxpdMOpcyBvZmZlcnRlcyBwYXIgbGVzIHBsdWdpbn
MgZGUgTmFnaW9zLOKApiIsInN0YXJ0IjozMjUwLCJlbmQiOjMz
OTF9LCJtRFFlbnQ2Z09zYlh3Um9sIjp7InRleHQiOiJFbmZpbi
Bwb3VyIGluc3RhbGxlciBOYWdpb3MgZXQgc2VzIHBsdWdpbnMg
dm91cyBhdXJleiBiZXNvaW4gZGVzIG91dGlscyBkZSBjb21w4o
CmIiwic3RhcnQiOjg1NzksImVuZCI6ODY5N30sIkE2UWFvUW1S
eUJnb2JQSnciOnsidGV4dCI6InV0aWxpc2F0ZXVyIiwic3Rhcn
QiOjk3MTEsImVuZCI6OTcyMn0sInFqWUJGa1lVbWlWY2RVV1gi
OnsidGV4dCI6IkNyw6lleiBsJ2FyYm9yZXNjZW5jZSBOYWdpb3
MiLCJzdGFydCI6MTc0NzIsImVuZCI6MTc0OTl9fSwiY29tbWVu
dHMiOnsiakVtTmpLakg0ZEJmd1QwOSI6eyJkaXNjdXNzaW9uSW
QiOiJNSnhMdGtBV0E4N1UyckMyIiwic3ViIjoiZ286MTAyMTIx
MDIwMjk1Njk5MTM1MzM4IiwidGV4dCI6IkxlcyB0aWxkZXMgKG
91IGFwb3N0cm9waGVzIMOgIGwnZW52ZXJzKSBzb250IGJpZW4g
dXRpbGVzIHBvdXIgbWV0dHJlIGVuIGF2YW50IGRlcyBtb3RzIG
91IGdyb3VwZXMgZGUgbW90cyBxdWkgZm9udCBwYXJ0aWUgZGUg
bGEgc3ludGF4ZSBkJ3VuIGxhbmdhZ2UgZGUgcHJvZ3JhbW1hdG
lvbiwgb3UgZGVzIG1vdHMgLyBub21zIGRlIHZhcmlhYmxlcyAv
IGNvbW1hbmRlcyBxdWUgbCd1dGlsaXNhdGV1ciB1dGlsaXNlcm
EgZGlyZWN0ZW1lbnQgZGFucyBzb24gY29kZSBvdSBzZXMgY29t
bWFuZGVzLiIsImNyZWF0ZWQiOjE1NDMyNDU4ODk0MzJ9LCJXQ2
JJdk1JVTd6eW4xaE5vIjp7ImRpc2N1c3Npb25JZCI6IlpPbUdy
Y3N2UTExbFBWZDMiLCJzdWIiOiJnbzoxMDIxMjEwMjAyOTU2OT
kxMzUzMzgiLCJ0ZXh0IjoiVHUgcGV1eCB1dGlsaXNlciBjZXR0
ZSBzeW50YXhlIHBvdXIgaW5zw6lyZXIgdW4gbGllbiBkYW5zIH
VuIGNoYW1wcyBkZSB0ZXh0ZS4gTWFpcyBzaSB0dSBzb3VoYWl0
ZXMgc2ltcGxlbWVudCBhZmZpY2hlciBsZSBsaWVuLCB0dSBwZX
V4IGxlIGNvcGllci1jb2xsZXIgZGlyZWN0ZW1lbnQuIiwiY3Jl
YXRlZCI6MTU0MzI0ODAyODE2Nn0sIlRNMWNFcTFWc0tYUDNEbm
wiOnsiZGlzY3Vzc2lvbklkIjoiVVFsUWFVd09yVms0RXNRbSIs
InN1YiI6ImdvOjEwMjEyMTAyMDI5NTY5OTEzNTMzOCIsInRleH
QiOiJDZWNpIHBlcm1ldCBkJ2luZGlxdWVyIHVuZSBiYWxpc2Ug
aW5mb3JtYXRpb24gcXVlIG5vdXMgdXRpbGlzb25zIHNvdXZlbn
QgZGFucyBsZXMgY291cnMgT3BlbkNsYXNzcm9vbXMgcG91ciBt
ZXR0cmUgZW4gdmFsZXVyIGRlcyBwYXJhZ3JhcGhlcyBpbXBvcn
RhbnRzLiIsImNyZWF0ZWQiOjE1NDMyNDk5ODQzNzl9LCJyeGYw
eXpBYmpJd0YwcGIyIjp7ImRpc2N1c3Npb25JZCI6IlFsRHFqQm
9uTHBLSHN0bXMiLCJzdWIiOiJnbzoxMDIxMjEwMjAyOTU2OTkx
MzUzMzgiLCJ0ZXh0IjoiQ2VjaSBlc3QgdW4gdGl0cmUgZGUgc2
VjdGlvbi4gSWNpLCB0dSBwZXV4IGxlcyBtYXJxdWVyIGF2ZWMg
NCMgKCMjIyMpLiIsImNyZWF0ZWQiOjE1NDMyNTAxNzg3Mzd9LC
IxTTNmdDRSbWtadWpDcWg5Ijp7ImRpc2N1c3Npb25JZCI6IlVR
bFFhVXdPclZrNEVzUW0iLCJzdWIiOiJnbzoxMDIxMjEwMjAyOT
U2OTkxMzUzMzgiLCJ0ZXh0IjoiQXR0ZW50aW9uLCBsZSByZW5k
dSBzdXIgU3RhY2tFZGl0IG4nZXN0IHBhcyBmaWTDqGxlIGF1IH
JlbmR1IE9DLiBBc3N1cmUgdG9pIHNpbXBsZW1lbnQgcXVlIHRv
dXQgbGUgdGV4dGUgcXVlIHR1IHNvdWhhaXRlcyBpbmNsdXJlIM
OgIGxhIGJhbGlzZSBlc3QgaW5kZW50w6kgKGNvbW1lIHVuZSBx
dW90ZSkgZXQgcXVlIGwnaWNvbmUgZCdpbmZvcm1hdGlvbiBhcH
BhcmFpdCBjb3JyZWN0ZW1lbnQgYXUgZMOpYnV0IGR1IHBhcmFn
cmFwaGUuIiwiY3JlYXRlZCI6MTU0MzI1MDUwMTg4NH0sIjVkTG
ZYTFM2VHh5WXNWTTIiOnsiZGlzY3Vzc2lvbklkIjoiTno4RTVK
TGVXVHRwc2dHNiIsInN1YiI6ImdvOjEwMjEyMTAyMDI5NTY5OT
EzNTMzOCIsInRleHQiOiJQb3VyIG1hcnF1ZXIgdW5lIHNlY3Rp
b24gY29tbWUgw6l0YW50IGR1IGNvZGUsIGVuY2FkcmUgbGUgcG
FyYWdyYXBoZSBwYXIgMyBgIHN1aXZpcyBkdSBub20gZHUgbGFu
Z2FnZS4gRGFucyB0b24gY291cnMsIGNlIHNlcmEgc3VyZW1lbn
QgdW5pcXVlbWVudCBcInNoZWxsXCIgcG91ciBsZXMgZW50csOp
ZXMgdXRpbGlzYXRldXIgZXQgXCJjb25zb2xlXCIgcG91ciBsZX
Mgc29ydGllcyBkdSB0ZXJtaW5hbC4gXG5cblBlbnNlIMOgIGJp
ZW4gZmFpcmUgY2V0dGUgZGlzdGluY3Rpb24gY2FyIGVsbGUgbW
9udHJlcmEgYmllbiDDoCBsJ8OpdHVkaWFudCBxdWUgbGVzIDIg
YWZmaWNoYWdlcyBvbnQgZGVzIHNpZ25pZmljYXRpb25zIGRpZm
bDqXJlbnRlcyBldCBsdWkgZmFjaWxpdGVyYSBsJ2FwcHJlbnRp
c3NhZ2UuIiwiY3JlYXRlZCI6MTU0MzI1MDc5NDk4Mn0sIlROc3
ZFMEFmZFF5aTRIVlMiOnsiZGlzY3Vzc2lvbklkIjoibVJOYm5X
dEFuOEtkeElnQSIsInN1YiI6ImdvOjEwMjEyMTAyMDI5NTY5OT
EzNTMzOCIsInRleHQiOiJFc3QpY2UgcXVlIHR1IHBlbnNlcyBx
dWUgw6dhIHZhdWRyYWl0IGxlIGNvw7t0IGRlIHLDqXN1bWVyIH
JhcGlkZW1lbnQgw6AgcXVvaSBzZXJ2ZW50IGNlcyBsaWJyYWly
aWVzID8gU2FucyBlbnRyZXIgZGFucyBsZXMgZMOpdGFpbHMsIG
1haXMgcGFyIGV4ZW1wbGUgcG9pbnRlciB1bmUgZm9uY3Rpb25u
YWxpdMOpIGVuIHBhcnRpY3VsaWVyIHF1aSBsZXMgbsOpY2Vzc2
l0ZS4gRXN0LWNlIHF1ZSB0dSB2YXMgbCd1dGlsaXNlciBkYW5z
IGNlIGNvdXJzID9cblBlbnNlcy10dSBxdWUgY2Ugc29pdCBwZX
J0aW5lbnQgZGUgcHLDqWNpc2VyIMOnYSA/IiwiY3JlYXRlZCI6
MTU0NDU0NTUxNDAzMH0sIklWNHBENkpQb1E2blBBaFkiOnsiZG
lzY3Vzc2lvbklkIjoibURRZW50NmdPc2JYd1JvbCIsInN1YiI6
ImdvOjEwMjEyMTAyMDI5NTY5OTEzNTMzOCIsInRleHQiOiJFbm
NvcmUgdW5lIGZvaXMsIGlsIG1lIHNlbWJsZSBxdWUgw6dhIHZh
dWRyYWl0IGxlIGNvdXAgZCdleHBsaXF1ZXIgZW4gdW5lIHBocm
FzZSDDoCBxdW9pIMOnYSBzZXJ2aXJhLCBwb3VyIMOpdml0ZXIg
cXVlIGwnw6l0dWRpYW50IG4naW5zdGFsbGUgZGVzIHBhY2thZ2
VzIMOgIGwnYXZldWdsZS4gUXUnZW4gcGVuc2VzLXR1ID8iLCJj
cmVhdGVkIjoxNTQ0NTQ1NjE0MjY3fSwiTUJob0dMR0NISmNzQ2
ZVNyI6eyJkaXNjdXNzaW9uSWQiOiJBNlFhb1FtUnlCZ29iUEp3
Iiwic3ViIjoiZ286MTAyMTIxMDIwMjk1Njk5MTM1MzM4IiwidG
V4dCI6IkRlIGNlIHF1ZSBqZSB2b2lzLCBsZSBjb25jZXB0IGTi
gJl1dGlsaXNhdGV1ciBpY2kgZXN0IGFzc2V6IHBhcnRpY3VsaW
VyLiBBIHF1b2kgc2VydC1pbCA/IFF1ZSByZXByw6lzZW50ZS10
LWlsID8gSWwgbWUgc2VtYmxlIHF14oCZaWwgZmF1ZHJhaXQgYm
llbiBkw6lmaW5pciBjZSBjb25jZXB0IGF2YW50IGRlIGNvbnRp
bnVlci4gUXXigJllbiBwZW5zZXMtdHUgPyIsImNyZWF0ZWQiOj
E1NDQ1NDYxNzYwOTV9LCJSeWVjbWFkVVRkekMwTnRrIjp7ImRp
c2N1c3Npb25JZCI6InFqWUJGa1lVbWlWY2RVV1giLCJzdWIiOi
JnbzoxMDIxMjEwMjAyOTU2OTkxMzUzMzgiLCJ0ZXh0IjoiQ2Vj
aSBlc3QgdW4gc291cy10aXRyZSBxdWkgdmllbnQgw6AgbOKAmW
ludMOpcmlldXIgZOKAmXVuZSBzZWN0aW9uLiBK4oCZYWkgcmVt
cGxhY8OpIHRvdXMgdGVzIHRpcmV0cyBwYXIgY2VzIHNvdXMtdG
l0cmVzIGzDoC4gSmUgdOKAmWludml0ZSDDoCBjb250aW51ZXIg
ZGUgbGVzIHV0aWxpc2VyIHBvdXIgcXVlIGNlIHNvaXQgdmlzdW
VsbGVtZW50IGNsYWlyIHBvdXIgbOKAmWFwcHJlbmFudCA6KSIs
ImNyZWF0ZWQiOjE1NDQ1NDYyNDQ1ODl9fSwiaGlzdG9yeSI6Wz
EzODYyMTQ2MjcsNDQ1NjE1MjMwLC05NjQxMDA2MDEsMTM0MDAx
MzExMSwtMTAwMzUzMzkxNiw4Mzg1NTcyODUsMTA4NzEzODA2N1
19
-->