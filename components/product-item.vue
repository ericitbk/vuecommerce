<template>
  <div>
    <div class="box product-box">
      <figure class="image product-image" @click="modalActive = true">
        <img :src="product.metadata.image.imgix_url" alt="">
      </figure>
      <nuxt-link  :to="`/product?productId=${product._id}`" class="back-to-shopping has-text-weight-bold">
        <p class="subtitle has-text-centered  has-text-link  product-title" >
          {{ product.title }}
        </p>
      </nuxt-link>
      <addToCartBtn :product="product" @addToCart="addToCart" />
    </div>

    <b-modal :active.sync="modalActive">
        <div class="box modal-box">
          <a class="close-button" @click.prevent="modalActive = false">X</a>
          <div class="columns columns-wrapper">
            <div class="column is-hidden-mobile">
              <figure class="image modal-image">
                <img :src="product.metadata.image.imgix_url" alt="">
              </figure>
            </div>
            <div class="column column-left">
              <div class="box is-bordered">
                <p class="is-size-4 has-text-weight-bold is-black">
                  {{ product.title }}
                </p>
                <p class="description" v-html="product.content"></p>
                <addToCartBtn :product="product" @addToCart="addToCart" />
              </div>
            </div>
          </div>
        </div>
    </b-modal>
  </div>
</template>

<script>
import addToCartBtn from "@/components/add-to-cart-btn";
export default {
  props: ["product"],
  components: {
    addToCartBtn
  },
  data() {
    return {
      modalActive: false
    };
  },
  methods: {
    addToCart(item) {
      this.$store.commit("addToCart", item);
      this.$toast.open({
        message: "Added to cart",
        type: "is-success"
      });
    }
  }
};
</script>

<style scoped>
.product-image img {
  height: 200px;
  width: auto;
  margin: 0 auto;
  cursor: pointer;
}
.modal-image img {
  height: 300px;
  width: auto;
  margin: 0 auto;
}
.product-box {
  height: 100%;
}

.product-box button {
  margin: 0 auto;
}
.product-title {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  cursor: pointer;
  margin: 12px auto;
}
.product-title:hover {
  text-decoration: underline;
}
.box {
  background: #fff;
  box-shadow: none;
}
.box:not(:last-child) {
  margin-bottom: auto;
}
.modal-box {
  background: #fff;
  padding: 0;
}
.columns-wrapper {
  margin: 0;
}
.modal-box .column-left {
  background: #dae2db;
  padding: 16px;
}
p.description {
  margin: 10px 0 30px;
  line-height: 1.25;
}
.box.is-bordered {
  border-radius: 0;
  height: 100%;
}
.close-button {
  position: absolute;
  right: 6px;
  font-size: 0.8rem;
  color: #fff;
  top: 6px;
  background: red;
  opacity: 0.5;
  width: 24px;
  height: 24px;
  border-radius: 12px;
  text-align: center;
  padding: 4px;
  font-weight: 600;
}
.close-button:hover {
  opacity: 1;
}
</style>
