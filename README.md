<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Monopoly PRO</title>
  <style>
    :root {
      --bg0: #091421;
      --bg1: #0c1a2d;
      --bg2: #132a40;
      --panel: rgba(17, 28, 40, 0.9);
      --panel-alt: rgba(12, 20, 31, 0.82);
      --line: rgba(255,255,255,0.08);
      --text: #edf5ff;
      --muted: #a8bfd7;
      --primary: #6d7cff;
      --primary-2: #8a6efb;
      --pink: #ff5ca8;
      --green: #27d07c;
      --danger: #ff5f5f;
      --warning: #ffb11a;
      --gold: #f8d76b;
      --board-green: #1f9a53;
      --board-border: #0c6732;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    html, body {
      width: 100%;
      height: 100%;
      background:
        radial-gradient(circle at 20% 10%, rgba(109,124,255,0.25), transparent 22%),
        radial-gradient(circle at 80% 80%, rgba(255,92,168,0.16), transparent 25%),
        linear-gradient(135deg, var(--bg0), var(--bg1) 45%, var(--bg2));
      font-family: "Segoe UI", Tahoma, sans-serif;
      color: var(--text);
      overflow: hidden;
    }

    body {
      display: flex;
      justify-content: center;
      align-items: stretch;
    }

    .app {
      width: min(1600px, 100%);
      height: 100vh;
      display: grid;
      grid-template-rows: 68px 1fr 52px;
      position: relative;
    }

    .topbar {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 18px;
      border-bottom: 1px solid var(--line);
      background: rgba(8, 14, 22, 0.72);
      backdrop-filter: blur(10px);
      box-shadow: 0 12px 30px rgba(0,0,0,0.18);
      z-index: 20;
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 12px;
      font-weight: 900;
      letter-spacing: 1.2px;
      text-transform: uppercase;
      font-size: clamp(18px, 1.4vw, 28px);
    }

    .brand-badge {
      width: 38px;
      height: 38px;
      border-radius: 12px;
      background: linear-gradient(135deg, var(--pink), var(--primary));
      display: grid;
      place-items: center;
      box-shadow: 0 10px 24px rgba(109,124,255,0.32);
      font-size: 18px;
    }

    .topbar-stats {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      align-items: center;
    }

    .chip {
      display: flex;
      align-items: center;
      gap: 8px;
      background: rgba(255,255,255,0.04);
      border: 1px solid var(--line);
      border-radius: 999px;
      padding: 8px 12px;
      font-size: 12px;
      color: var(--muted);
    }

    .chip strong {
      color: var(--text);
      font-weight: 800;
    }

    .content {
      display: grid;
      grid-template-columns: minmax(0, 1.5fr) minmax(320px, 0.8fr);
      gap: 16px;
      padding: 16px;
      min-height: 0;
      overflow: hidden;
    }

    .board-shell {
      background: rgba(11, 18, 28, 0.86);
      border: 1px solid var(--line);
      border-radius: 22px;
      box-shadow: 0 18px 42px rgba(0,0,0,0.25), inset 0 1px 0 rgba(255,255,255,0.05);
      padding: 12px;
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 0;
    }

    .board {
      position: relative;
      width: min(100%, 820px);
      aspect-ratio: 1;
      background: linear-gradient(135deg, var(--board-green), #157d3b);
      border: 7px solid var(--board-border);
      border-radius: 12px;
      display: grid;
      grid-template-columns: repeat(11, minmax(0, 1fr));
      grid-template-rows: repeat(11, minmax(0, 1fr));
      gap: 3px;
      padding: 3px;
      box-shadow: 0 20px 45px rgba(3, 12, 9, 0.45), inset 0 0 0 1px rgba(255,255,255,0.06);
      overflow: hidden;
    }

    .cell {
      position: relative;
      background: linear-gradient(180deg, #f7fbff, #ebf2fa);
      border: 1px solid rgba(94,114,140,0.8);
      border-radius: 4px;
      overflow: hidden;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
      padding: 5px 2px 4px;
      color: #17262f;
      cursor: pointer;
      transition: transform 0.15s ease, box-shadow 0.15s ease, border-color 0.15s ease;
      user-select: none;
      min-height: 0;
    }

    .cell:hover {
      transform: translateY(-1px);
      border-color: rgba(109,124,255,0.8);
      box-shadow: 0 8px 18px rgba(109,124,255,0.14);
    }

    .cell.selected {
      border-color: rgba(255,92,168,0.95);
      box-shadow: 0 0 0 2px rgba(255,92,168,0.20);
    }

    .cell.special {
      background: linear-gradient(180deg, #1a2432, #0d1724);
      color: var(--text);
      border-color: rgba(255,255,255,0.08);
    }

    .bar {
      width: 100%;
      height: 10px;
      display: block;
      border-radius: 4px 4px 0 0;
      box-shadow: inset 0 -2px 0 rgba(255,255,255,0.14);
      margin-bottom: 4px;
    }

    .cell.special .bar {
      display: none;
    }

    .cell .name {
      font-weight: 800;
      font-size: 7.4px;
      line-height: 1.08;
      text-align: center;
      padding: 0 2px;
      letter-spacing: 0.04em;
    }

    .cell .price {
      font-size: 6.7px;
      color: #364b62;
      font-weight: 700;
      margin-top: 3px;
    }

    .cell .owner {
      font-size: 6px;
      min-height: 10px;
      margin-top: 2px;
      font-weight: 700;
      color: #202d3a;
    }

    .board-center {
      grid-column: 2 / 11;
      grid-row: 2 / 11;
      background:
        radial-gradient(circle at top, rgba(109,124,255,0.15), transparent 42%),
        linear-gradient(180deg, rgba(17, 28, 40, 0.9), rgba(9, 16, 24, 0.95));
      border: 2px solid rgba(138,110,251,0.45);
      border-radius: 14px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 18px;
      padding: 14px;
      text-align: center;
      box-shadow: inset 0 1px 0 rgba(255,255,255,0.05);
    }

    .logo {
      font-size: clamp(28px, 1.8vw, 52px);
      font-weight: 900;
      letter-spacing: 4px;
      text-transform: uppercase;
      line-height: 1;
      background: linear-gradient(90deg, var(--primary), var(--pink));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      text-shadow: 0 0 22px rgba(109,124,255,0.22);
    }

    .dice-box {
      display: flex;
      gap: 16px;
      justify-content: center;
      align-items: center;
    }

    .die {
      width: 56px;
      height: 56px;
      border-radius: 14px;
      background: linear-gradient(145deg, rgba(255,255,255,0.08), rgba(255,255,255,0.02));
      border: 2px solid rgba(255,255,255,0.12);
      display: grid;
      place-items: center;
      font-size: 26px;
      font-weight: 900;
      color: var(--text);
      box-shadow: 0 10px 22px rgba(0,0,0,0.2);
      transition: transform 0.18s ease, border-color 0.18s ease;
    }

    .die.rolling {
      animation: diceRoll 0.5s ease;
      border-color: rgba(255,92,168,0.8);
      box-shadow: 0 0 0 4px rgba(255,92,168,0.12), 0 12px 24px rgba(255,92,168,0.18);
    }

    @keyframes diceRoll {
      0% { transform: rotate(0deg) scale(1); }
      25% { transform: rotate(30deg) scale(1.08); }
      50% { transform: rotate(-24deg) scale(1.12); }
      75% { transform: rotate(18deg) scale(1.04); }
      100% { transform: rotate(0deg) scale(1); }
    }

    .cell-token-layer {
      position: absolute;
      inset: 0;
      pointer-events: none;
      z-index: 8;
    }

    .token {
      position: absolute;
      width: 18px;
      height: 18px;
      border-radius: 50%;
      border: 2px solid rgba(255,255,255,0.82);
      box-shadow: 0 4px 12px rgba(0,0,0,0.35);
      transition: left 0.3s ease, top 0.3s ease, transform 0.3s ease;
      display: grid;
      place-items: center;
      font-size: 8px;
      font-weight: 900;
      color: white;
      user-select: none;
    }

    .token.red { background: linear-gradient(135deg, #ff625c, #dc2c2c); }
    .token.blue { background: linear-gradient(135deg, #559dff, #235fff); }
    .token.yellow { background: linear-gradient(135deg, #ffd85a, #e7a400); color: #2d2200; }
    .token.green { background: linear-gradient(135deg, #34d78a, #179b4d); }

    .sidebar {
      display: grid;
      grid-template-rows: auto auto auto auto;
      gap: 16px;
      min-height: 0;
      overflow: hidden;
    }

    .panel {
      background: rgba(13, 22, 33, 0.86);
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 14px;
      box-shadow: 0 16px 26px rgba(0,0,0,0.18), inset 0 1px 0 rgba(255,255,255,0.02);
    }

    .panel-title {
      display: flex;
      align-items: center;
      gap: 8px;
      margin-bottom: 12px;
      color: #9bb1cb;
      font-weight: 800;
      letter-spacing: 1.1px;
      text-transform: uppercase;
      font-size: 11px;
    }

    .players-grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 10px;
    }

    .player-card {
      position: relative;
      border: 1px solid rgba(255,255,255,0.1);
      border-radius: 14px;
      background: linear-gradient(180deg, rgba(255,255,255,0.04), rgba(255,255,255,0.015));
      padding: 11px 10px;
      min-height: 112px;
      overflow: hidden;
      transition: border-color 0.18s ease, box-shadow 0.18s ease, transform 0.18s ease;
    }

    .player-card.active {
      border-color: rgba(255,92,168,0.8);
      box-shadow: 0 0 0 1px rgba(255,92,168,0.20), 0 12px 24px rgba(255,92,168,0.12);
      background: linear-gradient(180deg, rgba(255,92,168,0.08), rgba(255,255,255,0.02));
    }

    .player-card.bankrupt {
      opacity: 0.44;
      text-decoration: line-through;
      filter: grayscale(0.35);
    }

    .player-card::before {
      content: "";
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 4px;
      background: linear-gradient(90deg, var(--primary), var(--pink));
      opacity: 0.7;
    }

    .player-name {
      display: flex;
      align-items: center;
      gap: 8px;
      font-weight: 800;
      font-size: 13px;
      margin-bottom: 8px;
    }

    .player-dot {
      width: 12px; height: 12px; border-radius: 50%;
      display: inline-block;
      border: 2px solid rgba(255,255,255,0.72);
      box-shadow: 0 0 0 2px rgba(255,255,255,0.05);
    }

    .stat-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 8px;
      color: var(--muted);
      font-size: 11px;
      margin-top: 6px;
    }

    .stat-row strong {
      color: var(--text);
      font-weight: 800;
    }

    .controls-grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0,1fr));
      gap: 8px;
    }

    button {
      appearance: none;
      border: none;
      cursor: pointer;
      border-radius: 12px;
      min-height: 46px;
      padding: 10px 12px;
      color: white;
      font-weight: 800;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      font-size: 10px;
      position: relative;
      overflow: hidden;
      transition: transform 0.15s ease, box-shadow 0.15s ease, opacity 0.15s ease;
      box-shadow: 0 14px 22px rgba(0,0,0,0.2);
    }

    button:hover {
      transform: translateY(-1px);
    }

    button:active {
      transform: translateY(0);
    }

    button:disabled {
      cursor: not-allowed;
      opacity: 0.45;
      box-shadow: none;
      transform: none;
    }

    .primary-btn {
      background: linear-gradient(135deg, var(--primary), var(--primary-2));
      box-shadow: 0 14px 24px rgba(109,124,255,0.26);
    }

    .success-btn {
      background: linear-gradient(135deg, #1fca7a, #22b976);
      box-shadow: 0 12px 24px rgba(39,208,124,0.24);
    }

    .warning-btn {
      background: linear-gradient(135deg, #ffb322, #ff930a);
      box-shadow: 0 12px 24px rgba(255,177,26,0.24);
    }

    .danger-btn {
      background: linear-gradient(135deg, #ff6c70, #ff4d8d);
      box-shadow: 0 12px 24px rgba(255,92,168,0.22);
    }

    .secondary-btn {
      background: linear-gradient(135deg, #1d2c3e, #0f1a2a);
      border: 1px solid rgba(255,255,255,0.08);
    }

    .selected-box {
      min-height: 68px;
      border-radius: 12px;
      padding: 12px 14px;
      background: linear-gradient(180deg, rgba(109,124,255,0.08), rgba(109,124,255,0.02));
      border: 1px solid rgba(109,124,255,0.34);
      color: var(--muted);
      font-size: 12px;
      line-height: 1.45;
      display: flex;
      align-items: center;
    }

    .info-box {
      min-height: 170px;
      max-height: 260px;
      overflow-y: auto;
      background: rgba(8,12,19,0.82);
      border: 1px solid var(--line);
      border-radius: 12px;
      padding: 12px;
      font-family: Consolas, monospace;
      font-size: 11px;
      line-height: 1.7;
      color: #dfeeff;
    }

    .msg {
      padding-left: 8px;
      margin-bottom: 6px;
      border-left: 2px solid rgba(138,110,251,0.7);
    }

    .msg strong {
      color: var(--gold);
    }

    .footer {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 18px;
      border-top: 1px solid var(--line);
      background: rgba(8, 14, 22, 0.72);
      backdrop-filter: blur(10px);
      font-size: 12px;
      color: var(--muted);
    }

    .footer-left {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .tiny {
      font-size: 10px;
      min-height: 34px;
      padding: 8px 10px;
      letter-spacing: 0.06em;
    }

    .modal-overlay {
      position: fixed;
      inset: 0;
      background: rgba(4, 9, 15, 0.8);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 999;
      backdrop-filter: blur(5px);
    }

    .modal-overlay.active {
      display: flex;
      animation: fadeIn 0.2s ease;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    .modal {
      width: min(520px, calc(100% - 32px));
      background: linear-gradient(180deg, rgba(18, 29, 43, 0.97), rgba(10, 17, 24, 0.97));
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 20px 18px 16px;
      box-shadow: 0 30px 60px rgba(0,0,0,0.44);
      animation: popIn 0.24s ease;
    }

    @keyframes popIn {
      from { opacity: 0; transform: translateY(10px) scale(0.98); }
      to { opacity: 1; transform: translateY(0) scale(1); }
    }

    .modal h3 {
      font-size: 20px;
      font-weight: 900;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      margin-bottom: 10px;
    }

    .modal p {
      color: var(--muted);
      font-size: 14px;
      line-height: 1.6;
      margin-bottom: 12px;
    }

    .modal .row {
      display: flex;
      gap: 10px;
      margin-top: 10px;
    }

    .modal input, .modal select {
      width: 100%;
      border: 1px solid rgba(255,255,255,0.08);
      background: rgba(255,255,255,0.04);
      color: var(--text);
      border-radius: 10px;
      padding: 10px 12px;
      outline: none;
      font-size: 14px;
    }

    .modal-actions {
      display: grid;
      grid-template-columns: repeat(2, minmax(0,1fr));
      gap: 10px;
      margin-top: 16px;
    }

    @media (max-width: 1200px) {
      .content {
        grid-template-columns: 1fr;
      }
      .sidebar {
        grid-template-columns: 1fr 1fr;
        grid-template-rows: auto auto;
      }
      .panel:last-child {
        grid-column: 1 / -1;
      }
    }

    @media (max-width: 720px) {
      .topbar-stats {
        display: none;
      }
      .content {
        padding: 10px;
        gap: 10px;
      }
      .board {
        border-width: 5px;
        gap: 2px;
        padding: 2px;
      }
      .cell .name {
        font-size: 6.4px;
      }
      .cell .price { font-size: 6px; }
      .players-grid {
        grid-template-columns: 1fr;
      }
      .sidebar {
        grid-template-columns: 1fr;
      }
      .controls-grid {
        grid-template-columns: 1fr;
      }
      .footer {
        padding: 0 12px;
      }
    }
  </style>
</head>
<body>
  <div class="app">
    <header class="topbar">
      <div class="brand">
        <div class="brand-badge">🎲</div>
        <span>Monopoly PRO</span>
      </div>

      <div class="topbar-stats">
        <div class="chip"><span>🎮</span><span>Игроков: <strong id="playersCount">4</strong></span></div>
        <div class="chip"><span>🏠</span><span>Домов: <strong id="totalHouses">0</strong></span></div>
        <div class="chip"><span>💰</span><span>Денег: <strong id="totalMoney">$6000</strong></span></div>
      </div>
    </header>

    <main class="content">
      <section class="board-shell">
        <div id="board" class="board"></div>
      </section>

      <aside class="sidebar">
        <div class="panel">
          <div class="panel-title">👥 Игроки</div>
          <div id="status" class="players-grid"></div>
        </div>

        <div class="panel">
          <div class="panel-title">🎮 Управление</div>
          <div class="controls-grid">
            <button id="rollBtn" class="success-btn">Бросить</button>
            <button id="buyBtn" class="success-btn">Купить</button>
            <button id="buildBtn" class="primary-btn">Дом</button>
            <button id="mortgageBtn" class="warning-btn">Залог</button>
            <button id="tradeBtn" class="danger-btn">Обмен</button>
            <button id="endTurnBtn" class="secondary-btn">Ход</button>
          </div>
        </div>

        <div class="panel">
          <div id="selectedCell" class="selected-box">Выберите клетку на поле.</div>
        </div>

        <div class="panel">
          <div class="panel-title">📜 Лог</div>
          <div id="logBox" class="info-box"></div>
        </div>
      </aside>
    </main>

    <footer class="footer">
      <div class="footer-left">
        <span>💡 Готово к игре</span>
      </div>
      <div class="footer-right">
        <button id="newGameBtn" class="secondary-btn tiny">Новая игра</button>
      </div>
    </footer>
  </div>

  <div id="modalOverlay" class="modal-overlay">
    <div class="modal">
      <h3 id="modalTitle">Заголовок</h3>
      <p id="modalText">Текст</p>
      <div id="modalContent"></div>
      <div class="modal-actions">
        <button id="modalCancelBtn" class="secondary-btn">Отмена</button>
        <button id="modalConfirmBtn" class="success-btn">Подтвердить</button>
      </div>
    </div>
  </div>

  <script>
    const boardData = [
      { name: "СТАРТ", type: "start", price: 0, color: "#fff", group: "special" },
      { name: "Средиземное авеню", type: "property", price: 60, rent: 2, color: "#955436", group: "brown" },
      { name: "Общественная казна", type: "chance", price: 0, color: "#fff", group: "special" },
      { name: "Балтийское авеню", type: "property", price: 60, rent: 4, color: "#955436", group: "brown" },
      { name: "Подоходный налог", type: "tax", price: 50, color: "#fff", group: "special" },
      { name: "Железная дорога Чтение", type: "railroad", price: 200, rent: 25, color: "#9ca3af", group: "railroad" },
      { name: "Восточное авеню", type: "property", price: 100, rent: 6, color: "#00a2e8", group: "cyan" },
      { name: "Шанс", type: "chance", price: 0, color: "#fff", group: "special" },
      { name: "Коммунальное авеню", type: "property", price: 100, rent: 6, color: "#00a2e8", group: "cyan" },
      { name: "Коннектикут авеню", type: "property", price: 120, rent: 8, color: "#00a2e8", group: "cyan" },
      { name: "ТЮРЬМА", type: "jail", price: 0, color: "#fff", group: "special" },
      { name: "Святой Чарльз Плейс", type: "property", price: 140, rent: 10, color: "#c9328a", group: "pink" },
      { name: "Электрическая компания", type: "utility", price: 150, rent: 10, color: "#9ca3af", group: "utility" },
      { name: "Штаты авеню", type: "property", price: 140, rent: 10, color: "#c9328a", group: "pink" },
      { name: "Вирджиния авеню", type: "property", price: 160, rent: 12, color: "#c9328a", group: "pink" },
      { name: "Пенсильванская ж/д", type: "railroad", price: 200, rent: 25, color: "#9ca3af", group: "railroad" },
      { name: "Сан-Джеймс Плейс", type: "property", price: 180, rent: 14, color: "#ff7f27", group: "orange" },
      { name: "Общественная казна", type: "chance", price: 0, color: "#fff", group: "special" },
      { name: "Теннесси авеню", type: "property", price: 180, rent: 14, color: "#ff7f27", group: "orange" },
      { name: "Нью-Йорк авеню", type: "property", price: 200, rent: 16, color: "#ff7f27", group: "orange" },
      { name: "Бесплатная стоянка", type: "freeparking", price: 0, color: "#fff", group: "special" },
      { name: "Кентукки авеню", type: "property", price: 220, rent: 18, color: "#ed1c24", group: "red" },
      { name: "Шанс", type: "chance", price: 0, color: "#fff", group: "special" },
      { name: "Индиана авеню", type: "property", price: 220, rent: 18, color: "#ed1c24", group: "red" },
      { name: "Иллинойс авеню", type: "property", price: 240, rent: 20, color: "#ed1c24", group: "red" },
      { name: "Ж/д B&O", type: "railroad", price: 200, rent: 25, color: "#9ca3af", group: "railroad" },
      { name: "Атлантик авеню", type: "property", price: 260, rent: 22, color: "#fff200", group: "yellow" },
      { name: "Вентнор авеню", type: "property", price: 260, rent: 22, color: "#fff200", group: "yellow" },
      { name: "Водопровод", type: "utility", price: 150, rent: 10, color: "#9ca3af", group: "utility" },
      { name: "Марвин Гарденс", type: "property", price: 280, rent: 24, color: "#fff200", group: "yellow" },
      { name: "ИДИ В ТЮРЬМУ", type: "gotojail", price: 0, color: "#fff", group: "special" },
      { name: "Тихоокеанское авеню", type: "property", price: 300, rent: 26, color: "#22b14c", group: "green" },
      { name: "Северная Каролина", type: "property", price: 300, rent: 26, color: "#22b14c", group: "green" },
      { name: "Общественная казна", type: "chance", price: 0, color: "#fff", group: "special" },
      { name: "Пенсильвания авеню", type: "property", price: 320, rent: 28, color: "#22b14c", group: "green" },
      { name: "Короткая линия", type: "railroad", price: 200, rent: 25, color: "#9ca3af", group: "railroad" },
      { name: "Шанс", type: "chance", price: 0, color: "#fff", group: "special" },
      { name: "Парк Плейс", type: "property", price: 350, rent: 35, color: "#3f48cc", group: "blue" },
      { name: "Сверхналог", type: "tax", price: 100, color: "#fff", group: "special" },
      { name: "Бродвей", type: "property", price: 400, rent: 50, color: "#3f48cc", group: "blue" }
    ];

    const basePlayers = [
      { id: 0, name: "Красный", color: "red", money: 1500, position: 0, inJail: false, bankrupt: false, ai: false, properties: [] },
      { id: 1, name: "Синий", color: "blue", money: 1500, position: 0, inJail: false, bankrupt: false, ai: true, properties: [] },
      { id: 2, name: "Желтый", color: "yellow", money: 1500, position: 0, inJail: false, bankrupt: false, ai: true, properties: [] },
      { id: 3, name: "Зеленый", color: "green", money: 1500, position: 0, inJail: false, bankrupt: false, ai: true, properties: [] }
    ];

    let players = [];
    let board = [];
    const state = {
      current: 0,
      waitForAction: false,
      gameOver: false,
      lastRoll: [1, 1],
      log: [],
      selectedCell: null,
      rolling: false
    };

    const boardEl = document.getElementById("board");
    const statusEl = document.getElementById("status");
    const logBox = document.getElementById("logBox");
    const selectedCellEl = document.getElementById("selectedCell");
    const totalMoneyEl = document.getElementById("totalMoney");
    const totalHousesEl = document.getElementById("totalHouses");
    const modalOverlay = document.getElementById("modalOverlay");
    const modalTitle = document.getElementById("modalTitle");
    const modalText = document.getElementById("modalText");
    const modalContent = document.getElementById("modalContent");
    const modalCancelBtn = document.getElementById("modalCancelBtn");
    const modalConfirmBtn = document.getElementById("modalConfirmBtn");

    function clonePlayers() {
      return JSON.parse(JSON.stringify(basePlayers));
    }

    function resetGame() {
      players = clonePlayers();
      board = boardData.map((cell, idx) => ({
        ...cell,
        id: idx,
        owner: null,
        mortgaged: false,
        houses: 0
      }));

      state.current = 0;
      state.waitForAction = false;
      state.gameOver = false;
      state.lastRoll = [1,1];
      state.log = [];
      state.selectedCell = null;
      state.rolling = false;

      buildBoard();
      renderEverything();
      log("🎮 Игра началась. Ходит Красный.");
      log("💡 Важное: собирайте монополии, стройте дома и следите за деньгами.");
      updateTotals();
    }

    function log(msg) {
      state.log.push(msg);
      if (state.log.length > 80) state.log.shift();
      logBox.innerHTML = state.log.map(item => `<div class="msg">${item}</div>`).join("");
      logBox.scrollTop = logBox.scrollHeight;
    }

    function getCurrentPlayer() {
      return players[state.current];
    }

    function updateTotals() {
      const totalMoney = players.reduce((sum, p) => sum + p.money, 0);
      totalMoneyEl.textContent = "$" + totalMoney;
      totalHousesEl.textContent = board.reduce((sum, cell) => sum + cell.houses, 0);
      document.getElementById("playersCount").textContent = players.length;
    }

    function buildBoard() {
      boardEl.innerHTML = "";
      const center = document.createElement("div");
      center.className = "board-center";
      center.innerHTML = `
        <div class="logo">MONOPOLY</div>
        <div class="dice-box">
          <div id="die1" class="die">1</div>
          <div id="die2" class="die">1</div>
        </div>
      `;
      boardEl.appendChild(center);

      board.forEach((cell, index) => {
        const div = document.createElement("div");
        div.className = "cell " + (["property","railroad","utility"].includes(cell.type) ? "" : "special");
        div.dataset.index = String(index);

        if (cell.color && cell.color !== "#fff") {
          const bar = document.createElement("div");
          bar.className = "bar";
          bar.style.background = cell.color;
          div.appendChild(bar);
        }

        const name = document.createElement("div");
        name.className = "name";
        name.textContent = cell.name;
        div.appendChild(name);

        if (cell.price > 0) {
          const price = document.createElement("div");
          price.className = "price";
          price.textContent = "$" + cell.price;
          div.appendChild(price);
        }

        const owner = document.createElement("div");
        owner.className = "owner";
        owner.id = "owner-" + index;
        div.appendChild(owner);

        const layer = document.createElement("div");
        layer.className = "cell-token-layer";
        layer.id = "tokens-" + index;
        div.appendChild(layer);

        div.addEventListener("click", () => {
          selectCell(index);
        });

        boardEl.appendChild(div);
      });

      renderBoardTokens();
    }

    function renderBoardTokens() {
      for (let i = 0; i < board.length; i++) {
        const layer = document.getElementById("tokens-" + i);
        if (!layer) continue;
        layer.innerHTML = "";
        players.forEach((player, pIndex) => {
          if (player.position !== i || player.bankrupt) return;
          const token = document.createElement("div");
          token.className = "token " + player.color;
          token.title = player.name;
          token.textContent = player.name[0];
          const offsetX = 8 + (pIndex % 2) * 7;
          const offsetY = 8 + Math.floor(pIndex / 2) * 7;
          token.style.left = offsetX + "px";
          token.style.top = offsetY + "px";
          layer.appendChild(token);
        });
      }
    }

    function renderOwners() {
      board.forEach((cell, idx) => {
        const el = document.getElementById("owner-" + idx);
        if (!el) return;
        if (cell.owner !== null) {
          el.textContent = "В: " + players[cell.owner].name;
        } else {
          el.textContent = cell.mortgaged ? "Залог" : "";
        }
      });
    }

    function renderStatus() {
      statusEl.innerHTML = "";

      players.forEach((player, idx) => {
        const card = document.createElement("div");
        const active = idx === state.current && !state.gameOver;
        card.className = `player-card ${active ? "active" : ""} ${player.bankrupt ? "bankrupt" : ""}`;

        const statusText = player.bankrupt ? "Банкрот" : player.inJail ? "Тюрьма" : "Играет";
        card.innerHTML = `
          <div class="player-name">
            <span class="player-dot" style="background:${player.color};"></span>
            ${player.name}
          </div>
          <div class="stat-row"><span>Деньги</span><strong>$${player.money}</strong></div>
          <div class="stat-row"><span>Позиция</span><strong>${player.position}</strong></div>
          <div class="stat-row"><span>Недвижимость</span><strong>${player.properties.length}</strong></div>
          <div class="stat-row"><span>Статус</span><strong>${statusText}</strong></div>
        `;
        statusEl.appendChild(card);
      });
    }

    function updateDice(a, b) {
      const d1 = document.getElementById("die1");
      const d2 = document.getElementById("die2");
      d1.textContent = a;
      d2.textContent = b;
      d1.classList.remove("rolling");
      d2.classList.remove("rolling");
      void d1.offsetWidth;
      d1.classList.add("rolling");
      d2.classList.add("rolling");
    }

    function selectCell(index) {
      state.selectedCell = index;
      const cell = board[index];

      document.querySelectorAll(".cell").forEach(el => el.classList.remove("selected"));
      const target = document.querySelector(`.cell[data-index="${index}"]`);
      if (target) target.classList.add("selected");

      const ownerText = cell.owner === null ? "Нет владельца" : "Владелец: " + players[cell.owner].name;
      const extra = ["property","railroad","utility"].includes(cell.type)
        ? ` | Цена: $${cell.price} | Аренда: $${cell.rent ?? 0} | Домов: ${cell.houses}`
        : "";
      selectedCellEl.textContent = `📍 ${cell.name} | ${cell.type} | ${ownerText}${extra}`;
    }

    function setButtons() {
      const p = getCurrentPlayer();
      const canAct = !state.gameOver && !p.bankrupt && !state.waitForAction;
      const canBuy = canBuyCurrentProperty();
      const canBuild = canBuildCurrentProperty();
      const canMortgage = hasMortgageAvailable(p);

      document.getElementById("rollBtn").disabled = !canAct || state.rolling;
      document.getElementById("buyBtn").disabled = !canAct || !canBuy;
      document.getElementById("buildBtn").disabled = !canAct || !canBuild;
      document.getElementById("mortgageBtn").disabled = !canAct || !canMortgage;
      document.getElementById("tradeBtn").disabled = !canAct || p.ai;
      document.getElementById("endTurnBtn").disabled = !(canAct && state.waitForAction);
    }

    function canBuyCurrentProperty() {
      const p = getCurrentPlayer();
      const cell = board[p.position];
      if (!["property","railroad","utility"].includes(cell.type)) return false;
      return cell.owner === null && p.money >= cell.price;
    }

    function hasMortgageAvailable(player) {
      return player.properties.some(index => board[index].owner === player.id && !board[index].mortgaged);
    }

    function canBuildCurrentProperty() {
      const p = getCurrentPlayer();
      const idx = p.position;
      const cell = board[idx];
      if (!cell || !["property","railroad","utility"].includes(cell.type)) return false;
      if (cell.owner !== p.id || cell.mortgaged) return false;

      const group = cell.group;
      const groupCells = board.filter(item => item.group === group && item.owner === p.id);
      if (groupCells.length === 0) return false;

      // Полная монополия по группе
      const groupOwned = board.filter(item => item.group === group && item.owner === p.id).length;
      const totalGroup = board.filter(item => item.group === group).length;
      if (groupOwned < totalGroup) return false;

      return true;
    }

    function buyCurrentProperty() {
      const p = getCurrentPlayer();
      const cell = board[p.position];
      if (!canBuyCurrentProperty()) return;

      p.money -= cell.price;
      cell.owner = p.id;
      p.properties.push(p.position);
      state.waitForAction = false;

      log(`🏠 ${p.name} купил ${cell.name} за $${cell.price}`);
      renderStatus();
      renderOwners();
      renderBoardTokens();
      updateTotals();
      setButtons();
      selectCell(p.position);
    }

    function playerOwnsFullGroup(player, group) {
      const groupCells = board.filter(item => item.group === group);
      if (!groupCells.length) return false;
      return groupCells.every(item => item.owner === player.id);
    }

    function housesCost(cell) {
      return Math.max(50, Math.floor(cell.price * 0.5));
    }

    function buildHouse() {
      const p = getCurrentPlayer();
      const idx = p.position;
      const cell = board[idx];

      if (!cell || cell.owner !== p.id || cell.mortgaged) {
        log("🚫 Нельзя строить здесь: эта клетка не принадлежит вам.");
        return;
      }

      const fullGroup = playerOwnsFullGroup(p, cell.group);
      if (!fullGroup) {
        log("🚫 Чтобы строить дом, нужно владеть всей группой.");
        return;
      }

      const cost = housesCost(cell);
      if (p.money < cost) {
        log(`🚫 Недостаточно денег для дома: нужно $${cost}.`);
        return;
      }

      const maxHouses = 4;
      if (cell.houses >= maxHouses) {
        log("🚫 На этом участке уже максимум домов.");
        return;
      }

      p.money -= cost;
      cell.houses += 1;
      log(`🏡 ${p.name} построил дом на ${cell.name} за $${cost}`);
      renderStatus();
      renderOwners();
      updateTotals();
      setButtons();
      selectCell(idx);
    }

    function payRent(player, cell) {
      const owner = players[cell.owner];
      if (!owner || owner.id === player.id) return;

      let rent = cell.rent ?? 0;

      if (cell.type === "utility") {
        rent = 10 * (player.position === 12 || player.position === 28 ? 1 : 1);
      }

      const sameGroupCount = board.filter(item => item.group === cell.group && item.owner === owner.id).length;
      if (sameGroupCount >= 2 && ["property","railroad","utility"].includes(cell.type)) {
        rent *= 2;
      }

      // Домами/гостями
      if (["property","railroad","utility"].includes(cell.type) && cell.houses > 0) {
        rent = cell.rent * (cell.houses + 1) * 2;
      }

      if (owner.properties.length > 0 && playerOwnsFullGroup(owner, cell.group)) {
        if (cell.houses === 0) rent *= 2;
      }

      if (player.money >= rent) {
        player.money -= rent;
        owner.money += rent;
        log(`💸 ${player.name} платит аренду ${owner.name}: $${rent}`);
      } else {
        owner.money += player.money;
        player.money = 0;
        log(`💀 ${player.name} не может оплатить аренду и банкротится!`);
        bankruptPlayer(player);
      }

      renderStatus();
      updateTotals();
      renderOwners();
    }

    function bankruptPlayer(player) {
      if (player.bankrupt) return;
      player.bankrupt = true;

      player.properties.forEach(index => {
        board[index].owner = null;
        board[index].mortgaged = false;
        board[index].houses = 0;
      });

      player.properties = [];
      player.money = 0;

      renderStatus();
      renderOwners();
      renderBoardTokens();
      updateTotals();
      checkWin();
      setButtons();
    }

    function handleChance(player) {
      const cards = [
        { text: "Случайная награда: +100", money: 100 },
        { text: "Штраф: -75", money: -75 },
        { text: "Переместились на старт: +200", money: 200, moveTo: 0 },
        { text: "Иди в тюрьму", moveTo: 10, jail: true },
        { text: "Премия: +150", money: 150 },
        { text: "Комиссия: -60", money: -60 }
      ];

      const card = cards[Math.floor(Math.random() * cards.length)];
      log(`🎴 ${player.name}: ${card.text}`);

      if (card.money) player.money += card.money;

      if (card.moveTo !== undefined) {
        player.position = card.moveTo;
        if (card.jail) {
          player.inJail = true;
          log(`🚓 ${player.name} отправляется в тюрьму`);
        } else {
          player.money += 200;
          log(`💰 ${player.name} получил $200 за старта`);
        }
      }

      renderStatus();
      updateTotals();
    }

    function handleCellEffect(player) {
      const cell = board[player.position];

      if (cell.type === "tax") {
        if (player.money >= cell.price) {
          player.money -= cell.price;
          log(`💸 ${player.name} уплатил налог $${cell.price}`);
        } else {
          log(`💀 ${player.name} не может уплатить налог и банкротится`);
          bankruptPlayer(player);
          return;
        }
      }

      if (cell.type === "chance") {
        handleChance(player);
      }

      if (cell.type === "gotojail") {
        player.position = 10;
        player.inJail = true;
        log(`🚓 ${player.name} отправляется в тюрьму`);
      }

      if (["property","railroad","utility"].includes(cell.type)) {
        if (cell.owner === null) {
          if (player.money >= cell.price) {
            if (player.ai) {
              player.money -= cell.price;
              cell.owner = player.id;
              player.properties.push(player.position);
              log(`🤖 ${player.name} автоматически купил ${cell.name} за $${cell.price}`);
            } else {
              state.waitForAction = true;
              log(`📌 ${player.name} может купить ${cell.name} за $${cell.price}`);
            }
          } else {
            log(`⚠️ ${player.name} не может купить ${cell.name}: недостаточно денег`);
          }
        } else if (cell.owner !== player.id) {
          payRent(player, cell);
        }
      }

      renderStatus();
      renderOwners();
      renderBoardTokens();
      updateTotals();
      setButtons();
      selectCell(player.position);
    }

    function processMove(player, totalSteps) {
      const start = player.position;
      const end = (start + totalSteps) % 40;

      if (end < start) {
        player.money += 200;
        log(`💰 ${player.name} прошел старт и получил $200`);
      }

      player.position = end;
      renderBoardTokens();
      renderStatus();

      setTimeout(() => {
        if (!state.gameOver) handleCellEffect(player);
      }, 260);
    }

    function doRoll(player, d1, d2) {
      if (state.gameOver) return;

      state.rolling = true;
      state.lastRoll = [d1, d2];
      updateDice(d1, d2);
      log(`🎲 ${player.name} бросил ${d1} и ${d2} (всего ${d1 + d2})`);

      if (player.inJail) {
        if (d1 === d2) {
          player.inJail = false;
          log(`🔓 ${player.name} выбросил дубль и вышел из тюрьмы`);
        } else {
          log(`🚫 ${player.name} не выбросил дубль и остаётся в тюрьме`);
          state.rolling = false;
          endTurn();
          return;
        }
      }

      processMove(player, d1 + d2);

      setTimeout(() => {
        state.rolling = false;
        setButtons();
      }, 320);
    }

    function rollTurn() {
      if (state.gameOver) return;
      const p = getCurrentPlayer();
      if (p.bankrupt) return;

      if (p.ai) {
        doAITurn(p);
        return;
      }

      doRoll(p, randomDie(), randomDie());
    }

    function doAITurn(player) {
      if (state.gameOver || player.bankrupt) return;
      const d1 = randomDie();
      const d2 = randomDie();
      doRoll(player, d1, d2);
    }

    function randomDie() {
      return Math.floor(Math.random() * 6) + 1;
    }

    function checkWin() {
      const alive = players.filter(p => !p.bankrupt);
      if (alive.length <= 1) {
        state.gameOver = true;
        const winner = alive[0];
        log(`🎉 Игра окончена! Победитель: <strong>${winner ? winner.name : "Никто"}</strong>`);
        showModal("Игра завершена", winner ? `${winner.name} победил!` : "Ничья. Победителя нет.", null);
        document.getElementById("rollBtn").disabled = true;
        document.getElementById("buyBtn").disabled = true;
        document.getElementById("buildBtn").disabled = true;
        document.getElementById("mortgageBtn").disabled = true;
        document.getElementById("tradeBtn").disabled = true;
        document.getElementById("endTurnBtn").disabled = true;
      }
    }

    function endTurn() {
      if (state.gameOver) return;
      state.waitForAction = false;

      let next = (state.current + 1) % players.length;
      let attempts = 0;

      while (players[next].bankrupt && attempts < players.length) {
        next = (next + 1) % players.length;
        attempts++;
      }

      state.current = next;
      log(`➡️ Ход переходит к ${players[state.current].name}`);
      renderStatus();
      setButtons();
      checkWin();

      if (!state.gameOver && players[state.current].ai) {
        setTimeout(() => {
          if (!state.gameOver) doAITurn(players[state.current]);
        }, 700);
      }
    }

    function showModal(title, text, contentHtml) {
      modalTitle.textContent = title;
      modalText.textContent = text;
      modalContent.innerHTML = contentHtml || "";
      modalOverlay.classList.add("active");
    }

    function hideModal() {
      modalOverlay.classList.remove("active");
    }

    function startAuction() {
      if (state.gameOver) return;
      const p = getCurrentPlayer();
      const cell = board[p.position];

      if (cell.owner !== null) {
        log("⚠️ Нельзя начать аукцион: клетка уже принадлежит кому-то.");
        return;
      }

      const participants = players.filter(pl => !pl.bankrupt && pl.money > 0);
      let highestBid = { playerId: p.id, amount: 0 };

      participants.forEach(pl => {
        if (pl.ai) {
          const offer = Math.max(10, Math.floor(Math.random() * (pl.money * 0.35)));
          if (offer > highestBid.amount) {
            highestBid = { playerId: pl.id, amount: offer };
          }
        }
      });

      const startingBid = Math.max(10, highestBid.amount + 10);

      showModal("Аукцион", `Сделайте ставку на ${cell.name}`, `
        <div class="row">
          <input id="auctionInput" type="number" min="10" value="${startingBid}" />
        </div>
      `);

      modalCancelBtn.onclick = hideModal;
      modalConfirmBtn.onclick = () => {
        const bid = Number(document.getElementById("auctionInput").value);
        if (!Number.isFinite(bid) || bid < 10) {
          log("⚠️ Некорректная ставка.");
          hideModal();
          return;
        }

        const winner = p;
        winner.money -= bid;
        cell.owner = winner.id;
        winner.properties.push(p.position);

        log(`🏷️ ${winner.name} выиграл аукцион за $${bid}`);
        hideModal();
        renderStatus();
        renderOwners();
        renderBoardTokens();
        updateTotals();
        setButtons();
      };
    }

    function mortgageSelectedProperty() {
      const p = getCurrentPlayer();
      const available = p.properties.filter(index => board[index].owner === p.id && !board[index].mortgaged);

      if (!available.length) {
        log("🚫 У вас нет заложимых объектов.");
        return;
      }

      showModal("Залог", "Выберите объект для залога.", `
        <div class="row">
          <select id="mortgageSelect">
            ${available.map(index => `<option value="${index}">${board[index].name}</option>`).join("")}
          </select>
        </div>
      `);

      modalCancelBtn.onclick = hideModal;
      modalConfirmBtn.onclick = () => {
        const chosen = Number(document.getElementById("mortgageSelect").value);
        const cell = board[chosen];
        const amount = Math.floor(cell.price / 2);
        p.money += amount;
        cell.mortgaged = true;

        log(`🏦 ${p.name} заложил ${cell.name} и получил $${amount}`);
        hideModal();
        renderStatus();
        renderOwners();
        updateTotals();
        setButtons();
      };
    }

    function tradeProperty() {
      const p = getCurrentPlayer();
      if (p.ai || !p.properties.length) {
        log("🚫 У вас нет собственности для обмена.");
        return;
      }

      const targets = players.filter(pl => pl.id !== p.id && !pl.bankrupt);
      if (!targets.length) {
        log("🚫 Нет подходящих игроков для обмена.");
        return;
      }

      showModal("Обмен", "Выберите игрока и собственность.", `
        <div class="row">
          <select id="tradeTarget">
            ${targets.map(pl => `<option value="${pl.id}">${pl.name}</option>`).join("")}
          </select>
        </div>
        <div class="row">
          <select id="tradeFrom">
            ${p.properties.map(index => `<option value="${index}">${board[index].name}</option>`).join("")}
          </select>
        </div>
      `);

      modalCancelBtn.onclick = hideModal;
      modalConfirmBtn.onclick = () => {
        const targetId = Number(document.getElementById("tradeTarget").value);
        const fromIndex = Number(document.getElementById("tradeFrom").value);
        const target = players[targetId];

        if (!target || !target.properties.length) {
          log("⚠️ У выбранного игрока нет собственности для обмена.");
          hideModal();
          return;
        }

        const tradeToHtml = `
          <div class="row">
            <select id="tradeTo">
              ${target.properties.map(index => `<option value="${index}">${board[index].name}</option>`).join("")}
            </select>
          </div>
        `;

        modalText.textContent = `Вы обмениваете ${board[fromIndex].name} на одну из собственностей ${target.name}.`;
        modalContent.innerHTML = tradeToHtml;

        modalConfirmBtn.onclick = () => {
          const toIndex = Number(document.getElementById("tradeTo").value);

          if (!p.properties.includes(fromIndex) || !target.properties.includes(toIndex)) {
            log("⚠️ Ошибка обмена: некорректная собственность.");
            hideModal();
            return;
          }

          const pIdx = p.properties.indexOf(fromIndex);
          const tIdx = target.properties.indexOf(toIndex);

          p.properties.splice(pIdx, 1);
          target.properties.splice(tIdx, 1);

          p.properties.push(toIndex);
          target.properties.push(fromIndex);

          board[fromIndex].owner = target.id;
          board[toIndex].owner = p.id;

          log(`🤝 ${p.name} обменял ${board[fromIndex].name} на ${board[toIndex].name} с ${target.name}`);
          hideModal();
          renderOwners();
          renderStatus();
          renderBoardTokens();
          updateTotals();
          setButtons();
        };
      };
    }

    document.getElementById("rollBtn").addEventListener("click", rollTurn);
    document.getElementById("buyBtn").addEventListener("click", buyCurrentProperty);
    document.getElementById("buildBtn").addEventListener("click", buildHouse);
    document.getElementById("auctionBtn")?.addEventListener("click", startAuction);
    document.getElementById("mortgageBtn").addEventListener("click", mortgageSelectedProperty);
    document.getElementById("tradeBtn").addEventListener("click", tradeProperty);
    document.getElementById("endTurnBtn").addEventListener("click", endTurn);
    document.getElementById("newGameBtn").addEventListener("click", resetGame);

    modalCancelBtn.addEventListener("click", hideModal);
    modalConfirmBtn.addEventListener("click", hideModal);

    function renderEverything() {
      renderStatus();
      renderOwners();
      renderBoardTokens();
      updateTotals();
      setButtons();
      selectCell(0);
    }

    // Кнопка аукциона отсутствовала в HTML, добавим её динамически.
    const auctionBtn = document.createElement("button");
    auctionBtn.id = "auctionBtn";
    auctionBtn.className = "primary-btn";
    auctionBtn.textContent = "Аукцион";
    const controls = document.querySelector(".controls-grid");
    controls.appendChild(auctionBtn);
    auctionBtn.addEventListener("click", startAuction);

    resetGame();
  </script>
</body>
</html>
