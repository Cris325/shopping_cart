<template>
  <div class="btn-add-group">
    <button @click="addItem" class="btn-add">Add New Item</button>
  </div>

  <div class="list">
    <div v-for="(item, index) in items" :key="index" class="items">
      <div class="details">
        <p class="name"><span>Name:</span> {{ item.name }}</p>
        <p class="price"><span>Quantity:</span> ₱{{ item.price }}</p>
      </div>
      <div class="btn-group">
        <button @click="addToCart(item)" class="btn-add-to-cart">Add to Cart</button>
        <button @click="updateItem(index)" class="btn-update">Update</button>
        <button @click="removeItem(index)" class="btn-remove">Remove</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { name: 'Office Slacks', price: 229 },
        { name: 'Mini Fan', price: 220 },
        { name: 'Wireless Mouse', price: 135 },
      ]
    };
  },
  methods: {
    addToCart(item) {
      this.$emit('add-to-cart', item);
    },
    addItem() {
      let name = prompt('Enter the name for the new item:');
      if (name === null || name.trim() === '') {
        alert('Please enter a non-empty name!');
        return;
      }

      let priceInput = prompt('Enter the price for the new item:');
      if (priceInput === null || priceInput.trim() === '' || isNaN(parseFloat(priceInput)) || parseFloat(priceInput) <= 0) {
        alert('Please enter a valid number for the price!');
        return;
      }
      
      const price = parseFloat(priceInput);

      this.items.push({ name, price });
    },
    updateItem(index) {
      const item = this.items[index];
      let newName = prompt('Enter the new name:', item.name);
      if (newName === null || newName.trim() === '') {
        alert('Please enter a non-empty name!');
        return;
      }

      let newPriceInput = prompt('Enter the new price:', item.price);
      if (newPriceInput === null || newPriceInput.trim() === '' || isNaN(parseFloat(newPriceInput)) || parseFloat(newPriceInput) <= 0) {
        alert('Please enter a valid number for the price!');
        return;
      }
      
      const newPrice = parseFloat(newPriceInput);

      item.name = newName;
      item.price = newPrice;
    },
    removeItem(index) {
      this.items.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.btn-add-group {
    text-align: right;
    margin-bottom: 20px;
}

.btn-add {
    background-color: #3333FF;
    color: #fff;
    border: 1px solid #3333FF;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
}

.btn-add:hover {
    background-color: #0056b3;
    border-color: #0056b3;
}

.btn-add-to-cart {
  padding: 5px 5px;
  cursor: pointer;
  background-color: #99FF00;
  border: 1px solid #000000;
}

.btn-add-to-cart:hover {
  background-color: #06b300;
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

.list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); 
  gap: 20px;
}

.items {
  border: 1px solid #000;
  border-radius: 20px;
  padding: 10px;
}

span {
  font-weight: bold;
}
</style>
