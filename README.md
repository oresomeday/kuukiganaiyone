<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>自転車に乗る男</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            margin: 0;
            padding: 5px;
            min-height: 100vh;
            color: white;
            overflow-x: hidden;
            user-select: none;
            -webkit-user-select: none;
            -webkit-touch-callout: none;
        }
        
        .game-container {
            max-width: 100%;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 10px;
            backdrop-filter: blur(10px);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }
        
        h1 {
            font-size: 2em;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        /* ミニゲーム用スタイル */
        .mini-game {
            display: block;
        }
        
        .mini-game.hidden {
            display: none;
        }
        
        .bike-game-area {
            width: 100%;
            height: 300px;
            background: linear-gradient(to bottom, #87CEEB 0%, #98FB98 100%);
            position: relative;
            border: 3px solid #fff;
            border-radius: 15px;
            overflow: hidden;
            margin: 10px 0;
            touch-action: none;
        }
        
        .road {
            position: absolute;
            bottom: 0;
            width: 100%;
            height: 40px;
            background: #555;
            border-top: 3px dashed #fff;
        }
        
        .bike {
            position: absolute;
            bottom: 50px;
            right: 30px;
            font-size: 2em;
            transition: top 0.15s ease;
            z-index: 10;
        }
        
        .obstacle-group {
            position: absolute;
            left: -50px;
            width: 20px;
            height: 260px;
            top: 30px;
        }
        
        .obstacle {
            position: absolute;
            width: 20px;
            background: #DC143C;
            border-radius: 10px;
            border: 2px solid #8B0000;
            box-shadow: 0 0 10px rgba(220, 20, 60, 0.5);
        }
        
        .obstacle.top {
            top: 0;
            height: 70px;
        }
        
        .obstacle.middle {
            top: 100px;
            height: 50px;
        }
        
        .obstacle.bottom {
            bottom: 0;
            height: 70px;
        }
        
        .gap-indicator {
            position: absolute;
            width: 25px;
            height: 50px;
            background: rgba(0, 255, 0, 0.3);
            border: 2px dashed #00FF00;
            border-radius: 10px;
            left: -2px;
        }
        
        .touch-controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 10px 0;
        }
        
        .touch-button {
            background: linear-gradient(45deg, #4CAF50, #45a049);
            color: white;
            border: none;
            padding: 15px;
            font-size: 1.8em;
            border-radius: 50%;
            cursor: pointer;
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }
        
        .touch-button:active {
            transform: scale(0.9);
            background: linear-gradient(45deg, #45a049, #4CAF50);
        }
        
        .control-button {
            background: linear-gradient(45deg, #4CAF50, #45a049);
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1.1em;
            border-radius: 25px;
            cursor: pointer;
            margin: 10px;
            transition: all 0.3s ease;
            touch-action: manipulation;
        }
        
        .control-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }
        
        .retry-button {
            background: linear-gradient(45deg, #ff6b6b, #ee5a24);
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1.1em;
            border-radius: 25px;
            cursor: pointer;
            margin: 10px;
            transition: all 0.3s ease;
            touch-action: manipulation;
        }
        
        /* スロットゲーム用スタイル */
        .slot-game {
            display: none;
        }
        
        .slot-game.show {
            display: block;
        }
        
        .slot-machine {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: 20px 0;
            flex-wrap: wrap;
        }
        
        .reel-container {
            width: 80px;
            height: 80px;
            border: 4px solid #fff;
            border-radius: 15px;
            overflow: hidden;
            position: relative;
            background: linear-gradient(45deg, #ff6b6b, #feca57);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }
        
        .reel {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1em;
            font-weight: bold;
            color: #333;
            transition: transform 0.1s linear;
        }
        
        .spin-button {
            background: linear-gradient(45deg, #ff6b6b, #ee5a24);
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1.2em;
            border-radius: 50px;
            cursor: pointer;
            margin: 15px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            touch-action: manipulation;
        }
        
        .spin-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
        }
        
        .spin-button:disabled {
            background: #999;
            cursor: not-allowed;
            transform: none;
        }
        
        .result {
            margin: 15px 0;
            font-size: 1em;
            min-height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 10px;
        }
        
        .sentence {
            font-size: 1.3em;
            color: #feca57;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .remaining-turns {
            font-size: 1em;
            margin: 10px 0;
            background: rgba(255, 255, 255, 0.3);
            padding: 8px 16px;
            border-radius: 20px;
            display: inline-block;
        }
        
        /* 紙吹雪エフェクト */
        .confetti-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 999;
            display: none;
        }
        
        .confetti-overlay.show {
            display: block;
        }
        
        .confetti-piece {
            position: absolute;
            font-size: 2em;
            animation: confettiFall 3s linear;
            pointer-events: none;
        }
        
        @keyframes confettiFall {
            0% { 
                transform: translateY(-100vh) rotate(0deg);
                opacity: 1;
            }
            100% { 
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }
        
        .ending-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #ff6b6b 0%, #feca57 25%, #48c6ef 50%, #ff6b6b 75%, #feca57 100%);
            background-size: 400% 400%;
            animation: gradientShift 3s ease infinite;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            overflow-y: auto;
            padding: 20px;
        }
        
        .ending-screen.show {
            display: flex;
        }
        
        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        
        .ending-message {
            font-size: 2em;
            font-weight: bold;
            text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.5);
            margin-bottom: 20px;
            text-align: center;
            line-height: 1.4;
            max-width: 90%;
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
            white-space: pre-line;
            /* 自動スクロール用 */
            animation: scrollUp 20s linear infinite;
            transform: translateY(50vh);
        }
        
        /* 自動スクロールアニメーション */
        @keyframes scrollUp {
            0% {
                transform: translateY(50vh);
            }
            100% {
                transform: translateY(-100%);
            }
        }
        
        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
            60% { transform: translateY(-5px); }
        }
        
        /* スマホ対応 */
        @media (max-width: 768px) {
            .game-container { padding: 5px; }
            .bike-game-area { height: 250px; margin: 5px 0; }
            .bike { font-size: 1.8em; bottom: 45px; right: 25px; }
            .touch-button { width: 50px; height: 50px; font-size: 1.5em; }
            .ending-message {
                font-size: 1.5em;
                padding: 15px;
                animation: scrollUp 25s linear infinite;
            }
            .reel-container { width: 70px; height: 70px; }
            .reel { font-size: 0.9em; }
            .obstacle-group { height: 210px; top: 30px; }
            .obstacle.top { height: 50px; }
            .obstacle.middle { height: 35px; top: 75px; }
            .obstacle.bottom { height: 50px; bottom: 0; }
            .gap-indicator { height: 35px; }
        }
        
        @media (max-width: 480px) {
            body { padding: 2px; }
            .game-container { padding: 3px; border-radius: 10px; }
            .bike-game-area { height: 220px; }
            .ending-message { 
                font-size: 1.2em;
                animation: scrollUp 30s linear infinite;
            }
            h1 { font-size: 1.5em; margin-bottom: 10px; }
            .obstacle-group { height: 180px; top: 30px; }
            .obstacle.top { height: 45px; }
            .obstacle.middle { height: 30px; top: 70px; }
            .obstacle.bottom { height: 45px; bottom: 0; }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>🚴‍♂️ 自転車に乗る男</h1>
        
        <!-- ミニゲーム部分 -->
        <div class="mini-game" id="miniGame">
            <div class="bike-game-area" id="gameArea">
                <div class="road"></div>
                <div class="bike" id="bike">🚴‍♂️</div>
            </div>
            
            <div class="touch-controls">
                <button class="touch-button" id="upButton">↑</button>
                <button class="touch-button" id="downButton">↓</button>
            </div>
            
            <div class="game-controls">
                <button class="control-button" id="startButton" onclick="startBikeGame()">スタート</button>
            </div>
        </div>
        
        <!-- スロットゲーム部分 -->
        <div class="slot-game" id="slotGame">
            <div class="slot-machine">
                <div class="reel-container">
                    <div class="reel" id="reel1">空気</div>
                </div>
                <div class="reel-container">
                    <div class="reel" id="reel2">が</div>
                </div>
                <div class="reel-container">
                    <div class="reel" id="reel3">ない</div>
                </div>
            </div>
            
            <button class="spin-button" id="spinButton" onclick="spin()">スピン！</button>
            
            <div class="remaining-turns">残り回数: <span id="remainingTurns">0</span></div>
            
            <div class="result" id="result">
                <div class="sentence" id="sentence">空気がない</div>
            </div>
            
                        <button class="retry-button" onclick="restartGame()" id="retryButton">最初からやり直す</button>
        </div>
    </div>

    <!-- 紙吹雪オーバーレイ -->
    <div class="confetti-overlay" id="confettiOverlay"></div>

    <!-- エンディング画面 -->
    <div class="ending-screen" id="endingScreen">
        <div class="ending-message" id="endingMessage">
            <!-- ここにA,B,C,D,Eのメッセージがランダムで表示されます -->
        </div>
        
        <button class="retry-button" onclick="restartFromEnding()">最初からやり直す</button>
    </div>

    <script>
        // ミニゲーム関連の変数
        let bikeGameRunning = false;
        let bikePosition = 1; // 0=上、1=中、2=下
        let obstacleGroups = [];
        let passCount = 0;
        let earnedTurns = 0;
        let gameSpeed = 3; // 初期スピード
        let speedLevel = 1;
        let obstacleSpawnTimer = 0;
        
        // スロットゲーム関連の変数
        const reel1Words = ['空気', '愛', '命', '髪'];
        const reel2Words = ['が', 'は', 'も'];
        const reel3Words = ['ある', 'ない'];
        let isSpinning = false;
        let remainingTurns = 0;
        
        // エンディングメッセージ（後で編集用）
        const endingMessages = {
            A: `メッセージA（ここに1000文字程度のメッセージを後で追加）

複数行でも大丈夫です。
バッククォートを使用しているので、
引用符も自由に使えます。`,
            
            B: `メッセージB（ここに1000文字程度のメッセージを後で追加）

複数行でも大丈夫です。`,
            
            C: `メッセージC（ここに1000文字程度のメッセージを後で追加）

複数行でも大丈夫です。`,
            
            D: `メッセージD（ここに1000文字程度のメッセージを後で追加）

複数行でも大丈夫です。`,
            
            E: `メッセージE（ここに1000文字程度のメッセージを後で追加）

複数行でも大丈夫です。`
        };
        
        // タッチコントロール設定
        function setupTouchControls() {
            const upButton = document.getElementById('upButton');
            const downButton = document.getElementById('downButton');
            
            upButton.addEventListener('touchstart', (e) => {
                e.preventDefault();
                moveUp();
            });
            
            upButton.addEventListener('mousedown', (e) => {
                e.preventDefault();
                moveUp();
            });
            
            downButton.addEventListener('touchstart', (e) => {
                e.preventDefault();
                moveDown();
            });
            
            downButton.addEventListener('mousedown', (e) => {
                e.preventDefault();
                moveDown();
            });
        }
        
        function moveUp() {
            if (bikeGameRunning && bikePosition > 0) {
                bikePosition--;
                updateBikePosition();
            }
        }
        
        function moveDown() {
            if (bikeGameRunning && bikePosition < 2) {
                bikePosition++;
                updateBikePosition();
            }
        }
        
        // 自転車ゲームの開始
        function startBikeGame() {
            if (bikeGameRunning) return;
            
            bikeGameRunning = true;
            obstacleGroups = [];
            passCount = 0;
            earnedTurns = 0;
            bikePosition = 1;
            gameSpeed = 3;
            speedLevel = 1;
            obstacleSpawnTimer = 0;
            
            document.getElementById('startButton').disabled = true;
            document.getElementById('startButton').textContent = 'プレイ中...';
            
            const existingObstacles = document.querySelectorAll('.obstacle-group');
            existingObstacles.forEach(obstacle => obstacle.remove());
            
            document.addEventListener('keydown', handleKeyPress);
            
            updateBikePosition();
            
            requestAnimationFrame(gameLoop);
        }
        
        function handleKeyPress(event) {
            if (!bikeGameRunning) return;
            
            if (event.key === 'ArrowUp') {
                event.preventDefault();
                moveUp();
            } else if (event.key === 'ArrowDown') {
                event.preventDefault();
                moveDown();
            }
        }
        
        function updateBikePosition() {
            const bike = document.getElementById('bike');
            // スマホでの位置調整
            const isMobile = window.innerWidth <= 768;
            const positions = isMobile ? [60, 105, 150] : [70, 125, 180];
            bike.style.top = positions[bikePosition] + 'px';
        }
        
        function createObstacleGroup() {
            const gameArea = document.getElementById('gameArea');
            
            const gapPosition = Math.floor(Math.random() * 3);
            
            const obstacleGroup = document.createElement('div');
            obstacleGroup.className = 'obstacle-group';
            obstacleGroup.style.left = '-50px';
            
            const gapIndicator = document.createElement('div');
            gapIndicator.className = 'gap-indicator';
            
            if (gapPosition === 0) {
                gapIndicator.style.top = '30px';
            } else if (gapPosition === 1) {
                gapIndicator.style.top = '85px';
            } else {
                gapIndicator.style.top = '140px';
            }
            
            obstacleGroup.appendChild(gapIndicator);
            
            for (let i = 0; i < 3; i++) {
                if (i !== gapPosition) {
                    const obstacle = document.createElement('div');
                    obstacle.className = 'obstacle';
                    
                    if (i === 0) {
                        obstacle.classList.add('top');
                    } else if (i === 1) {
                        obstacle.classList.add('middle');
                    } else {
                        obstacle.classList.add('bottom');
                    }
                    
                    obstacleGroup.appendChild(obstacle);
                }
            }
            
            gameArea.appendChild(obstacleGroup);
            
            obstacleGroups.push({
                element: obstacleGroup,
                x: -50,
                gapPosition: gapPosition,
                passed: false
            });
        }
        
        function updateObstacleGroups() {
            const gameAreaWidth = document.getElementById('gameArea').offsetWidth;
            
            obstacleGroups.forEach((group, index) => {
                group.x += gameSpeed;
                group.element.style.left = group.x + 'px';
                
                const bikeX = gameAreaWidth - 55;
                
                if (!group.passed && group.x > bikeX - 20 && group.x < bikeX + 20) {
                    if (bikePosition === group.gapPosition) {
                        group.passed = true;
                        passCount++;
                        earnedTurns++;
                        
                        if (passCount % 3 === 0) {
                            gameSpeed += 0.5;
                            speedLevel++;
                        }
                        
                        const obstacles = group.element.querySelectorAll('.obstacle');
                        obstacles.forEach(obstacle => {
                            obstacle.style.background = '#32CD32';
                            obstacle.style.borderColor = '#228B22';
                        });
                    } else {
                        gameOver();
                        return;
                    }
                }
                
                if (group.x > gameAreaWidth + 100) {
                    group.element.remove();
                    obstacleGroups.splice(index, 1);
                }
            });
        }
        
        function gameOver() {
            bikeGameRunning = false;
            document.removeEventListener('keydown', handleKeyPress);
            
            document.getElementById('startButton').disabled = false;
            document.getElementById('startButton').textContent = 'スタート';
            
            setTimeout(() => {
                switchToSlot();
            }, 1000);
        }
        
        function gameLoop() {
            if (!bikeGameRunning) return;
            
            obstacleSpawnTimer++;
            const spawnInterval = Math.max(80 - speedLevel * 5, 40);
            
            if (obstacleSpawnTimer > spawnInterval) {
                createObstacleGroup();
                obstacleSpawnTimer = 0;
            }
            
            updateObstacleGroups();
            
            requestAnimationFrame(gameLoop);
        }
        
        function switchToSlot() {
            obstacleGroups.forEach(group => {
                group.element.remove();
            });
            obstacleGroups = [];
            
            document.getElementById('miniGame').classList.add('hidden');
            document.getElementById('slotGame').classList.add('show');
            remainingTurns = earnedTurns;
            document.getElementById('remainingTurns').textContent = remainingTurns;
            updateSentence();
        }
        
        function restartGame() {
            document.getElementById('endingScreen').classList.remove('show');
            document.getElementById('confettiOverlay').classList.remove('show');
            
            document.getElementById('slotGame').classList.remove('show');
            
            document.getElementById('miniGame').classList.remove('hidden');
            
            isSpinning = false;
            remainingTurns = 0;
            passCount = 0;
            earnedTurns = 0;
            bikePosition = 1;
            gameSpeed = 3;
            speedLevel = 1;
            obstacleSpawnTimer = 0;
            bikeGameRunning = false;
            
            document.getElementById('remainingTurns').textContent = remainingTurns;
            
            document.getElementById('spinButton').disabled = false;
            
            document.getElementById('startButton').disabled = false;
            document.getElementById('startButton').textContent = 'スタート';
            
            const existingObstacles = document.querySelectorAll('.obstacle-group');
            existingObstacles.forEach(obstacle => obstacle.remove());
            
            updateSentence();
        }
        
        function restartFromEnding() {
            restartGame();
        }
        
        // 紙吹雪エフェクト
        function showConfetti() {
            const overlay = document.getElementById('confettiOverlay');
            overlay.classList.add('show');
            overlay.innerHTML = '';
            
            const confettiTypes = ['🎉', '🎊', '✨', '🌟', '💫', '🎈'];
            
            for (let i = 0; i < 50; i++) {
                setTimeout(() => {
                    const confetti = document.createElement('div');
                    confetti.className = 'confetti-piece';
                    confetti.textContent = confettiTypes[Math.floor(Math.random() * confettiTypes.length)];
                    confetti.style.left = Math.random() * 100 + '%';
                    confetti.style.animationDelay = Math.random() * 2 + 's';
                    confetti.style.animationDuration = (Math.random() * 2 + 2) + 's';
                    overlay.appendChild(confetti);
                    
                    setTimeout(() => {
                        if (confetti.parentNode) {
                            confetti.parentNode.removeChild(confetti);
                        }
                    }, 5000);
                }, i * 50);
            }
        }
        
        // スロットゲーム関数
        function getRandomWord(wordArray) {
            return wordArray[Math.floor(Math.random() * wordArray.length)];
        }
        
        function updateSentence() {
            const word1 = document.getElementById('reel1').textContent;
            const word2 = document.getElementById('reel2').textContent;
            const word3 = document.getElementById('reel3').textContent;
            
            document.getElementById('sentence').textContent = word1 + word2 + word3;
        }
        
        function checkWin(word1, word2, word3) {
            const sentence = word1 + word2 + word3;
            
            if (sentence === '空気がない') {
                // 紙吹雪を表示
                showConfetti();
                
                // 3秒後にエンディングを表示
                setTimeout(() => {
                    showEnding();
                }, 3000);
                
                return -1; // 特別な値でエンディング表示を示す
            }
            
            return 0;
        }
        
        function getEncouragementMessage(word1, word2, word3) {
            const messages = [
                'おしい！もう少し！',
                'あと一歩！',
                'がんばって！',
                'いい感じ！',
                'もう一回！',
                'きっと当たる！',
                'チャンスはまだある！',
                '諦めないで！'
            ];
            
            if (word1 === '空気') {
                return 'おしい！「空気」は合ってる！';
            } else if (word2 === 'が') {
                return 'いいね！「が」は正解！';
            } else if (word3 === 'ない') {
                return 'もう少し！「ない」は当たり！';
            }
            
            return messages[Math.floor(Math.random() * messages.length)];
        }
        
        function showEnding() {
            // ランダムにA,B,C,D,Eのいずれかを選択
            const messageKeys = ['A', 'B', 'C', 'D', 'E'];
            const randomKey = messageKeys[Math.floor(Math.random() * messageKeys.length)];
            
            document.getElementById('endingMessage').textContent = endingMessages[randomKey];
            document.getElementById('endingScreen').classList.add('show');
        }
        
        function animateReel(reelId, wordArray, duration) {
            return new Promise((resolve) => {
                const reel = document.getElementById(reelId);
                const startTime = Date.now();
                let position = 0;
                
                const animate = () => {
                    const elapsed = Date.now() - startTime;
                    
                    if (elapsed < duration) {
                        position += 20;
                        if (position >= 80) {
                            position = 0;
                            reel.textContent = getRandomWord(wordArray);
                        }
                        reel.style.transform = `translateY(${position}px)`;
                        requestAnimationFrame(animate);
                    } else {
                        reel.style.transform = 'translateY(0)';
                        reel.textContent = getRandomWord(wordArray);
                        resolve();
                    }
                };
                
                animate();
            });
        }
        
        async function spin() {
            if (isSpinning || remainingTurns <= 0) return;
            
            isSpinning = true;
            remainingTurns--;
                      document.getElementById('remainingTurns').textContent = remainingTurns;
            document.getElementById('spinButton').disabled = true;
            document.getElementById('result').innerHTML = '<div style="color: #feca57;">スピン中...</div>';
            
            try {
                await animateReel('reel1', reel1Words, 1000);
                await new Promise(resolve => setTimeout(resolve, 200));
                
                await animateReel('reel2', reel2Words, 1000);
                await new Promise(resolve => setTimeout(resolve, 200));
                
                await animateReel('reel3', reel3Words, 1000);
                
                const word1 = document.getElementById('reel1').textContent;
                const word2 = document.getElementById('reel2').textContent;
                const word3 = document.getElementById('reel3').textContent;
                
                const points = checkWin(word1, word2, word3);
                
                updateSentence();
                
                if (points === -1) {
                    // 大当たり表示
                    document.getElementById('result').innerHTML = '<div style="color: #feca57;">🎉 大当たり！「空気がない」 🎉</div>';
                    return;
                } else {
                    let resultText = getEncouragementMessage(word1, word2, word3);
                    
                    if (remainingTurns <= 0) {
                        resultText += ' 回数終了！やり直してもっと回数を稼ごう！';
                    }
                    
                    document.getElementById('result').innerHTML = '<div style="color: #feca57;">' + resultText + '</div>';
                }
                
            } catch (error) {
                document.getElementById('result').innerHTML = '<div style="color: #ff6b6b;">もう一度試してみて！</div>';
            } finally {
                isSpinning = false;
                // エンディング画面が表示されていない場合のみスピンボタンを有効化
                if (remainingTurns > 0 && !document.getElementById('endingScreen').classList.contains('show')) {
                    document.getElementById('spinButton').disabled = false;
                }
            }
        }
        
        // 初期設定
        setupTouchControls();
        updateSentence();
    </script>
</body>
</html>
