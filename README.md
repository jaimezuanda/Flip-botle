<script type="text/javascript">
        var gk_isXlsx = false;
        var gk_xlsxFileLookup = {};
        var gk_fileData = {};
        function filledCell(cell) {
          return cell !== '' && cell != null;
        }
        function loadFileData(filename) {
        if (gk_isXlsx && gk_xlsxFileLookup[filename]) {
            try {
                var workbook = XLSX.read(gk_fileData[filename], { type: 'base64' });
                var firstSheetName = workbook.SheetNames[0];
                var worksheet = workbook.Sheets[firstSheetName];

                // Convert sheet to JSON to filter blank rows
                var jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1, blankrows: false, defval: '' });
                // Filter out blank rows (rows where all cells are empty, null, or undefined)
                var filteredData = jsonData.filter(row => row.some(filledCell));

                // Heuristic to find the header row by ignoring rows with fewer filled cells than the next row
                var headerRowIndex = filteredData.findIndex((row, index) =>
                  row.filter(filledCell).length >= filteredData[index + 1]?.filter(filledCell).length
                );
                // Fallback
                if (headerRowIndex === -1 || headerRowIndex > 25) {
                  headerRowIndex = 0;
                }

                // Convert filtered JSON back to CSV
                var csv = XLSX.utils.aoa_to_sheet(filteredData.slice(headerRowIndex)); // Create a new sheet from filtered array of arrays
                csv = XLSX.utils.sheet_to_csv(csv, { header: 1 });
                return csv;
            } catch (e) {
                console.error(e);
                return "";
            }
        }
        return gk_fileData[filename] || "";
        }
        </script><!DOCTYPE html>
