<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Целевой Кликер | Яндекс.Игры</title>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            touch-action: manipulation;
        }
        
        body {
            font-family: 'Roboto', sans-serif;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            color: white;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
        }
        
        #gameContainer {
            width: 100%;
            max-width: 800px;
            height: 90vh;
            max-height: 600px;
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column;
        }
        
        #gameHeader {
            background: linear-gradient(to right, #ff416c, #ff4b2b);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 3px solid #ffcc00;
        }
        
        .scoreBoard {
            background-color: rgba(0, 0, 0, 0.6);
            padding: 10px 20px;
            border-radius: 30px;
            font-size: 1.5rem;
            font-weight: bold;
        }
        
        #gameArea {
            flex-grow: 1;
            position: relative;
            overflow: hidden;
        }
        
        #target {
            position: absolute;
            border-radius: 50%;
            cursor: pointer;
            transition: transform 0.1s ease;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            display: flex;
            justify-content: center;
            align-items: center;
            font-weight: bold;
            font-size: 1.8rem;
            color: white;
            text-shadow: 1px 1px 3px rgba(0,0,0,0.8);
        }
        
        #target:hover {
            transform: scale(1.05);
        }
        
        #target:active {
            transform: scale(0.95);
        }
        
        #startScreen, #endScreen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.85);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 10;
            text-align: center;
            padding: 20px;
        }
        
        #endScreen {
            display: none;
        }
        
        h1 {
            font-size: 2.8rem;
            margin-bottom: 20px;
            color: #ffcc00;
            text-shadow: 0 0 10px rgba(255, 204, 0, 0.5);
        }
        
        h2 {
            font-size: 2.2rem;
            margin-bottom: 30px;
            color: #4caf50;
        }
        
        p {
            font-size: 1.4rem;
            margin-bottom: 25px;
            max-width: 80%;
            line-height: 1.6;
        }
        
        button {
            background: linear-gradient(to right, #00c9ff, #92fe9d);
            border: none;
            padding: 15px 40px;
            font-size: 1.4rem;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            color: #1a237e;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
            margin-top: 20px;
        }
        
        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
        }
        
        button:active {
            transform: translateY(1px);
        }
        
        #timerBar {
            height: 12px;
            background-color: #333;
            position: relative;
        }
        
        #timerProgress {
            height: 100%;
            background: linear-gradient(to right, #4caf50, #ffeb3b, #f44336);
            width: 100%;
            transition: width 0.5s linear;
        }
        
        #particlesCanvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }
        
        @media (max-width: 600px) {
            h1 {
                font-size: 2.2rem;
            }
            
            h2 {
                font-size: 1.8rem;
            }
            
            p {
                font-size: 1.2rem;
            }
            
            .scoreBoard {
                font-size: 1.2rem;
                padding: 8px 15px;
            }
            
            button {
                padding: 12px 30px;
                font-size: 1.2rem;
            }
        }
    </style>
