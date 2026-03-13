# Les File Descriptors (fd)

> Les fd sont des numéros qui représentent des flux de données.

---

## Les 3 fd standards

| fd | Nom | Direction | Description |
|----|-----|-----------|-------------|
| 0 | stdin | Entrée | Ce qu'on reçoit |
| 1 | stdout | Sortie | Affichage normal |
| 2 | stderr | Sortie | Messages d'erreur |

---

## stdin (fd 0) — 3 façons de l'utiliser

### 1. Avec un pipe `|`

```bash
echo "hello" | ./a.out
```

### 2. Avec une redirection `<`

```bash
./a.out < fichier.txt
```

### 3. En tapant au clavier

```bash
./a.out
hello       # ← tu tapes
Ctrl+D      # ← fin (EOF)
```

---

## fd 3, 4, 5... — Fichiers ouverts
```c
int fd;
int fd2;

fd = open("fichier.txt", O_RDONLY);  // fd = 3
fd2 = open("fichier.txt", O_RDONLY);  // fd = 4
```

Le système donne le premier fd disponible.

---

## Arguments vs stdin

| Méthode | Comment récupérer |
|---------|-------------------|
| `./a.out hello` | `argv[1]` |
| `echo "hello" \| ./a.out` | `read(0, ...)` |

/️!\ **Les arguments ne sont pas des fd !**

---

## Code exemple
```c
// Lire depuis stdin
read(0, buffer, BUFFER_SIZE);

// Écrire vers stdout
write(1, buffer, len);

// Écrire vers stderr (erreurs)
write(2, "Error\n", 6);

// Ouvrir un fichier
int fd = open("test.txt", O_RDONLY);  // fd = 3
read(fd, buffer, BUFFER_SIZE);
close(fd);
```