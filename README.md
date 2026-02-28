<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>FastPeopleSearch - Live Firebase Version</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100">

    <div class="max-w-4xl mx-auto mt-20 p-6 bg-white shadow-lg rounded-lg">
        <h1 class="text-3xl font-bold text-blue-800 mb-6 italic">FastPeopleSearch Clone</h1>
        
        <div class="flex gap-2 mb-8">
            <input id="searchName" type="text" placeholder="Enter First Name (e.g. John)" class="flex-1 p-3 border rounded outline-none focus:ring-2 focus:ring-blue-500">
            <button onclick="searchFirebase()" class="bg-green-600 text-white px-8 py-3 rounded font-bold hover:bg-green-700">SEARCH</button>
        </div>

        <div id="results" class="space-y-4">
            </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getFirestore, collection, query, where, getDocs } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        const firebaseConfig = {
            apiKey: "AIzaSyCSD1O9tV7xDZu_kljq-0NMhA2DqtW5quE",
            authDomain: "live-chat-b810c.firebaseapp.com",
            projectId: "live-chat-b810c",
            storageBucket: "live-chat-b810c.firebasestorage.app",
            messagingSenderId: "555058795334",
            appId: "1:555058795334:web:f668887409800c32970b47"
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        window.searchFirebase = async function() {
            const name = document.getElementById('searchName').value.trim();
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = "<p class='text-blue-500'>Searching Firebase...</p>";

            try {
                const q = query(collection(db, "people"), where("firstName", "==", name));
                const querySnapshot = await getDocs(q);

                resultsDiv.innerHTML = "";
                if (querySnapshot.empty) {
                    resultsDiv.innerHTML = "<p class='text-red-500'>No results found, sweetie!</p>";
                } else {
                    querySnapshot.forEach((doc) => {
                        const data = doc.data();
                        resultsDiv.innerHTML += `
                            <div class="p-4 border-l-4 border-blue-600 bg-gray-50 rounded shadow-sm">
                                <h2 class="text-xl font-bold text-blue-700 uppercase">${data.firstName} ${data.lastName}</h2>
                                <p>Age: ${data.age} | City: ${data.city}</p>
                            </div>
                        `;
                    });
                }
            } catch (error) {
                console.error(error);
                resultsDiv.innerHTML = "<p class='text-red-500'>Error: Check Firebase Rules!</p>";
            }
        }
    </script>
</body>
</html>
