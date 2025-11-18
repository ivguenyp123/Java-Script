# 🚀 JavaScript : Guide Complet du Débutant à l'Expert - Partie 2

## 📚 Les Fondamentaux : Variables, Types et Opérateurs

> **"Maîtriser les fondamentaux, c'est construire des fondations solides pour devenir un expert !"**

**Par la Learning Schooling Foundation**  
*Niveau : MIT • Google • Berkeley • Stanford*

---

## 📖 Table des Matières - Partie 2

1. **Variables : Stocker des Données**
2. **Types de Données Primitifs**
3. **Opérateurs : Calculer et Comparer**
4. **Strings : Manipuler du Texte**
5. **Type Conversion et Coercion**
6. **Truthy et Falsy : Valeurs Booléennes**
7. **Template Literals : Strings Modernes**
8. **Premier Projet : Calculatrice Interactive**

---

## 1. Variables : Stocker des Données

### 1.1 Qu'est-ce qu'une Variable ?

Une **variable** est comme une boîte avec un nom qui contient une valeur.

#### 🎁 Analogie de la Boîte

```
┌─────────────────┐
│   NOM: age      │  ← Étiquette (nom de la variable)
├─────────────────┤
│   CONTENU: 25   │  ← Valeur stockée
└─────────────────┘
```

**Dans la vie réelle :**
- Tu as une boîte étiquetée "Chaussures"
- Dedans, tu mets des chaussures
- Plus tard, tu peux regarder ce qu'il y a dans la boîte "Chaussures"

**En JavaScript :**
```javascript
let age = 25;          // Créer une boîte "age" avec 25 dedans
console.log(age);      // Regarder ce qu'il y a dans la boîte
age = 26;              // Changer le contenu de la boîte
console.log(age);      // Regarder à nouveau
```

### 1.2 Les 3 Façons de Déclarer une Variable

JavaScript a **3 mots-clés** pour créer des variables :

1. **`let`** ← Moderne, recommandé ✅
2. **`const`** ← Moderne, pour les constantes ✅
3. **`var`** ← Ancien, à éviter ⚠️

#### 📘 `let` : Variable Modifiable

```javascript
let prenom = "Amina";
console.log(prenom);  // Affiche : Amina

prenom = "Fatou";     // On peut changer !
console.log(prenom);  // Affiche : Fatou
```

**Caractéristiques de `let` :**
- ✅ Peut être modifié
- ✅ Portée de bloc (block scope)
- ✅ Recommandé pour les valeurs qui changent

**Exemples d'usage :**
```javascript
let score = 0;          // Score d'un jeu (change)
let temperature = 25;   // Température (varie)
let compteur = 1;       // Compteur (s'incrémente)
```

#### 📗 `const` : Variable Constante

```javascript
const pays = "Sénégal";
console.log(pays);  // Affiche : Sénégal

pays = "France";    // ❌ ERREUR ! Cannot assign to const
```

**Caractéristiques de `const` :**
- ❌ Ne peut PAS être modifié
- ✅ Portée de bloc (block scope)
- ✅ Recommandé pour les valeurs qui ne changent pas

**Exemples d'usage :**
```javascript
const PI = 3.14159;              // Constante mathématique
const TAUX_TVA = 0.20;           // Taux fixe
const COULEUR_PRIMAIRE = "blue"; // Config qui ne change pas
```

**⚠️ Exception avec les Objets et Arrays :**
```javascript
const person = { nom: "Ali" };
person.nom = "Ahmed";     // ✅ OK ! On modifie le contenu
person.age = 30;          // ✅ OK ! On ajoute une propriété

person = { nom: "Omar" }; // ❌ ERREUR ! On ne peut pas réassigner

const nombres = [1, 2, 3];
nombres.push(4);          // ✅ OK ! On modifie le contenu
nombres = [5, 6, 7];      // ❌ ERREUR ! On ne peut pas réassigner
```

#### 📕 `var` : Ancienne Méthode (À Éviter)

```javascript
var ville = "Dakar";
var ville = "Thiès";  // ✅ Pas d'erreur (problématique!)
console.log(ville);   // Affiche : Thiès
```

**Problèmes avec `var` :**
- ⚠️ Peut être redéclaré (source de bugs)
- ⚠️ Portée de fonction (pas de bloc)
- ⚠️ Hoisting confus
- ⚠️ Ancien standard

**🚫 N'utilise JAMAIS `var` ! Utilise toujours `let` ou `const`.**

#### 📊 Comparaison : let vs const vs var

| Aspect | `let` | `const` | `var` |
|--------|-------|---------|-------|
| **Réassignation** | ✅ Oui | ❌ Non | ✅ Oui |
| **Redéclaration** | ❌ Non | ❌ Non | ✅ Oui (problème!) |
| **Portée** | Bloc | Bloc | Fonction |
| **Hoisting** | Oui (TDZ) | Oui (TDZ) | Oui |
| **Recommandé ?** | ✅ Oui | ✅ Oui | ❌ Non |

### 1.3 Règles de Nommage des Variables

#### ✅ Règles OBLIGATOIRES

**1. Commence par une lettre, $ ou _**
```javascript
let nom = "Ali";        // ✅ OK
let $prix = 100;        // ✅ OK
let _temp = 25;         // ✅ OK
let 5nombre = 10;       // ❌ ERREUR ! Ne peut pas commencer par un chiffre
```

**2. Peut contenir lettres, chiffres, $ et _**
```javascript
let age2 = 30;          // ✅ OK
let user_name = "Sara"; // ✅ OK
let prix$ = 50;         // ✅ OK
let mon-nom = "Ali";    // ❌ ERREUR ! Pas de tiret (-)
```

**3. Pas d'espaces**
```javascript
let mon nom = "Ali";    // ❌ ERREUR ! Espace interdit
let monNom = "Ali";     // ✅ OK (camelCase)
let mon_nom = "Ali";    // ✅ OK (snake_case)
```

**4. Sensible à la casse**
```javascript
let nom = "Ali";
let Nom = "Ahmed";
let NOM = "Omar";
// Ces 3 variables sont DIFFÉRENTES !
```

**5. Pas de mots réservés**
```javascript
let let = 10;           // ❌ ERREUR ! 'let' est réservé
let if = true;          // ❌ ERREUR ! 'if' est réservé
let function = "test";  // ❌ ERREUR ! 'function' est réservé
```

**Mots réservés JavaScript :**
```
break, case, catch, class, const, continue, debugger,
default, delete, do, else, export, extends, finally,
for, function, if, import, in, instanceof, let, new,
return, super, switch, this, throw, try, typeof, var,
void, while, with, yield
```

#### 🎨 Conventions de Nommage (Bonnes Pratiques)

**1. camelCase (Recommandé pour variables et fonctions)**
```javascript
let firstName = "Amina";         // ✅ Parfait
let userAge = 25;                // ✅ Parfait
let totalScore = 1000;           // ✅ Parfait
let isLoggedIn = true;           // ✅ Parfait
```

**2. PascalCase (Pour les classes)**
```javascript
class UserProfile { }            // ✅ OK (on verra plus tard)
class ProductCart { }            // ✅ OK
```

**3. SCREAMING_SNAKE_CASE (Pour les constantes)**
```javascript
const MAX_USERS = 100;           // ✅ Parfait
const API_KEY = "abc123";        // ✅ Parfait
const DEFAULT_TIMEOUT = 5000;    // ✅ Parfait
```

