<template>
  <div>
    <div class="header">
      <h1>Poster Shop</h1>
    </div>
    <div class="main">
      <div class="products">
        <div v-for="item in items" v-bind:key="item.id">{{item.title}}</div>
        <button v-on:click="addItem">Add Item</button>
      </div>
      <div class="cart">
        <h2>Shopping Cart</h2>
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
    ]
  }),
  methods: {
    addItem() {
      this.total += 9.99
      console.log(this.total)
    }
  }
}
</script>
