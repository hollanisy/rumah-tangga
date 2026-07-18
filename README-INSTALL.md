# Cara Pasang "Rumah Tangga Kita" Jadi App di HP + Login Google Drive

App ini terdiri dari 5 file yang harus tetap satu folder:
`index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png`.

Ada 2 tahap: **(A) taruh online (hosting)** dan **(B) aktifkan login Google Drive**.
Kalau cuma mau tahap A dulu (tanpa Drive), tombol **"Lanjut tanpa akun"** di layar
login tetap bisa dipakai dan data tersimpan di HP masing-masing (localStorage),
mirip versi sebelumnya.

---

## A. Taruh online (wajib, supaya bisa di-install & login Google jalan)

Browser (Android maupun iPhone) hanya mengizinkan "Install App" dan login Google
kalau app diakses lewat **https://**, bukan file yang dibuka langsung dari HP.
Cara termudah dan gratis: **GitHub Pages**.

1. Buat akun GitHub gratis di github.com (kalau belum punya).
2. Buat repository baru, misal namanya `rumah-tangga`.
3. Upload ke-6 file di atas ke repository tersebut (drag & drop lewat web GitHub juga bisa).
4. Buka **Settings → Pages** pada repo tsb → pada "Branch" pilih `main` folder `/root` → Save.
5. Tunggu 1-2 menit, GitHub akan kasih alamat seperti:
   `https://namakamu.github.io/rumah-tangga/`
6. Buka alamat itu di HP. Kalau berhasil, akan muncul layar login soft hijau.

(Alternatif lain yang juga gratis & mendukung https: Netlify, Vercel, Firebase Hosting.)

---

## B. Aktifkan Login Google + Google Drive (per akun, privat)

Supaya tombol **"Masuk dengan Google"** benar-benar menyimpan data ke Drive
masing-masing akun, kamu perlu bikin **Client ID OAuth** milik sendiri (gratis,
±10 menit, sekali saja):

1. Buka **console.cloud.google.com** → login dengan akun Google kamu.
2. Klik **Buat Project Baru** (nama bebas, misal "Rumah Tangga App").
3. Di menu kiri: **APIs & Services → Library** → cari **Google Drive API** → klik **Enable**.
4. Masih di **APIs & Services → OAuth consent screen**:
   - User Type: **External**
   - Isi nama app, email kamu, dst → Save
   - Di bagian **Test users**, tambahkan email Google kamu (dan email pasangan
     kalau mau pakai akun dia juga) — supaya bisa login tanpa harus proses
     verifikasi Google yang panjang. (Kalau nanti mau dipakai banyak orang umum,
     baru perlu "Publish app".)
5. Ke **APIs & Services → Credentials** → **Create Credentials → OAuth Client ID**.
   - Application type: **Web application**
   - **Authorized JavaScript origins**, isi alamat GitHub Pages kamu dari tahap A,
     contoh: `https://namakamu.github.io` (tanpa trailing slash / nama repo)
   - Klik **Create** → akan muncul **Client ID** seperti
     `xxxxxxxxxx-xxxxxxxxxxxxxxxxxxx.apps.googleusercontent.com`
6. Salin Client ID itu. Buka file `index.html`, cari baris:
   ```js
   const GOOGLE_CLIENT_ID = 'GANTI_DENGAN_CLIENT_ID_ANDA.apps.googleusercontent.com';
   ```
   Ganti bagian dalam kutip dengan Client ID kamu, simpan file.
7. Upload ulang `index.html` yang sudah diedit ke GitHub (replace file lama).
8. Buka lagi alamat GitHub Pages kamu → coba tombol **"Masuk dengan Google"**.

Setiap kali kamu atau pasangan login dengan akun Google masing-masing, data akan
otomatis dibuatkan file privat bernama `rumah-tangga-data.json` di folder khusus
app (`appDataFolder`) pada Drive akun tersebut — file ini **tidak terlihat** di
Drive biasa dan **tidak bisa diakses akun lain**, jadi datanya privat per orang.

---

## C. Install jadi "App" di HP

**Android (Chrome):**
1. Buka alamat app-nya di Chrome.
2. Ketuk menu titik tiga (⋮) di kanan atas → **"Add to Home screen"** / **"Install app"**.
3. Ikon app akan muncul di layar utama seperti app biasa.

**iPhone (Safari):**
1. Buka alamat app-nya di Safari (harus Safari, bukan Chrome, untuk fitur ini).
2. Ketuk ikon **Share/Bagikan** (kotak dengan panah ke atas) di bawah layar.
3. Pilih **"Add to Home Screen"**.
4. Ikon app akan muncul di layar utama.

Setelah di-install, app akan tampil layar penuh tanpa address bar browser,
persis seperti app biasa, dan tetap bisa dipakai offline untuk data yang sudah
tersimpan (sinkron ke Drive otomatis kembali saat online).

---

## Catatan

- Kalau `GOOGLE_CLIENT_ID` belum diganti, tombol "Masuk dengan Google" akan
  menampilkan peringatan di layar login — app tetap bisa dipakai lewat
  "Lanjut tanpa akun" sambil kamu menyiapkan Client ID.
- Setiap akun Google yang login akan punya data Drive-nya sendiri-sendiri —
  cocok kalau kamu dan pasangan ingin akun terpisah tapi tetap satu app yang sama.
- Data juga tetap dicadangkan di penyimpanan lokal HP/PC (localStorage) per akun,
  jadi tetap tampil cepat walau sedang tidak ada internet.
