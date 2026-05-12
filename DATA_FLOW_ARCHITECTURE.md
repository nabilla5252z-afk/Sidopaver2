# 🔄 DATA FLOW & SYSTEM ARCHITECTURE

## 1️⃣ SISTEM ENTRY GOOGLE FORMS (DOUBLE ENTRY PREVENTION)

### Flow Diagram:

```
┌─────────────────────────────────────────────────────────────┐
│ CUSTOMER BROWSER                                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Form Input                                                │
│  ├─ Nama Pembeli: "Ahmad"                                 │
│  ├─ Tanggal: "2025-06-01"                                 │
│  ├─ Metode: "Qris/TF"                                     │
│  └─ Keranjang: 2x Si' Ori                                 │
│       ↓                                                    │
│  Generate Unique Order ID:                                │
│  "Ahmad_2025-06-01_Qris/TF"                               │
│       ↓                                                    │
│  Check localStorage["submitted_orders"]:                  │
│  ├─ Ada entry untuk ID ini?                               │
│  ├─ Waktu entry < 5 detik yang lalu?                     │
│  │  YES → ❌ BLOCK dengan alert                           │
│  │  NO  → ✅ CONTINUE                                     │
│       ↓                                                    │
│  Open WhatsApp Link                                        │
│  (User klik = confirms order)                             │
│       ↓                                                    │
│  submitToGoogleForm()                                      │
│  ├─ POST data ke Google Forms endpoint                     │
│  ├─ FormData:                                              │
│  │  - entry.39058957: "Ahmad"                             │
│  │  - entry.894911535: "2025-06-01"                       │
│  │  - entry.168921694: 2 (qty ori)                        │
│  │  - ... dst                                              │
│  │       ↓                                                 │
│  └─ Response: NO-CORS (silent)                            │
│       ↓                                                    │
│  localStorage["submitted_orders"] = {                     │
│    "Ahmad_2025-06-01_Qris/TF": 1654321098 // timestamp    │
│  }                                                         │
│       ↓                                                    │
│  Clear Cart & Form                                         │
│  Alert: "✅ Pesanan berhasil dikirim!"                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ GOOGLE SHEETS (Result)                                      │
├─────────────────────────────────────────────────────────────┤
│ Row: [Ahmad | Taman STID | 2025-06-01 | Qris/TF | 2 | 0 | 0]
└─────────────────────────────────────────────────────────────┘
```

### Local Storage State After Order:

```javascript
localStorage = {
  // Stok (unchanged if no admin change)
  stock_ori: "25",
  stock_nori: "30",
  stock_spicy: "20",
  
  // Pre-order info (unchanged if no admin change)
  preorder_start: "2025-06-01",
  preorder_end: "2025-06-05",
  product_ready: "2025-06-08",
  pickup_time: "14:00",
  
  // Order tracking (UPDATED)
  submitted_orders: {
    "Ahmad_2025-06-01_Qris/TF": 1654321098,
    "Budi_2025-06-02_Cash": 1654321456
  }
}
```

---

## 2️⃣ SISTEM STOK PRODUK

### Admin Flow:

