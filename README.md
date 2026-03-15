<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Elite | Prime Solutions Official</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        
        :root { --primary: #1e3a8a; --accent: #3b82f6; --dark: #0f172a; }
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; color: var(--dark); overflow-x: hidden; }

        /* Premium Glass Effects */
        .premium-card { background: rgba(255, 255, 255, 0.9); backdrop-blur: 10px; border: 1px solid rgba(255,255,255,0.5); border-radius: 40px; box-shadow: 0 25px 50px -12px rgba(0,0,0,0.05); }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeInUp 0.6s ease both; }

        /* Level Cards Styling */
        .level-card { transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); position: relative; overflow: hidden; }
        .level-card:hover:not(.locked) { transform: translateY(-10px) scale(1.05); border-color: var(--primary); box-shadow: 0 20px 40px rgba(30,58,138,0.1); }
        .locked { filter: grayscale(1); opacity: 0.5; cursor: not-allowed; background: #f1f5f9 !important; }

        /* Animated Welcome Banner */
        .welcome-hero { background: linear-gradient(135deg, #0f172a 0%, #1e3a8a 100%); border-radius: 60px; position: relative; overflow: hidden; }
        .hero-circle { position: absolute; border-radius: 50%; background: rgba(59, 130, 246, 0.1); blur: 80px; }

        /* Custom Loader */
        #mainLoader { position: fixed; inset: 0; background: #0f172a; z-index: 99999; display: flex; flex-direction: column; items: center; justify-content: center; }
        .spinner { width: 50px; height: 50px; border: 5px solid #1e3a8a; border-top-color: #3b82f6; border-radius: 50%; animation: spin 1s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }

        @keyframes fadeInUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

    <div id="mainLoader">
        <div class="spinner mb-4"></div>
        <h1 class="text-white font-black tracking-[0.4em] italic uppercase animate__animated animate__pulse animate__infinite">GBTS ELITE</h1>
    </div>

    <div id="loginOverlay" class="fixed inset-0 bg-slate-950 z-[9999] flex items-center justify-center p-6 hidden">
        <div class="premium-card p-10 md:p-16 max-w-md w-full text-center animate__animated animate__zoomIn">
            <div class="w-24 h-24 bg-blue-900 text-white rounded-[35px] flex items-center justify-center mx-auto mb-8 text-4xl font-black italic shadow-2xl">GB</div>
            <h2 class="text-3xl font-black text-slate-900 mb-2 italic">CANDIDATE PORTAL</h2>
            <p class="text-[10px] font-black text-blue-500 mb-10 tracking-[0.4em] uppercase">Admission 2026 | Prime Solutions</p>
            
            <div class="space-y-4 mb-8">
                <input id="uName" type="text" placeholder="Enter Full Name" class="w-full p-6 bg-slate-100 rounded-[30px] font-bold text-center outline-none border-2 border-transparent focus:border-blue-900 transition-all text-lg">
            </div>

            <button onclick="handleLogin()" class="w-full bg-blue-900 text-white py-6 rounded-[30px] font-black text-xl shadow-xl hover:bg-blue-800 active:scale-95 transition-all">START TRAINING</button>
            <p class="mt-8 text-[9px] text-slate-400 font-bold uppercase tracking-widest">No Password Required - Instant Access</p>
        </div>
    </div>

    <nav class="bg-white/80 backdrop-blur-2xl sticky top-0 z-50 p-6 px-10 flex justify-between items-center border-b border-slate-200/50">
        <div class="flex items-center gap-4">
            <div class="w-12 h-12 bg-blue-900 text-white flex items-center justify-center rounded-2xl font-black italic text-xl shadow-lg">G</div>
            <div class="hidden md:block">
                <h1 class="font-black text-sm text-slate-900 uppercase italic leading-tight">GBTS Academy</h1>
                <p class="text-[8px] font-bold text-blue-500 uppercase tracking-widest">Learning Portal</p>
            </div>
        </div>
        <div class="flex gap-4 md:gap-8 items-center font-black text-[10px] uppercase italic">
            <button onclick="showTab('dashTab')" class="text-blue-900 hover:opacity-70 transition-opacity">Roadmap</button>
            <button onclick="showTab('rankTab')" class="text-slate-400 hover:text-blue-900 transition-all">Hall of Fame</button>
            <button onclick="logout()" class="bg-red-50 text-red-500 px-6 py-3 rounded-full hover:bg-red-100 transition-all">Sign Out</button>
        </div>
    </nav>

    <main class="max-w-7xl mx-auto p-6 md:p-10">
        
        <div id="dashTab" class="tab-content active-tab">
            <div class="welcome-hero p-10 md:p-20 text-white shadow-2xl mb-16 animate__animated animate__fadeIn">
                <div class="relative z-10 flex flex-col md:flex-row justify-between items-center gap-10">
                    <div class="text-center md:text-left">
                        <div class="inline-flex items-center gap-3 bg-white/10 px-5 py-2 rounded-full mb-6 border border-white/10">
                            <span class="w-2 h-2 bg-green-400 rounded-full animate-ping"></span>
                            <span class="text-[10px] font-black uppercase tracking-widest">Academic Session 2026</span>
                        </div>
                        <h2 id="dispName" class="text-5xl md:text-7xl font-black italic uppercase tracking-tighter mb-6 leading-none animate__animated animate__fadeInLeft">---</h2>
                        <div class="flex flex-wrap justify-center md:justify-start gap-4">
                            <div class="bg-white/10 backdrop-blur-xl p-5 rounded-3xl border border-white/10 min-w-[140px]">
                                <p class="text-[9px] font-bold opacity-60 uppercase mb-1">Rank Level</p>
                                <p class="text-3xl font-black italic" id="dispLvl">01</p>
                            </div>
                            <div class="bg-white/10 backdrop-blur-xl p-5 rounded-3xl border border-white/10 min-w-[140px]">
                                <p class="text-[9px] font-bold opacity-60 uppercase mb-1">Academy Status</p>
                                <p class="text-3xl font-black italic text-green-400">PRO</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="hero-circle w-96 h-96 -top-20 -right-20"></div>
                <div class="hero-circle w-64 h-64 -bottom-10 -left-10"></div>
            </div>

            <div class="flex items-center justify-between mb-10 px-4">
                <h3 class="text-2xl font-black uppercase italic text-slate-800 tracking-tighter">Learning Modules</h3>
                <div class="h-px flex-1 mx-8 bg-slate-200 hidden md:block"></div>
                <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest">12 Total Levels</p>
            </div>

            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-8"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-2xl mx-auto premium-card p-12 text-center animate__animated animate__fadeInUp">
                <div class="text-5xl mb-6">🏆</div>
                <h3 class="text-3xl font-black text-slate-900 mb-2 italic uppercase tracking-tighter">Hall of Fame</h3>
                <p class="text-[10px] font-black text-blue-500 mb-12 uppercase tracking-[0.3em]">Top 10 Performers</p>
                <div id="leaderList" class="space-y-4"></div>
            </div>
        </div>

    </main>

    <div id="quizModal" class="fixed inset-0 bg-slate-950/95 z-[10000] hidden items-center justify-center p-6 backdrop-blur-xl">
        <div class="premium-card p-12 max-w-lg w-full text-center shadow-2xl animate__animated animate__bounceIn">
            <h3 id="mTitle" class="text-3xl font-black text-blue-900 mb-6 italic uppercase tracking-tighter">Security Test</h3>
            <div id="mStudy" class="bg-blue-50 p-8 rounded-[40px] mb-8 text-sm italic text-blue-900 border border-blue-100 leading-relaxed shadow-inner"></div>
            <input id="ansInp" type="text" placeholder="Enter System Code" class="w-full p-6 bg-slate-100 rounded-[30px] mb-8 text-center font-black border-4 border-transparent focus:border-blue-900 outline-none transition-all text-xl">
            <button id="subBtn" class="w-full bg-blue-900 text-white py-6 rounded-[35px] font-black text-lg shadow-2xl hover:bg-blue-800 transition-all uppercase tracking-widest">Submit Response</button>
            <button onclick="closeModal()" class="mt-8 text-[10px] font-black text-slate-300 uppercase tracking-[0.2em] hover:text-slate-500 transition-colors">Abort Test</button>
        </div>
    </div>

    <script>
        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            databaseURL: "https://dark-web-9-default-rtdb.firebaseio.com/",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const db = firebase.database();

        let userData = null;

        // --- AUTH & AUTO-LOGIN ---
        window.onload = () => {
            setTimeout(() => {
                document.getElementById('mainLoader').style.opacity = '0';
                setTimeout(() => document.getElementById('mainLoader').style.display = 'none', 800);
                
                const savedUser = localStorage.getItem('gbts_premium_user');
                if(savedUser) processLogin(savedUser);
                else document.getElementById('loginOverlay').style.display = 'flex';
            }, 2500);
        };

        async function handleLogin() {
            const name = document.getElementById('uName').value.trim();
            if(!name || name.length < 3) return alert("Sweetie, apna mukammal naam likhein!");
            
            localStorage.setItem('gbts_premium_user', name);
            await processLogin(name);
        }

        async function processLogin(name) {
            // Safe Key based on Username
            const userKey = name.toLowerCase().replace(/\s/g, '_').substring(0, 20);
            try {
                const res = await auth.signInAnonymously();
                const userRef = db.ref('gbts_elite_v1/' + userKey);
                const snap = await userRef.get();

                if(!snap.exists()) {
                    await userRef.set({ uid: res.user.uid, name: name, level: 1, key: userKey });
                }
                
                document.getElementById('loginOverlay').style.display = 'none';
                initPortal(userKey);
            } catch (e) {
                alert("Connection Error! Check internet, sweetie.");
            }
        }

        function initPortal(key) {
            db.ref('gbts_elite_v1/' + key).on('value', snap => {
                userData = snap.val();
                if(userData) {
                    document.getElementById('dispName').innerText = userData.name;
                    document.getElementById('dispLvl').innerText = userData.level < 10 ? "0"+userData.level : userData.level;
                    renderMap();
                }
            });
            syncLeaderboard();
        }

        function renderMap() {
            const grid = document.getElementById('levelGrid'); grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `
                    <div onclick="openTest(${i}, ${lock})" class="premium-card level-card p-12 text-center ${lock ? 'locked' : 'shadow-sm animate__animated animate__fadeInUp'}" style="animation-delay: ${i*0.05}s">
                        <div class="text-5xl mb-5">${lock ? '🔒' : '🎖️'}</div>
                        <p class="text-[11px] font-black uppercase text-slate-400 tracking-tighter">Module ${i}</p>
                        ${!lock ? '<div class="absolute top-4 right-4 w-2 h-2 bg-blue-500 rounded-full animate-pulse"></div>' : ''}
                    </div>`;
            }
        }

        function openTest(num, lock) {
            if(lock || num !== userData.level) return;
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mTitle').innerText = "Module Assessment " + num;
            document.getElementById('mStudy').innerText = "Hello " + userData.name + "! Level " + (num + 1) + " unlock karne ke liye niche 'confirm' likhein.";
            
            document.getElementById('subBtn').onclick = () => {
                if(document.getElementById('ansInp').value.trim().toLowerCase() === 'confirm') {
                    confetti({ particleCount: 200, spread: 80, origin: { y: 0.6 } });
                    db.ref('gbts_elite_v1/' + userData.key).update({ level: userData.level + 1 });
                    db.ref('elite_ranks/' + userData.key).set({ name: userData.name, level: userData.level + 1 });
                    closeModal();
                } else { alert("Ghalat code! Dubara koshish karein."); }
            };
        }

        function syncLeaderboard() {
            db.ref('elite_ranks').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let ranks = []; snap.forEach(c => ranks.push(c.val()));
                ranks.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between items-center p-6 bg-slate-50 rounded-[35px] border border-slate-100 animate__animated animate__fadeInLeft">
                        <div class="flex items-center gap-4">
                            <span class="w-8 h-8 flex items-center justify-center bg-blue-900 text-white rounded-full text-[10px] font-black">#${i+1}</span>
                            <span class="font-black text-sm uppercase italic">${r.name}</span>
                        </div>
                        <span class="bg-blue-100 text-blue-900 px-5 py-2 rounded-full text-[10px] font-black italic">LEVEL ${r.level}</span>
                    </div>`;
                });
            });
        }

        function closeModal() { document.getElementById('quizModal').style.display = 'none'; document.getElementById('ansInp').value = ''; }
        function showTab(id) { document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab')); document.getElementById(id).classList.add('active-tab'); }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
