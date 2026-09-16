# my-first-web-app
This is my first web app that will be a github page.
<!DOCTYPE html>
<html lang="en" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Counter App</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    animation: {
                        'bounce-short': 'bounceShort 0.2s ease-in-out',
                        'pulse-glow': 'pulseGlow 1.5s infinite alternate',
                    },
                    keyframes: {
                        bounceShort: {
                            '0%, 100%': { transform: 'scale(1)' },
                            '50%': { transform: 'scale(1.08)' },
                        },
                        pulseGlow: {
                            '0%': { boxShadow: '0 0 15px rgba(99, 102, 241, 0.2)' },
                            '100%': { boxShadow: '0 0 30px rgba(99, 102, 241, 0.6)' }
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            touch-action: manipulation;
        }
        /* Hide spin buttons for number inputs */
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        input[type=number] {
            -moz-appearance: textfield;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 h-full flex flex-col justify-between select-none antialiased transition-colors duration-300">

    <!-- Top Bar Navigation -->
    <header class="w-full max-w-xl mx-auto px-6 pt-6 flex justify-between items-center">
        <div class="flex items-center space-x-2">
            <div class="w-8 h-8 rounded-lg bg-indigo-600 flex items-center justify-center font-bold text-white shadow-lg shadow-indigo-500/30">
                #
            </div>
            <h1 class="font-bold text-lg tracking-tight text-white">Counter</h1>
        </div>

        <div class="flex items-center space-x-2">
            <!-- Audio Toggle Button -->
            <button id="soundToggle" class="p-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-slate-300 hover:text-white transition shadow-sm border border-slate-700/50" title="Toggle Sound">
                <svg id="soundOnIcon" class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"></path>
                </svg>
                <svg id="soundOffIcon" class="w-5 h-5 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15zM17 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2"></path>
                </svg>
            </button>

            <!-- Reset Counter Button -->
            <button id="resetBtn" class="p-2.5 rounded-xl bg-slate-800 hover:bg-rose-950/50 text-slate-300 hover:text-rose-400 border border-slate-700/50 hover:border-rose-800/50 transition flex items-center space-x-1" title="Reset Counter (R)">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
                </svg>
            </button>
        </div>
    </header>

    <!-- Main Interactive Card -->
    <main class="w-full max-w-md mx-auto px-4 my-auto flex flex-col items-center">
        <div class="w-full bg-slate-800/80 backdrop-blur-xl border border-slate-700/60 rounded-3xl p-8 shadow-2xl flex flex-col items-center transition-all duration-200">
            
            <!-- Target Progress (Optional Indicator) -->
            <div id="targetBarContainer" class="w-full mb-4 hidden">
                <div class="flex justify-between text-xs text-slate-400 mb-1.5 font-medium">
                    <span>Goal: <span id="targetGoalLabel">100</span></span>
                    <span id="targetPercentLabel">0%</span>
                </div>
                <div class="w-full h-2 bg-slate-700 rounded-full overflow-hidden">
                    <div id="targetProgressBar" class="h-full bg-gradient-to-r from-indigo-500 to-emerald-400 transition-all duration-300 w-0"></div>
                </div>
            </div>

            <!-- Big Counter Value Number -->
            <div class="py-6 my-2 text-center relative w-full overflow-hidden">
                <span id="countDisplay" class="text-7xl sm:text-8xl font-black tracking-tight text-transparent bg-clip-text bg-gradient-to-b from-white to-slate-300 inline-block transition-transform duration-150">
                    0
                </span>
            </div>

            <!-- Main Controls (- / + Buttons) -->
            <div class="grid grid-cols-2 gap-4 w-full mt-4">
                <button id="decrementBtn" class="group relative flex items-center justify-center py-6 px-4 bg-slate-700/70 hover:bg-slate-700 active:bg-slate-600 active:scale-[0.97] rounded-2xl border border-slate-600/50 shadow-lg text-slate-200 font-semibold text-3xl transition-all duration-150 outline-none focus:ring-2 focus:ring-rose-500/50">
                    <span class="group-active:scale-125 transition-transform">-</span>
                </button>

                <button id="incrementBtn" class="group relative flex items-center justify-center py-6 px-4 bg-indigo-600 hover:bg-indigo-500 active:bg-indigo-700 active:scale-[0.97] rounded-2xl border border-indigo-400/30 shadow-lg shadow-indigo-600/30 text-white font-semibold text-3xl transition-all duration-150 outline-none focus:ring-2 focus:ring-indigo-400">
                    <span class="group-active:scale-125 transition-transform">+</span>
                </button>
            </div>

            <!-- Step Size Selector -->
            <div class="w-full mt-6 pt-6 border-t border-slate-700/60 flex items-center justify-between">
                <label for="stepInput" class="text-xs font-semibold text-slate-400 uppercase tracking-wider">
                    Step Amount
                </label>
                <div class="flex items-center space-x-1 bg-slate-900/60 p-1 rounded-xl border border-slate-700/80">
                    <button class="step-preset px-3 py-1 text-xs font-medium rounded-lg text-slate-300 hover:bg-slate-700 transition" data-step="1">1</button>
                    <button class="step-preset px-3 py-1 text-xs font-medium rounded-lg text-slate-300 hover:bg-slate-700 transition" data-step="5">5</button>
                    <button class="step-preset px-3 py-1 text-xs font-medium rounded-lg text-slate-300 hover:bg-slate-700 transition" data-step="10">10</button>
                    <input type="number" id="stepInput" value="1" min="1" max="1000" class="w-12 bg-slate-800 text-center text-xs font-bold text-indigo-300 rounded-lg py-1 border border-indigo-500/30 focus:outline-none focus:border-indigo-400" />
                </div>
            </div>
        </div>

        <!-- Notification Toast -->
        <div id="toast" class="opacity-0 translate-y-2 transition-all duration-200 mt-4 px-4 py-2 bg-slate-800/90 border border-slate-700 text-xs font-medium text-slate-300 rounded-full shadow-lg pointer-events-none">
            Reset complete
        </div>
    </main>

    <!-- Footer / Keyboard hint -->
    <footer class="w-full max-w-xl mx-auto px-6 pb-6 text-center">
        <div class="flex items-center justify-center space-x-4 text-xs text-slate-500">
            <span class="flex items-center gap-1"><kbd class="px-1.5 py-0.5 bg-slate-800 rounded border border-slate-700 text-slate-400">Space</kbd> / <kbd class="px-1.5 py-0.5 bg-slate-800 rounded border border-slate-700 text-slate-400">↑</kbd> Inc</span>
            <span>•</span>
            <span class="flex items-center gap-1"><kbd class="px-1.5 py-0.5 bg-slate-800 rounded border border-slate-700 text-slate-400">↓</kbd> Dec</span>
            <span>•</span>
            <span class="flex items-center gap-1"><kbd class="px-1.5 py-0.5 bg-slate-800 rounded border border-slate-700 text-slate-400">R</kbd> Reset</span>
        </div>
    </footer>

    <script>
        // Application State
        let count = 0;
        let step = 1;
        let soundEnabled = true;

        // Load saved state from LocalStorage
        try {
            const savedCount = localStorage.getItem('app_counter_value');
            if (savedCount !== null) count = parseInt(savedCount, 10) || 0;

            const savedStep = localStorage.getItem('app_counter_step');
            if (savedStep !== null) step = parseInt(savedStep, 10) || 1;

            const savedSound = localStorage.getItem('app_counter_sound');
            if (savedSound !== null) soundEnabled = savedSound === 'true';
        } catch (e) {
            console.warn('LocalStorage unavailable:', e);
        }

        // DOM Elements
        const countDisplay = document.getElementById('countDisplay');
        const incrementBtn = document.getElementById('incrementBtn');
        const decrementBtn = document.getElementById('decrementBtn');
        const resetBtn = document.getElementById('resetBtn');
        const stepInput = document.getElementById('stepInput');
        const stepPresetBtns = document.querySelectorAll('.step-preset');
        const soundToggle = document.getElementById('soundToggle');
        const soundOnIcon = document.getElementById('soundOnIcon');
        const soundOffIcon = document.getElementById('soundOffIcon');
        const toast = document.getElementById('toast');

        // Web Audio API Sound Generator (No external assets required)
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx = null;

        function playTone(freq, duration = 0.08, type = 'sine') {
            if (!soundEnabled) return;
            try {
                if (!audioCtx) audioCtx = new AudioContext();
                if (audioCtx.state === 'suspended') audioCtx.resume();

                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();

                osc.type = type;
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime);

                gain.gain.setValueAtTime(0.15, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);

                osc.connect(gain);
                gain.connect(audioCtx.destination);

                osc.start();
                osc.stop(audioCtx.currentTime + duration);
            } catch (e) {
                // Ignore audio errors on unsupported browsers
            }
        }

        // UI Render Function
        function updateUI(animated = true) {
            countDisplay.textContent = count.toLocaleString();

            // Animate number popup
            if (animated) {
                countDisplay.classList.remove('animate-bounce-short');
                void countDisplay.offsetWidth; // Trigger reflow
                countDisplay.classList.add('animate-bounce-short');
            }

            // Sync step controls
            stepInput.value = step;
            stepPresetBtns.forEach(btn => {
                const btnStep = parseInt(btn.dataset.step, 10);
                if (btnStep === step) {
                    btn.classList.add('bg-indigo-600', 'text-white');
                    btn.classList.remove('bg-slate-700', 'text-slate-300');
                } else {
                    btn.classList.remove('bg-indigo-600', 'text-white');
                    btn.classList.add('text-slate-300');
                }
            });

            // Sync Sound Icon
            if (soundEnabled) {
                soundOnIcon.classList.remove('hidden');
                soundOffIcon.classList.add('hidden');
            } else {
                soundOnIcon.classList.add('hidden');
                soundOffIcon.classList.remove('hidden');
            }

            // Save state
            try {
                localStorage.setItem('app_counter_value', count);
                localStorage.setItem('app_counter_step', step);
                localStorage.setItem('app_counter_sound', soundEnabled);
            } catch (e) {}
        }

        // Actions
        function increment() {
            count += step;
            playTone(440 + Math.min(count * 5, 400), 0.08, 'sine');
            updateUI(true);
        }

        function decrement() {
            count -= step;
            playTone(320, 0.08, 'triangle');
            updateUI(true);
        }

        function reset() {
            if (count === 0) return;
            count = 0;
            playTone(220, 0.15, 'sawtooth');
            updateUI(true);
            showToast('Counter reset to 0');
        }

        function setStep(newStep) {
            step = Math.max(1, parseInt(newStep, 10) || 1);
            updateUI(false);
        }

        function showToast(message) {
            toast.textContent = message;
            toast.classList.remove('opacity-0', 'translate-y-2');
            toast.classList.add('opacity-100', 'translate-y-0');
            setTimeout(() => {
                toast.classList.remove('opacity-100', 'translate-y-0');
                toast.classList.add('opacity-0', 'translate-y-2');
            }, 1800);
        }

        // Event Listeners
        incrementBtn.addEventListener('click', increment);
        decrementBtn.addEventListener('click', decrement);
        resetBtn.addEventListener('click', reset);

        stepInput.addEventListener('change', (e) => setStep(e.target.value));
        stepInput.addEventListener('keyup', (e) => setStep(e.target.value));

        stepPresetBtns.forEach(btn => {
            btn.addEventListener('click', () => {
                setStep(btn.dataset.step);
            });
        });

        soundToggle.addEventListener('click', () => {
            soundEnabled = !soundEnabled;
            updateUI(false);
            showToast(soundEnabled ? 'Sound Enabled' : 'Sound Muted');
        });

        // Keyboard Navigation Shortcuts
        window.addEventListener('keydown', (e) => {
            // Avoid triggering when user is editing the step input field
            if (document.activeElement === stepInput) return;

            if (e.code === 'Space' || e.code === 'ArrowUp') {
                e.preventDefault();
                increment();
            } else if (e.code === 'ArrowDown') {
                e.preventDefault();
                decrement();
            } else if (e.code === 'KeyR') {
                e.preventDefault();
                reset();
            }
        });

        // Initial Render
        updateUI(false);
    </script>
</body>
</html>
