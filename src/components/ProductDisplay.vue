<template>
  <div>
    <div class="background" :class="category">
      <div class="wrapper">
        <div class="card">
          <div v-if="loading" class="loading-screen">
            <span class="loader"></span>
            <span class="loader-text">Looking for the item. . . </span>
          </div>
          <div v-else>
            <div class="available" v-if="produk">
              <img :src="`${produk.image}`" alt="gambar-produk" />
              <div class="detail-produk">
                <div class="nama-produk" :class="category">
                  {{ produk.title }}
                </div>
                <div class="spek">
                  <div class="kategori">{{ produk.category }}</div>
                  <div class="rating">
                    <div class="skor">{{ produk.rating.rate }}/5</div>
                    <div class="rating-wrapper">
                      <div
                        class="rating-circle-number"
                        v-for="n in rating"
                        :key="n"
                      >
                        <div class="rating-circle" :class="category"></div>
                      </div>
                      <div
                        class="rating-circle-number"
                        v-for="m in sisaRating"
                        :key="m"
                      >
                        <div
                          class="rating-circle-blank"
                          :class="category"
                        ></div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="garis"></div>
                <div class="deskripsi-produk">
                  {{ produk.description }}
                </div>
              </div>
              <div class="transaksi">
                <div class="garis"></div>
                <div class="harga" :class="category">$ {{ produk.price }}</div>
                <div class="tombol">
                  <button
                    class="kotak-tombol buy-now"
                    :class="category"
                    @click="buyNow"
                  >
                    Buy now
                  </button>
                  <button
                    class="kotak-tombol next"
                    :class="category"
                    @click="nextItem"
                  >
                    Next product
                  </button>
                </div>
              </div>
            </div>
            <div class="unavailable" v-else>
              <img src="../assets/Frown/Slide1.jpg" alt="frown" class="frown" />
              <div class="unavailable-text">
                <p>This product is not available to show</p>
                <button class="unavailable-next" @click="nextItem">
                  Next Product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import page from "../assets/styles/page.css";
export default {
  data() {
    return {
      produk: {},
      index: 1,
      loading: true,
      rating: 0,
      sisaRating: 0,
      category: "",
    };
  },
  methods: {
    setCategory() {
      this.category = this.produk.category;
      this.category == "men's clothing"
        ? (this.category = "blue")
        : this.category == "women's clothing"
        ? (this.category = "pink")
        : "";
    },
    setRating() {
      this.rating = Math.round(this.produk.rating.rate);
      this.sisaRating = 5 - this.rating;
    },
    nextItem() {
      this.index < 20 ? this.index++ : (this.index = 1);
      this.loading = true;
      this.produk = {};
      fetch("https://fakestoreapi.com/products/" + this.index)
        .then((res) => res.json())
        .then((json) => {
          json.category == "men's clothing" ||
          json.category == "women's clothing"
            ? Object.assign(this.produk, json)
            : (this.produk = "");
          this.loading = false;
          if (this.produk) {
            this.setCategory();
            this.setRating();
          } else {
            this.category = "";
          }
        });
    },
    buyNow() {
      alert("Thank you for your purchase");
    },
  },
  created() {
    fetch("https://fakestoreapi.com/products/" + this.index)
      .then((res) => res.json())
      .then((json) => {
        json.category == "men's clothing" || json.category == "women's clothing"
          ? Object.assign(this.produk, json)
          : (this.produk = "");
        this.loading = false;
        if (this.produk) {
          this.setCategory();
          this.setRating();
        } else {
          return;
        }
      });
  },
};
</script>

<style></style>
