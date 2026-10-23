<html lang="zh-TW" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>星核守護者：最後曙光 (Core Sentinel: Last Dawn)</title>
    <!DOCTYPE html>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        cyber: {
                            bg: '#030712',
                            card: 'rgba(15, 23, 42, 0.85)',
                            cyan: '#06b6d4',
                            pink: '#ec4899',
                            purple: '#a855f7',
                            yellow: '#eab308',
                            amber: '#f59e0b'
                        }
                    },
                    fontFamily: {
                        orbitron: ['Orbitron', 'Noto Sans TC', 'sans-serif'],
                        sans: ['Inter', 'Noto Sans TC', 'sans-serif']
                    }
                }
            }
        }
    </script>
    
    <!-- Google Fonts & Font Awesome Icons -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Noto+Sans+TC:wght@400;700;900&family=Orbitron:wght@600;800;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        * {
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }

        body {
            background-color: #030712;
            color: #f8fafc;
            font-family: 'Inter', 'Noto Sans TC', sans-serif;
            overflow: hidden;
            width: 100vw;
            height: 100vh;
        }

        .font-orbitron {
            font-family: 'Orbitron', sans-serif;
        }

        .glow-cyan {
            box-shadow: 0 0 20px rgba(6, 182, 212, 0.5), inset 0 0 10px rgba(6, 182, 212, 0.3);
            border: 1px solid rgba(6, 182, 212, 0.8);
        }

        .glow-pink {
            box-shadow: 0 0 20px rgba(236, 72, 153, 0.5), inset 0 0 10px rgba(236, 72, 153, 0.3);
            border: 1px solid rgba(236, 72, 153, 0.8);
        }

        .glow-purple {
            box-shadow: 0 0 20px rgba(168, 85, 247, 0.5), inset 0 0 10px rgba(168, 85, 247, 0.3);
            border: 1px solid rgba(168, 85, 247, 0.8);
        }

        .glow-yellow {
            box-shadow: 0 0 20px rgba(234, 179, 8, 0.5), inset 0 0 10px rgba(234, 179, 8, 0.3);
            border: 1px solid rgba(234, 179, 8, 0.8);
        }

        .text-glow-cyan {
            text-shadow: 0 0 12px rgba(6, 182, 212, 0.8);
        }

        .text-glow-pink {
            text-shadow: 0 0 12px rgba(236, 72, 153, 0.8);
        }

        .glass-panel {
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .cyber-btn {
            position: relative;
            transition: all 0.2s ease;
            overflow: hidden;
        }

        .cyber-btn:hover {
            transform: translateY(-2px);
            filter: brightness(1.2);
        }

        .cyber-btn:active {
            transform: translateY(1px);
        }

        .cd-mask {
            transition: height 0.1s linear;
        }

        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(15, 23, 42, 0.6);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(6, 182, 212, 0.5);
            border-radius: 3px;
        }
    </style>
