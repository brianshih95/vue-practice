<template>
  <header class="top-bar spread">
    <nav class="top-bar-nav">
      <router-link to="/" class="top-bar-link">
        <i class="icofont-spoon-and-fork"></i>
        <span>Home</span>
      </router-link>
      <router-link to="/products" class="top-bar-link">
        <span>Products</span>
      </router-link>
    </nav>
    <div @click="toggleSidebar" class="top-bar-cart-link">
      <i class="icofont-cart-alt icofont-1x"></i>
      <span>Cart ({{ totalQuantity }})</span>
    </div>
  </header>
  <router-view :inventory="inventory" :addToCart="addToCart"/>

  <Sidebar
    v-if="showSidebar"
    :toggle="toggleSidebar"
    :cart="cart"
    :inventory="inventory"
    :remove="removeItem"
  />
</template>

<script>
import food from './data/food.json'
import Sidebar from './components/Sidebar.vue'

export default {
  components: {
    Sidebar
  },

  data () {
    return {
      inventory: food,
      cart: {},
      showSidebar: false
    }
  },

  computed: {
    totalQuantity () {
      return Object.values(this.cart).reduce((acc, curr) => acc + curr, 0)
    }
  },
  methods: {
    addToCart (name, quantity) {
      if (!quantity) {
        return
      }
      if (!this.cart[name]) {
        this.cart[name] = 0
      }
      this.cart[name] += quantity
    },

    toggleSidebar () {
      this.showSidebar = !this.showSidebar
    },
    removeItem (name) {
      delete this.cart[name]
    }
  }
}
</script>
