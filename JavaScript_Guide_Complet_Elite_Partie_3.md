# 🚀 JavaScript : Guide Complet du Débutant à l'Expert - Partie 3

## 📚 Structures de Contrôle, Fonctions et Arrays

> **"La vraie programmation commence quand tu maîtrises les conditions, les boucles et les fonctions !"**

**Par la Learning Schooling Foundation**  
*Niveau : MIT • Google • Berkeley • Stanford*

---

## 📖 Table des Matières - Partie 3

1. **Structures Conditionnelles (if, else, switch)**
2. **Boucles (for, while, do-while)**
3. **Fonctions : Créer du Code Réutilisable**
4. **Portée et Closures**
5. **Arrays (Tableaux) : Gérer des Listes**
6. **Méthodes d'Arrays Essentielles**
7. **Itération Avancée (forEach, map, filter)**
8. **Projet 1 : Todo List Interactive**
9. **Projet 2 : Jeu du Pendu**
10. **Projet 3 : Générateur de Mots de Passe**

---

## 1. Structures Conditionnelles

### 1.1 if : Exécuter du Code Conditionnellement

#### 🎯 Syntaxe de Base

```javascript
if (condition) {
  // Code exécuté si condition est vraie
}
```

**Exemple simple :**
```javascript
let age = 20;

if (age >= 18) {
  console.log("Tu es majeur !");  // ✅ Exécuté
}
```

#### 🔀 if...else : Deux Chemins Possibles

```javascript
if (condition) {
  // Code si vrai
} else {
  // Code si faux
}
```

**Exemple :**
```javascript
let age = 15;

if (age >= 18) {
  console.log("Tu es majeur");
} else {
  console.log("Tu es mineur");     // ✅ Exécuté
}
```

#### 🔀🔀 if...else if...else : Plusieurs Conditions

```javascript
let note = 85;

if (note >= 90) {
  console.log("Excellent !");
} else if (note >= 80) {
  console.log("Très bien !");      // ✅ Exécuté
} else if (note >= 70) {
  console.log("Bien");
} else if (note >= 50) {
  console.log("Passable");
} else {
  console.log("Insuffisant");
}
```

#### 💡 Conditions Multiples (AND, OR)

```javascript
let age = 25;
let aPermis = true;

// AND (&&) : TOUTES les conditions doivent être vraies
if (age >= 18 && aPermis) {
  console.log("Tu peux conduire !");  // ✅ Exécuté
}

// OR (||) : AU MOINS UNE condition doit être vraie
let estWeekend = false;
let estVacances = true;

if (estWeekend || estVacances) {
  console.log("Pas d'école !");    // ✅ Exécuté
}

// Combinaison complexe
let estEtudiant = true;
let revenu = 15000;
let credits = 30;

if (estEtudiant && (revenu < 20000 || credits >= 24)) {
  console.log("Éligible à la bourse");  // ✅ Exécuté
}
```

#### ⚠️ Erreurs Courantes

```javascript
// ❌ ERREUR : Assignation au lieu de comparaison
let x = 5;
if (x = 10) {  // Assigne 10 à x, toujours vrai !
  console.log("Exécuté");  // ⚠️ Bug !
}

// ✅ CORRECT : Comparaison
if (x === 10) {
  console.log("x vaut 10");
}

// ❌ ERREUR : Oublier les accolades (dangereux)
if (age >= 18)
  console.log("Majeur");
  console.log("Bienvenue");  // ⚠️ Toujours exécuté !

// ✅ CORRECT : Toujours utiliser les accolades
if (age >= 18) {
  console.log("Majeur");
  console.log("Bienvenue");
}
```

### 1.2 Opérateur Ternaire : if...else Compact

**Syntaxe : `condition ? valeurSiVrai : valeurSiFaux`**

```javascript
// Version longue avec if...else
let age = 20;
let statut;
if (age >= 18) {
  statut = "Majeur";
} else {
  statut = "Mineur";
}

// Version courte avec ternaire
let statut2 = age >= 18 ? "Majeur" : "Mineur";
console.log(statut2);  // "Majeur"
```

**Exemples pratiques :**

```javascript
// Message conditionnel
let score = 85;
let message = score >= 50 ? "Réussi ✓" : "Échoué ✗";
console.log(message);  // "Réussi ✓"

// Calcul conditionnel
let prix = 100;
let estMembre = true;
let prixFinal = estMembre ? prix * 0.9 : prix;  // -10% si membre
console.log(prixFinal);  // 90

// Dans template literals
let nom = "Ali";
let age = 25;
console.log(`${nom} est ${age >= 18 ? "majeur" : "mineur"}`);
// "Ali est majeur"

// Ternaires imbriqués (éviter si trop complexe)
let note = 75;
let mention = note >= 90 ? "Excellent" :
              note >= 80 ? "Très bien" :
              note >= 70 ? "Bien" :
              note >= 50 ? "Passable" : "Insuffisant";
console.log(mention);  // "Bien"
```

**💡 Quand utiliser le ternaire ?**
- ✅ Assignations simples
- ✅ Valeurs par défaut
- ✅ Affichage conditionnel court
- ❌ Logique complexe (utiliser if...else)

### 1.3 switch : Multiples Cas

**Utilisé quand on compare la MÊME variable à plusieurs valeurs**

```javascript
let jour = "lundi";

switch (jour) {
  case "lundi":
    console.log("Début de semaine");
    break;
  case "mardi":
  case "mercredi":
  case "jeudi":
    console.log("Milieu de semaine");
    break;
  case "vendredi":
    console.log("Bientôt le weekend !");
    break;
  case "samedi":
  case "dimanche":
    console.log("Weekend !");
    break;
  default:
    console.log("Jour invalide");
}
```

**⚠️ IMPORTANT : Le `break` !**

```javascript
// ❌ Sans break (fall-through)
let note = "B";
switch (note) {
  case "A":
    console.log("Excellent");
  case "B":
    console.log("Bien");         // Exécuté
  case "C":
    console.log("Passable");     // ⚠️ Aussi exécuté !
  case "D":
    console.log("Insuffisant");  // ⚠️ Aussi exécuté !
}

// ✅ Avec break
switch (note) {
  case "A":
    console.log("Excellent");
    break;
  case "B":
    console.log("Bien");         // ✅ Seul exécuté
    break;
  case "C":
    console.log("Passable");
    break;
  case "D":
    console.log("Insuffisant");
    break;
}
```

**Exemples pratiques :**

```javascript
// Calculatrice simple
function calculer(a, b, operation) {
  let resultat;
  
  switch (operation) {
    case "+":
      resultat = a + b;
      break;
    case "-":
      resultat = a - b;
      break;
    case "*":
      resultat = a * b;
      break;
    case "/":
      resultat = b !== 0 ? a / b : "Erreur";
      break;
    default:
      resultat = "Opération invalide";
  }
  
  return resultat;
}

console.log(calculer(10, 5, "+"));   // 15
console.log(calculer(10, 5, "*"));   // 50

// Tarification par catégorie
function getTarif(categorie) {
  switch (categorie) {
    case "enfant":
      return 5;
    case "etudiant":
      return 8;
    case "adulte":
      return 12;
    case "senior":
      return 7;
    default:
      return 12;  // Tarif par défaut
  }
}

console.log(getTarif("etudiant"));   // 8
```

