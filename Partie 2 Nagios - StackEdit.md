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

>Voilà, le petit tour du propriétaire de l’interface d’administration Nagios est terminé. Vous avez compris comment fonctionne cette interface et notamment la notion de **commande externe** via le ***pipe FIFO*** de Nagios. Vous savez également où trouvez les informations importantes sur cette interface et vérifiez les performances de vos sondes.

>Il est temps désormais de quitter le champs local de Nagios et de s’intéresser à la supervision d’un host distant accompagné de ses services. Je vous retrouve donc avec plaisir dans la troisième partie de ce cours qui consiste à mettre en place la supervision d’un serveur Web !





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
N0b3J5IjpbMjAyNDM4ODE4Nyw0NDU2MTUyMzAsLTk2NDEwMDYw
MSwxMzQwMDEzMTExLC0xMDAzNTMzOTE2LDgzODU1NzI4NSwxMD
g3MTM4MDY3XX0=
-->