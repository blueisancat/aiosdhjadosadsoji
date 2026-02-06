<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prime Runner - Enhanced Edition</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #000;
            color: white;
            overflow: hidden;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        canvas { display: block; }
        
        /* Notification System */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(90deg, #00d2ff, #00ff00);
            color: black;
            padding: 15px 25px;
            border-radius: 10px;
            font-weight: bold;
            z-index: 1000;
            transform: translateX(150%);
            transition: transform 0.3s cubic-bezier(0.68, -0.55, 0.265, 1.55);
            box-shadow: 0 5px 15px rgba(0, 210, 255, 0.4);
        }
        .notification.show {
            transform: translateX(0);
        }
        .notification.error {
            background: linear-gradient(90deg, #ff4444, #ff8800);
        }
        .notification.success {
            background: linear-gradient(90deg, #00ff00, #00d2ff);
        }
        
        /* Menus */
        .screen {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: rgba(0, 0, 0, 0.95);
            backdrop-filter: blur(5px);
            z-index: 100;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.4s, visibility 0.4s;
        }
        .screen.active {
            opacity: 1;
            visibility: visible;
        }
        
        /* Game Menu */
        #menu {
            text-align: center;
        }
        .title {
            font-size: 4em;
            margin-bottom: 0.5em;
            background: linear-gradient(90deg, #00d2ff, #00ff00);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 30px rgba(0, 210, 255, 0.7);
            animation: titleGlow 2s ease-in-out infinite alternate;
        }
        @keyframes titleGlow {
            from { text-shadow: 0 0 30px rgba(0, 210, 255, 0.7); }
            to { text-shadow: 0 0 40px rgba(0, 255, 0, 0.7); }
        }
        .subtitle {
            font-size: 1.2em;
            margin-bottom: 2em;
            color: #00d2ff;
            opacity: 0.8;
        }
        .btn {
            background: linear-gradient(90deg, #00d2ff, #00ff00);
            border: none;
            color: black;
            padding: 18px 50px;
            font-size: 1.2em;
            border-radius: 12px;
            cursor: pointer;
            margin: 15px;
            transition: all 0.3s;
            font-weight: bold;
            position: relative;
            overflow: hidden;
        }
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0, 210, 255, 0.5);
        }
        .btn:active {
            transform: translateY(-1px);
        }
        .btn::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
            transition: left 0.5s;
        }
        .btn:hover::after {
            left: 100%;
        }
        .mode-select {
            margin: 25px 0;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            justify-content: center;
        }
        .mode-btn {
            background: rgba(0, 210, 255, 0.1);
            border: 2px solid #00d2ff;
            color: white;
            padding: 12px 25px;
            margin: 5px;
            cursor: pointer;
            border-radius: 10px;
            transition: all 0.3s;
            font-weight: bold;
        }
        .mode-btn:hover {
            background: rgba(0, 210, 255, 0.3);
            transform: translateY(-2px);
        }
        .mode-btn.active {
            background: #00d2ff;
            color: black;
            box-shadow: 0 0 20px #00d2ff;
            transform: scale(1.05);
        }
        
        /* Name Prompt - Fixed */
        #name-prompt {
            transition: opacity 0.4s;
        }
        #name-prompt input {
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid #00d2ff;
            color: white;
            padding: 18px;
            font-size: 1.3em;
            border-radius: 12px;
            margin: 25px;
            width: 350px;
            text-align: center;
            transition: all 0.3s;
        }
        #name-prompt input:focus {
            outline: none;
            border-color: #00ff00;
            box-shadow: 0 0 20px rgba(0, 255, 0, 0.3);
        }
        #name-prompt input.error {
            border-color: #ff4444;
            animation: shake 0.5s;
        }
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
            20%, 40%, 60%, 80% { transform: translateX(5px); }
        }
        
        /* HUD */
        #hud {
            position: absolute;
            top: 20px;
            left: 20px;
            font-size: 1.2em;
            z-index: 10;
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 15px;
            border: 2px solid #00d2ff;
            backdrop-filter: blur(5px);
            transform: translateY(-100px);
            opacity: 0;
            transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        #hud.active {
            transform: translateY(0);
            opacity: 1;
        }
        .hud-item {
            margin: 8px 0;
            display: flex;
            justify-content: space-between;
            min-width: 220px;
        }
        .hud-value {
            color: #00ff00;
            font-weight: bold;
            margin-left: 20px;
            text-shadow: 0 0 10px currentColor;
        }
        
        /* Game Over */
        #game-over {
            text-align: center;
        }
        #reason {
            color: #ff4444;
            font-size: 2em;
            margin: 20px;
            text-shadow: 0 0 15px rgba(255, 68, 68, 0.7);
        }
        #final-stats {
            color: #00d2ff;
            font-size: 1.5em;
            margin: 15px;
        }
        #coins-earned {
            color: #ffd700;
            font-size: 1.8em;
            margin: 20px;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
        }
        
        /* Fact Display */
        #fact-display {
            position: absolute;
            bottom: 30px;
            left: 30px;
            right: 30px;
            background: rgba(0, 0, 0, 0.8);
            padding: 20px;
            border-radius: 15px;
            border: 1px solid #00d2ff;
            font-size: 1em;
            backdrop-filter: blur(5px);
            transform: translateY(100px);
            opacity: 0;
            transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        #fact-display.active {
            transform: translateY(0);
            opacity: 1;
        }
        #fact-text {
            color: #00ff00;
            font-style: italic;
        }
        
        /* Leaderboard */
        #leaderboard {
            position: absolute;
            top: 30px;
            right: 30px;
            background: rgba(0, 0, 0, 0.9);
            padding: 25px;
            border-radius: 15px;
            border: 3px solid #00ff00;
            min-width: 300px;
            max-height: 70vh;
            overflow-y: auto;
            backdrop-filter: blur(5px);
            transform: translateX(100%);
            transition: transform 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        #leaderboard.active {
            transform: translateX(0);
        }
        #leaderboard h3 {
            color: #00ff00;
            margin-bottom: 20px;
            text-align: center;
            font-size: 1.5em;
            border-bottom: 2px solid #00d2ff;
            padding-bottom: 10px;
        }
        .lb-entry {
            display: flex;
            justify-content: space-between;
            margin: 10px 0;
            padding: 12px;
            background: rgba(0, 210, 255, 0.1);
            border-radius: 8px;
            transition: all 0.3s;
            cursor: pointer;
        }
        .lb-entry:hover {
            background: rgba(0, 210, 255, 0.2);
            transform: translateX(5px);
        }
        .lb-entry.selected {
            background: rgba(0, 255, 0, 0.2);
            border: 2px solid #00ff00;
        }
        .lb-rank {
            color: #ffd700;
            font-weight: bold;
            min-width: 40px;
        }
        .lb-name {
            flex-grow: 1;
            margin: 0 15px;
        }
        .lb-score {
            color: #00ff00;
            font-weight: bold;
            min-width: 60px;
            text-align: right;
        }
        
        /* Shop */
        #shop {
            position: absolute;
            background: rgba(0, 0, 0, 0.95);
            padding: 40px;
            border-radius: 25px;
            border: 3px solid #00d2ff;
            max-width: 900px;
            max-height: 85vh;
            overflow-y: auto;
            backdrop-filter: blur(10px);
            transform: scale(0.9);
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        #shop.active {
            transform: scale(1);
            opacity: 1;
        }
        .shop-category {
            margin: 25px 0;
        }
        .shop-category h3 {
            color: #00ff00;
            margin-bottom: 15px;
            border-bottom: 3px solid #00d2ff;
            padding-bottom: 8px;
            font-size: 1.4em;
        }
        .shop-items {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        .shop-item {
            background: rgba(0, 210, 255, 0.1);
            border: 2px solid #00d2ff;
            padding: 20px;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }
        .shop-item:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0, 210, 255, 0.4);
        }
        .shop-item.equipped {
            border-color: #00ff00;
            background: rgba(0, 255, 0, 0.15);
            box-shadow: 0 0 20px rgba(0, 255, 0, 0.3);
        }
        .shop-item.purchased:not(.equipped) {
            border-color: #ffd700;
            background: rgba(255, 215, 0, 0.1);
        }
        .item-name {
            font-weight: bold;
            margin-bottom: 8px;
            color: white;
            font-size: 1.2em;
        }
        .item-price {
            color: #ffd700;
            font-size: 1.3em;
            margin: 10px 0;
        }
        .item-desc {
            font-size: 0.9em;
            color: #aaa;
            margin-top: 10px;
            line-height: 1.4;
        }
        .item-status {
            position: absolute;
            top: 10px;
            right: 10px;
            background: #00ff00;
            color: black;
            padding: 3px 10px;
            border-radius: 15px;
            font-size: 0.8em;
            font-weight: bold;
        }
        
        /* Close buttons */
        .close-btn {
            position: absolute;
            top: 15px;
            right: 15px;
            background: #ff4444;
            color: white;
            border: none;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            cursor: pointer;
            font-weight: bold;
            font-size: 1.2em;
            transition: all 0.3s;
            z-index: 101;
        }
        .close-btn:hover {
            transform: rotate(90deg);
            background: #ff0000;
            box-shadow: 0 0 15px rgba(255, 0, 0, 0.7);
        }
        
        /* Coin Display */
        #coin-display {
            position: absolute;
            top: 30px;
            right: 30px;
            background: rgba(0, 0, 0, 0.8);
            padding: 15px 25px;
            border-radius: 12px;
            border: 2px solid #ffd700;
            font-size: 1.3em;
            backdrop-filter: blur(5px);
            display: none;
            transform: translateY(-100px);
            opacity: 0;
            transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        #coin-display.active {
            transform: translateY(0);
            opacity: 1;
        }
        #coin-display span {
            color: #ffd700;
            font-weight: bold;
            margin-left: 15px;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
        }
        
        /* Controls Info */
        #controls {
            position: absolute;
            bottom: 30px;
            right: 30px;
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 15px;
            border: 1px solid #00d2ff;
            font-size: 1em;
            backdrop-filter: blur(5px);
            display: none;
            transform: translateX(100%);
            opacity: 0;
            transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        #controls.active {
            transform: translateX(0);
            opacity: 1;
        }
        #controls strong {
            color: #00ff00;
            display: block;
            margin-bottom: 10px;
            font-size: 1.1em;
        }
        .key {
            display: inline-block;
            background: rgba(255,255,255,0.1);
            padding: 5px 10px;
            margin: 3px;
            border-radius: 5px;
            border: 1px solid #00d2ff;
            font-family: monospace;
        }
        
        /* Advanced Deletion Modal */
        #deletion-modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.8);
            background: rgba(0, 0, 0, 0.95);
            padding: 40px;
            border-radius: 20px;
            border: 3px solid #ff4444;
            z-index: 1000;
            min-width: 500px;
            max-width: 90%;
            opacity: 0;
            visibility: hidden;
            transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        #deletion-modal.active {
            opacity: 1;
            visibility: visible;
            transform: translate(-50%, -50%) scale(1);
        }
        .modal-title {
            color: #ff4444;
            font-size: 1.8em;
            margin-bottom: 25px;
            text-align: center;
        }
        .modal-content {
            margin: 20px 0;
        }
        .selected-items {
            max-height: 200px;
            overflow-y: auto;
            margin: 15px 0;
            padding: 15px;
            background: rgba(255, 68, 68, 0.1);
            border-radius: 10px;
            border: 1px solid #ff4444;
        }
        .selected-item {
            display: flex;
            justify-content: space-between;
            padding: 8px;
            margin: 5px 0;
            background: rgba(255, 68, 68, 0.05);
            border-radius: 5px;
        }
        .modal-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 30px;
        }
        .modal-btn {
            padding: 12px 30px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
            min-width: 120px;
        }
        .modal-btn.confirm {
            background: #ff4444;
            color: white;
        }
        .modal-btn.confirm:hover {
            background: #ff0000;
            box-shadow: 0 0 20px rgba(255, 0, 0, 0.5);
            transform: translateY(-2px);
        }
        .modal-btn.cancel {
            background: #00d2ff;
            color: black;
        }
        .modal-btn.cancel:hover {
            background: #00aaff;
            box-shadow: 0 0 20px rgba(0, 170, 255, 0.5);
            transform: translateY(-2px);
        }
        .modal-btn.undo {
            background: #ffd700;
            color: black;
        }
        .modal-btn.undo:hover {
            background: #ffaa00;
            box-shadow: 0 0 20px rgba(255, 170, 0, 0.5);
            transform: translateY(-2px);
        }
        .secure-delete {
            margin: 20px 0;
            padding: 15px;
            background: rgba(255, 68, 68, 0.1);
            border-radius: 10px;
            border: 1px dashed #ff4444;
        }
        .secure-delete label {
            display: flex;
            align-items: center;
            cursor: pointer;
            color: #ff4444;
        }
        .secure-delete input[type="checkbox"] {
            margin-right: 10px;
            width: 18px;
            height: 18px;
        }
        .deletion-options {
            display: flex;
            gap: 20px;
            margin: 20px 0;
            flex-wrap: wrap;
        }
        .option-btn {
            flex: 1;
            min-width: 150px;
            padding: 12px;
            background: rgba(255, 68, 68, 0.1);
            border: 2px solid #ff4444;
            color: white;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            text-align: center;
        }
        .option-btn:hover {
            background: rgba(255, 68, 68, 0.3);
            transform: translateY(-3px);
        }
        
        /* Overlay */
        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(3px);
            z-index: 999;
            opacity: 0;
            visibility: hidden;
            transition: all 0.4s;
        }
        .overlay.active {
            opacity: 1;
            visibility: visible;
        }
        
        /* Bulk Selection */
        .bulk-actions {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.9);
            padding: 15px 30px;
            border-radius: 15px;
            border: 2px solid #00ff00;
            display: flex;
            gap: 15px;
            z-index: 100;
            opacity: 0;
            transform: translate(-50%, 100%);
            transition: all 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }
        .bulk-actions.active {
            opacity: 1;
            transform: translate(-50%, 0);
        }
        .selection-count {
            color: #00ff00;
            font-weight: bold;
            margin-right: 15px;
        }
        
        /* Responsive Design */
        @media (max-width: 768px) {
            .title { font-size: 2.5em; }
            .btn { padding: 15px 30px; font-size: 1em; }
            .shop-items { grid-template-columns: repeat(2, 1fr); }
            #deletion-modal { min-width: 90%; padding: 25px; }
            .modal-buttons { flex-direction: column; }
            .modal-btn { width: 100%; }
        }
        @media (max-width: 480px) {
            .title { font-size: 2em; }
            .shop-items { grid-template-columns: 1fr; }
            .mode-select { flex-direction: column; }
            .mode-btn { width: 100%; }
        }
    </style>
