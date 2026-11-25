# Design — IMUNISASIKU!

## Visual direction (dari ui-ux-pro-max `--design-system`, query "parenting health tracker baby immunization app")
- **Style: Claymorphism** — soft 3D, chunky, playful, bubbly; border tebal 3–4px,
  rounded 16–24px, double shadow lembut. Cocok untuk app anak/orang tua.
- **Mode: Operate** (impeccable) — pengguna menyelesaikan tugas: cek jadwal, tandai vaksin.
  Scanability dan kejelasan status lebih penting dari ekspresi.
- **Palet (token CSS):**
  - Primary pink `#EC4899` (hangat, ramah), aksen biru kepercayaan `#0284C7`
  - Background `#FDF2F8`, kartu `#FFFFFF`, teks `#0F172A`, muted `#475569`
  - Status: Sudah = hijau `#16A34A`, Segera = amber `#D97706`, Terlewat = merah `#DC2626`,
    Mendatang = abu `#64748B`
- **Tipografi:** Varela Round (judul, rounded & ramah) + Nunito Sans (body).
  Google Fonts CDN. Base 16px, line-height 1.5.
- **Ikon:** SVG inline (Lucide-style, stroke konsisten), bukan emoji.
- **Tanpa kicker/eyebrow di atas heading** (craft-floor ban). Heading bicara sendiri.

## Struktur (web artifact static, single-file, pola ARCHITECTURE.md §1)
```
index.html   # struktur + mounting point
styles.css   # design token :root → komponen → responsive (di-inline di artifact)
app.js       # state → render → events (di-inline di artifact)
```
- **STATE** — satu objek `state`: `{ children: [], activeChildId, filter, tab, detailId }`.
  Semua data anak + catatan pemberian di `state.children[].records`.
- **DATA** — konstanta `SCHEDULE` (array item: id, vaksin, dosis, offsetDays, program,
  bias, endemis, femaleOnly, disease, note) + `VACCINE_LIBRARY` terpisah dari logic.
- **STORE** — satu modul `store` untuk localStorage (`imunisasiku.v1`): load/save saja.
- **RENDER** — fungsi murni state → DOM (renderDashboard, renderTimeline, renderLibrary,
  renderCard). Tidak ada logic bisnis di render.
- **EVENTS** — handler tipis: baca input → update state → save → render.
- **LOGIC** — fungsi bernama: `dueDate(child, item)`, `itemStatus(child, item, today)`,
  `nextVaccine(child)`, `coreProgress(child)`, `fluDueDate(child)`.

## Komponen / layar
1. **Header** — logo "IMUNISASIKU!" + switcher anak (chip) + tombol tambah anak.
2. **Dashboard** — kartu "Vaksin Berikutnya" (nama, tanggal, countdown hari, tombol "Tandai Sudah"),
   cincin progres imunisasi dasar lengkap (SVG stroke-dashoffset, animasi), chip statistik
   (Selesai / Segera / Terlewat).
3. **Timeline** — filter chips (Semua/Berikutnya/Terlewat/Selesai), grup per tonggak usia,
   kartu item: nama vaksin + dosis, tanggal jatuh tempo, badge status, tombol tandai.
4. **Detail sheet** (modal bottom-sheet mobile) — penyakit dicegah, catatan IDAI,
   form tandai (tanggal, tempat, catatan) atau info pemberian + tombol batalkan.
5. **Perpustakaan Vaksin** — kartu per vaksin + kolom pencarian.
6. **Kartu Imunisasi** — ringkasan per anak + tombol Cetak & Salin Ringkasan & Ekspor/Impor JSON.
7. **Onboarding** — empty state hangat + form tambah anak pertama.

## Animasi & delight
- Cincin progres menganimasi saat nilai berubah (CSS transition stroke-dashoffset).
- Confetti canvas sederhana (partikel warna palet) saat menandai selesai + toast
  "Hebat! [Nama] sudah terlindungi dari [penyakit]."
- Entrance stagger halus pada kartu timeline (CSS, back.out-ish via cubic-bezier).
- `prefers-reduced-motion`: matikan confetti & stagger.

## State — lima keranjang (ARCHITECTURE.md)
- Lokal: filter, tab aktif, detail sheet terbuka → `useState`-setara (variabel state lokal).
- Persisten: data anak + records → localStorage via modul `store` (bukan global store server).
- Tidak ada server state, tidak ada URL state (single-page app, tab via state lokal).

## Verifikasi
- Semua SHALL di spec ditelusuri ke fitur; tidak ada tombol mati; tidak ada console error.
- Tanggal diuji: anak lahir hari ini → HB-0/Polio-0/BCG jatuh tempo benar; anak lahir
  2025-01-01 → DTP-1 jatuh tempo 2025-03-02 (60 hari); anak laki-laki → HPV tersembunyi.
- Print CSS dicek (kartu tercetak rapi, tanpa tombol).
