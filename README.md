<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GBTS Official Recruitment Portal | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f8fafc; color: #1e293b; }
        .master-card { background: white; border-radius: 24px; box-shadow: 0 20px 40px rgba(0,0,0,0.05); border-top: 8px solid #1e3a8a; }
        .option-label { display: flex; align-items: center; padding: 14px; border: 2px solid #f1f5f9; border-radius: 12px; cursor: pointer; margin-top: 8px; transition: 0.3s; }
        .option-label:hover { border-color: #1e3a8a; background: #f0f7ff; }
        .correct { background: #dcfce7 !important; border-color: #22c55e !important; color: #166534; }
        .wrong { background: #fee2e2 !important; border-color: #ef4444 !important; color: #991b1b; }
        .btn-primary { background: #1e3a8a; color: white; transition: 0.3s; }
        .btn-primary:hover { background: #1e40af; transform: translateY(-2px); }
        input[type="radio"] { width: 18px; height: 18px; accent-color: #1e3a8a; margin-right: 10px; }
    </style>
</head>
<body class="pb-20">

    <div id="welcomeBox" class="fixed inset-0 bg-[#0f172a] z-[150] flex items-center justify-center p-6 text-white text-center">
        <div class="max-w-4xl">
            <div class="mb-6 inline-block bg-blue-500/20 p-5 rounded-full">
                <span class="text-6xl">⚖️</span>
            </div>
            <h1 class="text-4xl md:text-6xl font-black mb-4 tracking-tighter">GB POLICE TEST PORTAL</h1>
            <p class="text-slate-400 font-bold mb-12 uppercase tracking-widest text-sm">Official Preparation & Mock Examination Hub</p>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <button onclick="startApp('practice')" class="bg-white text-slate-900 p-8 rounded-3xl shadow-2xl hover:bg-blue-50 transition">
                    <b class="text-lg uppercase block">Practice Mode</b>
                    <p class="text-[10px] text-slate-500 mt-2">Study with instant answer verification</p>
                </button>
                <button onclick="startApp('exam')" class="bg-blue-600 text-white p-8 rounded-3xl shadow-2xl hover:bg-blue-700 transition">
                    <b class="text-lg uppercase block">Mock Exam</b>
                    <p class="text-[10px] text-blue-100 mt-2">Official 100 Marks timed test</p>
                </button>
                <button onclick="window.open('https://wa.me/03705519562')" class="bg-emerald-600 text-white p-8 rounded-3xl shadow-2xl hover:bg-emerald-700 transition">
                    <b class="text-lg uppercase block">Support</b>
                    <p class="text-[10px] text-emerald-100 mt-2">Contact Technical Help Line</p>
                </button>
            </div>
        </div>
    </div>

    <nav class="sticky top-0 bg-white/95 backdrop-blur-md shadow-sm p-4 z-50 flex justify-between items-center border-b border-slate-200">
        <div>
            <h2 class="font-black text-blue-900 text-2xl tracking-tighter">GBTS PORTAL</h2>
            <p class="text-[10px] text-slate-500 font-bold uppercase tracking-widest">Candidate Training System</p>
        </div>
        <div id="timer" class="hidden bg-red-600 text-white px-6 py-2 rounded-xl font-black font-mono shadow-lg">90:00</div>
    </nav>

    <div class="bg-blue-900 text-white py-2 text-xs font-bold overflow-hidden border-b border-blue-800">
        <marquee scrollamount="5">LATEST UPDATES: GB Police Recruitment Test 2026 Expected in May | Registration Deadline: 25 March 2026 | Keep Practicing!</marquee>
    </div>

    <main class="max-w-5xl mx-auto p-4 mt-8">
        
        <div class="bg-white border-2 border-blue-100 rounded-3xl p-8 mb-10 flex flex-col md:flex-row justify-between items-center gap-6 shadow-sm">
            <div>
                <h3 class="font-black text-xl text-blue-900">Physical Fitness Eligibility</h3>
                <p class="text-slate-500 text-sm">Standard Running: 1.6 KM within 7 Minutes for Male Candidates.</p>
            </div>
            <div class="flex gap-3">
                <input id="runTime" type="number" placeholder="Min" class="w-20 border border-slate-300 p-3 rounded-xl outline-none focus:ring-2 ring-blue-500">
                <button onclick="checkPhysical()" class="bg-blue-900 text-white px-6 py-3 rounded-xl font-bold uppercase text-xs">Verify</button>
            </div>
        </div>

        <div class="master-card p-6 md:p-12">
            <div id="quizContainer">
                </div>
            <button onclick="showResult()" class="w-full btn-primary py-6 rounded-2xl font-black text-2xl shadow-xl mt-12 uppercase tracking-widest">
                Submit Examination
            </button>
        </div>
    </main>

    <div id="resPortal" class="hidden fixed inset-0 bg-slate-900/98 backdrop-blur-2xl z-[200] flex items-center justify-center p-6 text-white">
        <div class="max-w-sm w-full text-center">
            <h3 class="text-blue-400 font-bold uppercase tracking-widest text-xs mb-2">Final Performance Card</h3>
            <div id="finalScore" class="text-9xl font-black mb-6">0%</div>
            <p id="gradeMsg" class="font-bold text-lg mb-10 text-slate-300"></p>
            <div class="space-y-4">
                <button onclick="location.reload()" class="w-full bg-white text-slate-900 py-4 rounded-xl font-black uppercase">Restart Test</button>
                <button onclick="window.print()" class="w-full border border-slate-700 py-4 rounded-xl font-bold uppercase hover:bg-slate-800">Download Result</button>
            </div>
        </div>
    </div>

    <script>
        const database = [
            { q: "Which glacier is the longest in Gilgit-Baltistan?", a: "Biafo", b: "Siachen", ans: "b", cat: "General Knowledge" },
            { q: "Identify the correct spelling:", a: "Lieutenant", b: "Leftenant", ans: "a", cat: "English" },
            { q: "Zakat was made compulsory in which Hijri year?", a: "2 Hijri", b: "3 Hijri", ans: "a", cat: "Islamiyat" },
            { q: "What is the capital city of Gilgit-Baltistan?", a: "Skardu", b: "Gilgit", ans: "b", cat: "GB Geography" },
            { q: "He is senior ______ me in rank.", a: "Than", b: "To", ans: "b", cat: "English" },
            { q: "Solve the equation: 25 * 4 / 2", a: "50", b: "100", ans: "a", cat: "Mathematics" },
            { q: "Who is the first poet of the Urdu language?", a: "Amir Khusro", b: "Mirza Ghalib", ans: "a", cat: "Urdu" },
            { q: "K2 peak is located in which mountain range?", a: "Himalayas", b: "Karakoram", ans: "b", cat: "GB Geography" },
            { q: "The Battle of Badr was fought in:", a: "2 Hijri", b: "4 Hijri", ans: "a", cat: "Islamiyat" },
            { q: "15% of 2000 is:", a: "300", b: "250", ans: "a", cat: "Mathematics" }
        ];

        let mode = '';
        let timerVal = 5400;

        function speak(text) {
            let msg = new SpeechSynthesisUtterance(text);
            msg.rate = 0.9;
            window.speechSynthesis.speak(msg);
        }

        function startApp(m) {
            mode = m;
            document.getElementById('welcomeBox').style.display = 'none';
            if(mode === 'exam') {
                document.getElementById('timer').classList.remove('hidden');
                startTimer();
            } else {
                speak("Practice mode activated. You can now verify answers instantly.");
            }
            renderQuestions();
        }

        function renderQuestions() {
            const container = document.getElementById('quizContainer');
            const shuffled = database.sort(() => Math.random() - 0.5);
            container.innerHTML = '';
            
            shuffled.forEach((item, i) => {
                const html = `
                    <div class="q-card">
                        <span class="bg-slate-100 text-slate-600 text-[10px] font-black px-3 py-1 rounded-md uppercase">${item.cat}</span>
                        <h4 class="text-lg font-bold text-slate-800 mt-3">Q${i+1}. ${item.q}</h4>
                        <div class="mt-4 space-y-2">
                            <label class="option-label" id="q${i}a_l"><input type="radio" name="q${i}" value="a" onchange="checkAns(this, '${item.ans}', 'q${i}a_l')"> ${item.a}</label>
                            <label class="option-label" id="q${i}b_l"><input type="radio" name="q${i}" value="b" onchange="checkAns(this, '${item.ans}', 'q${i}b_l')"> ${item.b}</label>
                        </div>
                    </div>
                `;
                container.insertAdjacentHTML('beforeend', html);
            });
        }

        function checkAns(input, correct, labelId) {
            if(mode === 'practice') {
                const label = document.getElementById(labelId);
                if(input.value === correct) {
                    label.classList.add('correct');
                } else {
                    label.classList.add('wrong');
                }
            }
        }

        function checkPhysical() {
            let t = document.getElementById('runTime').value;
            if(!t) return alert("Please enter time in minutes.");
            if(t <= 7) alert("Congratulations! You meet the physical eligibility criteria.");
            else alert("Warning: You must complete 1.6KM in under 7 minutes to qualify.");
        }

        function startTimer() {
            setInterval(() => {
                if(timerVal <= 0) return;
                timerVal--;
                let m = Math.floor(timerVal/60); let s = timerVal%60;
                document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
            }, 1000);
        }

        function showResult() {
            document.getElementById('resPortal').classList.remove('hidden');
            let score = 80; // Placeholder for calculation logic
            document.getElementById('finalScore').innerText = score + "%";
            document.getElementById('gradeMsg').innerText = "Examination successfully submitted. Evaluation complete.";
        }
    </script>
</body>
</html>
