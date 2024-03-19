<template>
  <div class="cart">
    <!-- Cart items -->
    <div v-if="cart.length === 0">
      <p>Your cart is empty</p>
    </div>
    <ul v-else>
      <li v-for="(item, index) in cart" :key="index" class="cart-item">
        <div class="details">
          <p class="name">Name: {{ item.name }}</p>
          <p class="price">Price: ₱{{ item.price }}</p>
          <p class="quantity">Quantity: {{ item.quantity }}</p>
        </div>
        <div class="btn-group">
          <button @click="updateFromCart(index)" class="btn-update">Update</button>
          <button @click="removeFromCart(index)" class="btn-remove">Remove</button>
        </div>
      </li>
    </ul>

    <p class="total">Total: ₱ {{ total }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedItemIndex: null,
    };
  },
  props: {
    cart: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    total() {
      if (this.cart.length === 0) {
        return 0;
      }
      return this.cart.reduce((totalCost, item) => totalCost + item.price * item.quantity, 0);
    },
  },
  methods: {
    updateFromCart(index) {
      let newQuantity = prompt('Modify quantity for ' + this.cart[index].name + ':', this.cart[index].quantity);
      
      while (newQuantity !== null && newQuantity.trim() === '') {
        alert('Please enter quantity for : ' + this.cart[index].name );
        this.updateFromCart(index); 
        return;
      }
      
      if (newQuantity !== null) {
        const parsedQuantity = parseInt(newQuantity);
        if (!isNaN(parsedQuantity) && parsedQuantity >= 0) {
          this.$emit('update-quantity', { index, quantity: parsedQuantity });
        } else {
          alert('Please do no input negative integer!');
          this.updateFromCart(index);
        }
      }
    },
    removeFromCart(index) {
      this.$emit('remove-from-cart', index);
    },
  },
};
</script>

<style scoped>
.cart {
  margin-top: 20px;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

.details {
  text-align: left;
  margin: 0;
}

.details p {
  margin: 0;
  margin-bottom: 5px;
  font-weight: bold;
}

button {
  margin-left: 10px;
}

.btn-update {
  color: #000;
  padding: 5px 5px;
  cursor: pointer;
  background-color: #FFCC00;
  border: 1px solid #000000;
}

.btn-update:hover {
  background-color: #aab300;
}

.btn-remove {
  color: #fff;
  padding: 5px 5px;
  cursor: pointer;
  background-color: #CC0000;
  border: 1px solid #000000;
}

.btn-remove:hover {
  background-color: #b30000;
}

.total {
  font-size: 18px;
  font-weight: bold;
  margin-top: 10px;
}

</style>
