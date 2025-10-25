<template>
  <div :class="pageClass">
    <div class="product-container">
      <!-- Loader muncul ketika data sedang dimuat -->
      <div v-if="loading" class="loading-overlay">
        <div class="spinner"></div>
      </div>

      <!-- Jika produk tersedia -->
      <div v-else-if="product" class="product-card">
        <!-- Gambar produk -->
        <img :src="product.image" :alt="product.title" class="product-image" />
        <div class="product-info">
          <!-- Judul produk -->
          <h2 class="product-title">{{ product.title }}</h2>

          <!-- Kategori dan rating -->
          <div class="product-header">
            <p class="product-category">{{ product.category }}</p>

            <div class="rating">
              <span>{{ product.rating?.rate }}/5</span>
              <!-- rating dalam bentuk titik -->
              <div class="rating-dots">
                <span v-for="i in 5" :key="i" :class="['dot', { filled: i <= Math.round(product.rating?.rate) }]"></span>
              </div>
            </div>
          </div>

          <!-- Deskripsi produk -->
          <p class="product-desc">{{ product.description }}</p>
          
          <!-- Harga -->
          <p class="product-price">$ {{ product.price }}</p>

          <!-- Tombol aksi -->
          <div class="btn-group">
            <button class="btn-buy">Buy now</button>
            <button class="btn-next" @click="nextProduct">Next product</button>
          </div>
        </div>
      </div>

      <!-- Jika produk unavailable -->
      <div v-else class="unavailable-card">
        <div class="unavailable-face">
          <!-- gambar wajah sedih -->
          <svg viewBox="0 0 1028 439" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M221.295 114.085C221.295 99.5795 226.49 87.1321 236.879 76.7429C247.268 66.3537 259.716 61.1591 274.222 61.1591C288.727 61.1591 301.175 66.3537 311.564 76.7429C321.953 87.1321 327.148 99.5795 327.148 114.085C327.148 128.591 321.953 141.038 311.564 151.428C301.175 161.817 288.727 167.011 274.222 167.011C264.616 167.011 255.795 164.561 247.759 159.661C239.722 154.956 233.253 148.585 228.352 140.548C223.648 132.707 221.295 123.886 221.295 114.085ZM556.494 114.085C556.494 99.5795 561.689 87.1321 572.078 76.7429C582.467 66.3537 594.915 61.1591 609.42 61.1591C623.926 61.1591 636.374 66.3537 646.763 76.7429C657.152 87.1321 662.347 99.5795 662.347 114.085C662.347 128.591 657.152 141.038 646.763 151.428C636.374 161.817 623.926 167.011 609.42 167.011C599.815 167.011 590.994 164.561 582.957 159.661C574.92 154.956 568.452 148.585 563.551 140.548C558.847 132.707 556.494 123.886 556.494 114.085Z"
              fill="black"
              fill-opacity="0.1" />
            <path
              d="M454.045 227.21C528.142 227.21 596.26 236.815 658.399 256.026C720.734 275.432 778.071 303.071 830.409 338.943V400.102C811.003 385.989 787.088 372.757 758.665 360.408C730.438 348.254 699.368 337.571 665.456 328.358C631.74 319.145 596.848 311.892 560.78 306.599C524.712 301.503 489.134 298.955 454.045 298.955C407.392 298.955 360.053 303.463 312.027 312.48C264.001 321.497 219.406 333.651 178.241 348.94C137.077 364.23 103.557 381.284 77.6818 400.102V338.943C130.02 303.071 187.259 275.432 249.398 256.026C311.733 236.815 379.949 227.21 454.045 227.21Z"
              fill="black"
              fill-opacity="0.1" />
            <path d="M133 38.5C235.4 53.7 299.333 28.5 318.5 14" stroke="black" stroke-opacity="0.1" stroke-width="25" />
            <path d="M739 38.3236C636.324 53.4142 572.218 28.3956 553 14" stroke="black" stroke-opacity="0.1" stroke-width="25" />
          </svg>
        </div>

        <p class="unavailable-text">This product is unavailable to show</p>
        <button class="btn-next" @click="nextProduct">Next product</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // State data yang digunakan dalam komponen
  data() {
    return {
      index: 1,          // Menunjukkan ID produk saat ini
      product: null,     // Menyimpan data produk yang diambil dari API
      category: '',      // Menyimpan kategori produk
      loading: false,    // Status loading untuk spinner
    };
  },

  // Mengubah warna latar halaman berdasarkan kategori produk
  computed: {
    pageClass() {
      if (this.category === "men's clothing") return 'page-men';
      if (this.category === "women's clothing") return 'page-women';
      return 'page-unavailable';
    },
  },

  methods: {
    // Mengambil data produk dari FakeStore API
    async fetchProduct() {
      try {
        this.loading = true; // Tampilkan loader

        const res = await fetch(`https://fakestoreapi.com/products/${this.index}`);
        const data = await res.json();

        // Simulasi delay agar loader terlihat (UX purpose)
        await new Promise(resolve => setTimeout(resolve, 1000));

        // Filter hanya menampilkan kategori tertentu
        if (data.category === "men's clothing" || data.category === "women's clothing") {
          this.product = data;
          this.category = data.category;
        } else {
          this.product = null;
          this.category = 'unavailable';
        }
      } catch (err) {
        console.error('Fetch error:', err);
      } finally {
        this.loading = false; // Sembunyikan loader
      }
    },

    // Ganti ke produk berikutnya
    nextProduct() {
      this.index = this.index >= 20 ? 1 : this.index + 1;
      this.fetchProduct();
    },
  },

  // Panggil data pertama kali saat komponen dimuat
  mounted() {
    this.fetchProduct();
  },
};
</script>
