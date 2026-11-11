# 4-jogos-2
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quadrologia de Games for Health</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        
        .game-container {
            display: none;
            animation: fadeIn 0.3s ease-in;
        }
        
        .game-container.active {
            display: block;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        canvas {
            border: 4px solid #fff;
            border-radius: 12px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
            background: #f0f0f0;
            max-width: 100%;
            height: auto;
        }
        
        .btn-game {
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .btn-game:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0,0,0,0.4);
        }
        
        .hud {
            background: rgba(255,255,255,0.95);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 1rem;
            margin-bottom: 1rem;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }
        
        .breath-circle {
            transition: all 0.5s ease;
        }
        
        .health-bar {
            transition: width 0.3s ease;
        }
        
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
        
        .pulse-animation {
            animation: pulse 2s infinite;
        }
    </style>
</head>
<body class="p-4">
    <!-- Menu Principal -->
    <div id="mainMenu" class="game-container active">
        <div class="max-w-6xl mx-auto">
            <div class="text-center mb-12">
                <h1 class="text-6xl font-bold text-white mb-4 drop-shadow-lg">🎮 Quadrologia de Games for Health</h1>
                <p class="text-2xl text-white/90">4 Jogos Educativos para Promoção da Saúde</p>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <!-- Jogo 1: Dengue -->
                <div onclick="showGame('dengue')" class="btn-game bg-gradient-to-br from-red-500 to-red-700 p-8 rounded-2xl text-white cursor-pointer">
                    <div class="text-6xl mb-4">🦟</div>
                    <h2 class="text-3xl font-bold mb-2">Dengue Fighter</h2>
                    <p class="text-lg opacity-90">Combate à Dengue</p>
                    <p class="text-sm mt-4 opacity-75">Elimine focos do mosquito Aedes aegypti antes que se multipliquem!</p>
                </div>
                
                <!-- Jogo 2: Dieta -->
                <div onclick="showGame('dieta')" class="btn-game bg-gradient-to-br from-green-500 to-green-700 p-8 rounded-2xl text-white cursor-pointer">
                    <div class="text-6xl mb-4">🥗</div>
                    <h2 class="text-3xl font-bold mb-2">Dieta Equilibrada</h2>
                    <p class="text-lg opacity-90">Nutrição e Pirâmide Alimentar</p>
                    <p class="text-sm mt-4 opacity-75">Colete alimentos saudáveis e desvie dos ultraprocessados!</p>
                </div>
                
                <!-- Jogo 3: Respiração -->
                <div onclick="showGame('respiracao')" class="btn-game bg-gradient-to-br from-blue-500 to-blue-700 p-8 rounded-2xl text-white cursor-pointer">
                    <div class="text-6xl mb-4">🧘</div>
                    <h2 class="text-3xl font-bold mb-2">Mente Calma</h2>
                    <p class="text-lg opacity-90">Respiração & Relaxamento</p>
                    <p class="text-sm mt-4 opacity-75">Siga o ritmo da respiração quadrada para controlar a ansiedade!</p>
                </div>
                
                <!-- Jogo 4: Imunização -->
                <div onclick="showGame('imunizacao')" class="btn-game bg-gradient-to-br from-purple-500 to-purple-700 p-8 rounded-2xl text-white cursor-pointer">
                    <div class="text-6xl mb-4">💉</div>
                    <h2 class="text-3xl font-bold mb-2">Herói da Imunização</h2>
                    <p class="text-lg opacity-90">Vacinação & Sistema Imune</p>
                    <p class="text-sm mt-4 opacity-75">Aplique vacinas para deter patógenos em tempo real!</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Jogo 1: Dengue Fighter -->
    <div id="dengue" class="game-container">
        <div class="max-w-4xl mx-auto">
            <button onclick="backToMenu()" class="mb-4 bg-white text-gray-800 px-6 py-3 rounded-lg font-bold hover:bg-gray-100 transition">← Voltar ao Menu</button>
            <div class="hud">
                <div class="flex justify-between items-center flex-wrap gap-4">
                    <div class="text-2xl font-bold text-gray-800">🦟 Dengue Fighter</div>
                    <div class="flex gap-6">
                        <div class="text-xl font-bold text-red-600">Pontos: <span id="dengueScore">0</span></div>
                        <div class="text-xl font-bold text-blue-600">Vida: <span id="dengueHealth">100</span>%</div>
                    </div>
                </div>
                <div class="mt-3">
                    <div class="w-full bg-gray-200 rounded-full h-4 overflow-hidden">
                        <div id="dengueHealthBar" class="health-bar bg-gradient-to-r from-green-400 to-green-600 h-full" style="width: 100%"></div>
                    </div>
                </div>
            </div>
            <canvas id="dengueCanvas" width="800" height="600"></canvas>
            <div class="mt-4 text-center">
                <button onclick="startDengueGame()" class="bg-red-500 hover:bg-red-600 text-white px-8 py-3 rounded-lg font-bold text-lg transition">Iniciar Jogo</button>
                <button onclick="resetDengueGame()" class="bg-gray-500 hover:bg-gray-600 text-white px-8 py-3 rounded-lg font-bold text-lg transition ml-2">Reiniciar</button>
            </div>
        </div>
    </div>

    <!-- Jogo 2: Dieta Equilibrada -->
    <div id="dieta" class="game-container">
        <div class="max-w-4xl mx-auto">
            <button onclick="backToMenu()" class="mb-4 bg-white text-gray-800 px-6 py-3 rounded-lg font-bold hover:bg-gray-100 transition">← Voltar ao Menu</button>
            <div class="hud">
                <div class="flex justify-between items-center flex-wrap gap-4">
                    <div class="text-2xl font-bold text-gray-800">🥗 Dieta Equilibrada</div>
                    <div class="flex gap-6">
                        <div class="text-xl font-bold text-green-600">Nutrientes: <span id="dietaScore">0</span></div>
                        <div class="text-xl font-bold text-red-600">Vida: <span id="dietaHealth">100</span>%</div>
                    </div>
                </div>
                <div class="mt-3">
                    <div class="w-full bg-gray-200 rounded-full h-4 overflow-hidden">
                        <div id="dietaHealthBar" class="health-bar bg-gradient-to-r from-green-400 to-green-600 h-full" style="width: 100%"></div>
                    </div>
                </div>
            </div>
            <canvas id="dietaCanvas" width="800" height="600"></canvas>
            <div class="mt-4 text-center">
                <button onclick="startDietaGame()" class="bg-green-500 hover:bg-green-600 text-white px-8 py-3 rounded-lg font-bold text-lg transition">Iniciar Jogo</button>
                <button onclick="resetDietaGame()" class="bg-gray-500 hover:bg-gray-600 text-white px-8 py-3 rounded-lg font-bold text-lg transition ml-2">Reiniciar</button>
            </div>
        </div>
    </div>

    <!-- Jogo 3: Respiração -->
    <div id="respiracao" class="game-container">
        <div class="max-w-4xl mx-auto">
            <button onclick="backToMenu()" class="mb-4 bg-white text-gray-800 px-6 py-3 rounded-lg font-bold hover:bg-gray-100 transition">← Voltar ao Menu</button>
            <div class="hud">
                <div class="flex justify-between items-center flex-wrap gap-4">
                    <div class="text-2xl font-bold text-gray-800">🧘 Mente Calma</div>
                    <div class="flex gap-6">
                        <div class="text-xl font-bold text-blue-600">Ciclos: <span id="respScore">0</span></div>
                        <div class="text-xl font-bold text-purple-600">Calma: <span id="respCalm">0</span>%</div>
                    </div>
                </div>
            </div>
            <canvas id="respCanvas" width="800" height="600"></canvas>
            <div class="mt-4 text-center">
                <button onclick="startRespGame()" class="bg-blue-500 hover:bg-blue-600 text-white px-8 py-3 rounded-lg font-bold text-lg transition">Iniciar Sessão</button>
                <button onclick="resetRespGame()" class="bg-gray-500 hover:bg-gray-600 text-white px-8 py-3 rounded-lg font-bold text-lg transition ml-2">Reiniciar</button>
            </div>
        </div>
    </div>

    <!-- Jogo 4: Imunização -->
    <div id="imunizacao" class="game-container">
        <div class="max-w-4xl mx-auto">
            <button onclick="backToMenu()" class="mb-4 bg-white text-gray-800 px-6 py-3 rounded-lg font-bold hover:bg-gray-100 transition">← Voltar ao Menu</button>
            <div class="hud">
                <div class="flex justify-between items-center flex-wrap gap-4">
                    <div class="text-2xl font-bold text-gray-800">💉 Herói da Imunização</div>
                    <div class="flex gap-6">
                        <div class="text-xl font-bold text-purple-600">Pontos: <span id="imunScore">0</span></div>
                        <div class="text-xl font-bold text-red-600">Vida: <span id="imunHealth">100</span>%</div>
                    </div>
                </div>
                <div class="mt-3">
                    <div class="w-full bg-gray-200 rounded-full h-4 overflow-hidden">
                        <div id="imunHealthBar" class="health-bar bg-gradient-to-r from-green-400 to-green-600 h-full" style="width: 100%"></div>
                    </div>
                </div>
            </div>
            <canvas id="imunCanvas" width="800" height="600"></canvas>
            <div class="mt-4 text-center">
                <button onclick="startImunGame()" class="bg-purple-500 hover:bg-purple-600 text-white px-8 py-3 rounded-lg font-bold text-lg transition">Iniciar Jogo</button>
                <button onclick="resetImunGame()" class="bg-gray-500 hover:bg-gray-600 text-white px-8 py-3 rounded-lg font-bold text-lg transition ml-2">Reiniciar</button>
            </div>
        </div>
    </div>

    <script>
        // ============ AUDIO SYSTEM ============
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        const audioCtx = new AudioContext();

        function playSuccessSFX() {
            if (audioCtx.state === 'suspended') audioCtx.resume();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            
            osc.type = 'sine';
            osc.frequency.setValueAtTime(587.33, audioCtx.currentTime);
            osc.frequency.exponentialRampToValueAtTime(880, audioCtx.currentTime + 0.15);
            
            gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
            gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.15);
            
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            
            osc.start();
            osc.stop(audioCtx.currentTime + 0.15);
        }

        function playErrorSFX() {
            if (audioCtx.state === 'suspended') audioCtx.resume();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            
            osc.type = 'sawtooth';
            osc.frequency.setValueAtTime(150, audioCtx.currentTime);
            osc.frequency.linearRampToValueAtTime(60, audioCtx.currentTime + 0.2);
            
            gain.gain.setValueAtTime(0.4, audioCtx.currentTime);
            gain.gponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.2);
            
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            
            osc.start();
            osc.stop(audioCtx.currentTime + 0.2);
        }

        function playBreathSFX() {
            if (audioCtx.state === 'suspended') audioCtx.resume();
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            
            osc.type = 'sine';
            osc.frequency.setValueAtTime(220, audioCtx.currentTime);
            osc.frequency.linearRampToValueAtTime(440, audioCtx.currentTime + 0.5);
            
            gain.gain.setValueAtTime(0.15, audioCtx.currentTime);
            gain.gain.linearRampToValueAtTime(0.01, audioCtx.currentTime + 0.5);
            
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            
            osc.start();
            osc.stop(audioCtx.currentTime + 0.5);
        }

        // ============ NAVIGATION ============
        function showGame(gameId) {
            document.querySelectorAll('.game-container').forEach(el => el.classList.remove('active'));
            document.getElementById(gameId).classList.add('active');
        }

        function backToMenu() {
            document.querySelectorAll('.game-container').forEach(el => el.classList.remove('active'));
            document.getElementById('mainMenu').classList.add('active');
            
            // Reset all games
            resetDengueGame();
            resetDietaGame();
            resetRespGame();
            resetImunGame();
        }

        // ============ JOGO 1: DENGUE FIGHTER ============
        let dengueCanvas, dengueCtx;
        let dengueScore = 0, dengueHealth = 100;
        let dengueTargets = [];
        let dengueGameRunning = false;
        let dengueSpawnInterval;
        let dengueGameLoop;

        function initDengue() {
            dengueCanvas = document.getElementById('dengueCanvas');
            dengueCtx = dengueCanvas.getContext('2d');
            dengueCanvas.addEventListener('click', handleDengueClick);
        }

        function startDengueGame() {
            if (dengueGameRunning) return;
            dengueGameRunning = true;
            dengueScore = 0;
            dengueHealth = 100;
            dengueTargets = [];
            updateDengueHUD();
            
            dengueSpawnInterval = setInterval(spawnDengueTarget, 2000);
            dengueGameLoop = setInterval(updateDengueGame, 50);
        }

        function resetDengueGame() {
            dengueGameRunning = false;
            clearInterval(dengueSpawnInterval);
            clearInterval(dengueGameLoop);
            dengueScore = 0;
            dengueHealth = 100;
            dengueTargets = [];
            updateDengueHUD();
            drawDengueGame();
        }

        function spawnDengueTarget() {
            if (!dengueGameRunning) return;
            const types = ['🛞', '🪣', '🌺'];
            const type = types[Math.floor(Math.random() * types.length)];
            dengueTargets.push({
                x: Math.random() * (dengueCanvas.width - 60) + 30,
                y: Math.random() * (dengueCanvas.height - 60) + 30,
                type: type,
                size: 40,
                life: 100,
                decay: 0.5
            });
        }

        function handleDengueClick(e) {
            if (!dengueGameRunning) return;
            const rect = dengueCanvas.getBoundingClientRect();
            const x = (e.clientX - rect.left) * (dengueCanvas.width / rect.width);
            const y = (e.clientY - rect.top) * (dengueCanvas.height / rect.height);
            
            for (let i = dengueTargets.length - 1; i >= 0; i--) {
                const target = dengueTargets[i];
                const dist = Math.sqrt((x - target.x) ** 2 + (y - target.y) ** 2);
                if (dist < target.size) {
                    dengueTargets.splice(i, 1);
                    dengueScore += 10;
                    playSuccessSFX();
                    updateDengueHUD();
                    break;
                }
            }
        }

        function updateDengueGame() {
            if (!dengueGameRunning) return;
            
            for (let i = dengueTargets.length - 1; i >= 0; i--) {
                dengueTargets[i].life -= dengueTargets[i].decay;
                if (dengueTargets[i].life <= 0) {
                    dengueTargets.splice(i, 1);
                    dengueHealth -= 5;
                    playErrorSFX();
                    updateDengueHUD();
                }
            }
            
            if (dengueHealth <= 0) {
                dengueHealth = 0;
                dengueGameRunning = false;
                clearInterval(dengueSpawnInterval);
                clearInterval(dengueGameLoop);
                alert('Game Over! Pontos: ' + dengueScore);
            }
            
            drawDengueGame();
        }

        function drawDengueGame() {
            dengueCtx.fillStyle = '#87CEEB';
            dengueCtx.fillRect(0, 0, dengueCanvas.width, dengueCanvas.height);
            
            dengueCtx.fillStyle = '#228B22';
            dengueCtx.fillRect(0, dengueCanvas.height - 100, dengueCanvas.width, 100);
            
            dengueTargets.forEach(target => {
                dengueCtx.font = `${target.size}px Arial`;
                dengueCtx.textAlign = 'center';
                dengueCtx.textBaseline = 'middle';
                dengueCtx.globalAlpha = target.life / 100;
                dengueCtx.fillText(target.type, target.x, target.y);
                dengueCtx.globalAlpha = 1;
            });
            
            if (!dengueGameRunning && dengueHealth > 0) {
                dengueCtx.fillStyle = 'rgba(0,0,0,0.7)';
                dengueCtx.fillRect(0, 0, dengueCanvas.width, dengueCanvas.height);
                dengueCtx.fillStyle = '#fff';
                dengueCtx.font = '40px Arial';
                dengueCtx.textAlign = 'center';
                dengueCtx.fillText('Clique em "Iniciar Jogo"', dengueCanvas.width/2, dengueCanvas.height/2);
            }
        }

        function updateDengueHUD() {
            document.getElementById('dengueScore').textContent = dengueScore;
            document.getElementById('dengueHealth').textContent = dengueHealth;
            document.getElementById('dengueHealthBar').style.width = dengueHealth + '%';
        }

        // ============ JOGO 2: DIETA EQUILIBRADA ============
        let dietaCanvas, dietaCtx;
        let dietaScore = 0, dietaHealth = 100;
        let dietaPlayer = { x: 400, y: 500, width: 60, height: 60 };
        let dietaItems = [];
        let dietaGameRunning = false;
        let dietaSpawnInterval;
        let dietaGameLoop;
        let keys = {};

        function initDieta() {
            dietaCanvas = document.getElementById('dietaCanvas');
            dietaCtx = dietaCanvas.getContext('2d');
            
            document.addEventListener('keydown', (e) => keys[e.key] = true);
            document.addEventListener('keyup', (e) => keys[e.key] = false);
        }

        function startDietaGame() {
            if (dietaGameRunning) return;
            dietaGameRunning = true;
            dietaScore = 0;
            dietaHealth = 100;
            dietaItems = [];
            dietaPlayer.x = 400;
            updateDietaHUD();
            
            dietaSpawnInterval = setInterval(spawnDietaItem, 1000);
            dietaGameLoop = setInterval(updateDietaGame, 30);
        }

        function resetDietaGame() {
            dietaGameRunning = false;
            clearInterval(dietaSpawnInterval);
            clearInterval(dietaGameLoop);
            dietaScore = 0;
            dietaHealth = 100;
            dietaItems = [];
            dietaPlayer.x = 400;
            updateDietaHUD();
            drawDietaGame();
        }

        function spawnDietaItem() {
            if (!dietaGameRunning) return;
            const healthy = ['🍎', '🥕', '🥦', '🍌', '🥬'];
            const unhealthy = ['🍔', '🍟', '🍕', '🍩', '🥤'];
            const isHealthy = Math.random() > 0.4;
            const items = isHealthy ? healthy : unhealthy;
            
            dietaItems.push({
                x: Math.random() * (dietaCanvas.width - 40) + 20,
                y: -40,
                type: items[Math.floor(Math.random() * items.length)],
                healthy: isHealthy,
                speed: 3 + Math.random() * 2,
                size: 40
            });
        }

        function updateDietaGame() {
            if (!dietaGameRunning) return;
            
            if (keys['ArrowLeft'] && dietaPlayer.x > 30) dietaPlayer.x -= 5;
            if (keys['ArrowRight'] && dietaPlayer.x < dietaCanvas.width - 30) dietaPlayer.x += 5;
            
            for (let i = dietaItems.length - 1; i >= 0; i--) {
                dietaItems[i].y += dietaItems[i].speed;
                
                if (dietaItems[i].y > dietaCanvas.height) {
                    dietaItems.splice(i, 1);
                    continue;
                }
                
                const dist = Math.abs(dietaItems[i].x - dietaPlayer.x);
                if (dist < 40 && dietaItems[i].y > dietaPlayer.y - 20 && dietaItems[i].y < dietaPlayer.y + 40) {
                    if (dietaItems[i].healthy) {
                        dietaScore += 10;
                        playSuccessSFX();
                    } else {
                        dietaHealth -= 10;
                        playErrorSFX();
                    }
                    dietaItems.splice(i, 1);
                    updateDietaHUD();
                }
            }
            
            if (dietaHealth <= 0) {
                dietaHealth = 0;
                dietaGameRunning = false;
                clearInterval(dietaSpawnInterval);
                clearInterval(dietaGameLoop);
                alert('Game Over! Nutrientes: ' + dietaScore);
            }
            
            drawDietaGame();
        }

        function drawDietaGame() {
            dietaCtx.fillStyle = '#FFE4B5';
            dietaCtx.fillRect(0, 0, dietaCanvas.width, dietaCanvas.height);
            
            dietaCtx.font = '50px Arial';
            dietaCtx.textAlign = 'center';
            dietaCtx.textBaseline = 'middle';
            dietaCtx.fillText('🧺', dietaPlayer.x, dietaPlayer.y);
            
            dietaItems.forEach(item => {
                dietaCtx.font = `${item.size}px Arial`;
                dietaCtx.fillText(item.type, item.x, item.y);
            });
            
            if (!dietaGameRunning && dietaHealth > 0) {
                dietaCtx.fillStyle = 'rgba(0,0,0,0.7)';
                dietaCtx.fillRect(0, 0, dietaCanvas.width, dietaCanvas.height);
                dietaCtx.fillStyle = '#fff';
                dietaCtx.font = '40px Arial';
                dietaCtx.textAlign = 'center';
                dietaCtx.fillText('Use ← → para mover', dietaCanvas.width/2, dietaCanvas.height/2 - 30);
                dietaCtx.font = '30px Arial';
                dietaCtx.fillText('Clique em "Iniciar Jogo"', dietaCanvas.width/2, dietaCanvas.height/2 + 30);
            }
        }

        function updateDietaHUD() {
            document.getElementById('dietaScore').textContent = dietaScore;
            document.getElementById('dietaHealth').textContent = dietaHealth;
            document.getElementById('dietaHealthBar').style.width = dietaHealth + '%';
        }

        // ============ JOGO 3: RESPIRAÇÃO ============
        let respCanvas, respCtx;
        let respScore = 0, respCalm = 0;
        let respPhase = 0; // 0=inspire, 1=hold, 2=expire, 3=hold
        let respTimer = 0;
        let respGameRunning = false;
        let respGameLoop;
        const phaseDuration = 4000; // 4 seconds per phase

        function initResp() {
            respCanvas = document.getElementById('respCanvas');
            respCtx = respCanvas.getContext('2d');
        }

        function startRespGame() {
            if (respGameRunning) return;
            respGameRunning = true;
            respScore = 0;
            respCalm = 0;
            respPhase = 0;
            respTimer = Date.now();
            updateRespHUD();
            
            respGameLoop = setInterval(updateRespGame, 50);
        }

        function resetRespGame() {
            respGameRunning = false;
            clearInterval(respGameLoop);
            respScore = 0;
            respCalm = 0;
            respPhase = 0;
            updateRespHUD();
            drawRespGame();
        }

        function updateRespGame() {
            if (!respGameRunning) return;
            
            const elapsed = Date.now() - respTimer;
            if (elapsed >= phaseDuration) {
                respPhase = (respPhase + 1) % 4;
                respTimer = Date.now();
                if (respPhase === 0) {
                    respScore++;
                    respCalm = Math.min(100, respCalm + 10);
                    playBreathSFX();
                    updateRespHUD();
                }
            }
            
            drawRespGame();
        }

        function drawRespGame() {
            respCtx.fillStyle = '#E0F7FA';
            respCtx.fillRect(0, 0, respCanvas.width, respCanvas.height);
            
            const elapsed = Date.now() - respTimer;
            const progress = elapsed / phaseDuration;
            
            let circleSize = 100;
            if (respPhase === 0) circleSize = 100 + (progress * 100);
            else if (respPhase === 1) circleSize = 200;
            else if (respPhase === 2) circleSize = 200 - (progress * 100);
            else circleSize = 100;
            
            const gradient = respCtx.createRadialGradient(400, 300, 0, 400, 300, circleSize);
            gradient.addColorStop(0, 'rgba(100, 200, 255, 0.8)');
            gradient.addColorStop(1, 'rgba(100, 200, 255, 0.2)');
            
            respCtx.fillStyle = gradient;
            respCtx.beginPath();
            respCtx.arc(400, 300, circleSize, 0, Math.PI * 2);
            respCtx.fill();
            
            respCtx.fillStyle = '#333';
            respCtx.font = 'bold 48px Arial';
            respCtx.textAlign = 'center';
            respCtx.textBaseline = 'middle';
            
            const phases = ['INSPIRE', 'SEGURE', 'EXPIRE', 'SEGURE'];
            respCtx.fillText(phases[respPhase], 400, 300);
            
            respCtx.font = '24px Arial';
            respCtx.fillText(`${Math.ceil((phaseDuration - elapsed) / 1000)}`, 400, 350);
            
            if (!respGameRunning) {
                respCtx.fillStyle = 'rgba(0,0,0,0.7)';
                respCtx.fillRect(0, 0, respCanvas.width, respCanvas.height);
                respCtx.fillStyle = '#fff';
                respCtx.font = '40px Arial';
                respCtx.textAlign = 'center';
                respCtx.fillText('Siga o ritmo do círculo', respCanvas.width/2, respCanvas.height/2 - 30);
                respCtx.font = '30px Arial';
                respCtx.fillText('Clique em "Iniciar Sessão"', respCanvas.width/2, respCanvas.height/2 + 30);
            }
        }

        function updateRespHUD() {
            document.getElementById('respScore').textContent = respScore;
            document.getElementById('respCalm').textContent = respCalm;
        }

        // ============ JOGO 4: IMUNIZAÇÃO ============
        let imunCanvas, imunCtx;
        let imunScore = 0, imunHealth = 100;
        let imunPlayer = { x: 400, y: 300 };
        let imunPathogens = [];
        let imunBullets = [];
        let imunGameRunning = false;
        let imunSpawnInterval;
        let imunGameLoop;
        let imunKeys = {};

        function initImun() {
            imunCanvas = document.getElementById('imunCanvas');
            imunCtx = imunCanvas.getContext('2d');
            
            document.addEventListener('keydown', (e) => imunKeys[e.key] = true);
            document.addEventListener('keyup', (e) => imunKeys[e.key] = false);
            imunCanvas.addEventListener('click', shootImunBullet);
        }

        function startImunGame() {
            if (imunGameRunning) return;
            imunGameRunning = true;
            imunScore = 0;
            imunHealth = 100;
            imunPathogens = [];
            imunBullets = [];
            imunPlayer = { x: 400, y: 300 };
            updateImunHUD();
            
            imunSpawnInterval = setInterval(spawnImunPathogen, 1500);
            imunGameLoop = setInterval(updateImunGame, 30);
        }

        function resetImunGame() {
            imunGameRunning = false;
            clearInterval(imunSpawnInterval);
            clearInterval(imunGameLoop);
            imunScore = 0;
            imunHealth = 100;
            imunPathogens = [];
            imunBullets = [];
            imunPlayer = { x: 400, y: 300 };
            updateImunHUD();
            drawImunGame();
        }

        function spawnImunPathogen() {
            if (!imunGameRunning) return;
            const types = ['🦠', '🧫', '🧬'];
            imunPathogens.push({
                x: Math.random() * imunCanvas.width,
                y: -30,
                type: types[Math.floor(Math.random() * types.length)],
                speed: 2 + Math.random() * 2,
                size: 40
            });
        }

        function shootImunBullet(e) {
            if (!imunGameRunning) return;
            const rect = imunCanvas.getBoundingClientRect();
            const x = (e.clientX - rect.left) * (imunCanvas.width / rect.width);
            const y = (e.clientY - rect.top) * (imunCanvas.height / rect.height);
            
            const angle = Math.atan2(y - imunPlayer.y, x - imunPlayer.x);
            imunBullets.push({
                x: imunPlayer.x,
                y: imunPlayer.y,
                vx: Math.cos(angle) * 8,
                vy: Math.sin(angle) * 8,
                size: 10
            });
        }

        function updateImunGame() {
            if (!imunGameRunning) return;
            
            if (imunKeys['ArrowLeft'] && imunPlayer.x > 20) imunPlayer.x -= 4;
            if (imunKeys['ArrowRight'] && imunPlayer.x < imunCanvas.width - 20) imunPlayer.x += 4;
            if (imunKeys['ArrowUp'] && imunPlayer.y > 20) imunPlayer.y -= 4;
            if (imunKeys['ArrowDown'] && imunPlayer.y < imunCanvas.height - 20) imunPlayer.y += 4;
            
            for (let i = imunBullets.length - 1; i >= 0; i--) {
                imunBullets[i].x += imunBullets[i].vx;
                imunBullets[i].y += imunBullets[i].vy;
                
                if (imunBullets[i].x < 0 || imunBullets[i].x > imunCanvas.width ||
                    imunBullets[i].y < 0 || imunBullets[i].y > imunCanvas.height) {
                    imunBullets.splice(i, 1);
                    continue;
                }
                
                for (let j = imunPathogens.length - 1; j >= 0; j--) {
                    const dist = Math.sqrt((imunBullets[i].x - imunPathogens[j].x) ** 2 + 
                                          (imunBullets[i].y - imunPathogens[j].y) ** 2);
                    if (dist < 30) {
                        imunPathogens.splice(j, 1);
                        imunBullets.splice(i, 1);
                        imunScore += 15;
                        playSuccessSFX();
                        updateImunHUD();
                        break;
                    }
                }
            }
            
            for (let i = imunPathogens.length - 1; i >= 0; i--) {
                imunPathogens[i].y += imunPathogens[i].speed;
                
                if (imunPathogens[i].y > imunCanvas.height) {
                    imunPathogens.splice(i, 1);
                    imunHealth -= 5;
                    playErrorSFX();
                    updateImunHUD();
                    continue;
                }
                
                const dist = Math.sqrt((imunPathogens[i].x - imunPlayer.x) ** 2 + 
                                      (imunPathogens[i].y - imunPlayer.y) ** 2);
                if (dist < 35) {
                    imunPathogens.splice(i, 1);
                    imunHealth -= 10;
                    playErrorSFX();
                    updateImunHUD();
                }
            }
            
            if (imunHealth <= 0) {
                imunHealth = 0;
                imunGameRunning = false;
                clearInterval(imunSpawnInterval);
                clearInterval(imunGameLoop);
                alert('Game Over! Pontos: ' + imunScore);
            }
            
            drawImunGame();
        }

        function drawImunGame() {
            imunCtx.fillStyle = '#F0E6FF';
            imunCtx.fillRect(0, 0, imunCanvas.width, imunCanvas.height);
            
            imunCtx.font = '40px Arial';
            imunCtx.textAlign = 'center';
            imunCtx.textBaseline = 'middle';
            imunCtx.fillText('💉', imunPlayer.x, imunPlayer.y);
            
            imunCtx.fillStyle = '#FFD700';
            imunBullets.forEach(bullet => {
                imunCtx.beginPath();
                imunCtx.arc(bullet.x, bullet.y, bullet.size, 0, Math.PI * 2);
                imunCtx.fill();
            });
            
            imunPathogens.forEach(pathogen => {
                imunCtx.font = `${pathogen.size}px Arial`;
                imunCtx.fillText(pathogen.type, pathogen.x, pathogen.y);
            });
            
            if (!imunGameRunning && imunHealth > 0) {
                imunCtx.fillStyle = 'rgba(0,0,0,0.7)';
                imunCtx.fillRect(0, 0, imunCanvas.width, imunCanvas.height);
                imunCtx.fillStyle = '#fff';
                imunCtx.font = '40px Arial';
                imunCtx.textAlign = 'center';
                imunCtx.fillText('Use ← → ↑ ↓ para mover', imunCanvas.width/2, imunCanvas.height/2 - 30);
                imunCtx.font = '30px Arial';
                imunCtx.fillText('Clique para atirar anticorpos', imunCanvas.width/2, imunCanvas.height/2 + 30);
            }
        }

        function updateImunHUD() {
            document.getElementById('imunScore').textContent = imunScore;
            document.getElementById('imunHealth').textContent = imunHealth;
            document.getElementById('imunHealthBar').style.width = imunHealth + '%';
        }

        // ============ INITIALIZATION ============
        window.onload = function() {
            initDengue();
            initDieta();
            initResp();
            initImun();
            
            drawDengueGame();
            drawDietaGame();
            drawRespGame();
            drawImunGame();
        };
    </script>
</body>
</html>
