<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LeafPy Bird - By:LeafZuya</title>
    <!-- Tambahkan Firebase -->
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-database-compat.js"></script>
    <style>
        /* CSS tetap sama seperti sebelumnya */
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

        .connection-status {
            position: absolute;
            top: 20px;
            left: 20px;
            color: white;
            font-size: 0.8rem;
            background: rgba(0, 0, 0, 0.5);
            padding: 5px 10px;
            border-radius: 10px;
            z-index: 5;
        }

        .online-indicator {
            display: inline-block;
            width: 8px;
            height: 8px;
            border-radius: 50%;
            margin-right: 5px;
        }

        .online {
            background: #4CAF50;
        }

        .offline {
            background: #f44336;
        }

        /* ======================= */
        /* STYLE BARU UNTUK FITUR COIN & SHOP - DIKEMBANGKAN DENGAN SHINY LEAF */
        /* ======================= */
        
        /* Coin Display */
        .coin-display {
            position: absolute;
            top: 20px;
            right: 100px;
            color: #FFD700;
            font-size: 1.2rem;
            text-shadow: 2px 2px 0 #000;
            z-index: 5;
            background: rgba(0, 0, 0, 0.5);
            padding: 5px 10px;
            border-radius: 10px;
            display: flex;
            align-items: center;
        }
        
        .coin-icon {
            margin-right: 5px;
            font-size: 1.3rem;
        }
        
        /* Shiny Leaf Display */
        .leaf-display {
            position: absolute;
            top: 60px;
            right: 100px;
            color: #06D6A0;
            font-size: 1.2rem;
            text-shadow: 2px 2px 0 #000;
            z-index: 5;
            background: rgba(0, 0, 0, 0.5);
            padding: 5px 10px;
            border-radius: 10px;
            display: flex;
            align-items: center;
        }
        
        .leaf-icon {
            margin-right: 5px;
            font-size: 1.3rem;
        }
        
        /* Shop Screen */
        .shop-container {
            background: rgba(0, 0, 0, 0.8);
            padding: 20px;
            border-radius: 10px;
            width: 90%;
            max-width: 320px;
            text-align: center;
            max-height: 80vh;
            overflow-y: auto;
        }
        
        .shop-tabs {
            display: flex;
            justify-content: space-around;
            margin-bottom: 15px;
            border-bottom: 2px solid #FFD700;
        }
        
        .shop-tab {
            background: none;
            border: none;
            color: white;
            padding: 8px 15px;
            cursor: pointer;
            font-size: 1rem;
            transition: all 0.3s ease;
        }
        
        .shop-tab.active {
            color: #FFD700;
            border-bottom: 2px solid #FFD700;
        }
        
        .shop-items {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin: 15px 0;
        }
        
        .shop-item {
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid transparent;
            border-radius: 10px;
            padding: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }
        
        .shop-item:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.05);
        }
        
        .shop-item.owned {
            border-color: #4CAF50;
            background: rgba(76, 175, 80, 0.2);
        }
        
        .shop-item.equipped {
            border-color: #FFD700;
            background: rgba(255, 215, 0, 0.2);
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.5);
        }
        
        .shop-item.locked {
            opacity: 0.6;
            cursor: not-allowed;
        }
        
        .item-emoji {
            font-size: 2rem;
            margin-bottom: 5px;
        }
        
        .item-name {
            color: white;
            font-size: 0.8rem;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .item-price {
            color: #FFD700;
            font-size: 0.8rem;
            margin-bottom: 5px;
        }
        
        .item-price-leaf {
            color: #06D6A0;
            font-size: 0.8rem;
            margin-bottom: 5px;
        }
        
        .item-description {
            color: #CCCCCC;
            font-size: 0.7rem;
            margin-bottom: 5px;
        }

        .item-stack {
            color: #4ECDC4;
            font-size: 0.7rem;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        .shop-buttons {
            display: flex;
            justify-content: space-between;
            margin-top: 10px;
        }
        
        .btn-buy, .btn-equip, .btn-unequip {
            background: linear-gradient(to bottom, #4CAF50, #45a049);
            color: white;
            border: none;
            padding: 5px 10px;
            font-size: 0.8rem;
            border-radius: 15px;
            cursor: pointer;
            flex: 1;
            margin: 0 2px;
        }
        
        .btn-buy-leaf {
            background: linear-gradient(to bottom, #06D6A0, #05B48C);
            color: white;
            border: none;
            padding: 5px 10px;
            font-size: 0.8rem;
            border-radius: 15px;
            cursor: pointer;
            flex: 1;
            margin: 0 2px;
        }
        
        .btn-equip {
            background: linear-gradient(to bottom, #2196F3, #0b7dda);
        }
        
        .btn-unequip {
            background: linear-gradient(to bottom, #ff9800, #e68a00);
        }

        .btn-max-stack {
            background: linear-gradient(to bottom, #666, #444);
            color: #ccc;
            border: none;
            padding: 5px 10px;
            font-size: 0.8rem;
            border-radius: 15px;
            cursor: not-allowed;
            flex: 1;
            margin: 0 2px;
        }
        
        /* Skills Display */
        .skills-display {
            position: absolute;
            bottom: 20px;
            left: 20px;
            display: flex;
            gap: 10px;
            z-index: 5;
        }
        
        .skill-btn {
            background: rgba(0, 0, 0, 0.7);
            border: 2px solid #FFD700;
            color: white;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            font-size: 1.2rem;
            transition: all 0.3s ease;
            position: relative;
        }
        
        .skill-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            border-color: #666;
        }
        
        .skill-btn.active {
            background: rgba(255, 215, 0, 0.3);
            box-shadow: 0 0 10px rgba(255, 215, 0, 0.7);
        }

        .skill-stack {
            position: absolute;
            top: -5px;
            right: -5px;
            background: #FF6B6B;
            color: white;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            font-size: 0.7rem;
            display: flex;
            justify-content: center;
            align-items: center;
            font-weight: bold;
        }
        
        .skill-cooldown {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 0.7rem;
            font-weight: bold;
        }
        
        /* Skill Effects */
        .skill-effect {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 4;
        }
        
        .slow-motion-effect {
            background: rgba(0, 100, 255, 0.2);
        }
        
        .invincible-effect {
            background: rgba(255, 215, 0, 0.3);
            animation: pulse 1s infinite;
        }
        
        .boost-effect {
            background: rgba(255, 0, 0, 0.2);
            animation: shake 0.5s infinite;
        }
        
        @keyframes pulse {
            0% { opacity: 0.3; }
            50% { opacity: 0.6; }
            100% { opacity: 0.3; }
        }
        
        @keyframes shake {
            0% { transform: translateX(0); }
            25% { transform: translateX(-2px); }
            50% { transform: translateX(2px); }
            75% { transform: translateX(-2px); }
            100% { transform: translateX(0); }
        }
        
        /* Background Variants */
        .background-desert {
            background: linear-gradient(to bottom, #FFB347 0%, #FFCC33 100%) !important;
        }
        
        .background-night {
            background: linear-gradient(to bottom, #0F2027 0%, #203A43 50%, #2C5364 100%) !important;
        }
        
        .background-forest {
            background: linear-gradient(to bottom, #1E9600 0%, #FFF200 50%, #FF0000 100%) !important;
        }
        
        .background-ocean {
            background: linear-gradient(to bottom, #1CB5E0 0%, #000046 100%) !important;
        }
        
        .background-space {
            background: linear-gradient(to bottom, #0F0C29 0%, #302B63 50%, #24243E 100%) !important;
        }
        
        /* Pipe Variants */
        .pipe-desert {
            background: #D2691E !important;
            border-color: #8B4513 !important;
        }
        
        .pipe-night {
            background: #2F4F4F !important;
            border-color: #1C2E2E !important;
        }
        
        .pipe-forest {
            background: #228B22 !important;
            border-color: #006400 !important;
        }
        
        .pipe-ocean {
            background: #1E90FF !important;
            border-color: #0000FF !important;
        }
        
        .pipe-space {
            background: #4B0082 !important;
            border-color: #8A2BE2 !important;
        }

        /* Toast Notification */
        .toast {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.8);
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            z-index: 1000;
            animation: fadeInOut 3s ease-in-out;
        }

        @keyframes fadeInOut {
            0% { opacity: 0; top: 0; }
            10% { opacity: 1; top: 20px; }
            90% { opacity: 1; top: 20px; }
            100% { opacity: 0; top: 0; }
        }

        /* Daily Missions Styles */
        .missions-container {
            background: rgba(0, 0, 0, 0.8);
            padding: 20px;
            border-radius: 10px;
            width: 90%;
            max-width: 320px;
            text-align: center;
            max-height: 80vh;
            overflow-y: auto;
        }

        .mission-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 10px;
            margin-bottom: 10px;
            text-align: left;
        }

        .mission-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 5px;
        }

        .mission-title {
            color: white;
            font-size: 0.9rem;
            font-weight: bold;
        }

        .mission-reward {
            color: #06D6A0;
            font-size: 0.8rem;
            font-weight: bold;
            display: flex;
            align-items: center;
        }

        .mission-progress {
            color: #CCCCCC;
            font-size: 0.8rem;
            margin-bottom: 5px;
        }

        .mission-progress-bar {
            height: 6px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 3px;
            overflow: hidden;
        }

        .mission-progress-fill {
            height: 100%;
            background: #06D6A0;
            transition: width 0.3s ease;
        }

        .mission-completed {
            background: rgba(6, 214, 160, 0.2);
            border: 1px solid #06D6A0;
        }

        .btn-claim {
            background: linear-gradient(to bottom, #06D6A0, #05B48C);
            color: white;
            border: none;
            padding: 5px 10px;
            font-size: 0.8rem;
            border-radius: 15px;
            cursor: pointer;
            margin-top: 5px;
        }

        .btn-claim:disabled {
            background: #666;
            cursor: not-allowed;
        }

        /* Shiny Leaf Conversion Styles */
        .conversion-container {
            background: rgba(0, 0, 0, 0.8);
            padding: 20px;
            border-radius: 10px;
            width: 90%;
            max-width: 320px;
            text-align: center;
        }

        .conversion-rate {
            color: #FFD700;
            font-size: 1.2rem;
            margin: 15px 0;
            font-weight: bold;
        }

        .conversion-input {
            width: 100%;
            padding: 10px;
            border-radius: 5px;
            border: none;
            background: rgba(255, 255, 255, 0.9);
            margin-bottom: 15px;
            text-align: center;
            font-size: 1rem;
        }

        .conversion-result {
            color: white;
            font-size: 1rem;
            margin-bottom: 15px;
        }

        /* Tabs for Missions & Achievements */
        .missions-tabs {
            display: flex;
            justify-content: space-around;
            margin-bottom: 15px;
            border-bottom: 2px solid #FFD700;
        }
        
        .missions-tab {
            background: none;
            border: none;
            color: white;
            padding: 8px 15px;
            cursor: pointer;
            font-size: 1rem;
            transition: all 0.3s ease;
        }
        
        .missions-tab.active {
            color: #FFD700;
            border-bottom: 2px solid #FFD700;
        }

        /* Achievement Styles */
        .achievement-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 10px;
            margin-bottom: 10px;
            text-align: left;
        }

        .achievement-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 5px;
        }

        .achievement-title {
            color: white;
            font-size: 0.9rem;
            font-weight: bold;
        }

        .achievement-reward {
            color: #FFD700;
            font-size: 0.8rem;
            font-weight: bold;
            display: flex;
            align-items: center;
        }

        .achievement-progress {
            color: #CCCCCC;
            font-size: 0.8rem;
            margin-bottom: 5px;
        }

        .achievement-progress-bar {
            height: 6px;
            background: rgba(255, 255, 255, 0.2);
            border-radius: 3px;
            overflow: hidden;
        }

        .achievement-progress-fill {
            height: 100%;
            background: #FFD700;
            transition: width 0.3s ease;
        }

        .achievement-completed {
            background: rgba(255, 215, 0, 0.2);
            border: 1px solid #FFD700;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <canvas id="gameCanvas" width="360" height="640"></canvas>
        
        <div class="game-stats">
            Score: <span id="score">0</span>
        </div>
        
        <div class="coin-display">
            <span class="coin-icon">🪙</span>
            <span id="coinCount">0</span>
        </div>

        <div class="leaf-display">
            <span class="leaf-icon">🍃</span>
            <span id="leafCount">0</span>
        </div>

        <div class="user-info" id="userInfo"></div>
        <div class="connection-status" id="connectionStatus">
            <span class="online-indicator offline" id="onlineIndicator"></span>
            <span id="connectionText">Offline</span>
        </div>

        <div class="skills-display" id="skillsDisplay">
            <!-- Skill buttons akan diisi oleh JavaScript -->
        </div>

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
            <h1 class="title">LeafPy Bird</h1>
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
            <h1 class="title">LeafPy Bird</h1>
            
            <div style="display: flex; justify-content: center; gap: 15px; margin-bottom: 15px;">
                <div class="coin-display" style="position: relative; top: 0; right: 0;">
                    <span class="coin-icon">🪙</span>
                    <span id="menuCoinCount">0</span>
                </div>
                <div class="leaf-display" style="position: relative; top: 0; right: 0;">
                    <span class="leaf-icon">🍃</span>
                    <span id="menuLeafCount">0</span>
                </div>
            </div>
            
            <div class="bird-selection" id="birdSelection">
                <!-- Bird options akan diisi oleh JavaScript -->
            </div>
            
            <button id="startBtn" class="btn">Mulai Game</button>
            <button id="shopBtn" class="btn">🛒 Toko</button>
            <button id="missionsBtn" class="btn">📋 Misi & Pencapaian</button>
            <button id="convertBtn" class="btn">💱 Tukar Shiny Leaf</button>
            <button id="leaderboardBtn" class="btn">Leaderboard</button>
            <button id="changeNameBtn" class="btn">Ganti Nama</button>
            <button id="logoutBtn" class="btn">Logout</button>
            <div class="instructions">
                Pilih burung favoritmu!<br>
                Tekan SPACE atau klik untuk terbang<br>
                Hindari pipa merah dan tanah<br>
                Dapatkan 10 koin setiap melewati pipa!<br>
                <small style="color: #FFD700; margin-top: 10px; display: block;">
                    🎵 Backsound Wiwok + 🎬 Transisi Video Bahlil!<br>
                    🏆 Leaderboard Online Real-time!<br>
                    🪙 Koin & Skill Baru!<br>
                    🍃 Shiny Leaf & Misi Harian!<br>
                    🏅 50 Achievement & 20 Misi Mingguan!
                </small>
            </div>
        </div>
        
        <!-- Game Over Screen -->
        <div id="gameOverScreen" class="screen hidden">
            <h1 class="title">Game Over</h1>
            <div class="score-display">Score: <span id="finalScore">0</span></div>
            <div class="score-display" style="font-size: 1.5rem;">Koin: +<span id="coinsEarned">0</span></div>
            <div class="score-display" style="font-size: 1.5rem;">Shiny Leaf: +<span id="leavesEarned">0</span></div>
            <button id="restartBtn" class="btn">Main Lagi</button>
            <button id="menuBtn" class="btn">Menu Utama</button>
        </div>
        
        <!-- Leaderboard Screen -->
        <div id="leaderboardScreen" class="screen hidden">
            <h1 class="title">Leaderboard</h1>
            <div class="leaderboard-container">
                <h2 class="leaderboard-title">Top 10 Pemain</h2>
                <div id="leaderboardStatus" style="color: #FFD700; margin-bottom: 10px; font-size: 0.9rem;">
                    Loading leaderboard...
                </div>
                <div class="leaderboard-list" id="leaderboardList">
                    <!-- Leaderboard akan diisi oleh JavaScript -->
                </div>
                <button id="backBtn" class="btn">Kembali</button>
            </div>
        </div>

        <!-- Shop Screen -->
        <div id="shopScreen" class="screen hidden">
            <h1 class="title">🛒 Toko</h1>
            <div class="shop-container">
                <div style="display: flex; justify-content: center; gap: 15px; margin-bottom: 15px;">
                    <div class="coin-display" style="position: relative; top: 0; right: 0; justify-content: center;">
                        <span class="coin-icon">🪙</span>
                        <span id="shopCoinCount">0</span>
                    </div>
                    <div class="leaf-display" style="position: relative; top: 0; right: 0; justify-content: center;">
                        <span class="leaf-icon">🍃</span>
                        <span id="shopLeafCount">0</span>
                    </div>
                </div>
                
                <div class="shop-tabs">
                    <button class="shop-tab active" data-tab="birds">Burung</button>
                    <button class="shop-tab" data-tab="backgrounds">Latar</button>
                    <button class="shop-tab" data-tab="skills">Skill</button>
                </div>
                
                <div class="shop-content">
                    <div class="shop-tab-content active" id="tab-birds">
                        <div class="shop-items" id="shopBirds">
                            <!-- Items burung akan diisi oleh JavaScript -->
                        </div>
                    </div>
                    <div class="shop-tab-content" id="tab-backgrounds">
                        <div class="shop-items" id="shopBackgrounds">
                            <!-- Items background akan diisi oleh JavaScript -->
                        </div>
                    </div>
                    <div class="shop-tab-content" id="tab-skills">
                        <div class="shop-items" id="shopSkills">
                            <!-- Items skill akan diisi oleh JavaScript -->
                        </div>
                    </div>
                </div>
                
                <button id="shopBackBtn" class="btn">Kembali ke Menu</button>
            </div>
        </div>

        <!-- Missions & Achievements Screen -->
        <div id="missionsScreen" class="screen hidden">
            <h1 class="title">📋 Misi & Pencapaian</h1>
            <div class="missions-container">
                <div class="missions-tabs">
                    <button class="missions-tab active" data-tab="daily">Harian</button>
                    <button class="missions-tab" data-tab="weekly">Mingguan</button>
                    <button class="missions-tab" data-tab="achievements">Achievement</button>
                </div>
                
                <div class="missions-content">
                    <div class="missions-tab-content active" id="tab-daily">
                        <h2 style="color: #FFD700; margin-bottom: 15px;">Misi Harian</h2>
                        <div id="dailyMissionsList">
                            <!-- Misi harian akan diisi oleh JavaScript -->
                        </div>
                        <div id="dailyMissionsStatus" style="color: #CCCCCC; margin: 10px 0; font-size: 0.9rem;">
                            Misi harian akan diperbarui setiap hari
                        </div>
                    </div>
                    <div class="missions-tab-content" id="tab-weekly">
                        <h2 style="color: #FFD700; margin-bottom: 15px;">Misi Mingguan</h2>
                        <div id="weeklyMissionsList">
                            <!-- Misi mingguan akan diisi oleh JavaScript -->
                        </div>
                        <div id="weeklyMissionsStatus" style="color: #CCCCCC; margin: 10px 0; font-size: 0.9rem;">
                            Misi mingguan akan diperbarui setiap minggu
                        </div>
                    </div>
                    <div class="missions-tab-content" id="tab-achievements">
                        <h2 style="color: #FFD700; margin-bottom: 15px;">Achievement</h2>
                        <div id="achievementsList">
                            <!-- Achievement akan diisi oleh JavaScript -->
                        </div>
                        <div id="achievementsStatus" style="color: #CCCCCC; margin: 10px 0; font-size: 0.9rem;">
                            Total Achievement: <span id="achievementsCount">0</span>/50
                        </div>
                    </div>
                </div>
                
                <button id="missionsBackBtn" class="btn">Kembali ke Menu</button>
            </div>
        </div>

        <!-- Shiny Leaf Conversion Screen -->
        <div id="convertScreen" class="screen hidden">
            <h1 class="title">💱 Tukar Shiny Leaf</h1>
            <div class="conversion-container">
                <h2 style="color: #06D6A0; margin-bottom: 15px;">Tukar Shiny Leaf ke Koin</h2>
                <div class="conversion-rate">1 🍃 = 1000 🪙</div>
                <div class="form-group">
                    <label for="leafAmount">Jumlah Shiny Leaf:</label>
                    <input type="number" id="leafAmount" class="conversion-input" min="1" value="1">
                </div>
                <div class="conversion-result" id="conversionResult">
                    Akan mendapatkan: 1000 Koin
                </div>
                <button id="convertConfirmBtn" class="btn" style="background: linear-gradient(to bottom, #06D6A0, #05B48C);">
                    Tukar Sekarang
                </button>
                <div id="convertMessage" style="color: #FF6B6B; margin-top: 10px; min-height: 20px;"></div>
                <button id="convertBackBtn" class="btn">Kembali ke Menu</button>
            </div>
        </div>

        <!-- Ganti Nama Screen -->
        <div id="changeNameScreen" class="screen hidden">
            <h1 class="title">Ganti Nama</h1>
            <div class="login-container">
                <h2 style="color: white; margin-bottom: 20px;">Ganti Username</h2>
                <div class="form-group">
                    <label for="currentUsername">Username Saat Ini:</label>
                    <input type="text" id="currentUsername" class="form-control" readonly style="background: rgba(255,255,255,0.5);">
                </div>
                <div class="form-group">
                    <label for="newUsername">Username Baru:</label>
                    <input type="text" id="newUsername" class="form-control" placeholder="Masukkan username baru">
                </div>
                <div class="form-group">
                    <label for="confirmPassword">Konfirmasi Password:</label>
                    <input type="password" id="confirmPassword" class="form-control" placeholder="Masukkan password untuk konfirmasi">
                </div>
                <button id="saveNameBtn" class="btn" style="width: 100%;">Simpan Perubahan</button>
                <div id="changeNameMessage" style="color: #FF6B6B; margin-top: 10px; min-height: 20px;"></div>
                <button id="backToMenuBtn" class="btn" style="width: 100%; margin-top: 10px; background: #4ECDC4;">Kembali ke Menu</button>
            </div>
        </div>
    </div>

    <script>
        // =======================
        // FIREBASE CONFIGURATION - ONLINE LEADERBOARD
        // =======================
        
        // 🔥 CONFIG FIREBASE ANDA
        const firebaseConfig = {
            apiKey: "AIzaSyAIjChzkl47VERuLIIVKbwa1y7Ygx40Olc",
            authDomain: "leafzuya.firebaseapp.com",
            databaseURL: "https://leafzuya-default-rtdb.asia-southeast1.firebasedatabase.app",
            projectId: "leafzuya",
            storageBucket: "leafzuya.firebasestorage.app",
            messagingSenderId: "808364144065",
            appId: "1:808364144065:web:6bcf48794fe45cd0452b40"
        };

        // Initialize Firebase
        try {
            firebase.initializeApp(firebaseConfig);
            console.log("Firebase berhasil diinisialisasi");
        } catch (error) {
            console.error("Error inisialisasi Firebase:", error);
        }

        const database = firebase.database();

        // =======================
        // USER MANAGEMENT SYSTEM YANG DIPERBAIKI
        // =======================
        const CURRENT_USER_KEY = 'leafpybird_currentUser';
        const USER_DATA_KEY = 'leafpybird_userData';
        const DAILY_MISSIONS_KEY = 'leafpybird_dailyMissions';
        const WEEKLY_MISSIONS_KEY = 'leafpybird_weeklyMissions';
        const ACHIEVEMENTS_KEY = 'leafpybird_achievements';

        // Initialize storage
        function initializeStorage() {
            if (!localStorage.getItem(USER_DATA_KEY)) {
                localStorage.setItem(USER_DATA_KEY, JSON.stringify({}));
            }
            if (!localStorage.getItem(DAILY_MISSIONS_KEY)) {
                localStorage.setItem(DAILY_MISSIONS_KEY, JSON.stringify({}));
            }
            if (!localStorage.getItem(WEEKLY_MISSIONS_KEY)) {
                localStorage.setItem(WEEKLY_MISSIONS_KEY, JSON.stringify({}));
            }
            if (!localStorage.getItem(ACHIEVEMENTS_KEY)) {
                localStorage.setItem(ACHIEVEMENTS_KEY, JSON.stringify({}));
            }
        }

        // Get all users
        function getAllUsers() {
            return JSON.parse(localStorage.getItem(USER_DATA_KEY)) || {};
        }

        // Save all users
        function saveAllUsers(users) {
            localStorage.setItem(USER_DATA_KEY, JSON.stringify(users));
        }

        // Get current user
        function getCurrentUser() {
            return JSON.parse(localStorage.getItem(CURRENT_USER_KEY)) || null;
        }

        // Set current user
        function setCurrentUser(user) {
            localStorage.setItem(CURRENT_USER_KEY, JSON.stringify(user));
        }

        // Clear current user
        function clearCurrentUser() {
            localStorage.removeItem(CURRENT_USER_KEY);
        }

        // Get user data
        function getUserData(username) {
            const allUsers = getAllUsers();
            return allUsers[username] || null;
        }

        // Save user data
        function saveUserData(username, userData) {
            const allUsers = getAllUsers();
            allUsers[username] = userData;
            saveAllUsers(allUsers);
        }

        // Register new user - DIPERBAIKI
        function registerUser(username, password) {
            const allUsers = getAllUsers();
            
            if (allUsers[username]) {
                return { success: false, message: 'Username sudah digunakan' };
            }
            
            // Create new user
            const newUser = {
                username: username,
                password: password,
                createdAt: new Date().toISOString(),
                inventory: {
                    coins: 100, // Memberikan koin awal
                    leaves: 0,
                    ownedBirds: ['default'],
                    ownedBackgrounds: ['default'],
                    ownedSkills: {},
                    equippedBird: 'default',
                    equippedBackground: 'default',
                    equippedSkills: []
                },
                stats: {
                    gamesPlayed: 0,
                    totalScore: 0,
                    bestScore: 0,
                    totalCoins: 0,
                    totalLeaves: 0,
                    pipesPassed: 0,
                    skillsUsed: 0,
                    itemsBought: 0
                }
            };
            
            allUsers[username] = newUser;
            saveAllUsers(allUsers);
            
            return { success: true, message: 'Pendaftaran berhasil' };
        }

        // Login user - DIPERBAIKI
        function loginUser(username, password) {
            const allUsers = getAllUsers();
            const user = allUsers[username];
            
            if (!user) {
                return { success: false, message: 'Username tidak ditemukan' };
            }
            
            if (user.password !== password) {
                return { success: false, message: 'Password salah' };
            }
            
            setCurrentUser(user);
            return { success: true, message: 'Login berhasil' };
        }

        // Get current user inventory
        function getCurrentUserInventory() {
            const currentUser = getCurrentUser();
            if (currentUser && currentUser.inventory) {
                return currentUser.inventory;
            }
            
            return {
                coins: 0,
                leaves: 0,
                ownedBirds: ['default'],
                ownedBackgrounds: ['default'],
                ownedSkills: {},
                equippedBird: 'default',
                equippedBackground: 'default',
                equippedSkills: []
            };
        }

        // Update current user inventory
        function updateCurrentUserInventory(updates) {
            const currentUser = getCurrentUser();
            if (!currentUser) return null;
            
            currentUser.inventory = {
                ...currentUser.inventory,
                ...updates
            };
            
            setCurrentUser(currentUser);
            
            // Update in main storage
            const allUsers = getAllUsers();
            allUsers[currentUser.username] = currentUser;
            saveAllUsers(allUsers);
            
            return currentUser.inventory;
        }

        // Update user stats
        function updateUserStats(updates) {
            const currentUser = getCurrentUser();
            if (!currentUser) return null;
            
            if (!currentUser.stats) {
                currentUser.stats = {
                    gamesPlayed: 0,
                    totalScore: 0,
                    bestScore: 0,
                    totalCoins: 0,
                    totalLeaves: 0,
                    pipesPassed: 0,
                    skillsUsed: 0,
                    itemsBought: 0
                };
            }
            
            currentUser.stats = {
                ...currentUser.stats,
                ...updates
            };
            
            setCurrentUser(currentUser);
            
            // Update in main storage
            const allUsers = getAllUsers();
            allUsers[currentUser.username] = currentUser;
            saveAllUsers(allUsers);
            
            return currentUser.stats;
        }

        // Add coins to current user
        function addCoins(amount) {
            const inventory = getCurrentUserInventory();
            const newCoins = inventory.coins + amount;
            updateCurrentUserInventory({ coins: newCoins });
            updateUserStats({ totalCoins: (currentUser.stats?.totalCoins || 0) + amount });
            return newCoins;
        }

        // Add leaves to current user
        function addLeaves(amount) {
            const inventory = getCurrentUserInventory();
            const newLeaves = inventory.leaves + amount;
            updateCurrentUserInventory({ leaves: newLeaves });
            updateUserStats({ totalLeaves: (currentUser.stats?.totalLeaves || 0) + amount });
            return newLeaves;
        }

        // =======================
        // FITUR GANTI NAMA AKUN
        // =======================

        // DOM Elements untuk ganti nama
        const changeNameScreen = document.getElementById('changeNameScreen');
        const changeNameBtn = document.getElementById('changeNameBtn');
        const currentUsernameInput = document.getElementById('currentUsername');
        const newUsernameInput = document.getElementById('newUsername');
        const confirmPasswordInput = document.getElementById('confirmPassword');
        const saveNameBtn = document.getElementById('saveNameBtn');
        const changeNameMessage = document.getElementById('changeNameMessage');
        const backToMenuBtn = document.getElementById('backToMenuBtn');

        // Fungsi untuk show change name screen
        function showChangeNameScreen() {
            const currentUser = getCurrentUser();
            if (currentUser) {
                currentUsernameInput.value = currentUser.username;
                newUsernameInput.value = '';
                confirmPasswordInput.value = '';
                changeNameMessage.textContent = '';
                
                startScreen.classList.add('hidden');
                changeNameScreen.classList.remove('hidden');
            }
        }

        // Fungsi untuk update username di Firebase
        async function updateUsernameInFirebase(oldUsername, newUsername) {
            try {
                const snapshot = await database.ref('scores')
                    .orderByChild('username')
                    .equalTo(oldUsername)
                    .once('value');
                
                const updates = {};
                
                snapshot.forEach((childSnapshot) => {
                    updates[childSnapshot.key] = {
                        ...childSnapshot.val(),
                        username: newUsername
                    };
                });
                
                await database.ref('scores').update(updates);
                console.log('Username berhasil diupdate di Firebase');
                return true;
            } catch (error) {
                console.error('Error update username di Firebase:', error);
                return false;
            }
        }

        // Fungsi untuk ganti username
        function changeUsername(oldUsername, newUsername, password) {
            const allUsers = getAllUsers();
            const user = allUsers[oldUsername];
            
            if (!user) {
                return { success: false, message: 'User tidak ditemukan' };
            }
            
            if (user.password !== password) {
                return { success: false, message: 'Password salah' };
            }
            
            if (allUsers[newUsername]) {
                return { success: false, message: 'Username baru sudah digunakan' };
            }
            
            if (newUsername.length < 3) {
                return { success: false, message: 'Username minimal 3 karakter' };
            }
            
            // Update username
            user.username = newUsername;
            allUsers[newUsername] = user;
            delete allUsers[oldUsername];
            saveAllUsers(allUsers);
            
            // Update current user
            setCurrentUser(user);
            
            return { success: true, message: 'Username berhasil diubah' };
        }

        // Event Listeners untuk fitur ganti nama
        changeNameBtn.addEventListener('click', showChangeNameScreen);

        saveNameBtn.addEventListener('click', async () => {
            const currentUser = getCurrentUser();
            const newUsername = newUsernameInput.value.trim();
            const password = confirmPasswordInput.value;
            
            changeNameMessage.textContent = '';
            
            if (!newUsername || !password) {
                changeNameMessage.textContent = 'Username baru dan password harus diisi';
                return;
            }
            
            if (newUsername === currentUser.username) {
                changeNameMessage.textContent = 'Username baru harus berbeda dengan yang lama';
                return;
            }
            
            const result = changeUsername(currentUser.username, newUsername, password);
            changeNameMessage.textContent = result.message;
            
            if (result.success) {
                const firebaseSuccess = await updateUsernameInFirebase(currentUser.username, newUsername);
                
                if (firebaseSuccess) {
                    changeNameMessage.textContent = 'Username berhasil diubah! Score tetap aman.';
                    changeNameMessage.style.color = '#06D6A0';
                    
                    userInfo.textContent = `Halo, ${newUsername}`;
                    
                    setTimeout(() => {
                        changeNameScreen.classList.add('hidden');
                        startScreen.classList.remove('hidden');
                    }, 2000);
                } else {
                    changeNameMessage.textContent = 'Username diubah lokal, tapi gagal di server online';
                    changeNameMessage.style.color = '#FF6B6B';
                }
            } else {
                changeNameMessage.style.color = '#FF6B6B';
            }
        });

        backToMenuBtn.addEventListener('click', () => {
            changeNameScreen.classList.add('hidden');
            startScreen.classList.remove('hidden');
        });

        // =======================
        // FIREBASE LEADERBOARD FUNCTIONS
        // =======================

        // Submit score to Firebase
        async function submitScoreToFirebase(username, score) {
            try {
                const scoreData = {
                    username: username,
                    score: score,
                    timestamp: Date.now(),
                    date: new Date().toLocaleDateString('id-ID')
                };
                
                const newScoreRef = database.ref('scores').push();
                await newScoreRef.set(scoreData);
                
                console.log('Score berhasil disimpan ke Firebase');
                updateConnectionStatus(true);
                return true;
            } catch (error) {
                console.error('Error menyimpan score ke Firebase:', error);
                updateConnectionStatus(false);
                return false;
            }
        }

        // Get leaderboard from Firebase
        function getLeaderboardFromFirebase(callback) {
            database.ref('scores')
                .orderByChild('score')
                .limitToLast(50)
                .once('value')
                .then((snapshot) => {
                    const scores = [];
                    snapshot.forEach((childSnapshot) => {
                        const scoreData = childSnapshot.val();
                        scores.push(scoreData);
                    });
                    
                    const userBestScores = {};
                    scores.forEach(score => {
                        if (!userBestScores[score.username] || score.score > userBestScores[score.username].score) {
                            userBestScores[score.username] = score;
                        }
                    });
                    
                    const leaderboard = Object.values(userBestScores)
                        .sort((a, b) => b.score - a.score)
                        .slice(0, 10);
                    
                    callback(leaderboard);
                    updateConnectionStatus(true);
                })
                .catch((error) => {
                    console.error('Error mengambil leaderboard:', error);
                    callback([]);
                    updateConnectionStatus(false);
                });
        }

        // Real-time leaderboard listener
        function startRealTimeLeaderboard() {
            database.ref('scores')
                .orderByChild('score')
                .limitToLast(50)
                .on('value', (snapshot) => {
                    const scores = [];
                    snapshot.forEach((childSnapshot) => {
                        const scoreData = childSnapshot.val();
                        scores.push(scoreData);
                    });
                    
                    const userBestScores = {};
                    scores.forEach(score => {
                        if (!userBestScores[score.username] || score.score > userBestScores[score.username].score) {
                            userBestScores[score.username] = score;
                        }
                    });
                    
                    const leaderboard = Object.values(userBestScores)
                        .sort((a, b) => b.score - a.score)
                        .slice(0, 10);
                    
                    renderOnlineLeaderboard(leaderboard);
                    updateConnectionStatus(true);
                }, (error) => {
                    console.error('Error real-time leaderboard:', error);
                    updateConnectionStatus(false);
                });
        }

        // Update connection status
        function updateConnectionStatus(isConnected) {
            const indicator = document.getElementById('onlineIndicator');
            const text = document.getElementById('connectionText');
            
            if (isConnected) {
                indicator.className = 'online-indicator online';
                text.textContent = 'Online';
            } else {
                indicator.className = 'online-indicator offline';
                text.textContent = 'Offline';
            }
        }

        // =======================
        // UI MANAGEMENT
        // =======================
        const loginScreen = document.getElementById('loginScreen');
        const startScreen = document.getElementById('startScreen');
        const gameOverScreen = document.getElementById('gameOverScreen');
        const leaderboardScreen = document.getElementById('leaderboardScreen');
        const shopScreen = document.getElementById('shopScreen');
        const missionsScreen = document.getElementById('missionsScreen');
        const convertScreen = document.getElementById('convertScreen');
        const usernameInput = document.getElementById('username');
        const passwordInput = document.getElementById('password');
        const loginBtn = document.getElementById('loginBtn');
        const registerBtn = document.getElementById('registerBtn');
        const loginMessage = document.getElementById('loginMessage');
        const leaderboardBtn = document.getElementById('leaderboardBtn');
        const shopBtn = document.getElementById('shopBtn');
        const missionsBtn = document.getElementById('missionsBtn');
        const convertBtn = document.getElementById('convertBtn');
        const logoutBtn = document.getElementById('logoutBtn');
        const backBtn = document.getElementById('backBtn');
        const shopBackBtn = document.getElementById('shopBackBtn');
        const missionsBackBtn = document.getElementById('missionsBackBtn');
        const convertBackBtn = document.getElementById('convertBackBtn');
        const leaderboardList = document.getElementById('leaderboardList');
        const leaderboardStatus = document.getElementById('leaderboardStatus');
        const userInfo = document.getElementById('userInfo');
        const coinCount = document.getElementById('coinCount');
        const leafCount = document.getElementById('leafCount');
        const menuCoinCount = document.getElementById('menuCoinCount');
        const menuLeafCount = document.getElementById('menuLeafCount');
        const shopCoinCount = document.getElementById('shopCoinCount');
        const shopLeafCount = document.getElementById('shopLeafCount');
        const coinsEarned = document.getElementById('coinsEarned');
        const leavesEarned = document.getElementById('leavesEarned');

        // Show login screen
        function showLoginScreen() {
            loginScreen.classList.remove('hidden');
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.add('hidden');
            shopScreen.classList.add('hidden');
            missionsScreen.classList.add('hidden');
            convertScreen.classList.add('hidden');
            changeNameScreen.classList.add('hidden');
            userInfo.textContent = '';
            
            usernameInput.value = '';
            passwordInput.value = '';
            loginMessage.textContent = '';
        }

        // Show start screen
        function showStartScreen() {
            loginScreen.classList.add('hidden');
            startScreen.classList.remove('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.add('hidden');
            shopScreen.classList.add('hidden');
            missionsScreen.classList.add('hidden');
            convertScreen.classList.add('hidden');
            changeNameScreen.classList.add('hidden');
            
            const currentUser = getCurrentUser();
            if (currentUser) {
                userInfo.textContent = `Halo, ${currentUser.username}`;
            }
            
            updateCurrencyDisplay();
            initBirdSelection();
            startRealTimeLeaderboard();
        }

        // Show leaderboard screen
        function showLeaderboardScreen() {
            loginScreen.classList.add('hidden');
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.remove('hidden');
            shopScreen.classList.add('hidden');
            missionsScreen.classList.add('hidden');
            convertScreen.classList.add('hidden');
            changeNameScreen.classList.add('hidden');
            
            leaderboardStatus.textContent = 'Loading leaderboard...';
            getLeaderboardFromFirebase(renderOnlineLeaderboard);
        }

        // Show shop screen
        function showShopScreen() {
            loginScreen.classList.add('hidden');
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.add('hidden');
            shopScreen.classList.remove('hidden');
            missionsScreen.classList.add('hidden');
            convertScreen.classList.add('hidden');
            changeNameScreen.classList.add('hidden');
            
            updateCurrencyDisplay();
            renderShopItems();
        }

        // Show missions screen
        function showMissionsScreen() {
            loginScreen.classList.add('hidden');
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.add('hidden');
            shopScreen.classList.add('hidden');
            missionsScreen.classList.remove('hidden');
            convertScreen.classList.add('hidden');
            changeNameScreen.classList.add('hidden');
            
            updateCurrencyDisplay();
            renderAllMissions();
        }

        // Show convert screen
        function showConvertScreen() {
            loginScreen.classList.add('hidden');
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            leaderboardScreen.classList.add('hidden');
            shopScreen.classList.add('hidden');
            missionsScreen.classList.add('hidden');
            convertScreen.classList.remove('hidden');
            changeNameScreen.classList.add('hidden');
            
            updateCurrencyDisplay();
            updateConversionDisplay();
        }

        // Update currency display
        function updateCurrencyDisplay() {
            const inventory = getCurrentUserInventory();
            coinCount.textContent = inventory.coins;
            leafCount.textContent = inventory.leaves;
            menuCoinCount.textContent = inventory.coins;
            menuLeafCount.textContent = inventory.leaves;
            shopCoinCount.textContent = inventory.coins;
            shopLeafCount.textContent = inventory.leaves;
        }

        // Render online leaderboard
        function renderOnlineLeaderboard(onlineScores) {
            const currentUser = getCurrentUser();
            
            leaderboardList.innerHTML = '';
            leaderboardStatus.textContent = `Leaderboard Online (${onlineScores.length} pemain)`;
            
            if (onlineScores.length === 0) {
                leaderboardList.innerHTML = '<div style="color: white; text-align: center; padding: 20px;">Belum ada data leaderboard</div>';
                return;
            }
            
            onlineScores.forEach((score, index) => {
                const item = document.createElement('div');
                item.className = `leaderboard-item ${currentUser && score.username === currentUser.username ? 'current-user' : ''}`;
                
                item.innerHTML = `
                    <div class="rank">${index + 1}</div>
                    <div class="username">${score.username}</div>
                    <div class="score-value">${score.score}</div>
                `;
                
                leaderboardList.appendChild(item);
            });
        }

        // Event listeners for login/register - YANG DIPERBAIKI
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
                loginMessage.style.color = '#4CAF50';
                setTimeout(() => {
                    showStartScreen();
                    // Initialize missions for user
                    initDailyMissions();
                    initWeeklyMissions();
                    initAchievements();
                }, 500);
            } else {
                loginMessage.style.color = '#FF6B6B';
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
                loginMessage.style.color = '#4CAF50';
                // Auto login setelah register berhasil
                const loginResult = loginUser(username, password);
                if (loginResult.success) {
                    setTimeout(() => {
                        showStartScreen();
                        // Initialize missions for user
                        initDailyMissions();
                        initWeeklyMissions();
                        initAchievements();
                    }, 500);
                }
            } else {
                loginMessage.style.color = '#FF6B6B';
            }
        });

        leaderboardBtn.addEventListener('click', showLeaderboardScreen);
        shopBtn.addEventListener('click', showShopScreen);
        missionsBtn.addEventListener('click', showMissionsScreen);
        convertBtn.addEventListener('click', showConvertScreen);
        backBtn.addEventListener('click', showStartScreen);
        shopBackBtn.addEventListener('click', showStartScreen);
        missionsBackBtn.addEventListener('click', showStartScreen);
        convertBackBtn.addEventListener('click', showStartScreen);

        logoutBtn.addEventListener('click', () => {
            clearCurrentUser();
            showLoginScreen();
        });

        // Check if user is already logged in
        function checkLoginStatus() {
            const currentUser = getCurrentUser();
            if (currentUser) {
                showStartScreen();
                // Initialize missions for user
                initDailyMissions();
                initWeeklyMissions();
                initAchievements();
            } else {
                showLoginScreen();
            }
        }

        // =======================
        // DAILY MISSIONS SYSTEM - DIPERBESAR
        // =======================

        // Mission types - 15 MISI HARIAN
        const DAILY_MISSION_TYPES = {
            PLAY_GAMES: { id: 'play_games', title: 'Mainkan Game', description: 'Mainkan {target} game', target: 3, reward: 1 },
            REACH_SCORE: { id: 'reach_score', title: 'Capai Skor Tertentu', description: 'Dapatkan skor {target} dalam satu game', target: 20, reward: 2 },
            COLLECT_COINS: { id: 'collect_coins', title: 'Kumpulkan Koin', description: 'Kumpulkan {target} koin dalam satu game', target: 100, reward: 1 },
            USE_SKILLS: { id: 'use_skills', title: 'Gunakan Skill', description: 'Gunakan skill {target} kali dalam game', target: 5, reward: 1 },
            BUY_ITEMS: { id: 'buy_items', title: 'Beli Item', description: 'Beli {target} item dari toko', target: 2, reward: 2 },
            PASS_PIPES: { id: 'pass_pipes', title: 'Lewati Pipa', description: 'Lewati {target} pipa dalam satu game', target: 10, reward: 1 },
            PLAY_TIME: { id: 'play_time', title: 'Waktu Bermain', description: 'Bermain selama {target} menit', target: 5, reward: 2 },
            USE_BOOST: { id: 'use_boost', title: 'Gunakan Boost', description: 'Gunakan skill boost {target} kali', target: 3, reward: 1 },
            REACH_HIGH_SCORE: { id: 'reach_high_score', title: 'Capai Skor Tinggi', description: 'Dapatkan skor {target} dalam satu game', target: 50, reward: 3 },
            COLLECT_LEAVES: { id: 'collect_leaves', title: 'Kumpulkan Shiny Leaf', description: 'Kumpulkan {target} Shiny Leaf', target: 5, reward: 2 },
            EQUIP_BIRD: { id: 'equip_bird', title: 'Ganti Burung', description: 'Ganti burung {target} kali', target: 3, reward: 1 },
            EQUIP_BACKGROUND: { id: 'equip_background', title: 'Ganti Latar', description: 'Ganti latar belakang {target} kali', target: 2, reward: 1 },
            WATCH_VIDEO: { id: 'watch_video', title: 'Tonton Video', description: 'Tonton {target} video transisi', target: 2, reward: 2 },
            LOGIN_DAILY: { id: 'login_daily', title: 'Login Harian', description: 'Login {target} hari berturut-turut', target: 1, reward: 1 },
            COMPLETE_MISSIONS: { id: 'complete_missions', title: 'Selesaikan Misi', description: 'Selesaikan {target} misi harian', target: 5, reward: 3 }
        };

        // Get daily missions for current user
        function getDailyMissions() {
            const currentUser = getCurrentUser();
            if (!currentUser) return [];
            
            const allMissions = JSON.parse(localStorage.getItem(DAILY_MISSIONS_KEY)) || {};
            const userMissions = allMissions[currentUser.username];
            
            // If no missions or missions are from a different day, reset them
            if (!userMissions || isDifferentDay(userMissions.lastUpdated)) {
                return resetDailyMissions();
            }
            
            return userMissions.missions;
        }

        // Check if it's a different day
        function isDifferentDay(timestamp) {
            const missionDate = new Date(timestamp);
            const today = new Date();
            return missionDate.getDate() !== today.getDate() || 
                   missionDate.getMonth() !== today.getMonth() || 
                   missionDate.getFullYear() !== today.getFullYear();
        }

        // Reset daily missions
        function resetDailyMissions() {
            const currentUser = getCurrentUser();
            if (!currentUser) return [];
            
            const allMissions = JSON.parse(localStorage.getItem(DAILY_MISSIONS_KEY)) || {};
            
            // Get 10 random missions (dari 15 yang tersedia)
            const missionKeys = Object.keys(DAILY_MISSION_TYPES);
            const selectedMissions = [];
            
            // Shuffle mission keys
            const shuffledKeys = missionKeys.sort(() => 0.5 - Math.random());
            
            // Take first 10 missions
            for (let i = 0; i < 10; i++) {
                const missionType = DAILY_MISSION_TYPES[shuffledKeys[i]];
                
                selectedMissions.push({
                    id: missionType.id,
                    title: missionType.title,
                    description: missionType.description.replace('{target}', missionType.target),
                    target: missionType.target,
                    progress: 0,
                    completed: false,
                    claimed: false,
                    reward: missionType.reward
                });
            }
            
            allMissions[currentUser.username] = {
                missions: selectedMissions,
                lastUpdated: new Date().toISOString()
            };
            
            localStorage.setItem(DAILY_MISSIONS_KEY, JSON.stringify(allMissions));
            return selectedMissions;
        }

        // Initialize daily missions for user
        function initDailyMissions() {
            getDailyMissions(); // This will create missions if they don't exist
        }

        // Update daily mission progress
        function updateDailyMissionProgress(missionId, amount = 1) {
            const currentUser = getCurrentUser();
            if (!currentUser) return;
            
            const allMissions = JSON.parse(localStorage.getItem(DAILY_MISSIONS_KEY)) || {};
            const userMissions = allMissions[currentUser.username];
            
            if (!userMissions) return;
            
            const mission = userMissions.missions.find(m => m.id === missionId);
            if (mission && !mission.completed) {
                mission.progress += amount;
                
                if (mission.progress >= mission.target) {
                    mission.progress = mission.target;
                    mission.completed = true;
                }
                
                localStorage.setItem(DAILY_MISSIONS_KEY, JSON.stringify(allMissions));
            }
        }

        // Claim daily mission reward
        function claimDailyMissionReward(missionId) {
            const currentUser = getCurrentUser();
            if (!currentUser) return false;
            
            const allMissions = JSON.parse(localStorage.getItem(DAILY_MISSIONS_KEY)) || {};
            const userMissions = allMissions[currentUser.username];
            
            if (!userMissions) return false;
            
            const mission = userMissions.missions.find(m => m.id === missionId);
            if (mission && mission.completed && !mission.claimed) {
                mission.claimed = true;
                addLeaves(mission.reward);
                updateCurrencyDisplay();
                
                localStorage.setItem(DAILY_MISSIONS_KEY, JSON.stringify(allMissions));
                return true;
            }
            
            return false;
        }

        // =======================
        // WEEKLY MISSIONS SYSTEM - 20 MISI MINGGUAN
        // =======================

        // Weekly mission types - 20 MISI MINGGUAN
        const WEEKLY_MISSION_TYPES = {
            PLAY_GAMES_WEEKLY: { id: 'play_games_weekly', title: 'Mainkan Game Mingguan', description: 'Mainkan {target} game dalam seminggu', target: 10, reward: 5 },
            REACH_SCORE_WEEKLY: { id: 'reach_score_weekly', title: 'Capai Skor Tinggi Mingguan', description: 'Dapatkan skor {target} dalam satu game', target: 100, reward: 10 },
            COLLECT_COINS_WEEKLY: { id: 'collect_coins_weekly', title: 'Kumpulkan Koin Mingguan', description: 'Kumpulkan {target} koin', target: 1000, reward: 5 },
            USE_SKILLS_WEEKLY: { id: 'use_skills_weekly', title: 'Gunakan Skill Mingguan', description: 'Gunakan skill {target} kali', target: 20, reward: 5 },
            BUY_ITEMS_WEEKLY: { id: 'buy_items_weekly', title: 'Beli Item Mingguan', description: 'Beli {target} item dari toko', target: 5, reward: 8 },
            PASS_PIPES_WEEKLY: { id: 'pass_pipes_weekly', title: 'Lewati Pipa Mingguan', description: 'Lewati {target} pipa', target: 100, reward: 5 },
            PLAY_TIME_WEEKLY: { id: 'play_time_weekly', title: 'Waktu Bermain Mingguan', description: 'Bermain selama {target} menit', target: 30, reward: 8 },
            USE_BOOST_WEEKLY: { id: 'use_boost_weekly', title: 'Gunakan Boost Mingguan', description: 'Gunakan skill boost {target} kali', target: 10, reward: 5 },
            REACH_HIGH_SCORE_WEEKLY: { id: 'reach_high_score_weekly', title: 'Capai Skor Sangat Tinggi', description: 'Dapatkan skor {target} dalam satu game', target: 200, reward: 15 },
            COLLECT_LEAVES_WEEKLY: { id: 'collect_leaves_weekly', title: 'Kumpulkan Shiny Leaf Mingguan', description: 'Kumpulkan {target} Shiny Leaf', target: 20, reward: 10 },
            EQUIP_BIRD_WEEKLY: { id: 'equip_bird_weekly', title: 'Ganti Burung Mingguan', description: 'Ganti burung {target} kali', target: 10, reward: 5 },
            EQUIP_BACKGROUND_WEEKLY: { id: 'equip_background_weekly', title: 'Ganti Latar Mingguan', description: 'Ganti latar belakang {target} kali', target: 5, reward: 5 },
            WATCH_VIDEO_WEEKLY: { id: 'watch_video_weekly', title: 'Tonton Video Mingguan', description: 'Tonton {target} video transisi', target: 5, reward: 8 },
            LOGIN_DAILY_WEEKLY: { id: 'login_daily_weekly', title: 'Login Mingguan', description: 'Login {target} hari berturut-turut', target: 7, reward: 15 },
            COMPLETE_MISSIONS_WEEKLY: { id: 'complete_missions_weekly', title: 'Selesaikan Misi Mingguan', description: 'Selesaikan {target} misi harian', target: 20, reward: 12 },
            BUY_PREMIUM_BIRD: { id: 'buy_premium_bird', title: 'Beli Burung Premium', description: 'Beli {target} burung premium dengan Shiny Leaf', target: 1, reward: 20 },
            MAX_SKILL_STACK: { id: 'max_skill_stack', title: 'Max Skill Stack', description: 'Maksimalkan stack {target} skill', target: 2, reward: 15 },
            COMPLETE_WEEKLY_MISSIONS: { id: 'complete_weekly_missions', title: 'Selesaikan Misi Mingguan', description: 'Selesaikan {target} misi mingguan', target: 10, reward: 25 },
            REACH_TOP_SCORE: { id: 'reach_top_score', title: 'Capai Skor Top', description: 'Dapatkan skor {target} dalam satu game', target: 500, reward: 30 },
            COLLECT_ALL_BIRDS: { id: 'collect_all_birds', title: 'Koleksi Semua Burung', description: 'Miliki {target} burung berbeda', target: 8, reward: 50 }
        };

        // Get weekly missions for current user
        function getWeeklyMissions() {
            const currentUser = getCurrentUser();
            if (!currentUser) return [];
            
            const allMissions = JSON.parse(localStorage.getItem(WEEKLY_MISSIONS_KEY)) || {};
            const userMissions = allMissions[currentUser.username];
            
            // If no missions or missions are from a different week, reset them
            if (!userMissions || isDifferentWeek(userMissions.lastUpdated)) {
                return resetWeeklyMissions();
            }
            
            return userMissions.missions;
        }

        // Check if it's a different week
        function isDifferentWeek(timestamp) {
            const missionDate = new Date(timestamp);
            const today = new Date();
            
            // Get start of week for both dates (Monday as start of week)
            const missionWeekStart = new Date(missionDate);
            missionWeekStart.setDate(missionDate.getDate() - missionDate.getDay() + (missionDate.getDay() === 0 ? -6 : 1));
            missionWeekStart.setHours(0, 0, 0, 0);
            
            const todayWeekStart = new Date(today);
            todayWeekStart.setDate(today.getDate() - today.getDay() + (today.getDay() === 0 ? -6 : 1));
            todayWeekStart.setHours(0, 0, 0, 0);
            
            return missionWeekStart.getTime() !== todayWeekStart.getTime();
        }

        // Reset weekly missions
        function resetWeeklyMissions() {
            const currentUser = getCurrentUser();
            if (!currentUser) return [];
            
            const allMissions = JSON.parse(localStorage.getItem(WEEKLY_MISSIONS_KEY)) || {};
            
            // Get all 20 weekly missions
            const missionKeys = Object.keys(WEEKLY_MISSION_TYPES);
            const selectedMissions = [];
            
            for (const missionKey of missionKeys) {
                const missionType = WEEKLY_MISSION_TYPES[missionKey];
                
                selectedMissions.push({
                    id: missionType.id,
                    title: missionType.title,
                    description: missionType.description.replace('{target}', missionType.target),
                    target: missionType.target,
                    progress: 0,
                    completed: false,
                    claimed: false,
                    reward: missionType.reward
                });
            }
            
            allMissions[currentUser.username] = {
                missions: selectedMissions,
                lastUpdated: new Date().toISOString()
            };
            
            localStorage.setItem(WEEKLY_MISSIONS_KEY, JSON.stringify(allMissions));
            return selectedMissions;
        }

        // Initialize weekly missions for user
        function initWeeklyMissions() {
            getWeeklyMissions(); // This will create missions if they don't exist
        }

        // Update weekly mission progress
        function updateWeeklyMissionProgress(missionId, amount = 1) {
            const currentUser = getCurrentUser();
            if (!currentUser) return;
            
            const allMissions = JSON.parse(localStorage.getItem(WEEKLY_MISSIONS_KEY)) || {};
            const userMissions = allMissions[currentUser.username];
            
            if (!userMissions) return;
            
            const mission = userMissions.missions.find(m => m.id === missionId);
            if (mission && !mission.completed) {
                mission.progress += amount;
                
                if (mission.progress >= mission.target) {
                    mission.progress = mission.target;
                    mission.completed = true;
                }
                
                localStorage.setItem(WEEKLY_MISSIONS_KEY, JSON.stringify(allMissions));
            }
        }

        // Claim weekly mission reward
        function claimWeeklyMissionReward(missionId) {
            const currentUser = getCurrentUser();
            if (!currentUser) return false;
            
            const allMissions = JSON.parse(localStorage.getItem(WEEKLY_MISSIONS_KEY)) || {};
            const userMissions = allMissions[currentUser.username];
            
            if (!userMissions) return false;
            
            const mission = userMissions.missions.find(m => m.id === missionId);
            if (mission && mission.completed && !mission.claimed) {
                mission.claimed = true;
                addLeaves(mission.reward);
                updateCurrencyDisplay();
                
                localStorage.setItem(WEEKLY_MISSIONS_KEY, JSON.stringify(allMissions));
                return true;
            }
            
            return false;
        }

        // =======================
        // ACHIEVEMENTS SYSTEM - 50 ACHIEVEMENT
        // =======================

        // Achievement types - 50 ACHIEVEMENT
        const ACHIEVEMENT_TYPES = {
            // Game Progress Achievements
            FIRST_GAME: { id: 'first_game', title: 'Pertama Kali Main', description: 'Mainkan game pertama kali', target: 1, reward: 5 },
            SCORE_10: { id: 'score_10', title: 'Skor 10', description: 'Dapatkan skor 10', target: 10, reward: 2 },
            SCORE_50: { id: 'score_50', title: 'Skor 50', description: 'Dapatkan skor 50', target: 50, reward: 5 },
            SCORE_100: { id: 'score_100', title: 'Skor 100', description: 'Dapatkan skor 100', target: 100, reward: 10 },
            SCORE_200: { id: 'score_200', title: 'Skor 200', description: 'Dapatkan skor 200', target: 200, reward: 15 },
            SCORE_500: { id: 'score_500', title: 'Skor 500', description: 'Dapatkan skor 500', target: 500, reward: 25 },
            SCORE_1000: { id: 'score_1000', title: 'Master Skor', description: 'Dapatkan skor 1000', target: 1000, reward: 50 },
            
            // Game Count Achievements
            PLAY_10_GAMES: { id: 'play_10_games', title: 'Pemain Aktif', description: 'Mainkan 10 game', target: 10, reward: 5 },
            PLAY_50_GAMES: { id: 'play_50_games', title: 'Pemain Setia', description: 'Mainkan 50 game', target: 50, reward: 10 },
            PLAY_100_GAMES: { id: 'play_100_games', title: 'Pemang Handal', description: 'Mainkan 100 game', target: 100, reward: 20 },
            PLAY_500_GAMES: { id: 'play_500_games', title: 'Legenda Game', description: 'Mainkan 500 game', target: 500, reward: 50 },
            
            // Coin Achievements
            COLLECT_1000_COINS: { id: 'collect_1000_coins', title: 'Kaya Raya', description: 'Kumpulkan 1000 koin', target: 1000, reward: 10 },
            COLLECT_5000_COINS: { id: 'collect_5000_coins', title: 'Jutawan Koin', description: 'Kumpulkan 5000 koin', target: 5000, reward: 20 },
            COLLECT_10000_COINS: { id: 'collect_10000_coins', title: 'Raja Koin', description: 'Kumpulkan 10000 koin', target: 10000, reward: 40 },
            
            // Leaf Achievements
            COLLECT_10_LEAVES: { id: 'collect_10_leaves', title: 'Pengumpul Daun', description: 'Kumpulkan 10 Shiny Leaf', target: 10, reward: 5 },
            COLLECT_50_LEAVES: { id: 'collect_50_leaves', title: 'Kolektor Daun', description: 'Kumpulkan 50 Shiny Leaf', target: 50, reward: 15 },
            COLLECT_100_LEAVES: { id: 'collect_100_leaves', title: 'Raja Daun', description: 'Kumpulkan 100 Shiny Leaf', target: 100, reward: 30 },
            
            // Pipe Achievements
            PASS_100_PIPES: { id: 'pass_100_pipes', title: 'Penghindar Pipa', description: 'Lewati 100 pipa', target: 100, reward: 5 },
            PASS_500_PIPES: { id: 'pass_500_pipes', title: 'Master Pipa', description: 'Lewati 500 pipa', target: 500, reward: 10 },
            PASS_1000_PIPES: { id: 'pass_1000_pipes', title: 'Legenda Pipa', description: 'Lewati 1000 pipa', target: 1000, reward: 20 },
            
            // Skill Achievements
            USE_10_SKILLS: { id: 'use_10_skills', title: 'Pemula Skill', description: 'Gunakan 10 skill', target: 10, reward: 5 },
            USE_50_SKILLS: { id: 'use_50_skills', title: 'Ahli Skill', description: 'Gunakan 50 skill', target: 50, reward: 10 },
            USE_100_SKILLS: { id: 'use_100_skills', title: 'Master Skill', description: 'Gunakan 100 skill', target: 100, reward: 20 },
            
            // Shop Achievements
            BUY_FIRST_ITEM: { id: 'buy_first_item', title: 'Pembeli Pertama', description: 'Beli item pertama', target: 1, reward: 5 },
            BUY_10_ITEMS: { id: 'buy_10_items', title: 'Shopper Handal', description: 'Beli 10 item', target: 10, reward: 10 },
            BUY_ALL_BIRDS: { id: 'buy_all_birds', title: 'Kolektor Burung', description: 'Beli semua burung', target: 8, reward: 30 },
            BUY_ALL_BACKGROUNDS: { id: 'buy_all_backgrounds', title: 'Kolektor Latar', description: 'Beli semua latar', target: 6, reward: 25 },
            BUY_ALL_SKILLS: { id: 'buy_all_skills', title: 'Master Skill', description: 'Beli semua skill', target: 3, reward: 20 },
            
            // Bird Achievements
            EQUIP_EAGLE: { id: 'equip_eagle', title: 'Pengguna Elang', description: 'Gunakan burung Elang', target: 1, reward: 5 },
            EQUIP_PHOENIX: { id: 'equip_phoenix', title: 'Pengguna Phoenix', description: 'Gunakan burung Phoenix', target: 1, reward: 10 },
            EQUIP_DRAGON: { id: 'equip_dragon', title: 'Pengguna Naga', description: 'Gunakan burung Naga', target: 1, reward: 15 },
            EQUIP_LEAF_WING: { id: 'equip_leaf_wing', title: 'Pengguna Leaf Wing', description: 'Gunakan burung Leaf Wing', target: 1, reward: 20 },
            
            // Video Achievements
            WATCH_FIRST_VIDEO: { id: 'watch_first_video', title: 'Penonton Pertama', description: 'Tonton video pertama', target: 1, reward: 5 },
            WATCH_10_VIDEOS: { id: 'watch_10_videos', title: 'Penonton Setia', description: 'Tonton 10 video', target: 10, reward: 10 },
            
            // Login Achievements
            LOGIN_3_DAYS: { id: 'login_3_days', title: 'Pengguna Aktif', description: 'Login 3 hari berturut-turut', target: 3, reward: 5 },
            LOGIN_7_DAYS: { id: 'login_7_days', title: 'Pengguna Setia', description: 'Login 7 hari berturut-turut', target: 7, reward: 10 },
            LOGIN_30_DAYS: { id: 'login_30_days', title: 'Legenda Login', description: 'Login 30 hari berturut-turut', target: 30, reward: 30 },
            
            // Mission Achievements
            COMPLETE_FIRST_MISSION: { id: 'complete_first_mission', title: 'Penyelesai Misi', description: 'Selesaikan misi pertama', target: 1, reward: 5 },
            COMPLETE_10_MISSIONS: { id: 'complete_10_missions', title: 'Ahli Misi', description: 'Selesaikan 10 misi', target: 10, reward: 10 },
            COMPLETE_50_MISSIONS: { id: 'complete_50_missions', title: 'Master Misi', description: 'Selesaikan 50 misi', target: 50, reward: 20 },
            
            // Leaderboard Achievements
            TOP_10_LEADERBOARD: { id: 'top_10_leaderboard', title: 'Top 10', description: 'Masuk top 10 leaderboard', target: 1, reward: 15 },
            TOP_3_LEADERBOARD: { id: 'top_3_leaderboard', title: 'Top 3', description: 'Masuk top 3 leaderboard', target: 1, reward: 25 },
            TOP_1_LEADERBOARD: { id: 'top_1_leaderboard', title: 'Juara', description: 'Menjadi #1 di leaderboard', target: 1, reward: 50 },
            
            // Special Achievements
            PERFECT_GAME: { id: 'perfect_game', title: 'Game Sempurna', description: 'Dapatkan skor 100 tanpa mati', target: 1, reward: 30 },
            SPEEDRUNNER: { id: 'speedrunner', title: 'Speedrunner', description: 'Dapatkan skor 50 dalam 1 menit', target: 1, reward: 25 },
            SKILL_MASTER: { id: 'skill_master', title: 'Master Skill', description: 'Gunakan semua skill dalam satu game', target: 1, reward: 20 },
            COLLECTOR: { id: 'collector', title: 'Kolektor Ulung', description: 'Kumpulkan semua item di toko', target: 1, reward: 50 },
            LEGENDARY_PLAYER: { id: 'legendary_player', title: 'Pemain Legendaris', description: 'Selesaikan semua achievement', target: 49, reward: 100 }
        };

        // Get achievements for current user
        function getAchievements() {
            const currentUser = getCurrentUser();
            if (!currentUser) return [];
            
            const allAchievements = JSON.parse(localStorage.getItem(ACHIEVEMENTS_KEY)) || {};
            const userAchievements = allAchievements[currentUser.username];
            
            // If no achievements, initialize them
            if (!userAchievements) {
                return initializeAchievements();
            }
            
            return userAchievements.achievements;
        }

        // Initialize achievements
        function initializeAchievements() {
            const currentUser = getCurrentUser();
            if (!currentUser) return [];
            
            const allAchievements = JSON.parse(localStorage.getItem(ACHIEVEMENTS_KEY)) || {};
            
            const achievements = [];
            const achievementKeys = Object.keys(ACHIEVEMENT_TYPES);
            
            for (const achievementKey of achievementKeys) {
                const achievementType = ACHIEVEMENT_TYPES[achievementKey];
                
                achievements.push({
                    id: achievementType.id,
                    title: achievementType.title,
                    description: achievementType.description,
                    target: achievementType.target,
                    progress: 0,
                    completed: false,
                    claimed: false,
                    reward: achievementType.reward
                });
            }
            
            allAchievements[currentUser.username] = {
                achievements: achievements,
                lastUpdated: new Date().toISOString()
            };
            
            localStorage.setItem(ACHIEVEMENTS_KEY, JSON.stringify(allAchievements));
            return achievements;
        }

        // Initialize achievements for user
        function initAchievements() {
            getAchievements(); // This will create achievements if they don't exist
        }

        // Update achievement progress
        function updateAchievementProgress(achievementId, amount = 1) {
            const currentUser = getCurrentUser();
            if (!currentUser) return;
            
            const allAchievements = JSON.parse(localStorage.getItem(ACHIEVEMENTS_KEY)) || {};
            const userAchievements = allAchievements[currentUser.username];
            
            if (!userAchievements) return;
            
            const achievement = userAchievements.achievements.find(a => a.id === achievementId);
            if (achievement && !achievement.completed) {
                achievement.progress += amount;
                
                if (achievement.progress >= achievement.target) {
                    achievement.progress = achievement.target;
                    achievement.completed = true;
                }
                
                localStorage.setItem(ACHIEVEMENTS_KEY, JSON.stringify(allAchievements));
            }
        }

        // Claim achievement reward
        function claimAchievementReward(achievementId) {
            const currentUser = getCurrentUser();
            if (!currentUser) return false;
            
            const allAchievements = JSON.parse(localStorage.getItem(ACHIEVEMENTS_KEY)) || {};
            const userAchievements = allAchievements[currentUser.username];
            
            if (!userAchievements) return false;
            
            const achievement = userAchievements.achievements.find(a => a.id === achievementId);
            if (achievement && achievement.completed && !achievement.claimed) {
                achievement.claimed = true;
                addLeaves(achievement.reward);
                updateCurrencyDisplay();
                
                localStorage.setItem(ACHIEVEMENTS_KEY, JSON.stringify(allAchievements));
                return true;
            }
            
            return false;
        }

        // =======================
        // MISSIONS & ACHIEVEMENTS RENDERING
        // =======================

        // Mission tabs functionality
        document.querySelectorAll('.missions-tab').forEach(tab => {
            tab.addEventListener('click', () => {
                document.querySelectorAll('.missions-tab').forEach(t => {
                    t.classList.remove('active');
                });
                
                tab.classList.add('active');
                
                document.querySelectorAll('.missions-tab-content').forEach(content => {
                    content.classList.remove('active');
                });
                
                const tabId = tab.getAttribute('data-tab');
                document.getElementById(`tab-${tabId}`).classList.add('active');
            });
        });

        // Render all missions and achievements
        function renderAllMissions() {
            renderDailyMissions();
            renderWeeklyMissions();
            renderAchievements();
        }

        // Render daily missions
        function renderDailyMissions() {
            const dailyMissionsList = document.getElementById('dailyMissionsList');
            const dailyMissionsStatus = document.getElementById('dailyMissionsStatus');
            
            dailyMissionsList.innerHTML = '';
            
            const missions = getDailyMissions();
            const completedMissions = missions.filter(m => m.completed).length;
            const totalMissions = missions.length;
            
            dailyMissionsStatus.textContent = `Progress: ${completedMissions}/${totalMissions} misi selesai`;
            
            if (missions.length === 0) {
                dailyMissionsList.innerHTML = '<div style="color: white; text-align: center; padding: 20px;">Tidak ada misi hari ini</div>';
                return;
            }
            
            missions.forEach(mission => {
                const missionItem = document.createElement('div');
                missionItem.className = `mission-item ${mission.completed ? 'mission-completed' : ''}`;
                
                const progressPercent = (mission.progress / mission.target) * 100;
                
                missionItem.innerHTML = `
                    <div class="mission-header">
                        <div class="mission-title">${mission.title}</div>
                        <div class="mission-reward">+${mission.reward} 🍃</div>
                    </div>
                    <div class="mission-progress">${mission.description} (${mission.progress}/${mission.target})</div>
                    <div class="mission-progress-bar">
                        <div class="mission-progress-fill" style="width: ${progressPercent}%"></div>
                    </div>
                    ${mission.completed ? 
                        `<button class="btn-claim" data-mission-type="daily" data-mission-id="${mission.id}" ${mission.claimed ? 'disabled' : ''}>
                            ${mission.claimed ? 'Sudah Diklaim' : 'Klaim Hadiah'}
                        </button>` : 
                        ''
                    }
                `;
                
                dailyMissionsList.appendChild(missionItem);
            });
        }

        // Render weekly missions
        function renderWeeklyMissions() {
            const weeklyMissionsList = document.getElementById('weeklyMissionsList');
            const weeklyMissionsStatus = document.getElementById('weeklyMissionsStatus');
            
            weeklyMissionsList.innerHTML = '';
            
            const missions = getWeeklyMissions();
            const completedMissions = missions.filter(m => m.completed).length;
            const totalMissions = missions.length;
            
            weeklyMissionsStatus.textContent = `Progress: ${completedMissions}/${totalMissions} misi selesai`;
            
            if (missions.length === 0) {
                weeklyMissionsList.innerHTML = '<div style="color: white; text-align: center; padding: 20px;">Tidak ada misi minggu ini</div>';
                return;
            }
            
            missions.forEach(mission => {
                const missionItem = document.createElement('div');
                missionItem.className = `mission-item ${mission.completed ? 'mission-completed' : ''}`;
                
                const progressPercent = (mission.progress / mission.target) * 100;
                
                missionItem.innerHTML = `
                    <div class="mission-header">
                        <div class="mission-title">${mission.title}</div>
                        <div class="mission-reward">+${mission.reward} 🍃</div>
                    </div>
                    <div class="mission-progress">${mission.description} (${mission.progress}/${mission.target})</div>
                    <div class="mission-progress-bar">
                        <div class="mission-progress-fill" style="width: ${progressPercent}%"></div>
                    </div>
                    ${mission.completed ? 
                        `<button class="btn-claim" data-mission-type="weekly" data-mission-id="${mission.id}" ${mission.claimed ? 'disabled' : ''}>
                            ${mission.claimed ? 'Sudah Diklaim' : 'Klaim Hadiah'}
                        </button>` : 
                        ''
                    }
                `;
                
                weeklyMissionsList.appendChild(missionItem);
            });
        }

        // Render achievements
        function renderAchievements() {
            const achievementsList = document.getElementById('achievementsList');
            const achievementsStatus = document.getElementById('achievementsStatus');
            
            achievementsList.innerHTML = '';
            
            const achievements = getAchievements();
            const completedAchievements = achievements.filter(a => a.completed).length;
            const totalAchievements = achievements.length;
            
            achievementsStatus.innerHTML = `Total Achievement: <span id="achievementsCount">${completedAchievements}</span>/${totalAchievements}`;
            
            if (achievements.length === 0) {
                achievementsList.innerHTML = '<div style="color: white; text-align: center; padding: 20px;">Tidak ada achievement</div>';
                return;
            }
            
            achievements.forEach(achievement => {
                const achievementItem = document.createElement('div');
                achievementItem.className = `achievement-item ${achievement.completed ? 'achievement-completed' : ''}`;
                
                const progressPercent = (achievement.progress / achievement.target) * 100;
                
                achievementItem.innerHTML = `
                    <div class="achievement-header">
                        <div class="achievement-title">${achievement.title}</div>
                        <div class="achievement-reward">+${achievement.reward} 🍃</div>
                    </div>
                    <div class="achievement-progress">${achievement.description} (${achievement.progress}/${achievement.target})</div>
                    <div class="achievement-progress-bar">
                        <div class="achievement-progress-fill" style="width: ${progressPercent}%"></div>
                    </div>
                    ${achievement.completed ? 
                        `<button class="btn-claim" data-mission-type="achievement" data-mission-id="${achievement.id}" ${achievement.claimed ? 'disabled' : ''}>
                            ${achievement.claimed ? 'Sudah Diklaim' : 'Klaim Hadiah'}
                        </button>` : 
                        ''
                    }
                `;
                
                achievementsList.appendChild(achievementItem);
            });
        }

        // Add event listeners to claim buttons
        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('btn-claim')) {
                const missionType = e.target.getAttribute('data-mission-type');
                const missionId = e.target.getAttribute('data-mission-id');
                let success = false;
                
                if (missionType === 'daily') {
                    success = claimDailyMissionReward(missionId);
                } else if (missionType === 'weekly') {
                    success = claimWeeklyMissionReward(missionId);
                } else if (missionType === 'achievement') {
                    success = claimAchievementReward(missionId);
                }
                
                if (success) {
                    showToast(`Berhasil mengklaim hadiah!`);
                    renderAllMissions();
                }
            }
        });

        // =======================
        // SHINY LEAF CONVERSION SYSTEM
        // =======================

        // DOM Elements for conversion
        const leafAmountInput = document.getElementById('leafAmount');
        const conversionResult = document.getElementById('conversionResult');
        const convertConfirmBtn = document.getElementById('convertConfirmBtn');
        const convertMessage = document.getElementById('convertMessage');

        // Update conversion display
        function updateConversionDisplay() {
            const leafAmount = parseInt(leafAmountInput.value) || 1;
            const coinAmount = leafAmount * 1000;
            conversionResult.textContent = `Akan mendapatkan: ${coinAmount} Koin`;
            
            const inventory = getCurrentUserInventory();
            if (leafAmount > inventory.leaves) {
                convertConfirmBtn.disabled = true;
                convertMessage.textContent = 'Shiny Leaf tidak cukup';
                convertMessage.style.color = '#FF6B6B';
            } else {
                convertConfirmBtn.disabled = false;
                convertMessage.textContent = '';
            }
        }

        // Convert leaves to coins
        function convertLeavesToCoins(leafAmount) {
            const inventory = getCurrentUserInventory();
            
            if (leafAmount <= 0) {
                return { success: false, message: 'Jumlah Shiny Leaf harus lebih dari 0' };
            }
            
            if (leafAmount > inventory.leaves) {
                return { success: false, message: 'Shiny Leaf tidak cukup' };
            }
            
            const coinAmount = leafAmount * 1000;
            const newLeaves = inventory.leaves - leafAmount;
            const newCoins = inventory.coins + coinAmount;
            
            updateCurrentUserInventory({
                leaves: newLeaves,
                coins: newCoins
            });
            
            updateCurrencyDisplay();
            updateConversionDisplay();
            
            return { 
                success: true, 
                message: `Berhasil menukar ${leafAmount} Shiny Leaf menjadi ${coinAmount} Koin!` 
            };
        }

        // Event listeners for conversion
        leafAmountInput.addEventListener('input', updateConversionDisplay);
        
        convertConfirmBtn.addEventListener('click', () => {
            const leafAmount = parseInt(leafAmountInput.value) || 1;
            const result = convertLeavesToCoins(leafAmount);
            
            convertMessage.textContent = result.message;
            convertMessage.style.color = result.success ? '#06D6A0' : '#FF6B6B';
            
            if (result.success) {
                setTimeout(() => {
                    convertMessage.textContent = '';
                }, 3000);
            }
        });

        // =======================
        // SHOP SYSTEM - DIPERBESAR DENGAN BURUNG PREMIUM
        // =======================

        // Shop items data - DITAMBAH BURUNG PREMIUM
        const SHOP_ITEMS = {
            birds: [
                { id: 'default', name: 'Burung Biasa', emoji: '🐦', price: 0, description: 'Burung standar' },
                { id: 'eagle', name: 'Elang', emoji: '🦅', price: 100, description: 'Terbang lebih stabil' },
                { id: 'parrot', name: 'Parkit', emoji: '🦜', price: 150, description: 'Warna cerah' },
                { id: 'owl', name: 'Burung Hantu', emoji: '🦉', price: 200, description: 'Mata tajam' },
                { id: 'penguin', name: 'Pinguin', emoji: '🐧', price: 250, description: 'Tahan banting' },
                { id: 'flamingo', name: 'Flamingo', emoji: '🦩', price: 300, description: 'Elegant' },
                { id: 'phoenix', name: 'Phoenix', emoji: '🔥', price: 500, description: 'Legendaris' },
                { id: 'dragon', name: 'Naga', emoji: '🐲', price: 1000, description: 'Mitos' },
                // BURUNG PREMIUM DENGAN SHINY LEAF
                { id: 'leaf_wing', name: 'Leaf Wing', emoji: '🍃🐦', price: 0, leafPrice: 100, description: 'Burung daun berkilau', premium: true }
            ],
            backgrounds: [
                { id: 'default', name: 'Standar', emoji: '🌄', price: 0, description: 'Pemandangan biasa' },
                { id: 'desert', name: 'Gurun', emoji: '🏜️', price: 200, description: 'Pasir dan matahari' },
                { id: 'night', name: 'Malam', emoji: '🌃', price: 300, description: 'Bintang-bintang' },
                { id: 'forest', name: 'Hutan', emoji: '🌲', price: 400, description: 'Hijau dan segar' },
                { id: 'ocean', name: 'Lautan', emoji: '🌊', price: 500, description: 'Biru yang menenangkan' },
                { id: 'space', name: 'Luar Angkasa', emoji: '🚀', price: 1000, description: 'Galaksi jauh' }
            ],
            skills: [
                { 
                    id: 'slow_motion', 
                    name: 'Slow Motion', 
                    emoji: '🐌', 
                    price: 300, 
                    description: 'Perlambat waktu 6 detik',
                    duration: 6,
                    effect: 'slow'
                },
                { 
                    id: 'invincible', 
                    name: 'Tembus Tembok', 
                    emoji: '🛡️', 
                    price: 500, 
                    description: 'Tembus pipa 3 detik',
                    duration: 3,
                    effect: 'invincible'
                },
                { 
                    id: 'boost', 
                    name: 'Booster', 
                    emoji: '⚡', 
                    price: 400, 
                    description: 'Cepat & hancurkan pipa',
                    duration: 5,
                    effect: 'boost'
                }
            ]
        };

        // Bird Types - DITAMBAH BURUNG PREMIUM
        const BIRD_TYPES = {
            default: { emoji: '🐦', name: 'Burung Biasa', color: '#FFD700', wingColor: '#FFA500' },
            eagle: { emoji: '🦅', name: 'Elang', color: '#8B4513', wingColor: '#654321' },
            parrot: { emoji: '🦜', name: 'Parkit', color: '#FF69B4', wingColor: '#FF1493' },
            owl: { emoji: '🦉', name: 'Burung Hantu', color: '#808080', wingColor: '#696969' },
            penguin: { emoji: '🐧', name: 'Pinguin', color: '#000000', wingColor: '#2F4F4F' },
            flamingo: { emoji: '🦩', name: 'Flamingo', color: '#FF69B4', wingColor: '#FFB6C1' },
            phoenix: { emoji: '🔥', name: 'Phoenix', color: '#FF4500', wingColor: '#FF8C00' },
            dragon: { emoji: '🐲', name: 'Naga', color: '#8A2BE2', wingColor: '#4B0082' },
            leaf_wing: { emoji: '🍃🐦', name: 'Leaf Wing', color: '#06D6A0', wingColor: '#04A57D' }
        };

        // Shop tab functionality
        document.querySelectorAll('.shop-tab').forEach(tab => {
            tab.addEventListener('click', () => {
                document.querySelectorAll('.shop-tab').forEach(t => {
                    t.classList.remove('active');
                });
                
                tab.classList.add('active');
                
                document.querySelectorAll('.shop-tab-content').forEach(content => {
                    content.classList.remove('active');
                });
                
                const tabId = tab.getAttribute('data-tab');
                document.getElementById(`tab-${tabId}`).classList.add('active');
            });
        });

        // Render shop items
        function renderShopItems() {
            const inventory = getCurrentUserInventory();
            
            // Render birds
            const shopBirds = document.getElementById('shopBirds');
            shopBirds.innerHTML = '';
            
            SHOP_ITEMS.birds.forEach(bird => {
                const isOwned = inventory.ownedBirds.includes(bird.id);
                const isEquipped = inventory.equippedBird === bird.id;
                const isPremium = bird.premium || false;
                
                const item = document.createElement('div');
                item.className = `shop-item ${isOwned ? 'owned' : ''} ${isEquipped ? 'equipped' : ''} ${!isOwned && ((!isPremium && bird.price > inventory.coins) || (isPremium && bird.leafPrice > inventory.leaves)) ? 'locked' : ''}`;
                
                item.innerHTML = `
                    <div class="item-emoji">${bird.emoji}</div>
                    <div class="item-name">${bird.name}</div>
                    ${isPremium ? 
                        `<div class="item-price-leaf">${bird.leafPrice} 🍃</div>` :
                        `<div class="item-price">${bird.price} 🪙</div>`
                    }
                    <div class="item-description">${bird.description}</div>
                    <div class="shop-buttons">
                        ${!isOwned ? 
                            (isPremium ?
                                `<button class="btn-buy-leaf" data-type="bird" data-id="${bird.id}" ${bird.leafPrice > inventory.leaves ? 'disabled' : ''}>
                                    Beli
                                </button>` :
                                `<button class="btn-buy" data-type="bird" data-id="${bird.id}" ${bird.price > inventory.coins ? 'disabled' : ''}>
                                    Beli
                                </button>`
                            ) : 
                            (isEquipped ? 
                                `<button class="btn-unequip" data-type="bird" data-id="${bird.id}">
                                    Lepas
                                </button>` :
                                `<button class="btn-equip" data-type="bird" data-id="${bird.id}">
                                    Pakai
                                </button>`
                            )
                        }
                    </div>
                `;
                
                shopBirds.appendChild(item);
            });
            
            // Render backgrounds
            const shopBackgrounds = document.getElementById('shopBackgrounds');
            shopBackgrounds.innerHTML = '';
            
            SHOP_ITEMS.backgrounds.forEach(bg => {
                const isOwned = inventory.ownedBackgrounds.includes(bg.id);
                const isEquipped = inventory.equippedBackground === bg.id;
                
                const item = document.createElement('div');
                item.className = `shop-item ${isOwned ? 'owned' : ''} ${isEquipped ? 'equipped' : ''} ${!isOwned && bg.price > inventory.coins ? 'locked' : ''}`;
                
                item.innerHTML = `
                    <div class="item-emoji">${bg.emoji}</div>
                    <div class="item-name">${bg.name}</div>
                    <div class="item-price">${bg.price} 🪙</div>
                    <div class="item-description">${bg.description}</div>
                    <div class="shop-buttons">
                        ${!isOwned ? 
                            `<button class="btn-buy" data-type="background" data-id="${bg.id}" ${bg.price > inventory.coins ? 'disabled' : ''}>
                                Beli
                            </button>` : 
                            (isEquipped ? 
                                `<button class="btn-unequip" data-type="background" data-id="${bg.id}">
                                    Lepas
                                </button>` :
                                `<button class="btn-equip" data-type="background" data-id="${bg.id}">
                                    Pakai
                                </button>`
                            )
                        }
                    </div>
                `;
                
                shopBackgrounds.appendChild(item);
            });
            
            // Render skills
            const shopSkills = document.getElementById('shopSkills');
            shopSkills.innerHTML = '';
            
            SHOP_ITEMS.skills.forEach(skill => {
                const isOwned = inventory.ownedSkills.hasOwnProperty(skill.id);
                const stackCount = isOwned ? inventory.ownedSkills[skill.id] : 0;
                const isMaxStack = stackCount >= 10;
                
                const item = document.createElement('div');
                item.className = `shop-item ${isOwned ? 'owned' : ''} ${isMaxStack ? 'locked' : ''}`;
                
                item.innerHTML = `
                    <div class="item-emoji">${skill.emoji}</div>
                    <div class="item-name">${skill.name}</div>
                    <div class="item-price">${skill.price} 🪙</div>
                    <div class="item-description">${skill.description}</div>
                    ${isOwned ? `<div class="item-stack">Stack: ${stackCount}/10</div>` : ''}
                    <div class="shop-buttons">
                        ${isMaxStack ? 
                            `<button class="btn-max-stack" disabled>
                                Skill Max Stack
                            </button>` :
                            `<button class="btn-buy" data-type="skill" data-id="${skill.id}" ${skill.price > inventory.coins ? 'disabled' : ''}>
                                Beli
                            </button>`
                        }
                    </div>
                `;
                
                shopSkills.appendChild(item);
            });
            
            // Add event listeners to shop buttons
            document.querySelectorAll('.btn-buy').forEach(btn => {
                btn.addEventListener('click', handleBuyItem);
            });
            
            document.querySelectorAll('.btn-buy-leaf').forEach(btn => {
                btn.addEventListener('click', handleBuyItem);
            });
            
            document.querySelectorAll('.btn-equip').forEach(btn => {
                btn.addEventListener('click', handleEquipItem);
            });
            
            document.querySelectorAll('.btn-unequip').forEach(btn => {
                btn.addEventListener('click', handleUnequipItem);
            });
        }

        // Handle buying items
        function handleBuyItem(e) {
            const type = e.target.getAttribute('data-type');
            const id = e.target.getAttribute('data-id');
            const inventory = getCurrentUserInventory();
            
            let item;
            if (type === 'bird') {
                item = SHOP_ITEMS.birds.find(b => b.id === id);
            } else if (type === 'background') {
                item = SHOP_ITEMS.backgrounds.find(b => b.id === id);
            } else if (type === 'skill') {
                item = SHOP_ITEMS.skills.find(s => s.id === id);
            }
            
            if (!item) return;
            
            const isPremium = item.premium || false;
            const price = isPremium ? item.leafPrice : item.price;
            const currencyType = isPremium ? 'leaves' : 'coins';
            
            if (inventory[currencyType] >= price) {
                const newCurrency = inventory[currencyType] - price;
                
                if (type === 'bird') {
                    inventory.ownedBirds.push(id);
                } else if (type === 'background') {
                    inventory.ownedBackgrounds.push(id);
                } else if (type === 'skill') {
                    if (!inventory.ownedSkills[id]) {
                        inventory.ownedSkills[id] = 1;
                    } else {
                        inventory.ownedSkills[id]++;
                    }
                }
                
                updateCurrentUserInventory({
                    [currencyType]: newCurrency,
                    ownedBirds: inventory.ownedBirds,
                    ownedBackgrounds: inventory.ownedBackgrounds,
                    ownedSkills: inventory.ownedSkills
                });
                
                updateCurrencyDisplay();
                renderShopItems();
                
                showToast(`Berhasil membeli ${item.name}!`);
                
                // Update buy items mission
                updateDailyMissionProgress('buy_items');
                updateWeeklyMissionProgress('buy_items_weekly');
                
                // Update stats
                updateUserStats({ itemsBought: (currentUser.stats?.itemsBought || 0) + 1 });
                
                // Update achievements
                updateAchievementProgress('buy_first_item');
                updateAchievementProgress('buy_10_items');
                
                // Update specific achievements
                if (type === 'bird') {
                    updateAchievementProgress('buy_all_birds');
                    if (id === 'leaf_wing') {
                        updateAchievementProgress('equip_leaf_wing');
                        updateWeeklyMissionProgress('buy_premium_bird');
                    }
                } else if (type === 'background') {
                    updateAchievementProgress('buy_all_backgrounds');
                } else if (type === 'skill') {
                    updateAchievementProgress('buy_all_skills');
                }
            }
        }

        // Handle equipping items
        function handleEquipItem(e) {
            const type = e.target.getAttribute('data-type');
            const id = e.target.getAttribute('data-id');
            
            if (type === 'bird') {
                updateCurrentUserInventory({
                    equippedBird: id
                });
                
                // Update missions
                updateDailyMissionProgress('equip_bird');
                updateWeeklyMissionProgress('equip_bird_weekly');
                
                // Update achievements
                if (id === 'eagle') updateAchievementProgress('equip_eagle');
                if (id === 'phoenix') updateAchievementProgress('equip_phoenix');
                if (id === 'dragon') updateAchievementProgress('equip_dragon');
                if (id === 'leaf_wing') updateAchievementProgress('equip_leaf_wing');
            } else if (type === 'background') {
                updateCurrentUserInventory({
                    equippedBackground: id
                });
                
                // Update missions
                updateDailyMissionProgress('equip_background');
                updateWeeklyMissionProgress('equip_background_weekly');
            }
            
            renderShopItems();
            updateBirdSelection();
        }

        // Handle unequipping items
        function handleUnequipItem(e) {
            const type = e.target.getAttribute('data-type');
            const id = e.target.getAttribute('data-id');
            
            if (type === 'bird') {
                if (id !== 'default') {
                    updateCurrentUserInventory({
                        equippedBird: 'default'
                    });
                }
            } else if (type === 'background') {
                if (id !== 'default') {
                    updateCurrentUserInventory({
                        equippedBackground: 'default'
                    });
                }
            }
            
            renderShopItems();
            updateBirdSelection();
        }

        // Update bird selection on start screen
        function updateBirdSelection() {
            const inventory = getCurrentUserInventory();
            selectedBirdType = inventory.equippedBird;
            
            document.querySelectorAll('.bird-option').forEach(opt => {
                opt.classList.remove('selected');
                if (opt.getAttribute('data-bird-id') === selectedBirdType) {
                    opt.classList.add('selected');
                }
            });
        }

        // Toast notification
        function showToast(message) {
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = message;
            document.body.appendChild(toast);
            
            setTimeout(() => {
                toast.remove();
            }, 3000);
        }

        // =======================
        // SKILL SYSTEM
        // =======================

        let activeSkills = {};
        let availableSkills = {};

        // Initialize skills display
        function initSkillsDisplay() {
            const skillsDisplay = document.getElementById('skillsDisplay');
            skillsDisplay.innerHTML = '';
            
            const inventory = getCurrentUserInventory();
            
            availableSkills = {};
            
            SHOP_ITEMS.skills.forEach(skill => {
                const stackCount = inventory.ownedSkills[skill.id] || 0;
                if (stackCount > 0) {
                    availableSkills[skill.id] = stackCount;
                    
                    const skillBtn = document.createElement('button');
                    skillBtn.className = 'skill-btn';
                    skillBtn.setAttribute('data-skill-id', skill.id);
                    skillBtn.innerHTML = `
                        ${skill.emoji}
                        <div class="skill-stack">${stackCount}</div>
                    `;
                    
                    skillBtn.addEventListener('click', () => {
                        activateSkill(skill.id);
                    });
                    
                    skillsDisplay.appendChild(skillBtn);
                }
            });
            
            setInterval(updateSkills, 100);
        }

        // Activate a skill
        function activateSkill(skillId) {
            if (!gameRunning || isInTransition) return;
            
            const skill = SHOP_ITEMS.skills.find(s => s.id === skillId);
            if (!skill) return;
            
            if (!availableSkills[skillId] || availableSkills[skillId] <= 0) {
                return;
            }
            
            if (activeSkills[skillId]) {
                return;
            }
            
            availableSkills[skillId]--;
            
            updateSkillButtons();
            
            const now = Date.now();
            activeSkills[skillId] = {
                active: true,
                endTime: now + (skill.duration * 1000)
            };
            
            applySkillEffect(skillId, true);
            
            console.log(`Skill ${skill.name} diaktifkan! Sisa: ${availableSkills[skillId]}`);
            
            showToast(`${skill.name} aktif! Sisa: ${availableSkills[skillId]}`);
            
            // Update use skills mission
            updateDailyMissionProgress('use_skills');
            updateWeeklyMissionProgress('use_skills_weekly');
            
            // Update stats
            updateUserStats({ skillsUsed: (currentUser.stats?.skillsUsed || 0) + 1 });
            
            // Update achievements
            updateAchievementProgress('use_10_skills');
            updateAchievementProgress('use_50_skills');
            updateAchievementProgress('use_100_skills');
            
            // Update specific skill missions
            if (skillId === 'boost') {
                updateDailyMissionProgress('use_boost');
                updateWeeklyMissionProgress('use_boost_weekly');
            }
        }

        // Apply skill effect
        function applySkillEffect(skillId, activate) {
            const skill = SHOP_ITEMS.skills.find(s => s.id === skillId);
            if (!skill) return;
            
            document.querySelector('.skill-effect')?.remove();
            
            if (activate) {
                const effect = document.createElement('div');
                effect.className = `skill-effect ${skill.effect}-effect`;
                document.querySelector('.game-container').appendChild(effect);
                
                switch(skillId) {
                    case 'slow_motion':
                        pipeSpeedMultiplier = 0.5;
                        break;
                    case 'invincible':
                        isInvincible = true;
                        break;
                    case 'boost':
                        isBoosting = true;
                        bird.jump = -10;
                        break;
                }
            } else {
                switch(skillId) {
                    case 'slow_motion':
                        pipeSpeedMultiplier = 1;
                        break;
                    case 'invincible':
                        isInvincible = false;
                        break;
                    case 'boost':
                        isBoosting = false;
                        bird.jump = -7;
                        break;
                }
            }
        }

        // Update skills (check for expiration)
        function updateSkills() {
            if (!gameRunning) return;
            
            const now = Date.now();
            let needEffectUpdate = false;
            
            for (const skillId in activeSkills) {
                const skill = activeSkills[skillId];
                
                if (skill.active && skill.endTime <= now) {
                    skill.active = false;
                    applySkillEffect(skillId, false);
                    needEffectUpdate = true;
                    delete activeSkills[skillId];
                }
            }
            
            if (needEffectUpdate) {
                for (const skillId in activeSkills) {
                    if (activeSkills[skillId].active) {
                        applySkillEffect(skillId, true);
                        break;
                    }
                }
            }
            
            updateSkillButtons();
        }

        // Update skill buttons UI
        function updateSkillButtons() {
            const now = Date.now();
            
            document.querySelectorAll('.skill-btn').forEach(btn => {
                const skillId = btn.getAttribute('data-skill-id');
                const skill = activeSkills[skillId];
                const stackCount = availableSkills[skillId] || 0;
                
                const stackElement = btn.querySelector('.skill-stack');
                if (stackElement) {
                    stackElement.textContent = stackCount;
                }
                
                btn.querySelector('.skill-cooldown')?.remove();
                
                if (skill && skill.active) {
                    btn.classList.add('active');
                    btn.disabled = true;
                    
                    const cooldown = document.createElement('div');
                    cooldown.className = 'skill-cooldown';
                    const remaining = Math.ceil((skill.endTime - now) / 1000);
                    cooldown.textContent = remaining;
                    btn.appendChild(cooldown);
                } else if (stackCount <= 0) {
                    btn.disabled = true;
                    btn.style.opacity = '0.5';
                } else {
                    btn.classList.remove('active');
                    btn.disabled = false;
                    btn.style.opacity = '1';
                }
            });
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
        let selectedBirdType = 'default';
        let coinsCollectedThisGame = 0;
        let leavesCollectedThisGame = 0;
        let pipesPassedThisGame = 0;

        // New game variables for skills
        let pipeSpeedMultiplier = 1;
        let isInvincible = false;
        let isBoosting = false;

        // Background Types
        const BACKGROUND_TYPES = {
            default: { name: 'Standar', class: '' },
            desert: { name: 'Gurun', class: 'background-desert' },
            night: { name: 'Malam', class: 'background-night' },
            forest: { name: 'Hutan', class: 'background-forest' },
            ocean: { name: 'Lautan', class: 'background-ocean' },
            space: { name: 'Luar Angkasa', class: 'background-space' }
        };

        // Pipe Types
        const PIPE_TYPES = {
            default: { topColor: '#FF6B6B', bottomColor: '#FF4757' },
            desert: { topColor: '#D2691E', bottomColor: '#8B4513' },
            night: { topColor: '#2F4F4F', bottomColor: '#1C2E2E' },
            forest: { topColor: '#228B22', bottomColor: '#006400' },
            ocean: { topColor: '#1E90FF', bottomColor: '#0000FF' },
            space: { topColor: '#4B0082', bottomColor: '#8A2BE2' }
        };

        // Video URLs
        const VIDEO_URLS = {
            10: "Bahlil.mp4",
            50: "You.mp4", 
            100: "Bisa.mp4"
        };

        // Music
        const MUSIC_URLS = ["Wiwok.mp3"];
        const CURRENT_MUSIC_URL = MUSIC_URLS[0];

        // Bird Selection
        function initBirdSelection() {
            birdSelection.innerHTML = '';
            
            const inventory = getCurrentUserInventory();
            
            for (const [birdId, birdData] of Object.entries(BIRD_TYPES)) {
                if (inventory.ownedBirds.includes(birdId)) {
                    const birdOption = document.createElement('div');
                    birdOption.className = `bird-option ${birdId === inventory.equippedBird ? 'selected' : ''}`;
                    birdOption.setAttribute('data-bird-id', birdId);
                    birdOption.innerHTML = `
                        <div class="bird-emoji">${birdData.emoji}</div>
                        <div class="bird-name">${birdData.name}</div>
                    `;
                    
                    birdOption.addEventListener('click', () => {
                        if (inventory.ownedBirds.includes(birdId)) {
                            document.querySelectorAll('.bird-option').forEach(opt => {
                                opt.classList.remove('selected');
                            });
                            birdOption.classList.add('selected');
                            selectedBirdType = birdId;
                            
                            updateCurrentUserInventory({
                                equippedBird: birdId
                            });
                        }
                    });
                    
                    birdSelection.appendChild(birdOption);
                }
            }
        }

        // Bird Object (modified)
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
                
                // Special drawing for Leaf Wing bird
                if (this.type === 'leaf_wing') {
                    // Draw shiny leaf effect
                    ctx.fillStyle = birdData.color;
                    ctx.beginPath();
                    ctx.ellipse(0, 0, this.width/2, this.height/2, 0, 0, Math.PI * 2);
                    ctx.fill();
                    
                    // Draw leaf pattern
                    ctx.fillStyle = birdData.wingColor;
                    ctx.beginPath();
                    ctx.ellipse(-8, 2, 10, 8, Math.PI/4, 0, Math.PI * 2);
                    ctx.fill();
                    
                    // Draw leaf details
                    ctx.strokeStyle = '#04A57D';
                    ctx.lineWidth = 1;
                    ctx.beginPath();
                    ctx.moveTo(-5, 0);
                    ctx.lineTo(5, -3);
                    ctx.stroke();
                    
                } else {
                    // Regular bird drawing
                    ctx.fillStyle = birdData.color;
                    ctx.beginPath();
                    ctx.ellipse(0, 0, this.width/2, this.height/2, 0, 0, Math.PI * 2);
                    ctx.fill();
                    
                    ctx.fillStyle = birdData.wingColor;
                    ctx.beginPath();
                    ctx.ellipse(-8, 2, 10, 8, Math.PI/4, 0, Math.PI * 2);
                    ctx.fill();
                }
                
                ctx.fillStyle = 'black';
                ctx.beginPath();
                ctx.arc(8, -4, 3, 0, Math.PI * 2);
                ctx.fill();
                
                ctx.fillStyle = this.type === 'eagle' ? '#FF4500' : '#FF8C00';
                ctx.beginPath();
                ctx.moveTo(12, 0);
                ctx.lineTo(20, -3);
                ctx.lineTo(20, 3);
                ctx.closePath();
                ctx.fill();
                
                if (this.type === 'owl') {
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
                    ctx.fillStyle = 'white';
                    ctx.beginPath();
                    ctx.ellipse(0, 5, this.width/3, this.height/3, 0, 0, Math.PI * 2);
                    ctx.fill();
                }
                
                if (this.type === 'flamingo') {
                    ctx.fillStyle = '#FF69B4';
                    ctx.fillRect(-5, 12, 3, 8);
                    ctx.fillRect(2, 12, 3, 8);
                }
                
                if (this.type === 'phoenix') {
                    // Flame effect
                    ctx.fillStyle = '#FF4500';
                    ctx.beginPath();
                    ctx.ellipse(-15, 0, 8, 5, 0, 0, Math.PI * 2);
                    ctx.fill();
                }
                
                if (this.type === 'dragon') {
                    // Dragon wings
                    ctx.fillStyle = '#8A2BE2';
                    ctx.beginPath();
                    ctx.ellipse(-12, -5, 12, 6, -Math.PI/6, 0, Math.PI * 2);
                    ctx.fill();
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
                    if (gameRunning && !isInvincible) gameOver();
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
                
                const inventory = getCurrentUserInventory();
                this.type = inventory.equippedBird;
                selectedBirdType = this.type;
            }
        };

        // Video Transition Functions (tetap sama)
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
            
            videoTimer.textContent = "Loading video...";
            videoDuration = 0;
            videoCurrentTime = 0;
            
            transitionVideo.addEventListener('loadedmetadata', function() {
                videoDuration = transitionVideo.duration;
                updateVideoTimer();
            });
            
            transitionVideo.addEventListener('timeupdate', function() {
                videoCurrentTime = transitionVideo.currentTime;
                updateVideoTimer();
            });
            
            transitionVideo.addEventListener('ended', function() {
                endVideoTransition();
            });
            
            transitionVideo.addEventListener('error', function(e) {
                videoTimer.textContent = "Video error!";
                setTimeout(() => {
                    endVideoTransition();
                }, 2000);
            });
            
            const playPromise = transitionVideo.play();
            
            if (playPromise !== undefined) {
                playPromise.then(() => {
                    isVideoActuallyPlaying = true;
                    stopBackgroundMusic();
                    startVideoTimer();
                }).catch(error => {
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
            
            // Update video missions
            updateDailyMissionProgress('watch_video');
            updateWeeklyMissionProgress('watch_video_weekly');
            
            // Update achievements
            updateAchievementProgress('watch_first_video');
            updateAchievementProgress('watch_10_videos');
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
            if (videoTimerInterval) {
                clearInterval(videoTimerInterval);
                videoTimerInterval = null;
            }
            
            transitionVideo.pause();
            transitionVideo.currentTime = 0;
            
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

        // Music System
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

        // Pipe Class (modified for skills)
        class Pipe {
            constructor() {
                this.width = 60;
                this.x = canvas.width;
                this.gap = 160;
                this.topHeight = Math.random() * (canvas.height - this.gap - 200) + 50;
                this.bottomY = this.topHeight + this.gap;
                this.passed = false;
                this.speed = 2;
                this.destroyed = false;
                
                const inventory = getCurrentUserInventory();
                this.pipeType = inventory.equippedBackground;
            }
            
            draw() {
                if (this.destroyed) return;
                
                const pipeColors = PIPE_TYPES[this.pipeType] || PIPE_TYPES.default;
                
                ctx.fillStyle = pipeColors.topColor;
                ctx.fillRect(this.x, 0, this.width, this.topHeight);
                
                ctx.fillStyle = pipeColors.bottomColor;
                ctx.fillRect(this.x - 5, this.topHeight - 20, this.width + 10, 20);
                
                ctx.fillStyle = pipeColors.topColor;
                ctx.fillRect(this.x, this.bottomY, this.width, canvas.height - this.bottomY);
                
                ctx.fillStyle = pipeColors.bottomColor;
                ctx.fillRect(this.x - 5, this.bottomY, this.width + 10, 20);
            }
            
            update() {
                if (isInTransition) return;
                
                this.x -= this.speed * pipeSpeedMultiplier;
                
                if (this.collidesWith(bird) && !isInvincible) {
                    if (isBoosting) {
                        this.destroyed = true;
                        score += 2;
                        scoreElement.textContent = score;
                    } else {
                        gameOver();
                    }
                }
                
                if (!this.passed && this.x + this.width < bird.x) {
                    score++;
                    scoreElement.textContent = score;
                    this.passed = true;
                    pipesPassedThisGame++;
                    
                    coinsCollectedThisGame += 10;
                    addCoins(10);
                    updateCurrencyDisplay();
                    
                    // Update collect coins mission
                    updateDailyMissionProgress('collect_coins', 10);
                    updateWeeklyMissionProgress('collect_coins_weekly', 10);
                    
                    // Update pipe missions
                    updateDailyMissionProgress('pass_pipes');
                    updateWeeklyMissionProgress('pass_pipes_weekly');
                    
                    // Update stats
                    updateUserStats({ pipesPassed: (currentUser.stats?.pipesPassed || 0) + 1 });
                    
                    // Update achievements
                    updateAchievementProgress('pass_100_pipes');
                    updateAchievementProgress('pass_500_pipes');
                    updateAchievementProgress('pass_1000_pipes');
                    
                    checkForTransition();
                }
            }
            
            collidesWith(bird) {
                if (this.destroyed) return false;
                
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
                return this.x + this.width < 0 || this.destroyed;
            }
        }

        // Cloud & Tree Classes
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
                this.x -= this.speed * pipeSpeedMultiplier;
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
                this.x -= this.speed * pipeSpeedMultiplier;
                if (this.x + this.width < 0) {
                    this.x = canvas.width + 50;
                    this.height = 60 + Math.random() * 40;
                }
            }
        }

        // Game Functions (modified)
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
            const inventory = getCurrentUserInventory();
            const bgClass = BACKGROUND_TYPES[inventory.equippedBackground]?.class || '';
            
            const canvasElement = document.getElementById('gameCanvas');
            canvasElement.className = bgClass;
            
            if (!bgClass) {
                const skyGradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
                skyGradient.addColorStop(0, '#87CEEB');
                skyGradient.addColorStop(1, '#98FB98');
                ctx.fillStyle = skyGradient;
                ctx.fillRect(0, 0, canvas.width, canvas.height);
            }
            
            clouds.forEach(cloud => cloud.draw());
            
            let groundColor, grassColor;
            switch(inventory.equippedBackground) {
                case 'desert':
                    groundColor = '#D2691E';
                    grassColor = '#8B4513';
                    break;
                case 'night':
                    groundColor = '#2F4F4F';
                    grassColor = '#1C2E2E';
                    break;
                case 'forest':
                    groundColor = '#228B22';
                    grassColor = '#006400';
                    break;
                case 'ocean':
                    groundColor = '#1E90FF';
                    grassColor = '#0000FF';
                    break;
                case 'space':
                    groundColor = '#4B0082';
                    grassColor = '#8A2BE2';
                    break;
                default:
                    groundColor = '#32CD32';
                    grassColor = '#228B22';
            }
            
            ctx.fillStyle = groundColor;
            ctx.fillRect(0, canvas.height - 80, canvas.width, 80);
            
            ctx.fillStyle = grassColor;
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
            coinsCollectedThisGame = 0;
            leavesCollectedThisGame = 0;
            pipesPassedThisGame = 0;
            
            pipeSpeedMultiplier = 1;
            isInvincible = false;
            isBoosting = false;
            
            activeSkills = {};
            
            scoreElement.textContent = score;
            startScreen.classList.add('hidden');
            gameOverScreen.classList.add('hidden');
            videoTransition.classList.remove('active');
            bird.reset();
            initBackground();
            
            initSkillsDisplay();
            
            // Update play games mission
            updateDailyMissionProgress('play_games');
            updateWeeklyMissionProgress('play_games_weekly');
            
            // Update stats
            updateUserStats({ gamesPlayed: (currentUser.stats?.gamesPlayed || 0) + 1 });
            
            // Update achievements
            updateAchievementProgress('first_game');
            updateAchievementProgress('play_10_games');
            updateAchievementProgress('play_50_games');
            updateAchievementProgress('play_100_games');
            updateAchievementProgress('play_500_games');
            
            playBackgroundMusic();
            gameLoop();
        }

        function gameOver() {
            gameRunning = false;
            finalScoreElement.textContent = score;
            coinsEarned.textContent = coinsCollectedThisGame;
            
            // Calculate leaves earned based on score
            leavesCollectedThisGame = Math.floor(score / 20); // 1 leaf per 20 score
            leavesEarned.textContent = leavesCollectedThisGame;
            
            if (leavesCollectedThisGame > 0) {
                addLeaves(leavesCollectedThisGame);
                updateCurrencyDisplay();
                
                // Update collect leaves missions
                updateDailyMissionProgress('collect_leaves', leavesCollectedThisGame);
                updateWeeklyMissionProgress('collect_leaves_weekly', leavesCollectedThisGame);
                
                // Update achievements
                updateAchievementProgress('collect_10_leaves');
                updateAchievementProgress('collect_50_leaves');
                updateAchievementProgress('collect_100_leaves');
            }
            
            gameOverScreen.classList.remove('hidden');
            stopBackgroundMusic();
            
            // Update reach score mission
            updateDailyMissionProgress('reach_score', score);
            updateWeeklyMissionProgress('reach_score_weekly', score);
            updateDailyMissionProgress('reach_high_score', score);
            updateWeeklyMissionProgress('reach_high_score_weekly', score);
            
            // Update stats
            updateUserStats({ 
                totalScore: (currentUser.stats?.totalScore || 0) + score,
                bestScore: Math.max(currentUser.stats?.bestScore || 0, score)
            });
            
            // Update achievements based on score
            updateAchievementProgress('score_10');
            updateAchievementProgress('score_50');
            updateAchievementProgress('score_100');
            updateAchievementProgress('score_200');
            updateAchievementProgress('score_500');
            updateAchievementProgress('score_1000');
            
            // Special achievements
            if (score >= 100 && pipesPassedThisGame >= score) {
                updateAchievementProgress('perfect_game');
            }
            
            const currentUser = getCurrentUser();
            if (currentUser && score > 0) {
                submitScoreToFirebase(currentUser.username, score);
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

        // Event Listeners
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
        initBirdSelection();
        initBackground();
        drawBackground();
        bird.draw();
        checkLoginStatus();

        console.log("🎮 LeafPy Bird dengan Sistem Login & Register yang DIPERBAIKI!");
        console.log("🔥 Terhubung dengan Firebase Realtime Database");
        console.log("🏆 Leaderboard bisa dilihat oleh semua pemain");
        console.log("🐦 9 Variasi burung tersedia (1 premium dengan Shiny Leaf)");
        console.log("🛒 Toko dengan item yang bisa dibeli");
        console.log("⚡ 3 Skill khusus - Stackable hingga 10x!");
        console.log("🪙 Dapatkan 10 koin setiap melewati pipa!");
        console.log("🍃 Sistem Shiny Leaf & 15 Misi Harian!");
        console.log("📅 20 Misi Mingguan!");
        console.log("🏅 50 Achievement!");
        console.log("💱 Mesin Convert Shiny Leaf ke Koin (1:1000)!");
    </script>
</body>
</html>
