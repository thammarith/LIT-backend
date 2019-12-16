<template>
  <Card @click.native="addNewProduct" class="card">
    +
  </Card>
</template>

<script>
import Card from "./Card";

export default {
  components: {
    Card
  },
  methods: {
    addNewProduct() {
      this.$emit("add-new-product");
    }
  }
};
</script>

<style lang="scss" scoped>
.card {
  align-items: center;
  border: 0.5rem dashed #ccc;
  color: #ccc;
  display: flex;
  font-size: 6rem;
  font-weight: 500;
  justify-content: center;

  &:hover {
    border: 0.5rem dashed #0c0;
    color: #0c0;
    cursor: pointer;
  }
}
</style>
