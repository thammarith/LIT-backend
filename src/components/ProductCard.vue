<template>
  <Card
    class="product-card"
    :style="{ backgroundImage: `url(${product.prd_image})` }"
  >
    <div class="card-details">
      <h1>{{ product.prd_name }}</h1>
      <div class="card-subtitle">
        <h2>Price: {{ product.prd_price }}</h2>
        <div class="card-delete" @click="deleteProduct">Delete</div>
      </div>
    </div>
  </Card>
</template>

<script>
import axios from "axios";
import Card from "./Card";
import { apiUrl } from "../config";

export default {
  components: {
    Card
  },
  props: {
    product: Object
  },
  methods: {
    deleteProduct() {
      const prd_id = this.product._id;

      axios
        .delete(`${apiUrl}/products/${prd_id}`)
        .then(() => {
          this.$toasted.show("Product was successfully deleted", {
            theme: "toasted-primary",
            position: "top-right",
            duration: 5000
          });
          this.$emit("update-required");
        })
        .catch(err => {
          this.$toasted.show(
            "There was a problem deleting the product. Check console for more information",
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
.product-card {
  background-position: 50% 50%;
  background-size: cover;
  position: relative;
}

.card-details {
  background: #fafafa;
  bottom: 0;
  padding: 1rem;
  position: absolute;
  width: 100%;
}

.card-delete {
  color: #f00;
  font-weight: 700;

  &:hover {
    color: #c00;
    cursor: pointer;
  }
}

.card-subtitle {
  display: flex;
  font-size: 0.75rem;
  justify-content: space-between;
  margin: 0.5rem 0 0;
}

h1 {
  font-size: 1rem;
  margin: 0;
}

h2 {
  margin: 0;
  font-size: 0.75rem;
}
</style>
