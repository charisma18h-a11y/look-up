<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>اختبار المعادلات والمتباينات التفاعلي</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Cairo', sans-serif;
        }
    </style>
</head>
<body class="bg-gradient-to-br from-indigo-900 via-purple-900 to-slate-900 min-h-screen text-slate-100 flex items-center justify-center p-4">

    <div class="w-full max-w-2xl bg-slate-800/90 backdrop-blur-md rounded-2xl shadow-2xl border border-slate-700 overflow-hidden">
        
        <!-- Header -->
        <div class="bg-indigo-600/30 p-6 border-b border-indigo-500/30 text-center">
            <h1 class="text-2xl md:text-3xl font-black text-white tracking-wide">اختبار المعادلات والمتباينات</h1>
            <p class="text-indigo-200 text-sm mt-1">بناءً على مقرر الرياضيات - المعادلات والمتباينات (الدرجة الأولى والثانية)</p>
        </div>

        <!-- Registration Screen -->
        <div id="screen-reg" class="p-8">
            <div class="max-w-md mx-auto space-y-6 text-center">
                <div class="w-20 h-20 bg-indigo-500/20 text-indigo-400 rounded-full flex items-center justify-center mx-auto text-3xl font-bold border border-indigo-500/40 shadow-inner">
                    📝
                </div>
                <div>
                    <h2 class="text-xl font-bold text-white">أهلاً بك أيها البطل!</h2>
                    <p class="text-slate-400 text-sm mt-2">الرجاء إدخال اسمك الكريم للبدء في الاختبار التفاعلي وتسجيل نقاطك.</p>
                </div>
                <div class="space-y-4 text-right">
                    <label class="block text-sm font-semibold text-indigo-300">اسم المشارك:</label>
                    <input type="text" id="participant-name" placeholder="اكتب اسمك هنا..." class="w-full px-4 py-3 rounded-xl bg-slate-900 border border-slate-700 text-white focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-500/30 transition">
                    <p id="name-error" class="text-rose-400 text-xs hidden">الرجاء إدخال الاسم للبدء.</p>
                </div>
                <button onclick="startQuiz()" class="w-full py-3.5 bg-indigo-600 hover:bg-indigo-500 text-white font-bold rounded-xl shadow-lg shadow-indigo-600/30 transition transform hover:-translate-y-0.5 active:translate-y-0">
                    ابدأ الاختبار الآن 🚀
                </button>
            </div>
        </div>

        <!-- Quiz Screen -->
        <div id="screen-quiz" class="p-8 hidden">
            <div class="flex justify-between items-center mb-6 bg-slate-900/60 p-4 rounded-xl border border-slate-700/50">
                <div>
                    <span class="text-xs text-indigo-300 font-semibold uppercase tracking-wider">المشارك:</span>
                    <span id="display-name" class="font-bold text-white text-lg mr-2"></span>
                </div>
                <div class="flex items-center gap-4">
                    <div class="text-sm font-semibold text-slate-300">
                        السؤال <span id="current-q" class="text-indigo-400 font-bold">1</span> من <span id="total-q"></span>
                    </div>
                    <div class="bg-indigo-500/20 text-indigo-300 px-3 py-1 rounded-lg text-sm font-bold border border-indigo-500/30">
                        النقاط: <span id="score-counter">0</span>
                    </div>
                </div>
            </div>

            <!-- Question Box -->
            <div class="mb-6">
                <h3 id="question-text" class="text-lg md:text-xl font-bold text-white leading-relaxed"></h3>
            </div>

            <!-- Options Container -->
            <div id="options-container" class="space-y-3 mb-6">
                <!-- Dynamic Options -->
            </div>

            <!-- Rationale & Next Button Box -->
            <div id="feedback-box" class="hidden mb-6 p-4 rounded-xl border transition-all">
                <p id="feedback-text" class="text-sm font-medium mb-3"></p>
                <button onclick="nextQuestion()" class="w-full py-2.5 bg-indigo-600 hover:bg-indigo-500 text-white font-bold rounded-xl shadow transition">
                    السؤال التالي ⬅️
                </button>
            </div>
        </div>

        <!-- Results Screen -->
        <div id="screen-result" class="p-8 text-center hidden">
            <div class="max-w-md mx-auto space-y-6">
                <div class="w-24 h-24 bg-emerald-500/20 text-emerald-400 rounded-full flex items-center justify-center mx-auto text-4xl border border-emerald-500/40 shadow-inner">
                    🏆
                </div>
                <div>
                    <h2 class="text-2xl font-bold text-white">أحسنت يا <span id="res-name" class="text-indigo-400"></span>!</h2>
                    <p class="text-slate-400 text-sm mt-1">لقد أكملت اختبار المعادلات والمتباينات بنجاح.</p>
                </div>
                <div class="bg-slate-900/80 p-6 rounded-2xl border border-slate-700">
                    <p class="text-sm text-slate-400">النتيجة النهائية:</p>
                    <p class="text-4xl font-black text-indigo-400 mt-2"><span id="final-score">0</span> / <span id="max-score">0</span></p>
                    <p id="encouragement-msg" class="text-sm text-slate-300 mt-4"></p>
                </div>
                <button onclick="restartQuiz()" class="w-full py-3.5 bg-indigo-600 hover:bg-indigo-500 text-white font-bold rounded-xl shadow-lg shadow-indigo-600/30 transition">
                    إعادة الاختبار 🔄
                </button>
            </div>
        </div>

    </div>

    <script>
        const quizData = [
            {
                question: "أوجد قيمة $x$ التي تحقق المعادلة الخطية: $5x - 15 = 0$",
                answerOptions: [
                    { text: "$x = 3$", isCorrect: true, rationale: "بنقل 15 إلى الطرف الأيمن ثم القسمة على معامل $x$ وهو 5، نجد أن $x = 15 / 5 = 3$." },
                    { text: "$x = -3$", isCorrect: false, rationale: "هذا خطأ في إشارة الثابت عند نقله، فالنقل يغير السالب إلى موجب." },
                    { text: "$x = 5$", isCorrect: false, rationale: "هذا ينتج من قسمة غير صحيحة للقيم الثابتة والمعامل." },
                    { text: "$x = 15$", isCorrect: false, rationale: "نسيان قسمة الناتج على معامل $x$." }
                ],
                hint: "انقل الحد الثابت للطرف الأيمن ثم اقسم على المعامل."
            },
            {
                question: "ما هي مجموعة حل المعادلة التربيعية الصفرية: $x^2 + 4x - 5 = 0$ باستخدام القانون العام؟",
                answerOptions: [
                    { text: "$\\{1, -5\\}$", isCorrect: true, rationale: "بمقارنة المعادلة بالصورة العامة وحساب المميز وقيم القانون العام، نجد الجذور هي 1 و -5." },
                    { text: "$\\{-1, 5\\}$", isCorrect: false, rationale: "خطأ في انعكاس إشارات الجذور الناتجة." },
                    { text: "$\\{1, 5\\}$", isCorrect: false, rationale: "خطأ في تحليل أو حساب الحد الأوسط." },
                    { text: "$\\{-1, -5\\}$", isCorrect: false, rationale: "خطأ في إشارات نواتج الحسابات." }
                ],
                hint: "استخدم قيم $a=1, b=4, c=-5$ في القانون العام."
            },
