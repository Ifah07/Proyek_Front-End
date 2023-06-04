// main.js (atau file lain tempat Anda mengkonfigurasi Vue Router)
import Vue from 'vue'
import VueRouter from 'vue-router'
import ProductPage from './components/ProductPage.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/product',
    component: ProductPage
  },
  // Rute-rute lainnya...
]

const router = new VueRouter({
  routes
})

new Vue({
  router,
  // ...
}).$mount('#app')
