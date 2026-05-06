<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EduFlow Pro | Secure School Management</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .glass { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border: 1px solid #e2e8f0; }
        .screen { display: none; animation: slideUp 0.4s ease-out; }
        .active { display: block; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        input:focus { border-color: #4f46e5; ring: 2px; ring-color: #c7d2fe; outline: none; }
    </style>
</head>
<body class="bg-slate-50 min-h-screen font-sans text-slate-900">

    <nav class="bg-white border-b px-6 py-4 flex justify-between items-center sticky top-0 z-50">
        <h1 class="text-2xl font-black text-indigo-600 tracking-tighter">EDUFLOW <span class="text-slate-400 font-light">PRO</span></h1>
        <div id="user-info" class="text-sm font-medium text-slate-500"></div>
        <button id="logout-btn" onclick="logout()" class="hidden bg-red-50 text-red-600 px-4 py-2 rounded-lg text-sm font-bold hover:bg-red-100">Logout</button>
    </nav>

    <main class="max-w-6xl mx-auto p-4 md:p-8">

        <section id="screen-login" class="screen active">
            <div class="max-w-md mx-auto mt-10">
                <div class="bg-white p-8 rounded-3xl shadow-xl border border-slate-200">
                    <h2 class="text-2xl font-bold mb-6 text-center">System Portal</h2>
                    
                    <div class="space-y-4">
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-500 mb-1">User ID</label>
                            <input id="login-id" type="text" placeholder="e.g. 10A-1 or teacher_01" class="w-full border rounded-xl p-3">
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase text-slate-500 mb-1">Password</label>
                            <input id="login-pass" type="password" placeholder="••••••••" class="w-full border rounded-xl p-3">
                        </div>
                        <button onclick="handleGlobalLogin()" class="w-full bg-indigo-600 text-white p-4 rounded-xl font-bold hover:bg-indigo-700 transition-all shadow-lg shadow-indigo-200">Sign In</button>
                    </div>

                    <div class="mt-8 pt-6 border-t border-slate-100 text-center">
                        <p class="text-xs text-slate-400">Admin: admin | Pass: master789</p>
                        <p class="text-xs text-slate-400">Teacher: teacher | Pass: staff123</p>
                    </div>
                </div>
            </div>
        </section>

        <section id="screen-teacher" class="screen">
            <div class="grid grid-cols-1 md:grid-cols-4 gap-6">
                <div class="md:col-span-1 space-y-4">
                    <div class="bg-white p-6 rounded-2xl border shadow-sm">
                        <h3 class="font-bold mb-4">Class Management</h3>
                        <select id="t-class-select" class="w-full border p-2 rounded-lg mb-3">
                            <option value="10A">Class 10A</option>
                            <option value="10B">Class 10B</option>
                            <option value="9A">Class 9A</option>
                        </select>
                        <button onclick="loadClassList()" class="w-full bg-indigo-600 text-white py-2 rounded-lg text-sm">Load Students</button>
                    </div>
                </div>
                <div class="md:col-span-3 bg-white rounded-2xl border shadow-sm overflow-hidden">
                    <table class="w-full text-left border-collapse">
                        <thead class="bg-slate-50 border-b">
                            <tr>
                                <th class="p-4 text-xs font-bold uppercase text-slate-500">Roll</th>
                                <th class="p-4 text-xs font-bold uppercase text-slate-500">Student Name</th>
                                <th class="p-4 text-xs font-bold uppercase text-slate-500">Password</th>
                                <th class="p-4 text-xs font-bold uppercase text-slate-500">Action</th>
                            </tr>
                        </thead>
                        <tbody id="student-table-body"></tbody>
                    </table>
                </div>
            </div>
        </section>

        <section id="screen-admin" class="screen">
            <div class="bg-red-50 border border-red-200 p-8 rounded-3xl mb-8">
                <h2 class="text-2xl font-black text-red-700 mb-2">Master Admin Dashboard</h2>
                <p class="text-red-600/70 mb-6 font-medium">Critical system operations. Actions here are permanent.</p>
                
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <button onclick="adminDeleteAll()" class="bg-white border-2 border-red-600 text-red-600 p-4 rounded-2xl font-bold hover:bg-red-600 hover:text-white transition-all">
                        🗑️ Delete All School Records
                    </button>
                    <button onclick="location.reload()" class="bg-white border-2 border-slate-800 text-slate-800 p-4 rounded-2xl font-bold hover:bg-slate-800 hover:text-white">
                        🔄 Refresh System
                    </button>
                </div>
            </div>
        </section>

        <section id="screen-marks-entry" class="screen">
            <div class="bg-white p-8 rounded-3xl border shadow-2xl max-w-2xl mx-auto">
                <div class="flex justify-between items-center mb-6">
                    <h2 id="entry-name" class="text-2xl font-bold">Manage Marks</h2>
                    <select id="exam-type" class="border rounded-lg p-2 font-bold bg-slate-50">
                        <option>PT-1</option><option>PT-2</option><option>Annual</option>
                    </select>
                </div>
                <div id="marks-grid" class="grid grid-cols-2 gap-4 mb-6">
                    </div>
                <textarea id="entry-remarks" placeholder="Enter teacher remarks..." class="w-full border rounded-xl p-4 h-32 mb-4"></textarea>
                <div class="flex gap-3">
                    <button onclick="saveStudentData()" class="flex-1 bg-green-600 text-white p-4 rounded-xl font-bold">Update Record</button>
                    <button onclick="showScreen('screen-teacher')" class="bg-slate-100 text-slate-600 px-6 rounded-xl font-bold">Cancel</button>
                </div>
            </div>
        </section>

        <section id="screen-parent" class="screen">
            <div class="bg-indigo-600 p-8 rounded-3xl text-white shadow-xl mb-6">
                <h2 id="p-child-name" class="text-3xl font-bold italic">Student Name</h2>
                <p id="p-child-id" class="opacity-80 font-mono"></p>
            </div>
            <div class="bg-white p-8 rounded-3xl border shadow-sm">
                <div class="flex justify-between items-center mb-6">
                    <h3 class="font-bold text-xl text-slate-400 uppercase tracking-widest text-sm">Exam Report</h3>
                    <select id="p-exam-select" onchange="displayParentData()" class="border p-2 rounded-lg font-bold">
                        <option>PT-1</option><option>PT-2</option><option>Annual</option>
                    </select>
                </div>
                <div id="p-marks-list" class="space-y-3"></div>
            </div>
        </section>

    </main>

    <script>
        const SUBJECTS = ["Maths", "Science", "SST", "English", "Hindi", "IT"];
        
        // --- DATABASE LOGIC ---
        let db = JSON.parse(localStorage.getItem('edu_db_v2')) || {
            students: {}, // { "10A-1": { name: "", pass: "" } }
            records: {}   // { "10A-1_PT-1": { marks: {}, remarks: "" } }
        };

        function sync() { localStorage.setItem('edu_db_v2', JSON.stringify(db)); }

        // --- AUTHENTICATION ---
        let currentUser = null;
        let activeStudentId = null;

        function handleGlobalLogin() {
            const id = document.getElementById('login-id').value.trim();
            const pass = document.getElementById('login-pass').value.trim();

            if (id === 'admin' && pass === 'master789') {
                currentUser = { role: 'admin' };
                showScreen('screen-admin');
            } 
            else if (id === 'teacher' && pass === 'staff123') {
                currentUser = { role: 'teacher' };
                showScreen('screen-teacher');
            } 
            else if (db.students[id] && db.students[id].pass === pass) {
                currentUser = { role: 'parent', id: id };
                activeStudentId = id;
                displayParentData();
                showScreen('screen-parent');
            } 
            else {
                alert("Access Denied: Invalid Credentials");
            }
        }

        // --- TEACHER FUNCTIONS ---
        function loadClassList() {
            const cls = document.getElementById('t-class-select').value;
            const tbody = document.getElementById('student-table-body');
            tbody.innerHTML = '';

            for(let i=1; i<=45; i++) {
                const sId = `${cls}-${i}`;
                const student = db.students[sId] || { name: "", pass: "pass123" };
                tbody.innerHTML += `
                    <tr class="border-b hover:bg-slate-50 transition-colors">
                        <td class="p-4 font-mono font-bold text-slate-400">${i}</td>
                        <td class="p-4">
                            <input type="text" value="${student.name}" onchange="updateStudent('${sId}', 'name', this.value)" 
                            placeholder="Set Name" class="border-b border-transparent focus:border-indigo-400 bg-transparent outline-none w-full">
                        </td>
                        <td class="p-4">
                            <input type="text" value="${student.pass}" onchange="updateStudent('${sId}', 'pass', this.value)" 
                            class="bg-slate-100 text-xs px-2 py-1 rounded font-mono w-24">
                        </td>
                        <td class="p-4">
                            <button onclick="openMarksEntry('${sId}')" class="text-indigo-600 font-bold hover:underline">Edit Marks</button>
                        </td>
                    </tr>
                `;
            }
        }

        function updateStudent(id, field, val) {
            if(!db.students[id]) db.students[id] = { name: "", pass: "pass123" };
            db.students[id][field] = val;
            sync();
        }

        function openMarksEntry(id) {
            activeStudentId = id;
            const student = db.students[id];
            if(!student || !student.name) return alert("Enter student name first!");
            
            document.getElementById('entry-name').innerText = student.name;
            const grid = document.getElementById('marks-grid');
            grid.innerHTML = SUBJECTS.map(sub => `
                <div class="bg-slate-50 p-3 rounded-xl">
                    <label class="block text-[10px] font-black uppercase text-slate-400 mb-1">${sub}</label>
                    <input type="number" id="m-${sub}" class="w-full bg-transparent font-bold text-lg outline-none">
                </div>
            `).join('');
            
            loadExistingRecord();
            showScreen('screen-marks-entry');
        }

        function loadExistingRecord() {
            const exam = document.getElementById('exam-type').value;
            const record = db.records[`${activeStudentId}_${exam}`] || { marks: {}, remarks: "" };
            SUBJECTS.forEach(sub => document.getElementById(`m-${sub}`).value = record.marks[sub] || "");
            document.getElementById('entry-remarks').value = record.remarks || "";
        }

        function saveStudentData() {
            const exam = document.getElementById('exam-type').value;
            const key = `${activeStudentId}_${exam}`;
            const record = { marks: {}, remarks: document.getElementById('entry-remarks').value };
            
            SUBJECTS.forEach(sub => record.marks[sub] = document.getElementById(`m-${sub}`).value);
            db.records[key] = record;
            sync();
            alert("Data Permanently Secured!");
            showScreen('screen-teacher');
        }

        // --- PARENT FUNCTIONS ---
        function displayParentData() {
            const student = db.students[activeStudentId];
            const exam = document.getElementById('p-exam-select').value;
            const record = db.records[`${activeStudentId}_${exam}`];

            document.getElementById('p-child-name').innerText = student.name;
            document.getElementById('p-child-id').innerText = `IDENTIFIER: ${activeStudentId}`;
            
            const list = document.getElementById('p-marks-list');
            if(!record) {
                list.innerHTML = `<div class="p-10 text-center text-slate-400 italic">Marks not yet uploaded for ${exam}</div>`;
            } else {
                list.innerHTML = SUBJECTS.map(sub => `
                    <div class="flex justify-between items-center p-4 bg-slate-50 rounded-xl">
                        <span class="font-bold text-slate-600">${sub}</span>
                        <span class="text-xl font-black text-indigo-600">${record.marks[sub] || 0}</span>
                    </div>
                `).join('');
                list.innerHTML += `<div class="mt-6 p-4 border-l-4 border-indigo-200 bg-indigo-50 text-indigo-700 text-sm">"${record.remarks}"</div>`;
            }
        }

        // --- ADMIN FUNCTIONS ---
        function adminDeleteAll() {
            if(confirm("CRITICAL WARNING: This will permanently erase all names, passwords, and marks. Continue?")) {
                db = { students: {}, records: {} };
                sync();
                alert("Database Reset Complete.");
                location.reload();
            }
        }

        // --- UI HELPERS ---
        function showScreen(id) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            document.getElementById('logout-btn').classList.toggle('hidden', id === 'screen-login');
            document.getElementById('user-info').innerText = currentUser ? `${currentUser.role.toUpperCase()} MODE` : "";
        }

        function logout() { currentUser = null; location.reload(); }
        
        document.getElementById('exam-type').addEventListener('change', loadExistingRecord);
    </script>
</body>
</html>
