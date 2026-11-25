# IMUNISASIKU! — Jadwal Imunisasi Anak

**Pelacak jadwal imunisasi anak berdasarkan rekomendasi IDAI 2024.**

Satu tanggal lahir berubah menjadi peta perlindungan anak selama 18 tahun. Orang tua
memasukkan nama + tanggal lahir anak, dan app otomatis menyusun seluruh jadwal imunisasi
— lengkap dengan tanggal jatuh tempo, status (sudah / segera / terlewat / mendatang),
dan pencatatan pemberian (tanggal, tempat, catatan). Semua data tersimpan di HP sendiri
(localStorage), tanpa login, tanpa backend, tanpa akun.

> **Disclaimer:** Jadwal mengikuti rekomendasi IDAI 2024 sebagai panduan umum.
> Keputusan akhir tetap mengikuti dokter anak / petugas kesehatan.

## Kenapa ide ini dipilih

Semua orang tahu "imunisasi itu penting", tapi hampir tidak ada orang tua yang hafal
~40 jadwal vaksin sampai anak umur 18 tahun — dan kartu imunisasi kertas gampang hilang.
IMUNISASIKU! mengubah satu tanggal lahir menjadi seluruh peta perlindungan anak, dari
data medis resmi (IDAI 2024, diverifikasi silang dari idai.or.id, denta.kidi.id,
Halodoc, dan Biofarma). Efek kejutnya: yang biasanya butuh bolak-balik tanya bidan dan
bongkar buku KIA, sekarang terjawab dalam sekali buka.

## Fitur

- **Profil multi-anak** — tambah anak (nama, tanggal lahir, jenis kelamin), ganti anak
  aktif, hapus anak dengan konfirmasi. Validasi: tanggal lahir di masa depan ditolak.
- **Jadwal otomatis IDAI 2024** — 41 item: Hepatitis B, BCG, Polio (bOPV + IPV),
  DTP-HB-Hib, PCV, Rotavirus, Influenza, MR/MMR, JE, Hepatitis A, Varisela, Tifoid,
  Dengue (TAK-003), HPV, Td/Tdap. Tiap item punya tanggal jatuh tempo, penyakit yang
  dicegah, catatan IDAI, dan label Program / BIAS / Daerah endemis.
- **Status otomatis** — Sudah / Segera (≤14 hari) / Terlewat (catch-up) / Mendatang,
  plus hitung mundur hari ke vaksin berikutnya (yang terlewat didahulukan).
- **Cincin progres "Imunisasi Dasar Lengkap"** — persen vaksin Program yang sudah
  jatuh tempo dan sudah diberikan.
- **Tandai sudah imunisasi** — tanggal pemberian, tempat (Posyandu/Puskesmas/RS/
  Klinik/Dokter anak/Bidan/Lainnya), catatan bebas; bisa dibatalkan. Ada confetti +
  toast perayaan setiap kali menandai selesai.
- **Influenza tahunan** — item berulang: jatuh tempo pertama usia 6 bulan, lalu maju
  12 bulan setiap kali ditandai.
- **Aturan kondisional** — HPV otomatis tersembunyi untuk anak laki-laki (khusus
  perempuan, program BIAS kelas V–VI); JE diberi label daerah endemis.
- **Timeline per tonggak usia** — Lahir, 2 bulan, 3 bulan, … 17 tahun, dengan filter
  Semua / Berikutnya / Terlewat / Selesai.
- **Perpustakaan Vaksin** — 16 vaksin + penyakit yang dicegah + usia pemberian,
  bisa dicari.
- **Kartu Imunisasi** — ringkasan per anak, siap cetak (print CSS hanya mencetak kartu),
  salin ringkasan teks, ekspor/impor JSON (backup & pulihkan), hapus semua data.

## Cara pakai

1. Buka link hasil di HP (atau buka `src/index.html` langsung di browser).
2. Isi nama, tanggal lahir, dan jenis kelamin anak pertama → **Simpan**.
3. Lihat kartu "Vaksin Berikutnya" dan cincin progres di dashboard.
4. Ketuk vaksin di timeline untuk melihat detail, lalu **Tandai Sudah** setelah
   imunisasi diberikan (isi tanggal, tempat, catatan).
5. Tambah anak lain lewat chip nama di header.
6. Buka tab **Kartu** untuk mencetak atau menyalin ringkasan imunisasi anak.

## Cara menjalankan dari source

Tidak perlu build tool apa pun — ini satu file HTML statis:

```bash
# cukup buka di browser
open src/index.html        # macOS
xdg-open src/index.html    # Linux
```

Atau sajikan lewat server statis apa pun:

```bash
npx serve src
# atau
python3 -m http.server --directory src 8000
```

## Tech stack

- HTML + CSS + JavaScript vanilla (single-file, tanpa framework, tanpa build step)
- localStorage untuk persistensi (`imunisasiku.v1`)
- Google Fonts: Varela Round (judul) + Nunito Sans (body)
- Gaya Claymorphism (soft 3D, border tebal, rounded besar) — pink `#EC4899` +
  biru kepercayaan `#0284C7` di atas `#FDF2F8`

## Struktur folder

```
2026-11-26-imunisasiku/
├── README.md
├── src/
│   └── index.html            # app lengkap (single-file)
└── openspec/
    └── changes/
        ├── 2026-11-26-imunisasiku/
        │   ├── proposal.md
        │   ├── design.md
        │   ├── tasks.md
        │   └── specs/imunisasiku/spec.md
        └── archive/
            └── 2026-11-26-imunisasiku/   # salinan arsip change
```

## Link hasil

- **App:** artifact `imunisasiku-jadwal-imunisasi-anak` (link kartu di laporan harian)
- **Repo:** https://github.com/junanda24/daily-imunisasiku
- **Rilis:** 2026-11-26 — #42 program "1 Hari 1 Project"

## Catatan / temuan saat build

- Jadwal IDAI 2024 diverifikasi silang dari 4 sumber: PDF resmi IDAI
  (idai.or.id), denta.kidi.id, Halodoc, dan Biofarma. Semua offset hari di spec
  mengikuti tabel tersebut (DTP 2,3,4 bln; PCV 2,4,6 bln + booster 12–15 bln;
  MR 9 bln; booster 18 bln; BIAS 5 & 10 tahun; Dengue 6 tahun; HPV 9–9,5 tahun).
- Influenza dimodelkan sebagai item berulang (bukan 18 entri tetap) supaya timeline
  tetap bersih; jatuh tempo berikutnya dihitung dari pemberian terakhir.
- Tombol Cetak memakai `window.print()` — diblokir di dalam iframe sandbox Muse,
  tapi berfungsi normal saat halaman dibuka langsung di browser/perangkat pengguna.
  Print CSS sudah membatasi hasil cetak hanya pada kartu imunisasi.
- Sengaja TIDAK dibuat: pengingat notifikasi push (butuh backend/service worker +
  izin; di luar scope client-side murni), sinkronisasi antar-perangkat (alasan
  privasi data anak), dan jadwal catch-up otomatis yang mengubah tanggal (tetap
  tampil sebagai "Terlewat" supaya orang tua konsultasi ke dokter/posyandu).

## Lisensi

MIT