**4. Noms descriptifs**
```javascript
// ❌ Mauvais (pas clair)
let x = 25;
let temp = "Ali";
let data = 100;

// ✅ Bon (descriptif)
let userAge = 25;
let userName = "Ali";
let totalPrice = 100;
```

**5. Noms en anglais (standard international)**
```javascript
// ✅ Recommandé (anglais)
let firstName = "Amina";
let userAge = 25;
let totalPrice = 1000;

// ⚠️ Fonctionne mais non recommandé (français)
let prenom = "Amina";
let ageUtilisateur = 25;
let prixTotal = 1000;
```

#### 💡 Convention pour les Booléens

```javascript
// Préfixe avec is, has, can, should
let isActive = true;             // ✅ Question : Est actif ?
let hasPermission = false;       // ✅ Question : A la permission ?
let canEdit = true;              // ✅ Question : Peut éditer ?
let shouldUpdate = false;        // ✅ Question : Devrait mettre à jour ?
```

### 1.4 Déclaration vs Initialisation

#### 📝 Déclaration : Créer la Variable

```javascript
let age;                // Déclaration seule
console.log(age);       // undefined (pas de valeur)
```

#### 📝 Initialisation : Donner une Valeur

```javascript
age = 25;               // Initialisation (première valeur)
console.log(age);       // 25
```

#### 📝 Déclaration + Initialisation

```javascript
let age = 25;           // Déclaration ET initialisation en une ligne
console.log(age);       // 25
```

#### 🔄 Réassignation

```javascript
let age = 25;           // Déclaration + initialisation
age = 26;               // Réassignation (nouvelle valeur)
age = 27;               // Réassignation encore
console.log(age);       // 27
```

#### ⚠️ Erreurs Courantes

```javascript
// Erreur 1 : Oublier de déclarer
score = 100;            // ❌ En strict mode : ReferenceError
                        // (fonctionne en non-strict mais créer variable globale!)

// Erreur 2 : Déclarer plusieurs fois
let name = "Ali";
let name = "Ahmed";     // ❌ SyntaxError: Identifier 'name' has already been declared

// Erreur 3 : Réassigner const
const PI = 3.14;
PI = 3.14159;          // ❌ TypeError: Assignment to constant variable

// Erreur 4 : Utiliser avant déclaration (avec let/const)
console.log(age);      // ❌ ReferenceError: Cannot access 'age' before initialization
let age = 25;
```

### 1.5 Portée (Scope) des Variables

#### 🌍 Portée Globale

```javascript
let userName = "Ali";   // Variable globale

function greet() {
  console.log(userName); // ✅ Accessible
}

greet();               // Affiche : Ali
console.log(userName); // ✅ Accessible aussi ici
```

#### 📦 Portée de Fonction

```javascript
function calculateAge() {
  let birthYear = 1995;      // Variable locale à la fonction
  let age = 2024 - birthYear;
  console.log(age);          // ✅ Accessible ici
}

calculateAge();              // Affiche : 29
console.log(birthYear);      // ❌ ReferenceError: birthYear is not defined
```

#### 🧱 Portée de Bloc (let et const)

```javascript
if (true) {
  let message = "Hello";     // Variable de bloc
  console.log(message);      // ✅ Accessible ici
}

console.log(message);        // ❌ ReferenceError: message is not defined
```

**Comparaison let vs var :**
```javascript
// Avec let (portée de bloc)
if (true) {
  let x = 10;
}
console.log(x);  // ❌ ReferenceError

// Avec var (portée de fonction/globale)
if (true) {
  var y = 10;
}
console.log(y);  // ✅ 10 (fuite de scope! problématique!)
```

#### 🎯 Règle d'Or

```
Utilise toujours la portée la plus restreinte possible !

const > let > var (jamais !)

1. Commence par const
2. Si tu dois modifier, utilise let
3. N'utilise JAMAIS var
```

---

## 2. Types de Données Primitifs

### 2.1 Qu'est-ce qu'un Type de Données ?

Le **type** détermine quelle sorte de valeur une variable contient.

```javascript
let age = 25;           // Type : Number (nombre)
let nom = "Ali";        // Type : String (texte)
let estActif = true;    // Type : Boolean (vrai/faux)
```

### 2.2 Les 7 Types Primitifs

JavaScript a **7 types primitifs** :

#### 📊 Liste Complète

| Type | Exemple | Description |
|------|---------|-------------|
| **Number** | `42`, `3.14`, `-10` | Nombres (entiers et décimaux) |
| **String** | `"Hello"`, `'Bonjour'` | Texte |
| **Boolean** | `true`, `false` | Vrai ou Faux |
| **Undefined** | `undefined` | Variable déclarée sans valeur |
| **Null** | `null` | Absence intentionnelle de valeur |
| **Symbol** | `Symbol("id")` | Identifiant unique (avancé) |
| **BigInt** | `123n` | Très grands entiers (avancé) |

### 2.3 Number : Les Nombres

#### 🔢 Tous les Nombres sont de Type Number

```javascript
let entier = 42;                // Entier
let decimal = 3.14159;          // Décimal (float)
let negatif = -10;              // Négatif
let zero = 0;                   // Zéro

console.log(typeof entier);     // "number"
console.log(typeof decimal);    // "number"
console.log(typeof negatif);    // "number"
```

**Contrairement à d'autres langages :**
```
Java : int, float, double, long...
C++ : int, float, double, short...
JavaScript : TOUT est "number" ! ✅ Simple !
```

#### ✨ Valeurs Spéciales

**Infinity (Infini)**
```javascript
let infini = 1 / 0;
console.log(infini);           // Infinity

let infiniNegatif = -1 / 0;
console.log(infiniNegatif);    // -Infinity
```

**NaN (Not a Number)**
```javascript
let resultat = "hello" / 2;
console.log(resultat);         // NaN

let invalide = Math.sqrt(-1);
console.log(invalide);         // NaN

// Vérifier si c'est NaN
console.log(isNaN(resultat));  // true
```

**⚠️ Bizarrerie : NaN est de type "number"**
```javascript
console.log(typeof NaN);       // "number" (bizarre mais vrai!)
```

#### 🔬 Précision des Nombres

```javascript
// JavaScript utilise des nombres à virgule flottante (IEEE 754)
console.log(0.1 + 0.2);        // 0.30000000000000004 (pas 0.3!)

// Solution pour l'argent :
let prix = 19.99;
let taxe = 0.20;
let total = Math.round((prix * (1 + taxe)) * 100) / 100;
console.log(total);            // 23.99
```

#### 📏 Limites des Nombres

```javascript
// Plus grand nombre sûr
console.log(Number.MAX_SAFE_INTEGER);  // 9007199254740991

// Plus petit nombre sûr
console.log(Number.MIN_SAFE_INTEGER);  // -9007199254740991

// Pour de TRÈS grands nombres : BigInt
let tresgrand = 9007199254740991n;     // n à la fin = BigInt
console.log(typeof tresgrand);          // "bigint"
```

### 2.4 String : Les Chaînes de Caractères

#### 📝 Créer des Strings

**3 façons de créer un string :**

```javascript
let simple = 'Bonjour';              // Guillemets simples
let double = "Hello";                 // Guillemets doubles
let template = `Salut`;               // Backticks (ES6+)

// Tous sont équivalents pour des strings simples
console.log(typeof simple);           // "string"
console.log(typeof double);           // "string"
console.log(typeof template);         // "string"
```

#### 🔤 Guillemets Simples vs Doubles

