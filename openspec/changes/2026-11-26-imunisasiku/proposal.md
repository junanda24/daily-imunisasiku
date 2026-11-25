# Proposal — IMUNISASIKU! Jadwal Imunisasi Anak (IDAI 2024)

## Apa ini
Web app pelacak jadwal imunisasi anak berbasis rekomendasi **IDAI 2024** (Ikatan Dokter Anak Indonesia).
Orang tua memasukkan nama + tanggal lahir anak, dan app otomatis menyusun seluruh jadwal imunisasi
0–18 tahun lengkap dengan tanggal jatuh tempo, status (sudah / segera / terlewat / mendatang),
dan bisa menandai tiap vaksin yang sudah diberikan beserta tanggal, tempat, dan catatan.

## Kenapa ini mengejutkan
- **Bukan kalkulator receh.** Ini produk kesehatan keluarga yang nyata: ~40 jadwal vaksin dari data
  medis resmi (IDAI 2024, diverifikasi dari idai.or.id, denta.kidi.id, halodoc, biofarma),
  dihitung otomatis dari tanggal lahir, dengan logika catch-up (terlewat), pengingat tahunan
  (influenza), dan aturan kondisional (HPV khusus perempuan, JE daerah endemis, BIAS sekolah).
- **Sudut pandang yang tidak kepikiran:** semua orang tahu "imunisasi itu penting", tapi hampir
  tidak ada orang tua yang hafal 40 jadwal vaksin sampai anak umur 18 tahun — dan kartu imunisasi
  kertas gampang hilang. App ini mengubah satu tanggal lahir menjadi seluruh peta perlindungan anak.
- **Untuk siapa:** jutaan orang tua Indonesia dengan anak 0–18 tahun, plus kader posyandu /
  bidan yang butuh cek cepat jadwal pasien.

## Kenapa bentuk web app (bukan npm/Flutter/CLI)
- Target pengguna = orang tua awam, bukan developer → harus bisa dibuka langsung dari HP,
  tanpa install, tanpa login.
- Data sensitif anak sebaiknya tidak keluar dari HP → 100% client-side, localStorage,
  tanpa backend, tanpa akun. Backend tidak dibutuhkan: jadwal IDAI itu deterministik dari
  tanggal lahir (komputasi lokal murni).
- Rotasi bentuk output: kemarin (25 Nov) npm package → hari ini web app.

## Kenapa tanpa backend
Jadwal imunisasi adalah fungsi deterministik dari tanggal lahir + jenis kelamin.
Tidak ada data bersama, tidak ada login, tidak ada sinkronisasi. Backend hanya menambah
risiko privasi tanpa manfaat. Semua state bertahan di localStorage perangkat pengguna.

## Fitur inti
1. Profil multi-anak (nama, tanggal lahir, jenis kelamin), bisa tambah/hapus/ganti anak.
2. Jadwal otomatis IDAI 2024: ~40 item (HB, BCG, Polio, DTP-HB-Hib, PCV, Rotavirus,
   Influenza, MR/MMR, JE, Hepatitis A, Varisela, Tifoid, Dengue, HPV, Td/Tdap),
   masing-masing dengan tanggal jatuh tempo dari tanggal lahir.
3. Status otomatis: Sudah / Segera (≤14 hari) / Terlewat (catch-up) / Mendatang,
   plus hitung mundur hari ke vaksin berikutnya.
4. Tandai sudah imunisasi: tanggal pemberian, tempat (Posyandu/Puskesmas/RS/Dokter/Bidan),
   catatan; bisa batalkan (undo).
5. Dashboard: cincin progres imunisasi dasar lengkap (vaksin Program yang sudah jatuh tempo),
   kartu "vaksin berikutnya", penghitung terlewat.
6. Timeline per tonggak usia + filter (Semua / Berikutnya / Terlewat / Selesai).
7. Detail tiap vaksin: penyakit yang dicegah + catatan IDAI (Program, BIAS, endemis).
8. Influenza tahunan: item berulang yang jatuh temponya maju tiap tahun setelah ditandai.
9. Kartu Imunisasi: ringkasan siap cetak (print CSS) + salin ringkasan teks.
10. Perpustakaan vaksin: daftar semua vaksin + penyakit yang dicegah (edukasi).
11. Backup JSON (ekspor/impor) + hapus data.

## Uji bar TERCENGANG
- "Kalau Junan lihat jam 5 pagi, dia senyum kaget atau cuma 'oh'?" → Senyum kaget:
  satu tanggal lahir berubah jadi peta 18 tahun perlindungan anak, dari sumber medis resmi.
- Bukan sekelas kata-angka / Berani Resign: ada ~40 data nyata yang saling terkait,
  state persisten multi-anak, logika status + catch-up + recurring, alur end-to-end
  (tambah anak → lihat jadwal → tandai → cetak kartu).

## Risiko & mitigasi
- Akurasi jadwal: diverifikasi silang dari 4 sumber (IDAI PDF, denta.kidi.id, Halodoc,
  Biofarma). Disclaimer jelas di app: jadwal ini panduan umum, tetap ikuti dokter/posyandu.
- HPV hanya perempuan & JE endemis: aturan kondisional di kode + label jelas di UI.
