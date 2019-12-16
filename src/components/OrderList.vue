<template>
  <div id="orders-container">
    <div class="orders-list">
      <h2>New orders to serve</h2>
      <OrderCard
        v-for="order of unservedOrders"
        :key="order._id"
        :order="order"
        :products="products"
        @order-served="loadOrders"
      />
      <div v-if="!unservedOrders.length">No new orders</div>
    </div>
    <div class="orders-list">
      <h2>Served orders</h2>
      <OrderCard
        v-for="order of servedOrders"
        :key="order._id"
        :order="order"
        :products="products"
        @order-served="loadOrders"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { apiUrl } from "../config";
import OrderCard from "./OrderCard";

export default {
  components: {
    OrderCard
  },
  props: {
    products: {
      type: Array,
      default: () => []
    }
  },
  computed: {
    servedOrders() {
      return this.orders.filter(o => o.ord_served).reverse();
    },
    unservedOrders() {
      return this.orders.filter(o => !o.ord_served);
    }
  },
  data() {
    return {
      orders: []
    };
  },
  methods: {
    keepLoadingOrders() {
      setTimeout(this.loadOrders, 5000);
    },
    loadOrders() {
      axios
        .get(`${apiUrl}/orders`)
        .then(res => (this.orders = res.data))
        .catch(err => {
          this.$toasted.show(
            "Cannot load orders. Check console for more information",
            {
              theme: "toasted-primary",
              position: "top-right",
              duration: 5000
            }
          );
          console.error(err);
        })
        // Keeps loading orders even if the previous attempts fail
        .finally(this.keepLoadingOrders);
    }
  },
  mounted() {
    this.loadOrders();
  }
};
</script>

<style lang="scss" scoped>
#orders-container {
  display: flex;
}

.orders-list {
  padding: 2rem;
  width: 100%;
}
</style>
