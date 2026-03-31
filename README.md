<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Exam Master | Muhammad Nazim</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Noto+Nastaliq+Urdu:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Light Mode Colors */
            --bg: #f8fafc;
            --card: #ffffff;
            --text: #1e293b;
            --accent: #0ea5e9;
            --border: #e2e8f0;
            --success: #22c55e;
            --nav-bg: rgba(255, 255, 255, 0.8);
        }

        [data-theme="dark"] {
            /* Dark Mode Colors */
            --bg: #0f172a;
            --card: #1e293b;
            --text: #f1f5f9;
            --accent: #38bdf8;
            --border: #334155;
            --nav-bg: rgba(15, 23, 42, 0.8);
        }

        body {
            font-family: 'Poppins', 'Noto Nastaliq Urdu', sans-serif;
            background-color: var(--bg);
            color: var(--text);
            transition: all 0.4s ease;
            margin: 0;
            padding: 0;
            line-height: 1.7;
        }

        /* Floating Header */
        header {
            position: sticky;
            top: 0;
            z-index: 1000;
            background: var(--nav-bg);
            backdrop-filter: blur(15px);
            border-bottom: 1px solid var(--border);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo { font-weight: 700; font-size: 1.2rem; color: var(--accent); }

        /* Theme Toggle Button */
        #theme-toggle {
            background: var(--accent);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        .container {
            max-width: 1200px;
            margin: 40px auto;
            padding: 0 20px;
        }

        /* Modern Hero Section */
        .hero {
            text-align: center;
            padding: 60px 20px;
            background: linear-gradient(135deg, var(--accent), #6366f1);
            border-radius: 30px;
            color: white;
            margin-bottom: 50px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }

        /* Section Cards */
        .section-group {
            margin-bottom: 60px;
        }

        .section-title {
            font-size: 1.8rem;
            margin-bottom: 25px;
            display: flex;
            align-items: center;
            gap: 10px;
            color: var(--accent);
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
            gap: 20px;
        }

        .card {
            background: var(--card);
            border: 1px solid var(--border);
            padding: 25px;
            border-radius: 20px;
            cursor: pointer;
            transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05);
        }

        .card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 25px -5px rgba(0,0,0,0.1);
            border-color: var(--accent);
        }

        .q-label { font-size: 0.8rem; color: var(--accent); font-weight: 600; }
        .question { display: block; margin: 10px 0; font-size: 1.1rem; }
        
        .answer {
            max-height: 0;
            overflow: hidden;
            transition: 0.4s;
            color: var(--success);
            font-weight: 700;
            padding: 0 10px;
            border-right: 3px solid var(--success);
            margin-top: 5px;
        }

        .card.active .answer {
            max-height: 100px;
            padding: 10px;
        }

        /* Math Symbol Grid */
        .math-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(110px, 1fr));
            gap: 15px;
        }

        .math-item {
            background: var(--card);
            border: 1px solid var(--border);
            padding: 20px;
            text-align: center;
            border-radius: 15px;
        }

        .math-item b { font-size: 1.5rem; color: var(--accent); }

        footer {
            text-align: center;
            padding: 60px;
            border-top: 1px solid var(--border);
            color: var(--text);
            opacity: 0.6;
        }

    </style>
</head>
<body data-theme="light">

<header>
    <div class="logo">PRIME SOLUTIONS | GB POLICE</div>
    <button id="theme-toggle" onclick="toggleTheme()">🌓 موڈ بدلیں</button>
</header>

<div class="container">
    <div class="hero">
        <h1 style="margin:0">تیاری مرکز (Exam Hub)</h1>
        <p>امیدوار: محمد ناظم | پوسٹ: فٹ کانسٹیبل</p>
    </div>

    <div class="section-group">
        <h2 class="section-title">⛰️ گلگت بلتستان اسپیشل</h2>
        <div class="grid">
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">GK</span>
                <span class="question">K2 کی اونچائی کتنی ہے؟</span>
                <div class="answer">8611 میٹر</div>
            </div>
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">GB</span>
                <span class="question">ضلع غذر کا کل رقبہ کتنا ہے؟</span>
                <div class="answer">9635 مربع کلومیٹر</div>
            </div>
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">History</span>
                <span class="question">گلگت بلتستان کے کل کتنے اضلاع ہیں؟</span>
                <div class="answer">14 اضلاع</div>
            </div>
        </div>
    </div>

    <div class="section-group">
        <h2 class="section-title">📐 ریاضی کی علامات</h2>
        <div class="math-grid">
            <div class="math-item"><b>∴</b><br>لہٰذا</div>
            <div class="math-item"><b>∵</b><br>کیونکہ</div>
            <div class="math-item"><b>≅</b><br>مماثل ہے</div>
            <div class="math-item"><b>∞</b><br>لامحدود</div>
            <div class="math-item"><b>∝</b><br>تناسب</div>
            <div class="math-item"><b>∈</b><br>رکن ہے</div>
        </div>
    </div>

    <div class="section-group">
        <h2 class="section-title">🌍 مطالعہ پاکستان و جنرل نالج</h2>
        <div class="grid">
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">Pak Study</span>
                <span class="question">اردو کس زبان کا لفظ ہے؟</span>
                <div class="answer">ترکی (لشکر)</div>
            </div>
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">History</span>
                <span class="question">پاکستان کب UN کا رکن بنا؟</span>
                <div class="answer">30 ستمبر 1947</div>
            </div>
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">Religion</span>
                <span class="question">فتحِ مکہ پر کونسی سورہ تلاوت کی گئی؟</span>
                <div class="answer">سورہ الفتح</div>
            </div>
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">Current</span>
                <span class="question">پاکستان کی آبادی میں اضافے کی شرح؟</span>
                <div class="answer">2.8 فیصد</div>
            </div>
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">GK</span>
                <span class="question">دنیا کا سب سے بڑا اسلامی ملک (رقبہ)؟</span>
                <div class="answer">قازقستان</div>
            </div>
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="q-label">GK</span>
                <span class="question">OIC کا ہیڈ کوارٹر کہاں ہے؟</span>
                <div class="answer">جدہ (سعودی عرب)</div>
            </div>
        </div>
    </div>
</div>

<footer>
    <p>Developed by Prime Solutions | © 2026 Muhammad Nazim Portfolio</p>
</footer>

<script>
    function toggleTheme() {
        const body = document.body;
        const currentTheme = body.getAttribute('data-theme');
        const newTheme = currentTheme === 'light' ? 'dark' : 'light';
        body.setAttribute('data-theme', newTheme);
        
        // Save preference (optional but good)
        localStorage.setItem('theme', newTheme);
    }

    // Check for saved theme
    const savedTheme = localStorage.getItem('theme');
    if (savedTheme) {
        document.body.setAttribute('data-theme', savedTheme);
    }
</script>

</body>
</html>
