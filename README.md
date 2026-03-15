<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Elite | Official Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #0f172a; color: white; margin: 0; overflow-x: hidden; }
        
        /* Premium UI Elements */
        .glass-panel { background: rgba(255, 255, 255, 0.03); backdrop-blur: 20px; border: 1px solid rgba(255,255,255,0.1); border-radius: 50px; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeInUp 0.6s ease; }
        
        .level-card { background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 40px; transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); cursor: pointer; }
        .level-card:hover:not(.locked) { transform: scale(1.08); background: rgba(255,255,255,0.1); border-color: #3b82f6; }
        .locked { opacity: 0.3; cursor: not-allowed; filter: grayscale(1); }

        .btn-prime { background: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%); transition: 0.3s; }
        .btn-prime:hover { transform: translateY(-3px); box-shadow: 0 15px 30px rgba(59, 130, 246, 0.4); }

        @keyframes fadeInUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        
        #loader { position: fixed; inset: 0; background: #0f172a; z-index: 10000; display: flex; flex-direction: column; items: center; justify-content: center; }
    </style>
</head>
<body>

    <div id="loader">
        <div class="w-16 h-16 border-4 border-blue-900 border-t-blue-400 rounded-full animate-spin mb-6"></div>
        <p class="text-[10px] font-black tracking-[0.5em] uppercase italic opacity-50">Prime Solutions</p>
    </div>

    <div id="loginOverlay" class="fixed inset-0 bg-[#0f172a] z-[9999] flex items-center justify-center p-6 hidden">
        <div class="glass-panel p-10 md:p-14 max-w-md w-full text-center shadow-2xl">
            <div class="w-20 h-20 bg-blue-600 rounded-3xl flex items-center justify-center mx-auto mb-8 text-3xl font-black italic shadow-lg">GB</div>
            <h2 class="text-3xl font-black mb-2 italic">GBTS ENTRANCE</h2>
            <p class="text-[10px] font-bold text-blue-400 mb-10 tracking-[0.3em] uppercase opacity-60">Official Academy Portal 2026</p>

            <input id="uName" type="text" placeholder="Candidate Name" class="w-full p-6 bg-white/5 rounded-3xl font-bold text-center outline-none border-2 border-transparent focus:border-blue-500 transition-all text-white mb-6">
            <button onclick="handleLogin()" class="btn-prime w-full py-6 rounded-3xl font-black text-lg uppercase tracking-widest shadow-xl">Verify & Enter</button>
            <p class="mt-8 text-[9px] text-white/40 font-bold uppercase italic">Notice: Anonymous system active for 2026 session.</p>
        </div>
    </div>

    <nav class="p-6 md:px-12 flex justify-between items-center border-b border-white/5 sticky top-0 bg-[#0f172a]/80 backdrop-blur-md z-50">
        <div class="flex items-center gap-4">
            <div class="w-10 h-10 bg-blue-600 rounded-xl flex items-center justify-center font-black italic text-xl">G</div>
            <h1 class="font-black text-sm uppercase italic hidden md:block tracking-tighter">GBTS Academy</h1>
        </div>
        <div class="flex gap-6 text-[10px] font-black uppercase italic">
            <button onclick="showTab('dashTab')" class="text-blue-400">Roadmap</button>
            <button onclick="showTab('rankTab')" class="text-white/40">Hall of Fame</button>
            <button onclick="logout()" class="text-red-500">Exit</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6 md:p-12">
        <div id="dashTab" class="tab-content active-tab">
            <div class="p-10 md:p-20 rounded-[60px] bg-gradient-to-br from-blue-900/40 to-indigo-900/20 border border-white/10 mb-16 relative overflow-hidden" onclick="checkAdminTrigger()">
                <div class="relative z-10 text-center md:text-left">
                    <p class="text-[10px] font-black text-blue-400 mb-4 uppercase tracking-[0.4em]">Active Candidate Identity</p>
                    <h2 id="dispName" class="text-5xl md:text-7xl font-black italic uppercase mb-8 tracking-tighter">---</h2>
                    <div class="inline-flex bg-white/5 px-6 py-3 rounded-2xl text-[10px] font-black border border-white/10 italic">
                        ACADEMY STATUS: LEVEL <span id="dispLvl" class="text-blue-400">01</span>
                    </div>
                </div>
                <div class="absolute -right-20 -top-20 w-80 h-80 bg-blue-600/10 rounded-full blur-[100px]"></div>
            </div>

            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-8 px-2"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-xl mx-auto glass-panel p-10 md:p-14">
                <h3 class="text-3xl font-black text-center mb-12 italic uppercase tracking-tighter">Top Performers</h3>
                <div id="leaderList" class="space-y-4"></div>
            </div>
        </div>

        <div id="adminTab" class="tab-content">
            <div class="glass-panel p-10">
                <div class="flex justify-between items-center mb-10">
                    <h3 class="text-2xl font-black text-red-500 italic">ADMIN CONTROL</h3>
                    <button onclick="showTab('dashTab')" class="text-[10px] font-black underline">CLOSE</button>
                </div>
                <div id="adminUserList" class="space-y-3"></div>
            </div>
        </div>
    </main>

    <div id="quizModal" class="fixed inset-0 bg-slate-950/98 z-[10000] hidden items-center justify-center p-6 backdrop-blur-xl">
        <div class="glass-panel p-10 md:p-14 max-w-lg w-full text-center border-white/20">
            <h3 id="mTitle" class="text-3xl font-black mb-6 italic uppercase">Verification</h3>
            <div id="mStudy" class="bg-white/5 p-8 rounded-3xl mb-8 text-sm italic text-blue-200 border border-white/10"></div>
            <input id="ansInp" type="text" placeholder="Enter System Code" class="w-full p-6 bg-white/5 rounded-3xl mb-8 text-center font-black border-2 border-transparent focus:border-blue-500 outline-none text-xl">
            <button id="subBtn" class="btn-prime w-full py-6 rounded-3xl font-black text-lg">CONFIRM RESPONSE</button>
        </div>
    </div>

    <script>
        // --- UPDATED CONFIG WITH DATABASE URL ---
        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com/", // ADDED THIS
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const db = firebase.database();

        let userData = null;
        let adminClicks = 0;

        window.onload = () => {
            setTimeout(() => {
                document.getElementById('loader').style.display = 'none';
                const saved = localStorage.getItem('gbts_v4_user');
                if(saved) processLogin(saved);
                else document.getElementById('loginOverlay').style.display = 'flex';
            }, 2000);
        };

        async function handleLogin() {
            const name = document.getElementById('uName').value.trim();
            if(!name || name.length < 3) return alert("Sweetie, apna naam sahi se likhein!");
            localStorage.setItem('gbts_v4_user', name);
            processLogin(name);
        }

        async function processLogin(name) {
            const userKey = name.toLowerCase().replace(/\s/g, '_').substring(0, 20);
            try {
                const res = await auth.signInAnonymously(); // Anonymous system
                const userRef = db.ref('gbts_v4/' + userKey);
                const snap = await userRef.get();
                
                if(!snap.exists()) {
                    await userRef.set({ uid: res.user.uid, name: name, level: 1, key: userKey });
                }
                document.getElementById('loginOverlay').style.display = 'none';
                initApp(userKey);
            } catch (e) {
                console.error(e);
                alert("Database Error! Sweetie, Firebase Console mein 'databaseURL' aur 'Anonymous Auth' lazmi check karein.");
            }
        }

        function initApp(key) {
            db.ref('gbts_v4/' + key).on('value', snap => {
                userData = snap.val();
                if(userData) {
                    document.getElementById('dispName').innerText = userData.name;
                    document.getElementById('dispLvl').innerText = userData.level < 10 ? "0"+userData.level : userData.level;
                    renderMap();
                }
            });
            loadRanks();
            loadAdminPanel();
        }

        function renderMap() {
            const grid = document.getElementById('levelGrid'); grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `<div onclick="openTest(${i}, ${lock})" class="level-card p-10 text-center ${lock ? 'locked' : 'animate__animated animate__fadeInUp'}">
                    <div class="text-4xl mb-4">${lock ? '🔒' : '🎖️'}</div>
                    <p class="text-[10px] font-black uppercase text-white/30">Level ${i}</p>
                </div>`;
            }
        }

        function openTest(num, lock) {
            if(lock || num !== userData.level) return;
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mStudy').innerText = "Hello " + userData.name + "! Level " + (num+1) + " ko unlock karne ke liye niche 'confirm' likhein.";
            document.getElementById('subBtn').onclick = () => {
                if(document.getElementById('ansInp').value.trim().toLowerCase() === 'confirm') {
                    confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
                    db.ref('gbts_v4/' + userData.key).update({ level: userData.level + 1 });
                    db.ref('merit_v4/' + userData.key).set({ name: userData.name, level: userData.level + 1 });
                    document.getElementById('quizModal').style.display = 'none';
                    document.getElementById('ansInp').value = '';
                } else { alert("Ghalat code! Dubara koshish karein."); }
            };
        }

        function loadRanks() {
            db.ref('merit_v4').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let ranks = []; snap.forEach(c => ranks.push(c.val()));
                ranks.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between items-center p-5 bg-white/5 rounded-3xl border border-white/5">
                        <span class="font-black text-xs uppercase italic">#${i+1} ${r.name}</span>
                        <span class="bg-blue-600 px-4 py-1 rounded-full text-[10px] font-black">LVL ${r.level}</span>
                    </div>`;
                });
            });
        }

        function checkAdminTrigger() {
            adminClicks++;
            if(adminClicks >= 5) {
                const pass = prompt("Enter Admin Key, Sweetie:");
                if(pass === "prime786") showTab('adminTab');
                adminClicks = 0;
            }
        }

        function loadAdminPanel() {
            db.ref('gbts_v4').on('value', snap => {
                const list = document.getElementById('adminUserList'); list.innerHTML = '';
                snap.forEach(c => {
                    const u = c.val();
                    list.innerHTML += `<div class="flex justify-between items-center p-4 bg-white/5 rounded-2xl mb-2">
                        <span class="text-xs font-bold uppercase">${u.name} (LVL ${u.level})</span>
                        <div class="flex gap-4">
                            <button onclick="db.ref('gbts_v4/${u.key}').remove()" class="text-red-500 text-[10px] font-black underline">DELETE</button>
                        </div>
                    </div>`;
                });
            });
        }

        function showTab(id) { document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab')); document.getElementById(id).classList.add('active-tab'); }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
