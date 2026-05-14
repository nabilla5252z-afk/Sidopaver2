# 📋 DOKUMENTASI FITUR BARU SI DOPA WEBSITE

## 🔧 Fitur yang Ditambahkan

Sistem website Si Dopa telah diupgrade dengan 3 fitur utama:

---

## 1️⃣ SISTEM ENTRY GOOGLE FORMS - PERBAIKAN

### ✅ Mekanisme Kerja:

**SEBELUMNYA:**
- Data langsung masuk Google Forms saat user klik "Buka WhatsApp"
- Data masuk WALAU user tidak jadi membeli (hanya di form)
- Banyak data pesanan palsu di spreadsheet

**SESUDAH:**
- Data HANYA masuk Google Forms SETELAH user berhasil membuka link WhatsApp
- Pesanan dicatat dengan timestamp unik
- Sistem anti-double entry mencegah pesanan duplikat dalam 5 detik

### 🛡️ Cara Kerja Anti-Double Entry:

1. **Unique Order ID** dibuat dari: `Nama Pembeli + Tanggal Ambil + Metode Pembayaran`
2. **Timestamp tracking** di localStorage browser
3. Jika user klik 2x dalam 5 detik → **Sistem tolak dengan warning**
4. User bisa pesan produk berbeda/tanggal berbeda berkali-kali (aman)

### 🔍 Cara Mengecek Sistem Ini Berjalan:

#### **Di Browser (DevTools):**
1. Buka website Si Dopa
2. Tekan **F12** (buka DevTools)
3. Klik tab **"Application"** atau **"Storage"**
4. Di sidebar pilih **"Local Storage"** → pilih domain website Anda
5. Lihat key: **`submitted_orders`** - berisi data pesanan yang sudah terkirim dengan timestamp

#### **Testing Double Entry Prevention:**
1. Isi form pesanan (nama, tempat, tanggal, metode pembayaran)
2. Klik **"WhatsApp"** → jendela WhatsApp terbuka
3. Cepat klik **"WhatsApp"** lagi dengan data yang sama → **Muncul alert**: 
   ```
   ⏱️ Pesanan ini sudah dikirim baru-baru ini. 
   Silakan tunggu beberapa detik sebelum mengirim ulang.
   ```
4. Tunggu 5 detik, baru bisa submit ulang

#### **Di Google Forms Spreadsheet:**
- Data **HANYA** muncul di sheet SETELAH pesanan dikirim ke WhatsApp
- Di console (F12 → Console), akan muncul log:
  ```
  ✅ PESANAN BERHASIL MASUK KE SISTEM!
  ✅ Data yang dikirim:
    - Nama Pembeli: [nama]
    - Tempat Ambil: [lokasi]
    ... dst
  ```

### ⚠️ Perbedaan Penting:

| Aspek | Sebelumnya | Sesudah |
|-------|-----------|--------|
| **Waktu Submit** | Saat klik "WhatsApp" | **SETELAH jendela WhatsApp terbuka** |
| **Data Invalid** | Banyak (user batal beli) | **Minimal (hanya yang buka WhatsApp)** |
| **Double Entry** | Bisa terjadi | **Dicegah 5 detik** |
| **Clear Cart** | Tidak | **Otomatis cleared setelah submit sukses** |

---

## 2️⃣ SISTEM STOK PRODUK

### 📦 Fitur Unggulan:

✅ **Tampilkan stok** di setiap varian produk  
✅ **Admin control** tanpa edit kode  
✅ **Disable otomatis** jika stok habis  
✅ **Real-time validation** saat checkout  
✅ **Persistent storage** di browser customer  

### 🎮 Cara Admin Mengatur Stok:

#### **Step 1: Akses Admin Panel**
1. Refresh halaman website
2. Cari **"🔐 Admin Access"** (di bawah kotak Pre Order Info)
3. Klik tulisan kecil tersebut
4. Input password: **`sidopa2026`** → Klik OK

#### **Step 2: Masuk Tab Stok**
- Admin Panel terbuka dengan 2 tab: **"📦 Stok"** dan **"📅 Pre Order"**
- Klik tab **"📦 Stok"** (default terbuka)

#### **Step 3: Ubah Stok**
- Terlihat 3 field untuk setiap varian:
  ```
  Si' Ori (Rp 5.000)
  [input] ← Masukkan jumlah stok
  
  Si' Nori (Rp 5.000)
  [input]
  
  Si' Spicy (Rp 5.500)
  [input]
  ```

#### **Step 4: Simpan & Selesai**
- Klik tombol **"💾 Simpan"**
- Alert: `✅ Data berhasil disimpan!`
- Stok langsung terupdate di halaman utama

### 🛍️ Tampilan Stok untuk Customer:

