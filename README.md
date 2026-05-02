<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
<title>Kadek Capital — Crypto Scanner</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Share+Tech+Mono&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
<script src="https://unpkg.com/lightweight-charts@4.1.0/dist/lightweight-charts.standalone.production.js"></script>
<style>
:root{
  --bg:#050507;--panel:#0b0b0f;--card:#101015;--b1:#16161f;--b2:#1e1e2a;--b3:#262636;
  --orange:#f7931a;--amber:#ffb700;--cyan:#00d4ff;--green:#00c896;--red:#f6465d;
  --purple:#8b5cf6;--blue:#3b82f6;--muted:#3a3a55;--dim:#08080b;--text:#b8c0d0;--bright:#e2e6f0;
}
*{margin:0;padding:0;box-sizing:border-box;}
html,body{background:var(--bg);color:var(--text);font-family:'Inter',sans-serif;min-height:100%;}
::-webkit-scrollbar{width:3px}::-webkit-scrollbar-track{background:var(--dim)}::-webkit-scrollbar-thumb{background:var(--b3)}
/* TOPBAR */
#topbar{position:sticky;top:0;z-index:300;background:var(--dim);border-bottom:1px solid var(--b2);display:flex;align-items:center;justify-content:space-between;padding:0 14px;height:46px;}
.brand{display:flex;align-items:center;gap:10px;}
.brand-mark{background:var(--orange);width:32px;height:32px;display:flex;align-items:center;justify-content:center;border-radius:4px;}
.brand-k{font-family:'Bebas Neue',monospace;font-size:20px;color:var(--bg);letter-spacing:2px;}
.brand-text{}
.brand-name{font-family:'Bebas Neue',monospace;font-size:15px;color:var(--bright);letter-spacing:3px;display:block;line-height:1;}
.brand-sub{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);letter-spacing:2px;display:block;}
.topbar-r{display:flex;align-items:center;gap:14px;}
.live-pill{display:flex;align-items:center;gap:5px;background:var(--b1);border:1px solid var(--b2);padding:3px 9px;border-radius:2px;}
.dot{width:6px;height:6px;border-radius:50%;flex-shrink:0;}
.dot.g{background:var(--green);box-shadow:0 0 5px var(--green);animation:blink 1.2s infinite;}
.dot.r{background:var(--red);}
.dot.a{background:var(--amber);animation:blink 1.2s infinite;}
.pill-txt{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:1px;}
#clk{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}

/* WRAP */
.wrap{max-width:700px;margin:0 auto;padding:10px 12px 80px;}

/* PAIR SELECTOR */
.pair-bar{display:flex;gap:8px;margin-bottom:10px;overflow-x:auto;padding-bottom:2px;}
.pair-btn{font-family:'Share Tech Mono',monospace;font-size:10px;letter-spacing:2px;padding:7px 14px;border:1px solid var(--b2);background:var(--panel);color:var(--muted);cursor:pointer;transition:all .2s;white-space:nowrap;flex-shrink:0;}
.pair-btn:hover{border-color:var(--b3);color:var(--text);}
.pair-btn.active{background:rgba(247,147,26,.08);border-color:var(--orange);color:var(--orange);}
.pair-btn.eth-active{background:rgba(139,92,246,.08);border-color:var(--purple);color:var(--purple);}

/* STATUS */
.status{display:none;padding:9px 14px;margin-bottom:8px;font-family:'Share Tech Mono',monospace;font-size:10px;align-items:center;gap:10px;}
.status.show{display:flex;}
.status.f{background:rgba(247,147,26,.07);border:1px solid var(--orange);border-left:3px solid var(--orange);color:var(--orange);}
.status.s{background:rgba(255,183,0,.07);border:1px solid var(--amber);border-left:3px solid var(--amber);color:var(--amber);}
.status.e{background:rgba(246,70,93,.07);border:1px solid var(--red);border-left:3px solid var(--red);color:var(--red);}
.spin{width:13px;height:13px;border:2px solid rgba(255,255,255,.08);border-radius:50%;animation:spin .7s linear infinite;flex-shrink:0;}
@keyframes spin{to{transform:rotate(360deg)}}

/* PRICE HERO */
.price-hero{background:var(--panel);border:1px solid var(--b1);padding:14px 16px;margin-bottom:8px;display:flex;justify-content:space-between;align-items:flex-start;}
.price-hero.btc{border-left:3px solid var(--orange);}
.price-hero.eth{border-left:3px solid var(--purple);}
.plbl{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:3px;color:var(--muted);margin-bottom:3px;}
.pbig{font-family:'Bebas Neue',monospace;font-size:52px;color:var(--bright);letter-spacing:2px;line-height:1;transition:color .3s;}
.pbig.up{color:var(--green)!important;}.pbig.dn{color:var(--red)!important;}
.pchg{font-family:'Share Tech Mono',monospace;font-size:11px;margin-top:2px;}
.phl{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);margin-top:2px;}
.price-r{text-align:right;}
.ses{font-family:'Share Tech Mono',monospace;font-size:9px;padding:4px 10px;border:1px solid;display:inline-block;margin-bottom:5px;}
.ses.prime{border-color:var(--green);color:var(--green);background:rgba(0,200,150,.06);}
.ses.high{border-color:var(--amber);color:var(--amber);background:rgba(255,183,0,.06);}
.ses.low{border-color:var(--muted);color:var(--muted);}
.rinfo{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--muted);}

/* CHART */
.chart-wrap{background:var(--panel);border:1px solid var(--b1);margin-bottom:8px;overflow:hidden;}
.chart-top{background:var(--dim);padding:7px 14px;display:flex;justify-content:space-between;align-items:center;border-bottom:1px solid var(--b2);}
.chart-t{display:flex;align-items:center;gap:8px;}
.cbar{width:3px;height:13px;}
.clbl{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:3px;}
.chart-legs{display:flex;gap:12px;}
.cleg{font-family:'Share Tech Mono',monospace;font-size:8px;}
#chartEl{width:100%;}

/* SECTION */
.sec{display:flex;align-items:center;gap:8px;margin:10px 0 7px;}
.sb{width:3px;height:13px;}
.st{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:3px;}
.sl{flex:1;height:1px;background:linear-gradient(90deg,var(--b2),transparent);}

/* IND */
.ig4{display:grid;grid-template-columns:repeat(4,1fr);gap:5px;margin-bottom:6px;}
.ig3{display:grid;grid-template-columns:repeat(3,1fr);gap:5px;margin-bottom:8px;}
.ibox{background:var(--panel);border:1px solid var(--b1);padding:9px 7px;text-align:center;}
.ikey{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:1px;color:var(--muted);margin-bottom:3px;}
.ival{font-family:'Bebas Neue',monospace;font-size:19px;letter-spacing:1px;}
.ebox{background:var(--panel);border:1px solid var(--b1);padding:9px;position:relative;}
.ekey{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);}
.eval{font-family:'Share Tech Mono',monospace;font-size:11px;font-weight:600;margin-top:2px;}
.earr{position:absolute;right:7px;top:50%;transform:translateY(-50%);font-size:14px;}

/* STRUCT */
.sg{display:grid;grid-template-columns:1fr 1fr;gap:7px;margin-bottom:8px;}
.slbl{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);margin-bottom:5px;}
.stbox{background:var(--panel);border:1px solid var(--b1);padding:12px 10px;text-align:center;transition:.3s;}
.stbox.bull{border-color:var(--green);background:rgba(0,200,150,.04);}
.stbox.bear{border-color:var(--red);background:rgba(246,70,93,.04);}
.stbox.rang{border-color:var(--amber);background:rgba(255,183,0,.03);}
.sticon{font-size:17px;margin-bottom:2px;}
.stname{font-family:'Bebas Neue',monospace;font-size:15px;letter-spacing:3px;}
.stname.bull{color:var(--green);}.stname.bear{color:var(--red);}.stname.rang{color:var(--amber);}
.stsub{font-family:'Share Tech Mono',monospace;font-size:7px;color:var(--muted);margin-top:2px;}

