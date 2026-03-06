<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ApexDaily | Profit Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, onSnapshot } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-firestore.js";

        // --- Firebase Configuration ---
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
        const PKR_RATE = 285; 

        // --- Auth Logic (Fixed Email Error) ---
        window.handleAuth = async (type) => {
            let userVal = document.getElementById('username').value.trim();
            const passVal = document.getElementById('password').value;

            if(!userVal || !passVal) return alert("Fill all fields, sweetie!");

            // Automatically fix email format
            const finalEmail = userVal.includes('@') ? userVal : `${userVal}@profitpro.com`;

            try {
                if (type === 'signup') {
                    const res = await createUserWithEmailAndPassword(auth, finalEmail, passVal);
                    await setDoc(doc(db, "users", res.user.uid), {
                        username: userVal,
                        balance: 0,
                        pkrRate: PKR_RATE,
                        role: "user"
                    });
                    alert("Account Created Successfully!");
                } else {
                    await signInWithEmailAndPassword(auth, finalEmail, passVal);
                }
            } catch (err) {
                alert("Error: " + err.message);
            }
        };

        // --- Real-time Data Sync ---
        onAuthStateChanged(auth, (user) => {
            const authDiv = document.getElementById('authSection');
            const dashDiv = document.getElementById('mainDashboard');

            if (user) {
                authDiv.classList.add('hidden');
                dashDiv.classList.remove('hidden');

                // Listen for balance changes live
                onSnapshot(doc(db, "users", user.uid), (docSnap) => {
                    if (docSnap.exists()) {
                        const data = docSnap.data();
                        document.getElementById('userDisp').innerText = data.username;
                        document.getElementById('balUSD').innerText = "$" + data.balance.toFixed(2);
                        document.getElementById('balPKR').innerText = (data.balance * PKR_RATE).toLocaleString() + " PKR";
                    }
                });
            } else {
                authDiv.classList.remove('hidden');
                dashDiv.classList.add('hidden');
            }
        });

        // --- UI Calculations ---
        window.calcDep = () => {
            const usd = document.getElementById('depInp').value;
            document.getElementById('pkrLabel').innerText = (usd * PKR_RATE).toLocaleString() + " PKR";
        };

        window.logout = () => signOut(auth);
    </script>
</head>
<body class="bg-[#0b0f1a] text-white min-h-screen font-sans flex items-center justify-center p-4">

    <div id="authSection" class="w-full max-w-md bg-[#161b2c] p-8 rounded-3xl border border-gray-800 shadow-2xl">
        <h1 class="text-3xl font-bold text-center mb-2 text-blue-500">ApexDaily</h1>
        <p class="text-center text-gray-500 text-sm mb-8">Start your daily profit journey</p>
        
        <input id="username" type="text" placeholder="Username (e.g. ali123)" class="w-full p-4 mb-4 bg-[#1f263d] rounded-xl outline-none border border-transparent focus:border-blue-500 transition">
        <input id="password" type="password" placeholder="Password" class="w-full p-4 mb-8 bg-[#1f263d] rounded-xl outline-none border border-transparent focus:border-blue-500 transition">
        
        <div class="flex gap-4">
            <button onclick="handleAuth('login')" class="flex-1 bg-blue-600 hover:bg-blue-700 p-4 rounded-xl font-bold shadow-lg shadow-blue-900/20">Login</button>
            <button onclick="handleAuth('signup')" class="flex-1 bg-gray-700 hover:bg-gray-600 p-4 rounded-xl font-bold">Signup</button>
        </div>
    </div>

    <div id="mainDashboard" class="hidden w-full max-w-5xl">
        <header class="flex justify-between items-center mb-8">
            <div>
                <h2 class="text-2xl font-bold">Hello, <span id="userDisp" class="text-blue-400">...</span> 👋</h2>
                <p class="text-gray-500 text-sm">Today: March 06, 2026</p>
            </div>
            <button onclick="logout()" class="bg-red-500/10 text-red-500 px-4 py-2 rounded-lg text-sm font-bold border border-red-500/20">Logout Account</button>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <div class="lg:col-span-2 bg-gradient-to-br from-blue-600 to-blue-800 p-8 rounded-[2rem] shadow-xl relative overflow-hidden">
                <div class="relative z-10">
                    <p class="text-blue-100 text-sm uppercase tracking-widest font-semibold">Available Balance</p>
                    <h1 id="balUSD" class="text-6xl font-black my-4">$0.00</h1>
                    <div class="inline-block bg-white/20 backdrop-blur-md px-4 py-2 rounded-full">
                        <span id="balPKR" class="font-bold text-white">0 PKR</span>
                    </div>
                </div>
                <div class="absolute -bottom-10 -right-10 w-40 h-40 bg-white/10 rounded-full blur-3xl"></div>
            </div>

            <div class="bg-[#161b2c] p-6 rounded-[2rem] border border-gray-800 shadow-xl">
                <h3 class="text-xl font-bold mb-4 flex items-center gap-2">
                    <span class="w-2 h-6 bg-green-500 rounded-full"></span> Quick Deposit
                </h3>
                <input id="depInp" type="number" oninput="calcDep()" placeholder="Enter Dollars ($)" class="w-full p-4 bg-[#1f263d] rounded-xl mb-4 border border-gray-700 focus:border-green-500 outline-none">
                <div class="flex justify-between text-sm mb-6">
                    <span class="text-gray-500 font-medium italic">Amount in PKR:</span>
                    <span id="pkrLabel" class="text-green-400 font-bold text-lg">0 PKR</span>
                </div>
                <button class="w-full bg-green-600 hover:bg-green-700 p-4 rounded-xl font-bold transition">Deposit Now</button>
            </div>
        </div>

        <div class="mt-12 p-6 bg-[#161b2c]/50 rounded-2xl border border-dashed border-gray-800 text-center">
            <p class="text-gray-500 text-xs mb-4 uppercase tracking-widest">Supported Payment Methods</p>
            <div class="flex flex-wrap justify-center gap-8 opacity-40 grayscale hover:grayscale-0 transition duration-700">
                <span class="font-bold">Binance (USDT)</span>
                <span class="font-bold">JazzCash</span>
                <span class="font-bold">EasyPaisa</span>
                <span class="font-bold">SadaPay</span>
                <span class="font-bold">Bank Transfer</span>
            </div>
        </div>
    </div>

</body>
</html>
