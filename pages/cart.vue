<template>
  <main>
    <section class="section">
      <div class="container">
        <nuxt-link to="/" class="back-to-shopping has-text-weight-bold"> &#8592; Back to shopping</nuxt-link>
        <div class="box">
          <h1 class="title has-text-centered">
            Your Cart
          </h1>
          <hr>
          <div class="box-wrapper">
          <div class=" box-item" v-for="product in cart" :key="product._id">
            <article class="media cart-item" v-if="product.count">
              <div class="media-left">
                <figure class="image cart-image is-128x128">
                  <img :src="product.metadata.image.imgix_url" alt="">
                </figure>
              </div>
              <div class="media-content">
                <div class="content">
                  <h2 class="is-size-4 has-text-weight-bold has-text-link">
                  <nuxt-link  :to="`/product?productId=${product._id}`">
                    {{ product.title }}
                  </nuxt-link></h2>
                  <p class="is-size-5">
                    ${{product.metadata.price}}
                  </p>
                  <button class="button is-outlined is-danger is-small is-rounded" @click="removeItem(product)">Remove Item</button>
                </div>
                <div class="content">
                  <cartControl :product="product" :cart-count="product.count"/>
                </div>
              </div>

              <div class="media-right">
                <div class="content">
                  <h2 class="is-size-4 has-text-weight-bold">$ {{ product.metadata.price * product.count }}</h2>
                </div>
              </div>
            </article>
          </div>
          </div>
          <hr v-if="cartTotal">
          <h1 class="title has-text-centered" v-if="cartTotal"> Total : ${{ totalCost }}</h1>
          <button class="button is-medium is-fullwidth is-outlined is-rounded is-link checkout-button" :class="{'is-loading': submitted}" @click="checkout" v-if="cartTotal">Checkout</button>
          <div class="box has-text-centered" v-if="!cartTotal">
            <article class="emptyCart">
              <h1 class="title">Cart is empty</h1>
            </article>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
import cartControl from "@/components/cart-control";
import axios from "axios";
export default {
  components: {
    cartControl
  },
  data() {
    return {
      success: false,
      submitted: false,
      totalCost: 0
    };
  },
  computed: {
    cart() {
      return this.$store.getters.cart;
    },
    cartTotal() {
      return this.$store.state.cartTotal;
    }
  },
  watch: {
    cartTotal: {
      immediate: true,
      handler() {
        this.totalCost = Object.values(this.cart)
          .reduce((sum, el) => sum + el.count * el.metadata.price, 0)
          .toFixed(2);
      }
    }
  },
  methods: {
    removeItem(item) {
      this.$store.commit("removeItem", item);
      this.$toast.open({
        message: "Removed item from cart",
        type: "is-danger"
      });
    },
    checkout() {
      let amount = this.totalCost * 100;
      let items = Object.keys(this.cart).map((key, index) => {
        return {
          id: this.cart[key]._id,
          title: this.cart[key].title,
          count: this.cart[key].count,
          slug: this.cart[key].slug
        };
      });
      console.log(items);
      this.$checkout.open({
        amount: amount,
        token: token => {
          axios
            .post(
              "/api/charge",
              {
                token: token.id,
                email: token.email,
                amount: amount,
                items: items
              },
              {
                headers: {
                  "Content-Type": "application/json"
                }
              }
            )
            .then(res => {
              console.log("charge", res);
              this.success = true;
              this.$toast.open({
                message: "Order placed successfully",
                type: "is-success"
              });
              this.$store.commit("clearCart");
            })
            .catch(err => {
              this.submitted = false;
              console.log(err);
            });
        }
      });
    }
  }
};
</script>

<style scoped>
.box-item {
  padding: 0;
  margin-bottom: 1.25rem;
}
.box,
.back-to-shopping {
  box-shadow: none;
  margin: 0 auto;
}
.back-to-shopping {
  display: block;
  margin-bottom: 10px;
}
.media-left {
  border: 1px solid #dae2db;
  border-radius: 4px;
}
.cart-image img {
  height: 100px;
  width: auto;
  margin: 0 auto;
}
.cart-image {
  display: flex;
  align-items: center;
}
.cart-item {
  margin: 0 auto;
  align-items: center;
}
.checkout-button {
  margin: 0 auto;
  display: block;
}
button {
  margin: 0 auto;
}
.media-content {
  display: flex;
  align-items: center;
}
.media-content .content:first-child {
  width: 40%;
}
@media screen and (max-width: 500px) {
  .media-left {
    display: none;
  }
}
</style>
