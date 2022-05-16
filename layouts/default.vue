<template>
  <div>
    <nav class="navbar is-white has-shadow">
      <div class="container">
        <div class="navbar-brand">
          <router-link to="/" class="navbar-item is-size-3 logo has-text-dark">
            Vue<b>Commerce</b>
          </router-link>
        </div>
        <div class="navbar-menu is-active">
          <div class="navbar-end">
            <div class="navbar-item" v-if="cartTotal">
              <nuxt-link :to="{name: 'cart'}"> Cart ({{ cartTotal }})</nuxt-link>
            </div>
          </div>
        </div>
      </div>
    </nav>
    <transition>
    <nuxt/>
     </transition>
    <footer class="footer">
      <div class="container">
        <div class="content has-text-centered">
          Proudly Powered By <strong>VueCommerce</strong>
        </div>
      </div>
    </footer>
  </div>
</template>
<script>
export default {
  computed: {
    cartTotal() {
      return this.$store.state.cartTotal;
    }
  }
};
</script>

<style>
body {
  background: #dae2db;
}
</style>