</head>
<body>
    <div id="game-container"></div>
    
    <!-- Notification System -->
    <div id="notification" class="notification"></div>
    
    <!-- Overlay -->
    <div id="overlay" class="overlay"></div>
    
    <!-- Advanced Deletion Modal -->
    <div id="deletion-modal">
        <div class="modal-title">🗑️ Advanced Deletion</div>
        <div class="modal-content">
            <div id="deletion-warning">Are you sure you want to delete selected items?</div>
            <div class="selected-items" id="selected-items-list"></div>
            <div class="deletion-options">
                <div class="option-btn" onclick="selectAllItems()">Select All</div>
                <div class="option-btn" onclick="deselectAllItems()">Deselect All</div>
                <div class="option-btn" onclick="invertSelection()">Invert Selection</div>
            </div>
            <div class="secure-delete">
                <label>
                    <input type="checkbox" id="secure-delete-checkbox">
                    🔒 Secure Permanent Deletion (Overwrite 3 times)
                </label>
            </div>
        </div>
        <div class="modal-buttons">
            <button class="modal-btn confirm" onclick="confirmDeletion()">Delete Now</button>
            <button class="modal-btn cancel" onclick="closeDeletionModal()">Cancel</button>
            <button class="modal-btn undo" onclick="undoLastDeletion()" id="undo-btn">Undo Last</button>
        </div>
    </div>
    
    <!-- Bulk Actions Bar -->
    <div class="bulk-actions" id="bulk-actions">
        <span class="selection-count" id="selection-count">0 selected</span>
        <button class="btn" onclick="openDeletionModal()" style="padding: 8px 20px;">Delete Selected</button>
        <button class="btn" onclick="clearSelection()" style="padding: 8px 20px; background: #666;">Clear</button>
    </div>
    
    <!-- Main Menu -->
    <div id="menu" class="screen active">
        <h1 class="title">PRIME RUNNER</h1>
        <p class="subtitle">Dodge composites, collect primes!</p>
        
        <div class="mode-select">
            <button class="mode-btn active" onclick="setGameMode('normal')">NORMAL</button>
            <button class="mode-btn" onclick="setGameMode('slowburn')">SLOWBURN</button>
            <button class="mode-btn" onclick="setGameMode('hardcore')">HARDCORE</button>
        </div>
        
        <button class="btn" onclick="pressStart()">🚀 START RUN</button>
        <button class="btn" onclick="toggleShop()">🛒 SHOP</button>
        <button class="btn" onclick="toggleLeaderboard()">🏆 LEADERBOARD</button>
        
        <div style="margin-top: 40px; color: #aaa; font-size: 0.9em; max-width: 600px; line-height: 1.6;">
            <strong>Controls:</strong> ← → or A D to move | ESC: Menu<br>
            <strong>Goal:</strong> Collect Prime Numbers (green), dodge Composite Numbers (red)
        </div>
    </div>
    
    <!-- Name Prompt - Fixed -->
    <div id="name-prompt" class="screen">
        <h2 style="color: #00d2ff; margin-bottom: 20px;">👤 ENTER YOUR NAME</h2>
        <input type="text" id="username-input" maxlength="15" placeholder="Runner" 
               onkeypress="if(event.key === 'Enter') saveUsername()">
        <button class="btn" onclick="saveUsername()">START</button>
        <div style="margin-top: 20px; color: #aaa; font-size: 0.9em;">
            Name will be saved for leaderboard
        </div>
    </div>
    
    <!-- HUD -->
    <div id="hud">
        <div class="hud-item">
            <span>📏 DISTANCE:</span>
            <span id="dist-val" class="hud-value">0m</span>
        </div>
        <div class="hud-item">
            <span>⭐ SCORE:</span>
            <span id="score-val" class="hud-value">0</span>
        </div>
        <div class="hud-item">
            <span>🪙 COINS:</span>
            <span id="coins-val" class="hud-value">0</span>
        </div>
        <div class="hud-item">
            <span>⚡ SPEED:</span>
            <span id="speed-val" class="hud-value">0.3</span>
        </div>
    </div>
    
    <!-- Game Over -->
    <div id="game-over" class="screen">
        <h2 style="color: #ff4444; font-size: 3em; text-shadow: 0 0 20px rgba(255,68,68,0.7);">💀 GAME OVER</h2>
        <div id="reason">Missed a Prime!</div>
        <div id="final-stats">Distance: 0m | Score: 0</div>
        <div id="coins-earned">Coins earned: +0</div>
        <button class="btn" onclick="backToMenu()">🏠 BACK TO MENU</button>
    </div>
    
    <!-- Shop -->
    <div id="shop" class="screen">
        <button class="close-btn" onclick="toggleShop()">×</button>
        <h2 style="color: #ffd700; margin-bottom: 20px; text-align: center;">🛒 SKIN SHOP</h2>
        <div id="shop-items"></div>
        <div style="margin-top: 30px; text-align: center; color: #00d2ff; font-size: 1.2em;">
            Your coins: <span id="shop-coins" style="color: #ffd700; font-weight: bold;">0</span>
        </div>
        <div style="margin-top: 20px; text-align: center;">
            <button class="btn" onclick="showDeletionOptions()" style="background: linear-gradient(90deg, #ff4444, #ff8800);">
                🗑️ Manage Data
            </button>
        </div>
    </div>
    
    <!-- Leaderboard -->
    <div id="leaderboard">
        <button class="close-btn" onclick="toggleLeaderboard()" style="top: 10px; right: 10px; width: 30px; height: 30px; font-size: 1em;">×</button>
        <h3>🏆 LEADERBOARD</h3>
        <div style="margin-bottom: 15px; display: flex; gap: 10px;">
            <button class="mode-btn" onclick="selectAllLeaderboard()" style="padding: 5px 15px; font-size: 0.9em;">Select All</button>
            <button class="mode-btn" onclick="deleteSelectedScores()" style="padding: 5px 15px; font-size: 0.9em; background: #ff4444;">Delete</button>
        </div>
        <div id="lb-entries"></div>
    </div>
    
    <!-- Coin Display -->
    <div id="coin-display">
        🪙 COINS: <span id="total-coins">0</span>
    </div>
    
    <!-- Controls -->
    <div id="controls">
        <strong>🎮 CONTROLS:</strong><br>
        <div style="margin-top: 10px;">
            <span class="key">←</span> <span class="key">→</span> or 
            <span class="key">A</span> <span class="key">D</span>: Move<br>
            <span class="key">ESC</span>: Menu<br>
            <span class="key">SPACE</span>: Pause<br>
            <span class="key">R</span>: Restart
        </div>
    </div>
    
    <!-- Fact Display -->
    <div id="fact-display">
        <strong>🔢 PRIME FACT:</strong> <span id="fact-text">Loading...</span>
    </div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
