<template>
  <div class="shopping">
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
              <span>Shopping Cart</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
      <div class="container">
        <div class="row">
          <div class="col-lg-8">
            <div class="row">
              <div class="col-lg-12">
                <div class="cart-table">
                  <table>
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product Name</th>
                        <th>Price</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="item in cart" :key="item.id">
                        <td class="cart-pic first-row">
                          <img class="img-cart" :src="item.photo" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{ item.name }}</h5>
                        </td>
                        <td class="p-price first-row">Rp.{{ item.price }}</td>
                        <td class="delete-item">
                          <!-- <a href="#"> -->
                            <i class="material-icons" @click="removeItem(item.id)">close</i>
                          <!-- </a> -->
                        </td>
                      </tr>
                  
                    </tbody>
                  </table>
                </div>
              </div>
              <!-- Informasi Pembeli -->
              <div class="col-lg-8 text-left">
                <h4 class="mb-4">Informasi Pembeli:</h4>
                <div class="user-checkout">
                  <form>
                    <div class="form-group">
                      <label for="namaLengkap">Nama lengkap</label>
                      <input
                        type="text"
                        class="form-control"
                        id="namaLengkap"
                        aria-describedby="namaHelp"
                        placeholder="Masukan Nama"
                        v-model="customerInfo.name"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">Email Address</label>
                      <input
                        type="email"
                        class="form-control"
                        id="emailAddress"
                        aria-describedby="emailHelp"
                        placeholder="Masukan Email"
                        v-model="customerInfo.email"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">No. HP</label>
                      <input
                        type="text"
                        class="form-control"
                        id="noHP"
                        aria-describedby="noHPHelp"
                        placeholder="Masukan No. HP"
                        v-model="customerInfo.phone"
                      />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Alamat Lengkap</label>
                      <textarea class="form-control" 
                                id="alamatLengkap" 
                                rows="3"
                                v-model="customerInfo.address"></textarea>
                    </div>
                  </form>
                </div>
              </div>
              <!-- End Informasi Pembeli -->
            </div>
          </div>
          <div class="col-lg-4 text-left">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout">
                  <ul>
                    <li class="subtotal">
                      ID Transaction
                      <span>#SH12000</span>
                    </li>
                    <li class="subtotal mt-3">
                      Subtotal
                      <span>Rp. {{ totalPrice }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Pajak
                      <span>10%</span>
                    </li>
                    <li class="subtotal mt-3">
                      Total Biaya
                      <span>Rp. {{ totalCost }}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Bank Transfer
                      <span>Mandiri</span>
                    </li>
                    <li class="subtotal mt-3">
                      No. Rekening
                      <span>2208 1996 1403</span>
                    </li>
                    <li class="subtotal mt-3">
                      Nama Penerima
                      <span>Shayna</span>
                    </li>
                    <li class="subtotal mt-3">
                      Nama Pengirim
                      <span>{{ customerInfo.name }}</span>
                    </li>
                  </ul>
                  <a  @click="chekcout()" href="#" class="proceed-btn">I ALREADY PAID</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Shopping Cart Section End -->
  </div>
</template>

<script>
import Header from "@/components/Header.vue";
import axios from "axios";

export default {
  name: "Cart",
  components: {
    Header
  },
  data() {
    return {
      cart: [],
      customerInfo: {
        name: '',
        email: '',
        phone: '',
        address: ''
      }
    } 
  },
  mounted() {
    let cart = JSON.parse(localStorage.getItem('cart'));
    this.cart = cart;
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
    },
    chekcout() {
      let productIds = this.cart.map(product => product.id);
      let chekcoutData = {
        name: this.customerInfo.name,
        email: this.customerInfo.email,
        number: this.customerInfo.phone,
        address: this.customerInfo.address,
        transactions_total: this.totalCost,
        transactions_status: "PENDING",
        transaction_details: productIds
      };

      axios.
        post("http://127.0.0.1:8000/api/checkout",chekcoutData)
        .then(() => this.$router.push("success"))
        .catch(err => console.log(err));

        console.log(this.totalCost);
    }
  },
  computed: {
    totalPrice() {
      return this.cart.reduce((total, item) => total + item.price,0);
    },
    tax() {
      return (this.totalPrice * 10) / 100;
    },
    totalCost() {
      return this.totalPrice + this.tax;
    }
  }
};
</script>

<style scoped>
  .material-icons {
    cursor: pointer;
  }
  .img-cart {
    width: 100px;
    height: 100px;
  }
</style>