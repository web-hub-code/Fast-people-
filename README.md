<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Digital Academy | Prime Solutions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { font-family: 'Outfit', sans-serif; background: #f1f5f9; }
        .nav-glass { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(10px); }
        .level-card { transition: 0.3s; cursor: pointer; border: 2px solid transparent; }
        .level-card:hover { transform: translateY(-5px); border-color: #1e3a8a; }
        .book-section { background: #fff; border-left: 8px solid #1e3a8a; padding: 20px; border-radius: 15px; }
        .locked { opacity: 0.6; grayscale: 1; pointer-events: none; }
        .tab-content { display: none; }
        .active-tab { display: block; }
    </style>
</head>
<body class="pb-20">

    <nav class="nav-glass sticky top-0 z-50 p-4 border-b border-slate-200">
        <div class="max-w-6xl mx-auto flex justify-between items-center">
            <div>
                <h1 class="font-black text-2xl text-blue-900 tracking-tighter italic">PRIME ACADEMY</h1>
                <p class="text-[10px] font-bold text-slate-500 uppercase tracking-widest">Digital Learning Hub 2026</p>
            </div>
            <div class="flex gap-4">
                <button onclick="switchTab('training')" class="font-bold text-sm text-blue-900 px-4 py-2 border-b-2 border-blue-900">Training Hub</button>
                <button onclick="switchTab('library')" class="font-bold text-sm text-slate-500 px-4 py-2">Digital Book</button>
                <button onclick="switchTab('guide')" class="font-bold text-sm text-slate-500 px-4 py-2">Guidelines</button>
            </div>
        </div>
    </nav>

    <main class="max-w-6xl mx-auto p-4 mt-8">

        <div id="training" class="tab-content active-tab">
            <h2 class="text-3xl font-black text-slate-800 mb-8 uppercase italic tracking-tighter">Mission: 10 Levels to Success</h2>
            <div class="grid grid-cols-1 md:grid-cols-5 gap-4">
                <div onclick="startLevel(1)" class="level-card bg-white p-6 rounded-3xl shadow-sm text-center">
                    <span class="text-4xl">🛡️</span>
                    <h3 class="font-black mt-4">Level 01</h3>
                    <p class="text-[10px] text-slate-400">Basic General Knowledge</p>
                </div>
                <script>
                    for(let i=2; i<=10; i++) {
                        document.write(`
                            <div class="level-card bg-white p-6 rounded-3xl shadow-sm text-center">
                                <span class="text-4xl text-slate-300">🔒</span>
                                <h3 class="font-black mt-4 text-slate-400">Level 0${i}</h3>
                                <p class="text-[10px] text-slate-400">Locked - Finish Previous</p>
                            </div>
                        `);
                    }
                </script>
            </div>
        </div>

        <div id="library" class="tab-content">
            <h2 class="text-3xl font-black text-slate-800 mb-8 uppercase italic tracking-tighter">Candidate Study Material</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="book-section">
                    <h4 class="font-black text-blue-900 text-xl mb-4 underline">English Grammar Guide</h4>
                    <p class="text-sm text-slate-600 leading-relaxed">
                        1. <b>Prepositions:</b> Use 'Senior to', 'Junior to', 'Afraid of'.<br>
                        2. <b>Synonyms:</b> Brave = Courageous, Fast = Quick.<br>
                        3. <b>Spelling:</b> Always remember 'Lieutenant' and 'Maintenance'.
                    </p>
                </div>
                <div class="book-section border-green-700">
                    <h4 class="font-black text-green-700 text-xl mb-4 underline">Urdu Mazmoon Guide</h4>
                    <p class="text-sm text-slate-600 leading-relaxed">
                        Mazmoon likhte waqt 3 hissay banayein:<br>
                        1. <b>Tamheed:</b> Topic ka taaruf.<br>
                        2. <b>Nafs-e-Mazmoon:</b> Detail (Faide aur Nuqsan).<br>
                        3. <b>Ikhtitam:</b> Nateeja (Result).
                    </p>
                </div>
            </div>
        </div>

        <div id="guide" class="tab-content">
            <h2 class="text-3xl font-black text-slate-800 mb-8 uppercase italic tracking-tighter">Unlimited Guidelines</h2>
            <div class="space-y-4">
                <details class="bg-white p-6 rounded-3xl shadow-sm cursor-pointer">
                    <summary class="font-black text-blue-900">Physical Test Instructions (Running/Chest)</summary>
                    <p class="mt-4 text-sm text-slate-600">Male candidates must cover 1.6KM in 7 minutes. Running practice should be done early morning on an empty stomach.</p>
                </details>
                <details class="bg-white p-6 rounded-3xl shadow-sm cursor-pointer">
                    <summary class="font-black text-blue-900">Required Documents List</summary>
                    <p class="mt-4 text-sm text-slate-600">CNIC, Domicile (GB), Academic Certificates, and 4 Passport size photographs with blue background.</p>
                </details>
                <details class="bg-white p-6 rounded-3xl shadow-sm cursor-pointer">
                    <summary class="font-black text-blue-900">Interview Tips</summary>
                    <p class="mt-4 text-sm text-slate-600">Dress professionally (Pent shirt or Shalwar Kameez with Waistcoat). Be confident and keep eye contact with the board members.</p>
                </details>
            </div>
        </div>

    </main>

    <div id="testOverlay" class="hidden fixed inset-0 bg-white z-[100] p-6 overflow-y-auto">
        <button onclick="closeTest()" class="text-red-600 font-bold mb-8 uppercase tracking-widest text-sm">← Back to Academy</button>
        <div class="max-w-3xl mx-auto">
            <h2 id="levelTitle" class="text-4xl font-black text-blue-900 mb-10">LEVEL 01: BEGINNER</h2>
            <div id="paperContent" class="space-y-8">
                </div>
            <button onclick="alert('Congratulations! Level Completed.')" class="w-full bg-blue-900 text-white py-6 rounded-2xl mt-12 font-black">SUBMIT LEVEL</button>
        </div>
    </div>

    <script>
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active-tab'));
            document.getElementById(tabId).classList.add('active-tab');
        }

        function startLevel(level) {
            document.getElementById('testOverlay').classList.remove('hidden');
            document.getElementById('levelTitle').innerText = "LEVEL 0" + level + ": MISSION START";
            renderPaper();
        }

        function closeTest() {
            document.getElementById('testOverlay').classList.add('hidden');
        }

        function renderPaper() {
            const paper = document.getElementById('paperContent');
            paper.innerHTML = `
                <div class="p-8 border-2 border-slate-100 rounded-[40px] shadow-sm">
                    <p class="font-bold text-xl">Q1. What is the main range of K2 peak?</p>
                    <div class="grid grid-cols-2 gap-4 mt-6">
                        <label class="p-4 border-2 rounded-2xl cursor-pointer hover:bg-blue-50"><input type="radio" name="q1"> Karakoram</label>
                        <label class="p-4 border-2 rounded-2xl cursor-pointer hover:bg-blue-50"><input type="radio" name="q1"> Himalayas</label>
                    </div>
                </div>
            `;
        }
    </script>
</body>
</html>
