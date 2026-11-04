# Jurnal Proses — Tugas 2

## [4 November 2026]
- Opsi arsitektur yang dipertimbangkan: Monolitik, SOA (Service-Oriented Architecture), Publish-Subscribe (Pub-Sub), & Kombinasi SOA + Pub-Sub
- Kenapa akhirnya pilih [SOA/Pub-Sub]: Karena kebutuhan komunikasi di FoodGo tidak seragam. Bagian Pesanan–Pembayaran butuh kepastian langsung ("pembayaran berhasil atau gagal") sebelum sistem lanjut, jadi harus sinkron (SOA) — kalau dibuat asinkron, Order Service bisa lanjut memproses pesanan padahal hasil pembayaran belum diketahui. Sebaliknya, koordinasi lintas layanan seperti notifikasi pesanan baru ke resto, penugasan kurir, dan update status ke pelanggan sifatnya boleh tertunda, punya beberapa penerima, dan tidak butuh jawaban seketika, sehingga lebih tepat pakai Pub-Sub lewat message broker. Kombinasi ini juga menjawab masalah tight coupling dari Tugas 1: Order Service tidak perlu memanggil Catalog Resto, Courier, dan Notification secara langsung, cukup mem-publish event. Selain itu, urutan event OrderPaid → OrderAccepted → CourierAssigned menjamin kurir baru ditugaskan setelah resto menerima pesanan, bukan berjalan paralel secara kebetulan.
- Revisi diagram (versi 1 → versi 2, apa yang berubah dan kenapa): 
*semua komponen terhubung langsung satu sama lain (Order memanggil Catalog, Courier, dan Notification secara langsung). Alur resto dan kurir berjalan hampir bersamaan setelah pembayaran, dan Catalog Resto Service hanya muncul sebagai sumber data menu lewat API Gateway — belum terlibat dalam alur notifikasi pesanan.

*Ditambahkan Message Broker sebagai perantara; Order Service hanya mem-publish OrderPaid ke broker, bukan memanggil service lain langsung.

Catalog Resto Service dipindah menjadi subscriber OrderPaid dan publisher OrderAccepted — sehingga urutan "resto terima pesanan dulu → baru kurir ditugaskan" dijamin oleh urutan event, bukan sekadar proses paralel.

Ditambahkan event OrderAccepted sebagai langkah antara sebelum CourierAssigned, yang sebelumnya tidak ada di versi 1.

Alur lihat menu (langkah 0, Customer → Gateway → Catalog) dipisahkan sebagai alur putus-putus agar tidak tercampur dengan alur pemesanan utama.

Ditambahkan penomoran langkah 1–13 pada diagram agar keterangan sinkron vs asinkron bisa ditunjuk per langkah.

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|4 Nov 2026|ChatGPT|"Bandingkan arsitektur monolitik, SOA, dan publish-subscribe untuk aplikasi food delivery"|Menjelaskan karakteristik tiap gaya arsitektur, kapan cocok dipakai, dan contoh komunikasi sinkron vs asinkron|Dijadikan bahan pertimbangan awal; dipilih kombinasi SOA + Pub-Sub karena kebutuhan FoodGo tidak seragam, lalu alasannya ditulis ulang sendiri di bagian 1 dan 6|
| 4 Nov 2026 | ChatGPT | "Komponen apa saja yang biasanya ada di arsitektur food delivery dengan message broker?" | Menyebut API Gateway, Order, Payment, Catalog/Restaurant, Courier, Notification, dan Message Broker | Daftar komponen dipakai sebagai checklist; fungsi tiap komponen ditulis ulang sesuai alur FoodGo (mis. Courier Service baru bertindak setelah OrderAccepted) |