/* FUND */
.fund{background:var(--panel);border:1px solid var(--b1);border-left:3px solid var(--purple);padding:13px;margin-bottom:8px;}
.fgrid{display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-bottom:10px;}
.fitem{background:var(--dim);padding:9px 10px;}
.fkey{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);margin-bottom:4px;}
.fval{font-family:'Share Tech Mono',monospace;font-size:12px;font-weight:600;}
.score-row{display:flex;justify-content:space-between;font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);margin-bottom:4px;}
.score-track{height:5px;background:var(--dim);border:1px solid var(--b2);position:relative;overflow:hidden;}
.score-mid{position:absolute;left:50%;top:0;width:1px;height:100%;background:var(--b3);}
.score-fill{position:absolute;height:100%;transition:all .8s;}
.news-lbl{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);margin:10px 0 6px;}
.nitem{font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--text);padding:5px 0;border-bottom:1px solid var(--b1);line-height:1.5;}
.nitem:last-child{border:none;}
.narr{color:var(--orange);margin-right:4px;}

/* TG */
.tg-panel{background:var(--panel);border:1px solid var(--b1);border-left:2px solid var(--cyan);padding:13px;margin-bottom:8px;}
.tg-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:9px;}
.tg-lbl{font-family:'Share Tech Mono',monospace;font-size:9px;letter-spacing:3px;color:var(--cyan);}
.tg-st{font-family:'Share Tech Mono',monospace;font-size:9px;padding:3px 10px;}
.tg-st.off{background:rgba(246,70,93,.1);color:var(--red);}
.tg-st.on{background:rgba(0,200,150,.1);color:var(--green);}
.tg-st.chk{background:rgba(255,183,0,.1);color:var(--amber);}
.ilbl{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);margin-bottom:3px;}
.inp{background:var(--dim);border:1px solid var(--b2);color:var(--text);font-family:'Share Tech Mono',monospace;font-size:11px;padding:9px 11px;outline:none;width:100%;margin-bottom:7px;transition:border-color .2s;}
.inp:focus{border-color:var(--cyan);}
.inp::placeholder{color:var(--muted);}
.btn-row3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:6px;margin-bottom:7px;}
.btn{background:transparent;border:1px solid;font-family:'Share Tech Mono',monospace;font-size:9px;letter-spacing:1px;padding:9px 5px;cursor:pointer;transition:all .2s;width:100%;}
.btn:disabled{border-color:var(--muted)!important;color:var(--muted)!important;cursor:not-allowed;}
.btn.c{border-color:var(--cyan);color:var(--cyan);}.btn.c:hover:not(:disabled){background:var(--cyan);color:var(--bg);}
.btn.a{border-color:var(--amber);color:var(--amber);}.btn.a:hover:not(:disabled){background:var(--amber);color:var(--bg);}
.btn.m{border-color:var(--muted);color:var(--muted);}.btn.m:hover:not(:disabled){border-color:var(--cyan);color:var(--cyan);}
.alert{padding:9px 11px;font-family:'Share Tech Mono',monospace;font-size:10px;line-height:1.7;white-space:pre-line;display:none;margin-top:4px;}
.alert.ok{background:rgba(0,200,150,.06);border:1px solid rgba(0,200,150,.3);color:var(--green);display:block;}
.alert.w{background:rgba(255,183,0,.06);border:1px solid rgba(255,183,0,.3);color:var(--amber);display:block;}
.alert.e{background:rgba(246,70,93,.06);border:1px solid rgba(246,70,93,.3);color:var(--red);display:block;}

/* AUTO */
.autobar{display:flex;align-items:center;justify-content:space-between;background:var(--panel);border:1px solid var(--b1);padding:8px 14px;margin-bottom:10px;}
.at-t{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--bright);}
.at-s{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);margin-top:2px;}
.tog{position:relative;width:44px;height:24px;cursor:pointer;flex-shrink:0;}
.tog-track{position:absolute;inset:0;border-radius:24px;background:var(--dim);border:1px solid var(--b2);transition:.3s;}
.tog-track.on{background:rgba(0,200,150,.12);border-color:var(--green);}
.tog-thumb{position:absolute;width:18px;height:18px;top:2px;left:2px;border-radius:50%;transition:.3s;}
.tog-thumb.off{background:var(--muted);}
.tog-thumb.on{left:24px;background:var(--green);box-shadow:0 0 8px var(--green);}

/* SCAN BTN */
.scan-btn{width:100%;padding:16px;border:1px solid var(--orange);color:var(--orange);font-family:'Bebas Neue',monospace;font-size:21px;letter-spacing:7px;cursor:pointer;position:relative;overflow:hidden;background:linear-gradient(135deg,rgba(247,147,26,.08),rgba(255,183,0,.06));text-shadow:0 0 10px rgba(247,147,26,.4);box-shadow:0 0 20px rgba(247,147,26,.08);transition:all .3s;margin-bottom:10px;}
.scan-btn:hover:not(:disabled){box-shadow:0 0 30px rgba(247,147,26,.2);}
.scan-btn:disabled{border-color:var(--muted);color:var(--muted);cursor:not-allowed;text-shadow:none;box-shadow:none;background:transparent;}
.scan-btn.eth-mode{border-color:var(--purple);color:var(--purple);background:linear-gradient(135deg,rgba(139,92,246,.08),rgba(139,92,246,.04));text-shadow:0 0 10px rgba(139,92,246,.4);box-shadow:0 0 20px rgba(139,92,246,.08);}
.sweep{position:absolute;inset:0;background:linear-gradient(90deg,transparent,rgba(247,147,26,.06),transparent);transform:translateX(-100%);animation:sweep 1.5s linear infinite;}
@keyframes sweep{to{transform:translateX(100%)}}

/* SIGNAL */
.sigbox{padding:16px;margin-bottom:8px;border-width:1px 1px 1px 4px;border-style:solid;text-align:center;transition:all .4s;}
.sigbox.BUY{border-color:var(--green);background:rgba(0,200,150,.04);}
.sigbox.SELL{border-color:var(--red);background:rgba(246,70,93,.04);}
.sigbox.WAIT{border-color:var(--amber);background:rgba(255,183,0,.03);}
.sp{font-family:'Share Tech Mono',monospace;font-size:10px;letter-spacing:5px;color:var(--muted);margin-bottom:3px;}
.sd{font-family:'Bebas Neue',monospace;font-size:58px;letter-spacing:7px;line-height:1;}
.BUY .sd{color:var(--green);text-shadow:0 0 30px rgba(0,200,150,.5);}
.SELL .sd{color:var(--red);text-shadow:0 0 30px rgba(246,70,93,.5);}
.WAIT .sd{color:var(--amber);}
.st-time{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);margin-top:3px;}
.pgrid{display:grid;grid-template-columns:repeat(3,1fr);gap:5px;margin-top:13px;}
.pb{background:var(--dim);border:1px solid var(--b1);padding:10px 7px;}
.pk{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);margin-bottom:3px;}
.pv{font-family:'Bebas Neue',monospace;font-size:20px;letter-spacing:1px;}
.BUY .pb:nth-child(1) .pv{color:var(--amber);}
.BUY .pb:nth-child(2) .pv{color:var(--green);}
.BUY .pb:nth-child(3) .pv{color:var(--red);}
.SELL .pb:nth-child(1) .pv{color:var(--amber);}
.SELL .pb:nth-child(2) .pv{color:var(--green);}
.SELL .pb:nth-child(3) .pv{color:var(--red);}
.WAIT .pb .pv{color:var(--muted);}
.rr{display:inline-block;font-family:'Share Tech Mono',monospace;font-size:10px;padding:3px 13px;margin-top:10px;}
.BUY .rr{background:rgba(0,200,150,.1);color:var(--green);border:1px solid rgba(0,200,150,.3);}
.SELL .rr{background:rgba(246,70,93,.1);color:var(--red);border:1px solid rgba(246,70,93,.3);}
.WAIT .rr{display:none!important;}

/* ANLZ */
.anlz{background:var(--panel);border:1px solid var(--b1);padding:13px;margin-bottom:8px;}
.at2{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:3px;color:var(--muted);margin-bottom:8px;}
.atext{font-size:12px;line-height:1.8;color:var(--text);}
.ctags{display:flex;flex-wrap:wrap;gap:4px;margin-top:9px;}
.ctag{font-family:'Share Tech Mono',monospace;font-size:8px;padding:2px 9px;color:var(--orange);background:rgba(247,147,26,.06);border:1px solid rgba(247,147,26,.3);}
.cbar-wrap{margin-top:11px;}
.cbar-row{display:flex;justify-content:space-between;font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);margin-bottom:4px;}
.cbar-bg{height:4px;background:var(--dim);border:1px solid var(--b1);overflow:hidden;}
.cbar-fill{height:100%;transition:width 1s ease;}

