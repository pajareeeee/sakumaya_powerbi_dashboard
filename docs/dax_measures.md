# DAX measures (tabel `_Measures`)

**Total Nasabah** (Nasabah)
```dax
Total Nasabah = DISTINCTCOUNT(customers[customer_id])
```

**Nasabah Verified** (Nasabah)
```dax
Nasabah Verified = CALCULATE([Total Nasabah], customers[kyc_status] = "verified")
```

**KYC Verified %** (Nasabah)
```dax
KYC Verified % = DIVIDE([Nasabah Verified], [Total Nasabah])
```

**Nasabah Baru** (Nasabah)
```dax
Nasabah Baru = CALCULATE([Total Nasabah], USERELATIONSHIP(customers[registration_date], Kalender[Date]))
```

**Total Rekening** (Rekening)
```dax
Total Rekening = DISTINCTCOUNT(accounts[account_id])
```

**Total Saldo (Rp M)** (Rekening)
```dax
Total Saldo (Rp M) = DIVIDE(CALCULATE(SUM(accounts[balance_idr]), accounts[status] <> "closed"), 1000000000)
```

**Rekening Aktif KYC Bermasalah** (Rekening)
```dax
Rekening Aktif KYC Bermasalah = CALCULATE(DISTINCTCOUNT(accounts[account_id]), accounts[status] = "active", customers[kyc_status] IN {"rejected", "unverified"})
```

**Transaksi Sukses** (Transaksi)
```dax
Transaksi Sukses = CALCULATE(COUNTROWS(transactions), transactions[status] = "success")
```

**Transaksi Gagal** (Transaksi)
```dax
Transaksi Gagal = CALCULATE(COUNTROWS(transactions), transactions[status] = "failed")
```

**Failed Rate** (Transaksi)
```dax
Failed Rate = DIVIDE([Transaksi Gagal], COUNTROWS(transactions))
```

**Volume Sukses** (Transaksi)
```dax
Volume Sukses = CALCULATE(SUM(transactions[amount_idr]), transactions[status] = "success")
```

**Volume Sukses (Rp M)** (Transaksi)
```dax
Volume Sukses (Rp M) = DIVIDE([Volume Sukses], 1000000000)
```

**Rata-rata Transaksi (Rp Jt)** (Transaksi)
```dax
Rata-rata Transaksi (Rp Jt) = DIVIDE([Volume Sukses], [Transaksi Sukses]) / 1000000
```

**Belanja Sukses (Rp M)** (Transaksi)
```dax
Belanja Sukses (Rp M) = DIVIDE(CALCULATE([Volume Sukses], transactions[merchant_category] <> "transfer"), 1000000000)
```

**Total Pinjaman** (Kredit)
```dax
Total Pinjaman = COUNTROWS(loans)
```

**Pinjaman Bermasalah** (Kredit)
```dax
Pinjaman Bermasalah = CALCULATE(COUNTROWS(loans), loans[status] IN {"overdue", "written_off"})
```

**Loan Bermasalah %** (Kredit)
```dax
Loan Bermasalah % = DIVIDE([Pinjaman Bermasalah], [Total Pinjaman])
```

**Outstanding Pinjaman (Rp M)** (Kredit)
```dax
Outstanding Pinjaman (Rp M) = DIVIDE(SUM(loans[outstanding_idr]), 1000000000)
```

**Outstanding Bermasalah (Rp M)** (Kredit)
```dax
Outstanding Bermasalah (Rp M) = DIVIDE(CALCULATE(SUM(loans[outstanding_idr]), loans[status] IN {"overdue", "written_off"}), 1000000000)
```

**Pencairan (Rp M)** (Kredit)
```dax
Pencairan (Rp M) = DIVIDE(CALCULATE(SUM(loans[principal_idr]), USERELATIONSHIP(loans[disbursement_date], Kalender[Date])), 1000000000)
```

**Utilisasi Kartu Kredit** (Kredit)
```dax
Utilisasi Kartu Kredit = DIVIDE(CALCULATE(SUM(cards[outstanding_idr]), cards[card_type] = "credit"), CALCULATE(SUM(cards[credit_limit_idr]), cards[card_type] = "credit"))
```
