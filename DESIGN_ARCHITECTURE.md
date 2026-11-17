# Arsitektur Desain Fun Bahasa

Panduan ini mencatat keadaan frontend saat ini sebagai acuan bagi AI yang menambah halaman atau fitur. Ini adalah dokumentasi pola yang sudah ada, bukan rancangan ulang. Untuk aturan kerja repo dan detail build, baca juga [AGENTS.md](AGENTS.md).

## 1. Peta frontend dan sumber kebenaran

- Situs berupa halaman HTML statis di akar repo. Tidak ada framework komponen, backend, router, atau templating bersama. Setiap halaman memuat CSS dan asetnya sendiri; header dan footer diulang dalam HTML.
- [tailwind.config.js](tailwind.config.js) menentukan palet, breakpoint, tipografi, radius, dan bayangan. [styles.css](styles.css) berisi direktif Tailwind dan sedikit CSS sumber. [css/style.css](css/style.css) adalah hasil build yang dilacak Git; jangan edit langsung.
- Folder [img/](img/) menyimpan logo, ilustrasi, ikon SVG, dan gambar lokal. Folder [js/](js/) menyimpan data JavaScript per halaman. Bootstrap Icons, Google Fonts, dan Alpine.js dimuat lewat CDN dalam halaman HTML.
- Prioritas saat mencari contoh: konfigurasi dan CSS yang benar-benar terbangun → halaman aktif yang paling serupa → panduan ini. Class yang hanya tertulis di HTML belum tentu memiliki aturan CSS.

| Kelompok halaman      | Contoh                                                                                                                                         | Pola yang terlihat                                                             |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Beranda dan informasi | [index.html](index.html), [tentang_kami.html](tentang_kami.html), [faq.html](faq.html)                                                         | Hero, section informasi, tautan navigasi, footer.                              |
| Jelajah karya         | [baca_karya.html](baca_karya.html), [kategori-puisi.html](kategori-puisi.html), halaman `kategori-*.html` lain                                 | Kartu karya, kategori, pencarian, pagination berbasis Alpine.                  |
| Bacaan dan detail     | [artikel.html](artikel.html), [cerpen.html](cerpen.html), [karya-detail.html](karya-detail.html), [artikel-detail.html](artikel-detail.html)   | Konten dari berkas data, paragraf panjang, karya terkait.                      |
| Event dan pengumuman  | [Event.html](Event.html), [pengumuman.html](pengumuman.html)                                                                                   | Kartu event, status, pencarian, informasi acara.                               |
| Akun dan aksi         | [login.html](login.html), [register.html](register.html), [profil-pengguna.html](profil-pengguna.html), [unggah-karya.html](unggah-karya.html) | Formulir dan tata letak halaman khusus; tetap berupa template frontend statis. |

`coba.html`, `index2.html`, `index-after-login.html`, serta berkas `staging` adalah artefak/scratch yang dicatat di AGENTS.md; jangan jadikan contoh desain aktif.

## 2. Fondasi visual yang terpasang

