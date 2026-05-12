# 🚀 QUICK START - TESTING DALAM 5 MENIT

## ⚡ 3 Hal yang HARUS Dicoba (Langsung Testing)

### 1️⃣ TEST STOK (1 menit)

**Step:**
```
1. Buka website → Scroll ke produk
2. Klik "🔐 Admin Access" di bawah pre-order box
3. Input password: sidopa2026
4. Admin panel membuka → Klik tab "📦 Stok"
5. Ubah "Si' Ori" = 0
6. Klik "💾 Simpan"
7. Lihat halaman produk → badge Si' Ori jadi MERAH & tombol DISABLE
8. Set stok Si' Ori = 5 → tombol AKTIF lagi
```

**Expected:** ✅ Tombol enable/disable sesuai stok

---

### 2️⃣ TEST PRE-ORDER INFO (1 menit)

**Step:**
```
1. Admin panel masih terbuka
2. Klik tab "📅 Pre Order"
3. Isi:
   - Tanggal mulai: 2025-06-01
   - Tanggal akhir: 2025-06-05
   - Tanggal ready: 2025-06-08
   - Jam ambil: 14:00
4. Klik "💾 Simpan"
5. Scroll ke pre-order info box
6. Lihat info terupdate dengan format tanggal:
   "Sun, 01 Jun 2025 s/d Fri, 05 Jun 2025"
```

**Expected:** ✅ Tanggal muncul dengan format benar

---

### 3️⃣ TEST DOUBLE ENTRY PREVENTION (3 menit)

**Step:**
```
1. Tutup admin panel
2. Isi form order:
   - Nama: TestUser123
   - Tempat: Taman STID
   - Tanggal: [pilih tanggal besok]
   - Metode: Qris/Transfer Bank
3. Tambah produk: 1x Si' Ori
4. Klik "🛒 WhatsApp" → jendela WA terbuka
5. **CEPAT** klik "🛒 WhatsApp" lagi (dalam 5 detik, data sama)
6. Alert keluar: "⏱️ Pesanan ini sudah dikirim baru-baru ini..."
7. Tunggu 5 detik
8. Klik "🛒 WhatsApp" lagi → BOLEH (tidak di-block)
9. Buka Google Forms Spreadsheet
10. Lihat data masuk SATU kali (bukan dua)
```

**Expected:** ✅ Hanya satu data masuk di spreadsheet

---

## 🔍 VERIFY DENGAN DEVTOOLS (1 menit bonus)

### Check Stok:
```
1. F12 → Application → Local Storage → [domain]
2. Cari key: stock_ori, stock_nori, stock_spicy
3. Lihat nilainya ada disana ✅
```

### Check Pre-order:
```
1. F12 → Application → Local Storage → [domain]
2. Cari key: preorder_start, preorder_end, product_ready, pickup_time
3. Lihat nilainya ada disana ✅
```

### Check Double Entry:
```
1. F12 → Application → Local Storage → [domain]
2. Cari key: submitted_orders
3. Lihat timestamp terakhir pesanan ✅
```

---

## ✅ QUICK CHECKLIST

Setelah test 3 hal di atas, checklist ini:

- [ ] Stok display jalan (ada badge)
- [ ] Admin panel bisa masuk (password accepted)
- [ ] Stok bisa di-ubah (tombol enable/disable)
- [ ] Pre-order info tampil dengan format benar
- [ ] Double entry di-block dalam 5 detik
- [ ] Data masuk Google Forms hanya sekali
- [ ] Setelah 5 detik, pesanan bisa di-submit lagi

**Jika semua ✅ = SEMUA FITUR JALAN DENGAN BAIK!**

---

## ⚠️ JIKA ADA YANG TIDAK BERJALAN?

### Stok tidak muncul?
→ Clear cache (Ctrl+Shift+Delete) → Refresh (F5)

### Admin panel tidak buka?
→ Password HARUS tepat: `sidopa2026` (huruf kecil, tanpa spasi)

### Pre-order tanggal blank?
→ Format HARUS: YYYY-MM-DD (contoh: 2025-06-01)

### Double entry tidak di-block?
→ Tunggu minimal 5 detik antar order

### Data tidak masuk Google Forms?
→ Buka DevTools (F12) → Console → lihat error

---

## 🎯 TESTING SELESAI!

Jika semua 3 tes berhasil = **Website siap publish!** 🎉

Untuk test lebih detail, lihat: **TESTING_GUIDE.md**

Untuk pertanyaan, lihat: **FAQ_TROUBLESHOOTING.md**

---

**Estimated Time:** 5 minutes ⏱️  
**Difficulty:** 🟢 Easy  
**Status:** Ready for Testing ✅

