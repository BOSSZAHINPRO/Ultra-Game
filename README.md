<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Space Explorer - Rocket Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(180deg, #0B0B2F 0%, #1a1a3a 100%);
            overflow: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .game-container {
            width: 800px;
            height: 600px;
            background: linear-gradient(180deg, #0B0B2F 0%, #1a1a3a 50%, #2a2a4a 100%);
            border: 3px solid #4A4AFF;
            border-radius: 15px;
            position: relative;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        .plane {
            width: 60px;
            height: 40px;
            position: absolute;
            left: 100px;
            top: 280px;
            transition: top 0.1s ease;
            z-index: 10;
            animation: rocketHover 2s ease-in-out infinite alternate;
        }

        @keyframes rocketHover {
            from { transform: translateX(0px) rotate(0deg); }
            to { transform: translateX(3px) rotate(1deg); }
        }

        @keyframes flameFlicker {
            0% { transform: scaleX(1) scaleY(1); opacity: 1; }
            25% { transform: scaleX(1.2) scaleY(0.8); opacity: 0.8; }
            50% { transform: scaleX(0.9) scaleY(1.3); opacity: 1; }
            75% { transform: scaleX(1.1) scaleY(0.9); opacity: 0.9; }
            100% { transform: scaleX(1) scaleY(1); opacity: 1; }
        }

        .rocket-flame {
            animation: flameFlicker 0.3s ease-in-out infinite;
        }

        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: moveStar 10s linear infinite, twinkle 2s ease-in-out infinite alternate;
        }

        .star1 {
            width: 3px;
            height: 3px;
            top: 80px;
            animation-delay: 0s;
        }

        .star2 {
            width: 2px;
            height: 2px;
            top: 150px;
            animation-delay: -3s;
        }

        .star3 {
            width: 4px;
            height: 4px;
            top: 250px;
            animation-delay: -6s;
        }

        .star4 {
            width: 2px;
            height: 2px;
            top: 350px;
            animation-delay: -2s;
        }

        .star5 {
            width: 3px;
            height: 3px;
            top: 450px;
            animation-delay: -8s;
        }

        .alien {
            position: absolute;
            width: 40px;
            height: 40px;
            animation: moveAlien 6s linear infinite, alienFloat 2s ease-in-out infinite alternate;
            z-index: 5;
        }

        @keyframes alienFloat {
            from { transform: translateY(0px) scale(1); }
            to { transform: translateY(-8px) scale(1.05); }
        }

        @keyframes moveAlien {
            from { left: 800px; }
            to { left: -40px; }
        }

        @keyframes moveStar {
            from { left: 800px; }
            to { left: -10px; }
        }

        @keyframes twinkle {
            from { opacity: 0.3; }
            to { opacity: 1; }
        }



        @keyframes spin {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        .hud {
            position: absolute;
            top: 20px;
            left: 20px;
            color: #FFFFFF;
            font-size: 18px;
            font-weight: bold;
            z-index: 20;
            background: rgba(0,0,50,0.8);
            padding: 10px 15px;
            border-radius: 10px;
            border: 1px solid #4A4AFF;
            box-shadow: 0 2px 10px rgba(74,74,255,0.3);
            animation: hudPulse 3s ease-in-out infinite alternate;
        }

        @keyframes hudPulse {
            from { 
                box-shadow: 0 2px 10px rgba(74,74,255,0.3);
                border-color: #4A4AFF;
            }
            to { 
                box-shadow: 0 2px 15px rgba(74,74,255,0.6);
                border-color: #6A6AFF;
            }
        }

        .controls {
            position: absolute;
            bottom: 20px;
            left: 20px;
            color: #FFFFFF;
            font-size: 14px;
            background: rgba(0,0,50,0.8);
            padding: 10px 15px;
            border-radius: 10px;
            border: 1px solid #4A4AFF;
            box-shadow: 0 2px 10px rgba(74,74,255,0.3);
        }

        .game-over {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(255,255,255,0.95);
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            display: none;
            z-index: 30;
        }

        .restart-btn {
            background: #4CAF50;
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 16px;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 15px;
            transition: background 0.3s ease;
        }

        .restart-btn:hover {
            background: #45a049;
        }

        .mobile-controls {
            position: absolute;
            bottom: 20px;
            right: 20px;
            display: none;
        }

        .control-btn {
            background: rgba(0,0,50,0.8);
            border: 2px solid #4A4AFF;
            border-radius: 50%;
            width: 60px;
            height: 60px;
            font-size: 24px;
            color: #FFFFFF;
            cursor: pointer;
            margin: 5px;
            transition: all 0.2s ease;
            box-shadow: 0 2px 10px rgba(74,74,255,0.3);
        }

        .control-btn:hover {
            background: rgba(74,74,255,0.8);
            transform: scale(1.1);
        }

        .laser {
            position: absolute;
            width: 20px;
            height: 4px;
            background: linear-gradient(90deg, #00FFFF, #FFFFFF, #00FFFF);
            border-radius: 2px;
            animation: moveLaser 1s linear infinite;
            box-shadow: 0 0 10px #00FFFF, 0 0 20px #00FFFF;
            z-index: 8;
        }

        @keyframes moveLaser {
            from { left: 160px; }
            to { left: 820px; }
        }

        @media (max-width: 850px) {
            .game-container {
                width: 95vw;
                height: 70vh;
            }
            
            .mobile-controls {
                display: block;
            }
            
            .controls {
                display: none;
            }
        }
    </style>
</head>
<body>
    <div class="game-container" id="gameContainer">
        <!-- Plane SVG -->
        <div class="plane" id="plane">
            <svg viewBox="0 0 60 40" fill="none" xmlns="http://www.w3.org/2000/svg">
                <!-- Rocket body -->
                <ellipse cx="35" cy="20" rx="20" ry="8" fill="#E8E8E8"/>
                <ellipse cx="35" cy="20" rx="18" ry="6" fill="#F5F5F5"/>
                <!-- Rocket nose cone -->
                <path d="M53 20 L60 18 L60 22 Z" fill="#FF4444"/>
                <!-- Rocket fins -->
                <path d="M15 12 L25 16 L25 20 L15 16 Z" fill="#4444FF"/>
                <path d="M15 28 L25 24 L25 20 L15 24 Z" fill="#4444FF"/>
                <!-- Window -->
                <circle cx="42" cy="20" r="4" fill="#87CEEB" stroke="#333" stroke-width="1"/>
                <!-- Flame exhaust -->
                <g class="rocket-flame">
                    <path d="M15 20 L5 18 L8 20 L5 22 Z" fill="#FF6600"/>
                    <path d="M8 20 L2 19 L4 20 L2 21 Z" fill="#FFAA00"/>
                    <path d="M5 20 L1 19.5 L3 20 L1 20.5 Z" fill="#FF3300"/>
                </g>
            </svg>
        </div>

        <!-- Background stars -->
        <div class="star star1"></div>
        <div class="star star2"></div>
        <div class="star star3"></div>
        <div class="star star4"></div>
        <div class="star star5"></div>

        <!-- HUD -->
        <div class="hud">
            <div>Score: <span id="score">0</span> / 500</div>
            <div>Lives: <span id="lives">3</span></div>
            <div style="font-size: 14px; color: #FFD700; margin-top: 5px;">Mission: Defend Earth!</div>
        </div>

        <!-- Controls info -->
        <div class="controls">
            <div>↑ Arrow Key or W: Move Up</div>
            <div>↓ Arrow Key or S: Move Down</div>
            <div>X or Enter: Fire Laser</div>
            <div>Space: Pause</div>
        </div>

        <!-- Mobile controls -->
        <div class="mobile-controls">
            <div>
                <button class="control-btn" id="upBtn">↑</button>
            </div>
            <div>
                <button class="control-btn" id="downBtn">↓</button>
            </div>
            <div>
                <button class="control-btn" id="fireBtn">🔥</button>
            </div>
        </div>

        <!-- Game Over screen -->
        <div class="game-over" id="gameOver">
            <h2 style="color: #2F4F4F; margin-bottom: 10px;">Mission Complete!</h2>
            <p style="color: #666; margin-bottom: 5px;">Final Score: <span id="finalScore">0</span></p>
            <p style="color: #666; margin-bottom: 20px;">Excellent space navigation, commander!</p>
            <button class="restart-btn" onclick="restartGame()">Play Again</button>
        </div>
    </div>

    <script>
        class PlaneGame {
            constructor() {
                this.plane = document.getElementById('plane');
                this.gameContainer = document.getElementById('gameContainer');
                this.scoreElement = document.getElementById('score');
                this.livesElement = document.getElementById('lives');
                this.gameOverScreen = document.getElementById('gameOver');
                this.finalScoreElement = document.getElementById('finalScore');
                
                this.planeY = 280;
                this.score = 0;
                this.lives = 3;
                this.gameRunning = true;
                this.aliens = [];
                this.lasers = [];
                this.keys = {};
                
                this.init();
            }

            init() {
                this.setupEventListeners();
                this.gameLoop();
                this.spawnAliens();
            }

            setupEventListeners() {
                // Keyboard controls
                document.addEventListener('keydown', (e) => {
                    this.keys[e.key.toLowerCase()] = true;
                    if (e.key === ' ') {
                        e.preventDefault();
                        this.togglePause();
                    }
                    if (e.key.toLowerCase() === 'x' || e.key === 'Enter') {
                        e.preventDefault();
                        this.fireLaser();
                    }
                });

                document.addEventListener('keyup', (e) => {
                    this.keys[e.key.toLowerCase()] = false;
                });

                // Mobile controls
                document.getElementById('upBtn').addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    this.keys['arrowup'] = true;
                });

                document.getElementById('upBtn').addEventListener('touchend', (e) => {
                    e.preventDefault();
                    this.keys['arrowup'] = false;
                });

                document.getElementById('downBtn').addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    this.keys['arrowdown'] = true;
                });

                document.getElementById('downBtn').addEventListener('touchend', (e) => {
                    e.preventDefault();
                    this.keys['arrowdown'] = false;
                });

                document.getElementById('fireBtn').addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    this.fireLaser();
                });
            }

            updatePlane() {
                if (this.keys['arrowup'] || this.keys['w']) {
                    this.planeY = Math.max(0, this.planeY - 3);
                }
                if (this.keys['arrowdown'] || this.keys['s']) {
                    this.planeY = Math.min(560, this.planeY + 3);
                }
                
                this.plane.style.top = this.planeY + 'px';
            }

            fireLaser() {
                if (!this.gameRunning) return;
                
                const laser = document.createElement('div');
                laser.className = 'laser';
                laser.style.top = (this.planeY + 18) + 'px';
                laser.style.left = '160px';
                
                this.gameContainer.appendChild(laser);
                this.lasers.push(laser);
                
                // Remove laser after animation completes
                setTimeout(() => {
                    if (laser.parentNode) {
                        laser.remove();
                        const index = this.lasers.indexOf(laser);
                        if (index > -1) {
                            this.lasers.splice(index, 1);
                        }
                    }
                }, 1000);
            }

            spawnAliens() {
                if (!this.gameRunning) return;
                
                const alien = document.createElement('div');
                const alienType = Math.random();
                
                if (alienType < 0.4) {
                    // Green alien (normal)
                    alien.className = 'alien';
                    alien.innerHTML = `
                        <svg viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <!-- Alien body -->
                            <ellipse cx="20" cy="25" rx="15" ry="12" fill="#00FF88"/>
                            <!-- Alien head -->
                            <ellipse cx="20" cy="15" rx="12" ry="10" fill="#00DD77"/>
                            <!-- Eyes -->
                            <ellipse cx="16" cy="12" rx="3" ry="4" fill="#000"/>
                            <ellipse cx="24" cy="12" rx="3" ry="4" fill="#000"/>
                            <ellipse cx="16" cy="11" rx="1" ry="2" fill="#FFF"/>
                            <ellipse cx="24" cy="11" rx="1" ry="2" fill="#FFF"/>
                            <!-- Antennae -->
                            <circle cx="12" cy="6" r="2" fill="#FF4444"/>
                            <circle cx="28" cy="6" r="2" fill="#FF4444"/>
                            <!-- Arms -->
                            <ellipse cx="8" cy="22" rx="3" ry="8" fill="#00DD77"/>
                            <ellipse cx="32" cy="22" rx="3" ry="8" fill="#00DD77"/>
                        </svg>
                    `;
                    alien.dataset.points = '1';
                    alien.dataset.speed = '6';
                } else if (alienType < 0.7) {
                    // Red alien (fast and dangerous)
                    alien.className = 'alien';
                    alien.style.animationDuration = '3s';
                    alien.innerHTML = `
                        <svg viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <!-- Alien body -->
                            <ellipse cx="20" cy="25" rx="15" ry="12" fill="#FF4444"/>
                            <!-- Alien head -->
                            <ellipse cx="20" cy="15" rx="12" ry="10" fill="#DD2222"/>
                            <!-- Eyes -->
                            <ellipse cx="16" cy="12" rx="3" ry="4" fill="#000"/>
                            <ellipse cx="24" cy="12" rx="3" ry="4" fill="#000"/>
                            <ellipse cx="16" cy="11" rx="1" ry="2" fill="#FFF"/>
                            <ellipse cx="24" cy="11" rx="1" ry="2" fill="#FFF"/>
                            <!-- Antennae -->
                            <circle cx="12" cy="6" r="2" fill="#FFFF00"/>
                            <circle cx="28" cy="6" r="2" fill="#FFFF00"/>
                            <!-- Arms -->
                            <ellipse cx="8" cy="22" rx="3" ry="8" fill="#DD2222"/>
                            <ellipse cx="32" cy="22" rx="3" ry="8" fill="#DD2222"/>
                            <!-- Spikes -->
                            <polygon points="20,5 18,10 22,10" fill="#FFFF00"/>
                        </svg>
                    `;
                    alien.dataset.points = '1';
                    alien.dataset.speed = '3';
                    alien.dataset.damage = '2';
                } else {
                    // Purple alien (big and tough)
                    alien.className = 'alien';
                    alien.style.width = '60px';
                    alien.style.height = '60px';
                    alien.innerHTML = `
                        <svg viewBox="0 0 60 60" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <!-- Alien body -->
                            <ellipse cx="30" cy="37" rx="22" ry="18" fill="#8844FF"/>
                            <!-- Alien head -->
                            <ellipse cx="30" cy="22" rx="18" ry="15" fill="#6622DD"/>
                            <!-- Eyes -->
                            <ellipse cx="24" cy="18" rx="4" ry="6" fill="#000"/>
                            <ellipse cx="36" cy="18" rx="4" ry="6" fill="#000"/>
                            <ellipse cx="24" cy="16" rx="2" ry="3" fill="#FFF"/>
                            <ellipse cx="36" cy="16" rx="2" ry="3" fill="#FFF"/>
                            <!-- Antennae -->
                            <circle cx="18" cy="9" r="3" fill="#FF4444"/>
                            <circle cx="42" cy="9" r="3" fill="#FF4444"/>
                            <!-- Arms -->
                            <ellipse cx="12" cy="33" rx="4" ry="12" fill="#6622DD"/>
                            <ellipse cx="48" cy="33" rx="4" ry="12" fill="#6622DD"/>
                            <!-- Armor -->
                            <rect x="22" y="30" width="16" height="8" fill="#444444" rx="2"/>
                        </svg>
                    `;
                    alien.dataset.points = '1';
                    alien.dataset.speed = '8';
                    alien.dataset.health = '2';
                }
                
                alien.style.top = Math.random() * 520 + 'px';
                alien.style.left = '800px';
                
                this.gameContainer.appendChild(alien);
                this.aliens.push(alien);
                
                setTimeout(() => this.spawnAliens(), Math.random() * 1500 + 800);
            }

            checkCollisions() {
                const planeRect = {
                    left: 100,
                    right: 160,
                    top: this.planeY,
                    bottom: this.planeY + 40
                };

                // Check alien collisions
                this.aliens.forEach((alien, index) => {
                    const alienRect = alien.getBoundingClientRect();
                    const containerRect = this.gameContainer.getBoundingClientRect();
                    
                    const relativeLeft = alienRect.left - containerRect.left;
                    const relativeTop = alienRect.top - containerRect.top;
                    const alienWidth = alien.style.width === '60px' ? 60 : 40;
                    const alienHeight = alien.style.height === '60px' ? 60 : 40;
                    
                    if (relativeLeft < planeRect.right && 
                        relativeLeft + alienWidth > planeRect.left &&
                        relativeTop < planeRect.bottom && 
                        relativeTop + alienHeight > planeRect.top) {
                        
                        const damage = parseInt(alien.dataset.damage) || 1;
                        
                        this.lives -= damage;
                        this.livesElement.textContent = this.lives;
                        alien.remove();
                        this.aliens.splice(index, 1);
                        
                        if (this.lives <= 0) {
                            this.gameOver();
                        }
                    }
                });

                // Check laser collisions with aliens
                this.lasers.forEach((laser, laserIndex) => {
                    const laserRect = laser.getBoundingClientRect();
                    const containerRect = this.gameContainer.getBoundingClientRect();
                    
                    const laserLeft = laserRect.left - containerRect.left;
                    const laserTop = laserRect.top - containerRect.top;
                    
                    // Check laser vs aliens
                    this.aliens.forEach((alien, alienIndex) => {
                        const alienRect = alien.getBoundingClientRect();
                        const alienLeft = alienRect.left - containerRect.left;
                        const alienTop = alienRect.top - containerRect.top;
                        const alienWidth = alien.style.width === '60px' ? 60 : 40;
                        const alienHeight = alien.style.height === '60px' ? 60 : 40;
                        
                        if (laserLeft < alienLeft + alienWidth && 
                            laserLeft + 20 > alienLeft &&
                            laserTop < alienTop + alienHeight && 
                            laserTop + 4 > alienTop) {
                            
                            const points = parseInt(alien.dataset.points) || 1;
                            let health = parseInt(alien.dataset.health) || 1;
                            
                            // Remove laser
                            laser.remove();
                            this.lasers.splice(laserIndex, 1);
                            
                            health--;
                            if (health <= 0) {
                                // Destroy alien
                                alien.remove();
                                this.aliens.splice(alienIndex, 1);
                                this.score += points;
                            } else {
                                // Alien takes damage but survives
                                alien.dataset.health = health;
                                alien.style.opacity = '0.7';
                            }
                            
                            this.scoreElement.textContent = this.score;
                            
                            // Check for victory condition
                            if (this.score >= 500) {
                                this.missionComplete();
                            }
                        }
                    });
                });
            }

            cleanupElements() {
                // Remove aliens that have moved off screen
                this.aliens.forEach((alien, index) => {
                    const rect = alien.getBoundingClientRect();
                    const containerRect = this.gameContainer.getBoundingClientRect();
                    
                    if (rect.right < containerRect.left) {
                        alien.remove();
                        this.aliens.splice(index, 1);
                    }
                });
            }

            gameLoop() {
                if (!this.gameRunning) return;
                
                this.updatePlane();
                this.checkCollisions();
                this.cleanupElements();
                
                requestAnimationFrame(() => this.gameLoop());
            }

            togglePause() {
                this.gameRunning = !this.gameRunning;
                if (this.gameRunning) {
                    this.gameLoop();
                }
            }

            gameOver() {
                this.gameRunning = false;
                this.finalScoreElement.textContent = this.score;
                this.gameOverScreen.style.display = 'block';
            }

            missionComplete() {
                this.gameRunning = false;
                this.finalScoreElement.textContent = this.score;
                
                // Update the game over screen to show victory
                const gameOverTitle = this.gameOverScreen.querySelector('h2');
                const gameOverMessage = this.gameOverScreen.querySelector('p:last-of-type');
                
                gameOverTitle.textContent = '🎉 MISSION ACCOMPLISHED! 🎉';
                gameOverTitle.style.color = '#4CAF50';
                gameOverMessage.textContent = 'Outstanding work, commander! You\'ve successfully defended Earth from the alien invasion!';
                
                this.gameOverScreen.style.display = 'block';
            }

            restart() {
                this.gameRunning = true;
                this.score = 0;
                this.lives = 3;
                this.planeY = 280;
                
                this.scoreElement.textContent = this.score;
                this.livesElement.textContent = this.lives;
                this.plane.style.top = this.planeY + 'px';
                this.gameOverScreen.style.display = 'none';
                
                // Reset the game over screen to default
                const gameOverTitle = this.gameOverScreen.querySelector('h2');
                const gameOverMessage = this.gameOverScreen.querySelector('p:last-of-type');
                
                gameOverTitle.textContent = 'Mission Complete!';
                gameOverTitle.style.color = '#2F4F4F';
                gameOverMessage.textContent = 'Excellent space navigation, commander!';
                
                // Clear all aliens and lasers
                this.aliens.forEach(alien => alien.remove());
                this.lasers.forEach(laser => laser.remove());
                this.aliens = [];
                this.lasers = [];
                
                this.gameLoop();
                this.spawnAliens();
            }
        }

        let game = new PlaneGame();

        function restartGame() {
            game.restart();
        }
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'96b5a9b010ca4e9b',t:'MTc1NDU1ODA1Ny4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
