# JavaScript Moderne - Partie 4 : Le DOM et la Manipulation d'Éléments

**🔗 Repository GitHub :** [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)

---

## Table des Matières

1. [Introduction au DOM](#1-introduction-au-dom)
2. [Comprendre l'Arbre DOM](#2-comprendre-larbre-dom)
3. [Sélectionner des Éléments](#3-sélectionner-des-éléments)
4. [Modifier le Contenu](#4-modifier-le-contenu)
5. [Manipuler les Styles](#5-manipuler-les-styles)
6. [Créer et Insérer des Éléments](#6-créer-et-insérer-des-éléments)
7. [Supprimer des Éléments](#7-supprimer-des-éléments)
8. [Les Événements](#8-les-événements)
9. [Event Delegation](#9-event-delegation)
10. [Formulaires et Validation](#10-formulaires-et-validation)
11. [Le localStorage](#11-le-localstorage)
12. [Projet Complet : To-Do List](#12-projet-complet-to-do-list)
13. [Exercices Progressifs](#13-exercices-progressifs)
14. [Debugging et Erreurs Communes](#14-debugging-et-erreurs-communes)
15. [Aller Plus Loin](#15-aller-plus-loin)

---

## 1. Introduction au DOM

### Qu'est-ce que le DOM ?

Le **DOM (Document Object Model)** est une interface de programmation qui représente ta page HTML comme un arbre d'objets que JavaScript peut manipuler.

**Analogie simple :**
Imagine une maison (ta page web). Le DOM, c'est le plan de la maison qui te permet de :
- Trouver n'importe quelle pièce (élément HTML)
- Modifier la décoration (changer le contenu)
- Repeindre les murs (modifier le style)
- Ajouter de nouvelles pièces (créer des éléments)
- Démolir des murs (supprimer des éléments)

### Pourquoi le DOM est crucial ?

Sans le DOM, JavaScript ne pourrait pas :
- Changer le texte d'une page
- Réagir aux clics de l'utilisateur
- Créer des interfaces interactives
- Faire des Single Page Applications (SPA)

**Tout ce que tu vois d'interactif sur le web (Facebook, Gmail, YouTube) utilise massivement le DOM.**

### Ta première manipulation DOM

Ouvre la console de ton navigateur (F12) sur n'importe quel site et tape :

```javascript
document.body.style.backgroundColor = 'red';
```

**Tu viens de modifier le DOM !** La page change instantanément sans recharger.

---

## 2. Comprendre l'Arbre DOM

### Structure HTML vs Structure DOM

Quand tu écris du HTML :

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Ma Page</title>
  </head>
  <body>
    <h1 id="titre">Bienvenue</h1>
    <p class="description">Ceci est un paragraphe</p>
    <button>Clique-moi</button>
  </body>
</html>
```

Le navigateur crée un **arbre DOM** :

```
Document
  └── html
      ├── head
      │   └── title
      │       └── #text: "Ma Page"
      └── body
          ├── h1 (id="titre")
          │   └── #text: "Bienvenue"
          ├── p (class="description")
          │   └── #text: "Ceci est un paragraphe"
          └── button
              └── #text: "Clique-moi"
```

### Types de nœuds

Le DOM contient différents types de nœuds :

1. **Nœuds d'éléments** : Les balises HTML (`<div>`, `<p>`, `<button>`)
2. **Nœuds de texte** : Le texte à l'intérieur des balises
3. **Nœuds d'attributs** : Les attributs (`id`, `class`, `src`)
4. **Nœuds de commentaire** : Les commentaires HTML

### L'objet `document`

`document` est l'objet racine qui représente toute ta page :

```javascript
// Afficher le titre de la page
console.log(document.title);

// Afficher l'URL de la page
console.log(document.URL);

// Accéder au body
console.log(document.body);

// Accéder au head
console.log(document.head);
```

**Important :** `document` est automatiquement disponible dès que ta page charge. Tu n'as rien à importer.

---

## 3. Sélectionner des Éléments

Avant de modifier un élément, tu dois le **sélectionner**. C'est comme dire "je veux travailler sur CETTE pièce de la maison".

### 3.1 getElementById()

Sélectionne UN élément par son attribut `id`.

**HTML :**
```html
<h1 id="main-title">Mon Titre</h1>
```

**JavaScript :**
```javascript
const titre = document.getElementById('main-title');
console.log(titre); // <h1 id="main-title">Mon Titre</h1>
console.log(titre.textContent); // "Mon Titre"
```

**Points importants :**
- L'ID doit être unique sur la page
- Retourne `null` si l'élément n'existe pas
- Pas besoin du `#` (contrairement au CSS)

**Exemple complet :**
```html
<!DOCTYPE html>
<html>
<body>
  <h1 id="titre-principal">Bonjour</h1>
  
  <script>
    // Sélectionner l'élément
    const titre = document.getElementById('titre-principal');
    
    // Vérifier qu'il existe
    if (titre) {
      console.log('Élément trouvé :', titre);
    } else {
      console.log('Élément non trouvé');
    }
  </script>
</body>
</html>
```

### 3.2 getElementsByClassName()

Sélectionne TOUS les éléments qui ont une certaine classe.

**HTML :**
```html
<p class="texte">Paragraphe 1</p>
<p class="texte">Paragraphe 2</p>
<p class="texte">Paragraphe 3</p>
```

**JavaScript :**
```javascript
const paragraphes = document.getElementsByClassName('texte');
console.log(paragraphes); // HTMLCollection de 3 éléments
console.log(paragraphes.length); // 3

// Accéder au premier élément
console.log(paragraphes[0]); // <p class="texte">Paragraphe 1</p>

// Parcourir tous les éléments
for (let i = 0; i < paragraphes.length; i++) {
  console.log(paragraphes[i].textContent);
}
```

**Points importants :**
- Retourne une **HTMLCollection** (comme un tableau mais pas exactement)
- La HTMLCollection est "live" : elle se met à jour automatiquement si tu ajoutes/supprimes des éléments
- Pas besoin du `.` (contrairement au CSS)

**Exemple : Changer tous les éléments d'une classe**
```javascript
const paragraphes = document.getElementsByClassName('texte');

for (let i = 0; i < paragraphes.length; i++) {
  paragraphes[i].style.color = 'blue';
}
```

### 3.3 getElementsByTagName()

Sélectionne TOUS les éléments d'une certaine balise.

**HTML :**
```html
<p>Premier paragraphe</p>
<p>Deuxième paragraphe</p>
<div>Une div</div>
```

**JavaScript :**
```javascript
const tousLesParagraphes = document.getElementsByTagName('p');
console.log(tousLesParagraphes.length); // 2

const toutesLesDivs = document.getElementsByTagName('div');
console.log(toutesLesDivs.length); // 1

// Sélectionner TOUS les éléments de la page
const tousLesElements = document.getElementsByTagName('*');
console.log(tousLesElements.length); // Tous les éléments HTML
```

### 3.4 querySelector() ⭐

**LA MÉTHODE MODERNE ET RECOMMANDÉE**

Sélectionne le PREMIER élément qui correspond à un sélecteur CSS.

**Syntaxe :**
```javascript
const element = document.querySelector('selecteur-css');
```

**Exemples :**
```javascript
// Par ID (utilise #)
const titre = document.querySelector('#main-title');

// Par classe (utilise .)
const premierTexte = document.querySelector('.texte');

// Par balise
const premierParagraphe = document.querySelector('p');

// Sélecteurs complexes (comme en CSS)
const lienDansHeader = document.querySelector('header a');
const inputEmail = document.querySelector('input[type="email"]');
const premierItemListe = document.querySelector('ul li:first-child');
```

**Pourquoi c'est mieux :**
- Utilise la syntaxe CSS que tu connais déjà
- Plus flexible et puissant
- Un seul outil pour tout

### 3.5 querySelectorAll() ⭐⭐

Sélectionne TOUS les éléments qui correspondent à un sélecteur CSS.

**Exemples :**
```javascript
// Tous les paragraphes
const paragraphes = document.querySelectorAll('p');

// Tous les éléments avec la classe 'texte'
const textes = document.querySelectorAll('.texte');

// Tous les liens dans un nav
const liens = document.querySelectorAll('nav a');

// Tous les inputs de type text
const inputsTexte = document.querySelectorAll('input[type="text"]');
```

**Points importants :**
- Retourne une **NodeList** (comme un tableau)
- La NodeList n'est PAS "live" (statique)
- On peut utiliser `.forEach()` directement

**Exemple complet :**
```javascript
const paragraphes = document.querySelectorAll('.texte');

// Méthode 1 : forEach
paragraphes.forEach(function(p) {
  p.style.color = 'red';
});

// Méthode 2 : boucle for classique
for (let i = 0; i < paragraphes.length; i++) {
  paragraphes[i].style.fontSize = '18px';
}

// Méthode 3 : for...of
for (const p of paragraphes) {
  console.log(p.textContent);
}
```

### 3.6 Quelle méthode utiliser ?

**Recommandation simple :**

```javascript
// ✅ UTILISE TOUJOURS querySelector et querySelectorAll
const element = document.querySelector('.ma-classe');
const elements = document.querySelectorAll('.ma-classe');

// ❌ Évite les anciennes méthodes (sauf cas très spécifiques)
// Elles sont moins flexibles et moins lisibles
```

**Tableau récapitulatif :**

| Méthode | Sélectionne | Retourne | Recommandé |
|---------|-------------|----------|------------|
| `getElementById()` | 1 élément par ID | Element ou null | ❌ |
| `getElementsByClassName()` | Tous par classe | HTMLCollection (live) | ❌ |
| `getElementsByTagName()` | Tous par balise | HTMLCollection (live) | ❌ |
| `querySelector()` | Premier qui correspond | Element ou null | ✅ |
| `querySelectorAll()` | Tous qui correspondent | NodeList (statique) | ✅ |

---

## 4. Modifier le Contenu

Une fois qu'on a sélectionné un élément, on peut modifier son contenu.

### 4.1 textContent

Modifie le **texte brut** d'un élément.

**Exemple :**
```html
<h1 id="titre">Ancien titre</h1>

<script>
  const titre = document.querySelector('#titre');
  
  // Lire le contenu
  console.log(titre.textContent); // "Ancien titre"
  
  // Modifier le contenu
  titre.textContent = 'Nouveau titre';
</script>
```

**Résultat :** Le titre devient "Nouveau titre".

**Caractéristiques :**
- Retourne/modifie TOUT le texte (même dans les enfants)
- N'interprète PAS le HTML
- Sécurisé (pas de risque XSS)

**Exemple avec HTML :**
```javascript
const titre = document.querySelector('#titre');
titre.textContent = 'Titre en <strong>gras</strong>';
// Affiche : "Titre en <strong>gras</strong>" (texte brut, pas de gras)
```

### 4.2 innerHTML

Modifie le **HTML complet** d'un élément.

**Exemple :**
```html
<div id="container">Ancien contenu</div>

<script>
  const container = document.querySelector('#container');
  
  // Lire le HTML
  console.log(container.innerHTML); // "Ancien contenu"
  
  // Modifier avec du HTML
  container.innerHTML = '<p>Nouveau <strong>contenu</strong></p>';
</script>
```

**Résultat :** Le div contient maintenant un paragraphe avec un mot en gras.

**⚠️ ATTENTION - DANGER DE SÉCURITÉ :**

```javascript
// ❌ TRÈS DANGEREUX si userInput vient d'un utilisateur
const userInput = prompt('Entrez votre nom');
element.innerHTML = userInput;
// Si l'utilisateur entre "<script>alert('Hack!')</script>", ça s'exécute !

// ✅ BON - Utilise textContent pour du contenu utilisateur
element.textContent = userInput;
```

**Règle simple :**
- Contenu que TU contrôles → `innerHTML` OK
- Contenu utilisateur → `textContent` TOUJOURS

### 4.3 innerText vs textContent

Il existe aussi `innerText`, mais **utilise toujours `textContent`**.

**Différences :**
```html
<div id="test" style="display: none;">
  Texte caché
</div>

<script>
  const div = document.querySelector('#test');
  
  console.log(div.textContent); // "Texte caché"
  console.log(div.innerText);   // "" (vide car l'élément est caché)
</script>
```

**Pourquoi textContent est mieux :**
- Plus rapide (ne calcule pas le style)
- Plus prévisible
- Standard dans tous les navigateurs modernes

### 4.4 Modifier les Attributs

Les attributs HTML (`src`, `href`, `alt`, `title`, etc.) peuvent être modifiés.

**Méthode 1 : Accès direct**
```javascript
const image = document.querySelector('img');

// Lire un attribut
console.log(image.src);
console.log(image.alt);

// Modifier un attribut
image.src = 'nouvelle-image.jpg';
image.alt = 'Description de la nouvelle image';
```

**Méthode 2 : setAttribute() et getAttribute()**
```javascript
const lien = document.querySelector('a');

// Lire
const href = lien.getAttribute('href');
console.log(href);

// Modifier
lien.setAttribute('href', 'https://example.com');
lien.setAttribute('target', '_blank');
lien.setAttribute('title', 'Aller sur Example');

// Supprimer
lien.removeAttribute('target');

// Vérifier l'existence
if (lien.hasAttribute('target')) {
  console.log('Le lien a un attribut target');
}
```

**Exemple pratique : Galerie d'images**
```html
<!DOCTYPE html>
<html>
<body>
  <img id="main-image" src="image1.jpg" alt="Image 1">
  <br>
  <button onclick="changeImage()">Image suivante</button>
  
  <script>
    let imageActuelle = 1;
    
    function changeImage() {
      imageActuelle++;
      if (imageActuelle > 3) {
        imageActuelle = 1;
      }
      
      const img = document.querySelector('#main-image');
      img.src = 'image' + imageActuelle + '.jpg';
      img.alt = 'Image ' + imageActuelle;
    }
  </script>
</body>
</html>
```

### 4.5 Modifier les Data Attributes

Les data attributes permettent de stocker des données personnalisées.

**HTML :**
```html
<div id="produit" data-id="123" data-prix="29.99" data-categorie="electronique">
  Smartphone
</div>
```

**JavaScript :**
```javascript
const produit = document.querySelector('#produit');

// Lire avec dataset
console.log(produit.dataset.id);        // "123"
console.log(produit.dataset.prix);      // "29.99"
console.log(produit.dataset.categorie); // "electronique"

// Modifier
produit.dataset.prix = "24.99";
produit.dataset.stock = "50";

// Supprimer
delete produit.dataset.categorie;
```

**Exemple pratique : Système de filtres**
```html
<div class="produits">
  <div class="produit" data-prix="10" data-categorie="livres">Livre 1</div>
  <div class="produit" data-prix="25" data-categorie="electronique">Écouteurs</div>
  <div class="produit" data-prix="5" data-categorie="livres">Livre 2</div>
</div>

<button onclick="filtrerPrix(20)">Moins de 20€</button>

<script>
  function filtrerPrix(maxPrix) {
    const produits = document.querySelectorAll('.produit');
    
    produits.forEach(function(produit) {
      const prix = parseFloat(produit.dataset.prix);
      
      if (prix <= maxPrix) {
        produit.style.display = 'block';
      } else {
        produit.style.display = 'none';
      }
    });
  }
</script>
```

---

## 5. Manipuler les Styles

### 5.1 La propriété style

Modifie le style CSS inline d'un élément.

**Syntaxe :**
```javascript
element.style.proprieteCSS = 'valeur';
```

**Exemples :**
```javascript
const box = document.querySelector('.box');

// Couleur de fond
box.style.backgroundColor = 'blue';  // background-color en camelCase

// Largeur et hauteur
box.style.width = '200px';
box.style.height = '200px';

// Texte
box.style.color = 'white';
box.style.fontSize = '20px';
box.style.textAlign = 'center';

// Bordure
box.style.border = '2px solid red';
box.style.borderRadius = '10px';

// Position
box.style.position = 'absolute';
box.style.top = '50px';
box.style.left = '100px';

// Affichage
box.style.display = 'none';  // Cacher
box.style.display = 'block'; // Afficher
```

**Important - Conversion des noms CSS :**
```
CSS             JavaScript
---             ----------
background-color → backgroundColor
font-size       → fontSize
border-radius   → borderRadius
text-align      → textAlign
```

**Règle :** Enlève les `-` et mets en majuscule la lettre qui suit.

### 5.2 classList - La méthode professionnelle ⭐

Au lieu de modifier `style` directement, on préfère **ajouter/supprimer des classes CSS**.

**Pourquoi ?**
- Plus propre et maintenable
- Sépare le JS du CSS
- Plus performant
- Réutilisable

**CSS :**
```css
.active {
  background-color: green;
  color: white;
  font-weight: bold;
}

.hidden {
  display: none;
}

.large {
  font-size: 24px;
}
```

**JavaScript :**
```javascript
const element = document.querySelector('.mon-element');

// Ajouter une classe
element.classList.add('active');

// Supprimer une classe
element.classList.remove('hidden');

// Basculer une classe (toggle)
// Si elle existe, la retire. Sinon, l'ajoute.
element.classList.toggle('large');

// Vérifier si une classe existe
if (element.classList.contains('active')) {
  console.log('L\'élément est actif');
}

// Remplacer une classe
element.classList.replace('old-class', 'new-class');
```

**Exemple pratique : Menu mobile**
```html
<!DOCTYPE html>
<html>
<head>
  <style>
    .menu {
      display: none;
    }
    
    .menu.open {
      display: block;
    }
    
    .menu-btn.active {
      background-color: #333;
      color: white;
    }
  </style>
</head>
<body>
  <button class="menu-btn" onclick="toggleMenu()">☰ Menu</button>
  <nav class="menu">
    <a href="#">Accueil</a>
    <a href="#">À propos</a>
    <a href="#">Contact</a>
  </nav>
  
  <script>
    function toggleMenu() {
      const menu = document.querySelector('.menu');
      const btn = document.querySelector('.menu-btn');
      
      menu.classList.toggle('open');
      btn.classList.toggle('active');
    }
  </script>
</body>
</html>
```

### 5.3 Ajouter plusieurs classes

```javascript
const element = document.querySelector('.element');

// Ajouter plusieurs classes d'un coup
element.classList.add('classe1', 'classe2', 'classe3');

// Supprimer plusieurs classes
element.classList.remove('classe1', 'classe2');
```

### 5.4 getComputedStyle()

Obtenir le style calculé final d'un élément (incluant le CSS externe).

```javascript
const element = document.querySelector('.element');

// Style calculé final
const styles = window.getComputedStyle(element);

console.log(styles.backgroundColor);  // Couleur de fond réelle
console.log(styles.fontSize);         // Taille de police réelle
console.log(styles.width);            // Largeur réelle en pixels

// Même si défini en CSS, pas en inline
```

---

## 6. Créer et Insérer des Éléments

### 6.1 createElement()

Crée un nouvel élément HTML en mémoire.

**Syntaxe :**
```javascript
const element = document.createElement('nomBalise');
```

**Exemple :**
```javascript
// Créer un paragraphe
const p = document.createElement('p');

// Ajouter du texte
p.textContent = 'Ceci est un nouveau paragraphe';

// Ajouter une classe
p.classList.add('texte');

// Ajouter un style
p.style.color = 'blue';

console.log(p); // <p class="texte" style="color: blue;">Ceci est...</p>

// ⚠️ L'élément existe mais n'est PAS encore dans la page !
```

### 6.2 appendChild()

Ajoute un élément comme dernier enfant d'un parent.

```javascript
const container = document.querySelector('#container');
const p = document.createElement('p');
p.textContent = 'Nouveau paragraphe';

// Ajouter le paragraphe au container
container.appendChild(p);

// Maintenant le paragraphe est visible dans la page
```

**Exemple complet :**
```html
<!DOCTYPE html>
<html>
<body>
  <div id="liste-taches"></div>
  <button onclick="ajouterTache()">Ajouter tâche</button>
  
  <script>
    function ajouterTache() {
      // 1. Créer un élément div
      const div = document.createElement('div');
      
      // 2. Ajouter une classe
      div.classList.add('tache');
      
      // 3. Ajouter du contenu
      div.textContent = 'Nouvelle tâche ' + Date.now();
      
      // 4. Ajouter un style
      div.style.padding = '10px';
      div.style.margin = '5px';
      div.style.backgroundColor = '#f0f0f0';
      
      // 5. Insérer dans la page
      const liste = document.querySelector('#liste-taches');
      liste.appendChild(div);
    }
  </script>
</body>
</html>
```

### 6.3 prepend(), before(), after()

Méthodes modernes d'insertion.

```javascript
const container = document.querySelector('#container');
const p = document.createElement('p');
p.textContent = 'Nouveau paragraphe';

// Insérer comme PREMIER enfant
container.prepend(p);

// Insérer AVANT le container (comme frère)
container.before(p);

// Insérer APRÈS le container (comme frère)
container.after(p);
```

**Diagramme :**
```
        before(p)
            ↓
    [CONTAINER]
     ↓       ↑
prepend(p)  appendChild(p)
            ↓
        after(p)
```

### 6.4 insertBefore()

Méthode ancienne mais parfois utile.

```javascript
const liste = document.querySelector('#liste');
const nouveauItem = document.createElement('li');
nouveauItem.textContent = 'Nouvel item';

// Insérer avant un élément spécifique
const premierItem = liste.querySelector('li:first-child');
liste.insertBefore(nouveauItem, premierItem);
```

### 6.5 Créer des structures complexes

**Exemple : Créer une carte de produit**
```javascript
function creerCarteProduit(nom, prix, image) {
  // Créer le container principal
  const carte = document.createElement('div');
  carte.classList.add('produit-carte');
  
  // Créer l'image
  const img = document.createElement('img');
  img.src = image;
  img.alt = nom;
  
  // Créer le titre
  const titre = document.createElement('h3');
  titre.textContent = nom;
  
  // Créer le prix
  const prixElement = document.createElement('p');
  prixElement.textContent = prix + '€';
  prixElement.classList.add('prix');
  
  // Créer le bouton
  const bouton = document.createElement('button');
  bouton.textContent = 'Ajouter au panier';
  bouton.onclick = function() {
    alert('Ajouté : ' + nom);
  };
  
  // Assembler le tout
  carte.appendChild(img);
  carte.appendChild(titre);
  carte.appendChild(prixElement);
  carte.appendChild(bouton);
  
  return carte;
}

// Utilisation
const produit = creerCarteProduit('Smartphone', 299, 'phone.jpg');
document.querySelector('#produits').appendChild(produit);
```

### 6.6 innerHTML vs createElement - Quelle méthode ?

**innerHTML - Plus rapide à écrire :**
```javascript
container.innerHTML = `
  <div class="produit">
    <h3>${nom}</h3>
    <p>${prix}€</p>
    <button>Acheter</button>
  </div>
`;
```

**createElement - Plus sûr et performant :**
```javascript
const div = document.createElement('div');
div.className = 'produit';
// ... (code précédent)
container.appendChild(div);
```

**Quand utiliser quoi :**
- `innerHTML` → Contenu statique, template simple
- `createElement` → Contenu dynamique, événements complexes, performance

---

## 7. Supprimer des Éléments

### 7.1 remove()

Méthode moderne pour supprimer un élément.

```javascript
const element = document.querySelector('#element-a-supprimer');
element.remove();

// L'élément disparaît de la page
```

**Exemple pratique :**
```html
<div id="notification">
  Ceci est une notification
  <button onclick="fermer()">×</button>
</div>

<script>
  function fermer() {
    const notif = document.querySelector('#notification');
    notif.remove();
  }
</script>
```

### 7.2 removeChild()

Méthode ancienne (mais parfois nécessaire).

```javascript
const parent = document.querySelector('#parent');
const enfant = document.querySelector('#enfant');

parent.removeChild(enfant);
```

### 7.3 Vider un élément

```javascript
const container = document.querySelector('#container');

// Méthode 1 : innerHTML
container.innerHTML = '';

// Méthode 2 : Boucle
while (container.firstChild) {
  container.removeChild(container.firstChild);
}

// Méthode 3 : replaceChildren (moderne)
container.replaceChildren();
```

### 7.4 Exemple : Liste avec suppression

```html
<!DOCTYPE html>
<html>
<body>
  <input type="text" id="item-input" placeholder="Nouvel élément">
  <button onclick="ajouter()">Ajouter</button>
  <ul id="liste"></ul>
  
  <script>
    function ajouter() {
      const input = document.querySelector('#item-input');
      const texte = input.value.trim();
      
      if (texte === '') {
        alert('Entrez un texte');
        return;
      }
      
      // Créer le li
      const li = document.createElement('li');
      li.textContent = texte;
      
      // Créer le bouton supprimer
      const btnSupprimer = document.createElement('button');
      btnSupprimer.textContent = 'Supprimer';
      btnSupprimer.onclick = function() {
        li.remove();
      };
      
      li.appendChild(btnSupprimer);
      
      // Ajouter à la liste
      document.querySelector('#liste').appendChild(li);
      
      // Vider l'input
      input.value = '';
    }
  </script>
</body>
</html>
```

---

## 8. Les Événements

Les événements permettent à ton code de **réagir** aux actions de l'utilisateur.

### 8.1 Qu'est-ce qu'un événement ?

Un événement, c'est quelque chose qui se passe :
- L'utilisateur clique sur un bouton
- Il tape au clavier
- Il bouge la souris
- La page finit de charger
- Un formulaire est soumis

**Sans événements, ton site serait statique et non interactif.**

### 8.2 addEventListener() - LA méthode

**Syntaxe :**
```javascript
element.addEventListener('typeEvenement', function() {
  // Code à exécuter
});
```

**Premier exemple :**
```html
<button id="mon-bouton">Clique-moi</button>

<script>
  const bouton = document.querySelector('#mon-bouton');
  
  bouton.addEventListener('click', function() {
    alert('Bouton cliqué !');
  });
</script>
```

### 8.3 Types d'événements courants

#### Événements de souris

```javascript
const element = document.querySelector('.element');

// Clic simple
element.addEventListener('click', function() {
  console.log('Clic');
});

// Double-clic
element.addEventListener('dblclick', function() {
  console.log('Double-clic');
});

// Souris entre dans l'élément
element.addEventListener('mouseenter', function() {
  console.log('Souris entrée');
});

// Souris sort de l'élément
element.addEventListener('mouseleave', function() {
  console.log('Souris sortie');
});

// Souris bouge sur l'élément
element.addEventListener('mousemove', function(e) {
  console.log('Position:', e.clientX, e.clientY);
});

// Clic maintenu
element.addEventListener('mousedown', function() {
  console.log('Bouton pressé');
});

// Clic relâché
element.addEventListener('mouseup', function() {
  console.log('Bouton relâché');
});
```

#### Événements de clavier

```javascript
const input = document.querySelector('input');

// Touche pressée
input.addEventListener('keydown', function(e) {
  console.log('Touche pressée:', e.key);
});

// Touche relâchée
input.addEventListener('keyup', function(e) {
  console.log('Touche relâchée:', e.key);
});

// Caractère entré (après traitement du clavier)
input.addEventListener('keypress', function(e) {
  console.log('Caractère:', e.key);
});
```

#### Événements de formulaire

```javascript
const input = document.querySelector('input');
const form = document.querySelector('form');

// Valeur change (en temps réel)
input.addEventListener('input', function() {
  console.log('Nouvelle valeur:', input.value);
});

// Valeur change (après blur)
input.addEventListener('change', function() {
  console.log('Changement finalisé:', input.value);
});

// Input reçoit le focus
input.addEventListener('focus', function() {
  console.log('Input sélectionné');
});

// Input perd le focus
input.addEventListener('blur', function() {
  console.log('Input déselectionné');
});

// Formulaire soumis
form.addEventListener('submit', function(e) {
  e.preventDefault(); // Empêche le rechargement
  console.log('Formulaire soumis');
});
```

#### Événements de fenêtre

```javascript
// Page chargée
window.addEventListener('load', function() {
  console.log('Page complètement chargée');
});

// DOM chargé (avant les images)
document.addEventListener('DOMContentLoaded', function() {
  console.log('DOM prêt');
});

// Scroll
window.addEventListener('scroll', function() {
  console.log('Position scroll:', window.scrollY);
});

// Redimensionnement
window.addEventListener('resize', function() {
  console.log('Nouvelle taille:', window.innerWidth, window.innerHeight);
});
```

### 8.4 L'objet Event

Chaque événement passe un objet `event` à ta fonction.

```javascript
element.addEventListener('click', function(event) {
  // ou souvent écrit : function(e)
  
  console.log(event.type);           // Type d'événement ("click")
  console.log(event.target);         // Élément qui a déclenché l'événement
  console.log(event.currentTarget);  // Élément qui écoute l'événement
  console.log(event.timeStamp);      // Moment de l'événement
  
  // Pour les clics de souris
  console.log(event.clientX);        // Position X de la souris
  console.log(event.clientY);        // Position Y de la souris
  console.log(event.button);         // Quel bouton (0=gauche, 1=milieu, 2=droit)
  
  // Pour le clavier
  console.log(event.key);            // Touche pressée
  console.log(event.code);           // Code physique de la touche
  console.log(event.ctrlKey);        // Ctrl est pressé ?
  console.log(event.shiftKey);       // Shift est pressé ?
  console.log(event.altKey);         // Alt est pressé ?
});
```

### 8.5 preventDefault()

Empêche le comportement par défaut.

**Exemple 1 : Empêcher la soumission d'un formulaire**
```javascript
const form = document.querySelector('form');

form.addEventListener('submit', function(e) {
  e.preventDefault(); // La page ne recharge pas
  
  // Traiter les données ici
  console.log('Formulaire traité en JavaScript');
});
```

**Exemple 2 : Empêcher l'ouverture d'un lien**
```javascript
const lien = document.querySelector('a');

lien.addEventListener('click', function(e) {
  e.preventDefault(); // Le lien ne s'ouvre pas
  
  if (confirm('Êtes-vous sûr de vouloir quitter ?')) {
    window.location.href = lien.href;
  }
});
```

### 8.6 stopPropagation()

Empêche la propagation de l'événement.

**Le problème de la propagation :**
```html
<div id="parent">
  <button id="enfant">Clique</button>
</div>

<script>
  document.querySelector('#parent').addEventListener('click', function() {
    console.log('Clic sur parent');
  });
  
  document.querySelector('#enfant').addEventListener('click', function() {
    console.log('Clic sur enfant');
  });
  
  // Si tu cliques sur le bouton, tu verras :
  // "Clic sur enfant"
  // "Clic sur parent"
  // L'événement "remonte" l'arbre DOM (propagation)
</script>
```

**Solution :**
```javascript
document.querySelector('#enfant').addEventListener('click', function(e) {
  e.stopPropagation(); // Arrête la propagation
  console.log('Clic sur enfant');
});

// Maintenant seul "Clic sur enfant" s'affiche
```

### 8.7 Retirer un événement

```javascript
function handleClick() {
  console.log('Cliqué');
}

const bouton = document.querySelector('button');

// Ajouter
bouton.addEventListener('click', handleClick);

// Retirer
bouton.removeEventListener('click', handleClick);
```

**⚠️ Important :** Pour retirer un événement, tu dois utiliser la MÊME fonction (pas une anonyme).

```javascript
// ❌ NE MARCHE PAS
bouton.addEventListener('click', function() { console.log('Hi'); });
bouton.removeEventListener('click', function() { console.log('Hi'); });
// Ce sont 2 fonctions différentes !

// ✅ MARCHE
function greet() { console.log('Hi'); }
bouton.addEventListener('click', greet);
bouton.removeEventListener('click', greet);
```

### 8.8 Événement une seule fois

```javascript
bouton.addEventListener('click', function() {
  console.log('Ceci ne s\'exécutera qu\'une fois');
}, { once: true });
```

### 8.9 Exemple complet : Compteur de clics

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    .compteur {
      font-size: 48px;
      text-align: center;
      margin: 20px;
    }
    
    button {
      font-size: 24px;
      padding: 10px 20px;
      margin: 5px;
    }
  </style>
</head>
<body>
  <div class="compteur" id="compteur">0</div>
  <button id="incrementer">+1</button>
  <button id="decrementer">-1</button>
  <button id="reset">Reset</button>
  
  <script>
    let compte = 0;
    
    const affichage = document.querySelector('#compteur');
    const btnIncrementer = document.querySelector('#incrementer');
    const btnDecrementer = document.querySelector('#decrementer');
    const btnReset = document.querySelector('#reset');
    
    function mettreAJourAffichage() {
      affichage.textContent = compte;
      
      // Changer la couleur selon la valeur
      if (compte > 0) {
        affichage.style.color = 'green';
      } else if (compte < 0) {
        affichage.style.color = 'red';
      } else {
        affichage.style.color = 'black';
      }
    }
    
    btnIncrementer.addEventListener('click', function() {
      compte++;
      mettreAJourAffichage();
    });
    
    btnDecrementer.addEventListener('click', function() {
      compte--;
      mettreAJourAffichage();
    });
    
    btnReset.addEventListener('click', function() {
      compte = 0;
      mettreAJourAffichage();
    });
  </script>
</body>
</html>
```

---

## 9. Event Delegation

L'event delegation est une technique **cruciale** pour des applications performantes.

### 9.1 Le problème

Imagine une liste de 1000 items. Chaque item a un bouton "Supprimer".

**❌ Approche naïve (INEFFICACE) :**
```javascript
const boutons = document.querySelectorAll('.supprimer');

// Ajouter un listener à CHAQUE bouton
boutons.forEach(function(bouton) {
  bouton.addEventListener('click', function() {
    // Supprimer l'item
  });
});

// Problèmes :
// 1. 1000 listeners = beaucoup de mémoire
// 2. Si on ajoute un nouvel item dynamiquement, il n'a PAS de listener
```

### 9.2 La solution : Event Delegation

**Au lieu d'écouter chaque bouton, écoute le PARENT une seule fois.**

```javascript
const liste = document.querySelector('#liste');

// UN SEUL listener sur le parent
liste.addEventListener('click', function(e) {
  // Vérifier si le clic vient d'un bouton "supprimer"
  if (e.target.classList.contains('supprimer')) {
    // Supprimer l'item
    e.target.closest('li').remove();
  }
});

// Avantages :
// 1. Un seul listener = moins de mémoire
// 2. Nouveaux items ajoutés dynamiquement fonctionnent automatiquement
```

### 9.3 Comprendre event.target vs event.currentTarget

```html
<div id="parent">
  <button class="enfant">Cliquez-moi</button>
</div>

<script>
  document.querySelector('#parent').addEventListener('click', function(e) {
    console.log('target:', e.target);           // <button>
    console.log('currentTarget:', e.currentTarget); // <div id="parent">
  });
</script>
```

- `e.target` = Élément qui a VRAIMENT été cliqué
- `e.currentTarget` = Élément qui ÉCOUTE l'événement

### 9.4 La méthode closest()

`closest()` remonte l'arbre DOM jusqu'à trouver un élément correspondant.

```html
<ul id="liste">
  <li>
    <span>Item 1</span>
    <button class="supprimer">X</button>
  </li>
</ul>

<script>
  document.querySelector('#liste').addEventListener('click', function(e) {
    if (e.target.classList.contains('supprimer')) {
      // Trouver le <li> parent le plus proche
      const li = e.target.closest('li');
      li.remove();
    }
  });
</script>
```

### 9.5 Exemple complet : To-Do List avec Event Delegation

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    .tache {
      padding: 10px;
      margin: 5px 0;
      background: #f0f0f0;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    
    .tache.completee {
      text-decoration: line-through;
      opacity: 0.6;
    }
    
    button {
      padding: 5px 10px;
      margin-left: 10px;
    }
  </style>
</head>
<body>
  <input type="text" id="nouvelle-tache" placeholder="Nouvelle tâche">
  <button id="ajouter">Ajouter</button>
  
  <div id="liste-taches"></div>
  
  <script>
    const input = document.querySelector('#nouvelle-tache');
    const btnAjouter = document.querySelector('#ajouter');
    const liste = document.querySelector('#liste-taches');
    
    // Ajouter une tâche
    btnAjouter.addEventListener('click', ajouterTache);
    input.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') {
        ajouterTache();
      }
    });
    
    function ajouterTache() {
      const texte = input.value.trim();
      if (texte === '') return;
      
      const div = document.createElement('div');
      div.className = 'tache';
      div.innerHTML = `
        <span class="texte">${texte}</span>
        <div>
          <button class="completer">✓</button>
          <button class="supprimer">✗</button>
        </div>
      `;
      
      liste.appendChild(div);
      input.value = '';
    }
    
    // Event delegation sur TOUTE la liste
    liste.addEventListener('click', function(e) {
      const tache = e.target.closest('.tache');
      if (!tache) return;
      
      // Bouton completer
      if (e.target.classList.contains('completer')) {
        tache.classList.toggle('completee');
      }
      
      // Bouton supprimer
      if (e.target.classList.contains('supprimer')) {
        if (confirm('Supprimer cette tâche ?')) {
          tache.remove();
        }
      }
    });
  </script>
</body>
</html>
```

---

## 10. Formulaires et Validation

Les formulaires sont partout sur le web. Maîtrise-les parfaitement.

### 10.1 Accéder aux valeurs d'un formulaire

```html
<form id="mon-form">
  <input type="text" name="nom" id="nom">
  <input type="email" name="email" id="email">
  <input type="password" name="password" id="password">
  <button type="submit">Envoyer</button>
</form>

<script>
  const form = document.querySelector('#mon-form');
  
  form.addEventListener('submit', function(e) {
    e.preventDefault(); // Important !
    
    // Méthode 1 : Par getElementById
    const nom = document.querySelector('#nom').value;
    const email = document.querySelector('#email').value;
    const password = document.querySelector('#password').value;
    
    // Méthode 2 : FormData (plus moderne)
    const formData = new FormData(form);
    const nom2 = formData.get('nom');
    const email2 = formData.get('email');
    
    console.log(nom, email, password);
  });
</script>
```

### 10.2 Validation en temps réel

```html
<form>
  <div>
    <label>Email:</label>
    <input type="email" id="email">
    <span id="email-error" style="color: red;"></span>
  </div>
  
  <div>
    <label>Mot de passe:</label>
    <input type="password" id="password">
    <span id="password-error" style="color: red;"></span>
  </div>
  
  <button type="submit" id="submit-btn" disabled>S'inscrire</button>
</form>

<script>
  const emailInput = document.querySelector('#email');
  const passwordInput = document.querySelector('#password');
  const emailError = document.querySelector('#email-error');
  const passwordError = document.querySelector('#password-error');
  const submitBtn = document.querySelector('#submit-btn');
  
  let emailValide = false;
  let passwordValide = false;
  
  // Validation email
  emailInput.addEventListener('input', function() {
    const email = emailInput.value;
    
    if (email.length === 0) {
      emailError.textContent = '';
      emailInput.style.borderColor = '';
      emailValide = false;
    } else if (!email.includes('@') || !email.includes('.')) {
      emailError.textContent = 'Email invalide';
      emailInput.style.borderColor = 'red';
      emailValide = false;
    } else {
      emailError.textContent = '✓';
      emailError.style.color = 'green';
      emailInput.style.borderColor = 'green';
      emailValide = true;
    }
    
    verifierFormulaire();
  });
  
  // Validation mot de passe
  passwordInput.addEventListener('input', function() {
    const password = passwordInput.value;
    
    if (password.length === 0) {
      passwordError.textContent = '';
      passwordInput.style.borderColor = '';
      passwordValide = false;
    } else if (password.length < 8) {
      passwordError.textContent = 'Minimum 8 caractères';
      passwordInput.style.borderColor = 'red';
      passwordValide = false;
    } else {
      passwordError.textContent = '✓';
      passwordError.style.color = 'green';
      passwordInput.style.borderColor = 'green';
      passwordValide = true;
    }
    
    verifierFormulaire();
  });
  
  function verifierFormulaire() {
    if (emailValide && passwordValide) {
      submitBtn.disabled = false;
    } else {
      submitBtn.disabled = true;
    }
  }
</script>
```

### 10.3 Checkbox et Radio Buttons

```html
<form id="preferences">
  <!-- Checkbox -->
  <label>
    <input type="checkbox" name="newsletter" value="oui"> Newsletter
  </label>
  
  <!-- Radio buttons -->
  <div>
    <label><input type="radio" name="theme" value="clair"> Clair</label>
    <label><input type="radio" name="theme" value="sombre"> Sombre</label>
  </div>
  
  <button type="submit">Sauvegarder</button>
</form>

<script>
  document.querySelector('#preferences').addEventListener('submit', function(e) {
    e.preventDefault();
    
    // Checkbox
    const newsletter = document.querySelector('input[name="newsletter"]');
    console.log('Newsletter:', newsletter.checked); // true ou false
    
    // Radio button
    const theme = document.querySelector('input[name="theme"]:checked');
    console.log('Thème:', theme ? theme.value : 'Aucun');
  });
</script>
```

### 10.4 Select (menu déroulant)

```html
<select id="pays">
  <option value="">Choisissez un pays</option>
  <option value="fr">France</option>
  <option value="be">Belgique</option>
  <option value="ch">Suisse</option>
</select>

<script>
  const select = document.querySelector('#pays');
  
  select.addEventListener('change', function() {
    console.log('Pays choisi:', select.value);
    console.log('Texte affiché:', select.options[select.selectedIndex].text);
  });
</script>
```

### 10.5 Exemple complet : Formulaire d'inscription

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background: #f0f0f0;
    }
    
    form {
      max-width: 400px;
      margin: 0 auto;
      background: white;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    
    .form-group {
      margin-bottom: 20px;
    }
    
    label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }
    
    input {
      width: 100%;
      padding: 10px;
      border: 2px solid #ddd;
      border-radius: 5px;
      font-size: 16px;
    }
    
    input.valide {
      border-color: green;
    }
    
    input.invalide {
      border-color: red;
    }
    
    .erreur {
      color: red;
      font-size: 14px;
      margin-top: 5px;
    }
    
    button {
      width: 100%;
      padding: 12px;
      background: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
      font-size: 16px;
      cursor: pointer;
    }
    
    button:hover {
      background: #0056b3;
    }
    
    button:disabled {
      background: #ccc;
      cursor: not-allowed;
    }
    
    .message-succes {
      padding: 15px;
      background: #d4edda;
      color: #155724;
      border-radius: 5px;
      margin-bottom: 20px;
      display: none;
    }
  </style>
</head>
<body>
  <form id="inscription">
    <h2 style="text-align: center; margin-bottom: 30px;">Inscription</h2>
    
    <div class="message-succes" id="message-succes"></div>
    
    <div class="form-group">
      <label for="nom">Nom complet</label>
      <input type="text" id="nom" required>
      <div class="erreur" id="erreur-nom"></div>
    </div>
    
    <div class="form-group">
      <label for="email">Email</label>
      <input type="email" id="email" required>
      <div class="erreur" id="erreur-email"></div>
    </div>
    
    <div class="form-group">
      <label for="password">Mot de passe</label>
      <input type="password" id="password" required>
      <div class="erreur" id="erreur-password"></div>
    </div>
    
    <div class="form-group">
      <label for="password-confirm">Confirmer le mot de passe</label>
      <input type="password" id="password-confirm" required>
      <div class="erreur" id="erreur-password-confirm"></div>
    </div>
    
    <button type="submit" id="btn-submit">S'inscrire</button>
  </form>
  
  <script>
    const form = document.querySelector('#inscription');
    const nomInput = document.querySelector('#nom');
    const emailInput = document.querySelector('#email');
    const passwordInput = document.querySelector('#password');
    const passwordConfirmInput = document.querySelector('#password-confirm');
    
    const erreurNom = document.querySelector('#erreur-nom');
    const erreurEmail = document.querySelector('#erreur-email');
    const erreurPassword = document.querySelector('#erreur-password');
    const erreurPasswordConfirm = document.querySelector('#erreur-password-confirm');
    
    const btnSubmit = document.querySelector('#btn-submit');
    const messageSucces = document.querySelector('#message-succes');
    
    // État de validation
    let validations = {
      nom: false,
      email: false,
      password: false,
      passwordConfirm: false
    };
    
    // Validation nom
    nomInput.addEventListener('input', function() {
      const nom = nomInput.value.trim();
      
      if (nom.length < 2) {
        afficherErreur(nomInput, erreurNom, 'Le nom doit contenir au moins 2 caractères');
        validations.nom = false;
      } else if (!/^[a-zA-ZÀ-ÿ\s]+$/.test(nom)) {
        afficherErreur(nomInput, erreurNom, 'Le nom ne doit contenir que des lettres');
        validations.nom = false;
      } else {
        afficherSucces(nomInput, erreurNom);
        validations.nom = true;
      }
      
      verifierFormulaire();
    });
    
    // Validation email
    emailInput.addEventListener('input', function() {
      const email = emailInput.value.trim();
      const regexEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      
      if (!regexEmail.test(email)) {
        afficherErreur(emailInput, erreurEmail, 'Email invalide');
        validations.email = false;
      } else {
        afficherSucces(emailInput, erreurEmail);
        validations.email = true;
      }
      
      verifierFormulaire();
    });
    
    // Validation mot de passe
    passwordInput.addEventListener('input', function() {
      const password = passwordInput.value;
      
      if (password.length < 8) {
        afficherErreur(passwordInput, erreurPassword, 'Minimum 8 caractères');
        validations.password = false;
      } else if (!/[A-Z]/.test(password)) {
        afficherErreur(passwordInput, erreurPassword, 'Au moins une majuscule');
        validations.password = false;
      } else if (!/[0-9]/.test(password)) {
        afficherErreur(passwordInput, erreurPassword, 'Au moins un chiffre');
        validations.password = false;
      } else {
        afficherSucces(passwordInput, erreurPassword);
        validations.password = true;
      }
      
      // Revérifier la confirmation si elle a déjà été remplie
      if (passwordConfirmInput.value.length > 0) {
        passwordConfirmInput.dispatchEvent(new Event('input'));
      }
      
      verifierFormulaire();
    });
    
    // Validation confirmation mot de passe
    passwordConfirmInput.addEventListener('input', function() {
      const password = passwordInput.value;
      const passwordConfirm = passwordConfirmInput.value;
      
      if (passwordConfirm !== password) {
        afficherErreur(passwordConfirmInput, erreurPasswordConfirm, 'Les mots de passe ne correspondent pas');
        validations.passwordConfirm = false;
      } else {
        afficherSucces(passwordConfirmInput, erreurPasswordConfirm);
        validations.passwordConfirm = true;
      }
      
      verifierFormulaire();
    });
    
    // Soumission du formulaire
    form.addEventListener('submit', function(e) {
      e.preventDefault();
      
      if (toutValide()) {
        // Simuler l'envoi au serveur
        btnSubmit.disabled = true;
        btnSubmit.textContent = 'Inscription en cours...';
        
        setTimeout(function() {
          messageSucces.textContent = '✓ Inscription réussie ! Bienvenue ' + nomInput.value;
          messageSucces.style.display = 'block';
          
          form.reset();
          resetValidations();
          
          btnSubmit.disabled = false;
          btnSubmit.textContent = 'S\'inscrire';
          
          setTimeout(function() {
            messageSucces.style.display = 'none';
          }, 5000);
        }, 1500);
      }
    });
    
    // Fonctions utilitaires
    function afficherErreur(input, zoneErreur, message) {
      input.classList.remove('valide');
      input.classList.add('invalide');
      zoneErreur.textContent = message;
    }
    
    function afficherSucces(input, zoneErreur) {
      input.classList.remove('invalide');
      input.classList.add('valide');
      zoneErreur.textContent = '';
    }
    
    function verifierFormulaire() {
      btnSubmit.disabled = !toutValide();
    }
    
    function toutValide() {
      return validations.nom && 
             validations.email && 
             validations.password && 
             validations.passwordConfirm;
    }
    
    function resetValidations() {
      validations = {
        nom: false,
        email: false,
        password: false,
        passwordConfirm: false
      };
      
      [nomInput, emailInput, passwordInput, passwordConfirmInput].forEach(function(input) {
        input.classList.remove('valide', 'invalide');
      });
      
      [erreurNom, erreurEmail, erreurPassword, erreurPasswordConfirm].forEach(function(zone) {
        zone.textContent = '';
      });
    }
  </script>
</body>
</html>
```

---

## 11. Le localStorage

Le `localStorage` permet de **sauvegarder des données** dans le navigateur.

### 11.1 Pourquoi localStorage ?

Par défaut, les variables JavaScript disparaissent quand tu recharges la page :

```javascript
let compteur = 0;
// Si tu recharges → compteur revient à 0
```

Avec `localStorage`, les données **persistent** :

```javascript
localStorage.setItem('compteur', '0');
// Même après rechargement, la donnée est toujours là
```

### 11.2 Les 4 méthodes essentielles

```javascript
// 1. Sauvegarder une donnée
localStorage.setItem('nom', 'Alice');
localStorage.setItem('age', '25');

// 2. Lire une donnée
const nom = localStorage.getItem('nom');
console.log(nom); // "Alice"

// 3. Supprimer une donnée
localStorage.removeItem('age');

// 4. Tout effacer
localStorage.clear();
```

**Important :** localStorage ne stocke QUE des chaînes de caractères (strings).

### 11.3 Sauvegarder des nombres

```javascript
// ❌ ATTENTION
localStorage.setItem('age', 25);
const age = localStorage.getItem('age');
console.log(typeof age); // "string" (pas "number")
console.log(age + 5);    // "255" (concaténation, pas addition)

// ✅ SOLUTION : Convertir
localStorage.setItem('age', '25');
const ageString = localStorage.getItem('age');
const ageNumber = parseInt(ageString);
console.log(ageNumber + 5); // 30
```

### 11.4 Sauvegarder des objets et tableaux

```javascript
// Objet
const utilisateur = {
  nom: 'Alice',
  age: 25,
  email: 'alice@example.com'
};

// ❌ NE MARCHE PAS
localStorage.setItem('user', utilisateur);
// Sauvegarde "[object Object]"

// ✅ SOLUTION : JSON.stringify()
localStorage.setItem('user', JSON.stringify(utilisateur));

// Pour lire
const userString = localStorage.getItem('user');
const userObject = JSON.parse(userString);
console.log(userObject.nom); // "Alice"
```

**Même principe pour les tableaux :**
```javascript
const taches = ['Faire courses', 'Coder', 'Sport'];

// Sauvegarder
localStorage.setItem('taches', JSON.stringify(taches));

// Lire
const tachesString = localStorage.getItem('taches');
const tachesArray = JSON.parse(tachesString);
console.log(tachesArray[0]); // "Faire courses"
```

### 11.5 Vérifier l'existence d'une donnée

```javascript
const nom = localStorage.getItem('nom');

if (nom === null) {
  console.log('Pas de nom sauvegardé');
} else {
  console.log('Nom trouvé:', nom);
}

// Ou avec valeur par défaut
const theme = localStorage.getItem('theme') || 'clair';
console.log(theme); // "clair" si pas de donnée
```

### 11.6 Exemple : Compteur persistant

```html
<!DOCTYPE html>
<html>
<body>
  <h1>Compteur: <span id="compteur">0</span></h1>
  <button onclick="incrementer()">+1</button>
  <button onclick="reset()">Reset</button>
  
  <script>
    const affichage = document.querySelector('#compteur');
    
    // Charger la valeur au démarrage
    let compteur = parseInt(localStorage.getItem('compteur')) || 0;
    affichage.textContent = compteur;
    
    function incrementer() {
      compteur++;
      affichage.textContent = compteur;
      localStorage.setItem('compteur', compteur);
    }
    
    function reset() {
      compteur = 0;
      affichage.textContent = compteur;
      localStorage.setItem('compteur', compteur);
    }
  </script>
</body>
</html>
```

**Recharge la page → Le compteur garde sa valeur !**

### 11.7 Limites du localStorage

1. **Capacité limitée** : ~5-10 MB selon les navigateurs
2. **Synchrone** : Peut ralentir si tu sauvegardes beaucoup
3. **Pas sécurisé** : N'y stocke JAMAIS de mots de passe ou données sensibles
4. **Par domaine** : Chaque site a son propre localStorage

---

## 12. Projet Complet : To-Do List

Maintenant on combine TOUT ce qu'on a appris pour créer une vraie application.

### 12.1 Cahier des charges

Notre To-Do List doit :
- ✅ Ajouter des tâches
- ✅ Marquer comme complétée
- ✅ Supprimer des tâches
- ✅ Filtrer (Toutes / Actives / Complétées)
- ✅ Compteur de tâches
- ✅ Sauvegarder dans localStorage
- ✅ Design professionnel

### 12.2 Code complet

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ma To-Do List</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      padding: 20px;
    }
    
    .container {
      max-width: 600px;
      margin: 0 auto;
      background: white;
      border-radius: 15px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
      overflow: hidden;
    }
    
    .header {
      background: #667eea;
      color: white;
      padding: 30px;
      text-align: center;
    }
    
    .header h1 {
      font-size: 2.5em;
      margin-bottom: 10px;
    }
    
    .add-task {
      padding: 20px;
      border-bottom: 1px solid #eee;
    }
    
    .input-group {
      display: flex;
      gap: 10px;
    }
    
    #taskInput {
      flex: 1;
      padding: 12px;
      border: 2px solid #ddd;
      border-radius: 5px;
      font-size: 16px;
    }
    
    #taskInput:focus {
      outline: none;
      border-color: #667eea;
    }
    
    #addBtn {
      padding: 12px 24px;
      background: #667eea;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
      font-weight: bold;
    }
    
    #addBtn:hover {
      background: #5568d3;
    }
    
    .filters {
      padding: 15px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #f5f5f5;
      border-bottom: 1px solid #eee;
    }
    
    .filter-buttons {
      display: flex;
      gap: 10px;
    }
    
    .filter-btn {
      padding: 8px 16px;
      background: white;
      border: 1px solid #ddd;
      border-radius: 5px;
      cursor: pointer;
      font-size: 14px;
    }
    
    .filter-btn.active {
      background: #667eea;
      color: white;
      border-color: #667eea;
    }
    
    .task-count {
      color: #666;
      font-size: 14px;
    }
    
    .task-list {
      list-style: none;
      min-height: 200px;
    }
    
    .task-item {
      padding: 15px 20px;
      border-bottom: 1px solid #eee;
      display: flex;
      align-items: center;
      gap: 15px;
      transition: background 0.2s;
    }
    
    .task-item:hover {
      background: #f9f9f9;
    }
    
    .task-item.completed .task-text {
      text-decoration: line-through;
      color: #999;
    }
    
    .task-checkbox {
      width: 20px;
      height: 20px;
      cursor: pointer;
    }
    
    .task-text {
      flex: 1;
      font-size: 16px;
    }
    
    .delete-btn {
      padding: 6px 12px;
      background: #ff4757;
      color: white;
      border: none;
      border-radius: 3px;
      cursor: pointer;
      font-size: 14px;
    }
    
    .delete-btn:hover {
      background: #ff3838;
    }
    
    .empty-state {
      padding: 60px 20px;
      text-align: center;
      color: #999;
    }
    
    .empty-state-icon {
      font-size: 4rem;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <h1>📝 Ma To-Do List</h1>
      <p>Organisez vos tâches efficacement</p>
    </div>
    
    <div class="add-task">
      <div class="input-group">
        <input 
          type="text" 
          id="taskInput" 
          placeholder="Ajouter une nouvelle tâche..."
          autocomplete="off"
        >
        <button id="addBtn">Ajouter</button>
      </div>
    </div>
    
    <div class="filters">
      <div class="filter-buttons">
        <button class="filter-btn active" data-filter="all">Toutes</button>
        <button class="filter-btn" data-filter="active">Actives</button>
        <button class="filter-btn" data-filter="completed">Complétées</button>
      </div>
      <span class="task-count" id="taskCount">0 tâche(s)</span>
    </div>
    
    <ul class="task-list" id="taskList">
      <div class="empty-state">
        <div class="empty-state-icon">✨</div>
        <p>Aucune tâche pour le moment</p>
        <p>Commencez par en ajouter une !</p>
      </div>
    </ul>
  </div>
  
  <script>
    // Sélection des éléments
    const taskInput = document.querySelector('#taskInput');
    const addBtn = document.querySelector('#addBtn');
    const taskList = document.querySelector('#taskList');
    const taskCount = document.querySelector('#taskCount');
    const filterButtons = document.querySelectorAll('.filter-btn');
    
    // État de l'application
    let tasks = [];
    let currentFilter = 'all';
    
    // Initialisation
    loadTasks();
    renderTasks();
    
    // Event Listeners
    addBtn.addEventListener('click', addTask);
    taskInput.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') {
        addTask();
      }
    });
    
    // Event delegation pour les actions sur les tâches
    taskList.addEventListener('click', function(e) {
      const taskItem = e.target.closest('.task-item');
      if (!taskItem) return;
      
      const taskId = parseInt(taskItem.dataset.id);
      
      if (e.target.classList.contains('task-checkbox')) {
        toggleTask(taskId);
      } else if (e.target.classList.contains('delete-btn')) {
        deleteTask(taskId);
      }
    });
    
    // Filtres
    filterButtons.forEach(function(button) {
      button.addEventListener('click', function() {
        // Mettre à jour le bouton actif
        filterButtons.forEach(btn => btn.classList.remove('active'));
        this.classList.add('active');
        
        // Appliquer le filtre
        currentFilter = this.dataset.filter;
        renderTasks();
      });
    });
    
    // Fonctions
    function addTask() {
      const text = taskInput.value.trim();
      
      if (text === '') {
        alert('Veuillez entrer une tâche');
        return;
      }
      
      const task = {
        id: Date.now(),
        text: text,
        completed: false,
        createdAt: new Date().toISOString()
      };
      
      tasks.push(task);
      saveTasks();
      renderTasks();
      
      taskInput.value = '';
      taskInput.focus();
    }
    
    function toggleTask(id) {
      const task = tasks.find(t => t.id === id);
      if (task) {
        task.completed = !task.completed;
        saveTasks();
        renderTasks();
      }
    }
    
    function deleteTask(id) {
      if (confirm('Êtes-vous sûr de vouloir supprimer cette tâche ?')) {
        tasks = tasks.filter(t => t.id !== id);
        saveTasks();
        renderTasks();
      }
    }
    
    function renderTasks() {
      // Filtrer les tâches
      let filteredTasks = tasks;
      
      if (currentFilter === 'active') {
        filteredTasks = tasks.filter(t => !t.completed);
      } else if (currentFilter === 'completed') {
        filteredTasks = tasks.filter(t => t.completed);
      }
      
      // Afficher l'état vide si nécessaire
      if (filteredTasks.length === 0) {
        taskList.innerHTML = `
          <div class="empty-state">
            <div class="empty-state-icon">✨</div>
            <p>${getEmptyStateMessage()}</p>
          </div>
        `;
      } else {
        // Générer le HTML des tâches
        taskList.innerHTML = filteredTasks.map(task => `
          <li class="task-item ${task.completed ? 'completed' : ''}" data-id="${task.id}">
            <input 
              type="checkbox" 
              class="task-checkbox" 
              ${task.completed ? 'checked' : ''}
            >
            <span class="task-text">${escapeHtml(task.text)}</span>
            <button class="delete-btn">Supprimer</button>
          </li>
        `).join('');
      }
      
      // Mettre à jour le compteur
      updateTaskCount();
    }
    
    function updateTaskCount() {
      const activeTasks = tasks.filter(t => !t.completed).length;
      const totalTasks = tasks.length;
      taskCount.textContent = `${activeTasks} active(s) sur ${totalTasks}`;
    }
    
    function getEmptyStateMessage() {
      if (currentFilter === 'active') {
        return 'Aucune tâche active ! 🎉';
      } else if (currentFilter === 'completed') {
        return 'Aucune tâche complétée pour le moment';
      } else {
        return 'Commencez par ajouter une tâche !';
      }
    }
    
    // Persistance avec localStorage
    function saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }
    
    function loadTasks() {
      const saved = localStorage.getItem('tasks');
      if (saved) {
        tasks = JSON.parse(saved);
      }
    }
    
    // Sécurité : échapper le HTML
    function escapeHtml(text) {
      const div = document.createElement('div');
      div.textContent = text;
      return div.innerHTML;
    }
  </script>
</body>
</html>
```

### 12.3 Fonctionnalités expliquées

**1. Structure des données**
```javascript
const task = {
  id: Date.now(),          // ID unique basé sur le timestamp
  text: 'Faire courses',   // Texte de la tâche
  completed: false,        // État (fait ou non)
  createdAt: new Date()    // Date de création
};
```

**2. Event Delegation**
```javascript
// Un seul listener pour TOUTE la liste
taskList.addEventListener('click', function(e) {
  // On détecte quel élément a été cliqué
  if (e.target.classList.contains('task-checkbox')) {
    toggleTask(taskId);
  }
});
```

**3. Filtres**
```javascript
let filteredTasks = tasks;

if (currentFilter === 'active') {
  filteredTasks = tasks.filter(t => !t.completed);
}
```

**4. Persistence**
```javascript
// Sauvegarder
localStorage.setItem('tasks', JSON.stringify(tasks));

// Charger
const saved = localStorage.getItem('tasks');
tasks = JSON.parse(saved);
```

**5. Sécurité XSS**
```javascript
// Échapper le HTML pour éviter l'injection de code
function escapeHtml(text) {
  const div = document.createElement('div');
  div.textContent = text; // textContent est sûr
  return div.innerHTML;
}
```

---

## 13. Exercices Progressifs

### Exercice 1 : Compteur ⭐
**Difficulté : Débutant**

Crée un compteur avec :
- Un affichage du nombre
- Bouton + (incrémente de 1)
- Bouton - (décrémente de 1)
- Bouton Reset (remet à 0)
- Le nombre devient rouge si négatif, vert si positif

**Bonus :**
- Ajoute un bouton "+10" et "-10"
- Sauvegarde la valeur dans localStorage

---

### Exercice 2 : Changeur de Couleur ⭐
**Difficulté : Débutant**

Crée une page avec :
- Un carré de couleur
- 5 boutons avec différentes couleurs
- Cliquer sur un bouton change la couleur du carré

**Bonus :**
- Ajoute un bouton "Couleur aléatoire"
- Affiche le code couleur (ex: #FF5733)

---

### Exercice 3 : Calculatrice ⭐⭐
**Difficulté : Intermédiaire**

Crée une calculatrice avec :
- 2 inputs pour les nombres
- 4 boutons : +, -, ×, ÷
- Affichage du résultat

**Bonus :**
- Validation (empêcher la division par 0)
- Historique des 5 derniers calculs
- Bouton "Clear"

---

### Exercice 4 : Galerie d'Images ⭐⭐
**Difficulté : Intermédiaire**

Crée une galerie avec :
- 6 images miniatures
- Cliquer sur une miniature l'affiche en grand
- Boutons "Précédent" et "Suivant"

**Bonus :**
- Ajoute des transitions CSS
- Navigation au clavier (flèches)
- Lightbox (fond noir transparent)

---

### Exercice 5 : Quiz Interactif ⭐⭐⭐
**Difficulté : Avancé**

Crée un quiz avec :
- 10 questions à choix multiples
- Affichage d'une question à la fois
- Bouton "Question suivante"
- Score final

**Bonus :**
- Chronomètre (30 secondes par question)
- Sauvegarde du meilleur score
- Affichage des réponses correctes à la fin

---

### Exercice 6 : Système de Tabs ⭐⭐⭐
**Difficulté : Avancé**

Crée un système d'onglets avec :
- 4 onglets (Accueil, Profil, Messages, Paramètres)
- Cliquer sur un onglet affiche son contenu
- L'onglet actif est surligné

**Bonus :**
- Navigation au clavier (Tab et flèches)
- URL change selon l'onglet (#accueil, #profil...)
- Animation de transition

---

### Exercice 7 : Panier E-commerce ⭐⭐⭐
**Difficulté : Avancé**

Crée un panier avec :
- Liste de produits avec prix
- Bouton "Ajouter au panier"
- Affichage du panier (produits + quantités)
- Total calculé automatiquement

**Bonus :**
- Boutons +/- pour changer la quantité
- Bouton "Supprimer" par produit
- Sauvegarde dans localStorage
- Code promo (-10%)

---

## 14. Debugging et Erreurs Communes

### 14.1 Console.log() - Ton meilleur ami

```javascript
const element = document.querySelector('.inexistant');
console.log(element); // null

if (element) {
  element.textContent = 'Hello';
} else {
  console.log('Élément non trouvé !');
}
```

**Toujours vérifier que tes sélecteurs retournent quelque chose !**

### 14.2 Erreurs fréquentes

**1. Cannot read property of null**
```javascript
// ❌ Erreur
const titre = document.querySelector('#titre');
titre.textContent = 'Hello';
// Si #titre n'existe pas → CRASH

// ✅ Solution
const titre = document.querySelector('#titre');
if (titre) {
  titre.textContent = 'Hello';
}
```

**2. Script exécuté trop tôt**
```html
<script>
  // ❌ ERREUR : Le body n'existe pas encore
  const body = document.querySelector('body');
</script>
<body>
  <!-- Le body est ici -->
</body>

<!-- ✅ SOLUTION 1 : Mettre le script à la fin -->
<body>
  <!-- Contenu -->
</body>
<script>
  const body = document.querySelector('body'); // Maintenant ça marche
</script>

<!-- ✅ SOLUTION 2 : DOMContentLoaded -->
<script>
  document.addEventListener('DOMContentLoaded', function() {
    const body = document.querySelector('body'); // Exécuté quand le DOM est prêt
  });
</script>
```

**3. Oublier preventDefault()**
```javascript
// ❌ Le formulaire recharge la page
form.addEventListener('submit', function() {
  console.log('Soumis');
  // La page recharge → on ne voit pas le log
});

// ✅ Empêcher le rechargement
form.addEventListener('submit', function(e) {
  e.preventDefault();
  console.log('Soumis');
});
```

**4. Confondre = et ==**
```javascript
// ❌ ERREUR : = assigne, ne compare pas
if (x = 5) { // Assigne 5 à x (toujours vrai)
  console.log('Toujours affiché');
}

// ✅ CORRECT : == ou === comparent
if (x === 5) {
  console.log('x est 5');
}
```

**5. Oublier de convertir les valeurs**
```javascript
// ❌ Concaténation au lieu d'addition
const age = input.value; // "25" (string)
console.log(age + 5);    // "255"

// ✅ Convertir en nombre
const age = parseInt(input.value); // 25 (number)
console.log(age + 5);               // 30
```

### 14.3 DevTools - Inspecteur d'éléments

**Ouvre les DevTools (F12) et :**
1. Onglet "Elements" → Voir le HTML en temps réel
2. Onglet "Console" → Voir les logs et erreurs
3. Onglet "Sources" → Déboguer ligne par ligne

**Astuces :**
```javascript
// Voir tous les event listeners d'un élément
// Dans la console :
getEventListeners(document.querySelector('button'));

// Break on → pause le code quand un élément change
// Clique droit sur l'élément → Break on → Attribute modifications
```

---

## 15. Aller Plus Loin

### 15.1 Ce qu'on a couvert

Tu sais maintenant :
- ✅ Comprendre le DOM
- ✅ Sélectionner n'importe quel élément
- ✅ Modifier le contenu et le style
- ✅ Créer et supprimer des éléments
- ✅ Gérer tous les types d'événements
- ✅ Faire de la validation de formulaires
- ✅ Utiliser localStorage
- ✅ Créer des applications complètes

**C'est ÉNORME. Tu as les bases solides du JavaScript moderne.**

### 15.2 Prochaines étapes

**Partie 5 : JavaScript Asynchrone**
- Fetch API (appeler des APIs)
- Promises
- Async/Await
- Gérer des données externes

**Partie 6 : JavaScript Avancé**
- ES6+ features
- Modules
- Destructuring
- Spread operator

**Partie 7 : Frameworks**
- React
- Vue.js
- Angular

### 15.3 Ressources

**Documentation :**
- MDN Web Docs : https://developer.mozilla.org/fr/
- JavaScript.info : https://javascript.info/

**Pratique :**
- Codewars : Défis JavaScript
- Frontend Mentor : Projets réels
- 100 Days of Code : Challenge personnel

### 15.4 Projets pour t'entraîner

**Niveau 1 :**
- Chronomètre
- Générateur de citations
- Convertisseur de devises
- Jeu Pierre-Papier-Ciseaux

**Niveau 2 :**
- Application météo (avec API)
- Clone de calculatrice iPhone
- Jeu du pendu
- Gestionnaire de budget

**Niveau 3 :**
- Clone de Twitter (simple)
- Trello-like (drag & drop)
- Jeu de mémoire (Memory)
- Chat en temps réel

---

## Conclusion

**Félicitations !** 🎉

Tu viens de terminer le guide le plus complet du DOM et de la manipulation d'éléments.

**Ce que tu as appris équivaut à plusieurs semaines de bootcamp payant.**

**Tu as maintenant les compétences pour :**
- Créer n'importe quelle interface web
- Rendre n'importe quel site interactif
- Construire des applications web complètes

**La prochaine étape ?**
- PRATIQUE, PRATIQUE, PRATIQUE
- Fais les exercices
- Crée tes propres projets
- Ne copie-colle pas : tape le code toi-même

**Tu es maintenant capable de construire des vraies applications.**

**Continue comme ça, champion !** 💪🔥

---

**📚 JavaScript Moderne - Partie 4 : Le DOM et la Manipulation d'Éléments**
**💎 100% Gratuit • Pour Tous • À Jamais**
**🔗 GitHub : [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)**