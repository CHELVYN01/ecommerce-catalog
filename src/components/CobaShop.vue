<template>
  <div>
    <div v-if="dataLoaded">
      <h2 class="category-title">Semua Produk:</h2>
      <div class="product-container">
        <div v-show="visibleProducts.length > 0">
          <img
            v-if="isClothingCategory(visibleProducts[productIndex].category)"
            :src="visibleProducts[productIndex].image"
            :alt="visibleProducts[productIndex].title"
            class="product-image"
          />
          <p
            v-if="isClothingCategory(visibleProducts[productIndex].category)"
            class="product-title"
          >
            {{ visibleProducts[productIndex].title }}
          </p>
          <div v-else class="not-found">
            <p class="not-found-text">Not Found</p>
          </div>
        </div>
        <div v-show="visibleProducts.length === 0" class="not-found">
          <p class="not-found-text">Not Found</p>
        </div>
      </div>
      <button @click="nextProduct" class="next-button">Next</button>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: null,
      productIndex: 0,
      visibleProducts: [],
      dataLoaded: false,
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
        this.visibleProducts = data;
        this.dataLoaded = true;
      } catch (error) {
        console.error(error);
      }
    },
    nextProduct() {
      if (this.productIndex < this.visibleProducts.length - 1) {
        this.productIndex++;
      } else {
        this.productIndex = 0;
      }
    },
    isClothingCategory(category) {
      return category === "men's clothing" || category === "women's clothing";
    },
  },
};
</script>

<style>
.category-title {
  font-size: 24px;
  margin-bottom: 20px;
}

.product-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

.product-image {
  width: 300px;
  height: auto;
  margin-bottom: 10px;
}

.product-title {
  font-size: 16px;
  margin-bottom: 10px;
}

.not-found {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 200px;
  background-color: #ffffff;
  margin-top: 20px;
}

.not-found-text {
  font-size: 16px;
  margin-bottom: 10px;
  color: red;
}

.next-button {
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
}
</style>
