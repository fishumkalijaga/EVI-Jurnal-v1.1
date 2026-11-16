# Panduan mengaktifkan audit OJS di GitHub Pages

**Kenapa perlu Worker?** Browser tidak mengizinkan JavaScript pada GitHub Pages membaca situs OJS lain yang tidak mengirim header CORS. Tidak ada perbaikan pada HTML/JS saja yang dapat melampaui pembatasan tersebut. Cloudflare Worker bertindak sebagai perantara.

## 1. Terbitkan frontend
Unggah `index.html`, `intelligence.js`, dan `instrumen.json` ke root repository GitHub. Aktifkan GitHub Pages dari branch main/(root).

## 2. Deploy Worker publik (tanpa API key untuk audit & DOI)
1. Buat akun Cloudflare dan instal Node.js bila belum tersedia.
2. Di terminal pada folder `worker/`, jalankan `npx wrangler login`, kemudian `npx wrangler deploy`.
3. Sesuaikan `ALLOWED_ORIGIN` dalam `worker/wrangler.toml` menjadi origin alamat GitHub Pages, misalnya `https://contoh.github.io`, **bukan** `https://contoh.github.io/nama-repo/`.
4. Deploy ulang dan buka `https://nama-worker.workers.dev/health` untuk memeriksa servis (respons `ok:true`).
5. Pada aplikasi buka tab **Audit URL · DOI · PDF · AI** → **Pengaturan integrasi**, tempel endpoint `https://nama-worker.workers.dev`, klik **Simpan alamat endpoint**.
6. Masukkan URL OJS dan klik **Audit + prediksi 32 sub-unsur**.

Audit `/audit` dan metadata `/doi` tidak memerlukan token akses. Untuk membatasi penyalahgunaan endpoint publik, gunakan Cloudflare WAF/rate limiting, kuota dan pengetesan agar tidak mengirim banyak permintaan ke jurnal.

## 3. AI opsional
AI tidak dibutuhkan untuk pemindaian URL dan prediksi. Jika ingin AI, set dua secret di Cloudflare:

```bash
npx wrangler secret put OPENAI_API_KEY
npx wrangler secret put ACCESS_TOKEN
```

Jangan memasukkan secret ke file GitHub. Gunakan token panjang untuk panel AI. Model diatur melalui `OPENAI_MODEL` pada `wrangler.toml`.

## 4. Batas interpretasi
Prediksi merupakan indikator berbasis jejak laman publik, bukan keputusan akreditasi; tidak bisa membuktikan kualitas penelaahan privat, rekam jejak reviewer, mutu metodologi, sitasi riil, dan integritas akademik hanya dari URL. Seluruh skor tetap perlu diverifikasi terhadap rubrik Juknis 374/2026. Ketika metadata DOI tidak ditemukan di Crossref/DataCite, statusnya tidak boleh dianggap DOI tidak sah.
