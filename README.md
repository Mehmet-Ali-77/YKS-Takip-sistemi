<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Soru Bankam (TYT - AYT)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        .no-scrollbar::-webkit-scrollbar { display: none; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen flex flex-col pb-10">

    <header class="bg-indigo-600 text-white py-4 px-6 shadow-md flex justify-between items-center sticky top-0 z-50">
        <h1 class="text-xl font-bold tracking-wide cursor-pointer flex items-center" onclick="showMainCategories()">
            📚 Soru Kutusu
        </h1>
        
        <div class="flex items-center space-x-3">
            <div class="flex items-center space-x-3 bg-indigo-700 px-3 py-1.5 rounded-xl text-xs md:text-sm shadow-inner">
                <span class="font-semibold text-emerald-300">⏱️ <span id="header-total-hours">0</span>s</span>
                <span class="opacity-30">|</span>
                <span class="font-semibold text-yellow-300">✍️ <span id="header-total-questions">0</span> Soru</span>
            </div>
            
            <button onclick="showNetForm()" class="bg-emerald-500 text-white font-semibold px-3 py-2 rounded-lg shadow text-sm hover:bg-emerald-600 transition-all">
                📊 Net Ekle
            </button>
            <button onclick="showAddQuestionForm()" class="bg-white text-indigo-600 font-semibold px-3 py-2 rounded-lg shadow text-sm hover:bg-indigo-50 transition-all">
                ➕ Soru Ekle
            </button>
        </div>
    </header>

    <main class="flex-grow p-6 max-w-4xl mx-auto w-full">

        <section id="main-categories" class="space-y-8">
            <h2 class="text-2xl font-bold text-slate-700 text-center mb-4">Sınav Türünü Seçin</h2>
            <div class="grid grid-cols-2 gap-6">
                <button onclick="selectMainCategory('TYT')" class="bg-gradient-to-br from-blue-500 to-indigo-600 text-white h-40 rounded-2xl shadow-lg flex flex-col justify-center items-center text-3xl font-extrabold hover:scale-105 transition-transform">
                    TYT
                    <span class="text-sm font-light mt-2 opacity-80">Temel Yeterlilik Testi</span>
                </button>
                <button onclick="selectMainCategory('AYT')" class="bg-gradient-to-br from-purple-500 to-pink-600 text-white h-40 rounded-2xl shadow-lg flex flex-col justify-center items-center text-3xl font-extrabold hover:scale-105 transition-transform">
                    AYT
                    <span class="text-sm font-light mt-2 opacity-80">Alan Yeterlilik Testi</span>
                </button>
            </div>

            <hr class="border-slate-200">
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-white p-4 rounded-2xl shadow-md border border-slate-100">
                    <div class="flex justify-between items-center mb-2">
                        <h3 class="font-bold text-slate-700 text-lg">📈 TYT Net Gelişimi</h3>
                        <button onclick="clearNets('TYT')" class="text-xs text-red-500 hover:text-red-700">Sıfırla</button>
                    </div>
                    <div class="h-64">
                        <canvas id="tytChart"></canvas>
                    </div>
                </div>
                <div class="bg-white p-4 rounded-2xl shadow-md border border-slate-100">
                    <div class="flex justify-between items-center mb-2">
                        <h3 class="font-bold text-slate-700 text-lg">📈 AYT Net Gelişimi</h3>
                        <button onclick="clearNets('AYT')" class="text-xs text-red-500 hover:text-red-700">Sıfırla</button>
                    </div>
                    <div class="h-64">
                        <canvas id="aytChart"></canvas>
                    </div>
                </div>
            </div>

            <hr class="border-slate-200">
            <div class="bg-white p-6 rounded-2xl shadow-md border border-slate-100 space-y-4">
                <div class="flex justify-between items-center">
                    <h3 class="font-bold text-slate-800 text-lg">📝 Günlük Çalışma Günlüğüm</h3>
                    <span class="text-xs text-slate-400">Yazdıklarınız otomatik taranır ve kaydedilir.</span>
                </div>
                
                <textarea id="study-diary" oninput="analyzeAndSaveDiary()" class="w-full h-36 p-4 border border-slate-200 rounded-xl bg-slate-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-slate-700" placeholder="Örn: Bugün 2.5 saat matematik çalıştım. Kimya dersinden 20 soru çözdüm. Fizikten de 15 soru bitti..."></textarea>
                
                <div>
                    <h4 class="font-bold text-slate-600 text-sm mb-2">📊 Günlük Çalışma Çizelgesi</h4>
                    <div class="overflow-x-auto rounded-xl border border-slate-100">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-slate-100 text-slate-600 text-xs uppercase font-bold">
                                    <th class="p-3">Ders / Konu</th>
                                    <th class="p-3 text-right">Çözülen Soru</th>
                                </tr>
                            </thead>
                            <tbody id="diary-analysis-table" class="text-sm text-slate-700 divide-y divide-slate-100">
                                <tr>
                                    <td colspan="2" class="p-3 text-center text-slate-400">Yazmaya başladığınızda analiz burada belirecektir.</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

        <section id="sub-categories" class="hidden space-y-6">
            <div class="flex items-center space-x-4 mb-4">
                <button onclick="showMainCategories()" class="text-indigo-600 font-semibold">&larr; Geri</button>
                <h2 id="current-main-title" class="text-2xl font-bold text-slate-700">TYT Dersleri</h2>
            </div>
            <div id="sub-category-list" class="grid grid-cols-2 md:grid-cols-4 gap-4"></div>
        </section>

        <section id="questions-list-section" class="hidden space-y-6">
            <div class="flex items-center justify-between mb-4">
                <div class="flex items-center space-x-4">
                    <button onclick="backToSubCategories()" class="text-indigo-600 font-semibold">&larr; Dersler</button>
                    <h2 id="current-subject-title" class="text-2xl font-bold text-slate-700">Matematik Soruları</h2>
                </div>
            </div>
            <div id="questions-grid" class="grid grid-cols-1 md:grid-cols-2 gap-6"></div>
        </section>

        <section id="question-detail-section" class="hidden space-y-6">
            <div class="flex items-center space-x-4 mb-4">
                <button onclick="backToQuestionsList()" class="text-indigo-600 font-semibold">&larr; Listeye Dön</button>
                <h2 id="detail-question-title" class="text-2xl font-bold text-slate-700">Soru Detayı</h2>
            </div>
            <div class="bg-white rounded-2xl p-6 shadow-md space-y-6">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="border rounded-xl p-4 bg-slate-100">
                        <h3 class="text-lg font-bold text-red-600 mb-2">❓ Soru</h3>
                        <img id="detail-question-img" class="w-full rounded-lg max-h-[500px] object-contain" src="" alt="Soru">
                    </div>
                    <div class="border rounded-xl p-4 bg-slate-100">
                        <h3 class="text-lg font-bold text-green-600 mb-2">✅ Çözüm</h3>
                        <img id="detail-solution-img" class="w-full rounded-lg max-h-[500px] object-contain" src="" alt="Çözüm">
                    </div>
                </div>
            </div>
        </section>

        <section id="add-question-section" class="hidden space-y-6">
            <div class="flex items-center space-x-4 mb-4">
                <button onclick="showMainCategories()" class="text-indigo-600 font-semibold">&larr; İptal Et</button>
                <h2 class="text-2xl font-bold text-slate-700">Yeni Soru Ekle</h2>
            </div>
            <form id="question-form" onsubmit="saveQuestion(event)" class="bg-white rounded-2xl p-6 shadow-md space-y-4">
                <div>
                    <label class="block text-sm font-semibold text-slate-600 mb-1">Sınav Türü</label>
                    <select id="form-exam" class="w-full border p-3 rounded-lg bg-slate-50" onchange="updateFormSubjects()" required>
                        <option value="TYT">TYT</option>
                        <option value="AYT">AYT</option>
                    </select>
                </div>
                <div>
                    <label class="block text-sm font-semibold text-slate-600 mb-1">Ders</label>
                    <select id="form-subject" class="w-full border p-3 rounded-lg bg-slate-50" required></select>
                </div>
                <div>
                    <label class="block text-sm font-semibold text-slate-600 mb-1">Soru Başlığı / Konusu</label>
                    <input type="text" id="form-title" class="w-full border p-3 rounded-lg bg-slate-50" placeholder="Örn: Limit Süreklilik ÖSYM 2024" required>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-semibold text-red-600 mb-1">Soru Fotoğrafı</label>
                        <input type="file" id="form-question-file" accept="image/*" capture="environment" class="w-full border p-2 rounded-lg bg-slate-50" required>
                    </div>
                    <div>
                        <label class="block text-sm font-semibold text-green-600 mb-1">Çözüm Fotoğrafı</label>
                        <input type="file" id="form-solution-file" accept="image/*" capture="environment" class="w-full border p-2 rounded-lg bg-slate-50" required>
                    </div>
                </div>
                <button type="submit" class="w-full bg-indigo-600 text-white font-bold py-3 rounded-xl shadow hover:bg-indigo-700 transition-all">
                    Kaydet ve Arşive Ekle
                </button>
            </form>
        </section>

        <section id="add-net-section" class="hidden space-y-6">
            <div class="flex items-center space-x-4 mb-4">
                <button onclick="showMainCategories()" class="text-indigo-600 font-semibold">&larr; İptal Et</button>
                <h2 class="text-2xl font-bold text-slate-700">Yeni Net Kaydı</h2>
            </div>
            <form id="net-form" onsubmit="saveNet(event)" class="bg-white rounded-2xl p-6 shadow-md space-y-4">
                <div>
                    <label class="block text-sm font-semibold text-slate-600 mb-1">Sınav Türü</label>
                    <select id="net-exam" class="w-full border p-3 rounded-lg bg-slate-50" required>
                        <option value="TYT">TYT</option>
                        <option value="AYT">AYT</option>
                    </select>
                </div>
                <div>
                    <label class="block text-sm font-semibold text-slate-600 mb-1">Deneme Adı / Tarih</label>
                    <input type="text" id="net-label" class="w-full border p-3 rounded-lg bg-slate-50" placeholder="Örn: Özdebir 1" required>
                </div>
                <div>
                    <label class="block text-sm font-semibold text-slate-600 mb-1">Toplam Net</label>
                    <input type="number" step="0.25" min="0" max="120" id="net-value" class="w-full border p-3 rounded-lg bg-slate-50" placeholder="Örn: 74.5" required>
                </div>
                <button type="submit" class="w-full bg-emerald-500 text-white font-bold py-3 rounded-xl shadow hover:bg-emerald-600 transition-all">
                    Neti Kaydet
                </button>
            </form>
        </section>

    </main>

    <script>
        const categories = {
            TYT: ["Matematik", "Türkçe", "Sosyal", "Fen"],
            AYT: ["Matematik", "Fizik", "Biyoloji", "Kimya"]
        };

        let currentExam = "";
        let currentSubject = "";
        let db;
        let tytChart, aytChart;

        // Çakışmaları önlemek için veritabanı adını 'SoruKutusuDB_v3' olarak güncelledik.
        const request = indexedDB.open("SoruKutusuDB_v3", 1);
        
        request.onupgradeneeded = function(e) {
            db = e.target.result;
            if (!db.objectStoreNames.contains("questions")) {
                db.createObjectStore("questions", { keyPath: "id", autoIncrement: true });
            }
            if (!db.objectStoreNames.contains("nets")) {
                db.createObjectStore("nets", { keyPath: "id", autoIncrement: true });
            }
            if (!db.objectStoreNames.contains("diary")) {
                db.createObjectStore("diary", { keyPath: "id" });
            }
        };

        request.onsuccess = function(e) {
            db = e.target.result;
            initCharts();
            loadNetData();
            loadDiary();
        };

        request.onerror = function(e) {
            console.error("Veritabanı yüklenemedi:", e.target.error);
        };

        function hideAllSections() {
            document.getElementById('main-categories').classList.add('hidden');
            document.getElementById('sub-categories').classList.add('hidden');
            document.getElementById('questions-list-section').classList.add('hidden');
            document.getElementById('question-detail-section').classList.add('hidden');
            document.getElementById('add-question-section').classList.add('hidden');
            document.getElementById('add-net-section').classList.add('hidden');
        }

        function showMainCategories() {
            hideAllSections();
            document.getElementById('main-categories').classList.remove('hidden');
            loadNetData();
            loadDiary();
        }

        function selectMainCategory(exam) {
            currentExam = exam;
            hideAllSections();
            const subSec = document.getElementById('sub-categories');
            subSec.classList.remove('hidden');
            document.getElementById('current-main-title').innerText = `${exam} Dersleri`;

            const listDiv = document.getElementById('sub-category-list');
            listDiv.innerHTML = "";

            categories[exam].forEach(subject => {
                const btn = document.createElement('button');
                btn.className = "bg-white p-6 rounded-xl shadow border border-slate-200 hover:border-indigo-500 font-bold text-lg text-slate-700 text-center hover:scale-105 transition-all";
                btn.innerText = subject;
                btn.onclick = () => selectSubject(subject);
                listDiv.appendChild(btn);
            });
        }

        function selectSubject(subject) {
            currentSubject = subject;
            hideAllSections();
            document.getElementById('questions-list-section').classList.remove('hidden');
            document.getElementById('current-subject-title').innerText = `${currentExam} - ${subject}`;
            loadQuestions();
        }

        function backToSubCategories() {
            selectMainCategory(currentExam);
        }

        function backToQuestionsList() {
            hideAllSections();
            document.getElementById('questions-list-section').classList.remove('hidden');
        }

        function showAddQuestionForm() {
            hideAllSections();
            document.getElementById('add-question-section').classList.remove('hidden');
            updateFormSubjects();
        }

        function showNetForm() {
            hideAllSections();
            document.getElementById('add-net-section').classList.remove('hidden');
        }

        function updateFormSubjects() {
            const exam = document.getElementById('form-exam').value;
            const subjectSelect = document.getElementById('form-subject');
            subjectSelect.innerHTML = "";
            categories[exam].forEach(sub => {
                const opt = document.createElement('option');
                opt.value = sub;
                opt.innerText = sub;
                subjectSelect.appendChild(opt);
            });
        }

        function fileToBuffer(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result);
                reader.onerror = reject;
                reader.readAsDataURL(file);
            });
        }

        async function saveQuestion(event) {
            event.preventDefault();
            if (!db) {
                alert("Hata: Veritabanı hazır değil! Dosyalardan (file://) doğrudan açtıysanız tarayıcınız kaydetmeyi engeller. Lütfen Koder üzerinden localhost ile açın.");
                return;
            }

            const exam = document.getElementById('form-exam').value;
            const subject = document.getElementById('form-subject').value;
            const title = document.getElementById('form-title').value;
            const qFile = document.getElementById('form-question-file').files[0];
            const sFile = document.getElementById('form-solution-file').files[0];

            if (!qFile || !sFile) {
                alert("Lütfen hem soru hem de çözüm fotoğrafını seçin.");
                return;
            }

            const questionImg = await fileToBuffer(qFile);
            const solutionImg = await fileToBuffer(sFile);

            const newQuestion = { exam, subject, title, questionImg, solutionImg };

            const transaction = db.transaction(["questions"], "readwrite");
            const store = transaction.objectStore("questions");
            store.add(newQuestion);

            transaction.oncomplete = function() {
                alert("Soru başarıyla kaydedildi!");
                document.getElementById('question-form').reset();
                selectMainCategory(exam);
            };
        }

        function loadQuestions() {
            const grid = document.getElementById('questions-grid');
            grid.innerHTML = "";

            if (!db) return;

            const transaction = db.transaction(["questions"], "readonly");
            const store = transaction.objectStore("questions");
            const request = store.openCursor();

            let hasData = false;

            request.onsuccess = function(e) {
                const cursor = e.target.result;
                if (cursor) {
                    const question = cursor.value;
                    if (question.exam === currentExam && question.subject === currentSubject) {
                        hasData = true;
                        const card = document.createElement('div');
                        card.className = "bg-white p-4 rounded-xl shadow-md border hover:border-indigo-500 cursor-pointer transition-all flex flex-col justify-between";
                        card.innerHTML = `
                            <div onclick="viewQuestionDetails(${question.id})">
                                <h3 class="font-bold text-lg text-slate-800 mb-2">${question.title}</h3>
                                <div class="grid grid-cols-2 gap-2">
                                    <img class="h-24 w-full object-cover rounded" src="${question.questionImg}">
                                    <img class="h-24 w-full object-cover rounded" src="${question.solutionImg}">
                                </div>
                            </div>
                            <div class="flex justify-end mt-4">
                                <button onclick="deleteQuestion(${question.id})" class="text-sm text-red-500 hover:text-red-700 font-semibold">Sil</button>
                            </div>
                        `;
                        grid.appendChild(card);
                    }
                    cursor.continue();
                } else if (!hasData) {
                    grid.innerHTML = `<p class="text-slate-500 col-span-full text-center py-10">Bu derse henüz eklenmiş soru yok.</p>`;
                }
            };
        }

        function viewQuestionDetails(id) {
            if (!db) return;
            const transaction = db.transaction(["questions"], "readonly");
            const store = transaction.objectStore("questions");
            store.get(id).onsuccess = function(e) {
                const question = e.target.result;
                hideAllSections();
                document.getElementById('question-detail-section').classList.remove('hidden');
                document.getElementById('detail-question-title').innerText = question.title;
                document.getElementById('detail-question-img').src = question.questionImg;
                document.getElementById('detail-solution-img').src = question.solutionImg;
            };
        }

        function deleteQuestion(id) {
            if(confirm("Bu soruyu silmek istediğinize emin misiniz?")) {
                const transaction = db.transaction(["questions"], "readwrite");
                const store = transaction.objectStore("questions");
                store.delete(id);
                transaction.oncomplete = function() {
                    loadQuestions();
                };
            }
        }

        // --- GRAFİK VE NET YÖNETİMİ ---

        function initCharts() {
            const tytCtx = document.getElementById('tytChart').getContext('2d');
            const aytCtx = document.getElementById('aytChart').getContext('2d');

            const chartOptions = {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true,
                        max: 120,
                        ticks: { stepSize: 10 }
                    }
                },
                plugins: {
                    legend: { display: false }
                }
            };

            tytChart = new Chart(tytCtx, {
                type: 'line',
                data: { labels: [], datasets: [{ data: [], borderColor: '#3b82f6', backgroundColor: 'rgba(59, 130, 246, 0.1)', tension: 0.3, fill: true, borderWidth: 3 }] },
                options: chartOptions
            });

            aytChart = new Chart(aytCtx, {
                type: 'line',
                data: { labels: [], datasets: [{ data: [], borderColor: '#a855f7', backgroundColor: 'rgba(168, 85, 247, 0.1)', tension: 0.3, fill: true, borderWidth: 3 }] },
                options: { ...chartOptions, scales: { y: { beginAtZero: true, max: 80, ticks: { stepSize: 10 } } } }
            });
        }

        function saveNet(event) {
            event.preventDefault();
            if (!db) {
                alert("Hata: Veritabanı bağlantısı yok! Sayfayı Dosyalar'dan (file://) açtıysanız tarayıcınız güvenlik nedeniyle kaydetmeyi engeller. Lütfen Koder üzerinden localhost bağlantısını açın.");
                return;
            }

            const exam = document.getElementById('net-exam').value;
            const label = document.getElementById('net-label').value;
            const value = parseFloat(document.getElementById('net-value').value);

            const newNet = { exam, label, value, timestamp: Date.now() };

            const transaction = db.transaction(["nets"], "readwrite");
            const store = transaction.objectStore("nets");
            store.add(newNet);

            transaction.oncomplete = function() {
                alert("Net başarıyla kaydedildi!");
                document.getElementById('net-form').reset();
                showMainCategories();
            };
        }

        function loadNetData() {
            if (!db) return;
            const transaction = db.transaction(["nets"], "readonly");
            const store = transaction.objectStore("nets");
            const request = store.getAll();

            request.onsuccess = function(e) {
                const allNets = e.target.result;
                allNets.sort((a, b) => a.timestamp - b.timestamp);

                const tytData = allNets.filter(n => n.exam === "TYT");
                const aytData = allNets.filter(n => n.exam === "AYT");

                tytChart.data.labels = tytData.map(n => n.label);
                tytChart.data.datasets[0].data = tytData.map(n => n.value);
                tytChart.update();

                aytChart.data.labels = aytData.map(n => n.label);
                aytChart.data.datasets[0].data = aytData.map(n => n.value);
                aytChart.update();
            };
        }

        function clearNets(exam) {
            if (confirm(`${exam} net geçmişini sıfırlamak istediğinize emin misiniz?`)) {
                const transaction = db.transaction(["nets"], "readwrite");
                const store = transaction.objectStore("nets");
                const request = store.openCursor();

                request.onsuccess = function(e) {
                    const cursor = e.target.result;
                    if (cursor) {
                        if (cursor.value.exam === exam) {
                            store.delete(cursor.primaryKey);
                        }
                        cursor.continue();
                    }
                };

                transaction.oncomplete = function() {
                    loadNetData();
                };
            }
        }

        // --- GÜNLÜK AKILLI ANALİZ SİSTEMİ ---

        function analyzeAndSaveDiary() {
            const text = document.getElementById('study-diary').value;
            
            const questionRegex = /([a-zA-ZçıışğöüÖÜİĞŞÇ]+)\s*(\d+)\s*soru/gi;
            const hourRegex = /(\d+(?:\.\d+)?)\s*saat/gi;

            let matches;
            let questionsData = {};
            let totalQuestions = 0;
            let totalHours = 0;

            while ((matches = questionRegex.exec(text)) !== null) {
                const subject = matches[1].trim().toLowerCase();
                const count = parseInt(matches[2], 10);
                const capitalizedSubject = subject.charAt(0).toUpperCase() + subject.slice(1);

                if (questionsData[capitalizedSubject]) {
                    questionsData[capitalizedSubject] += count;
                } else {
                    questionsData[capitalizedSubject] = count;
                }
                totalQuestions += count;
            }

            while ((matches = hourRegex.exec(text)) !== null) {
                const hours = parseFloat(matches[1]);
                totalHours += hours;
            }

            document.getElementById('header-total-hours').innerText = totalHours;
            document.getElementById('header-total-questions').innerText = totalQuestions;

            const tbody = document.getElementById('diary-analysis-table');
            tbody.innerHTML = "";

            if (Object.keys(questionsData).length === 0) {
                tbody.innerHTML = `<tr><td colspan="2" class="p-3 text-center text-slate-400">Yazmaya başladığınızda analiz burada belirecektir.</td></tr>`;
            } else {
                for (const [subj, count] of Object.entries(questionsData)) {
                    const row = document.createElement('tr');
                    row.className = "hover:bg-slate-50 transition-colors";
                    row.innerHTML = `
                        <td class="p-3 font-semibold text-slate-700">📖 ${subj}</td>
                        <td class="p-3 text-right text-indigo-600 font-bold">${count} Soru</td>
                    `;
                    tbody.appendChild(row);
                }
                const totalRow = document.createElement('tr');
                totalRow.className = "bg-indigo-50 font-bold text-indigo-900";
                totalRow.innerHTML = `
                    <td class="p-3">✨ Genel Toplam</td>
                    <td class="p-3 text-right">${totalQuestions} Soru</td>
                `;
                tbody.appendChild(totalRow);
            }

            if (db) {
                const transaction = db.transaction(["diary"], "readwrite");
                const store = transaction.objectStore("diary");
                store.put({ id: "daily", text: text });
            }
        }

        function loadDiary() {
            if (!db) return;
            const transaction = db.transaction(["diary"], "readonly");
            const store = transaction.objectStore("diary");
            const request = store.get("daily");

            request.onsuccess = function(e) {
                if (e.target.result) {
                    document.getElementById('study-diary').value = e.target.result.text;
                    analyzeAndSaveDiary();
                }
            };
        }
    </script>
</body>
</html>
