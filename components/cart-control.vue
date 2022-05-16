<template>
<div class="cart-control">
  <button class="button is-link" :aria-disabled="!cartInput" @click="decreaseCart()"> - </button>
  <div class="control cart-input">
  <input class="input" type="number" min="1" placeholder="0" :value="cartInput" readonly>
  </div>
  <button class="button is-link"  @click="increaseCart()"> +  </button>
</div>
</template>

<script>
export default {
  name: "cart-control",
  props: {
    cartCount: {
      type: Number,
      default: 0
    },
    product: {
      type: Object
    }
  },
  data() {
    return {
      cartInput: this.cartCount
    };
  },
  computed: {
    currentProduct() {
      return this.product;
    }
  },
  methods: {
    increaseCart() {
      this.cartInput++;
      this.$store.commit("addToCart", this.currentProduct);
      this.$toast.open({
        message: "Added to cart",
        type: "is-success"
      });
    },
    decreaseCart() {
      --this.cartInput;
      this.$store.commit("removeFromCart", this.currentProduct);
      this.$toast.open({
        message: "Removed from cart",
        type: "is-danger"
      });
    }
  }
};
</script>

<style scoped>
.cart-control {
  margin: 12px auto;
  display: flex;
}
.cart-input {
  max-width: 100px;
}
.cart-input input.input {
  border: none;
  text-align: center;
}
.cart-input input.input,
button {
  border-radius: 0;
}
button {
  width: 34px;
}
</style>
