<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KADEK CAPITAL - BTC Auto Signal</title>
    <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Share+Tech+Mono&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lightweight-charts@4.1.0/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        :root {
            --bg:#060608; --panel:#0c0c10; --orange:#ff6600; --green:#00d97e; --red:#ff2d55;
        }
        body { background:var(--bg); color:#c0c8d8; font-family:'Inter',sans-serif; margin:0; padding:0; }
        .wrap { max-width:720px; margin:0 auto; padding:10px; }
        h1 { text-align:center; color:var(--orange); font-family:'Bebas Neue',sans-serif; font-size:28px; margin:15px 0; }
        #price { font-size:52px; font-family:'Bebas Neue',sans-serif; text-align:center; }
        #chartEl { height:380px; margin:15px 0; background:var(--panel); }
        button { width:100%; padding:16px; margin:10px 0; font-size:18px; font-weight:bold; border:none; cursor:pointer; }
        .status { padding:12px; text-align:center; background:rgba(255,183,0,0.15); margin:10px 0; border-radius:6px; }
    </style>
</head>
<body>

<div class="wrap">
    <h1>KADEK CAPITAL</h1>
    <div style="text-align:center; font-size:18px; color:#ff8800;">BTCUSDT • M15 • AUTO SIGNAL</div>
    
    <div class="status" id="status">Menunggu analisa...</div>
    <div id="price" style="color:#00d97e;">——</div>
    <div id="chartEl"></div>

    <button onclick="toggleAuto()" id="autoBtn" style="background:#00d97e; color:black;">🤖 AUTO SCAN + KIRIM TELEGRAM : OFF</button>
</div>

<script>
let chart, candleSeries;
let autoInterval = null;
let isAuto = false;
let lastSignal = null;

// Telegram Config (isi dengan milikmu)
const BOT_TOKEN = "8596503581:AAHyxjtTkaeutnvy62VlOPTfOr2EmN5tMBg"; // ganti kalau beda
const CHAT_ID = "5132441992"; // ganti dengan chat id kamu

async function fetchData() {
    try {
        const res = await fetch("https://api.bybit.com/v5/market/kline?category=linear&symbol=BTCUSDT&interval=15&limit=100");
        const json = await res.json();
        const raw = json.result.list.reverse();

        const candles = raw.map(d => ({
            time: Math.floor(d[0]/1000),
            open: +d[1], high: +d[2], low: +d[3], close: +d[4]
        }));

        candleSeries.setData(candles);
        const last = candles[candles.length-1];
        document.getElementById('price').textContent = last.close.toFixed(2);

        // Simple Signal Logic
        const prev = candles[candles.length-2];
        if (last.close > prev.close * 1.001) {
            sendSignal("BUY", last.close);
        } else if (last.close < prev.close * 0.999) {
            sendSignal("SELL", last.close);
        }
    } catch(e) {
        console.error(e);
    }
}

async function sendSignal(type, price) {
    if (lastSignal === type) return;
    lastSignal = type;

    const tp = type === "BUY" ? (price * 1.015).toFixed(2) : (price * 0.985).toFixed(2);
    const sl = type === "BUY" ? (price * 0.99).toFixed(2) : (price * 1.01).toFixed(2);

    const message = `${type} BTCUSDT\nPrice: ${price}\nTP: ${tp}\nSL: ${sl}\nTime: ${new Date().toLocaleTimeString('id-ID')}`;

    try {
        await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify({chat_id: CHAT_ID, text: message})
        });
        document.getElementById('status').innerHTML = `✅ ${type} SIGNAL dikirim ke Telegram`;
    } catch(e) {
        console.error("Gagal kirim Telegram");
    }
}

function toggleAuto() {
    isAuto = !isAuto;
    const btn = document.getElementById('autoBtn');
    if (isAuto) {
        btn.textContent = "🤖 AUTO MODE : ON (Scanning...)";
        btn.style.background = "#ff6600";
        autoInterval = setInterval(fetchData, 30000); // setiap 30 detik
        fetchData();
    } else {
        btn.textContent = "🤖 AUTO MODE : OFF";
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
