<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pac-Man Flappy</title>
    <style>
           
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Arial Rounded MT Bold', 'Arial', sans-serif;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 10px;
            color: #333;
        }
        
        .container {
            max-width: 320px;
            width: 100%;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
            overflow: hidden;
            padding: 15px;
        }
        
        .game-title {
            text-align: center;
            margin-bottom: 15px;
            padding: 8px;
            background: linear-gradient(45deg, #FF5722, #FF9800);
            border-radius: 12px;
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
        }
        
        .game-title h1 {
            color: white;
            font-size: 1.8rem;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
            letter-spacing: 1px;
        }
        
        .game-screen {
            position: relative;
            border: 2px solid #333;
            border-radius: 8px;
            overflow: hidden;
            background: #87CEEB;
            box-shadow: inset 0 0 15px rgba(0, 0, 0, 0.1);
        }
        
        #game {
            display: block;
            width: 100%;
            background: #87CEEB;
        }
        
        .game-description {
            text-align: center;
            padding: 8px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            font-weight: bold;
            margin-top: 8px;
            border-radius: 8px;
            font-size: 0.9rem;
        }
        
        #level-indicator {
            position: absolute;
            top: 8px;
            left: 8px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 4px 8px;
            border-radius: 15px;
            font-size: 0.8rem;
            font-weight: bold;
        }
        
        #score-display {
            position: absolute;
            top: 8px;
            right: 8px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 4px 8px;
            border-radius: 15px;
            font-size: 0.8rem;
            font-weight: bold;
        }
        
        #pipes-count {
            position: absolute;
            top: 35px;
            left: 8px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 3px 8px;
            border-radius: 15px;
            font-size: 0.7rem;
        }
        
        #menu, #level-select, #game-over, #level-up, #victory {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(255, 255, 255, 0.98);
            padding: 20px;
            border-radius: 12px;
            text-align: center;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.25);
            width: 85%;
            max-width: 260px;
        }
        
        #level-select, #game-over, #level-up, #victory {
            display: none;
        }
        
        #victory {
            background: linear-gradient(45deg, #FFD700, #FFA000);
            color: #8B4513;
            box-shadow: 0 0 25px rgba(255, 215, 0, 0.4);
        }
        
        h2 {
            color: #FF5722;
            margin-bottom: 12px;
            font-size: 1.5rem;
        }
        
        h3 {
            color: #333;
            margin-bottom: 12px;
            font-size: 1.2rem;
        }
        
        p {
            margin-bottom: 12px;
            line-height: 1.4;
            font-size: 0.9rem;
        }
        
        button {
            background: linear-gradient(45deg, #4CAF50, #45a049);
            color: white;
            border: none;
            padding: 10px 16px;
            margin: 6px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 0.9rem;
            font-weight: bold;
            transition: all 0.3s;
            width: 90%;
            max-width: 180px;
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.3);
        }
        
        .level-btn {
            width: 90%;
            max-width: 180px;
            margin: 4px;
            padding: 8px;
            font-size: 0.8rem;
        }
        
        .level-btn.lvl1 { background: linear-gradient(45deg, #4CAF50, #45a049); }
        .level-btn.lvl2 { background: linear-gradient(45deg, #FF9800, #F57C00); }
        .level-btn.lvl3 { background: linear-gradient(45deg, #F44336, #D32F2F); }
        
        #final-score {
            font-size: 1.3rem;
            font-weight: bold;
            color: #FF5722;
            margin: 8px 0;
        }
        
        #max-level {
            font-size: 1rem;
            font-weight: bold;
            color: #2196F3;
            margin: 4px 0;
        }
        
        .instructions {
            background: rgba(255, 255, 255, 0.8);
            padding: 12px;
            border-radius: 8px;
            margin-top: 12px;
            font-size: 0.8rem;
            text-align: left;
        }
        
        .instructions h4 {
            color: #FF5722;
            margin-bottom: 6px;
            text-align: center;
            font-size: 0.9rem;
        }
        
        .instructions ul {
            padding-left: 18px;
        }
        
        .instructions li {
            margin-bottom: 4px;
        }
        
        .level-grid {
            display: flex;
            flex-direction: column;
            gap: 6px;
            margin: 12px 0;
        }
        
        .progress-bar {
            width: 100%;
            height: 8px;
            background: #e0e0e0;
            border-radius: 4px;
            margin: 8px 0;
            overflow: hidden;
        }
        
        .progress {
            height: 100%;
            background: linear-gradient(90deg, #4CAF50, #FF9800, #F44336);
            border-radius: 4px;
            transition: width 0.5s ease;
        }
        
        .victory-title {
            font-size: 1.6rem;
            font-weight: bold;
            color: #D32F2F;
            margin-bottom: 12px;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
        }
        
        .victory-message {
            font-size: 1.1rem;
            color: #5D4037;
            margin: 12px 0;
            font-weight: bold;
        }
        
        .secret-message {
            background: rgba(255, 255, 255, 0.9);
            padding: 10px;
            border-radius: 8px;
            margin: 10px 0;
            border: 2px dashed #FF5722;
            font-family: 'Courier New', monospace;
            font-weight: bold;
        }
        
        .confetti {
            position: absolute;
            width: 8px;
            height: 8px;
            background: #FFD700;
            border-radius: 50%;
            animation: fall linear infinite;
        }
        
        @keyframes fall {
            to {
                transform: translateY(400px) rotate(360deg);
                opacity: 0;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="game-title">
            <h1>PAC-MAN FLAPPY</h1>
        </div>
        <div class="game-screen">
            <canvas id="game" width="280" height="400"></canvas>
            <div id="level-indicator">Niveau: 1</div>
            <div id="score-display">Score: 0</div>
            <div id="pipes-count">Tuyaux: 0/15</div>
            
            <!-- Menu principal -->
            <div id="menu">
                <h2>Pac-Man Flappy</h2>
                <p>Guidez Pac-Man à travers les tuyaux !</p>
                <button id="start-btn">Jouer Niveau 1</button>
                <button id="level-btn">Choisir Niveau</button>
                <div class="progress-bar">
                    <div class="progress" id="global-progress" style="width: 0%"></div>
                </div>
                <div class="instructions">
                    <h4>Objectifs</h4>
                    <ul>
                        <li>Niveau 1: 15 tuyaux</li>
                        <li>Niveau 2: 12 tuyaux</li>
                        <li>Niveau 3: 10 tuyaux</li>
                    </ul>
                </div>
            </div>
            
            <!-- Sélection de niveau -->
            <div id="level-select">
                <h2>Choisir un Niveau</h2>
                <p>Progression: <span id="progress-percent">0%</span></p>
                <div class="progress-bar">
                    <div class="progress" id="select-progress" style="width: 0%"></div>
                </div>
                <div class="level-grid">
                    <button class="level-btn lvl1" data-level="1">Niveau 1<br>15 Tuyaux</button>
                    <button class="level-btn lvl2" data-level="2">Niveau 2<br>12 Tuyaux</button>
                    <button class="level-btn lvl3" data-level="3">Niveau 3<br>10 Tuyaux</button>
                </div>
                <button id="back-btn">Retour</button>
            </div>
            
            <!-- Écran de fin de jeu -->
            <div id="game-over">
                <h2>Game Over</h2>
                <p>Niveau atteint:</p>
                <div id="max-level">Niveau 1</div>
                <button id="restart-btn">Rejouer</button>
                <button id="menu-btn">Menu Principal</button>
            </div>
            
            <!-- Notification de niveau supérieur -->
            <div id="level-up">
                Niveau Supérieur !
            </div>
            
            <!-- Écran de victoire -->
            <div id="victory">
                <div class="victory-title">🎉 FÉLICITATIONS ! 🎉</div>
                <div class="victory-message">BRAVO !</div>
                <p>Vous avez terminé tous les niveaux !</p>
                
                <div class="secret-message">
                    Mot secret pour Certitude :<br>
                    <strong id="secret-word"></strong>
                </div>
                
                <button id="victory-restart">Rejouer</button>
                <button id="victory-menu">Menu Principal</button>
            </div>
            
            <p id="description" class="game-description">ESPACE ou CLIC pour sauter</p>
        </div>
    </div>

    <script>
        
        document.addEventListener('contextmenu', function(e) {
            e.preventDefault();
            return false;
        });

        
        const canvas = document.getElementById('game');
        const ctx = canvas.getContext('2d');
        const menu = document.getElementById('menu');
        const levelSelect = document.getElementById('level-select');
        const gameOverScreen = document.getElementById('game-over');
        const levelUpScreen = document.getElementById('level-up');
        const victoryScreen = document.getElementById('victory');
        const startBtn = document.getElementById('start-btn');
        const levelBtn = document.getElementById('level-btn');
        const backBtn = document.getElementById('back-btn');
        const restartBtn = document.getElementById('restart-btn');
        const menuBtn = document.getElementById('menu-btn');
        const victoryRestartBtn = document.getElementById('victory-restart');
        const victoryMenuBtn = document.getElementById('victory-menu');
        const levelIndicator = document.getElementById('level-indicator');
        const scoreDisplay = document.getElementById('score-display');
        const pipesCount = document.getElementById('pipes-count');
        const finalScore = document.getElementById('final-score');
        const maxLevel = document.getElementById('max-level');
        const description = document.getElementById('description');
        const levelButtons = document.querySelectorAll('.level-btn');
        const globalProgress = document.getElementById('global-progress');
        const selectProgress = document.getElementById('select-progress');
        const progressPercent = document.getElementById('progress-percent');
        const secretWord = document.getElementById('secret-word');

        
        let gameRunning = false;
        let score = 0;
        let frames = 0;
        let currentLevel = 1;
        let pipesPassed = 0;
        let pipesToCompleteLevel = 15;
        let maxLevelReached = 1;
        let gameSpeed = 2;
        let mouthAnimation = 0;
        let mouthDirection = 1;
        let lastPipeDirection = 0;
        
        
        const levels = {
            1: {
                name: "Facile",
                pipesToComplete: 15,
                pipeGap: 110,
                pipeInterval: 110,
                gravity: 0.35,
                jumpForce: -6.2,
                speed: 2.0,
                color: '#4CAF50',
                bgColor: '#87CEEB',
                cloudSpeed: 0.3,
                pipeWidth: 55,
                maxHeightChange: 120,
                directionChangeChance: 0.4
            },
            2: {
                name: "Moyen",
                pipesToComplete: 12,
                pipeGap: 100,
                pipeInterval: 100,
                gravity: 0.38,
                jumpForce: -6.0,
                speed: 2.3,
                color: '#FF9800',
                bgColor: '#81D4FA',
                cloudSpeed: 0.4,
                pipeWidth: 60,
                maxHeightChange: 140,
                directionChangeChance: 0.5
            },
            3: {
                name: "Difficile",
                pipesToComplete: 10,
                pipeGap: 90,
                pipeInterval: 90,
                gravity: 0.42,
                jumpForce: -5.8,
                speed: 2.6,
                color: '#F44336',
                bgColor: '#4FC3F7',
                cloudSpeed: 0.5,
                pipeWidth: 65,
                maxHeightChange: 160,
                directionChangeChance: 0.6
            }
        };

        
        const encodedSecret = "TVlTVEVSTE9ZQUw="; 

       
        function decodeBase64(str) {
            try {
                return decodeURIComponent(Array.prototype.map.call(atob(str), function(c) {
                    return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
                }).join(''));
            } catch (e) {
                return str;
            }
        }

        
        function revealSecret() {
            return decodeBase64(encodedSecret);
        }

        
        const clouds = {
            positions: [],
            
            init: function() {
                this.positions = [];
                for (let i = 0; i < 6; i++) {
                    this.positions.push({
                        x: Math.random() * canvas.width,
                        y: Math.random() * 150 + 20,
                        size: Math.random() * 20 + 10,
                        speed: Math.random() * 0.3 + 0.1
                    });
                }
            },
            
            update: function() {
                for (let i = 0; i < this.positions.length; i++) {
                    let cloud = this.positions[i];
                    cloud.x -= cloud.speed;
                    
                    if (cloud.x + cloud.size * 3 < 0) {
                        cloud.x = canvas.width + cloud.size;
                        cloud.y = Math.random() * 150 + 20;
                        cloud.size = Math.random() * 20 + 10;
                        cloud.speed = Math.random() * 0.3 + 0.1;
                    }
                }
            },
            
            draw: function() {
                ctx.fillStyle = 'rgba(255, 255, 255, 0.85)';
                for (let i = 0; i < this.positions.length; i++) {
                    let cloud = this.positions[i];
                    this.drawCloud(cloud.x, cloud.y, cloud.size);
                }
            },
            
            drawCloud: function(x, y, size) {
                ctx.beginPath();
                ctx.arc(x, y, size, 0, Math.PI * 2);
                ctx.arc(x + size * 0.8, y - size * 0.2, size * 0.8, 0, Math.PI * 2);
                ctx.arc(x + size * 1.5, y, size * 0.9, 0, Math.PI * 2);
                ctx.fill();
            }
        };

        const pacman = {
            x: 40,
            y: canvas.height / 2,
            radius: 12,
            velocity: 0,
            gravity: levels[currentLevel].gravity,
            jumpForce: levels[currentLevel].jumpForce,
            mouthAngle: 0.4,
            
            draw: function() {
                ctx.save();
                ctx.translate(this.x, this.y);
                
                ctx.fillStyle = '#FFEB3B';
                ctx.beginPath();
                ctx.arc(0, 0, this.radius, this.mouthAngle, Math.PI * 2 - this.mouthAngle);
                ctx.lineTo(0, 0);
                ctx.fill();
                
                ctx.strokeStyle = '#000';
                ctx.lineWidth = 1.5;
                ctx.beginPath();
                ctx.arc(0, 0, this.radius, this.mouthAngle, Math.PI * 2 - this.mouthAngle);
                ctx.lineTo(0, 0);
                ctx.stroke();
                
                ctx.fillStyle = '#000';
                ctx.beginPath();
                ctx.arc(-3, -5, 1.5, 0, Math.PI * 2);
                ctx.fill();
                
                ctx.restore();
                
                this.animateMouth();
            },
            
            animateMouth: function() {
                mouthAnimation += mouthDirection * 0.08;
                if (mouthAnimation >= 1 || mouthAnimation <= 0) {
                    mouthDirection *= -1;
                }
                this.mouthAngle = 0.2 + mouthAnimation * 0.4;
            },
            
            update: function() {
                this.velocity += this.gravity;
                this.y += this.velocity;
                
                if (this.y + this.radius >= canvas.height - 60) {
                    this.y = canvas.height - 60 - this.radius;
                    this.velocity = 0;
                    gameOver();
                }
                
                if (this.y - this.radius <= 0) {
                    this.y = this.radius;
                    this.velocity = 0;
                }
            },
            
            jump: function() {
                this.velocity = this.jumpForce;
            },
            
            reset: function() {
                this.y = canvas.height / 2;
                this.velocity = 0;
                this.gravity = levels[currentLevel].gravity;
                this.jumpForce = levels[currentLevel].jumpForce;
                this.mouthAngle = 0.4;
            }
        };

        const pipes = {
            position: [],
            lastTop: 120,
            
            draw: function() {
                for (let i = 0; i < this.position.length; i++) {
                    let p = this.position[i];
                    
                    this.drawClassicPipe(p.x, 0, p.width, p.top, true);
                    
                    const bottomPipeY = p.top + levels[currentLevel].pipeGap;
                    const bottomPipeHeight = canvas.height - bottomPipeY - 60;
                    this.drawClassicPipe(p.x, bottomPipeY, p.width, bottomPipeHeight, false);
                }
            },
            
            drawClassicPipe: function(x, y, width, height, isTop) {
                ctx.fillStyle = levels[currentLevel].color;
                ctx.fillRect(x, y, width, height);
                
                ctx.fillStyle = this.darkenColor(levels[currentLevel].color, 30);
                ctx.fillRect(x - 2, y, 4, height);
                ctx.fillRect(x + width - 2, y, 4, height);
                
                ctx.fillStyle = this.darkenColor(levels[currentLevel].color, 15);
                if (isTop) {
                    ctx.fillRect(x - 8, y + height - 25, width + 16, 25);
                } else {
                    ctx.fillRect(x - 8, y, width + 16, 25);
                }
            },
            
            darkenColor: function(color, percent) {
                const num = parseInt(color.slice(1), 16);
                const amt = Math.round(2.55 * percent);
                const R = (num >> 16) - amt;
                const G = (num >> 8 & 0x00FF) - amt;
                const B = (num & 0x0000FF) - amt;
                return `#${(0x1000000 + (R > 0 ? R : 0) * 0x10000 +
                    (G > 0 ? G : 0) * 0x100 +
                    (B > 0 ? B : 0)).toString(16).slice(1)}`;
            },
            
            update: function() {
                if (frames % levels[currentLevel].pipeInterval === 0) {
                    let minTop = 50;
                    let maxTop = canvas.height - levels[currentLevel].pipeGap - 80;
                    
                    let newTop;
                    
                    if (Math.random() < levels[currentLevel].directionChangeChance) {
                        lastPipeDirection = Math.random() < 0.5 ? 1 : -1;
                    }
                    
                    let baseVariation = (Math.random() * levels[currentLevel].maxHeightChange) - (levels[currentLevel].maxHeightChange / 2);
                    
                    if (lastPipeDirection !== 0) {
                        baseVariation += lastPipeDirection * (levels[currentLevel].maxHeightChange / 3);
                    }
                    
                    if (Math.random() < 0.2) {
                        baseVariation *= 1.5;
                    }
                    
                    newTop = this.lastTop + baseVariation;
                    newTop = Math.max(minTop, Math.min(maxTop, newTop));
                    
                    if (Math.random() < 0.1) {
                        newTop = this.lastTop > (minTop + maxTop) / 2 ? minTop + 30 : maxTop - 30;
                    }
                    
                    this.lastTop = newTop;
                    
                    this.position.push({
                        x: canvas.width,
                        width: levels[currentLevel].pipeWidth,
                        top: newTop,
                        passed: false
                    });
                }
                
                for (let i = 0; i < this.position.length; i++) {
                    let p = this.position[i];
                    p.x -= gameSpeed;
                    
                    const topPipeCollision = (
                        pacman.x + pacman.radius - 2 > p.x &&
                        pacman.x - pacman.radius + 2 < p.x + p.width && 
                        pacman.y - pacman.radius < p.top
                    );
                    
                    const bottomPipeY = p.top + levels[currentLevel].pipeGap;
                    const bottomPipeCollision = (
                        pacman.x + pacman.radius - 2 > p.x &&
                        pacman.x - pacman.radius + 2 < p.x + p.width && 
                        pacman.y + pacman.radius > bottomPipeY
                    );
                    
                    if (topPipeCollision || bottomPipeCollision) {
                        gameOver();
                    }
                    
                    if (p.x + p.width < pacman.x && !p.passed) {
                        score++;
                        pipesPassed++;
                        p.passed = true;
                        scoreDisplay.textContent = `Score: ${score}`;
                        pipesCount.textContent = `Tuyaux: ${pipesPassed}/${pipesToCompleteLevel}`;
                        
                        if (pipesPassed >= pipesToCompleteLevel) {
                            if (currentLevel < 3) {
                                levelUp();
                            } else {
                                victory();
                            }
                        }
                    }
                    
                    if (p.x + p.width < 0) {
                        this.position.shift();
                    }
                }
            },
            
            reset: function() {
                this.position = [];
                this.lastTop = 120;
                lastPipeDirection = 0;
            }
        };

        const ground = {
            y: canvas.height - 60,
            height: 60,
            
            draw: function() {
                const groundGradient = ctx.createLinearGradient(0, this.y, 0, this.y + this.height);
                groundGradient.addColorStop(0, '#8B4513');
                groundGradient.addColorStop(1, '#5D4037');
                ctx.fillStyle = groundGradient;
                ctx.fillRect(0, this.y, canvas.width, this.height);
                
                ctx.fillStyle = '#7CFC00';
                ctx.fillRect(0, this.y, canvas.width, 8);
            }
        };

        const background = {
            draw: function() {
                const gradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
                gradient.addColorStop(0, levels[currentLevel].bgColor);
                gradient.addColorStop(1, '#E0F7FA');
                ctx.fillStyle = gradient;
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                
                clouds.draw();
            }
        };

        function draw() {
            background.draw();
            pipes.draw();
            ground.draw();
            pacman.draw();
        }

        function update() {
            if (gameRunning) {
                pacman.update();
                pipes.update();
            }
            clouds.update();
        }

        function loop() {
            update();
            draw();
            frames++;
            requestAnimationFrame(loop);
        }

        function startGame(level = 1) {
            currentLevel = level;
            pipesToCompleteLevel = levels[currentLevel].pipesToComplete;
            
            gameSpeed = levels[currentLevel].speed;
            pacman.gravity = levels[currentLevel].gravity;
            pacman.jumpForce = levels[currentLevel].jumpForce;
            
            score = 0;
            frames = 0;
            pipesPassed = 0;
            
            levelIndicator.textContent = `Niveau: ${currentLevel}`;
            levelIndicator.style.color = levels[currentLevel].color;
            scoreDisplay.textContent = `Score: ${score}`;
            pipesCount.textContent = `Tuyaux: ${pipesPassed}/${pipesToCompleteLevel}`;
            
            menu.style.display = 'none';
            levelSelect.style.display = 'none';
            gameOverScreen.style.display = 'none';
            levelUpScreen.style.display = 'none';
            victoryScreen.style.display = 'none';
            description.style.display = 'block';
            
            pacman.reset();
            pipes.reset();
            
            gameRunning = true;
        }

        function levelUp() {
            if (currentLevel < 3) {
                gameRunning = false;
                currentLevel++;
                if (currentLevel > maxLevelReached) {
                    maxLevelReached = currentLevel;
                    updateProgress();
                }
                
                levelUpScreen.textContent = `Niveau ${currentLevel} !\n${levels[currentLevel].name}`;
                levelUpScreen.style.display = 'block';
                levelUpScreen.style.background = levels[currentLevel].color;
                
                setTimeout(() => {
                    levelUpScreen.style.display = 'none';
                    startGame(currentLevel);
                }, 1800);
            }
        }

        function victory() {
            gameRunning = false;
            victoryScreen.style.display = 'block';
            description.style.display = 'none';
            createConfetti();
            
            
            secretWord.textContent = revealSecret();
        }

        function createConfetti() {
            for (let i = 0; i < 30; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + '%';
                confetti.style.top = '-8px';
                confetti.style.background = ['#FFD700', '#FF6B6B', '#4ECDC4', '#45B7D1', '#96CEB4'][Math.floor(Math.random() * 5)];
                confetti.style.animationDuration = (Math.random() * 2 + 1.5) + 's';
                confetti.style.animationDelay = (Math.random() * 1.5) + 's';
                document.querySelector('.game-screen').appendChild(confetti);
                
                setTimeout(() => {
                    confetti.remove();
                }, 4000);
            }
        }

        function gameOver() {
            gameRunning = false;
            maxLevel.textContent = `Niveau ${Math.max(currentLevel, maxLevelReached)}`;
            gameOverScreen.style.display = 'block';
            description.style.display = 'none';
            
            if (currentLevel > maxLevelReached) {
                maxLevelReached = currentLevel;
                updateProgress();
            }
        }

        function updateProgress() {
            const progress = Math.min(100, ((maxLevelReached - 1) / 2) * 100);
            globalProgress.style.width = `${progress}%`;
            selectProgress.style.width = `${progress}%`;
            progressPercent.textContent = `${Math.round(progress)}%`;
        }

        // Gestion des événements
        startBtn.addEventListener('click', () => startGame());
        levelBtn.addEventListener('click', () => {
            menu.style.display = 'none';
            levelSelect.style.display = 'block';
        });
        backBtn.addEventListener('click', () => {
            levelSelect.style.display = 'none';
            menu.style.display = 'block';
        });
        restartBtn.addEventListener('click', () => startGame(currentLevel));
        menuBtn.addEventListener('click', () => {
            gameOverScreen.style.display = 'none';
            menu.style.display = 'block';
        });
        victoryRestartBtn.addEventListener('click', () => startGame(1));
        victoryMenuBtn.addEventListener('click', () => {
            victoryScreen.style.display = 'none';
            menu.style.display = 'block';
        });
        
        levelButtons.forEach(btn => {
            btn.addEventListener('click', () => {
                const level = parseInt(btn.getAttribute('data-level'));
                if (level <= maxLevelReached) {
                    startGame(level);
                } else {
                    alert(`Vous devez d'abord atteindre le niveau ${level - 1} !`);
                }
            });
        });

        document.addEventListener('keydown', (e) => {
            if (e.code === 'Space' && gameRunning) {
                pacman.jump();
            } else if (e.code === 'Space' && !gameRunning && menu.style.display !== 'none') {
                startGame();
            }
        });

        canvas.addEventListener('click', () => {
            if (gameRunning) {
                pacman.jump();
            } else if (menu.style.display !== 'none') {
                startGame();
            }
        });

        // Initialisation
        clouds.init();
        updateProgress();
        loop();
    </script>
</body>
</html>
