<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>
    <title>PakGold | Cloud Mining Node</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap');
        body { font-family: 'Inter', sans-serif; background: #010409; color: white; padding-bottom: 100px; }
        .cloud-card { background: #0d1117; border: 1px solid #30363d; border-radius: 20px; overflow: hidden; transition: 0.3s; }
        .cloud-card:hover { border-color: #fbbf24; transform: translateY(-5px); }
        .btn-buy { background: #fbbf24; color: #000; font-weight: 800; border-radius: 12px; transition: 0.2s; }
        .btn-buy:active { transform: scale(0.95); }
        .page { display: none; animation: fadeIn 0.4s ease; }
        .active-page { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        nav { background: rgba(13, 17, 23, 0.95); backdrop-filter: blur(10px); border-top: 1px solid #30363d; }
    </style>
</head>
<body>

    <header class="p-5 flex justify-between items-center border-b border-[#30363d]">
        <div>
            <h1 class="text-xl font-black text-yellow-500 italic">PakGold</h1>
            <p class="text-[8px] uppercase tracking-widest opacity-50">CloudSky Infrastructure</p>
        </div>
        <div class="text-right">
            <p class="text-[9px] font-bold opacity-60">BALANCE</p>
            <p id="top-bal" class="text-sm font-black text-yellow-500">₨ 0.00</p>
        </div>
    </header>

    <main id="app-ui" class="p-4 space-y-6">
        
        <div id="p-home" class="page active-page space-y-4">
            <div class="h-40 rounded-3xl bg-gradient-to-br from-yellow-500 to-yellow-800 p-6 flex flex-col justify-end shadow-2xl">
                <h2 class="text-2xl font-black text-black leading-none">START MINING<br>TODAY</h2>
                <p class="text-[9px] font-bold text-black/60 mt-2 uppercase">Official Prime Solutions Node</p>
            </div>

            <div class="flex gap-2 p-1 bg-white/5 rounded-xl">
                <button onclick="renderCloudPlans('normal')" id="btn-n" class="flex-1 py-3 text-[10px] font-black uppercase rounded-lg bg-yellow-500 text-black">Standard</button>
                <button onclick="renderCloudPlans('vip')" id="btn-v" class="flex-1 py-3 text-[10px] font-black uppercase rounded-lg opacity-40">VIP Nodes</button>
            </div>

            <div id="machine-grid" class="grid grid-cols-1 gap-4">
                </div>
        </div>

        <div id="p-wallet" class="page space-y-4 text-center">
            <div class="cloud-card p-6">
                <h3 class="text-yellow-500 font-bold mb-4">DEPOSIT FUNDS</h3>
                <p class="text-[10px] mb-2 opacity-60">Send PKR to accounts below</p>
                <div class="bg-white/5 p-4 rounded-xl text-left text-xs mb-4">
                    <p><b>EasyPaisa:</b> 03379827882</p>
                    <p><b>JazzCash:</b> 03705519562</p>
                </div>
                <input type="number" id="dep-amt" placeholder="Amount Sent" class="w-full p-4 bg-black border border-[#30363d] rounded-xl mb-3 outline-none">
                <button onclick="alert('Proof Submitted!')" class="w-full p-4 btn-buy uppercase text-xs">Confirm Payment</button>
            </div>
        </div>
    </main>

    <nav class="fixed bottom-0 w-full p-4 flex justify-around items-center z-[5000]">
        <button onclick="changePage('home')" class="flex flex-col items-center gap-1"><span class="text-xl">⚡</span><span class="text-[8px] font-bold">MINING</span></button>
        <button onclick="changePage('wallet')" class="flex flex-col items-center gap-1 opacity-40"><span class="text-xl">💰</span><span class="text-[8px] font-bold">RECHARGE</span></button>
        <button onclick="location.reload()" class="flex flex-col items-center gap-1 opacity-40"><span class="text-xl">👤</span><span class="text-[8px] font-bold">ME</span></button>
    </nav>

    <script>
        // --- MACHINE GENERATION (15+5) ---
        const CLOUD_MACHINES = [];
        for(let i=1; i<=15; i++) {
            let cost = 200 + (i-1)*1500;
            CLOUD_MACHINES.push({ id: `CLOUD-V${i}`, price: cost, daily: (cost*0.1).toFixed(0), total: (cost*3).toFixed(0) });
        }

        function renderCloudPlans(type) {
            const grid = document.getElementById('machine-grid');
            grid.innerHTML = '';
            
            CLOUD_MACHINES.forEach(m => {
                grid.innerHTML += `
                <div class="cloud-card">
                    <div class="p-4 flex justify-between items-center border-b border-white/5">
                        <span class="text-[10px] font-black text-yellow-500 uppercase">${m.id} MACHINE</span>
                        <span class="text-[9px] bg-green-500/20 text-green-500 px-2 py-1 rounded-md">ACTIVE NOW</span>
                    </div>
                    <div class="p-5 grid grid-cols-2 gap-4">
                        <div><p class="text-[8px] opacity-40">DAILY PROFIT</p><p class="font-bold text-sm">₨ ${m.daily}</p></div>
                        <div><p class="text-[8px] opacity-40">TOTAL REVENUE</p><p class="font-bold text-sm">₨ ${m.total}</p></div>
                        <div><p class="text-[8px] opacity-40">PRICE</p><p class="font-black text-lg text-yellow-500">₨ ${m.price}</p></div>
                        <div class="flex items-end"><button class="btn-buy w-full py-2 text-[10px] uppercase">Rent Now</button></div>
                    </div>
                </div>`;
            });
        }

        function changePage(p) {
            document.querySelectorAll('.page').forEach(pg => pg.classList.remove('active-page'));
            document.getElementById('p-' + p).classList.add('active-page');
        }

        window.onload = () => { renderCloudPlans('normal'); }
    </script>
</body>
</html>
