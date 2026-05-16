# 📄 Human-in-the-Loop Leave Request & Approval Automation using n8n

Sistem otomasi alur kerja (Workflow Automation) komprehensif untuk menyederhanakan birokrasi pengajuan cuti karyawan. Proyek ini mengintegrasikan **n8n** sebagai orchestrator utama, **Google Sheets** sebagai database terpusat, dan **Gmail API** sebagai antarmuka interaktif, dengan mengusung konsep desain *Human-in-the-Loop* (sistem menangani seluruh administrasi, manusia memegang keputusan final).

Proyek ini disusun dan diajukan sebagai portofolio utama dalam Uji Kompetensi Solusi Artificial Intelligence & Otomasi berbasis **SKKNI**.

## 🚀 Fitur Utama

- **Real-Time Submission Handling:** Pengumpulan data pendaftaran langsung melalui n8n Form Trigger.
- **Automated Date & Balance Validation:** Memanfaatkan logika JavaScript dengan library `Luxon` untuk menghitung durasi hari secara inklusif serta melakukan validasi sisa kuota saldo cuti secara real-time.
- **Smart Auto-Rejection (Jalur FALSE Workflow 1):** Sistem secara otomatis memotong jalur pengajuan dan mengirimkan email penolakan premium ke karyawan jika kuota saldo tidak mencukupi, mencegah beban kerja berlebih pada Manager.
- **Instant Webhook Approval Handler:** Mengirimkan email berbasis struktur HTML premium ke Manager yang dilengkapi tombol keputusan instan (*One-click approval*) memanfaatkan arsitektur dinamis HTTP GET Query Parameters.
- **Responsive Web Feedback UI:** Menampilkan halaman respon balik berbasis HTML dinamis pada browser Manager sesaat setelah tombol keputusan diklik untuk memastikan transparansi status data integritas.
- **Centralized Audit Trail:** Sinkronisasi otomatis ke Google Sheets untuk mencatat log aktivitas pengajuan secara terstruktur demi kebutuhan audit tim HRD.

## 📊 Arsitektur Sistem & Aliran Data

1. **Formulir Pengajuan Cuti (Trigger)** -> Data karyawan ditangkap dalam format JSON object.
2. **Read Database (Filter UI)** -> Sistem mencari baris karyawan di Sheets berdasarkan kecocokan email.
3. **Logic Processor (Code Node)** -> Perhitungan durasi hari dan perbandingan saldo kuota sisa cuti.
4. **Branching (Node IF):**
   - **Jalur TRUE:** Menyimpan log berstatus *Pending*, mengirim email interaktif ke Manager.
   - **Jalur FALSE:** Memicu *Auto-Rejection*, mengirim email penolakan otomatis ke Karyawan.
5. **Approval Action (Webhook):**
   - **Status Approved:** Update saldo `DB_Karyawan`, ubah status log menjadi *Approved*, tampilkan UI sukses web, kirim email penerimaan ke karyawan.
   - **Status Rejected:** Ubah status log menjadi *Rejected* tanpa memotong saldo kuota, tampilkan UI web penolakan, kirim email pemberitahuan ke karyawan.

## 🛠️ Tech Stack yang Digunakan

- **n8n Cloud / Self-Hosted:** Selaku Orchestrator Alur Kerja otomatis.
- **Google Sheets API:** Selaku basis data transaksional (Karyawan & Log Pengajuan).
- **Gmail API (OAuth2 Authentication):** Selaku infrastruktur pengiriman notifikasi surat elektronik.
- **JavaScript & Luxon Library:** Selaku mesin pengolah logika bisnis dan validasi variabel waktu ISO format.
- **HTML5 & CSS Inline Styling:** Selaku penyusun antarmuka email premium dan halaman umpan balik Webhook.

## 📁 Struktur File Proyek

- `Workflow_Pengajuan.json`: Blueprint konfigurasi JSON untuk Workflow 1 (Formulir, Validasi Saldo, & Notifikasi Manager).
- `Approval_Handler.json`: Blueprint konfigurasi JSON untuk Workflow 2 (Penangkap Webhook, Sinkronisasi Balik Database, & Notifikasi Akhir Karyawan).
- `DB_Cuti_Karyawan.xlsx`: File data sampel lembar kerja yang berisi struktur tabel `DB_Karyawan` dan `Log Pengajuan` sebagai acuan skema integrasi.

## 🔮 Rencana Pengembangan (Future Roadmap)

- **Conflict Schedule Checker:** Pengembangan fungsi kueri silang antar divisi untuk mendeteksi adanya bentrok jadwal cuti massal di hari yang sama guna menghindari kelangkaan personel operasional (*short-staffed*).
- **Dynamic Rejection Reason:** Penambahan kolom input teks dinamis bagi Manager saat mengeklik tombol tolak agar karyawan menerima alasan spesifik penolakan langsung di dalam email sistem.

---
**Developed by Muhammad Syahril Rizky Fauzan - STT Terpadu Nurul Fikri - 2026**
