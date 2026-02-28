<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Free People Search | FastPeopleSearch Clone</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        .fps-blue { background-color: #004a99; }
        .fps-text-blue { color: #004a99; }
        .fps-green { background-color: #4caf50; }
    </style>
</head>
<body class="bg-gray-100 font-sans">

    <nav class="fps-blue py-3 px-6 md:px-20 flex justify-between items-center text-white">
        <div class="text-2xl font-bold tracking-tighter">FastPeopleSearch</div>
        <div class="hidden md:flex space-x-6 text-sm font-medium">
            <a href="#" class="hover:underline">PEOPLE SEARCH</a>
            <a href="#" class="hover:underline">REVERSE PHONE</a>
            <a href="#" class="hover:underline">REVERSE ADDRESS</a>
        </div>
    </nav>

    <div class="max-w-5xl mx-auto mt-10 px-4">
        <div class="bg-white shadow-lg rounded-md overflow-hidden border border-gray-200">
            
            <div class="flex border-b">
                <button class="px-8 py-4 font-bold fps-text-blue border-b-4 border-blue-800 bg-white">NAME</button>
                <button class="px-8 py-4 font-bold text-gray-500 hover:bg-gray-50">PHONE</button>
                <button class="px-8 py-4 font-bold text-gray-500 hover:bg-gray-50">ADDRESS</button>
            </div>

            <div class="p-6 md:p-10">
                <h1 class="text-2xl font-bold text-gray-800 mb-6 text-center md:text-left">100% Free People Search</h1>
                
                <div class="grid grid-cols-1 md:grid-cols-4 gap-3">
                    <div class="md:col-span-1">
                        <label class="block text-xs font-bold text-gray-500 mb-1">FIRST NAME</label>
                        <input id="fName" type="text" placeholder="e.g. John" class="w-full p-3 border border-gray-300 rounded focus:border-blue-500 outline-none">
                    </div>
                    <div class="md:col-span-1">
                        <label class="block text-xs font-bold text-gray-500 mb-1">LAST NAME</label>
                        <input id="lName" type="text" placeholder="e.g. Smith" class="w-full p-3 border border-gray-300 rounded focus:border-blue-500 outline-none">
                    </div>
                    <div class="md:col-span-1">
                        <label class="block text-xs font-bold text-gray-500 mb-1">CITY, STATE OR ZIP</label>
                        <input id="city" type="text" placeholder="e.g. New York, NY" class="w-full p-3 border border-gray-300 rounded focus:border-blue-500 outline-none">
                    </div>
                    <div class="md:col-span-1 flex items-end">
                        <button onclick="runSearch()" class="w-full fps-green hover:bg-green-600 text-white font-bold py-3 px-4 rounded shadow-md flex items-center justify-center transition-all">
                            <i class="fa fa-search mr-2"></i> FREE SEARCH
                        </button>
                    </div>
                </div>
                <p class="mt-4 text-xs text-gray-400 italic text-center">By clicking "FREE SEARCH", you agree to our Terms of Service.</p>
            </div>
        </div>

        <div id="resultsBox" class="mt-8 pb-20 hidden">
            <h2 id="resultHeading" class="text-xl font-bold text-gray-700 mb-4 border-b pb-2"></h2>
            <div id="resultsList" class="space-y-4">
                </div>
        </div>
    </div>

    <script>
        // Sample Database for demo
        const directory = [
            { first: "John", last: "Smith", age: "45", location: "New York, NY", phone: "212-XXX-XXXX", relatives: "Mary Smith, David Smith" },
            { first: "John", last: "Doe", age: "29", location: "Los Angeles, CA", phone: "310-XXX-XXXX", relatives: "Jane Doe" },
            { first: "Sarah", last: "Miller", age: "38", location: "Chicago, IL", phone: "773-XXX-XXXX", relatives: "Tom Miller" },
            { first: "Ali", last: "Khan", age: "27", location: "Houston, TX", phone: "832-XXX-XXXX", relatives: "Ahmed Khan" }
        ];

        function runSearch() {
            const firstInput = document.getElementById('fName').value.trim().toLowerCase();
            const lastInput = document.getElementById('lName').value.trim().toLowerCase();
            const resultsBox = document.getElementById('resultsBox');
            const resultsList = document.getElementById('resultsList');
            
            // Search logic
            const filtered = directory.filter(p => {
                return (firstInput && p.first.toLowerCase().includes(firstInput)) || 
                       (lastInput && p.last.toLowerCase().includes(lastInput));
            });

            resultsList.innerHTML = "";
            
            if (filtered.length > 0) {
                resultsBox.classList.remove('hidden');
                document.getElementById('resultHeading').innerText = `Found ${filtered.length} matching records`;
                
                filtered.forEach(person => {
                    resultsList.innerHTML += `
                        <div class="bg-white p-5 rounded border border-gray-200 shadow-sm flex flex-col md:flex-row justify-between hover:bg-gray-50 transition">
                            <div class="space-y-1">
                                <h3 class="text-xl font-bold fps-text-blue uppercase">${person.first} ${person.last}</h3>
                                <p class="text-sm"><strong>Age:</strong> ${person.age}</p>
                                <p class="text-sm"><strong>Current City:</strong> ${person.location}</p>
                                <p class="text-sm text-gray-500"><strong>Possible Relatives:</strong> ${person.relatives}</p>
                            </div>
                            <div class="mt-4 md:mt-0 flex items-center">
                                <button class="bg-blue-50 text-blue-700 font-bold py-2 px-6 border border-blue-200 rounded hover:bg-blue-100">
                                    VIEW FREE DETAILS
                                </button>
                            </div>
                        </div>
                    `;
                });
            } else {
                alert("Koi record nahi mila, sweetie! Try searching for 'John' or 'Ali'.");
            }
        }
    </script>
</body>
</html>
