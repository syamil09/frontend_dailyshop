<template>
  <div>
    <header class="header-section">
      <div class="header-top">
        <div class="container">
          <div class="ht-left">
            <div class="mail-service">
              <i class="fa fa-envelope"></i> hello.shayna@gmail.com
            </div>
            <div class="phone-service">
              <i class="fa fa-phone"></i> +628 22081996
            </div>
          </div>
        </div>
      </div>
      <div class="container">
        <div class="inner-header">
          <div class="row">
            <div class="col-lg-2 col-md-2">
              <div class="logo">
                <router-link to="/">
                  <!-- <a href="#"> -->
                  <img src="img/logo_website_shayna.png" alt />
                  <!-- </a> -->
                </router-link>
              </div>
            </div>
            <div class="col-lg-7 col-md-7"></div>
            <div class="col-lg-3 text-right col-md-3">
              <ul class="nav-right">
                <li class="cart-icon">
                  Keranjang Belanja &nbsp;
                  <a href="#">
                    <i class="icon_bag_alt"></i>
                    <span>{{ cart.length }}</span>
                  </a>
                  <div class="cart-hover">
                    <div class="select-items">
                      <table>
                        <tbody v-if="cart.length > 0">
                          <tr 
                            v-for="item in cart"
                            :key="item.id">
                            <td class="si-pic">
                              <img :src="item.photo" class="photo-item" alt />
                            </td>
                            <td class="si-text">
                              <div class="product-selected">
                                <p>Rp.{{ item.price }}</p>
                                <h6>{{ item.name }}</h6>
                              </div>
                            </td>
                            <td @click="removeItem(item.id)" class="si-close">
                              <i class="ti-close"></i>
                            </td>
                          </tr>
                        </tbody>
                        <tbody v-else>
                          <tr>
                            <td>Empty cart</td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                    <div class="select-total">
                      <span>total:</span>
                      <h5>$120.00</h5>
                    </div>
                    <div class="select-button">
                      <a href="#" class="primary-btn view-card">
                        <router-link to="/cart" class="text-white">VIEW CARD</router-link>
                      </a>

                      <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </header>
  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      cart: []
    }
  },
  methods: {
    removeItem(idx) {

      let cartStorage = JSON.parse(localStorage.getItem('cart'));
      let itemCartStorage = cartStorage.map(item => item.id);
      let index = itemCartStorage.findIndex(id => id == idx);
      this.cart.splice(index, 1);

      const parsed = JSON.stringify(this.cart);
      localStorage.setItem('cart', parsed);
      window.location.reload();
    }
  },
  mounted() {
    if (localStorage.getItem('cart')) {
      try {
        this.cart = JSON.parse(localStorage.getItem('cart'));
      } catch(e) {
        localStorage.removeItem('cart');
      }
    }
  }
};
</script>

<style scoped>
  .photo-item {
    width: 80px;
    height: 80px;
  }
</style>