/* SEND */
.send-row{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:10px;}
.sbtn{padding:13px;font-family:'Bebas Neue',monospace;font-size:15px;letter-spacing:4px;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:7px;transition:all .3s;border:1px solid;}
.sbtn.tg{background:rgba(0,212,255,.06);border-color:var(--cyan);color:var(--cyan);}
.sbtn.tg:hover:not(:disabled){background:var(--cyan);color:var(--bg);}
.sbtn.cp{background:rgba(255,183,0,.06);border-color:var(--amber);color:var(--amber);}
.sbtn.cp:hover:not(:disabled){background:var(--amber);color:var(--bg);}
.sbtn:disabled{border-color:var(--muted)!important;color:var(--muted)!important;cursor:not-allowed;background:transparent!important;}

/* LOG */
.log{background:var(--panel);border:1px solid var(--b1);padding:10px;max-height:110px;overflow-y:auto;}
.ltxt{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:2px;color:var(--muted);margin-bottom:5px;}
.le{font-family:'Share Tech Mono',monospace;font-size:9px;line-height:1.9;display:flex;gap:6px;}
.lt{color:var(--muted);}.lok{color:var(--green);}.lerr{color:var(--red);}.linf{color:var(--cyan);}.lw{color:var(--amber);}

/* COPY MODAL */
#copyModal{display:none;position:fixed;inset:0;background:rgba(0,0,0,.92);z-index:1000;align-items:center;justify-content:center;padding:14px;}
#copyModal.show{display:flex;}
.mbox{background:var(--panel);border:1px solid var(--b2);padding:16px;max-width:480px;width:100%;}
.mtitle{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--orange);letter-spacing:3px;margin-bottom:8px;}
.msteps{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--muted);margin-bottom:10px;line-height:1.8;}
.mta{width:100%;height:170px;background:var(--dim);border:1px solid var(--b2);color:var(--text);font-family:'Share Tech Mono',monospace;font-size:10px;padding:10px;resize:none;outline:none;line-height:1.6;margin-bottom:10px;}
.mbtns{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:8px;}
</style>
</head>
<body>

<!-- TOPBAR -->
<div id="topbar">
  <div class="brand">
    <div class="brand-mark"><span class="brand-k">KC</span></div>
    <div class="brand-text">
      <span class="brand-name">KADEK CAPITAL</span>
      <span class="brand-sub">CRYPTO SCANNER M15</span>
    </div>
  </div>
  <div class="topbar-r">
    <div class="live-pill">
      <div class="dot g" id="bdot"></div>
      <span class="pill-txt" id="blbl" style="color:var(--green)">BINANCE</span>
    </div>
    <div class="live-pill">
      <div class="dot r" id="tdot"></div>
      <span class="pill-txt" id="tlbl" style="color:var(--red)">TG</span>
    </div>
    <span id="clk">--:--:--</span>
  </div>
</div>

<div class="wrap">
  <div class="status" id="sb"><div class="spin" id="sp"></div><span id="stxt"></span></div>

  <!-- PAIR SELECTOR -->
  <div class="pair-bar">
    <button class="pair-btn active" id="pb-btc" onclick="switchPair('BTCUSDT')">₿ BITCOIN</button>
    <button class="pair-btn" id="pb-eth" onclick="switchPair('ETHUSDT')">Ξ ETHEREUM</button>
  </div>

  <!-- PRICE HERO -->
  <div class="price-hero btc" id="priceHero">
    <div>
      <div class="plbl" id="pairLabel">BTCUSDT · BINANCE · M15</div>
      <div class="pbig" id="pm">——</div>
      <div class="pchg" id="pc"></div>
      <div class="phl" id="phl"></div>
    </div>
    <div class="price-r">
      <div class="ses low" id="sesbadge">⏳ LOADING</div>
      <div class="rinfo">Refresh: <b id="cdn" style="color:var(--cyan)">60</b>s</div>
      <div class="rinfo" id="lupd" style="margin-top:2px;"></div>
    </div>
  </div>

  <!-- CHART -->
  <div class="chart-wrap">
    <div class="chart-top">
      <div class="chart-t">
        <div class="cbar" id="chartBar" style="background:var(--orange)"></div>
        <span class="clbl" id="chartLbl" style="color:var(--orange)">CANDLESTICK M15 · BINANCE</span>
      </div>
      <div class="chart-legs">
        <span class="cleg" style="color:var(--orange)">── EMA20</span>
        <span class="cleg" style="color:var(--cyan)">── EMA50</span>
        <span class="cleg" style="color:var(--purple)">── EMA200</span>
      </div>
    </div>
    <div id="chartEl"></div>
  </div>

  <!-- INDICATORS -->
  <div class="sec"><div class="sb" style="background:var(--orange)"></div><span class="st" style="color:var(--orange)" id="indTitle">INDIKATOR TEKNIKAL · BTC M15</span><div class="sl"></div></div>
  <div class="ig4">
    <div class="ibox"><div class="ikey">RSI (14)</div><div class="ival" id="vRSI" style="color:var(--amber)">—</div></div>
    <div class="ibox"><div class="ikey">ATR (14)</div><div class="ival" id="vATR" style="color:var(--orange)">—</div></div>
    <div class="ibox"><div class="ikey">VOLUME %</div><div class="ival" id="vVol" style="color:var(--muted)">—</div></div>
    <div class="ibox"><div class="ikey">EMA TREND</div><div class="ival" id="vTrend" style="color:var(--muted);font-size:11px">—</div></div>
  </div>
  <div class="ig3">
    <div class="ebox"><div class="ekey">EMA 20</div><div class="eval" id="e20" style="color:var(--orange)">—</div><div class="earr" id="a20"></div></div>
    <div class="ebox"><div class="ekey">EMA 50</div><div class="eval" id="e50" style="color:var(--cyan)">—</div><div class="earr" id="a50"></div></div>
    <div class="ebox"><div class="ekey">EMA 200</div><div class="eval" id="e200" style="color:var(--purple)">—</div><div class="earr" id="a200"></div></div>
  </div>

  <!-- STRUCTURE -->
  <div class="sec"><div class="sb" style="background:var(--cyan)"></div><span class="st" style="color:var(--cyan)">MARKET STRUCTURE · AUTO DETECT</span><div class="sl"></div></div>
  <div class="sg">
    <div><div class="slbl">M15 STRUCTURE</div><div class="stbox rang" id="m15s"><div class="sticon">⏳</div><div class="stname rang">LOADING</div><div class="stsub">fetching...</div></div></div>
    <div><div class="slbl">H4 BIAS (HTF)</div><div class="stbox rang" id="h4s"><div class="sticon">⏳</div><div class="stname rang">LOADING</div><div class="stsub">fetching...</div></div></div>
  </div>

  <!-- FUNDAMENTAL -->
  <div class="sec"><div class="sb" style="background:var(--purple)"></div><span class="st" style="color:var(--purple)">💎 FUNDAMENTAL & SMART MONEY</span><div class="sl"></div></div>
  <div class="fund">
    <div class="fgrid">
      <div class="fitem"><div class="fkey">DOMINASI BTC</div><div class="fval" id="fDom" style="color:var(--muted)">—</div></div>
      <div class="fitem"><div class="fkey">FUND SCORE</div><div class="fval" id="fScore" style="color:var(--muted)">—</div></div>
      <div class="fitem"><div class="fkey">MARKET SENTIMENT</div><div class="fval" id="fSent" style="color:var(--muted)">—</div></div>
      <div class="fitem"><div class="fkey">WHALE SIGNAL</div><div class="fval" id="fWhale" style="color:var(--muted)">—</div></div>
    </div>
    <div class="score-row"><span>◀️ BEARISH</span><span id="scoreLbl" style="color:var(--amber)">SCORE —/100</span><span>BULLISH ▶️</span></div>
    <div class="score-track"><div class="score-mid"></div><div class="score-fill" id="scoreFill" style="width:0;left:50%;height:100%;border-radius:1px"></div></div>
    <div class="news-lbl">📊 MARKET INTELLIGENCE</div>
    <div id="newsBox"><div class="nitem"><span class="narr">›</span>Menunggu data...</div></div>
  </div>

  <!-- TELEGRAM -->
  <div class="sec"><div class="sb" style="background:var(--cyan)"></div><span class="st" style="color:var(--cyan)">📡 TELEGRAM SIGNAL BOT</span><div class="sl"></div></div>
  <div class="tg-panel">
    <div class="tg-hdr"><span class="tg-lbl">@KADEKPIRANG_BOT</span><span class="tg-st off" id="tgSt">● OFFLINE</span></div>
    <div class="ilbl">BOT TOKEN</div>
    <input class="inp" id="tok" type="password" value="8596503581:AAHyxjtTkaeutnvy62VlOPTfOr2EmN5tMBg" placeholder="Bot Token...">
    <div class="ilbl">CHAT ID</div>
    <input class="inp" id="cid" type="text" value="5132441992" placeholder="Chat ID...">
    <div class="btn-row3">
      <button class="btn c" id="btnTest" onclick="testTG()">TEST ▶️</button>
      <button class="btn a" onclick="detectCID()">🔍 DETECT</button>
      <button class="btn m" onclick="window.open('https://t.me/Kadekpirang_bot','_blank')">📱 BUKA</button>
    </div>
    <div class="alert" id="tgAlert"></div>
  </div>

  <!-- AUTO -->
  <div class="autobar">
    <div><div class="at-t">🤖 AUTO SCAN + KIRIM TELEGRAM</div><div class="at-s" id="autoTxt">○ OFF — tap toggle untuk aktifkan</div></div>
    <div class="tog" onclick="toggleAuto()"><div class="tog-track" id="togTrack"></div><div class="tog-thumb off" id="togThumb"></div></div>
  </div>

  <!-- SCAN -->
  <button class="scan-btn" id="btnScan" onclick="runScan()" disabled>
    <div class="sweep"></div>
    <span id="scanBtnTxt">⚡ SCAN &amp; ANALISA</span>
  </button>

  <!-- SIGNAL -->
  <div id="sigSection" style="display:none">
    <div class="sec"><div class="sb" id="sigAccent" style="background:var(--amber)"></div><span class="st" id="sigSecTxt" style="color:var(--amber)">SIGNAL OUTPUT</span><div class="sl"></div></div>
    <div class="sigbox WAIT" id="sigBox">
      <div class="sp" id="sigPair">— · M15 · BINANCE</div>
      <div class="sd" id="sigDir">—</div>
      <div class="st-time" id="sigTime"></div>
      <div class="pgrid" id="pgrid">
        <div class="pb"><div class="pk">ENTRY</div><div class="pv" id="pgE">—</div></div>
        <div class="pb"><div class="pk">TAKE PROFIT</div><div class="pv" id="pgTP">—</div></div>
        <div class="pb"><div class="pk">STOP LOSS</div><div class="pv" id="pgSL">—</div></div>
      </div>
      <div class="rr" id="sigRR">RR 1:—</div>
    </div>
    <div class="anlz">
      <div class="at2">ANALISA · TEKNIKAL + FUNDAMENTAL + SMART MONEY</div>
      <div class="atext" id="anlzTxt">—</div>
      <div class="ctags" id="ctags"></div>
      <div class="cbar-wrap">
        <div class="cbar-row"><span>CONFIDENCE SCORE</span><span id="cpct">—</span></div>
        <div class="cbar-bg"><div class="cbar-fill" id="cfill" style="width:0"></div></div>
      </div>
    </div>
    <div class="send-row" id="sendRow" style="display:none">
      <button class="sbtn tg" id="btnSend" onclick="sendTG()">📤 KIRIM TELEGRAM</button>
      <button class="sbtn cp" onclick="openCopy()">📋 COPY SINYAL</button>
    </div>
  </div>

  <button class="btn m" id="btnFetch" onclick="fetchData()" style="width:100%;padding:11px;margin-bottom:8px;font-size:10px">⟳ FETCH DATA ULANG</button>
  <div class="log"><div class="ltxt">SYSTEM LOG</div><div id="logEl"></div></div>
