<template>
  <div>
    <div class="header">
      <h1>Poster Shop</h1>
    </div>
    <div class="main">
      <div class="products">
        <div class="product" v-for="(item, index) in items" v-bind:key="item.id">
          <h4 class="product-title">{{item.title}}</h4>
          <button class="add-to-cart" v-on:click="addItem(index)">Add to cart</button>
        </div>
      </div>
      <div class="cart">
        <h2>Shopping Cart</h2>
        <ul>
          <li class="cart-item" v-for="item in cart" v-bind:key="item.id">
            <div class="item-title">{{item.title}}</div>
          </li>
        </ul>
        <div>
          <div>Total: $ {{total}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'home',
  data: () => ({
    total: 0,
    items: [
      { id: 1, title: 'Item 1' },
      { id: 2, title: 'Item 2' },
      { id: 3, title: 'Item 3' },
      { id: 4, title: 'Item 4' }
    ],
    cart: []
  }),
  methods: {
    addItem(index) {
      this.total += 9.99
      this.cart.push(this.items[index])
    }
  }
}
</script>
