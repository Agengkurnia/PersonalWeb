# Standar Pembuatan & Update Dokumen CV (DOCX)

Standar ini dipakai AI (dan manusia) saat menambah **skill** atau **experience** ke CV.  
Update dilakukan **manual via AI** (perintah di chat), bukan lewat form/CLI wajib. Project ini bisa dihubungkan ke workspace lain; aturan folder & penamaan di bawah tetap berlaku.

---

## 1. Tujuan

- Menjaga CV Word tetap rapi dan konsisten setiap ada perubahan skill / experience.
- Selalu punya **versi terkini** di `Document/Output`.
- Selalu punya **jejak perubahan** di `Document/Output/History`.

---

## 2. Struktur Folder (kontrak)

```
Document/
  Master/
    CV - Ageng Kurniawan Sugianto.docx   ← template awal (hanya dibaca / jarang diubah)
  Output/
    CV - Ageng Kurniawan Sugianto.docx   ← sumber acuan & hasil terkini
    History/
      YYYYMMDD_CV_AgengSugianto.docx     ← snapshot per hari
Source/
  CV-Document-Standard.md                ← standar ini
```

| Path | Peran |
|------|--------|
| `Document/Master/CV - Ageng Kurniawan Sugianto.docx` | Template awal. **Jangan** jadi sumber edit harian. |
| `Document/Output/CV - Ageng Kurniawan Sugianto.docx` | **Source of truth** setelah bootstrap. Semua edit berikutnya dari sini. Nama file **sama** dengan Master. |
| `Document/Output/History/YYYYMMDD_CV_AgengSugianto.docx` | Arsip tiap ada perubahan. |

---

## 3. Source of Truth

1. **Pertama kali** (belum ada file di `Output`): salin dari Master → `Document/Output/CV - Ageng Kurniawan Sugianto.docx`.
2. **Selanjutnya**: selalu buka & edit dari `Document/Output/...`.
3. Master hanya diubah jika user **eksplisit** minta sinkronkan ulang template dasar.

---

## 4. Alur Kerja Saat AI Diminta Update

Setiap permintaan menambah/mengubah **skill** atau **experience**, AI wajib mengikuti urutan ini:

```
1. Bootstrap (jika Output belum ada)
   └─ Copy Master → Output (nama file sama)

2. Baca Output terkini
   └─ Document/Output/CV - Ageng Kurniawan Sugianto.docx

3. Simpan History DULU (sebelum menimpa Output)
   └─ Copy Output saat ini → History/YYYYMMDD_CV_AgengSugianto.docx
   └─ Jika file History tanggal hari ini sudah ada → timpa (1 snapshot per hari = versi terakhir hari itu)

4. Terapkan perubahan konten pada salinan kerja
   └─ Skill → bagian TECHNICAL SKILLS
   └─ Experience → bagian PROFESSIONAL EXPERIENCE
   └─ Pertahankan formatting / gaya dokumen sebisa mungkin

5. Tulis hasil akhir
   └─ Timpa Document/Output/CV - Ageng Kurniawan Sugianto.docx
```

**Catatan History:** History menyimpan kondisi **sebelum** perubahan hari itu diterapkan (atau, jika sudah ada update di hari yang sama, menimpa snapshot hari itu dengan kondisi sebelum overwrite Output terbaru — praktis: selalu copy Output ke History tanggal hari ini tepat sebelum menulis Output baru).

---

## 5. Aturan Penamaan

| Lokasi | Format nama | Contoh |
|--------|-------------|--------|
| Output (current) | `CV - Ageng Kurniawan Sugianto.docx` | sama dengan Master |
| History | `YYYYMMDD_CV_AgengSugianto.docx` | `20260810_CV_AgengSugianto.docx` |

- `YYYYMMDD` = tanggal lokal saat update (zona waktu mesin user).
- Satu file History per tanggal; update berulang di hari yang sama **menimpa** file History tanggal itu.

---

## 6. Aturan Konten yang Boleh Diubah

