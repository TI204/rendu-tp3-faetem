# Exercice 1 : Création et navigation entre branches 
## Instructions 
### Création de branches nommées feature-1 et feature-2
``` bash
$ git branch feature-1
$ git branch feature-2
```

### Affichez la liste des branches
``` bash
$ git branch
  feature-1
  feature-2
* main
```

### Déplacez-vous sur feature-1
```bash
$ git checkout feature-1
Switched to branch 'feature-1'
```

### Modifiez le README pour ajouter une ligne “Modification dans feature-1”
``` bash
$ cat README.md
# Projet d'exercice
Version initiale du projet

Modification dans feature-1
```
### Committez ce changement
``` bash
$ git log
commit 1da0f021586251009bc2576de122ff6d781adf99 (HEAD -> feature-1)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 12:03:29 2025 +0100

    Modification du README

commit 9d848e3ed27677aab3973112fea8efa9fa142a2a (main, feature-2)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 12:01:32 2025 +0100

    Premier commit : création du README
```

### Retournez sur la branche main
``` bash
$ git checkout main
Switched to branch 'main'
```

### Vérifiez que le README n’a pas la modification
``` bash
$ cat README.md
# Projet d'exercice
Version initiale du projet
``` 

### Questions bonus : 
Que se passe-t-il lorsqu'on supprime la branche sur laquelle on travaille ? 
``` bash
$ git branch -d main
error: cannot delete branch 'main' used by worktree at 'C:/Users/Faetem/Desktop/EFREI/B1/Versionning_et_repository/xin206/tp3'
```
Comment voir les différences entre les branches `main` et `feature-1` ?
``` bash
$ git diff main feature-1
```

# Exercice 2 : Fusion simple
## Instructions 
### Partez de la branche main
``` bash 
$ git branch
  feature-1
  feature-2
* main
```

### Créez une branche dev
``` bash
$ git branch dev
``` 

### Dans dev, créez un fichier notes.txt avec du contenu
``` bash
$ git checkout dev
Switched to branch 'dev'
```
``` bash
$ cat notes.txt
Notes du premier semestre :

Anglais : 19/20
JavaScript : 17/20
Conception de BDD : 13/20
```

### Committez ce fichier
``` bash
$ git log
commit 9852bc289b5b0347c78313b65c5790b5314619b4 (HEAD -> dev)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 12:28:25 2025 +0100

    Ajout de notes.txt
``` 

### Retournez sur main
``` bash
$ git checkout main
Switched to branch 'main'

### Fusionnez dev dans main
``` bash
$ git merge dev
Updating 9d848e3..9852bc2
Fast-forward
 notes.txt | 6 ++++++
 1 file changed, 6 insertions(+)
 create mode 100644 notes.txt
```

### Vérifiez que le fichier notes.txt est bien présent
``` bash
$ git branch
  dev
  feature-1
  feature-2
* main
```
``` bash
$ ls
notes.txt  README.md  reponses.md
```

### Supprimez la branche dev
``` bash
$ git branch -d dev
Deleted branch dev (was 9852bc2).
```

# Exercice 3 : Gestion des conflits 
## Instructions 
### Créez une nouvelle branche conflit-test
``` bash
$ git branch conflit-test
```
``` bash
$ git branch
  conflit-test
  feature-1
  feature-2
* main
```

### Sur la branche main, modifiez la première ligne du README 
``` bash
$ cat README.md
# Projet d'exercice - Version Main
Version initiale du projet
``` 

### Committez ce changement
``` bash
$ git log
commit 568bb6de1e57008daf92fd2e35d9c365e3cc7605 (HEAD -> main)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:06:39 2025 +0100

    Modification du README sur la branche main

commit 9852bc289b5b0347c78313b65c5790b5314619b4 (conflit-test)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 12:28:25 2025 +0100

    Ajout de notes.txt

commit 9d848e3ed27677aab3973112fea8efa9fa142a2a (feature-2)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 12:01:32 2025 +0100

    Premier commit : création du README
``` 

### Basculez sur conflit-test
``` bash
$ git checkout conflit-test
Switched to branch 'conflit-test'
```

### Modifiez la même ligne du README différemment 
``` bash 
$ cat README.md
# Projet d'exercice - Version Test
Version initiale du projet
```

### Committez ce changement
``` bash
$ git log
commit 731018d73a183de3415c8292a4ca8326be25b693 (HEAD -> conflit-test)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:08:39 2025 +0100

    Modification du README sur la branche conflit-test

commit 9852bc289b5b0347c78313b65c5790b5314619b4
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 12:28:25 2025 +0100

    Ajout de notes.txt

commit 9d848e3ed27677aab3973112fea8efa9fa142a2a (feature-2)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 12:01:32 2025 +0100

    Premier commit : création du README
