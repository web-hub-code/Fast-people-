<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FastPeopleSearch Pro | Live Search Engine</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        .fps-blue { background-color: #003366; }
        .fps-green { background-color: #4caf50; }
        .result-card { border-left: 6px solid #003366; transition: 0.3s; }
        .result-card:hover { transform: translateY(-2px); shadow: lg; }
    </style>
</head>
<body class="bg-gray-50 font-sans">

    <header class="fps-blue text-white py-4 px-6 md:px-20 flex justify-between items-center shadow-md">
        <div class="text-2xl font-black italic tracking-tighter cursor-pointer">FastPeopleSearch</div>
        <nav class="hidden md:flex space-x-8 text-xs font-bold uppercase tracking-widest">
            <a href="#" class="hover:underline">People Search</a>
            <a href="#" class="hover:underline">Reverse Phone</a>
            <a href="#" class="hover:underline">Address</a>
        </nav>
    </header>

    <div class="max-w-5xl mx-auto mt-12 px-4">
        <h1 class="text-3xl md:text-5xl font-extrabold text-center text-gray-900 mb-8 tracking-tight">100% Free People Search</h1>

        <div class="bg-white rounded-lg shadow-2xl border border-gray-200 overflow-hidden">
            <div class="flex bg-gray-100 border-b">
                <button class="px-10 py-5 font-bold text-sm border-b-4 border-blue-900 bg-white text-blue-900 uppercase">Name Search</button>
                <button class="px-10 py-5 font-bold text-sm text-gray-400 hover:bg-gray-50 uppercase">Phone</button>
                <button class="px-10 py-5 font-bold text-sm text-gray-400 hover:bg-gray-50 uppercase">Address</button>
            </div>

            <div class="p-8 grid grid-cols-1 md:grid-cols-3 gap-4">
                <input id="qName" type="text" placeholder="First & Last Name (e.g. John Smith)" class="p-4 border border-gray-300 rounded focus:ring-2 focus:ring-blue-800 outline-none text-lg">
                <input id="qLocation" type="text" placeholder="City, State or ZIP" class="p-4 border border-gray-300 rounded focus:ring-2 focus:ring-blue-800 outline-none text-lg">
                <button onclick="masterSearch()" class="fps-green hover:bg-green-600 text-white font-black text-xl py-4 rounded shadow-lg transition-all flex items-center justify-center">
                    <i class="fa fa-search mr-3"></i> FREE SEARCH
                </button>
            </div>
            <p class="text-center pb-4 text-[10px] text-gray-400 uppercase font-bold tracking-widest italic">The most accurate free people search on the web</p>
        </div>

        <div id="resultsArea" class="mt-12 mb-20 hidden">
            <h2 class="text-2xl font-bold text-gray-700 mb-6 border-b-2 border-gray-200 pb-2 flex items-center">
                <i class="fas fa-list-ul mr-3 text-blue-900"></i> LIVE RECORDS FOUND
            </h2>
            <div id="resultsList" class="space-y-6">
                </div>
        </div>
    </div>

    <script>
        // Sweetie, yahan aapko apni Google API Key lagani hogi (Main niche batati hoon kaise)
        const GOOGLE_API_KEY = "YOUR_GOOGLE_API_KEY"; 
        const CX_ID = "YOUR_SEARCH_ENGINE_ID";

        async function masterSearch() {
            const name = document.getElementById('qName').value.trim();
            const location = document.getElementById('qLocation').value.trim();
            const list = document.getElementById('resultsList');
            const area = document.getElementById('resultsArea');

            if(!name) { alert("Sweetie, please enter a name!"); return; }

            list.innerHTML = `<div class="text-center py-10"><i class="fas fa-spinner fa-spin text-4xl text-blue-900"></i><p class="mt-2 font-bold text-blue-900">Searching Real Public Records...</p></div>`;
            area.classList.remove('hidden');

            try {
                // Google Custom Search API Call (Live Search)
                const searchQuery = `${name} ${location} people search address phone`;
                const response = await fetch(`https://www.googleapis.com/customsearch/v1?key=${GOOGLE_API_KEY}&cx=${CX_ID}&q=${searchQuery}`);
                const data = await response.json();

                list.innerHTML = "";
                
                if (data.items) {
                    data.items.slice(0, 5).forEach(item => {
                        list.innerHTML += `
                            <div class="result-card bg-white p-6 rounded shadow hover:shadow-xl transition flex flex-col md:flex-row justify-between items-start md:items-center">
                                <div class="flex-1">
                                    <h3 class="text-2xl font-black text-blue-800 uppercase hover:underline cursor-pointer tracking-tight">${item.title}</h3>
                                    <p class="text-sm text-gray-600 mt-2 leading-relaxed">${item.snippet}</p>
                                    <p class="text-xs text-green-600 font-bold mt-2 uppercase tracking-tighter"><i class="fas fa-link mr-1"></i> Source: ${item.displayLink}</p>
                                </div>
                                <a href="${item.link}" target="_blank" class="mt-4 md:mt-0 bg-blue-50 text-blue-900 font-black py-3 px-8 border border-blue-200 rounded-lg hover:bg-blue-900 hover:text-white transition uppercase text-xs shadow-sm">
                                    View Full Record
                                </a>
                            </div>
                        `;
                    });
                } else {
                    list.innerHTML = `<div class="bg-red-50 p-6 text-red-700 rounded border border-red-200">No public records found for this name. Try a different variation, sweetie!</div>`;
                }
            } catch (error) {
                list.innerHTML = `<p class="text-red-500 font-bold">Error: Sweetie, API Key check karein!</p>`;
            }
        }
    </script>
</body>
</html>