</div>

<!-- COPY MODAL -->
<div id="copyModal">
  <div class="mbox">
    <div class="mtitle">📋 COPY SINYAL → PASTE KE TELEGRAM</div>
    <div class="msteps">1. Tekan COPY → 2. Tekan BUKA TELEGRAM → 3. Paste & Kirim</div>
    <textarea class="mta" id="copyArea" readonly></textarea>
    <div class="mbtns">
      <button class="btn c" id="btnCopy" onclick="doCopy()" style="padding:11px">📋 COPY</button>
      <button class="btn a" onclick="window.open('https://t.me/Kadekpirang_bot','_blank')" style="padding:11px">📱 BUKA TELEGRAM</button>
    </div>
    <button class="btn m" onclick="closeCopy()" style="width:100%;padding:9px">TUTUP ✕</button>
  </div>
</div>

<script>
// ══════════════════════════════════════════
//  STATE
// ══════════════════════════════════════════
let chart, cSeries, e20s, e50s, e200s, vols;
let IND = {}, lastSig = null, tgOK = false, autoOn = false;
let cdTimer = null, cdV = 60;
let currentPair = 'BTCUSDT';
const pad = x => String(x).padStart(2,'0');

// ══════════════════════════════════════════
//  CLOCK + SESSION
// ══════════════════════════════════════════
const DAYS=['Min','Sen','Sel','Rab','Kam','Jum','Sab'];
const MONS=['Jan','Feb','Mar','Apr','Mei','Jun','Jul','Agu','Sep','Okt','Nov','Des'];
setInterval(() => {
  const n = new Date();
  document.getElementById('clk').textContent =
    `${DAYS[n.getDay()]} ${n.getDate()} ${MONS[n.getMonth()]} · ${pad(n.getHours())}:${pad(n.getMinutes())}:${pad(n.getSeconds())} WIB`;
  const s = getSession();
  const el = document.getElementById('sesbadge');
  el.className = 'ses ' + s.cls;
  el.textContent = '▶️ ' + s.name;
}, 1000);

function getSession() {
  const n = new Date(), m = n.getHours()*60 + n.getMinutes();
  if(m>=19*60&&m<21*60)    return{name:'LONDON-NY OVERLAP 🔥',cls:'prime',good:true};
  if(m>=14*60&&m<17*60)    return{name:'LONDON PRIME ⭐',cls:'prime',good:true};
  if(m>=19*60+30&&m<22*60) return{name:'NEW YORK PRIME ⭐',cls:'prime',good:true};
  if(m>=17*60&&m<19*60+30) return{name:'PRE NEW YORK',cls:'high',good:true};
  return{name:'ASIA / OFF SESSION',cls:'low',good:false};
}

// ══════════════════════════════════════════
//  LOG
// ══════════════════════════════════════════
function log(t, m) {
  const n = new Date(), ts = `${pad(n.getHours())}:${pad(n.getMinutes())}:${pad(n.getSeconds())}`;
  const el = document.getElementById('logEl');
  const r = document.createElement('div'); r.className = 'le';
  const c = {ok:'lok',err:'lerr',info:'linf',warn:'lw'}[t]||'lt';
  r.innerHTML = `<span class="lt">[${ts}]</span><span class="${c}">${m}</span>`;
  el.appendChild(r); el.scrollTop = el.scrollHeight;
  while(el.children.length > 30) el.removeChild(el.firstChild);
}

function setStatus(t, m) {
  const s = document.getElementById('sb');
  s.className = 'status show ' + t;
  document.getElementById('stxt').textContent = m;
  document.getElementById('sp').style.borderTopColor = t==='f'?'var(--orange)':t==='s'?'var(--amber)':'var(--red)';
}
function clrStatus() { document.getElementById('sb').className = 'status'; }

// ══════════════════════════════════════════
//  PAIR SWITCHER
// ══════════════════════════════════════════
function switchPair(pair) {
  currentPair = pair;
  const isBTC = pair === 'BTCUSDT';
  const col = isBTC ? 'var(--orange)' : 'var(--purple)';
  const name = isBTC ? 'BTC' : 'ETH';

  document.getElementById('pb-btc').className = 'pair-btn' + (isBTC ? ' active' : '');
  document.getElementById('pb-eth').className = 'pair-btn' + (!isBTC ? ' eth-active' : '');
  document.getElementById('priceHero').className = 'price-hero ' + (isBTC ? 'btc' : 'eth');
  document.getElementById('pairLabel').textContent = `${pair} · BINANCE · M15`;
  document.getElementById('chartBar').style.background = col;
  document.getElementById('chartLbl').style.color = col;
  document.getElementById('chartLbl').textContent = `CANDLESTICK M15 · BINANCE · ${name}`;
  document.getElementById('indTitle').textContent = `INDIKATOR TEKNIKAL · ${name} M15`;
  document.getElementById('sigPair').textContent = `${pair} · M15 · BINANCE`;

  const sb = document.getElementById('btnScan');
  sb.className = 'scan-btn' + (!isBTC ? ' eth-mode' : '');

  fetchData();
}

