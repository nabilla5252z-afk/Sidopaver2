# ❓ FAQ & TROUBLESHOOTING SI DOPA v2.0

## 📌 PERTANYAAN UMUM

### 1️⃣ SISTEM ENTRY GOOGLE FORMS

#### Q: Apa perbedaan sistem lama vs baru?
**A:**
- **Lama:** Data langsung submit saat klik "WhatsApp" (walau customer belum buka WhatsApp)
- **Baru:** Data hanya submit SETELAH customer berhasil membuka link WhatsApp
- **Hasil:** Spreadsheet lebih bersih, tidak ada fake orders

#### Q: Bagaimana caranya prevent double order?
**A:**
Sistem membuat **Unique Order ID** dari 3 parameter:
1. Nama Pembeli
2. Tanggal Ambil
3. Metode Pembayaran

Jika urutan ketiga parameter ini **sama persis** dalam **5 detik**:
- ❌ Sistem BLOCK dengan warning: `⏱️ Pesanan ini sudah dikirim...`
- ✅ Customer bisa pesan ulang setelah 5 detik
- ✅ Customer bisa pesan produk berbeda/tanggal berbeda kapan saja

#### Q: Apakah data masih aman jika ada 2 order dalam 5 detik?
**A:** 
✅ **YA, AMAN!** Hanya pesanan pertama yang masuk Google Forms
- Pesanan kedua di-block oleh sistem
- Tidak ada data duplikat di spreadsheet
- Log di console akan mencatat attempt

#### Q: Bagaimana jika customer buka di 2 tab browser?
**A:**
- Tab 1: Order (data masuk)
- Tab 2: Order dalam 5 detik (BLOCKED)
- Jika tunggu 5 detik di Tab 2: Boleh submit
- Sistem track berdasarkan localStorage (per-device, per-browser)

#### Q: Bagaimana jika customer ganti perangkat (HP vs Laptop)?
**A:**
- Di HP: Bisa order sebanyak sesuai unique ID (tidak sharing data)
- Di Laptop: Bisa order sebanyak sesuai unique ID
- Sistem tidak block karena **localStorage terpisah per device**
- **Admin harus pantau spreadsheet** untuk detect manual double order cross-device

---

### 2️⃣ SISTEM STOK PRODUK

#### Q: Dimana data stok disimpan?
**A:**
Data stok disimpan di **localStorage browser** (per-browser, per-device):
- Jika admin ubah stok di browser A → hanya A yang terupdate
- Browser B tetap punya cache lama
- Solusi: **Refresh browser** atau **Clear cache**

#### Q: Apakah stok bisa disinkronisasi antar perangkat?
**A:**
❌ **TIDAK** - saat ini menggunakan localStorage lokal
- Stok hanya synchronized dalam 1 browser
- Jika admin ubah stok di laptop, customer di HP tidak otomatis terupdate
- **Workaround:** Customer buka incognito / clear cache

**Note:** Bisa upgrade ke **Cloud Database** di masa depan untuk real-time sync

#### Q: Jika stok habis, apakah customer bisa lihat ?
**A:**
✅ **YA, JELAS TERLIHAT:**
- Badge berubah **MERAH**
- Text: `🔴 Stok: 0`
- Tombol "Tambah ke Keranjang" jadi **DISABLED** (abu-abu)
- Tidak bisa diklik

#### Q: Bagaimana jika admin lupa update stok?
**A:**
⚠️ **RISIKO:** Customer bisa order product yang sudah habis

**Preventive:**
1. Set stok SEBELUM pre-order dibuka
2. Update stok setiap hari
3. Cek Google Forms untuk lihat berapa yang sudah dipesan
4. Kurangi stok: `Stok Awal - Pesanan Masuk = Stok Sisa`

**Contoh:**
```
Si' Ori awal: 100
Pesanan: 25
Sisa: 75
Admin update stok = 75
```

#### Q: Apakah stok berpengaruh pada Google Forms?
**A:**
✅ **TIDAK BERDAMPAK NEGATIF:**
- Stok hanya **UI validation** (tampilan)
- Google Forms tetap menerima data
- **TAPI:** Admin harus validasi manual di spreadsheet
- Jika order melebihi stok, **admin harus decline** via WhatsApp

#### Q: Bagaimana jika customer buka 2 tab dan order melebihi stok?
**A:**
Misal: Stok Si' Ori = 5
- Tab A: order 3 (keranjang menampilkan 3)
- Tab B: order 3 (keranjang menampilkan 3)
- Total: 6 (melebihi stok 5!)

**Sistem tidak bisa prevent ini** karena stok di localStorage per-device
**Solusi:** Admin harus pantau Google Forms dan **decline via WhatsApp** jika melebihi

---

### 3️⃣ FITUR PRE ORDER INFO

#### Q: Dimana tempat pre-order info ditampilkan?
**A:**
Tepat setelah hero section:
```
Header
    ↓
Hero ("Bukan hanya camilan...")
    ↓
📅 PRE-ORDER INFO BOX ← DI SINI
    ↓
Produk & Keranjang
```