### 6.1 TECHNICAL SKILLS

- Tambah / ubah item di kategori yang sudah ada, misalnya:
  - Frontend
  - Backend
  - Database
  - Infrastructure
  - Tools & Others
- Jangan buat kategori baru kecuali user meminta.
- Hindari duplikat skill yang sama (case-insensitive).
- Gaya penulisan mengikuti dokumen yang ada (daftar dipisah koma / label kategori).

### 6.2 PROFESSIONAL EXPERIENCE

Setiap entri experience idealnya punya:

- Nama perusahaan (+ legal entity jika ada di pola lama)
- Lokasi (mis. Jakarta, Indonesia)
- Job title / role
- Periode (konsisten dengan format yang sudah dipakai di CV, mis. `Jan 2024 – 2025`)
- **1–2 bullet garis besar** saja (tanggung jawab / fokus role)

**Tingkat detail CV: RINGKAS.** Jangan masukkan detail modul, nama project internal, alur teknis, daftar field, atau penjelasan panjang. Cukup gambaran umum role. Detail mendalam milik FSD/prototype/portfolio — bukan CV.

Urutan: **pengalaman terbaru di atas** (chronological descending), kecuali user minta lain.

### 6.3 Yang tidak diubah tanpa permintaan eksplisit

- PROFESSIONAL SUMMARY
- KEY ACHIEVEMENTS
- EDUCATION
- LANGUAGES
- Header (nama, email, GitHub, lokasi)
- Layout, font embedded, warna, atau styling global dokumen

---

## 7. Perintah yang Diharapkan dari User (contoh)

AI mengenali intent seperti:

- “Tambah skill Docker Swarm ke Infrastructure”
- “Tambah experience di perusahaan X sebagai Y dari Jan 2026”
- “Update title role Kalbe jadi …”

Setelah selesai, AI melaporkan singkat:

- File Output yang di-update
- File History yang dibuat/ditimpa
- Ringkasan perubahan (skill / experience apa)

---

## 8. Integrasi Workspace Lain

Project ini boleh di-attach ke workspace lain. Aturan wajib tetap:

1. Path relatif di atas (`Document/...`, `Source/...`) tidak diganti semaunya.
2. Standar acuan tetap file ini: `Source/CV-Document-Standard.md`.
3. AI di workspace lain harus membaca standar ini sebelum mengubah CV.
4. Jangan commit file `.docx` hasil generate kecuali user meminta.

---

## 9. Helper (opsional, disarankan)

Boleh ada script kecil (mis. Python + `python-docx`) yang hanya mengurus:

- Bootstrap Master → Output
- Copy Output → History dengan nama `YYYYMMDD_CV_AgengSugianto.docx`
- Simpan Output final

Isi skill/experience tetap ditentukan AI (atau parameter yang AI isi), agar formatting CV tetap terkendali.

Jika helper belum ada, AI tetap wajib mengikuti alur di §4 secara manual dengan tool yang tersedia.

---

## 10. Definition of Done

Update dianggap selesai jika:

- [ ] `Document/Output/CV - Ageng Kurniawan Sugianto.docx` berisi perubahan terbaru
- [ ] `Document/Output/History/YYYYMMDD_CV_AgengSugianto.docx` ada untuk tanggal update
- [ ] Master tidak berubah (kecuali user minta)
- [ ] Hanya bagian skill dan/atau experience yang diminta yang berubah
- [ ] User mendapat ringkasan path + perubahan

---

## 11. Ringkas (cheat sheet)

```
Baca:     Document/Output/CV - Ageng Kurniawan Sugianto.docx
          (jika belum ada ← copy dari Master)
History:  Document/Output/History/YYYYMMDD_CV_AgengSugianto.docx  (sebelum overwrite Output)
Tulis:    Document/Output/CV - Ageng Kurniawan Sugianto.docx
Edit:     TECHNICAL SKILLS | PROFESSIONAL EXPERIENCE saja (default)
Acuan:    Source/CV-Document-Standard.md
```