### 1.4 Exercices : Structures Conditionnelles

#### 🟢 Exercice 1 : Année Bissextile

```javascript
// Une année est bissextile si :
// - Divisible par 4 ET
// - (Pas divisible par 100 OU divisible par 400)

function estBissextile(annee) {
  // TON CODE ICI
}

console.log(estBissextile(2024));  // true
console.log(estBissextile(2023));  // false
console.log(estBissextile(2000));  // true (divisible par 400)
console.log(estBissextile(1900));  // false (divisible par 100 mais pas 400)
```

**💡 Solution :**
```javascript
function estBissextile(annee) {
  return (annee % 4 === 0 && annee % 100 !== 0) || (annee % 400 === 0);
}
```

#### 🟡 Exercice 2 : Calculateur d'IMC

```javascript
// IMC = poids / (taille * taille)
// < 18.5 : Insuffisant
// 18.5-24.9 : Normal
// 25-29.9 : Surpoids
// >= 30 : Obésité

function calculerIMC(poids, taille) {
  // TON CODE ICI
}

console.log(calculerIMC(70, 1.75));  // "Normal"
console.log(calculerIMC(90, 1.75));  // "Surpoids"
```

**💡 Solution :**
```javascript
function calculerIMC(poids, taille) {
  const imc = poids / (taille * taille);
  
  if (imc < 18.5) {
    return "Insuffisant";
  } else if (imc < 25) {
    return "Normal";
  } else if (imc < 30) {
    return "Surpoids";
  } else {
    return "Obésité";
  }
}
```

#### 🔴 Exercice 3 : Pierre-Feuille-Ciseaux

```javascript
// Retourne "Joueur 1 gagne", "Joueur 2 gagne" ou "Égalité"
function pierreFeuilleCiseaux(joueur1, joueur2) {
  // TON CODE ICI
}

console.log(pierreFeuilleCiseaux("pierre", "ciseaux"));  // "Joueur 1 gagne"
console.log(pierreFeuilleCiseaux("feuille", "pierre"));  // "Joueur 1 gagne"
console.log(pierreFeuilleCiseaux("pierre", "pierre"));   // "Égalité"
```

**💡 Solution :**
```javascript
function pierreFeuilleCiseaux(joueur1, joueur2) {
  if (joueur1 === joueur2) {
    return "Égalité";
  }
  
  if (
    (joueur1 === "pierre" && joueur2 === "ciseaux") ||
    (joueur1 === "feuille" && joueur2 === "pierre") ||
    (joueur1 === "ciseaux" && joueur2 === "feuille")
  ) {
    return "Joueur 1 gagne";
  }
  
  return "Joueur 2 gagne";
}
```

---

## 2. Boucles : Répéter du Code

### 2.1 for : Boucle Classique

**Syntaxe : `for (initialisation; condition; incrémentation) { }`**

```javascript
// Compter de 0 à 4
for (let i = 0; i < 5; i++) {
  console.log(i);
}
// Affiche : 0, 1, 2, 3, 4

// Explication :
// 1. let i = 0        → Initialisation (exécuté UNE fois)
// 2. i < 5            → Condition (vérifié avant chaque itération)
// 3. i++              → Incrémentation (exécuté après chaque itération)
```

**Exemples pratiques :**

```javascript
// Afficher les nombres de 1 à 10
for (let i = 1; i <= 10; i++) {
  console.log(i);
}

// Table de multiplication
let nombre = 7;
for (let i = 1; i <= 10; i++) {
  console.log(`${nombre} × ${i} = ${nombre * i}`);
}
// Affiche :
// 7 × 1 = 7
// 7 × 2 = 14
// ...

// Compter à l'envers
for (let i = 10; i >= 1; i--) {
  console.log(i);
}
console.log("Décollage ! 🚀");

// Incrémenter de 2 (nombres pairs)
for (let i = 0; i <= 20; i += 2) {
  console.log(i);
}
// Affiche : 0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20

// Parcourir un string
let mot = "JavaScript";
for (let i = 0; i < mot.length; i++) {
  console.log(mot[i]);
}
// Affiche chaque lettre
```

#### 🔄 Boucles Imbriquées

```javascript
// Créer une grille
for (let ligne = 1; ligne <= 3; ligne++) {
  let rangee = "";
  for (let colonne = 1; colonne <= 3; colonne++) {
    rangee += "* ";
  }
  console.log(rangee);
}
// Affiche :
// * * *
// * * *
// * * *

// Table de multiplication complète
for (let i = 1; i <= 5; i++) {
  for (let j = 1; j <= 5; j++) {
    console.log(`${i} × ${j} = ${i * j}`);
  }
  console.log("---");
}
```

### 2.2 while : Boucle avec Condition

**Syntaxe : Tant que la condition est vraie, continue**

```javascript
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}
// Affiche : 0, 1, 2, 3, 4
```

**Exemples pratiques :**

```javascript
// Deviner un nombre
let nombreSecret = 7;
let tentative = 0;
let trouve = false;

while (!trouve && tentative < 10) {
  let guess = Math.floor(Math.random() * 10) + 1;
  tentative++;
  
  if (guess === nombreSecret) {
    console.log(`Trouvé en ${tentative} tentative(s) !`);
    trouve = true;
  } else {
    console.log(`Tentative ${tentative}: ${guess} (raté)`);
  }
}

// Somme jusqu'à un total
let somme = 0;
let nombre = 1;

while (somme < 100) {
  somme += nombre;
  nombre++;
}

console.log(`Somme: ${somme}, Dernière nombre ajouté: ${nombre - 1}`);

// Validation d'entrée (simulée)
let motDePasse = "";
let tentatives = 0;

while (motDePasse !== "secret" && tentatives < 3) {
  // Normalement : motDePasse = prompt("Mot de passe:");
  console.log("Tentative de connexion...");
  tentatives++;
}
```

**⚠️ Attention aux Boucles Infinies !**

```javascript
// ❌ BOUCLE INFINIE (crash!)
// let i = 0;
// while (i < 10) {
//   console.log(i);
//   // Oublié i++ → i reste 0 → condition toujours vraie → INFINI !
// }

// ✅ CORRECT
let i = 0;
while (i < 10) {
  console.log(i);
  i++;  // N'oublie JAMAIS d'incrémenter !
}
```

### 2.3 do...while : Exécute AU MOINS Une Fois

**Différence avec while : Le code s'exécute AVANT de vérifier la condition**

```javascript
// do...while
let i = 0;
do {
  console.log(i);  // Exécuté AU MOINS une fois
  i++;
} while (i < 5);
// Affiche : 0, 1, 2, 3, 4

// Même si condition fausse dès le début
let x = 10;
do {
  console.log(x);  // ✅ Exécuté quand même !
} while (x < 5);
// Affiche : 10

// Comparaison avec while
let y = 10;
while (y < 5) {
  console.log(y);  // ❌ Pas exécuté (condition fausse)
}
// N'affiche rien
```

**Usage typique : Menus et validations**