Setiap varian menampilkan stok di bawah benefit tag:
```
✓ Menghilangkan Bosan dengan Renyahnya Tekstur Original🤩
🟢 Stok: 25        ← Hijau (tersedia)

atau jika habis:

🔴 Stok: 0         ← Merah (habis)
❌ Stok Habis      ← Tombol berubah jadi abu-abu & disabled
```

### 🔒 Validasi Pembelian Otomatis:

**Jika stok = 0:**
1. ❌ Tombol "Tambah ke Keranjang" jadi **DISABLED** (tidak bisa diklik)
2. 🔴 Warna badge stok berubah **MERAH**
3. Tombol menampilkan: `❌ Stok Habis`
4. Customer melihat jelas produk tidak tersedia

**Jika customer membuka 2 tab sekaligus:**
- Sistem cek stok **ULANG** saat checkout
- Jika stok kurang → alert warning
- Data Google Forms tetap aman (tidak masuk)

### 🔍 Cara Mengecek Stok Berjalan:

#### **Method 1: Visual Check**
1. Buka website → lihat badge stok di setiap varian
2. Ubah stok via admin panel
3. **Refresh halaman** → stok terupdate

#### **Method 2: DevTools Check**
1. Buka F12 → **"Application"** → **"Local Storage"**
2. Cari keys:
   - `stock_ori` → Stok Si' Ori
   - `stock_nori` → Stok Si' Nori  
   - `stock_spicy` → Stok Si' Spicy
3. Nilai ditampilkan dalam format angka

#### **Method 3: Functional Test**
1. Set stok Si' Ori = **0**
2. Tombol "Tambah ke Keranjang" Si' Ori jadi **DISABLED**
3. Set stok Si' Ori = **5**
4. Tombol Si' Ori **AKTIF** lagi

### ❓ Apakah Stok Berpengaruh pada Google Forms?

**JAWAB: TIDAK BERDAMPAK NEGATIF**

- ✅ Stok hanya **UI validation** (validasi tampilan)
- ✅ Customer tidak bisa add to cart jika stok 0
- ✅ Google Forms tetap menerima data yang valid
- ⚠️ **PENTING**: Admin harus pantau stok manual di admin panel
- ⚠️ Jika stok berkurang, update manual di admin panel

**REKOMENDASI:**
- Update stok SEBELUM pre-order dibuka
- Pantau stok setiap hari during pre-order period
- Kurangi stok saat customer order masuk

---

## 3️⃣ FITUR INFO PRE-ORDER

### 📅 Informasi yang Ditampilkan:

Kotak Pre-Order Info menampilkan **3 informasi penting**:

```
┌─────────────────────────────────────────┐
│   📅 Informasi Pre Order Si Dopa        │
├─────────────────────────────────────────┤
│ Periode Pre Order   │ Tanggal Ready     │
│ 01 Jun - 05 Jun     │ 08 Jun 2025       │ 
│                     │                   │
│     Jam Ambil       │                   │
│     14:00 - 17:00   │                   │
└─────────────────────────────────────────┘
```

### 🎯 Fungsi:

1. **Periode Pre Order** → Kapan customer bisa order
2. **Tanggal Ready** → Kapan produk siap
3. **Jam Ambil** → Jam operasional pengambilan

### 🔐 Cara Admin Mengatur Pre Order Info:

#### **Step 1: Akses Admin Panel**
- Klik **"🔐 Admin Access"** di bawah kotak pre-order
- Input password: **`sidopa2026`**

#### **Step 2: Klik Tab Pre Order**
- Admin Panel sudah terbuka
- Klik tab **"📅 Pre Order"** (di sebelah tab Stok)

#### **Step 3: Isi 4 Field**

```
Tanggal Pre Order Dimulai        Tanggal Pre Order Berakhir
[datepicker] Pilih tanggal       [datepicker] Pilih tanggal
Contoh: 2025-06-01              Contoh: 2025-06-05

Tanggal Produk Ready            Jam Ambil
[datepicker] Pilih tanggal      [timepicker] Format HH:MM
Contoh: 2025-06-08              Contoh: 14:00
```

#### **Step 4: Simpan**
- Klik **"💾 Simpan"**
- Info langsung tampil di kotak Pre Order Info

### 📍 Lokasi Pre Order Info:

```
┌─ Header Si Dopa ─────────────────────┐
│  Logo & Deskripsi Produk             │
└──────────────────────────────────────┘
            ↓
┌─ Hero Section ───────────────────────┐
│  "Bukan hanya camilan..." dst         │
└──────────────────────────────────────┘
            ↓
┌─ PRE-ORDER INFO BOX ──────────────────┐  ← DI SINI!
│  📅 Informasi Pre Order Si Dopa      │
│  Periode | Ready | Jam Ambil         │
└──────────────────────────────────────┘
            ↓
┌─ Products & Cart ─────────────────────┐
│  Si' Ori, Si' Nori, Si' Spicy         │
└──────────────────────────────────────┘
```

### 🔍 Cara Mengecek Fitur Pre Order:

