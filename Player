<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>羽球雙打自動排班組合器 v5.0</title>
    
    <!-- PWA 手機 App 設定 -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <meta name="apple-mobile-web-app-title" content="羽球排班">
    <link rel="apple-touch-icon" href="https://cdn-icons-png.flaticon.com/512/3277/3277341.png">
    
    <style>
        :root {
            --primary: #2c3e50;
            --accent: #27ae60;
            --bg: #f4f6f9;
            --card: #ffffff;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg);
            color: var(--primary);
            margin: 0;
            padding: 15px;
            -webkit-user-select: none; /* 防止手機長按誤選文字 */
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
        }
        h1 {
            text-align: center;
            font-size: 1.5rem;
            margin-bottom: 15px;
        }
        .grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 15px;
            margin-bottom: 15px;
        }
        .card {
            background: var(--card);
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.06);
        }
        .form-group {
            margin-bottom: 12px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            font-size: 0.9rem;
        }
        input[type="text"], select {
            width: 100%;
            padding: 10px;
            box-sizing: border-box;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem; /* 防止 iOS 放大 */
        }
        .gender-radio {
            margin-top: 5px;
            display: flex;
            gap: 20px;
        }
        .gender-radio label {
            display: inline-flex;
            align-items: center;
            gap: 5px;
            font-weight: normal;
            cursor: pointer;
        }
        button {
            background-color: var(--accent);
            color: white;
            border: none;
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1rem;
            width: 100%;
            font-weight: bold;
        }
        .btn-delete {
            background-color: #e74c3c;
            padding: 6px 12px;
            width: auto;
            font-size: 0.85rem;
        }
        .player-list {
            margin-top: 15px;
            max-height: 200px;
            overflow-y: auto;
            border: 1px solid #eee;
            border-radius: 8px;
            padding: 5px;
        }
        .player-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 10px;
            background: #f8f9fa;
            margin-bottom: 5px;
            border-radius: 6px;
        }
        .tag {
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 11px;
            color: white;
            margin-left: 5px;
        }
        .tag-m { background-color: #3498db; }
        .tag-f { background-color: #e84393; }
        
        /* 響應式表格優化，適合手機閱讀 */
        .table-container {
            overflow-x: auto;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
            background: white;
            font-size: 0.9rem;
        }
        th, td {
            border: 1px solid #eee;
            padding: 10px 6px;
            text-align: center;
        }
        th { background-color: #f8f9fa; }
        .rest-list { color: #7f8c8d; font-size: 0.8rem; }
        .alert { color: #e74c3c; font-weight: bold; text-align: center; margin-bottom: 10px; }
        
        .match-type-tag {
            display: inline-block;
            padding: 2px 6px;
            border-radius: 4px;
            font-size: 10px;
            font-weight: bold;
            color: #fff;
            margin-top: 4px;
        }
        .type-mm { background-color: #2980b9; }
        .type-ff { background-color: #e84393; }
        .type-mf { background-color: #8e44ad; }
        .type-rand { background-color: #7f8c8d; }
    </style>
</head>
<body>

<div class="container">
    <h1>🏸 羽球雙打自動排班組合器</h1>
    
    <div class="grid">
        <div class="card">
            <h3>👥 班底名單 (自動儲存)</h3>
            <div class="form-group">
                <input type="text" id="playerName" placeholder="輸入姓名後按新增">
            </div>
            <div class="form-group gender-radio">
                <label><input type="radio" id="genderM" name="gender" value="男" checked> 男 ♂</label>
                <label><input type="radio" id="genderF" name="gender" value="女"> 女 ♀</label>
            </div>
            <button onclick="addPlayer()">新增球員</button>
            <div class="player-list" id="playerList"></div>
        </div>

        <div class="card">
            <h3>⚙️ 參數設定</h3>
            <div class="form-group">
                <label for="duration">打球總時間</label>
                <select id="duration" onchange="calculateRotation()">
                    <option value="1">1 小時 (6 場)</option>
                    <option value="1.5">1.5 小時 (9 場)</option>
                    <option value="2" selected>2 小時 (12 場)</option>
                    <option value="2.5">2.5 小時 (15 場)</option>
                    <option value="3">3 小時 (18 場)</option>
                </select>
            </div>
            <div class="form-group">
                <label for="matchMode">組合模式選取</label>
                <select id="matchMode" onchange="calculateRotation()">
                    <option value="random">完全隨機組合 (場次最平均)</option>
                    <option value="mixed_clean">智慧雙打組合 (男雙/女雙/混雙)</option>
                </select>
            </div>
            <button style="background-color: #2c3e50;" onclick="calculateRotation()">🔄 重新生成對戰表</button>
        </div>
    </div>

    <div class="card">
        <h3>📊 個人場次統計</h3>
        <div class="table-container" id="summarySection"></div>
    </div>

    <div class="card">
        <h3>📅 全部對戰場次</h3>
        <div id="alertMsg" class="alert"></div>
        <div class="table-container" id="matchSection"></div>
    </div>
</div>

<script>
    let players = [];
    let playerIdCounter = 1;

    function loadPlayersFromStorage() {
        const savedPlayers = localStorage.getItem('badminton_players');
        if (savedPlayers) {
            players = JSON.parse(savedPlayers);
            if (players.length > 0) {
                playerIdCounter = Math.max(...players.map(p => p.id)) + 1;
            }
        } else {
            players = [
                { id: 1, name: "阿豪", gender: "男", count: 0, consecutivePlays: 0, consecutiveRests: 0 },
                { id: 2, name: "小明", gender: "男", count: 0, consecutivePlays: 0, consecutiveRests: 0 },
                { id: 3, name: "志強", gender: "男", count: 0, consecutivePlays: 0, consecutiveRests: 0 },
                { id: 4, name: "婷婷", gender: "女", count: 0, consecutivePlays: 0, consecutiveRests: 0 },
                { id: 5, name: "雅婷", gender: "女", count: 0, consecutivePlays: 0, consecutiveRests: 0 }
            ];
            playerIdCounter = 6;
            savePlayersToStorage();
        }
    }

    function savePlayersToStorage() {
        localStorage.setItem('badminton_players', JSON.stringify(players));
    }

    document.getElementById('playerName').addEventListener('keypress', function(e) {
        if (e.key === 'Enter') addPlayer();
    });

    loadPlayersFromStorage();
    updatePlayerList();
    calculateRotation();

    function addPlayer() {
        const nameInput = document.getElementById('playerName');
        const name = nameInput.value.trim();
        if (!name) return;
        const gender = document.querySelector('input[name="gender"]:checked').value;
        
        players.push({ id: playerIdCounter++, name: name, gender: gender, count: 0, consecutivePlays: 0, consecutiveRests: 0 });
        nameInput.value = '';
        savePlayersToStorage();
        updatePlayerList();
        calculateRotation();
    }

    function deletePlayer(id) {
        players = players.filter(p => p.id !== id);
        savePlayersToStorage();
        updatePlayerList();
        calculateRotation();
    }

    function updatePlayerList() {
        const listDiv = document.getElementById('playerList');
        listDiv.innerHTML = '';
        players.forEach(p => {
            const genTag = p.gender === '男' ? '<span class="tag tag-m">男</span>' : '<span class="tag tag-f">女</span>';
            listDiv.innerHTML += `
                <div class="player-item">
                    <div><strong>${p.name}</strong>${genTag}</div>
                    <button class="btn-delete" onclick="deletePlayer(${p.id})">刪除</button>
                </div>
            `;
        });
    }

    function getPlayerScore(p) {
        let score = p.count * 10000; 
        if (p.consecutivePlays >= 2) score += 3000 * p.consecutivePlays; 
        if (p.consecutiveRests > 0) score -= (p.consecutiveRests * 2500); 
        score += Math.random();
        return score;
    }

    function calculateRotation() {
        const alertMsg = document.getElementById('alertMsg');
        const summarySection = document.getElementById('summarySection');
        const matchSection = document.getElementById('matchSection');

        if (players.length < 4) {
            alertMsg.innerText = "⚠️ 人數不足 4 人！";
            summarySection.innerHTML = '';
            matchSection.innerHTML = '';
            return;
        } else {
            alertMsg.innerText = "";
        }

        players.forEach(p => { p.count = 0; p.consecutivePlays = 0; p.consecutiveRests = 0; });
        const hours = parseFloat(document.getElementById('duration').value);
        const mode = document.getElementById('matchMode').value;
        const totalMatches = Math.round(hours * 60 / 10);
        let schedule = [];

        for (let m = 1; m <= totalMatches; m++) {
            let sortedPlayers = [...players].sort((a, b) => getPlayerScore(a) - getPlayerScore(b));
            let teamA = [], teamB = [], resting = [];
            let matchTypeLabel = "隨機組合";
            let matchTypeClass = "type-rand";

            if (mode === 'random') {
                let onCourt = sortedPlayers.slice(0, 4);
                teamA = [onCourt[0], onCourt[1]];
                teamB = [onCourt[2], onCourt[3]];
            } else {
                let candidates = sortedPlayers.slice(0, Math.min(6, sortedPlayers.length));
                let cMen = candidates.filter(p => p.gender === '男');
                let cWomen = candidates.filter(p => p.gender === '女');

                if (cMen.length >= 2 && cWomen.length >= 2) {
                    teamA = [cMen[0], cWomen[0]]; teamB = [cMen[1], cWomen[1]];
                    matchTypeLabel = "混雙"; matchTypeClass = "type-mf";
                } else if (cMen.length >= 4) {
                    teamA = [cMen[0], cMen[1]]; teamB = [cMen[2], cMen[3]];
                    matchTypeLabel = "男雙"; matchTypeClass = "type-mm";
                } else if (cWomen.length >= 4) {
                    teamA = [cWomen[0], cWomen[1]]; teamB = [cWomen[2], cWomen[3]];
                    matchTypeLabel = "女雙"; matchTypeClass = "type-ff";
                } else {
                    let onCourt = sortedPlayers.slice(0, 4);
                    teamA = [onCourt[0], onCourt[1]]; teamB = [onCourt[2], onCourt[3]];
                    matchTypeLabel = "常規雙打"; matchTypeClass = "type-rand";
                }
            }

            let activeIds = [...teamA, ...teamB].map(x => x.id);
            players.forEach(p => {
                if (activeIds.includes(p.id)) {
                    p.count++; p.consecutivePlays++; p.consecutiveRests = 0;
                } else {
                    p.consecutivePlays = 0; p.consecutiveRests++; resting.push(p);
                }
            });

            schedule.push({ matchNum: m, teamA: teamA, teamB: teamB, resting: resting, label: matchTypeLabel, cssClass: matchTypeClass });
        }

        let summaryHTML = `<table><tr><th>球員</th><th>場次</th></tr>`;
        [...players].sort((a,b) => b.count - a.count).forEach(p => {
            summaryHTML += `<tr><td>${p.name} (${p.gender})</td><td><strong>${p.count} 場</strong></td></tr>`;
        });
        summaryHTML += `</table>`;
        summarySection.innerHTML = summaryHTML;

        let matchHTML = `<table><tr><th>場次</th><th>藍方</th><th>紅方</th><th>休息</th></tr>`;
        schedule.forEach(s => {
            matchHTML += `
                <tr>
                    <td><strong>${s.matchNum}</strong><br><span class="match-type-tag ${s.cssClass}">${s.label}</span></td>
                    <td style="color: #2980b9;">${s.teamA[0].name}<br>${s.teamA[1].name}</td>
                    <td style="color: #c0392b;">${s.teamB[0].name}<br>${s.teamB[1].name}</td>
                    <td class="rest-list">${s.resting.map(x => x.name).join('<br>') || '無'}</td>
                </tr>`;
        });
        matchHTML += `</table>`;
        matchSection.innerHTML = matchHTML;
    }
</script>
</body>
</html>
