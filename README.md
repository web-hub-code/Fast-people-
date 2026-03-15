<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro Elite | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; overflow-x: hidden; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: slideUp 0.4s ease; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; } }
        .badge-card { background: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%); color: white; border-radius: 40px; }
        .level-node { transition: 0.3s; cursor: pointer; border-radius: 35px; border: 2px solid transparent; position: relative; }
        .level-node:hover:not(.locked) { transform: scale(1.05); border-color: #3b82f6; background: white; }
        .locked { opacity: 0.4; cursor: not-allowed; filter: grayscale(1); background: #e2e8f0; }
        .quiz-modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.9); z-index: 9999; align-items: center; justify-content: center; padding: 20px; }
    </style>
</head>
<body id="mainBody">

    <audio id="successSound" src="https://assets.mixkit.co/active_storage/sfx/2013/2013-preview.mp3" preload="auto"></audio>
    <audio id="errorSound" src="https://assets.mixkit.co/active_storage/sfx/2571/2571-preview.mp3" preload="auto"></audio>

    <div id="quizModal" class="quiz-modal">
        <div class="bg-white p-8 rounded-[40px] max-w-lg w-full shadow-2xl relative">
            <div id="timer" class="absolute -top-6 left-1/2 -translate-x-1/2 bg-red-600 text-white px-8 py-2 rounded-full font-black shadow-xl border-4 border-white">
                ⏱️ <span id="timeSec">30</span>s
            </div>
            <h3 id="mTitle" class="text-2xl font-black text-blue-900 mb-4 uppercase italic">Level Test</h3>
            <div id="mStudy" class="text-sm bg-blue-50 p-4 rounded-2xl mb-6 border border-blue-100 text-blue-800 italic"></div>
            <div id="mQuestion" class="space-y-4"></div>
            <button onclick="closeQuiz()" class="mt-4 text-[10px] font-black text-gray-400 uppercase w-full">Cancel Test</button>
        </div>
    </div>

    <div id="loginOverlay" class="fixed inset-0 bg-slate-950 z-[9999] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-12 rounded-[60px] max-w-md w-full text-center shadow-2xl">
            <h2 id="adminHeader" onclick="handleAdminTap()" class="text-4xl font-black text-blue-950 mb-2 italic tracking-tighter cursor-pointer select-none">GBTS ENTRANCE</h2>
            <p class="text-[10px] font-black text-blue-400 mb-8 tracking-widest uppercase italic">Secure Recruitment Portal</p>
            <input id="uName" type="text" placeholder="Full Name" class="w-full p-5 bg-slate-50 rounded-3xl mb-3 outline-none border-2 border-transparent focus:border-blue-600 text-center font-black">
            <input id="uPin" type="password" maxlength="4" placeholder="4-Digit PIN" class="w-full p-5 bg-slate-50 rounded-3xl mb-8 outline-none border-2 border-transparent focus:border-blue-600 text-center font-black tracking-widest">
            <button onclick="handleLogin()" class="w-full bg-blue-900 text-white py-5 rounded-[30px] font-black text-xl hover:bg-blue-800 transition shadow-xl">START TRAINING</button>
        </div>
    </div>

    <nav class="bg-white/80 backdrop-blur-xl sticky top-0 z-50 p-5 flex justify-between items-center px-10 border-b">
        <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-blue-900 text-white flex items-center justify-center rounded-2xl font-black">G</div>
            <h1 class="font-black text-xl italic text-blue-950">GBTS ELITE</h1>
        </div>
        <div class="flex gap-6 items-center font-black uppercase text-[10px]">
            <button onclick="showTab('dashTab')" class="text-blue-900">Portal</button>
            <button onclick="showTab('rankTab')" class="text-slate-400">Ranks</button>
            <button onclick="showTab('idTab')" class="text-slate-400">ID Card</button>
            <button onclick="logout()" class="text-red-500">Exit</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6 mt-4">
        <div id="dashTab" class="tab-content active-tab">
            <div class="badge-card p-12 mb-12 flex justify-between items-end relative overflow-hidden">
                <div class="relative z-10">
                    <h2 id="dispName" class="text-5xl font-black italic uppercase tracking-tighter mb-2 leading-none">---</h2>
                    <div class="flex gap-3 mt-4">
                        <a href="#" class="bg-yellow-400 text-black px-4 py-2 rounded-xl font-black text-[10px] uppercase">📥 Syllabus PDF</a>
                        <span class="bg-white/20 px-4 py-2 rounded-xl text-[10px] font-black uppercase">Level: <span id="dispLvl">01</span></span>
                    </div>
                </div>
                <div class="absolute -right-16 -top-16 w-80 h-80 bg-white/10 rounded-full blur-[80px]"></div>
            </div>
            <h3 class="text-2xl font-black italic uppercase mb-8">Training Modules</h3>
            <div id="levelGrid" class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-6 gap-6"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="bg-white rounded-[40px] p-8 shadow-sm">
                <h3 class="text-2xl font-black text-blue-900 mb-6 uppercase text-center italic">Merit List</h3>
                <div id="leaderList" class="space-y-3"></div>
            </div>
        </div>

        <div id="idTab" class="tab-content">
            <div class="max-w-md mx-auto bg-white p-10 rounded-[50px] shadow-2xl text-center border">
                <div class="w-20 h-20 bg-blue-900 text-white rounded-full mx-auto mb-6 flex items-center justify-center text-3xl font-black italic">GB</div>
                <h3 id="cardName" class="text-3xl font-black text-blue-950 uppercase italic tracking-tighter">---</h3>
                <p class="text-[10px] font-black text-slate-400 mb-8 uppercase tracking-widest">Candidate Pass 2026</p>
                <div class="bg-slate-50 p-4 rounded-3xl">
                    <p class="text-[8px] font-black opacity-50 uppercase">System Identity</p>
                    <p id="cardId" class="text-sm font-black">#UID-0000</p>
                </div>
            </div>
        </div>
    </main>

    <footer class="p-10 text-center">
        <p class="text-[10px] font-black text-slate-300 uppercase tracking-widest">Designed & Developed by Prime Solutions</p>
    </footer>

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

        let taps = 0, timer = null, userData = { name: "", level: 1, pin: "" };
        const successSfx = document.getElementById('successSound'), errorSfx = document.getElementById('errorSound');

        const modules = {
            1: { s: "GK: Pakistan's capital is Islamabad.", q: "What is the Capital of Pakistan?", a: "Islamabad" },
            2: { s: "Eng: 'Fast' antonym is 'Slow'.", q: "Opposite of Fast?", a: "Slow" },
            3: { s: "Math: 12 x 12 is 144.", q: "What is 12 x 12?", a: "144" }
        };

        function handleAdminTap() {
            taps++;
            if(taps >= 5) {
                let p = prompt("Admin Key:");
                if(p === "prime786") alert("Admin Access Verified.");
                taps = 0;
            }
        }

        auth.onAuthStateChanged(user => {
            if(user) {
                document.getElementById('loginOverlay').classList.add('hidden');
                loadUserData(user.uid);
            } else {
                document.getElementById('loginOverlay').classList.remove('hidden');
            }
        });

        async function handleLogin() {
            const name = document.getElementById('uName').value.trim();
            const pin = document.getElementById('uPin').value.trim();
            if(!name || pin.length < 4) return alert("Enter Name & 4-Digit PIN!");
            const path = name.toLowerCase().replace(/\s/g, '') + "_" + pin;
            const snap = await db.ref('gbts_final/' + path).get();
            auth.signInAnonymously().then(res => {
                if(!snap.exists()) db.ref('gbts_final/' + path).set({ uid: res.user.uid, name, level: 1, pin });
                location.reload();
            });
        }

        function loadUserData(uid) {
            db.ref('gbts_final').orderByChild('uid').equalTo(uid).on('value', snap => {
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
            document.getElementById('cardId').innerText = "#" + data.uid.substring(0, 8).toUpperCase();
            renderLevels();
        }

        function renderLevels() {
            const grid = document.getElementById('levelGrid');
            grid.innerHTML = '';
            for(let i=1; i<=12; i++) {
                let lock = i > userData.level;
                grid.innerHTML += `<div onclick="startTest(${i}, ${lock})" class="level-node p-10 bg-white text-center border-2 border-slate-50 ${lock ? 'locked' : 'shadow-sm'}">
                    <div class="text-3xl mb-3">${lock ? '🔒' : '🎖️'}</div>
                    <p class="text-[10px] font-black uppercase">Module ${i}</p>
                </div>`;
            }
        }

        function startTest(num, lock) {
            if(lock) return errorSfx.play();
            if(num !== userData.level) return alert("Pehle pichla level mukammal karein!");
            const d = modules[num] || { s: "General Knowledge Review.", q: "Type 'pass' to continue.", a: "pass" };
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mTitle').innerText = "Module " + num;
            document.getElementById('mStudy').innerHTML = d.s;
            document.getElementById('mQuestion').innerHTML = `<p class="font-bold mb-4">${d.q}</p>
                <input id="ansInp" type="text" class="w-full p-4 bg-gray-100 rounded-2xl outline-none font-black text-center border-2 focus:border-blue-900">
                <button onclick="checkAns(${num}, '${d.a}')" class="w-full bg-blue-900 text-white py-4 mt-4 rounded-2xl font-black">SUBMIT</button>`;
            startTimer();
        }

        function startTimer() {
            let s = 30; document.getElementById('timeSec').innerText = s;
            clearInterval(timer);
            timer = setInterval(() => {
                s--; document.getElementById('timeSec').innerText = s;
                if(s <= 0) { clearInterval(timer); alert("Time Up! Try again."); closeQuiz(); }
            }, 1000);
        }

        function checkAns(num, correct) {
            const val = document.getElementById('ansInp').value.trim().toLowerCase();
            if(val === correct.toLowerCase()) {
                clearInterval(timer); successSfx.play();
                confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
                const path = userData.name.toLowerCase().replace(/\s/g, '') + "_" + userData.pin;
                db.ref('gbts_final/' + path).update({ level: userData.level + 1 });
                db.ref('leaderboard/' + auth.currentUser.uid).set({ name: userData.name, level: userData.level + 1 });
                closeQuiz();
            } else { alert("Wrong Answer! Try again."); }
        }

        function loadLeaderboard() {
            db.ref('leaderboard').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let ranks = []; snap.forEach(c => ranks.push(c.val()));
                ranks.reverse().forEach((r, i) => {
                    list.innerHTML += `<div class="flex justify-between p-4 bg-slate-50 rounded-2xl border">
                        <span class="font-black text-sm uppercase">#${i+1} ${r.name}</span>
                        <span class="text-[10px] font-bold bg-blue-900 text-white px-3 py-1 rounded-full">LVL ${r.level}</span>
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