// ══════════════════════════════════════════
//  CHART
// ══════════════════════════════════════════
function initChart() {
  const el = document.getElementById('chartEl');
  el.style.height = '270px';
  chart = LightweightCharts.createChart(el, {
    width: el.clientWidth, height: 270,
    layout: { background:{color:'#08080b'}, textColor:'#505870' },
    grid: { vertLines:{color:'#14141e'}, horzLines:{color:'#14141e'} },
    rightPriceScale: { borderColor:'#14141e', scaleMargins:{top:0.06,bottom:0.25} },
    timeScale: { borderColor:'#14141e', timeVisible:true, secondsVisible:false },
    crosshair: { mode:LightweightCharts.CrosshairMode.Normal,
      vertLine:{color:'rgba(247,147,26,.35)'}, horzLine:{color:'rgba(247,147,26,.35)'} },
  });
  cSeries = chart.addCandlestickSeries({
    upColor:'#00c896', downColor:'#f6465d',
    borderVisible:false, wickUpColor:'#00c896', wickDownColor:'#f6465d'
  });
  e20s  = chart.addLineSeries({color:'#f7931a',lineWidth:1.5,priceLineVisible:false,lastValueVisible:false});
  e50s  = chart.addLineSeries({color:'#00d4ff',lineWidth:1.2,priceLineVisible:false,lastValueVisible:false});
  e200s = chart.addLineSeries({color:'#8b5cf6',lineWidth:1,lineStyle:1,priceLineVisible:false,lastValueVisible:false});
  vols  = chart.addHistogramSeries({
    color:'#26a69a', priceFormat:{type:'volume'}, priceScaleId:'v', scaleMargins:{top:0.82,bottom:0}
  });
  chart.priceScale('v').applyOptions({scaleMargins:{top:0.82,bottom:0}});
  window.addEventListener('resize', () => chart && chart.applyOptions({width:el.clientWidth}));
}

function renderChart(candles) {
  if(!chart || !candles?.length) return;
  const toT = ts => Math.floor(ts/1000);
  cSeries.setData(candles.map(c => ({time:toT(c.t),open:c.o,high:c.h,low:c.l,close:c.c})));
  const cls = candles.map(c => c.c);
  const ema = (c, p) => {
    if(c.length<p) return [];
    const k=2/(p+1); let v=c.slice(0,p).reduce((a,b)=>a+b)/p;
    const r=Array(p-1).fill(null); r.push(v);
    for(let i=p;i<c.length;i++){v=c[i]*k+v*(1-k);r.push(v);}
    return r;
  };
  const mapE = arr => arr.map((v,i) => v===null?null:{time:toT(candles[i].t),value:v}).filter(Boolean);
  e20s.setData(mapE(ema(cls,20)));
  e50s.setData(mapE(ema(cls,50)));
  e200s.setData(mapE(ema(cls,200)));
  vols.setData(candles.map(c => ({
    time:toT(c.t), value:c.v,
    color: c.c>=c.o ? 'rgba(0,200,150,.4)' : 'rgba(246,70,93,.4)'
  })));
  chart.timeScale().fitContent();
  setBinanceOK(true);
}

function setBinanceOK(ok) {
  document.getElementById('bdot').className = 'dot ' + (ok?'g':'r');
  document.getElementById('blbl').style.color = ok ? 'var(--green)' : 'var(--red)';
}

// ══════════════════════════════════════════
//  MATH
// ══════════════════════════════════════════
function calcEMA(c,p) {
  if(c.length<p) return [];
  const k=2/(p+1); let v=c.slice(0,p).reduce((a,b)=>a+b)/p;
  const r=Array(p-1).fill(null); r.push(v);
  for(let i=p;i<c.length;i++){v=c[i]*k+v*(1-k);r.push(v);}
  return r;
}
function calcRSI(c,p=14) {
  if(c.length<p+1) return null;
  let g=0,l=0;
  for(let i=1;i<=p;i++){const d=c[i]-c[i-1];d>0?g+=d:l-=d;}
  let ag=g/p,al=l/p;
  for(let i=p+1;i<c.length;i++){const d=c[i]-c[i-1];ag=(ag*(p-1)+Math.max(d,0))/p;al=(al*(p-1)+Math.max(-d,0))/p;}
  return al===0?100:100-100/(1+ag/al);
}
function calcATR(h,l,c,p=14) {
  if(h.length<p+1) return null;
  const tr=[];
  for(let i=1;i<c.length;i++) tr.push(Math.max(h[i]-l[i],Math.abs(h[i]-c[i-1]),Math.abs(l[i]-c[i-1])));
  let a=tr.slice(0,p).reduce((x,y)=>x+y)/p;
  for(let i=p;i<tr.length;i++) a=(a*(p-1)+tr[i])/p;
  return a;
}
function detectStruct(h,l,lb=5) {
  const sH=[],sL=[];
  for(let i=lb;i<h.length-lb;i++){
    let iH=true,iL=true;
    for(let j=i-lb;j<=i+lb;j++){if(j===i)continue;if(h[j]>=h[i])iH=false;if(l[j]<=l[i])iL=false;}
    if(iH)sH.push(h[i]);if(iL)sL.push(l[i]);
  }
  if(sH.length<2||sL.length<2) return 'RANGING';
  const HH=sH[sH.length-1]>sH[sH.length-2], HL=sL[sL.length-1]>sL[sL.length-2];
  const LH=sH[sH.length-1]<sH[sH.length-2], LL=sL[sL.length-1]<sL[sL.length-2];
  if(HH&&HL) return 'BULLISH'; if(LH&&LL) return 'BEARISH'; return 'RANGING';
}

function computeInd(m15, h4) {
  const cls=m15.map(c=>c.c), his=m15.map(c=>c.h), los=m15.map(c=>c.l), vol=m15.map(c=>c.v);
  const price=cls[cls.length-1], prev=cls[cls.length-2];
  const e20a=calcEMA(cls,20), e50a=calcEMA(cls,50), e200a=calcEMA(cls,200);
  const e20=e20a[e20a.length-1], e50=e50a[e50a.length-1], e200=e200a[e200a.length-1];
  const rsi=calcRSI(cls), atr=calcATR(his,los,cls);
  const lv=vol[vol.length-1], av=vol.slice(-20).reduce((a,b)=>a+b)/20;
  const volPct=Math.round(lv/av*100);
  const lb=Math.abs(m15[m15.length-1].c-m15[m15.length-1].o);
  const ab=m15.slice(-20).reduce((a,c)=>a+Math.abs(c.c-c.o),0)/20;
  const bigCandle=lb>ab*1.8, whale=volPct>160||(volPct>130&&bigCandle);
  const m15s=detectStruct(his,los,5);
  const h4s=h4?.length?detectStruct(h4.map(c=>c.h),h4.map(c=>c.l),3):'RANGING';
  let trend='MIXED';
  if(price>e20&&price>e50&&price>e200&&e20>e50&&e50>e200) trend='FULL BULL ▲';
  else if(price<e20&&price<e50&&price<e200&&e20<e50&&e50<e200) trend='FULL BEAR ▼';
  else if(price>e50&&price>e200) trend='DI ATAS EMA';
  else if(price<e50&&price<e200) trend='DI BAWAH EMA';
  let fs=50;
  if(h4s==='BULLISH')fs+=15; else if(h4s==='BEARISH')fs-=15;
  if(m15s==='BULLISH')fs+=10; else if(m15s==='BEARISH')fs-=10;
  if(price>e200)fs+=10; else fs-=10;
  if(price>e50)fs+=5; else fs-=5;
  if(rsi>50&&rsi<70)fs+=5; else if(rsi<50&&rsi>30)fs-=5;
  if(volPct>120)fs+=(h4s==='BULLISH'?8:-8);
  if(whale)fs+=(h4s==='BULLISH'?7:-7);
  fs=Math.max(5,Math.min(95,fs));
  const chgPct=((price-prev)/prev*100);
  const domBias=currentPair==='BTCUSDT'?(chgPct>0.3?'DOMINASI ↑':chgPct<-0.3?'DOMINASI ↓':'STABIL'):(chgPct>0.3?'ETH KUAT ↑':chgPct<-0.3?'ETH LEMAH ↓':'KONSOLIDASI');
  return {price,prev,e20,e50,e200,rsi,atr,volPct,bigCandle,whale,m15s,h4s,trend,fs,domBias,
    h24:Math.max(...his.slice(-96)), l24:Math.min(...los.slice(-96))};
}