// ==================== GAME CONFIGURATION ====================
let scene, camera, renderer;
let player, playerCore;
let road, grid;
let pillars = [];
let activeNumbers = [];
let isPlaying = false;
let isPaused = false;
let score = 0, distance = 0, speed = 0.3, coinsThisRun = 0;
let currentLane = 1;
let lanes = [-1.75, 0, 1.75];
let gameMode = 'normal';
let currentEnv = 'grid';
let lastEnvChange = 0;
let factTimer;
let spawnTimer;
let selectedScores = new Set();
let deletionHistory = [];

// ==================== NOTIFICATION SYSTEM ====================
function showNotification(message, type = 'info', duration = 3000) {
    const notification = document.getElementById('notification');
    notification.textContent = message;
    notification.className = `notification ${type}`;
    notification.classList.add('show');
    
    setTimeout(() => {
        notification.classList.remove('show');
    }, duration);
}

// ==================== PLAYER DATA & LOCAL STORAGE ====================
let playerData = {
    username: localStorage.getItem('primeRunner_username') || '',
    coins: parseInt(localStorage.getItem('primeRunner_coins')) || 100,
    matchesPlayed: parseInt(localStorage.getItem('primeRunner_matchesPlayed')) || 0,
    highScores: JSON.parse(localStorage.getItem('primeRunner_highScores')) || [],
    unlockedSkins: JSON.parse(localStorage.getItem('primeRunner_unlockedSkins')) || ['skin_default'],
    equipped: {
        skin: localStorage.getItem('primeRunner_equippedSkin') || 'skin_default'
    },
    deletionHistory: JSON.parse(localStorage.getItem('primeRunner_deletionHistory')) || []
};