```

### Retournez sur main et essayez de fusionner conflit-test dans main
``` bash
$ git checkout main
Switched to branch 'main'
```
```bash
$ git merge conflit-test
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```
Conflit dans la fusion car le fichier README diffère d'une branche à l'autre 


### Résolvez le conflit en gardant les deux informations :
Tout d'abord on modifie le README sur la branche main
``` bash
$ cat README.md
# Projet d'exercice - Version Main et Test

Version initiale du projet
```
Ensuite on ajoute le fichier et on commit 
``` bash
$ git add README.md
$ git commit -m "Résolution du conflit de merge"
[main 5a58988] Résolution du conflit de merge
```

Ensuite on passe sur la branche conflit-test pour vérifier que le README a bien été modifié 
``` bash
$ git checkout conflit-test
Switched to branch 'conflit-test'
```
``` bash
$ cat README.md
# Projet d'exercice - Version Test
Version initiale du projet
```

On peut maintenant supprimer la branche conflit-test une fois la fusion effectuée 
``` bash
$ git branch -d conflit-test
Deleted branch conflit-test (was 731018d)
```

# Exercice 4 : Rebase 
## Instructions 
### Créez une branche feature-rebase
``` bash
$ git branch feature-rebase
```

### Dans cette branche :
#### Ajoutez un fichier feature.txt
#### Faites plusieurs commits
``` bash
$ git checkout feature-rebase
Switched to branch 'feature-rebase'
```
``` bash
$ git log
commit 5a2a723052dd20109d9f62f55d008f11b31f1d80 (HEAD -> feature-rebase)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:28:33 2025 +0100

    Troisième modif de feature.txt

commit d3f996a977792d030f9dba5f19e92e0040be0430
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:27:54 2025 +0100

    Deuxième modif de feature.txt

commit 9eeb41dae9b916c93e4ab44df4e6cae109296a9d
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:27:11 2025 +0100

    Première modif de feature.txt
```

### Pendant ce temps, dans main :
#### Faites des modifications différentes
#### Créez quelques commits
``` bash
$ git checkout main
Switched to branch 'main'
```
``` bash
$ git log
commit fe2071b4a9c086e70401f659d9cbe846b0c1c2ba (HEAD -> main)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:34:13 2025 +0100

    Dernière modif de notes.txt

commit 8ea0931cada61d54e3e48015b99396d063fb6630
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:33:18 2025 +0100

    Deuxième modif de notes.txt

commit 60e321f147bacb8095ea40441e91e21437df8b22
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:32:51 2025 +0100

    Première modif de notes.txt
```
### Dans feature-rebase :
#### Faites un rebase sur main
#### Observez l’historique des commits
``` bash 
$ git checkout feature-rebase
Switched to branch 'feature-rebase'
```
Historique avant le rebase : 
``` bash 
$ git log --graph
* commit fe2071b4a9c086e70401f659d9cbe846b0c1c2ba (HEAD -> main)
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:34:13 2025 +0100
|
|     Dernière modif de notes.txt
|
* commit 8ea0931cada61d54e3e48015b99396d063fb6630
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:33:18 2025 +0100
|
|     Deuxième modif de notes.txt
|
* commit 60e321f147bacb8095ea40441e91e21437df8b22
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:32:51 2025 +0100
|
|     Première modif de notes.txt
|
*   commit 5a58988f2cf8cf00e9f716a6ce1d9eb709bffe85
|\  Merge: 568bb6d 731018d
| | Author: faetem <tourefaetemh@gmail.com>
| | Date:   Mon Mar 10 13:15:55 2025 +0100
| |
| |     Résolution du conflit de merge
| |
| * commit 731018d73a183de3415c8292a4ca8326be25b693
| | Author: faetem <tourefaetemh@gmail.com>
| | Date:   Mon Mar 10 13:08:39 2025 +0100
| |
| |     Modification du README sur la branche conflit-test
| |
* | commit 568bb6de1e57008daf92fd2e35d9c365e3cc7605
|/  Author: faetem <tourefaetemh@gmail.com>
|   Date:   Mon Mar 10 13:06:39 2025 +0100
|
|       Modification du README sur la branche main
|
* commit 9852bc289b5b0347c78313b65c5790b5314619b4
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 12:28:25 2025 +0100
```
``` bash
$ git rebase main
Successfully rebased and updated refs/heads/feature-rebase.
```
Historique après le rebase : 
``` bash
$ git log --graph
* commit 1ccca510e197fbc8627911bb731023fccac4be68 (HEAD -> feature-rebase)
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:28:33 2025 +0100
|
|     Troisième modif de feature.txt
|
* commit 763be5278d486784620e823959c4ea1b889d4a1d
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:27:54 2025 +0100
|
|     Deuxième modif de feature.txt
|
* commit 0057d2046ddfd025c0d9aa1b4efb952f635128cb
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:27:11 2025 +0100
|
|     Première modif de feature.txt
|
* commit fe2071b4a9c086e70401f659d9cbe846b0c1c2ba (main)
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:34:13 2025 +0100
|
|     Dernière modif de notes.txt
|
* commit 8ea0931cada61d54e3e48015b99396d063fb6630
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:33:18 2025 +0100
|
|     Deuxième modif de notes.txt
|
* commit 60e321f147bacb8095ea40441e91e21437df8b22
| Author: faetem <tourefaetemh@gmail.com>
| Date:   Mon Mar 10 13:32:51 2025 +0100
|
|     Première modif de notes.txt
|
*   commit 5a58988f2cf8cf00e9f716a6ce1d9eb709bffe85
|\  Merge: 568bb6d 731018d
| | Author: faetem <tourefaetemh@gmail.com>
| | Date:   Mon Mar 10 13:15:55 2025 +0100
| |
```
Nous observons qu'après le rebase l'historique de commit devient linéaire.

# Exercice 5 : Branches et historique
## Instructions
### Créez plusieurs branches 
``` bash
$ git branch
  feature-1
  feature-2
  feature-a
  feature-b
  feature-c
  feature-rebase