```
┌─────────────────────────────────────────────────────────────┐
│ ADMIN ACCESS                                                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Click "🔐 Admin Access"                                  │
│       ↓                                                    │
│  Prompt Password                                           │
│  Input: "sidopa2026"                                       │
│       ↓                                                    │
│  Check: password === "sidopa2026"?                         │
│  ├─ NO  → Alert "❌ Password salah!" → CLOSE              │
│  └─ YES → openAdminPanel()                                │
│       ↓                                                    │
│  Show Admin Panel Modal                                    │
│  loadAdminData(): Baca dari localStorage                   │
│  ├─ stock_ori = 25                                         │
│  ├─ stock_nori = 30                                        │
│  └─ stock_spicy = 20                                       │
│  │   (Load ke form inputs)                                │
│       ↓                                                    │
│  Admin Ubah Nilai                                          │
│  ├─ Si' Ori: 25 → 0                                        │
│  └─ Click "💾 Simpan"                                     │
│       ↓                                                    │
│  saveAdminChanges()                                        │
│  ├─ localStorage.stock_ori = "0"                           │
│  ├─ Call updateStockDisplay()                              │
│  └─ Alert "✅ Data berhasil disimpan!"                    │
│       ↓                                                    │
│  updateStockDisplay()                                      │
│  ├─ Read: localStorage.stock_ori = "0"                    │
│  ├─ Update Badge: "Stok: 0"                               │
│  ├─ Update Badge Color: RED                               │
│  ├─ Call updateAddButtonState("Si' Ori", 0)               │
│  │  ├─ Button.disabled = true                             │
│  │  ├─ Button.style.opacity = 0.5                         │
│  │  ├─ Button.innerHTML = "❌ Stok Habis"                 │
│  │  └─ Button.style.cursor = "not-allowed"                │
│  └─ Page Updated!                                          │
│       ↓                                                    │
│  closeAdminPanel()                                         │
│  Close modal, continue shopping                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Customer View Flow:

```
┌─────────────────────────────────────────────────────────────┐
│ CUSTOMER PAGE LOAD                                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  DOMContentLoaded event                                     │
│       ↓                                                    │
│  Call updateStockDisplay()                                │
│       ↓                                                    │
│  For each product:                                         │
│  ├─ Si' Ori: Read localStorage.stock_ori                  │
│  │  ├─ Value = "0"                                         │
│  │  ├─ Display Badge: "🔴 Stok: 0"                        │
│  │  ├─ updateAddButtonState("Si' Ori", 0)                │
│  │  ├─ Button Disabled ✅                                 │
│  │  └─ Text: "❌ Stok Habis"                              │
│  │                                                        │
│  ├─ Si' Nori: Read localStorage.stock_nori               │
│  │  ├─ Value = "30"                                        │
│  │  ├─ Display Badge: "🟢 Stok: 30"                       │
│  │  ├─ updateAddButtonState("Si' Nori", 30)               │
│  │  ├─ Button Enabled ✅                                  │
│  │  └─ Text: "Tambah ke Keranjang"                        │
│  │                                                        │
│  └─ Si' Spicy: Read localStorage.stock_spicy              │
│     ├─ Value = "20"                                        │
│     ├─ Display Badge: "🟢 Stok: 20"                       │
│     ├─ updateAddButtonState("Si' Spicy", 20)              │
│     ├─ Button Enabled ✅                                  │
│     └─ Text: "Tambah ke Keranjang"                        │
│                                                             │
│  PAGE READY                                                │
│  Customer lihat:                                           │
│  - Si' Ori: 🔴 Stok: 0 + ❌ Button (DISABLED)              │
│  - Si' Nori: 🟢 Stok: 30 + Tombol aktif                   │
│  - Si' Spicy: 🟢 Stok: 20 + Tombol aktif                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Add to Cart with Stock Validation:

```
Customer Click "Tambah ke Keranjang" (Si' Ori)
      ↓
validateStockBeforeAddToCart("Si' Ori")
├─ Read: localStorage.stock_ori
├─ Value = 0
├─ Return: false
└─ NOT ADDED TO CART
      ↓
Alert: "❌ Produk ini sedang tidak tersedia!"
```

---

## 3️⃣ SISTEM PRE-ORDER INFO

### Admin Setup Flow:

