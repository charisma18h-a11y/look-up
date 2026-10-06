<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>اختبار المعادلات والمتباينات - المستوى الجامعي</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Cairo', sans-serif; }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex items-center justify-center p-4">

    <div class="w-full max-w-2xl bg-slate-900 rounded-xl shadow-xl border border-slate-800 overflow-hidden">
        
        <!-- Header -->
        <div class="bg-slate-800/60 p-6 border-b border-slate-800">
            <h1 class="text-xl font-bold text-white">اختبار تحصيلي: المعادلات والمتباينات</h1>
            <p class="text-slate-400 text-sm mt-1">مقرر الرياضيات الجامعي</p>
        </div>

        <!-- Registration Screen -->
        <div id="screen-reg" class="p-8">
            <div class="space-y-6">
                <div>
                    <label class="block text-sm font-semibold text-slate-300 mb-2">اسم الطالب / الطالبة:</label>
                    <input type="text" id="participant-name" placeholder="أدخل الاسم الكريم..." class="w-full px-4 py-3 rounded-lg bg-slate-950 border border-slate-800 text-white focus:outline-none focus:border-blue-600 transition">
                    <p id="name-error" class="text-red-400 text-xs mt-1 hidden">الرجاء إدخال الاسم للبدء.</p>
                </div>
                <button onclick="startQuiz()" class="w-full py-3 bg-blue-600 hover:bg-blue-500 text-white font-semibold rounded-lg transition">
                    بدء الاختبار
                </button>
            </div>
        </div>

        <!-- Quiz Screen -->
        <div id="screen-quiz" class="p-8 hidden">
            <div class="flex justify-between items-center mb-6 text-sm bg-slate-950/50 p-4 rounded-lg border border-slate-800">
                <div>
                    <span class="text-slate-400">الطالب:</span>
                    <span id="display-name" class="font-semibold text-white mr-1"></span>
                </div>
                <div class="flex gap-4">
                    <span>السؤال <span id="current-q" class="font-bold text-blue-400">1</span> من <span id="total-q"></span></span>
                    <span class="text-slate-400">|</span>
                    <span>الدرجة: <span id="score-counter" class="font-bold text-emerald-400">0</span></span>
                </div>
            </div>

            <div class="mb-6">
                <h3 id="question-text" class="text-lg font-semibold text-slate-200 leading-relaxed"></h3>
            </div>

            <div id="options-container" class="space-y-3 mb-6"></div>

            <div id="feedback-box" class="hidden mb-6 p-4 rounded-lg border">
                <p id="feedback-text" class="text-sm mb-3"></p>
                <button onclick="nextQuestion()" class="w-full py-2.5 bg-blue-600 hover:bg-blue-500 text-white font-semibold rounded-lg transition">
                    السؤال التالي
                </button>
            </div>
        </div>

        <!-- Results Screen -->
        <div id="screen-result" class="p-8 text-center hidden">
            <div class="space-y-6">
                <h2 class="text-2xl font-bold text-white">نتيجة الاختبار النهائي</h2>
                <div class="bg-slate-950 p-6 rounded-xl border border-slate-800">
                    <p class="text-slate-400 text-sm">الطالب: <span id="res-name" class="text-white font-bold"></span></p>
                    <p class="text-4xl font-black text-blue-400 mt-3"><span id="final-score">0</span> / <span id="max-score">0</span></p>
                    <p id="encouragement-msg" class="text-sm text-slate-300 mt-4"></p>
                </div>
                <button onclick="restartQuiz()" class="w-full py-3 bg-slate-800 hover:bg-slate-700 text-white font-semibold rounded-lg transition">
                    إعادة الاختبار
                </button>
            </div>
        </div>

    </div>

    <script>
        const quizData = [
            {
                question: "أوجد قيمة $x$ التي تحقق المعادلة الخطية: $5x - 15 = 0$",
                answerOptions: [
                    { text: "x = 3", isCorrect: true, rationale: "بنقل 15 للطرف الأيمن والقسمة على المعامل 5 ينتج x = 3." },
                    { text: "x = -3", isCorrect: false, rationale: "خطأ في معالجة إشارة الثابت." },
                    { text: "x = 5", isCorrect: false, rationale: "خطأ في عملية القسمة." },
                    { text: "x = 15", isCorrect: false, rationale: "تم إهمال معامل x." }
                ]
            },
            {
                question: "ما هي مجموعة حل المعادلة التربيعية: $x^2 + 4x - 5 = 0$؟",
                answerOptions: [
                    { text: "{1, -5}", isCorrect: true, rationale: "بتحليل العبارة التربيعية (x-1)(x+5)=0 إذن الجذور هي 1 و -5." },
                    { text: "{-1, 5}", isCorrect: false, rationale: "خطأ في إشارات الجذور." },
                    { text: "{1, 5}", isCorrect: false, rationale: "خطأ في قيم الحد الأوسط." },
                    { text: "{-1, -5}", isCorrect: false, rationale: "خطأ في استخراج الأصفار." }
                ]
            },
            {
                question: "أوجد مجموعة حل المعادلة ذات القيمة المطلقة: $\vert{}x - 3\vert{} = 5$",
                answerOptions: [
                    { text: "{8, -2}", isCorrect: true, rationale: "إما x-3 = 5 ومنها x=8 أو x-3 = -5 ومنها x=-2." },
                    { text: "{2, -8}", isCorrect: false, rationale: "خطأ في تعويض قيم الثوابت." },
                    { text: "{2, 8}", isCorrect: false, rationale: "تم إهمال الحالة السالبة للقيمة المطلقة." },
                    { text: "{-2, -8}", isCorrect: false, rationale: "خطأ كامل في حسابات الإشارات." }
                ]
            },
            {
                question: "ما هي فترة حل المتباينة: $3x - 4 \\le 5$؟",
                answerOptions: [
                    { text: "(-∞, 3]", isCorrect: true, rationale: "باستنتاج أن 3x ≤ 9 إذن x ≤ 3، وتتمثل في الفترة المغلقة (-∞, 3]." },
                    { text: "[3, ∞)", isCorrect: false, rationale: "عكس اتجاه التباين." },
                    { text: "(-∞, 3)", isCorrect: false, rationale: "تم تجاهل علامة المساواة (≤)." },
                    { text: "[-3, 3]", isCorrect: false, rationale: "افتراض خاطئ لفترة محدودة الطرفين." }
                ]
            }
        ];

        let currentQuestion = 0;
        let score = 0;
        let participantName = "";

        document.getElementById('total-q').innerText = quizData.length;
        document.getElementById('max-score').innerText = quizData.length;

        function startQuiz() {
            const nameInput = document.getElementById('participant-name');
            const errorText = document.getElementById('name-error');
            
            if (!nameInput.value.trim()) {
                errorText.classList.remove('hidden');
                return;
            }
            
            participantName = nameInput.value.trim();
            document.getElementById('display-name').innerText = participantName;
            
            document.getElementById('screen-reg').classList.add('hidden');
            document.getElementById('screen-quiz').classList.remove('hidden');
            
            loadQuestion();
        }

        function loadQuestion() {
            const q = quizData[currentQuestion];
            document.getElementById('current-q').innerText = currentQuestion + 1;
            document.getElementById('question-text').innerText = q.question;
            
            const container = document.getElementById('options-container');
            container.innerHTML = '';
            
            document.getElementById('feedback-box').classList.add('hidden');

            q.answerOptions.forEach((opt) => {
                const btn = document.createElement('button');
                btn.className = "w-full text-right px-4 py-3 rounded-lg bg-slate-950 border border-slate-800 hover:border-slate-600 text-slate-200 text-sm transition flex justify-between items-center";
                btn.innerHTML = `<span>${opt.text}</span>`;
                btn.onclick = () => selectOption(opt, btn, q.answerOptions);
                container.appendChild(btn);
            });
        }

        function selectOption(selectedOpt, selectedBtn, allOptions) {
            const buttons = document.getElementById('options-container').getElementsByTagName('button');
            for (let b of buttons) b.disabled = true;

            const feedbackBox = document.getElementById('feedback-box');
            const feedbackText = document.getElementById('feedback-text');

            if (selectedOpt.isCorrect) {
                score++;
                document.getElementById('score-counter').innerText = score;
                selectedBtn.className = "w-full text-right px-4 py-3 rounded-lg bg-emerald-950/40 border border-emerald-600 text-emerald-200 text-sm flex justify-between items-center";
                feedbackBox.className = "mb-6 p-4 rounded-lg border bg-emerald-950/20 border-emerald-800 text-emerald-200 text-sm";
                feedbackText.innerHTML = "<strong>إجابة صحيحة:</strong> " + selectedOpt.rationale;
            } else {
                selectedBtn.className = "w-full text-right px-4 py-3 rounded-lg bg-red-950/40 border border-red-600 text-red-200 text-sm flex justify-between items-center";
                
                for (let i = 0; i < buttons.length; i++) {
                    if (allOptions[i].isCorrect) {
                        buttons[i].className = "w-full text-right px-4 py-3 rounded-lg bg-emerald-950/40 border border-emerald-600 text-emerald-200 text-sm flex justify-between items-center";
                    }
                }

                feedbackBox.className = "mb-6 p-4 rounded-lg border bg-red-950/20 border-red-800 text-red-200 text-sm";
                feedbackText.innerHTML = "<strong>إجابة غير صحيحة:</strong> " + selectedOpt.rationale;
            }

            feedbackBox.classList.remove('hidden');
        }

        function nextQuestion() {
            currentQuestion++;
            if (currentQuestion < quizData.length) {
                loadQuestion();
            } else {
                showResults();
            }
        }

        function showResults() {
            document.getElementById('screen-quiz').classList.add('hidden');
            document.getElementById('screen-result').classList.remove('hidden');
            
            document.getElementById('res-name').innerText = participantName;
            document.getElementById('final-score').innerText = score;

            const msgEl = document.getElementById('encouragement-msg');
            const percentage = (score / quizData.length) * 100;

            if (percentage === 100) {
                msgEl.innerText = "أداء ممتاز، استيعاب كامل للمفاهيم الرياضية.";
            } else if (percentage >= 50) {
                msgEl.innerText = "نتيجة جيدة، يفضل مراجعة بعض تفاصيل خطوات الحل.";
            } else {
                msgEl.innerText = "تحتاج إلى مراجعة إضافية لمقرر المعادلات والمتباينات.";
            }
        }

        function restartQuiz() {
            currentQuestion = 0;
            score = 0;
            document.getElementById('score-counter').innerText = score;
            document.getElementById('participant-name').value = "";
            document.getElementById('name-error').classList.add('hidden');
            
            document.getElementById('screen-result').classList.add('hidden');
            document.getElementById('screen-reg').classList.remove('hidden');
        }
    </script>
</body>
</html>
