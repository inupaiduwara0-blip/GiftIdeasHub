<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GiftIdeas Hub | Admin Dashboard</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:wght@600;700&display=swap" rel="stylesheet">
    
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <!-- Tailwind Configuration -->
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        serif: ['Playfair Display', 'serif'],
                    },
                    colors: {
                        primary: '#ec4899',
                        secondary: '#8b5cf6',
                        darkBg: '#0f172a',
                        cardDark: '#1e293b',
                        adminBg: '#f3f4f6'
                    }
                }
            }
        }
    </script>

    <style>
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        .dark ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
        
        .glass-nav { background: rgba(255, 255, 255, 0.85); backdrop-filter: blur(12px); }
        .dark .glass-nav { background: rgba(15, 23, 42, 0.85); }

        .card-hover { transition: all 0.3s ease; }
        .card-hover:hover { transform: translateY(-5px); box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1); }
        
        .upload-area { border: 2px dashed #cbd5e1; transition: all 0.3s; }
        .upload-area:hover { border-color: #ec4899; background-color: rgba(236, 72, 153, 0.05); }
        .dark .upload-area { border-color: #475569; }
        .dark .upload-area:hover { background-color: rgba(236, 72, 153, 0.1); }
        
        .fade-in { animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="bg-gray-50 text-slate-800 dark:bg-darkBg dark:text-gray-100 transition-colors duration-300 min-h-screen flex flex-col">

    <!-- Navigation -->
    <nav class="fixed w-full z-50 glass-nav border-b border-gray-200 dark:border-gray-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16 items-center">
                <div class="flex-shrink-0 flex items-center cursor-pointer" onclick="app.goHome()">
                    <i class="fa-solid fa-gift text-primary text-2xl mr-2"></i>
                    <span class="font-serif font-bold text-xl tracking-tight">GiftIdeas<span class="text-primary">Hub</span></span>
                </div>

                <div class="hidden md:flex space-x-6 items-center">
                    <button onclick="app.goHome()" class="hover:text-primary transition font-medium">Home</button>
                    <button onclick="app.router('blog')" class="hover:text-primary transition font-medium">Blog</button>
                    
                    <div class="relative">
                        <input type="text" id="searchInput" placeholder="Search gifts..." class="bg-gray-100 dark:bg-gray-800 rounded-full py-1.5 px-4 pl-10 text-sm focus:outline-none focus:ring-2 focus:ring-primary w-52 transition-all">
                        <i class="fa-solid fa-search absolute left-3.5 top-2.5 text-gray-400 text-xs"></i>
                    </div>

                    <button id="themeToggle" class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition">
                        <i class="fa-solid fa-moon dark:hidden"></i>
                        <i class="fa-solid fa-sun hidden dark:block text-yellow-400"></i>
                    </button>

                    <button id="navAuthBtn" onclick="app.handleAuthClick()" class="flex items-center gap-2 px-4 py-2 rounded-full font-medium transition">
                        <i class="fa-solid fa-user-lock text-primary"></i> Login
                    </button>
                </div>

                <div class="md:hidden flex items-center">
                    <button id="mobileMenuBtn" class="text-gray-600 dark:text-gray-300 hover:text-primary p-2">
                        <i class="fa-solid fa-bars text-xl"></i>
                    </button>
                </div>
            </div>
        </div>
        
        <div id="mobileMenu" class="hidden md:hidden bg-white dark:bg-cardDark border-t dark:border-gray-700 shadow-lg">
            <div class="px-2 pt-2 pb-3 space-y-1">
                <button onclick="app.goHome(); app.toggleMobileMenu()" class="block w-full text-left px-3 py-2 rounded-md hover:bg-gray-100 dark:hover:bg-gray-700">Home</button>
                <button onclick="app.router('blog'); app.toggleMobileMenu()" class="block w-full text-left px-3 py-2 rounded-md hover:bg-gray-100 dark:hover:bg-gray-700">Blog</button>
                <button id="mobileAuthBtn" onclick="app.handleAuthClick(); app.toggleMobileMenu()" class="block w-full text-left px-3 py-2 rounded-md hover:bg-gray-100 dark:hover:bg-gray-700 text-primary">Login</button>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <main id="app-container" class="flex-grow pt-16"></main>

    <!-- Footer -->
    <footer class="bg-white dark:bg-cardDark border-t border-gray-200 dark:border-gray-800 mt-12">
        <div class="max-w-7xl mx-auto py-8 px-4 sm:px-6 lg:px-8">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                <div>
                    <span class="font-serif font-bold text-lg">GiftIdeas<span class="text-primary">Hub</span></span>
                    <p class="mt-2 text-sm text-gray-500 dark:text-gray-400">Curated gifts for every occasion. Fast, modern, and affiliate-ready.</p>
                </div>
                <div>
                    <h3 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">Quick Links</h3>
                    <ul class="mt-4 space-y-2">
                        <li><button onclick="app.filterCategory('Birthday')" class="text-base text-gray-500 hover:text-primary">Birthday</button></li>
                        <li><button onclick="app.filterCategory('Anniversary')" class="text-base text-gray-500 hover:text-primary">Anniversary</button></li>
                    </ul>
                </div>
                <div>
                    <h3 class="text-sm font-semibold text-gray-400 uppercase tracking-wider">Support</h3>
                    <div class="mt-4 flex space-x-6">
                        <a href="#" class="text-gray-400 hover:text-blue-600"><i class="fa-brands fa-facebook text-xl"></i></a>
                        <a href="#" class="text-gray-400 hover:text-green-600"><i class="fa-brands fa-whatsapp text-xl"></i></a>
                        <a href="#" class="text-gray-400 hover:text-red-600"><i class="fa-brands fa-pinterest text-xl"></i></a>
                    </div>
                </div>
            </div>
            <div class="mt-8 border-t border-gray-200 dark:border-gray-700 pt-4 text-center text-sm text-gray-400">
                &copy; 2026 GiftIdeas Hub. All rights reserved.
            </div>
        </div>
    </footer>

    <script>
        // --- Configuration & Constants ---
        const ADMIN_CREDENTIALS = {
            email: "inupaiduwara0@gmail.com",
            password: "Inupa85+Idauduwara*"
        };

        const STORES = ['Amazon', 'Daraz', 'AliExpress', 'eBay', 'Other'];
        const CATEGORIES = ['Birthday', 'Anniversary', 'Friendship', 'Last Minute', 'Corporate'];

        const defaultProducts = [
            { id: 1, title: "Personalized Gold Necklace", category: "Anniversary", price: "$45 - $80", image: "https://placehold.co/600x400/e2e8f0/1e293b?text=Gold+Necklace", description: "Beautiful 18k gold plated necklace with custom engraving. Perfect for anniversaries.", store: "Amazon", link: "https://amazon.com", trending: true },
            { id: 2, title: "Smart Watch Series 5", category: "Birthday", price: "$199", image: "https://placehold.co/600x400/e2e8f0/1e293b?text=Smart+Watch", description: "Track fitness, sleep, and notifications with this sleek smartwatch.", store: "Daraz", link: "https://daraz.lk", trending: true },
            { id: 3, title: "Custom Photo Frame", category: "Friendship", price: "$25", image: "https://placehold.co/600x400/e2e8f0/1e293b?text=Photo+Frame", description: "High-quality wooden frame with your favorite memory printed inside.", store: "AliExpress", link: "https://aliexpress.com", trending: false }
        ];

        class App {
            constructor() {
                this.products = JSON.parse(localStorage.getItem('gift_products')) || defaultProducts;
                this.isAdmin = sessionStorage.getItem('isAdmin') === 'true';
                this.userEmail = sessionStorage.getItem('userEmail') || '';
                this.editingId = null;
                this.currentView = 'home';

                this.initTheme();
                this.setupListeners();
                this.updateAuthUI();
                this.router('home');
            }

            initTheme() {
                if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
                    document.documentElement.classList.add('dark');
                } else {
                    document.documentElement.classList.remove('dark');
                }
            }

            setupListeners() {
                document.getElementById('themeToggle').addEventListener('click', () => this.toggleTheme());
                document.getElementById('searchInput').addEventListener('keyup', (e) => e.key === 'Enter' && this.handleSearch(e.target.value));
                document.getElementById('mobileMenuBtn').addEventListener('click', () => this.toggleMobileMenu());
            }

            toggleTheme() {
                document.documentElement.classList.toggle('dark');
                localStorage.theme = document.documentElement.classList.contains('dark') ? 'dark' : 'light';
            }

            toggleMobileMenu() {
                document.getElementById('mobileMenu').classList.toggle('hidden');
            }

            updateAuthUI() {
                const desktopBtn = document.getElementById('navAuthBtn');
                const mobileBtn = document.getElementById('mobileAuthBtn');
                if (!desktopBtn) return;

                if (this.isAdmin) {
                    const html = '<i class="fa-solid fa-user-shield text-green-500"></i> Admin';
                    const cls = "flex items-center gap-2 px-4 py-2 rounded-full bg-green-50 dark:bg-green-900/20 text-green-700 dark:text-green-400 border border-green-200 dark:border-green-800 font-medium";
                    desktopBtn.innerHTML = html; desktopBtn.className = cls; desktopBtn.onclick = () => this.router('admin');
                    mobileBtn.innerHTML = '<i class="fa-solid fa-user-shield text-green-500 mr-2"></i> Admin Dashboard'; mobileBtn.className = "block w-full text-left px-3 py-2 rounded-md hover:bg-green-50 dark:hover:bg-green-900 text-green-600 font-medium";
                } else if (this.userEmail) {
                    const name = this.userEmail.split('@')[0];
                    const html = `<i class="fa-solid fa-user-circle text-blue-500"></i> ${name}`;
                    const cls = "flex items-center gap-2 px-4 py-2 rounded-full bg-blue-50 dark:bg-blue-900/20 text-blue-700 dark:text-blue-400 border border-blue-200 dark:border-blue-800 font-medium";
                    desktopBtn.innerHTML = html; desktopBtn.className = cls; desktopBtn.onclick = () => this.logout();
                    mobileBtn.innerHTML = `<i class="fa-solid fa-user-circle text-blue-500 mr-2"></i> Logout (${name})`; mobileBtn.className = "block w-full text-left px-3 py-2 rounded-md hover:bg-blue-50 dark:hover:bg-blue-900 text-blue-600 font-medium";
                } else {
                    const html = '<i class="fa-solid fa-user-lock text-primary"></i> Login';
                    const cls = "flex items-center gap-2 px-4 py-2 rounded-full bg-gray-100 dark:bg-gray-800 text-gray-700 dark:text-gray-300 border border-gray-200 dark:border-gray-700 font-medium hover:bg-gray-200 dark:hover:bg-gray-700 transition";
                    desktopBtn.innerHTML = html; desktopBtn.className = cls; desktopBtn.onclick = () => this.router('login');
                    mobileBtn.innerHTML = '<i class="fa-solid fa-user-lock text-primary mr-2"></i> Login'; mobileBtn.className = "block w-full text-left px-3 py-2 rounded-md hover:bg-gray-100 dark:hover:bg-gray-700 text-primary font-medium";
                }
            }

            handleAuthClick() {
                if (this.isAdmin) this.router('admin');
                else if (this.userEmail) this.logout();
                else this.router('login');
            }

            goHome() { this.router('home'); }

            // --- Routing ---
            router(view, data = null) {
                this.currentView = view;
                const container = document.getElementById('app-container');
                window.scrollTo({ top: 0, behavior: 'smooth' });

                if (view === 'admin' && !this.isAdmin) view = 'login';
                if (view === 'login' && this.isAdmin) view = 'admin';

                switch(view) {
                    case 'home': container.innerHTML = this.renderHome(); break;
                    case 'product': container.innerHTML = this.renderProductDetail(data); break;
                    case 'admin': container.innerHTML = this.renderAdminDashboard(); break;
                    case 'login': container.innerHTML = this.renderLogin(); break;
                    case 'blog': container.innerHTML = this.renderBlog(); break;
                    case 'search': container.innerHTML = this.renderSearch(data); break;
                    default: container.innerHTML = this.renderHome();
                }
            }

            // --- Renderers ---
            renderHome() {
                const trending = this.products.filter(p => p.trending).slice(0, 4);
                return `
                    <section class="bg-gradient-to-r from-pink-500 to-violet-600 text-white py-20 px-4">
                        <div class="max-w-7xl mx-auto text-center fade-in">
                            <h1 class="text-4xl md:text-6xl font-serif font-bold mb-4">Find the Perfect Gift</h1>
                            <p class="text-lg md:text-xl mb-8 opacity-90">Curated ideas for every special moment in your life.</p>
                            <div class="flex flex-wrap justify-center gap-4">
                                <button onclick="app.filterCategory('Birthday')" class="bg-white text-pink-600 px-6 py-3 rounded-full font-bold hover:bg-gray-100 shadow-lg transition">Shop Birthday</button>
                                <button onclick="app.filterCategory('Trending')" class="border-2 border-white px-6 py-3 rounded-full font-bold hover:bg-white/10 transition">See Trending</button>
                            </div>
                        </div>
                    </section>
                    <section class="py-12 max-w-7xl mx-auto px-4">
                        <h2 class="text-2xl font-bold mb-6">Categories</h2>
                        <div class="grid grid-cols-2 md:grid-cols-5 gap-4">
                            ${CATEGORIES.map(c => `
                                <button onclick="app.filterCategory('${c}')" class="bg-white dark:bg-cardDark p-4 rounded-xl shadow-sm hover:shadow-md transition border border-gray-100 dark:border-gray-700 text-center">
                                    <i class="fa-solid fa-gift text-primary text-xl mb-2"></i>
                                    <div class="font-medium text-sm">${c}</div>
                                </button>
                            `).join('')}
                        </div>
                    </section>
                    <section class="py-12 bg-gray-100 dark:bg-gray-900 px-4">
                        <div class="max-w-7xl mx-auto">
                            <div class="flex justify-between items-end mb-6">
                                <h2 class="text-3xl font-serif font-bold">Trending Now</h2>
                                <button onclick="app.filterCategory('Trending')" class="text-primary hover:underline font-medium">View All</button>
                            </div>
                            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 fade-in">
                                ${trending.map(p => this.createCard(p)).join('')}
                            </div>
                        </div>
                    </section>
                `;
            }

            createCard(product) {
                return `
                    <div class="bg-white dark:bg-cardDark rounded-xl shadow-sm overflow-hidden card-hover border border-gray-100 dark:border-gray-700 flex flex-col h-full">
                        <div class="relative h-48 overflow-hidden cursor-pointer" onclick="app.router('product', ${product.id})">
                            <img src="${product.image}" alt="${product.title}" class="w-full h-full object-cover" onerror="this.src='https://placehold.co/600x400/e2e8f0/1e293b?text=Gift+Image'">
                            ${product.trending ? '<span class="absolute top-2 right-2 bg-yellow-400 text-yellow-900 text-xs font-bold px-2 py-1 rounded-full">HOT</span>' : ''}
                        </div>
                        <div class="p-4 flex flex-col flex-grow">
                            <div class="text-xs text-primary font-semibold mb-1 uppercase tracking-wide">${product.store}</div>
                            <h3 class="font-bold text-lg mb-2 cursor-pointer hover:text-primary transition" onclick="app.router('product', ${product.id})">${product.title}</h3>
                            <p class="text-gray-500 dark:text-gray-400 text-sm line-clamp-2 mb-4 flex-grow">${product.description}</p>
                            <div class="flex items-center justify-between mt-auto pt-3 border-t border-gray-100 dark:border-gray-700">
                                <span class="font-bold text-lg">${product.price}</span>
                                <button onclick="window.open('${product.link}', '_blank')" class="bg-gray-900 dark:bg-white dark:text-gray-900 text-white px-4 py-2 rounded-lg text-sm font-medium hover:opacity-90 transition">Buy Now</button>
                            </div>
                        </div>
                    </div>
                `;
            }

            renderProductDetail(id) {
                const p = this.products.find(x => x.id === id);
                if (!p) return `<div class="p-12 text-center text-gray-500">Product not found</div>`;
                
                let clicks = parseInt(localStorage.getItem(`clicks_${id}`) || '0');
                localStorage.setItem(`clicks_${id}`, clicks + 1);

                let icon = p.store === 'Amazon' ? 'fa-amazon' : p.store === 'Daraz' ? 'fa-box-open' : 'fa-store';

                return `
                    <div class="max-w-7xl mx-auto px-4 py-10 fade-in">
                        <button onclick="window.history.back()" class="mb-6 text-gray-500 hover:text-primary flex items-center"><i class="fa-solid fa-arrow-left mr-2"></i> Back</button>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-10 bg-white dark:bg-cardDark p-4 md:p-8 rounded-2xl shadow-lg">
                            <div class="rounded-xl overflow-hidden bg-gray-100 dark:bg-gray-800 aspect-square flex items-center justify-center">
                                <img src="${p.image}" class="w-full h-full object-cover" onerror="this.src='https://placehold.co/600x600/e2e8f0/1e293b?text=Gift+Image'">
                            </div>
                            <div class="flex flex-col justify-center">
                                <div class="flex items-center gap-3 mb-4">
                                    <span class="bg-primary/10 text-primary px-3 py-1 rounded-full text-sm font-semibold">${p.category}</span>
                                    <span class="text-gray-400 text-sm">• Sold on ${p.store}</span>
                                </div>
                                <h1 class="text-3xl md:text-4xl font-serif font-bold mb-3">${p.title}</h1>
                                <p class="text-2xl font-bold text-gray-900 dark:text-white mb-6">${p.price}</p>
                                <p class="text-gray-600 dark:text-gray-300 mb-8 text-lg leading-relaxed">${p.description}</p>
                                <div class="flex flex-col sm:flex-row gap-4">
                                    <a href="${p.link}" target="_blank" class="flex-1 bg-primary hover:bg-pink-600 text-white text-center py-4 rounded-xl font-bold text-lg shadow-lg shadow-pink-500/30 transition transform hover:-translate-y-1">
                                        <i class="fa-brands ${icon} mr-2"></i> Buy on ${p.store}
                                    </a>
                                    <button onclick="navigator.clipboard.writeText(window.location.href); alert('Link copied!')" class="px-6 py-4 border border-gray-300 dark:border-gray-600 rounded-xl hover:bg-gray-50 dark:hover:bg-gray-800 transition">
                                        <i class="fa-solid fa-share-nodes"></i> Share
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                `;
            }

            renderSearch(query) {
                const q = query.toLowerCase();
                const results = this.products.filter(p => p.title.toLowerCase().includes(q) || p.description.toLowerCase().includes(q) || p.category.toLowerCase().includes(q));
                return `
                    <div class="max-w-7xl mx-auto px-4 py-12 fade-in">
                        <h2 class="text-2xl font-bold mb-6">Results for "<span class="text-primary">${query}</span>"</h2>
                        ${results.length === 0 ? `<div class="text-center py-12 text-gray-500">No gifts found. Try another keyword.</div>` : 
                        `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">${results.map(p => this.createCard(p)).join('')}</div>`}
                    </div>
                `;
            }

            renderBlog() {
                return `<div class="max-w-4xl mx-auto px-4 py-12 text-center fade-in">
                    <h1 class="text-3xl font-serif font-bold mb-4">Gift Guides & Articles</h1>
                    <p class="text-gray-500">Coming soon: Expert tips and curated lists for every occasion.</p>
                </div>`;
            }

            renderLogin() {
                return `
                    <div class="min-h-[80vh] flex items-center justify-center px-4 fade-in">
                        <div class="bg-white dark:bg-cardDark p-8 rounded-2xl shadow-xl w-full max-w-md border border-gray-100 dark:border-gray-700">
                            <div class="text-center mb-6">
                                <div class="w-16 h-16 bg-primary/10 text-primary rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">
                                    <i class="fa-solid fa-lock"></i>
                                </div>
                                <h2 class="text-2xl font-bold">Welcome Back</h2>
                                <p class="text-gray-500 text-sm mt-1">Sign in to manage your gifts</p>
                            </div>
                            <form onsubmit="app.handleLogin(event)" class="space-y-4">
                                <div>
                                    <label class="block text-sm font-medium mb-1">Email</label>
                                    <input type="email" id="loginEmail" class="w-full px-4 py-2 rounded-lg border border-gray-300 dark:border-gray-600 dark:bg-gray-800 focus:ring-2 focus:ring-primary outline-none" required>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium mb-1">Password</label>
                                    <input type="password" id="loginPass" class="w-full px-4 py-2 rounded-lg border border-gray-300 dark:border-gray-600 dark:bg-gray-800 focus:ring-2 focus:ring-primary outline-none" required>
                                </div>
                                <button type="submit" class="w-full bg-primary text-white py-2.5 rounded-lg font-bold hover:bg-pink-600 transition shadow-md">Sign In</button>
                                <p id="loginError" class="text-red-500 text-sm text-center hidden bg-red-50 dark:bg-red-900/20 p-2 rounded"></p>
                            </form>
                        </div>
                    </div>
                `;
            }

            // --- ADMIN DASHBOARD ---
            renderAdminDashboard() {
                const totalClicks = this.products.reduce((acc, p) => acc + (parseInt(localStorage.getItem(`clicks_${p.id}`) || '0')), 0);
                const isEditing = !!this.editingId;
                const currentProduct = isEditing ? this.products.find(p => p.id === this.editingId) : null;

                return `
                    <div class="max-w-7xl mx-auto px-4 py-8 fade-in">
                        <div class="flex flex-col md:flex-row justify-between items-center mb-8 gap-4">
                            <h1 class="text-3xl font-bold flex items-center gap-2"><i class="fa-solid fa-chart-line text-primary"></i> Admin Dashboard</h1>
                            <div class="flex gap-2">
                                <button onclick="app.goHome()" class="px-4 py-2 bg-gray-100 dark:bg-gray-800 rounded-lg text-sm hover:bg-gray-200 dark:hover:bg-gray-700 transition">View Site</button>
                                <button onclick="app.logout()" class="px-4 py-2 bg-red-50 dark:bg-red-900/20 text-red-600 rounded-lg text-sm hover:bg-red-100 transition">Logout</button>
                            </div>
                        </div>

                        <!-- Stats -->
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-8">
                            <div class="bg-white dark:bg-cardDark p-5 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm">
                                <div class="text-gray-500 text-sm">Total Products</div>
                                <div class="text-2xl font-bold mt-1">${this.products.length}</div>
                            </div>
                            <div class="bg-white dark:bg-cardDark p-5 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm">
                                <div class="text-gray-500 text-sm">Total Affiliate Clicks</div>
                                <div class="text-2xl font-bold mt-1">${totalClicks}</div>
                            </div>
                            <div class="bg-white dark:bg-cardDark p-5 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm">
                                <div class="text-gray-500 text-sm">Admin Status</div>
                                <div class="text-2xl font-bold mt-1 text-green-600"><i class="fa-solid fa-check-circle"></i> Verified</div>
                            </div>
                        </div>

                        <!-- Product Form -->
                        <div class="bg-white dark:bg-cardDark p-6 rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm mb-8">
                            <h3 class="text-xl font-bold mb-4 flex items-center gap-2">
                                <i class="fa-solid ${isEditing ? 'fa-pen-to-square text-blue-500' : 'fa-plus-circle text-green-500'}"></i>
                                ${isEditing ? 'Edit Product' : 'Add New Product'}
                            </h3>
                            <form onsubmit="app.saveProduct(event)" class="space-y-4">
                                <div>
                                    <label class="block text-sm font-medium mb-1">Product Image</label>
                                    <div class="upload-area p-4 rounded-lg text-center cursor-pointer relative">
                                        <input type="file" id="newImage" accept="image/*" class="absolute inset-0 w-full h-full opacity-0 cursor-pointer" onchange="app.handleImageUpload(this)">
                                        <div id="uploadPlaceholder"><i class="fa-solid fa-cloud-arrow-up text-2xl text-gray-400 mb-1"></i><p class="text-xs text-gray-500">Click or drag to upload</p></div>
                                        <img id="imagePreview" class="hidden max-h-32 mx-auto rounded shadow object-contain" />
                                    </div>
                                </div>
                                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                    <input type="text" id="fTitle" placeholder="Title" required class="p-2 border rounded dark:bg-gray-800 dark:border-gray-600 w-full">
                                    <select id="fCategory" class="p-2 border rounded dark:bg-gray-800 dark:border-gray-600 w-full">${CATEGORIES.map(c=>`<option>${c}</option>`).join('')}</select>
                                    <input type="text" id="fPrice" placeholder="Price ($50)" required class="p-2 border rounded dark:bg-gray-800 dark:border-gray-600 w-full">
                                    <select id="fStore" class="p-2 border rounded dark:bg-gray-800 dark:border-gray-600 w-full">${STORES.map(s=>`<option>${s}</option>`).join('')}</select>
                                    <input type="url" id="fLink" placeholder="Affiliate URL (https://...)" required class="p-2 border rounded dark:bg-gray-800 dark:border-gray-600 w-full md:col-span-2">
                                    <textarea id="fDesc" placeholder="Description" required class="p-2 border rounded dark:bg-gray-800 dark:border-gray-600 w-full md:col-span-2 h-20"></textarea>
                                    <div class="md:col-span-2 flex items-center gap-4">
                                        <label class="flex items-center gap-2 cursor-pointer"><input type="checkbox" id="fTrending" class="w-4 h-4 text-primary rounded"> Trending Item</label>
                                        <div class="flex-1 flex justify-end gap-2">
                                            ${isEditing ? `<button type="button" onclick="app.cancelEdit()" class="px-4 py-2 border rounded-lg text-sm">Cancel</button>` : ''}
                                            <button type="submit" class="px-6 py-2 ${isEditing ? 'bg-blue-600 hover:bg-blue-700' : 'bg-green-600 hover:bg-green-700'} text-white rounded-lg font-medium transition">${isEditing ? 'Update Product' : 'Save Product'}</button>
                                        </div>
                                    </div>
                                </div>
                            </form>
                        </div>

                        <!-- Product List -->
                        <div class="bg-white dark:bg-cardDark rounded-xl border border-gray-200 dark:border-gray-700 shadow-sm overflow-hidden">
                            <table class="w-full text-left">
                                <thead class="bg-gray-50 dark:bg-gray-800 border-b dark:border-gray-700 text-sm uppercase text-gray-500">
                                    <tr><th class="p-4">Image</th><th class="p-4">Details</th><th class="p-4">Store/Link</th><th class="p-4">Clicks</th><th class="p-4">Actions</th></tr>
                                </thead>
                                <tbody class="divide-y dark:divide-gray-700">
                                    ${this.products.map(p => `
                                        <tr class="hover:bg-gray-50 dark:hover:bg-gray-800/50 transition">
                                            <td class="p-4"><img src="${p.image}" class="w-10 h-10 rounded object-cover bg-gray-200" onerror="this.src='https://placehold.co/100x100/e2e8f0/94a3b8?text=?'"></td>
                                            <td class="p-4"><div class="font-medium">${p.title}</div><div class="text-xs text-gray-500">${p.category} • ${p.price}</div></td>
                                            <td class="p-4 text-xs"><span class="bg-blue-100 dark:bg-blue-900/30 text-blue-700 dark:text-blue-300 px-2 py-0.5 rounded">${p.store}</span></td>
                                            <td class="p-4 text-sm font-mono">${localStorage.getItem(`clicks_${p.id}`) || 0}</td>
                                            <td class="p-4 flex gap-2">
                                                <button onclick="app.editProduct(${p.id})" class="text-blue-600 hover:bg-blue-50 dark:hover:bg-blue-900/20 p-2 rounded"><i class="fa-solid fa-pen"></i></button>
                                                <button onclick="app.deleteProduct(${p.id})" class="text-red-600 hover:bg-red-50 dark:hover:bg-red-900/20 p-2 rounded"><i class="fa-solid fa-trash"></i></button>
                                            </td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                            </table>
                        </div>
                    </div>
                `;
            }

            // --- Admin Actions ---
            handleImageUpload(input) {
                if (input.files && input.files[0]) {
                    const reader = new FileReader();
                    reader.onload = (e) => {
                        const img = document.getElementById('imagePreview');
                        img.src = e.target.result; img.classList.remove('hidden');
                        document.getElementById('uploadPlaceholder').classList.add('hidden');
                    };
                    reader.readAsDataURL(input.files[0]);
                }
            }

            handleLogin(e) {
                e.preventDefault();
                const email = document.getElementById('loginEmail').value.trim();
                const pass = document.getElementById('loginPass').value;
                const err = document.getElementById('loginError');

                if (email === ADMIN_CREDENTIALS.email && pass === ADMIN_CREDENTIALS.password) {
                    this.isAdmin = true;
                    this.userEmail = email;
                    sessionStorage.setItem('isAdmin', 'true');
                    sessionStorage.setItem('userEmail', email);
                    this.updateAuthUI();
                    this.router('admin');
                } else if (email.includes('@') && pass.length >= 4) {
                    // Normal user login
                    this.isAdmin = false;
                    this.userEmail = email;
                    sessionStorage.setItem('isAdmin', 'false');
                    sessionStorage.setItem('userEmail', email);
                    this.updateAuthUI();
                    this.goHome();
                } else {
                    err.textContent = 'Invalid email or password. Please try again.';
                    err.classList.remove('hidden');
                }
            }

            logout() {
                this.isAdmin = false;
                this.userEmail = '';
                this.editingId = null;
                sessionStorage.clear();
                this.updateAuthUI();
                this.goHome();
            }

            saveProduct(e) {
                e.preventDefault();
                const imgEl = document.getElementById('imagePreview');
                const fallbackImg = `https://placehold.co/600x400/e2e8f0/1e293b?text=${encodeURIComponent(document.getElementById('fTitle').value)}`;
                const image = !imgEl.classList.contains('hidden') ? imgEl.src : fallbackImg;

                const data = {
                    title: document.getElementById('fTitle').value,
                    category: document.getElementById('fCategory').value,
                    price: document.getElementById('fPrice').value,
                    store: document.getElementById('fStore').value,
                    link: document.getElementById('fLink').value,
                    description: document.getElementById('fDesc').value,
                    trending: document.getElementById('fTrending').checked,
                    image: image
                };

                if (this.editingId) {
                    const idx = this.products.findIndex(p => p.id === this.editingId);
                    if (idx !== -1) {
                        this.products[idx] = { ...this.products[idx], ...data };
                    }
                    this.editingId = null;
                } else {
                    this.products.unshift({ id: Date.now(), ...data });
                }

                localStorage.setItem('gift_products', JSON.stringify(this.products));
                this.router('admin');
            }

            editProduct(id) {
                this.editingId = id;
                const p = this.products.find(x => x.id === id);
                this.router('admin'); // Re-render form area
                
                // Fill form after DOM update
                setTimeout(() => {
                    document.getElementById('fTitle').value = p.title;
                    document.getElementById('fCategory').value = p.category;
                    document.getElementById('fPrice').value = p.price;
                    document.getElementById('fStore').value = p.store;
                    document.getElementById('fLink').value = p.link;
                    document.getElementById('fDesc').value = p.description;
                    document.getElementById('fTrending').checked = p.trending;
                    
                    const img = document.getElementById('imagePreview');
                    img.src = p.image; img.classList.remove('hidden');
                    document.getElementById('uploadPlaceholder').classList.add('hidden');
                    window.scrollTo({ top: 0, behavior: 'smooth' });
                }, 50);
            }

            cancelEdit() {
                this.editingId = null;
                this.router('admin');
            }

            deleteProduct(id) {
                if (confirm('Are you sure you want to delete this gift?')) {
                    this.products = this.products.filter(p => p.id !== id);
                    localStorage.setItem('gift_products', JSON.stringify(this.products));
                    localStorage.removeItem(`clicks_${id}`);
                    this.router('admin');
                }
            }

            filterCategory(cat) {
                const filtered = cat === 'Trending' ? this.products.filter(p => p.trending) : this.products.filter(p => p.category === cat);
                const container = document.getElementById('app-container');
                container.innerHTML = `
                    <div class="max-w-7xl mx-auto px-4 py-12 fade-in">
                        <h1 class="text-3xl font-bold mb-8">${cat === 'Trending' ? 'Trending Gifts' : cat + ' Gift Ideas'}</h1>
                        ${filtered.length === 0 ? '<div class="text-center text-gray-500 py-10">No items found in this category.</div>' : 
                        `<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">${filtered.map(p => this.createCard(p)).join('')}</div>`}
                    </div>
                `;
            }

            handleSearch(q) { if(q.trim()) this.router('search', q.trim()); }
        }

        const app = new App();
    </script>
</body>
</html>

