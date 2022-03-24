<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <!-- if -->
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel
            class="product-slider"
            :items="3"
            :dots="false"
            :nav="false"
            :autoplay="true"
          >
            <div
              class="product-item"
              v-for="itemProduct in products"
              v-bind:key="itemProduct.id"
            >
              <div class="pi-pic">
                <!-- mengambil photo product  -->
                <img v-bind:src="itemProduct.galleries[0].photo" alt="" />
                <ul>
                  <li
                    @click="
                      saveKeranjang(
                        itemProduct.id,
                        itemProduct.name,
                        itemProduct.price,
                        itemProduct.galleries[0].photo
                      )
                    "
                    class="w-icon active"
                  >
                    <a href="#"><i class="icon_bag_alt"></i></a>
                  </li>
                  <li class="quick-view">
                    <!-- mengambil id berdasarkan product -->
                    <router-link v-bind:to="'/product/' + itemProduct.id">
                      <a href="#">+ Quick View</a>
                    </router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <!-- mengambil type product -->
                <div class="catagory-name">{{ itemProduct.type }}</div>
                <router-link to="/product"
                  ><a href="#">
                    <!-- mengambil nama product -->
                    <h5>{{ itemProduct.name }}</h5>
                  </a>
                </router-link>
                <div class="product-price">
                  ${{ itemProduct.price }}
                  <span>$35.00</span>
                </div>
              </div>
            </div>
          </carousel>
        </div>
        <!-- else -->
        <div class="col-lg-" v-else>
          <p>Produk Terbaru belum tersedia untuk saat ini</p>
        </div>
      </div>
    </div>
  </section>
  <!-- Women Banner Section End -->
</template>

<script>
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
  nama: 'WomanShayna',
  components: {
    carousel,
  },
  data() {
    return {
      products: [],
      keranjangUser: [],
    };
  },
  mounted() {
    // mengambil api pada backend
    axios
      .get('http://127.0.0.1:8001/api/products')
      .then((res) => (this.products = res.data.data.data))
      .catch((err) => console.log(err));

    if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
      } catch (e) {
        localStorage.removeItem('keranjangUser');
      }
    }
  },
  methods: {
    //save keranjang add to cart
    saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {
      var productStored = {
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        photo: photoProduct,
      };

      //simpan data dari local storage
      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem('keranjangUser', parsed);

      window.location.reload();
    },
  },
};
</script>

<style scoped>
.product-item {
  margin-right: 25px;
}
</style>
