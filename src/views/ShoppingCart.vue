<template>
  <div class="container">
    <div class="product-list">
      <div v-for="product in products" :key="product.id" class="product">
        <img :src="product.image">
        <p class="product-name">{{ product.name }}</p>
        <p class="product-price">₱{{ product.price }}</p>
        <button @click="addToCart(product)" class="add-to-cart-btn">Add to Cart</button>
      </div>
    </div>

    <div class="cart">
      <h2>Shopping Cart</h2>
        <div v-if="cart.length === 0" class="empty-cart">Cart is empty</div>
        <div v-else>
          <div v-for="(item, index) in cart" :key="index" class="cart-item">
            <img :src="item.product.image">
            <div class="item-details">
              <p class="item-name">{{ item.product.name }}</p>
              <p class="item-price">₱{{ item.product.price }}</p>
              <div class="quantity">
                <button @click="updateQuantity(index, -1)" class="quantity-btn">-</button>
                <input type="number" v-model="item.quantity" min="1" class="quantity-input">
                <button @click="updateQuantity(index, 1)" class="quantity-btn">+</button>
              </div>
              <button @click="removeFromCart(index)" class="remove-btn">Remove</button>
            </div>
          </div>
          <p class="total-price">Total: ₱{{ totalPrice }}</p>
          <button @click="checkout" class="checkout-btn">Checkout</button>
        </div>
      </div>
    </div>

    <div class="logout">
      <button @click="handleLogout" class="logout-btn">Logout</button>
    </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'A4 size', price: 12, image: require('@/assets/bondpaper.jpg'), available: true },
        { id: 2, name: 'Short size', price: 15, image: require('@/assets/bondpaper1.jpg'), available: true },
        { id: 3, name: 'Long size', price: 20, image: require('@/assets/bondpaper2.jpg'), available: true }
      ],      
      cart: [],
      isLoggedIn: false
    };
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((total, item) => {
        return total + (item.product.price * item.quantity);
      }, 0);
    },
    productAvailable() {
      return this.products.some(product => product.available);
    }
  },
  methods: {
    handleLogout() {
      localStorage.removeItem('token');
      this.isLoggedIn = false;
      this.$router.push({ name: 'login' });
    },
    addToCart(product) {
      const found = this.cart.find(item => item.product.id === product.id);
      if (found) {
        found.quantity++;
      } else {
        this.cart.push({ product: product, quantity: 1 });
      }
    },
    removeFromCart(index) {
      this.cart.splice(index, 1);
    },
    updateQuantity(index, change) {
      const quantity = parseInt(this.cart[index].quantity);
      const newQuantity = quantity + change;
      if (newQuantity > 0) {
        this.cart[index].quantity = newQuantity;
      } else {
        this.removeFromCart(index);
      }
    },
    checkout() {
      alert('Checkout successful!');
      this.cart = [];
    }
  }
};
</script>



<style>
.container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-wrap: wrap;
}

.product-list {
  display: flex;
  flex-wrap: wrap;
  margin-right: -10px;
  margin-bottom: -10px;
}

.product {
  margin: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #fff;
  text-align: center;
  max-width: 200px;
}

.product img {
  max-width: 100%;
}

.product-name {
  margin-top: 10px;
  font-weight: bold;
}

.product-price {
  font-size: 18px;
  color: #f5587b;
}

.logout {
  position: fixed;
  bottom: 20px;
  right: 20px;
}

.logout-btn {
  background-color: #f5587b;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 8px 16px;
  cursor: pointer;
}

.add-to-cart-btn {
  margin-top: 10px;
  background-color: #f5587b;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 8px 16px;
  cursor: pointer;
}

.cart {
  margin-top: 20px;
  width: 100%;
}

.cart h2 {
  text-align: center;
}

.empty-cart {
  text-align: center;
  color: #888;
}

.cart-item {
display: center;
justify-content: center;
align-items: center;
margin-bottom: 20px;
text-align: center;
}

.cart-item img {
max-width: 10%;
margin-right: 20px;
align-items: center;
}

.item-details {
  flex-grow: 1;
}

.item-name {
  font-weight: bold;
}

.item-price {
  color: #f5587b;
}

.quantity {
  margin-top: 10px;
}

.quantity-btn {
  background-color: #f5587b;
  color: #fff;
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  cursor: pointer;
  margin: 0 5px;
}

.quantity-input {
  width: 50px;
  text-align: center;
}

.remove-btn {
  background-color: #f5587b;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 8px 16px;
  cursor: pointer;
  margin-top: 10px;
}

.total-price {
  text-align: center;
  font-weight: bold;
  font-size: 20px;
  margin-top: 20px;
  color: #f5587b;
}

.checkout-btn{
  padding: 15px 30px;
    font-size: 20px;
    background-color: #ff6f61; /* Vibrant button color */
    color: #fff; /* Text color */
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.checkout-btn:hover {
    background-color: #e94b3c; /* Darker shade of button color on hover */
}
</style>