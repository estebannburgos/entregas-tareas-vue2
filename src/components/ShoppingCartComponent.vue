<template>
  <div class="shopping-cart">
    <div class="cart-content">
      <button class="close-button" @click="closeCart">X</button>
      <h2>Carrito de compras</h2>
      <div v-if="cartItems.length === 0" class="empty-cart">
        No hay productos en el carrito.
      </div>
      <div v-else>
        <table class="table">
          <thead>
            <tr>
              <th>Producto</th>
              <th>Precio</th>
              <th>Cantidad</th>
              <th>Subtotal</th>
              <th>Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in cartItems" :key="item.id">
              <td>{{ item.name }}</td>
              <td>{{ item.price }}</td>
              <td>
                <button class="quantity-button" @click="decreaseQuantity(item)">
                  -
                </button>
                {{ item.quantity }}
                <button class="quantity-button" @click="increaseQuantity(item)">
                  +
                </button>
              </td>
              <td>{{ item.price * item.quantity }}</td>
              <td>
                <button class="remove-button" @click="removeItem(item.id)">
                  <i class="fa fa-trash"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="total">
          <strong>Total:</strong> {{ cartSubtotal }}
        </div>
        <button class="confirm-button" @click="confirmPurchase">
          Confirmar compra
        </button>
      </div>
    </div>
  </div>
</template>
  
<script>
export default {
  name: "ShoppingCartComponent",
  props: {
    cartItems: {
      type: Array,
      required: true,
    },
    cartSubtotal: {
      type: Number,
      required: true,
    },
    cartTotal: {
      type: Number,
      required: true,
    },
  },
  methods: {
    closeCart() {
      this.$emit("toggle-cart");
    },
    removeItem(itemId) {
      this.$emit("remove-item", itemId);
    },
    increaseQuantity(item) {
      item.quantity++;
      this.updateCartTotal();
    },
    decreaseQuantity(item) {
      if (item.quantity > 1) {
        item.quantity--;
        this.updateCartTotal();
      }
    },
    confirmPurchase() {
      alert('¡Felicidades por tu compra!');
      this.cartItems = [];
      this.$emit('update-cart-count', 0);
      this.closeCart();
    },
    updateCartTotal() {
      this.$forceUpdate();
    },
  },
};
</script>
  
<style scoped>
.shopping-cart {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.cart-content {
  width: 35rem;
  background-color: #fff;
  padding: 2rem;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  position: relative;
}

.close-button {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
}

.empty-cart {
  text-align: center;
  margin-top: 2rem;
  font-size: 1.2rem;
}

.cart-content h2 {
  margin-bottom: 1.5rem;
}

.table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 1.5rem;
}

.table th,
.table td {
  padding: 0.5rem;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.table th {
  font-weight: bold;
}

.quantity-button {
  background: none;
  border: none;
  cursor: pointer;
}

.remove-button {
  cursor: pointer;
  color: red;
}

.total {
  text-align: right;
  margin-bottom: 1.5rem;
}

.confirm-button {
  display: block;
  width: 100%;
  padding: 0.75rem 1rem;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
}
</style>