```javascript
// Les deux marchent pareil
let nom1 = 'Ali';
let nom2 = "Ali";

// Utile pour inclure l'autre type de guillemets
let phrase1 = "J'aime JavaScript";    // ✅ ' dans "
let phrase2 = 'Il dit "Bonjour"';     // ✅ " dans '

// Échappement avec \
let phrase3 = 'J\'aime JavaScript';   // ✅ Échapper '
let phrase4 = "Il dit \"Bonjour\"";   // ✅ Échapper "
```

#### ✨ Template Literals (Backticks) - ES6

**Super pouvoir des backticks :**

```javascript
// 1. Interpolation de variables
let nom = "Amina";
let age = 25;
let message = `Je m'appelle ${nom} et j'ai ${age} ans.`;
console.log(message);
// Affiche : Je m'appelle Amina et j'ai 25 ans.

// 2. Expressions JavaScript
let a = 10;
let b = 20;
console.log(`La somme est : ${a + b}`);
// Affiche : La somme est : 30

// 3. Multi-lignes (SANS \n)
let texte = `Ligne 1
Ligne 2
Ligne 3`;
console.log(texte);
// Affiche :
// Ligne 1
// Ligne 2
// Ligne 3
```

**Comparaison Avant/Après ES6 :**

```javascript
// ❌ Ancienne méthode (pénible)
let prenom = "Ali";
let ville = "Dakar";
let ancien = "Je m'appelle " + prenom + " et j'habite à " + ville + ".";

// ✅ Nouvelle méthode (moderne et lisible)
let moderne = `Je m'appelle ${prenom} et j'habite à ${ville}.`;
```

#### 📊 Propriété length et Indexation

```javascript
let mot = "JavaScript";

// Longueur du string
console.log(mot.length);              // 10

// Accès par index (commence à 0)
console.log(mot[0]);                  // "J"
console.log(mot[1]);                  // "a"
console.log(mot[9]);                  // "t"
console.log(mot[10]);                 // undefined (hors limite)

// Dernier caractère
console.log(mot[mot.length - 1]);     // "t"
```

#### 🔧 Méthodes de String Courantes

```javascript
let texte = "  Hello World  ";

// Majuscules/Minuscules
console.log(texte.toLowerCase());     // "  hello world  "
console.log(texte.toUpperCase());     // "  HELLO WORLD  "

// Enlever espaces début/fin
console.log(texte.trim());            // "Hello World"
console.log(texte.trimStart());       // "Hello World  "
console.log(texte.trimEnd());         // "  Hello World"

// Vérifier contenu
console.log(texte.includes("World")); // true
console.log(texte.includes("Bye"));   // false

// Remplacer
console.log(texte.replace("World", "JS")); // "  Hello JS  "
console.log(texte.replaceAll("l", "L"));   // "  HeLLo WorLd  " (ES2021)

// Découper
console.log(texte.split(" "));        // ["", "", "Hello", "World", "", ""]

// Sous-chaîne
let mot = "JavaScript";
console.log(mot.substring(0, 4));     // "Java"
console.log(mot.slice(4));            // "Script"
console.log(mot.slice(-6));           // "Script" (depuis la fin)
```

**⚠️ Les Strings sont IMMUTABLES**

```javascript
let texte = "Hello";
texte[0] = "J";              // ❌ Ne fonctionne PAS
console.log(texte);          // Toujours "Hello"

// Pour modifier, créer un nouveau string
texte = "J" + texte.slice(1);
console.log(texte);          // "Jello"
```

#### 🌍 Caractères Spéciaux

```javascript
// Retour à la ligne
let ligne = "Ligne 1\nLigne 2";
console.log(ligne);
// Affiche :
// Ligne 1
// Ligne 2

// Tabulation
let tab = "Col1\tCol2\tCol3";
console.log(tab);
// Affiche : Col1    Col2    Col3

// Backslash
let chemin = "C:\\Users\\Ali\\Documents";
console.log(chemin);         // C:\Users\Ali\Documents

// Guillemets
let citation = "Il dit \"Bonjour\"";
console.log(citation);       // Il dit "Bonjour"
```

**Liste des caractères d'échappement :**
```
\n  : Nouvelle ligne
\t  : Tabulation
\\  : Backslash
\'  : Guillemet simple
\"  : Guillemet double
\r  : Retour chariot
```

### 2.5 Boolean : Vrai ou Faux

#### ✅❌ Seulement 2 Valeurs

```javascript
let estConnecte = true;
let aDesErreurs = false;

console.log(typeof estConnecte);     // "boolean"
console.log(typeof aDesErreurs);     // "boolean"
```

#### 🔍 Opérateurs de Comparaison Produisent des Booleans

```javascript
let age = 25;

console.log(age > 18);               // true
console.log(age < 18);               // false
console.log(age === 25);             // true
console.log(age !== 30);             // true

// Stocker dans une variable
let estMajeur = age >= 18;
console.log(estMajeur);              // true
```

#### 💡 Utilisation dans les Conditions

```javascript
let estConnecte = true;

if (estConnecte) {
  console.log("Bienvenue !");        // ✅ Exécuté
} else {
  console.log("Connectez-vous");
}

// Négation avec !
if (!estConnecte) {
  console.log("Pas connecté");
} else {
  console.log("Connecté");           // ✅ Exécuté
}
```

### 2.6 Undefined : Non Défini

#### ❓ Qu'est-ce que Undefined ?

`undefined` = **Variable déclarée mais sans valeur**

```javascript
let nom;
console.log(nom);                    // undefined
console.log(typeof nom);             // "undefined"

// Fonction sans return
function test() {
  // pas de return
}
console.log(test());                 // undefined

// Propriété inexistante
let user = { age: 25 };
console.log(user.nom);               // undefined
```

#### ⚠️ Différence avec "not defined"

```javascript
let x;
console.log(x);                      // undefined (déclaré)

console.log(y);                      // ❌ ReferenceError: y is not defined (non déclaré)
```

### 2.7 Null : Valeur Vide Intentionnelle

#### ⭕ Qu'est-ce que Null ?

`null` = **Absence INTENTIONNELLE de valeur**

```javascript
let resultat = null;                 // Intentionnellement vide
console.log(resultat);               // null
console.log(typeof resultat);        // "object" (bug historique!)
```

#### 🆚 Undefined vs Null

```javascript
let a;                               // undefined (par défaut)
let b = null;                        // null (intentionnel)

// Comparaison
console.log(a == b);                 // true (valeur)
console.log(a === b);                // false (type différent)

console.log(typeof a);               // "undefined"
console.log(typeof b);               // "object" (bug JS!)
```

**Quand utiliser quoi ?**

```javascript
// undefined : JavaScript décide
let nom;                             // undefined automatiquement

// null : Tu décides
let resultatRecherche = null;        // Pas encore de résultat
// ... après recherche ...
resultatRecherche = { id: 1 };       // Maintenant on a un résultat
```

### 2.8 Symbol (Avancé)

```javascript
// Crée un identifiant unique
let id1 = Symbol("id");
let id2 = Symbol("id");

console.log(id1 === id2);            // false (uniques!)
console.log(typeof id1);             // "symbol"

// Usage : propriétés uniques d'objets (avancé)
let user = {
  [id1]: 12345
};
```

**💡 À savoir mais pas urgent pour débuter !**

### 2.9 BigInt (Très Grands Entiers)

