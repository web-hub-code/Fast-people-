<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ApexDaily | Mobile Trading</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;900&display=swap');
        body { background: #020617; color: white; font-family: 'Outfit', sans-serif; }
        
        .glass { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(15px); border: 1px solid rgba(255, 255, 255, 0.1); }
        .neon-btn { background: linear-gradient(90deg, #0ea5e9, #a855f7); box-shadow: 0 10px 20px rgba(168, 85, 247, 0.2); transition: 0.3s; }
        .neon-btn:active { transform: scale(0.95); }
        
        input, select { background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(255, 255, 255, 0.1); outline: none; transition: 0.3s; }
        input:focus { border-color: #0ea5e9; box-shadow: 0 0 10px rgba(14, 165, 233, 0.2); }

        #verifyBox { display: none; animation: slideIn 0.4s ease; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
        import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
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

        // Secret Click Logic for Admin
        let clicks = 0;
        window.secretTap = () => {
            clicks++;
            if(clicks === 3) {
                const key = prompt("Enter Secret Admin Key:");
                if(key === "786786") {
                    signInWithEmailAndPassword(auth, "admin@apexdaily.com", "admin123");
                }
                clicks = 0;
            }
        };

        window.toggleVerify = () => {
            document.getElementById('authForm').classList.add('hidden');
            document.getElementById('verifyBox').style.display = 'block';
        };

        window.submitAuth = async (mode) => {
            const phone = document.getElementById('phone').value;
            const pass = document.getElementById('pass').value;
            const user = document.getElementById('user').value;
            const code = document.getElementById('country').value;
            
            // Crafting a fake email for Firebase to use phone numbers as ID
            const fakeEmail = `${phone}@phone.com`;

            try {
                if(mode === 'signup') {
                    const r = await createUserWithEmailAndPassword(auth, fakeEmail, pass);
                    await setDoc(doc(db, "users", r.user.uid), { 
                        username: user, 
                        phone: code + phone, 
                        balance: 0 
                    });
                } else {
                    await signInWithEmailAndPassword(auth, fakeEmail, pass);
                }
            } catch(e) { alert("Error: " + e.message); }
        };

        onAuthStateChanged(auth, (u) => {
            if(u) {
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('dashScreen').classList.remove('hidden');
                onSnapshot(doc(db, "users", u.uid), (d) => {
                    if(d.exists()) {
                        document.getElementById('uName').innerText = d.data().username;
                        document.getElementById('uBal').innerText = "Rs. " + (d.data().balance || 0).toLocaleString();
                    }
                });
            }
        });
    </script>
</head>
<body class="p-0 m-0">

    <div id="loginScreen" class="min-h-screen p-8 flex flex-col justify-center items-center">
        <h1 onclick="secretTap()" class="text-4xl font-black italic mb-2 bg-clip-text text-transparent bg-gradient-to-r from-cyan-400 to-purple-500">APEXDAILY</h1>
        <p class="text-[9px] tracking-[0.4em] text-gray-500 uppercase mb-10">Mobile Trading Portal</p>

        <div id="authForm" class="w-full max-w-sm glass p-8 rounded-[2.5rem] space-y-4">
            <input id="user" type="text" placeholder="Full Name" class="w-full p-4 rounded-2xl text-sm">
            
            <div class="flex gap-2">
                <select id="country" class="p-4 rounded-2xl text-sm w-24">
                    <option value="+92">+92</option>
                    <option value="+91">+91</option>
                    <option value="+971">+971</option>
                    <option value="+1">+1</option>
                </select>
                <input id="phone" type="number" placeholder="Phone Number" class="flex-1 p-4 rounded-2xl text-sm">
            </div>

            <input id="pass" type="password" placeholder="Password" class="w-full p-4 rounded-2xl text-sm">
            
            <button onclick="toggleVerify()" class="w-full neon-btn py-4 rounded-2xl font-black text-sm mt-4">NEXT STEP</button>
            <p onclick="submitAuth('login')" class="text-center text-[11px] text-gray-400">Already have an account? <span class="text-cyan-400 font-bold">Login</span></p>
        </div>

        <div id="verifyBox" class="w-full max-w-sm glass p-8 rounded-[2.5rem] text-center">
            <h3 class="font-black text-lg mb-2">Verify Phone</h3>
            <p class="text-[10px] text-gray-500 mb-6">Enter the 6-digit code sent to your number</p>
            <div class="flex justify-between gap-2 mb-8">
                <input type="number" maxlength="1" class="w-12 h-12 text-center rounded-xl font-black text-xl bg-white/5 border border-white/10">
                <input type="number" maxlength="1" class="w-12 h-12 text-center rounded-xl font-black text-xl bg-white/5 border border-white/10">
                <input type="number" maxlength="1" class="w-12 h-12 text-center rounded-xl font-black text-xl bg-white/5 border border-white/10">
                <input type="number" maxlength="1" class="w-12 h-12 text-center rounded-xl font-black text-xl bg-white/5 border border-white/10">
            </div>
            <button onclick="submitAuth('signup')" class="w-full neon-btn py-4 rounded-2xl font-black">FINISH SIGNUP</button>
        </div>
    </div>

    <div id="dashScreen" class="hidden min-h-screen pb-24">
        <div class="p-6 flex justify-between items-center sticky top-0 bg-[#020617]/90 backdrop-blur-md z-50">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-cyan-500 to-blue-600 flex items-center justify-center font-black">A</div>
                <div>
                    <h2 class="text-sm font-black tracking-tight">@<span id="uName">User</span> <i class="fa-solid fa-circle-check text-cyan-400 text-[10px] ml-1"></i></h2>
                    <p class="text-[8px] text-green-500 font-bold uppercase tracking-widest">Verified Account</p>
                </div>
            </div>
            <button onclick="location.reload()" class="w-10 h-10 glass rounded-xl flex items-center justify-center text-red-500"><i class="fa-solid fa-power-off"></i></button>
        </div>

        <div class="p-6">
            <div class="bg-gradient-to-br from-blue-600 to-purple-700 p-8 rounded-[2.5rem] shadow-2xl mb-8 relative overflow-hidden">
                <p class="text-[10px] uppercase font-bold opacity-80 tracking-widest">Total Savings</p>
                <h1 id="uBal" class="text-5xl font-black my-3 tracking-tighter">Rs. 0</h1>
                <div class="flex gap-3 mt-6">
                    <button class="flex-1 bg-white text-blue-700 py-3 rounded-xl font-black text-xs">DEPOSIT</button>
                    <button class="flex-1 bg-black/20 py-3 rounded-xl font-black text-xs border border-white/20">WITHDRAW</button>
                </div>
            </div>

            <div class="space-y-4">
                <div class="glass p-6 rounded-[2rem] flex justify-between items-center border-l-4 border-cyan-500">
                    <div><h4 class="font-black text-lg">Daily Plan</h4><p class="text-[10px] text-gray-400">Invest Rs. 200 -> Earn Rs. 25</p></div>
                    <button class="neon-btn px-6 py-2.5 rounded-xl text-[10px] font-black">BUY</button>
                </div>
            </div>
        </div>

        <nav class="fixed bottom-0 left-0 right-0 glass p-5 flex justify-around items-center rounded-t-[2.5rem] z-50">
            <button class="text-cyan-400 flex flex-col items-center gap-1"><i class="fa-solid fa-house-chimney text-xl"></i><span class="text-[8px] font-bold">HOME</span></button>
            <button class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-wallet text-xl"></i><span class="text-[8px] font-bold">WALLET</span></button>
            <button class="text-gray-500 flex flex-col items-center gap-1"><i class="fa-solid fa-headset text-xl"></i><span class="text-[8px] font-bold">HELP</span></button>
        </nav>
    </div>

</body>
</html>
