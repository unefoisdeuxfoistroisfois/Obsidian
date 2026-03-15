# Exam Rank 03

> Il y'a 2 niveaux :  **la lecture** et **le backtracking**

---

## Niveau 1 — La lecture

| Exercice | Concept         | fd         |
| -------- | --------------- | ---------- |
| filter   | stdin + pattern | 0 → 1      |
| gnl      | lecture ligne   | fd fichier |

Le niveau 1 est consitué de 2 exercice il y'a [[filter|filter]] et [[broken_GNL|broken_GNL]], ils sont basé sur la lecture soit par file descriptore ou depuis la ligne de commande ou soit direcment depuis un fichier.
### gnl
- Buffer statique
- Retourne une ligne à la fois

---

## Niveau 2 — Le backtracking

| Exercice    | Principe                |
| ----------- | ----------------------- |
| permutation | Toutes les combinaisons |
| n_queens    | Placer N reines         |
| rip         | Équilibrer parenthèses  |

### Structure backtracking
```c
void ft_back(...)
{
    if (condition_fin)
    {
        // Afficher solution
        return;
    }
    // Essayer
    // Appel récursif
    // Annuler (backtrack)
}
	```