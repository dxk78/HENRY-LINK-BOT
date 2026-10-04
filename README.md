<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1">
<title>HENRY 🚩 — WhatsApp Pairing</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600;700;800&family=Sora:wght@400;500;600;700;800&family=JetBrains+Mono:wght@600;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<style>
  :root{
    --radius-lg: 22px;
    --radius-md: 13px;
    --radius-sm: 8px;
  }

  html[data-theme="dark"]{
    --bg: #050507;
    --card-bg: #121216;
    --card-bg-2: #1d1d23;
    --card-border: rgba(255,255,255,0.16);
    --ember: #ff8f5a;
    --card-spot: rgba(255,58,80,0.16);
    --beam-a: 0.22;
    --card-border-accent: rgba(255,58,80,0.55);
    --line: rgba(255,255,255,0.09);
    --red: #ff3a50;
    --red-2: #ff7086;
    --red-dim: #6e0f1e;
    --red-glow: rgba(255,58,80,0.55);
    --silver: #d3d8e0;
    --silver-dim: #98a0ad;
    --silver-bright: #ffffff;
    --success: #34d67f;
    --input-bg: rgba(255,255,255,0.045);
    --input-border: rgba(255,255,255,0.12);
    --shadow-ambient: 0 40px 90px -35px rgba(0,0,0,0.9);
    --code-muted: #565d69;
    --grain-opacity: 0.05;
  }

  html[data-theme="light"]{
    --bg: #eeece9;
    --ember: #ff7a45;
    --card-spot: rgba(224,25,50,0.07);
    --beam-a: 0.12;
    --card-bg: #ffffff;
    --card-bg-2: #fbfaf8;
    --card-border: rgba(20,20,25,0.12);
    --card-border-accent: rgba(224,25,50,0.4);
    --line: rgba(20,20,25,0.09);
    --red: #e01932;
    --red-2: #ff4d64;
    --red-dim: #ffd7dc;
    --red-glow: rgba(224,25,50,0.28);
    --silver: #4b515c;
    --silver-dim: #6b7180;
    --silver-bright: #16171b;
    --success: #1c9c5f;
    --input-bg: rgba(20,20,25,0.035);
    --input-border: rgba(20,20,25,0.1);
    --shadow-ambient: 0 30px 70px -35px rgba(30,20,25,0.2);
    --code-muted: #a3a8b1;
    --grain-opacity: 0.025;
  }

  *{ box-sizing:border-box; }
  html,body{ margin:0; padding:0; }
  body{
    background: var(--bg);
    color: var(--silver);
    font-family: 'Sora', sans-serif;
    min-height:100vh;
    overflow-x:hidden;
    position:relative;
    transition: background .35s ease, color .35s ease;
    -webkit-font-smoothing: antialiased;
  }

  /* ---------- premium layered background ---------- */
  .bg-layer{ position:fixed; inset:0; z-index:0; pointer-events:none; overflow:hidden; }

  .bg-glow{
    position:absolute; border-radius:50%; filter: blur(120px);
    transition: opacity .4s ease;
  }
  .bg-glow.top{ width:900px; height:600px; top:-320px; left:50%; transform:translateX(-50%);
    background: radial-gradient(ellipse, var(--red-glow), transparent 65%); opacity:0.28; }
  html[data-theme="light"] .bg-glow.top{ opacity:0.18; }
  .bg-glow.left{ width:500px; height:500px; bottom:-220px; left:-220px;
    background: radial-gradient(circle, rgba(160,170,190,0.18), transparent 70%); opacity:0.5; }
  .bg-glow.right{ width:420px; height:420px; top:20%; right:-200px;
    background: radial-gradient(circle, rgba(255,58,80,0.14), transparent 70%); opacity:0.6;
    animation: drift 16s ease-in-out infinite alternate;
  }
  @keyframes drift{ from{ transform:translateY(0); } to{ transform:translateY(60px); } }

  .bg-beam{
    position:absolute; top:-8%; left:50%; width:1200px; height:115%; transform:translateX(-50%);
    background: conic-gradient(from 180deg at 50% 0%, transparent 152deg, rgba(255,58,80,calc(var(--beam-a) * 0.35)) 165deg, rgba(255,58,80,var(--beam-a)) 180deg, rgba(255,58,80,calc(var(--beam-a) * 0.35)) 195deg, transparent 208deg);
    -webkit-mask-image: linear-gradient(180deg, #000 10%, transparent 85%); mask-image: linear-gradient(180deg, #000 10%, transparent 85%);
    will-change: opacity;
    animation: beamBreathe 7s ease-in-out infinite alternate;
  }
  @keyframes beamBreathe{ from{ opacity:0.55; } to{ opacity:1; } }
  .bg-lines{
    position:absolute; inset:0;
    background-image:
      repeating-linear-gradient(115deg, rgba(255,255,255,0.02) 0px, rgba(255,255,255,0.02) 1px, transparent 1px, transparent 90px);
  }
  html[data-theme="light"] .bg-lines{
    background-image: repeating-linear-gradient(115deg, rgba(20,20,25,0.025) 0px, rgba(20,20,25,0.025) 1px, transparent 1px, transparent 90px);
  }

  .bg-grain{
    position:absolute; inset:-10%; width:120%; height:120%;
    opacity: var(--grain-opacity);
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='120' height='120'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='2' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    mix-blend-mode: overlay;
  }

  #neonWaves{
    position:fixed; inset:0; z-index:0; width:100%; height:100%;
    opacity:0.85; transition: opacity .4s ease;
  }
  html[data-theme="light"] #neonWaves{ opacity:0.55; }

  /* ---------- nav ---------- */
  nav{
    position:relative; z-index:3;
    display:flex; align-items:center; justify-content:space-between;
    padding: 18px 40px;
    border-bottom: 1px solid var(--line);
    backdrop-filter: blur(10px);
  }
  .brand{ display:flex; align-items:center; gap:12px; }
  .brand-mark{
    width:36px; height:36px; border-radius:10px;
    border:1px solid var(--card-border-accent);
    background: linear-gradient(145deg, rgba(255,58,80,0.14), rgba(255,255,255,0.02));
    display:flex; align-items:center; justify-content:center;
    box-shadow: 0 0 14px var(--red-glow);
    flex-shrink:0;
  }
  .brand-mark svg{ width:17px; height:17px; color: var(--red); }
  .brand-name{ font-family:'Orbitron', sans-serif; font-weight:800; letter-spacing:1px; font-size:16px; color: var(--silver-bright); white-space:nowrap; }
  .brand-name span{ color: var(--red); }

  .nav-right{ display:flex; align-items:center; gap:22px; }
  .nav-links{ display:flex; align-items:center; gap:22px; }
  .nav-links a{
    color: var(--silver-dim); text-decoration:none; font-size:13.5px; font-weight:600; letter-spacing:0.2px;
    display:flex; align-items:center; gap:6px; transition: color .2s ease;
  }
  .nav-links a:hover{ color: var(--silver-bright); }
  .nav-links a.active{ color: var(--red); }
  .nav-links a svg{ width:14px; height:14px; }

  .theme-toggle{
    width:40px; height:40px; border-radius:11px;
    border:1px solid var(--card-border);
    background: var(--input-bg);
    display:flex; align-items:center; justify-content:center;
    cursor:pointer; position:relative;
    transition: border-color .2s ease, transform .15s ease;
    flex-shrink:0;
  }
  .theme-toggle:hover{ border-color: var(--red); transform: translateY(-1px); }
  .theme-toggle svg{ width:17px; height:17px; color: var(--silver-bright); position:absolute; transition: opacity .25s ease, transform .35s ease; }
  .icon-sun{ opacity:0; transform: rotate(-60deg) scale(.5); }
  .icon-moon{ opacity:1; transform: rotate(0) scale(1); }
  html[data-theme="light"] .icon-sun{ opacity:1; transform: rotate(0) scale(1); color:#e01932; }
  html[data-theme="light"] .icon-moon{ opacity:0; transform: rotate(60deg) scale(.5); }

  /* ---------- main ---------- */
  main{ position:relative; z-index:2; display:flex; justify-content:center; padding: 58px 20px 44px; }

  .card{
    width:100%; max-width:680px;
    background: radial-gradient(90% 55% at 50% 0%, var(--card-spot), transparent 72%), linear-gradient(175deg, var(--card-bg-2), var(--card-bg));
    border:1.5px solid var(--card-border);
    border-radius: var(--radius-lg);
    padding:56px 52px 44px;
    position:relative;
    box-shadow: var(--shadow-ambient), 0 0 0 1px rgba(255,255,255,0.02), 0 0 46px -18px var(--red-glow);
  }
  .card::before{
    content:""; position:absolute; inset:-1.5px; border-radius: var(--radius-lg); padding:1.5px;
    background: linear-gradient(140deg, var(--card-border-accent), rgba(255,255,255,0.16) 35%, rgba(255,255,255,0.16) 65%, var(--card-border-accent));
    background-size: 240% 240%; animation: borderFlow 9s ease-in-out infinite alternate;
    -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor; mask-composite: exclude;
    pointer-events:none;
  }
  .card-light{
    position:absolute; inset:0; border-radius:inherit; pointer-events:none; opacity:0; transition: opacity .35s ease;
    background: radial-gradient(340px circle at var(--mx, 50%) var(--my, 0%), rgba(255,110,125,0.12), transparent 65%);
  }
  .card:hover .card-light{ opacity:1; }
  @keyframes borderFlow{ from{ background-position:0% 0%; } to{ background-position:100% 100%; } }
  .card::after{
    content:""; position:absolute; top:0; left:12%; right:12%; height:1px;
    background: linear-gradient(90deg, transparent, var(--red), transparent);
    opacity:0.7; filter: blur(0.3px);
  }

  /* ---------- signature gauge ---------- */
  .gauge-wrap{ display:flex; justify-content:center; margin-bottom:20px; }
  .gauge{ width:116px; height:116px; position:relative; }
  .gauge svg{ width:100%; height:100%; overflow:visible; }
  .gauge-track{ stroke: var(--line); }
  .gauge-fill{
    stroke: url(#gaugeGrad);
    stroke-linecap:round;
    filter: drop-shadow(0 0 6px var(--red-glow));
    animation: fillPulse 3.2s ease-in-out infinite;
  }
  @keyframes fillPulse{
    0%,100%{ stroke-dashoffset: 130; opacity:0.85; }
    50%{ stroke-dashoffset: 96; opacity:1; }
  }
  .gauge-tick{ stroke: var(--silver-dim); opacity:0.5; }
  .gauge-center{
    position:absolute; inset:0; display:flex; align-items:center; justify-content:center;
  }
  .gauge-center::before{
    content:""; position:absolute; inset:0; margin:auto; width:68%; height:68%; border-radius:50%;
    background: conic-gradient(from 0deg, transparent 0 50%, var(--red) 80%, #fff 100%);
    -webkit-mask: radial-gradient(farthest-side, transparent calc(100% - 3px), #000 calc(100% - 2px));
            mask: radial-gradient(farthest-side, transparent calc(100% - 3px), #000 calc(100% - 2px));
    filter: drop-shadow(0 0 6px var(--red-glow));
    animation: rotate 3.4s linear infinite;
  }
  .gauge-center-inner{
    width:64px; height:64px; border-radius:50%;
    background: radial-gradient(circle at 30% 22%, #ffffff 0%, #e6e9ef 28%, #aeb5c2 66%, #737a8a 100%);
    border:1px solid rgba(255,255,255,0.6);
    display:flex; align-items:center; justify-content:center;
    box-shadow: 0 0 0 4px var(--card-bg), 0 0 34px var(--red-glow), inset 0 -8px 14px rgba(0,0,0,0.26), inset 0 2px 3px rgba(255,255,255,0.95);
    overflow:hidden;
  }
  .gauge-center-inner svg{ width:54%; height:54%; filter: drop-shadow(0 1px 0 rgba(255,255,255,0.75)) drop-shadow(0 3px 6px rgba(200,20,45,0.4)); }
  .gauge-center-inner img{ width:100%; height:100%; object-fit:cover; border-radius:50%; display:block; }

  h1.title{
    font-family:'Orbitron', sans-serif; text-align:center; font-size:29px; font-weight:800; letter-spacing:1.4px; margin:0 0 8px;
    background: linear-gradient(180deg, var(--silver-bright) 30%, var(--silver-dim));
    -webkit-background-clip:text; background-clip:text; -webkit-text-fill-color: transparent; color: transparent;
  }
  h1.title span{
    background: linear-gradient(90deg, var(--red), var(--ember));
    -webkit-background-clip:text; background-clip:text; -webkit-text-fill-color: transparent; color: transparent;
  }
  html[data-theme="dark"] h1.title{ filter: drop-shadow(0 0 20px rgba(255,58,80,0.28)); }
  .subtitle{ text-align:center; color: var(--silver-dim); font-size:15px; margin:0 0 34px; font-weight:500; }

  .field-row{
    display:flex; align-items:center; justify-content:space-between; gap:12px;
    background: var(--input-bg); border:1px solid var(--input-border); border-radius: var(--radius-md);
    padding:14px 18px; margin-bottom:20px;
    transition: border-color .2s ease, box-shadow .2s ease;
  }
  .field-row:hover{ border-color: var(--card-border-accent); box-shadow: 0 0 18px -8px var(--red-glow); }
  #serverSummary{ cursor:pointer; scroll-margin-top:18px; }
  #serverSummary.attention{ animation: attention .6s ease; border-color: var(--red); }
  @keyframes attention{
    0%,100%{ transform: translateX(0); }
    20%{ transform: translateX(-6px); } 40%{ transform: translateX(6px); }
    60%{ transform: translateX(-4px); } 80%{ transform: translateX(4px); }
  }
  .field-left{ display:flex; align-items:center; gap:14px; min-width:0; }
  .field-icon{ width:34px; height:34px; border-radius:9px; background: rgba(255,58,80,0.1); border:1px solid rgba(255,58,80,0.3); display:flex; align-items:center; justify-content:center; flex-shrink:0; }
  .field-icon svg{ width:16px; height:16px; color: var(--red); }
  .field-title{ font-weight:700; font-size:14px; color: var(--silver-bright); white-space:nowrap; }
  .field-sub{ font-size:11.5px; color: var(--silver-dim); margin-top:2px; display:flex; align-items:center; gap:6px; }
  .status-dot{ width:7px; height:7px; border-radius:50%; background: var(--success); box-shadow:0 0 8px var(--success); display:inline-block; flex-shrink:0; }
  .status-dot.offline{ background: var(--red); box-shadow:0 0 8px var(--red); }

  select.server-select{
    appearance:none; background: var(--input-bg); border:1px solid var(--card-border-accent);
    color: var(--silver-bright); font-family:'Sora', sans-serif; font-weight:700; font-size:13px;
    padding:8px 28px 8px 14px; border-radius: var(--radius-sm); cursor:pointer; flex-shrink:0;
    background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23ff3a50' stroke-width='2.5'><path d='M6 9l6 6 6-6'/></svg>");
    background-repeat:no-repeat; background-position: right 9px center; background-size: 12px;
  }

  .server-select-btn{
    background: transparent; border:1px solid var(--card-border-accent); color: var(--silver-bright);
    font-family:'Sora', sans-serif; font-weight:700; font-size:12.5px; padding:9px 16px; border-radius: var(--radius-sm);
    cursor:pointer; display:flex; align-items:center; gap:8px; transition: all .25s ease; flex-shrink:0;
  }
  .server-select-btn:hover{ background: rgba(255,58,80,0.08); box-shadow: 0 0 18px -8px var(--red-glow); }
  .server-select-btn svg{ width:11px; height:11px; color: var(--red); transition: transform .25s ease; }
  .server-select-btn.open svg{ transform: rotate(180deg); }

  .picker{
    display:none; background: var(--card-bg); border:1px solid var(--card-border-accent); border-radius: var(--radius-md);
    padding:20px; margin-bottom:20px; max-height:430px; overflow-y:auto;
    box-shadow: var(--shadow-ambient);
    scrollbar-width:thin; scrollbar-color:var(--red) var(--input-bg);
  }
  .picker.show{ display:block; animation: dropDown .3s ease; }
  @keyframes dropDown{ from{ opacity:0; transform: translateY(-8px) scale(.98); } to{ opacity:1; transform: translateY(0) scale(1); } }

  .picker::-webkit-scrollbar{width:10px;}
  .picker::-webkit-scrollbar-track{background:var(--input-bg);border-radius:10px;}
  .picker::-webkit-scrollbar-thumb{background:linear-gradient(180deg,var(--red),rgba(255,58,80,.48));border:2px solid var(--card-bg);border-radius:10px;}
  .picker-title{ font-size:18px; font-weight:800; color: var(--silver-bright); letter-spacing:-.02em; margin-bottom:16px; }
  .picker-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:11px;}

  .server-option{
    background:linear-gradient(145deg,var(--input-bg),rgba(255,255,255,.015)); border:1px solid var(--input-border); border-radius:15px;
    padding:14px 14px 13px; min-height:78px; cursor:pointer; display:flex; flex-direction:column; align-items:stretch; justify-content:space-between; gap:8px; min-width:0;
    color: var(--silver-bright); font-family:'Sora', sans-serif; transition:transform .2s ease, border-color .2s ease, box-shadow .2s ease, background .2s ease; text-align:left; width:100%;
  }
  .server-option:hover{ background:rgba(255,58,80,.08);border-color:var(--card-border-accent);transform:translateY(-2px);box-shadow:0 10px 22px -18px var(--red-glow); }
  .server-option.selected{background:linear-gradient(135deg,rgba(255,58,80,.18),rgba(255,58,80,.055));border-color:var(--red);box-shadow:inset 0 0 0 1px rgba(255,58,80,.14),0 8px 24px -20px var(--red-glow);}
  .server-option .opt-row{ display:flex; align-items:center; justify-content:space-between; gap:8px; min-width:0; }
  .server-option .opt-name{ font-weight:700; font-size:13px; overflow:hidden; text-overflow:ellipsis; white-space:nowrap; min-width:0; }
  .server-option .opt-meta{ font-size:11px; color: var(--silver-dim); display:block; }
  .server-option .opt-tail{ display:flex; align-items:center; gap:7px; flex-shrink:0; }
  .server-option .opt-check{ color:var(--red); font-size:13px; font-weight:800; line-height:1; }
  .server-option .opt-bar{ display:block; height:3px; border-radius:3px; background: var(--line); overflow:hidden; }
  .server-option .opt-bar i{ display:block; height:100%; border-radius:3px; background: linear-gradient(90deg, var(--red), var(--ember)); transition: width .4s ease; }
  .server-option .opt-dot{ width:8px; height:8px; border-radius:50%; background: var(--silver-dim); flex-shrink:0; transition: all .3s ease; }
  .server-option .opt-dot.online{ background: var(--success); box-shadow: 0 0 10px var(--success); animation: dotPulse 1.5s ease-in-out infinite; }
  @keyframes dotPulse{ 0%,100%{ opacity:1; transform: scale(1); } 50%{ opacity:.6; transform: scale(.8); } }
  .picker-empty{ grid-column:1 / -1; color: var(--silver-dim); text-align:center; padding:16px; font-size:12.5px; }
  @media(max-width:480px){
    .picker{ padding:14px; max-height:380px; }
    .picker-grid{ gap:9px; }
    .picker-title{ font-size:16px; }
    .server-option{ padding:12px 11px 11px; min-height:70px; border-radius:13px; }
    .server-option .opt-name{ font-size:12.5px; }
    .server-option .opt-meta{ font-size:10.5px; }
  }

  .steps-box{ background: rgba(255,58,80,0.05); border:1px solid rgba(255,58,80,0.22); border-radius: var(--radius-md); padding:18px 20px 20px; margin-bottom:20px; }
  .steps-label{ display:flex; align-items:center; gap:8px; color: var(--red); font-weight:700; font-size:12.5px; letter-spacing:0.5px; text-transform:uppercase; margin-bottom:16px; }
  .steps-label svg{ width:14px; height:14px; }
  .steps{ display:flex; align-items:flex-start; justify-content:space-between; }
  .step{ display:flex; flex-direction:column; align-items:center; gap:8px; flex:0 0 auto; }
  .step-num{ width:30px; height:30px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-family:'Orbitron', sans-serif; font-weight:700; font-size:12px; border:1px solid var(--red); color: var(--silver-bright); background: rgba(255,58,80,0.14); box-shadow: 0 0 12px var(--red-glow); flex-shrink:0; transition: background .25s ease, transform .2s ease; }
  .step-num.done{ background: linear-gradient(135deg, var(--red), var(--ember)); border-color: transparent; color:#fff; transform: scale(1.03); }
  .step-label{ font-size:11px; color: var(--silver-dim); font-weight:600; text-align:center; white-space:nowrap; }
  .step-connector{ height:1px; flex:1 1 auto; margin-top:15px; background: linear-gradient(90deg, var(--red), var(--ember) 60%, rgba(255,143,90,0.12)); position:relative; overflow:hidden; min-width:20px; }
  .step-connector::after{ content:""; position:absolute; top:0; left:-40%; height:100%; width:40%; background: linear-gradient(90deg, transparent, rgba(255,255,255,0.9), transparent); animation: travel 2.6s linear infinite; }
  @keyframes travel{ to{ left:120%; } }

  label.field-label{ display:block; font-size:12px; font-weight:700; color: var(--silver-dim); text-transform:uppercase; letter-spacing:0.5px; margin-bottom:8px; }
  .phone-input{ display:flex; align-items:center; gap:12px; background: var(--input-bg); border:1px solid var(--input-border); border-radius: var(--radius-md); padding:14px 16px; margin-bottom:20px; transition: border-color .2s ease, box-shadow .2s ease; }
  .phone-input:focus-within{ border-color: var(--red); box-shadow: 0 0 0 3px rgba(255,58,80,0.15), 0 0 20px -6px var(--red-glow); }
  .phone-input svg{ width:17px; height:17px; color: var(--red); flex-shrink:0; }
  .phone-input input{ background:none; border:none; outline:none; color: var(--silver-bright); font-family:'Sora', sans-serif; font-weight:600; font-size:15px; width:100%; letter-spacing:0.3px; }
  .phone-input input::placeholder{ color: var(--code-muted); }

  .generate-btn{
    width:100%; border:none; border-radius: var(--radius-md); padding:16px;
    font-family:'Sora', sans-serif; font-weight:700; font-size:15px; letter-spacing:0.3px; color:#fff;
    cursor:pointer; position:relative; overflow:hidden; display:flex; align-items:center; justify-content:center; gap:10px;
    background: linear-gradient(90deg, var(--red-dim), var(--red) 45%, var(--ember));
    background-size:220% 100%;
    box-shadow: 0 10px 32px -10px var(--red-glow);
    transition: transform .15s ease, box-shadow .2s ease, background-position .3s ease;
    margin-bottom:20px;
  }
  html[data-theme="light"] .generate-btn{ background: linear-gradient(90deg, var(--red), var(--red-2) 50%, var(--ember)); background-size:220% 100%; }
  .generate-btn::after{
    content:""; position:absolute; top:0; left:-60%; width:38%; height:100%; pointer-events:none;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent); transform: skewX(-20deg);
    animation: btnShine 4s ease-in-out infinite;
  }
  @keyframes btnShine{ 0%,55%{ left:-60%; } 100%{ left:140%; } }
  .generate-btn:hover{ transform: translateY(-1px); box-shadow: 0 14px 40px -8px var(--red-glow); background-position: 100% 0; }
  .generate-btn:active{ transform: translateY(0) scale(0.99); }
  .generate-btn svg{ width:17px; height:17px; }
  .generate-btn.loading{ pointer-events:none; opacity:0.88; }
  .spinner{ width:16px; height:16px; border-radius:50%; border:2px solid rgba(255,255,255,0.35); border-top-color:#fff; animation: rotate 0.7s linear infinite; display:none; }
  .generate-btn.loading .spinner{ display:inline-block; }
  .generate-btn.loading .btn-icon{ display:none; }
  @keyframes rotate{ to{ transform: rotate(360deg); } }

  .manual-note{ display:flex; gap:12px; background: var(--input-bg); border:1px solid var(--input-border); border-radius: var(--radius-md); padding:14px 16px; margin-bottom:18px; }
  .manual-note svg{ width:17px; height:17px; color: var(--red); flex-shrink:0; margin-top:2px; }
  .manual-note-title{ color: var(--red); font-weight:700; font-size:13px; margin-bottom:4px; }
  .manual-note-text{ color: var(--silver-dim); font-size:12px; line-height:1.5; }

  .code-box{
    display:flex; align-items:center; justify-content:center; gap:10px;
    text-align:center; background: var(--input-bg); border:1.5px dashed var(--card-border-accent); border-radius: var(--radius-md);
    padding:22px; margin-bottom:14px; font-family:'JetBrains Mono', monospace; letter-spacing:5px; font-size:20px; font-weight:700;
    color: var(--silver-bright); transition: color .3s ease, border-color .3s ease, background .3s ease; min-height:24px;
    word-break: break-all;
  }
  .code-box .empty-state{
    display:flex; align-items:center; gap:8px; font-family:'Sora', sans-serif;
    letter-spacing:normal; font-size:13px; font-weight:500; color: var(--silver-dim);
  }
  .code-box .empty-state svg{ width:16px; height:16px; color: var(--red); opacity:0.8; animation: softPulse 2.4s ease-in-out infinite; }
  @keyframes softPulse{ 0%,100%{ opacity:0.5; } 50%{ opacity:1; } }
  .code-box.active{ border-style:solid; border-color: var(--red); background: rgba(255,58,80,0.07); box-shadow: 0 0 32px -10px var(--red-glow), inset 0 0 24px -14px var(--red-glow); }

  .copy-btn{
    display:none; margin: 0 auto 24px; background:none; border:1px solid var(--input-border); color:var(--silver-dim);
    font-family:'Sora', sans-serif; font-weight:600; font-size:12px; padding:8px 16px; border-radius:20px;
    cursor:pointer; align-items:center; gap:6px; transition: all .2s ease;
  }
  .copy-btn.show{ display:flex; }
  .copy-btn:hover{ border-color: var(--red); color: var(--red); }
  .copy-btn svg{ width:13px; height:13px; }

  footer{ text-align:center; font-size:11.5px; color: var(--silver-dim); opacity:0.8; padding-top:16px; border-top:1px solid var(--line); }
  footer .accent{ color: var(--red); font-weight:700; }

  .toast{
    position: fixed; bottom: 30px; left: 50%;
    transform: translateX(-50%) translateY(120px);
    background: var(--card-bg); backdrop-filter: blur(16px);
    border: 1px solid var(--card-border-accent);
    color: var(--silver-bright);
    padding: 14px 28px; border-radius: 50px;
    font-family: 'Sora', sans-serif; font-weight:600; font-size:13px;
    box-shadow: var(--shadow-ambient);
    z-index: 200; opacity: 0;
    display:flex; align-items:center; gap:10px;
    transition: all .5s cubic-bezier(0.68, -0.55, 0.27, 1.55);
    pointer-events: none; white-space: nowrap;
  }
  .toast.show{ transform: translateX(-50%) translateY(0); opacity: 1; pointer-events: auto; }
  .toast.error{ border-color: var(--red); }
  .toast svg{ width:15px; height:15px; color: var(--red); flex-shrink:0; }

  @media (prefers-reduced-motion: reduce){
    .card::before, .generate-btn::after, .gauge-fill, .step-connector::after, .bg-glow.right, .gauge-center::before, .bg-beam{ animation:none; }
  }

  @media (max-width: 640px){
    nav{ padding:14px 18px; }
    .brand-name{ font-size:14.5px; }
    .nav-links{ gap:14px; }
    .nav-links a span{ display:none; }
    main{ padding:32px 12px 30px; }
    .card{ padding:30px 20px 24px; border-radius:18px; }
    h1.title{ font-size:20px; }
    .subtitle{ font-size:12.5px; margin-bottom:24px; }
    .gauge{ width:82px; height:82px; }
    .gauge-center-inner{ width:46px; height:46px; }
    .field-row{ padding:12px 14px; }
    .field-title{ font-size:13px; }
    select.server-select{ font-size:12px; padding:7px 24px 7px 10px; }
    .steps-box{ padding:16px 12px 18px; }
    .step-label{ font-size:9.5px; }
    .step-num{ width:26px; height:26px; font-size:11px; }
    .code-box{ font-size:15px; letter-spacing:2.5px; padding:16px 10px; }
    .generate-btn{ font-size:13px; padding:15px; }
  }

  @media (max-width: 380px){
    .steps{ flex-wrap:nowrap; }
    .step-connector{ min-width:10px; }
  }
</style>
</head>
<body>

<div class="bg-layer">
  <div class="bg-glow top"></div>
  <div class="bg-glow left"></div>
  <div class="bg-glow right"></div>
  <div class="bg-beam"></div>
  <div class="bg-lines"></div>
  <div class="bg-grain"></div>
</div>
<canvas id="neonWaves"></canvas>

<!-- ===== NAVBAR WITH ADMIN LINK ===== -->
<nav>
  <div class="brand">
    <div class="brand-mark">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4" stroke-linecap="round" stroke-linejoin="round"><path d="M13 2 3 14h7l-1 8 10-12h-7l1-8z"/></svg>
    </div>
    <div class="brand-name">SILVER<span>-Tech</span></div>
  </div>
  <div class="nav-right">
    <div class="nav-links">
      <a href="#" class="active"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 10.5 12 3l9 7.5"/><path d="M5 9.5V21h14V9.5"/></svg><span>Home</span></a>
      <!-- ✅ ADMIN LINK ADDED ✅ -->
      <a href="/admin" target="_blank" rel="noopener"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="4"/><path d="M4 21c0-4 3.6-7 8-7s8 3 8 7"/></svg><span>Admin</span></a>
    </div>
    <button class="theme-toggle" id="themeToggle" aria-label="Toggle light and dark mode">
      <svg class="icon-moon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/></svg>
      <svg class="icon-sun" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="4"/><path d="M12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M4.9 19.1l1.4-1.4M17.7 6.3l1.4-1.4"/></svg>
    </button>
  </div>
</nav>

<main>
  <div class="card">
    <div class="card-light"></div>

    <div class="gauge-wrap">
      <div class="gauge">
        <svg viewBox="0 0 100 100">
          <defs>
            <linearGradient id="gaugeGrad" x1="0" y1="1" x2="1" y2="0">
              <stop offset="0" stop-color="#ff3a50"/><stop offset="1" stop-color="#ff8f5a"/>
            </linearGradient>
          </defs>
          <circle class="gauge-track" cx="50" cy="50" r="42" fill="none" stroke-width="3" stroke-dasharray="198 264" stroke-dashoffset="0" transform="rotate(135 50 50)"/>
          <circle class="gauge-fill" cx="50" cy="50" r="42" fill="none" stroke-width="3" stroke-dasharray="198 264" transform="rotate(135 50 50)"/>
          <g class="gauge-tick">
            <line x1="50" y1="6" x2="50" y2="12" stroke-width="2" transform="rotate(45 50 50)"/>
            <line x1="50" y1="6" x2="50" y2="12" stroke-width="2" transform="rotate(90 50 50)"/>
            <line x1="50" y1="6" x2="50" y2="12" stroke-width="2" transform="rotate(135 50 50)"/>
            <line x1="50" y1="6" x2="50" y2="12" stroke-width="2" transform="rotate(180 50 50)"/>
            <line x1="50" y1="6" x2="50" y2="12" stroke-width="2" transform="rotate(225 50 50)"/>
            <line x1="50" y1="6" x2="50" y2="12" stroke-width="2" transform="rotate(270 50 50)"/>
            <line x1="50" y1="6" x2="50" y2="12" stroke-width="2" transform="rotate(315 50 50)"/>
          </g>
        </svg>
        <div class="gauge-center">
          <div class="gauge-center-inner">
            <svg viewBox="0 0 24 24" role="img" aria-label="WhatsApp"><defs><linearGradient id="waGrad" x1="0" y1="0" x2="1" y2="1"><stop offset="0" stop-color="#ff4d64"/><stop offset="1" stop-color="#b30e26"/></linearGradient></defs><path fill="url(#waGrad)" d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg>
          </div>
        </div>
      </div>
    </div>

    <h1 class="title">SILVER<span>-Tech</span></h1>
    <p class="subtitle">Generate your WhatsApp pairing code</p>

    <div class="field-row" id="serverSummary">
      <div class="field-left">
        <div class="field-icon">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="4" width="18" height="6" rx="1.5"/><rect x="3" y="14" width="18" height="6" rx="1.5"/><circle cx="7" cy="7" r="0.6" fill="currentColor"/><circle cx="7" cy="17" r="0.6" fill="currentColor"/></svg>
        </div>
        <div>
          <div class="field-title" id="summaryName">Select a server</div>
          <div class="field-sub"><span class="status-dot offline" id="statusDot"></span><span id="summaryStatus">Offline</span> · <span id="summaryLoad">Load 0/50</span></div>
        </div>
      </div>
      <button type="button" class="server-select-btn" id="togglePicker">
        Select <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M6 9l6 6 6-6"/></svg>
      </button>
    </div>

    <div class="picker" id="picker">
      <div class="picker-title">Choose a server</div>
      <div class="picker-grid" id="pickerGrid">
        <div class="picker-empty">Loading servers…</div>
      </div>
    </div>

    <div class="steps-box">
      <div class="steps-label">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 8v4l3 2"/></svg>
        Quick setup
      </div>
      <div class="steps">
        <div class="step"><div class="step-num done">1</div><div class="step-label">Select Server</div></div>
        <div class="step-connector"></div>
        <div class="step"><div class="step-num" id="stepNum2">2</div><div class="step-label">Enter Number</div></div>
        <div class="step-connector"></div>
        <div class="step"><div class="step-num" id="stepNum3">3</div><div class="step-label">Get Code</div></div>
      </div>
    </div>

    <label class="field-label">WhatsApp Number</label>
    <div class="phone-input">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-6-6 19.79 19.79 0 0 1-3.07-8.67A2 2 0 0 1 4.11 2h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22 16.92z"/></svg>
      <input type="tel" id="phoneInput" placeholder="923001234567">
    </div>

    <button class="generate-btn" id="generateBtn">
      <svg class="btn-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2"><circle cx="7.5" cy="15.5" r="5.5"/><path d="M21 2l-9.6 9.6"/><path d="M15.5 7.5 19 11"/></svg>
      <span class="spinner"></span>
      <span id="btnLabel">Generate Pairing Code</span>
    </button>

    <div class="manual-note">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><path d="M12 8h.01M11 12h1v4h1"/></svg>
      <div>
        <div class="manual-note-title">Enter code manually in WhatsApp</div>
        <div class="manual-note-text">A notification will not be sent. Open WhatsApp on your phone → Linked Devices → Link a Device → Enter the code shown below manually.</div>
      </div>
    </div>

    <div class="code-box" id="codeBox">
      <span class="empty-state">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="4" y="11" width="16" height="9" rx="2"/><path d="M8 11V7a4 4 0 0 1 8 0v4"/></svg>
        Waiting to generate your code
      </span>
    </div>
    <button class="copy-btn" id="copyBtn">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="9" y="9" width="12" height="12" rx="2"/><path d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"/></svg>
      Copy code
    </button>

    <footer>© 2026 <span class="accent">HENRY 🚩</span> — powered by HENRY 🚩</footer>
  </div>
</main>

<div class="toast" id="toast">
  <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6 9 17l-5-5"/></svg>
  <span>Done</span>
</div>

<script>
  const root = document.documentElement;
  const toggle = document.getElementById('themeToggle');
  root.setAttribute('data-theme', 'dark');

  toggle.addEventListener('click', ()=>{
    const next = root.getAttribute('data-theme') === 'dark' ? 'light' : 'dark';
    root.setAttribute('data-theme', next);
  });

  const btn = document.getElementById('generateBtn');
  const btnLabel = document.getElementById('btnLabel');
  const codeBox = document.getElementById('codeBox');
  const phoneInput = document.getElementById('phoneInput');
  const copyBtn = document.getElementById('copyBtn');
  const stepNum2 = document.getElementById('stepNum2');
  const stepNum3 = document.getElementById('stepNum3');
  const serverSummary = document.getElementById('serverSummary');
  const togglePicker = document.getElementById('togglePicker');
  const picker = document.getElementById('picker');
  const pickerGrid = document.getElementById('pickerGrid');
  const summaryName = document.getElementById('summaryName');
  const summaryStatus = document.getElementById('summaryStatus');
  const summaryLoad = document.getElementById('summaryLoad');
  const statusDot = document.getElementById('statusDot');
  const toast = document.getElementById('toast');

  function showToast(message, isError = false){
    const icon = toast.querySelector('svg');
    const span = toast.querySelector('span');
    icon.innerHTML = isError
      ? '<circle cx="12" cy="12" r="10"/><path d="M12 8v4M12 16h.01"/>'
      : '<path d="M20 6 9 17l-5-5"/>';
    span.textContent = message;
    toast.className = `toast ${isError ? 'error' : ''} show`;
    clearTimeout(toast._timer);
    toast._timer = setTimeout(()=> toast.classList.remove('show'), 2600);
  }

  // ===== REAL SERVER STATE (no fake data) =====
  let servers = [];
  let selectedServerId = null;
  let currentCode = null;

  function updateSummary(){
    const s = servers.find(x => x.id === selectedServerId);
    if(!s){
      summaryName.textContent = 'Select a server';
      summaryStatus.textContent = 'Offline';
      statusDot.classList.add('offline');
      summaryLoad.textContent = 'Load 0/50';
      return;
    }
    const isOnline = s.healthy !== false && s.active !== false;
    const count = s.count ?? s.lastKnownCount ?? 0;
    const limit = s.limit ?? s.lastKnownLimit ?? 50;
    summaryName.textContent = s.name || s.id;
    summaryStatus.textContent = isOnline ? 'Online' : 'Offline';
    statusDot.classList.toggle('offline', !isOnline);
    summaryLoad.textContent = `Load ${count}/${limit}`;
  }

  function renderPicker(){
    if(!servers.length){
      pickerGrid.innerHTML = '<div class="picker-empty">No servers available</div>';
      return;
    }
    pickerGrid.innerHTML = '';
    servers.forEach(s => {
      const isOnline = s.healthy !== false && s.active !== false;
      const count = s.count ?? s.lastKnownCount ?? 0;
      const limit = s.limit ?? s.lastKnownLimit ?? 50;
      const isSelected = selectedServerId === s.id;

      const opt = document.createElement('button');
      opt.type = 'button';
      opt.className = `server-option ${isSelected ? 'selected' : ''}`;
      const pct = Math.max(0, Math.min(100, limit ? Math.round((count / limit) * 100) : 0));
      opt.innerHTML = `
        <span class="opt-row">
          <span class="opt-name">${s.name || s.id}</span>
          <span class="opt-tail">
            ${isSelected ? '<span class="opt-check" aria-hidden="true">✓</span>' : ''}
            <span class="opt-dot ${isOnline ? 'online' : ''}"></span>
          </span>
        </span>
        <span class="opt-meta">${count}/${limit} sessions · ${isOnline ? 'online' : 'offline'}</span>
        <span class="opt-bar"><i style="width:${pct}%"></i></span>
      `;
      opt.addEventListener('click', () => {
        selectedServerId = s.id;
        renderPicker();
        updateSummary();
        closePicker();
      });
      pickerGrid.appendChild(opt);
    });
  }

  function openPicker(){
    picker.classList.add('show');
    togglePicker.classList.add('open');
  }
  function closePicker(){
    picker.classList.remove('show');
    togglePicker.classList.remove('open');
  }
  function togglePickerState(){
    picker.classList.contains('show') ? closePicker() : openPicker();
  }

  togglePicker.addEventListener('click', (e) => {
    e.stopPropagation();
    togglePickerState();
  });

  serverSummary.addEventListener('click', () => {
    togglePickerState();
  });

  document.addEventListener('click', (e) => {
    if (!picker.contains(e.target) && e.target !== togglePicker && !togglePicker.contains(e.target) && e.target !== serverSummary && !serverSummary.contains(e.target)) {
      closePicker();
    }
  });

  async function loadServers(){
    try {
      const res = await axios.get('/api/servers?live=true');
      servers = res.data.servers || [];
    } catch (_) {
      try {
        const res2 = await axios.get('/api/servers');
        servers = res2.data.servers || [];
      } catch (__) {
        servers = [];
      }
    }
    renderPicker();
    updateSummary();
  }

  phoneInput.addEventListener('input', ()=>{
    phoneInput.value = phoneInput.value.replace(/[^\d]/g, '');
    if(phoneInput.value.trim()){ stepNum2.classList.add('done'); }
    else{ stepNum2.classList.remove('done'); }
  });

  // Generate — real API call, no fake/random code
  btn.addEventListener('click', async (e) => {
    if(!selectedServerId){
      // stop this click from reaching the document handler (it would close the picker again)
      e.stopPropagation();
      codeBox.innerHTML = '<span class="empty-state">Select a server first</span>';
      codeBox.classList.remove('active');
      showToast('Select a server first', true);
      openPicker();
      serverSummary.classList.remove('attention');
      void serverSummary.offsetWidth;
      serverSummary.classList.add('attention');
      setTimeout(() => serverSummary.classList.remove('attention'), 700);
      setTimeout(() => serverSummary.scrollIntoView({ behavior: 'smooth', block: 'start' }), 60);
      return;
    }

    const raw = phoneInput.value.replace(/[^\d]/g, '');
    if(raw.length < 10){
      phoneInput.focus();
      phoneInput.parentElement.style.borderColor = '#ff3a50';
      codeBox.innerHTML = '<span class="empty-state">Enter a valid WhatsApp number</span>';
      codeBox.classList.remove('active');
      showToast('Invalid phone number', true);
      return;
    }

    btn.classList.add('loading');
    btn.disabled = true;
    btnLabel.textContent = 'Generating...';
    codeBox.innerHTML = '<span class="empty-state"><svg style="animation:rotate 0.9s linear infinite" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M21 12a9 9 0 1 1-6.219-8.56"/></svg>Generating your code…</span>';
    codeBox.classList.remove('active');
    copyBtn.classList.remove('show');

    try {
      const res = await axios.get('/api/code', {
        params: { server: selectedServerId, number: raw, ref: new URLSearchParams(location.search).get('ref') || undefined }
      });
      if(res.data.code){
        currentCode = res.data.code;
        codeBox.textContent = currentCode;
        codeBox.classList.add('active');
        copyBtn.classList.add('show');
        stepNum3.classList.add('done');
        showToast('Pairing code generated');
        loadServers();
        setTimeout(()=>{
          const rect = codeBox.getBoundingClientRect();
          const fullyVisible = rect.top >= 0 && rect.bottom <= window.innerHeight;
          if(!fullyVisible){
            codeBox.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
          }
        }, 100);
      } else {
        throw new Error(res.data.error || 'Failed to generate code');
      }
    } catch (err) {
      const msg = err.response?.data?.message || err.response?.data?.error || err.message || 'Request failed';
      codeBox.innerHTML = `<span class="empty-state">${msg}</span>`;
      codeBox.classList.remove('active');
      showToast('Request failed', true);
    } finally {
      btn.classList.remove('loading');
      btn.disabled = false;
      btnLabel.textContent = 'Generate Pairing Code';
    }
  });

  copyBtn.addEventListener('click', async ()=>{
    if(!currentCode) return;
    try{
      await navigator.clipboard.writeText(currentCode);
      const original = copyBtn.innerHTML;
      copyBtn.innerHTML = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 6 9 17l-5-5"/></svg> Copied';
      showToast('Code copied to clipboard');
      setTimeout(()=>{ copyBtn.innerHTML = original; }, 1500);
    } catch(_){
      showToast('Copy failed', true);
    }
  });

  // cursor-following light on the card
  const cardEl = document.querySelector('.card');
  cardEl.addEventListener('pointermove', (e) => {
    const r = cardEl.getBoundingClientRect();
    cardEl.style.setProperty('--mx', (e.clientX - r.left) + 'px');
    cardEl.style.setProperty('--my', (e.clientY - r.top) + 'px');
  });

  // ===== INIT =====
  loadServers();
  setInterval(loadServers, 20000);

  // ---- distinctive "voltage" background: drifting energy sparks + occasional lightning flashes ----
  const canvas = document.getElementById('neonWaves');
  const ctx = canvas.getContext('2d');
  let w, h, sparks = [], bolts = [], pulses = [];

  function isLight(){ return root.getAttribute('data-theme') === 'light'; }
  function isMobile(){ return w < 640; }

  function makeSpark(){
    return {
      x: Math.random()*w,
      y: h + Math.random()*100,
      r: 1.2 + Math.random()*2.4,
      speed: 16 + Math.random()*30,
      drift: (Math.random()-0.5)*14,
      flicker: Math.random()*Math.PI*2,
      hue: Math.random() > 0.4 ? 'red' : 'silver'
    };
  }

  function buildSparks(){
    sparks = [];
    const density = isMobile() ? 34000 : 24000;
    const count = Math.max(isMobile() ? 16 : 28, Math.round((w*h)/density));
    for(let i=0;i<count;i++){
      const s = makeSpark();
      s.y = Math.random()*h;
      sparks.push(s);
    }
  }

  function resizeNeon(){
    w = canvas.width = window.innerWidth;
    h = canvas.height = window.innerHeight;
    buildSparks();
  }
  window.addEventListener('resize', resizeNeon);
  resizeNeon();

  // jagged lightning bolt generator
  function buildBoltPath(x1, y1, x2, y2, displaceScale){
    const points = [{x:x1,y:y1}];
    function subdivide(p1, p2, scale){
      if(Math.hypot(p2.x-p1.x, p2.y-p1.y) < 22){ points.push(p2); return; }
      const mx = (p1.x+p2.x)/2 + (Math.random()-0.5)*scale;
      const my = (p1.y+p2.y)/2 + (Math.random()-0.5)*scale*0.4;
      subdivide(p1, {x:mx,y:my}, scale*0.62);
      subdivide({x:mx,y:my}, p2, scale*0.62);
    }
    subdivide({x:x1,y:y1}, {x:x2,y:y2}, displaceScale);
    return points;
  }

  function spawnBolt(){
    const x1 = Math.random()*w;
    const y1 = -20;
    const x2 = x1 + (Math.random()-0.5)*260;
    const y2 = h*(0.35+Math.random()*0.5);
    const path = buildBoltPath(x1,y1,x2,y2, 90);
    bolts.push({ path, life: 1, branches: Math.random() > 0.5 });
  }

  // expanding "voltage pulse" rings — a second signature motif, distinct from sparks/bolts
  function spawnPulse(){
    const margin = isMobile() ? 40 : 100;
    pulses.push({
      x: margin + Math.random()*(w - margin*2),
      y: margin + Math.random()*(h - margin*2),
      r: 4,
      maxR: (isMobile() ? 90 : 150) + Math.random()*80,
      life: 1,
      hue: Math.random() > 0.5 ? 'red' : 'silver'
    });
  }

  let lastBoltTime = 0;
  let lastPulseTime = 0;
  let lastTime = performance.now();

  function drawBolt(points, alpha, colorRgb, width){
    ctx.beginPath();
    points.forEach((p,i)=>{ i===0 ? ctx.moveTo(p.x,p.y) : ctx.lineTo(p.x,p.y); });
    ctx.strokeStyle = `rgba(${colorRgb},${alpha})`;
    ctx.lineWidth = width;
    ctx.lineJoin = 'round';
    ctx.lineCap = 'round';
    ctx.shadowColor = `rgba(${colorRgb},${Math.min(alpha*1.4,1)})`;
    ctx.shadowBlur = 18;
    ctx.stroke();
    ctx.shadowBlur = 0;
  }

  function drawNeon(now){
    const dt = Math.min((now - lastTime) / 1000, 0.05);
    lastTime = now;

    ctx.clearRect(0, 0, w, h);

    // ambient drifting sparks
    const light = isLight();
    sparks.forEach(s => {
      s.y -= s.speed * dt;
      s.x += Math.sin((now/1000) + s.flicker) * 0.3;
      if(s.y < -20){ Object.assign(s, makeSpark()); s.y = h + 10; }
      const flick = 0.55 + Math.sin(now/240 + s.flicker) * 0.45;
      const color = s.hue === 'red' ? '255,64,84' : '205,213,224';
      const baseAlpha = (light ? 0.35 : 0.75) * flick;
      const grad = ctx.createRadialGradient(s.x, s.y, 0, s.x, s.y, s.r*4);
      grad.addColorStop(0, `rgba(${color},${baseAlpha})`);
      grad.addColorStop(1, `rgba(${color},0)`);
      ctx.fillStyle = grad;
      ctx.beginPath();
      ctx.arc(s.x, s.y, s.r*4, 0, Math.PI*2);
      ctx.fill();
      ctx.fillStyle = `rgba(${color},${Math.min(baseAlpha*1.3,1)})`;
      ctx.beginPath();
      ctx.arc(s.x, s.y, s.r, 0, Math.PI*2);
      ctx.fill();
    });

    // random lightning flashes
    if(now - lastBoltTime > 2600 + Math.random()*2600){
      spawnBolt();
      lastBoltTime = now;
    }

    bolts.forEach(b => {
      b.life -= dt * 1.8;
      const a = Math.max(b.life, 0);
      const boltColor = light ? '224,25,50' : '255,70,90';
      drawBolt(b.path, a*0.9, boltColor, 2);
      drawBolt(b.path, a*0.35, boltColor, 5);
    });
    bolts = bolts.filter(b => b.life > 0);

    // expanding voltage pulse rings
    if(now - lastPulseTime > 1800 + Math.random()*1600){
      spawnPulse();
      lastPulseTime = now;
    }

    pulses.forEach(p => {
      p.life -= dt * 0.55;
      p.r = p.maxR * (1 - Math.max(p.life,0));
      const a = Math.max(p.life, 0) * (light ? 0.5 : 0.85);
      const color = p.hue === 'red' ? '255,64,84' : '210,216,226';
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI*2);
      ctx.strokeStyle = `rgba(${color},${a})`;
      ctx.lineWidth = 1.6;
      ctx.shadowColor = `rgba(${color},${Math.min(a*1.2,1)})`;
      ctx.shadowBlur = 14;
      ctx.stroke();
      ctx.shadowBlur = 0;

      ctx.beginPath();
      ctx.arc(p.x, p.y, 2.4, 0, Math.PI*2);
      ctx.fillStyle = `rgba(${color},${Math.min(a*1.4,1)})`;
      ctx.fill();
    });
    pulses = pulses.filter(p => p.life > 0);

    requestAnimationFrame(drawNeon);
  }
  requestAnimationFrame(drawNeon);
</script>

</body>
</html>
