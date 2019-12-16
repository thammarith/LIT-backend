<template>
  <div class="order-card-container">
    <div
      v-if="orderedProduct"
      :style="{ backgroundImage: `url(${orderedProduct.prd_image})` }"
      class="order-image"
    ></div>
    <div class="order-details">
      <h2 v-if="orderedProduct">
        {{ orderedProduct.prd_name }} ({{ orderedProduct.prd_price }})
      </h2>
      <h3 v-if="order.ord_served">Served at {{ order.ord_served_at }}</h3>
      <h3 v-else>Ordered at {{ order.ord_at }}</h3>
    </div>
    <div
      class="order-mark-as-served"
      v-if="!order.ord_served"
      @click="markAsServed"
    >
      MARK AS SERVED
    </div>
    <div class="order-served" v-else>Served</div>
  </div>
</template>

<script>
import axios from "axios";
import { apiUrl } from "../config";

export default {
  props: {
    order: Object,
    products: Array
  },
  computed: {
    orderedProduct() {
      return this.products.find(p => p._id === this.order.prd_id);
    }
  },
  methods: {
    markAsServed() {
      const order = {
        ...this.order,
        ord_served: true,
        ord_served_at: new Date().toISOString()
      };

      axios
        .put(`${apiUrl}/orders/${this.order._id}`, order)
        .then(() => {
          this.$toasted.show("Marked order as served", {
            theme: "toasted-primary",
            position: "top-right",
            duration: 5000
          });
          this.$emit("order-served");
        })
        .catch(err => {
          this.$toasted.show(
            "Cannot mark order as served. Check console for more information",
            {
              theme: "toasted-primary",
              position: "top-right",
              duration: 5000
            }
          );
          console.error(err);
        });
    }
  }
};
</script>

<style lang="scss" scoped>
.order-image {
  background-size: cover;
  background-position: 50% 50%;
  height: 4rem;
  width: 4rem;
}

.order-card-container {
  display: flex;
  margin-top: 1rem;
}

.order-details {
  margin-left: 1rem;
}

.order-mark-as-served {
  align-items: center;
  background: #0a0;
  color: #fff;
  display: flex;
  font-weight: 700;
  height: 4rem;
  margin-left: auto;
  padding: 0 1rem;

  &:hover {
    background: #080;
    cursor: pointer;
  }
}

.order-served {
  align-items: center;
  color: #0a0;
  display: flex;
  font-weight: 700;
  height: 4rem;
  margin-left: auto;
  padding: 0 1rem;
}

h2,
h3 {
  margin: 0;
}

h3 {
  font-size: 1rem;
  font-weight: 400;
  margin-top: 1rem;
}
</style>
