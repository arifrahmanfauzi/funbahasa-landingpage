# Product Requirements Document (PRD): Funbahasa v2.0

**Fokus Pengembangan:** Otomatisasi Alur Kompetisi, Portofolio Penulis Permanen, dan Ekspansi Monetisasi

---

## 1. Informasi Dokumen & Ringkasan Eksekutif

| Atribut            | Detail                                                                                                                                                                                                           |
|:------------------ |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Nama Produk**    | Funbahasa Web Platform v2.0                                                                                                                                                                                      |
| **Status Dokumen** | Draft v1.0 (Siap untuk Review Teknis & UI/UX)                                                                                                                                                                    |
| **Tujuan Utama**   | Menghilangkan *bottleneck* konfirmasi manual via WhatsApp, meningkatkan retensi penulis melalui portofolio permanen, serta mendongkrak konversi *Pre-Order* (PO) antologi dan *Average Revenue Per User* (ARPU). |
| **Target Rilis**   | Bertahap (Fase 1: Otomatisasi Inti & PO; Fase 2: Retensi & Monetisasi Lanjutan)                                                                                                                                  |

---

## 2. Latar Belakang & Masalah yang Diselesaikan

Saat ini, Funbahasa telah memiliki validasi pasar yang kuat melalui *event* menulis berkala (Cerpen, Puisi, Cerita Anak), sistem kurasi dua tahap yang kredibel, serta monetisasi sekunder melalui PO buku antologi. Namun, terdapat empat hambatan struktural untuk skala yang lebih besar:

1. **Gesekan Pendaftaran Tinggi (*High Friction*):** Peserta harus mendaftar, membayar, menunggu verifikasi manual admin di WhatsApp, baru bisa mengunggah naskah. Jeda ini memicu pembatalan (*drop-off*) dan membebani admin menjelang tenggat waktu (*deadline*).
2. **Siklus Hidup Konten Pendek:** Naskah juara 1–10 hanya dipajang di halaman depan hingga *event* berikutnya dimulai, sehingga aset konten hilang dan tidak terindeks mesin pencari (SEO).
3. **Fluktuasi Penjualan PO Antologi:** Penjualan buku sangat bergantung pada inisiatif promosi pribadi peserta, sementara belum ada mekanisme insentif atau pelacakan sederhana bagi kontributor non-juara.
4. **Pendapatan Terbatas pada Tiket Tunggal:** Belum ada opsi produk tambahan (*upsell*) saat pendaftaran berlangsung.

---

## 3. Target Pengguna (*User Personas*)

* **Penulis Pemula & Pelajar (Peserta Reguler):** Mencari validasi karya, pengalaman berkompetisi, dan sertifikat prestasi resmi. Membutuhkan alur daftar-bayar-unggah yang cepat.
* **Penulis Ambisius (Kontributor Serius):** Ingin karyanya dibukukan, membutuhkan umpan balik (*feedback*) untuk meningkatkan kualitas tulisan, dan ingin memiliki jejak rekam publikasi digital.
* **Tim Kurator (Internal & Profesional):** Membutuhkan ruang kerja yang bersih untuk membaca, menilai, dan menyaring puluhan hingga ratusan naskah secara objektif tanpa harus mengunduh file satu per satu secara manual.
* **Admin & Tim Bisnis Funbahasa:** Membutuhkan dasbor terpusat untuk memantau transaksi masuk, status naskah, dan rekap data pembeli PO tanpa proses rekonsiliasi manual di WhatsApp.

---

## 4. Spesifikasi Fungsional (Functional Requirements)

### Epic 1: Otomatisasi Pendaftaran, Pembayaran, & Unggah Naskah (Prioritas: P0)

Menggantikan alur verifikasi WhatsApp menjadi alur satu pintu (*end-to-end*) di dalam website.

* **FR-1.1 Akun & Profil Dasar Pengguna:** Pengguna mendaftar/masuk menggunakan email atau Google OAuth sebelum mengikuti *event*.
* **FR-1.2 Pemilihan Paket Tiket (*Tiered Ticketing*):**
  * **Tiket Reguler:** Akses unggah 1 naskah + *e-sertifikat* peserta otomatis.
  * **Tiket Plus Evaluasi (*Paid Critique Add-on*):** Akses unggah 1 naskah + *e-sertifikat* + catatan evaluasi tertulis (minimal 100 kata) dari kurator internal setelah pengumuman selesai.
* **FR-1.3 Integrasi *Payment Gateway*:** Integrasi otomatis (Midtrans / Xendit) mendukung QRIS, Virtual Account, dan E-Wallet.
* **FR-1.4 Gerbang Unggah Naskah Otomatis:** Begitu status pembayaran terkonfirmasi `PAID` oleh sistem, formulir unggah naskah langsung terbuka. Sistem mendukung unggah file (`.doc`, `.docx`) dan/atau *rich-text editor* langsung di web, lengkap dengan validasi jumlah kata otomatis sesuai aturan *event*.
* **FR-1.5 Notifikasi Status Otomatis:** Pengiriman bukti pembayaran dan tanda terima naskah secara otomatis melalui email dan/atau WhatsApp API.

