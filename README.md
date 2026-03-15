<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro | Prime Solutions Official</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f0f4f8; overflow-x: hidden; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeInUp 0.5s ease; }
        .glass-card { background: white; border-radius: 50px; box-shadow: 0 40px 70px -15px rgba(0,0,0,0.1); border: 1px solid rgba(255,255,255,0.5); }
        .level-card { transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); cursor: pointer; border-radius: 40px; background: white; border: 1px solid #e2e8f0; position: relative; }
        .level-card:hover:not(.locked) { transform: scale(1.08); box-shadow: 0 20px 40px rgba(30, 58, 138, 0.1); border-color: #1e3a8a; }
        .locked { opacity: 0.4; filter: grayscale(1); cursor: not-allowed; background: #f1f5f9; }
        #loader { position: fixed; inset: 0; background: #0f172a; z-index: 10000; display: flex; flex-direction: column; items: center; justify-content: center; transition: 0.5s; }
        .loader-circle { width: 60px; height: 60px; border: 6px solid #1e3a8a; border-top-color: #3b82f6; border-radius: 50%; animation: spin 1s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #1e3a8a; border-radius: 10px; }
    </style>
</head>
<body>

    <div id="loader">
        <div class="loader-circle mb-4"></div>
        <p class="text-white font-black tracking-[0.3em] uppercase italic animate__animated animate__pulse animate__infinite">GBTS Academy Live</p>
    </div>

    <audio id="successSfx" src="https://assets.mixkit.co/active_storage/sfx/2013/2013-preview.mp3"></audio>

    <div id="loginOverlay" class="fixed inset-0 bg-slate-900 z-[9998] flex items-center justify-center p-6 hidden">
        <div class="glass-card p-10 md:p-14 max-w-md w-full text-center animate__animated animate__zoomIn">
            <div class="w-20 h-20 bg-blue-900 text-white rounded-3xl flex items-center justify-center mx-auto mb-6 text-3xl font-black italic shadow-2xl">GB</div>
            <h2 class="text-3xl font-black text-slate-900 mb-2 italic uppercase">Portal Access</h2>
            <p class="text-[10px] font-black text-blue-500 mb-10 tracking-[0.4em] uppercase italic">Username & Password Required</p>

            <div class="space-y-4 mb-8">
                <input id="uName" type="text" placeholder="Username" class="w-full p-5 bg-slate-100 rounded-3xl font-bold text-center outline-none border-2 border-transparent focus:border-blue-900 transition-all">
                <input id="uPass" type="password" placeholder="Password" class="w-full p-5 bg-slate-100 rounded-3xl font-bold text-center outline-none border-2 border-transparent focus:border-blue-900 transition-all tracking-widest">
            </div>

            <button onclick="handleLogin()" class="w-full bg-blue-900 text-white py-5 rounded-[30px] font-black text-lg shadow-xl hover:bg-blue-800 transition-all">LOGIN SECURELY</button>
        </div>
    </div>

    <nav class="bg-white/90 backdrop-blur-xl sticky top-0 z-50 p-5 px-10 flex justify-between items-center border-b border-slate-200">
        <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-blue-900 text-white flex items-center justify-center rounded-xl font-black italic shadow-md">G</div>
            <h1 class="font-black text-sm text-slate-900 uppercase italic">GBTS Portal</h1>
        </div>
        <div class="flex gap-6 items-center text-[10px] font-black uppercase italic">
            <button onclick="showTab('dashTab')" class="text-blue-900">Training</button>
            <button onclick="showTab('rankTab')" class="text-slate-400">Merit List</button>
            <button onclick="logout()" class="text-red-500">Exit</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6 mt-6">
        <div id="dashTab" class="tab-content active-tab">
            <div class="bg-slate-950 p-12 rounded-[60px] text-white shadow-2xl mb-12 relative overflow-hidden animate__animated animate__fadeIn">
                <div class="relative z-10 grid md:grid-cols-2 gap-8 items-center">
                    <div>
                        <p class="text-[10px] font-black text-blue-400 mb-2 uppercase tracking-widest">Candidate Active</p>
                        <h2 id="dispName" class="text-5xl font-black italic uppercase tracking-tighter mb-4 leading-none">---</h2>
                        <span class="bg-white/10 px-6 py-2 rounded-2xl text-[10px] font-black uppercase border border-white/10">Rank: Level <span id="dispLvl">01</span></span>
                    </div>
                    <div class="bg-white/5 p-6 rounded-[35px] border border-white/10">
                        <p class="text-[10px] font-black mb-2 opacity-50 uppercase tracking-widest">Course Progress</p>
                        <div class="w-full bg-white/10 h-3 rounded-full overflow-hidden mb-3">
                            <div id="progBar" class="h-full bg-blue-500 transition-all duration-1000" style="width: 0%"></div>
                        </div>
                        <p class="text-[10px] font-bold"><span id="progPercent">0</span>% Completed</p>
                    </div>
                </div>
                <div class="absolute -right-20 -top-20 w-80 h-80 bg-blue-600/10 rounded-full blur-[100px]"></div>
            </div>
            
            <div id="levelGrid" class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-6 gap-8"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-md mx-auto bg-white p-12 rounded-[55px] shadow-sm border border-slate-100 animate__animated animate__fadeInUp">
                <h3 class="text-2xl font-black text-center text-blue-950 mb-10 italic uppercase tracking-tighter">Live Merit Rankings</h3>
                <div id="leaderList" class="space-y-4"></div>
            </div>
        </div>
    </main>

    <div id="quizModal" class="fixed inset-0 bg-slate-950/95 z-[10000] hidden items-center justify-center p-6 backdrop-blur-md">
        <div class="bg-white p-12 rounded-[60px] max-w-lg w-full text-center shadow-2xl animate__animated animate__bounceIn">
            <h3 id="mTitle" class="text-3xl font-black text-blue-950 mb-6 italic uppercase">Assesment</h3>
            <div id="mStudy" class="bg-blue-50 p-8 rounded-[40px] mb-8 text-sm italic text-blue-900 border border-blue-100 leading-relaxed shadow-inner"></div>
            <input id="ansInp" type="text" placeholder="Security Code" class="w-full p-6 bg-slate-50 rounded-3xl mb-6 text-center font-black border-2 border-transparent focus:border-blue-900 outline-none transition-all">
            <button id="subBtn" class="w-full bg-blue-950 text-white py-5 rounded-[30px] font-black shadow-xl hover:bg-blue-800">COMPLETE MODULE</button>
            <button onclick="closeModal()" class="mt-6 text-[10px] font-black text-slate-300 uppercase tracking-widest">Cancel</button>
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

        // --- CORE FUNCTIONS ---
        async function handleLogin() {
            const name = document.getElementById('uName').value.trim();
            const pass = document.getElementById('uPass').value.trim();
            if(!name || !pass) return alert("Please enter Username and Password, sweetie!");

            localStorage.setItem('gbts_user', name);
            localStorage.setItem('gbts_pass', pass);
            await authAndLoad(name, pass);
        }

        async function authAndLoad(name, pass) {
            const safePath = btoa(name.toLowerCase().replace(/\s/g, '') + pass).substring(0, 20);
            try {
                const res = await auth.signInAnonymously();
                const userRef = db.ref('gbts_live_v1/' + safePath);
                const snap = await userRef.get();

                if(!snap.exists()) {
                    await userRef.set({ uid: res.user.uid, name, level: 1, pass, path: safePath });
                }
                
                document.getElementById('loginOverlay').style.display = 'none';
                startAppSession(res.user.uid);
            } catch (e) {
                alert("Connection Error! Check if your Firebase is set to 'Public' rules, sweetie.");
            }
        }

        function startAppSession(uid) {
            db.ref('gbts_live_v1').orderByChild('uid').equalTo(uid).on('value', snap => {
                snap.forEach(c => {
                    userData = c.val();
                    document.getElementById('dispName').innerText = userData.name;
                    document.getElementById('dispLvl').innerText = userData.level;
                    const p = Math.round((userData.level / 12) * 100);
                    document.getElementById('progBar').style.width = p + "%";
                    document.getElementById('progPercent').innerText = p;
                    renderLevels();
                });
            });
            syncLeaderboard();
        }

        window.onload = () => {
            setTimeout(() => {
                document.getElementById('loader').style.opacity = '0';
                setTimeout(() => document.getElementById('loader').style.display = 'none', 500);
                
                const u = localStorage.getItem('gbts_user');
                const p = localStorage.getItem('gbts_pass');
                if(u && p) authAndLoad(u, p);
                else document.getElementById('loginOverlay').style.display = 'flex';
            }, 2000);
        };

        function renderLevels() {
            const grid = document.getElementById('levelGrid'); grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `
                    <div onclick="openTest(${i}, ${lock})" class="level-card p-12 text-center ${lock ? 'locked' : 'shadow-sm animate__animated animate__fadeInUp'}" style="animation-delay: ${i*0.05}s">
                        <div class="text-4xl mb-4">${lock ? '🔒' : '🎖️'}</div>
                        <p class="text-[10px] font-black uppercase text-slate-400">Level ${i}</p>
                    </div>
                `;
            }
        }

        function openTest(num, lock) {
            if(lock || num !== userData.level) return;
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mTitle').innerText = "Level " + num;
            document.getElementById('mStudy').innerText = "Welcome to your level " + num + " test! To proceed to the next rank, type 'prime' below.";
            
            document.getElementById('subBtn').onclick = () => {
                if(document.getElementById('ansInp').value.trim().toLowerCase() === 'prime') {
                    confetti({ particleCount: 200, spread: 80, origin: { y: 0.6 } });
                    document.getElementById('successSfx').play();
                    db.ref('gbts_live_v1/' + userData.path).update({ level: userData.level + 1 });
                    db.ref('ranks_live/' + userData.uid).set({ name: userData.name, level: userData.level + 1 });
                    closeModal();
                } else { alert("Incorrect password for this level!"); }
            };
        }

        function syncLeaderboard() {
            db.ref('ranks_live').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let data = []; snap.forEach(c => data.push(c.val()));
                data.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between items-center p-5 bg-slate-50 rounded-[30px] border border-slate-100 animate__animated animate__fadeInLeft">
                        <span class="font-black text-xs uppercase italic">#${i+1} ${r.name}</span>
                        <span class="bg-blue-900 text-white px-4 py-1 rounded-full text-[10px] font-black italic">Level ${r.level}</span>
                    </div>`;
                });
            });
        }

        function closeModal() { document.getElementById('quizModal').style.display = 'none'; document.getElementById('ansInp').value = ''; }
        function showTab(id) { document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab')); document.getElementById(id).classList.add('active-tab'); }
        function logout() { localStorage.clear(); auth.signOut().then(() => location.reload()); }
    </script>
</body>
</html>
