<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
<title>XAUUSD M15 · Kadek Bloomberg</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Share+Tech+Mono&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
<script src="https://unpkg.com/lightweight-charts@4.1.0/dist/lightweight-charts.standalone.production.js"></script>
<style>
:root{
  --bg:#060608;--panel:#0c0c10;--card:#111116;--b1:#181820;--b2:#222230;
  --orange:#ff6600;--amber:#ffb700;--cyan:#00c8ff;--green:#00d97e;--red:#ff2d55;
  --purple:#a855f7;--muted:#404055;--dim:#09090c;--text:#c0c8d8;--bright:#e8ecf4;
}
*{margin:0;padding:0;box-sizing:border-box;}
html,body{background:var(--bg);color:var(--text);font-family:'Inter',sans-serif;min-height:100%;}
::-webkit-scrollbar{width:3px}::-webkit-scrollbar-track{background:var(--dim)}::-webkit-scrollbar-thumb{background:var(--b2)}
/* TOPBAR */
#topbar{position:sticky;top:0;z-index:200;background:var(--dim);border-bottom:1px solid var(--b2);display:flex;align-items:center;justify-content:space-between;padding:0 12px;height:42px;}
.brand{display:flex;align-items:center;gap:8px;}
.brand-box{background:var(--orange);padding:2px 8px;}
.brand-name{font-family:'Bebas Neue',monospace;font-size:16px;color:var(--bg);letter-spacing:3px;}
.brand-sub{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);letter-spacing:3px;}
.topbar-right{display:flex;align-items:center;gap:12px;}
.live-badge{display:flex;align-items:center;gap:4px;}
.dot{width:6px;height:6px;border-radius:50%;flex-shrink:0;}
.dot.g{background:var(--green);box-shadow:0 0 5px var(--green);animation:blink 1.2s infinite;}
.dot.r{background:var(--red);}
.dot.a{background:var(--amber);animation:blink 1.2s infinite;}
.dot-txt{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:1px;}
#clk{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);}
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
/* WRAP */
.wrap{max-width:700px;margin:0 auto;padding:10px 11px 80px;}
/* STATUS */
.status{display:none;padding:9px 14px;margin-bottom:8px;font-family:'Share Tech Mono',monospace;font-size:10px;align-items:center;gap:10px;}
.status.show{display:flex;}
.status.f{background:rgba(255,102,0,.07);border:1px solid var(--orange);border-left:3px solid var(--orange);color:var(--orange);}
.status.s{background:rgba(255,183,0,.07);border:1px solid var(--amber);border-left:3px solid var(--amber);color:var(--amber);}
.status.e{background:rgba(255,45,85,.07);border:1px solid var(--red);border-left:3px solid var(--red);color:var(--red);}
.spin{width:13px;height:13px;border:2px solid rgba(255,255,255,.1);border-radius:50%;animation:spin .7s linear infinite;flex-shrink:0;}
@keyframes spin{to{transform:rotate(360deg)}}
/* PRICE */
.price-hero{background:var(--panel);border:1px solid var(--b1);border-left:3px solid var(--orange);padding:13px 15px;margin-bottom:8px;display:flex;justify-content:space-between;align-items:flex-start;}
.plbl{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:3px;color:var(--muted);margin-bottom:3px;}
.pbig{font-family:'Bebas Neue',monospace;font-size:54px;color:var(--bright);letter-spacing:2px;line-height:1;transition:color .3s;}
.pbig.up{color:var(--green)!important;} .pbig.dn{color:var(--red)!important;}
.pchg{font-family:'Share Tech Mono',monospace;font-size:11px;margin-top:2px;}
.phl{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--muted);margin-top:2px;}
.ses{font-family:'Share Tech Mono',monospace;font-size:9px;letter-spacing:1px;padding:4px 10px;border:1px solid;display:inline-block;margin-bottom:5px;}
.ses.prime{border-color:var(--green);color:var(--green);background:rgba(0,217,126,.06);}
.ses.high{border-color:var(--amber);color:var(--amber);background:rgba(255,183,0,.06);}
.ses.low{border-color:var(--muted);color:var(--muted);}
.rinfo{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--muted);}
/* CHART */
.chart-wrap{background:var(--panel);border:1px solid var(--b1);margin-bottom:8px;overflow:hidden;}
.chart-bar{background:var(--dim);padding:7px 12px;display:flex;justify-content:space-between;align-items:center;border-bottom:1px solid var(--b2);}
.chart-title{display:flex;align-items:center;gap:7px;}
.cbar{width:3px;height:13px;background:var(--orange);}
.clbl{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--orange);letter-spacing:3px;}
.chart-legs{display:flex;gap:12px;}
.cleg{font-family:'Share Tech Mono',monospace;font-size:8px;}
#chartEl{width:100%;}
/* SECTION */
.sec{display:flex;align-items:center;gap:7px;margin:10px 0 7px;}
.sbar{width:3px;height:13px;}
.stxt{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:3px;}
.sline{flex:1;height:1px;background:linear-gradient(90deg,var(--b2),transparent);}
/* IND GRID */
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
.stbox.bull{border-color:var(--green);background:rgba(0,217,126,.04);}
.stbox.bear{border-color:var(--red);background:rgba(255,45,85,.04);}
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
.score-mid{position:absolute;left:50%;top:0;width:1px;height:100%;background:var(--b2);}
.score-fill{position:absolute;height:100%;transition:all .8s;}
.news-lbl{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);margin:10px 0 6px;}
.nitem{font-family:'Share Tech Mono',monospace;font-size:10px;color:var(--text);padding:5px 0;border-bottom:1px solid var(--b1);line-height:1.5;}
.nitem:last-child{border:none;}
.narr{color:var(--orange);margin-right:4px;}
/* TG */
.tg-panel{background:var(--panel);border:1px solid var(--b1);border-left:2px solid var(--cyan);padding:13px;margin-bottom:8px;}
.tg-hdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:9px;}
.tg-lbl{font-family:'Share Tech Mono',monospace;font-size:9px;letter-spacing:3px;color:var(--cyan);}
.tg-st{font-family:'Share Tech Mono',monospace;font-size:9px;padding:3px 9px;}
.tg-st.off{background:rgba(255,45,85,.1);color:var(--red);}
.tg-st.on{background:rgba(0,217,126,.1);color:var(--green);}
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
.alert.ok{background:rgba(0,217,126,.06);border:1px solid rgba(0,217,126,.3);color:var(--green);display:block;}
.alert.w{background:rgba(255,183,0,.06);border:1px solid rgba(255,183,0,.3);color:var(--amber);display:block;}
.alert.e{background:rgba(255,45,85,.06);border:1px solid rgba(255,45,85,.3);color:var(--red);display:block;}
/* AUTO */
.autobar{display:flex;align-items:center;justify-content:space-between;background:var(--panel);border:1px solid var(--b1);padding:8px 13px;margin-bottom:10px;}
.at{font-family:'Share Tech Mono',monospace;font-size:9px;color:var(--bright);}
.as{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);margin-top:2px;}
.tog{position:relative;width:44px;height:24px;cursor:pointer;flex-shrink:0;}
.tog-track{position:absolute;inset:0;border-radius:24px;background:var(--dim);border:1px solid var(--b2);transition:.3s;}
.tog-track.on{background:rgba(0,217,126,.12);border-color:var(--green);}
.tog-thumb{position:absolute;width:18px;height:18px;top:2px;left:2px;border-radius:50%;transition:.3s;}
.tog-thumb.off{background:var(--muted);}
.tog-thumb.on{left:24px;background:var(--green);box-shadow:0 0 8px var(--green);}
/* SCAN BTN */
.scan-btn{width:100%;padding:16px;background:linear-gradient(135deg,rgba(255,102,0,.08),rgba(255,183,0,.08));border:1px solid var(--orange);color:var(--orange);font-family:'Bebas Neue',monospace;font-size:21px;letter-spacing:7px;cursor:pointer;position:relative;overflow:hidden;text-shadow:0 0 10px rgba(255,102,0,.4);box-shadow:0 0 20px rgba(255,102,0,.08);transition:all .3s;margin-bottom:10px;}
.scan-btn:hover:not(:disabled){box-shadow:0 0 30px rgba(255,102,0,.2);}
.scan-btn:disabled{border-color:var(--muted);color:var(--muted);cursor:not-allowed;text-shadow:none;box-shadow:none;background:transparent;}
.sweep{position:absolute;inset:0;background:linear-gradient(90deg,transparent,rgba(255,102,0,.06),transparent);transform:translateX(-100%);animation:sweep 1.5s linear infinite;}
@keyframes sweep{to{transform:translateX(100%)}}
/* SIGNAL */
.sigbox{padding:16px;margin-bottom:8px;border-width:1px 1px 1px 4px;border-style:solid;text-align:center;transition:all .4s;}
.sigbox.BUY{border-color:var(--green);background:rgba(0,217,126,.04);}
.sigbox.SELL{border-color:var(--red);background:rgba(255,45,85,.04);}
.sigbox.WAIT{border-color:var(--amber);background:rgba(255,183,0,.03);}
.sp{font-family:'Share Tech Mono',monospace;font-size:10px;letter-spacing:5px;color:var(--muted);margin-bottom:3px;}
.sd{font-family:'Bebas Neue',monospace;font-size:58px;letter-spacing:7px;line-height:1;}
.BUY .sd{color:var(--green);text-shadow:0 0 30px rgba(0,217,126,.5);}
.SELL .sd{color:var(--red);text-shadow:0 0 30px rgba(255,45,85,.5);}
.WAIT .sd{color:var(--amber);}
.st{font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);margin-top:3px;}
.pgrid{display:grid;grid-template-columns:repeat(3,1fr);gap:5px;margin-top:13px;}
.pb{background:var(--dim);border:1px solid var(--b1);padding:10px 7px;}
.pk{font-family:'Share Tech Mono',monospace;font-size:7px;letter-spacing:2px;color:var(--muted);margin-bottom:3px;}
.pv{font-family:'Bebas Neue',monospace;font-size:23px;letter-spacing:1px;}
.BUY .pb:nth-child(1) .pv{color:var(--amber);}
.BUY .pb:nth-child(2) .pv{color:var(--green);}
.BUY .pb:nth-child(3) .pv{color:var(--red);}
.SELL .pb:nth-child(1) .pv{color:var(--amber);}
.SELL .pb:nth-child(2) .pv{color:var(--green);}
.SELL .pb:nth-child(3) .pv{color:var(--red);}
.WAIT .pb .pv{color:var(--muted);}
.rr{display:inline-block;font-family:'Share Tech Mono',monospace;font-size:10px;padding:3px 13px;margin-top:10px;}
.BUY .rr{background:rgba(0,217,126,.1);color:var(--green);border:1px solid rgba(0,217,126,.3);}
.SELL .rr{background:rgba(255,45,85,.1);color:var(--red);border:1px solid rgba(255,45,85,.3);}
.WAIT .rr{display:none!important;}
/* ANLZ */
.anlz{background:var(--panel);border:1px solid var(--b1);padding:13px;margin-bottom:8px;}
.at2{font-family:'Share Tech Mono',monospace;font-size:8px;letter-spacing:3px;color:var(--muted);margin-bottom:8px;}
.atext{font-size:12px;line-height:1.8;color:var(--text);}
.ctags{display:flex;flex-wrap:wrap;gap:4px;margin-top:9px;}
.ctag{font-family:'Share Tech Mono',monospace;font-size:8px;padding:2px 9px;border:1px solid rgba(255,102,0,.4);color:var(--orange);background:rgba(255,102,0,.06);}
.cbar-wrap{margin-top:11px;}
.cbar-row{display:flex;justify-content:space-between;font-family:'Share Tech Mono',monospace;font-size:8px;color:var(--muted);margin-bottom:4px;}
.cbar-bg{height:4px;background:var(--dim);border:1px solid var(--b1);overflow:hidden;}
.cbar-fill{height:100%;transition:width 1s ease;}
/* SEND */
.send-row{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:10px;}
.sbtn{padding:13px;font-family:'Bebas Neue',monospace;font-size:15px;letter-spacing:4px;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:7px;transition:all .3s;}
.sbtn.tg{background:rgba(0,200,255,.06);border:1px solid var(--cyan);color:var(--cyan);}
.sbtn.tg:hover:not(:disabled){background:var(--cyan);color:var(--bg);}
.sbtn.cp{background:rgba(255,183,0,.06);border:1px solid var(--amber);color:var(--amber);}
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
<div id="topbar">
  <div class="brand">
    <div class="brand-box"><span class="brand-name">KADEK</span></div>
    <span class="brand-sub">BLOOMBERG TERMINAL</span>
  </div>
  <div class="topbar-right">
    <div class="live-badge"><div class="dot r" id="bdot"></div><span class="dot-txt" id="blbl" style="color:var(--red)">BYBIT</span></div>
    <div class="live-badge"><div class="dot r" id="tdot"></div><span class="dot-txt" id="tlbl" style="color:var(--red)">TELEGRAM</span></div>
    <span id="clk">--:--:--</span>
  </div>
