<script>
import "../assets/css/Style.css";
export default {
  data() {
    return {
      data: null,
      productIndex: 0,
      visibleProducts: [],
      dataLoaded: false,
      loading: false,
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        this.loading = true; // Mengatur loading ke true sebelum permintaan data
        const response = await fetch("https://fakestoreapi.com/products");
        const data = await response.json();
        this.data = data;
        this.visibleProducts = data;
        this.dataLoaded = true;
      } catch (error) {
        console.error(error);
      } finally {
        setTimeout(() => {
          this.loading = false; // Mengatur loading ke false setelah sedikit penundaan
        }, 500); // Mengatur loading ke false setelah permintaan data selesai
      }
    },
    nextProduct() {
      if (this.productIndex < this.visibleProducts.length - 1) {
        this.productIndex++;
      } else {
        this.productIndex = 0;
      }
      this.loading = true; // Mengatur loading ke true setelah tombol "next product" ditekan
      setTimeout(() => {
        this.loading = false; // Mengatur loading ke false setelah penundaan (Anda bisa menyesuaikan waktu penundaan)
      }, 200); // Mengatur loading ke false setelah penundaan (Anda bisa menyesuaikan waktu penundaan)
    },
    isClothingCategory(category) {
      return category === "men's clothing" || category === "women's clothing";
    },
  },
  computed: {
    currentProduct() {
      return this.visibleProducts[this.productIndex];
    },
    roundRating() {
      if (this.currentProduct && this.currentProduct.rating) {
        const rating = this.currentProduct.rating.rate;
        const fullRound = Math.floor(rating); // Pembulatan ke bawah dari rating
        const halfRound = rating % 1 !== 0; // Cek apakah rating memiliki pecahan (setengah)
        const emptyRound = 5 - Math.ceil(rating); // Jumlah rating kosong yang perlu ditampilkan
        return {
          fullRound: fullRound,
          halfRound: halfRound,
          emptyRound: emptyRound,
        };
      } else {
        return null; // Mengembalikan null jika tidak ada rating
      }
    },
  },
};
</script>
<template>
  <div>
    <section v-if="data">
      <div v-show="!loading">
        <div
          v-for="product in visibleProducts"
          :key="product.category"
          v-show="productIndex === visibleProducts.indexOf(product)"
        >
          <div
            v-if="isClothingCategory(visibleProducts[productIndex].category)"
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
                      <span
                        v-for="(item, index) in roundRating.fullRound"
                        :key="index"
                        class="round-icon"
                      ></span>
                      <span
                        v-if="roundRating.halfRound"
                        class="round-icon"
                      ></span>
                      <span
                        v-for="(item, index) in roundRating.emptyRound"
                        :key="index"
                        class="round-icon empty"
                      ></span>
                    </template>
                  </div>
                </div>
                <hr />
                <br />
                <p class="product-description">{{ product.description }}</p>
                <div class="line"></div>
                <h2 class="product-price">${{ product.price }}</h2>
                <div class="button-pruduct">
                  <button class="button-pruduct buy">buy now</button>
                  <button @click="nextProduct" class="button-pruduct next">
                    next product
                  </button>
                </div>
              </div>
            </div>
          </div>
          <div v-else>
            <div class="unavailable">
              <div class="bg">
                <div class="container-unavailable">
                  <p class="unavailable-font">
                    This product is unavailable to show
                  </p>
                  <button @click="nextProduct" class="unavailable-next">
                    next product
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-show="loading" class="loading">
        <!-- <div class="bg"></div> -->
      </div>
    </section>
  </div>
</template>