```javascript
// Pour nombres > Number.MAX_SAFE_INTEGER
let grand = 9007199254740991n;       // n à la fin
console.log(typeof grand);           // "bigint"

// Opérations
let a = 100n;
let b = 200n;
console.log(a + b);                  // 300n

// ❌ Ne peut pas mélanger avec Number
let x = 10n + 5;                     // ❌ TypeError
let y = 10n + 5n;                    // ✅ 15n
```

**💡 Rarement utilisé pour débuter !**

### 2.10 typeof : Vérifier le Type

```javascript
console.log(typeof 42);              // "number"
console.log(typeof "hello");         // "string"
console.log(typeof true);            // "boolean"
console.log(typeof undefined);       // "undefined"
console.log(typeof null);            // "object" (bug historique!)
console.log(typeof Symbol());        // "symbol"
console.log(typeof 123n);            // "bigint"

// Variables
let age = 25;
console.log(typeof age);             // "number"

let nom = "Ali";
console.log(typeof nom);             // "string"
```

---

## 3. Opérateurs : Calculer et Comparer

### 3.1 Opérateurs Arithmétiques

#### ➕ Addition

```javascript
let a = 10;
let b = 5;
let somme = a + b;
console.log(somme);                  // 15

// Addition de strings (concaténation)
let prenom = "Ali";
let nom = "Diallo";
let nomComplet = prenom + " " + nom;
console.log(nomComplet);             // "Ali Diallo"

// Mélange number + string
console.log(10 + "5");               // "105" (string!)
console.log("Score: " + 100);        // "Score: 100"
```

#### ➖ Soustraction

```javascript
let x = 20;
let y = 8;
let difference = x - y;
console.log(difference);             // 12

// Avec strings : conversion automatique
console.log("10" - 5);               // 5 (convertit "10" en number)
console.log("20" - "5");             // 15
```

#### ✖️ Multiplication

```javascript
let largeur = 10;
let hauteur = 5;
let aire = largeur * hauteur;
console.log(aire);                   // 50

console.log(5 * 3);                  // 15
console.log("5" * 3);                // 15 (conversion auto)
```

#### ➗ Division

```javascript
let total = 100;
let parts = 4;
let parPart = total / parts;
console.log(parPart);                // 25

console.log(10 / 2);                 // 5
console.log(10 / 3);                 // 3.3333333333333335
console.log(10 / 0);                 // Infinity
```

#### 📐 Modulo (Reste de Division)

```javascript
console.log(10 % 3);                 // 1 (10 = 3*3 + 1)
console.log(15 % 4);                 // 3 (15 = 4*3 + 3)
console.log(20 % 5);                 // 0 (division exacte)

// Usage : nombres pairs/impairs
let nombre = 7;
if (nombre % 2 === 0) {
  console.log("Pair");
} else {
  console.log("Impair");             // ✅ Exécuté
}
```

#### 🔢 Exponentiation (Puissance)

```javascript
console.log(2 ** 3);                 // 8 (2³ = 2*2*2)
console.log(5 ** 2);                 // 25 (5² = 5*5)
console.log(10 ** 6);                // 1000000

// Équivalent à Math.pow()
console.log(Math.pow(2, 3));         // 8
```

#### 📊 Ordre des Opérations (PEMDAS)

```javascript
// Parenthèses > Exposants > Mult/Div > Add/Sous
console.log(2 + 3 * 4);              // 14 (pas 20!)
console.log((2 + 3) * 4);            // 20

console.log(10 + 5 * 2);             // 20 (5*2 d'abord)
console.log((10 + 5) * 2);           // 30

// Toujours utiliser des parenthèses pour clarifier
let prix = 100;
let taxe = 0.20;
let total = prix * (1 + taxe);       // Clair !
console.log(total);                  // 120
```

### 3.2 Opérateurs d'Assignation

#### = Assignation Simple

```javascript
let x = 10;                          // Assigner 10 à x
let y = x;                           // Copier x dans y
console.log(y);                      // 10
```

#### += Addition et Assignation

```javascript
let score = 100;
score = score + 50;                  // Ancienne façon
console.log(score);                  // 150

score = 100;
score += 50;                         // Raccourci !
console.log(score);                  // 150
```

#### Autres Opérateurs Composés

```javascript
let x = 10;

x += 5;    // x = x + 5      → 15
x -= 3;    // x = x - 3      → 12
x *= 2;    // x = x * 2      → 24
x /= 4;    // x = x / 4      → 6
x %= 5;    // x = x % 5      → 1
x **= 3;   // x = x ** 3     → 1

console.log(x);                      // 1
```

### 3.3 Incrémentation et Décrémentation

#### ++ Incrémenter (Ajouter 1)

```javascript
let compteur = 0;

// Post-incrémentation (x++)
let a = compteur++;                  // a = 0, puis compteur = 1
console.log(a);                      // 0
console.log(compteur);               // 1

// Pré-incrémentation (++x)
compteur = 0;
let b = ++compteur;                  // compteur = 1, puis b = 1
console.log(b);                      // 1
console.log(compteur);               // 1
```

#### -- Décrémenter (Retirer 1)

```javascript
let vies = 3;

vies--;                              // Post-décrémentation
console.log(vies);                   // 2

--vies;                              // Pré-décrémentation
console.log(vies);                   // 1
```

**💡 Dans la plupart des cas, pas de différence :**

```javascript
// Ces deux sont équivalents dans un for
for (let i = 0; i < 10; i++) { }
for (let i = 0; i < 10; ++i) { }

// Usage typique
let compteur = 0;
compteur++;                          // Simple et clair
```

### 3.4 Opérateurs de Comparaison

#### == vs === (IMPORTANT!)

**== : Égalité avec conversion de type**
```javascript
console.log(5 == 5);                 // true
console.log(5 == "5");               // true (convertit "5" en 5)
console.log(true == 1);              // true (true converti en 1)
console.log(false == 0);             // true (false converti en 0)
console.log(null == undefined);      // true (cas spécial)
```

**=== : Égalité STRICTE (type + valeur)**
```javascript
console.log(5 === 5);                // true
console.log(5 === "5");              // false (types différents)
console.log(true === 1);             // false
console.log(false === 0);            // false
console.log(null === undefined);     // false
```

**🎯 Règle d'Or : TOUJOURS utiliser === et !==**

```javascript
// ❌ Mauvais (bugs potentiels)
if (age == 18) { }
if (nom == "Ali") { }

// ✅ Bon (comparaison stricte)
if (age === 18) { }
if (nom === "Ali") { }
```

#### != vs !== (Inégalité)

```javascript
// != : Inégalité avec conversion
console.log(5 != "5");               // false
console.log(5 != 10);                // true

// !== : Inégalité stricte
console.log(5 !== "5");              // true (types différents)
console.log(5 !== 10);               // true
```

#### > < >= <= (Comparaisons)

```javascript
let age = 25;

console.log(age > 18);               // true
console.log(age >= 25);              // true
console.log(age < 30);               // true
console.log(age <= 20);              // false

// Avec strings (ordre alphabétique)
console.log("a" < "b");              // true
console.log("Ali" < "Amir");         // true
console.log("Z" < "a");              // true (majuscules < minuscules)
```

### 3.5 Opérateurs Logiques

#### && (AND - ET)

**Vrai seulement si TOUS sont vrais**

