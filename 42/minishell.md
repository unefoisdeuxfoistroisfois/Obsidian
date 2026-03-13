# minishell

Le projet minishell est un projet ou on doit repproduire un terminal sous le langage bash, il faut savoir que le projet est séparé en 2 grande partie le **parsing** et l'**exec** Ceci sa permet de mieux donné les taches a son mate.

---

## Compilation
```bash
make
```

Le Makefile détecte automatiquement l'OS (Linux ou macOS) et utilise la bonne version de MiniLibX.

## Utilisation
```bash
./minishell
```

On arrive dans notre Shell et de la on peut lancer les commandes. Je l'ai appelé "minibiendur".

---

# Parsing (Bradley)

## Lexer
- [x] Lecture input (readline)
- [x] Historique (add_history)
- [x] Sauter les espaces
- [x] Détecter les mots
- [x] Détecter les opérateurs (| < > << >>)
- [x] Stocker tokens dans t_list

## Structures créées
- [x] Enum t_token_type (WORD, PIPE, REDIR_IN, REDIR_OUT, HEREDOC, APPEND)
- [x] Structure t_token (value + type)

## Fonctions créées
- [x] ft_create_token() - crée un token
- [x] ft_get_op_type() - retourne le type d'opérateur
- [x] ft_operateur() - détecte la taille d'un opérateur
- [x] ft_word() - détecte la fin d'un mot
- [x] ft_space() - saute les espaces
- [x] ft_lexer() - découpe la ligne en tokens
- [x] ft_print_tokens() - affiche les tokens (debug)

## Flux du lexer
```
Input: "ls -la | grep txt"
         ↓
    ft_lexer()
         ↓
    [ls] -> [-la] -> [|] -> [grep] -> [txt]
         ↓
    ft_print_tokens()
         ↓
    type: 0 | value: ls
    type: 0 | value: -la
    type: 1 | value: |
    type: 0 | value: grep
    type: 0 | value: txt
```

## Quotes
- [ ] Gérer les double quotes ("")
- [ ] Gérer les single quotes ('')

## Expansion
- [ ] Expand $VARIABLE
- [ ] Expand $?

## Validation syntaxe
- [ ] Erreur si pipe au début
- [ ] Erreur si pipe à la fin
- [ ] Erreur si redirection sans fichier
- [ ] Erreur si double pipe (| |)

## Commandes
- [ ] Créer structure t_cmd
- [ ] Transformer tokens en t_cmd
- [ ] Lier les commandes (pipes)

---

# Exec (Oussama)

---

# Prochaines étapes

1. Gérer les quotes
2. Validation syntaxe
3. Expansion des variables
4. Structure t_cmd pour l'exec