// ══════════════════════════════════════════
//  RENDER INDICATORS
// ══════════════════════════════════════════
function renderInd(d) {
  const {price,prev,e20,e50,e200,rsi,atr,volPct,whale,bigCandle,m15s,h4s,trend,fs,domBias,h24,l24}=d;
  const isBTC = currentPair==='BTCUSDT';
  const chg=price-prev, pct=(chg/prev*100).toFixed(2);
  const pm=document.getElementById('pm');
  pm.textContent = price > 999 ? price.toLocaleString('en',{minimumFractionDigits:2,maximumFractionDigits:2}) : price.toFixed(4);
  pm.className='pbig '+(chg>=0?'up':'dn');
  document.getElementById('pc').innerHTML=`<span style="color:${chg>=0?'var(--green)':'var(--red)'}">${chg>=0?'+':''}${chg.toFixed(2)} (${chg>=0?'+':''}${pct}%)</span>`;
  document.getElementById('phl').textContent=`H:${h24.toFixed(2)}  L:${l24.toFixed(2)}`;
  document.getElementById('lupd').textContent='Updated '+new Date().toLocaleTimeString('id-ID');

  const rv=document.getElementById('vRSI'); rv.textContent=rsi?rsi.toFixed(1):'—';
  rv.style.color=rsi>70?'var(--red)':rsi<30?'var(--green)':'var(--amber)';
  document.getElementById('vATR').textContent=atr?atr.toFixed(2):'—';
  const vv=document.getElementById('vVol'); vv.textContent=volPct+'%';
  vv.style.color=volPct>150?'var(--cyan)':volPct>110?'var(--green)':volPct<80?'var(--red)':'var(--amber)';
  const tv=document.getElementById('vTrend'); tv.textContent=trend;
  tv.style.color=trend.includes('BULL')?'var(--green)':trend.includes('BEAR')?'var(--red)':'var(--amber)';

  function setE(id,aid,v){
    const el=document.getElementById(id),ar=document.getElementById(aid);
    el.textContent=v?v.toFixed(2):'—';
    el.style.color=v?(price>v?'var(--green)':'var(--red)'):'var(--muted)';
    if(ar){ar.textContent=v?(price>v?'↑':'↓'):'';ar.style.color=el.style.color;}
  }
  setE('e20','a20',e20); setE('e50','a50',e50); setE('e200','a200',e200);

  function setStr(id,s){
    const el=document.getElementById(id);
    const mp={BULLISH:{c:'bull',i:'📈',n:'BULLISH',s:'HH → HL aktif'},BEARISH:{c:'bear',i:'📉',n:'BEARISH',s:'LH → LL aktif'},RANGING:{c:'rang',i:'↔️',n:'RANGING',s:'Tidak ada trend'}};
    const d=mp[s]||mp.RANGING;
    el.className='stbox '+d.c;
    el.innerHTML=`<div class="sticon">${d.i}</div><div class="stname ${d.c}">${d.n}</div><div class="stsub">${d.s}</div>`;
  }
  setStr('m15s',m15s); setStr('h4s',h4s);

  const de=document.getElementById('fDom'); de.textContent=domBias;
  de.style.color=domBias.includes('↑')?'var(--green)':domBias.includes('↓')?'var(--red)':'var(--amber)';
  const sc=document.getElementById('fScore'); sc.textContent=fs+'/100';
  sc.style.color=fs>60?'var(--green)':fs<40?'var(--red)':'var(--amber)';
  const sent=h4s==='BULLISH'?'BULLISH 🟢':h4s==='BEARISH'?'BEARISH 🔴':'NEUTRAL ↔️';
  const se=document.getElementById('fSent'); se.textContent=sent;
  se.style.color=h4s==='BULLISH'?'var(--green)':h4s==='BEARISH'?'var(--red)':'var(--amber)';
  const we=document.getElementById('fWhale'); we.textContent=whale?'🐋 ACTIVE':'QUIET';
  we.style.color=whale?'var(--cyan)':'var(--muted)';

  document.getElementById('scoreLbl').textContent=`SCORE ${fs}/100`;
  document.getElementById('scoreLbl').style.color=fs>60?'var(--green)':fs<40?'var(--red)':'var(--amber)';
  const sf=document.getElementById('scoreFill'), diff=fs-50;
  if(diff>=0){sf.style.left='50%';sf.style.width=diff+'%';sf.style.background='linear-gradient(90deg,var(--green),var(--cyan))';}
  else{sf.style.left=fs+'%';sf.style.width=(-diff)+'%';sf.style.background='linear-gradient(90deg,var(--red),#ff6b00)';}

  const news=[];
  if(h4s==='BULLISH')news.push(`HTF H4 BULLISH — struktur HH/HL aktif, bias ${isBTC?'beli BTC':'beli ETH'} terkonfirmasi`);
  else if(h4s==='BEARISH')news.push(`HTF H4 BEARISH — struktur LH/LL aktif, bias jual dominan`);
  if(m15s===h4s&&m15s!=='RANGING')news.push(`Multi-timeframe alignment ${m15s} — konfirmasi kuat searah HTF`);
  if(rsi>70)news.push(`RSI ${rsi?.toFixed(1)} zona overbought — waspadai potensi koreksi`);
  else if(rsi<30)news.push(`RSI ${rsi?.toFixed(1)} zona oversold — potensi technical bounce`);
  if(whale)news.push(`Volume ${volPct}% + Big candle — Smart money/Whale activity terdeteksi`);
  if(!news.length)news.push(`Market dalam kondisi normal — ${m15s} di M15, Score ${fs}/100`);
  document.getElementById('newsBox').innerHTML=news.map(n=>`<div class="nitem"><span class="narr">›</span>${n}</div>`).join('');
}

// ══════════════════════════════════════════
//  FETCH BINANCE (no CORS issues!)
// ══════════════════════════════════════════
async function fetchBinance(symbol, interval, limit) {
  const url = `https://api.binance.com/api/v3/klines?symbol=${symbol}&interval=${interval}&limit=${limit}`;
  const r = await fetch(url, {signal: AbortSignal.timeout(10000)});
  if(!r.ok) throw new Error('Binance HTTP ' + r.status);
  const d = await r.json();
  return d.map(c => ({
    t: c[0], o: +c[1], h: +c[2], l: +c[3], c: +c[4], v: +c[5]
  }));
}

// ══════════════════════════════════════════
//  COUNTDOWN
// ══════════════════════════════════════════
function startCD() {
  clearInterval(cdTimer); cdV=60;
  document.getElementById('cdn').textContent=cdV;
  cdTimer=setInterval(()=>{
    cdV--; document.getElementById('cdn').textContent=cdV;
    if(cdV<=0){clearInterval(cdTimer);fetchData();}
  },1000);
}

// ══════════════════════════════════════════
//  MAIN FETCH
// ══════════════════════════════════════════
async function fetchData() {
  setStatus('f', `FETCHING ${currentPair} FROM BINANCE API...`);
  log('info', `🔄 Mengambil data ${currentPair} dari Binance...`);
  document.getElementById('btnScan').disabled = true;
  document.getElementById('btnFetch').disabled = true;
  try {
    const [m15, h4] = await Promise.all([
      fetchBinance(currentPair,'15m',200),
      fetchBinance(currentPair,'4h',100)
    ]);
    renderChart(m15);
    IND = computeInd(m15, h4);
    renderInd(IND);
    clrStatus();
    document.getElementById('btnScan').disabled = false;
    log('ok', `💰 ${currentPair}: ${IND.price.toFixed(2)} | RSI:${IND.rsi?.toFixed(1)} | ATR:${IND.atr?.toFixed(2)} | Vol:${IND.volPct}%`);
    log('ok', `📊 M15:${IND.m15s} | H4:${IND.h4s} | ${IND.trend} | Score:${IND.fs}`);
    startCD();
    if(autoOn && tgOK) setTimeout(() => runScan(true), 600);
  } catch(e) {
    setStatus('e', 'FETCH FAILED: ' + e.message);
    log('err', '❌ ' + e.message);
    setBinanceOK(false);
    document.getElementById('btnScan').disabled = false;
  }
  document.getElementById('btnFetch').disabled = false;
}

