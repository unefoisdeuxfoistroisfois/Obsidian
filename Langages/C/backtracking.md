# Backtracking

> Le backtracking est une technique de résolution qui consiste à essayer toutes les possibilités, et revenir en arrière quand on est bloqué.

---

## Le principe

1. **Essayer** une option
2. **Avancer** (appel récursif)
3. **Si bloqué** -> revenir en arrière et essayer une autre option

---

## Structure de base
```c
void ft_backtracking(...)
{
    // 1. CONDITION DE FIN : on a trouvé une solution
    if (condition_fin)
    {
        // Afficher la solution
        return;
    }

    // 2. ESSAYER chaque possibilité
    while (...)
    {
        // Marquer comme utilisé
        // Appel récursif (avancer)
        // Démarquer (backtrack)
    }
}
```

---

## Les 3 étapes clés

| Étape | Action | Exemple |
|-------|--------|---------|
| Marquer | Réserver l'option | `tab[i] = 1` |
| Avancer | Appel récursif | `ft_back(..., pos + 1, ...)` |
| Backtrack | Libérer l'option | `tab[i] = 0` |

---

## Ordre d'exécution

À chaque appel de `ft_backtracking`, il commence **toujours** par vérifier la condition de fin :
```
ft_backtracking() appelé
        ↓
Vérifie : condition de fin ?
        ↓
    OUI → Affiche + return
    NON → Continue vers la boucle while
```

---

## Le return

Après avoir affiché une solution, le `return` revient à l'appelant :
```c
tmp = str[pos];
str[pos] = ' ';
ft_backtracking(...);  // ← On était ici
str[pos] = tmp;        // ← On continue ici après le return
```

C'est pour ça qu'on trouve **toutes** les solutions !

---

## Conditions de fin selon l'exercice

| Exercice | Condition de fin |
|----------|------------------|
| Permutations | `pos == size` |
| N-Queens | `colonne == n` |
| Rip | `fix == must_fix && équilibré` |

---

## Exemple visuel avec "ab"
```
ft_backtracking(pos=0)
│
├─ Essaie 'a' (tab=[1,0], newstr="a_")
│  │
│  └─ ft_backtracking(pos=1)
│     │
│     └─ Essaie 'b' (tab=[1,1], newstr="ab")
│        │
│        └─ pos == size → AFFICHE "ab" ✓
│        │
│        └─ Backtrack: tab[1]=0
│
│  └─ Backtrack: tab[0]=0
│
├─ Essaie 'b' (tab=[0,1], newstr="b_")
│  │
│  └─ ft_backtracking(pos=1)
│     │
│     └─ Essaie 'a' (tab=[1,1], newstr="ba")
│        │
│        └─ pos == size → AFFICHE "ba" ✓
│        │
│        └─ Backtrack: tab[0]=0
│
│  └─ Backtrack: tab[1]=0

FIN -> Affiché "ab" et "ba"
```

---

## Exemple avec rip "(())"
```
ft_back(pos=0, fix=0)
│
├─ str = " ()"              // Supprime 1ère (
├─ ft_back(pos=1, fix=1)
│   │
│   └─ fix == nombreasupp && équilibré ?
│   └─ OUI → AFFICHE " ()" ✓
│   └─ return
│
├─ str = "(()"              // Backtrack : remet '('
├─ pos++
│
├─ str = "( )"              // Supprime 2ème (
├─ ft_back(pos=2, fix=1)
│   │
│   └─ fix == nombreasupp && équilibré ?
│   └─ OUI → AFFICHE "( )" ✓
│   └─ return
│
├─ str = "(()"              // Backtrack : remet '('
└─ ... continue

FIN → Affiché " ()" et "( )"
```

---

## Exercices utilisant le backtracking

- [[permutations]] — Toutes les combinaisons d'une chaîne
- [[n_queens]] — Placer N reines sur un échiquier
- [[rip]] — Équilibrer des parenthèses

---