```javascript
// Menu interactif (simulé)
let choix;
do {
  console.log("\n=== MENU ===");
  console.log("1. Jouer");
  console.log("2. Scores");
  console.log("3. Quitter");
  
  // Normalement : choix = prompt("Choix:");
  choix = "3";  // Simulation
  
  switch (choix) {
    case "1":
      console.log("Lancement du jeu...");
      break;
    case "2":
      console.log("Affichage des scores...");
      break;
    case "3":
      console.log("Au revoir !");
      break;
    default:
      console.log("Choix invalide");
  }
} while (choix !== "3");

// Validation avec do...while
let age;
do {
  // age = prompt("Votre âge (0-120):");
  age = 25;  // Simulation
  if (age < 0 || age > 120) {
    console.log("Âge invalide, réessayez");
  }
} while (age < 0 || age > 120);
```

### 2.4 break et continue

#### 🛑 break : Sortir de la Boucle

```javascript
// Chercher un nombre
for (let i = 1; i <= 100; i++) {
  console.log(`Vérification de ${i}`);
  
  if (i === 42) {
    console.log("Trouvé !");
    break;  // ✅ Sort de la boucle
  }
}
// S'arrête à 42

// Chercher dans un array
let nombres = [5, 10, 15, 20, 25, 30];
let cible = 20;
let trouve = false;

for (let i = 0; i < nombres.length; i++) {
  if (nombres[i] === cible) {
    console.log(`Trouvé à l'index ${i}`);
    trouve = true;
    break;  // Pas besoin de continuer
  }
}

if (!trouve) {
  console.log("Pas trouvé");
}
```

#### ⏭️ continue : Passer à l'Itération Suivante

```javascript
// Afficher seulement les nombres impairs
for (let i = 1; i <= 10; i++) {
  if (i % 2 === 0) {
    continue;  // ✅ Saute les pairs
  }
  console.log(i);
}
// Affiche : 1, 3, 5, 7, 9

// Ignorer les valeurs négatives
let nombres = [5, -3, 8, -1, 10, -5, 15];
let somme = 0;

for (let i = 0; i < nombres.length; i++) {
  if (nombres[i] < 0) {
    continue;  // Ignore les négatifs
  }
  somme += nombres[i];
}

console.log(`Somme des positifs: ${somme}`);  // 38

// Filtrer avec continue
for (let i = 1; i <= 20; i++) {
  // Ignorer multiples de 3
  if (i % 3 === 0) {
    continue;
  }
  
  // Ignorer multiples de 5
  if (i % 5 === 0) {
    continue;
  }
  
  console.log(i);
}
// Affiche tous sauf multiples de 3 ou 5
```

### 2.5 for...of : Parcourir des Valeurs (ES6)

**Syntaxe moderne pour parcourir arrays et strings**

```javascript
// Parcourir un array
let fruits = ["Pomme", "Banane", "Orange"];

for (let fruit of fruits) {
  console.log(fruit);
}
// Affiche :
// Pomme
// Banane
// Orange

// Parcourir un string
let mot = "Bonjour";
for (let lettre of mot) {
  console.log(lettre);
}
// Affiche chaque lettre

// Avec index (moins courant)
let couleurs = ["Rouge", "Vert", "Bleu"];
let index = 0;
for (let couleur of couleurs) {
  console.log(`${index}: ${couleur}`);
  index++;
}
```

**Comparaison for vs for...of :**

```javascript
let nombres = [10, 20, 30, 40, 50];

// Avec for classique
for (let i = 0; i < nombres.length; i++) {
  console.log(nombres[i]);
}

// Avec for...of (plus lisible)
for (let nombre of nombres) {
  console.log(nombre);
}

// ✅ Utilise for...of quand tu n'as pas besoin de l'index
```

### 2.6 for...in : Parcourir des Propriétés (Objets)

```javascript
// Parcourir un objet
let personne = {
  nom: "Ali",
  age: 25,
  ville: "Dakar"
};

for (let cle in personne) {
  console.log(`${cle}: ${personne[cle]}`);
}
// Affiche :
// nom: Ali
// age: 25
// ville: Dakar

// ⚠️ for...in sur array (pas recommandé)
let nombres = [10, 20, 30];
for (let index in nombres) {
  console.log(index);  // "0", "1", "2" (strings!)
}

// ✅ Préférer for...of pour arrays
for (let nombre of nombres) {
  console.log(nombre);  // 10, 20, 30 (valeurs)
}
```

### 2.7 Exercices : Boucles

#### 🟢 Exercice 1 : Somme des N Premiers Entiers

```javascript
// Calculer 1 + 2 + 3 + ... + n
function sommeN(n) {
  // TON CODE ICI
}

console.log(sommeN(5));   // 15 (1+2+3+4+5)
console.log(sommeN(100)); // 5050
```

**💡 Solution :**
```javascript
function sommeN(n) {
  let somme = 0;
  for (let i = 1; i <= n; i++) {
    somme += i;
  }
  return somme;
}

// Ou formule mathématique : n * (n + 1) / 2
function sommeNRapide(n) {
  return n * (n + 1) / 2;
}
```

#### 🟡 Exercice 2 : FizzBuzz

```javascript
// Pour les nombres de 1 à 100 :
// - Multiple de 3 : afficher "Fizz"
// - Multiple de 5 : afficher "Buzz"
// - Multiple de 3 ET 5 : afficher "FizzBuzz"
// - Sinon : afficher le nombre

function fizzBuzz() {
  // TON CODE ICI
}

fizzBuzz();
// 1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz...
```

**💡 Solution :**
```javascript
function fizzBuzz() {
  for (let i = 1; i <= 100; i++) {
    if (i % 15 === 0) {
      console.log("FizzBuzz");
    } else if (i % 3 === 0) {
      console.log("Fizz");
    } else if (i % 5 === 0) {
      console.log("Buzz");
    } else {
      console.log(i);
    }
  }
}
```

#### 🔴 Exercice 3 : Pyramide d'Étoiles

```javascript
// Afficher une pyramide de taille n
function pyramide(n) {
  // TON CODE ICI
}

pyramide(5);
// Affiche :
//     *
//    ***
//   *****
//  *******
// *********
```

**💡 Solution :**
```javascript
function pyramide(n) {
  for (let i = 1; i <= n; i++) {
    // Espaces
    let ligne = " ".repeat(n - i);
    
    // Étoiles
    ligne += "*".repeat(2 * i - 1);
    
    console.log(ligne);
  }
}
```

---

## 3. Fonctions : Créer du Code Réutilisable

### 3.1 Qu'est-ce qu'une Fonction ?

Une **fonction** est un bloc de code réutilisable qui effectue une tâche spécifique.

**Analogie :**
```
Fonction = Machine
- Entrée (paramètres)
- Traitement
- Sortie (retour)

Exemple : Machine à café
- Entrée : Eau + Café
- Traitement : Chauffe et filtre
- Sortie : Tasse de café
```

**Pourquoi utiliser des fonctions ?**
1. 🔄 **Réutilisabilité** : Écrire une fois, utiliser partout
2. 📦 **Organisation** : Code structuré et lisible
3. 🧪 **Testabilité** : Facile à tester et debugger
4. 🛠️ **Maintenance** : Modifier à un seul endroit

### 3.2 Déclaration de Fonction

```javascript
// Syntaxe
function nomFonction(parametre1, parametre2) {
  // Code à exécuter
  return resultat;
}

