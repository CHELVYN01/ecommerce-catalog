<script>
import "../assets/Style.css";
export default {
  data() {
    return {
      data: null,
    };
  },
  methods: {
    async fetchData() {
      try {
        const response = await fetch("https://fakestoreapi.com/products");
        const data = await response.json();
        this.data = data;
        console.log(data);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>
<template>
  <div>
    <section class="Containerbg-Pattern">
      <div class="bg-Pattern">
        <img src="../assets/img/bg-pattern.svg" alt="" />
      </div>
      <div>
        <button @click="fetchData">Ambil Data</button>
        <div v-if="data">
          <ul>
            <li v-for="product in data" :key="product.id">
              {{ product.title }}
            </li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>

<style></style>
