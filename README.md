<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily Pro | Fixed Auth</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, onSnapshot, collection, query, where } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

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

        // --- FIXED AUTH LOGIC ---
        window.handleAuth = async (type) => {
            const u = document.getElementById('username').value.trim();
            const p = document.getElementById('password').value;
            
            if(!u || !p) return alert("Username aur Password bharen, sweetie!");
            
            const email = u.includes('@') ? u : `${u}@apex.com`;

            try {
                if(type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, email, p);
                    // Create User Document in Firestore
                    await setDoc(doc(db, "users", res.user.uid), {
                        username: u,
                        balance: 0,
                        role: "user",
                        createdAt: new Date()
                    });
                    alert("Account ban gaya, sweetie! 😘");
                } else {
                    await signInWithEmailAndPassword(auth, email, p);
                    alert("Welcome Back!");
                }
            } catch (e) {
                alert("Error: " + e.message);
            }
        };

        // UI View Controller
        window.showPage = (id) => {
            ['homePage','plansPage','historyPage','morePage'].forEach(p => document.getElementById(p).classList.add('hidden'));
            document.getElementById(id).classList.remove('hidden');
        };

        onAuthStateChanged(auth, (user) => {
            const authSec = document.getElementById('authSection');
            const appBody = document.getElementById('appBody');
            
            if(user) {
                authSec.classList.add('hidden');
                appBody.classList.remove('hidden');
                
                onSnapshot(doc(db, "users", user.uid), (d) => {
                    if(d.exists()) {
                        const data = d.data();
                        document.getElementById('uBal').innerText = "$" + (data.balance || 0).toFixed(2);
                        document.getElementById('uName').innerText = data.username;
                        document.getElementById('refLink').innerText = "apexdaily.com/ref=" + data.username;
                    }
                });
            } else {
                authSec.classList.remove('hidden');
                appBody.classList.add('hidden');
            }
        });

        // Fake Notification Logic
        function showFakeNotif() {
            const names = ["Ali", "Sara", "Hamza", "Zain", "Dua"];
            const amounts = [10, 50, 100, 25, 500];
            const randomName = names[Math.floor(Math.random() * names.length)];
            const randomAmt = amounts[Math.floor(Math.random() * amounts.length)];
            
            const notif = document.getElementById('fakeNotif');
            notif.innerText = `${randomName} just earned $${randomAmt} from Level ${Math.floor(Math.random()*5)+1}! 🚀`;
            notif.classList.remove('hidden');
            setTimeout(() => notif.classList.add('hidden'), 4000);
        }
        setInterval(showFakeNotif, 15000); // Har 15 second baad

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="bg-[#050810] text-slate-200 pb-24 font-sans">

    <div id="fakeNotif" class="hidden fixed top-10 left-1/2 -translate-x-1/2 bg-blue-600/90 backdrop-blur-md px-6 py-2 rounded-full text-[10px] font-bold z-[100] shadow-2xl border border-blue-400/30"></div>

    <div id="authSection" class="h-screen flex items-center justify-center p-6">
        <div class="w-full max-w-md bg-[#161b2c] p-10 rounded-[2.5rem] border border-white/5 text-center shadow-2xl">
            <h1 class="text-4xl font-black text-blue-500 mb-2 italic tracking-tighter">APEXDAILY</h1>
            <p class="text-xs text-gray-500 mb-10 uppercase tracking-widest">Global Trading System</p>
            
            <input id="username" type="text" placeholder="Username" class="w-full p-4 bg-black/20 rounded-2xl mb-4 outline-none border border-white/5 focus:border-blue-500 transition-all">
            <input id="password" type="password" placeholder="Password" class="w-full p-4 bg-black/20 rounded-2xl mb-8 outline-none border border-white/5 focus:border-blue-500 transition-all">
            
            <div class="flex gap-4">
                <button onclick="handleAuth('login')" class="flex-1 bg-blue-600 py-4 rounded-2xl font-bold shadow-lg shadow-blue-500/20 active:scale-95 transition">Login</button>
                <button onclick="handleAuth('signup')" class="flex-1 bg-white/5 py-4 rounded-2xl font-bold border border-white/10 active:scale-95 transition">Signup</button>
            </div>
        </div>
    </div>

    <div id="appBody" class="hidden">
        <div id="homePage" class="p-6">
            <div class="bg-gradient-to-br from-blue-600 to-indigo-900 p-8 rounded-[2.5rem] shadow-2xl mb-8">
                <p class="text-xs opacity-70">Total Profit</p>
                <h1 id="uBal" class="text-6xl font-black my-2">$0.00</h1>
                <div class="flex gap-4 mt-6">
                    <button class="flex-1 bg-white/20 p-3 rounded-xl font-bold">Deposit</button>
                    <button class="flex-1 bg-black/20 p-3 rounded-xl font-bold">Withdraw</button>
                </div>
            </div>
            <div class="bg-white/5 p-5 rounded-3xl">
                <p class="text-[10px] text-gray-500 mb-2">My Referral Link</p>
                <div id="refLink" class="text-[10px] font-mono text-blue-400">...</div>
            </div>
        </div>
        
        <nav class="fixed bottom-0 left-0 right-0 bg-[#050810]/95 backdrop-blur-lg border-t border-white/5 p-5 flex justify-around items-center z-50">
            <button onclick="showPage('homePage')" class="text-blue-500 flex flex-col items-center"><i class="fa-solid fa-house"></i><span class="text-[8px] mt-1">Home</span></button>
            <button onclick="showPage('plansPage')" class="text-gray-500 flex flex-col items-center"><i class="fa-solid fa-bolt"></i><span class="text-[8px] mt-1">Plans</span></button>
            <button onclick="showPage('historyPage')" class="text-gray-500 flex flex-col items-center"><i class="fa-solid fa-receipt"></i><span class="text-[8px] mt-1">History</span></button>
            <button onclick="showPage('morePage')" class="text-gray-500 flex flex-col items-center"><i class="fa-solid fa-bars"></i><span class="text-[8px] mt-1">More</span></button>
        </nav>
    </div>

</body>
</html>
