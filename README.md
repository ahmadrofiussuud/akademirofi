# akademirofi

MVP lokal untuk belajar BMC, BPC, dan Essay. React + Vite, Tailwind, Motion, Supabase. Tidak dideploy.

## Menjalankan

```powershell
cd C:\laragon\www\akademirofi
npm install
npm run dev
```

Buka http://127.0.0.1:3000. Tanpa env Supabase, mode demo aktif otomatis. Klik Masuk > Coba peserta atau Coba admin. Registrasi akun demo juga tersedia. Gunakan password khusus demo; akun tersimpan di browser, bukan autentikasi produksi. Data hilang jika penyimpanan browser dihapus. Progres dipisahkan berdasarkan pengguna; PDF upload disimpan di IndexedDB. Role demo dapat dimanipulasi di browser dan hanya untuk pengujian lokal.

## Supabase

1. Buat proyek Supabase atau instance lokal menggunakan Supabase CLI.
2. Jalankan `supabase/migrations/001_initial.sql` di SQL editor/migration runner. Skema ini untuk database baru.
3. Salin `.env.example` ke `.env.local`, isi project URL dan anon/publishable key. Jangan masukkan service-role key ke frontend.
4. Daftar pengguna, konfirmasi email jika diaktifkan. Supabase Auth Site URL untuk pengujian: `http://127.0.0.1:3000`.
5. Tetapkan admin melalui SQL editor tepercaya: `update public.profiles set role = 'admin' where id = '<UUID pengguna>';` Pengguna tidak dapat menaikkan role lewat aplikasi.
6. Restart dev server. Login admin untuk upload PDF dan publish materi. Database Supabase tidak diisi materi contoh otomatis.
7. Isi `VITE_WHATSAPP_NUMBER` dengan nomor Kak Rofi dalam format internasional tanpa tanda tambah. Jika kosong, menggunakan nomor Kak Rofi 6282142179454. Klik CTA dengan nomor valid membuka WhatsApp berisi draft; pengguna tetap menekan Kirim sendiri.

RLS membatasi materi terbit ke pengguna login, progres ke pemilik, dan mutasi materi ke admin. Bucket PDF privat dengan signed URL satu jam. Supabase belum diverifikasi terhadap layanan nyata jika credentials tidak disediakan.

## Pemeriksaan

```powershell
npm run build
npm run typecheck
npx playwright test
```

Tes browser memerlukan aplikasi berjalan di port 3000 dan Microsoft Edge terpasang. File tes mencakup registrasi/login/logout, gagal login, navigasi slide, resume, selesai lesson, isolasi progres, pesan WhatsApp, upload PDF, publish/unpublish, serta lebar mobile. PDF demo tiga slide dapat dibuat ulang dengan `node scripts/sample-pdf.mjs`.

## Batas MVP

- Enam lesson awal memakai PDF contoh yang sama, jelas ditandai; ganti dengan materi asli melalui admin.
- Kategori BMC/BPC/Essay tetap; modul adalah nama pengelompokan per lesson.
- Jumlah slide dimasukkan admin; belum dihitung otomatis dari PDF.
- Previous/Next menyimpan posisi yang dipilih pada kontrol aplikasi. Scroll internal viewer bawaan browser tidak dilacak.
- Admin dapat tambah/edit/publish/unpublish; penghapusan permanen dan pembersihan PDF lama belum tersedia.
- Belum ada reset password, pengujian Supabase live, atau pengiriman WhatsApp nyata.
- Browser dengan viewer PDF terbatas dapat menggunakan tautan Buka PDF di tab baru.

## Hasil verifikasi lokal

Build Vite dan pemeriksaan TypeScript berhasil. Tiga tes Playwright lulus pada Microsoft Edge: alur peserta, alur admin, dan lebar mobile. Pemeriksaan desktop tidak menemukan pageerror; video memiliki readyState 4 dan paused false. Supabase nyata dan pengiriman WhatsApp belum diuji.

## Pelengkapan alur

Halaman jalur menampilkan progres per kategori, filter admin mendukung kategori/status/pencarian, editor memperingatkan perubahan belum disimpan, dan PDF gagal muat menyediakan retry. Nomor WhatsApp Kak Rofi: 6282142179454. Tes WhatsApp memverifikasi URL dan isi draft melalui intersepsi; tidak mengirim pesan. Supabase live tetap memerlukan konfigurasi dan belum diuji.
