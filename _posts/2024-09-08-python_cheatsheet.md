---
title: "python cheat sheet"
date: 2024-09-08
permalink: /posts/2024/09/08/python/
tags:
  - python
  - til
---

Aujourd'hui, j'ai dû coder un petit script python alors que cela faisait
plusieurs mois que je n'en avais pas fait.
J'ai essayé de retrouver les deux feuilles de [cheatsheet](https://perso.limsi.fr/pointal/_media/python:cours:mementopython3-english.pdf) que je m'étais
imprimé, en vain. J'ai dû consulter quelques scripts plus anciens pour retrouver ce
savoir faire.

Ce post a pour but de rassembler quelques points dont j'ai régulièrement besoin,
mais dont j'ai un peu de mal à me rappeler. Ceci est mon pense-bête avec
l'intention secondaire d'être un brouillon pour les vidéos youtube que j'aurais
voulu trouver sur ces sujets.

# boiler plate

Un script python doit commencer par les lignes

```python
#! /usr/bin/env python
# -*- coding: utf-8 -*-
from __future__ import print_function
```

La première ligne, le "hash bang" indique l'exécutable qui doit interpréter le
script. Si on doit utiliser le python installé dans le système, on précise
`/usr/bin/python`. Si on préfére utiliser le python dans le path, on utilise `env`.
Ce dernier cas est le plus fréquent.

