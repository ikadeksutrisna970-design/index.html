<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KADEK CAPITAL</title>
    <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Share+Tech+Mono&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lightweight-charts@4.1.0/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        :root { --bg:#060608; --panel:#0c0c10; --orange:#ff6600; --green:#00d97e; --red:#ff2d55; --cyan:#00c8ff; }
        body { background:var(--bg); color:#c0c8d8; font-family:'Inter',sans-serif; margin:0; padding:0; }
        .wrap { max-width:720px; margin:0 auto; padding:10px; }
        h1 { text-align:center; color:var(--orange); font-family:'Bebas Neue',sans-serif; font-size:32px; letter-spacing:3px; margin:10px 0; }
        .subtitle { text-align:center; color:#ffcc00; font-size:17px; }
        #price { font-size:62px; font-family:'Bebas Neue',sans-serif; text-align:center; margin:8px 0; }
        #chartEl { height:360px; margin:15px 0; background:var(--panel); }
        .status { padding:14px; background:rgba(255,183,0,0.2); margin:10px 0; border-radius:8px; text-align:center; font-family:'Share Tech Mono',monospace; }
        .tg-status { padding:8px 12px; border-radius:6px; font-size:13px; display:inline-block; margin:8px 0; }
        button { width:100%; padding:18px; margin:12px 0; font-size:20px; font-weight:bold; border:none; border-radius:8px; cursor:pointer; }
    </style>
</head>
<body>

<div class="wrap">
    <h1>KADEK CAPITAL</h1>
    <div class="subtitle">BTC-USD • M15 • AUTO SIGNAL</div>
    
    <div style="text-align:center;">
        <span class="tg-status" id="tgStatus">🔴 Telegram Not Connected</span>
    </div>

    <div class="status" id="status">🚀 Menghubungkan sistem...</div>
    <div id="price" style="color:#00ffaa;">——</div>
    <div id="chartEl"></div>

    <button onclick="toggleAuto()" id="autoBtn" style="background:#00d97e;color:black;">🤖 AKTIFKAN AUTO SIGNAL TELEGRAM</button>
</div>

<script>
const BOT_TOKEN = "8596503581:AAHyxjtTkaeutnvy62VlOPTfOr2EmN5tMBg";
const CHAT_ID = "5132441992";

let chart, candleSeries;
let autoInterval = null;
let isAuto = false;
let tgConnected = false;

// Test Telegram Connection
async function testTelegram() {
    const tgEl = document.getElementById('tgStatus');
    try {
        const res = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/getMe`);
        if (res.ok) {
            tgConnected = true;
            tgEl.innerHTML = "🟢 Telegram Terhubung";
            tgEl.style.background = "rgba(0,217,126,0.2)";
            tgEl.style.color = "#00d97e";
        }
    } catch(e) {
        tgEl.innerHTML = "🔴 Telegram Gagal Terhubung";
    }
}

async function fetchData() { ... } // (sama seperti sebelumnya, saya ringkas agar tidak terlalu panjang)

function toggleAuto() { ... } // (sama seperti sebelumnya)

window.onload = () => {
    testTelegram(); // Test koneksi Telegram saat load
    // ... inisialisasi chart dan fetchData
};
</script>
</body>
</html>