```
┌─────────────────────────────────────────────────────────────┐
│ ADMIN ACCESS                                                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Click "🔐 Admin Access"                                  │
│  Password: "sidopa2026" ✅                                │
│       ↓                                                    │
│  Admin Panel Opens                                        │
│  Click Tab "📅 Pre Order"                                 │
│       ↓                                                    │
│  Form Fields:                                              │
│  ├─ Tanggal Pre Order Dimulai: [datepicker]              │
│  │  User select: 2025-06-01                               │
│  │                                                        │
│  ├─ Tanggal Pre Order Berakhir: [datepicker]             │
│  │  User select: 2025-06-05                               │
│  │                                                        │
│  ├─ Tanggal Produk Ready: [datepicker]                   │
│  │  User select: 2025-06-08                               │
│  │                                                        │
│  └─ Jam Ambil: [timepicker]                              │
│     User select: 14:00                                    │
│       ↓                                                    │
│  Click "💾 Simpan"                                        │
│       ↓                                                    │
│  saveAdminChanges()                                       │
│  ├─ localStorage["preorder_start"] = "2025-06-01"         │
│  ├─ localStorage["preorder_end"] = "2025-06-05"           │
│  ├─ localStorage["product_ready"] = "2025-06-08"          │
│  ├─ localStorage["pickup_time"] = "14:00"                 │
│  ├─ Call updatePreorderDisplay()                          │
│  └─ Alert "✅ Data berhasil disimpan!"                   │
│       ↓                                                    │
│  MODAL CLOSES, PAGE UPDATES                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Pre-Order Info Display:

```
┌─────────────────────────────────────────────────────────────┐
│ updatePreorderDisplay()                                      │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Read from localStorage:                                   │
│  ├─ preorder_start = "2025-06-01"                          │
│  ├─ preorder_end = "2025-06-05"                            │
│  ├─ product_ready = "2025-06-08"                           │
│  └─ pickup_time = "14:00"                                  │
│       ↓                                                    │
│  Format Dates to Indonesian Locale:                        │
│  ├─ "2025-06-01" → "Sun, 01 Jun 2025"                     │
│  ├─ "2025-06-05" → "Fri, 05 Jun 2025"                     │
│  └─ "2025-06-08" → "Sun, 08 Jun 2025"                     │
│       ↓                                                    │
│  Build Display String:                                     │
│  ├─ Periode: "Sun, 01 Jun 2025 s/d Fri, 05 Jun 2025"     │
│  ├─ Ready: "Sun, 08 Jun 2025"                             │
│  └─ Jam: "14:00"                                           │
│       ↓                                                    │
│  Update DOM Elements:                                      │
│  ├─ displayPreorderPeriod.textContent = periode            │
│  ├─ displayProductReady.textContent = ready                │
│  └─ displayPickupTime.textContent = jam                    │
│       ↓                                                    │
│ ┌─────────────────────────────────────────┐               │
│ │ 📅 Informasi Pre Order Si Dopa          │               │
│ ├─────────────────────────────────────────┤               │
│ │ Periode Pre Order │ Tanggal Ready       │               │
│ │ Sun, 01 Jun -     │ Sun, 08 Jun 2025    │               │
│ │ Fri, 05 Jun 2025  │                     │               │
│ │        Jam Ambil        │               │               │
│ │        14:00            │               │               │
│ └─────────────────────────────────────────┘               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔄 STATE MANAGEMENT (localStorage)

### Initialization State:
```javascript
{
  // Stok (default: tidak ada)
  stock_ori: undefined,
  stock_nori: undefined,
  stock_spicy: undefined,
  
  // Pre-order (default: tidak ada)
  preorder_start: undefined,
  preorder_end: undefined,
  product_ready: undefined,
  pickup_time: undefined,
  
  // Orders (default: object kosong)
  submitted_orders: {}
}
```

### After Admin Set Data:
```javascript
{
  stock_ori: "5",
  stock_nori: "30",
  stock_spicy: "20",
  
  preorder_start: "2025-06-01",
  preorder_end: "2025-06-05",
  product_ready: "2025-06-08",
  pickup_time: "14:00",
  
  submitted_orders: {}
}
```

### After Customer Order:
```javascript
{
  // ... stok & preorder unchanged
  
  submitted_orders: {
    "Ahmad_2025-06-01_Qris": 1654321098,
    "Budi_2025-06-02_Cash": 1654321456,
    "Citra_2025-06-01_Qris": 1654321789
  }
}
```

---

## 📊 DATABASE SCHEMA (localStorage)

### Stok Table:
```
Key: stock_ori | stock_nori | stock_spicy
Type: String (numeric)
Value: "0" to "999"
Scope: Per browser, per device
Update: Admin panel
Read: Page load, button state
```

### Pre-Order Info Table:
```
Key: preorder_start | preorder_end | product_ready | pickup_time
Type: String
Value: 
  - Dates: "YYYY-MM-DD"
  - Time: "HH:MM"
Scope: Per browser, per device
Update: Admin panel
Read: Page load, info display
```

### Order Tracking Table:
```
Key: submitted_orders
Type: JSON Object
Value: {
  "OrderID": timestamp,
  "OrderID": timestamp
}
OrderID = "Name_Date_Method"
Timestamp = milliseconds since epoch
Scope: Per browser, per device
Update: On successful order submit
Read: Before submit (double-entry check)
TTL: Permanent (or until cache clear)
```

---

## 🔐 SECURITY FLOW

### Admin Password Check:
```
User Input: "sidopa2026"
    ↓
Password Check:
if (input === "sidopa2026") ✓
    ↓
Grant Access to:
├─ Stok Management
└─ Pre-Order Management
```

### Key Notes:
- Password stored in JavaScript (visible via inspect element)
- Acceptable untuk website seperti ini (not high-security)
- If compromised: can change password in code
- No backend authentication needed for this use case

