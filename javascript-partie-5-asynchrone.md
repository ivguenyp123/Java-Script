# JavaScript Moderne - Partie 5 : Asynchrone, Promises et Fetch API

**🔗 Repository GitHub :** [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)

---

## Table des Matières

1. [Introduction à l'Asynchrone](#1-introduction-à-lasynchrone)
2. [Comprendre le Problème](#2-comprendre-le-problème)
3. [Les Callbacks](#3-les-callbacks)
4. [Les Promises](#4-les-promises)
5. [Async/Await](#5-asyncawait)
6. [Fetch API](#6-fetch-api)
7. [Gérer les Erreurs](#7-gérer-les-erreurs)
8. [APIs REST](#8-apis-rest)
9. [Projet : Application Météo](#9-projet-application-météo)
10. [Projet : Recherche GitHub](#10-projet-recherche-github)
11. [setTimeout et setInterval](#11-settimeout-et-setinterval)
12. [Exercices Progressifs](#12-exercices-progressifs)
13. [Patterns Avancés](#13-patterns-avancés)
14. [Debugging Asynchrone](#14-debugging-asynchrone)
15. [Aller Plus Loin](#15-aller-plus-loin)

---

## 1. Introduction à l'Asynchrone

### Qu'est-ce que le code asynchrone ?

Imagine que tu commandes une pizza :

**Code SYNCHRONE (normal) :**
```
Tu appelles la pizzeria
↓
Tu ATTENDS au téléphone (bloqué)
↓ (30 minutes...)
↓
La pizza arrive
↓
Tu peux enfin manger
```

**Code ASYNCHRONE :**
```
Tu appelles la pizzeria
↓
Tu raccroches IMMÉDIATEMENT
↓
Tu continues ta vie (regarder la TV, jouer, etc.)
↓
*DING DONG* La pizza arrive
↓
Tu manges
```

**En programmation, c'est pareil :**

**Synchrone** : Le code attend qu'une tâche se termine avant de continuer
**Asynchrone** : Le code continue pendant qu'une tâche se fait en arrière-plan

### Pourquoi c'est crucial ?

Sur le web, beaucoup d'opérations prennent du temps :
- Charger des données depuis un serveur (API)
- Télécharger des images
- Lire des fichiers
- Attendre un clic utilisateur
- Faire des calculs complexes

**Sans asynchrone :** Ton site se fige pendant que ça charge → Expérience utilisateur HORRIBLE

**Avec asynchrone :** Ton site reste fluide, l'utilisateur peut continuer à interagir

### Exemple visuel

**Code synchrone (bloquant) :**
```javascript
console.log('1. Début');

// Imagine que cette fonction prend 5 secondes
function tacheLongue() {
  // ... 5 secondes d'attente ...
  console.log('2. Tâche terminée');
}

tacheLongue(); // ⏸️ TOUT SE BLOQUE ICI pendant 5 secondes

console.log('3. Fin'); // N'apparaît qu'après 5 secondes

// Résultat :
// 1. Début
// ... 5 secondes d'attente ...
// 2. Tâche terminée
// 3. Fin
```

**Code asynchrone (non-bloquant) :**
```javascript
console.log('1. Début');

// Tâche asynchrone
setTimeout(function() {
  console.log('2. Tâche terminée');
}, 5000); // Se lance en arrière-plan

console.log('3. Fin'); // S'exécute IMMÉDIATEMENT

// Résultat :
// 1. Début
// 3. Fin        ← Immédiat !
// ... 5 secondes plus tard ...
// 2. Tâche terminée
```

---

## 2. Comprendre le Problème

### JavaScript est mono-thread

**Mono-thread** = Une seule chose à la fois

Imagine un restaurant avec UN SEUL cuisinier :

```
Client 1 commande → Cuisinier cuisine → Client 1 servi
Client 2 commande → Cuisinier cuisine → Client 2 servi
Client 3 commande → Cuisinier cuisine → Client 3 servi

Si le cuisinier doit attendre que l'eau bouille, il NE PEUT RIEN FAIRE D'AUTRE
```

**Problème :** Si une tâche prend du temps, tout le reste attend.

### L'Event Loop à la rescousse

JavaScript utilise un système appelé **Event Loop** (boucle d'événements) :

```
┌─────────────────────────────────┐
│   Call Stack (Pile d'exécution) │  ← Code en cours d'exécution
└─────────────────────────────────┘
         ↑                ↓
┌─────────────────────────────────┐
│   Web APIs (Arrière-plan)       │  ← Tâches asynchrones
└─────────────────────────────────┘
         ↓
┌─────────────────────────────────┐
│   Callback Queue (File d'attente)│  ← Callbacks en attente
└─────────────────────────────────┘
         ↓
    Event Loop vérifie : "Call Stack vide ?"
         ↓ Oui
    Prend le premier callback de la queue
```

**Ce que ça signifie :**
1. Le code synchrone s'exécute normalement
2. Les tâches asynchrones sont envoyées en "arrière-plan" (Web APIs)
3. Quand elles sont terminées, leur callback va dans la queue
4. Quand le call stack est vide, l'Event Loop prend un callback de la queue

### Exemple concret

```javascript
console.log('1');

setTimeout(function() {
  console.log('2');
}, 0); // 0 milliseconde !

console.log('3');

// Résultat :
// 1
// 3
// 2

// Pourquoi ? Même avec 0ms, setTimeout est ASYNCHRONE
// Il passe par la queue
```

**Visualisation :**
```
1. console.log('1') → Exécuté immédiatement → "1"
2. setTimeout → Envoyé en arrière-plan (Web API)
3. console.log('3') → Exécuté immédiatement → "3"
4. Call stack vide → Event Loop prend le callback → "2"
```

---

## 3. Les Callbacks

### Qu'est-ce qu'un callback ?

Un **callback** est une fonction passée en argument à une autre fonction, qui sera appelée plus tard.

**Analogie :** Tu donnes ton numéro de téléphone à la pizzeria. Ils te "rappellent" (callback) quand la pizza est prête.

### Exemple simple

```javascript
function direBonjour(nom) {
  console.log('Bonjour ' + nom);
}

function traiterUtilisateur(callback) {
  const nom = 'Alice';
  callback(nom); // On "rappelle" la fonction
}

traiterUtilisateur(direBonjour);
// Affiche : "Bonjour Alice"
```

### Callbacks asynchrones

```javascript
console.log('Début');

setTimeout(function() {
  console.log('Ceci apparaît après 2 secondes');
}, 2000);

console.log('Fin');

// Résultat :
// Début
// Fin
// ... 2 secondes plus tard ...
// Ceci apparaît après 2 secondes
```

### Exemple réel : Charger des données

```javascript
function chargerUtilisateur(id, callback) {
  console.log('Chargement de l\'utilisateur ' + id + '...');
  
  // Simuler un appel réseau qui prend 2 secondes
  setTimeout(function() {
    const utilisateur = {
      id: id,
      nom: 'Alice',
      email: 'alice@example.com'
    };
    
    callback(utilisateur);
  }, 2000);
}

// Utilisation
chargerUtilisateur(1, function(utilisateur) {
  console.log('Utilisateur chargé :', utilisateur.nom);
});

console.log('En attente...');

// Résultat :
// Chargement de l'utilisateur 1...
// En attente...
// ... 2 secondes plus tard ...
// Utilisateur chargé : Alice
```

### Le problème : Callback Hell (Enfer des callbacks)

Quand tu dois faire plusieurs tâches asynchrones qui dépendent les unes des autres :

```javascript
// ❌ CALLBACK HELL - Code illisible et cauchemardesque
chargerUtilisateur(1, function(utilisateur) {
  console.log('Utilisateur chargé:', utilisateur.nom);
  
  chargerPosts(utilisateur.id, function(posts) {
    console.log('Posts chargés:', posts.length);
    
    chargerCommentaires(posts[0].id, function(commentaires) {
      console.log('Commentaires chargés:', commentaires.length);
      
      chargerAuteur(commentaires[0].auteurId, function(auteur) {
        console.log('Auteur chargé:', auteur.nom);
        
        // Et ça continue... 🤮
      });
    });
  });
});

// C'est appelé la "Pyramide of Doom" (Pyramide du Chaos)
// Le code part vers la droite et devient ingérable
```

**Ce problème est RÉEL.** Avant les Promises, c'était comme ça partout.

---

## 4. Les Promises

### Qu'est-ce qu'une Promise ?

Une **Promise** (promesse) est un objet qui représente une valeur qui sera disponible **maintenant, plus tard, ou jamais**.

**Analogie de la pizzeria (encore) :**
- Tu commandes une pizza
- On te donne un **ticket** (la Promise)
- Le ticket a 3 états possibles :
  - **Pending** (en attente) : La pizza est en cours de préparation
  - **Fulfilled** (résolue) : La pizza est prête ✅
  - **Rejected** (rejetée) : Problème, pas de pizza ❌

### États d'une Promise

```
┌──────────────┐
│   PENDING    │  ← État initial
└──────────────┘
       │
       ├─→ FULFILLED (résolue avec succès)
       │   → .then() est appelé
       │
       └─→ REJECTED (échouée)
           → .catch() est appelé
```

### Créer une Promise

```javascript
const maPromise = new Promise(function(resolve, reject) {
  // Code asynchrone ici
  
  const succes = true; // Simuler un succès ou échec
  
  if (succes) {
    resolve('Tout s\'est bien passé !'); // ✅ Réussie
  } else {
    reject('Erreur !'); // ❌ Échec
  }
});
```

**Les paramètres :**
- `resolve` : Fonction à appeler si tout va bien
- `reject` : Fonction à appeler si ça échoue

### Utiliser une Promise

```javascript
maPromise
  .then(function(resultat) {
    // Ce code s'exécute si la promise est RÉSOLUE
    console.log('Succès :', resultat);
  })
  .catch(function(erreur) {
    // Ce code s'exécute si la promise est REJETÉE
    console.log('Erreur :', erreur);
  });
```

### Exemple complet : Simuler un chargement

```javascript
function chargerUtilisateur(id) {
  return new Promise(function(resolve, reject) {
    console.log('Chargement de l\'utilisateur ' + id + '...');
    
    // Simuler un délai réseau
    setTimeout(function() {
      // Simuler un utilisateur
      const utilisateur = {
        id: id,
        nom: 'Alice',
        email: 'alice@example.com'
      };
      
      // Succès
      resolve(utilisateur);
      
      // Pour simuler une erreur :
      // reject(new Error('Utilisateur non trouvé'));
      
    }, 2000);
  });
}

// Utilisation
chargerUtilisateur(1)
  .then(function(utilisateur) {
    console.log('✅ Utilisateur chargé :', utilisateur.nom);
    console.log('Email :', utilisateur.email);
  })
  .catch(function(erreur) {
    console.log('❌ Erreur :', erreur.message);
  });

console.log('Code continue pendant le chargement...');

// Résultat :
// Chargement de l'utilisateur 1...
// Code continue pendant le chargement...
// ... 2 secondes plus tard ...
// ✅ Utilisateur chargé : Alice
// Email : alice@example.com
```

### Chaîner les Promises

**ÉNORME AVANTAGE** : On peut chaîner les `.then()` pour éviter le callback hell.

```javascript
chargerUtilisateur(1)
  .then(function(utilisateur) {
    console.log('Utilisateur :', utilisateur.nom);
    return chargerPosts(utilisateur.id); // Retourne une nouvelle Promise
  })
  .then(function(posts) {
    console.log('Posts :', posts.length);
    return chargerCommentaires(posts[0].id); // Encore une Promise
  })
  .then(function(commentaires) {
    console.log('Commentaires :', commentaires.length);
  })
  .catch(function(erreur) {
    // UN SEUL catch pour TOUTES les erreurs
    console.log('Erreur quelque part :', erreur);
  });

// ✅ Code lisible, linéaire, maintenable
// vs le callback hell qui partait vers la droite
```

### Promise.all() - Exécuter plusieurs Promises en parallèle

```javascript
const promise1 = chargerUtilisateur(1);
const promise2 = chargerUtilisateur(2);
const promise3 = chargerUtilisateur(3);

Promise.all([promise1, promise2, promise3])
  .then(function(utilisateurs) {
    // utilisateurs est un tableau avec les 3 résultats
    console.log('Tous chargés :');
    utilisateurs.forEach(function(u) {
      console.log('- ' + u.nom);
    });
  })
  .catch(function(erreur) {
    // Si UNE SEULE échoue, tout échoue
    console.log('Erreur :', erreur);
  });
```

**Cas d'usage :** Charger plusieurs données en même temps sans attendre séquentiellement.

### Promise.race() - La première qui termine

```javascript
const lent = new Promise(resolve => setTimeout(() => resolve('Lent'), 3000));
const rapide = new Promise(resolve => setTimeout(() => resolve('Rapide'), 1000));

Promise.race([lent, rapide])
  .then(function(resultat) {
    console.log('Gagnant :', resultat); // "Rapide"
  });
```

**Cas d'usage :** Timeout (annuler si trop long), charger depuis plusieurs sources.

### Promise.allSettled() - Attendre toutes, même si certaines échouent

```javascript
const promise1 = Promise.resolve('Succès 1');
const promise2 = Promise.reject('Erreur 2');
const promise3 = Promise.resolve('Succès 3');

Promise.allSettled([promise1, promise2, promise3])
  .then(function(resultats) {
    resultats.forEach(function(resultat) {
      if (resultat.status === 'fulfilled') {
        console.log('✅', resultat.value);
      } else {
        console.log('❌', resultat.reason);
      }
    });
  });

// Résultat :
// ✅ Succès 1
// ❌ Erreur 2
// ✅ Succès 3
```

---

## 5. Async/Await

### La syntaxe moderne (ES2017+)

`async/await` rend le code asynchrone **aussi lisible que du code synchrone**.

**C'est du sucre syntaxique** au-dessus des Promises. Ça ne change rien au fonctionnement, c'est juste plus beau.

### Fonction async

```javascript
// Ancienne façon avec Promise
function chargerUtilisateur() {
  return new Promise(function(resolve, reject) {
    // ...
    resolve(utilisateur);
  });
}

// Nouvelle façon avec async
async function chargerUtilisateur() {
  // Une fonction async retourne TOUJOURS une Promise
  const utilisateur = { nom: 'Alice' };
  return utilisateur; // Automatiquement wrappé dans une Promise
}

// Équivalent à :
// return Promise.resolve(utilisateur);
```

**Important :** Une fonction `async` retourne TOUJOURS une Promise, même si tu retournes une valeur normale.

### Le mot-clé await

`await` met le code en "pause" jusqu'à ce que la Promise soit résolue.

**⚠️ On ne peut utiliser `await` QUE dans une fonction `async`**

```javascript
async function exemple() {
  console.log('1. Début');
  
  // await "attend" que la Promise se résolve
  const utilisateur = await chargerUtilisateur(1);
  
  console.log('2. Utilisateur chargé :', utilisateur.nom);
  console.log('3. Fin');
}

exemple();

// Résultat :
// 1. Début
// ... attente ...
// 2. Utilisateur chargé : Alice
// 3. Fin
```

### Comparaison : Promise vs Async/Await

**Avec Promise (.then) :**
```javascript
function afficherUtilisateur() {
  chargerUtilisateur(1)
    .then(function(utilisateur) {
      console.log(utilisateur.nom);
      return chargerPosts(utilisateur.id);
    })
    .then(function(posts) {
      console.log(posts.length + ' posts');
    })
    .catch(function(erreur) {
      console.log('Erreur :', erreur);
    });
}
```

**Avec Async/Await :**
```javascript
async function afficherUtilisateur() {
  try {
    const utilisateur = await chargerUtilisateur(1);
    console.log(utilisateur.nom);
    
    const posts = await chargerPosts(utilisateur.id);
    console.log(posts.length + ' posts');
    
  } catch (erreur) {
    console.log('Erreur :', erreur);
  }
}
```

**✅ Async/await est BEAUCOUP plus lisible !**

### Gérer les erreurs avec try/catch

```javascript
async function exemple() {
  try {
    // Code qui peut échouer
    const utilisateur = await chargerUtilisateur(999); // ID inexistant
    console.log(utilisateur.nom);
    
  } catch (erreur) {
    // Si une erreur se produit, on arrive ici
    console.log('❌ Erreur :', erreur.message);
    
  } finally {
    // Code qui s'exécute TOUJOURS (succès ou erreur)
    console.log('Terminé');
  }
}
```

### Exécuter plusieurs await en parallèle

```javascript
// ❌ LENT - Les await s'exécutent l'un après l'autre
async function lent() {
  const user1 = await chargerUtilisateur(1); // 2 secondes
  const user2 = await chargerUtilisateur(2); // 2 secondes
  const user3 = await chargerUtilisateur(3); // 2 secondes
  // Total : 6 secondes
}

// ✅ RAPIDE - Les await s'exécutent en parallèle
async function rapide() {
  const [user1, user2, user3] = await Promise.all([
    chargerUtilisateur(1),
    chargerUtilisateur(2),
    chargerUtilisateur(3)
  ]);
  // Total : 2 secondes (tous en même temps)
}
```

### Exemple complet

```javascript
// Simuler des fonctions asynchrones
function chargerUtilisateur(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id > 0) {
        resolve({ id: id, nom: 'Utilisateur ' + id });
      } else {
        reject(new Error('ID invalide'));
      }
    }, 1000);
  });
}

function chargerPosts(userId) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve([
        { id: 1, titre: 'Post 1' },
        { id: 2, titre: 'Post 2' }
      ]);
    }, 1000);
  });
}

// Fonction principale avec async/await
async function afficherDonnees() {
  try {
    console.log('🔄 Chargement de l\'utilisateur...');
    const utilisateur = await chargerUtilisateur(1);
    console.log('✅ Utilisateur :', utilisateur.nom);
    
    console.log('🔄 Chargement des posts...');
    const posts = await chargerPosts(utilisateur.id);
    console.log('✅ Posts :', posts.length);
    
    posts.forEach(post => {
      console.log('  - ' + post.titre);
    });
    
  } catch (erreur) {
    console.log('❌ Erreur :', erreur.message);
  }
}

afficherDonnees();

// Résultat :
// 🔄 Chargement de l'utilisateur...
// ... 1 seconde ...
// ✅ Utilisateur : Utilisateur 1
// 🔄 Chargement des posts...
// ... 1 seconde ...
// ✅ Posts : 2
//   - Post 1
//   - Post 2
```

---

## 6. Fetch API

### Qu'est-ce que Fetch ?

`fetch()` est la fonction moderne pour faire des **requêtes HTTP** (appeler des APIs).

**Avant fetch :** On utilisait `XMLHttpRequest` (horrible syntaxe)
**Maintenant :** `fetch()` retourne une Promise ❤️

### Syntaxe de base

```javascript
fetch('https://api.example.com/users')
  .then(function(response) {
    return response.json(); // Convertir en JSON
  })
  .then(function(data) {
    console.log(data); // Les données
  })
  .catch(function(erreur) {
    console.log('Erreur :', erreur);
  });
```

### Avec async/await (RECOMMANDÉ)

```javascript
async function chargerUtilisateurs() {
  try {
    const response = await fetch('https://api.example.com/users');
    const data = await response.json();
    console.log(data);
  } catch (erreur) {
    console.log('Erreur :', erreur);
  }
}
```

### Anatomie d'une requête fetch

```javascript
async function exemple() {
  // 1. Faire la requête
  const response = await fetch('https://api.example.com/users');
  
  // 2. L'objet Response contient :
  console.log(response.status);     // 200, 404, 500, etc.
  console.log(response.statusText); // "OK", "Not Found", etc.
  console.log(response.ok);         // true si status 200-299
  console.log(response.headers);    // Headers HTTP
  
  // 3. Extraire les données
  const data = await response.json();  // Pour du JSON
  // ou
  const text = await response.text();  // Pour du texte
  // ou
  const blob = await response.blob();  // Pour des fichiers
  
  return data;
}
```

### Vérifier les erreurs correctement

**⚠️ IMPORTANT :** fetch ne rejette QUE si :
- Pas de connexion internet
- URL invalide
- Erreur réseau

**fetch ne rejette PAS si le serveur répond 404, 500, etc.**

```javascript
// ❌ INCOMPLET - Ne gère pas les erreurs 404, 500
async function mauvais() {
  try {
    const response = await fetch('https://api.example.com/users');
    const data = await response.json();
    return data;
  } catch (erreur) {
    // N'attrape PAS les 404, 500, etc.
    console.log('Erreur :', erreur);
  }
}

// ✅ CORRECT - Vérifie response.ok
async function bon() {
  try {
    const response = await fetch('https://api.example.com/users');
    
    // Vérifier si la réponse est OK (status 200-299)
    if (!response.ok) {
      throw new Error('Erreur HTTP : ' + response.status);
    }
    
    const data = await response.json();
    return data;
    
  } catch (erreur) {
    console.log('❌ Erreur :', erreur.message);
  }
}
```

### Méthodes HTTP

**GET - Récupérer des données (par défaut)**
```javascript
const response = await fetch('https://api.example.com/users');
```

**POST - Envoyer des données**
```javascript
const response = await fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    nom: 'Alice',
    email: 'alice@example.com'
  })
});
```

**PUT - Modifier des données**
```javascript
const response = await fetch('https://api.example.com/users/1', {
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    nom: 'Alice Dupont'
  })
});
```

**DELETE - Supprimer**
```javascript
const response = await fetch('https://api.example.com/users/1', {
  method: 'DELETE'
});
```

### Exemple complet : API de blagues

```javascript
async function chargerBlague() {
  try {
    // API publique gratuite
    const response = await fetch('https://official-joke-api.appspot.com/random_joke');
    
    if (!response.ok) {
      throw new Error('Erreur : ' + response.status);
    }
    
    const blague = await response.json();
    
    console.log('Setup:', blague.setup);
    console.log('Punchline:', blague.punchline);
    
  } catch (erreur) {
    console.log('❌ Erreur :', erreur.message);
  }
}

chargerBlague();

// Exemple de résultat :
// Setup: Why don't scientists trust atoms?
// Punchline: Because they make up everything.
```

### Headers et authentification

```javascript
async function chargerAvecAuth() {
  const response = await fetch('https://api.example.com/protected', {
    method: 'GET',
    headers: {
      'Authorization': 'Bearer ' + token,
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    }
  });
  
  const data = await response.json();
  return data;
}
```

### Timeout (annuler si trop long)

```javascript
async function fetchAvecTimeout(url, timeout = 5000) {
  const controller = new AbortController();
  const signal = controller.signal;
  
  // Annuler après X millisecondes
  const timeoutId = setTimeout(() => controller.abort(), timeout);
  
  try {
    const response = await fetch(url, { signal });
    clearTimeout(timeoutId);
    return await response.json();
    
  } catch (erreur) {
    if (erreur.name === 'AbortError') {
      console.log('❌ Timeout : Trop long !');
    } else {
      console.log('❌ Erreur :', erreur);
    }
  }
}

// Utilisation
fetchAvecTimeout('https://api.example.com/data', 3000);
```

---

## 7. Gérer les Erreurs

### Les types d'erreurs asynchrones

**1. Erreur réseau**
```javascript
try {
  const response = await fetch('https://sitekinexistepas.com');
} catch (erreur) {
  console.log('Pas de connexion ou URL invalide');
}
```

**2. Erreur serveur (404, 500, etc.)**
```javascript
const response = await fetch('https://api.example.com/users/999999');
if (!response.ok) {
  console.log('Serveur a répondu avec une erreur :', response.status);
}
```

**3. Erreur de parsing JSON**
```javascript
try {
  const data = await response.json();
} catch (erreur) {
  console.log('Réponse n\'est pas du JSON valide');
}
```

### Pattern de gestion d'erreurs robuste

```javascript
async function chargerDonnees(url) {
  let response;
  
  try {
    // Étape 1 : Faire la requête
    response = await fetch(url);
    
  } catch (erreur) {
    // Erreur réseau (pas de connexion, etc.)
    return {
      succes: false,
      erreur: 'Erreur réseau : ' + erreur.message
    };
  }
  
  // Étape 2 : Vérifier le status
  if (!response.ok) {
    return {
      succes: false,
      erreur: 'Erreur serveur : ' + response.status,
      status: response.status
    };
  }
  
  try {
    // Étape 3 : Parser le JSON
    const data = await response.json();
    
    return {
      succes: true,
      data: data
    };
    
  } catch (erreur) {
    // Erreur de parsing
    return {
      succes: false,
      erreur: 'Réponse invalide : ' + erreur.message
    };
  }
}

// Utilisation
async function exemple() {
  const resultat = await chargerDonnees('https://api.example.com/users');
  
  if (resultat.succes) {
    console.log('✅ Données :', resultat.data);
  } else {
    console.log('❌ Erreur :', resultat.erreur);
  }
}
```

### Retry (Réessayer automatiquement)

```javascript
async function fetchAvecRetry(url, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      console.log(`Tentative ${i + 1}/${maxRetries}...`);
      
      const response = await fetch(url);
      
      if (response.ok) {
        return await response.json();
      }
      
      // Si pas OK et c'est la dernière tentative
      if (i === maxRetries - 1) {
        throw new Error('Échec après ' + maxRetries + ' tentatives');
      }
      
      // Attendre avant de réessayer (backoff)
      await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
      
    } catch (erreur) {
      if (i === maxRetries - 1) {
        throw erreur;
      }
    }
  }
}

// Utilisation
try {
  const data = await fetchAvecRetry('https://api.unstable.com/data', 3);
  console.log('✅ Données chargées :', data);
} catch (erreur) {
  console.log('❌ Échec final :', erreur.message);
}
```

---

## 8. APIs REST

### Qu'est-ce qu'une API REST ?

**API** = Application Programming Interface (Interface de Programmation)

**REST** = REpresentational State Transfer (un style d'architecture)

**En gros :** Une façon standardisée pour les applications de communiquer via HTTP.

### Structure d'une URL d'API

```
https://api.example.com/v1/users/123/posts?limit=10

└─────┬────────────┘ └┬┘ └─┬─┘ └┬┘ └──┬─┘ └────┬────┘
   Domaine          Version Base ID   Relation  Query
```

### Les endpoints courants

**Collection (liste)**
```
GET /users → Liste tous les utilisateurs
```

**Ressource (item unique)**
```
GET /users/123 → Récupère l'utilisateur 123
```

**Création**
```
POST /users → Crée un nouvel utilisateur
```

**Modification**
```
PUT /users/123 → Remplace l'utilisateur 123
PATCH /users/123 → Modifie partiellement l'utilisateur 123
```

**Suppression**
```
DELETE /users/123 → Supprime l'utilisateur 123
```

**Relations**
```
GET /users/123/posts → Posts de l'utilisateur 123
GET /posts/456/comments → Commentaires du post 456
```

### Exemple : CRUD complet (Create, Read, Update, Delete)

```javascript
const API_URL = 'https://jsonplaceholder.typicode.com';

// CREATE - Créer un utilisateur
async function creerUtilisateur(utilisateur) {
  const response = await fetch(`${API_URL}/users`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(utilisateur)
  });
  
  if (!response.ok) {
    throw new Error('Erreur création');
  }
  
  return await response.json();
}

// READ - Lire un utilisateur
async function lireUtilisateur(id) {
  const response = await fetch(`${API_URL}/users/${id}`);
  
  if (!response.ok) {
    throw new Error('Utilisateur non trouvé');
  }
  
  return await response.json();
}

// UPDATE - Modifier un utilisateur
async function modifierUtilisateur(id, modifications) {
  const response = await fetch(`${API_URL}/users/${id}`, {
    method: 'PATCH',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(modifications)
  });
  
  if (!response.ok) {
    throw new Error('Erreur modification');
  }
  
  return await response.json();
}

// DELETE - Supprimer un utilisateur
async function supprimerUtilisateur(id) {
  const response = await fetch(`${API_URL}/users/${id}`, {
    method: 'DELETE'
  });
  
  if (!response.ok) {
    throw new Error('Erreur suppression');
  }
  
  return true;
}

// Utilisation
async function exemple() {
  try {
    // Créer
    const nouveau = await creerUtilisateur({
      name: 'Alice',
      email: 'alice@example.com'
    });
    console.log('✅ Créé :', nouveau);
    
    // Lire
    const utilisateur = await lireUtilisateur(1);
    console.log('✅ Lu :', utilisateur);
    
    // Modifier
    const modifie = await modifierUtilisateur(1, {
      name: 'Alice Dupont'
    });
    console.log('✅ Modifié :', modifie);
    
    // Supprimer
    await supprimerUtilisateur(1);
    console.log('✅ Supprimé');
    
  } catch (erreur) {
    console.log('❌ Erreur :', erreur.message);
  }
}
```

### Query parameters (paramètres de requête)

```javascript
// Filtrer, trier, paginer
const params = new URLSearchParams({
  page: 2,
  limit: 10,
  sort: 'name',
  order: 'asc',
  search: 'alice'
});

const url = `https://api.example.com/users?${params}`;
// https://api.example.com/users?page=2&limit=10&sort=name&order=asc&search=alice

const response = await fetch(url);
const data = await response.json();
```

### Status codes HTTP importants

```
2xx - Succès
200 OK              → Succès général
201 Created         → Ressource créée
204 No Content      → Succès sans contenu (DELETE)

4xx - Erreur client
400 Bad Request     → Requête invalide
401 Unauthorized    → Non authentifié
403 Forbidden       → Pas les permissions
404 Not Found       → Ressource introuvable
422 Unprocessable   → Données invalides

5xx - Erreur serveur
500 Internal Error  → Erreur serveur
502 Bad Gateway     → Problème de proxy
503 Service Unavailable → Service down
```

---

## 9. Projet : Application Météo

### Objectif

Créer une application météo qui :
- Affiche la météo d'une ville
- Utilise une vraie API météo
- Gère les erreurs
- A une interface propre

### APIs météo gratuites

**OpenWeatherMap** : https://openweathermap.org/api
- Gratuit jusqu'à 1000 appels/jour
- Inscription requise (clé API)

**WeatherAPI** : https://www.weatherapi.com/
- Gratuit jusqu'à 1M appels/mois
- Inscription requise

Pour ce projet, on va utiliser OpenWeatherMap.

### Étape 1 : Structure HTML

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Application Météo</title>
  <style>
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
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
      max-width: 500px;
      width: 100%;
    }
    
    h1 {
      text-align: center;
      color: #333;
      margin-bottom: 30px;
    }
    
    .search-box {
      display: flex;
      gap: 10px;
      margin-bottom: 30px;
    }
    
    input {
      flex: 1;
      padding: 15px;
      border: 2px solid #ddd;
      border-radius: 10px;
      font-size: 16px;
    }
    
    input:focus {
      outline: none;
      border-color: #667eea;
    }
    
    button {
      padding: 15px 30px;
      background: #667eea;
      color: white;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      font-weight: bold;
    }
    
    button:hover {
      background: #5568d3;
    }
    
    button:disabled {
      background: #ccc;
      cursor: not-allowed;
    }
    
    .weather-container {
      display: none;
    }
    
    .weather-container.show {
      display: block;
    }
    
    .city-name {
      font-size: 32px;
      font-weight: bold;
      text-align: center;
      color: #333;
      margin-bottom: 10px;
    }
    
    .weather-icon {
      text-align: center;
      font-size: 80px;
      margin: 20px 0;
    }
    
    .temperature {
      text-align: center;
      font-size: 60px;
      font-weight: bold;
      color: #667eea;
      margin-bottom: 10px;
    }
    
    .description {
      text-align: center;
      font-size: 24px;
      color: #666;
      text-transform: capitalize;
      margin-bottom: 30px;
    }
    
    .details {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }
    
    .detail-item {
      background: #f5f5f5;
      padding: 15px;
      border-radius: 10px;
      text-align: center;
    }
    
    .detail-label {
      font-size: 14px;
      color: #666;
      margin-bottom: 5px;
    }
    
    .detail-value {
      font-size: 20px;
      font-weight: bold;
      color: #333;
    }
    
    .error {
      background: #ff4757;
      color: white;
      padding: 15px;
      border-radius: 10px;
      text-align: center;
      margin-bottom: 20px;
      display: none;
    }
    
    .error.show {
      display: block;
    }
    
    .loading {
      text-align: center;
      color: #667eea;
      font-size: 18px;
      display: none;
    }
    
    .loading.show {
      display: block;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>🌤️ Météo</h1>
    
    <div class="search-box">
      <input type="text" id="cityInput" placeholder="Entrez une ville..." autocomplete="off">
      <button id="searchBtn">Rechercher</button>
    </div>
    
    <div class="error" id="error"></div>
    <div class="loading" id="loading">Chargement...</div>
    
    <div class="weather-container" id="weatherContainer">
      <div class="city-name" id="cityName"></div>
      <div class="weather-icon" id="weatherIcon"></div>
      <div class="temperature" id="temperature"></div>
      <div class="description" id="description"></div>
      
      <div class="details">
        <div class="detail-item">
          <div class="detail-label">Ressenti</div>
          <div class="detail-value" id="feelsLike"></div>
        </div>
        <div class="detail-item">
          <div class="detail-label">Humidité</div>
          <div class="detail-value" id="humidity"></div>
        </div>
        <div class="detail-item">
          <div class="detail-label">Vent</div>
          <div class="detail-value" id="wind"></div>
        </div>
        <div class="detail-item">
          <div class="detail-label">Pression</div>
          <div class="detail-value" id="pressure"></div>
        </div>
      </div>
    </div>
  </div>
  
  <script src="app.js"></script>
</body>
</html>
```

### Étape 2 : JavaScript (app.js)

```javascript
// ⚠️ REMPLACE PAR TA CLÉ API
// Obtiens-la sur : https://openweathermap.org/api
const API_KEY = 'TA_CLE_API_ICI';
const API_URL = 'https://api.openweathermap.org/data/2.5/weather';

// Sélection des éléments
const cityInput = document.querySelector('#cityInput');
const searchBtn = document.querySelector('#searchBtn');
const weatherContainer = document.querySelector('#weatherContainer');
const error = document.querySelector('#error');
const loading = document.querySelector('#loading');

// Éléments météo
const cityName = document.querySelector('#cityName');
const weatherIcon = document.querySelector('#weatherIcon');
const temperature = document.querySelector('#temperature');
const description = document.querySelector('#description');
const feelsLike = document.querySelector('#feelsLike');
const humidity = document.querySelector('#humidity');
const wind = document.querySelector('#wind');
const pressure = document.querySelector('#pressure');

// Event listeners
searchBtn.addEventListener('click', rechercherMeteo);
cityInput.addEventListener('keypress', function(e) {
  if (e.key === 'Enter') {
    rechercherMeteo();
  }
});

// Fonction principale
async function rechercherMeteo() {
  const ville = cityInput.value.trim();
  
  if (ville === '') {
    afficherErreur('Veuillez entrer une ville');
    return;
  }
  
  // Cacher les éléments
  cacherTout();
  
  // Afficher le loading
  loading.classList.add('show');
  searchBtn.disabled = true;
  
  try {
    // Construire l'URL
    const url = `${API_URL}?q=${ville}&appid=${API_KEY}&units=metric&lang=fr`;
    
    // Faire la requête
    const response = await fetch(url);
    
    // Vérifier la réponse
    if (!response.ok) {
      if (response.status === 404) {
        throw new Error('Ville non trouvée');
      } else if (response.status === 401) {
        throw new Error('Clé API invalide');
      } else {
        throw new Error('Erreur serveur');
      }
    }
    
    // Parser les données
    const data = await response.json();
    
    // Afficher les données
    afficherMeteo(data);
    
  } catch (erreur) {
    afficherErreur(erreur.message);
  } finally {
    loading.classList.remove('show');
    searchBtn.disabled = false;
  }
}

function afficherMeteo(data) {
  // Nom de la ville
  cityName.textContent = `${data.name}, ${data.sys.country}`;
  
  // Icône météo
  const iconCode = data.weather[0].icon;
  weatherIcon.textContent = obtenirEmoji(iconCode);
  
  // Température
  temperature.textContent = Math.round(data.main.temp) + '°C';
  
  // Description
  description.textContent = data.weather[0].description;
  
  // Détails
  feelsLike.textContent = Math.round(data.main.feels_like) + '°C';
  humidity.textContent = data.main.humidity + '%';
  wind.textContent = Math.round(data.wind.speed * 3.6) + ' km/h';
  pressure.textContent = data.main.pressure + ' hPa';
  
  // Afficher le container
  weatherContainer.classList.add('show');
}

function obtenirEmoji(iconCode) {
  const emojis = {
    '01d': '☀️', '01n': '🌙',
    '02d': '⛅', '02n': '⛅',
    '03d': '☁️', '03n': '☁️',
    '04d': '☁️', '04n': '☁️',
    '09d': '🌧️', '09n': '🌧️',
    '10d': '🌦️', '10n': '🌦️',
    '11d': '⛈️', '11n': '⛈️',
    '13d': '❄️', '13n': '❄️',
    '50d': '🌫️', '50n': '🌫️'
  };
  
  return emojis[iconCode] || '🌤️';
}

function afficherErreur(message) {
  error.textContent = '❌ ' + message;
  error.classList.add('show');
  
  setTimeout(() => {
    error.classList.remove('show');
  }, 3000);
}

function cacherTout() {
  weatherContainer.classList.remove('show');
  error.classList.remove('show');
}

// Charger Paris par défaut au démarrage
window.addEventListener('load', () => {
  cityInput.value = 'Paris';
  rechercherMeteo();
});
```

### Comment ça fonctionne

**1. Obtenir une clé API**
- Va sur https://openweathermap.org/api
- Crée un compte gratuit
- Copie ta clé API
- Remplace `TA_CLE_API_ICI` dans le code

**2. L'URL de l'API**
```
https://api.openweathermap.org/data/2.5/weather?q=Paris&appid=CLEF&units=metric&lang=fr

Paramètres :
- q=Paris : La ville
- appid=CLEF : Ta clé API
- units=metric : Températures en Celsius
- lang=fr : Descriptions en français
```

**3. Structure de la réponse**
```json
{
  "name": "Paris",
  "sys": { "country": "FR" },
  "main": {
    "temp": 15.5,
    "feels_like": 14.2,
    "humidity": 72,
    "pressure": 1013
  },
  "weather": [
    {
      "description": "nuageux",
      "icon": "04d"
    }
  ],
  "wind": {
    "speed": 5.2
  }
}
```

**4. Gestion des erreurs**
- 404 : Ville non trouvée
- 401 : Clé API invalide
- Autre : Erreur serveur

### Améliorations possibles

1. **Géolocalisation**
```javascript
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(async (position) => {
    const lat = position.coords.latitude;
    const lon = position.coords.longitude;
    const url = `${API_URL}?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric&lang=fr`;
    // ...
  });
}
```

2. **Prévisions 5 jours**
```javascript
const url = 'https://api.openweathermap.org/data/2.5/forecast?q=Paris&appid=CLEF';
```

3. **Historique des recherches (localStorage)**
```javascript
const historique = JSON.parse(localStorage.getItem('historique')) || [];
historique.unshift(ville);
localStorage.setItem('historique', JSON.stringify(historique.slice(0, 5)));
```

4. **Autocomplete des villes**

---

## 10. Projet : Recherche GitHub

### Objectif

Créer une recherche d'utilisateurs GitHub avec :
- Recherche en temps réel
- Affichage des résultats
- Détails de l'utilisateur
- Utilisation de l'API GitHub (pas de clé requise)

### Code complet

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Recherche GitHub</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #0d1117;
      color: #c9d1d9;
      padding: 20px;
    }
    
    .container {
      max-width: 800px;
      margin: 0 auto;
    }
    
    h1 {
      text-align: center;
      margin-bottom: 30px;
      color: #58a6ff;
    }
    
    .search-box {
      background: #161b22;
      padding: 20px;
      border-radius: 10px;
      margin-bottom: 30px;
    }
    
    input {
      width: 100%;
      padding: 15px;
      background: #0d1117;
      border: 1px solid #30363d;
      border-radius: 6px;
      color: #c9d1d9;
      font-size: 16px;
    }
    
    input:focus {
      outline: none;
      border-color: #58a6ff;
    }
    
    .loading {
      text-align: center;
      padding: 20px;
      display: none;
    }
    
    .loading.show {
      display: block;
    }
    
    .results {
      display: grid;
      gap: 20px;
    }
    
    .user-card {
      background: #161b22;
      border: 1px solid #30363d;
      border-radius: 10px;
      padding: 20px;
      display: flex;
      gap: 20px;
      align-items: center;
      transition: all 0.3s;
      cursor: pointer;
    }
    
    .user-card:hover {
      border-color: #58a6ff;
      transform: translateY(-2px);
    }
    
    .avatar {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      border: 2px solid #30363d;
    }
    
    .user-info {
      flex: 1;
    }
    
    .username {
      font-size: 20px;
      font-weight: bold;
      color: #58a6ff;
      margin-bottom: 5px;
    }
    
    .bio {
      color: #8b949e;
      margin-bottom: 10px;
    }
    
    .stats {
      display: flex;
      gap: 15px;
      font-size: 14px;
    }
    
    .stat {
      color: #8b949e;
    }
    
    .stat strong {
      color: #c9d1d9;
    }
    
    .error {
      background: #f85149;
      color: white;
      padding: 15px;
      border-radius: 6px;
      text-align: center;
      display: none;
    }
    
    .error.show {
      display: block;
    }
    
    .empty {
      text-align: center;
      padding: 60px 20px;
      color: #8b949e;
    }
    
    .empty-icon {
      font-size: 60px;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>🔍 Recherche GitHub</h1>
    
    <div class="search-box">
      <input type="text" id="searchInput" placeholder="Rechercher un utilisateur...">
    </div>
    
    <div class="loading" id="loading">
      <div>⏳ Recherche en cours...</div>
    </div>
    
    <div class="error" id="error"></div>
    
    <div id="results" class="results">
      <div class="empty">
        <div class="empty-icon">👤</div>
        <p>Entrez un nom d'utilisateur pour commencer</p>
      </div>
    </div>
  </div>
  
  <script>
    const searchInput = document.querySelector('#searchInput');
    const loading = document.querySelector('#loading');
    const error = document.querySelector('#error');
    const results = document.querySelector('#results');
    
    let timeoutId;
    
    // Recherche avec debounce (attendre que l'utilisateur arrête de taper)
    searchInput.addEventListener('input', function() {
      clearTimeout(timeoutId);
      
      const query = searchInput.value.trim();
      
      if (query === '') {
        afficherEmpty();
        return;
      }
      
      // Attendre 500ms après la dernière frappe
      timeoutId = setTimeout(() => {
        rechercherUtilisateurs(query);
      }, 500);
    });
    
    async function rechercherUtilisateurs(query) {
      // Afficher le loading
      loading.classList.add('show');
      error.classList.remove('show');
      results.innerHTML = '';
      
      try {
        // API GitHub - Recherche d'utilisateurs
        const url = `https://api.github.com/search/users?q=${query}&per_page=10`;
        const response = await fetch(url);
        
        if (!response.ok) {
          throw new Error('Erreur API GitHub');
        }
        
        const data = await response.json();
        
        if (data.items.length === 0) {
          results.innerHTML = `
            <div class="empty">
              <div class="empty-icon">🔍</div>
              <p>Aucun utilisateur trouvé pour "${query}"</p>
            </div>
          `;
          return;
        }
        
        // Charger les détails de chaque utilisateur
        await afficherUtilisateurs(data.items);
        
      } catch (erreur) {
        afficherErreur(erreur.message);
      } finally {
        loading.classList.remove('show');
      }
    }
    
    async function afficherUtilisateurs(utilisateurs) {
      // Charger les détails de chaque utilisateur en parallèle
      const promises = utilisateurs.map(user => 
        fetch(user.url).then(res => res.json())
      );
      
      const details = await Promise.all(promises);
      
      // Afficher les cartes
      results.innerHTML = details.map(user => `
        <div class="user-card" onclick="window.open('${user.html_url}', '_blank')">
          <img src="${user.avatar_url}" alt="${user.login}" class="avatar">
          <div class="user-info">
            <div class="username">${user.login}</div>
            ${user.name ? `<div style="color: #c9d1d9; margin-bottom: 5px;">${user.name}</div>` : ''}
            ${user.bio ? `<div class="bio">${user.bio}</div>` : ''}
            <div class="stats">
              <div class="stat">
                <strong>${user.public_repos}</strong> repos
              </div>
              <div class="stat">
                <strong>${user.followers}</strong> followers
              </div>
              <div class="stat">
                <strong>${user.following}</strong> following
              </div>
            </div>
          </div>
        </div>
      `).join('');
    }
    
    function afficherErreur(message) {
      error.textContent = '❌ ' + message;
      error.classList.add('show');
    }
    
    function afficherEmpty() {
      results.innerHTML = `
        <div class="empty">
          <div class="empty-icon">👤</div>
          <p>Entrez un nom d'utilisateur pour commencer</p>
        </div>
      `;
    }
  </script>
</body>
</html>
```

### Concepts utilisés

**1. Debounce**
```javascript
let timeoutId;

searchInput.addEventListener('input', function() {
  clearTimeout(timeoutId);
  
  timeoutId = setTimeout(() => {
    // Exécuter la recherche
  }, 500);
});

// Évite de faire une requête à chaque frappe
// Attend que l'utilisateur arrête de taper
```

**2. Promise.all() pour charger en parallèle**
```javascript
const promises = utilisateurs.map(user => 
  fetch(user.url).then(res => res.json())
);

const details = await Promise.all(promises);

// Charge tous les utilisateurs EN MÊME TEMPS
// Au lieu de les charger un par un
```

**3. API GitHub**
```
https://api.github.com/search/users?q=QUERY

Retourne :
{
  "items": [
    {
      "login": "username",
      "avatar_url": "...",
      "url": "https://api.github.com/users/username"
    }
  ]
}

Puis pour chaque utilisateur :
https://api.github.com/users/username

Retourne les détails complets (bio, repos, followers, etc.)
```

---

## 11. setTimeout et setInterval

### setTimeout - Exécuter une fois après un délai

```javascript
setTimeout(function() {
  console.log('Ceci s\'exécute après 2 secondes');
}, 2000);

// Avec fonction nommée
function direBonjour() {
  console.log('Bonjour !');
}

setTimeout(direBonjour, 3000);

// Avec paramètres
function saluer(nom) {
  console.log('Bonjour ' + nom);
}

setTimeout(saluer, 1000, 'Alice');
// Après 1 seconde : "Bonjour Alice"
```

### Annuler un setTimeout

```javascript
const timeoutId = setTimeout(function() {
  console.log('Ceci ne s\'affichera jamais');
}, 5000);

// Annuler avant qu'il s'exécute
clearTimeout(timeoutId);
```

### setInterval - Exécuter répétitivement

```javascript
setInterval(function() {
  console.log('Ceci s\'exécute toutes les 2 secondes');
}, 2000);

// Compteur
let compteur = 0;

const intervalId = setInterval(function() {
  compteur++;
  console.log('Compteur :', compteur);
  
  if (compteur === 5) {
    clearInterval(intervalId); // Arrêter après 5 fois
  }
}, 1000);
```

### Exemple : Chronomètre

```html
<!DOCTYPE html>
<html>
<body>
  <h1 id="chrono">00:00</h1>
  <button id="startBtn">Démarrer</button>
  <button id="stopBtn">Arrêter</button>
  <button id="resetBtn">Réinitialiser</button>
  
  <script>
    const chrono = document.querySelector('#chrono');
    const startBtn = document.querySelector('#startBtn');
    const stopBtn = document.querySelector('#stopBtn');
    const resetBtn = document.querySelector('#resetBtn');
    
    let secondes = 0;
    let intervalId = null;
    
    function afficher() {
      const minutes = Math.floor(secondes / 60);
      const secs = secondes % 60;
      
      chrono.textContent = 
        String(minutes).padStart(2, '0') + ':' + 
        String(secs).padStart(2, '0');
    }
    
    startBtn.addEventListener('click', function() {
      if (intervalId) return; // Déjà démarré
      
      intervalId = setInterval(function() {
        secondes++;
        afficher();
      }, 1000);
    });
    
    stopBtn.addEventListener('click', function() {
      clearInterval(intervalId);
      intervalId = null;
    });
    
    resetBtn.addEventListener('click', function() {
      clearInterval(intervalId);
      intervalId = null;
      secondes = 0;
      afficher();
    });
  </script>
</body>
</html>
```

### Exemple : Défilement automatique d'images

```javascript
const images = [
  'image1.jpg',
  'image2.jpg',
  'image3.jpg',
  'image4.jpg'
];

let index = 0;
const img = document.querySelector('#carousel-img');

setInterval(function() {
  index = (index + 1) % images.length;
  img.src = images[index];
}, 3000); // Change toutes les 3 secondes
```

### setTimeout récursif vs setInterval

**setInterval** : Délai FIXE entre chaque exécution
```javascript
// Si la fonction prend 2 secondes et interval = 3 secondes
// → Exécutions : 0s, 3s, 6s, 9s...
// Même si la fonction n'est pas terminée
```

**setTimeout récursif** : Délai APRÈS la fin de l'exécution
```javascript
function executer() {
  // Faire quelque chose qui prend du temps
  
  setTimeout(executer, 3000); // Rappeler après 3 secondes
}

executer();

// → Exécutions : 0s, 5s (2s exec + 3s wait), 10s, 15s...
// Attend que la fonction soit terminée
```

**Recommandation :** Utilise setTimeout récursif pour des tâches longues.

---

## 12. Exercices Progressifs

### Exercice 1 : Citation Aléatoire ⭐
**Difficulté : Débutant**

Crée une page qui :
- Affiche une citation aléatoire
- Bouton "Nouvelle citation"
- Utilise l'API : `https://api.quotable.io/random`

**Bonus :**
- Ajoute une animation de transition
- Affiche l'auteur

---

### Exercice 2 : Convertisseur de Devises ⭐⭐
**Difficulté : Intermédiaire**

Crée un convertisseur qui :
- Convertit entre EUR, USD, GBP
- Utilise l'API : `https://api.exchangerate-api.com`
- Met à jour en temps réel

**Bonus :**
- Ajoute plus de devises
- Inverse les devises avec un bouton

---

### Exercice 3 : Recherche de Films ⭐⭐
**Difficulté : Intermédiaire**

Crée une recherche de films avec :
- Recherche par titre
- Affichage des résultats (poster, titre, année)
- Utilise l'API : `https://www.omdbapi.com/` (clé gratuite)

**Bonus :**
- Détails au clic
- Pagination
- Filtres (année, genre)

---

### Exercice 4 : To-Do avec Backend ⭐⭐⭐
**Difficulté : Avancé**

Améliore la To-Do List de la Partie 4 :
- Sauvegarde sur un serveur (JSONPlaceholder)
- Synchronise entre plusieurs onglets
- Gère les conflits

---

### Exercice 5 : Chat en Temps Réel (Polling) ⭐⭐⭐
**Difficulté : Avancé**

Crée un chat qui :
- Envoie des messages
- Récupère les nouveaux messages toutes les 2 secondes
- Affiche la liste des utilisateurs

---

## 13. Patterns Avancés

### Pattern 1 : Wrapper de fetch

```javascript
class API {
  constructor(baseURL) {
    this.baseURL = baseURL;
  }
  
  async get(endpoint) {
    return this._request('GET', endpoint);
  }
  
  async post(endpoint, data) {
    return this._request('POST', endpoint, data);
  }
  
  async put(endpoint, data) {
    return this._request('PUT', endpoint, data);
  }
  
  async delete(endpoint) {
    return this._request('DELETE', endpoint);
  }
  
  async _request(method, endpoint, data = null) {
    const options = {
      method: method,
      headers: {
        'Content-Type': 'application/json'
      }
    };
    
    if (data) {
      options.body = JSON.stringify(data);
    }
    
    try {
      const response = await fetch(this.baseURL + endpoint, options);
      
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }
      
      return await response.json();
      
    } catch (erreur) {
      console.error('Erreur API :', erreur);
      throw erreur;
    }
  }
}

// Utilisation
const api = new API('https://jsonplaceholder.typicode.com');

async function exemple() {
  // GET
  const users = await api.get('/users');
  
  // POST
  const newUser = await api.post('/users', {
    name: 'Alice',
    email: 'alice@example.com'
  });
  
  // PUT
  const updated = await api.put('/users/1', {
    name: 'Alice Dupont'
  });
  
  // DELETE
  await api.delete('/users/1');
}
```

### Pattern 2 : Cache des requêtes

```javascript
class CachedAPI {
  constructor(baseURL, cacheDuration = 60000) {
    this.baseURL = baseURL;
    this.cacheDuration = cacheDuration; // En millisecondes
    this.cache = new Map();
  }
  
  async get(endpoint) {
    const cacheKey = endpoint;
    const cached = this.cache.get(cacheKey);
    
    // Vérifier si en cache et pas expiré
    if (cached && Date.now() - cached.timestamp < this.cacheDuration) {
      console.log('✅ Depuis le cache');
      return cached.data;
    }
    
    // Sinon, faire la requête
    console.log('🌐 Depuis l\'API');
    const response = await fetch(this.baseURL + endpoint);
    const data = await response.json();
    
    // Mettre en cache
    this.cache.set(cacheKey, {
      data: data,
      timestamp: Date.now()
    });
    
    return data;
  }
  
  clearCache() {
    this.cache.clear();
  }
}

// Utilisation
const api = new CachedAPI('https://api.example.com', 30000); // Cache 30s

// Premier appel : depuis l'API
const data1 = await api.get('/users');

// Deuxième appel immédiat : depuis le cache
const data2 = await api.get('/users');
```

### Pattern 3 : Loading state centralisé

```javascript
class LoadingManager {
  constructor() {
    this.loadingCount = 0;
    this.loadingElement = document.querySelector('#loading');
  }
  
  start() {
    this.loadingCount++;
    this.update();
  }
  
  stop() {
    this.loadingCount--;
    if (this.loadingCount < 0) this.loadingCount = 0;
    this.update();
  }
  
  update() {
    if (this.loadingCount > 0) {
      this.loadingElement.style.display = 'block';
    } else {
      this.loadingElement.style.display = 'none';
    }
  }
}

const loading = new LoadingManager();

async function chargerDonnees() {
  loading.start();
  try {
    const data = await fetch('/api/data');
    return await data.json();
  } finally {
    loading.stop();
  }
}

// Même si plusieurs requêtes en parallèle
// Le loading s'affiche jusqu'à ce que TOUTES soient terminées
```

---

## 14. Debugging Asynchrone

### Console.log stratégique

```javascript
async function debugExample() {
  console.log('1. Début');
  
  try {
    console.log('2. Avant fetch');
    const response = await fetch(url);
    console.log('3. Réponse reçue:', response.status);
    
    const data = await response.json();
    console.log('4. Données parsées:', data);
    
    return data;
    
  } catch (erreur) {
    console.log('❌ Erreur attrapée:', erreur);
  }
  
  console.log('5. Fin');
}
```

### DevTools - Network tab

**Ouvre les DevTools (F12) → Onglet Network**

Tu verras :
- Toutes les requêtes HTTP
- Leur status (200, 404, etc.)
- Le temps de réponse
- Les headers
- Le contenu de la réponse

**Astuce :** Filtre par "Fetch/XHR" pour voir seulement les appels API.

### Debugger avec breakpoints

```javascript
async function exemple() {
  const response = await fetch(url);
  
  debugger; // Le code se met en pause ici
  
  const data = await response.json();
  return data;
}
```

**Quand le code se met en pause :**
- Tu peux inspecter les variables
- Avancer pas à pas
- Voir la call stack

### Erreurs communes

**1. Oublier await**
```javascript
// ❌ ERREUR
async function mauvais() {
  const data = fetch(url); // Retourne une Promise, pas les données
  console.log(data); // Promise { <pending> }
}

// ✅ CORRECT
async function bon() {
  const data = await fetch(url);
  console.log(data); // Response object
}
```

**2. Utiliser await hors d'une fonction async**
```javascript
// ❌ ERREUR
function normal() {
  const data = await fetch(url); // SyntaxError
}

// ✅ CORRECT
async function asyncFunc() {
  const data = await fetch(url); // OK
}
```

**3. Oublier try/catch**
```javascript
// ❌ DANGEREUX
async function risque() {
  const data = await fetch(url); // Si ça plante, tout crash
  return data;
}

// ✅ SÛR
async function sur() {
  try {
    const data = await fetch(url);
    return data;
  } catch (erreur) {
    console.error('Erreur:', erreur);
  }
}
```

---

## 15. Aller Plus Loin

### Ce qu'on a couvert

Tu sais maintenant :
- ✅ Comprendre l'asynchrone et l'Event Loop
- ✅ Utiliser les Callbacks
- ✅ Maîtriser les Promises
- ✅ Écrire du code moderne avec async/await
- ✅ Appeler des APIs avec fetch
- ✅ Gérer les erreurs robustement
- ✅ Comprendre REST et CRUD
- ✅ Créer des applications complètes avec APIs
- ✅ Utiliser setTimeout et setInterval
- ✅ Patterns avancés et optimisations

**C'est ÉNORME.** Tu as maintenant les compétences pour construire n'importe quelle application web moderne qui communique avec des serveurs.

### Concepts à approfondir

**1. WebSockets**
- Communication bidirectionnelle en temps réel
- Chat, notifications live, jeux multijoueurs

**2. Service Workers**
- Cache avancé
- Applications offline
- Progressive Web Apps (PWA)

**3. GraphQL**
- Alternative à REST
- Plus flexible et efficace

**4. Server-Sent Events (SSE)**
- Push du serveur vers le client
- Notifications en temps réel

### APIs publiques pour pratiquer

**Gratuites, sans clé :**
- JSONPlaceholder : https://jsonplaceholder.typicode.com/
- Dog CEO : https://dog.ceo/dog-api/ (images de chiens)
- Random User : https://randomuser.me/api/ (utilisateurs fictifs)
- PokéAPI : https://pokeapi.co/ (Pokémon)
- REST Countries : https://restcountries.com/v3.1/all

**Gratuites, avec clé :**
- OpenWeatherMap : https://openweathermap.org/api
- OMDB (films) : https://www.omdbapi.com/
- NASA : https://api.nasa.gov/
- News API : https://newsapi.org/
- Giphy : https://developers.giphy.com/

### Projets pour s'entraîner

**Niveau 1 :**
- Application de blagues aléatoires
- Générateur de noms aléatoires
- Vérificateur de disponibilité de nom d'utilisateur
- Afficheur de faits aléatoires

**Niveau 2 :**
- Application météo complète (avec prévisions)
- Recherche de recettes de cuisine
- Traducteur (API de traduction)
- Pokédex (avec recherche et filtres)

**Niveau 3 :**
- Clone de Twitter (backend + frontend)
- Application de gestion de tâches avec backend
- Dashboard d'analytics
- Plateforme de blog avec CMS

### Ressources

**Documentation :**
- MDN - Fetch API : https://developer.mozilla.org/fr/docs/Web/API/Fetch_API
- MDN - Promises : https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Promise
- JavaScript.info - Async : https://javascript.info/async

**Pratique :**
- APIs publiques : https://github.com/public-apis/public-apis
- Exercices async : https://www.codingame.com/

---

## Conclusion

**FÉLICITATIONS ! 🎉**

Tu viens de terminer le guide le plus complet sur l'asynchrone en JavaScript.

**Ce que tu as appris équivaut à plusieurs mois de bootcamp.**

**Tu peux maintenant :**
- Comprendre comment JavaScript gère l'asynchrone
- Utiliser n'importe quelle API REST
- Créer des applications qui communiquent avec des serveurs
- Gérer des erreurs robustement
- Écrire du code asynchrone propre et maintenable

**La différence entre toi maintenant et toi avant ce guide ?**

**AVANT :** Tu ne pouvais faire que des sites statiques

**MAINTENANT :** Tu peux créer des applications web complètes qui :
- Récupèrent des données en temps réel
- Affichent la météo
- Cherchent sur GitHub
- Communiquent avec n'importe quelle API
- Gèrent des utilisateurs
- Sauvegardent des données

**C'EST LA DIFFÉRENCE ENTRE UN DÉBUTANT ET UN DÉVELOPPEUR PROFESSIONNEL.**

**Prochaine étape ?**
- **PRATIQUE, PRATIQUE, PRATIQUE**
- Fais les exercices
- Crée tes propres projets
- Utilise des APIs publiques
- Construis ton portfolio

**Tu as maintenant les compétences pour postuler à des postes de développeur web junior.**

**Continue comme ça, champion !** 💪🔥

---

**📚 JavaScript Moderne - Partie 5 : Asynchrone, Promises et Fetch API**
**💎 100% Gratuit • Pour Tous • À Jamais**
**🔗 GitHub : [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)**