// Exemple simple
function direBonjour() {
  console.log("Bonjour !");
}

// Appeler la fonction
direBonjour();  // Affiche : Bonjour !
direBonjour();  // Affiche : Bonjour !
```

**Avec paramètres :**

```javascript
function saluer(nom) {
  console.log(`Bonjour ${nom} !`);
}

saluer("Ali");     // Bonjour Ali !
saluer("Amina");   // Bonjour Amina !

// Plusieurs paramètres
function additionner(a, b) {
  let somme = a + b;
  console.log(`${a} + ${b} = ${somme}`);
}

additionner(5, 3);   // 5 + 3 = 8
additionner(10, 20); // 10 + 20 = 30
```

### 3.3 return : Retourner une Valeur

```javascript
// Sans return (affiche seulement)
function additionner1(a, b) {
  console.log(a + b);
}

let resultat1 = additionner1(5, 3);
console.log(resultat1);  // undefined (pas de return)

// Avec return (retourne la valeur)
function additionner2(a, b) {
  return a + b;
}

let resultat2 = additionner2(5, 3);
console.log(resultat2);  // 8 ✅

// Utiliser le résultat
let total = additionner2(10, 20) + additionner2(5, 15);
console.log(total);  // 50
```

**return arrête l'exécution :**

```javascript
function verifierAge(age) {
  if (age < 0) {
    return "Âge invalide";  // ✅ Sort immédiatement
  }
  
  if (age < 18) {
    return "Mineur";
  }
  
  return "Majeur";
  
  console.log("Jamais exécuté");  // ⚠️ Code mort
}

console.log(verifierAge(25));   // "Majeur"
console.log(verifierAge(15));   // "Mineur"
console.log(verifierAge(-5));   // "Âge invalide"
```

### 3.4 Paramètres par Défaut (ES6)

```javascript
// Avant ES6 (ancien)
function saluer(nom) {
  nom = nom || "Invité";  // Valeur par défaut
  console.log(`Bonjour ${nom}`);
}

// ES6+ (moderne)
function saluer2(nom = "Invité") {
  console.log(`Bonjour ${nom}`);
}

saluer2("Ali");    // Bonjour Ali
saluer2();         // Bonjour Invité

// Plusieurs paramètres avec défauts
function creerUtilisateur(nom, age = 18, ville = "Inconnu") {
  return {
    nom: nom,
    age: age,
    ville: ville
  };
}

console.log(creerUtilisateur("Ali", 25, "Dakar"));
// { nom: "Ali", age: 25, ville: "Dakar" }

console.log(creerUtilisateur("Amina"));
// { nom: "Amina", age: 18, ville: "Inconnu" }
```

### 3.5 Expression de Fonction

**Assigner une fonction à une variable :**

```javascript
// Déclaration de fonction (méthode 1)
function direBonjour1() {
  console.log("Bonjour !");
}

// Expression de fonction (méthode 2)
const direBonjour2 = function() {
  console.log("Bonjour !");
};

// Utilisation identique
direBonjour1();  // Bonjour !
direBonjour2();  // Bonjour !

// Avec paramètres
const multiplier = function(a, b) {
  return a * b;
};

console.log(multiplier(5, 3));  // 15
```

**Différence : Hoisting**

```javascript
// ✅ Fonctionne (hoisting)
direBonjour1();  // Bonjour !

function direBonjour1() {
  console.log("Bonjour !");
}

// ❌ Erreur (pas de hoisting pour expressions)
// direBonjour2();  // ReferenceError

const direBonjour2 = function() {
  console.log("Bonjour !");
};
```

### 3.6 Arrow Functions (ES6) 🏹

**Syntaxe moderne et concise :**

```javascript
// Fonction classique
const additionner1 = function(a, b) {
  return a + b;
};

// Arrow function
const additionner2 = (a, b) => {
  return a + b;
};

// Arrow function ultra-courte (return implicite)
const additionner3 = (a, b) => a + b;

console.log(additionner3(5, 3));  // 8
```

**Différents formats :**

```javascript
// Pas de paramètres
const direBonjour = () => console.log("Bonjour");
direBonjour();

// Un seul paramètre (parenthèses optionnelles)
const doubler1 = x => x * 2;
const doubler2 = (x) => x * 2;  // Identique

console.log(doubler1(5));  // 10

// Plusieurs paramètres
const additionner = (a, b) => a + b;

// Plusieurs lignes (return explicite)
const calculer = (a, b) => {
  let somme = a + b;
  let produit = a * b;
  return { somme, produit };
};

console.log(calculer(3, 4));  // { somme: 7, produit: 12 }

// Retourner un objet (attention aux parenthèses)
const creerUser = (nom, age) => ({ nom: nom, age: age });
// OU avec shorthand
const creerUser2 = (nom, age) => ({ nom, age });

console.log(creerUser("Ali", 25));  // { nom: "Ali", age: 25 }
```

**Quand utiliser Arrow Functions ?**

✅ **Utilise Arrow Functions pour :**
- Callbacks (map, filter, forEach)
- Fonctions courtes et simples
- Code moderne et concis

⚠️ **Évite Arrow Functions pour :**
- Méthodes d'objets (problème avec `this`)
- Constructeurs
- Fonctions avec logique complexe

```javascript
// ✅ Bon usage (callback)
const nombres = [1, 2, 3, 4, 5];
const doubles = nombres.map(n => n * 2);
console.log(doubles);  // [2, 4, 6, 8, 10]

// ⚠️ Mauvais usage (méthode d'objet)
const user = {
  nom: "Ali",
  direBonjour: () => {
    console.log(`Bonjour ${this.nom}`);  // ❌ this ne fonctionne pas
  }
};

// ✅ Utilise fonction classique pour méthodes
const user2 = {
  nom: "Ali",
  direBonjour: function() {
    console.log(`Bonjour ${this.nom}`);  // ✅ Fonctionne
  }
};
```

### 3.7 Fonctions comme Valeurs

**En JavaScript, les fonctions sont des "first-class citizens"**

```javascript
// Assigner à une variable
const maFonction = function() {
  console.log("Hello");
};

// Passer comme argument
function executerDeuxFois(fn) {
  fn();
  fn();
}

executerDeuxFois(maFonction);
// Affiche :
// Hello
// Hello

// Retourner une fonction
function creerMultiplicateur(facteur) {
  return function(nombre) {
    return nombre * facteur;
  };
}

const doubler = creerMultiplicateur(2);
const tripler = creerMultiplicateur(3);

console.log(doubler(5));   // 10
console.log(tripler(5));   // 15

// Stocker dans un array
const operations = [
  (a, b) => a + b,
  (a, b) => a - b,
  (a, b) => a * b,
  (a, b) => a / b
];

console.log(operations[0](10, 5));  // 15 (addition)
console.log(operations[2](10, 5));  // 50 (multiplication)
```

### 3.8 Exercices : Fonctions

#### 🟢 Exercice 1 : Fonction de Température

```javascript
// Convertir Celsius en Fahrenheit
// Formule : F = C * 9/5 + 32