### Epic 2: Sistem Manajemen Kurasi 2 Tahap (Prioritas: P0)

Mempermudah alur kerja penyaringan naskah dari tahap internal hingga kurator profesional.

* **FR-2.1 *Anonymized View* (Penilaian Buta):** Kurator hanya melihat judul, isi naskah, dan nomor registrasi (nama penulis disembunyikan untuk menjaga objektivitas).
* **FR-2.2 Tahap 1 — Kurasi Internal:**
  * Kurator internal dapat memberikan skor (skala 1–100), status (`Lolos ke Tahap 2` / `Tidak Lolos`), serta mengisi kotak teks evaluasi khusus bagi peserta yang membeli *Tiket Plus Evaluasi*.
* **FR-2.3 Tahap 2 — Kurasi Profesional:**
  * Naskah yang berstatus `Lolos ke Tahap 2` otomatis masuk ke dasbor kurator profesional.
  * Kurator profesional memberikan skor akhir dan catatan apresiasi untuk menentukan peringkat Juara 1–10.
* **FR-2.4 Generator *E-Sertifikat* Otomatis:** Setelah pemenang dikunci oleh admin, sistem otomatis menghasilkan *e-sertifikat* unik (dengan QR code verifikasi) untuk Juara 1–10 dan seluruh peserta aktif.

### Epic 3: Portofolio Penulis Permanen & Arsip Karya (Prioritas: P1)

Mengubah naskah pemenang dan arsip *event* menjadi aset jangka panjang untuk retensi dan SEO.

* **FR-3.1 Halaman Profil Penulis Publik:** Setiap pengguna memiliki URL profil permanen (contoh: `funbahasa.id/penulis/nama-penulis`) yang menampilkan bio singkat, daftar lencana juara, dan karya yang pernah dipublikasikan.
* **FR-3.2 Arsip *Event* & Hall of Fame:** Halaman depan tetap menampilkan Juara 1–10 *event* terbaru, namun ketika *event* baru dimulai, karya pemenang sebelumnya dipindahkan ke halaman **Arsip Pemenang** yang dapat dibaca publik dan terindeks Google.
* **FR-3.3 Fitur Berbagi Sosial (*Social Share Card*):** Tombol bagikan langsung ke Instagram Story, X, dan WhatsApp dengan *preview* gambar kutipan naskah dan nama penulis.

### Epic 4: Sistem Pre-Order (PO) Antologi & Pengungkit Penjualan (Prioritas: P1)

Menstabilkan fluktuasi pembeli PO tanpa menambah kerumitan pelacakan teknis.

* **FR-4.1 Halaman Katalog & Pemesanan PO:** Halaman khusus untuk setiap buku antologi yang sedang dibuka masa PO-nya, terhubung dengan *payment gateway* dan kalkulator ongkos kirim otomatis (integrasi API RajaOngkir/Biteship).
* **FR-4.2 Kolom Perujuk Sederhana (*Referral Attribution*):** Menambahkan kolom *dropdown* atau pencarian nama kontributor pada form *checkout* PO (*"Tahu buku ini dari penulis siapa?"*). Sistem otomatis merekap peringkat "Penulis Terlaris" di dasbor admin untuk pemberian hadiah di akhir periode PO.
* **FR-4.3 Paket Grosir Penulis (*Author Bundle*):** Opsi pembelian paket khusus (misal: Beli 5 Buku Diskon 15%) yang muncul otomatis pada akun peserta yang naskahnya lolos masuk buku antologi.
* **FR-4.4 *Progress Bar* Target Kolektif (*Stretch Goal*):** Indikator visual di halaman PO yang menunjukkan jumlah eksemplar terjual menuju target bonus bersama (contoh: *"142 / 200 Eksemplar Terjual — Buka Bonus E-Book Eksklusif untuk Semua Pembeli!"*).

### Epic 5: Fondasi Ekspansi Monetisasi — Fase 2 (Prioritas: P2)

Fitur lanjutan setelah otomasi inti stabil.

* **FR-5.1 *Season Pass* / Keanggotaan Tahunan:** Paket berlangganan 6 bulan atau 1 tahun yang memberikan kuota gratis pendaftaran untuk seluruh *event* reguler dan diskon otomatis pembelian PO buku.
* **FR-5.2 Modul *Lead Capture* Penerbitan Solo:** Formulir pengajuan naskah buku tunggal yang ditampilkan secara eksklusif kepada penulis yang sudah mengikuti minimal 2 *event* atau pernah masuk 10 besar.

---

## 5. User Flow & Skema Database Dasar

### 5.1 Alur Pengguna Utama (User Flow)

1. **Alur Pendaftaran & Unggah Naskah:**
   `Pilih Event Aktif` -> `Login/Register` -> `Pilih Tiket (Reguler / Plus Evaluasi)` -> `Bayar via Payment Gateway` -> `Webhook PAID` -> `Form Unggah Naskah Terbuka` -> `Submit Naskah` -> `Konfirmasi Otomatis (Email/WA)`.
