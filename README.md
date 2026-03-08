<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PakGold | Elite Mining Farm 🇵🇰</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root { --gold: #fbbf24; --bg: #020617; --glass: rgba(255, 255, 255, 0.03); --green: #10b981; }
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Outfit', sans-serif; }
        body { background: var(--bg); color: white; min-height: 100vh; overflow-x: hidden; }
        
        /* Layouts */
        .glass-card { background: var(--glass); backdrop-filter: blur(20px); border: 1px solid rgba(255,255,255,0.08); border-radius: 24px; padding: 20px; width: 92%; max-width: 450px; margin: 15px auto; }
        .brand-name { font-size: 2.5rem; font-weight: 600; color: var(--gold); text-align: center; margin: 20px 0; user-select: none; }
        
        /* Buttons & Inputs */
        input, select { width: 100%; padding: 14px; background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1); border-radius: 14px; color: white; margin-bottom: 12px; outline: none; font-size: 0.9rem; }
        .btn-main { width: 100%; padding: 15px; background: var(--gold); border: none; border-radius: 14px; color: #000; font-weight: 700; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        .btn-main:hover { transform: scale(1.02); box-shadow: 0 10px 20px rgba(251,191,36,0.2); }
        
        /* Grid & Components */
        .m-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 15px; }
        .m-card { background: var(--glass); border: 1px solid rgba(255,255,255,0.05); padding: 12px; border-radius: 18px; text-align: center; position: relative; }
        .hot-tag { position: absolute; top: 5px; right: 5px; background: #ef4444; font-size: 0.5rem; padding: 2px 6px; border-radius: 4px; }
        
        #welcomeScreen, #authSection, #dashboardPage, #adminPage, #adminTrigger, #termsModal { display: none; }
        .active { display: block !important; }
        .flex-active { display: flex !important; }

        /* Animation */
        @keyframes pulse { 0% { opacity: 0.5; } 50% { opacity: 1; } 100% { opacity: 0.5; } }
    </style>
</head>
<body>

    <div id="welcomeScreen" class="flex-active" style="position:fixed; top:0; left:0; width:100%; height:100%; background:#020617; z-index:10000; flex-direction:column; align-items:center; justify-content:center; text-align:center;">
        <div class="brand-name" style="font-size: 3.5rem;">PakGold</div>
        <p style="color: #94a3b8; letter-spacing: 2px; font-size: 0.8rem;">THE FUTURE OF MINING</p>
        <button onclick="enterApp()" class="btn-main" style="margin-top: 40px; width: 200px;">Get Started 🚀</button>
    </div>

    <div id="authSection">
        <h1 class="brand-name" id="secretTap">PakGold</h1>
        <div class="glass-card">
            <h2 id="authTitle" style="margin-bottom: 20px; font-size: 1.2rem;">Welcome Back</h2>
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <div id="signupExtra" style="display:none;"><input type="text" id="refBy" placeholder="Referral Code (Optional)"></div>
            <button id="authBtn" class="btn-main">Login</button>
            <p id="toggleAuth" style="font-size: 0.8rem; color: #94a3b8; margin-top: 20px; text-align: center; cursor: pointer;">New here? <span style="color: var(--gold)">Create Account</span></p>
            
            <div id="adminTrigger" style="margin-top: 15px;">
                <input type="password" id="adminKey" placeholder="Admin Key" style="border-color: #ef4444;">
                <button onclick="checkAdmin()" class="btn-main" style="background:#ef4444; color:white;">Open Vault</button>
            </div>
        </div>
    </div>

    <div id="dashboardPage">
        <div class="glass-card" style="margin-top:10px; padding:10px; border-color:#4facfe;">
            <marquee id="adminNotice" style="font-size: 0.7rem; color: #cbd5e1;">Welcome to PakGold! Start your mining journey today. ⛏️</marquee>
        </div>

        <div class="glass-card">
            <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px;">
                <span style="color: var(--gold); font-size: 0.8rem;">Hello, <b id="displayUser"></b></span>
                <button onclick="logout()" style="background:none; border:none; color:#ef4444; font-size:0.7rem;">Logout</button>
            </div>
            
            <div style="text-align: center; background:rgba(251,191,36,0.05); padding:20px; border-radius:20px; border:1px solid rgba(251,191,36,0.1);">
                <p style="font-size: 0.7rem; color: #94a3b8;">Mining Balance</p>
                <h1 style="color: var(--gold); font-size: 2.2rem; margin: 5px 0;">PKR <span id="bal">0.00</span></h1>
                <p style="font-size: 0.6rem; color: var(--green);">Rig: <span id="machine">None</span></p>
            </div>

            <button id="claimBtn" class="btn-main" style="background: var(--green); color: white; margin-top: 15px;">Claim Daily Yield ⛏️</button>
            
            <div style="margin-top: 15px; text-align: center;">
                <button onclick="claimLucky()" id="luckyBtn" style="background:linear-gradient(90deg, #ec4899, #8b5cf6); border:none; border-radius:12px; color:white; padding:10px; width:100%; font-size:0.7rem; font-weight:bold; cursor:pointer;">🎁 Daily Lucky Box (Win up to 20)</button>
            </div>

            <div style="display:grid; grid-template-columns: 1fr 1fr; gap:10px; margin-top:15px;">
                <div class="m-card" style="font-size:0.6rem;">Invite: <b id="myCode" style="color:var(--gold);"></b><br><button onclick="copyRef()" style="background:var(--gold); border:none; border-radius:4px; padding:2px 5px; margin-top:4px;">Copy Link</button></div>
                <div class="m-card" style="font-size:0.6rem;">Recent Payouts<div id="payoutScroll" style="height:20px; overflow:hidden; color:var(--green); margin-top:4px;"></div></div>
            </div>

            <h4 style="margin-top: 20px; font-size: 0.8rem; color: var(--gold);">Mining Rig Store 🏪</h4>
            <div id="mGrid" class="m-grid"></div>
        </div>
    </div>

    <div id="adminPage" class="glass-card">
        <h2 style="color: #ef4444; margin-bottom: 15px;">Admin Dashboard</h2>
        <div id="adminList"></div>
        <input type="text" id="newNotice" placeholder="New Notice..." style="margin-top:20px;">
        <button onclick="updateNotice()" class="btn-main" style="padding:10px;">Update Notice</button>
        <button onclick="location.reload()" class="btn-main" style="background:#334155; color:white; margin-top:10px;">Exit</button>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-app.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, onSnapshot, collection, query, where, increment, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/9.17.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCMG6KG_oD8cjEk4YpbxXik-C5q8K5MDHk",
            authDomain: "dark-web-9.firebaseapp.com",
            projectId: "dark-web-9",
            storageBucket: "dark-web-9.firebasestorage.app",
            messagingSenderId: "564328425161",
            appId: "1:564328425161:web:eb109ab77356dafe7f4f18"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        // --- AUTH LOGIC ---
        let isLogin = true;
        let currentUser = localStorage.getItem('pakgold_user');

        window.enterApp = () => {
            document.getElementById('welcomeScreen').style.display = 'none';
            if(!currentUser) document.getElementById('authSection').classList.add('active');
            else showDashboard();
        };

        document.getElementById('toggleAuth').onclick = () => {
            isLogin = !isLogin;
            document.getElementById('authTitle').innerText = isLogin ? "Welcome Back" : "Create Account";
            document.getElementById('authBtn').innerText = isLogin ? "Login" : "Register";
            document.getElementById('signupExtra').style.display = isLogin ? 'none' : 'block';
        };

        document.getElementById('authBtn').onclick = async () => {
            const user = document.getElementById('username').value.toLowerCase().trim();
            const pass = document.getElementById('password').value;
            const ref = document.getElementById('refBy').value.toLowerCase().trim();
            
            if(!user || !pass) return alert("Fill all fields sweetie! 😘");
            const uRef = doc(db, "users", user);
            const snap = await getDoc(uRef);

            if(isLogin) {
                if(snap.exists() && snap.data().pass === pass) login(user);
                else alert("Wrong credentials!");
            } else {
                if(snap.exists()) return alert("Username taken!");
                await setDoc(uRef, { user, pass, balance: 0, machine: "None", profit: 0, lastClaim: 0, lastLucky: 0, referredBy: ref || null });
                login(user);
            }
        };

        function login(u) { localStorage.setItem('pakgold_user', u); location.reload(); }
        window.logout = () => { localStorage.removeItem('pakgold_user'); location.reload(); };

        // --- DASHBOARD ---
        function showDashboard() {
            document.getElementById('authSection').classList.remove('active');
            document.getElementById('dashboardPage').classList.add('active');
            document.getElementById('displayUser').innerText = currentUser;
            document.getElementById('myCode').innerText = currentUser;

            onSnapshot(doc(db, "users", currentUser), (d) => {
                const data = d.data();
                document.getElementById('bal').innerText = data.balance.toFixed(2);
                document.getElementById('machine').innerText = data.machine;
            });
            
            onSnapshot(doc(db, "settings", "global"), (d) => {
                if(d.exists()) document.getElementById('adminNotice').innerText = d.data().notice;
            });
        }

        // --- RIGS ---
        const machines = [
            {n:"Nano",p:200,d:14,t:"reg"}, {n:"Micro",p:500,d:35,t:"reg"},
            {n:"Elite",p:1000,d:75,t:"reg"}, {n:"Pro",p:2000,d:160,t:"reg"},
            {n:"Dragon 🔥",p:3000,d:350,t:"hot"}, {n:"Quantum 🔥",p:10000,d:1500,t:"hot"}
        ];
        machines.forEach(m => {
            document.getElementById('mGrid').innerHTML += `
                <div class="m-card">
                    ${m.t=='hot'?'<div class="hot-tag">SPECIAL</div>':''}
                    <p><b>${m.n}</b></p>
                    <p style="color:var(--green); font-size:0.6rem;">+PKR ${m.d}/day</p>
                    <button onclick="buyM('${m.n}',${m.p},${m.d})" class="btn-main" style="padding:5px; margin-top:5px; font-size:0.6rem;">PKR ${m.p}</button>
                </div>`;
        });

        window.buyM = async (n, p, d) => {
            const uRef = doc(db, "users", currentUser);
            const snap = await getDoc(uRef);
            if(snap.data().balance < p) return alert("Low balance sweetie! 😘");
            
            // Referral Bonus logic
            if(snap.data().referredBy) {
                const rRef = doc(db, "users", snap.data().referredBy);
                if((await getDoc(rRef)).exists()) await updateDoc(rRef, { balance: increment(50) });
            }
            
            await updateDoc(uRef, { balance: increment(-p), machine: n, profit: d });
            alert(n + " Rig Online! ⚡");
        };

        // --- REWARDS ---
        document.getElementById('claimBtn').onclick = async () => {
            const uRef = doc(db, "users", currentUser);
            const d = (await getDoc(uRef)).data();
            if(d.profit === 0) return alert("No active rig!");
            if(Date.now() - d.lastClaim < 86400000) return alert("Wait 24h!");
            await updateDoc(uRef, { balance: increment(d.profit), lastClaim: Date.now() });
            alert("Yield Collected! 💰");
        };

        window.claimLucky = async () => {
            const uRef = doc(db, "users", currentUser);
            const d = (await getDoc(uRef)).data();
            if(Date.now() - d.lastLucky < 86400000) return alert("Tomorrow sweetie! 😘");
            const prizes = [2, 5, 7, 10, 15, 20, 0];
            const p = prizes[Math.floor(Math.random()*prizes.length)];
            if(p===0) alert("Try Next Time! 💔"); else alert("Won PKR "+p+"! 🎉");
            await updateDoc(uRef, { balance: increment(p), lastLucky: Date.now() });
        };

        // --- GHOST ADMIN ---
        let taps = 0;
        document.getElementById('secretTap').onclick = () => {
            taps++; if(taps===4) { document.getElementById('adminTrigger').style.display='block'; taps=0; }
        };
        window.checkAdmin = () => {
            if(document.getElementById('adminKey').value === "admin123") {
                document.getElementById('authSection').style.display='none';
                document.getElementById('adminPage').classList.add('active');
            }
        };

        // --- FAKE PAYOUTS ---
        setInterval(() => {
            const names = ["Ali", "Sana", "Hamza", "Zain", "Mehak", "Sara", "Waqas"];
            const amts = [200, 500, 1000, 1500, 300];
            document.getElementById('payoutScroll').innerText = `@${names[Math.floor(Math.random()*names.length)]} withdrawn PKR ${amts[Math.floor(Math.random()*amts.length)]} ✅`;
        }, 3000);

        if(currentUser) enterApp();
    </script>
</body>
</html>
