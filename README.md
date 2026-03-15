<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Recruitment Portal | Official Exam</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f1f5f9; scroll-behavior: smooth; }
        .exam-card { background: white; border-radius: 20px; padding: 25px; border-left: 8px solid #1e3a8a; box-shadow: 0 10px 25px rgba(0,0,0,0.05); margin-bottom: 30px; transition: 0.3s; }
        .exam-card:hover { transform: translateY(-5px); box-shadow: 0 15px 35px rgba(0,0,0,0.1); }
        .option-label { display: flex; align-items: center; padding: 15px; border: 2px solid #e2e8f0; border-radius: 12px; cursor: pointer; transition: 0.2s; margin-top: 10px; font-weight: 500; }
        .option-label:hover { background: #eff6ff; border-color: #1e3a8a; }
        input[type="radio"]:checked + span { color: #1e3a8a; font-weight: 800; }
        .section-badge { background: #1e3a8a; color: white; padding: 8px 20px; border-radius: 50px; font-size: 0.8rem; font-weight: 900; letter-spacing: 1px; display: inline-block; margin-bottom: 20px; text-transform: uppercase; }
        input[type="radio"] { width: 20px; height: 20px; accent-color: #1e3a8a; margin-right: 15px; }
    </style>
</head>
<body class="pb-24">

    <nav class="sticky top-0 z-50 bg-blue-900 text-white p-5 shadow-2xl">
        <div class="max-w-5xl mx-auto flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="bg-white p-2 rounded-lg">
                    <div class="w-8 h-8 bg-blue-900 rounded-full"></div>
                </div>
                <div>
                    <h1 class="text-xl md:text-2xl font-black leading-none">GB POLICE TEST</h1>
                    <p class="text-[10px] text-blue-300 uppercase tracking-widest font-bold">Official Recruitment 2026</p>
                </div>
            </div>
            <div class="bg-blue-800 px-6 py-2 rounded-2xl border border-blue-400 shadow-inner">
                <p class="text-[10px] font-bold text-center text-blue-200">REMAINING</p>
                <div id="countdown" class="text-xl font-mono font-black text-yellow-400">90:00</div>
            </div>
        </div>
    </nav>

    <main class="max-w-4xl mx-auto p-4 mt-10">
        <form id="officialExam">

            <div class="section-badge">Part I: English Grammar</div>
            
            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">1. Which sentence is grammatically correct?</p>
                <label class="option-label"><input type="radio" name="q1" value="A"><span>He do not like apples.</span></label>
                <label class="option-label"><input type="radio" name="q1" value="B"><span>He does not like apples.</span></label>
            </div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">2. Synonym of 'Vigilant' is:</p>
                <label class="option-label"><input type="radio" name="q2" value="A"><span>Careless</span></label>
                <label class="option-label"><input type="radio" name="q2" value="B"><span>Watchful</span></label>
            </div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">3. Antonym of 'Transparent' is:</p>
                <label class="option-label"><input type="radio" name="q3" value="A"><span>Opaque</span></label>
                <label class="option-label"><input type="radio" name="q3" value="B"><span>Clear</span></label>
            </div>

            <div class="section-badge" style="background: #047857;">Part II: Gilgit-Baltistan GK</div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">4. K2 is also known as ______.</p>
                <label class="option-label"><input type="radio" name="q4" value="A"><span>Mount Everest</span></label>
                <label class="option-label"><input type="radio" name="q4" value="B"><span>Godwin-Austen</span></label>
            </div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">5. Which river flows through Gilgit city?</p>
                <label class="option-label"><input type="radio" name="q5" value="A"><span>Indus River</span></label>
                <label class="option-label"><input type="radio" name="q5" value="B"><span>Gilgit River</span></label>
            </div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">6. Skardu is located at the bank of which river?</p>
                <label class="option-label"><input type="radio" name="q6" value="A"><span>Indus</span></label>
                <label class="option-label"><input type="radio" name="q6" value="B"><span>Hunza</span></label>
            </div>

            <div class="section-badge" style="background: #b45309;">Part III: Islamiyat</div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">7. How many Sajdas are in the Holy Quran?</p>
                <label class="option-label"><input type="radio" name="q7" value="A"><span>12</span></label>
                <label class="option-label"><input type="radio" name="q7" value="B"><span>14</span></label>
            </div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">8. Prophet Muhammad (PBUH) performed ______ Hajj.</p>
                <label class="option-label"><input type="radio" name="q8" value="A"><span>One</span></label>
                <label class="option-label"><input type="radio" name="q8" value="B"><span>Two</span></label>
            </div>

            <div class="section-badge" style="background: #be123c;">Part IV: Math & IQ</div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">9. If a person buys a pen for 50 and sells for 60, profit is:</p>
                <label class="option-label"><input type="radio" name="q9" value="A"><span>10%</span></label>
                <label class="option-label"><input type="radio" name="q9" value="B"><span>20%</span></label>
            </div>

            <div class="exam-card">
                <p class="text-lg font-bold text-gray-800">10. Complete: 2, 4, 8, 16, ___?</p>
                <label class="option-label"><input type="radio" name="q10" value="A"><span>24</span></label>
                <label class="option-label"><input type="radio" name="q10" value="B"><span>32</span></label>
            </div>

            <button type="button" onclick="processResult()" class="w-full bg-blue-900 text-white py-6 rounded-2xl font-black text-2xl shadow-2xl hover:bg-black transition-all mb-24 uppercase tracking-widest">
                Submit All Answers
            </button>

        </form>
    </main>

    <div id="resultModal" class="hidden fixed inset-0 bg-blue-900/95 backdrop-blur-xl flex items-center justify-center p-6 z-[100]">
        <div class="bg-white rounded-[40px] p-12 max-w-sm w-full text-center shadow-inner">
            <div class="w-20 h-20 bg-green-100 text-green-600 rounded-full flex items-center justify-center mx-auto mb-6 text-4xl">✓</div>
            <h2 class="text-2xl font-black text-gray-900 uppercase">Test Result</h2>
            <div id="marks" class="text-8xl font-black text-blue-700 my-4 tracking-tighter">0%</div>
            <p id="feedback" class="text-gray-500 font-bold mb-8 italic"></p>
            <button onclick="location.reload()" class="bg-blue-900 text-white w-full py-4 rounded-2xl font-black shadow-lg">RETREAT EXAM</button>
        </div>
    </div>

    <script>
        // Official Answer Key
        const masterKey = { q1: "B", q2: "B", q3: "A", q4: "B", q5: "B", q6: "A", q7: "B", q8: "A", q9: "B", q10: "B" };

        function processResult() {
            let points = 0;
            const totalQ = Object.keys(masterKey).length;
            const formData = new FormData(document.getElementById('officialExam'));

            for(let key in masterKey) {
                if(formData.get(key) === masterKey[key]) points++;
            }

            const percentage = Math.round((points / totalQ) * 100);
            document.getElementById('marks').innerText = percentage + "%";
            document.getElementById('feedback').innerText = percentage >= 50 ? "Qualified for Interview!" : "Not Qualified. Try Again.";
            document.getElementById('resultModal').classList.remove('hidden');
        }

        // 90 Minutes Timer
        let timeRemaining = 5400;
        setInterval(() => {
            if(timeRemaining <= 0) return;
            timeRemaining--;
            let mm = Math.floor(timeRemaining / 60);
            let ss = timeRemaining % 60;
            document.getElementById('countdown').innerText = `${mm}:${ss < 10 ? '0' : ''}${ss}`;
        }, 1000);
    </script>
</body>
</html>
