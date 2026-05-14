# 🧪 PANDUAN TESTING CEPAT SI DOPA v2.0

## ✅ Pre-Testing Checklist

Sebelum launch, pastikan sudah test:

- [ ] Sistem Stok
- [ ] Admin Panel Password
- [ ] Pre-Order Info Box
- [ ] Google Forms Entry (Double-Entry Prevention)
- [ ] Responsive Design
- [ ] WhatsApp Integration
- [ ] Anti-Double Entry System

---

## 1️⃣ TEST SISTEM STOK

### Test Case 1.1: Tampilkan Stok di Varian
```
✓ Buka website
✓ Lihat setiap varian produk
✓ VERIFY: Muncul badge "Stok: X" di bawah benefit tag (warna hijau)
```

### Test Case 1.2: Admin Ubah Stok
```
✓ Klik "🔐 Admin Access" di bawah pre-order box
✓ Input password: sidopa2026
✓ Admin Panel membuka
✓ Ubah Si' Ori menjadi 5
✓ Ubah Si' Nori menjadi 0
✓ Ubah Si' Spicy menjadi 3
✓ Klik "💾 Simpan"
✓ VERIFY: Alert "✅ Data berhasil disimpan!"
✓ VERIFY: Badge stok terupdate di halaman utama
✓ VERIFY: Si' Nori badge berwarna MERAH dengan text "Stok: 0"
```

### Test Case 1.3: Disable Tombol Jika Stok 0
```
✓ Dari Test 1.2, Si' Nori sudah set stok = 0
✓ Lihat tombol "Tambah ke Keranjang" Si' Nori
✓ VERIFY: Tombol jadi ABU-ABU dan DISABLED
✓ VERIFY: Text berubah jadi "❌ Stok Habis"
✓ VERIFY: Tidak bisa diklik (cursor jadi not-allowed)
✓ Coba klik Si' Ori (stok 5) → VERIFY: Tombol masih AKTIF
✓ Coba klik Si' Spicy (stok 3) → VERIFY: Tombol masih AKTIF
```

### Test Case 1.4: Refresh Halaman - Stok Persisten
```
✓ Set stok dari Test 1.2 sudah ada
✓ Tekan F5 (Refresh halaman)
✓ VERIFY: Stok tetap sama (tidak reset)
✓ VERIFY: Si' Nori masih 0 dan tombol masih disabled
```

### Test Case 1.5: Tambah ke Keranjang dengan Stok Valid
```
✓ Si' Ori stok 5
✓ Klik dropdown qty, ubah jadi 3
✓ Klik "Tambah ke Keranjang"
✓ VERIFY: Item muncul di keranjang dengan qty 3
✓ VERIFY: Tidak ada error
```

---

## 2️⃣ TEST PRE-ORDER INFO BOX

### Test Case 2.1: Tampilan Default
```
✓ Buka website
✓ Scroll ke bawah hero section
✓ VERIFY: Muncul kotak putih dengan border oranye
✓ VERIFY: Judul "📅 Informasi Pre Order Si Dopa"
✓ VERIFY: 3 kolom: Periode, Ready, Jam Ambil
✓ VERIFY: Semua isi menampilkan "-" (belum ada data)
✓ VERIFY: Ada text kecil "🔐 Admin Access" di bawah (clickable)
```

### Test Case 2.2: Admin Set Pre-Order Info
```
✓ Klik "🔐 Admin Access"
✓ Input password: sidopa2026
✓ VERIFY: Admin Panel terbuka
✓ VERIFY: 2 tab: "📦 Stok" dan "📅 Pre Order"
✓ Klik tab "📅 Pre Order"
✓ Isi field:
  - Tanggal Pre Order Dimulai: 2025-06-01
  - Tanggal Pre Order Berakhir: 2025-06-05
  - Tanggal Produk Ready: 2025-06-08
  - Jam Ambil: 14:00
✓ Klik "💾 Simpan"
✓ VERIFY: Alert "✅ Data berhasil disimpan!"
✓ Close admin panel (klik "Tutup")
```

### Test Case 2.3: Verify Pre-Order Info Terupdate
```
✓ Dari Test 2.2, admin sudah set data
✓ Scroll ke kotak pre-order info
✓ VERIFY: Periode menampilkan "Sun, 01 Jun 2025 s/d Fri, 05 Jun 2025"
✓ VERIFY: Ready menampilkan "Sun, 08 Jun 2025"
✓ VERIFY: Jam Ambil menampilkan "14:00"
```