#### **Method 1: Buka Admin & Set Data**
1. Klik "🔐 Admin Access"
2. Password: `sidopa2026`
3. Tab Pre Order → Isi semua field
4. Klik Simpan → Info terupdate

#### **Method 2: DevTools Check**
1. F12 → Application → Local Storage
2. Cari keys:
   - `preorder_start` → Tanggal mulai
   - `preorder_end` → Tanggal berakhir
   - `product_ready` → Tanggal ready
   - `pickup_time` → Jam ambil

#### **Method 3: Format Check**
Format yang benar:
- Tanggal: **YYYY-MM-DD** (contoh: 2025-06-01)
- Waktu: **HH:MM** (contoh: 14:00)

Jika format salah → tampil **"-"** (belum tersimpan)

### 💡 Use Case:

**Contoh Kasus:**
- Pre Order dimulai: 1 Juni
- Pre Order ditutup: 5 Juni  
- Produk ready: 8 Juni
- Jam ambil: 14:00 - 17:00

Customer melihat dan tahu:
- ✅ Bisa order 1-5 Juni
- ✅ Ambil 8 Juni jam 2 sore - 5 sore
- ✅ Tidak ada kekeliruan tanggal pengambilan

---

## 🔑 PASSWORD KEAMANAN

**Password Admin Panel (sama untuk Stok & Pre Order):**
```
sidopa2026
```

⚠️ **PENTING:** Jangan share password ini ke customer!

---

## 📊 Data Storage (LocalStorage)

Semua data tersimpan di **browser customer** dan **admin** menggunakan localStorage:

### **Keys yang Digunakan:**

| Key | Fungsi | Contoh Value |
|-----|--------|--------------|
| `stock_ori` | Stok Si' Ori | `25` |
| `stock_nori` | Stok Si' Nori | `30` |
| `stock_spicy` | Stok Si' Spicy | `20` |
| `preorder_start` | Tanggal mulai pre-order | `2025-06-01` |
| `preorder_end` | Tanggal tutup pre-order | `2025-06-05` |
| `product_ready` | Tanggal produk ready | `2025-06-08` |
| `pickup_time` | Jam ambil | `14:00` |
| `submitted_orders` | Data pesanan terkirim | `{"nama_tgl_metode": 1654321098}` |

### 🔄 Persistent atau Per-Session?

- ✅ **Persistent** - Data tersimpan sampai browser dihapus cache
- ⚠️ Jika customer hapus browser cache → data reset ke default
- ✅ Admin bisa set ulang kapan saja via admin panel

---

## 🚀 WORKFLOW LENGKAP

### **Senario: Pre-Order Baru**

**Minggu 1 (Admin Setup):**
1. Login admin panel → set stok awal
2. Set periode pre-order
3. Publish website

**Minggu 1-2 (Customer Order):**
1. Customer lihat kotak pre-order info
2. Tahu kapan pre-order dibuka/tutup
3. Lihat jam ambil
4. Pilih produk & tambah ke keranjang
5. Klik WhatsApp → data masuk Google Forms
6. Anti-double-entry mencegah pesanan ganda

**Minggu 3 (Sebelum Ready Date):**
1. Admin update stok (kurang seiring pesanan masuk)
2. Jika stok habis → set ke 0
3. Tombol customer auto-disable

**Minggu 3-4 (Ready & Pickup):**
1. Stok siap
2. Customer datang ambil sesuai jam yang ditampilkan
3. Transaksi selesai

---

## ⚙️ TROUBLESHOOTING

### ❓ Stok tidak terupdate?
- Refresh halaman (F5)
- Clear cache browser (Ctrl+Shift+Delete)
- Check DevTools → Local Storage

### ❓ Password tidak bekerja?
- **Pastikan huruf besar-kecil benar**: `sidopa2026`
- Jangan pake spasi di depan/belakang
- Coba di incognito window (jika browser hang)

### ❓ Double entry tetap terjadi?
- Sistem block 5 detik, tunggu
- Data masuk Google Forms sekali saja
- Check spreadsheet untuk validasi

### ❓ Pre Order Info blank ("-")?
- Belum set data di admin panel
- Format tanggal salah (harus YYYY-MM-DD)
- Clear cache → set ulang

### ❓ Google Forms tidak terima data?
- Check internet connection
- Formasi entry ID benar di script
- Open DevTools → Console → lihat error
- WhatsApp window harus berhasil terbuka

---

## 📞 SUPPORT INFO

Jika ada masalah:
1. **Check DevTools Console** (F12) untuk error message
2. **Screenshot error** untuk dokumentasi
3. **Test di incognito window** untuk isolasi cache issue
4. **Hubungi developer** dengan detail masalahnya

---

**Last Updated:** May 2025  
**Version:** 2.0 - Stok + Pre-Order + Double-Entry Prevention  
**Status:** ✅ Production Ready

