<template>
  <div class="layout-row wrap justify-content-center">
    <section v-for="(product, index) in localProducts" :key="index" :data-testid="'product-item-' + index" class="w-30 product-item">
      <div class="card ma-16">
        <img :src="product.image" class="product-image" :alt="product.name"/>
        <div class="card-text pa-4">
          <h5 class="ma-0 text-center">{{ product.name }}</h5>
          <p class="ma-0 mt-8 text-center">${{ product.price }}</p>
        </div>
        <div class="card-actions justify-content-center pa-4">
          <button v-if="product.cartQuantity === 0" class="x-small outlined" data-testid="btn-item-add" @click="addToCart(product)">Add To Cart</button>
          <button v-else class="x-small danger" data-testid="btn-item-remove" @click="removeFromCart(product)">Remove</button>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: "ProductList",
  props: {
    products: Array
  },
  data() {
    return {
      localProducts: []
    };
  },
  mounted() {
    this.localProducts = [...this.products]; // Create a copy of the prop
  },
  methods: {
    addToCart(product) {
      const index = this.localProducts.findIndex(p => p.id === product.id);
      this.localProducts[index].cartQuantity = 1;
      this.$emit("add-to-cart", product);
    },
    removeFromCart(product) {
      const index = this.localProducts.findIndex(p => p.id === product.id);
      this.localProducts[index].cartQuantity = 0;
      this.$emit("remove-from-cart", product);
    }
  }
};
</script>

<style scoped lang="scss">
.product-image {
  margin: 32px 30%;
  width: 40%;
  min-height: 90px;
}
</style>