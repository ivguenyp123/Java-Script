# JavaScript - Partie 8.2 : E-commerce Complet (Cart, Admin, Tests)

**🔗 Repository GitHub :** [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)

---

## Table des Matières - Partie 2

5. [E-commerce : Panier & Checkout](#5-e-commerce-panier--checkout)
6. [E-commerce : Admin Dashboard](#6-e-commerce-admin-dashboard)
7. [E-commerce : Tests Complets](#7-e-commerce-tests-complets)
8. [E-commerce : Performance & Optimisation](#8-e-commerce-performance--optimisation)

---

## 5. E-commerce : Panier & Checkout

### Controller : Cart

```javascript
// controllers/CartController.js

import { Cart } from '../models/Cart.js';
import { CartView } from '../views/CartView.js';
import { storage } from '../services/StorageService.js';
import { authService } from '../services/AuthService.js';

export class CartController {
  constructor() {
    this.cart = new Cart();
    this.view = new CartView();
  }

  async init() {
    await this.loadCart();
    this.setupEventListeners();
    this.render();
  }

  async loadCart() {
    const savedCart = storage.load('cart');
    
    if (savedCart) {
      this.cart = Cart.fromJSON(savedCart);
    }
  }

  setupEventListeners() {
    // Écoute les événements d'ajout au panier depuis ProductController
    window.addEventListener('addToCart', (e) => {
      this.addItem(e.detail.product, e.detail.quantity);
    });

    // Mise à jour quantité
    this.view.onUpdateQuantity((productId, quantity) => {
      this.updateQuantity(productId, quantity);
    });

    // Suppression d'item
    this.view.onRemoveItem((productId) => {
      this.removeItem(productId);
    });

    // Checkout
    this.view.onCheckout(() => {
      this.handleCheckout();
    });

    // Vider le panier
    this.view.onClearCart(() => {
      this.clearCart();
    });
  }

  addItem(product, quantity = 1) {
    // Vérifie le stock
    if (quantity > product.stock) {
      this.view.showError(`Seulement ${product.stock} disponible(s)`);
      return;
    }

    this.cart.addItem(product, quantity);
    this.saveCart();
    this.render();
    this.updateCartBadge();

    this.view.showSuccess(`${product.name} ajouté au panier`);
  }

  updateQuantity(productId, quantity) {
    const item = this.cart.items.find(item => item.product.id === productId);
    
    if (!item) return;

    // Vérifie le stock
    if (quantity > item.product.stock) {
      this.view.showError(`Seulement ${item.product.stock} disponible(s)`);
      return;
    }

    this.cart.updateQuantity(productId, quantity);
    this.saveCart();
    this.render();
    this.updateCartBadge();
  }

  removeItem(productId) {
    this.cart.removeItem(productId);
    this.saveCart();
    this.render();
    this.updateCartBadge();

    this.view.showSuccess('Produit retiré du panier');
  }

  clearCart() {
    if (confirm('Êtes-vous sûr de vouloir vider le panier ?')) {
      this.cart.clear();
      this.saveCart();
      this.render();
      this.updateCartBadge();

      this.view.showSuccess('Panier vidé');
    }
  }

  handleCheckout() {
    if (this.cart.items.length === 0) {
      this.view.showError('Votre panier est vide');
      return;
    }

    if (!authService.isAuthenticated()) {
      this.view.showError('Vous devez être connecté pour commander');
      // Ouvre le modal de connexion
      window.dispatchEvent(new CustomEvent('openAuthModal'));
      return;
    }

    // Navigue vers le checkout
    window.location.hash = '#checkout';
  }

  saveCart() {
    storage.save('cart', this.cart.toJSON());
  }

  render() {
    this.view.render(this.cart);
  }

  updateCartBadge() {
    const badge = document.getElementById('cart-badge');
    if (badge) {
      const count = this.cart.getItemCount();
      badge.textContent = count;
      badge.style.display = count > 0 ? 'block' : 'none';
    }
  }

  getCart() {
    return this.cart;
  }
}
```

### View : Cart

```javascript
// views/CartView.js

export class CartView {
  constructor() {
    this.container = document.getElementById('cart-container');
    this.cartSidebar = document.getElementById('cart-sidebar');
  }

  render(cart) {
    if (!this.container) return;

    if (cart.items.length === 0) {
      this.container.innerHTML = this.renderEmptyCart();
      return;
    }

    this.container.innerHTML = `
      <div class="cart-content">
        <div class="cart-items">
          <h2>Panier (${cart.getItemCount()} articles)</h2>
          ${cart.items.map(item => this.renderCartItem(item)).join('')}
          
          <div class="cart-actions">
            <button class="btn btn-outline" id="clear-cart-btn">
              Vider le panier
            </button>
          </div>
        </div>

        <div class="cart-summary">
          ${this.renderSummary(cart)}
        </div>
      </div>
    `;

    this.attachListeners();
  }

  renderCartItem(item) {
    const { product, quantity } = item;
    const subtotal = product.price * quantity;

    return `
      <div class="cart-item" data-product-id="${product.id}">
        <div class="cart-item-image">
          <img src="${product.image}" alt="${product.name}">
        </div>

        <div class="cart-item-details">
          <h3>${product.name}</h3>
          <p class="cart-item-price">${product.getFormattedPrice()}</p>
          
          <div class="quantity-selector">
            <button class="quantity-btn" data-action="decrease">-</button>
            <input 
              type="number" 
              class="quantity-input" 
              value="${quantity}" 
              min="1" 
              max="${product.stock}"
            >
            <button class="quantity-btn" data-action="increase">+</button>
          </div>

          ${quantity >= product.stock ? 
            '<span class="stock-warning">Stock maximum atteint</span>' : 
            ''
          }
        </div>

        <div class="cart-item-total">
          <p class="subtotal">${this.formatPrice(subtotal)}</p>
          <button class="btn-icon btn-remove" title="Supprimer">
            <i class="icon-trash"></i>
          </button>
        </div>
      </div>
    `;
  }

  renderSummary(cart) {
    const subtotal = cart.getTotal();
    const shipping = subtotal > 100 ? 0 : 9.99;
    const total = subtotal + shipping;

    return `
      <div class="summary-card">
        <h3>Résumé</h3>
        
        <div class="summary-line">
          <span>Sous-total</span>
          <span>${cart.getFormattedTotal()}</span>
        </div>

        <div class="summary-line">
          <span>Livraison</span>
          <span>${shipping === 0 ? 'GRATUITE' : this.formatPrice(shipping)}</span>
        </div>

        ${shipping > 0 ? `
          <div class="free-shipping-notice">
            Plus que ${this.formatPrice(100 - subtotal)} pour la livraison gratuite !
          </div>
        ` : ''}

        <div class="summary-line summary-total">
          <span>Total</span>
          <span>${this.formatPrice(total)}</span>
        </div>

        <button class="btn btn-primary btn-block" id="checkout-btn">
          Passer la commande
        </button>

        <div class="trust-badges">
          <div class="badge">
            <i class="icon-lock"></i>
            <span>Paiement sécurisé</span>
          </div>
          <div class="badge">
            <i class="icon-truck"></i>
            <span>Livraison rapide</span>
          </div>
          <div class="badge">
            <i class="icon-return"></i>
            <span>Retour gratuit</span>
          </div>
        </div>
      </div>
    `;
  }

  renderEmptyCart() {
    return `
      <div class="empty-cart">
        <i class="icon-cart-empty"></i>
        <h3>Votre panier est vide</h3>
        <p>Découvrez nos produits et ajoutez-les à votre panier</p>
        <a href="#products" class="btn btn-primary">
          Voir les produits
        </a>
      </div>
    `;
  }

  attachListeners() {
    // Quantity changes
    const quantityInputs = this.container.querySelectorAll('.quantity-input');
    quantityInputs.forEach(input => {
      input.addEventListener('change', (e) => {
        const productId = parseInt(e.target.closest('.cart-item').dataset.productId);
        const quantity = parseInt(e.target.value);
        
        if (this.updateQuantityCallback) {
          this.updateQuantityCallback(productId, quantity);
        }
      });
    });

    // Quantity buttons
    const quantityButtons = this.container.querySelectorAll('.quantity-btn');
    quantityButtons.forEach(btn => {
      btn.addEventListener('click', (e) => {
        const action = e.currentTarget.dataset.action;
        const item = e.currentTarget.closest('.cart-item');
        const productId = parseInt(item.dataset.productId);
        const input = item.querySelector('.quantity-input');
        const currentValue = parseInt(input.value);

        let newValue = currentValue;
        if (action === 'increase' && currentValue < parseInt(input.max)) {
          newValue = currentValue + 1;
        } else if (action === 'decrease' && currentValue > 1) {
          newValue = currentValue - 1;
        }

        if (newValue !== currentValue) {
          input.value = newValue;
          if (this.updateQuantityCallback) {
            this.updateQuantityCallback(productId, newValue);
          }
        }
      });
    });

    // Remove buttons
    const removeButtons = this.container.querySelectorAll('.btn-remove');
    removeButtons.forEach(btn => {
      btn.addEventListener('click', (e) => {
        const productId = parseInt(e.currentTarget.closest('.cart-item').dataset.productId);
        
        if (this.removeItemCallback) {
          this.removeItemCallback(productId);
        }
      });
    });

    // Checkout button
    const checkoutBtn = this.container.querySelector('#checkout-btn');
    if (checkoutBtn) {
      checkoutBtn.addEventListener('click', () => {
        if (this.checkoutCallback) {
          this.checkoutCallback();
        }
      });
    }

    // Clear cart button
    const clearCartBtn = this.container.querySelector('#clear-cart-btn');
    if (clearCartBtn) {
      clearCartBtn.addEventListener('click', () => {
        if (this.clearCartCallback) {
          this.clearCartCallback();
        }
      });
    }
  }

  // Event handlers
  onUpdateQuantity(callback) {
    this.updateQuantityCallback = callback;
  }

  onRemoveItem(callback) {
    this.removeItemCallback = callback;
  }

  onCheckout(callback) {
    this.checkoutCallback = callback;
  }

  onClearCart(callback) {
    this.clearCartCallback = callback;
  }

  // Helpers
  formatPrice(price) {
    return new Intl.NumberFormat('fr-FR', {
      style: 'currency',
      currency: 'EUR'
    }).format(price);
  }

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
    setTimeout(() => toast.classList.add('show'), 10);
    setTimeout(() => {
      toast.classList.remove('show');
      setTimeout(() => toast.remove(), 300);
    }, 3000);
  }
}
```

### Model : Order

```javascript
// models/Order.js

export class Order {
  constructor({
    id,
    userId,
    items,
    subtotal,
    shipping,
    total,
    shippingAddress,
    billingAddress,
    paymentMethod,
    status = 'pending',
    createdAt
  }) {
    this.id = id;
    this.userId = userId;
    this.items = items;
    this.subtotal = subtotal;
    this.shipping = shipping;
    this.total = total;
    this.shippingAddress = shippingAddress;
    this.billingAddress = billingAddress;
    this.paymentMethod = paymentMethod;
    this.status = status; // pending, processing, shipped, delivered, cancelled
    this.createdAt = createdAt || new Date().toISOString();
  }

  // Status helpers
  isPending() {
    return this.status === 'pending';
  }

  isProcessing() {
    return this.status === 'processing';
  }

  isShipped() {
    return this.status === 'shipped';
  }

  isDelivered() {
    return this.status === 'delivered';
  }

  isCancelled() {
    return this.status === 'cancelled';
  }

  canBeCancelled() {
    return this.status === 'pending' || this.status === 'processing';
  }

  // Format date
  getFormattedDate() {
    return new Intl.DateTimeFormat('fr-FR', {
      year: 'numeric',
      month: 'long',
      day: 'numeric',
      hour: '2-digit',
      minute: '2-digit'
    }).format(new Date(this.createdAt));
  }

  // Format status
  getStatusLabel() {
    const labels = {
      pending: 'En attente',
      processing: 'En traitement',
      shipped: 'Expédiée',
      delivered: 'Livrée',
      cancelled: 'Annulée'
    };

    return labels[this.status] || this.status;
  }

  getStatusClass() {
    const classes = {
      pending: 'status-pending',
      processing: 'status-processing',
      shipped: 'status-shipped',
      delivered: 'status-delivered',
      cancelled: 'status-cancelled'
    };

    return classes[this.status] || '';
  }

  // Validation
  static validate(data) {
    const errors = [];

    if (!data.items || data.items.length === 0) {
      errors.push('La commande doit contenir au moins un article');
    }

    if (!data.shippingAddress || !data.shippingAddress.name) {
      errors.push('Adresse de livraison invalide');
    }

    if (!data.paymentMethod) {
      errors.push('Méthode de paiement requise');
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
      userId: this.userId,
      items: this.items,
      subtotal: this.subtotal,
      shipping: this.shipping,
      total: this.total,
      shippingAddress: this.shippingAddress,
      billingAddress: this.billingAddress,
      paymentMethod: this.paymentMethod,
      status: this.status,
      createdAt: this.createdAt
    };
  }

  static fromJSON(json) {
    return new Order(json);
  }
}
```

### Controller : Checkout

```javascript
// controllers/CheckoutController.js

import { Order } from '../models/Order.js';
import { CheckoutView } from '../views/CheckoutView.js';
import { storage } from '../services/StorageService.js';
import { authService } from '../services/AuthService.js';

export class CheckoutController {
  constructor(cartController) {
    this.cartController = cartController;
    this.view = new CheckoutView();
    this.currentStep = 1;
  }

  init() {
    if (!authService.isAuthenticated()) {
      window.location.hash = '#home';
      return;
    }

    this.setupEventListeners();
    this.render();
  }

  setupEventListeners() {
    // Navigation entre steps
    this.view.onNextStep((step) => {
      this.handleNextStep(step);
    });

    this.view.onPreviousStep((step) => {
      this.handlePreviousStep(step);
    });

    // Soumission finale
    this.view.onSubmitOrder((orderData) => {
      this.handleSubmitOrder(orderData);
    });
  }

  handleNextStep(currentStep) {
    const validation = this.validateStep(currentStep);

    if (!validation.isValid) {
      this.view.showErrors(validation.errors);
      return;
    }

    this.currentStep = currentStep + 1;
    this.render();
  }

  handlePreviousStep(currentStep) {
    this.currentStep = currentStep - 1;
    this.render();
  }

  validateStep(step) {
    const errors = [];

    switch (step) {
      case 1: // Shipping address
        const shippingForm = document.getElementById('shipping-form');
        if (!shippingForm) break;

        const name = shippingForm.querySelector('[name="name"]').value;
        const address = shippingForm.querySelector('[name="address"]').value;
        const city = shippingForm.querySelector('[name="city"]').value;
        const zipCode = shippingForm.querySelector('[name="zipCode"]').value;
        const phone = shippingForm.querySelector('[name="phone"]').value;

        if (!name || name.length < 2) {
          errors.push('Nom complet requis');
        }
        if (!address || address.length < 5) {
          errors.push('Adresse complète requise');
        }
        if (!city) {
          errors.push('Ville requise');
        }
        if (!zipCode || !/^\d{5}$/.test(zipCode)) {
          errors.push('Code postal invalide (5 chiffres)');
        }
        if (!phone || !/^[\d\s+()-]{10,}$/.test(phone)) {
          errors.push('Numéro de téléphone invalide');
        }
        break;

      case 2: // Payment
        const paymentMethod = document.querySelector('input[name="paymentMethod"]:checked');
        
        if (!paymentMethod) {
          errors.push('Veuillez sélectionner un mode de paiement');
        }

        if (paymentMethod && paymentMethod.value === 'card') {
          const cardNumber = document.querySelector('[name="cardNumber"]').value;
          const cardExpiry = document.querySelector('[name="cardExpiry"]').value;
          const cardCvv = document.querySelector('[name="cardCvv"]').value;

          if (!cardNumber || !/^\d{16}$/.test(cardNumber.replace(/\s/g, ''))) {
            errors.push('Numéro de carte invalide');
          }
          if (!cardExpiry || !/^\d{2}\/\d{2}$/.test(cardExpiry)) {
            errors.push('Date d\'expiration invalide (MM/YY)');
          }
          if (!cardCvv || !/^\d{3,4}$/.test(cardCvv)) {
            errors.push('CVV invalide');
          }
        }
        break;
    }

    return {
      isValid: errors.length === 0,
      errors
    };
  }

  async handleSubmitOrder(orderData) {
    try {
      this.view.showLoading(true);

      // Validation finale
      const validation = Order.validate(orderData);

      if (!validation.isValid) {
        this.view.showErrors(validation.errors);
        this.view.showLoading(false);
        return;
      }

      // Crée la commande
      const cart = this.cartController.getCart();
      const user = authService.getCurrentUser();

      const order = new Order({
        id: Date.now(),
        userId: user.id,
        items: cart.items.map(item => ({
          productId: item.product.id,
          name: item.product.name,
          price: item.product.price,
          quantity: item.quantity,
          image: item.product.image
        })),
        subtotal: cart.getTotal(),
        shipping: cart.getTotal() > 100 ? 0 : 9.99,
        total: cart.getTotal() + (cart.getTotal() > 100 ? 0 : 9.99),
        shippingAddress: orderData.shippingAddress,
        billingAddress: orderData.billingAddress || orderData.shippingAddress,
        paymentMethod: orderData.paymentMethod,
        status: 'pending'
      });

      // Sauvegarde la commande
      const orders = storage.load('orders') || [];
      orders.push(order.toJSON());
      storage.save('orders', orders);

      // Vide le panier
      this.cartController.clearCart();

      // Redirige vers la confirmation
      window.location.hash = `#order-confirmation/${order.id}`;

    } catch (error) {
      console.error('Error submitting order:', error);
      this.view.showError('Une erreur est survenue lors de la commande');
    } finally {
      this.view.showLoading(false);
    }
  }

  render() {
    const cart = this.cartController.getCart();
    this.view.render(this.currentStep, cart);
  }
}
```

### View : Checkout

```javascript
// views/CheckoutView.js

export class CheckoutView {
  constructor() {
    this.container = document.getElementById('checkout-container');
  }

  render(step, cart) {
    if (!this.container) return;

    this.container.innerHTML = `
      <div class="checkout-layout">
        <div class="checkout-steps">
          ${this.renderProgressBar(step)}
          ${this.renderStep(step, cart)}
        </div>

        <div class="checkout-summary">
          ${this.renderOrderSummary(cart)}
        </div>
      </div>
    `;

    this.attachListeners();
  }

  renderProgressBar(currentStep) {
    const steps = [
      { number: 1, title: 'Livraison' },
      { number: 2, title: 'Paiement' },
      { number: 3, title: 'Confirmation' }
    ];

    return `
      <div class="progress-bar">
        ${steps.map(step => `
          <div class="progress-step ${step.number === currentStep ? 'active' : ''} ${step.number < currentStep ? 'completed' : ''}">
            <div class="step-number">${step.number}</div>
            <div class="step-title">${step.title}</div>
          </div>
        `).join('<div class="progress-line"></div>')}
      </div>
    `;
  }

  renderStep(step, cart) {
    switch (step) {
      case 1:
        return this.renderShippingStep();
      case 2:
        return this.renderPaymentStep();
      case 3:
        return this.renderConfirmationStep(cart);
      default:
        return '';
    }
  }

  renderShippingStep() {
    return `
      <div class="checkout-step">
        <h2>Adresse de livraison</h2>

        <form id="shipping-form" class="checkout-form">
          <div class="form-group">
            <label for="name">Nom complet *</label>
            <input type="text" id="name" name="name" required>
          </div>

          <div class="form-group">
            <label for="address">Adresse *</label>
            <input type="text" id="address" name="address" required>
          </div>

          <div class="form-row">
            <div class="form-group">
              <label for="city">Ville *</label>
              <input type="text" id="city" name="city" required>
            </div>

            <div class="form-group">
              <label for="zipCode">Code postal *</label>
              <input type="text" id="zipCode" name="zipCode" pattern="\\d{5}" required>
            </div>
          </div>

          <div class="form-group">
            <label for="phone">Téléphone *</label>
            <input type="tel" id="phone" name="phone" required>
          </div>

          <div class="form-group">
            <label for="notes">Instructions de livraison (optionnel)</label>
            <textarea id="notes" name="notes" rows="3"></textarea>
          </div>

          <div class="form-actions">
            <button type="button" class="btn btn-primary" data-action="next">
              Continuer vers le paiement
            </button>
          </div>
        </form>
      </div>
    `;
  }

  renderPaymentStep() {
    return `
      <div class="checkout-step">
        <h2>Mode de paiement</h2>

        <form id="payment-form" class="checkout-form">
          <div class="payment-methods">
            <label class="payment-method">
              <input type="radio" name="paymentMethod" value="card" checked>
              <div class="payment-method-content">
                <i class="icon-credit-card"></i>
                <span>Carte bancaire</span>
              </div>
            </label>

            <label class="payment-method">
              <input type="radio" name="paymentMethod" value="paypal">
              <div class="payment-method-content">
                <i class="icon-paypal"></i>
                <span>PayPal</span>
              </div>
            </label>

            <label class="payment-method">
              <input type="radio" name="paymentMethod" value="transfer">
              <div class="payment-method-content">
                <i class="icon-bank"></i>
                <span>Virement bancaire</span>
              </div>
            </label>
          </div>

          <div id="card-details" class="card-details">
            <div class="form-group">
              <label for="cardNumber">Numéro de carte *</label>
              <input type="text" id="cardNumber" name="cardNumber" placeholder="1234 5678 9012 3456" maxlength="19">
            </div>

            <div class="form-row">
              <div class="form-group">
                <label for="cardExpiry">Date d'expiration *</label>
                <input type="text" id="cardExpiry" name="cardExpiry" placeholder="MM/YY" maxlength="5">
              </div>

              <div class="form-group">
                <label for="cardCvv">CVV *</label>
                <input type="text" id="cardCvv" name="cardCvv" placeholder="123" maxlength="4">
              </div>
            </div>
          </div>

          <div class="form-actions">
            <button type="button" class="btn btn-outline" data-action="previous">
              Retour
            </button>
            <button type="button" class="btn btn-primary" data-action="next">
              Continuer
            </button>
          </div>
        </form>
      </div>
    `;
  }

  renderConfirmationStep(cart) {
    return `
      <div class="checkout-step">
        <h2>Confirmation de la commande</h2>

        <div class="confirmation-section">
          <h3>Récapitulatif</h3>
          <div class="confirmation-items">
            ${cart.items.map(item => `
              <div class="confirmation-item">
                <img src="${item.product.image}" alt="${item.product.name}">
                <div class="item-details">
                  <p class="item-name">${item.product.name}</p>
                  <p class="item-quantity">Quantité: ${item.quantity}</p>
                </div>
                <p class="item-price">${this.formatPrice(item.product.price * item.quantity)}</p>
              </div>
            `).join('')}
          </div>
        </div>

        <div class="form-actions">
          <button type="button" class="btn btn-outline" data-action="previous">
            Retour
          </button>
          <button type="button" class="btn btn-primary btn-lg" data-action="submit">
            <i class="icon-lock"></i>
            Confirmer et payer
          </button>
        </div>
      </div>
    `;
  }

  renderOrderSummary(cart) {
    const subtotal = cart.getTotal();
    const shipping = subtotal > 100 ? 0 : 9.99;
    const total = subtotal + shipping;

    return `
      <div class="summary-card">
        <h3>Récapitulatif</h3>
        
        <div class="summary-items">
          ${cart.items.map(item => `
            <div class="summary-item">
              <span>${item.product.name} x${item.quantity}</span>
              <span>${this.formatPrice(item.product.price * item.quantity)}</span>
            </div>
          `).join('')}
        </div>

        <hr>

        <div class="summary-line">
          <span>Sous-total</span>
          <span>${this.formatPrice(subtotal)}</span>
        </div>

        <div class="summary-line">
          <span>Livraison</span>
          <span>${shipping === 0 ? 'GRATUITE' : this.formatPrice(shipping)}</span>
        </div>

        <hr>

        <div class="summary-line summary-total">
          <span>Total</span>
          <span class="total-amount">${this.formatPrice(total)}</span>
        </div>

        <div class="trust-badges">
          <div class="badge">
            <i class="icon-lock"></i>
            <span>Paiement 100% sécurisé</span>
          </div>
          <div class="badge">
            <i class="icon-shield"></i>
            <span>Données protégées</span>
          </div>
        </div>
      </div>
    `;
  }

  attachListeners() {
    // Navigation buttons
    const nextButtons = this.container.querySelectorAll('[data-action="next"]');
    nextButtons.forEach(btn => {
      btn.addEventListener('click', () => {
        const currentStep = this.getCurrentStep();
        if (this.nextStepCallback) {
          this.nextStepCallback(currentStep);
        }
      });
    });

    const previousButtons = this.container.querySelectorAll('[data-action="previous"]');
    previousButtons.forEach(btn => {
      btn.addEventListener('click', () => {
        const currentStep = this.getCurrentStep();
        if (this.previousStepCallback) {
          this.previousStepCallback(currentStep);
        }
      });
    });

    // Submit button
    const submitButton = this.container.querySelector('[data-action="submit"]');
    if (submitButton) {
      submitButton.addEventListener('click', () => {
        if (this.submitOrderCallback) {
          const orderData = this.collectOrderData();
          this.submitOrderCallback(orderData);
        }
      });
    }

    // Payment method toggle
    const paymentMethods = this.container.querySelectorAll('input[name="paymentMethod"]');
    paymentMethods.forEach(input => {
      input.addEventListener('change', (e) => {
        const cardDetails = this.container.querySelector('#card-details');
        if (cardDetails) {
          cardDetails.style.display = e.target.value === 'card' ? 'block' : 'none';
        }
      });
    });

    // Card number formatting
    const cardNumberInput = this.container.querySelector('[name="cardNumber"]');
    if (cardNumberInput) {
      cardNumberInput.addEventListener('input', (e) => {
        let value = e.target.value.replace(/\s/g, '');
        let formattedValue = value.match(/.{1,4}/g)?.join(' ') || value;
        e.target.value = formattedValue;
      });
    }

    // Card expiry formatting
    const cardExpiryInput = this.container.querySelector('[name="cardExpiry"]');
    if (cardExpiryInput) {
      cardExpiryInput.addEventListener('input', (e) => {
        let value = e.target.value.replace(/\D/g, '');
        if (value.length >= 2) {
          value = value.slice(0, 2) + '/' + value.slice(2, 4);
        }
        e.target.value = value;
      });
    }
  }

  getCurrentStep() {
    const progressSteps = this.container.querySelectorAll('.progress-step');
    for (let i = 0; i < progressSteps.length; i++) {
      if (progressSteps[i].classList.contains('active')) {
        return i + 1;
      }
    }
    return 1;
  }

  collectOrderData() {
    const shippingForm = document.getElementById('shipping-form');
    const paymentForm = document.getElementById('payment-form');

    const data = {
      shippingAddress: {},
      paymentMethod: null
    };

    if (shippingForm) {
      const formData = new FormData(shippingForm);
      data.shippingAddress = {
        name: formData.get('name'),
        address: formData.get('address'),
        city: formData.get('city'),
        zipCode: formData.get('zipCode'),
        phone: formData.get('phone'),
        notes: formData.get('notes')
      };
    }

    if (paymentForm) {
      const paymentMethod = paymentForm.querySelector('input[name="paymentMethod"]:checked');
      data.paymentMethod = paymentMethod ? paymentMethod.value : null;
    }

    return data;
  }

  // Event handlers
  onNextStep(callback) {
    this.nextStepCallback = callback;
  }

  onPreviousStep(callback) {
    this.previousStepCallback = callback;
  }

  onSubmitOrder(callback) {
    this.submitOrderCallback = callback;
  }

  // Helpers
  formatPrice(price) {
    return new Intl.NumberFormat('fr-FR', {
      style: 'currency',
      currency: 'EUR'
    }).format(price);
  }

  showLoading(show) {
    const submitButton = this.container.querySelector('[data-action="submit"]');
    if (submitButton) {
      submitButton.disabled = show;
      submitButton.innerHTML = show ? 
        '<span class="spinner"></span> Traitement...' : 
        '<i class="icon-lock"></i> Confirmer et payer';
    }
  }

  showErrors(errors) {
    const errorContainer = document.createElement('div');
    errorContainer.className = 'checkout-errors';
    errorContainer.innerHTML = errors.map(error => 
      `<div class="error-message"><i class="icon-alert"></i> ${error}</div>`
    ).join('');

    const checkoutStep = this.container.querySelector('.checkout-step');
    if (checkoutStep) {
      // Remove existing errors
      const existingErrors = checkoutStep.querySelector('.checkout-errors');
      if (existingErrors) {
        existingErrors.remove();
      }

      checkoutStep.insertBefore(errorContainer, checkoutStep.firstChild);

      // Scroll to errors
      errorContainer.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
    }
  }

  showError(message) {
    this.showErrors([message]);
  }
}
```

---

## 6. E-commerce : Admin Dashboard

### Controller : Admin

```javascript
// controllers/AdminController.js

import { AdminView } from '../views/AdminView.js';
import { storage } from '../services/StorageService.js';
import { authService } from '../services/AuthService.js';
import { Product } from '../models/Product.js';
import { Order } from '../models/Order.js';

export class AdminController {
  constructor() {
    this.view = new AdminView();
    this.currentTab = 'overview';
  }

  init() {
    // Vérifie si l'utilisateur est admin
    if (!authService.isAdmin()) {
      window.location.hash = '#home';
      return;
    }

    this.setupEventListeners();
    this.render();
  }

  setupEventListeners() {
    // Tab switching
    this.view.onTabChange((tab) => {
      this.currentTab = tab;
      this.render();
    });

    // Product management
    this.view.onAddProduct((productData) => {
      this.handleAddProduct(productData);
    });

    this.view.onEditProduct((id, productData) => {
      this.handleEditProduct(id, productData);
    });

    this.view.onDeleteProduct((id) => {
      this.handleDeleteProduct(id);
    });

    // Order management
    this.view.onUpdateOrderStatus((id, status) => {
      this.handleUpdateOrderStatus(id, status);
    });
  }

  async handleAddProduct(productData) {
    try {
      const validation = Product.validate(productData);

      if (!validation.isValid) {
        this.view.showErrors(validation.errors);
        return;
      }

      const products = storage.load('products') || [];
      const newProduct = new Product({
        ...productData,
        id: Date.now()
      });

      products.push(newProduct.toJSON());
      storage.save('products', products);

      this.view.showSuccess('Produit ajouté avec succès');
      this.render();

      // Dispatch event pour rafraîchir le catalogue
      window.dispatchEvent(new CustomEvent('productsUpdated'));

    } catch (error) {
      console.error('Error adding product:', error);
      this.view.showError('Erreur lors de l\'ajout du produit');
    }
  }

  async handleEditProduct(id, productData) {
    try {
      const validation = Product.validate(productData);

      if (!validation.isValid) {
        this.view.showErrors(validation.errors);
        return;
      }

      const products = storage.load('products') || [];
      const index = products.findIndex(p => p.id === id);

      if (index === -1) {
        this.view.showError('Produit introuvable');
        return;
      }

      products[index] = { ...productData, id };
      storage.save('products', products);

      this.view.showSuccess('Produit modifié avec succès');
      this.render();

      window.dispatchEvent(new CustomEvent('productsUpdated'));

    } catch (error) {
      console.error('Error editing product:', error);
      this.view.showError('Erreur lors de la modification du produit');
    }
  }

  async handleDeleteProduct(id) {
    if (!confirm('Êtes-vous sûr de vouloir supprimer ce produit ?')) {
      return;
    }

    try {
      const products = storage.load('products') || [];
      const filtered = products.filter(p => p.id !== id);

      storage.save('products', filtered);

      this.view.showSuccess('Produit supprimé avec succès');
      this.render();

      window.dispatchEvent(new CustomEvent('productsUpdated'));

    } catch (error) {
      console.error('Error deleting product:', error);
      this.view.showError('Erreur lors de la suppression du produit');
    }
  }

  async handleUpdateOrderStatus(id, status) {
    try {
      const orders = storage.load('orders') || [];
      const index = orders.findIndex(o => o.id === id);

      if (index === -1) {
        this.view.showError('Commande introuvable');
        return;
      }

      orders[index].status = status;
      storage.save('orders', orders);

      this.view.showSuccess('Statut mis à jour');
      this.render();

    } catch (error) {
      console.error('Error updating order:', error);
      this.view.showError('Erreur lors de la mise à jour');
    }
  }

  render() {
    const data = this.getData();
    this.view.render(this.currentTab, data);
  }

  getData() {
    switch (this.currentTab) {
      case 'overview':
        return this.getOverviewData();
      case 'products':
        return this.getProductsData();
      case 'orders':
        return this.getOrdersData();
      case 'users':
        return this.getUsersData();
      default:
        return {};
    }
  }

  getOverviewData() {
    const products = storage.load('products') || [];
    const orders = storage.load('orders') || [];
    const users = storage.load('users') || [];

    const totalRevenue = orders.reduce((sum, order) => {
      return order.status !== 'cancelled' ? sum + order.total : sum;
    }, 0);

    const pendingOrders = orders.filter(o => o.status === 'pending').length;
    const lowStockProducts = products.filter(p => p.stock < 10).length;

    return {
      stats: {
        totalProducts: products.length,
        totalOrders: orders.length,
        totalUsers: users.length,
        totalRevenue,
        pendingOrders,
        lowStockProducts
      },
      recentOrders: orders.slice(-5).reverse(),
      lowStockProducts: products.filter(p => p.stock < 10)
    };
  }

  getProductsData() {
    return {
      products: storage.load('products') || []
    };
  }

  getOrdersData() {
    return {
      orders: (storage.load('orders') || []).reverse()
    };
  }

  getUsersData() {
    return {
      users: storage.load('users') || []
    };
  }
}
```

### View : Admin Dashboard

```javascript
// views/AdminView.js

export class AdminView {
  constructor() {
    this.container = document.getElementById('admin-container');
  }

  render(tab, data) {
    if (!this.container) return;

    this.container.innerHTML = `
      <div class="admin-layout">
        <aside class="admin-sidebar">
          ${this.renderSidebar(tab)}
        </aside>

        <main class="admin-content">
          ${this.renderContent(tab, data)}
        </main>
      </div>
    `;

    this.attachListeners();
  }

  renderSidebar(activeTab) {
    const tabs = [
      { id: 'overview', icon: 'icon-dashboard', label: 'Vue d\'ensemble' },
      { id: 'products', icon: 'icon-package', label: 'Produits' },
      { id: 'orders', icon: 'icon-shopping-bag', label: 'Commandes' },
      { id: 'users', icon: 'icon-users', label: 'Utilisateurs' }
    ];

    return `
      <div class="admin-sidebar-header">
        <h2>Administration</h2>
      </div>

      <nav class="admin-nav">
        ${tabs.map(tab => `
          <a href="#" 
             class="admin-nav-item ${tab.id === activeTab ? 'active' : ''}" 
             data-tab="${tab.id}">
            <i class="${tab.icon}"></i>
            <span>${tab.label}</span>
          </a>
        `).join('')}
      </nav>
    `;
  }

  renderContent(tab, data) {
    switch (tab) {
      case 'overview':
        return this.renderOverview(data);
      case 'products':
        return this.renderProducts(data.products);
      case 'orders':
        return this.renderOrders(data.orders);
      case 'users':
        return this.renderUsers(data.users);
      default:
        return '';
    }
  }

  renderOverview(data) {
    const { stats, recentOrders, lowStockProducts } = data;

    return `
      <div class="admin-overview">
        <h1>Vue d'ensemble</h1>

        <div class="stats-grid">
          <div class="stat-card">
            <div class="stat-icon stat-primary">
              <i class="icon-package"></i>
            </div>
            <div class="stat-content">
              <p class="stat-label">Produits</p>
              <p class="stat-value">${stats.totalProducts}</p>
            </div>
          </div>

          <div class="stat-card">
            <div class="stat-icon stat-success">
              <i class="icon-shopping-bag"></i>
            </div>
            <div class="stat-content">
              <p class="stat-label">Commandes</p>
              <p class="stat-value">${stats.totalOrders}</p>
            </div>
          </div>

          <div class="stat-card">
            <div class="stat-icon stat-info">
              <i class="icon-users"></i>
            </div>
            <div class="stat-content">
              <p class="stat-label">Utilisateurs</p>
              <p class="stat-value">${stats.totalUsers}</p>
            </div>
          </div>

          <div class="stat-card">
            <div class="stat-icon stat-warning">
              <i class="icon-euro"></i>
            </div>
            <div class="stat-content">
              <p class="stat-label">Chiffre d'affaires</p>
              <p class="stat-value">${this.formatPrice(stats.totalRevenue)}</p>
            </div>
          </div>
        </div>

        ${stats.pendingOrders > 0 || stats.lowStockProducts > 0 ? `
          <div class="alerts">
            ${stats.pendingOrders > 0 ? `
              <div class="alert alert-warning">
                <i class="icon-alert"></i>
                ${stats.pendingOrders} commande(s) en attente de traitement
              </div>
            ` : ''}
            
            ${stats.lowStockProducts > 0 ? `
              <div class="alert alert-danger">
                <i class="icon-alert"></i>
                ${stats.lowStockProducts} produit(s) en stock faible
              </div>
            ` : ''}
          </div>
        ` : ''}

        <div class="admin-row">
          <div class="admin-card">
            <h3>Commandes récentes</h3>
            ${recentOrders.length > 0 ? `
              <div class="recent-orders">
                ${recentOrders.map(order => `
                  <div class="recent-order">
                    <div class="order-info">
                      <p class="order-id">#${order.id}</p>
                      <p class="order-customer">${order.shippingAddress.name}</p>
                    </div>
                    <div class="order-status">
                      <span class="badge ${order.status}">${this.getStatusLabel(order.status)}</span>
                    </div>
                    <div class="order-total">
                      ${this.formatPrice(order.total)}
                    </div>
                  </div>
                `).join('')}
              </div>
            ` : '<p class="empty-state">Aucune commande récente</p>'}
          </div>

          <div class="admin-card">
            <h3>Stock faible</h3>
            ${lowStockProducts.length > 0 ? `
              <div class="low-stock-products">
                ${lowStockProducts.map(product => `
                  <div class="low-stock-product">
                    <img src="${product.image}" alt="${product.name}">
                    <div class="product-info">
                      <p class="product-name">${product.name}</p>
                      <p class="product-stock ${product.stock === 0 ? 'out-of-stock' : ''}">
                        ${product.stock === 0 ? 'Rupture de stock' : `${product.stock} restant(s)`}
                      </p>
                    </div>
                  </div>
                `).join('')}
              </div>
            ` : '<p class="empty-state">Tous les produits sont en stock suffisant</p>'}
          </div>
        </div>
      </div>
    `;
  }

  renderProducts(products) {
    return `
      <div class="admin-products">
        <div class="admin-header">
          <h1>Gestion des produits</h1>
          <button class="btn btn-primary" id="add-product-btn">
            <i class="icon-plus"></i>
            Ajouter un produit
          </button>
        </div>

        <div class="admin-table-container">
          <table class="admin-table">
            <thead>
              <tr>
                <th>Image</th>
                <th>Nom</th>
                <th>Catégorie</th>
                <th>Prix</th>
                <th>Stock</th>
                <th>Note</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              ${products.map(product => `
                <tr data-product-id="${product.id}">
                  <td>
                    <img src="${product.image}" alt="${product.name}" class="product-thumb">
                  </td>
                  <td>${product.name}</td>
                  <td>${this.formatCategory(product.category)}</td>
                  <td>${this.formatPrice(product.price)}</td>
                  <td>
                    <span class="stock-badge ${product.stock < 10 ? 'stock-low' : ''}">
                      ${product.stock}
                    </span>
                  </td>
                  <td>${product.rating}/5</td>
                  <td>
                    <button class="btn-icon" data-action="edit" title="Modifier">
                      <i class="icon-edit"></i>
                    </button>
                    <button class="btn-icon btn-danger" data-action="delete" title="Supprimer">
                      <i class="icon-trash"></i>
                    </button>
                  </td>
                </tr>
              `).join('')}
            </tbody>
          </table>
        </div>
      </div>

      ${this.renderProductModal()}
    `;
  }

  renderProductModal() {
    return `
      <div id="product-modal" class="modal">
        <div class="modal-content">
          <div class="modal-header">
            <h3 id="modal-title">Ajouter un produit</h3>
            <button class="modal-close">&times;</button>
          </div>

          <form id="product-form" class="modal-body">
            <input type="hidden" name="id">

            <div class="form-group">
              <label for="product-name">Nom *</label>
              <input type="text" id="product-name" name="name" required>
            </div>

            <div class="form-group">
              <label for="product-description">Description *</label>
              <textarea id="product-description" name="description" rows="3" required></textarea>
            </div>

            <div class="form-row">
              <div class="form-group">
                <label for="product-price">Prix (€) *</label>
                <input type="number" id="product-price" name="price" step="0.01" min="0" required>
              </div>

              <div class="form-group">
                <label for="product-stock">Stock *</label>
                <input type="number" id="product-stock" name="stock" min="0" required>
              </div>
            </div>

            <div class="form-row">
              <div class="form-group">
                <label for="product-category">Catégorie *</label>
                <select id="product-category" name="category" required>
                  <option value="">Sélectionner</option>
                  <option value="laptops">Ordinateurs</option>
                  <option value="phones">Smartphones</option>
                  <option value="tablets">Tablettes</option>
                  <option value="audio">Audio</option>
                  <option value="wearables">Montres & Accessoires</option>
                </select>
              </div>

              <div class="form-group">
                <label for="product-rating">Note *</label>
                <input type="number" id="product-rating" name="rating" step="0.1" min="0" max="5" required>
              </div>
            </div>

            <div class="form-group">
              <label for="product-image">URL de l'image *</label>
              <input type="url" id="product-image" name="image" required>
            </div>

            <div class="modal-footer">
              <button type="button" class="btn btn-outline modal-close">Annuler</button>
              <button type="submit" class="btn btn-primary">Enregistrer</button>
            </div>
          </form>
        </div>
      </div>
    `;
  }

  renderOrders(orders) {
    return `
      <div class="admin-orders">
        <h1>Gestion des commandes</h1>

        <div class="admin-table-container">
          <table class="admin-table">
            <thead>
              <tr>
                <th>N° Commande</th>
                <th>Client</th>
                <th>Date</th>
                <th>Articles</th>
                <th>Total</th>
                <th>Statut</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              ${orders.map(order => `
                <tr data-order-id="${order.id}">
                  <td>#${order.id}</td>
                  <td>${order.shippingAddress.name}</td>
                  <td>${new Date(order.createdAt).toLocaleDateString('fr-FR')}</td>
                  <td>${order.items.length} article(s)</td>
                  <td>${this.formatPrice(order.total)}</td>
                  <td>
                    <select class="status-select" data-order-id="${order.id}">
                      <option value="pending" ${order.status === 'pending' ? 'selected' : ''}>En attente</option>
                      <option value="processing" ${order.status === 'processing' ? 'selected' : ''}>En traitement</option>
                      <option value="shipped" ${order.status === 'shipped' ? 'selected' : ''}>Expédiée</option>
                      <option value="delivered" ${order.status === 'delivered' ? 'selected' : ''}>Livrée</option>
                      <option value="cancelled" ${order.status === 'cancelled' ? 'selected' : ''}>Annulée</option>
                    </select>
                  </td>
                  <td>
                    <button class="btn-icon" data-action="view" title="Voir détails">
                      <i class="icon-eye"></i>
                    </button>
                  </td>
                </tr>
              `).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;
  }

  renderUsers(users) {
    return `
      <div class="admin-users">
        <h1>Utilisateurs</h1>

        <div class="admin-table-container">
          <table class="admin-table">
            <thead>
              <tr>
                <th>Nom</th>
                <th>Email</th>
                <th>Rôle</th>
                <th>Date d'inscription</th>
              </tr>
            </thead>
            <tbody>
              ${users.map(user => `
                <tr>
                  <td>${user.name}</td>
                  <td>${user.email}</td>
                  <td>
                    <span class="badge ${user.role === 'admin' ? 'badge-primary' : 'badge-secondary'}">
                      ${user.role === 'admin' ? 'Administrateur' : 'Client'}
                    </span>
                  </td>
                  <td>${new Date(user.createdAt).toLocaleDateString('fr-FR')}</td>
                </tr>
              `).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;
  }

  attachListeners() {
    // Tab switching
    const tabButtons = this.container.querySelectorAll('[data-tab]');
    tabButtons.forEach(btn => {
      btn.addEventListener('click', (e) => {
        e.preventDefault();
        const tab = e.currentTarget.dataset.tab;
        if (this.tabChangeCallback) {
          this.tabChangeCallback(tab);
        }
      });
    });

    // Add product button
    const addProductBtn = this.container.querySelector('#add-product-btn');
    if (addProductBtn) {
      addProductBtn.addEventListener('click', () => {
        this.openProductModal();
      });
    }

    // Product actions
    const productActions = this.container.querySelectorAll('[data-product-id] [data-action]');
    productActions.forEach(btn => {
      btn.addEventListener('click', (e) => {
        const row = e.currentTarget.closest('[data-product-id]');
        const productId = parseInt(row.dataset.productId);
        const action = e.currentTarget.dataset.action;

        if (action === 'edit') {
          this.openProductModal(productId);
        } else if (action === 'delete') {
          if (this.deleteProductCallback) {
            this.deleteProductCallback(productId);
          }
        }
      });
    });

    // Order status change
    const statusSelects = this.container.querySelectorAll('.status-select');
    statusSelects.forEach(select => {
      select.addEventListener('change', (e) => {
        const orderId = parseInt(e.target.dataset.orderId);
        const status = e.target.value;

        if (this.updateOrderStatusCallback) {
          this.updateOrderStatusCallback(orderId, status);
        }
      });
    });

    // Product form
    const productForm = this.container.querySelector('#product-form');
    if (productForm) {
      productForm.addEventListener('submit', (e) => {
        e.preventDefault();

        const formData = new FormData(productForm);
        const productData = {
          name: formData.get('name'),
          description: formData.get('description'),
          price: parseFloat(formData.get('price')),
          stock: parseInt(formData.get('stock')),
          category: formData.get('category'),
          rating: parseFloat(formData.get('rating')),
          image: formData.get('image')
        };

        const productId = formData.get('id');

        if (productId) {
          // Edit
          if (this.editProductCallback) {
            this.editProductCallback(parseInt(productId), productData);
          }
        } else {
          // Add
          if (this.addProductCallback) {
            this.addProductCallback(productData);
          }
        }

        this.closeProductModal();
      });
    }

    // Modal close
    const modalCloseButtons = this.container.querySelectorAll('.modal-close');
    modalCloseButtons.forEach(btn => {
      btn.addEventListener('click', () => {
        this.closeProductModal();
      });
    });
  }

  openProductModal(productId = null) {
    const modal = this.container.querySelector('#product-modal');
    const form = this.container.querySelector('#product-form');
    const title = this.container.querySelector('#modal-title');

    if (!modal || !form) return;

    if (productId) {
      // Edit mode
      title.textContent = 'Modifier le produit';
      
      // Load product data
      const products = storage.load('products') || [];
      const product = products.find(p => p.id === productId);

      if (product) {
        form.querySelector('[name="id"]').value = product.id;
        form.querySelector('[name="name"]').value = product.name;
        form.querySelector('[name="description"]').value = product.description;
        form.querySelector('[name="price"]').value = product.price;
        form.querySelector('[name="stock"]').value = product.stock;
        form.querySelector('[name="category"]').value = product.category;
        form.querySelector('[name="rating"]').value = product.rating;
        form.querySelector('[name="image"]').value = product.image;
      }
    } else {
      // Add mode
      title.textContent = 'Ajouter un produit';
      form.reset();
    }

    modal.classList.add('show');
  }

  closeProductModal() {
    const modal = this.container.querySelector('#product-modal');
    if (modal) {
      modal.classList.remove('show');
    }
  }

  // Event handlers
  onTabChange(callback) {
    this.tabChangeCallback = callback;
  }

  onAddProduct(callback) {
    this.addProductCallback = callback;
  }

  onEditProduct(callback) {
    this.editProductCallback = callback;
  }

  onDeleteProduct(callback) {
    this.deleteProductCallback = callback;
  }

  onUpdateOrderStatus(callback) {
    this.updateOrderStatusCallback = callback;
  }

  // Helpers
  formatPrice(price) {
    return new Intl.NumberFormat('fr-FR', {
      style: 'currency',
      currency: 'EUR'
    }).format(price);
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

  getStatusLabel(status) {
    const labels = {
      pending: 'En attente',
      processing: 'En traitement',
      shipped: 'Expédiée',
      delivered: 'Livrée',
      cancelled: 'Annulée'
    };
    return labels[status] || status;
  }

  showSuccess(message) {
    this.showToast(message, 'success');
  }

  showError(message) {
    this.showToast(message, 'error');
  }

  showErrors(errors) {
    errors.forEach(error => this.showError(error));
  }

  showToast(message, type = 'info') {
    const toast = document.createElement('div');
    toast.className = `toast toast-${type}`;
    toast.textContent = message;

    document.body.appendChild(toast);
    setTimeout(() => toast.classList.add('show'), 10);
    setTimeout(() => {
      toast.classList.remove('show');
      setTimeout(() => toast.remove(), 300);
    }, 3000);
  }
}
```

---

## 7. E-commerce : Tests Complets

### Configuration Vitest

```javascript
// tests/setup.js

import { beforeEach } from 'vitest';

// Mock localStorage
const localStorageMock = {
  getItem: vi.fn(),
  setItem: vi.fn(),
  removeItem: vi.fn(),
  clear: vi.fn(),
};

global.localStorage = localStorageMock;

// Reset mocks avant chaque test
beforeEach(() => {
  localStorage.getItem.mockClear();
  localStorage.setItem.mockClear();
  localStorage.removeItem.mockClear();
  localStorage.clear.mockClear();
});
```

### Tests : Product Model

```javascript
// tests/models/Product.test.js

import { describe, it, expect } from 'vitest';
import { Product } from '../../src/js/models/Product.js';

describe('Product Model', () => {
  describe('Constructor', () => {
    it('should create a product with all properties', () => {
      const productData = {
        id: 1,
        name: 'Test Product',
        description: 'A test product',
        price: 99.99,
        image: '/test.jpg',
        category: 'test',
        stock: 10,
        rating: 4.5
      };

      const product = new Product(productData);

      expect(product.id).toBe(1);
      expect(product.name).toBe('Test Product');
      expect(product.price).toBe(99.99);
      expect(product.stock).toBe(10);
    });

    it('should set default rating to 0 if not provided', () => {
      const product = new Product({
        id: 1,
        name: 'Test',
        price: 10,
        stock: 5
      });

      expect(product.rating).toBe(0);
    });
  });

  describe('isInStock', () => {
    it('should return true when stock > 0', () => {
      const product = new Product({ stock: 5 });
      expect(product.isInStock()).toBe(true);
    });

    it('should return false when stock is 0', () => {
      const product = new Product({ stock: 0 });
      expect(product.isInStock()).toBe(false);
    });
  });

  describe('getFormattedPrice', () => {
    it('should format price as EUR currency', () => {
      const product = new Product({ price: 99.99 });
      const formatted = product.getFormattedPrice();

      expect(formatted).toContain('99,99');
      expect(formatted).toContain('€');
    });
  });

  describe('getDiscountedPrice', () => {
    it('should calculate discounted price correctly', () => {
      const product = new Product({ price: 100 });
      
      expect(product.getDiscountedPrice(10)).toBe(90);
      expect(product.getDiscountedPrice(25)).toBe(75);
      expect(product.getDiscountedPrice(50)).toBe(50);
    });
  });

  describe('validate', () => {
    it('should validate a correct product', () => {
      const data = {
        name: 'Valid Product',
        price: 50,
        stock: 10
      };

      const result = Product.validate(data);

      expect(result.isValid).toBe(true);
      expect(result.errors).toHaveLength(0);
    });

    it('should fail if name is too short', () => {
      const data = {
        name: 'AB',
        price: 50,
        stock: 10
      };

      const result = Product.validate(data);

      expect(result.isValid).toBe(false);
      expect(result.errors).toContain('Le nom doit contenir au moins 3 caractères');
    });

    it('should fail if price is invalid', () => {
      const data = {
        name: 'Valid Name',
        price: 0,
        stock: 10
      };

      const result = Product.validate(data);

      expect(result.isValid).toBe(false);
      expect(result.errors).toContain('Le prix doit être supérieur à 0');
    });

    it('should fail if stock is negative', () => {
      const data = {
        name: 'Valid Name',
        price: 50,
        stock: -5
      };

      const result = Product.validate(data);

      expect(result.isValid).toBe(false);
      expect(result.errors).toContain('Le stock ne peut pas être négatif');
    });
  });

  describe('toJSON and fromJSON', () => {
    it('should serialize and deserialize correctly', () => {
      const original = new Product({
        id: 1,
        name: 'Test',
        price: 50,
        stock: 10
      });

      const json = original.toJSON();
      const deserialized = Product.fromJSON(json);

      expect(deserialized.id).toBe(original.id);
      expect(deserialized.name).toBe(original.name);
      expect(deserialized.price).toBe(original.price);
    });
  });
});
```

### Tests : Cart Model

```javascript
// tests/models/Cart.test.js

import { describe, it, expect, beforeEach } from 'vitest';
import { Cart } from '../../src/js/models/Cart.js';
import { Product } from '../../src/js/models/Product.js';

describe('Cart Model', () => {
  let cart;
  let product1;
  let product2;

  beforeEach(() => {
    cart = new Cart();
    
    product1 = new Product({
      id: 1,
      name: 'Product 1',
      price: 10,
      stock: 5
    });

    product2 = new Product({
      id: 2,
      name: 'Product 2',
      price: 20,
      stock: 3
    });
  });

  describe('addItem', () => {
    it('should add a new item to the cart', () => {
      cart.addItem(product1, 2);

      expect(cart.items).toHaveLength(1);
      expect(cart.items[0].product.id).toBe(1);
      expect(cart.items[0].quantity).toBe(2);
    });

    it('should increase quantity if product already exists', () => {
      cart.addItem(product1, 1);
      cart.addItem(product1, 2);

      expect(cart.items).toHaveLength(1);
      expect(cart.items[0].quantity).toBe(3);
    });

    it('should default to quantity 1 if not specified', () => {
      cart.addItem(product1);

      expect(cart.items[0].quantity).toBe(1);
    });
  });

  describe('removeItem', () => {
    it('should remove an item from the cart', () => {
      cart.addItem(product1, 1);
      cart.addItem(product2, 1);

      expect(cart.items).toHaveLength(2);

      cart.removeItem(1);

      expect(cart.items).toHaveLength(1);
      expect(cart.items[0].product.id).toBe(2);
    });
  });

  describe('updateQuantity', () => {
    it('should update item quantity', () => {
      cart.addItem(product1, 1);
      cart.updateQuantity(1, 5);

      expect(cart.items[0].quantity).toBe(5);
    });

    it('should remove item if quantity is 0', () => {
      cart.addItem(product1, 1);
      cart.updateQuantity(1, 0);

      expect(cart.items).toHaveLength(0);
    });

    it('should remove item if quantity is negative', () => {
      cart.addItem(product1, 1);
      cart.updateQuantity(1, -1);

      expect(cart.items).toHaveLength(0);
    });
  });

  describe('clear', () => {
    it('should remove all items', () => {
      cart.addItem(product1, 1);
      cart.addItem(product2, 1);

      expect(cart.items).toHaveLength(2);

      cart.clear();

      expect(cart.items).toHaveLength(0);
    });
  });

  describe('getTotal', () => {
    it('should calculate correct total', () => {
      cart.addItem(product1, 2); // 2 * 10 = 20
      cart.addItem(product2, 1); // 1 * 20 = 20

      expect(cart.getTotal()).toBe(40);
    });

    it('should return 0 for empty cart', () => {
      expect(cart.getTotal()).toBe(0);
    });
  });

  describe('getItemCount', () => {
    it('should count total number of items', () => {
      cart.addItem(product1, 2);
      cart.addItem(product2, 3);

      expect(cart.getItemCount()).toBe(5);
    });

    it('should return 0 for empty cart', () => {
      expect(cart.getItemCount()).toBe(0);
    });
  });

  describe('toJSON and fromJSON', () => {
    it('should serialize and deserialize correctly', () => {
      cart.addItem(product1, 2);
      cart.addItem(product2, 1);

      const json = cart.toJSON();
      const deserialized = Cart.fromJSON(json);

      expect(deserialized.items).toHaveLength(2);
      expect(deserialized.items[0].quantity).toBe(2);
      expect(deserialized.getTotal()).toBe(cart.getTotal());
    });
  });
});
```

### Tests : StorageService

```javascript
// tests/services/StorageService.test.js

import { describe, it, expect, beforeEach, vi } from 'vitest';
import { StorageService } from '../../src/js/services/StorageService.js';

describe('StorageService', () => {
  let service;

  beforeEach(() => {
    service = new StorageService('test');
    localStorage.clear.mockClear();
    localStorage.getItem.mockClear();
    localStorage.setItem.mockClear();
  });

  describe('save', () => {
    it('should save data to localStorage', () => {
      const data = { test: 'value' };
      
      service.save('key', data);

      expect(localStorage.setItem).toHaveBeenCalledWith(
        'test_key',
        JSON.stringify(data)
      );
    });

    it('should return true on success', () => {
      const result = service.save('key', {});
      expect(result).toBe(true);
    });

    it('should return false on error', () => {
      localStorage.setItem.mockImplementation(() => {
        throw new Error('Storage error');
      });

      const result = service.save('key', {});
      expect(result).toBe(false);
    });
  });

  describe('load', () => {
    it('should load data from localStorage', () => {
      const data = { test: 'value' };
      localStorage.getItem.mockReturnValue(JSON.stringify(data));

      const result = service.load('key');

      expect(localStorage.getItem).toHaveBeenCalledWith('test_key');
      expect(result).toEqual(data);
    });

    it('should return null if key does not exist', () => {
      localStorage.getItem.mockReturnValue(null);

      const result = service.load('nonexistent');

      expect(result).toBeNull();
    });

    it('should return null on error', () => {
      localStorage.getItem.mockImplementation(() => {
        throw new Error('Storage error');
      });

      const result = service.load('key');
      expect(result).toBeNull();
    });
  });

  describe('remove', () => {
    it('should remove data from localStorage', () => {
      service.remove('key');

      expect(localStorage.removeItem).toHaveBeenCalledWith('test_key');
    });

    it('should return true on success', () => {
      const result = service.remove('key');
      expect(result).toBe(true);
    });
  });

  describe('clear', () => {
    it('should clear only keys with the app prefix', () => {
      Object.defineProperty(global, 'localStorage', {
        value: {
          ...localStorageMock,
          length: 3,
          key: (i) => ['test_key1', 'test_key2', 'other_key'][i]
        },
        writable: true
      });

      service.clear();

      expect(localStorage.removeItem).toHaveBeenCalledTimes(2);
      expect(localStorage.removeItem).toHaveBeenCalledWith('test_key1');
      expect(localStorage.removeItem).toHaveBeenCalledWith('test_key2');
    });
  });
});
```

### Tests : AuthService

```javascript
// tests/services/AuthService.test.js

import { describe, it, expect, beforeEach, vi } from 'vitest';
import { AuthService } from '../../src/js/services/AuthService.js';
import { User } from '../../src/js/models/User.js';

describe('AuthService', () => {
  let service;

  beforeEach(() => {
    service = new AuthService();
    localStorage.clear.mockClear();
  });

  describe('register', () => {
    it('should register a new user', async () => {
      localStorage.getItem.mockReturnValue(JSON.stringify([]));

      const result = await service.register('test@example.com', 'Password123', 'Test User');

      expect(result.success).toBe(true);
      expect(result.user).toBeInstanceOf(User);
      expect(result.user.email).toBe('test@example.com');
    });

    it('should fail if email already exists', async () => {
      const existingUsers = [{
        id: 1,
        email: 'test@example.com',
        name: 'Existing',
        password: 'hashed'
      }];

      localStorage.getItem.mockReturnValue(JSON.stringify(existingUsers));

      const result = await service.register('test@example.com', 'Password123', 'New User');

      expect(result.success).toBe(false);
      expect(result.errors).toContain('Cet email est déjà utilisé');
    });

    it('should fail validation for invalid data', async () => {
      const result = await service.register('invalid-email', 'weak', 'A');

      expect(result.success).toBe(false);
      expect(result.errors.length).toBeGreaterThan(0);
    });
  });

  describe('login', () => {
    beforeEach(async () => {
      // Setup: Register a user first
      localStorage.getItem.mockReturnValue(JSON.stringify([]));
      await service.register('test@example.com', 'Password123', 'Test');
    });

    it('should login with correct credentials', async () => {
      const result = await service.login('test@example.com', 'Password123');

      expect(result.success).toBe(true);
      expect(service.isAuthenticated()).toBe(true);
    });

    it('should fail with wrong password', async () => {
      const result = await service.login('test@example.com', 'WrongPassword');

      expect(result.success).toBe(false);
      expect(result.errors).toContain('Email ou mot de passe incorrect');
    });

    it('should fail with non-existent email', async () => {
      const result = await service.login('nonexistent@example.com', 'Password123');

      expect(result.success).toBe(false);
      expect(result.errors).toContain('Email ou mot de passe incorrect');
    });
  });

  describe('logout', () => {
    it('should clear current user and session', async () => {
      localStorage.getItem.mockReturnValue(JSON.stringify([]));
      await service.register('test@example.com', 'Password123', 'Test');

      service.logout();

      expect(service.isAuthenticated()).toBe(false);
      expect(service.getCurrentUser()).toBeNull();
    });
  });

  describe('isAuthenticated', () => {
    it('should return false by default', () => {
      expect(service.isAuthenticated()).toBe(false);
    });

    it('should return true after login', async () => {
      localStorage.getItem.mockReturnValue(JSON.stringify([]));
      await service.register('test@example.com', 'Password123', 'Test');

      expect(service.isAuthenticated()).toBe(true);
    });
  });
});
```

### Script de Tests

```json
{
  "scripts": {
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest --coverage",
    "test:run": "vitest run"
  }
}
```

---

## 8. E-commerce : Performance & Optimisation

### Lazy Loading des Images

```javascript
// utils/lazyLoad.js

export class LazyLoader {
  constructor(options = {}) {
    this.options = {
      root: null,
      rootMargin: '50px',
      threshold: 0.01,
      ...options
    };

    this.observer = null;
    this.init();
  }

  init() {
    if ('IntersectionObserver' in window) {
      this.observer = new IntersectionObserver(
        this.handleIntersection.bind(this),
        this.options
      );
    } else {
      // Fallback : charge toutes les images immédiatement
      this.loadAllImages();
    }
  }

  observe(elements) {
    if (!this.observer) {
      this.loadAllImages();
      return;
    }

    elements.forEach(element => {
      this.observer.observe(element);
    });
  }

  handleIntersection(entries) {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        this.loadImage(entry.target);
        this.observer.unobserve(entry.target);
      }
    });
  }

  loadImage(img) {
    const src = img.dataset.src;
    
    if (!src) return;

    // Précharge l'image
    const tempImg = new Image();
    tempImg.onload = () => {
      img.src = src;
      img.classList.add('loaded');
    };
    tempImg.src = src;
  }

  loadAllImages() {
    const images = document.querySelectorAll('img[data-src]');
    images.forEach(img => this.loadImage(img));
  }

  disconnect() {
    if (this.observer) {
      this.observer.disconnect();
    }
  }
}

