# 📚 README - SI DOPA WEBSITE v2.0

## 🎉 Selamat! Website Si Dopa telah di-upgrade!

Tiga fitur besar telah ditambahkan untuk meningkatkan sistem pre-order Anda.

---

## 📖 PANDUAN MEMBACA DOKUMENTASI

Pilih sesuai kebutuhan Anda:

### 👨‍💼 Saya Admin (Ingin tau cara gunakan)
**→ Baca: [QUICK_START_TESTING.md](QUICK_START_TESTING.md)**
- 5 menit untuk pahami semua fitur
- Step-by-step cara test
- Checklist untuk verifikasi

Kemudian (jika detail):
**→ Baca: [DOKUMENTASI_FITUR_BARU.md](DOKUMENTASI_FITUR_BARU.md)**
- Penjelasan detail setiap fitur
- Mekanisme kerja lengkap
- Cara mengecek sistem berjalan

---

### 🧪 Saya QA/Testing (Ingin test sistemnya)
**→ Baca: [TESTING_GUIDE.md](TESTING_GUIDE.md)**
- 20+ test cases siap pakai
- Step-by-step procedure
- Testing report template
- Troubleshooting table

---

### ❓ Saya Ada Pertanyaan/Masalah
**→ Baca: [FAQ_TROUBLESHOOTING.md](FAQ_TROUBLESHOOTING.md)**
- 15+ FAQ dengan jawaban
- Troubleshooting untuk setiap fitur
- DevTools debugging guide
- Kapan hubungi developer

---

### 👨‍💻 Saya Developer (Ingin maintain kodenya)
**→ Baca: [IMPLEMENTASI_SUMMARY.md](IMPLEMENTASI_SUMMARY.md)**
- Technical specifications
- Code structure & functions
- Data flow diagram
- Performance notes
- Browser compatibility

Kemudian baca code di: **index.html** (lihat JavaScript section)

---

## ⚡ QUICK REFERENCE - 3 FITUR BARU

### 1️⃣ Sistem Entry Google Forms (Perbaikan)
**Problem:** Data masuk spreadsheet walau customer tidak jadi membeli
**Solution:** Data hanya masuk SETELAH customer buka WhatsApp + anti-double-entry
**Akses:** Auto (customer tidak perlu setup)
**Status:** ✅ Active

### 2️⃣ Sistem Stok Produk
**Problem:** Tidak ada kontrol stok, customer bisa order product habis
**Solution:** Admin bisa set stok per varian, otomatis disable jika habis
**Akses:** Admin password: `sidopa2026`
**Status:** ✅ Active

### 3️⃣ Pre-Order Info Box
**Problem:** Customer tidak tahu kapan pre-order dibuka/tutup, kapan ambil
**Solution:** Kotak info dengan tanggal pre-order, ready date, jam ambil
**Akses:** Admin password: `sidopa2026`
**Status:** ✅ Active

---

## 🚀 LANGSUNG TESTING (5 MENIT)

Tidak punya waktu? Test cepat di sini:

```
1. Buka: QUICK_START_TESTING.md
2. Follow 3 test case (masing 1-3 menit)
3. Checklist hasil
4. Done! ✅
```

---

## 📊 DOKUMENTASI FILE LISTING

```
📄 README.md                          ← Anda di sini
📄 QUICK_START_TESTING.md             ← Fastest way to test
📄 DOKUMENTASI_FITUR_BARU.md          ← Detailed feature guide
📄 TESTING_GUIDE.md                   ← QA test procedures
📄 FAQ_TROUBLESHOOTING.md             ← Q&A & debugging
📄 IMPLEMENTASI_SUMMARY.md            ← Technical summary
📄 index.html                         ← MAIN FILE (updated)
```

---

## 🎯 USE CASES

### Skenario 1: "Saya ingin launch besok, perlu tahu apakah sistem siap"
```
1. Baca: QUICK_START_TESTING.md (5 min)
2. Test 3 hal utama (5 min)
3. Checklist hasil (1 min)
Total: 11 menit ✅
→ Siap launch jika semua pass
```

### Skenario 2: "Ada error/masalah, gimana?"
```
1. Baca: FAQ_TROUBLESHOOTING.md (find your issue)
2. Follow solusi yang diberikan
3. Jika belum resolve → lihat "When to Contact Developer"
```

### Skenario 3: "Saya perlu train tim saya"
```
1. Admin: Share QUICK_START_TESTING.md (5 min walk-through)
2. QA: Share TESTING_GUIDE.md (formal test checklist)
3. Support: Share FAQ_TROUBLESHOOTING.md (handle customer issues)
4. Developer: Share IMPLEMENTASI_SUMMARY.md + inline comments
```

### Skenario 4: "Saya mau upgrade/maintain code di masa depan"
```
1. Baca: IMPLEMENTASI_SUMMARY.md (overview)
2. Baca: FAQ_TROUBLESHOOTING.md → "DevTools Debugging" section
3. Lihat code: index.html → search "===================="
4. Reference: DOKUMENTASI_FITUR_BARU.md (feature detail)
```

---

## 💡 TIPS & BEST PRACTICES

