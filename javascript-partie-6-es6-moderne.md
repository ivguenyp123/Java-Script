# JavaScript Moderne - Partie 6 : ES6+ et Fonctionnalités Modernes

**🔗 Repository GitHub :** [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)

---

## Table des Matières

1. [Introduction à ES6+](#1-introduction-à-es6)
2. [Let et Const](#2-let-et-const)
3. [Arrow Functions](#3-arrow-functions)
4. [Template Literals](#4-template-literals)
5. [Destructuring](#5-destructuring)
6. [Spread et Rest](#6-spread-et-rest)
7. [Default Parameters](#7-default-parameters)
8. [Enhanced Object Literals](#8-enhanced-object-literals)
9. [Classes](#9-classes)
10. [Modules](#10-modules)
11. [Map et Set](#11-map-et-set)
12. [Symbol](#12-symbol)
13. [Iterators et Generators](#13-iterators-et-generators)
14. [Nouveautés ES2020+](#14-nouveautés-es2020)
15. [Best Practices Modernes](#15-best-practices-modernes)

---

## 1. Introduction à ES6+

### Qu'est-ce qu'ES6 ?

**ES6 = ECMAScript 2015**

ECMAScript est le **standard** qui définit JavaScript. Chaque année depuis 2015, de nouvelles fonctionnalités sont ajoutées.

**Versions importantes :**
- ES5 (2009) : La version "ancienne" encore utilisée
- **ES6/ES2015** : ÉNORME mise à jour (arrow functions, classes, modules...)
- ES2016, ES2017, ES2018, ES2019, ES2020, ES2021, ES2022, ES2023...

**Pourquoi c'est crucial ?**

```javascript
// Code ES5 (ancien, verbeux, complexe)
var self = this;
var numbers = [1, 2, 3, 4, 5];
var doubled = numbers.map(function(n) {
  return n * 2;
});

// Code ES6+ (moderne, concis, élégant)
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
```

**Le code moderne est :**
- ✅ Plus court et lisible
- ✅ Moins de bugs
- ✅ Plus performant
- ✅ Standard dans l'industrie

**Si tu ne connais pas ES6+, tu ne peux pas :**
- Lire du code moderne
- Travailler dans une équipe pro
- Utiliser des frameworks (React, Vue, Angular)
- Passer des entretiens techniques

### Support navigateurs

**Tous les navigateurs modernes supportent ES6+ :**
- Chrome 51+
- Firefox 54+
- Safari 10+
- Edge 14+

**Pour les vieux navigateurs :** On utilise **Babel** (transpile ES6+ → ES5)

Mais aujourd'hui, **écris TOUJOURS en ES6+ moderne.**

---

## 2. Let et Const

### Le problème avec var

```javascript
// var a un SCOPE DE FONCTION (bizarre)
function exemple() {
  if (true) {
    var x = 5;
  }
  console.log(x); // 5 (accessible en dehors du if !)
}

// var est HOISTED (remonté)
console.log(y); // undefined (pas d'erreur !)
var y = 10;

// var peut être REDÉCLARÉ (danger)
var z = 1;
var z = 2; // Pas d'erreur
```

**Ces comportements causent des BUGS.**

### let - Variable de bloc

```javascript
// let a un SCOPE DE BLOC (logique)
function exemple() {
  if (true) {
    let x = 5;
  }
  console.log(x); // ReferenceError: x is not defined
}

// let n'est PAS hoisted utilisable
console.log(y); // ReferenceError
let y = 10;

// let ne peut PAS être redéclaré
let z = 1;
let z = 2; // SyntaxError
```

### const - Constante

```javascript
// const ne peut pas être RÉASSIGNÉ
const PI = 3.14159;
PI = 3; // TypeError

// MAIS les objets/arrays peuvent être MODIFIÉS
const person = { name: 'Alice' };
person.name = 'Bob'; // ✅ OK
person.age = 25;     // ✅ OK
person = {};         // ❌ TypeError

const numbers = [1, 2, 3];
numbers.push(4);     // ✅ OK
numbers = [];        // ❌ TypeError
```

**Pense à const comme "référence constante", pas "valeur constante".**

### Règle d'or moderne

```javascript
// ✅ PAR DÉFAUT : const
const name = 'Alice';
const age = 25;

// ✅ Si tu DOIS réassigner : let
let compteur = 0;
compteur++;

// ❌ JAMAIS : var
// var est obsolète, ne l'utilise plus
```

**Ordre de préférence : const > let > jamais var**

### Exemples pratiques

```javascript
// ❌ ANCIEN (var)
var items = [];
for (var i = 0; i < 5; i++) {
  items.push(function() {
    console.log(i);
  });
}
items[0](); // 5 (pas 0 !)
items[1](); // 5 (pas 1 !)
// i est partagé par toutes les fonctions

// ✅ MODERNE (let)
const items = [];
for (let i = 0; i < 5; i++) {
  items.push(function() {
    console.log(i);
  });
}
items[0](); // 0 ✅
items[1](); // 1 ✅
// Chaque itération a son propre i
```

### Scope de bloc expliqué

```javascript
// Bloc = tout ce qui est entre { }

// if
if (true) {
  const x = 5;
  console.log(x); // 5
}
console.log(x); // ReferenceError

// for
for (let i = 0; i < 3; i++) {
  const doubled = i * 2;
  console.log(doubled);
}
console.log(i); // ReferenceError
console.log(doubled); // ReferenceError

// Bloc simple
{
  const secret = 'hello';
  console.log(secret); // 'hello'
}
console.log(secret); // ReferenceError
```

### Temporal Dead Zone (TDZ)

```javascript
// Zone entre le début du scope et la déclaration
// où la variable existe mais n'est pas accessible

function exemple() {
  // TDZ pour x commence ici
  console.log(x); // ReferenceError
  
  let x = 5; // TDZ pour x se termine ici
  
  console.log(x); // 5
}
```

**Conseil pratique :** Déclare toujours tes variables en HAUT du scope.

---

## 3. Arrow Functions

### Syntaxe de base

**Fonction classique :**
```javascript
function add(a, b) {
  return a + b;
}
```

**Arrow function :**
```javascript
const add = (a, b) => {
  return a + b;
};
```

### Syntaxe courte

**Si une seule expression, return implicite :**
```javascript
// ✅ Très court
const add = (a, b) => a + b;

// ✅ Un seul paramètre : pas besoin de parenthèses
const double = n => n * 2;

// ✅ Pas de paramètre : parenthèses vides
const random = () => Math.random();

// ✅ Retourner un objet : mettre entre parenthèses
const createPerson = (name, age) => ({ name, age });
```

### Exemples de conversion

**Array.map() :**
```javascript
// ❌ ANCIEN
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(function(n) {
  return n * 2;
});

// ✅ MODERNE
const doubled = numbers.map(n => n * 2);
```

**Array.filter() :**
```javascript
// ❌ ANCIEN
const numbers = [1, 2, 3, 4, 5];
const evens = numbers.filter(function(n) {
  return n % 2 === 0;
});

// ✅ MODERNE
const evens = numbers.filter(n => n % 2 === 0);
```

**setTimeout() :**
```javascript
// ❌ ANCIEN
setTimeout(function() {
  console.log('Hello');
}, 1000);

// ✅ MODERNE
setTimeout(() => {
  console.log('Hello');
}, 1000);
```

### La différence avec `this`

**C'est LA différence majeure avec les fonctions classiques.**

**Fonction classique : `this` change selon le contexte**
```javascript
const person = {
  name: 'Alice',
  hobbies: ['lecture', 'sport'],
  
  showHobbies: function() {
    this.hobbies.forEach(function(hobby) {
      // ❌ this est undefined ici !
      console.log(this.name + ' aime ' + hobby);
    });
  }
};

person.showHobbies(); // TypeError: Cannot read property 'name' of undefined
```

**Arrow function : `this` est hérité du contexte parent**
```javascript
const person = {
  name: 'Alice',
  hobbies: ['lecture', 'sport'],
  
  showHobbies: function() {
    this.hobbies.forEach(hobby => {
      // ✅ this pointe vers person
      console.log(this.name + ' aime ' + hobby);
    });
  }
};

person.showHobbies();
// Alice aime lecture
// Alice aime sport
```

**Solution ES5 (ancienne) :**
```javascript
showHobbies: function() {
  var self = this; // Sauvegarder this
  this.hobbies.forEach(function(hobby) {
    console.log(self.name + ' aime ' + hobby);
  });
}
```

### Quand NE PAS utiliser les arrow functions

**1. Méthodes d'objets qui utilisent `this`**
```javascript
// ❌ MAUVAIS
const person = {
  name: 'Alice',
  greet: () => {
    console.log('Hello ' + this.name); // this n'est pas person
  }
};

// ✅ BON
const person = {
  name: 'Alice',
  greet() { // Syntaxe courte ES6
    console.log('Hello ' + this.name);
  }
};
```

**2. Event handlers DOM qui utilisent `this`**
```javascript
// ❌ MAUVAIS
button.addEventListener('click', () => {
  this.classList.add('active'); // this n'est pas button
});

// ✅ BON
button.addEventListener('click', function() {
  this.classList.add('active');
});
```

**3. Constructeurs**
```javascript
// ❌ IMPOSSIBLE
const Person = (name) => {
  this.name = name;
};
const p = new Person('Alice'); // TypeError
```

### Exemples pratiques

**Callback hell → Arrow functions**
```javascript
// ❌ ANCIEN (illisible)
fetch(url)
  .then(function(response) {
    return response.json();
  })
  .then(function(data) {
    return processData(data);
  })
  .then(function(result) {
    console.log(result);
  });

// ✅ MODERNE (élégant)
fetch(url)
  .then(response => response.json())
  .then(data => processData(data))
  .then(result => console.log(result));
```

**Fonctions de tri**
```javascript
const users = [
  { name: 'Bob', age: 30 },
  { name: 'Alice', age: 25 },
  { name: 'Charlie', age: 35 }
];

// ❌ ANCIEN
users.sort(function(a, b) {
  return a.age - b.age;
});

// ✅ MODERNE
users.sort((a, b) => a.age - b.age);
```

---

## 4. Template Literals

### Syntaxe de base

**Ancien (concaténation) :**
```javascript
const name = 'Alice';
const age = 25;
const message = 'Bonjour ' + name + ', tu as ' + age + ' ans.';
```

**Moderne (template literals avec backticks) :**
```javascript
const name = 'Alice';
const age = 25;
const message = `Bonjour ${name}, tu as ${age} ans.`;
```

**Utilise `` ` `` (backtick) au lieu de `'` ou `"`**

### Expressions dans les templates

```javascript
const a = 10;
const b = 20;

console.log(`La somme est ${a + b}`); // La somme est 30
console.log(`Le double de ${a} est ${a * 2}`); // Le double de 10 est 20

// Fonctions
const price = 99.99;
console.log(`Prix TTC: ${(price * 1.20).toFixed(2)}€`);

// Ternaire
const age = 17;
console.log(`Tu es ${age >= 18 ? 'majeur' : 'mineur'}`);
```

### Multi-lignes

**Ancien (horrible) :**
```javascript
const html = '<div>\n' +
  '  <h1>Titre</h1>\n' +
  '  <p>Paragraphe</p>\n' +
  '</div>';
```

**Moderne (naturel) :**
```javascript
const html = `
  <div>
    <h1>Titre</h1>
    <p>Paragraphe</p>
  </div>
`;
```

**Préserve les retours à la ligne et l'indentation !**

### Cas d'usage pratiques

**1. HTML dynamique**
```javascript
function createUserCard(user) {
  return `
    <div class="user-card">
      <img src="${user.avatar}" alt="${user.name}">
      <h2>${user.name}</h2>
      <p>${user.email}</p>
      <span class="age">${user.age} ans</span>
    </div>
  `;
}

const user = {
  name: 'Alice',
  email: 'alice@example.com',
  age: 25,
  avatar: 'avatar.jpg'
};

document.body.innerHTML = createUserCard(user);
```

**2. Messages de log**
```javascript
const user = { id: 123, name: 'Alice' };
const action = 'login';

// ❌ ANCIEN
console.log('User ' + user.id + ' (' + user.name + ') performed action: ' + action);

// ✅ MODERNE
console.log(`User ${user.id} (${user.name}) performed action: ${action}`);
```

**3. URLs avec paramètres**
```javascript
const API_URL = 'https://api.example.com';
const userId = 123;
const limit = 10;

// ❌ ANCIEN
const url = API_URL + '/users/' + userId + '/posts?limit=' + limit;

// ✅ MODERNE
const url = `${API_URL}/users/${userId}/posts?limit=${limit}`;
```

**4. SQL Queries (simulation)**
```javascript
const userId = 42;
const status = 'active';

const query = `
  SELECT *
  FROM users
  WHERE id = ${userId}
    AND status = '${status}'
  ORDER BY created_at DESC
`;
```

**⚠️ Attention SQL Injection ! En production, utilise des prepared statements.**

### Tagged Templates (avancé)

Tu peux créer des fonctions qui traitent les template literals.

```javascript
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) => {
    const value = values[i] || '';
    return result + str + `<strong>${value}</strong>`;
  }, '');
}

const name = 'Alice';
const age = 25;

const message = highlight`Bonjour ${name}, tu as ${age} ans.`;
// "Bonjour <strong>Alice</strong>, tu as <strong>25</strong> ans."
```

**Cas d'usage :** Styled Components en React, i18n, sanitization...

---

## 5. Destructuring

### Destructuring d'objets

**Extraire des propriétés d'un objet :**

```javascript
const user = {
  name: 'Alice',
  age: 25,
  email: 'alice@example.com'
};

// ❌ ANCIEN
const name = user.name;
const age = user.age;
const email = user.email;

// ✅ MODERNE
const { name, age, email } = user;

console.log(name);  // 'Alice'
console.log(age);   // 25
console.log(email); // 'alice@example.com'
```

**Renommer les variables :**
```javascript
const user = { name: 'Alice', age: 25 };

// Extraire 'name' dans une variable 'userName'
const { name: userName, age: userAge } = user;

console.log(userName); // 'Alice'
console.log(userAge);  // 25
// console.log(name); // ReferenceError
```

**Valeurs par défaut :**
```javascript
const user = { name: 'Alice' };

const { name, age = 18, country = 'France' } = user;

console.log(name);    // 'Alice'
console.log(age);     // 18 (valeur par défaut)
console.log(country); // 'France' (valeur par défaut)
```

**Destructuring imbriqué :**
```javascript
const user = {
  name: 'Alice',
  address: {
    city: 'Paris',
    country: 'France'
  }
};

const { name, address: { city, country } } = user;

console.log(name);    // 'Alice'
console.log(city);    // 'Paris'
console.log(country); // 'France'
// console.log(address); // ReferenceError (pas extrait)
```

### Destructuring de tableaux

**Extraire des éléments d'un tableau :**

```javascript
const numbers = [1, 2, 3, 4, 5];

// ❌ ANCIEN
const first = numbers[0];
const second = numbers[1];

// ✅ MODERNE
const [first, second] = numbers;

console.log(first);  // 1
console.log(second); // 2
```

**Sauter des éléments :**
```javascript
const numbers = [1, 2, 3, 4, 5];

const [first, , third, , fifth] = numbers;

console.log(first); // 1
console.log(third); // 3
console.log(fifth); // 5
```

**Rest dans les arrays :**
```javascript
const numbers = [1, 2, 3, 4, 5];

const [first, second, ...rest] = numbers;

console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]
```

**Swap de variables :**
```javascript
let a = 1;
let b = 2;

// ❌ ANCIEN (avec variable temporaire)
let temp = a;
a = b;
b = temp;

// ✅ MODERNE (élégant)
[a, b] = [b, a];

console.log(a); // 2
console.log(b); // 1
```

### Destructuring dans les paramètres de fonction

**Objets :**
```javascript
// ❌ ANCIEN
function createUser(options) {
  const name = options.name;
  const age = options.age || 18;
  const role = options.role || 'user';
  // ...
}

// ✅ MODERNE
function createUser({ name, age = 18, role = 'user' }) {
  console.log(name, age, role);
}

createUser({ name: 'Alice', age: 25 });
// Alice 25 user
```

**Arrays :**
```javascript
function displayCoordinates([x, y]) {
  console.log(`X: ${x}, Y: ${y}`);
}

displayCoordinates([10, 20]); // X: 10, Y: 20
```

### Cas d'usage pratiques

**1. APIs REST**
```javascript
async function fetchUser(userId) {
  const response = await fetch(`/api/users/${userId}`);
  const { id, name, email, avatar } = await response.json();
  
  return { id, name, email, avatar };
  // Extraire seulement ce qu'on veut
}
```

**2. React props**
```javascript
// ❌ ANCIEN
function UserCard(props) {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>{props.email}</p>
    </div>
  );
}

// ✅ MODERNE
function UserCard({ name, email }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{email}</p>
    </div>
  );
}
```

**3. Retours multiples de fonctions**
```javascript
function getMinMax(numbers) {
  return {
    min: Math.min(...numbers),
    max: Math.max(...numbers)
  };
}

const { min, max } = getMinMax([1, 5, 3, 9, 2]);
console.log(min, max); // 1 9
```

**4. Import de modules**
```javascript
// Destructuring avec import
import { useState, useEffect } from 'react';
import { map, filter, reduce } from 'lodash';
```

---

## 6. Spread et Rest

### Spread Operator (...)

**"Déployer" un tableau ou objet**

**Avec les arrays :**
```javascript
const numbers = [1, 2, 3];
const moreNumbers = [4, 5, 6];

// ❌ ANCIEN (concat)
const all = numbers.concat(moreNumbers);

// ✅ MODERNE (spread)
const all = [...numbers, ...moreNumbers];
// [1, 2, 3, 4, 5, 6]

// Ajouter des éléments
const extended = [0, ...numbers, 4];
// [0, 1, 2, 3, 4]
```

**Copier un array :**
```javascript
const original = [1, 2, 3];

// ❌ FAUX (référence, pas copie)
const copy1 = original;
copy1.push(4);
console.log(original); // [1, 2, 3, 4] ← modifié !

// ✅ BON (vraie copie)
const copy2 = [...original];
copy2.push(4);
console.log(original); // [1, 2, 3] ← intact
```

**Passer des arguments :**
```javascript
const numbers = [5, 2, 8, 1, 9];

// ❌ ANCIEN
Math.max.apply(null, numbers);

// ✅ MODERNE
Math.max(...numbers); // 9
```

**Avec les objets :**
```javascript
const person = {
  name: 'Alice',
  age: 25
};

const address = {
  city: 'Paris',
  country: 'France'
};

// ❌ ANCIEN (Object.assign)
const user = Object.assign({}, person, address);

// ✅ MODERNE (spread)
const user = { ...person, ...address };
// { name: 'Alice', age: 25, city: 'Paris', country: 'France' }
```

**Copier et modifier :**
```javascript
const person = {
  name: 'Alice',
  age: 25,
  city: 'Paris'
};

// Copier et changer age
const updated = { ...person, age: 26 };
// { name: 'Alice', age: 26, city: 'Paris' }

// L'ordre compte !
const test1 = { ...person, age: 26 };     // age: 26
const test2 = { age: 26, ...person };     // age: 25 (écrasé)
```

### Rest Operator (...)

**"Collecter" le reste des éléments**

**Dans les paramètres de fonction :**
```javascript
// ❌ ANCIEN (arguments)
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

// ✅ MODERNE (rest)
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3, 4, 5); // 15
```

**Mélanger paramètres normaux et rest :**
```javascript
function greet(greeting, ...names) {
  return `${greeting} ${names.join(', ')}`;
}

greet('Bonjour', 'Alice', 'Bob', 'Charlie');
// "Bonjour Alice, Bob, Charlie"
```

**⚠️ Rest doit être le DERNIER paramètre**
```javascript
// ✅ OK
function example(a, b, ...rest) {}

// ❌ ERREUR
function example(a, ...rest, b) {} // SyntaxError
```

**Dans le destructuring :**
```javascript
// Arrays
const [first, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(rest);  // [2, 3, 4, 5]

// Objects
const person = {
  name: 'Alice',
  age: 25,
  city: 'Paris',
  country: 'France'
};

const { name, ...details } = person;
console.log(name);    // 'Alice'
console.log(details); // { age: 25, city: 'Paris', country: 'France' }
```

### Cas d'usage pratiques

**1. Immutabilité (React/Redux)**
```javascript
// Ajouter un élément sans muter
const todos = ['Tâche 1', 'Tâche 2'];
const newTodos = [...todos, 'Tâche 3'];

// Supprimer un élément sans muter
const todos = ['Tâche 1', 'Tâche 2', 'Tâche 3'];
const index = 1;
const filtered = [
  ...todos.slice(0, index),
  ...todos.slice(index + 1)
];
// ['Tâche 1', 'Tâche 3']
```

**2. Fusionner des configurations**
```javascript
const defaultConfig = {
  host: 'localhost',
  port: 3000,
  timeout: 5000
};

const userConfig = {
  port: 8080
};

const config = { ...defaultConfig, ...userConfig };
// { host: 'localhost', port: 8080, timeout: 5000 }
```

**3. Passer des props en React**
```javascript
const user = {
  name: 'Alice',
  email: 'alice@example.com',
  avatar: 'avatar.jpg'
};

// Au lieu de :
<UserCard name={user.name} email={user.email} avatar={user.avatar} />

// Utilise spread :
<UserCard {...user} />
```

**4. Fonctions avec nombre variable d'arguments**
```javascript
function multiply(multiplier, ...numbers) {
  return numbers.map(n => n * multiplier);
}

multiply(2, 1, 2, 3, 4);    // [2, 4, 6, 8]
multiply(10, 5, 10, 15);    // [50, 100, 150]
```

---

## 7. Default Parameters

### Valeurs par défaut des paramètres

**Ancien (vérifications manuelles) :**
```javascript
function greet(name, greeting) {
  name = name || 'Invité';
  greeting = greeting || 'Bonjour';
  return greeting + ' ' + name;
}

// Problème avec les valeurs falsy
greet('', 'Salut'); // "Salut Invité" (mauvais !)
greet(0, 'Hey');    // "Hey Invité" (mauvais !)
```

**Moderne (default parameters) :**
```javascript
function greet(name = 'Invité', greeting = 'Bonjour') {
  return `${greeting} ${name}`;
}

greet();                    // "Bonjour Invité"
greet('Alice');             // "Bonjour Alice"
greet('Alice', 'Salut');    // "Salut Alice"

// undefined déclenche le défaut
greet(undefined, 'Hey');    // "Hey Invité"

// Mais pas null ou autres valeurs falsy
greet('', 'Salut');         // "Salut " (vide accepté)
greet(0, 'Hey');            // "Hey 0" (0 accepté)
```

### Expressions comme valeurs par défaut

```javascript
// Fonction
function getDefaultName() {
  return 'Invité';
}

function greet(name = getDefaultName()) {
  return `Bonjour ${name}`;
}

// Autre paramètre
function createUser(name, id = name.toLowerCase()) {
  return { name, id };
}

createUser('Alice'); // { name: 'Alice', id: 'alice' }

// Calcul
function calculatePrice(price, tax = price * 0.20) {
  return price + tax;
}

calculatePrice(100);     // 120
calculatePrice(100, 10); // 110
```

### Avec destructuring

```javascript
function createUser({
  name = 'Anonyme',
  age = 18,
  role = 'user'
} = {}) {
  return { name, age, role };
}

createUser();
// { name: 'Anonyme', age: 18, role: 'user' }

createUser({ name: 'Alice', age: 25 });
// { name: 'Alice', age: 25, role: 'user' }

// Le = {} à la fin permet d'appeler sans arguments
// Sans ça, createUser() → erreur
```

### Cas d'usage pratiques

**1. Configuration d'API**
```javascript
async function fetchData({
  url,
  method = 'GET',
  headers = {},
  timeout = 5000
}) {
  const controller = new AbortController();
  const timeoutId = setTimeout(() => controller.abort(), timeout);
  
  try {
    const response = await fetch(url, {
      method,
      headers: {
        'Content-Type': 'application/json',
        ...headers
      },
      signal: controller.signal
    });
    
    return await response.json();
  } finally {
    clearTimeout(timeoutId);
  }
}

// Utilisation simple
fetchData({ url: '/api/users' });

// Avec options
fetchData({
  url: '/api/users',
  method: 'POST',
  headers: { 'Authorization': 'Bearer token' },
  timeout: 10000
});
```

**2. Création d'éléments DOM**
```javascript
function createElement({
  tag = 'div',
  className = '',
  textContent = '',
  attributes = {}
} = {}) {
  const element = document.createElement(tag);
  
  if (className) {
    element.className = className;
  }
  
  if (textContent) {
    element.textContent = textContent;
  }
  
  Object.entries(attributes).forEach(([key, value]) => {
    element.setAttribute(key, value);
  });
  
  return element;
}

// Utilisation
const button = createElement({
  tag: 'button',
  className: 'btn btn-primary',
  textContent: 'Cliquez-moi',
  attributes: { 'data-id': '123' }
});
```

---

## 8. Enhanced Object Literals

### Propriétés raccourcies

**Ancien :**
```javascript
const name = 'Alice';
const age = 25;

const person = {
  name: name,
  age: age
};
```

**Moderne :**
```javascript
const name = 'Alice';
const age = 25;

const person = {
  name,  // Équivalent à name: name
  age    // Équivalent à age: age
};
```

**Très utile avec les retours de fonctions :**
```javascript
function createUser(name, email) {
  const id = Date.now();
  const createdAt = new Date();
  
  return { id, name, email, createdAt };
}
```

### Méthodes raccourcies

**Ancien :**
```javascript
const person = {
  name: 'Alice',
  greet: function() {
    return 'Bonjour ' + this.name;
  }
};
```

**Moderne :**
```javascript
const person = {
  name: 'Alice',
  greet() {
    return `Bonjour ${this.name}`;
  }
};
```

### Propriétés calculées

**Créer des propriétés dynamiquement :**

```javascript
const field = 'email';
const value = 'alice@example.com';

// ❌ ANCIEN
const user = {};
user[field] = value;

// ✅ MODERNE
const user = {
  [field]: value
};
// { email: 'alice@example.com' }

// Avec expressions
const prefix = 'user';
const id = 123;

const obj = {
  [prefix + '_' + id]: 'Alice',
  [prefix.toUpperCase()]: 'data'
};
// { user_123: 'Alice', USER: 'data' }
```

**Cas d'usage : Créer des objets dynamiques**
```javascript
function createResponse(status, data) {
  return {
    [`${status}Response`]: data,
    timestamp: Date.now()
  };
}

createResponse('success', { userId: 123 });
// { successResponse: { userId: 123 }, timestamp: 1234567890 }

createResponse('error', { message: 'Not found' });
// { errorResponse: { message: 'Not found' }, timestamp: 1234567891 }
```

### Tout combiné

```javascript
const id = 1;
const name = 'Alice';
const email = 'alice@example.com';
const status = 'active';

const user = {
  id,
  name,
  email,
  
  [`is${status.charAt(0).toUpperCase() + status.slice(1)}`]: true,
  // isActive: true
  
  getInfo() {
    return `${this.name} (${this.email})`;
  },
  
  updateEmail(newEmail) {
    this.email = newEmail;
  }
};
```

---

## 9. Classes

### Syntaxe de base

**Ancien (fonctions constructeurs) :**
```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  return 'Bonjour ' + this.name;
};

const alice = new Person('Alice', 25);
```

**Moderne (classes) :**
```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    return `Bonjour ${this.name}`;
  }
}

const alice = new Person('Alice', 25);
alice.greet(); // "Bonjour Alice"
```

### Propriétés et méthodes

```javascript
class User {
  // Propriétés de classe (ES2022+)
  role = 'user';
  active = true;
  
  constructor(name, email) {
    this.name = name;
    this.email = email;
    this.createdAt = new Date();
  }
  
  // Méthode d'instance
  getInfo() {
    return `${this.name} (${this.email})`;
  }
  
  // Méthode statique (appelée sur la classe, pas l'instance)
  static createAdmin(name, email) {
    const user = new User(name, email);
    user.role = 'admin';
    return user;
  }
}

// Utilisation
const user = new User('Alice', 'alice@example.com');
console.log(user.getInfo());

const admin = User.createAdmin('Bob', 'bob@example.com');
console.log(admin.role); // 'admin'
```

### Getters et Setters

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  
  // Getter : propriété calculée
  get area() {
    return this.width * this.height;
  }
  
  // Setter : validation
  set width(value) {
    if (value <= 0) {
      throw new Error('Width must be positive');
    }
    this._width = value;
  }
  
  get width() {
    return this._width;
  }
}

const rect = new Rectangle(10, 5);
console.log(rect.area); // 50 (pas de parenthèses !)

rect.width = 20;
console.log(rect.area); // 100

rect.width = -5; // Error: Width must be positive
```

### Héritage (extends)

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  makeSound() {
    return 'Some sound';
  }
  
  introduce() {
    return `Je suis ${this.name}`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Appeler le constructor parent
    this.breed = breed;
  }
  
  // Override (surcharge)
  makeSound() {
    return 'Woof!';
  }
  
  // Nouvelle méthode
  fetch() {
    return `${this.name} va chercher la balle`;
  }
}

const dog = new Dog('Rex', 'Labrador');
console.log(dog.name);         // 'Rex'
console.log(dog.breed);        // 'Labrador'
console.log(dog.makeSound());  // 'Woof!'
console.log(dog.introduce());  // 'Je suis Rex' (hérité)
console.log(dog.fetch());      // 'Rex va chercher la balle'
```

### Propriétés privées (ES2022+)

```javascript
class BankAccount {
  #balance = 0; // Privé (commence par #)
  
  constructor(owner) {
    this.owner = owner;
  }
  
  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
    }
  }
  
  withdraw(amount) {
    if (amount > 0 && amount <= this.#balance) {
      this.#balance -= amount;
      return amount;
    }
    return 0;
  }
  
  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount('Alice');
account.deposit(1000);
console.log(account.getBalance()); // 1000

// ❌ IMPOSSIBLE d'accéder directement
console.log(account.#balance); // SyntaxError
```

### Cas d'usage pratiques

**1. Modèle de données**
```javascript
class User {
  constructor(data) {
    this.id = data.id;
    this.name = data.name;
    this.email = data.email;
    this.createdAt = new Date(data.createdAt);
  }
  
  static async fetchById(id) {
    const response = await fetch(`/api/users/${id}`);
    const data = await response.json();
    return new User(data);
  }
  
  async save() {
    const response = await fetch(`/api/users/${this.id}`, {
      method: 'PUT',
      body: JSON.stringify(this)
    });
    return response.ok;
  }
  
  get age() {
    const now = new Date();
    const birthYear = this.createdAt.getFullYear();
    return now.getFullYear() - birthYear;
  }
}

// Utilisation
const user = await User.fetchById(123);
user.name = 'Alice Dupont';
await user.save();
```

**2. Composant réutilisable**
```javascript
class Modal {
  constructor(title, content) {
    this.title = title;
    this.content = content;
    this.element = this.#create();
  }
  
  #create() {
    const modal = document.createElement('div');
    modal.className = 'modal';
    modal.innerHTML = `
      <div class="modal-content">
        <h2>${this.title}</h2>
        <p>${this.content}</p>
        <button class="close">Fermer</button>
      </div>
    `;
    
    modal.querySelector('.close').addEventListener('click', () => {
      this.close();
    });
    
    return modal;
  }
  
  show() {
    document.body.appendChild(this.element);
    this.element.classList.add('visible');
  }
  
  close() {
    this.element.classList.remove('visible');
    setTimeout(() => this.element.remove(), 300);
  }
}

// Utilisation
const modal = new Modal('Confirmation', 'Êtes-vous sûr ?');
modal.show();
```

---

## 10. Modules

### Pourquoi les modules ?

**Problème sans modules :**
```html
<script src="utils.js"></script>
<script src="api.js"></script>
<script src="app.js"></script>

<!-- Tous les fichiers partagent le scope global -->
<!-- Risque de conflits de noms -->
<!-- Pas de contrôle des dépendances -->
```

**Solution avec modules :**
- ✅ Chaque fichier a son propre scope
- ✅ Import/export explicites
- ✅ Dépendances claires
- ✅ Code réutilisable

### Export

**Named exports (plusieurs par fichier) :**
```javascript
// utils.js

// Export inline
export const PI = 3.14159;

export function add(a, b) {
  return a + b;
}

export class Calculator {
  // ...
}

// Ou export en fin de fichier
const API_URL = 'https://api.example.com';

function fetchData(endpoint) {
  // ...
}

export { API_URL, fetchData };
```

**Default export (un seul par fichier) :**
```javascript
// user.js

export default class User {
  constructor(name) {
    this.name = name;
  }
}

// Ou avec fonction
export default function createUser(name) {
  return { name, id: Date.now() };
}

// Ou avec objet
export default {
  apiUrl: 'https://api.example.com',
  timeout: 5000
};
```

### Import

**Named imports :**
```javascript
// Importer des exports nommés
import { add, PI } from './utils.js';

console.log(add(5, 3)); // 8
console.log(PI);        // 3.14159

// Renommer à l'import
import { add as sum, PI as pi } from './utils.js';

// Importer tout
import * as utils from './utils.js';
console.log(utils.add(5, 3));
console.log(utils.PI);
```

**Default import :**
```javascript
// Le nom est libre (pas de {})
import User from './user.js';
import MyUser from './user.js'; // Même chose

const user = new User('Alice');
```

**Mélanger default et named :**
```javascript
// api.js
export const API_URL = 'https://api.example.com';
export default class API {
  // ...
}

// app.js
import API, { API_URL } from './api.js';
```

### Exemple de structure de projet

```
project/
├── index.html
├── src/
│   ├── main.js
│   ├── utils/
│   │   ├── math.js
│   │   └── string.js
│   ├── api/
│   │   ├── client.js
│   │   └── endpoints.js
│   └── components/
│       ├── Header.js
│       └── Footer.js
```

**math.js :**
```javascript
export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

export const PI = 3.14159;
```

**string.js :**
```javascript
export function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

export function slugify(str) {
  return str.toLowerCase().replace(/\s+/g, '-');
}
```

**client.js :**
```javascript
export default class APIClient {
  constructor(baseURL) {
    this.baseURL = baseURL;
  }
  
  async get(endpoint) {
    const response = await fetch(this.baseURL + endpoint);
    return response.json();
  }
  
  async post(endpoint, data) {
    const response = await fetch(this.baseURL + endpoint, {
      method: 'POST',
      body: JSON.stringify(data)
    });
    return response.json();
  }
}
```

**main.js :**
```javascript
import { add, multiply, PI } from './utils/math.js';
import { capitalize, slugify } from './utils/string.js';
import APIClient from './api/client.js';

const result = add(5, multiply(2, PI));
console.log(result);

const title = capitalize('hello world');
const slug = slugify('Hello World');
console.log(title, slug);

const api = new APIClient('https://api.example.com');
const users = await api.get('/users');
```

**index.html :**
```html
<!DOCTYPE html>
<html>
<head>
  <title>App</title>
</head>
<body>
  <!-- Attention : type="module" obligatoire -->
  <script type="module" src="src/main.js"></script>
</body>
</html>
```

### Dynamic imports (chargement à la demande)

```javascript
// Charger un module conditionnellement
async function loadModule() {
  if (condition) {
    const { add } = await import('./math.js');
    console.log(add(5, 3));
  }
}

// Lazy loading (performance)
button.addEventListener('click', async () => {
  const { Modal } = await import('./components/Modal.js');
  const modal = new Modal('Titre', 'Contenu');
  modal.show();
});
```

### Re-export

**Créer un "barrel" (point d'entrée unique) :**

```javascript
// utils/index.js
export { add, multiply, PI } from './math.js';
export { capitalize, slugify } from './string.js';
export { default as APIClient } from '../api/client.js';

// Maintenant on peut importer depuis un seul fichier
import { add, capitalize, APIClient } from './utils/index.js';
```

---

## 11. Map et Set

### Map - Dictionnaire clé-valeur

**Map vs Object :**
```javascript
// Object : clés limitées aux strings/symbols
const obj = {};
obj['key'] = 'value';
obj[123] = 'number key'; // Converti en string "123"

// Map : n'importe quelle valeur comme clé
const map = new Map();
map.set('key', 'value');
map.set(123, 'number key');
map.set({}, 'object key');
map.set(true, 'boolean key');
```

### Opérations de base

```javascript
const users = new Map();

// set() - Ajouter/modifier
users.set('alice', { name: 'Alice', age: 25 });
users.set('bob', { name: 'Bob', age: 30 });

// get() - Récupérer
const alice = users.get('alice');
console.log(alice); // { name: 'Alice', age: 25 }

// has() - Vérifier existence
console.log(users.has('alice')); // true
console.log(users.has('charlie')); // false

// delete() - Supprimer
users.delete('bob');

// size - Taille
console.log(users.size); // 1

// clear() - Tout supprimer
users.clear();
```

### Itération

```javascript
const map = new Map([
  ['name', 'Alice'],
  ['age', 25],
  ['city', 'Paris']
]);

// forEach
map.forEach((value, key) => {
  console.log(`${key}: ${value}`);
});

// for...of sur les entrées
for (const [key, value] of map) {
  console.log(`${key}: ${value}`);
}

// Juste les clés
for (const key of map.keys()) {
  console.log(key);
}

// Juste les valeurs
for (const value of map.values()) {
  console.log(value);
}

// Entrées (paires key-value)
for (const [key, value] of map.entries()) {
  console.log(key, value);
}
```

### Conversion Map ↔ Object/Array

```javascript
// Object → Map
const obj = { a: 1, b: 2, c: 3 };
const map = new Map(Object.entries(obj));

// Map → Object
const mapToObj = Object.fromEntries(map);

// Map → Array
const mapToArray = Array.from(map);
// [['a', 1], ['b', 2], ['c', 3]]
```

### Cas d'usage : Cache

```javascript
class Cache {
  constructor(maxSize = 100) {
    this.cache = new Map();
    this.maxSize = maxSize;
  }
  
  get(key) {
    return this.cache.get(key);
  }
  
  set(key, value) {
    // Si cache plein, supprimer le plus ancien
    if (this.cache.size >= this.maxSize) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    
    this.cache.set(key, value);
  }
  
  has(key) {
    return this.cache.has(key);
  }
  
  clear() {
    this.cache.clear();
  }
}

const cache = new Cache(3);
cache.set('user1', { name: 'Alice' });
cache.set('user2', { name: 'Bob' });
cache.set('user3', { name: 'Charlie' });
cache.set('user4', { name: 'David' }); // user1 supprimé
```

### Set - Collection de valeurs uniques

```javascript
// Créer un Set
const numbers = new Set([1, 2, 3, 4, 5]);

// Ajouter
numbers.add(6);
numbers.add(3); // Ignoré (déjà présent)

// Supprimer
numbers.delete(2);

// Vérifier
console.log(numbers.has(3)); // true
console.log(numbers.has(10)); // false

// Taille
console.log(numbers.size); // 5

// Itération
for (const num of numbers) {
  console.log(num);
}

// Clear
numbers.clear();
```

### Cas d'usage : Supprimer les doublons

```javascript
// Array avec doublons
const numbers = [1, 2, 2, 3, 4, 4, 5, 5, 5];

// ❌ ANCIEN (complexe)
const unique = numbers.filter((n, i) => numbers.indexOf(n) === i);

// ✅ MODERNE (Set)
const unique = [...new Set(numbers)];
// [1, 2, 3, 4, 5]

// Avec des objets (attention, par référence)
const users = [
  { id: 1, name: 'Alice' },
  { id: 2, name: 'Bob' },
  { id: 1, name: 'Alice' } // Même id
];

// Supprimer par id
const seen = new Set();
const uniqueUsers = users.filter(user => {
  if (seen.has(user.id)) {
    return false;
  }
  seen.add(user.id);
  return true;
});
```

### WeakMap et WeakSet (avancé)

**WeakMap :** Clés doivent être des objets, garbage collected automatiquement
```javascript
const weakmap = new WeakMap();
let obj = { id: 1 };

weakmap.set(obj, 'metadata');
console.log(weakmap.get(obj)); // 'metadata'

obj = null; // L'objet peut être garbage collected
```

**WeakSet :** Pareil mais pour Set
```javascript
const weakset = new WeakSet();
let obj = { id: 1 };

weakset.add(obj);
console.log(weakset.has(obj)); // true

obj = null; // Garbage collected
```

**Cas d'usage :** Métadonnées privées, cache, event listeners...

---

## 12. Symbol

### Qu'est-ce qu'un Symbol ?

Un **Symbol** est une valeur primitive **unique et immuable**.

```javascript
const sym1 = Symbol();
const sym2 = Symbol();

console.log(sym1 === sym2); // false (toujours unique)

// Avec description (pour debug)
const sym3 = Symbol('mySymbol');
console.log(sym3.toString()); // "Symbol(mySymbol)"
```

### Propriétés d'objets avec Symbol

```javascript
const EMAIL = Symbol('email');
const user = {
  name: 'Alice',
  age: 25,
  [EMAIL]: 'alice@example.com' // Propriété Symbol
};

console.log(user.name); // 'Alice'
console.log(user[EMAIL]); // 'alice@example.com'
console.log(user.email); // undefined

// Les Symbols ne sont PAS énumérables
Object.keys(user); // ['name', 'age'] (pas EMAIL)
for (const key in user) {
  console.log(key); // 'name', 'age' (pas EMAIL)
}

// Pour obtenir les Symbols
Object.getOwnPropertySymbols(user); // [Symbol(email)]
```

### Cas d'usage : Propriétés "privées"

```javascript
const _balance = Symbol('balance');

class BankAccount {
  constructor(initialBalance) {
    this[_balance] = initialBalance;
  }
  
  getBalance() {
    return this[_balance];
  }
  
  deposit(amount) {
    this[_balance] += amount;
  }
}

const account = new BankAccount(1000);
console.log(account.getBalance()); // 1000

// "Caché" (mais pas vraiment privé)
console.log(account[_balance]); // undefined si tu n'as pas accès au Symbol
```

### Well-known Symbols

JavaScript a des Symbols prédéfinis pour personnaliser le comportement.

**Symbol.iterator :**
```javascript
const range = {
  from: 1,
  to: 5,
  
  [Symbol.iterator]() {
    let current = this.from;
    const last = this.to;
    
    return {
      next() {
        if (current <= last) {
          return { value: current++, done: false };
        } else {
          return { done: true };
        }
      }
    };
  }
};

// Maintenant on peut boucler dessus
for (const num of range) {
  console.log(num); // 1, 2, 3, 4, 5
}

// Spread fonctionne aussi
const numbers = [...range]; // [1, 2, 3, 4, 5]
```

**Symbol.toStringTag :**
```javascript
class User {
  get [Symbol.toStringTag]() {
    return 'User';
  }
}

const user = new User();
console.log(Object.prototype.toString.call(user));
// "[object User]" (au lieu de "[object Object]")
```

---

## 13. Iterators et Generators

### Iterators

Un **iterator** est un objet qui sait parcourir une collection.

```javascript
const array = [1, 2, 3];
const iterator = array[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

**Créer un iterator manuel :**
```javascript
function createRangeIterator(start, end) {
  let current = start;
  
  return {
    next() {
      if (current <= end) {
        return { value: current++, done: false };
      } else {
        return { done: true };
      }
    }
  };
}

const iterator = createRangeIterator(1, 3);
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { done: true }
```

### Generators

**Generator** = fonction qui peut être mise en pause et reprise.

```javascript
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numberGenerator();

console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

**Syntaxe :** `function*` (avec astérisque) et `yield` (au lieu de return).

### Generator avec boucle

```javascript
function* range(start, end) {
  for (let i = start; i <= end; i++) {
    yield i;
  }
}

// Utilisation
for (const num of range(1, 5)) {
  console.log(num); // 1, 2, 3, 4, 5
}

// Spread
const numbers = [...range(1, 5)]; // [1, 2, 3, 4, 5]
```

### Generator infini

```javascript
function* fibonacci() {
  let a = 0, b = 1;
  
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

const fib = fibonacci();
console.log(fib.next().value); // 0
console.log(fib.next().value); // 1
console.log(fib.next().value); // 1
console.log(fib.next().value); // 2
console.log(fib.next().value); // 3
console.log(fib.next().value); // 5
```

### Passer des valeurs avec next()

```javascript
function* dialogue() {
  const name = yield 'Quel est ton nom ?';
  const age = yield 'Quel âge as-tu ?';
  return `Bonjour ${name}, tu as ${age} ans`;
}

const conv = dialogue();

console.log(conv.next().value);        // "Quel est ton nom ?"
console.log(conv.next('Alice').value); // "Quel âge as-tu ?"
console.log(conv.next(25).value);      // "Bonjour Alice, tu as 25 ans"
```

### Cas d'usage : Pagination API

```javascript
async function* fetchPages(url) {
  let page = 1;
  let hasMore = true;
  
  while (hasMore) {
    const response = await fetch(`${url}?page=${page}`);
    const data = await response.json();
    
    if (data.results.length === 0) {
      hasMore = false;
    } else {
      yield data.results;
      page++;
    }
  }
}

// Utilisation
const pages = fetchPages('/api/users');

for await (const users of pages) {
  console.log('Page:', users);
  // Traiter les users
}
```

---

## 14. Nouveautés ES2020+

### Optional Chaining (?.) - ES2020

**Accéder à des propriétés potentiellement undefined/null :**

```javascript
const user = {
  name: 'Alice',
  address: {
    city: 'Paris'
  }
};

// ❌ ANCIEN (verbeux)
const city = user && user.address && user.address.city;

// ✅ MODERNE (optional chaining)
const city = user?.address?.city;

// Si user.address n'existe pas
const country = user?.address?.country; // undefined (pas d'erreur)

// Avec fonctions
const result = obj.method?.();

// Avec arrays
const first = array?.[0];
```

**Exemple pratique :**
```javascript
async function fetchUser(id) {
  const response = await fetch(`/api/users/${id}`);
  const data = await response.json();
  
  // Accès sécurisé
  const city = data?.user?.address?.city ?? 'Inconnu';
  const email = data?.user?.contact?.email ?? 'Pas d'email';
  
  return { city, email };
}
```

### Nullish Coalescing (??) - ES2020

**Valeur par défaut SEULEMENT si null/undefined :**

```javascript
// || considère TOUTES les valeurs falsy (0, '', false, null, undefined)
const a = 0 || 10;     // 10 (0 est falsy)
const b = '' || 'hey'; // 'hey' ('' est falsy)

// ?? considère SEULEMENT null/undefined
const c = 0 ?? 10;     // 0 (0 n'est pas null/undefined)
const d = '' ?? 'hey'; // '' ('' n'est pas null/undefined)
const e = null ?? 10;  // 10 (null compte)
const f = undefined ?? 10; // 10 (undefined compte)
```

**Cas d'usage :**
```javascript
function createUser(options) {
  return {
    name: options.name ?? 'Anonyme',
    age: options.age ?? 0,           // ✅ 0 est valide
    showAge: options.showAge ?? true // ✅ false est valide
  };
}

createUser({ name: 'Alice', age: 0, showAge: false });
// { name: 'Alice', age: 0, showAge: false }
```

### Promise.allSettled() - ES2020

**Attendre toutes les promises, même si certaines échouent :**

```javascript
const promises = [
  fetch('/api/users'),
  fetch('/api/posts'),
  fetch('/api/comments')
];

const results = await Promise.allSettled(promises);

results.forEach((result, index) => {
  if (result.status === 'fulfilled') {
    console.log(`Promise ${index} réussie:`, result.value);
  } else {
    console.log(`Promise ${index} échouée:`, result.reason);
  }
});

// vs Promise.all qui échoue si UNE promise échoue
```

### String.prototype.matchAll() - ES2020

```javascript
const text = 'test1 test2 test3';
const regex = /test(\d)/g;

// ❌ ANCIEN (complexe)
let match;
while ((match = regex.exec(text)) !== null) {
  console.log(match);
}

// ✅ MODERNE
for (const match of text.matchAll(regex)) {
  console.log(match[1]); // 1, 2, 3
}
```

### BigInt - ES2020

**Entiers de taille arbitraire :**

```javascript
// Number a une limite
const bigNumber = 9007199254740991; // Number.MAX_SAFE_INTEGER
console.log(bigNumber + 1); // 9007199254740992
console.log(bigNumber + 2); // 9007199254740992 (même résultat !)

// BigInt n'a pas de limite
const bigInt = 9007199254740991n; // n à la fin
console.log(bigInt + 1n); // 9007199254740992n
console.log(bigInt + 2n); // 9007199254740993n

// Opérations
const a = 100n;
const b = 50n;
console.log(a + b);  // 150n
console.log(a * b);  // 5000n
console.log(a / b);  // 2n (division entière)
```

### Logical Assignment (&&=, ||=, ??=) - ES2021

```javascript
// ||= : Assigne si falsy
let a = 0;
a ||= 10; // a = 0 || 10
console.log(a); // 10

// &&= : Assigne si truthy
let b = 5;
b &&= 20; // b = 5 && 20
console.log(b); // 20

// ??= : Assigne si null/undefined
let c = null;
c ??= 30; // c = c ?? 30
console.log(c); // 30

// Cas d'usage
const user = {};
user.name ??= 'Anonyme';
user.settings ||= {};
```

### Numeric Separators - ES2021

**Améliorer la lisibilité des nombres :**

```javascript
// ❌ Difficile à lire
const budget = 1000000000;

// ✅ Facile à lire
const budget = 1_000_000_000; // 1 milliard

// Marche partout
const hex = 0xFF_FF_FF;
const binary = 0b1010_0001_1000_0101;
const decimal = 123_456.789_012;
```

### Array.prototype.at() - ES2022

**Accès par index négatif :**

```javascript
const array = [1, 2, 3, 4, 5];

// ❌ ANCIEN (pour le dernier)
const last = array[array.length - 1]; // 5

// ✅ MODERNE
const last = array.at(-1); // 5
const secondLast = array.at(-2); // 4

// Fonctionne aussi avec index positif
array.at(0); // 1 (pareil que array[0])
```

### Top-level await - ES2022

**await en dehors des fonctions async :**

```javascript
// ❌ ANCIEN
async function main() {
  const users = await fetch('/api/users');
  const data = await users.json();
  console.log(data);
}
main();

// ✅ MODERNE (dans les modules)
const users = await fetch('/api/users');
const data = await users.json();
console.log(data);

// Plus besoin de wrapper function
```

**⚠️ Seulement dans les modules (type="module")**

---

## 15. Best Practices Modernes

### 1. const par défaut

```javascript
// ✅ BON
const users = [];
users.push(newUser); // OK (modifie le contenu, pas la référence)

const config = { apiUrl: 'https://api.example.com' };
config.timeout = 5000; // OK

// ❌ À éviter
let users = [];
```

### 2. Arrow functions pour les callbacks

```javascript
// ✅ BON
[1, 2, 3].map(n => n * 2);

setTimeout(() => console.log('Hi'), 1000);

button.addEventListener('click', () => handleClick());

// ❌ À éviter (verbeux)
[1, 2, 3].map(function(n) {
  return n * 2;
});
```

### 3. Template literals pour strings

```javascript
// ✅ BON
const message = `Bonjour ${name}, tu as ${age} ans`;

const html = `
  <div class="card">
    <h2>${title}</h2>
  </div>
`;

// ❌ À éviter
const message = 'Bonjour ' + name + ', tu as ' + age + ' ans';
```

### 4. Destructuring pour extraire

```javascript
// ✅ BON
const { name, email } = user;
const [first, second] = array;

function greet({ name, age }) {
  return `${name} (${age})`;
}

// ❌ À éviter
const name = user.name;
const email = user.email;
```

### 5. Spread pour copier/fusionner

```javascript
// ✅ BON
const copy = [...original];
const merged = { ...obj1, ...obj2 };

// ❌ À éviter
const copy = original.slice();
const merged = Object.assign({}, obj1, obj2);
```

### 6. Default parameters

```javascript
// ✅ BON
function greet(name = 'Invité') {
  return `Bonjour ${name}`;
}

// ❌ À éviter
function greet(name) {
  name = name || 'Invité';
  return 'Bonjour ' + name;
}
```

### 7. Classes pour les objets complexes

```javascript
// ✅ BON
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
  
  getInfo() {
    return `${this.name} (${this.email})`;
  }
}

// ❌ À éviter
function User(name, email) {
  this.name = name;
  this.email = email;
}
User.prototype.getInfo = function() {
  return this.name + ' (' + this.email + ')';
};
```

### 8. Modules pour organiser

```javascript
// ✅ BON
// utils.js
export function add(a, b) {
  return a + b;
}

// main.js
import { add } from './utils.js';

// ❌ À éviter (scripts globaux)
// <script src="utils.js"></script>
// <script src="main.js"></script>
```

### 9. async/await au lieu de .then()

```javascript
// ✅ BON
async function fetchData() {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error(error);
  }
}

// ❌ À éviter (moins lisible)
function fetchData() {
  return fetch(url)
    .then(response => response.json())
    .then(data => data)
    .catch(error => console.error(error));
}
```

### 10. Optional chaining et nullish coalescing

```javascript
// ✅ BON
const city = user?.address?.city ?? 'Inconnu';

// ❌ À éviter
const city = (user && user.address && user.address.city) || 'Inconnu';
```

---

## Conclusion

**FÉLICITATIONS ! 🎉**

Tu viens de terminer le guide le plus complet sur JavaScript moderne ES6+.

**Ce que tu as appris :**

✅ **let/const** - Variables modernes  
✅ **Arrow functions** - Syntaxe concise  
✅ **Template literals** - Strings puissants  
✅ **Destructuring** - Extraction élégante  
✅ **Spread/Rest** - Manipulation d'arrays/objets  
✅ **Default parameters** - Valeurs par défaut  
✅ **Enhanced object literals** - Objets modernes  
✅ **Classes** - POO moderne  
✅ **Modules** - Organisation du code  
✅ **Map/Set** - Structures de données  
✅ **Symbol** - Valeurs uniques  
✅ **Iterators/Generators** - Itération avancée  
✅ **ES2020+** - Dernières fonctionnalités  
✅ **Best practices** - Code professionnel  

**La différence entre toi maintenant et toi avant ce guide ?**

**AVANT :** Tu codais en JavaScript "old school"  
**MAINTENANT :** Tu codes en JavaScript MODERNE, comme les professionnels de l'industrie

**Tu peux maintenant :**
- Lire n'importe quel code JavaScript moderne
- Travailler avec React, Vue, Angular
- Passer des entretiens techniques
- Contribuer à des projets open source
- Écrire du code propre et maintenable

**Prochaine partie : JavaScript Avancé (Partie 7)**
- Closures, Scope, Hoisting
- This, Bind, Call, Apply
- Prototypes et Héritage
- Design Patterns
- Performance et Optimisation

**Continue comme ça, champion !** 💪🔥

---

**📚 JavaScript Moderne - Partie 6 : ES6+ et Fonctionnalités Modernes**
**💎 100% Gratuit • Pour Tous • À Jamais**
**🔗 GitHub : [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)**