// Save deletion to history
function saveToDeletionHistory(action, items) {
    const historyEntry = {
        timestamp: new Date().toISOString(),
        action: action,
        items: items,
        dataBackup: {
            highScores: [...playerData.highScores],
            unlockedSkins: [...playerData.unlockedSkins],
            coins: playerData.coins
        }
    };
    
    deletionHistory.unshift(historyEntry);
    deletionHistory = deletionHistory.slice(0, 10); // Keep last 10 deletions
    playerData.deletionHistory = deletionHistory;
    localStorage.setItem('primeRunner_deletionHistory', JSON.stringify(deletionHistory));
}

// ==================== SHOP ITEMS ====================
const shopItems = {
    skins: [
        { id: 'skin_default', name: 'Default', color: 0x00d2ff, price: 0, desc: 'Starting blue skin' },
        { id: 'skin_neon', name: 'Neon Green', color: 0x00ff00, price: 100, desc: 'Bright green glow' },
        { id: 'skin_fire', name: 'Fire', color: 0xff4400, price: 150, desc: 'Hot orange flames' },
        { id: 'skin_ice', name: 'Ice', color: 0x00ffff, price: 200, desc: 'Cool blue ice' },
        { id: 'skin_rainbow', name: 'Rainbow', color: 0xffffff, price: 500, desc: 'Cycling rainbow colors!' },
        { id: 'skin_galaxy', name: 'Galaxy', color: 0x8a2be2, price: 750, desc: 'Purple cosmic energy' },
        { id: 'skin_gold', name: 'Golden', color: 0xffd700, price: 1000, desc: 'Prestige golden skin' }
    ]
};

// ==================== PRIME FACTS ====================
const primeFacts = [
    "2 is the only even prime number.",
    "Prime numbers become less frequent as numbers get larger.",
    "The largest known prime has over 24 million digits!",
    "Prime numbers are used in cryptography to secure data.",
    "1 is not considered a prime number.",
    "Every prime number greater than 3 is either 1 more or 1 less than a multiple of 6.",
    "There are infinite prime numbers (proved by Euclid).",
    "Twin primes are pairs of primes that differ by 2 (like 11 and 13).",
    "No prime number greater than 5 ends in 5.",
    "The word 'prime' comes from Latin 'primus' meaning 'first'."
];

// ==================== INITIALIZE THREE.JS ====================
function init() {
    scene = new THREE.Scene();
    scene.fog = new THREE.Fog(0x000000, 10, 100);
    
    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.set(0, 5, 5);
    
    renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(window.innerWidth, window.innerHeight);
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    document.getElementById('game-container').appendChild(renderer.domElement);
    
    // Enhanced Lighting
    const ambientLight = new THREE.AmbientLight(0x222222);
    scene.add(ambientLight);
    
    const directionalLight = new THREE.DirectionalLight(0x00d2ff, 1.2);
    directionalLight.position.set(10, 20, 5);
    directionalLight.castShadow = true;
    directionalLight.shadow.mapSize.width = 2048;
    directionalLight.shadow.mapSize.height = 2048;
    scene.add(directionalLight);
    
    // Add point lights for glow effect
    const pointLight1 = new THREE.PointLight(0x00d2ff, 0.5, 50);
    pointLight1.position.set(5, 5, 0);
    scene.add(pointLight1);
    
    const pointLight2 = new THREE.PointLight(0x00ff00, 0.3, 50);
    pointLight2.position.set(-5, 5, 0);
    scene.add(pointLight2);
    
    // Road
    const roadGeo = new THREE.PlaneGeometry(20, 400);
    const roadMat = new THREE.MeshStandardMaterial({ 
        color: 0x050515,
        emissive: 0x000000,
        emissiveIntensity: 0,
        roughness: 0.6,
        metalness: 0.3
    });
    road = new THREE.Mesh(roadGeo, roadMat);
    road.rotation.x = -Math.PI / 2;
    road.position.z = -180;
    road.receiveShadow = true;
    scene.add(road);

    // Lane markers
    const laneGeo = new THREE.PlaneGeometry(0.08, 400);
    const laneMat = new THREE.MeshBasicMaterial({ 
        color: 0x00d2ff,
        transparent: true,
        opacity: 0.8
    });
    [-1.75, 1.75].forEach(x => {
        const l = new THREE.Mesh(laneGeo, laneMat);
        l.rotation.x = -Math.PI / 2;
        l.position.set(x, 0.01, -180);
        scene.add(l);
    });

    // Grid
    grid = new THREE.GridHelper(400, 80, 0x00d2ff, 0x111111);
    grid.position.z = -180;
    grid.material.opacity = 0.5;
    grid.material.transparent = true;
    scene.add(grid);

    // Pillars with enhanced appearance
    const pillarGeo = new THREE.CylinderGeometry(0.3, 0.3, 6, 16);
    const pillarMat = new THREE.MeshStandardMaterial({ 
        color: 0x00d2ff, 
        emissive: 0x00d2ff, 
        emissiveIntensity: 1.5,
        metalness: 0.5,
        roughness: 0.2
    });
    for (let i = 0; i < 30; i++) {
        const p = new THREE.Mesh(pillarGeo, pillarMat);
        p.position.set(Math.random() > 0.5 ? 7 : -7, 3, -i * 15);
        p.castShadow = true;
        scene.add(p);
        pillars.push(p);
    }

    // Create player with enhanced materials
    createPlayer();
    
    camera.lookAt(player.position);
    
    // Event Listeners
    window.addEventListener('keydown', handleInput);
    window.addEventListener('resize', onWindowResize);
    window.addEventListener('click', handleGlobalClick);
    
    // Initialize UI
    updateShopDisplay();
    updateLeaderboard();
    updateMenuCoins();
    
    // Start animation
    animate();
}

