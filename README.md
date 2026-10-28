# Cari Jawaban Soal — versi GitHub Pages (hosting statis, tanpa server)

Bank soal **17.751 soal** (data lama tidak diubah + 780 soal baru dari 10 file Excel terbaru), login password tim,
panel Admin untuk mengganti password, baca 1–50 soal sekaligus, soal dari gambar, unggah Excel, referensi AI.

## A. Pasang di GitHub Pages (±5 menit)
1. Buka https://github.com → **New repository** → nama bebas (mis. `cari-soal`) → **Public** → Create.
2. Di halaman repo klik **uploading an existing file**, lalu **tarik ISI folder ini** (bukan zip-nya): `index.html`, folder `assets`, folder `data`, folder `tools`, folder `test`, `README.md`, dan `.nojekyll`.
   Pastikan folder `data` ikut (isinya `bank.js` dan `keys.js`). Klik **Commit changes**.
3. **Settings → Pages** → *Build and deployment* → Source: **Deploy from a branch** → Branch: `main` / folder `/ (root)` → **Save**.
4. Tunggu 1–2 menit. Alamatnya: `https://USERNAME.github.io/NAMA-REPO/`.

## B. Login
- **Tim:** Username (boleh dikosongkan) + Password. Huruf besar/kecil password tim tidak dibedakan.
- **Admin:** klik “Masuk sebagai Admin” → email admin + password admin.
- Sesi aktif **maksimal 2 jam**, lalu otomatis keluar dan harus login lagi. Tidak keluar sendiri sebelum itu. Berlaku sama di HP mana pun.
- Password awal diberikan terpisah oleh pembuat (tidak ditulis di repo). **Admin awal harus segera ganti password admin** (Panel Admin → Password admin).

## C. Ganti password tim (Panel Admin)
Login Admin → tombol **⚙️ Admin**:
1. **Akun tim**: ketik password baru lalu **Ganti**; bisa **Tambah akun** / **Hapus**.
2. **Terbitkan ke GitHub** → isi *token* → klik **Terbitkan akun & password**. Situs diperbarui ±1 menit; setelah itu password lama mati.
   - Membuat token: GitHub → Settings → Developer settings → Personal access tokens → **Fine-grained tokens** → Generate →
     Repository access: *Only select repositories* → pilih repo ini → Permissions: **Contents: Read and write** → Generate. Tempel di kolom Token
     (hanya disimpan di tab itu, hilang saat tab ditutup).
   - Tanpa token: klik **Unduh keys.js**, lalu di repo ganti file `data/keys.js` (Add file → Upload files).
3. **Rotasi penuh** (bagian bawah tab itu): dipakai bila password lama sudah bocor. Membuat kunci data baru, mengenkripsi ulang bank soal,
   dan memaksa semua password diganti. Perubahan password biasa **tidak** mencabut akses orang yang sudah menyimpan file `keys.js` lama.

## D. Menambah soal
- **Semua orang:** kotak “Tambah soal dari Excel” (XLSX, XLS, XLSM, XLSB, CSV, ODS, ZIP). Tersimpan **di perangkat itu saja**.
  Soal yang sudah ada dilewati (isi lama tidak diubah).
- **Agar semua orang melihatnya:** Admin → Terbitkan ke GitHub → **Terbitkan data soal** (atau Unduh bank.js lalu ganti `data/bank.js`).

## E. Soal dari gambar & Referensi AI
Tombol “Referensi AI → Pengaturan”: pilih Gemini/Claude dan tempel API key Anda (disimpan di perangkat, dikirim langsung ke penyedia AI).
Tanpa key, gambar dibaca dengan OCR di browser (Tesseract, perlu internet, hasil lebih kasar).

## F. Keamanan — baca ini
Hosting statis tidak punya server, jadi pengamanan memakai **enkripsi**: file `data/bank.js` terenkripsi AES-256; password membuka kuncinya
(PBKDF2 600.000 putaran). Isi soal tidak terbaca tanpa password. Tetapi file terenkripsi itu publik, jadi orang yang menebak password secara offline
tidak dibatasi percobaan — **pakai password yang panjang & sulit ditebak** (mis. 12+ karakter). Password pendek seperti angka+nama mudah ditebak.
Kode email sekali pakai tidak mungkin tanpa server, karena itu 3 email dipakai sebagai identitas **admin** (email + password admin).

## G. Membangun ulang data dari nol (opsional)
`CS_TEAM="shanum:SHANUM:sandi1;balqis:BALQIS:sandi2" CS_ADMIN_EMAILS="a@x.com,b@y.com" CS_ADMIN_PASSWORD="sandiadmin" node tools/build.mjs entries.json "label"`
Uji: `npm i jsdom xlsx fflate && node --test test/app.test.mjs`.
