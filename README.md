<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sembol Dedektifi Ali: Şehir Macerası</title>
    <!-- Tailwind CSS for UI styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for visual icons -->
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <!-- Canvas Confetti for victory celebration -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Fredoka:wght@500;600;700;800&display=swap');
        
        body {
            font-family: 'Fredoka', sans-serif;
            background-color: #ecfdf5;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }

        /* Bounce animation for interactive avatar */
        .bounce-avatar {
            animation: gentleBounce 2s infinite ease-in-out;
        }

        @keyframes gentleBounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-12px); }
        }

        /* Talking animation for speech indicators */
        .talking-pulse {
            animation: pulseGlow 1.2s infinite alternate;
        }

        @keyframes pulseGlow {
            from { box-shadow: 0 0 10px rgba(16, 185, 129, 0.4); }
            to { box-shadow: 0 0 30px rgba(16, 185, 129, 0.9); }
        }

        /* Shake animation for wrong answer */
        .shake-element {
            animation: shake 0.5s cubic-bezier(.36,.07,.19,.97) both;
        }

        @keyframes shake {
            10%, 90% { transform: translate3d(-2px, 0, 0); }
            20%, 80% { transform: translate3d(4px, 0, 0); }
            30%, 50%, 70% { transform: translate3d(-8px, 0, 0); }
            40%, 60% { transform: translate3d(8px, 0, 0); }
        }

        /* Big touch feedback */
        .big-btn:active {
            transform: scale(0.92);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between items-center p-2 sm:p-4 bg-gradient-to-b from-sky-100 via-emerald-50 to-teal-100">

    <!-- Top Bar / Visual Progress Header -->
    <header class="w-full max-w-4xl bg-white/95 backdrop-blur-md rounded-3xl p-3 sm:p-4 shadow-lg border-4 border-emerald-300 flex justify-between items-center">
        <!-- Ali Voice Assistant Info -->
        <div class="flex items-center gap-3">
            <div id="ali-avatar-mini" class="w-12 h-12 sm:w-16 sm:h-16 bg-amber-300 rounded-full border-4 border-amber-400 flex items-center justify-center text-2xl sm:text-4xl shadow-md bounce-avatar cursor-pointer" onclick="repeatQuestion()">
                👦
            </div>
            <div>
                <h1 class="text-base sm:text-2xl font-extrabold text-emerald-800 tracking-wide">Sembol Dedektifi Ali</h1>
                <p class="text-xs sm:text-sm font-bold text-emerald-600 flex items-center gap-1">
                    <i class="fa-solid fa-volume-high animate-pulse"></i> Sesli & Görsel Oyun
                </p>
            </div>
        </div>

        <!-- Visual Progress Dots (No numbers required for non-readers) -->
        <div class="flex items-center gap-2" id="progress-dots">
            <!-- Progress indicators rendered dynamically -->
        </div>

        <!-- Big Speaker Replay Button -->
        <button onclick="repeatQuestion()" class="w-12 h-12 sm:w-16 sm:h-16 bg-amber-400 hover:bg-amber-500 active:scale-90 text-amber-950 rounded-2xl flex items-center justify-center text-2xl sm:text-3xl shadow-md border-b-4 border-amber-600 transition-all" title="Sesi Tekrar Et">
            <i class="fa-solid fa-volume-high"></i>
        </button>
    </header>

    <!-- Main Interactive Area -->
    <main class="w-full max-w-4xl my-auto bg-white/95 rounded-3xl p-4 sm:p-6 shadow-2xl border-4 border-emerald-400 min-h-[480px] flex flex-col justify-between relative overflow-hidden">

        <!-- START SCREEN -->
        <div id="screen-start" class="flex flex-col items-center justify-center text-center space-y-6 py-6">
            <!-- Big Friendly Ali Avatar -->
            <div class="relative">
                <div class="w-36 h-36 sm:w-48 sm:h-48 bg-amber-200 rounded-full border-8 border-amber-400 flex items-center justify-center text-7xl sm:text-9xl shadow-2xl bounce-avatar">
                    👦
                </div>
                <div class="absolute -bottom-2 -right-2 bg-emerald-500 text-white p-3 sm:p-4 rounded-full text-2xl sm:text-3xl shadow-lg border-4 border-white">
                    🔍
                </div>
            </div>

            <div class="bg-amber-100 border-4 border-amber-300 rounded-3xl p-4 sm:p-6 max-w-md shadow-sm">
                <p class="text-2xl sm:text-3xl font-black text-amber-950">Şehir Macerasına Hoş Geldin!</p>
                <p class="text-lg sm:text-xl font-bold text-emerald-700 mt-1">Soru Sesi Dinlemek İçin Butona Bas!</p>
            </div>

            <!-- Big Audio Play Button -->
            <button onclick="startGame()" class="big-btn bg-emerald-500 hover:bg-emerald-600 text-white font-black text-2xl sm:text-4xl px-10 py-6 rounded-3xl shadow-xl border-b-8 border-emerald-700 transition-all flex items-center gap-4 group">
                <i class="fa-solid fa-circle-play text-4xl sm:text-5xl group-hover:scale-110 transition-transform"></i>
                <span>OYUNA BAŞLA</span>
            </button>
        </div>

        <!-- GAME LEVEL SCREEN -->
        <div id="screen-level" class="hidden flex-col justify-between h-full space-y-4">
            
            <!-- Voice Prompt Bar with Big Loudspeaker -->
            <div onclick="repeatQuestion()" class="bg-amber-100 hover:bg-amber-200 cursor-pointer border-4 border-amber-300 p-3 sm:p-4 rounded-2xl flex items-center gap-3 sm:gap-4 shadow-md transition-all talking-pulse">
                <div class="w-12 h-12 sm:w-16 sm:h-16 bg-amber-400 text-amber-950 rounded-full flex items-center justify-center text-2xl sm:text-3xl shrink-0 shadow">
                    <i class="fa-solid fa-bullhorn"></i>
                </div>
                <div class="flex-1">
                    <span id="level-voice-hint" class="text-base sm:text-2xl font-bold text-amber-950 block">...</span>
                </div>
                <div class="text-amber-800 text-2xl sm:text-3xl pr-2">
                    <i class="fa-solid fa-rotate-right"></i>
                </div>
            </div>

            <!-- Main Question Symbol Illustration Box -->
            <div class="bg-slate-50 border-4 border-dashed border-slate-300 rounded-3xl p-4 sm:p-6 flex items-center justify-center min-h-[170px] sm:min-h-[200px] shadow-inner relative">
                <div id="symbol-display" class="flex items-center justify-center w-full">
                    <!-- Target visual symbol renders here -->
                </div>
            </div>

            <!-- Choice Options Grid (2 Big Visual Buttons for easy selection) -->
            <div id="choices-grid" class="grid grid-cols-2 gap-3 sm:gap-6">
                <!-- Choice buttons rendered dynamically -->
            </div>
        </div>

        <!-- VICTORY / DEDECITVE CERTIFICATE SCREEN -->
        <div id="screen-victory" class="hidden flex-col items-center justify-center text-center space-y-6 py-4">
            <!-- Glowing Trophy & Ali -->
            <div class="flex items-center justify-center gap-4">
                <div class="w-24 h-24 sm:w-32 sm:h-32 bg-amber-300 rounded-full border-4 border-amber-400 flex items-center justify-center text-5xl sm:text-6xl shadow-xl bounce-avatar">
                    👦
                </div>
                <div class="w-24 h-24 sm:w-32 sm:h-32 bg-amber-400 rounded-full border-4 border-amber-500 flex items-center justify-center text-5xl sm:text-6xl shadow-xl animate-bounce">
                    🏆
                </div>
            </div>

            <div class="bg-amber-100 border-4 border-amber-400 rounded-3xl p-6 shadow-md max-w-lg">
                <p class="text-3xl sm:text-4xl font-extrabold text-amber-950">TEBRİKLER!</p>
                <p class="text-xl sm:text-2xl font-black text-emerald-700 mt-2">Sen Artık Harika Bir Sembol Dedektifisin! 🏅</p>
            </div>

            <!-- Restart Big Button -->
            <button onclick="startGame()" class="big-btn bg-sky-500 hover:bg-sky-600 text-white font-black text-2xl sm:text-3xl px-10 py-5 rounded-3xl shadow-xl border-b-8 border-sky-700 transition-all flex items-center gap-4">
                <i class="fa-solid fa-rotate-right text-3xl"></i>
                <span>TEKRAR OYNA</span>
            </button>
        </div>

    </main>

    <footer class="w-full text-center py-2 text-emerald-800 text-xs sm:text-sm font-bold opacity-90">
        1. Sınıf Dostu • Sesli ve Görsel Rehberli Oyun
    </footer>

    <script>
        let currentLevel = 0;
        let isSpeaking = false;
        const speechSynth = window.speechSynthesis;

        // Sound Synthesis helper using Web Audio API for interactive feedback
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

        function playBeep(type) {
            try {
                if (audioCtx.state === 'suspended') {
                    audioCtx.resume();
                }
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);

                if (type === 'success') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(523.25, audioCtx.currentTime); // C5
                    osc.frequency.setValueAtTime(659.25, audioCtx.currentTime + 0.1); // E5
                    osc.frequency.setValueAtTime(783.99, audioCtx.currentTime + 0.2); // G5
                    gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
                    osc.start(audioCtx.currentTime);
                    osc.stop(audioCtx.currentTime + 0.4);
                } else if (type === 'fail') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(220, audioCtx.currentTime); // A3
                    osc.frequency.setValueAtTime(164.81, audioCtx.currentTime + 0.15); // E3
                    gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
                    gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.35);
                    osc.start(audioCtx.currentTime);
                    osc.stop(audioCtx.currentTime + 0.35);
                }
            } catch (e) {
                console.log("Audio FX not supported");
            }
        }

        /* 
           Game Levels Definition based strictly on updated requirements:
           1. Pedestrian Red Signal (DUR/STOP)
           2. Pedestrian Green Signal (GÜVENLE GEÇ)
           3. No Grass Walking Prohibition Sign (Görseldeki gibi: Çim üzerine basan ayakkabı ve kırmızı yasak çizgisi)
           4. Yellow Tactile Paving (GÖRME ENGELLİLER İÇİN)
        */
        const gameLevels = [
            {
                // Level 1: Red Pedestrian Signal
                promptAudio: "Yayalar için kırmızı ışık yanarken ne yapmalıyız?",
                symbolHtml: `
                    <div class="flex flex-col items-center bg-slate-900 p-5 rounded-3xl border-4 border-slate-700 shadow-2xl">
                        <!-- Red Standing Light -->
                        <div class="w-20 h-20 sm:w-24 sm:h-24 bg-red-600 rounded-full flex items-center justify-center text-white text-4xl sm:text-5xl shadow-[0_0_25px_rgba(220,38,38,0.9)] border-4 border-red-400 animate-pulse">
                            🧍
                        </div>
                        <!-- Green Off Light -->
                        <div class="w-16 h-16 sm:w-20 sm:h-20 bg-slate-800 rounded-full mt-3 opacity-30 border-2 border-slate-600"></div>
                    </div>
                `,
                options: [
                    {
                        correct: true,
                        audio: "Harikasın! Kırmızı ışıkta DUR VE BEKLE!",
                        failAudio: "Tekrar dene bakalım! Kırmızı ışıkta durmalıyız.",
                        icon: "🛑",
                        label: "DUR VE BEKLE",
                        color: "bg-red-500 border-red-700 text-white"
                    },
                    {
                        correct: false,
                        audio: "Kırmızı ışıkta yola atlamamalıyız!",
                        failAudio: "Tekrar dene bakalım! Kırmızı ışıkta geçilmez.",
                        icon: "🏃",
                        label: "YOLA ATLA",
                        color: "bg-emerald-500 border-emerald-700 text-white"
                    }
                ]
            },
            {
                // Level 2: Green Pedestrian Signal
                promptAudio: "Yayalar için yeşil ışık yandığında ne yapmalıyız?",
                symbolHtml: `
                    <div class="flex flex-col items-center bg-slate-900 p-5 rounded-3xl border-4 border-slate-700 shadow-2xl">
                        <!-- Red Off Light -->
                        <div class="w-16 h-16 sm:w-20 sm:h-20 bg-slate-800 rounded-full opacity-30 border-2 border-slate-600"></div>
                        <!-- Green Walking Light -->
                        <div class="w-20 h-20 sm:w-24 sm:h-24 bg-emerald-500 rounded-full mt-3 flex items-center justify-center text-white text-4xl sm:text-5xl shadow-[0_0_25px_rgba(16,185,129,0.9)] border-4 border-emerald-300 animate-pulse">
                            🚶
                        </div>
                    </div>
                `,
                options: [
                    {
                        correct: true,
                        audio: "Çok doğru! Yeşil ışıkta GÜVENLE GEÇEBİLİRİZ!",
                        failAudio: "Tekrar dene bakalım! Yeşil ışık bize izin verir.",
                        icon: "🚶‍♂️",
                        label: "GÜVENLE GEÇ",
                        color: "bg-emerald-500 border-emerald-700 text-white"
                    },
                    {
                        correct: false,
                        audio: "Yeşil ışık yandığında beklemen gerekmez!",
                        failAudio: "Tekrar dene bakalım!",
                        icon: "🛑",
                        label: "BEKLE / DUR",
                        color: "bg-amber-500 border-amber-700 text-white"
                    }
                ]
            },
            {
                // Level 3: Prohibition Sign - Do Not Walk on Grass (Matches user image exactly)
                promptAudio: "Kırmızı daire içinde çime basan ayakkabı ve kırmızı çizgi olan bu tabela bize ne söyler?",
                symbolHtml: `
                    <div class="relative w-44 h-44 sm:w-56 sm:h-56 bg-white rounded-full border-[10px] sm:border-[14px] border-red-600 shadow-2xl flex items-center justify-center p-2 overflow-hidden">
                        <!-- Detailed Custom SVG matching the exact illustration provided by user -->
                        <svg viewBox="0 0 200 200" class="w-full h-full">
                            <!-- Grass background blades inside circle -->
                            <path d="M 40 150 Q 45 110 50 145" stroke="#22c55e" stroke-width="6" stroke-linecap="round" fill="none" />
                            <path d="M 55 150 Q 60 100 68 145" stroke="#16a34a" stroke-width="7" stroke-linecap="round" fill="none" />
                            <path d="M 72 150 Q 80 95 88 145" stroke="#22c55e" stroke-width="6" stroke-linecap="round" fill="none" />
                            <path d="M 92 150 Q 100 105 108 145" stroke="#15803d" stroke-width="7" stroke-linecap="round" fill="none" />
                            <path d="M 112 150 Q 120 90 128 145" stroke="#22c55e" stroke-width="6" stroke-linecap="round" fill="none" />
                            <path d="M 132 150 Q 140 100 148 145" stroke="#16a34a" stroke-width="7" stroke-linecap="round" fill="none" />
                            <path d="M 152 150 Q 158 115 162 145" stroke="#22c55e" stroke-width="6" stroke-linecap="round" fill="none" />

                            <!-- Sneaker stepping down onto grass -->
                            <g transform="translate(30, 20) rotate(-15)">
                                <!-- Leg/Pant -->
                                <path d="M 85 20 L 115 25 L 110 65 L 80 60 Z" fill="#2563eb" />
                                <ellipse cx="95" cy="62" rx="15" ry="4" fill="#1d4ed8" />
                                <!-- Sock -->
                                <path d="M 83 60 L 107 63 L 105 75 L 81 72 Z" fill="#f8fafc" />
                                <!-- Sneaker Body -->
                                <path d="M 75 72 C 75 72, 100 70, 120 80 C 130 85, 140 100, 135 112 C 130 118, 110 118, 65 112 C 55 110, 50 100, 52 90 C 55 80, 65 72, 75 72 Z" fill="#881337" />
                                <!-- Sneaker Sole -->
                                <path d="M 50 106 C 50 106, 110 114, 137 112 C 140 118, 132 124, 120 124 C 90 124, 50 120, 48 112 Z" fill="#ffffff" stroke="#cbd5e1" stroke-width="2" />
                                <!-- Toe Cap -->
                                <path d="M 120 80 C 130 85, 138 98, 135 112 C 122 112, 118 95, 120 80 Z" fill="#ffffff" />
                                <!-- Laces -->
                                <line x1="85" y1="78" x2="95" y2="82" stroke="#ffffff" stroke-width="3" stroke-linecap="round" />
                                <line x1="90" y1="86" x2="102" y2="90" stroke="#ffffff" stroke-width="3" stroke-linecap="round" />
                                <line x1="96" y1="94" x2="108" y2="98" stroke="#ffffff" stroke-width="3" stroke-linecap="round" />
                            </g>

                            <!-- Red Diagonal Prohibition Slash Line -->
                            <line x1="30" y1="30" x2="170" y2="170" stroke="#dc2626" stroke-width="14" stroke-linecap="round" />
                        </svg>
                    </div>
                `,
                options: [
                    {
                        correct: true,
                        audio: "Harikasın! Çimlere basmak yasaktır, doğayı korumalıyız!",
                        failAudio: "Tekrar dene bakalım! Kırmızı çizgi çimlere basılmayacağını söyler.",
                        icon: "🚫",
                        label: "ÇİMLERE BASMA",
                        color: "bg-rose-500 border-rose-700 text-white"
                    },
                    {
                        correct: false,
                        audio: "Bu tabela çimlerde yürümemize izin vermez!",
                        failAudio: "Tekrar dene bakalım!",
                        icon: "🏃‍♀️",
                        label: "ÇİMLERDE YÜRÜ",
                        color: "bg-emerald-500 border-emerald-700 text-white"
                    }
                ]
            },
            {
                // Level 4: Yellow Tactile Paving
                promptAudio: "Yerdeki bu sarı kabartmalı taşlar kimlerin güvenle yürümesi içindir?",
                symbolHtml: `
                    <div class="w-40 h-40 sm:w-52 sm:h-52 bg-amber-400 rounded-3xl border-8 border-amber-500 shadow-2xl grid grid-cols-3 gap-2.5 p-4">
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                        <div class="bg-amber-600 rounded-full shadow-inner"></div>
                    </div>
                `,
                options: [
                    {
                        correct: true,
                        audio: "Tebrikler! Görme engelli arkadaşlarımızın güvenliği içindir!",
                        failAudio: "Tekrar dene bakalım! Onlar özel kılavuz taşlarıdır.",
                        icon: "👨‍🦯",
                        label: "GÖRME ENGELLİLER",
                        color: "bg-indigo-600 border-indigo-800 text-white"
                    },
                    {
                        correct: false,
                        audio: "Sarı taşlar sadece oyun oynamak için değildir!",
                        failAudio: "Tekrar dene bakalım!",
                        icon: "🎮",
                        label: "OYUN OYNAMAK İÇİN",
                        color: "bg-orange-500 border-orange-700 text-white"
                    }
                ]
            }
        ];

        function speakText(text, onComplete = null) {
            if (!speechSynth) {
                if (onComplete) onComplete();
                return;
            }

            // Cancel any ongoing speech
            speechSynth.cancel();

            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'tr-TR';
            utterance.rate = 0.85; // Slower rate tailored for 1st graders
            utterance.pitch = 1.15; // Friendly warm pitch

            const avatar = document.getElementById('ali-avatar-mini');
            if (avatar) avatar.classList.add('talking-pulse');

            utterance.onend = () => {
                if (avatar) avatar.classList.remove('talking-pulse');
                if (onComplete) onComplete();
            };

            utterance.onerror = () => {
                if (avatar) avatar.classList.remove('talking-pulse');
                if (onComplete) onComplete();
            };

            speechSynth.speak(utterance);
        }

        function startGame() {
            currentLevel = 0;
            showScreen('screen-level');
            loadLevel();
            speakText("Merhaba! Ben Ali. Sembol dedektifi olmaya hazır mısın? Soruyu dinle ve doğru resmi seç!");
        }

        function showScreen(screenId) {
            document.getElementById('screen-start').classList.add('hidden');
            document.getElementById('screen-level').classList.add('hidden');
            document.getElementById('screen-victory').classList.add('hidden');

            const active = document.getElementById(screenId);
            active.classList.remove('hidden');
            if (screenId === 'screen-level' || screenId === 'screen-victory') {
                active.style.display = 'flex';
            }
        }

        function updateProgressDots() {
            const container = document.getElementById('progress-dots');
            container.innerHTML = '';
            for (let i = 0; i < gameLevels.length; i++) {
                const dot = document.createElement('div');
                if (i === currentLevel) {
                    dot.className = 'w-5 h-5 sm:w-7 sm:h-7 rounded-full bg-emerald-500 border-4 border-emerald-300 shadow-md scale-125 animate-pulse';
                } else if (i < currentLevel) {
                    dot.className = 'w-4 h-4 sm:w-6 sm:h-6 rounded-full bg-emerald-600 border-2 border-emerald-700 flex items-center justify-center text-white text-xs';
                    dot.innerHTML = '<i class="fa-solid fa-check"></i>';
                } else {
                    dot.className = 'w-4 h-4 sm:w-6 sm:h-6 rounded-full bg-slate-300 border-2 border-slate-400';
                }
                container.appendChild(dot);
            }
        }

        function loadLevel() {
            updateProgressDots();
            const lvl = gameLevels[currentLevel];

            // Render level visual symbol
            document.getElementById('symbol-display').innerHTML = lvl.symbolHtml;

            // Set Voice Hint Text
            document.getElementById('level-voice-hint').innerText = lvl.promptAudio;

            // Render Big Choice Buttons
            const choicesGrid = document.getElementById('choices-grid');
            choicesGrid.innerHTML = '';

            lvl.options.forEach((opt) => {
                const btn = document.createElement('button');
                btn.className = `big-btn flex flex-col items-center justify-center p-4 sm:p-6 rounded-3xl border-b-8 shadow-xl transition-all active:scale-95 ${opt.color}`;
                btn.innerHTML = `
                    <span class="text-6xl sm:text-8xl mb-2">${opt.icon}</span>
                    <span class="text-xl sm:text-3xl font-black text-center tracking-wide">${opt.label}</span>
                `;

                // Single tap handles logic with voice guidance
                btn.onclick = () => handleChoice(opt, btn);
                choicesGrid.appendChild(btn);
            });

            // Speak current level question automatically
            setTimeout(() => {
                speakText(lvl.promptAudio);
            }, 300);
        }

        function repeatQuestion() {
            if (currentLevel < gameLevels.length) {
                speakText(gameLevels[currentLevel].promptAudio);
            }
        }

        function handleChoice(option, btnElement) {
            // Disable all choice buttons temporarily during feedback
            const btns = document.querySelectorAll('#choices-grid button');
            btns.forEach(b => b.disabled = true);

            if (option.correct) {
                playBeep('success');
                btnElement.classList.add('ring-8', 'ring-emerald-300', 'scale-105');
                
                speakText(option.audio, () => {
                    currentLevel++;
                    if (currentLevel < gameLevels.length) {
                        loadLevel();
                    } else {
                        finishGame();
                    }
                });
            } else {
                playBeep('fail');
                btnElement.classList.add('shake-element', 'opacity-60');
                
                speakText(option.failAudio || "Tekrar dene bakalım!", () => {
                    btnElement.classList.remove('shake-element');
                    btns.forEach(b => b.disabled = false);
                });
            }
        }

        function finishGame() {
            updateProgressDots();
            showScreen('screen-victory');

            // Trigger Victory Confetti Animation
            if (typeof confetti === 'function') {
                confetti({
                    particleCount: 180,
                    spread: 100,
                    origin: { y: 0.6 }
                });
            }

            speakText("Tebrikler! Bütün sembolleri bildin! Sen artık resmi bir Sembol Dedektifisin!");
        }

        // Initialize Speech Voices when available
        if (speechSynthesis) {
            speechSynthesis.onvoiceschanged = () => {
                speechSynthesis.getVoices();
            };
        }
    </script>
</body>
</html>
