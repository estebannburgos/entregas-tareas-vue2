<template>
    <nav class="navbar">
      <div class="navbar-logo">
        <a href="#" class="navbar-logo-link">
          Logo
        </a>
      </div>
  
      <div class="navbar-items">
      <button class="navbar-button" @click="toggleCart">
        Carrito
        <span class="navbar-counter">{{ cartItemCount }}</span>
      </button>
      <div v-if="!loggedIn">
        <button class="navbar-button" @click="toggleLogin">Login</button>
      </div>
      <div v-else>
        <button class="navbar-button" @click="logout">Logout</button>
      </div>
    </div>
    </nav>
  </template>
  
  
<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background-color: #f8f8f8;
}

.navbar-logo {
  margin-right: 2rem;
}

.navbar-logo-link {
  text-decoration: none;
}

.navbar-logo-img {
  width: 50px;
  height: 50px;
}

.navbar-items {
  display: flex;
  align-items: center;
}

.navbar-button {
  padding: 0.5rem 1rem;
  margin-left: 1rem;
  background-color: rgb(42, 124, 134);
  color: #fff;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
}

.navbar-counter {
  margin-left: 0.25rem;
  background-color: #dc3545;
  color: #fff;
  border-radius: 50%;
  padding: 0.25rem;
  font-size: 0.75rem;
}
</style>
  
  <script>
  export default {
    name: "NavbarComponent",
    props: {
      cartItemCount: {
        type: Number,
        default: 0,
      },
    },
    methods: {
      toggleCart() {
        this.$emit("toggle-cart");
      },
      handleCartClick(event) {
        event.preventDefault();
        this.$emit("toggle-cart");
      },
      loginClicked() {
      this.$emit("login-clicked");
    },
    toggleLogin() {
      this.$emit("toggle-login");
    },
    logout() {
      this.$emit("logout");
    },
    },
  };
  </script>