<template>
  <div id="app">
    <div class="items">
      <h1 class="title-list">List of Items</h1>
      <ItemList @add-to-cart="addToCart"></ItemList>
    </div>

    <div class="carts">
      <h1>Shopping Cart</h1>
      <ShoppingCart :cart="cart" @update-quantity="updateQuantity" @remove-from-cart="removeFromCart"></ShoppingCart>
    </div>
  </div>
</template>

<script>
import ItemList from '@/components/ItemList.vue';
import ShoppingCart from '@/components/ShoppingCart.vue';

export default {
  components: {
    ItemList,
    ShoppingCart
  },
  data() {
    return {
      cart: []
    };
  },
  methods: {
    addToCart(item) {
      const existingItemIndex = this.cart.findIndex(i => i.name === item.name);
      if (existingItemIndex !== -1) {
        this.cart[existingItemIndex].quantity++;
      } else {
        this.cart.push({ ...item, quantity: 1 });
      }
    },
    updateQuantity({ index, quantity }) {
      const item = this.cart[index];
      item.quantity = quantity;
      if (quantity <= 0) {
        this.removeFromCart(index);
      }
    },
    removeFromCart(index) {
      this.cart.splice(index, 1);
    }
  }
};
</script>

<style scoped>
  #app {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
  }

  .items,
  .carts {
    padding: 20px;
    box-sizing: border-box;
  }

  .items {
    flex: 1; 
  }

  .carts {
    flex: 2; 
    max-width: 500px;
    border: 2px solid #000;
    border-radius: 5px;
  }

  .title-list {
    text-align: left;
  }

</style>