// Singleton
export const lazyLoader = new LazyLoader();
```

### Debounce pour la Recherche

```javascript
// utils/debounce.js

export function debounce(func, delay) {
  let timeoutId;

  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeoutId);
      func.apply(this, args);
    };

    clearTimeout(timeoutId);
    timeoutId = setTimeout(later, delay);
  };
}

// Utilisation dans ProductController
setupEventListeners() {
  const searchInput = document.getElementById('search-input');
  
  if (searchInput) {
    searchInput.addEventListener('input', debounce((e) => {
      this.searchQuery = e.target.value.trim();
      this.applyFilters();
    }, 300));
  }
}
```

### Pagination des Produits

```javascript
// controllers/ProductController.js

export class ProductController {
  constructor() {
    this.productsPerPage = 12;
    this.currentPage = 1;
    this.totalPages = 1;
  }

  applyFilters() {
    // ... filtres existants

    // Calcule la pagination
    this.totalPages = Math.ceil(this.filteredProducts.length / this.productsPerPage);
    this.currentPage = Math.min(this.currentPage, this.totalPages || 1);

    this.render();
  }

  getCurrentPageProducts() {
    const start = (this.currentPage - 1) * this.productsPerPage;
    const end = start + this.productsPerPage;
    return this.filteredProducts.slice(start, end);
  }

