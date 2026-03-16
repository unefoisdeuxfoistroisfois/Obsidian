# permutations

> Cet exercice a pour but de créer toutes les possibilités d'une chaîne reçue en argument.

---

## Étapes

1. Prendre la taille de la chaîne
2. La trier grâce à la taille qu'on a (ça va nous faciliter l'ordre alphabétique)
3. Allocation pour le tableau qui va vérifier la disponibilité (`tab`)
4. Allocation pour les nouvelles chaînes permutées (`newstr`)
5. Compléter notre tableau de disponibilité en mettant partout 0
6. Commencer le backtracking
7. Ne pas oublier de `free` à la fin

---

## Les allocations

| Variable | Type | Rôle |
|----------|------|------|
| `tab` | `int *` | Marque quels caractères sont utilisés (0 = libre, 1 = pris) |
| `newstr` | `char *` | Construit la nouvelle chaîne |

> Pas besoin de `'\0'` pour `newstr` car on utilise `write` avec la taille.

---

## Le backtracking

```c
void ft_backtracking(char *str, int *tab, char *newstr, int pos, int size)
{
    int i;

    // 1. Condition de fin : on a rempli toutes les positions
    if (pos == size)
    {
        write(1, newstr, size);
        write(1, "\n", 1);
        return;
    }

    // 2. Essayer chaque possibilité
    i = 0;
    while (i < size)
    {
        if (tab[i] == 0)  // Si caractère libre
        {
            tab[i] = 1;             // Marquer comme pris
            newstr[pos] = str[i];   // Placer le caractère
            ft_backtracking(str, tab, newstr, pos + 1, size);  // Avancer
            tab[i] = 0;             // Backtrack : libérer
        }
        i++;
    }
}
```

Le backtracking aura besoin de la chaine actuelle, du tab pour testé, la nouvelle chaine ou copié, la position et la taille de la chaine.

## Voir aussi

- [[backtracking]] — Le principe du backtracking