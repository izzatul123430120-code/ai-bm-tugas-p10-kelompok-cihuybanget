# Segmentasi Polip pada Citra Kolonoskopi dengan U-Net

<!--
  TEMPLATE LAPORAN — Tugas P10 Kecerdasan Buatan (BM25-41204), Teknik Biomedis ITERA.
  Petunjuk lengkap: https://mctm.web.id/ai-bm/tugas

  Cara memakai:
  - Ganti semua teks yang diawali "✍️" dengan tulisan kelompokmu, lalu hapus tanda ✍️-nya.
  - Blok komentar seperti ini adalah petunjuk; tidak tampil di GitHub. Boleh dihapus.
  - Gambar diambil dari folder hasil/ yang dibuat otomatis oleh notebook.
  - Jangan hapus judul-judul bagian (##).
-->

**Tugas P10 — Kecerdasan Buatan (BM25-41204), Teknik Biomedis ITERA**

## Anggota kelompok

| Nama | NIM | Username GitHub | Kontribusi utama |
|---|---|---|---|
| ✍️ | ✍️ | ✍️ | ✍️ misal: menjalankan eksperimen, menulis Bagian 3 |
| ✍️ | ✍️ | ✍️ | ✍️ |
| ✍️ | ✍️ | ✍️ | ✍️ |

## 1. Pertanyaan

Augmentasi apa yang membantu U-Net menemukan polip pada citra kolonoskopi, dan augmentasi apa yang tidak? Kenapa?

## 2. Data & pengaturan

<!-- Isi dari Bagian 2 notebook. Pastikan CEPAT = False untuk hasil akhir. -->

- Dataset: Kvasir-SEG (1000 citra kolonoskopi + mask polip), diperkecil ke 128 × 128 piksel.
- Data uji: 200 citra (tetap). Data latih: ✍️ citra per ulangan.
- Model: U-Net mini (±0,5 juta parameter), ✍️ epoch, ✍️ ulangan per skenario.
- Dijalankan di: ✍️ (misal: Google Colab T4 / laptop CPU / Mac M1).

![Contoh data](hasil/contoh_data.png)

## 3. Skenario augmentasi & hipotesis

<!-- Tulis bagian ini SEBELUM menjalankan eksperimen. Commit dulu, baru jalankan.
     Alasan klinis = kenapa variasi ini mungkin (atau tidak mungkin) terjadi pada kolonoskopi sungguhan. -->

| Skenario | Augmentasi (kode) | Alasan klinis | Hipotesis: Dice naik/turun/sama? |
|---|---|---|---|
| Tanpa augmentasi | — | Pembanding | — |
| Horizontal flip | `v2.RandomHorizontalFlip()` | Polip dapat muncul di sisi kiri maupun kanan lumen usus, sehingga model perlu tidak bias terhadap orientasi horizontal | Naik: variasi orientasi membantu generalisasi |
| Vertical flip | `v2.RandomVerticalFlip()` | Sudut masuk endoskop bervariasi sehingga posisi atas-bawah polip tidak selalu konsisten | Naik: alasan serupa dengan horizontal flip |
| Brightness & contrast | `v2.ColorJitter(brightness=0.3, contrast=0.3)` | Intensitas cahaya endoskop tidak merata dan berubah tergantung jarak kamera ke dinding usus | Naik: model lebih tahan terhadap variasi pencahayaan |

Contoh augmentasi yang dicoba:

![Contoh augmentasi](hasil/augmentasi_flip.png)
<!-- Ganti nama file sesuai skenario kalian (lihat isi folder hasil/). -->

## 4. Hasil

<!-- Tempel isi hasil/ringkasan.md di sini. -->

✍️ tempel tabel dari `hasil/ringkasan.md`

![Perbandingan Dice](hasil/perbandingan.png)

## 5. Analisis peta kesalahan

![Peta kesalahan](hasil/peta_kesalahan.png)

<!-- Hijau = benar, merah = false positive, biru = false negative.
     Pertanyaan pemandu: Polip seperti apa yang sering terlewat (kecil? datar? warna mirip dinding usus?)
     Apa yang sering dikira polip padahal bukan? Apakah augmentasi memperbaiki kasus sulit? -->

✍️

## 6. Pembahasan & kesimpulan

<!-- Bandingkan hasil dengan hipotesis di Bagian 3. Hipotesis yang ternyata salah tetap bernilai,
     asalkan dijelaskan kenapa. Perbedaan kecil (lebih kecil dari simpangan baku) belum tentu berarti. -->

Berdasarkan hasil eksperimen, seluruh skenario augmentasi menghasilkan nilai Dice Score dan IoU rata-rata yang lebih tinggi dibandingkan skenario tanpa augmentasi. Skenario horizontal flip memperoleh Dice Score tertinggi sebesar 0,605 ± 0,052 dan IoU sebesar 0,502 ± 0,045. Hasil ini menunjukkan bahwa augmentasi horizontal flip memberikan performa rata-rata terbaik dalam eksperimen yang dilakukan.

Skenario vertical flip menghasilkan Dice Score sebesar 0,604 ± 0,011 dan IoU sebesar 0,480 ± 0,013. Nilai Dice Score tersebut hampir sama dengan skenario horizontal flip, tetapi memiliki simpangan baku yang lebih kecil. Hal ini menunjukkan bahwa hasil pengulangan pada skenario vertical flip lebih konsisten. Sementara itu, augmentasi kecerahan dan kontras menghasilkan Dice Score sebesar 0,579 ± 0,031 dan IoU sebesar 0,466 ± 0,033. Meskipun hasilnya lebih baik dibandingkan skenario tanpa augmentasi, peningkatannya masih berada di bawah kedua skenario flip.

Perbedaan hasil tersebut menunjukkan bahwa jenis augmentasi memberikan pengaruh yang berbeda terhadap performa model U-Net dalam melakukan segmentasi polip. Namun, selisih Dice Score antara horizontal flip dan vertical flip hanya sebesar 0,001. Oleh karena itu, nilai rata-rata yang lebih tinggi pada horizontal flip belum cukup untuk menyatakan bahwa skenario tersebut secara nyata lebih unggul. Hasil ini perlu dipahami berdasarkan nilai rata-rata dan variasi dari setiap pengulangan.

Secara keseluruhan, augmentasi dapat membantu meningkatkan performa model U-Net pada eksperimen segmentasi polip yang dilakukan. Skenario horizontal flip memperoleh nilai rata-rata Dice Score dan IoU tertinggi, sedangkan vertical flip menunjukkan variasi hasil Dice Score yang lebih kecil. Dengan demikian, pemilihan augmentasi perlu mempertimbangkan tidak hanya nilai performa rata-rata, tetapi juga konsistensi hasil pengujian.

## 7. Pembagian kerja

<!-- Siapa mengerjakan apa. Penilaian proses kerja dilihat dari riwayat commit tiap anggota. -->

✍️

## 8. Penggunaan AI

<!-- Wajib diisi, walau tidak memakai AI (tulis "Tidak memakai AI").
     Contoh: "Claude Code (CLI) untuk menulis skenario augmentasi dan memperbaiki error di Bagian 4.
     Draf Bagian 6 ditulis sendiri, lalu dirapikan bahasanya dengan bantuan AI." -->

✍️

## Cara menjalankan ulang

```bash
uv sync
uv run jupyter lab segmentasi_polip.ipynb
```

Atau buka notebook di Google Colab (Runtime → Change runtime type → T4 GPU).

## Referensi

- Jha, D., dkk. (2020). Kvasir-SEG: A Segmented Polyp Dataset. *MultiMedia Modeling (MMM 2020)*, 451–462.
- Ronneberger, O., Fischer, P., & Brox, T. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. *MICCAI*. arXiv:1505.04597.
- ✍️ tambahkan referensi lain yang kalian pakai
