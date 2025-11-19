# JavaScript - Partie 8 : Projet Final Complet

**🔗 Repository GitHub :** [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)

---

## Table des Matières

### Introduction
1. [Vue d'Ensemble](#1-vue-densemble)
2. [Prérequis et Setup](#2-prérequis-et-setup)

### Projet 1 : E-commerce Pro
3. [E-commerce : Architecture](#3-e-commerce-architecture)
4. [E-commerce : Authentification](#4-e-commerce-authentification)
5. [E-commerce : Catalogue Produits](#5-e-commerce-catalogue-produits)
6. [E-commerce : Panier & Checkout](#6-e-commerce-panier--checkout)
7. [E-commerce : Admin Dashboard](#7-e-commerce-admin-dashboard)
8. [E-commerce : Tests](#8-e-commerce-tests)

### Projet 2 : Réseau Social
9. [Réseau Social : Architecture](#9-réseau-social-architecture)
10. [Réseau Social : Authentification](#10-réseau-social-authentification)
11. [Réseau Social : Posts & Feed](#11-réseau-social-posts--feed)
12. [Réseau Social : Interactions](#12-réseau-social-interactions)
13. [Réseau Social : Profils & Followers](#13-réseau-social-profils--followers)
14. [Réseau Social : Tests](#14-réseau-social-tests)

### Finalisation
15. [Déploiement & CI/CD](#15-déploiement--cicd)
16. [Best Practices Appliquées](#16-best-practices-appliquées)
17. [Portfolio & Carrière](#17-portfolio--carrière)

---

## 1. Vue d'Ensemble

### Pourquoi ce guide ?

**Ce n'est PAS un tutoriel de plus.**

C'est LE projet qui va transformer ton portfolio et ta carrière.

**Après ces projets, tu auras :**
- ✅ **2 applications complètes** production-ready
- ✅ **Code architecture niveau senior**
- ✅ **Tests automatisés**
- ✅ **Applications déployées** (liens live)
- ✅ **Portfolio qui impressionne** les recruteurs
- ✅ **Expérience concrète** pour les entretiens

### Ce que tu vas construire

**Projet 1 : E-commerce Pro "TechStore"**

Une boutique en ligne complète avec :
- 🛍️ Catalogue de produits avec recherche & filtres
- 🛒 Panier persistant
- 💳 Processus de checkout
- 👤 Authentification utilisateur
- 📊 Dashboard admin
- 📱 Responsive design
- ⚡ Performance optimisée

**Stack Tech :**
- JavaScript Vanilla (ES6+)
- Architecture MVC
- LocalStorage + IndexedDB
- CSS Grid + Flexbox
- REST API simulation

**Projet 2 : Réseau Social "DevConnect"**

Plateforme sociale pour développeurs avec :
- 📝 Création de posts
- ❤️ Likes & Commentaires
- 👥 Système de followers
- 🔔 Notifications en temps réel
- 💬 Messagerie
- 🖼️ Upload d'images
- 🌙 Dark mode

**Stack Tech :**
- JavaScript Vanilla (ES6+)
- Architecture Component-based
- IndexedDB
- WebSockets (simulation)
- CSS Variables + Animations

### Philosophie des Projets

**1. Code Production-Ready**
- Architecture scalable
- Error handling complet
- Tests unitaires & intégration
- Documentation

**2. Best Practices Appliquées**
- Design patterns
- SOLID principles
- Clean code
- Security

**3. Portfolio-Oriented**
- Visuellement impressionnant
- Fonctionnalités complètes
- Déployé et accessible
- Code bien organisé sur GitHub

---

## 2. Prérequis et Setup

### Ce que tu DOIS maîtriser

**JavaScript Avancé (Parties 1-7) :**
- ✅ ES6+ (classes, modules, async/await)
- ✅ DOM manipulation
- ✅ Fetch API
- ✅ Closures, this, prototypes
- ✅ Design patterns

**Outils :**
- ✅ Git & GitHub
- ✅ Node.js & npm
- ✅ Terminal/Command line
- ✅ VS Code (ou éditeur)

### Setup Initial

**1. Structure du projet**

```
javascript-projects/
├── ecommerce/
│   ├── src/
│   │   ├── js/
│   │   │   ├── models/
│   │   │   ├── views/
│   │   │   ├── controllers/
│   │   │   ├── services/
│   │   │   ├── utils/
│   │   │   └── main.js
│   │   ├── css/
│   │   ├── assets/
│   │   └── index.html
│   ├── tests/
│   ├── package.json
│   └── README.md
│
├── social-network/
│   ├── src/
│   │   ├── js/
│   │   │   ├── components/
│   │   │   ├── services/
│   │   │   ├── state/
│   │   │   ├── utils/
│   │   │   └── app.js
│   │   ├── css/
│   │   ├── assets/
│   │   └── index.html
│   ├── tests/
│   ├── package.json
│   └── README.md
│
└── README.md
```

**2. Installation des outils**

```bash
# Crée le répertoire principal
mkdir javascript-projects
cd javascript-projects

# E-commerce
mkdir ecommerce
cd ecommerce
npm init -y

# Installe les dépendances
npm install --save-dev \
  vite \
  vitest \
  @vitest/ui \
  jsdom \
  eslint \
  prettier

# Retour et Social Network
cd ..
mkdir social-network
cd social-network
npm init -y

npm install --save-dev \
  vite \
  vitest \
  @vitest/ui \
  jsdom \
  eslint \
  prettier
```

**3. Configuration Vite**

```javascript
// vite.config.js (pour chaque projet)
import { defineConfig } from 'vite';

export default defineConfig({
  root: 'src',
  build: {
    outDir: '../dist',
    emptyOutDir: true
  },
  server: {
    port: 3000,
    open: true
  }
});
```

**4. Configuration des tests**

```javascript
// vitest.config.js
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './tests/setup.js'
  }
});
```

**5. Scripts package.json**

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest --coverage",
    "lint": "eslint src/**/*.js",
    "format": "prettier --write src/**/*.js"
  }
}
```

---

## 3. E-commerce : Architecture

### Architecture MVC Professionnelle

**Pourquoi MVC ?**
- ✅ Séparation des responsabilités
- ✅ Code maintenable
- ✅ Testable facilement
- ✅ Scalable

**Structure détaillée :**

```
ecommerce/src/js/
├── models/              # Logique métier & données
│   ├── Product.js
│   ├── User.js
│   ├── Cart.js
│   └── Order.js
│
├── views/               # Interface utilisateur
│   ├── ProductView.js
│   ├── CartView.js
│   ├── CheckoutView.js
│   └── AuthView.js
│
├── controllers/         # Coordination Model-View
│   ├── ProductController.js
│   ├── CartController.js
│   ├── AuthController.js
│   └── AppController.js
│
├── services/           # Services externes
│   ├── StorageService.js
│   ├── ApiService.js
│   └── AuthService.js
│
├── utils/              # Utilitaires
│   ├── validators.js
│   ├── formatters.js
│   └── helpers.js
│
└── main.js             # Point d'entrée
```

### Model : Product

```javascript
// models/Product.js

export class Product {
  constructor({ id, name, description, price, image, category, stock, rating }) {
    this.id = id;
    this.name = name;
    this.description = description;
    this.price = price;
    this.image = image;
    this.category = category;
    this.stock = stock;
    this.rating = rating || 0;
  }

  // Business logic
  isInStock() {
    return this.stock > 0;
  }

  getFormattedPrice() {
    return new Intl.NumberFormat('fr-FR', {
      style: 'currency',
      currency: 'EUR'
    }).format(this.price);
  }

  getDiscountedPrice(discountPercent) {
    return this.price * (1 - discountPercent / 100);
  }

  // Validation
  static validate(data) {
    const errors = [];

    if (!data.name || data.name.trim().length < 3) {
      errors.push('Le nom doit contenir au moins 3 caractères');
    }

    if (!data.price || data.price <= 0) {
      errors.push('Le prix doit être supérieur à 0');
    }

    if (!data.stock || data.stock < 0) {
      errors.push('Le stock ne peut pas être négatif');
    }

    return {
      isValid: errors.length === 0,
      errors
    };
  }

  // Serialization
  toJSON() {
    return {
      id: this.id,
      name: this.name,
      description: this.description,
      price: this.price,
      image: this.image,
      category: this.category,
      stock: this.stock,
      rating: this.rating
    };
  }

  static fromJSON(json) {
    return new Product(json);
  }
}
```

### Model : Cart

```javascript
// models/Cart.js

export class Cart {
  constructor() {
    this.items = []; // { product, quantity }
  }

  addItem(product, quantity = 1) {
    // Vérifie si le produit existe déjà
    const existingItem = this.items.find(item => item.product.id === product.id);

    if (existingItem) {
      existingItem.quantity += quantity;
    } else {
      this.items.push({ product, quantity });
    }
  }

  removeItem(productId) {
    this.items = this.items.filter(item => item.product.id !== productId);
  }

  updateQuantity(productId, quantity) {
    const item = this.items.find(item => item.product.id === productId);
    
    if (item) {
      if (quantity <= 0) {
        this.removeItem(productId);
      } else {
        item.quantity = quantity;
      }
    }
  }

  clear() {
    this.items = [];
  }

  getTotal() {
    return this.items.reduce((total, item) => {
      return total + (item.product.price * item.quantity);
    }, 0);
  }

  getItemCount() {
    return this.items.reduce((count, item) => count + item.quantity, 0);
  }

  getFormattedTotal() {
    return new Intl.NumberFormat('fr-FR', {
      style: 'currency',
      currency: 'EUR'
    }).format(this.getTotal());
  }

  // Serialization
  toJSON() {
    return {
      items: this.items.map(item => ({
        product: item.product.toJSON(),
        quantity: item.quantity
      }))
    };
  }

  static fromJSON(json) {
    const cart = new Cart();
    
    if (json.items) {
      cart.items = json.items.map(item => ({
        product: Product.fromJSON(item.product),
        quantity: item.quantity
      }));
    }

    return cart;
  }
}
```

### Model : User

```javascript
// models/User.js

export class User {
  constructor({ id, email, name, role = 'customer', createdAt }) {
    this.id = id;
    this.email = email;
    this.name = name;
    this.role = role; // 'customer' | 'admin'
    this.createdAt = createdAt || new Date().toISOString();
  }

  isAdmin() {
    return this.role === 'admin';
  }

  // Validation
  static validateEmail(email) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
  }

  static validatePassword(password) {
    // Au moins 8 caractères, 1 majuscule, 1 chiffre
    return password.length >= 8 && 
           /[A-Z]/.test(password) && 
           /[0-9]/.test(password);
  }

  static validate(data) {
    const errors = [];

    if (!data.email || !User.validateEmail(data.email)) {
      errors.push('Email invalide');
    }

    if (!data.name || data.name.trim().length < 2) {
      errors.push('Le nom doit contenir au moins 2 caractères');
    }

    if (data.password && !User.validatePassword(data.password)) {
      errors.push('Le mot de passe doit contenir au moins 8 caractères, une majuscule et un chiffre');
    }

    return {
      isValid: errors.length === 0,
      errors
    };
  }

  toJSON() {
    return {
      id: this.id,
      email: this.email,
      name: this.name,
      role: this.role,
      createdAt: this.createdAt
    };
  }

  static fromJSON(json) {
    return new User(json);
  }
}
```

### Service : Storage

```javascript
// services/StorageService.js

export class StorageService {
  constructor(storageKey = 'techstore') {
    this.storageKey = storageKey;
  }

  // LocalStorage pour données simples
  save(key, data) {
    try {
      const fullKey = `${this.storageKey}_${key}`;
      localStorage.setItem(fullKey, JSON.stringify(data));
      return true;
    } catch (error) {
      console.error('Error saving to localStorage:', error);
      return false;
    }
  }

  load(key) {
    try {
      const fullKey = `${this.storageKey}_${key}`;
      const data = localStorage.getItem(fullKey);
      return data ? JSON.parse(data) : null;
    } catch (error) {
      console.error('Error loading from localStorage:', error);
      return null;
    }
  }

  remove(key) {
    try {
      const fullKey = `${this.storageKey}_${key}`;
      localStorage.removeItem(fullKey);
      return true;
    } catch (error) {
      console.error('Error removing from localStorage:', error);
      return false;
    }
  }

  clear() {
    try {
      // Supprime seulement les clés de notre app
      Object.keys(localStorage)
        .filter(key => key.startsWith(this.storageKey))
        .forEach(key => localStorage.removeItem(key));
      return true;
    } catch (error) {
      console.error('Error clearing localStorage:', error);
      return false;
    }
  }

  // IndexedDB pour données plus complexes
  async saveToIndexedDB(storeName, data) {
    return new Promise((resolve, reject) => {
      const request = indexedDB.open(this.storageKey, 1);

      request.onerror = () => reject(request.error);

      request.onsuccess = () => {
        const db = request.result;
        const transaction = db.transaction([storeName], 'readwrite');
        const store = transaction.objectStore(storeName);
        
        const saveRequest = store.put(data);

        saveRequest.onsuccess = () => resolve(true);
        saveRequest.onerror = () => reject(saveRequest.error);
      };

      request.onupgradeneeded = (event) => {
        const db = event.target.result;
        
        if (!db.objectStoreNames.contains(storeName)) {
          db.createObjectStore(storeName, { keyPath: 'id', autoIncrement: true });
        }
      };
    });
  }

  async loadFromIndexedDB(storeName, id) {
    return new Promise((resolve, reject) => {
      const request = indexedDB.open(this.storageKey, 1);

      request.onerror = () => reject(request.error);

      request.onsuccess = () => {
        const db = request.result;
        const transaction = db.transaction([storeName], 'readonly');
        const store = transaction.objectStore(storeName);
        
        const getRequest = store.get(id);

        getRequest.onsuccess = () => resolve(getRequest.result);
        getRequest.onerror = () => reject(getRequest.error);
      };
    });
  }

  async getAllFromIndexedDB(storeName) {
    return new Promise((resolve, reject) => {
      const request = indexedDB.open(this.storageKey, 1);

      request.onerror = () => reject(request.error);

      request.onsuccess = () => {
        const db = request.result;
        const transaction = db.transaction([storeName], 'readonly');
        const store = transaction.objectStore(storeName);
        
        const getAllRequest = store.getAll();

        getAllRequest.onsuccess = () => resolve(getAllRequest.result);
        getAllRequest.onerror = () => reject(getAllRequest.error);
      };
    });
  }
}

// Singleton
export const storage = new StorageService();
```

### Controller : Product

```javascript
// controllers/ProductController.js

import { Product } from '../models/Product.js';
import { ProductView } from '../views/ProductView.js';
import { storage } from '../services/StorageService.js';

export class ProductController {
  constructor() {
    this.view = new ProductView();
    this.products = [];
    this.filteredProducts = [];
    this.currentCategory = 'all';
    this.searchQuery = '';
    this.sortBy = 'default';
  }

  async init() {
    await this.loadProducts();
    this.setupEventListeners();
    this.render();
  }

  async loadProducts() {
    // Charge depuis le storage ou utilise des données par défaut
    const savedProducts = storage.load('products');

    if (savedProducts && savedProducts.length > 0) {
      this.products = savedProducts.map(p => Product.fromJSON(p));
    } else {
      // Données initiales
      this.products = this.getInitialProducts();
      this.saveProducts();
    }

    this.filteredProducts = [...this.products];
  }

  getInitialProducts() {
    // Données de démo
    return [
      new Product({
        id: 1,
        name: 'MacBook Pro M3',
        description: 'Le laptop le plus puissant pour les développeurs',
        price: 2499,
        image: '/assets/macbook.jpg',
        category: 'laptops',
        stock: 15,
        rating: 4.8
      }),
      new Product({
        id: 2,
        name: 'iPhone 15 Pro',
        description: 'Le smartphone le plus avancé',
        price: 1299,
        image: '/assets/iphone.jpg',
        category: 'phones',
        stock: 30,
        rating: 4.7
      }),
      new Product({
        id: 3,
        name: 'AirPods Pro',
        description: 'Écouteurs sans fil avec réduction de bruit',
        price: 279,
        image: '/assets/airpods.jpg',
        category: 'audio',
        stock: 50,
        rating: 4.6
      }),
      new Product({
        id: 4,
        name: 'iPad Air',
        description: 'Tablette polyvalente et puissante',
        price: 699,
        image: '/assets/ipad.jpg',
        category: 'tablets',
        stock: 20,
        rating: 4.5
      }),
      new Product({
        id: 5,
        name: 'Apple Watch Ultra',
        description: 'Montre connectée pour les aventuriers',
        price: 899,
        image: '/assets/watch.jpg',
        category: 'wearables',
        stock: 25,
        rating: 4.9
      })
    ];
  }

  setupEventListeners() {
    // Recherche
    this.view.onSearch((query) => {
      this.searchQuery = query;
      this.applyFilters();
    });

    // Filtrage par catégorie
    this.view.onCategoryChange((category) => {
      this.currentCategory = category;
      this.applyFilters();
    });

    // Tri
    this.view.onSortChange((sortBy) => {
      this.sortBy = sortBy;
      this.sortProducts();
      this.render();
    });

    // Ajout au panier
    this.view.onAddToCart((productId) => {
      this.handleAddToCart(productId);
    });
  }

  applyFilters() {
    this.filteredProducts = this.products.filter(product => {
      // Filtre par catégorie
      const categoryMatch = this.currentCategory === 'all' || 
                           product.category === this.currentCategory;

      // Filtre par recherche
      const searchMatch = !this.searchQuery ||
                         product.name.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
                         product.description.toLowerCase().includes(this.searchQuery.toLowerCase());

      return categoryMatch && searchMatch;
    });

    this.sortProducts();
    this.render();
  }

  sortProducts() {
    switch (this.sortBy) {
      case 'price-asc':
        this.filteredProducts.sort((a, b) => a.price - b.price);
        break;
      case 'price-desc':
        this.filteredProducts.sort((a, b) => b.price - a.price);
        break;
      case 'name':
        this.filteredProducts.sort((a, b) => a.name.localeCompare(b.name));
        break;
      case 'rating':
        this.filteredProducts.sort((a, b) => b.rating - a.rating);
        break;
      default:
        // Tri par défaut (ordre original)
        break;
    }
  }

  handleAddToCart(productId) {
    const product = this.products.find(p => p.id === productId);
    
    if (!product) {
      this.view.showError('Produit introuvable');
      return;
    }

    if (!product.isInStock()) {
      this.view.showError('Produit en rupture de stock');
      return;
    }

    // Dispatch event pour le CartController
    window.dispatchEvent(new CustomEvent('addToCart', {
      detail: { product, quantity: 1 }
    }));

    this.view.showSuccess(`${product.name} ajouté au panier`);
  }

  render() {
    this.view.render(this.filteredProducts);
  }

  saveProducts() {
    storage.save('products', this.products.map(p => p.toJSON()));
  }

  // Admin methods
  async addProduct(productData) {
    const validation = Product.validate(productData);

    if (!validation.isValid) {
      return { success: false, errors: validation.errors };
    }

    const newProduct = new Product({
      ...productData,
      id: Date.now() // Simple ID generation
    });

    this.products.push(newProduct);
    this.saveProducts();
    this.applyFilters();

    return { success: true, product: newProduct };
  }

  async updateProduct(id, productData) {
    const index = this.products.findIndex(p => p.id === id);

    if (index === -1) {
      return { success: false, error: 'Produit introuvable' };
    }

    const validation = Product.validate(productData);

    if (!validation.isValid) {
      return { success: false, errors: validation.errors };
    }

    this.products[index] = new Product({ ...productData, id });
    this.saveProducts();
    this.applyFilters();

    return { success: true };
  }

  async deleteProduct(id) {
    const index = this.products.findIndex(p => p.id === id);

    if (index === -1) {
      return { success: false, error: 'Produit introuvable' };
    }

    this.products.splice(index, 1);
    this.saveProducts();
    this.applyFilters();

    return { success: true };
  }
}
```

### View : Product

```javascript
// views/ProductView.js

export class ProductView {
  constructor() {
    this.container = document.getElementById('products-container');
    this.searchInput = document.getElementById('search-input');
    this.categoryFilter = document.getElementById('category-filter');
    this.sortSelect = document.getElementById('sort-select');
  }

  render(products) {
    if (!this.container) return;

    if (products.length === 0) {
      this.container.innerHTML = this.renderEmptyState();
      return;
    }

    this.container.innerHTML = products.map(product => 
      this.renderProductCard(product)
    ).join('');

    this.attachCardListeners();
  }

  renderProductCard(product) {
    return `
      <div class="product-card" data-product-id="${product.id}">
        <div class="product-image">
          <img src="${product.image}" alt="${product.name}" loading="lazy">
          ${!product.isInStock() ? '<span class="badge badge-danger">Rupture de stock</span>' : ''}
          ${product.rating >= 4.5 ? '<span class="badge badge-success">Top ventes</span>' : ''}
        </div>
        
        <div class="product-info">
          <span class="product-category">${this.formatCategory(product.category)}</span>
          <h3 class="product-name">${product.name}</h3>
          <p class="product-description">${product.description}</p>
          
          <div class="product-rating">
            ${this.renderStars(product.rating)}
            <span class="rating-value">(${product.rating})</span>
          </div>
          
          <div class="product-footer">
            <div class="product-price">
              <span class="price">${product.getFormattedPrice()}</span>
              ${product.stock < 10 ? `<span class="stock-warning">Plus que ${product.stock} en stock</span>` : ''}
            </div>
            
            <button 
              class="btn btn-primary add-to-cart-btn" 
              data-product-id="${product.id}"
              ${!product.isInStock() ? 'disabled' : ''}
            >
              ${product.isInStock() ? 
                '<i class="icon-cart"></i> Ajouter au panier' : 
                'Indisponible'
              }
            </button>
          </div>
        </div>
      </div>
    `;
  }

  renderStars(rating) {
    const fullStars = Math.floor(rating);
    const hasHalfStar = rating % 1 >= 0.5;
    const emptyStars = 5 - fullStars - (hasHalfStar ? 1 : 0);

    return `
      <div class="stars">
        ${'<i class="icon-star-full"></i>'.repeat(fullStars)}
        ${hasHalfStar ? '<i class="icon-star-half"></i>' : ''}
        ${'<i class="icon-star-empty"></i>'.repeat(emptyStars)}
      </div>
    `;
  }

  renderEmptyState() {
    return `
      <div class="empty-state">
        <i class="icon-search-empty"></i>
        <h3>Aucun produit trouvé</h3>
        <p>Essayez de modifier vos critères de recherche</p>
      </div>
    `;
  }

  formatCategory(category) {
    const categories = {
      'laptops': 'Ordinateurs',
      'phones': 'Smartphones',
      'tablets': 'Tablettes',
      'audio': 'Audio',
      'wearables': 'Montres & Accessoires'
    };

    return categories[category] || category;
  }

  attachCardListeners() {
    const addToCartButtons = this.container.querySelectorAll('.add-to-cart-btn');
    
    addToCartButtons.forEach(button => {
      button.addEventListener('click', (e) => {
        const productId = parseInt(e.currentTarget.dataset.productId);
        this.handleAddToCart(productId);
      });
    });
  }

  // Event handlers
  onSearch(callback) {
    if (!this.searchInput) return;

    let debounceTimer;
    this.searchInput.addEventListener('input', (e) => {
      clearTimeout(debounceTimer);
      debounceTimer = setTimeout(() => {
        callback(e.target.value.trim());
      }, 300);
    });
  }

  onCategoryChange(callback) {
    if (!this.categoryFilter) return;

    this.categoryFilter.addEventListener('change', (e) => {
      callback(e.target.value);
    });
  }

  onSortChange(callback) {
    if (!this.sortSelect) return;

    this.sortSelect.addEventListener('change', (e) => {
      callback(e.target.value);
    });
  }

  onAddToCart(callback) {
    this.addToCartCallback = callback;
  }

  handleAddToCart(productId) {
    if (this.addToCartCallback) {
      this.addToCartCallback(productId);
    }
  }

  // Feedback
  showSuccess(message) {
    this.showToast(message, 'success');
  }

  showError(message) {
    this.showToast(message, 'error');
  }

  showToast(message, type = 'info') {
    const toast = document.createElement('div');
    toast.className = `toast toast-${type}`;
    toast.textContent = message;

    document.body.appendChild(toast);

    // Animation d'entrée
    setTimeout(() => toast.classList.add('show'), 10);

    // Suppression après 3 secondes
    setTimeout(() => {
      toast.classList.remove('show');
      setTimeout(() => toast.remove(), 300);
    }, 3000);
  }
}
```

### Main Entry Point

```javascript
// main.js

import { ProductController } from './controllers/ProductController.js';
import { CartController } from './controllers/CartController.js';
import { AuthController } from './controllers/AuthController.js';

class App {
  constructor() {
    this.productController = null;
    this.cartController = null;
    this.authController = null;
  }

  async init() {
    try {
      // Initialize controllers
      this.authController = new AuthController();
      await this.authController.init();

      this.cartController = new CartController();
      await this.cartController.init();

      this.productController = new ProductController();
      await this.productController.init();

      // Setup global event listeners
      this.setupGlobalListeners();

      console.log('✅ TechStore initialized successfully');
    } catch (error) {
      console.error('❌ Error initializing app:', error);
      this.showError('Erreur lors du chargement de l\'application');
    }
  }

  setupGlobalListeners() {
    // Navigation
    document.addEventListener('click', (e) => {
      if (e.target.matches('[data-nav]')) {
        e.preventDefault();
        const page = e.target.dataset.nav;
        this.navigateTo(page);
      }
    });

    // Responsive menu
    const menuToggle = document.getElementById('menu-toggle');
    if (menuToggle) {
      menuToggle.addEventListener('click', () => {
        document.body.classList.toggle('menu-open');
      });
    }
  }

  navigateTo(page) {
    // Simple SPA navigation
    const pages = document.querySelectorAll('[data-page]');
    pages.forEach(p => p.classList.remove('active'));

    const targetPage = document.querySelector(`[data-page="${page}"]`);
    if (targetPage) {
      targetPage.classList.add('active');
    }

    // Update URL sans recharger
    history.pushState({ page }, '', `/${page}`);
  }

  showError(message) {
    const errorDiv = document.createElement('div');
    errorDiv.className = 'error-message';
    errorDiv.textContent = message;
    document.body.appendChild(errorDiv);

    setTimeout(() => errorDiv.remove(), 5000);
  }
}

// Initialize app when DOM is ready
if (document.readyState === 'loading') {
  document.addEventListener('DOMContentLoaded', () => {
    const app = new App();
    app.init();
  });
} else {
  const app = new App();
  app.init();
}
```

---

## 4. E-commerce : Authentification

### Service d'Authentification

```javascript
// services/AuthService.js

import { User } from '../models/User.js';
import { storage } from './StorageService.js';

export class AuthService {
  constructor() {
    this.currentUser = null;
    this.sessionTimeout = 3600000; // 1 heure
  }

  async init() {
    // Vérifie si une session existe
    const session = storage.load('session');
    
    if (session && this.isSessionValid(session)) {
      this.currentUser = User.fromJSON(session.user);
      this.refreshSession();
    } else {
      this.clearSession();
    }
  }

  async register(email, password, name) {
    // Validation
    const validation = User.validate({ email, name, password });
    
    if (!validation.isValid) {
      return { success: false, errors: validation.errors };
    }

    // Vérifie si l'utilisateur existe déjà
    const users = storage.load('users') || [];
    const existingUser = users.find(u => u.email === email);

    if (existingUser) {
      return { success: false, errors: ['Cet email est déjà utilisé'] };
    }

    // Hash le mot de passe (simulation - en prod utiliser bcrypt côté serveur)
    const hashedPassword = await this.hashPassword(password);

    // Crée le nouvel utilisateur
    const newUser = new User({
      id: Date.now(),
      email,
      name,
      role: 'customer'
    });

    // Sauvegarde
    users.push({
      ...newUser.toJSON(),
      password: hashedPassword
    });

    storage.save('users', users);

    // Connexion automatique
    await this.login(email, password);

    return { success: true, user: newUser };
  }

  async login(email, password) {
    const users = storage.load('users') || [];
    const user = users.find(u => u.email === email);

    if (!user) {
      return { success: false, errors: ['Email ou mot de passe incorrect'] };
    }

    // Vérifie le mot de passe
    const isPasswordValid = await this.verifyPassword(password, user.password);

    if (!isPasswordValid) {
      return { success: false, errors: ['Email ou mot de passe incorrect'] };
    }

    // Crée la session
    this.currentUser = User.fromJSON(user);
    this.createSession();

    return { success: true, user: this.currentUser };
  }

  logout() {
    this.currentUser = null;
    this.clearSession();
    
    // Dispatch event
    window.dispatchEvent(new CustomEvent('userLogout'));
  }

  isAuthenticated() {
    return this.currentUser !== null;
  }

  getCurrentUser() {
    return this.currentUser;
  }

  isAdmin() {
    return this.currentUser && this.currentUser.isAdmin();
  }

  // Session management
  createSession() {
    const session = {
      user: this.currentUser.toJSON(),
      expiresAt: Date.now() + this.sessionTimeout
    };

    storage.save('session', session);

    // Auto-refresh session
    this.setupSessionRefresh();
  }

  refreshSession() {
    if (this.currentUser) {
      this.createSession();
    }
  }

  isSessionValid(session) {
    return session && session.expiresAt > Date.now();
  }

  clearSession() {
    storage.remove('session');
    this.currentUser = null;
  }

  setupSessionRefresh() {
    // Rafraîchit la session toutes les 30 minutes
    setInterval(() => {
      if (this.currentUser) {
        this.refreshSession();
      }
    }, 1800000); // 30 minutes
  }

  // Password hashing (SIMULATION - en prod utiliser bcrypt côté serveur)
  async hashPassword(password) {
    // Simple hash pour la démo
    // EN PRODUCTION : NE JAMAIS faire ça, utiliser bcrypt côté serveur
    const encoder = new TextEncoder();
    const data = encoder.encode(password + 'salt-demo-insecure');
    const hashBuffer = await crypto.subtle.digest('SHA-256', data);
    const hashArray = Array.from(new Uint8Array(hashBuffer));
    return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
  }

  async verifyPassword(password, hashedPassword) {
    const hash = await this.hashPassword(password);
    return hash === hashedPassword;
  }

  // Password reset (simulation)
  async requestPasswordReset(email) {
    const users = storage.load('users') || [];
    const user = users.find(u => u.email === email);

    if (!user) {
      // Ne pas révéler si l'email existe
      return { success: true };
    }

    // En prod : Envoyer un email avec un token
    console.log('Password reset email would be sent to:', email);

    return { success: true };
  }

  async resetPassword(email, token, newPassword) {
    // En prod : Vérifier le token
    const validation = User.validatePassword(newPassword);

    if (!validation) {
      return { success: false, errors: ['Mot de passe invalide'] };
    }

    const users = storage.load('users') || [];
    const userIndex = users.findIndex(u => u.email === email);

    if (userIndex === -1) {
      return { success: false, errors: ['Utilisateur introuvable'] };
    }

    const hashedPassword = await this.hashPassword(newPassword);
    users[userIndex].password = hashedPassword;

    storage.save('users', users);

    return { success: true };
  }
}

// Singleton
export const authService = new AuthService();
```

### Controller d'Authentification

```javascript
// controllers/AuthController.js

import { AuthView } from '../views/AuthView.js';
import { authService } from '../services/AuthService.js';

export class AuthController {
  constructor() {
    this.view = new AuthView();
  }

  async init() {
    await authService.init();
    this.setupEventListeners();
    this.updateUI();
  }

  setupEventListeners() {
    // Login
    this.view.onLogin(async (email, password) => {
      await this.handleLogin(email, password);
    });

    // Register
    this.view.onRegister(async (email, password, name) => {
      await this.handleRegister(email, password, name);
    });

    // Logout
    this.view.onLogout(() => {
      this.handleLogout();
    });

    // Écoute les changements d'auth
    window.addEventListener('userLogout', () => {
      this.updateUI();
    });
  }

  async handleLogin(email, password) {
    this.view.showLoading(true);

    const result = await authService.login(email, password);

    this.view.showLoading(false);

    if (result.success) {
      this.view.showSuccess('Connexion réussie');
      this.view.closeModal();
      this.updateUI();
      
      // Redirect to home
      window.location.hash = '#home';
    } else {
      this.view.showErrors(result.errors);
    }
  }

  async handleRegister(email, password, name) {
    this.view.showLoading(true);

    const result = await authService.register(email, password, name);

    this.view.showLoading(false);

    if (result.success) {
      this.view.showSuccess('Compte créé avec succès');
      this.view.closeModal();
      this.updateUI();
      
      // Redirect to home
      window.location.hash = '#home';
    } else {
      this.view.showErrors(result.errors);
    }
  }

  handleLogout() {
    authService.logout();
    this.view.showSuccess('Déconnexion réussie');
    this.updateUI();
    
    // Redirect to home
    window.location.hash = '#home';
  }

  updateUI() {
    const user = authService.getCurrentUser();
    this.view.updateUserUI(user);
  }
}
```

### View d'Authentification

```javascript
// views/AuthView.js

export class AuthView {
  constructor() {
    this.loginForm = document.getElementById('login-form');
    this.registerForm = document.getElementById('register-form');
    this.userMenu = document.getElementById('user-menu');
    this.loginBtn = document.getElementById('login-btn');
    this.logoutBtn = document.getElementById('logout-btn');
    this.modal = document.getElementById('auth-modal');
  }

  updateUserUI(user) {
    if (user) {
      // Utilisateur connecté
      if (this.loginBtn) this.loginBtn.style.display = 'none';
      if (this.userMenu) {
        this.userMenu.style.display = 'block';
        this.userMenu.querySelector('.user-name').textContent = user.name;
      }

      // Affiche le menu admin si admin
      const adminMenu = document.getElementById('admin-menu');
      if (adminMenu) {
        adminMenu.style.display = user.isAdmin() ? 'block' : 'none';
      }
    } else {
      // Utilisateur non connecté
      if (this.loginBtn) this.loginBtn.style.display = 'block';
      if (this.userMenu) this.userMenu.style.display = 'none';

      const adminMenu = document.getElementById('admin-menu');
      if (adminMenu) adminMenu.style.display = 'none';
    }
  }

  onLogin(callback) {
    if (!this.loginForm) return;

    this.loginForm.addEventListener('submit', (e) => {
      e.preventDefault();

      const email = this.loginForm.querySelector('[name="email"]').value;
      const password = this.loginForm.querySelector('[name="password"]').value;

      callback(email, password);
    });
  }

  onRegister(callback) {
    if (!this.registerForm) return;

    this.registerForm.addEventListener('submit', (e) => {
      e.preventDefault();

      const email = this.registerForm.querySelector('[name="email"]').value;
      const password = this.registerForm.querySelector('[name="password"]').value;
      const name = this.registerForm.querySelector('[name="name"]').value;

      callback(email, password, name);
    });
  }

  onLogout(callback) {
    if (!this.logoutBtn) return;

    this.logoutBtn.addEventListener('click', (e) => {
      e.preventDefault();
      callback();
    });
  }

  showLoading(show) {
    const buttons = document.querySelectorAll('.auth-submit-btn');
    buttons.forEach(btn => {
      btn.disabled = show;
      btn.innerHTML = show ? '<span class="spinner"></span> Chargement...' : 'Valider';
    });
  }

  showErrors(errors) {
    const errorContainer = document.querySelector('.auth-errors');
    if (!errorContainer) return;

    errorContainer.innerHTML = errors.map(error => 
      `<div class="error-message">${error}</div>`
    ).join('');

    errorContainer.style.display = 'block';
  }

  showSuccess(message) {
    const toast = document.createElement('div');
    toast.className = 'toast toast-success';
    toast.textContent = message;

    document.body.appendChild(toast);
    setTimeout(() => toast.classList.add('show'), 10);
    setTimeout(() => {
      toast.classList.remove('show');
      setTimeout(() => toast.remove(), 300);
    }, 3000);
  }

  closeModal() {
    if (this.modal) {
      this.modal.classList.remove('show');
    }
  }

  openModal(tab = 'login') {
    if (this.modal) {
      this.modal.classList.add('show');
      
      // Switch to the right tab
      const tabButtons = this.modal.querySelectorAll('[data-tab]');
      tabButtons.forEach(btn => {
        if (btn.dataset.tab === tab) {
          btn.click();
        }
      });
    }
  }
}
```

---

*Note : Ce fichier contient les bases de l'architecture et de l'authentification. La suite du guide couvrira le panier, le checkout, l'admin dashboard, puis le projet réseau social complet.*

**📚 JavaScript - Partie 8 : Projet Final (1/3)**
**💎 100% Gratuit • Pour Tous • À Jamais**
**🔗 GitHub : [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)**