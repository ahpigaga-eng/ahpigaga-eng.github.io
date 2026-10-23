<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>星核任務 (Star Core Mission)</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-user-select: none;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #050508;
            color: #fff;
        }

        #game-container {
            position: relative;
            width: 100vw;
            height: 100vh;
        }

        canvas {
            display: block;
            background: #080810;
        }

        /* --- UI Screens --- */
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
            background: rgba(5, 5, 12, 0.92);
            backdrop-filter: blur(8px);
            z-index: 10;
            transition: opacity 0.3s ease;
        }

        .hidden {
            display: none !important;
        }

        h1 {
            font-size: 3rem;
            color: #00f0ff;
            text-shadow: 0 0 20px rgba(0, 240, 255, 0.6);
            letter-spacing: 4px;
            margin-bottom: 10px;
            text-align: center;
        }

        .subtitle {
            font-size: 1.1rem;
            color: #8a99ad;
            margin-bottom: 30px;
        }

        .currency-display {
            position: absolute;
            top: 25px;
            right: 30px;
            font-size: 1.4rem;
            color: #ffd700;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
            background: rgba(0, 0, 0, 0.5);
            padding: 8px 18px;
            border-radius: 20px;
            border: 1px solid rgba(255, 215, 0, 0.4);
        }

        /* Hero Cards */
        .hero-select-container {
            display: flex;
            gap: 20px;
            margin-bottom: 30px;
            flex-wrap: wrap;
            justify-content: center;
            max-width: 1200px;
        }

        .hero-card {
            width: 240px;
            background: rgba(20, 25, 40, 0.8);
            border: 2px solid #1a2942;
            border-radius: 12px;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
        }

        .hero-card:hover {
            transform: translateY(-5px);
            border-color: #00f0ff;
            box-shadow: 0 0 20px rgba(0, 240, 255, 0.3);
        }

        .hero-card.selected {
            border-color: #00ff88;
            box-shadow: 0 0 25px rgba(0, 255, 136, 0.5);
            background: rgba(20, 40, 50, 0.9);
        }

        .hero-icon {
            width: 70px;
            height: 70px;
            border-radius: 50%;
            margin-bottom: 15px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.2);
        }

        .hero-name {
            font-size: 1.3rem;
            font-weight: bold;
            color: #fff;
            margin-bottom: 8px;
        }

        .hero-desc {
            font-size: 0.85rem;
            color: #a0b0c5;
            text-align: center;
            line-height: 1.4;
            margin-bottom: 15px;
            flex-grow: 1;
        }

        .hero-stats {
            width: 100%;
            font-size: 0.8rem;
            color: #88a0c0;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            padding-top: 10px;
            margin-bottom: 15px;
        }

        .stat-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 4px;
        }

        .btn {
            padding: 12px 30px;
            font-size: 1.1rem;
            font-weight: bold;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            background: linear-gradient(135deg, #00f0ff, #0077ff);
            color: #fff;
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.4);
            transition: all 0.2s ease;
        }

        .btn:hover {
            transform: scale(1.05);
            box-shadow: 0 0 25px rgba(0, 240, 255, 0.7);
        }

        .btn:disabled {
            background: #334;
            color: #667;
            box-shadow: none;
            cursor: not-allowed;
            transform: none;
        }

        .btn-unlock {
            background: linear-gradient(135deg, #ffd700, #ff8800);
            color: #000;
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.4);
            padding: 8px 16px;
            font-size: 0.95rem;
        }

        /* --- In-Game HUD --- */
        #hud {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        .hud-top {
            position: absolute;
            top: 15px;
            left: 20px;
            right: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .bar-container {
            width: 250px;
            background: rgba(0, 0, 0, 0.6);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 4px;
            margin-bottom: 6px;
        }

        .bar-label {
            font-size: 0.75rem;
            color: #aaa;
            margin-bottom: 2px;
            display: flex;
            justify-content: space-between;
        }

        .bar-fill {
            height: 12px;
            border-radius: 6px;
            transition: width 0.1s ease;
        }

        .hp-fill { background: linear-gradient(90deg, #ff3366, #ff6699); }
        .exp-fill { background: linear-gradient(90deg, #00ff88, #00f0ff); }
        .heat-fill { background: linear-gradient(90deg, #ffaa00, #ff3300); }

        .hud-center-top {
            text-align: center;
            background: rgba(0, 0, 0, 0.5);
            padding: 8px 20px;
            border-radius: 10px;
            border: 1px solid rgba(0, 240, 255, 0.3);
        }

        .wave-text {
            font-size: 1.3rem;
            font-weight: bold;
            color: #00f0ff;
        }

        .hud-right {
            text-align: right;
            font-size: 1rem;
            color: #fff;
            background: rgba(0, 0, 0, 0.5);
            padding: 10px 18px;
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        /* Active Item Notification */
        #item-status {
            position: absolute;
            top: 80px;
            left: 20px;
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .item-badge {
            background: rgba(0, 0, 0, 0.7);
            border: 1px solid #00f0ff;
            color: #00f0ff;
            padding: 6px 12px;
            border-radius: 6px;
            font-size: 0.85rem;
            display: flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 0 10px rgba(0, 240, 255, 0.3);
        }

        /* Bottom Controls / Skills */
        .hud-bottom {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 15px;
            pointer-events: auto;
        }

        .action-btn {
            width: 70px;
            height: 70px;
            background: rgba(10, 15, 30, 0.8);
            border: 2px solid #00f0ff;
            border-radius: 12px;
            color: #fff;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: all 0.2s ease;
            position: relative;
        }

        .action-btn:hover {
            background: rgba(0, 240, 255, 0.2);
        }

        .action-btn.disabled {
            border-color: #445;
            color: #556;
            cursor: not-allowed;
            background: rgba(10, 10, 15, 0.8);
        }

        .action-key {
            position: absolute;
            top: 4px;
            left: 6px;
            font-size: 0.7rem;
            color: #00f0ff;
            font-weight: bold;
        }

        .action-cost {
            font-size: 0.75rem;
            color: #ffd700;
        }

        /* Upgrade Modal */
        .upgrade-card-container {
            display: flex;
            gap: 20px;
            margin-top: 20px;
        }

        .upgrade-card {
            width: 220px;
            background: rgba(15, 20, 35, 0.95);
            border: 2px solid #00f0ff;
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            pointer-events: auto;
        }

        .upgrade-card:hover {
            transform: translateY(-8px) scale(1.03);
            box-shadow: 0 0 25px rgba(0, 240, 255, 0.6);
            border-color: #00ff88;
        }

        .upgrade-card.lucky {
            border-color: #ffd700;
            background: linear-gradient(145deg, rgba(30, 25, 10, 0.95), rgba(60, 50, 10, 0.95));
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.4);
        }

        .upgrade-title {
            font-size: 1.2rem;
            font-weight: bold;
            margin-bottom: 10px;
            color: #00f0ff;
        }

        .upgrade-card.lucky .upgrade-title {
            color: #ffd700;
        }

        .upgrade-desc {
            font-size: 0.9rem;
            color: #b0c0d5;
            line-height: 1.4;
        }

        .lucky-badge {
            background: #ffd700;
            color: #000;
            font-weight: bold;
            font-size: 0.7rem;
            padding: 2px 8px;
            border-radius: 10px;
            margin-bottom: 8px;
            display: inline-block;
        }
    </style>
</head>
<body>

<div id="game-container">
    <canvas id="gameCanvas"></canvas>

    <!-- 主選單 / 首頁 -->
    <div id="menu-screen" class="screen">
        <div class="currency-display">⭐ <span id="star-coins-display">0</span></div>
        <h1>星核守護者：最後曙光</h1>
        <div class="subtitle">選擇你的戰術機甲並抵禦 10 波極限攻勢</div>

        <div class="hero-select-container" id="hero-list">
            <!-- 角色卡片將動態產生 -->
        </div>

        <button class="btn" id="start-game-btn" onclick="startGame()">展開作戰</button>
    </div>

    <!-- 遊戲中 HUD -->
    <div id="hud" class="hidden">
        <div class="hud-top">
            <div>
                <div class="bar-container">
                    <div class="bar-label"><span>星核能量 (HP)</span><span id="hp-text">100/100</span></div>
                    <div class="bar-fill hp-fill" id="hp-bar" style="width: 100%;"></div>
                </div>
                <div class="bar-container">
                    <div class="bar-label"><span>經驗值 (EXP)</span><span id="exp-text">0/100</span></div>
                    <div class="bar-fill exp-fill" id="exp-bar" style="width: 0%;"></div>
                </div>
                <div class="bar-container">
                    <div class="bar-label"><span>槍管熱能</span><span id="heat-text">0%</span></div>
                    <div class="bar-fill heat-fill" id="heat-bar" style="width: 0%;"></div>
                </div>
            </div>

            <div class="hud-center-top">
                <div class="wave-text" id="wave-display">WAVE 1 / 10</div>
                <div style="font-size: 0.8rem; color: #8a99ad; margin-top:2px;" id="enemy-count">剩餘敵人: 0</div>
            </div>

            <div class="hud-right">
                <div>戰術點數: 💎 <span id="credits-display">0</span></div>
                <div>等級: LV <span id="level-display">1</span></div>
                <div>得分: <span id="score-display">0</span></div>
            </div>
        </div>

        <div id="item-status"></div>

        <div class="hud-bottom">
            <div class="action-btn" id="btn-build-turret" onclick="buildTurret()">
                <span class="action-key">SPACE</span>
                <span style="font-size: 1.2rem;">🛡️</span>
                <span style="font-size: 0.75rem;">建造砲塔</span>
                <span class="action-cost">💎 100 (<span id="turret-count">0/10</span>)</span>
            </div>
            <div class="action-btn" id="btn-skill-emp" onclick="useEMPSkill()">
                <span class="action-key">E</span>
                <span style="font-size: 1.2rem;">⚡</span>
                <span style="font-size: 0.75rem;">EMP衝擊</span>
                <span class="action-cost">💎 150</span>
            </div>
        </div>
    </div>

    <!-- 升級卡片選單 -->
    <div id="upgrade-screen" class="screen hidden">
        <h2 style="color: #00f0ff; margin-bottom: 10px;">系統強化升級</h2>
        <div style="color: #8a99ad; margin-bottom: 20px;">請選擇一項戰術模組進行加強</div>
        <div class="upgrade-card-container" id="upgrade-options">
            <!-- 動態生成升級選項 -->
        </div>
    </div>

    <!-- 結算畫面 (勝利 / 戰敗) -->
    <div id="game-over-screen" class="screen hidden">
        <h1 id="game-over-title">任務失敗</h1>
        <div class="subtitle" id="game-over-subtitle">星核已被摧毀...</div>

        <div style="background: rgba(0,0,0,0.6); padding: 20px; border-radius: 12px; border: 1px solid rgba(255,255,255,0.1); margin-bottom: 25px; text-align: center; min-width: 300px;">
            <div style="margin-bottom: 10px; font-size: 1.1rem;">最終得分: <span id="final-score" style="color: #00f0ff; font-weight: bold;">0</span></div>
            <div style="margin-bottom: 10px; font-size: 1.1rem;">到達波次: <span id="final-wave" style="color: #00ff88; font-weight: bold;">1</span></div>
            <div style="font-size: 1.3rem; color: #ffd700; font-weight: bold;">獲得星星幣: ⭐ +<span id="earned-coins">0</span></div>
        </div>

        <button class="btn" onclick="returnToMenu()">返回主選單</button>
    </div>
</div>

<script>
    // --- 角色數據配置 ---
    const HEROES = {
        sentinel: {
            id: 'sentinel',
            name: '標準機甲',
            icon: '🤖',
            desc: '中規中矩的戰術機甲，具有均衡的火力與裝甲表現。',
            cost: 0,
            hp: 1000,
            speed: 4.0,
            damage: 35,
            fireRate: 0.15, // 發射間隔(秒)
            range: Infinity,
            canHoldFire: true,
            maxTurrets: 10,
            unlocked: true
        },
        tank: {
            id: 'tank',
            name: '重裝泰坦',
            icon: '🛡️',
            desc: '極高血量與遠程高威能砲火。無法連發，但單發傷害毀滅性極高。',
            cost: 500,
            hp: 2500,
            speed: 2.8,
            damage: 280,
            fireRate: 0.8,
            range: Infinity,
            canHoldFire: false, // 不可長按連發
            maxTurrets: 10,
            unlocked: false
        },
        assassin: {
            id: 'assassin',
            name: '幻影刺客',
            icon: '⚡',
            desc: '移動極快，射程較短。貼身距離下子彈能造成高達 3 倍爆發傷害！',
            cost: 800,
            hp: 750,
            speed: 6.2,
            damage: 45,
            fireRate: 0.1,
            range: 280,
            canHoldFire: true,
            maxTurrets: 10,
            unlocked: false
        },
        mage: {
            id: 'mage',
            name: '秘法大導師',
            icon: '🔮',
            desc: '射程無限，子彈附帶全域緩速。防禦塔容量翻倍（最多可建 20 座）！',
            cost: 1200,
            hp: 600,
            speed: 3.5,
            damage: 40,
            fireRate: 0.2,
            range: Infinity,
            canHoldFire: true,
            maxTurrets: 20,
            unlocked: false
        }
    };

    // --- 升級選項數據 ---
    const NORMAL_UPGRADES = [
        { id: 'dmg', title: '火力強化', desc: '子彈傷害提升 25%' },
        { id: 'rate', title: '槍管冷卻', desc: '射擊速率提升 20%' },
        { id: 'speed', title: '動能推進', desc: '角色移動速度提升 20%' },
        { id: 'hp', title: '裝甲擴充', desc: '最大血量提升 300 並補滿能量' },
        { id: 'knockback', title: '衝擊重彈', desc: '子彈擊退力道大幅提升 80%' },
        { id: 'turret_dmg', title: '砲塔過載', desc: '防禦塔傷害與射速提升 30%' }
    ];

    const LUCKY_UPGRADES = [
        { id: 'lucky_360', title: '360°全方位彈幕', desc: '每次射擊向全方位 360 度環狀散射子彈！' },
        { id: 'lucky_dmg', title: '星核過載', desc: '子彈傷害暴增 500%！' },
        { id: 'lucky_blackhole', title: '黑洞重力彈', desc: '子彈具有重力吸附與強烈範圍爆破效果！' }
    ];

    // --- 遊戲狀態變數 ---
    let canvas, ctx;
    let selectedHeroId = 'sentinel';
    let starCoins = 0;
    let unlockedHeroes = ['sentinel'];

    let gameActive = false;
    let isPaused = false;

    let player;
    let bullets = [];
    let enemies = [];
    let turrets = [];
    let particles = [];
    let expGems = [];
    let items = [];

    let wave = 1;
    const MAX_WAVES = 10;
    let waveEnemiesToSpawn = 0;
    let waveSpawnTimer = 0;
    let credits = 100;
    let score = 0;
    let level = 1;
    let exp = 0;
    let expToNextLevel = 100;

    let keys = {};
    let mousePos = { x: 0, y: 0 };
    let isMouseDown = false;

    let itemSpawnTimer = 0; // 地圖道具生成計時
    let acquiredLuckyUpgrades = []; // 已獲得的幸運升級

    // 初始化頁面
    window.onload = () => {
        canvas = document.getElementById('gameCanvas');
        ctx = canvas.getContext('2d');
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);

        loadStorageData();
        renderHeroSelectScreen();

        // 綁定事件
        window.addEventListener('keydown', e => {
            keys[e.code] = true;
            if (e.code === 'Space' && gameActive && !isPaused) buildTurret();
            if (e.code === 'KeyE' && gameActive && !isPaused) useEMPSkill();
        });
        window.addEventListener('keyup', e => keys[e.code] = false);

        canvas.addEventListener('mousemove', e => {
            const rect = canvas.getBoundingClientRect();
            mousePos.x = e.clientX - rect.left;
            mousePos.y = e.clientY - rect.top;
        });

        canvas.addEventListener('mousedown', e => {
            if (e.button === 0) {
                isMouseDown = true;
                if (gameActive && !isPaused) {
                    player.tryShoot();
                }
            }
        });

        canvas.addEventListener('mouseup', e => {
            if (e.button === 0) {
                isMouseDown = false;
                if (player) player.mustReleaseToFire = false;
            }
        });

        requestAnimationFrame(gameLoop);
    };

    function resizeCanvas() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
    }

    // --- 儲存機制 (LocalStorage) ---
    function loadStorageData() {
        const savedCoins = localStorage.getItem('sentinel_star_coins');
        if (savedCoins !== null) starCoins = parseInt(savedCoins);

        const savedHeroes = localStorage.getItem('sentinel_unlocked_heroes');
        if (savedHeroes !== null) {
            unlockedHeroes = JSON.parse(savedHeroes);
            unlockedHeroes.forEach(id => {
                if (HEROES[id]) HEROES[id].unlocked = true;
            });
        }
        updateCoinDisplay();
    }

    function saveStorageData() {
        localStorage.setItem('sentinel_star_coins', starCoins.toString());
        localStorage.setItem('sentinel_unlocked_heroes', JSON.stringify(unlockedHeroes));
        updateCoinDisplay();
    }

    function updateCoinDisplay() {
        document.getElementById('star-coins-display').innerText = starCoins;
    }

    // --- 首頁選角 UI 渲染 ---
    function renderHeroSelectScreen() {
        const container = document.getElementById('hero-list');
        container.innerHTML = '';

        Object.values(HEROES).forEach(hero => {
            const card = document.createElement('div');
            card.className = `hero-card ${selectedHeroId === hero.id ? 'selected' : ''}`;
            card.onclick = () => {
                if (hero.unlocked) {
                    selectedHeroId = hero.id;
                    renderHeroSelectScreen();
                }
            };

            const isUnlocked = hero.unlocked;

            card.innerHTML = `
                <div class="hero-icon" style="background: ${getHeroColor(hero.id)}22; border: 2px solid ${getHeroColor(hero.id)};">${hero.icon}</div>
                <div class="hero-name">${hero.name}</div>
                <div class="hero-desc">${hero.desc}</div>
                <div class="hero-stats">
                    <div class="stat-row"><span>生命值:</span><span>${hero.hp}</span></div>
                    <div class="stat-row"><span>基礎傷害:</span><span>${hero.damage}</span></div>
                    <div class="stat-row"><span>移動速度:</span><span>${hero.speed}</span></div>
                    <div class="stat-row"><span>塔容量:</span><span>${hero.maxTurrets} 座</span></div>
                </div>
                ${!isUnlocked ? `
                    <button class="btn btn-unlock" onclick="unlockHero(event, '${hero.id}')">⭐ ${hero.cost} 解鎖</button>
                ` : `
                    <div style="color: ${selectedHeroId === hero.id ? '#00ff88' : '#8a99ad'}; font-size: 0.85rem; font-weight: bold;">
                        ${selectedHeroId === hero.id ? '已選擇' : '點擊選擇'}
                    </div>
                `}
            `;
            container.appendChild(card);
        });
    }

    function getHeroColor(id) {
        switch(id) {
            case 'sentinel': return '#00f0ff';
            case 'tank': return '#ffaa00';
            case 'assassin': return '#ff0055';
            case 'mage': return '#aa00ff';
            default: return '#ffffff';
        }
    }

    function unlockHero(event, heroId) {
        event.stopPropagation();
        const hero = HEROES[heroId];
        if (starCoins >= hero.cost) {
            starCoins -= hero.cost;
            hero.unlocked = true;
            unlockedHeroes.push(heroId);
            selectedHeroId = heroId;
            saveStorageData();
            renderHeroSelectScreen();
        } else {
            alert('星星幣不足！完成戰鬥以獲取更多星星幣。');
        }
    }

    // --- 遊戲開啟與重置 ---
    function startGame() {
        document.getElementById('menu-screen').classList.add('hidden');
        document.getElementById('game-over-screen').classList.add('hidden');
        document.getElementById('hud').classList.remove('hidden');

        resetGame();
        gameActive = true;
        isPaused = false;
    }

    function resetGame() {
        const heroConfig = HEROES[selectedHeroId];

        player = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 20,
            heroId: heroConfig.id,
            maxHp: heroConfig.hp,
            hp: heroConfig.hp,
            speed: heroConfig.speed,
            baseDamage: heroConfig.damage,
            damageMultiplier: 1.0,
            fireRate: heroConfig.fireRate,
            fireTimer: 0,
            range: heroConfig.range,
            canHoldFire: heroConfig.canHoldFire,
            maxTurrets: heroConfig.maxTurrets,
            
            // 熱能系統
            heat: 0,
            maxHeat: 100,
            isOverheated: false,
            mustReleaseToFire: false,

            // 回血與無傷機制
            lastDamageTime: Date.now(),
            lastAttackTime: Date.now(),

            // 戰術屬性
            knockbackMult: 1.0,
            has360Shot: false,
            hasBlackhole: false,
            turretDmgMult: 1.0,

            // 道具Buff狀態
            autoAimTimer: 0,
            slowTimer: 0,
            coolTimer: 0,
            shieldLayers: 0,

            tryShoot() {
                if (this.isOverheated || this.mustReleaseToFire) return;

                const now = Date.now();
                if (now - this.lastAttackTime < this.fireRate * 1000) return;

                this.lastAttackTime = now;
                this.shoot();

                // 槍管積熱 (若非無效積熱 Buff 下)
                if (this.coolTimer <= 0) {
                    this.heat += 4;
                    if (this.heat >= this.maxHeat) {
                        this.heat = this.maxHeat;
                        this.isOverheated = true;
                        this.mustReleaseToFire = true; // 需重新長按
                        createPulseWave(this.x, this.y, 180, '#ff3300'); // 純擊退無傷害
                    }
                }
            },

            shoot() {
                let targetAngle = Math.atan2(mousePos.y - this.y, mousePos.x - this.x);

                // 自動瞄準道具判斷
                if (this.autoAimTimer > 0 && enemies.length > 0) {
                    let nearest = null;
                    let minDist = Infinity;
                    enemies.forEach(e => {
                        const d = Math.hypot(e.x - this.x, e.y - this.y);
                        if (d < minDist) { minDist = d; nearest = e; }
                    });
                    if (nearest) {
                        targetAngle = Math.atan2(nearest.y - this.y, nearest.x - this.x);
                    }
                }

                const finalDamage = this.baseDamage * this.damageMultiplier;

                if (this.has360Shot) {
                    for (let i = 0; i < 12; i++) {
                        const ang = (Math.PI * 2 / 12) * i;
                        spawnBullet(this.x, this.y, ang, finalDamage, this.range, this.knockbackMult, this.hasBlackhole);
                    }
                } else {
                    spawnBullet(this.x, this.y, targetAngle, finalDamage, this.range, this.knockbackMult, this.hasBlackhole);
                }
            }
        };

        bullets = [];
        enemies = [];
        turrets = [];
        particles = [];
        expGems = [];
        items = [];

        wave = 1;
        credits = 100;
        score = 0;
        level = 1;
        exp = 0;
        expToNextLevel = 100;
        itemSpawnTimer = 0;
        acquiredLuckyUpgrades = [];

        startWave();
        updateHUD();
    }

    function spawnBullet(x, y, angle, damage, range, knockbackMult, isBlackhole = false) {
        bullets.push({
            x: x,
            y: y,
            startX: x,
            startY: y,
            vx: Math.cos(angle) * 14,
            vy: Math.sin(angle) * 14,
            damage: damage,
            range: range,
            knockbackMult: knockbackMult,
            isBlackhole: isBlackhole,
            radius: isBlackhole ? 8 : 4,
            color: isBlackhole ? '#aa00ff' : '#00f0ff'
        });
    }

    function createPulseWave(x, y, radius, color) {
        // 槍管過熱擊退波（無傷害）
        enemies.forEach(e => {
            const dist = Math.hypot(e.x - x, e.y - y);
            if (dist < radius) {
                const angle = Math.atan2(e.y - y, e.x - x);
                const force = (1 - dist / radius) * 25;
                e.vx += Math.cos(angle) * force;
                e.vy += Math.sin(angle) * force;
            }
        });
        // 視覺粒子
        for (let i = 0; i < 30; i++) {
            const ang = Math.random() * Math.PI * 2;
            const spd = Math.random() * 6 + 2;
            particles.push({
                x: x, y: y,
                vx: Math.cos(ang) * spd,
                vy: Math.sin(ang) * spd,
                life: 1.0,
                color: color,
                size: Math.random() * 4 + 2
            });
        }
    }

    // --- 波次生成系統 ---
    function startWave() {
        waveEnemiesToSpawn = 10 + wave * 8;
        waveSpawnTimer = 0;
        updateHUD();
    }

    function spawnEnemy() {
        // 邊界隨機生成
        let x, y;
        if (Math.random() < 0.5) {
            x = Math.random() < 0.5 ? -30 : canvas.width + 30;
            y = Math.random() * canvas.height;
        } else {
            x = Math.random() * canvas.width;
            y = Math.random() < 0.5 ? -30 : canvas.height + 30;
        }

        const isBossWave = (wave % 5 === 0);
        const isBoss = isBossWave && waveEnemiesToSpawn === 1; // Boss 作為最後一隻

        if (isBoss) {
            enemies.push({
                x: x, y: y,
                vx: 0, vy: 0,
                radius: 40,
                hp: 1500 * (wave / 5),
                maxHp: 1500 * (wave / 5),
                speed: 0.7, // 調慢 Boss 移速
                damage: 80,
                scoreValue: 1000,
                isBoss: true,
                color: '#ff0055'
            });
        } else {
            const speed = Math.random() * 0.8 + 1.2 + (wave * 0.1);
            enemies.push({
                x: x, y: y,
                vx: 0, vy: 0,
                radius: 15,
                hp: 40 + wave * 20,
                maxHp: 40 + wave * 20,
                speed: speed,
                damage: 20,
                scoreValue: 50,
                isBoss: false,
                color: '#ff3300'
            });
        }
    }

    // --- 道具系統 (每 30 秒生成，存留 40 秒，效果 10 秒) ---
    function spawnMapItem() {
        const types = ['aim', 'slow', 'heal', 'shield', 'nuke', 'cool'];
        const selectedType = types[Math.floor(Math.random() * types.length)];

        items.push({
            x: Math.random() * (canvas.width - 100) + 50,
            y: Math.random() * (canvas.height - 100) + 50,
            type: selectedType,
            spawnTime: Date.now(),
            duration: 40000, // 40 秒後消失
            radius: 16
        });
    }

    function getItemColor(type) {
        switch(type) {
            case 'aim': return '#ffd700';
            case 'slow': return '#00f0ff';
            case 'heal': return '#00ff88';
            case 'shield': return '#0099ff';
            case 'nuke': return '#ff0033';
            case 'cool': return '#ff8800';
            default: return '#fff';
        }
    }

    function getItemIcon(type) {
        switch(type) {
            case 'aim': return '🎯';
            case 'slow': return '❄️';
            case 'heal': return '❤️';
            case 'shield': return '🛡️';
            case 'nuke': return '💣';
            case 'cool': return '⚡';
            default: return '🎁';
        }
    }

    function applyItemEffect(type) {
        switch(type) {
            case 'aim':
                player.autoAimTimer = 10;
                break;
            case 'slow':
                player.slowTimer = 10;
                break;
            case 'heal':
                player.hp = Math.min(player.maxHp, player.hp + player.maxHp * 0.5);
                break;
            case 'shield':
                player.shieldLayers++;
                break;
            case 'cool':
                player.coolTimer = 5;
                player.heat = 0;
                player.isOverheated = false;
                break;
            case 'nuke':
                // 核爆打擊：全場高額傷害 + 正確擊殺與資源結算
                createPulseWave(canvas.width / 2, canvas.height / 2, 1000, '#ff0033');
                for (let i = enemies.length - 1; i >= 0; i--) {
                    let e = enemies[i];
                    e.hp -= 800;
                    if (e.hp <= 0) {
                        onEnemyKill(e);
                        enemies.splice(i, 1);
                    }
                }
                break;
        }
        updateHUD();
    }

    // 擊殺敵人結算 (調降戰術點數獲得 rate)
    function onEnemyKill(e) {
        score += e.scoreValue;
        // 戰術點數調降為原本的約 1/3.5 (擊殺普通怪約獲得 1-2 點)
        credits += Math.max(1, Math.floor(e.scoreValue / 35));

        // 掉落經驗寶石
        expGems.push({
            x: e.x, y: e.y,
            value: e.isBoss ? 150 : 25
        });

        updateHUD();
    }

    // --- 防禦塔與技能 ---
    function buildTurret() {
        if (credits < 100) return;
        if (turrets.length >= player.maxTurrets) return;

        credits -= 100;
        turrets.push({
            x: player.x,
            y: player.y,
            radius: 18,
            range: 220,
            fireRate: 0.4,
            fireTimer: 0,
            damage: 25 * player.turretDmgMult
        });
        updateHUD();
    }

    function useEMPSkill() {
        if (credits < 150) return;
        credits -= 150;

        createPulseWave(player.x, player.y, 400, '#00f0ff');

        // EMP 傷害與擊杀邏輯修復
        for (let i = enemies.length - 1; i >= 0; i--) {
            let e = enemies[i];
            const dist = Math.hypot(e.x - player.x, e.y - player.y);
            if (dist < 400) {
                e.hp -= 300;
                if (e.hp <= 0) {
                    onEnemyKill(e);
                    enemies.splice(i, 1);
                }
            }
        }
        updateHUD();
    }

    // --- 升級系統邏輯 ---
    function addExp(amount) {
        exp += amount;
        if (exp >= expToNextLevel) {
            exp -= expToNextLevel;
            level++;
            expToNextLevel = Math.floor(expToNextLevel * 1.3);
            triggerUpgradeMenu();
        }
        updateHUD();
    }

    function triggerUpgradeMenu() {
        isPaused = true;
        const container = document.getElementById('upgrade-options');
        container.innerHTML = '';

        // 判定 1% 機率出現幸運升級
        const isLuckyRoll = Math.random() < 0.01;
        let availableLucky = LUCKY_UPGRADES.filter(u => !acquiredLuckyUpgrades.includes(u.id));

        let cardPool = [];

        if (isLuckyRoll && availableLucky.length > 0) {
            // 隨機選一個幸運升級 + 兩個普通升級
            const luckyChoice = availableLucky[Math.floor(Math.random() * availableLucky.length)];
            cardPool.push({ ...luckyChoice, isLucky: true });
        }

        // 補滿 3 張卡片
        while (cardPool.length < 3) {
            const randNorm = NORMAL_UPGRADES[Math.floor(Math.random() * NORMAL_UPGRADES.length)];
            if (!cardPool.some(c => c.id === randNorm.id)) {
                cardPool.push({ ...randNorm, isLucky: false });
            }
        }

        cardPool.forEach(opt => {
            const card = document.createElement('div');
            card.className = `upgrade-card ${opt.isLucky ? 'lucky' : ''}`;
            card.onclick = () => selectUpgrade(opt);

            card.innerHTML = `
                ${opt.isLucky ? '<div class="lucky-badge">✨ 幸運天賦</div>' : ''}
                <div class="upgrade-title">${opt.title}</div>
                <div class="upgrade-desc">${opt.desc}</div>
            `;
            container.appendChild(card);
        });

        document.getElementById('upgrade-screen').classList.remove('hidden');
    }

    function selectUpgrade(option) {
        if (option.isLucky) {
            acquiredLuckyUpgrades.push(option.id);
            if (option.id === 'lucky_360') player.has360Shot = true;
            if (option.id === 'lucky_dmg') player.damageMultiplier *= 6.0;
            if (option.id === 'lucky_blackhole') player.hasBlackhole = true;
        } else {
            switch(option.id) {
                case 'dmg': player.damageMultiplier *= 1.25; break;
                case 'rate': player.fireRate *= 0.8; break;
                case 'speed': player.speed *= 1.2; break;
                case 'hp':
                    player.maxHp += 300;
                    player.hp = player.maxHp;
                    break;
                case 'knockback': player.knockbackMult *= 1.8; break;
                case 'turret_dmg': player.turretDmgMult *= 1.3; break;
            }
        }

        document.getElementById('upgrade-screen').classList.add('hidden');
        isPaused = false;
        updateHUD();
    }

    // --- 遊戲 Update 與 Loop ---
    function gameLoop() {
        if (gameActive && !isPaused) {
            update();
        }
        draw();
        requestAnimationFrame(gameLoop);
    }

    function update() {
        const now = Date.now();

        // 1. 移動控制
        let moveX = 0, moveY = 0;
        if (keys['KeyW'] || keys['ArrowUp']) moveY -= 1;
        if (keys['KeyS'] || keys['ArrowDown']) moveY += 1;
        if (keys['KeyA'] || keys['ArrowLeft']) moveX -= 1;
        if (keys['KeyD'] || keys['ArrowRight']) moveX += 1;

        if (moveX !== 0 && moveY !== 0) {
            moveX *= 0.7071;
            moveY *= 0.7071;
        }

        player.x += moveX * player.speed;
        player.y += moveY * player.speed;

        // 邊界限制
        player.x = Math.max(player.radius, Math.min(canvas.width - player.radius, player.x));
        player.y = Math.max(player.radius, Math.min(canvas.height - player.radius, player.y));

        // 長按連發處理
        if (isMouseDown && player.canHoldFire) {
            player.tryShoot();
        }

        // 熱能自然冷卻
        if (!isMouseDown || player.isOverheated) {
            player.heat = Math.max(0, player.heat - 0.5);
            if (player.heat === 0) {
                player.isOverheated = false;
            }
        }

        // 脫戰回血判定：完全停止攻擊 1 秒以上 且 1 秒內未受到傷害
        if (now - player.lastDamageTime > 1000 && now - player.lastAttackTime > 1000) {
            if (player.hp < player.maxHp) {
                player.hp = Math.min(player.maxHp, player.hp + player.maxHp * 0.005);
            }
        }

        // 道具 Timer 遞減
        if (player.autoAimTimer > 0) player.autoAimTimer -= 1/60;
        if (player.slowTimer > 0) player.slowTimer -= 1/60;
        if (player.coolTimer > 0) player.coolTimer -= 1/60;

        // 地圖道具生成 (每 30 秒)
        itemSpawnTimer += 1/60;
        if (itemSpawnTimer >= 30) {
            itemSpawnTimer = 0;
            spawnMapItem();
        }

        // 波次生成邏輯
        if (waveEnemiesToSpawn > 0) {
            waveSpawnTimer += 1/60;
            if (waveSpawnTimer >= 0.8) {
                waveSpawnTimer = 0;
                spawnEnemy();
                waveEnemiesToSpawn--;
            }
        } else if (enemies.length === 0) {
            // 波次完成
            if (wave < MAX_WAVES) {
                wave++;
                startWave();
            } else {
                // 成功通關 10 波
                gameWin();
            }
        }

        // 更新子彈
        for (let i = bullets.length - 1; i >= 0; i--) {
            let b = bullets[i];
            b.x += b.vx;
            b.y += b.vy;

            // 超出射程或螢幕
            const traveled = Math.hypot(b.x - b.startX, b.y - b.startY);
            if (traveled > b.range || b.x < 0 || b.x > canvas.width || b.y < 0 || b.y > canvas.height) {
                bullets.splice(i, 1);
                continue;
            }

            // 子彈與敵軍碰撞
            for (let j = enemies.length - 1; j >= 0; j--) {
                let e = enemies[j];
                const dist = Math.hypot(b.x - e.x, b.y - e.y);
                if (dist < b.radius + e.radius) {
                    // 刺客距離傷害加成 (越近最高 3 倍)
                    let dmg = b.damage;
                    if (player.heroId === 'assassin') {
                        const distToPlayer = Math.hypot(e.x - player.x, e.y - player.y);
                        const proxRatio = Math.max(0, 1 - distToPlayer / 280);
                        dmg *= (1 + proxRatio * 2);
                    }

                    e.hp -= dmg;

                    // 擊退
                    const angle = Math.atan2(e.y - b.y, e.x - b.x);
                    const force = (e.isBoss ? 1.5 : 6) * b.knockbackMult;
                    e.vx += Math.cos(angle) * force;
                    e.vy += Math.sin(angle) * force;

                    // 黑洞吸附
                    if (b.isBlackhole) {
                        createPulseWave(b.x, b.y, 120, '#aa00ff');
                    }

                    bullets.splice(i, 1);

                    if (e.hp <= 0) {
                        onEnemyKill(e);
                        enemies.splice(j, 1);
                    }
                    break;
                }
            }
        }

        // 更新敵人與物理推開
        const slowMult = player.slowTimer > 0 ? 0.4 : 1.0;
        for (let i = enemies.length - 1; i >= 0; i--) {
            let e = enemies[i];

            // 慣性衰減
            e.vx *= 0.85;
            e.vy *= 0.85;

            const angle = Math.atan2(player.y - e.y, player.x - e.x);
            e.x += (Math.cos(angle) * e.speed * slowMult) + e.vx;
            e.y += (Math.sin(angle) * e.speed * slowMult) + e.vy;

            // 觸碰玩家傷害與「物理防重疊推開」
            const distToPlayer = Math.hypot(e.x - player.x, e.y - player.y);
            const minDist = e.radius + player.radius;

            if (distToPlayer < minDist) {
                // 強制將敵人推開至玩家圓圈外圍，解決卡在中心問題
                const pushAngle = Math.atan2(e.y - player.y, e.x - player.x);
                e.x = player.x + Math.cos(pushAngle) * (minDist + 2);
                e.y = player.y + Math.sin(pushAngle) * (minDist + 2);

                // 傷害判定
                if (player.shieldLayers > 0) {
                    player.shieldLayers--;
                } else {
                    player.hp -= e.damage * 0.02; // 持續碰撞傷害
                    player.lastDamageTime = now;
                }

                if (player.hp <= 0) {
                    player.hp = 0;
                    gameOver(false);
                }
            }
        }

        // 更新防禦塔
        turrets.forEach(t => {
            t.fireTimer += 1/60;
            if (t.fireTimer >= t.fireRate) {
                let nearest = null;
                let minDist = t.range;
                enemies.forEach(e => {
                    const d = Math.hypot(e.x - t.x, e.y - t.y);
                    if (d < minDist) { minDist = d; nearest = e; }
                });

                if (nearest) {
                    t.fireTimer = 0;
                    const ang = Math.atan2(nearest.y - t.y, nearest.x - t.x);
                    spawnBullet(t.x, t.y, ang, t.damage, t.range, 0.5);
                }
            }
        });

        // 經驗寶石拾取
        for (let i = expGems.length - 1; i >= 0; i--) {
            let g = expGems[i];
            const d = Math.hypot(g.x - player.x, g.y - player.y);
            if (d < 120) {
                // 吸附
                const ang = Math.atan2(player.y - g.y, player.x - g.x);
                g.x += Math.cos(ang) * 8;
                g.y += Math.sin(ang) * 8;
            }
            if (d < player.radius + 10) {
                addExp(g.value);
                expGems.splice(i, 1);
            }
        }

        // 地圖道具拾取與 40 秒逾時消失
        for (let i = items.length - 1; i >= 0; i--) {
            let item = items[i];
            if (now - item.spawnTime > item.duration) {
                items.splice(i, 1); // 40 秒未拾取消失
                continue;
            }

            const d = Math.hypot(item.x - player.x, item.y - player.y);
            if (d < player.radius + item.radius) {
                applyItemEffect(item.type);
                items.splice(i, 1);
            }
        }

        // 粒子更新
        for (let i = particles.length - 1; i >= 0; i--) {
            let p = particles[i];
            p.x += p.vx;
            p.y += p.vy;
            p.life -= 0.03;
            if (p.life <= 0) particles.splice(i, 1);
        }

        updateHUD();
    }

    // --- 繪製畫布 ---
    function draw() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);

        // 背景網格
        ctx.strokeStyle = 'rgba(255,255,255,0.03)';
        ctx.lineWidth = 1;
        const gridSize = 50;
        for (let x = 0; x < canvas.width; x += gridSize) {
            ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
        }
        for (let y = 0; y < canvas.height; y += gridSize) {
            ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
        }

        if (!gameActive) return;

        // 繪製經驗寶石
        expGems.forEach(g => {
            ctx.fillStyle = '#00ff88';
            ctx.shadowColor = '#00ff88';
            ctx.shadowBlur = 8;
            ctx.beginPath();
            ctx.arc(g.x, g.y, 4, 0, Math.PI * 2);
            ctx.fill();
            ctx.shadowBlur = 0;
        });

        // 繪製地圖道具 (小於 10 秒閃爍)
        const now = Date.now();
        items.forEach(item => {
            const timeLeft = (item.duration - (now - item.spawnTime)) / 1000;
            if (timeLeft < 10 && Math.floor(now / 200) % 2 === 0) return; // 閃爍效果

            const color = getItemColor(item.type);
            ctx.fillStyle = color;
            ctx.shadowColor = color;
            ctx.shadowBlur = 12;
            ctx.beginPath();
            ctx.arc(item.x, item.y, item.radius, 0, Math.PI * 2);
            ctx.fill();
            ctx.shadowBlur = 0;

            ctx.fillStyle = '#000';
            ctx.font = '14px sans-serif';
            ctx.textAlign = 'center';
            ctx.textBaseline = 'middle';
            ctx.fillText(getItemIcon(item.type), item.x, item.y);
        });

        // 繪製防禦塔
        turrets.forEach(t => {
            ctx.fillStyle = '#00f0ff';
            ctx.beginPath();
            ctx.arc(t.x, t.y, t.radius, 0, Math.PI * 2);
            ctx.fill();
            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = 2;
            ctx.stroke();
        });

        // 繪製敵人
        enemies.forEach(e => {
            ctx.fillStyle = e.color;
            ctx.shadowColor = e.color;
            ctx.shadowBlur = e.isBoss ? 20 : 8;
            ctx.beginPath();
            ctx.arc(e.x, e.y, e.radius, 0, Math.PI * 2);
            ctx.fill();
            ctx.shadowBlur = 0;

            // 血條
            if (e.hp < e.maxHp) {
                const barWidth = e.radius * 2;
                ctx.fillStyle = 'rgba(0,0,0,0.5)';
                ctx.fillRect(e.x - e.radius, e.y - e.radius - 10, barWidth, 4);
                ctx.fillStyle = '#ff3366';
                ctx.fillRect(e.x - e.radius, e.y - e.radius - 10, barWidth * (e.hp / e.maxHp), 4);
            }
        });

        // 繪製子彈
        bullets.forEach(b => {
            ctx.fillStyle = b.color;
            ctx.shadowColor = b.color;
            ctx.shadowBlur = 10;
            ctx.beginPath();
            ctx.arc(b.x, b.y, b.radius, 0, Math.PI * 2);
            ctx.fill();
            ctx.shadowBlur = 0;
        });

        // 繪製粒子
        particles.forEach(p => {
            ctx.fillStyle = p.color;
            ctx.globalAlpha = p.life;
            ctx.beginPath();
            ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
            ctx.fill();
            ctx.globalAlpha = 1.0;
        });

        // 繪製玩家角色
        ctx.fillStyle = getHeroColor(player.heroId);
        ctx.shadowColor = getHeroColor(player.heroId);
        ctx.shadowBlur = 15;
        ctx.beginPath();
        ctx.arc(player.x, player.y, player.radius, 0, Math.PI * 2);
        ctx.fill();
        ctx.shadowBlur = 0;

        // 無傷回血綠色光暈
        if (now - player.lastDamageTime > 1000 && now - player.lastAttackTime > 1000 && player.hp < player.maxHp) {
            ctx.strokeStyle = '#00ff88';
            ctx.lineWidth = 3;
            ctx.beginPath();
            ctx.arc(player.x, player.y, player.radius + 6, 0, Math.PI * 2);
            ctx.stroke();
        }

        // 護盾環
        if (player.shieldLayers > 0) {
            ctx.strokeStyle = '#0099ff';
            ctx.lineWidth = 4;
            ctx.beginPath();
            ctx.arc(player.x, player.y, player.radius + 10, 0, Math.PI * 2);
            ctx.stroke();
        }

        // 準星指向線
        ctx.strokeStyle = 'rgba(0, 240, 255, 0.3)';
        ctx.lineWidth = 1;
        ctx.beginPath();
        ctx.moveTo(player.x, player.y);
        ctx.lineTo(mousePos.x, mousePos.y);
        ctx.stroke();
    }

    // --- HUD 界面更新 ---
    function updateHUD() {
        if (!player) return;

        document.getElementById('hp-text').innerText = `${Math.ceil(player.hp)} / ${player.maxHp}`;
        document.getElementById('hp-bar').style.width = `${(player.hp / player.maxHp) * 100}%`;

        document.getElementById('exp-text').innerText = `${exp} / ${expToNextLevel}`;
        document.getElementById('exp-bar').style.width = `${(exp / expToNextLevel) * 100}%`;

        document.getElementById('heat-text').innerText = `${Math.floor(player.heat)}%`;
        document.getElementById('heat-bar').style.width = `${player.heat}%`;

        document.getElementById('wave-display').innerText = `WAVE ${wave} / ${MAX_WAVES}`;
        document.getElementById('enemy-count').innerText = `剩餘敵人: ${enemies.length + waveEnemiesToSpawn}`;

        document.getElementById('credits-display').innerText = credits;
        document.getElementById('level-display').innerText = level;
        document.getElementById('score-display').innerText = score;

        document.getElementById('turret-count').innerText = `${turrets.length}/${player.maxTurrets}`;

        // 道具 Buff 狀態顯示
        const itemStatus = document.getElementById('item-status');
        itemStatus.innerHTML = '';
        if (player.autoAimTimer > 0) itemStatus.innerHTML += `<div class="item-badge">🎯 自動瞄準 (${Math.ceil(player.autoAimTimer)}s)</div>`;
        if (player.slowTimer > 0) itemStatus.innerHTML += `<div class="item-badge">❄️ 全域緩速 (${Math.ceil(player.slowTimer)}s)</div>`;
        if (player.coolTimer > 0) itemStatus.innerHTML += `<div class="item-badge">⚡ 無限槍管 (${Math.ceil(player.coolTimer)}s)</div>`;
        if (player.shieldLayers > 0) itemStatus.innerHTML += `<div class="item-badge">🛡️ 能量護盾 x${player.shieldLayers}</div>`;

        // 建造按鈕狀態
        const buildBtn = document.getElementById('btn-build-turret');
        if (credits < 100 || turrets.length >= player.maxTurrets) buildBtn.classList.add('disabled');
        else buildBtn.classList.remove('disabled');

        const empBtn = document.getElementById('btn-skill-emp');
        if (credits < 150) empBtn.classList.add('disabled');
        else empBtn.classList.remove('disabled');
    }

    // --- 遊戲結束與結算 ---
    function gameOver(isWin) {
        gameActive = false;
        document.getElementById('hud').classList.add('hidden');

        // 星星幣結算：波次 * 20 + 得分 5%
        const earnedCoins = Math.floor(wave * 20 + score * 0.05);
        starCoins += earnedCoins;
        saveStorageData();

        document.getElementById('game-over-title').innerText = isWin ? '任務成功！' : '任務失敗';
        document.getElementById('game-over-title').style.color = isWin ? '#00ff88' : '#ff3366';
        document.getElementById('game-over-subtitle').innerText = isWin ? '成功守護星核，擊退所有攻勢！' : '星核已被毀滅...';
        document.getElementById('final-score').innerText = score;
        document.getElementById('final-wave').innerText = wave;
        document.getElementById('earned-coins').innerText = earnedCoins;

        document.getElementById('game-over-screen').classList.remove('hidden');
    }

    function gameWin() {
        gameOver(true);
    }

    function returnToMenu() {
        document.getElementById('game-over-screen').classList.add('hidden');
        document.getElementById('menu-screen').classList.remove('hidden');
        renderHeroSelectScreen();
    }
</script>
</body>
</html>
