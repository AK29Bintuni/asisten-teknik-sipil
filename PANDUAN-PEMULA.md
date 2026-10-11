# Panduan pemula — Asisten Teknik Sipil & Proyek Konstruksi

Paket ini adalah **starter aplikasi web**, belum website online sampai dipublikasikan di GitHub/Vercel dan dihubungkan ke proyek Supabase milik Anda.

## Isi yang tersedia
- Login email + kata sandi melalui Supabase Auth.
- Dashboard daftar proyek yang diizinkan oleh Row Level Security.
- Tambah proyek dan item pekerjaan sederhana.
- Hitung subtotal volume × harga satuan.
- Ekspor daftar item sebagai CSV yang dapat dibuka di Excel.
- Cetak / simpan halaman sebagai PDF dari browser.

Belum termasuk AHSP resmi, harga satuan daerah terverifikasi, pajak, S-curve, jadwal, desain geometrik/perkerasan, CAD/DWG/BIM, atau validasi teknis otomatis. Jangan memakai subtotal awal sebagai RAB final tanpa pemeriksaan.

## 1. Unggah ke GitHub
1. Masuk GitHub dan buat repository baru bernama `asisten-teknik-sipil`.
2. Untuk memudahkan, pilih **Add a README file** saat membuat repository.
3. Di repository, gunakan **Add file → Upload files** dan unggah semua isi folder paket ini (bukan folder ZIP-nya). Alternatif: unggah ZIP lalu ekstrak secara lokal terlebih dahulu.
4. Klik **Commit changes**.
5. Jangan unggah file `.env` berisi kunci rahasia. File `.env.example` hanya contoh.

## 2. Terbitkan di Vercel
1. Buka https://vercel.com/ lalu masuk dengan GitHub.
2. Klik **Add New → Project** dan impor repository `asisten-teknik-sipil`.
3. Framework biasanya terdeteksi sebagai Next.js.
4. Sebelum menekan Deploy, tambahkan Environment Variables:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
5. Ambil nilai di Supabase → Project Settings → API (atau Data API / API Keys sesuai tampilan). URL adalah Project URL. Untuk key, gunakan **publishable key** (awalan `sb_publishable_`) atau anon key lama yang memang disediakan untuk browser. Jangan gunakan `service_role` atau `secret key`.
6. Klik Deploy. Tunggu sampai Vercel memberi alamat website.

## 3. Atur URL Auth Supabase
Setelah mendapat alamat Vercel:
1. Supabase → Authentication → URL Configuration.
2. Isi Site URL dengan alamat website Vercel Anda (https://...vercel.app).
3. Tambahkan alamat yang sama ke Redirect URLs jika diminta.
4. Simpan perubahan.

## 4. Buat akun pemilik pertama
Karena pendaftaran publik dimatikan:
1. Supabase → Authentication → Users.
2. Pilih Add user / Create new user.
3. Masukkan email Anda dan buat kata sandi kuat. Jika tersedia, tandai email terkonfirmasi hanya jika Anda telah memverifikasi bahwa alamat itu milik Anda.
4. Login melalui website.

## 5. Penting tentang akses proyek
Skema database awal yang dipasang sebelumnya punya kebijakan anggota proyek. Akun pertama harus membuat proyek lewat aplikasi, sehingga trigger otomatis menjadikan pembuat sebagai owner. Setelah itu, penambahan anggota belum tersedia di UI starter ini. Jangan menambahkan anggota langsung ke tabel tanpa memeriksa kebijakan akses dan alur undangan terlebih dahulu.

## Pemecahan masalah
- Login gagal: pastikan akun ada di Authentication → Users dan email sudah dikonfirmasi.
- `Pengaturan Supabase belum diisi`: periksa dua Environment Variables di Vercel lalu lakukan redeploy.
- `Invalid API key`: pastikan memakai publishable/anon key, bukan secret/service_role.
- `new row violates row-level security policy`: akun belum memiliki akses yang sesuai atau skema SQL berbeda dari panduan.
- Error build: lihat Build Logs Vercel, salin teks error tanpa kunci rahasia.

## Keamanan
- Jangan bagikan kata sandi, OTP, recovery codes, `service_role`, atau secret key.
- Gunakan publishable/anon key pada browser; RLS harus aktif.
- Paket ini belum diaudit keamanan dan belum diuji untuk penggunaan produksi. Uji dengan data contoh dahulu.
