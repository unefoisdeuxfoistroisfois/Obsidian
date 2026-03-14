
# La fonction memmem()

---

> Cherche une séquence d'octets (pattern) dans une zone mémoire. Retourne un pointeur vers la première occurrence trouvée.

---

## Prototype
```c
#define _GNU_SOURCE
#include <string.h>

void *memmem(const void *haystack, size_t haystacklen,
             const void *needle, size_t needlelen);
```

/!\ `#define _GNU_SOURCE` doit être **avant** les includes !

---

## Paramètres

| Paramètre | Type | Description |
|-----------|------|-------------|
| `haystack` | const void * | Zone mémoire où chercher |
| `haystacklen` | size_t | Taille de la zone |
| `needle` | const void * | Pattern à chercher |
| `needlelen` | size_t | Taille du pattern |

---

## Valeur de retour

| Retour | Signification |
|--------|---------------|
| `pointeur` | Adresse de la première occurrence |
| `NULL` | Pattern non trouvé |

---

## Exemple simple
```c
char *str = "hello world hello";
char *found;

found = memmem(str, 17, "hello", 5);
// found pointe vers le premier "hello"
```

---

## Mon utilisation dans filter
```c
void ft_remplace_pattern(char *str, int resread, char *pattern, int len)
{
    char *found;
    int i;

    found = memmem(str, resread, pattern, len);
    while (found != NULL)
    {
        i = 0;
        while (i < len)
        {
            found[i] = '*';
            i++;
        }
        found = memmem(str, resread, pattern, len);
    }
}
```

---

## Différence avec strstr

| Fonction | Cherche dans | Arrêt |
|----------|--------------|-------|
| `strstr` | Chaîne de caractères | Au `\0` |
| `memmem` | Zone mémoire | Après `haystacklen` octets |

`memmem` est plus sûr car il ne dépend pas du `\0`.

---

## Piège courant
```c
#include <string.h>
#define _GNU_SOURCE  // Doit être AVANT l'include !
```

### Correction
```c
#define _GNU_SOURCE  // En premier !
#include <string.h>
```

---

## Voir aussi

- [[filter]] — Exercice utilisant memmem