<template>
  <div class="display" :class=
  "{
    'bg-brightBlue': ProductAvailable && (products.data.category === 'men\'s clothing'),
    'bg-palePurple': ProductAvailable && (products.data.category === 'women\'s clothing'),
  }">
    <div v-if="loading">
      <Loader />
    </div>
    <div class="card">
      <div class="product-available" :class="!ProductAvailable ? 'display-none' : 'product-avaible'">
        <div class="card-product-stuff">
          <img :src="products?.data?.image" class="product-photos" />
        </div>
        <div class="card-product-description">
          <h1 class="product-name" :class="{
            'color-darkBlue': products?.data?.category === 'men\'s clothing',
            'color-darkPurple': products?.data?.category === 'women\'s clothing',
          }">
            {{ products?.data?.title }}
          </h1>
          <div class="product-explanation">
            <p class="product_category">{{ products?.data?.category }}</p>
            <div class="rating">
              <p class="rating_product">{{ products?.data?.rating?.rate }}</p>
              <div class="rating-circle">
                <div v-for="maxRate in maxRates" :key="maxRate" class="circle" :class="(products?.data?.category === 'men\'s clothing' || products?.data?.category === 'women\'s clothing') ? 'bg-darkBlue' : 'bg-darkPurple'"></div>
              </div>
            </div>
          </div>
          <hr />
          <p class="description">{{ products?.data?.description }}</p>
          <div class="down">
            <hr />
            <h2 class="price" :class="(products?.data?.category === 'men\'s clothing' || products?.data?.category === 'women\'s clothing') ? 'color-darkBlue' : 'color-darkPurple'">
              ${{ products?.data?.price }}
            </h2>
            <div class="button-container">
              <button class="button" :class="(products?.data?.category === 'men\'s clothing' || products?.data?.category === 'women\'s clothing') ? 'font-white bg-darkBlue' : 'font-white bg-darkPurple'">
                Buy Now
              </button>
              <button @click="singleProduct()" class="button button-outline" :class="(products?.data?.category === 'men\'s clothing' || products?.data?.category === 'women\'s clothing') ? 'border-darkBlue' : 'border-darkPurple'">
                Next Product
              </button>
            </div>
          </div>
        </div>
      </div>
   
      <div class="card-unavailable" :class="ProductAvailable ? 'display-none' : 'product_cart-no-avaible'">
        <p class="no">This product is unavailable to show</p>
        <button class="button-unavailable" @click="singleProduct()">Next product</button>
      </div>
    </div>
  </div>
</template>
  
<script>
import Loader from '../components/Loader.vue'
export default {
  name: 'EcommerceLoader',
  components: {
    Loader
  },

  data() {
    return {
      currentProductIndex: 0,
      products: {},
      maxRates: [1, 2, 3, 4, 5],
      loading: false,
      ProductAvailable: false
    };
  },

  computed: {
    isMenClothing() {
      return this.ProductAvailable && (this.products.data.category === "men's clothing");
    },
    isWomenClothing() {
      return this.ProductAvailable && (this.products.data.category === "women's clothing");
    },
  },

  methods: {
    async API() {
      try {
        const url = `https://fakestoreapi.com/products/${this.currentProductIndex}`;
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error('Request Failed');
        }
        const result = await response.json();
        return result;
      } catch (error) {
        this.error = error.message;
      }
    },

    async singleProduct() {
      this.loading = true;

      if (this.currentProductIndex !== 20) {
        this.currentProductIndex++;
      } else {
        this.currentProductIndex = 1;
      }

      const data = await this.API();
      console.log(data);
      if (data.category === "men's clothing" || data.category === "women's clothing") {
        console.log("Product is available:", data);
        this.products = { data };
        this.ProductAvailable = true;
      } else {
        console.log("Product is not available:", data);
        this.ProductAvailable = false;
      }

      this.loading = false;
    }
  },

  mounted() {
    this.singleProduct();
  }
}
</script>
  
<style>
@import url('../assets/style/page.css');
@import url('../assets/style/root.css');
</style>
