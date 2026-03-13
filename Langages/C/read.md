# La fonctions read()

---

> Lit les données depuis le fd donné. Le nombre d'éléments lus depuis notre fd va dépendre de la taille qu'on va lui donner. Les éléments lus vont être stockés dans la nouvelle chaîne et grâce à ça, la fonction va retourner le nombre d'éléments lus.

---

## Prototype
```c
#include <unistd.h>

ssize_t read(int fd, void *buf, size_t count);
```

---

## Paramètres

| Paramètre | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| `fd`      | int    | File descriptor (0 = stdin, 3+ = fichiers) |
| `buf`     | void * | Buffer où stocker les données lues         |
| `count`   | size_t | Nombre max d'octets à lire                 |

---

## Valeur de retour

| Retour | Signification        |
| ------ | -------------------- |
| `> 0`  | Nombre d'octets lus  |
| `0`    | Fin de fichier (EOF) |
| `-1`   | Erreur               |

---

## Les fd à retenir

| fd  | Nom    | Usage                       |
| --- | ------ | --------------------------- |
| 0   | stdin  | `read(0, ...)` — lecture    |
| 1   | stdout | `write(1, ...)` — affichage |
| 2   | stderr | `write(2, ...)` — erreurs   |

---

## Les fd fichiers (3, 4, 5...)

Quand tu ouvres un fichier avec `open()`, le système donne le premier fd disponible :
```c
int fd1 = open("fichier1.txt", O_RDONLY);  // fd = 3
int fd2 = open("fichier2.txt", O_RDONLY);  // fd = 4
int fd3 = open("fichier3.txt", O_RDONLY);  // fd = 5
```

### Exemple lecture depuis un fichier
```c
int fd;
char buffer[100];
int len;

fd = open("test.txt", O_RDONLY);  // fd = 3
if (fd < 0)
    return (1);  // Erreur d'ouverture

len = read(fd, buffer, 99);  // Lire depuis fd 3
buffer[len] = '\0';

write(1, buffer, len);  // Afficher sur stdout
close(fd);  // Toujours fermer !
```

### Différence stdin vs fichier

| Source         | fd  | Comment lire                    |
| -------------- | --- | ------------------------------- |
| Clavier / pipe | 0   | `read(0, ...)`                  |
| Fichier        | 3+  | `fd = open(...); read(fd, ...)` |

---

## Mon utilisation dans filter
```c
int resread;
char *newstr;

newstr = malloc(sizeof(char) * (BUFFER_SIZE + 1));
resread = 1;

while (resread > 0)
{
    resread = read(0, newstr, BUFFER_SIZE);  // Lire depuis stdin
    if (resread < 0)
    {
        write(2, "Error\n", 6);  // Erreur sur stderr
        free(newstr);
        return (1);
    }
    // Traiter newstr...
    write(1, newstr, resread);  // Écrire sur stdout
}
free(newstr);
```

---

## Pièges courants

### 1. Résultat ignoré
```c
read(0, newstr, BUFFER_SIZE);  // On ne sait pas combien on a lu
```

### 2. Comparer la fonction au lieu du résultat
```c
if (read < 0)  // "read" est une fonction !
```

### 3. Mauvais fd pour write
```c
write(0, newstr, resread);  // 0 = stdin (entrée, pas sortie)
```

### 4. Oublier de fermer le fichier
```c
fd = open("test.txt", O_RDONLY);
read(fd, buffer, 100);
// Pas de close(fd) → fuite de ressources
```

### Corrections
```c
resread = read(0, newstr, BUFFER_SIZE);  // Stocker le résultat
if (resread < 0)                          // Comparer le résultat
write(1, newstr, resread);                // stdout pour affichage
write(2, "Error\n", 6);                   // stderr pour erreurs
close(fd);                                // Toujours fermer !
```

---

## Voir aussi

- [[open]] — Ouvrir un fichier
- [[close]] — Fermer un fichier
- [[Langages/C/File descriptors|File descriptors]] — Les fd expliqués
- [[filter]] — Exercice utilisant read