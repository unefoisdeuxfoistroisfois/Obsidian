# Vim - Notes et astuces

## Suppression avec motions

### `dt` vs `df`
- **`dt<char>`** → Delete **to** (jusqu'à) le caractère **sans l'inclure**
  - Exemple : `dts` supprime tout avant le `s` (le `s` reste)
- **`df<char>`** → Delete **find** (trouve) le caractère **en l'incluant**
  - Exemple : `dfs` supprime tout jusqu'au `s` (le `s` est supprimé aussi)

## Incrémenter/Décrémenter des nombres

- **`Ctrl+a`** → Incrémente le nombre sous le curseur (+1)
- **`Ctrl+x`** → Décrémente le nombre sous le curseur (-1)

Exemple :
```
42  →  Ctrl+a  →  43
42  →  Ctrl+x  →  41
```

## Recherche et modification de mots

### Sélectionner toutes les occurrences d'un mot

**`*`** → Cherche le mot sous le curseur dans tout le fichier

**Workflow de remplacement rapide :**
1. Place le curseur sur un mot
2. Appuie sur `*` → Toutes les occurrences sont surlignées
3. Appuie sur `n` pour aller à l'occurrence suivante
4. Tape `cw` pour changer le mot (**c**hange **w**ord)
5. Écris le nouveau mot et appuie sur `Esc`
6. Va aux occurrences suivantes avec `n`
7. Appuie sur `.` pour répéter le changement automatiquement

**Exemple pratique :**
```
hello world, hello everyone, hello there
```
1. Curseur sur `hello`
2. `*` → Toutes les occurrences de "hello" surlignées
3. `cw` → Change le premier "hello"
4. Écris "salut" + `Esc`
5. `n` → Va au prochain "hello"
6. `.` → Répète automatiquement "salut"
7. Continue avec `n` puis `.`

**Résultat :** `salut world, salut everyone, salut there`

## Astuces

- `#` → Même chose que `*` mais cherche en **arrière** (vers le haut)
- `gn` → Sélectionne visuellement la prochaine occurrence
- `cgn` → Change directement la prochaine occurrence (encore plus rapide !)