### Test Case 2.4: Format Validasi
```
✓ Klik "🔐 Admin Access", password: sidopa2026
✓ Klik tab "📅 Pre Order"
✓ Isi SALAH:
  - Tanggal Pre Order Dimulai: 01/06/2025 (salah format, harus YYYY-MM-DD)
✓ Klik "💾 Simpan"
✓ Close panel
✓ VERIFY: Periode menampilkan "-" (format salah ditolak)
✓ REPEAT dengan format benar: 2025-06-01
✓ VERIFY: Sekarang muncul tanggal dengan benar
```

---

## 3️⃣ TEST ADMIN PANEL SECURITY

### Test Case 3.1: Password Salah
```
✓ Klik "🔐 Admin Access"
✓ Input password: salahpassword
✓ VERIFY: Alert "❌ Password salah!"
✓ Admin Panel tidak membuka
```

### Test Case 3.2: Password Benar
```
✓ Klik "🔐 Admin Access"
✓ Input password: sidopa2026 (TEPAT sama, case-sensitive)
✓ VERIFY: Admin Panel membuka dengan mulus
```

### Test Case 3.3: Tab Switching
```
✓ Admin Panel terbuka
✓ Klik tab "📦 Stok"
✓ VERIFY: Stock input fields terlihat
✓ Klik tab "📅 Pre Order"
✓ VERIFY: Pre-order input fields terlihat
✓ Stock fields disembunyikan
✓ Klik tab "📦 Stok" lagi
✓ VERIFY: Pre-order fields disembunyikan, Stock fields muncul
```

---

## 4️⃣ TEST GOOGLE FORMS INTEGRATION (DOUBLE-ENTRY PREVENTION)

### Test Case 4.1: Normal Order Flow
```
✓ Buka website di browser BARU (atau incognito window)
✓ Tambah produk ke keranjang (misal: 2x Si' Ori)
✓ Isi form:
  - Nama: Test User 001
  - Tempat Ambil: Taman STID
  - Tanggal Ambil: [pilih tanggal besok]
  - Metode: Qris/Transfer Bank
✓ Klik "🛒 WhatsApp"
✓ VERIFY: Jendela WhatsApp terbuka dengan pesan terformat
✓ Tunggu 2 detik
✓ VERIFY: Console (F12 → Console) menampilkan:
  ✅ PESANAN BERHASIL MASUK KE SISTEM!
  ✅ Data yang dikirim:
    - Nama Pembeli: Test User 001
    - Si' Ori: 2 pcs
    ... dst
✓ VERIFY: Alert: "✅ Pesanan berhasil dikirim!"
✓ VERIFY: Keranjang dikosongkan otomatis
✓ VERIFY: Form fields dikosongkan otomatis
```

### Test Case 4.2: Double Entry Prevention (Dalam 5 Detik)
```
✓ SETUP: Dari Test 4.1 hasil order belum 5 detik berlalu
✓ Isi form lagi dengan DATA SAMA:
  - Nama: Test User 001
  - Tempat: Taman STID
  - Tanggal: [tanggal yang sama]
  - Metode: Qris/Transfer Bank
✓ Klik "🛒 WhatsApp" (CEPAT, dalam 5 detik)
✓ VERIFY: Alert muncul:
  ⏱️ Pesanan ini sudah dikirim baru-baru ini. 
  Silakan tunggu beberapa detik sebelum mengirim ulang.
✓ VERIFY: WhatsApp TIDAK membuka
✓ VERIFY: Data TIDAK masuk Google Forms 2x
✓ Tunggu 5 detik
✓ Klik "🛒 WhatsApp" lagi (data sama)
✓ VERIFY: Kali ini BOLEH (sudah lewat 5 detik)
```

### Test Case 4.3: Pesanan Berbeda Tanggal (Diizinkan)
```
✓ SETUP: Test 4.2 sudah complete, tunggu 5 detik
✓ Isi form dengan DATA BEDA:
  - Nama: Test User 001 (SAMA)
  - Tempat: Diantar Kerumahmu (BEDA)
  - Tanggal: [pilih tanggal lain] (BEDA)
  - Metode: Cash (BEDA)
✓ Klik "🛒 WhatsApp"
✓ VERIFY: WhatsApp membuka LANGSUNG (tidak diblock)
✓ VERIFY: Data masuk Google Forms
✓ Ini BOLEH karena kombinasi beda
```

### Test Case 4.4: Check Google Sheets
```
✓ Buka Google Forms Spreadsheet (hasil responses sheet)
✓ Scroll ke baris terbaru
✓ VERIFY: Data dari Test 4.1 & 4.3 muncul
✓ VERIFY: Data Test 4.2 TIDAK muncul (blocked)
✓ VERIFY: Kolom terisi: Nama, Tempat, Tanggal, Metode, Qty Ori, Qty Nori, Qty Spicy, Total, Sales Code
```

