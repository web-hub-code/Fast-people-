<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dark Web 9 | Modern Platform</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    
    <style>
        /* --- MODERN CSS --- */
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #0f172a; color: white; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow-x: hidden; }

        /* Animated Blobs */
        .blob { position: fixed; width: 400px; height: 400px; background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); filter: blur(100px); border-radius: 50%; z-index: -1; top: -10%; right: -10%; opacity: 0.5; animation: move 15s infinite alternate; }
        @keyframes move { from { transform: translate(0,0); } to { transform: translate(-50px, 100px); } }

        /* Glass Cards */
        .glass-card { background: rgba(255, 255, 255, 0.05); backdrop-filter: blur(15px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 20px; padding: 25px; width: 380px; box-shadow: 0 10px 30px rgba(0,0,0,0.5); text-align: center; margin: 10px; }

        /* Input & Buttons */
        .input-group { margin-bottom: 15px; }
        input, select { width: 100%; padding: 12px; background: rgba(255,255,255,0.08); border: none; border-radius: 10px; color: white; outline: none; transition: 0.3s; }
        input:focus { background: rgba(255,255,255,0.15); border-left: 4px solid #4facfe; }

        .btn-main { width: 100%; padding: 12px; background: linear-gradient(to right, #4facfe, #00f2fe); border: none; border-radius: 10px; color: white; font-weight: bold; cursor: pointer; transition: 0.3s; margin-top: 10px; }
        .btn-main:hover { transform: scale(1.05); box-shadow: 0 5px 15px rgba(79, 172, 254, 0.4); }

        /* Sections Control */
        #loginPage, #dashboardPage { display: none; }
        .active { display: block !important; }

        /* Notification */
        #notify { position: fixed; top: -50px; right: 20px; background: #10b981; padding: 15px 25px; border-radius: 10px; transition: 0.5s; z-index: 100; }
        #notify.show { top: 20px; }

        .balance-txt { font-size: 2rem; font-weight: 600; color: #4facfe; margin: 10px 0; }
        .tier-tag { font-size: 0.8rem; background: gold; color: black; padding: 2px 10px; border-radius: 20px; font-weight: bold; }
    </style>
</head>
<body>

    <div class="blob"></div>
    <div id="notify">Success, sweetie! 🚀</div>

    <div id="loginPage" class="glass-card active">
        <h2>Welcome Back</h2>
        <p style="font-size: 0.8rem; color: #94a3b8; margin-bottom: 20px;">Secure Anonymous Access</p>
        <button id="anonLoginBtn" class="btn-main">Login Anonymously</button>
    </div>

    <div id="dashboardPage" class="glass-card">
        <span class="tier-tag">Gold Member</span>
        <p style="margin-top: 10px;">Total Balance</p>
        <h1 class="balance-txt">PKR <span id="userBal">0.00</span></h1>

        <hr style="opacity: 0.1; margin: 20px 0;">

        <div class="input-group">
            <select id="depMethod">
                <option value="JazzCash">JazzCash</option>
                <option value="EasyPaisa">EasyPaisa</option>
                <option value="Binance">Binance</option>
            </select>
        </div>
        <input type="number" id="depAmount" placeholder="Deposit Amount">
        <button onclick="sendDeposit()" class="btn-main">Send Deposit Request</button>

        <div style="margin-top: 25px; font-size: 0.8rem; background: rgba(0,0,0,0.2); padding: 10px; border-radius: 10px;">
            <p>Your Referral Link:</p>
            <code id="refLink" style="color: #4facfe;">Loading...</code>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, addDoc, collection } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

        // Your Config
        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);

        const loginPage = document.getElementById('loginPage');
        const dashboardPage = document.getElementById('dashboardPage');
        const notify = document.getElementById('notify');

        // Login Logic
        document.getElementById('anonLoginBtn').onclick = async () => {
            await signInAnonymously(auth);
            showNotify("Logged in successfully! ✨");
        };

        // UI Auth State
        onAuthStateChanged(auth, async (user) => {
            if (user) {
                loginPage.classList.remove('active');
                dashboardPage.classList.add('active');
                
                // Set Referral Link
                document.getElementById('refLink').innerText = `https://dark-web-9.web.app/?ref=${user.uid}`;
                
                // Fetch Data
                const userRef = doc(db, "users", user.uid);
                const snap = await getDoc(userRef);
                if (!snap.exists()) {
                    await setDoc(userRef, { balance: 0, tier: 'Gold' });
                } else {
                    document.getElementById('userBal').innerText = snap.data().balance;
                }
            }
        });

        // Deposit Logic
        window.sendDeposit = async () => {
            const amt = document.getElementById('depAmount').value;
            const method = document.getElementById('depMethod').value;
            if(!amt) return alert("Amount dalo sweetie!");

            await addDoc(collection(db, "deposits"), {
                uid: auth.currentUser.uid,
                amount: amt,
                method: method,
                status: "pending",
                time: new Date()
            });
            showNotify("Request Sent! Admin will approve soon.");
        };

        function showNotify(msg) {
            notify.innerText = msg;
            notify.classList.add('show');
            setTimeout(() => notify.classList.remove('show'), 3000);
        }
    </script>
</body>
</html>
