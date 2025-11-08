<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Leafpy Bird</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a6c2a, #1f8bb2, #2dfdd8);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            color: white;
        }
        
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            max-width: 1000px;
            width: 100%;
        }
        
        header {
            text-align: center;
            margin-bottom: 10px;
        }
        
        h1 {
            font-size: 3.5rem;
            color: #ffcc00;
            text-shadow: 3px 3px 0 #ff6600, 6px 6px 0 #cc3300;
            margin-bottom: 10px;
            letter-spacing: 2px;
        }
        
        .subtitle {
            font-size: 1.2rem;
            color: #ffeb99;
            margin-bottom: 20px;
        }
        
        .game-container {
            display: flex;
            gap: 30px;
            width: 100%;
            justify-content: center;
            flex-wrap: wrap;
        }
        
        .game-area {
            position: relative;
            width: 320px;
            height: 480px;
            border: 4px solid #ffcc00;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
            background: linear-gradient(to bottom, #87CEEB, #1E90FF);
        }
        
        #gameCanvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        
        .ui-panel {
            background: rgba(30, 30, 60, 0.85);
            border-radius: 15px;
            padding: 20px;
            width: 300px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            border: 2px solid #4d94ff;
        }
        
        .stats {
            margin-bottom: 20px;
            text-align: center;
        }
        
        .score-display {
            font-size: 1.8rem;
            margin-bottom: 10px;
            color: #ffcc00;
        }
        
        .coins-display {
            font-size: 1.4rem;
            color: #ffeb99;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }
        
        .coin-icon {
            color: gold;
            font-size: 1.6rem;
        }
        
        .clover-display {
            font-size: 1.4rem;
            color: #4dff4d;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
            margin-top: 5px;
        }
        
        .clover-icon {
            color: #4dff4d;
            font-size: 1.6rem;
        }
        
        .controls {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        button {
            background: linear-gradient(to bottom, #ffcc00, #ff9900);
            border: none;
            border-radius: 50px;
            color: #8a2b06;
            padding: 12px 20px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 0 #cc6600;
        }
        
        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 0 #cc6600;
        }
        
        button:active {
            transform: translateY(1px);
            box-shadow: 0 2px 0 #cc6600;
        }
        
        .shop-button {
            background: linear-gradient(to bottom, #4d94ff, #0066cc);
            box-shadow: 0 4px 0 #004499;
            color: white;
        }
        
        .shop-button:hover {
            box-shadow: 0 6px 0 #004499;
        }
        
        .shop-button:active {
            box-shadow: 0 2px 0 #004499;
        }
        
        .missions-button {
            background: linear-gradient(to bottom, #4CAF50, #2E7D32);
            box-shadow: 0 4px 0 #1B5E20;
            color: white;
            margin-top: 10px;
        }
        
        .missions-button:hover {
            box-shadow: 0 6px 0 #1B5E20;
        }
        
        .missions-button:active {
            box-shadow: 0 2px 0 #1B5E20;
        }
        
        .converter-panel {
            background: rgba(30, 30, 60, 0.85);
            border-radius: 15px;
            padding: 20px;
            width: 300px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            border: 2px solid #4d94ff;
            margin-top: 20px;
        }
        
        .converter-title {
            color: #ffcc00;
            margin-bottom: 15px;
            text-align: center;
        }
        
        .converter-input {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }
        
        .converter-input input {
            flex: 1;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #4d94ff;
            background: rgba(255, 255, 255, 0.1);
            color: white;
        }
        
        .converter-buttons {
            display: flex;
            gap: 10px;
        }
        
        .converter-buttons button {
            flex: 1;
        }
        
        .leaderboard-panel {
            background: rgba(30, 30, 60, 0.85);
            border-radius: 15px;
            padding: 20px;
            width: 300px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            border: 2px solid #ffcc00;
            margin-top: 20px;
        }
        
        .leaderboard-title {
            color: #ffcc00;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.5rem;
        }
        
        .leaderboard-list {
            max-height: 200px;
            overflow-y: auto;
            margin-bottom: 15px;
        }
        
        .leaderboard-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 12px;
            margin-bottom: 5px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
        }
        
        .leaderboard-rank {
            font-weight: bold;
            color: #ffcc00;
            width: 30px;
        }
        
        .leaderboard-name {
            flex: 1;
            margin: 0 10px;
        }
        
        .leaderboard-score {
            font-weight: bold;
            color: #4dff4d;
        }
        
        .current-player {
            background: rgba(255, 204, 0, 0.2);
            border: 1px solid #ffcc00;
        }
        
        .leaderboard-buttons {
            display: flex;
            gap: 10px;
        }
        
        .leaderboard-button {
            flex: 1;
            padding: 8px 15px;
            font-size: 0.9rem;
        }
        
        .player-name-input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ffcc00;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            margin-bottom: 10px;
            text-align: center;
        }
        
        .name-submit-button {
            width: 100%;
        }
        
        .instructions {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin-top: 10px;
        }
        
        .instructions h3 {
            color: #ffcc00;
            margin-bottom: 10px;
        }
        
        .instructions p {
            margin-bottom: 8px;
            font-size: 0.9rem;
        }
        
        /* Modal Styles */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 100;
            justify-content: center;
            align-items: center;
        }
        
        .modal-content {
            background: linear-gradient(135deg, #2d3a8b, #1c2a6e);
            border-radius: 15px;
            padding: 25px;
            width: 90%;
            max-width: 800px;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            border: 3px solid #4d94ff;
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            border-bottom: 2px solid #4d94ff;
            padding-bottom: 10px;
        }
        
        .modal-title {
            font-size: 1.8rem;
            color: #ffcc00;
        }
        
        .close-button {
            background: none;
            border: none;
            color: white;
            font-size: 2rem;
            cursor: pointer;
            padding: 0;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: none;
        }
        
        .close-button:hover {
            transform: none;
            box-shadow: none;
            color: #ffcc00;
        }
        
        .shop-categories {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        .category-button {
            flex: 1;
            padding: 10px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .category-button.active {
            background: #4d94ff;
            color: white;
        }
        
        .shop-items {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 15px;
        }
        
        .shop-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 15px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }
        
        .shop-item:hover {
            transform: translateY(-5px);
            background: rgba(255, 255, 255, 0.2);
        }
        
        .shop-item.selected {
            border-color: #ffcc00;
            background: rgba(255, 204, 0, 0.2);
        }
        
        .shop-item.owned {
            border-color: #4dff4d;
        }
        
        .item-image {
            width: 80px;
            height: 80px;
            margin: 0 auto 10px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            background: rgba(255, 255, 255, 0.2);
        }
        
        .item-name {
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .item-price {
            color: #ffcc00;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }
        
        .purchase-button {
            margin-top: 10px;
            padding: 8px 15px;
            font-size: 0.9rem;
            width: 100%;
        }
        
        .missions-modal .shop-items {
            display: flex;
            flex-direction: column;
            gap: 10px;
            max-height: 400px;
            overflow-y: auto;
        }
        
        .mission-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .mission-info {
            flex: 1;
        }
        
        .mission-name {
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .mission-description {
            font-size: 0.9rem;
            color: #cccccc;
        }
        
        .mission-reward {
            display: flex;
            align-items: center;
            gap: 5px;
            margin-top: 5px;
        }
        
        .mission-progress {
            width: 100%;
            height: 10px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 5px;
            margin-top: 5px;
            overflow: hidden;
        }
        
        .mission-progress-bar {
            height: 100%;
            background: linear-gradient(to right, #4CAF50, #8BC34A);
            border-radius: 5px;
            transition: width 0.3s ease;
        }
        
        .mission-claim-button {
            padding: 8px 15px;
            font-size: 0.9rem;
        }
        
        .mission-completed {
            border-color: #4CAF50;
        }
        
        .mission-claimed {
            border-color: #FF9800;
        }
        
        .missions-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        .missions-tab {
            flex: 1;
            padding: 10px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .missions-tab.active {
            background: #4CAF50;
            color: white;
        }
        
        .achievement-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 10px;
        }
        
        .achievement-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .achievement-name {
            font-weight: bold;
            color: #ffcc00;
        }
        
        .achievement-reward {
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .achievement-description {
            font-size: 0.9rem;
            color: #cccccc;
            margin-bottom: 10px;
        }
        
        .achievement-progress {
            width: 100%;
            height: 10px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 5px;
            margin-bottom: 10px;
            overflow: hidden;
        }
        
        .achievement-progress-bar {
            height: 100%;
            background: linear-gradient(to right, #FF9800, #FFC107);
            border-radius: 5px;
            transition: width 0.3s ease;
        }
        
        .achievement-claim-button {
            width: 100%;
            padding: 8px 15px;
            font-size: 0.9rem;
        }
        
        .achievement-completed {
            border-color: #FF9800;
        }
        
        .achievement-claimed {
            border-color: #9C27B0;
        }
        
        .game-over {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 10;
            color: white;
            text-align: center;
        }
        
        .game-over h2 {
            font-size: 3rem;
            color: #ff3333;
            margin-bottom: 20px;
            text-shadow: 2px 2px 0 #000;
        }
        
        .final-score {
            font-size: 2rem;
            margin-bottom: 30px;
            color: #ffcc00;
        }
        
        .game-start {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.6);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 10;
            color: white;
            text-align: center;
        }
        
        .game-start h2 {
            font-size: 2.5rem;
            color: #ffcc00;
            margin-bottom: 20px;
            text-shadow: 2px 2px 0 #000;
        }
        
        .game-start p {
            margin-bottom: 30px;
            font-size: 1.2rem;
            max-width: 80%;
        }
        
        .coin-effect {
            position: absolute;
            color: gold;
            font-size: 1.5rem;
            font-weight: bold;
            animation: coinFloat 1s ease-out forwards;
            pointer-events: none;
            z-index: 5;
        }
        
        @keyframes coinFloat {
            0% {
                transform: translateY(0);
                opacity: 1;
            }
            100% {
                transform: translateY(-50px);
                opacity: 0;
            }
        }
        
        /* Tambahkan style untuk kontrol audio */
        .audio-control {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: rgba(30, 30, 60, 0.85);
            border-radius: 50%;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            z-index: 1000;
            border: 2px solid #ffcc00;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
        }
        
        .audio-control:hover {
            transform: scale(1.1);
        }
        
        .audio-control.muted {
            background: rgba(255, 0, 0, 0.7);
        }
        
        .audio-icon {
            font-size: 24px;
            color: white;
        }
        
        @media (max-width: 768px) {
            .game-container {
                flex-direction: column;
                align-items: center;
            }
            
            .ui-panel {
                width: 320px;
            }
        }
    </style>
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-firestore-compat.js"></script>
</head>
<body>
    <!-- Tambahkan kontrol audio -->
    <div class="audio-control" id="audioControl">
        <span class="audio-icon">🔊</span>
    </div>
    
    <div class="container">
        <header>
            <h1>LEAFPY BIRD</h1>
            <p class="subtitle">Terbang melalui rintangan dan kumpulkan koin untuk membeli karakter dan latar baru!</p>
        </header>
        
        <div class="game-container">
            <div class="game-area">
                <canvas id="gameCanvas" width="320" height="480"></canvas>
                
                <div class="game-start" id="gameStart">
                    <h2>LEAFPY BIRD</h2>
                    <p>Klik/tap atau tekan Spasi untuk terbang. Hindari pipa dan kumpulkan koin!</p>
                    <button id="startButton">Mulai Bermain</button>
                </div>
                
                <div class="game-over" id="gameOver">
                    <h2>GAME OVER</h2>
                    <div class="final-score">Skor: <span id="finalScore">0</span></div>
                    <button id="restartButton">Main Lagi</button>
                </div>
            </div>
            
            <div class="ui-panel">
                <div class="stats">
                    <div class="score-display">Skor: <span id="score">0</span></div>
                    <div class="coins-display">
                        <span class="coin-icon">🪙</span> 
                        <span id="coins">0</span> Koin
                    </div>
                    <div class="clover-display">
                        <span class="clover-icon">🍀</span> 
                        <span id="clovers">0</span> Clover
                    </div>
                </div>
                
                <div class="controls">
                    <button id="jumpButton">Terbang!</button>
                    <button class="shop-button" id="openShop">Buka Toko</button>
                    <button class="missions-button" id="openMissions">Misi & Pencapaian</button>
                    <button class="shop-button" id="resetData">Reset Data</button>
                </div>
                
                <div class="converter-panel">
                    <div class="converter-title">Konverter Mata Uang</div>
                    <div class="converter-input">
                        <input type="number" id="converterAmount" placeholder="Jumlah" min="1">
                    </div>
                    <div class="converter-buttons">
                        <button id="convertToClover">Coin → Clover</button>
                        <button id="convertToCoin">Clover → Coin</button>
                    </div>
                </div>
                
                <div class="leaderboard-panel">
                    <div class="leaderboard-title">🏆 Leaderboard Online</div>
                    <div class="leaderboard-list" id="leaderboardList">
                        <div class="leaderboard-item">Loading leaderboard...</div>
                    </div>
                    <div id="nameInputSection">
                        <input type="text" class="player-name-input" id="playerNameInput" placeholder="Masukkan nama Anda (2-15 karakter)" maxlength="15">
                        <button class="leaderboard-button name-submit-button" id="submitName">Daftar & Aktifkan Leaderboard</button>
                    </div>
                    <div class="leaderboard-buttons" id="leaderboardButtons" style="display: none;">
                        <button class="leaderboard-button" id="refreshLeaderboard">🔄 Refresh</button>
                    </div>
                    <div style="text-align: center; margin-top: 10px; font-size: 0.8rem; color: #cccccc;">
                        Skor tersimpan otomatis saat High Score bertambah
                    </div>
                </div>
                
                <div class="instructions">
                    <h3>Cara Bermain:</h3>
                    <p>• Klik/tap atau tekan Spasi untuk terbang</p>
                    <p>• Hindari pipa dan tanah</p>
                    <p>• Dapatkan 10 koin untuk setiap pipa yang berhasil dilewati</p>
                    <p>• Buka toko untuk membeli karakter dan latar baru</p>
                    <p>• Selesaikan misi untuk mendapatkan Clover dan Coin</p>
                    <p>• Data tersimpan otomatis!</p>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Toko Modal -->
    <div class="modal" id="shopModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Toko Leafpy Bird</h2>
                <button class="close-button" id="closeShop">&times;</button>
            </div>
            
            <div class="shop-categories">
                <div class="category-button active" data-category="birds">Karakter Burung</div>
                <div class="category-button" data-category="backgrounds">Latar Belakang</div>
            </div>
            
            <div class="coins-display" style="justify-content: center; margin-bottom: 20px;">
                <span class="coin-icon">🪙</span> 
                <span id="shopCoins">0</span> Koin
                <span style="margin: 0 10px">|</span>
                <span class="clover-icon">🍀</span> 
                <span id="shopClovers">0</span> Clover
            </div>
            
            <div class="shop-items" id="shopItems">
                <!-- Items will be populated by JavaScript -->
            </div>
        </div>
    </div>
    
    <!-- Misi & Pencapaian Modal -->
    <div class="modal missions-modal" id="missionsModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Misi & Pencapaian</h2>
                <button class="close-button" id="closeMissions">&times;</button>
            </div>
            
            <div class="missions-tabs">
                <div class="missions-tab active" data-tab="daily">Misi Harian</div>
                <div class="missions-tab" data-tab="weekly">Misi Mingguan</div>
                <div class="missions-tab" data-tab="achievements">Pencapaian</div>
            </div>
            
            <div class="coins-display" style="justify-content: center; margin-bottom: 20px;">
                <span class="coin-icon">🪙</span> 
                <span id="missionsCoins">0</span> Koin
                <span style="margin: 0 10px">|</span>
                <span class="clover-icon">🍀</span> 
                <span id="missionsClovers">0</span> Clover
            </div>
            
            <div class="shop-items" id="missionsItems">
                <!-- Misi dan pencapaian akan diisi oleh JavaScript -->
            </div>
        </div>
    </div>

    <script>
        // Game Variables
        let canvas, ctx;
        let gameActive = false;
        let score = 0;
        let coins = 0;
        let clovers = 0;
        let frames = 0;
        
        // VARIABEL BARU: Backsound Music
        let backgroundMusic;
        let isMusicPlaying = false;
        
        // Firebase Configuration - GANTI DENGAN CONFIG ANDA SENDIRI
        const firebaseConfig = {
            apiKey: "AIzaSyAIjChzkl47VERuLIIVKbwa1y7Ygx40Olc",
            authDomain: "leafzuya.firebaseapp.com",
            projectId: "leafzuya",
            storageBucket: "leafzuya.firebasestorage.app",
            messagingSenderId: "808364144065",
            appId: "1:808364144065:web:6bcf48794fe45cd0452b40"
        };
        
        // Initialize Firebase
        let db;
        let leaderboardInitialized = false;
        try {
            firebase.initializeApp(firebaseConfig);
            db = firebase.firestore();
            leaderboardInitialized = true;
            console.log("Firebase initialized successfully");
        } catch (error) {
            console.error("Firebase initialization error:", error);
            leaderboardInitialized = false;
        }
        
        // Player data for leaderboard
        let playerName = localStorage.getItem('leafpyBirdPlayerName') || '';
        let playerId = localStorage.getItem('leafpyBirdPlayerId') || generatePlayerId();
        let autoSubmitEnabled = localStorage.getItem('leafpyBirdAutoSubmit') === 'true';
        
        // Bird Variables - Adjusted for slower movement
        let bird = {
            x: 50,
            y: 150,
            width: 40,
            height: 30,
            gravity: 0.3,  // Reduced gravity for slower falling
            lift: -8,      // Reduced lift for slower jumping
            velocity: 0,
            currentBird: 'leafpy',
            wingAngle: 0,
            wingDirection: 1
        };
        
        // Pipes Array
        let pipes = [];
        
        // Background Elements
        let clouds = [];
        let trees = [];
        let bushes = [];
        let mountains = [];
        
        // Shop Data
        const birds = [
            { id: 'leafpy', name: 'Burung Leafpy(Terlalu Classic)', price: 0, owned: true, selected: true, color: '#4CAF50', emoji: '🍃' },
            { id: 'fire', name: 'Burung Api(Menyala Abangkuhh)', price: 100, owned: false, selected: false, color: '#FF5722', emoji: '🔥' },
            { id: 'ice', name: 'Burung Es(Dinginn cuyyy)', price: 150, owned: false, selected: false, color: '#03A9F4', emoji: '❄️' },
            { id: 'gold', name: 'Burung Emas(Behh,Sultan Abiezz)', price: 300, owned: false, selected: false, color: '#FFD700', emoji: '🪙' },
            { id: 'phantom', name: 'Burung Phantom(Terlalu Mistis)', price: 250, owned: false, selected: false, color: '#9C27B0', emoji: '👻' },
            { id: 'robot', name: 'Robot Burung(Tes Tut Tut Wuk Wuk 1 2...aku dari masa depan)', price: 200, owned: false, selected: false, color: '#607D8B', emoji: '🤖' },
            { id: 'rainbow', name: 'Burung Pelangi(Indahnya~)', price: 500, owned: false, selected: false, color: 'rainbow', emoji: '🌈' }
        ];
        
        const backgrounds = [
            { id: 'default', name: 'Hijau Alami', price: 0, owned: true, selected: true, color: '#70c5ce' },
            { id: 'desert', name: 'Gurun Pasir', price: 150, owned: false, selected: false, color: '#e6b87e' },
            { id: 'ocean', name: 'Lautan Biru', price: 200, owned: false, selected: false, color: '#4a90e2' },
            { id: 'forest', name: 'Hutan Ajaib', price: 180, owned: false, selected: false, color: '#2e8b57' },
            { id: 'night', name: 'Malam Bintang', price: 250, owned: false, selected: false, color: '#191970' },
            { id: 'sunset', name: 'Senja Merah', price: 220, owned: false, selected: false, color: '#ff7e5f' },
            { id: 'winter', name: 'Musim Salju', price: 300, owned: false, selected: false, color: '#b0e0e6' },
            { id: 'volcano', name: 'Gunung Api', price: 350, owned: false, selected: false, color: '#8b4513' },
            { id: 'space', name: 'Angkasa Luar', price: 500, owned: false, selected: false, color: '#0d0d2b' },
            { id: 'candy', name: 'Dunia Permen', price: 400, owned: false, selected: false, color: '#ffb6c1' },
            { id: 'neon', name: 'Neon City', price: 450, owned: false, selected: false, color: '#0f3460' }
        ];
        
        let currentBackground = 'default';
        
        // Sistem Misi dan Achievement
        let missions = {
            daily: [
                { id: 'daily1', name: 'Terbang 100m', description: 'Kumpulkan total 100 poin dalam satu hari', type: 'score', target: 100, progress: 0, reward: { coins: 50, clovers: 0 }, claimed: false },
                { id: 'daily2', name: 'Kumpulkan 50 Koin', description: 'Kumpulkan 50 koin dalam satu hari', type: 'coins', target: 50, progress: 0, reward: { coins: 30, clovers: 0 }, claimed: false },
                { id: 'daily3', name: 'Lewati 5 Pipa', description: 'Lewati 5 pipa dalam satu permainan', type: 'pipes', target: 5, progress: 0, reward: { coins: 40, clovers: 0 }, claimed: false },
                { id: 'daily4', name: 'Main 3 Game', description: 'Mainkan game sebanyak 3 kali', type: 'games', target: 3, progress: 0, reward: { coins: 25, clovers: 0 }, claimed: false },
                { id: 'daily5', name: 'Terbang Tinggi', description: 'Capai ketinggian maksimum', type: 'height', target: 300, progress: 0, reward: { coins: 60, clovers: 0 }, claimed: false }
            ],
            weekly: [
                { id: 'weekly1', name: 'Terbang 1000m', description: 'Kumpulkan total 1000 poin dalam satu minggu', type: 'score', target: 1000, progress: 0, reward: { coins: 200, clovers: 1 }, claimed: false },
                { id: 'weekly2', name: 'Kumpulkan 500 Koin', description: 'Kumpulkan 500 koin dalam satu minggu', type: 'coins', target: 500, progress: 0, reward: { coins: 150, clovers: 1 }, claimed: false },
                { id: 'weekly3', name: 'Lewati 50 Pipa', description: 'Lewati 50 pipa dalam satu minggu', type: 'pipes', target: 50, progress: 0, reward: { coins: 180, clovers: 1 }, claimed: false },
                { id: 'weekly4', name: 'Main 15 Game', description: 'Mainkan game sebanyak 15 kali', type: 'games', target: 15, progress: 0, reward: { coins: 120, clovers: 1 }, claimed: false },
                { id: 'weekly5', name: 'Beli 3 Item', description: 'Beli 3 item dari toko', type: 'purchases', target: 3, progress: 0, reward: { coins: 250, clovers: 2 }, claimed: false },
                { id: 'weekly6', name: 'Selesaikan Semua Misi Harian', description: 'Selesaikan semua misi harian 5 kali', type: 'dailyCompleted', target: 5, progress: 0, reward: { coins: 300, clovers: 2 }, claimed: false },
                { id: 'weekly7', name: 'Capai High Score 200', description: 'Capai skor tertinggi 200 dalam satu permainan', type: 'highScore', target: 200, progress: 0, reward: { coins: 400, clovers: 3 }, claimed: false }
            ],
            achievements: [
                { id: 'ach1', name: 'Pemula Terbang', description: 'Dapatkan skor 10 untuk pertama kalinya', type: 'score', target: 10, progress: 0, reward: { coins: 100, clovers: 1 }, claimed: false },
                { id: 'ach2', name: 'Kolektor Koin', description: 'Kumpulkan 1000 koin secara total', type: 'totalCoins', target: 1000, progress: 0, reward: { coins: 200, clovers: 2 }, claimed: false },
                { id: 'ach3', name: 'Master Pipa', description: 'Lewati 500 pipa secara total', type: 'totalPipes', target: 500, progress: 0, reward: { coins: 300, clovers: 3 }, claimed: false },
                { id: 'ach4', name: 'Pemain Setia', description: 'Mainkan 100 game', type: 'totalGames', target: 100, progress: 0, reward: { coins: 500, clovers: 5 }, claimed: false },
                { id: 'ach5', name: 'Spesialis Burung', description: 'Beli 5 karakter burung', type: 'birdsOwned', target: 5, progress: 0, reward: { coins: 400, clovers: 4 }, claimed: false },
                { id: 'ach6', name: 'Desainer Latar', description: 'Beli 5 latar belakang', type: 'backgroundsOwned', target: 5, progress: 0, reward: { coins: 400, clovers: 4 }, claimed: false },
                { id: 'ach7', name: 'Kolektor Clover', description: 'Kumpulkan 50 clover', type: 'totalClovers', target: 50, progress: 0, reward: { coins: 1000, clovers: 10 }, claimed: false },
                { id: 'ach8', name: 'Pembeli Royal', description: 'Habiskan 2000 koin di toko', type: 'coinsSpent', target: 2000, progress: 0, reward: { coins: 500, clovers: 5 }, claimed: false },
                { id: 'ach9', name: 'Penyelesai Misi', description: 'Selesaikan 50 misi harian', type: 'dailyMissions', target: 50, progress: 0, reward: { coins: 800, clovers: 8 }, claimed: false },
                { id: 'ach10', name: 'Legenda Terbang', description: 'Capai skor 500 dalam satu permainan', type: 'highScore', target: 500, progress: 0, reward: { coins: 1000, clovers: 10 }, claimed: false }
            ]
        };
        
        // Statistik pemain
        let playerStats = {
            totalCoins: 0,
            totalPipes: 0,
            totalGames: 0,
            totalClovers: 0,
            coinsSpent: 0,
            dailyMissions: 0,
            birdsOwned: 1,
            backgroundsOwned: 1,
            lastDailyReset: new Date().toDateString(),
            lastWeeklyReset: getMondayOfCurrentWeek().toDateString()
        };
        
        // DOM Elements
        const gameCanvas = document.getElementById('gameCanvas');
        const scoreDisplay = document.getElementById('score');
        const coinsDisplay = document.getElementById('coins');
        const cloversDisplay = document.getElementById('clovers');
        const finalScoreDisplay = document.getElementById('finalScore');
        const gameStartScreen = document.getElementById('gameStart');
        const gameOverScreen = document.getElementById('gameOver');
        const startButton = document.getElementById('startButton');
        const restartButton = document.getElementById('restartButton');
        const jumpButton = document.getElementById('jumpButton');
        const openShopButton = document.getElementById('openShop');
        const closeShopButton = document.getElementById('closeShop');
        const openMissionsButton = document.getElementById('openMissions');
        const closeMissionsButton = document.getElementById('closeMissions');
        const resetDataButton = document.getElementById('resetData');
        const shopModal = document.getElementById('shopModal');
        const missionsModal = document.getElementById('missionsModal');
        const shopCoinsDisplay = document.getElementById('shopCoins');
        const shopCloversDisplay = document.getElementById('shopClovers');
        const missionsCoinsDisplay = document.getElementById('missionsCoins');
        const missionsCloversDisplay = document.getElementById('missionsClovers');
        const shopItemsContainer = document.getElementById('shopItems');
        const missionsItemsContainer = document.getElementById('missionsItems');
        const categoryButtons = document.querySelectorAll('.category-button');
        const missionsTabs = document.querySelectorAll('.missions-tab');
        const gameArea = document.querySelector('.game-area');
        const converterAmount = document.getElementById('converterAmount');
        const convertToCloverButton = document.getElementById('convertToClover');
        const convertToCoinButton = document.getElementById('convertToCoin');
        
        // ELEMENT BARU: Kontrol Audio
        const audioControl = document.getElementById('audioControl');
        const audioIcon = audioControl.querySelector('.audio-icon');
        
        // =============================================
        // FUNGSI BARU: Backsound Music
        // =============================================
        
        // Inisialisasi backsound music
        function initBackgroundMusic() {
            backgroundMusic = new Audio('Wiwok.mp3');
            backgroundMusic.loop = true;
            backgroundMusic.volume = 0.5; // Volume 50%
            
            // Event listener untuk toggle mute/unmute
            audioControl.addEventListener('click', toggleMusic);
            
            // Cek status mute dari localStorage
            const isMuted = localStorage.getItem('leafpyBirdMusicMuted') === 'true';
            if (isMuted) {
                backgroundMusic.muted = true;
                audioControl.classList.add('muted');
                audioIcon.textContent = '🔇';
            }
        }
        
        // Fungsi untuk memutar backsound
        function playBackgroundMusic() {
            if (backgroundMusic && !backgroundMusic.muted) {
                backgroundMusic.play().catch(e => {
                    console.log('Autoplay prevented, waiting for user interaction');
                });
                isMusicPlaying = true;
            }
        }
        
        // Fungsi untuk menghentikan backsound
        function stopBackgroundMusic() {
            if (backgroundMusic) {
                backgroundMusic.pause();
                backgroundMusic.currentTime = 0;
                isMusicPlaying = false;
            }
        }
        
        // Fungsi untuk toggle mute/unmute
        function toggleMusic() {
            if (backgroundMusic) {
                backgroundMusic.muted = !backgroundMusic.muted;
                
                if (backgroundMusic.muted) {
                    audioControl.classList.add('muted');
                    audioIcon.textContent = '🔇';
                    localStorage.setItem('leafpyBirdMusicMuted', 'true');
                } else {
                    audioControl.classList.remove('muted');
                    audioIcon.textContent = '🔊';
                    localStorage.setItem('leafpyBirdMusicMuted', 'false');
                    
                    // Jika game sedang aktif, putar musik
                    if (gameActive && !isMusicPlaying) {
                        playBackgroundMusic();
                    }
                }
            }
        }
        
        // =============================================
        // FUNGSI LEADERBOARD YANG DIPERBAIKI
        // =============================================
        
        // Generate unique player ID
        function generatePlayerId() {
            const id = 'player_' + Math.random().toString(36).substr(2, 9);
            localStorage.setItem('leafpyBirdPlayerId', id);
            return id;
        }
        
        // Auto submit score when high score changes
        async function autoSubmitScore() {
            if (!leaderboardInitialized || !playerName) return;
            
            const highScore = getHighScore();
            if (highScore === 0) return;
            
            try {
                await db.collection('leaderboard').doc(playerId).set({
                    name: playerName,
                    score: highScore,
                    lastUpdated: firebase.firestore.FieldValue.serverTimestamp()
                }, { merge: true });
                
                console.log('Score automatically submitted to leaderboard');
                loadLeaderboard(); // Refresh leaderboard setelah submit
            } catch (error) {
                console.error('Error auto-submitting score:', error);
            }
        }
        
        // Load leaderboard data
        async function loadLeaderboard() {
            const leaderboardList = document.getElementById('leaderboardList');
            
            if (!leaderboardInitialized) {
                leaderboardList.innerHTML = '<div class="leaderboard-item">Leaderboard Offline</div>';
                return;
            }
            
            // Show loading
            leaderboardList.innerHTML = '<div class="leaderboard-item">Loading...</div>';
            
            try {
                const snapshot = await db.collection('leaderboard')
                    .orderBy('score', 'desc')
                    .limit(10)
                    .get();
                
                leaderboardList.innerHTML = '';
                
                if (snapshot.empty) {
                    leaderboardList.innerHTML = '<div class="leaderboard-item">Belum ada data</div>';
                    return;
                }
                
                let rank = 1;
                let currentPlayerFound = false;
                
                snapshot.forEach(doc => {
                    const data = doc.data();
                    const isCurrentPlayer = doc.id === playerId;
                    if (isCurrentPlayer) currentPlayerFound = true;
                    
                    const item = document.createElement('div');
                    item.className = 'leaderboard-item';
                    if (isCurrentPlayer) {
                        item.classList.add('current-player');
                    }
                    
                    // Add crown for top 3
                    let rankDisplay = rank;
                    if (rank === 1) rankDisplay = '🥇';
                    else if (rank === 2) rankDisplay = '🥈';
                    else if (rank === 3) rankDisplay = '🥉';
                    
                    item.innerHTML = `
                        <div class="leaderboard-rank">${rankDisplay}</div>
                        <div class="leaderboard-name">${data.name}</div>
                        <div class="leaderboard-score">${data.score}</div>
                    `;
                    
                    leaderboardList.appendChild(item);
                    rank++;
                });
                
                // Jika player tidak ada di top 10, tambahkan di bawah
                if (!currentPlayerFound && playerName) {
                    try {
                        const currentPlayerDoc = await db.collection('leaderboard').doc(playerId).get();
                        if (currentPlayerDoc.exists) {
                            const data = currentPlayerDoc.data();
                            const item = document.createElement('div');
                            item.className = 'leaderboard-item current-player';
                            item.innerHTML = `
                                <div class="leaderboard-rank">...</div>
                                <div class="leaderboard-name">${data.name} (Anda)</div>
                                <div class="leaderboard-score">${data.score}</div>
                            `;
                            leaderboardList.appendChild(item);
                        }
                    } catch (error) {
                        console.error('Error loading current player rank:', error);
                    }
                }
                
            } catch (error) {
                console.error('Error loading leaderboard:', error);
                leaderboardList.innerHTML = '<div class="leaderboard-item">Error loading leaderboard</div>';
            }
        }
        
        // Show name input
        function showNameInput() {
            document.getElementById('nameInputSection').style.display = 'block';
            document.getElementById('leaderboardButtons').style.display = 'none';
        }
        
        // Hide name input
        function hideNameInput() {
            document.getElementById('nameInputSection').style.display = 'none';
            document.getElementById('leaderboardButtons').style.display = 'flex';
        }
        
        // Submit player name
        function submitPlayerName() {
            const nameInput = document.getElementById('playerNameInput');
            const name = nameInput.value.trim();
            
            if (name.length < 2 || name.length > 15) {
                alert('Nama harus 2-15 karakter!');
                return;
            }
            
            playerName = name;
            localStorage.setItem('leafpyBirdPlayerName', playerName);
            
            // Enable auto-submit
            autoSubmitEnabled = true;
            localStorage.setItem('leafpyBirdAutoSubmit', 'true');
            
            hideNameInput();
            
            // Auto submit score after setting name
            setTimeout(() => {
                autoSubmitScore();
                alert(`Selamat datang ${playerName}! Skor Anda akan otomatis tersimpan di leaderboard.`);
            }, 500);
        }
        
        // =============================================
        // FUNGSI YANG SUDAH ADA (TIDAK DIUBAH)
        // =============================================
        
        // Local Storage Functions
        function saveGameData() {
            const gameData = {
                coins: coins,
                clovers: clovers,
                highScore: Math.max(score, getHighScore()),
                ownedBirds: birds.filter(bird => bird.owned).map(bird => bird.id),
                ownedBackgrounds: backgrounds.filter(bg => bg.owned).map(bg => bg.id),
                selectedBird: bird.currentBird,
                selectedBackground: currentBackground,
                missions: missions,
                playerStats: playerStats,
                lastDailyReset: playerStats.lastDailyReset,
                lastWeeklyReset: playerStats.lastWeeklyReset
            };
            localStorage.setItem('leafpyBirdData', JSON.stringify(gameData));
            
            // AUTO SUBMIT TO LEADERBOARD JIKA HIGH SCORE BERUBAH
            if (leaderboardInitialized && playerName) {
                const oldHighScore = getHighScore();
                const newHighScore = Math.max(score, oldHighScore);
                
                if (newHighScore > oldHighScore) {
                    setTimeout(autoSubmitScore, 1000); // Delay sedikit
                }
            }
        }
        
        function loadGameData() {
            const savedData = localStorage.getItem('leafpyBirdData');
            if (savedData) {
                const gameData = JSON.parse(savedData);
                
                // Load coins and clovers
                coins = gameData.coins || 0;
                clovers = gameData.clovers || 0;
                
                // Load owned birds
                if (gameData.ownedBirds) {
                    birds.forEach(bird => {
                        bird.owned = gameData.ownedBirds.includes(bird.id);
                        bird.selected = (bird.id === gameData.selectedBird);
                    });
                }
                
                // Load owned backgrounds
                if (gameData.ownedBackgrounds) {
                    backgrounds.forEach(bg => {
                        bg.owned = gameData.ownedBackgrounds.includes(bg.id);
                        bg.selected = (bg.id === gameData.selectedBackground);
                    });
                }
                
                // Load selected items
                if (gameData.selectedBird) {
                    bird.currentBird = gameData.selectedBird;
                }
                if (gameData.selectedBackground) {
                    currentBackground = gameData.selectedBackground;
                }
                
                // Load missions and player stats
                if (gameData.missions) {
                    missions = gameData.missions;
                }
                if (gameData.playerStats) {
                    playerStats = gameData.playerStats;
                }
                
                // Check for daily and weekly resets
                checkForReset();
                
                updateUI();
            }
        }
        
        function getHighScore() {
            const savedData = localStorage.getItem('leafpyBirdData');
            if (savedData) {
                const gameData = JSON.parse(savedData);
                return gameData.highScore || 0;
            }
            return 0;
        }
        
        function resetGameData() {
            if (confirm('Apakah Anda yakin ingin mereset semua data? Semua koin, item yang dibeli, dan high score akan dihapus!')) {
                localStorage.removeItem('leafpyBirdData');
                localStorage.removeItem('leafpyBirdPlayerName');
                localStorage.removeItem('leafpyBirdPlayerId');
                localStorage.removeItem('leafpyBirdAutoSubmit');
                localStorage.removeItem('leafpyBirdMusicMuted');
                
                // Reset game state
                coins = 0;
                clovers = 0;
                score = 0;
                
                // Reset birds
                birds.forEach(bird => {
                    bird.owned = bird.id === 'leafpy';
                    bird.selected = bird.id === 'leafpy';
                });
                
                // Reset backgrounds
                backgrounds.forEach(bg => {
                    bg.owned = bg.id === 'default';
                    bg.selected = bg.id === 'default';
                });
                
                // Reset current selections
                bird.currentBird = 'leafpy';
                currentBackground = 'default';
                
                // Reset player data
                playerName = '';
                playerId = generatePlayerId();
                autoSubmitEnabled = false;
                
                // Reset missions and player stats
                resetMissions();
                resetPlayerStats();
                
                // Reset music state
                if (backgroundMusic) {
                    backgroundMusic.muted = false;
                    audioControl.classList.remove('muted');
                    audioIcon.textContent = '🔊';
                }
                
                updateUI();
                if (shopModal.style.display === 'flex') {
                    renderShopItems('birds');
                }
                
                // Tampilkan form nama lagi
                showNameInput();
                
                alert('Data berhasil direset!');
            }
        }
        
        // Initialize Game
        function init() {
            canvas = gameCanvas;
            ctx = canvas.getContext('2d');
            
            // Load saved data
            loadGameData();
            
            // BARU: Inisialisasi backsound music
            initBackgroundMusic();
            
            // Generate initial background elements
            generateClouds();
            generateTrees();
            generateBushes();
            generateMountains();
            
            // Event Listeners
            startButton.addEventListener('click', startGame);
            restartButton.addEventListener('click', restartGame);
            jumpButton.addEventListener('click', jump);
            openShopButton.addEventListener('click', openShop);
            closeShopButton.addEventListener('click', closeShop);
            openMissionsButton.addEventListener('click', openMissions);
            closeMissionsButton.addEventListener('click', closeMissions);
            resetDataButton.addEventListener('click', resetGameData);
            convertToCloverButton.addEventListener('click', convertToClover);
            convertToCoinButton.addEventListener('click', convertToCoin);
            
            // Leaderboard event listeners
            document.getElementById('refreshLeaderboard').addEventListener('click', loadLeaderboard);
            document.getElementById('submitName').addEventListener('click', submitPlayerName);
            
            // Enter key for name input
            document.getElementById('playerNameInput').addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    submitPlayerName();
                }
            });
            
            // Keyboard controls
            document.addEventListener('keydown', function(e) {
                if (e.code === 'Space') {
                    jump();
                    e.preventDefault();
                }
            });
            
            // Touch controls for mobile
            canvas.addEventListener('touchstart', function(e) {
                jump();
                e.preventDefault();
            });
            
            // Category buttons in shop
            categoryButtons.forEach(button => {
                button.addEventListener('click', function() {
                    categoryButtons.forEach(btn => btn.classList.remove('active'));
                    this.classList.add('active');
                    renderShopItems(this.dataset.category);
                });
            });
            
            // Missions tabs
            missionsTabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    missionsTabs.forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                    renderMissionsItems(this.dataset.tab);
                });
            });
            
            // Save data when page is about to unload
            window.addEventListener('beforeunload', saveGameData);
            
            // Initial render of shop items and missions
            renderShopItems('birds');
            renderMissionsItems('daily');
            
            // Load leaderboard on init
            setTimeout(() => {
                loadLeaderboard();
                
                // Jika sudah ada nama, sembunyikan form input
                if (playerName) {
                    hideNameInput();
                    
                    // Auto-submit jika sudah ada nama dan high score
                    if (getHighScore() > 0) {
                        setTimeout(autoSubmitScore, 2000);
                    }
                }
            }, 1500);
            
            // Start game loop
            gameLoop();
        }
        
        // Game Loop
        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }
        
        // Update Game State
        function update() {
            if (!gameActive) return;
            
            frames++;
            
            // Update bird wing animation
            bird.wingAngle += 0.2 * bird.wingDirection;
            if (bird.wingAngle > 0.5 || bird.wingAngle < -0.5) {
                bird.wingDirection *= -1;
            }
            
            // Apply gravity to bird
            bird.velocity += bird.gravity;
            bird.y += bird.velocity;
            
            // Generate new pipes
            if (frames % 120 === 0) { // Reduced pipe generation frequency
                pipes.push(newPipe());
            }
            
            // Update pipes
            for (let i = pipes.length - 1; i >= 0; i--) {
                pipes[i].x -= 1.5; // Slower pipe movement
                
                // Check if pipe is off screen
                if (pipes[i].x + pipes[i].width < 0) {
                    pipes.splice(i, 1);
                }
                
                // Check for passing pipes and add score - FIXED CONDITION
                if (pipes[i].x + pipes[i].width < bird.x && !pipes[i].passed) {
                    score++;
                    coins += 10;
                    playerStats.totalCoins += 10;
                    playerStats.totalPipes++;
                    pipes[i].passed = true;
                    updateUI();
                    updateMissionsProgress('pipes', 1);
                    updateMissionsProgress('totalPipes', 1);
                    saveGameData(); // Save when coins change
                    
                    // Show coin effect
                    showCoinEffect(pipes[i].x + pipes[i].width/2, pipes[i].top + (pipes[i].bottom - pipes[i].top)/2);
                }
                
                // Check for collisions
                if (checkCollision(bird, pipes[i])) {
                    gameOver();
                }
            }
            
            // Check for collisions with ground or ceiling
            if (bird.y + bird.height >= canvas.height - 80 || bird.y <= 0) {
                gameOver();
            }
            
            // Update background elements
            updateBackgroundElements();
        }
        
        // Show coin effect animation
        function showCoinEffect(x, y) {
            const coinEffect = document.createElement('div');
            coinEffect.className = 'coin-effect';
            coinEffect.textContent = '+10 🪙';
            coinEffect.style.left = x + 'px';
            coinEffect.style.top = y + 'px';
            gameArea.appendChild(coinEffect);
            
            // Remove effect after animation
            setTimeout(() => {
                if (gameArea.contains(coinEffect)) {
                    gameArea.removeChild(coinEffect);
                }
            }, 1000);
        }
        
        // Draw Game
        function draw() {
            // Clear canvas
            ctx.fillStyle = getBackgroundColor();
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            
            // Draw background elements
            drawBackgroundElements();
            
            // Draw pipes
            drawPipes();
            
            // Draw bird
            drawBird();
            
            // Draw ground
            drawGround();
            
            // Draw score
            ctx.fillStyle = 'white';
            ctx.font = '20px Arial';
            ctx.fillText(`Skor: ${score}`, 10, 30);
            
            // Draw coins
            ctx.fillStyle = 'gold';
            ctx.font = '20px Arial';
            ctx.fillText(`🪙 ${coins}`, canvas.width - 80, 30);
            
            // Draw clovers
            ctx.fillStyle = '#4dff4d';
            ctx.font = '20px Arial';
            ctx.fillText(`🍀 ${clovers}`, canvas.width - 80, 60);
            
            // Draw high score
            const highScore = getHighScore();
            if (highScore > 0) {
                ctx.fillStyle = '#ffcc00';
                ctx.font = '14px Arial';
                ctx.fillText(`High Score: ${highScore}`, 10, 50);
            }
        }
        
        // Draw Bird with more details
        function drawBird() {
            const currentBirdData = birds.find(b => b.id === bird.currentBird);
            ctx.save();
            ctx.translate(bird.x + bird.width/2, bird.y + bird.height/2);
            
            // Rotate bird based on velocity for more natural movement
            let rotation = bird.velocity * 0.05;
            ctx.rotate(rotation);
            
            if (currentBirdData.color === 'rainbow') {
                // Draw rainbow bird with gradient
                const gradient = ctx.createLinearGradient(-bird.width/2, -bird.height/2, bird.width/2, bird.height/2);
                gradient.addColorStop(0, 'red');
                gradient.addColorStop(0.2, 'orange');
                gradient.addColorStop(0.4, 'yellow');
                gradient.addColorStop(0.6, 'green');
                gradient.addColorStop(0.8, 'blue');
                gradient.addColorStop(1, 'purple');
                
                ctx.fillStyle = gradient;
            } else {
                ctx.fillStyle = currentBirdData.color;
            }
            
            // Draw bird body (ellipse)
            ctx.beginPath();
            ctx.ellipse(0, 0, bird.width/2, bird.height/2, 0, 0, Math.PI * 2);
            ctx.fill();
            
            // Draw bird wing with animation and yellow color
            ctx.save();
            ctx.translate(-5, 5);
            
            // Wing animation based on frames
            const wingFlap = Math.sin(frames * 0.2) * 0.5;
            
            // Main wing (yellow gradient)
            const wingGradient = ctx.createLinearGradient(-10, -5, 10, 5);
            wingGradient.addColorStop(0, '#FFD700'); // Gold
            wingGradient.addColorStop(0.5, '#FFA500'); // Orange
            wingGradient.addColorStop(1, '#FF8C00'); // Dark Orange
            
            ctx.fillStyle = wingGradient;
            ctx.rotate(Math.PI/4 + wingFlap);
            
            // Draw wing shape
            ctx.beginPath();
            ctx.ellipse(0, 0, 10, 7, 0, 0, Math.PI * 2);
            ctx.fill();
            
            // Wing details - feathers
            ctx.fillStyle = '#FFA500';
            ctx.beginPath();
            ctx.ellipse(-3, -2, 6, 4, Math.PI/6, 0, Math.PI * 2);
            ctx.fill();
            
            ctx.fillStyle = '#FF8C00';
            ctx.beginPath();
            ctx.ellipse(-6, -1, 4, 3, Math.PI/4, 0, Math.PI * 2);
            ctx.fill();
            
            // Wing highlights
            ctx.fillStyle = '#FFEC8B';
            ctx.beginPath();
            ctx.ellipse(2, -1, 3, 2, Math.PI/8, 0, Math.PI * 2);
            ctx.fill();
            
            ctx.restore();
            
            // Draw bird feathers (details)
            ctx.fillStyle = currentBirdData.color === 'rainbow' ? '#FF5722' : darkenColor(currentBirdData.color, 20);
            for (let i = 0; i < 5; i++) {
                ctx.beginPath();
                ctx.ellipse(-bird.width/4 + i*3, -bird.height/4, 3, 6, Math.PI/4, 0, Math.PI * 2);
                ctx.fill();
            }
            
            // Draw bird beak
            ctx.fillStyle = '#FFA500';
            ctx.beginPath();
            ctx.moveTo(bird.width/2 - 5, 0);
            ctx.lineTo(bird.width/2 + 10, -5);
            ctx.lineTo(bird.width/2 + 10, 5);
            ctx.closePath();
            ctx.fill();
            
            // Draw bird eye
            ctx.fillStyle = 'white';
            ctx.beginPath();
            ctx.arc(bird.width/2 - 10, -5, 5, 0, Math.PI * 2);
            ctx.fill();
            
            ctx.fillStyle = 'black';
            ctx.beginPath();
            ctx.arc(bird.width/2 - 10, -5, 2, 0, Math.PI * 2);
            ctx.fill();
            
            // Draw bird crest
            ctx.fillStyle = currentBirdData.color === 'rainbow' ? '#9C27B0' : darkenColor(currentBirdData.color, 10);
            ctx.beginPath();
            ctx.moveTo(0, -bird.height/2);
            ctx.lineTo(-5, -bird.height/2 - 8);
            ctx.lineTo(5, -bird.height/2 - 5);
            ctx.closePath();
            ctx.fill();
            
            ctx.restore();
        }
        
        // Draw Pipes
        function drawPipes() {
            pipes.forEach(pipe => {
                ctx.fillStyle = '#4CAF50';
                
                // Draw top pipe
                ctx.fillRect(pipe.x, 0, pipe.width, pipe.top);
                
                // Draw bottom pipe
                ctx.fillRect(pipe.x, pipe.bottom, pipe.width, canvas.height - pipe.bottom);
                
                // Draw pipe caps
                ctx.fillStyle = '#388E3C';
                ctx.fillRect(pipe.x - 3, pipe.top - 15, pipe.width + 6, 15);
                ctx.fillRect(pipe.x - 3, pipe.bottom, pipe.width + 6, 15);
                
                // Draw pipe details
                ctx.fillStyle = '#2E7D32';
                for (let i = 5; i < pipe.top; i += 20) {
                    ctx.fillRect(pipe.x + 10, i, pipe.width - 20, 10);
                }
                for (let i = pipe.bottom + 10; i < canvas.height - 80; i += 20) {
                    ctx.fillRect(pipe.x + 10, i, pipe.width - 20, 10);
                }
            });
        }
        
        // Draw Ground
        function drawGround() {
            ctx.fillStyle = '#8B4513';
            ctx.fillRect(0, canvas.height - 80, canvas.width, 80);
            
            ctx.fillStyle = '#5D4037';
            ctx.fillRect(0, canvas.height - 80, canvas.width, 10);
            
            // Draw grass with movement
            ctx.fillStyle = '#4CAF50';
            for (let i = 0; i < canvas.width; i += 20) {
                const wave = Math.sin(frames * 0.05 + i * 0.1) * 3;
                ctx.beginPath();
                ctx.moveTo(i, canvas.height - 80);
                ctx.lineTo(i + 10, canvas.height - 90 + wave);
                ctx.lineTo(i + 20, canvas.height - 80);
                ctx.fill();
            }
        }
        
        // Generate New Pipe with larger gap - FIXED
        function newPipe() {
            const gap = 200; // Increased from 150 to 200 for larger gap
            const minTop = 50; // Minimum top pipe height
            const maxTop = canvas.height - gap - 120; // Adjusted maximum top pipe height
            
            const top = Math.floor(Math.random() * (maxTop - minTop)) + minTop;
            
            return {
                x: canvas.width,
                y: 0,
                width: 50,
                top: top,
                bottom: top + gap,
                passed: false
            };
        }
        
        // Check Collision
        function checkCollision(bird, pipe) {
            return (
                bird.x < pipe.x + pipe.width &&
                bird.x + bird.width > pipe.x &&
                (
                    bird.y < pipe.top ||
                    bird.y + bird.height > pipe.bottom
                )
            );
        }
        
        // Jump Function
        function jump() {
            if (!gameActive) {
                startGame();
                return;
            }
            
            bird.velocity = bird.lift;
        }
        
        // Start Game
        function startGame() {
            gameActive = true;
            gameStartScreen.style.display = 'none';
            score = 0;
            pipes = [];
            bird.y = 150;
            bird.velocity = 0;
            playerStats.totalGames++;
            updateUI();
            
            // BARU: Putar backsound music saat game dimulai
            playBackgroundMusic();
        }
        
        // Restart Game
        function restartGame() {
            gameActive = true;
            gameOverScreen.style.display = 'none';
            score = 0;
            pipes = [];
            bird.y = 150;
            bird.velocity = 0;
            playerStats.totalGames++;
            updateUI();
            
            // BARU: Putar backsound music saat game restart
            playBackgroundMusic();
        }
        
        // Game Over
        function gameOver() {
            gameActive = false;
            gameOverScreen.style.display = 'flex';
            finalScoreDisplay.textContent = score;
            
            // BARU: Hentikan backsound music saat game over
            stopBackgroundMusic();
            
            // Update missions progress
            updateMissionsProgress('score', score);
            updateMissionsProgress('coins', coins);
            updateMissionsProgress('games', 1);
            updateMissionsProgress('highScore', score);
            
            saveGameData(); // Save when game ends
        }
        
        // Update UI
        function updateUI() {
            scoreDisplay.textContent = score;
            coinsDisplay.textContent = coins;
            cloversDisplay.textContent = clovers;
            shopCoinsDisplay.textContent = coins;
            shopCloversDisplay.textContent = clovers;
            missionsCoinsDisplay.textContent = coins;
            missionsCloversDisplay.textContent = clovers;
        }
        
        // Open Shop
        function openShop() {
            shopModal.style.display = 'flex';
            updateUI();
        }
        
        // Close Shop
        function closeShop() {
            shopModal.style.display = 'none';
        }
        
        // Open Missions
        function openMissions() {
            missionsModal.style.display = 'flex';
            updateUI();
            renderMissionsItems('daily');
        }
        
        // Close Missions
        function closeMissions() {
            missionsModal.style.display = 'none';
        }
        
        // Render Shop Items
        function renderShopItems(category) {
            shopItemsContainer.innerHTML = '';
            
            const items = category === 'birds' ? birds : backgrounds;
            
            items.forEach(item => {
                const itemElement = document.createElement('div');
                itemElement.className = 'shop-item';
                
                if (item.selected) itemElement.classList.add('selected');
                if (item.owned) itemElement.classList.add('owned');
                
                itemElement.innerHTML = `
                    <div class="item-image" style="background: ${item.color === 'rainbow' ? 'linear-gradient(45deg, red, orange, yellow, green, blue, purple)' : item.color}">
                        ${category === 'birds' ? item.emoji : '🌄'}
                    </div>
                    <div class="item-name">${item.name}</div>
                    <div class="item-price">
                        ${item.owned ? 'DIMILIKI' : `🪙 ${item.price}`}
                    </div>
                    ${!item.owned ? `<button class="purchase-button" data-id="${item.id}" data-category="${category}">Beli</button>` : ''}
                    ${item.owned && !item.selected ? `<button class="purchase-button" data-id="${item.id}" data-category="${category}">Pilih</button>` : ''}
                `;
                
                shopItemsContainer.appendChild(itemElement);
            });
            
            // Add event listeners to purchase/select buttons
            document.querySelectorAll('.purchase-button').forEach(button => {
                button.addEventListener('click', function() {
                    const id = this.dataset.id;
                    const category = this.dataset.category;
                    
                    if (category === 'birds') {
                        handleBirdPurchase(id);
                    } else {
                        handleBackgroundPurchase(id);
                    }
                });
            });
        }
        
        // Handle Bird Purchase
        function handleBirdPurchase(id) {
            const birdItem = birds.find(b => b.id === id);
            
            if (birdItem.owned) {
                // Select the bird
                birds.forEach(b => b.selected = false);
                birdItem.selected = true;
                bird.currentBird = id;
                saveGameData(); // Save when selection changes
                renderShopItems('birds');
            } else if (coins >= birdItem.price) {
                // Purchase the bird
                coins -= birdItem.price;
                playerStats.coinsSpent += birdItem.price;
                playerStats.birdsOwned++;
                birdItem.owned = true;
                birdItem.selected = true;
                birds.forEach(b => {
                    if (b.id !== id) b.selected = false;
                });
                bird.currentBird = id;
                updateUI();
                updateMissionsProgress('purchases', 1);
                updateMissionsProgress('birdsOwned', 1);
                saveGameData(); // Save when purchase happens
                renderShopItems('birds');
            } else {
                alert('Koin tidak cukup!');
            }
        }
        
        // Handle Background Purchase
        function handleBackgroundPurchase(id) {
            const bg = backgrounds.find(b => b.id === id);
            
            if (bg.owned) {
                // Select the background
                backgrounds.forEach(b => b.selected = false);
                bg.selected = true;
                currentBackground = id;
                saveGameData(); // Save when selection changes
                renderShopItems('backgrounds');
            } else if (coins >= bg.price) {
                // Purchase the background
                coins -= bg.price;
                playerStats.coinsSpent += bg.price;
                playerStats.backgroundsOwned++;
                bg.owned = true;
                bg.selected = true;
                backgrounds.forEach(b => {
                    if (b.id !== id) b.selected = false;
                });
                currentBackground = id;
                updateUI();
                updateMissionsProgress('purchases', 1);
                updateMissionsProgress('backgroundsOwned', 1);
                saveGameData(); // Save when purchase happens
                renderShopItems('backgrounds');
            } else {
                alert('Koin tidak cukup!');
            }
        }
        
        // Get Background Color
        function getBackgroundColor() {
            const bg = backgrounds.find(b => b.id === currentBackground);
            return bg ? bg.color : '#70c5ce';
        }
        
        // Generate Clouds
        function generateClouds() {
            clouds = [];
            for (let i = 0; i < 5; i++) {
                clouds.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * 200,
                    width: 60 + Math.random() * 40,
                    speed: 0.2 + Math.random() * 0.3 // Slower clouds
                });
            }
        }
        
        // Generate Trees
        function generateTrees() {
            trees = [];
            for (let i = 0; i < 3; i++) {
                trees.push({
                    x: Math.random() * canvas.width,
                    height: 30 + Math.random() * 30,
                    width: 20 + Math.random() * 10,
                    speed: 0.5 + Math.random() * 0.3
                });
            }
        }
        
        // Generate Bushes
        function generateBushes() {
            bushes = [];
            for (let i = 0; i < 4; i++) {
                bushes.push({
                    x: Math.random() * canvas.width,
                    size: 15 + Math.random() * 10,
                    speed: 0.8 + Math.random() * 0.4
                });
            }
        }
        
        // Generate Mountains
        function generateMountains() {
            mountains = [];
            for (let i = 0; i < 2; i++) {
                mountains.push({
                    x: Math.random() * canvas.width,
                    height: 80 + Math.random() * 40,
                    width: 100 + Math.random() * 50,
                    speed: 0.1 + Math.random() * 0.1
                });
            }
        }
        
        // Update Background Elements
        function updateBackgroundElements() {
            // Update clouds
            clouds.forEach(cloud => {
                cloud.x -= cloud.speed;
                if (cloud.x + cloud.width < 0) {
                    cloud.x = canvas.width;
                    cloud.y = Math.random() * 200;
                }
            });
            
            // Update trees
            trees.forEach(tree => {
                tree.x -= tree.speed;
                if (tree.x + tree.width < 0) {
                    tree.x = canvas.width;
                    tree.height = 30 + Math.random() * 30;
                }
            });
            
            // Update bushes
            bushes.forEach(bush => {
                bush.x -= bush.speed;
                if (bush.x + bush.size * 2 < 0) {
                    bush.x = canvas.width;
                    bush.size = 15 + Math.random() * 10;
                }
            });
            
            // Update mountains
            mountains.forEach(mountain => {
                mountain.x -= mountain.speed;
                if (mountain.x + mountain.width < 0) {
                    mountain.x = canvas.width;
                    mountain.height = 80 + Math.random() * 40;
                    mountain.width = 100 + Math.random() * 50;
                }
            });
        }
        
        // Draw Background Elements
        function drawBackgroundElements() {
            // Draw mountains
            mountains.forEach(mountain => {
                ctx.fillStyle = 'rgba(100, 100, 100, 0.5)';
                ctx.beginPath();
                ctx.moveTo(mountain.x, canvas.height - 80);
                ctx.lineTo(mountain.x + mountain.width/2, canvas.height - 80 - mountain.height);
                ctx.lineTo(mountain.x + mountain.width, canvas.height - 80);
                ctx.closePath();
                ctx.fill();
            });
            
            // Draw clouds
            clouds.forEach(cloud => {
                ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
                ctx.beginPath();
                ctx.arc(cloud.x, cloud.y, cloud.width/3, 0, Math.PI * 2);
                ctx.arc(cloud.x + cloud.width/3, cloud.y - 10, cloud.width/3, 0, Math.PI * 2);
                ctx.arc(cloud.x + cloud.width/1.5, cloud.y, cloud.width/3, 0, Math.PI * 2);
                ctx.fill();
            });
            
            // Draw trees
            trees.forEach(tree => {
                // Draw trunk
                ctx.fillStyle = '#8B4513';
                ctx.fillRect(tree.x, canvas.height - 80 - tree.height, tree.width/3, tree.height);
                
                // Draw leaves
                ctx.fillStyle = '#2E7D32';
                ctx.beginPath();
                ctx.arc(tree.x + tree.width/6, canvas.height - 80 - tree.height - 15, tree.width/2, 0, Math.PI * 2);
                ctx.fill();
            });
            
            // Draw bushes
            bushes.forEach(bush => {
                ctx.fillStyle = '#2E7D32';
                ctx.beginPath();
                ctx.arc(bush.x, canvas.height - 80, bush.size, 0, Math.PI * 2);
                ctx.arc(bush.x + bush.size, canvas.height - 80 - bush.size/2, bush.size, 0, Math.PI * 2);
                ctx.arc(bush.x + bush.size * 2, canvas.height - 80, bush.size, 0, Math.PI * 2);
                ctx.fill();
            });
        }
        
        // Utility function to darken a color
        function darkenColor(color, percent) {
            // Simplified version - in a real implementation you would parse the color
            return color; // Placeholder
        }
        
        // =============================================
        // FUNGSI BARU UNTUK SISTEM MATA UANG DAN MISI
        // =============================================
        
        // Konversi Coin ke Clover (1 Clover = 100 Coin)
        function convertToClover() {
            const amount = parseInt(converterAmount.value);
            if (isNaN(amount) || amount <= 0) {
                alert('Masukkan jumlah yang valid!');
                return;
            }
            
            if (coins >= amount) {
                const cloverAmount = Math.floor(amount / 100);
                if (cloverAmount > 0) {
                    coins -= cloverAmount * 100;
                    clovers += cloverAmount;
                    playerStats.totalClovers += cloverAmount;
                    updateUI();
                    updateMissionsProgress('totalClovers', cloverAmount);
                    saveGameData();
                    alert(`Berhasil mengkonversi ${cloverAmount * 100} koin menjadi ${cloverAmount} clover!`);
                    converterAmount.value = '';
                } else {
                    alert('Minimal konversi adalah 100 koin untuk 1 clover!');
                }
            } else {
                alert('Koin tidak cukup!');
            }
        }
        
        // Konversi Clover ke Coin (1 Clover = 100 Coin)
        function convertToCoin() {
            const amount = parseInt(converterAmount.value);
            if (isNaN(amount) || amount <= 0) {
                alert('Masukkan jumlah yang valid!');
                return;
            }
            
            if (clovers >= amount) {
                const coinAmount = amount * 100;
                clovers -= amount;
                coins += coinAmount;
                updateUI();
                saveGameData();
                alert(`Berhasil mengkonversi ${amount} clover menjadi ${coinAmount} koin!`);
                converterAmount.value = '';
            } else {
                alert('Clover tidak cukup!');
            }
        }
        
        // Render Misi dan Pencapaian
        function renderMissionsItems(tab) {
            missionsItemsContainer.innerHTML = '';
            
            const items = missions[tab];
            
            if (items.length === 0) {
                missionsItemsContainer.innerHTML = '<p style="text-align: center;">Tidak ada misi yang tersedia.</p>';
                return;
            }
            
            items.forEach(mission => {
                const progressPercent = Math.min((mission.progress / mission.target) * 100, 100);
                const isCompleted = mission.progress >= mission.target;
                
                let missionElement;
                
                if (tab === 'achievements') {
                    missionElement = document.createElement('div');
                    missionElement.className = 'achievement-item';
                    if (isCompleted) missionElement.classList.add('achievement-completed');
                    if (mission.claimed) missionElement.classList.add('achievement-claimed');
                    
                    missionElement.innerHTML = `
                        <div class="achievement-header">
                            <div class="achievement-name">${mission.name}</div>
                            <div class="achievement-reward">
                                ${mission.reward.coins > 0 ? `<span>🪙 ${mission.reward.coins}</span>` : ''}
                                ${mission.reward.clovers > 0 ? `<span>🍀 ${mission.reward.clovers}</span>` : ''}
                            </div>
                        </div>
                        <div class="achievement-description">${mission.description}</div>
                        <div class="achievement-progress">
                            <div class="achievement-progress-bar" style="width: ${progressPercent}%"></div>
                        </div>
                        <div style="display: flex; justify-content: space-between; align-items: center;">
                            <span>${mission.progress}/${mission.target}</span>
                            ${isCompleted && !mission.claimed ? 
                                `<button class="achievement-claim-button" data-id="${mission.id}" data-tab="${tab}">Klaim Hadiah</button>` :
                                (mission.claimed ? '<span style="color: #9C27B0;">Telah Diklaim</span>' : '<span style="color: #FF9800;">Belum Selesai</span>')
                            }
                        </div>
                    `;
                } else {
                    missionElement = document.createElement('div');
                    missionElement.className = 'mission-item';
                    if (isCompleted) missionElement.classList.add('mission-completed');
                    if (mission.claimed) missionElement.classList.add('mission-claimed');
                    
                    missionElement.innerHTML = `
                        <div class="mission-info">
                            <div class="mission-name">${mission.name}</div>
                            <div class="mission-description">${mission.description}</div>
                            <div class="mission-reward">
                                ${mission.reward.coins > 0 ? `<span>🪙 ${mission.reward.coins}</span>` : ''}
                                ${mission.reward.clovers > 0 ? `<span>🍀 ${mission.reward.clovers}</span>` : ''}
                            </div>
                            <div class="mission-progress">
                                <div class="mission-progress-bar" style="width: ${progressPercent}%"></div>
                            </div>
                            <div>${mission.progress}/${mission.target}</div>
                        </div>
                        <div>
                            ${isCompleted && !mission.claimed ? 
                                `<button class="mission-claim-button" data-id="${mission.id}" data-tab="${tab}">Klaim</button>` :
                                (mission.claimed ? '<span style="color: #FF9800;">Telah Diklaim</span>' : '')
                            }
                        </div>
                    `;
                }
                
                missionsItemsContainer.appendChild(missionElement);
            });
            
            // Add event listeners to claim buttons
            document.querySelectorAll('.mission-claim-button, .achievement-claim-button').forEach(button => {
                button.addEventListener('click', function() {
                    const id = this.dataset.id;
                    const tab = this.dataset.tab;
                    claimMissionReward(id, tab);
                });
            });
        }
        
        // Klaim Hadiah Misi
        function claimMissionReward(id, tab) {
            const mission = missions[tab].find(m => m.id === id);
            
            if (mission && mission.progress >= mission.target && !mission.claimed) {
                mission.claimed = true;
                
                // Berikan reward
                if (mission.reward.coins > 0) {
                    coins += mission.reward.coins;
                    playerStats.totalCoins += mission.reward.coins;
                }
                if (mission.reward.clovers > 0) {
                    clovers += mission.reward.clovers;
                    playerStats.totalClovers += mission.reward.clovers;
                }
                
                // Untuk misi harian, tambahkan statistik
                if (tab === 'daily') {
                    playerStats.dailyMissions++;
                    updateMissionsProgress('dailyCompleted', 1);
                }
                
                updateUI();
                saveGameData();
                renderMissionsItems(tab);
                
                alert(`Hadiah berhasil diklaim! Anda mendapatkan: ${mission.reward.coins > 0 ? mission.reward.coins + ' koin ' : ''}${mission.reward.clovers > 0 ? mission.reward.clovers + ' clover' : ''}`);
            }
        }
        
        // Update Progress Misi
        function updateMissionsProgress(type, value) {
            // Update daily missions
            missions.daily.forEach(mission => {
                if (mission.type === type && !mission.claimed) {
                    mission.progress = Math.min(mission.progress + value, mission.target);
                }
            });
            
            // Update weekly missions
            missions.weekly.forEach(mission => {
                if (mission.type === type && !mission.claimed) {
                    mission.progress = Math.min(mission.progress + value, mission.target);
                }
            });
            
            // Update achievements
            missions.achievements.forEach(achievement => {
                if (achievement.type === type && !achievement.claimed) {
                    achievement.progress = Math.min(achievement.progress + value, achievement.target);
                }
            });
            
            // Update berdasarkan statistik pemain
            missions.achievements.forEach(achievement => {
                if (!achievement.claimed) {
                    switch(achievement.type) {
                        case 'totalCoins':
                            achievement.progress = Math.min(playerStats.totalCoins, achievement.target);
                            break;
                        case 'totalPipes':
                            achievement.progress = Math.min(playerStats.totalPipes, achievement.target);
                            break;
                        case 'totalGames':
                            achievement.progress = Math.min(playerStats.totalGames, achievement.target);
                            break;
                        case 'totalClovers':
                            achievement.progress = Math.min(playerStats.totalClovers, achievement.target);
                            break;
                        case 'coinsSpent':
                            achievement.progress = Math.min(playerStats.coinsSpent, achievement.target);
                            break;
                        case 'dailyMissions':
                            achievement.progress = Math.min(playerStats.dailyMissions, achievement.target);
                            break;
                        case 'birdsOwned':
                            achievement.progress = Math.min(playerStats.birdsOwned, achievement.target);
                            break;
                        case 'backgroundsOwned':
                            achievement.progress = Math.min(playerStats.backgroundsOwned, achievement.target);
                            break;
                        case 'highScore':
                            achievement.progress = Math.min(getHighScore(), achievement.target);
                            break;
                    }
                }
            });
        }
        
        // Reset Misi Harian dan Mingguan
        function checkForReset() {
            const now = new Date();
            const today = now.toDateString();
            const monday = getMondayOfCurrentWeek().toDateString();
            
            // Reset misi harian jika sudah hari baru
            if (today !== playerStats.lastDailyReset) {
                resetDailyMissions();
                playerStats.lastDailyReset = today;
            }
            
            // Reset misi mingguan jika sudah minggu baru
            if (monday !== playerStats.lastWeeklyReset) {
                resetWeeklyMissions();
                playerStats.lastWeeklyReset = monday;
            }
        }
        
        function resetDailyMissions() {
            missions.daily.forEach(mission => {
                mission.progress = 0;
                mission.claimed = false;
            });
        }
        
        function resetWeeklyMissions() {
            missions.weekly.forEach(mission => {
                mission.progress = 0;
                mission.claimed = false;
            });
        }
        
        function resetMissions() {
            resetDailyMissions();
            resetWeeklyMissions();
            missions.achievements.forEach(achievement => {
                achievement.progress = 0;
                achievement.claimed = false;
            });
        }
        
        function resetPlayerStats() {
            playerStats = {
                totalCoins: 0,
                totalPipes: 0,
                totalGames: 0,
                totalClovers: 0,
                coinsSpent: 0,
                dailyMissions: 0,
                birdsOwned: 1,
                backgroundsOwned: 1,
                lastDailyReset: new Date().toDateString(),
                lastWeeklyReset: getMondayOfCurrentWeek().toDateString()
            };
        }
        
        // Helper function untuk mendapatkan hari Senin dalam minggu ini
        function getMondayOfCurrentWeek() {
            const today = new Date();
            const day = today.getDay();
            const diff = today.getDate() - day + (day === 0 ? -6 : 1); // adjust when day is Sunday
            return new Date(today.setDate(diff));
        }
        
        // Initialize the game when the page loads
        window.onload = init;
    </script>
</body>
</html>
