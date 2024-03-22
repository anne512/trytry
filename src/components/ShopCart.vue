<template>
    <div class="container">
      <!-- Product Menu -->
      <div class="menu">
        <h2>Coffee Menu List</h2>
        <div class="product-list">
          <div class="row" v-for="(row, index) in menuRows" :key="index">
            <div v-for="product in row" :key="product.id" class="product card">
              <div class="card-body">
                <div class="product-info">
                  <p>{{ product.name }} - ₱{{ product.price }}</p>
                  <hr class="break-line">
                  <button @click="addToCart(product)" class="add-to-cart-button">Add to Cart</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Shopping Cart -->
      <div class="cart">
        <h2>Your Cart</h2>
        <div v-if="cart.length === 0" class="empty-cart-message">Your Cart is Empty!</div>
        <div v-else>
          <div v-for="(item, index) in cart" :key="index" class="cart-item">
            <div class="cart-info">
              <p>{{ item.product.name }} - ₱{{ item.product.price }}</p>
              <div class="quantity-controls">
                <button @click="decrementQuantity(index)" class="quantity-button">-</button>
                <input type="number" v-model="item.quantity" min="1" class="quantity-input" @input="updateQuantity(index)">
                <button @click="incrementQuantity(index)" class="quantity-button">+</button>
              </div>
              <button class="remove-button" @click="removeFromCart(index)">Remove</button>
            </div>
          </div>
          <p>Total: <span class="total-price">₱{{ totalPrice }}</span></p>
        </div>
      </div>
    </div>
  </template>
  
  
  <script>
  export default {
    data() {
      return {
        products: [
          { id: 1, name: 'Black Coffee', price: 39 },
          { id: 2, name: 'Espresso', price: 49 },
          { id: 3, name: 'Iced Latte', price: 59 },
          { id: 4, name: 'Americano', price: 69 },
          { id: 5, name: 'Cappuccino', price: 79 },
          { id: 6, name: 'Mocha', price: 89 },
          { id: 7, name: 'Caramel Macchiato', price: 99 },
          { id: 8, name: 'Matcha', price: 109 },
          { id: 9, name: 'Cold Brew', price: 119 },
          { id: 10, name: 'Frappe', price: 129 },
        ],
        cart: [],
      };
    },
    computed: {
      totalPrice() {
        return this.cart.reduce((total, item) => {
          return total + (item.product.price * item.quantity);
        }, 0);
      },
      menuRows() {
        const rows = [];
        for (let i = 0; i < this.products.length; i += 3) {
          rows.push(this.products.slice(i, i + 3));
        }
        return rows;
      }
    },
    methods: {
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
      updateQuantity(index) {
        const quantity = parseInt(this.cart[index].quantity);
        if (quantity <= 0) {
          this.cart.splice(index, 1);
        }
      },
      incrementQuantity(index) {
        this.cart[index].quantity++;
      },
      decrementQuantity(index) {
        if (this.cart[index].quantity > 1) {
          this.cart[index].quantity--;
        }
      }
    }
  };
  </script>
  
  <style>
  .container {
    display: flex;
  }
  
  .menu {
    flex: 1;
    margin-right: 20px;
  }
  
  .cart {
    flex: 1;
    margin-left: 20px;
  }
  
  .row {
    display: flex;
  }
  
  .product {
    flex: 0 0 calc(33.33% - 20px);
    margin: 10px;
  }
  
  .card {
    border: 1px solid black;
    border-radius: 5px;
  }
  
  .card-body {
    padding: 10px;
  }
  
  .product-info {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .add-to-cart-button {
    background-color: green;
    color: white;
    border: none;
    padding: 8px 16px;
    cursor: pointer;
  }
  
  .add-to-cart-button:hover {
    background-color: darkgreen;
  }
  
  .cart-item {
    margin-bottom: 10px;
  }
  
  .cart-info {
    display: flex;
    align-items: center;
    margin-right: 10px; /* Add margin-right to create space */
  }
  
  .quantity-controls {
    display: flex;
    align-items: center;
    margin-left: 10px; /* Add margin-left to create space */
  }
  
  .quantity-button {
    color: black;
    border: 1px solid LightGray;
    border-radius: 5px;
    padding: 5px 10px;
    cursor: pointer;
    background-color: White;
  }
  
  .quantity-input {
    width: 50px;
    text-align: center;
    border: 1px solid LightGray; /* Represent #ccc in CSS color name */
    border-radius: 5px; /* Add border-radius for rounded corners */
    padding: 5px;
    box-sizing: border-box; /* Ensure padding and border are included in the width */
  }
  
  .quantity-input::-webkit-inner-spin-button,
  .quantity-input::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  
  .quantity-input::-moz-outer-spinbutton {
    appearance: none;
    margin: 0;
  }
  
  .remove-button {
    background-color: red;
    color: white;
    border: none;
    padding: 8px 16px;
    cursor: pointer;
    margin-left: 10px; /* Add margin-left to create space */
  }
  
  .remove-button:hover {
    background-color: darkred;
  }
  
  .empty-cart-message {
    color: red;
  }
  
  .total-price {
    font-weight: bold;
  }
  </style>
  