```javascript
console.log(true && true);           // true
console.log(true && false);          // false
console.log(false && true);          // false
console.log(false && false);         // false

// Exemple pratique
let age = 25;
let aPermis = true;

if (age >= 18 && aPermis) {
  console.log("Peut conduire");      // ✅ Exécuté
}

// Plusieurs conditions
let estConnecte = true;
let estAdmin = true;
let aAcces = false;

if (estConnecte && estAdmin && aAcces) {
  console.log("Accès autorisé");
} else {
  console.log("Accès refusé");       // ✅ Exécuté (aAcces = false)
}
```

#### || (OR - OU)

**Vrai si AU MOINS UN est vrai**

```javascript
console.log(true || true);           // true
console.log(true || false);          // true
console.log(false || true);          // true
console.log(false || false);         // false

// Exemple pratique
let estWeekend = false;
let estVacances = true;

if (estWeekend || estVacances) {
  console.log("Pas d'école !");      // ✅ Exécuté
}

// Valeur par défaut
let nom = "";
let nomAffiche = nom || "Anonyme";
console.log(nomAffiche);             // "Anonyme"
```

#### ! (NOT - Négation)

**Inverse la valeur booléenne**

```javascript
console.log(!true);                  // false
console.log(!false);                 // true

let estConnecte = false;
if (!estConnecte) {
  console.log("Veuillez vous connecter"); // ✅ Exécuté
}

// Double négation (convertir en boolean)
console.log(!!"Hello");              // true
console.log(!!0);                    // false
console.log(!!"");                   // false
```

#### 📊 Table de Vérité Complète

```javascript
// AND (&&)
true  && true   → true
true  && false  → false
false && true   → false
false && false  → false

// OR (||)
true  || true   → true
true  || false  → true
false || true   → true
false || false  → false

// NOT (!)
!true   → false
!false  → true
```

#### 💡 Court-Circuit (Short-Circuit)

```javascript
// && s'arrête au premier false
console.log(false && console.log("Pas exécuté")); // false (pas de log)

// || s'arrête au premier true
console.log(true || console.log("Pas exécuté"));  // true (pas de log)

// Usage pratique
let user = null;
let userName = user && user.name;    // undefined (pas d'erreur)

let defaultName = userName || "Invité";
console.log(defaultName);            // "Invité"
```

### 3.6 Opérateur Ternaire (Conditionnel)

**Syntaxe : `condition ? valeurSiVrai : valeurSiFaux`**

```javascript
// If/else traditionnel
let age = 20;
let statut;
if (age >= 18) {
  statut = "Majeur";
} else {
  statut = "Mineur";
}

// Opérateur ternaire (plus court)
let statut2 = age >= 18 ? "Majeur" : "Mineur";
console.log(statut2);                // "Majeur"

// Exemples
let score = 85;
let resultat = score >= 50 ? "Réussi" : "Échoué";
console.log(resultat);               // "Réussi"

let estConnecte = true;
let message = estConnecte ? "Bienvenue" : "Connectez-vous";
console.log(message);                // "Bienvenue"

// Ternaires imbriqués (éviter!)
let note = 75;
let mention = note >= 90 ? "Excellent" :
              note >= 70 ? "Bien" :
              note >= 50 ? "Passable" : "Insuffisant";
console.log(mention);                // "Bien"
```

**⚠️ Conseil : Utilise pour des cas simples seulement !**

### 3.7 Opérateurs Modernes (ES2020+)

#### ?? Nullish Coalescing

**Retourne la valeur de droite si gauche est `null` ou `undefined`**

```javascript
let nom = null;
console.log(nom ?? "Anonyme");       // "Anonyme"

let age = 0;
console.log(age ?? 18);              // 0 (0 n'est ni null ni undefined)

// Différence avec ||
console.log(0 || 100);               // 100 (0 est falsy)
console.log(0 ?? 100);               // 0 (0 n'est pas null/undefined)

console.log("" || "Défaut");         // "Défaut" (string vide est falsy)
console.log("" ?? "Défaut");         // "" (string vide n'est pas null/undefined)
```

#### ?. Optional Chaining

**Évite les erreurs sur propriétés inexistantes**

```javascript
let user = null;

// ❌ Sans optional chaining (erreur)
// console.log(user.name);           // TypeError

// ✅ Avec optional chaining (pas d'erreur)
console.log(user?.name);             // undefined

// Objet avec propriétés
let person = {
  nom: "Ali",
  adresse: {
    ville: "Dakar"
  }
};

console.log(person?.adresse?.ville);      // "Dakar"
console.log(person?.adresse?.pays);       // undefined
console.log(person?.contact?.email);      // undefined (pas d'erreur)

// Avec fonctions
let obj = {};
console.log(obj.methode?.());             // undefined (pas d'erreur)
```

---

## 4. Strings : Manipuler du Texte

### 4.1 Concaténation

#### + Opérateur Plus

```javascript
let prenom = "Amina";
let nom = "Sow";

// Concaténation basique
let nomComplet = prenom + " " + nom;
console.log(nomComplet);             // "Amina Sow"

// Avec variables et texte
let age = 25;
let phrase = prenom + " a " + age + " ans.";
console.log(phrase);                 // "Amina a 25 ans."

// Problème : vite illisible
let message = "Bonjour " + prenom + ", vous avez " + (age + 1) + " ans l'année prochaine.";
```

#### ✨ Template Literals (Recommandé)

```javascript
let prenom = "Amina";
let nom = "Sow";
let age = 25;

// Plus lisible et puissant
let message = `Bonjour ${prenom} ${nom}, vous avez ${age} ans.`;
console.log(message);

// Expressions JavaScript
let prix = 100;
let taxe = 0.20;
console.log(`Prix TTC: ${prix * (1 + taxe)}€`);  // "Prix TTC: 120€"

// Multi-lignes
let email = `
Bonjour ${prenom},

Merci pour votre inscription.

Cordialement,
L'équipe
`;
console.log(email);
```

### 4.2 Méthodes de String Essentielles

#### 📏 Longueur et Accès

```javascript
let mot = "JavaScript";

console.log(mot.length);             // 10
console.log(mot[0]);                 // "J"
console.log(mot[mot.length - 1]);    // "t"

// charAt (alternative)
console.log(mot.charAt(0));          // "J"
console.log(mot.charAt(100));        // "" (vide, pas undefined)
```

#### 🔍 Recherche

```javascript
let texte = "J'adore JavaScript et JavaScript est génial";

// includes (contient?)
console.log(texte.includes("JavaScript"));    // true
console.log(texte.includes("Python"));        // false

// indexOf (position première occurrence)
console.log(texte.indexOf("JavaScript"));     // 8
console.log(texte.indexOf("Python"));         // -1 (pas trouvé)

// lastIndexOf (position dernière occurrence)
console.log(texte.lastIndexOf("JavaScript")); // 25

// startsWith et endsWith
console.log(texte.startsWith("J'adore"));     // true
console.log(texte.endsWith("génial"));        // true
```

#### ✂️ Extraction

```javascript
let phrase = "Hello World";

// slice (index début, index fin)
console.log(phrase.slice(0, 5));     // "Hello"
console.log(phrase.slice(6));        // "World"
console.log(phrase.slice(-5));       // "World" (depuis la fin)

// substring (similaire mais pas de négatif)
console.log(phrase.substring(0, 5)); // "Hello"
console.log(phrase.substring(6));    // "World"

// substr (déprécié, ne pas utiliser)
```

#### 🔄 Remplacement

