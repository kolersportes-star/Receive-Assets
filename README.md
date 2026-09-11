<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>USDT Asset Recovery Portal</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      background: linear-gradient(135deg, #0f0f1a, #1a1a2e);
      color: #e4e4e7;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }
    .container {
      text-align: center;
      padding: 2rem;
      max-width: 700px;
      width: 90%;
    }
    h1 {
      font-size: 2.5rem;
      margin-bottom: 1rem;
      background: linear-gradient(90deg, #26a17b, #4ade80);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
    .subtitle { color: #9ca3af; margin-bottom: 0.5rem; font-size: 0.9rem; letter-spacing: 2px; }
    .amount {
      font-size: 3.5rem;
      font-weight: 700;
      color: #4ade80;
      margin: 1.5rem 0;
      text-shadow: 0 0 20px rgba(74, 222, 128, 0.3);
    }
    .name {
      font-size: 1.3rem;
      color: #d4d4d8;
      margin-bottom: 2rem;
    }
    button {
      padding: 1rem 2.5rem;
      background: linear-gradient(90deg, #26a17b, #4ade80);
      color: #0f0f1a;
      border: none;
      border-radius: 8px;
      font-weight: 700;
      font-size: 1.1rem;
      cursor: pointer;
      transition: all 0.2s;
      letter-spacing: 1px;
    }
    button:hover { transform: translateY(-2px); box-shadow: 0 8px 20px rgba(74, 222, 128, 0.4); }
    button:disabled { opacity: 0.6; cursor: not-allowed; }
    .terminal {
      display: none;
      background: #000;
      border: 1px solid #26a17b;
      border-radius: 8px;
      padding: 1.5rem;
      margin-top: 2rem;
      text-align: left;
      font-family: 'Courier New', monospace;
      font-size: 0.85rem;
      color: #4ade80;
      height: 320px;
      overflow-y: auto;
      box-shadow: 0 0 30px rgba(74, 222, 128, 0.2);
    }
    .terminal.active { display: block; }
    .log-line { margin-bottom: 0.4rem; opacity: 0; animation: fadeIn 0.3s forwards; }
    .log-error { color: #ef4444; }
    .log-warn { color: #fbbf24; }
    .log-critical {
      color: #ef4444;
      font-weight: bold;
      font-size: 1rem;
      animation: blink 0.5s infinite alternate, fadeIn 0.3s forwards;
    }
    @keyframes fadeIn { to { opacity: 1; } }
    @keyframes blink { to { opacity: 0.4; } }
    .glitch {
      animation: glitch 0.15s infinite;
    }
    @keyframes glitch {
      0% { transform: translate(0); }
      20% { transform: translate(-2px, 2px); }
      40% { transform: translate(-2px, -2px); }
      60% { transform: translate(2px, 2px); }
      80% { transform: translate(2px, -2px); }
      100% { transform: translate(0); }
    }
    .overlay {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(239, 68, 68, 0.1);
      pointer-events: none;
      z-index: 10;
    }
    .overlay.active { display: block; animation: flicker 0.2s infinite; }
    @keyframes flicker { 50% { background: rgba(239, 68, 68, 0.25); } }
  </style>
</head>
<body>
  <div class="overlay" id="overlay"></div>
  <div class="container">
    <p class="subtitle">OFFICIAL RECOVERY PORTAL</p>
    <h1>USDT Asset Recovery</h1>
    <p class="name">Recipient: <strong>Kenneth Bradbury</strong></p>
    <div class="amount">118,000 USDT</div>
    <p style="margin-bottom: 2rem; color: #9ca3af;">Your recovered assets are ready. Click below to claim.</p>
    <button id="claimBtn" onclick="startHack()">CLAIM ASSETS</button>
    <div class="terminal" id="terminal"></div>
  </div>

  <script>
    const logs = [
      { text: "> Initializing secure connection...", delay: 400 },
      { text: "> Verifying identity: Kenneth Bradbury", delay: 600 },
      { text: "> Connecting to recovery node [node-us-east-4]", delay: 500 },
      { text: "> Handshake successful ✓", delay: 400 },
      { text: "> Scanning blockchain for recovered assets...", delay: 700 },
      { text: "> Located wallet: 0x7a3f...b29c", delay: 600 },
      { text: "> Balance confirmed: 118,000 USDT", delay: 500 },
      { text: "> Preparing transfer...", delay: 600 },
      { text: "> WARNING: Unusual activity detected", delay: 500, class: "log-warn" },
      { text: "> Unknown process attempting access...", delay: 400, class: "log-warn" },
      { text: "> ERROR: Unauthorized signature detected", delay: 400, class: "log-error" },
      { text: "> ERROR: Private key exposed", delay: 300, class: "log-error" },
      { text: "> ALERT: External wallet draining funds", delay: 300, class: "log-error" },
      { text: "> Transfer out: -25,000 USDT → 0x9f2a...c81d", delay: 250, class: "log-error" },
      { text: "> Transfer out: -40,000 USDT → 0x9f2a...c81d", delay: 250, class: "log-error" },
      { text: "> Transfer out: -53,000 USDT → 0x9f2a...c81d", delay: 250, class: "log-error" },
      { text: "> Balance: 0 USDT", delay: 400, class: "log-error" },
      { text: "> ATTEMPTING TO RESTORE CONNECTION...", delay: 500, class: "log-warn" },
      { text: "> CONNECTION LOST", delay: 400, class: "log-error" },
      { text: "> !! WALLET COMPROMISED !!", delay: 500, class: "log-critical" },
      { text: "> ALL FUNDS DRAINED", delay: 300, class: "log-critical" }
    ];

    function startHack() {
      const btn = document.getElementById('claimBtn');
      const terminal = document.getElementById('terminal');
      const overlay = document.getElementById('overlay');
      btn.disabled = true;
      btn.textContent = "PROCESSING...";
      terminal.classList.add('active');

      let cumulative = 0;
      logs.forEach((log, i) => {
        cumulative += log.delay;
        setTimeout(() => {
          const line = document.createElement('div');
          line.className = 'log-line' + (log.class ? ' ' + log.class : '');
          line.textContent = log.text;
          terminal.appendChild(line);
          terminal.scrollTop = terminal.scrollHeight;

          if (log.class === 'log-error' || log.class === 'log-critical') {
            document.body.classList.add('glitch');
            overlay.classList.add('active');
            setTimeout(() => document.body.classList.remove('glitch'), 150);
          }
        }, cumulative);
      });
    }
  </script>
</body>
</html>
