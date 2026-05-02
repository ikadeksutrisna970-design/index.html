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
        #chartEl { height:380px; margin:15px 0; background:var(--panel); }
        .status { padding:14px; background:rgba(255,183,0,0.2); margin:10px 0; border-radius:8px; text-align:center; font-family:'Share Tech Mono',monospace; }
        button { width:100%; padding:18px; margin:12px 0; font-size:20px; font-weight:bold; border:none; border-radius:8px; cursor:pointer; }
    </style>
</head>
<body>

<div class="wrap">
    <h1>KADEK CAPITAL</h1>
    <div class="subtitle">BTCUSDT • M15 • SMART AUTO SIGNAL</div>
    
    <div class="status" id="status">🚀 Menghubungkan ke pasar...</div>
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

async function fetchWithProxy() {
    const proxies = [
        url => url,
        url => `https://corsproxy.io/?${encodeURIComponent(url)}`,
        url => `https://api.allorigins.win/raw?url=${encodeURIComponent(url)}`
    ];

    for (let proxy of proxies) {
        try {
            const res = await fetch(proxy("https://api.bybit.com/v5/market/kline?category=linear&symbol=BTCUSDT&interval=15&limit=100"));
            if (res.ok) {
                const json = await res.json();
                return json.result.list.reverse();
            }
        } catch(e) {}
    }
    throw new Error("Gagal");
}

async function fetchData() {
    const status = document.getElementById('status');
    status.textContent = "📡 Mengambil data BTC...";

    try {
        const raw = await fetchWithProxy();
        const candles = raw.map(d => ({
            time: Math.floor(d[0]/1000),
            open: +d[1], high: +d[2], low: +d[3], close: +d[4]
        }));

        candleSeries.setData(candles);
        const last = candles[candles.length-1];
        document.getElementById('price').textContent = last.close.toFixed(2);

        analyzeSignal(candles);
    } catch(e) {
        document.getElementById('status').innerHTML = "❌ Masih gagal ambil data. Coba refresh.";
    }
}

function analyzeSignal(candles) {
    const last = candles[candles.length-1];
    const change = ((last.close - candles[candles.length-2].close) / candles[candles.length-2].close) * 100;

    if (Math.abs(change) > 0.5) {
        const type = change > 0 ? "🟢 STRONG BUY" : "🔴 STRONG SELL";
        sendSignal(type, last.close, change);
    }
}

async function sendSignal(type, price, change) {
    const tp = (type.includes("BUY") ? price * 1.02 : price * 0.98).toFixed(2);
    const sl = (type.includes("BUY") ? price * 0.985 : price * 1.02).toFixed(2);

    const msg = `${type} BTCUSDT\n\n` +
                `💰 Price : ${price}\n` +
                `📈 Change : ${change.toFixed(2)}%\n` +
                `🎯 TP : ${tp}\n` +
                `🛑 SL : ${sl}\n` +
                `⏰ ${new Date().toLocaleString('id-ID')}`;

    try {
        await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify({chat_id: CHAT_ID, text: msg})
        });
        document.getElementById('status').innerHTML = `✅ ${type} SIGNAL TERKIRIM!`;
    } catch(e) {}
}

function toggleAuto() {
    isAuto = !isAuto;
    const btn = document.getElementById('autoBtn');
    if (isAuto) {
        btn.textContent = "⏹ STOP AUTO SIGNAL";
        btn.style.background = "#ff6600";
        autoInterval = setInterval(fetchData, 30000);
        fetchData();
    } else {
        btn.textContent = "🤖 AKTIFKAN AUTO SIGNAL TELEGRAM";
        btn.style.background = "#00d97e";
        clearInterval(autoInterval);
    }
}

window.onload = () => {
    const el = document.getElementById('chartEl');
    chart = LightweightCharts.createChart(el, { width: el.clientWidth, height: 380 });
    candleSeries = chart.addCandlestickSeries();
    fetchData();
};
</script>
</body>
</html>
