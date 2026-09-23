<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cricket Central</title>
    <style>
        :root {
            --primary: #009688;
            --primary-dark: #00796b;
            --bg-light: #f4f6f9;
            --card-bg: #ffffff;
            --text-main: #333333;
            --text-muted: #666666;
            --accent: #ff9800;
            --danger: #dc3545;
        }
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background: var(--bg-light); 
            margin: 0; 
            padding: 0; 
            color: var(--text-main); 
        }
        /* Top Green Header */
        .app-header {
            background: var(--primary);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        .app-header h2 { margin: 0; font-size: 20px; }
        .admin-badge-btn {
            background: white;
            color: var(--primary);
            border: none;
            padding: 6px 12px;
            border-radius: 4px;
            font-weight: bold;
            cursor: pointer;
        }
        /* Tabs */
        .tabs-container {
            background: var(--primary-dark);
            display: flex;
            justify-content: space-around;
            padding: 0 10px;
        }
        .tab-btn {
            background: none;
            border: none;
            color: rgba(255,255,255,0.7);
            padding: 12px 15px;
            font-weight: bold;
            cursor: pointer;
            font-size: 14px;
            text-transform: uppercase;
        }
        .tab-btn.active {
            color: white;
            border-bottom: 3px solid white;
        }
        .container { max-width: 500px; margin: auto; padding: 15px; }
        .card { 
            background: var(--card-bg); 
            padding: 15px; 
            border-radius: 8px; 
            margin-bottom: 15px; 
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            border: 1px solid #e0e0e0;
        }
        input, select, button { 
            width: 100%; 
            padding: 10px; 
            margin-top: 8px; 
            border: 1px solid #ccc; 
            border-radius: 6px; 
            box-sizing: border-box; 
            font-size: 14px;
        }
        button.action-btn { 
            background: var(--primary); 
            color: white; 
            border: none; 
            font-weight: bold; 
            cursor: pointer; 
        }
        button.action-btn:hover { background: var(--primary-dark); }
        .logout-btn { background: var(--danger); color: white; margin-top: 10px; border: none; font-weight: bold; padding: 10px; border-radius: 6px; cursor: pointer;}
        .hidden { display: none !important; }
        
        /* Match Card Styling */
        .series-title {
            background: #e0f2f1;
            color: var(--primary-dark);
            padding: 8px 12px;
            font-weight: bold;
            border-radius: 6px;
            margin-bottom: 10px;
            font-size: 14px;
        }
        .match-box {
            background: #fff;
            border: 1px solid #e0e0e0;
            border-radius: 8px;
            padding: 12px;
            margin-bottom: 10px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }
        .match-info-top {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: var(--text-muted);
            margin-bottom: 6px;
        }
        .match-teams {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: bold;
            font-size: 15px;
            margin: 8px 0;
        }
        .match-result-banner {
            background: #ffebee;
            color: #c62828;
            padding: 5px 8px;
            font-size: 12px;
            border-radius: 4px;
            margin-top: 8px;
            font-weight: bold;
        }
        /* Table Styling */
        table { width: 100%; border-collapse: collapse; margin-top: 5px; font-size: 13px; }
        th, td { border: 1px solid #e0e0e0; padding: 8px; text-align: center; }
        th { background: #f5f5f5; color: #333; }
        .plan-box { border: 1px solid var(--primary); padding: 10px; border-radius: 6px; margin-top: 8px; background: #f9f9f9; }
    </style>
</head>
<body>

    <!-- TOP HEADER -->
    <div class="app-header">
        <h2>Cricket Central</h2>
        <button class="admin-badge-btn" onclick="checkAdminAccess()">Admin</button>
    </div>

    <!-- TABS -->
    <div id="appTabs" class="tabs-container hidden">
        <button class="tab-btn active" onclick="switchTab('matches')">Matches</button>
        <button class="tab-btn" onclick="switchTab('points')">Points Table</button>
        <button class="tab-btn" onclick="switchTab('groups')">Groups</button>
        <button class="tab-btn" onclick="switchTab('wallet')">Wallet</button>
    </div>

<div class="container">
    <!-- LOGIN SCREEN -->
    <div id="loginSection" class="card">
        <h3>Cricket App Login</h3>
        <p style="text-align: center; font-size: 13px; color: var(--text-muted);">Aage badhne ke liye apna phone number dalein:<br><span style="color: var(--primary); font-weight: bold;">(Naye number par 260 Free Points milenge!)</span></p>
        
        <!-- Step 1: Phone Input -->
        <div id="phoneStep">
            <input type="tel" id="userPhoneInput" placeholder="Apna Mobile Number Dalein" maxlength="10">
            <button class="action-btn" onclick="sendOTP()">OTP Bhejein</button>
        </div>

        <!-- Step 2: OTP Verification -->
        <div id="otpStep" class="hidden" style="margin-top: 15px; border-top: 1px dashed #ccc; padding-top: 10px;">
            <p style="font-size: 13px; color: #2e7d32; text-align: center;">Aapke number par OTP bheja gaya hai.</p>
            <input type="number" id="otpInput" placeholder="4-digit OTP Dalein" maxlength="4">
            <button class="action-btn" onclick="verifyOTP()" style="background: #2e7d32;">Verify & Login Karein</button>
            <button onclick="resetLogin()" style="background: #757575; color:white; border:none; padding:8px; border-radius:6px; margin-top:5px; width:100%; cursor:pointer;">Number Badlein</button>
        </div>
    </div>

    <!-- MAIN DASHBOARD CONTAINER -->
    <div id="dashboardSection" class="hidden">
        
        <!-- TAB 1: MATCHES -->
        <div id="tabMatches" class="tab-content">
            <div class="card">
                <h3>📅 Live & Upcoming Schedule</h3>
                <div id="scheduleList"></div>
            </div>
        </div>

        <!-- TAB 2: POINTS TABLE -->
        <div id="tabPoints" class="tab-content hidden">
            <div class="card">
                <h3>🏆 Points Table (ICC Formula)</h3>
                <div id="pointsTableContainer"></div>
            </div>
        </div>

        <!-- TAB 3: GROUPS -->
        <div id="tabGroups" class="tab-content hidden">
            <div class="card">
                <h3>👥 Group Management</h3>
                <input type="text" id="groupNameInput" placeholder="Group Name (jaise: Group A)">
                <input type="text" id="groupTeamsInput" placeholder="Teams comma se alag karein (jaise: IND, PAK, AUS)">
                <button class="action-btn" onclick="createGroup()">Group Banayein</button>
                <div id="groupsContainer" style="margin-top: 15px;"></div>
            </div>
        </div>

        <!-- TAB 4: WALLET & SUBSCRIPTION -->
        <div id="tabWallet" class="tab-content hidden">
            <div class="card" style="background: #e0f2f1;">
                <h3 style="color: var(--primary-dark);">💰 Aapka Wallet</h3>
                <p style="font-size: 16px; text-align: center;"><strong>Available Points:</strong> <span id="userPoints" style="color: var(--primary); font-weight: bold; font-size: 20px;">0</span></p>
                <p id="subStatus" style="text-align: center; font-weight: bold; color: #2e7d32; font-size: 13px;"></p>
            </div>

            <!-- SUBSCRIPTION PLANS -->
            <div class="card">
                <h3>⭐ Subscription Plans</h3>
                <p style="font-size: 12px; color: var(--text-muted); text-align: center;">Admin Mode aur features unlock karne ke liye plan lein:</p>
                
                <div class="plan-box">
                    <p style="margin: 0 0 5px 0;"><strong>30 Minutes Plan:</strong> 149 Points</p>
                    <button class="action-btn" onclick="buySubscription(149, '30 Minutes Plan')">Buy 30 Mins Plan</button>
                </div>
                <div class="plan-box">
                    <p style="margin: 0 0 5px 0;"><strong>Monthly Plan:</strong> 690 Points</p>
                    <button class="action-btn" onclick="buySubscription(690, 'Monthly Plan')">Buy Monthly</button>
                </div>
                <div class="plan-box">
                    <p style="margin: 0 0 5px 0;"><strong>Half Yearly Plan:</strong> 256 Points</p>
                    <button class="action-btn" onclick="buySubscription(256, 'Half Yearly Plan')">Buy Half Yearly</button>
                </div>
                <div class="plan-box">
                    <p style="margin: 0 0 5px 0;"><strong>Yearly Plan:</strong> 8000 Points</p>
                    <button class="action-btn" onclick="buySubscription(8000, 'Yearly Plan')">Buy Yearly</button>
                </div>
            </div>

            <!-- BUY POINTS INFO -->
            <div class="card">
                <h3>💳 Points Kaise Kharidein?</h3>
                <ul style="font-size: 13px; padding-left: 18px; color: var(--text-muted);">
                    <li>₹80 = 800 Points</li>
                    <li>₹150 = 1500 Points</li>
                    <li>₹249 = 4000 Points</li>
                </ul>
                <p style="text-align: center; font-weight: bold; color: var(--danger);">Sampark Karein: 9568981484</p>
            </div>
        </div>

        <!-- ADMIN PANEL SECTION -->
        <div id="adminPanelSection" class="card hidden" style="border: 2px solid var(--accent); background: #fffde7;">
            <h3 style="color: #f57c00;">👑 Admin Control Panel</h3>
            
            <!-- Send Points to User (Only Admin 9569981484) -->
            <div id="adminSendPointsBox" class="hidden" style="background: #fff9c4; padding: 10px; border-radius: 6px; margin-bottom: 10px;">
                <h4 style="margin: 0 0 5px 0; color: #f57c00;">Point Transfer (Admin Special)</h4>
                <input type="tel" id="targetUserPhone" placeholder="User ka 10-digit Mobile Number" maxlength="10">
                <input type="number" id="transferPointsAmount" placeholder="Kitne points bhejne hain?">
                <button class="action-btn" onclick="adminTransferPoints()" style="background: #f57c00;">Points Bhejein / Add Karein</button>
            </div>

            <h4 style="margin-top: 10px;">Match Schedule Jodein</h4>
            <input type="text" id="seriesName" placeholder="Series Name (jaise: West Indies tour of India)">
            <input type="text" id="matchFormat" placeholder="Format (jaise: 1st T20 / ODI / Test)">
            <input type="text" id="team1" placeholder="Team 1 (jaise: India)">
            <input type="text" id="team2" placeholder="Team 2 (jaise: West Indies)">
            <input type="text" id="venue" placeholder="Venue / Stadium">
            <input type="text" id="matchTime" placeholder="Date & Time (jaise: 28 Sep, 2026 | 3:30 PM)">
            <button class="action-btn" onclick="addNewMatch()">Match Save Karein</button>

            <h4 style="margin-top: 15px;">Match Result & Scores Update</h4>
            <select id="matchSelectForUpdate"></select>
            <input type="text" id="tossWinner" placeholder="Toss Jeetne Wali Team">
            <input type="text" id="matchWinner" placeholder="Match Winner Team">
            <input type="text" id="team1ScoreDetails" placeholder="Team 1 Score (jaise: 346/4 in 20 ov)">
            <input type="text" id="team2ScoreDetails" placeholder="Team 2 Score (jaise: 340/6 in 20 ov)">
            <button class="action-btn" onclick="updateMatchResult()" style="background: #0284c7;">Result & Points Table Update Karein</button>
            
            <button onclick="closeAdminPanel()" class="logout-btn" style="background: #757575;">Admin Panel Band Karein</button>
        </div>

        <button class="logout-btn" onclick="logoutUser()">Logout</button>
    </div>
</div>

<script>
    const ADMIN_NUMBER = "9569981484";
    let generatedOTP = "";
    let tempPhone = "";

    function getUsers() { return JSON.parse(localStorage.getItem('app_users')) || {}; }
    function saveUsers(users) { localStorage.setItem('app_users', JSON.stringify(users)); }
    
    function getMatches() { return JSON.parse(localStorage.getItem('app_matches')) || []; }
    function saveMatches(matches) { localStorage.setItem('app_matches', JSON.stringify(matches)); }

    function getPointsTable() { return JSON.parse(localStorage.getItem('app_points_table')) || {}; }
    function savePointsTable(table) { localStorage.setItem('app_points_table', JSON.stringify(table)); }

    function getGroups() { return JSON.parse(localStorage.getItem('app_groups')) || {}; }
    function saveGroups(groups) { localStorage.setItem('app_groups', JSON.stringify(groups)); }

    function sendOTP() {
        let phone = document.getElementById('userPhoneInput').value.trim();
        if(phone.length !== 10) {
            alert("Kripya sahi 10-digit ka mobile number dalein!");
            return;
        }

        tempPhone = phone;
        // Generate random 4 digit OTP
        generatedOTP = Math.floor(1000 + Math.random() * 9000).toString();
        
        // Show OTP in alert for testing/simulation purpose
        alert("Aapka OTP hai: " + generatedOTP);

        document.getElementById('phoneStep').classList.add('hidden');
        document.getElementById('otpStep').classList.remove('hidden');
    }

    function verifyOTP() {
        let enteredOTP = document.getElementById('otpInput').value.trim();
        if(enteredOTP !== generatedOTP) {
            alert("Galat OTP! Kripya sahi OTP dalein.");
            return;
        }

        let users = getUsers();
        if(!users[tempPhone]) {
            let initialPoints = (tempPhone === ADMIN_NUMBER ? 50000 : 260);
            users[tempPhone] = { points: initialPoints, subscription: null, subExpiry: 0 };
            saveUsers(users);
        }

        localStorage.setItem('current_user', tempPhone);
        loadDashboard();
    }

    function resetLogin() {
        document.getElementById('phoneStep').classList.remove('hidden');
        document.getElementById('otpStep').classList.add('hidden');
        document.getElementById('otpInput').value = "";
    }

    function loadDashboard() {
        let currentPhone = localStorage.getItem('current_user');
        if(!currentPhone) return;

        checkSubscriptionExpiry();

        document.getElementById('loginSection').classList.add('hidden');
        document.getElementById('dashboardSection').classList.remove('hidden');
        document.getElementById('appTabs').classList.remove('hidden');

        let users = getUsers();
        let userData = users[currentPhone];

        document.getElementById('userPoints').innerText = userData.points;

        if(userData.subscription) {
            document.getElementById('subStatus').innerHTML = "Active Plan: <span style='color:#009688;'>" + userData.subscription + "</span>";
        } else {
            document.getElementById('subStatus').innerHTML = "<span style='color:#d32f2f;'>No Active Subscription</span>";
        }

        renderSchedule();
        renderPointsTable();
        renderGroups();
        updateMatchDropdown();
    }

    function switchTab(tabName) {
        document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
        document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
        document.getElementById('adminPanelSection').classList.add('hidden');

        if(tabName === 'matches') {
            document.getElementById('tabMatches').classList.remove('hidden');
            event.target.classList.add('active');
        } else if(tabName === 'points') {
            document.getElementById('tabPoints').classList.remove('hidden');
            event.target.classList.add('active');
        } else if(tabName === 'groups') {
            document.getElementById('tabGroups').classList.remove('hidden');
            event.target.classList.add('active');
        } else if(tabName === 'wallet') {
            document.getElementById('tabWallet').classList.remove('hidden');
            event.target.classList.add('active');
        }
    }

    function checkAdminAccess() {
        let currentPhone = localStorage.getItem('current_user');
        if(!currentPhone) {
            alert("Pehle login karein!");
            return;
        }

        let users = getUsers();
        let userData = users[currentPhone];

        let hasActiveSub = userData.subscription && (userData.subExpiry > Date.now() || userData.subscription !== '30 Minutes Plan');
        
        if(!hasActiveSub && currentPhone !== ADMIN_NUMBER) {
            alert("Admin Mode / Panel kholne ke liye pehle koi active subscription (jaise 30 Mins Plan ya Monthly Plan) lena anivarya hai!");
            switchTab('wallet');
            return;
        }

        let pwd = prompt("Admin Password Dalein (password: admin):");
        if(pwd === "admin") {
            document.getElementById('adminPanelSection').classList.remove('hidden');
            if(currentPhone === ADMIN_NUMBER) {
                document.getElementById('adminSendPointsBox').classList.remove('hidden');
            } else {
                document.getElementById('adminSendPointsBox').classList.add('hidden');
            }
            alert("Admin Mode Successfully Unlocked!");
        } else if(pwd !== null) {
            alert("Galat password!");
        }
    }

    function closeAdminPanel() {
        document.getElementById('adminPanelSection').classList.add('hidden');
    }

    function buySubscription(cost, planName) {
        let currentPhone = localStorage.getItem('current_user');
        let users = getUsers();

        if(users[currentPhone].points < cost) {
            alert("Aapke paas subscription lene ke liye pure points nahi hain!");
            return;
        }

        users[currentPhone].points -= cost;
        
        if(!users[ADMIN_NUMBER]) users[ADMIN_NUMBER] = { points: 0, subscription: null };
        users[ADMIN_NUMBER].points += cost;

        users[currentPhone].subscription = planName;
        if(planName === '30 Minutes Plan') {
            users[currentPhone].subExpiry = Date.now() + (30 * 60 * 1000);
        } else {
            users[currentPhone].subExpiry = Date.now() + (30 * 24 * 60 * 60 * 1000);
        }

        saveUsers(users);
        loadDashboard();
        alert(planName + " successfully activated! Points kat kar admin ke paas chale gaye.");
    }

    function checkSubscriptionExpiry() {
        let currentPhone = localStorage.getItem('current_user');
        let users = getUsers();
        let userData = users[currentPhone];
        if(userData && userData.subscription === '30 Minutes Plan' && userData.subExpiry < Date.now()) {
            userData.subscription = null;
            userData.subExpiry = 0;
            saveUsers(users);
        }
    }

    function adminTransferPoints() {
        let currentPhone = localStorage.getItem('current_user');
        if(currentPhone !== ADMIN_NUMBER) {
            alert("Ye feature sirf main admin ke liye hai!");
            return;
        }

        let targetPhone = document.getElementById('targetUserPhone').value.trim();
        let amount = parseInt(document.getElementById('transferPointsAmount').value);

        let users = getUsers();
        if(!users[targetPhone]) {
            alert("Ye user mobile number database mein nahi hai (usne login nahi kiya hai)!");
            return;
        }

        if(isNaN(amount) || amount <= 0) {
            alert("Sahi points amount dalein!");
            return;
        }

        users[targetPhone].points += amount;
        saveUsers(users);
        alert(amount + " points successfully added to " + targetPhone + "!");
        document.getElementById('targetUserPhone').value = "";
        document.getElementById('transferPointsAmount').value = "";
    }

    function addNewMatch() {
        let series = document.getElementById('seriesName').value.trim();
        let format = document.getElementById('matchFormat').value.trim();
        let t1 = document.getElementById('team1').value.trim();
        let t2 = document.getElementById('team2').value.trim();
        let venue = document.getElementById('venue').value.trim();
        let time = document.getElementById('matchTime').value.trim();

        if(!series || !format || !t1 || !t2) {
            alert("Sabhi zaroori fields bharein!");
            return;
        }

        let matches = getMatches();
        matches.push({ id: Date.now(), series, format, t1, t2, venue, time, toss: "Yet to happen", winner: "Upcoming", t1Score: "", t2Score: "" });
        saveMatches(matches);

        alert("Match successfully schedule ho gaya!");
        document.getElementById('seriesName').value = "";
        document.getElementById('matchFormat').value = "";
        document.getElementById('team1').value = "";
        document.getElementById('team2').value = "";
        document.getElementById('venue').value = "";
        document.getElementById('matchTime').value = "";

        loadDashboard();
    }

    function renderSchedule() {
        let matches = getMatches();
        let container = document.getElementById('scheduleList');
        if(matches.length === 0) {
            container.innerHTML = "<p style='font-size:13px; color:var(--text-muted); text-align:center;'>Abhi koi match schedule nahi hai.</p>";
            return;
        }

        let html = "";
        let currentSeries = "";

        matches.forEach((m) => {
            if(m.series !== currentSeries) {
                currentSeries = m.series;
                html += `<div class="series-title">📌 ${currentSeries}</div>`;
            }
            html += `<div class="match-box">
                <div class="match-info-top">
                    <span>${m.format}</span>
                    <span>📍 ${m.venue} | ⏰ ${m.time}</span>
                </div>
                <div class="match-teams">
                    <span>🏏 ${m.t1} <span style="font-size:12px; color:#555;">${m.t1Score}</span></span>
                    <span>vs</span>
                    <span>${m.t2} 🏏 <span style="font-size:12px; color:#555;">${m.t2Score}</span></span>
                </div>`;
            if(m.winner !== "Upcoming") {
                html += `<div class="match-result-banner">🏆 ${m.winner}</div>`;
            }
            html += `</div>`;
        });
        container.innerHTML = html;
    }

    function updateMatchDropdown() {
        let matches = getMatches();
        let select = document.getElementById('matchSelectForUpdate');
        select.innerHTML = "<option value=''>Match Chunein update karne ke liye</option>";
        matches.forEach((m) => {
            select.innerHTML += `<option value="${m.id}">${m.series} - ${m.t1} vs ${m.t2} (${m.format})</option>`;
        });
    }

    function updateMatchResult() {
        let matchId = document.getElementById('matchSelectForUpdate').value;
        let toss = document.getElementById('tossWinner').value.trim();
        let winner = document.getElementById('matchWinner').value.trim();
        let t1Score = document.getElementById('team1ScoreDetails').value.trim();
        let t2Score = document.getElementById('team2ScoreDetails').value.trim();

        if(!matchId) {
            alert("Kripya match chunein!");
            return;
        }

        let matches = getMatches();
        let match = matches.find(m => m.id == matchId);
        if(match) {
            match.toss = toss || match.toss;
            match.winner = winner || match.winner;
            match.t1Score = t1Score || match.t1Score;
            match.t2Score = t2Score || match.t2Score;
            saveMatches(matches);

            let table = getPointsTable();
            [match.t1, match.t2].forEach(team => {
                if(!table[team]) table[team] = { played: 0, won: 0, lost: 0, points: 0, nrr: '0.000' };
            });

            table[match.t1].played += 1;
            table[match.t2].played += 1;

            if(winner.toLowerCase() === match.t1.toLowerCase()) {
                table[match.t1].won += 1;
                table[match.t1].points += 2;
                table[match.t2].lost += 1;
            } else if(winner.toLowerCase() === match.t2.toLowerCase()) {
                table[match.t2].won += 1;
                table[match.t2].points += 2;
                table[match.t1].lost += 1;
            }

            savePointsTable(table);
            alert("Result aur Points Table update ho gayi!");
            loadDashboard();
        }
    }

    function renderPointsTable() {
        let table = getPointsTable();
        let container = document.getElementById('pointsTableContainer');
        let teams = Object.keys(table);

        if(teams.length === 0) {
            container.innerHTML = "<p style='font-size:13px; color:var(--text-muted); text-align:center;'>Points table khali hai.</p>";
            return;
        }

        teams.sort((a, b) => table[b].points - table[a].points);

        let html = `<table><tr><th>Team</th><th>P</th><th>W</th><th>L</th><th>Pts</th><th>NRR</th></tr>`;
        teams.forEach(t => {
            let d = table[t];
            html += `<tr><td><b>${t}</b></td><td>${d.played}</td><td>${d.won}</td><td>${d.lost}</td><td><b>${d.points}</b></td><td>${d.nrr || '0.000'}</td></tr>`;
        });
        html += `</table>`;
        container.innerHTML = html;
    }

    function createGroup() {
        let gName = document.getElementById('groupNameInput').value.trim();
        let teamsInput = document.getElementById('groupTeamsInput').value.trim();

        if(!gName || !teamsInput) {
            alert("Group name aur teams dalein!");
            return;
        }

        let groups = getGroups();
        let teamsArr = teamsInput.split(',').map(t => t.trim());
        groups[gName] = teamsArr;
        saveGroups(groups);

        alert("Group successfully ban gaya!");
        document.getElementById('groupNameInput').value = "";
        document.getElementById('groupTeamsInput').value = "";
        renderGroups();
    }

    function renderGroups() {
        let groups = getGroups();
        let container = document.getElementById('groupsContainer');
        let keys = Object.keys(groups);

        if(keys.length === 0) {
            container.innerHTML = "<p style='font-size:13px; color:var(--text-muted);'>Abhi koi group nahi banaya gaya hai.</p>";
            return;
        }

        let html = "";
        keys.forEach(g => {
            html += `<div style="background:#f9f9f9; border:1px solid #ddd; padding:8px; border-radius:6px; margin-bottom:8px;">
                <strong>📌 ${g}</strong><br><span style="font-size:13px; color:#555;">Teams: ${groups[g].join(', ')}</span>
            </div>`;
        });
        container.innerHTML = html;
    }

    function logoutUser() {
        localStorage.removeItem('current_user');
        document.getElementById('dashboardSection').classList.add('hidden');
        document.getElementById('appTabs').classList.add('hidden');
        document.getElementById('loginSection').classList.remove('hidden');
        document.getElementById('userPhoneInput').value = "";
        resetLogin();
    }

    window.onload = function() {
        if(localStorage.getItem('current_user')) {
            loadDashboard();
        }
    }
</script>

</body>
</html>