2. **Alur Kurasi & Pengumuman:**
   `Event Ditutup` -> `Kurasi Tahap 1 (Internal + Input Evaluasi)` -> `Filter Lolos Tahap 2` -> `Kurasi Tahap 2 (Profesional)` -> `Penetapan Juara 1-10` -> `Publish Halaman Depan + Profil Penulis + Generate E-Sertifikat`.
3. **Alur Pre-Order Buku Antologi:**
   `Buka Halaman PO Buku` -> `Pilih Paket (Satuan / Paket Grosir Penulis)` -> `Pilih Nama Penulis Perujuk (Opsional)` -> `Hitung Ongkir & Bayar` -> `Update Progress Bar Stretch Goal`.

### 5.2 Entitas Database Utama (ERD Ringkas)

* `users`: `id`, `name`, `email`, `slug_username`, `bio`, `role` (`participant`, `curator_internal`, `curator_pro`, `admin`), `created_at`
* `events`: `id`, `title`, `category` (`cerpen`, `puisi`, `cerita_anak`), `theme`, `start_date`, `end_date`, `price_regular`, `price_critique`, `status`
* `registrations`: `id`, `user_id`, `event_id`, `ticket_type` (`regular`, `with_critique`), `payment_status` (`pending`, `paid`, `expired`), `payment_gateway_ref`
* `submissions`: `id`, `registration_id`, `anonymous_code`, `title`, `content_body`, `file_url`, `word_count`, `score_internal`, `critique_notes`, `is_shortlisted`, `score_pro`, `final_rank` (`1-10` atau `null`)
* `anthology_books`: `id`, `event_id`, `title`, `price`, `stretch_goal_target`, `current_sold_count`, `po_deadline`
* `book_orders`: `id`, `book_id`, `buyer_name`, `buyer_contact`, `shipping_address`, `quantity`, `is_author_bundle`, `referred_by_user_id`, `total_amount`, `payment_status`

---

## 6. Kebutuhan Non-Fungsional (Non-Functional Requirements)

1. **Keamanan & Hak Cipta Naskah:** Naskah yang diunggah peserta tidak dapat diunduh atau disalin secara massal oleh publik. Fitur klik-kanan dan *copy-paste* pada halaman baca karya pemenang dinonaktifkan untuk meminimalkan risiko plagiarisme.
2. **Skalabilitas Trafik *Deadline*:** Server harus mampu menangani lonjakan trafik hingga 5x lipat dalam 6 jam terakhir sebelum penutupan *event* tanpa mengalami *downtime* pada proses unggah dan pembayaran.
3. **Mobile-First Responsive:** Minimal 75% pengguna di Indonesia mengakses melalui ponsel; seluruh alur pendaftaran, pembayaran QRIS, dan pembacaan karya harus optimal di layar *mobile*.

---

## 7. Metrik Keberhasilan (Key Performance Indicators)

| Area                      | Metrik Utama                          | Target Keberhasilan (3 Bulan Pasca-Rilis)                                                               |
|:------------------------- |:------------------------------------- |:------------------------------------------------------------------------------------------------------- |
| **Efisiensi Operasional** | Waktu verifikasi pembayaran           | Turun dari hitungan jam (manual WA) menjadi **< 1 menit** (otomatis).                                   |
| **Konversi Pendaftaran**  | *Registration-to-Submission Rate*     | Meningkat minimal **25%** karena hilangnya jeda konfirmasi.                                             |
| **Ekspansi ARPU**         | Tingkat adopsi *Tiket Plus Evaluasi*  | Minimal **15–20%** total peserta memilih tiket dengan evaluasi kurator.                                 |
| **Stabilitas PO Buku**    | Rata-rata eksemplar terjual per judul | Meningkat **30%** dan varians antar-*event* lebih stabil berkat fitur *Author Bundle* & *Stretch Goal*. |
| **Pertumbuhan Organik**   | Trafik organik (SEO) ke halaman karya | Peningkatan kunjungan bulanan sebesar **40%** dari halaman profil penulis dan arsip naskah.             |

---

## 8. Roadmap Pengembangan Bertahap

* **Sprint 1–2 (Fondasi & Otomatisasi Inti):** Pengembangan sistem autentikasi akun pengguna, integrasi *payment gateway*, alur pendaftaran dua tipe tiket (Reguler & Plus Evaluasi), serta formulir unggah naskah otomatis.
* **Sprint 3 (Dasbor Kurasi & Sertifikat):** Pembuatan *workspace* kurator internal dan profesional serta generator *e-sertifikat* otomatis.
* **Sprint 4 (Portofolio & Modul PO Antologi):** Peluncuran halaman profil penulis permanen, arsip karya pemenang, serta halaman *checkout* PO lengkap dengan *referral dropdown*, paket grosir, dan *stretch goal bar*.
* **Sprint 5 (Optimasi & Fase 2):** Pengujian beban (*load testing*), peluncuran *Season Pass*, dan integrasi *funnel* penerbitan buku solo.
