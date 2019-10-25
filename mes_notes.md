# Bonnes pratiques de développement

## Intro à Git
- besoins :
	- garder les différentes versions pour pouvoir revenir en arrière
	- pouvoir travailler de manière collaborative
- plus ancien SVN (Subversion) => sourceforge par exemple
- Git : né en 2005 pour dév Linux
- principes Git :
	- on travaille dans un dépôt (ou repository)
	- Git crée un dossier caché dans lequel il va tout archiver
	- utilisateur est maître du moment où il archive
	- archivage = un commit
	- 3 états dans le workflow :
		1.  on modifie un / des fichiers dans le répertoire de travail (*working directory*).
		2.  on ajoute les fichiers modifiés à la zone de transit, ou état de futur enregistrement (*staging area*)
		3.  on effectue un *commit*, i.e. on archive la version du fichier dans le dépôt (*repository*)

## Git : les commandes de base
- commandes de base :
	- `$ git init` : initialisation => crée le dépôt
	- `$ git add [Nom_de_fichier]` : ajoute un fichier à l'archive
	- `$ git commit -m "Message_du_commit" [Nom_de_fichier]` : enregistrement des modifications
	- `$ git log` : historique du repository
	- `$ git diff` : différence entre état archivé et état actuel, de manière détaillée
	- `$ git status` : différence entre état archivé et état actuel

- un conseil : disposer d'un dossier dev à la racine utilisateur

## Git : les branches
- concept de branche
- branch par défaut = master
- head : le nom du dernier commit
- commandes de base :
	- `$ git branch [nom_branch]` : créer une branch
	- `$ git checkout [nom_branch]` : se déplacer dans branche
	- `$ git checkout -b [nom_branch]` : créer et se déplacer dans branche
	- `$ git merge [nom_branch]` : fusionner
	- `$ git checkout [numero commit]` : revenir à un certain commit, puis créer une nouvelle branch pour faire des modifs
	- `$ git rm --cached [nom_fichier]` : enlever un fichier après git add et avant git commit
- bonnes pratiques :
	- garder une version master en état de fonctionnement
	- avoir une branch dev : plus ou en moins en état de marche
	- avoir une troisième branche = une fonctionnalité ou un bug
- pour gérer un conflit sur une fusion : modifier directement dans fichier

- pour supprimer un commit :
    - `$ git commit revert` : le dernier... si nécessaire remonter


## Git : serveurs distants
- un serveur distant = un remote
    - `$ git remote` : donne liste des serveurs
        -  convention : pour le premier serveur : origin ; pour le second : upstream
    - `$ git push` : pour envoyer données vers serveur
    - `$ git pull` : pour synchroniser un autre ordinateur depuis serveur


- on ajoute un second serveur distant à un repo
`$ git remote add upstream https://github.com/ragbx/courstest.git`
`$ git push -u upstream master`

- si on veut changer le nom du serveur de repo
`$ git remote set-url https://github.com/ragbx/courstest.git`


## Git : gestion des *forks*
*Voir les deux pages suivantes sur GitHub :
[Configuring a remote for a fork](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/configuring-a-remote-for-a-fork) et [Sync a fork of a repository to keep it up-to-date with the upstream repository](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/syncing-a-fork)*

1. Sur GitHub :
    - cliquer sur "Fork" sur la page. Le dépôt s'ajoute à notre compte.
2. Sur la machine locale :
    - configuration :
        - on clone le fork depuis notre compte
            `$ git clone https://github.com/YOUR_USERNAME/YOUR_FORK.git`
        - on spécifie un nouveau serveur distant *upstream* :
            `$ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git`
        - si on liste les dépôts distants, on doit obtenir un résultat de ce type :
            ```
            $ git remote -v
            > origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
            > origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
            > upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
            > upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
            ```
    - si on veut suivre les évolutions du dépôt original :
        - on récupère les données du dépôt original :
            `$ git fetch upstream`
        - on se place sur la branche *master* du dépôt local :
            `$ git checkout master`
        - on fait un merge entre master et upstream/master :
            `$ git merge upstream/master`


## Tests et intégration continue
- une régression : risque d'introduire un bug ou une perte de perf

- intégration continue : lancer batterie de tests développés par l'équipe de dev

- cf Bridget Almas, conférence sur le site de l'Ecole des chartes
