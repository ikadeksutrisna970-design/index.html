<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>XAUUSD M15 • Kadek Bloomberg Terminal</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Share+Tech+Mono&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lightweight-charts@4.1.0/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        :root {
            --bg:#060608; --panel:#0c0c10; --b1:#181820; --orange:#ff6600;
            --green:#00d97e; --red:#ff2d55; --cyan:#00c8ff; --amber:#ffb700;
        }
        * { margin:0; padding:0; box-sizing:border-box; }
        body { background:var(--bg); color:#c0c8d8; font-family:'Inter',sans-serif; min-height:100vh; }
        .wrap { max-width:720px; margin:0 auto; padding:10px 12px; }
        h1 { text-align:center; color:var(--orange); font-family:'Bebas Neue',sans-serif; letter-spacing:3px; margin:15px 0; }
        .price-hero { background:var(--panel); border:1px solid var(--b1); border-left:4px solid var(--orange); padding:15px; margin:10px 0; display:flex; justify-content:space-between; }
        .pbig { font-family:'Bebas Neue',sans-serif; font-size:54px; line-height:1; }
        .pbig.up { color:var(--green); } .pbig.dn { color:var(--red); }
        #chartEl { width:100%; height:340px; background:var(--panel); margin:15px 0; border:1px solid var(--b1); }
        button {
            width:100%; padding:16px; margin:10px 0; font-family:'Bebas Neue',sans-serif;
            font-size:21px; letter-spacing:2px; cursor:pointer; border:none;
        }
        .scan-btn { background:linear-gradient(135deg,#ff6600,#ff8800); color:black; }
        .status { padding:12px; text-align:center; font-family:'Share Tech Mono',monospace; background:rgba(255,183,0,0.15); margin:10px 0; border-radius:4px; }
    </style>
</head>
<body>

<div class="wrap">
    <h1>KADEK BLOOMBERG TERMINAL</h1>
    
    <div class="status" id="status">Menunggu data XAUUSD...</div>

    <div class="price-hero">
        <div>
            <div style="font-size:13px; color:#888;">XAUUSD • M15 • BYBIT</div>
            <div class="pbig" id="price">——</div>
            <div id="change" style="font-size:14px;"></div>
        </div>
    </div>

    <div id="chartEl"></div>

    <button class="scan-btn" onclick="fetchData()">⚡ SCAN & ANALISA SEKARANG</button>
    <button onclick="toggleAuto()" id="autoBtn">🤖 AUTO MODE : OFF</button>
</div>

<script>
let chart, candleSeries;
let autoInterval = null;
let isAuto = false;

function initChart() {
    const el = document.getElementById('chartEl');
    chart = LightweightCharts.createChart(el, {
        width: el.clientWidth,
        height: 340,
        layout: { background: { color: '#0c0c10' }, textColor: '#ccc' },
        grid: { vertLines: { color: '#1f2335' }, horzLines: { color: '#1f2335' } },
    });
    candleSeries = chart.addCandlestickSeries();
}

async function fetchData() {
    const status = document.getElementById('status');
    status.textContent = "🔄 Mengambil data...";

    try {
        const res = await fetch("https://api.bybit.com/v5/market/kline?category=linear&symbol=XAUUSDT&interval=15&limit=200");
        const json = await res.json();
        const raw = json.result.list.reverse();

        const candles = raw.map(d => ({
            time: Math.floor(d[0]/1000),
            open: +d[1], high: +d[2], low: +d[3], close: +d[4]
        }));

        candleSeries.setData(candles);
        chart.timeScale().fitContent();

        const last = candles[candles.length-1];
        document.getElementById('price').textContent = last.close.toFixed(2);
        document.getElementById('price').className = 'pbig ' + (last.close >= candles[candles.length-2].close ? 'up' : 'dn');

        status.textContent = `✅ Berhasil update • ${new Date().toLocaleTimeString('id-ID')}`;
    } catch(e) {
        status.textContent = "❌ Gagal mengambil data";
    }
}

function toggleAuto() {
    isAuto = !isAuto;
    const btn = document.getElementById('autoBtn');
    if (isAuto) {
        btn.textContent = "🤖 AUTO MODE : ON";
        btn.style.background = "#00d97e";
        btn.style.color = "black";
        autoInterval = setInterval(fetchData, 45000);
        fetchData();
    } else {
        btn.textContent = "🤖 AUTO MODE : OFF";
        btn.style.background = "";
        clearInterval(autoInterval);
    }
}

window.onload = () => {
    initChart();
    fetchData();
};
</script>
</body>
</html>
