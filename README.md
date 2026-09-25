# FLOWTECH-APP

<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>FLOWTECH INSTRUMENTATION - Medical Division</title>
    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#2C1A1D">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <link rel="apple-touch-icon" href="4bd04978-c60c-4fce-bc98-362d5c149660.jpg">
    
    <!-- Tailwind CSS & Lucide Icons -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- EmailJS SDK -->
    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
    <script type="text/javascript">
        (function() {
            emailjs.init("Xb1jkPgn-2agF0DPK");
        })();
    </script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brandDark: '#2C1A1D',
                        brandCream: '#FDFBF7',
                        brandCopper: '#8C4B33',
                        brandCopperHover: '#6D3825',
                        brandLightBg: '#F7F3EE'
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-brandCream text-brandDark min-h-full font-sans antialiased flex flex-col select-none">

    <!-- Mobile Status Bar Simulation -->
    <div class="bg-brandDark text-stone-300 text-[11px] px-4 py-1 flex justify-between items-center border-b border-brandCopper/30">
        <span id="appClock" class="font-mono">12:00</span>
        <div class="flex items-center space-x-2">
            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
            <span class="text-[10px] tracking-wider uppercase">Standalone App Active</span>
        </div>
        <div class="flex items-center space-x-1 font-mono text-[10px]">
            <span>5G</span>
            <span>100%</span>
        </div>
    </div>

    <!-- App Header Bar -->
    <header class="bg-white border-b border-stone-200 sticky top-0 z-50 shadow-sm px-4 py-3 flex items-center justify-between">
        <div class="flex items-center space-x-3">
            <img src="4bd04978-c60c-4fce-bc98-362d5c149660.jpg" alt="Logo" class="h-10 w-10 object-contain rounded-lg border border-stone-200">
            <div>
                <h1 class="text-base font-bold tracking-tight text-brandDark leading-none">FLOWTECH</h1>
                <p class="text-[9px] font-bold uppercase tracking-widest text-brandCopper mt-0.5">Medical App PH</p>
            </div>
        </div>
        <button onclick="switchTab('quote')" class="relative p-2 bg-brandCream border border-brandCopper/20 text-brandCopper rounded-xl">
            <i data-lucide="shopping-cart" class="w-5 h-5"></i>
            <span id="cart-badge" class="absolute -top-1 -right-1 bg-brandCopper text-white text-[9px] font-bold w-4 h-4 rounded-full flex items-center justify-center">0</span>
        </button>
    </header>

    <!-- Main View Area -->
    <main class="flex-grow p-4 pb-24 overflow-y-auto">

        <!-- 1. STORE TAB -->
        <div id="view-store" class="space-y-4">
            <div class="bg-brandDark text-white p-6 rounded-2xl shadow-lg relative overflow-hidden space-y-3">
                <span class="bg-brandCopper/40 text-amber-200 text-[10px] font-bold px-2.5 py-1 rounded-full uppercase tracking-wider">Catalog 2026</span>
                <h2 class="text-xl font-serif font-medium">Medical Robotics & Equipment</h2>
                <p class="text-stone-300 text-xs">Official hospital procurement system for the Philippines.</p>
            </div>

            <div class="relative">
                <i data-lucide="search" class="w-4 h-4 text-stone-400 absolute left-3 top-3"></i>
                <input type="text" id="searchInput" oninput="renderProducts()" placeholder="Search equipment..." class="w-full pl-9 pr-4 py-2.5 bg-white rounded-xl text-xs border border-stone-200 focus:outline-none focus:ring-2 focus:ring-brandCopper shadow-sm">
            </div>

            <div id="productGrid" class="grid grid-cols-1 gap-4">
                <!-- Populated via JS -->
            </div>
        </div>

        <!-- 2. TELEMETRY TAB -->
        <div id="view-telemetry" class="hidden space-y-4">
            <div class="bg-stone-900 text-white p-5 rounded-2xl shadow-xl space-y-3 border border-stone-800">
                <div class="flex justify-between items-center">
                    <span class="text-xs font-bold text-emerald-400 flex items-center gap-2">
                        <span class="w-2.5 h-2.5 rounded-full bg-emerald-500 animate-ping"></span> Live ECG & Oxygen Telemetry
                    </span>
                    <span class="text-[10px] font-mono bg-emerald-950 text-emerald-300 px-2 py-0.5 rounded">240 Hz</span>
                </div>
                <canvas id="liveCanvas" width="700" height="200" class="w-full h-44 bg-black rounded-xl border border-stone-800"></canvas>
            </div>
        </div>

        <!-- 3. CALIBRATOR TAB -->
        <div id="view-calibrator" class="hidden space-y-4">
            <div class="bg-white p-6 rounded-2xl border border-stone-200 shadow-sm space-y-4">
                <h3 class="font-bold text-sm text-brandDark border-b pb-2">SmartPump Flow Rate Calibrator</h3>
                <div class="space-y-3 text-xs">
                    <div>
                        <label class="block font-semibold text-stone-700 mb-1">Target Dosage (mg/hr):</label>
                        <input type="number" id="dose" value="50" oninput="calcFlow()" class="w-full p-2.5 bg-brandCream border border-stone-200 rounded-xl font-mono text-sm">
                    </div>
                    <div>
                        <label class="block font-semibold text-stone-700 mb-1">Fluid Concentration (mg/mL):</label>
                        <input type="number" id="conc" value="2" oninput="calcFlow()" class="w-full p-2.5 bg-brandCream border border-stone-200 rounded-xl font-mono text-sm">
                    </div>
                    <div class="p-4 bg-brandCream rounded-xl text-center border border-brandCopper/30">
                        <p class="text-stone-500 text-[11px]">Required Infusion Rate</p>
                        <p class="text-xl font-bold font-mono text-brandCopper mt-1" id="flowResult">25.00 mL/hr</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- 4. QUOTE & TENDER TAB -->
        <div id="view-quote" class="hidden space-y-4">
            <div class="bg-white p-6 rounded-2xl border border-stone-200 shadow-sm space-y-4">
                <h3 class="font-bold text-sm text-brandDark border-b pb-2">Hospital Purchase Requisition</h3>
                <div id="cartItems" class="space-y-2 divide-y divide-stone-100"></div>

                <div id="deliveryFormSection" class="border-t pt-4 space-y-3 hidden text-xs">
                    <h4 class="font-bold text-brandDark">Delivery Details</h4>
                    <input type="text" id="hospName" placeholder="Hospital Name *" class="w-full p-2.5 bg-brandCream border rounded-xl">
                    <input type="text" id="hospDept" placeholder="Department (e.g. ICU) *" class="w-full p-2.5 bg-brandCream border rounded-xl">
                    <input type="text" id="contactPerson" placeholder="Contact Person *" class="w-full p-2.5 bg-brandCream border rounded-xl">
                    <input type="text" id="contactPhone" placeholder="Mobile Number (+63) *" class="w-full p-2.5 bg-brandCream border rounded-xl">
                    <textarea id="deliveryAddress" placeholder="Complete Hospital Address *" rows="2" class="w-full p-2.5 bg-brandCream border rounded-xl"></textarea>
                </div>

                <div id="cartSummary" class="border-t pt-3 space-y-1 text-xs hidden">
                    <div class="flex justify-between"><span>Subtotal:</span><span id="subtotalVal">â‚±0</span></div>
                    <div class="flex justify-between"><span>VAT (12%):</span><span id="vatVal">â‚±0</span></div>
                    <div class="flex justify-between font-bold text-sm text-brandDark pt-2 border-t">
                        <span>Total:</span><span id="totalVal" class="text-brandCopper">â‚±0</span>
                    </div>
                    <button id="submitBtn" onclick="submitRequisition()" class="w-full mt-3 bg-brandCopper text-white py-3 rounded-xl font-bold text-xs shadow-md">
                        Submit Requisition (Email Admin)
                    </button>
                </div>
            </div>
        </div>

    </main>

    <!-- Native App Bottom Navigation Bar -->
    <nav class="fixed bottom-0 left-0 right-0 bg-white border-t border-stone-200 px-6 py-2 flex justify-around items-center z-50 shadow-lg">
        <button onclick="switchTab('store')" id="tab-store" class="nav-tab flex flex-col items-center text-brandCopper">
            <i data-lucide="store" class="w-5 h-5"></i>
            <span class="text-[10px] font-bold mt-1">Store</span>
        </button>
        <button onclick="switchTab('telemetry')" id="tab-telemetry" class="nav-tab flex flex-col items-center text-stone-400">
            <i data-lucide="activity" class="w-5 h-5"></i>
            <span class="text-[10px] font-bold mt-1">Telemetry</span>
        </button>
        <button onclick="switchTab('calibrator')" id="tab-calibrator" class="nav-tab flex flex-col items-center text-stone-400">
            <i data-lucide="calculator" class="w-5 h-5"></i>
            <span class="text-[10px] font-bold mt-1">Calibrator</span>
        </button>
        <button onclick="switchTab('quote')" id="tab-quote" class="nav-tab flex flex-col items-center text-stone-400">
            <i data-lucide="file-text" class="w-5 h-5"></i>
            <span class="text-[10px] font-bold mt-1">Requisition</span>
        </button>
    </nav>

    <script>
        const products = [
            { id: 1, name: "SonoMotion Non-Invasive Kidney Stone Device", category: "Therapy", price: 20000, desc: "Acoustic lithotripsy stone clearance system." },
            { id: 2, name: "FT-AutoGlide Smart Wheelchair", category: "Mobility", price: 35000, desc: "Autonomous mobile LiDAR navigation wheelchair." },
            { id: 3, name: "FT SmartPump 800 Infusion System", category: "Infusion", price: 15000, desc: "Volumetric infusion pump with micro-flow precision." },
            { id: 4, name: "FT-900 ICU Oxygen Flowmeter", category: "Respiratory", price: 4000, desc: "Digital oxygen flowmeter with inline sensors." },
            { id: 5, name: "FT-RoboSurge 5000 Robot Surgery System", category: "Robotics", price: 150000000, desc: "4-arm 3D HD surgical haptic console." }
        ];

        let cart = [];

        function updateClock() {
            const now = new Date();
            document.getElementById('appClock').innerText = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
        }
        setInterval(updateClock, 1000);
        updateClock();

        function switchTab(tabId) {
            ['store', 'telemetry', 'calibrator', 'quote'].forEach(t => {
                document.getElementById('view-' + t).classList.add('hidden');
                const btn = document.getElementById('tab-' + t);
                btn.classList.remove('text-brandCopper');
                btn.classList.add('text-stone-400');
            });
            document.getElementById('view-' + tabId).classList.remove('hidden');
            const activeBtn = document.getElementById('tab-' + tabId);
            activeBtn.classList.remove('text-stone-400');
            activeBtn.classList.add('text-brandCopper');
        }

        function renderProducts() {
            const search = document.getElementById("searchInput").value.toLowerCase();
            const grid = document.getElementById("productGrid");
            const filtered = products.filter(p => p.name.toLowerCase().includes(search) || p.desc.toLowerCase().includes(search));

            grid.innerHTML = filtered.map(p => `
                <div class="bg-white rounded-2xl border border-stone-200 p-4 space-y-2 shadow-sm">
                    <h3 class="font-bold text-brandDark text-sm">${p.name}</h3>
                    <p class="text-stone-500 text-xs">${p.desc}</p>
                    <div class="flex justify-between items-center pt-2">
                        <span class="text-base font-bold text-brandCopper">â‚±${p.price.toLocaleString()}</span>
                        <button onclick="addToCart(${p.id})" class="bg-brandDark text-white px-3 py-1.5 rounded-xl text-xs font-bold">Add to Cart</button>
                    </div>
                </div>
            `).join('');
        }

        function addToCart(id) {
            const product = products.find(p => p.id === id);
            const existing = cart.find(item => item.product.id === id);
            if (existing) { existing.qty++; } else { cart.push({ product, qty: 1 }); }
            updateCartUI();
        }

        function updateCartUI() {
            document.getElementById("cart-badge").innerText = cart.reduce((a, c) => a + c.qty, 0);
            const container = document.getElementById("cartItems");
            const form = document.getElementById("deliveryFormSection");
            const summary = document.getElementById("cartSummary");

            if (cart.length === 0) {
                container.innerHTML = `<p class="text-xs text-stone-400 text-center py-4">Cart is empty.</p>`;
                form.classList.add("hidden");
                summary.classList.add("hidden");
                return;
            }

            let subtotal = 0;
            container.innerHTML = cart.map(i => {
                subtotal += i.product.price * i.qty;
                return `<div class="py-2 flex justify-between text-xs"><span>${i.product.name} (x${i.qty})</span><span class="font-bold">â‚±${(i.product.price * i.qty).toLocaleString()}</span></div>`;
            }).join('');

            const vat = subtotal * 0.12;
            document.getElementById("subtotalVal").innerText = "â‚±" + subtotal.toLocaleString();
            document.getElementById("vatVal").innerText = "â‚±" + vat.toLocaleString();
            document.getElementById("totalVal").innerText = "â‚±" + (subtotal + vat).toLocaleString();
            form.classList.remove("hidden");
            summary.classList.remove("hidden");
        }

        function submitRequisition() {
            const name = document.getElementById("hospName").value;
            const contact = document.getElementById("contactPerson").value;
            const phone = document.getElementById("contactPhone").value;
            if (!name || !contact || !phone) { alert("Please fill in required hospital fields."); return; }

            const templateParams = {
                to_email: "kcaleb090808@gmail.com",
                hospital_name: name,
                contact_person: contact,
                contact_phone: phone,
                order_items: cart.map(i => `${i.product.name} x${i.qty}`).join(', '),
                total_amount: document.getElementById("totalVal").innerText
            };

            emailjs.send("service_123456", "template_xdzxdnh", templateParams).then(() => {
                alert("Requisition sent successfully to kcaleb090808@gmail.com!");
                cart = [];
                updateCartUI();
            }).catch(err => alert("Error sending requisition: " + JSON.stringify(err)));
        }

        function calcFlow() {
            const d = parseFloat(document.getElementById("dose").value) || 0;
            const c = parseFloat(document.getElementById("conc").value) || 0;
            document.getElementById("flowResult").innerText = c > 0 ? (d / c).toFixed(2) + " mL/hr" : "0.00 mL/hr";
        }

        // Live ECG Canvas
        const canvas = document.getElementById("liveCanvas");
        const ctx = canvas.getContext("2d");
        let x = 0;
        function drawECG() {
            ctx.fillStyle = "rgba(0,0,0,0.1)";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            const y = canvas.height / 2 + Math.sin(x * 0.05) * 30 + (Math.random() - 0.5) * 6;
            ctx.strokeStyle = "#10B981"; ctx.lineWidth = 2;
            ctx.beginPath(); ctx.arc(x, y, 1, 0, Math.PI * 2); ctx.stroke();
            x = (x + 2) % canvas.width;
            requestAnimationFrame(drawECG);
        }
        drawECG();

        if ('serviceWorker' in navigator) { navigator.serviceWorker.register('sw.js'); }
        document.addEventListener("DOMContentLoaded", () => { renderProducts(); lucide.createIcons(); });
    </script>
</body>
</html>