```javascript
let texte = "J'aime Python et Python est cool";

// replace (remplace la PREMIÈRE occurrence)
console.log(texte.replace("Python", "JavaScript"));
// "J'aime JavaScript et Python est cool"

// replaceAll (remplace TOUTES les occurrences) - ES2021
console.log(texte.replaceAll("Python", "JavaScript"));
// "J'aime JavaScript et JavaScript est cool"

// Avec regex (avancé)
console.log(texte.replace(/Python/g, "JavaScript"));
// "J'aime JavaScript et JavaScript est cool"
```

#### ✂️ Découpage

```javascript
let csv = "Ali,25,Dakar";

// split (convertir string en array)
let parties = csv.split(",");
console.log(parties);                // ["Ali", "25", "Dakar"]
console.log(parties[0]);             // "Ali"
console.log(parties[1]);             // "25"

// Découper sur espaces
let phrase = "Bonjour tout le monde";
let mots = phrase.split(" ");
console.log(mots);                   // ["Bonjour", "tout", "le", "monde"]

// Limite
let texte = "a-b-c-d-e";
console.log(texte.split("-", 2));    // ["a", "b"]
```

#### 🔤 Casse (Majuscules/Minuscules)

```javascript
let texte = "JavaScript";

console.log(texte.toLowerCase());    // "javascript"
console.log(texte.toUpperCase());    // "JAVASCRIPT"

// Usage : comparaison insensible à la casse
let input = "HELLO";
if (input.toLowerCase() === "hello") {
  console.log("Match !");            // ✅ Exécuté
}
```

#### ✨ Nettoyage

```javascript
let texte = "   Hello World   ";

console.log(texte.trim());           // "Hello World"
console.log(texte.trimStart());      // "Hello World   "
console.log(texte.trimEnd());        // "   Hello World"

// Usage : formulaires
let username = "  Ali  ";
username = username.trim();
console.log(username);               // "Ali"
```

#### 🔁 Répétition

```javascript
let motif = "* ";
console.log(motif.repeat(5));        // "* * * * * "

let ligne = "-";
console.log(ligne.repeat(20));       // "--------------------"

// Créer un padding
let nom = "Ali";
console.log(nom + " ".repeat(10 - nom.length) + "100");
// "Ali       100"
```

#### 📐 Padding (Remplissage)

```javascript
let nombre = "5";

// padStart (remplir au début)
console.log(nombre.padStart(3, "0")); // "005"
console.log(nombre.padStart(5, "*")); // "****5"

// padEnd (remplir à la fin)
console.log(nombre.padEnd(3, "0"));   // "500"

// Usage : formater des nombres
let id = "42";
console.log(id.padStart(6, "0"));     // "000042"
```

### 4.3 Exercices Strings

#### 🟢 Exercice 1 : Compter les Voyelles

```javascript
// Écris une fonction qui compte les voyelles dans un mot
function compterVoyelles(mot) {
  // TON CODE ICI
}

console.log(compterVoyelles("JavaScript")); // Devrait afficher : 3
console.log(compterVoyelles("Hello"));      // Devrait afficher : 2
```

**💡 Solution :**
```javascript
function compterVoyelles(mot) {
  const voyelles = "aeiouAEIOU";
  let compteur = 0;
  
  for (let i = 0; i < mot.length; i++) {
    if (voyelles.includes(mot[i])) {
      compteur++;
    }
  }
  
  return compteur;
}
```

#### 🟡 Exercice 2 : Inverser un String

```javascript
// Écris une fonction qui inverse un string
function inverser(texte) {
  // TON CODE ICI
}

console.log(inverser("Hello"));  // "olleH"
console.log(inverser("Ali"));    // "ilA"
```

**💡 Solution :**
```javascript
function inverser(texte) {
  return texte.split("").reverse().join("");
}

// Ou avec une boucle
function inverserBoucle(texte) {
  let resultat = "";
  for (let i = texte.length - 1; i >= 0; i--) {
    resultat += texte[i];
  }
  return resultat;
}
```

#### 🔴 Exercice 3 : Palindrome

```javascript
// Vérifie si un mot est un palindrome (se lit pareil dans les 2 sens)
function estPalindrome(mot) {
  // TON CODE ICI
}

console.log(estPalindrome("kayak"));  // true
console.log(estPalindrome("radar"));  // true
console.log(estPalindrome("hello"));  // false
```

**💡 Solution :**
```javascript
function estPalindrome(mot) {
  const motNettoye = mot.toLowerCase();
  const motInverse = motNettoye.split("").reverse().join("");
  return motNettoye === motInverse;
}
```

---

## 5. Type Conversion et Coercion

### 5.1 Conversion Explicite (Type Casting)

#### 🔢 Convertir en Number

```javascript
// String → Number
console.log(Number("123"));          // 123
console.log(Number("3.14"));         // 3.14
console.log(Number("hello"));        // NaN

// Boolean → Number
console.log(Number(true));           // 1
console.log(Number(false));          // 0

// Alternative : opérateur unaire +
console.log(+"123");                 // 123
console.log(+"3.14");                // 3.14

// parseInt (string → entier)
console.log(parseInt("123"));        // 123
console.log(parseInt("123.99"));     // 123 (ignore les décimales)
console.log(parseInt("123abc"));     // 123 (s'arrête aux lettres)

// parseFloat (string → float)
console.log(parseFloat("3.14"));     // 3.14
console.log(parseFloat("3.14abc"));  // 3.14
```

#### 📝 Convertir en String

```javascript
// Number → String
console.log(String(123));            // "123"
console.log(String(3.14));           // "3.14"

// Boolean → String
console.log(String(true));           // "true"
console.log(String(false));          // "false"

// Alternative : toString()
console.log((123).toString());       // "123"
console.log(true.toString());        // "true"

// Alternative : concaténation avec ""
console.log(123 + "");               // "123"
console.log(true + "");              // "true"
```

#### ✅ Convertir en Boolean

```javascript
// Explicit conversion
console.log(Boolean(1));             // true
console.log(Boolean(0));             // false
console.log(Boolean("hello"));       // true
console.log(Boolean(""));            // false

// Alternative : double négation !!
console.log(!!"hello");              // true
console.log(!!0);                    // false
console.log(!!"");                   // false
console.log(!!null);                 // false
```

### 5.2 Coercion Implicite (Type Coercion)

**JavaScript convertit automatiquement les types dans certaines situations**

#### 🔀 String + Number → String

```javascript
console.log("5" + 3);                // "53" (concaténation)
console.log("Hello" + 5);            // "Hello5"
console.log(5 + "5");                // "55"

// Plusieurs additions
console.log(1 + 2 + "3");            // "33" (1+2=3, puis "3"+"3")
console.log("1" + 2 + 3);            // "123" ("1"+2="12", "12"+3="123")
```

#### 🔢 String - Number → Number

```javascript
console.log("10" - 5);               // 5
console.log("10" * "2");             // 20
console.log("10" / "2");             // 5
console.log("10" % "3");             // 1

// ⚠️ SAUF pour + qui concatène
console.log("10" + 5);               // "105" (concaténation)
```

#### ✅ Valeurs dans Conditions

```javascript
// Conversion automatique en boolean
if ("hello") {
  console.log("Exécuté");            // ✅ "hello" est truthy
}

if (0) {
  console.log("Pas exécuté");        // ❌ 0 est falsy
}

// Exemples
console.log(5 == "5");               // true (coercion)
console.log(5 === "5");              // false (pas de coercion)

console.log(true == 1);              // true (coercion)
console.log(true === 1);             // false (pas de coercion)
```

### 5.3 Pièges de la Coercion

