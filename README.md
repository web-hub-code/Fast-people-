<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Professional Earnings 🇵🇰</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    
    <style>
        /* --- GLOBAL STYLES --- */
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: #020617; color: white; min-height: 100vh; overflow-x: hidden; display: flex; flex-direction: column; align-items: center; }

        .blob { position: fixed; width: 400px; height: 400px; background: linear-gradient(135deg, #fbbf24 0%, #d97706 100%); filter: blur(120px); border-radius: 50%; z-index: -1; top: -10%; right: -10%; opacity: 0.2; }
        .glass-card { background: rgba(255, 255, 255, 0.03); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.1); border-radius: 24px; padding: 25px; width: 92%; max-width: 450px; text-align: center; margin: 20px auto; box-shadow: 0 10px 40px rgba(0,0,0,0.6); }

        /* Branding */
        .brand-name { font-size: 1.8rem; font-weight: 600; color: #fbbf24; text-shadow: 0 0 15px rgba(251, 191, 36, 0.4); }
        .tier-tag { font-size: 0.7rem; background: #fbbf24; color: #000; padding: 4px 12px; border-radius: 20px; font-weight: bold; }

        /* Inputs & Buttons */
        input, select { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 12px; color: white; margin-bottom: 12px; outline: none; }
        .btn-main { width: 100%; padding: 14px; background: linear-gradient(90deg, #fbbf24, #d97706); border: none; border-radius: 12px; color: black; font-weight: bold; cursor: pointer; transition: 0.3s; margin-top: 10px; }
        .btn-main:hover { transform: scale(1.02); box-shadow: 0 0 20px rgba(251, 191, 36, 0.4); }
        .btn-secondary { background: rgba(255,255,255,0.1); color: white; border: 1px solid rgba(255,255,255,0.2); }

        /* Sections Control */
        #loginPage, #dashboardPage, #adminPage, #termsModal { display: none; }
        .active-flex { display: flex !important; }
        .active-block { display: block !important; }

        /* Table & History */
        .history-table { width: 100%; border-collapse: collapse; margin-top: 15px; font-size: 0.75rem; text-align: left; }
        .history-table th { color: #94a3b8; padding: 10px; border-bottom: 1px solid rgba(255,255,255,0.1); }
        .history-table td { padding: 10px; border-bottom: 1px solid rgba(255,255,255,0.05); }

        /* Modal Overlay */
        .overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.9); z-index: 2000; display: flex; justify-content: center; align-items: center; }
        #notify { position: fixed; top: -100px; background: #fbbf24; color: black; padding: 15px 30px; border-radius: 12px; font-weight: bold; transition: 0.5s; z-index: 3000; }
    </style>
</head>
<body>

    <div class="blob"></div>
    <div id="notify">Action Successful, sweetie! ✅</div>

    <div id="termsModal" class="overlay active-flex">
        <div class="glass-card">
            <h2 style="color: #fbbf24;">PakGold Rules 🇵🇰</h2>
            <div style="text-align: left; font-size: 0.8rem; margin: 20px 0; color: #cbd5e1; line-height: 1.6;">
                <p>• Min. Deposit: <b>PKR 500</b></p>
                <p>• Min. Withdraw: <b>PKR 500</b></p>
                <p>• Daily Bonus: Once every 24 Hours.</p>
                <p>• Approval: 1 to 24 hours required.</p>
            </div>
            <button onclick="acceptTerms()" class="btn-main">Accept & Continue</button>
        </div>
    </div>

    <div id="loginPage" class="glass-card">
        <div class="brand-name">PakGold</div>
        <p style="color: #94a3b8; font-size: 0.8rem; margin-bottom: 30px;">Digital Assets, Real Growth.</p>
        <button id="userLoginBtn" class="btn-main">Enter Dashboard</button>
        <button onclick="showAdminLogin()" class="btn-main btn-secondary" style="margin-top: 15px; font-size: 0.7rem;">Admin Access</button>
    </div>

    <div id="dashboardPage" class="glass-card">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">
            <span class="brand-name" style="font-size: 1.2rem;">PakGold</span>
            <button id="logoutBtn" style="background:none; border:1px solid #ef4444; color:#ef4444; padding:4px 10px; border-radius:8px; font-size: 0.7rem; cursor:pointer;">Logout</button>
        </div>

        <div style="background: rgba(255,255,255,0.03); padding: 20px; border-radius: 20px; margin-bottom: 20px;">
            <span id="userTier" class="tier-tag">Starter Tier</span>
            <p style="font-size: 0.8rem; margin-top: 10px; color: #94a3b8;">Available Balance</p>
            <h1 style="font-size: 2.2rem; color: #fbbf24;">PKR <span id="userBal">0.00</span></h1>
            <button id="claimBtn" class="btn-main" style="background: #10b981; margin-top: 15px;">Claim Daily Bonus 💰</button>
        </div>

        <select id="actionType" style="background: #1e293b;">
            <option value="Deposit">Add Funds (Deposit)</option>
            <option value="Withdraw">Cash Out (Withdraw)</option>
        </select>
        <select id="method">
            <option value="JazzCash">JazzCash</option>
            <option value="EasyPaisa">EasyPaisa</option>
            <option value="Binance">Binance (USDT)</option>
        </select>
        <input type="number" id="amount" placeholder="Amount (Min 500)">
        <button onclick="submitRequest()" class="btn-main">Submit Request</button>

        <h4 style="text-align: left; margin-top: 25px; font-size: 0.9rem; color: #fbbf24;">Transaction History</h4>
        <table class="history-table">
            <thead><tr><th>Type</th><th>Amount</th><th>Status</th></tr></thead>
            <tbody id="txnHistory"></tbody>
        </table>

        <div style="margin-top: 20px; font-size: 0.65rem; background: rgba(0,0,0,0.3); padding: 10px; border-radius: 10px;">
            <p>Your Referral Link:</p>
            <code id="refLink" style="color: #fbbf24;">https://pakgold.app/ref=...</code>
        </div>
    </div>

    <div id="adminPage" class="glass-card" style="max-width: 800px;">
        <div style="display: flex; justify-content: space-between; align-items: center;">
            <h2 style="color: #fbbf24;">Admin Control 👑</h2>
            <button onclick="location.reload()" style="background: #ef4444; border:none; padding:5px 10px; border-radius:5px; color:white; cursor:pointer;">Exit</button>
        </div>
        
        <h4 style="margin-top: 20px; text-align: left;">Pending Requests</h4>
        <table class="history-table">
            <thead><tr><th>User</th><th>Type</th><th>Amt</th><th>Action</th></tr></thead>
            <tbody id="adminReqTable"></tbody>
        </table>
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

        // --- UI NAVIGATION ---
        window.acceptTerms = () => {
            document.getElementById('termsModal').style.display = 'none';
            localStorage.setItem('pakgold_terms', 'true');
        }
        if(localStorage.getItem('pakgold_terms')) document.getElementById('termsModal').style.display = 'none';

        window.showAdminLogin = () => {
            const pw = prompt("Enter Admin Password, sweetie:");
            if(pw === "admin123") {
                document.getElementById('loginPage').style.display = 'none';
                document.getElementById('adminPage').classList.add('active-block');
                loadAdminData();
            } else { alert("Wrong password! ❌"); }
        }

        document.getElementById('userLoginBtn').onclick = () => signInAnonymously(auth);
        document.getElementById('logoutBtn').onclick = () => signOut(auth).then(()=>location.reload());

        // --- AUTH OBSERVER ---
        onAuthStateChanged(auth, async (user) => {
            if(user) {
                document.getElementById('loginPage').style.display = 'none';
                document.getElementById('dashboardPage').classList.add('active-block');
                
                const userRef = doc(db, "users", user.uid);
                const snap = await getDoc(userRef);
                if(!snap.exists()) {
                    await setDoc(userRef, { balance: 0, tier: 'Starter', uid: user.uid, lastClaim: null });
                }

                // Live Update Dashboard
                onSnapshot(userRef, (d) => {
                    document.getElementById('userBal').innerText = d.data().balance.toFixed(2);
                    document.getElementById('userTier').innerText = d.data().tier + " Tier";
                    document.getElementById('refLink').innerText = `https://pakgold.github.io/?ref=${user.uid}`;
                });

                // Live History
                const qTxn = query(collection(db, "transactions"), where("uid", "==", user.uid));
                onSnapshot(qTxn, (s) => {
                    const h = document.getElementById('txnHistory');
                    h.innerHTML = "";
                    s.forEach(doc => {
                        const d = doc.data();
                        h.innerHTML += `<tr><td>${d.type}</td><td>PKR ${d.amount}</td><td style="color:${d.status=='approved'?'#10b981':'#fbbf24'}">${d.status}</td></tr>`;
                    });
                });
            } else {
                document.getElementById('loginPage').classList.add('active-block');
            }
        });

        // --- TRANSACTIONS ---
        window.submitRequest = async () => {
            const amt = Number(document.getElementById('amount').value);
            const type = document.getElementById('actionType').value;
            const mthd = document.getElementById('method').value;

            if(amt < 500) return alert("Min limit is PKR 500, sweetie! ✋");

            if(type === "Withdraw") {
                const uSnap = await getDoc(doc(db, "users", auth.currentUser.uid));
                if(uSnap.data().balance < amt) return alert("Insufficient Balance! ❌");
            }

            await addDoc(collection(db, "transactions"), {
                uid: auth.currentUser.uid,
                amount: amt,
                type: type,
                method: mthd,
                status: "pending",
                timestamp: serverTimestamp()
            });
            showNotify("Request Sent! Wait for approval. ✨");
        };

        // --- DAILY BONUS (Strict 24h) ---
        document.getElementById('claimBtn').onclick = async () => {
            const userRef = doc(db, "users", auth.currentUser.uid);
            const snap = await getDoc(userRef);
            const lastClaim = snap.data().lastClaim ? snap.data().lastClaim.toMillis() : 0;
            const now = Date.now();

            if(now - lastClaim < 86400000) {
                const hours = Math.floor((86400000 - (now - lastClaim)) / 3600000);
                return alert(`Wait ${hours} hours more, sweetie! ⏳`);
            }
            
            await updateDoc(userRef, { balance: increment(50), lastClaim: serverTimestamp() });
            showNotify("PKR 50 Bonus Claimed! 💰");
        };

        // --- ADMIN LOGIC ---
        function loadAdminData() {
            const q = query(collection(db, "transactions"), where("status", "==", "pending"));
            onSnapshot(q, (s) => {
                const table = document.getElementById('adminReqTable');
                table.innerHTML = "";
                s.forEach(docSnap => {
                    const d = docSnap.data();
                    table.innerHTML += `<tr>
                        <td>${d.uid.substring(0,5)}</td>
                        <td>${d.type}</td>
                        <td>${d.amount}</td>
                        <td><button onclick="approveTxn('${docSnap.id}', '${d.uid}', ${d.amount}, '${d.type}')" style="background:#10b981; color:white; border:none; padding:4px; border-radius:4px; cursor:pointer;">Approve</button></td>
                    </tr>`;
                });
            });
        }

        window.approveTxn = async (id, uid, amt, type) => {
            await updateDoc(doc(db, "transactions", id), { status: "approved" });
            const userRef = doc(db, "users", uid);
            const change = type === "Deposit" ? amt : -amt;
            await updateDoc(userRef, { balance: increment(change) });
            alert("Transaction Approved! ✅");
        };

        function showNotify(msg) {
            const n = document.getElementById('notify');
            n.innerText = msg; n.style.top = "20px";
            setTimeout(() => n.style.top = "-100px", 3000);
        }
    </script>
</body>
</html>
