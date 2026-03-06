<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ApexDaily | Pro Mobile Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: white; font-family: 'Outfit', sans-serif; -webkit-tap-highlight-color: transparent; overflow-x: hidden; }
        
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.1); }
        .neon-card { background: linear-gradient(135deg, #0ea5e9, #a855f7, #ec4899); box-shadow: 0 15px 40px rgba(168, 85, 247, 0.3); }
        .btn-modern { background: linear-gradient(90deg, #3b82f6, #d946ef); transition: 0.3s; }
        .btn-modern:active { transform: scale(0.95); }
        
        input, select { background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(255, 255, 255, 0.1); outline: none; transition: 0.3s; }
        input:focus { border-color: #0ea5e9; }

        .tab-content { display: none; animation: slideUp 0.4s ease; }
        .tab-content.active { display: block; }
        @keyframes slideUp { from { transform: translateY(20px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, RecaptchaVerifier, signInWithPhoneNumber, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyDt3ChZHyDdtM4Ir1oXRZJUywcOiV30Wtg",
            authDomain: "investment-84f4e.firebaseapp.com",
            projectId: "investment-84f4e",
            storageBucket: "investment-84f4e.firebasestorage.app",
            messagingSenderId: "975293889308",
            appId: "1:975293889308:web:6d034a99cc966c75ff58d9"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        auth.useDeviceLanguage();

        // --- SECRET ADMIN CLICK ---
        let taps = 0;
        window.handleSecret = () => {
            taps++;
            if(taps === 3) {
                const key = prompt("Enter Secret Admin Code:");
                if(key === "786786") { alert("Admin Panel Unlocked Sweetie! 🤫"); window.isAdmin = true; }
                taps = 0;
            }
        };

        // --- OTP LOGIC ---
        window.setupCaptcha = () => {
            window.recaptchaVerifier = new RecaptchaVerifier(auth, 'recaptcha-container', {
                'size': 'invisible'
            });
        };

        window.sendSMS = async () => {
            const phone = document.getElementById('country').value + document.getElementById('phone').value;
            if(!window.recaptchaVerifier) window.setupCaptcha();
            
            try {
                const result = await signInWithPhoneNumber(auth, phone, window.recaptchaVerifier);
                window.confirmation = result;
                document.getElementById('authBox').classList.add('hidden');
                document.getElementById('otpBox').classList.remove('hidden');
            } catch (err) { alert("SMS Error: " + err.message); }
        };

        window.verifyAndLogin = async () => {
            const code = document.getElementById('digit').value;
            try {
                const result = await window.confirmation.confirm(code);
                const user = result.user;
                await setDoc(doc(db, "users", user.uid), { 
                    username: document.getElementById('user').value || "Investor",
                    phone: user.phoneNumber,
                    balance: 0 
                }, { merge: true });
            } catch (err) { alert("Invalid Code Sweetie!"); }
        };

        // --- DATA SYNC ---
        onAuthStateChanged(auth, (u) => {
            if(u) {
                document.getElementById('authScreen').classList.add('hidden');
                document.getElementById('mainScreen').classList.remove('hidden');
                onSnapshot(doc(db, "users", u.uid), (d) => {
                    if(d.exists()){
                        document.getElementById('uBal').innerText = "Rs. " + d.data().balance.toLocaleString();
                        document.getElementById('uName').innerText = d.data().username;
                    }
                });
            }
        });

        window.showTab = (id) => {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.remove('active'));
            document.getElementById(id).classList.add('active');
        };
    </script>
</head>
<body>

    <div id="recaptcha-container"></div>

    <div id="authScreen" class="min-h-screen flex flex-col items-center justify-center p-8">
        <h1 onclick="handleSecret()" class="text-5xl font-black italic mb-2 bg-clip-text text-transparent bg-gradient-to-r from-sky-400 to-pink-500">APEXDAILY</h1>
        <p class="text-[10px] tracking-[0.5em] text-gray-500 uppercase mb-12">Next-Gen PKR Trading</p>

        <div id="authBox" class="w-full max-w-sm glass p-8 rounded-[2.5rem] space-y-4">
            <input id="user" type="text" placeholder="Full Name" class="w-full p-4 rounded-2xl text-sm">
            <div class="flex gap-2">
                <select id="country" class="p-4 rounded-2xl text-sm w-24">
                    <option value="+92">+92</option>
                    <option value="+971">+971</option>
                </select>
                <input id="phone" type="number" placeholder="3001234567" class="flex-1 p-4 rounded-2xl text-sm">
            </div>
            <button onclick="sendSMS()" class="w-full btn-modern py-4 rounded-2xl font-black text-sm shadow-xl">SEND VERIFICATION</button>
        </div>

        <div id="otpBox" class="hidden w-full max-w-sm glass p-8 rounded-[2.5rem] text-center">
            <p class="text-xs text-gray-400 mb-6 font-bold">ENTER 6-DIGIT SMS CODE</p>
            <input id="digit" type="number" placeholder="000000" class="w-full p-4 bg-white/5 rounded-2xl text-center text-2xl font-black tracking-[0.5em] mb-6">
            <button onclick="verifyAndLogin()" class="w-full btn-modern py-4 rounded-2xl font-black">VERIFY & ENTER</button>
        </div>
    </div>

    <div id="mainScreen" class="hidden min-h-screen pb-24">
        <div class="p-6 flex justify-between items-center sticky top-0 bg-[#020617]/80 backdrop-blur-xl z-50">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl neon-card flex items-center justify-center font-black">A</div>
                <div>
                    <h2 class="text-sm font-black tracking-tight">@<span id="uName">User</span> <i class="fa-solid fa-certificate text-sky-400 ml-1 text-[10px]"></i></h2>
                    <p class="text-[8px] text-green-500 font-bold uppercase tracking-widest">Verified Account</p>
                </div>
            </div>
            <button onclick="location.reload()" class="w-10 h-10 glass rounded-xl flex items-center justify-center text-red-500"><i class="fa-solid fa-power-off"></i></button>
        </div>

        <div id="homeTab" class="tab-content active p-6">
            <div class="neon-card p-8 rounded-[3rem] relative overflow-hidden mb-8">
                <p class="text-[10px] font-bold opacity-80 uppercase tracking-widest">Active PKR Balance</p>
                <h1 id="uBal" class="text-5xl font-black my-3 tracking-tighter">Rs. 0</h1>
                <div class="flex gap-3 mt-8">
                    <button onclick="showTab('walletTab')" class="flex-1 bg-white text-blue-600 py-3.5 rounded-2xl font-black text-xs">RECHARGE</button>
                    <button onclick="showTab('walletTab')" class="flex-1 bg-black/20 py-3.5 rounded-2xl font-black text-xs border border-white/20">WITHDRAW</button>
                </div>
            </div>

            <div class="glass p-6 rounded-3xl mb-8 flex justify-between items-center border-l-4 border-sky-500">
                <div><p class="text-[10px] text-gray-500 font-bold">NEXT PROFIT IN</p><h3 class="text-xl font-black text-sky-400">23:59:45</h3></div>
                <i class="fa-solid fa-clock-rotate-left text-2xl opacity-20"></i>
            </div>

            <h3 class="text-xs font-black text-gray-500 uppercase tracking-widest mb-4 ml-2">Investment Plans</h3>
            <div class="space-y-4">
                <div class="glass p-6 rounded-[2.5rem] flex justify-between items-center border border-white/5">
                    <div><h4 class="font-black text-lg">Daily Starter</h4><p class="text-[10px] text-green-400">Rs. 200 Plan (Daily +25)</p></div>
                    <button class="btn-modern px-5 py-2.5 rounded-xl text-[10px] font-black">BUY</button>
                </div>
                <div class="glass p-6 rounded-[2.5rem] flex justify-between items-center border border-white/5">
                    <div><h4 class="font-black text-lg">Pro Trader</h4><p class="text-[10px] text-green-400">Rs. 500 Plan (Daily +65)</p></div>
                    <button class="btn-modern px-5 py-2.5 rounded-xl text-[10px] font-black">BUY</button>
                </div>
            </div>
        </div>

        <div id="walletTab" class="tab-content p-6">
            <h2 class="text-2xl font-black italic mb-8">Financial Hub</h2>
            <div class="glass p-6 rounded-[2rem] mb-6 border-l-4 border-yellow-500">
                <p class="text-[10px] text-gray-400 font-bold mb-1 uppercase">JazzCash / SadaPay</p>
                <div class="flex justify-between items-center"><span class="font-black text-lg">03705519562</span><i class="fa-solid fa-bolt text-yellow-500"></i></div>
            </div>
            <div class="glass p-8 rounded-[2.5rem]">
                <input type="number" placeholder="Amount (PKR)" class="w-full p-4 rounded-2xl mb-4 text-sm">
                <input type="text" placeholder="TID Number" class="w-full p-4 rounded-2xl mb-8 text-sm">
                <button class="w-full btn-modern py-4 rounded-2xl font-black shadow-lg">SUBMIT DEPOSIT</button>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 glass p-5 flex justify-around items-center rounded-t-[3rem] z-50 border-t border-white/10">
            <button onclick="showTab('homeTab')" class="flex flex-col items-center gap-1 focus:text-sky-400"><i class="fa-solid fa-house-chimney text-xl"></i><span class="text-[8px] font-bold">HOME</span></button>
            <button onclick="showTab('walletTab')" class="flex flex-col items-center gap-1 focus:text-sky-400"><i class="fa-solid fa-wallet text-xl"></i><span class="text-[8px] font-bold">WALLET</span></button>
            <button class="flex flex-col items-center gap-1 focus:text-sky-400"><i class="fa-solid fa-shield-halved text-xl"></i><span class="text-[8px] font-bold">TRUST</span></button>
            <button class="flex flex-col items-center gap-1 focus:text-sky-400"><i class="fa-solid fa-headset text-xl"></i><span class="text-[8px] font-bold">HELP</span></button>
        </nav>
    </div>

</body>
</html>
