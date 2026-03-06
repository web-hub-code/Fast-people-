<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily Pro | Premium Investment</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background-color: #020617; color: #f1f5f9; font-family: 'Outfit', sans-serif; }
        .glass { background: rgba(30, 41, 59, 0.4); backdrop-filter: blur(16px); border: 1px solid rgba(255, 255, 255, 0.05); }
        .gradient-btn { background: linear-gradient(135deg, #3b82f6, #2563eb); transition: 0.3s; }
        .gradient-btn:active { transform: scale(0.96); }
        .coming-soon { opacity: 0.5; filter: grayscale(1); pointer-events: none; }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signOut, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, collection, addDoc, query, where, updateDoc } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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

        const PKR_RATE = 280;

        onAuthStateChanged(auth, (user) => {
            if(user) {
                document.getElementById('authSec').classList.add('hidden');
                document.getElementById('appBody').classList.remove('hidden');
                
                if(user.email === "admin@apexdaily.com") {
                    document.getElementById('adminView').classList.remove('hidden');
                    loadAdminData();
                } else {
                    document.getElementById('userView').classList.remove('hidden');
                    syncUserData(user.uid);
                }
            }
        });

        // --- USER LOGIC ---
        function syncUserData(uid) {
            onSnapshot(doc(db, "users", uid), (d) => {
                if(d.exists()) {
                    const data = d.data();
                    document.getElementById('uBal').innerText = "$" + data.balance.toFixed(2);
                    document.getElementById('uBalPKR').innerText = "Rs. " + (data.balance * PKR_RATE).toLocaleString();
                }
            });
            loadHistory(uid);
        }

        window.updatePKR = () => {
            const usd = document.getElementById('depAmount').value;
            document.getElementById('pkrLabel').innerText = usd ? `Approx: Rs. ${usd * PKR_RATE}` : "Rs. 0";
        };

        window.applyPromo = async () => {
            const code = document.getElementById('promoCode').value;
            if(!code) return alert("Enter code sweetie!");
            await addDoc(collection(db, "promo_requests"), {
                uid: auth.currentUser.uid,
                email: auth.currentUser.email,
                code: code,
                status: "pending",
                time: new Date()
            });
            alert("Promo request sent to Admin! Bonus will be added after review.");
        };

        // --- ADMIN LOGIC ---
        window.sendBroadcast = async () => {
            const msg = document.getElementById('bcMsg').value;
            await setDoc(doc(db, "settings", "broadcast"), { message: msg, time: new Date() });
            alert("Broadcast sent sweetie!");
        };

        // --- UI ---
        window.showTab = (id) => {
            ['homeTab','walletTab','historyTab','aboutTab'].forEach(t => document.getElementById(t).classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        };

        window.handleAuth = async (type) => {
            const u = document.getElementById('uInp').value;
            const p = document.getElementById('pInp').value;
            const email = u.includes('@') ? u : `${u}@apex.com`;
            try {
                if(type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, p);
                    await setDoc(doc(db, "users", res.user.uid), { username: u, balance: 0, createdAt: new Date() });
                } else { await signInWithEmailAndPassword(auth, email, p); }
            } catch(e) { alert(e.message); }
        };
    </script>
</head>
<body class="pb-24">

    <div id="authSec" class="h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-sm glass p-10 rounded-[2.5rem] text-center border-t-2 border-blue-500/20">
            <h1 class="text-4xl font-black text-blue-500 mb-8 italic">APEXDAILY</h1>
            <input id="uInp" type="text" placeholder="Username" class="w-full p-4 bg-white/5 rounded-2xl mb-4 outline-none border border-white/10">
            <input id="pInp" type="password" placeholder="Password" class="w-full p-4 bg-white/5 rounded-2xl mb-8 outline-none border border-white/10">
            <button onclick="handleAuth('login')" class="w-full gradient-btn py-4 rounded-2xl font-bold">Login</button>
        </div>
    </div>

    <div id="appBody" class="hidden">
        
        <div id="adminView" class="hidden p-6">
            <h1 class="text-2xl font-black text-yellow-500 mb-6 italic">ADMIN BROADCAST</h1>
            <textarea id="bcMsg" placeholder="Sab ko message bhejo sweetie..." class="w-full h-24 bg-white/5 rounded-2xl p-4 outline-none border border-white/10 mb-4"></textarea>
            <button onclick="sendBroadcast()" class="w-full bg-yellow-600 py-3 rounded-xl font-bold">Send Announcement</button>
        </div>

        <div id="userView" class="hidden">
            <div id="homeTab" class="p-6">
                <div class="glass p-8 rounded-[2.5rem] bg-gradient-to-br from-blue-600 to-indigo-900 border-none shadow-2xl mb-8 relative overflow-hidden">
                    <p class="text-[10px] text-blue-100 uppercase tracking-widest font-bold">Trading Capital</p>
                    <h1 id="uBal" class="text-6xl font-black my-2 tracking-tighter">$0.00</h1>
                    <p id="uBalPKR" class="text-xs opacity-70">Rs. 0</p>
                    <button onclick="showTab('walletTab')" class="mt-6 w-full bg-white/20 py-3 rounded-xl font-bold backdrop-blur-md">Add Funds</button>
                </div>

                <div class="glass p-6 rounded-3xl mb-8">
                    <p class="text-[10px] text-gray-500 mb-3 uppercase font-bold">Gift / Promo Code</p>
                    <div class="flex gap-2">
                        <input id="promoCode" type="text" placeholder="Enter Code" class="bg-black/40 p-3 rounded-xl flex-1 text-xs outline-none border border-white/5">
                        <button onclick="applyPromo()" class="bg-blue-600 px-4 rounded-xl text-xs font-bold">Apply</button>
                    </div>
                </div>
            </div>

            <div id="walletTab" class="hidden p-6">
                <h2 class="text-2xl font-black mb-6 italic text-green-400">Secure Deposit</h2>
                <div class="space-y-4 mb-8">
                    <div class="glass p-4 rounded-2xl border-l-4 border-yellow-500">
                        <p class="text-[10px] text-gray-400">JazzCash / SadaPay</p>
                        <p class="font-black text-lg">03705519562</p>
                    </div>
                    <div class="glass p-4 rounded-2xl border-l-4 border-green-500">
                        <p class="text-[10px] text-gray-400">EasyPaisa</p>
                        <p class="font-black text-lg">03379827882</p>
                    </div>
                    <div class="glass p-4 rounded-2xl coming-soon flex justify-between">
                        <p class="text-sm">Bank / Binance</p>
                        <span class="text-[8px] bg-white/10 px-2 py-1 rounded">COMING SOON</span>
                    </div>
                </div>

                <input id="depAmount" oninput="updatePKR()" type="number" placeholder="Amount in USD ($)" class="w-full p-4 bg-white/5 rounded-2xl mb-2 outline-none border border-white/10">
                <p id="pkrLabel" class="text-[10px] text-blue-400 font-bold mb-4 ml-2">Rs. 0</p>
                <input id="depTid" type="text" placeholder="Transaction ID (TID)" class="w-full p-4 bg-white/5 rounded-2xl mb-6 outline-none border border-white/10">
                <button class="w-full gradient-btn py-4 rounded-2xl font-bold">Confirm Deposit</button>
            </div>

            <div id="aboutTab" class="hidden p-6">
                <h2 class="text-2xl font-black mb-6 italic text-blue-400">Trusted Platform</h2>
                <div class="glass p-6 rounded-3xl space-y-4 text-xs leading-relaxed opacity-80">
                    <p><b class="text-blue-400">Who we are:</b> ApexDaily is a certified high-frequency trading firm operating since 2024. We use AI algorithms to generate daily 2.8% profit.</p>
                    <p><b class="text-blue-400">Privacy Policy:</b> Your data is encrypted with SSL-256 bit security. We never share your personal information or wallet details with third parties.</p>
                    <p><b class="text-blue-400">Terms:</b> Minimum withdrawal is $5. Payments are processed within 2-24 hours. Multiple accounts are strictly prohibited.</p>
                </div>
            </div>

            <nav class="fixed bottom-0 left-0 right-0 glass p-6 flex justify-around items-center rounded-t-[2.5rem] z-50">
                <button onclick="showTab('homeTab')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-house"></i><span class="text-[8px]">Home</span></button>
                <button onclick="showTab('walletTab')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-wallet"></i><span class="text-[8px]">Wallet</span></button>
                <button onclick="showTab('aboutTab')" class="flex flex-col items-center gap-1 focus:text-blue-500"><i class="fa-solid fa-shield-halved"></i><span class="text-[8px]">Trust</span></button>
                <button onclick="signOut(auth)" class="flex flex-col items-center gap-1 text-red-500 opacity-50"><i class="fa-solid fa-power-off"></i><span class="text-[8px]">Exit</span></button>
            </nav>
        </div>
    </div>

</body>
</html>
