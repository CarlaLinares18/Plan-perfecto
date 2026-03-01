# Pla-perfecte
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pla Perfecte</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root {
            --color-primary: #8d6e63;
            --color-secondary: #d7ccc8;
            --color-accent: #5d4037;
            --color-bg: #fdfaf6;
        }

        body {
            background-color: var(--color-bg);
            color: var(--color-accent);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            overflow-x: hidden;
        }

        .header-gradient {
            background: linear-gradient(to right, #a1887f, #8d6e63);
        }

        .card {
            background: white;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(141, 110, 99, 0.15);
        }

        .btn-primary {
            background-color: var(--color-primary);
            color: white;
            transition: all 0.3s;
        }

        .btn-primary:hover {
            background-color: var(--color-accent);
            transform: translateY(-2px);
        }

        .star-rating {
            color: #ffb300;
        }

        .collage-container {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            padding: 10px;
        }

        @media (min-width: 768px) {
            .collage-container {
                grid-template-columns: repeat(3, 1fr);
                gap: 20px;
                padding: 20px;
            }
        }

        .collage-item {
            position: relative;
            overflow: hidden;
            border-radius: 10px;
            aspect-ratio: 1 / 1;
            transition: transform 0.3s;
            border: 3px solid white;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }

        @media (min-width: 768px) {
            .collage-item {
                border-width: 5px;
            }
        }

        .collage-item:hover {
            transform: scale(1.05) rotate(2deg);
            z-index: 10;
        }

        .collage-item img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .flower-icon {
            color: #d7ccc8;
            position: absolute;
            opacity: 0.4;
            pointer-events: none;
            animation: float 6s ease-in-out infinite;
            z-index: 0;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(20deg); }
        }

        #historyModal {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.6);
            z-index: 100;
            align-items: center;
            justify-content: center;
        }

        select, input {
            cursor: pointer;
            font-size: 16px;
        }
    </style>