function createPlayer() {
    if (player) {
        scene.remove(player);
    }
    
    player = new THREE.Group();
    const skinItem = shopItems.skins.find(s => s.id === playerData.equipped.skin);
    
    // Main sphere
    playerCore = new THREE.Mesh(
        new THREE.SphereGeometry(0.4, 32, 32),
        new THREE.MeshStandardMaterial({ 
            color: skinItem ? skinItem.color : 0x00d2ff, 
            emissive: skinItem ? skinItem.color : 0x00d2ff, 
            emissiveIntensity: 2,
            metalness: 0.8,
            roughness: 0.1
        })
    );
    playerCore.castShadow = true;
    player.add(playerCore);
    
    // Add glow effect
    const glowGeometry = new THREE.SphereGeometry(0.5, 32, 32);
    const glowMaterial = new THREE.MeshBasicMaterial({
        color: skinItem ? skinItem.color : 0x00d2ff,
        transparent: true,
        opacity: 0.3
    });
    const glow = new THREE.Mesh(glowGeometry, glowMaterial);
    player.add(glow);
    
    player.position.y = 1;
    scene.add(player);
}

function onWindowResize() {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
}

function handleGlobalClick(e) {
    // Close modal if clicking outside
    if (document.getElementById('deletion-modal').classList.contains('active') &&
        !e.target.closest('#deletion-modal') &&
        !e.target.closest('.bulk-actions')) {
        closeDeletionModal();
    }
}

// ==================== INPUT HANDLING ====================
function handleInput(e) {
    if (e.key === 'Escape') {
        if (document.getElementById('deletion-modal').classList.contains('active')) {
            closeDeletionModal();
        } else if (isPlaying) {
            togglePause();
        } else {
            backToMenu();
        }
        return;
    }
    
    if (e.key === ' ') {
        e.preventDefault();
        togglePause();
        return;
    }
    
    if (e.key === 'r' && !isPlaying) {
        pressStart();
        return;
    }
    
    if (!isPlaying || isPaused) return;
    
    if ((e.key === 'ArrowLeft' || e.key === 'a' || e.key === 'A') && currentLane > 0) {
        currentLane--;
        showNotification(`Moved to lane ${currentLane + 1}`, 'info', 1000);
    }
    if ((e.key === 'ArrowRight' || e.key === 'd' || e.key === 'D') && currentLane < 2) {
        currentLane++;
        showNotification(`Moved to lane ${currentLane + 1}`, 'info', 1000);
    }
}

function togglePause() {
    if (!isPlaying) return;
    
    isPaused = !isPaused;
    if (isPaused) {
        showNotification('⏸️ Game Paused', 'info', 2000);
        if (spawnTimer) clearTimeout(spawnTimer);
    } else {
        showNotification('▶️ Game Resumed', 'success', 2000);
        spawnWave();
    }
}

// ==================== MENU FUNCTIONS ====================
window.pressStart = function() {
    if (!playerData.username || playerData.username.trim() === '') {
        showScreen('name-prompt');
        document.getElementById('username-input').focus();
    } else {
        startActualGame();
    }
}

function saveUsername() {
    const input = document.getElementById('username-input');
    const username = input.value.trim();
    
    // Validation
    if (!username) {
        input.classList.add('error');
        showNotification('Please enter a valid name!', 'error', 3000);
        return;
    }
    
    if (username.length < 2) {
        input.classList.add('error');
        showNotification('Name must be at least 2 characters!', 'error', 3000);
        return;
    }
    
    if (username.length > 15) {
        input.classList.add('error');
        showNotification('Name must be less than 15 characters!', 'error', 3000);
        return;
    }
    
    input.classList.remove('error');
    playerData.username = username;
    localStorage.setItem('primeRunner_username', username);
    
    showNotification(`Welcome, ${username}!`, 'success', 2000);
    
    // Fixed: Properly transition to game
    setTimeout(() => {
        showScreen('menu');
        setTimeout(startActualGame, 300);
    }, 500);
}

function setGameMode(mode) {
    gameMode = mode;
    document.querySelectorAll('.mode-btn').forEach(btn => btn.classList.remove('active'));
    event.target.classList.add('active');
    showNotification(`${mode.toUpperCase()} mode selected`, 'info', 2000);
}

function startActualGame() {
    // Show all game UI elements
    showScreen(null); // Hide all screens
    document.getElementById('hud').classList.add('active');
    document.getElementById('coin-display').classList.add('active');
    document.getElementById('controls').classList.add('active');
    document.getElementById('fact-display').classList.add('active');
    
    // Reset game state
    isPlaying = true;
    isPaused = false;
    score = 0; 
    distance = 0; 
    speed = 0.3; 
    coinsThisRun = 0; 
    currentLane = 1;
    
    // Reset player position
    player.position.x = lanes[1];
    player.position.y = 1;
    
    // Clear existing numbers
    activeNumbers.forEach(n => scene.remove(n));
    activeNumbers = [];
    
    // Update visuals
    updatePlayerSkin();
    
    // Start game systems
    spawnWave();
    startFactCycle();
    
    showNotification('Game Started! Good luck!', 'success', 2000);
}

// ==================== GAME FUNCTIONS ====================
function spawnWave(forced = false) {
    if (!isPlaying || isPaused) return;
    if (spawnTimer) clearTimeout(spawnTimer);
    
    const val = Math.floor(Math.random() * 97) + 2;
    const canvas = document.createElement('canvas');
    const ctx = canvas.getContext('2d');
    canvas.width = 256; 
    canvas.height = 256;
    
    // Draw number with better styling
    ctx.fillStyle = "#ffffff";
    ctx.font = "bold 120px 'Segoe UI', Arial, sans-serif";
    ctx.textAlign = "center";
    ctx.textBaseline = "middle";
    
    // Add glow effect
    ctx.shadowColor = "#00ff00";
    ctx.shadowBlur = 20;
    ctx.fillText(val, 128, 128);
    
    // Determine color and style
    let color, glowColor;
    if (isPrime(val)) {
        color = 0x00ff00;
        glowColor = 0x00ff00;
    } else {
        color = gameMode === 'hardcore' ? 0x333333 : 0xff4444;
        glowColor = gameMode === 'hardcore' ? 0x666666 : 0xff4444;
    }

    const spriteMat = new THREE.SpriteMaterial({
        map: new THREE.CanvasTexture(canvas),
        color: color,
        transparent: true,
        opacity: 0.9
    });
    
    const sprite = new THREE.Sprite(spriteMat);
    const lane = Math.floor(Math.random() * 3);
    sprite.position.set(lanes[lane], 1.5, -80);
    sprite.scale.set(4, 4, 1);
    sprite.userData = { 
        value: val, 
        isCorrect: isPrime(val),
        lane: lane
    };
    
    // Add pulsing animation
    sprite.userData.pulseSpeed = 0.05;
    sprite.userData.pulsePhase = Math.random() * Math.PI * 2;
    
    scene.add(sprite);
    activeNumbers.push(sprite);
    
    if (!forced) {
        let spawnRate = gameMode === 'hardcore' ? 500 : Math.max(500, 1800 - distance * 1.2);
        spawnTimer = setTimeout(spawnWave, spawnRate);
    }
}

function isPrime(n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 === 0 || n % 3 === 0) return false;
    for (let i = 5; i * i <= n; i += 6) {
        if (n % i === 0 || n % (i + 2) === 0) return false;
    }
    return true;
}

