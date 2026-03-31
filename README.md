<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Elite Prep | Muhammad Nazim</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Noto+Nastaliq+Urdu:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-deep: #050b18;
            --glass-bg: rgba(30, 41, 59, 0.7);
            --accent-neon: #00f2ff;
            --accent-glow: rgba(0, 242, 255, 0.5);
            --text-silver: #cbd5e1;
            --success-neon: #39ff14;
        }

        body {
            font-family: 'Poppins', 'Noto Nastaliq Urdu', sans-serif;
            background-color: var(--bg-deep);
            background-image: radial-gradient(circle at 50% 50%, #1e293b 0%, #050b18 100%);
            color: white;
            margin: 0;
            padding: 0;
            overflow-x: hidden;
        }

        /* Animated Header */
        header {
            height: 300px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: linear-gradient(45deg, rgba(15, 23, 42, 0.9), rgba(5, 11, 24, 0.9));
            border-bottom: 2px solid var(--accent-neon);
            box-shadow: 0 0 30px var(--accent-glow);
            position: relative;
        }

        header h1 {
            font-size: 3rem;
            margin: 0;
            background: linear-gradient(to right, #fff, var(--accent-neon));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 15px var(--accent-glow);
            letter-spacing: 2px;
        }

        .user-info {
            background: rgba(255, 255, 255, 0.1);
            padding: 5px 20px;
            border-radius: 50px;
            border: 1px solid var(--accent-neon);
            margin-top: 15px;
            font-size: 0.9rem;
            backdrop-filter: blur(5px);
        }

        /* Futuristic Nav */
        .section-nav {
            position: sticky;
            top: 20px;
            z-index: 100;
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: -30px auto 50px;
            padding: 10px;
        }

        .nav-btn {
            background: var(--glass-bg);
            color: white;
            padding: 12px 25px;
            border-radius: 12px;
            text-decoration: none;
            border: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(15px);
            transition: 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            font-weight: 500;
        }

        .nav-btn:hover {
            border-color: var(--accent-neon);
            box-shadow: 0 0 20px var(--accent-glow);
            transform: scale(1.1);
            color: var(--accent-neon);
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .category-title {
            font-size: 2rem;
            margin: 60px 0 30px;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .category-title::after {
            content: '';
            height: 2px;
            flex-grow: 1;
            background: linear-gradient(to left, var(--accent-neon), transparent);
        }

        /* Modern Grid & Cards */
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
            gap: 25px;
        }

        .card {
            background: var(--glass-bg);
            padding: 25px;
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            cursor: pointer;
            transition: 0.3s;
            position: relative;
            overflow: hidden;
        }

        .card::before {
            content: '';
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: linear-gradient(135deg, transparent, rgba(0, 242, 255, 0.05));
            pointer-events: none;
        }

        .card:hover {
            transform: translateY(-10px);
            border-color: var(--accent-neon);
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        .q-num {
            color: var(--accent-neon);
            font-size: 0.7rem;
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 10px;
            display: block;
        }

        .question {
            font-size: 1.2rem;
            line-height: 1.8;
            margin-bottom: 15px;
            display: block;
        }

        /* Interactive Answer System */
        .answer {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.5s ease-out, opacity 0.3s;
            opacity: 0;
            color: var(--success-neon);
            background: rgba(57, 255, 20, 0.05);
            border-radius: 10px;
            padding: 0 15px;
            margin-top: 10px;
            display: flex;
            align-items: center;
            height: 50px;
        }

        .card.active .answer {
            max-height: 100px;
            opacity: 1;
            padding: 10px 15px;
            border: 1px dashed var(--success-neon);
        }

        .click-hint {
            font-size: 0.7rem;
            color: var(--text-silver);
            font-style: italic;
            margin-top: 10px;
            display: block;
            text-align: left;
        }

        footer {
            margin-top: 100px;
            padding: 50px;
            background: rgba(0,0,0,0.5);
            text-align: center;
            border-top: 1px solid rgba(255,255,255,0.1);
        }

        .pulse {
            animation: pulse-animation 2s infinite;
        }

        @keyframes pulse-animation {
            0% { box-shadow: 0 0 0 0px rgba(0, 242, 255, 0.4); }
            100% { box-shadow: 0 0 0 20px rgba(0, 242, 255, 0); }
        }
    </style>
</head>
<body>

<header>
    <h1>GB POLICE ELITE</h1>
    <div class="user-info">
        👤 <b>Muhammad Nazim</b> | 📍 Astore | 🎖️ Foot Constable Prep
    </div>
</header>

<div class="container">
    <div class="section-nav">
        <a href="#islamiyat" class="nav-btn">اسلامیات</a>
        <a href="#pakstudies" class="nav-btn">مطالعہ پاکستان</a>
        <a href="#gk" class="nav-btn">جنرل نالج</a>
    </div>

    <h2 id="islamiyat" class="category-title">🌙 اسلامیات</h2>
    <div class="grid">
        <div class="card" onclick="toggleAnswer(this)">
            <span class="q-num">Question 01</span>
            <span class="question">اسلام قبول کرنے والے پہلے رومی صحابی کون تھے؟</span>
            <div class="answer">حضرت صہیب رومی (R.A)</div>
            <span class="click-hint">جواب دیکھنے کے لیے کلک کریں...</span>
        </div>

        <div class="card" onclick="toggleAnswer(this)">
            <span class="q-num">Question 02</span>
            <span class="question">پہلے حافظ قرآن صحابی کا نام بتائیں:</span>
            <div class="answer">حضرت عثمان غنی (R.A)</div>
            <span class="click-hint">جواب دیکھنے کے لیے کلک کریں...</span>
        </div>
        
        <div class="card" onclick="toggleAnswer(this)">
            <span class="q-num">Question 03</span>
            <span class="question">غزوہ خندق میں مدینہ کا محاصرہ کتنے دن رہا؟</span>
            <div class="answer">30 دن</div>
            <span class="click-hint">جواب دیکھنے کے لیے کلک کریں...</span>
        </div>
    </div>

    <h2 id="pakstudies" class="category-title">🇵🇰 مطالعہ پاکستان</h2>
    <div class="grid">
        <div class="card" onclick="toggleAnswer(this)">
            <span class="q-num">Geo-Data</span>
            <span class="question">واخان کی پٹی پاکستان کو کس ملک سے الگ کرتی ہے؟</span>
            <div class="answer">تاجکستان</div>
            <span class="click-hint">جواب دیکھنے کے لیے کلک کریں...</span>
        </div>
        <div class="card" onclick="toggleAnswer(this)">
            <span class="q-num">Constitution</span>
            <span class="question">1973 کے آئین کی تیاری کے لیے کتنے ارکان تھے؟</span>
            <div class="answer">25 ارکان</div>
            <span class="click-hint">جواب دیکھنے کے لیے کلک کریں...</span>
        </div>
    </div>
</div>

<footer>
    <p>Powered by <b>Prime Solutions</b> Portfolio System</p>
    <p style="font-size: 0.8rem; color: var(--accent-neon);">© 2026 Futuristic Recruitment Portal</p>
</footer>

<script>
    function toggleAnswer(card) {
        // Close other cards (optional)
        // document.querySelectorAll('.card').forEach(c => c.classList.remove('active'));
        
        // Toggle current card
        card.classList.toggle('active');
        
        // Update hint text
        const hint = card.querySelector('.click-hint');
        if(card.classList.contains('active')) {
            hint.style.display = 'none';
        } else {
            hint.style.display = 'block';
        }
    }
</script>

</body>
</html>