function celsiusVersFahrenheit(celsius) {
  // TON CODE ICI
}

console.log(celsiusVersFahrenheit(0));    // 32
console.log(celsiusVersFahrenheit(100));  // 212
console.log(celsiusVersFahrenheit(25));   // 77
```

**💡 Solution :**
```javascript
function celsiusVersFahrenheit(celsius) {
  return celsius * 9/5 + 32;
}

// Ou en Arrow Function
const celsiusVersFahrenheit2 = celsius => celsius * 9/5 + 32;
```

#### 🟡 Exercice 2 : Compter les Voyelles

```javascript
// Retourne le nombre de voyelles dans un string
function compterVoyelles(texte) {
  // TON CODE ICI
}

console.log(compterVoyelles("Hello"));       // 2
console.log(compterVoyelles("JavaScript"));  // 3
console.log(compterVoyelles("Bonjour"));     // 3
```

**💡 Solution :**
```javascript
function compterVoyelles(texte) {
  const voyelles = "aeiouAEIOU";
  let compteur = 0;
  
  for (let lettre of texte) {
    if (voyelles.includes(lettre)) {
      compteur++;
    }
  }
  
  return compteur;
}
```

#### 🔴 Exercice 3 : Nombre Premier

```javascript
// Vérifier si un nombre est premier
// (divisible uniquement par 1 et lui-même)

function estPremier(nombre) {
  // TON CODE ICI
}

console.log(estPremier(2));   // true
console.log(estPremier(17));  // true
console.log(estPremier(20));  // false
console.log(estPremier(1));   // false
```

**💡 Solution :**
```javascript
function estPremier(nombre) {
  if (nombre <= 1) return false;
  if (nombre === 2) return true;
  if (nombre % 2 === 0) return false;
  
  // Vérifier divisibilité jusqu'à √nombre
  for (let i = 3; i <= Math.sqrt(nombre); i += 2) {
    if (nombre % i === 0) {
      return false;
    }
  }
  
  return true;
}
```

---

## 4. Portée et Closures

### 4.1 Portée (Scope)

**La portée détermine où une variable est accessible**

#### 🌍 Portée Globale

```javascript
let nomGlobal = "Ali";  // Variable globale

function afficher() {
  console.log(nomGlobal);  // ✅ Accessible
}

afficher();  // "Ali"
console.log(nomGlobal);  // "Ali"
```

#### 📦 Portée de Fonction

```javascript
function maFonction() {
  let nomLocal = "Amina";  // Variable locale
  console.log(nomLocal);   // ✅ Accessible ici
}

maFonction();  // "Amina"
// console.log(nomLocal);  // ❌ ReferenceError (hors portée)
```

#### 🧱 Portée de Bloc (let et const)

```javascript
if (true) {
  let x = 10;
  const y = 20;
  console.log(x, y);  // ✅ 10 20
}

// console.log(x, y);  // ❌ ReferenceError

// Comparaison avec var
if (true) {
  var z = 30;
  console.log(z);  // ✅ 30
}
console.log(z);  // ✅ 30 (var n'a pas de portée de bloc!)
```

#### 🎯 Règles de Portée

```javascript
let global = "Global";

function externe() {
  let externeVar = "Externe";
  
  function interne() {
    let interneVar = "Interne";
    
    // Accès à toutes les portées
    console.log(global);      // ✅ "Global"
    console.log(externeVar);  // ✅ "Externe"
    console.log(interneVar);  // ✅ "Interne"
  }
  
  interne();
  console.log(global);      // ✅ "Global"
  console.log(externeVar);  // ✅ "Externe"
  // console.log(interneVar);  // ❌ ReferenceError
}

externe();
console.log(global);      // ✅ "Global"
// console.log(externeVar);  // ❌ ReferenceError
```

### 4.2 Closures (Fermetures)

**Une closure = fonction qui "se souvient" de son environnement**

```javascript
function creerCompteur() {
  let compte = 0;  // Variable privée
  
  return function() {
    compte++;
    return compte;
  };
}

const compteur1 = creerCompteur();
console.log(compteur1());  // 1
console.log(compteur1());  // 2
console.log(compteur1());  // 3

const compteur2 = creerCompteur();  // Nouveau compteur
console.log(compteur2());  // 1
console.log(compteur2());  // 2

console.log(compteur1());  // 4 (indépendant)
```

**Explication :**
- `compte` est privée (pas accessible directement)
- La fonction retournée "se souvient" de `compte`
- Chaque appel de `creerCompteur()` crée un nouveau `compte`

**Exemples pratiques :**

```javascript
// Factory de multiplicateurs
function creerMultiplicateur(facteur) {
  return function(nombre) {
    return nombre * facteur;
  };
}

const doubler = creerMultiplicateur(2);
const tripler = creerMultiplicateur(3);
const quintupler = creerMultiplicateur(5);

console.log(doubler(10));      // 20
console.log(tripler(10));      // 30
console.log(quintupler(10));   // 50

// Encapsulation de données
function creerBanque() {
  let solde = 0;  // Privé !
  
  return {
    deposer: function(montant) {
      solde += montant;
      return solde;
    },
    retirer: function(montant) {
      if (montant <= solde) {
        solde -= montant;
        return solde;
      }
      return "Solde insuffisant";
    },
    consulter: function() {
      return solde;
    }
  };
}

const monCompte = creerBanque();
console.log(monCompte.deposer(1000));   // 1000
console.log(monCompte.retirer(300));    // 700
console.log(monCompte.consulter());     // 700
// console.log(monCompte.solde);        // undefined (privé!)
```

---

## 5. Arrays (Tableaux) : Gérer des Listes

### 5.1 Créer des Arrays

```javascript
// Méthode 1 : Littéral (recommandé)
let fruits = ["Pomme", "Banane", "Orange"];

// Méthode 2 : Constructeur
let nombres = new Array(1, 2, 3, 4, 5);

// Array vide
let liste = [];

// Array avec types mixtes (pas recommandé mais possible)
let mixte = [1, "texte", true, null, { nom: "Ali" }];

// Accès par index (commence à 0)
console.log(fruits[0]);  // "Pomme"
console.log(fruits[1]);  // "Banane"
console.log(fruits[2]);  // "Orange"

// Longueur
console.log(fruits.length);  // 3

// Dernier élément
console.log(fruits[fruits.length - 1]);  // "Orange"
```

### 5.2 Modifier des Arrays

```javascript
let fruits = ["Pomme", "Banane", "Orange"];

// Modifier un élément
fruits[1] = "Fraise";
console.log(fruits);  // ["Pomme", "Fraise", "Orange"]

// Ajouter à la fin
fruits.push("Mangue");
console.log(fruits);  // ["Pomme", "Fraise", "Orange", "Mangue"]

// Ajouter au début
fruits.unshift("Ananas");
console.log(fruits);  // ["Ananas", "Pomme", "Fraise", "Orange", "Mangue"]

// Retirer le dernier
let dernier = fruits.pop();
console.log(dernier);  // "Mangue"
console.log(fruits);   // ["Ananas", "Pomme", "Fraise", "Orange"]

