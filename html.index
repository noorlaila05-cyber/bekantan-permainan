<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English Education Game: Bekantan Edition</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        'primary-dark': '#3730a3',
                        'primary-light': '#38bdf8',
                        'secondary': '#a855f7',
                        'correct': '#10b981',
                        'wrong': '#ef4444',
                        'timer-warn': '#f59e0b',
                        'bg-start': '#e0f2fe', /* Light Blue for Sky */
                        'bg-end': '#fef3c7', /* Light Yellow for Sand/Sun */
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(to bottom right, #e0f2fe, #fef3c7);
        }
        
        #game-container {
            background-color: #ffffff;
            border-radius: 1.5rem;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
            overflow: hidden; /* Penting untuk animasi */
        }

        .answer-button {
            transition: all 0.2s;
        }
        .answer-button:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .matching-button:not(.selected):not(:disabled):hover {
            background-color: #f0f9ff;
        }
        .matching-button.selected {
            background-color: #c7d2fe;
            border-color: #3730a3;
            transform: scale(1.05);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        /* --- BEKANTAN SCENE ANIMATION --- */
        #bekantan-scene {
            position: relative;
            width: 100%;
            height: 150px;
            overflow: hidden;
            background: linear-gradient(to top, #38bdf8, #7dd3fc); /* Langit biru */
            border-radius: 1.5rem 1.5rem 0 0;
            margin-bottom: 1.5rem;
        }

        /* Matahari */
        .sun {
            position: absolute;
            top: 10px;
            right: 10px;
            width: 40px;
            height: 40px;
            background-color: #fcd34d;
            border-radius: 50%;
            box-shadow: 0 0 15px #fcd34d;
            animation: sunPulse 3s infinite alternate;
        }

        @keyframes sunPulse {
            from { transform: scale(1); }
            to { transform: scale(1.1); }
        }

        /* Awan */
        .cloud {
            position: absolute;
            background: #fff;
            border-radius: 50%;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
            animation: moveCloud 30s linear infinite;
        }
        .cloud:nth-child(2) { width: 60px; height: 30px; top: 50px; left: -100px; animation-duration: 45s; opacity: 0.8; transform: scale(1.2); }
        .cloud:nth-child(3) { width: 90px; height: 45px; top: 20px; left: -300px; animation-duration: 35s; transform: scale(0.9); }
        .cloud:nth-child(4) { width: 70px; height: 35px; top: 80px; left: -500px; animation-duration: 40s; opacity: 0.7; }
        .cloud::before, .cloud::after {
            content: '';
            position: absolute;
            background: #fff;
            border-radius: 50%;
        }
        .cloud::before { width: 50%; height: 100%; left: -20%; top: 20%; }
        .cloud::after { width: 70%; height: 70%; right: -20%; top: -20%; }

        @keyframes moveCloud {
            0% { transform: translateX(100vw); }
            100% { transform: translateX(-100%); }
        }

        /* Bekantan */
        .bekantan {
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 100px; /* Lebar ilustrasi */
            height: 100px;
            z-index: 10;
        }
        
        .bekantan-svg {
            width: 100%;
            height: 100%;
            /* Animasi mengayun atau bergerak ringan */
            animation: subtleWiggle 5s infinite ease-in-out;
        }

        @keyframes subtleWiggle {
            0%, 100% { transform: rotate(0deg) translateY(0); }
            25% { transform: rotate(-1deg) translateY(-2px); }
            75% { transform: rotate(1deg) translateY(2px); }
        }

        /* Burung */
        .bird {
            position: absolute;
            width: 20px;
            height: 20px;
            color: #1f2937;
            animation: flyBird 15s linear infinite;
        }

        .bird:nth-child(1) { top: 30px; animation-duration: 18s; animation-delay: 0s; }
        .bird:nth-child(2) { top: 60px; animation-duration: 16s; animation-delay: -5s; transform: scale(0.7); }

        @keyframes flyBird {
            0% { left: -50px; opacity: 1; }
            50% { opacity: 0.8; }
            100% { left: 110%; opacity: 1; }
        }
        /* --- END BEKANTAN SCENE --- */


        /* --- BALLOONS CSS (Same as before) --- */
        #balloon-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1000;
            overflow: hidden;
        }

        .balloon {
            position: absolute;
            width: 40px;
            height: 50px;
            border-radius: 50%;
            transform-origin: bottom center;
            animation: floatUp 5s ease-out forwards;
        }

        .balloon::after {
            content: '';
            position: absolute;
            top: 100%;
            left: 50%;
            width: 2px;
            height: 30px;
            background: rgba(0, 0, 0, 0.4);
        }

        @keyframes floatUp {
            0% { opacity: 0; transform: translateY(0) rotate(0deg); }
            10% { opacity: 1; }
            100% { opacity: 0.5; transform: translateY(-100vh) rotate(30deg); }
        }

        /* --- BOOM CSS (Same as before) --- */
        #boom-effect {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 0;
            opacity: 0;
            pointer-events: none;
            z-index: 1001;
        }

        .active-boom {
            animation: boomAnimation 0.5s ease-out forwards;
        }

        @keyframes boomAnimation {
            0% { font-size: 50px; opacity: 1; transform: translate(-50%, -50%) scale(1); }
            50% { font-size: 150px; opacity: 1; transform: translate(-50%, -50%) scale(2); }
            100% { font-size: 200px; opacity: 0; transform: translate(-50%, -50%) scale(2.5); }
        }

        .game-screen { display: none; }
        .game-screen.active { display: block; }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">

    <!-- Effects Containers -->
    <div id="balloon-container"></div>
    <div id="boom-effect">💥</div>

    <div id="game-container" class="w-full max-w-lg bg-white p-0 rounded-xl shadow-2xl">
        
        <!-- --- 1. START SCREEN --- -->
        <div id="start-screen" class="game-screen active">
            
            <!-- BEKANTAN SCENE CONTAINER -->
            <div id="bekantan-scene">
                <!-- Sun -->
                <div class="sun"></div>
                <!-- Clouds -->
                <div class="cloud"></div>
                <div class="cloud"></div>
                <div class="cloud"></div>
                <!-- Birds -->
                <svg class="bird" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 18l-4-4-4 4"></path><path d="M16 2l-4 4-4-4"></path></svg>
                <svg class="bird" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 18l-4-4-4 4"></path><path d="M16 2l-4 4-4-4"></path></svg>
                
                <!-- Bekantan Illustration (Simplified SVG for visual) -->
                <div class="bekantan">
                    <svg class="bekantan-svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
                        <!-- Trunk (Brown) -->
                        <path d="M50 95 C 40 85, 30 50, 50 40 C 70 50, 60 85, 50 95 Z" fill="#964B00"/>
                        <!-- White Fur/Chest -->
                        <circle cx="50" cy="55" r="30" fill="#fff"/>
                        <!-- Head (Brown) -->
                        <circle cx="50" cy="35" r="25" fill="#964B00"/>
                        <!-- Nose (Orange/Red - Iconic) -->
                        <ellipse cx="50" cy="40" rx="15" ry="10" fill="#FF4500" stroke="#8B0000" stroke-width="1"/>
                        <!-- Eyes -->
                        <circle cx="40" cy="30" r="3" fill="#1f2937"/>
                        <circle cx="60" cy="30" r="3" fill="#1f2937"/>
                        <!-- Smile -->
                        <path d="M45 50 Q 50 55, 55 50" stroke="#1f2937" stroke-width="1.5" fill="none"/>
                    </svg>
                </div>
            </div>

            <div class="p-6 md:p-8 pt-0">
                <h1 class="text-3xl font-bold text-center text-primary-dark mb-4">
                    English Education Game
                </h1>
                <p class="text-center text-gray-600 mb-8">Hello! Please enter your name to start the fun!</p>
                
                <div class="space-y-6">
                    <div>
                        <label for="name-input" class="block text-sm font-medium text-gray-700 mb-1">Name</label>
                        <input type="text" id="name-input" placeholder="Your Full Name"
                            class="w-full p-3 border border-gray-300 rounded-lg focus:ring-primary-light focus:border-primary-light transition shadow-sm">
                    </div>

                    <div>
                        <label for="class-input" class="block text-sm font-medium text-gray-700 mb-1">Class/Grade</label>
                        <input type="text" id="class-input" placeholder="e.g., Grade 7A"
                            class="w-full p-3 border border-gray-300 rounded-lg focus:ring-primary-light focus:border-primary-light transition shadow-sm">
                    </div>
                </div>

                <button id="start-game-button"
                    class="w-full bg-secondary text-white font-semibold py-3 px-4 rounded-lg mt-8 shadow-xl hover:bg-purple-600 transition duration-200 focus:outline-none focus:ring-2 focus:ring-secondary focus:ring-opacity-50"
                    onclick="validateAndStartGame()" disabled>
                    Start Game
                </button>
                <p id="start-feedback" class="text-sm text-wrong mt-2 text-center hidden">Please enter your Name and Class.</p>
            </div>
        </div>

        <!-- --- 2. GAME SCREEN --- -->
        <div id="main-game-screen" class="game-screen p-6 md:p-8">
            <h1 class="text-3xl font-bold text-center text-primary-dark mb-6">
                English Education Game
            </h1>
             <div class="text-sm text-center text-gray-500 mb-4">
                Player: <span id="player-info" class="font-semibold text-secondary"></span>
            </div>

            <div class="flex justify-between items-center mb-6 border-b pb-4">
                <span class="text-lg font-semibold text-gray-700">Level: <span id="level-display" class="text-secondary">1 - Reading</span></span>
                <div class="flex items-center space-x-2">
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-timer-warn">
                        <circle cx="12" cy="12" r="10"></circle>
                        <polyline points="12 6 12 12 16 14"></polyline>
                    </svg>
                    <span class="text-lg font-extrabold text-timer-warn">Time: <span id="timer-display">60</span>s</span>
                </div>
                <span class="text-lg font-semibold text-gray-700">Score: <span id="score-display" class="text-primary-light">0</span></span>
            </div>

            <div id="content-area"></div>
            <div id="feedback-message" class="mt-6 p-3 text-center rounded-lg font-medium hidden transition-all duration-300"></div>

            <button id="next-button"
                class="w-full bg-primary-dark text-white font-semibold py-3 px-4 rounded-lg mt-6 shadow-xl hover:bg-indigo-900 transition duration-200 focus:outline-none focus:ring-2 focus:ring-primary-dark focus:ring-opacity-50"
                onclick="nextQuestion()" disabled>
                Next
            </button>
        </div>

        <!-- --- 3. GAME END MODAL --- -->
        <div id="end-modal" class="fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center p-4 hidden">
            <div class="bg-white p-8 rounded-xl shadow-2xl max-w-sm w-full text-center">
                <h2 class="text-3xl font-bold text-secondary mb-4">Game Over!</h2>
                <p class="text-gray-700 text-lg mb-2">Great job, <span id="final-name" class="font-bold text-primary-dark"></span>!</p>
                <p class="text-gray-700 text-lg mb-6">Your Total Score:</p>
                <p id="final-score-display" class="text-5xl font-extrabold text-primary-dark mb-8">0</p>
                <button onclick="showStartScreen()" class="bg-primary-light text-white font-semibold py-3 px-6 rounded-lg hover:bg-sky-500 transition shadow-lg">
                    Play Again
                </button>
            </div>
        </div>
    </div>

    <script>
        // --- GAME DATA (ENGLISH) ---
        const gameData = {
            level1: [
                { type: 'reading', passage: "My name is Tino. I have a small cat. Her name is Miu. Miu likes to play with a red ball. Every morning, I give Miu milk. She always finishes it quickly. Miu is a happy cat.", question: "What is the name of Tino's cat?", options: ["Tino", "Red", "Miu", "Milk"], answer: "Miu" },
                { type: 'reading', passage: "I live in a big house with my family. My house has three bedrooms. We have a garden outside where my mother grows flowers. I love my house because it is very cozy.", question: "How many bedrooms does the house have?", options: ["One", "Two", "Three", "Four"], answer: "Three" },
                { type: 'reading', passage: "Today is Sunday. I go to the park with my dog, Roko. Roko runs fast and chases butterflies. We stay there until the sun goes down, and then we go home.", question: "Where does the person go with Roko?", options: ["School", "Park", "Market", "Home"], answer: "Park" }
            ],
            level2: [
                { type: 'complete', sentence: "The sun is very ____ today.", question: "Complete the sentence above:", options: ["cold", "hot", "rainy", "small"], answer: "hot" },
                { type: 'complete', sentence: "I like to eat an ____ for breakfast.", question: "Complete the sentence above:", options: ["car", "apple", "door", "shoe"], answer: "apple" },
                { type: 'complete', sentence: "A bird can ____ in the sky.", question: "Complete the sentence above:", options: ["swim", "run", "fly", "sleep"], answer: "fly" }
            ],
            level3: [
                { type: 'box', question: "What is the past tense of 'go'?", options: ["went", "gone", "goed", "going"], answer: "went", boxTitle: "Box 1" },
                { type: 'box', question: "How many letters are in the English alphabet?", options: ["24", "25", "26", "27"], answer: "26", boxTitle: "Box 2" },
                { type: 'box', question: "Which of these is a fruit?", options: ["Carrot", "Potato", "Tomato", "Broccoli"], answer: "Tomato", boxTitle: "Box 3" },
                { type: 'box', question: "What is the opposite of 'big'?", options: ["Tall", "Small", "Long", "Fast"], answer: "Small", boxTitle: "Box 4" }
            ],
            level4: [
                { adjective: "Happy", synonym: "Joyful" }, { adjective: "Big", synonym: "Large" }, { adjective: "Fast", synonym: "Quick" }, { adjective: "Small", synonym: "Tiny" }
            ],
            level5: [
                { type: 'scramble', correctSentence: "She is learning English at school now.", words: ["learning", "at", "She", "school", "is", "English", "now", "."], points: 20 },
                { type: 'scramble', correctSentence: "My favorite color is blue and white.", words: ["is", "My", "blue", "and", "color", "favorite", "white", "."], points: 20 },
                { type: 'scramble', correctSentence: "Did you finish your homework yesterday?", words: ["Did", "homework", "your", "finish", "yesterday", "?"], points: 20 },
                { type: 'scramble', correctSentence: "The children are playing outside in the park.", words: ["are", "playing", "The", "outside", "park", "children", "in", "the", "."], points: 20 },
                { type: 'scramble', correctSentence: "I think mathematics is a difficult subject.", words: ["mathematics", "is", "think", "I", "a", "difficult", "subject", "."], points: 20 }
            ]
        };

        // --- VARIABLES ---
        let playerName = ''; let playerClass = ''; let currentLevel = 1; let currentQuestionIndex = 0; let score = 0; let currentQuestions = [];
        let isAnswered = false; let selectedAdjectiveButton = null; let selectedSynonymButton = null; let wordBank = []; let currentSentence = [];
        const TIME_LIMIT = 60; let timerInterval;

        // --- ELEMENTS ---
        const startScreen = document.getElementById('start-screen'); const nameInput = document.getElementById('name-input'); const classInput = document.getElementById('class-input');
        const startGameButton = document.getElementById('start-game-button'); const startFeedback = document.getElementById('start-feedback'); const mainGameScreen = document.getElementById('main-game-screen');
        const playerInfoDisplay = document.getElementById('player-info'); const contentArea = document.getElementById('content-area'); const levelDisplay = document.getElementById('level-display');
        const scoreDisplay = document.getElementById('score-display'); const feedbackMessage = document.getElementById('feedback-message'); const nextButton = document.getElementById('next-button');
        const endModal = document.getElementById('end-modal'); const finalScoreDisplay = document.getElementById('final-score-display'); const finalNameDisplay = document.getElementById('final-name');
        const balloonContainer = document.getElementById('balloon-container'); const timerDisplay = document.getElementById('timer-display'); const boomEffect = document.getElementById('boom-effect');

        // --- SCREEN LOGIC ---
        function switchScreen(screenId) {
            document.querySelectorAll('.game-screen').forEach(screen => screen.classList.remove('active'));
            document.getElementById(screenId).classList.add('active');
        }

        function showStartScreen() {
            nameInput.value = ''; classInput.value = ''; startGameButton.disabled = true; startFeedback.classList.add('hidden'); endModal.classList.add('hidden');
            switchScreen('start-screen');
        }

        function validateAndStartGame() {
            playerName = nameInput.value.trim(); playerClass = classInput.value.trim();
            if (playerName === '' || playerClass === '') { startFeedback.classList.remove('hidden'); return; }
            startFeedback.classList.add('hidden'); playerInfoDisplay.textContent = `${playerName} | Class: ${playerClass}`;
            switchScreen('main-game-screen'); startGame();
        }

        nameInput.addEventListener('input', checkInputs); classInput.addEventListener('input', checkInputs);
        function checkInputs() { startGameButton.disabled = !(nameInput.value.trim() && classInput.value.trim()); }

        // --- AUDIO ---
        const audioContext = new (window.AudioContext || window.webkitAudioContext)();
        function playCorrectSound() {
            try {
                const osc = audioContext.createOscillator(); const gain = audioContext.createGain();
                osc.connect(gain); gain.connect(audioContext.destination); osc.type = 'triangle';
                gain.gain.setValueAtTime(0, audioContext.currentTime); gain.gain.linearRampToValueAtTime(0.3, audioContext.currentTime + 0.05);
                osc.frequency.setValueAtTime(880, audioContext.currentTime); osc.frequency.linearRampToValueAtTime(1320, audioContext.currentTime + 0.15);
                osc.start(audioContext.currentTime); gain.gain.exponentialRampToValueAtTime(0.001, audioContext.currentTime + 0.3); osc.stop(audioContext.currentTime + 0.3);
            } catch(e) {}
        }
        function playWrongSound() {
            try {
                const osc = audioContext.createOscillator(); const gain = audioContext.createGain();
                osc.connect(gain); gain.connect(audioContext.destination); osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(150, audioContext.currentTime); gain.gain.setValueAtTime(0.5, audioContext.currentTime);
                osc.start(audioContext.currentTime); gain.gain.exponentialRampToValueAtTime(0.001, audioContext.currentTime + 0.2); osc.stop(audioContext.currentTime + 0.2);
            } catch(e) {}
        }

        // --- HELPERS ---
        function shuffleArray(array) { for (let i = array.length - 1; i > 0; i--) { const j = Math.floor(Math.random() * (i + 1)); [array[i], array[j]] = [array[j], array[i]]; } return array; }
        function triggerBoom() { boomEffect.classList.remove('active-boom'); void boomEffect.offsetWidth; boomEffect.classList.add('active-boom'); setTimeout(() => { boomEffect.classList.remove('active-boom'); }, 500); playWrongSound(); }
        function triggerBalloons() {
            const colors = ['#38bdf8', '#a855f7', '#6366f1', '#10b981', '#3730a3', '#ff6b6b'];
            for(let i=0; i<10; i++) {
                const b = document.createElement('div'); b.className = 'balloon'; b.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                b.style.boxShadow = `0 4px 6px rgba(0,0,0,0.2)`; b.style.left = `${Math.random() * window.innerWidth}px`; b.style.top = `${window.innerHeight + 10}px`;
                b.style.animationDuration = `${4 + Math.random() * 2}s`; b.style.animationDelay = `${Math.random() * 0.5}s`;
                balloonContainer.appendChild(b); setTimeout(() => { if(balloonContainer.contains(b)) balloonContainer.removeChild(b); }, (4.5 + Math.random()) * 1000);
            }
            playCorrectSound();
        }

        // --- TIMER ---
        function startTimer() {
            clearInterval(timerInterval); let timeLeft = TIME_LIMIT; timerDisplay.textContent = timeLeft; timerDisplay.classList.remove('text-wrong');
            timerInterval = setInterval(() => {
                timeLeft--; timerDisplay.textContent = timeLeft;
                if (timeLeft <= 10) timerDisplay.classList.add('text-wrong'); else timerDisplay.classList.remove('text-wrong');
                if (timeLeft <= 0) { clearInterval(timerInterval); handleTimeUp(); }
            }, 1000);
        }
        function handleTimeUp() {
            if (isAnswered) return; isAnswered = true; triggerBoom();
            feedbackMessage.classList.remove('hidden', 'bg-correct'); feedbackMessage.classList.add('bg-wrong', 'text-white');
            feedbackMessage.textContent = currentLevel === 3 ? "Time's Up! 0 Points." : "Time's Up! 0 Points. Next.";
            nextButton.textContent = currentLevel === 3 ? "Back to Boxes" : "Next";
            document.querySelectorAll('#content-area button').forEach(btn => btn.disabled = true); nextButton.disabled = false;
            if (currentLevel === 3 && currentQuestions[currentQuestionIndex]) currentQuestions[currentQuestionIndex].answered = true;
        }

        // --- GAME LOGIC ---
        function startGame() { currentLevel = 1; score = 0; currentQuestionIndex = 0; updateScoreDisplay(); endModal.classList.add('hidden'); loadLevel(currentLevel); }
        function loadLevel(level) {
            currentLevel = level; currentQuestionIndex = 0; isAnswered = false; nextButton.disabled = true; feedbackMessage.classList.add('hidden'); nextButton.textContent = "Next"; clearInterval(timerInterval);
            if (level === 1) { currentQuestions = shuffleArray([...gameData.level1]); levelDisplay.textContent = "1 - Reading"; displayQuestion(currentQuestions[currentQuestionIndex]); startTimer(); }
            else if (level === 2) { currentQuestions = shuffleArray([...gameData.level2]); levelDisplay.textContent = "2 - Sentences"; displayQuestion(currentQuestions[currentQuestionIndex]); startTimer(); }
            else if (level === 3) { currentQuestions = shuffleArray([...gameData.level3]).map(q => ({...q, answered: false})); levelDisplay.textContent = "3 - Open Box"; displayLevel3BoxSelection(); }
            else if (level === 4) { currentQuestions = shuffleArray([...gameData.level4]).map(q => ({...q, matched: false})); levelDisplay.textContent = "4 - Synonyms"; selectedAdjectiveButton = null; selectedSynonymButton = null; displayMatchingQuestion(); startTimer(); }
            else if (level === 5) { currentQuestions = shuffleArray([...gameData.level5]); levelDisplay.textContent = "5 - Scramble"; displayScrambleQuestion(currentQuestions[currentQuestionIndex]); startTimer(); }
            else { showEndModal(); }
        }

        function displayQuestion(q) {
            contentArea.innerHTML = '';
            if (q.type === 'reading') contentArea.innerHTML += `<div class="mb-5 p-4 bg-primary-light bg-opacity-10 border-l-4 border-primary-light rounded-lg shadow-inner"><p class="font-bold text-primary-dark mb-2">Reading:</p><p class="text-sm text-gray-700">${q.passage}</p></div>`;
            contentArea.innerHTML += `<div class="mb-5"><p class="text-xl font-bold text-gray-800 mb-4">${currentQuestionIndex + 1}. ${q.question}</p>`;
            let opts = `<div id="options-container" class="space-y-3">`;
            q.options.forEach(opt => {
                const txt = q.type === 'complete' ? q.sentence.replace('____', `<span class="text-primary-dark font-semibold">${opt}</span>`) : opt;
                opts += `<button class="answer-button w-full text-left bg-white border border-gray-300 p-3 rounded-lg shadow-md text-gray-800 hover:bg-bg-start" data-option="${opt}" onclick="checkAnswer(this, '${q.answer}')">${txt}</button>`;
            });
            contentArea.innerHTML += opts + `</div></div>`;
        }

        function checkAnswer(btn, ans, boxIdx = -1) {
            if (isAnswered) return; clearInterval(timerInterval); isAnswered = true;
            const sel = btn.getAttribute('data-option');
            document.querySelectorAll('#options-container button').forEach(b => {
                b.disabled = true; b.classList.remove('hover:bg-bg-start');
                if (b.getAttribute('data-option') === ans) b.classList.add('bg-correct', 'text-white', 'border-correct');
                else if (b === btn) b.classList.add('bg-wrong', 'text-white', 'border-wrong');
                else b.classList.add('bg-gray-100', 'text-gray-500');
            });
            feedbackMessage.classList.remove('hidden', 'bg-correct', 'bg-wrong', 'text-white');
            if (sel === ans) { score += 10; feedbackMessage.textContent = "Correct! +10 pts"; feedbackMessage.classList.add('bg-correct', 'text-white'); triggerBalloons(); if (boxIdx !== -1) currentQuestions[boxIdx].answered = true; }
            else { feedbackMessage.textContent = `Wrong. Answer: "${ans}"`; feedbackMessage.classList.add('bg-wrong', 'text-white'); triggerBoom(); }
            updateScoreDisplay(); nextButton.textContent = currentLevel === 3 ? "Back to Boxes" : "Next"; nextButton.disabled = false;
        }

        function displayLevel3BoxSelection() {
            clearInterval(timerInterval); timerDisplay.textContent = TIME_LIMIT; contentArea.innerHTML = ''; nextButton.disabled = true; feedbackMessage.classList.add('hidden'); nextButton.textContent = "Next";
            if (currentQuestions.every(q => q.answered)) { loadLevel(4); return; }
            let html = `<p class="text-xl font-bold text-gray-800 mb-6 text-center">Select a Box:</p><div class="grid grid-cols-2 gap-4">`;
            currentQuestions.forEach((q, idx) => {
                html += `<button class="flex items-center justify-center h-28 p-4 rounded-xl font-extrabold text-2xl transition duration-200 transform ${q.answered ? '' : 'hover:scale-105'} ${q.answered ? 'bg-secondary text-white shadow-inner opacity-70 cursor-not-allowed' : 'bg-primary-dark text-white shadow-xl hover:bg-indigo-900 cursor-pointer'}" onclick="openBox(${idx})" ${q.answered ? 'disabled' : ''}>${q.answered ? 'DONE' : q.boxTitle}</button>`;
            });
            contentArea.innerHTML = html + `</div>`;
        }
        function openBox(idx) { currentQuestionIndex = idx; contentArea.innerHTML = ''; displayQuestion(currentQuestions[idx]); startTimer(); nextButton.textContent = "Back to Boxes"; }

        function displayMatchingQuestion() {
            contentArea.innerHTML = ''; feedbackMessage.classList.add('hidden'); nextButton.disabled = true;
            if (currentQuestions.every(q => q.matched)) { clearInterval(timerInterval); isAnswered = true; nextButton.disabled = false; feedbackMessage.textContent = "Done! Click Next."; return; }
            const unmatched = currentQuestions.filter(q => !q.matched);
            const adjs = shuffleArray(unmatched.map(p => p.adjective)); const syns = shuffleArray(unmatched.map(p => p.synonym));
            let html = `<p class="text-xl font-bold text-gray-800 mb-6 text-center">Match Adjectives & Synonyms:</p><div class="flex justify-around space-x-4"><div class="w-1/2 space-y-3"><p class="font-semibold text-primary-dark text-center">Adjectives</p>`;
            adjs.forEach(w => html += `<button class="matching-button w-full text-center bg-white border border-gray-300 p-3 rounded-lg shadow-md text-gray-800 transition duration-150" data-type="adj" data-val="${w}" onclick="handleMatch(this, 'adj')">${w}</button>`);
            html += `</div><div class="w-1/2 space-y-3"><p class="font-semibold text-primary-light text-center">Synonyms</p>`;
            syns.forEach(w => html += `<button class="matching-button w-full text-center bg-white border border-gray-300 p-3 rounded-lg shadow-md text-gray-800 transition duration-150" data-type="syn" data-val="${w}" onclick="handleMatch(this, 'syn')">${w}</button>`);
            contentArea.innerHTML = html + `</div></div>`;
        }
        function handleMatch(btn, type) {
            if (isAnswered) return; feedbackMessage.classList.add('hidden');
            if (type === 'adj') { if (selectedAdjectiveButton) selectedAdjectiveButton.classList.remove('selected'); selectedAdjectiveButton = btn; btn.classList.add('selected'); }
            else { if (selectedSynonymButton) selectedSynonymButton.classList.remove('selected'); selectedSynonymButton = btn; btn.classList.add('selected'); }
            if (selectedAdjectiveButton && selectedSynonymButton) {
                const a = selectedAdjectiveButton.getAttribute('data-val'); const s = selectedSynonymButton.getAttribute('data-val');
                const match = gameData.level4.find(p => p.adjective === a && p.synonym === s);
                feedbackMessage.classList.remove('hidden', 'bg-correct', 'bg-wrong', 'text-white');
                if (match) {
                    score += 10; updateScoreDisplay(); triggerBalloons(); feedbackMessage.textContent = "Matched!"; feedbackMessage.classList.add('bg-correct', 'text-white');
                    currentQuestions.find(q => q.adjective === a).matched = true;
                    [selectedAdjectiveButton, selectedSynonymButton].forEach(b => { b.disabled = true; b.classList.remove('selected'); b.classList.add('bg-primary-light', 'text-white', 'opacity-50'); });
                } else {
                    feedbackMessage.textContent = "Wrong pair."; feedbackMessage.classList.add('bg-wrong', 'text-white'); triggerBoom();
                    [selectedAdjectiveButton, selectedSynonymButton].forEach(b => b.classList.remove('selected'));
                }
                selectedAdjectiveButton = null; selectedSynonymButton = null;
                if (currentQuestions.every(q => q.matched)) { clearInterval(timerInterval); isAnswered = true; nextButton.disabled = false; feedbackMessage.textContent = "Done! Click Next."; }
            }
        }

        function displayScrambleQuestion(q) {
            contentArea.innerHTML = ''; isAnswered = false; nextButton.disabled = true; feedbackMessage.classList.add('hidden'); nextButton.textContent = "Next";
            wordBank = shuffleArray([...q.words]); currentSentence = [];
            const progress = (currentQuestionIndex / gameData.level5.length) * 100;
            let html = `<div class="mb-6"><p class="text-xl font-bold text-gray-800 mb-4 text-center">Car Race: Q${currentQuestionIndex+1}/${gameData.level5.length}</p><div class="w-full h-4 bg-gray-200 rounded-full overflow-hidden relative shadow-inner mb-6"><div class="h-full bg-primary-light transition-all duration-500" style="width: ${progress}%;"></div><span class="absolute top-1/2 -translate-y-1/2 text-xl transition-all duration-500 transform -translate-x-1/2" style="left: ${progress}%;">🚗</span></div>`;
            html += `<div class="mb-6 p-4 border-2 border-dashed border-secondary rounded-lg min-h-[80px] bg-purple-50 flex flex-wrap gap-2" id="sentence-area"><p class="text-gray-500 italic text-center w-full" id="placeholder">Arrange here...</p></div>`;
            html += `<div class="mb-6"><p class="font-semibold text-gray-700 mb-2">Words:</p><div class="flex flex-wrap gap-2" id="word-bank">`;
            wordBank.forEach((w, i) => html += `<button class="scramble-button bg-white border border-gray-300 py-2 px-4 rounded-full text-sm text-gray-800 shadow-sm hover:bg-yellow-100 transition" data-word="${w}" onclick="moveWord(this, 'bank')">${w}</button>`);
            contentArea.innerHTML = html + `</div></div></div><div class="flex justify-center space-x-4"><button class="bg-gray-500 text-white font-semibold py-2 px-4 rounded-lg shadow-md hover:bg-gray-600" onclick="resetScramble()">Reset</button><button id="check-scramble" class="bg-secondary text-white font-semibold py-2 px-4 rounded-lg shadow-md hover:bg-purple-600" onclick="checkScramble('${q.correctSentence}', ${q.points})">Check</button></div>`;
        }
        function moveWord(btn, from) {
            if (isAnswered) return; const w = btn.getAttribute('data-word'); btn.remove();
            const newBtn = document.createElement('button'); newBtn.textContent = w; newBtn.setAttribute('data-word', w);
            if (from === 'bank') {
                newBtn.className = 'scramble-button bg-primary-dark text-white py-2 px-4 rounded-full text-sm shadow-md hover:bg-indigo-900 transition';
                newBtn.onclick = () => moveWord(newBtn, 'sent'); document.getElementById('sentence-area').appendChild(newBtn); currentSentence.push(w);
                document.getElementById('placeholder').classList.add('hidden');
            } else {
                newBtn.className = 'scramble-button bg-white border border-gray-300 py-2 px-4 rounded-full text-sm text-gray-800 shadow-sm hover:bg-yellow-100 transition';
                newBtn.onclick = () => moveWord(newBtn, 'bank'); document.getElementById('word-bank').appendChild(newBtn);
                currentSentence.splice(currentSentence.indexOf(w), 1);
                if (currentSentence.length === 0) document.getElementById('placeholder').classList.remove('hidden');
            }
        }
        function resetScramble() { if (!isAnswered) displayScrambleQuestion(currentQuestions[currentQuestionIndex]); }
        function checkScramble(ans, pts) {
            if (isAnswered) return; clearInterval(timerInterval);
            let userAns = currentSentence.join(' ').trim().replace(/\s([.,?!])/g, '$1');
            isAnswered = true; nextButton.disabled = false; document.getElementById('check-scramble').disabled = true;
            document.querySelectorAll('.scramble-button').forEach(b => b.disabled = true);
            const area = document.getElementById('sentence-area'); area.innerHTML = '';
            feedbackMessage.classList.remove('hidden', 'bg-correct', 'bg-wrong', 'text-white');
            if (userAns === ans.trim()) {
                score += pts; feedbackMessage.textContent = "Correct! +20 pts"; feedbackMessage.classList.add('bg-correct', 'text-white'); triggerBalloons();
                area.innerHTML = `<p class="text-lg font-semibold text-correct text-center">${ans}</p>`; area.classList.add('bg-green-100', 'border-correct', 'border-solid'); area.classList.remove('border-dashed');
            } else {
                feedbackMessage.textContent = `Wrong. Correct: "${ans}"`; feedbackMessage.classList.add('bg-wrong', 'text-white'); triggerBoom();
                area.innerHTML = `<p class="text-lg font-semibold text-wrong text-center">Yours: ${userAns}</p>`; area.classList.add('bg-red-100', 'border-wrong', 'border-solid'); area.classList.remove('border-dashed');
            }
            updateScoreDisplay();
        }

        function nextQuestion() {
            clearInterval(timerInterval);
            // FIX START: Reset isAnswered here so the buttons on the next question are clickable
            isAnswered = false; 
            // FIX END

            if (currentLevel <= 2) { currentQuestionIndex++; if (currentQuestionIndex < currentQuestions.length) { displayQuestion(currentQuestions[currentQuestionIndex]); startTimer(); } else loadLevel(currentLevel + 1); }
            else if (currentLevel === 3) { if (currentQuestions.every(q => q.answered)) loadLevel(4); else displayLevel3BoxSelection(); }
            else if (currentLevel === 4) loadLevel(5);
            else if (currentLevel === 5) { currentQuestionIndex++; if (currentQuestionIndex < currentQuestions.length) { displayScrambleQuestion(currentQuestions[currentQuestionIndex]); startTimer(); } else loadLevel(6); }
            else showEndModal();
        }

        function updateScoreDisplay() { scoreDisplay.textContent = score; }
        function showEndModal() { clearInterval(timerInterval); finalScoreDisplay.textContent = score; finalNameDisplay.textContent = playerName; endModal.classList.remove('hidden'); }

        window.onload = showStartScreen;
    </script>
</body>
</html>
