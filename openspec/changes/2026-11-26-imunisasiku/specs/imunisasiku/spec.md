# Spec — imunisasiku

## Capability: profil anak
- SHALL menyimpan daftar anak di localStorage (nama, tanggal lahir, jenis kelamin).
- SHALL mendukung tambah anak baru, hapus anak (dengan konfirmasi), dan ganti anak aktif.
- WHEN pengguna pertama kali membuka app dan belum ada anak THEN tampilkan layar sambutan +
  form tambah anak (nama wajib, tanggal lahir wajib, jenis kelamin wajib dipilih).
- WHEN tanggal lahir di masa depan THEN tolak dengan pesan error yang jelas.

## Capability: jadwal IDAI 2024
- SHALL menghitung tanggal jatuh tempo tiap item jadwal dari tanggal lahir anak
  (offset hari tetap per item, sesuai tabel IDAI 2024 di bawah).
- SHALL memuat minimal item-item berikut (offset hari dari lahir, nama, dosis, program?):
  - 0: Hepatitis B 0 (Program), Polio oral 0 / bOPV-0 (Program)
  - 0 (jatuh tempo 30 hari): BCG (Program)
  - 60: DTP-HB-Hib 1 (Program), Polio 1 bOPV (Program), PCV 1 (Program), Rotavirus 1
  - 90: DTP-HB-Hib 2 (Program), Polio 2 bOPV (Program)
  - 120: DTP-HB-Hib 3 (Program), Polio 3 bOPV (Program), IPV 1 (Program), PCV 2 (Program), Rotavirus 2
  - 180: PCV 3, Rotavirus 3
  - 210: Influenza 2 (dosis ke-2 tahun pertama)
  - 270: MR 1 (Program), IPV 2 (Program), JE 1 (endemis)
  - 365: PCV booster (Program), Hepatitis A 1, Varisela 1
  - 425: Varisela 2
  - 450: MR/MMR 2 (Program)
  - 540: DTP-HB-Hib booster (Program), Polio 4 (Program), Hepatitis A 2
  - 730: JE booster, Tifoid 1 (diulang tiap 3 tahun)
  - 1825: DT/DTPa booster (Program, BIAS kelas 1), MR/MMR 3 (Program, BIAS kelas 1)
  - 2190: Dengue 1 (TAK-003), 2280: Dengue 2
  - 2920: Tifoid 2, 4015: Tifoid 3, 5110: Tifoid 4, 6205: Tifoid 5
  - 3285: HPV 1 (perempuan saja), 3470: HPV 2 (perempuan saja)
  - 3650: Td/Tdap (Program, BIAS kelas 2 & 5)
- SHALL menyembunyikan item HPV bila jenis kelamin anak laki-laki; bila perempuan tampil
  dengan label "khusus anak perempuan, program BIAS kelas V–VI".
- SHALL menampilkan label Program / BIAS / Daerah endemis pada item yang bersangkutan.
- SHALL menampilkan untuk tiap vaksin: penyakit yang dicegah + catatan singkat IDAI.

## Capability: status & progres
- SHALL menghitung status tiap item: Sudah (ada catatan pemberian), Terlewat (belum dan
  tanggal jatuh tempo sudah lewat), Segera (belum dan jatuh tempo ≤14 hari lagi),
  Mendatang (sisanya).
- SHALL menampilkan vaksin berikutnya = item belum selesai dengan tanggal jatuh tempo
  terdekat (termasuk yang terlewat, yang terlewat didahulukan), beserta hitung mundur hari.
- SHALL menghitung progres "imunisasi dasar lengkap" = item Program yang tanggal jatuh
  temponya sudah tiba dan sudah ditandai, dibagi total item Program yang sudah jatuh tempo.
- SHALL menampilkan jumlah item Terlewat secara menonjol bila > 0, dengan ajakan catch-up.

## Capability: pencatatan pemberian
- WHEN pengguna menandai item sebagai sudah THEN simpan tanggal pemberian (default hari ini,
  bisa diubah, tidak boleh di masa depan), tempat (Posyandu/Puskesmas/RS/Klinik/Dokter/Bidan/Lainnya),
  dan catatan bebas (opsional).
- SHALL bisa membatalkan tanda (hapus catatan pemberian) dari detail item.
- SHALL memberi umpan balik perayaan singkat (confetti + pesan) saat menandai selesai.

## Capability: influenza tahunan
- SHALL menampilkan item Influenza sebagai item berulang: jatuh tempo pertama di usia
  6 bulan, lalu tiap 12 bulan setelah pemberian terakhir (atau tiap 12 bulan setelah
  jatuh tempo bila belum pernah diberikan), sampai usia 18 tahun.
- WHEN pengguna menandai influenza tahun berjalan THEN jatuh tempo berikutnya maju 12 bulan.

## Capability: timeline, filter, perpustakaan
- SHALL menampilkan timeline yang dikelompokkan per tonggak usia (Lahir, 2 bulan, 3 bulan,
  4 bulan, 6 bulan, 7 bulan, 9 bulan, 12 bulan, 14 bulan, 15 bulan, 18 bulan, 2 tahun,
  5 tahun, 6 tahun, 7 tahun, 8 tahun, 9 tahun, 10 tahun, 11 tahun, 14 tahun, 17 tahun).
- SHALL menyediakan filter: Semua / Berikutnya / Terlewat / Selesai.
- SHALL menyediakan tab Perpustakaan Vaksin: daftar semua vaksin + penyakit yang dicegah
  + usia pemberian, bisa dicari.
- SHALL menyediakan tampilan Kartu Imunisasi (ringkasan per anak: sudah vs belum) dengan
  tombol Cetak (print CSS rapi) dan Salin Ringkasan.

## Capability: data & privasi
- SHALL menyimpan SEMUA data hanya di localStorage perangkat (tanpa backend, tanpa akun).
- SHALL menyediakan ekspor JSON (unduh) dan impor JSON (pulihkan), serta hapus semua data
  dengan konfirmasi.
- SHALL menampilkan disclaimer: "Jadwal mengikuti rekomendasi IDAI 2024 sebagai panduan
  umum. Keputusan akhir tetap mengikuti dokter anak / petugas kesehatan."

## Capability: UX
- SHALL mobile-first, responsif, bisa dipakai satu tangan di HP.
- SHALL memakai Bahasa Indonesia natural, tanpa lorem ipsum, tanpa emoji sebagai ikon
  (ikon SVG), tanpa placeholder palsu.
- SHALL punya momen delight: animasi cincin progres, confetti saat menandai selesai,
  transisi halus; hormati prefers-reduced-motion.
- SHALL tidak ada tombol mati, tidak ada error console.
