<template>
  <div class="app-container">
    <h1>Vue.js E-commerce Store</h1>
    <div class="main-content">
      <ProductList @add-to-cart="addToCart" />
      <Cart :cartItems="cartItems" @update-quantity="updateQuantity" @checkout="startCheckout" />
    </div>
    <Checkout v-if="checkingOut" :cartItems="cartItems" @payment-complete="paymentComplete" @cancel="cancelCheckout" />
  </div>
</template>

<script>
import ProductList from './components/ProductList.vue';
import Cart from './components/Cart.vue';
import Checkout from './components/Checkout.vue';

export default {
  components: {
    ProductList,
    Cart,
    Checkout
  },
  data() {
    return {
      cartItems: [],
      checkingOut: false
    };
  },
  methods: {
    addToCart(product) {
      const existing = this.cartItems.find(item => item.id === product.id);
      if (existing) {
        existing.quantity++;
      } else {
        this.cartItems.push({ ...product, quantity: 1 });
      }
    },
    updateQuantity({ id, quantity }) {
      const item = this.cartItems.find(i => i.id === id);
      if (item) {
        item.quantity = quantity;
        if (item.quantity <= 0) {
          this.cartItems = this.cartItems.filter(i => i.id !== id);
        }
      }
    },
    startCheckout() {
      if (this.cartItems.length > 0) {
        this.checkingOut = true;
      } else {
        alert('Your cart is empty.');
      }
    },
    paymentComplete() {
      alert('Payment successful! Thank you for your purchase.');
      this.cartItems = [];
      this.checkingOut = false;
    },
    cancelCheckout() {
      this.checkingOut = false;
    }
  }
};
</script>

<style scoped>
.app-container {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}
h1 {
  text-align: center;
  margin-bottom: 20px;
}
.main-content {
  display: flex;
  gap: 20px;
}
</style>
</create_file>
