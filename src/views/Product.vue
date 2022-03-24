<template>
  <div class="product">
    <HeaderShayna />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
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
                  <img class="product-big-img" :src="gambar_default" alt="" />
                </div>
                <!-- mengatur agar foto yg di bawah ke kanan tidak ke bawah -->
                <div
                  class="product-thumbs"
                  v-if="productDetails.galleries.length > 0"
                >
                  <carousel
                    :dots="false"
                    :nav="false"
                    class="product-thumbs-track ps-slider"
                  >
                    <div
                      v-for="ss in productDetails.galleries"
                      :key="ss.id"
                      class="pt"
                      @click="changeImage(ss.photo)"
                      :class="ss.photo == gambar_default ? 'active' : ''"
                    >
                      <img :src="ss.photo" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description"></p>
                    <h4>${{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart">
                      <a
                        @click="
                          saveKeranjang(
                            productDetails.id,
                            productDetails.name,
                            productDetails.price,
                            productDetails.galleries[0].photo
                          )
                        "
                        href="#"
                        class="primary-btn pd-cart"
                        >Add To Cart</a
                      >
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->
    <ReleatedProduct />
    <FooterShayna />
  </div>
</template>

<script>
import HeaderShayna from '@/components/HeaderShayna.vue';
import ReleatedProduct from '@/components/ReleatedProduct.vue';
import FooterShayna from '@/components/FooterShayna.vue';
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
  name: 'Product',
  components: {
    HeaderShayna,
    ReleatedProduct,
    FooterShayna,
    carousel,
  },
  data() {
    return {
      gambar_default: '',
      productDetails: [],
      keranjangUser: [],
    };
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
    },
    setDataPicture(data) {
      //replace object product details dengan data dari api
      this.productDetails = data;
      //replace value gambar default dengan data dari api galleries
      this.gambar_default = data.galleries[0].photo;
    },
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
    },
  },

  mounted() {
    if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
      } catch (error) {
        localStorage.removeItem('keranjangUser');
      }
    }
    // mengambil api pada backend
    axios
      .get('http://127.0.0.1:8001/api/products', {
        params: {
          // mendapatkan id pada item product
          id: this.$route.params.id,
        },
      })
      .then((res) => this.setDataPicture(res.data.data))
      .catch((err) => console.log(err));
  },
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 14px;
}
</style>
