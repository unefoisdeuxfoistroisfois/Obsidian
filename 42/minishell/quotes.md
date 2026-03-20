# Gestion des Quotes - Minishell

## Principe de base

Quand tu parcours la ligne, tu dois tracker si tu es "dans" une quote ou pas.

```
echo 'hello world' test
     ^     ^     ^
     |     |     |
     1     2     3

1 = quote ouverte → on ignore les espaces
2 = espace mais on est dans quote → on continue
3 = quote fermée → les espaces redeviennent des séparateurs
```

## Différence entre ' et "

| Quote | Comportement |
|-------|-------------|
| `'...'` | Tout est littéral, `$VAR` reste `$VAR` |
| `"..."` | Expansion de `$VAR` en sa valeur |

## Fonctions créées

### ft_word() — modifié pour gérer les quotes
```c
int	ft_word(char *str, int index)
{
	char	quote;

	quote = 0;
	while (str[index] != '\0')
	{
		if ((str[index] == '\'' || str[index] == '"') && quote == 0)
			quote = str[index];        // ouvre quote
		else if (str[index] == quote)
			quote = 0;                 // ferme quote
		if (str[index] == ' ' && quote == 0)
			break ;
		if (ft_operateur(str, index) && quote == 0)
			break ;
		index++;
	}
	return (index);
}
```

### ft_remove_quotes() — retire les quotes du token
```c
char	*ft_remove_quotes(char *str)
{
	char	*result;
	char	quote;
	int		i;
	int		j;

	result = malloc(ft_strlen(str) + 1);
	if (!result)
		return (NULL);
	quote = 0;
	i = 0;
	j = 0;
	while (str[i])
	{
		if ((str[i] == '\'' || str[i] == '"') && quote == 0)
			quote = str[i];
		else if (str[i] == quote)
			quote = 0;
		else
			result[j++] = str[i];
		i++;
	}
	result[j] = '\0';
	return (result);
}
```

**Exemples :**
```
'hello world'  ->  hello world
"test"         ->  test
'l'"o"'l'      ->  lol
```

### ft_check_quotes() — détecte les quotes non fermées
```c
int	ft_check_quotes(char *str)
{
	char	quote;
	int		i;

	quote = 0;
	i = 0;
	while (str[i])
	{
		if ((str[i] == '\'' || str[i] == '"') && quote == 0)
			quote = str[i];
		else if (str[i] == quote)
			quote = 0;
		i++;
	}
	if (quote != 0)
		return (1);
	return (0);
}
```

**Retour :** `0` = quotes OK, `1` = quote non fermée

**Utilisation dans ps1.c :**
```c
if (ft_check_quotes(line))
{
	printf("minishell: syntax error: unclosed quote\n");
	free(line);
	return ;
}
```

## Note importante

Le sujet 42 dit qu'on ne gère pas les quotes multilignes. Bash affiche `>` et attend la suite, mais minishell doit juste afficher une erreur.

## Fichiers concernés

- `quotes.c` — contient `ft_remove_quotes()` et `ft_check_quotes()`
- `args.c` — `ft_word()` modifié pour gérer les quotes
- `ps1.c` — appelle `ft_check_quotes()` avant le lexer