// Retirer le premier
let premier = fruits.shift();
console.log(premier);  // "Ananas"
console.log(fruits);   // ["Pomme", "Fraise", "Orange"]
```

### 5.3 Parcourir des Arrays

```javascript
let nombres = [10, 20, 30, 40, 50];

// Méthode 1 : for classique
for (let i = 0; i < nombres.length; i++) {
  console.log(nombres[i]);
}

// Méthode 2 : for...of (moderne)
for (let nombre of nombres) {
  console.log(nombre);
}

// Méthode 3 : forEach (fonctionnel)
nombres.forEach(function(nombre) {
  console.log(nombre);
});

// forEach avec arrow function
nombres.forEach(nombre => console.log(nombre));

// forEach avec index
nombres.forEach((nombre, index) => {
  console.log(`Index ${index}: ${nombre}`);
});
```

### 5.4 Méthodes d'Arrays Essentielles

#### 🔍 Recherche

```javascript
let fruits = ["Pomme", "Banane", "Orange", "Banane"];

// includes : vérifie présence
console.log(fruits.includes("Banane"));  // true
console.log(fruits.includes("Mangue"));  // false

// indexOf : première occurrence
console.log(fruits.indexOf("Banane"));   // 1
console.log(fruits.indexOf("Mangue"));   // -1 (pas trouvé)

// lastIndexOf : dernière occurrence
console.log(fruits.lastIndexOf("Banane"));  // 3

// find : premier élément qui satisfait condition
let nombres = [5, 12, 8, 130, 44];
let grand = nombres.find(n => n > 10);
console.log(grand);  // 12

// findIndex : index du premier qui satisfait
let index = nombres.findIndex(n => n > 10);
console.log(index);  // 1
```

#### ✂️ Extraction et Modification

```javascript
let nombres = [1, 2, 3, 4, 5];

// slice : copie une partie (ne modifie pas l'original)
let partie = nombres.slice(1, 4);
console.log(partie);    // [2, 3, 4]
console.log(nombres);   // [1, 2, 3, 4, 5] (inchangé)

// splice : modifie l'array
let fruits = ["Pomme", "Banane", "Orange", "Mangue"];

// Supprimer 2 éléments à partir de l'index 1
let supprimes = fruits.splice(1, 2);
console.log(supprimes);  // ["Banane", "Orange"]
console.log(fruits);     // ["Pomme", "Mangue"]

// Ajouter des éléments
fruits.splice(1, 0, "Fraise", "Kiwi");
console.log(fruits);  // ["Pomme", "Fraise", "Kiwi", "Mangue"]

// Remplacer
fruits.splice(1, 2, "Ananas");
console.log(fruits);  // ["Pomme", "Ananas", "Mangue"]
```

#### 🔗 Combiner et Diviser

```javascript
// concat : combiner arrays
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combine = arr1.concat(arr2);
console.log(combine);  // [1, 2, 3, 4, 5, 6]

// Spread operator (moderne)
let combine2 = [...arr1, ...arr2];
console.log(combine2);  // [1, 2, 3, 4, 5, 6]

// join : array → string
let mots = ["Bonjour", "le", "monde"];
let phrase = mots.join(" ");
console.log(phrase);  // "Bonjour le monde"

// split : string → array
let texte = "ali,amina,omar";
let noms = texte.split(",");
console.log(noms);  // ["ali", "amina", "omar"]
```

#### 🔄 Transformation

```javascript
// reverse : inverser l'ordre (modifie l'original)
let nombres = [1, 2, 3, 4, 5];
nombres.reverse();
console.log(nombres);  // [5, 4, 3, 2, 1]

// sort : trier (modifie l'original)
let fruits = ["Orange", "Pomme", "Banane"];
fruits.sort();
console.log(fruits);  // ["Banane", "Orange", "Pomme"]

// ⚠️ sort avec nombres (problème!)
let nums = [10, 5, 40, 25, 1000];
nums.sort();
console.log(nums);  // [10, 1000, 25, 40, 5] (trie comme strings!)

// ✅ sort correct pour nombres
nums.sort((a, b) => a - b);
console.log(nums);  // [5, 10, 25, 40, 1000]

// Ordre décroissant
nums.sort((a, b) => b - a);
console.log(nums);  // [1000, 40, 25, 10, 5]
```

---

## 6. Méthodes d'Arrays Fonctionnelles

### 6.1 forEach : Itérer sur Chaque Élément

```javascript
let nombres = [1, 2, 3, 4, 5];

// forEach ne retourne rien (undefined)
nombres.forEach(function(nombre) {
  console.log(nombre * 2);
});
// Affiche : 2, 4, 6, 8, 10

// Avec index et array
nombres.forEach((nombre, index, arr) => {
  console.log(`${index}: ${nombre} (total: ${arr.length})`);
});

// Usage pratique : affichage
let users = ["Ali", "Amina", "Omar"];
users.forEach(user => {
  console.log(`Bonjour ${user} !`);
});
```

### 6.2 map : Transformer Chaque Élément

```javascript
let nombres = [1, 2, 3, 4, 5];

// map retourne un NOUVEAU array
let doubles = nombres.map(n => n * 2);
console.log(doubles);  // [2, 4, 6, 8, 10]
console.log(nombres);  // [1, 2, 3, 4, 5] (inchangé)

// Transformer des objets
let users = [
  { nom: "Ali", age: 25 },
  { nom: "Amina", age: 30 },
  { nom: "Omar", age: 22 }
];

let noms = users.map(user => user.nom);
console.log(noms);  // ["Ali", "Amina", "Omar"]

let ages = users.map(user => user.age);
console.log(ages);  // [25, 30, 22]

// Transformation complexe
let infos = users.map(user => {
  return {
    nom: user.nom.toUpperCase(),
    estMajeur: user.age >= 18
  };
});
console.log(infos);
// [
//   { nom: "ALI", estMajeur: true },
//   { nom: "AMINA", estMajeur: true },
//   { nom: "OMAR", estMajeur: true }
// ]
```

### 6.3 filter : Filtrer des Éléments

```javascript
let nombres = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// filter retourne un NOUVEAU array avec éléments qui passent le test
let pairs = nombres.filter(n => n % 2 === 0);
console.log(pairs);  // [2, 4, 6, 8, 10]

let impairs = nombres.filter(n => n % 2 !== 0);
console.log(impairs);  // [1, 3, 5, 7, 9]

let grands = nombres.filter(n => n > 5);
console.log(grands);  // [6, 7, 8, 9, 10]

// Filtrer des objets
let users = [
  { nom: "Ali", age: 17 },
  { nom: "Amina", age: 25 },
  { nom: "Omar", age: 30 },
  { nom: "Fatou", age: 16 }
];

let majeurs = users.filter(user => user.age >= 18);
console.log(majeurs);
// [
//   { nom: "Amina", age: 25 },
//   { nom: "Omar", age: 30 }
// ]

// Combiner avec map
let nomsMajeurs = users
  .filter(user => user.age >= 18)
  .map(user => user.nom);
console.log(nomsMajeurs);  // ["Amina", "Omar"]
```

### 6.4 reduce : Réduire à une Valeur

```javascript
let nombres = [1, 2, 3, 4, 5];

// reduce accumule une valeur
let somme = nombres.reduce((acc, n) => acc + n, 0);
console.log(somme);  // 15