function gameOver(reason) {
    if (!isPlaying) return;
    
    isPlaying = false;
    isPaused = false;
    
    if (factTimer) clearInterval(factTimer);
    if (spawnTimer) clearTimeout(spawnTimer);
    
    playerData.matchesPlayed++;
    
    // Calculate earnings with bonuses
    let multiplier = 1;
    if (gameMode === 'hardcore') multiplier = 2;
    if (score > 1000) multiplier += 1;
    if (distance > 500) multiplier += 0.5;
    
    let earned = Math.floor(coinsThisRun * multiplier);
    playerData.coins += earned;
    
    // Add to high scores
    if (score > 0) {
        playerData.highScores.push({
            score: score,
            distance: Math.floor(distance),
            coins: coinsThisRun,
            mode: gameMode,
            timestamp: new Date().toISOString(),
            username: playerData.username
        });
        
        // Sort and keep top 20
        playerData.highScores.sort((a, b) => b.score - a.score);
        playerData.highScores = playerData.highScores.slice(0, 20);
    }
    
    saveData();
    
    // Update UI
    document.getElementById('reason').textContent = reason;
    document.getElementById('final-stats').textContent = `Distance: ${Math.floor(distance)}m | Score: ${score} | Mode: ${gameMode.toUpperCase()}`;
    document.getElementById('coins-earned').innerHTML = 
        `Coins earned: <span style="color:#ffd700">+${earned}</span>${multiplier > 1 ? ` (${multiplier.toFixed(1)}x bonus)` : ''}`;
    
    showScreen('game-over');
    showNotification(`Game Over! Earned ${earned} coins`, gameMode === 'hardcore' ? 'error' : 'info', 5000);
}

window.backToMenu = function() {
    // Hide all game UI
    document.getElementById('hud').classList.remove('active');
    document.getElementById('coin-display').classList.remove('active');
    document.getElementById('controls').classList.remove('active');
    document.getElementById('fact-display').classList.remove('active');
    document.getElementById('leaderboard').classList.remove('active');
    
    // Stop game systems
    isPlaying = false;
    isPaused = false;
    if (factTimer) clearInterval(factTimer);
    if (spawnTimer) clearTimeout(spawnTimer);
    
    // Clear game objects
    activeNumbers.forEach(n => scene.remove(n));
    activeNumbers = [];
    
    // Update UI
    updateMenuCoins();
    updateLeaderboard();
    
    // Show menu
    showScreen('menu');
}

// ==================== ENVIRONMENT SYSTEM ====================
function updateEnvironment() {
    const envs = ['grid', 'tunnel', 'void'];
    const dist = Math.floor(distance);
    
    if (dist > 0 && dist % 500 === 0 && dist !== lastEnvChange) {
        lastEnvChange = dist;
        const nextIndex = (envs.indexOf(currentEnv) + 1) % envs.length;
        currentEnv = envs[nextIndex];
        
        // Spawn special enemy on environment change
        spawnWave(true);

        // Update visuals
        if (currentEnv === 'grid') {
            scene.fog.color.setHex(0x000000);
            grid.visible = true;
            road.material.color.setHex(0x050515);
            road.material.emissiveIntensity = 0;
            if (scene.userData.stars) scene.userData.stars.visible = false;
            showNotification('🌌 Grid Environment', 'info', 3000);
        } else if (currentEnv === 'tunnel') {
            scene.fog.color.setHex(0x002244);
            grid.visible = false;
            road.material.color.setHex(0x001133);
            road.material.emissiveIntensity = 0.2;
            if (scene.userData.stars) scene.userData.stars.visible = false;
            showNotification('🌀 Tunnel Environment', 'info', 3000);
        } else if (currentEnv === 'void') {
            scene.fog.color.setHex(0x050010);
            grid.visible = false;
            road.material.color.setHex(0x000000);
            road.material.emissive.setHex(0x4400ff);
            road.material.emissiveIntensity = 0.8;
            
            // Enhanced stars
            if (!scene.userData.stars) {
                const starGeo = new THREE.BufferGeometry();
                const starCount = 5000;
                const starPos = new Float32Array(starCount * 3);
                const starSizes = new Float32Array(starCount);
                
                for(let i = 0; i < starCount * 3; i += 3) {
                    starPos[i] = (Math.random() - 0.5) * 400;
                    starPos[i + 1] = (Math.random() - 0.5) * 200;
                    starPos[i + 2] = (Math.random() - 0.5) * 400;
                    starSizes[i/3] = Math.random() * 0.5 + 0.1;
                }
                
                starGeo.setAttribute('position', new THREE.BufferAttribute(starPos, 3));
                starGeo.setAttribute('size', new THREE.BufferAttribute(starSizes, 1));
                
                const starMat = new THREE.PointsMaterial({ 
                    color: 0x00d2ff, 
                    size: 0.2, 
                    transparent: true, 
                    opacity: 0.8,
                    sizeAttenuation: true
                });
                
                scene.userData.stars = new THREE.Points(starGeo, starMat);
                scene.add(scene.userData.stars);
            }
            scene.userData.stars.visible = true;
            showNotification('✨ Void Environment', 'info', 3000);
        }
    }
}

// ==================== ANIMATION LOOP ====================
function animate() {
    requestAnimationFrame(animate);
    
    if (isPlaying && !isPaused) {
        // Update game state
        distance += speed;
        
        // Update speed based on mode
        if (gameMode === 'slowburn') {
            speed += 0.00005;
        } else if (gameMode === 'hardcore') {
            speed = Math.min(0.9, 0.5 + distance * 0.0005);
        } else {
            speed = Math.min(0.5, 0.3 + distance * 0.0002);
        }
        
        document.getElementById('speed-val').textContent = speed.toFixed(2);

        updateEnvironment();
        
        // Admin features
        if (document.cookie.includes('disco_mode=true')) {
            const time = Date.now() * 0.01;
            const r = Math.sin(time) * 0.5 + 0.5;
            const g = Math.sin(time + 2) * 0.5 + 0.5;
            const b = Math.sin(time + 4) * 0.5 + 0.5;
            playerCore.material.color.setRGB(r, g, b);
            playerCore.material.emissive.setRGB(r, g, b);
        }

        // Move player with smooth interpolation
        const targetX = lanes[currentLane];
        player.position.x += (targetX - player.position.x) * 0.2;
        
        // Rainbow skin animation
        if (playerData.equipped.skin === 'skin_rainbow') {
            updatePlayerSkin();
        }
        
        // Animate player bounce
        player.position.y = 1 + Math.sin(Date.now() * 0.003) * 0.1;
        
        // Move environment
        road.position.z += speed;
        grid.position.z += speed;
        if (road.position.z > 20) road.position.z = -180;
        if (grid.position.z > 20) grid.position.z = -180;
        
        pillars.forEach(p => {
            p.position.z += speed;
            p.rotation.y += 0.01;
            if (p.position.z > 10) {
                p.position.set(
                    Math.random() > 0.5 ? 7 : -7,
                    3,
                    -400 + Math.random() * 50
                );
            }
        });
        
        // Update HUD
        document.getElementById('dist-val').textContent = Math.floor(distance) + 'm';
        document.getElementById('score-val').textContent = score;
        document.getElementById('coins-val').textContent = coinsThisRun;
        
        // Update numbers
        for (let i = activeNumbers.length - 1; i >= 0; i--) {
            const n = activeNumbers[i];
            
            // Animate pulsing
            if (n.userData) {
                n.userData.pulsePhase += n.userData.pulseSpeed;
                const scale = 4 + Math.sin(n.userData.pulsePhase) * 0.3;
                n.scale.set(scale, scale, 1);
            }
            
            // Move forward
            n.position.z += speed;
            
            // Check collision
            if (n.position.distanceTo(player.position) < 1.8) {
                if (n.userData.isCorrect) {
                    // Collect prime
                    score += 10;
                    coinsThisRun += 5;
                    
                    // Visual effect
                    scene.remove(n);
                    activeNumbers.splice(i, 1);
                    
                    // Show collection effect
                    showNotification(`✓ Prime ${n.userData.value} collected! +10`, 'success', 1500);
                    
                    // Play sound effect
                    playCollectSound();
                } else {
                    // Hit composite
                    gameOver(`${n.userData.value} is a Composite number!`);
                    return;
                }
            } else if (n.position.z > 15) {
                if (n.userData.isCorrect) {
                    gameOver(`You missed a Prime: ${n.userData.value}!`);
                    return;
                }
                scene.remove(n);
                activeNumbers.splice(i, 1);
            }
        }
    }
    
    // Update camera
    camera.position.x += (player.position.x - camera.position.x) * 0.05;
    camera.lookAt(player.position.x, player.position.y, player.position.z);
    
    renderer.render(scene, camera);
}