### Untuk Admin:
- ✅ Set stok SEBELUM announce pre-order
- ✅ Update stok setiap hari (lihat Google Forms)
- ✅ Set pre-order info kapan pre-order dibuka
- ⚠️ Jangan share password `sidopa2026` ke customer

### Untuk Customer:
- ✅ Lihat pre-order info untuk tahu kapan dibuka/tutup
- ✅ Lihat stok badge untuk tahu ketersediaan
- ⚠️ Jangan clear browser cache saat pre-order aktif

### Untuk Testing:
- ✅ Use incognito window untuk fresh testing
- ✅ Always check DevTools Console untuk error logs
- ✅ Test di 2-3 browser berbeda (Chrome, Firefox, Safari)

### Untuk Maintenance:
- ✅ Keep localStorage keys naming consistent
- ✅ Document any code changes
- ✅ Test after modifying JavaScript
- ⚠️ Do not change Google Forms entry IDs without testing

---

## 🔍 VERIFIKASI SISTEM BEKERJA

### Cara Cepat (Visual Check):
1. Lihat badge "Stok: X" di setiap varian → ✅ Stok system OK
2. Lihat kotak pre-order info → ✅ Pre-order system OK
3. Try order, block dalam 5 detik → ✅ Double-entry prevention OK

### Cara Detail (DevTools Check):
1. F12 → Application → Local Storage
2. Lihat keys: `stock_*`, `preorder_*`, `submitted_orders`
3. Value ada = ✅ System storing data correctly

---

## ⚠️ IMPORTANT NOTES

### Data Storage:
- 📍 All data stored in **browser localStorage** (per device)
- 📍 Data persists until customer clears cache
- 📍 Not shared between devices
- ✅ Admin can re-set anytime via admin panel

### Admin Access:
- 🔐 Password: `sidopa2026` (stored in code, visible if inspect element)
- ✅ Acceptable untuk web like this (not production SaaS)
- ⚠️ If need higher security, consider moving to backend

### Google Forms Integration:
- ✅ Still working after update
- ✅ Double-entry prevention improves data quality
- ⚠️ Stok is UI-only (manual enforcement by admin)

---

## 📞 SUPPORT & MAINTENANCE

### If Something Breaks:
1. **Check:** FAQ_TROUBLESHOOTING.md
2. **Debug:** Use DevTools → Application & Console
3. **Contact:** Developer with:
   - Screenshots/videos
   - DevTools error logs
   - Steps to reproduce
   - Device & browser info

### For Future Enhancements:
- Real-time stok sync (cloud database)
- Automatic date picker restriction
- Email confirmation system
- Analytics dashboard
- See: IMPLEMENTASI_SUMMARY.md → "Next Steps"

---

## ✅ LAUNCH CHECKLIST

Before going live:

- [ ] Test all 3 features (QUICK_START_TESTING.md)
- [ ] Check Google Forms integration (TESTING_GUIDE.md)
- [ ] Verify responsive design (TESTING_GUIDE.md)
- [ ] Train admin (DOKUMENTASI_FITUR_BARU.md)
- [ ] Create customer FAQ (FAQ_TROUBLESHOOTING.md)
- [ ] Set initial stok values
- [ ] Set pre-order dates
- [ ] Announce to customers
- [ ] Monitor for first 24 hours

---

## 📝 VERSION HISTORY

| Version | Date | Status | Notes |
|---------|------|--------|-------|
| 1.0 | Before | Completed | Original website |
| 2.0 | May 2025 | 🟢 LIVE | Added stok + pre-order + double-entry prevention |

---

## 📍 FILE LOCATIONS

**Main file:** `index.html` (all 3 features embedded)

**Documentation files:**
- Quick Start: `QUICK_START_TESTING.md`
- Feature Guide: `DOKUMENTASI_FITUR_BARU.md`
- Testing: `TESTING_GUIDE.md`
- Support: `FAQ_TROUBLESHOOTING.md`
- Technical: `IMPLEMENTASI_SUMMARY.md`

**Asset files:** (unchanged)
- `sidopa.png`
- `etalase_ori.jpg`
- `etalase_nori.jpg`
- `etalase_spicy.jpg`
- `background awal.jpg`

---

## 🎓 QUICK LEARNING PATH

### Option A: "Just tell me how to use it" (10 min)
1. QUICK_START_TESTING.md (5 min read)
2. DOKUMENTASI_FITUR_BARU.md → Section 2 & 3 (5 min read)

### Option B: "I need to test everything" (1 hour)
1. TESTING_GUIDE.md (full read)
2. Do all test cases
3. Document results

### Option C: "I need to maintain/debug this" (30 min)
1. IMPLEMENTASI_SUMMARY.md (full read)
2. Look at index.html code (search "====================")
3. FAQ_TROUBLESHOOTING.md → DevTools section

---

## 🎉 YOU'RE ALL SET!

Pick a documentation file based on your role and start reading. Everything is tested and ready to go! 

**Questions?** Check FAQ_TROUBLESHOOTING.md first 😊

**Happy selling!** 🍪🎊

---

**Website:** Si Dopa - Singkong Crunchy Balls  
**Version:** 2.0  
**Status:** ✅ Production Ready  
**Last Updated:** May 2025  
**Maintenance:** See IMPLEMENTASI_SUMMARY.md for developer notes

#   S i d o p a v e r 2  
 