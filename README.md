<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PINTAR CERITA – Pembuat Buku Cerita Anak Digital</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Comic+Neue:wght@400;700&family=Quicksand:wght@500;600;700&display=swap" rel="stylesheet">
    <!-- html2canvas & jsPDF for high quality PDF Export -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        pastel: {
                            blue: '#E0F2FE',
                            pink: '#FCE7F3',
                            yellow: '#FEF9C3',
                            green: '#DCFCE7',
                            purple: '#F3E8FF',
                            orange: '#E0F2FE',
                            brand: '#2563EB',
                            primary: '#1D4ED8',
                            accent: '#0284C7'
                        }
                    },
                    fontFamily: {
                        sans: ['"Plus Jakarta Sans"', 'sans-serif'],
                        heading: ['Fredoka', 'cursive'],
                        kids: ['"Comic Neue"', 'cursive']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #F8FAFC;
        }
        h1, h2, h3, .font-heading {
            font-family: 'Fredoka', cursive;
        }
        .story-text {
            font-family: 'Quicksand', sans-serif;
        }

        /* Scrollbar styling */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #F1F5F9;
        }
        ::-webkit-scrollbar-thumb {
            background: #CBD5E1;
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94A3B8;
        }

        /* Sound Wave Animation */
        .sound-wave-bar {
            display: inline-block;
            width: 4px;
            height: 16px;
            background-color: #0284C7;
            border-radius: 9999px;
            animation: soundWave 1.2s ease-in-out infinite;
        }
        .sound-wave-bar:nth-child(2) { animation-delay: 0.2s; height: 24px; }
        .sound-wave-bar:nth-child(3) { animation-delay: 0.4s; height: 12px; }
        .sound-wave-bar:nth-child(4) { animation-delay: 0.1s; height: 20px; }
        @keyframes soundWave {
            0%, 100% { transform: scaleY(0.4); }
            50% { transform: scaleY(1.2); }
        }

        /* Printable Page styles */
        .page-sheet {
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }

        /* Print Media Styles */
        @media print {
            body * {
                visibility: hidden;
            }
            #print-area, #print-area * {
                visibility: visible;
            }
            #print-area {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
            }
            .page-break {
                page-break-after: always;
                break-after: page;
            }
        }
    </style>
