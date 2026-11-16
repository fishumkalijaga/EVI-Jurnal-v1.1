# Pengujian versi 6

Pengujian pada 16 November 2026:

- Sintaks JavaScript di `intelligence.js` dan JavaScript internal `index.html`: berhasil (`node --check`).
- Browser Chromium (Playwright) dengan mock halaman jurnal dan artikel: klik "Audit + prediksi 32 sub-unsur" menghasilkan 32 prediksi. Tanpa kesalahan JavaScript.
- CORS pada situs simulasi sengaja digagalkan, sedangkan layanan pembaca publik simulasi diizinkan: fallback berhasil.
- Crossref dan DataCite dalam pengujian mengembalikan 404; fallback halaman DOI simulasi ditemukan dan diberi status "deposit metadata belum terverifikasi".
- **Bukan uji live**: audit ke situs OJS sebenarnya, akses Jina Reader dari browser pengguna, dan Worker Cloudflare produksi belum teruji pada deployment publik. Kemungkinan ada CORS, rate limit, robots, dan kebijakan masing-masing situs.

## Catatan interpretasi
Nilai otomatis adalah estimasi heuristik dari data publik dan tidak boleh menggantikan pemeriksaan rubrik 374/2026. Perkiraan untuk sitasi, kualitas peer-review, kebaruan, mutu metodologi, rasio referensi, dan disinsentif memerlukan pemeriksaan tambahan.
