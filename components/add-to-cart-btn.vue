<template>
    <button class="button is-link is-outlined is-rounded is-block " @click="addToCart(product)">
        ${{product.metadata.price}} - Add to Cart
    </button>
</template>

<script>
export default {
  name: "add-to-cart-btn",
  props: ["product"],
  data() {
    return {
      modalActive: false
    };
  },
  methods: {
    addToCart(item) {
      this.$emit("addToCart", item);
    }
  }
};
</script>

<style scoped>

</style>
