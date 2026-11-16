# APJIKI RamaLabs — Audit Jurnal via URL v6

Aplikasi statis GitHub Pages untuk evaluasi mandiri sesuai struktur instrumen Juknis 374/2026 (32 sub-unsur A–H). Copyright © 2026 ramalabs.

## Cara menggunakan
1. Unggah `index.html`, `intelligence.js`, dan `instrumen.json` ke root repository GitHub.
2. Aktifkan GitHub Pages dari branch `main` / `(root)`.
3. Di tab **Audit URL · DOI · PDF · AI**, masukkan URL jurnal publik dan tekan **Audit + prediksi 32 sub-unsur**.
4. Aplikasi mencoba (i) Worker pribadi jika diatur, (ii) pembacaan langsung, (iii) pembacaan melalui Jina Reader publik. Jika berhasil, tampil hasil sumber dan prediksi per butir.
5. Masukkan DOI dan klik **Periksa DOI** untuk Crossref/DataCite; jika keduanya tidak tersedia, sistem mencoba laman pendaratan DOI.
6. Koreksi skor menurut rubrik, periksa bukti, dan ekspor JSON sebagai cadangan.

## Batasan penting
- Audit dari browser **tidak dapat menjamin** setiap situs dapat dibaca karena CORS, robots, autentikasi, pembatasan layanan publik, dan throttling. Untuk hasil lebih konsisten, deploy Cloudflare Worker (petunjuk `DEPLOY-WORKER.md`). Tidak ada Worker publik bawaan yang secara pasti bisa digunakan semua orang.
- **Jina Reader adalah layanan pihak ketiga.** URL publik yang diperiksa akan dikirim ke sana jika akses langsung gagal. Jangan masukkan halaman privat atau token autentikasi.
- Pemeriksaan URL hanya memberi indikasi, bukan pembuktian mutu proses peer review, sitasi, substansi ilmiah, plagiarisme, atau sanksi. Prediksi 32 butir bersifat heuristik dan perlu verifikasi reviewer.
- Crossref HTTP 404 tidak otomatis menunjukkan DOI tidak sah. Jika halaman penerbit ditemukan, status deposit metadata tetap diberi label belum terverifikasi.
- AI generatif opsional memerlukan Worker dan API key yang disimpan di server.

## Uji penerimaan
Gunakan contoh URL `https://ejournal.uin-suka.ac.id/isoshum/profetik`. Bila audit gagal pada browser/layanan publik, aktifkan Worker sesuai panduan. Gunakan DOI `10.14421/pjk.v19i1.3089` sebagai uji pemeriksaan metadata dan halaman artikel; jangan samakan tidak adanya metadata Crossref dengan DOI invalid.


## Perbaikan DOI versi 7
- Menghapus fallback yang pernah salah membaca **Error: DOI Not Found** sebagai artikel.
- Mencocokkan DOI sumber dengan input, dan menolak sumber yang tidak memiliki judul bibliografis valid.
- Menambah indeks publik OpenAlex dan Semantic Scholar setelah Crossref/DataCite. Hasil kedua indeks bukan bukti registrasi Crossref.
- Tombol Tambahkan bukti DOI hanya aktif ketika terdapat metadata bibliografis yang cocok.
- Bila empat sumber gagal, aplikasi menampilkan **belum dapat diverifikasi**, bukan nama artikel palsu.
- Crossref, DataCite, OpenAlex, dan Semantic Scholar masing-masing memiliki ketersediaan API dan CORS yang dapat berubah; Worker tetap opsional.
