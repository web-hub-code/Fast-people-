<html lang="ur" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GB Police Dark Portal | Muhammad Nazim</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&family=Noto+Nastaliq+Urdu:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* Ultra Dark Palette */
            --pure-black: #020617;
            --card-dark: #0f172a;
            --accent-cyan: #22d3ee;
            --accent-purple: #818cf8;
            --text-silver: #94a3b8;
            --text-white: #f8fafc;
            --neon-border: rgba(34, 211, 238, 0.3);
        }

        body {
            font-family: 'Poppins', 'Noto Nastaliq Urdu', sans-serif;
            background-color: var(--pure-black);
            background-image: 
                radial-gradient(at 0% 0%, rgba(129, 140, 248, 0.05) 0px, transparent 50%),
                radial-gradient(at 100% 0%, rgba(34, 211, 238, 0.05) 0px, transparent 50%);
            color: var(--text-white);
            margin: 0;
            padding: 0;
            line-height: 1.6;
        }

        /* Dark Header with Glassmorphism */
        header {
            padding: 60px 20px;
            text-align: center;
            background: rgba(15, 23, 42, 0.8);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid var(--neon-border);
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
        }

        header h1 {
            font-size: 2.8rem;
            margin: 0;
            background: linear-gradient(to right, var(--accent-cyan), var(--accent-purple));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 10px rgba(34, 211, 238, 0.5));
        }

        .status-badge {
            display: inline-block;
            margin-top: 15px;
            padding: 5px 15px;
            background: rgba(34, 211, 238, 0.1);
            border: 1px solid var(--accent-cyan);
            border-radius: 20px;
            font-size: 0.85rem;
            color: var(--accent-cyan);
        }

        .container {
            max-width: 1100px;
            margin: 40px auto;
            padding: 0 20px;
        }

        /* Dark Navigation */
        .section-nav {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-bottom: 50px;
            flex-wrap: wrap;
        }

        .nav-link {
            background: var(--card-dark);
            color: var(--text-silver);
            padding: 12px 25px;
            border-radius: 12px;
            text-decoration: none;
            border: 1px solid rgba(255,255,255,0.05);
            transition: all 0.3s ease;
        }

        .nav-link:hover {
            background: var(--accent-cyan);
            color: var(--pure-black);
            box-shadow: 0 0 20px rgba(34, 211, 238, 0.4);
            transform: translateY(-3px);
        }

        .category-header {
            color: var(--accent-cyan);
            border-right: 4px solid var(--accent-cyan);
            padding-right: 15px;
            margin: 50px 0 25px;
            font-size: 1.8rem;
        }

        /* Dark Mode Cards */
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
        }

        .card {
            background: var(--card-dark);
            border: 1px solid rgba(255,255,255,0.05);
            padding: 25px;
            border-radius: 16px;
            transition: 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .card:hover {
            border-color: var(--accent-cyan);
            background: rgba(30, 41, 59, 0.8);
            box-shadow: 0 10px 40px rgba(0,0,0,0.6);
            transform: scale(1.02);
        }

        .card::before {
            content: '';
            position: absolute;
            top: 0; left: 0; width: 4px; height: 100%;
            background: var(--accent-cyan);
            opacity: 0;
            transition: 0.3s;
        }

        .card:hover::before {
            opacity: 1;
        }

        .q-tag {
            color: var(--accent-purple);
            font-size: 0.75rem;
            font-weight: bold;
            text-transform: uppercase;
            display: block;
            margin-bottom: 10px;
        }

        .question-text {
            font-size: 1.15rem;
            color: var(--text-white);
            margin-bottom: 15px;
            display: block;
        }

        .ans-box {
            background: rgba(34, 211, 238, 0.05);
            color: var(--accent-cyan);
            padding: 10px 15px;
            border-radius: 8px;
            border-right: 3px solid var(--accent-cyan);
            font-weight: 500;
        }

        footer {
            text-align: center;
            padding: 60px;
            border-top: 1px solid rgba(255,255,255,0.05);
            color: var(--text-silver);
            margin-top: 80px;
        }

        /* Custom Scrollbar for Dark Mode */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: var(--pure-black); }
        ::-webkit-scrollbar-thumb { background: var(--card-dark); border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: var(--accent-cyan); }

    </style>
</head>
<body>

<header>
    <h1>GB POLICE PORTAL</h1>
    <div class="status-badge">● Candidate: Muhammad Nazim | Post: Foot Constable</div>
</header>

<div class="container">
    <div class="section-nav">
        <a href="#" class="nav-link">اسلامیات</a>
        <a href="#" class="nav-link">مطالعہ پاکستان</a>
        <a href="#" class="nav-link">جنرل نالج</a>
        <a href="#" class="nav-link">ریاضی</a>
    </div>

    <h2 class="category-header">🌙 اسلامیات (Deep Dark Mode)</h2>
    <div class="grid">
        <div class="card">
            <span class="q-tag">Question</span>
            <span class="question-text">پہلے حافظ قرآن صحابی کا نام بتائیں؟</span>
            <div class="ans-box">حضرت عثمان غنی (R.A)</div>
        </div>

        <div class="card">
            <span class="q-tag">Question</span>
            <span class="question-text">اسلام قبول کرنے والے پہلے رومی صحابی؟</span>
            <div class="ans-box">حضرت صہیب رومی (R.A)</div>
        </div>

        <div class="card">
            <span class="q-tag">Question</span>
            <span class="question-text">غزوہ خندق میں مدینہ کا محاصرہ کتنے دن رہا؟</span>
            <div class="ans-box">30 دن</div>
        </div>
    </div>

    <h2 class="category-header">🇵🇰 مطالعہ پاکستان</h2>
    <div class="grid">
        <div class="card">
            <span class="q-tag">Geography</span>
            <span class="question-text">پاکستان کا کل رقبہ کتنا ہے؟</span>
            <div class="ans-box">796,096 مربع کلومیٹر</div>
        </div>
        <div class="card">
            <span class="q-tag">History</span>
            <span class="question-text">اردو ہندی تنازعہ کب شروع ہوا؟</span>
            <div class="ans-box">1867 میں</div>
        </div>
    </div>
</div>

<footer>
    <p>Created by <b>Prime Solutions</b> | Dark Ops Edition</p>
</footer>

</body>
</html>
