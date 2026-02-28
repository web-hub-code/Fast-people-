<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Free People Search | Official Fast Search Clone</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        .nav-blue { background-color: #003366; }
        .btn-green { background-color: #4caf50; }
        .text-link { color: #0066cc; }
        .bg-grey-f6 { background-color: #f6f6f6; }
    </style>
</head>
<body class="bg-white font-sans text-gray-800">

    <header class="nav-blue text-white py-3 px-4 md:px-20 flex justify-between items-center shadow-lg">
        <div class="text-2xl font-black italic tracking-tighter cursor-pointer">FastPeopleSearch</div>
        <nav class="hidden md:flex space-x-6 text-xs font-bold uppercase tracking-widest">
            <a href="#" class="hover:underline">People Search</a>
            <a href="#" class="hover:underline">Phone Lookup</a>
            <a href="#" class="hover:underline">Address Search</a>
        </nav>
    </header>

    <div class="bg-grey-f6 py-2 px-4 md:px-20 text-xs text-gray-500 border-b">
        <span class="text-link">Home</span> > <span class="text-link">People Directory</span> > Search
    </div>

    <main class="max-w-5xl mx-auto mt-8 px-4">
        <h1 class="text-3xl md:text-4xl font-extrabold text-center text-gray-900 mb-8">100% Free People Search</h1>

        <div class="bg-white border rounded shadow-sm overflow-hidden mb-12">
            <div class="flex bg-gray-50 border-b">
                <button class="px-8 py-4 font-bold text-sm border-b-4 border-blue-900 bg-white">BY NAME</button>
                <button class="px-8 py-4 font-bold text-sm text-gray-500 hover:bg-gray-100">BY PHONE</button>
                <button class="px-8 py-4 font-bold text-sm text-gray-500 hover:bg-gray-100">BY ADDRESS</button>
            </div>

            <div class="p-6 md:p-8 grid grid-cols-1 md:grid-cols-3 gap-4">
                <div>
                    <label class="block text-[10px] font-bold text-gray-400 uppercase mb-1">First Name</label>
                    <input id="fName" type="text" placeholder="John" class="w-full p-3 border rounded focus:ring-1 focus:ring-blue-500 outline-none">
                </div>
                <div>
                    <label class="block text-[10px] font-bold text-gray-400 uppercase mb-1">Last Name</label>
                    <input id="lName" type="text" placeholder="Smith" class="w-full p-3 border rounded focus:ring-1 focus:ring-blue-500 outline-none">
                </div>
                <div>
                    <label class="block text-[10px] font-bold text-gray-400 uppercase mb-1">City, State or ZIP</label>
                    <div class="flex">
                        <input id="city" type="text" placeholder="New York, NY" class="w-full p-3 border rounded-l focus:ring-1 focus:ring-blue-500 outline-none">
                        <button onclick="runSearch()" class="btn-green hover:bg-green-600 text-white font-bold px-6 rounded-r">
                            <i class="fa fa-search"></i>
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <div id="resultsArea" class="hidden mb-16">
            <h2 id="countLabel" class="text-xl font-bold mb-4 border-b pb-2"></h2>
            <div id="resultsList" class="space-y-4"></div>
        </div>

        <section class="border-t pt-10 pb-20">
            <h2 class="text-xl font-bold mb-6">Free People Directory</h2>
            <div class="grid grid-cols-4 md:grid-cols-8 gap-2 text-center">
                <script>
                    const alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('');
                    alphabet.forEach(letter => {
                        document.write(`<a href="#" class="bg-gray-100 p-2 text-link font-bold hover:bg-blue-100 rounded text-sm transition">${letter}</a>`);
                    });
                </script>
            </div>
            <p class="mt-8 text-sm text-gray-600 leading-relaxed">
                FastPeopleSearch clone helps you find people easily. Our 100% free people search finds current addresses, phone numbers, and relatives for almost every adult in the US.
            </p>
        </section>
    </main>

    <footer class="bg-gray-100 py-10 px-4 md:px-20 border-t text-xs text-gray-500 flex justify-between italic">
        <div>© 2026 People Search Clone. All rights reserved.</div>
        <div class="space-x-4">
            <span class="hover:underline cursor-pointer">Privacy</span>
            <span class="hover:underline cursor-pointer">Terms</span>
            <span class="hover:underline cursor-pointer">Contact</span>
        </div>
    </footer>

    <script>
        // Demo Data
        const people = [
            { first: "John", last: "Smith", age: "45", city: "New York, NY", phone: "212-555-0101", relatives: "Mary Smith, David Smith" },
            { first: "Sarah", last: "Jones", age: "32", city: "Miami, FL", phone: "305-998-1234", relatives: "Mark Jones" },
            { first: "John", last: "Doe", age: "29", city: "Chicago, IL", phone: "773-442-9000", relatives: "Jane Doe" }
        ];

        function runSearch() {
            const f = document.getElementById('fName').value.toLowerCase();
            const l = document.getElementById('lName').value.toLowerCase();
            const list = document.getElementById('resultsList');
            const area = document.getElementById('resultsArea');

            const filtered = people.filter(p => (f && p.first.toLowerCase().includes(f)) || (l && p.last.toLowerCase().includes(l)));

            list.innerHTML = "";
            if(filtered.length > 0) {
                area.classList.remove('hidden');
                document.getElementById('countLabel').innerText = `${filtered.length} People Found`;
                filtered.forEach(p => {
                    list.innerHTML += `
                        <div class="bg-white p-6 border rounded shadow-sm hover:shadow-md flex flex-col md:flex-row justify-between items-start transition">
                            <div class="w-full">
                                <div class="text-xs font-bold text-gray-400 uppercase mb-1">Full Name</div>
                                <h3 class="text-2xl font-black text-link uppercase mb-3 underline cursor-pointer">${p.first} ${p.last}</h3>
                                <div class="grid grid-cols-2 md:grid-cols-4 gap-4 text-sm mt-2">
                                    <div><span class="font-bold block uppercase text-[10px] text-gray-400">Age</span> ${p.age}</div>
                                    <div><span class="font-bold block uppercase text-[10px] text-gray-400">Location</span> ${p.city}</div>
                                    <div class="col-span-2"><span class="font-bold block uppercase text-[10px] text-gray-400">Possible Relatives</span> ${p.relatives}</div>
                                </div>
                            </div>
                            <div class="mt-6 md:mt-0 md:ml-6 w-full md:w-auto">
                                <button class="w-full bg-blue-50 text-link font-bold py-3 px-8 border border-blue-200 rounded hover:bg-blue-100 transition shadow-sm uppercase text-xs">View Free Details</button>
                            </div>
                        </div>
                    `;
                });
            } else {
                alert("Koi record nahi mila, sweetie! 'John' search karein.");
            }
        }
    </script>
</body>
</html>
