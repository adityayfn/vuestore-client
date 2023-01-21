<template>
  <div>
    <div id="page-wrap">
      <h1>Shopping Cart</h1>
      <ItemCart
        v-for="item in cartItems"
        :key="item.id"
        :item="item"
        v-on:remove-item="removeFromCart($event)"
      />
      <h3 id="total-price">Total Rp{{ totalPrice }}</h3>
      <button id="checkout-button">Checkout</button>
    </div>
  </div>
</template>
<script>
import axios from "axios"
import ItemCart from "../../components/ItemCart.vue"
export default {
  data() {
    return {
      cartItems: [],
    }
  },
  components: {
    ItemCart,
  },
  computed: {
    totalPrice() {
      return this.cartItems.reduce((sum, item) => sum + Number(item.price), 0)
    },
  },
  methods: {
    async removeFromCart(product) {
      await axios.delete(
        `http://localhost:8000/api/orders/delete/user/1/product/${product}`
      )
      let indexCart = this.cartItems
        .map(function (item) {
          return item.code
        })
        .indexOf(product)
      this.cartItems.splice(indexCart, 1)
    },
  },
  async created() {
    const res = await axios.get("http://localhost:8000/api/orders/user/1")
    this.cartItems = res.data[0].products
  },
}
</script>
<style scoped>
h1 {
  border-bottom: 1px solid #41b883;
  margin: 0;
  margin-top: 16px;
  padding: 16px;
}
#total-price {
  padding: 16px;
  text-align: right;
}
#checkout-button {
  width: 100%;
}
</style>
