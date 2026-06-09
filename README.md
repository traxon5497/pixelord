<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎨 PixelArt Marketplace - 1000x1000 Pixel Art Canvas</title>
    
    <!-- Google Sign-In Library -->
    <script src="https://accounts.google.com/gsi/client" async defer></script>
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            min-height: 100vh;
            color: white;
        }

        /* Login Overlay */
        .login-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.95);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            backdrop-filter: blur(10px);
        }

        .login-container {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            padding: 40px;
            border-radius: 20px;
            width: 420px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.5);
            animation: slideIn 0.5s ease;
        }

        @keyframes slideIn {
            from {
                transform: translateY(-50px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        .login-container h2 {
            text-align: center;
            margin-bottom: 20px;
            font-size: 28px;
        }

        .login-container input {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: none;
            border-radius: 8px;
            font-size: 16px;
        }

        .login-container button {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            background: #ff6b6b;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 18px;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .login-container button:hover {
            transform: scale(1.02);
        }

        .google-btn-container {
            display: flex;
            justify-content: center;
            margin: 20px 0;
        }

        .divider {
            text-align: center;
            margin: 20px 0;
            position: relative;
        }

        .divider::before,
        .divider::after {
            content: '';
            position: absolute;
            top: 50%;
            width: 45%;
            height: 1px;
            background: rgba(255,255,255,0.3);
        }

        .divider::before {
            left: 0;
        }

        .divider::after {
            right: 0;
        }

        .divider span {
            background: rgba(255,255,255,0.2);
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 12px;
        }

        .switch-mode {
            text-align: center;
            margin-top: 15px;
            cursor: pointer;
            color: #ffd93d;
        }

        .error-msg {
            color: #ff6b6b;
            text-align: center;
            margin-top: 10px;
            font-size: 14px;
        }

        /* Main App */
        .app-container {
            display: none;
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }

        /* Header */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding: 15px 25px;
            background: rgba(255,255,255,0.1);
            border-radius: 15px;
            backdrop-filter: blur(10px);
            flex-wrap: wrap;
            gap: 15px;
        }

        .logo h1 {
            font-size: 28px;
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .logo p {
            font-size: 12px;
            color: #aaa;
        }

        .stats {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
        }

        .stat-card {
            text-align: center;
            background: rgba(0,0,0,0.3);
            padding: 8px 15px;
            border-radius: 10px;
        }

        .stat-value {
            font-size: 20px;
            font-weight: bold;
            color: #4ecdc4;
        }

        .stat-label {
            font-size: 11px;
            color: #aaa;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
        }

        .logout-btn {
            padding: 8px 20px;
            background: #ff6b6b;
            border: none;
            border-radius: 8px;
            color: white;
            cursor: pointer;
        }

        /* Controls */
        .controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding: 15px;
            background: rgba(255,255,255,0.1);
            border-radius: 15px;
            flex-wrap: wrap;
            gap: 15px;
        }

        .zoom-controls button {
            padding: 8px 15px;
            margin: 0 5px;
            background: #4ecdc4;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
        }

        .selected-info {
            background: rgba(0,0,0,0.5);
            padding: 8px 15px;
            border-radius: 10px;
            font-size: 14px;
        }

        .buy-btn {
            padding: 10px 30px;
            background: linear-gradient(135deg, #ff6b6b, #ff8e53);
            border: none;
            border-radius: 25px;
            color: white;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s;
        }

        .buy-btn:hover:not(:disabled) {
            transform: scale(1.05);
        }

        .buy-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        /* Admin Panel */
        .admin-panel {
            margin-top: 20px;
            padding: 20px;
            background: linear-gradient(135deg, rgba(255,215,0,0.2), rgba(255,100,0,0.1));
            border-radius: 15px;
            border: 2px solid #ffd700;
            display: none;
        }

        .admin-panel h3 {
            color: #ffd700;
            margin-bottom: 15px;
        }

        .admin-controls {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
        }

        .admin-controls input, .admin-controls select {
            padding: 10px;
            border-radius: 8px;
            border: none;
            font-size: 14px;
        }

        .admin-controls button {
            padding: 10px 20px;
            background: #ffd700;
            color: #333;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
        }

        .admin-controls button:hover {
            background: #ffed4a;
        }

        /* Canvas Container */
        .canvas-container {
            overflow: auto;
            border-radius: 15px;
            background: #000;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
            max-height: 55vh;
            border: 3px solid #4ecdc4;
        }

        canvas {
            display: block;
            cursor: pointer;
            image-rendering: crisp-edges;
            image-rendering: pixelated;
        }

        /* Instructions */
        .instructions {
            margin-top: 20px;
            padding: 15px;
            background: rgba(255,255,255,0.1);
            border-radius: 15px;
            text-align: center;
            font-size: 14px;
        }

        .instruction-badge {
            display: inline-block;
            background: #ff6b6b;
            padding: 5px 10px;
            border-radius: 20px;
            margin: 0 5px;
        }

        .admin-badge {
            background: #ffd700;
            color: #333;
        }

        @media (max-width: 768px) {
            .header, .controls {
                flex-direction: column;
                text-align: center;
            }
            .stat-value {
                font-size: 16px;
            }
            .login-container {
                width: 90%;
                margin: 20px;
                padding: 30px 20px;
            }
        }

        .toast {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #4ecdc4;
            color: white;
            padding: 12px 24px;
            border-radius: 8px;
            z-index: 1000;
            animation: slideInRight 0.3s ease;
        }

        @keyframes slideInRight {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }
    </style>
</head>
<body>

<!-- Login Overlay -->
<div class="login-overlay" id="loginOverlay">
    <div class="login-container">
        <h2>🎨 PixelArt Marketplace</h2>
        
        <!-- Google Sign-In Button -->
        <div class="google-btn-container">
            <div id="g_id_onload"
                 data-client_id="YOUR_GOOGLE_CLIENT_ID_HERE"
                 data-callback="handleGoogleLogin"
                 data-auto_prompt="false">
            </div>
            <div class="g_id_signin"
                 data-type="standard"
                 data-size="large"
                 data-theme="outline"
                 data-text="sign_in_with"
                 data-shape="rectangular"
                 data-logo_alignment="left">
            </div>
        </div>
        
        <div class="divider">
            <span>OR</span>
        </div>
        
        <div id="authForm">
            <input type="text" id="loginUsername" placeholder="Username" autocomplete="off">
            <input type="password" id="loginPassword" placeholder="Password">
            <button onclick="login()">Sign in with Email</button>
            <div class="switch-mode" onclick="showSignup()">Don't have an account? Sign up</div>
            <div id="loginError" class="error-msg"></div>
        </div>
        
        <div id="signupForm" style="display:none;">
            <input type="text" id="signupUsername" placeholder="Username">
            <input type="email" id="signupEmail" placeholder="Email">
            <input type="password" id="signupPassword" placeholder="Password">
            <button onclick="signup()">Create Account</button>
            <div class="switch-mode" onclick="showLogin()">Already have an account? Login</div>
            <div id="signupError" class="error-msg"></div>
        </div>
    </div>
</div>

<!-- Main App -->
<div class="app-container" id="appContainer">
    <div class="header">
        <div class="logo">
            <h1>🎨 PixelArt Marketplace</h1>
            <p>1000x1000 canvas • $0.50 per pixel</p>
        </div>
        <div class="stats">
            <div class="stat-card">
                <div class="stat-value" id="soldPixels">0</div>
                <div class="stat-label">Pixels Sold</div>
            </div>
            <div class="stat-card">
                <div class="stat-value" id="totalRevenue">$0</div>
                <div class="stat-label">Total Revenue</div>
            </div>
            <div class="stat-card">
                <div class="stat-value" id="userBalance">$0</div>
                <div class="stat-label">Your Balance</div>
            </div>
        </div>
        <div class="user-info">
            <img id="userAvatar" class="user-avatar" style="display:none;" alt="Avatar">
            <span id="usernameDisplay">User</span>
            <span id="adminBadge" style="display:none; background:#ffd700; color:#333; padding:2px 8px; border-radius:20px; font-size:12px;">👑 ADMIN</span>
            <button class="logout-btn" onclick="logout()">Logout</button>
        </div>
    </div>

    <div class="controls">
        <div class="zoom-controls">
            <button onclick="zoomIn()">🔍 Zoom In</button>
            <button onclick="zoomOut()">🔍 Zoom Out</button>
            <span style="margin-left: 10px;">Zoom: <span id="zoomLevel">1</span>x</span>
        </div>
        <div class="selected-info">
            📍 (<span id="selectedX">-</span>, <span id="selectedY">-</span>) • 
            💰 $<span id="selectedPrice">0.50</span> • 
            🎨 <span id="selectedStatus">Available</span>
        </div>
        <button class="buy-btn" id="buyBtn" onclick="buyPixel()" disabled>Buy Pixel ($0.50)</button>
    </div>

    <div class="canvas-container">
        <canvas id="pixelCanvas" width="1000" height="1000"></canvas>
    </div>

    <!-- Admin Panel (Only visible to uaxolive@gmail.com) -->
    <div class="admin-panel" id="adminPanel">
        <h3>👑 Admin Control Panel</h3>
        <div class="admin-controls">
            <input type="number" id="adminPrice" placeholder="Price in cents (e.g., 50)" value="50">
            <button onclick="adminSetGlobalPrice()">💰 Set All Unowned Pixels Price</button>
            <button onclick="adminResetUnsoldPixels()">🔄 Reset All Unsold Pixels</button>
            <button onclick="adminAddFunds()">💵 Add $100 to All Users</button>
            <button onclick="adminViewStats()">📊 View Detailed Stats</button>
        </div>
        <div style="margin-top: 15px;">
            <input type="text" id="adminInviteEmail" placeholder="Email to invite as admin">
            <button onclick="adminInvite()">📧 Send Admin Invite</button>
        </div>
        <div id="adminMessage" style="margin-top: 10px; color: #ffd700; font-size: 12px;"></div>
    </div>

    <div class="instructions">
        🎯 <strong>How it works:</strong> Each pixel costs <span class="instruction-badge">$0.50</span> • 
        Click any empty pixel to purchase • Your color will fill the spot • 
        Together we create ONE massive pixel art masterpiece! • 
        <span class="instruction-badge">1000x1000 = 1,000,000 pixels total</span> • 
        <span class="instruction-badge admin-badge">👑 Admin: uaxolive@gmail.com</span>
    </div>
</div>

<script>
    // ==================== ADMIN EMAIL (ONLY THIS EMAIL CAN SEE ADMIN PANEL) ====================
    const ADMIN_EMAIL = "uaxolive@gmail.com";
    
    // ==================== GOOGLE SIGN-IN ====================
    // Replace with your Google Client ID (get from Google Cloud Console)
    const GOOGLE_CLIENT_ID = "YOUR_GOOGLE_CLIENT_ID_HERE";
    
    function updateGoogleConfig() {
        const googleConfig = document.getElementById('g_id_onload');
        if (googleConfig && GOOGLE_CLIENT_ID !== "YOUR_GOOGLE_CLIENT_ID_HERE") {
            googleConfig.setAttribute('data-client_id', GOOGLE_CLIENT_ID);
        }
    }
    
    window.handleGoogleLogin = function(response) {
        const payload = parseJwt(response.credential);
        
        if (payload) {
            const googleUser = {
                email: payload.email,
                name: payload.name,
                picture: payload.picture,
                googleId: payload.sub
            };
            
            const users = JSON.parse(localStorage.getItem('pixelart_users'));
            let username = googleUser.name.replace(/\s/g, '').toLowerCase();
            
            let finalUsername = username;
            let counter = 1;
            while (users[finalUsername] && users[finalUsername].googleId !== googleUser.googleId) {
                finalUsername = username + counter;
                counter++;
            }
            
            if (users[finalUsername] && users[finalUsername].googleId === googleUser.googleId) {
                currentUser = {
                    username: finalUsername,
                    email: users[finalUsername].email,
                    balance: users[finalUsername].balance,
                    totalSpent: users[finalUsername].totalSpent,
                    pixelsOwned: users[finalUsername].pixelsOwned,
                    avatar: googleUser.picture,
                    isGoogleUser: true,
                    isAdmin: users[finalUsername].email === ADMIN_EMAIL
                };
            } else if (users[finalUsername]) {
                finalUsername = username + Math.floor(Math.random() * 1000);
                createGoogleUser(finalUsername, googleUser);
            } else {
                createGoogleUser(finalUsername, googleUser);
            }
            
            if (currentUser) {
                completeLogin();
            }
        }
    };
    
    function createGoogleUser(username, googleUser) {
        const users = JSON.parse(localStorage.getItem('pixelart_users'));
        
        users[username] = {
            username: username,
            email: googleUser.email,
            password: null,
            googleId: googleUser.googleId,
            balance: 10000,
            totalSpent: 0,
            pixelsOwned: 0,
            avatar: googleUser.picture,
            createdAt: new Date().toISOString(),
            isAdmin: googleUser.email === ADMIN_EMAIL
        };
        
        localStorage.setItem('pixelart_users', JSON.stringify(users));
        
        currentUser = {
            username: username,
            email: googleUser.email,
            balance: 10000,
            totalSpent: 0,
            pixelsOwned: 0,
            avatar: googleUser.picture,
            isGoogleUser: true,
            isAdmin: googleUser.email === ADMIN_EMAIL
        };
    }
    
    function parseJwt(token) {
        try {
            const base64Url = token.split('.')[1];
            const base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            const jsonPayload = decodeURIComponent(atob(base64).split('').map(function(c) {
                return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));
            return JSON.parse(jsonPayload);
        } catch (e) {
            return null;
        }
    }
    
    // ==================== GLOBAL STATE ====================
    let currentUser = null;
    let zoomLevel = 1;
    let selectedPixel = { x: -1, y: -1 };
    let pixelData = [];
    let canvas = document.getElementById('pixelCanvas');
    let ctx = canvas.getContext('2d');
    
    // ==================== DATABASE ====================
    function initDatabase() {
        if (!localStorage.getItem('pixelart_users')) {
            const users = {
                'admin': {
                    username: 'admin',
                    email: ADMIN_EMAIL,
                    password: 'admin123',
                    balance: 100000,
                    totalSpent: 0,
                    pixelsOwned: 0,
                    isAdmin: true
                }
            };
            localStorage.setItem('pixelart_users', JSON.stringify(users));
        }
        
        if (!localStorage.getItem('pixelart_pixels')) {
            console.log('Initializing 1,000,000 pixels...');
            const pixels = [];
            for (let x = 0; x < 1000; x++) {
                pixels[x] = [];
                for (let y = 0; y < 1000; y++) {
                    const r = Math.floor(128 + 127 * Math.sin(x / 100));
                    const g = Math.floor(128 + 127 * Math.sin(y / 100));
                    const b = Math.floor(128 + 127 * Math.sin((x + y) / 100));
                    const color = `rgb(${r}, ${g}, ${b})`;
                    
                    pixels[x][y] = {
                        owner: null,
                        ownerName: null,
                        ownerEmail: null,
                        color: color,
                        targetColor: color,
                        price: 50,
                        soldAt: null
                    };
                }
            }
            localStorage.setItem('pixelart_pixels', JSON.stringify(pixels));
            console.log('Pixel board initialized!');
        }
        
        if (!localStorage.getItem('pixelart_transactions')) {
            localStorage.setItem('pixelart_transactions', JSON.stringify([]));
        }
    }
    
    function completeLogin() {
        sessionStorage.setItem('pixelart_currentUser', JSON.stringify(currentUser));
        document.getElementById('loginOverlay').style.display = 'none';
        document.getElementById('appContainer').style.display = 'block';
        document.getElementById('usernameDisplay').textContent = currentUser.username;
        
        if (currentUser.avatar) {
            const avatarImg = document.getElementById('userAvatar');
            avatarImg.src = currentUser.avatar;
            avatarImg.style.display = 'inline-block';
        }
        
        // Show admin panel ONLY for uaxolive@gmail.com
        if (currentUser.email === ADMIN_EMAIL) {
            document.getElementById('adminBadge').style.display = 'inline-block';
            document.getElementById('adminPanel').style.display = 'block';
            showToast('👑 Welcome Admin! You have full control over the marketplace.', '#ffd700');
        } else {
            document.getElementById('adminBadge').style.display = 'none';
            document.getElementById('adminPanel').style.display = 'none';
            showToast(`Welcome ${currentUser.username}! You have $${(currentUser.balance / 100).toFixed(2)} to spend.`, '#4ecdc4');
        }
        
        loadPixels();
        updateStats();
    }
    
    function showToast(message, color = '#4ecdc4') {
        const toast = document.createElement('div');
        toast.className = 'toast';
        toast.style.backgroundColor = color;
        toast.textContent = message;
        document.body.appendChild(toast);
        setTimeout(() => toast.remove(), 3000);
    }
    
    function loadPixels() {
        const pixels = JSON.parse(localStorage.getItem('pixelart_pixels'));
        pixelData = pixels;
        drawPixels();
        updateStats();
    }
    
    function drawPixels() {
        if (!ctx || !pixelData.length) return;
        
        const scale = zoomLevel;
        canvas.style.width = `${1000 * scale}px`;
        canvas.style.height = `${1000 * scale}px`;
        
        ctx.clearRect(0, 0, 1000, 1000);
        
        for (let x = 0; x < 1000; x++) {
            for (let y = 0; y < 1000; y++) {
                const pixel = pixelData[x][y];
                
                if (pixel) {
                    ctx.fillStyle = pixel.owner ? pixel.color : '#2a2a2a';
                    ctx.fillRect(x, y, 1, 1);
                    
                    if (selectedPixel.x === x && selectedPixel.y === y) {
                        ctx.strokeStyle = '#ffd700';
                        ctx.lineWidth = 2;
                        ctx.strokeRect(x, y, 1, 1);
                    }
                    
                    ctx.strokeStyle = '#333';
                    ctx.lineWidth = 0.2;
                    ctx.strokeRect(x, y, 1, 1);
                }
            }
        }
    }
    
    function updateStats() {
        if (!pixelData.length) return;
        
        let soldCount = 0;
        let totalRevenue = 0;
        
        for (let x = 0; x < 1000; x++) {
            for (let y = 0; y < 1000; y++) {
                if (pixelData[x][y].owner) {
                    soldCount++;
                    totalRevenue += pixelData[x][y].price;
                }
            }
        }
        
        document.getElementById('soldPixels').textContent = `${soldCount.toLocaleString()} / 1,000,000`;
        document.getElementById('totalRevenue').textContent = `$${(totalRevenue / 100).toLocaleString()}`;
        
        if (currentUser) {
            document.getElementById('userBalance').textContent = `$${(currentUser.balance / 100).toFixed(2)}`;
        }
    }
    
    function updateSelectedInfo(x, y) {
        if (!pixelData[x] || !pixelData[x][y]) return;
        
        const pixel = pixelData[x][y];
        
        document.getElementById('selectedX').textContent = x;
        document.getElementById('selectedY').textContent = y;
        document.getElementById('selectedPrice').textContent = (pixel.price / 100).toFixed(2);
        
        const isOwned = pixel.owner !== null;
        document.getElementById('selectedStatus').textContent = isOwned ? `Owned by ${pixel.ownerName}` : 'Available';
        
        const buyBtn = document.getElementById('buyBtn');
        if (!isOwned && currentUser && currentUser.balance >= pixel.price) {
            buyBtn.disabled = false;
            buyBtn.textContent = `Buy Pixel ($${(pixel.price / 100).toFixed(2)})`;
        } else if (!currentUser) {
            buyBtn.disabled = true;
            buyBtn.textContent = 'Login to Buy';
        } else if (currentUser && currentUser.balance < pixel.price) {
            buyBtn.disabled = true;
            buyBtn.textContent = `Need $${((pixel.price - currentUser.balance) / 100).toFixed(2)} more`;
        } else {
            buyBtn.disabled = true;
            buyBtn.textContent = 'Already Owned';
        }
    }
    
    function zoomIn() {
        if (zoomLevel < 4) {
            zoomLevel++;
            document.getElementById('zoomLevel').textContent = zoomLevel;
            drawPixels();
        }
    }
    
    function zoomOut() {
        if (zoomLevel > 0.5) {
            zoomLevel--;
            document.getElementById('zoomLevel').textContent = zoomLevel;
            drawPixels();
        }
    }
    
    function buyPixel() {
        if (!currentUser || selectedPixel.x === -1 || selectedPixel.y === -1) {
            alert('Please login first!');
            return;
        }
        
        const x = selectedPixel.x;
        const y = selectedPixel.y;
        const pixel = pixelData[x][y];
        
        if (pixel.owner) {
            alert('This pixel is already owned!');
            return;
        }
        
        if (currentUser.balance < pixel.price) {
            alert(`Insufficient balance! You need $${(pixel.price / 100).toFixed(2)}`);
            return;
        }
        
        const price = pixel.price;
        
        const users = JSON.parse(localStorage.getItem('pixelart_users'));
        users[currentUser.username].balance -= price;
        users[currentUser.username].totalSpent += price;
        users[currentUser.username].pixelsOwned += 1;
        localStorage.setItem('pixelart_users', JSON.stringify(users));
        
        const pixels = JSON.parse(localStorage.getItem('pixelart_pixels'));
        pixels[x][y] = {
            ...pixel,
            owner: currentUser.username,
            ownerName: currentUser.username,
            ownerEmail: currentUser.email,
            soldAt: new Date().toISOString()
        };
        localStorage.setItem('pixelart_pixels', JSON.stringify(pixels));
        
        currentUser.balance = users[currentUser.username].balance;
        currentUser.totalSpent = users[currentUser.username].totalSpent;
        currentUser.pixelsOwned = users[currentUser.username].pixelsOwned;
        
        const transactions = JSON.parse(localStorage.getItem('pixelart_transactions'));
        transactions.push({
            id: Date.now(),
            userId: currentUser.username,
            userEmail: currentUser.email,
            pixelX: x,
            pixelY: y,
            amount: price,
            date: new Date().toISOString()
        });
        localStorage.setItem('pixelart_transactions', JSON.stringify(transactions));
        
        loadPixels();
        updateSelectedInfo(x, y);
        
        const buyBtn = document.getElementById('buyBtn');
        buyBtn.textContent = '✓ Purchased!';
        setTimeout(() => {
            if (buyBtn) {
                buyBtn.textContent = `Buy Pixel ($${(price / 100).toFixed(2)})`;
            }
        }, 1500);
        
        showToast(`🎉 You bought pixel (${x}, ${y}) for $${(price/100).toFixed(2)}!`, '#4ecdc4');
        checkCompletion();
    }
    
    function checkCompletion() {
        let soldCount = 0;
        for (let x = 0; x < 1000; x++) {
            for (let y = 0; y < 1000; y++) {
                if (pixelData[x][y].owner) soldCount++;
            }
        }
        
        if (soldCount === 1000000) {
            setTimeout(() => {
                alert('🏆 AMAZING! All 1,000,000 pixels have been sold! The masterpiece is complete! 🏆');
            }, 500);
        }
    }
    
    // ==================== ADMIN FUNCTIONS (Only work if user is admin) ====================
    function adminSetGlobalPrice() {
        if (currentUser?.email !== ADMIN_EMAIL) {
            alert('Admin access required');
            return;
        }
        
        const newPrice = parseInt(document.getElementById('adminPrice').value);
        if (isNaN(newPrice) || newPrice < 1) {
            alert('Please enter a valid price in cents');
            return;
        }
        
        const pixels = JSON.parse(localStorage.getItem('pixelart_pixels'));
        let count = 0;
        
        for (let x = 0; x < 1000; x++) {
            for (let y = 0; y < 1000; y++) {
                if (!pixels[x][y].owner) {
                    pixels[x][y].price = newPrice;
                    count++;
                }
            }
        }
        
        localStorage.setItem('pixelart_pixels', JSON.stringify(pixels));
        loadPixels();
        document.getElementById('adminMessage').innerHTML = `✅ Set price for ${count.toLocaleString()} unsold pixels to $${(newPrice/100).toFixed(2)}`;
        showToast(`Admin: Set price for ${count.toLocaleString()} pixels to $${(newPrice/100).toFixed(2)}`, '#ffd700');
    }
    
    function adminResetUnsoldPixels() {
        if (currentUser?.email !== ADMIN_EMAIL) {
            alert('Admin access required');
            return;
        }
        
        if (confirm('Are you sure? This will reset ALL unsold pixels to their original colors.')) {
            const pixels = JSON.parse(localStorage.getItem('pixelart_pixels'));
            let count = 0;
            
            for (let x = 0; x < 1000; x++) {
                for (let y = 0; y < 1000; y++) {
                    if (!pixels[x][y].owner) {
                        const r = Math.floor(128 + 127 * Math.sin(x / 100));
                        const g = Math.floor(128 + 127 * Math.sin(y / 100));
                        const b = Math.floor(128 + 127 * Math.sin((x + y) / 100));
                        pixels[x][y].color = `rgb(${r}, ${g}, ${b})`;
                        count++;
                    }
                }
            }
            
            localStorage.setItem('pixelart_pixels', JSON.stringify(pixels));
            loadPixels();
            document.getElementById('adminMessage').innerHTML = `✅ Reset ${count.toLocaleString()} unsold pixels`;
            showToast(`Admin: Reset ${count.toLocaleString()} unsold pixels`, '#ffd700');
        }
    }
    
    function adminAddFunds() {
        if (currentUser?.email !== ADMIN_EMAIL) {
            alert('Admin access required');
            return;
        }
        
        const users = JSON.parse(localStorage.getItem('pixelart_users'));
        let count = 0;
        
        for (let username in users) {
            users[username].balance += 10000; // Add $100
            count++;
        }
        
        localStorage.setItem('pixelart_users', JSON.stringify(users));
        
        // Update current user if they are in the list
        if (currentUser && users[currentUser.username]) {
            currentUser.balance = users[currentUser.username].balance;
            sessionStorage.setItem('pixelart_currentUser', JSON.stringify(currentUser));
        }
        
        updateStats();
        document.getElementById('adminMessage').innerHTML = `✅ Added $100 to ${count} users`;
        showToast(`Admin: Added $100 to all ${count} users!`, '#ffd700');
    }
    
    function adminViewStats() {
        if (currentUser?.email !== ADMIN_EMAIL) {
            alert('Admin access required');
            return;
        }
        
        const users = JSON.parse(localStorage.getItem('pixelart_users'));
        const transactions = JSON.parse(localStorage.getItem('pixelart_transactions'));
        
        let totalUsers = Object.keys(users).length;
        let totalTransactions = transactions.length;
        let totalRevenue = 0;
        
        for (let t of transactions) {
            totalRevenue += t.amount;
        }
        
        alert(`📊 ADMIN STATS:\n\n👥 Total Users: ${totalUsers}\n💸 Total Transactions: ${totalTransactions}\n💰 Total Revenue: $${(totalRevenue/100).toFixed(2)}\n🎨 Total Pixels: 1,000,000\n👑 Admin Email: ${ADMIN_EMAIL}`);
    }
    
    function adminInvite() {
        if (currentUser?.email !== ADMIN_EMAIL) {
            alert('Admin access required');
            return;
        }
        
        const email = document.getElementById('adminInviteEmail').value;
        if (!email) {
            alert('Please enter an email to invite');
            return;
        }
        
        // Find user by email and make them admin
        const users = JSON.parse(localStorage.getItem('pixelart_users'));
        let found = false;
        
        for (let username in users) {
            if (users[username].email === email) {
                users[username].isAdmin = true;
                found = true;
                break;
            }
        }
        
        if (found) {
            localStorage.setItem('pixelart_users', JSON.stringify(users));
            document.getElementById('adminMessage').innerHTML = `✅ Invited ${email} as admin! They will see admin panel on next login.`;
            showToast(`Admin invited ${email}`, '#ffd700');
        } else {
            document.getElementById('adminMessage').innerHTML = `❌ User with email ${email} not found. They need to sign up first.`;
        }
        
        document
