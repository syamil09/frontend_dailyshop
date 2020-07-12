<template>
  <div class="product">
    <Header />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/">
                <i class="fa fa-home"></i> Home
              </router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="image" alt />
                </div>
                <div class="product-thumbs" v-if="productDetail.galleries.length > 0">
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :dots="false"
                    :nav="false"
                    :autoplay="true"
                    :loop="true"
                  >
                    <div
                      v-for="thumb in thumbs"
                      :key="thumb.id"
                      class="pt"
                      @click="changeImage(thumb.photo)"
                      :class="thumb.photo == image ? 'active' : '' "   
                    >
                      <img :src="thumb.photo" alt />
                    </div>

                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetail.type }}</span>
                    <h3>{{ productDetail.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetail.description"></p>
                    <h4>Rp. {{ toRupiah(productDetail.price) }}</h4>
                  </div>
                  <div class="quantity">
                    <!-- <router-link to="/cart"> -->
                      <a 
                        href="#" 
                        @click="addToCart(productDetail.id, productDetail.name, productDetail.price, productDetail.galleries[0].photo)" 
                        class="primary-btn pd-cart">Add To Cart</a>
                    <!-- </router-link> -->
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProduct />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import carousel from "vue-owl-carousel";
import Header from "@/components/Header.vue";
import RelatedProduct from "@/components/RelatedProduct.vue";
import Footer from "@/components/Footer.vue";
import axios from "axios";

export default {
  name: "Product",
  components: {
    carousel,
    Header,
    RelatedProduct,
    Footer
  },
  data() {
    return {
      image: "",
      thumbs: [],
      productDetail: [],
      cart: []
    };
  },
  methods: {
    changeImage(urlImage) { 
      this.image = urlImage;
    },
    setDataPicture(data) {
      this.productDetail = data;
      this.image = data.galleries[0].photo;
      this.thumbs = data.galleries;
    },
    addToCart(id, name, price, photo) {
      let cartStorage = JSON.parse(localStorage.getItem('cart')) ?? [];
      let findId = cartStorage.find(item => item.id == id);

      if (findId == undefined) {
        const product = {
          'id': id,
          'name': name,
          'price': price,
          'photo': photo 
        }
        this.cart.push(product);
        const parsed = JSON.stringify(this.cart);
        localStorage.setItem('cart', parsed);
        window.location.reload();
      } 
    },
    toRupiah(number) {
      let number_string = number.toString(),
        sisa  = number_string.length % 3,
        rupiah  = number_string.substr(0, sisa),
        ribuan  = number_string.substr(sisa).match(/\d{3}/g);
          
      if (ribuan) {
        let separator = sisa ? '.' : '';
        rupiah += separator + ribuan.join('.');
      }

      return rupiah;
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

    axios
      .get('http://127.0.0.1:8000/api/products', {
        params: {
          id: this.$route.params.id
        }
      })
      .then(res => {
        this.setDataPicture(res.data.data);
        console.log(res.data.data);
      })
      .catch(err => console.log(err));

  }
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 10px;
}
</style>