---

## 5️⃣ TEST RESPONSIVENESS

### Test Case 5.1: Desktop View
```
✓ Browser resolution: 1920x1080
✓ VERIFY: Pre-order box centered dan proportional
✓ VERIFY: Stok badge terlihat jelas
✓ VERIFY: Admin panel modal terbuka dengan ukuran tepat
```

### Test Case 5.2: Tablet View
```
✓ Browser resolution: 768x1024 (atau resize window)
✓ VERIFY: Pre-order box tetap readable
✓ VERIFY: Stok badge tetap ada
✓ VERIFY: Admin panel responsive
```

### Test Case 5.3: Mobile View
```
✓ Browser resolution: 480x800 (atau test di HP)
✓ VERIFY: Pre-order box stack dengan baik
✓ VERIFY: Admin panel bisa di-scroll
✓ VERIFY: Password input bisa diisi
```

---

## 6️⃣ TEST EDGE CASES

### Test Case 6.1: Clear Browser Cache
```
✓ Buka DevTools (F12) → Application → Clear site data
✓ Refresh halaman
✓ VERIFY: Stok reset ke default (belum ada value)
✓ VERIFY: Pre-order info menampilkan "-"
✓ VERIFY: Website tetap functioning
```

### Test Case 6.2: Multiple Tabs
```
✓ Buka website di 2 tab berbeda
✓ Di Tab A: Set stok Si' Ori = 5
✓ Refresh Tab B
✓ VERIFY: Stok Si' Ori Tab B juga = 5 (synchronized via localStorage)
```

### Test Case 6.3: Offline Mode
```
✓ Buka DevTools → Network → Offline
✓ Coba ubah stok di admin
✓ VERIFY: Tetap bisa save (localStorage offline-capable)
✓ Online lagi
✓ VERIFY: Data tetap ada
```

---

## 📋 TESTING REPORT TEMPLATE

Setelah testing, isi template ini:

```
TESTING REPORT - SI DOPA v2.0
=============================

Date: ___________
Tester: ___________
Browser: ___________
Device: ___________

✅ STOK SYSTEM
  - Tampilan stok: [ ] Pass [ ] Fail
  - Admin ubah stok: [ ] Pass [ ] Fail
  - Disable button: [ ] Pass [ ] Fail
  - Persistent stok: [ ] Pass [ ] Fail

✅ PRE-ORDER INFO
  - Tampilan box: [ ] Pass [ ] Fail
  - Admin set data: [ ] Pass [ ] Fail
  - Format validasi: [ ] Pass [ ] Fail
  - Info terupdate: [ ] Pass [ ] Fail

✅ ADMIN PANEL
  - Password security: [ ] Pass [ ] Fail
  - Tab switching: [ ] Pass [ ] Fail
  - Data save: [ ] Pass [ ] Fail

✅ GOOGLE FORMS
  - Normal order: [ ] Pass [ ] Fail
  - Double entry block: [ ] Pass [ ] Fail
  - Data in sheets: [ ] Pass [ ] Fail

✅ RESPONSIVENESS
  - Desktop: [ ] Pass [ ] Fail
  - Tablet: [ ] Pass [ ] Fail
  - Mobile: [ ] Pass [ ] Fail

✅ EDGE CASES
  - Clear cache: [ ] Pass [ ] Fail
  - Multiple tabs: [ ] Pass [ ] Fail
  - Offline mode: [ ] Pass [ ] Fail

ISSUES FOUND:
1. _____________________
2. _____________________
3. _____________________

NOTES:
_____________________

SIGN-OFF: ___________ (Date: ___________)
```

---

## 🚨 QUICK TROUBLESHOOTING DURING TESTING

| Issue | Solution |
|-------|----------|
| Stok tidak muncul | Clear cache (Ctrl+Shift+Del) → Refresh |
| Password tidak bekerja | Pastikan: `sidopa2026` (case-sensitive, no spaces) |
| Pre-order info blank | Admin belum set data, format harus YYYY-MM-DD |
| WhatsApp tidak buka | Check popup blocker, internet koneksi |
| Double entry tidak block | Clear localStorage, refresh page |
| Google Forms blank | Check entry IDs di script, internet connection |

---

**Testing Version:** 2.0  
**Last Updated:** May 2025  
**Status:** Ready for UAT (User Acceptance Testing)

