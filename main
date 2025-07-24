<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prompt Element Sorter</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            /* Dark mode background inspired by the image */
            background: linear-gradient(to bottom right, #1a0033, #0a001a); /* Deep purple to very dark blue/black */
            color: #e2e8f0; /* Light gray for general text */
            line-height: 1.6;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        .container {
            max-width: 1400px;
            flex-grow: 1;
        }
        .card {
            background-color: #2d004d; /* Darker purple for cards */
            border-radius: 0.75rem; /* rounded-xl */
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3); /* More pronounced shadow for dark mode depth */
            border: 1px solid #4a0080; /* Darker purple border */
        }
        .jumbled-statement {
            cursor: grab;
            background-color: #3b0066; /* Slightly lighter dark purple */
            color: #d1d5db; /* Light gray text */
            border: 1px solid #6b21a8; /* Vibrant purple border */
            padding: 0.75rem 1rem;
            border-radius: 0.5rem;
            font-weight: 500;
            margin-bottom: 0.75rem;
            flex-shrink: 0;
            transition: transform 0.2s ease-out, opacity 0.2s ease-out, background-color 0.2s, border-color 0.2s, color 0.2s, box-shadow 0.2s;
            touch-action: none; /* For touch devices */
        }
        .jumbled-statement:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.4);
        }
        .jumbled-statement:active {
            cursor: grabbing;
            transform: translateY(0); /* Reset transform on active drag */
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
        }
        .jumbled-statement.dragged {
            opacity: 0.6;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
            transform: scale(1.02); /* Slightly scale up when dragging */
        }
        .jumbled-statement.placed {
            opacity: 0.9; /* Slightly faded when placed */
            box-shadow: none; /* Remove shadow once placed */
            transform: none; /* Remove any hover transform */
        }
        /* Styles applied after evaluation */
        .jumbled-statement.correct {
            background-color: #0d301a; /* Darker green for correct */
            border-color: #10b981; /* Green-500 */
            color: #a7f3d0; /* Light green text */
        }
        .jumbled-statement.incorrect {
            background-color: #3f0f1f; /* Darker red for incorrect */
            border-color: #ef4444; /* Red-500 */
            color: #fca5a5; /* Light red text */
        }

        .drop-zone-element {
            min-height: 120px; /* Min height for each drop zone */
            border: 2px dashed #6b7280; /* Gray-500 dashed border */
            background-color: #1a0033; /* Slightly lighter than body for contrast */
            border-radius: 0.75rem; /* Consistent with cards */
            padding: 1rem;
            margin-bottom: 1rem;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start; /* Align dropped items to top */
            text-align: center;
            transition: border-color 0.2s, background-color 0.2s;
            position: relative; /* Keep relative for containing elements if needed later */
        }
        .drop-zone-element.active {
            border-color: #8b5cf6; /* Vibrant purple for active */
            background-color: #4c0080; /* Darker purple for active background */
        }
        .drop-zone-placeholder {
            /* Removed absolute positioning */
            color: #9ca3af; /* Light gray placeholder text */
            font-style: italic;
            margin-top: 0.5rem; /* Add some space below the title */
        }

        .btn {
            padding: 0.75rem 1.5rem;
            border-radius: 0.5rem;
            font-weight: 600;
            transition: all 0.2s ease-in-out;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* Standard button shadow for dark mode */
        }
        .btn:hover {
            opacity: 0.9;
            transform: translateY(-2px); /* Lift slightly on hover */
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.3); /* Enhanced shadow on hover */
        }
        .btn:active {
            transform: translateY(0); /* Reset on click */
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
        }
        .btn-primary {
            background-color: #8b5cf6; /* Vibrant purple for primary */
            color: white;
        }
        .btn-secondary {
            background-color: #4a0080; /* Dark purple for secondary */
            color: #d1d5db; /* Light gray text */
        }
        .btn-success {
            background-color: #10b981; /* green-500 */
            color: white;
        }
        .feedback-message {
            padding: 1rem;
            border-radius: 0.5rem;
            margin-top: 1rem;
            font-weight: 600;
            border: 1px solid;
        }
        .feedback-success {
            background-color: #0d301a; /* Darker green */
            color: #a7f3d0; /* Light green */
            border-color: #38a169;
        }
        .feedback-warning {
            background-color: #4d2600; /* Darker orange */
            color: #fdb022; /* Light orange */
            border-color: #ea580c;
        }
        .feedback-error {
            background-color: #3f0f1f; /* Darker red */
            color: #fca5a5; /* Light red */
            border-color: #ef4444;
        }
        .modal {
            display: none; /* Hidden by default */
            position: fixed; /* Stay in place */
            z-index: 1000; /* Sit on top */
            left: 0;
            top: 0;
            width: 100%; /* Full width */
            height: 100%; /* Full height */
            overflow: auto; /* Enable scroll if needed */
            background-color: rgba(0,0,0,0.7); /* Black w/ more opacity for dark mode */
            justify-content: center;
            align-items: center;
            animation: fadeIn 0.3s ease-out; /* Fade in animation */
        }
        .modal-content {
            background-color: #2d004d; /* Dark purple for modal content */
            margin: auto;
            padding: 2.5rem;
            border-radius: 1rem; /* More rounded */
            box-shadow: 0 10px 25px rgba(0,0,0,0.5); /* Stronger shadow */
            width: 90%;
            max-width: 700px;
            color: #e2e8f0; /* Light text */
            animation: scaleIn 0.3s ease-out; /* Scale in animation */
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        @keyframes scaleIn {
            from { transform: translate(-50%, -50%) scale(0.9); opacity: 0; }
            to { transform: translate(-50%, -50%) scale(1); opacity: 1; }
        }
        .modal-content { /* Adjust for new scaleIn animation transform */
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        .close-button {
            color: #9ca3af; /* Light gray close button */
            float: right;
            font-size: 28px;
            font-weight: bold;
            transition: color 0.2s ease;
        }
        .close-button:hover,
        .close-button:focus {
            color: #cbd5e0; /* Lighter gray on hover */
            text-decoration: none;
            cursor: pointer;
        }
    </style>
</head>
<body class="p-6">
    <div class="container mx-auto py-8 flex flex-col items-center">
        <h1 class="text-4xl font-bold text-center text-purple-400 mb-6">Prompt Element Sorter</h1>
        <p class="text-center text-gray-300 mb-8 max-w-3xl">Drag the prompt statements from the right into their correct "Prompt Element" categories on the left. Build a complete and effective prompt!</p>

        <!-- Game Info / Status Bar -->
        <div class="w-full card p-4 mb-8 flex flex-wrap justify-around items-center text-lg font-semibold bg-purple-900 border-purple-700">
            <span id="round-display" class="text-purple-300 text-xl font-bold">Scenario 1</span>
            <!-- Removed Lives Display -->
            <div class="flex items-center gap-2">
                <span class="text-gray-400">⏰ Time:</span>
                <span id="timer-display" class="text-red-400 font-extrabold text-2xl">00:00</span>
            </div>
            <div class="flex items-center gap-2">
                <span class="text-gray-400">🏆 Score:</span>
                <span id="score-display" class="text-green-400 font-extrabold text-2xl">0</span>
            </div>
        </div>

        <!-- Current Scenario (Moved to top) -->
        <div class="w-full card p-6 mb-8">
            <h2 class="text-2xl font-semibold mb-3 text-purple-300">Current Scenario</h2>
            <p id="scenario-text" class="text-gray-200 mb-2"></p>
            <p id="task-prompt-text" class="text-gray-400 text-sm italic"></p>
        </div>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 w-full flex-grow">
            <!-- Left Column: Prompt Element Drop Zones -->
            <div id="prompt-element-dropzones" class="lg:col-span-1 grid grid-cols-1 md:grid-cols-2 gap-4 auto-rows-min">
                <!-- Drop zones will be dynamically rendered here -->
            </div>

            <!-- Right Column: Jumbled Statements & Buttons -->
            <div class="lg:col-span-1 flex flex-col gap-6">
                <div class="card p-6 flex flex-col items-center flex-grow">
                    <h2 class="text-2xl font-semibold mb-4 text-purple-300">Jumbled Prompt Statements</h2>
                    <div id="jumbled-statements-container" class="flex flex-col items-center w-full max-h-96 overflow-y-auto pr-2">
                        <!-- Jumbled statements will be injected here by JS -->
                    </div>
                </div>

                <div class="flex justify-center gap-4 mt-4">
                    <button id="check-prompt-btn" class="btn btn-primary">Check Prompt!</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Debrief Modal -->
    <div id="debrief-modal" class="modal">
        <div class="modal-content">
            <span class="close-button" id="close-debrief-modal">&times;</span>
            <h2 class="text-3xl font-bold text-purple-400 mb-4">Round Results!</h2>
            <h3 class="text-xl font-semibold mb-3 text-gray-300">Score: <span id="modal-round-score" class="text-green-400"></span> points</h3>
            <div id="debrief-feedback-content" class="mb-6 text-gray-300 space-y-3">
                <!-- Feedback on correct/incorrect/missed will go here -->
            </div>
            <div class="flex justify-end mt-4">
                <button id="modal-continue-btn" class="btn-primary px-6 py-2 rounded-lg font-bold">Continue</button>
            </div>
        </div>
    </div>

    <!-- Game Over Modal -->
    <div id="game-over-modal" class="modal">
        <div class="modal-content text-center">
            <h2 class="text-4xl font-bold text-purple-400 mb-4" id="game-over-title">Game Over!</h2>
            <p class="text-xl text-gray-300 mb-2">Your Final Score: <span id="final-score" class="text-green-400 font-bold">0</span></p>
            <div class="flex justify-center mt-6">
                <button id="restart-game-btn" class="btn btn-primary px-6 py-3 text-xl">Play Again!</button>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, onSnapshot, setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Set Firebase logging level to Debug
        setLogLevel('Debug');

        const promptElementsData = [
            { id: 'context', name: 'Contextual Clarity', placeholder: 'Background & Scenario' },
            { id: 'audience', name: 'Audience & Persona', placeholder: 'Who is the output for? AI\'s role?' },
            { id: 'purpose', name: 'Purpose & Goal', placeholder: 'Why are we doing this? Main objective?' },
            { id: 'constraints', name: 'Constraints & Limitations', placeholder: 'What to avoid/include (e.g., bias, length)?' },
            { id: 'format', name: 'Format & Structure', placeholder: 'How should the output look?' },
            { id: 'tone', name: 'Tone & Style', placeholder: 'How should the output sound?' },
            { id: 'evaluation', name: 'Evaluation Criteria', placeholder: 'How will success be judged?' }
        ];

        const scenarios = [
            {
                id: 1,
                scenario: "You're building an AI assistant to summarize user feedback for a new mobile app feature. The app allows users to easily track their daily water intake.",
                taskPrompt: "Your goal is to get a summary focusing on user experience, not just bug reports. The summary needs to be concise for busy product managers.",
                timerSeconds: 90,
                jumbledStatements: [
                    { id: 's1-1', text: 'The AI should act as a neutral data analyst.', correctElementId: 'audience' },
                    { id: 's1-2', text: 'Analyze reviews from users aged 25-45 who use fitness trackers.', correctElementId: 'audience' },
                    { id: 's1-3', text: 'The summary should be presented in 3-5 bullet points.', correctElementId: 'format' },
                    { id: 's1-4', text: 'Identify positive and negative sentiment regarding usability.', correctElementId: 'evaluation' },
                    { id: 's1-5', text: 'Do not include any suggestions for new features.', correctElementId: 'constraints' },
                    { id: 's1-6', text: 'Focus on how the feature integrates into daily routines.', correctElementId: 'context' },
                    { id: 's1-7', text: 'The main goal is to identify core UX pain points and delights.', correctElementId: 'purpose' },
                ],
            },
            {
                id: 2,
                scenario: "Your team is drafting user persona archetypes for a new mental wellness app. The app aims to support diverse needs, and it's crucial to avoid any pre-existing stereotypes.",
                taskPrompt: "Create 3 distinct user persona archetypes for a mental wellness app.",
                timerSeconds: 120,
                jumbledStatements: [
                    { id: 's2-1', text: 'Ensure personas are gender-neutral and avoid ageism.', correctElementId: 'constraints' },
                    { id: 's2-2', text: 'Focus on motivations, pain points, and goals related to mental health.', correctElementId: 'purpose' },
                    { id: 's2-3', text: 'The output is for UX designers and content creators.', correctElementId: 'audience' },
                    { id: 's2-4', text: 'Each persona should include a short narrative and 3 key characteristics.', correctElementId: 'format' },
                    { id: 's2-5', text: 'Consider users seeking support for stress, anxiety, or general well-being.', correctElementId: 'context' },
                    { id: 's2-6', text: 'The tone should be empathetic and non-judgmental.', correctElementId: 'tone' },
                    { id: 's2-7', text: 'Success means personas accurately reflect diverse user needs.', correctElementId: 'evaluation' },
                ],
            },
            {
                id: 3,
                scenario: "You need to generate ideas for **UI element variations** for a 'confirm purchase' button. The key challenge is to ensure these variations consider **accessibility for users with cognitive impairments**.",
                taskPrompt: "Suggest 4 distinct UI element variations for a 'confirm purchase' button.",
                timerSeconds: 100,
                jumbledStatements: [
                    { id: 's3-1', text: 'The suggestions are for a web development team.', correctElementId: 'audience' },
                    { id: 's3-2', text: 'Focus on clear, unambiguous language and reduced cognitive load.', correctElementId: 'constraints' },
                    { id: 's3-3', text: 'The goal is to prevent accidental purchases by easily confused users.', correctElementId: 'purpose' },
                    { id: 's3-4', text: 'Provide a brief explanation for each variation.', correctElementId: 'format' },
                    { id: 's3-5', text: 'Ensure options for varied visual contrast and simplified interactions.', correctElementId: 'evaluation' },
                    { id: 's3-6', text: 'Consider users who might struggle with complex interfaces or multiple options.', correctElementId: 'context' },
                    { id: 's3-7', text: 'The tone should be helpful and solution-oriented.', correctElementId: 'tone' },
                ]
            }
        ];

        // Firebase variables (kept for potential future use, but not actively used for high score/user ID display)
        let app;
        let db;
        let auth;
        let userId = null; // User ID is still internally available from Firebase auth

        // Game state variables
        let currentScenarioCount = 0; // Tracks how many scenarios have been played this game
        let totalScore = 0;
        // Removed: let currentLives = 3;

        let timerId;
        let timeRemaining;

        // Add a new array to hold scenarios for the current game instance, to enable random selection without repetition
        let playableScenarios = [];
        const originalScenarios = [...scenarios]; // Keep a copy of the original scenarios data

        // HTML Element references
        const promptElementDropzones = document.getElementById('prompt-element-dropzones');
        const scenarioText = document.getElementById('scenario-text');
        const taskPromptText = document.getElementById('task-prompt-text');
        const jumbledStatementsContainer = document.getElementById('jumbled-statements-container');
        const timerDisplay = document.getElementById('timer-display');
        const scoreDisplay = document.getElementById('score-display');
        // Removed: const livesDisplay = document.getElementById('lives-display');
        const roundDisplay = document.getElementById('round-display');
        const checkPromptBtn = document.getElementById('check-prompt-btn');

        // Debrief Modal elements
        const debriefModal = document.getElementById('debrief-modal');
        const closeDebriefModalBtn = document.getElementById('close-debrief-modal');
        const modalRoundScore = document.getElementById('modal-round-score');
        const debriefFeedbackContent = document.getElementById('debrief-feedback-content');
        const modalContinueBtn = document.getElementById('modal-continue-btn');

        // Game Over Modal elements
        const gameOverModal = document.getElementById('game-over-modal');
        const gameOverTitle = document.getElementById('game-over-title');
        const finalScoreDisplay = document.getElementById('final-score');
        const restartGameBtn = document.getElementById('restart-game-btn');

        // Internal state to track what's where
        // Map<statementId (string), currentElementId (string, or 'jumbled-statements-container')>
        let statementCurrentLocations = new Map();
        let currentJumbledStatementsData = []; // Full statement objects for current scenario

        // --- Firebase Setup (simplified as high score/user ID display removed) ---
        async function setupFirebaseAndAuth() {
            try {
                // Safely get __app_id and __firebase_config
                const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
                const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;

                if (!firebaseConfig) {
                    console.warn("Firebase config not available. Skipping Firebase initialization.");
                    return;
                }

                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);

                // Authenticate
                onAuthStateChanged(auth, (user) => {
                    if (user) {
                        userId = user.uid;
                        // console.log("Firebase Authenticated. User ID:", userId); // Removed console.log as user ID is not displayed
                    } else {
                        userId = crypto.randomUUID(); // Fallback for unauthenticated or anonymous
                        // console.log("Firebase Not Authenticated, using anonymous user ID:", userId); // Removed console.log
                    }
                });

                if (typeof __initial_auth_token !== 'undefined') {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }

            } catch (error) {
                console.error("Error setting up Firebase:", error);
            }
        }

        // --- Game Initialization ---
        function initGame() {
            renderPromptElementDropzones(); // Render static drop zones once
            addGlobalDragListeners(); // Attach drag-and-drop event listeners
            checkPromptBtn.addEventListener('click', endScenario);

            // Debrief modal button listeners
            closeDebriefModalBtn.addEventListener('click', () => debriefModal.style.display = 'none');
            modalContinueBtn.addEventListener('click', () => {
                debriefModal.style.display = 'none';
                loadNextScenario(); // Continue to next scenario or end game
            });

            // Game Over modal button listener
            restartGameBtn.addEventListener('click', () => {
                gameOverModal.style.display = 'none';
                resetGame();
                loadNextScenario(); // Start fresh
            });

            setupFirebaseAndAuth(); // Initialize Firebase and auth
            resetGame(); // Set initial game state (shuffles scenarios)
            loadNextScenario(); // Start the first scenario
        }

        // --- Render UI Elements ---

        function renderPromptElementDropzones() {
            promptElementDropzones.innerHTML = '';
            promptElementsData.forEach(element => {
                const div = document.createElement('div');
                div.id = `drop-zone-${element.id}`;
                div.className = 'drop-zone-element card p-4';
                div.setAttribute('data-element-id', element.id); // Custom attribute for element ID
                // Added text-center to the title for consistency
                div.innerHTML = `
                    <h4 class="font-bold text-purple-300 text-lg mb-2 text-center">${element.name}</h4>
                    <p class="drop-zone-placeholder text-sm">${element.placeholder}</p>
                `;
                promptElementDropzones.appendChild(div);
            });
        }

        function renderJumbledStatements(statements) {
            jumbledStatementsContainer.innerHTML = '';
            statementCurrentLocations.clear(); // Clear tracking for new round

            statements.forEach(statement => {
                const div = document.createElement('div');
                div.id = `statement-${statement.id}`;
                div.className = 'jumbled-statement w-11/12';
                div.draggable = true;
                div.textContent = statement.text;
                div.setAttribute('data-statement-id', statement.id);
                div.setAttribute('data-correct-element-id', statement.correctElementId);
                jumbledStatementsContainer.appendChild(div);
                statementCurrentLocations.set(statement.id, 'jumbled-statements-container'); // Initially all are in the jumbled container
            });
        }

        function updateScoreDisplay() {
            scoreDisplay.textContent = totalScore;
        }

        // Removed updateLivesDisplay function
        /*
        function updateLivesDisplay() {
            livesDisplay.textContent = currentLives;
            if (currentLives <= 1) {
                livesDisplay.classList.add('text-red-400');
            } else {
                livesDisplay.classList.remove('text-red-400');
            }
        }
        */

        function updateTimerDisplay() {
            const minutes = Math.floor(timeRemaining / 60);
            const seconds = timeRemaining % 60;
            timerDisplay.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        function resetDropzones() {
            promptElementDropzones.querySelectorAll('.drop-zone-element').forEach(dropZone => {
                // Remove all child elements that are jumbled statements
                Array.from(dropZone.children).forEach(child => {
                    if (child.classList.contains('jumbled-statement')) {
                        child.remove();
                    }
                });
                dropZone.classList.remove('active');
                // Ensure placeholder is visible for empty dropzones
                dropZone.querySelector('.drop-zone-placeholder').classList.remove('hidden');
            });
        }

        function resetJumbledStatementsContainer() {
            jumbledStatementsContainer.innerHTML = ''; // Clear all statements from the jumbled container
            jumbledStatementsContainer.style.outline = ''; // Remove any active outline
        }

        // --- Game Flow Control ---

        function resetGame() {
            currentScenarioCount = 0; // Reset for round display
            totalScore = 0;
            // Removed: currentLives = 3; // Lives are no longer a feature
            updateScoreDisplay();
            // Removed updateLivesDisplay();
            gameOverModal.style.display = 'none';

            // Populate and shuffle playable scenarios for a new game
            playableScenarios = [...originalScenarios]; // Reset with all scenarios
            // Fisher-Yates shuffle algorithm
            for (let i = playableScenarios.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [playableScenarios[i], playableScenarios[j]] = [playableScenarios[j], playableScenarios[i]];
            }
        }

        function loadNextScenario() {
            stopTimer();

            if (playableScenarios.length === 0) {
                endGame(true); // All scenarios completed
                return;
            }

            // Get the next scenario from the shuffled list
            const scenario = playableScenarios.pop(); // Remove the last element for efficiency

            currentScenarioCount++; // Increment for display purposes (Scenario 1, Scenario 2, etc.)
            // Clone statements to ensure clean state for each round
            currentJumbledStatementsData = scenario.jumbledStatements.map(stmt => ({ ...stmt }));

            resetDropzones();
            resetJumbledStatementsContainer(); // Clear old statements from the jumbled container

            scenarioText.textContent = scenario.scenario;
            taskPromptText.textContent = scenario.taskPrompt;
            roundDisplay.textContent = `Scenario ${currentScenarioCount}/${originalScenarios.length}`; // Display X/Total scenarios

            renderJumbledStatements(currentJumbledStatementsData); // Populate with new statements

            checkPromptBtn.classList.remove('hidden');

            timeRemaining = scenario.timerSeconds;
            updateTimerDisplay();
            startTimer();
        }

        function startTimer() {
            timerId = setInterval(() => {
                timeRemaining--;
                updateTimerDisplay();
                if (timeRemaining <= 0) {
                    clearInterval(timerId);
                    endScenario();
                }
            }, 1000);
        }

        function stopTimer() {
            if (timerId) {
                clearInterval(timerId);
                timerId = null;
            }
        }

        function endScenario() {
            stopTimer();
            checkPromptBtn.classList.add('hidden'); // Hide check button

            evaluateScenario(); // This updates score

            // Game now always shows debrief and then proceeds or ends if all scenarios are done.
            // Lives are no longer a game-over condition here.
            showDebriefModal();
        }

        function endGame(completedAllScenarios) {
            stopTimer();
            gameOverModal.style.display = 'flex'; // Show game over modal

            if (completedAllScenarios) {
                gameOverTitle.textContent = "Congratulations! You completed all scenarios!";
            } else {
                gameOverTitle.textContent = "Time's Up!"; // Changed message as lives are removed
                // If it wasn't all scenarios completed, it means time ran out or user clicked check.
                // We'll just say "Game Over!" or "Time's Up!" if not all scenarios are done.
            }

            finalScoreDisplay.textContent = totalScore;
            updateScoreDisplay(); // Update top bar score as well
        }

        // --- Drag & Drop Logic ---

        function addGlobalDragListeners() {
            let draggedStatementId = null;

            document.addEventListener('dragstart', (e) => {
                if (e.target.classList.contains('jumbled-statement')) {
                    draggedStatementId = e.target.dataset.statementId;
                    e.dataTransfer.setData('text/plain', draggedStatementId);
                    e.target.classList.add('dragged');
                }
            });

            document.addEventListener('dragend', (e) => {
                if (e.target.classList.contains('jumbled-statement')) {
                    e.target.classList.remove('dragged');
                }
                draggedStatementId = null;
            });

            // Make all drop zones, including the initial jumbled statements container, valid drop targets
            document.querySelectorAll('.drop-zone-element, #jumbled-statements-container').forEach(dropTarget => {
                dropTarget.addEventListener('dragover', (e) => {
                    e.preventDefault(); // Allow drop
                    if (dropTarget.classList.contains('drop-zone-element')) {
                        dropTarget.classList.add('active'); // Visual cue for drop zone
                    } else if (dropTarget.id === 'jumbled-statements-container') {
                        // Optional: Add visual cue for jumbled container
                        dropTarget.style.outline = '2px dashed #8b5cf6'; /* Vibrant purple outline for jumbled container */
                    }
                });

                dropTarget.addEventListener('dragleave', () => {
                    if (dropTarget.classList.contains('drop-zone-element')) {
                        dropTarget.classList.remove('active');
                    } else if (dropTarget.id === 'jumbled-statements-container') {
                        dropTarget.style.outline = '';
                    }
                });

                dropTarget.addEventListener('drop', (e) => {
                    e.preventDefault();
                    if (dropTarget.classList.contains('drop-zone-element')) {
                        dropTarget.classList.remove('active');
                    } else if (dropTarget.id === 'jumbled-statements-container') {
                        dropTarget.style.outline = '';
                    }

                    const droppedId = e.dataTransfer.getData('text/plain');
                    const droppedElement = document.getElementById(`statement-${droppedId}`);

                    if (droppedElement) {
                        // Remove previous correct/incorrect styling when moved
                        droppedElement.classList.remove('correct', 'incorrect');

                        // Update internal tracking BEFORE appending, based on where it *was*
                        const previousLocationId = statementCurrentLocations.get(droppedId);
                        if (previousLocationId && previousLocationId !== dropTarget.id) { // If it's actually moving
                            // Hide placeholder for the old empty dropzone if it was one
                            if (previousLocationId !== 'jumbled-statements-container') {
                                const oldDropZone = document.getElementById(`drop-zone-${previousLocationId}`);
                                // Check if oldDropZone exists and if it's now empty after this move
                                // (This condition checks if there was only one statement in the old dropzone)
                                if (oldDropZone && Array.from(oldDropZone.children).filter(c => c.classList.contains('jumbled-statement')).length === 1) {
                                    oldDropZone.querySelector('.drop-zone-placeholder').classList.remove('hidden');
                                }
                            }
                        }

                        // Append the element to the new target
                        dropTarget.appendChild(droppedElement);

                        // Update internal tracking for the new location
                        statementCurrentLocations.set(droppedId, dropTarget.dataset.elementId || dropTarget.id);

                        // Hide placeholder for the new dropzone if it's not the jumbled container
                        if (dropTarget.classList.contains('drop-zone-element')) {
                            dropTarget.querySelector('.drop-zone-placeholder').classList.add('hidden');
                        }
                    }
                });
            });
        }

        // --- Evaluation Logic ---

        function evaluateScenario() {
            let roundScore = 0;
            // Removed: let livesLostThisRound = 0;
            debriefFeedbackContent.innerHTML = '';

            const correctStatementsCount = new Set();
            const incorrectStatementsDetails = []; // To store {stmt, droppedIntoElement, correctElement}

            currentJumbledStatementsData.forEach(stmt => {
                const statementElement = document.getElementById(`statement-${stmt.id}`);
                statementElement.classList.remove('correct', 'incorrect'); // Reset visual state

                const currentParentId = statementCurrentLocations.get(stmt.id);
                const isPlacedInDropZone = promptElementsData.some(el => el.id === currentParentId);

                if (isPlacedInDropZone) {
                    if (currentParentId === stmt.correctElementId) {
                        correctStatementsCount.add(stmt.id);
                        statementElement.classList.add('correct');
                        roundScore += 20; // Points for correct placement
                    } else {
                        incorrectStatementsDetails.push({
                            stmt: stmt,
                            droppedIntoElementId: currentParentId,
                            correctElementId: stmt.correctElementId
                        });
                        statementElement.classList.add('incorrect');
                        roundScore -= 10; // Penalty for incorrect placement
                        // Removed: livesLostThisRound++;
                    }
                } else { // Statement is still in jumbled-statements-container or otherwise unplaced
                    incorrectStatementsDetails.push({
                        stmt: stmt,
                        droppedIntoElementId: 'jumbled-statements-container', // Special ID for unplaced
                        correctElementId: stmt.correctElementId
                    });
                    statementElement.classList.add('incorrect');
                    roundScore -= 5; // Penalty for unplaced
                    // Removed: livesLostThisRound++;
                }
            });

            // Removed lives update
            // currentLives = Math.max(0, currentLives - livesLostThisRound);
            // updateLivesDisplay();

            // Debrief Modal Feedback
            // Correctly Placed
            if (correctStatementsCount.size > 0) {
                const div = document.createElement('div');
                div.className = 'bg-green-50 p-3 rounded-md text-green-800 border border-green-200'; /* Adjusted for dark mode */
                div.innerHTML = `<p class="font-bold">✅ Correctly Placed (${correctStatementsCount.size}/${currentJumbledStatementsData.length}):</p>`;
                correctStatementsCount.forEach(id => {
                    const stmt = currentJumbledStatementsData.find(s => s.id === id);
                    const element = promptElementsData.find(e => e.id === stmt.correctElementId);
                    div.innerHTML += `<p class="text-sm ml-2">- "${stmt.text}" was correctly placed in "${element.name}".</p>`;
                });
                debriefFeedbackContent.appendChild(div);
            }

            // Incorrect/Missed Placements
            if (incorrectStatementsDetails.length > 0) {
                const div = document.createElement('div');
                div.className = 'bg-red-50 p-3 rounded-md text-red-800 border border-red-200'; /* Adjusted for dark mode */
                div.innerHTML = `<p class="font-bold">❌ Incorrect / Missed Placements (${incorrectStatementsDetails.length}):</p>`;
                incorrectStatementsDetails.forEach(detail => {
                    const stmt = detail.stmt;
                    const correctElement = promptElementsData.find(e => e.id === detail.correctElementId);
                    let message;
                    if (detail.droppedIntoElementId === 'jumbled-statements-container') {
                        message = `- "${stmt.text}" was not placed. It belongs in "${correctElement.name}". (-5 pts)`; // Removed lives mention
                    } else {
                        const droppedIntoElement = promptElementsData.find(e => e.id === detail.droppedIntoElementId);
                        message = `- "${stmt.text}" was placed in "${droppedIntoElement.name}", but it belongs in "${correctElement.name}". (-10 pts)`; // Removed lives mention
                    }
                    div.innerHTML += `<p class="text-sm ml-2">${message}</p>`;
                });
                debriefFeedbackContent.appendChild(div);
            }

            // Completion Bonus
            if (correctStatementsCount.size === currentJumbledStatementsData.length) {
                const div = document.createElement('div');
                div.className = 'bg-purple-900 p-3 rounded-md text-purple-300 border border-purple-700 mt-4'; /* Adjusted for dark mode */
                div.innerHTML = `<p class="font-bold">✨ Completion Bonus: +50 points for correctly categorizing all statements!</p>`;
                debriefFeedbackContent.appendChild(div);
            }

            // Time Bonus
            if (timeRemaining > 0) {
                const timeBonus = Math.floor(timeRemaining / 5); // 1 point for every 5 seconds remaining
                roundScore += timeBonus;
                const div = document.createElement('div');
                div.className = 'bg-blue-900 p-3 rounded-md text-blue-300 border border-blue-700 mt-4'; /* Adjusted for dark mode */
                div.innerHTML = `<p class="font-bold">⏱️ Time Bonus: +${timeBonus} points for ${timeRemaining} seconds remaining!</p>`;
                debriefFeedbackContent.appendChild(div);
            }

            roundScore = Math.max(0, roundScore); // Ensure score doesn't go negative for the round
            totalScore += roundScore;
            updateScoreDisplay();
            modalRoundScore.textContent = roundScore;
        }

        function showDebriefModal() {
            debriefModal.style.display = 'flex'; // Use flex to center
        }

        // Initialize the game when the DOM is fully loaded
        document.addEventListener('DOMContentLoaded', initGame);
    </script>
</body>
</html>
```