* main
```
### Dans chaque branche 
#### Faites au moins deux commits
#### Utilisez des fichiers différents pour éviter les conflits
``` bash
$ git log --all
commit 4422a01dad2afd5818034073a69ed22064c4c291 (feature-c)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:59:00 2025 +0100

    Ajout d'une fonction à feature-c.py

commit 181dfc56af091195e595ede43500f3529d17606b
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:58:13 2025 +0100

    Création de feature-c.py

commit 4d8b92decf6080a0e60a8d73c4fbd0f517ec2b46 (feature-b)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:57:21 2025 +0100

    Ajout d'une fonction à feature-b.py

commit 240763c4841f76a79bada90dcf1e3d1acb1e947d
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:56:42 2025 +0100

    Création de feature-b.py

commit 1886e8fdcea8d8602c1d7a571edddcb4108c3967 (feature-a)
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:55:56 2025 +0100

    Ajout d'une fonction à feature-a.py

commit ec9db5552543c5624d9f8a85b281e64d0a5080ef
Author: faetem <tourefaetemh@gmail.com>
Date:   Mon Mar 10 13:54:52 2025 +0100

    Création de feature-a.py
```

### Fusionnez toutes les branches dans main
``` bash
$ git merge feature-a
Updating fe2071b..1886e8f
Fast-forward
 feature-a.py | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 feature-a.py

$ git merge feature-b
Merge made by the 'ort' strategy.
 feature-b.py | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 feature-b.py

$ git merge feature-c
Merge made by the 'ort' strategy.
 feature-c.py | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 feature-c.py
```

### Pratiquez les commandes de visualisation 
``` bash
$ git adog
*   b020db7 (HEAD -> main) Merge branch 'feature-c'
|\
| * 4422a01 (feature-c) Ajout d'une fonction à feature-c.py
| * 181dfc5 Création de feature-c.py
* |   710ed67 Merge branch 'feature-b'
|\ \
| * | 4d8b92d (feature-b) Ajout d'une fonction à feature-b.py
| * | 240763c Création de feature-b.py
| |/
* | 1886e8f (feature-a) Ajout d'une fonction à feature-a.py
* | ec9db55 Création de feature-a.py
|/
| * 1ccca51 (feature-rebase) Troisième modif de feature.txt
| * 763be52 Deuxième modif de feature.txt
| * 0057d20 Première modif de feature.txt
|/
* fe2071b Dernière modif de notes.txt
* 8ea0931 Deuxième modif de notes.txt
* 60e321f Première modif de notes.txt
*   5a58988 Résolution du conflit de merge
|\
| * 731018d Modification du README sur la branche conflit-test
* | 568bb6d Modification du README sur la branche main
|/
* 9852bc2 Ajout de notes.txt
| * 1da0f02 (feature-1) Modification du README
|/
* 9d848e3 (feature-2) Premier commit : création du README
```

``` bash
$ git branch -v
  feature-1      1da0f02 Modification du README
  feature-2      9d848e3 Premier commit : création du README
  feature-a      1886e8f Ajout d'une fonction à feature-a.py
  feature-b      4d8b92d Ajout d'une fonction à feature-b.py
  feature-c      4422a01 Ajout d'une fonction à feature-c.py
  feature-rebase 1ccca51 Troisième modif de feature.txt
* main           b020db7 Merge branch 'feature-c'
```
``` bash
$ git branch --merged
  feature-2
  feature-a
  feature-b
  feature-c
* main
``` 
``` bash
$ git branch --no-merged
  feature-1
  feature-rebase
```