</head>
<body class="relative flex flex-col items-center justify-center min-h-screen select-none">

    <!-- 遊戲主要畫布 -->
    <canvas id="gameCanvas" class="absolute inset-0 w-full h-full z-0 block cursor-crosshair"></canvas>

    <!-- 頂部 HUD 狀態欄 -->
    <div id="hud" class="absolute top-4 left-4 right-4 z-10 flex justify-between items-center pointer-events-none max-w-7xl mx-auto w-full px-2 hidden">
        <!-- 核心生命值與經驗 -->
        <div class="glass-panel rounded-2xl p-3 sm:p-4 flex items-center gap-4 border border-cyan-500/30 shadow-2xl pointer-events-auto">
            <div id="heroBadge" class="relative w-12 h-12 sm:w-14 sm:h-14 rounded-xl bg-gradient-to-tr from-cyan-600 to-blue-600 flex flex-col items-center justify-center text-white font-orbitron border border-cyan-300 shadow-inner">
                <span class="text-[9px] uppercase tracking-wider text-cyan-200">LVL</span>
                <span id="playerLevel" class="text-xl font-black">1</span>
            </div>
            <div class="flex flex-col gap-1.5 w-36 sm:w-56">
                <div class="flex justify-between text-xs font-bold text-cyan-300">
                    <span class="flex items-center gap-1">
                        <i class="fa-solid fa-shield-halved text-cyan-400"></i><span id="heroNameHUD">星核守護者</span>
                        <span id="shieldTag" class="hidden text-[10px] text-blue-300 bg-blue-500/30 px-1 rounded border border-blue-400/50"></span>
                    </span>
                    <span id="hpText" class="font-orbitron">1000 / 1000</span>
                </div>
                <div class="w-full h-3 bg-gray-900/90 rounded-full overflow-hidden border border-cyan-500/30 relative">
                    <div id="hpBar" class="h-full bg-gradient-to-r from-cyan-500 to-blue-500 w-full transition-all duration-200 shadow-lg shadow-cyan-500/50"></div>
                </div>
                <div class="flex justify-between text-[10px] font-semibold text-purple-300">
                    <span><i class="fa-solid fa-atom text-purple-400 mr-1"></i>晶核採集</span>
                    <span id="expText" class="font-orbitron">0%</span>
                </div>
                <div class="w-full h-1.5 bg-gray-900/90 rounded-full overflow-hidden border border-purple-500/30">
                    <div id="expBar" class="h-full bg-gradient-to-r from-purple-500 to-pink-500 w-0 transition-all duration-200"></div>
                </div>
            </div>
        </div>

        <!-- 關卡與統計資料 -->
        <div class="glass-panel rounded-2xl px-5 py-3 flex items-center gap-4 sm:gap-8 border border-pink-500/30 text-center pointer-events-auto">
            <div>
                <div class="text-[10px] uppercase tracking-wider text-pink-400 font-bold">目前波次</div>
                <div id="waveText" class="font-orbitron text-xl sm:text-2xl font-black text-white text-glow-pink">WAVE 1/10</div>
            </div>
            <div class="h-8 w-px bg-gray-700"></div>
            <div>
                <div class="text-[10px] uppercase tracking-wider text-cyan-400 font-bold">戰術得分</div>
                <div id="scoreText" class="font-orbitron text-xl sm:text-2xl font-black text-cyan-400 text-glow-cyan">0</div>
            </div>
            <div class="h-8 w-px bg-gray-700 hidden sm:block"></div>
            <div class="hidden sm:block">
                <div class="text-[10px] uppercase tracking-wider text-yellow-400 font-bold">戰術點數</div>
                <div id="creditText" class="font-orbitron text-xl sm:text-2xl font-black text-yellow-300"><i class="fa-solid fa-microchip text-sm mr-1"></i>0</div>
            </div>
        </div>

        <!-- 功能按鈕 -->
        <div class="flex gap-2 pointer-events-auto">
            <button id="btnPause" class="glass-panel w-11 h-11 sm:w-12 sm:h-12 rounded-xl flex items-center justify-center text-cyan-400 hover:bg-cyan-500/20 border border-cyan-500/40 transition-all active:scale-95">
                <i class="fa-solid fa-pause text-lg"></i>
            </button>
            <button id="btnAudio" class="glass-panel w-11 h-11 sm:w-12 sm:h-12 rounded-xl flex items-center justify-center text-purple-400 hover:bg-purple-500/20 border border-purple-500/40 transition-all active:scale-95">
                <i id="audioIcon" class="fa-solid fa-volume-high text-lg"></i>
            </button>
        </div>
    </div>

    <!-- 活躍 Buff 狀態提示欄 -->
    <div id="buffContainer" class="absolute top-24 z-10 flex gap-2 pointer-events-none"></div>

    <!-- 底部主動技能列與防禦塔建設 -->
    <div id="skillBar" class="absolute bottom-6 z-10 flex items-center gap-3 sm:gap-6 pointer-events-auto hidden">
        <div class="flex flex-col items-center">
            <button id="btnSkill1" class="glass-panel w-14 h-14 sm:w-16 sm:h-16 rounded-2xl flex flex-col items-center justify-center text-cyan-400 border border-cyan-500/50 hover:bg-cyan-500/20 active:scale-95 transition-all relative overflow-hidden group">
                <i class="fa-solid fa-wind text-xl mb-0.5 group-hover:scale-110 transition-transform"></i>
                <span class="text-[9px] font-orbitron font-bold text-cyan-200">DASH [Space]</span>
                <div id="cdSkill1" class="cd-mask absolute inset-x-0 bottom-0 bg-black/80 h-0"></div>
            </button>
        </div>

        <div class="flex flex-col items-center">
            <button id="btnSkill2" class="glass-panel w-14 h-14 sm:w-16 sm:h-16 rounded-2xl flex flex-col items-center justify-center text-pink-400 border border-pink-500/50 hover:bg-pink-500/20 active:scale-95 transition-all relative overflow-hidden group">
                <i class="fa-solid fa-burst text-xl mb-0.5 group-hover:scale-110 transition-transform"></i>
                <span class="text-[9px] font-orbitron font-bold text-pink-200">EMP [E]</span>
                <div id="cdSkill2" class="cd-mask absolute inset-x-0 bottom-0 bg-black/80 h-0"></div>
            </button>
        </div>

        <div class="flex flex-col items-center">
            <button id="btnBuildTurret" class="glass-panel w-14 h-14 sm:w-16 sm:h-16 rounded-2xl flex flex-col items-center justify-center text-yellow-400 border border-yellow-500/50 hover:bg-yellow-500/20 active:scale-95 transition-all relative overflow-hidden group">
                <i class="fa-solid fa-tower-cell text-xl mb-0.5 group-hover:scale-110 transition-transform"></i>
                <span class="text-[9px] font-orbitron font-bold text-yellow-200">TURRET [Q]</span>
                <span id="turretCountTag" class="text-[9px] font-orbitron font-bold text-yellow-400 mt-0.5">0 / 10 (50pt)</span>
            </button>
        </div>
    </div>

    <!-- 槍管過熱指示條 UI -->
    <div id="overheatContainer" class="absolute bottom-24 z-10 flex flex-col items-center gap-1 pointer-events-none transition-opacity duration-200 opacity-0">
        <div id="overheatTitle" class="text-[10px] font-orbitron font-bold text-yellow-400 flex items-center gap-1">
            <i class="fa-solid fa-temperature-full"></i> <span id="overheatText">槍管熱能</span>
        </div>
        <div class="w-48 h-2 bg-gray-900/90 rounded-full border border-yellow-500/40 overflow-hidden">
            <div id="overheatBar" class="h-full bg-gradient-to-r from-yellow-500 via-orange-500 to-red-600 w-0 transition-all duration-75"></div>
        </div>
    </div>

    <!-- 首頁與選角 Modal -->
    <div id="startModal" class="absolute inset-0 z-30 flex items-center justify-center bg-black/85 backdrop-blur-md overflow-y-auto p-4">
        <div class="glass-panel max-w-4xl w-full p-6 sm:p-8 rounded-3xl border border-cyan-500/50 glow-cyan text-center flex flex-col items-center gap-6 my-auto">
            <!-- 標題與星星幣貨幣顯示 -->
            <div class="flex flex-col sm:flex-row justify-between items-center w-full gap-4">
                <div class="text-left">
                    <h1 class="text-3xl sm:text-4xl font-black font-orbitron text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-pink-400 to-purple-400">星核守護者</h1>
                    <p class="text-xs uppercase tracking-widest text-cyan-300 font-orbitron font-semibold">LAST DAWN - HEROES OF THE CORE</p>
                </div>
                <div class="glass-panel px-4 py-2 rounded-2xl border border-yellow-500/50 flex items-center gap-2">
                    <i class="fa-solid fa-star text-yellow-400 text-xl animate-spin" style="animation-duration: 8s;"></i>
                    <span class="text-xs text-yellow-200 font-orbitron">星星幣:</span>
                    <span id="starCoinText" class="text-xl font-black font-orbitron text-yellow-400">0</span>
                </div>
            </div>

            <!-- 角色選擇區域 -->
            <div class="w-full">
                <h2 class="text-lg font-bold font-orbitron text-cyan-300 mb-3 text-left flex items-center gap-2">
                    <i class="fa-solid fa-user-gear"></i> 選擇戰術機甲 (CHOOSE HERO)
                </h2>
                <div id="heroCardContainer" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                    <!-- 動態注入角色卡片 -->
                </div>
            </div>

            <div class="w-full bg-gray-900/60 p-4 rounded-xl border border-gray-800 text-left text-xs text-gray-400 flex flex-wrap gap-4 justify-between">
                <div><i class="fa-solid fa-keyboard text-cyan-400 mr-1"></i> <strong class="text-gray-200">WASD / 鍵盤</strong>：移動</div>
                <div><i class="fa-solid fa-mouse text-pink-400 mr-1"></i> <strong class="text-gray-200">長按左鍵</strong>：連續射擊</div>
                <div><i class="fa-solid fa-trophy text-yellow-400 mr-1"></i> <strong class="text-gray-200">通關條件</strong>：生存並擊敗第 10 波 Boss！</div>
            </div>

            <button id="btnStartGame" class="w-full py-4 rounded-2xl bg-gradient-to-r from-cyan-500 via-purple-600 to-pink-500 text-white font-orbitron font-bold text-lg tracking-wider cyber-btn shadow-lg shadow-purple-500/30">
                出擊 (DEPLOY SELECTED HERO)
            </button>
        </div>
    </div>

    <!-- 升級天賦選擇 Modal -->
    <div id="upgradeModal" class="absolute inset-0 z-30 hidden flex items-center justify-center bg-black/80 backdrop-blur-md">
        <div class="glass-panel max-w-2xl w-full mx-4 p-6 sm:p-8 rounded-3xl border border-purple-500/50 glow-purple text-center">
            <div class="inline-block px-4 py-1 rounded-full bg-purple-500/20 border border-purple-500/40 text-purple-300 font-orbitron text-xs font-bold mb-2">
                LEVEL UPGRADE AVAILABLE
            </div>
            <h2 class="text-2xl sm:text-3xl font-black font-orbitron text-white mb-6">選擇戰術升級模組</h2>
            <div id="upgradeOptionsContainer" class="grid grid-cols-1 sm:grid-cols-3 gap-4"></div>
        </div>
    </div>

    <!-- 遊戲結束/勝利 Modal -->
    <div id="gameOverModal" class="absolute inset-0 z-30 hidden flex items-center justify-center bg-black/90 backdrop-blur-md">
        <div class="glass-panel max-w-md w-full mx-4 p-8 rounded-3xl border border-pink-500/50 glow-pink text-center flex flex-col items-center gap-6">
            <div id="gameOverIcon" class="w-20 h-20 rounded-2xl bg-red-500/20 border border-red-500/50 flex items-center justify-center text-red-500 text-4xl shadow-lg shadow-red-500/30">
                <i class="fa-solid fa-skull"></i>
            </div>
            <div>
                <h2 id="gameOverTitle" class="text-3xl font-black font-orbitron text-red-500 mb-1">星核崩毀</h2>
                <p id="gameOverSubTitle" class="text-xs uppercase tracking-widest text-gray-400 font-orbitron">SYSTEM OVERRIDE DETECTED</p>
            </div>

            <div class="w-full bg-gray-900/80 rounded-2xl p-4 border border-gray-800 flex flex-col gap-3">
                <div class="flex justify-between items-center text-sm">
                    <span class="text-gray-400">生存波次</span>
                    <span id="finalWave" class="font-orbitron font-bold text-pink-400 text-lg">WAVE 0</span>
                </div>
                <div class="flex justify-between items-center text-sm">
                    <span class="text-gray-400">最終戰術得分</span>
                    <span id="finalScore" class="font-orbitron font-bold text-cyan-400 text-lg">0</span>
                </div>
                <div class="flex justify-between items-center text-sm">
                    <span class="text-gray-400">獲得星星幣</span>
                    <span id="earnedCoins" class="font-orbitron font-bold text-yellow-400 text-xl"><i class="fa-solid fa-star mr-1"></i>0</span>
                </div>
            </div>

            <button id="btnRestartGame" class="w-full py-4 rounded-2xl bg-gradient-to-r from-pink-500 to-purple-600 text-white font-orbitron font-bold text-lg tracking-wider cyber-btn shadow-lg shadow-pink-500/30">
                返回大廳 (RETURN TO BASE)
            </button>
        </div>
    </div>

    <script>
        class SoundEngine {
            constructor() {
                this.ctx = null;
                this.muted = false;
            }

            init() {
                if (!this.ctx) {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                }
            }

            playShoot() {
                if (this.muted || !this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(600, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(100, this.ctx.currentTime + 0.08);
                gain.gain.setValueAtTime(0.15, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.08);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.08);
            }

            playExplosion() {
                if (this.muted || !this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'square';
                osc.frequency.setValueAtTime(120, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(20, this.ctx.currentTime + 0.3);
                gain.gain.setValueAtTime(0.3, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.3);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.3);
            }

            playPowerup() {
                if (this.muted || !this.ctx) return;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(300, this.ctx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(900, this.ctx.currentTime + 0.25);
                gain.gain.setValueAtTime(0.2, this.ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.25);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start();
                osc.stop(this.ctx.currentTime + 0.25);
            }
        }

        const audioManager = new SoundEngine();

        // Canvas 設定
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // 星星幣與解鎖資料 (持久化)
        let starCoins = parseInt(localStorage.getItem('sentinel_star_coins')) || 0;
        let unlockedHeroes = JSON.parse(localStorage.getItem('sentinel_unlocked_heroes')) || ['default'];

        function saveGameProgress() {
            localStorage.setItem('sentinel_star_coins', starCoins);
            localStorage.setItem('sentinel_unlocked_heroes', JSON.stringify(unlockedHeroes));
        }

        // 角色設定資料庫
        const HEROES = {
            default: {
                id: 'default',
                name: '星核守護者',
                role: '標準型',
                icon: 'fa-shield-cat',
                color: '#06b6d4',
                price: 0,
                hp: 1000,
                speed: 4.5,
                bulletDamage: 35,
                bulletSpeed: 12,
                fireRate: 150,
                maxTurrets: 10,
                canContinuousFire: true,
                range: Infinity,
                desc: '各項屬性均衡，擁有穩定的連發槍管系統。'
            },
            tank: {
                id: 'tank',
                name: '重裝泰坦',
                role: '坦克型',
                icon: 'fa-shield-halved',
                color: '#f59e0b',
                price: 300,
                hp: 2500,
                speed: 3.2,
                bulletDamage: 280,
                bulletSpeed: 18,
                fireRate: 700,
                maxTurrets: 10,
                canContinuousFire: false, // 不可連發
                range: 1200, // 射程長
                desc: '高血量且射程極遠。無法連發，但重型單發傷害極高與強力擊退！'
            },
            assassin: {
                id: 'assassin',
                name: '幻影刺客',
                role: '刺客型',
                icon: 'fa-bolt',
                color: '#ec4899',
                price: 500,
                hp: 750,
                speed: 6.8,
                bulletDamage: 45,
                bulletSpeed: 14,
                fireRate: 120,
                maxTurrets: 10,
                canContinuousFire: true,
                range: 220, // 射程短
                isDistanceDamage: true, // 越近傷害越高
                desc: '極速機動但射程短。距離敵人越近，傷害最高可加成至 300%！'
            },
            mage: {
                id: 'mage',
                name: '秘法大導師',
                role: '法師型',
                icon: 'fa-hat-wizard',
                color: '#a855f7',
                price: 800,
                hp: 600,
                speed: 4.0,
                bulletDamage: 40,
                bulletSpeed: 7, // 子彈慢
                fireRate: 200,
                maxTurrets: 20, // 上限乘2 (20座)
                canContinuousFire: true,
                range: Infinity, // 射程無限
                isSlowControl: true, // 附帶控場
                desc: '血量低、子彈慢但射程無限，子彈命中可減速敵人，且防禦塔上限翻倍 (20座)！'
            }
        };

        let selectedHeroId = 'default';

        // 遊戲狀態與控制變數
        let gameState = 'MENU';
        let mousePos = { x: canvas.width / 2, y: canvas.height / 2 };
        let keys = {};
        let isMouseDown = false; 

        // 玩家物件數據
        let player = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 20,
            speed: 4.5,
            hp: 1000,
            maxHp: 1000,
            shield: 0,
            level: 1,
            exp: 0,
            nextExp: 100,
            score: 0,
            credits: 0,
            kills: 0,
            fireRate: 150,
            lastFire: 0,
            bulletDamage: 35,
            bulletSpeed: 12,
            knockback: 6,
            multishot: 1,
            dashCd: 0,
            empCd: 0,
            dashMaxCd: 3000,
            empMaxCd: 12000,
            isDashing: false,
            dashDuration: 0,
            
            // 天賦與過熱
            is360Shot: false,
            gravityBullet: false,
            heatMult: 1.0,
            continuousFireTime: 0,
            maxContinuousFire: 5000,
            isOverheated: false,
            overheatCooldown: 0,
            mustReleaseTrigger: false,

            // 道具
            autoAimTime: 0,
            slowDownTime: 0,
            infiniteHeatTime: 0
        };

        let acquiredLuckyUpgrades = new Set();
        let bullets = [];
        let enemies = [];
        let particles = [];
        let turrets = [];
        let items = [];
        let wave = 1;
        const MAX_WAVE = 10; // 固定每場 10 波
        let enemySpawnTimer = 0;
        let itemSpawnTimer = 0;
        let enemiesToSpawn = 10;
        let spawnedEnemiesCount = 0;

        window.addEventListener('keydown', (e) => {
            keys[e.code] = true;
            if (e.code === 'Space') useDash();
            if (e.code === 'KeyE') useEMP();
            if (e.code === 'KeyQ') buildTurret();
        });

        window.addEventListener('keyup', (e) => {
            keys[e.code] = false;
        });

        canvas.addEventListener('mousemove', (e) => {
            mousePos.x = e.clientX;
            mousePos.y = e.clientY;
        });

        canvas.addEventListener('mousedown', (e) => {
            if (e.button === 0) {
                isMouseDown = true;
            }
        });

        window.addEventListener('mouseup', (e) => {
            if (e.button === 0) {
                isMouseDown = false;
                player.mustReleaseTrigger = false;
            }
        });

        canvas.addEventListener('touchstart', (e) => {
            if (e.touches.length > 0) {
                isMouseDown = true;
                mousePos.x = e.touches[0].clientX;
                mousePos.y = e.touches[0].clientY;
            }
        });

        canvas.addEventListener('touchmove', (e) => {
            if (e.touches.length > 0) {
                mousePos.x = e.touches[0].clientX;
                mousePos.y = e.touches[0].clientY;
            }
        });

        window.addEventListener('touchend', () => {
            isMouseDown = false;
            player.mustReleaseTrigger = false;
        });

        // 渲染角色選擇卡片 UI
        function renderHeroCards() {
            document.getElementById('starCoinText').innerText = starCoins;
            const container = document.getElementById('heroCardContainer');
            container.innerHTML = '';

            Object.values(HEROES).forEach(hero => {
                const isUnlocked = unlockedHeroes.includes(hero.id);
                const isSelected = selectedHeroId === hero.id;

                const card = document.createElement('div');
                card.className = `glass-panel p-4 rounded-2xl flex flex-col justify-between items-center text-center border cursor-pointer transition-all ${
                    isSelected ? 'border-cyan-400 glow-cyan bg-cyan-500/10 scale-105' : 'border-gray-800 hover:border-gray-600'
                }`;

                card.innerHTML = `
                    <div class="flex flex-col items-center gap-2 w-full">
                        <div class="w-12 h-12 rounded-xl flex items-center justify-center text-2xl text-white shadow-md" style="background-color: ${hero.color}">
                            <i class="fa-solid ${hero.icon}"></i>
                        </div>
                        <div class="font-black font-orbitron text-sm text-white">${hero.name}</div>
                        <span class="text-[10px] font-orbitron px-2 py-0.5 rounded-full border border-gray-700 text-gray-300">${hero.role}</span>
                        <p class="text-[11px] text-gray-400 leading-tight my-2 h-12 flex items-center justify-center">${hero.desc}</p>
                    </div>

                    <div class="w-full mt-2">
                        ${isUnlocked ? `
                            <button class="w-full py-2 rounded-xl text-xs font-bold font-orbitron ${isSelected ? 'bg-cyan-500 text-black' : 'bg-gray-800 text-gray-300 hover:bg-gray-700'}">
                                ${isSelected ? '已選擇 (SELECTED)' : '選擇機甲'}
                            </button>
                        ` : `
                            <button class="btnUnlockHero w-full py-2 rounded-xl bg-yellow-500/20 hover:bg-yellow-500/40 border border-yellow-500/50 text-yellow-300 text-xs font-bold font-orbitron flex items-center justify-center gap-1">
                                <i class="fa-solid fa-lock text-xs"></i> 解鎖 (${hero.price} <i class="fa-solid fa-star text-[10px]"></i>)
                            </button>
                        `}
                    </div>
                `;

                card.onclick = (e) => {
                    if (!isUnlocked) {
                        if (starCoins >= hero.price) {
                            starCoins -= hero.price;
                            unlockedHeroes.push(hero.id);
                            selectedHeroId = hero.id;
                            saveGameProgress();
                            renderHeroCards();
                            audioManager.playPowerup();
                        } else {
                            alert('星星幣不足！完成戰鬥關卡可獲得星星幣！');
                        }
                    } else {
                        selectedHeroId = hero.id;
                        renderHeroCards();
                    }
                };

                container.appendChild(card);
            });
        }

        document.getElementById('btnStartGame').addEventListener('click', () => {
            audioManager.init();
            document.getElementById('startModal').classList.add('hidden');
            document.getElementById('hud').classList.remove('hidden');
            document.getElementById('skillBar').classList.remove('hidden');
            resetGame();
            gameState = 'PLAYING';
        });

        document.getElementById('btnRestartGame').addEventListener('click', () => {
            document.getElementById('gameOverModal').classList.add('hidden');
            document.getElementById('hud').classList.add('hidden');
            document.getElementById('skillBar').classList.add('hidden');
            document.getElementById('startModal').classList.remove('hidden');
            renderHeroCards();
            gameState = 'MENU';
        });

        document.getElementById('btnPause').addEventListener('click', () => {
            if (gameState === 'PLAYING') gameState = 'PAUSED';
            else if (gameState === 'PAUSED') gameState = 'PLAYING';
        });

        document.getElementById('btnAudio').addEventListener('click', () => {
            audioManager.muted = !audioManager.muted;
            document.getElementById('audioIcon').className = audioManager.muted ? 'fa-solid fa-volume-xmark text-lg text-red-400' : 'fa-solid fa-volume-high text-lg text-purple-400';
        });

        document.getElementById('btnSkill1').addEventListener('click', useDash);
        document.getElementById('btnSkill2').addEventListener('click', useEMP);
        document.getElementById('btnBuildTurret').addEventListener('click', buildTurret);

        function useDash() {
            if (gameState !== 'PLAYING' || player.dashCd > 0) return;
            player.dashCd = player.dashMaxCd;
            player.isDashing = true;
            player.dashDuration = 12;
            audioManager.playPowerup();
            for (let i = 0; i < 20; i++) {
                particles.push(new Particle(player.x, player.y, '#06b6d4', 4, 3));
            }
        }

        function useEMP() {
            if (gameState !== 'PLAYING' || player.empCd > 0) return;
            player.empCd = player.empMaxCd;
            audioManager.playExplosion();
            
            // 修復 E 鍵技能：給予 EMP 傷害並檢測擊殺與更新 HUD
            for (let i = enemies.length - 1; i >= 0; i--) {
                const enemy = enemies[i];
                const dist = Math.hypot(enemy.x - player.x, enemy.y - player.y);
                if (dist < 400) {
                    enemy.hp -= 300;
                    if (enemy.hp <= 0) {
                        player.score += enemy.scoreValue;
                        player.credits += Math.floor(enemy.scoreValue / 10);
                        player.kills++;
                        gainExp(enemy.expValue);
                        enemies.splice(i, 1);
                    }
                }
            }
            updateHUD();

            for (let i = 0; i < 60; i++) {
                const angle = (Math.PI * 2 / 60) * i;
                particles.push(new Particle(player.x, player.y, '#ec4899', 6, 8, Math.cos(angle) * 10, Math.sin(angle) * 10));
            }
        }

        function buildTurret() {
            const hero = HEROES[selectedHeroId];
            if (gameState !== 'PLAYING' || player.credits < 50) return;
            if (turrets.length >= hero.maxTurrets) return;

            player.credits -= 50;
            turrets.push({
                x: player.x,
                y: player.y,
                radius: 16,
                lastFire: 0,
                fireRate: 400,
                range: 250
            });
            audioManager.playPowerup();
            updateHUD();
        }

        class Item {
            constructor(type) {
                this.x = Math.random() * (canvas.width - 120) + 60;
                this.y = Math.random() * (canvas.height - 120) + 60;
                this.radius = 16;
                this.type = type; 
                this.duration = 40000; // 道具場上存活時間：40 秒 (毫秒)
                this.lifeTimer = 0;

                // 道具種類與顏色樣式
                switch(type) {
                    case 'autoAim':
                        this.color = '#eab308'; // 黃色
                        this.icon = '🎯';
                        break;
                    case 'slowDown':
                        this.color = '#3b82f6'; // 藍色
                        this.icon = '❄️';
                        break;
                    case 'heal':
                        this.color = '#22c55e'; // 綠色
                        this.icon = '❤️';
                        break;
                    case 'shield':
                        this.color = '#06b6d4'; // 青色
                        this.icon = '🛡️';
                        break;
                    case 'nuke':
                        this.color = '#ef4444'; // 紅色
                        this.icon = '💣';
                        break;
                    case 'instantCool':
                        this.color = '#f97316'; // 橘色
                        this.icon = '⚡';
                        break;
                }
            }

            update(dt) {
                this.lifeTimer += dt;
                return this.lifeTimer >= this.duration; // 超過 40 秒返回 true (銷毀)
            }

            draw() {
                const timeLeft = this.duration - this.lifeTimer;
                // 當時間小於 10 秒時，開始閃爍提醒
                if (timeLeft < 10000 && Math.floor(Date.now() / 200) % 2 === 0) {
                    return;
                }

                ctx.save();
                ctx.shadowBlur = 15;
                ctx.shadowColor = this.color;
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                ctx.fill();
                ctx.strokeStyle = '#ffffff';
                ctx.lineWidth = 2;
                ctx.stroke();

                // 繪製道具圖標
                ctx.font = '12px sans-serif';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText(this.icon, this.x, this.y);
                ctx.restore();
            }
        }

        class Particle {
            constructor(x, y, color, size, speed, vx = null, vy = null) {
                this.x = x;
                this.y = y;
                this.color = color;
                this.size = Math.random() * size + 1;
                this.alpha = 1;
                const angle = Math.random() * Math.PI * 2;
                const spd = Math.random() * speed;
                this.vx = vx !== null ? vx : Math.cos(angle) * spd;
                this.vy = vy !== null ? vy : Math.sin(angle) * spd;
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;
                this.alpha -= 0.02;
            }

            draw() {
                ctx.save();
                ctx.globalAlpha = Math.max(0, this.alpha);
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }
        }

        class Enemy {
            constructor(type) {
                if (Math.random() < 0.5) {
                    this.x = Math.random() < 0.5 ? -30 : canvas.width + 30;
                    this.y = Math.random() * canvas.height;
                } else {
                    this.x = Math.random() * canvas.width;
                    this.y = Math.random() < 0.5 ? -30 : canvas.height + 30;
                }

                this.type = type;
                this.slowFactor = 1.0;

                if (type === 'boss') {
                    this.radius = 42;
                    this.hp = 1500 + wave * 400;
                    this.maxHp = this.hp;
                    this.baseSpeed = 0.7;
                    this.color = '#ec4899';
                    this.scoreValue = 500;
                    this.expValue = 100;
                } else if (type === 'tank') {
                    this.radius = 24;
                    this.hp = 250 + wave * 50;
                    this.maxHp = this.hp;
                    this.baseSpeed = 1.8;
                    this.color = '#a855f7';
                    this.scoreValue = 100;
                    this.expValue = 30;
                } else {
                    this.radius = 14;
                    this.hp = 60 + wave * 18;
                    this.maxHp = this.hp;
                    this.baseSpeed = 3.2;
                    this.color = '#06b6d4';
                    this.scoreValue = 30;
                    this.expValue = 12;
                }
                this.speed = this.baseSpeed;
            }

            update() {
                const speedMult = (player.slowDownTime > 0 ? 0.4 : 1.0) * this.slowFactor;
                this.speed = this.baseSpeed * speedMult;

                const angle = Math.atan2(player.y - this.y, player.x - this.x);
                this.x += Math.cos(angle) * this.speed;
                this.y += Math.sin(angle) * this.speed;

                // 逐漸恢復法師控場緩速
                if (this.slowFactor < 1.0) {
                    this.slowFactor = Math.min(1.0, this.slowFactor + 0.005);
                }
            }

            draw() {
                ctx.save();
                ctx.shadowBlur = 15;
                ctx.shadowColor = this.slowFactor < 1.0 ? '#a855f7' : this.color;
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                ctx.fill();

                if (this.hp < this.maxHp) {
                    const barW = this.radius * 2;
                    const barH = 4;
                    ctx.fillStyle = 'rgba(0, 0, 0, 0.5)';
                    ctx.fillRect(this.x - this.radius, this.y - this.radius - 10, barW, barH);
                    ctx.fillStyle = '#22c55e';
                    ctx.fillRect(this.x - this.radius, this.y - this.radius - 10, barW * (this.hp / this.maxHp), barH);
                }
                ctx.restore();
            }
        }

        function resetGame() {
            const hero = HEROES[selectedHeroId];

            player.x = canvas.width / 2;
            player.y = canvas.height / 2;
            player.maxHp = hero.hp;
            player.hp = hero.hp;
            player.speed = hero.speed;
            player.bulletDamage = hero.bulletDamage;
            player.bulletSpeed = hero.bulletSpeed;
            player.fireRate = hero.fireRate;
            player.multishot = 1; // 重置多重彈幕數量
            player.knockback = 6;
            player.shield = 0;
            player.level = 1;
            player.exp = 0;
            player.nextExp = 100;
            player.score = 0;
            player.credits = 0;
            player.kills = 0;
            player.dashCd = 0;
            player.empCd = 0;
            player.is360Shot = false; // 重置 360 度散射
            player.gravityBullet = false; // 重置黑洞引力彈
            player.heatMult = 1.0;
            player.continuousFireTime = 0;
            player.isOverheated = false;
            player.overheatCooldown = 0;
            player.mustReleaseTrigger = false;
            player.autoAimTime = 0;
            player.slowDownTime = 0;
            player.infiniteHeatTime = 0;

            acquiredLuckyUpgrades.clear(); // 清空幸運升級紀錄
            bullets = [];
            enemies = [];
            particles = [];
            turrets = [];
            items = [];
            wave = 1;
            enemySpawnTimer = 0;
            itemSpawnTimer = 0;
            enemiesToSpawn = 10;
            spawnedEnemiesCount = 0;

            document.getElementById('heroNameHUD').innerText = hero.name;
            updateHUD();
        }

        function shootBullet() {
            const hero = HEROES[selectedHeroId];
            const now = Date.now();
            if (player.isOverheated || player.mustReleaseTrigger || now - player.lastFire < player.fireRate) return;

            player.lastFire = now;
            audioManager.playShoot();

            let targetAngle = Math.atan2(mousePos.y - player.y, mousePos.x - player.x);

            if (player.autoAimTime > 0 && enemies.length > 0) {
                let nearest = null;
                let minDist = Infinity;
                enemies.forEach(e => {
                    const d = Math.hypot(e.x - player.x, e.y - player.y);
                    if (d < minDist) {
                        minDist = d;
                        nearest = e;
                    }
                });
                if (nearest) targetAngle = Math.atan2(nearest.y - player.y, nearest.x - player.x);
            }

            const baseDamage = player.bulletDamage;

            if (player.is360Shot) {
                const count = 12;
                for (let i = 0; i < count; i++) {
                    const a = (Math.PI * 2 / count) * i;
                    bullets.push({
                        x: player.x,
                        y: player.y,
                        startX: player.x,
                        startY: player.y,
                        vx: Math.cos(a) * player.bulletSpeed,
                        vy: Math.sin(a) * player.bulletSpeed,
                        damage: baseDamage,
                        maxRange: hero.range,
                        radius: 5,
                        color: hero.color,
                        isAutoAim: player.autoAimTime > 0,
                        isGravity: player.gravityBullet
                    });
                }
            } else {
                const spreadCount = player.multishot;
                const spreadAngle = 0.15;
                const startAngle = targetAngle - ((spreadCount - 1) * spreadAngle) / 2;

                for (let i = 0; i < spreadCount; i++) {
                    const currentAngle = startAngle + i * spreadAngle;
                    bullets.push({
                        x: player.x,
                        y: player.y,
                        startX: player.x,
                        startY: player.y,
                        vx: Math.cos(currentAngle) * player.bulletSpeed,
                        vy: Math.sin(currentAngle) * player.bulletSpeed,
                        damage: baseDamage,
                        maxRange: hero.range,
                        radius: player.gravityBullet ? 7 : (hero.id === 'tank' ? 8 : 4),
                        color: hero.color,
                        isAutoAim: player.autoAimTime > 0,
                        isGravity: player.gravityBullet
                    });
                }
            }

            // 坦克無法連發：單發後強制要求放開按鈕
            if (!hero.canContinuousFire) {
                player.mustReleaseTrigger = true;
            }
        }

        function triggerOverheatBurst() {
            player.isOverheated = true;
            player.overheatCooldown = 1500;
            player.mustReleaseTrigger = true;
            audioManager.playExplosion();
            for (let i = 0; i < 30; i++) {
                particles.push(new Particle(player.x, player.y, '#ef4444', 5, 6));
            }
        }

        function update() {
            if (gameState !== 'PLAYING') return;

            const hero = HEROES[selectedHeroId];
            const dt = 16; 

            if (player.autoAimTime > 0) player.autoAimTime = Math.max(0, player.autoAimTime - dt);
            if (player.slowDownTime > 0) player.slowDownTime = Math.max(0, player.slowDownTime - dt);
            if (player.infiniteHeatTime > 0) player.infiniteHeatTime = Math.max(0, player.infiniteHeatTime - dt);

            const buffContainer = document.getElementById('buffContainer');
            let buffHTML = '';
            if (player.autoAimTime > 0) {
                buffHTML += `<div class="glass-panel px-3 py-1 rounded-full border border-yellow-500/60 text-yellow-300 font-orbitron text-xs flex items-center gap-1.5"><i class="fa-solid fa-crosshair"></i> 自動瞄準 (${(player.autoAimTime/1000).toFixed(1)}s)</div>`;
            }
            if (player.slowDownTime > 0) {
                buffHTML += `<div class="glass-panel px-3 py-1 rounded-full border border-blue-500/60 text-blue-300 font-orbitron text-xs flex items-center gap-1.5"><i class="fa-solid fa-snowflake"></i> 全域緩速 (${(player.slowDownTime/1000).toFixed(1)}s)</div>`;
            }
            if (player.infiniteHeatTime > 0) {
                buffHTML += `<div class="glass-panel px-3 py-1 rounded-full border border-orange-500/60 text-orange-300 font-orbitron text-xs flex items-center gap-1.5"><i class="fa-solid fa-bolt"></i> 無限射擊 (${(player.infiniteHeatTime/1000).toFixed(1)}s)</div>`;
            }
            buffContainer.innerHTML = buffHTML;

            if (player.dashCd > 0) player.dashCd = Math.max(0, player.dashCd - dt);
            if (player.empCd > 0) player.empCd = Math.max(0, player.empCd - dt);

            document.getElementById('cdSkill1').style.height = (player.dashCd / player.dashMaxCd * 100) + '%';
            document.getElementById('cdSkill2').style.height = (player.empCd / player.empMaxCd * 100) + '%';

            const overheatContainer = document.getElementById('overheatContainer');
            const overheatBar = document.getElementById('overheatBar');
            const overheatTitle = document.getElementById('overheatTitle');

            // 槍管熱能更新
            if (hero.canContinuousFire) {
                if (player.isOverheated) {
                    player.overheatCooldown -= dt;
                    overheatContainer.style.opacity = '1';
                    overheatBar.style.width = (player.overheatCooldown / 1500 * 100) + '%';
                    overheatTitle.innerHTML = `<i class="fa-solid fa-fire text-red-500 animate-pulse"></i> <span class="text-red-400">過熱冷卻中...</span>`;

                    if (player.overheatCooldown <= 0) {
                        player.isOverheated = false;
                        player.continuousFireTime = 0;
                        overheatContainer.style.opacity = '0';
                    }
                } else if (isMouseDown) {
                    if (!player.mustReleaseTrigger) {
                        if (player.infiniteHeatTime <= 0) {
                            player.continuousFireTime += dt * player.heatMult;
                        }
                        shootBullet();
                        overheatContainer.style.opacity = '1';
                        overheatTitle.innerHTML = player.infiniteHeatTime > 0 ? `<i class="fa-solid fa-bolt text-orange-400 animate-bounce"></i> <span class="text-orange-300">無限散熱中</span>` : `<i class="fa-solid fa-temperature-full"></i> <span>槍管熱能</span>`;
                        overheatBar.style.width = Math.min(100, (player.continuousFireTime / player.maxContinuousFire * 100)) + '%';

                        if (player.continuousFireTime >= player.maxContinuousFire) {
                            triggerOverheatBurst();
                        }
                    }
                } else {
                    player.continuousFireTime = Math.max(0, player.continuousFireTime - dt * 2);
                    overheatContainer.style.opacity = player.continuousFireTime === 0 ? '0' : '1';
                    if (player.continuousFireTime > 0) overheatBar.style.width = (player.continuousFireTime / player.maxContinuousFire * 100) + '%';
                }
            } else if (isMouseDown) {
                shootBullet();
            }

            // 移動操作
            let moveX = 0, moveY = 0;
            if (keys['KeyW'] || keys['ArrowUp']) moveY -= 1;
            if (keys['KeyS'] || keys['ArrowDown']) moveY += 1;
            if (keys['KeyA'] || keys['ArrowLeft']) moveX -= 1;
            if (keys['KeyD'] || keys['ArrowRight']) moveX += 1;

            if (moveX !== 0 && moveY !== 0) {
                moveX *= 0.7071;
                moveY *= 0.7071;
            }

            let currentSpeed = player.speed;
            if (player.isDashing) {
                currentSpeed *= 2.5;
                player.dashDuration--;
                if (player.dashDuration <= 0) player.isDashing = false;
            }

            player.x = Math.max(player.radius, Math.min(canvas.width - player.radius, player.x + moveX * currentSpeed));
            player.y = Math.max(player.radius, Math.min(canvas.height - player.radius, player.y + moveY * currentSpeed));

            // 地圖道具生成 (每 15 秒生成一個)
            itemSpawnTimer += dt;
            if (itemSpawnTimer >= 15000) {
                itemSpawnTimer = 0;
                const itemTypes = ['autoAim', 'slowDown', 'heal', 'shield', 'nuke', 'instantCool'];
                items.push(new Item(itemTypes[Math.floor(Math.random() * itemTypes.length)]));
            }

            // 道具更新與逾時 40 秒清除
            for (let i = items.length - 1; i >= 0; i--) {
                const item = items[i];
                const isExpired = item.update(dt);

                if (isExpired) {
                    items.splice(i, 1);
                    continue;
                }

                // 拾取道具邏輯
                if (Math.hypot(player.x - item.x, player.y - item.y) < player.radius + item.radius) {
                    audioManager.playPowerup();
                    if (item.type === 'autoAim') {
                        player.autoAimTime = 10000;
                    } else if (item.type === 'slowDown') {
                        player.slowDownTime = 10000;
                    } else if (item.type === 'heal') {
                        player.hp = Math.min(player.maxHp, player.hp + player.maxHp * 0.5);
                        updateHUD();
                    } else if (item.type === 'shield') {
                        player.shield++;
                        updateHUD();
                    } else if (item.type === 'nuke') {
                        // 核爆：殺傷全場敵人並正確認算擊殺與血量更新
                        audioManager.playExplosion();
                        for (let i = enemies.length - 1; i >= 0; i--) {
                            const e = enemies[i];
                            e.hp -= 800;
                            if (e.hp <= 0) {
                                player.score += e.scoreValue;
                                player.credits += Math.floor(e.scoreValue / 10);
                                player.kills++;
                                gainExp(e.expValue);
                                enemies.splice(i, 1);
                            }
                        }
                        updateHUD();
                        for (let k = 0; k < 50; k++) {
                            particles.push(new Particle(canvas.width / 2, canvas.height / 2, '#ef4444', 8, 12));
                        }
                    } else if (item.type === 'instantCool') {
                        player.continuousFireTime = 0;
                        player.isOverheated = false;
                        player.infiniteHeatTime = 5000;
                    }
                    items.splice(i, 1);
                }
            }

            // 子彈移動與邊界/射程檢查
            for (let i = bullets.length - 1; i >= 0; i--) {
                const b = bullets[i];

                if (b.isAutoAim && enemies.length > 0) {
                    let nearest = null, minDist = 300;
                    enemies.forEach(e => {
                        const d = Math.hypot(e.x - b.x, e.y - b.y);
                        if (d < minDist) { minDist = d; nearest = e; }
                    });
                    if (nearest) {
                        const angle = Math.atan2(nearest.y - b.y, nearest.x - b.x);
                        b.vx = Math.cos(angle) * player.bulletSpeed;
                        b.vy = Math.sin(angle) * player.bulletSpeed;
                    }
                }

                if (b.isGravity) {
                    enemies.forEach(e => {
                        if (Math.hypot(e.x - b.x, e.y - b.y) < 180) {
                            const pullAngle = Math.atan2(b.y - e.y, b.x - e.x);
                            e.x += Math.cos(pullAngle) * 2;
                            e.y += Math.sin(pullAngle) * 2;
                        }
                    });
                }

                b.x += b.vx;
                b.y += b.vy;

                const traveledDist = Math.hypot(b.x - b.startX, b.y - b.startY);
                if (b.x < 0 || b.x > canvas.width || b.y < 0 || b.y > canvas.height || traveledDist > b.maxRange) {
                    bullets.splice(i, 1);
                }
            }

            // 防禦塔自動攻擊
            turrets.forEach(turret => {
                if (Date.now() - turret.lastFire > turret.fireRate) {
                    let nearestEnemy = null, minDist = turret.range;
                    enemies.forEach(enemy => {
                        const dist = Math.hypot(enemy.x - turret.x, enemy.y - turret.y);
                        if (dist < minDist) { minDist = dist; nearestEnemy = enemy; }
                    });

                    if (nearestEnemy) {
                        turret.lastFire = Date.now();
                        const angle = Math.atan2(nearestEnemy.y - turret.y, nearestEnemy.x - turret.x);
                        bullets.push({
                            x: turret.x,
                            y: turret.y,
                            startX: turret.x,
                            startY: turret.y,
                            vx: Math.cos(angle) * 14,
                            vy: Math.sin(angle) * 14,
                            damage: player.bulletDamage * 0.8,
                            maxRange: 300,
                            radius: 3,
                            color: '#eab308'
                        });
                    }
                }
            });

            // 敵軍生成
            enemySpawnTimer += dt;
            if (enemySpawnTimer > Math.max(300, 1500 - wave * 100) && spawnedEnemiesCount < enemiesToSpawn) {
                enemySpawnTimer = 0;
                spawnedEnemiesCount++;
                let enemyType = (wave % 5 === 0 && spawnedEnemiesCount === 1) ? 'boss' : Math.random() < 0.25 ? 'tank' : 'normal';
                enemies.push(new Enemy(enemyType));
            }

            // 敵軍碰撞與傷害邏輯
            for (let i = enemies.length - 1; i >= 0; i--) {
                const enemy = enemies[i];
                enemy.update();

                const distToPlayer = Math.hypot(player.x - enemy.x, player.y - enemy.y);
                const minDist = player.radius + enemy.radius;

                if (distToPlayer < minDist) {
                    const angle = Math.atan2(enemy.y - player.y, enemy.x - player.x);
                    enemy.x = player.x + Math.cos(angle) * minDist;
                    enemy.y = player.y + Math.sin(angle) * minDist;

                    if (player.shield > 0) {
                        player.shield--;
                        audioManager.playExplosion();
                    } else {
                        player.hp -= (enemy.type === 'boss' ? 2 : 1);
                    }

                    updateHUD();
                    if (player.hp <= 0) {
                        endGame(false);
                        return;
                    }
                }

                // 子彈命中判定
                for (let j = bullets.length - 1; j >= 0; j--) {
                    const b = bullets[j];
                    const distToBullet = Math.hypot(b.x - enemy.x, b.y - enemy.y);

                    if (distToBullet < enemy.radius + b.radius) {
                        let finalDamage = b.damage;

                        // 刺客專屬：距離越近傷害越高 (最高 3 倍)
                        if (hero.isDistanceDamage) {
                            const distFromPlayer = Math.hypot(player.x - enemy.x, player.y - enemy.y);
                            const proxRatio = Math.max(0, 1 - (distFromPlayer / hero.range));
                            finalDamage *= (1 + proxRatio * 2); // 1x ~ 3x
                        }

                        // 法師專屬：命中施加減速
                        if (hero.isSlowControl) {
                            enemy.slowFactor = 0.4;
                        }

                        enemy.hp -= finalDamage;

                        const kbAngle = Math.atan2(b.vy, b.vx);
                        const kbForce = (player.knockback * (hero.id === 'tank' ? 2.5 : 1)) / (enemy.type === 'boss' ? 4 : 1);
                        enemy.x += Math.cos(kbAngle) * kbForce;
                        enemy.y += Math.sin(kbAngle) * kbForce;

                        bullets.splice(j, 1);

                        if (enemy.hp <= 0) {
                            audioManager.playExplosion();
                            player.score += enemy.scoreValue;
                            player.credits += Math.floor(enemy.scoreValue / 10);
                            player.kills++;
                            gainExp(enemy.expValue);
                            enemies.splice(i, 1);
                            break;
                        }
                    }
                }
            }

            // 波次檢查 (10 波勝利條件)
            if (spawnedEnemiesCount >= enemiesToSpawn && enemies.length === 0) {
                if (wave >= MAX_WAVE) {
                    endGame(true); // 通關勝利！
                    return;
                }
                wave++;
                spawnedEnemiesCount = 0;
                enemiesToSpawn = 10 + wave * 5;
                updateHUD();
            }

            for (let i = particles.length - 1; i >= 0; i--) {
                particles[i].update();
                if (particles[i].alpha <= 0) particles.splice(i, 1);
            }
        }

        function gainExp(amount) {
            player.exp += amount;
            if (player.exp >= player.nextExp) {
                player.exp -= player.nextExp;
                player.level++;
                player.nextExp = Math.floor(player.nextExp * 1.4);
                showUpgradeModal();
            }
            updateHUD();
        }

        function updateHUD() {
            const hero = HEROES[selectedHeroId];
            document.getElementById('playerLevel').innerText = player.level;
            document.getElementById('hpText').innerText = `${Math.ceil(player.hp)} / ${player.maxHp}`;
            document.getElementById('hpBar').style.width = Math.max(0, (player.hp / player.maxHp * 100)) + '%';
            
            const shieldTag = document.getElementById('shieldTag');
            if (player.shield > 0) {
                shieldTag.innerText = `護盾 x${player.shield}`;
                shieldTag.classList.remove('hidden');
            } else {
                shieldTag.classList.add('hidden');
            }

            const expPct = Math.floor((player.exp / player.nextExp) * 100);
            document.getElementById('expText').innerText = `${expPct}%`;
            document.getElementById('expBar').style.width = `${expPct}%`;

            document.getElementById('waveText').innerText = `WAVE ${wave}/${MAX_WAVE}`;
            document.getElementById('scoreText').innerText = player.score;
            document.getElementById('creditText').innerHTML = `<i class="fa-solid fa-microchip text-sm mr-1"></i>${player.credits}`;

            const turretTag = document.getElementById('turretCountTag');
            const turretBtn = document.getElementById('btnBuildTurret');
            turretTag.innerText = `${turrets.length} / ${hero.maxTurrets} (50pt)`;
            
            if (turrets.length >= hero.maxTurrets) {
                turretBtn.classList.add('opacity-50', 'cursor-not-allowed');
                turretTag.innerText = `已達上限 (${hero.maxTurrets})`;
            } else {
                turretBtn.classList.remove('opacity-50', 'cursor-not-allowed');
            }
        }

        function showUpgradeModal() {
            gameState = 'UPGRADE';
            const modal = document.getElementById('upgradeModal');
            const container = document.getElementById('upgradeOptionsContainer');
            container.innerHTML = '';

            const normalUpgrades = [
                { id: 'multishot', title: '多重彈幕 +1', desc: '增加每次發射的子彈數量', lucky: false, action: () => { player.multishot += 1; } },
                { id: 'firerate', title: '射擊速率 +25%', desc: '提升連發與冷卻速度', lucky: false, action: () => { player.fireRate = Math.max(50, player.fireRate * 0.75); } },
                { id: 'damage', title: '星核加壓 (+30% 傷害)', desc: '提升基礎子彈殺傷力', lucky: false, action: () => { player.bulletDamage *= 1.3; } },
                { id: 'shield', title: '能量護盾矩陣 (+2 層)', desc: '獲得 2 層抵擋敵人的防護盾', lucky: false, action: () => { player.shield += 2; } },
                { id: 'speed', title: '動能靴 (+25% 速度)', desc: '提升機體移動速度', lucky: false, action: () => { player.speed *= 1.25; } }
            ];

            const luckyUpgrades = [
                { id: 'lucky_damage', title: '星核過載 (+500% 傷害)', desc: '【幸運】武器獲得狂暴級爆破加成', lucky: true, action: () => { player.bulletDamage *= 6; } },
                { id: 'lucky_360', title: '360° 萬向彈幕', desc: '【幸運】射擊轉化為全方位 360 度環狀散射', lucky: true, action: () => { player.is360Shot = true; } },
                { id: 'lucky_gravity', title: '黑洞引力彈', desc: '【幸運】子彈附帶強烈吸引力牽引敵軍', lucky: true, action: () => { player.gravityBullet = true; } }
            ];

            const availableLucky = luckyUpgrades.filter(u => !acquiredLuckyUpgrades.has(u.id));
            const choices = [];

            for (let i = 0; i < 3; i++) {
                if (Math.random() < 0.01 && availableLucky.length > 0) { // 1% 機率幸運
                    const lIdx = Math.floor(Math.random() * availableLucky.length);
                    choices.push(availableLucky.splice(lIdx, 1)[0]);
                } else {
                    const unusedNormals = normalUpgrades.filter(n => !choices.includes(n));
                    if (unusedNormals.length > 0) choices.push(unusedNormals[Math.floor(Math.random() * unusedNormals.length)]);
                }
            }

            choices.forEach(opt => {
                const card = document.createElement('div');
                card.className = `glass-panel p-5 rounded-2xl flex flex-col justify-between items-center text-center border cursor-pointer hover:scale-105 transition-all ${opt.lucky ? 'border-yellow-500/80 glow-yellow bg-yellow-500/10' : 'border-cyan-500/40 hover:border-cyan-400'}`;
                card.innerHTML = `
                    <div>
                        ${opt.lucky ? '<span class="text-[10px] font-orbitron font-bold text-yellow-400 bg-yellow-500/20 px-2 py-0.5 rounded-full mb-2 inline-block">LUCKY UPGRADE</span>' : ''}
                        <h3 class="font-orbitron font-bold text-lg text-white mb-2">${opt.title}</h3>
                        <p class="text-xs text-gray-300 mb-4">${opt.desc}</p>
                    </div>
                    <button class="w-full py-2 rounded-xl ${opt.lucky ? 'bg-yellow-500 text-black font-bold' : 'bg-cyan-500/20 hover:bg-cyan-500/40 text-cyan-300'} font-orbitron text-xs">選擇模組</button>
                `;
                card.onclick = () => {
                    opt.action();
                    if (opt.lucky) acquiredLuckyUpgrades.add(opt.id);
                    modal.classList.add('hidden');
                    gameState = 'PLAYING';
                    updateHUD();
                };
                container.appendChild(card);
            });

            modal.classList.remove('hidden');
        }

        function endGame(isVictory) {
            gameState = 'GAMEOVER';

            // 結算星星幣: 通關獲得 150⭐ + 每波 15⭐ + 擊殺數/10⭐
            const earnedCoins = (isVictory ? 150 : 0) + (wave * 15) + Math.floor(player.kills / 10);
            starCoins += earnedCoins;
            saveGameProgress();

            const iconEl = document.getElementById('gameOverIcon');
            const titleEl = document.getElementById('gameOverTitle');
            const subTitleEl = document.getElementById('gameOverSubTitle');

            if (isVictory) {
                iconEl.className = 'w-20 h-20 rounded-2xl bg-yellow-500/20 border border-yellow-500/50 flex items-center justify-center text-yellow-400 text-4xl shadow-lg shadow-yellow-500/30';
                iconEl.innerHTML = '<i class="fa-solid fa-trophy"></i>';
                titleEl.className = 'text-3xl font-black font-orbitron text-yellow-400 mb-1';
                titleEl.innerText = '星核防衛成功！';
                subTitleEl.innerText = 'MISSION ACCOMPLISHED';
            } else {
                iconEl.className = 'w-20 h-20 rounded-2xl bg-red-500/20 border border-red-500/50 flex items-center justify-center text-red-500 text-4xl shadow-lg shadow-red-500/30';
                iconEl.innerHTML = '<i class="fa-solid fa-skull"></i>';
                titleEl.className = 'text-3xl font-black font-orbitron text-red-500 mb-1';
                titleEl.innerText = '星核崩毀';
                subTitleEl.innerText = 'SYSTEM OVERRIDE DETECTED';
            }

            document.getElementById('finalWave').innerText = `WAVE ${wave}/10`;
            document.getElementById('finalScore').innerText = player.score;
            document.getElementById('earnedCoins').innerHTML = `<i class="fa-solid fa-star mr-1"></i>+${earnedCoins}`;

            document.getElementById('gameOverModal').classList.remove('hidden');
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // 背景網格
            ctx.strokeStyle = 'rgba(6, 182, 212, 0.05)';
            ctx.lineWidth = 1;
            for (let x = 0; x < canvas.width; x += 40) {
                ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += 40) {
                ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
            }

            items.forEach(item => item.draw());

            turrets.forEach(turret => {
                ctx.save();
                ctx.shadowBlur = 10;
                ctx.shadowColor = '#eab308';
                ctx.fillStyle = '#eab308';
                ctx.beginPath();
                ctx.arc(turret.x, turret.y, turret.radius, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            });

            // 繪製玩家機甲
            const hero = HEROES[selectedHeroId];
            ctx.save();
            ctx.shadowBlur = 20;
            ctx.shadowColor = hero.color;
            ctx.fillStyle = hero.color;
            ctx.beginPath();
            ctx.arc(player.x, player.y, player.radius, 0, Math.PI * 2);
            ctx.fill();

            if (player.shield > 0) {
                ctx.strokeStyle = '#60a5fa';
                ctx.lineWidth = 3;
                ctx.beginPath();
                ctx.arc(player.x, player.y, player.radius + 6, 0, Math.PI * 2);
                ctx.stroke();
            }

            // 砲管方向指標
            const aimAngle = Math.atan2(mousePos.y - player.y, mousePos.x - player.x);
            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = 4;
            ctx.beginPath();
            ctx.moveTo(player.x, player.y);
            ctx.lineTo(player.x + Math.cos(aimAngle) * 28, player.y + Math.sin(aimAngle) * 28);
            ctx.stroke();
            ctx.restore();

            // 繪製子彈
            bullets.forEach(b => {
                ctx.save();
                ctx.shadowBlur = b.isGravity ? 15 : 8;
                ctx.shadowColor = b.color;
                ctx.fillStyle = b.color;
                ctx.beginPath();
                ctx.arc(b.x, b.y, b.radius, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            });

            enemies.forEach(enemy => enemy.draw());
            particles.forEach(p => p.draw());
        }

        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        window.onload = function() {
            renderHeroCards();
            requestAnimationFrame(gameLoop);
        };
    </script>
</body>
</html>
