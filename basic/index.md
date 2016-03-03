# GIT

*Versionner ses sources.*

---

# Introduction

Objectifs

* **connaître les commandes de base**
* **expérimenter avec le workflow de développement** pour participer à un projet Érudit
* **avoir un aperçu de GitLab et GitHub**

Documentation

* [Site officiel][git]
* [Documentation officielle][git-doc]
* Tutoriel
    * http://try.github.com/
    * https://www.atlassian.com/git/
        * https://www.atlassian.com/fr/git/
* [Livre gratuit][git-book]

Environnement

* Local
    * terminal ou git gui
* Distant
    * [GitLab d'Érudit][erudit-gitlab]
    * [Érudit sur GitHub][erudit-github]

[Setup technique][setup]

---

# I. Théorie

## Pourquoi versionner avec Git?

Pourquoi versionner?

* *historique* : voir les versions passées
* *comparaison* : entre différentes versions (qu'est-ce qui a changé?)
* *expérimentation* : explorer une nouvelle version possible
* *travail collaboratif* : travail sur versions en parallèle (en local, sur branches...) puis fusion des versions

Pourquoi Git?

* nouvelle génération (3e) : git, baazar, mercurial...
    * anciennes génération : SVN (2e), CVS (1ere)
* distribué
    * *local* : chaque dépôt est parfaitement autonome...
    * *hors-ligne* : ... ce qui veut dire que vous n'avez pas besoin de réseau pour travailler en local

Versionner quoi?

* code source
* tout fichier texte
    * pas top pour fichiers binaires (png, odt, ...)

*Ne pas* versionner quoi?

* toute information confidentielle
    * mots de passe, paramètres de connexion à DB
    * documents clients
* toute information spécifique à un environnement
    * configuration
    * fichiers de projet de l'IDE
* environnement virtuel, ex.: venv
* répertoires contenants fichiers uploadés
* sources compilées
* etc.

## Dépôt Git

Schéma : [Dépôts Git][schema-depots]

### Git sur votre machine

System side

* _git, le programme qui gère vos dépôts_

        /usr/bin/git

User side

* _configuration globale, pour tout dépôt local_

        /home/<user>/.gitconfig
        /home/<user>/.gitignore

Project side

* _dépôt local git et sa configuration pour le projet_

        /<path>/.git

* _fichiers non suivis pour ce projet_

        /<path>/.gitignore


### Dépôts distribués

Un même contenu dans plusieurs dépôts :

* Érudit
    * local (développeur)
        * machines perso
    * serveur (release manager)
        * GitLab
        * GitHub

Les commandes `git` accessibles dans :

* le répertoire où on a créé le dépôt Git
* ses sous-répertoires (attention aux paths)

## Terminologie

### Working directory

Le répertoire du dépôt Git (et donc ses sous-répertoires et fichiers).

### États des fichiers

* *untracked*: ignoré par git
* *tracked*: suivi par git
    * *modified*: le fichier a été modifié depuis le dernier commit
    * *staged*: le fichier modifié a été ajouté (à l'index) pour le prochain commit
    * *commited*: enregistré dans l'historique

### Index

Modifications enregistrées par Git, prêtes à être commitées.

### Commit

* ensemble de modifications ajoutées à l'historique en une fois (en un bloc)
* identifiant unique = hash : SHA1
* message   
Le commentaire à saisir doit être de la forme suivante : `"[#12345] message"`, où 12345 représente le numéro de la demande redmine concernée par le commit.

### Tree

L'arborescence de fichiers présente dans un commit.

### HEAD

Pointeur vers la branche en cours

### Branches

Ensemble de commit

---

# II. Démo

Git live en [mode graphique][git-d3]
* visualiser les branches

---

# III. Pratique (hands-on : faites-le)

## 1. Configurer globalement git

Identité de son user

    $ git config --global user.name "Prénom Nom"
    $ git config --global user.email "prenom.nom@erudit.org"
    $ git config --global color.ui auto
    $ git config --global push.default simple

Ignorer des fichiers ou répertoires (ex.: `private`, `venv`, `data`, `conf`)

    /home/<user>/.gitignore

## 2. Versionner en local

### Initialiser un dépôt git

Créer un répertoire en local et s'y déplacer

    $ mkdir atelier
    $ cd atelier

Initialiser un dépôt vierge

    $ git init

* `.git` créé = tout le dépôt en local

Vérifier l'état du dépôt

    $ git status

Créer du contenu (un répertoire et un fichier) sous `atelier`

* `README.md` : bonne pratique car affiché sur accueil du dépôt sur le serveur GitHub, GitLab
    * [syntaxe markdown][markdown]

Créer le fichier caché commandant à git d'ignorer certains fichiers ou répertoires

* `.gitignore`
    * [exemples de .gitignore][gitignore]

Ajouter les fichiers et répertoires à l'index de git

    $ git add .

### Amorcer le suivi

Engager ses changements

    $ git commit -m "Initial commit"

Vérifier le journal des commits, le dernier commit

    $ git log
    $ git log -1
    $ git show

Vérifier l'historique des versions (commits), de toutes les branches

    $ gitk
    $ gitk --all

Vérifier l'historique des versions (commits et qui a fait modif) pour chaque ligne d'un fichier particulier.

    $ git blame nom_fichier.py

### Revenir en arrière

    $ git commit --amend
    $ git reset
    $ git checkout

### Commandes

    git init
    git status

    git add
    git rm

    git commit
    git commit --amend

    git log
    git show
    git blame
    gitk --all

### Exercices

Initialisation

1. initialiser un dépôt git local pour l'atelier
1. ajouter du contenu à suivre par git
1. ajouter du contenu à ignorer par git
1. créer un premier commit

Modification

1. modifier des fichiers
1. créer un second commit

Ajout

1. ajouter des fichiers
1. créer un troisième commit

Suppression

1. supprimer des fichiers
1. un quatrième commit

Répertoires

1. créer un répertoire vide
1. vérifier s'il y a du contenu à suivre
1. ajouter des fichiers au répertoire
1. vérifier s'il y a du contenu à suivre
1. ajouter un répertoire ignoré globalement (config user, pout tous les projets)
1. tester si Git suit ses modifications
1. ajouter un répertoire ignoré localement (config locale, pour ce projet)
1. tester si Git suit ses modifications

## GitLab

* Accueil : pas public
* Activity
* Projets : rechercher
    * **Projet = Dépôt**
    * URL pour cloner
    * Commits
        * une branche est automatiquement choisie
        * détail d'un **commit**
            * hash identifiant commit et parent(s)
            * message de commit : première ligne et les autres
            * diff
                * fichiers impactés par modifications commitées
                    * voir au complet? browse code
                * détail des lignes modifiées pour chaque fichier
                * commenter ligne de modification
            * commentaire
    * Branches
        * tree
             * contenu de l'arborescence pour dernier commit de cette branche
             * ~ état du code
        * README.md
        * history -> commits
    * Tags
        * rien d'autre qu'un commit étiquetté (ne dépend pas d'une branche)
    * Merge
        * liste des Merge Request
        * détail d'un **Merge Request** (cliquer sur nom suivant numéro)
            * commits
            * discussion : release manager et développeur
            * Jenkins
    * Settings
        * si vous gérez le projet
        * Project
        * Members
        * Protected branches
* My snippets
* New Project
    * voir ci-dessous
* Profile settings
    * Gravatar, c'est cool :)

## 3. Versionner sur serveur

Créer un dépôt git sur le serveur à partir d'un dépôt local

* créer le projet sur le serveur GitLab
    * GitLab : *New Project*
        * *Repository name* : **atelier-git**
        * *Visibility Level* : Internal

* configurer en local l'URL où sera le dépôt sur serveur (l'une ou l'autre commande)

        $ git config remote.origin.url git@gitlab.erudit.team:prenom.nom/atelier-git.git
        $ git remote add origin git@gitlab.erudit.team:prenom.nom/atelier-git.git

* plusieurs dépôts distants peuvent être configurés, il s'agit de leur fournir un nom (mot clé)

        $ git remote add github git@github.com:prenom.nom/atelier-git.git

* retrouver les noms configurés pour ses dépôts distants

        $ git remote

* pousser son dépôt local sur le serveur (en précisant le nom (mot clé) du serveur distant)

        $ git push -u origin master

Récupérer localement un dépôt git existant sur serveur

* trouver l'URL du dépôt sur la page d'accueil du dépôt sur le serveur (SSH ou HTTPS)
* cloner, dans le répertoire voulu (défaut = nom du dépôt cloné)

        $ git clone git@gitlab.erudit.team:giotta/atelier-git.git giotta-ssh
        $ git clone https://gitlab.erudit.team/giotta/atelier-git.git giotta-https

Tirer localement la dernière version mise à jour d'un autre dépôt git (ex.: existant sur serveur)<br />
*récupère et merge localement les mises à jour distantes (remote)*

    $ git pull [origin] [master]

Autoriser d'autres utilisateurs à commiter dans votre projet

* GitLab : *`<Project> > Settings > Members > New project member`*

Régler les conflits

* Vérifier un à un les fichiers impliqués
* Chercher les versions en conflit

        >>>>>
        <<<<<

* Choisir entre la version du HEAD et l'autre version
* ... ou lancer via git un outil pour faciliter les correctifs à faire

        $ git mergetool

* outils recommandés : `meld`, `xxdiff`, `vimdiff`, `gvimdiff`

        $ sudo apt-get install meld

Supprimer un dépôt sur le serveur

* GitLab : *`<Project> > Settings > Dangerous settings > Remove project`*

### Commandes

    git clone
    git config
    git remote
    git pull
    git mergetool
    git push

### Exercices

1. S'assurer d'avoir cloné le dépôt de l'atelier
1. Ajouter un fichier portant votre prénom à la racine du projet
1. Commiter la modification
1. Pusher le commit sur le serveur
1. Puller la dernière version du serveur
1. Vérifier les commits sur le web et dans gitk
1. Modifier le fichier indiqué par le présentateur de l'atelier
1. Commiter, pusher
1. Régler un conflit :
    * Éditer la ligne du fichier indiqué par le présentateur de l'atelier
    * Essayer de pusher à son signal
    * Puller à son signal
    * Régler le conflit (éditer version finale voulue)
    * Commiter, pusher

## 4. Branches

Des branches pour faire évoluer le code en parallèle...

Ex.: pour expérimenter (nouvelle fonctionnalité) ou gérer différents degrés de maturé du code (workflow de développement)

Branches permanentes (convention, peut varier en fonction des équipes/projets)
* `master` : dernière version stable déployée
* `develop` : derniers développements
* `<version>` : ex.: `1.1.3` pour logiciels qui maintiennent plusieurs versions

*Convention : la branche **master** fait toujours autorité.*

Lister les branches   
*(Branche en cours = précédée d'astérisque)*

* locales

        $ git branch

* distantes (remotes)

        $ git branch -r

        $ git fetch --all
        $ git branch -r

* toutes (all)

        $ git branch -a

Créer en local

* créer branche neuve à partir du commit en cours

        $ git branch mabranchelocale

* créer branche en local *à partir de* et *liée à* branche du serveur

        $ git branch mabranchelocale -t labrancheserveur

Créer sur serveur

    $ git push origin labrancheserveur

Configurer la branche

    $ git config branch.mabranchelocale.remote origin
    $ git config branch.mabranchelocale.merge refs/heads/mabrancheserveur

Changer de branche

    $ git checkout autre_branche

  > Se lit: "Je fais le checkout de la branche en cours pour aller vers cette autre branche"

Fusionner les branches

    $ git merge autre_branche

  > Se lit: "Je fusionne cette autre branche, ici, dans la branche en cours"

Fusion pas symétrique :   

    (branche1) git merge branche2

**&ne;**

    (branche2) git merge branche1

Supprimer en local

    $ git branch -d mabranchelocale

Supprimer sur serveur

    $ git push :labrancheserveur

### Commandes

    git fetch
    git branch
    git push
    git rebase
    git config
    git checkout
    git merge

### Exercices

1. Aller dans votre le dépôt git local de l'atelier
1. Vérifier qu'aucune branche n'existe sur le serveur nommée avec votre prénom
1. Créer une branche locale perso nommée avec votre prénom (ou autre si existe déjà) : ex.: pgault
1. Vérifier la création de la branche locale dans gitk
1. Mettre cette nouvelle branche sur le serveur# Vérifier la création de la branche distante sur le web et dans gitk
1. Configurer votre branche pour pouvoir puller et pusher directement (sans ajouter ''origin labrancheserveur'')
1. Attendre les instructions du ''release manager'' (ici = présentateur) pour fusionner la branche master à jour dans votre branche perso
1. Mettre à jour votre branche perso sur le serveur
1. Trouver un collègue et faites en sorte que vos 2 branches soient identiques :
    1. Copier en local la branche du collègue
    1. Fusionner la branche du collègue avec la vôtre
    1. Seulement l'un de vous met sa branche perso fusionnée sur le serveur
    1. Vérifier l'asymétrie : seulement une branche inclut l'autre
    1. second met sa branche perso fusionnée sur le serveur
    1. Vérifier la symétrie : les 2 tags verts sont sur le même commit

## 5. Comparer

Comparer deux commits, deux branches, deux versions (avec tags)

    $ git diff abc56d c12db3
    $ git diff branche1 branche2
    $ git diff 1.0 1.0.1

    $ git diff --summary abc56d c12db3
    $ git diff --summary branche1 branche2
    $ git diff --summary 1.0 1.0.1

### Commandes

    git diff

## 6. Merge Requests / Pull Requests

* fusionner des branches
    * Merge Request (MR) = vocable GitLab
    * Pull Request (PR) = vocable GitHub
* séparation des rôles : release manager (reviewer), développeur

### Exercices

Reprendre exercice sur branches, mais faire la fusion des branches via Merge Requests

## 7. Tags

Lorsque votre contribution correspond à une version stable, l'identifier en y apposant une étiquette avec le nom de la version.
* Convention de nommage des version : 1.2.3 : 1 = major, 2 = minor, 3 = adjustment
    * http://semver.org/
        * http://legacy.python.org/dev/peps/pep-0440/
        * http://legacy.python.org/dev/peps/pep-0396/

Ajouter une étiquette

    $ git tag -a -m "Commentaire etiquette" nom_etiquette
    $ git tag -a -m "Security release" 1.2.3

Les étiquettes sont locales et ne seront pas transmises au serveur lors d'un push à moins de le spécifier explicitement.

    $ git push --tags

* Note: ce push ne transmet pas les commit au serveur, seulement les tags.

Lister toutes les étiquettes

    $ git tag -l

Supprimer une étiquette localement

    $ git tag -d nom_etiquette

Supprimer une étiquette sur le serveur (à éviter)

    $ git push origin :refs/tags/nom_etiquette

Utiliser une étiquette

    $ git pull origin tag 1.3.1

* Fait un pull du serveur de la version correspondant à une étiquette spécifique (ici 1.3.1).

### Commandes

    git tag
    git push --tags

### Exercices

## 8. Suspendre des modifications

Mettre son code sur la glace (ex.: pour changer de branche sans commiter)

    $ git stash
    $ git stash pop

### Commandes

    git stash

## 9. Workflow

Vérifier la branche et l'état du dépôt local

    $ git status

Tirer en local la dernière version du serveur

    $ git pull

Éditer l'arborescence du projet

Vérifier ses changements

    $ git show
    $ git diff
    $ git diff --summary

Annuler complètement toutes ses modifications

        $ git checkout <file>
        $ git checkout .

Ajouter les fichiers et répertoires à l'index de git

    $ git add .

Au besoin, supprimer des fichiers ou répertoires de l'index de git

    $ git rm nom_fichier

Engager ses changements

    $ git commit -m "[#12345] Message significatif de commit"

Retravailler son ensemble de commit (pas couvert dans cet atelier)

    $ git rebase

S'assurer d'avoir la dernière version avant de pousser sur serveur

    $ git pull

Pousser sa nouvelle version locale sur le serveur

    $ git push

Demander un Merge Request / Pull Request

### Développement projet

* http://nvie.com/posts/a-successful-git-branching-model/
* https://www.atlassian.com/git/tutorials/comparing-workflows/

### Commandes

    git checkout .

### Commandes non vues

    git reset
    git revert

# Conclusion

* Git tire en sale!
* En savoir plus :
  * [Documentation officielle][git-doc]
  * [Cheat Sheet][git-cheat] publié par GitHub
  * [Git for Computer Scientists](http://eagain.net/articles/git-for-computer-scientists/)
  * [A Visual Git Reference](http://marklodato.github.com/visual-git-guide/index-en.html)
  * [Interactive Git Cheatsheet](http://ndpsoftware.com/git-cheatsheet.html)

---

[setup]: setup.md
[gitignore]: https://github.com/github/gitignore

[schema-depots]: schema-depots.jpg

[git]: http://git-scm.com/
[git-doc]: http://git-scm.com/doc
[git-cheat]: https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf
[git-d3]: http://onlywei.github.io/explain-git-with-d3/
[git-book]: https://git-scm.com/book

[gitlab]: https://about.gitlab.com/
[github]: https://github.com/

[erudit-gitlab]: https://gitlab.erudit.team/
[erudit-github]: https://github.com/erudit

[markdown]: http://daringfireball.net/projects/markdown/syntax
