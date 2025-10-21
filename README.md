<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flappy Bird -By:LeafZuya</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background: linear-gradient(to bottom, #87CEEB 0%, #98FB98 100%);
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
        }
        
        .game-container {
            position: relative;
            width: 360px;
            height: 640px;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
        }
        
        #gameCanvas {
            background: linear-gradient(to bottom, #87CEEB 0%, #98FB98 100%);
            display: block;
        }
        
        .screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: rgba(0, 0, 0, 0.7);
            z-index: 10;
        }
        
        .title {
            font-size: 2.5rem;
            font-weight: bold;
            color: #FFD700;
            margin-bottom: 15px;
            text-shadow: 3px 3px 0 #FF6B6B;
        }
        
        .score-display {
            font-size: 2rem;
            color: white;
            margin-bottom: 20px;
            text-shadow: 2px 2px 0 #000;
        }
        
        .btn {
            background: linear-gradient(to bottom, #FF6B6B, #FF8E53);
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1.1rem;
            border-radius: 25px;
            cursor: pointer;
            margin: 8px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
        }
        
        .instructions {
            color: white;
            text-align: center;
            margin-top: 15px;
            max-width: 80%;
            line-height: 1.5;
            font-size: 0.9rem;
        }
        
        .hidden {
            display: none;
        }
        
        .game-stats {
            position: absolute;
            top: 20px;
            left: 20px;
            color: white;
            font-size: 1.5rem;
            text-shadow: 2px 2px 0 #000;
            z-index: 5;
        }

        .music-control {
            position: absolute;
            top: 60px;
            right: 20px;
            z-index: 5;
        }

        .btn-music {
            background: rgba(0, 0, 0, 0.5);
            border: none;
            color: white;
            padding: 8px 12px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 1.2rem;
        }

        .music-info {
            position: absolute;
            top: 100px;
            right: 20px;
            color: white;
            font-size: 0.8rem;
            text-align: right;
            max-width: 100px;
            text-shadow: 1px 1px 0 #000;
        }

        /* Bird Selection Styles */
        .bird-selection {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 20px 0;
            max-width: 300px;
        }

        .bird-option {
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid transparent;
            border-radius: 10px;
            padding: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }

        .bird-option:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.05);
        }

        .bird-option.selected {
            border-color: #FFD700;
            background: rgba(255, 215, 0, 0.2);
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.5);
        }

        .bird-emoji {
            font-size: 2rem;
            margin-bottom: 5px;
        }

        .bird-name {
            color: white;
            font-size: 0.8rem;
            font-weight: bold;
        }

        /* Video Transition Styles */
        .video-transition {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: black;
            z-index: 20;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        .video-transition.active {
            display: flex;
        }

        .transition-video {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .transition-info {
            position: absolute;
            bottom: 20px;
            left: 0;
            width: 100%;
            text-align: center;
            color: white;
            background: rgba(0, 0, 0, 0.7);
            padding: 10px;
            font-size: 1.2rem;
        }

        .skip-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(255, 0, 0, 0.7);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            z-index: 21;
        }

        .video-timer {
            position: absolute;
            top: 20px;
            left: 20px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 5px 10px;
            border-radius: 10px;
            font-size: 0.9rem;
        }

        .celebration {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 15;
        }

        .confetti {
            position: absolute;
            width: 10px;
            height: 10px;
            background: #ff0000;
            animation: fall linear forwards;
        }

        @keyframes fall {
            to {
                transform: translateY(100vh) rotate(360deg);
            }
        }

        /* Login & Leaderboard Styles */
        .login-container, .leaderboard-container {
            background: rgba(0, 0, 0, 0.8);
            padding: 20px;
            border-radius: 10px;
            width: 90%;
            max-width: 320px;
            text-align: center;
        }

        .form-group {
            margin-bottom: 15px;
            text-align: left;
        }

        .form-group label {
            display: block;
            color: white;
            margin-bottom: 5px;
            font-size: 0.9rem;
        }

        .form-group input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: none;
            background: rgba(255, 255, 255, 0.9);
        }

        .login-options {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
        }

        .leaderboard-list {
            max-height: 300px;
            overflow-y: auto;
            margin: 15px 0;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 5px;
            padding: 10px;
        }

        .leaderboard-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 5px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            color: white;
        }

        .leaderboard-item:last-child {
            border-bottom: none;
        }

        .rank {
            font-weight: bold;
            width: 30px;
            text-align: center;
        }

        .username {
            flex: 1;
            text-align: left;
            padding-left: 10px;
        }

        .score-value {
            font-weight: bold;
            color: #FFD700;
        }

        .current-user {
            background: rgba(255, 215, 0, 0.2);
            border-radius: 5px;
        }

        .leaderboard-title {
            color: #FFD700;
            margin-bottom: 10px;
            font-size: 1.5rem;
        }

        .user-info {
            position: absolute;
            top: 20px;
            right: 20px;
            color: white;
            font-size: 0.9rem;
            text-shadow: 1px 1px 0 #000;
            z-index: 5;
            background: rgba(0, 0, 0, 0.5);
            padding: 5px 10px;
            border-radius: 10px;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <canvas id="gameCanvas" width="360" height="640"></canvas>
        
        <div class="game-stats">
            Score: <span id="score">0</span>
        </div>

        <div class="user-info" id="userInfo"></div>

        <div class="music-control">
            <button id="musicBtn" class="btn-music">🎵</button>
        </div>

        <div class="music-info" id="musicInfo">
            Musik: Ready
        </div>

        <!-- Video Transition Screen -->
        <div id="videoTransition" class="video-transition">
            <button id="skipBtn" class="skip-btn">Skip ❌</button>
            <div class="video-timer" id="videoTimer">Loading...</div>
            <video id="transitionVideo" class="transition-video" muted>
                <!-- Video akan di-set via JavaScript -->
            </video>
            <div class="transition-info" id="transitionInfo">
                Level Up! Score: <span id="transitionScore">0</span>
            </div>
        </div>
        
        <!-- Login Screen -->
        <div id="loginScreen" class="screen">
            <h1 class="title">Flappy Bird</h1>
            <div class="login-container">
                <h2 style="color: white; margin-bottom: 20px;">Login atau Daftar</h2>
                <div class="form-group">
                    <label for="username">Username:</label>
                    <input type="text" id="username" placeholder="Masukkan username">
                </div>
                <div class="form-group">
                    <label for="password">Password:</label>
                    <input type="password" id="password" placeholder="Masukkan password">
                </div>
                <div class="login-options">
                    <button id="loginBtn" class="btn">Login</button>
                    <button id="registerBtn" class="btn">Daftar</button>
                </div>
                <div id="loginMessage" style="color: #FF6B6B; margin-top: 10px; min-height: 20px;"></div>
            </div>
        </div>
        
        <!-- Start Screen -->
        <div id="startScreen" class="screen hidden">
            <h1 class="title">Flappy Bird</h1>
            
            <div class="bird-selection" id="birdSelection">
                <!-- Bird options akan diisi oleh JavaScript -->
            </div>
            
            <button id="startBtn" class="btn">Mulai Game</button>
            <button id="leaderboardBtn" class="btn">Leaderboard</button>
            <button id="logoutBtn" class="btn">Logout</button>
            <div class="instructions">
                Pilih burung favoritmu!<br>
                Tekan SPACE atau klik untuk terbang<br>
                Hindari pipa merah dan tanah<br>
                <small style="color: #FFD700; margin-top: 10px; display: block;">
                    🎵 Backsound Wiwok + 🎬 Transisi Video Bahlil!
                </small>
            </div>
        </div>
        
        <!-- Game Over Screen -->
        <div id="gameOverScreen" class="screen hidden">
            <h1 class="title">Game Over</h1>
            <div class="score-display">Score: <span id="finalScore">0</span></div>
            <button id="restartBtn" class="btn">Main Lagi</button>
            <button id="menuBtn" class="btn">Menu Utama</button>
        </div>
        
        <!-- Leaderboard Screen -->
        <div id="leaderboardScreen" class="screen hidden">
            <h1 class="title">Leaderboard</h1>
            <div class="leaderboard-container">
                <h2 class="leaderboard-title">Top 10 Pemain</h2>
                <div class="leaderboard-list" id="leaderboardList">
                    <!-- Leaderboard akan diisi oleh JavaScript -->
                </div>
                <button id="backBtn" class="btn">Kembali</button>
            </div>
        </div>
    </div>

    <script>
        // =======================
        // USER MANAGEMENT SYSTEM
        // =======================
        const USER_STORAGE_KEY = 'flappyBirdUsers';
        const LEADERBOARD_STORAGE_KEY = 'flappyBirdLeaderboard';
        const CURRENT_USER_KEY = 'flappyBirdCurrentUser';

        // Initialize storage if not exists
        function initializeStorage() {
            if (!localStorage.getItem(USER_STORAGE_KEY)) {
                localStorage.setItem(USER_STORAGE_KEY, JSON.stringify([]));
            }
            if (!localStorage.getItem(LEADERBOARD_STORAGE_KEY)) {
                localStorage.setItem(LEADERBOARD_STORAGE_KEY, JSON.stringify([]));
            }
        }

        // Get all users
        function getUsers() {
            return JSON.parse(localStorage.getItem(USER_STORAGE_KEY)) || [];
        }

        // Save users
        function saveUsers(users) {
            localStorage.setItem(USER_STORAGE_KEY, JSON.stringify(users));
        }

        // Get leaderboard
        function getLeaderboard() {
            return JSON.parse(localStorage.getItem(LEADERBOARD_STORAGE_KEY)) || [];
        }

        // Save leaderboard
        function saveLeaderboard(leaderboard) {
            localStorage.setItem(LEADERBOARD_STORAGE_KEY, JSON.stringify(leaderboard));
        }

        // Get current user
        function getCurrentUser() {
            return JSON.parse(localStorage.getItem(CURRENT_USER_KEY)) || null;
        }

        // Set current user
        function setCurrentUser(user) {
            localStorage.setItem(CURRENT_USER_KEY, JSON.stringify(user));
        }

        // Clear current user (logout)
        function clearCurrentUser() {
            localStorage.removeItem(CURRENT_USER_KEY);
        }

        // Register new user
        function registerUser(username, password) {
            const users = getUsers();
            
            // Check if username already exists
            if (users.find(user => user.username === username)) {
                return { success: false, message: 'Username sudah digunakan' };
            }
            
            // Add new user
            const newUser = {
                username,
                password, // In a real app, this should be hashed
                createdAt: new Date().toISOString()
            };
            
            users.push(newUser);
            saveUsers(users);
            
            return { success: true, message: 'Pendaftaran berhasil' };
        }

        // Login user
        function loginUser(username, password) {
            const users = getUsers();
            const user = users.find(user => user.username === username && user.password === password);
            
            if (user) {
                setCurrentUser(user);
                return { success: true, message: 'Login berhasil' };
            } else {
                return { success: false, message: 'Username atau password salah' };
            }
        }

        // Update leaderboard with new score
        function updateLeaderboard(username, score) {
            const leaderboard = getLeaderboard();
            const existingEntryIndex = leaderboard.findIndex(entry => entry.username === username);
            
            if (existingEntryIndex !== -1) {
                // Update if new score is higher
                if (score > leaderboard[existingEntryIndex].score) {
                    leaderboard[existingEntryIndex].score = score;
                    leaderboard[existingEntryIndex].updatedAt = new Date().toISOString();
                }
            } else {
                // Add new entry
                leaderboard.push({
                    username,
                    score,
                    createdAt: new Date().toISOString(),
                    updatedAt: new Date().toISOString()
                });
            }
            
            // Sort by score (descending)
            leaderboard.sort((a, b) => b.score - a.score);
            
            // Keep only top 100 entries
            if (leaderboard.length > 100) {
                leaderboard.splice(100);
            }
            
            saveLeaderboard(leaderboard);
        }

        // =======================
        // UI MANAGEMENT
        // =======================
        const loginScreen = document.getElementById('loginScreen');
        const startScreen = document.getElementById('startScreen');
        const gameOverScreen = document.getElementById('gameOverScreen');
        const leaderboardScreen = document.getElementById('leaderboardScreen');
        const usernameInput = document.getElementById('username');
        const passwordInput = document.getElementById('password');
        const loginBtn = document.getElementById('loginBtn');
        const registerBtn = document.getElementById('registerBtn');
        const loginMessage = document.getElementById('loginMessage');
        const leaderboardBtn = document.getElementById('leaderboardBtn');
        const logoutBtn = document.getElementById('logoutBtn');
        const backBtn = document.getElementById('backBtn');
        const leaderboardList = document.getElementById('leaderboardList');
        const userInfo = document.getElementById('userInfo');

        // Show login screen
        function showLoginScreen() {
            loginScreen.classList.remove('hidden');
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.add('hidden');
            userInfo.textContent = '';
        }

        // Show start screen
        function showStartScreen() {
            loginScreen.classList.add('hidden');
            startScreen.classList.remove('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.add('hidden');
            
            const currentUser = getCurrentUser();
            if (currentUser) {
                userInfo.textContent = `Halo, ${currentUser.username}`;
            }
        }

        // Show leaderboard screen
        function showLeaderboardScreen() {
            loginScreen.classList.add('hidden');
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.remove('hidden');
            
            renderLeaderboard();
        }

        // Render leaderboard
        function renderLeaderboard() {
            const leaderboard = getLeaderboard();
            const currentUser = getCurrentUser();
            
            leaderboardList.innerHTML = '';
            
            if (leaderboard.length === 0) {
                leaderboardList.innerHTML = '<div style="color: white; text-align: center;">Belum ada data</div>';
                return;
            }
            
            // Show top 10
            const top10 = leaderboard.slice(0, 10);
            
            top10.forEach((entry, index) => {
                const item = document.createElement('div');
                item.className = `leaderboard-item ${entry.username === currentUser.username ? 'current-user' : ''}`;
                
                item.innerHTML = `
                    <div class="rank">${index + 1}</div>
                    <div class="username">${entry.username}</div>
                    <div class="score-value">${entry.score}</div>
                `;
                
                leaderboardList.appendChild(item);
            });
        }

        // Event listeners for login/register
        loginBtn.addEventListener('click', () => {
            const username = usernameInput.value.trim();
            const password = passwordInput.value.trim();
            
            if (!username || !password) {
                loginMessage.textContent = 'Username dan password harus diisi';
                return;
            }
            
            const result = loginUser(username, password);
            loginMessage.textContent = result.message;
            
            if (result.success) {
                setTimeout(() => {
                    showStartScreen();
                }, 1000);
            }
        });

        registerBtn.addEventListener('click', () => {
            const username = usernameInput.value.trim();
            const password = passwordInput.value.trim();
            
            if (!username || !password) {
                loginMessage.textContent = 'Username dan password harus diisi';
                return;
            }
            
            if (username.length < 3) {
                loginMessage.textContent = 'Username minimal 3 karakter';
                return;
            }
            
            if (password.length < 3) {
                loginMessage.textContent = 'Password minimal 3 karakter';
                return;
            }
            
            const result = registerUser(username, password);
            loginMessage.textContent = result.message;
            
            if (result.success) {
                setTimeout(() => {
                    loginMessage.textContent = 'Silakan login dengan akun baru Anda';
                }, 1000);
            }
        });

        leaderboardBtn.addEventListener('click', showLeaderboardScreen);
        backBtn.addEventListener('click', showStartScreen);

        logoutBtn.addEventListener('click', () => {
            clearCurrentUser();
            showLoginScreen();
        });

        // Check if user is already logged in
        function checkLoginStatus() {
            const currentUser = getCurrentUser();
            if (currentUser) {
                showStartScreen();
            } else {
                showLoginScreen();
            }
        }

        // =======================
        // GAME VARIABLES
        // =======================
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const scoreElement = document.getElementById('score');
        const finalScoreElement = document.getElementById('finalScore');
        const startBtn = document.getElementById('startBtn');
        const restartBtn = document.getElementById('restartBtn');
        const menuBtn = document.getElementById('menuBtn');
        const musicBtn = document.getElementById('musicBtn');
        const musicInfo = document.getElementById('musicInfo');
        const videoTransition = document.getElementById('videoTransition');
        const transitionVideo = document.getElementById('transitionVideo');
        const transitionInfo = document.getElementById('transitionInfo');
        const transitionScore = document.getElementById('transitionScore');
        const skipBtn = document.getElementById('skipBtn');
        const videoTimer = document.getElementById('videoTimer');
        const birdSelection = document.getElementById('birdSelection');

        // Game State
        let gameRunning = false;
        let score = 0;
        let frames = 0;
        let pipes = [];
        let clouds = [];
        let trees = [];
        let isInTransition = false;
        let isVideoActuallyPlaying = false;
        let transitionScores = [10, 50, 100];
        let completedTransitions = [];
        let videoDuration = 0;
        let videoCurrentTime = 0;
        let videoTimerInterval = null;
        let selectedBirdType = 'default'; // Default bird

        // =======================
        // BIRD TYPES - 6 VARIASI BURUNG
        // =======================
        const BIRD_TYPES = {
            default: { emoji: '🐦', name: 'Burung Biasa', color: '#FFD700', wingColor: '#FFA500' },
            eagle: { emoji: '🦅', name: 'Elang', color: '#8B4513', wingColor: '#654321' },
            parrot: { emoji: '🦜', name: 'Parkit', color: '#FF69B4', wingColor: '#FF1493' },
            owl: { emoji: '🦉', name: 'Burung Hantu', color: '#808080', wingColor: '#696969' },
            penguin: { emoji: '🐧', name: 'Pinguin', color: '#000000', wingColor: '#2F4F4F' },
            flamingo: { emoji: '🦩', name: 'Flamingo', color: '#FF69B4', wingColor: '#FFB6C1' }
        };

        // =======================
        // VIDEO TRANSITION SYSTEM - CUSTOM ASSETS
        // =======================
        const VIDEO_URLS = {
            10: "Bahlil.mp4",
            50: "You.mp4",
            100: "Bisa.mp4"
        };

        // =======================
        // MUSIC SYSTEM - CUSTOM BACKSOUND
        // =======================
        const MUSIC_URLS = [
            "Wiwok.mp3"
        ];

        const CURRENT_MUSIC_URL = MUSIC_URLS[0];

        // =======================
        // BIRD SELECTION SYSTEM
        // =======================
        function initBirdSelection() {
            birdSelection.innerHTML = '';
            
            for (const [birdId, birdData] of Object.entries(BIRD_TYPES)) {
                const birdOption = document.createElement('div');
                birdOption.className = `bird-option ${birdId === selectedBirdType ? 'selected' : ''}`;
                birdOption.innerHTML = `
                    <div class="bird-emoji">${birdData.emoji}</div>
                    <div class="bird-name">${birdData.name}</div>
                `;
                
                birdOption.addEventListener('click', () => {
                    // Remove selected class from all options
                    document.querySelectorAll('.bird-option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    
                    // Add selected class to clicked option
                    birdOption.classList.add('selected');
                    selectedBirdType = birdId;
                    
                    console.log(`Burung dipilih: ${birdData.name}`);
                });
                
                birdSelection.appendChild(birdOption);
            }
        }

        // =======================
        // BIRD PROPERTIES - DIPERBARUI DENGAN VARIASI
        // =======================
        const bird = {
            x: 50,
            y: canvas.height / 2,
            width: 34,
            height: 24,
            gravity: 0.25,
            velocity: 0,
            jump: -7,
            rotation: 0,
            maxFallSpeed: 8,
            type: 'default',
            
            draw: function() {
                const birdData = BIRD_TYPES[this.type];
                
                ctx.save();
                ctx.translate(this.x, this.y);
                ctx.rotate(this.rotation);
                
                // Body (warna sesuai jenis burung)
                ctx.fillStyle = birdData.color;
                ctx.beginPath();
                ctx.ellipse(0, 0, this.width/2, this.height/2, 0, 0, Math.PI * 2);
                ctx.fill();
                
                // Wing (warna sayap sesuai jenis burung)
                ctx.fillStyle = birdData.wingColor;
                ctx.beginPath();
                ctx.ellipse(-8, 2, 10, 8, Math.PI/4, 0, Math.PI * 2);
                ctx.fill();
                
                // Eye
                ctx.fillStyle = 'black';
                ctx.beginPath();
                ctx.arc(8, -4, 3, 0, Math.PI * 2);
                ctx.fill();
                
                // Beak (warna berbeda untuk beberapa burung)
                ctx.fillStyle = this.type === 'eagle' ? '#FF4500' : '#FF8C00';
                ctx.beginPath();
                ctx.moveTo(12, 0);
                ctx.lineTo(20, -3);
                ctx.lineTo(20, 3);
                ctx.closePath();
                ctx.fill();
                
                // Special features untuk burung tertentu
                if (this.type === 'owl') {
                    // Cincin mata burung hantu
                    ctx.fillStyle = 'white';
                    ctx.beginPath();
                    ctx.arc(8, -4, 5, 0, Math.PI * 2);
                    ctx.fill();
                    
                    ctx.fillStyle = 'black';
                    ctx.beginPath();
                    ctx.arc(8, -4, 3, 0, Math.PI * 2);
                    ctx.fill();
                }
                
                if (this.type === 'penguin') {
                    // Perut pinguin
                    ctx.fillStyle = 'white';
                    ctx.beginPath();
                    ctx.ellipse(0, 5, this.width/3, this.height/3, 0, 0, Math.PI * 2);
                    ctx.fill();
                }
                
                if (this.type === 'flamingo') {
                    // Kaki flamingo
                    ctx.fillStyle = '#FF69B4';
                    ctx.fillRect(-5, 12, 3, 8);
                    ctx.fillRect(2, 12, 3, 8);
                }
                
                ctx.restore();
            },
            
            update: function() {
                if (isInTransition) return;
                
                this.velocity += this.gravity;
                
                if (this.velocity > this.maxFallSpeed) {
                    this.velocity = this.maxFallSpeed;
                }
                
                this.y += this.velocity;
                this.rotation = Math.min(Math.PI/6, Math.max(-Math.PI/6, this.velocity * 0.08));
                
                if (this.y + this.height/2 >= canvas.height - 80) {
                    this.y = canvas.height - 80 - this.height/2;
                    if (gameRunning) gameOver();
                }
                
                if (this.y - this.height/2 <= 0) {
                    this.y = this.height/2;
                    this.velocity = 0;
                }
            },
            
            flap: function() {
                if (isInTransition) return;
                this.velocity = this.jump;
                this.rotation = -Math.PI/6;
            },
            
            reset: function() {
                this.y = canvas.height / 2;
                this.velocity = 0;
                this.rotation = 0;
                this.type = selectedBirdType; // Set jenis burung yang dipilih
            }
        };

        // =======================
        // VIDEO TRANSITION FUNCTIONS
        // =======================
        function checkForTransition() {
            for (const threshold of transitionScores) {
                if (score >= threshold && !completedTransitions.includes(threshold)) {
                    startVideoTransition(threshold);
                    completedTransitions.push(threshold);
                    return true;
                }
            }
            return false;
        }

        function startVideoTransition(scoreThreshold) {
            console.log(`Memulai transisi video untuk score ${scoreThreshold}`);
            
            isInTransition = true;
            isVideoActuallyPlaying = false;
            videoTransition.classList.add('active');
            transitionScore.textContent = scoreThreshold;
            transitionInfo.textContent = `Level Up! Score: ${scoreThreshold}`;
            
            const videoUrl = VIDEO_URLS[scoreThreshold] || VIDEO_URLS[10];
            transitionVideo.src = videoUrl;
            transitionVideo.currentTime = 0;
            transitionVideo.muted = false;
            transitionVideo.volume = 0.7;
            
            // Reset timer info
            videoTimer.textContent = "Loading video...";
            videoDuration = 0;
            videoCurrentTime = 0;
            
            // Event listeners untuk video
            transitionVideo.addEventListener('loadedmetadata', function() {
                videoDuration = transitionVideo.duration;
                console.log(`Durasi video: ${videoDuration} detik`);
                updateVideoTimer();
            });
            
            transitionVideo.addEventListener('timeupdate', function() {
                videoCurrentTime = transitionVideo.currentTime;
                updateVideoTimer();
            });
            
            transitionVideo.addEventListener('ended', function() {
                console.log("Video selesai secara natural");
                endVideoTransition();
            });
            
            transitionVideo.addEventListener('error', function(e) {
                console.log("Error video:", e);
                videoTimer.textContent = "Video error!";
                setTimeout(() => {
                    endVideoTransition();
                }, 2000);
            });
            
            // Coba play video
            const playPromise = transitionVideo.play();
            
            if (playPromise !== undefined) {
                playPromise.then(() => {
                    console.log("Video berhasil diputar");
                    isVideoActuallyPlaying = true;
                    stopBackgroundMusic();
                    startVideoTimer();
                }).catch(error => {
                    console.log("Gagal memutar video:", error);
                    isVideoActuallyPlaying = false;
                    videoTimer.textContent = "Klik untuk play";
                    
                    const playOnClick = () => {
                        transitionVideo.play().then(() => {
                            isVideoActuallyPlaying = true;
                            stopBackgroundMusic();
                        });
                        document.removeEventListener('click', playOnClick);
                    };
                    document.addEventListener('click', playOnClick);
                });
            }
            
            createCelebration();
        }

        function startVideoTimer() {
            if (videoTimerInterval) {
                clearInterval(videoTimerInterval);
            }
            
            videoTimerInterval = setInterval(() => {
                if (!isInTransition) {
                    clearInterval(videoTimerInterval);
                    return;
                }
                updateVideoTimer();
            }, 1000);
        }

        function updateVideoTimer() {
            if (videoDuration > 0) {
                const remaining = videoDuration - videoCurrentTime;
                const seconds = Math.ceil(remaining);
                videoTimer.textContent = `Video: ${seconds}s`;
            } else {
                videoTimer.textContent = "Loading...";
            }
        }

        function endVideoTransition() {
            console.log("Mengakhiri transisi video");
            
            if (videoTimerInterval) {
                clearInterval(videoTimerInterval);
                videoTimerInterval = null;
            }
            
            transitionVideo.pause();
            transitionVideo.currentTime = 0;
            
            transitionVideo.removeEventListener('loadedmetadata', null);
            transitionVideo.removeEventListener('timeupdate', null);
            transitionVideo.removeEventListener('ended', null);
            transitionVideo.removeEventListener('error', null);
            
            videoTransition.classList.remove('active');
            isInTransition = false;
            isVideoActuallyPlaying = false;
            
            if (gameRunning && musicEnabled) {
                playBackgroundMusic();
            }
            
            const celebration = document.querySelector('.celebration');
            if (celebration) celebration.remove();
        }

        function createCelebration() {
            const celebration = document.createElement('div');
            celebration.className = 'celebration';
            document.querySelector('.game-container').appendChild(celebration);
            
            for (let i = 0; i < 50; i++) {
                setTimeout(() => {
                    const confetti = document.createElement('div');
                    confetti.className = 'confetti';
                    confetti.style.left = Math.random() * 100 + '%';
                    confetti.style.background = getRandomColor();
                    confetti.style.animationDuration = (Math.random() * 3 + 2) + 's';
                    celebration.appendChild(confetti);
                    
                    setTimeout(() => {
                        if (confetti.parentNode) {
                            confetti.remove();
                        }
                    }, 5000);
                }, i * 100);
            }
        }

        function getRandomColor() {
            const colors = ['#FF6B6B', '#4ECDC4', '#FFD166', '#06D6A0', '#118AB2', '#EF476F'];
            return colors[Math.floor(Math.random() * colors.length)];
        }

        // =======================
        // MUSIC SYSTEM
        // =======================
        let musicEnabled = true;
        let backgroundMusic = null;

        function playBackgroundMusic() {
            if (!musicEnabled || isInTransition || isVideoActuallyPlaying) return;

            try {
                stopBackgroundMusic();
                
                musicInfo.textContent = "Musik: Loading...";
                
                backgroundMusic = new Audio();
                backgroundMusic.src = CURRENT_MUSIC_URL;
                backgroundMusic.volume = 0.4;
                backgroundMusic.loop = true;
                
                backgroundMusic.addEventListener('canplaythrough', function() {
                    if (!isInTransition && !isVideoActuallyPlaying) {
                        musicInfo.textContent = "Musik: Playing";
                        const playPromise = backgroundMusic.play();
                        
                        if (playPromise !== undefined) {
                            playPromise.catch(error => {
                                musicInfo.textContent = "Musik: Click to play";
                            });
                        }
                    }
                });
                
                backgroundMusic.addEventListener('error', function(e) {
                    musicInfo.textContent = "Musik: Error";
                });
                
            } catch (error) {
                musicInfo.textContent = "Musik: Error";
            }
        }

        function stopBackgroundMusic() {
            if (backgroundMusic) {
                backgroundMusic.pause();
                backgroundMusic.currentTime = 0;
                backgroundMusic = null;
            }
            musicInfo.textContent = "Musik: Stopped";
        }

        function toggleMusic() {
            musicEnabled = !musicEnabled;
            musicBtn.textContent = musicEnabled ? '🎵' : '🔇';
            musicInfo.textContent = musicEnabled ? "Musik: On" : "Musik: Off";
            
            if (musicEnabled && gameRunning && !isInTransition && !isVideoActuallyPlaying) {
                playBackgroundMusic();
            } else {
                stopBackgroundMusic();
            }
        }

        // =======================
        // PIPE CLASS (Tetap sama)
        // =======================
        class Pipe {
            constructor() {
                this.width = 60;
                this.x = canvas.width;
                this.gap = 160;
                this.topHeight = Math.random() * (canvas.height - this.gap - 200) + 50;
                this.bottomY = this.topHeight + this.gap;
                this.passed = false;
                this.speed = 2;
            }
            
            draw() {
                ctx.fillStyle = '#FF6B6B';
                ctx.fillRect(this.x, 0, this.width, this.topHeight);
                
                ctx.fillStyle = '#FF4757';
                ctx.fillRect(this.x - 5, this.topHeight - 20, this.width + 10, 20);
                
                ctx.fillStyle = '#FF6B6B';
                ctx.fillRect(this.x, this.bottomY, this.width, canvas.height - this.bottomY);
                
                ctx.fillStyle = '#FF4757';
                ctx.fillRect(this.x - 5, this.bottomY, this.width + 10, 20);
            }
            
            update() {
                if (isInTransition) return;
                
                this.x -= this.speed;
                
                if (this.collidesWith(bird)) {
                    gameOver();
                }
                
                if (!this.passed && this.x + this.width < bird.x) {
                    score++;
                    scoreElement.textContent = score;
                    this.passed = true;
                    
                    checkForTransition();
                }
            }
            
            collidesWith(bird) {
                const birdLeft = bird.x - bird.width/2 + 5;
                const birdRight = bird.x + bird.width/2 - 5;
                const birdTop = bird.y - bird.height/2 + 5;
                const birdBottom = bird.y + bird.height/2 - 5;
                
                return (
                    birdRight > this.x &&
                    birdLeft < this.x + this.width &&
                    (
                        birdTop < this.topHeight ||
                        birdBottom > this.bottomY
                    )
                );
            }
            
            isOffscreen() {
                return this.x + this.width < 0;
            }
        }

        // =======================
        // CLOUD & TREE CLASSES (Tetap sama)
        // =======================
        class Cloud {
            constructor() {
                this.x = canvas.width + Math.random() * 100;
                this.y = Math.random() * 200 + 50;
                this.width = 80 + Math.random() * 40;
                this.height = 40 + Math.random() * 20;
                this.speed = 0.5 + Math.random() * 0.5;
            }
            
            draw() {
                ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.height/2, 0, Math.PI * 2);
                ctx.arc(this.x + this.width/3, this.y - this.height/4, this.height/2.5, 0, Math.PI * 2);
                ctx.arc(this.x + this.width/1.5, this.y, this.height/2, 0, Math.PI * 2);
                ctx.fill();
            }
            
            update() {
                if (isInTransition) return;
                this.x -= this.speed;
                if (this.x + this.width < 0) {
                    this.x = canvas.width + 50;
                    this.y = Math.random() * 200 + 50;
                }
            }
        }

        class Tree {
            constructor() {
                this.x = canvas.width + Math.random() * 200;
                this.height = 60 + Math.random() * 40;
                this.width = 30 + Math.random() * 20;
                this.speed = 1;
            }
            
            draw() {
                ctx.fillStyle = '#8B4513';
                ctx.fillRect(this.x, canvas.height - 80 - this.height, this.width/3, this.height);
                
                ctx.fillStyle = '#228B22';
                ctx.beginPath();
                ctx.arc(this.x + this.width/6, canvas.height - 80 - this.height, this.width/1.5, 0, Math.PI * 2);
                ctx.fill();
            }
            
            update() {
                if (isInTransition) return;
                this.x -= this.speed;
                if (this.x + this.width < 0) {
                    this.x = canvas.width + 50;
                    this.height = 60 + Math.random() * 40;
                }
            }
        }

        // =======================
        // GAME FUNCTIONS
        // =======================
        function initBackground() {
            clouds = [];
            trees = [];
            
            for (let i = 0; i < 5; i++) {
                clouds.push(new Cloud());
                clouds[i].x = Math.random() * canvas.width;
            }
            
            for (let i = 0; i < 3; i++) {
                trees.push(new Tree());
                trees[i].x = Math.random() * canvas.width;
            }
        }

        function drawBackground() {
            const skyGradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
            skyGradient.addColorStop(0, '#87CEEB');
            skyGradient.addColorStop(1, '#98FB98');
            ctx.fillStyle = skyGradient;
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            clouds.forEach(cloud => cloud.draw());
            
            ctx.fillStyle = '#32CD32';
            ctx.fillRect(0, canvas.height - 80, canvas.width, 80);
            
            ctx.fillStyle = '#228B22';
            for (let i = 0; i < canvas.width; i += 20) {
                ctx.fillRect(i, canvas.height - 80, 10, 5);
            }
            
            trees.forEach(tree => tree.draw());
        }

        function startGame() {
            gameRunning = true;
            score = 0;
            frames = 0;
            pipes = [];
            completedTransitions = [];
            isInTransition = false;
            isVideoActuallyPlaying = false;
            scoreElement.textContent = score;
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            videoTransition.classList.remove('active');
            bird.reset();
            initBackground();
            
            playBackgroundMusic();
            gameLoop();
        }

        function gameOver() {
            gameRunning = false;
            finalScoreElement.textContent = score;
            gameOverScreen.classList.remove('hidden');
            stopBackgroundMusic();
            
            // Update leaderboard dengan score baru
            const currentUser = getCurrentUser();
            if (currentUser) {
                updateLeaderboard(currentUser.username, score);
            }
        }

        function gameLoop() {
            if (!gameRunning || isInTransition) {
                if (isInTransition) {
                    requestAnimationFrame(gameLoop);
                }
                return;
            }
            
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBackground();
            
            clouds.forEach(cloud => cloud.update());
            trees.forEach(tree => tree.update());
            
            if (frames % 140 === 0) {
                pipes.push(new Pipe());
            }
            
            pipes.forEach((pipe, index) => {
                pipe.update();
                pipe.draw();
                
                if (pipe.isOffscreen()) {
                    pipes.splice(index, 1);
                }
            });
            
            bird.update();
            bird.draw();
            
            frames++;
            requestAnimationFrame(gameLoop);
        }

        // =======================
        // EVENT LISTENERS
        // =======================
        startBtn.addEventListener('click', startGame);
        restartBtn.addEventListener('click', startGame);
        menuBtn.addEventListener('click', () => {
            gameOverScreen.classList.add('hidden');
            startScreen.classList.remove('hidden');
            stopBackgroundMusic();
        });

        musicBtn.addEventListener('click', toggleMusic);
        skipBtn.addEventListener('click', endVideoTransition);

        document.addEventListener('keydown', (e) => {
            if (e.code === 'Space') {
                if (gameRunning && !isInTransition) {
                    bird.flap();
                } else if (startScreen.classList.contains('hidden') && !isInTransition) {
                    startGame();
                }
                e.preventDefault();
            }
        });

        canvas.addEventListener('click', () => {
            if (gameRunning && !isInTransition) {
                bird.flap();
            } else if (startScreen.classList.contains('hidden') && !isInTransition) {
                startGame();
            }
        });

        window.addEventListener('keydown', (e) => {
            if(e.keyCode === 32 && e.target === document.body) {
                e.preventDefault();
            }
        });

        // =======================
        // INITIALIZE GAME
        // =======================
        initializeStorage();
        initBirdSelection(); // Initialize bird selection
        initBackground();
        drawBackground();
        bird.draw();
        checkLoginStatus();

        console.log("🎮 Flappy Bird Lengkap dengan Login & Leaderboard!");
        console.log("🐦 6 Variasi burung tersedia");
        console.log("👤 Sistem login dengan localStorage");
        console.log("🏆 Leaderboard online tersimpan di browser");
    </script>
</body>
</html>
