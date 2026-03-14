# broken_GNL

> Le but de l'exercice est de corriger un GNL bien cassé.

---

## Main

Création d'un main :
1. Utiliser la fonction [[open]] pour lire dans un fichier
2. Une boucle `while` qui lit chaque ligne grâce à GNL
3. Stocker le résultat de GNL dans une variable `char *`
4. Tant que le `char *` n'est pas égal à `NULL`, on continue
5. Afficher la ligne avec `printf`
6. Ne pas oublier de fermer le fichier avec [[close]]

```c
int main(int argc, char **argv)
{
    int fd;
    char *line;

    fd = open(argv[1], O_RDONLY);
    if (fd < 0)
        return (1);
    while ((line = get_next_line(fd)) != NULL)
    {
        printf("%s", line);
        printf("OKOKOK\n");  // Debug visuel
        free(line);
    }
    close(fd);
    return (0);
}
```

---

## Corrections à apporter

### ft_strchr
- Vérifier si `s[i]` existe avant de comparer
```c
// Avant
while (s[i] != c)

// Après
while (s[i] && s[i] != c)
```

---

### ft_memcpy
- Remettre la boucle dans le bon ordre (croissant)
- Créer un index de type `size_t`, l'initialiser à 0
- Changer les `n - 1` en `i`
```c
// Avant
while (--n > 0)
    ((char *)dest)[n - 1] = ((char *)src)[n - 1];

// Après
size_t i = 0;
while (i < n)
{
    ((char *)dest)[i] = ((char *)src)[i];
    i++;
}
```

---

### str_append_mem
- Appeler `ft_strlen` et le premier `ft_memcpy` seulement si `*s1` existe
```c
// Avant
size_t size1 = ft_strlen(*s1);

// Après
size_t size1 = 0;
if (*s1)
    size1 = ft_strlen(*s1);
```

---

### ft_memmove
- Inverser le premier `>` en `<`
- Remplacer le `strlen` en `n`
- Mettre le `i--` au-dessus de l'assignation
```c
// Avant
if (dest > src)
size_t i = ft_strlen((char *)src) - 1;
while (i >= 0)
{
    ((char *)dest)[i] = ((char *)src)[i];
    i--;
}

// Après
if (dest < src)
size_t i = n;
while (i > 0)
{
    i--;
    ((char *)dest)[i] = ((char *)src)[i];
}
```

---

### get_next_line
- Reset le buffer → `b[0] = '\0'`
- Gestion de l'EOF → si `read_ret == 0` on `break`
- Mise à jour de `tmp` → `tmp = ft_strchr(b, '\n')`
- Ajouter un `if (tmp)` avec `ft_memmove`
- Ajouter un `else` pour gérer la fin
```c
// Dans la boucle while
b[0] = '\0';  // Reset buffer
int read_ret = read(fd, b, BUFFER_SIZE);
if (read_ret == -1)
    return NULL;
if (read_ret == 0)  // EOF
    break;
b[read_ret] = 0;
tmp = ft_strchr(b, '\n');  // Mise à jour tmp

// Après la boucle
if (tmp)
{
    if (!str_append_mem(&ret, b, tmp - b + 1))
    {
        free(ret);
        return NULL;
    }
    ft_memmove(b, tmp + 1, ft_strlen(tmp + 1) + 1);
}
else
{
    b[0] = '\0';  // Reset buffer
    if (!ret || !*ret)
    {
        free(ret);
        return NULL;
    }
}
```

---

## Résumé des bugs

| Fonction | Bug | Correction |
|----------|-----|------------|
| `ft_strchr` | Pas de vérif `\0` | Ajouter `s[i] &&` |
| `ft_memcpy` | Ordre décroissant | Utiliser `i` croissant |
| `str_append_mem` | `*s1` peut être NULL | Vérifier avant `ft_strlen` |
| `ft_memmove` | Boucle infinie `size_t` | `i > 0` et `i--` avant |
| `get_next_line` | `tmp` jamais mis à jour | Ajouter dans la boucle |

---

## Voir aussi

- [[open]] — Ouvrir un fichier
- [[close]] — Fermer un fichier
- [[File descriptors]] — Les fd expliqués