# Afficher une chaîne

---

> Il existe plusieurs façons d'afficher une chaîne de caractères.

---

## Les 3 fonctions principales

| Fonction | Include | Besoin de `'\0'` | Retour à la ligne |
|----------|---------|------------------|-------------------|
| `printf` | `<stdio.h>` | Oui | Non (sauf `\n`) |
| `puts` | `<stdio.h>` | Oui | Oui (automatique) |
| `write` | `<unistd.h>` | Non | Non |

---

## printf
```c
#include <stdio.h>

printf("%s", str);    // Sans retour à la ligne
printf("%s\n", str);  // Avec retour à la ligne
```

- Besoin du `'\0'` pour savoir où s'arrêter
- Peut formater avec `%d`, `%c`, `%s`, etc.

---

## puts
```c
#include <stdio.h>

puts(str);  // Ajoute automatiquement un '\n'
```

- Besoin du `'\0'` pour savoir où s'arrêter
- Ajoute automatiquement un retour à la ligne
- Plus simple que `printf` pour une chaîne seule

---

## write
```c
#include <unistd.h>

write(1, str, len);   // Affiche len caractères
write(1, "\n", 1);    // Retour à la ligne manuel
```

- Pas besoin du `'\0'` (on donne la taille)
- Choix du fd (1 = stdout, 2 = stderr)
- Plus bas niveau

---

## Afficher toute la chaîne vs caractère par caractère

### Toute la chaîne
```c
printf("%s", str);
puts(str);
write(1, str, len);  // str est déjà une adresse
```

### Caractère par caractère
```c
printf("%c", str[i]);
write(1, &str[i], 1);  // & car str[i] est un char, pas une adresse
```

---

## Exemples comparés
```c
char *str = "hello";
int len = 5;

// Avec printf
printf("%s\n", str);      // Affiche: hello\n

// Avec puts
puts(str);                // Affiche: hello\n (auto)

// Avec write
write(1, str, len);       // Affiche: hello
write(1, "\n", 1);        // Affiche: \n
```

---

## Quelle fonction choisir ?

| Situation | Fonction |
|-----------|----------|
| Affichage simple avec `'\0'` | `puts` |
| Affichage formaté (%d, %s...) | `printf` |
| Pas de `'\0'` / choix du fd | `write` |
| Examen 42 (souvent limité) | `write` |

---

## Pièges courants

### Oublier la taille avec write
```c
write(1, str);  // Il manque la taille !
```

### Oublier le '\n' avec printf et write
```c
printf("%s", str);   // Pas de retour à la ligne
write(1, str, len);  // Pas de retour à la ligne
```

### Oublier le '\0' avec printf et puts
```c
char *str = malloc(5);
str[0] = 'h';
str[1] = 'e';
str[2] = 'l';
str[3] = 'l';
str[4] = 'o';
puts(str);  // Pas de '\0' -> comportement indéfini !
```

---

## Voir aussi

- [[write]] — Écrire des données
- [[quand_mettre_le_'\0']] — Quand ajouter le '\0'
- [[file_descriptors]] — Les fd expliqués