#### Q: Informasi apa saja yang ditampilkan?
**A:**
Kotak pre-order info menampilkan:
1. **Periode Pre Order** → Kapan pre order dibuka-tutup
2. **Tanggal Produk Ready** → Kapan produk siap
3. **Jam Ambil** → Jam operasional pengambilan

#### Q: Bagaimana format tanggal yang benar?
**A:**
**FORMAT HARUS:** `YYYY-MM-DD` (ISO format)

Contoh BENAR:
- `2025-06-01` ✅ (1 Juni 2025)
- `2025-06-30` ✅ (30 Juni 2025)

Contoh SALAH:
- `01-06-2025` ❌ (format tanggal Indonesia)
- `06/01/2025` ❌ (format tanggal US)
- `1 Juni 2025` ❌ (text)

**Sistem akan menampilkan "-" jika format salah**

#### Q: Bagaimana format jam yang benar?
**A:**
**FORMAT HARUS:** `HH:MM` (24-jam format)

Contoh BENAR:
- `14:00` ✅ (2 sore)
- `09:30` ✅ (9 pagi 30 menit)
- `00:00` ✅ (12 malam)
- `23:59` ✅ (11 malam 59 menit)

Contoh SALAH:
- `2:00 PM` ❌ (12-jam format)
- `14.00` ❌ (dot separator)
- `1400` ❌ (no separator)

#### Q: Apakah tanggal ambil (date picker form) ter-sync dengan pre-order info?
**A:**
❌ **TIDAK otomatis ter-sync**

**Pre-order info** = Info edukatif untuk customer
**Date picker** = Opsi yang bisa dipilih customer

**Rekomendasi:**
- Set **Jam Ambil** sebagai informasi saja (contoh: 14:00 - 17:00)
- Di date picker, customer bisa pilih tanggal yang sudah ready
- Admin harus **manually restrict** tanggal yang tidak ready

**Future upgrade:** Bisa integrate keduanya secara otomatis

#### Q: Bagaimana jika pre-order info belum diset?
**A:**
Kotak akan menampilkan:
```
Periode Pre Order: -
Tanggal Ready Produk: -
Jam Ambil: -
```

Tidak ada masalah, customer tetap bisa order.
**Rekomendasi:** Set data PRE-ORDER INFO sebelum announce pre order kepada customer

---

## 🐛 TROUBLESHOOTING

### STOK ISSUES

#### ❌ Stok tidak muncul di halaman
**Solusi:**
1. Refresh halaman (F5)
2. Clear browser cache:
   - Windows: Ctrl+Shift+Delete
   - Mac: Cmd+Shift+Delete
3. Buka incognito/private window (test apakah cache issue)

#### ❌ Admin ubah stok tapi tidak terupdate
**Solusi:**
1. Pastikan klik "💾 Simpan" (jangan lupa)
2. Check alert: muncul `✅ Data berhasil disimpan!` ?
3. Refresh halaman (F5) untuk lihat perubahan
4. Check DevTools (F12) → Application → LocalStorage → stock_ori, stock_nori, stock_spicy

#### ❌ Tombol "Tambah ke Keranjang" tetap aktif padahal stok 0
**Solusi:**
1. Admin belum set stok = 0 (check lagi)
2. Refresh halaman
3. Clear localStorage:
   - Open DevTools (F12)
   - Application → Local Storage → [domain] → hapus keys stock_*
   - Set stok ulang dari admin panel

#### ❌ Stok reset setelah close browser
**Ini NORMAL** jika:
- Browser crash
- Clear cache secara paksa
- Browser privacy mode (data tidak persistent)

**Solusi:** Set stok ulang dari admin panel

---

### ADMIN PANEL ISSUES

#### ❌ Admin panel tidak membuka meskipun password benar
**Solusi:**
1. Refresh halaman
2. Password **CASE SENSITIVE**: `sidopa2026` (huruf kecil semua)
3. Tidak boleh ada space di depan/belakang
4. Coba di incognito window
5. Check DevTools Console (F12 → Console) untuk error

#### ❌ Data admin panel tidak tersimpan
**Solusi:**
1. Pastikan klik tombol "💾 Simpan" (bukan "Tutup")
2. Alert harus muncul: `✅ Data berhasil disimpan!`
3. Check browser privacy/security setting (bisa block localStorage)
4. Coba di incognito window

#### ❌ Tab stok/pre-order tidak bisa switch
**Solusi:**
1. Refresh halaman
2. Admin panel close dan buka ulang
3. Check DevTools Console untuk error

---

### PRE-ORDER INFO ISSUES

#### ❌ Pre-order info blank ("-")
**Penyebab & Solusi:**
- **Admin belum set data** → Set via admin panel
- **Format tanggal salah** → Gunakan YYYY-MM-DD
- **Format jam salah** → Gunakan HH:MM
- **Browser cache** → Clear cache & refresh

