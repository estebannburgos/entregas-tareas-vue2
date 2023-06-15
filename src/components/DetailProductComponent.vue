<template>
    <div class="detail-product-modal" v-if="selectedProduct">
      <div class="detail-product-card">
        <span class="close-button" @click="closeModal">X</span>
        <img :src="selectedProduct.picture" class="card-img-top" alt="Product Image">
        <div class="card-body">
          <h5 class="card-title">{{ selectedProduct.name }}</h5>
          <p class="card-text">{{ selectedProduct.description }}</p>
          <p class="card-text">Precio: {{ selectedProduct.price | formatPrice }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <style scoped>
  .detail-product-modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.5);
  }
  
  .detail-product-card {
    width: 400px;
    background-color: #fff;
    padding: 1rem;
    border-radius: 5px;
  }
  
  .detail-product-card img {
    width: 100%;
    height: auto;
    border-radius: 5px;
    margin-bottom: 1rem;
  }
  
  .detail-product-card button {
    margin-top: 1rem;
  }
  .close-button {
  position: absolute;
  cursor: pointer;
  font-size: 20px;
  font-weight: bold;
  color: silver;
}
  </style>
  
  <script>
  export default {
    name: "DetailProductComponent",
    props: {
      selectedProduct: {
        type: Object,
        default: null,
      },
    },
    methods: {
      closeModal() {
        this.$emit("close-modal");
      },
    },
    filters: {
    formatPrice(value) {
      return `CLP $${value}`;
    },
  },
//     data() {
//     return {
//       selectedProduct: {},
//     };
//   },
  };
  </script>
  