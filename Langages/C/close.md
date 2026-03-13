# close

---

> Ferme un file descriptor ouvert avec `open()`.

---

## Prototype
```c
#include <unistd.h>

int close(int fd);
```

---

## Paramètre

| Paramètre | Type | Description |
|-----------|------|-------------|
| `fd` | int | File descriptor à fermer |

---

## Valeur de retour

| Retour | Signification |
|--------|---------------|
| `0` | Succès |
| `-1` | Erreur |

---

## Exemple
```c
int fd;

fd = open("fichier.txt", O_RDONLY);
if (fd < 0)
    return (1);
// ... utilisation avec read() ...
close(fd);  // Toujours fermer !
```

---

## Piège courant
```c
fd = open("fichier.txt", O_RDONLY);
read(fd, buffer, 100);
// Pas de close(fd) → fuite de ressources
```

---

## Voir aussi

- [[open]] — Ouvrir un fichier