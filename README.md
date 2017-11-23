# Setup
See `environment.yml` for dependencies.

Generate `antlr4` classes from our grammar:
```bash
antlr4 -o gen -listener -visitor -Dlanguage=Python3 GrammaireSQL.g4
```

# Description des objectifs
Gérer des requêtes portant sur les articles, éventuellement avec des contraintes de contenu et ou de temporalité.

Exemples de phrases que nous souhaitons être capables de gérer:
* Affiche-moi les articles qui contiennent des actualités.
* Afficher les articles plus vieux que 2013.
* Articles parlant d'innovation.
* Donner les articles parus en 2011.
* Donnez moi les articles sur le diabète

Ce que nous décidons de ne pas gérer:
* Toutes les aggrégations sur les auteurs des articles.
* Toutes les aggrégations sur les rubriques des articles.
* Les requêtes dont la structure (l'ordre des éléments) change complètement de celle que nous avons pour base (la première requête des exemples précedents).


# Documentation
Tables de la base de données
http://www4.utc.fr/~lo17/faq.html#faq10

## Architecture
Architecture overview:
![alt text](./documentation/natural_to_sql.png?raw=true)

On a donc deux lexiques différents: un pour la structure des requêtes et un pour les paramètres de la partie WHERE de la requête SQL finale.

## Pre-parser
Statégie du pre-parser:
![alt text](./documentation/pre_parser.png?raw=true)

Détails sur l'algorithme du pre-parser, l'`Organisateur`:
* Dans "Je `veux` article contenir `afficheur`", `veux` sera interprété comme un mot de la structure signifiant `vouloir`, tandis que `afficheur` venant après l'action de `vouloir` sera interprété comme un paramètre.
* TODO

## Grammaire Antlr
### Exemple d'arbre
vouloir article contenir plastique et universite parution 2015

![alt text](./documentation/request_example_tree.png)


# Questions:
On a dit que:

Dans "Je `veux` article contenir `afficheur`", `veux` sera interprété comme un mot de la structure signifiant `vouloir`, tandis que `afficheur` venant après l'action de `vouloir` sera interprété comme un paramètre.

Mais que faire pour:

Je `veux` article `parlant` de toto
`parlant` vient après `veux`, et donc sera considéré comme paramètre, cherché dans le lexique où il ne figure pas.

Comment intégrer tous les types de mot participant à la structure ?
