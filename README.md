<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>⚡ 復古怪誕療癒街機 15合1 ⚡</title>
</head>
<body>
  <div id="menu-screen">
    <div id="menu-grid"></div>
  </div>
  <div id="game-screen">
    <canvas id="gameCanvas"></canvas>
    <div id="hud"></div>
    <div id="controls"></div>
  </div>
  <script>
    // your game code here
  </script>
</body>
</html><!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>⚡ 復古怪誕療癒街機 15合1 ⚡</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; -webkit-user-select: none; user-select: none; }
    body, html { width: 100%; height: 100%; overflow: hidden; background: #2b2d42; font-family: 'Courier New', monospace; color: #edf2f4; }
    #menu-screen { position: absolute; width: 100%; height: 100%; overflow-y: auto; padding: 20px; z-index: 100; background: #1a1a24; display: flex; flex-direction: column; align-items: center; }
    .title { font-size: 28px; font-weight: bold; color: #ef233c; text-shadow: 3px 3px 0px #8d99ae; text-align: center; margin: 20px 0; }
    .subtitle { font-size: 14px; color: #8d99ae; margin-bottom: 20px; text-align: center; }
    .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 12px; width: 100%; max-width: 600px; }
    .game-card { background: #3d405b; border: 4px solid #f4f1de; border-radius: 8px; padding: 12px; text-align: center; cursor: pointer; box-shadow: 0 4px 0 #e07a5f; transition: 0.1s; font-size: 13px; font-weight: bold; min-height: 80px; display: flex; align-items: center; justify-content: center; }
    .game-card:active { transform: translateY(4px); box-shadow: 0 0 0 #000; }
    #game-screen { position: absolute; width: 100%; height: 100%; display: none; z-index: 50; }
    #canvas-container { width: 100%; height: 100%; background: #feeafa; position: absolute; }
    canvas { display: block; width: 100%; height: 100%; }
    #hud { position: absolute; top: 15px; width: 100%; text-align: center; font-size: 18px; font-weight: bold; text-shadow: 1px 1px 0px #000; pointer-events: none; z-index: 80; color: #3d405b; }
    #controls { position: absolute; bottom: 30px; width: 100%; display: flex; justify-content: center; gap: 15px; z-index: 80; flex-wrap: wrap; padding: 0 12px; }
    .btn { background: #e07a5f; color: white; border: 3px solid #3d405b; padding: 10px 20px; font-size: 14px; font-weight: bold; border-radius: 6px; cursor: pointer; box-shadow: 0 3px 0 #3d405b; }
    .btn:active { transform: translateY(3px); box-shadow: 0 0 0 #000; }
    #back-btn { position: absolute; top: 15px; left: 15px; background: #8d99ae; z-index: 90; padding: 6px 12px; font-size: 12px; }
    @media (max-width: 700px) {
      .title { font-size: 24px; }
      #hud { font-size: 15px; }
      .btn { padding: 9px 14px; font-size: 13px; }
    }
  </style>
</head>
<body>
  <div id="menu-screen">
    <div class="title">👾 荒謬療癒街機 👾</div>
    <div class="subtitle">低模/像素/減壓/隨開即玩 (附原始代碼)</div>
    <div class="grid" id="menu-grid"></div>
  </div>

  <div id="game-screen">
    <button class="btn" id="back-btn">⬅ 選單 Menu</button>
    <div id="hud"></div>
    <div id="canvas-container"><canvas id="gameCanvas"></canvas></div>
    <div id="controls"></div>
  </div>

  <script>
    const games = [
      { id: 1, name: "1. 尖叫駝羊 🦙", desc: "越摸越大的爆笑駝羊", hud: "點擊下方按鈕互動", btns: ["撫摸 Pet", "餵食 Feed"] },
      { id: 2, name: "2. 爆炸馬鈴薯 🥔", desc: "隨時會自爆的禪意疊石", hud: "小心疊放！", btns: ["放下石頭 Drop"] },
      { id: 3, name: "3. 異星外星咖啡 👽", desc: "傾聽存在主義危機", hud: "外星人：我很焦慮...", btns: ["倒岩漿茶 Pour", "講冷笑話 Joke"] },
      { id: 4, name: "4. 靈魂古董配對 ⏳", desc: "解鎖被詛咒文物的記憶", hud: "點擊方塊配對消除", btns: ["洗牌 Shuffle"] },
      { id: 5, name: "5. 焦慮幾何粉碎 🔺", desc: "沒有失敗的形狀填滿", hud: "拖曳或點擊填滿空間", btns: ["下一關 Next"] },
      { id: 6, name: "6. 尖叫泡泡紙 🧼", desc: "會發出怪聲的無限泡泡", hud: "點擊畫面瘋狂擠壓！", btns: ["換一張 Reset"] },
      { id: 7, name: "7. 業障流水撈魚 🐟", desc: "用念力極速釣魚", hud: "當泛起漣漪時點擊！", btns: ["拉線起魚！"] },
      { id: 8, name: "8. 宇宙觀星大炮 🔭", desc: "用光束尋找荒謬星座", hud: "滑動螢幕探索深空", btns: ["切換頻率 Tune"] },
      { id: 9, name: "9. 摺紙尖叫雞 🐔", desc: "用手指壓平尖叫雞", hud: "跟著節奏拍打折疊", btns: ["用力折！ Fold"] },
      { id: 10, name: "10. 鴿子暴食廣場 🐦", desc: "召喚巨型鴿子軍團", hud: "點擊地面撒下聖餐", btns: ["撒金屑 Scatter"] },
      { id: 11, name: "11. 憤怒落葉清掃 🍂", desc: "狂風也吹不走的療癒", hud: "滑動或點擊清理落葉", btns: ["呼喚狂風 Blow"] },
      { id: 12, name: "12. 刺蝟音速彈跳 🦔", desc: "跳過一切煩惱的單鍵遊戲", hud: "障礙物來了！跳！", btns: ["跳躍！ JUMP"] },
      { id: 13, name: "13. 迷宮鋼琴家 🎹", desc: "走一步就是一首交響樂", hud: "探索迷宮，自動彈奏", btns: ["上 Up", "左 Left", "右 Right"] },
      { id: 14, name: "14. 瘋狂金幣推土 🪙", desc: "無盡吐出垃圾的推幣機", hud: "狂下代幣吧！", btns: ["投幣 Drop Coin"] },
      { id: 15, name: "15. 尖叫盆栽 🪵", desc: "越剪叫得越好聽的樹", hud: "修剪枝葉釋放壓力", btns: ["剪枝 Prune", "澆聖水 Water"] }
    ];

    const menuGrid = document.getElementById('menu-grid');
    const menuScreen = document.getElementById('menu-screen');
    const gameScreen = document.getElementById('game-screen');
    const backBtn = document.getElementById('back-btn');
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const hud = document.getElementById('hud');
    const controls = document.getElementById('controls');

    let currentActiveGame = null;
    let animationFrameId = null;
    let audioCtx = null;
    let gameState = {};

    function resizeCanvas() {
      canvas.width = canvas.parentElement.clientWidth;
      canvas.height = canvas.parentElement.clientHeight;
    }

    function initAudio() {
      if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      if (audioCtx.state === 'suspended') audioCtx.resume();
    }

    function playSynthSound(freq, type = 'sine', duration = 0.3) {
      initAudio();
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = type;
      osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
      gain.gain.setValueAtTime(0.15, audioCtx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      osc.stop(audioCtx.currentTime + duration);
    }

    games.forEach(g => {
      const card = document.createElement('div');
      card.className = 'game-card';
      card.innerHTML = `<div>${g.name}<br><span style="font-size:10px;color:#8d99ae;font-weight:normal;">${g.desc}</span></div>`;
      card.onclick = () => launchGame(g.id);
      menuGrid.appendChild(card);
    });

    backBtn.onclick = () => {
      cancelAnimationFrame(animationFrameId);
      gameScreen.style.display = 'none';
      menuScreen.style.display = 'flex';
      currentActiveGame = null;
    }
    window.onresize = resizeCanvas;

        function launchGame(id) {
      menuScreen.style.display = 'none';
      gameScreen.style.display = 'block';
      resizeCanvas();
      initAudio();

      const gameData = games.find(g => g.id === id);
      hud.innerText = gameData.hud;
      currentActiveGame = id;

      controls.innerHTML = '';
      gameData.btns.forEach((btnText, index) => {
        const b = document.createElement('button');
        b.className = 'btn';
        b.innerText = btnText;
        b.onclick = () => handleGameInput(id, index);
        controls.appendChild(b);
      });

      gameState = {
        timer: 0,
        entities: [],
        score: 0,
        customVal: 1,
        jumping: false,
        jumpY: 0,
        player: { x: 40, y: 150 }
      };

      canvas.onclick = (e) => {
        const rect = canvas.getBoundingClientRect();
        handleCanvasClick(id, e.clientX - rect.left, e.clientY - rect.top);
      };

      setupSpecificGame(id);
      cancelAnimationFrame(animationFrameId);
      gameLoop();
    }

    function setupSpecificGame(id) {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      if (id === 2) {
        gameState.entities = [{ x: canvas.width / 2, y: canvas.height - 40, r: 40, col: '#8d99ae' }];
      } else if (id === 4) {
        for (let i = 0; i < 4; i++) {
          for (let j = 0; j < 4; j++) {
            gameState.entities.push({ x: 50 + i * 70, y: 150 + j * 70, w: 60, h: 60, id: (i + j) % 4, state: 'hidden' });
          }
        }
      } else if (id === 6) {
        for (let i = 0; i < 6; i++) {
          for (let j = 0; j < 8; j++) {
            gameState.entities.push({ x: 40 + i * 50, y: 120 + j * 50, r: 20, popped: false });
          }
        }
      } else if (id === 11) {
        for (let i = 0; i < 30; i++) {
          gameState.entities.push({
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height,
            r: 12,
            c: Math.random() > 0.5 ? '#e07a5f' : '#f4f1de'
          });
        }
      } else if (id === 13) {
        gameState.player = { x: 40, y: 150 };
      } else if (id === 14) {
        gameState.entities = [];
      }
    }

    function handleGameInput(gameId, btnIndex) {
      if (gameId === 1) {
        if (btnIndex === 0) {
          gameState.customVal += 0.15;
          playSynthSound(200 + gameState.customVal * 100, 'sawtooth');
          if (gameState.customVal > 2.5) {
            gameState.customVal = 0.5;
            hud.innerText = "💥 駝羊氣球爆炸重組！ 💥";
            playSynthSound(100, 'triangle', 0.6);
          }
        } else {
          playSynthSound(600, 'sine');
          gameState.timer = 10;
        }
      }

      if (gameId === 2) {
        const last = gameState.entities[gameState.entities.length - 1];
        if (gameState.entities.length > 6 || Math.random() > 0.7) {
          gameState.entities = [{ x: canvas.width / 2, y: canvas.height - 40, r: 40, col: '#8d99ae' }];
          hud.innerText = "💥 慘了！馬鈴薯自爆了！";
          playSynthSound(120, 'sawtooth', 0.5);
        } else {
          gameState.entities.push({
            x: canvas.width / 2 + (Math.random() * 20 - 10),
            y: last.y - 45,
            r: 20 + Math.random() * 15,
            col: '#e07a5f'
          });
          playSynthSound(300 + gameState.entities.length * 50, 'triangle');
        }
      }

      if (gameId === 3) {
        if (btnIndex === 0) {
          hud.innerText = "👽「好燙！這岩漿很解壓。」";
          playSynthSound(180, 'sine');
        } else {
          hud.innerText = "👽「我的存在只是像素組編嗎？」";
          playSynthSound(800, 'square');
        }
      }

      if (gameId === 4) {
        gameState.entities.forEach(e => e.state = e.state === 'hidden' ? 'shown' : 'hidden');
        hud.innerText = "記憶正在翻面...";
        playSynthSound(420, 'triangle');
      }

      if (gameId === 5) {
        playSynthSound(500, 'sine');
        hud.innerText = "關卡完成！新謎題生成！";
      }

      if (gameId === 6) {
        setupSpecificGame(6);
        playSynthSound(400, 'triangle');
      }

      if (gameId === 7) {
        if (Math.abs(Math.sin(gameState.timer) - 0.9) < 0.2) {
          hud.innerText = "🎣 成功釣起一隻怪味蝦！";
          playSynthSound(800, 'sine', 0.5);
        } else {
          hud.innerText = "💨 魚脫鉤跑了！再試一次";
          playSynthSound(150, 'sawtooth');
        }
      }

      if (gameId === 8) {
        hud.innerText = "📡 調整頻率中...";
        playSynthSound(260 + btnIndex * 80, 'sine');
      }

      if (gameId === 9) {
        gameState.customVal += 20;
        playSynthSound(300 + gameState.customVal, 'square');
        if (gameState.customVal > 360) gameState.customVal = 0;
      }

      if (gameId === 10) {
        playSynthSound(900, 'sine');
        for (let i = 0; i < 5; i++) {
          gameState.entities.push({ x: Math.random() * canvas.width, y: Math.random() * canvas.height, r: 8 });
        }
      }

      if (gameId === 11) {
        playSynthSound(200, 'sine', 0.8);
        gameState.entities.forEach(l => {
          l.x += Math.random() * 200 - 100;
          l.y += Math.random() * 200 - 100;
        });
      }

      if (gameId === 12) {
        if (!gameState.jumping) {
          gameState.jumping = true;
          gameState.jumpY = 0;
          playSynthSound(450, 'triangle');
        }
      }

      if (gameId === 13) {
        if (btnIndex === 0) gameState.player.y -= 20;
        if (btnIndex === 1) gameState.player.x -= 20;
        if (btnIndex === 2) gameState.player.x += 20;
        playSynthSound(261.6 + (gameState.player.x % 7) * 30, 'sine');
      }

      if (gameId === 14) {
        gameState.entities.push({ x: 100 + Math.random() * 150, y: 50, r: 15, vY: 2 });
        playSynthSound(900, 'sine', 0.1);
      }

      if (gameId === 15) {
        if (btnIndex === 0) {
          hud.innerText = "✂️「哎喲！輕一點！」";
          playSynthSound(700, 'sawtooth');
        } else {
          hud.innerText = "💧「咕嚕咕嚕...真好喝」";
          playSynthSound(500, 'sine');
        }
      }
    }

    function handleCanvasClick(gameId, x, y) {
      if (gameId === 6) {
        gameState.entities.forEach(p => {
          const d = Math.hypot(p.x - x, p.y - y);
          if (d < p.r && !p.popped) {
            p.popped = true;
            playSynthSound(800 + Math.random() * 300, 'sine', 0.05);
          }
        });
      }

      if (gameId === 10) {
        gameState.entities.push({ x, y, r: 10 });
        playSynthSound(400, 'triangle');
      }
    }

        function drawTextCentered(text, y, color = "#3d405b", size = 18) {
      ctx.fillStyle = color;
      ctx.font = `bold ${size}px Courier New, monospace`;
      ctx.textAlign = "center";
      ctx.fillText(text, canvas.width / 2, y);
      ctx.textAlign = "start";
    }

    function gameLoop() {
      animationFrameId = requestAnimationFrame(gameLoop);
      gameState.timer += 0.05;

      ctx.fillStyle = "#f4f1de";
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      if (currentActiveGame === 1) {
        const size = gameState.customVal;
        const bounce = gameState.timer < 10 ? Math.sin(gameState.timer) * 30 : 0;
        if (gameState.timer > 0) gameState.timer -= 0.01;
        ctx.save();
        ctx.translate(canvas.width / 2, canvas.height / 2 + 50 - bounce);
        ctx.scale(size, size);
        ctx.fillStyle = "#ffffff";
        ctx.fillRect(-30, -20, 60, 40);
        ctx.fillRect(15, -60, 20, 50);
        ctx.fillRect(15, -75, 30, 20);
        ctx.fillStyle = "#e07a5f";
        ctx.fillRect(30, -68, 20, 12);
        ctx.fillStyle = "#3d405b";
        ctx.fillRect(25, -72, 6, 6);
        ctx.fillRect(-20, 20, 8, 20);
        ctx.fillRect(10, 20, 8, 20);
        ctx.restore();
      }

      if (currentActiveGame === 2) {
        gameState.entities.forEach(st => {
          ctx.fillStyle = st.col;
          ctx.beginPath();
          ctx.arc(st.x, st.y, st.r, 0, Math.PI * 2);
          ctx.fill();
          ctx.lineWidth = 4;
          ctx.strokeStyle = "#3d405b";
          ctx.stroke();
        });
      }

      if (currentActiveGame === 3) {
        ctx.fillStyle = "#3d405b";
        ctx.fillRect(canvas.width / 2 - 60, canvas.height / 2, 120, 100);
        ctx.fillStyle = "#81b29a";
        ctx.beginPath();
        ctx.arc(canvas.width / 2, canvas.height / 2 - 40, 35, 0, Math.PI * 2);
        ctx.fill();
        ctx.fillStyle = "#3d405b";
        ctx.fillRect(canvas.width / 2 - 20, canvas.height / 2 - 45, 10, 20);
        ctx.fillRect(canvas.width / 2 + 10, canvas.height / 2 - 45, 10, 20);
      }

      if (currentActiveGame === 4) {
        gameState.entities.forEach(box => {
          ctx.fillStyle = box.state === 'hidden' ? '#3d405b' : '#f2cc8f';
          ctx.fillRect(box.x, box.y, box.w, box.h);
          ctx.strokeStyle = '#2b2d42';
          ctx.lineWidth = 2;
          ctx.strokeRect(box.x, box.y, box.w, box.h);
        });
      }

      if (currentActiveGame === 5) {
        ctx.fillStyle = "#e07a5f";
        ctx.beginPath();
        ctx.moveTo(canvas.width / 2, 150);
        ctx.lineTo(canvas.width / 2 - 80, 300);
        ctx.lineTo(canvas.width / 2 + 80, 300);
        ctx.closePath();
        ctx.fill();
        ctx.fillStyle = "rgba(61,64,91,0.3)";
        ctx.fillRect(canvas.width / 2 - 40, 200, 80, 80);
      }

      if (currentActiveGame === 6) {
        gameState.entities.forEach(p => {
          ctx.fillStyle = p.popped ? "#8d99ae" : "#81b29a";
          ctx.beginPath();
          ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
          ctx.fill();
          ctx.strokeStyle = "#3d405b";
          ctx.stroke();
        });
      }

      if (currentActiveGame === 7) {
        const wave = Math.sin(gameState.timer) * 40 + canvas.width / 2;
        ctx.fillStyle = "#81b29a";
        ctx.beginPath();
        ctx.arc(canvas.width / 2, canvas.height / 2, Math.abs(Math.sin(gameState.timer)) * 120, 0, Math.PI * 2);
        ctx.fill();
        ctx.fillStyle = "#e07a5f";
        ctx.fillRect(wave - 10, canvas.height / 2 - 10, 20, 20);
      }

      if (currentActiveGame === 8) {
        ctx.strokeStyle = "rgba(61,64,91,0.4)";
        ctx.lineWidth = 3;
        ctx.beginPath();
        ctx.moveTo(100, 150);
        ctx.lineTo(200, 220);
        ctx.lineTo(150, 320);
        ctx.stroke();
        ctx.fillStyle = "#e07a5f";
        ctx.fillRect(100, 150, 10, 10);
        ctx.fillRect(200, 220, 10, 10);
        ctx.fillRect(150, 320, 10, 10);
      }

      if (currentActiveGame === 9) {
        ctx.fillStyle = "#f2cc8f";
        ctx.save();
        ctx.translate(canvas.width / 2, canvas.height / 2);
        ctx.rotate(gameState.customVal * Math.PI / 180);
        ctx.fillRect(-50, -50, 100, 100);
        ctx.restore();
      }

      if (currentActiveGame === 10) {
        gameState.entities.forEach(p => {
          ctx.fillStyle = "#8d99ae";
          ctx.fillRect(p.x, p.y, 25, 20);
          ctx.fillStyle = "#e07a5f";
          ctx.fillRect(p.x + 20, p.y + 5, 8, 5);
        });
      }

      if (currentActiveGame === 11) {
        gameState.entities.forEach(l => {
          ctx.fillStyle = l.c;
          ctx.fillRect(l.x, l.y, l.r, l.r + 5);
        });
      }

      if (currentActiveGame === 12) {
        if (gameState.jumping) {
          gameState.jumpY += 4;
          if (gameState.jumpY > 100) {
            gameState.jumping = false;
            gameState.jumpY = 0;
          }
        }
        const yOffset = gameState.jumping ? Math.sin(gameState.jumpY / 100 * Math.PI) * 80 : 0;
        ctx.fillStyle = "#3d405b";
        ctx.fillRect(80, canvas.height - 100 - yOffset, 40, 30);
        const obsX = canvas.width - (gameState.timer * 150) % canvas.width;
        ctx.fillStyle = "#ef233c";
        ctx.fillRect(obsX, canvas.height - 100, 20, 30);
      }

      if (currentActiveGame === 13) {
        ctx.fillStyle = "rgba(61,64,91,0.2)";
        ctx.fillRect(30, 100, canvas.width - 60, 200);
        ctx.fillStyle = "#e07a5f";
        ctx.beginPath();
        ctx.arc(gameState.player.x, gameState.player.y, 12, 0, Math.PI * 2);
        ctx.fill();
      }

      if (currentActiveGame === 14) {
        ctx.fillStyle = "#3d405b";
        ctx.fillRect(50, canvas.height - 150, canvas.width - 100, 20);
        gameState.entities.forEach(c => {
          c.y += c.vY;
          if (c.y > canvas.height - 150) {
            c.y = canvas.height - 150;
            c.vY = 0;
          }
          ctx.fillStyle = "#f2cc8f";
          ctx.beginPath();
          ctx.arc(c.x, c.y, c.r, 0, Math.PI * 2);
          ctx.fill();
          ctx.strokeStyle = "#3d405b";
          ctx.stroke();
        });
      }

      if (currentActiveGame === 15) {
        ctx.fillStyle = "#81b29a";
        ctx.fillRect(canvas.width / 2 - 15, canvas.height / 2 - 40 + Math.sin(gameState.timer) * 5, 30, 120);
        ctx.fillStyle = "#e07a5f";
        ctx.fillRect(canvas.width / 2 - 40, canvas.height / 2 + 80, 80, 30);
      }

      drawTextCentered("", 30);
    }
  </script>
</body>
</html> 