// Comment ça marche:
// 1. acc = 0, n = 1 → retourne 0 + 1 = 1
// 2. acc = 1, n = 2 → retourne 1 + 2 = 3
// 3. acc = 3, n = 3 → retourne 3 + 3 = 6
// 4. acc = 6, n = 4 → retourne 6 + 4 = 10
// 5. acc = 10, n = 5 → retourne 10 + 5 = 15

// Produit
let produit = nombres.reduce((acc, n) => acc * n, 1);
console.log(produit);  // 120 (1*2*3*4*5)

// Maximum
let max = nombres.reduce((acc, n) => Math.max(acc, n));
console.log(max);  // 5

// Compter occurrences
let fruits = ["pomme", "banane", "pomme", "orange", "banane", "pomme"];
let compte = fruits.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});
console.log(compte);
// { pomme: 3, banane: 2, orange: 1 }

// Aplatir un array
let nested = [[1, 2], [3, 4], [5, 6]];
let flat = nested.reduce((acc, arr) => acc.concat(arr), []);
console.log(flat);  // [1, 2, 3, 4, 5, 6]
```

### 6.5 find et findIndex

```javascript
let users = [
  { id: 1, nom: "Ali", age: 25 },
  { id: 2, nom: "Amina", age: 30 },
  { id: 3, nom: "Omar", age: 22 }
];

// find : retourne le PREMIER élément qui satisfait
let user = users.find(u => u.age > 25);
console.log(user);  // { id: 2, nom: "Amina", age: 30 }

let userById = users.find(u => u.id === 3);
console.log(userById);  // { id: 3, nom: "Omar", age: 22 }

let introuvable = users.find(u => u.age > 100);
console.log(introuvable);  // undefined

// findIndex : retourne l'INDEX
let index = users.findIndex(u => u.nom === "Omar");
console.log(index);  // 2
```

### 6.6 some et every

```javascript
let nombres = [1, 2, 3, 4, 5];

// some : AU MOINS UN satisfait la condition
let aPairs = nombres.some(n => n % 2 === 0);
console.log(aPairs);  // true (2, 4 sont pairs)

let aGrands = nombres.some(n => n > 10);
console.log(aGrands);  // false

// every : TOUS satisfont la condition
let tousPositifs = nombres.every(n => n > 0);
console.log(tousPositifs);  // true

let tousPairs = nombres.every(n => n % 2 === 0);
console.log(tousPairs);  // false (1, 3, 5 sont impairs)

// Usage pratique : validation
let users = [
  { nom: "Ali", age: 25 },
  { nom: "Amina", age: 30 },
  { nom: "Omar", age: 22 }
];

let tousMajeurs = users.every(user => user.age >= 18);
console.log(tousMajeurs);  // true

let aSenior = users.some(user => user.age >= 60);
console.log(aSenior);  // false
```

### 6.7 Chaîner les Méthodes

```javascript
let nombres = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Filtrer, mapper, puis réduire
let resultat = nombres
  .filter(n => n % 2 === 0)     // [2, 4, 6, 8, 10]
  .map(n => n * 2)              // [4, 8, 12, 16, 20]
  .reduce((acc, n) => acc + n, 0);  // 60

console.log(resultat);  // 60

// Exemple complexe
let users = [
  { nom: "Ali", age: 17, ville: "Dakar" },
  { nom: "Amina", age: 25, ville: "Dakar" },
  { nom: "Omar", age: 30, ville: "Thiès" },
  { nom: "Fatou", age: 22, ville: "Dakar" }
];

// Majeurs de Dakar uniquement leurs noms
let nomsMajeursDakar = users
  .filter(u => u.age >= 18)        // Majeurs
  .filter(u => u.ville === "Dakar") // De Dakar
  .map(u => u.nom);                 // Noms seulement

console.log(nomsMajeursDakar);  // ["Amina", "Fatou"]
```

---

## 7. Projet 1 : Todo List Interactive

### 7.1 Objectif

Créer une application Todo List avec :
- Ajouter des tâches
- Marquer comme complétées
- Supprimer des tâches
- Filtrer (Toutes / Actives / Complétées)
- Compteur de tâches
- LocalStorage (sauvegarde)

### 7.2 Structure

```bash
todo-list/
├── index.html
├── style.css
└── script.js
```

### 7.3 Code HTML

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>📝 Todo List</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>📝 Ma Todo List</h1>
    
    <!-- Formulaire -->
    <div class="input-section">
      <input 
        type="text" 
        id="todoInput" 
        placeholder="Ajouter une nouvelle tâche..."
        maxlength="100"
      />
      <button id="addBtn">Ajouter</button>
    </div>
    
    <!-- Filtres -->
    <div class="filters">
      <button class="filter-btn active" data-filter="all">
        Toutes <span id="countAll">0</span>
      </button>
      <button class="filter-btn" data-filter="active">
        Actives <span id="countActive">0</span>
      </button>
      <button class="filter-btn" data-filter="completed">
        Complétées <span id="countCompleted">0</span>
      </button>
    </div>
    
    <!-- Liste des todos -->
    <ul id="todoList" class="todo-list"></ul>
    
    <!-- Actions -->
    <div class="actions">
      <button id="clearCompleted">Supprimer les complétées</button>
      <button id="clearAll">Tout supprimer</button>
    </div>
  </div>
  
  <script src="script.js"></script>
</body>
</html>
```

### 7.4 Code CSS

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
  max-width: 600px;
  width: 100%;
}

h1 {
  text-align: center;
  color: #667eea;
  margin-bottom: 30px;
  font-size: 2.5em;
}

/* Input Section */
.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

#todoInput {
  flex: 1;
  padding: 15px;
  font-size: 1em;
  border: 2px solid #e0e0e0;
  border-radius: 10px;
  outline: none;
  transition: all 0.3s;
}

#todoInput:focus {
  border-color: #667eea;
}

#addBtn {
  padding: 15px 30px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s;
}

#addBtn:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
}

/* Filtres */
.filters {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.filter-btn {
  flex: 1;
  padding: 10px;
  background: #f5f5f5;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: bold;
}

.filter-btn:hover {
  background: #e8e8e8;
}

.filter-btn.active {
  background: #667eea;
  color: white;
  border-color: #667eea;
}

.filter-btn span {
  display: inline-block;
  background: rgba(255, 255, 255, 0.3);
  padding: 2px 8px;
  border-radius: 10px;
  margin-left: 5px;
}

/* Todo List */
.todo-list {
  list-style: none;
  margin-bottom: 20px;
  max-height: 400px;
  overflow-y: auto;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 15px;
  background: #f5f5f5;
  border-radius: 10px;
  margin-bottom: 10px;
  transition: all 0.3s;
  animation: slideIn 0.3s ease-out;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.todo-item:hover {
  background: #e8e8e8;
}

.todo-item.completed {
  opacity: 0.6;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  color: #999;
}

.todo-checkbox {
  width: 20px;
  height: 20px;
  margin-right: 15px;
  cursor: pointer;
}

.todo-text {
  flex: 1;
  font-size: 1em;
}

.todo-delete {
  background: #f5576c;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s;
}

.todo-delete:hover {
  background: #d4455a;
  transform: scale(1.1);
}

/* Actions */
.actions {
  display: flex;
  gap: 10px;
}

.actions button {
  flex: 1;
  padding: 12px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s;
}

#clearCompleted {
  background: #ffa726;
  color: white;
}

#clearAll {
  background: #ef5350;
  color: white;
}