</head>
<body class="min-h-screen relative">

    <i class="fas fa-seedling flower-icon text-3xl md:text-4xl" style="top: 10%; left: 3%;"></i>
    <i class="fas fa-spa flower-icon text-2xl md:text-3xl" style="top: 35%; right: 4%; animation-delay: 2s;"></i>
    <i class="fas fa-leaf flower-icon text-xl md:text-2xl" style="bottom: 15%; left: 5%; animation-delay: 4s;"></i>

    <!-- CABECERA -->
    <header class="header-gradient text-white p-4 md:p-6 shadow-lg relative z-10">
        <div class="max-w-6xl mx-auto grid grid-cols-2 md:grid-cols-3 items-center gap-y-4">
            
            <!-- IZQUIERDA: Historial -->
            <div class="flex justify-start">
                <button onclick="toggleHistory()" class="flex items-center space-x-2 hover:opacity-80 transition">
                    <div class="bg-white p-1.5 md:p-2 rounded-full">
                        <i class="fas fa-book-open text-amber-800 text-lg md:text-xl"></i>
                    </div>
                    <span id="txt-history-btn" class="font-semibold text-sm md:text-base">Historial</span>
                </button>
            </div>

            <!-- CENTRO: Título -->
            <div class="text-center order-last md:order-none col-span-2 md:col-span-1 mt-2 md:mt-0">
                <h1 id="app-title" class="text-2xl md:text-4xl font-serif font-bold uppercase tracking-widest">Pla Perfecte</h1>
                <p id="txt-motto" class="italic text-[10px] md:text-sm mt-1 opacity-90">"Cada día es una nueva oportunidad para crear recuerdos inolvidables"</p>
            </div>

            <!-- DERECHA: Idioma -->
            <div class="flex justify-end items-center space-x-2">
                <i class="fas fa-globe text-sm md:text-base"></i>
                <select id="lang-selector" onchange="changeLanguage()" class="bg-transparent border border-white/40 rounded px-1 md:px-2 py-1 text-xs md:text-sm outline-none">
                    <option value="ca" class="text-black">CA</option>
                    <option value="es" class="text-black">ES</option>
                    <option value="en" class="text-black">EN</option>
                </select>
            </div>
        </div>
    </header>

    <main class="max-w-4xl mx-auto p-4 md:p-6 mt-4 md:mt-8 relative z-10">
        <div class="card p-5 md:p-8 space-y-6 md:space-y-8">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 md:gap-6">
                <div class="flex flex-col space-y-2">
                    <label class="font-bold flex items-center space-x-2 text-sm md:text-base">
                        <i class="far fa-calendar-alt text-amber-700"></i>
                        <span id="lbl-date">Data</span>
                    </label>
                    <input type="date" id="input-date" class="border border-secondary rounded-lg p-2.5 md:p-3 outline-none focus:ring-2 focus:ring-primary w-full text-sm">
                </div>

                <div class="flex flex-col space-y-2">
                    <label class="font-bold flex items-center space-x-2 text-sm md:text-base">
                        <i class="fas fa-thermometer-half text-amber-700"></i>
                        <span id="lbl-temp">Temperatura (°C)</span>
                    </label>
                    <input type="number" id="input-temp" placeholder="Ej: 22" class="border border-secondary rounded-lg p-2.5 md:p-3 outline-none focus:ring-2 focus:ring-primary w-full text-sm">
                </div>

                <div class="flex flex-col space-y-2">
                    <label class="font-bold flex items-center space-x-2 text-sm md:text-base">
                        <i class="fas fa-users text-amber-700"></i>
                        <span id="lbl-with">Amb qui vas?</span>
                    </label>
                    <select id="input-with" class="border border-secondary rounded-lg p-2.5 md:p-3 outline-none focus:ring-2 focus:ring-primary bg-white w-full text-sm">
                    </select>
                </div>
            </div>

            <div class="text-center pt-2">
                <button onclick="generatePlan()" class="btn-primary w-full md:w-auto px-8 md:px-12 py-3.5 md:py-4 rounded-full text-lg md:text-xl font-bold uppercase tracking-wider shadow-md active:scale-95 transition">
                    <i class="fas fa-magic mr-2"></i> <span id="btn-generate">Trobar Pla</span>
                </button>
            </div>
        </div>

        <div id="plan-result" class="hidden mt-8 md:mt-12 card p-6 md:p-8 border-2 border-primary text-center">
            <h3 class="text-xl md:text-2xl font-bold mb-4 text-amber-900" id="txt-suggested-title">El teu pla ideal és...</h3>
            <div id="suggested-activity" class="text-2xl md:text-3xl font-serif text-amber-800 mb-8 italic px-2"></div>
            
            <div class="flex flex-col sm:flex-row justify-center gap-3 md:gap-4">
                <button onclick="acceptPlan()" class="bg-green-600 hover:bg-green-700 text-white px-8 py-3 rounded-lg font-bold transition shadow-sm w-full sm:w-auto">
                    <i class="fas fa-check mr-2"></i> <span id="btn-do-it">Ho faig!</span>
                </button>
                <button onclick="generatePlan()" class="bg-amber-600 hover:bg-amber-700 text-white px-8 py-3 rounded-lg font-bold transition shadow-sm w-full sm:w-auto">
                    <i class="fas fa-redo mr-2"></i> <span id="btn-other">Un altre pla</span>
                </button>
            </div>
        </div>

        <section class="mt-16 md:mt-24">
            <div class="flex items-center justify-center space-x-4 mb-6 px-4">
                <div class="h-px bg-secondary flex-1"></div>
                <h2 class="text-xl md:text-2xl font-serif italic text-amber-800 flex items-center shrink-0">
                    <i class="fas fa-spa mr-2 text-primary"></i> <span id="txt-inspire">Inspira't</span> <i class="fas fa-spa ml-2 text-primary"></i>
                </h2>
                <div class="h-px bg-secondary flex-1"></div>
            </div>
            
            <div class="collage-container max-w-3xl mx-auto">
                <div class="collage-item">
                    <img src="https://images.unsplash.com/photo-1501785888041-af3ef285b470?auto=format&fit=crop&w=300&q=80" alt="Naturaleza">
                </div>
                <div class="collage-item">
                    <img src="https://images.unsplash.com/photo-1485846234645-a62644f84728?auto=format&fit=crop&w=300&q=80" alt="Cine">
                </div>
                <div class="collage-item">
                    <img src="https://images.unsplash.com/photo-1523301343968-6a6ebf63c672?auto=format&fit=crop&w=300&q=80" alt="Picnic">
                </div>
                <div class="collage-item">
                    <img src="https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?auto=format&fit=crop&w=300&q=80" alt="Café">
                </div>
                <div class="collage-item">
                    <img src="https://images.unsplash.com/photo-1526047932273-341f2a7631f9?auto=format&fit=crop&w=300&q=80" alt="Flores">
                </div>
                <div class="collage-item">
                    <img src="https://images.unsplash.com/photo-1516627145497-ae6968895b74?auto=format&fit=crop&w=300&q=80" alt="Helado">
                </div>
            </div>
        </section>
    </main>

    <footer class="text-center p-8 text-amber-800 opacity-50 text-xs md:text-sm">
        <p id="footer-text">Pla Perfecte &copy; 2024 - Fet amb amor i flors</p>
    </footer>

    <div id="historyModal">
        <div class="bg-white rounded-2xl w-full max-w-2xl max-h-[85vh] overflow-hidden flex flex-col m-3 md:m-4 shadow-2xl">
            <div class="p-4 md:p-6 header-gradient text-white flex justify-between items-center">
                <h2 class="text-xl md:text-2xl font-bold flex items-center">
                    <i class="fas fa-history mr-3"></i> <span id="lbl-history-title">Els Meus Plans Guardats</span>
                </h2>
                <button onclick="toggleHistory()" class="text-3xl">&times;</button>
            </div>
            
            <div id="history-content" class="p-4 md:p-6 overflow-y-auto flex-1 space-y-4">
            </div>

            <div class="p-4 bg-gray-50 border-t flex flex-col sm:flex-row gap-3 justify-between">
                <button onclick="clearHistory()" class="text-red-600 hover:text-red-800 font-bold text-xs md:text-sm uppercase py-2">
                    <i class="fas fa-trash-alt mr-1"></i> <span id="btn-clear-all">Esborrar Historial</span>
                </button>
                <button onclick="toggleHistory()" class="btn-primary px-6 py-2.5 rounded font-bold text-xs md:text-sm uppercase">
                    <span id="btn-close">Tancar</span>
                </button>
            </div>
        </div>
    </div>

    <script>
        let currentPlan = null;
        let history = JSON.parse(localStorage.getItem('planHistory')) || [];

        const translations = {
            ca: {
                title: "Pla Perfecte",
                motto: '"Cada dia és una nova oportunitat per crear records inoblidables"',
                footer: "Pla Perfecte © 2024 - Fet amb amor i flors",
                historyBtn: "Historial",
                date: "Data",
                temp: "Temperatura (°C)",
                with: "Amb qui vas?",
                generate: "Trobar Pla",
                suggested: "El teu pla ideal és...",
                doIt: "Ho faig!",
                other: "Un altre pla",
                historyTitle: "Els Meus Plans Guardats",
                clearAll: "Esborrar Historial",
                close: "Tancar",
                noPlans: "Encara no has guardat cap pla.",
                optionsWith: ["Parella", "Amics", "Sol", "Família"],
                inspire: "Inspira't",
                plans: ["Prendre un gelat artesà", "Passejada per la muntanya", "Tarda de cinema i crispetes", "Visitar un museu local", "Sopar romàntic", "Veure la posta de sol a la platja", "Pícnic al parc", "Sessió de jocs de taula", "Llegir un llibre en una cafeteria", "Ruta en bicicleta", "Anar a patinar", "Cuinar una cosa nova"]
            },
            es: {
                title: "Plan Perfecto",
                motto: '"Cada día es una nueva oportunidad para crear recuerdos inolvidables"',
                footer: "Plan Perfecto © 2024 - Hecho con amor y flores",
                historyBtn: "Historial",
                date: "Fecha",
                temp: "Temperatura (°C)",
                with: "¿Con quién vas?",
                generate: "Encontrar Plan",
                suggested: "Tu plan ideal es...",
                doIt: "¡Lo hago!",
                other: "Otro plan",
                historyTitle: "Mis Planes Guardados",
                clearAll: "Borrar Historial",
                close: "Cerrar",
                noPlans: "Aún no has guardado ningún plan.",
                optionsWith: ["Pareja", "Amigos", "Solo", "Familia"],
                inspire: "Inspírate",
                plans: ["Ir a tomar un helado artesano", "Paseo por la montaña", "Tarde de cine y palomitas", "Visitar un museo local", "Cena romántica", "Ver el atardecer en la playa", "Picnic en el parque", "Sesión de juegos de mesa", "Leer un libro en una cafetería", "Ruta en bicicleta", "Ir a patinar", "Cocinar algo nuevo"]
            },
            en: {
                title: "Perfect Plan",
                motto: '"Every day is a new opportunity to create unforgettable memories"',
                footer: "Perfect Plan © 2024 - Made with love and flowers",
                historyBtn: "History",
                date: "Date",
                temp: "Temperature (°C)",
                with: "Who are you with?",
                generate: "Find a Plan",
                suggested: "Your ideal plan is...",
                doIt: "I'll do it!",
                other: "Other plan",
                historyTitle: "My Saved Plans",
                clearAll: "Clear History",
                close: "Close",
                noPlans: "You haven't saved any plans yet.",
                optionsWith: ["Partner", "Friends", "Alone", "Family"],
                inspire: "Get Inspired",
                plans: ["Get an artisan ice cream", "Mountain walk", "Movie and popcorn", "Visit a local museum", "Romantic dinner", "Watch sunset at the beach", "Picnic in the park", "Board game session", "Read a book in a cafe", "Bike route", "Go skating", "Cook something new"]
            }
        };

        function changeLanguage() {
            const lang = document.getElementById('lang-selector').value;
            const t = translations[lang];

            // Título Principal
            document.getElementById('app-title').innerText = t.title;
            document.title = t.title;
            document.getElementById('footer-text').innerHTML = `${t.title} &copy; 2024 - ${t.footer.split(' - ')[1]}`;

            document.getElementById('txt-motto').innerText = t.motto;
            document.getElementById('txt-history-btn').innerText = t.historyBtn;
            document.getElementById('lbl-date').innerText = t.date;
            document.getElementById('lbl-temp').innerText = t.temp;
            document.getElementById('lbl-with').innerText = t.with;
            document.getElementById('btn-generate').innerText = t.generate;
            document.getElementById('txt-suggested-title').innerText = t.suggested;
            document.getElementById('btn-do-it').innerText = t.doIt;
            document.getElementById('btn-other').innerText = t.other;
            document.getElementById('lbl-history-title').innerText = t.historyTitle;
            document.getElementById('btn-clear-all').innerText = t.clearAll;
            document.getElementById('btn-close').innerText = t.close;
            document.getElementById('txt-inspire').innerText = t.inspire;

            const withSelector = document.getElementById('input-with');
            withSelector.innerHTML = '';
            t.optionsWith.forEach(opt => {
                const option = document.createElement('option');
                option.value = opt;
                option.innerText = opt;
                withSelector.appendChild(option);
            });
        }

        function generatePlan() {
            const lang = document.getElementById('lang-selector').value;
            const plans = translations[lang].plans;
            const randomPlan = plans[Math.floor(Math.random() * plans.length)];
            
            currentPlan = randomPlan;
            document.getElementById('suggested-activity').innerText = randomPlan;
            document.getElementById('plan-result').classList.remove('hidden');
            
            document.getElementById('plan-result').scrollIntoView({ behavior: 'smooth' });
        }

        function acceptPlan() {
            const dateInput = document.getElementById('input-date').value;
            const temp = document.getElementById('input-temp').value || "--";
            const withWho = document.getElementById('input-with').value;
            
            const newRecord = {
                id: Date.now(),
                plan: currentPlan,
                date: dateInput || new Date().toLocaleDateString(),
                temp: temp,
                withWho: withWho,
                rating: 0
            };

            history.unshift(newRecord);
            saveHistory();
            
            const btn = document.getElementById('btn-do-it');
            const lang = document.getElementById('lang-selector').value;
            btn.innerText = lang === 'en' ? "Saved!" : (lang === 'ca' ? "Guardat!" : "¡Guardado!");
            setTimeout(() => {
                btn.innerText = translations[lang].doIt;
                document.getElementById('plan-result').classList.add('hidden');
                toggleHistory();
            }, 800);
        }

        function toggleHistory() {
            const modal = document.getElementById('historyModal');
            if (modal.style.display === 'flex') {
                modal.style.display = 'none';
                document.body.style.overflow = 'auto';
            } else {
                renderHistory();
                modal.style.display = 'flex';
                document.body.style.overflow = 'hidden';
            }
        }

        function renderHistory() {
            const container = document.getElementById('history-content');
            const lang = document.getElementById('lang-selector').value;
            container.innerHTML = '';

            if (history.length === 0) {
                container.innerHTML = `<p class="text-center text-gray-500 italic py-10">${translations[lang].noPlans}</p>`;
                return;
            }

            history.forEach(item => {