</div>

<div class="wrap">
  <div class="status" id="sb"><div class="spin" id="sp"></div><span id="stxt"></span></div>

  <!-- PRICE -->
  <div class="price-hero">
    <div>
      <div class="plbl">XAUUSD · SPOT GOLD · USD/OZ</div>
      <div class="pbig" id="pm">——</div>
      <div class="pchg" id="pc"></div>
      <div class="phl" id="phl"></div>
    </div>
    <div style="text-align:right">
      <div class="ses low" id="sesbadge">⏳ LOADING</div>
      <div class="rinfo">Refresh: <b id="cdn" style="color:var(--cyan)">60</b>s</div>
      <div class="rinfo" id="lupd"></div>
    </div>
  </div>

  <!-- CHART -->
  <div class="chart-wrap">
    <div class="chart-bar">
      <div class="chart-title"><div class="cbar"></div><span class="clbl">CANDLESTICK M15 · BYBIT</span></div>
      <div class="chart-legs">
        <span class="cleg" style="color:var(--orange)">── EMA20</span>
        <span class="cleg" style="color:var(--cyan)">── EMA50</span>
        <span class="cleg" style="color:var(--purple)">── EMA200</span>
      </div>
    </div>
    <div id="chartEl"></div>
  </div>

  <!-- INDICATORS -->
  <div class="sec"><div class="sbar" style="background:var(--orange)"></div><span class="stxt" style="color:var(--orange)">INDIKATOR TEKNIKAL M15</span><div class="sline"></div></div>
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
  <div class="sec"><div class="sbar" style="background:var(--cyan)"></div><span class="stxt" style="color:var(--cyan)">MARKET STRUCTURE · AUTO DETECT</span><div class="sline"></div></div>
  <div class="sg">
    <div><div class="slbl">M15 STRUCTURE</div><div class="stbox rang" id="m15s"><div class="sticon">⏳</div><div class="stname rang">LOADING</div><div class="stsub">fetching...</div></div></div>
    <div><div class="slbl">H4 BIAS (HTF)</div><div class="stbox rang" id="h4s"><div class="sticon">⏳</div><div class="stname rang">LOADING</div><div class="stsub">fetching...</div></div></div>
  </div>

  <!-- FUNDAMENTAL -->
  <div class="sec">
