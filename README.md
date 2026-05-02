<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KADEK CAPITAL - BTC Auto Signal</title>
    <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Share+Tech+Mono&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lightweight-charts@4.1.0/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        :root { --bg:#060608; --panel:#0c0c10; --orange:#ff6600; --green:#00d97e; --red:#ff2d55; }
        body { background:var(--bg); color:#c0c8d8; font-family:'Inter',sans-serif; margin:0; padding:0; }
        .wrap { max-width:720px; margin:0 auto; padding:10px; }
        h1 { text-align:center; color:var(--orange); font-family:'Bebas Neue',sans-serif; font-size:32px; margin:10px 0; letter-spacing:2px; }
        .subtitle { text-align:center; color:#ff8800; font-size:18px; margin-bottom:10px; }
        #price { font-size:58px; font-family:'Bebas Neue',sans-serif; text-align:center; margin:10px 0; }
        #chartEl { height:380px; margin:15px 0; background:var(--panel); }
        .status { padding:14px; text-align:center; background:rgba(255,183,0,0.2); margin:10px 0; border-radius:8px; font-family:'Share Tech Mono',monospace; }
        button { width:100%; padding:18px; margin:12px 0; font-size:20px; font-weight:bold; border:none; border-radius:8px; cursor:pointer; }
    </style>
</head>
<body>

<div class="wrap">
    <h1>KADEK CAPITAL</h1>
    <div class="subtitle">BTCUSDT • M15 • AUTO SIGNAL</div>
    
    <div class="status" id="status">🚀 Sistem siap scanning...</div>
    <div id="price" style="color:#00ffaa;">——</div>
    <div id="chartEl"></div>

    <button onclick="toggleAuto()" id="autoBtn" style="background:#00d97e; color:black;">🤖 AKTIFKAN AUTO SCAN + TELEGRAM</button>
</div>

<script>
// === CONFIG ===
const BOT_TOKEN = "8596503581:AAHyxjtTkaeutnvy62VlOPTfOr2EmN5tMBg";
const CHAT_ID = "5132441992";

let chart, candleSeries;
let autoInterval = null;
let isAuto = false;

// Inisialisasi Chart
window.onload = () => {
    const el = document.getElementById('chartEl');
    chart = LightweightCharts.createChart(el, { width: el.clientWidth, height: 380, layout: { background: { color: '#0c0c10' } } });
    candleSeries = chart.addCandlestickSeries();
    fetchData();
};

// Ambil data & analisa
async function fetchData() {
    try {
        const res = await fetch("https://api.bybit.com/v5/market/kline?category=linear&symbol=BTCUSDT&interval=15&limit=80");
        const json = await res.json();
        const raw = json.result.list.reverse();

        const candles = raw.map(d => ({
            time: Math.floor(d[0]/1000),
            open: +d[1], high: +d[2], low: +d[3], close: +d[4]
        }));

        candleSeries.setData(candles);
        const last = candles[candles.length-1];
        document.getElementById('price').textContent = last.close.toFixed(2);

        analyzeSignal(candles);
    } catch(e) {
        document.getElementById('status').innerHTML = "❌ Gagal ambil data BTC";
    }
}

// Logic Signal yang lebih rame
function analyzeSignal(candles) {
    const last = candles[candles.length-1];
    const prev = candles[candles.length-2];

    const change = ((last.close - prev.close) / prev.close) * 100;

    if (change > 0.6) {
        sendTelegramSignal("🟢 BUY", last.close, "STRONG MOMENTUM");
    } else if (change < -0.6) {
        sendTelegramSignal("🔴 SELL", last.close, "BEARISH PRESSURE");
    } else {
        document.getElementById('status').innerHTML = `📊 Sideways • Change ${change.toFixed(2)}%`;
    }
}

async function sendTelegramSignal(type, price, reason) {
    const tp = (type.includes("BUY") ? price * 1.018 : price * 0.982).toFixed(2);
    const sl = (type.includes("BUY") ? price * 0.988 : price * 1.015).toFixed(2);

    const message = `${type} BTCUSDT\n\n` +
                    `💰 Entry : ${price}\n` +
                    `🎯 TP     : ${tp}\n` +
                    `🛑 SL     : ${sl}\n` +
                    `📈 Reason : ${reason}\n` +
                    `⏰ Time   : ${new Date().toLocaleTimeString('id-ID')}\n\n` +
                    `⚠️ Risk Management : Max 1-2%`;

    document.getElementById('status').innerHTML = `📤 Mengirim ${type} ke Telegram...`;

    try {
        await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ chat_id: CHAT_ID, text: message })
        });
        document.getElementById('status').innerHTML = `✅ ${type} SIGNAL TERKIRIM!`;
    } catch(e) {
        document.getElementById('status').innerHTML = `⚠️ Signal terdeteksi tapi gagal kirim`;
    }
}

function toggleAuto() {
    isAuto = !isAuto;
    const btn = document.getElementById('autoBtn');
    if (isAuto) {
        btn.style.background = "#ff6600";
        btn.textContent = "⏹ STOP AUTO SCAN";
        autoInterval = setInterval(fetchData, 25000);
        fetchData();
    } else {
        btn.style.background = "#00d97e";
        btn.textContent = "🤖 AKTIFKAN AUTO SCAN + TELEGRAM";
        clearInterval(autoInterval);
    }
}
</script>
</body>
</html>
