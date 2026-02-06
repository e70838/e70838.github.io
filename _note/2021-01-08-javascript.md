---
title: "javascript"
date: 2021-01-08
---

# Historique

Le langage javascript a été créé en 10 jours en mai 1995 par Brendan Eich pour le navigateur Netscape Navigator 2.0. S'en suivi une guerre avec Microsoft qui développa un langage proche (JScript), mais lègérement différent. La syntaxe s'est progressivement enrichie tout en supportant les syntaxes anciennes pour ne pas "casser" les anciennes pages.

Pour imposer un controle plus strict de la syntaxe, il faut ajouter:
`'use strict'` au début du module ou de la fonction.

Cela supprime la déclaration implicite d'une variable lorsqu'on écrit `x = 1`.
Cela supprime l'écriture octale des entiers (0644 déclanche une erreur).
le mot clé `delete` permet d'enlever une clé d'un objet, mais plus de supprimer une variable.

Certaines clé sont impossibles à supprimer (par exemple .prototype)

async function sum(num1, num2) {
return num1 + num2;
}
sum(1, 2).then(alert);

Promises are set up with resolve and reject parameters. Resolve is called withe .then() function and reject is called with the .catch() function.
C'est fait automatiquement avec le mote clé async. Sans ce mot clé, on peut faire:
const p = new Promise((resolve, reject) => {
const value = 5;
if (value === 5) { resolve('Success'); } else { reject('Failure'); } });

async function fetchData(url) {
let response = await fetch(url);
let jsonRespone = await response.json();
console.log (JSON.stringify(jsonResponse));
}

npm i node-fetch --save
const fetch = require('node-fetch');
fetch('http://...').then(function(response) {return response.json();})
.then(function(myJson) { console.log(JSON.stringify(myJson));});

Suite à un post sur [hacker news](https://news.ycombinator.com/item?id=23216642), voici quelques liens concernant le markdown:

[Un tutorial](https://www.markdowntutorial.com/)

[le cheatsheet de duck duck go](https://duckduckgo.com/?t=ffab&q=markdown+cheatsheet&ia=answer&iax=1)

## groupBy Method in javascript
Group objects by any property

```javascript
const people = [ { name: 'Alice', age: 25 }, { name: 'Bob', age: 30 }, { name: 'Charlie', age: 25 } ];
Object.groupBy(people, p => p.age)
```
