<template>
  <div>
    <div class="header">
      <h1>Poster Shop</h1>
      <form class="searchbar" v-on:submit.prevent="onSubmit">
        <input type="text" placeholder="Search for posters" v-model="search">
        <input type="submit" value="Search" class="btn">
      </form>
    </div>
    <div class="main">
      <div class="products">
        <div class="search-results" v-if="searchResults.total != '' && searchResults.term != '' && !loading">
          Found {{searchResults.total}} results for search term: {{searchResults.term}}.
        </div>
        <div class="search-results" v-if="loading">
          Searching Please Wait...
        </div>
        <div class="product" v-for="(item, index) in items" v-bind:key="item.id">
          <div>
            <div class="product-image">
              <img v-bind:src="item.link" alt="">
            </div>
            </div>
            <div>
              <h4 class="product-title">{{item.title}}</h4>
              <p>Price:
                <strong>{{price | currency}}</strong>
              </p>
              <button class="add-to-cart btn" v-on:click="addItem(index)">Add to cart</button>
            </div>
          </div>
        </div>
        <div class="cart">
          <h2>Shopping Cart</h2>
          <transition-group name="fade" tag="ul">
            <li class="cart-item" v-for="(item, index) in cart" v-bind:key="item.id">
              <div class="item-title">{{item.title}}</div>
              <span class="item-qty">{{item.price | currency}} x {{item.quantity}}</span>
              <button class="btn" v-on:click="addQuantity(index)">+</button>
              <button class="btn" v-on:click="removeQuantity(index)">-</button>
            </li>
          </transition-group>
          <transition name="fade">
            <div v-if="cart.length > 0">
              <div>Total: {{total | currency}}</div>
            </div>
          </transition>
          <div v-if="cart.length ===0" class="empty-cart">
            <div>There are currently no items in the cart</div>
          </div>
        </div>
      </div>
      <div id="product-list-bottom"></div>
    </div>
</template>

<script>
import scrollMonitor from 'scrollmonitor'

const PRICE = 9.99
const LIMIT = 10
export default {
  name: 'home',
  data: () => ({
    total: 0,
    items: [],
    cart: [],
    results: [],
    search: 'random',
    loading: false,
    price: PRICE,
    searchResults: {
      term: '',
      total: 0
    }
  }),
  mounted() {
    this.onSubmit()

    var vm = this
    var elem = document.getElementById('product-list-bottom')
    var watcher = scrollMonitor.create(elem)

    watcher.enterViewport(() => {
      if (this.results && this.results.length > 0) {
        vm.appendItems()
      }
    })
  },
  methods: {
    appendItems() {
      if (this.items.length < this.results.length) {
        var append = this.results.slice(
          this.items.length,
          this.items.length + LIMIT
        )
        this.items = this.items.concat(append)
        this.searchResults.term = this.search
        this.searchResults.total = this.items.length
      }
    },
    addItem(index) {
      let cartItem = this.cart.filter(
        item => item.id === this.items[index]['id']
      )

      if (cartItem.length === 1) {
        const cartItemIndex = this.cart.indexOf(cartItem[0])
        this.addQuantity(cartItemIndex)
      } else {
        this.cart.push({
          ...this.items[index],
          quantity: 1,
          price: PRICE
        })
        this.total += PRICE
      }
    },
    addQuantity(index) {
      if (this.cart[index].quantity <= 0) {
        this.deleteCartItem(index)
      } else {
        this.cart[index].quantity++
        this.cart[index].price += PRICE
        this.total += PRICE
      }
    },
    removeQuantity(index) {
      if (this.cart[index].quantity <= 1) {
        this.deleteCartItem(index)
      } else {
        this.cart[index].quantity--
        this.cart[index].price -= PRICE
        this.total -= PRICE
      }
    },
    deleteCartItem(index) {
      this.cart.splice(index, 1)
    },
    onSubmit() {
      this.items = []
      this.loading = true
      this.$http
        .get('/api/search/'.concat(this.search))
        .then(response => {
          this.results = response.data
          this.appendItems()
          this.loading = false
        })
        .catch(error => {
          this.loading = false
          console.log(error)
        })
    }
  },
  filters: {
    currency(price) {
      return '$'.concat(price.toFixed(2))
    }
  }
}
</script>
