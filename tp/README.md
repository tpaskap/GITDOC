# TP: D�veloppement collaboratif d�centralis� avec GIT

## Sommaire
**[Installation de git](#installation-de-git)**

**[Installation outils graphiques](#installation-outils-graphiques)**

**[Customisation environnement](#customisation-environnement)**

**[Ajouter .bashrc](#ajouter-bashrc)**

**[Ajouter .gitconfig](#ajouter-gitconfig)**

**[Ajout .gitignore](#ajout-gitignore)**

**[Rappels des fondamentaux](#rappels-des-fondamentaux)**

**[1 Publication de revisions](#1-publication-de-revisions)**

**[2 Branches de developpement](#2-branches-de-developpement)**

**[3 Synchronisation de plusieurs repositories](#3-synchronisation-de-plusieurs-repositories)**

**[4 Modifications publiees, modifications non publiees](#4-modifications-publiees-modifications-non-publiees)**

**Note:** Pour convertir ce fichier en pdf : http://dillinger.io, Top menu > Utilities > Export as PDF File

## Installation de git
```sh
$ sudo apt-get install git
```

## Installation outils graphiques
```sh
$ sudo apt-get install gitk
$ sudo apt-get install git-gui
$ sudo apt-get install gitg
$ sudo apt-get install meld
```

## Customisation environnement
```sh
$ cd ~
$ git clone https://github.com/magicmonty/bash-git-prompt.git .bash-git-prompt
```

### Ajouter .bashrc
    GIT_PROMPT_ONLY_IN_REPO=1
    if [ -f ~/.bash-git-prompt/gitprompt.sh ]; then
          . ~/.bash-git-prompt/gitprompt.sh
    fi

### Ajouter .gitconfig
Ajouter dans ~/.gitconfig

     [user]
     	name = Prenom Nom
     	email = monaddresse@mail.fr

     [alias]
      co = checkout
      ci = commit
      st = status
      br = branch
      hist = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
      type = cat-file -t
      dump = cat-file -p


### Ajout .gitignore
Pour chaque copie de travail ajouter dans .gitignore

     [._]*.s[a-w][a-z]
     [._]s[a-w][a-z]
     *.un~
     Session.vim
     .netrwhist
     *~

## Rappels des fondamentaux

Git est un logiciel de contr�le de versions d�centralis�. Contrairement � Subversioni ou CVS, qui se base sur un unique d�p�t avec lequel se synchronisent une ou plusieurs copies de travail (les copies de travail ne peuvent se synchroniser entre elles : elles doivent passer par le d�p�t), chaque copie de travail d'un projet version� avec Git joue aussi le r�le de d�p�t, et il est possible de synchroniser entre elles n'importe quelles copies de travail. De plus, Git permet d'utiliser une ou plusieurs branche de d�veloppement et de fusionner entre elles ces branches de d�veloppement.

### 1 Publication de revisions
Nous allons tout d'abord nous int�resser � l'aspect gestionnaire de version de Git: comment enregistrer l'historique des modifications apport�es � un projet. Pour initialiser un repository, il convient d'invoquer la commande
```sh
$ git init monrepo
```
Cette commande initialise un d�p�t Git dans le repertoire monrepo (qui est cr�e si celui-ci n'existe pas). Ce r�pertoire contient alors � la fois une version de travail (dans monrepo) et un repository Git (dans monrepo/.git). Bien que Git ait �t� con�u pour g�rer du code source, nous allons nous en servir dans ce TP pour g�rer des fichiers textes simples, pour nous concentrer sur le fonctionnement de Git plut�t que sur du code.

#### Question 1.1.
Initialiser un d�p�t Git, et cr�ez le fichier burger.txt � la racine du repo qui contient la liste des ingredients d'un burger (un ingredient par ligne).

    steak
    salade
    tomate
    cornichon
    fromage

Git a plusieurs interfaces utilisateur, la plus compl�te �tant l'interface en ligne de commande (CLI), mais nous allons aussi utiliser Gitg ou Gitk qui sont des interface graphique � Git.

Depuis le r�pertoire de votre d�p�t, lancez Gitg (ou lancez Gitg puis ouvrez votre d�p�t).

Gitg a deux onglets History et Commit. Dans l'onglet Commit, on remarque 4 cadres :
 - Unstaged qui contient la liste des modifications qui ont �t� apport�es dans le d�p�t et qui n'ont pas �t� s�lectionnees pour �tre commit�es.
 - Staged qui contient la liste des modifications qui ont �t� apport�es et qui ont �t� s�lectionn�es pour �tre commitees.
 - Changes qui affiche une modification
 - Commit message qui contient le message du commit courant

#### Question 1.2.
Selectionnez votre fichier burger.txt comme modication � �tre commit�e; editez un message de commit, puis commitez. Retournez dans l'onglet History pour observer votre commit.

#### Question 1.3.
Rajoutez un ingredient dans burger.txt, puis creez quelques autres sandwich.txt et commitez toutes ces modications. Regardez l'onglet History, votre deuxieme commit doit appara�tre.

#### Question 1.4.
Creez un nouveau sandwich, et modiifiez un sandwich existant. Nous allons commiter ces changements avec l'interface en ligne de commande. Commencons par taper
```sh
$ git commit
```
Que se passe-t-il ? Lisez le paragraphe DESCRIPTION de la page de manuel git-commit(1) (ce que l'on peut faire soit en invoquant man git-commit, soit git help commit). Commitez ces changements. Observer l'arbre de commit dans l'onglet History de Gitg (Attention, il faudra sans doute rafra�chir avec Ctrl+R).

### 2 Branches de developpement
La partie 1 pr�sentait l'utilisation simple de Git pour creer un historique des modications. Nous allons maintenant nous concentrer sur la notion de branche. Lors du d�veloppement d'un projet, il peut arriver que l'on veuille introduire 1 une nouvelle fonctionnalite dans le projet, sans "casser" le projet. Nous voudrions donc pouvoir basculer instantanement de la version stable du projet � sa version "en d�veloppement". C'est ce que nous permettent de faire les branches.

#### Question 2.1.
Cr�ez une nouvelle branche intitul�e "developpement" dans votre repository. Avec Gitg, dans l'onglet History, selectionnez le dernier commit (attention l'�tiquette
master repr�sente la branche master), clic-droit puis Create New Branch.

Nous avons donc cree une nouvelle branche, qui est pour l'instant la m�me que la branche principale. Il est possible de basculer d'une branche � l'autre en cliquant droit sur la branche et selectionnant Checkout working copy.

#### Question 2.2.
Dans la branche developpement effectuez quelques modifications (modications dans les fichiers ou ajout/suppression de fichier), puis commitez-les. Observez ce qu'il se passe dans l'onglet History. (Selectionnez l'affichage de toutes les branches).
En ligne de commande, pour afficher la branche courante, il suffit d'invoquer git branch. Une branche est cr��e � partir d'une autre au moyen de
```sh
$ git branch MANOUVELLEBRANCHE
```
, et le passage d'une branche � l'autre se fait au moyen de

```sh
$ git checkout MABRANCHE
```

#### Question 2.3.
Constatez les diff�rences dans chacune des deux branches. Jusqu'� pr�sent nous avons suivi le sc�nario simple ou il y a une branche de d�veloppement, et une branche stable. Supposons que les modifications dans la branche de d�veloppement soient finies et que nous voulions nous lancer dans de nouvelles modifications, il convient de synchroniser les deux branches. Cela s'appelle une fusion de branches (merge en anglais).

#### Question 2.4.
Essayez sous Gitg, de fusionner la branche master avec la branche d�veloppement. Est-ce le r�sultat attendu ?
Ce scenario simple ne s'applique pas toujours : pendant que des modifications sont effectuees dans la branche de d�veloppement, il peut falloir aussi effectuer des correctifs mineurs dans la branche stable.

#### Question 2.5.
Effectuez (et commitez) des modications dans la branche d�veloppement et d'autres modifications
dans la branche master (attention � ce que ces modifications ne soient pas conflictuelles). Qu'est-ce qui est affich� dans l'onglet History de Gitg ?
Il est bien entendu possible de fusionner deux branches avec l'interface en ligne de commande. Lisez le paragraphe DESCRIPTION dans la page de manuel de git-merge(1)

#### Question 2.6.
Avec l'interface en ligne de commande, fusionnez les branches master et developpement.

Jusqu'� present, nous n'avons envisage que des scenarios dans lequels la fusion des branches est simple, mais il peut y arriver qu'il y ait des conflits, par exemple un m�me bogue corrige de mani�re sensiblement diff�rente dans deux branches diff�rentes.

#### Question 2.7.
Que se passe-t-il dans ce cas-l� ? Essayez d'implementer ce scenario. Comment Git vous permet-il de resoudre les conflits ?
Ecrase-t-il unilateralement les modications effectu�es dans une branche ?

### 3 Synchronisation de plusieurs repositories
Jusqu'� pr�sent, nous avons vu quelques fonctionalit�s de Git sans nous interesser son aspect collaboratif.
Git permet un travail collaboratif sur un d�p�t. C'est-�-dire qu'il est possible de synchroniser entre elles des branches de deux d�p�ts diff�rents.

#### Question 3.1.
Creez un nouveau d�p�t (avec git init --bare)).
```sh
$ mkdir $PATH_TO_REPO2
$ git init --bare
```
Ceci initialise un d�p�t Git sans copie de travail. Il y a deux facons de synchroniser entre eux deux d�p�ts :
- soit en r�cup�rant les commits du d�p�t distant (pull)
- soit en envoyant des commits vers le d�p�t distant (push).
Dans le deuxieme cas, il faut que le d�p�t distant soit un d�p�t bare, c'est � dire sans copie de travail.

#### Question 3.2.
Envoyez les commits de votre premier d�p�t vers le second avec les commandes (executees depuis votre premier d�p�t _monrepo_) :
```sh
$ git push file://$PATH_TO_REPO2 master:master
```
Cette commande va envoyer la branche master du premier d�p�t dans une branche appelee master dans le second d�p�t.
La seconde va effectuer la m�me chose avec la branche d�veloppement.
```sh
$ git push file://$PATH_TO_REPO2 developpement:developpement
```
Observez le r�sultat en lancant Gitg depuis le second d�p�t.

Pour rendre la synchronisation plus int�ressante, nous allons utiliser une deuxi�me copie de travail.

#### Question 3.3.
Cr�ez une nouvelle copie de travail � partir du d�p�t bare:
```sh
$ git clone file://$PATH_TO_REPO2 copietravail
```
qui cr�e une copie de travail du second d�p�t dans le r�pertoire _copietravail_.

#### Question 3.4.
Effectuez quelques modifications dans votre premi�re copie de travail. 
Propagez ces modications dans votre troisi�me d�p�t : _copietravail_.
- Envoyez ces modifications dans le second d�p�t (avec git push file://PATH_TO_REPO2 BRANCHE:BRANCHE)
- Puis depuis le troisi�me d�p�t, r�cup�rez avec git pull, ces modications depuis le second d�p�t. (Comme le troisieme d�p�t a �t� cr�� � partir du second au moyen de git clone, il n'est pas n�cessaire de pr�ciser ici ou Git doit chercher les commits)

Nous avons mis en place avec le second et troisieme d�p�t le sch�ma de collaboration avec Git le plus courant : il
y a un d�p�t qui fait office de d�p�t ma�tre, et le troisieme d�p�t qui peut r�cup�rer et envoyer des commits sur le d�p�t ma�tre. Nous allons maintenant nous int�resser � l'acc�s concurrent � ce d�p�t ma�tre.

#### Question 3.5.
Effectuez des modifications dans le premier d�p�t, et envoyez ces modifications dans le second d�p�t. Sans synchroniser le troisi�me d�p�t avec le second, effectuez (et commitez) des modifications dans le troisi�me d�p�t. Que se passe-t-il maintenant lorsqu'on fait git pull dans le troisi�me d�p�t ?

#### Question 3.6.
git pull peut �tre d�compos� en git fetch suivi de git merge. R�it�rez le scenario de la Question pr�c�dente mais en faisant git fetch au lieu de git pull, observez "toutes les branches" du d�p�t 3 dans Gitg. Un des aspects fondamental de Git est qu'il est d�centralis�. Nous avons ici donn� un r�le special de d�p�t central au d�p�t 2, mais s'il venait � dispara�tre, il serait toujours possible de synchroniser entre eux les d�p�ts 1 et 3.

#### Question 3.7.
Dans le d�p�t 1, nous allons d�clarer l'addresse du d�p�t 3, nous allons cr�er pour cela une remote appel�e _repo3_.
```sh
$ git remote add repo3 file://$PATH_TO_REPO3
```
De m�me, dans le d�p�t 3, nous allons cr�ez une remote appel�e repo1 qui pointe vers le premier d�p�t.

#### Question 3.8.
Effectuez (et commitez) des modifications dans le d�p�t 3 et r�cup�rez-les dans le d�p�t 1 au moyen de :
```sh
$ git fetch repo3
$ git checkout master
$ git merge remotes/repo3/master
```
Nous pouvons simplifier cette demarche en declarant que la branche master du d�p�t 1 "suit" la branche master du d�p�t 3 (depuis le d�p�t 1), � l'aide de 
```sh
$ git branch master --set-upstream repo3/master
```

#### Question 3.9.
Effectuez des modifications dans le d�p�t 1 puis r�cup�rez-les dans le d�p�t 3 au moyen de git pull.

#### Question 3.10.
Synchronisez entre eux les trois d�p�ts.

### 4 Modifications publiees, modifications non publiees
Nous avons vu que les objets que s'�changent les d�p�t gits sont des commits. Afin de maintenir une int�grite des arbres de commit, Git utilise des primitives cryptographiques. Chaque commit est en fait sign�, en fonction du patch qu'il repr�sente, du nom d'auteur, de la date de cr�ation, et aussi de la signature du commit parent (ou des deux parents, dans le cas d'un commit de fusion). Cette signature est un hachage SHA1 de toutes ces informations, et il est possible de se r�ferer � un commit uniquement par cette signature (de la forme f4ccba7ba89d4f6f8f0853056d47912c640a19c1) ou par un pr�fixe non ambigu de celle-ci (f4ccba7b).
Ainsi Git n'appliquera pas un commit ailleurs que sur son p�re. L'utilisateur de Git pourra vouloir appliquer un commit ailleurs dans l'arbre de commit (par exemple sur une autre branche), il va pour cela devoir cr�er un nouveau commit (c'est � dire avec un SHA1 dff�rent) mais qui contient les m�mes modifications.

Supposons que nous clonions un d�p�t (qu'on appelera repo et nommons commitA le dernier commit sur ce d�p�t),
et que nous effectuions un commit dans notre copie de travail (commitB dont le p�re est commitA).

Parall�lement, un autre d�veloppeur effectue un commit commitC au dessus du commitA et envoie ce commit dans le d�p�t repo.

Il n'est plus possible d'envoyer notre commit commitB, car le dernier commit du depot repo n'est pas commitA, mais commitC.

La strat�gie consiste alors � fusionner notre branche locale avec la branche distante, ce qui va cr�er un commit de fusion, fils de commitB et commitC et d'envoyer ce commit et le commit commitB vers repo.

Nous allons voir qu'une autre strategie est possible, de demander (avec la commande rebase) � Git de "modifier" le commitB pour que son p�re soit commitC.

#### Question 4.1.
Implementez ce scenario, constater que git push renvoie une erreur, puis au lieu d'invoquer git merge, invoquez
```sh
$ git rebase origin/master
```
(origin/master �tant le nom de la branche distante avec laquelle on voudrait normalement effectuer un merge).
Git va recr�er les commits de votre branche master qui ne sont pas dans la branche master du d�p�t repo et va les placer au dessus du dernier commit de la branche
master de repo).
**Attention, il est tr�s fortement d�conseille de rebase des commits qui ont dej� �t� publies**, c'est � dire pr�sent sur un autre d�p�t. La Question suivante va donc vous montrer ce qu'il faut �viter de faire.

#### Question 4.2.
Synchonisez vos 3 d�p�ts.
Dans le d�p�t 3, effectuez un commit.
Publiez-le dans le depot 2.
Dans le d�p�t 1 effectuez d'autres commits, r�cup�rez ce commit dans le d�p�t 3 � l'aide de
```sh
$ git fetch repo1
```
(car nous avons d�clare une remote appel�e repo1 dans le d�p�t 3 qui pointe vers le premier d�p�t),
puis (dans le d�p�t 3), rebasez votre branche master au dessus de la branche repo1/master avec
```sh
$ git rebase repo1/master
```
Que se passe-t-il si on essaie de merger cette branche avec la branche pr�sente dans le d�p�t 2 ?

Un autre cas de modification de commit est avec la sous-commande amend de Git.
amend permet d'�diter, de modifier le contenu d'un commit. Supposons qu'on vienne de commiter un commit intitul� orthographe et qu'il corrige des fautes d'orthographes.
Supposons qu'une faute ne soit pas corrig�e par ce commit, et qu'on ne veuille pas cr�er un autre commit par dessus, il est possible de modifier le dernier commit avec
```sh
$ git commit --amend
```

#### Question 4.3.
Creez un commit, puis apportez d'autres modifications, et editez le pr�c�dent commit au lieu d'en cr�er un nouveau.
L� encore, il est imp�ratif de ne pas �diter un commit qui a d�j� �t� publi�, **c'est ce que la Question suivante vous demande de faire et qu'il faut �viter de faire.**

#### Question 4.4.
Creez un commit, envoyez-le vers un d�p�t distant, puis amendez votre commit, synchronisez votre d�p�t avec le d�p�t distant.