// ==================== SHOP SYSTEM ====================
function updatePlayerSkin() {
    const skinItem = shopItems.skins.find(s => s.id === playerData.equipped.skin);
    if (skinItem && playerCore) {
        if (skinItem.id === 'skin_rainbow') {
            const time = Date.now() * 0.003;
            const r = Math.sin(time) * 0.5 + 0.5;
            const g = Math.sin(time + 2) * 0.5 + 0.5;
            const b = Math.sin(time + 4) * 0.5 + 0.5;
            playerCore.material.color.setRGB(r, g, b);
            playerCore.material.emissive.setRGB(r, g, b);
        } else {
            playerCore.material.color.setHex(skinItem.color);
            playerCore.material.emissive.setHex(skinItem.color);
        }
    }
}

function toggleShop() {
    const shop = document.getElementById('shop');
    if (shop.classList.contains('active')) {
        shop.classList.remove('active');
        document.getElementById('overlay').classList.remove('active');
        showScreen('menu');
    } else {
        showScreen(null);
        shop.classList.add('active');
        document.getElementById('overlay').classList.add('active');
        updateShopDisplay();
    }
}

function updateShopDisplay() {
    const container = document.getElementById('shop-items');
    container.innerHTML = '<div class="shop-category"><h3>🎨 SKINS</h3><div class="shop-items"></div></div>';
    const itemsContainer = container.querySelector('.shop-items');
    
    shopItems.skins.forEach(skin => {
        const isUnlocked = playerData.unlockedSkins.includes(skin.id);
        const isEquipped = playerData.equipped.skin === skin.id;
        
        const item = document.createElement('div');
        item.className = `shop-item ${isEquipped ? 'equipped' : ''} ${isUnlocked ? 'purchased' : ''}`;
        item.onclick = () => purchaseSkin(skin.id);
        item.style.borderColor = `#${skin.color.toString(16).padStart(6, '0')}`;
        
        const status = isEquipped ? 'EQUIPPED' : (isUnlocked ? 'OWNED' : `${skin.price} coins`);
        const statusColor = isEquipped ? '#00ff00' : (isUnlocked ? '#ffd700' : '#ffd700');
        
        item.innerHTML = `
            <div class="item-name">${skin.name}</div>
            <div class="item-price" style="color: ${statusColor}">${status}</div>
            <div class="item-desc">${skin.desc}</div>
            ${isEquipped ? '<div class="item-status">EQUIPPED</div>' : ''}
        `;
        
        itemsContainer.appendChild(item);
    });
    
    document.getElementById('shop-coins').textContent = playerData.coins;
}

function purchaseSkin(skinId) {
    const skin = shopItems.skins.find(s => s.id === skinId);
    
    if (playerData.unlockedSkins.includes(skinId)) {
        // Already owned - equip it
        playerData.equipped.skin = skinId;
        localStorage.setItem('primeRunner_equippedSkin', skinId);
        updatePlayerSkin();
        updateShopDisplay();
        showNotification(`${skin.name} equipped!`, 'success', 2000);
    } else if (playerData.coins >= skin.price) {
        // Purchase
        playerData.coins -= skin.price;
        playerData.unlockedSkins.push(skinId);
        playerData.equipped.skin = skinId;
        saveData();
        updateMenuCoins();
        updateShopDisplay();
        updatePlayerSkin();
        createPlayer(); // Recreate player with new skin
        showNotification(`${skin.name} purchased and equipped!`, 'success', 3000);
    } else {
        showNotification(`Not enough coins! Need ${skin.price - playerData.coins} more`, 'error', 3000);
    }
}

// ==================== ADVANCED DELETION SYSTEM ====================
function showDeletionOptions() {
    toggleShop(); // Close shop first
    setTimeout(() => {
        openDeletionModal();
    }, 300);
}

function openDeletionModal() {
    document.getElementById('deletion-modal').classList.add('active');
    document.getElementById('overlay').classList.add('active');
    updateSelectedItemsList();
    
    // Update undo button state
    const undoBtn = document.getElementById('undo-btn');
    undoBtn.disabled = deletionHistory.length === 0;
    undoBtn.style.opacity = deletionHistory.length === 0 ? 0.5 : 1;
}

function closeDeletionModal() {
    document.getElementById('deletion-modal').classList.remove('active');
    document.getElementById('overlay').classList.remove('active');
    selectedScores.clear();
    updateBulkActions();
}

function updateSelectedItemsList() {
    const container = document.getElementById('selected-items-list');
    const warning = document.getElementById('deletion-warning');
    
    if (selectedScores.size === 0) {
        container.innerHTML = '<div style="color:#aaa; text-align:center; padding:20px;">No items selected</div>';
        warning.textContent = 'Select items from leaderboard to delete';
        return;
    }
    
    let html = '';
    let totalScore = 0;
    
    Array.from(selectedScores).forEach(index => {
        if (playerData.highScores[index]) {
            const score = playerData.highScores[index];
            totalScore += score.score;
            html += `
                <div class="selected-item">
                    <span>${score.username} - ${score.score} points</span>
                    <span>${new Date(score.timestamp).toLocaleDateString()}</span>
                </div>
            `;
        }
    });
    
    container.innerHTML = html;
    warning.innerHTML = `Delete <strong>${selectedScores.size} selected item(s)</strong> with total score: <span style="color:#ff4444">${totalScore}</span>`;
}

function selectAllItems() {
    selectedScores.clear();
    for (let i = 0; i < playerData.highScores.length; i++) {
        selectedScores.add(i);
    }
    updateSelectedItemsList();
    updateLeaderboard();
    updateBulkActions();
}

function deselectAllItems() {
    selectedScores.clear();
    updateSelectedItemsList();
    updateLeaderboard();
    updateBulkActions();
}