  goToPage(page) {
    this.currentPage = Math.max(1, Math.min(page, this.totalPages));
    this.render();
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  render() {
    const products = this.getCurrentPageProducts();
    this.view.render(products, {
      currentPage: this.currentPage,
      totalPages: this.totalPages,
      totalProducts: this.filteredProducts.length
    });
  }
}
```

### Cache des Requêtes

```javascript
// services/CacheService.js

export class CacheService {
  constructor(ttl = 300000) { // 5 minutes par défaut
    this.cache = new Map();
    this.ttl = ttl;
  }

  set(key, value) {
    this.cache.set(key, {
      value,
      timestamp: Date.now()
    });
  }

  get(key) {
    const item = this.cache.get(key);

    if (!item) {
      return null;
    }

    // Vérifie si le cache est expiré
    if (Date.now() - item.timestamp > this.ttl) {
      this.cache.delete(key);
      return null;
    }

    return item.value;
  }

  has(key) {
    return this.get(key) !== null;
  }

  clear() {
    this.cache.clear();
  }

  delete(key) {
    this.cache.delete(key);
  }
}

export const cache = new CacheService();
```

---

**🎉 PARTIE 8.2 TERMINÉE !**

Tu as maintenant :
✅ **Panier complet** avec gestion d'état
✅ **Checkout multi-étapes** avec validation
✅ **Admin Dashboard** CRUD complet
✅ **Tests unitaires** et d'intégration
✅ **Optimisations** performance

**Prochaine étape : Partie 8.3**
- 📱 Réseau Social DevConnect complet
- 🚀 Déploiement & CI/CD
- 💼 Portfolio & Carrière

**📚 JavaScript - Partie 8.2 : E-commerce Complet**
**💎 100% Gratuit • Pour Tous • À Jamais**
**🔗 GitHub : [ivguenyp-dev/Java-script](https://github.com/ivguenyp-dev/Java-script)**