// ══════════════════════════════════════════
//  SIGNAL GENERATION
// ══════════════════════════════════════════
function generateSignal(ind, ses) {
  const {price,prev,e20,e50,e200,rsi,atr,volPct,bigCandle,whale,m15s,h4s,trend,fs}=ind;
  let score=50; const cf=[];
  if(h4s==='BULLISH'){score+=18;cf.push('HTF H4 BULLISH — bias beli dominan');}
  else if(h4s==='BEARISH'){score-=18;cf.push('HTF H4 BEARISH — bias jual dominan');}
  if(m15s==='BULLISH'){score+=12;cf.push('M15 HH/HL structure aktif');}
  else if(m15s==='BEARISH'){score-=12;cf.push('M15 LH/LL structure aktif');}
  if(price>e20&&price>e50&&price>e200){score+=10;cf.push('Harga di atas EMA 20/50/200 (bullish stack)');}
  else if(price<e20&&price<e50&&price<e200){score-=10;cf.push('Harga di bawah EMA 20/50/200 (bearish stack)');}
  if(rsi>=30&&rsi<=65){if(score>50)score+=5;} else if(rsi>70)score-=12; else if(rsi<30){if(score<50)score+=8;}
  if(!ses.good){score-=20;} else cf.push('Sesi ' + ses.name + ' — likuiditas optimal');
  if(volPct>130&&bigCandle){score+=(score>50?10:-10);cf.push('Volume spike + Big candle — momentum institusional');}
  if(whale){score+=(score>50?8:-8);cf.push('Whale activity — smart money flow terdeteksi');}
  score=Math.max(5,Math.min(95,score));
  const satr=atr||price*0.005;
  let signal,entry,sl,tp,rr=2.0;
  if(score>=66&&ses.good&&rsi<72){
    signal='BUY';entry=price;sl=+(price-satr*1.5).toFixed(2);tp=+(price+satr*3.0).toFixed(2);rr=((tp-entry)/(entry-sl)).toFixed(1);
  } else if(score<=34&&ses.good&&rsi>28){
    signal='SELL';entry=price;sl=+(price+satr*1.5).toFixed(2);tp=+(price-satr*3.0).toFixed(2);rr=((entry-tp)/(sl-entry)).toFixed(1);
  } else {
    signal='WAIT';entry=price;sl=price;tp=price;
  }
  const name=currentPair==='BTCUSDT'?'Bitcoin':'Ethereum';
  let reasoning='';
  if(signal==='BUY')reasoning=`Teknikal bullish kuat: ${trend} dengan struktur ${m15s} di M15 dan bias ${h4s} di H4. RSI ${rsi?.toFixed(1)} dalam zona ideal menunjukkan momentum beli ${name} masih valid. Sesi ${ses.name} memberikan likuiditas untuk eksekusi.`;
  else if(signal==='SELL')reasoning=`Teknikal bearish: ${trend} dengan struktur ${m15s} di M15 dan bias ${h4s} di H4. RSI ${rsi?.toFixed(1)} mengkonfirmasi tekanan jual pada ${name} masih dominan. Sesi ${ses.name} mendukung momentum.`;
  else reasoning=`Konfluens belum mencukupi (score ${score}/100) untuk entry ${name}. ${!ses.good?`Sesi ${ses.name} tidak optimal untuk M15. `:''}Tunggu minimal 3 konfluens selaras sebelum entry.`;
  return {signal,entry,sl,tp,rr:parseFloat(rr),confidence:score,reasoning,confluences:cf};
}

// ══════════════════════════════════════════
//  RUN SCAN
// ══════════════════════════════════════════
function runScan(auto=false) {
  if(!IND.price){log('warn','⚠ Fetch data dulu');return;}
  setStatus('s','ANALYSING: TEKNIKAL + FUNDAMENTAL + SMART MONEY...');
  log('info','📊 Generating signal ' + currentPair + '...');
  document.getElementById('btnScan').disabled=true;
  const ses=getSession();
  setTimeout(()=>{
    try{
      const sig=generateSignal(IND,ses);
      renderSignal(sig,ses);
      clrStatus();
      log('ok',`✅ ${sig.signal} | Conf:${sig.confidence}% | E:${sig.entry?.toFixed(2)} TP:${sig.tp?.toFixed(2)} SL:${sig.sl?.toFixed(2)}`);
      if(auto&&sig.signal!=='WAIT') sendTG();
    }catch(e){setStatus('e','SCAN ERROR: '+e.message);log('err','❌ '+e.message);}
    document.getElementById('btnScan').disabled=false;
  },300);
}

// ══════════════════════════════════════════
//  RENDER SIGNAL
// ══════════════════════════════════════════
function renderSignal(sig, ses) {
  lastSig={...sig,ses:ses.name,pair:currentPair,price:IND.price,fundScore:IND.fs,
    rsi:IND.rsi,atr:IND.atr,volPct:IND.volPct,whale:IND.whale};
  document.getElementById('sigSection').style.display='block';
  const C={BUY:'var(--green)',SELL:'var(--red)',WAIT:'var(--amber)'}[sig.signal];
  document.getElementById('sigAccent').style.background=C;
  document.getElementById('sigSecTxt').style.color=C;
  document.getElementById('sigSecTxt').textContent=sig.signal==='WAIT'?'NO TRADE':'SIGNAL TERDETEKSI';
  const sb=document.getElementById('sigBox'); sb.className='sigbox '+sig.signal;
  document.getElementById('sigDir').textContent=sig.signal==='WAIT'?'NO TRADE':sig.signal;
  const n=new Date();
  document.getElementById('sigTime').textContent=`${pad(n.getHours())}:${pad(n.getMinutes())} WIB · ${ses.name}`;
  const pg=document.getElementById('pgrid');
  if(sig.signal!=='WAIT'){
    pg.style.display='grid';
    document.getElementById('pgE').textContent=sig.entry?.toFixed(2)||'—';
    document.getElementById('pgTP').textContent=sig.tp?.toFixed(2)||'—';
    document.getElementById('pgSL').textContent=sig.sl?.toFixed(2)||'—';
    document.getElementById('sigRR').textContent='RR 1:'+sig.rr;
    document.getElementById('sendRow').style.display='grid';
    document.getElementById('btnSend').disabled=false;
  } else {
    pg.style.display='none';
    document.getElementById('sendRow').style.display='none';
  }
  document.getElementById('anlzTxt').textContent=sig.reasoning||'—';
  const ct=document.getElementById('ctags'); ct.innerHTML='';
  sig.confluences?.forEach(c=>{const t=document.createElement('div');t.className='ctag';t.textContent='✓ '+c;ct.appendChild(t);});
  const conf=sig.confidence||0;
  document.getElementById('cpct').textContent=conf+'%';
  document.getElementById('cpct').style.color=conf>=70?'var(--green)':conf>=50?'var(--amber)':'var(--red)';
  setTimeout(()=>{
    const f=document.getElementById('cfill');
    f.style.width=conf+'%';
    f.style.background=conf>=70?'linear-gradient(90deg,var(--green),var(--cyan))':conf>=50?'var(--amber)':'var(--red)';
  },100);
}

// ══════════════════════════════════════════
//  TELEGRAM
// ══════════════════════════════════════════
function buildMsg(s) {
  const n=new Date(), t=`${pad(n.getHours())}:${pad(n.getMinutes())} WIB`;
  const name=s.pair==='BTCUSDT'?'₿ BITCOIN':'Ξ ETHEREUM';
  if(s.signal==='WAIT') return `🟡 NO TRADE — ${name} M15\n\n⏰ ${t} | ${s.ses}\n\n${s.reasoning}\n\n@Kadekpirang_bot`;
  const em=s.signal==='BUY'?'🟢':'🔴', ar=s.signal==='BUY'?'⬆️':'⬇️';
  const cf=s.confluences?.map(c=>`  ✓ ${c}`).join('\n')||'';
  return `${em} M15 ${s.signal} ${name} ${ar}\n\n`+
    `⏰ ${t} | ${s.ses}\n\n`+
    `━━━━━━━━━━━━━━━━\n`+
    `💰 ENTRY  :  ${s.entry?.toFixed(2)}\n`+
    `🎯 TP       :  ${s.tp?.toFixed(2)}\n`+
    `🛑 SL       :  ${s.sl?.toFixed(2)}\n`+
    `⚖️  RR      :  1:${s.rr||2}\n`+
    `━━━━━━━━━━━━━━━━\n\n`+
    `📊 Confidence : ${s.confidence}%\n`+
    `💎 Fund Score  : ${s.fundScore}/100\n`+
    `📈 RSI           : ${s.rsi?.toFixed(1)||'—'}\n`+
    `📉 ATR           : ${s.atr?.toFixed(2)||'—'}\n`+
    `📦 Volume      : ${s.volPct||'—'}%\n`+
    (s.whale?`🐋 Whale       : ACTIVE!\n`:'')+
    (cf?`\nKonfluens:\n${cf}\n`:'')+
    `\n📝 ${s.reasoning}\n\n`+
    `⚠️  Risk max 1-2% per trade!\n🤖 @Kadekpirang_bot | Kadek Capital`;
}

