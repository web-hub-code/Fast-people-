<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Exam Portal | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #f3f4f6; }
        .quiz-container { max-width: 800px; margin: 2rem auto; }
        .option-label { transition: all 0.2s; border: 2px solid #e5e7eb; }
        .option-label:hover { border-color: #2563eb; background-color: #eff6ff; }
        input[type="radio"]:checked + span { color: #2563eb; font-weight: 600; }
    </style>
</head>
<body class="p-4">

    <div class="quiz-container">
        <div class="bg-blue-900 text-white p-6 rounded-t-2xl shadow-lg">
            <h1 class="text-2xl font-bold">GB Police Recruitment Test 2026</h1>
            <p class="text-blue-200">Total Marks: 100 | Subject: All-in-One Mock Paper</p>
            <div class="mt-4 flex justify-between items-center bg-blue-800 p-3 rounded-lg">
                <span>Time Remaining: <span id="timer" class="font-mono font-bold text-yellow-400">90:00</span></span>
                <span id="score-display" class="hidden font-bold">Score: 0/100</span>
            </div>
        </div>

        <form id="quizForm" class="bg-white p-6 rounded-b-2xl shadow-md space-y-8">
            
            <div class="border-b pb-6">
                <h2 class="text-blue-700 font-bold mb-4 uppercase text-sm tracking-widest">Section A: English</h2>
                <p class="mb-4 font-medium">1. Choose the correct spelling:</p>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                    <label class="option-label p-3 rounded-xl cursor-pointer"><input type="radio" name="q1" value="0"> Commitee</label>
                    <label class="option-label p-3 rounded-xl cursor-pointer"><input type="radio" name="q1" value="1"> Committee</label>
                </div>
            </div>

            <div class="border-b pb-6">
                <h2 class="text-blue-700 font-bold mb-4 uppercase text-sm tracking-widest">Section B: General Knowledge</h2>
                <p class="mb-4 font-medium">2. Which is the largest district of Gilgit-Baltistan by area?</p>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                    <label class="option-label p-3 rounded-xl cursor-pointer"><input type="radio" name="q2" value="0"> Gilgit</label>
                    <label class="option-label p-3 rounded-xl cursor-pointer"><input type="radio" name="q2" value="1"> Ghanche</label>
                </div>
            </div>

            <div class="border-b pb-6">
                <h2 class="text-blue-700 font-bold mb-4 uppercase text-sm tracking-widest">Section C: Islamiyat</h2>
                <p class="mb-4 font-medium">3. How many Ghazwas are mentioned in the Holy Quran?</p>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-3">
                    <label class="option-label p-3 rounded-xl cursor-pointer"><input type="radio" name="q3" value="0"> 12</label>
                    <label class="option-label p-3 rounded-xl cursor-pointer"><input type="radio" name="q3" value="1"> 27</label>
                </div>
            </div>

            <button type="button" onclick="calculateScore()" class="w-full bg-green-600 text-white font-bold py-4 rounded-xl hover:bg-green-700 transition shadow-lg">
                FINISH & SHOW RESULT
            </button>
        </form>

        <div id="resultModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4">
            <div class="bg-white p-8 rounded-2xl max-w-sm w-full text-center">
                <h2 class="text-3xl font-bold mb-2">Test Completed!</h2>
                <p class="text-gray-600 mb-6">Your total marks are:</p>
                <div id="finalScore" class="text-5xl font-black text-blue-600 mb-6">0</div>
                <button onclick="location.reload()" class="bg-blue-600 text-white px-6 py-2 rounded-lg">Retake Test</button>
            </div>
        </div>
    </div>

    <script>
        // Correct Answers Matrix
        const answers = { q1: "1", q2: "1", q3: "1" };

        function calculateScore() {
            let score = 0;
            const form = document.forms['quizForm'];
            
            // Logic to check answers
            if(form['q1'].value === answers.q1) score += 33;
            if(form['q2'].value === answers.q2) score += 33;
            if(form['q3'].value === answers.q3) score += 34;

            document.getElementById('finalScore').innerText = score + "/100";
            document.getElementById('resultModal').classList.remove('hidden');
        }

        // Timer Logic
        let timeLeft = 5400;
        setInterval(() => {
            if(timeLeft <= 0) return;
            timeLeft--;
            let mins = Math.floor(timeLeft / 60);
            let secs = timeLeft % 60;
            document.getElementById('timer').innerText = `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        }, 1000);
    </script>
</body>
</html>
