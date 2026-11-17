ALVEND Ready-Mix Management — Setup Supabase
Panduan singkat untuk mengubah aplikasi agar menggunakan Supabase sebagai storage (menggantikan localStorage).

1. Persiapan di Supabase
1.1 Buat Project Supabase
Buka https://supabase.com → New Project
Name: Alvend Ready Mix
Set password database → Create project
1.2 Jalankan SQL Schema
Buka SQL Editor di sidebar Supabase
Klik New Query
Buka file supabase_schema.sql (sudah disertakan)
Copy semua isinya → paste ke editor
Klik Run (Ctrl + Enter)
Pastikan tidak ada error. Sukses akan muncul "Success. No rows returned"
1.3 Ambil API Key
Buka Project Settings → API Keys
Salin Publishable key (format: sb_publishable_xxxx_yyyy)
Jangan gunakan Secret key di sisi browser! Hanya untuk server.
Catat juga Project URL (di Settings → General → https://xxxx.supabase.co)
2. Konfigurasi Aplikasi
Buka file alvend_readymix_supabase.html dengan text editor. Cari bagian paling atas pada <script>:

javascript
const SUPABASE_URL = 'https://74S88rRafH0wVi70p40hWA.supabase.co';
const SUPABASE_PUBLISHABLE_KEY = 'sb_publishable_74S88rRafH0wVi70p40hWA_b8ivR...';
Ganti dua nilai di atas dengan URL dan Publishable Key dari project Supabase Anda sendiri.

URL sudah diisi otomatis berdasarkan prefix publishable key yang terlihat di screenshot. Jika project URL Anda berbeda, ganti sesuai dashboard.

3. Menjalankan Aplikasi
Cukup buka file alvend_readymix_supabase.html di browser (Chrome/Edge/Firefox).

Tidak perlu web server — file ini standalone.

Akun Demo
Role
User ID / Email
Password
Developer	bookingeast0@gmail.com	sultan3110
Owner	owner	password
Admin	admin	password
Sales	sales1	password
Customer	customer	password

Akun Developer (Email Auth)
Akun developer bisa login dengan 2 cara:

User ID: developer + password sultan3110
Email: bookingeast0@gmail.com + password sultan3110
Login via Supabase Auth (Opsional — Lebih Aman)
Default-nya, akun developer diverifikasi via credential lokal (localStorage).
Untuk meningkatkan keamanan, daftarkan juga di Supabase Authentication:

Buka Supabase Dashboard → Authentication → Users → Add user
Email: bookingeast0@gmail.com
Password: sultan3110
Centang Auto Confirm User (supaya bisa langsung login tanpa verifikasi email)
Klik Create user
Setelah didaftarkan, app otomatis memakai Supabase Auth saat developer login via email. Bila belum didaftarkan, app tetap fallback ke credential lokal — login tetap berfungsi.

Catatan: Akun developer TIDAK muncul di tabel users Supabase dan TIDAK muncul di menu Kelola User / Log Aktivitas. Aktivitas developer juga TIDAK dicatat di log aktivitas.

4. Yang Berubah dari Versi localStorage
Aspek
localStorage (lama)
Supabase (baru)
Storage	localStorage browser	Supabase PostgreSQL (cloud)
Multi-user sharing	❌ Tidak bisa	✅ Semua user lihat data yang sama
Data hilang	Jika clear browser cache	Tetap aman di Supabase
Operasi	Sinkron (blocking)	Asynchronous (Promise-based)
Schema	Tidak perlu	Wajib jalankan SQL supabase_schema.sql
Akun Developer	localStorage	Tetap localStorage (tidak di-cloud-kan)
Session login	localStorage beton_current	Tetap localStorage (agar tetap login setelah refresh)
Activity log	localStorage (auto-prune 24 jam)	Tabel login_logs di Supabase (auto-prune 24 jam via app)

5. ⚠ Catatan Keamanan (PENTING)
Pada versi demo ini, RLS (Row Level Security) DISEMBUNYIKAN pada semua tabel. Ini artinya:

Siapapun yang punya publishable key bisa baca/tulis semua data
Password disimpan plaintext di tabel users (mengikuti pola lama)
Untuk Production, WAJIB:
Enable RLS di setiap tabel
Buat policy berbasis role atau auth.uid()
Gunakan Supabase Auth (email/password atau OAuth) — jangan simpan password plaintext
Pindahkan logic sensitif ke Edge Functions (gunakan Secret Key di server-side)
Hapus akun demo atau ganti passwordnya
Setup backup otomatis di Supabase
6. Troubleshooting
Error: "Supabase belum terhubung"
Periksa SUPABASE_URL dan SUPABASE_PUBLISHABLE_KEY sudah benar
Cek koneksi internet
Pastikan tidak ada script blocker (uBlock, dll)
Error: "relation users does not exist"
Berarti SQL schema belum dijalankan
Ulangi langkah 1.2
Data tidak muncul
Buka Browser DevTools (F12) → tab Console → cek error
Buka tab Network → cek request ke Supabase (status 200 OK)
Cek Supabase Dashboard → Table Editor → pastikan ada data di tabel users
Login gagal padahal user sudah ada
Pastikan is_active = true di tabel users
Pastikan password cocok (case-sensitive)
Cek RLS belum enable (jika enable, perlu policy)
Tabel masih kosong
Buka Supabase → Table Editor → pilih tabel → harusnya ada data seed
Jika kosong, jalankan ulang supabase_schema.sql
7. File Deliverable
File
Keterangan
alvend_readymix_supabase.html	Aplikasi utama (buka di browser)
supabase_schema.sql	SQL untuk setup tabel + seed di Supabase
README_SETUP.md	Dokumen ini

Selamat menggunakan! 🚀
