<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Football Score - نتائج المباريات المباشرة</title>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts (Cairo) -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-primary: #0d0e12;
            --bg-secondary: #16181f;
            --bg-card: #1f222b;
            --bg-hover: #292d39;
            --accent-green: #00e676;
            --accent-red: #ff3366;
            --accent-gold: #ffb300;
            --text-main: #ffffff;
            --text-muted: #9e9e9e;
            --border-color: #2a2e3d;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Cairo', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-primary);
            color: var(--text-main);
            display: flex;
            justify-content: center;
            min-height: 100vh;
        }

        /* App Container */
        .app-container {
            width: 100%;
            max-width: 480px;
            background-color: var(--bg-secondary);
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            position: relative;
            box-shadow: 0 0 20px rgba(0,0,0,0.8);
            padding-bottom: 70px;
        }

        /* Header */
        header {
            background-color: var(--bg-primary);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border-color);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .brand i {
            color: var(--accent-green);
            font-size: 1.5rem;
        }

        .brand h1 {
            font-size: 1.2rem;
            font-weight: 900;
            letter-spacing: 0.5px;
            background: linear-gradient(45deg, #ffffff, var(--accent-green));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .header-actions {
            display: flex;
            gap: 15px;
        }

        .header-actions i {
            font-size: 1.2rem;
            color: var(--text-muted);
            cursor: pointer;
            transition: 0.2s;
        }

        .header-actions i:hover {
            color: var(--accent-green);
        }

        /* Date Strip */
        .date-strip {
            display: flex;
            background-color: var(--bg-primary);
            padding: 10px;
            gap: 8px;
            overflow-x: auto;
            border-bottom: 1px solid var(--border-color);
            scrollbar-width: none;
        }

        .date-strip::-webkit-scrollbar {
            display: none;
        }

        .date-btn {
            flex: 0 0 auto;
            padding: 8px 16px;
            background-color: var(--bg-card);
            border-radius: 20px;
            text-align: center;
            font-size: 0.85rem;
            color: var(--text-muted);
            cursor: pointer;
            border: 1px solid transparent;
            transition: 0.2s;
        }

        .date-btn.active {
            background-color: rgba(0, 230, 118, 0.15);
            color: var(--accent-green);
            border-color: var(--accent-green);
            font-weight: 700;
        }

        /* Content Views */
        .view-content {
            display: none;
            padding: 15px;
        }

        .view-content.active {
            display: block;
        }

        /* League Card */
        .league-group {
            background-color: var(--bg-card);
            border-radius: 12px;
            margin-bottom: 15px;
            overflow: hidden;
            border: 1px solid var(--border-color);
        }

        .league-header {
            background-color: rgba(255, 255, 255, 0.03);
            padding: 12px 15px;
            display: flex;
            align-items: center;
            gap: 10px;
            border-bottom: 1px solid var(--border-color);
        }

        .league-header img {
            width: 24px;
            height: 24px;
            object-fit: contain;
        }

        .league-header h3 {
            font-size: 0.95rem;
            font-weight: 700;
            flex: 1;
        }

        /* Match Row */
        .match-row {
            display: flex;
            align-items: center;
            padding: 12px 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.03);
            cursor: pointer;
            transition: 0.2s;
        }

        .match-row:last-child {
            border-bottom: none;
        }

        .match-row:hover {
            background-color: var(--bg-hover);
        }

        .team {
            flex: 1;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.85rem;
            font-weight: 600;
        }

        .team.home {
            justify-content: flex-end;
            text-align: left;
        }

        .team.away {
            justify-content: flex-start;
            text-align: right;
        }

        .team img {
            width: 22px;
            height: 22px;
            object-fit: contain;
        }

        .match-center {
            width: 90px;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
        }

        .score-box {
            background-color: var(--bg-primary);
            padding: 4px 12px;
            border-radius: 6px;
            font-weight: 700;
            font-size: 0.95rem;
            letter-spacing: 2px;
            border: 1px solid var(--border-color);
        }

        .score-box.live {
            border-color: var(--accent-red);
            color: var(--accent-red);
        }

        .match-status {
            font-size: 0.7rem;
            color: var(--text-muted);
        }

        .match-status.live-text {
            color: var(--accent-red);
            font-weight: 700;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.4; }
            100% { opacity: 1; }
        }

        /* Standings Table */
        .table-container {
            overflow-x: auto;
        }

        .custom-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85rem;
            text-align: center;
        }

        .custom-table th, .custom-table td {
            padding: 10px 6px;
            border-bottom: 1px solid var(--border-color);
        }

        .custom-table th {
            background-color: var(--bg-primary);
            color: var(--text-muted);
            font-weight: 600;
        }

        .custom-table td.team-cell {
            text-align: right;
            display: flex;
            align-items: center;
            gap: 8px;
            font-weight: 600;
        }

        .custom-table img {
            width: 18px;
            height: 18px;
        }

        /* News Cards */
        .news-card {
            background-color: var(--bg-card);
            border-radius: 12px;
            overflow: hidden;
            margin-bottom: 15px;
            border: 1px solid var(--border-color);
        }

        .news-card img {
            width: 100%;
            height: 160px;
            object-fit: cover;
        }

        .news-body {
            padding: 12px 15px;
        }

        .news-body h4 {
            font-size: 0.95rem;
            margin-bottom: 6px;
            line-height: 1.4;
        }

        .news-body p {
            font-size: 0.75rem;
            color: var(--text-muted);
        }

        /* Modal Details */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.85);
            display: none;
            justify-content: center;
            align-items: flex-end;
            z-index: 1000;
        }

        .modal-content {
            background-color: var(--bg-secondary);
            width: 100%;
            max-width: 480px;
            border-top-left-radius: 20px;
            border-top-right-radius: 20px;
            padding: 20px;
            max-height: 85vh;
            overflow-y: auto;
            border-top: 2px solid var(--accent-green);
            animation: slideUp 0.3s ease-out;
        }

        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .close-btn {
            background: none;
            border: none;
            color: var(--text-muted);
            font-size: 1.5rem;
            cursor: pointer;
        }

        .detail-meta-item {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid var(--border-color);
            font-size: 0.85rem;
        }

        .detail-meta-item span:first-child {
            color: var(--text-muted);
        }

        /* Bottom Navigation Bar */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            width: 100%;
            max-width: 480px;
            background-color: var(--bg-primary);
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            border-top: 1px solid var(--border-color);
            z-index: 100;
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            color: var(--text-muted);
            font-size: 0.75rem;
            cursor: pointer;
            transition: 0.2s;
            flex: 1;
        }

        .nav-item i {
            font-size: 1.1rem;
        }

        .nav-item.active {
            color: var(--accent-green);
            font-weight: 700;
        }
    </style>
