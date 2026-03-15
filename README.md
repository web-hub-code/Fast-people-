<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro Official | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f0f4f8; overflow-x: hidden; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .level-card { transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); cursor: pointer; border-radius: 35px; overflow: hidden; }
        .level-card:hover:not(.locked) { transform: scale(1.05); box-shadow: 0 20px 30px -10px rgba(0,0,0,0.1); }
        .locked { opacity: 0.5; filter: grayscale(1); cursor: not-allowed; background: #e2e8f0; }
        .quiz-modal { display: none; position: fixed; inset: 0; background: rgba(15, 23, 42, 0.98); z-index: 9999; align-items: center; justify-content: center; padding: 20px; }
        .progress-bar { transition: width 1s ease-in-out; }
    </style>
</head>
<body>

    <audio id="successSfx" src="https://assets.mixkit.co/active_storage/sfx/2013/2013-preview.mp3"></audio>
    <audio id="errorSfx" src="https://assets.mixkit.co/active_storage/sfx/2571/2571-preview.mp3"></audio>

    <div id="loginOverlay" class="fixed inset-0 bg-slate-900 z-[9999] flex items-center justify-center p-4 hidden">
        <div class="bg-white p-10 rounded-[50px] max-w-md w-full shadow-2xl text-center border-b-[10px] border-blue-900">
            <div class="w-16 h-16 bg-blue-900 text-white rounded-2xl flex items-center justify-center mx-auto mb-6 text-2xl font-black italic shadow-lg">GB</div>
            <h2 id="adminTitle" onclick="handleAdminTap()" class="text-3xl font-black text-slate-900 mb-2 uppercase select-none">GBTS Entrance</h2>
            <p class="text-[10px] font-bold text-blue-500 mb-8 uppercase tracking-widest">Official Academy Portal 2026</p>
            
            <div class="space-y-4 mb-8">
                <input id="uName" type="text" placeholder="Candidate Full Name" class="w-full p-5 bg-slate-50 rounded-3xl outline-none border-2 focus:border-blue-600 font-bold text-center">
                <input id="uPin" type="password" maxlength="4" placeholder="4-Digit Security PIN" class="w-full p-5 bg-slate-50 rounded-3xl outline-none border-2 focus:border-blue-600 font-bold tracking-widest text-center">
            </div>
            
            <button onclick="handleLogin()" class="w-full bg-blue-900 text-white py-5 rounded-[25px] font-black text-lg hover:bg-blue-800 transition active:scale-95 shadow-xl">VERIFY & ENTER</button>
            <p class="mt-6 text-[9px] text-slate-400 font-bold">Safe System: Your PIN is your account key.</p>
        </div>
    </div>

    <div id="quizModal" class="quiz-modal">
        <div class="bg-white p-10 rounded-[50px] max-w-lg w-full relative shadow-2xl">
            <div class="absolute -top-6 left-1/2 -translate-x-1/2 bg-red-600 text-white px-8 py-2 rounded-full font-black border-4 border-white shadow-xl">⏱️ <span id="timeSec">30</span>s</div>
            <h3 id="mTitle" class="text-2xl font-black text-blue-950 mb-4 italic">Test Mode</h3>
            <div id="mStudy" class="text-sm bg-blue-50 p-6 rounded-3xl mb-6 border border-blue-100 text-blue-900 italic leading-relaxed"></div>
            <div id="mQuestionArea"></div>
            <button onclick="closeQuiz()" class="mt-6 text-[10px] font-black text-slate-300 uppercase w-full">Cancel Examination</button>
        </div>
    </div>

    <nav class="bg-white/90 backdrop-blur-md sticky top-0 z-50 p-4 px-8 flex justify-between items-center border-b border-slate-200">
        <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-blue-900 text-white flex items-center justify-center rounded-xl font-black shadow-md">G</div>
            <h1 class="font-black text-sm text-slate-900 uppercase">GBTS Academy</h1>
        </div>
        <div class="flex gap-5 font-black uppercase text-[10px]">
            <button onclick="showTab('dashTab')" class="text-blue-900">Training</button>
            <button onclick="showTab('rankTab')" class="text-slate-400">Merit</button>
            <button onclick="showTab('idTab')" class="text-slate-400">ID Card</button>
            <button onclick="logout()" class="text-red-500">Exit</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6">
        <div id="dashTab" class="tab-content active-tab">
            <div class="bg-slate-900 p-10 rounded-[50px] text-white shadow-2xl mb-12 relative overflow-hidden">
                <div class="relative z-10 grid md:grid-cols-2 gap-10">
                    <div>
                        <p class="text-[10px] font-black text-blue-400 mb-2 uppercase">Verified Candidate</p>
                        <h2 id="dispName" class="text-4xl font-black italic uppercase mb-6 leading-none">---</h2>
                        <span class="bg-white/10 px-5 py-2 rounded-xl text-xs font-bold border border-white/20">Current Level: <span id="dispLvl">01</span></span>
                    </div>
                    <div class="bg-white/5 p-6 rounded-3xl border border-white/10">
                        <p class="text-[10px] font-black mb-2 opacity-50 uppercase">Completion Stats</p>
                        <div class="w-full bg-white/10 h-3 rounded-full overflow-hidden mb-2">
                            <div id="progBar" class="progress-bar h-full bg-blue-500" style="width: 0%"></div>
                        </div>
                        <p class="text-[10px] font-bold"><span id="progText">0</span>% Path Completed</p>
                    </div>
                </div>
            </div>
            <div id="levelGrid" class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-6 gap-6"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-xl mx-auto bg-white p-10 rounded-[50px] shadow-lg">
                <h3 class="text-2xl font-black text-center text-blue-950 mb-8 italic uppercase">Academy Merit List</h3>
                <div id="leaderList" class="space-y-4"></div>
            </div>
        </div>

        <div id="idTab" class="tab-content">
            <div class="max-w-sm mx-auto bg-white p-10 rounded-[60px] shadow-2xl text-center border relative">
                <div class="w-24 h-24 bg-blue-900 rounded-full mx-auto mb-6 flex items-center justify-center text-4xl text-white italic border-4 border-white shadow-xl">👤</div>
                <h3 id="cardName" class="text-3xl font-black text-slate-900 mb-2 italic">---</h3>
                <p class="text-[10px] font-black text-blue-500 mb-8 uppercase tracking-widest">Official Recruitment Pass</p>
                <div class="bg-slate-50 p-4 rounded-3xl">
                    <p class="text-[8px] font-black opacity-30 uppercase">System Identity</p>
                    <p id="cardId" class="text-xs font-black">#UID-000000</p>
                </div>
            </div>
        </div>
    </main>

    <footer class="p-12 text-center text-[10px] font-black text-slate-300 uppercase tracking-widest">
        Powered by Prime Solutions Technology
    </footer>

    <script>
        // FIREBASE CONFIGURATION
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

        let userData = { name: "", level: 1, pin: "" };
        let timer = null, taps = 0;
        const successSfx = document.getElementById('successSfx'), errorSfx = document.getElementById('errorSfx');

        // Study Material Data
        const material = {
            1: { s: "GK: Pakistan was founded in 1947.", q: "In which year was Pakistan founded?", a: "1947" },
            2: { s: "Computer: CPU stands for Central Processing Unit.", q: "What does CPU stand for?", a: "Central Processing Unit" },
            3: { s: "Math: A triangle has 3 sides.", q: "How many sides does a triangle have?", a: "3" }
        };

        // ADMIN SECRET TAP
        function handleAdminTap() {
            taps++;
            if(taps >= 5) {
                let p = prompt("Admin Authorization Code:");
                if(p === "prime786") alert("Access Authorized.");
                taps = 0;
            }
        }

        // AUTH WATCHER
        auth.onAuthStateChanged(user => {
            if(user) {
                document.getElementById('loginOverlay').classList.add('hidden');
                loadData(user.uid);
            } else {
                document.getElementById('loginOverlay').classList.remove('hidden');
            }
        });

        // SECURE LOGIN SYSTEM
        async function handleLogin() {
            const name = document.getElementById('uName').value.trim();
            const pin = document.getElementById('uPin').value.trim();
            if(!name || pin.length < 4) return alert("Sweetie, enter Name & 4-Digit PIN!");

            const path = btoa(name.toLowerCase().replace(/\s/g, '') + pin);
            
            try {
                // Check if user exists in DB first
                const snap = await db.ref('gbts_trusted/' + path).get();
                
                auth.signInAnonymously().then(res => {
                    if(!snap.exists()) {
                        db.ref('gbts_trusted/' + path).set({ 
                            uid: res.user.uid, name, level: 1, pin, path 
                        }).then(() => location.reload());
                    } else {
                        location.reload();
                    }
                }).catch(err => alert("Auth Error: Enable Anonymous Auth in Firebase Console!"));
            } catch (e) {
                alert("Database Error: Check your Firebase Rules!");
            }
        }

        function loadData(uid) {
            db.ref('gbts_trusted').orderByChild('uid').equalTo(uid).on('value', snap => {
                snap.forEach(c => {
                    userData = c.val();
                    updateUI(userData);
                });
            });
            loadLeaderboard();
        }

        function updateUI(data) {
            document.getElementById('dispName').innerText = data.name;
            document.getElementById('dispLvl').innerText = data.level;
            document.getElementById('cardName').innerText = data.name;
            document.getElementById('cardId').innerText = "#" + data.uid.substring(0, 10).toUpperCase();
            
            const p = Math.round((data.level / 12) * 100);
            document.getElementById('progBar').style.width = p + "%";
            document.getElementById('progText').innerText = p;
            renderLevels();
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `
                    <div onclick="openTest(${i}, ${lock})" class="level-card p-10 bg-white border-2 border-slate-50 text-center ${lock ? 'locked' : 'shadow-sm'}">
                        <div class="text-4xl mb-4">${lock ? '🔒' : '🎖️'}</div>
                        <p class="text-[10px] font-black uppercase text-slate-400">Module ${i}</p>
                    </div>
                `;
            }
        }

        function openTest(num, lock) {
            if(lock) return errorSfx.play();
            if(num !== userData.level) return alert("Pehle पिछला level complete karein!");

            const d = material[num] || { s: "Standard Academy Assessment. Focus and reply.", q: "Type 'ready' to pass.", a: "ready" };
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mTitle').innerText = "Module " + num;
            document.getElementById('mStudy').innerText = d.s;
            document.getElementById('mQuestionArea').innerHTML = `
                <p class="text-center mb-6 font-bold text-slate-700">${d.q}</p>
                <input id="ansInp" type="text" placeholder="Your Answer" class="w-full p-5 bg-slate-50 rounded-3xl outline-none border-2 focus:border-blue-600 font-black text-center mb-4">
                <button onclick="checkAns(${num}, '${d.a}')" class="w-full bg-blue-900 text-white py-5 rounded-3xl font-black shadow-xl hover:bg-blue-800 transition">SUBMIT</button>
            `;
            startTimer();
        }

        function startTimer() {
            let s = 30; document.getElementById('timeSec').innerText = s;
            clearInterval(timer);
            timer = setInterval(() => {
                s--; document.getElementById('timeSec').innerText = s;
                if(s <= 0) { clearInterval(timer); alert("TIME OUT!"); closeQuiz(); }
            }, 1000);
        }

        function checkAns(num, correct) {
            const val = document.getElementById('ansInp').value.trim().toLowerCase();
            if(val === correct.toLowerCase()) {
                clearInterval(timer); successSfx.play();
                confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
                
                db.ref('gbts_trusted/' + userData.path).update({ level: userData.level + 1 });
                db.ref('gbts_ranks/' + auth.currentUser.uid).set({ name: userData.name, level: userData.level + 1 });
                
                setTimeout(() => { alert("Passed! Module " + num + " Complete."); closeQuiz(); }, 500);
            } else { errorSfx.play(); alert("Wrong answer sweetie! Try again."); }
        }

        function loadLeaderboard() {
            db.ref('gbts_ranks').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let ranks = []; snap.forEach(c => ranks.push(c.val()));
                ranks.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between p-5 bg-slate-50 rounded-[30px] border">
                        <span class="font-black text-sm uppercase italic">#${i+1} ${r.name}</span>
                        <span class="text-[10px] font-bold bg-blue-900 text-white px-4 py-1 rounded-full">LVL ${r.level}</span>
                    </div>`;
                });
            });
        }

        function closeQuiz() { clearInterval(timer); document.getElementById('quizModal').style.display = 'none'; }
        function showTab(id) { document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab')); document.getElementById(id).classList.add('active-tab'); }
        function logout() { auth.signOut().then(() => location.reload()); }
    </script>
</body>
</html>