</head>
<body class="text-slate-800 min-h-screen flex flex-col antialiased selection:bg-blue-200">

    <!-- TOP NAVBAR -->
    <header class="bg-white border-b border-slate-200 sticky top-0 z-40 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <!-- Brand Logo -->
            <div class="flex items-center gap-3 cursor-pointer" onclick="navigateTo('library')">
                <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-sky-400 via-blue-600 to-indigo-600 flex items-center justify-center text-white shadow-md shadow-blue-200">
                    <i class="fa-solid fa-book-open-reader text-xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-wide bg-gradient-to-r from-blue-700 via-indigo-600 to-sky-600 bg-clip-text text-transparent">
                        PINTAR CERITA
                    </h1>
                    <p class="text-[10px] font-semibold text-slate-400 -mt-1 tracking-wider uppercase">Pembuat Buku Cerita Anak Digital</p>
                </div>
            </div>

            <!-- Navigation Actions -->
            <div class="flex items-center gap-2 sm:gap-4">
                <button onclick="navigateTo('library')" id="nav-btn-library" class="px-3.5 py-2 rounded-xl text-sm font-semibold transition flex items-center gap-2 bg-indigo-50 text-indigo-600">
                    <i class="fa-solid fa-shapes"></i>
                    <span class="hidden sm:inline">Koleksi Buku</span>
                </button>
                <button onclick="navigateTo('create')" id="nav-btn-create" class="px-4 py-2 rounded-xl text-sm font-bold bg-gradient-to-r from-blue-600 to-indigo-600 text-white shadow-md shadow-blue-200 hover:opacity-95 transition flex items-center gap-2">
                    <i class="fa-solid fa-wand-magic-sparkles"></i>
                    <span>Buat Buku Baru</span>
                </button>
                <button onclick="openApiKeyModal()" title="Pengaturan AI API Key" class="w-9 h-9 rounded-xl border border-slate-200 bg-slate-50 hover:bg-slate-100 flex items-center justify-center text-slate-600 transition">
                    <i class="fa-solid fa-key text-xs"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- MAIN CONTENT CONTAINER -->
    <main id="app-container" class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-8">
        <!-- Dynamic Views injected here -->
    </main>

    <!-- MODAL: API KEY SETTINGS -->
    <div id="modal-apikey" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-3xl max-w-md w-full p-6 shadow-2xl border border-slate-100 transform transition-all">
            <div class="flex justify-between items-center mb-4">
                <div class="flex items-center gap-2.5">
                    <div class="w-8 h-8 rounded-xl bg-sky-100 text-sky-600 flex items-center justify-center">
                        <i class="fa-solid fa-key"></i>
                    </div>
                    <h3 class="text-lg font-bold text-slate-800">Pengaturan Gemini API</h3>
                </div>
                <button onclick="closeApiKeyModal()" class="text-slate-400 hover:text-slate-600 w-8 h-8 rounded-full flex items-center justify-center">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            <p class="text-xs text-slate-500 mb-4 leading-relaxed">
                Aplikasi ini menggunakan Gemini API untuk naskah cerita, ilustrasi gambar, dan suara narasi dongeng anak. Masukkan Gemini API key Anda jika ada. Jika tidak, aplikasi memakai mode gratis bawaan Canvas.
            </p>
            <div class="mb-4">
                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1.5">Gemini API Key (Opsional)</label>
                <input type="password" id="input-api-key" placeholder="AIzaSy..." class="w-full px-3.5 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
            </div>
            <div class="flex justify-end gap-2">
                <button onclick="closeApiKeyModal()" class="px-4 py-2 rounded-xl text-xs font-semibold text-slate-600 hover:bg-slate-100">Batal</button>
                <button onclick="saveApiKey()" class="px-4 py-2 rounded-xl text-xs font-bold bg-indigo-600 text-white hover:bg-indigo-700 shadow-sm">Simpan Kunci</button>
            </div>
        </div>
    </div>

    <!-- MODAL: GENERATION PROGRESS OVERLAY -->
    <div id="modal-progress" class="fixed inset-0 bg-slate-900/70 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-3xl max-w-lg w-full p-8 shadow-2xl text-center border border-slate-100 relative overflow-hidden">
            <div class="absolute -top-12 -right-12 w-32 h-32 bg-sky-200 rounded-full blur-2xl opacity-50"></div>
            <div class="absolute -bottom-12 -left-12 w-32 h-32 bg-blue-200 rounded-full blur-2xl opacity-50"></div>
            
            <div class="relative z-10">
                <div class="w-20 h-20 mx-auto mb-5 rounded-3xl bg-gradient-to-tr from-sky-400 via-blue-600 to-indigo-700 flex items-center justify-center text-white text-3xl shadow-xl shadow-blue-200 animate-bounce">
                    <i class="fa-solid fa-wand-magic-sparkles"></i>
                </div>
                <h3 class="text-2xl font-bold font-heading text-slate-800 mb-2">Keajaiban AI Sedang Bekerja...</h3>
                <p id="progress-status-text" class="text-sm font-semibold text-indigo-600 mb-6">Menyiapkan naskah cerita per halaman...</p>
                
                <!-- Progress Bar -->
                <div class="w-full bg-slate-100 rounded-full h-3 mb-3 overflow-hidden p-0.5 border border-slate-200">
                    <div id="progress-bar-fill" class="bg-gradient-to-r from-sky-400 via-blue-600 to-indigo-700 h-full rounded-full transition-all duration-500 w-5"></div>
                </div>
                <div class="flex justify-between text-xs font-bold text-slate-400 px-1">
                    <span id="progress-percent">5%</span>
                    <span>Proses Otomatis</span>
                </div>

                <div class="mt-6 pt-4 border-t border-slate-100 text-xs text-slate-400 flex items-center justify-center gap-2">
                    <i class="fa-solid fa-sparkles text-sky-400"></i>
                    <span>Menulis cerita, melukis ilustrasi, & menyiapkan narasi suara...</span>
                </div>
            </div>
        </div>
    </div>

    <!-- FOOTER -->
    <footer class="bg-white border-t border-slate-200 py-6 mt-12 text-center text-xs text-slate-500">
        <div class="max-w-7xl mx-auto px-4 flex flex-col sm:flex-row items-center justify-between gap-3">
            <div class="flex items-center gap-2 font-semibold">
                <span class="text-rose-500"><i class="fa-solid fa-heart"></i></span>
                <span>PINTAR CERITA &copy; 2026 - Pembuat Buku Cerita & Narasi Suara Anak</span>
            </div>
            <div class="flex gap-4 font-medium text-slate-400">
                <span>Siap Cetak PDF</span>
                <span>•</span>
                <span>Ilustrasi AI</span>
                <span>•</span>
                <span>Suara Narasi Dongeng</span>
            </div>
        </div>
    </footer>

    <!-- PRINT CONTAINER (HIDDEN IN REGULAR VIEW) -->
    <div id="print-area" class="hidden"></div>

    <!-- APP ENGINE SCRIPT -->
    <script>
        // Global State & Storage Key
        const STORAGE_KEY = 'pintar_cerita_books_v1';
        const API_KEY_STORAGE = 'pintar_cerita_gemini_key';

        let appState = {
            currentView: 'library', // 'library' | 'create' | 'studio' | 'flipbook' | 'quiz'
            books: [],
            currentBookId: null,
            filterGenre: 'ALL',
            filterSearch: '',
            favoritesOnly: false,
            customApiKey: localStorage.getItem(API_KEY_STORAGE) || '',
            
            // Audio Narration State
            isAudioPlaying: false,
            autoPlayMode: false,
            selectedVoice: 'Puck', // Puck, Kore, Aoede, Zephyr, Fenrir
            activeAudioElement: null,
            isTTSLoading: false
        };

        // DOM Ready Initializer
        window.addEventListener('DOMContentLoaded', () => {
            loadBooksFromStorage();
            if (appState.books.length === 0) {
                seedSampleBook();
            }
            renderApp();
        });

        // -----------------------------------------------------------------
        // STORAGE & SAMPLE DATA
        // -----------------------------------------------------------------
        function loadBooksFromStorage() {
            try {
                const stored = localStorage.getItem(STORAGE_KEY);
                if (stored) {
                    appState.books = JSON.parse(stored);
                }
            } catch (e) {
                console.error("Error reading localStorage:", e);
                appState.books = [];
            }
        }

        function saveBooksToStorage() {
            try {
                localStorage.setItem(STORAGE_KEY, JSON.stringify(appState.books));
            } catch (e) {
                console.error("Error saving to localStorage:", e);
            }
        }

        function seedSampleBook() {
            const sampleBook = {
                id: 'book_sample_01',
                title: 'Petualangan Kiki di Hutan Pelangi',
                author: 'Budi Raharjo, S.Pd.',
                school: 'SD Negeri 1 Nusantara',
                grade: 'Kelas 2 SD',
                targetAge: '7-8 Tahun',
                genre: 'Fabel (cerita binatang)',
                theme: 'Gotong Royong & Menjaga Lingkungan',
                language: 'Indonesia',
                illustrationStyle: 'Kartun 3D',
                characterRef: 'Kiki si kelinci putih dengan telinga panjang dan rompi hijau muda',
                orientation: 'portrait',
                pageSize: 'A4',
                synopsis: 'Kiki si kelinci cerdik bertualang bersama teman-temannya membersihkan Danau Pelangi agar bunga-bunga ajaib kembali bermekaran.',
                moralMessage: 'Kerja sama dan gotong royong dapat menyelesaikan masalah berat menjadi ringan dan membahagiakan.',
                createdAt: new Date().toISOString(),
                isFavorite: true,
                pages: [
                    {
                        pageNumber: 1,
                        text: 'Di sebuah hutan indah bernama Hutan Pelangi, hiduplah seekor kelinci putih yang sangat ramah bernama Kiki. Kiki selalu mengenakan rompi hijau kesayangannya setiap kali berpetualang.',
                        illustrationPrompt: 'Kiki the cute 3D cartoon white rabbit wearing a light green vest, standing in a colorful magical rainbow forest with tall green trees and sunlight.',
                        imageUrl: generateDynamicSvg('Kiki di Hutan Pelangi', '#86EFAC', '🐰🌲✨', 'Kartun 3D'),
                        bgColor: '#F0FDF4',
                        textColor: '#166534',
                        fontSize: 'medium'
                    },
                    {
                        pageNumber: 2,
                        text: 'Suatu pagi, Kiki berjalan menuju Danau Jernih. Namun, Kiki terkejut melihat danau tersumbat oleh ranting-ranting pohon yang gugur. Airnya tidak dapat mengalir lagi!',
                        illustrationPrompt: 'Kiki the rabbit looking surprised at a small forest lake blocked by fallen wooden branches, colorful forest surroundings.',
                        imageUrl: generateDynamicSvg('Danau Tersumbat Ranting', '#FEF08A', '🐰🌊🪵', 'Kartun 3D'),
                        bgColor: '#FEFCE8',
                        textColor: '#854D0E',
                        fontSize: 'medium'
                    },
                    {
                        pageNumber: 3,
                        text: '"Aku tidak bisa membersihkan ini sendirian," bisik Kiki. Ia lalu berlari memanggil Tupi si tupai lincah dan Gajah kecil bernama Milo.',
                        illustrationPrompt: 'Kiki rabbit calling his friends: Tupi the squirrel and Milo the little cute elephant in a bright sunny forest path.',
                        imageUrl: generateDynamicSvg('Memanggil Teman-Teman', '#FBCFE8', '🐰🐿️🐘', 'Kartun 3D'),
                        bgColor: '#FDF2F8',
                        textColor: '#9D174D',
                        fontSize: 'medium'
                    },
                    {
                        pageNumber: 4,
                        text: 'Milo menggunakan belalainya untuk mengangkat kayu besar, Tupi mengumpulkan daun, dan Kiki membersihkan sisa sampah kecil. Mereka bekerja sambil bernyanyi riang.',
                        illustrationPrompt: 'Cute elephant, squirrel, and rabbit working together happily picking up branches near a forest stream.',
                        imageUrl: generateDynamicSvg('Gotong Royong Bersama', '#BAE6FD', '🐘🐿️🐰💪', 'Kartun 3D'),
                        bgColor: '#F0F9FF',
                        textColor: '#075985',
                        fontSize: 'medium'
                    },
                    {
                        pageNumber: 5,
                        text: 'Byuur! Air danau kembali mengalir jernih. Bunga-bunga pelangi di tepi danau mekar memancarkan warna-warni yang sangat indah.',
                        illustrationPrompt: 'Clear blue lake flowing smoothly with colorful glowing magic flowers blooming along the lake bank, animals celebrating.',
                        imageUrl: generateDynamicSvg('Danau Jernih & Bunga Mekar', '#DDD6FE', '🌊🌸🌈✨', 'Kartun 3D'),
                        bgColor: '#F5F3FF',
                        textColor: '#5B21B6',
                        fontSize: 'medium'
                    },
                    {
                        pageNumber: 6,
                        text: 'Kiki, Milo, dan Tupi tersenyum bahagia. Hutan Pelangi kembali asri dan bersih karena mereka mau bekerja sama menjaga lingkungan bersama-sama.',
                        illustrationPrompt: 'Cute rabbit, squirrel and baby elephant hugging and celebrating happily in front of glowing blooming lake.',
                        imageUrl: generateDynamicSvg('Sahabat Bahagia Bersama', '#FED7AA', '🎉🐰🐿️🐘', 'Kartun 3D'),
                        bgColor: '#FFF7ED',
                        textColor: '#9A3412',
                        fontSize: 'medium'
                    }
                ],
                quiz: [
                    {
                        question: 'Siapakah tokoh utama dalam cerita di atas?',
                        options: ['Milo si Gajah', 'Kiki si Kelinci', 'Tupi si Tupai', 'Budi Penulis'],
                        answerIndex: 1
                    },
                    {
                        question: 'Apa masalah yang terjadi di Danau Jernih?',
                        options: ['Danau kering', 'Tersumbat ranting pohon', 'Banjir besar', 'Airnya beracun'],
                        answerIndex: 1
                    },
                    {
                        question: 'Nilai kebaikan apa yang diajarkan oleh Kiki dan teman-temannya?',
                        options: ['Bermain sendirian', 'Gotong royong dan menjaga lingkungan', 'Acuh tak acuh', 'Marah saat kesulitan'],
                        answerIndex: 1
                    }
                ],
                glossary: [
                    { word: 'Asri', meaning: 'Indah, bersih, dan segar dipandang mata.' },
                    { word: 'Gotong Royong', meaning: 'Bekerja bersama-sama untuk mencapai hasil yang baik.' },
                    { word: 'Belalai', meaning: 'Hidung panjang pada gajah yang berguna untuk mengambil benda.' }
                ]
            };
            appState.books.push(sampleBook);
            saveBooksToStorage();
        }

        // Dynamic Vector SVG Art generator (Fallback)
        function generateDynamicSvg(titleText, bgColorHex, emojiIcon, styleName) {
            const svgStr = `
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 600" width="100%" height="100%">
                <defs>
                    <linearGradient id="grad" x1="0%" y1="0%" x2="100%" y2="100%">
                        <stop offset="0%" style="stop-color:${bgColorHex};stop-opacity:1" />
                        <stop offset="100%" style="stop-color:#ffffff;stop-opacity:0.9" />
                    </linearGradient>
                    <filter id="shadow" x="-10%" y="-10%" width="120%" height="120%">
                        <feDropShadow dx="0" dy="8" stdDeviation="6" flood-opacity="0.15"/>
                    </filter>
                </defs>
                <rect width="800" height="600" fill="url(#grad)" />
                <circle cx="700" cy="100" r="140" fill="#FFFFFF" opacity="0.4" />
                <circle cx="100" cy="500" r="180" fill="#FFFFFF" opacity="0.3" />
                <g filter="url(#shadow)" transform="translate(400, 260)">
                    <rect x="-160" y="-140" width="320" height="260" rx="36" fill="#FFFFFF" opacity="0.95" />
                    <text x="0" y="20" font-family="'Fredoka', sans-serif" font-size="90" text-anchor="middle" dominant-baseline="middle">${emojiIcon}</text>
                    <rect x="-140" y="80" width="280" height="28" rx="14" fill="#6366F1" opacity="0.15" />
                    <text x="0" y="99" font-family="'Fredoka', sans-serif" font-size="14" font-weight="bold" fill="#4F46E5" text-anchor="middle">${styleName} - Ilustrasi Digital</text>
                </g>
                <g filter="url(#shadow)" transform="translate(400, 480)">
                    <rect x="-260" y="-30" width="520" height="60" rx="20" fill="#1E293B" />
                    <text x="0" y="8" font-family="'Fredoka', sans-serif" font-size="22" font-weight="bold" fill="#FFFFFF" text-anchor="middle">${titleText}</text>
                </g>
            </svg>`;
            return 'data:image/svg+xml;charset=utf-8,' + encodeURIComponent(svgStr);
        }

        // -----------------------------------------------------------------
        // ROUTING & NAVIGATION
        // -----------------------------------------------------------------
        function navigateTo(view, bookId = null) {
            stopAudioNarration();
            appState.currentView = view;
            if (bookId) {
                appState.currentBookId = bookId;
            }
            renderApp();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function renderApp() {
            const container = document.getElementById('app-container');
            
            document.getElementById('nav-btn-library').className = appState.currentView === 'library' 
                ? "px-3.5 py-2 rounded-xl text-sm font-bold bg-indigo-50 text-indigo-600 border border-indigo-200"
                : "px-3.5 py-2 rounded-xl text-sm font-semibold text-slate-600 hover:bg-slate-100 transition flex items-center gap-2";

            switch (appState.currentView) {
                case 'library':
                    container.innerHTML = renderLibraryView();
                    break;
                case 'create':
                    container.innerHTML = renderCreateFormView();
                    break;
                case 'studio':
                    container.innerHTML = renderStudioEditorView();
                    break;
                case 'flipbook':
                    container.innerHTML = renderFlipbookPreviewView();
                    break;
                case 'quiz':
                    container.innerHTML = renderQuizView();
                    break;
                default:
                    container.innerHTML = renderLibraryView();
            }
        }

        // -----------------------------------------------------------------
        // 1. HALAMAN UTAMA (LIBRARY VIEW)
        // -----------------------------------------------------------------
        function renderLibraryView() {
            let filtered = appState.books.filter(book => {
                const matchesSearch = book.title.toLowerCase().includes(appState.filterSearch.toLowerCase()) ||
                                      book.author.toLowerCase().includes(appState.filterSearch.toLowerCase());
                const matchesGenre = appState.filterGenre === 'ALL' || book.genre === appState.filterGenre;
                const matchesFav = !appState.favoritesOnly || book.isFavorite;
                return matchesSearch && matchesGenre && matchesFav;
            });

            return `
            <div class="space-y-8">
                <!-- HERO BANNER -->
                <div class="relative rounded-3xl overflow-hidden bg-gradient-to-r from-slate-900 via-blue-900 to-indigo-900 p-6 sm:p-10 text-white shadow-xl">
                    <div class="absolute -right-10 -bottom-10 opacity-20 text-[180px] pointer-events-none font-heading">
                        <i class="fa-solid fa-book-open"></i>
                    </div>
                    <div class="max-w-2xl relative z-10">
                        <span class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-white/20 backdrop-blur-md text-xs font-bold tracking-wider uppercase mb-3">
                            <i class="fa-solid fa-volume-high text-sky-300"></i> AI Storybook & Voice Narration
                        </span>
                        <h2 class="text-3xl sm:text-5xl font-extrabold font-heading leading-tight mb-3">
                            Ciptakan Buku Cerita Anak Berilustrasi & Bersuara!
                        </h2>
                        <p class="text-white/90 text-sm sm:text-base mb-6 font-medium leading-relaxed">
                            Buku cerita otomatis dengan ilustrasi konsisten, pengisi suara dongeng AI anak, dan dokumen PDF berkualitas tinggi yang siap dicetak.
                        </p>
                        <div class="flex flex-wrap gap-3">
                            <button onclick="navigateTo('create')" class="px-6 py-3.5 rounded-2xl bg-white text-blue-700 font-bold text-sm shadow-lg hover:bg-slate-50 transition transform hover:-translate-y-0.5 flex items-center gap-2">
                                <i class="fa-solid fa-plus-circle text-blue-600 text-lg"></i>
                                <span>Buat Buku Cerita Baru</span>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- FILTER & SEARCH BAR -->
                <div class="flex flex-col md:flex-row gap-4 items-center justify-between bg-white p-4 rounded-2xl border border-slate-200 shadow-sm">
                    <div class="relative w-full md:w-80">
                        <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
                        <input type="text" placeholder="Cari judul atau nama penulis..." value="${appState.filterSearch}" oninput="handleSearchInput(this.value)" class="w-full pl-10 pr-4 py-2.5 rounded-xl border border-slate-200 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    </div>

                    <div class="flex flex-wrap items-center gap-3 w-full md:w-auto justify-end">
                        <select onchange="handleGenreFilter(this.value)" class="px-3.5 py-2.5 rounded-xl border border-slate-200 text-xs font-semibold text-slate-700 bg-slate-50 focus:outline-none">
                            <option value="ALL" ${appState.filterGenre === 'ALL' ? 'selected' : ''}>Semua Jenis Cerita</option>
                            <option value="Dongeng">Dongeng</option>
                            <option value="Fabel (cerita binatang)">Fabel (Cerita Binatang)</option>
                            <option value="Legenda">Legenda / Cerita Rakyat</option>
                            <option value="Petualangan">Petualangan</option>
                            <option value="Cerita persahabatan">Persahabatan</option>
                            <option value="Cerita pendidikan">Pendidikan</option>
                        </select>

                        <button onclick="toggleFavoriteFilter()" class="px-4 py-2.5 rounded-xl text-xs font-bold transition flex items-center gap-2 ${appState.favoritesOnly ? 'bg-blue-100 text-blue-800 border border-blue-300' : 'bg-slate-100 text-slate-600 hover:bg-slate-200'}">
                            <i class="fa-solid fa-star ${appState.favoritesOnly ? 'text-blue-600' : 'text-slate-400'}"></i>
                            <span>Favorit</span>
                        </button>
                    </div>
                </div>

                <!-- BOOK GRID COLLECTION -->
                <div>
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-xl font-bold font-heading text-slate-800 flex items-center gap-2">
                            <span>Koleksi Buku</span>
                            <span class="text-xs px-2.5 py-0.5 rounded-full bg-indigo-100 text-indigo-700 font-bold">${filtered.length} Buku</span>
                        </h3>
                    </div>

                    ${filtered.length === 0 ? `
                        <div class="text-center py-16 bg-white rounded-3xl border border-dashed border-slate-300 p-8">
                            <div class="w-20 h-20 mx-auto mb-4 rounded-3xl bg-blue-50 text-blue-600 flex items-center justify-center text-3xl">
                                <i class="fa-solid fa-book-bookmark"></i>
                            </div>
                            <h4 class="text-lg font-bold text-slate-700 mb-1">Belum Ada Buku Cerita</h4>
                            <p class="text-xs text-slate-400 max-w-sm mx-auto mb-6">Mulai buat karya pertama Anda untuk anak-anak atau peserta didik sekarang juga!</p>
                            <button onclick="navigateTo('create')" class="px-5 py-2.5 rounded-xl bg-indigo-600 text-white font-bold text-xs hover:bg-indigo-700 transition">
                                <i class="fa-solid fa-wand-magic-sparkles mr-1"></i> Buat Buku Pertama
                            </button>
                        </div>
                    ` : `
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                            ${filtered.map(book => renderBookCard(book)).join('')}
                        </div>
                    `}
                </div>
            </div>`;
        }

        function renderBookCard(book) {
            const coverImage = book.pages && book.pages[0] ? book.pages[0].imageUrl : '';
            return `
            <div class="bg-white rounded-3xl border border-slate-200 overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 flex flex-col group">
                <div class="relative h-56 bg-slate-100 overflow-hidden flex items-center justify-center cursor-pointer" onclick="navigateTo('flipbook', '${book.id}')">
                    <img src="${coverImage}" alt="${book.title}" class="w-full h-full object-cover group-hover:scale-105 transition duration-500">
                    <div class="absolute inset-0 bg-gradient-to-t from-slate-900/80 via-transparent to-transparent"></div>
                    
                    <span class="absolute top-3 left-3 px-3 py-1 rounded-full bg-white/90 backdrop-blur-md text-[10px] font-extrabold text-slate-700 uppercase tracking-wider shadow-sm">
                        ${book.genre || 'Cerita Anak'}
                    </span>

                    <button onclick="event.stopPropagation(); toggleFavoriteBook('${book.id}')" class="absolute top-3 right-3 w-8 h-8 rounded-full bg-white/90 backdrop-blur-md flex items-center justify-center text-slate-400 hover:text-blue-600 transition shadow-sm">
                        <i class="fa-solid fa-star ${book.isFavorite ? 'text-blue-600' : ''}"></i>
                    </button>

                    <div class="absolute bottom-3 left-3 right-3 text-white">
                        <span class="text-[10px] font-bold text-sky-300 block uppercase tracking-wider">${book.grade || 'Semua Umur'} • ${book.pages ? book.pages.length : 0} Halaman</span>
                        <h4 class="text-base font-bold font-heading line-clamp-1 leading-snug drop-shadow">${book.title}</h4>
                    </div>
                </div>

                <div class="p-4 flex-grow flex flex-col justify-between space-y-3">
                    <div>
                        <p class="text-xs text-slate-500 line-clamp-2 leading-relaxed mb-2">${book.synopsis || 'Tidak ada sinopsis.'}</p>
                        <div class="flex items-center gap-2 text-[11px] font-medium text-slate-400">
                            <i class="fa-solid fa-pen-nib text-indigo-400"></i>
                            <span class="truncate">${book.author || 'Penulis Anonim'}</span>
                        </div>
                    </div>

                    <div class="pt-3 border-t border-slate-100 flex items-center justify-between gap-1">
                        <button onclick="navigateTo('flipbook', '${book.id}')" class="px-3 py-1.5 rounded-xl bg-indigo-50 hover:bg-indigo-100 text-indigo-600 text-xs font-bold transition flex items-center gap-1.5">
                            <i class="fa-solid fa-volume-high text-rose-500"></i> Baca & Dengar
                        </button>
                        <button onclick="navigateTo('studio', '${book.id}')" class="px-3 py-1.5 rounded-xl bg-sky-50 hover:bg-sky-100 text-sky-700 text-xs font-bold transition flex items-center gap-1.5">
                            <i class="fa-solid fa-pen-to-square"></i> Edit
                        </button>
                        <button onclick="exportBookPdf('${book.id}')" class="px-3 py-1.5 rounded-xl bg-emerald-50 hover:bg-emerald-100 text-emerald-700 text-xs font-bold transition flex items-center gap-1.5">
                            <i class="fa-solid fa-file-pdf"></i> PDF
                        </button>
                        <button onclick="confirmDeleteBook('${book.id}')" class="w-7 h-7 rounded-lg text-slate-300 hover:text-rose-500 hover:bg-rose-50 flex items-center justify-center transition">
                            <i class="fa-solid fa-trash-can text-xs"></i>
                        </button>
                    </div>
                </div>
            </div>`;
        }

        function handleSearchInput(val) { appState.filterSearch = val; renderApp(); }
        function handleGenreFilter(val) { appState.filterGenre = val; renderApp(); }
        function toggleFavoriteFilter() { appState.favoritesOnly = !appState.favoritesOnly; renderApp(); }
        function toggleFavoriteBook(bookId) {
            const book = appState.books.find(b => b.id === bookId);
            if (book) { book.isFavorite = !book.isFavorite; saveBooksToStorage(); renderApp(); }
        }
        function confirmDeleteBook(bookId) {
            if (confirm("Apakah Anda yakin ingin menghapus buku cerita ini?")) {
                appState.books = appState.books.filter(b => b.id !== bookId);
                saveBooksToStorage();
                renderApp();
            }
        }

        // -----------------------------------------------------------------
        // 2. FORMULIR PEMBUATAN CERITA (CREATE FORM WIZARD)
        // -----------------------------------------------------------------
        function renderCreateFormView() {
            return `
            <div class="max-w-4xl mx-auto space-y-6">
                <div class="flex items-center gap-4">
                    <button onclick="navigateTo('library')" class="w-10 h-10 rounded-2xl bg-white border border-slate-200 flex items-center justify-center text-slate-600 hover:bg-slate-50 transition">
                        <i class="fa-solid fa-arrow-left"></i>
                    </button>
                    <div>
                        <h2 class="text-2xl font-bold font-heading text-slate-800">Formulir Pembuatan Buku Cerita AI</h2>
                        <p class="text-xs font-semibold text-slate-400">Atur cerita, ilustrasi AI, dan narasi suara anak otomatis.</p>
                    </div>
                </div>

                <form id="form-create-story" onsubmit="handleCreateStorySubmit(event)" class="space-y-6">
                    <div class="bg-white rounded-3xl p-6 border border-slate-200 shadow-sm space-y-4">
                        <div class="flex items-center gap-2 border-b border-slate-100 pb-3">
                            <div class="w-7 h-7 rounded-xl bg-blue-100 text-blue-600 flex items-center justify-center text-xs">
                                <i class="fa-solid fa-id-card"></i>
                            </div>
                            <h3 class="text-base font-bold font-heading text-slate-800">A. Identitas Buku & Penulis</h3>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div class="sm:col-span-2">
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Gagasan Cerita / Judul (Opsional)</label>
                                <input type="text" id="field-title" placeholder="Misal: Kiko si Tupai Penyelamat Hutan (Biarkan kosong agar AI buatkan)" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Nama Penulis / Guru</label>
                                <input type="text" id="field-author" required placeholder="Contoh: Ibu Guru Rahma" value="Ibu Guru Rahma" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Nama Sekolah / Instansi</label>
                                <input type="text" id="field-school" placeholder="Contoh: SD Negeri 1 Ceria" value="SD Negeri 1 Ceria" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Kelas / Jenjang</label>
                                <select id="field-grade" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm bg-white focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                    <option value="TK / PAUD">TK / PAUD</option>
                                    <option value="Kelas 1-2 SD" selected>Kelas 1 - 2 SD</option>
                                    <option value="Kelas 3-4 SD">Kelas 3 - 4 SD</option>
                                    <option value="Kelas 5-6 SD">Kelas 5 - 6 SD</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Usia Pembaca</label>
                                <select id="field-age" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm bg-white focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                    <option value="4-6 Tahun">4 - 6 Tahun</option>
                                    <option value="7-9 Tahun" selected>7 - 9 Tahun</option>
                                    <option value="10-12 Tahun">10 - 12 Tahun</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="bg-white rounded-3xl p-6 border border-slate-200 shadow-sm space-y-4">
                        <div class="flex items-center gap-2 border-b border-slate-100 pb-3">
                            <div class="w-7 h-7 rounded-xl bg-rose-100 text-rose-600 flex items-center justify-center text-xs">
                                <i class="fa-solid fa-masks-theater"></i>
                            </div>
                            <h3 class="text-base font-bold font-heading text-slate-800">B. Jenis & Tema Cerita</h3>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Jenis Cerita</label>
                                <select id="field-genre" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm bg-white focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                    <option value="Fabel (cerita binatang)" selected>Fabel (Cerita Binatang)</option>
                                    <option value="Dongeng">Dongeng Fantasi</option>
                                    <option value="Petualangan">Petualangan Seru</option>
                                    <option value="Cerita persahabatan">Persahabatan</option>
                                    <option value="Cerita keluarga">Keluarga & Kasih Sayang</option>
                                    <option value="Cerita pendidikan">Pendidikan & Sekolah</option>
                                    <option value="Cerita islami">Cerita Islami / Nilai Agama</option>
                                </select>
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Pesan Moral Utama</label>
                                <select id="field-theme" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm bg-white focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                    <option value="Gotong Royong & Kerja Sama" selected>Gotong Royong & Kerja Sama</option>
                                    <option value="Kejujuran & Tanggung Jawab">Kejujuran & Tanggung Jawab</option>
                                    <option value="Kedisiplinan & Rajin Belajar">Kedisiplinan & Rajin Belajar</option>
                                    <option value="Menghormati Orang Tua & Guru">Menghormati Orang Tua & Guru</option>
                                    <option value="Menjaga Lingkungan Alam">Menjaga Lingkungan Alam</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <div class="bg-white rounded-3xl p-6 border border-slate-200 shadow-sm space-y-4">
                        <div class="flex items-center gap-2 border-b border-slate-100 pb-3">
                            <div class="w-7 h-7 rounded-xl bg-indigo-100 text-indigo-600 flex items-center justify-center text-xs">
                                <i class="fa-solid fa-palette"></i>
                            </div>
                            <h3 class="text-base font-bold font-heading text-slate-800">C. Format & Konsistensi Ilustrasi AI</h3>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Jumlah Halaman Cerita</label>
                                <select id="field-pagecount" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm bg-white focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                    <option value="6" selected>6 Halaman</option>
                                    <option value="8">8 Halaman</option>
                                    <option value="10">10 Halaman</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Bahasa Naskah</label>
                                <select id="field-lang" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm bg-white focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                    <option value="Indonesia" selected>Bahasa Indonesia</option>
                                    <option value="Sunda">Bahasa Sunda</option>
                                    <option value="Inggris">Bahasa Inggris (English)</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Gaya Gambar Ilustrasi</label>
                                <select id="field-style" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm bg-white focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                    <option value="Kartun 3D" selected>Kartun 3D Lucu</option>
                                    <option value="Watercolor">Cat Air (Watercolor)</option>
                                    <option value="Anime Anak">Anime Anak-Anak</option>
                                    <option value="Kartun 2D">Kartun 2D Buku Dongeng</option>
                                    <option value="Semi-realistis">Semi-Realistis</option>
                                </select>
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Deskripsi Karakter Utama (Character Reference)</label>
                            <textarea id="field-char-ref" rows="2" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 text-sm focus:ring-2 focus:ring-indigo-500 focus:outline-none">Kancil cerdik berbulu cokelat keemasan memakai kaos merah muda dan ramah</textarea>
                        </div>
                    </div>

                    <div class="flex justify-end gap-3 pt-2">
                        <button type="button" onclick="navigateTo('library')" class="px-6 py-3 rounded-2xl border border-slate-200 font-bold text-sm text-slate-600 hover:bg-slate-50 transition">Batal</button>
                        <button type="submit" class="px-8 py-3.5 rounded-2xl bg-gradient-to-r from-blue-600 via-indigo-600 to-slate-800 text-white font-extrabold text-sm shadow-xl hover:opacity-95 transition flex items-center gap-2">
                            <i class="fa-solid fa-wand-magic-sparkles text-sky-300"></i>
                            <span>Sihir AI: Buat Cerita & Gambar!</span>
                        </button>
                    </div>
                </form>
            </div>`;
        }

        // -----------------------------------------------------------------
        // 3. AI GENERATION WORKFLOW (TEXT & IMAGE GENERATOR)
        // -----------------------------------------------------------------
        async function handleCreateStorySubmit(e) {
            e.preventDefault();
            
            const titleInput = document.getElementById('field-title').value.trim();
            const author = document.getElementById('field-author').value.trim() || 'Penulis';
            const school = document.getElementById('field-school').value.trim() || 'Sekolah Dasar';
            const grade = document.getElementById('field-grade').value;
            const age = document.getElementById('field-age').value;
            const genre = document.getElementById('field-genre').value;
            const theme = document.getElementById('field-theme').value;
            const pageCount = parseInt(document.getElementById('field-pagecount').value, 10) || 6;
            const language = document.getElementById('field-lang').value;
            const style = document.getElementById('field-style').value;
            const charRef = document.getElementById('field-char-ref').value.trim();

            showProgressModal("Menulis cerita lengkap & menyusun halaman dengan Gemini AI...", 10);

            try {
                const storyData = await generateStoryTextFromGemini({
                    titleInput, author, school, grade, age, genre, theme, pageCount, language, style, charRef
                });

                updateProgress("Cerita berhasil ditulis! Sekarang melukis ilustrasi AI...", 40);

                const pagesWithImages = [];
                const totalPages = storyData.pages.length;

                for (let i = 0; i < totalPages; i++) {
                    const page = storyData.pages[i];
                    const percent = 40 + Math.floor(((i + 1) / totalPages) * 55);
                    updateProgress(`Melukis Gambar Halaman ${i + 1} dari ${totalPages}...`, percent);

                    const imgPrompt = `${style} illustration for children storybook page ${i + 1}. Main Character details: (${charRef}). Scene description: ${page.illustrationPrompt}. Bright colors, simple kid-friendly visual, high quality, no text in image.`;
                    
                    let imageUrl = await generateImageFromGemini(imgPrompt, style, page.illustrationPrompt);
                    
                    pagesWithImages.push({
                        pageNumber: i + 1,
                        text: page.text,
                        illustrationPrompt: page.illustrationPrompt,
                        imageUrl: imageUrl,
                        bgColor: '#FFFFFF',
                        textColor: '#1E293B',
                        fontSize: 'medium'
                    });
                }

                const newBook = {
                    id: 'book_' + Date.now(),
                    title: storyData.title || titleInput || 'Buku Cerita Anak',
                    author: author,
                    school: school,
                    grade: grade,
                    targetAge: age,
                    genre: genre,
                    theme: theme,
                    language: language,
                    illustrationStyle: style,
                    characterRef: charRef,
                    orientation: 'portrait',
                    pageSize: 'A4',
                    synopsis: storyData.synopsis || 'Cerita seru sarat pesan kebaikan untuk anak.',
                    moralMessage: storyData.moralMessage || 'Bekerja sama dan selalu berbuat baik.',
                    createdAt: new Date().toISOString(),
                    isFavorite: false,
                    pages: pagesWithImages,
                    quiz: storyData.quiz || [],
                    glossary: storyData.glossary || []
                };

                appState.books.unshift(newBook);
                saveBooksToStorage();

                updateProgress("Buku & Suara Selesai Dibuat!", 100);
                setTimeout(() => {
                    hideProgressModal();
                    navigateTo('studio', newBook.id);
                }, 800);

            } catch (err) {
                console.error("AI Generation Error:", err);
                hideProgressModal();
                alert("Sistem akan menggunakan mode draf cerita otomatis.");
                createFallbackBook(titleInput, author, school, grade, age, genre, theme, pageCount, language, style, charRef);
            }
        }

        async function generateStoryTextFromGemini(params) {
            const systemPrompt = "Anda adalah penulis cerita anak profesional. Buatkan cerita anak yang edukatif, tanpa kekerasan, memiliki struktur cerita lengkap (awal, konflik, penyelesaian, penutup), serta dilengkapi pesan moral, kuis pemahaman, dan glosarium kata.";
            const userPrompt = `Buatkan cerita anak:
- Judul: ${params.titleInput || 'Buatkan judul menarik'}
- Penulis: ${params.author}
- Usia: ${params.age}
- Jenis Cerita: ${params.genre}
- Tema: ${params.theme}
- Jumlah Halaman: ${params.pageCount}
- Bahasa: ${params.language}
- Tokoh Utama: ${params.charRef}

Berikan output JSON SAJA tanpa format markdown lain dengan struktur:
{
  "title": "Judul Cerita",
  "synopsis": "Sinopsis singkat cerita",
  "moralMessage": "Pesan moral cerita",
  "pages": [
    {
      "pageNumber": 1,
      "text": "Narasi cerita untuk halaman 1",
      "illustrationPrompt": "Detailed scene description in English for AI image generation"
    }
  ],
  "quiz": [
    {
      "question": "Pertanyaan pemahaman?",
      "options": ["Pilihan A", "Pilihan B", "Pilihan C", "Pilihan D"],
      "answerIndex": 0
    }
  ],
  "glossary": [
    {
      "word": "Kata Sulit",
      "meaning": "Arti kata"
    }
  ]
}`;

            const apiKey = appState.customApiKey || "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-flash-preview:generateContent?key=${apiKey}`;

            const payload = {
                contents: [{ parts: [{ text: userPrompt }] }],
                systemInstruction: { parts: [{ text: systemPrompt }] },
                generationConfig: {
                    responseMimeType: "application/json"
                }
            };

            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });

            if (!response.ok) {
                throw new Error("Gagal menghubungi API Gemini: " + response.statusText);
            }

            const result = await response.json();
            const textResult = result.candidates?.[0]?.content?.parts?.[0]?.text;
            if (!textResult) throw new Error("Format balasan API Gemini tidak sesuai.");

            return JSON.parse(textResult);
        }

        async function generateImageFromGemini(fullPrompt, styleName, sceneTitle) {
            const apiKey = appState.customApiKey || "";
            if (apiKey) {
                try {
                    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3.1-flash-image:generateContent?key=${apiKey}`;
                    const payload = {
                        contents: [{ parts: [{ text: fullPrompt }] }],
                        generationConfig: {
                            responseModalities: ['IMAGE']
                        }
                    };
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    if (response.ok) {
                        const result = await response.json();
                        const part = result?.candidates?.[0]?.content?.parts?.find(p => p.inlineData);
                        if (part && part.inlineData) {
                            return `data:${part.inlineData.mimeType};base64,${part.inlineData.data}`;
                        }
                    }
                } catch (e) {
                    console.warn("Image API fallback used:", e);
                }
            }
            return generateDynamicSvg(sceneTitle || 'Ilustrasi Halaman', '#BAE6FD', '🎨✨', styleName);
        }

        function createFallbackBook(titleInput, author, school, grade, age, genre, theme, pageCount, language, style, charRef) {
            const title = titleInput || `Petualangan Hebat di Hutan Ceria`;
            const pages = [];
            for (let i = 1; i <= pageCount; i++) {
                pages.push({
                    pageNumber: i,
                    text: `Halaman ${i}: Di sebuah tempat yang indah, tokoh utama belajar tentang ${theme.toLowerCase()}. Setiap hari adalah petualangan baru bersama sahabat.`,
                    illustrationPrompt: `Page ${i} of ${title}, character: ${charRef}`,
                    imageUrl: generateDynamicSvg(`Adegan Halaman ${i}`, '#DDD6FE', '📖🌟', style),
                    bgColor: '#FFFFFF',
                    textColor: '#1E293B',
                    fontSize: 'medium'
                });
            }

            const fallbackBook = {
                id: 'book_' + Date.now(),
                title: title,
                author: author,
                school: school,
                grade: grade,
                targetAge: age,
                genre: genre,
                theme: theme,
                language: language,
                illustrationStyle: style,
                characterRef: charRef,
                orientation: 'portrait',
                pageSize: 'A4',
                synopsis: `Kisah fabel inspiratif tentang ${theme.toLowerCase()}.`,
                moralMessage: `Selalu utamakan ${theme.toLowerCase()} dalam kehidupan sehari-hari.`,
                createdAt: new Date().toISOString(),
                isFavorite: false,
                pages: pages,
                quiz: [ { question: `Pesan moral cerita?`, options: [theme, 'Bermain terus', 'Marah'], answerIndex: 0 } ],
                glossary: [ { word: 'Sahabat', meaning: 'Kawan dekat yang menyayangi.' } ]
            };

            appState.books.unshift(fallbackBook);
            saveBooksToStorage();
            navigateTo('studio', fallbackBook.id);
        }

        function showProgressModal(msg, percent) {
            document.getElementById('modal-progress').classList.remove('hidden');
            updateProgress(msg, percent);
        }
        function updateProgress(msg, percent) {
            document.getElementById('progress-status-text').innerText = msg;
            document.getElementById('progress-bar-fill').style.width = percent + '%';
            document.getElementById('progress-percent').innerText = percent + '%';
        }
        function hideProgressModal() {
            document.getElementById('modal-progress').classList.add('hidden');
        }

        // -----------------------------------------------------------------
        // 4. FITUR NARA SUARA (AUDIO NARRATION & GEMINI TTS ENGINE)
        // -----------------------------------------------------------------
        async function playNarratorVoice(textToRead, onEndedCallback = null) {
            stopAudioNarration();

            if (!textToRead || textToRead.trim() === '') return;

            appState.isAudioPlaying = true;
            renderAudioControlsUI(true);

            // TRY GEMINI TTS FIRST IF API KEY PRESENT OR AVAILABLE
            const apiKey = appState.customApiKey || "";
            if (apiKey) {
                try {
                    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`;
                    const promptText = `Bacakan cerita anak berikut ini dengan suara hangat, jelas, riang, dan perlahan agar disukai anak-anak: "${textToRead}"`;

                    const payload = {
                        contents: [{ parts: [{ text: promptText }] }],
                        generationConfig: {
                            responseModalities: ["AUDIO"],
                            speechConfig: {
                                voiceConfig: {
                                    prebuiltVoiceConfig: { voiceName: appState.selectedVoice || 'Puck' }
                                }
                            }
                        }
                    };

                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    if (response.ok) {
                        const result = await response.json();
                        const part = result?.candidates?.[0]?.content?.parts?.[0];
                        const audioDataBase64 = part?.inlineData?.data;
                        const mimeType = part?.inlineData?.mimeType || 'audio/L16;rate=24000';

                        if (audioDataBase64) {
                            const rateMatch = mimeType.match(/rate=(\d+)/);
                            const sampleRate = rateMatch ? parseInt(rateMatch[1], 10) : 24000;
                            const pcmBuffer = base64ToArrayBuffer(audioDataBase64);
                            const pcm16 = new Int16Array(pcmBuffer);
                            const wavBlob = pcmToWav(pcm16, sampleRate);
                            const audioUrl = URL.createObjectURL(wavBlob);

                            const audio = new Audio(audioUrl);
                            appState.activeAudioElement = audio;

                            audio.onended = () => {
                                appState.isAudioPlaying = false;
                                renderAudioControlsUI(false);
                                if (onEndedCallback) onEndedCallback();
                            };

                            audio.onerror = () => {
                                playWebSpeechFallback(textToRead, onEndedCallback);
                            };

                            await audio.play();
                            return;
                        }
                    }
                } catch (err) {
                    console.warn("Gemini TTS API error, falling back to Web Speech API:", err);
                }
            }

            // FALLBACK TO BROWSER NATIVE WEB SPEECH SYNTHESIS
            playWebSpeechFallback(textToRead, onEndedCallback);
        }

        function playWebSpeechFallback(textToRead, onEndedCallback) {
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const utterance = new SpeechSynthesisUtterance(textToRead);
                
                const book = appState.books.find(b => b.id === appState.currentBookId);
                utterance.lang = book && book.language === 'Inggris' ? 'en-US' : 'id-ID';
                utterance.rate = 0.9; // Friendly kid speed

                utterance.onend = () => {
                    appState.isAudioPlaying = false;
                    renderAudioControlsUI(false);
                    if (onEndedCallback) onEndedCallback();
                };

                utterance.onerror = () => {
                    appState.isAudioPlaying = false;
                    renderAudioControlsUI(false);
                };

                window.speechSynthesis.speak(utterance);
            } else {
                appState.isAudioPlaying = false;
                renderAudioControlsUI(false);
                alert("Perangkat Anda tidak mendukung fitur pemutar suara otomatis.");
            }
        }

        function stopAudioNarration() {
            appState.isAudioPlaying = false;
            if (appState.activeAudioElement) {
                appState.activeAudioElement.pause();
                appState.activeAudioElement = null;
            }
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
            }
            renderAudioControlsUI(false);
        }

        function toggleNarrationVoice() {
            if (appState.isAudioPlaying) {
                stopAudioNarration();
            } else {
                readCurrentSlideNarration();
            }
        }

        function readCurrentSlideNarration() {
            const book = appState.books.find(b => b.id === appState.currentBookId);
            if (!book) return;

            const totalSlides = book.pages.length + 2;
            let textToRead = "";

            if (flipCurrentIdx === 0) {
                textToRead = `Buku cerita berjudul ${book.title}, ditulis oleh ${book.author}. Selamat mendengarkan!`;
            } else if (flipCurrentIdx === totalSlides - 1) {
                textToRead = `Akhir cerita. Pesan moral dari buku ini adalah: ${book.moralMessage}`;
            } else {
                const currentPage = book.pages[flipCurrentIdx - 1];
                textToRead = currentPage ? currentPage.text : "";
            }

            playNarratorVoice(textToRead, () => {
                if (appState.autoPlayMode && flipCurrentIdx < totalSlides - 1) {
                    setTimeout(() => {
                        flipNextSlide();
                        readCurrentSlideNarration();
                    }, 1200);
                }
            });
        }

        function toggleAutoPlayDongeng() {
            appState.autoPlayMode = !appState.autoPlayMode;
            if (appState.autoPlayMode && !appState.isAudioPlaying) {
                readCurrentSlideNarration();
            } else if (!appState.autoPlayMode) {
                stopAudioNarration();
            }
            renderApp();
        }

        function renderAudioControlsUI(isPlaying) {
            const playBtnNode = document.getElementById('btn-play-audio-narration');
            const waveNode = document.getElementById('narration-wave-anim');
            if (playBtnNode) {
                playBtnNode.innerHTML = isPlaying 
                    ? `<i class="fa-solid fa-pause text-rose-500"></i> Hentikan Suara`
                    : `<i class="fa-solid fa-volume-high text-rose-500"></i> Putar Suara Narasi`;
            }
            if (waveNode) {
                waveNode.className = isPlaying ? "flex items-center gap-1 px-2" : "hidden";
            }
        }

        // Helper Audio PCM16 to WAV Converter
        function pcmToWav(pcm16Data, sampleRate) {
            const numChannels = 1;
            const byteRate = sampleRate * numChannels * 2;
            const blockAlign = numChannels * 2;
            const dataSize = pcm16Data.length * 2;
            const buffer = new ArrayBuffer(44 + dataSize);
            const view = new DataView(buffer);

            writeString(view, 0, 'RIFF');
            view.setUint32(4, 36 + dataSize, true);
            writeString(view, 8, 'WAVE');
            writeString(view, 12, 'fmt ');
            view.setUint32(16, 16, true);
            view.setUint16(20, 1, true);
            view.setUint16(22, numChannels, true);
            view.setUint32(24, sampleRate, true);
            view.setUint32(28, byteRate, true);
            view.setUint16(32, blockAlign, true);
            view.setUint16(34, 16, true);
            writeString(view, 36, 'data');
            view.setUint32(40, dataSize, true);

            let offset = 44;
            for (let i = 0; i < pcm16Data.length; i++, offset += 2) {
                view.setInt16(offset, pcm16Data[i], true);
            }
            return new Blob([buffer], { type: 'audio/wav' });
        }
        function writeString(view, offset, string) {
            for (let i = 0; i < string.length; i++) {
                view.setUint8(offset + i, string.charCodeAt(i));
            }
        }
        function base64ToArrayBuffer(base64) {
            const binaryString = atob(base64);
            const len = binaryString.length;
            const bytes = new Uint8Array(len);
            for (let i = 0; i < len; i++) {
                bytes[i] = binaryString.charCodeAt(i);
            }
            return bytes.buffer;
        }

        // -----------------------------------------------------------------
        // 5. EDITOR BUKU CERITA (STUDIO EDITOR VIEW)
        // -----------------------------------------------------------------
        let activePageIndex = 0;

        function renderStudioEditorView() {
            const book = appState.books.find(b => b.id === appState.currentBookId);
            if (!book) return `<div class="p-8 text-center">Buku tidak ditemukan.</div>`;

            const totalPages = book.pages.length;
            if (activePageIndex > totalPages) activePageIndex = totalPages;

            const isCover = activePageIndex === 0;
            const currentPage = isCover ? null : book.pages[activePageIndex - 1];

            return `
            <div class="space-y-4">
                <div class="bg-white p-3 sm:p-4 rounded-2xl border border-slate-200 shadow-sm flex flex-wrap items-center justify-between gap-3">
                    <div class="flex items-center gap-3">
                        <button onclick="navigateTo('library')" class="w-9 h-9 rounded-xl border border-slate-200 flex items-center justify-center text-slate-600 hover:bg-slate-50 transition">
                            <i class="fa-solid fa-arrow-left"></i>
                        </button>
                        <div>
                            <span class="text-[10px] font-bold text-slate-400 uppercase tracking-wider">Studio Editor Buku</span>
                            <h3 class="text-base font-bold text-slate-800 font-heading line-clamp-1">${book.title}</h3>
                        </div>
                    </div>

                    <div class="flex flex-wrap items-center gap-2">
                        <button onclick="navigateTo('flipbook', '${book.id}')" class="px-3.5 py-2 rounded-xl bg-indigo-50 hover:bg-indigo-100 text-indigo-600 text-xs font-bold transition flex items-center gap-1.5">
                            <i class="fa-solid fa-eye"></i> Pratinjau & Suara
                        </button>
                        <button onclick="navigateTo('quiz', '${book.id}')" class="px-3.5 py-2 rounded-xl bg-purple-50 hover:bg-purple-100 text-purple-600 text-xs font-bold transition flex items-center gap-1.5">
                            <i class="fa-solid fa-graduation-cap"></i> Kuis & Glosarium
                        </button>
                        <button onclick="exportBookPdf('${book.id}')" class="px-4 py-2 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-extrabold shadow-sm transition flex items-center gap-1.5">
                            <i class="fa-solid fa-file-pdf"></i> Cetak PDF
                        </button>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                    <div class="lg:col-span-3 bg-white p-4 rounded-3xl border border-slate-200 shadow-sm space-y-3 max-h-[750px] overflow-y-auto">
                        <div class="flex items-center justify-between pb-2 border-b border-slate-100">
                            <span class="text-xs font-extrabold text-slate-700 uppercase tracking-wider">Halaman Buku</span>
                            <button onclick="addNewPage('${book.id}')" class="px-2.5 py-1 rounded-lg bg-indigo-50 text-indigo-600 hover:bg-indigo-100 text-xs font-bold transition flex items-center gap-1">
                                <i class="fa-solid fa-plus text-[10px]"></i> Tambah
                            </button>
                        </div>

                        <div onclick="switchStudioPage(0)" class="p-2.5 rounded-2xl border ${activePageIndex === 0 ? 'border-indigo-500 bg-indigo-50/50 ring-2 ring-indigo-200' : 'border-slate-200 hover:border-slate-300'} cursor-pointer transition flex items-center gap-3">
                            <div class="w-12 h-14 bg-slate-200 rounded-xl overflow-hidden flex-shrink-0">
                                <img src="${book.pages[0]?.imageUrl || ''}" class="w-full h-full object-cover">
                            </div>
                            <div>
                                <span class="text-xs font-bold text-slate-800 block">Sampul Depan</span>
                                <span class="text-[10px] font-semibold text-slate-400">Judul & Sampul</span>
                            </div>
                        </div>

                        ${book.pages.map((p, idx) => `
                            <div onclick="switchStudioPage(${idx + 1})" class="p-2.5 rounded-2xl border ${activePageIndex === idx + 1 ? 'border-indigo-500 bg-indigo-50/50 ring-2 ring-indigo-200' : 'border-slate-200 hover:border-slate-300'} cursor-pointer transition flex items-center justify-between group">
                                <div class="flex items-center gap-3">
                                    <div class="w-12 h-14 bg-slate-100 rounded-xl overflow-hidden flex-shrink-0 border border-slate-200">
                                        <img src="${p.imageUrl}" class="w-full h-full object-cover">
                                    </div>
                                    <div>
                                        <span class="text-xs font-bold text-slate-800 block">Halaman ${p.pageNumber}</span>
                                        <span class="text-[10px] font-semibold text-slate-400 line-clamp-1">${p.text}</span>
                                    </div>
                                </div>
                                ${book.pages.length > 2 ? `
                                    <button onclick="event.stopPropagation(); deleteStudioPage(${idx})" class="w-7 h-7 rounded-lg text-slate-300 hover:text-rose-500 hover:bg-rose-50 flex items-center justify-center opacity-0 group-hover:opacity-100 transition">
                                        <i class="fa-solid fa-trash-can text-xs"></i>
                                    </button>
                                ` : ''}
                            </div>
                        `).join('')}
                    </div>

                    <div class="lg:col-span-6 bg-slate-100 rounded-3xl p-4 sm:p-8 border border-slate-200 flex flex-col items-center justify-center min-h-[550px] relative overflow-hidden">
                        <div class="w-full max-w-md bg-white rounded-2xl shadow-xl overflow-hidden border border-slate-200 transition-all duration-300" style="background-color: ${isCover ? '#FFFFFF' : (currentPage.bgColor || '#FFFFFF')}; text-color: ${isCover ? '#1E293B' : (currentPage.textColor || '#1E293B')};">
                            ${isCover ? `
                                <div class="p-6 text-center space-y-4">
                                    <div class="border-b border-slate-100 pb-2">
                                        <span class="text-[10px] font-extrabold text-indigo-600 tracking-widest uppercase block">${book.school}</span>
                                        <h2 class="text-2xl font-extrabold font-heading text-slate-900 leading-tight">${book.title}</h2>
                                        <span class="text-xs font-semibold text-slate-500">Karya: ${book.author}</span>
                                    </div>
                                    <div class="aspect-square rounded-2xl overflow-hidden border border-slate-200 shadow-inner">
                                        <img src="${book.pages[0]?.imageUrl}" class="w-full h-full object-cover">
                                    </div>
                                </div>
                            ` : `
                                <div class="p-6 space-y-4">
                                    <div class="relative group aspect-4/3 rounded-2xl overflow-hidden border border-slate-200 bg-slate-50 shadow-inner">
                                        <img src="${currentPage.imageUrl}" class="w-full h-full object-cover">
                                        <div class="absolute inset-0 bg-slate-900/40 opacity-0 group-hover:opacity-100 transition flex items-center justify-center gap-2">
                                            <button onclick="regeneratePageImage(${activePageIndex - 1})" class="px-3 py-1.5 rounded-xl bg-white text-slate-800 text-xs font-bold shadow-lg hover:bg-sky-100 transition flex items-center gap-1">
                                                <i class="fa-solid fa-wand-magic-sparkles text-sky-600"></i> AI Regenerate
                                            </button>
                                            <label class="px-3 py-1.5 rounded-xl bg-white text-slate-800 text-xs font-bold shadow-lg hover:bg-slate-100 cursor-pointer transition flex items-center gap-1">
                                                <i class="fa-solid fa-upload text-indigo-500"></i> Upload
                                                <input type="file" accept="image/*" class="hidden" onchange="uploadCustomImage(event, ${activePageIndex - 1})">
                                            </label>
                                        </div>
                                    </div>

                                    <div>
                                        <div class="flex items-center justify-between mb-1">
                                            <label class="block text-[10px] font-bold text-slate-400 uppercase tracking-wider">Narasi Cerita Halaman ${currentPage.pageNumber}</label>
                                            <button onclick="playNarratorVoice('${currentPage.text.replace(/'/g, "\\'")}')" class="text-[10px] font-bold text-rose-500 hover:underline flex items-center gap-1">
                                                <i class="fa-solid fa-volume-high"></i> Tes Suara
                                            </button>
                                        </div>
                                        <textarea onchange="updatePageText(${activePageIndex - 1}, this.value)" rows="4" class="w-full p-3 rounded-xl border border-slate-200 text-sm font-medium focus:ring-2 focus:ring-indigo-500 focus:outline-none leading-relaxed bg-white/80" style="color: ${currentPage.textColor || '#1E293B'}">${currentPage.text}</textarea>
                                    </div>
                                </div>
                            `}
                        </div>
                    </div>

                    <div class="lg:col-span-3 bg-white p-5 rounded-3xl border border-slate-200 shadow-sm space-y-5">
                        <h4 class="text-xs font-extrabold text-slate-700 uppercase tracking-wider border-b border-slate-100 pb-2">Pengaturan Layout & Gaya</h4>

                        ${isCover ? `
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Judul Buku</label>
                                    <input type="text" value="${book.title}" onchange="updateBookTitle('${book.id}', this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-200 text-xs font-semibold">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Nama Penulis</label>
                                    <input type="text" value="${book.author}" onchange="updateBookAuthor('${book.id}', this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-200 text-xs font-semibold">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Sinopsis Ringkas</label>
                                    <textarea rows="3" onchange="updateBookSynopsis('${book.id}', this.value)" class="w-full px-3 py-2 rounded-xl border border-slate-200 text-xs font-medium">${book.synopsis}</textarea>
                                </div>
                            </div>
                        ` : `
                            <div class="space-y-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-1">Prompt Ilustrasi AI</label>
                                    <textarea rows="3" onchange="updatePagePrompt(${activePageIndex - 1}, this.value)" class="w-full p-2.5 rounded-xl border border-slate-200 text-xs font-mono text-slate-600">${currentPage.illustrationPrompt}</textarea>
                                </div>

                                <div>
                                    <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Warna Latar Halaman</label>
                                    <div class="flex items-center gap-2">
                                        ${['#FFFFFF', '#F0F9FF', '#E0F2FE', '#EFF6FF', '#F0FDF4', '#F5F3FF'].map(hex => `
                                            <button onclick="updatePageBgColor(${activePageIndex - 1}, '${hex}')" class="w-7 h-7 rounded-full border border-slate-300 shadow-sm transition transform hover:scale-110" style="background-color: ${hex}"></button>
                                        `).join('')}
                                    </div>
                                </div>

                                <div>
                                    <label class="block text-xs font-bold text-slate-700 uppercase tracking-wider mb-2">Warna Teks</label>
                                    <div class="flex items-center gap-2">
                                        ${['#1E293B', '#1E3A8A', '#0369A1', '#1E40AF', '#166534', '#5B21B6'].map(hex => `
                                            <button onclick="updatePageTextColor(${activePageIndex - 1}, '${hex}')" class="w-7 h-7 rounded-full border border-slate-300 shadow-sm transition transform hover:scale-110" style="background-color: ${hex}"></button>
                                        `).join('')}
                                    </div>
                                </div>
                            </div>
                        `}
                    </div>
                </div>
            </div>`;
        }

        function switchStudioPage(idx) { activePageIndex = idx; renderApp(); }
        function updateBookTitle(bookId, val) { const b = appState.books.find(x => x.id === bookId); if (b) { b.title = val; saveBooksToStorage(); renderApp(); } }
        function updateBookAuthor(bookId, val) { const b = appState.books.find(x => x.id === bookId); if (b) { b.author = val; saveBooksToStorage(); renderApp(); } }
        function updateBookSynopsis(bookId, val) { const b = appState.books.find(x => x.id === bookId); if (b) { b.synopsis = val; saveBooksToStorage(); renderApp(); } }
        function updatePageText(pageIdx, val) { const b = appState.books.find(x => x.id === appState.currentBookId); if (b && b.pages[pageIdx]) { b.pages[pageIdx].text = val; saveBooksToStorage(); } }
        function updatePagePrompt(pageIdx, val) { const b = appState.books.find(x => x.id === appState.currentBookId); if (b && b.pages[pageIdx]) { b.pages[pageIdx].illustrationPrompt = val; saveBooksToStorage(); } }
        function updatePageBgColor(pageIdx, hex) { const b = appState.books.find(x => x.id === appState.currentBookId); if (b && b.pages[pageIdx]) { b.pages[pageIdx].bgColor = hex; saveBooksToStorage(); renderApp(); } }
        function updatePageTextColor(pageIdx, hex) { const b = appState.books.find(x => x.id === appState.currentBookId); if (b && b.pages[pageIdx]) { b.pages[pageIdx].textColor = hex; saveBooksToStorage(); renderApp(); } }

        async function regeneratePageImage(pageIdx) {
            const book = appState.books.find(b => b.id === appState.currentBookId);
            if (!book || !book.pages[pageIdx]) return;
            showProgressModal(`Memproses gambar AI Halaman ${pageIdx + 1}...`, 40);
            const page = book.pages[pageIdx];
            const fullPrompt = `${book.illustrationStyle} children storybook illustration. Main character: ${book.characterRef}. Scene: ${page.illustrationPrompt}. High quality, colorful.`;
            const newImg = await generateImageFromGemini(fullPrompt, book.illustrationStyle, page.illustrationPrompt);
            page.imageUrl = newImg;
            saveBooksToStorage();
            hideProgressModal();
            renderApp();
        }

        function uploadCustomImage(event, pageIdx) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                const book = appState.books.find(b => b.id === appState.currentBookId);
                if (book && book.pages[pageIdx]) {
                    book.pages[pageIdx].imageUrl = e.target.result;
                    saveBooksToStorage();
                    renderApp();
                }
            };
            reader.readAsDataURL(file);
        }

        function addNewPage(bookId) {
            const book = appState.books.find(b => b.id === bookId);
            if (!book) return;
            const newNum = book.pages.length + 1;
            book.pages.push({
                pageNumber: newNum,
                text: `Halaman ${newNum}: Tulis narasi cerita baru Anda di sini...`,
                illustrationPrompt: `New scene for story page ${newNum}`,
                imageUrl: generateDynamicSvg(`Adegan Halaman ${newNum}`, '#E0F2FE', '🎨', book.illustrationStyle),
                bgColor: '#FFFFFF',
                textColor: '#1E293B',
                fontSize: 'medium'
            });
            saveBooksToStorage();
            activePageIndex = newNum;
            renderApp();
        }

        function deleteStudioPage(pageIdx) {
            const book = appState.books.find(b => b.id === appState.currentBookId);
            if (!book || book.pages.length <= 2) return;
            book.pages.splice(pageIdx, 1);
            book.pages.forEach((p, idx) => p.pageNumber = idx + 1);
            saveBooksToStorage();
            activePageIndex = Math.max(0, pageIdx);
            renderApp();
        }

        // -----------------------------------------------------------------
        // 6. PRATINJAU BUKU INTERAKTIF & PEMUTAR SUARA NARASI
        // -----------------------------------------------------------------
        let flipCurrentIdx = 0;

        function renderFlipbookPreviewView() {
            const book = appState.books.find(b => b.id === appState.currentBookId);
            if (!book) return `<div class="p-8 text-center">Buku tidak ditemukan.</div>`;

            const totalSlides = book.pages.length + 2;
            if (flipCurrentIdx >= totalSlides) flipCurrentIdx = totalSlides - 1;

            const isFrontCover = flipCurrentIdx === 0;
            const isBackCover = flipCurrentIdx === totalSlides - 1;
            const currentPage = (!isFrontCover && !isBackCover) ? book.pages[flipCurrentIdx - 1] : null;

            return `
            <div class="max-w-4xl mx-auto space-y-6">
                <!-- Header Toolbar & Sound Bar -->
                <div class="flex flex-col sm:flex-row items-center justify-between bg-white p-4 rounded-2xl border border-slate-200 shadow-sm gap-3">
                    <div class="flex items-center gap-3">
                        <button onclick="navigateTo('studio', '${book.id}')" class="w-9 h-9 rounded-xl border border-slate-200 flex items-center justify-center text-slate-600 hover:bg-slate-50 transition">
                            <i class="fa-solid fa-arrow-left"></i>
                        </button>
                        <div>
                            <span class="text-[10px] font-bold text-sky-600 uppercase tracking-wider">Pratinjau Cerita & Suara Narasi</span>
                            <h3 class="text-base font-bold text-slate-800 font-heading">${book.title}</h3>
                        </div>
                    </div>

                    <!-- Voice Controls -->
                    <div class="flex flex-wrap items-center gap-2">
                        <!-- Sound Wave Visualizer Badge -->
                        <div id="narration-wave-anim" class="${appState.isAudioPlaying ? 'flex items-center gap-1 px-2' : 'hidden'}">
                            <span class="sound-wave-bar"></span>
                            <span class="sound-wave-bar"></span>
                            <span class="sound-wave-bar"></span>
                            <span class="sound-wave-bar"></span>
                        </div>

                        <!-- Voice Select -->
                        <select onchange="appState.selectedVoice = this.value" class="px-2.5 py-1.5 rounded-xl border border-slate-200 text-xs font-semibold text-slate-700 bg-slate-50">
                            <option value="Puck" ${appState.selectedVoice === 'Puck' ? 'selected' : ''}>Suara Puck (Ceria)</option>
                            <option value="Kore" ${appState.selectedVoice === 'Kore' ? 'selected' : ''}>Suara Kore (Hangat)</option>
                            <option value="Aoede" ${appState.selectedVoice === 'Aoede' ? 'selected' : ''}>Suara Aoede (Lembut)</option>
                            <option value="Zephyr" ${appState.selectedVoice === 'Zephyr' ? 'selected' : ''}>Suara Zephyr (Semangat)</option>
                        </select>

                        <!-- Play Sound Button -->
                        <button id="btn-play-audio-narration" onclick="toggleNarrationVoice()" class="px-3.5 py-2 rounded-xl bg-rose-50 hover:bg-rose-100 text-rose-600 text-xs font-extrabold transition flex items-center gap-1.5">
                            <i class="fa-solid ${appState.isAudioPlaying ? 'fa-pause' : 'fa-volume-high'} text-rose-500"></i>
                            <span>${appState.isAudioPlaying ? 'Hentikan Suara' : 'Putar Suara Narasi'}</span>
                        </button>

                        <!-- Auto Play Story Mode Toggle -->
                        <button onclick="toggleAutoPlayDongeng()" class="px-3.5 py-2 rounded-xl text-xs font-bold transition flex items-center gap-1.5 ${appState.autoPlayMode ? 'bg-blue-600 text-white shadow-md' : 'bg-slate-100 text-slate-600 hover:bg-slate-200'}">
                            <i class="fa-solid fa-rotate-right ${appState.autoPlayMode ? 'animate-spin' : ''}"></i>
                            <span>Mode Dongeng Otomatis</span>
                        </button>
                    </div>
                </div>

                <!-- FLIPBOOK DISPLAY CONTAINER -->
                <div class="relative bg-gradient-to-b from-slate-800 to-slate-900 rounded-3xl p-4 sm:p-10 flex flex-col items-center justify-center min-h-[580px] shadow-2xl overflow-hidden">
                    <div class="w-full max-w-lg bg-white rounded-3xl shadow-2xl overflow-hidden border-4 border-white/20 transition-all duration-500 transform" style="background-color: ${currentPage ? currentPage.bgColor : '#FFFFFF'};">
                        ${isFrontCover ? `
                            <div class="p-8 text-center space-y-5">
                                <span class="px-3 py-1 rounded-full bg-blue-100 text-blue-900 text-xs font-extrabold uppercase tracking-wider">${book.school}</span>
                                <h1 class="text-3xl font-extrabold font-heading text-slate-900 leading-tight">${book.title}</h1>
                                <p class="text-xs font-bold text-slate-500 uppercase tracking-widest">Penulis: ${book.author}</p>
                                <div class="p-4 bg-blue-50 rounded-2xl border border-blue-200">
                                    <span class="text-xs font-bold text-blue-900 block mb-1">Pesan Moral Utama:</span>
                                    <p class="text-xs font-semibold text-blue-950">${book.moralMessage}</p>
                                </div>
                                <div class="pt-2 text-xs font-bold text-slate-400">
                                    <span>${book.genre} • Usia ${book.targetAge}</span>
                                </div>
                            </div>
                        ` : isBackCover ? `
                            <div class="p-8 text-center space-y-6 flex flex-col justify-between min-h-[500px]">
                                <div>
                                    <h2 class="text-xl font-bold font-heading text-slate-800 mb-2">${book.title}</h2>
                                    <p class="text-xs text-slate-500 leading-relaxed italic bg-slate-50 p-4 rounded-2xl border border-slate-100">
                                        "${book.synopsis}"
                                    </p>
                                </div>
                                <div class="p-4 bg-amber-50 rounded-2xl border border-amber-200">
                                    <span class="text-xs font-bold text-amber-800 block mb-1">Pesan Moral Utama:</span>
                                    <p class="text-xs font-semibold text-amber-900">${book.moralMessage}</p>
                                </div>
                                <div class="flex items-center justify-center gap-4 border-t border-slate-100 pt-4">
                                    <div class="w-14 h-14 bg-slate-100 p-1.5 rounded-xl border border-slate-200">
                                        <svg viewBox="0 0 100 100" class="w-full h-full text-slate-800" fill="currentColor">
                                            <rect width="100" height="100" fill="#FFFFFF"/>
                                            <path d="M10 10h30v30H10zM15 15v20h20V15zM50 10h40v30H50zM55 15v20h30V15zM10 50h30v40H10zM15 55v30h20V55zM60 60h10v10H60zM80 60h10v10H80zM70 70h10v20H70z" />
                                        </svg>
                                    </div>
                                    <div class="text-left">
                                        <span class="text-[10px] font-bold text-slate-400 block uppercase">Versi Digital & Voice</span>
                                        <span class="text-xs font-extrabold text-slate-700">${book.school}</span>
                                    </div>
                                </div>
                            </div>
                        ` : `
                            <div class="p-6 sm:p-8 space-y-5">
                                <div class="aspect-4/3 rounded-2xl overflow-hidden shadow-md border border-slate-200/60">
                                    <img src="${currentPage.imageUrl}" class="w-full h-full object-cover">
                                </div>
                                <div class="story-text text-base sm:text-lg font-bold leading-relaxed px-2 text-center" style="color: ${currentPage.textColor || '#1E293B'}">
                                    ${currentPage.text}
                                </div>
                                <div class="text-center pt-2">
                                    <span class="text-[11px] font-extrabold text-slate-400 px-3 py-1 rounded-full bg-slate-100">
                                        - Halaman ${currentPage.pageNumber} dari ${book.pages.length} -
                                    </span>
                                </div>
                            </div>
                        `}
                    </div>

                    <!-- Slide Controls -->
                    <div class="flex items-center justify-between w-full max-w-lg mt-6">
                        <button onclick="flipPrevSlide()" ${flipCurrentIdx === 0 ? 'disabled class="opacity-30 cursor-not-allowed text-white/50"' : 'class="px-4 py-2 rounded-2xl bg-white/10 hover:bg-white/20 text-white font-bold text-xs backdrop-blur-md transition flex items-center gap-2"'}>
                            <i class="fa-solid fa-chevron-left"></i> SEBELUMNYA
                        </button>
                        <span class="text-xs font-bold text-white/70">
                            ${flipCurrentIdx === 0 ? 'Sampul Depan' : flipCurrentIdx === totalSlides - 1 ? 'Sampul Belakang' : `Halaman ${flipCurrentIdx}`}
                        </span>
                        <button onclick="flipNextSlide()" ${flipCurrentIdx === totalSlides - 1 ? 'disabled class="opacity-30 cursor-not-allowed text-white/50"' : 'class="px-4 py-2 rounded-2xl bg-white/10 hover:bg-white/20 text-white font-bold text-xs backdrop-blur-md transition flex items-center gap-2"'}>
                            NEXT <i class="fa-solid fa-chevron-right"></i>
                        </button>
                    </div>
                </div>
            </div>`;
        }

        function flipPrevSlide() {
            stopAudioNarration();
            if (flipCurrentIdx > 0) {
                flipCurrentIdx--;
                renderApp();
            }
        }
        function flipNextSlide() {
            stopAudioNarration();
            const book = appState.books.find(b => b.id === appState.currentBookId);
            if (book && flipCurrentIdx < book.pages.length + 1) {
                flipCurrentIdx++;
                renderApp();
            }
        }

        // -----------------------------------------------------------------
        // 7. KUIS & GLOSARIUM
        // -----------------------------------------------------------------
        let userQuizAnswers = {};

        function renderQuizView() {
            const book = appState.books.find(b => b.id === appState.currentBookId);
            if (!book) return `<div class="p-8 text-center">Buku tidak ditemukan.</div>`;

            const quizList = book.quiz || [];
            const glossaryList = book.glossary || [];

            return `
            <div class="max-w-3xl mx-auto space-y-6">
                <div class="flex items-center gap-4 bg-white p-4 rounded-2xl border border-slate-200 shadow-sm">
                    <button onclick="navigateTo('studio', '${book.id}')" class="w-9 h-9 rounded-xl border border-slate-200 flex items-center justify-center text-slate-600 hover:bg-slate-50 transition">
                        <i class="fa-solid fa-arrow-left"></i>
                    </button>
                    <div>
                        <span class="text-[10px] font-bold text-indigo-600 uppercase tracking-wider">Aktivitas Pemahaman Bacaan</span>
                        <h3 class="text-base font-bold text-slate-800 font-heading">${book.title}</h3>
                    </div>
                </div>

                <div class="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm space-y-6">
                    <div class="flex items-center gap-2 border-b border-slate-100 pb-3">
                        <div class="w-8 h-8 rounded-xl bg-indigo-100 text-indigo-600 flex items-center justify-center">
                            <i class="fa-solid fa-graduation-cap"></i>
                        </div>
                        <h4 class="text-lg font-bold font-heading text-slate-800">Kuis Pemahaman Anak</h4>
                    </div>

                    ${quizList.length === 0 ? `
                        <p class="text-xs text-slate-400 italic">Belum ada pertanyaan kuis.</p>
                    ` : `
                        <div class="space-y-6">
                            ${quizList.map((q, qIdx) => `
                                <div class="p-4 rounded-2xl bg-slate-50 border border-slate-100 space-y-3">
                                    <p class="text-sm font-bold text-slate-800">${qIdx + 1}. ${q.question}</p>
                                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                                        ${q.options.map((opt, optIdx) => `
                                            <button onclick="selectQuizAnswer(${qIdx}, ${optIdx})" class="p-3 rounded-xl border text-left text-xs font-semibold transition ${userQuizAnswers[qIdx] === optIdx ? 'bg-indigo-600 text-white border-indigo-600 shadow-sm' : 'bg-white text-slate-700 border-slate-200 hover:border-slate-300'}">
                                                ${String.fromCharCode(65 + optIdx)}. ${opt}
                                            </button>
                                        `).join('')}
                                    </div>
                                    ${userQuizAnswers[qIdx] !== undefined ? `
                                        <div class="mt-2 text-xs font-bold ${userQuizAnswers[qIdx] === q.answerIndex ? 'text-emerald-600' : 'text-rose-500'}">
                                            ${userQuizAnswers[qIdx] === q.answerIndex ? '✓ Jawaban Benar!' : `✗ Jawaban kurang tepat. Jawaban benar: ${String.fromCharCode(65 + q.answerIndex)}. ${q.options[q.answerIndex]}`}
                                        </div>
                                    ` : ''}
                                </div>
                            `).join('')}
                        </div>
                    `}
                </div>

                <div class="bg-white p-6 rounded-3xl border border-slate-200 shadow-sm space-y-4">
                    <div class="flex items-center gap-2 border-b border-slate-100 pb-3">
                        <div class="w-8 h-8 rounded-xl bg-sky-100 text-sky-600 flex items-center justify-center">
                            <i class="fa-solid fa-book-atlas"></i>
                        </div>
                        <h4 class="text-lg font-bold font-heading text-slate-800">Glosarium Kata</h4>
                    </div>

                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                        ${glossaryList.map(item => `
                            <div class="p-3.5 rounded-2xl bg-sky-50/50 border border-sky-200/60">
                                <span class="text-xs font-bold text-sky-900 block mb-0.5">${item.word}</span>
                                <span class="text-xs font-medium text-slate-600">${item.meaning}</span>
                            </div>
                        `).join('')}
                    </div>
                </div>

                <div class="bg-blue-50 p-6 rounded-3xl border border-blue-200 text-center space-y-2">
                    <span class="text-xs font-extrabold text-blue-950 uppercase tracking-wider block">Pesan Moral Cerita</span>
                    <p class="text-sm font-semibold text-blue-900">${book.moralMessage}</p>
                </div>
            </div>`;
        }

        function selectQuizAnswer(qIdx, optIdx) {
            userQuizAnswers[qIdx] = optIdx;
            renderApp();
        }

        // -----------------------------------------------------------------
        // 8. PDF EXPORT ENGINE
        // -----------------------------------------------------------------
        async function exportBookPdf(bookId) {
            const book = appState.books.find(b => b.id === bookId);
            if (!book) return;

            showProgressModal("Menyiapkan dokumen PDF berkualitas tinggi...", 10);

            const printArea = document.getElementById('print-area');
            printArea.innerHTML = '';
            printArea.classList.remove('hidden');

            let htmlPages = `
            <div class="print-page w-[800px] h-[1130px] p-12 bg-white flex flex-col justify-between border border-slate-200 page-break">
                <div class="text-center space-y-4">
                    <span class="text-sm font-extrabold text-indigo-600 uppercase tracking-widest">${book.school}</span>
                    <h1 class="text-4xl font-extrabold text-slate-900 leading-tight">${book.title}</h1>
                    <p class="text-base font-bold text-slate-500">Karya Penulis: ${book.author}</p>
                </div>
                <div class="w-[500px] h-[500px] mx-auto rounded-3xl overflow-hidden border-4 border-slate-100 shadow-md">
                    <img src="${book.pages[0]?.imageUrl}" class="w-full h-full object-cover">
                </div>
                <div class="text-center border-t border-slate-200 pt-6 text-sm text-slate-500 font-semibold">
                    <span>Target: ${book.targetAge} • Jenis: ${book.genre}</span>
                </div>
            </div>`;

            book.pages.forEach(p => {
                htmlPages += `
                <div class="print-page w-[800px] h-[1130px] p-12 bg-white flex flex-col justify-between border border-slate-200 page-break" style="background-color: ${p.bgColor || '#FFFFFF'}">
                    <div class="w-full h-[600px] rounded-3xl overflow-hidden border border-slate-200">
                        <img src="${p.imageUrl}" class="w-full h-full object-cover">
                    </div>
                    <div class="text-xl font-bold text-center leading-relaxed px-6 my-auto" style="color: ${p.textColor || '#1E293B'}">
                        ${p.text}
                    </div>
                    <div class="text-center text-xs font-bold text-slate-400 border-t border-slate-200/50 pt-4">
                        Halaman ${p.pageNumber}
                    </div>
                </div>`;
            });

            htmlPages += `
            <div class="print-page w-[800px] h-[1130px] p-12 bg-white flex flex-col justify-between border border-slate-200 page-break">
                <div class="text-center space-y-4">
                    <h2 class="text-3xl font-bold text-slate-800">${book.title}</h2>
                    <p class="text-sm text-slate-600 italic bg-slate-50 p-6 rounded-2xl border border-slate-200">
                        "${book.synopsis}"
                    </p>
                </div>
                <div class="p-6 bg-amber-50 rounded-2xl border border-amber-200 text-center">
                    <span class="text-sm font-bold text-amber-900 block mb-2">Pesan Moral Cerita</span>
                    <p class="text-sm font-semibold text-amber-800">${book.moralMessage}</p>
                </div>
                <div class="text-center text-xs text-slate-400 border-t border-slate-200 pt-6">
                    <p>Diterbitkan oleh ${book.school} • ${book.author}</p>
                </div>
            </div>`;

            printArea.innerHTML = htmlPages;

            try {
                updateProgress("Mengonversi halaman menjadi PDF...", 50);

                const { jsPDF } = window.jspdf;
                const pdf = new jsPDF({
                    orientation: 'portrait',
                    unit: 'px',
                    format: [800, 1130]
                });

                const pageElements = printArea.querySelectorAll('.print-page');

                for (let i = 0; i < pageElements.length; i++) {
                    updateProgress(`Memproses halaman PDF ${i + 1} dari ${pageElements.length}...`, 50 + Math.floor(((i + 1) / pageElements.length) * 45));
                    
                    const canvas = await html2canvas(pageElements[i], {
                        scale: 2,
                        useCORS: true,
                        logging: false
                    });

                    const imgData = canvas.toDataURL('image/jpeg', 0.95);
                    if (i > 0) pdf.addPage([800, 1130], 'portrait');
                    pdf.addImage(imgData, 'JPEG', 0, 0, 800, 1130);
                }

                updateProgress("Menyelesaikan PDF...", 98);
                pdf.save(`${book.title.replace(/[^a-z0-9]/gi, '_').toLowerCase()}_buku_anak.pdf`);

                setTimeout(() => {
                    hideProgressModal();
                    printArea.classList.add('hidden');
                    printArea.innerHTML = '';
                }, 500);

            } catch (err) {
                console.error("PDF Export Error:", err);
                hideProgressModal();
                alert("Gagal mengunduh PDF secara otomatis. Membuka dialog cetak.");
                window.print();
            }
        }

        // -----------------------------------------------------------------
        // API KEY MODAL HELPERS
        // -----------------------------------------------------------------
        function openApiKeyModal() {
            document.getElementById('input-api-key').value = appState.customApiKey;
            document.getElementById('modal-apikey').classList.remove('hidden');
        }
        function closeApiKeyModal() {
            document.getElementById('modal-apikey').classList.add('hidden');
        }
        function saveApiKey() {
            const key = document.getElementById('input-api-key').value.trim();
            appState.customApiKey = key;
            localStorage.setItem(API_KEY_STORAGE, key);
            closeApiKeyModal();
            alert("Kunci API berhasil disimpan!");
        }
    </script>
</body>
</html>
