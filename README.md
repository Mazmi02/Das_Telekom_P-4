<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Tier I (Data Center)?", "id": "Tier Dasar (Tanpa Redundansi)." },
  { "en": "Apa Itu Tier II (Data Center)?", "id": "Tier (Redundansi Parsial)." },
  { "en": "Apa Itu Tier III (Data Center)?", "id": "Tier (Dapat Dirawat Tanpa Padam)." },
  { "en": "Apa Itu Tier IV (Data Center)?", "id": "Tier Tertinggi (Fault Tolerant)." },
  { "en": "Apa Itu Storage Area Network (SAN)?", "id": "Jaringan Khusus Penyimpanan (Blok)." },
  { "en": "Apa Itu Network-Attached Storage (NAS)?", "id": "Penyimpanan Terhubung Jaringan (File)." },
  { "en": "Apa Itu iSCSI (Internet Small Computer System Interface)?", "id": "Protokol SAN (Storage Area Network) IP." },
  { "en": "Apa Itu FCoE (Fibre Channel over Ethernet)?", "id": "Fibre Channel (FC) Lewat Ethernet." },
  { "en": "Apa Itu World Wide Name (WWN)?", "id": "Alamat Unik Perangkat Fibre Channel." },
  { "en": "Apa Itu Zoning (SAN)?", "id": "Membatasi Komunikasi Antar Perangkat SAN." },
  { "en": "Apa Itu Logical Unit Number (LUN)?", "id": "Nomor Identifikasi Unit Penyimpanan (Blok)." },
  { "en": "Apa Itu LUN (Logical Unit Number) Masking?", "id": "Membatasi Akses Host Ke LUN." },
  { "en": "Apa Itu Multipathing?", "id": "Memiliki Beberapa Jalur Ke Penyimpanan." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks)?", "id": "Menggabungkan Disk (Kinerja Redundansi)." },
  { "en": "Apa Itu RAID 0 (Striping)?", "id": "Kinerja (Data Dibagi, Tanpa Redundansi)." },
  { "en": "Apa Itu RAID 1 (Mirroring)?", "id": "Redundansi (Data Disalin Persis)." },
  { "en": "Apa Itu RAID 5 (Striping With Parity)?", "id": "Keseimbangan (Striping Dengan Paritas)." },
  { "en": "Apa Itu RAID 6 (Striping With Double Parity)?", "id": "RAID 5 (Paritas Ganda)." },
  { "en": "Apa Itu RAID 10 (1+0)?", "id": "Kombinasi (Mirroring Dan Striping)." },
  { "en": "Apa Itu Hot Spare (Disk)?", "id": "Disk Cadangan Siaga (Menggantikan Gagal)." },
  { "en": "Apa Itu Telemetry (Telemetri)?", "id": "Pengumpulan Data Kinerja Jarak Jauh." },
  { "en": "Apa Itu OpenConfig?", "id": "Inisiatif Model Data Jaringan (Vendor-Neutral)." },
  { "en": "Apa Itu gRPC (gRPC Remote Procedure Calls)?", "id": "Framework RPC (Remote Procedure Call) Google." },
  { "en": "Apa Itu gNMI (gRPC Network Management Interface)?", "id": "Antarmuka Manajemen Jaringan (gRPC)." },
  { "en": "Apa Itu Prometheus?", "id": "Alat Monitoring Pemantauan (Open-Source)." },
  { "en": "Apa Itu Grafana?", "id": "Alat Visualisasi Dashboard (Open-Source)." },
  { "en": "Apa Itu ELK Stack?", "id": "Tumpukan Manajemen Log (Monitoring)." },
  { "en": "Apa Kepanjangan Dari ELK (Elasticsearch, Logstash, Kibana)?", "id": "Elasticsearch, Logstash, Kibana." },
  { "en": "Apa Itu Elasticsearch?", "id": "Mesin Pencari Penyimpanan Log (ELK)." },
  { "en": "Apa Itu Logstash?", "id": "Pengumpul Pemroses Log (ELK)." },
  { "en": "Apa Itu Kibana?", "id": "Visualisasi Dashboard Log (ELK)." },
  { "en": "Apa Itu Filebeat?", "id": "Pengirim Log Ringan (Ke Logstash)." },
  { "en": "Apa Itu Network Management System (NMS)?", "id": "Sistem Pemantauan Jaringan Terpusat." },
  { "en": "Apa Itu Model FCAPS?", "id": "Kerangka Kerja Manajemen Jaringan (ISO)." },
  { "en": "Apa Itu Fault Management (Manajemen Kesalahan)?", "id": "Proses Mendeteksi Mengisolasi Kesalahan." },
  { "en": "Apa Itu Configuration Management (Manajemen Konfigurasi)?", "id": "Proses Melacak Mengelola Konfigurasi." },
  { "en": "Apa Itu Accounting Management (Manajemen Akuntansi)?", "id": "Proses Melacak Penggunaan Jaringan." },
  { "en": "Apa Itu Performance Management (Manajemen Kinerja)?", "id": "Proses Memantau Menganalisis Kinerja." },
  { "en": "Apa Itu Security Management (Manajemen Keamanan)?", "id": "Proses Melindungi Jaringan Dari Ancaman." },
  { "en": "Apa Itu Network Operations Center (NOC)?", "id": "Pusat Pemantauan Operasi Jaringan." },
  { "en": "Apa Itu Security Operations Center (SOC)?", "id": "Pusat Pemantauan Keamanan Jaringan." },
  { "en": "Apa Beda NOC (Network Operations Center) Dan SOC (Security Operations Center)?", "id": "NOC Operasi, SOC Keamanan." },
  { "en": "Apa Itu Trouble Ticket?", "id": "Catatan Laporan Insiden Atau Masalah." },
  { "en": "Apa Itu Eskalasi (Manajemen Insiden)?", "id": "Meningkatkan Insiden Ke Level Support." },
  { "en": "Mengapa Network Baseline Penting?", "id": "Acuan Deteksi Anomali Kinerja." },
  { "en": "Apa Itu Bottleneck (Leher Botol)?", "id": "Titik Kepadatan Penghambat Kinerja." },
  { "en": "Apa Itu Wireshark?", "id": "Alat Analisis Protokol Jaringan (Sniffer)." },
  { "en": "Apa Itu Packet Sniffer?", "id": "Perangkat Lunak Merekam Paket Jaringan." },
  { "en": "Apa Itu MTR (My Traceroute)?", "id": "Alat Diagnostik (Gabungan Ping Traceroute)." },
  { "en": "Apa Kepanjangan Dari MTR (My Traceroute)?", "id": "My Traceroute." },
  { "en": "Apa Itu Iperf?", "id": "Alat Pengukur Performa Jaringan (Bandwidth)." },
  { "en": "Apa Itu Nmap (Network Mapper)?", "id": "Alat Pemindai Jaringan Port Scanner." },
  { "en": "Apa Kepanjangan Dari Nmap (Network Mapper)?", "id": "Network Mapper." },
  { "en": "Apa Itu Perintah 'Netstat'?", "id": "Menampilkan Statistik Koneksi Jaringan (Aktif)." },
  { "en": "Apa Itu Perintah 'Ipconfig'?", "id": "Menampilkan Konfigurasi IP (Windows)." },
  { "en": "Apa Itu Perintah 'Ifconfig'?", "id": "Menampilkan Konfigurasi IP (Linux, Lama)." },
  { "en": "Apa Itu Perintah 'Ip Address' (Linux)?", "id": "Perintah Konfigurasi IP (Pengganti Ifconfig)." },
  { "en": "Apa Itu Perintah 'Arp -a'?", "id": "Menampilkan Isi Cache ARP (Address Resolution Protocol)." },
  { "en": "Apa Itu Perintah 'Route Print'?", "id": "Menampilkan Tabel Routing (Windows)." },
  { "en": "Apa Itu Perintah 'Netsh' (Windows)?", "id": "Alat Konfigurasi Jaringan (Command-Line)." },
  { "en": "Apa Itu Model Troubleshooting OSI (Open Systems Interconnection)?", "id": "Pendekatan Lapis Demi Lapis." },
  { "en": "Apa Itu Metode Bottom-Up (Troubleshooting)?", "id": "Memulai Dari Lapis 1 Ke Atas." },
  { "en": "Apa Itu Metode Top-Down (Troubleshooting)?", "id": "Memulai Dari Lapis 7 Ke Bawah." },
  { "en": "Apa Itu Metode Divide-and-Conquer?", "id": "Memulai Dari Lapis Tengah (Lapis 3)." },
  { "en": "Masalah Apa Di Lapis Fisik?", "id": "Kabel Putus, Port Rusak, Sinyal." },
  { "en": "Masalah Apa Di Lapis Data Link?", "id": "Error MAC, Masalah VLAN, Loop STP." },
  { "en": "Masalah Apa Di Lapis Jaringan?", "id": "Kesalahan Routing, Subnet Mask, ACL." },
  { "en": "Masalah Apa Di Lapis Transport?", "id": "Port Diblokir Firewall, Error TCP." },
  { "en": "Masalah Apa Di Lapis Aplikasi?", "id": "Kesalahan DNS, Konfigurasi Aplikasi." },
  { "en": "Apa Itu Kabel Konsol (Console Cable)?", "id": "Kabel Akses Manajemen Perangkat (CLI)." },
  { "en": "Apa Itu Out-of-Band (OOB) Management?", "id": "Manajemen Lewat Jaringan Terpisah." },
  { "en": "Apa Kepanjangan Dari OOB (Out-of-Band)?", "id": "Out-of-Band." },
  { "en": "Apa Itu In-Band Management?", "id": "Manajemen Lewat Jaringan Data (Telnet)." },
  { "en": "Apa Itu Jaringan Hierarkis?", "id": "Model Desain Jaringan (Lapis Terstruktur)." },
  { "en": "Sebutkan Tiga Lapis Model Hierarkis?", "id": "Core, Distribution, Access." },
  { "en": "Apa Itu Lapis Core (Core Layer)?", "id": "Lapis Inti (Backbone Kecepatan Tinggi)." },
  { "en": "Fungsi Lapis Core?", "id": "Switching Paket Sangat Cepat." },
  { "en": "Fungsi Lapis Distribusi?", "id": "Routing Antar VLAN, ACL, QoS." },
  { "en": "Apa Itu Lapis Akses (Access Layer)?", "id": "Lapis Tepi (Koneksi Pengguna Akhir)." },
  { "en": "Fungsi Lapis Akses?", "id": "Port Security, PoE, VLAN." },
  { "en": "Apa Itu Desain Collapsed Core?", "id": "Gabungan Lapis Core Distribusi." },
  { "en": "Apa Itu Redundansi Jaringan?", "id": "Duplikasi Perangkat Jalur (Cadangan)." },
  { "en": "Apa Itu Link Aggregation (LAG)?", "id": "Menggabungkan Beberapa Link Fisik." },
  { "en": "Apa Itu Load Balancing?", "id": "Distribusi Trafik Di Beberapa Jalur." },
  { "en": "Apa Itu Failover?", "id": "Pengalihan Otomatis Ke Jalur Cadangan." },
  { "en": "Apa Itu Site-to-Site VPN (Virtual Private Network)?", "id": "Menghubungkan Dua Jaringan (Kantor)." },
  { "en": "Apa Itu Remote Access VPN (Virtual Private Network)?", "id": "Menghubungkan Pengguna Individu (Remote)." },
  { "en": "Apa Itu Split Tunneling (VPN)?", "id": "Membagi Trafik (Internet VPN)." },
  { "en": "Apa Itu Full Tunneling (VPN)?", "id": "Semua Trafik Melewati Tunnel VPN." },
  { "en": "Apa Itu Dynamic Multipoint VPN (DMVPN)?", "id": "Solusi VPN (Virtual Private Network) Skalabel." },
  { "en": "Apa Itu IPsec (Internet Protocol Security) Over GRE (Generic Routing Encapsulation)?", "id": "Tunnel GRE (Generic Routing Encapsulation) Aman." },
  { "en": "Apa Itu Borderless Network?", "id": "Konsep Jaringan (Akses Dimanapun Aman)." },
  { "en": "Apa Itu BYOD (Bring Your Own Device)?", "id": "Membawa Perangkat Pribadi Bekerja." },
  { "en": "Apa Itu Tamu (Guest) Network?", "id": "Jaringan Terisolasi Untuk Tamu." },
  { "en": "Apa Itu Karantina (Quarantine) Jaringan?", "id": "Isolasi Perangkat Tidak Aman (NAC)." },
  { "en": "Apa Itu Patch Panel?", "id": "Panel Terminasi Kabel Terstruktur." },
  { "en": "Apa Itu Pengkabelan Terstruktur?", "id": "Sistem Pengkabelan Standar Gedung." },
  { "en": "Apa Itu Main Distribution Frame (MDF)?", "id": "Titik Distribusi Kabel Utama Gedung." },
  { "en": "Apa Kepanjangan Dari MDF (Main Distribution Frame)?", "id": "Main Distribution Frame." },
  { "en": "Apa Itu Intermediate Distribution Frame (IDF)?", "id": "Titik Distribusi Kabel Sekunder (Per Lantai)." },
  { "en": "Apa Kepanjangan Dari IDF (Intermediate Distribution Frame)?", "id": "Intermediate Distribution Frame." },
  { "en": "Apa Itu Horizontal Cabling?", "id": "Pengkabelan Dari IDF (Intermediate Distribution Frame) Outlet." },
  { "en": "Apa Itu Vertical Cabling (Backbone)?", "id": "Pengkabelan Antar MDF (Main Distribution Frame) IDF." },
  { "en": "Apa Itu Work Area?", "id": "Area Outlet Kabel Ke Perangkat." },
  { "en": "Apa Itu Telecommunications Room (TR)?", "id": "Ruangan Tempat IDF (Intermediate Distribution Frame)." },
  { "en": "Apa Kepanjangan Dari TR (Telecommunications Room)?", "id": "Telecommunications Room." },
  { "en": "Apa Itu Equipment Room (ER)?", "id": "Ruangan Tempat Peralatan Jaringan (MDF)." },
  { "en": "Apa Kepanjangan Dari ER (Equipment Room)?", "id": "Equipment Room." },
  { "en": "Apa Itu Entrance Facility (EF)?", "id": "Titik Masuk Layanan Provider Gedung." },
  { "en": "Apa Kepanjangan Dari EF (Entrance Facility)?", "id": "Entrance Facility." },
  { "en": "Apa Itu Smart Jack?", "id": "Perangkat Demarc (Diagnostik Loopback)." },
  { "en": "Apa Itu Loopback Test?", "id": "Tes Diagnostik (Sinyal Kembali Ke Sumber)." },
  { "en": "Apa Itu Time-Domain Reflectometer (TDR)?", "id": "Alat Uji (Lokasi Putus Kabel Tembaga)." },
  { "en": "Apa Kepanjangan Dari TDR (Time-Domain Reflectometer)?", "id": "Time-Domain Reflectometer." },
  { "en": "Apa Itu Optical TDR (OTDR)?", "id": "TDR (Time-Domain Reflectometer) Kabel Serat Optik." },
  { "en": "Apa Itu Cable Certifier?", "id": "Alat Uji (Validasi Kinerja Kabel)." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur Listrik (Voltase, Resistansi)." },
  { "en": "Apa Itu Cable Stripper?", "id": "Alat Pengupas Kulit Kabel." },
  { "en": "Apa Itu Crimper?", "id": "Alat Pemasang Konektor (RJ45)." },
  { "en": "Apa Itu Punch-Down Tool?", "id": "Alat Terminasi Kabel (Ke Patch Panel)." },
  { "en": "Apa Itu Fox and Hound (Tone Generator)?", "id": "Alat Pelacak Kabel (Tone Probe)." },
  { "en": "Apa Itu Standar EIA/TIA-568?", "id": "Standar Pengkabelan Telekomunikasi Komersial." },
  { "en": "Apa Itu Standar T568A?", "id": "Urutan Pasangan Kabel (Hijau-Putih, Hijau)." },
  { "en": "Apa Itu Standar T568B?", "id": "Urutan Pasangan Kabel (Oranye-Putih, Oranye)." },
  { "en": "Standar Mana Paling Umum (Komersial)?", "id": "T568B." },
  { "en": "Apa Itu Pasangan Terpilin (Twisted Pair)?", "id": "Dua Kawat Dipilin (Mengurangi Crosstalk)." },
  { "en": "Apa Itu Crosstalk?", "id": "Interferensi Antar Kabel Berdekatan." },
  { "en": "Apa Itu Near-End Crosstalk (NEXT)?", "id": "Crosstalk Diukur Di Ujung Pengirim." },
  { "en": "Apa Itu Far-End Crosstalk (FEXT)?", "id": "Crosstalk Diukur Di Ujung Penerima." },
  { "en": "Apa Itu Alien Crosstalk (AXT)?", "id": "Crosstalk Antar Kabel Yang Berbeda." },
  { "en": "Apa Kepanjangan Dari AXT (Alien Crosstalk)?", "id": "Alien Crosstalk." },
  { "en": "Bagaimana Mengurangi Alien Crosstalk (AXT)?", "id": "Gunakan Kabel STP (Shielded Twisted Pair)." },
  { "en": "Apa Itu Return Loss (RL)?", "id": "Ukuran Sinyal Yang Dipantulkan Balik." },
  { "en": "Apa Kepanjangan Dari RL (Return Loss)?", "id": "Return Loss." },
  { "en": "Nilai Return Loss (RL) Baik (Satuan dB)?", "id": "Semakin Tinggi Semakin Baik." },
  { "en": "Apa Itu Insertion Loss (IL)?", "id": "Pelemahan Sinyal Akibat Komponen." },
  { "en": "Apa Kepanjangan Dari IL (Insertion Loss)?", "id": "Insertion Loss." },
  { "en": "Nilai Insertion Loss (IL) Baik (Satuan dB)?", "id": "Semakin Rendah Semakin Baik." },
  { "en": "Apa Itu Propagation Delay (Jaringan)?", "id": "Waktu Sinyal Merambat Media." },
  { "en": "Apa Itu Delay Skew?", "id": "Perbedaan Waktu Tiba (Antar Pasangan)." },
  { "en": "Apa Itu Power Sum NEXT (PSNEXT)?", "id": "Total NEXT (Near-End Crosstalk) Semua Pasangan." },
  { "en": "Apa Kepanjangan Dari PSNEXT (Power Sum NEXT)?", "id": "Power Sum Near-End Crosstalk." },
  { "en": "Apa Itu Power Sum FEXT (PSFEXT)?", "id": "Total FEXT (Far-End Crosstalk) Semua Pasangan." },
  { "en": "Apa Kepanjangan Dari PSFEXT (Power Sum FEXT)?", "id": "Power Sum Far-End Crosstalk." },
  { "en": "Apa Itu Kabel Kategori 6a (Cat 6a)?", "id": "Kabel UTP (Unshielded Twisted Pair) 10 Gbps." },
  { "en": "Apa Itu Kabel Kategori 7 (Cat 7)?", "id": "Kabel STP (Shielded Twisted Pair) (Shield Per Pasang)." },
  { "en": "Apa Itu Kabel Kategori 8 (Cat 8)?", "id": "Kabel STP (Shielded Twisted Pair) (Data Center)." },
  { "en": "Apa Itu Balanced (Kabel)?", "id": "Sinyal Dikirim Lewat Dua Kawat." },
  { "en": "Contoh Kabel Balanced?", "id": "Kabel Twisted Pair." },
  { "en": "Apa Itu Unbalanced (Kabel)?", "id": "Sinyal Dikirim (Satu Kawat, Satu Ground)." },
  { "en": "Contoh Kabel Unbalanced?", "id": "Kabel Koaksial." },
  { "en": "Apa Itu Common Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Menolak Noise Bersama." },
  { "en": "Apa Kepanjangan Dari CMRR (Common Mode Rejection Ratio)?", "id": "Common Mode Rejection Ratio." },
  { "en": "Apa Itu Differential Signal?", "id": "Sinyal Berbasis Perbedaan Tegangan (Balanced)." },
  { "en": "Apa Itu Network Interface (Antarmuka Jaringan)?", "id": "Titik Koneksi Perangkat Ke Jaringan." },
  { "en": "Apa Itu 10/100/1000 Port?", "id": "Port Mendukung Tiga Kecepatan Ethernet." },
  { "en": "Apa Itu Small Form-factor Pluggable (SFP)?", "id": "Modul Transceiver Optik Modular (1G)." },
  { "en": "Apa Itu QSFP (Quad Small Form-factor Pluggable)?", "id": "Modul Transceiver (40G Atau 100G)." },
  { "en": "Apa Itu Breakout Cable (Kabel Breakout)?", "id": "Kabel (Satu QSFP Ke Empat SFP+)." },
  { "en": "Apa Itu Direct Attach Copper (DAC)?", "id": "Kabel Tembaga Transceiver Terpasang." },
  { "en": "Apa Kepanjangan Dari DAC (Direct Attach Copper)?", "id": "Direct Attach Copper." },
  { "en": "Apa Itu Active Optical Cable (AOC)?", "id": "Kabel Optik Transceiver Terpasang." },
  { "en": "Apa Keuntungan DAC (Direct Attach Copper) AOC (Active Optical Cable)?", "id": "Lebih Murah, Sederhana." },
  { "en": "Apa Itu Hot-Swappable?", "id": "Dapat Dicabut Dipasang (Tanpa Mematikan)." },
  { "en": "Apakah Modul SFP (Small Form-factor Pluggable) Hot-Swappable?", "id": "Ya, Sebagian Besar Hot-Swappable." },
  { "en": "Apa Itu Jaringan Nirkabel Ad Hoc?", "id": "Jaringan Peer-to-Peer (Tanpa AP)." },
  { "en": "Apa Itu Jaringan Nirkabel Infrastruktur?", "id": "Jaringan Nirkabel (Menggunakan Access Point)." },
  { "en": "Apa Itu RTS (Request to Send)?", "id": "Sinyal Klien (Meminta Izin Kirim)." },
  { "en": "Apa Itu CTS (Clear to Send)?", "id": "Sinyal AP (Memberi Izin Kirim)." },
  { "en": "Apa Fungsi RTS/CTS (Request to Send/Clear to Send)?", "id": "Mengatasi Masalah Hidden Node." },
  { "en": "Apa Itu Masalah Exposed Node?", "id": "Klien (Tertahan Mengirim, Sebenarnya Bisa)." },
  { "en": "Apa Itu Inter-Frame Space (IFS)?", "id": "Waktu Jeda Antar Frame Nirkabel." },
  { "en": "Apa Kepanjangan Dari IFS (Inter-Frame Space)?", "id": "Inter-Frame Space." },
  { "en": "Apa Itu DIFS (Distributed IFS)?", "id": "Waktu Tunggu Klien (Kirim Data)." },
  { "en": "Apa Kepanjangan Dari DIFS (Distributed Inter-Frame Space)?", "id": "Distributed Inter-Frame Space." },
  { "en": "Apa Itu SIFS (Short IFS)?", "id": "Waktu Tunggu Terpendek (Balasan ACK)." },
  { "en": "Apa Kepanjangan Dari SIFS (Short Inter-Frame Space)?", "id": "Short Inter-Frame Space." },
  { "en": "Apa Itu Contention Window (CW)?", "id": "Waktu Tunggu Acak (Backoff)." },
  { "en": "Apa Kepanjangan Dari CW (Contention Window)?", "id": "Contention Window." },
  { "en": "Apa Itu Network Allocation Vector (NAV)?", "id": "Timer (Menandakan Media Sibuk)." },
  { "en": "Apa Kepanjangan Dari NAV (Network Allocation Vector)?", "id": "Network Allocation Vector." },
  { "en": "Apa Itu Beacon Frame?", "id": "Sinyal Periodik (Informasi Jaringan AP)." },
  { "en": "Apa Itu Probe Request Frame?", "id": "Frame Klien (Mencari Jaringan Aktif)." },
  { "en": "Apa Itu Probe Response Frame?", "id": "Frame Balasan AP (Atas Probe)." },
  { "en": "Apa Itu Association Request Frame?", "id": "Frame Klien (Meminta Bergabung BSS)." },
  { "en": "Apa Itu Association Response Frame?", "id": "Frame Balasan AP (Status Asosiasi)." },
  { "en": "Apa Itu Authentication Frame?", "id": "Frame Proses Autentikasi Nirkabel." },
  { "en": "Apa Itu Deauthentication Frame?", "id": "Frame Memutuskan Koneksi (Paksa)." },
  { "en": "Apa Itu Disassociation Frame?", "id": "Frame Memutuskan Koneksi (Normal)." },
  { "en": "Apa Itu Power Saving Mode (PSM) Wi-Fi?", "id": "Mode Hemat Daya (Klien Nirkabel)." },
  { "en": "Apa Kepanjangan Dari PSM (Power Saving Mode)?", "id": "Power Saving Mode." },
  { "en": "Apa Itu DTIM (Delivery Traffic Indication Message)?", "id": "Indikasi (Data Broadcast Untuk Klien PSM)." },
  { "en": "Apa Kepanjangan Dari DTIM (Delivery Traffic Indication Message)?", "id": "Delivery Traffic Indication Message." },
  { "en": "Apa Itu Wi-Fi Multimedia (WMM)?", "id": "Standar QoS (Quality of Service) Nirkabel." },
  { "en": "Apa Kepanjangan Dari WMM (Wi-Fi Multimedia)?", "id": "Wi-Fi Multimedia." },
  { "en": "Apa Itu EDCA (Enhanced Distributed Channel Access)?", "id": "Metode Akses (Prioritas WMM)." },
  { "en": "Apa Kepanjangan Dari EDCA (Enhanced Distributed Channel Access)?", "id": "Enhanced Distributed Channel Access." },
  { "en": "Sebutkan Kategori Akses EDCA (Enhanced Distributed Channel Access)?", "id": "Suara, Video, Best Effort, Background." },
  { "en": "Apa Itu WMM (Wi-Fi Multimedia) Power Save (WMM-PS)?", "id": "Hemat Daya (Lebih Baik Dari PSM)." },
  { "en": "Apa Kepanjangan Dari WMM-PS (WMM Power Save)?", "id": "Wi-Fi Multimedia Power Save." },
  { "en": "Apa Itu 802.11k (Radio Resource Management)?", "id": "Standar (Informasi Tetangga Untuk Roaming)." },
  { "en": "Apa Itu 802.11r (Fast BSS Transition)?", "id": "Standar (Roaming Cepat Antar AP)." },
  { "en": "Apa Kepanjangan Dari FT (Fast BSS Transition)?", "id": "Fast BSS Transition." },
  { "en": "Apa Itu 802.11v (Wireless Network Management)?", "id": "Standar (Manajemen Klien Nirkabel)." },
  { "en": "Apa Itu Roaming (WLAN)?", "id": "Perpindahan Klien Antar Access Point." },
  { "en": "Apa Itu Sticky Client Problem?", "id": "Klien (Tetap Terhubung AP Jauh)." },
  { "en": "Apa Itu Client Steering?", "id": "AP (Access Point) Mengarahkan Klien (Ke AP Lain)." },
  { "en": "Apa Itu Airtime Fairness?", "id": "Fitur (Waktu Akses Adil Antar Klien)." },
  { "en": "Apa Itu Wireless Mesh Network (WMN)?", "id": "Jaringan Nirkabel (AP Terhubung Nirkabel)." },
  { "en": "Apa Kepanjangan Dari WMN (Wireless Mesh Network)?", "id": "Wireless Mesh Network." },
  { "en": "Apa Itu Root AP (Access Point) (Mesh)?", "id": "AP (Access Point) Terhubung Jaringan Kabel." },
  { "en": "Apa Itu Mesh AP (Access Point)?", "id": "AP (Access Point) Terhubung Nirkabel (Ke Root)." },
  { "en": "Apa Itu Wireless Distribution System (WDS)?", "id": "Sistem Menghubungkan AP (Access Point) Nirkabel." },
  { "en": "Apa Itu Site Survey (WLAN)?", "id": "Survei Penempatan AP (Access Point)." },
  { "en": "Apa Itu Site Survey Aktif?", "id": "Survei (Klien Terhubung Ke Jaringan)." },
  { "en": "Apa Itu Site Survey Pasif?", "id": "Survei (Hanya Mendengarkan Sinyal AP)." },
  { "en": "Apa Itu Site Survey Prediktif?", "id": "Survei (Simulasi Menggunakan Perangkat Lunak)." },
  { "en": "Apa Itu Spectrum Analyzer (WLAN)?", "id": "Alat Analisis Spektrum Frekuensi Radio." },
  { "en": "Apa Fungsi Spectrum Analyzer?", "id": "Mendeteksi Sumber Interferensi Non-Wi-Fi." },
  { "en": "Apa Itu Interferensi Co-Channel?", "id": "Interferensi (Dua AP Channel Sama)." },
  { "en": "Apa Itu Interferensi Adjacent-Channel?", "id": "Interferensi (Dua AP Channel Dekat)." },
  { "en": "Apa Itu Jaringan Seluler?", "id": "Jaringan Nirkabel (Area Terbagi Sel)." },
  { "en": "Apa Itu Sel (Cell)?", "id": "Area Geografis Dilayani Satu BTS." },
  { "en": "Apa Itu Base Transceiver Station (BTS)?", "id": "Perangkat Pemancar Penerima (Sel)." },
  { "en": "Apa Itu Frequency Reuse?", "id": "Penggunaan Ulang Frekuensi (Sel Berbeda)." },
  { "en": "Apa Itu Cluster (Seluler)?", "id": "Grup Sel (Frekuensi Tidak Berulang)." },
  { "en": "Apa Itu Sektorisasi (Antena)?", "id": "Membagi Sel Menjadi Sektor (Antena)." },
  { "en": "Apa Itu Handover (Handoff)?", "id": "Proses Perpindahan Panggilan Antar Sel." },
  { "en": "Apa Itu Global System for Mobile Communications (GSM)?", "id": "Standar Jaringan Seluler 2G." },
  { "en": "Apa Kepanjangan Dari GSM (Global System for Mobile Communications)?", "id": "Global System for Mobile Communications." },
  { "en": "Teknologi Akses Apa Digunakan GSM (Global System for Mobile Communications)?", "id": "TDMA (Time Division Multiple Access) FDMA." },
  { "en": "Apa Itu Subscriber Identity Module (SIM)?", "id": "Kartu Identitas Pelanggan GSM." },
  { "en": "Apa Itu Base Station Subsystem (BSS)?", "id": "Bagian RAN (Radio Access Network) Jaringan GSM." },
  { "en": "Apa Kepanjangan Dari BSS (Base Station Subsystem)?", "id": "Base Station Subsystem." },
  { "en": "Komponen Apa Saja Dalam BSS (Base Station Subsystem)?", "id": "BTS (Base Transceiver Station) BSC." },
  { "en": "Apa Itu Base Station Controller (BSC)?", "id": "Pengontrol Beberapa BTS (Base Transceiver Station)." },
  { "en": "Apa Itu Network Switching Subsystem (NSS)?", "id": "Jaringan Inti (Core Network) GSM." },
  { "en": "Apa Kepanjangan Dari NSS (Network Switching Subsystem)?", "id": "Network Switching Subsystem." },
  { "en": "Apa Itu Mobile Switching Center (MSC)?", "id": "Pusat Penyambungan Panggilan (NSS)." },
  { "en": "Apa Itu Home Location Register (HLR)?", "id": "Database Utama Pelanggan (NSS)." },
  { "en": "Apa Itu Visitor Location Register (VLR)?", "id": "Database Temporer Pelanggan (NSS)." },
  { "en": "Apa Itu Authentication Center (AUC)?", "id": "Pusat Autentikasi Keamanan (NSS)." },
  { "en": "Apa Itu Equipment Identity Register (EIR)?", "id": "Database Validasi IMEI (International Mobile Equipment Identity)." },
  { "en": "Apa Itu Enhanced Data rates for GSM Evolution (EDGE)?", "id": "Peningkatan GPRS (General Packet Radio Service)." },
  { "en": "Apa Itu Serving GPRS Support Node (SGSN)?", "id": "Node GPRS (General Packet Radio Service) (Mobilitas)." },
  { "en": "Apa Itu Gateway GPRS Support Node (GGSN)?", "id": "Node GPRS (General Packet Radio Service) (Gateway)." },
  { "en": "Apa Itu Code Division Multiple Access (CDMA)?", "id": "Teknologi Akses (Kode Unik)." },
  { "en": "Standar CDMA (Code Division Multiple Access) 2G?", "id": "IS-95 (cdmaOne)." },
  { "en": "Standar CDMA (Code Division Multiple Access) 3G?", "id": "CDMA2000 (EV-DO)." },
  { "en": "Apa Itu Universal Mobile Telecommunications System (UMTS)?", "id": "Standar Jaringan 3G (Turunan GSM)." },
  { "en": "Apa Itu Wideband CDMA (W-CDMA)?", "id": "Teknologi Antarmuka Udara UMTS." },
  { "en": "Apa Kepanjangan Dari W-CDMA (Wideband CDMA)?", "id": "Wideband Code Division Multiple Access." },
  { "en": "Apa Itu Node B?", "id": "Nama BTS (Base Transceiver Station) Jaringan 3G." },
  { "en": "Apa Itu Radio Network Controller (RNC)?", "id": "Pengontrol Node B (Jaringan 3G)." },
  { "en": "Apa Itu UTRAN (UMTS Terrestrial Radio Access Network)?", "id": "Jaringan RAN (Radio Access Network) 3G." },
  { "en": "Apa Kepanjangan Dari UTRAN (UMTS Terrestrial Radio Access Network)?", "id": "UMTS Terrestrial Radio Access Network." },
  { "en": "Apa Itu High-Speed Packet Access (HSPA)?", "id": "Peningkatan Kecepatan Data 3G." },
  { "en": "Apa Itu HSDPA (High-Speed Downlink Packet Access)?", "id": "HSPA (High-Speed Packet Access) (Downlink)." },
  { "en": "Apa Kepanjangan Dari HSDPA (High-Speed Downlink Packet Access)?", "id": "High-Speed Downlink Packet Access." },
  { "en": "Apa Itu HSUPA (High-Speed Uplink Packet Access)?", "id": "HSPA (High-Speed Packet Access) (Uplink)." },
  { "en": "Apa Kepanjangan Dari HSUPA (High-Speed Uplink Packet Access)?", "id": "High-Speed Uplink Packet Access." },
  { "en": "Apa Itu HSPA+ (Evolved HSPA)?", "id": "Peningkatan HSPA (High-Speed Packet Access)." },
  { "en": "Apa Itu Long-Term Evolution (LTE)?", "id": "Standar Jaringan 4G." },
  { "en": "Teknologi Akses Downlink LTE (Long-Term Evolution)?", "id": "OFDMA (Orthogonal Frequency Division Multiple Access)." },
  { "en": "Teknologi Akses Uplink LTE (Long-Term Evolution)?", "id": "SC-FDMA (Single-Carrier Frequency Division Multiple Access)." },
  { "en": "Apa Itu E-UTRAN (Evolved UTRAN)?", "id": "Jaringan RAN (Radio Access Network) 4G LTE." },
  { "en": "Apa Kepanjangan Dari E-UTRAN (Evolved UTRAN)?", "id": "Evolved UMTS Terrestrial Radio Access Network." },
  { "en": "Apa Itu eNodeB (Evolved Node B)?", "id": "BTS (Base Transceiver Station) Jaringan 4G LTE." },
  { "en": "Fungsi Apa Digabung eNodeB?", "id": "Fungsi Node B Dan RNC." },
  { "en": "Apa Itu Mobility Management Entity (MME)?", "id": "Node EPC (Evolved Packet Core) (Manajemen Sesi)." },
  { "en": "Apa Itu Serving Gateway (S-GW)?", "id": "Node EPC (Evolved Packet Core) (Routing Data)." },
  { "en": "Apa Itu PDN (Packet Data Network) Gateway (P-GW)?", "id": "Node EPC (Evolved Packet Core) (Gerbang Internet)." },
  { "en": "Apa Itu Home Subscriber Server (HSS)?", "id": "Database Pelanggan 4G (HLR + AUC)." },
  { "en": "Apa Itu Policy and Charging Rules Function (PCRF)?", "id": "Node EPC (Evolved Packet Core) (Aturan QoS)." },
  { "en": "Apa Kepanjangan Dari PCRF (Policy and Charging Rules Function)?", "id": "Policy and Charging Rules Function." },
  { "en": "Apa Itu Antarmuka S1 (LTE)?", "id": "Antarmuka Antara eNodeB EPC." },
  { "en": "Apa Itu Antarmuka X2 (LTE)?", "id": "Antarmuka Antara eNodeB (Handover)." },
  { "en": "Apa Itu IP (Internet Protocol) Multimedia Subsystem (IMS)?", "id": "Platform Layanan Multimedia (VoLTE)." },
  { "en": "Apa Itu LTE (Long-Term Evolution) Advanced (LTE-A)?", "id": "Peningkatan Standar LTE (Generasi 4.5G)." },
  { "en": "Apa Itu Carrier Aggregation (CA)?", "id": "Fitur LTE-A (Menggabungkan Frekuensi)." },
  { "en": "Apa Kepanjangan Dari CA (Carrier Aggregation)?", "id": "Carrier Aggregation." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output) Lanjutan?", "id": "Fitur LTE-A (Antena Ganda)." },
  { "en": "Apa Itu Coordinated Multi-Point (CoMP)?", "id": "Fitur LTE-A (Koordinasi Antar Sel)." },
  { "en": "Apa Kepanjangan Dari CoMP (Coordinated Multi-Point)?", "id": "Coordinated Multi-Point." },
  { "en": "Apa Itu 5G New Radio (NR)?", "id": "Antarmuka Udara Standar 5G." },
  { "en": "Apa Kepanjangan Dari 5GC (5G Core)?", "id": "5G Core." },
  { "en": "Apa Itu Enhanced Mobile Broadband (eMBB)?", "id": "Kasus Penggunaan 5G (Data Cepat)." },
  { "en": "Apa Itu Massive Machine-Type Communication (mMTC)?", "id": "Kasus Penggunaan 5G (Banyak IoT)." },
  { "en": "Apa Itu Network Slicing (5G)?", "id": "Membagi Jaringan (Virtual Slices)." },
  { "en": "Apa Itu Frekuensi mmWave (Milimeter Wave)?", "id": "Pita Frekuensi 5G (Sangat Tinggi)." },
  { "en": "Apa Itu Frekuensi Sub-6 GHz?", "id": "Pita Frekuensi 5G (Di Bawah 6 GHz)." },
  { "en": "Apa Itu Beamforming (5G)?", "id": "Menggunakan Sinyal Terfokus Ke Pengguna." },
  { "en": "Apa Itu Massive MIMO (Multiple Input Multiple Output)?", "id": "Sistem MIMO (Banyak Antena 5G)." },
  { "en": "Apa Itu Open RAN (O-RAN)?", "id": "Arsitektur RAN (Radio Access Network) Terbuka." },
  { "en": "Apa Kepanjangan Dari O-RAN (Open RAN)?", "id": "Open Radio Access Network." },
  { "en": "Apa Itu Centralized Unit (CU) 5G?", "id": "Unit Terpusat (Proses Lapis Atas)." },
  { "en": "Apa Itu Distributed Unit (DU) 5G?", "id": "Unit Terdistribusi (Proses Lapis Bawah)." },
  { "en": "Apa Itu Radio Unit (RU) 5G?", "id": "Unit Radio (Fisik Antena)." },
  { "en": "Apa Itu Fronthaul (5G)?", "id": "Koneksi Antara DU (Distributed Unit) RU." },
  { "en": "Apa Itu Midhaul (5G)?", "id": "Koneksi Antara CU (Centralized Unit) DU." },
  { "en": "Apa Itu Backhaul (5G)?", "id": "Koneksi Antara CU (Centralized Unit) Core." },
  { "en": "Apa Itu Core Network (Jaringan Inti)?", "id": "Jaringan Pusat (Fungsi Kontrol Data)." },
  { "en": "Apa Itu Access Network (Jaringan Akses)?", "id": "Jaringan Tepi (Penghubung Pengguna)." },
  { "en": "Apa Itu Transport Network (Jaringan Transport)?", "id": "Jaringan Penghubung (Backhaul, Fronthaul)." },
  { "en": "Apa Itu Visible Light Communication (VLC)?", "id": "Komunikasi Menggunakan Cahaya Tampak (LED)." },
  { "en": "Apa Keuntungan Li-Fi (Light Fidelity)?", "id": "Sangat Cepat, Aman (Tidak Tembus Dinding)." },
  { "en": "Apa Kerugian Li-Fi (Light Fidelity)?", "id": "Jangkauan Pendek, Memerlukan Garis Pandang." },
  { "en": "Apa Itu Quantum Communication (Komunikasi Kuantum)?", "id": "Komunikasi Berbasis Prinsip Kuantum." },
  { "en": "Apa Itu Quantum Key Distribution (QKD)?", "id": "Distribusi Kunci Kriptografi Kuantum." },
  { "en": "Apa Keuntungan QKD (Quantum Key Distribution)?", "id": "Sangat Aman (Tidak Bisa Disadap)." },
  { "en": "Apa Itu Quantum Entanglement?", "id": "Keterikatan Kuantum (Partikel Terhubung)." },
  { "en": "Apa Itu Free-Space Optical (FSO) Communication?", "id": "Komunikasi Optik Lewat Udara (Laser)." },
  { "en": "Apa Kepanjangan Dari FSO (Free-Space Optical)?", "id": "Free-Space Optical." },
  { "en": "Apa Tantangan FSO (Free-Space Optical)?", "id": "Cuaca Buruk (Kabut, Hujan)." },
  { "en": "Apa Itu Acoustic Communication (Komunikasi Akustik)?", "id": "Komunikasi Menggunakan Gelombang Suara." },
  { "en": "Dimana Komunikasi Akustik Digunakan?", "id": "Komunikasi Bawah Air (Sonar)." },
  { "en": "Apa Itu Software-Defined Radio (SDR)?", "id": "Radio (Komponen Didefinisikan Perangkat Lunak)." },
  { "en": "Apa Itu Cognitive Radio (CR)?", "id": "Radio Cerdas (Mendeteksi Spektrum Kosong)." },
  { "en": "Apa Itu Dynamic Spectrum Access (DSA)?", "id": "Akses Spektrum Secara Dinamis (Opportunistik)." },
  { "en": "Apa Itu Spectrum Sensing?", "id": "Proses Mendeteksi Spektrum Yang Kosong." },
  { "en": "Apa Itu White Space (Spektrum)?", "id": "Spektrum Frekuensi (Tidak Digunakan Primer)." },
  { "en": "Apa Itu TV White Space (TVWS)?", "id": "White Space (Spektrum) Di Pita TV." },
  { "en": "Apa Itu Information Theory (Teori Informasi)?", "id": "Studi Kuantifikasi Informasi (Shannon)." },
  { "en": "Apa Itu Entropi (Teori Informasi)?", "id": "Ukuran Ketidakpastian Suatu Informasi." },
  { "en": "Apa Itu Channel Capacity (Kapasitas Saluran)?", "id": "Laju Data Maksimal (Tanpa Error)." },
  { "en": "Apa Itu Teorema Shannon-Hartley?", "id": "Rumus Menghitung Kapasitas Saluran." },
  { "en": "Apa Itu Source Coding (Pengkodean Sumber)?", "id": "Proses Kompresi Data (Redundansi)." },
  { "en": "Tujuan Source Coding?", "id": "Mengurangi Jumlah Bit Data (Efisien)." },
  { "en": "Contoh Source Coding?", "id": "Huffman Coding, Lempel-Ziv." },
  { "en": "Apa Itu Channel Coding (Pengkodean Kanal)?", "id": "Proses Menambah Redundansi (Error)." },
  { "en": "Tujuan Channel Coding?", "id": "Deteksi Koreksi Kesalahan Transmisi." },
  { "en": "Contoh Channel Coding?", "id": "Hamming Code, Turbo Code, LDPC." },
  { "en": "Apa Itu Bit Error Rate (BER)?", "id": "Rasio Perbandingan Bit Error Bit Total." },
  { "en": "Nilai BER (Bit Error Rate) Baik?", "id": "Sangat Rendah (Mendekati Nol)." },
  { "en": "Apa Itu Block Code (Channel Coding)?", "id": "Kode Bekerja Pada Blok Data Tetap." },
  { "en": "Apa Itu Convolutional Code (Channel Coding)?", "id": "Kode Bekerja Pada Aliran Data." },
  { "en": "Apa Itu Turbo Code?", "id": "Kode Koreksi Error (Kinerja Tinggi)." },
  { "en": "Apa Itu Low-Density Parity-Check (LDPC) Code?", "id": "Kode Koreksi Error (Sangat Efisien)." },
  { "en": "Apa Kepanjangan Dari LDPC (Low-Density Parity-Check)?", "id": "Low-Density Parity-Check." },
  { "en": "Apa Itu Reed-Solomon (RS) Code?", "id": "Kode Koreksi Error (Burst Error)." },
  { "en": "Dimana RS (Reed-Solomon) Code Digunakan?", "id": "CD, DVD, QR Code." },
  { "en": "Apa Itu Interleaving (Interlasi)?", "id": "Mengacak Urutan Bit (Melawan Burst Error)." },
  { "en": "Apa Itu Burst Error?", "id": "Error Data Berkelompok (Berurutan)." },
  { "en": "Apa Itu Random Error?", "id": "Error Data Acak (Tersebar)." },
  { "en": "Apa Itu Stop-and-Wait ARQ (Automatic Repeat Request)?", "id": "Kirim Satu, Tunggu ACK (Automatic Repeat Request)." },
  { "en": "Apa Itu Go-Back-N ARQ (Automatic Repeat Request)?", "id": "Kirim Ulang Semua (Dari Yang Error)." },
  { "en": "Apa Itu Selective Repeat ARQ (Automatic Repeat Request)?", "id": "Kirim Ulang Hanya (Yang Error)." },
  { "en": "Apa Itu Sinyal (Signal)?", "id": "Besaran Fisik Pembawa Informasi." },
  { "en": "Apa Itu Sinyal Periodik?", "id": "Sinyal Berulang (Pola Tetap)." },
  { "en": "Apa Itu Sinyal Aperiodik?", "id": "Sinyal Tidak Berulang (Acak)." },
  { "en": "Apa Itu Amplitudo?", "id": "Kekuatan Atau Ketinggian Sinyal." },
  { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Sinyal Per Detik." },
  { "en": "Apa Satuan Frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Fasa (Phase)?", "id": "Posisi Sinyal Relatif Waktu." },
  { "en": "Apa Itu Panjang Gelombang?", "id": "Jarak Fisik Satu Siklus Gelombang." },
  { "en": "Apa Hubungan Frekuensi Panjang Gelombang?", "id": "Berbanding Terbalik." },
  { "en": "Apa Itu Domain Waktu (Time Domain)?", "id": "Representasi Sinyal (Amplitudo Vs Waktu)." },
  { "en": "Apa Itu Domain Frekuensi (Frequency Domain)?", "id": "Representasi Sinyal (Amplitudo Vs Frekuensi)." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Metode Mengubah Domain Waktu Frekuensi." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sinyal Asli (Frekuensi Rendah)." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sinyal (Frekuensi Tinggi, Modulasi)." },
  { "en": "Apa Itu Spektrum Sinyal?", "id": "Rentang Frekuensi Yang Ditempati Sinyal." },
  { "en": "Apa Itu Bandwidth Sinyal?", "id": "Lebar Spektrum Sinyal." },
  { "en": "Apa Itu Saluran (Channel)?", "id": "Media Transmisi Sinyal." },
  { "en": "Apa Itu Bandwidth Saluran?", "id": "Kapasitas Frekuensi Saluran." },
  { "en": "Apa Itu Distorsi (Distortion)?", "id": "Perubahan Bentuk Sinyal Asli." },
  { "en": "Apa Itu Distorsi Atenuasi?", "id": "Pelemahan Sinyal Berbeda Tiap Frekuensi." },
  { "en": "Apa Itu Distorsi Delay?", "id": "Perambatan Sinyal Berbeda Tiap Frekuensi." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Sinyal Gangguan Acak." },
  { "en": "Apa Itu Thermal Noise (Noise Termal)?", "id": "Noise Akibat Gerakan Elektron (Suhu)." },
  { "en": "Apa Itu Crosstalk?", "id": "Interferensi Sinyal Antar Kabel Berdekatan." },
  { "en": "Apa Itu Impulse Noise?", "id": "Noise Sesaat (Kekuatan Tinggi)." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Rasio Perbandingan Daya Sinyal Noise." },
  { "en": "Apa Satuan SNR (Signal-to-Noise Ratio)?", "id": "Desibel (dB)." },
  { "en": "Semakin Tinggi SNR (Signal-to-Noise Ratio) Semakin?", "id": "Baik Kualitas Sinyal." },
  { "en": "Apa Itu Throughput?", "id": "Laju Data Efektif (Aktual)." },
  { "en": "Apa Itu Model Jaringan Hierarkis?", "id": "Model Desain Jaringan (Core, Distribution, Access)." },
  { "en": "Apa Fungsi Lapis Core?", "id": "Backbone Jaringan (Switching Cepat)." },
  { "en": "Apa Fungsi Lapis Distribusi?", "id": "Agregasi, Kebijakan Routing, QoS." },
  { "en": "Apa Fungsi Lapis Akses?", "id": "Konektivitas Pengguna Akhir (Port)." },
  { "en": "Apa Itu Desain Collapsed Core?", "id": "Menggabungkan Lapis Core Dan Distribusi." },
  { "en": "Kapan Desain Collapsed Core Digunakan?", "id": "Jaringan Skala Kecil Menengah." },
  { "en": "Apa Itu Redundansi Jaringan?", "id": "Duplikasi Perangkat Atau Link Cadangan." },
  { "en": "Mengapa Redundansi Jaringan Penting?", "id": "Menjamin Ketersediaan Layanan (High Availability)." },
  { "en": "Apa Tujuan Link Aggregation (LAG)?", "id": "Meningkatkan Bandwidth Dan Redundansi." },
  { "en": "Apa Itu EtherChannel?", "id": "Nama Teknologi LAG (Link Aggregation) Cisco." },
  { "en": "Apa Itu LACP (Link Aggregation Control Protocol)?", "id": "Protokol Standar Negosiasi LAG." },
  { "en": "Apa Itu PAgP (Port Aggregation Protocol)?", "id": "Protokol Negosiasi LAG (Link Aggregation) Cisco." },
  { "en": "Apa Itu Load Balancing (LAG)?", "id": "Distribusi Trafik Di Antara Link." },
  { "en": "Apa Itu First Hop Redundancy Protocol (FHRP)?", "id": "Protokol Redundansi Gateway Default." },
  { "en": "Apa Itu HSRP (Hot Standby Router Protocol)?", "id": "Protokol FHRP (First Hop Redundancy Protocol) Cisco." },
  { "en": "Apa Itu VRRP (Virtual Router Redundancy Protocol)?", "id": "Protokol FHRP (First Hop Redundancy Protocol) Standar." },
  { "en": "Apa Itu GLBP (Gateway Load Balancing Protocol)?", "id": "Protokol FHRP (First Hop Redundancy Protocol) Cisco (Load Balance)." },
  { "en": "Apa Itu Virtual IP (FHRP)?", "id": "Alamat IP Gateway Virtual Bersama." },
  { "en": "Apa Itu Virtual MAC (FHRP)?", "id": "Alamat MAC Gateway Virtual Bersama." },
  { "en": "Apa Itu Router Aktif (HSRP)?", "id": "Router Yang Meneruskan Trafik (Utama)." },
  { "en": "Apa Itu Preemption (HSRP)?", "id": "Pengambilalihan Paksa Router Prioritas Tinggi." },
  { "en": "Apa Itu Object Tracking (FHRP)?", "id": "Melacak Status Interface (Untuk Failover)." },
  { "en": "Apa Itu Network Management (Manajemen Jaringan)?", "id": "Proses Mengelola Jaringan Komputer." },
  { "en": "Apa Itu Network Monitoring (Pemantauan Jaringan)?", "id": "Pemantauan Kinerja Status Jaringan." },
  { "en": "Apa Itu Fault Tolerance (Toleransi Kesalahan)?", "id": "Kemampuan Sistem Beroperasi Saat Gagal." },
  { "en": "Apa Itu High Availability (HA)?", "id": "Ketersediaan Layanan Yang Sangat Tinggi." },
  { "en": "Apa Kepanjangan Dari DR (Disaster Recovery)?", "id": "Disaster Recovery." },
  { "en": "Apa Itu Business Continuity Plan (BCP)?", "id": "Rencana Keberlangsungan Bisnis (Saat Bencana)." },
  { "en": "Apa Itu Hot Site (DR)?", "id": "Situs Pemulihan Siap Pakai (Data Sinkron)." },
  { "en": "Apa Itu Cold Site (DR)?", "id": "Situs Pemulihan (Fasilitas Dasar Saja)." },
  { "en": "Apa Itu Warm Site (DR)?", "id": "Situs Pemulihan (Sebagian Siap Pakai)." },
  { "en": "Apa Itu Recovery Time Objective (RTO)?", "id": "Target Waktu Maksimal Pemulihan Layanan." },
  { "en": "Apa Itu Recovery Point Objective (RPO)?", "id": "Target Kehilangan Data Maksimal (Toleransi)." },
  { "en": "Apa Itu Backup (Cadangan)?", "id": "Proses Penyalinan Data Cadangan." },
  { "en": "Apa Itu Full Backup?", "id": "Menyalin Seluruh Data." },
  { "en": "Apa Itu Incremental Backup?", "id": "Menyalin Data (Berubah Sejak Backup Terakhir)." },
  { "en": "Apa Itu Differential Backup?", "id": "Menyalin Data (Berubah Sejak Full Terakhir)." },
  { "en": "Apa Itu Aturan Backup 3-2-1?", "id": "Tiga Salinan, Dua Media, Satu Offsite." },
  { "en": "Apa Itu Snapshot (Virtualisasi)?", "id": "Gambaran Status Sistem Sesaat." },
  { "en": "Apa Itu Service Level Agreement (SLA)?", "id": "Perjanjian Kontrak Kualitas Layanan." },
  { "en": "Apa Itu Uptime (Waktu Hidup)?", "id": "Waktu Sistem Beroperasi Normal." },
  { "en": "Apa Itu Downtime (Waktu Mati)?", "id": "Waktu Sistem Gagal Beroperasi." },
  { "en": "Apa Itu Ketersediaan 'Five Nines'?", "id": "Ketersediaan 99.999 Persen." },
  { "en": "Apa Itu Mean Time Between Failures (MTBF)?", "id": "Rata-Rata Waktu Antar Kegagalan." },
  { "en": "Apa Itu Network Troubleshooting?", "id": "Proses Diagnosis Masalah Jaringan." },
  { "en": "Apa Itu Perintah 'Traceroute' (Atau 'Tracert')?", "id": "Melacak Rute Paket Ke Tujuan." },
  { "en": "Apa Itu Perintah 'Ifconfig'?", "id": "Menampilkan Konfigurasi IP (Linux/Unix)." },
  { "en": "Apa Itu Perintah 'Dig' (Domain Information Groper)?", "id": "Alat Kueri DNS (Domain Name System) (Linux)." },
  { "en": "Apa Itu Perintah 'Netstat'?", "id": "Menampilkan Koneksi Jaringan Aktif." },
  { "en": "Apa Itu Perintah 'Arp'?", "id": "Menampilkan Cache ARP (Address Resolution Protocol)." },
  { "en": "Apa Itu Wireshark?", "id": "Alat Penganalisis Protokol Jaringan (Sniffer)." },
  { "en": "Apa Itu Packet Sniffing?", "id": "Proses Menangkap Menganalisis Paket." },
  { "en": "Apa Itu Port Scanning?", "id": "Proses Memindai Port Terbuka." },
  { "en": "Apa Itu MTR (My Traceroute)?", "id": "Gabungan Perintah Ping Dan Traceroute." },
  { "en": "Apa Itu Iperf?", "id": "Alat Pengukur Throughput Jaringan." },
  { "en": "Apa Itu Log Jaringan?", "id": "Catatan Peristiwa Aktivitas Jaringan." },
  { "en": "Apa Itu Syslog Server?", "id": "Server Penerima Pesan Log Terpusat." },
  { "en": "Apa Itu Level Severity (Syslog)?", "id": "Tingkat Keparahan Pesan (0-7)." },
  { "en": "Level 0 (Syslog)?", "id": "Emergency." },
  { "en": "Level 7 (Syslog)?", "id": "Debug." },
  { "en": "Apa Itu NetFlow?", "id": "Fitur Cisco Monitoring Aliran Trafik." },
  { "en": "Apa Itu IPFIX (IP Flow Information Export)?", "id": "Standar IETF (Internet Engineering Task Force) Flow." },
  { "en": "Beda NetFlow Dan sFlow?", "id": "NetFlow Berbasis Aliran, sFlow Sampling." },
  { "en": "Apa Itu RSPAN (Remote SPAN)?", "id": "SPAN (Switched Port Analyzer) Antar Switch." },
  { "en": "Apa Itu ERSPAN (Encapsulated Remote SPAN)?", "id": "RSPAN (Remote SPAN) (Enkapsulasi GRE)." },
  { "en": "Apa Itu Network TAP (Test Access Point)?", "id": "Perangkat Keras Menyalin Trafik Jaringan." },
  { "en": "Apa Kepanjangan Dari TAP (Test Access Point)?", "id": "Test Access Point." },
  { "en": "Beda TAP (Test Access Point) Dan SPAN (Switched Port Analyzer)?", "id": "TAP Hardware, SPAN Fitur Switch." },
  { "en": "Apa Itu IP SLA (Service Level Agreement)?", "id": "Fitur Cisco (Monitoring Kinerja Aktif)." },
  { "en": "Apa Itu Serat Optik (Fiber Optic)?", "id": "Media Transmisi (Kaca/Plastik) Cahaya." },
  { "en": "Apa Itu Inti (Core) Serat Optik?", "id": "Bagian Tengah Serat (Jalur Cahaya)." },
  { "en": "Apa Itu Total Internal Reflection?", "id": "Prinsip Pantulan Cahaya (Serat Optik)." },
  { "en": "Apa Itu Serat Optik Single-Mode (SMF)?", "id": "Serat (Inti Kecil, Jarak Jauh)." },
  { "en": "Apa Itu Serat Optik Multi-Mode (MMF)?", "id": "Serat (Inti Besar, Jarak Pendek)." },
  { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Pelemahan Kekuatan Sinyal." },
  { "en": "Apa Penyebab Atenuasi?", "id": "Jarak Tempuh, Hambatan Media." },
  { "en": "Apa Itu Amplifier (Penguat)?", "id": "Perangkat Menguatkan Sinyal (Termasuk Noise)." },
  { "en": "Apa Itu Repeater (Pengulang)?", "id": "Perangkat Regenerasi Sinyal (Tanpa Noise)." },
  { "en": "Apa Beda Amplifier Dan Repeater?", "id": "Repeater Membersihkan Noise, Amplifier Tidak." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Gangguan Acak Pada Sinyal Transmisi." },
  { "en": "Apa Itu Thermal Noise?", "id": "Noise Akibat Gerakan Elektron (Panas)." },
  { "en": "Apa Itu Impulse Noise?", "id": "Noise Sesaat (Intensitas Tinggi, Petir)." },
  { "en": "Apa Itu Noise Figure (NF)?", "id": "Ukuran Degradasi SNR (Akibat Perangkat)." },
  { "en": "Apa Kepanjangan Dari NF (Noise Figure)?", "id": "Noise Figure." },
  { "en": "Apa Itu Attenuation Distortion?", "id": "Atenuasi Berbeda Tiap Frekuensi." },
  { "en": "Apa Itu Delay Distortion?", "id": "Waktu Tunda Berbeda Tiap Frekuensi." },
  { "en": "Apa Itu Data Rate (Laju Data)?", "id": "Kecepatan Transfer Data (Bit Per Detik)." },
  { "en": "Apa Itu Channel Capacity (Kapasitas Saluran)?", "id": "Data Rate Maksimal Suatu Saluran." },
  { "en": "Apa Itu Teorema Kapasitas Shannon?", "id": "Rumus Menghitung Kapasitas Saluran (Ideal)." },
  { "en": "Faktor Apa Mempengaruhi Kapasitas Shannon?", "id": "Bandwidth Dan SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Menentukan Laju Sampling Minimum." },
  { "en": "Berapa Laju Sampling Nyquist?", "id": "Dua Kali Frekuensi Tertinggi Sinyal." },
  { "en": "Apa Itu Aliasing (Sinyal)?", "id": "Error Akibat Laju Sampling Rendah." },
  { "en": "Apa Itu Pulse Code Modulation (PCM)?", "id": "Metode Konversi Analog Ke Digital." },
  { "en": "Apa Langkah-Langkah PCM (Pulse Code Modulation)?", "id": "Sampling, Quantizing, Encoding." },
  { "en": "Apa Itu Quantization Error?", "id": "Selisih Nilai Asli Dan Pembulatan." },
  { "en": "Apa Itu Encoding (PCM)?", "id": "Mengubah Level Ke Kode Biner." },
  { "en": "Apa Itu Delta Modulation (DM)?", "id": "Metode PCM (Pulse Code Modulation) Sederhana." },
  { "en": "Apa Itu Adaptive Delta Modulation (ADM)?", "id": "Delta Modulation (Ukuran Langkah Adaptif)." },
  { "en": "Apa Itu Differential PCM (DPCM)?", "id": "PCM (Pulse Code Modulation) (Encoding Perbedaan)." },
  { "en": "Apa Kepanjangan Dari DPCM (Differential PCM)?", "id": "Differential Pulse Code Modulation." },
  { "en": "Apa Itu Frequency Division Multiplexing (FDM)?", "id": "Multiplexing Berbasis Frekuensi." },
  { "en": "Apa Itu Time Division Multiplexing (TDM)?", "id": "Multiplexing Berbasis Waktu." },
  { "en": "Apa Itu Asynchronous TDM (Time Division Multiplexing)?", "id": "TDM (Time Division Multiplexing) Slot Waktu Dinamis." },
  { "en": "Nama Lain Asynchronous TDM (Time Division Multiplexing)?", "id": "Statistical TDM (Time Division Multiplexing)." },
  { "en": "Apa Itu Guard Band (FDM)?", "id": "Pita Frekuensi Pemisah (Mencegah Interferensi)." },
  { "en": "Apa Itu Guard Time (TDM)?", "id": "Waktu Jeda Pemisah (TDM)." },
  { "en": "Apa Itu Digital Subscriber Line (DSL)?", "id": "Internet Kecepatan Tinggi (Saluran Telepon)." },
  { "en": "Apa Itu Asymmetric DSL (ADSL)?", "id": "DSL (Kecepatan Unduh Unggah Beda)." },
  { "en": "Apa Kepanjangan Dari ADSL (Asymmetric DSL)?", "id": "Asymmetric Digital Subscriber Line." },
  { "en": "Mengapa ADSL (Asymmetric DSL) Asimetris?", "id": "Unduh Lebih Cepat Daripada Unggah." },
  { "en": "Apa Itu Symmetric DSL (SDSL)?", "id": "DSL (Kecepatan Unduh Unggah Sama)." },
  { "en": "Apa Kepanjangan Dari SDSL (Symmetric DSL)?", "id": "Symmetric Digital Subscriber Line." },
  { "en": "Apa Itu DSLAM (DSL Access Multiplexer)?", "id": "Perangkat Sentral (Multiplexer DSL)." },
  { "en": "Apa Kepanjangan Dari DSLAM (DSL Access Multiplexer)?", "id": "Digital Subscriber Line Access Multiplexer." },
  { "en": "Apa Itu DSL (Digital Subscriber Line) Splitter?", "id": "Pemisah Sinyal Suara Data (Telepon)." },
  { "en": "Apa Itu Kabel Modem?", "id": "Modem Internet (Jaringan TV Kabel)." },
  { "en": "Apa Itu CMTS (Cable Modem Termination System)?", "id": "Perangkat Sentral (Provider TV Kabel)." },
  { "en": "Apa Kepanjangan Dari CMTS (Cable Modem Termination System)?", "id": "Cable Modem Termination System." },
  { "en": "Apa Itu HFC (Hybrid Fiber-Coaxial)?", "id": "Jaringan (Gabungan Serat Optik Koaksial)." },
  { "en": "Apa Kepanjangan Dari HFC (Hybrid Fiber-Coaxial)?", "id": "Hybrid Fiber-Coaxial." },
  { "en": "Apa Itu DOCSIS (Data Over Cable Service Interface Specification)?", "id": "Standar Transmisi Data TV Kabel." },
  { "en": "Apa Kepanjangan Dari DOCSIS (Data Over Cable Service Interface Specification)?", "id": "Data Over Cable Service Interface Specification." },
  { "en": "Apa Itu Fiber to the Home (FTTH)?", "id": "Serat Optik Sampai Ke Rumah." },
  { "en": "Apa Itu Fiber to the Building (FTTB)?", "id": "Serat Optik Sampai Ke Gedung." },
  { "en": "Apa Itu Fiber to the Curb (FTTC)?", "id": "Serat Optik Sampai Tepi Jalan." },
  { "en": "Apa Itu Fiber to the Node (FTTN)?", "id": "Serat Optik Sampai Ke Node." },
  { "en": "Apa Itu FTTX (Fiber to the X)?", "id": "Istilah Umum Jaringan Akses Fiber." },
  { "en": "Apa Itu Passive Optical Network (PON)?", "id": "Jaringan Serat Optik Pasif." },
  { "en": "Apa Itu Optical Line Terminal (OLT)?", "id": "Perangkat Pusat Jaringan PON." },
  { "en": "Apa Itu Optical Network Terminal (ONT)?", "id": "Perangkat Sisi Pelanggan (PON)." },
  { "en": "Apa Itu Optical Network Unit (ONU)?", "id": "Perangkat Sisi Pelanggan (Umum)." },
  { "en": "Apa Itu Optical Splitter?", "id": "Perangkat Pasif Pembagi Sinyal Optik." },
  { "en": "Apa Itu GPON (Gigabit PON)?", "id": "Standar PON (Passive Optical Network) Kecepatan Gigabit." },
  { "en": "Apa Kepanjangan Dari GPON (Gigabit PON)?", "id": "Gigabit Passive Optical Network." },
  { "en": "Apa Itu EPON (Ethernet PON)?", "id": "Standar PON (Passive Optical Network) Berbasis Ethernet." },
  { "en": "Apa Kepanjangan Dari EPON (Ethernet PON)?", "id": "Ethernet Passive Optical Network." },
  { "en": "Apa Itu Active Optical Network (AON)?", "id": "Jaringan Optik (Menggunakan Perangkat Aktif)." },
  { "en": "Apa Kepanjangan Dari AON (Active Optical Network)?", "id": "Active Optical Network." },
  { "en": "Apa Beda PON (Passive Optical Network) Dan AON (Active Optical Network)?", "id": "PON Pasif, AON Aktif (Switch)." },
  { "en": "Apa Itu Protokol (Jaringan)?", "id": "Aturan Formal Komunikasi Jaringan." },
  { "en": "Apa Itu Standar De Jure?", "id": "Standar Resmi (Ditetapkan Badan Standar)." },
  { "en": "Apa Itu Standar De Facto?", "id": "Standar (Diterima Karena Penggunaan Luas)." },
  { "en": "Apa Itu IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Badan Standarisasi (Teknik Elektro)." },
  { "en": "Apa Itu IETF (Internet Engineering Task Force)?", "id": "Badan Standarisasi Protokol Internet." },
  { "en": "Apa Itu ITU (International Telecommunication Union)?", "id": "Badan Standarisasi Telekomunikasi PBB." },
  { "en": "Apa Itu ISO (International Organization for Standardization)?", "id": "Organisasi Standarisasi Internasional (OSI)." },
  { "en": "Apa Itu ANSI (American National Standards Institute)?", "id": "Badan Standarisasi Nasional Amerika." },
  { "en": "Apa Itu EIA (Electronic Industries Alliance)?", "id": "Asosiasi Standarisasi Elektronik (AS)." },
  { "en": "Apa Itu TIA (Telecommunications Industry Association)?", "id": "Asosiasi Standarisasi Telekomunikasi (AS)." },
  { "en": "Apa Itu Request for Comments (RFC)?", "id": "Dokumen Publikasi Standar Internet (IETF)." },
  { "en": "Apa Itu Komunikasi Optik?", "id": "Komunikasi Menggunakan Sinyal Cahaya." },
  { "en": "Apa Itu Serat Optik?", "id": "Media Transmisi (Kaca) Pembawa Cahaya." },
  { "en": "Apa Komponen Utama Serat Optik?", "id": "Inti (Core), Cladding, Buffer." },
  { "en": "Apa Fungsi Inti (Core) Serat Optik?", "id": "Tempat Merambatnya Sinyal Cahaya." },
  { "en": "Apa Fungsi Cladding Serat Optik?", "id": "Memantulkan Cahaya Kembali Ke Inti." },
  { "en": "Apa Itu Total Internal Reflection?", "id": "Prinsip Pantulan Cahaya Dalam Serat." },
  { "en": "Apa Itu Indeks Bias?", "id": "Ukuran Pembiasan Cahaya Suatu Medium." },
  { "en": "Indeks Bias Mana Lebih Tinggi (Serat)?", "id": "Inti (Core) Lebih Tinggi Dari Cladding." },
  { "en": "Apa Itu Serat Optik Single-Mode?", "id": "Serat (Inti Sangat Kecil, Satu Jalur)." },
  { "en": "Apa Keuntungan SMF (Single-Mode Fiber)?", "id": "Jarak Sangat Jauh, Bandwidth Besar." },
  { "en": "Apa Itu Serat Optik Multi-Mode?", "id": "Serat (Inti Lebih Besar, Banyak Jalur)." },
  { "en": "Apa Kerugian MMF (Multi-Mode Fiber)?", "id": "Jarak Pendek, Dispersi Modal." },
  { "en": "Apa Itu Dispersi (Serat Optik)?", "id": "Pelebaran Pulsa Sinyal Cahaya." },
  { "en": "Apa Itu Dispersi Modal?", "id": "Dispersi Akibat Beda Jalur (MMF)." },
  { "en": "Apa Itu Dispersi Kromatik?", "id": "Dispersi Akibat Beda Kecepatan Warna." },
  { "en": "Apa Itu Dispersi Polarisasi Mode (PMD)?", "id": "Dispersi Akibat Beda Polarisasi." },
  { "en": "Apa Itu Atenuasi (Serat Optik)?", "id": "Pelemahan Sinyal Cahaya Dalam Serat." },
  { "en": "Apa Satuan Atenuasi Optik?", "id": "Desibel Per Kilometer (dB/Km)." },
  { "en": "Apa Itu Jendela Transmisi Optik?", "id": "Panjang Gelombang (Atenuasi Terendah)." },
  { "en": "Panjang Gelombang Jendela Pertama?", "id": "850 Nanometer (nm) (MMF)." },
  { "en": "Panjang Gelombang Jendela Kedua?", "id": "1310 Nanometer (nm) (SMF)." },
  { "en": "Panjang Gelombang Jendela Ketiga?", "id": "1550 Nanometer (nm) (SMF, Terendah)." },
  { "en": "Apa Itu Konektor Serat Optik?", "id": "Penyambung Ujung Kabel Serat Optik." },
  { "en": "Contoh Konektor Serat Optik?", "id": "SC, LC, ST, FC." },
  { "en": "Apa Itu Konektor SC (Subscriber Connector)?", "id": "Konektor (Kotak, Dorong-Tarik)." },
  { "en": "Apa Itu Konektor LC (Lucent Connector)?", "id": "Konektor (Kecil, Mirip RJ45)." },
  { "en": "Apa Itu Konektor ST (Straight Tip)?", "id": "Konektor (Bulat, Putar-Kunci)." },
  { "en": "Apa Itu Splicing (Serat Optik)?", "id": "Proses Penyambungan Inti Serat Optik." },
  { "en": "Apa Itu Fusion Splicing?", "id": "Penyambungan Serat (Peleburan Panas)." },
  { "en": "Apa Itu Mechanical Splicing?", "id": "Penyambungan Serat (Mekanis, Jepit)." },
  { "en": "Apa Itu Pigtail (Serat Optik)?", "id": "Kabel (Satu Sisi Konektor)." },
  { "en": "Apa Itu Patch Cord (Serat Optik)?", "id": "Kabel (Dua Sisi Konektor)." },
  { "en": "Apa Itu Optical Light Source (OLS)?", "id": "Sumber Cahaya (Untuk Pengujian OPM)." },
  { "en": "Apa Itu Optical Time-Domain Reflectometer (OTDR)?", "id": "Alat Uji (Lokasi Putus, Redaman)." },
  { "en": "Apa Itu Visual Fault Locator (VFL)?", "id": "Laser Merah (Mencari Patahan Visual)." },
  { "en": "Apa Itu Passive Optical Network (PON)?", "id": "Jaringan Optik (Titik Ke Multi-Titik)." },
  { "en": "Mengapa Disebut Pasif (PON)?", "id": "Menggunakan Splitter Pasif (Tanpa Listrik)." },
  { "en": "Apa Itu Optical Line Terminal (OLT)?", "id": "Perangkat Pusat (Provider) Jaringan PON." },
  { "en": "Apa Itu Optical Network Terminal (ONT)?", "id": "Perangkat Akhir (Pelanggan) Jaringan PON." },
  { "en": "Apa Itu Optical Splitter?", "id": "Perangkat Pasif (Membagi Sinyal Optik)." },
  { "en": "Apa Itu Sinyal Downstream (PON)?", "id": "Sinyal Dari OLT (Optical Line Terminal) ONT." },
  { "en": "Apa Itu Sinyal Upstream (PON)?", "id": "Sinyal Dari ONT (Optical Network Terminal) OLT." },
  { "en": "Bagaimana Upstream PON (Passive Optical Network) Bekerja?", "id": "Menggunakan TDMA (Time Division Multiple Access)." },
  { "en": "Apa Itu GPON (Gigabit PON)?", "id": "Standar PON (Passive Optical Network) (ITU-T G.984)." },
  { "en": "Apa Itu EPON (Ethernet PON)?", "id": "Standar PON (Passive Optical Network) (IEEE 802.3ah)." },
  { "en": "Apa Itu Wavelength Division Multiplexing (WDM)?", "id": "Multiplexing (Berbasis Panjang Gelombang)." },
  { "en": "Apa Itu Coarse WDM (CWDM)?", "id": "WDM (Wavelength Division Multiplexing) (Jarak Saluran Lebar)." },
  { "en": "Apa Kepanjangan Dari CWDM (Coarse WDM)?", "id": "Coarse Wavelength Division Multiplexing." },
  { "en": "Apa Itu Dense WDM (DWDM)?", "id": "WDM (Wavelength Division Multiplexing) (Jarak Saluran Rapat)." },
  { "en": "Apa Kepanjangan Dari DWDM (Dense WDM)?", "id": "Dense Wavelength Division Multiplexing." },
  { "en": "Mana Kapasitas Lebih Besar (WDM)?", "id": "DWDM (Dense Wavelength Division Multiplexing)." },
  { "en": "Apa Itu Optical Amplifier?", "id": "Penguat Sinyal Optik (Tanpa Konversi)." },
  { "en": "Apa Itu EDFA (Erbium-Doped Fiber Amplifier)?", "id": "Penguat Optik (Menggunakan Serat Erbium)." },
  { "en": "Apa Itu Raman Amplifier?", "id": "Penguat Optik (Menggunakan Efek Raman)." },
  { "en": "Apa Itu Optical Add-Drop Multiplexer (OADM)?", "id": "Perangkat (Menambah/Mengambil Saluran WDM)." },
  { "en": "Apa Itu Reconfigurable OADM (ROADM)?", "id": "OADM (Optical Add-Drop Multiplexer) (Dapat Dikonfigurasi)." },
  { "en": "Apa Kepanjangan Dari ROADM (Reconfigurable OADM)?", "id": "Reconfigurable Optical Add-Drop Multiplexer." },
  { "en": "Apa Itu SONET (Synchronous Optical Networking)?", "id": "Standar Transport Optik (Amerika Utara)." },
  { "en": "Apa Itu SDH (Synchronous Digital Hierarchy)?", "id": "Standar Transport Optik (Internasional/ITU)." },
  { "en": "Apa Itu Optical Carrier (OC) Level?", "id": "Penanda Kecepatan Sinyal SONET." },
  { "en": "Berapa Kecepatan OC-1 (Optical Carrier 1)?", "id": "51.84 Mbps." },
  { "en": "Berapa Kecepatan OC-3 (Optical Carrier 3)?", "id": "155.52 Mbps." },
  { "en": "Berapa Kecepatan OC-12 (Optical Carrier 12)?", "id": "622.08 Mbps." },
  { "en": "Berapa Kecepatan OC-48 (Optical Carrier 48)?", "id": "2.488 Gbps." },
  { "en": "Berapa Kecepatan OC-192 (Optical Carrier 192)?", "id": "9.953 Gbps." },
  { "en": "Apa Itu Synchronous Transport Module (STM)?", "id": "Penanda Kecepatan Sinyal SDH." },
  { "en": "Apa Itu STM-1 (Synchronous Transport Module 1)?", "id": "Setara OC-3 (Optical Carrier 3) (155.52 Mbps)." },
  { "en": "Apa Itu STM-4 (Synchronous Transport Module 4)?", "id": "Setara OC-12 (Optical Carrier 12) (622.08 Mbps)." },
  { "en": "Apa Itu STM-16 (Synchronous Transport Module 16)?", "id": "Setara OC-48 (Optical Carrier 48) (2.488 Gbps)." },
  { "en": "Apa Itu STM-64 (Synchronous Transport Module 64)?", "id": "Setara OC-192 (Optical Carrier 192) (9.953 Gbps)." },
  { "en": "Apa Itu Optical Transport Network (OTN)?", "id": "Standar Transport Optik (Pengganti SONET/SDH)." },
  { "en": "Apa Itu Client Signal (OTN)?", "id": "Sinyal Data (Ethernet, SDH) Dibawa OTN." },
  { "en": "Apa Itu Antena?", "id": "Perangkat Pengubah Sinyal Listrik Radio." },
  { "en": "Apa Fungsi Antena (Pemancar)?", "id": "Mengubah Listrik Menjadi Gelombang Radio." },
  { "en": "Apa Fungsi Antena (Penerima)?", "id": "Mengubah Gelombang Radio Menjadi Listrik." },
  { "en": "Apa Itu Radiasi (Antena)?", "id": "Pemancaran Energi Elektromagnetik." },
  { "en": "Apa Itu Pola Radiasi (Antena)?", "id": "Pola Arah Pemancaran Sinyal Antena." },
  { "en": "Apa Itu Antena Isotropik?", "id": "Antena Ideal (Memancar Sama Rata)." },
  { "en": "Apa Itu Antena Omni-Directional?", "id": "Antena Memancar (Satu Bidang Horizontal)." },
  { "en": "Contoh Antena Omni-Directional?", "id": "Antena Wi-Fi (Router Rumahan)." },
  { "en": "Apa Itu Antena Directional?", "id": "Antena Memancar (Fokus Satu Arah)." },
  { "en": "Contoh Antena Directional?", "id": "Antena Parabola, Antena Yagi." },
  { "en": "Apa Itu Antena Parabola?", "id": "Antena (Reflektor Bentuk Piringan)." },
  { "en": "Apa Itu Antena Yagi?", "id": "Antena Directional (Banyak Elemen)." },
  { "en": "Apa Itu Gain (Antena)?", "id": "Kemampuan Antena Memfokuskan Sinyal." },
  { "en": "Apa Satuan Gain (Antena)?", "id": "Desibel (dBi Atau dBd)." },
  { "en": "Apa Itu dBi (Decibels-isotropic)?", "id": "Gain (Antena) Dibanding Antena Isotropik." },
  { "en": "Apa Itu dBd (Decibels-dipole)?", "id": "Gain (Antena) Dibanding Antena Dipole." },
  { "en": "Apa Itu Polarisasi (Antena)?", "id": "Orientasi Arah Gelombang Elektromagnetik." },
  { "en": "Apa Itu Polarisasi Vertikal?", "id": "Orientasi Gelombang (Vertikal)." },
  { "en": "Apa Itu Polarisasi Horizontal?", "id": "Orientasi Gelombang (Horizontal)." },
  { "en": "Apa Itu Polarisasi Circular?", "id": "Orientasi Gelombang (Berputar)." },
  { "en": "Apa Itu Impedansi (Antena)?", "id": "Hambatan Antena Pada Frekuensi." },
  { "en": "Berapa Impedansi Umum Antena?", "id": "50 Ohm Atau 75 Ohm." },
  { "en": "Apa Itu Standing Wave Ratio (SWR)?", "id": "Rasio Gelombang Berdiri (Kecocokan)." },
  { "en": "Berapa Nilai SWR (Standing Wave Ratio) Ideal?", "id": "1:1 (Satu Banding Satu)." },
  { "en": "Apa Itu Return Loss?", "id": "Ukuran Daya Yang Dipantulkan Balik." },
  { "en": "Apa Itu Bandwidth (Antena)?", "id": "Rentang Frekuensi Kerja Efektif Antena." },
  { "en": "Apa Itu Beamwidth (Antena)?", "id": "Lebar Sudut Pancaran Utama Antena." },
  { "en": "Apa Itu Front-to-Back Ratio (F/B Ratio)?", "id": "Rasio Pancaran Depan Belakang (Directional)." },
  { "en": "Apa Itu Antena Monopole?", "id": "Antena (Satu Elemen, Ground Plane)." },
  { "en": "Apa Itu Antena Array?", "id": "Gabungan Beberapa Elemen Antena." },
  { "en": "Apa Itu Antena Patch?", "id": "Antena Datar (Microstrip, Di PCB)." },
  { "en": "Dimana Antena Patch Digunakan?", "id": "Ponsel, GPS (Global Positioning System)." },
  { "en": "Apa Itu Smart Antenna (Antena Pintar)?", "id": "Antena (Mengatur Pola Radiasi Otomatis)." },
  { "en": "Apa Itu Adaptive Array Antenna?", "id": "Antena (Mengarahkan Sinyal, Mengurangi Interferensi)." },
  { "en": "Apa Itu Phased Array Antenna?", "id": "Antena Array (Arah Diatur Fasa)." },
  { "en": "Apa Itu Waveguide (Pemandu Gelombang)?", "id": "Saluran Logam (Transmisi Frekuensi Tinggi)." },
  { "en": "Dimana Waveguide Digunakan?", "id": "Microwave, Radar." },
  { "en": "Apa Itu Saluran Transmisi?", "id": "Media Penghubung Pemancar Penerima." },
  { "en": "Contoh Saluran Transmisi?", "id": "Kabel Koaksial, Waveguide, Twisted Pair." },
  { "en": "Apa Itu Impedance Matching?", "id": "Menyamakan Impedansi (Transfer Daya Maksimal)." },
  { "en": "Apa Akibat Impedance Mismatch?", "id": "Sinyal Dipantulkan (SWR Tinggi)." },
  { "en": "Apa Itu Balun?", "id": "Mengubah Saluran Balanced Ke Unbalanced." },
  { "en": "Apa Kepanjangan Dari Balun (Balanced-Unbalanced)?", "id": "Balanced-Unbalanced." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel (Konduktor Pusat, Pelindung Logam)." },
  { "en": "Apa Itu Propagasi Gelombang Tanah (Ground Wave)?", "id": "Merambat Mengikuti Permukaan Bumi (Frekuensi Rendah)." },
  { "en": "Apa Itu Propagasi Gelombang Langit (Sky Wave)?", "id": "Merambat (Dipantulkan Lapisan Ionosfer)." },
  { "en": "Frekuensi Apa Menggunakan Sky Wave?", "id": "Frekuensi Tinggi (HF, 3-30 MHz)." },
  { "en": "Apa Itu Propagasi Garis Pandang (Line-of-Sight)?", "id": "Merambat Lurus (Frekuensi Tinggi)." },
  { "en": "Apa Kepanjangan Dari LOS (Line-of-Sight)?", "id": "Line-of-Sight." },
  { "en": "Apa Itu Fresnel Zone?", "id": "Area Elips Sekitar Jalur LOS." },
  { "en": "Mengapa Fresnel Zone Harus Bebas?", "id": "Menghindari Pantulan Merusak Sinyal." },
  { "en": "Apa Itu Fading?", "id": "Fluktuasi Kekuatan Sinyal." },
  { "en": "Apa Itu Shadowing?", "id": "Pelemahan Sinyal Akibat Hambatan Besar." },
  { "en": "Apa Itu Refleksi (Gelombang)?", "id": "Pemantulan Gelombang (Mengenai Permukaan)." },
  { "en": "Apa Itu Refraksi (Gelombang)?", "id": "Pembelokan Gelombang (Perubahan Medium)." },
  { "en": "Apa Itu Difraksi (Gelombang)?", "id": "Pembelokan Gelombang (Tepi Hambatan)." },
  { "en": "Apa Itu Scattering (Hamburan)?", "id": "Penyebaran Gelombang (Objek Kecil)." },
  { "en": "Apa Itu Atmosfer?", "id": "Lapisan Gas Pelindung Bumi." },
  { "en": "Apa Itu Troposfer?", "id": "Lapisan Atmosfer Terbawah (Cuaca)." },
  { "en": "Apa Itu Stratosfer?", "id": "Lapisan Atmosfer (Lapisan Ozon)." },
  { "en": "Apa Itu Ionosfer?", "id": "Lapisan Atmosfer (Bermuatan Ion)." },
  { "en": "Apa Fungsi Ionosfer (Telekomunikasi)?", "id": "Memantulkan Gelombang Radio HF." },
  { "en": "Apa Itu Microwave Terrestrial?", "id": "Komunikasi Microwave (Antar Menara Bumi)." },
  { "en": "Apa Syarat Microwave Terrestrial?", "id": "Harus Line-of-Sight (LOS)." },
  { "en": "Apa Itu Komunikasi Satelit?", "id": "Komunikasi Menggunakan Satelit Buatan." },
  { "en": "Apa Itu Satelit?", "id": "Benda Mengorbit Benda Lain." },
  { "en": "Apa Itu Orbit?", "id": "Jalur Lintasan Benda Mengelilingi." },
  { "en": "Apa Itu Satelit Geostasioner (GEO)?", "id": "Satelit (Tetap Di Atas Ekuator)." },
  { "en": "Ketinggian Orbit GEO (Geostationary Earth Orbit)?", "id": "Sekitar 35.786 Kilometer." },
  { "en": "Apa Keuntungan GEO (Geostationary Earth Orbit)?", "id": "Cakupan Luas, Posisi Tetap." },
  { "en": "Apa Kerugian GEO (Geostationary Earth Orbit)?", "id": "Latensi (Delay) Sangat Tinggi." },
  { "en": "Apa Itu Satelit Low Earth Orbit (LEO)?", "id": "Satelit (Orbit Rendah, Bergerak Cepat)." },
  { "en": "Ketinggian Orbit LEO (Low Earth Orbit)?", "id": "Di Bawah 2.000 Kilometer." },
  { "en": "Apa Keuntungan LEO (Low Earth Orbit)?", "id": "Latensi (Delay) Sangat Rendah." },
  { "en": "Apa Kerugian LEO (Low Earth Orbit)?", "id": "Cakupan Sempit, Butuh Banyak Satelit." },
  { "en": "Apa Itu Konstelasi Satelit LEO (Low Earth Orbit)?", "id": "Jaringan Satelit LEO (Low Earth Orbit)." },
  { "en": "Contoh Konstelasi LEO (Low Earth Orbit)?", "id": "Starlink, Iridium, OneWeb." },
  { "en": "Apa Itu Satelit Medium Earth Orbit (MEO)?", "id": "Satelit (Antara LEO Dan GEO)." },
  { "en": "Contoh Jaringan MEO (Medium Earth Orbit)?", "id": "GPS (Global Positioning System), Galileo." },
  { "en": "Apa Itu Uplink (Satelit)?", "id": "Transmisi Sinyal (Bumi Ke Satelit)." },
  { "en": "Apa Itu Downlink (Satelit)?", "id": "Transmisi Sinyal (Satelit Ke Bumi)." },
  { "en": "Frekuensi Uplink Dan Downlink (Sama/Beda)?", "id": "Selalu Berbeda (Menghindari Interferensi)." },
  { "en": "Apa Itu Transponder (Satelit)?", "id": "Penerima Penguat Pemancar Ulang." },
  { "en": "Apa Fungsi Transponder?", "id": "Menerima Uplink, Mengirim Downlink." },
  { "en": "Apa Itu Spot Beam (Satelit)?", "id": "Sinyal Satelit (Terfokus Area Kecil)." },
  { "en": "Dimana VSAT (Very Small Aperture Terminal) Digunakan?", "id": "Area Terpencil (Bank, ATM)." },
  { "en": "Apa Itu Topologi Star (VSAT)?", "id": "VSAT (Very Small Aperture Terminal) Terhubung Ke Hub." },
  { "en": "Apa Itu Topologi Mesh (VSAT)?", "id": "VSAT (Very Small Aperture Terminal) Saling Terhubung." },
  { "en": "Apa Itu Latensi (Delay) Satelit?", "id": "Waktu Tunda Sinyal (Akibat Jarak)." },
  { "en": "Satelit Apa Latensi Tertinggi?", "id": "GEO (Geostationary Earth Orbit)." },
  { "en": "Apa Itu Rain Fade?", "id": "Pelemahan Sinyal Satelit Akibat Hujan." },
  { "en": "Apa Itu Pita C (C-Band)?", "id": "Frekuensi Satelit (4-8 GHz, Tahan Hujan)." },
  { "en": "Apa Itu Pita Ku (Ku-Band)?", "id": "Frekuensi Satelit (12-18 GHz)." },
  { "en": "Apa Itu Pita Ka (Ka-Band)?", "id": "Frekuensi Satelit (26.5-40 GHz)." },
  { "en": "Apa Itu Sun Outage?", "id": "Gangguan Sinyal (Satelit Sejajar Matahari)." },
  { "en": "Apa Itu Global Positioning System (GPS)?", "id": "Sistem Navigasi Global (Berbasis Satelit)." },
  { "en": "Apa Kepanjangan Dari GPS (Global Positioning System)?", "id": "Global Positioning System." },
  { "en": "Jenis Orbit Satelit GPS (Global Positioning System)?", "id": "MEO (Medium Earth Orbit)." },
  { "en": "Apa Itu Trilateration (GPS)?", "id": "Metode Penentuan Posisi (Tiga Satelit)." },
  { "en": "Apa Itu Sektor ITU-R (Radiocommunication)?", "id": "Sektor ITU (International Telecommunication Union) (Regulasi Radio)." },
  { "en": "Apa Itu Sektor ITU-T (Telecommunication)?", "id": "Sektor ITU (International Telecommunication Union) (Standar Teknis)." },
  { "en": "Apa Itu Sektor ITU-D (Development)?", "id": "Sektor ITU (International Telecommunication Union) (Pengembangan)." },
  { "en": "Apa Itu World Radiocommunication Conference (WRC)?", "id": "Konferensi ITU (International Telecommunication Union) (Alokasi Spektrum)." },
  { "en": "Apa Kepanjangan Dari WRC (World Radiocommunication Conference)?", "id": "World Radiocommunication Conference." },
  { "en": "Apa Itu Regulasi Radio (ITU)?", "id": "Perjanjian Internasional (Penggunaan Spektrum)." },
  { "en": "Apa Itu Alokasi Spektrum?", "id": "Pembagian Pita Frekuensi (Untuk Layanan)." },
  { "en": "Apa Itu Lisensi Spektrum?", "id": "Izin Resmi Menggunakan Pita Frekuensi." },
  { "en": "Apa Itu Lelang Spektrum?", "id": "Proses Penjualan Lisensi Spektrum." },
  { "en": "Apa Itu Netralitas Jaringan (Net Neutrality)?", "id": "Prinsip Perlakuan Setara Trafik Internet." },
  { "en": "Apa Itu Throttling (Internet)?", "id": "Pembatasan Kecepatan Internet (Sengaja)." },
  { "en": "Apa Itu Interkoneksi (Telekomunikasi)?", "id": "Koneksi Fisik Antar Jaringan Operator." },
  { "en": "Apa Itu Biaya Interkoneksi?", "id": "Biaya Panggilan Antar Operator." },
  { "en": "Apa Itu Roaming?", "id": "Menggunakan Layanan Di Jaringan Operator Lain." },
  { "en": "Apa Itu Over-The-Top (OTT)?", "id": "Layanan (Lewat Internet, Bukan Operator)." },
  { "en": "Contoh Layanan OTT (Over-The-Top)?", "id": "WhatsApp, Netflix, Skype." },
  { "en": "Apa Itu Call Sign (Tanda Panggilan)?", "id": "Identitas Unik Stasiun Radio Amatir." },
  { "en": "Apa Itu Pita HF (High Frequency)?", "id": "Frekuensi 3-30 MHz (Jarak Jauh)." },
  { "en": "Apa Itu Pita VHF (Very High Frequency)?", "id": "Frekuensi 30-300 MHz (Lokal)." },
  { "en": "Apa Itu Pita UHF (Ultra High Frequency)?", "id": "Frekuensi 300-3000 MHz (Lokal)." },
  { "en": "Apa Itu Repeater (Radio Amatir)?", "id": "Penerima Pemancar Ulang (Memperluas Jangkauan)." },
  { "en": "Apa Itu Simplex (Radio)?", "id": "Komunikasi Satu Frekuensi (Bergantian)." },
  { "en": "Apa Itu Duplex (Radio)?", "id": "Komunikasi Dua Frekuensi (Bersamaan)." },
  { "en": "Apa Itu Mode FM (Frequency Modulation) (Radio Amatir)?", "id": "Mode Suara (Kualitas Tinggi, Jarak Dekat)." },
  { "en": "Apa Arti QSO (Q-Code)?", "id": "Kontak Atau Percakapan Radio." },
  { "en": "Apa Arti QTH (Q-Code)?", "id": "Lokasi Stasiun Anda." },
  { "en": "Apa Arti QRM (Q-Code)?", "id": "Gangguan Dari Stasiun Lain." },
  { "en": "Apa Arti QRN (Q-Code)?", "id": "Gangguan Dari Sumber Alami (Listrik)." },
  { "en": "Apa Itu QSL Card?", "id": "Kartu Konfirmasi Kontak QSO." },
  { "en": "Apa Itu Komunikasi Digital (Radio Amatir)?", "id": "Komunikasi Data (PSK31, FT8)." },
  { "en": "Apa Itu FT8 (Franke-Taylor 8)?", "id": "Mode Digital (Sinyal Sangat Lemah)." },
  { "en": "Apa Itu APRS (Automatic Packet Reporting System)?", "id": "Sistem Pelaporan Posisi Otomatis (Radio)." },
  { "en": "Apa Kepanjangan Dari APRS (Automatic Packet Reporting System)?", "id": "Automatic Packet Reporting System." },
  { "en": "Apa Itu Echolink?", "id": "Sistem VoIP (Voice over IP) Radio Amatir." },
  { "en": "Apa Itu Push-to-Talk (PTT)?", "id": "Tombol Untuk Berbicara (Radio)." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Terhubung Internet." },
  { "en": "Apa Itu Gateway (IoT)?", "id": "Gerbang Penghubung Perangkat IoT Internet." },
  { "en": "Apa Itu Model Publish/Subscribe (MQTT)?", "id": "Model Komunikasi (Broker, Topik)." },
  { "en": "Apa Itu Topik (MQTT)?", "id": "Label Kategori Pesan (MQTT)." },
  { "en": "Apa Itu CoAP (Constrained Application Protocol)?", "id": "Protokol Aplikasi IoT (Mirip HTTP)." },
  { "en": "Protokol Apa Digunakan CoAP (Constrained Application Protocol)?", "id": "UDP (User Datagram Protocol)." },
  { "en": "Apa Itu Low-Power Wide-Area Network (LPWAN)?", "id": "Jaringan IoT (Daya Rendah, Jangkauan Luas)." },
  { "en": "Apa Itu LoRa (Long Range)?", "id": "Teknologi Modulasi Radio (Spread Spectrum)." },
  { "en": "Apa Itu LoRaWAN (Long Range Wide Area Network)?", "id": "Protokol Jaringan LPWAN (LoRa)." },
  { "en": "Apa Itu Gateway (LoRaWAN)?", "id": "Penerima Sinyal LoRa (Long Range)." },
  { "en": "Apa Itu Sigfox?", "id": "Teknologi LPWAN (Ultra-Narrowband)." },
  { "en": "Apa Itu NB-IoT (Narrowband-IoT)?", "id": "Standar LPWAN (Jaringan Seluler LTE)." },
  { "en": "Apa Kepanjangan Dari NB-IoT (Narrowband-IoT)?", "id": "Narrowband-Internet of Things." },
  { "en": "Apa Itu LTE-M (Long-Term Evolution for Machines)?", "id": "Standar LPWAN (Seluler, Bandwidth Lebih)." },
  { "en": "Apa Itu 6LoWPAN?", "id": "IPv6 (Internet Protocol Version 6) Di Atas Jaringan IoT." },
  { "en": "Apa Kepanjangan Dari 6LoWPAN (IPv6 over Low-Power Wireless Personal Area Networks)?", "id": "IPv6 over Low-Power Wireless Personal Area Networks." },
  { "en": "Apa Itu Thread (Protokol Jaringan)?", "id": "Protokol Jaringan Mesh (Berbasis 6LoWPAN)." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Versi Bluetooth (Sangat Hemat Daya)." },
  { "en": "Apa Itu Beacon?", "id": "Pemancar Sinyal (BLE) Satu Arah." },
  { "en": "Apa Itu iBeacon?", "id": "Standar Beacon Milik Apple." },
  { "en": "Apa Itu Eddystone?", "id": "Standar Beacon Milik Google." },
  { "en": "Apa Itu Industrial IoT (IIoT)?", "id": "Aplikasi IoT (Internet of Things) Di Industri." },
  { "en": "Apa Kepanjangan Dari IIoT (Industrial IoT)?", "id": "Industrial Internet of Things." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Kontrol Pengawasan Industri." },
  { "en": "Apa Kepanjangan Dari SCADA (Supervisory Control and Data Acquisition)?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Kepanjangan Dari PLC (Programmable Logic Controller)?", "id": "Programmable Logic Controller." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel (WSN)?", "id": "Jaringan Sensor Terdistribusi Nirkabel." },
  { "en": "Apa Kepanjangan Dari WSN (Wireless Sensor Network)?", "id": "Wireless Sensor Network." },
  { "en": "Apa Itu Machine-to-Machine (M2M) Communication?", "id": "Komunikasi Langsung Antar Mesin (Otomatis)." },
  { "en": "Apa Beda IoT (Internet of Things) Dan M2M (Machine-to-Machine)?", "id": "IoT (Internet of Things) Lebih Luas (Cloud)." },
  { "en": "Apa Itu Smart City (Kota Pintar)?", "id": "Kota Menggunakan Teknologi (Mengelola Sumber Daya)." },
  { "en": "Apa Itu Smart Home (Rumah Pintar)?", "id": "Rumah (Perangkat Terhubung, Otomatisasi)." },
  { "en": "Apa Itu Wearable Technology?", "id": "Teknologi (Perangkat Yang Dapat Dikenakan)." },
  { "en": "Contoh Wearable Technology?", "id": "Smartwatch, Fitness Tracker." },
  { "en": "Apa Itu Vehicle-to-Everything (V2X) Communication?", "id": "Komunikasi Kendaraan Ke Segala Sesuatu." },
  { "en": "Apa Kepanjangan Dari V2X (Vehicle-to-Everything)?", "id": "Vehicle-to-Everything." },
  { "en": "Apa Itu V2V (Vehicle-to-Vehicle)?", "id": "Komunikasi Antar Kendaraan." },
  { "en": "Apa Itu V2I (Vehicle-to-Infrastructure)?", "id": "Komunikasi Kendaraan Ke Infrastruktur." },
  { "en": "Apa Itu V2P (Vehicle-to-Pedestrian)?", "id": "Komunikasi Kendaraan Ke Pejalan Kaki." },
  { "en": "Apa Itu Jaringan Ad Hoc?", "id": "Jaringan Nirkabel (Tanpa Infrastruktur Tetap)." },
  { "en": "Apa Itu Mobile Ad Hoc Network (MANET)?", "id": "Jaringan Ad Hoc (Perangkat Bergerak)." },
  { "en": "Apa Kepanjangan Dari MANET (Mobile Ad Hoc Network)?", "id": "Mobile Ad Hoc Network." },
  { "en": "Apa Itu Vehicular Ad Hoc Network (VANET)?", "id": "Jaringan Ad Hoc (Antar Kendaraan)." },
  { "en": "Apa Kepanjangan Dari VANET (Vehicular Ad Hoc Network)?", "id": "Vehicular Ad Hoc Network." },
  { "en": "Apa Itu Protokol Routing Ad Hoc?", "id": "Protokol Routing (Jaringan Dinamis)." },
  { "en": "Contoh Protokol Routing Ad Hoc?", "id": "AODV (Ad Hoc On-Demand Distance Vector)." },
  { "en": "Apa Kepanjangan Dari AODV (Ad Hoc On-Demand Distance Vector)?", "id": "Ad Hoc On-Demand Distance Vector." },
  { "en": "Apa Itu DSR (Dynamic Source Routing)?", "id": "Protokol Routing Ad Hoc (Source Routing)." },
  { "en": "Apa Kepanjangan Dari DSR (Dynamic Source Routing)?", "id": "Dynamic Source Routing." },
  { "en": "Apa Itu Telekomunikasi?", "id": "Komunikasi Jarak Jauh (Elektronik)." },
  { "en": "Siapa Penemu Telegraf Listrik?", "id": "Samuel Morse (Secara Umum)." },
  { "en": "Apa Itu Kode Morse?", "id": "Sistem Kode (Titik Garis)." },
  { "en": "Siapa Penemu Radio?", "id": "Guglielmo Marconi (Secara Umum)." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Gelombang (Medan Listrik Magnet)." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Semua Frekuensi Gelombang." },
  { "en": "Urutkan Spektrum (Frekuensi Terendah)?", "id": "Radio, Microwave, Inframerah, Cahaya." },
  { "en": "Lanjutan Urutan Spektrum (Tertinggi)?", "id": "Ultraviolet, Sinar-X, Sinar Gamma." },
  { "en": "Apa Itu Pita Frekuensi Radio?", "id": "Bagian Spektrum (Komunikasi Radio)." },
  { "en": "Apa Itu ELF (Extremely Low Frequency)?", "id": "Frekuensi Sangat Rendah (3-30 Hz)." },
  { "en": "Apa Itu SLF (Super Low Frequency)?", "id": "Frekuensi Super Rendah (30-300 Hz)." },
  { "en": "Apa Itu ULF (Ultra Low Frequency)?", "id": "Frekuensi Ultra Rendah (300-3000 Hz)." },
  { "en": "Apa Itu VLF (Very Low Frequency)?", "id": "Frekuensi Sangat Rendah (3-30 KHz)." },
  { "en": "Apa Itu LF (Low Frequency)?", "id": "Frekuensi Rendah (30-300 KHz)." },
  { "en": "Apa Itu MF (Medium Frequency)?", "id": "Frekuensi Menengah (300 KHz-3 MHz)." },
  { "en": "Dimana Pita MF (Medium Frequency) Digunakan?", "id": "Siaran Radio AM (Amplitude Modulation)." },
  { "en": "Apa Itu HF (High Frequency)?", "id": "Frekuensi Tinggi (3-30 MHz)." },
  { "en": "Dimana Pita HF (High Frequency) Digunakan?", "id": "Radio Amatir (Jarak Jauh, SW)." },
  { "en": "Apa Itu VHF (Very High Frequency)?", "id": "Frekuensi Sangat Tinggi (30-300 MHz)." },
  { "en": "Dimana Pita VHF (Very High Frequency) Digunakan?", "id": "Siaran Radio FM, TV Analog." },
  { "en": "Apa Itu UHF (Ultra High Frequency)?", "id": "Frekuensi Ultra Tinggi (300 MHz-3 GHz)." },
  { "en": "Dimana Pita UHF (Ultra High Frequency) Digunakan?", "id": "TV Digital, Ponsel, Wi-Fi." },
  { "en": "Apa Itu SHF (Super High Frequency)?", "id": "Frekuensi Super Tinggi (3-30 GHz)." },
  { "en": "Dimana Pita SHF (Super High Frequency) Digunakan?", "id": "Microwave, Satelit (Ku-Band), Radar." },
  { "en": "Apa Itu EHF (Extremely High Frequency)?", "id": "Frekuensi Ekstrem Tinggi (30-300 GHz)." },
  { "en": "Dimana Pita EHF (Extremely High Frequency) Digunakan?", "id": "Satelit (Ka-Band), Radio Astronomi." },
  { "en": "Apa Nama Lain Pita EHF (Extremely High Frequency)?", "id": "Gelombang Milimeter (mmWave)." },
  { "en": "Apa Itu Inframerah (Infrared)?", "id": "Gelombang Elektromagnetik (Di Bawah Merah)." },
  { "en": "Dimana Inframerah Digunakan?", "id": "Remote Control, Komunikasi Jarak Dekat." },
  { "en": "Apa Itu Ultraviolet (UV)?", "id": "Gelombang Elektromagnetik (Di Atas Ungu)." },
  { "en": "Apa Itu Sinar-X (X-Rays)?", "id": "Gelombang Elektromagnetik (Energi Tinggi, Medis)." },
  { "en": "Apa Itu Sinar Gamma (Gamma Rays)?", "id": "Gelombang Elektromagnetik (Energi Tertinggi, Nuklir)." },
  { "en": "Apa Itu Modulasi Analog?", "id": "Modulasi Sinyal Pembawa Analog." },
  { "en": "Apa Itu Amplitude Modulation (AM)?", "id": "Modulasi Amplitudo Sinyal Pembawa." },
  { "en": "Apa Itu Frequency Modulation (FM)?", "id": "Modulasi Frekuensi Sinyal Pembawa." },
  { "en": "Apa Itu Phase Modulation (PM)?", "id": "Modulasi Fasa Sinyal Pembawa." },
  { "en": "Kelebihan FM (Frequency Modulation) Dari AM (Amplitude Modulation)?", "id": "Kualitas Suara Lebih Baik (Tahan Noise)." },
  { "en": "Kelebihan AM (Amplitude Modulation) Dari FM (Frequency Modulation)?", "id": "Jangkauan Lebih Jauh, Bandwidth Sempit." },
  { "en": "Apa Itu Modulasi Digital?", "id": "Modulasi Sinyal Pembawa (Data Digital)." },
  { "en": "Apa Itu Amplitude Shift Keying (ASK)?", "id": "Modulasi Digital (Perubahan Amplitudo)." },
  { "en": "Apa Kepanjangan Dari ASK (Amplitude Shift Keying)?", "id": "Amplitude Shift Keying." },
  { "en": "Apa Itu Frequency Shift Keying (FSK)?", "id": "Modulasi Digital (Perubahan Frekuensi)." },
  { "en": "Apa Kepanjangan Dari FSK (Frequency Shift Keying)?", "id": "Frequency Shift Keying." },
  { "en": "Apa Itu Phase Shift Keying (PSK)?", "id": "Modulasi Digital (Perubahan Fasa)." },
  { "en": "Apa Kepanjangan Dari PSK (Phase Shift Keying)?", "id": "Phase Shift Keying." },
  { "en": "Apa Itu Binary PSK (BPSK)?", "id": "PSK (Phase Shift Keying) (Dua Fasa, 1 Bit)." },
  { "en": "Apa Itu Quadrature PSK (QPSK)?", "id": "PSK (Phase Shift Keying) (Empat Fasa, 2 Bit)." },
  { "en": "Apa Itu Quadrature Amplitude Modulation (QAM)?", "id": "Modulasi Digital (Amplitudo Dan Fasa)." },
  { "en": "Apa Itu 16-QAM (16-Quadrature Amplitude Modulation)?", "id": "QAM (Quadrature Amplitude Modulation) (16 Titik, 4 Bit)." },
  { "en": "Apa Itu 64-QAM (64-Quadrature Amplitude Modulation)?", "id": "QAM (Quadrature Amplitude Modulation) (64 Titik, 6 Bit)." },
  { "en": "Apa Itu Diagram Konstelasi?", "id": "Diagram Visualisasi Modulasi Digital." },
  { "en": "Apa Itu Bit Rate (Laju Bit)?", "id": "Jumlah Bit Per Detik (Bps)." },
  { "en": "Apa Itu Baud Rate (Laju Baud)?", "id": "Jumlah Simbol Per Detik (Baud)." },
  { "en": "Apa Itu Simbol (Modulasi)?", "id": "Satu Perubahan Sinyal (Pembawa Bit)." },
  { "en": "Apa Itu Modem (Modulator-Demodulator)?", "id": "Perangkat Modulasi Dan Demodulasi." },
  { "en": "Apa Kepanjangan Dari Modem (Modulator-Demodulator)?", "id": "Modulator-Demodulator." },
  { "en": "Fungsi Modulator?", "id": "Mengubah Digital Ke Analog (Modulasi)." },
  { "en": "Fungsi Demodulator?", "id": "Mengubah Analog Ke Digital (Demodulasi)." },
  { "en": "Apa Itu Kodek (Codec)?", "id": "Perangkat Keras/Lunak (Kompresi Data)." },
  { "en": "Apa Kepanjangan Dari Codec (Coder-Decoder)?", "id": "Coder-Decoder." },
  { "en": "Apa Itu Kompresi Data?", "id": "Proses Mengurangi Ukuran Data." },
  { "en": "Apa Itu Kompresi Lossy?", "id": "Kompresi (Sebagian Data Hilang, Kualitas Turun)." },
  { "en": "Contoh Kompresi Lossy?", "id": "JPEG, MP3, MPEG." },
  { "en": "Apa Itu Kompresi Lossless?", "id": "Kompresi (Data Asli Utuh, Tanpa Kehilangan)." },
  { "en": "Contoh Kompresi Lossless?", "id": "ZIP, PNG, FLAC." },
  { "en": "Apa Itu Transmisi Serial?", "id": "Pengiriman Data Bit Per Bit (Satu Jalur)." },
  { "en": "Apa Itu Transmisi Paralel?", "id": "Pengiriman Data (Beberapa Bit Bersamaan)." },
  { "en": "Apa Itu Transmisi Asinkron?", "id": "Transmisi (Menggunakan Start/Stop Bit)." },
  { "en": "Apa Itu Transmisi Sinkron?", "id": "Transmisi (Menggunakan Sinyal Clock)." },
  { "en": "Apa Itu Mode Simplex?", "id": "Komunikasi Satu Arah Saja." },
  { "en": "Contoh Mode Simplex?", "id": "Siaran Radio, Siaran Televisi." },
  { "en": "Contoh Mode Half-Duplex?", "id": "Walkie-Talkie." },
  { "en": "Contoh Mode Full-Duplex?", "id": "Telepon, Panggilan Video." },
  { "en": "Apa Itu Duplexing?", "id": "Teknik Transmisi Full-Duplex." },
  { "en": "Apa Itu Frequency Division Duplex (FDD)?", "id": "Duplexing (Frekuensi Uplink Downlink Beda)." },
  { "en": "Apa Kepanjangan Dari FDD (Frequency Division Duplex)?", "id": "Frequency Division Duplex." },
  { "en": "Apa Itu Time Division Duplex (TDD)?", "id": "Duplexing (Slot Waktu Uplink Downlink Beda)." },
  { "en": "Apa Kepanjangan Dari TDD (Time Division Duplex)?", "id": "Time Division Duplex." },
  { "en": "Apa Itu Public Switched Telephone Network (PSTN)?", "id": "Jaringan Telepon Publik Global (Kabel)." },
  { "en": "Apa Itu Local Loop (Telepon)?", "id": "Kabel Telepon (Rumah Ke Sentral)." },
  { "en": "Apa Nama Lain Local Loop?", "id": "Subscriber Line (Saluran Pelanggan)." },
  { "en": "Apa Itu Central Office (CO)?", "id": "Kantor Sentral Telepon Lokal." },
  { "en": "Apa Itu Trunk (Telepon)?", "id": "Saluran Penghubung Antar Sentral Telepon." },
  { "en": "Apa Itu Sinyal Analog (Suara)?", "id": "Gelombang Kontinu Mewakili Suara." },
  { "en": "Apa Itu Sinyal Digital (Suara)?", "id": "Representasi Biner Dari Sinyal Suara." },
  { "en": "Apa Itu Digital Signal 0 (DS0)?", "id": "Satu Saluran Suara Digital (64 Kbps)." },
  { "en": "Apa Itu T-Carrier System?", "id": "Sistem Transmisi Digital (Amerika Utara)." },
  { "en": "Apa Itu T1 (T-Carrier 1)?", "id": "Saluran Transmisi Digital (1.544 Mbps)." },
  { "en": "Berapa Saluran DS0 (Digital Signal 0) Pada T1?", "id": "24 (Dua Puluh Empat) Saluran." },
  { "en": "Apa Itu T3 (T-Carrier 3)?", "id": "Saluran Transmisi (Setara 28 T1)." },
  { "en": "Apa Itu E-Carrier System?", "id": "Sistem Transmisi Digital (Eropa)." },
  { "en": "Apa Itu E1 (E-Carrier 1)?", "id": "Saluran Transmisi Digital (2.048 Mbps)." },
  { "en": "Berapa Saluran DS0 (Digital Signal 0) Pada E1?", "id": "32 (Tiga Puluh Dua) Saluran." },
  { "en": "Apa Itu E3 (E-Carrier 3)?", "id": "Saluran Transmisi (Setara 16 E1)." },
  { "en": "Apa Itu Plesiochronous Digital Hierarchy (PDH)?", "id": "Hierarki Digital (T1/E1, Lama)." },
  { "en": "Apa Itu Synchronous Digital Hierarchy (SDH)?", "id": "Hierarki Digital Sinkron (Pengganti PDH)." },
  { "en": "Apa Itu Synchronous Optical Networking (SONET)?", "id": "Standar SDH (Synchronous Digital Hierarchy) (Amerika)." },
  { "en": "Apa Itu Signaling (Pensinyalan Telepon)?", "id": "Proses Kontrol Panggilan (Membuat, Mengakhiri)." },
  { "en": "Apa Itu Out-of-Band Signaling?", "id": "Pensinyalan Saluran Terpisah (Khusus)." },
  { "en": "Apa Itu Signaling System 7 (SS7)?", "id": "Protokol Pensinyalan Out-of-Band (PSTN)." },
  { "en": "Apa Fungsi SS7 (Signaling System 7)?", "id": "Mengatur Panggilan Telepon Global." },
  { "en": "Apa Itu Integrated Services Digital Network (ISDN)?", "id": "Jaringan Digital (Suara Data Video)." },
  { "en": "Apa Itu Basic Rate Interface (BRI)?", "id": "Layanan ISDN (Integrated Services Digital Network) Dasar." },
  { "en": "Struktur Saluran BRI (Basic Rate Interface)?", "id": "Dua Saluran B, Satu Saluran D." },
  { "en": "Apa Itu B-Channel (Bearer Channel)?", "id": "Saluran Pembawa Data (Suara)." },
  { "en": "Apa Itu D-Channel (Delta Channel)?", "id": "Saluran Pensinyalan (Kontrol)." },
  { "en": "Apa Itu Primary Rate Interface (PRI)?", "id": "Layanan ISDN (Integrated Services Digital Network) Kapasitas Tinggi." },
  { "en": "Struktur Saluran PRI (Primary Rate Interface) (Amerika Utara)?", "id": "23 Saluran B, 1 Saluran D." },
  { "en": "Struktur Saluran PRI (Primary Rate Interface) (Eropa)?", "id": "30 Saluran B, 2 Saluran D." },
  { "en": "Apa Itu Voice over Internet Protocol (VoIP)?", "id": "Layanan Suara Melalui Jaringan IP." },
  { "en": "Apa Keuntungan VoIP (Voice over Internet Protocol)?", "id": "Biaya Murah, Fleksibel, Fitur Kaya." },
  { "en": "Tantangan VoIP (Voice over Internet Protocol)?", "id": "Membutuhkan Jaringan Stabil (QoS)." },
  { "en": "Apa Itu Session Initiation Protocol (SIP)?", "id": "Protokol Pensinyalan Sesi VoIP." },
  { "en": "Apa Fungsi SIP (Session Initiation Protocol)?", "id": "Memulai, Mengatur, Mengakhiri Panggilan." },
  { "en": "Apa Itu H.323?", "id": "Standar Protokol Multimedia (VoIP, Lama)." },
  { "en": "Apa Itu Real-time Transport Protocol (RTP)?", "id": "Protokol Transportasi Media (Suara, Video)." },
  { "en": "Apa Itu Real-time Transport Control Protocol (RTCP)?", "id": "Protokol Kontrol (Umpan Balik RTP)." },
  { "en": "Apa Itu Kodek (Codec) Suara?", "id": "Algoritma Kompresi Dekompresi Suara." },
  { "en": "Apa Itu G.711?", "id": "Kodek Suara (Kualitas Tinggi, 64 Kbps)." },
  { "en": "Apa Itu G.729?", "id": "Kodek Suara (Kompresi Tinggi, 8 Kbps)." },
  { "en": "Apa Itu Mean Opinion Score (MOS)?", "id": "Skor Kualitas Suara Subjektif (1-5)." },
  { "en": "Apa Itu Jitter (VoIP)?", "id": "Variasi Waktu Tiba Paket Suara." },
  { "en": "Apa Itu Latency (VoIP)?", "id": "Waktu Tunda Paket Suara." },
  { "en": "Batas Latency (VoIP) Satu Arah?", "id": "Disarankan Di Bawah 150 Ms." },
  { "en": "Apa Itu Packet Loss (VoIP)?", "id": "Paket Suara Hilang Saat Transmisi." },
  { "en": "Apa Itu Echo (Gema)?", "id": "Suara Pemanggil Terdengar Kembali." },
  { "en": "Apa Itu Echo Cancellation?", "id": "Proses Menghilangkan Gema." },
  { "en": "Apa Itu Media Gateway?", "id": "Perangkat Penghubung Jaringan (VoIP PSTN)." },
  { "en": "Apa Itu IP (Internet Protocol) Phone?", "id": "Telepon Yang Menggunakan Jaringan IP." },
  { "en": "Apa Itu Softphone?", "id": "Aplikasi Telepon (Perangkat Lunak)." },
  { "en": "Apa Itu Interactive Voice Response (IVR)?", "id": "Sistem Jawaban Suara Otomatis." },
  { "en": "Apa Itu Automatic Call Distributor (ACD)?", "id": "Sistem Distribusi Panggilan (Call Center)." },
  { "en": "Apa Itu Konferensi Video?", "id": "Komunikasi Visual Audio Real-Time." },
  { "en": "Apa Itu WebRTC (Web Real-Time Communication)?", "id": "Komunikasi Real-Time Dalam Browser Web." },
  { "en": "Apa Itu Televisi Analog?", "id": "Penyiaran TV (Sinyal Analog)." },
  { "en": "Apa Itu NTSC (National Television System Committee)?", "id": "Standar TV Analog (Amerika Utara)." },
  { "en": "Apa Kepanjangan Dari NTSC (National Television System Committee)?", "id": "National Television System Committee." },
  { "en": "Apa Itu PAL (Phase Alternating Line)?", "id": "Standar TV Analog (Eropa, Asia)." },
  { "en": "Apa Itu SECAM (Séquentiel Couleur à Mémoire)?", "id": "Standar TV Analog (Prancis, Rusia)." },
  { "en": "Apa Itu Televisi Digital?", "id": "Penyiaran TV (Sinyal Digital)." },
  { "en": "Apa Itu ATSC (Advanced Television Systems Committee)?", "id": "Standar TV Digital (Amerika Utara)." },
  { "en": "Apa Itu DVB-T (Digital Video Broadcasting-Terrestrial)?", "id": "Standar TV Digital (Eropa, Asia)." },
  { "en": "Apa Itu ISDB-T (Integrated Services Digital Broadcasting-Terrestrial)?", "id": "Standar TV Digital (Jepang, Brazil)." },
  { "en": "Apa Itu Standard Definition Television (SDTV)?", "id": "Resolusi TV Standar (480i, 576i)." },
  { "en": "Apa Kepanjangan Dari SDTV (Standard Definition Television)?", "id": "Standard Definition Television." },
  { "en": "Apa Itu High Definition Television (HDTV)?", "id": "Resolusi TV Tinggi (720p, 1080i)." },
  { "en": "Apa Kepanjangan Dari HDTV (High Definition Television)?", "id": "High Definition Television." },
  { "en": "Apa Itu Full HD (HD Penuh)?", "id": "Resolusi TV (1080p)." },
  { "en": "Apa Itu Ultra High Definition (UHD)?", "id": "Resolusi TV Sangat Tinggi (4K, 8K)." },
  { "en": "Apa Kepanjangan Dari UHD (Ultra High Definition)?", "id": "Ultra High Definition." },
  { "en": "Apa Itu Resolusi 4K?", "id": "Resolusi (Sekitar 3840 X 2160)." },
  { "en": "Apa Itu Resolusi 8K?", "id": "Resolusi (Sekitar 7680 X 4320)." },
  { "en": "Apa Itu Set-Top Box (STB)?", "id": "Perangkat Penerima Sinyal TV Digital." },
  { "en": "Apa Kepanjangan Dari VOD (Video on Demand)?", "id": "Video on Demand." },
  { "en": "Apa Itu Kodek Video?", "id": "Algoritma Kompresi Dekompresi Video." },
  { "en": "Apa Itu H.264 (AVC)?", "id": "Standar Kompresi Video Populer." },
  { "en": "Apa Itu H.265 (HEVC)?", "id": "Standar Kompresi Video (Lebih Efisien)." },
  { "en": "Apa Itu Streaming (Media)?", "id": "Transmisi Media Kontinu (Internet)." },
  { "en": "Apa Itu HTTP Live Streaming (HLS)?", "id": "Protokol Streaming Adaptif (Apple)." },
  { "en": "Apa Itu Dynamic Adaptive Streaming over HTTP (DASH)?", "id": "Protokol Streaming Adaptif (Standar)." },
  { "en": "Apa Itu Content Delivery Network (CDN)?", "id": "Jaringan Server Cache (Distribusi Konten)." },
  { "en": "Apa Itu Edge Server (CDN)?", "id": "Server CDN (Content Delivery Network) Dekat Pengguna." },
  { "en": "Apa Itu Origin Server (CDN)?", "id": "Server Asli (Sumber Konten)." },
  { "en": "Apa Itu Caching (Cache)?", "id": "Penyimpanan Data Temporer (Akses Cepat)." },
  { "en": "Apa Itu QoS (Quality of Service)?", "id": "Ukuran Kinerja Layanan Jaringan." },
  { "en": "Apa Itu Quality of Experience (QoE)?", "id": "Ukuran Kepuasan Subjektif Pengguna." },
  { "en": "Apa Beda QoS (Quality of Service) Dan QoE (Quality of Experience)?", "id": "QoS (Quality of Service) Teknis, QoE Pengguna." },
  { "en": "Parameter Apa Mempengaruhi QoS (Quality of Service)?", "id": "Latency, Jitter, Bandwidth, Loss." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Tunda (Latency)." },
  { "en": "Apa Itu Bandwidth?", "id": "Kapasitas Maksimal Jalur Transmisi." },
  { "en": "Apa Itu Throughput?", "id": "Laju Data Aktual (Efektif)." },
  { "en": "Apa Itu Integrated Services (IntServ)?", "id": "Model QoS (Quality of Service) (Reservasi Jalur)." },
  { "en": "Apa Kepanjangan Dari IntServ (Integrated Services)?", "id": "Integrated Services." },
  { "en": "Protokol Apa Digunakan IntServ (Integrated Services)?", "id": "RSVP (Resource Reservation Protocol)." },
  { "en": "Apa Itu Differentiated Services (DiffServ)?", "id": "Model QoS (Quality of Service) (Prioritas Kelas)." },
  { "en": "Apa Kepanjangan Dari DiffServ (Differentiated Services)?", "id": "Differentiated Services." },
  { "en": "Model QoS (Quality of Service) Mana Lebih Skalabel?", "id": "DiffServ (Differentiated Services)." },
  { "en": "Apa Itu Differentiated Services Code Point (DSCP)?", "id": "Nilai Penanda Prioritas (Lapis 3)." },
  { "en": "Apa Itu Class of Service (CoS)?", "id": "Nilai Penanda Prioritas (Lapis 2)." },
  { "en": "Apa Itu Penandaan (QoS)?", "id": "Proses Menandai Paket (DSCP/CoS)." },
  { "en": "Apa Itu Antrian (Queuing)?", "id": "Proses Mengelola Antrian Paket." },
  { "en": "Apa Itu FIFO (First-In, First-Out) Queueing?", "id": "Antrian (Datang Pertama, Keluar Pertama)." },
  { "en": "Apa Itu Priority Queuing (PQ)?", "id": "Antrian (Prioritas Tinggi Didahulukan)." },
  { "en": "Apa Itu Weighted Fair Queuing (WFQ)?", "id": "Antrian Adil (Berbasis Bobot)." },
  { "en": "Apa Itu Class-Based Weighted Fair Queuing (CBWFQ)?", "id": "WFQ (Weighted Fair Queuing) Berbasis Kelas." },
  { "en": "Apa Itu Low Latency Queuing (LLQ)?", "id": "CBWFQ (Class-Based Weighted Fair Queuing) Prioritas." },
  { "en": "Apa Itu Traffic Shaping?", "id": "Mengatur Laju Trafik (Menghaluskan, Buffer)." },
  { "en": "Apa Itu Traffic Policing?", "id": "Membatasi Laju Trafik (Membuang Kelebihan)." },
  { "en": "Apa Itu Congestion Avoidance (Pencegahan Kepadatan)?", "id": "Mekanisme Mencegah Kepadatan Jaringan." },
  { "en": "Apa Itu Weighted RED (WRED)?", "id": "RED (Random Early Detection) Berbasis Prioritas." },
  { "en": "Apa Kepanjangan Dari WRED (Weighted RED)?", "id": "Weighted Random Early Detection." },
  { "en": "Apa Itu Link Fragmentation and Interleaving (LFI)?", "id": "Teknik QoS (Quality of Service) Link Lambat." },
  { "en": "Apa Itu Network Security (Keamanan Jaringan)?", "id": "Praktik Melindungi Jaringan Komputer." },
  { "en": "Apa Itu CIA (Confidentiality, Integrity, Availability) Triad?", "id": "Tiga Pilar Keamanan Informasi." },
  { "en": "Apa Itu Confidentiality (Kerahasiaan)?", "id": "Menjaga Informasi (Akses Tidak Sah)." },
  { "en": "Apa Itu Integrity (Integritas)?", "id": "Menjaga Data (Tidak Diubah Sah)." },
  { "en": "Apa Itu Threat (Ancaman)?", "id": "Potensi Penyebab Kerusakan." },
  { "en": "Apa Itu Vulnerability (Kerentanan)?", "id": "Kelemahan Sistem Yang Dapat Dieksploitasi." },
  { "en": "Apa Itu Risk (Risiko)?", "id": "Kemungkinan Kerugian (Ancaman Kerentanan)." },
  { "en": "Apa Itu Exploit?", "id": "Kode Memanfaatkan Kerentanan." },
  { "en": "Apa Itu Zero-Day Exploit?", "id": "Exploit Untuk Kerentanan Belum Diketahui." },
  { "en": "Apa Itu Virus?", "id": "Malware (Memerlukan Inang File)." },
  { "en": "Apa Itu Worm?", "id": "Malware (Menyebar Sendiri Lewat Jaringan)." },
  { "en": "Apa Itu Trojan?", "id": "Malware (Menyamar Program Legal)." },
  { "en": "Apa Itu Ransomware?", "id": "Malware (Mengenkripsi Data Minta Tebusan)." },
  { "en": "Apa Itu Spyware?", "id": "Malware (Memata-matai Pengguna)." },
  { "en": "Apa Itu Adware?", "id": "Malware (Menampilkan Iklan)." },
  { "en": "Apa Itu Rootkit?", "id": "Malware (Menyembunyikan Diri Akses Root)." },
  { "en": "Apa Itu Keylogger?", "id": "Malware (Merekam Ketukan Keyboard)." },
  { "en": "Apa Itu Botnet?", "id": "Jaringan Komputer Zombie (DDoS)." },
  { "en": "Apa Itu Denial of Service (DoS)?", "id": "Serangan Melumpuhkan Layanan (Satu Sumber)." },
  { "en": "Apa Itu Distributed DoS (DDoS)?", "id": "Serangan DoS (Denial of Service) (Banyak Sumber)." },
  { "en": "Apa Itu Phishing?", "id": "Penipuan Mendapatkan Data Sensitif (Email)." },
  { "en": "Apa Itu Spear Phishing?", "id": "Phishing Tertarget (Spesifik)." },
  { "en": "Apa Itu Whaling?", "id": "Spear Phishing (Menargetkan Eksekutif)." },
  { "en": "Apa Itu ARP (Address Resolution Protocol) Poisoning?", "id": "Serangan MITM (Man-in-the-Middle) (Memalsukan ARP)." },
  { "en": "Apa Itu DNS (Domain Name System) Poisoning?", "id": "Serangan MITM (Man-in-the-Middle) (Memalsukan DNS)." },
  { "en": "Apa Itu Access Control List (ACL)?", "id": "Aturan Filter Paket (Firewall)." },
  { "en": "Apa Itu Stateless Firewall?", "id": "Firewall Memeriksa Paket Individual." },
  { "en": "Apa Itu Intrusion Detection System (IDS)?", "id": "Sistem Pendeteksi Intrusi (Peringatan)." },
  { "en": "Apa Itu Intrusion Prevention System (IPS)?", "id": "Sistem Pencegah Intrusi (Blokir)." },
  { "en": "Apa Itu Virtual Private Network (VPN)?", "id": "Jaringan Pribadi Aman (Internet Publik)." },
  { "en": "Apa Itu Tunneling (VPN)?", "id": "Proses Enkapsulasi Paket (VPN)." },
  { "en": "Apa Itu IPsec (Internet Protocol Security)?", "id": "Protokol Keamanan VPN (Virtual Private Network) Lapis 3." },
  { "en": "Apa Itu SSL/TLS (Secure Sockets Layer/Transport Layer Security) VPN?", "id": "VPN (Virtual Private Network) (SSL/TLS, Port 443)." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Proses Mengacak Data (Agar Rahasia)." },
  { "en": "Apa Itu Dekripsi (Decryption)?", "id": "Proses Mengembalikan Data Terenkripsi." },
  { "en": "Apa Itu Enkripsi Simetris?", "id": "Enkripsi (Menggunakan Satu Kunci Sama)." },
  { "en": "Contoh Algoritma Simetris?", "id": "AES (Advanced Encryption Standard), DES, 3DES." },
  { "en": "Apa Itu Enkripsi Asimetris?", "id": "Enkripsi (Menggunakan Dua Kunci Berbeda)." },
  { "en": "Contoh Algoritma Asimetris?", "id": "RSA (Rivest–Shamir–Adleman), ECC." },
  { "en": "Apa Itu Kunci Publik (Public Key)?", "id": "Kunci (Untuk Enkripsi, Boleh Dibagi)." },
  { "en": "Apa Itu Kunci Privat (Private Key)?", "id": "Kunci (Untuk Dekripsi, Harus Rahasia)." },
  { "en": "Apa Itu Algoritma Hashing?", "id": "Fungsi Satu Arah (Membuat Sidik Jari)." },
  { "en": "Apa Fungsi Hashing?", "id": "Memverifikasi Integritas Data." },
  { "en": "Contoh Algoritma Hashing?", "id": "MD5 (Message Digest 5), SHA-256." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Membuktikan Keaslian Integritas Dokumen." },
  { "en": "Bagaimana Tanda Tangan Digital Dibuat?", "id": "Hash Dienkripsi Dengan Kunci Privat." },
  { "en": "Apa Itu Sertifikat Digital?", "id": "Identitas Digital (Mengikat Kunci Publik)." },
  { "en": "Apa Itu Certificate Authority (CA)?", "id": "Lembaga Penerbit Sertifikat Digital." },
  { "en": "Apa Itu Public Key Infrastructure (PKI)?", "id": "Infrastruktur Pengelola Sertifikat Kunci." },
  { "en": "Apa Itu Standar X.509?", "id": "Standar Format Sertifikat Digital." },
  { "en": "Apa Itu Certificate Revocation List (CRL)?", "id": "Daftar Sertifikat Digital Yang Dicabut." },
  { "en": "Apa Itu Online Certificate Status Protocol (OCSP)?", "id": "Protokol Pengecekan Status Sertifikat (Real-time)." },
  { "en": "Apa Itu Electromagnetic Interference (EMI)?", "id": "Gangguan Akibat Gelombang Elektromagnetik." },
  { "en": "Apa Kepanjangan Dari EMI (Electromagnetic Interference)?", "id": "Electromagnetic Interference." },
  { "en": "Apa Itu Radio Frequency Interference (RFI)?", "id": "Gangguan Akibat Gelombang Frekuensi Radio." },
  { "en": "Apa Kepanjangan Dari RFI (Radio Frequency Interference)?", "id": "Radio Frequency Interference." },
  { "en": "Bagaimana Mengurangi EMI (Electronic Industries Alliance) RFI?", "id": "Menggunakan Kabel STP (Shielded Twisted Pair)." },
  { "en": "Apa Itu Grounding (Pentanahan)?", "id": "Menghubungkan Peralatan Ke Tanah (Listrik)." },
  { "en": "Apa Fungsi Grounding?", "id": "Keamanan Listrik, Mengurangi Noise." },
  { "en": "Apa Itu Electrostatic Discharge (ESD)?", "id": "Pelepasan Listrik Statis (Merusak Komponen)." },
  { "en": "Apa Itu Power Surge (Lonjakan Listrik)?", "id": "Kenaikan Tegangan Listrik Tiba-Tiba." },
  { "en": "Apa Itu Surge Protector?", "id": "Perangkat Pelindung Lonjakan Listrik." },
  { "en": "Apa Itu Standby UPS (Offline)?", "id": "UPS (Uninterruptible Power Supply) Aktif Saat Listrik Mati." },
  { "en": "Apa Itu Line-Interactive UPS?", "id": "UPS (Uninterruptible Power Supply) (Stabilisator Tegangan)." },
  { "en": "Apa Itu Online UPS (Double Conversion)?", "id": "UPS (Uninterruptible Power Supply) (Daya Selalu Lewat Baterai)." },
  { "en": "UPS (Uninterruptible Power Supply) Mana Paling Baik?", "id": "Online UPS (Double Conversion)." },
  { "en": "Apa Itu Generator (Genset)?", "id": "Sumber Daya Cadangan Jangka Panjang." },
  { "en": "Apa Itu Automatic Transfer Switch (ATS)?", "id": "Saklar Otomatis (PLN Ke Genset)." },
  { "en": "Apa Itu Power Distribution Unit (PDU)?", "id": "Distributor Stop Kontak Di Rak." },
  { "en": "Apa Itu Hot Aisle (Data Center)?", "id": "Lorong Pembuangan Udara Panas Server." },
  { "en": "Apa Itu Cold Aisle (Data Center)?", "id": "Lorong Pemasukan Udara Dingin Server." },
  { "en": "Apa Itu CRAC (Computer Room Air Conditioner)?", "id": "Pendingin Presisi Ruang Komputer." },
  { "en": "Apa Itu Kelembaban (Humidity)?", "id": "Kadar Uap Air Di Udara." },
  { "en": "Mengapa Kelembaban Penting (Data Center)?", "id": "Mencegah ESD (Electrostatic Discharge) (Terlalu Kering)." },
  { "en": "Apa Itu Sistem Pemadam Api?", "id": "Sistem Proteksi Kebakaran." },
  { "en": "Contoh Gas Pemadam Api (Data Center)?", "id": "FM-200, Novec 1230, Argonite." },
  { "en": "Mengapa Gas Digunakan (Data Center)?", "id": "Tidak Merusak Peralatan Elektronik." },
  { "en": "Apa Itu Raised Floor?", "id": "Lantai Panggung (Distribusi Udara Kabel)." },
  { "en": "Apa Itu Cable Tray (Rak Kabel)?", "id": "Jalur Penyangga Kabel Di Atas." },
  { "en": "Apa Itu Keamanan Fisik?", "id": "Perlindungan Fisik Aset (Kunci, CCTV)." },
  { "en": "Apa Itu CCTV (Closed-Circuit Television)?", "id": "Kamera Pengawas Keamanan." },
  { "en": "Apa Kepanjangan Dari CCTV (Closed-Circuit Television)?", "id": "Closed-Circuit Television." },
  { "en": "Apa Itu Kontrol Akses Biometrik?", "id": "Kontrol Akses (Sidik Jari, Iris)." },
  { "en": "Apa Itu Mantraps?", "id": "Ruang Kecil (Sistem Pintu Ganda)." },
  { "en": "Apa Itu Asset Tagging (Penandaan Aset)?", "id": "Memberi Label Identifikasi Pada Aset." },
  { "en": "Apa Itu Metode Top-Down (Troubleshooting)?", "id": "Memulai Dari Lapis Aplikasi (Atas)." },
  { "en": "Apa Itu Metode Bottom-Up (Troubleshooting)?", "id": "Memulai Dari Lapis Fisik (Bawah)." },
  { "en": "Apa Itu Metode Divide-and-Conquer?", "id": "Memulai Dari Lapis Tengah (Network)." },
  { "en": "Langkah Pertama Troubleshooting?", "id": "Identifikasi Masalah (Mengumpulkan Informasi)." },
  { "en": "Langkah Terakhir Troubleshooting?", "id": "Dokumentasi Temuan Dan Solusi." },
  { "en": "Apa Itu Disaster Recovery Plan (DRP)?", "id": "Rencana Pemulihan Pasca Bencana (IT)." },
  { "en": "Apa Itu Business Continuity Plan (BCP)?", "id": "Rencana Keberlangsungan Operasi Bisnis." },
  { "en": "Apa Itu Business Impact Analysis (BIA)?", "id": "Analisis Dampak Gangguan Bisnis." },
  { "en": "Apa Itu Recovery Time Objective (RTO)?", "id": "Target Waktu Maksimal Pemulihan." },
  { "en": "Apa Itu Recovery Point Objective (RPO)?", "id": "Target Toleransi Kehilangan Data." },
  { "en": "Apa Itu Hot Site?", "id": "Situs Pemulihan (Siap Pakai, Sinkron)." },
  { "en": "Apa Itu Cold Site?", "id": "Situs Pemulihan (Fasilitas Kosong)." },
  { "en": "Apa Itu Warm Site?", "id": "Situs Pemulihan (Perangkat Ada, Data Tidak)." },
  { "en": "Apa Itu Backup (Cadangan)?", "id": "Salinan Data Untuk Pemulihan." },
  { "en": "Apa Itu Incremental Backup?", "id": "Menyalin Data (Berubah Sejak Terakhir)." },
  { "en": "Apa Itu Differential Backup?", "id": "Menyalin Data (Berubah Sejak Full)." },
  { "en": "Apa Itu Aturan Backup 3-2-1?", "id": "3 Salinan, 2 Media, 1 Offsite." },
  { "en": "Apa Itu Snapshot (Cadangan)?", "id": "Gambaran Sistem Sesaat (VM)." },
  { "en": "Apa Itu Change Management (Manajemen Perubahan)?", "id": "Proses Pengelolaan Perubahan Terkontrol." },
  { "en": "Apa Itu Change Request (Permintaan Perubahan)?", "id": "Dokumen Formal Meminta Perubahan." },
  { "en": "Apa Itu Change Advisory Board (CAB)?", "id": "Dewan Peninjau Menyetujui Perubahan." },
  { "en": "Apa Kepanjangan Dari CAB (Change Advisory Board)?", "id": "Change Advisory Board." },
  { "en": "Apa Itu Rollback Plan (Rencana Mundur)?", "id": "Rencana Mengembalikan Ke Kondisi Awal." },
  { "en": "Apa Itu Maintenance Window (Jendela Pemeliharaan)?", "id": "Waktu Terjadwal Untuk Pemeliharaan Sistem." },
  { "en": "Apa Itu Network Documentation (Dokumentasi Jaringan)?", "id": "Catatan Detail Konfigurasi Jaringan." },
  { "en": "Apa Isi Network Documentation?", "id": "Diagram, Konfigurasi, Alamat IP." },
  { "en": "Apa Itu Network Diagram (Diagram Jaringan)?", "id": "Representasi Visual Topologi Jaringan." },
  { "en": "Apa Itu Diagram Fisik?", "id": "Diagram (Menggambarkan Lokasi Fisik Kabel)." },
  { "en": "Apa Itu Diagram Logis?", "id": "Diagram (Menggambarkan Aliran Data IP)." },
  { "en": "Apa Itu Rack Diagram?", "id": "Diagram (Tata Letak Perangkat Rak)." },
  { "en": "Apa Itu Asset Management (Manajemen Aset)?", "id": "Proses Pelacakan Inventaris Aset IT." },
  { "en": "Apa Itu Vendor Documentation?", "id": "Dokumentasi Resmi Dari Produsen Perangkat." },
  { "en": "Apa Itu Request for Proposal (RFP)?", "id": "Permintaan Proposal (Pengadaan Proyek)." },
  { "en": "Apa Kepanjangan Dari RFP (Request for Proposal)?", "id": "Request for Proposal." },
  { "en": "Apa Itu Request for Quote (RFQ)?", "id": "Permintaan Penawaran Harga (Barang Jasa)." },
  { "en": "Apa Kepanjangan Dari RFQ (Request for Quote)?", "id": "Request for Quote." },
  { "en": "Apa Itu Request for Information (RFI)?", "id": "Permintaan Informasi (Pengumpulan Data Vendor)." },
  { "en": "Apa Kepanjangan Dari RFI (Request for Information)?", "id": "Request for Information." },
  { "en": "Apa Itu Statement of Work (SOW)?", "id": "Dokumen Detail Lingkup Kerja Proyek." },
  { "en": "Apa Kepanjangan Dari SOW (Statement of Work)?", "id": "Statement of Work." },
  { "en": "Apa Itu Master Service Agreement (MSA)?", "id": "Perjanjian Induk Layanan Jangka Panjang." },
  { "en": "Apa Kepanjangan Dari MSA (Master Service Agreement)?", "id": "Master Service Agreement." },
  { "en": "Apa Itu Service Level Agreement (SLA)?", "id": "Perjanjian Tingkat Layanan (Jaminan Kinerja)." },
  { "en": "Apa Itu Memorandum of Understanding (MOU)?", "id": "Nota Kesepahaman (Perjanjian Awal)." },
  { "en": "Apa Kepanjangan Dari MOU (Memorandum of Understanding)?", "id": "Memorandum of Understanding." },
  { "en": "Apa Itu Non-Disclosure Agreement (NDA)?", "id": "Perjanjian Kerahasiaan Informasi." },
  { "en": "Apa Kepanjangan Dari NDA (Non-Disclosure Agreement)?", "id": "Non-Disclosure Agreement." },
  { "en": "Apa Itu Software Licensing (Lisensi Perangkat Lunak)?", "id": "Izin Legal Penggunaan Perangkat Lunak." },
  { "en": "Apa Itu Open-Source Software?", "id": "Perangkat Lunak (Kode Sumber Terbuka)." },
  { "en": "Apa Itu Proprietary Software?", "id": "Perangkat Lunak (Kode Sumber Tertutup)." },
  { "en": "Apa Itu End-User License Agreement (EULA)?", "id": "Perjanjian Lisensi Pengguna Akhir." },
  { "en": "Apa Kepanjangan Dari EULA (End-User License Agreement)?", "id": "End-User License Agreement." },
  { "en": "Apa Itu Digital Rights Management (DRM)?", "id": "Manajemen Hak Digital (Proteksi Konten)." },
  { "en": "Apa Kepanjangan Dari DRM (Digital Rights Management)?", "id": "Digital Rights Management." },
  { "en": "Apa Itu Incident Management (ITIL)?", "id": "Manajemen Insiden (Pemulihan Cepat)." },
  { "en": "Apa Itu Problem Management (ITIL)?", "id": "Manajemen Masalah (Mencari Akar Penyebab)." },
  { "en": "Apa Itu Change Management (ITIL)?", "id": "Manajemen Perubahan (Terkontrol)." },
  { "en": "Apa Itu Service Desk?", "id": "Titik Kontak Tunggal (Layanan IT)." },
  { "en": "Apa Itu Project Management (Manajemen Proyek)?", "id": "Proses Mengelola Proyek (Awal Akhir)." },
  { "en": "Apa Itu Project Scope (Lingkup Proyek)?", "id": "Batasan Pekerjaan Dalam Proyek." },
  { "en": "Apa Itu Project Timeline (Garis Waktu Proyek)?", "id": "Jadwal Pelaksanaan Proyek." },
  { "en": "Apa Itu Gantt Chart?", "id": "Diagram Batang (Visualisasi Jadwal Proyek)." },
  { "en": "Apa Itu Milestone (Tonggak Pencapaian)?", "id": "Titik Penting Dalam Proyek." },
  { "en": "Apa Itu Critical Path Method (CPM)?", "id": "Metode Penentuan Jalur Kritis Proyek." },
  { "en": "Apa Kepanjangan Dari CPM (Critical Path Method)?", "id": "Critical Path Method." },
  { "en": "Apa Itu Jalur Kritis (Critical Path)?", "id": "Urutan Tugas (Penentu Durasi Proyek)." },
  { "en": "Apa Itu Work Breakdown Structure (WBS)?", "id": "Pemecahan Pekerjaan Proyek (Terstruktur)." },
  { "en": "Apa Kepanjangan Dari WBS (Work Breakdown Structure)?", "id": "Work Breakdown Structure." },
  { "en": "Apa Itu Agile (Project Management)?", "id": "Metodologi Proyek (Iteratif, Fleksibel)." },
  { "en": "Apa Itu Scrum (Agile)?", "id": "Kerangka Kerja Agile (Sprint, Peran)." },
  { "en": "Apa Itu Kanban (Agile)?", "id": "Metodologi Agile (Visualisasi Alur Kerja)." },
  { "en": "Apa Itu Sprint (Scrum)?", "id": "Iterasi Waktu Kerja (Scrum)." },
  { "en": "Apa Itu Product Owner (Scrum)?", "id": "Peran (Mengelola Product Backlog)." },
  { "en": "Apa Itu Scrum Master (Scrum)?", "id": "Peran (Memfasilitasi Tim Scrum)." },
  { "en": "Apa Itu DevOps?", "id": "Kultur (Kolaborasi Development Operations)." },
  { "en": "Apa Kepanjangan Dari DevOps (Development Operations)?", "id": "Development Operations." },
  { "en": "Tujuan DevOps?", "id": "Mempercepat Siklus Rilis Perangkat Lunak." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Otomatisasi Integrasi Pengujian Kode." },
  { "en": "Apa Itu Command Line Interface (CLI)?", "id": "Antarmuka Perintah Berbasis Teks." },
  { "en": "Apa Kepanjangan Dari CLI (Command Line Interface)?", "id": "Command Line Interface." },
  { "en": "Apa Itu Graphical User Interface (GUI)?", "id": "Antarmuka Pengguna Berbasis Grafis." },
  { "en": "Apa Itu Shell?", "id": "Penerjemah Perintah (Antara User Kernel)." },
  { "en": "Contoh Shell (Linux/Unix)?", "id": "Bash (Bourne Again Shell), Zsh." },
  { "en": "Apa Itu PowerShell?", "id": "Shell Baris Perintah (Microsoft)." },
  { "en": "Apa Itu Command Prompt (Windows)?", "id": "Shell Baris Perintah (Lama, Windows)." },
  { "en": "Apa Itu Kernel (Sistem Operasi)?", "id": "Inti Sistem Operasi (Manajemen Hardware)." },
  { "en": "Apa Itu Sistem Operasi (OS)?", "id": "Perangkat Lunak (Pengelola Sumber Daya)." },
  { "en": "Apa Kepanjangan Dari OS (Operating System)?", "id": "Operating System." },
  { "en": "Apa Itu Multitasking?", "id": "Menjalankan Beberapa Tugas Bersamaan." },
  { "en": "Apa Itu Multithreading?", "id": "Menjalankan Beberapa Thread (Satu Proses)." },
  { "en": "Apa Itu Proses (Sistem Operasi)?", "id": "Program Yang Sedang Dieksekusi." },
  { "en": "Apa Itu Thread (Sistem Operasi)?", "id": "Unit Eksekusi Terkecil (Dalam Proses)." },
  { "en": "Apa Itu Virtual Memory (Memori Virtual)?", "id": "Penggunaan Disk (Sebagai Ekstensi RAM)." },
  { "en": "Apa Itu Paging (Memori)?", "id": "Teknik Manajemen Memori Virtual." },
  { "en": "Apa Itu File System (Sistem Berkas)?", "id": "Struktur Penyimpanan Pengelolaan File." },
  { "en": "Contoh File System (Windows)?", "id": "NTFS (New Technology File System), FAT32." },
  { "en": "Contoh File System (Linux)?", "id": "Ext4, XFS, Btrfs." },
  { "en": "Apa Itu NTFS (New Technology File System)?", "id": "File System Standar Windows Modern." },
  { "en": "Apa Itu FAT32 (File Allocation Table 32)?", "id": "File System Lama (Kompatibilitas Luas)." },
  { "en": "Apa Itu HFS+ (Hierarchical File System Plus)?", "id": "File System Lama (Apple macOS)." },
  { "en": "Apa Itu APFS (Apple File System)?", "id": "File System Modern (Apple macOS, iOS)." },
  { "en": "Apa Itu Open Source Software (OSS)?", "id": "Perangkat Lunak (Kode Sumber Terbuka)." },
  { "en": "Apa Kepanjangan Dari OSS (Open Source Software)?", "id": "Open Source Software." },
  { "en": "Apa Itu Lisensi GPL (General Public License)?", "id": "Lisensi Open Source (Copyleft Kuat)." },
  { "en": "Apa Kepanjangan Dari GPL (General Public License)?", "id": "General Public License." },
  { "en": "Apa Itu Lisensi MIT (Massachusetts Institute of Technology)?", "id": "Lisensi Open Source (Permisif, Bebas)." },
  { "en": "Apa Itu Lisensi Apache?", "id": "Lisensi Open Source (Permisif, Paten)." },
  { "en": "Apa Itu Scripting (Skrip)?", "id": "Penulisan Skrip (Otomatisasi Tugas)." },
  { "en": "Contoh Bahasa Skrip?", "id": "Python, Bash, PowerShell." },
  { "en": "Apa Itu Bash (Bourne Again Shell)?", "id": "Shell Standar Di Linux." },
  { "en": "Apa Itu Python?", "id": "Bahasa Pemrograman (Tingkat Tinggi, Serbaguna)." },
  { "en": "Apa Itu Perl?", "id": "Bahasa Pemrograman (Pengolahan Teks, CGI)." },
  { "en": "Apa Itu Environment Variable (Variabel Lingkungan)?", "id": "Variabel Konfigurasi Sistem Operasi." },
  { "en": "Apa Itu Path (Variabel Lingkungan)?", "id": "Daftar Direktori (Lokasi Program)." },
  { "en": "Apa Itu Regular Expression (Regex)?", "id": "Pola Pencarian Teks (String)." },
  { "en": "Apa Kepanjangan Dari Regex (Regular Expression)?", "id": "Regular Expression." },
  { "en": "Apa Itu REST (Representational State Transfer) API?", "id": "API (Application Programming Interface) Web (HTTP)." },
  { "en": "Apa Itu SOAP (Simple Object Access Protocol)?", "id": "Protokol API (Application Programming Interface) (Berbasis XML)." },
  { "en": "Apa Beda REST (Representational State Transfer) Dan SOAP (Simple Object Access Protocol)?", "id": "REST (Representational State Transfer) Ringan, SOAP (Simple Object Access Protocol) Terstruktur." },
  { "en": "Apa Itu Markdown?", "id": "Bahasa Penandaan Teks Sederhana." },
  { "en": "Apa Itu Virtual Machine (VM)?", "id": "Simulasi Komputer Perangkat Lunak." },
  { "en": "Apa Itu Hypervisor?", "id": "Perangkat Lunak Pembuat VM (Virtual Machine)." },
  { "en": "Apa Itu Hypervisor Tipe 1 (Bare-Metal)?", "id": "Hypervisor Berjalan Langsung Di Keras." },
  { "en": "Contoh Hypervisor Tipe 1?", "id": "VMware ESXi, KVM (Kernel-based Virtual Machine)." },
  { "en": "Apa Itu Hypervisor Tipe 2 (Hosted)?", "id": "Hypervisor Berjalan Di Atas OS (Operating System)." },
  { "en": "Contoh Hypervisor Tipe 2?", "id": "VirtualBox, VMware Workstation." },
  { "en": "Apa Itu vSwitch (Virtual Switch)?", "id": "Switch Virtual (Menghubungkan VM)." },
  { "en": "Apa Itu Network Virtualization (Virtualisasi Jaringan)?", "id": "Membuat Jaringan Virtual (Logis)." },
  { "en": "Contoh Virtualisasi Jaringan?", "id": "VLAN (Virtual Local Area Network), VRF, VXLAN." },
  { "en": "Apa Itu VRF (Virtual Routing and Forwarding)?", "id": "Tabel Routing Virtual (Isolasi)." },
  { "en": "Apa Itu NVGRE (Network Virtualization using Generic Routing Encapsulation)?", "id": "Protokol Tunneling (Mirip VXLAN)." },
  { "en": "Apa Kepanjangan Dari NVGRE (Network Virtualization using Generic Routing Encapsulation)?", "id": "Network Virtualization using Generic Routing Encapsulation." },
  { "en": "Apa Itu Kontainer (Container)?", "id": "Virtualisasi Lapis Sistem Operasi (Ringan)." },
  { "en": "Apa Beda Kontainer Dan VM (Virtual Machine)?", "id": "Kontainer Berbagi Kernel, VM (Virtual Machine) Tidak." },
  { "en": "Apa Itu Pod (Kubernetes)?", "id": "Unit Terkecil Kubernetes (Berisi Kontainer)." },
  { "en": "Apa Itu Service (Kubernetes)?", "id": "Abstraksi Jaringan (Load Balancer Pod)." },
  { "en": "Apa Itu CNI (Container Network Interface)?", "id": "Standar Antarmuka Jaringan Kontainer." },
  { "en": "Apa Kepanjangan Dari CNI (Container Network Interface)?", "id": "Container Network Interface." },
  { "en": "Contoh Plugin CNI (Container Network Interface)?", "id": "Calico, Flannel, Weave." },
  { "en": "Apa Itu Calico (CNI)?", "id": "Plugin CNI (Container Network Interface) (Jaringan Lapis 3)." },
  { "en": "Apa Itu Flannel (CNI)?", "id": "Plugin CNI (Container Network Interface) (Jaringan Overlay Lapis 2)." },
  { "en": "Apa Itu Istio?", "id": "Platform Service Mesh (Open Source)." },
  { "en": "Apa Itu Sidecar Proxy (Istio)?", "id": "Proxy (Ditempelkan Di Setiap Pod)." },
  { "en": "Apa Itu Envoy (Proxy)?", "id": "Proxy Kinerja Tinggi (Sidecar Istio)." },
  { "en": "Apa Itu Control Plane (Istio)?", "id": "Bagian Manajemen Kebijakan (Service Mesh)." },
  { "en": "Apa Itu Data Plane (Istio)?", "id": "Bagian Penerusan Trafik (Envoy Proxy)." },
  { "en": "Apa Itu Microservices (Layanan Mikro)?", "id": "Arsitektur (Aplikasi Pecahan Layanan Kecil)." },
  { "en": "Apa Itu Arsitektur Monolitik?", "id": "Arsitektur (Aplikasi Satu Kesatuan Besar)." },
  { "en": "Apa Itu API (Application Programming Interface) Gateway?", "id": "Gerbang Titik Masuk API (Application Programming Interface)." },
  { "en": "Fungsi API (Application Programming Interface) Gateway?", "id": "Routing, Autentikasi, Rate Limiting." },
  { "en": "Apa Itu Rate Limiting?", "id": "Membatasi Jumlah Permintaan API (Application Programming Interface)." },
  { "en": "Apa Itu Circuit Breaker (Pola Desain)?", "id": "Pola (Mencegah Kegagalan Berantai Layanan)." },
  { "en": "Apa Itu Service Discovery?", "id": "Proses Menemukan Lokasi Layanan (Otomatis)." },
  { "en": "Contoh Alat Service Discovery?", "id": "Consul, Etcd, Zookeeper." },
  { "en": "Apa Itu Consul?", "id": "Alat (Service Discovery, Konfigurasi)." },
  { "en": "Apa Itu Etcd?", "id": "Database Key-Value (Penyimpanan Konfigurasi)." },
  { "en": "Apa Itu Infrastructure as Code (IaC)?", "id": "Mengelola Infrastruktur Menggunakan Kode." },
  { "en": "Apa Itu Terraform?", "id": "Alat IaC (Infrastructure as Code) (Multi-Cloud)." },
  { "en": "Apa Itu Ansible?", "id": "Alat Otomasi (Manajemen Konfigurasi)." },
  { "en": "Apa Itu Puppet?", "id": "Alat Otomasi (Berbasis Model, Deklaratif)." },
  { "en": "Apa Itu Chef?", "id": "Alat Otomasi (Berbasis Resep, Prosedural)." },
  { "en": "Apa Itu Repository (Git)?", "id": "Tempat Penyimpanan Kode (Proyek Git)." },
  { "en": "Apa Itu CI/CD (Continuous Integration/Continuous Deployment)?", "id": "Otomatisasi Pembangunan Rilis Perangkat Lunak." },
  { "en": "Apa Itu Continuous Deployment (CD)?", "id": "Otomatisasi Rilis Ke Produksi." },
  { "en": "Apa Itu GitLab CI/CD?", "id": "Fitur CI/CD (Continuous Integration/Continuous Deployment) GitLab." },
  { "en": "Apa Itu GitHub Actions?", "id": "Platform Otomasi (CI/CD) GitHub." },
  { "en": "Apa Itu Monitoring (Pemantauan)?", "id": "Proses Pengumpulan Analisis Data Kinerja." },
  { "en": "Apa Itu Logging (Pencatatan Log)?", "id": "Proses Perekaman Peristiwa Sistem." },
  { "en": "Apa Itu Metrik (Monitoring)?", "id": "Ukuran Kuantitatif (CPU, Memori)." },
  { "en": "Apa Itu Observability (Keteramatan)?", "id": "Kemampuan Memahami Status Internal Sistem." },
  { "en": "Tiga Pilar Observability?", "id": "Metrik, Log, Tracing." },
  { "en": "Apa Itu Tracing (Pelacakan)?", "id": "Melacak Alur Permintaan (Microservices)." },
  { "en": "Apa Itu Prometheus?", "id": "Sistem Monitoring (Berbasis Metrik)." },
  { "en": "Apa Itu Grafana?", "id": "Platform Visualisasi (Dashboard Metrik)." },
  { "en": "Apa Itu ELK (Elasticsearch, Logstash, Kibana) Stack?", "id": "Tumpukan Manajemen Log Terpusat." },
  { "en": "Apa Itu Elasticsearch?", "id": "Mesin Pencari (Penyimpanan Log)." },
  { "en": "Apa Itu Logstash?", "id": "Pengumpul Pemroses Log." },
  { "en": "Apa Itu Fluentd?", "id": "Pengumpul Data Log (Alternatif Logstash)." },
  { "en": "Apa Itu Jaeger?", "id": "Sistem Distributed Tracing (Open Source)." },
  { "en": "Apa Itu Zipkin?", "id": "Sistem Distributed Tracing (Open Source)." },
  { "en": "Apa Itu OpenTelemetry (OTel)?", "id": "Standar Pengumpulan Telemetri (Tracing, Metrik)." },
  { "en": "Apa Kepanjangan Dari OTel (OpenTelemetry)?", "id": "OpenTelemetry." },
  { "en": "Apa Itu Alerting (Peringatan)?", "id": "Notifikasi Otomatis (Masalah Terdeteksi)." },
  { "en": "Apa Itu Service Level Indicator (SLI)?", "id": "Indikator Kinerja Layanan (Metrik)." },
  { "en": "Apa Kepanjangan Dari SLI (Service Level Indicator)?", "id": "Service Level Indicator." },
  { "en": "Apa Itu Service Level Objective (SLO)?", "id": "Target Kinerja (Berdasarkan SLI)." },
  { "en": "Apa Kepanjangan Dari SLO (Service Level Objective)?", "id": "Service Level Objective." },
  { "en": "Apa Itu Service Level Agreement (SLA)?", "id": "Perjanjian Kontrak (Berbasis SLO)." },
  { "en": "Apa Itu Error Budget (Anggaran Kesalahan)?", "id": "Jumlah Waktu Gagal (Ditoleransi SLO)." },
  { "en": "Apa Itu Site Reliability Engineering (SRE)?", "id": "Disiplin (Mengelola Layanan Andal Skalabel)." },
  { "en": "Apa Kepanjangan Dari SRE (Site Reliability Engineering)?", "id": "Site Reliability Engineering." },
  { "en": "Apa Itu Toil (Pekerjaan Sia-sia)?", "id": "Pekerjaan Manual Berulang (SRE)." },
  { "en": "Apa Itu Chaos Engineering?", "id": "Eksperimen (Menguji Ketahanan Sistem)." },
  { "en": "Apa Itu Blameless Postmortem?", "id": "Analisis Insiden (Tanpa Menyalahkan Individu)." },
  { "en": "Apa Itu Big Data?", "id": "Data (Volume Sangat Besar, Kompleks)." },
  { "en": "Apa Itu Tiga V (Big Data)?", "id": "Volume, Velocity, Variety." },
  { "en": "Apa Itu Volume (Big Data)?", "id": "Ukuran Data Yang Sangat Besar." },
  { "en": "Apa Itu Velocity (Big Data)?", "id": "Kecepatan Data (Masuk Diproses)." },
  { "en": "Apa Itu Variety (Big Data)?", "id": "Keberagaman Jenis Data (Terstruktur, Tidak)." },
  { "en": "Apa Itu Hadoop?", "id": "Kerangka Kerja Pemrosesan Big Data." },
  { "en": "Apa Itu HDFS (Hadoop Distributed File System)?", "id": "Sistem File Terdistribusi Hadoop." },
  { "en": "Apa Kepanjangan Dari HDFS (Hadoop Distributed File System)?", "id": "Hadoop Distributed File System." },
  { "en": "Apa Itu MapReduce?", "id": "Model Pemrograman Pemrosesan Data (Hadoop)." },
  { "en": "Apa Itu Apache Spark?", "id": "Mesin Pemrosesan Data (Cepat, Terdistribusi)." },
  { "en": "Apa Itu Data Warehouse (Gudang Data)?", "id": "Sistem Penyimpanan Data (Analisis Bisnis)." },
  { "en": "Apa Itu ETL (Extract, Transform, Load)?", "id": "Proses Integrasi Data (Gudang Data)." },
  { "en": "Apa Kepanjangan Dari ETL (Extract, Transform, Load)?", "id": "Extract, Transform, Load." },
  { "en": "Apa Itu Extract (ETL)?", "id": "Proses Pengambilan Data Dari Sumber." },
  { "en": "Apa Itu Transform (ETL)?", "id": "Proses Mengubah Data (Membersihkan, Format)." },
  { "en": "Apa Itu Load (ETL)?", "id": "Proses Memasukkan Data Ke Tujuan." },
  { "en": "Apa Itu ELT (Extract, Load, Transform)?", "id": "Proses (Ekstrak, Muat, Lalu Transformasi)." },
  { "en": "Apa Kepanjangan Dari ELT (Extract, Load, Transform)?", "id": "Extract, Load, Transform." },
  { "en": "Apa Itu Data Lake (Danau Data)?", "id": "Penyimpanan Data Mentah (Skala Besar)." },
  { "en": "Apa Beda Data Warehouse Dan Data Lake?", "id": "Warehouse Terstruktur, Lake Mentah." },
  { "en": "Apa Itu Data Mart?", "id": "Bagian Data Warehouse (Spesifik Departemen)." },
  { "en": "Apa Itu Business Intelligence (BI)?", "id": "Teknologi Analisis Data Bisnis." },
  { "en": "Apa Kepanjangan Dari BI (Business Intelligence)?", "id": "Business Intelligence." },
  { "en": "Apa Itu Data Mining (Penambangan Data)?", "id": "Proses Penemuan Pola Dalam Data." },
  { "en": "Apa Itu Machine Learning (ML)?", "id": "Sistem Belajar Dari Data (AI)." },
  { "en": "Apa Kepanjangan Dari ML (Machine Learning)?", "id": "Machine Learning." },
  { "en": "Apa Itu Artificial Intelligence (AI)?", "id": "Kecerdasan Buatan (Simulasi Manusia)." },
  { "en": "Apa Kepanjangan Dari AI (Artificial Intelligence)?", "id": "Artificial Intelligence." },
  { "en": "Apa Itu Deep Learning?", "id": "Cabang Machine Learning (Jaringan Saraf Tiruan)." },
  { "en": "Apa Itu Neural Network (Jaringan Saraf Tiruan)?", "id": "Model Komputasi (Terinspirasi Otak)." },
  { "en": "Apa Itu Data Terstruktur?", "id": "Data (Format Tetap, Tabel, Database)." },
  { "en": "Apa Itu Data Tidak Terstruktur?", "id": "Data (Tanpa Format Tetap, Teks, Gambar)." },
  { "en": "Apa Itu Data Semi-Terstruktur?", "id": "Data (Format Fleksibel, JSON, XML)." },
  { "en": "Apa Itu Database Relasional?", "id": "Database Berbasis Tabel (SQL)." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Kueri Database Relasional." },
  { "en": "Apa Kepanjangan Dari SQL (Structured Query Language)?", "id": "Structured Query Language." },
  { "en": "Apa Itu NoSQL (Not Only SQL)?", "id": "Database Non-Relasional (Fleksibel)." },
  { "en": "Contoh Database NoSQL (Not Only SQL)?", "id": "MongoDB, Cassandra, Redis." },
  { "en": "Apa Itu MongoDB?", "id": "Database NoSQL (Not Only SQL) Berbasis Dokumen." },
  { "en": "Apa Itu Redis?", "id": "Database NoSQL (Not Only SQL) In-Memory." },
  { "en": "Apa Itu Apache Kafka?", "id": "Platform Streaming Data Terdistribusi." },
  { "en": "Apa Itu Message Queue (Antrian Pesan)?", "id": "Sistem Komunikasi Asinkron (Antrian)." },
  { "en": "Contoh Message Queue?", "id": "RabbitMQ, ActiveMQ, SQS." },
  { "en": "Apa Itu RabbitMQ?", "id": "Broker Pesan (Message Queue)." },
  { "en": "Apa Itu Protokol AMQP (Advanced Message Queuing Protocol)?", "id": "Protokol Standar Antrian Pesan." },
  { "en": "Apa Kepanjangan Dari AMQP (Advanced Message Queuing Protocol)?", "id": "Advanced Message Queuing Protocol." },
  { "en": "Apa Itu API (Application Programming Interface) Gateway?", "id": "Gerbang Pengelola API (Application Programming Interface)." },
  { "en": "Apa Fungsi API (Application Programming Interface) Gateway?", "id": "Manajemen Trafik, Keamanan API." },
  { "en": "Apa Itu Authentication (API)?", "id": "Verifikasi Identitas Pengguna API (Application Programming Interface)." },
  { "en": "Apa Itu Authorization (API)?", "id": "Pemberian Izin Akses API (Application Programming Interface)." },
  { "en": "Apa Itu API (Application Programming Interface) Key?", "id": "Kunci Token (Autentikasi API Sederhana)." },
  { "en": "Apa Itu JSON Web Token (JWT)?", "id": "Standar Token (Klaim Identitas Aman)." },
  { "en": "Apa Itu Idempotent (API)?", "id": "Operasi (Hasil Sama Jika Diulang)." },
  { "en": "Metode HTTP (Hypertext Transfer Protocol) Apa Idempotent?", "id": "GET, PUT, DELETE." },
  { "en": "Metode HTTP (Hypertext Transfer Protocol) Apa Tidak Idempotent?", "id": "POST (Post)." },
  { "en": "Apa Itu WebSocket?", "id": "Protokol Komunikasi Dua Arah (Real-Time)." },
  { "en": "Apa Beda WebSocket Dan HTTP (Hypertext Transfer Protocol)?", "id": "WebSocket Koneksi Penuh, HTTP (Request-Response)." },
  { "en": "Apa Itu Webhook?", "id": "Callback HTTP (Hypertext Transfer Protocol) (Notifikasi Event)." },
  { "en": "Apa Itu Server-Sent Events (SSE)?", "id": "Komunikasi Satu Arah (Server Ke Klien)." },
  { "en": "Apa Kepanjangan Dari SSE (Server-Sent Events)?", "id": "Server-Sent Events." },
  { "en": "Apa Itu Long Polling?", "id": "Teknik (Klien Menunggu Respon Server)." },
  { "en": "Apa Itu Short Polling?", "id": "Teknik (Klien Bertanya Terus Menerus)." },
  { "en": "Apa Itu Latency (Jaringan)?", "id": "Waktu Tunda Pengiriman Data." },
  { "en": "Apa Itu Throughput?", "id": "Laju Data Aktual Yang Sukses." },
  { "en": "Apa Itu Goodput?", "id": "Laju Data Berguna (Tanpa Overhead)." },
  { "en": "Apa Itu Bandwidth-Delay Product (BDP)?", "id": "Kapasitas Data Maksimal Dalam Jaringan." },
  { "en": "Apa Kepanjangan Dari BDP (Bandwidth-Delay Product)?", "id": "Bandwidth-Delay Product." },
  { "en": "Apa Itu TCP (Transmission Control Protocol) Window Size?", "id": "Ukuran Buffer Penerima TCP (Transmission Control Protocol)." },
  { "en": "Apa Itu TCP (Transmission Control Protocol) Congestion Window?", "id": "Ukuran Jendela (Kontrol Kepadatan TCP)." },
  { "en": "Apa Itu Algoritma Slow Start (TCP)?", "id": "Tahap Awal Peningkatan Jendela TCP (Transmission Control Protocol)." },
  { "en": "Apa Itu Congestion Avoidance (TCP)?", "id": "Tahap Peningkatan Linear Jendela TCP (Transmission Control Protocol)." },
  { "en": "Apa Itu Fast Retransmit (TCP)?", "id": "Kirim Ulang Cepat (Setelah 3 ACK Duplikat)." },
  { "en": "Apa Itu Fast Recovery (TCP)?", "id": "Pemulihan Cepat (Setelah Fast Retransmit)." },
  { "en": "Apa Itu TCP (Transmission Control Protocol) Tahoe?", "id": "Versi TCP (Transmission Control Protocol) (Slow Start Ulang)." },
  { "en": "Apa Itu TCP (Transmission Control Protocol) Reno?", "id": "Versi TCP (Transmission Control Protocol) (Fast Recovery)." },
  { "en": "Apa Itu TCP (Transmission Control Protocol) CUBIC?", "id": "Algoritma Kontrol Kepadatan (Default Linux)." },
  { "en": "Apa Itu BBR (Bottleneck Bandwidth and Round-trip propagation time)?", "id": "Algoritma Kontrol Kepadatan (Google)." },
  { "en": "Apa Itu Selective Acknowledgment (SACK)?", "id": "Opsi TCP (Transmission Control Protocol) (ACK Selektif)." },
  { "en": "Apa Kepanjangan Dari SACK (Selective Acknowledgment)?", "id": "Selective Acknowledgment." },
  { "en": "Apa Itu Nagle Algorithm?", "id": "Algoritma TCP (Transmission Control Protocol) (Mengurangi Paket Kecil)." },
  { "en": "Apa Itu TCP (Transmission Control Protocol) Keepalive?", "id": "Sinyal Pengecekan Koneksi Aktif." },
  { "en": "Apa Itu Head-of-Line (HOL) Blocking?", "id": "Masalah (Paket Antrian Terdepan Memblokir)." },
  { "en": "Apa Kepanjangan Dari HOL (Head-of-Line)?", "id": "Head-of-Line." },
  { "en": "Protokol Apa Mengalami HOL (Head-of-Line) Blocking?", "id": "TCP (Transmission Control Protocol)." },
  { "en": "Apa Itu QUIC (Quick UDP Internet Connections)?", "id": "Protokol Transport Modern (Berbasis UDP)." },
  { "en": "Apa Keuntungan QUIC (Quick UDP Internet Connections)?", "id": "Mengatasi HOL (Head-of-Line) Blocking, Koneksi Cepat." },
  { "en": "Versi HTTP (Hypertext Transfer Protocol) Apa Menggunakan QUIC?", "id": "HTTP/3." },
  { "en": "Apa Itu Internet Backbone?", "id": "Jaringan Inti Internet (Kapasitas Sangat Tinggi)." },
  { "en": "Apa Itu Peering (Internet)?", "id": "Pertukaran Trafik Langsung Antar ISP (Internet Service Provider)." },
  { "en": "Apa Itu Transit (Internet)?", "id": "Membayar ISP (Internet Service Provider) Akses Internet." },
  { "en": "Apa Itu Tier 1 ISP (Internet Service Provider)?", "id": "ISP (Internet Service Provider) Global (Tidak Bayar Transit)." },
  { "en": "Apa Itu Tier 2 ISP (Internet Service Provider)?", "id": "ISP (Internet Service Provider) Regional (Bayar Transit Tier 1)." },
  { "en": "Apa Itu Tier 3 ISP (Internet Service Provider)?", "id": "ISP (Internet Service Provider) Lokal (Bayar Transit Tier 2)." },
  { "en": "Apa Itu Internet Exchange Point (IXP)?", "id": "Titik Pertukaran Trafik (Peering)." },
  { "en": "Apa Itu Autonomous System (AS)?", "id": "Kumpulan Jaringan (Satu Kebijakan Routing)." },
  { "en": "Apa Itu Autonomous System Number (ASN)?", "id": "Nomor Unik Identifikasi AS (Autonomous System)." },
  { "en": "Apa Itu Border Gateway Protocol (BGP)?", "id": "Protokol Routing Antar AS (Autonomous System)." },
  { "en": "Apa Itu Interior Gateway Protocol (IGP)?", "id": "Protokol Routing Dalam AS (Autonomous System)." },
  { "en": "Contoh Protokol IGP (Interior Gateway Protocol)?", "id": "OSPF (Open Shortest Path First), EIGRP, IS-IS." },
  { "en": "Apa Itu Exterior Gateway Protocol (EGP)?", "id": "Protokol Routing Antar AS (Autonomous System)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol)?", "id": "Protokol Routing Internet (Antar AS)." },
  { "en": "Apa Itu External BGP (eBGP)?", "id": "Sesi BGP (Border Gateway Protocol) Antar AS Berbeda." },
  { "en": "Apa Kepanjangan Dari eBGP (External BGP)?", "id": "External Border Gateway Protocol." },
  { "en": "Apa Itu Internal BGP (iBGP)?", "id": "Sesi BGP (Border Gateway Protocol) Dalam Satu AS." },
  { "en": "Apa Kepanjangan Dari iBGP (Internal BGP)?", "id": "Internal Border Gateway Protocol." },
  { "en": "Protokol Transport Apa Digunakan BGP (Border Gateway Protocol)?", "id": "TCP (Transmission Control Protocol) Port 179." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Path Vector?", "id": "Protokol (Memilih Jalur Berbasis Atribut)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Path Attribute (Atribut Jalur)?", "id": "Informasi Penentu Jalur Terbaik BGP." },
  { "en": "Apa Itu Atribut AS-Path (BGP)?", "id": "Daftar AS (Autonomous System) Dilewati Rute." },
  { "en": "Apa Fungsi Atribut AS-Path?", "id": "Mencegah Loop Routing BGP." },
  { "en": "Apa Itu Atribut Next-Hop (BGP)?", "id": "Alamat IP (Internet Protocol) Hop Berikutnya." },
  { "en": "Apa Itu Atribut Local Preference (BGP)?", "id": "Atribut (Menentukan Jalur Keluar AS)." },
  { "en": "Siapa Yang Mengatur Local Preference?", "id": "Administrator Jaringan Internal (iBGP)." },
  { "en": "Nilai Local Preference Default?", "id": "100 (Seratus)." },
  { "en": "Nilai Local Preference Mana Dipilih?", "id": "Nilai Tertinggi." },
  { "en": "Apa Itu Atribut MED (Multi-Exit Discriminator)?", "id": "Atribut (Menentukan Jalur Masuk AS)." },
  { "en": "Apa Kepanjangan Dari MED (Multi-Exit Discriminator)?", "id": "Multi-Exit Discriminator." },
  { "en": "Nilai MED (Multi-Exit Discriminator) Mana Dipilih?", "id": "Nilai Terendah." },
  { "en": "Apa Itu Atribut Weight (BGP)?", "id": "Atribut (Lokal Router, Cisco)." },
  { "en": "Nilai Weight Mana Dipilih?", "id": "Nilai Tertinggi." },
  { "en": "Atribut Mana Prioritas Pertama (BGP Cisco)?", "id": "Weight (Tertinggi)." },
  { "en": "Atribut Mana Prioritas Kedua (BGP)?", "id": "Local Preference (Tertinggi)." },
  { "en": "Atribut Mana Prioritas Ketiga (BGP)?", "id": "Rute Berasal Lokal (Router Sendiri)." },
  { "en": "Atribut Mana Prioritas Keempat (BGP)?", "id": "AS-Path (Terpendek)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Community?", "id": "Atribut (Penanda Rute, Tagging)." },
  { "en": "Contoh BGP (Border Gateway Protocol) Community Terkenal?", "id": "No-Export, No-Advertise, Local-AS." },
  { "en": "Apa Arti Community 'No-Export'?", "id": "Jangan Iklankan Rute Keluar AS." },
  { "en": "Apa Arti Community 'Local-AS'?", "id": "Jangan Iklankan Keluar Sub-Konfederasi." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Peer (Tetangga)?", "id": "Router BGP (Border Gateway Protocol) Terhubung." },
  { "en": "Apa Itu Pesan Open (BGP)?", "id": "Pesan Membuka Sesi BGP (Border Gateway Protocol)." },
  { "en": "Apa Itu Pesan Update (BGP)?", "id": "Pesan Mengirim Informasi Rute." },
  { "en": "Apa Itu Pesan Keepalive (BGP)?", "id": "Pesan Menjaga Sesi BGP (Border Gateway Protocol)." },
  { "en": "Apa Itu Pesan Notification (BGP)?", "id": "Pesan Error (Menutup Sesi)." },
  { "en": "Apa Itu Aturan Split Horizon (iBGP)?", "id": "Rute (iBGP) Tidak Diteruskan (iBGP)." },
  { "en": "Tujuan Aturan Split Horizon (iBGP)?", "id": "Mencegah Loop Routing Internal." },
  { "en": "Apa Itu Full Mesh (iBGP)?", "id": "Semua Router iBGP (Internal BGP) Saling Peer." },
  { "en": "Apa Kerugian Full Mesh (iBGP)?", "id": "Tidak Skalabel (Banyak Sesi)." },
  { "en": "Apa Itu Route Reflector (RR)?", "id": "Solusi Skalabilitas iBGP (Internal BGP)." },
  { "en": "Apa Fungsi Route Reflector (RR)?", "id": "Memantulkan Rute (Klien iBGP)." },
  { "en": "Apa Itu Klien (Route Reflector)?", "id": "Router iBGP (Internal BGP) (Peer Ke RR)." },
  { "en": "Apa Itu Non-Klien (Route Reflector)?", "id": "Router RR (Route Reflector) (Peer Penuh)." },
  { "en": "Apa Itu Cluster ID (Route Reflector)?", "id": "Identifikasi Cluster RR (Route Reflector)." },
  { "en": "Apa Itu Atribut Originator ID?", "id": "Atribut RR (Route Reflector) (Mencegah Loop)." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Confederation?", "id": "Solusi Skalabilitas (Membagi AS Internal)." },
  { "en": "Apa Itu Sub-AS (Confederation)?", "id": "Autonomous System (AS) Bagian Konfederasi." },
  { "en": "Apa Itu Multiprotocol BGP (MP-BGP)?", "id": "BGP (Border Gateway Protocol) (Mendukung Banyak Protokol)." },
  { "en": "Apa Itu Address Family (MP-BGP)?", "id": "Kategori Protokol (Contoh: IPv6, VPNv4)." },
  { "en": "Apa Itu Address Family VPNv4?", "id": "Address Family (Untuk MPLS VPN)." },
  { "en": "Apa Itu Route Target (RT)?", "id": "Atribut BGP (Border Gateway Protocol) (Kontrol Rute VPN)." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Jaringan Pribadi Virtual (Internet)." },
  { "en": "Apa Itu MPLS (Multiprotocol Label Switching) VPN?", "id": "VPN (Virtual Private Network) (Lapis 3, MPLS)." },
  { "en": "Apa Itu PE (Provider Edge) Router?", "id": "Router Tepi Jaringan Provider." },
  { "en": "Apa Itu CE (Customer Edge) Router?", "id": "Router Tepi Jaringan Pelanggan." },
  { "en": "Apa Itu P (Provider) Router?", "id": "Router Inti Jaringan Provider." },
  { "en": "Protokol Apa Antara PE (Provider Edge) CE (Customer Edge)?", "id": "BGP (Border Gateway Protocol) Statis IGP." },
  { "en": "Protokol Apa Antara PE (Provider Edge) PE?", "id": "MP-BGP (Multiprotocol BGP)." },
  { "en": "Protokol Apa Antara P (Provider) P (Provider)?", "id": "IGP (Interior Gateway Protocol) LDP." },
  { "en": "Apa Itu Seed Metric?", "id": "Nilai Metrik Awal (Redistribusi)." },
  { "en": "Mengapa Seed Metric Penting?", "id": "Protokol Tujuan Memerlukan Metrik." },
  { "en": "Masalah Apa Saat Redistribusi Dua Arah?", "id": "Routing Loop." },
  { "en": "Bagaimana Mencegah Loop Redistribusi?", "id": "Distribute List, Route Map, AD." },
  { "en": "Apa Itu Route Filtering?", "id": "Menyaring Rute Yang Diiklankan Diterima." },
  { "en": "Apa Itu Distribute List?", "id": "Filter Rute (Menggunakan Access List)." },
  { "en": "Apa Itu Prefix List?", "id": "Filter Rute (Berdasarkan Prefix IP)." },
  { "en": "Apa Keuntungan Prefix List?", "id": "Lebih Fleksibel Daripada Access List." },
  { "en": "Apa Itu Route Map?", "id": "Alat Kebijakan Rute (Sangat Fleksibel)." },
  { "en": "Apa Fungsi Route Map?", "id": "Filter Rute, Modifikasi Atribut." },
  { "en": "Apa Itu Perintah 'Match' (Route Map)?", "id": "Mencocokkan Kriteria Rute." },
  { "en": "Apa Itu Perintah 'Set' (Route Map)?", "id": "Mengubah Atribut Rute." },
  { "en": "Bagaimana Implementasi PBR (Policy-Based Routing)?", "id": "Menggunakan Route Map." },
  { "en": "Apa Itu IP (Internet Protocol) SLA (Service Level Agreement)?", "id": "Fitur Cisco (Monitoring Kinerja Aktif)." },
  { "en": "Apa Itu Object Tracking?", "id": "Melacak Status Objek (IP SLA)." },
  { "en": "Bagaimana Kombinasi IP SLA (Service Level Agreement) Tracking?", "id": "Failover Rute Statis Otomatis." },
  { "en": "Mengapa Sinkronisasi Waktu Penting?", "id": "Log Akurat, Autentikasi, Troubleshooting." },
  { "en": "Apa Itu Stratum 16 (NTP)?", "id": "Server Dianggap Tidak Sinkron (Gagal)." },
  { "en": "Protokol Apa Digunakan NTP (Network Time Protocol)?", "id": "UDP (User Datagram Protocol) Port 123." },
  { "en": "Apa Itu Simple Network Time Protocol (SNTP)?", "id": "Versi NTP (Network Time Protocol) Sederhana (Klien)." },
  { "en": "Apa Itu Simple Network Management Protocol (SNMP)?", "id": "Protokol Manajemen Jaringan." },
  { "en": "Apa Itu SNMP (Simple Network Management Protocol) Manager?", "id": "Sistem Pemantau Jaringan (NMS)." },
  { "en": "Apa Itu SNMP (Simple Network Management Protocol) Agent?", "id": "Perangkat Lunak Di Perangkat Jaringan." },
  { "en": "Apa Itu Management Information Base (MIB)?", "id": "Database Objek Manajemen (SNMP)." },
  { "en": "Apa Itu Perintah Get (SNMP)?", "id": "Meminta Nilai OID (Object Identifier)." },
  { "en": "Apa Itu Perintah Set (SNMP)?", "id": "Mengubah Nilai OID (Object Identifier)." },
  { "en": "Apa Itu SNMPv1 (Simple Network Management Protocol Version 1)?", "id": "Versi SNMP (Simple Network Management Protocol) (Tidak Aman)." },
  { "en": "Apa Itu SNMPv2c (Simple Network Management Protocol Version 2c)?", "id": "Versi SNMP (Simple Network Management Protocol) (Community)." },
  { "en": "Apa Itu SNMPv3 (Simple Network Management Protocol Version 3)?", "id": "Versi SNMP (Simple Network Management Protocol) (Aman, Enkripsi)." },
  { "en": "Apa Itu Community String (SNMP)?", "id": "Password Teks (SNMPv1, V2c)." },
  { "en": "Apa Itu RMON (Remote Monitoring)?", "id": "Ekstensi SNMP (Simple Network Management Protocol) Lapis 2." },
  { "en": "Apa Itu Aliran (Flow) NetFlow?", "id": "Trafik Searah (Tujuh Atribut)." },
  { "en": "Apa Itu NetFlow Collector?", "id": "Server Penerima Data Aliran." },
  { "en": "Apa Itu Level Fasilitas (Syslog)?", "id": "Kategori Sumber Pesan Log." },
  { "en": "Apa Itu Level Keparahan (Syslog)?", "id": "Tingkat Keparahan Log (0 Hingga 7)." },
  { "en": "Apa Itu Level 0 (Syslog)?", "id": "Emergency (Darurat)." },
  { "en": "Apa Itu Level 6 (Syslog)?", "id": "Informational (Informasi)." },
  { "en": "Apa Itu Level 7 (Syslog)?", "id": "Debug (Debugging)." },
  { "en": "Mengapa Sinkronisasi Waktu Penting?", "id": "Akurasi Log, Autentikasi." },
  { "en": "Apa Itu Simple Network Time Protocol (SNTP)?", "id": "Versi NTP (Network Time Protocol) Sederhana." },
  { "en": "Apa Itu Precision Time Protocol (PTP)?", "id": "Protokol Sinkronisasi (Sangat Akurat)." },
  { "en": "Standar IEEE (Institute of Electrical and Electronics Engineers) PTP?", "id": "IEEE (Institute of Electrical Electronics Engineers) 1588." },
  { "en": "Apa Itu Port Mirroring?", "id": "Menyalin Trafik Port Ke Port Monitor." },
  { "en": "Apa Itu SPAN (Switched Port Analyzer)?", "id": "Nama Lain Port Mirroring (Cisco)." },
  { "en": "Apa Itu RSPAN (Remote SPAN)?", "id": "SPAN (Switched Port Analyzer) Antar Switch (VLAN)." },
  { "en": "Apa Itu Network TAP (Test Access Point)?", "id": "Perangkat Keras (Mirroring Trafik)." },
  { "en": "Apa Beda TAP (Test Access Point) Dan SPAN (Switched Port Analyzer)?", "id": "TAP (Test Access Point) Hardware Khusus." },
  { "en": "Apa Itu Incident Management (Manajemen Insiden)?", "id": "Proses Pemulihan Layanan Cepat." },
  { "en": "Apa Itu Insiden (ITIL)?", "id": "Gangguan Layanan Tidak Terencana." },
  { "en": "Apa Itu Problem Management (Manajemen Masalah)?", "id": "Proses Pencarian Akar Penyebab Insiden." },
  { "en": "Apa Itu Masalah (Problem)?", "id": "Akar Penyebab Satu Insiden." },
  { "en": "Apa Itu Change Management (Manajemen Perubahan)?", "id": "Proses Kontrol Perubahan Sistem IT." },
  { "en": "Apa Itu Request for Change (RFC)?", "id": "Permintaan Formal Untuk Perubahan." },
  { "en": "Apa Kepanjangan Dari RFC (Request for Change)?", "id": "Request for Change." },
  { "en": "Apa Itu Change Advisory Board (CAB)?", "id": "Dewan Peninjau Perubahan (RFC)." },
  { "en": "Apa Itu Rollback Plan (Rencana Mundur)?", "id": "Rencana Mengembalikan (Jika Gagal)." },
  { "en": "Apa Itu Configuration Management (Manajemen Konfigurasi)?", "id": "Manajemen Aset Konfigurasi IT." },
  { "en": "Apa Itu Configuration Management Database (CMDB)?", "id": "Database Penyimpanan Aset Konfigurasi." },
  { "en": "Apa Itu Configuration Item (CI)?", "id": "Aset Yang Dikelola (Server, Router)." },
  { "en": "Apa Kepanjangan Dari CI (Configuration Item)?", "id": "Configuration Item." },
  { "en": "Apa Itu Service Desk (Layanan Meja)?", "id": "Titik Kontak Tunggal (Pengguna IT)." },
  { "en": "Apa Itu Single Point of Contact (SPOC)?", "id": "Konsep Service Desk (Satu Pintu)." },
  { "en": "Apa Kepanjangan Dari SPOC (Single Point of Contact)?", "id": "Single Point of Contact." },
  { "en": "Apa Itu Level 1 Support (Dukungan)?", "id": "Dukungan Awal (Help Desk)." },
  { "en": "Apa Itu Level 2 Support (Dukungan)?", "id": "Dukungan Teknis (Lebih Mendalam)." },
  { "en": "Apa Itu Level 3 Support (Dukungan)?", "id": "Dukungan Pakar (Vendor, Developer)." },
  { "en": "Apa Itu Service Level Agreement (SLA)?", "id": "Perjanjian Tingkat Layanan (Kontrak)." },
  { "en": "Apa Itu Operational Level Agreement (OLA)?", "id": "Perjanjian Layanan Internal (Antar Tim)." },
  { "en": "Apa Kepanjangan Dari OLA (Operational Level Agreement)?", "id": "Operational Level Agreement." },
  { "en": "Apa Itu Underpinning Contract (UC)?", "id": "Kontrak Dukungan Dengan Pihak Ketiga." },
  { "en": "Apa Kepanjangan Dari UC (Underpinning Contract)?", "id": "Underpinning Contract." },
  { "en": "Apa Itu Digital Video Broadcasting (DVB)?", "id": "Standar Penyiaran Televisi Digital." },
  { "en": "Apa Kepanjangan Dari DVB (Digital Video Broadcasting)?", "id": "Digital Video Broadcasting." },
  { "en": "Apa Itu DVB-S (Digital Video Broadcasting-Satellite)?", "id": "Standar DVB (Digital Video Broadcasting) Satelit." },
  { "en": "Apa Itu DVB-S2 (Digital Video Broadcasting-Satellite 2)?", "id": "Peningkatan DVB-S (Digital Video Broadcasting-Satellite)." },
  { "en": "Apa Itu DVB-C (Digital Video Broadcasting-Cable)?", "id": "Standar DVB (Digital Video Broadcasting) Kabel." },
  { "en": "Apa Itu DVB-T (Digital Video Broadcasting-Terrestrial)?", "id": "Standar DVB (Digital Video Broadcasting) Terestrial." },
  { "en": "Apa Itu DVB-T2 (Digital Video Broadcasting-Terrestrial 2)?", "id": "Peningkatan DVB-T (Digital Video Broadcasting-Terrestrial)." },
  { "en": "Apa Itu Advanced Television Systems Committee (ATSC)?", "id": "Standar TV Digital (Amerika Utara)." },
  { "en": "Apa Itu Integrated Services Digital Broadcasting (ISDB)?", "id": "Standar TV Digital (Jepang, Brazil)." },
  { "en": "Apa Kepanjangan Dari ISDB (Integrated Services Digital Broadcasting)?", "id": "Integrated Services Digital Broadcasting." },
  { "en": "Apa Itu Conditional Access (Akses Bersyarat)?", "id": "Sistem Enkripsi (Siaran Berbayar)." },
  { "en": "Apa Itu Smart Card (TV)?", "id": "Kartu (Dekripsi Siaran Berbayar)." },
  { "en": "Apa Itu Electronic Program Guide (EPG)?", "id": "Panduan Jadwal Program Elektronik." },
  { "en": "Apa Kepanjangan Dari EPG (Electronic Program Guide)?", "id": "Electronic Program Guide." },
  { "en": "Apa Itu High-Definition Multimedia Interface (HDMI)?", "id": "Antarmuka Audio Video Digital." },
  { "en": "Apa Kepanjangan Dari HDMI (High-Definition Multimedia Interface)?", "id": "High-Definition Multimedia Interface." },
  { "en": "Apa Itu High-Bandwidth Digital Content Protection (HDCP)?", "id": "Proteksi Salinan Digital (HDMI)." },
  { "en": "Apa Kepanjangan Dari HDCP (High-Bandwidth Digital Content Protection)?", "id": "High-Bandwidth Digital Content Protection." },
  { "en": "Apa Itu Public Safety Answering Point (PSAP)?", "id": "Pusat Penerima Panggilan Darurat." },
  { "en": "Apa Itu Enhanced 911 (E911)?", "id": "Layanan Darurat (Informasi Lokasi Otomatis)." },
  { "en": "Apa Kepanjangan Dari E911 (Enhanced 911)?", "id": "Enhanced 911." },
  { "en": "Apa Itu Next Generation 911 (NG911)?", "id": "Evolusi E911 (Berbasis IP, Multimedia)." },
  { "en": "Apa Kepanjangan Dari NG911 (Next Generation 911)?", "id": "Next Generation 911." },
  { "en": "Apa Itu Emergency Alert System (EAS)?", "id": "Sistem Peringatan Darurat Publik (AS)." },
  { "en": "Apa Kepanjangan Dari EAS (Emergency Alert System)?", "id": "Emergency Alert System." },
  { "en": "Apa Itu Common Alerting Protocol (CAP)?", "id": "Standar Format Peringatan Darurat (XML)." },
  { "en": "Apa Kepanjangan Dari CAP (Common Alerting Protocol)?", "id": "Common Alerting Protocol." },
  { "en": "Apa Itu Wireless Emergency Alerts (WEA)?", "id": "Peringatan Darurat Nirkabel (Seluler)." },
  { "en": "Apa Kepanjangan Dari WEA (Wireless Emergency Alerts)?", "id": "Wireless Emergency Alerts." },
  { "en": "Apa Itu Internet Engineering Task Force (IETF)?", "id": "Organisasi Standarisasi Protokol Internet." },
  { "en": "Apa Itu Request for Comments (RFC)?", "id": "Dokumen Publikasi Standar IETF (Internet Engineering Task Force)." },
  { "en": "Apa Itu Internet Assigned Numbers Authority (IANA)?", "id": "Otoritas Pengelola Alokasi IP (Internet Protocol)." },
  { "en": "Apa Itu Internet Corporation for Assigned Names and Numbers (ICANN)?", "id": "Organisasi Pengawas IANA (Internet Assigned Numbers Authority)." },
  { "en": "Apa Itu Regional Internet Registry (RIR)?", "id": "Organisasi Regional Pengelola IP (Internet Protocol)." },
  { "en": "Apa Itu APNIC (Asia-Pacific Network Information Centre)?", "id": "RIR (Regional Internet Registry) Wilayah Asia Pasifik." },
  { "en": "Apa Itu ARIN (American Registry for Internet Numbers)?", "id": "RIR (Regional Internet Registry) Wilayah Amerika Utara." },
  { "en": "Apa Itu RIPE NCC (Réseaux IP Européens Network Coordination Centre)?", "id": "RIR (Regional Internet Registry) Wilayah Eropa." },
  { "en": "Apa Itu LACNIC (Latin America and Caribbean Network Information Centre)?", "id": "RIR (Regional Internet Registry) Amerika Latin." },
  { "en": "Apa Itu AfriNIC (African Network Information Centre)?", "id": "RIR (Regional Internet Registry) Wilayah Afrika." },
  { "en": "Apa Itu Domain Name System (DNS)?", "id": "Sistem Penamaan Domain Internet." },
  { "en": "Apa Fungsi DNS (Domain Name System)?", "id": "Menerjemahkan Nama Domain Ke IP (Internet Protocol)." },
  { "en": "Apa Itu Domain Name?", "id": "Nama Unik Identifikasi Website (Google.com)." },
  { "en": "Apa Itu Top-Level Domain (TLD)?", "id": "Bagian Akhir Nama Domain (.com)." },
  { "en": "Apa Itu Generic TLD (gTLD)?", "id": "TLD (Top-Level Domain) Generik (.com, .org, .net)." },
  { "en": "Apa Itu Country Code TLD (ccTLD)?", "id": "TLD (Top-Level Domain) Kode Negara (.id, .au, .jp)." },
  { "en": "Apa Itu Second-Level Domain (SLD)?", "id": "Bagian Nama Domain (Sebelum TLD)." },
  { "en": "Apa Itu Subdomain?", "id": "Bagian Nama Domain (Sebelum SLD)." },
  { "en": "Apa Itu Fully Qualified Domain Name (FQDN)?", "id": "Nama Domain Lengkap (Termasuk Host)." },
  { "en": "Apa Kepanjangan Dari FQDN (Fully Qualified Domain Name)?", "id": "Fully Qualified Domain Name." },
  { "en": "Apa Itu DNS (Domain Name System) Resolver?", "id": "Klien DNS (Domain Name System) (Memulai Kueri)." },
  { "en": "Apa Itu Recursive DNS (Domain Name System) Server?", "id": "Server (Mencari Jawaban Kueri Penuh)." },
  { "en": "Apa Itu Authoritative DNS (Domain Name System) Server?", "id": "Server (Pemegang Rekor DNS Asli)." },
  { "en": "Apa Itu Root DNS (Domain Name System) Server?", "id": "Server DNS (Domain Name System) Tertinggi." },
  { "en": "Apa Itu DNS (Domain Name System) Record A?", "id": "Memetakan Hostname Ke Alamat IPv4." },
  { "en": "Apa Itu DNS (Domain Name System) Record AAAA?", "id": "Memetakan Hostname Ke Alamat IPv6." },
  { "en": "Apa Itu DNS (Domain Name System) Record CNAME?", "id": "Memetakan Alias Ke Nama Host Asli." },
  { "en": "Apa Itu DNS (Domain Name System) Record MX?", "id": "Menentukan Server Email (Mail Exchanger)." },
  { "en": "Apa Itu DNS (Domain Name System) Record NS?", "id": "Menentukan Server Nama Otoritatif." },
  { "en": "Apa Itu DNS (Domain Name System) Record PTR?", "id": "Memetakan IP (Internet Protocol) Ke Hostname." },
  { "en": "Apa Itu Reverse DNS (Domain Name System) Lookup?", "id": "Mencari Hostname Dari Alamat IP (Internet Protocol)." },
  { "en": "Apa Itu DNS (Domain Name System) Record SOA?", "id": "Informasi Utama Otoritas Zona." },
  { "en": "Apa Itu DNS (Domain Name System) Zone?", "id": "Bagian Namespace DNS (Domain Name System)." },
  { "en": "Apa Itu DNS (Domain Name System) Zone File?", "id": "File Teks (Berisi Rekor DNS)." },
  { "en": "Apa Itu Primary DNS (Domain Name System) Server?", "id": "Server DNS (Domain Name System) Master (Utama)." },
  { "en": "Apa Itu Secondary DNS (Domain Name System) Server?", "id": "Server DNS (Domain Name System) Slave (Cadangan)." },
  { "en": "Apa Itu DNS (Domain Name System) Zone Transfer?", "id": "Sinkronisasi Data Antar Server DNS." },
  { "en": "Apa Itu AXFR (Full Zone Transfer)?", "id": "Transfer Seluruh Zona DNS." },
  { "en": "Apa Itu IXFR (Incremental Zone Transfer)?", "id": "Transfer Perubahan Zona DNS." },
  { "en": "Apa Itu TTL (Time To Live) DNS?", "id": "Durasi Penyimpanan Cache Rekor DNS." },
  { "en": "Apa Itu DNS (Domain Name System) Spoofing?", "id": "Serangan (Memalsukan Respon DNS)." },
  { "en": "Apa Itu DNS (Domain Name System) Cache Poisoning?", "id": "Serangan (Meracuni Cache DNS Resolver)." },
  { "en": "Apa Itu DNSSEC (Domain Name System Security Extensions)?", "id": "Ekstensi Keamanan (Memvalidasi DNS)." },
  { "en": "Apa Fungsi DNSSEC (DNS Security Extensions)?", "id": "Memberikan Integritas Otentikasi Data DNS." },
  { "en": "Apa Itu RRSIG (Resource Record Signature)?", "id": "Rekor DNSSEC (Tanda Tangan Digital)." },
  { "en": "Apa Itu DNSKEY (DNS Key)?", "id": "Rekor DNSSEC (Kunci Publik)." },
  { "en": "Apa Itu DS (Delegation Signer)?", "id": "Rekor DNSSEC (Validasi Delegasi)." },
  { "en": "Apa Itu DNS (Domain Name System) Over HTTPS (DoH)?", "id": "Protokol Kueri DNS (Terenkripsi HTTPS)." },
  { "en": "Apa Kepanjangan Dari DoH (DNS over HTTPS)?", "id": "DNS (Domain Name System) Over HTTPS." },
  { "en": "Apa Itu DNS (Domain Name System) Over TLS (DoT)?", "id": "Protokol Kueri DNS (Terenkripsi TLS)." },
  { "en": "Apa Kepanjangan Dari DoT (DNS over TLS)?", "id": "DNS (Domain Name System) Over TLS." },
  { "en": "Port Apa Digunakan DoT (DNS over TLS)?", "id": "Port 853 (Delapan Ratus Lima Puluh Tiga)." },
  { "en": "Apa Itu Split-Horizon DNS (Domain Name System)?", "id": "Memberi Respon DNS (Beda Internal Eksternal)." },
  { "en": "Apa Itu Dynamic DNS (DDNS)?", "id": "DNS (Domain Name System) (Update IP Dinamis)." },
  { "en": "Apa Itu URL (Uniform Resource Locator)?", "id": "Alamat Web Lengkap (Lokasi Sumber Daya)." },
  { "en": "Apa Kepanjangan Dari URL (Uniform Resource Locator)?", "id": "Uniform Resource Locator." },
  { "en": "Apa Itu URI (Uniform Resource Identifier)?", "id": "Pengidentifikasi Sumber Daya (Lebih Umum)." },
  { "en": "Apa Kepanjangan Dari URI (Uniform Resource Identifier)?", "id": "Uniform Resource Identifier." },
  { "en": "Apa Itu URN (Uniform Resource Name)?", "id": "Nama Sumber Daya (Bagian URI)." },
  { "en": "Apa Kepanjangan Dari URN (Uniform Resource Name)?", "id": "Uniform Resource Name." },
  { "en": "Apa Itu Skema (URL)?", "id": "Bagian Awal URL (Contoh: http, https)." },
  { "en": "Apa Itu Otoritas (URL)?", "id": "Bagian (Nama Domain Port)." },
  { "en": "Apa Itu Jalur (Path) (URL)?", "id": "Lokasi Spesifik Sumber Daya (Server)." },
  { "en": "Apa Itu Kueri (Query) (URL)?", "id": "Parameter Tambahan (Dimulai Tanda ?)." },
  { "en": "Apa Itu Fragmen (URL)?", "id": "Bagian Spesifik Halaman (Dimulai #)." },
  { "en": "Apa Itu Hyperlink?", "id": "Tautan (Link) Ke Sumber Daya Lain." },
  { "en": "Apa Itu HTML (Hypertext Markup Language)?", "id": "Bahasa Penanda (Struktur Halaman Web)." },
  { "en": "Apa Itu CSS (Cascading Style Sheets)?", "id": "Bahasa (Gaya Tampilan Halaman Web)." },
  { "en": "Apa Itu JavaScript?", "id": "Bahasa Pemrograman (Interaktivitas Web)." },
  { "en": "Apa Itu Web Browser?", "id": "Aplikasi (Menampilkan Halaman Web)." },
  { "en": "Apa Itu Web Server?", "id": "Server (Menyimpan Menyajikan Halaman Web)." },
  { "en": "Contoh Web Server?", "id": "Apache, Nginx, Microsoft IIS." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Komunikasi Web (Browser Server)." },
  { "en": "Apa Itu HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Versi Aman HTTP (Hypertext Transfer Protocol)." },
  { "en": "Apa Itu Cookie (Web)?", "id": "Data Kecil (Disimpan Browser Klien)." },
  { "en": "Apa Fungsi Cookie?", "id": "Menyimpan Informasi Sesi, Preferensi." },
  { "en": "Apa Itu Web Cache?", "id": "Penyimpanan Temporer (Sumber Daya Web)." },
  { "en": "Apa Fungsi Web Cache?", "id": "Mempercepat Pemuatan Halaman." },
  { "en": "Apa Itu CDN (Content Delivery Network)?", "id": "Jaringan Server (Cache Konten Global)." },
  { "en": "Apa Itu Web Hosting?", "id": "Layanan Penyimpanan File Website." },
  { "en": "Apa Itu Shared Hosting?", "id": "Hosting (Berbagi Server Banyak Situs)." },
  { "en": "Apa Itu VPS (Virtual Private Server) Hosting?", "id": "Hosting (Server Virtual Pribadi)." },
  { "en": "Apa Kepanjangan Dari VPS (Virtual Private Server)?", "id": "Virtual Private Server." },
  { "en": "Apa Itu Dedicated Hosting?", "id": "Hosting (Satu Server Fisik Khusus)." },
  { "en": "Apa Itu Cloud Hosting?", "id": "Hosting (Sumber Daya Cloud Skalabel)." },
  { "en": "Apa Itu Web Socket?", "id": "Protokol Komunikasi Dua Arah (Real-Time)." },
  { "en": "Apa Itu WebRTC (Web Real-Time Communication)?", "id": "Komunikasi Real-Time (Browser)." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Data (Populer Untuk API)." },
  { "en": "Apa Itu SOAP (Simple Object Access Protocol)?", "id": "Protokol API (Application Programming Interface) (XML)." },
  { "en": "Apa Beda REST (Representational State Transfer) API (Application Programming Interface) Dan SOAP (Simple Object Access Protocol)?", "id": "REST (Representational State Transfer) Sederhana, SOAP (Simple Object Access Protocol) Standar." },
  { "en": "Apa Itu XML (Extensible Markup Language)?", "id": "Bahasa Penanda (Struktur Data)." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Data Ringan (Alternatif XML)." },
  { "en": "Mengapa JSON (JavaScript Object Notation) Populer?", "id": "Mudah Dibaca Manusia Mesin." },
  { "en": "Apa Itu Web Service (Layanan Web)?", "id": "Layanan Komunikasi Antar Aplikasi (Web)." },
  { "en": "Apa Itu WSDL (Web Services Description Language)?", "id": "Bahasa Deskripsi Layanan Web (SOAP)." },
  { "en": "Apa Kepanjangan Dari WSDL (Web Services Description Language)?", "id": "Web Services Description Language." },
  { "en": "Apa Itu UDDI (Universal Description, Discovery and Integration)?", "id": "Direktori Penemuan Layanan Web." },
  { "en": "Apa Kepanjangan Dari UDDI (Universal Description, Discovery and Integration)?", "id": "Universal Description, Discovery and Integration." },
  { "en": "Apa Itu AJAX (Asynchronous JavaScript and XML)?", "id": "Teknik Web (Update Halaman Asinkron)." },
  { "en": "Apa Kepanjangan Dari AJAX (Asynchronous JavaScript and XML)?", "id": "Asynchronous JavaScript and XML." },
  { "en": "Apa Itu XMLHttpRequest (XHR)?", "id": "Objek JavaScript (Permintaan Data AJAX)." },
  { "en": "Apa Itu Fetch API (Application Programming Interface)?", "id": "API (Application Programming Interface) Modern (Pengganti XHR)." },
  { "en": "Apa Itu Cross-Origin Resource Sharing (CORS)?", "id": "Mekanisme Keamanan (Permintaan Lintas Domain)." },
  { "en": "Apa Kepanjangan Dari CORS (Cross-Origin Resource Sharing)?", "id": "Cross-Origin Resource Sharing." },
  { "en": "Apa Itu Same-Origin Policy (SOP)?", "id": "Kebijakan Keamanan Browser (Satu Asal)." },
  { "en": "Apa Kepanjangan Dari SOP (Same-Origin Policy)?", "id": "Same-Origin Policy." },
  { "en": "Apa Keuntungan WebSocket?", "id": "Komunikasi Cepat (Overhead Rendah)." },
  { "en": "Apa Itu WebRTC (Web Real-Time Communication)?", "id": "Komunikasi Real-Time (Audio Video Browser)." },
  { "en": "Apa Itu STUN (Session Traversal Utilities for NAT)?", "id": "Protokol (Menemukan Alamat IP Publik)." },
  { "en": "Apa Itu TURN (Traversal Using Relays around NAT)?", "id": "Protokol (Relay Trafik Lewat Server)." },
  { "en": "Apa Itu ICE (Interactive Connectivity Establishment)?", "id": "Kerangka Kerja (Menemukan Jalur Koneksi)." },
  { "en": "Apa Itu Session Description Protocol (SDP)?", "id": "Format Deskripsi Sesi Multimedia." },
  { "en": "Apa Kepanjangan Dari SDP (Session Description Protocol)?", "id": "Session Description Protocol." },
  { "en": "Apa Itu Voice over IP (VoIP)?", "id": "Layanan Suara Melalui Jaringan IP." },
  { "en": "Apa Kepanjangan Dari VoIP (Voice over IP)?", "id": "Voice over Internet Protocol." },
  { "en": "Apa Itu SIP (Session Initiation Protocol)?", "id": "Protokol Pensinyalan Sesi VoIP." },
  { "en": "Apa Itu RTP (Real-time Transport Protocol)?", "id": "Protokol Transportasi Media (VoIP)." },
  { "en": "Apa Itu RTCP (Real-time Transport Control Protocol)?", "id": "Protokol Kontrol (Umpan Balik QoS RTP)." },
  { "en": "Apa Itu Kodek Suara (Audio Codec)?", "id": "Algoritma Kompresi Dekompresi Suara." },
  { "en": "Apa Itu Opus (Kodek)?", "id": "Kodek Suara (Sangat Fleksibel, Kualitas Tinggi)." },
  { "en": "Apa Itu Jitter Buffer?", "id": "Buffer (Mengurangi Dampak Jitter)." },
  { "en": "Apa Itu Packet Loss (VoIP)?", "id": "Paket Suara Hilang Selama Transmisi." },
  { "en": "Apa Itu Mean Opinion Score (MOS)?", "id": "Skor Subjektif Kualitas Suara (1-5)." },
  { "en": "Apa Itu IP (Internet Protocol) PBX (Private Branch Exchange)?", "id": "PBX (Private Branch Exchange) Berbasis Jaringan IP." },
  { "en": "Apa Itu Media Gateway?", "id": "Perangkat Konversi (VoIP Ke PSTN)." },
  { "en": "Apa Itu Unified Communications (UC)?", "id": "Integrasi Alat Komunikasi (Suara, Video, Chat)." },
  { "en": "Apa Itu Presence (UC)?", "id": "Informasi Status Ketersediaan (Online, Sibuk)." },
  { "en": "Apa Itu Instant Messaging (IM)?", "id": "Layanan Pesan Instan (Chat Real-Time)." },
  { "en": "Apa Itu Extensible Messaging and Presence Protocol (XMPP)?", "id": "Protokol Terbuka (IM Dan Presence)." },
  { "en": "Apa Itu Streaming Media?", "id": "Transmisi Media Kontinu (Internet)." },
  { "en": "Apa Itu Real Time Streaming Protocol (RTSP)?", "id": "Protokol Kontrol Sesi Streaming." },
  { "en": "Apa Kepanjangan Dari RTSP (Real Time Streaming Protocol)?", "id": "Real Time Streaming Protocol." },
  { "en": "Apa Beda RTSP (Real Time Streaming Protocol) Dan HTTP (Hypertext Transfer Protocol)?", "id": "RTSP (Real Time Streaming Protocol) Kontrol, HTTP (Hypertext Transfer Protocol) Transfer." },
  { "en": "Apa Itu Adaptive Bitrate Streaming (ABR)?", "id": "Streaming (Kualitas Menyesuaikan Jaringan)." },
  { "en": "Apa Kepanjangan Dari ABR (Adaptive Bitrate Streaming)?", "id": "Adaptive Bitrate Streaming." },
  { "en": "Apa Itu HTTP Live Streaming (HLS)?", "id": "Protokol ABR (Adaptive Bitrate Streaming) (Apple)." },
  { "en": "Apa Itu Dynamic Adaptive Streaming over HTTP (DASH)?", "id": "Protokol ABR (Adaptive Bitrate Streaming) (Standar)." },
  { "en": "Apa Itu Edge Server (CDN)?", "id": "Server CDN (Content Delivery Network) (Lokasi Tepi)." },
  { "en": "Apa Itu Cache Hit (CDN)?", "id": "Permintaan Konten (Tersedia Di Cache)." },
  { "en": "Apa Itu Cache Miss (CDN)?", "id": "Permintaan Konten (Tidak Ada Di Cache)." },
  { "en": "Apa Itu Cache Invalidation?", "id": "Proses Menghapus Konten Cache (Update)." },
  { "en": "Apa Itu Peer-to-Peer (P2P) Networking?", "id": "Jaringan (Perangkat Saling Terhubung Langsung)." },
  { "en": "Apa Kepanjangan Dari P2P (Peer-to-Peer)?", "id": "Peer-to-Peer." },
  { "en": "Contoh Jaringan P2P (Peer-to-Peer)?", "id": "BitTorrent, Jaringan Ad Hoc." },
  { "en": "Apa Itu BitTorrent?", "id": "Protokol Berbagi File P2P (Peer-to-Peer)." },
  { "en": "Apa Itu Tracker (BitTorrent)?", "id": "Server (Koordinasi Peer BitTorrent)." },
  { "en": "Apa Itu Swarm (BitTorrent)?", "id": "Kumpulan Peer (Berbagi File Sama)." },
  { "en": "Apa Itu Seed (BitTorrent)?", "id": "Peer (Memiliki File Lengkap)." },
  { "en": "Apa Itu Leech (BitTorrent)?", "id": "Peer (Mengunduh File, Belum Lengkap)." },
  { "en": "Apa Itu Distributed Hash Table (DHT)?", "id": "Sistem Terdistribusi (Pencarian Peer)." },
  { "en": "Apa Kepanjangan Dari DHT (Distributed Hash Table)?", "id": "Distributed Hash Table." },
  { "en": "Fungsi DHT (Distributed Hash Table) (BitTorrent)?", "id": "Menemukan Peer Tanpa Tracker." },
  { "en": "Apa Itu Magnet Link?", "id": "Tautan (Mengidentifikasi File BitTorrent)." },
  { "en": "Apa Itu Gnutella?", "id": "Protokol Jaringan P2P (Peer-to-Peer) (Desentralisasi)." },
  { "en": "Apa Itu Blockchain?", "id": "Buku Besar Digital (Terdistribusi, Terenkripsi)." },
  { "en": "Apa Itu Bitcoin?", "id": "Mata Uang Kripto (Berbasis Blockchain)." },
  { "en": "Apa Itu Cryptocurrency (Mata Uang Kripto)?", "id": "Mata Uang Digital (Menggunakan Kriptografi)." },
  { "en": "Apa Itu Mining (Penambangan Kripto)?", "id": "Proses Validasi Transaksi (Blockchain)." },
  { "en": "Apa Itu Proof-of-Work (PoW)?", "id": "Mekanisme Konsensus (Mining, Bitcoin)." },
  { "en": "Apa Kepanjangan Dari PoW (Proof-of-Work)?", "id": "Proof-of-Work." },
  { "en": "Apa Itu Proof-of-Stake (PoS)?", "id": "Mekanisme Konsensus (Validasi Aset)." },
  { "en": "Apa Kepanjangan Dari PoS (Proof-of-Stake)?", "id": "Proof-of-Stake." },
  { "en": "Apa Itu Smart Contract (Kontrak Pintar)?", "id": "Program (Berjalan Otomatis Di Blockchain)." },
  { "en": "Platform Smart Contract Populer?", "id": "Ethereum." },
  { "en": "Apa Itu Decentralized Application (DApp)?", "id": "Aplikasi Terdesentralisasi (Blockchain)." },
  { "en": "Apa Kepanjangan Dari DApp (Decentralized Application)?", "id": "Decentralized Application." },
  { "en": "Apa Itu Inter-AS (Autonomous System) VPN (Virtual Private Network)?", "id": "VPN (Virtual Private Network) Antar AS Berbeda." },
  { "en": "Apa Itu Opsi A (Inter-AS VPN)?", "id": "Inter-AS (Autonomous System) (Back-to-Back VRF)." },
  { "en": "Apa Itu Opsi B (Inter-AS VPN)?", "id": "Inter-AS (Autonomous System) (eBGP VPNv4)." },
  { "en": "Apa Itu Opsi C (Inter-AS VPN)?", "id": "Inter-AS (Autonomous System) (Multihop eBGP)." },
  { "en": "Apa Itu Carrier Supporting Carrier (CSC)?", "id": "Provider (MPLS) Mendukung Provider Lain." },
  { "en": "Apa Kepanjangan Dari CSC (Carrier Supporting Carrier)?", "id": "Carrier Supporting Carrier." },
  { "en": "Apa Itu BGP (Border Gateway Protocol) Route Dampening?", "id": "Mekanisme (Menstabilkan Rute Fluktuatif)." },
  { "en": "Apa Itu Rute Fluktuatif (Flapping)?", "id": "Rute (Sering Naik Turun)." },
  { "en": "Apa Itu BFD (Bidirectional Forwarding Detection)?", "id": "Protokol Deteksi Kegagalan Jalur Cepat." },
  { "en": "Apa Kepanjangan Dari BFD (Bidirectional Forwarding Detection)?", "id": "Bidirectional Forwarding Detection." },
  { "en": "Apa Fungsi BFD (Bidirectional Forwarding Detection)?", "id": "Mempercepat Konvergensi Protokol Routing." },
  { "en": "Protokol Apa Menggunakan BFD (Bidirectional Forwarding Detection)?", "id": "OSPF (Open Shortest Path First), BGP, EIGRP." },
  { "en": "Apa Itu Non-Stop Forwarding (NSF)?", "id": "Fitur (Data Plane Tetap Jalan)." },
  { "en": "Apa Kepanjangan Dari NSF (Non-Stop Forwarding)?", "id": "Non-Stop Forwarding." },
  { "en": "Apa Itu Graceful Restart (GR)?", "id": "Fitur (Restart Control Plane Mulus)." },
  { "en": "Apa Kepanjangan Dari GR (Graceful Restart)?", "id": "Graceful Restart." },
  { "en": "Tujuan NSF (Non-Stop Forwarding) Dan GR (Graceful Restart)?", "id": "Meningkatkan Ketersediaan Jaringan (HA)." },
  { "en": "Apa Itu In-Service Software Upgrade (ISSU)?", "id": "Upgrade Software Tanpa Downtime." },
  { "en": "Apa Kepanjangan Dari ISSU (In-Service Software Upgrade)?", "id": "In-Service Software Upgrade." },
  { "en": "Apa Itu Virtual Router?", "id": "Instansi Router Logis (Dalam Fisik)." },
  { "en": "Apa Itu Overlay Network (Jaringan Overlay)?", "id": "Jaringan Virtual (Di Atas Fisik)." },
  { "en": "Apa Itu Underlay Network (Jaringan Underlay)?", "id": "Jaringan Fisik (Transport Dasar)." },
  { "en": "Contoh Jaringan Overlay?", "id": "GRE (Generic Routing Encapsulation), VXLAN, DMVPN." },
  { "en": "Apa Itu Generic Routing Encapsulation (GRE)?", "id": "Protokol Tunneling Sederhana (Cisco)." },
  { "en": "Apa Itu VXLAN (Virtual Extensible LAN)?", "id": "Protokol Overlay Lapis 2 (Skala Besar)." },
  { "en": "Apa Itu VNI (VXLAN Network Identifier)?", "id": "Identifier Jaringan VXLAN (24-Bit)." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
