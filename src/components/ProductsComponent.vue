<template>
  <div class="container">
    <h2 class="title-products">Productos</h2>
    <div class="row">
      <div
        class="col-md-4 mb-4"
        v-for="product in products"
        :key="product.id"
      >
        <div class="card">
          <img :src="product.picture" class="card-img-top" alt="Product Image" @click="showProductDetails(product)">
          <div class="card-body">
            <h5 class="card-title">{{ product.name }}</h5>
            <p class="card-text">{{ product.description }}</p>
            <p class="card-text">Precio: {{ product.price | formatPrice }}</p>
            <button class="btn btn-primary" @click="addToCart(product)">
              Agregar al carrito
            </button>
          </div>
        </div>
      </div>
    </div>
    <DetailProductComponent
      :selected-product="selectedProduct"
      @close-modal="closeModal"
    />
  </div>
</template>

<script>
import DetailProductComponent from "@/components/DetailProductComponent.vue";

export default {
  name: "ProductsComponent",
  props: {
    products: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selectedProduct: null,
    };
  },
  components: {
    DetailProductComponent,
  },
  methods: {
    addToCart(product) {
      this.$emit("add-to-cart", product);
    },
    showProductDetails(product) {
      this.selectedProduct = product;
    },
    closeModal() {
      this.selectedProduct = null;
    },
  },
  filters: {
    formatPrice(value) {
      return `CLP $${value}`;
    },
  },
};
</script>
<style scoped>
  .container {
    margin-top: 12vh;
  }
</style>