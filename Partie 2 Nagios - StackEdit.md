## Partie 2 Installez votre outil de supervision
### Chapitre 2.1. Installez la solution Nagios CORE

>Maintenant que vous avez connaissance des problématiques de supervision, 
>des différents acteurs sur le marché ainsi que des avantages et inconvénients de faire
>le choix de la solution Nagios, je vous retrouve donc dans ce chapitre avec l’objectif d’installer Nagios Core depuis les sources sur un système GNU/Linux. Ce processus peut se détailler en 4 grandes étapes :

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

## Partie 2 Installez votre outil de supervision
### Chapitre 2.2. Installez les plugins standards de Nagios.

>**:information_source:** Dans ce chapitre vous allez installer les plugins standards de Nagios. Ces plugins sont dits « standards » car ils sont développés et maintenus par l’équipe de développement Nagios. Contrairement aux plugins dits « communautaires » qui sont mis à disposition par la communauté des utilisateurs Nagios via la plateforme [https://exchange.nagios.org](https://exchange.nagios.org).

#### A. Préparez un serveur Debian à recevoir les plugins standards de Nagios.

La phase de préparation est normalement déjà effectuée avec le premier chapitre de cette partie, car vous avez installé toutes les dépendances nécessaires.

Nagios précise tout de même quelques spécificités concernant l’utilisation de plugins particuliers. Vous pouvez retrouver ces informations ainsi que le processus d’installation des plugins sur la page suivante : [https://support.nagios.com/kb/article/nagios-plugins-installing-nagios-plugins-from-source-569.html](https://support.nagios.com/kb/article/nagios-plugins-installing-nagios-plugins-from-source-569.html).

Vous pourrez notamment trouver les instructions pour des plugins spécifiques comme par exemple la gestion d’un LDAP ou d’une base de données Mysql ou encore de Samba : 

**[SCREENSHOT : 2.2-1_PluginsSpecificDependencies ]**

Quoi qu’il en soit, le processus d’installation des plugins va également vérifier que les composants nécessaires sont bien installés et signaler lorsqu’il en manque ou prévenir que tel ou tel plugin ne pourra être compilé.

#### B. Téléchargez et compilez les sources des plugins standards de Nagios.

Les sources des plugins standards de Nagios sont également disponibles gratuitement sur le site de Nagios : [https://www.nagios.org](https://www.nagios.org)

Le chemin pour accéder au téléchargement est quasiment le même que pour les sources de Nagios. L’écran final vous propose le lien direct vers le fichier archive tel que : 

**[SCREENSHOT : 2.2-2_DownloadNagiosPlugins ]**

Effectuez un click droit sur le lien de téléchargement des sources `nagios-plugins-X.X.X.tar.gz` et copiez l’adresse dans le presse papier.

A partir de votre terminal d’accès au serveur Nagios, téléchargez les sources dans le répertoire `/home/nagios/downloads` avec les commandes suivantes : 

````shell
cd /home/nagios/downloads
wget https://nagios-plugins.org/download/nagios-plugins-2.2.1.tar.gz
````

Si tout se passe bien, les sources des plugins standards (ici de la version 2.2.1) sont téléchargées sur le serveur : 

````Console
Résolution de nagios-plugins.org (nagios-plugins.org)… 72.14.186.43
Connexion à nagios-plugins.org (nagios-plugins.org)|72.14.186.43|:443… connecté.
requête HTTP transmise, en attente de la réponse… 200 OK
Taille : 2728818 (2,6M) [application/x-gzip]
Sauvegarde en : « nagios-plugins-2.2.1.tar.gz »

nagios-plugins-2.2.1.tar.gz        100%[==============================================================>]   2,60M   896KB/s    in 3,0s    

2018-11-23 12:25:15 (896 KB/s) — « nagios-plugins-2.2.1.tar.gz » sauvegardé [2728818/2728818]
````

Décompressez l’archive des sources via la commande suivante : 

````shell
tar -zxvf nagios-plugins-2.2.1.tar.gz
````

Cette commande créé un répertoire `nagios-plugins-2.2.1` dans le répertoire courant et y décompresse les sources.

Positionnez vous dans ce nouveau répertoire via la commande suivante : 

````shell
cd nagios-plugins-2.2.1/
````

A l’identique du processus de compilation des sources de Nagios vu dans le chapitre précédent, pour compiler les plugins standards de Nagios il est nécessaire de lancer le script configure permettant notamment de vérifier que les éléments nécessaires sont disponibles sur le systèmes mais également de positionner quelques paramètres.

En l’occurence ici, vous allez indiquez qui sont l’utilisateur et le groupe propriétaires par défaut de ces plugins sur le système en lançant le script tel que : 

````shell
./configure --with-nagios-user=nagios --with-nagios-group=nagcmd 
````

Si tout se passe bien, le résultat de cette commande doit produire une sortie telle que : 

````console
configure: creating ./config.status
[...]
config.status: executing po-directories commands
config.status: creating po/POTFILES
config.status: creating po/Makefile
````

>*REMARQUE* : Si vous le souhaitez, vous pouvez retrouver toutes les informations recensées par le script configure dans le fichier suivant : 

````shell
more /home/nagios/downloads/nagios-plugins-2.2.1/config.log
````

Ce fichier est assez imposant, mais détaille précisément toutes les vérifications effectuées sur le système et contiendra notamment les éventuelles lignes expliquant pourquoi tel ou tel plugin n’a pas pu être compilé. Souvent parce que telle ou telle librairie n’est pas présente sur le système.

````console
This file contains any messages produced by compilers while
running configure, to aid debugging if configure makes a mistake.

It was created by nagios-plugins configure 2.2.1, which was
generated by GNU Autoconf 2.63.  Invocation command line was

   ./configure --with-nagios-user=nagios --with-nagios-group=nagcmd

hostname = NagiosDebian
uname -m = x86_64
uname -r = 4.9.0-8-amd64
uname -s = Linux
uname -v = #1 SMP Debian 4.9.130-2 (2018-10-27)
````

Bref, si à terme, vous cherchez un plugin standard qui n’est pas compilé, référez vous à ce fichier, il vous indiquera surement pourquoi !

Le reste du processus est identique, lancez dans un premier temps la commande suivante pour compiler les binaires : 

````shell
make
````

Cette commande compile tous les plugins compatibles avec le système et produit une sortie telle que : 

````console
make[2] : on quitte le répertoire « /home/nagios/downloads/nagios-plugins-2.2.1/plugins-root »
Making all in po
make[2] : on entre dans le répertoire « /home/nagios/downloads/nagios-plugins-2.2.1/po »
make[2]: rien à faire pour « all ».
make[2] : on quitte le répertoire « /home/nagios/downloads/nagios-plugins-2.2.1/po »
make[2] : on entre dans le répertoire « /home/nagios/downloads/nagios-plugins-2.2.1 »
make[2] : on quitte le répertoire « /home/nagios/downloads/nagios-plugins-2.2.1 »
make[1] : on quitte le répertoire « /home/nagios/downloads/nagios-plugins-2.2.1 »
````


#### C. Installez les plugins standards dans l’arborescence de Nagios.

Dernière étape, lancez la commande suivante : 

````shell
make install
````

Vous pourrez notamment constater que cette commande déploie les plugins standards dans l’arborescence de Nagios : 

````console
libtool: install: /usr/bin/install -c -o nagios -g nagcmd check_apt /usr/local/nagios/libexec/check_apt
libtool: install: /usr/bin/install -c -o nagios -g nagcmd check_cluster /usr/local/nagios/
[...]
libtool: install: /usr/bin/install -c -o nagios -g nagcmd check_mrtgtraf /usr/local/nagios/libexec/check_mrtgtraf
libtool: install: /usr/bin/install -c -o nagios -g nagcmd check_ntp /usr/local/nagios/libexec/check_ntp
````

Le répertoire par défaut des plugins standards Nagios est `/usr/local/nagios/libexec/`, pour le vérifier, lancez la commande suivante : 

````shell
ls -lrtha /usr/local/nagios/libexec
````

Vous pourrez également constater qu’à quelques exceptions prêts les 49 plugins standards appartiennent bien à l’utilisateur nagios et au groupe nagcmd.

#### D. Vérifiez que Nagios dispose bien de ses plugins.

Retournez désormais sur le site d’administration de Nagios, sur le même onglet « Alerts » du menu « Reports » à gauche de la fenêtre : 

*[SCREENSHOT : 2.2-3_AllGreen ]*

A LA BONNE HEURE ! Les alertes rouges de Nagios se sont transformées en vert !

BRAVO ! Vous venez de compléter l’installation des plugins standards de Nagios sur un système GNU/Linux Debian.

#### E. Définissez des alias de commande pour gagner du temps.

Le processus pourrait tout à fait s’arrêter là, mais tant que nous sommes dans les actions d’installation je vous propose d’ajouter deux petites manipulations qui vous feront gagner beaucoup de temps dans la suite du cours.

Vous verrez lors que la mise en place de vos sondes qu’il sera souvent nécessaire de tester la configuration de Nagios.

Pour cela, Nagios propose un argument spécifique à l’exécution de son programme principal permettant d’effectuer un premier jeu de test de sa configuration. Il suffit pour cela de lancer Nagios avec l’option « -v » en indiquant son fichier de configuration principal.

Lancez la commande suivante : 

````shell
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
````

A cet instant, la configuration de Nagios est minimale, donc cette commande doit normalement se terminer correctement, en affichant aucune erreur ni aucun avertissement, tel que : 

````console
Checking objects...
	Checked 8 services.
	Checked 1 hosts.
	Checked 1 host groups.
	Checked 0 service groups.
	Checked 1 contacts.
	Checked 1 contact groups.
	Checked 24 commands.
	Checked 5 time periods.
	Checked 0 host escalations.
	Checked 0 service escalations.
Checking for circular paths...
	Checked 1 hosts
	Checked 0 service dependencies
	Checked 0 host dependencies
	Checked 5 timeperiods
Checking global event handlers...
Checking obsessive compulsive processor commands...
Checking misc settings...

Total Warnings: 0
Total Errors:   0
````

Lorsque vous allez configurer des sondes Nagios, vous le ferez sous le compte utilisateur `nagios`, c’est toujours mieux de ne pas travailler directement sous le compte root si ce n’est pas nécessaire.

Je vous propose donc de créer un raccourcis de cette commande, que vous allez nommer « `testNagios` » dans le fichier de configuration du shell interactif (sans login) de l’utilisateur `nagios` (en effet, le service Nagios tourne de manière indépendante et ne nécessite heureusement aucune connexion réelle du compte `nagios`). Pour créer cet alias, exécutez la commande suivante : 

````shell
echo "alias testNagios='/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg'" >> /home/nagios/.bashrc
````

Attention à bien vérifier la syntaxe de cette commande (sur une seule ligne) et notamment les simples quotes et les doubles quotes.

Par ailleurs, lorsque vous allez changer la configuration de Nagios, en ajoutant/supprimant/modifiant les sondes, outre le fait de tester cette nouvelle configuration, il est nécessaire d’indiquer à Nagios de la prendre en compte. Cette opération peut être effectuée en rechargeant (reload ou restart) le service Nagios.

Cependant, l’utilisateur `nagios` n’a pas les droits de relancer le service Nagios, c’est normal, cette opération nécessite des privilèges que les comptes standards ne disposent pas par défaut.

Pour donner le droit au compte `nagios` de relancer le service Nagios, vous devez ajouter cette autorisation dans un fichier dédié à cet effet : `/etc/sudoers`
Ce fichier contient les règles à vérifier lorsqu’un compte standard demande des droits supplémentaires avec la commande sudo.

Exécutez la commande suivante : 

````shell
echo "nagios ALL=NOPASSWD:/bin/systemctl restart nagios" >> /etc/sudoers
````

Cette commande autorise le compte `nagios`, sans confirmer son mot de passe, à redémarrer le service Nagios sur le serveur.

Il vous reste désormais à créer le raccourci associé avec la commande suivante : 

````shell
echo "alias restartNagios='sudo systemctl restart nagios'" >> /home/nagios/.bashrc
````

Voilà, vous allez maintenant pouvoir tester ces deux alias.

Pour cela, passez sous le compte de `nagios` avec la commande suivante : 

````shell
su - nagios
````

Cette commande vous permet de prendre l’identité de l’utilisateur `nagios` en chargeant son environnement. Et comme vous l’exécutez depuis le compte root, vous n’avez pas besoin de fournir de mot de passe : 

````console
root@NagiosDebian:/etc/selinux# su - nagios
nagios@NagiosDebian:~$ 
nagios@NagiosDebian:~$ 
nagios@NagiosDebian:~$ 
````

Testez dans un premier le raccourci permettant de vérifier la configuration de Nagios, avec la commande suivante : 

````shell
nagios@NagiosDebian:~$ testNagios 
````

Le résultat doit être identique au précédent : 

````console
Total Warnings: 0
Total Errors:   0

Things look okay - No serious problems were detected during the pre-flight check
nagios@NagiosDebian:~$ 
````

Testez maintenant, toujours sous le compte `nagios`, la relance du service avec la commande suivante : 

````shell
nagios@NagiosDebian:~$ restartNagios
````

Cette commande ne doit renvoyer aucune sortie. Pour confirmer que le service vient d’être relancer, lancez la commande suivante : 

````shell
nagios@NagiosDebian:~$ systemctl status nagios | grep Active
````

Et vérifiez que le service est bien actif depuis quelques instants : 

````shell
nagios@NagiosDebian:~$ restartNagios 
nagios@NagiosDebian:~$ systemctl status nagios | grep Active
   Active: active (running) since Fri 2018-11-23 15:45:03 CET; 2s ago
nagios@NagiosDebian:~$ 
````

>VOILA ! Le premier chapitre de cette seconde partie a permis l’installation de Nagios, ce second chapitre celle de ses plugins standards ainsi que la mise en place des droits pour l’utilisateur `nagios`. ll est temps de s’intéresser maintenant au fonctionnement détaillé de ces outils. C’est ce que je vous propose de faire dans le chapitre suivant !


## Partie 2 Installez votre outil de supervision
### Chapitre 2.3. Comprenez le fonctionnement de Nagios.

>Vous avez précédemment installé Nagios et ses plugins standards. Vous avez pu également rapidement vérifier votre connexion à l’interface d’administration et notamment constater que Nagios disposait bien de ses plugins. Dans ce chapitre je vais détailler avec vous le principe de fonctionnement de Nagios, son fichier de configuration principal, ainsi que les différents fichiers sous-jacents. Ce chapitre vous permet également de préparer votre environnement de travail et le répertoire qui contiendra votre configuration spécifique. Enfin, j’aborderai la configuration détaillée par défaut de Nagios lui permettant de s’auto-superviser !

Il existe une page dédiée pour les « débutants » sur la documentation officielle de Nagios que je trouve très probante : [https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/beginners.html](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/beginners.html)

**[SCREENSHOT : 2.3-1_Beginners ]**

Prenez votre temps ! Vous verrez que Nagios possède un fonctionnement finalement assez simple, mais assez long à maitriser, et même une fois bien pratiqué, il réserve encore quelques pièges sympathiques ;
Lisez la documentation ! Elle est très fournie et très claire, c’est une force pour Nagios. N’hésitez pas à la consulter régulièrement, elle vous apportera beaucoup de solutions ;
Vous n’êtes pas seuls ! L’autre grande force de Nagios réside dans sa communauté, et si vous n’avez pas trouvé la solution dans la documentation, vous la trouverez très probablement en posant simplement la question : [https://support.nagios.com/forum/](https://support.nagios.com/forum/).

#### A. Recensez les différents éléments constituant de Nagios

Vous avez vu pendant le processus d’installation que le répertoire natif de Nagios est `/usr/local/nagios/`

Exécutez la commande suivante pour lister le contenu de ce répertoire :

````shell
ls -l /usr/local/nagios
````

Le résultat devrait produire quelque chose comme : 

````console
drwxrwsr-x  2 nagios nagios 4096 nov.  23 10:37 bin
drwxrwsr-x  3 nagios nagios 4096 nov.  23 11:02 etc
drwxr-sr-x  2 root   staff  4096 nov.  23 12:42 include
drwxrwsr-x  2 nagios nagios 4096 nov.  23 12:42 libexec
drwxrwsr-x  2 nagios nagios 4096 nov.  23 10:37 sbin
drwxrwsr-x 15 nagios nagios 4096 nov.  23 12:42 share
drwxrwsr-x  5 nagios nagios 4096 nov.  24 09:19 var
````

Vous connaissez déjà deux des éléments constituant cette arborescence : 

- Le répertoire `bin`, qui contient le programme principal de Nagios (ainsi qu’un autre programme permettant d’établir quelques statistiques) ;
- Le répertoire `libexec` qui contient l’ensemble des plugins standards Nagios compilés sur ce serveur.

Je vous propose maintenant de passer en revue les autres répertoires.

- Le répertoire `etc` contient l’ensemble de la configuration de Nagios, exécutez la commande suivante pour lister son contenu : 

````shell
ls -lRtha /usr/local/nagios/etc
````

Vous devriez obtenir la sortie suivante : 

````console
nagios@NagiosDebian:~$ ls -lRtha /usr/local/nagios/etc
/usr/local/nagios/etc:
total 84K
drwxr-sr-x 9 root   staff  4,0K nov.  23 12:42 ..
drwxrwsr-x 3 nagios nagios 4,0K nov.  23 11:02 .
-rw-r--r-- 1 root   nagios   50 nov.  23 11:02 htpasswd.users
drwxrwsr-x 2 nagios nagios 4,0K nov.  23 10:52 objects
-rw-rw---- 1 nagios nagios 1,3K nov.  23 10:52 resource.cfg
-rw-rw-r-- 1 nagios nagios  14K nov.  23 10:52 cgi.cfg
-rw-rw-r-- 1 nagios nagios  45K nov.  23 10:52 nagios.cfg

/usr/local/nagios/etc/objects:
total 60K
drwxrwsr-x 3 nagios nagios 4,0K nov.  23 11:02 ..
drwxrwsr-x 2 nagios nagios 4,0K nov.  23 10:52 .
-rw-rw-r-- 1 nagios nagios 3,0K nov.  23 10:52 printer.cfg
-rw-rw-r-- 1 nagios nagios 3,5K nov.  23 10:52 switch.cfg
-rw-rw-r-- 1 nagios nagios 4,7K nov.  23 10:52 localhost.cfg
-rw-rw-r-- 1 nagios nagios 3,5K nov.  23 10:52 timeperiods.cfg
-rw-rw-r-- 1 nagios nagios 4,0K nov.  23 10:52 windows.cfg
-rw-rw-r-- 1 nagios nagios 6,6K nov.  23 10:52 commands.cfg
-rw-rw-r-- 1 nagios nagios 1,8K nov.  23 10:52 contacts.cfg
-rw-rw-r-- 1 nagios nagios  13K nov.  23 10:52 templates.cfg
````

En racine du répertoire, vous retrouvez le fichier `htpasswd.users` que vous avez créé lors de l’installation de Nagios et qui permet de protéger l’accès à l’interface d’administration Web par un mot de passe htaccess.

Le fichier `nagios.cfg` est le fichier de configuration principal de Nagios. C’est ce fichier que le programme Nagios va lire en premier lieu lors de son démarrage et toute la configuration Nagios est incluse dans ce fichier ou dans d’autres fichiers référencés dans celui là !

Le fichier `resource.cfg` est un fichier de configuration référencé dans `nagios.cfg` contenant généralement des paramètres spécifiques à votre contexte nommés **USER MACROS**. Je reviendrai dessus un peu plus loin dans ce chapitre.

Enfin le fichier `cgi.cfg` est un fichier de configuration dédié à l’interface d’administration, vous y trouverez notamment quelques directives permettant de définir (plus ou moins) les droits des utilisateurs se connectant à l’interface.

Ensuite, vous trouvez le répertoire `objects`, qui contient lui même 8 fichiers de configuration Nagios avec une extension **.cfg**. Ces fichiers contiennent des modèles de configuration, fournis directement par Nagios, nommés **TEMPLATES**, permettant de factoriser beaucoup de travail et ainsi de gagner du temps sur la configuration et la maintenance de vos sondes. Par exemples, dans le fichier `printer.cfg` vous trouverez des modèles de configuration d’imprimantes, dans le fichier `switch.cfg` des modèles pour les équipements d’interconnexion, etc.

- Les répertoires `share` et `sbin` contiennent les éléments nécessaires à l’interface d’administration de Nagios.

Pour les lister, exécutez la commande suivante : 

````shell
nagios@NagiosDebian:~$ ls -l /usr/local/nagios/share/ /usr/local/nagios/sbin/
````

Le résultat de cette commande devrait produire la sortie suivante : 

````console
/usr/local/nagios/sbin/:
total 5332
-rwxrwxr-x 1 nagios nagios 345272 nov.  23 10:37 archivejson.cgi
-rwxrwxr-x 1 nagios nagios 318376 nov.  23 10:37 avail.cgi
[...]
-rwxrwxr-x 1 nagios nagios 261040 nov.  23 10:37 tac.cgi
-rwxrwxr-x 1 nagios nagios 277528 nov.  23 10:37 trends.cgi

/usr/local/nagios/share/:
total 176
drwxrwsr-x 4 nagios nagios 4096 nov.  23 10:37 angularjs
drwxrwsr-x 3 nagios nagios 4096 nov.  23 10:37 bootstrap-3.3.7
[...]
-rw-rw-r-- 1 nagios nagios 2990 nov.  23 10:37 trends.html
-rw-rw-r-- 1 nagios nagios 3586 nov.  23 10:37 trends-links.html
````

- Le répertoire `sbin`, contient les fichiers cgi compilés traitant de certaines fonctionnalités disponibles sur l’interface d’administration, par exemple, le fichier `cmd.cgi` va traiter les actions utilisateurs, le fichier `notifications.cgi` les notifications, `summary.cgi` l’affichage condensé des objets supervisés, etc.

- Le répertoire `share` contient lui le code des pages de l’interfaces Web, pour les plus développeurs d’entre vous, vous aurez éventuellement l’occasion d’aller modifier ces fichiers à la main afin de rendre l’interface un peu plus spécifique à votre besoin (il existe aussi des « **skins** » disponibles sur la plateforme [https://exchange.nagios.org](https://exchange.nagios.org) permettant de modifier les fichiers CSS par défaut de cette interface.). Vous remarquerez également que l’interface de la version 4 de Nagios inclus **bootstrap** et **angular JS** !

- Le répertoire `var` contient toutes les données variables maintenues par Nagios :

Pour lister le contenu de ce répertoire, lancez la commande suivante : 

````shell
nagios@NagiosDebian:~$ ls /usr/local/nagios/var/
````

Cette commande devrait fournir le résultat suivant : 

````console
drwxrwsr-x 2 nagios nagios  4096 nov.  24 00:00 archives
-rw-r--r-- 1 nagios nagios  1770 nov.  24 09:45 nagios.log
-rw-r--r-- 1 nagios nagios 12649 nov.  23 15:45 objects.cache
-rw------- 1 nagios nagios 13349 nov.  24 09:45 retention.dat
drwxrwsr-x 2 nagios nagcmd  4096 nov.  23 15:45 rw
drwxr-sr-x 3 root   nagios  4096 nov.  23 10:37 spool
-rw-rw-r-- 1 nagios nagios 13828 nov.  24 10:00 status.dat
````

Le fichier `nagios.log` est le fichier de trace du processus Nagios. 

Nagios écrit dans ce fichier dès qu’il se passe quelque chose, que ce soit un redémarrage du service, un équipement qui n’est plus joignable, ou un service qui passe en alerte. 
>Il est important de bien comprendre que Nagios n’écrira pas dans ce fichier lorsque tout va bien, mais uniquement lorsqu’un évènement se produit.

Nagios propose une rotation de son fichier de log, configurable dans son fichier `nagios.cfg`. Les archives des fichiers de trace seront conservées dans le répertoire `archives`.

Les deux fichiers suivants : `status.dat` et `retention.dat` sont un peu particuliers : 

Le fichier `status.dat` contient l’ensemble des données de supervision de Nagios, que soit sur le processus Nagios en tant que tel (nombres d’équipements / services supervisés, date de démarrage du programme, directives générales, etc.) mais aussi concernant tous les équipements et services supervisés (dernier statut détecté, temps de latence et durée d’exécution des sondes pour chaque équipement/service, etc.). C’est un fichier très important, il appartient à Nagios, vous ne devez pas le modifier à la main. Ce fichier est mis à jour selon une directive inscrite dans le fichier `nagios.cfg`.

Le fichier `retention.dat` est construit un peu sur le même principe que `status.dat`, mais ce fichier sera mis à jour moins souvent la plupart du temps car il ne sert qu’en cas de redémarrage du service Nagios. Le programme principal vient alors lire ce fichier et positionner le statut et les données des équipements/services à superviser par défaut selon leur valeur respective inscrite dans ce fichier. Cela permet d’éviter que toutes vos sondes soient remises à zéro entre chaque redémarrage du service ! Le comportement de Nagios vis à vis de ce fichier est également configuré dans `nagios.cfg`.

Le fichier `object.cache` fonctionne sur le même principe que ces deux précédents, mais contient simplement le cache de toute la configuration de Nagios. Vous ne devez pas intervenir directement dans ce fichier.

- Le répertoire `rw` contient notamment le **pipe** de Nagios, souvenez vous, vous avez rencontré ce répertoire lors du processus d’installation.

Exécutez cette commande pour lister ce répertoire : 

````shell
nagios@NagiosDebian:~$ ls -l /usr/local/nagios/var/rw/
````

Si votre service Nagios est actif, cette commande devrait renvoyer la sortie suivante : 

````console
nagios@NagiosDebian:~$ ls -l /usr/local/nagios/var/rw/
total 0
prw-rw---- 1 nagios nagcmd 0 nov.  23 15:45 nagios.cmd
srw-rw---- 1 nagios nagcmd 0 nov.  23 15:45 nagios.qh
````

Le fichier `nagios.cmd` est le « **pipe FIFO** » (*First In First Out*) de Nagios. C’est un fichier très important. Comme vous pouvez le constater par ces attributs Linux, le petit « **p** » indique que ce fichier est de type « *named pipe* ». Le principe est simple, c’est une connexion directe vers le processus Nagios qui va lire de manière continue les instructions passées dans ce fichier et les exécuter si elles sont compréhensibles. Les instructions passées dans le fichier sont nommées les **COMMANDES EXTERNES**, et c’est sur ce mécanisme que s’appuie l’interface d’administration de Nagios pour discuter avec son processus. Je vous en reparle dans le détail dans le chapitre 4.

Le fichier *nagios.qh* est une des principales nouveautés de la version 4 de Nagios. L’attribut Linux « **s** » indique que ce fichier est une « *socket* ». Ce fichier est notamment utilisé par les processus fils enfantés par Nagios pour discuter avec le processus principal, mais peut être également utilisé par un développement spécifique pour s’adresser à Nagios.

OK ! Vous avez fait le tour de l’arborescence Nagios, il est temps désormais de se pencher sur le fichier le plus important de cette arborescence : `nagios.cfg`

#### B. Maitrisez les principales directives du fichier de configuration de Nagios

Pour afficher les 20 premières lignes du fichier `nagios.cfg`, lancez la commande suivante : 

````shell
nagios@NagiosDebian:~$ head -20 /usr/local/nagios/etc/nagios.cfg 
````

Cette commande devrait produire la sortie suivante : 

````console
##############################################################################
#
# NAGIOS.CFG - Sample Main Config File for Nagios 4.4.2
#
# Read the documentation for more information on this configuration
# file.  I've provided some comments here, but things may not be so
# clear without further explanation.
#
#
##############################################################################


# LOG FILE
# This is the main log file where service and host events are logged
# for historical purposes.  This should be the first option specified
# in the config file!!!

log_file=/usr/local/nagios/var/nagios.log
````

La première directive présente dans le fichier `nagios.cfg` est le chemin vers le fichier de trace, ce qui parait logique car cela permet à Nagios au moins d’écrire la trace d’un évènement quel qu’il soit, y compris une erreur de configuration.

Les lignes qui débutent par le caractère « *#* » sont ignorées par le programme et servent à décrire l’objectif des directives via des commentaires au format texte.

Le fichier de configuration Nagios est dense, pour vous en rendre compte, lancez la commande suivante : 

````shell
nagios@NagiosDebian:~$ cat /usr/local/nagios/etc/nagios.cfg | wc -l
1378
````

Plus de 1300 lignes, c’est imposant !

Essayons désormais de relever uniquement les lignes qui ne sont pas des commentaires. Lancez la commande suivante : 

````shell
nagios@NagiosDebian:~$ cat /usr/local/nagios/etc/nagios.cfg | grep -e ^[^#] | wc -l
109
````

Bien, il ne reste « plus que 109 » lignes, soient 109 directives. Evidemment ce serait beaucoup trop long pour expliquer toutes ses directives, mais je vais vous présenter les plus importantes.

- **log_file**

Vous connaissez désormais cette directive qui indique à Nagios le chemin vers son fichier de trace. Il est possible de changer le chemin par défaut à condition que le compte `nagios` ait les droits d’écriture.

- **cfg_file**

Cette directive permet d’indiquer à Nagios de manière spécifique le chemin vers un fichier de configuration à prendre en compte au démarrage.

Exécutez la commande suivante : 

````shell
nagios@NagiosDebian:~$ grep ^cfg_file /usr/local/nagios/etc/nagios.cfg 
````

Vous pourrez alors constater que le fichier `nagios.cfg` référence 5 fichiers de configuration contenus dans le répertoire `/usr/local/nagios/etc/objects` :

````console
cfg_file=/usr/local/nagios/etc/objects/commands.cfg
cfg_file=/usr/local/nagios/etc/objects/contacts.cfg
cfg_file=/usr/local/nagios/etc/objects/timeperiods.cfg
cfg_file=/usr/local/nagios/etc/objects/templates.cfg
cfg_file=/usr/local/nagios/etc/objects/localhost.cfg
````

- **cfg_dir**

Cette directive a le même objectif que **cfg_file**, mais plutôt que de référencer les fichiers de manière individuelle, **cfg_dir** va référencer tous les fichiers avec une extension **.cfg** contenu dans le chemin fournit en paramètre, de manière récursive (en y incluant tous les répertoires et sous répertoires).

Par défaut, il n’y pas de répertoire référencé, donc vous allez créer le votre. Mais du coup la question se pose : où mettre votre répertoire de configuration spécifique ? C’est une question importante, notamment si vous devez un jour migrer de serveur Nagios.

Je vous conseille fortement de laisser les répertoires de l’arborescence Nagios inchangés et de créer votre propre répertoire de travail à la racine du répertoire d’installation de Nagios avec la commande suivante : 

````shell
nagios@NagiosDebian:~$ mkdir /usr/local/nagios/opencr_conf
````

Il vous reste maintenant à référencer ce répertoire dans le fichier de configuration Nagios, avec les commandes suivantes par exemple 
>**ATTENTION à bien indiquer DEUX chevrons pour AJOUTER ces lignes en fin de fichier** : 

````shell
nagios@NagiosDebian:/usr/local$ echo "#CONFIG OPENCLASSROOMS" >> /usr/local/nagios/etc/nagios.cfg 
nagios@NagiosDebian:/usr/local$ echo "cfg_dir=/usr/local/nagios/opencr_conf" >> /usr/local/nagios/etc/nagios.cfg
````

Vous pouvez vérifier avec la commande suivante : 

````shell
nagios@NagiosDebian:/usr/local$ tail /usr/local/nagios/etc/nagios.cfg 
#   jobs_min        The minimum amount of jobs to run at one time
[...]
#loadctl_options=jobs_max=100;backoff_limit=10;rampup_change=5
#CONFIG OPENCLASSROOMS
cfg_dir=/usr/local/nagios/opencr_conf
````

- **status_file** et **status_update**

Ces deux directives indiquent à Nagios le chemin vers le fichier status.dat que vous avez vu précédemment dans ce cours, ainsi que l’intervalle de temps en seconde entre chaque mise à jour de ce fichier.

````shell
nagios@NagiosDebian:/usr/local$ grep ^status_ /usr/local/nagios/etc/nagios.cfg 
status_file=/usr/local/nagios/var/status.dat
status_update_interval=10
````

Vous pouvez constater ici que le fichier `status.dat` est bien référencé et qu’il sera mise à jour toutes les 10 secondes (je reviendrai sur ces 10 secondes dans prochain chapitre).

- **resource_file**

Cette directive permet d’indiquer les fichiers contenant la définition des **USER MACROS** évoquées précédemment dans le cours. La commande suivante permet de visualiser la valeur par défaut de cette directive : 

````shell
nagios@NagiosDebian:/usr/local$ grep ^resource_ /usr/local/nagios/etc/nagios.cfg 
resource_file=/usr/local/nagios/etc/resource.cfg
````

Observez désormais le contenu du fichier passé en paramètre avec la commande suivante : 

````shell
nagios@NagiosDebian:/usr/local$ cat /usr/local/nagios/etc/resource.cfg
````

La documentation interne de ce fichier est très claire : 

````console
###########################################################################
#
# RESOURCE.CFG - Sample Resource File for Nagios 4.4.2
#
#
# You can define $USERx$ macros in this file, which can in turn be used
# in command definitions in your host config file(s).  $USERx$ macros are
# useful for storing sensitive information such as usernames, passwords,
# etc.  They are also handy for specifying the path to plugins and
# event handlers - if you decide to move the plugins or event handlers to
# a different directory in the future, you can just update one or two
# $USERx$ macros, instead of modifying a lot of command definitions.
#
# The CGIs will not attempt to read the contents of resource files, so
# you can set restrictive permissions (600 or 660) on them.
#
# Nagios supports up to 256 $USERx$ macros ($USER1$ through $USER256$)
#
# Resource files may also be used to store configuration directives for
# external data sources like MySQL...
#
###########################################################################

# Sets $USER1$ to be the path to the plugins
$USER1$=/usr/local/nagios/libexec

# Sets $USER2$ to be the path to event handlers
#$USER2$=/usr/local/nagios/libexec/eventhandlers

# Store some usernames and passwords (hidden from the CGIs)
#$USER3$=someuser
#$USER4$=somepassword
````

Elle indique notamment que vous disposez de 256 variables, nommées **$USER1$** à **$USER256$**, vous permettant de stocker des valeurs « constantes » dans votre configuration Nagios.

D’ailleurs, Nagios vous défini votre toute première macro **$USER1$** telle que : 

````console
$USER1$=/usr/local/nagios/libexec
````

Souvenez vous que ce répertoire contient l’ensemble des plugins standards compilés de Nagios. Vous pourrez donc adresser ce répertoire dans votre configuration directement en utilisant **$USER1$** lorsque ce sera nécessaire (et ce sera très souvent nécessaire). Le gros avantage de l’utilisation de cette macro, réside dans deux conséquences : 

Si vous êtes amenés à changer le répertoire des plugins pour x raisons, vous n’aurez alors que ce fichier à changer pour positionner la nouvelle valeur de **$USER1$** pour immédiatement migrer toute votre configuration.
Le second point relève de la sécurité et je vous en reparlerai lorsque vous verrez l’interface d’administration dans le détail.

- **command_file**, **check_external_commands**, **log_external_commands**

Exécutez la commande suivante : 

````shell
nagios@NagiosDebian:/usr/local$ grep command /usr/local/nagios/etc/nagios.cfg | grep -E ^[^#]
````

Ignorez la première ligne résultat de cette commande, vous connaissez déjà sa signification : 


````console
cfg_file=/usr/local/nagios/etc/objects/commands.cfg
check_external_commands=1
command_file=/usr/local/nagios/var/rw/nagios.cmd
log_external_commands=1
````

La directive **command_file** indique le chemin vers le « **pipe FIFO** » de Nagios ;
La directive **check_external_commands** indique si Nagios doit écouter ce pipe ;
La directive **log_external_commands** indique si Nagios doit tracer les commandes exécutées via son pipe.

Les 3 trois valeurs par défaut de ces directives sont interessantes et vont vous permettre de lancer des **COMMANDES EXTERNES**.

Voilà quelques directives importantes. Vous pouvez tout à fait consulter la signification de toutes les directives possibles en consultant la documentation en ligne : [https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/configmain.html](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/configmain.html)

>Lorsque vous serez en production avec un serveur Nagios supervisant beaucoup d’équipements et de services, je suis sur que vous irez notamment consulter les directives liées à la gestion des performances…

#### C. Comprenez comment Nagios se supervise lui même !

Avant de se plonger dans la configuration minimale de Nagios, il est important que vous compreniez son schéma de fonctionnement.

Le fonctionnement de Nagios peut se résumer à 4 éléments essentiels : 

- Les « **plugins** ». Vous les connaissez désormais, ce sont des programmes exécutables par Nagios, binaires compilés, script perl, script bash, etc.
- La « **command** ». Cet objet appartient à Nagios et permet de définir le lancement d’un plugin.
- Le « **host** ». Cet objet appartient à Nagios et représente tout équipement à superviser, possédant généralement une adresse IP et pouvant répondre aux pings ICMP standards.
- Le « **service** ». Cet objet appartient à Nagios et représente toutes les données à superviser sur un host, que ce soit des services réseaux, par exemple les ports ouverts d’un switch ou des services applicatifs, le service SSH ou HTTP d’un serveur, mais aussi la charge CPU, etc.

Ces éléments peuvent se schématiser tels que : 

**[SCREENSHOT : 2.2-2_NagiosSchema ]**

##### Les plugins 

Au plus bas niveau, se situe le plugin. Ce programme contient finalement toute l’intelligence de Nagios. C’est lui qui est responsable de contacter le host ou le service à superviser, de tester son statut et de récupérer les données associées.

La notion la plus importante à comprendre ici réside dans le fait que le plugin fonctionne de manière autonome et indépendante !

Par exemple, prenez le plugin `check_ping` et exécutez le sous le compte `nagios` (qui est le compte du service Nagios) de manière indépendante en lançant la commande suivante : 

````shell
nagios@NagiosDebian:/usr/local$ /usr/local/nagios/libexec/check_ping
````

Cette commande produit la sortie suivante :

````console
nagios@NagiosDebian:/usr/local$ /usr/local/nagios/libexec/check_ping
check_ping: Impossible de décomposer les arguments
Utilisation:
check_ping -H <host_address> -w <wrta>,<wpl>% -c <crta>,<cpl>%
 [-p packets] [-t timeout] [-4|-6]
````

Le plugin vous indique ici qu’il attend un minimum d’arguments, notamment l’adresse du host à vérifier (adresse IP ou nom si Nagios a accès à un DNS), ainsi que les deux arguments : 

« **w** », pour le seuil WARNING ;

« **c** », pour le seuil CRITICAL.

Ces deux arguments prennent pour valeur le temps nécessaire au retour du ping et/ou le pourcentage de paquets perdus que le réseau.

Relancez désormais cette commande avec les arguments tels que : 

````shell
nagios@NagiosDebian:/usr/local$ /usr/local/nagios/libexec/check_ping -H localhost -w 40,40% -c 60,60%
````

Le premier argument indique de vérifier le serveur « localhost » tel que défini dans le fichier `/etc/hosts` du serveur, le second argument indique un seuil WARNING à 40ms et/ou 40% de paquets perdus, et le troisième argument indique un seuil CRITICAL à 60ms et/ou 60% de paquets perdus. Bien entendu ces seuils ne devraient pas se déclencher avec une vérification sur localhost ! Observez le résultat de cette commande : 

````console
nagios@NagiosDebian:/usr/local$ /usr/local/nagios/libexec/check_ping -H localhost -w 40,40% -c 60,60%
PING OK -  Paquets perdus = 0%, RTA = 0.05 ms|rta=0.049000ms;40.000000;60.000000;0.000000 pl=0%;40;60;0
````

Vous pouvez constater que le plugin renvoie « OK » avec un certain nombre d’information, notamment le temps aller/retour de la requête ainsi que le pourcentage de paquet perdu. Ces informations sont comparés avec les seuils passés en paramètre afin de déterminer le statut de cette supervision. Vous comprenez ici que **ce sont bien les PLUGINS** de manière indépendante **qui DETERMINENT LE STATUT** de l’objet à superviser.

Second exemple, lancez la commande suivante afin de sonder le site [www.openclassrooms.fr](www.openclassrooms.fr) : 

````shell
nagios@NagiosDebian:/usr/local$ /usr/local/nagios/libexec/check_ping -H www.openclassrooms.fr -w 05,40% -c 20,60%
````

Ici les seuils WARNING et CRITICAL concernant le temps aller/retour de la requête sont volontairement baissés à 5ms et 20ms. Observez désormais le résultat de cette exécution : 

````console
nagios@NagiosDebian:/usr/local$ /usr/local/nagios/libexec/check_ping -H www.openclassrooms.fr -w 05,40% -c 20,60%
PING WARNING -  Paquets perdus = 0%, RTA = 10.69 ms|rta=10.685000ms;5.000000;20.000000;0.000000 pl=0%;40;60;0
````

Le plugin renvoie un status WARNING car le temps aller retour de la requête dépasse les 5ms mais reste inférieur à 20ms.

##### Les commandes

**[SCREENSHOT : 2.2-2_NagiosSchema ]**

Au dessus des plugins se trouvent les commandes Nagios. Les commandes Nagios se définissent de manière très simple par : 

- Le nom de la commande, avec l’idée de bien nommer ces commandes afin de comprendre immédiatement leur rôle uniquement en lisant leur nom ;
- Le chemin vers le plugin à lancer avec éventuellement les arguments associés.

Pour définir une commande qui exécuterait le plugin `check_ping` sur localhost tel que vu précédemment, ll faut ajouter la syntaxe suivante dans un fichier de configuration Nagios : 

````console
define command {
	command_name	check-ping-localhost
	command_line	/usr/local/nagios/libexec/check_ping -H localhost -w 40,40% -c 60,60%
}
````

C’est tout ! Comme vous pouvez le constater, le nom de la commande est très explicite, et la ligne de la commande correspond parfaitement à notre précédente exécution.

… Mais attendez voire, souvenez vous du point A de ce chapitre, et notamment de la notion de **USER MACROS**. Vous avez pu constater que Nagios proposait par défaut une première macro nommée **$USER1$** qui référençait justement le chemin vers les plugins. Vous pouvez donc transformer cette déclaration de commande telle que : 

````console
define command {
	command_name	check-ping-localhost
	command_line	$USER1$/check_ping -H localhost -w 40,40% -c 60,60%
}
````

Ainsi, si à l’avenir vos plugins changent de place, il vous suffit simplement de modifier le fichier resource.cfg sans devoir toucher à votre configuration de commande !

##### Les hosts

**[SCREENSHOT : 2.2-2_NagiosSchema ]**

Maintenant que vous disposez d’une commande vous permettant d’effectuer un `check_ping` avec vos propres arguments, il est nécessaire de définir l’objet Nagios host afin d’ajouter sa supervision.

L’objet host Nagios dispose d’un peu plus de directives que l’objet command. Elles sont disponibles et parfaitement documentées ici : [https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#host](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#host)

Cependant, vous allez devoir au moins définir les directives minimales suivantes : 

- **host_name** : c’est le nom de l’objet tel qu’il sera reconnu par Nagios, le champs est libre, mais il faut être le plus précis possible pour éviter les confusions ;
- **address** : l’adresse IP ou le nom résolu pour joindre l’objet (par exemple, locahost ou www.unsupersite.com, etc.)
- **max_check_attemps** : cette directive permet de spécifier à Nagios le nombre de tentatives successives maximum renvoyant un résultat négatif avant que Nagios considère ce résultat comme fiable et passe l’objet au statut déclenchant des notifications. C’est une directive très importante sur laquelle vous ne manquerez pas de vous documenter sérieusement, notamment pour maîtriser les notions de statut **SOFT** et **HARD**.
- **check_command** : c’est cette directive qui fait la liaison entre votre commande précédemment définie et cet objet host !

Bref ! Pour reprendre notre précédent exemple, il faut ajouter la syntaxe suivant dans un fichier de configuration Nagios : 

````console
define host {
	host_name			Nagios Server
	address				localhost
	check_command		check-ping-localhost
	max_check_attempts	3
}
````

##### Les services

**[SCREENSHOT : 2.3-2_NagiosSchema ]**

Dernier élément de configuration, les services sont des objets obligatoirement rattachés à un host.

Pour détailler leur mise en oeuvre, lancez dans un premier temps la commande suivante : 

````shell
nagios@NagiosDebian:~$ /usr/local/nagios/libexec/check_ssh 
````

Vous avez compris désormais que cette commande exécute un plugin Nagios. Et il est fort probable que ce plugin teste la réponse d’un service SSH. Elle doit renvoyer la sortie suivante : 

````console
nagios@NagiosDebian:~$ /usr/local/nagios/libexec/check_ssh 
check_ssh: Impossible de décomposer les arguments
Utilisation:
check_ssh  [-4|-6] [-t <timeout>] [-r <remote version>] [-p <port>] <host>
````

Le seul paramètre obligatoire indiqué par ce plugin est host. Relancez donc la commande avec ce paramètre fixé à la valeur locahost telle que : 

````shell
nagios@NagiosDebian:~$ /usr/local/nagios/libexec/check_ssh localhost
SSH OK - OpenSSH_7.4p1 Debian-10+deb9u4 (protocol 2.0) | time=0,005931s;;;0,000000;10,000000
````

Le plugin renvoie ici le status OK, le service SSH est bien disponible sur localhost.

De manière identique au host, pour superviser ce service, il faut commencer par créer la commande associée, par exemple : 

````console
define command {
	command_name	check-ssh-localhost
	command_line	$USER1$/check_ssh localhost
}
````

Ensuite, il faut maintenant définir un objet service. Cet objet possède également ses propres directives qui sont très bien documentées ici : [https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#service](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#service).

Voici la liste des directives minimale pour l’objet service : 

- **service_description** : c’est l’identifiant du service, il doit être unique pour chaque host ;
- **host_name** : c’est la référence à l’objet host hébergeant le service à superviser, ATTENTION la valeur de cette directive doit correspondre à celle de la directive host_name de l’objet host concerné ;
- **check_command** : cette directive permet d’indiquer quelle est la commande à exécuter pour effectuer la supervision ;
- **max_check_attempt** : possède la même signification que pour le host.

La définition d’un tel objet service relevant de l’exemple donnerait quelque chose comme : 

````console
define service {
        service_description     SSH sur Nagios Server
        host_name               Nagios Server
        check_command           check-ssh-localhost
        max_check_attempts      3
}
````

##### Très bien, maintenant APPLICATION !

Créez un nouveau fichier dans votre répertoire de travail `/usr/local/nagios/opencr_conf` nommé `nagios-server.cfg` avec la commande suivante : 

````shell
touch /usr/local/nagios/opencr_conf/nagios-server.cfg
````

Rassemblez les lignes précédemment vues dans cette partie C. dans ce fichier telles que  : 

````console
define command {
	command_name	check-ping-localhost
	command_line	$USER1$/usr/local/nagios/libexec/check_ping -H localhost -w 40,40% -c 60,60%
}

define command {
	command_name	check-ssh-localhost
	command_line	$USER1$/check_ssh localhost
}

define host {
	host_name	Nagios 	Server
	address			localhost
	check_command		check-ping-localhost
	max_check_attempts		3
}

define service {
	service_description	SSH sur Nagios Server
	host_name				Nagios Server
	check_command			check-ssh-localhost
	max_check_attempts		3
}
````

Enregistrez votre fichier et lancez la commande `testNagios` pour effectuer une première vérification  : 

````shell
nagios@NagiosDebian:~$ testNagios 
````
Le résultat de cette commande devrait ressembler à quelque chose comme : 

````console
Nagios Core 4.4.2
Copyright (c) 2009-present Nagios Core Development Team and Community Contributors
[...]
Total Warnings: 4
Total Errors:   0

Things look okay - No serious problems were detected during the pre-flight check
````

Vous allez constater 3 avertissements sur notre configuration minimale pour le service et 1 avertissement sur celle du host. C’est normal, certaines directives ne sont pas encore définies, mais ce n’est pas bloquant ! Vous pouvez relancer le service pour prise en compte avec la commande restartNagios.

````shell
nagios@NagiosDebian:~$ restartNagios 
nagios@NagiosDebian:~$ 
````

Si tout se passe bien, cette commande ne doit rien renvoyer en sortie.

Pour consulter la prise en compte de votre nouvelle configuration, rendez vous sur l’interface d’administration de Nagios. Dans un premier temps cliquez sur le lien « Hosts » à gauche de la page : 

**[SCREENSHOT : 2.3-3_NagiosInterfaceHosts ]**

Vous y retrouvez votre nouvel équipement supervisé « Nagios Server ».

Cliquez maintenant sur le lien « Services » à gauche de la page : 

**[SCREENSHOT : 2.3-4_NagiosInterfaceServices ]**

Vous y retrouverez la supervision de votre service « SSH sur Serveur Nagios » sur le host « Nagios Server ».

Vous venez d’ajouter votre serveur Nagios ainsi que son service SSH en supervision. Je vous invite désormais à étudier le contenu du fichier `/usr/local/nagios/etc/objets/localhost.cfg`

Vous allez découvrir la définition d’un host nommé localhost et de 8 services définis sur le même modèle que les vôtres. Vous venez de comprendre comment Nagios se supervise lui même !

````console
define host {
    use                     linux-server            
    host_name               localhost
    alias                   localhost
    address                 127.0.0.1
}
define service {
    use                     local-service 
    host_name               localhost
    service_description     PING
    check_command           check_ping!100.0,20%!500.0,60%
}
define service {
    use                     local-service           
    host_name               localhost
    service_description     Root Partition
    check_command           check_local_disk!20%!10%!/
}
[...]
````

Vous remarquerez cependant quelques différences dans la définition de la directive **check_command** avec notamment l’utilisation du caractère « **!** ». Je reviendrai sur ce point dans la 3ième partie de ce cours avec l’utilisation du plugin `check_http`.

>BIEN ! Vous connaissez maintenant le fonctionnement de Nagios, les éléments constituant son arborescence et les principales directives de son fichier de configuration. Vous avez même ajouté un nouvel équipement accompagné de son service à superviser et pu constater les premiers retours des sondes sur l’interface d’administration Nagios.
Je vous propose justement dans le prochain chapitre d’étudier dans le détail le fonctionnement de cette interface Web.

## Partie 2 Installez votre outil de supervision
### Chapitre 2.4. Administrez Nagios via son interface.

>A ce stade, vous possédez les premières briques de la configuration Nagios. Vous avez même ajouté un équipement et un service à superviser. Il est temps de se pencher un peu plus dans le détail de l’interface d’administration Nagios. Dans ce chapitre, je vous présente les différentes fonctionnalités de base de l’interface, avec les vues détaillées des équipements et des services supervisés. J’aborderai aussi également les actions possibles pour l’administrateur via cette interface et enfin vous verrez également les onglets plus « systèmes » mis à disposition par Nagios sur les pages de ce site.

L’interface d’administration de Nagios est fonctionnelle, mais elle reste le point faible de la solution Nagios Core. Même avec l’inclusion des dernières technologies comme Bootstrap et Angular JS, le développement de cette interface n’est pas la priorité des équipes de Nagios, et pour cause, la sortie de Nagios XI (payant sous licence) vient combler très justement ce manque et de quelle manière ! L’interface Nagios XI est juste magnifique, mais c’est pas donné...

Vous pourrez trouver des informations sur l’interface de Nagios XI ici : [https://www.nagios.com/products/nagios-xi/#features](https://www.nagios.com/products/nagios-xi/#features)

Quelques copies d’écran cependant : 

**[SCREENSHOT : 2.4-1_NagiosXI-1 ]**

**[SCREENSHOT : 2.4-2_NagiosXI-2 ]**

#### A. Faites le tour du propriétaire.

Lorsque vous accédez à l’interface d’administration de Nagios, la page d’accueil vous présente les menus de navigation sur la gauche, et le contenu principal de la page sur la droite : 

**[SCREENSHOT : 2.4-3_NagiosIHMAccueil ]**

##### Menu « General » 

Le menu « général » vous propose deux liens simples : 

Le lien « Home » : vous ramène à la page d’accueil ;
Le lien « Documentation » : vous amène sur la page officiel de la documentation Nagios Core version : [https://library.nagios.com/library/products/nagios-core/manuals/](https://library.nagios.com/library/products/nagios-core/manuals/)

**[SCREENSHOT : 2.4-4_NagiosDocumentationHome ]**

##### Menu « Current Status »

Ce menu se concentre sur les données de supervision des équipements et des services.

Le lien « Tactical Overview » vous permet d’obtenir une vue globale des objets supervisés, ainsi que quelques options de configuration.

**[SCREENSHOT : 2.4-5_NagiosTacticalOverview ]**

Le lien « Map » vous permet d’obtenir une vue en fonction de la topologie telle qu’elle est définie dans les objets Nagios. ATTENTION cette carte ne reflète pas votre topologie réelle sauf à la configurer intégralement dans Nagios.

**[SCREENSHOT : 2.4-6_NagiosMaps-1 ]**

Par défaut, les objets supervisés seront rattachés directement à votre serveur Nagios. C’est notamment le cas de votre host « Nagios Server » qui est au même niveau que le host « localhost » (qui est le même !).

La topologie Nagios se configure avec la directive « **parents** » au niveau des objets hosts. Il est possible d’indiquer plusieurs hosts dans cette directive, et souvent ce sont des équipements d’interconnexion comme des switchs, des routers, des firewalls, etc. 

C’est une directive TRES IMPORTANTE, car si Nagios détecte qu’un de ces équipements ne répond plus, il va passer tous les hosts et les services dont il est le parent dans un status « **UNREACHABLE** » ou « **UNKNOWN** » qui ne déclenche pas de notification.

Exemple, sur le schéma suivant disponible sur la documentation de Nagios : [https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/networkreachability.html](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/networkreachability.html)

**[SCREENSHOT : 2.4-7_NagiosParents ]**

Le Router1 est déclaré « **parents** » du Switch2 et du Router2. Lorsque le Router1 n’est plus joignable (le plugin `check_ping` renvoi **CRITICAL** par exemple), alors Nagios va déclarer Switch2 et Router2 dans un status **UNREACHABLE**, mais également tous les équipements qui en dépendent (c’est à dire ici Workstation1, HPLJ2605, et somesiteweb.com). Seuls les contacts associés à Router1 seront notifiés d’une alerte.

Pour la pratique, vous allez définir le host « localhost » en tant que parent du host « Nagios Server ». Pour cela, éditez le le fichier `/usr/local/nagios/opencr_conf/nagios-server.cfg` et ajoutez la directive **`parents`** dans l’objet host Nagios Server telle que : 

````console
define host {
        host_name       		Nagios Server
        address         		localhost
        check_command   		check-ping-localhost
        max_check_attempts     	3
        parents         		localhost
}
````

Vérifiez la configuration et relancez le service avec les commandes suivantes : 

````shell
testNagios
restartNagios
````

Revenez sur le lien « Map » pour observer la prise en compte de cette topologie : 

**[SCREENSHOT : 2.4-8_NagiosMaps-2 ]**

Vous pouvez constater désormais que « localhost » est bien le parent de « Nagios Server ».

##### Le menu Hosts

Le lien « Hosts », vous propose une vue d’ensemble des objets host supervisés par votre serveur Nagios.

**[SCREENSHOT : 2.4-9_NagiosHosts ]**

Les colonnes présentés affichent les informations suivantes : 

- **Host** : Le nom de l’objet tel que défini dans le fichier de configuration ;
- **Status** : L’état courant du host ;
- **Last Check** : La date de dernière exécution de la commande définie par la directive `check_command` du host ;
- **Duration** : Le temps cumulé de supervision du host ;
- **Status Information** : Les informations renvoyées par le plugin lors de sa dernière exécution.

>IMPORTANT : Vous venez de rencontrer pour la première fois la notion de « **CHECK** ». Au sens Nagios, un **check** est l’action d’exécuter la commande définie dans la directive **check_command** d’un objet, et donc de lancer le plugin défini dans cette commande.

En cliquant sur le lien affiché sous le nom du host, vous obtenez un écran d’affichage détaillé de l’objet : 

**[SCREENSHOT : 2.4-10_NagiosHost ]**

Le pavé « Host State Information » reprend les données de l’écran précédent et y ajoute quelques informations dont les suivantes : 

- **Performance Data** : ces données sont renvoyées de manière normalisée par le plugin et peuvent être interprétées par des moteurs de rendu graphique ;
- **Next Scheduled Active Check** : Indique le moment ou l’objet sera à nouveau supervisé via sa commande check_command et le plugin associé.
- **Last State Change** : Indique le moment ou l’objet a changé de statut pour la dernière fois.

Le pavé « **Hosts Commands** » permet d’interagir avec l’objet directement depuis l’interface d’administration via des **COMMANDES EXTERNES** (souvenez-vous le chapitre précédent avec le pipe FIFO Nagios) 

Pour illustrer cette fonctionnalité, vous allez demander à Nagios de ne pas attendre le prochain check de cet objet, mais d’effectuer ce check de manière immédiate. Pour cela, cliquez sur le lien « **Re-schedule the next check of this host** » :

**[SCREENSHOT : 2.4-11_NagiosIHMExternalCommand-1 ]**

Vous avez la possibilité de fixer le moment du prochain check sur un champs qui vous propose l’instant présent comme valeur par défaut. Il vous suffit simplement de cliquer sur le bouton « **commit** » pour exécuter le check.

Si tout s’est bien passé, vous devez obtenir le message de confirmation suivant : 

**[SCREENSHOT : 2.4-12_NagiosIHMExternalCommand-2 ]**

En cliquant sur le lien « **Done** » vous revenez à la vue détaillée de la supervision de ce host. Et vous pouvez constater l’exécution de votre commande externe en observant la nouvelle valeur du champs « **Last Check Time** » 

>IMPORTANT :  L’interface Nagios affiche les informations en se basant sur le fichier `/usr/local/nagios/var/status.dat` vu au chapitre précédent. Souvenez vous également de la valeur de la directive `status_update_interval=10` dans le fichier `nagios.cfg`. Il faut donc en comprendre que l’interface d’administration Nagios peut mettre au maximum 10 secondes à afficher le résultat de cette commande externe, c’est à dire le temps nécessaire maximum à Nagios pour écrire dans le fichier. Donc si le résultat de votre commande externe ne s’affiche pas immédiatement, patientez 10 secondes et rafraîchissez la page !

Pour aller au bout de ces histoires de commandes externes, vous allez répéter exactement la même opération depuis l’interface, mais en ajoutant une commande dans le terminal afin d’écouter sur le fichier de trace de Nagios en temps réel.

Exécutez dans un premier temps la commande suivante : 

````shell
nagios@NagiosDebian:~$ tail -f /usr/local/nagios/var/nagios.log 
````

Puis relancez un check immédiat sur le host localhost depuis le pavé de commandes de l’interface d’administration Nagios tel que vous venez de le faire. Vous devriez voir apparaitre la ligne suivante : 

````conole
[1543075682] EXTERNAL COMMAND: SCHEDULE_FORCED_HOST_CHECK;localhost;1543075681
````

Que vous dit cette ligne ? Et bien que Nagios vient d’exécuter au temps indiqué dans le premier champs entre crochets (qui représente le nombre de seconde écoulé depuis le 1er janvier 1970 à 00h00, le fameux timestamp des informaticiens) une commande externe nommée « **SCHEDULE_FORCE_HOST_CHECK** » prenant pour paramètre le host « localhost » et un nouveau timestamp indiquant le moment souhaité du check.

C’est à croire que le click sur le lien depuis le navigateur a envoyé cette commande à Nagios, et bien c’est exactement ce qui s’est passé ! Et comment votre navigateur a t’il envoyé cette commande à Nagios ? Vous l’aurez compris, en passant par le pipe FIFO de nagios (d’ou l’intérêt d’ajouter le compte www-data dans le groupe nagcmd lors de l’instantion de Nagios).

En résumé, le click sur le lien « **Re-schedule the next check of this host** » correspond à l’écriture dans le pipe FIFO de Nagios de la commande : 

**`[TIMESTAMP] SCHEDULE_FORCED_HOST_CHECK;localhost;TIMESTAMP`**

Et vous pouvez vérifier vous même cet état de fait en exécutant la commande suivante ( sur la même ligne ! ) : 

````shell
nagios@NagiosDebian:~$ DATE=`date +%s`;echo "[$DATE] SCHEDULE_FORCED_HOST_CHECK;"Nagios Server";$DATE" > /usr/local/nagios/var/rw/nagios.cmd 
````

Vérifiez dans le fichier trace de Nagios l’exécution de votre commande externe avec : 

````shell
nagios@NagiosDebian:~$ tail /usr/local/nagios/var/nagios.log 
[1543073348] Warning: Service 'SSH sur Nagios Server' on host 'Nagios Server' has no check time period defined!
[1543073348] Warning: Service 'SSH sur Nagios Server' on host 'Nagios Server' has no notification time period defined!
[1543073348] Warning: Host 'Nagios Server' has no default contacts or contactgroups defined!
[1543073348] Successfully launched command file worker with pid 29007
[1543075211] EXTERNAL COMMAND: SCHEDULE_FORCED_HOST_CHECK;localhost;1543075116
[1543075223] EXTERNAL COMMAND: SCHEDULE_FORCED_HOST_CHECK;localhost;1543075222
[1543075300] EXTERNAL COMMAND: SCHEDULE_FORCED_HOST_CHECK;localhost;1543075299
[1543075682] EXTERNAL COMMAND: SCHEDULE_FORCED_HOST_CHECK;localhost;1543075681
[1543076216] EXTERNAL COMMAND: SCHEDULE_FORCED_HOST_CHECK;localhost;1543076216
[1543076289] EXTERNAL COMMAND: SCHEDULE_FORCED_HOST_CHECK;Nagios Server;1543076289
````

La dernière ligne correspond à votre commande pour le host « Nagios Server » !

Vous avez désormais compris comment fonctionnait l’interface entre le site Web d’administration de Nagios et le service nagios. Toutes les commandes externes que vous allez rencontrer fonctionne sur le même principe.

>IMPORTANT : Les commandes externes Nagios sont normalisées et sont disponibles sur le site de la documentation : [https://assets.nagios.com/downloads/nagioscore/docs/externalcmds/](https://assets.nagios.com/downloads/nagioscore/docs/externalcmds/) et il y en a quelques unes !

**[SCREENSHOT : 2.4-13_NagiosIHMExternalCommand-3 ]**

##### Le menu Services

Le lien « Services », vous propose une vue d’ensemble des objets services supervisés par votre serveur Nagios.

**[SCREENSHOT : 2.4-14_NagiosServices ]**

Cet écran fonctionne sur le même principe que celui des hosts.

>ATTENTION toutefois, pour aller sur l’écran du détail d’un service, ll faut cliquer sur le lien SOUS LE CHAMPS SERVICE et non sous le champ HOST.

**[SCREENSHOT : 2.4-15_NagiosService ]**

Vous aurez l’occasion de découvrir les liens restants de ce menu : 

- Le lien « **Hosts Groups** » vous permet de visualiser la supervision des objets de type host par groupe. Les groupes se définissent également par des objets spécifiques : [https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#hostgroup](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#hostgroup)
- Le lien « **Service Groups** » fait la même chose pour les objets de type services : [https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#servicegroup](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#servicegroup)

Je reviendrai sur ces notions de groupe dans la 3 partie de ce cours.

Enfin, le lien « **Problems** » vous permet de recenser sur le même écran, les alertes en cours relevés par Nagios.


#### B. Exploitez les outils systèmes de l’interface d’administration.

L’interface d’administration de Nagios propose également à l’utilisateur quelques fonctionnalités système.

##### Le menu « Reports » 

Le menu « Reports » propose de construire quelques rapports en fonction des compteurs stockés par le moteur Nagios.

Le lien « **Availability** » permet d’établir des rapports proposant les pourcentages de temps sur les états pour les objets de type hosts ou services : 

**[SCREENSHOT : 2.4-16_NagiosHostStateBreakdowns ]**

**[SCREENSHOT : 2.4-17_NagiosServiceStateBreakdowns ]**

Le lien « **Trends** » propose de construire des rapports concernant l’historique d’évolution de l’état d’un objet host ou service : 

**[SCREENSHOT : 2.4-18_NagiosHostTrends ]**

Le lien « **Alerts** » affiche un condensé horodaté de tous les évènements survenus sur les objets services et hosts supervisés : 

**[SCREENSHOT : 2.4-19_NagiosAlerts ]**

Le lien « **Notifications** » affiche un résumé des différentes notifications générées par Nagios.

Enfin le lien « Event Log » affiche tout simplement tout le contenu du fichier de traces de Nagios `/usr/local/nagios/var/nagios.log` : 

**[SCREENSHOT : 2.4-20_NagiosEventLog ]**

##### Le menu « System » 

Le menu « System » est un menu important qui fourni beaucoup d’informations sur le comportement du serveur Nagios.

Le lien « **comments** » permet de recenser tous les commentaires déposés par les utilisateurs de l’interface d’administration sur les évènements constatés pour les objets supervisés.

**[SCREENSHOT : 2.4-21_NagiosComments ]**

Le lien « **Downtime** » offre la possibilité de définir une période de temps pendant laquelle Nagios ne tiendra pas compte des résultats des plugins pour les notifications normales, seules les notifications indiquant le départ et l’arrêt de ces périodes seront envoyées.

**[SCREENSHOT : 2.4-22_NagiosDowntime ]**

Le lien « **Process info** » permet d’obtenir des statistiques sur le fonctionnement de Nagios : 

**[SCREENSHOT : 2.4-23_NagiosProcessInfos ]**

Vous pouvez constater notamment que cette page contient un panel de « **Process Commands** » fonctionnant exactement comme les « **Host Commands** » et les « **Service Commands** ». C’est à dire qu’un click sur ces liens se traduit pas une commande externe envoyée sur le pipe FIFO de Nagios.

Notez notamment la possibilité :

- D’arrêter ou de redémarrer le service Nagios !
- De stopper toutes les sondes pour les objets de type host ;
- De stopper toutes les sondes pour les objets de type services ;
- Etc.

Le lien « **Performance Info** » affiche les informations de performance de Nagios : 

**[SCREENSHOT : 2.4-24_NagiosPerformanceInfo ]**

C’est écran est très important car il donne quelques indicateurs permettant de juger de la « santé » du serveur Nagios dont : 

- Le nombre de checks actifs pour les hosts par minute ;
- Le nombre de checks actifs pour les services par minute ; 
- Le temps d’exécution maximum pour les checks hosts et services.

Par exemple, sur cette page vous pouvez constater que le check d’un host prend 4 secondes ! C’est beaucoup étant donné que pour l’instant vous surveillez uniquement le serveur Nagios.

Comment faire pour trouver spécifiquement le check du host qui prend autant de temps ? Simple, lancez la commande suivante : 

````shell
nagios@NagiosDebian:~$ grep -n check_execution_time /usr/local/nagios/var/status.dat 
````

Observez le résultat de cette commande : 

````console
68:	check_execution_time=4.094
123:	check_execution_time=4.103
179:	check_execution_time=0.007
236:	check_execution_time=0.002
293:	check_execution_time=0.001
350:	check_execution_time=0.001
407:	check_execution_time=4.095
464:	check_execution_time=0.001
521:	check_execution_time=0.005
578:	check_execution_time=0.001
635:	check_execution_time=0.008
````

Vous pouvez constater que 3 sondes mettent plus de 4 secondes à s’exécuter. Consultez désormais ce fichier à partir du premier numéro de ligne renvoyée par la commande (ici 68) : 

````console
     56 hoststatus {
     57         host_name=Nagios Server
     58         modified_attributes=0
     59         check_command=check-ping-localhost
     60         check_period=
     61         notification_period=
     62         importance=0
     63         check_interval=5.000000
     64         retry_interval=1.000000
     65         event_handler=
     66         has_been_checked=1
     67         should_be_scheduled=1
     68         check_execution_time=4.094
````

Vous constatez alors que la sonde pour votre équipement « Nagios Server » met 4 secondes alors qu’elle est exécutée à partir de cet équipement !

Pour vérifiez ce temps d’exécution, relancez à la main le plugin effectuant cette sonde (souvenez vous le `check_ping` que vous avez configuré dans le fichier `/usr/local/nagios/opencr_conf/nagios-server.cfg`) et ajoutez devant la commande time permettant de relever le temps d’exécution telle que : 

````shell
nagios@NagiosDebian:~$ time /usr/local/nagios/libexec/check_ping -H localhost -w 40,40% -c 60,60%
PING OK -  Paquets perdus = 0%, RTA = 0.04 ms|rta=0.041000ms;40.000000;60.000000;0.000000 pl=0%;40;60;0

real	0m4,104s
user	0m0,000s
sys	0m0,000s
````

Effectivement, le ping du localhost prend 4 secondes !

Relancez le plugin avec son option `-help` pour comprendre ce phénomène et notamment intéressez vous à l’option **`-p`** telle que : 

````shell
nagios@NagiosDebian:~$ /usr/local/nagios/libexec/check_ping -help 
````

Et le texte décrivant l’option **`-p`** : 

````console
 -p, --packets=INTEGER
    nombre de paquets ICMP à envoyer (Défaut: 5)
 ````

Et voilà ! La sonde met 4 secondes car elle envoie 5 paquets ! Concernant un check sur la même machine, ce n’est pas forcément probant. Peut être peut on descendre le nombre de paquet à 1, mais alors, les seuils concernant le pourcentage de paquets perdus ne seront plus probants. Encore une fois pour un check local, ce n’est pas grave.

Relancez la commande telle que : 

````shell
nagios@NagiosDebian:~$ time /usr/local/nagios/libexec/check_ping -H localhost -w 40,40% -c 60,60% -p 1
PING OK -  Paquets perdus = 0%, RTA = 0.03 ms|rta=0.027000ms;40.000000;60.000000;0.000000 pl=0%;40;60;0

real	0m0,004s
user	0m0,000s
sys	0m0,000s
````

Bingo ! La sonde est bien plus rapide.

>A RETENIR DONC : Consultez régulièrement ce lien « Performance Info » et vérifiez que les valeurs importantes sont maitrisées !

>Attention aux options par défaut des plugins lors de l’exécution des sondes !

Il reste un dernier lien nommé « **Configuration** » que je ne vais pas détailler ici de manière volontaire. Pour comprendre le fonctionnement de cette fonctionnalité, rendez vous dans la partie 3 avec la mise en place d’une sonde HTTP.

Vous aurez remarqué qu’il n’est pas possible d’ajouter un objet à superviser directement depuis l’interface d’administration de Nagios Core. C’est son grand défaut. C’est aussi pour cette raison que la communauté a essayé de combler ce manque par des interfaces spécifiques qui ne sont plus vraiment maintenues à ce jour. Je pense notamment à NCONF qui a connu sa période de succès [https://exchange.nagios.org/directory/Addons/Configuration/NConf/details](https://exchange.nagios.org/directory/Addons/Configuration/NConf/details), ou encore Centreon qui s’est bien implanté sur le marché français justement en proposant une interface de configuration complète ! [https://www.centreon.com](https://www.centreon.com)

Il existe aussi quelques outils qui ont réussit à intégrer une interface complète, je pense notamment à Eyes Of Network que l’on peut également rencontrer dans quelques institutions publiques françaises. [https://www.eyesofnetwork.com/?lang=fr](https://www.eyesofnetwork.com/?lang=fr) . Cette solution intègre également d’autres outils interessant comme CACTI notamment, et c’est diffusé sous licence GPL Version 2 !

Mais, comme je l’indiquais en introduction de ce chapitre, la nouvelle interface d’administration web de Nagios XI est SA grande force, qui est à mon sens,  l’interface de configuration la plus aboutie. Mais il faut sortir le portefeuille...

>Voilà, le petit tour du propriétaire de l’interface d’administration Nagios est terminé. Vous avez compris comment fonctionne cette interface et notamment la notion de COMMANDE EXTERNE via le PIPE FIFO de Nagios. Vous savez également où trouvez les informations importantes sur cette interface et vérifiez les performances de vos sondes.

>Il est temps désormais de quitter le champs local de Nagios et de s’intéresser à la supervision d’un host distant accompagné de ses services. Je vous retrouve donc avec plaisir dans la 3ième partie de ce cours qui consiste à mettre en place la supervision d’un serveur Web !





<!--stackedit_data:
eyJkaXNjdXNzaW9ucyI6eyJNSnhMdGtBV0E4N1UyckMyIjp7In
RleHQiOiJgbmFnaW9zYCIsInN0YXJ0IjoyNzg1NSwiZW5kIjoy
Nzg1NX0sIlpPbUdyY3N2UTExbFBWZDMiOnsidGV4dCI6IltDZS
Bkb2N1bWVudF0oaHR0cHM6Ly9hc3NldHMubmFnaW9zLmNvbS9k
b3dubG9hZHMvbmFnaW9zeGkvZG9jcy9JbnN0YWxsaW5nLU5hZ2
nigKYiLCJzdGFydCI6Mjc4NTUsImVuZCI6Mjc4NTV9LCJVUWxR
YVV3T3JWazRFc1FtIjp7InRleHQiOiI+Kio6aW5mb3JtYXRpb2
5fc291cmNlOioqIiwic3RhcnQiOjI3ODU1LCJlbmQiOjI3ODU1
fSwiUWxEcWpCb25McEtIc3RtcyI6eyJ0ZXh0IjoiIyMjIyBQcs
OpcGFyZXogdW4gc2VydmV1ciBEZWJpYW4gw6AgcmVjZXZvaXIg
TmFnaW9zIiwic3RhcnQiOjI3ODU1LCJlbmQiOjI3ODU1fSwiTn
o4RTVKTGVXVHRwc2dHNiI6eyJ0ZXh0IjoiYGBgc2hlbGwiLCJz
dGFydCI6Mjc4NTUsImVuZCI6Mjc4NTV9LCJtUk5ibld0QW44S2
R4SWdBIjp7InN0YXJ0IjozMjUyLCJlbmQiOjMzOTMsInRleHQi
OiJQb3VyIGV4cGxvaXRlciBhdSBtYXhpbXVtIGxlcyBwb3NzaW
JpbGl0w6lzIG9mZmVydGVzIHBhciBsZXMgcGx1Z2lucyBkZSBO
YWdpb3Ms4oCmIn0sIm1EUWVudDZnT3NiWHdSb2wiOnsic3Rhcn
QiOjg1ODEsImVuZCI6ODY5OSwidGV4dCI6IkVuZmluIHBvdXIg
aW5zdGFsbGVyIE5hZ2lvcyBldCBzZXMgcGx1Z2lucyB2b3VzIG
F1cmV6IGJlc29pbiBkZXMgb3V0aWxzIGRlIGNvbXDigKYifSwi
QTZRYW9RbVJ5QmdvYlBKdyI6eyJzdGFydCI6OTcxMywiZW5kIj
o5NzI0LCJ0ZXh0IjoidXRpbGlzYXRldXIifSwicWpZQkZrWVVt
aVZjZFVXWCI6eyJzdGFydCI6MTc0NzQsImVuZCI6MTc1MDEsIn
RleHQiOiJDcsOpZXogbCdhcmJvcmVzY2VuY2UgTmFnaW9zIn19
LCJjb21tZW50cyI6eyJqRW1OaktqSDRkQmZ3VDA5Ijp7ImRpc2
N1c3Npb25JZCI6Ik1KeEx0a0FXQTg3VTJyQzIiLCJzdWIiOiJn
bzoxMDIxMjEwMjAyOTU2OTkxMzUzMzgiLCJ0ZXh0IjoiTGVzIH
RpbGRlcyAob3UgYXBvc3Ryb3BoZXMgw6AgbCdlbnZlcnMpIHNv
bnQgYmllbiB1dGlsZXMgcG91ciBtZXR0cmUgZW4gYXZhbnQgZG
VzIG1vdHMgb3UgZ3JvdXBlcyBkZSBtb3RzIHF1aSBmb250IHBh
cnRpZSBkZSBsYSBzeW50YXhlIGQndW4gbGFuZ2FnZSBkZSBwcm
9ncmFtbWF0aW9uLCBvdSBkZXMgbW90cyAvIG5vbXMgZGUgdmFy
aWFibGVzIC8gY29tbWFuZGVzIHF1ZSBsJ3V0aWxpc2F0ZXVyIH
V0aWxpc2VyYSBkaXJlY3RlbWVudCBkYW5zIHNvbiBjb2RlIG91
IHNlcyBjb21tYW5kZXMuIiwiY3JlYXRlZCI6MTU0MzI0NTg4OT
QzMn0sIldDYkl2TUlVN3p5bjFoTm8iOnsiZGlzY3Vzc2lvbklk
IjoiWk9tR3Jjc3ZRMTFsUFZkMyIsInN1YiI6ImdvOjEwMjEyMT
AyMDI5NTY5OTEzNTMzOCIsInRleHQiOiJUdSBwZXV4IHV0aWxp
c2VyIGNldHRlIHN5bnRheGUgcG91ciBpbnPDqXJlciB1biBsaW
VuIGRhbnMgdW4gY2hhbXBzIGRlIHRleHRlLiBNYWlzIHNpIHR1
IHNvdWhhaXRlcyBzaW1wbGVtZW50IGFmZmljaGVyIGxlIGxpZW
4sIHR1IHBldXggbGUgY29waWVyLWNvbGxlciBkaXJlY3RlbWVu
dC4iLCJjcmVhdGVkIjoxNTQzMjQ4MDI4MTY2fSwiVE0xY0VxMV
ZzS1hQM0RubCI6eyJkaXNjdXNzaW9uSWQiOiJVUWxRYVV3T3JW
azRFc1FtIiwic3ViIjoiZ286MTAyMTIxMDIwMjk1Njk5MTM1Mz
M4IiwidGV4dCI6IkNlY2kgcGVybWV0IGQnaW5kaXF1ZXIgdW5l
IGJhbGlzZSBpbmZvcm1hdGlvbiBxdWUgbm91cyB1dGlsaXNvbn
Mgc291dmVudCBkYW5zIGxlcyBjb3VycyBPcGVuQ2xhc3Nyb29t
cyBwb3VyIG1ldHRyZSBlbiB2YWxldXIgZGVzIHBhcmFncmFwaG
VzIGltcG9ydGFudHMuIiwiY3JlYXRlZCI6MTU0MzI0OTk4NDM3
OX0sInJ4ZjB5ekFiakl3RjBwYjIiOnsiZGlzY3Vzc2lvbklkIj
oiUWxEcWpCb25McEtIc3RtcyIsInN1YiI6ImdvOjEwMjEyMTAy
MDI5NTY5OTEzNTMzOCIsInRleHQiOiJDZWNpIGVzdCB1biB0aX
RyZSBkZSBzZWN0aW9uLiBJY2ksIHR1IHBldXggbGVzIG1hcnF1
ZXIgYXZlYyA0IyAoIyMjIykuIiwiY3JlYXRlZCI6MTU0MzI1MD
E3ODczN30sIjFNM2Z0NFJta1p1akNxaDkiOnsiZGlzY3Vzc2lv
bklkIjoiVVFsUWFVd09yVms0RXNRbSIsInN1YiI6ImdvOjEwMj
EyMTAyMDI5NTY5OTEzNTMzOCIsInRleHQiOiJBdHRlbnRpb24s
IGxlIHJlbmR1IHN1ciBTdGFja0VkaXQgbidlc3QgcGFzIGZpZM
OobGUgYXUgcmVuZHUgT0MuIEFzc3VyZSB0b2kgc2ltcGxlbWVu
dCBxdWUgdG91dCBsZSB0ZXh0ZSBxdWUgdHUgc291aGFpdGVzIG
luY2x1cmUgw6AgbGEgYmFsaXNlIGVzdCBpbmRlbnTDqSAoY29t
bWUgdW5lIHF1b3RlKSBldCBxdWUgbCdpY29uZSBkJ2luZm9ybW
F0aW9uIGFwcGFyYWl0IGNvcnJlY3RlbWVudCBhdSBkw6lidXQg
ZHUgcGFyYWdyYXBoZS4iLCJjcmVhdGVkIjoxNTQzMjUwNTAxOD
g0fSwiNWRMZlhMUzZUeHlZc1ZNMiI6eyJkaXNjdXNzaW9uSWQi
OiJOejhFNUpMZVdUdHBzZ0c2Iiwic3ViIjoiZ286MTAyMTIxMD
IwMjk1Njk5MTM1MzM4IiwidGV4dCI6IlBvdXIgbWFycXVlciB1
bmUgc2VjdGlvbiBjb21tZSDDqXRhbnQgZHUgY29kZSwgZW5jYW
RyZSBsZSBwYXJhZ3JhcGhlIHBhciAzIGAgc3VpdmlzIGR1IG5v
bSBkdSBsYW5nYWdlLiBEYW5zIHRvbiBjb3VycywgY2Ugc2VyYS
BzdXJlbWVudCB1bmlxdWVtZW50IFwic2hlbGxcIiBwb3VyIGxl
cyBlbnRyw6llcyB1dGlsaXNhdGV1ciBldCBcImNvbnNvbGVcIi
Bwb3VyIGxlcyBzb3J0aWVzIGR1IHRlcm1pbmFsLiBcblxuUGVu
c2Ugw6AgYmllbiBmYWlyZSBjZXR0ZSBkaXN0aW5jdGlvbiBjYX
IgZWxsZSBtb250cmVyYSBiaWVuIMOgIGwnw6l0dWRpYW50IHF1
ZSBsZXMgMiBhZmZpY2hhZ2VzIG9udCBkZXMgc2lnbmlmaWNhdG
lvbnMgZGlmZsOpcmVudGVzIGV0IGx1aSBmYWNpbGl0ZXJhIGwn
YXBwcmVudGlzc2FnZS4iLCJjcmVhdGVkIjoxNTQzMjUwNzk0OT
gyfSwiVE5zdkUwQWZkUXlpNEhWUyI6eyJkaXNjdXNzaW9uSWQi
OiJtUk5ibld0QW44S2R4SWdBIiwic3ViIjoiZ286MTAyMTIxMD
IwMjk1Njk5MTM1MzM4IiwidGV4dCI6IkVzdCljZSBxdWUgdHUg
cGVuc2VzIHF1ZSDDp2EgdmF1ZHJhaXQgbGUgY2/Du3QgZGUgcs
Opc3VtZXIgcmFwaWRlbWVudCDDoCBxdW9pIHNlcnZlbnQgY2Vz
IGxpYnJhaXJpZXMgPyBTYW5zIGVudHJlciBkYW5zIGxlcyBkw6
l0YWlscywgbWFpcyBwYXIgZXhlbXBsZSBwb2ludGVyIHVuZSBm
b25jdGlvbm5hbGl0w6kgZW4gcGFydGljdWxpZXIgcXVpIGxlcy
Buw6ljZXNzaXRlLiBFc3QtY2UgcXVlIHR1IHZhcyBsJ3V0aWxp
c2VyIGRhbnMgY2UgY291cnMgP1xuUGVuc2VzLXR1IHF1ZSBjZS
Bzb2l0IHBlcnRpbmVudCBkZSBwcsOpY2lzZXIgw6dhID8iLCJj
cmVhdGVkIjoxNTQ0NTQ1NTE0MDMwfSwiSVY0cEQ2SlBvUTZuUE
FoWSI6eyJkaXNjdXNzaW9uSWQiOiJtRFFlbnQ2Z09zYlh3Um9s
Iiwic3ViIjoiZ286MTAyMTIxMDIwMjk1Njk5MTM1MzM4IiwidG
V4dCI6IkVuY29yZSB1bmUgZm9pcywgaWwgbWUgc2VtYmxlIHF1
ZSDDp2EgdmF1ZHJhaXQgbGUgY291cCBkJ2V4cGxpcXVlciBlbi
B1bmUgcGhyYXNlIMOgIHF1b2kgw6dhIHNlcnZpcmEsIHBvdXIg
w6l2aXRlciBxdWUgbCfDqXR1ZGlhbnQgbidpbnN0YWxsZSBkZX
MgcGFja2FnZXMgw6AgbCdhdmV1Z2xlLiBRdSdlbiBwZW5zZXMt
dHUgPyIsImNyZWF0ZWQiOjE1NDQ1NDU2MTQyNjd9LCJNQmhvR0
xHQ0hKY3NDZlU3Ijp7ImRpc2N1c3Npb25JZCI6IkE2UWFvUW1S
eUJnb2JQSnciLCJzdWIiOiJnbzoxMDIxMjEwMjAyOTU2OTkxMz
UzMzgiLCJ0ZXh0IjoiRGUgY2UgcXVlIGplIHZvaXMsIGxlIGNv
bmNlcHQgZOKAmXV0aWxpc2F0ZXVyIGljaSBlc3QgYXNzZXogcG
FydGljdWxpZXIuIEEgcXVvaSBzZXJ0LWlsID8gUXVlIHJlcHLD
qXNlbnRlLXQtaWwgPyBJbCBtZSBzZW1ibGUgcXXigJlpbCBmYX
VkcmFpdCBiaWVuIGTDqWZpbmlyIGNlIGNvbmNlcHQgYXZhbnQg
ZGUgY29udGludWVyLiBRdeKAmWVuIHBlbnNlcy10dSA/IiwiY3
JlYXRlZCI6MTU0NDU0NjE3NjA5NX0sIlJ5ZWNtYWRVVGR6QzBO
dGsiOnsiZGlzY3Vzc2lvbklkIjoicWpZQkZrWVVtaVZjZFVXWC
IsInN1YiI6ImdvOjEwMjEyMTAyMDI5NTY5OTEzNTMzOCIsInRl
eHQiOiJDZWNpIGVzdCB1biBzb3VzLXRpdHJlIHF1aSB2aWVudC
DDoCBs4oCZaW50w6lyaWV1ciBk4oCZdW5lIHNlY3Rpb24uIEri
gJlhaSByZW1wbGFjw6kgdG91cyB0ZXMgdGlyZXRzIHBhciBjZX
Mgc291cy10aXRyZXMgbMOgLiBKZSB04oCZaW52aXRlIMOgIGNv
bnRpbnVlciBkZSBsZXMgdXRpbGlzZXIgcG91ciBxdWUgY2Ugc2
9pdCB2aXN1ZWxsZW1lbnQgY2xhaXIgcG91ciBs4oCZYXBwcmVu
YW50IDopIiwiY3JlYXRlZCI6MTU0NDU0NjI0NDU4OX19LCJoaX
N0b3J5IjpbODM4NTU3Mjg1LDEwODcxMzgwNjddfQ==
-->