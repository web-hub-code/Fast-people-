<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Digital Earnings 🇵🇰</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        /* --- PREMIUM UI DESIGN --- */
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #020617; color: white; min-height: 100vh; overflow-x: hidden; display: flex; flex-direction: column; align-items: center; }
        .blob { position: fixed; width: 300px; height: 300px; background: #fbbf24; filter: blur(100px); border-radius: 50%; z-index: -1; top: -5%; right: -5%; opacity: 0.15; }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 20px; padding: 20px; width: 92%; max-width: 450px; text-align: center; margin: 15px auto; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        .brand-name { font-size: 1.6rem; font-weight: 600; color: #fbbf24; text-shadow: 0 0 10px rgba(251, 191, 36, 0.3); }
        input, select { width: 100%; padding: 12px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 10px; color: white; margin-bottom: 10px; outline: none; }
        .btn-main { width: 100%; padding: 12px; background: linear-gradient(90deg, #fbbf24, #d97706); border: none; border-radius: 10px; color: black; font-weight: bold; cursor: pointer; transition: 0.3s; margin-top: 5px; }
        .btn-main:hover { transform: scale(1.02); opacity: 0.9; }
        .tier-tag { font-size: 0.7rem; background: #fbbf24; color: #000; padding: 3px 10px; border-radius: 15px; font-weight: bold; }
        #loginPage, #dashboardPage, #adminPage, #termsModal, #newsModal { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }
        .overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.92); z-index: 5000; display: flex; justify-content: center; align-items: center; }
        .task-box { background: rgba(16, 185, 129, 0.1); padding: 12px; border-radius: 10px; margin-bottom: 8px; font-size: 0.8rem; border: 1px solid rgba(16, 185, 129, 0.2); cursor: pointer; text-align: left; }
        .history-table { width: 100%; font-size: 0.7rem; border-collapse: collapse; margin-top: 10px; text-align: left; }
        .history-table td { padding: 8px; border-bottom: 1px solid rgba(255,255,255,0.05); }
    </style>
</head>
<body>
    <div class="blob"></div>

    <div id="termsModal" class="overlay">
        <div class="glass-card">
            <h2 style="color: #fbbf24;">PakGold Rules 🇵🇰</h2>
            <div style="text-align: left; font-size: 0.75rem; margin: 15px 0; color: #cbd5e1; line-height: 1.6;">
                <p>• Min. Deposit: <b>PKR 200</b></p>
                <p>• Min. Withdraw: <b>PKR 100</b></p>
                <p>• Withdrawal Fee: <b>5% Service Fee</b></p>
                <p>• Daily Work: Complete tasks for profit.</p>
            </div>
            <button id="acceptBtn" class="btn-main">Accept & Continue</button>
        </div>
    </div>

    <div id="newsModal" class="overlay">
        <div class="glass-card">
            <h3 style="color: #fbbf24;">Latest Announcement 📢</h3>
            <p id="newsText" style="font-size: 0.8rem; margin: 15px 0; color: #cbd5e1;">Welcome to PakGold! Start your earning journey today.</p>
            <button onclick="document.getElementById('newsModal').classList.remove('flex-active')" class="btn-main">Got it! ✅</button>
        </div>
    </div>

    <div id="loginPage" class="glass-card">
        <div class="brand-name">PakGold</div>
        <p style="font-size: 0.7rem; color: #94a3b8; margin-bottom: 25px;">Premium Investment Platform</p>
        <button id="loginBtn" class="btn-main">Enter Dashboard</button>
        <button onclick="adminAccess()" style="background:transparent; border:1px solid #4facfe; color:#4facfe; margin-top:15px; font-size:0.65rem;" class="btn-main">Admin Login</button>
    </div>

    <div id="dashboardPage" class="glass-card">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
            <span class="brand-name" style="font-size: 1.1rem;">PakGold</span>
            <div onclick="showAnnouncement()" style="cursor:pointer; position:relative;">🔔<span id="dot" style="position:absolute; top:0; right:0; width:6px; height:6px; background:red; border-radius:50%; display:none;"></span></div>
            <button id="logoutBtn" style="background:none; color:#ef4444; border:1px solid #ef4444; font-size:0.6rem; padding:3px 7px; border-radius:5px; cursor:pointer;">Logout</button>
        </div>

        <div style="background: rgba(255,255,255,0.02); padding: 15px; border-radius: 15px; border: 1px solid rgba(251, 191, 36, 0.2);">
            <span id="tierBadge" class="tier-tag">Starter Member</span>
            <p style="font-size: 0.7rem; color: #94a3b8; margin-top: 10px;">Available Balance</p>
            <h2 style="color: #fbbf24;">PKR <span id="userBal">0.00</span></h2>
        </div>

        <div id="vipBox" style="margin-top:15px; border:1px solid #fbbf24; padding:10px; border-radius:10px; display:none;">
            <p style="font-size:0.65rem;">👑 <b>VIP Upgrade:</b> Earn PKR 50/task instead of 20!</p>
            <button onclick="upgradeVIP()" class="btn-main" style="padding:5px; font-size:0.7rem;">Upgrade (PKR 1000)</button>
        </div>

        <div style="margin-top: 20px; text-align: left;">
            <h4 style="font-size: 0.85rem; color: #10b981;">Daily Earning Tasks 🛠️</h4>
            <div id="tasks" style="margin-top:10px;">
                <div class="task-box" onclick="startTask(1)">Task 1: Market Analysis (Earn PKR 20/50)</div>
                <div class="task-box" onclick="startTask(2)">Task 2: Mining Activation (Earn PKR 20/50)</div>
            </div>
        </div>

        <div style="margin-top:20px; background:rgba(0,0,0,0.2); padding:10px; border-radius:10px;">
            <p style="font-size:0.7rem; margin-bottom:5px;">Profit Calculator 📊</p>
            <input type="number" id="calcIn" placeholder="Enter Invest Amt" oninput="calcProfit()" style="font-size:0.7rem; padding:8px;">
            <p id="calcOut" style="font-size:0.65rem; color:#10b981;">Daily: PKR 0 | Monthly: PKR 0</p>
        </div>

        <hr style="opacity: 0.1; margin: 15px 0;">

        <select id="txnType"><option value="Deposit">Deposit (Min 200)</option><option value="Withdraw">Withdraw (Min 100)</option></select>
        <select id="mthd"><option value="JazzCash">JazzCash</option><option value="EasyPaisa">EasyPaisa</option></select>
        <input type="number" id="txnAmt" placeholder="Amount">
        <button id="txnBtn" class="btn-main">Submit Request</button>

        <table class="history-table"><thead><tr style="color:#94a3b8;"><th>Type</th><th>Amt</th><th>Status</th></tr></thead><tbody id="logs"></tbody></table>
    </div>

    <div id="adminPage" class="glass-card" style="max-width:500px;">
        <h2 style="color:#fbbf24;">Admin Control 👑</h2>
        <div id="adminList" style="margin-top:20px;"></div>
        <button onclick="location.reload()" class="btn-main" style="background:#ef4444; color:white; margin-top:20px;">Close Admin</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, onSnapshot, collection, addDoc, query, where, increment, serverTimestamp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

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

        // --- TERMS ---
        if(!localStorage.getItem('pakgold_terms')) document.getElementById('termsModal').classList.add('flex-active');
        document.getElementById('acceptBtn').onclick = () => { document.getElementById('termsModal').classList.remove('flex-active'); localStorage.setItem('pakgold_terms', 'true'); };

        // --- AUTH ---
        document.getElementById('loginBtn').onclick = () => signInAnonymously(auth);
        document.getElementById('logoutBtn').onclick = () => signOut(auth).then(()=>location.reload());

        onAuthStateChanged(auth, async (user) => {
            if(user) {
                document.getElementById('loginPage').style.display='none';
                document.getElementById('dashboardPage').classList.add('active');
                const userRef = doc(db, "users", user.uid);
                const snap = await getDoc(userRef);
                if(!snap.exists()) await setDoc(userRef, { balance: 0, tier: 'Starter', isVIP: false, uid: user.uid, lastTask: 0 });

                onSnapshot(userRef, (d) => {
                    const data = d.data();
                    document.getElementById('userBal').innerText = data.balance.toFixed(2);
                    document.getElementById('tierBadge').innerText = data.tier + " Member";
                    if(!data.isVIP) document.getElementById('vipBox').style.display='block';
                    else document.getElementById('vipBox').style.display='none';
                });

                onSnapshot(query(collection(db, "transactions"), where("uid", "==", user.uid)), (s) => {
                    const l = document.getElementById('logs'); l.innerHTML = "";
                    s.forEach(doc => { const d = doc.data(); l.innerHTML += `<tr><td>${d.type}</td><td>${d.amount}</td><td style="color:${d.status=='approved'?'#10b981':'#fbbf24'}">${d.status}</td></tr>`; });
                });
            } else { document.getElementById('loginPage').classList.add('active'); }
        });

        // --- CORE FUNCTIONS ---
        window.startTask = async (id) => {
            const userRef = doc(db, "users", auth.currentUser.uid);
            const d = (await getDoc(userRef)).data();
            if(Date.now() - d.lastTask < 86400000) return alert("Wait 24h, sweetie! ⏳");
            
            alert("Work in Progress... Wait 10s! ⏳");
            setTimeout(async () => {
                const reward = d.isVIP ? 50 : 20;
                await updateDoc(userRef, { balance: increment(reward), lastTask: Date.now() });
                alert(`Task Complete! PKR ${reward} added. 💰`);
            }, 10000);
        };

        window.calcProfit = () => {
            const val = document.getElementById('calcIn').value;
            const daily = (val * 0.10).toFixed(0);
            document.getElementById('calcOut').innerText = `Daily: PKR ${daily} | Monthly: PKR ${daily*30}`;
        };

        document.getElementById('txnBtn').onclick = async () => {
            const amt = Number(document.getElementById('txnAmt').value);
            const type = document.getElementById('txnType').value;
            if(type === "Deposit" && amt < 200) return alert("Min Deposit 200! 😘");
            if(type === "Withdraw" && amt < 100) return alert("Min Withdraw 100! 😘");

            await addDoc(collection(db, "transactions"), { uid: auth.currentUser.uid, amount: amt, type: type, method: document.getElementById('mthd').value, status: "pending", timestamp: serverTimestamp() });
            alert("Request Sent! ✅");
        };

        window.upgradeVIP = async () => {
            const userRef = doc(db, "users", auth.currentUser.uid);
            if((await getDoc(userRef)).data().balance < 1000) return alert("Need PKR 1000 balance! ❌");
            await updateDoc(userRef, { balance: increment(-1000), isVIP: true, tier: 'VIP' });
            alert("Mubarak ho sweetie! You are VIP now. 👑");
        };

        // --- ADMIN ---
        window.adminAccess = () => { if(prompt("Password:") === "admin123") { document.getElementById('loginPage').style.display='none'; document.getElementById('adminPage').classList.add('active'); loadAdmin(); } };
        function loadAdmin() {
            onSnapshot(query(collection(db, "transactions"), where("status", "==", "pending")), (s) => {
                const al = document.getElementById('adminList'); al.innerHTML = "";
                s.forEach(ds => { const d = ds.data(); al.innerHTML += `<div class="glass-card" style="font-size:0.7rem;">${d.type}: PKR ${d.amount} (${d.method})<br><button onclick="approve('${ds.id}','${d.uid}',${d.amount},'${d.type}')" class="btn-main" style="padding:5px; background:#10b981;">Approve</button></div>`; });
            });
        }
        window.approve = async (id, uid, amt, type) => {
            await updateDoc(doc(db, "transactions", id), { status: "approved" });
            const change = type === "Deposit" ? amt : -(amt + (amt*0.05));
            await updateDoc(doc(db, "users", uid), { balance: increment(change) });
            alert("Approved! ✅");
        };
    </script>
</body>
</html>
