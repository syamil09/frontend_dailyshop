<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel
            class="product-slider"
            :nav="false"
            :dots="false"
            :autoplay="true"
            :autoWidth="true"
            :loop="false"
          >

            <div class="product-item">
              <div class="pi-pic">
                <img src="img/products/women-2.jpg" alt />
                <ul>
                  <li class="w-icon active">
                    <a href="#">
                      <i class="icon_bag_alt"></i>
                    </a>
                  </li>
                  <li class="quick-view">
                    <router-link to="/product">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">Shoes</div>
                <router-link to="/product">
                  <h5>Guangzhou sweater</h5>
                </router-link>
                <div class="product-price">$13.00</div>
              </div>
            </div>

          </carousel>
        </div>

        <div class="col-lg-12" v-else>
          <p>
            Produk Kosong
          </p>
        </div>
      </div>
    </div>
  </section>
  <!-- Women Banner Section End -->
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "Women",
  components: {
    carousel
  },
  data() {
    return {
      products: []
    }
  },
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/products")
      .then(res => {this.products = res.data.data.data})
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.pi-pic img {
  height: 400px;
}

.product-item {
  margin-right: 25px;
}
</style>