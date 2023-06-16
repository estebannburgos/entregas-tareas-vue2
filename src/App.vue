<template>
  <div id="app">
    <NavbarComponent
      :cart-item-count="cartItemCount"
      :user-avatar="userAvatar"
      @toggle-login="toggleLogin"
      @toggle-cart="toggleCart"
      @logout-clicked="logout"
    />
    <div v-if="!showLogin">
      <ProductsComponent
        :products="products"
        :cart-items="cartItems"
        @add-to-cart="addToCart"
        @select-product="selectProduct"
      />
      <ShoppingCartComponent
        v-if="showCart"
        :cart-items="cartItems"
        :cart-subtotal="cartSubtotal"
        :cart-total="cartTotal"
        @toggle-cart="toggleCart"
        @remove-item="removeItem"
        @update-quantity="updateQuantity"
      />
    </div>
    <LoginRegisterComponent
      v-show="showLogin" 
      @close-login="toggleLogin"
    />
  </div>
</template>

<script>
import NavbarComponent from "@/components/NavbarComponent.vue";
import ProductsComponent from "@/components/ProductsComponent.vue";
import ShoppingCartComponent from "@/components/ShoppingCartComponent.vue";
import DetailProductComponent from "@/components/DetailProductComponent.vue";
import products from "@/assets/database/products.json";
import users from "@/assets/database/users.json";
import LoginRegisterComponent from "@/components/LoginRegisterComponent.vue";

export default {
  name: "App",
  components: {
    NavbarComponent,
    ProductsComponent,
    ShoppingCartComponent,
    DetailProductComponent,
    LoginRegisterComponent,
  },
  data() {
    return {
      products: products,
      cartItems: [],
      showCart: false,
      showLogin: false,
      userAvatar: "",
      selectedProduct: null,
    };
  },
  computed: {
    cartItemCount() {
      return this.cartItems.reduce((total, item) => total + item.quantity, 0);
    },
    cartSubtotal() {
      return this.cartItems.reduce(
        (total, item) => total + item.price * item.quantity,
        0
      );
    },
    cartTotal() {
      return this.cartSubtotal || 0;
    },
  },
  methods: {
    addToCart(product) {
      const existingProduct = this.cartItems.find(
        (item) => item.id === product.id
      );
      if (existingProduct) {
        existingProduct.quantity++;
      } else {
        this.cartItems.push({ ...product, quantity: 1 });
      }
    },
    toggleCart() {
      this.showCart = !this.showCart;
    },
    removeItem(itemId) {
      this.cartItems = this.cartItems.filter((item) => item.id !== itemId);
    },
    updateQuantity(item, quantity) {
      item.quantity = quantity;
    },
    toggleLogin() {
      this.showLogin = !this.showLogin;
    },
    logout() {
      this.userAvatar = "";
      this.showLogin = false;
    },
    selectProduct(product) { // Agregamos el método para seleccionar un producto
      this.selectedProduct = product;
    },
    closeModal() {
      this.selectedProduct = null; // Reiniciamos el producto seleccionado cuando se cierra el modal
    },
  },
};
</script>