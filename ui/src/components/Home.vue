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
            <span class="item-qty">{{item.quantity}}</span>
          </li>
        </ul>
        <div v-if="cart.length > 0">
          <div>Total: $ {{total}}</div>
        </div>
        <div v-else class="empty-cart">
          <div>There are currently no items in the cart</div>
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

      let cartItem = this.cart.filter(
        item => item.id === this.items[index]['id']
      )

      if (cartItem.length === 1) {
        const cartItemIndex = this.cart.indexOf(cartItem[0])
        this.cart[cartItemIndex].quantity++
      } else {
        this.cart.push({
          ...this.items[index],
          quantity: 1
        })
      }
    }
  }
}
</script>
