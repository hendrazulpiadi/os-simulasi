# OS Installation Simulator

Simulasi instalasi sistem operasi berbasis web untuk media pembelajaran Teknik Komputer
dan Jaringan (TKJ). Siswa berjalan menyusuri 5 langkah instalasi OS secara interaktif:
memilih OS, membuat bootable USB, mengatur BIOS, menjalankan wizard installer, hingga
desktop akhir — sambil mengumpulkan skor dan bintang.

Situs ini **berbasis satu berkas HTML** (`index.html`) tanpa build step, tanpa framework,
tanpa dependensi npm. Tinggal buka di browser atau deploy langsung ke GitHub Pages.

## Isi repository

| Berkas | Keterangan |
| --- | --- |
| `index.html` | Seluruh aplikasi (HTML + CSS + JS inline) |
| `.nojekyll` | Mematikan proses Jekyll agar struktur repo tidak diabaikan |
| `.gitattributes` | Normalisasi baris akhir ke LF |
| `.gitignore` | Berkas yang tidak perlu di-commit |

## Menjalankan secara lokal

Cukup buka `index.html` di browser, atau sajikan lewat server statis (disarankan agar
perilaku sama seperti di server):

```bash
# Python
python -m http.server 8000

# atau Node
npx serve .
```

Lalu buka `http://localhost:8000`.

> Membuka `index.html` lewat `file://` juga berfungsi, tetapi disarankan memakai server
> statis agar konsisten dengan lingkungan produksi.

## Deploy ke GitHub Pages

> **Penting:** GitHub Pages gratis hanya tersedia untuk repository **public**, dan
> berkas masuk harus bernama `index.html` serta berada di root repository. GitHub Pages
> tidak otomatis menampilkan `simulasi_instalasi_sistem_operasi.html` sebagai halaman
> utama dan tidak ada proses rename saat deploy — karena itu berkasnya sudah dinamai
> `index.html`.

### Deploy from a branch (dipakai repo ini)

1. Buka **Settings → Pages** pada repository.
2. Pada **Build and deployment → Source**, pilih **Deploy from a branch**.
3. Branch: `main`, folder: `/ (root)`.
4. Klik **Save**. GitHub mempublish `index.html` di root repo.

Alamat situsnya: `https://hendrazulpiadi.github.io/os-simulasi/`

Setiap `push` ke `main` otomatis memperbarui situs (dengan jeda ±1 menit).
Tidak ada proses build, sehingga tidak ada tooling yang perlu disiapkan.

### Opsional: auto-deploy via GitHub Actions

Alternatif bila nanti ingin proses deploy tercatat di tab **Actions** (dengan badge
status). Tambahkan `.github/workflows/pages.yml`, lalu ubah **Settings → Pages →
Source** menjadi **GitHub Actions**.

Perlu diperhatikan: workflow hanya bisa di-push dengan token yang punya scope
`workflow`, jadi pakai GitHub CLI perlu di-autentikasi ulang lebih dulu:

```bash
gh auth refresh -h github.com -s workflow
```

## Alur simulasi

1. **Pilih OS** — Windows 11 Pro atau Ubuntu 24.04 LTS (pilihan terkunci setelah 확정).
2. **Buat bootable USB** — jawab quiz, pilih ISO, lalu flashing dengan Rufus atau Ventoy.
3. **Hardware & BIOS** — pasang USB ke PC, nyalakan, ubah Boot #1 ke USB Flash Drive.
4. **Wizard installer** — partisi disk (Windows) atau konfigurasi pengguna (Ubuntu),
   lalu proses salin file.
5. **Selesai** — ringkasan skor, bintang, dan pratinjau desktop.

Skor maksimal 145 poin (Windows) / 125 poin (Ubuntu, tanpa langkah partisi).
Tiap aksi bernilai poin hanya bisa-direbut satu kali per putaran.

## Catatan teknis

- **Styling** memakai Tailwind CSS Play CDN (`cdn.tailwindcss.com`, versi 3.4.x),
  Font Awesome 6.4, dan Google Fonts — semuanya dimuat dari CDN, jadi halaman
  membutuhkan koneksi internet. Tidak ada aset gambar; ikon favicon dibuat inline
  sebagai SVG data-URI.
- **Audio** disintesis langsung dengan Web Audio API (tidak ada berkas audio) dan
  `AudioContext` baru dibuat setelah interaksi pertama pengguna agar tidak melanggar
  kebijakan autoplay browser.
- **State** hanya ada di memori; tidak memakai `localStorage`, sehingga simulasi selalu
  dimulai bersih saat halaman dimuat ulang.
- **Animasi** menghormati `prefers-reduced-motion`.

### Kalau ingin tanpa CDN

Tailwind Play CDN bersifat "development". Untuk pemakaian produksi/ofliline, bisa
dikompilasi Tailwind menjadi CSS statis dan mengganti tag `<script>` dengan
`<link rel="stylesheet">`. Karena proyek ini murni statis, tidak ada tooling build
yang perlu disiapkan.

## Lisensi

Belum ada lisensi. Tambahkan berkas `LICENSE` (mis. MIT) bila proyek ini akan dibagikan
secara terbuka.