La seconde ligne précise l'encodage du texte. [pep 263](https://peps.python.org/pep-0263/)
En python 2, l'encodage par défaut est Latin-1. C'est devenu utf-8 depuis
python 3.15 [pep 686](https://peps.python.org/pep-0686/).

La troisième ligne permet d'utiliser la nouvelle syntaxe de l'instruction print
même en python 2.

Les lignes 2 et 3 sont utiles pour permettre d'avoir des scripts qui fonctionnent
aussi bien en python 2 qu'en python 3 et d'utiliser des éditeurs modernes
(aujourd'hui, utf-8 s'est imposé comme le meilleur encodage).

Python 2 est presque mort, il est donc important de s'habituer à la nouvelle
commande print. Si vous êtes déjà à 100% en python 3, oubliez la ligne 3.
Pour moi cette ligne 3 sert à indiquer si un script est portable. Si on code
un nouveau script en Python 2, il me parait normal de l'écrire portable.

# argument parsing

Comme je n'avais aucune contrainte sur le passage d'arguments, j'ai utilisé la
solution la plus simple : le module [argparse](https://docs.python.org/3/library/argparse.html)
(installé par défaut en python 2 et 3).

```python
import argparse

if __name__ == "__main__":
  parser = argparse.ArgumentParser(description='Titre du script')
  parser.add_argumen('file_name', type=str, help='Imported file name')
  parser.add_argument('airspace_name', type=str, help='Airspace name')
  parser.add_argument('traffic_name', type=str, help='Traffic name')
  args = parser.parse_args()
  main(args.file_name, args.airspace_name, args.traffic_name)
```

# csv reading et collections.namedtuple

Le fichier que je dois parser ressemble à du csv. Les champs sont séparés par
des points-virgules et la première ligne définit les titres des colonnes.
La différence, c'est que certaines colonnes sont répétées. Cela permet pour
un vol de donner la liste des points qu'il parcourt.
Malgré cette différence le module [csv](https://docs.python.org/3/library/csv.html) est utilisable.

```python
import csv, collections
with open (file_name, 'r') as file:
  reader = csv.reader(file, delimiter=';')
  headers = next(reader)
  field_names = list(set(headers))
  field_names.sort(key=lambda x:headers.index(x))

  Traf = collections.namedtuple('Traf', field_names)

  data = {}
  for row in reader:
    v = {}
    for header, value in zip (headers, row):
      v.setdefault(header, []).append(value)
    v = { k: v[k][0] if headers.count(k) == 1 else v[k] for k in v }
    data[v['CALLS']] = Traf(*[ v[f] for f in field_names ])
  # return Traf, data
```

J'ai donc récupéré les titres des colonnes. Dans field_names, j'ai supprimé les
doublons.
J'ai utilisé [namedtuple](https://realpython.com/python-namedtuple/). Cela réduit la
verbosité du code (donc le rend plus lisible pour ceux qui connaissent namedtuple).

On remarque l'utilisation du `setdefault` qui est régulièrement utilisé pour
compenser l'absence de l'autovivification en python.
On remarque l'utilisation du `expression if` qui est aussi moche en python que
ne l'est le `?` en C:

```python
resultat = valeur_1 if condition else valeur_2
```

Le remplacement des listes par la première valeur utilise une [compréhension de
dict](https://www.pythoniste.fr/python/comprendre-les-comprehensions-en-python/).

L'utilisation du `*` pour passer un tableau en paramètre est aussi à noter.

L'utilisation du zip est pratique quand les tableaux ont la même taille.
L'alternative est `enumerate`.

```python
    for idx, value in enumerate(row):
      header = headers[idx]
      ...
```

# lecture de données Oracle

Je suis plutôt contre l'utilisation d'ORM pour accéder aux données. C'est un
intermédiaire qui donne l'impression que l'on peut se passer d'apprendre SQL.
C'est faux, il faut bien apprendre SQL et lorsqu'on connait bien SQL,
l'utilisation d'un ORM se révèle très peu utile car l'économie de code est
compensée par le temps d'apprentissage de l'ORM du jour et parce qu'on
introduit une dépendance supplémentaire au projet qu'il faudra mettre
à jour et retester régulièrement.

```python
import cx_Oracle
database = 'USER/PASSWORD@DB_INSTANCE'

def read_routes(connection, airspace_name):
  cursor = connection.cursor()
  cursor.execute('''select ROUTE_NAME, POINT_NAME#GEO_PT from RTE_SECT_GEO_PT where AIRSPACE_ENV_NAME = :airspace order by 1, 2''', [airspace_name])
  routes = {}
  for route_name, point_name in cursor:
    routes.setdefault(route_name, []).append(point_name)
  return routes
def main(file_name, airspace_name, traffic_name):
  connection = cx_Oracle.connect(database)
  routes = read_routes(connection, airspace_name)
  ...
```

Pas grand chose à dire, si ce n'est le passage d'arguments pour les requêtes. Ne
jamais construire la string utilisée pour la requête.

# autres

Il y a encore plein d'autres aspects et trucs et astuces importants à connaître
en python.

## polynome d'interpolation

```python
import numpy as np
sequence = [1, 4, 8, 13, 19, 26]
# Fit a polynomial to the sequence
poly = np.polynomial.Polynomial.fit(np.arange(len(sequence)), sequence, deg=len(sequence)-1)
print(str(poly.convert()))
print( poly(len(sequence)))
```

## python -i script.py

Exécute le script et reste sous python pour inspecter l'état des variables globales
ou appeler les fonctions. L'utilisation du prompt en python pour faire des tests
est une bonne pratique. Cela incite à structurer le code de manière à permettre
ces tests.

## dir(object)

Renvoit les propriétés et méthodes de l'objet.

## type(object)

Renvoit le type de l'objet. Par exemple
`<class 'numpy.polynomial.polynomial.Polynomial'>`
`<class 'bool'>`

## lancer un serveur web statique sur le répertoire courant

python 3: `python -m http.server`
python 2: `python -m SimpleHTTPServer`

## pip

`pip install package` permet d'installer un nouveau package. Souvent cela nécessite des permissions (sudo).
Une alternative, c'est l'option --user qui installe spécifiquement pour l'utilisateur.

`pip install -r requirements.txt` installe un ensemble de packages.
`pip freeze > requirements.txt` permet de créer le fichier.