---

## ⚠️ ERROR HANDLING

### Stock Validation Failure:
```
validateStockBeforeAddToCart("Si' Ori")
    ↓
Return: false
    ↓
Alert: "❌ Produk ini sedang tidak tersedia!"
    ↓
Item NOT added to cart
```

### Double Entry Prevention:
```
isOrderAlreadySubmitted(orderId)
    ↓
Found in localStorage & within 5 seconds
    ↓
Alert: "⏱️ Pesanan ini sudah dikirim baru-baru ini..."
    ↓
WhatsApp NOT opened
Google Forms NOT submitted
```

### Password Failure:
```
promptAdminPassword()
    ↓
User Input: wrong password
    ↓
Check: === "sidopa2026"? NO
    ↓
Alert: "❌ Password salah!"
    ↓
Admin Panel NOT opened
```

---

## 📈 PERFORMANCE METRICS

### localStorage Performance:
- Write: ~0.1ms per key
- Read: ~0.05ms per key
- Capacity: ~5-10MB typical
- Current usage: ~200 bytes

### DOM Update Performance:
- updateStockDisplay(): ~1-2ms
- updatePreorderDisplay(): ~0.5-1ms
- updateCart(): ~2-5ms (depends on item count)

### Form Submission Performance:
- Google Forms fetch: ~200-500ms (depends on internet)
- localStorage write: ~1ms
- Page update: ~10-20ms

**Total order flow: ~500-1000ms** ✅ Acceptable

---

## 🧪 STATE TRANSITIONS

### Stok State Machine:
```
┌─────────────────┐
│  Uninitialized  │ (stock_ori = undefined)
│   Stok: "-"     │
│  Button: ACTIVE │
└────────┬────────┘
         │ Admin set stok
         ↓
┌──────────────────┐
│  After Saved     │ (stock_ori = "5")
│  Stok: "5"       │
│  Badge: GREEN    │
│  Button: ACTIVE  │
└────────┬────────┘
         │ Customer order 5
         │ (manual stok update needed)
         ↓
┌──────────────────┐
│  Stock Running   │ (stock_ori = "0")
│  Stok: "0"       │
│  Badge: RED      │
│  Button: DISABLE │
└────────┬────────┘
         │ Admin restock
         ↓
         ├→ Back to "After Saved" (stock_ori = "50")
```

### Order State Machine:
```
┌─────────────────────┐
│  Not Yet Submitted  │ (not in submitted_orders)
│  Customer fill form │
└────────┬────────────┘
         │ Click WhatsApp
         ↓
┌─────────────────────┐
│  Pending Submit     │ (generateOrderId, check exists)
│  Check double entry │
└────────┬────────────┘
         │
    NO ──┼── YES
        │      │
        ↓      ↓
    Open WA   BLOCK
        │       +
        ↓       Alert
      Submit
        │
        ↓
┌──────────────────────┐
│  Successfully Sent   │ (in submitted_orders)
│  data in GForms      │
│  cart cleared        │
└──────────────────────┘
```

---

## 🎯 CRITICAL PATH

**Most Important Paths:**

### Path 1: Stock Update
```
Admin → Password → Set Stock → Save → Button Update → Customer See
Time: ~100ms
Critical: YES (UI depends on this)
```

### Path 2: Order Submission
```
Form Fill → Click WA → Check Order ID → Submit GForms → Clear Cart
Time: ~500-1000ms
Critical: YES (main conversion)
```

### Path 3: Double Entry Block
```
Order ID Check → localStorage lookup → Timestamp compare → Block/Allow
Time: <5ms
Critical: YES (data integrity)
```

---

## 📍 INTEGRATION POINTS

### External: Google Forms
```
Website → POST /formResponse
├─ Method: POST
├─ Mode: no-cors
├─ Data: FormData with entry.* keys
└─ Response: Ignored (silent submit)
```

### Internal: localStorage
```
Website ↔ Browser localStorage
├─ Write: setItem(key, value)
├─ Read: getItem(key)
└─ Update: setItem overwrite
```

### Internal: WhatsApp
```
Website → window.open(wa.me/...)
├─ User confirms: Opens WhatsApp
└─ Returns: window object (success) or null (blocked)
```

---

**Architecture Version:** 2.0  
**Last Updated:** May 2025  
**Status:** ✅ Production Architecture

