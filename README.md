<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Colorful Trading</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        
        body { 
            background: radial-gradient(circle at top left, #0f172a, #020617); 
            color: #f1f5f9; 
            font-family: 'Outfit', sans-serif; 
            overflow-x: hidden;
        }

        /* Animated Neon Border for Balance Card */
        .balance-card {
            background: linear-gradient(135deg, #3b82f6 0%, #a855f7 50%, #ec4899 100%);
            box-shadow: 0 10px 40px rgba(168, 85, 247, 0.4);
            border: 2px solid rgba(255, 255, 255, 0.2);
            position: relative;
            overflow: hidden;
        }

        .balance-card::before {
            content: "";
            position: absolute;
            top: -50%; left: -50%; width: 200%; height: 200%;
            background: conic-gradient(transparent, transparent, transparent, #ffffff);
            animation: rotate 6s linear infinite;
            opacity: 0.2;
        }

        @keyframes rotate { 100% { transform: rotate(360deg); } }

        /* Colorful Glassmorphism */
        .glass-blue { background: rgba(59, 130, 246, 0.15); border: 1px solid rgba(59, 130, 246, 0.3); backdrop-filter: blur(10px); }
        .glass-green { background: rgba(34, 197, 94, 0.15); border: 1px solid rgba(34, 197, 94, 0.3); backdrop-filter: blur(10px); }
        .glass-purple { background: rgba(168, 85, 247, 0.15); border: 1px solid rgba(168, 85, 247, 0.3); backdrop-filter: blur(10px); }
        .glass-pink { background: rgba(236, 72, 153, 0.15); border: 1px solid rgba(236, 72, 153, 0.3); backdrop-filter: blur(10px); }

        .neon-text-blue { text-shadow: 0 0 10px #3b82f6; }
        .neon-text-green { text-shadow: 0 0 10px #22c55e; }

        .btn-gradient {
            background: linear-gradient(90deg, #3b82f6, #8b5cf6, #ec4899);
            background-size: 200% auto;
            transition: 0.5s;
        }
        .btn-gradient:hover { background-position: right center; transform: scale(1.05); }

        /* Floating Nav bar */
        nav {
            background: rgba(15, 23, 42, 0.8);
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
        }
    </style>

    <script type="module">
        // (Firebase Logic remains the same as previous)
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        // ... (Insert your Firebase and logic here sweetie)
    </script>
</head>
<body class="pb-32">

    <div class="fixed top-0 left-1/2 -translate-x-1/2 w-full h-40 bg-blue-600/20 blur-[100px] pointer-events-none"></div>

    <div id="userDash" class="p-6">
        <div class="flex justify-between items-center mb-8">
            <h1 class="text-3xl font-black italic neon-text-blue tracking-tighter">APEX<span class="text-white">PRO</span></h1>
            <div class="w-10 h-10 rounded-full bg-gradient-to-tr from-pink-500 to-yellow-500 p-[2px]">
                <div class="w-full h-full bg-[#020617] rounded-full flex items-center justify-center">
                    <i class="fa-solid fa-user text-xs"></i>
                </div>
            </div>
        </div>

        <div class="balance-card p-8 rounded-[2.5rem] mb-10 text-white relative">
            <div class="relative z-10">
                <p class="text-[10px] uppercase font-bold tracking-[3px] mb-1 opacity-90">Net Worth</p>
                <h1 id="uBal" class="text-6xl font-black mb-1 tracking-tighter">$1,250.00</h1>
                <p class="text-xs font-medium opacity-80">≈ Rs. 350,000</p>
                
                <div class="flex gap-4 mt-8">
                    <button class="flex-1 bg-white text-blue-600 py-4 rounded-2xl font-black text-sm shadow-xl shadow-white/10 active:scale-95 transition">DEPOSIT</button>
                    <button class="flex-1 bg-black/30 backdrop-blur-md py-4 rounded-2xl font-black text-sm border border-white/20 active:scale-95 transition">WITHDRAW</button>
                </div>
            </div>
        </div>

        <div class="grid grid-cols-2 gap-4 mb-10">
            <div class="glass-green p-5 rounded-3xl">
                <i class="fa-solid fa-chart-line text-green-400 mb-3"></i>
                <p class="text-[10px] text-gray-400 uppercase font-bold">Daily Profit</p>
                <h4 class="text-xl font-black text-green-400">+2.85%</h4>
            </div>
            <div class="glass-purple p-5 rounded-3xl">
                <i class="fa-solid fa-gift text-purple-400 mb-3"></i>
                <p class="text-[10px] text-gray-400 uppercase font-bold">Bonus Won</p>
                <h4 class="text-xl font-black text-purple-400">$15.00</h4>
            </div>
        </div>

        <div class="flex items-center gap-3 mb-6">
            <div class="h-[2px] w-8 bg-blue-500"></div>
            <h2 class="text-lg font-black uppercase tracking-widest text-blue-400">Trading Plans</h2>
        </div>

        <div class="space-y-4">
            <div class="glass-blue p-6 rounded-[2rem] flex justify-between items-center border-l-8 border-blue-500">
                <div>
                    <h3 class="text-2xl font-black">Plan $5</h3>
                    <p class="text-[10px] text-blue-300 font-bold">Daily: Rs. 1,400 returns</p>
                </div>
                <button class="btn-gradient px-6 py-3 rounded-xl font-bold text-xs">BUY NOW</button>
            </div>

            <div class="glass-pink p-6 rounded-[2rem] flex justify-between items-center border-l-8 border-pink-500">
                <div>
                    <h3 class="text-2xl font-black">Plan $20</h3>
                    <p class="text-[10px] text-pink-300 font-bold">Daily: Rs. 5,600 returns</p>
                </div>
                <button class="btn-gradient px-6 py-3 rounded-xl font-bold text-xs">BUY NOW</button>
            </div>
        </div>
    </div>

    <nav class="fixed bottom-0 left-0 right-0 p-5 flex justify-around items-center rounded-t-[3rem] z-50">
        <button class="text-blue-500 flex flex-col items-center gap-1">
            <i class="fa-solid fa-house-chimney text-xl"></i>
            <span class="text-[9px] font-bold">Home</span>
        </button>
        <button class="text-gray-500 hover:text-purple-500 flex flex-col items-center gap-1 transition">
            <i class="fa-solid fa-wallet text-xl"></i>
            <span class="text-[9px] font-bold">Wallet</span>
        </button>
        <button class="text-gray-500 hover:text-green-500 flex flex-col items-center gap-1 transition">
            <i class="fa-solid fa-clock-rotate-left text-xl"></i>
            <span class="text-[9px] font-bold">History</span>
        </button>
        <button class="text-gray-500 hover:text-pink-500 flex flex-col items-center gap-1 transition">
            <i class="fa-solid fa-headset text-xl"></i>
            <span class="text-[9px] font-bold">Support</span>
        </button>
    </nav>

</body>
</html>