async function tgSend(tok, cid, msg) {
  const short = msg.slice(0,3800);
  // Method 1: GET (paling kompatibel, jalan dari file:// dan GitHub)
  try {
    const p = new URLSearchParams({chat_id:String(cid), text:short});
    const r = await fetch(`https://api.telegram.org/bot${tok}/sendMessage?${p}`,
      {signal:AbortSignal.timeout(10000)});
    const d = await r.json();
    if(d.ok) return {ok:true, method:'GET'};
    throw new Error(d.description);
  } catch(e1) {
    // Method 2: POST form-urlencoded (no preflight CORS)
    try {
      const p = new URLSearchParams({chat_id:String(cid), text:short});
      const r = await fetch(`https://api.telegram.org/bot${tok}/sendMessage`,{
        method:'POST',
        headers:{'Content-Type':'application/x-www-form-urlencoded'},
        body: p.toString(),
        signal: AbortSignal.timeout(10000)
      });
      const d = await r.json();
      if(d.ok) return {ok:true, method:'POST form'};
      throw new Error(d.description);
    } catch(e2) {
      // Method 3: no-cors fallback
      try {
        const p = new URLSearchParams({chat_id:String(cid), text:short.slice(0,800)});
        await fetch(`https://api.telegram.org/bot${tok}/sendMessage?${p}`,
          {mode:'no-cors', signal:AbortSignal.timeout(8000)});
        return {ok:'maybe', method:'no-cors'};
      } catch(e3) { throw new Error(e1.message); }
    }
  }
}

async function sendTG() {
  const s=lastSig; if(!s){log('warn','⚠ Scan dulu');return;}
  const tok=document.getElementById('tok').value.trim(), cid=document.getElementById('cid').value.trim();
  if(!tok||!cid){showTGAlert('w','⚠ Isi Token & Chat ID dulu');return;}
  const btn=document.getElementById('btnSend'); btn.disabled=true; btn.textContent='⏳ MENGIRIM...';
  log('info','📤 Mengirim sinyal ke Telegram...');
  const msg=buildMsg(s);
  try{
    const r=await tgSend(tok,cid,msg);
    if(r.ok===true){log('ok',`📱 ${s.signal} ${s.pair} → @Kadekpirang_bot ✓`);setTGStatus('on');}
    else{log('ok','📱 Sinyal dikirim — cek Telegram');setTGStatus('on');}
  }catch(e){
    log('warn','⚠ Kirim gagal — buka Copy Modal');
    document.getElementById('copyArea').value=msg;
    document.getElementById('copyModal').className='show';
  }
  btn.disabled=false; btn.innerHTML='📤 KIRIM TELEGRAM';
}

async function testTG() {
  const tok=document.getElementById('tok').value.trim(), cid=document.getElementById('cid').value.trim();
  if(!tok||!cid){showTGAlert('w','⚠ Isi Token & Chat ID dulu');return;}
  setTGStatus('chk'); document.getElementById('btnTest').disabled=true;
  showTGAlert('w','🔄 Testing koneksi...');
  const msg='✅ Kadek Capital Bot Aktif!\n\nCrypto Scanner M15 siap\nAnalisa: BTC & ETH\nTeknikal + Fundamental + Smart Money\nData real-time Binance API\n\nSelamat trading! 🚀';
  try{
    const r=await tgSend(tok,cid,msg);
    setTGStatus('on'); tgOK=true;
    showTGAlert('ok',r.ok===true?'✅ Bot terhubung! Cek @Kadekpirang_bot':'✅ Pesan terkirim — cek Telegram');
    log('ok','📱 @Kadekpirang_bot AKTIF ✓');
  }catch(e){
    setTGStatus('off'); tgOK=false;
    let h='';
    if(e.message?.includes('chat not found'))h='\n💡 Chat ID salah → tekan 🔍 DETECT';
    else if(e.message?.includes('Unauthorized'))h='\n💡 Token salah → @BotFather → /mybots';
    showTGAlert('w','⚠ '+e.message+h+'\n\nGunakan 📋 COPY SINYAL untuk kirim manual');
    log('warn','TG: '+e.message);
  }
  document.getElementById('btnTest').disabled=false;
}

async function detectCID() {
  const tok=document.getElementById('tok').value.trim();
  if(!tok){showTGAlert('w','⚠ Isi Bot Token dulu');return;}
  showTGAlert('w','🔍 Mendeteksi Chat ID...');
  try{
    const r=await fetch(`https://api.telegram.org/bot${tok}/getUpdates?limit=10&offset=-10`,{signal:AbortSignal.timeout(10000)});
    const d=await r.json(); if(!d.ok)throw new Error(d.description);
    if(!d.result?.length){showTGAlert('w','⚠ Bot belum ada pesan.\nBuka @Kadekpirang_bot → kirim /start → DETECT lagi');return;}
    const ids={}; d.result.forEach(u=>{if(u.message)ids[String(u.message.chat.id)]=u.message.chat.first_name||'User';});
    const keys=Object.keys(ids);
    if(!keys.length){showTGAlert('w','⚠ Tidak ditemukan. Kirim /start ke bot dulu.');return;}
    const id=keys[keys.length-1];
    document.getElementById('cid').value=id;
    showTGAlert('ok',`✅ Chat ID: ${id} (${ids[id]})\nSudah terisi → tekan TEST ▶️`);
    log('ok','Chat ID: '+id);
  }catch(e){showTGAlert('w','⚠ Auto-detect gagal.\nCari Chat ID di @userinfobot');}
}

function setTGStatus(st) {
  const el=document.getElementById('tgSt');
  el.className='tg-st '+st;
  el.textContent=st==='on'?'● ONLINE ✓':st==='chk'?'● CHECKING...':'● OFFLINE';
  document.getElementById('tdot').className='dot '+(st==='on'?'g':st==='chk'?'a':'r');
  document.getElementById('tlbl').style.color=st==='on'?'var(--green)':st==='chk'?'var(--amber)':'var(--red)';
}
function showTGAlert(t,m){const el=document.getElementById('tgAlert');el.textContent=m;el.className='alert '+t;}

// ══════════════════════════════════════════
//  AUTO TOGGLE
// ══════════════════════════════════════════
function toggleAuto() {
  autoOn=!autoOn;
  document.getElementById('togTrack').className='tog-track '+(autoOn?'on':'');
  document.getElementById('togThumb').className='tog-thumb '+(autoOn?'on':'off');
  document.getElementById('autoTxt').textContent=autoOn?'● AKTIF — signal otomatis tiap 60s':'○ OFF — tap toggle untuk aktifkan';
  document.getElementById('autoTxt').style.color=autoOn?'var(--green)':'var(--muted)';
  log(autoOn?'ok':'info',autoOn?'🟢 Auto scan ON':'⚫ Auto scan OFF');
}

// ══════════════════════════════════════════
//  COPY MODAL
// ══════════════════════════════════════════
function openCopy(){if(!lastSig)return;document.getElementById('copyArea').value=buildMsg(lastSig);document.getElementById('copyModal').className='show';}
function closeCopy(){document.getElementById('copyModal').className='';}
async function doCopy(){
  const ta=document.getElementById('copyArea');
  try{await navigator.clipboard.writeText(ta.value);}catch{ta.select();document.execCommand('copy');}
  const b=document.getElementById('btnCopy');
  b.textContent='✅ TERSALIN!';b.style.borderColor='var(--green)';b.style.color='var(--green)';
  setTimeout(()=>{b.textContent='📋 COPY';b.style.borderColor='';b.style.color='';},3000);
}

// ══════════════════════════════════════════
//  INIT
// ══════════════════════════════════════════
window.addEventListener('load', () => {
  log('info','🚀 Kadek Capital Crypto Scanner dimulai...');
  log('info','📡 Connecting to Binance API...');
  initChart();
  setTimeout(fetchData, 400);
  setTimeout(testTG, 2000);
});
</script>
</body>
</html>
