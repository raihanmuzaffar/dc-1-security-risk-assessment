# LAPORAN PRAKTIKUM
## Analisis dan Penilaian Risiko pada Mesin Virtual DC-1

**Nama:** ........................................  
**NIM:** .........................................  
**Mata Kuliah:** ................................  
**Kelas:** .......................................

---

## 1. Tujuan

Praktikum ini bertujuan mengidentifikasi aset dan layanan yang terpapar pada mesin virtual DC-1, menilai risiko keamanan berdasarkan bukti pemeriksaan, serta menyusun rekomendasi mitigasi yang sesuai.

## 2. Ruang Lingkup

Pengujian dilakukan dari Kali Linux terhadap mesin virtual DC-1 pada jaringan Host-only VirtualBox.

| Komponen | Informasi |
|---|---|
| Mesin penguji | Kali Linux |
| IP Kali pada jaringan lab | 192.168.52.10 |
| Target | DC-1 |
| IP target | 192.168.52.11 |
| Jaringan pengujian | 192.168.52.0/24 |
| Metode | Pemindaian layanan, pemeriksaan HTTP, robots.txt, dan RPC |

Pengujian dibatasi pada lingkungan virtual laboratorium.

![Diagram topologi laboratorium](system-scope.png)

## 3. Metodologi

Tahapan yang dilakukan:
1. Mengidentifikasi target melalui pemindaian jaringan lokal.
2. Memindai port TCP dan mengidentifikasi versi layanan.
3. Memeriksa header HTTP dan halaman web.
4. Memeriksa robots.txt.
5. Mengidentifikasi program RPC.
6. Menyusun daftar aset, register risiko, matriks risiko, dan rekomendasi mitigasi.

## 4. Hasil Pemeriksaan

Pemindaian menemukan tiga port TCP terbuka:

| Port | Layanan | Hasil identifikasi |
|---|---|---|
| 22/tcp | SSH | OpenSSH 6.0p1 Debian |
| 80/tcp | HTTP | Apache httpd 2.2.22 (Debian) |
| 111/tcp | RPC | rpcbind |

Header HTTP juga mengungkap PHP 5.4.45 dan Drupal 7. Pemeriksaan RPC menemukan program rpcbind dan status.

Halaman web merespons HTTP 200 OK. File robots.txt mencantumkan sejumlah jalur standar Drupal. Keberadaan jalur dalam robots.txt tidak membuktikan bahwa jalur tersebut dapat diakses tanpa autentikasi.

Bukti pendukung tersedia dalam:
- `dc1-services.txt`
- `dc1-http-headers.txt`
- `dc1-webpage.html`
- `dc1-robots.txt`
- `dc1-rpcinfo.txt`
- `dc1-findings-summary.txt`

## 5. Penilaian Risiko

Skor risiko dihitung menggunakan rumus:

**Skor Risiko = Likelihood × Impact**

Keduanya menggunakan skala 1–5.

| Skor | Kategori |
|---|---|
| 1–4 | Rendah |
| 5–9 | Sedang |
| 10–16 | Tinggi |
| 17–25 | Sangat Tinggi |

Hasil penilaian awal:

| ID | Risiko | Likelihood | Impact | Skor | Kategori |
|---|---|---:|---:|---:|---|
| R01 | Komponen web usang | 4 | 4 | 16 | Tinggi |
| R02 | Potensi akses SSH tanpa otorisasi | 3 | 5 | 15 | Tinggi |
| R03 | Paparan layanan RPC | 3 | 3 | 9 | Sedang |
| R04 | Pengungkapan versi teknologi web | 3 | 2 | 6 | Sedang |
| R05 | Gangguan ketersediaan layanan | 2 | 4 | 8 | Sedang |
| R06 | Penyalahgunaan halaman akun atau administrasi | 2 | 4 | 8 | Sedang |

Nilai likelihood dan impact merupakan estimasi awal untuk pembelajaran. Nilai tersebut bukan hasil pengukuran statistik atau bukti bahwa serangan telah berhasil.

Register risiko lengkap terdapat dalam `risk-register.csv`. Matriks terdapat dalam `risk-matrix.txt`.

## 6. Perhitungan Kuantitatif

Sebagai contoh metode, risiko gangguan layanan dianalisis menggunakan asumsi berikut:
- Durasi gangguan: 3 jam per kejadian.
- Estimasi kerugian: Rp100.000 per jam.
- Frekuensi: 2 kejadian per tahun.

Kerugian per kejadian:

3 × Rp100.000 = Rp300.000.

Annual Loss Expectancy (ALE):

Rp300.000 × 2 = Rp600.000 per tahun.

Jika biaya mitigasi diasumsikan Rp400.000 per tahun, selisih sederhana antara estimasi kerugian tahunan dan biaya mitigasi adalah Rp200.000.

Seluruh angka tersebut bersifat ilustratif, bukan data kerugian aktual DC-1. Perhitungan ini belum memperhitungkan seluruh manfaat, efektivitas, dan biaya kontrol.

Rincian tersedia dalam `quantitative-risk.txt`.

## 7. Rekomendasi Mitigasi

| Prioritas | Rekomendasi | Penanggung jawab |
|---|---|---|
| 1 | Tingkatkan komponen web dan sistem operasi ke versi yang masih didukung | Administrator sistem |
| 2 | Batasi akses SSH dan terapkan autentikasi kuat | Administrator sistem |
| 3 | Nonaktifkan RPC apabila tidak diperlukan atau batasi akses jaringan | Administrator jaringan |
| 4 | Tinjau pengungkapan informasi versi dan konfigurasi aplikasi | Administrator web |
| 5 | Audit autentikasi, otorisasi, dan akses halaman aplikasi | Administrator aplikasi |
| 6 | Terapkan monitoring, backup, dan uji pemulihan | Administrator sistem |

Perubahan konfigurasi perlu diuji terlebih dahulu agar tidak mengganggu layanan yang dibutuhkan.

## 8. Keterbatasan

Penilaian ini didasarkan pada pemeriksaan jaringan dan respons layanan yang tersedia. Konfigurasi internal, status patch lengkap, kekuatan autentikasi, keberhasilan eksploitasi, serta dampak dan biaya gangguan aktual belum diverifikasi.

Karena itu, temuan paparan layanan harus dibedakan dari kerentanan yang sudah dikonfirmasi dan eksploitasi yang berhasil.

## 9. Kesimpulan

DC-1 memperlihatkan layanan SSH, HTTP, dan RPC yang dapat dijangkau dari jaringan laboratorium. Identifikasi layanan web juga menunjukkan komponen dengan versi lama yang perlu dievaluasi dan ditingkatkan.

Berdasarkan estimasi awal, risiko komponen web dan akses SSH menjadi prioritas penanganan. Rekomendasi utama meliputi pembaruan komponen, pembatasan akses jaringan, audit konfigurasi, dan peningkatan pemantauan.

Pengujian lanjutan serta validasi pemilik sistem diperlukan sebelum menarik kesimpulan operasional.

---

## Lampiran Bukti

Bukti pemeriksaan dan dokumen pendukung disimpan di direktori `~/Labs/DC-1/evidence/`.
