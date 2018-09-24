<template>
  <div>
    <div class="header">
      <h1>Poster Shop</h1>
    </div>
    <div class="main">
      <div class="products">
        <p>Products go here.</p>
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
    total: 0
  }),
  methods: {
    addItem() {
      this.total += 9.99
    }
  }
}
</script>
