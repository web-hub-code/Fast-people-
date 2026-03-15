<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Official Portal | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f0f4f8; color: #1e293b; }
        .tab-content { display: none; }
        .active-tab { display: block; animation: fadeIn 0.5s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .glass { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.2); }
        .level-card { transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); cursor: pointer; border-radius: 30px; position: relative; overflow: hidden; }
        .level-card:hover:not(.locked) { transform: translateY(-5px); box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1); }
        .locked { opacity: 0.5; filter: grayscale(1); cursor: not-allowed; }
        .progress-fill { transition: width 1s ease-in-out; }
        .quiz-modal { display: none; position: fixed; inset: 0; background: rgba(15, 23, 42, 0.95); z-index: 9999; align-items: center; justify-content: center; padding: 20px; }
    </style>
</head>
<body>

    <audio id="successSfx" src="https://assets.mixkit.co/active_storage/sfx/2013/2013-preview.mp3"></audio>
    <audio id="errorSfx" src="https://assets.mixkit.co/active_storage/sfx/2571/2571-preview.mp3"></audio>

    <div id="loginOverlay" class="fixed inset-0 bg-slate-900 z-[9999] flex items-center justify-center p-4 hidden">
        <div class="bg-white p-8 md:p-12 rounded-[50px] max-w-md w-full shadow-2xl text-center border-b-[8px] border-blue-900">
            <div class="w-16 h-16 bg-blue-900 text-white rounded-2xl flex items-center justify-center mx-auto mb-6 text-2xl font-black italic shadow-lg">GB</div>
            <h2 id="adminHeader" onclick="secretAdminTap()" class="text-3xl font-black text-slate-900 mb-2 uppercase tracking-tighter cursor-pointer">GBTS Entrance</h2>
            <p class="text-[10px] font-bold text-blue-500 mb-8 uppercase tracking-[0.2em]">Official Academy Portal 2026</p>
            
            <div class="space-y-4 mb-8">
                <input id="uName" type="text" placeholder="Candidate Full Name" class="w-full p-5 bg-slate-50 rounded-3xl outline-none border-2 focus:border-blue-600 font-bold transition-all text-center">
                <input id="uPin" type="password" maxlength="4" placeholder="Create 4-Digit Security PIN" class="w-full p-5 bg-slate-50 rounded-3xl outline-none border-2 focus:border-blue-600 font-bold tracking-widest transition-all text-center">
            </div>
            
            <button onclick="handleLogin()" class="w-full bg-blue-900 text-white py-5 rounded-[25px] font-black text-lg hover:bg-blue-800 shadow-xl transition-all active:scale-95">VERIFY & ENTER</button>
            <p class="mt-6 text-[9px] text-slate-400 font-bold leading-relaxed">NOTICE: Please remember your Name and PIN to recover your progress on any device.</p>
        </div>
    </div>

    <div id="quizModal" class="quiz-modal">
        <div class="bg-white p-10 rounded-[45px] max-w-lg w-full relative shadow-2xl">
            <div id="timer" class="absolute -top-6 left-1/2 -translate-x-1/2 bg-red-600 text-white px-8 py-2 rounded-full font-black shadow-xl border-4 border-white">⏱️ <span id="timeSec">30</span>s</div>
            <h3 id="mTitle" class="text-2xl font-black text-blue-950 mb-2 italic">Module Test</h3>
            <div id="mStudy" class="text-sm bg-blue-50 p-5 rounded-3xl mb-6 border border-blue-100 text-blue-900 leading-relaxed italic"></div>
            <div id="mQuestionArea"></div>
            <button onclick="closeQuiz()" class="mt-6 text-[10px] font-black text-slate-300 uppercase w-full hover:text-red-500 transition">Exit Examination</button>
        </div>
    </div>

    <nav class="glass sticky top-0 z-50 p-4 px-8 flex justify-between items-center border-b border-slate-200">
        <div class="flex items-center gap-3">
            <div class="w-10 h-10 bg-blue-900 text-white flex items-center justify-center rounded-xl font-black shadow-md">G</div>
            <div>
                <h1 class="font-black text-sm text-slate-900 uppercase leading-none">GBTS Academy</h1>
                <p class="text-[8px] font-bold text-blue-600 uppercase">Prime Solutions Verified</p>
            </div>
        </div>
        <div class="flex gap-4 md:gap-8 items-center">
            <button onclick="showTab('dashTab')" class="text-[10px] font-black uppercase text-blue-900">Training</button>
            <button onclick="showTab('rankTab')" class="text-[10px] font-black uppercase text-slate-400 hover:text-blue-900 transition">Merit</button>
            <button onclick="showTab('idTab')" class="text-[10px] font-black uppercase text-slate-400 hover:text-blue-900 transition">ID Card</button>
            <button onclick="logout()" class="w-8 h-8 flex items-center justify-center bg-red-50 text-red-500 rounded-full hover:bg-red-500 hover:text-white transition">✖</button>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-6">
        <div id="dashTab" class="tab-content active-tab">
            <div class="bg-slate-900 p-10 rounded-[50px] text-white shadow-2xl mb-12 relative overflow-hidden">
                <div class="relative z-10 grid md:grid-cols-2 gap-8 items-center">
                    <div>
                        <p class="text-[10px] font-black text-blue-400 uppercase mb-2 tracking-[0.2em]">Candidate Welcome</p>
                        <h2 id="dispName" class="text-4xl md:text-5xl font-black italic uppercase tracking-tighter mb-4 leading-none">---</h2>
                        <div class="flex flex-wrap gap-3">
                            <span class="bg-white/10 px-4 py-2 rounded-xl text-[10px] font-bold border border-white/10 uppercase">Rank: <span id="dispLvl">01</span></span>
                            <a href="#" class="bg-blue-600 hover:bg-blue-500 px-4 py-2 rounded-xl text-[10px] font-bold transition uppercase">📥 Syllabus PDF</a>
                        </div>
                    </div>
                    <div class="bg-white/5 p-6 rounded-3xl border border-white/10">
                        <p class="text-[10px] font-black mb-2 opacity-60 uppercase">Completion Progress</p>
                        <div class="w-full bg-white/10 h-4 rounded-full overflow-hidden mb-2">
                            <div id="progBar" class="progress-fill h-full bg-blue-500" style="width: 0%"></div>
                        </div>
                        <p class="text-[10px] font-bold"><span id="progText">0</span>% Complete</p>
                    </div>
                </div>
            </div>

            <div class="flex items-center justify-between mb-8 px-4">
                <h3 class="text-xl font-black uppercase italic text-slate-900">Training Roadmap</h3>
                <p class="text-[10px] font-bold text-slate-400">SELECT ACTIVE MODULE</p>
            </div>
            
            <div id="levelGrid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-6"></div>
        </div>

        <div id="rankTab" class="tab-content">
            <div class="max-w-2xl mx-auto bg-white rounded-[50px] p-10 shadow-xl border border-slate-100">
                <div class="text-center mb-10">
                    <h3 class="text-3xl font-black text-blue-950 uppercase italic mb-2 tracking-tighter">Elite Merit List</h3>
                    <p class="text-[10px] font-bold text-slate-400 uppercase">Top 10 Performers Worldwide</p>
                </div>
                <div id="leaderList" class="space-y-4"></div>
            </div>
        </div>

        <div id="idTab" class="tab-content">
            <div class="max-w-sm mx-auto bg-white p-10 rounded-[60px] shadow-2xl border-2 border-slate-50 text-center relative overflow-hidden">
                <div class="absolute top-0 left-0 w-full h-24 bg-blue-900 -z-10"></div>
                <div class="w-24 h-24 bg-white rounded-full mx-auto mt-4 mb-6 flex items-center justify-center shadow-xl border-4 border-white text-4xl">👤</div>
                <h3 id="cardName" class="text-3xl font-black text-slate-900 uppercase italic mb-2">---</h3>
                <p class="text-[9px] font-black text-blue-500 mb-8 uppercase tracking-[0.3em]">Official Recruitment Pass</p>
                <div class="grid grid-cols-2 gap-3 mb-8">
                    <div class="bg-slate-50 p-4 rounded-3xl">
                        <p class="text-[8px] font-black opacity-40 uppercase">Level</p>
                        <p id="cardLvl" class="text-xl font-black">01</p>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-3xl">
                        <p class="text-[8px] font-black opacity-40 uppercase">Status</p>
                        <p class="text-[10px] font-black text-green-600">ACTIVE</p>
                    </div>
                </div>
                <p class="text-[8px] font-black text-slate-300 uppercase">System ID: <span id="cardId">#000000</span></p>
            </div>
        </div>
    </main>

    <footer class="mt-20 p-12 text-center">
        <div class="w-12 h-1 bg-slate-200 mx-auto mb-6 rounded-full"></div>
        <p class="text-[10px] font-black text-slate-300 uppercase tracking-[0.4em]">Prime Solutions Official Technology</p>
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

        let userData = { name: "", level: 1, pin: "" };
        let timer = null, taps = 0;
        const successSfx = document.getElementById('successSfx'), errorSfx = document.getElementById('errorSfx');

        const studyMaterial = {
            1: { s: "Pakistan's National Anthem was written by Hafeez Jalandhari.", q: "Who wrote the National Anthem?", a: "Hafeez Jalandhari" },
            2: { s: "The value of PI (π) is approximately 3.14.", q: "What is the approximate value of PI?", a: "3.14" },
            3: { s: "The largest planet in our solar system is Jupiter.", q: "Which is the largest planet?", a: "Jupiter" }
        };

        function secretAdminTap() {
            taps++;
            if(taps >= 5) {
                let p = prompt("Admin Authorization:");
                if(p === "prime786") alert("Admin Privileges Unlocked.");
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
            if(!name || pin.length < 4) return alert("Please enter Full Name & 4-Digit PIN!");

            const userPath = btoa(name.toLowerCase().replace(/\s/g, '') + pin);
            const snap = await db.ref('gbts_trusted/' + userPath).get();

            auth.signInAnonymously().then(res => {
                if(!snap.exists()) {
                    db.ref('gbts_trusted/' + userPath).set({ uid: res.user.uid, name, level: 1, pin, path: userPath });
                }
                location.reload();
            });
        }

        function loadUserData(uid) {
            db.ref('gbts_trusted').orderByChild('uid').equalTo(uid).on('value', snap => {
                snap.forEach(c => {
                    userData = c.val();
                    updateInterface(userData);
                });
            });
            loadLeaderboard();
        }

        function updateInterface(data) {
            document.getElementById('dispName').innerText = data.name;
            document.getElementById('dispLvl').innerText = data.level < 10 ? "0"+data.level : data.level;
            document.getElementById('cardName').innerText = data.name;
            document.getElementById('cardLvl').innerText = data.level;
            document.getElementById('cardId').innerText = "#" + data.uid.substring(0, 8).toUpperCase();
            
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
                    <div onclick="openModule(${i}, ${lock})" class="level-card p-10 bg-white border-2 border-slate-50 text-center ${lock ? 'locked' : 'shadow-sm'}">
                        <div class="text-4xl mb-4">${lock ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-[10px] uppercase tracking-widest text-slate-400">Module</h4>
                        <p class="text-2xl font-black text-slate-900">${i < 10 ? "0"+i : i}</p>
                        ${!lock && i == userData.level ? '<div class="mt-2 text-[8px] font-black text-blue-600 animate-pulse">START NOW</div>' : ''}
                    </div>
                `;
            }
        }

        function openModule(num, lock) {
            if(lock) return errorSfx.play();
            if(num !== userData.level) return alert("Please complete Module " + userData.level + " first sweetie!");
            
            const data = studyMaterial[num] || { s: "Official training module review. Read the instructions carefully.", q: "Type 'confirmed' to pass.", a: "confirmed" };
            document.getElementById('quizModal').style.display = 'flex';
            document.getElementById('mTitle').innerText = "Module " + num;
            document.getElementById('mStudy').innerText = data.s;
            document.getElementById('mQuestionArea').innerHTML = `
                <p class="font-bold text-slate-900 mb-4 text-center underline decoration-blue-500">EXAMINATION QUESTION</p>
                <p class="text-center mb-6 font-bold text-slate-600">${data.q}</p>
                <input id="ansInp" type="text" placeholder="Type Answer Here" class="w-full p-5 bg-slate-50 rounded-3xl outline-none border-2 focus:border-blue-600 font-black text-center mb-4 transition-all">
                <button onclick="checkAnswer(${num}, '${data.a}')" class="w-full bg-blue-900 text-white py-5 rounded-3xl font-black hover:bg-blue-800 shadow-xl transition-all">SUBMIT RESPONSE</button>
            `;
            startTimer();
        }

        function startTimer() {
            let s = 30; document.getElementById('timeSec').innerText = s;
            clearInterval(timer);
            timer = setInterval(() => {
                s--; document.getElementById('timeSec').innerText = s;
                if(s <= 0) { clearInterval(timer); alert("EXAMINATION TIME OUT!"); closeQuiz(); }
            }, 1000);
        }

        function checkAnswer(num, correct) {
            const val = document.getElementById('ansInp').value.trim().toLowerCase();
            if(val === correct.toLowerCase()) {
                clearInterval(timer); successSfx.play();
                confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 }, colors: ['#1e3a8a', '#3b82f6', '#fbbf24'] });
                
                db.ref('gbts_trusted/' + userData.path).update({ level: userData.level + 1 });
                db.ref('trusted_ranks/' + auth.currentUser.uid).set({ name: userData.name, level: userData.level + 1 });
                
                setTimeout(() => { alert("WELL DONE! Module Complete."); closeQuiz(); }, 500);
            } else { errorSfx.play(); alert("INCORRECT RESPONSE. Review study material again."); }
        }

        function loadLeaderboard() {
            db.ref('trusted_ranks').orderByChild('level').limitToLast(10).on('value', snap => {
                const list = document.getElementById('leaderList'); list.innerHTML = '';
                let data = []; snap.forEach(c => data.push(c.val()));
                data.reverse().forEach((r, i) => {
                    list.innerHTML += `
                        <div class="flex justify-between items-center p-5 bg-slate-50 rounded-[30px] border border-slate-100">
                            <div class="flex items-center gap-4">
                                <span class="w-8 h-8 flex items-center justify-center bg-blue-900 text-white rounded-full font-black text-xs">${i+1}</span>
                                <span class="font-black uppercase text-sm italic text-slate-800">${r.name}</span>
                            </div>
                            <span class="text-[10px] font-black bg-blue-100 text-blue-900 px-4 py-1 rounded-full">LEVEL ${r.level}</span>
                        </div>
                    `;
                });
            });
        }

        function closeQuiz() { clearInterval(timer); document.getElementById('quizModal').style.display = 'none'; }
        function showTab(id) { document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab')); document.getElementById(id).classList.add('active-tab'); }
        function logout() { auth.signOut().then(() => location.reload()); }
    </script>
</body>
</html>
