# open

---

> La fonction open a pour but d'ouvre un fichier et retourne un file descriptor (fd) pour le manipuler.

---

## Prototype

```c
#include <fcntl.h>

int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
```

---

## Paramètres

| Paramètre | Type | Description |
|-----------|------|-------------|
| `pathname` | const char * | Chemin du fichier |
| `flags` | int | Mode d'ouverture |
| `mode` | mode_t | Permissions (si création) |

---

## Flags principaux

| Flag | Description |
|------|-------------|
| `O_RDONLY` | Lecture seule |
| `O_WRONLY` | Écriture seule |
| `O_RDWR` | Lecture et écriture |
| `O_CREAT` | Créer le fichier s'il n'existe pas |
| `O_TRUNC` | Vider le fichier à l'ouverture |
| `O_APPEND` | Écrire à la fin du fichier |

---

## Valeur de retour

| Retour | Signification |
|--------|---------------|
| `>= 0` | fd du fichier (3, 4, 5...) |
| `-1` | Erreur |

---

## Exemples

### Lecture seule (pour GNL)
```c
int fd;

fd = open("fichier.txt", O_RDONLY);
if (fd < 0)
    return (NULL);  // Erreur
// Utiliser fd avec read()...
close(fd);
```

### Écriture seule (créer ou écraser)
```c
int fd;

fd = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
if (fd < 0)
    return (1);
write(fd, "Hello\n", 6);
close(fd);
```

### Écriture à la fin (append)
```c
int fd;

fd = open("log.txt", O_WRONLY | O_CREAT | O_APPEND, 0644);
write(fd, "Nouvelle ligne\n", 15);
close(fd);
```

---

## Mon utilisation dans GNL
```c
int main(int argc, char **argv)
{
    int fd;
    char *line;

    fd = open(argv[1], O_RDONLY);  // Ouvre en lecture seule
    if (fd < 0)
        return (1);
    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s", line);
        free(line);
    }
    close(fd);  // Toujours fermer !
    return (0);
}
```

---

## Permissions (mode)

Utilisé avec `O_CREAT` :

| Mode | Description |
|------|-------------|
| `0644` | rw-r--r-- (lecture tous, écriture owner) |
| `0666` | rw-rw-rw- (lecture/écriture tous) |
| `0755` | rwxr-xr-x (exécutable) |

---

## Pièges courants

### 1. Oublier de vérifier l'erreur
```c
fd = open("fichier.txt", O_RDONLY);
read(fd, buffer, 100);  // Si fd = -1, crash !
```

### 2. Oublier de fermer
```c
fd = open("fichier.txt", O_RDONLY);
// ... utilisation ...
// Pas de close(fd) -> fuite de ressources
```

### Corrections
```c
fd = open("fichier.txt", O_RDONLY);
if (fd < 0)
    return (1);  // Gérer l'erreur
// ... utilisation ...
close(fd);  // Toujours fermer !
```

---

## Voir aussi


- [[Langages/C/File descriptors|File descriptors]] — Les fd expliqués
- [[read]] — Lire des données
- [[close]] — Fermer un fichier