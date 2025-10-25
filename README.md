# E-Commerce Catalog – Vue.js 2

Proyek ini dibuat sebagai tugas **Frontend Developer** untuk menampilkan data produk dari **[FakeStore API](https://fakestoreapi.com)**.  
Website ini menampilkan **satu produk per halaman** dan dapat berubah ketika pengguna menekan tombol **Next Product**.  
Dilengkapi dengan **loader interaktif** untuk menjaga tampilan tetap **UX-friendly**.

---

## Fitur Utama

- Mengambil data produk dari **FakeStore API**
- Menampilkan produk berdasarkan **index (1–20)**
- Filter kategori hanya untuk:
  - `men's clothing`
  - `women's clothing`
- Tombol **Next Product** untuk berpindah antar produk
- Loader animasi (spinner/skeleton) saat menunggu respon API
- Warna background berubah sesuai kategori
- Menampilkan **rating produk**, **deskripsi**, dan **harga**

---

## Struktur Proyek

```
ecommerce/
│
├── public/
├── src/
│   ├── assets/
│   │   └── style/
│   │       └── page.css        # Style utama halaman
│   ├── components/
│   │   └── ProductDisplay.vue  # Komponen utama tampilan produk
│   ├── App.vue                 # Root component
│   └── main.js                 # Entry point aplikasi
│
├── package.json
├── babel.config.js
├── vue.config.js
└── README.md
```

---

## ⚙️ Instalasi & Menjalankan Proyek

### 1️. Clone repository
```bash
git clone git@github.com:machfudn/ecommerce-catalog.git
cd ecommerce-catalog
```

### 2️. Install dependencies
```bash
npm install
```

### 3️. Jalankan server lokal
```bash
npm run serve
```

Akses di browser melalui:  
👉 [http://localhost:8080](http://localhost:8080)

---

## Penjelasan Komponen: `ProductDisplay.vue`

File ini berfungsi untuk:
- Mengambil data produk menggunakan `fetch()`
- Menampilkan tampilan produk berdasarkan kategori
- Mengatur loading state agar UX tetap halus
- Memberikan aksi “Next Product” untuk menampilkan produk berikutnya

### Alur Kerja:
1. Komponen dimuat → `mounted()` memanggil `fetchProduct()`
2. Data produk diambil dari API berdasarkan `index`
3. Loader ditampilkan selama proses pengambilan data
4. Produk ditampilkan (atau pesan “unavailable” jika kategori tidak sesuai)
5. Klik tombol “Next Product” untuk mengganti data

---

## Contoh Fungsi Utama

```javascript
async fetchProduct() {
  try {
    this.loading = true;
    const res = await fetch(`https://fakestoreapi.com/products/${this.index}`);
    const data = await res.json();

    // Simulasi delay 1 detik
    await new Promise(resolve => setTimeout(resolve, 1000));

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
    this.loading = false;
  }
}
```

---

## Styling

- File CSS disimpan di:  
  `src/assets/style/page.css`
- Mengatur warna latar, animasi spinner, dan layout kartu produk
- Warna latar disesuaikan dengan kategori:
  - `men's clothing` → `page-men`
  - `women's clothing` → `page-women`
  - Lainnya → `page-unavailable`

---

## Teknologi yang Digunakan

| Teknologi | Keterangan |
|------------|-------------|
| Vue.js 2   | Framework utama frontend |
| HTML5 & CSS3 | Struktur & styling |
| FakeStore API | Sumber data produk |
| JavaScript (ES6) | Logika aplikasi |
| GitHub | Version control  |
| GitHub | Version control  |
---

## 📸 Preview 
![User Interface](https://github.com/user-attachments/assets/c878e6d6-cecb-4b6c-ac8a-473811dd92f0)


### ✨ “Frontend bukan sekadar tampilan — tapi pengalaman pengguna.”
