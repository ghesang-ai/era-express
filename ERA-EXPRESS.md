# ERA-EXPRESS

> PRD ini di-generate lewat PRD Generator di **ERA-VIBECODING**, disubmit ke Showcase, di-review dan di-approve/featured oleh Admin. Disalin ke sini sebagai spec awal untuk pengembangan langsung dengan Claude Code.

**Ide awal:** Saya Mau Buat ERA-EXPRESS, jadi setiap saya mau kirim barang
**Deskripsi submission:** Masalah Pengiriman akan mudah dan cepat dengan adanya ERA-EXPRESS ini.
**Status di ERA-VIBECODING:** Featured
**Tanggal dibuat:** 14 September 2026

---

## 1. Ringkasan
**Nama Produk:** ERA‑EXPRESS – Form Otomatis Pengiriman 21 Express
**Tujuan:** Memungkinkan Admin Region 5 membuat dan mengirimkan formulir pengiriman barang (Flyer, Spanduk, Signboard, Umbul‑Umbul, dll) ke layanan kurir 21 Express hanya dalam 2‑3 klik.
**Nilai Bisnis:** Mengurangi waktu pembuatan form manual ≈ 5 menit per pengiriman → percepatan proses logistik, mengurangi error input data, serta meningkatkan kepatuhan standar pengiriman.

---

## 2. Masalah & Dampak
| Masalah | Dampak | Solusi ERA‑EXPRESS |
|---------|--------|-------------------|
| Admin harus menyalin plant code toko, mengisi data pengirim & penerima secara manual di form 21 Express. | • Waktu lama, potensi typo.<br>• Kesulitan melacak toko yang sudah diproses. | Dropdown "Daftar Toko" → auto‑populate plant code & alamat penerima. |
| Form 21 Express berbeda‑beda tergantung jenis barang (Flyer, Spanduk, dll). | • Admin harus mencari template yang tepat. | Pilihan "Jenis Barang" menampilkan field yang relevan secara dinamis. |
| Tidak ada riwayat satu‑klik kirim sehingga harus mencatat secara terpisah. | • Duplikasi pekerjaan, laporan tidak akurat. | Simpan log otomatis setiap kali form berhasil dikirim. |

---

## 3. Pengguna & Skenario
**Pengguna utama:** Admin logistik Region 5 (mis. *Admin_Region5_01*).
**Skenario tipikal:**
1. Admin membuka **ERA‑EXPRESS** di browser atau aplikasi internal.
2. Memilih toko tujuan dari daftar (bisa satu atau multiple).
3. Memilih **Jenis Barang** yang akan dikirim (Flyer, Spanduk, Signboard, Umbul‑Umbul).
4. Sistem menampilkan **Template Form 21 Express** yang sudah terisi otomatis (plant code, alamat, kontak).
5. Admin melengkapi field khusus (jumlah, ukuran, catatan khusus).
6. Klik **Kirim ke 21 Express** → sistem mengirim data ke API 21 Express (atau menghasilkan file PDF/email).
7. Tampilan **Ringkasan Pengiriman** muncul, termasuk nomor tracking sementara.
8. Admin dapat **Export** riwayat pengiriman dalam format CSV/Excel.

---

## 4. Ruang Lingkup

| In Scope | Out of Scope |
|----------|--------------|
| • Daftar toko Region 5 (placeholder `{store_list}`) dengan plant code & alamat.<br>• Dropdown jenis barang & dinamisasi field.<br>• Integrasi (simulasi) ke API 21 Express atau generate PDF/email.<br>• Penyimpanan log pengiriman (database sederhana).<br>• UI berbasis web (responsive). | • Integrasi pembayaran atau invoice.<br>• Manajemen inventori barang di gudang.<br>• Otomatisasi pelacakan real‑time tracking 21 Express (hanya simpan nomor tracking).<br>• Multi‑region (hanya Region 5). |

---

## 5. Alur Utama (Langkah per Langkah)

1. **Login** – Admin memasukkan username/password (placeholder).
2. **Dashboard** – Menampilkan tombol **"Buat Pengiriman 21 Express"** dan tabel riwayat 7 hari terakhir.
3. **Pilih Toko** –
   - Klik **"Pilih Toko"** → muncul modal dengan pencarian & checkbox multi‑select.
   - Setelah dipilih, sistem menampilkan tabel ringkas plant code & alamat.
4. **Pilih Jenis Barang** – Dropdown dengan opsi: Flyer, Spanduk, Signboard, Umbul‑Umbul.
5. **Generate Form** – Sistem menampilkan form 21 Express yang sudah terisi:
   - **Pengirim** (nama kantor pusat, alamat, kontak – placeholder).
   - **Penerima** (plant code, alamat toko, kontak).
   - **Detail Barang** (jenis, ukuran, jumlah, berat perkiraan).