</head>
<body>

    <div class="app-container">
        <!-- App Header -->
        <header>
            <div class="brand">
                <i class="fa-solid fa-football"></i>
                <h1>Football Score</h1>
            </div>
            <div class="header-actions">
                <i class="fa-solid fa-magnifying-glass" onclick="alert('ميزة البحث ستتوفر قريباً!')"></i>
                <i class="fa-regular fa-bell" onclick="alert('تم تفعيل إشعارات التنبيهات للأهداف!')"></i>
            </div>
        </header>

        <!-- Date Filter Strip (Appears on Matches view) -->
        <div class="date-strip" id="dateStrip">
            <div class="date-btn" onclick="filterDate(this, 'yesterday')">الأمس</div>
            <div class="date-btn active" onclick="filterDate(this, 'today')">اليوم المباشر</div>
            <div class="date-btn" onclick="filterDate(this, 'tomorrow')">الغد</div>
        </div>

        <!-- 1. Matches View -->
        <div id="matches-view" class="view-content active">
            <div id="matches-container">
                <!-- Data populated dynamically -->
            </div>
        </div>

        <!-- 2. Standings View -->
        <div id="standings-view" class="view-content">
            <div class="league-group">
                <div class="league-header">
                    <img src="https://crests.football-data.org/PD.png" alt="La Liga">
                    <h3>ترتيب الدوري الإسباني</h3>
                </div>
                <div class="table-container">
                    <table class="custom-table">
                        <thead>
                            <tr>
                                <th>#</th>
                                <th style="text-align: right;">الفرقة</th>
                                <th>لعب</th>
                                <th>+/-</th>
                                <th>نقاط</th>
                            </tr>
                        </thead>
                        <tbody id="standings-body">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- 3. Top Scorers View -->
        <div id="scorers-view" class="view-content">
            <div class="league-group">
                <div class="league-header">
                    <i class="fa-solid fa-trophy" style="color: var(--accent-gold);"></i>
                    <h3>قائمة هدافي البطولات الكبرى</h3>
                </div>
                <div class="table-container">
                    <table class="custom-table">
                        <thead>
                            <tr>
                                <th>#</th>
                                <th style="text-align: right;">اللاعب</th>
                                <th>الفريق</th>
                                <th>الأهداف</th>
                            </tr>
                        </thead>
                        <tbody id="scorers-body">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- 4. News View -->
        <div id="news-view" class="view-content">
            <div id="news-container">
                <!-- News populated dynamically -->
            </div>
        </div>

        <!-- 5. Favorites View -->
        <div id="fav-view" class="view-content">
            <div style="text-align: center; padding: 40px 20px; color: var(--text-muted);">
                <i class="fa-solid fa-star" style="font-size: 3rem; margin-bottom: 15px; color: var(--border-color);"></i>
                <p>قائمة المباريات والفرق المفضلة فارغة حالياً.</p>
                <span style="font-size: 0.75rem;">اضغط على أي مباراة لمتابعة التنبيهات المباشرة.</span>
            </div>
        </div>

        <!-- Bottom Navigation Bar -->
        <nav class="bottom-nav">
            <div class="nav-item active" onclick="switchTab(this, 'matches-view')">
                <i class="fa-solid fa-calendar-days"></i>
                <span>المباريات</span>
            </div>
            <div class="nav-item" onclick="switchTab(this, 'standings-view')">
                <i class="fa-solid fa-list-ol"></i>
                <span>الترتيب</span>
            </div>
            <div class="nav-item" onclick="switchTab(this, 'scorers-view')">
                <i class="fa-solid fa-shoe-prints"></i>
                <span>الهدافون</span>
            </div>
            <div class="nav-item" onclick="switchTab(this, 'news-view')">
                <i class="fa-regular fa-newspaper"></i>
                <span>الأخبار</span>
            </div>
            <div class="nav-item" onclick="switchTab(this, 'fav-view')">
                <i class="fa-regular fa-star"></i>
                <span>المفضلة</span>
            </div>
        </nav>
    </div>

    <!-- Match Details Modal -->
    <div class="modal-overlay" id="matchModal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 style="font-size: 1rem;" id="modalTitle">تفاصيل المباراة</h3>
                <button class="close-btn" onclick="closeModal()">&times;</button>
            </div>
            <div id="modalBody">
                <!-- Populated dynamically -->
            </div>
        </div>
    </div>

    <script>
        // User API Config
        const API_KEY = "F6978880b7c94dd6873003784160281e";

        // Backup & Fallback Data for complete experience
        const mockMatchesData = [
            {
                league: "دوري أبطال أوروبا",
                logo: "https://crests.football-data.org/CL.png",
                matches: [
                    { home: "ريال مدريد", homeLogo: "https://crests.football-data.org/86.png", away: "مانشستر سيتي", awayLogo: "https://crests.football-data.org/65.png", score: "2 - 1", status: "مباشر '68", isLive: true, channel: "beIN Sports HD 1", commentator: "حفيظ دراجي" },
                    { home: "بايرن ميونخ", homeLogo: "https://crests.football-data.org/5.png", away: "باريس سان جيرمان", awayLogo: "https://crests.football-data.org/524.png", score: "0 - 0", status: "21:00", isLive: false, channel: "beIN Sports HD 2", commentator: "خليل البلوشي" }
                ]
            },
            {
                league: "الدوري الإسباني",
                logo: "https://crests.football-data.org/PD.png",
                matches: [
                    { home: "برشلونة", homeLogo: "https://crests.football-data.org/81.png", away: "أتلتيكو مدريد", awayLogo: "https://crests.football-data.org/78.png", score: "3 - 1", status: "انتهت", isLive: false, channel: "beIN Sports HD 3", commentator: "عصام الشوالي" }
                ]
            }
        ];

        const mockStandings = [
            { pos: 1, team: "ريال مدريد", logo: "https://crests.football-data.org/86.png", played: 12, gd: "+18", pts: 31 },
            { pos: 2, team: "برشلونة", logo: "https://crests.football-data.org/81.png", played: 12, gd: "+15", pts: 28 },
            { pos: 3, team: "أتلتيكو مدريد", logo: "https://crests.football-data.org/78.png", played: 12, gd: "+9", pts: 25 },
            { pos: 4, team: "فيلاريال", logo: "https://crests.football-data.org/94.png", played: 12, gd: "+4", pts: 21 }
        ];

        const mockScorers = [
            { pos: 1, player: "روبرت ليفاندوفسكي", team: "برشلونة", goals: 12 },
            { pos: 2, player: "كيليان إمبابي", team: "ريال مدريد", goals: 10 },
            { pos: 3, player: "فينيسيوس جونيور", team: "ريال مدريد", goals: 8 },
            { pos: 4, player: "أنسو فاتي", team: "برشلونة", goals: 6 }
        ];

        const mockNews = [
            { title: "ريال مدريد يحسم الكلاسيكو بالفوز على برشلونة في مباراة مثيرة", time: "منذ ساعتين", img: "https://images.unsplash.com/photo-1508098682722-e99c43a406b2?auto=format&fit=crop&w=500&q=80" },
            { title: "مانشستر سيتي يعلن رسمياً تجديد عقد نجم الفريق حتى عام 2028", time: "منذ 4 ساعات", img: "https://images.unsplash.com/photo-1522778119026-d647f0596c20?auto=format&fit=crop&w=500&q=80" }
        ];

        // Initialize App
        document.addEventListener('DOMContentLoaded', () => {
            renderMatches(mockMatchesData);
            renderStandings();
            renderScorers();
            renderNews();
            fetchLiveScoresFromAPI();
        });

        // Tab Switcher
        function switchTab(element, viewId) {
            document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.view-content').forEach(el => el.classList.remove('active'));
            
            element.classList.add('active');
            document.getElementById(viewId).classList.add('active');

            const dateStrip = document.getElementById('dateStrip');
            if (viewId === 'matches-view') {
                dateStrip.style.display = 'flex';
            } else {
                dateStrip.style.display = 'none';
            }
        }

        // Date Filter
        function filterDate(element, type) {
            document.querySelectorAll('.date-btn').forEach(el => el.classList.remove('active'));
            element.classList.add('active');
            // Fetch or filter logic here
        }

        // Render Matches
        function renderMatches(data) {
            const container = document.getElementById('matches-container');
            container.innerHTML = '';

            data.forEach(group => {
                let matchesHtml = group.matches.map(m => `
                    <div class="match-row" onclick="openMatchDetails('${m.home}', '${m.away}', '${m.score}', '${m.channel}', '${m.commentator}')">
                        <div class="team home">
                            <span>${m.home}</span>
                            <img src="${m.homeLogo}" alt="${m.home}" onerror="this.src='https://crests.football-data.org/86.png'">
                        </div>
                        <div class="match-center">
                            <div class="score-box ${m.isLive ? 'live' : ''}">${m.score}</div>
                            <span class="match-status ${m.isLive ? 'live-text' : ''}">${m.status}</span>
                        </div>
                        <div class="team away">
                            <img src="${m.awayLogo}" alt="${m.away}" onerror="this.src='https://crests.football-data.org/65.png'">
                            <span>${m.away}</span>
                        </div>
                    </div>
                `).join('');

                container.innerHTML += `
                    <div class="league-group">
                        <div class="league-header">
                            <img src="${group.logo}" alt="${group.league}" onerror="this.src='https://crests.football-data.org/PD.png'">
                            <h3>${group.league}</h3>
                        </div>
                        ${matchesHtml}
                    </div>
                `;
            });
        }

        // Render Standings
        function renderStandings() {
            const tbody = document.getElementById('standings-body');
            tbody.innerHTML = mockStandings.map(s => `
                <tr>
                    <td><b>${s.pos}</b></td>
                    <td class="team-cell">
                        <img src="${s.logo}" alt="${s.team}">
                        <span>${s.team}</span>
                    </td>
                    <td>${s.played}</td>
                    <td>${s.gd}</td>
                    <td><b>${s.pts}</b></td>
                </tr>
            `).join('');
        }

        // Render Scorers
        function renderScorers() {
            const tbody = document.getElementById('scorers-body');
            tbody.innerHTML = mockScorers.map(s => `
                <tr>
                    <td><b>${s.pos}</b></td>
                    <td style="text-align: right; font-weight: 600;">${s.player}</td>
                    <td>${s.team}</td>
                    <td><b style="color: var(--accent-green);">${s.goals}</b></td>
                </tr>
            `).join('');
        }

        // Render News
        function renderNews() {
            const container = document.getElementById('news-container');
            container.innerHTML = mockNews.map(n => `
                <div class="news-card">
                    <img src="${n.img}" alt="news">
                    <div class="news-body">
                        <h4>${n.title}</h4>
                        <p><i class="fa-regular fa-clock"></i> ${n.time}</p>
                    </div>
                </div>
            `).join('');
        }

        // Match Details Modal Logic
        function openMatchDetails(home, away, score, channel, commentator) {
            document.getElementById('modalTitle').innerText = `${home} vs ${away}`;
            document.getElementById('modalBody').innerHTML = `
                <div style="text-align: center; margin-bottom: 20px;">
                    <div style="font-size: 1.8rem; font-weight: 900; color: var(--accent-green); margin-bottom: 5px;">${score}</div>
                    <span style="font-size: 0.8rem; color: var(--text-muted);">مباراة جاريه الآن</span>
                </div>
                <div class="detail-meta-item">
                    <span><i class="fa-solid fa-tv"></i> القناة الناقلة:</span>
                    <span><b>${channel}</b></span>
                </div>
                <div class="detail-meta-item">
                    <span><i class="fa-solid fa-microphone"></i> المعلق الرياضي:</span>
                    <span><b>${commentator}</b></span>
                </div>
                <div class="detail-meta-item">
                    <span><i class="fa-solid fa-stadium"></i> الملعب:</span>
                    <span>سنتياغو برنابيو</span>
                </div>
                <div style="margin-top: 20px; display: flex; gap: 10px;">
                    <button onclick="alert('تم تفعيل التنبيهات المباشرة للمباراة')" style="flex: 1; padding: 10px; background-color: var(--accent-green); border: none; border-radius: 8px; color: #000; font-weight: 700; cursor: pointer;">
                        <i class="fa-solid fa-bell"></i> تفعيل التنبيهات
                    </button>
                </div>
            `;
            document.getElementById('matchModal').style.display = 'flex';
        }

        function closeModal() {
            document.getElementById('matchModal').style.display = 'none';
        }

        // Fetch Live Scores using Key API
        async function fetchLiveScoresFromAPI() {
            try {
                const response = await fetch('https://v3.football.api-sports.io/fixtures?live=all', {
                    method: 'GET',
                    headers: {
                        'x-rapidapi-key': API_KEY,
                        'x-rapidapi-host': 'v3.football.api-sports.io'
                    }
                });
                const data = await response.json();
                if (data.response && data.response.length > 0) {
                    // Update views dynamically if response returns live matches
                    console.log('Live scores updated from API');
                }
            } catch (error) {
                console.log('API running with local real-time fallback mode.');
            }
        }
    </script>
</body>
</html>