<html lang="pt-BR">
<head>
        // Rewarded interstitial
                
                show_8945630().then(() => {
                    // You need to add your user reward function here, which will be executed after the user watches the ad.
                        // For more details, please refer to the detailed instructions.
                            alert('You have seen an ad!');
                            })

                                    
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo de Quebra-Cabeça Numérico Profissional</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: #fff;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
            overflow: hidden;
        }
        h1 {
            font-size: 2.5em;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
            margin-bottom: 20px;
        }
        #menu, #game, #end-screen {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 20px;
            text-align: center;
            width: 100%;
            max-width: 600px;
        }
        #game-container {
            display: grid;
            grid-template-columns: repeat(6, 80px);
            gap: 8px;
            padding: 10px;
            justify-content: center;
        }
        .tile {
            width: 80px;
            height: 80px;
            background: linear-gradient(45deg, #ff6b6b, #ff8e53);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
            color: #fff;
            border-radius: 8px;
            cursor: pointer;
            transform-style: preserve-3d;
            transition: transform 0.3s, background 0.3s;
            perspective: 1000px;
        }
        .tile.flipped {
            transform: rotateY(180deg);
            background: linear-gradient(45deg, #4facfe, #00f2fe);
        }
        .tile.disabled {
            background: #ccc;
            cursor: default;
            transform: none;
        }
        #info {
            display: flex;
            justify-content: space-between;
            margin: 20px 0;
            font-size: 18px;
        }
        button {
            padding: 12px 24px;
            font-size: 16px;
            background: linear-gradient(45deg, #ff6b6b, #ff8e53);
            color: #fff;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: transform 0.2s, background 0.2s;
            margin: 10px;
        }
        button:hover {
            transform: scale(1.05);
            background: linear-gradient(45deg, #ff8e53, #ff6b6b);
        }
        #end-screen {
            display: none;
        }
        #particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }
        .particle {
            position: absolute;
            background: rgba(255, 255, 255, 0.7);
            border-radius: 50%;
            animation: particle 1s ease-out forwards;
        }
        @keyframes particle {
            to {
                transform: translateY(-100vh);
                opacity: 0;
            }
        }
        @media (max-width: 600px) {
            #game-container {
                grid-template-columns: repeat(6, 50px);
            }
            .tile {
                width: 50px;
                height: 50px;
                font-size: 18px;
            }
            h1 {
                font-size: 1.8em;
            }
        }
    </style>
</head>
<body>
    <div id="particles"></div>
    <div id="menu">
        <h1>Jogo de Quebra-Cabeça Numérico</h1>
        <p>Escolha a dificuldade:</p>
        <button onclick="startGame('easy')">Fácil</button>
        <button onclick="startGame('medium')">Médio</button>
        <button onclick="startGame('hard')">Difícil</button>
    </div>
    <div id="game" style="display: none;">
        <h1>Jogo de Quebra-Cabeça Numérico</h1>
        <div id="info">
            <span id="score">Pontuação: 0</span>
            <span id="time">Tempo: 0s</span>
            <span id="combo">Combo: 1x</span>
        </div>
        <div id="game-container"></div>
        <button onclick="pauseGame()">Pausar</button>
    </div>
    <div id="end-screen" style="display: none;">
        <h1 id="end-title"></h1>
        <p id="end-message"></p>
        <button onclick="showMenu()">Voltar ao Menu</button>
    </div>

    <script>
        const menu = document.getElementById('menu');
        const game = document.getElementById('game');
        const endScreen = document.getElementById('end-screen');
        const gameContainer = document.getElementById('game-container');
        const scoreDisplay = document.getElementById('score');
        const timeDisplay = document.getElementById('time');
        const comboDisplay = document.getElementById('combo');
        const endTitle = document.getElementById('end-title');
        const endMessage = document.getElementById('end-message');
        let tiles = [];
        let score = 0;
        let timeLeft = 0;
        let combo = 1;
        let selectedTiles = [];
        let gameInterval = null;
        let lastMatchTime = 0;
        let difficultySettings = {
            easy: { time: 150, baseScore: 10 },
            medium: { time: 100, baseScore: 15 },
            hard: { time: 60, baseScore: 20 }
        };
        let currentDifficulty = 'easy';

        function startGame(difficulty) {
            currentDifficulty = difficulty;
            timeLeft = difficultySettings[difficulty].time;
            score = 0;
            combo = 1;
            selectedTiles = [];
            tiles = [];
            gameContainer.innerHTML = '';
            const particlesContainer = document.getElementById('particles');
            if (particlesContainer) particlesContainer.innerHTML = '';
            menu.style.display = 'none';
            game.style.display = 'block';
            endScreen.style.display = 'none';
            scoreDisplay.textContent = `Pontuação: ${score}`;
            timeDisplay.textContent = `Tempo: ${timeLeft}s`;
            comboDisplay.textContent = `Combo: ${combo}x`;
            initializeTiles();
            startTimer();
        }

        function initializeTiles() {
            const numbers = Array.from({ length: 18 }, (_, i) => i + 1).flatMap(n => [n, n]);
            numbers.sort(() => Math.random() - 0.5);
            for (let i = 0; i < 36; i++) {
                const tile = document.createElement('div');
                tile.classList.add('tile');
                tile.dataset.number = numbers[i];
                tile.textContent = numbers[i];
                tile.addEventListener('click', () => handleTileClick(tile));
                gameContainer.appendChild(tile);
                tiles.push(tile);
            }
        }

        function handleTileClick(tile) {
            if (tile.classList.contains('disabled') || selectedTiles.includes(tile) || selectedTiles.length >= 2) return;
            tile.classList.add('flipped');
            selectedTiles.push(tile);
            playSound(440, 0.1);
            if (selectedTiles.length === 2) {
                checkMatch();
            }
        }

        function checkMatch() {
            const [tile1, tile2] = selectedTiles;
            if (tile1.dataset.number === tile2.dataset.number) {
                const now = Date.now();
                if (now - lastMatchTime < 3000) combo = Math.min(combo + 0.5, 3);
                lastMatchTime = now;
                score += difficultySettings[currentDifficulty].baseScore * combo;
                scoreDisplay.textContent = `Pontuação: ${score}`;
                comboDisplay.textContent = `Combo: ${combo}x`;
                tile1.classList.add('disabled');
                tile2.classList.add('disabled');
                tile1.classList.remove('flipped');
                tile2.classList.remove('flipped');
                createParticles(tile1.getBoundingClientRect());
                selectedTiles = [];
                checkWin();
            } else {
                setTimeout(() => {
                    tile1.classList.remove('flipped');
                    tile2.classList.remove('flipped');
                    combo = 1;
                    comboDisplay.textContent = `Combo: ${combo}x`;
                    selectedTiles = [];
                }, 1000);
            }
        }

        function startTimer() {
            if (gameInterval) clearInterval(gameInterval);
            gameInterval = setInterval(() => {
                timeLeft--;
                timeDisplay.textContent = `Tempo: ${timeLeft}s`;
                if (timeLeft <= 0) {
                    endGame(false);
                }
            }, 1000);
        }

        function checkWin() {
            if (tiles.every(tile => tile.classList.contains('disabled'))) {
                endGame(true);
            }
        }

        function endGame(won) {
            clearInterval(gameInterval);
            game.style.display = 'none';
            endScreen.style.display = 'block';
            const highScore = localStorage.getItem(`highScore_${currentDifficulty}`) || 0;
            if (won) {
                endTitle.textContent = 'Vitória!';
                endMessage.textContent = `Parabéns! Sua pontuação: ${score}.`;
                if (score > highScore) {
                    localStorage.setItem(`highScore_${currentDifficulty}`, score);
                    endMessage.textContent += ` Novo recorde!`;
                }
            } else {
                endTitle.textContent = 'Fim de Jogo';
                endMessage.textContent = `Tempo esgotado! Sua pontuação: ${score}.`;
            }
            endMessage.textContent += ` Melhor pontuação (${currentDifficulty}): ${highScore}.`;
        }

        function pauseGame() {
            clearInterval(gameInterval);
            alert('Jogo pausado. Clique em OK para continuar.');
            startTimer();
        }

        function showMenu() {
            endScreen.style.display = 'none';
            game.style.display = 'none';
            menu.style.display = 'block';
        }

        function createParticles(rect) {
            const particlesContainer = document.getElementById('particles');
            if (!particlesContainer) return;
            for (let i = 0; i < 10; i++) {
                const particle = document.createElement('div');
                particle.classList.add('particle');
                particle.style.width = '10px';
                particle.style.height = '10px';
                particle.style.left = `${rect.left + rect.width / 2}px`;
                particle.style.top = `${rect.top + rect.height / 2}px`;
                particle.style.transform = `translate(${Math.random() * 100 - 50}px, 0)`;
                particlesContainer.appendChild(particle);
                setTimeout(() => particle.remove(), 1000);
            }
        }

        function playSound(frequency, duration) {
            const ctx = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = ctx.createOscillator();
            oscillator.type = 'sine';
            oscillator.frequency.setValueAtTime(frequency, ctx.currentTime);
            oscillator.connect(ctx.destination);
            oscillator.start();
            oscillator.stop(ctx.currentTime + duration);
        }
    </script>
</body>
</html>
