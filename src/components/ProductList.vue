<template>
  <div>
    <NewProductModal
      v-if="isShowingNewProductModal"
      @close-modal="closeModal"
      @product-created="reloadProducts"
    />

    <div id="products-container">
      <NewProductCard @add-new-product="addNewProduct" />
      <ProductCard
        @update-required="reloadProducts"
        v-for="product of products"
        :key="product._id"
        :product="product"
      />
    </div>
  </div>
</template>

<script>
import NewProductCard from "./NewProductCard";
import NewProductModal from "./NewProductModal";
import ProductCard from "./ProductCard";

export default {
  components: {
    NewProductCard,
    NewProductModal,
    ProductCard
  },
  props: {
    products: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      isShowingNewProductModal: false
    };
  },
  methods: {
    addNewProduct() {
      this.isShowingNewProductModal = true;
    },
    closeModal() {
      this.isShowingNewProductModal = false;
    },
    reloadProducts() {
      this.$emit("reload-products");
    }
  }
};
</script>

<style lang="scss" scoped>
#products-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}
</style>