.actions button:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

/* Message vide */
.empty-message {
  text-align: center;
  padding: 40px;
  color: #999;
  font-size: 1.2em;
}
```

### 7.5 Code JavaScript

```javascript
// ============================================
// TODO LIST APPLICATION
// ============================================

// État de l'application
let todos = [];
let filter = 'all';  // 'all', 'active', 'completed'

// Éléments DOM
const todoInput = document.getElementById('todoInput');
const addBtn = document.getElementById('addBtn');
const todoList = document.getElementById('todoList');
const filterBtns = document.querySelectorAll('.filter-btn');
const clearCompletedBtn = document.getElementById('clearCompleted');
const clearAllBtn = document.getElementById('clearAll');
const countAll = document.getElementById('countAll');
const countActive = document.getElementById('countActive');
const countCompleted = document.getElementById('countCompleted');

// Charger depuis LocalStorage
function loadTodos() {
  const saved = localStorage.getItem('todos');
  if (saved) {
    todos = JSON.parse(saved);
    renderTodos();
  }
}

// Sauvegarder dans LocalStorage
function saveTodos() {
  localStorage.setItem('todos', JSON.stringify(todos));
}

// Ajouter une todo
function addTodo() {
  const text = todoInput.value.trim();
  
  if (text === '') {
    alert('Veuillez entrer une tâche');
    return;
  }
  
  const todo = {
    id: Date.now(),
    text: text,
    completed: false,
    createdAt: new Date().toISOString()
  };
  
  todos.push(todo);
  todoInput.value = '';
  saveTodos();
  renderTodos();
}

// Toggle completed
function toggleTodo(id) {
  const todo = todos.find(t => t.id === id);
  if (todo) {
    todo.completed = !todo.completed;
    saveTodos();
    renderTodos();
  }
}

// Supprimer une todo
function deleteTodo(id) {
  todos = todos.filter(t => t.id !== id);
  saveTodos();
  renderTodos();
}

// Supprimer toutes les complétées
function clearCompleted() {
  todos = todos.filter(t => !t.completed);
  saveTodos();
  renderTodos();
}

// Tout supprimer
function clearAll() {
  if (confirm('Supprimer toutes les tâches ?')) {
    todos = [];
    saveTodos();
    renderTodos();
  }
}

// Filtrer les todos
function getFilteredTodos() {
  switch (filter) {
    case 'active':
      return todos.filter(t => !t.completed);
    case 'completed':
      return todos.filter(t => t.completed);
    default:
      return todos;
  }
}

// Mettre à jour les compteurs
function updateCounts() {
  countAll.textContent = todos.length;
  countActive.textContent = todos.filter(t => !t.completed).length;
  countCompleted.textContent = todos.filter(t => t.completed).length;
}

// Rendre les todos
function renderTodos() {
  const filtered = getFilteredTodos();
  
  // Vider la liste
  todoList.innerHTML = '';
  
  // Si vide
  if (filtered.length === 0) {
    todoList.innerHTML = `
      <div class="empty-message">
        ${filter === 'all' ? 'Aucune tâche' :
          filter === 'active' ? 'Aucune tâche active' :
          'Aucune tâche complétée'}
      </div>
    `;
  } else {
    // Créer les éléments
    filtered.forEach(todo => {
      const li = document.createElement('li');
      li.className = `todo-item ${todo.completed ? 'completed' : ''}`;
      li.innerHTML = `
        <input 
          type="checkbox" 
          class="todo-checkbox"
          ${todo.completed ? 'checked' : ''}
          onchange="toggleTodo(${todo.id})"
        />
        <span class="todo-text">${todo.text}</span>
        <button class="todo-delete" onclick="deleteTodo(${todo.id})">
          🗑️
        </button>
      `;
      todoList.appendChild(li);
    });
  }
  
  updateCounts();
}

// Changer le filtre
function setFilter(newFilter) {
  filter = newFilter;
  
  // Mettre à jour les boutons
  filterBtns.forEach(btn => {
    btn.classList.remove('active');
    if (btn.dataset.filter === filter) {
      btn.classList.add('active');
    }
  });
  
  renderTodos();
}

// Event Listeners
addBtn.addEventListener('click', addTodo);

todoInput.addEventListener('keypress', (e) => {
  if (e.key === 'Enter') {
    addTodo();
  }
});

filterBtns.forEach(btn => {
  btn.addEventListener('click', () => {
    setFilter(btn.dataset.filter);
  });
});

clearCompletedBtn.addEventListener('click', clearCompleted);
clearAllBtn.addEventListener('click', clearAll);

// Initialisation
loadTodos();
console.log('📝 Todo List chargée !');
```

---

## 🎯 Checkpoint Partie 3

### ✅ Ce que tu as accompli

Félicitations ! Tu maîtrises maintenant :

- [x] Structures conditionnelles (if, else, switch)
- [x] Boucles (for, while, do-while, for...of)
- [x] Fonctions (classiques, expressions, arrow)
- [x] Portée et closures
- [x] Arrays et manipulation
- [x] Méthodes fonctionnelles (map, filter, reduce)
- [x] Projet Todo List complet !

### 🎓 Niveau actuel : Programmeur JavaScript Fonctionnel

```
Progression : ████████████████░░░░ 45%

Prochaine étape : DOM et manipulation du navigateur !
```

---

## 📝 Exercices Finaux

### 🏆 Défi 1 : Analyseur de Texte

```javascript
// Créer une fonction qui analyse un texte et retourne :
// - Nombre de mots
// - Nombre de phrases
// - Nombre de paragraphes
// - Mot le plus long
// - Mot le plus fréquent

function analyserTexte(texte) {
  // TON CODE ICI
}

const texte = "Bonjour le monde. JavaScript est génial. JavaScript est puissant.";
console.log(analyserTexte(texte));
```

### 🏆 Défi 2 : Gestionnaire de Budget

```javascript
// Créer des fonctions pour gérer un budget :
// - Ajouter une dépense
// - Calculer le total
// - Filtrer par catégorie
// - Obtenir la dépense la plus élevée

let depenses = [];

function ajouterDepense(description, montant, categorie) {
  // TON CODE ICI
}

function calculerTotal() {
  // TON CODE ICI
}

function filtrerParCategorie(categorie) {
  // TON CODE ICI
}
```

---

## 🚀 Suite dans la Partie 4

**Dans la Partie 4, tu vas apprendre :**

- DOM (Document Object Model)
- Sélectionner et modifier des éléments HTML
- Events et Event Listeners
- Formulaires et validation
- LocalStorage et SessionStorage
- Projets : Quiz interactif, Minuteur, Jeu

**Durée estimée Partie 4 : 25-30 heures**

---

*Ce guide est créé avec ❤️ par la Learning Schooling Foundation*  
*100% Gratuit • Pour Tous • À Jamais*

**🎉 FIN DE LA PARTIE 3 🎉**

---
