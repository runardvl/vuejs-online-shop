<template>
  <div class="v-cart">
    <router-link :to="{ name: 'catalog' }">
      <div class="v-cart__link_to_cart">Back to Catalog</div>
    </router-link>
    <h1>Cart</h1>
    <p v-if="!cart_data.length">There are no products in cart...</p>
    <v-cart-item
      v-for="(item, index) in cart_data"
      :key="item.id"
      :cart_item_data="item"
      @deleteFromCart="deleteFromCart(index)"
      @increment="increment(index)"
      @decrement="decrement(index)"
    />
    <div class="v-cart__total">
      <p class="total__name">Total:</p>
      <p>{{ cartTotal }} ₽</p>
    </div>
  </div>
</template>

<script>
import vCartItem from "@/components/cart/v-cart-item";
import { mapActions } from "vuex";
export default {
  name: "v-cart",
  components: {
    vCartItem
  },
  props: {
    cart_data: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {};
  },
  computed: {
    cartTotal() {
      return this.cart_data.reduce(
        (res, item) => res + item.price * item.quantity,
        0
      );
    }
  },
  methods: {
    ...mapActions([
      "DELETE_FROM_CART",
      "INCREMENT_CART_ITEM",
      "DECREMENT_CART_ITEM"
    ]),
    increment(index) {
      this.INCREMENT_CART_ITEM(index);
    },
    decrement(index) {
      this.DECREMENT_CART_ITEM(index);
    },
    deleteFromCart(index) {
      this.DELETE_FROM_CART(index);
    }
  }
};
</script>

<style lang="scss">
.v-cart {
  margin-bottom: 121px;
  &__link_to_cart {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: $padding * 2 $padding * 3;
    border: solid 1px $green-bg;
  }
  &__total {
    position: fixed;
    bottom: 0;
    right: 0;
    left: 0;
    padding: $padding * 3;
    display: flex;
    justify-content: center;
    background-color: $green-bg;
    color: #fff;
    font-size: 20px;
  }
  .total__name {
    margin-right: $margin * 2;
  }
}
</style>
