<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>People Search Pro - Working Version</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
</head>
<body class="bg-gray-100 font-sans min-h-screen">

    <nav class="bg-white shadow-md py-4 px-6 flex justify-between items-center border-b-4 border-blue-600">
        <div class="text-2xl font-black text-blue-700 tracking-tighter">FAST<span class="text-gray-800">PEOPLE</span>SEARCH</div>
        <div class="hidden md:flex space-x-6 font-semibold text-gray-600">
            <a href="#" class="hover:text-blue-600">PEOPLE SEARCH</a>
            <a href="#" class="hover:text-blue-600">REVERSE PHONE</a>
            <a href="#" class="hover:text-blue-600">ADDRESS</a>
        </div>
    </nav>

    <div class="max-w-5xl mx-auto mt-12 px-4 text-center">
        <h1 class="text-3xl md:text-5xl font-bold text-gray-900 mb-2">Free People Search</h1>
        <p class="text-lg text-gray-600 mb-8">Get instant results: addresses, phone numbers, and relatives.</p>

        <div class="bg-white p-2 md:p-6 shadow-2xl rounded-xl border border-gray-200">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                <input id="firstName" type="text" placeholder="First Name (e.g. John)" class="p-4 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none">
                <input id="lastName" type="text" placeholder="Last Name (e.g. Smith)" class="p-4 border rounded-lg focus:ring-2 focus:ring-blue-500 outline-none">
                <button onclick="performSearch()" class="bg-green-600 hover:bg-green-700 text-white font-black text-xl py-4 rounded-lg shadow-lg transition-all transform hover:scale-105 uppercase">
                    <i class="fa fa-search mr-2"></i> Search
                </button>
            </div>
        </div>
    </div>

    <div id="resultsSection" class="max-w-4xl mx-auto mt-12 px-4 pb-20 hidden">
        <div class="flex justify-between items-center mb-6">
            <h2 class="text-2xl font-bold text-gray-800">Search Results</h2>
            <span id="resultCount" class="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-bold"></span>
        </div>
        <div id="resultsList" class="space-y-4">
            </div>
    </div>

    <div id="noResults" class="max-w-4xl mx-auto mt-12 px-4 text-center hidden">
        <div class="bg-yellow-50 border border-yellow-200 p-8 rounded-xl text-yellow-800">
            <i class="fas fa-exclamation-triangle text-4xl mb-4"></i>
            <p class="text-xl font-bold">No results found for that name.</p>
            <p>Try searching for 'John' or 'Sarah' to see the demo data.</p>
        </div>
    </div>

    <script>
        // Sample Database (Sweetie, aap yahan aur names add kar sakti hain)
        const peopleData = [
            { first: "John", last: "Smith", age: 45, location: "New York, NY", phone: "(212) 555-0198", relatives: "Mary Smith, David Smith" },
            { first: "Sarah", last: "Jones", age: 32, location: "Los Angeles, CA", phone: "(310) 998-1234", relatives: "Mark Jones, Linda Jones" },
            { first: "John", last: "Doe", age: 29, location: "Chicago, IL", phone: "(773) 442-9000", relatives: "Jane Doe" },
            { first: "Ali", last: "Khan", age: 27, location: "Houston, TX", phone: "(832) 111-2233", relatives: "Ahmed Khan" }
        ];

        function performSearch() {
            const fName = document.getElementById('firstName').value.trim().toLowerCase();
            const lName = document.getElementById('lastName').value.trim().toLowerCase();
            const resultsList = document.getElementById('resultsList');
            const resultsSection = document.getElementById('resultsSection');
            const noResults = document.getElementById('noResults');
            
            // Filter logic
            const filtered = peopleData.filter(person => {
                const matchFirst = fName === "" || person.first.toLowerCase().includes(fName);
                const matchLast = lName === "" || person.last.toLowerCase().includes(lName);
                return matchFirst && matchLast;
            });

            // Display Logic
            resultsList.innerHTML = "";
            if (filtered.length > 0 && (fName !== "" || lName !== "")) {
                resultsSection.classList.remove('hidden');
                noResults.classList.add('hidden');
                document.getElementById('resultCount').innerText = `${filtered.length} Person(s) Found`;

                filtered.forEach(person => {
                    resultsList.innerHTML += `
                        <div class="bg-white p-6 rounded-xl shadow-md border-l-8 border-blue-600 flex flex-col md:flex-row justify-between items-start md:items-center hover:shadow-xl transition-shadow">
                            <div>
                                <h3 class="text-2xl font-black text-blue-800 uppercase">${person.first} ${person.last}</h3>
                                <p class="text-gray-700 font-bold mt-1">Age: ${person.age} <span class="mx-2 text-gray-300">|</span> Lives in: ${person.location}</p>
                                <div class="mt-4 grid grid-cols-1 md:grid-cols-2 gap-2 text-sm">
                                    <p class="text-gray-600"><i class="fas fa-phone mr-2 text-green-600"></i>${person.phone}</p>
                                    <p class="text-gray-600"><i class="fas fa-users mr-2 text-blue-400"></i>Relatives: ${person.relatives}</p>
                                </div>
                            </div>
                            <button class="mt-4 md:mt-0 bg-blue-600 text-white px-6 py-2 rounded-lg font-bold hover:bg-blue-700 shadow">FREE DETAILS</button>
                        </div>
                    `;
                });
            } else {
                resultsSection.classList.add('hidden');
                noResults.classList.remove('hidden');
            }
        }
    </script>
</body>
</html>
