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
            :items="3"
          >

            <div class="product-item"
                 v-for="itemProduct in products"
                 :key="itemProduct.id">
              <div class="pi-pic">
                <img :src="itemProduct.galleries[0].photo" alt />
                <ul>
                  <li class="w-icon active">
                    <a href="#">
                      <i class="icon_bag_alt"></i>
                    </a>
                  </li>
                  <li class="quick-view">
                    <router-link :to="'/product/'+itemProduct.id">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ itemProduct.type }}</div>
                <router-link :to="'/product/'+itemProduct.id">
                  <h5>{{ itemProduct.name }}</h5>
                </router-link>
                <div class="product-price">Rp. {{ itemProduct.price }}</div>
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
      // .get("http://127.0.0.1:8000/api/products")
      // .then(res => {this.products = res.data.data.data})
      .then(res => {this.products = res.data.data.data;console.log(res.data.data.data[1].galleries[0].photo);})
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