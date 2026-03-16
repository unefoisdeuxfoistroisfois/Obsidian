# rip

> Cette exercices pour but d'équilibrer des parenthèses en supprimant le **minimum** de parenthèses nécessaire, et afficher **toutes les solutions possibles**.

---

## Étapes

1. Compter le nombre de parenthèses à supprimer avec `ft_balance`
2. Commencer le backtracking avec `pos = 0` et `fix = 0`
3. Dans le backtracking, parcourir la chaîne
4. Si on trouve `(` ou `)`, essayer de le supprimer (remplacer par espace)
5. Quand `fix == must_fix` et chaîne équilibrée ->
6. Backtrack : remettre le caractère original

---

## ft_balance

Compte combien de parenthèses sont mal placées :

| Retour | Signification |
|--------|---------------|
| `0` | Déjà équilibré |
| `> 0` | Nombre à supprimer |

```c
int ft_balance(char *str)
{
    int i;
    int ouvert;
    int fermer;

    i = 0;
    ouvert = 0;
    fermer = 0;
    while (str[i] != '\0')
    {
        if (str[i] == '(')
            ouvert++;
        else if (str[i] == ')')
        {
            if (ouvert > 0)
                ouvert--;  // Cette ) ferme une (
            else
                fermer++;  // ) en trop
        }
        i++;
    }
    return (ouvert + fermer);
}
```

---

## Le backtracking

| Paramètre | Description |
|-----------|-------------|
| `str` | Notre chaîne donnée en argument |
| `must_fix` | Le nombre d'éléments à supprimer (provenant de `ft_balance`) |
| `pos` | Position actuelle dans la chaîne, +1 à chaque récursion |
| `fix` | Compteur du nombre de parenthèses déjà supprimées |

```c
void ft_backtracking(char *str, int must_fix, int pos, int fix)
{
    char tmp;

    // 1. Condition de fin : on a supprimé assez ET c'est équilibré
    if (fix == must_fix && ft_balance(str) == 0)
    {
        puts(str);
        return;
    }

    // 2. Parcourir la chaîne
    while (str[pos] != '\0')
    {
        if (str[pos] == '(' || str[pos] == ')')
        {
            tmp = str[pos];      // Sauvegarder
            str[pos] = ' ';      // Supprimer (remplacer par espace)
            ft_backtracking(str, must_fix, pos + 1, fix + 1);  // Avancer
            str[pos] = tmp;      // Backtrack : remettre
        }
        pos++;
    }
}
```

---

## ft_rip
```c
void ft_rip(char *str)
{
    int must_fix;

    must_fix = ft_balance(str);
    ft_backtracking(str, must_fix, 0, 0);
}
```

---

## Exemple avec `"(()"`
```
must_fix = 1 (1 parenthèse en trop)

ft_back(pos=0, fix=0)
│
├─ str = " ()"              // Supprime 1ère (
├─ ft_back(pos=1, fix=1)
│   └─ fix(1) == must_fix(1) && équilibré → AFFICHE " ()"
│   └─ return
│
├─ str = "(()"              // Backtrack : remet '('
├─ pos++
│
├─ str = "( )"              // Supprime 2ème (
├─ ft_back(pos=2, fix=1)
│   └─ fix(1) == must_fix(1) && équilibré → AFFICHE "( )"
│   └─ return
│
├─ str = "(()"              // Backtrack : remet '('
└─ fin

Résultat : " ()" et "( )"
```

---

## Voir aussi

- [[backtracking]] — Le principe du backtracking
- [[permutations]] — Autre exercice de backtracking
- [[n_queens]] — Autre exercice de backtracking