```javascript
// Piège 1 : Addition vs Concaténation
console.log(1 + 2 + "3");            // "33"
console.log("1" + 2 + 3);            // "123"
console.log("1" + (2 + 3));          // "15" (parenthèses!)

// Piège 2 : Comparaisons étranges
console.log("" == 0);                // true (WTF?)
console.log("" === 0);               // false
console.log(null == undefined);      // true
console.log(null === undefined);     // false

// Piège 3 : NaN
console.log(NaN == NaN);             // false (bizarre!)
console.log(NaN === NaN);            // false
console.log(isNaN(NaN));             // true (bonne méthode)

// Piège 4 : typeof null
console.log(typeof null);            // "object" (bug historique)
```

---

## 6. Truthy et Falsy : Valeurs Booléennes

### 6.1 Valeurs Falsy (Fausses)

**Il y a SEULEMENT 6 valeurs falsy en JavaScript :**

```javascript
// Les 6 valeurs falsy :
false        // Boolean false
0            // Number zero
""           // String vide
null         // Null
undefined    // Undefined
NaN          // Not a Number

// Test
if (false) { console.log("Non"); }       // Pas exécuté
if (0) { console.log("Non"); }           // Pas exécuté
if ("") { console.log("Non"); }          // Pas exécuté
if (null) { console.log("Non"); }        // Pas exécuté
if (undefined) { console.log("Non"); }   // Pas exécuté
if (NaN) { console.log("Non"); }         // Pas exécuté
```

### 6.2 Valeurs Truthy (Vraies)

**TOUT LE RESTE est truthy !**

```javascript
// Nombres (sauf 0)
if (1) { console.log("Truthy"); }        // ✅
if (-1) { console.log("Truthy"); }       // ✅
if (3.14) { console.log("Truthy"); }     // ✅

// Strings (sauf "")
if ("hello") { console.log("Truthy"); }  // ✅
if ("0") { console.log("Truthy"); }      // ✅ (string "0" pas number 0)
if (" ") { console.log("Truthy"); }      // ✅ (espace)

// Objets et Arrays (toujours truthy!)
if ({}) { console.log("Truthy"); }       // ✅
if ([]) { console.log("Truthy"); }       // ✅

// Fonctions
if (function() {}) { console.log("Truthy"); } // ✅
```

### 6.3 Utilisation Pratique

#### ✅ Valeurs par Défaut

```javascript
function saluer(nom) {
  nom = nom || "Invité";               // Si nom est falsy, utiliser "Invité"
  console.log(`Bonjour ${nom}`);
}

saluer("Ali");                         // "Bonjour Ali"
saluer();                              // "Bonjour Invité"

// ⚠️ Attention avec 0 ou ""
function afficherScore(score) {
  score = score || 100;                // Bug si score = 0 !
  console.log(score);
}

afficherScore(0);                      // 100 (pas 0 !)

// Solution : nullish coalescing (??)
function afficherScoreFix(score) {
  score = score ?? 100;                // OK si score = 0
  console.log(score);
}

afficherScoreFix(0);                   // 0 ✅
afficherScoreFix(null);                // 100 ✅
```

#### ✅ Vérification d'Existence

```javascript
let user = { nom: "Ali" };

// Vérifier propriété
if (user.nom) {
  console.log("Nom existe");           // ✅
}

// Vérifier objet
if (user) {
  console.log("User existe");          // ✅
}

// Avec optional chaining
let ville = user?.adresse?.ville;
if (ville) {
  console.log(ville);
}
```

---

## 7. Template Literals : Strings Modernes

### 7.1 Syntaxe de Base

```javascript
// Backticks : ``
let nom = "Ali";
let message = `Bonjour ${nom}`;
console.log(message);                  // "Bonjour Ali"

// Expressions JavaScript
let a = 5;
let b = 10;
console.log(`${a} + ${b} = ${a + b}`); // "5 + 10 = 15"

// Appels de fonction
function getNom() {
  return "Amina";
}
console.log(`Bonjour ${getNom()}`);    // "Bonjour Amina"
```

### 7.2 Multi-lignes

```javascript
// Ancien (concaténation)
let ancien = "Ligne 1\n" +
             "Ligne 2\n" +
             "Ligne 3";

// Moderne (template literal)
let moderne = `
Ligne 1
Ligne 2
Ligne 3
`;

console.log(moderne);
```

### 7.3 Expressions Complexes

```javascript
let user = {
  nom: "Ali",
  age: 25,
  ville: "Dakar"
};

let profil = `
Nom: ${user.nom}
Âge: ${user.age}
Statut: ${user.age >= 18 ? "Majeur" : "Mineur"}
Ville: ${user.ville.toUpperCase()}
`;

console.log(profil);
```

### 7.4 Tagged Templates (Avancé)

```javascript
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) => {
    return `${result}${str}<strong>${values[i] || ""}</strong>`;
  }, "");
}

let nom = "Ali";
let age = 25;
let html = highlight`Bonjour ${nom}, vous avez ${age} ans.`;
console.log(html);
// "Bonjour <strong>Ali</strong>, vous avez <strong>25</strong> ans."
```

---

## 8. Premier Projet : Calculatrice Interactive

### 8.1 Objectif du Projet

Créer une calculatrice simple qui :
- Additionne, soustrait, multiplie, divise
- Gère les erreurs (division par zéro)
- Interface utilisateur basique

### 8.2 Structure des Fichiers

```bash
calculatrice/
├── index.html
├── style.css
└── script.js
```

### 8.3 Code HTML (index.html)

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculatrice JavaScript</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>🧮 Calculatrice</h1>
    
    <div class="calculator">
      <input type="number" id="num1" placeholder="Nombre 1" />
      
      <select id="operation">
        <option value="+">+ Addition</option>
        <option value="-">- Soustraction</option>
        <option value="*">× Multiplication</option>
        <option value="/">÷ Division</option>
        <option value="%">% Modulo</option>
        <option value="**">^ Puissance</option>
      </select>
      
      <input type="number" id="num2" placeholder="Nombre 2" />
      
      <button id="calculateBtn">Calculer</button>
      <button id="clearBtn">Effacer</button>
      
      <div id="result"></div>
    </div>
    
    <div class="history">
      <h2>Historique</h2>
      <ul id="historyList"></ul>
    </div>
  </div>
  
  <script src="script.js"></script>
</body>
</html>
```

### 8.4 Code CSS (style.css)

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.container {
  background: white;
  border-radius: 20px;
  padding: 40px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  max-width: 500px;
  width: 100%;
}

h1 {
  text-align: center;
  color: #667eea;
  margin-bottom: 30px;
  font-size: 2.5em;
}

.calculator {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

input, select, button {
  padding: 15px;
  font-size: 1.1em;
  border: 2px solid #e0e0e0;
  border-radius: 10px;
  outline: none;
  transition: all 0.3s;
}

input:focus, select:focus {
  border-color: #667eea;
  transform: scale(1.02);
}

button {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  cursor: pointer;
  font-weight: bold;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
}

button:active {
  transform: translateY(0);
}

#clearBtn {
  background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
}

#result {
  margin-top: 20px;
  padding: 20px;
  background: #f5f5f5;
  border-radius: 10px;
  text-align: center;
  font-size: 1.5em;
  font-weight: bold;
  min-height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}

#result.success {
  background: #d4edda;
  color: #155724;
}

#result.error {
  background: #f8d7da;
  color: #721c24;
}

.history {
  margin-top: 30px;
}

.history h2 {
  color: #667eea;
  margin-bottom: 15px;
}

#historyList {
  list-style: none;
  max-height: 200px;
  overflow-y: auto;
}

#historyList li {
  padding: 10px;
  background: #f5f5f5;
  margin-bottom: 5px;
  border-radius: 5px;
  font-family: 'Courier New', monospace;
}
```

