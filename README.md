<div align="center">

  <img src="assets/img/logo.png" alt="PT Metindo Erasakti Logo" width="120" style="margin-bottom: 10px;" />

  # Management Tooling & RFID Integration System
  ### PT Metindo Erasakti

  <p align="center">
    Inventory Aset Stamping Dies
  </p>

  <p align="center">
    <a href="#-fitur-utama"><img src="https://img.shields.io/badge/Status-Production%20Ready-success?style=for-the-badge&logo=statuspage" alt="Status"></a>
    <a href="#-tech-stack"><img src="https://img.shields.io/badge/PHP-8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white" alt="PHP"></a>
    <a href="#-tech-stack"><img src="https://img.shields.io/badge/Database-MariaDB%2010.4+-003545?style=for-the-badge&logo=mariadb&logoColor=white" alt="MariaDB"></a>
    <a href="#-arsitektur--alur-kerja-workflow"><img src="https://img.shields.io/badge/Architecture-Modular%20MVC--S-blue?style=for-the-badge" alt="Architecture"></a>
    <a href="#-keamanan--audit-trail"><img src="https://img.shields.io/badge/Security-OWASP%20Hardened-red?style=for-the-badge&logo=securityscorecard&logoColor=white" alt="Security"></a>
    <a href="#-integrasi-hardware-uhf-rfid"><img src="https://img.shields.io/badge/IoT-UHF%20RFID%20EPC%20Gen2-orange?style=for-the-badge&logo=espressif" alt="IoT RFID"></a>
  </p>

</div>

---

## 📑 Daftar Isi

