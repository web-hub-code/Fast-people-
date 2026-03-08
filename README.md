<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Professional Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap');
        
        :root { 
            --bg: #f8fafc; --card: #ffffff; --text: #0f172a; 
            --blue: #2563eb; --border: #e2e8f0; 
        }

        .dark-mode { 
            --bg: #020617; --card: #0f172a; --text: #f8fafc; 
            --border: #1e293b; 
        }

        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background: var(--bg); color: var(--text); 
            transition: 0.4s cubic-bezier(0.4, 0, 0.2, 1); 
            padding-bottom: 100px; overflow-x: hidden; 
        }

        .glass { 
            background: var(--card); border: 1px solid var(--border); 
            border-radius: 28px; box-shadow: 0 10px 25px rgba(0,0,0,0.03); 
        }

        .page { display: none; animation: slideUp 0.5s ease forwards; }
        .active-page { display: block !important; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }

        .btn-premium { 
            background: linear-gradient(135deg, #2563eb, #1d4ed8); 
            color: white; font-weight: 800; border-radius: 20px; 
            box-shadow: 0 10px 20px rgba(37, 99, 235, 0.2); 
        }

        input { 
            background: var(--card); border: 1px solid var(--border); 
            border-radius: 18px; padding: 18px; color: var(--text); width: 100%; outline: none; 
        }

        .nav-item { opacity: 0.4; transition: 0.3s; font-size: 9px; font-weight: 800; color: var(--text); }
        .nav-active { opacity: 1; color: var(--blue); transform: translateY(-5px); }

        marquee { background: rgba(37, 99, 235, 0.05); color: var(--blue); font-size: 11px; font-weight: 800; padding: 12px 0; }
    </style>
</head>
<body id="app-body">

    <div id="auth-screen" class="fixed inset-0 bg-white z-[99999] flex flex-col items-center justify-center p-8 transition-all">
        <div class="mb-10 text-center">
            <h1 class="text-4xl font-black italic text-blue-600 tracking-tighter uppercase">PAK<span class="text-slate-900">GOLD</span></h1>
            <p class="text-[10px] font-bold opacity-40 uppercase tracking-[4px] mt-2">Certified Investment Node</p>
        </div>
        <div class="glass w-full p-8 space-y-5">
            <input type="text" id="login-user" placeholder="Username">
            <input type="password" id="login-pass" placeholder="Password">
            <button onclick="handleAuth()" class="w-full p-5 btn-premium uppercase text-sm tracking-widest mt-2">Start Mining</button>
        </div>
    </div>

    <marquee id="v-news">Welcome Sweetie! ₨ 200 Plans are LIVE • 10% Referral Commission Active 😘</marquee>

    <header class="p-6 flex justify-between items-center sticky top-0 bg-opacity-80 backdrop-blur-lg z-[5000]">
        <div onclick="tapAdmin()">
            <h1 class="text-xl font-black text-blue-600 italic uppercase">PAK<span id="logo-gold" class="text-slate-900 transition-colors">GOLD</span></h1>
            <p id="u-id" class="text-[8px] font-bold opacity-50 uppercase tracking-[2px]">@User</p>
        </div>
        <div class="flex items-center gap-4">
            <button onclick="toggleTheme()" class="w-10 h-10 glass flex items-center justify-center text-lg">🌓</button>
            <div class="text-right">
                <h2 id="u-bal" class="text-2xl font-black tracking-tighter text-blue-600">₨ 0.00</h2>
            </div>
        </div>
    </header>

    <main class="p-4 space-y-6">
        <div id="p-home" class="page active-page space-y-6">
            <div class="glass p-7 border-l-8 border-blue-600 flex justify-between items-center">
                <div>
                    <p class="text-[9px] font-black opacity-40 uppercase tracking-widest">Global Payout Timer</p>
                    <h3 id="timer" class="text-2xl font-black text-blue-600 tracking-widest">23:59:59</h3>
                </div>
                <div class="w-14 h-14 rounded-full bg-blue-50/50 flex items-center justify-center text-2xl animate-bounce">⚡</div>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <button onclick="showPage('deposit')" class="glass p-6 text-center font-black text-[11px] uppercase border-b-4 border-green-500 hover:bg-green-50">Deposit</button>
                <button onclick="showPage('withdraw')" class="glass p-6 text-center font-black text-[11px] uppercase border-b-4 border-red-500 hover:bg-red-50">Withdraw</button>
            </div>
        </div>

        <div id="p-nodes" class="page space-y-4">
            <div class="flex gap-2 p-1.5 bg-slate-100 rounded-2xl mb-6">
                <button onclick="renderNodes('normal')" id="btn-n" class="flex-1 p-3.5 rounded-xl bg-blue-600 text-white text-[10px] font-black uppercase shadow-lg">Cloud Nodes</button>
                <button onclick="renderNodes('vip')" id="btn-v" class="flex-1 p-3.5 rounded-xl text-[10px] font-black uppercase opacity-40">VIP Servers</button>
            </div>
            <div id="nodes-grid" class="space-y-4"></div>
        </div>

        <div id="p-deposit" class="page space-y-4">
            <div class="glass p-8">
                <h3 class="text-xs font-black uppercase text-blue-600 mb-6 tracking-widest text-center italic">Fast Funding Gateway</h3>
                <div class="bg-blue-600 p-6 rounded-3xl text-white shadow-xl shadow-blue-200 mb-8">
                    <p class="text-[10px] font-bold opacity-80 mb-1 uppercase tracking-widest">Pay Via EasyPaisa/JazzCash</p>
                    <p class="text-2xl font-black tracking-tight">03379827882</p>
                    <p class="text-[8px] mt-4 opacity-60">ADMIN: MUHAMMAD NAZIM</p>
                </div>
                <div class="space-y-4">
                    <input type="number" id="d-amt" placeholder="Amount (Minimum ₨ 200)">
                    <input type="text" id="d-tid" placeholder="Transaction ID (TID)">
                    <p class="text-[9px] font-bold opacity-40 ml-2">UPLOAD RECEIPT SCREENSHOT</p>
                    <input type="file" id="d-proof" class="!p-3 text-xs border-dashed border-2 border-blue-200">
                    <button onclick="submitDeposit()" class="w-full p-5 btn-premium uppercase text-[11px] tracking-[2px] mt-4">Confirm Deposit</button>
                </div>
            </div>
        </div>

        <div id="p-menu" class="page space-y-4 text-center">
            <div class="glass p-7 bg-gradient-to-br from-blue-50 to-white">
                <p class="text-[9px] font-black opacity-40 uppercase mb-3">Invite & Earn 10%</p>
                <p id="ref-link" class="text-[10px] text-blue-600 p-4 bg-white rounded-2xl border border-blue-100 break-all font-bold">Generating...</p>
                <button onclick="copyRef()" class="mt-4 text-[10px] font-black uppercase text-blue-600 tracking-widest">Copy My Link</button>
            </div>
            <button onclick="window.location.href='https://wa.me/923705519562'" class="w-full glass p-5 text-left text-xs font-black uppercase text-green-600 flex justify-between items-center">
                <span>Official WhatsApp Support</span>
                <span>💬</span>
            </button>
            <button onclick="logout()" class="w-full p-5 bg-red-50 text-red-600 rounded-2xl font-black uppercase text-xs mt-10 tracking-widest border border-red-100">Secure Logout</button>
        </div>
    </main>

    <nav class="fixed bottom-0 w-full h-24 glass rounded-t-[3rem] border-t border-slate-100 flex justify-around items-center px-8 z-[10000]">
        <button onclick="showPage('home')" id="n-home" class="nav-item nav-active flex flex-col items-center gap-1"><span class="text-2xl">🏠</span><span>HOME</span></button>
        <button onclick="showPage('nodes')" id="n-nodes" class="nav-item flex flex-col items-center gap-1"><span class="text-2xl">⚡</span><span>NODES</span></button>
        <button onclick="showPage('history')" id="n-history" class="nav-item flex flex-col items-center gap-1"><span class="text-2xl">📜</span><span>LOGS</span></button>
        <button onclick="showPage('menu')" id="n-menu" class="nav-item flex flex-col items-center gap-1"><span class="text-2xl">👤</span><span>MENU</span></button>
    </nav>

    <script>
        const firebaseConfig = { apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg", authDomain: "investment-84f4e.firebaseapp.com", projectId: "investment-84f4e", storageBucket: "investment-84f4e.appspot.com", messagingSenderId: "975293889308", appId: "1:975293889308:web:6d034a99cc966c75ff58d9" };
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        let user = localStorage.getItem('pg_user');
        let tapCount = 0;

        function toggleTheme() {
            document.body.classList.toggle('dark-mode');
            const logoGold = document.getElementById('logo-gold');
            if(document.body.classList.contains('dark-mode')) logoGold.style.color = '#ffffff';
            else logoGold.style.color = '#0f172a';
        }

        function tapAdmin() {
            tapCount++;
            if(tapCount >= 4) {
                let p = prompt("Admin Key:");
                if(p === "admin123") { localStorage.setItem('is_admin', 'true'); alert("Admin Mode Active! 😘"); }
                tapCount = 0;
            }
            setTimeout(() => tapCount = 0, 2000);
        }

        function handleAuth() {
            const u = document.getElementById('login-user').value.toLowerCase().trim();
            if(u.length < 3) return alert("Username too short! 😘");
            localStorage.setItem('pg_user', u);
            location.reload();
        }

        function renderNodes(type) {
            const grid = document.getElementById('nodes-grid'); grid.innerHTML = '';
            const isNormal = type === 'normal';
            document.getElementById('btn-n').className = isNormal ? 'flex-1 p-3.5 rounded-xl bg-blue-600 text-white text-[10px] font-black uppercase shadow-lg' : 'flex-1 p-3.5 rounded-xl text-[10px] font-black uppercase opacity-40';
            document.getElementById('btn-v').className = !isNormal ? 'flex-1 p-3.5 rounded-xl bg-blue-600 text-white text-[10px] font-black uppercase shadow-lg' : 'flex-1 p-3.5 rounded-xl text-[10px] font-black uppercase opacity-40';

            if(isNormal) {
                for(let i=1; i<=20; i++) {
                    let cost = i === 1 ? 200 : 200 + (i-1)*700;
                    grid.innerHTML += `<div class="glass p-6 flex justify-between items-center border-r-8 border-blue-600"><div class="flex items-center gap-4"><div class="w-12 h-12 rounded-2xl bg-blue-50 flex items-center justify-center">⚙️</div><div><p class="text-[8px] font-bold text-blue-600 uppercase tracking-widest">S-NODE v${i}</p><h4 class="text-xl font-black">₨ ${cost.toLocaleString()}</h4></div></div><button class="px-7 py-3.5 btn-premium text-[10px] uppercase">Rent</button></div>`;
                }
            } else {
                [50000, 100000, 250000, 500000, 1000000].forEach((v, idx) => {
                    grid.innerHTML += `<div class="glass p-6 flex justify-between items-center border-r-8 border-yellow-500"><div class="flex items-center gap-4"><div class="w-12 h-12 rounded-2xl bg-yellow-50 flex items-center justify-center">👑</div><div><p class="text-[8px] font-bold text-yellow-500 uppercase tracking-widest italic">VIP Cluster 0${idx+1}</p><h4 class="text-xl font-black">₨ ${v.toLocaleString()}</h4></div></div><button class="px-7 py-3.5 btn-premium text-[10px] uppercase">Rent</button></div>`;
                });
            }
        }

        async function submitDeposit() {
            const a = document.getElementById('d-amt').value; const t = document.getElementById('d-tid').value; const f = document.getElementById('d-proof').files[0];
            if(!a || !t || !f) return alert("Fill all details with screenshot! 😘");
            const r = new FileReader(); r.readAsDataURL(f);
            r.onload = async () => {
                await db.collection("requests").add({ user, amount: parseFloat(a), tid: t, proof: r.result, status: 'pending', type: 'deposit', time: Date.now() });
                alert("Deposit Received! Verified soon. 😘"); showPage('home');
            };
        }

        function init() {
            if(user) {
                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('u-id').innerText = "@" + user.toUpperCase();
                document.getElementById('ref-link').innerText = window.location.origin + "?ref=" + user;
                db.collection("users").doc(user).onSnapshot(d => {
                    document.getElementById('u-bal').innerText = "₨ " + (d.data()?.balance || 0).toLocaleString();
                });
                renderNodes('normal'); startTimer();
            }
        }

        function startTimer() {
            setInterval(() => {
                let d = new Date(); let h = 23-d.getHours(); let m = 59-d.getMinutes(); let s = 59-d.getSeconds();
                document.getElementById('timer').innerText = `${h<10?'0'+h:h}:${m<10?'0'+m:m}:${s<10?'0'+s:s}`;
            }, 1000);
        }

        function showPage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-'+p).classList.add('active-page');
            document.querySelectorAll('.nav-item').forEach(b => { b.classList.remove('nav-active'); b.classList.add('opacity-40'); });
            document.getElementById('n-'+p)?.classList.add('nav-active');
            document.getElementById('n-'+p)?.classList.remove('opacity-40');
        }

        function logout() { localStorage.clear(); location.reload(); }
        window.onload = init;
    </script>
</body>
</html>
