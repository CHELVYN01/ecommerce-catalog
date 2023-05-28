<script>
export default {
  data() {
    return {
      data: null,
      productIndex: 0,
      visibleProducts: [],
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch("https://fakestoreapi.com/products");
        const data = await response.json();
        this.data = data;
        console.log(data);
        this.filterProducts();
      } catch (error) {
        console.error(error);
      }
    },
    filterProducts() {
      if (this.data) {
        this.visibleProducts = this.data.filter(
          (product) =>
            product.category === "men's clothing" ||
            product.category === "women's clothing"
        );
      }
    },
    nextProduct() {
      if (this.productIndex < this.visibleProducts.length - 1) {
        this.productIndex++;
      }
    },
  },
};
</script>

<template>
  <div>
    <div v-if="data">
      <h2 class="category-title">Semua Kategori:</h2>
      <div class="product-container">
        <div
          v-for="product in visibleProducts"
          :key="product.id"
          v-show="productIndex === visibleProducts.indexOf(product)"
          :class="{
            'women-clothing': product.category === 'women\'s clothing',
            'men-clothing': product.category === 'men\'s clothing',
          }"
        >
          <img
            :src="product.image"
            :alt="product.title"
            class="product-image"
          />
          <p class="product-title">{{ product.title }}</p>
          <button @click="nextProduct" class="next-button">Next</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.category-title {
  font-size: 24px;
  margin-bottom: 20px;
}

.product-container {
  display: flex;
  flex-wrap: wrap;
}

.product-container > div {
  flex: 0 0 300px;
  margin: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  text-align: center;
}

.women-clothing {
  background-color: #fde2ff;
}

.men-clothing {
  background-color: #d6e6ff;
}

.product-image {
  width: 200px;
  height: 200px;
  object-fit: cover;
  margin-bottom: 10px;
}

.product-title {
  font-size: 16px;
  margin-bottom: 10px;
}

.next-button {
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
