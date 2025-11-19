# JavaScript Avancé - Partie 7 : Maîtrise Complète (1/2)

**🔗 Repository GitHub :** [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)

---

## Table des Matières - Partie 1

1. [Introduction au JavaScript Avancé](#1-introduction-au-javascript-avancé)
2. [Scope et Contexte d'Exécution](#2-scope-et-contexte-dexécution)
3. [Hoisting](#3-hoisting)
4. [Closures](#4-closures)
5. [This - Contexte d'Exécution](#5-this---contexte-dexécution)
6. [Bind, Call, Apply](#6-bind-call-apply)

---

*Note : Ce fichier contient les 6 premiers chapitres. La Partie 2 couvrira les chapitres 7-15 (Prototypes, POO, Design Patterns, Event Loop, Memory Management, Performance, Techniques Avancées, Debugging, et Best Practices).*

---

## 1. Introduction au JavaScript Avancé

### Pourquoi ce guide ?

**STOP aux tutoriels superficiels.**

Ce guide te donne les **vraies compétences** que les entreprises recherchent :
- Comprendre **comment** JavaScript fonctionne (pas juste **quoi** faire)
- Débugger des problèmes **complexes**
- Écrire du code **maintenable** à grande échelle
- Passer des **entretiens techniques** niveau senior

**La différence entre un dev junior et senior ?**

**Junior :** "Ça marche, je ne sais pas pourquoi"  
**Senior :** "Je sais exactement pourquoi ça marche"

### Ce que tu vas apprendre

**Concepts fondamentaux :**
- Comment JavaScript **exécute** ton code
- Pourquoi les bugs bizarres arrivent
- Comment résoudre les problèmes **à la racine**

**Compétences professionnelles :**
- Architecture de code **scalable**
- Patterns utilisés dans **React, Vue, Node.js**
- Optimisation de **performance**

**Après ce guide, tu pourras :**
- ✅ Expliquer n'importe quel comportement JS
- ✅ Débugger des problèmes complexes
- ✅ Passer des entretiens techniques senior
- ✅ Contribuer à n'importe quel codebase
- ✅ Mentorer d'autres développeurs

### Prérequis

**Tu DOIS maîtriser :**
- Parties 1-6 de cette série
- ES6+ (let/const, arrow functions, promises, async/await)
- Programmation fonctionnelle de base (map, filter, reduce)

**Si tu ne maîtrises pas ces bases, RETOURNE aux parties précédentes.**

Ce guide va **loin** et suppose que tu connais les fondamentaux.

---

## 2. Scope et Contexte d'Exécution

### Qu'est-ce que le Scope ?

**Scope = Portée = Où les variables sont accessibles**

**3 types de scope en JavaScript :**
1. **Global Scope** - Accessible partout
2. **Function Scope** - Accessible dans la fonction
3. **Block Scope** - Accessible dans le bloc { }

### Global Scope

```javascript
// Variables déclarées en dehors de toute fonction = global scope
const globalVar = 'Je suis global';

function exemple() {
  console.log(globalVar); // ✅ Accessible
}

if (true) {
  console.log(globalVar); // ✅ Accessible
}

console.log(globalVar); // ✅ Accessible
```

**⚠️ Danger : Pollution du namespace global**

```javascript
// ❌ MAUVAIS - Pollue le global scope
var utilisateur = 'Alice';
var compteur = 0;

// Si tu inclus une autre bibliothèque qui utilise les mêmes noms...
// CONFLIT !

// ✅ BON - Encapsule dans un module ou IIFE
(function() {
  const utilisateur = 'Alice';
  const compteur = 0;
  // Isolé du reste
})();
```

### Function Scope

```javascript
function exemple() {
  const x = 10; // Function scope
  
  if (true) {
    console.log(x); // ✅ Accessible (dans la même fonction)
  }
  
  console.log(x); // ✅ Accessible
}

console.log(x); // ❌ ReferenceError (hors de la fonction)
```

**var vs let/const dans les fonctions :**

```javascript
function testVar() {
  if (true) {
    var x = 10;
  }
  console.log(x); // 10 (var a un function scope)
}

function testLet() {
  if (true) {
    let y = 10;
  }
  console.log(y); // ReferenceError (let a un block scope)
}
```

### Block Scope (ES6+)

**Block = Tout ce qui est entre { }**

```javascript
// if
if (true) {
  const x = 10;
  console.log(x); // ✅ 10
}
console.log(x); // ❌ ReferenceError

// for
for (let i = 0; i < 3; i++) {
  const doubled = i * 2;
  console.log(doubled); // ✅ 0, 2, 4
}
console.log(i); // ❌ ReferenceError
console.log(doubled); // ❌ ReferenceError

// Bloc simple
{
  const secret = 'hello';
  console.log(secret); // ✅ 'hello'
}
console.log(secret); // ❌ ReferenceError
```

### Lexical Scope (Scope Statique)

**JavaScript utilise le "Lexical Scope" = Le scope est déterminé à l'ÉCRITURE du code, pas à l'exécution.**

```javascript
const name = 'Global';

function outer() {
  const name = 'Outer';
  
  function inner() {
    console.log(name); // Quelle valeur ?
  }
  
  return inner;
}

const innerFunc = outer();
innerFunc(); // 'Outer' (pas 'Global')

// Pourquoi ? Parce que inner() a été DÉFINIE dans outer()
// Le scope est déterminé à la DÉFINITION, pas à l'APPEL
```

**Exemple plus complexe :**

```javascript
function makeMultiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = makeMultiplier(2);
const triple = makeMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15

// Chaque fonction retournée "se souvient" de son factor
// C'est le lexical scope en action
```

### Scope Chain (Chaîne de Scope)

**JavaScript cherche les variables de l'intérieur vers l'extérieur.**

```javascript
const a = 'global';

function outer() {
  const b = 'outer';
  
  function middle() {
    const c = 'middle';
    
    function inner() {
      const d = 'inner';
      
      // JavaScript cherche dans cet ordre :
      // 1. inner scope (d)
      // 2. middle scope (c)
      // 3. outer scope (b)
      // 4. global scope (a)
      
      console.log(d); // 'inner' (trouvé au niveau 1)
      console.log(c); // 'middle' (trouvé au niveau 2)
      console.log(b); // 'outer' (trouvé au niveau 3)
      console.log(a); // 'global' (trouvé au niveau 4)
    }
    
    inner();
  }
  
  middle();
}

outer();
```

**Visualisation de la scope chain :**

```
inner scope     →  { d: 'inner' }
    ↓
middle scope    →  { c: 'middle' }
    ↓
outer scope     →  { b: 'outer' }
    ↓
global scope    →  { a: 'global' }
```

### Shadowing (Masquage de Variables)

**Une variable locale peut "cacher" une variable externe.**

```javascript
const name = 'Alice';

function greet() {
  const name = 'Bob'; // Masque la variable globale
  console.log(name); // 'Bob'
}

greet();
console.log(name); // 'Alice'
```

**Exemple complexe :**

```javascript
const value = 10;

function outer() {
  const value = 20;
  
  function inner() {
    const value = 30;
    console.log(value); // 30 (utilise la plus proche)
  }
  
  inner();
  console.log(value); // 20
}

outer();
console.log(value); // 10
```

### Execution Context (Contexte d'Exécution)

**Chaque fois qu'une fonction est appelée, JavaScript crée un "Execution Context".**

**Un Execution Context contient :**
1. **Variable Environment** - Où les variables sont stockées
2. **Lexical Environment** - Lien vers le scope parent
3. **This binding** - Valeur de `this`

```javascript
// Global Execution Context
const globalVar = 'global';

function outer() {
  // Outer Execution Context
  const outerVar = 'outer';
  
  function inner() {
    // Inner Execution Context
    const innerVar = 'inner';
    console.log(innerVar, outerVar, globalVar);
  }
  
  inner();
}

outer();
```

**Stack des Execution Contexts :**

```
[inner() context]     ← Top of stack
[outer() context]
[global context]      ← Bottom of stack
```

### Exemple Pratique : Counter avec Scope

```javascript
// ❌ MAUVAIS - Variable globale
let counter = 0;

function increment() {
  counter++;
  return counter;
}

function reset() {
  counter = 0;
}

// Problème : counter peut être modifié de n'importe où
counter = 999; // Oups !
```

```javascript
// ✅ BON - Encapsulation avec scope
function createCounter() {
  let counter = 0; // Private variable
  
  return {
    increment() {
      counter++;
      return counter;
    },
    
    decrement() {
      counter--;
      return counter;
    },
    
    reset() {
      counter = 0;
    },
    
    getValue() {
      return counter;
    }
  };
}

const counter1 = createCounter();
console.log(counter1.increment()); // 1
console.log(counter1.increment()); // 2
console.log(counter1.getValue()); // 2

// counter n'est PAS accessible directement
console.log(counter1.counter); // undefined
```

---

## 3. Hoisting

### Qu'est-ce que le Hoisting ?

**Hoisting = "Levage" = Les déclarations sont "remontées" en haut du scope**

**JavaScript traite ton code en 2 phases :**
1. **Creation Phase** - Déclare toutes les variables/fonctions
2. **Execution Phase** - Exécute le code ligne par ligne

```javascript
// Ce que tu écris
console.log(x); // undefined
var x = 10;

// Ce que JavaScript exécute
var x; // Declaration remontée
console.log(x); // undefined
x = 10; // Assignment reste en place
```

### Hoisting avec var

```javascript
console.log(a); // undefined (pas d'erreur)
var a = 10;
console.log(a); // 10

// Équivalent à :
var a;
console.log(a); // undefined
a = 10;
console.log(a); // 10
```

**⚠️ C'est pourquoi var est dangereux !**

```javascript
function exemple() {
  console.log(temp); // undefined (pas d'erreur !)
  
  if (false) {
    var temp = 'valeur'; // Jamais exécuté
  }
  
  console.log(temp); // undefined
}

// temp est déclaré même si le if est false
```

### Hoisting avec let/const

**let/const sont AUSSI hoistés, mais dans la "Temporal Dead Zone"**

```javascript
console.log(x); // ReferenceError (pas undefined)
let x = 10;

// La TDZ protège contre l'utilisation avant déclaration
```

**Temporal Dead Zone (TDZ) :**

```javascript
function exemple() {
  // TDZ pour x commence ici
  
  console.log(x); // ReferenceError
  
  let x = 10; // TDZ se termine ici
  
  console.log(x); // 10
}
```

**Pourquoi c'est MIEUX que var :**

```javascript
// ❌ var : Bug silencieux
function testVar() {
  console.log(temp); // undefined (bizarre)
  var temp = 'valeur';
}

// ✅ let : Erreur explicite
function testLet() {
  console.log(temp); // ReferenceError (erreur claire)
  let temp = 'valeur';
}
```

### Hoisting des Fonctions

**Les DÉCLARATIONS de fonctions sont complètement hoistées.**

```javascript
// ✅ Fonctionne
greet(); // 'Hello'

function greet() {
  console.log('Hello');
}

// La fonction entière est remontée
```

**Mais pas les EXPRESSIONS de fonctions :**

```javascript
// ❌ Erreur
greet(); // TypeError: greet is not a function

var greet = function() {
  console.log('Hello');
};

// Équivalent à :
var greet; // Declaration remontée
greet(); // greet est undefined, pas une fonction
greet = function() {
  console.log('Hello');
};
```

**Avec const/let :**

```javascript
// ❌ Erreur
greet(); // ReferenceError

const greet = function() {
  console.log('Hello');
};
```

### Function Declaration vs Function Expression

```javascript
// DÉCLARATION - Complètement hoistée
function declaree() {
  return 'Je suis déclarée';
}

// EXPRESSION - Seulement la variable est hoistée
const expression = function() {
  return 'Je suis une expression';
};

// ARROW FUNCTION - Seulement la variable est hoistée
const arrow = () => {
  return 'Je suis une arrow function';
};
```

**Ordre d'exécution :**

```javascript
// Ce que tu écris
console.log(typeof declaree); // 'function'
console.log(typeof expression); // ReferenceError

function declaree() {}
const expression = function() {};

// Ce que JavaScript exécute
function declaree() {} // Complètement remonté

console.log(typeof declaree); // 'function'
console.log(typeof expression); // ReferenceError (TDZ)

const expression = function() {};
```

### Hoisting dans les Blocs

```javascript
// var ignore les blocs
if (true) {
  var x = 10;
}
console.log(x); // 10 (accessible)

// let/const respectent les blocs
if (true) {
  let y = 20;
}
console.log(y); // ReferenceError
```

### Cas Piège : Hoisting dans les Boucles

```javascript
// ❌ PIÈGE CLASSIQUE avec var
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // 3, 3, 3 (pas 0, 1, 2)
  }, 1000);
}

// Pourquoi ? var i est hoisted hors de la boucle
// Quand les setTimeout s'exécutent, i vaut 3

// ✅ SOLUTION avec let
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i); // 0, 1, 2
  }, 1000);
}

// Chaque itération crée un nouveau scope avec son propre i
```

### Best Practices : Éviter les Problèmes de Hoisting

**1. Déclare toutes les variables EN HAUT du scope**

```javascript
// ✅ BON
function exemple() {
  const a = 1;
  const b = 2;
  const c = 3;
  
  // Logique ici
  console.log(a + b + c);
}

// ❌ À éviter
function exemple() {
  console.log(a); // Où est déclaré a ?
  
  // Beaucoup de code...
  
  const a = 1; // Ah, ici !
}
```

**2. Utilise const par défaut, let si nécessaire, JAMAIS var**

```javascript
// ✅ BON
const API_URL = 'https://api.example.com';
let counter = 0;

// ❌ JAMAIS
var API_URL = 'https://api.example.com';
var counter = 0;
```

**3. Utilise des function declarations pour les fonctions utilitaires**

```javascript
// ✅ BON - Peut être appelée avant la déclaration
greet();

function greet() {
  console.log('Hello');
}

// ✅ Ou déclare en haut
const greet = () => {
  console.log('Hello');
};

greet();
```

---

## 4. Closures

### Qu'est-ce qu'une Closure ?

**Closure = Une fonction qui "se souvient" des variables de son scope parent, même après que le parent ait fini son exécution.**

**Définition simple :** Une fonction + son environnement lexical

```javascript
function outer() {
  const message = 'Hello';
  
  function inner() {
    console.log(message); // Accède à message
  }
  
  return inner;
}

const myFunc = outer(); // outer() a fini son exécution
myFunc(); // 'Hello' - Mais inner se souvient de message !

// C'EST UNE CLOSURE
```

**Pourquoi c'est important ?**
- Data privacy (variables privées)
- Factory functions
- Callbacks et event handlers
- Currying et partial application
- Module pattern

### Exemple Simple de Closure

```javascript
function createGreeter(greeting) {
  // Cette variable sera "capturée" par la closure
  
  return function(name) {
    console.log(`${greeting}, ${name}!`);
  };
}

const sayHello = createGreeter('Hello');
const sayBonjour = createGreeter('Bonjour');

sayHello('Alice');    // 'Hello, Alice!'
sayBonjour('Bob');    // 'Bonjour, Bob!'

// Chaque fonction retournée se souvient de son greeting
```

### Closures et Variables Privées

**Le pattern le plus utile : Encapsulation de données**

```javascript
function createBankAccount(initialBalance) {
  let balance = initialBalance; // Variable PRIVÉE
  
  return {
    deposit(amount) {
      if (amount > 0) {
        balance += amount;
        return balance;
      }
    },
    
    withdraw(amount) {
      if (amount > 0 && amount <= balance) {
        balance -= amount;
        return balance;
      }
      return 'Fonds insuffisants';
    },
    
    getBalance() {
      return balance;
    }
  };
}

const myAccount = createBankAccount(1000);

console.log(myAccount.getBalance()); // 1000
myAccount.deposit(500);              // 1500
myAccount.withdraw(200);             // 1300

// ❌ Impossible d'accéder directement à balance
console.log(myAccount.balance); // undefined

// ❌ Impossible de modifier balance directement
myAccount.balance = 999999; // N'affecte pas la vraie balance
console.log(myAccount.getBalance()); // 1300 (inchangé)
```

### Closures dans les Boucles

**Le piège classique :**

```javascript
// ❌ PIÈGE
function createFunctions() {
  const functions = [];
  
  for (var i = 0; i < 3; i++) {
    functions.push(function() {
      console.log(i);
    });
  }
  
  return functions;
}

const funcs = createFunctions();
funcs[0](); // 3 (pas 0)
funcs[1](); // 3 (pas 1)
funcs[2](); // 3 (pas 2)

// Pourquoi ? Toutes les fonctions partagent la même variable i
// Quand elles s'exécutent, i vaut 3
```

**Solution 1 : let au lieu de var**

```javascript
// ✅ SOLUTION 1
function createFunctions() {
  const functions = [];
  
  for (let i = 0; i < 3; i++) {
    functions.push(function() {
      console.log(i);
    });
  }
  
  return functions;
}

const funcs = createFunctions();
funcs[0](); // 0 ✅
funcs[1](); // 1 ✅
funcs[2](); // 2 ✅

// let crée un nouveau scope à chaque itération
```

**Solution 2 : IIFE (ancienne méthode)**

```javascript
// ✅ SOLUTION 2 (ES5)
function createFunctions() {
  const functions = [];
  
  for (var i = 0; i < 3; i++) {
    functions.push((function(index) {
      return function() {
        console.log(index);
      };
    })(i));
  }
  
  return functions;
}

const funcs = createFunctions();
funcs[0](); // 0 ✅
funcs[1](); // 1 ✅
funcs[2](); // 2 ✅
```

### Closures et Event Handlers

```javascript
// ❌ PROBLÈME
function setupButtons() {
  const buttons = document.querySelectorAll('button');
  
  for (var i = 0; i < buttons.length; i++) {
    buttons[i].addEventListener('click', function() {
      console.log('Button ' + i + ' clicked');
    });
  }
}

// Tous les boutons afficheront le même i (le dernier)

// ✅ SOLUTION
function setupButtons() {
  const buttons = document.querySelectorAll('button');
  
  for (let i = 0; i < buttons.length; i++) {
    buttons[i].addEventListener('click', function() {
      console.log('Button ' + i + ' clicked');
    });
  }
}

// Chaque handler se souvient de son i
```

**Autre exemple avec données spécifiques :**

```javascript
function createButton(id, label) {
  const button = document.createElement('button');
  button.textContent = label;
  
  // La closure capture id et label
  button.addEventListener('click', function() {
    console.log(`Button ${id} (${label}) clicked`);
  });
  
  return button;
}

const btn1 = createButton(1, 'First');
const btn2 = createButton(2, 'Second');

// Chaque bouton se souvient de ses propres id et label
```

### Closures et setTimeout

```javascript
// ❌ PROBLÈME
for (var i = 1; i <= 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, i * 1000);
}
// Affiche : 4, 4, 4 (toutes les fonctions voient le même i)

// ✅ SOLUTION 1 : let
for (let i = 1; i <= 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, i * 1000);
}
// Affiche : 1, 2, 3

// ✅ SOLUTION 2 : Closure explicite
for (var i = 1; i <= 3; i++) {
  (function(index) {
    setTimeout(function() {
      console.log(index);
    }, index * 1000);
  })(i);
}
// Affiche : 1, 2, 3
```

### Practical Example : Counter avec Closures

```javascript
function createCounter() {
  let count = 0;
  
  return {
    increment() {
      count++;
      return count;
    },
    
    decrement() {
      count--;
      return count;
    },
    
    reset() {
      count = 0;
      return count;
    },
    
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();

console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
console.log(counter.decrement()); // 1
console.log(counter.reset());     // 0

// count est complètement privé
console.log(counter.count); // undefined
```

### Practical Example : Module Pattern

```javascript
const calculator = (function() {
  // Variables privées
  let history = [];
  
  // Fonctions privées
  function addToHistory(operation, result) {
    history.push({ operation, result, timestamp: new Date() });
  }
  
  // API publique
  return {
    add(a, b) {
      const result = a + b;
      addToHistory(`${a} + ${b}`, result);
      return result;
    },
    
    subtract(a, b) {
      const result = a - b;
      addToHistory(`${a} - ${b}`, result);
      return result;
    },
    
    multiply(a, b) {
      const result = a * b;
      addToHistory(`${a} * ${b}`, result);
      return result;
    },
    
    divide(a, b) {
      if (b === 0) throw new Error('Division par zéro');
      const result = a / b;
      addToHistory(`${a} / ${b}`, result);
      return result;
    },
    
    getHistory() {
      return [...history]; // Retourne une copie
    },
    
    clearHistory() {
      history = [];
    }
  };
})();

// Utilisation
calculator.add(5, 3);        // 8
calculator.multiply(4, 2);   // 8
console.log(calculator.getHistory());

// ❌ Impossible d'accéder aux variables/fonctions privées
console.log(calculator.history);        // undefined
console.log(calculator.addToHistory);  // undefined
```

### Practical Example : Once Function

```javascript
function once(fn) {
  let called = false;
  let result;
  
  return function(...args) {
    if (!called) {
      called = true;
      result = fn.apply(this, args);
    }
    return result;
  };
}

const initialize = once(() => {
  console.log('Initializing...');
  return { status: 'initialized' };
});

initialize(); // 'Initializing...'
initialize(); // Rien (déjà appelé)
initialize(); // Rien (déjà appelé)
```

### Practical Example : Memoization

```javascript
function memoize(fn) {
  const cache = {};
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (key in cache) {
      console.log('Récupéré du cache');
      return cache[key];
    }
    
    console.log('Calculé');
    const result = fn.apply(this, args);
    cache[key] = result;
    return result;
  };
}

const slowFunction = memoize((n) => {
  // Simulation d'un calcul lent
  let result = 0;
  for (let i = 0; i < 1000000; i++) {
    result += i;
  }
  return result + n;
});

slowFunction(5); // 'Calculé' (lent)
slowFunction(5); // 'Récupéré du cache' (instantané)
slowFunction(10); // 'Calculé' (lent)
slowFunction(10); // 'Récupéré du cache' (instantané)
```

### Memory Leaks avec les Closures

**⚠️ Les closures peuvent causer des memory leaks si mal utilisées**

```javascript
// ❌ MEMORY LEAK
function attachHandler() {
  const hugeData = new Array(1000000).fill('data');
  
  document.getElementById('button').addEventListener('click', function() {
    console.log('clicked');
    // Cette closure garde hugeData en mémoire
    // même si hugeData n'est jamais utilisé !
  });
}

// ✅ SOLUTION 1 : Ne pas capturer de données inutiles
function attachHandler() {
  const hugeData = new Array(1000000).fill('data');
  const processedData = processData(hugeData); // Traite les données
  
  document.getElementById('button').addEventListener('click', function() {
    console.log('clicked');
    // Seul processedData est gardé, pas hugeData
  });
}

// ✅ SOLUTION 2 : Libérer la référence
function attachHandler() {
  let hugeData = new Array(1000000).fill('data');
  const result = hugeData.length;
  hugeData = null; // Libère la mémoire
  
  document.getElementById('button').addEventListener('click', function() {
    console.log(result); // Utilise seulement result, pas hugeData
  });
}
```

### Closures : Interview Questions

**Q1 : Qu'affiche ce code ?**

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```

**Réponse :** `3, 3, 3` (toutes les fonctions voient le même `i`)

**Q2 : Comment fixer ce code ?**

```javascript
// Solution 1 : let
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}

// Solution 2 : Closure
for (var i = 0; i < 3; i++) {
  ((index) => {
    setTimeout(() => console.log(index), 100);
  })(i);
}
```

**Q3 : Qu'affiche ce code ?**

```javascript
function createFunctions() {
  const arr = [];
  
  for (let i = 0; i < 3; i++) {
    arr.push(() => i);
  }
  
  return arr;
}

const funcs = createFunctions();
console.log(funcs[0]()); // ?
console.log(funcs[1]()); // ?
console.log(funcs[2]()); // ?
```

**Réponse :** `0, 1, 2` (chaque fonction capture son propre `i`)

---

## 5. This - Contexte d'Exécution

### Qu'est-ce que `this` ?

**`this` = Référence au contexte d'exécution actuel**

**La valeur de `this` dépend de COMMENT la fonction est APPELÉE (pas où elle est définie).**

### Les 4 Règles de `this`

**1. Default Binding (Binding par défaut)**
**2. Implicit Binding (Binding implicite)**
**3. Explicit Binding (Binding explicite)**
**4. New Binding (Binding avec new)**

### 1. Default Binding

**Fonction appelée sans contexte = `this` pointe vers l'objet global (ou undefined en strict mode)**

```javascript
function showThis() {
  console.log(this);
}

showThis(); // window (navigateur) ou global (Node.js)

// En strict mode
'use strict';
function showThis() {
  console.log(this);
}

showThis(); // undefined
```

### 2. Implicit Binding

**Fonction appelée comme méthode d'un objet = `this` pointe vers l'objet**

```javascript
const person = {
  name: 'Alice',
  greet() {
    console.log(`Hello, je suis ${this.name}`);
  }
};

person.greet(); // 'Hello, je suis Alice'
// this = person (l'objet avant le .)
```

**Nested objects :**

```javascript
const company = {
  name: 'TechCorp',
  employee: {
    name: 'Bob',
    greet() {
      console.log(`Je travaille chez ${this.name}`);
    }
  }
};

company.employee.greet(); // 'Je travaille chez Bob'
// this = employee (l'objet directement avant le .)
```

### Perte du Contexte (Piège Classique)

```javascript
const person = {
  name: 'Alice',
  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

person.greet(); // 'Hello, Alice' ✅

// ❌ Perte du contexte
const greetFunction = person.greet;
greetFunction(); // 'Hello, undefined'
// this n'est plus person

// ❌ Dans un callback
setTimeout(person.greet, 1000); // 'Hello, undefined'

// ✅ SOLUTIONS
// Solution 1 : Arrow function wrapper
setTimeout(() => person.greet(), 1000);

// Solution 2 : bind
setTimeout(person.greet.bind(person), 1000);
```

### 3. Explicit Binding : call, apply, bind

**Tu peux FORCER la valeur de `this` avec `call`, `apply`, ou `bind`**

#### call()

```javascript
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person1 = { name: 'Alice' };
const person2 = { name: 'Bob' };

greet.call(person1, 'Hello', '!'); // 'Hello, Alice!'
greet.call(person2, 'Hi', '?');    // 'Hi, Bob?'

// call(thisArg, arg1, arg2, ...)
```

#### apply()

```javascript
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}

const person = { name: 'Alice' };

greet.apply(person, ['Hello', '!']); // 'Hello, Alice!'

// apply(thisArg, [arg1, arg2, ...])
// La différence : les arguments sont dans un array
```

#### bind()

```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const person = { name: 'Alice' };

const boundGreet = greet.bind(person);
boundGreet('Hello'); // 'Hello, Alice'

// bind retourne une NOUVELLE fonction avec this fixé
```

**Différence call/apply vs bind :**

```javascript
// call et apply : EXÉCUTENT la fonction immédiatement
greet.call(person, 'Hello');   // Exécute tout de suite
greet.apply(person, ['Hello']); // Exécute tout de suite

// bind : RETOURNE une nouvelle fonction
const boundGreet = greet.bind(person); // Ne l'exécute pas
boundGreet('Hello'); // Exécute plus tard
```

### 4. New Binding

**Avec `new`, `this` pointe vers le nouvel objet créé**

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const alice = new Person('Alice', 25);
console.log(alice.name); // 'Alice'

// Quand tu utilises new :
// 1. Un nouvel objet vide est créé
// 2. this pointe vers ce nouvel objet
// 3. L'objet est retourné automatiquement
```

### Arrow Functions et `this`

**Les arrow functions N'ONT PAS leur propre `this`**

**Elles HÉRITENT du `this` du scope parent (lexical this)**

```javascript
const person = {
  name: 'Alice',
  
  // ✅ Fonction classique
  greet: function() {
    console.log(this.name); // this = person
    
    setTimeout(function() {
      console.log(this.name); // this = undefined (ou window)
    }, 1000);
  },
  
  // ✅ Arrow function dans setTimeout
  greetArrow: function() {
    console.log(this.name); // this = person
    
    setTimeout(() => {
      console.log(this.name); // this = person (hérité)
    }, 1000);
  }
};

person.greet();      // 'Alice', puis undefined
person.greetArrow(); // 'Alice', puis 'Alice'
```

**Arrow functions dans des méthodes d'objets :**

```javascript
// ❌ MAUVAIS - Arrow function comme méthode
const person = {
  name: 'Alice',
  greet: () => {
    console.log(this.name); // this n'est PAS person
  }
};

person.greet(); // undefined

// ✅ BON - Fonction classique ou shorthand
const person = {
  name: 'Alice',
  greet() {
    console.log(this.name); // this = person
  }
};

person.greet(); // 'Alice'
```

### Priorité des Règles de `this`

**De la plus haute à la plus basse :**

1. **new binding** - `new Foo()`
2. **Explicit binding** - `call`, `apply`, `bind`
3. **Implicit binding** - `obj.method()`
4. **Default binding** - Fonction simple

```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const person1 = { name: 'Alice' };
const person2 = { name: 'Bob' };

// Implicit binding
person1.greet = greet;
person1.greet('Hello'); // 'Hello, Alice'

// Explicit binding bat implicit binding
person1.greet.call(person2, 'Hi'); // 'Hi, Bob'

// bind bat implicit binding
const boundGreet = greet.bind(person2);
person1.greet = boundGreet;
person1.greet('Hey'); // 'Hey, Bob' (pas Alice)
```

### Cas Pratique : Event Handlers

```javascript
class Button {
  constructor(label) {
    this.label = label;
    this.clickCount = 0;
  }
  
  // ❌ PROBLÈME : this n'est pas Button dans le handler
  handleClick1() {
    console.log(`${this.label} clicked`); // this = element DOM
    this.clickCount++; // Erreur
  }
  
  // ✅ SOLUTION 1 : Arrow function
  handleClick2 = () => {
    console.log(`${this.label} clicked`); // this = Button instance
    this.clickCount++;
  }
  
  // ✅ SOLUTION 2 : bind dans le constructor
  constructor(label) {
    this.label = label;
    this.clickCount = 0;
    this.handleClick3 = this.handleClick3.bind(this);
  }
  
  handleClick3() {
    console.log(`${this.label} clicked`);
    this.clickCount++;
  }
  
  // ✅ SOLUTION 3 : Wrapper arrow function
  attach(element) {
    element.addEventListener('click', () => {
      this.handleClick1(); // this = Button instance dans la arrow
    });
  }
}

const btn = new Button('Submit');

// ❌ Ne marche pas
document.querySelector('#btn1').addEventListener('click', btn.handleClick1);

// ✅ Marche
document.querySelector('#btn2').addEventListener('click', btn.handleClick2);
document.querySelector('#btn3').addEventListener('click', btn.handleClick3);
```

### Interview Questions sur `this`

**Q1 : Qu'affiche ce code ?**

```javascript
const obj = {
  name: 'Alice',
  greet() {
    console.log(this.name);
  }
};

const greet = obj.greet;
greet();
```

**Réponse :** `undefined` (perte du contexte, default binding)

**Q2 : Comment fixer ce code pour afficher 'Alice' ?**

```javascript
// Solution 1
const greet = obj.greet.bind(obj);
greet();

// Solution 2
const greet = () => obj.greet();
greet();
```

**Q3 : Qu'affiche ce code ?**

```javascript
const obj = {
  name: 'Alice',
  greet: () => {
    console.log(this.name);
  }
};

obj.greet();
```

**Réponse :** `undefined` (arrow function n'a pas son propre `this`)

---

## 6. Bind, Call, Apply

### call() - Appel avec Arguments

**Syntaxe :** `function.call(thisArg, arg1, arg2, ...)`

**Exécute la fonction IMMÉDIATEMENT avec `this` défini**

```javascript
function introduce(greeting, punctuation) {
  console.log(`${greeting}, je suis ${this.name}${punctuation}`);
}

const person1 = { name: 'Alice' };
const person2 = { name: 'Bob' };

introduce.call(person1, 'Bonjour', '!'); // 'Bonjour, je suis Alice!'
introduce.call(person2, 'Salut', '?');   // 'Salut, je suis Bob?'
```

### apply() - Appel avec Array

**Syntaxe :** `function.apply(thisArg, [arg1, arg2, ...])`

**Identique à `call`, mais les arguments sont dans un array**

```javascript
function introduce(greeting, punctuation) {
  console.log(`${greeting}, je suis ${this.name}${punctuation}`);
}

const person = { name: 'Alice' };

introduce.apply(person, ['Bonjour', '!']); // 'Bonjour, je suis Alice!'
```

**Cas d'usage classique : Math.max/min avec array**

```javascript
const numbers = [5, 6, 2, 3, 7];

// ❌ Ne marche pas
const max = Math.max(numbers); // NaN

// ✅ Avec apply
const max = Math.max.apply(null, numbers); // 7

// ✅ Moderne : spread operator
const max = Math.max(...numbers); // 7
```

### bind() - Créer une Fonction Liée

**Syntaxe :** `function.bind(thisArg, arg1, arg2, ...)`

**Retourne une NOUVELLE fonction avec `this` fixé (ne l'exécute pas)**

```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const person = { name: 'Alice' };

const boundGreet = greet.bind(person);
boundGreet('Hello'); // 'Hello, Alice'
boundGreet('Hi');    // 'Hi, Alice'

// this est TOUJOURS person, peu importe comment tu appelles boundGreet
```

### Différences call/apply/bind

```javascript
function greet(greeting, punctuation) {
  console.log(`${greeting} ${this.name}${punctuation}`);
}

const person = { name: 'Alice' };

// call : Arguments séparés, exécute immédiatement
greet.call(person, 'Hello', '!'); // 'Hello Alice!'

// apply : Arguments dans un array, exécute immédiatement
greet.apply(person, ['Hello', '!']); // 'Hello Alice!'

// bind : Arguments séparés, retourne une nouvelle fonction
const boundGreet = greet.bind(person, 'Hello');
boundGreet('!'); // 'Hello Alice!'
```

**Tableau récapitulatif :**

| Méthode | Arguments | Exécution | Retour |
|---------|-----------|-----------|---------|
| `call` | Séparés | Immédiate | Résultat de la fonction |
| `apply` | Array | Immédiate | Résultat de la fonction |
| `bind` | Séparés | Différée | Nouvelle fonction |

### Partial Application avec bind

**Tu peux pré-remplir des arguments avec `bind`**

```javascript
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2); // a = 2
const triple = multiply.bind(null, 3); // a = 3

console.log(double(5)); // 10 (2 * 5)
console.log(triple(5)); // 15 (3 * 5)
```

**Exemple plus complexe :**

```javascript
function log(level, message) {
  console.log(`[${level}] ${message}`);
}

const logError = log.bind(null, 'ERROR');
const logWarning = log.bind(null, 'WARNING');
const logInfo = log.bind(null, 'INFO');

logError('Database connection failed');   // [ERROR] Database connection failed
logWarning('Deprecated API used');         // [WARNING] Deprecated API used
logInfo('User logged in');                 // [INFO] User logged in
```

### call/apply pour Emprunter des Méthodes

**Tu peux utiliser des méthodes d'un objet sur un autre objet**

```javascript
const person1 = {
  name: 'Alice',
  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

const person2 = { name: 'Bob' };

// Emprunte la méthode greet de person1 pour person2
person1.greet.call(person2); // 'Hello, Bob'
```

**Exemple avec Array methods :**

```javascript
// arguments est un objet array-like, pas un vrai array
function sum() {
  // ❌ arguments.reduce n'existe pas
  // console.log(arguments.reduce((a, b) => a + b));
  
  // ✅ Emprunte reduce de Array.prototype
  const result = Array.prototype.reduce.call(arguments, (a, b) => a + b);
  return result;
}

console.log(sum(1, 2, 3, 4)); // 10

// ✅ Moderne : Spread ou Array.from
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b);
}

function sum() {
  return Array.from(arguments).reduce((a, b) => a + b);
}
```

**Convertir NodeList en Array :**

```javascript
// ❌ NodeList n'a pas tous les méthodes d'array
const divs = document.querySelectorAll('div');
divs.map(div => div.textContent); // Erreur

// ✅ Solution 1 : call avec slice
const divsArray = Array.prototype.slice.call(divs);
divsArray.map(div => div.textContent);

// ✅ Solution 2 : Array.from (moderne)
const divsArray = Array.from(divs);
divsArray.map(div => div.textContent);

// ✅ Solution 3 : Spread (moderne)
const divsArray = [...divs];
divsArray.map(div => div.textContent);
```

### bind pour Event Handlers

```javascript
class Counter {
  constructor() {
    this.count = 0;
    this.button = document.getElementById('btn');
    
    // ❌ Sans bind, this sera l'élément DOM
    // this.button.addEventListener('click', this.handleClick);
    
    // ✅ Avec bind, this sera l'instance Counter
    this.button.addEventListener('click', this.handleClick.bind(this));
  }
  
  handleClick() {
    this.count++;
    console.log(`Count: ${this.count}`);
  }
}

const counter = new Counter();
```

**Alternative moderne : Arrow function**

```javascript
class Counter {
  constructor() {
    this.count = 0;
    this.button = document.getElementById('btn');
    
    // ✅ Arrow function property
    this.button.addEventListener('click', this.handleClick);
  }
  
  handleClick = () => {
    this.count++;
    console.log(`Count: ${this.count}`);
  }
}
```

### Polyfill bind (Comment bind fonctionne)

**Comprendre comment `bind` est implémenté :**

```javascript
// Implémentation simplifiée de bind
Function.prototype.myBind = function(context, ...boundArgs) {
  const fn = this; // La fonction à binder
  
  return function(...args) {
    // Fusionne les arguments pré-remplis et les nouveaux arguments
    return fn.apply(context, [...boundArgs, ...args]);
  };
};

// Utilisation
function greet(greeting, punctuation) {
  console.log(`${greeting} ${this.name}${punctuation}`);
}

const person = { name: 'Alice' };
const boundGreet = greet.myBind(person, 'Hello');

boundGreet('!'); // 'Hello Alice!'
```

### Cas Pratique : Debounce avec bind

```javascript
function debounce(func, delay) {
  let timeoutId;
  
  return function(...args) {
    clearTimeout(timeoutId);
    
    timeoutId = setTimeout(() => {
      func.apply(this, args); // Preserve le contexte
    }, delay);
  };
}

// Utilisation
const input = document.getElementById('search');

const searchAPI = function(event) {
  console.log('Searching for:', event.target.value);
  console.log('Context:', this); // L'élément input
};

input.addEventListener('input', debounce(searchAPI, 300));
```

### Interview Questions

**Q1 : Quelle est la différence entre call et apply ?**

**Réponse :** `call` prend des arguments séparés, `apply` prend un array d'arguments.

**Q2 : Quelle est la différence entre call et bind ?**

**Réponse :** `call` exécute immédiatement, `bind` retourne une nouvelle fonction.

**Q3 : Comment créer un partial application de cette fonction ?**

```javascript
function add(a, b, c) {
  return a + b + c;
}

// Créer add5 qui ajoute toujours 5 au premier argument
const add5 = add.bind(null, 5);
console.log(add5(10, 20)); // 35 (5 + 10 + 20)
```

**Q4 : Qu'affiche ce code ?**

```javascript
const obj = {
  name: 'Alice',
  greet: function() {
    console.log(this.name);
  }.bind({ name: 'Bob' })
};

obj.greet();
```

**Réponse :** `'Bob'` (bind fixe le this de manière permanente)

---

## Conclusion de la Partie 1

**🎉 BRAVO ! Tu as complété les 6 premiers chapitres !**

### Ce que tu maîtrises maintenant

**Concepts Fondamentaux Maîtrisés :**
✅ **Scope** - Global, Function, Block, Lexical  
✅ **Hoisting** - var, let, const, functions  
✅ **Closures** - Variables privées, factory functions, module pattern  
✅ **This** - Les 4 règles, arrow functions, pièges classiques  
✅ **Bind/Call/Apply** - Manipulation du contexte, partial application  

### Tu es capable de :

🚀 **Expliquer** comment JavaScript gère les scopes  
🚀 **Éviter** les pièges du hoisting  
🚀 **Utiliser** les closures pour créer du code élégant  
🚀 **Maîtriser** this dans tous les contextes  
🚀 **Manipuler** le contexte d'exécution avec bind/call/apply  

### Prochaine Étape : Partie 2

**La Partie 2 couvrira les 9 chapitres restants :**

7. **Prototypes et Héritage** - Chaîne de prototypes, héritage prototypal  
8. **POO** - Classes, encapsulation, polymorphisme  
9. **Design Patterns** - Singleton, Factory, Observer, Proxy, etc.  
10. **Event Loop** - Microtasks, Macrotasks, asynchrone  
11. **Memory Management** - Garbage collection, memory leaks  
12. **Performance** - Optimisation, debounce, throttle  
13. **Techniques Avancées** - Currying, composition, generators  
14. **Debugging Avancé** - Console API, error handling  
15. **Best Practices** - Code organisation, sécurité  

**Tu es sur la voie royale pour devenir un développeur JavaScript SENIOR !** 💪🔥

---

**📚 JavaScript Avancé - Partie 7.1 : Maîtrise Complète**
**💎 100% Gratuit • Pour Tous • À Jamais**
**🔗 GitHub : [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)**