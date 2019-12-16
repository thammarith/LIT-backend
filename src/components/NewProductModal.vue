<template>
  <div id="new-product-modal-container">
    <div id="new-product-modal">
      <h1>Enter product details</h1>

      <form
        @submit.prevent="submit"
        method="post"
        enctype="multipart/form-data"
      >
        <label for="prd_name">Product name:</label>
        <input type="text" name="prd_name" v-model="prd_name" required />

        <label for="prd_price">Product price:</label>
        <input type="text" name="prd_price" v-model="prd_price" required />

        <label for="prd_image">Product image:</label>
        <input type="file" name="prd_image" ref="prd_image" required />

        <div id="button-container">
          <button class="button success" type="submit">Submit</button>
          <div class="button danger" @click="closeModal">Cancel</div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { apiUrl } from "../config";

export default {
  computed: {
    prd_image() {
      return this.$refs.prd_image.files[0];
    }
  },
  data() {
    return {
      prd_name: "",
      prd_price: undefined
    };
  },
  methods: {
    closeModal() {
      this.$emit("close-modal");
    },
    submit() {
      const formData = new FormData();

      formData.append("prd_image", this.prd_image);
      formData.append("prd_name", this.prd_name);
      formData.append("prd_price", this.prd_price);

      axios
        .post(`${apiUrl}/products`, formData)
        .then(() => {
          this.$toasted.show("The product was successfully added", {
            theme: "toasted-primary",
            position: "top-right",
            duration: 5000
          });
          this.$emit("product-created");
          this.closeModal();
        })
        .catch(err => {
          this.$toasted.show(
            "There was a problem adding a new product. Check console for more information",
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
#button-container {
  display: flex;
}

#new-product-modal {
  background: #fafafa;
  padding: 2rem;
  overflow: auto;

  &-container {
    align-items: center;
    background: #000b;
    display: flex;
    height: 100vh;
    justify-content: center;
    left: 0;
    position: fixed;
    top: 0;
    width: 100vw;
    z-index: 1000;
  }
}

h1 {
  margin: 0 0 3rem;
}

label {
  display: block;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

input {
  height: 3rem;
  margin-bottom: 2rem;
  width: 24rem;

  &[type="text"] {
    border: 1px solid #ddd;
    color: #222;
    font-size: 1.25rem;
    padding: 0 0.75rem;
  }
}

.button {
  border: none;
  display: block;
  font-weight: 700;
  font-size: 1.25rem;
  padding: 0.75rem 1.5rem;

  &.danger {
    color: #d00;

    &:hover {
      color: #a00;
    }
  }

  &.success {
    background: #090;
    color: #fff;

    &:hover {
      background: #060;
    }
  }

  &:hover {
    cursor: pointer;
  }
}
</style>
