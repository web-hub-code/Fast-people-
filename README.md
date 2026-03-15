<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Pro Academy | Elite Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f4f7fc; }
        .glass-nav { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(15px); }
        .level-card { transition: 0.4s; border-radius: 35px; background: white; position: relative; }
        .locked { filter: grayscale(1); opacity: 0.5; cursor: not-allowed; }
        .pending { border: 2px solid #f59e0b; background: #fffbeb; }
        .q-card { background: #fff; padding: 25px; border-radius: 20px; border: 1px solid #e5e7eb; margin-bottom: 15px; }
        .option-label { display: block; padding: 15px; border: 2px solid #f3f4f6; border-radius: 15px; cursor: pointer; margin-top: 8px; transition: 0.2s; }
        .option-label:hover { background: #eff6ff; border-color: #1e3a8a; }
        .tab-content { display: none; }
        .active-tab { display: block; }
    </style>
</head>
<body class="pb-24">

    <div id="loginScreen" class="fixed inset-0 bg-[#0f172a] z-[500] flex items-center justify-center p-6 hidden">
        <div class="bg-white p-10 rounded-[45px] max-w-md w-full shadow-2xl text-center">
            <h2 class="text-4xl font-black text-blue-900 mb-2 italic tracking-tighter">PRIME ACADEMY</h2>
            <p class="text-xs font-bold text-gray-400 mb-8 uppercase tracking-widest">Candidate Login Portal</p>
            <input id="candidateName" type="text" placeholder="Enter Full Name" class="w-full p-5 bg-gray-100 rounded-2xl mb-4 border-2 border-transparent focus:border-blue-600 outline-none">
            <button onclick="loginUser()" class="w-full bg-blue-900 text-white py-5 rounded-2xl font-black text-lg shadow-xl hover:scale-95 transition">LOGIN TO PORTAL</button>
        </div>
    </div>

    <nav class="glass-nav sticky top-0 z-50 p-4 border-b">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <h1 class="font-black text-blue-900 text-2xl italic tracking-tighter">PRIME PORTAL</h1>
            <div class="flex gap-4">
                <button onclick="showTab('levelsTab')" class="text-xs font-black uppercase text-blue-900">Exams</button>
                <button onclick="showTab('bookTab')" class="text-xs font-black uppercase text-gray-500">Syllabus Book</button>
                <button onclick="logout()" class="text-xs font-black uppercase text-red-500 underline">Logout</button>
            </div>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-4 mt-8">
        
        <div id="levelsTab" class="tab-content active-tab">
            <div class="mb-10">
                <h2 class="text-4xl font-black italic uppercase text-slate-800 tracking-tighter">Exam Progression</h2>
                <p class="text-blue-600 font-bold">Scoring 50% or above requires Admin Approval to move forward.</p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-4 gap-6" id="levelContainer"></div>
        </div>

        <div id="bookTab" class="tab-content">
            <h2 class="text-4xl font-black mb-10 italic uppercase text-slate-800 tracking-tighter">Official Study Guide</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-white p-8 rounded-[40px] border-t-8 border-blue-900 shadow-sm">
                    <h3 class="font-black text-2xl mb-4 text-blue-900 underline">Exam Pattern & Marks</h3>
                    <ul class="text-sm space-y-3 text-gray-600">
                        <li><b>Total Marks:</b> 100 Marks per level.</li>
                        <li><b>Passing Criteria:</b> Minimum 50% Marks required.</li>
                        <li><b>Subjects:</b> English (20), Urdu (20), Islamiyat (20), GK (20), Math (20).</li>
                        <li><b>Note:</b> After passing, wait for Admin to verify your attempt.</li>
                    </ul>
                </div>
                <div class="bg-white p-8 rounded-[40px] border-t-8 border-green-700 shadow-sm">
                    <h3 class="font-black text-2xl mb-4 text-green-700 underline">Subject Wise Details</h3>
                    <p class="text-sm text-gray-600 leading-relaxed">
                        <b>English:</b> Focused on Synonyms, Tenses, and Prepositions.<br>
                        <b>GB GK:</b> Must know 14 districts, mountains, and rivers names.<br>
                        <b>Islamiyat:</b> Basic knowledge of Quranic Surahs and Battles of Islam.
                    </p>
                </div>
            </div>
        </div>

        <div id="adminPanel" class="tab-content mt-20 p-10 bg-slate-900 text-white rounded-[40px]">
            <h2 class="text-3xl font-black mb-6 text-yellow-400">ADMIN CONTROL CENTER</h2>
            <div id="approvalRequests" class="space-y-4 text-slate-900">
                <p class="text-white opacity-50 italic">No pending requests...</p>
            </div>
        </div>
    </main>

    <div id="testBox" class="hidden fixed inset-0 bg-white z-[400] p-6 overflow-y-auto">
        <div class="max-w-3xl mx-auto">
            <div class="flex justify-between items-center mb-10">
                <h2 id="currentLevelTitle" class="text-4xl font-black text-blue-900 italic">LEVEL 01</h2>
                <span class="font-black text-gray-400">Total: 100 Marks</span>
            </div>
            <div id="quizContent" class="space-y-6"></div>
            <button onclick="submitExam()" class="w-full bg-blue-900 text-white py-6 rounded-[30px] mt-10 font-black text-xl shadow-2xl">SUBMIT FOR REVIEW</button>
        </div>
    </div>

    <footer class="fixed bottom-4 left-0 right-0 text-center">
        <span onclick="triggerAdmin()" class="text-[10px] text-gray-400 cursor-default opacity-20">Prime Solutions Admin Gateway</span>
    </footer>

    <script>
        let user = { name: "", level: 1, status: "ready" };
        let tapCount = 0;

        const questionsData = [
            { q: "What is the capital of Gilgit-Baltistan?", a: "Skardu", b: "Gilgit", c: "Hunza", d: "Astore", ans: "b" },
            { q: "Which is the highest peak in GB?", a: "Nanga Parbat", b: "K2", c: "Rakaposhi", d: "Broad Peak", ans: "b" },
            { q: "Total districts in GB?", a: "10", b: "12", c: "14", d: "16", ans: "c" },
            { q: "Battle of Badr was fought in?", a: "1 AH", b: "2 AH", c: "3 AH", d: "5 AH", ans: "b" },
            { q: "Senior is always followed by preposition?", a: "Than", b: "To", c: "By", d: "Of", ans: "b" }
        ];

        window.onload = () => {
            const savedName = localStorage.getItem('gb_user_v3');
            if (savedName) {
                user.name = savedName;
                user.level = parseInt(localStorage.getItem('gb_level_v3_' + savedName)) || 1;
                user.status = localStorage.getItem('gb_status_v3_' + savedName) || "ready";
                renderLevels();
            } else {
                document.getElementById('loginScreen').classList.remove('hidden');
            }
        };

        function loginUser() {
            const name = document.getElementById('candidateName').value.trim();
            if(!name) return alert("Please enter your name!");
            user.name = name;
            localStorage.setItem('gb_user_v3', name);
            if(!localStorage.getItem('gb_level_v3_' + name)) {
                localStorage.setItem('gb_level_v3_' + name, 1);
                localStorage.setItem('gb_status_v3_' + name, "ready");
            }
            location.reload();
        }

        function logout() { localStorage.clear(); location.reload(); }

        function renderLevels() {
            const container = document.getElementById('levelContainer');
            container.innerHTML = '';
            for(let i=1; i<=10; i++) {
                let statusClass = i > user.level ? 'locked' : '';
                let statusText = i > user.level ? 'LOCKED' : 'AVAILABLE';
                
                if(i === user.level && user.status === "pending") {
                    statusClass = 'pending';
                    statusText = 'PENDING APPROVAL';
                }

                container.innerHTML += `
                    <div onclick="openTest(${i}, '${statusClass}')" class="level-card p-10 text-center shadow-sm border-2 ${statusClass}">
                        <div class="text-5xl mb-4">${statusClass === 'locked' ? '🔒' : '🎖️'}</div>
                        <h4 class="font-black text-slate-800 uppercase">Level ${i}</h4>
                        <p class="text-[10px] font-black mt-2 ${statusClass === 'pending' ? 'text-amber-600' : 'text-blue-500'}">${statusText}</p>
                    </div>
                `;
            }
            updateAdminPanel();
        }

        function openTest(num, state) {
            if(state === 'locked') return alert("Complete previous level first!");
            if(state === 'pending') return alert("Wait for Admin to approve your result!");
            
            document.getElementById('testBox').classList.remove('hidden');
            const quiz = document.getElementById('quizContent');
            quiz.innerHTML = '';
            questionsData.forEach((item, idx) => {
                quiz.innerHTML += `
                    <div class="q-card">
                        <p class="font-bold text-lg mb-4">Q${idx+1}: ${item.q}</p>
                        <div class="space-y-2">
                            <label class="option-label"><input type="radio" name="q${idx}" value="a"> ${item.a}</label>
                            <label class="option-label"><input type="radio" name="q${idx}" value="b"> ${item.b}</label>
                            <label class="option-label"><input type="radio" name="q${idx}" value="c"> ${item.c}</label>
                            <label class="option-label"><input type="radio" name="q${idx}" value="d"> ${item.d}</label>
                        </div>
                    </div>
                `;
            });
        }

        function submitExam() {
            let score = 0;
            questionsData.forEach((item, idx) => {
                const selected = document.querySelector(`input[name="q${idx}"]:checked`);
                if(selected && selected.value === item.ans) score += 20;
            });

            if(score < 50) {
                alert(`Result: ${score}%. You Failed! Need 50% to pass. Try Again.`);
                location.reload();
            } else {
                alert(`Result: ${score}%. PASSED! Your request sent to Admin for Level Unlocking.`);
                user.status = "pending";
                localStorage.setItem('gb_status_v3_' + user.name, "pending");
                localStorage.setItem('gb_score_v3_' + user.name, score);
                location.reload();
            }
        }

        function triggerAdmin() {
            tapCount++;
            if(tapCount === 4) {
                const key = prompt("Enter Secret Admin Key:");
                if(key === "prime786") {
                    showTab('adminPanel');
                    alert("Admin Access Granted!");
                }
                tapCount = 0;
            }
        }

        function updateAdminPanel() {
            const panel = document.getElementById('approvalRequests');
            if(user.status === "pending") {
                panel.innerHTML = `
                    <div class="bg-white p-6 rounded-2xl flex justify-between items-center shadow-lg">
                        <div>
                            <p class="font-black text-xl">${user.name}</p>
                            <p class="text-sm text-gray-500">Level: ${user.level} | Score: ${localStorage.getItem('gb_score_v3_' + user.name)}%</p>
                        </div>
                        <div class="flex gap-2">
                            <button onclick="approveLevel()" class="bg-green-500 text-white px-6 py-2 rounded-xl font-bold">Approve</button>
                            <button onclick="rejectLevel()" class="bg-red-500 text-white px-6 py-2 rounded-xl font-bold">Reject</button>
                        </div>
                    </div>
                `;
            }
        }

        function approveLevel() {
            user.level++;
            user.status = "ready";
            localStorage.setItem('gb_level_v3_' + user.name, user.level);
            localStorage.setItem('gb_status_v3_' + user.name, "ready");
            alert("Level Approved Successfully!");
            location.reload();
        }

        function rejectLevel() {
            user.status = "ready";
            localStorage.setItem('gb_status_v3_' + user.name, "ready");
            alert("Level Rejected. Student must re-take the exam.");
            location.reload();
        }

        function showTab(id) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(id).classList.add('active-tab');
        }
    </script>
</body>
</html>
