<template>
  <div>
    <h2>Cart</h2>
    <table>
      <tr>
        <th>Id</th>
        <th>Name</th>
        <th>Quantity</th>
        <th>Price</th>
        <th>Total</th>
        <th></th>
      </tr>
      <tr v-for="article in cart" :key="article.id">
        <td>{{article.id}}</td>
        <td>{{article.name}}</td>
        <td>{{article.quantity}}</td>
        <td>{{article.price}}</td>
        <td>{{article.price * article.quantity}}</td>
        <td><button @click="handleRemoveCartItem(article)">Remove</button></td>
      </tr>
    </table>
    <h3>Total: {{total}}</h3>
  </div>
</template>

<script>
import { mapState, mapGetters, mapMutations } from "vuex";

export default {
  sockets: {
    connect() {
      console.log("connected: ", this.$socket.id);
    },
    "cart:removeItem"(item) {
      console.log("removeItem", item);
      this.removeCartItem(item);
    }
  },
  computed: {
    ...mapState("cart", ["cart"]),
    ...mapGetters("cart", ["total"])
  },
  methods: {
    ...mapMutations("cart", ["removeCartItem"]),
    handleRemoveCartItem(item) {
      this.$socket.emit("cart:removeItem", item);
      this.removeCartItem(item);
    }
  }
};
</script>