6. **Edit Tambahan** – Admin dapat menambah catatan khusus atau mengubah jumlah.
7. **Validasi** – Tombol **"Cek Kesesuaian"** memeriksa wajib field & format nomor telepon.
8. **Kirim** – Klik **"Kirim ke 21 Express"** →
   - (a) Jika API tersedia: POST data ke endpoint `/api/21express/create`.
   - (b) Jika tidak: Generate PDF & kirim email ke `express@21express.com` (placeholder).
9. **Konfirmasi** – Muncul pop‑up "Pengiriman berhasil" dengan **Nomor Referensi** (placeholder).
10. **Simpan Riwayat** – Data tersimpan di tabel **`shipping_logs`** dengan status *sent*.
11. **Export / Cetak** – Opsional tombol **"Export CSV"** atau **"Print PDF"** di halaman riwayat.

---

## 6. Data & Field yang Dibutuhkan

| Kategori | Field | Tipe | Keterangan |
|----------|-------|------|------------|
| **Pengirim** | `sender_name` | teks | Nama kantor pusat (placeholder). |
| | `sender_address` | teks | Alamat kantor pusat. |
| | `sender_phone` | teks | No. telepon kantor. |
| **Penerima (Toko)** | `store_id` | kode | Di‑select dari `{store_list}`. |
| | `plant_code` | teks | Auto‑populate. |
| | `store_name` | teks | Auto‑populate. |
| | `store_address` | teks | Auto‑populate. |
| | `store_phone` | teks | Auto‑populate. |
| **Barang** | `item_type` | enum | Flyer / Spanduk / Signboard / Umbul‑Umbul. |
| | `item_quantity` | angka | Wajib. |
| | `item_size` | teks | Opsional (contoh: "A4", "3x2 m"). |
| | `item_weight` | angka | Estimasi kg (auto‑calc bila ada ukuran). |
| | `special_note` | teks | Opsional. |
| **Pengiriman** | `courier` | enum | Fixed: "21 Express". |
| | `reference_no` | teks | Generated otomatis (e.g., `EXP-YYYYMMDD-####`). |
| | `created_at` | datetime | Timestamp. |
| | `status` | enum | Draft / Sent / Error. |

*Semua data disimpan dalam tabel sementara `temp_form` (draft) hingga tombol **Kirim** ditekan.*

---

## 7. Kriteria Selesai (Checklist)

- [ ] UI responsif dengan tema ERA (warna biru/hijau).
- [ ] Daftar toko Region 5 dapat di‑search & multi‑select.
- [ ] Form 21 Express otomatis terisi plant code, alamat, kontak.
- [ ] Field dinamis berubah sesuai pilihan **Jenis Barang**.
- [ ] Validasi wajib (tidak boleh kosong, format telepon, angka positif).
- [ ] Integrasi simulasi API 21 Express (POST) atau generate PDF + email.
- [ ] Penyimpanan log pengiriman lengkap (referensi, tanggal, toko, barang).
- [ ] Fitur **Export CSV** & **Print PDF** pada halaman riwayat.
- [ ] Dokumentasi singkat untuk Admin (panduan 2‑halaman).
- [ ] Uji coba dengan 5 toko contoh + 2 jenis barang, semua skenario "happy path" berhasil.

---

## 8. Risiko & Catatan Keamanan Data

| Risiko | Dampak | Mitigasi |
|--------|--------|----------|
| **Data toko (alamat, telepon) bocor** | Pelanggaran privasi internal. | Simpan hanya *placeholder* di lingkungan dev; gunakan enkripsi at‑rest pada tabel `shipping_logs`. |
| **Kesalahan pengiriman karena field tidak tervalidasi** | Pengiriman barang ke alamat salah. | Implementasi validasi front‑end + back‑end; notifikasi error jelas. |
| **Integrasi API 21 Express gagal** | Pengiriman tidak tercatat. | Fallback: generate PDF & email manual; simpan status *Error* di log. |
| **Pengguna tidak familiar UI** | Penurunan adopsi. | Sesi onboarding singkat (video 2 menit) & tooltip pada tiap field. |
| **Penggunaan multi‑select toko menyebabkan duplikasi** | Duplikat entri di log. | Pada proses **Generate Form**, sistem menolak toko yang sudah berada di status *Sent* pada hari yang sama. |

*Semua data yang disimpan bersifat **internal** dan **tidak mengandung data pelanggan akhir**. Pastikan server berada di jaringan VPN Erajaya dengan kontrol akses berbasis peran (role‑based access).*

---

**Catatan akhir:** Dokumen ini dirancang agar tim non‑engineer dapat langsung memasukkan spesifikasi ke dalam AI website/app builder (mis. Bubble, Softr, atau internal low‑code). Cukup pilih komponen UI, definisikan field sesuai tabel di atas, dan hubungkan aksi tombol ke endpoint dummy atau generator PDF. Setelah prototipe selesai, lakukan uji coba dengan 5 toko contoh untuk verifikasi alur.
