<template>
  <main>
    <section class="section category-list">
      <div class="container">
        <div class="">
          <div class="column-item column-item-main">
            <div class="column-item-main-top columns">
              <div class="column-item-main-top-left column">
                <figure class="image product-image">
                  <img :src="product.metadata.image.imgix_url" alt="">
                </figure>
              </div>

              <div class="column-item-main-top-right column">
                <nuxt-link  :to="`/product?productId=${product._id}`" class="back-to-shopping has-text-weight-bold">
                  <h1 class="is-size-4 has-text-weight-bold subtitle   has-text-link  product-title" >
                    {{ product.title }}
                  </h1>
                </nuxt-link>
                <div>
                  <strong>Category:</strong> <span class="category-title"> {{ product.metadata.type }}</span>
                </div>
                <div>
                  <strong>Total Sold:</strong> <span class="category-title"> 200+</span>
                </div>
                <h2 class="is-size-4 has-text-weight-bold">$ {{ product.metadata.price}}</h2>
                <cartControl :product="product"/>
              </div>
            </div>

            <div class="column-item-main-bottom" v-if="product.content" v-html="product.content"/>
          </div>

        <div class="column-item">
          <div class="columns is-multiline is-variable is-2">
            <div class="column is-one-third" v-for="product in similarProducts" :key="product._id">
              <product-item :product="product"></product-item>
            </div>
          </div>
        </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import productItem from "@/components/product-item";
import addToCartBtn from "@/components/add-to-cart-btn";
import cartControl from "@/components/cart-control";
export default {
  components: {
    productItem,
    addToCartBtn,
    cartControl
  },
  data() {
    return {
      quantity: 1,
      types: ["painted", "pattern", "gradient"]
    };
  },
  computed: {
    productId() {
      return this.$route.query.productId;
    },
    product() {
      return this.$store.state.products.filter(
        el => el._id == this.productId
      )[0];
    },
    similarProducts() {
      return this.$store.state.products.filter(
        el =>
          el.metadata.type == this.product.metadata.type &&
          el._id != this.productId
      );
    }
  },
  async fetch({ store }) {
    await store.dispatch("getProducts");
  }
};
</script>

<style scoped>
.column-item-main-top {
  margin: 0;
  border-radius: 4px;
  margin-bottom: 24px;
}
.column-item-main-top-left {
  background: #fff;
  border-radius: 4px;
}
.column-item-main-top-left .image {
  margin: auto;
  width: 180px;
}
.column-item-main-bottom {
  background: #fff;
  border-radius: 4px;
  margin-bottom: 24px;
  padding: 16px;
}
.category-title {
  text-transform: capitalize;
}
</style>
