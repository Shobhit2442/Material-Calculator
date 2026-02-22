<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Construction Material Calculator</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            align-items: center;
            min-height: 100vh;
            background-color: #1a202c;
            padding: 1.5rem;
            box-sizing: border-box;
            color: #e2e8f0;
        }
        .container {
            background-color: #2d3748;
            border-radius: 1.5rem;
            padding: 2rem;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5);
            width: 100%;
            max-width: 600px;
            border: 2px solid #4a5568;
            position: relative;
            margin-bottom: 1.5rem;
        }
        h1 {
            font-size: 2.5rem;
            font-weight: bold;
            color: #00ffff;
            text-align: center;
            margin-bottom: 2rem;
        }
        .global-settings {
            background-color: #3a475a;
            border-radius: 1rem;
            padding: 1rem 1.5rem;
            margin-bottom: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }
        .global-settings label {
            font-size: 1rem;
            color: #cbd5e0;
            margin-right: 0.5rem;
        }
        .global-settings input,
        .global-settings select {
            background-color: #4a5568;
            border: 1px solid #6b7280;
            border-radius: 0.5rem;
            padding: 0.5rem;
            font-size: 1rem;
            color: #e2e8f0;
            flex-grow: 1;
        }

        .section-selector {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            justify-content: center;
            margin-bottom: 2rem;
        }
        .section-btn {
            background-color: #4299e1;
            color: white;
            font-size: 1.1rem;
            padding: 0.75rem 1.5rem;
            border-radius: 0.75rem;
            cursor: pointer;
            transition: background-color 0.2s ease-in-out, transform 0.1s ease-in-out;
            border: none;
            outline: none;
            flex-grow: 1;
            max-width: 180px;
            text-align: center;
        }
        .section-btn:hover {
            background-color: #3182ce;
            transform: translateY(-2px);
        }
        .section-btn.active {
            background-color: #38a169;
            box-shadow: 0 0 10px rgba(0, 255, 0, 0.5);
        }

        .calculator-section {
            display: none;
            flex-direction: column;
            gap: 1.5rem;
            padding: 1.5rem;
            background-color: #3a475a;
            border-radius: 1rem;
            margin-top: 1.5rem;
        }
        .calculator-section.active {
            display: flex;
        }
        .calculator-section h2 {
            font-size: 1.8rem;
            font-weight: bold;
            color: #f6ad55;
            margin-bottom: 1rem;
            text-align: center;
        }
        .input-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }
        .input-group label {
            font-size: 1rem;
            color: #cbd5e0;
        }
        .input-group input, .input-group select {
            background-color: #4a5568;
            border: 1px solid #6b7280;
            border-radius: 0.5rem;
            padding: 0.75rem;
            font-size: 1.1rem;
            color: #e2e8f0;
            width: 100%;
            box-sizing: border-box;
        }
        .calculate-btn {
            background-color: #38a169;
            color: white;
            font-size: 1.2rem;
            padding: 1rem 2rem;
            border-radius: 0.75rem;
            cursor: pointer;
            transition: background-color 0.2s ease-in-out, transform 0.1s ease-in-out;
            border: none;
            outline: none;
            margin-top: 1.5rem;
            width: fit-content;
            align-self: center;
        }
        .calculate-btn:hover {
            background-color: #2f855a;
            transform: translateY(-2px);
        }

        .results-section {
            background-color: #3a475a;
            border-radius: 1rem;
            padding: 1.5rem;
            margin-top: 2rem;
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.3);
            display: none;
        }
        .results-section.active {
            display: block;
        }
        .results-section h2 {
            font-size: 1.8rem;
            font-weight: bold;
            color: #00ffff;
            margin-bottom: 1rem;
            text-align: center;
        }
        .results-section p {
            font-size: 1.1rem;
            margin-bottom: 0.5rem;
            line-height: 1.5;
        }
        .results-section p strong {
            color: #f6ad55;
        }

        .message-box-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out;
        }
        .message-box-overlay.active {
            opacity: 1;
            visibility: visible;
        }
        .message-box {
            background-color: #2d3748;
            padding: 2rem;
            border-radius: 1rem;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            text-align: center;
            max-width: 400px;
            color: #e2e8f0;
        }

        .history-container {
            background-color: #2d3748;
            border-radius: 1.5rem;
            padding: 1.5rem;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5);
            width: 100%;
            max-width: 600px;
            border: 2px solid #4a5568;
            color: #e2e8f0;
            max-height: 350px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            margin-top: 1.5rem;
        }
        #calculationHistoryList li {
            background-color: #4a5568;
            padding: 0.75rem;
            border-radius: 0.5rem;
            margin-bottom: 0.5rem;
            font-size: 0.9rem;
        }
        .user-id-display {
            font-size: 0.8rem;
            color: #a0aec0;
            text-align: center;
            margin-top: 1rem;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>निर्माण सामग्री कैलकुलेटर</h1>

        <div class="global-settings">
            <div class="input-group">
                <label for="input-unit-selector">इनपुट यूनिट:</label>
                <select id="input-unit-selector">
                    <option value="meter">मीटर</option>
                    <option value="feet">फीट</option>
                    <option value="inch">इंच</option>
                </select>
            </div>
            <div class="input-group">
                <label for="default-dry-volume-factor">डिफ़ॉल्ट ड्राई वॉल्यूम (कंक्रीट):</label>
                <input type="number" id="default-dry-volume-factor" value="1.54" min="1" step="0.01">
            </div>
            <div class="input-group">
                <label for="default-dry-mortar-factor">डिफ़ॉल्ट ड्राई मोर्टार (ईंट):</label>
                <input type="number" id="default-dry-mortar-factor" value="1.33" min="1" step="0.01">
            </div>
        </div>

        <div class="section-selector">
            <button class="section-btn active" data-target="bitumen-road-calc">बिटुमिन सड़क</button>
            <button class="section-btn" data-target="cc-road-calc">सीसी सड़क</button>
            <button class="section-btn" data-target="drain-calc">नाली</button>
            <button class="section-btn" data-target="brick-wall-calc">ईंट की दीवार</button>
        </div>

        <!-- Bitumen Road -->
        <div id="bitumen-road-calc" class="calculator-section active">
            <h2>बिटुमिन सड़क सामग्री</h2>
            <div class="input-group">
                <label for="br-length">लंबाई:</label>
                <input type="number" id="br-length" placeholder="100" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="br-width">चौड़ाई:</label>
                <input type="number" id="br-width" placeholder="3.5" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="br-thickness">मोटाई:</label>
                <input type="number" id="br-thickness" placeholder="0.05" min="0" step="0.001">
            </div>
            <button class="calculate-btn" onclick="calculateBitumenRoad()">गणना करें</button>
        </div>

        <!-- CC Road -->
        <div id="cc-road-calc" class="calculator-section">
            <h2>सीसी सड़क सामग्री</h2>
            <div class="input-group">
                <label for="ccr-length">लंबाई:</label>
                <input type="number" id="ccr-length" placeholder="50" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="ccr-width">चौड़ाई:</label>
                <input type="number" id="ccr-width" placeholder="4" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="ccr-thickness">मोटाई:</label>
                <input type="number" id="ccr-thickness" placeholder="0.15" min="0" step="0.001">
            </div>
            <div class="input-group">
                <label for="ccr-mix-ratio">कंक्रीट मिक्स:</label>
                <select id="ccr-mix-ratio">
                    <option value="1:1.5:3">M20 (1:1.5:3)</option>
                    <option value="1:2:4">M15 (1:2:4)</option>
                    <option value="1:3:6">M10 (1:3:6)</option>
                </select>
            </div>
            <button class="calculate-btn" onclick="calculateCCRoad()">गणना करें</button>
        </div>

        <!-- Drain Calculator Section -->
        <div id="drain-calc" class="calculator-section">
            <h2>नाली सामग्री</h2>
            <div class="input-group">
                <label for="drain-length">लंबाई:</label>
                <input type="number" id="drain-length" placeholder="20" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="drain-width">अंदरूनी चौड़ाई (Clear Width):</label>
                <input type="number" id="drain-width" placeholder="0.4" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="drain-height">अंदरूनी ऊंचाई (Height):</label>
                <input type="number" id="drain-height" placeholder="0.5" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="drain-shape">नाली का आकार (Shape):</label>
                <select id="drain-shape">
                    <option value="u-type">U-Type (Base + 2 Walls)</option>
                    <option value="l-type">L-Type (Base + 1 Wall)</option>
                </select>
            </div>
            <div class="input-group">
                <label for="drain-type">सामग्री का प्रकार:</label>
                <select id="drain-type" onchange="toggleDrainMaterialInputs()">
                    <option value="concrete">कंक्रीट नाली</option>
                    <option value="brick">ईंट की नाली</option>
                </select>
            </div>
            
            <div id="drain-brick-inputs" style="display: none;">
                <div class="input-group">
                    <label for="drain-brick-thickness">दीवार और बेस की मोटाई:</label>
                    <input type="number" id="drain-brick-thickness" placeholder="0.1" min="0" step="0.001">
                </div>
                <div class="input-group">
                    <label for="drain-mortar-ratio">मोर्टार अनुपात:</label>
                    <select id="drain-mortar-ratio">
                        <option value="1:4">1:4</option>
                        <option value="1:6">1:6</option>
                    </select>
                </div>
            </div>

            <div id="drain-concrete-inputs">
                <div class="input-group">
                    <label for="drain-conc-thickness">कंक्रीट की मोटाई (Base/Wall):</label>
                    <input type="number" id="drain-conc-thickness" placeholder="0.1" min="0" step="0.001">
                </div>
                <div class="input-group">
                    <label for="drain-concrete-mix-ratio">कंक्रीट मिक्स:</label>
                    <select id="drain-concrete-mix-ratio">
                        <option value="1:1.5:3">M20 (1:1.5:3)</option>
                        <option value="1:2:4">M15 (1:2:4)</option>
                    </select>
                </div>
            </div>
            <button class="calculate-btn" onclick="calculateDrain()">गणना करें</button>
        </div>

        <!-- Brick Wall -->
        <div id="brick-wall-calc" class="calculator-section">
            <h2>ईंट की दीवार सामग्री</h2>
            <div class="input-group">
                <label for="bw-length">लंबाई:</label>
                <input type="number" id="bw-length" placeholder="10" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="bw-height">ऊंचाई:</label>
                <input type="number" id="bw-height" placeholder="3" min="0" step="0.01">
            </div>
            <div class="input-group">
                <label for="bw-thickness">दीवार की मोटाई:</label>
                <input type="number" id="bw-thickness" placeholder="0.1" min="0" step="0.001">
            </div>
            <div class="input-group">
                <label for="bw-mortar-ratio">मोर्टार अनुपात:</label>
                <select id="bw-mortar-ratio">
                    <option value="1:4">1:4</option>
                    <option value="1:6">1:6</option>
                </select>
            </div>
            <button class="calculate-btn" onclick="calculateBrickWall()">गणना करें</button>
        </div>

        <!-- Results Section -->
        <div id="results-section" class="results-section">
            <div id="results-content"></div>
        </div>
    </div>

    <!-- History Section -->
    <div class="history-container">
        <h2>गणना इतिहास</h2>
        <ul id="calculationHistoryList"></ul>
        <div id="userIdDisplay" class="user-id-display"></div>
    </div>

    <!-- Custom Message Box -->
    <div id="messageBoxOverlay" class="message-box-overlay">
        <div class="message-box">
            <h3 id="messageBoxTitle" class="text-xl font-bold mb-2"></h3>
            <p id="messageBoxContent" class="mb-4"></p>
            <button class="bg-blue-500 text-white px-4 py-2 rounded" onclick="hideMessageBox()">ठीक है</button>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, addDoc, onSnapshot, collection, query, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        let db, auth, userId = null, calculationsCollectionRef = null, unsubscribeCalculations = null;

        function showSection(sectionId) {
            document.querySelectorAll('.calculator-section').forEach(s => s.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            document.querySelectorAll('.section-btn').forEach(b => b.classList.remove('active'));
            document.querySelector(`.section-btn[data-target="${sectionId}"]`).classList.add('active');
            document.getElementById('results-section').classList.remove('active');
        }

        function displayResults(title, html) {
            const container = document.getElementById('results-section');
            document.getElementById('results-content').innerHTML = `<h2 class="text-xl font-bold text-cyan-400 mb-2">${title}</h2>` + html;
            container.classList.add('active');
            container.scrollIntoView({ behavior: 'smooth' });
        }

        function getInputValueInMeters(id) {
            const val = parseFloat(document.getElementById(id).value);
            if (isNaN(val) || val <= 0) return null;
            const unit = document.getElementById('input-unit-selector').value;
            if (unit === 'feet') return val * 0.3048;
            if (unit === 'inch') return val * 0.0254;
            return val;
        }

        function showMessageBox(title, msg, type = 'info') {
            const overlay = document.getElementById('messageBoxOverlay');
            document.getElementById('messageBoxTitle').textContent = title;
            document.getElementById('messageBoxContent').textContent = msg;
            overlay.classList.add('active');
        }

        window.hideMessageBox = () => document.getElementById('messageBoxOverlay').classList.remove('active');

        window.toggleDrainMaterialInputs = () => {
            const type = document.getElementById('drain-type').value;
            document.getElementById('drain-brick-inputs').style.display = type === 'brick' ? 'block' : 'none';
            document.getElementById('drain-concrete-inputs').style.display = type === 'concrete' ? 'block' : 'none';
        };

        async function saveToFirestore(type, inputs, results) {
            if (userId && calculationsCollectionRef) {
                await addDoc(calculationsCollectionRef, { type, inputs, results, timestamp: serverTimestamp() });
            }
        }

        // Logic for Bitumen Road
        window.calculateBitumenRoad = () => {
            const l = getInputValueInMeters('br-length'), w = getInputValueInMeters('br-width'), t = getInputValueInMeters('br-thickness');
            if (!l || !w || !t) return showMessageBox('Error', 'Sabhi details bharen');
            const vol = l * w * t;
            const weight = vol * 2.35; 
            const html = `<p>Vol: <strong>${vol.toFixed(3)} m³</strong></p><p>Weight: <strong>${weight.toFixed(2)} Tonnes</strong></p>`;
            displayResults('Bitumen Result', html);
            saveToFirestore('Bitumen Road', {l, w, t}, {vol, weight});
        };

        // Logic for CC Road
        window.calculateCCRoad = () => {
            const l = getInputValueInMeters('ccr-length'), w = getInputValueInMeters('ccr-width'), t = getInputValueInMeters('ccr-thickness');
            if (!l || !w || !t) return showMessageBox('Error', 'Input check karein');
            const wetVol = l * w * t;
            const dryVol = wetVol * parseFloat(document.getElementById('default-dry-volume-factor').value);
            const ratio = document.getElementById('ccr-mix-ratio').value.split(':').map(Number);
            const sum = ratio.reduce((a,b) => a+b, 0);
            const cement = (ratio[0]/sum) * dryVol;
            const bags = (cement * 1440) / 50;
            const html = `<p>Wet Vol: <strong>${wetVol.toFixed(3)} m³</strong></p><p>Cement: <strong>${Math.ceil(bags)} Bags</strong></p>`;
            displayResults('CC Road Result', html);
            saveToFirestore('CC Road', {l, w, t}, {wetVol, bags: Math.ceil(bags)});
        };

        // Logic for Drain (Updated with L/U Type)
        window.calculateDrain = () => {
            const l = getInputValueInMeters('drain-length');
            const innerW = getInputValueInMeters('drain-width');
            const innerH = getInputValueInMeters('drain-height');
            const shape = document.getElementById('drain-shape').value;
            const material = document.getElementById('drain-type').value;

            if (!l || !innerW || !innerH) return showMessageBox('Error', 'Input sahi bharein');

            let wetVol = 0;
            let resultsHtml = "";

            if (material === 'concrete') {
                const thick = getInputValueInMeters('drain-conc-thickness') || 0.1;
                // Base Volume
                const baseVol = l * (innerW + 2 * thick) * thick;
                // Walls Volume
                const wallCount = shape === 'u-type' ? 2 : 1;
                const wallVol = wallCount * (l * thick * innerH);
                wetVol = baseVol + wallVol;

                const dryFactor = parseFloat(document.getElementById('default-dry-volume-factor').value);
                const dryVol = wetVol * dryFactor;
                const ratio = document.getElementById('drain-concrete-mix-ratio').value.split(':').map(Number);
                const sum = ratio.reduce((a,b) => a+b, 0);
                const cementBags = Math.ceil(((ratio[0]/sum) * dryVol * 1440) / 50);

                resultsHtml = `<p>Shape: <strong>${shape.toUpperCase()}</strong></p>
                               <p>Total Concrete: <strong>${wetVol.toFixed(3)} m³</strong></p>
                               <p>Cement: <strong>${cementBags} Bags</strong></p>`;
                saveToFirestore('Drain', {l, innerW, innerH, shape, material}, {wetVol, cementBags});
            } else {
                // Brick Logic
                const thick = getInputValueInMeters('drain-brick-thickness') || 0.1;
                const wallCount = shape === 'u-type' ? 2 : 1;
                const wallVol = wallCount * (l * thick * innerH);
                const baseVol = l * (innerW + (wallCount * thick)) * thick;
                const totalBrickVol = wallVol + baseVol;

                const brickSizeWithMortar = (0.2 * 0.1 * 0.1); // Standard 190x90x90 + 10mm
                const bricks = Math.ceil(totalBrickVol / brickSizeWithMortar);

                resultsHtml = `<p>Shape: <strong>${shape.toUpperCase()}</strong></p>
                               <p>Total Bricks: <strong>${bricks} Nos</strong></p>
                               <p>Brickwork Vol: <strong>${totalBrickVol.toFixed(3)} m³</strong></p>`;
                saveToFirestore('Drain', {l, innerW, innerH, shape, material}, {bricks});
            }

            displayResults('Drain Result', resultsHtml);
        };

        // Logic for Brick Wall
        window.calculateBrickWall = () => {
            const l = getInputValueInMeters('bw-length'), h = getInputValueInMeters('bw-height'), t = getInputValueInMeters('bw-thickness');
            if(!l || !h || !t) return showMessageBox('Error', 'Details check karein');
            const vol = l * h * t;
            const bricks = Math.ceil(vol / (0.2 * 0.1 * 0.1));
            displayResults('Brick Wall Result', `<p>Bricks: <strong>${bricks} Nos</strong></p>`);
            saveToFirestore('Brick Wall', {l, h, t}, {bricks});
        };

        // Firebase Setup
        async function init() {
            db = getFirestore(initializeApp(firebaseConfig));
            auth = getAuth();
            if (initialAuthToken) await signInWithCustomToken(auth, initialAuthToken);
            else await signInAnonymously(auth);

            onAuthStateChanged(auth, user => {
                if (user) {
                    userId = user.uid;
                    document.getElementById('userIdDisplay').textContent = `ID: ${userId}`;
                    calculationsCollectionRef = collection(db, 'artifacts', appId, 'users', userId, 'construction_calculations');
                    
                    if (unsubscribeCalculations) unsubscribeCalculations();
                    unsubscribeCalculations = onSnapshot(query(calculationsCollectionRef), snap => {
                        const list = document.getElementById('calculationHistoryList');
                        list.innerHTML = '';
                        const docs = snap.docs.map(d => d.data()).sort((a,b) => (b.timestamp?.seconds || 0) - (a.timestamp?.seconds || 0));
                        docs.slice(0, 10).forEach(d => {
                            const li = document.createElement('li');
                            li.textContent = `${d.type}: ${new Date(d.timestamp?.seconds*1000).toLocaleTimeString()}`;
                            list.appendChild(li);
                        });
                    });
                }
            });
        }

        document.querySelectorAll('.section-btn').forEach(btn => btn.onclick = () => showSection(btn.dataset.target));
        window.onload = init;
    </script>
</body>
</html>
