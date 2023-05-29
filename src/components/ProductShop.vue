<script>
import "../assets/Style.css";
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
  computed: {
    currentProduct() {
      return this.visibleProducts[this.productIndex];
    },
    roundRating() {
      if (this.currentProduct && this.currentProduct.rating) {
        const rating = this.currentProduct.rating.rate;
        const fullRound = Math.floor(rating);
        const halfRound = rating % 1 !== 0;
        const emptyRound = 5 - Math.ceil(rating);
        return {
          fullRound: fullRound,
          halfRound: halfRound,
          emptyRound: emptyRound,
        };
      } else {
        return null;
      }
    },
  },
};
</script>
<template>
  <div>
    <section v-if="data">
      <div
        v-for="product in visibleProducts"
        :key="product.id"
        v-show="productIndex === visibleProducts.indexOf(product)"
        :class="{
          'women-clothing': product.category === 'women\'s clothing',
          'men-clothing': product.category === 'men\'s clothing',
        }"
      >
        <div class="bg">
          <img
            :src="product.image"
            :alt="product.title"
            class="product-image"
          />
          <div class="product">
            <h2 class="product-title">{{ product.title }}</h2>
            <div class="information">
              <h3 class="product-category">{{ product.category }}</h3>
              <div class="rating">
                <h2 class="rate">{{ product.rating.rate }}/5</h2>
                <template v-if="roundRating">
                  <template
                    v-for="(item, index) in roundRating.fullRound"
                    :key="index"
                  >
                    <span class="round-icon"></span>
                  </template>
                  <span v-if="roundRating.halfRound" class="round-icon"></span>
                  <template
                    v-for="(item, index) in roundRating.emptyRound"
                    :key="index"
                  >
                    <span class="round-icon empty"></span>
                  </template>
                </template>
              </div>
            </div>
            <hr />
            <br />
            <p class="product-description">{{ product.description }}</p>

            <hr class="line" />
            <h2 class="product-price">{{ product.price }}</h2>
            <div class="button-pruduct">
              <button @click="nextProduct" class="next-button">next</button>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