</head>
<body>
    <div id="gameContainer">
        <div id="gameHeader">
            <div class="scoreBoard">Очки: <span id="score">0</span></div>
            <div class="scoreBoard">Время: <span id="timer">30</span>с</div>
        </div>
        
        <div id="timerBar">
            <div id="timerProgress"></div>
        </div>
        
        <div id="gameArea">
            <canvas id="particlesCanvas"></canvas>
            <div id="target"></div>
            
            <div id="startScreen">
                <h1>🎯 Целевой Кликер 🎯</h1>
                <p>Кликайте на мишени, которые появляются на экране! Набирайте как можно больше очков за 30 секунд!</p>
                <p>Чем быстрее вы кликаете, тем больше очков получаете!</p>
                <button id="startButton">Играть!</button>
            </div>
            
            <div id="endScreen">
                <h1>Игра Окончена!</h1>
                <h2>Ваш счет: <span id="finalScore">0</span></h2>
                <p id="resultMessage">Попробуйте еще раз, чтобы побить свой рекорд!</p>
                <button id="restartButton">Играть снова</button>
            </div>
        </div>
    </div>

    <script>
        // Элементы игры
        const gameContainer = document.getElementById('gameContainer');
        const gameArea = document.getElementById('gameArea');
        const target = document.getElementById('target');
        const scoreElement = document.getElementById('score');
        const timerElement = document.getElementById('timer');
        const timerProgress = document.getElementById('timerProgress');
        const startScreen = document.getElementById('startScreen');
        const endScreen = document.getElementById('endScreen');
        const finalScoreElement = document.getElementById('finalScore');
        const resultMessage = document.getElementById('resultMessage');
        const startButton = document.getElementById('startButton');
        const restartButton = document.getElementById('restartButton');
        const particlesCanvas = document.getElementById('particlesCanvas');
        const ctx = particlesCanvas.getContext('2d');
        
        // Переменные игры
        let score = 0;
        let timeLeft = 30;
        let gameActive = false;
        let gameTimer;
        let targetSize = 80;
        let particles = [];
        let highScore = localStorage.getItem('highScore') || 0;
        
        // Установка размеров canvas
        function setCanvasSize() {
            particlesCanvas.width = gameArea.clientWidth;
            particlesCanvas.height = gameArea.clientHeight;
        }
        
        // Создание новой мишени
        function createTarget() {
            if (!gameActive) return;
            
            // Размер мишени (случайный в пределах)
            const size = targetSize + Math.random() * 40;
            
            // Позиция мишени (учитывая границы)
            const maxX = gameArea.clientWidth - size;
            const maxY = gameArea.clientHeight - size;
            
            const x = Math.floor(Math.random() * maxX);
            const y = Math.floor(Math.random() * maxY);
            
            // Цвет мишени (случайный яркий цвет)
            const hue = Math.floor(Math.random() * 360);
            const color = `hsl(${hue}, 90%, 60%)`;
            
            // Применяем стили
            target.style.width = `${size}px`;
            target.style.height = `${size}px`;
            target.style.left = `${x}px`;
            target.style.top = `${y}px`;
            target.style.backgroundColor = color;
            target.style.boxShadow = `0 0 0 4px white, 0 0 20px ${color}`;
            target.style.border = `4px solid ${color}`;
            
            // Текст внутри мишени
            target.textContent = "+1";
        }
        
        // Обработчик клика по мишени
        function hitTarget() {
            if (!gameActive) return;
            
            // Увеличиваем счет
            score++;
            scoreElement.textContent = score;
            
            // Создаем частицы эффекта
            createParticles(parseInt(target.style.left) + target.offsetWidth/2, 
                            parseInt(target.style.top) + target.offsetHeight/2, 
                            target.style.backgroundColor);
            
            // Создаем новую мишень
            createTarget();
            
            // Уменьшаем размер мишени для увеличения сложности
            targetSize = Math.max(40, targetSize * 0.98);
        }
        
        // Создание частиц для эффекта
        function createParticles(x, y, color) {
            const count = 20;
            
            for (let i = 0; i < count; i++) {
                particles.push({
                    x: x,
                    y: y,
                    size: Math.random() * 6 + 2,
                    speedX: (Math.random() - 0.5) * 10,
                    speedY: (Math.random() - 0.5) * 10,
                    color: color,
                    life: 30
                });
            }
        }
        
        // Обновление частиц
        function updateParticles() {
            for (let i = 0; i < particles.length; i++) {
                const p = particles[i];
                p.x += p.speedX;
                p.y += p.speedY;
                p.life--;
                
                if (p.life <= 0) {
                    particles.splice(i, 1);
                    i--;
                }
            }
        }
        
        // Отрисовка частиц
        function drawParticles() {
            ctx.clearRect(0, 0, particlesCanvas.width, particlesCanvas.height);
            
            for (const p of particles) {
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                ctx.fillStyle = p.color;
                ctx.fill();
                
                // Эффект свечения
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size * 2, 0, Math.PI * 2);
                const gradient = ctx.createRadialGradient(
                    p.x, p.y, 0, 
                    p.x, p.y, p.size * 2
                );
                gradient.addColorStop(0, p.color);
                gradient.addColorStop(1, 'rgba(255,255,255,0)');
                ctx.fillStyle = gradient;
                ctx.fill();
            }
        }
        
        // Обновление таймера
        function updateTimer() {
            timeLeft--;
            timerElement.textContent = timeLeft;
            
            // Обновление прогресс-бара
            const percent = (timeLeft / 30) * 100;
            timerProgress.style.width = `${percent}%`;
            
            if (timeLeft <= 0) {
                endGame();
            }
        }
        
        // Начало игры
        function startGame() {
            // Сброс значений
            score = 0;
            timeLeft = 30;
            targetSize = 80;
            scoreElement.textContent = score;
            timerElement.textContent = timeLeft;
            timerProgress.style.width = '100%';
            
            // Активируем игру
            gameActive = true;
            startScreen.style.display = 'none';
            endScreen.style.display = 'none';
            
            // Создаем первую мишень
            createTarget();
            
            // Запускаем таймер
            gameTimer = setInterval(updateTimer, 1000);
            
            // Запускаем анимацию частиц
            function animate() {
                if (!gameActive) return;
                
                updateParticles();
                drawParticles();
                requestAnimationFrame(animate);
            }
            
            animate();
        }
        
        // Завершение игры
        function endGame() {
            gameActive = false;
            clearInterval(gameTimer);
            
            // Обновляем рекорд
            if (score > highScore) {
                highScore = score;
                localStorage.setItem('highScore', highScore);
                resultMessage.textContent = `Новый рекорд! 🏆`;
            } else {
                resultMessage.textContent = `Ваш рекорд: ${highScore} очков!`;
            }
            
            // Показываем экран завершения
            finalScoreElement.textContent = score;
            endScreen.style.display = 'flex';
        }
        
        // Инициализация игры
        function init() {
            setCanvasSize();
            
            // Обработчики событий
            target.addEventListener('click', hitTarget);
            startButton.addEventListener('click', startGame);
            restartButton.addEventListener('click', startGame);
            
            // Обработчик изменения размера окна
            window.addEventListener('resize', setCanvasSize);
        }
        
        // Запуск игры после загрузки страницы
        window.onload = init;
    </script>
</body>
</html># Kocmo
