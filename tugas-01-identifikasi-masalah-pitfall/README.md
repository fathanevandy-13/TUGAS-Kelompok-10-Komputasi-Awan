# Tugas 1 — Analisis Pitfall FoodGo

**Kelompok:** KELOMPOK 10

| Nama | NIM | Kontribusi |
|---|---|---|
| [Wafii Fathan Evandy] | [103072400004] | Pitfall 1: Tight Coupling |
| [Tungkot Isahito] | [103072430013] | Pitfall 2: Single Point of Failure |
| [Nama 3] | [NIM 3] | Pitfall 3: Scalability dan Performance |

## Pitfall 1: Tight Coupling — ditulis oleh [Nama 1]

**Bukti di skenario:** Komponen pemesanan, pembayaran, dan pengelolaan pesanan FoodGo memiliki ketergantungan yang tinggi satu sama lain.

**Kenapa ini keliru:** Tight coupling membuat perubahan pada satu komponen dapat memengaruhi komponen lainnya. Akibatnya, pengembangan dan pemeliharaan sistem menjadi lebih sulit.

**Dampak ke FoodGo:** Ketika sistem pembayaran mengalami perubahan atau gangguan, proses pemesanan makanan juga dapat terganggu karena kedua komponen terlalu bergantung satu sama lain.

**Solusi desain awal:** Memisahkan komponen pemesanan dan pembayaran berdasarkan tanggung jawabnya serta menggunakan API atau antarmuka yang jelas untuk komunikasi antarkomponen.

**Trade-off:** Desain menjadi lebih fleksibel dan mudah dikembangkan, tetapi membutuhkan pengaturan komunikasi antarkomponen yang lebih baik.

---

## Pitfall 2: Single Point of Failure — ditulis oleh [Nama 2]

**Bukti di skenario:** Sistem FoodGo bergantung pada satu komponen utama yang apabila mengalami gangguan dapat menghentikan proses layanan lainnya.

**Kenapa ini keliru:** Ketergantungan pada satu komponen utama menyebabkan sistem tidak memiliki alternatif ketika komponen tersebut gagal.

**Dampak ke FoodGo:** Jika server utama mengalami gangguan, pengguna dapat kehilangan akses untuk memesan makanan, melakukan pembayaran, atau memantau status pesanan.

**Solusi desain awal:** Menerapkan mekanisme redundansi, pencadangan data, pemantauan sistem, serta pemulihan layanan ketika terjadi kegagalan.

**Trade-off:** Ketersediaan sistem meningkat, tetapi biaya infrastruktur dan kompleksitas pengelolaan menjadi lebih besar.

---

## Pitfall 3: Scalability dan Performance — ditulis oleh [Nama 3]

**Bukti di skenario:** FoodGo berpotensi mengalami peningkatan jumlah pengguna dan pesanan pada waktu tertentu, misalnya ketika jam makan siang atau saat promosi berlangsung.

**Kenapa ini keliru:** Arsitektur yang tidak dirancang untuk menangani peningkatan beban dapat menyebabkan penurunan performa ketika jumlah pengguna bertambah.

**Dampak ke FoodGo:** Aplikasi menjadi lambat, proses pembayaran membutuhkan waktu lebih lama, atau pesanan gagal diproses ketika banyak pengguna mengakses sistem secara bersamaan.

**Solusi desain awal:** Menerapkan load balancing, caching untuk data yang sering diakses, optimasi basis data, dan penskalaan sumber daya sesuai kebutuhan.

**Trade-off:** Sistem dapat menangani lebih banyak pengguna, tetapi membutuhkan biaya tambahan serta pengujian dan pemantauan performa secara berkala.

---

## Kesimpulan Kelompok

Berdasarkan analisis ketiga pitfall tersebut, FoodGo disarankan menggunakan arsitektur yang modular, memiliki mekanisme pemulihan ketika terjadi kegagalan, dan mampu menyesuaikan kapasitas sesuai jumlah pengguna. Setiap komponen perlu memiliki tanggung jawab yang jelas agar mudah dikembangkan dan dipelihara. Selain itu, sistem perlu dirancang agar gangguan pada satu komponen tidak langsung menghentikan seluruh layanan. Rekomendasi ini menjadi dasar untuk Tugas 2, yaitu mengembangkan rancangan arsitektur FoodGo yang lebih andal, fleksibel, dan mampu menangani peningkatan beban pengguna.