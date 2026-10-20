<!DOCTYPE html>
<html lang="id" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PackStok UMKM - Manajemen Stok Kemasan</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts (Inter) -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- FontAwesome CDN -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            800: '#166534',
                            900: '#14532d',
                        }
                    }
                }
            }
        }
    </script>

    <style>
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="h-full text-slate-800 antialiased flex flex-col md:flex-row bg-slate-50 font-sans">

    <!-- Mobile Header Navigation -->
    <div class="md:hidden bg-slate-900 text-white p-4 flex justify-between items-center sticky top-0 z-40 shadow-md">
        <div class="flex items-center space-x-3">
            <div class="bg-gradient-to-tr from-emerald-500 to-teal-400 p-2 rounded-lg text-white">
                <i class="fa-solid font-bold fa-box-archive text-lg"></i>
            </div>
            <div>
                <h1 class="font-bold text-base leading-none">PackStok</h1>
                <span class="text-xs text-emerald-400 font-medium">Stok Kemasan UMKM</span>
            </div>
        </div>
        <div class="flex items-center space-x-2">
            <button onclick="confirmResetAll()" title="Kosongkan Semua Data" class="bg-rose-600/80 hover:bg-rose-600 text-white p-2 rounded-lg text-xs flex items-center space-x-1">
                <i class="fa-solid fa-trash-can"></i>
            </button>
            <button id="mobileMenuBtn" class="text-slate-300 hover:text-white focus:outline-none p-1">
                <i class="fa-solid fa-bars text-xl"></i>
            </button>
        </div>
    </div>

    <!-- Sidebar Navigation -->
    <aside id="sidebar" class="fixed inset-y-0 left-0 transform -translate-x-full md:translate-x-0 md:static transition-transform duration-200 ease-in-out z-50 w-64 bg-slate-900 text-slate-300 flex flex-col justify-between shadow-xl">
        <div>
            <!-- Sidebar Header -->
            <div class="p-6 hidden md:flex items-center space-x-3 border-b border-slate-800">
                <div class="bg-gradient-to-tr from-emerald-500 to-teal-400 p-2.5 rounded-xl text-white shadow-lg shadow-emerald-500/20">
                    <i class="fa-solid fa-box-archive text-xl"></i>
                </div>
                <div>
                    <h1 class="font-bold text-lg text-white tracking-wide">PackStok</h1>
                    <span class="text-xs text-emerald-400 font-medium bg-emerald-950/80 px-2 py-0.5 rounded-full border border-emerald-800/50">Stok Kemasan</span>
                </div>
            </div>

            <!-- Mobile Sidebar Header Close -->
            <div class="p-4 md:hidden flex justify-between items-center border-b border-slate-800">
                <span class="font-semibold text-white">Menu Navigasi</span>
                <button id="closeMobileMenuBtn" class="text-slate-400 hover:text-white">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>

            <!-- Navigation Links -->
            <nav class="p-4 space-y-1.5">
                <button onclick="switchTab('dashboard')" id="nav-dashboard" class="nav-btn active w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all duration-150 text-white bg-emerald-600 shadow-lg shadow-emerald-600/30">
                    <i class="fa-solid fa-chart-pie w-5 text-center"></i>
                    <span>Dashboard</span>
                </button>

                <button onclick="switchTab('inventory')" id="nav-inventory" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all duration-150 hover:bg-slate-800 hover:text-white text-slate-400">
                    <i class="fa-solid fa-boxes-stacked w-5 text-center"></i>
                    <span>Daftar Stok Kemasan</span>
                </button>

                <button onclick="switchTab('history')" id="nav-history" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all duration-150 hover:bg-slate-800 hover:text-white text-slate-400">
                    <i class="fa-solid fa-clock-rotate-left w-5 text-center"></i>
                    <span>Riwayat Mutasi</span>
                </button>

                <button onclick="switchTab('calculator')" id="nav-calculator" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all duration-150 hover:bg-slate-800 hover:text-white text-slate-400">
                    <i class="fa-solid fa-calculator w-5 text-center"></i>
                    <span>Kalkulator Estimasi</span>
                </button>
            </nav>
        </div>

        <!-- Quick Daily Update Button & Demo / Reset Actions -->
        <div class="p-4 space-y-2 border-t border-slate-800">
            <button onclick="openDailyUpdateModal()" class="w-full bg-gradient-to-r from-emerald-500 to-teal-500 hover:from-emerald-600 hover:to-teal-600 text-white font-medium py-2.5 px-4 rounded-xl shadow-md flex items-center justify-center space-x-2 transition text-sm">
                <i class="fa-solid fa-bolt"></i>
                <span>Update Stok Harian</span>
            </button>
            
            <button onclick="confirmResetAll()" class="w-full bg-rose-950/50 border border-rose-800/40 hover:bg-rose-900/60 text-rose-300 hover:text-white text-xs py-2 px-3 rounded-xl flex items-center justify-center space-x-2 transition">
                <i class="fa-solid fa-trash-can text-xs"></i>
                <span>Reset / Kosongkan Data</span>
            </button>
            
            <button onclick="loadSampleData()" class="w-full bg-slate-800 hover:bg-slate-700 text-slate-400 hover:text-slate-200 text-xs py-2 px-3 rounded-xl flex items-center justify-center space-x-2 transition">
                <i class="fa-solid fa-rotate text-xs"></i>
                <span>Muat Contoh Data Demo</span>
            </button>
        </div>
    </aside>

    <!-- Sidebar Overlay for Mobile -->
    <div id="sidebarOverlay" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-40 hidden md:hidden"></div>

    <!-- Main Content Area -->
    <main class="flex-1 flex flex-col h-screen overflow-y-auto">
        <!-- Top App Bar -->
        <header class="bg-white border-b border-slate-200 px-4 md:px-8 py-3.5 flex justify-between items-center sticky top-0 z-10 shadow-sm">
            <div>
                <h2 id="pageTitle" class="text-lg md:text-xl font-bold text-slate-800">Dashboard Overview</h2>
                <p class="text-xs text-slate-500 hidden sm:block">Kelola dan pantau persediaan kemasan usaha Anda secara realtime.</p>
            </div>
            <div class="flex items-center space-x-3">
                <!-- Status Auto-Saved Indicator -->
                <div id="saveIndicator" class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-semibold bg-emerald-50 text-emerald-700 border border-emerald-200/80 shadow-xs transition-all duration-300">
                    <span id="saveDot" class="w-2 h-2 mr-2 bg-emerald-500 rounded-full animate-pulse"></span>
                    <span id="saveText">Auto-saved (Permanen)</span>
                </div>

                <!-- Reset All Button in Desktop Topbar -->
                <button onclick="confirmResetAll()" title="Hapus seluruh data dan mulai dari awal" class="hidden md:flex items-center space-x-1.5 px-3 py-1.5 text-xs font-medium text-rose-700 bg-rose-50 hover:bg-rose-100 border border-rose-200 rounded-xl transition">
                    <i class="fa-solid fa-trash-arrow-up"></i>
                    <span>Reset All</span>
                </button>
            </div>
        </header>

        <!-- Main Content Sections -->
        <div class="p-4 md:p-8 space-y-6 flex-1">

            <!-- TAB 1: DASHBOARD -->
            <section id="tab-dashboard" class="tab-content space-y-6">
                <!-- Stat Cards Grid -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 md:gap-6">
                    <!-- Total Varian -->
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm hover:shadow-md transition">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Total Varian Kemasan</p>
                                <h3 class="text-2xl font-bold text-slate-800 mt-1" id="statTotalVarian">0</h3>
                            </div>
                            <div class="p-3 bg-blue-50 text-blue-600 rounded-xl">
                                <i class="fa-solid fa-boxes-packing text-lg"></i>
                            </div>
                        </div>
                        <p class="text-xs text-slate-400 mt-3"><span class="text-blue-600 font-medium" id="statKategoriCount">0</span> Kategori Terdaftar</p>
                    </div>

                    <!-- Total Item Tersisa -->
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm hover:shadow-md transition">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Total Stok Tersisa</p>
                                <h3 class="text-2xl font-bold text-slate-800 mt-1" id="statTotalStok">0</h3>
                            </div>
                            <div class="p-3 bg-emerald-50 text-emerald-600 rounded-xl">
                                <i class="fa-solid fa-cubes text-lg"></i>
                            </div>
                        </div>
                        <p class="text-xs text-slate-400 mt-3">Unit fisik kemasan di gudang</p>
                    </div>

                    <!-- Peringatan Stok Menipis -->
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm hover:shadow-md transition">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Perlu Reorder</p>
                                <h3 class="text-2xl font-bold text-amber-600 mt-1" id="statLowStock">0</h3>
                            </div>
                            <div class="p-3 bg-amber-50 text-amber-600 rounded-xl">
                                <i class="fa-solid fa-triangle-exclamation text-lg"></i>
                            </div>
                        </div>
                        <p class="text-xs text-amber-600/80 mt-3 font-medium">Stok &le; batas minimum</p>
                    </div>

                    <!-- Estimasi Nilai Stok -->
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm hover:shadow-md transition">
                        <div class="flex justify-between items-start">
                            <div>
                                <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">Nilai Aset Kemasan</p>
                                <h3 class="text-xl font-bold text-slate-800 mt-1" id="statTotalAsset">Rp 0</h3>
                            </div>
                            <div class="p-3 bg-indigo-50 text-indigo-600 rounded-xl">
                                <i class="fa-solid fa-wallet text-lg"></i>
                            </div>
                        </div>
                        <p class="text-xs text-slate-400 mt-3">Kalkulasi modal per unit</p>
                    </div>
                </div>

                <!-- Alert Warning Banner if low stock exists -->
                <div id="lowStockBanner" class="hidden bg-amber-50 border border-amber-200 rounded-2xl p-4 flex items-start space-x-3 text-amber-800">
                    <i class="fa-solid fa-circle-exclamation text-amber-500 text-xl mt-0.5"></i>
                    <div class="flex-1">
                        <h4 class="font-semibold text-sm">Peringatan: Stok kemasan menipis!</h4>
                        <p class="text-xs text-amber-700 mt-0.5" id="lowStockBannerText">Beberapa kemasan telah mendekati batas minimum reorder.</p>
                    </div>
                    <button onclick="switchTab('inventory'); filterLowStockOnly();" class="text-xs font-medium bg-amber-200 hover:bg-amber-300 px-3 py-1.5 rounded-lg transition text-amber-900">
                        Lihat Item
                    </button>
                </div>

                <!-- Visual Charts Section -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Distribution Chart -->
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm lg:col-span-1 flex flex-col justify-between">
                        <div>
                            <h3 class="text-base font-bold text-slate-800 mb-1">Komposisi Stok</h3>
                            <p class="text-xs text-slate-400 mb-4">Persentase stok berdasarkan kategori</p>
                        </div>
                        <div class="relative flex items-center justify-center h-64">
                            <canvas id="categoryChart"></canvas>
                        </div>
                    </div>

                    <!-- Stock Levels Bar Chart -->
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm lg:col-span-2 flex flex-col justify-between">
                        <div class="flex justify-between items-center mb-4">
                            <div>
                                <h3 class="text-base font-bold text-slate-800">Stok Saat Ini vs Batas Minimum</h3>
                                <p class="text-xs text-slate-400">Monitoring kecukupan stok per jenis kemasan</p>
                            </div>
                        </div>
                        <div class="relative h-64 w-full">
                            <canvas id="stockBarChart"></canvas>
                        </div>
                    </div>
                </div>

                <!-- Recent Activity Overview Table -->
                <div class="bg-white rounded-2xl border border-slate-100 shadow-sm overflow-hidden">
                    <div class="p-5 border-b border-slate-100 flex justify-between items-center">
                        <h3 class="text-base font-bold text-slate-800">Aktivitas Mutasi Terakhir</h3>
                        <button onclick="switchTab('history')" class="text-xs text-emerald-600 hover:text-emerald-700 font-semibold flex items-center">
                            Lihat Semua <i class="fa-solid fa-arrow-right ml-1"></i>
                        </button>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs text-slate-600">
                            <thead class="bg-slate-50 text-slate-500 uppercase font-semibold border-b border-slate-100">
                                <tr>
                                    <th class="px-5 py-3">Waktu</th>
                                    <th class="px-5 py-3">Kemasan</th>
                                    <th class="px-5 py-3">Tipe</th>
                                    <th class="px-5 py-3 text-right">Jumlah</th>
                                    <th class="px-5 py-3">Keterangan</th>
                                </tr>
                            </thead>
                            <tbody id="recentLogsTable" class="divide-y divide-slate-100">
                                <!-- Dynamic Recent Logs -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- TAB 2: INVENTORY MANAGEMENT -->
            <section id="tab-inventory" class="tab-content space-y-6 hidden">
                <!-- Action & Filter Bar -->
                <div class="flex flex-col md:flex-row justify-between items-stretch md:items-center gap-4 bg-white p-4 rounded-2xl border border-slate-100 shadow-sm">
                    <!-- Search & Category Filter -->
                    <div class="flex flex-col sm:flex-row gap-3 flex-1">
                        <div class="relative flex-1">
                            <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
                            <input type="text" id="searchInput" oninput="renderInventoryTable()" placeholder="Cari kemasan, supplier, ukuran..." class="w-full pl-10 pr-4 py-2 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                        </div>
                        <select id="categoryFilter" onchange="renderInventoryTable()" class="bg-slate-50 border border-slate-200 text-slate-700 text-sm rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                            <option value="ALL">Semua Kategori</option>
                        </select>
                        <select id="statusFilter" onchange="renderInventoryTable()" class="bg-slate-50 border border-slate-200 text-slate-700 text-sm rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                            <option value="ALL">Semua Status</option>
                            <option value="SAFE">Aman</option>
                            <option value="LOW">Menipis / Reorder</option>
                            <option value="EMPTY">Habis</option>
                        </select>
                    </div>

                    <!-- Action Buttons -->
                    <div class="flex items-center gap-2">
                        <button onclick="openModal('add')" class="bg-emerald-600 hover:bg-emerald-700 text-white font-medium px-4 py-2 rounded-xl text-sm shadow-sm flex items-center justify-center space-x-2 transition">
                            <i class="fa-solid fa-plus text-xs"></i>
                            <span>Tambah Kemasan</span>
                        </button>
                    </div>
                </div>

                <!-- Inventory Table -->
                <div class="bg-white rounded-2xl border border-slate-100 shadow-sm overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm text-slate-600">
                            <thead class="bg-slate-50 text-slate-500 uppercase font-semibold text-xs border-b border-slate-100">
                                <tr>
                                    <th class="px-5 py-3.5">Kemasan & Detail</th>
                                    <th class="px-5 py-3.5">Kategori</th>
                                    <th class="px-5 py-3.5 text-center">Stok Saat Ini</th>
                                    <th class="px-5 py-3.5 text-center">Batas Min.</th>
                                    <th class="px-5 py-3.5 text-right">Harga / Pcs</th>
                                    <th class="px-5 py-3.5">Supplier</th>
                                    <th class="px-5 py-3.5 text-center">Status</th>
                                    <th class="px-5 py-3.5 text-center">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="inventoryTable" class="divide-y divide-slate-100">
                                <!-- Dynamic Items Rendered Here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- TAB 3: TRANSACTION LOGS -->
            <section id="tab-history" class="tab-content space-y-6 hidden">
                <!-- Filters & Export Header -->
                <div class="flex flex-col md:flex-row justify-between items-stretch md:items-center gap-4 bg-white p-4 rounded-2xl border border-slate-100 shadow-sm">
                    <div class="flex flex-col sm:flex-row gap-3 flex-1">
                        <div class="relative flex-1">
                            <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
                            <input type="text" id="logSearchInput" oninput="renderLogTable()" placeholder="Cari transaksi / keterangan..." class="w-full pl-10 pr-4 py-2 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                        </div>
                        <input type="date" id="logDateFilter" onchange="renderLogTable()" class="bg-slate-50 border border-slate-200 text-slate-700 text-sm rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                        <select id="logTypeFilter" onchange="renderLogTable()" class="bg-slate-50 border border-slate-200 text-slate-700 text-sm rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                            <option value="ALL">Semua Jenis Mutasi</option>
                            <option value="IN">Stok Masuk (Restock)</option>
                            <option value="OUT">Stok Keluar (Pemakaian)</option>
                        </select>
                    </div>

                    <div class="flex items-center gap-2">
                        <button onclick="exportToCSV()" class="bg-slate-800 hover:bg-slate-900 text-white font-medium px-4 py-2 rounded-xl text-sm shadow-sm flex items-center justify-center space-x-2 transition">
                            <i class="fa-solid fa-file-csv text-emerald-400"></i>
                            <span>Ekspor CSV</span>
                        </button>
                    </div>
                </div>

                <!-- Logs Table -->
                <div class="bg-white rounded-2xl border border-slate-100 shadow-sm overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-sm text-slate-600">
                            <thead class="bg-slate-50 text-slate-500 uppercase font-semibold text-xs border-b border-slate-100">
                                <tr>
                                    <th class="px-5 py-3.5">Tanggal & Waktu</th>
                                    <th class="px-5 py-3.5">Nama Kemasan</th>
                                    <th class="px-5 py-3.5 text-center">Jenis Mutasi</th>
                                    <th class="px-5 py-3.5 text-right">Jumlah</th>
                                    <th class="px-5 py-3.5 text-right">Sisa Stok Akhir</th>
                                    <th class="px-5 py-3.5">Catatan / Keterangan</th>
                                </tr>
                            </thead>
                            <tbody id="logsTable" class="divide-y divide-slate-100">
                                <!-- Dynamic Log Rows -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>

            <!-- TAB 4: PACKAGING CALCULATOR -->
            <section id="tab-calculator" class="tab-content space-y-6 hidden">
                <div class="max-w-3xl mx-auto bg-white p-6 md:p-8 rounded-2xl border border-slate-100 shadow-sm">
                    <div class="flex items-center space-x-3 mb-6 border-b border-slate-100 pb-4">
                        <div class="p-3 bg-emerald-100 text-emerald-600 rounded-xl">
                            <i class="fa-solid fa-calculator text-xl"></i>
                        </div>
                        <div>
                            <h3 class="text-lg font-bold text-slate-800">Kalkulator Perencanaan Stok Kemasan</h3>
                            <p class="text-xs text-slate-500">Hitung estimasi daya tahan stok dan waktu ideal reorder ke supplier.</p>
                        </div>
                    </div>

                    <div id="calcContainer" class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="space-y-4">
                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Pilih Produk Kemasan</label>
                                <select id="calcProductSelect" onchange="calculateDemand()" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                                    <!-- Dynamic Options -->
                                </select>
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Rata-Rata Pemakaian Harian (pcs)</label>
                                <input type="number" id="calcDailyUsage" oninput="calculateDemand()" min="1" value="25" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                            </div>

                            <div>
                                <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Waktu Tunggu Supplier / Lead Time (Hari)</label>
                                <input type="number" id="calcLeadTime" oninput="calculateDemand()" min="1" value="3" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                            </div>
                        </div>

                        <!-- Results Card -->
                        <div class="bg-slate-900 text-white rounded-2xl p-6 flex flex-col justify-between shadow-lg">
                            <div>
                                <span class="text-xs font-semibold uppercase tracking-wider text-emerald-400">Hasil Estimasi</span>
                                <div class="mt-4 space-y-4">
                                    <div>
                                        <p class="text-xs text-slate-400">Stok Cukup Untuk:</p>
                                        <h4 class="text-3xl font-extrabold text-white mt-1" id="calcDaysRemaining">0 Hari</h4>
                                    </div>
                                    <div class="pt-3 border-t border-slate-800">
                                        <p class="text-xs text-slate-400">Tanggal Rekomendasi Reorder:</p>
                                        <p class="text-sm font-semibold text-emerald-400 mt-1" id="calcReorderDate">-</p>
                                    </div>
                                    <div class="pt-3 border-t border-slate-800">
                                        <p class="text-xs text-slate-400">Batas Stok Minimum Ideal (Safety Stock):</p>
                                        <p class="text-sm font-semibold text-amber-400 mt-1" id="calcSafetyStock">0 pcs</p>
                                    </div>
                                </div>
                            </div>
                            <p class="text-xs text-slate-500 mt-6">*Perhitungan diasumsikan pemakaian harian konsisten.</p>
                        </div>
                    </div>
                </div>
            </section>
        </div>
    </main>

    <!-- MODAL: ADD / EDIT PRODUCT -->
    <div id="productModal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-md w-full shadow-2xl overflow-hidden transform transition-all">
            <div class="px-6 py-4 border-b border-slate-100 flex justify-between items-center bg-slate-50">
                <h3 class="font-bold text-slate-800" id="modalProductTitle">Tambah Jenis Kemasan</h3>
                <button onclick="closeModal('productModal')" class="text-slate-400 hover:text-slate-600">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>
            <form id="productForm" onsubmit="saveProduct(event)" class="p-6 space-y-4">
                <input type="hidden" id="prodId">
                
                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Nama Kemasan *</label>
                    <input type="text" id="prodName" required placeholder="Contoh: Stand Up Pouch Klip 14x22" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Kategori *</label>
                        <select id="prodCategory" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3 py-2 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                            <option value="Botol">Botol</option>
                            <option value="Pouch">Pouch / Plastik Klip</option>
                            <option value="Box">Box / Dus Packaging</option>
                            <option value="Kantong Plastik">Kantong / Tas</option>
                            <option value="Label">Label & Stiker</option>
                            <option value="Lainnya">Lainnya</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Ukuran / Dimensi</label>
                        <input type="text" id="prodSize" placeholder="250ml / 15x20 cm" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Stok Awal *</label>
                        <input type="number" id="prodStock" min="0" required placeholder="100" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Batas Min. Reorder *</label>
                        <input type="number" id="prodMinStock" min="1" required placeholder="20" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Harga Satuan (Rp)</label>
                        <input type="number" id="prodPrice" min="0" placeholder="1200" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Nama Supplier</label>
                        <input type="text" id="prodSupplier" placeholder="CV Plastik Jaya" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                    </div>
                </div>

                <div class="pt-4 flex items-center justify-end space-x-3 border-t border-slate-100">
                    <button type="button" onclick="closeModal('productModal')" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-100 transition">Batal</button>
                    <button type="submit" class="px-4 py-2 rounded-xl text-sm font-medium bg-emerald-600 hover:bg-emerald-700 text-white transition shadow-sm">Simpan Kemasan</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL: QUICK DAILY STOCK ADJUSTMENT -->
    <div id="dailyUpdateModal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-md w-full shadow-2xl overflow-hidden transform transition-all">
            <div class="px-6 py-4 border-b border-slate-100 flex justify-between items-center bg-slate-50">
                <div class="flex items-center space-x-2">
                    <div class="p-1.5 bg-emerald-100 text-emerald-600 rounded-lg">
                        <i class="fa-solid fa-bolt text-sm"></i>
                    </div>
                    <h3 class="font-bold text-slate-800">Quick Daily Stock Update</h3>
                </div>
                <button onclick="closeModal('dailyUpdateModal')" class="text-slate-400 hover:text-slate-600">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>
            <form id="dailyUpdateForm" onsubmit="saveDailyUpdate(event)" class="p-6 space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Pilih Produk Kemasan *</label>
                    <select id="dailyProdSelect" required class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                        <!-- Dynamic Options -->
                    </select>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Jenis Mutasi Stok *</label>
                    <div class="grid grid-cols-2 gap-3">
                        <label class="cursor-pointer">
                            <input type="radio" name="dailyType" value="OUT" checked class="peer sr-only">
                            <div class="p-3 border-2 border-slate-200 rounded-xl text-center peer-checked:border-amber-500 peer-checked:bg-amber-50 peer-checked:text-amber-800 font-semibold text-sm transition">
                                <i class="fa-solid fa-arrow-up-from-bracket mr-1"></i> Terpakai (Keluar)
                            </div>
                        </label>
                        <label class="cursor-pointer">
                            <input type="radio" name="dailyType" value="IN" class="peer sr-only">
                            <div class="p-3 border-2 border-slate-200 rounded-xl text-center peer-checked:border-emerald-500 peer-checked:bg-emerald-50 peer-checked:text-emerald-800 font-semibold text-sm transition">
                                <i class="fa-solid fa-arrow-down-to-bracket mr-1"></i> Restock (Masuk)
                            </div>
                        </label>
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Jumlah (pcs) *</label>
                    <input type="number" id="dailyAmount" min="1" required placeholder="misal: 50" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 uppercase mb-1">Catatan / Keterangan</label>
                    <input type="text" id="dailyNote" placeholder="misal: Pemakaian Produksi / Restock Supplier" class="w-full bg-slate-50 border border-slate-200 rounded-xl px-3.5 py-2.5 text-sm focus:ring-2 focus:ring-emerald-500/20 focus:border-emerald-500">
                </div>

                <div class="pt-4 flex items-center justify-end space-x-3 border-t border-slate-100">
                    <button type="button" onclick="closeModal('dailyUpdateModal')" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-100 transition">Batal</button>
                    <button type="submit" class="px-5 py-2 rounded-xl text-sm font-medium bg-emerald-600 hover:bg-emerald-700 text-white transition shadow-md">Update Stok</button>
                </div>
            </form>
        </div>
    </div>

    <!-- UI TOAST / MESSAGE BOX -->
    <div id="toast" class="fixed bottom-5 right-5 transform translate-y-20 opacity-0 transition-all duration-300 z-50 bg-slate-900 text-white px-5 py-3.5 rounded-2xl shadow-2xl flex items-center space-x-3 border border-slate-800">
        <i id="toastIcon" class="fa-solid fa-circle-check text-emerald-400 text-lg"></i>
        <span id="toastMessage" class="text-sm font-medium">Berhasil memperbarui data.</span>
    </div>

    <!-- APPLICATION JAVASCRIPT LOGIC -->
    <script>
        // Default Sample Data (Only used if user explicitly clicks "Muat Contoh Data Demo")
        const SAMPLE_PRODUCTS = [
            { id: "p1", name: "Stand Up Pouch Klip 14x22", category: "Pouch", size: "14x22 cm", stock: 120, minStock: 50, price: 1200, supplier: "CV Kemasan Perkasa" },
            { id: "p2", name: "Botol Kale Bening 250ml", category: "Botol", size: "250 ml", stock: 15, minStock: 30, price: 2100, supplier: "PT Botol Plastik Utama" },
            { id: "p3", name: "Box Pizza Mini Craft 15x15", category: "Box", size: "15x15x5 cm", stock: 250, minStock: 40, price: 1800, supplier: "Karton Indah Box" }
        ];

        const SAMPLE_LOGS = [
            { id: "l1", timestamp: "2026-10-18 14:30", productId: "p1", productName: "Stand Up Pouch Klip 14x22", type: "OUT", amount: 30, remainingStock: 120, note: "Pemakaian Produksi Keripik" },
            { id: "l2", timestamp: "2026-10-18 09:15", productId: "p2", productName: "Botol Kale Bening 250ml", type: "OUT", amount: 35, remainingStock: 15, note: "Pemakaian Minuman Kopi" }
        ];

        // Global State Variables
        let products = [];
        let logs = [];
        let categoryChartInstance = null;
        let stockBarChartInstance = null;

        // Initialize App on DOM Load
        document.addEventListener("DOMContentLoaded", () => {
            initData();
            setupMobileMenu();
            renderDashboard();
            renderInventoryTable();
            renderLogTable();
            populateDropdowns();
            calculateDemand();
        });

        // Initialize LocalStorage Data Persistence
        function initData() {
            const storedProducts = localStorage.getItem("packstok_products");
            const storedLogs = localStorage.getItem("packstok_logs");

            if (storedProducts !== null) {
                products = JSON.parse(storedProducts);
            } else {
                products = []; // Empty default as requested
            }

            if (storedLogs !== null) {
                logs = JSON.parse(storedLogs);
            } else {
                logs = [];
            }
        }

        // Realtime Auto-Save to Permanent LocalStorage
        function saveToLocalStorage() {
            localStorage.setItem("packstok_products", JSON.stringify(products));
            localStorage.setItem("packstok_logs", JSON.stringify(logs));
            
            // Visual indicator feedback
            triggerSaveIndicator();
        }

        function triggerSaveIndicator() {
            const ind = document.getElementById("saveIndicator");
            const dot = document.getElementById("saveDot");
            const txt = document.getElementById("saveText");

            if (ind) {
                ind.classList.remove("bg-emerald-50", "text-emerald-700");
                ind.classList.add("bg-emerald-500", "text-white", "shadow-md");
                txt.innerText = "Tersimpan Permanen!";

                setTimeout(() => {
                    ind.classList.remove("bg-emerald-500", "text-white", "shadow-md");
                    ind.classList.add("bg-emerald-50", "text-emerald-700");
                    txt.innerText = "Auto-saved (Permanen)";
                }, 1200);
            }
        }

        // FEATURE 1: RESET ALL DATA PERMANENTLY
        function confirmResetAll() {
            const firstConfirm = confirm("APAKAH ANDA YAKIN?\n\nTindakan ini akan MENGHAPUS PERMANEN seluruh daftar kemasan dan riwayat transaksi stok Anda dari penyimpanan browser.");
            
            if (firstConfirm) {
                products = [];
                logs = [];
                saveToLocalStorage();
                refreshAllViews();
                showToast("Seluruh data stok kemasan telah berhasil dikosongkan.", "danger");
            }
        }

        // Load Preset Demo Data optionally
        function loadSampleData() {
            if (products.length > 0) {
                if (!confirm("Memuat data demo akan menggantikan data Anda saat ini. Lanjutkan?")) return;
            }
            products = [...SAMPLE_PRODUCTS];
            logs = [...SAMPLE_LOGS];
            saveToLocalStorage();
            refreshAllViews();
            showToast("Data sampel demo berhasil dimuat!");
        }

        // Tab Navigation Switcher
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('bg-emerald-600', 'text-white', 'shadow-lg', 'shadow-emerald-600/30');
                btn.classList.add('hover:bg-slate-800', 'hover:text-white', 'text-slate-400');
            });

            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`nav-${tabId}`);
            if (activeBtn) {
                activeBtn.classList.remove('hover:bg-slate-800', 'hover:text-white', 'text-slate-400');
                activeBtn.classList.add('bg-emerald-600', 'text-white', 'shadow-lg', 'shadow-emerald-600/30');
            }

            // Update Page Titles
            const titles = {
                dashboard: "Dashboard Overview",
                inventory: "Daftar Stok Kemasan",
                history: "Riwayat Mutasi Stok",
                calculator: "Kalkulator Estimasi Stok"
            };
            document.getElementById("pageTitle").innerText = titles[tabId] || "PackStok UMKM";

            if (tabId === 'dashboard') {
                setTimeout(() => renderCharts(), 50);
            }

            closeMobileSidebar();
        }

        // Mobile Sidebar Controls
        function setupMobileMenu() {
            const btn = document.getElementById("mobileMenuBtn");
            const closeBtn = document.getElementById("closeMobileMenuBtn");
            const sidebar = document.getElementById("sidebar");
            const overlay = document.getElementById("sidebarOverlay");

            function open() {
                sidebar.classList.remove("-translate-x-full");
                overlay.classList.remove("hidden");
            }

            function close() {
                sidebar.classList.add("-translate-x-full");
                overlay.classList.add("hidden");
            }

            btn.addEventListener("click", open);
            closeBtn.addEventListener("click", close);
            overlay.addEventListener("click", close);
        }

        function closeMobileSidebar() {
            document.getElementById("sidebar").classList.add("-translate-x-full");
            document.getElementById("sidebarOverlay").classList.add("hidden");
        }

        // Render Dashboard Stats & Visual Charts
        function renderDashboard() {
            const totalVarian = products.length;
            const totalStok = products.reduce((acc, item) => acc + Number(item.stock), 0);
            const lowStockItems = products.filter(item => Number(item.stock) <= Number(item.minStock));
            const totalAsset = products.reduce((acc, item) => acc + (Number(item.stock) * (Number(item.price) || 0)), 0);

            const categories = [...new Set(products.map(p => p.category))];

            document.getElementById("statTotalVarian").innerText = totalVarian;
            document.getElementById("statTotalStok").innerText = totalStok.toLocaleString('id-ID');
            document.getElementById("statLowStock").innerText = lowStockItems.length;
            document.getElementById("statKategoriCount").innerText = categories.length;
            document.getElementById("statTotalAsset").innerText = `Rp ${totalAsset.toLocaleString('id-ID')}`;

            // Low Stock Warning Banner
            const banner = document.getElementById("lowStockBanner");
            if (lowStockItems.length > 0) {
                banner.classList.remove("hidden");
                document.getElementById("lowStockBannerText").innerText = 
                    `Terdapat ${lowStockItems.length} jenis kemasan (${lowStockItems.map(i => i.name).join(', ')}) yang stoknya sudah mencapai/dibawah batas minimum reorder.`;
            } else {
                banner.classList.add("hidden");
            }

            // Recent Logs Table (Top 5)
            const recentLogsTable = document.getElementById("recentLogsTable");
            recentLogsTable.innerHTML = "";
            const recentLogs = logs.slice(0, 5);

            if (recentLogs.length === 0) {
                recentLogsTable.innerHTML = `<tr><td colspan="5" class="px-5 py-6 text-center text-slate-400">Belum ada riwayat transaksi. Seluruh data kosong.</td></tr>`;
            } else {
                recentLogs.forEach(log => {
                    const isOut = log.type === 'OUT';
                    const badgeClass = isOut ? 'bg-amber-100 text-amber-800' : 'bg-emerald-100 text-emerald-800';
                    const typeLabel = isOut ? 'Keluar' : 'Masuk';
                    const sign = isOut ? '-' : '+';

                    recentLogsTable.innerHTML += `
                        <tr class="hover:bg-slate-50 transition">
                            <td class="px-5 py-3 whitespace-nowrap text-slate-500">${log.timestamp}</td>
                            <td class="px-5 py-3 font-semibold text-slate-800">${log.productName}</td>
                            <td class="px-5 py-3">
                                <span class="inline-flex items-center px-2 py-0.5 rounded text-xs font-medium ${badgeClass}">
                                    ${typeLabel}
                                </span>
                            </td>
                            <td class="px-5 py-3 text-right font-bold ${isOut ? 'text-amber-600' : 'text-emerald-600'}">${sign}${log.amount} pcs</td>
                            <td class="px-5 py-3 text-slate-500 truncate max-w-xs">${log.note || '-'}</td>
                        </tr>
                    `;
                });
            }

            renderCharts();
        }

        // Render Dynamic Chart.js with Empty State Handlers
        function renderCharts() {
            const ctx1 = document.getElementById('categoryChart').getContext('2d');
            const ctx2 = document.getElementById('stockBarChart').getContext('2d');

            if (categoryChartInstance) categoryChartInstance.destroy();
            if (stockBarChartInstance) stockBarChartInstance.destroy();

            if (products.length === 0) {
                // Empty Chart Fallback
                categoryChartInstance = new Chart(ctx1, {
                    type: 'doughnut',
                    data: {
                        labels: ['Belum ada data'],
                        datasets: [{ data: [1], backgroundColor: ['#e2e8f0'] }]
                    },
                    options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
                });

                stockBarChartInstance = new Chart(ctx2, {
                    type: 'bar',
                    data: {
                        labels: ['Data Kosong'],
                        datasets: [{ label: 'Stok Kosong', data: [0], backgroundColor: '#cbd5e1' }]
                    },
                    options: { responsive: true, maintainAspectRatio: false }
                });
                return;
            }

            // Kategori Donut Chart
            const categoryCounts = {};
            products.forEach(p => {
                categoryCounts[p.category] = (categoryCounts[p.category] || 0) + Number(p.stock);
            });

            categoryChartInstance = new Chart(ctx1, {
                type: 'doughnut',
                data: {
                    labels: Object.keys(categoryCounts),
                    datasets: [{
                        data: Object.values(categoryCounts),
                        backgroundColor: ['#10b981', '#3b82f6', '#f59e0b', '#8b5cf6', '#ec4899', '#64748b'],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { position: 'bottom', labels: { boxWidth: 12, font: { size: 11 } } } }
                }
            });

            // Stock Levels Bar Chart
            const prodLabels = products.map(p => p.name.length > 14 ? p.name.substring(0, 14) + '...' : p.name);
            stockBarChartInstance = new Chart(ctx2, {
                type: 'bar',
                data: {
                    labels: prodLabels,
                    datasets: [
                        { label: 'Stok Saat Ini', data: products.map(p => Number(p.stock)), backgroundColor: '#22c55e', borderRadius: 6 },
                        { label: 'Batas Min (Reorder)', data: products.map(p => Number(p.minStock)), backgroundColor: '#f59e0b', borderRadius: 6 }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: { beginAtZero: true, grid: { color: '#f1f5f9' } },
                        x: { grid: { display: false } }
                    },
                    plugins: { legend: { position: 'top', labels: { boxWidth: 12, font: { size: 11 } } } }
                }
            });
        }

        // Render Inventory Table with Clean Empty State
        function renderInventoryTable() {
            const tableBody = document.getElementById("inventoryTable");
            const searchQuery = document.getElementById("searchInput").value.toLowerCase();
            const selectedCategory = document.getElementById("categoryFilter").value;
            const selectedStatus = document.getElementById("statusFilter").value;

            let filtered = products.filter(p => {
                const matchesSearch = p.name.toLowerCase().includes(searchQuery) || 
                                      (p.supplier && p.supplier.toLowerCase().includes(searchQuery)) ||
                                      (p.size && p.size.toLowerCase().includes(searchQuery));
                const matchesCategory = selectedCategory === 'ALL' || p.category === selectedCategory;
                
                let matchesStatus = true;
                const stock = Number(p.stock);
                const min = Number(p.minStock);

                if (selectedStatus === 'SAFE') matchesStatus = stock > min;
                else if (selectedStatus === 'LOW') matchesStatus = stock <= min && stock > 0;
                else if (selectedStatus === 'EMPTY') matchesStatus = stock === 0;

                return matchesSearch && matchesCategory && matchesStatus;
            });

            tableBody.innerHTML = "";

            if (filtered.length === 0) {
                tableBody.innerHTML = `
                    <tr>
                        <td colspan="8" class="px-5 py-12 text-center">
                            <div class="max-w-xs mx-auto text-center space-y-3">
                                <div class="w-12 h-12 bg-slate-100 text-slate-400 rounded-2xl flex items-center justify-center mx-auto text-xl">
                                    <i class="fa-solid fa-box-open"></i>
                                </div>
                                <div>
                                    <h4 class="font-bold text-slate-700 text-sm">Belum Ada Kemasan</h4>
                                    <p class="text-xs text-slate-400 mt-1">Belum ada data kemasan terdaftar atau sesuai pencarian.</p>
                                </div>
                                <button onclick="openModal('add')" class="inline-flex items-center space-x-2 bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-semibold px-4 py-2 rounded-xl shadow-sm transition">
                                    <i class="fa-solid fa-plus"></i>
                                    <span>Tambah Kemasan Pertama</span>
                                </button>
                            </div>
                        </td>
                    </tr>
                `;
                return;
            }

            filtered.forEach(p => {
                const stock = Number(p.stock);
                const minStock = Number(p.minStock);

                let badge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-medium bg-emerald-100 text-emerald-800"><i class="fa-solid fa-circle-check mr-1.5 text-[10px]"></i>Aman</span>`;
                if (stock === 0) {
                    badge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-medium bg-rose-100 text-rose-800"><i class="fa-solid fa-circle-xmark mr-1.5 text-[10px]"></i>Habis</span>`;
                } else if (stock <= minStock) {
                    badge = `<span class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-medium bg-amber-100 text-amber-800"><i class="fa-solid fa-triangle-exclamation mr-1.5 text-[10px]"></i>Menipis</span>`;
                }

                tableBody.innerHTML += `
                    <tr class="hover:bg-slate-50 transition">
                        <td class="px-5 py-4">
                            <div class="font-bold text-slate-800">${p.name}</div>
                            <div class="text-xs text-slate-400">Ukuran: ${p.size || '-'}</div>
                        </td>
                        <td class="px-5 py-4 whitespace-nowrap">
                            <span class="px-2.5 py-1 rounded-lg text-xs font-medium bg-slate-100 text-slate-700">${p.category}</span>
                        </td>
                        <td class="px-5 py-4 text-center font-bold text-base text-slate-800">${stock.toLocaleString('id-ID')}</td>
                        <td class="px-5 py-4 text-center text-slate-500">${minStock.toLocaleString('id-ID')}</td>
                        <td class="px-5 py-4 text-right text-slate-700 font-medium">Rp ${(Number(p.price) || 0).toLocaleString('id-ID')}</td>
                        <td class="px-5 py-4 text-slate-600 text-xs">${p.supplier || '-'}</td>
                        <td class="px-5 py-4 text-center whitespace-nowrap">${badge}</td>
                        <td class="px-5 py-4 text-center whitespace-nowrap">
                            <div class="flex justify-center space-x-2">
                                <button onclick="openModal('edit', '${p.id}')" title="Edit Kemasan" class="p-2 text-slate-400 hover:text-blue-600 hover:bg-blue-50 rounded-lg transition">
                                    <i class="fa-solid fa-pen-to-square"></i>
                                </button>
                                <button onclick="deleteProduct('${p.id}')" title="Hapus Kemasan" class="p-2 text-slate-400 hover:text-rose-600 hover:bg-rose-50 rounded-lg transition">
                                    <i class="fa-solid fa-trash-can"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `;
            });
        }

        // Render History / Log Table
        function renderLogTable() {
            const tableBody = document.getElementById("logsTable");
            const searchQuery = document.getElementById("logSearchInput").value.toLowerCase();
            const dateQuery = document.getElementById("logDateFilter").value;
            const typeQuery = document.getElementById("logTypeFilter").value;

            let filtered = logs.filter(log => {
                const matchesSearch = log.productName.toLowerCase().includes(searchQuery) ||
                                      (log.note && log.note.toLowerCase().includes(searchQuery));
                const matchesType = typeQuery === 'ALL' || log.type === typeQuery;
                const matchesDate = !dateQuery || log.timestamp.startsWith(dateQuery);

                return matchesSearch && matchesType && matchesDate;
            });

            tableBody.innerHTML = "";

            if (filtered.length === 0) {
                tableBody.innerHTML = `<tr><td colspan="6" class="px-5 py-8 text-center text-slate-400">Belum ada riwayat transaksi mutasi stok.</td></tr>`;
                return;
            }

            filtered.forEach(log => {
                const isOut = log.type === 'OUT';
                const badgeClass = isOut ? 'bg-amber-100 text-amber-800' : 'bg-emerald-100 text-emerald-800';
                const typeLabel = isOut ? 'Keluar (Pemakaian)' : 'Masuk (Restock)';
                const sign = isOut ? '-' : '+';

                tableBody.innerHTML += `
                    <tr class="hover:bg-slate-50 transition">
                        <td class="px-5 py-3.5 whitespace-nowrap text-slate-500 font-mono text-xs">${log.timestamp}</td>
                        <td class="px-5 py-3.5 font-bold text-slate-800">${log.productName}</td>
                        <td class="px-5 py-3.5 text-center">
                            <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium ${badgeClass}">
                                ${typeLabel}
                            </span>
                        </td>
                        <td class="px-5 py-3.5 text-right font-bold ${isOut ? 'text-amber-600' : 'text-emerald-600'}">${sign}${log.amount}</td>
                        <td class="px-5 py-3.5 text-right font-medium text-slate-700">${log.remainingStock}</td>
                        <td class="px-5 py-3.5 text-slate-500 text-xs">${log.note || '-'}</td>
                    </tr>
                `;
            });
        }

        function filterLowStockOnly() {
            document.getElementById("statusFilter").value = "LOW";
            renderInventoryTable();
        }

        // Dropdown Helper Population
        function populateDropdowns() {
            const categoryFilter = document.getElementById("categoryFilter");
            const categories = [...new Set(products.map(p => p.category))];
            
            let catOptions = `<option value="ALL">Semua Kategori</option>`;
            categories.forEach(cat => {
                catOptions += `<option value="${cat}">${cat}</option>`;
            });
            categoryFilter.innerHTML = catOptions;

            const dailySelect = document.getElementById("dailyProdSelect");
            const calcSelect = document.getElementById("calcProductSelect");

            let prodOptions = "";
            if (products.length === 0) {
                prodOptions = `<option value="">-- Belum Ada Produk --</option>`;
            } else {
                products.forEach(p => {
                    prodOptions += `<option value="${p.id}">${p.name} (Stok: ${p.stock})</option>`;
                });
            }

            if (dailySelect) dailySelect.innerHTML = prodOptions;
            if (calcSelect) calcSelect.innerHTML = prodOptions;
        }

        // Add / Edit Product Modal
        function openModal(mode, productId = null) {
            const modal = document.getElementById("productModal");
            const title = document.getElementById("modalProductTitle");
            const form = document.getElementById("productForm");

            form.reset();

            if (mode === 'add') {
                title.innerText = "Tambah Kemasan Baru";
                document.getElementById("prodId").value = "";
            } else if (mode === 'edit' && productId) {
                title.innerText = "Edit Detail Kemasan";
                const prod = products.find(p => p.id === productId);
                if (prod) {
                    document.getElementById("prodId").value = prod.id;
                    document.getElementById("prodName").value = prod.name;
                    document.getElementById("prodCategory").value = prod.category;
                    document.getElementById("prodSize").value = prod.size || "";
                    document.getElementById("prodStock").value = prod.stock;
                    document.getElementById("prodMinStock").value = prod.minStock;
                    document.getElementById("prodPrice").value = prod.price || "";
                    document.getElementById("prodSupplier").value = prod.supplier || "";
                }
            }

            modal.classList.remove("hidden");
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.add("hidden");
        }

        function saveProduct(e) {
            e.preventDefault();

            const id = document.getElementById("prodId").value;
            const name = document.getElementById("prodName").value.trim();
            const category = document.getElementById("prodCategory").value;
            const size = document.getElementById("prodSize").value.trim();
            const stock = parseInt(document.getElementById("prodStock").value) || 0;
            const minStock = parseInt(document.getElementById("prodMinStock").value) || 0;
            const price = parseInt(document.getElementById("prodPrice").value) || 0;
            const supplier = document.getElementById("prodSupplier").value.trim();

            if (id) {
                // Update Existing Item
                const index = products.findIndex(p => p.id === id);
                if (index !== -1) {
                    products[index] = { ...products[index], name, category, size, stock, minStock, price, supplier };
                    showToast("Kemasan berhasil diperbarui!");
                }
            } else {
                // Add New Item
                const newId = 'p_' + Date.now();
                products.push({ id: newId, name, category, size, stock, minStock, price, supplier });
                
                // Add Initial Log Entry
                const now = new Date();
                const timeString = now.toISOString().slice(0, 10) + ' ' + now.toTimeString().slice(0, 5);
                logs.unshift({
                    id: 'l_' + Date.now(),
                    timestamp: timeString,
                    productId: newId,
                    productName: name,
                    type: 'IN',
                    amount: stock,
                    remainingStock: stock,
                    note: 'Pendaftaran kemasan baru'
                });

                showToast("Kemasan baru tersimpan permanen!");
            }

            saveToLocalStorage();
            refreshAllViews();
            closeModal("productModal");
        }

        function deleteProduct(id) {
            const prod = products.find(p => p.id === id);
            if (!prod) return;

            if (confirm(`Hapus "${prod.name}"? Data yang dihapus tidak bisa dikembalikan.`)) {
                products = products.filter(p => p.id !== id);
                saveToLocalStorage();
                refreshAllViews();
                showToast("Kemasan berhasil dihapus.", "danger");
            }
        }

        // Quick Daily Update Handlers
        function openDailyUpdateModal() {
            if (products.length === 0) {
                alert("Belum ada kemasan terdaftar! Tambahkan kemasan terlebih dahulu.");
                openModal('add');
                return;
            }
            populateDropdowns();
            document.getElementById("dailyUpdateForm").reset();
            document.getElementById("dailyUpdateModal").classList.remove("hidden");
        }

        function saveDailyUpdate(e) {
            e.preventDefault();

            const productId = document.getElementById("dailyProdSelect").value;
            const type = document.querySelector('input[name="dailyType"]:checked').value;
            const amount = parseInt(document.getElementById("dailyAmount").value) || 0;
            const note = document.getElementById("dailyNote").value.trim();

            const product = products.find(p => p.id === productId);
            if (!product) return;

            if (type === 'OUT' && amount > product.stock) {
                alert(`Stok tidak mencukupi! Stok saat ini hanya ${product.stock} pcs.`);
                return;
            }

            // Update Stock
            if (type === 'IN') {
                product.stock += amount;
            } else {
                product.stock -= amount;
            }

            // Create Log
            const now = new Date();
            const timeString = now.toISOString().slice(0, 10) + ' ' + now.toTimeString().slice(0, 5);
            logs.unshift({
                id: 'l_' + Date.now(),
                timestamp: timeString,
                productId: product.id,
                productName: product.name,
                type: type,
                amount: amount,
                remainingStock: product.stock,
                note: note || (type === 'IN' ? 'Restock Harian' : 'Pemakaian Harian')
            });

            saveToLocalStorage();
            refreshAllViews();
            closeModal("dailyUpdateModal");
            showToast(`Update stok ${product.name} berhasil tersimpan!`);
        }

        // Calculator Logic
        function calculateDemand() {
            const select = document.getElementById("calcProductSelect");
            if (!select || !select.value || products.length === 0) {
                document.getElementById("calcDaysRemaining").innerText = "0 Hari";
                document.getElementById("calcReorderDate").innerText = "-";
                document.getElementById("calcSafetyStock").innerText = "0 pcs";
                return;
            }

            const productId = select.value;
            const dailyUsage = parseInt(document.getElementById("calcDailyUsage").value) || 1;
            const leadTime = parseInt(document.getElementById("calcLeadTime").value) || 1;

            const product = products.find(p => p.id === productId);
            if (!product) return;

            const currentStock = Number(product.stock);
            const daysRemaining = Math.floor(currentStock / dailyUsage);
            const safetyStock = dailyUsage * leadTime;

            const today = new Date();
            const reorderDaysBefore = daysRemaining - leadTime;
            let reorderText = "";

            if (reorderDaysBefore <= 0) {
                reorderText = "SEGERA REORDER HARI INI!";
            } else {
                const reorderDate = new Date(today);
                reorderDate.setDate(today.getDate() + reorderDaysBefore);
                const options = { year: 'numeric', month: 'short', day: 'numeric' };
                reorderText = reorderDate.toLocaleDateString('id-ID', options);
            }

            document.getElementById("calcDaysRemaining").innerText = `${daysRemaining} Hari`;
            document.getElementById("calcReorderDate").innerText = reorderText;
            document.getElementById("calcSafetyStock").innerText = `${safetyStock} pcs`;
        }

        // CSV Export
        function exportToCSV() {
            if (logs.length === 0) {
                alert("Belum ada riwayat transaksi mutasi untuk diekspor!");
                return;
            }

            let csvContent = "data:text/csv;charset=utf-8,";
            csvContent += "Waktu,Nama Kemasan,Jenis Mutasi,Jumlah,Sisa Stok,Keterangan\n";

            logs.forEach(log => {
                const row = [
                    `"${log.timestamp}"`,
                    `"${log.productName}"`,
                    `"${log.type === 'IN' ? 'MASUK' : 'KELUAR'}"`,
                    log.amount,
                    log.remainingStock,
                    `"${log.note || ''}"`
                ];
                csvContent += row.join(",") + "\n";
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `PackStok_Mutasi_${new Date().toISOString().slice(0, 10)}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);

            showToast("Riwayat mutasi diekspor ke CSV!");
        }

        // Refresh UI Helper
        function refreshAllViews() {
            renderDashboard();
            renderInventoryTable();
            renderLogTable();
            populateDropdowns();
            calculateDemand();
        }

        // Toast Feedback Helper
        function showToast(message, type = "success") {
            const toast = document.getElementById("toast");
            const toastMessage = document.getElementById("toastMessage");
            const toastIcon = document.getElementById("toastIcon");

            toastMessage.innerText = message;

            if (type === "danger") {
                toastIcon.className = "fa-solid fa-circle-xmark text-rose-400 text-lg";
            } else {
                toastIcon.className = "fa-solid fa-circle-check text-emerald-400 text-lg";
            }

            toast.classList.remove("translate-y-20", "opacity-0");
            toast.classList.add("translate-y-0", "opacity-100");

            setTimeout(() => {
                toast.classList.remove("translate-y-0", "opacity-100");
                toast.classList.add("translate-y-20", "opacity-0");
            }, 3000);
        }
    </script>
</body>
</html>    
