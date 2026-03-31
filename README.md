<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Ultimate Portal | Muhammad Nazim</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Noto+Nastaliq+Urdu:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #f8fafc;
            --card: #ffffff;
            --text: #1e293b;
            --accent: #0ea5e9;
            --border: #e2e8f0;
            --nav: rgba(255, 255, 255, 0.95);
        }

        [data-theme="dark"] {
            --bg: #020617;
            --card: #0f172a;
            --text: #f1f5f9;
            --accent: #38bdf8;
            --border: #1e293b;
            --nav: rgba(15, 23, 42, 0.95);
        }

        body {
            font-family: 'Poppins', 'Noto Nastaliq Urdu', sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            transition: 0.4s;
        }

        /* Modern Navigation */
        header {
            position: sticky;
            top: 0;
            background: var(--nav);
            backdrop-filter: blur(15px);
            padding: 10px 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
            z-index: 1000;
        }

        .search-box {
            background: var(--bg);
            border: 1px solid var(--border);
            padding: 8px 15px;
            border-radius: 50px;
            width: 250px;
            outline: none;
            color: var(--text);
        }

        /* Hero Section */
        .hero {
            background: linear-gradient(135deg, var(--accent), #6366f1);
            padding: 50px 20px;
            text-align: center;
            color: white;
            border-radius: 0 0 40px 40px;
        }

        .container { max-width: 1200px; margin: 0 auto; padding: 20px; }

        .section-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin: 40px 0 20px;
            border-bottom: 2px solid var(--accent);
            padding-bottom: 10px;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
            gap: 20px;
        }

        .card {
            background: var(--card);
            padding: 20px;
            border-radius: 20px;
            border: 1px solid var(--border);
            cursor: pointer;
            transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
        }

        .card:hover {
            transform: translateY(-5px);
            border-color: var(--accent);
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        .category-tag {
            font-size: 0.7rem;
            background: var(--accent);
            color: white;
            padding: 2px 10px;
            border-radius: 50px;
            position: absolute;
            top: -10px;
            right: 20px;
        }

        .question { display: block; margin-top: 10px; font-size: 1.1rem; }

        .answer {
            max-height: 0;
            overflow: hidden;
            transition: 0.4s;
            color: #10b981;
            font-weight: bold;
            background: rgba(16, 185, 129, 0.05);
            border-radius: 10px;
        }

        .card.active .answer {
            max-height: 200px;
            padding: 15px;
            margin-top: 15px;
            border: 1px solid rgba(16, 185, 129, 0.2);
        }

        .math-symbol {
            background: var(--card);
            border: 1px solid var(--border);
            padding: 15px;
            border-radius: 15px;
            text-align: center;
        }

        .math-symbol b { font-size: 1.8rem; color: var(--accent); display: block; }

        .theme-btn {
            background: var(--accent);
            color: white;
            border: none;
            padding: 8px 18px;
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
        }

        footer { text-align: center; padding: 50px; opacity: 0.5; }

        /* Login Overlay */
        #login-box {
            position: fixed;
            inset: 0;
            background: var(--bg);
            z-index: 2000;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    </style>
</head>
<body data-theme="light">

<div id="login-box">
    <div style="background:var(--card); padding:40px; border-radius:30px; text-align:center; border:1px solid var(--border); width:350px;">
        <h2 style="color:var(--accent)">GB Master Portal</h2>
        <input type="text" id="nameInput" placeholder="اپنا نام لکھیں..." style="width:100%; padding:12px; margin:20px 0; border-radius:10px; border:1px solid var(--border);">
        <button class="theme-btn" style="width:100%" onclick="login()">داخل ہوں</button>
    </div>
</div>

<div id="app" style="display:none">
    <header>
        <div id="user-info" style="font-weight:bold"></div>
        <input type="text" class="search-box" id="search" placeholder="سوال تلاش کریں..." onkeyup="searchQ()">
        <button class="theme-btn" onclick="toggleTheme()">🌓 موڈ</button>
    </header>

    <div class="hero">
        <h1 style="margin:0">GB Police Exam Master</h1>
        <p>تیاری کریں اور کامیابی حاصل کریں</p>
    </div>

    <div class="container">
        <div class="section-header">
            <h2 style="margin:0">🧠 ذہانت اور منطق (IQ & Logic)</h2>
        </div>
        <div class="grid" id="iq-grid">
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="category-tag">IQ</span>
                <span class="question">اگر ایک قطار میں آپ دونوں طرف سے نویں (9th) نمبر پر ہیں، تو قطار میں کل کتنے لوگ ہیں؟</span>
                <div class="answer">17 لوگ (8+1+8)</div>
            </div>
            <div class="card" onclick="this.classList.toggle('active')">
                <span class="category-tag">IQ</span>
                <span class="question">پاکستان کا سب سے بڑا ریلوے اسٹیشن کون سا ہے؟</span>
                <div class="answer">لاہور جنکشن</div>
            </div>
        </div>

        <div class="section-header">
            <h2 style="margin:0">📚 تمام تصاویر کا ڈیٹا</h2>
        </div>
        <div class="grid" id="main-grid">
            </div>

        <div class="section-header">
            <h2 style="margin:0">📐 ریاضی کی اہم علامات</h2>
        </div>
        <div class="grid" id="math-grid" style="grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));">
            </div>
    </div>

    <footer>
        <p>Developed by <b>Prime Solutions</b> | Muhammad Nazim</p>
    </footer>
</div>

<script>
    const allData = [
        // Image Data
        { c: "GK", q: "K2 کی اونچائی کتنی ہے؟", a: "8611 میٹر" },
        { c: "GB", q: "ضلع غذر کا کل رقبہ کتنا ہے؟", a: "9,635 مربع کلومیٹر" },
        { c: "History", q: "اردو ہندی تنازعہ کب شروع ہوا؟", a: "1867 میں" },
        { c: "Islamiat", q: "زکوٰۃ و عشر آرڈیننس کب نافذ ہوا؟", a: "20 جون 1980" },
        { c: "History", q: "شاہِ ایران نے پہلی مرتبہ پاکستان کا دورہ کب کیا؟", a: "1950 میں" },
        { c: "GK", q: "واخان کی پٹی پاکستان کو کس سے الگ کرتی ہے؟", a: "تاجکستان" },
        { c: "GB", q: "گلگت بلتستان کے کل کتنے اضلاع ہیں؟", a: "14 اضلاع" },
        { c: "Islamiat", q: "فتح مکہ پر آپ ﷺ نے کونسی سورہ تلاوت فرمائی؟", a: "سورہ الفتح" },
        { c: "GK", q: "پاکستان UN کا رکن کب بنا؟", a: "30 ستمبر 1947" },
        { c: "GK", q: "دنیا کا سب سے بڑا اسلامی ملک (رقبہ)؟", a: "قازقستان" },
        { c: "Islamiat", q: "پہلے حافظ قرآن صحابی کون تھے؟", a: "حضرت عثمان غنی (R.A)" },
        { c: "History", q: "پاکستان کا پہلا مارشل لاء کب لگا؟", a: "7 اکتوبر 1958" },
        // Extra Questions added by Gemini
        { c: "Secret", q: "پولیس کا نصب العین (Motto) کیا ہے؟", a: "خدمت اور تحفظ" },
        { c: "Secret", q: "پنجاب پولیس کا قیام کب عمل میں آیا؟", a: "1861 میں" },
        { c: "Secret", q: "آبادی کے لحاظ سے پاکستان کا سب سے بڑا شہر؟", a: "کراچی" }
    ];

    const mathSymbols = [
        { s: "∴", m: "لہٰذا" }, { s: "∵", m: "کیونکہ" }, { s: "≅", m: "مماثل ہے" },
        { s: "∝", m: "تناسب" }, { s: "∞", m: "لامحدود" }, { s: "∈", m: "رکن ہے" },
        { s: "∑", m: "مجموعہ (Sigma)" }, { s: "√", m: "جزر (Square Root)" }
    ];

    function login() {
        const name = document.getElementById('nameInput').value;
        if(name) {
            localStorage.setItem('portal_user', name);
            renderUI(name);
        }
    }

    function renderUI(name) {
        document.getElementById('login-box').style.display = 'none';
        document.getElementById('app').style.display = 'block';
        document.getElementById('user-info').innerText = `👤 ${name}`;
        
        const mainGrid = document.getElementById('main-grid');
        allData.forEach(item => {
            mainGrid.innerHTML += `
                <div class="card" data-q="${item.q}">
                    <span class="category-tag">${item.c}</span>
                    <span class="question">${item.q}</span>
                    <div class="answer">${item.a}</div>
                </div>`;
        });
        
        const mathGrid = document.getElementById('math-grid');
        mathSymbols.forEach(item => {
            mathGrid.innerHTML += `<div class="math-symbol"><b>${item.s}</b><small>${item.m}</small></div>`;
        });

        // Click to toggle
        document.querySelectorAll('.card').forEach(card => {
            card.onclick = () => card.classList.toggle('active');
        });
    }

    function searchQ() {
        const val = document.getElementById('search').value.toLowerCase();
        document.querySelectorAll('.card').forEach(card => {
            const text = card.getAttribute('data-q').toLowerCase();
            card.style.display = text.includes(val) ? 'block' : 'none';
        });
    }

    function toggleTheme() {
        const body = document.body;
        body.setAttribute('data-theme', body.getAttribute('data-theme') === 'light' ? 'dark' : 'light');
    }

    window.onload = () => {
        const saved = localStorage.getItem('portal_user');
        if(saved) renderUI(saved);
    }
</script>

</body>
</html>