#### ❌ Tanggal menampilkan angka/format aneh
**Solusi:**
1. Check format di admin panel
2. Format harus: `YYYY-MM-DD`
3. Hapus & set ulang
4. Refresh halaman

#### ❌ Jam menampilkan "-" padahal sudah diset
**Penyebab:**
- Format jam tidak sesuai (harus HH:MM)
- Cache belum ter-update

**Solusi:**
1. Admin panel → Ubah jam format (contoh: 14:00)
2. Simpan
3. Refresh halaman (F5)

---

### GOOGLE FORMS ISSUES

#### ❌ Data tidak masuk Google Forms
**Penyebab:**
- Internet mati/lambat
- Popup browser terblokir
- Form ID salah
- Entry ID salah

**Solusi:**
1. Check internet connection (buka website lain)
2. Allow popup: Settings → Privacy/Security → Popup blocker OFF
3. Buka DevTools (F12 → Console) → cari error message
4. Test di incognito (jika cache issue)
5. Hubungi developer jika persistent

#### ❌ WhatsApp tidak buka
**Penyebab:**
- Popup blocker aktif
- Nomor WhatsApp salah di script
- Internet issue

**Solusi:**
1. Disable popup blocker untuk website ini
2. Check DevTools Console untuk link yang dibuka
3. Manual copy-paste link ke browser address bar
4. Install aplikasi WhatsApp di device

#### ❌ Data masuk Google Forms tapi format aneh
**Penyebab:**
- Entry ID salah
- Data tidak ter-escape dengan benar

**Solusi:**
1. Check console log format
2. Hubungi developer untuk debug entry IDs

---

### DOUBLE-ENTRY PREVENTION ISSUES

#### ❌ Double entry tetap terjadi (data masuk 2x)
**Penyebab:**
- 5 detik belum habis (tunggu sebelum test)
- localStorage dihapus
- Device/browser berbeda

**Solusi:**
1. Tunggu 5 detik antar order
2. Cek localStorage di DevTools: key `submitted_orders`
3. Device berbeda = tidak sharing data (admin harus monitor)
4. Clear localStorage dan test ulang

#### ❌ Blockade tidak terjadi padahal order sama
**Penyebab:**
- Belum 5 detik (tapi mungkin data nya berbeda)
- Order ID berbeda (nama/tanggal/metode berbeda)

**Validation order ID:**
```
Order ID = Nama + Tanggal + Metode

Contoh SAMA:
- User A, 2025-06-01, Qris → SAMA (akan diblock)
- User A, 2025-06-01, Qris → SAMA (akan diblock)

Contoh BEDA (diizinkan):
- User A, 2025-06-01, Qris → Beda metode
- User A, 2025-06-01, Cash → (akan diizinkan)
```

---

### RESPONSIVE DESIGN ISSUES

#### ❌ Pre-order box tidak centered di mobile
**Solusi:**
1. Check CSS media queries
2. Resize browser untuk test responsiveness
3. Clear cache & refresh

#### ❌ Admin panel modal terlalu besar di mobile
**Ini EXPECTED** tapi bisa di-scroll
- Mobile screen kecil, modal ada scrollbar
- Content semua bisa diakses dengan scroll

---

## 🔍 DEBUGGING DENGAN DEVTOOLS

### Cara Check Stok:
```
1. F12 → Application tab
2. Left sidebar → Local Storage
3. Pilih domain website Anda
4. Cari keys: stock_ori, stock_nori, stock_spicy
5. Lihat valuenya
```

### Cara Check Pre-order Info:
```
1. F12 → Application tab
2. Local Storage → domain
3. Cari keys: preorder_start, preorder_end, product_ready, pickup_time
4. Lihat valuenya
```

### Cara Check Double Entry Log:
```
1. F12 → Application tab
2. Local Storage → domain
3. Cari key: submitted_orders
4. Value adalah JSON object dengan timestamp
5. Format: {"nama_tanggal_metode": 1654321098}
```

### Cara Check Google Forms Submit Log:
```
1. F12 → Console tab
2. Order sesuatu
3. Lihat output:
   ✅ PESANAN BERHASIL MASUK KE SISTEM!
   ✅ Data yang dikirim:
   ...
4. Jika error, muncul message merah ❌
```

---

## 📞 WHEN TO CONTACT DEVELOPER

Hubungi developer jika:

1. **Console error tidak resolve:** Screenshot error + rekam video
2. **Data tidak masuk Google Forms** (sudah test semua solusi)
3. **Browser kompatibilitas issue** (spesifik browser/device)
4. **Performa issue** (web sangat lambat)
5. **Security issue** (ada yang suspicious)
6. **Feature request** (upgrade stok system ke cloud)

**Informasi yang diperlukan:**
- Screenshot/video masalahnya
- Browser & OS yang digunakan
- DevTools Console output (jika ada error)
- Langkah reproduksi (step-by-step)
- Sudah dicoba solusi apa saja

---

**FAQ Version:** 2.0  
**Last Updated:** May 2025  
**Maintained By:** [Developer Name]

