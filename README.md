<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Official Examination Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;800&display=swap');
        body { font-family: 'Poppins', sans-serif; background: #f8fafc; }
        .question-card { background: white; border-radius: 15px; padding: 20px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1); border-left: 6px solid #1e3a8a; margin-bottom: 20px; }
        .option-item { display: flex; align-items: center; padding: 12px; border: 1px solid #e2e8f0; border-radius: 10px; cursor: pointer; transition: 0.2s; margin-top: 8px; }
        .option-item:hover { background: #eff6ff; border-color: #1e3a8a; }
        input[type="radio"] { width: 18px; height: 18px; margin-right: 12px; cursor: pointer; }
    </style>
</head>
<body>

    <header class="bg-blue-900 text-white sticky top-0 z-50 shadow-2xl p-4">
        <div class="max-w-5xl mx-auto flex justify-between items-center">
            <div>
                <h1 class="text-xl md:text-2xl font-extrabold uppercase tracking-tighter">GB Police Mock 2026</h1>
                <p class="text-xs text-blue-300 italic">Created by Prime Solutions</p>
            </div>
            <div class="text-right">
                <span class="block text-[10px] uppercase font-bold text-blue-200">Time Remaining</span>
                <div id="timer" class="text-xl font-mono font-bold text-yellow-400">90:00</div>
            </div>
        </div>
    </header>

    <main class="max-w-4xl mx-auto p-4 mt-6">
        <form id="fullExamForm">
            
            <div class="mb-10">
                <h2 class="bg-blue-100 text-blue-900 px-4 py-2 rounded-lg font-bold mb-6 inline-block">SECTION A: ENGLISH (20 Marks)</h2>
                
                <div class="question-card">
                    <p class="font-bold">1. Find the synonym of "COURAGEOUS":</p>
                    <label class="option-item"><input type="radio" name="q1" value="A"> Brave</label>
                    <label class="option-item"><input type="radio" name="q1" value="B"> Fearful</label>
                    <label class="option-item"><input type="radio" name="q1" value="C"> Weak</label>
                </div>

                <div class="question-card">
                    <p class="font-bold">2. He has been living here ______ 2010.</p>
                    <label class="option-item"><input type="radio" name="q2" value="A"> For</label>
                    <label class="option-item"><input type="radio" name="q2" value="B"> Since</label>
                    <label class="option-item"><input type="radio" name="q2" value="C"> From</label>
                </div>
            </div>

            <div class="mb-10">
                <h2 class="bg-green-100 text-green-900 px-4 py-2 rounded-lg font-bold mb-6 inline-block">SECTION B: GK & GB GEOGRAPHY (20 Marks)</h2>
                
                <div class="question-card">
                    <p class="font-bold">3. Which is the highest peak in Gilgit-Baltistan?</p>
                    <label class="option-item"><input type="radio" name="q3" value="A"> Nanga Parbat</label>
                    <label class="option-item"><input type="radio" name="q3" value="B"> K2</label>
                    <label class="option-item"><input type="radio" name="q3" value="C"> Rakaposhi</label>
                </div>

                <div class="question-card">
                    <p class="font-bold">4. The "Khunjerab Pass" connects Pakistan with:</p>
                    <label class="option-item"><input type="radio" name="q4" value="A"> Afghanistan</label>
                    <label class="option-item"><input type="radio" name="q4" value="B"> China</label>
                    <label class="option-item"><input type="radio" name="q4" value="C"> India</label>
                </div>
            </div>

            <div class="mb-10">
                <h2 class="bg-yellow-100 text-yellow-900 px-4 py-2 rounded-lg font-bold mb-6 inline-block">SECTION C: ISLAMIYAT (20 Marks)</h2>
                
                <div class="question-card">
                    <p class="font-bold">5. How many Madni Surahs are in the Holy Quran?</p>
                    <label class="option-item"><input type="radio" name="q5" value="A"> 86</label>
                    <label class="option-item"><input type="radio" name="q5" value="B"> 28</label>
                    <label class="option-item"><input type="radio" name="q5" value="C"> 114</label>
                </div>
            </div>

            <div class="mb-10">
                <h2 class="bg-red-100 text-red-900 px-4 py-2 rounded-lg font-bold mb-6 inline-block">SECTION D: MATH & IQ (20 Marks)</h2>
                
                <div class="question-card">
                    <p class="font-bold">6. Solve: 15% of 200 is?</p>
                    <label class="option-item"><input type="radio" name="q6" value="A"> 20</label>
                    <label class="option-item"><input type="radio" name="q6" value="B"> 30</label>
                    <label class="option-item"><input type="radio" name="q6" value="C"> 40</label>
                </div>
            </div>

            <button type="button" onclick="submitExam()" class="w-full bg-blue-900 text-white py-5 rounded-2xl font-bold text-xl shadow-xl hover:bg-black transition mb-20">
                SUBMIT FINAL EXAM
            </button>

        </form>
    </main>

    <div id="resultModal" class="hidden fixed inset-0 bg-black/90 backdrop-blur-md flex items-center justify-center p-4 z-[100]">
        <div class="bg-white p-10 rounded-3xl max-w-md w-full text-center shadow-2xl">
            <h2 class="text-2xl font-bold text-gray-800">Exam Results</h2>
            <div id="percentage" class="text-7xl font-black text-blue-700 my-6">0%</div>
            <p id="status" class="text-lg font-semibold mb-6 italic"></p>
            <button onclick="location.reload()" class="bg-blue-900 text-white px-10 py-3 rounded-full font-bold">Try Again</button>
        </div>
    </div>

    <script>
        const answers = { q1: "A", q2: "B", q3: "B", q4: "B", q5: "B", q6: "B" };

        function submitExam() {
            let score = 0;
            const total = Object.keys(answers).length;
            const form = new FormData(document.getElementById('fullExamForm'));

            for(let key in answers) {
                if(form.get(key) === answers[key]) score++;
            }

            const perc = Math.round((score / total) * 100);
            document.getElementById('percentage').innerText = perc + "%";
            document.getElementById('status').innerText = perc >= 50 ? "Mubarak ho! You Passed." : "Hard luck! Try again.";
            document.getElementById('resultModal').classList.remove('hidden');
        }

        let time = 5400;
        setInterval(() => {
            if(time <= 0) return;
            time--;
            let m = Math.floor(time / 60);
            let s = time % 60;
            document.getElementById('timer').innerText = `${m}:${s < 10 ? '0' : ''}${s}`;
        }, 1000);
    </script>
</body>
</html>
