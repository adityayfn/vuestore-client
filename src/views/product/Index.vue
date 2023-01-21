<template>
  <div>
    <div id="page-wrap">
      <div class="grid-wrap">
        <ProductItem
          v-for="product in products"
          :key="product.id"
          :product="product"
        />
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios"
import ProductItem from "../../components/ProductItem.vue"
export default {
  data() {
    return {
      products: [],
    }
  },
  components: {
    ProductItem,
  },
  async created() {
    const res = await axios.get("http://localhost:8000/api/products")
    this.products = res.data
  },
}
</script>
<style scoped>
.grid-wrap {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-top: 16px;
}
</style>