### 8.5 Code JavaScript (script.js)

```javascript
// ============================================
// CALCULATRICE JAVASCRIPT
// Description : Calculatrice avec historique
// ============================================

// Sélection des éléments DOM
const num1Input = document.getElementById('num1');
const num2Input = document.getElementById('num2');
const operationSelect = document.getElementById('operation');
const calculateBtn = document.getElementById('calculateBtn');
const clearBtn = document.getElementById('clearBtn');
const resultDiv = document.getElementById('result');
const historyList = document.getElementById('historyList');

// Tableau pour stocker l'historique
let history = [];

// Fonction principale de calcul
function calculate() {
  // Récupérer les valeurs
  const num1 = parseFloat(num1Input.value);
  const num2 = parseFloat(num2Input.value);
  const operation = operationSelect.value;
  
  // Valider les entrées
  if (isNaN(num1) || isNaN(num2)) {
    showResult('Erreur : Entrez des nombres valides', 'error');
    return;
  }
  
  // Effectuer le calcul selon l'opération
  let result;
  let operationSymbol;
  
  switch (operation) {
    case '+':
      result = num1 + num2;
      operationSymbol = '+';
      break;
    case '-':
      result = num1 - num2;
      operationSymbol = '-';
      break;
    case '*':
      result = num1 * num2;
      operationSymbol = '×';
      break;
    case '/':
      if (num2 === 0) {
        showResult('Erreur : Division par zéro impossible', 'error');
        return;
      }
      result = num1 / num2;
      operationSymbol = '÷';
      break;
    case '%':
      result = num1 % num2;
      operationSymbol = '%';
      break;
    case '**':
      result = num1 ** num2;
      operationSymbol = '^';
      break;
    default:
      showResult('Erreur : Opération invalide', 'error');
      return;
  }
  
  // Arrondir à 2 décimales
  result = Math.round(result * 100) / 100;
  
  // Afficher le résultat
  showResult(`${num1} ${operationSymbol} ${num2} = ${result}`, 'success');
  
  // Ajouter à l'historique
  addToHistory(`${num1} ${operationSymbol} ${num2} = ${result}`);
}

// Afficher le résultat
function showResult(message, type) {
  resultDiv.textContent = message;
  resultDiv.className = type;
}

// Ajouter à l'historique
function addToHistory(calculation) {
  history.unshift(calculation); // Ajouter au début
  
  // Limiter à 10 entrées
  if (history.length > 10) {
    history.pop();
  }
  
  // Mettre à jour l'affichage
  updateHistoryDisplay();
}

// Mettre à jour l'affichage de l'historique
function updateHistoryDisplay() {
  historyList.innerHTML = '';
  
  history.forEach(item => {
    const li = document.createElement('li');
    li.textContent = item;
    historyList.appendChild(li);
  });
}

// Effacer
function clear() {
  num1Input.value = '';
  num2Input.value = '';
  resultDiv.textContent = '';
  resultDiv.className = '';
}

// Event listeners
calculateBtn.addEventListener('click', calculate);
clearBtn.addEventListener('click', clear);

// Calculer avec la touche Enter
num1Input.addEventListener('keypress', (e) => {
  if (e.key === 'Enter') calculate();
});

num2Input.addEventListener('keypress', (e) => {
  if (e.key === 'Enter') calculate();
});

// Message de bienvenue
console.log('🧮 Calculatrice JavaScript chargée avec succès !');
console.log('Créée avec ❤️ par Learning Schooling Foundation');
```

### 8.6 Fonctionnalités

✅ **Ce que fait la calculatrice :**
- Addition, soustraction, multiplication, division
- Modulo et puissance
- Validation des entrées
- Gestion division par zéro
- Historique des 10 derniers calculs
- Interface responsive
- Calcul avec Enter

### 8.7 Améliorations Possibles

🚀 **Défis pour toi :**

1. **Ajouter une racine carrée**
   ```javascript
   case 'sqrt':
     result = Math.sqrt(num1);
     break;
   ```

2. **Sauvegarder l'historique dans LocalStorage**
   ```javascript
   localStorage.setItem('history', JSON.stringify(history));
   ```

3. **Thème sombre/clair**
4. **Calculs en chaîne (utiliser le résultat précédent)**
5. **Clavier numérique visuel**

---

## 🎯 Checkpoint Partie 2

### ✅ Ce que tu as accompli

Si tu as suivi jusqu'ici, tu maîtrises :

- [x] Variables (let, const, var)
- [x] Types primitifs (Number, String, Boolean, etc.)
- [x] Opérateurs (arithmétiques, logiques, comparaison)
- [x] Manipulation de strings
- [x] Type conversion et coercion
- [x] Truthy et Falsy
- [x] Template literals
- [x] Premier projet fonctionnel !

### 🎓 Niveau actuel : Fondamentaux Solides

```
Progression : ████████████░░░░░░░░ 30%

Prochaine étape : Structures de contrôle et fonctions !
```

---

## 📝 Exercices de Révision

### 🟢 Exercice 1 : Variables et Types

```javascript
// Déclare les variables suivantes :
// - prénom (string) : ton prénom
// - age (number) : ton âge
// - estEtudiant (boolean) : true
// - ville (string) : ta ville
// Affiche tout dans un message avec template literal

// TON CODE ICI
```

### 🟡 Exercice 2 : Calculs

```javascript
// Crée un programme qui calcule :
// - L'aire d'un rectangle (longueur × largeur)
// - Le périmètre (2 × (longueur + largeur))
let longueur = 10;
let largeur = 5;

// TON CODE ICI
```

### 🔴 Exercice 3 : Validation Email

```javascript
// Écris une fonction qui vérifie si un email est valide
// Règles simples :
// - Doit contenir @
// - Doit contenir un point après le @
// - Ne doit pas être vide

function validerEmail(email) {
  // TON CODE ICI
}

console.log(validerEmail("ali@example.com"));  // true
console.log(validerEmail("invalide"));         // false
```

---

## 🚀 Suite dans la Partie 3

**Dans la Partie 3, tu vas apprendre :**

- Structures conditionnelles (if, else if, else, switch)
- Boucles (for, while, do-while)
- Fonctions (déclaration, expression, arrow)
- Portée et closures
- Arrays (tableaux)
- Méthodes d'arrays
- Projets : Todo List, Quiz, Jeux

**Durée estimée Partie 3 : 20-25 heures**

---

## 💎 Message de Motivation

Tu viens de franchir une étape CRUCIALE ! Les fondamentaux que tu viens d'apprendre sont utilisés dans CHAQUE programme JavaScript.

**Ne sous-estime pas ce que tu sais déjà :**
- Variables : La base de TOUT programme
- Types : Comprendre les données
- Opérateurs : Manipuler les données
- Strings : 80% du web

**Continue comme ça !** 🚀

Tu es maintenant prêt pour la suite : structures de contrôle et fonctions, qui vont vraiment donner vie à tes programmes !

---

*Ce guide est créé avec ❤️ par la Learning Schooling Foundation*  
*100% Gratuit • Pour Tous • À Jamais*  
*Licence : Creative Commons BY-NC 4.0*

**🎉 FIN DE LA PARTIE 2 🎉**

---
