# git

Cette note a pour but de documenter git, mais uniquement dans le cadre d'un travail sur plusieurs branches.

---
### Comment push dans sa branche ? :
```bash
git add
git commit -m "Message"
git push origin nomdebranch
```

#### Récupérer les éléments dans la branche qui a push ? : 
Imaginons que la branche dans laquelle on travaille s'appelle **brad** et que l'on veut push ce qu'on a fait dans la branche main, il y a **2 versions** : *une longue et une courte*. Je recommande la version 2 qui est basée sur une seule commande après avoir fait `git pull origin main`.

*Version 1 (longue) :*
```bash
git switch main #git checkout main
git pull origin main # mettre main à jour d'abord
git fetch origin
git merge origin/brad
git push origin main #ne pas oublier de push
```

*Version 2 (courte) :*

```bash
git switch main #git checkout main
git pull origin main # mettre main à jour d'abord
git pull origin brad
git push origin main #ne pas oublier de push
```

---
#### git pull = fetch + merge :
`git pull` est un raccourci pour deux commandes :
```bash
git fetch origin   # récupère les nouveautés du remote
git merge origin/main  # les fusionne dans ta branche courante
```
C'est pour ça que dans la version 1, le `git fetch` + `git merge` est équivalent au `git pull` de la version 2.

---
#### Récupérer les dernières nouveautés du main dans sa branche :
Pour mettre sa branche à jour avec ce qu'il y a dans `main` :

```bash
git switch brad      # se mettre sur sa branche
git pull origin main # tirer les changements du main dans brad
```

> **Note :** s'il y a des conflits, Git te le signalera et il faudra les résoudre manuellement avant de `git add` + `git commit`.