- [Tentang Proyek](#-tentang-proyek)
- [Tech Stack](#-tech-stack)
- [Fitur Utama](#-fitur-utama)
- [Arsitektur & Alur Kerja (Workflow)](#-arsitektur--alur-kerja-workflow)
  - [Struktur Direktori Proyek](#struktur-direktori-proyek)
  - [Panduan Navigasi Pindah Tangan (Handover Cheat Sheet)](#panduan-navigasi-pindah-tangan-handover-cheat-sheet)
  - [Alur Siklus Request (Request Lifecycle)](#alur-siklus-request-request-lifecycle)
  - [SOP Penambahan Modul Baru](#sop-penambahan-modul-baru)
- [Instalasi & Pengaturan Lokal](#-instalasi--pengaturan-lokal)
  - [Prasyarat Sistem](#prasyarat-sistem)
  - [Langkah Instalasi](#langkah-instalasi)
  - [Konfigurasi Environment (.env)](#konfigurasi-environment-env)
- [Hak Akses Pengguna (RBAC Matrix)](#-hak-akses-pengguna-rbac-matrix)
- [API Reference (Hardware RFID & Mobile)](#-api-reference-hardware-rfid--mobile)
- [Panduan Operasional & SOP Tambahan](#-panduan-operasional--sop-tambahan)
- [Kontributor & Lisensi](#-kontributor--lisensi)

---

## 💡 Tentang Proyek

Sistem **Management Tooling** dirancang khusus untuk mendigitalkan siklus hidup seluruh aset *stamping dies* produksi di **PT Metindo Erasakti**. Sistem ini meniadakan pencatatan kertas manual, mencegah salah lokasi penempatan rak, mendeteksi perpindahan dies secara otomatis menggunakan tiang sensor gerbang RFID .

---

## 🛠️ Tech Stack

| Kategori | Teknologi | Deskripsi |
| :--- | :--- | :--- |
| **Backend** | ![PHP](https://img.shields.io/badge/PHP-8.2-777BB4?style=flat-square&logo=php&logoColor=white) | PHP Native Full-Stack dengan pendekatan OOP & Strict Types |
| **Database** | ![MariaDB](https://img.shields.io/badge/MariaDB-10.4+-003545?style=flat-square&logo=mariadb&logoColor=white) | Relational Database dengan ACID Transaction & Prepared Statements |
| **Server** | ![Apache](https://img.shields.io/badge/Apache-2.4-D22128?style=flat-square&logo=apache&logoColor=white) | Web Server dengan `.htaccess` URL Rewrite Engine |
| **Frontend** | ![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black) ![CSS3](https://img.shields.io/badge/CSS-Custom%20Design%20System-1572B6?style=flat-square&logo=css3) | Vanilla JS, Select2, SweetAlert2, HTML5 Camera Scanner |
| **Paket Pustaka** | ![Composer](https://img.shields.io/badge/Composer-2.x-885630?style=flat-square&logo=composer&logoColor=white) | `vlucas/phpdotenv`, `endroid/qr-code`, `phpoffice/phpspreadsheet`, `smalot/pdfparser` |
| **Hardware IoT** | UHF RFID EPC Gen 2 | Long-Range Fixed Gate Reader (TCP/IP & Wi-Fi), Anti-Metal Tags |

---

## 🚀 Fitur Utama

- 📊 **Live Executive Dashboard**: Pemantauan inventaris dies mobil (R4) & motor (R2), analitik kapasitas gudang/plant, dan matriks kesehatan dies.
- 📦 **Master Dies & Part Management**: Database spesifikasi komprehensif (Dimensi, Berat, Tonase, No. Part/Assy), galeri visual terkompresi otomatis (WebP/JPEG), dan batch import Excel.
- 🚚 **Movement Tracking**: Pelacakan perpindahan *Inhouse* (antar-plant) dan *Outhouse* (vendor reparasi eksternal) dengan verifikasi foto surat jalan dan auto-healing data gantung.
- 🗺️ **Visual Layout Basement 2D**: Peta persegi visual seluruh slot rak basement (Hall H2, Hall H3, Zona A, Zona D, Zona E) dengan warna indikator kapasitas dan modal detail dies.
- 📡 **Integrasi Otomatis UHF RFID**:
  - Deteksi tanpa sentuh saat forklift melintas gerbang sensor.
  - Aturan bisnis **Skid Tooling Family** (1 tag fisik pada skid mentrigger seluruh sub-proses dies part secara bersamaan).
  - *Anti-hovering cooldown* (45 detik) untuk mencegah duplikasi event scan.
  - Uji aktif koneksi IP perangkat langsung via **TCP Socket Ping**.
- 🔒 **Keamanan Terpadu (OWASP Hardened)**: Proteksi multi-login sesi tunggal, validasi CSRF token pada seluruh form mutasi data, proteksi SQL Injection mutlak, dan logging fail-safe terpusat via `log_system_error()`.

---

## 🏛️ Arsitektur & Alur Kerja (Workflow)

Sistem menggunakan arsitektur **Modular MVC-S (Model - View - Controller - Service Layer)**. Controller dijaga tetap ramping (*Slim Controller*), sedangkan seluruh proses bisnis, validasi, dan transaksi database diisolasi di *Service Layer*.

### Struktur Direktori Proyek

```text
Management Tooling/
├── api/                             # [REST API CONTROLLERS — Ingestion IoT & Mobile]
│   ├── dashboard_api.php            # Endpoint analitik KPI & tren pergerakan
│   ├── movement_log_api.php         # Provider log riwayat pergerakan dies
│   ├── upload_image.php             # Upload foto dies/part (CSRF-protected)
│   ├── qr.php                       # Generator QR Code dinamis (non-blocking)
│   └── rfid/                        # REST API Hardware RFID (Stateless, Auth X-API-Key)
│       ├── scan.php                 # Ingestion event scan tag dari gate/forklift
│       ├── heartbeat.php            # Healthcheck status reader (tiap 30 detik)
│       ├── gate_push.php            # Direct webhook push untuk controller industri
│       └── tag_check.php            # Lookup dies untuk handheld scanner
│
├── services/                        # [SERVICE LAYER — Logika Bisnis & Query DB]
│   ├── DiesService.php              # Query dies, cascade delete aman, PM window
│   ├── LocationService.php          # Master lokasi, Poka-Yoke duplikasi, layout basement 2D
│   ├── RfidService.php              # Kelola reader, tag, TCP socket ping, Skid family grouping
│   ├── MovementService.php          # State machine pergerakan, smart scan, & auto-healing
│   └── DashboardService.php         # Kalkulasi metrik analitik & tren pergerakan
│
├── modules/                         # [SLIM CONTROLLERS — Validasi Sesi, RBAC & CSRF]
│   ├── dies/                        # Controller database dies (index, action, movement)
│   ├── locations/                   # Controller master lokasi (index, form, basement_layout, print_qr)
│   ├── rfid/                        # Controller hardware RFID (readers, tags, reader_action, tag_action)
│   ├── maintenance/                 # Controller perawatan dies (schedule, logs, view)
│   ├── parts/                       # Controller master part number & relasi assy
│   ├── reports/                     # Controller pelaporan & export CSV
│   └── audit/                       # Controller audit trail
│
├── views/                           # [PRESENTATION LAYER — Template UI Murni HTML/CSS/JS]
│   ├── dies/                        # View tabel dies, modal formulir, & detail modal
│   ├── locations/                   # View tabel lokasi, form tambah/edit, & peta 2D basement
│   ├── rfid/                        # View monitoring reader, status badge, & assign tag
│   ├── movement/                    # View pergerakan, HUD smart detect, & modal scan
│   ├── dashboard/                   # View analitik dashboard & komponen KPI
│   └── layouts/                     # Template master header & footer
│
├── assets/                          # [STATIC ASSETS — CSS, JavaScript, Foto Dies, Icons]
├── includes/                        # [CORE BOOTSTRAP, SECURITY & HELPERS]
│   ├── config.php                   # Web bootstrap: Sesi, RBAC, CSRF, .env, log_system_error()
│   ├── db.php                       # Hardware bootstrap: Koneksi MySQL stateless
│   ├── auth.php                     # Role matrix & otentikasi login
│   ├── audit.php                    # Logging audit aktivitas pengguna
│   ├── dies_image_helper.php        # Helper kompresi gambar & sanitasi path
│   └── rfid_helper.php              # Wrapper helper kompatibilitas RFID
│
├── database/                        # Skema DDL dan migrasi database SQL
├── logs/                            # Centralized log: app_error.log (Pencatatan error developer)
└── .htaccess                        # Clean URL Engine (/rfid/tags, /locations, /movement)
```

---

### Panduan Navigasi

Jika tim pengembang ingin memperbarui atau menelusuri kode sistem, gunakan panduan rujukan berikut:

| Kebutuhan Pekerjaan | Folder yang Dibuka | Contoh Berkas |
| :--- | :--- | :--- |
| **Ubah Tampilan / UI** *(HTML, CSS, Modal, Badge, Tabel)* | `views/<modul>/` | `views/rfid/readers.view.php`<br>`views/locations/index.view.php`<br>`views/dies/index.view.php` |
| **Ubah Query SQL & Logika Bisnis** *(Rumus, kalkulasi, validasi Poka-Yoke, filter)* | `services/` | `services/RfidService.php`<br>`services/LocationService.php`<br>`services/DiesService.php` |
| **Atur Alur Halaman & Hak Akses** *(Periksa RBAC, token CSRF, alur POST/GET)* | `modules/<modul>/` | `modules/rfid/readers.php`<br>`modules/locations/form.php`<br>`modules/dies/index.php` |
| **Integrasi Mesin / REST API** *(Endpoint JSON IoT, handheld, RFID gate)* | `api/` | `api/rfid/scan.php`<br>`api/rfid/gate_push.php`<br>`api/upload_image.php` |
| **Konfigurasi Global & Keamanan** *(Koneksi database, session, error logger)* | `includes/` | `includes/config.php`<br>`includes/auth.php`<br>`includes/sidebar.php` |
| **Aturan Alamat URL Bersih** *(Rewrite URL tanpa ekstensi .php)* | `.htaccess` | File `.htaccess` pada root folder proyek |

---

### Alur Siklus Request (Request Lifecycle)

```
[1. BROWSER / CLIENT]
      │  User melakukan aksi (Klik Menu, Simpan Data, Hapus Data)
      │  (Mengirim HTTP Request: GET / POST + CSRF Token)
      ▼
[2. ROUTER (.htaccess)]
      │  Memetakan URL bersih ke Controller fisik yang sesuai
      ▼
[3. CONTROLLER (modules/<modul>/)]
      │  - Validasi Sesi Login & Izin RBAC (rbac_require_module)
      │  - Verifikasi Keamanan CSRF (verify_csrf_token)
      │  - Meneruskan data masukan ke Service Layer
      ▼
[4. SERVICE LAYER (services/<Modul>Service.php)]
      │  - Otak Logika Bisnis (Poka-Yoke, Validasi, Transaksi ACID)
      │  - Eksekusi Prepared Statement SQL ke Database
      │  - Fail-safe error logging via log_system_error()
      ▼
[5. DATABASE (MariaDB)]
      │  COMMIT / ROLLBACK & Mengembalikan Recordset murni
      ▼
[6. CONTROLLER]
      │  Menyertakan template View (require views/<modul>/<nama>.view.php)
      ▼
[7. VIEW (views/<modul>/)]
      │  Merender data array ke dalam HTML/UI murni
      ▼
[8. BROWSER]
      Hasil tampilan interaktif ditampilkan ke pengguna
```

---

### SOP Penambahan Modul Baru

1. **Service (`services/<Nama>Service.php`)**: Buat class logika bisnis dan database query menggunakan prepared statements.
2. **Controller (`modules/<nama>/index.php`)**: Buat controller ramping (30–40 baris). Validasi otentikasi login, modul RBAC, panggil Service, lalu muat View.
3. **View (`views/<nama>/index.view.php`)**: Buat template UI murni. Ambil variabel yang telah disiapkan Controller. **Dilarang menulis query SQL di dalam View.**
4. **Routing**: Daftarkan URL bersih modul di file [`.htaccess`](file:///c:/xampp/htdocs/Management%20Tooling/.htaccess) dan tambahkan tautan menu di [`includes/sidebar.php`](file:///c:/xampp/htdocs/Management%20Tooling/includes/sidebar.php).

---

## ⚡ Instalasi & Pengaturan Lokal

### Prasyarat Sistem
- **Web Server**: Apache 2.4+ (Disarankan paket XAMPP)
- **PHP**: Versi 8.1 atau 8.2+ (Wajib ekstensi `mysqli`, `pdo_mysql`, `gd`, `zip`, `fileinfo`)
- **Basis Data**: MariaDB 10.4+ / MySQL 8.0+
- **Manajer Paket**: Composer 2.x

### Langkah Instalasi

```bash
# 1. Posisikan repositori tepat pada direktori htdocs web server
cd C:\xampp\htdocs\
git clone <URL_REPOSITORY> "Management Tooling"
cd "Management Tooling"

# 2. Pasang pustaka dependensi via Composer
composer install

# 3. Buat berkas environment dari template
copy .env.example .env
```

### Konfigurasi Environment (`.env`)

Sesuaikan kredensial pada berkas `.env`:

```ini
APP_ENV=development

# Konfigurasi Database
DB_HOST=localhost
DB_USER=root
DB_PASS=
DB_NAME=dbstampingdies

# Kunci Autentikasi Hardware RFID (X-API-Key)
RFID_API_KEY=dev

# Autentikasi Tambahan Controller Direct Push
RFID_GATE_USER=admin_gate
RFID_GATE_PASS=gate_secret_pass
```

### Setup Database

1. Buat database baru di phpMyAdmin / MySQL CLI dengan nama `dbstampingdies`.
2. Import skema utama Anda.
3. Jalankan skrip pelengkap DDL RFID jika diperlukan:
   ```bash
   mysql -u root dbstampingdies < database/schema_rfid_full.sql
   ```
4. Buka peramban web dan akses alamat:
   ```text
   http://localhost/Management%20Tooling/
   ```

---

## 🛡️ Hak Akses Pengguna (RBAC Matrix)

Sistem menerapkan **Role-Based Access Control (RBAC)** berjenjang dengan 5 tingkatan peran:

| Modul Sistem | Superadmin | Section Head | Foreman | Dept Head | Operator |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Executive Dashboard** | ✅ Full | ✅ Full | ✅ View | ✅ View | ✅ View |
| **Dies Master Database** | ✅ Full | ✅ Full | ✅ Edit | ✅ View | ❌ No |
| **Movement Tracking (Scan)**| ✅ Full | ✅ Full | ✅ Move | ✅ View | ✅ Move |
| **Parts Master** | ✅ Full | ✅ Full | ✅ Edit | ✅ View | ❌ No |
| **Locations Master** | ✅ Full | ✅ Full | ✅ Edit | ✅ View | ❌ No |
| **Technicians** | ✅ Full | ✅ Full | ✅ Edit | ✅ View | ❌ No |
| **PM Schedule & Planning** | ✅ Full | ✅ Full | ✅ Manage | ✅ View | ❌ No |
| **PM Logs & Service Entry** | ✅ Full | ✅ Full | ✅ Run | ❌ No | ❌ No |
| **RFID Readers & Tags** | ✅ Full | ✅ Full | ❌ No | ❌ No | ❌ No |
| **Audit Log System** | ✅ Full | ✅ View | ❌ No | ❌ No | ❌ No |

---

## 📡 API Reference (Hardware RFID & Mobile)

Semua endpoint hardware IoT bersifat **Stateless** dan diverifikasi menggunakan header HTTP `X-API-Key`.

#### 1. Heartbeat Reader (Kirim tiap 30 detik)
```http
POST /api/rfid/heartbeat.php
Header: X-API-Key: <RFID_API_KEY>
Content-Type: application/json

{
  "reader_code": "GATE-BASEMENT-01"
}
```
**Response (200 OK):**
```json
{
  "success": true,
  "status": "online"
}
```

#### 2. Event Scan Tag (Otomatisasi Gerbang)
```http
POST /api/rfid/scan.php
Header: X-API-Key: <RFID_API_KEY>
Content-Type: application/json

{
  "reader_code": "GATE-BASEMENT-01",
  "epc_code": "E28086813C3FB89DFED3CBC6",
  "rssi": -58.2,
  "antenna": 1
}
```
**Response (200 OK):**
```json
{
  "success": true,
  "message": "Movement recorded",
  "assetcode": "718722",
  "status_transisi": "Ready -> Ongoing",
  "skid_family_count": 3
}
```

#### 3. Lookup Tag Handheld Scanner
```http
GET /api/rfid/tag_check.php?epc=E28086813C3FB89DFED3CBC6
Header: X-API-Key: <RFID_API_KEY>
```

---

## 📖 Panduan Operasional & SOP Tambahan

<details>
<summary><b>🛠️ SOP Pindah Perangkat & Server Pengganti (Klik untuk Membuka)</b></summary>

Saat memindahkan sistem ke server baru atau laptop tim:
1. **Aturan Kesamaan Path**: Letakkan selalu di `C:\xampp\htdocs\Management Tooling\`.
2. **Aturan Database**: Selalu lakukan dump database utuh dengan menyertakan *views*, *triggers*, dan *routines*:
   ```bash
   mysqldump -u root --routines --triggers dbstampingdies > backup_lengkap.sql
   ```
3. **Aturan Berkas `.env`**: Salin file `.env` fisik secara terpisah (karena `.env` diabaikan oleh Git untuk keamanan kredensial).
4. **Aturan Dependensi**: Jalankan perintah `composer install` pada folder proyek di komputer baru.
5. **Aturan IP Statis & Firewall Jaringan Pabrik**:
   - Jika laptop berfungsi sebagai server penerima RFID atau diakses dari HP teknisi, berikan **IP Statis** (misal: `192.168.1.150`).
   - Buka izin port **Apache (Port 80 & 443)** di **Windows Defender Firewall** laptop baru:
     *Masuk ke Control Panel &rarr; Windows Defender Firewall &rarr; Advanced Settings &rarr; Inbound Rules &rarr; New Rule &rarr; Port &rarr; TCP &rarr; 80, 443 &rarr; Allow the connection.*
6. **Aturan Sesi Tunggal (Single Session)**: Klik tombol **Logout** pada laptop lama sebelum membuka di perangkat baru, atau kosongkan nilai `session_id` pada tabel `users` database.

</details>

<details>
<summary><b>📡 SOP Instalasi Fisik & Tuning Hardware RFID (Klik untuk Membuka)</b></summary>

1. **Posisi Tiang**: Pasang di sisi kanan dan kiri gerbang jalur keluar-masuk forklift (lebar gerbang ideal: 3 - 5 meter).
2. **Ketinggian Antena**: Posisikan antena pada ketinggian **1.8 meter s/d 2.2 meter** dari lantai.
3. **Kemiringan (Tilt)**: Miringkan antena menunduk ke bawah sebesar **15° s/d 20°** mengarah ke bagian tengah muatan forklift untuk memaksimalkan pantulan gelombang.
4. **Proteksi Fisik**: Pasang tiang bollard pengaman pipa besi cor di depan tiang reader agar aman dari benturan forklift.
5. **Penempelan Tag**: Wajib menggunakan **UHF Anti-Metal Tag (On-Metal Tag)** berbahan casing PCB / ABS industri. Rekatkan pada sisi samping dies yang bebas dari alur *clamping slot* atau area cetakan (*molding face*).

</details>

<details>
<summary><b>🔍 Matriks Troubleshooting Cepat Kendala Lapangan (Klik untuk Membuka)</b></summary>

| Gejala Masalah | Kemungkinan Penyebab | Tindakan Solusi |
| :--- | :--- | :--- |
| **Status Reader di Web "OFFLINE"** | Kabel adaptor lepas atau Wi-Fi/LAN terputus > 90 detik. | Periksa adaptor daya 12V reader dan tes koneksi jaringan menggunakan tombol **Test Ping IP** pada menu RFID Readers. |
| **Dies lewat tapi tidak terdeteksi** | Tag rusak, jarak terlalu jauh, atau tag bukan jenis anti-metal. | 1. Uji tag menggunakan scanner handheld.<br>2. Naikkan nilai transmit power RF pada menu vendor menjadi 26–30 dBm.<br>3. Pastikan tag berstatus `ACTIVE` di menu web. |
| **Event Scan Terduplikasi (Spam)** | Forklift berhenti terlalu lama di bawah gerbang. | Sistem secara otomatis menerapkan *anti-hovering cooldown* 45 detik. Pastikan setting *Filter Duplicate Time* di controller reader diaktifkan. |
| **Error 401 / 403 saat Push Data** | Header `X-API-Key` salah atau tidak cocok dengan `.env`. | Periksa konfigurasi `RFID_API_KEY` pada file `.env` di server dan samakan dengan header request yang dikirimkan alat reader. |
| **Aplikasi Web Lambat saat Load Gambar / Scan** | File lock sesi PHP mengunci antrean paralel. | Pastikan fungsi `session_write_close()` aktif pada endpoint read-only sesuai standar. |

</details>

---

## 👥 Kontributor & Lisensi

- **Pengembang & Arsitektur Sistem**: Tim Advanced Engineering & Tooling PT Metindo Erasakti
- **Lisensi**: Proprietary & Confidential — Hak Cipta Dilindungi Undang-Undang. Khusus digunakan untuk operasional internal **PT Metindo Erasakti**.
