<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DokumenDigital AMDK - Sistem Manajemen Dokumen</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        * {
            font-family: 'Plus Jakarta Sans', sans-serif;
        }
        
        .glass-effect {
            background: rgba(255, 255, 255, 0.7);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
        }
        
        .ocean-gradient {
            background: linear-gradient(135deg, #0066cc 0%, #00a8e8 50%, #00d4aa 100%);
        }
        
        .card-hover {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        
        .card-hover:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px -5px rgba(0, 102, 204, 0.15);
        }
        
        .sidebar-item {
            transition: all 0.2s ease;
        }
        
        .sidebar-item:hover {
            background: rgba(255, 255, 255, 0.1);
            transform: translateX(5px);
        }
        
        .sidebar-item.active {
            background: rgba(255, 255, 255, 0.2);
            border-right: 3px solid #00d4aa;
        }
        
        .document-card {
            transition: all 0.3s ease;
        }
        
        .document-card:hover {
            transform: scale(1.02);
            box-shadow: 0 10px 30px -5px rgba(0, 0, 0, 0.1);
        }
        
        .status-badge {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.8; }
        }
        
        .wave-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.03;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 1440 320'%3E%3Cpath fill='%230066cc' fill-opacity='1' d='M0,96L48,112C96,128,192,160,288,160C384,160,480,128,576,112C672,96,768,96,864,112C960,128,1056,160,1152,160C1248,160,1344,128,1392,112L1440,96L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z'%3E%3C/path%3E%3C/svg%3E");
            background-size: cover;
        }
        
        .drop-zone {
            border: 2px dashed #cbd5e1;
            transition: all 0.3s ease;
        }
        
        .drop-zone.dragover {
            border-color: #0066cc;
            background: rgba(0, 102, 204, 0.05);
        }
        
        .search-input:focus {
            box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.1);
        }
        
        .category-tag {
            transition: all 0.2s ease;
        }
        
        .category-tag:hover {
            transform: scale(1.05);
        }
        
        @keyframes slideIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .animate-slide-in {
            animation: slideIn 0.5s ease-out forwards;
        }
        
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
        }
        
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 3px;
        }
        
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 overflow-hidden">

    <div class="wave-bg"></div>

    <!-- Sidebar -->
    <aside class="fixed left-0 top-0 h-full w-72 ocean-gradient text-white shadow-2xl z-50 flex flex-col">
        <div class="p-6 border-b border-white/10">
            <div class="flex items-center gap-3 mb-1">
                <div class="w-10 h-10 bg-white/20 rounded-xl flex items-center justify-center backdrop-blur-sm">
                    <i class="fas fa-water text-2xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-tight">AquaDoc</h1>
                    <p class="text-xs text-blue-100 opacity-80">AMDK Management System</p>
                </div>
            </div>
        </div>

        <nav class="flex-1 overflow-y-auto py-6 px-3 custom-scrollbar">
            <div class="mb-6">
                <p class="px-4 text-xs font-semibold text-blue-200 uppercase tracking-wider mb-3">Menu Utama</p>
                <a href="#" onclick="showSection('dashboard')" class="sidebar-item active flex items-center gap-3 px-4 py-3 rounded-lg mb-1" id="nav-dashboard">
                    <i class="fas fa-th-large w-5"></i>
                    <span class="font-medium">Dashboard</span>
                </a>
                <a href="#" onclick="showSection('documents')" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1" id="nav-documents">
                    <i class="fas fa-folder-open w-5"></i>
                    <span class="font-medium">Semua Dokumen</span>
                    <span class="ml-auto bg-white/20 text-xs px-2 py-1 rounded-full" id="doc-count">24</span>
                </a>
                <a href="#" onclick="showSection('upload')" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1" id="nav-upload">
                    <i class="fas fa-cloud-upload-alt w-5"></i>
                    <span class="font-medium">Upload Dokumen</span>
                </a>
            </div>

            <div class="mb-6">
                <p class="px-4 text-xs font-semibold text-blue-200 uppercase tracking-wider mb-3">Kategori AMDK</p>
                <a href="#" onclick="filterByCategory('izin')" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1">
                    <i class="fas fa-certificate w-5 text-yellow-300"></i>
                    <span>Izin & Perizinan</span>
                </a>
                <a href="#" onclick="filterByCategory('quality')" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1">
                    <i class="fas fa-flask w-5 text-green-300"></i>
                    <span>Uji Kualitas Air</span>
                </a>
                <a href="#" onclick="filterByCategory('production')" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1">
                    <i class="fas fa-industry w-5 text-orange-300"></i>
                    <span>Laporan Produksi</span>
                </a>
                <a href="#" onclick="filterByCategory('distribution')" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1">
                    <i class="fas fa-truck w-5 text-purple-300"></i>
                    <span>Distribusi</span>
                </a>
                <a href="#" onclick="filterByCategory('hr')" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1">
                    <i class="fas fa-users w-5 text-pink-300"></i>
                    <span>SDM & Pelatihan</span>
                </a>
            </div>

            <div>
                <p class="px-4 text-xs font-semibold text-blue-200 uppercase tracking-wider mb-3">Sistem</p>
                <a href="#" onclick="showSection('settings')" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1" id="nav-settings">
                    <i class="fas fa-cog w-5"></i>
                    <span>Pengaturan</span>
                </a>
                <a href="#" class="sidebar-item flex items-center gap-3 px-4 py-3 rounded-lg mb-1">
                    <i class="fas fa-sign-out-alt w-5"></i>
                    <span>Keluar</span>
                </a>
            </div>
        </nav>

        <div class="p-4 border-t border-white/10">
            <div class="flex items-center gap-3 bg-white/10 rounded-lg p-3">
                <img src="https://ui-avatars.com/api/?name=Admin+User&background=0D8ABC&color=fff" class="w-10 h-10 rounded-full border-2 border-white/30">
                <div class="flex-1 min-w-0">
                    <p class="text-sm font-semibold truncate">Administrator</p>
                    <p class="text-xs text-blue-200 truncate">admin@aquadoc.com</p>
                </div>
            </div>
        </div>
    </aside>

    <!-- Main Content -->
    <main class="ml-72 h-screen overflow-hidden flex flex-col">
        
        <!-- Header -->
        <header class="h-16 bg-white/80 backdrop-blur-md border-b border-slate-200 flex items-center justify-between px-8 sticky top-0 z-40">
            <div class="flex items-center gap-4 flex-1">
                <div class="relative w-96">
                    <i class="fas fa-search absolute left-3 top-1/2 -translate-y-1/2 text-slate-400"></i>
                    <input type="text" id="searchInput" placeholder="Cari dokumen, izin, laporan..." 
                           class="search-input w-full pl-10 pr-4 py-2 bg-slate-100 border-none rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all"
                           onkeyup="searchDocuments()">
                </div>
            </div>
            
            <div class="flex items-center gap-4">
                <button class="relative p-2 text-slate-600 hover:bg-slate-100 rounded-lg transition-colors">
                    <i class="fas fa-bell text-xl"></i>
                    <span class="absolute top-1 right-1 w-2 h-2 bg-red-500 rounded-full animate-pulse"></span>
                </button>
                <button class="relative p-2 text-slate-600 hover:bg-slate-100 rounded-lg transition-colors">
                    <i class="fas fa-envelope text-xl"></i>
                    <span class="absolute top-1 right-1 w-2 h-2 bg-blue-500 rounded-full"></span>
                </button>
                <div class="h-8 w-px bg-slate-200 mx-2"></div>
                <span class="text-sm text-slate-500" id="current-date"></span>
            </div>
        </header>

        <!-- Content Area -->
        <div class="flex-1 overflow-y-auto custom-scrollbar p-8" id="main-content">
            
            <!-- Dashboard Section -->
            <div id="dashboard-section" class="animate-slide-in">
                <div class="mb-8">
                    <h2 class="text-3xl font-bold text-slate-800 mb-2">Dashboard Dokumen</h2>
                    <p class="text-slate-500">Kelola dan pantau semua dokumen regulasi AMDK Anda</p>
                </div>

                <!-- Stats Cards -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100 card-hover">
                        <div class="flex justify-between items-start mb-4">
                            <div class="p-3 bg-blue-50 rounded-xl">
                                <i class="fas fa-file-alt text-2xl text-blue-600"></i>
                            </div>
                            <span class="text-xs font-semibold text-green-600 bg-green-50 px-2 py-1 rounded-full">+12%</span>
                        </div>
                        <h3 class="text-3xl font-bold text-slate-800 mb-1">1,284</h3>
                        <p class="text-sm text-slate-500">Total Dokumen</p>
                    </div>

                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100 card-hover">
                        <div class="flex justify-between items-start mb-4">
                            <div class="p-3 bg-yellow-50 rounded-xl">
                                <i class="fas fa-clock text-2xl text-yellow-600"></i>
                            </div>
                            <span class="text-xs font-semibold text-red-600 bg-red-50 px-2 py-1 rounded-full">5 urgent</span>
                        </div>
                        <h3 class="text-3xl font-bold text-slate-800 mb-1">23</h3>
                        <p class="text-sm text-slate-500">Menunggu Review</p>
                    </div>

                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100 card-hover">
                        <div class="flex justify-between items-start mb-4">
                            <div class="p-3 bg-green-50 rounded-xl">
                                <i class="fas fa-check-circle text-2xl text-green-600"></i>
                            </div>
                            <span class="text-xs font-semibold text-green-600 bg-green-50 px-2 py-1 rounded-full">98%</span>
                        </div>
                        <h3 class="text-3xl font-bold text-slate-800 mb-1">156</h3>
                        <p class="text-sm text-slate-500">Tervalidasi 2024</p>
                    </div>

                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100 card-hover">
                        <div class="flex justify-between items-start mb-4">
                            <div class="p-3 bg-purple-50 rounded-xl">
                                <i class="fas fa-exclamation-triangle text-2xl text-purple-600"></i>
                            </div>
                            <span class="text-xs font-semibold text-orange-600 bg-orange-50 px-2 py-1 rounded-full">Action</span>
                        </div>
                        <h3 class="text-3xl font-bold text-slate-800 mb-1">8</h3>
                        <p class="text-sm text-slate-500">Akan Kadaluarsa</p>
                    </div>
                </div>

                <!-- Charts & Recent -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 mb-8">
                    <div class="lg:col-span-2 bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
                        <div class="flex justify-between items-center mb-6">
                            <h3 class="text-lg font-bold text-slate-800">Aktivitas Dokumen</h3>
                            <select class="text-sm border-slate-200 rounded-lg bg-slate-50 px-3 py-1">
                                <option>7 Hari Terakhir</option>
                                <option>30 Hari Terakhir</option>
                                <option>Tahun Ini</option>
                            </select>
                        </div>
                        <canvas id="activityChart" height="200"></canvas>
                    </div>

                    <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
                        <h3 class="text-lg font-bold text-slate-800 mb-6">Distribusi Kategori</h3>
                        <canvas id="categoryChart" height="220"></canvas>
                    </div>
                </div>

                <!-- Recent Documents -->
                <div class="bg-white rounded-2xl shadow-sm border border-slate-100 overflow-hidden">
                    <div class="p-6 border-b border-slate-100 flex justify-between items-center">
                        <h3 class="text-lg font-bold text-slate-800">Dokumen Terbaru</h3>
                        <button onclick="showSection('documents')" class="text-sm text-blue-600 hover:text-blue-700 font-medium">
                            Lihat Semua <i class="fas fa-arrow-right ml-1"></i>
                        </button>
                    </div>
                    <div class="divide-y divide-slate-100" id="recent-documents-list">
                        <!-- Populated by JS -->
                    </div>
                </div>
            </div>

            <!-- Documents Section -->
            <div id="documents-section" class="hidden animate-slide-in">
                <div class="flex justify-between items-center mb-8">
                    <div>
                        <h2 class="text-3xl font-bold text-slate-800 mb-2">Database Dokumen</h2>
                        <p class="text-slate-500">Kelola dan akses semua dokumen perusahaan</p>
                    </div>
                    <button onclick="showSection('upload')" class="bg-blue-600 hover:bg-blue-700 text-white px-6 py-3 rounded-xl font-medium flex items-center gap-2 transition-all hover:shadow-lg hover:shadow-blue-500/30">
                        <i class="fas fa-plus"></i>
                        Upload Baru
                    </button>
                </div>

                <!-- Filters -->
                <div class="bg-white rounded-2xl p-4 shadow-sm border border-slate-100 mb-6 flex flex-wrap gap-4 items-center">
                    <div class="flex gap-2">
                        <button onclick="filterDocuments('all')" class="filter-btn active px-4 py-2 rounded-lg text-sm font-medium bg-blue-600 text-white transition-all" data-filter="all">
                            Semua
                        </button>
                        <button onclick="filterDocuments('izin')" class="filter-btn px-4 py-2 rounded-lg text-sm font-medium text-slate-600 hover:bg-slate-100 transition-all" data-filter="izin">
                            Perizinan
                        </button>
                        <button onclick="filterDocuments('quality')" class="filter-btn px-4 py-2 rounded-lg text-sm font-medium text-slate-600 hover:bg-slate-100 transition-all" data-filter="quality">
                            Kualitas
                        </button>
                        <button onclick="filterDocuments('production')" class="filter-btn px-4 py-2 rounded-lg text-sm font-medium text-slate-600 hover:bg-slate-100 transition-all" data-filter="production">
                            Produksi
                        </button>
                    </div>
                    <div class="h-6 w-px bg-slate-200"></div>
                    <select id="sortSelect" onchange="sortDocuments()" class="text-sm border-slate-200 rounded-lg bg-slate-50 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <option value="newest">Terbaru</option>
                        <option value="oldest">Terlama</option>
                        <option value="name">Nama A-Z</option>
                        <option value="size">Ukuran File</option>
                    </select>
                    <div class="ml-auto flex gap-2">
                        <button onclick="toggleView('grid')" class="p-2 rounded-lg bg-blue-100 text-blue-600" id="view-grid">
                            <i class="fas fa-th-large"></i>
                        </button>
                        <button onclick="toggleView('list')" class="p-2 rounded-lg text-slate-400 hover:bg-slate-100" id="view-list">
                            <i class="fas fa-list"></i>
                        </button>
                    </div>
                </div>

                <!-- Documents Grid -->
                <div id="documents-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                    <!-- Populated by JS -->
                </div>

                <!-- Pagination -->
                <div class="mt-8 flex justify-center gap-2">
                    <button class="px-4 py-2 rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50 disabled:opacity-50" disabled>
                        <i class="fas fa-chevron-left"></i>
                    </button>
                    <button class="px-4 py-2 rounded-lg bg-blue-600 text-white font-medium">1</button>
                    <button class="px-4 py-2 rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50">2</button>
                    <button class="px-4 py-2 rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50">3</button>
                    <button class="px-4 py-2 rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50">
                        <i class="fas fa-chevron-right"></i>
                    </button>
                </div>
            </div>

            <!-- Upload Section -->
            <div id="upload-section" class="hidden animate-slide-in">
                <div class="mb-8">
                    <h2 class="text-3xl font-bold text-slate-800 mb-2">Upload Dokumen Baru</h2>
                    <p class="text-slate-500">Tambahkan dokumen baru ke database AMDK</p>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <div class="lg:col-span-2">
                        <div class="bg-white rounded-2xl p-8 shadow-sm border border-slate-100 mb-6">
                            <div id="dropZone" class="drop-zone rounded-xl p-12 text-center cursor-pointer" onclick="document.getElementById('fileInput').click()">
                                <input type="file" id="fileInput" class="hidden" multiple onchange="handleFileSelect(event)">
                                <div class="w-20 h-20 bg-blue-50 rounded-full flex items-center justify-center mx-auto mb-4">
                                    <i class="fas fa-cloud-upload-alt text-4xl text-blue-600"></i>
                                </div>
                                <h3 class="text-xl font-semibold text-slate-800 mb-2">Drag & Drop file disini</h3>
                                <p class="text-slate-500 mb-4">atau klik untuk browse</p>
                                <p class="text-xs text-slate-400">Support: PDF, DOC, DOCX, XLS, XLSX, JPG, PNG (Max 50MB)</p>
                            </div>

                            <div id="fileList" class="mt-6 space-y-3 hidden">
                                <!-- File items will appear here -->
                            </div>
                        </div>

                        <div class="bg-white rounded-2xl p-8 shadow-sm border border-slate-100">
                            <h3 class="text-lg font-bold text-slate-800 mb-6">Detail Dokumen</h3>
                            <form id="uploadForm" class="space-y-6" onsubmit="handleUpload(event)">
                                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                                    <div>
                                        <label class="block text-sm font-medium text-slate-700 mb-2">Judul Dokumen</label>
                                        <input type="text" required class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all" placeholder="Contoh: Laporan Uji Lab Januari 2024">
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-slate-700 mb-2">Nomor Dokumen</label>
                                        <input type="text" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all" placeholder="Contoh: AMDK/2024/001">
                                    </div>
                                </div>

                                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                                    <div>
                                        <label class="block text-sm font-medium text-slate-700 mb-2">Kategori</label>
                                        <select required class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all">
                                            <option value="">Pilih Kategori</option>
                                            <option value="izin">Izin & Perizinan</option>
                                            <option value="quality">Uji Kualitas Air</option>
                                            <option value="production">Laporan Produksi</option>
                                            <option value="distribution">Distribusi</option>
                                            <option value="hr">SDM & Pelatihan</option>
                                            <option value="maintenance">Pemeliharaan</option>
                                        </select>
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-slate-700 mb-2">Tanggal Dokumen</label>
                                        <input type="date" required class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all">
                                    </div>
                                </div>

                                <div>
                                    <label class="block text-sm font-medium text-slate-700 mb-2">Departemen Terkait</label>
                                    <div class="flex flex-wrap gap-3">
                                        <label class="flex items-center gap-2 px-4 py-2 border border-slate-200 rounded-lg cursor-pointer hover:bg-slate-50 transition-colors">
                                            <input type="checkbox" class="rounded text-blue-600 focus:ring-blue-500">
                                            <span class="text-sm">Produksi</span>
                                        </label>
                                        <label class="flex items-center gap-2 px-4 py-2 border border-slate-200 rounded-lg cursor-pointer hover:bg-slate-50 transition-colors">
                                            <input type="checkbox" class="rounded text-blue-600 focus:ring-blue-500">
                                            <span class="text-sm">QC/Lab</span>
                                        </label>
                                        <label class="flex items-center gap-2 px-4 py-2 border border-slate-200 rounded-lg cursor-pointer hover:bg-slate-50 transition-colors">
                                            <input type="checkbox" class="rounded text-blue-600 focus:ring-blue-500">
                                            <span class="text-sm">Distribusi</span>
                                        </label>
                                        <label class="flex items-center gap-2 px-4 py-2 border border-slate-200 rounded-lg cursor-pointer hover:bg-slate-50 transition-colors">
                                            <input type="checkbox" class="rounded text-blue-600 focus:ring-blue-500">
                                            <span class="text-sm">HRD</span>
                                        </label>
                                    </div>
                                </div>

                                <div>
                                    <label class="block text-sm font-medium text-slate-700 mb-2">Deskripsi</label>
                                    <textarea rows="3" class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all" placeholder="Deskripsi singkat dokumen..."></textarea>
                                </div>

                                <div class="flex items-center gap-2">
                                    <input type="checkbox" id="confidential" class="rounded text-blue-600 focus:ring-blue-500">
                                    <label for="confidential" class="text-sm text-slate-700">Dokumen Rahasia (Confidential)</label>
                                </div>

                                <div class="flex gap-4 pt-4">
                                    <button type="button" onclick="showSection('documents')" class="px-6 py-3 border border-slate-200 rounded-xl text-slate-600 hover:bg-slate-50 font-medium transition-all">
                                        Batal
                                    </button>
                                    <button type="submit" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white px-6 py-3 rounded-xl font-medium transition-all hover:shadow-lg hover:shadow-blue-500/30 flex items-center justify-center gap-2">
                                        <i class="fas fa-save"></i>
                                        Simpan Dokumen
                                    </button>
                                </div>
                            </form>
                        </div>
                    </div>

                    <div class="lg:col-span-1">
                        <div class="bg-gradient-to-br from-blue-600 to-blue-800 rounded-2xl p-6 text-white mb-6">
                            <h3 class="font-bold text-lg mb-4"><i class="fas fa-lightbulb mr-2"></i>Tips Upload</h3>
                            <ul class="space-y-3 text-sm text-blue-100">
                                <li class="flex gap-2">
                                    <i class="fas fa-check-circle mt-0.5"></i>
                                    <span>Pastikan file tidak corrupt dan bisa dibuka</span>
                                </li>
                                <li class="flex gap-2">
                                    <i class="fas fa-check-circle mt-0.5"></i>
                                    <span>Gunakan nama file yang deskriptif</span>
                                </li>
                                <li class="flex gap-2">
                                    <i class="fas fa-check-circle mt-0.5"></i>
                                    <span>Untuk dokumen penting, aktifkan enkripsi</span>
                                </li>
                                <li class="flex gap-2">
                                    <i class="fas fa-check-circle mt-0.5"></i>
                                    <span>Tambahkan tag untuk memudahkan pencarian</span>
                                </li>
                            </ul>
                        </div>

                        <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
                            <h3 class="font-bold text-slate-800 mb-4">Template Dokumen</h3>
                            <div class="space-y-3">
                                <a href="#" class="flex items-center gap-3 p-3 rounded-lg hover:bg-slate-50 transition-colors group">
                                    <div class="w-10 h-10 bg-red-100 rounded-lg flex items-center justify-center group-hover:scale-110 transition-transform">
                                        <i class="fas fa-file-pdf text-red-600"></i>
                                    </div>
                                    <div class="flex-1">
                                        <p class="text-sm font-medium text-slate-800">Formulir Izin Edar</p>
                                        <p class="text-xs text-slate-500">PDF • 245 KB</p>
                                    </div>
                                    <i class="fas fa-download text-slate-400"></i>
                                </a>
                                <a href="#" class="flex items-center gap-3 p-3 rounded-lg hover:bg-slate-50 transition-colors group">
                                    <div class="w-10 h-10 bg-blue-100 rounded-lg flex items-center justify-center group-hover:scale-110 transition-transform">
                                        <i class="fas fa-file-word text-blue-600"></i>
                                    </div>
                                    <div class="flex-1">
                                        <p class="text-sm font-medium text-slate-800">Laporan Harian Produksi</p>
                                        <p class="text-xs text-slate-500">DOCX • 120 KB</p>
                                    </div>
                                    <i class="fas fa-download text-slate-400"></i>
                                </a>
                                <a href="#" class="flex items-center gap-3 p-3 rounded-lg hover:bg-slate-50 transition-colors group">
                                    <div class="w-10 h-10 bg-green-100 rounded-lg flex items-center justify-center group-hover:scale-110 transition-transform">
                                        <i class="fas fa-file-excel text-green-600"></i>
                                    </div>
                                    <div class="flex-1">
                                        <p class="text-sm font-medium text-slate-800">Checklist QC</p>
                                        <p class="text-xs text-slate-500">XLSX • 89 KB</p>
                                    </div>
                                    <i class="fas fa-download text-slate-400"></i>
                                </a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Settings Section -->
            <div id="settings-section" class="hidden animate-slide-in">
                <div class="mb-8">
                    <h2 class="text-3xl font-bold text-slate-800 mb-2">Pengaturan Sistem</h2>
                    <p class="text-slate-500">Konfigurasi aplikasi dan manajemen pengguna</p>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                    <div class="lg:col-span-2 space-y-6">
                        <div class="bg-white rounded-2xl p-8 shadow-sm border border-slate-100">
                            <h3 class="text-lg font-bold text-slate-800 mb-6 flex items-center gap-2">
                                <i class="fas fa-bell text-blue-600"></i>
                                Notifikasi
                            </h3>
                            <div class="space-y-4">
                                <div class="flex items-center justify-between p-4 bg-slate-50 rounded-xl">
                                    <div>
                                        <p class="font-medium text-slate-800">Peringatan Kadaluarsa</p>
                                        <p class="text-sm text-slate-500">Notifikasi 30 hari sebelum izin kadaluarsa</p>
                                    </div>
                                    <label class="relative inline-flex items-center cursor-pointer">
                                        <input type="checkbox" checked class="sr-only peer">
                                        <div class="w-11 h-6 bg-slate-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-blue-600"></div>
                                    </label>
                                </div>
                                <div class="flex items-center justify-between p-4 bg-slate-50 rounded-xl">
                                    <div>
                                        <p class="font-medium text-slate-800">Upload Dokumen Baru</p>
                                        <p class="text-sm text-slate-500">Email notifikasi saat ada dokumen baru</p>
                                    </div>
                                    <label class="relative inline-flex items-center cursor-pointer">
                                        <input type="checkbox" checked class="sr-only peer">
                                        <div class="w-11 h-6 bg-slate-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-blue-600"></div>
                                    </label>
                                </div>
                                <div class="flex items-center justify-between p-4 bg-slate-50 rounded-xl">
                                    <div>
                                        <p class="font-medium text-slate-800">Approval Request</p>
                                        <p class="text-sm text-slate-500">Notifikasi untuk persetujuan dokumen</p>
                                    </div>
                                    <label class="relative inline-flex items-center cursor-pointer">
                                        <input type="checkbox" class="sr-only peer">
                                        <div class="w-11 h-6 bg-slate-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-blue-600"></div>
                                    </label>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white rounded-2xl p-8 shadow-sm border border-slate-100">
                            <h3 class="text-lg font-bold text-slate-800 mb-6 flex items-center gap-2">
                                <i class="fas fa-shield-alt text-blue-600"></i>
                                Keamanan & Akses
                            </h3>
                            <div class="space-y-4">
                                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                    <div>
                                        <label class="block text-sm font-medium text-slate-700 mb-2">Password Policy</label>
                                        <select class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500">
                                            <option>Strong (8+ chars, symbols)</option>
                                            <option>Medium (8+ chars)</option>
                                            <option>Basic (6+ chars)</option>
                                        </select>
                                    </div>
                                    <div>
                                        <label class="block text-sm font-medium text-slate-700 mb-2">Session Timeout</label>
                                        <select class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500">
                                            <option>30 menit</option>
                                            <option selected>1 jam</option>
                                            <option>4 jam</option>
                                            <option>8 jam</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="flex items-center gap-2 mt-4">
                                    <input type="checkbox" checked class="rounded text-blue-600 focus:ring-blue-500">
                                    <span class="text-sm text-slate-700">Enkripsi dokumen confidential</span>
                                </div>
                                <div class="flex items-center gap-2">
                                    <input type="checkbox" checked class="rounded text-blue-600 focus:ring-blue-500">
                                    <span class="text-sm text-slate-700">Two-factor authentication untuk admin</span>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="space-y-6">
                        <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
                            <h3 class="font-bold text-slate-800 mb-4">Storage Usage</h3>
                            <div class="relative pt-1">
                                <div class="flex mb-2 items-center justify-between">
                                    <div>
                                        <span class="text-xs font-semibold inline-block py-1 px-2 uppercase rounded-full text-blue-600 bg-blue-200">
                                            Used
                                        </span>
                                    </div>
                                    <div class="text-right">
                                        <span class="text-xs font-semibold inline-block text-blue-600">
                                            75%
                                        </span>
                                    </div>
                                </div>
                                <div class="overflow-hidden h-2 mb-4 text-xs flex rounded bg-blue-100">
                                    <div style="width:75%" class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center bg-blue-600"></div>
                                </div>
                                <p class="text-sm text-slate-500">75 GB dari 100 GB terpakai</p>
                            </div>
                            <button class="w-full mt-4 px-4 py-2 border border-blue-600 text-blue-600 rounded-lg hover:bg-blue-50 transition-colors text-sm font-medium">
                                Upgrade Storage
                            </button>
                        </div>

                        <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-100">
                            <h3 class="font-bold text-slate-800 mb-4">Backup</h3>
                            <div class="space-y-3">
                                <div class="flex items-center justify-between">
                                    <span class="text-sm text-slate-600">Auto-backup</span>
                                    <span class="text-xs bg-green-100 text-green-700 px-2 py-1 rounded-full">Aktif</span>
                                </div>
                                <p class="text-xs text-slate-500">Backup terakhir: Hari ini, 03:00 WIB</p>
                                <button class="w-full px-4 py-2 bg-slate-100 hover:bg-slate-200 rounded-lg text-sm font-medium transition-colors">
                                    Backup Manual
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </main>

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-6 right-6 transform translate-y-20 opacity-0 transition-all duration-300 z-50">
        <div class="bg-slate-800 text-white px-6 py-4 rounded-xl shadow-2xl flex items-center gap-3">
            <i class="fas fa-check-circle text-green-400 text-xl"></i>
            <div>
                <h4 class="font-semibold" id="toast-title">Success</h4>
                <p class="text-sm text-slate-300" id="toast-message">Operation completed successfully</p>
            </div>
        </div>
    </div>

    <script>
        // Data Dokumen Sample
        const documents = [
            {
                id: 1,
                title: "Izin Edar AMDK Nomor 123/2024",
                category: "izin",
                type: "pdf",
                size: "2.4 MB",
                date: "2024-03-15",
                status: "active",
                owner: "BPOM",
                icon: "fa-certificate",
                color: "yellow"
            },
            {
                id: 2,
                title: "Hasil Uji Lab Mikrobiologi Q1",
                category: "quality",
                type: "pdf",
                size: "4.1 MB",
                date: "2024-03-14",
                status: "active",
                owner: "Lab QC",
                icon: "fa-flask",
                color: "green"
            },
            {
                id: 3,
                title: "Laporan Produksi Bulan Februari",
                category: "production",
                type: "xlsx",
                size: "1.2 MB",
                date: "2024-03-10",
                status: "review",
                owner: "Produksi",
                icon: "fa-industry",
                color: "orange"
            },
            {
                id: 4,
                title: "Sertifikat Halal MUI",
                category: "izin",
                type: "pdf",
                size: "3.5 MB",
                date: "2024-03-08",
                status: "active",
                owner: "HRD",
                icon: "fa-certificate",
                color: "yellow"
            },
            {
                id: 5,
                title: "Jadwal Distribusi Area Jakarta",
                category: "distribution",
                type: "xlsx",
                size: "890 KB",
                date: "2024-03-12",
                status: "active",
                owner: "Logistik",
                icon: "fa-truck",
                color: "purple"
            },
            {
                id: 6,
                title: "Training HACCP Karyawan Baru",
                category: "hr",
                type: "pptx",
                size: "12.5 MB",
                date: "2024-03-05",
                status: "active",
                owner: "HRD",
                icon: "fa-users",
                color: "pink"
            },
            {
                id: 7,
                title: "Analisis Kualitas Air Sumur",
                category: "quality",
                type: "pdf",
                size: "5.2 MB",
                date: "2024-03-01",
                status: "expired",
                owner: "Lab QC",
                icon: "fa-flask",
                color: "green"
            },
            {
                id: 8,
                title: "Perawatan Mesin RO Unit 1",
                category: "maintenance",
                type: "pdf",
                size: "1.8 MB",
                date: "2024-02-28",
                status: "active",
                owner: "Maintenance",
                icon: "fa-wrench",
                color: "gray"
            }
        ];

        let currentFilter = 'all';
        let currentView = 'grid';

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            updateDate();
            renderRecentDocuments();
            renderDocuments();
            initCharts();
            setupDragAndDrop();
        });

        function updateDate() {
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('current-date').textContent = new Date().toLocaleDateString('id-ID', options);
        }

        function showSection(sectionName) {
            // Hide all sections
            document.getElementById('dashboard-section').classList.add('hidden');
            document.getElementById('documents-section').classList.add('hidden');
            document.getElementById('upload-section').classList.add('hidden');
            document.getElementById('settings-section').classList.add('hidden');
            
            // Remove active class from all nav items
            document.querySelectorAll('.sidebar-item').forEach(item => {
                item.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionName + '-section').classList.remove('hidden');
            document.getElementById('nav-' + sectionName).classList.add('active');
            
            // Update document count
            if (sectionName === 'documents') {
                document.getElementById('doc-count').textContent = documents.length;
            }
        }

        function renderRecentDocuments() {
            const recentDocs = documents.slice(0, 5);
            const container = document.getElementById('recent-documents-list');
            
            container.innerHTML = recentDocs.map(doc => `
                <div class="p-4 flex items-center gap-4 hover:bg-slate-50 transition-colors cursor-pointer group">
                    <div class="w-12 h-12 rounded-xl bg-${doc.color}-100 flex items-center justify-center flex-shrink-0 group-hover:scale-110 transition-transform">
                        <i class="fas ${getFileIcon(doc.type)} text-${doc.color}-600 text-xl"></i>
                    </div>
                    <div class="flex-1 min-w-0">
                        <h4 class="text-sm font-semibold text-slate-800 truncate group-hover:text-blue-600 transition-colors">${doc.title}</h4>
                        <p class="text-xs text-slate-500 mt-0.5">${doc.category} • ${doc.date}</p>
                    </div>
                    <span class="px-3 py-1 rounded-full text-xs font-medium ${getStatusStyle(doc.status)}">
                        ${getStatusLabel(doc.status)}
                    </span>
                </div>
            `).join('');
        }

        function renderDocuments() {
            const container = document.getElementById('documents-grid');
            const filtered = currentFilter === 'all' 
                ? documents 
                : documents.filter(d => d.category === currentFilter);
            
            if (filtered.length === 0) {
                container.innerHTML = `
                    <div class="col-span-full text-center py-12">
                        <div class="w-24 h-24 bg-slate-100 rounded-full flex items-center justify-center mx-auto mb-4">
                            <i class="fas fa-search text-4xl text-slate-400"></i>
                        </div>
                        <h3 class="text-lg font-medium text-slate-600">Tidak ada dokumen ditemukan</h3>
                        <p class="text-slate-500 mt-1">Coba ubah filter atau kata kunci pencarian</p>
                    </div>
                `;
                return;
            }
            
            container.innerHTML = filtered.map(doc => `
                <div class="document-card bg-white rounded-2xl p-6 shadow-sm border border-slate-100 group cursor-pointer" onclick="viewDocument(${doc.id})">
                    <div class="flex justify-between items-start mb-4">
                        <div class="w-14 h-14 rounded-xl bg-${doc.color}-100 flex items-center justify-center group-hover:scale-110 transition-transform duration-300">
                            <i class="fas ${getFileIcon(doc.type)} text-2xl text-${doc.color}-600"></i>
                        </div>
                        <div class="relative">
                            <button onclick="event.stopPropagation(); toggleMenu(${doc.id})" class="p-2 hover:bg-slate-100 rounded-lg transition-colors">
                                <i class="fas fa-ellipsis-v text-slate-400"></i>
                            </button>
                            <div id="menu-${doc.id}" class="hidden absolute right-0 top-full mt-1 w-48 bg-white rounded-xl shadow-lg border border-slate-100 py-2 z-10">
                                <a href="#" class="block px-4 py-2 text-sm text-slate-700 hover:bg-slate-50"><i class="fas fa-eye mr-2"></i>Preview</a>
                                <a href="#" class="block px-4 py-2 text-sm text-slate-700 hover:bg-slate-50"><i class="fas fa-download mr-2"></i>Download</a>
                                <a href="#" class="block px-4 py-2 text-sm text-slate-700 hover:bg-slate-50"><i class="fas fa-share mr-2"></i>Share</a>
                                <div class="border-t border-slate-100 my-1"></div>
                                <a href="#" class="block px-4 py-2 text-sm text-red-600 hover:bg-red-50"><i class="fas fa-trash mr-2"></i>Hapus</a>
                            </div>
                        </div>
                    </div>
                    
                    <h3 class="font-bold text-slate-800 mb-2 line-clamp-2 group-hover:text-blue-600 transition-colors">${doc.title}</h3>
                    
                    <div class="flex items-center gap-2 mb-4">
                        <span class="px-2 py-1 rounded-md text-xs font-medium bg-${doc.color}-100 text-${doc.color}-700">
                            ${doc.category.toUpperCase()}
                        </span>
                        <span class="text-xs text-slate-400">${doc.size}</span>
                    </div>
                    
                    <div class="flex items-center justify-between pt-4 border-t border-slate-100">
                        <div class="flex items-center gap-2">
                            <img src="https://ui-avatars.com/api/?name=${doc.owner}&background=random&size=24" class="w-6 h-6 rounded-full">
                            <span class="text-xs text-slate-500">${doc.owner}</span>
                        </div>
                        <span class="text-xs text-slate-400">${doc.date}</span>
                    </div>
                    
                    ${doc.status === 'expired' ? '<div class="absolute top-4 right-4 w-3 h-3 bg-red-500 rounded-full status-badge"></div>' : ''}
                </div>
            `).join('');
        }

        function getFileIcon(type) {
            const icons = {
                'pdf': 'fa-file-pdf',
                'docx': 'fa-file-word',
                'xlsx': 'fa-file-excel',
                'pptx': 'fa-file-powerpoint',
                'jpg': 'fa-file-image',
                'png': 'fa-file-image'
            };
            return icons[type] || 'fa-file';
        }

        function getStatusStyle(status) {
            const styles = {
                'active': 'bg-green-100 text-green-700',
                'review': 'bg-yellow-100 text-yellow-700',
                'expired': 'bg-red-100 text-red-700'
            };
            return styles[status] || 'bg-slate-100 text-slate-700';
        }

        function getStatusLabel(status) {
            const labels = {
                'active': 'Aktif',
                'review': 'Review',
                'expired': 'Kadaluarsa'
            };
            return labels[status] || status;
        }

        function filterDocuments(category) {
            currentFilter = category;
            
            // Update button styles
            document.querySelectorAll('.filter-btn').forEach(btn => {
                if (btn.dataset.filter === category) {
                    btn.classList.remove('text-slate-600', 'hover:bg-slate-100');
                    btn.classList.add('bg-blue-600', 'text-white');
                } else {
                    btn.classList.add('text-slate-600', 'hover:bg-slate-100');
                    btn.classList.remove('bg-blue-600', 'text-white');
                }
            });
            
            renderDocuments();
        }

        function filterByCategory(category) {
            showSection('documents');
            filterDocuments(category);
        }

        function toggleView(view) {
            currentView = view;
            const grid = document.getElementById('documents-grid');
            const btnGrid = document.getElementById('view-grid');
            const btnList = document.getElementById('view-list');
            
            if (view === 'list') {
                grid.classList.remove('grid-cols-1', 'md:grid-cols-2', 'lg:grid-cols-3', 'xl:grid-cols-4');
                grid.classList.add('grid-cols-1');
                btnList.classList.add('bg-blue-100', 'text-blue-600');
                btnList.classList.remove('text-slate-400');
                btnGrid.classList.remove('bg-blue-100', 'text-blue-600');
                btnGrid.classList.add('text-slate-400');
            } else {
                grid.classList.add('grid-cols-1', 'md:grid-cols-2', 'lg:grid-cols-3', 'xl:grid-cols-4');
                grid.classList.remove('grid-cols-1');
                btnGrid.classList.add('bg-blue-100', 'text-blue-600');
                btnGrid.classList.remove('text-slate-400');
                btnList.classList.remove('bg-blue-100', 'text-blue-600');
                btnList.classList.add('text-slate-400');
            }
        }

        function searchDocuments() {
            const query = document.getElementById('searchInput').value.toLowerCase();
            const container = document.getElementById('documents-grid');
            
            const filtered = documents.filter(doc => 
                doc.title.toLowerCase().includes(query) ||
                doc.category.toLowerCase().includes(query) ||
                doc.owner.toLowerCase().includes(query)
            );
            
            if (filtered.length === 0) {
                container.innerHTML = `
                    <div class="col-span-full text-center py-12">
                        <div class="w-24 h-24 bg-slate-100 rounded-full flex items-center justify-center mx-auto mb-4">
                            <i class="fas fa-search text-4xl text-slate-400"></i>
                        </div>
                        <h3 class="text-lg font-medium text-slate-600">Tidak ada dokumen ditemukan</h3>
                        <p class="text-slate-500 mt-1">Coba kata kunci lain</p>
                    </div>
                `;
                return;
            }
            
            container.innerHTML = filtered.map(doc => `
                <div class="document-card bg-white rounded-2xl p-6 shadow-sm border border-slate-100 group cursor-pointer">
                    <div class="flex justify-between items-start mb-4">
                        <div class="w-14 h-14 rounded-xl bg-${doc.color}-100 flex items-center justify-center">
                            <i class="fas ${getFileIcon(doc.type)} text-2xl text-${doc.color}-600"></i>
                        </div>
                    </div>
                    <h3 class="font-bold text-slate-800 mb-2 line-clamp-2">${doc.title}</h3>
                    <div class="flex items-center gap-2 mb-4">
                        <span class="px-2 py-1 rounded-md text-xs font-medium bg-${doc.color}-100 text-${doc.color}-700">
                            ${doc.category.toUpperCase()}
                        </span>
                    </div>
                    <div class="flex items-center justify-between pt-4 border-t border-slate-100">
                        <span class="text-xs text-slate-500">${doc.date}</span>
                        <span class="text-xs text-slate-400">${doc.size}</span>
                    </div>
                </div>
            `).join('');
        }

        function toggleMenu(id) {
            const menu = document.getElementById(`menu-${id}`);
            const isHidden = menu.classList.contains('hidden');
            
            // Close all menus
            document.querySelectorAll('[id^="menu-"]').forEach(m => m.classList.add('hidden'));
            
            if (isHidden) {
                menu.classList.remove('hidden');
            }
        }

        function viewDocument(id) {
            showToast('Info', 'Membuka dokumen...');
        }

        function setupDragAndDrop() {
            const dropZone = document.getElementById('dropZone');
            
            ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                dropZone.addEventListener(eventName, preventDefaults, false);
            });
            
            function preventDefaults(e) {
                e.preventDefault();
                e.stopPropagation();
            }
            
            ['dragenter', 'dragover'].forEach(eventName => {
                dropZone.addEventListener(eventName, () => {
                    dropZone.classList.add('dragover');
                });
            });
            
            ['dragleave', 'drop'].forEach(eventName => {
                dropZone.addEventListener(eventName, () => {
                    dropZone.classList.remove('dragover');
                });
            });
            
            dropZone.addEventListener('drop', handleDrop);
        }

        function handleDrop(e) {
            const dt = e.dataTransfer;
            const files = dt.files;
            handleFiles(files);
        }

        function handleFileSelect(e) {
            const files = e.target.files;
            handleFiles(files);
        }

        function handleFiles(files) {
            const fileList = document.getElementById('fileList');
            fileList.classList.remove('hidden');
            fileList.innerHTML = '';
            
            Array.from(files).forEach(file => {
                const div = document.createElement('div');
                div.className = 'flex items-center gap-3 p-3 bg-slate-50 rounded-lg border border-slate-200';
                div.innerHTML = `
                    <i class="fas fa-file text-blue-600"></i>
                    <div class="flex-1 min-w-0">
                        <p class="text-sm font-medium text-slate-800 truncate">${file.name}</p>
                        <p class="text-xs text-slate-500">${(file.size / 1024 / 1024).toFixed(2)} MB</p>
                    </div>
                    <button onclick="this.parentElement.remove()" class="text-red-500 hover:text-red-700">
                        <i class="fas fa-times"></i>
                    </button>
                `;
                fileList.appendChild(div);
            });
        }

        function handleUpload(e) {
            e.preventDefault();
            showToast('Success', 'Dokumen berhasil diupload!');
            setTimeout(() => {
                showSection('documents');
            }, 1500);
        }

        function showToast(title, message) {
            const toast = document.getElementById('toast');
            document.getElementById('toast-title').textContent = title;
            document.getElementById('toast-message').textContent = message;
            
            toast.classList.remove('translate-y-20', 'opacity-0');
            
            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }

        function initCharts() {
            // Activity Chart
            const ctx1 = document.getElementById('activityChart').getContext('2d');
            new Chart(ctx1, {
                type: 'line',
                data: {
                    labels: ['Sen', 'Sel', 'Rab', 'Kam', 'Jum', 'Sab', 'Min'],
                    datasets: [{
                        label: 'Upload',
                        data: [12, 19, 15, 25, 22, 30, 28],
                        borderColor: '#0066cc',
                        backgroundColor: 'rgba(0, 102, 204, 0.1)',
                        tension: 0.4,
                        fill: true
                    }, {
                        label: 'Download',
                        data: [8, 15, 12, 18, 20, 25, 22],
                        borderColor: '#00d4aa',
                        backgroundColor: 'rgba(0, 212, 170, 0.1)',
                        tension: 0.4,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true,
                            grid: {
                                color: 'rgba(0,0,0,0.05)'
                            }
                        },
                        x: {
                            grid: {
                                display: false
                            }
                        }
                    }
                }
            });

            // Category Chart
            const ctx2 = document.getElementById('categoryChart').getContext('2d');
            new Chart(ctx2, {
                type: 'doughnut',
                data: {
                    labels: ['Izin', 'Kualitas', 'Produksi', 'Distribusi', 'SDM'],
                    datasets: [{
                        data: [30, 25, 20, 15, 10],
                        backgroundColor: [
                            '#fbbf24',
                            '#34d399',
                            '#fb923c',
                            '#a78bfa',
                            '#f472b6'
                        ],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    cutout: '70%',
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                boxWidth: 12,
                                padding: 15
                            }
                        }
                    }
                }
            });
        }

        // Close menus when clicking outside
        document.addEventListener('click', function(e) {
            if (!e.target.closest('.relative')) {
                document.querySelectorAll('[id^="menu-"]').forEach(m => m.classList.add('hidden'));
            }
        });
    </script>
</body>
</html>
