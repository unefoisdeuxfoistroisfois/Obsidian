# filter

Le but de l'exercice est de remplacer les patterns d'un texte par des `*`.

---

## Étapes

1. Prendre la taille du mot à trouver
2. Un `malloc` pour notre nouvelle chaîne qui aura la taille du `BUFFER_SIZE`
3. Une boucle tant que notre lecture est supérieure à 0 (si on passe en dessous, c'est qu'il n'y a plus rien à lire)
4. Appeler `ft_remplace_pattern` qui va remplacer les patterns par `*` grâce à la fonction `memmem`

---

## ft_remplace_pattern

Elle a besoin de :
- La nouvelle chaîne
- Le `resread` (nombre d'octets lus)
- Le mot à chercher
- La taille du pattern

On utilise 2 boucles :
- Une pour continuer tant qu'on trouve le pattern (`while (found != NULL)`)
- Une pour remplacer chaque caractère par `*` (`while (i < pattern_len)`)

### Deux façons de modifier la chaîne

**1. Modifier en mémoire (pas de return) :**
```c
void ft_remplace_pattern(char *str, int resread, char *pattern, int len)
{
    // ...
    found[i] = '*';  // Modifie directement en mémoire
}

// Appel :
ft_remplace_pattern(newstr, resread, motatrouver, reslen);
write(1, newstr, resread);  // newstr est déjà modifié
```

**2. Avec return :**
```c
char *ft_remplace_pattern(char *str, int resread, char *pattern, int len)
{
    // ...
    found[i] = '*';
    return (str);  // Retourne la chaîne modifiée
}

// Appel :
newstr = ft_remplace_pattern(newstr, resread, motatrouver, reslen);
write(1, newstr, resread);
```

> Les deux méthodes donnent le même résultat car on modifie la même adresse mémoire. Le `return` est surtout utile si on crée une nouvelle chaîne avec `malloc`.

---

## Vérification des arguments

Ne pas oublier de vérifier si on donne bien un élément à lire :
```c
if (argc != 2 || argv[1][0] == '\0')
    return (1);
```

⚠️ C'est `'\0'` (caractère nul), pas `'0'` (le chiffre zéro) !

---

## Gestion des erreurs

En cas d'erreur, on écrit dans la sortie erreur qui est le [[Langages/C/File descriptors|File descriptor]] 2 (stderr).

Deux choix :

**Avec write :**
```c
write(2, "Error\n", 6);  // stderr = fd 2
```

**Avec perror :**
```c
perror("Error");  // Affiche "Error: <message système>"
```

---

## Résumé

| Étape                 | Description                    |
| --------------------- | ------------------------------ |
| `read(0, ...)`        | Lit depuis stdin (fd 0)        |
| `ft_remplace_pattern` | Remplace le pattern par `*`    |
| `write(1, ...)`       | Affiche le résultat sur stdout |
| `write(2, ...)`       | Affiche les erreurs sur stderr |

---

## Voir aussi

- [[Langages/C/Read|Read]] — Lire des données
- [[Langages/C/File descriptors|File descriptors]] — Les fd expliqués
- [[memmem]] — Trouver un pattern dans une chaîne