<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Elite | Prime Solutions Master Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; color: #0f172a; overflow-x: hidden; }
        .premium-card { background: rgba(255, 255, 255, 0.9); backdrop-blur: 15px; border: 1px solid rgba(255,255,255,0.6); border-radius: 45px; box-shadow: 0 30px 60px -12px rgba(0,0,0,0.08); }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeInUp 0.7s ease both; }
        .level-card { transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275); cursor: pointer; border-radius: 40px; background: white; border: 1px solid #e2e8f0; position: relative; }
        .level-card:hover:not(.locked) { transform: translateY(-12px) scale(1.06); border-color: #1e3a8a; box-shadow: 0 20px 40px rgba(30,58,138,0.15); }
        .locked { filter: grayscale(1); opacity: 0.4; cursor: not-allowed; background: #f1f5f9 !important; }
        .welcome-hero { background: linear-gradient(135deg, #0f172a 0%, #1e3a8a 100%); border-radius: 65px; position: relative; overflow: hidden; }
        #mainLoader { position: fixed; inset: 0; background: #0f172a; z-index: 100000; display: flex; flex-direction: column; items: center; justify-content: center; }
        @keyframes fadeInUp { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }
        .qr-container canvas { margin: 0 auto; border-radius: 20px; }
    </style>
</head>
<body>

    <div id="mainLoader">
        <div class="w-16 h-16 border-4 border-blue-900 border-t-blue-400 rounded-full animate-spin mb-6"></div>
        <h1 class="text-white font-black tracking-[0.5em] italic animate__animated animate__pulse animate__infinite">GBTS ELITE</h1>
    </div>

    <div id="loginOverlay" class="fixed inset-0 bg-slate-950 z-[99999] flex items-center justify-center p-6 hidden">
        <div class="premium-card p-12 max-w-md w-full text-center animate__animated animate__zoomIn">
            <div class="w-24 h-24 bg-blue-900 text-white rounded-[35px] flex items-center justify-center mx-auto mb-8 text-4xl font-black italic shadow-2xl">GB</div>
            <h2 class="text-3xl font-black text-slate-900 mb-2 italic uppercase">Candidate Portal</h2>
            <p class="text-[10px] font-black text-blue-500 mb-10 tracking-[0.4em] uppercase">Admission 2026 | Prime Solutions</p>
            <input id="uName" type="text" placeholder="Full Name" class="w-full p-6 bg-slate-100 rounded-[30px] font-bold text-center outline-none border-2 border-transparent focus:border-blue-900 transition-all mb-8 text-lg">
            <button onclick="handleLogin()" class="w-full bg-blue-900 text-white py-6 rounded-[30px] font-black text-xl shadow-2xl hover:bg-blue-800 active:scale-95 transition-all">ENTER ACADEMY</button>
        </div>
    </div>

    <nav class="bg-white/80 backdrop-blur-2xl sticky top-0 z-50 p-6 px-10 flex justify-between items-center border-b border-slate-200/50">
        <div class="flex items-center gap-4">
            <div class="w-12 h-12 bg-blue-900 text-white flex items-center justify-center rounded-2xl font-black italic text-xl shadow-lg">G</div>
            <h1 class="font-black text-sm uppercase italic hidden md:block">GBTS Elite Portal</h1>
        </div>
        <div class="flex gap-6 md:gap-10 font-black text-[10px] uppercase italic">
            <button onclick="showTab('dashTab')" class="text-blue-900">Training</button>
            <button onclick="showTab('rankTab')" class="text-slate-400">Merit List</button>
            <button onclick="showTab('qrTab')" class="text-slate-400">Share QR</button>
            <button onclick="logout()" class="text-red-500 bg-red-50 px-5 py-2 rounded-full">Exit</button>
        </div>
    </nav>

    <main class="max-w-7xl mx-auto p-6 md:p-10">
        
        <div id="dashTab" class="tab-content active-tab">
            <div class="welcome-hero p-12 md:p-20 text-white shadow-2xl mb-16 animate__animated animate__fadeIn" onclick="checkAdminTrigger()">
                <p class="text-[10px] font-black text-blue-400 mb-3 uppercase tracking-widest">Candidate Identity Active</p>
                <h2 id="dispName" class="text-5xl md:text-7xl font-black italic uppercase tracking-tighter mb-8 leading-none">---</h2>
                <div class="inline-flex items-center gap-4 bg-white/10 px-8 py-4 rounded-3xl border border-white/10 backdrop-blur-md">
                    <span class="text-[12px] font-black uppercase italic">Current Rank: Level <span id="dispLvl">01</span></span>
                </div>
            </div>
            <div id="levelGrid" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-10"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-2xl mx-auto premium-card p-12 text-center">
                <div class="text-5xl mb-6">🏆</div>
                <h3 class="text-3xl font-black mb-10 italic uppercase tracking-tight text-slate-900">Top Performers List</h3>
                <div id="leaderList" class="space-y-4"></div>
            </div>
        </div>

        <div id="qrTab" class="tab-content">
            <div class="max-w-md mx-auto premium-card p-12 text-center">
                <h3 class="text-2xl font-black mb-6 italic uppercase">Share Academy</h3>
                <div id="qrcode" class="qr-container bg-white p-6 rounded-[35px] mb-8 shadow-inner flex justify-center"></div>
                <p class="text-[10px] font-black text-slate-400 uppercase leading-relaxed">Scan this QR to join the GBTS Elite Portal instantly.</p>
            </div>
        </div>

        <div id="adminTab" class="tab-content">
            <div class="premium-card p-12 overflow-hidden">
                <div class="flex justify-between items-center mb-12">
                    <h3 class="text-3xl font-black text-red-600 italic uppercase">Admin Control Room</h3>
                    <button onclick="showTab('dashTab')" class="bg-slate-900 text-white px-6 py-2 rounded-full text-[10px] font-black">CLOSE ADMIN</button>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left">
                        <thead>
                            <tr class="text-[10px] font-black uppercase text-slate-400 border-b border-slate-100">
                                <th class="pb-6">Candidate</th>
                                <th class="pb-6">Rank</th>
                                <th class="pb-6 text-right">Commands</th>
                            </tr>
                        </thead>
                        <tbody id="adminUserList" class="font-bold text-slate-700"></tbody>
                    </table>
                </div>
            </div>
        </div>

    </main>

    <div id="quizModal" class="fixed inset-0 bg-slate-950/95 z-[100000] hidden items-center justify-center p-6 backdrop-blur-xl">
        <div class="premium-card p-12 max-w-lg w-full text-center shadow-2xl animate__animated animate__bounceIn">
            <h3 id="mTitle" class="text-3xl font-black text-blue-900 mb-6 italic uppercase">Assessment</h3>
            <div id="mStudy" class="bg-blue-50 p-8 rounded-[40px] mb-8 text-sm italic text-blue-900 border border-blue-100 leading-relaxed"></div>
            <input id="ansInp" type="text" placeholder="Security Code" class="w-full p-6 bg-slate-100 rounded-[35px] mb-8 text-center font-black border-4 border-transparent focus:border-blue-900 outline-none transition-all text-xl">
            <button id="subBtn" class="w-full bg-blue-900 text-white py-6 rounded-[35px] font-black text-lg shadow-2xl">VERIFY & UNLOCK</button>
            <button onclick="closeModal()" class="mt-8 text-[10px] font-black text-slate-300 uppercase underline">Cancel</button>
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
        let adminClicks = 0;

        // --- CORE INITIALIZATION ---
        window.onload = () => {
            // Generate QR Code for Current Link
            new QRCode(document.getElementById("qrcode"), { text: window.location.href, width: 180, height: 180, colorDark : "#0f172a", colorLight : "#ffffff" });

            setTimeout(() => {
                document.getElementById('mainLoader').style.opacity = '0';
                setTimeout(() => document.getElementById('mainLoader').style.display = 'none', 800);
                
                const savedUser = localStorage.getItem('gbts_elite_v4');
                if(savedUser) processLogin(savedUser);
                else document.getElementById('loginOverlay').style.display = 'flex';
            }, 2000);
        };

        async function handleLogin() {
            const name = document.getElementById('uName').value.trim();
            if(!name || name.length < 3) return alert("Sweetie, apna naam likhein!");
            localStorage.setItem('gbts_elite_v4', name);
            processLogin(name);
        }

        async function processLogin(name) {
            const userKey = name.toLowerCase().replace(/\s/g, '_').substring(0, 20);
            try {
                const res = await auth.signInAnonymously();
                const userRef = db.ref('gbts_master_v1/' + userKey);
                const snap = await userRef.get();
                if(!snap.exists()) {
                    await userRef.set({ uid: res.user.uid, name: name, level: 1, key: userKey });
                }
                document.getElementById('loginOverlay').style.display = 'none';
                initPortal(userKey);
            } catch (e) { alert("Database Error! Connection lost."); }
        }

        function initPortal(key) {
            db.ref('gbts_master_v1/' + key).on('value', snap => {
                userData = snap.val();
                if(userData) {
                    document.getElementById('dispName').innerText = userData.name;
                    document.getElementById('dispLvl').innerText = userData.level;
                    renderLevels();
                }
            });
            syncRankings();
            loadAdminPanel();
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid'); grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `<div onclick="openTest(${i}, ${lock})" class="level-card p-12 text-center ${lock ? 'locked' : 'shadow-sm animate__animated animate__fadeInUp'}">
                    <div class="text-5xl mb-5">${lock ? '🔒' : '🎖️'}</div>
                    <p class="text-[10px] font-black uppercase text-slate-400 tracking-tighter">Module ${i}</p>
                </div>`;
            }
        }

        function openTest(num, lock) {
            if(lock || num !== userData.level) return;
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mStudy').innerText = "Hello " + userData.name + "! To reach Level " + (num+1) + ", type 'prime' in the box below.";
            document.getElementById('subBtn').onclick = () => {
                if(document.getElementById('ansInp').value.trim().toLowerCase() === 'prime') {
                    confetti({ particleCount: 250, spread: 90, origin: { y: 0.6 } });
                    db.ref('gbts_master_v1/' + userData.key).update({ level: userData.level + 1 });
                    db.ref('merit_master_v1/' + userData.key).set({ name: userData.name, level: userData.level + 1 });
                    closeModal();
                } else { alert("Wrong verification code!"); }
            };
        }

        function syncRankings() {
            db.ref('merit_master_v1').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let ranks = []; snap.forEach(c => ranks.push(c.val()));
                ranks.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between items-center p-6 bg-slate-50 rounded-[35px] border border-slate-100 animate__animated animate__fadeInLeft">
                        <span class="font-black text-xs uppercase italic">#${i+1} ${r.name}</span>
                        <span class="bg-blue-900 text-white px-5 py-2 rounded-full text-[10px] font-black italic">LEVEL ${r.level}</span>
                    </div>`;
                });
            });
        }

        // --- SECRET ADMIN LOGIC ---
        function checkAdminTrigger() {
            adminClicks++;
            if(adminClicks >= 5) {
                const pass = prompt("Enter Admin Secret Password, Sweetie:");
                if(pass === "prime786") showTab('adminTab');
                else alert("Access Denied!");
                adminClicks = 0;
            }
        }

        function loadAdminPanel() {
            db.ref('gbts_master_v1').on('value', snap => {
                const list = document.getElementById('adminUserList'); list.innerHTML = '';
                snap.forEach(c => {
                    const u = c.val();
                    list.innerHTML += `<tr class="border-b border-slate-50">
                        <td class="py-6 uppercase">${u.name}</td>
                        <td class="py-6 font-black text-blue-900 italic">Level ${u.level}</td>
                        <td class="py-6 text-right space-x-4">
                            <button onclick="editUser('${u.key}', ${u.level})" class="text-blue-500 text-xs font-black uppercase underline">Update</button>
                            <button onclick="delUser('${u.key}')" class="text-red-500 text-xs font-black uppercase underline">Remove</button>
                        </td>
                    </tr>`;
                });
            });
        }

        function editUser(key, l) {
            const nl = prompt("Update Level to:", l);
            if(nl) {
                db.ref('gbts_master_v1/' + key).update({ level: parseInt(nl) });
                db.ref('merit_master_v1/' + key).update({ level: parseInt(nl) });
            }
        }

        function delUser(key) {
            if(confirm("Confirm Delete Candidate?")) {
                db.ref('gbts_master_v1/' + key).remove();
                db.ref('merit_master_v1/' + key).remove();
            }
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }

        function closeModal() { document.getElementById('quizModal').style.display = 'none'; document.getElementById('ansInp').value = ''; }
        function logout() { localStorage.clear(); location.reload(); }
    </script>
</body>
</html>