| Aspek               | Sumber dan penggunaan saat ini                                                                                                                                                        |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Bahasa              | `<html lang="id">`; teks antarmuka berbahasa Indonesia.                                                                                                                               |
| Font                | `font-sans` memakai **Poppins**, `font-title` memakai **Nunito** untuk judul/navigasi/tombol (keduanya dimuat dari Google Fonts). Keduanya kini terdaftar di `tailwind.config.js`. |
| Warna merek         | `blue-500` **#25aae3** untuk aksi dan aksen; skala `blue-100` sampai `blue-900` ada di konfigurasi. Biru muda dipakai untuk latar/penanda, biru lebih gelap untuk teks atau hover.    |
| Warna netral        | Skala `gray-100` sampai `gray-900` (**#d9dad9** sampai **#0d0e0d**), dipakai untuk teks, batas, dan hierarki visual; permukaan utama sering putih.                                    |
| Ketebalan teks      | Judul, navigasi, tombol, dan label footer yang baru dirapikan memakai `font-semibold`. Teks isi memakai bobot normal atau medium sesuai halaman.                                      |
| Radius dan bayangan | Kartu sering `rounded-2xl`; tombol dapat `rounded-lg` atau `rounded-full`. `shadow-soft` dan `shadow-pop` tersedia di konfigurasi untuk elevasi lembut.                               |
| Responsif           | Breakpoint Tailwind v2: `sm` 640px, `md` 768px, `lg` 1024px, `xl` 1280px, `2xl` 1536px, `3xl` 1920px, `4xl` 2304px. Class dasar berlaku di layar kecil, lalu berubah pada breakpoint. |
| Ikon dan ilustrasi  | Logo/ilustrasi dari `img/`; ikon antarmuka juga menggunakan Bootstrap Icons.                                                                                                          |

Contoh kartu karya di [baca_karya.html](baca_karya.html): latar putih, batas `gray-100`, `rounded-2xl`, lalu `transition duration-200 ease-out hover:-translate-y-1 hover:shadow-soft`. Isi kartu memakai kolom fleksibel dan baris penulis `mt-auto` agar posisi penulis sejajar ketika panjang ringkasan berbeda. Ini adalah contoh yang lebih tepat untuk kartu karya daripada kartu dekoratif pada halaman lain.

**Perbedaan penting antara HTML dan CSS:** `font-title`, `bg-gray-50`, `bg-blue-150`, dan `bg-orange` sekarang dibangkitkan dari `tailwind.config.js` dan aman dipakai. Class lama `font-body`, `bg-body`, `text-body`, `title-font`, `xs:*`, `w-screen-md`, dan `border-1` tidak pernah terbangun dan hanya tersisa di halaman artefak; jangan dipakai sebagai contoh. Nilai `orange` sudah diperbaiki menjadi `#FCB316`. Jika ingin menambah utility baru, ubah konfigurasi lalu jalankan ulang `npx tailwindcss build styles.css -o css/style.css`.

## 3. Pola halaman dan interaksi

- **Kerangka halaman:** halaman utama umumnya memakai header putih dengan logo, tautan desktop, tombol menu seluler, area konten, dan footer. Karena markup diulang, lihat halaman sejenis sebelum menambah atau mengubah navigasi. Tidak ada komponen header/footer bersama.
- **Hierarki konten:** judul halaman berukuran sekitar `text-3xl` hingga `xl:text-4xl`; judul kartu lebih kecil; informasi pendukung memakai abu-abu lebih muda. Contoh: [Event.html](Event.html) dan [baca_karya.html](baca_karya.html).
- **Grid karya:** [baca_karya.html](baca_karya.html) menampilkan satu kolom pada layar kecil dan tiga kolom mulai `md`, dengan `gap-6`. Halaman kategori memakai susunan yang lebih sempit dan kontrol pencarian/pagination sendiri.
- **Detail bacaan:** [karya-detail.html](karya-detail.html) menggunakan kolom konten utama dan kolom karya terkait pada `lg`. Paragraf diulang dengan Alpine `x-for`; konten dan metadata berasal dari data JavaScript, bukan HTML yang ditulis satu per satu.
- **Gerak dan state:** banyak tautan, kartu, tab, panah pagination, dan ikon sosial menggunakan transisi sekitar 200 ms. Halaman yang lebih baru menggunakan `ease-out`, efek hover lembut, dan beberapa `motion-reduce` untuk mengurangi transformasi.
- **Navigasi dalam halaman:** `styles.css` mengatur `scroll-behavior: smooth`. [baca_karya.html](baca_karya.html) menggunakan target kategori dan offset scroll agar judul bagian tidak tertutup area atas.

Pola aksesibilitas belum seragam di seluruh halaman. Contoh lama mencakup tautan bersarang, tautan placeholder, label ikon yang kurang spesifik, dan impor Alpine ganda pada beberapa halaman. Itu adalah kondisi saat ini yang perlu diketahui, bukan contoh untuk disalin. Ketika menambah UI, gunakan satu tautan per tujuan, nama kontrol yang jelas, fokus keyboard yang terlihat, dan teks alternatif gambar sesuai fungsinya.

## 4. Alur data frontend

Data disimpan sebagai `var` global dalam `js/data-*.js`. Halaman memuat berkas data **sebelum** skrip inline yang mendefinisikan fungsi `load*()` atau aplikasi detail. Alpine v2.8.1 kemudian membaca fungsi itu melalui `x-data`, memakai `x-for`, `x-text`, dan binding atribut untuk merender elemen.

- [baca_karya.html](baca_karya.html) membaca [js/data-karya.js](js/data-karya.js): `dataPuisi`, `dataCerpen`, `dataCeritaAnak`, `dataArtikel`, dan `dataEsai`.
- Halaman `kategori-*.html` membaca berkas `js/data-kategori-*.js` masing-masing; data umumnya bernama `sourceData`. Pencarian dan pagination dihitung di fungsi Alpine halaman.
- [Event.html](Event.html) membaca [js/data-event.js](js/data-event.js), termasuk daftar event berlangsung dan event sebelumnya.
- [karya-detail.html](karya-detail.html) membaca parameter `kategori` dan `id`, memetakan slug kategori ke array karya, lalu merender `fullContent`. [artikel-detail.html](artikel-detail.html) memakai parameter `id` dan `sourceData` dari [js/data-detail-artikel.js](js/data-detail-artikel.js), dengan paragraf dalam `content`.
- Halaman artikel, cerpen, cerita anak, dan esai yang lebih lama juga memakai berkas `js/data-detail-*.js` masing-masing. Periksa bentuk objek pada halaman tujuan sebelum memakai data dari keluarga halaman lain.

URL detail dan ID string pada data merupakan bagian dari hubungan antarhalaman. Saat menambah kartu atau konten, ikuti format tautan dan nama field yang dipakai halaman tujuan.

## 5. Cara memakai panduan ini saat menambah fitur

1. Tentukan keluarga halaman yang paling dekat; baca HTML halaman tersebut bersama berkas data yang dimuatnya.
2. Ambil warna, font, radius, bayangan, dan breakpoint dari konfigurasi yang ada. Periksa class di CSS hasil build bila sebuah utility tampak tidak standar.
3. Pertahankan pola HTML statis + Alpine v2 untuk interaksi kecil dan data lokal yang sudah mengikuti pola ini. Pastikan urutan `<script>` benar.
4. Untuk perubahan gaya global, edit `styles.css` atau `tailwind.config.js`, lalu bangun ulang dengan CLI Tailwind v2 lokal: `npx tailwindcss build styles.css -o css/style.css`. `npm run build` memakai path keluaran Windows dan tidak portabel ke macOS/Linux.
5. Periksa halaman terkait pada lebar ponsel dan desktop: susunan grid, menu, keterbacaan teks, hover/fokus, gambar, serta tautan ke detail. Repo tidak menyediakan test, lint, atau typecheck yang berfungsi.

`purge: []` saat ini mempertahankan seluruh utility Tailwind dalam CSS yang besar. Jika pengaturan ini suatu saat diubah, class yang muncul di string JavaScript juga harus ikut dihitung agar tidak hilang dari hasil build.