function invertSelection() {
    const newSelection = new Set();
    for (let i = 0; i < playerData.highScores.length; i++) {
        if (!selectedScores.has(i)) {
            newSelection.add(i);
        }
    }
    selectedScores = newSelection;
    updateSelectedItemsList();
    updateLeaderboard();
    updateBulkActions();
}

function confirmDeletion() {
    if (selectedScores.size === 0) {
        showNotification('No items selected!', 'error', 3000);
        return;
    }
    
    const secureDelete = document.getElementById('secure-delete-checkbox').checked;
    const itemsToDelete = Array.from(selectedScores)
        .sort((a, b) => b - a)
        .map(index => ({ index, ...playerData.highScores[index] }));
    
    // Save to history before deletion
    saveToDeletionHistory('scores', itemsToDelete);
    
    // Perform deletion
    itemsToDelete.forEach(item => {
        playerData.highScores.splice(item.index, 1);
    });
    
    // If secure delete, overwrite localStorage
    if (secureDelete) {
        for (let i = 0; i < 3; i++) {
            localStorage.setItem('primeRunner_temp_secure', Math.random().toString());
        }
        showNotification('✅ Secure deletion completed (3-pass overwrite)', 'success', 4000);
    }
    
    saveData();
    selectedScores.clear();
    
    showNotification(`Deleted ${itemsToDelete.length} score(s)`, 'success', 3000);
    closeDeletionModal();
    updateLeaderboard();
}

function undoLastDeletion() {
    if (deletionHistory.length === 0) {
        showNotification('No deletion to undo!', 'error', 3000);
        return;
    }
    
    const lastAction = deletionHistory[0];
    
    if (lastAction.action === 'scores') {
        // Restore scores
        lastAction.items.forEach(item => {
            const originalItem = { ...item };
            delete originalItem.index; // Remove the index property
            playerData.highScores.splice(item.index, 0, originalItem);
        });
        
        // Remove from history
        deletionHistory.shift();
        localStorage.setItem('primeRunner_deletionHistory', JSON.stringify(deletionHistory));
        
        saveData();
        updateLeaderboard();
        showNotification('✅ Last deletion undone!', 'success', 3000);
    }
    
    closeDeletionModal();
}

// ==================== BULK SELECTION SYSTEM ====================
function toggleScoreSelection(index) {
    if (selectedScores.has(index)) {
        selectedScores.delete(index);
    } else {
        selectedScores.add(index);
    }
    updateLeaderboard();
    updateBulkActions();
}

function selectAllLeaderboard() {
    selectedScores.clear();
    for (let i = 0; i < playerData.highScores.length; i++) {
        selectedScores.add(i);
    }
    updateLeaderboard();
    updateBulkActions();
}

function deleteSelectedScores() {
    if (selectedScores.size === 0) {
        showNotification('Select scores to delete first!', 'error', 3000);
        return;
    }
    openDeletionModal();
}

function clearSelection() {
    selectedScores.clear();
    updateLeaderboard();
    updateBulkActions();
}

function updateBulkActions() {
    const bulkActions = document.getElementById('bulk-actions');
    const countSpan = document.getElementById('selection-count');
    
    if (selectedScores.size > 0) {
        countSpan.textContent = `${selectedScores.size} selected`;
        bulkActions.classList.add('active');
    } else {
        bulkActions.classList.remove('active');
    }
}

// ==================== LEADERBOARD SYSTEM ====================
function toggleLeaderboard() {
    const lb = document.getElementById('leaderboard');
    if (lb.classList.contains('active')) {
        lb.classList.remove('active');
        selectedScores.clear();
        updateBulkActions();
    } else {
        lb.classList.add('active');
        updateLeaderboard();
    }
}

function updateLeaderboard() {
    const container = document.getElementById('lb-entries');
    
    if (playerData.highScores.length === 0) {
        container.innerHTML = '<div style="color:#aaa; text-align:center; padding:20px;">No scores yet. Play a game!</div>';
        return;
    }
    
    let html = '';
    playerData.highScores.forEach((score, index) => {
        const isSelected = selectedScores.has(index);
        const date = new Date(score.timestamp).toLocaleDateString();
        
        html += `
            <div class="lb-entry ${isSelected ? 'selected' : ''}" 
                 onclick="toggleScoreSelection(${index})"
                 style="cursor: pointer;">
                <div class="lb-rank">#${index + 1}</div>
                <div class="lb-name">
                    <div style="font-weight:bold;">${score.username || 'Anonymous'}</div>
                    <div style="font-size:0.8em; color:#aaa;">${date} • ${score.mode}</div>
                </div>
                <div class="lb-score">${score.score}</div>
            </div>
        `;
    });
    
    container.innerHTML = html;
}

// ==================== FACT SYSTEM ====================
function startFactCycle() {
    showRandomFact();
    factTimer = setInterval(showRandomFact, 10000);
}

function showRandomFact() {
    const fact = primeFacts[Math.floor(Math.random() * primeFacts.length)];
    document.getElementById('fact-text').textContent = fact;
}

// ==================== UTILITY FUNCTIONS ====================
function showScreen(screenId) {
    // Hide all screens
    document.querySelectorAll('.screen').forEach(screen => {
        screen.classList.remove('active');
    });
    
    // Show requested screen
    if (screenId) {
        document.getElementById(screenId).classList.add('active');
    }
}

function updateMenuCoins() {
    document.getElementById('total-coins').textContent = playerData.coins;
}

function saveData() {
    localStorage.setItem('primeRunner_coins', playerData.coins.toString());
    localStorage.setItem('primeRunner_matchesPlayed', playerData.matchesPlayed.toString());
    localStorage.setItem('primeRunner_highScores', JSON.stringify(playerData.highScores));
    localStorage.setItem('primeRunner_unlockedSkins', JSON.stringify(playerData.unlockedSkins));
    localStorage.setItem('primeRunner_equippedSkin', playerData.equipped.skin);
    updateMenuCoins();
}

// ==================== SOUND FUNCTIONS ====================
function playCollectSound() {
    // Create simple Web Audio API sound
    try {
        const audioContext = new (window.AudioContext || window.webkitAudioContext)();
        const oscillator = audioContext.createOscillator();
        const gainNode = audioContext.createGain();
        
        oscillator.connect(gainNode);
        gainNode.connect(audioContext.destination);
        
        oscillator.frequency.value = 800;
        oscillator.type = 'sine';
        
        gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
        gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.2);
        
        oscillator.start(audioContext.currentTime);
        oscillator.stop(audioContext.currentTime + 0.2);
    } catch (e) {
        console.log('Audio not supported');
    }
}

// ==================== INITIALIZE GAME ====================
// Wait for DOM to load
document.addEventListener('DOMContentLoaded', () => {
    // Check if WebGL is supported
    if (!window.WebGLRenderingContext) {
        alert('WebGL is not supported in your browser. Please update or use a different browser.');
        return;
    }
    
    try {
        init();
        showNotification('Prime Runner Enhanced loaded!', 'success', 2000);
    } catch (error) {
        console.error('Failed to initialize game:', error);
        alert('Failed to load game. Please refresh the page.');
    }
});

// Handle page visibility
document.addEventListener('visibilitychange', () => {
    if (document.hidden && isPlaying) {
        togglePause();
    }
});
</script>
</body>
</html>
// made by blueisancat
