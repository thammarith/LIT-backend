<template>
  <div id="app">
    <div id="mode-selector">
      <h1>Mode:</h1>
      <div
        :class="['mode-container', { selected: mode === 'orders' }]"
        @click="selectMode('orders')"
      >
        Orders
      </div>
      <div
        :class="['mode-container', { selected: mode === 'products' }]"
        @click="selectMode('products')"
      >
        Products
      </div>
    </div>

    <OrderList v-if="mode === 'orders'" :products="products" />
    <ProductList
      v-if="mode === 'products'"
      @reload-products="reloadProducts"
      :products="products"
    />
  </div>
</template>

<script>
import axios from "axios";

import OrderList from "./components/OrderList";
import ProductList from "./components/ProductList";
import { apiUrl } from "./config";

export default {
  components: {
    OrderList,
    ProductList
  },
  data() {
    return {
      products: [],
      mode: "orders"
    };
  },
  methods: {
    loadProducts() {
      axios
        .get(`${apiUrl}/products`)
        .then(res => (this.products = res.data))
        .catch(err => {
          this.$toasted.show(
            "There was a problem loading products. Check console for more information",
            {
              theme: "toasted-primary",
              position: "top-right",
              duration: 5000
            }
          );
          console.error(err);
        });
    },
    reloadProducts() {
      this.loadProducts();
    },
    selectMode(mode) {
      this.mode = mode;
    }
  },
  mounted() {
    this.loadProducts();
  }
};
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

html,
body {
  margin: 0;
}

#mode-selector {
  align-items: center;
  display: flex;
  padding: 2rem;

  h1 {
    margin: 0 1rem 0 0;
  }
}

.mode-container {
  align-items: center;
  display: flex;
  font-weight: 700;
  padding: 1rem 2rem;

  &:hover {
    background: #ddd;
    cursor: pointer;
  }

  &.selected {
    background: #0a0;
    color: #fff;

    &:hover {
      background: #080;
    }
  }
}
</style>
