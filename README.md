<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no, viewport-fit=cover">
<meta name="theme-color" content="#07060f">
<title>NEON DISTRICT — نيون ديستريكت</title>
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3E%3Crect width='16' height='16' fill='%2307060f'/%3E%3Crect x='3' y='3' width='4' height='10' fill='%23ff2e88'/%3E%3Crect x='9' y='6' width='4' height='7' fill='%2322d3ee'/%3E%3C/svg%3E">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700;900&family=Press+Start+2P&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#07060f; --bg2:#0d0a1c; --surface:#120e26; --overlay:#0b0819;
  --pink:#ff2e88; --magenta:#c026d3; --violet:#7c3aed; --cyan:#22d3ee;
  --blue:#3b82f6; --amber:#ffb020; --red:#ff3b3b; --green:#34d399;
  --ink:#f4f1ff; --ink-dim:#9b93c9; --cool:#1a1533; --warm:#241a2e;
  --reserve:132px;
  --px:'Press Start 2P',monospace; --ar:'Cairo',system-ui,sans-serif;
}
@supports not (height:100dvh){ :root{ --vh:1vh } }
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent}
html,body{height:100%;overflow:hidden}
body{
  background:var(--bg); color:var(--ink); font-family:var(--ar);
  user-select:none;-webkit-user-select:none; touch-action:manipulation;
  background-image:
    radial-gradient(1200px 500px at 85% -10%, rgba(192,38,211,.14), transparent 60%),
    radial-gradient(900px 420px at 8% 110%, rgba(34,211,238,.10), transparent 60%),
    linear-gradient(180deg,#07060f 0%,#0b0718 55%,#07060f 100%);
}
#app{height:100dvh;height:100vh;display:flex;flex-direction:column}
@supports (height:100dvh){ #app{height:100dvh} }

/* ---------- TOP BAR ---------- */
#topbar{
  flex:0 0 auto; display:flex; align-items:center; gap:10px;
  padding:8px 12px; padding-top:calc(8px + env(safe-area-inset-top));
  background:linear-gradient(180deg,rgba(18,14,38,.92),rgba(18,14,38,.55));
  border-bottom:1px solid rgba(255,46,136,.28);
  box-shadow:0 1px 0 rgba(34,211,238,.12), 0 8px 24px rgba(0,0,0,.45);
  position:relative; z-index:40;
}
#logo{font-family:var(--px);font-size:10px;letter-spacing:1px;color:var(--pink);
  text-shadow:0 0 8px rgba(255,46,136,.8),2px 2px 0 rgba(34,211,238,.55);white-space:nowrap}
#logo b{color:var(--cyan);text-shadow:0 0 8px rgba(34,211,238,.8)}
#sectorTag{font-size:12px;font-weight:700;color:var(--ink-dim);flex:1;text-align:center;
  overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
#sectorTag em{font-style:normal;color:var(--amber)}
.tbtn{
  font-family:var(--ar);font-weight:700;font-size:13px;color:var(--ink);
  background:rgba(124,58,237,.14);border:1px solid rgba(124,58,237,.55);
  border-radius:9px;padding:6px 10px;cursor:pointer;display:flex;align-items:center;gap:6px;
  transition:box-shadow .15s,transform .1s,background .15s;min-height:34px;
}
.tbtn:hover{background:rgba(124,58,237,.3);box-shadow:0 0 14px rgba(124,58,237,.5)}
.tbtn:active{transform:translateY(1px) scale(.97)}
.tbtn .ic{font-size:15px;line-height:1}
#btnSound[data-muted="1"]{border-color:rgba(255,59,59,.6);background:rgba(255,59,59,.12);color:#ffb4b4}
#btnSound[data-muted="0"]{border-color:rgba(52,211,153,.55);background:rgba(52,211,153,.1)}
#btnPause:disabled{opacity:.35;cursor:default;box-shadow:none}

/* ---------- STAGE / FRAME ---------- */
#stage{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:10px;
  padding:8px;min-height:0;position:relative}
#frame{
  position:relative; width:min(100%, calc((100dvh - var(--reserve)) * 16 / 9));
  aspect-ratio:16/9; border-radius:10px; overflow:hidden;
  border:1px solid rgba(255,46,136,.35);
  box-shadow:0 0 0 1px rgba(34,211,238,.15), 0 0 34px rgba(192,38,211,.28),
    0 24px 60px rgba(0,0,0,.6), inset 0 0 60px rgba(0,0,0,.5);
  background:#05030c;
}
@supports not (height:100dvh){ #frame{width:min(100%, calc((100vh - var(--reserve)) * 16 / 9))} }
#game{width:100%;height:100%;display:block;image-rendering:pixelated;image-rendering:crisp-edges}
#frame::after{ /* scanlines + vignette */
  content:"";position:absolute;inset:0;pointer-events:none;z-index:5;
  background:
    repeating-linear-gradient(0deg, rgba(0,0,0,.14) 0 1px, transparent 1px 3px),
    radial-gradient(120% 90% at 50% 45%, transparent 55%, rgba(2,0,10,.55) 100%);
  mix-blend-mode:multiply;
}
#frame::before{ /* chromatic edge glow */
  content:"";position:absolute;inset:0;pointer-events:none;z-index:6;
  box-shadow:inset 2px 0 0 rgba(255,46,136,.12), inset -2px 0 0 rgba(34,211,238,.12);
}

/* ---------- HUD ---------- */
#hud{position:absolute;inset:0;pointer-events:none;z-index:10;display:none;font-family:var(--ar)}
body[data-state="playing"] #hud, body[data-state="paused"] #hud{display:block}
.hudTL{position:absolute;top:8px;right:8px;display:flex;flex-direction:column;gap:5px;width:150px}
.bar{height:11px;background:rgba(7,6,15,.75);border:1px solid rgba(155,147,201,.4);
  border-radius:3px;overflow:hidden;position:relative;direction:ltr}
.bar i{position:absolute;inset:0;transform-origin:left center;transition:transform .12s linear;display:block}
#hpFill{background:linear-gradient(90deg,#ff3b3b,#ff2e88 60%,#ff7ab0);box-shadow:0 0 10px rgba(255,46,136,.7)}
#enFill{background:linear-gradient(90deg,#0ea5b7,#22d3ee 60%,#a5f3fc);box-shadow:0 0 10px rgba(34,211,238,.7)}
.barLbl{position:absolute;inset:0;display:flex;align-items:center;justify-content:space-between;
  padding:0 5px;font-size:8px;font-weight:900;color:#fff;text-shadow:0 1px 2px #000;direction:rtl}
.bar.deny{animation:deny .3s}
@keyframes deny{0%,100%{transform:translateX(0)}25%{transform:translateX(-3px)}75%{transform:translateX(3px)}}
.hudTR{position:absolute;top:8px;left:8px;text-align:left;direction:ltr}
#scoreVal{font-family:var(--px);font-size:13px;color:var(--amber);text-shadow:0 0 10px rgba(255,176,32,.8),2px 2px 0 #000}
#comboWrap{margin-top:5px;display:none}
#comboWrap.on{display:block}
#comboVal{font-family:var(--px);font-size:10px;color:var(--pink);text-shadow:0 0 8px rgba(255,46,136,.9)}
#comboBar{width:70px;height:4px;background:rgba(7,6,15,.7);border-radius:2px;margin-top:3px;overflow:hidden}
#comboBar i{display:block;height:100%;background:var(--pink);box-shadow:0 0 6px var(--pink)}
#objWrap{position:absolute;top:8px;left:50%;transform:translateX(-50%);text-align:center;
  background:rgba(7,6,15,.6);border:1px solid rgba(34,211,238,.35);border-radius:8px;padding:3px 12px;
  font-size:11px;font-weight:700;color:var(--cyan);text-shadow:0 0 6px rgba(34,211,238,.6);white-space:nowrap}
#bossWrap{position:absolute;top:34px;left:50%;transform:translateX(-50%);width:62%;display:none}
#bossWrap.on{display:block}
#bossName{font-family:var(--px);font-size:8px;color:var(--red);text-align:center;margin-bottom:3px;
  text-shadow:0 0 8px rgba(255,59,59,.8)}
#bossBar{height:9px;background:rgba(7,6,15,.8);border:1px solid rgba(255,59,59,.6);border-radius:3px;overflow:hidden;direction:ltr}
#bossFill{display:block;height:100%;background:linear-gradient(90deg,#7c3aed,#ff2e88,#ff3b3b);
  box-shadow:0 0 12px rgba(255,46,136,.8);transition:width .15s}
#prog{position:absolute;bottom:6px;left:8px;right:8px;height:4px;background:rgba(7,6,15,.7);
  border-radius:2px;direction:ltr;border:1px solid rgba(155,147,201,.25)}
#progFill{display:block;height:100%;width:0;background:linear-gradient(90deg,var(--violet),var(--cyan));border-radius:2px}
#progMark{position:absolute;top:-4px;width:3px;height:10px;background:#fff;box-shadow:0 0 6px var(--cyan);border-radius:1px}
#progGate{position:absolute;top:-3px;right:-1px;width:5px;height:8px;background:var(--green);box-shadow:0 0 6px var(--green);border-radius:1px}

/* ---------- TOUCH ---------- */
#touch{position:absolute;left:0;right:0;bottom:0;z-index:20;display:none;
  justify-content:space-between;align-items:flex-end;padding:12px;direction:ltr;pointer-events:none}
body[data-state="playing"] #touch{display:flex}
body:not(.coarse) #touch{display:none!important}
.tgroup{display:flex;gap:12px;pointer-events:auto}
.tc{
  width:62px;height:62px;border-radius:50%;border:2px solid;display:flex;flex-direction:column;
  align-items:center;justify-content:center;gap:1px;font-family:var(--ar);font-weight:900;font-size:9px;
  color:var(--ink);background:rgba(7,6,15,.55);backdrop-filter:blur(2px);
  transition:transform .08s,box-shadow .12s,background .12s;touch-action:none;
}
.tc .ti{font-size:20px;line-height:1}
.tc:active,.tc.held{transform:scale(.92)}
.tc.move{border-color:rgba(155,147,201,.7)} .tc.move.held{background:rgba(155,147,201,.3);box-shadow:0 0 16px rgba(155,147,201,.6)}
.tc.jump{border-color:rgba(52,211,153,.8)} .tc.jump.held{background:rgba(52,211,153,.28);box-shadow:0 0 18px rgba(52,211,153,.7)}
.tc.shoot{border-color:rgba(255,46,136,.85);width:70px;height:70px} .tc.shoot.held{background:rgba(255,46,136,.3);box-shadow:0 0 20px rgba(255,46,136,.8)}
.tc.dash{border-color:rgba(34,211,238,.85)} .tc.dash.held{background:rgba(34,211,238,.28);box-shadow:0 0 18px rgba(34,211,238,.8)}
@media (pointer:coarse) and (orientation:portrait){
  :root{--reserve:236px}
  #touch{position:static;width:100%;max-width:540px;padding:4px 8px calc(8px + env(safe-area-inset-bottom))}
  body[data-state="playing"] #stage{justify-content:flex-start;padding-top:4px}
}
@media (pointer:coarse) and (orientation:landscape){
  :root{--reserve:88px}
  .tc{width:54px;height:54px}.tc.shoot{width:60px;height:60px}
  #topbar{padding:4px 10px}.tbtn{min-height:28px;padding:3px 8px;font-size:11px}
}

/* ---------- HINTS ---------- */
#hints{flex:0 0 auto;text-align:center;font-size:11px;color:var(--ink-dim);padding:4px 8px 8px;direction:ltr}
#hints b{color:var(--cyan);font-weight:700}
body.coarse #hints{display:none}

/* ---------- OVERLAYS ---------- */
.overlay{position:absolute;inset:0;z-index:30;display:grid;place-items:center;padding:14px;
  background:radial-gradient(90% 90% at 50% 40%, rgba(11,8,25,.72), rgba(5,3,12,.9));
  backdrop-filter:blur(2px);overflow:auto;scrollbar-width:thin;scrollbar-color:var(--magenta) var(--cool)}
.overlay::-webkit-scrollbar{width:8px}
.overlay::-webkit-scrollbar-track{background:var(--cool)}
.overlay::-webkit-scrollbar-thumb{background:linear-gradient(var(--pink),var(--violet));border-radius:4px}
.overlay::-webkit-scrollbar-thumb:hover{background:var(--cyan)}
[hidden]{display:none!important}
.panel{width:min(94%,440px);text-align:center;padding:20px 18px;border-radius:14px;position:relative;
  background:linear-gradient(160deg,rgba(24,16,48,.94),rgba(11,8,25,.96));
  border:1px solid rgba(255,46,136,.5);
  box-shadow:0 0 0 1px rgba(34,211,238,.18),0 0 40px rgba(192,38,211,.35),0 20px 50px rgba(0,0,0,.6);
  animation:pop .35s cubic-bezier(.2,1.4,.4,1)}
@keyframes pop{from{transform:scale(.86) translateY(14px);opacity:0}to{transform:none;opacity:1}}
.panel::before{content:"";position:absolute;inset:6px;border:1px dashed rgba(34,211,238,.22);border-radius:9px;pointer-events:none}
.bigLogo{font-family:var(--px);font-size:clamp(17px,5.4vw,27px);line-height:1.5;color:var(--pink);
  text-shadow:0 0 14px rgba(255,46,136,.9),3px 3px 0 rgba(34,211,238,.6),-2px -2px 0 rgba(124,58,237,.5);
  animation:glitch 3.2s infinite}
.bigLogo b{color:var(--cyan);text-shadow:0 0 14px rgba(34,211,238,.9),3px 3px 0 rgba(255,46,136,.55)}
@keyframes glitch{0%,92%,100%{transform:none;filter:none}93%{transform:translateX(2px) skewX(-4deg);filter:hue-rotate(40deg)}95%{transform:translateX(-2px);filter:hue-rotate(-50deg)}97%{transform:none}}
.subAr{font-size:16px;font-weight:900;color:var(--ink);margin-top:8px}
.tagline{font-size:12px;color:var(--ink-dim);margin-top:4px;line-height:1.7}
.pills{display:flex;gap:8px;justify-content:center;margin:14px 0 4px;flex-wrap:wrap}
.pill{font-family:var(--ar);font-weight:700;font-size:12px;padding:6px 14px;border-radius:20px;cursor:pointer;
  border:1px solid rgba(155,147,201,.5);background:rgba(26,21,51,.6);color:var(--ink-dim);transition:all .15s}
.pill:hover{border-color:var(--cyan);color:var(--ink)}
.pill[aria-pressed="true"]{background:linear-gradient(90deg,rgba(255,46,136,.3),rgba(124,58,237,.3));
  border-color:var(--pink);color:#fff;box-shadow:0 0 14px rgba(255,46,136,.5)}
.btn{font-family:var(--ar);font-weight:900;font-size:15px;color:#fff;cursor:pointer;border-radius:11px;
  padding:11px 26px;margin:6px 4px 0;border:1px solid;position:relative;transition:all .15s;min-width:130px}
.btn:active{transform:translateY(2px) scale(.98)}
.btn.primary{background:linear-gradient(90deg,var(--pink),var(--magenta));border-color:#ff7ab0;
  box-shadow:0 0 22px rgba(255,46,136,.6),inset 0 1px 0 rgba(255,255,255,.3)}
.btn.primary:hover{box-shadow:0 0 34px rgba(255,46,136,.9);filter:brightness(1.1)}
.btn.ghost{background:rgba(34,211,238,.08);border-color:rgba(34,211,238,.6);color:var(--cyan)}
.btn.ghost:hover{background:rgba(34,211,238,.2);box-shadow:0 0 18px rgba(34,211,238,.5)}
.btn.warn{background:rgba(255,176,32,.1);border-color:rgba(255,176,32,.6);color:var(--amber)}
.btn.warn:hover{background:rgba(255,176,32,.22);box-shadow:0 0 18px rgba(255,176,32,.5)}
.legend{margin-top:14px;font-size:11px;color:var(--ink-dim);line-height:2;direction:ltr}
.legend b{color:var(--cyan);font-family:var(--px);font-size:8px;background:rgba(34,211,238,.1);
  border:1px solid rgba(34,211,238,.4);border-radius:4px;padding:2px 5px;margin:0 2px}
.bestLine{margin-top:10px;font-size:12px;color:var(--amber)}
.bestLine span{font-family:var(--px);font-size:10px}
.statGrid{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin:14px 0 4px;text-align:right}
.stat{background:rgba(26,21,51,.7);border:1px solid rgba(124,58,237,.4);border-radius:9px;padding:8px 10px}
.stat .k{font-size:10px;color:var(--ink-dim)}
.stat .v{font-family:var(--px);font-size:12px;color:var(--cyan);margin-top:4px;direction:ltr;text-align:right}
.stat.gold .v{color:var(--amber)}
h2.ovTitle{font-family:var(--px);font-size:15px;line-height:1.6;margin-bottom:4px}
h2.ovTitle.green{color:var(--green);text-shadow:0 0 14px rgba(52,211,153,.8)}
h2.ovTitle.red{color:var(--red);text-shadow:0 0 14px rgba(255,59,59,.8);animation:glitch 2.4s infinite}
h2.ovTitle.gold{color:var(--amber);text-shadow:0 0 16px rgba(255,176,32,.9)}
.ovSub{font-size:13px;font-weight:700;color:var(--ink-dim)}
#rankGlyph{font-family:var(--px);font-size:44px;color:var(--pink);margin:8px 0;
  text-shadow:0 0 24px rgba(255,46,136,1),4px 4px 0 rgba(34,211,238,.6)}
#toast{position:absolute;top:52px;left:50%;transform:translateX(-50%) translateY(-8px);z-index:25;
  background:rgba(7,6,15,.85);border:1px solid var(--cyan);border-radius:9px;padding:6px 16px;
  font-size:13px;font-weight:900;color:var(--cyan);text-shadow:0 0 8px rgba(34,211,238,.7);
  opacity:0;pointer-events:none;transition:opacity .25s,transform .25s;white-space:nowrap;max-width:92%;
  overflow:hidden;text-overflow:ellipsis;box-shadow:0 0 20px rgba(34,211,238,.35)}
#toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
#toast.warn{border-color:var(--amber);color:var(--amber);text-shadow:0 0 8px rgba(255,176,32,.7);box-shadow:0 0 20px rgba(255,176,32,.35)}
#toast.bad{border-color:var(--red);color:#ff9b9b;text-shadow:0 0 8px rgba(255,59,59,.7);box-shadow:0 0 20px rgba(255,59,59,.35)}
#toast.big{font-size:15px;padding:10px 22px;border-color:var(--pink);color:#fff;
  text-shadow:0 0 10px rgba(255,46,136,.9);box-shadow:0 0 30px rgba(255,46,136,.5)}
.credit{margin-top:12px;font-size:9px;color:#5e568c;font-family:var(--px);direction:ltr}
noscript{position:fixed;inset:0;display:grid;place-items:center;background:var(--bg);color:var(--ink);font-size:18px;z-index:99}
</style>
</head>
<body data-state="title">
<noscript>فعّل JavaScript لتشغيل اللعبة 🎮</noscript>
<div id="app">
  <header id="topbar">
    <div id="logo">NEON<b>▮</b>DISTRICT</div>
    <div id="sectorTag">نيون ديستريكت <em>// جاهز للمهمة</em></div>
    <button class="tbtn" id="btnSound" data-muted="0" type="button" aria-label="كتم الصوت"><span class="ic" id="sndIc">🔊</span><span id="sndTxt">الصوت</span></button>
    <button class="tbtn" id="btnPause" type="button" disabled aria-label="إيقاف مؤقت"><span class="ic" id="pauseIc">⏸</span></button>
    <button class="tbtn" id="btnFull" type="button" aria-label="ملء الشاشة"><span class="ic">⛶</span></button>
  </header>

  <div id="stage">
    <div id="frame">
      <canvas id="game" width="480" height="270"></canvas>

      <div id="hud">
        <div class="hudTL">
          <div class="bar" id="hpBar"><i id="hpFill"></i><span class="barLbl"><span>الصحة</span><span id="hpNum">100</span></span></div>
          <div class="bar" id="enBar"><i id="enFill"></i><span class="barLbl"><span>الطاقة</span><span id="enNum">100</span></span></div>
        </div>
        <div class="hudTR">
          <div id="scoreVal">0</div>
          <div id="comboWrap"><span id="comboVal">×2</span><div id="comboBar"><i></i></div></div>
        </div>
        <div id="objWrap"><span id="objVal">الأهداف 0/6</span></div>
        <div id="bossWrap"><div id="bossName">WARDEN-09 // الحارس</div><div id="bossBar"><i id="bossFill"></i></div></div>
        <div id="prog"><i id="progFill"></i><span id="progMark"></span><span id="progGate"></span></div>
      </div>

      <div id="touch">
        <div class="tgroup">
          <button class="tc move" data-k="left" type="button"><span class="ti">◀</span>يسار</button>
          <button class="tc move" data-k="right" type="button"><span class="ti">▶</span>يمين</button>
        </div>
        <div class="tgroup">
          <button class="tc dash" data-k="dash" type="button"><span class="ti">⚡</span>اندفاع</button>
          <button class="tc jump" data-k="jump" type="button"><span class="ti">⤒</span>قفز</button>
          <button class="tc shoot" data-k="shoot" type="button"><span class="ti">◎</span>إطلاق</button>
        </div>
      </div>

      <div id="toast"></div>

      <!-- TITLE -->
      <div class="overlay" id="ovTitle">
        <div class="panel">
          <div class="bigLogo">NEON<br><b>DISTRICT</b></div>
          <div class="subAr">نيون ديستريكت — شارع المطر</div>
          <div class="tagline">مدينة تغرق في النيون والمطر… درونات الحراسة تطاردك.<br>دمّرها، اجمع رقائق البيانات، وافتح بوابة كل قطاع للهروب.</div>
          <div class="pills" id="diffPills">
            <button class="pill" data-d="0" type="button" aria-pressed="false">عادي</button>
            <button class="pill" data-d="1" type="button" aria-pressed="true">صعب</button>
            <button class="pill" data-d="2" type="button" aria-pressed="false">كابوس</button>
          </div>
          <button class="btn primary" id="btnStart" type="button">▶ ابدأ المهمة</button>
          <div class="bestLine">أفضل نتيجة: <span id="bestVal">0</span></div>
          <div class="legend" id="legendDesk"><b>←→</b> حركة &nbsp; <b>SPACE</b> قفز مزدوج &nbsp; <b>J</b> إطلاق &nbsp; <b>K</b> اندفاع &nbsp; <b>P</b> إيقاف</div>
          <div class="legend" id="legendTouch" hidden>◀ ▶ حركة &nbsp;•&nbsp; ⤒ قفز مزدوج &nbsp;•&nbsp; ◎ إطلاق (استمر بالضغط) &nbsp;•&nbsp; ⚡ اندفاع</div>
          <div class="credit">v1.0 // RAIN CITY BUILD</div>
        </div>
      </div>

      <!-- PAUSE -->
      <div class="overlay" id="ovPause" hidden>
        <div class="panel">
          <h2 class="ovTitle" style="color:var(--cyan)">PAUSED</h2>
          <div class="ovSub">المهمة متوقفة مؤقتاً</div>
          <div style="margin-top:12px">
            <button class="btn primary" id="btnResume" type="button">▶ استئناف</button><br>
            <button class="btn warn" id="btnRetryP" type="button">↺ إعادة القطاع</button>
            <button class="btn ghost" id="btnMenuP" type="button">القائمة</button>
          </div>
        </div>
      </div>

      <!-- SECTOR CLEAR -->
      <div class="overlay" id="ovClear" hidden>
        <div class="panel">
          <h2 class="ovTitle green">SECTOR CLEAR ✔</h2>
          <div class="ovSub" id="clearName">تم تأمين القطاع</div>
          <div class="statGrid">
            <div class="stat"><div class="k">الوقت</div><div class="v" id="stTime">0s</div></div>
            <div class="stat"><div class="k">درونات مدمّرة</div><div class="v" id="stKills">0</div></div>
            <div class="stat"><div class="k">رقائق بيانات</div><div class="v" id="stChips">0</div></div>
            <div class="stat gold"><div class="k">مكافأة السرعة</div><div class="v" id="stBonus">+0</div></div>
          </div>
          <button class="btn primary" id="btnNext" type="button">القطاع التالي ◀</button>
        </div>
      </div>

      <!-- GAME OVER -->
      <div class="overlay" id="ovOver" hidden>
        <div class="panel">
          <h2 class="ovTitle red">SIGNAL LOST</h2>
          <div class="ovSub">سقطت في شارع المطر…</div>
          <div class="statGrid">
            <div class="stat gold"><div class="k">النتيجة</div><div class="v" id="ovScore">0</div></div>
            <div class="stat"><div class="k">أفضل نتيجة</div><div class="v" id="ovBest">0</div></div>
          </div>
          <button class="btn primary" id="btnRetry" type="button">↺ أعد المحاولة</button>
          <button class="btn ghost" id="btnMenuO" type="button">القائمة</button>
        </div>
      </div>

      <!-- WIN -->
      <div class="overlay" id="ovWin" hidden>
        <div class="panel">
          <h2 class="ovTitle gold">CITY ESCAPED ★</h2>
          <div class="ovSub">هربت من نيون ديستريكت!</div>
          <div id="rankGlyph">S</div>
          <div class="statGrid">
            <div class="stat gold"><div class="k">النتيجة النهائية</div><div class="v" id="wScore">0</div></div>
            <div class="stat"><div class="k">إجمالي الوقت</div><div class="v" id="wTime">0s</div></div>
            <div class="stat"><div class="k">درونات مدمّرة</div><div class="v" id="wKills">0</div></div>
            <div class="stat"><div class="k">رقائق بيانات</div><div class="v" id="wChips">0</div></div>
          </div>
          <button class="btn primary" id="btnAgain" type="button">↺ العب مجدداً</button>
          <button class="btn ghost" id="btnMenuW" type="button">القائمة</button>
        </div>
      </div>
    </div>

    <div id="hints"><b>←→</b> حركة • <b>SPACE</b> قفز مزدوج • <b>J</b> إطلاق • <b>K</b> اندفاع • <b>P</b> إيقاف • <b>M</b> صوت</div>
  </div>
</div>

<script>
(function(){
'use strict';
/* ================= CONSTANTS / HELPERS ================= */
var VW=480, VH=270, GROUND=232, STEP=1/60;
var cv=document.getElementById('game'), ctx=cv.getContext('2d');
ctx.imageSmoothingEnabled=false;
function $(id){return document.getElementById(id)}
function clamp(v,a,b){return v<a?a:v>b?b:v}
function lerp(a,b,t){return a+(b-a)*t}
function rnd(a,b){return a+Math.random()*(b-a)}
function mulberry(seed){return function(){seed|=0;seed=seed+0x6D2B79F5|0;var t=Math.imul(seed^seed>>>15,1|seed);t=t+Math.imul(t^t>>>7,61|t)^t;return((t^t>>>14)>>>0)/4294967296}}
function hexRGB(h){var n=parseInt(h.slice(1),16);return[(n>>16)&255,(n>>8)&255,n&255]}
function rgba(h,a){var c=hexRGB(h);return'rgba('+c[0]+','+c[1]+','+c[2]+','+a+')'}
function mtof(m){return 440*Math.pow(2,(m-69)/12)}
function LSget(k,d){try{var v=localStorage.getItem(k);return v===null?d:v}catch(e){return d}}
function LSset(k,v){try{localStorage.setItem(k,v)}catch(e){}}
function mkCanvas(w,h){var c=document.createElement('canvas');c.width=Math.max(1,Math.ceil(w));c.height=Math.max(1,Math.ceil(h));var x=c.getContext('2d');x.imageSmoothingEnabled=false;return[c,x]}

/* ================= 3x5 PIXEL FONT ================= */
var FONT={
A:[2,5,7,5,5],B:[6,5,6,5,6],C:[3,4,4,4,3],D:[6,5,5,5,6],E:[7,4,6,4,7],F:[7,4,6,4,4],
G:[3,4,5,5,3],H:[5,5,7,5,5],I:[7,2,2,2,7],J:[1,1,1,5,2],K:[5,5,6,5,5],L:[4,4,4,4,7],
M:[7,7,5,5,5],N:[6,5,5,5,5],O:[2,5,5,5,2],P:[6,5,6,4,4],Q:[2,5,5,2,1],R:[6,5,6,5,5],
S:[3,4,2,1,6],T:[7,2,2,2,2],U:[5,5,5,5,7],V:[5,5,5,5,2],W:[5,5,5,7,5],X:[5,5,2,5,5],
Y:[5,5,2,2,2],Z:[7,1,2,4,7],'0':[7,5,5,5,7],'1':[2,6,2,2,7],'2':[6,1,2,4,7],'3':[7,1,3,1,7],
'4':[5,5,7,1,1],'5':[7,4,6,1,6],'6':[3,4,7,5,7],'7':[7,1,2,2,2],'8':[7,5,7,5,7],'9':[7,5,7,1,6],
'-':[0,0,7,0,0],'+':[0,2,7,2,0],'.':[0,0,0,0,2],':':[0,2,0,2,0],'/':[1,1,2,4,4],'!':[2,2,2,0,2],
'>':[4,2,1,2,4],'<':[1,2,4,2,1],' ':[0,0,0,0,0],'×':[5,5,2,5,5]
};
function pText(c,str,x,y,col,s){
  s=s||1;c.fillStyle=col;
  for(var i=0;i<str.length;i++){
    var g=FONT[str[i].toUpperCase()];if(!g)continue;
    for(var r=0;r<5;r++){var bits=g[r];
      for(var b=0;b<3;b++)if(bits&(4>>b))c.fillRect(x+i*4*s+b*s,y+r*s,s,s);
    }
  }
}
function pWidth(str,s){return str.length*4*(s||1)-(s||1)}
function glow(c,x,y,r,col,a){
  if(r<=0)return;var g=c.createRadialGradient(x,y,0,x,y,r);
  g.addColorStop(0,rgba(col,a));g.addColorStop(1,rgba(col,0));
  c.fillStyle=g;c.fillRect(x-r,y-r,r*2,r*2);
}

/* ================= AUDIO ================= */
var AC=null,master,musicG,sfxG,ambG,noiseBuf,dly,muted=LSget('nd_mute','0')==='1';
var musicTimer=null,nextNote=0,stepIdx=0,BPM=104,SPB=60/BPM;
var CHORDS=[[57,60,64],[53,57,60],[60,64,67],[55,59,62]],BASS=[33,29,36,31];
function initAudio(){
  if(AC)return;
  try{AC=new (window.AudioContext||window.webkitAudioContext)()}catch(e){return}
  var comp=AC.createDynamicsCompressor();comp.threshold.value=-18;comp.ratio.value=4;comp.connect(AC.destination);
  master=AC.createGain();master.gain.value=muted?0:1;master.connect(comp);
  musicG=AC.createGain();musicG.gain.value=.5;musicG.connect(master);
  sfxG=AC.createGain();sfxG.gain.value=.9;sfxG.connect(master);
  ambG=AC.createGain();ambG.gain.value=.3;ambG.connect(master);
  noiseBuf=AC.createBuffer(1,AC.sampleRate*2,AC.sampleRate);
  var d=noiseBuf.getChannelData(0);for(var i=0;i<d.length;i++)d[i]=Math.random()*2-1;
  dly=AC.createDelay(.8);dly.delayTime.value=SPB*.75;
  var fb=AC.createGain();fb.gain.value=.32;dly.connect(fb);fb.connect(dly);dly.connect(musicG);
  // rain ambience
  var src=AC.createBufferSource();src.buffer=noiseBuf;src.loop=true;
  var lp=AC.createBiquadFilter();lp.type='lowpass';lp.frequency.value=1300;
  var hp=AC.createBiquadFilter();hp.type='highpass';hp.frequency.value=350;
  var rg=AC.createGain();rg.gain.value=.5;
  src.connect(lp);lp.connect(hp);hp.connect(rg);rg.connect(ambG);src.start();
  var lfo=AC.createOscillator();lfo.frequency.value=.08;var lg=AC.createGain();lg.gain.value=.12;
  lfo.connect(lg);lg.connect(rg.gain);lfo.start();
  nextNote=AC.currentTime+.08;stepIdx=0;
  musicTimer=setInterval(scheduleMusic,25);
}
function scheduleMusic(){
  if(!AC)return;
  while(nextNote<AC.currentTime+.12){
    schedStep(stepIdx,nextNote);
    stepIdx=(stepIdx+1)%128;nextNote+=SPB/4;
  }
}
function schedStep(st,t){
  var bar=Math.floor(st/16)%4,s16=st%16,ch=CHORDS[bar];
  if(s16%2===0){ // bass 8ths
    var o=AC.createOscillator();o.type='sawtooth';o.frequency.value=mtof(BASS[bar]+(s16===14?12:0));
    var f=AC.createBiquadFilter();f.type='lowpass';f.frequency.value=320;
    var g=AC.createGain();g.gain.setValueAtTime(.0001,t);g.gain.exponentialRampToValueAtTime(.24,t+.015);
    g.gain.exponentialRampToValueAtTime(.001,t+SPB*.45);
    o.connect(f);f.connect(g);g.connect(musicG);o.start(t);o.stop(t+SPB*.5);
  }
  if(s16===0){ // pad
    for(var i=0;i<3;i++){
      var o2=AC.createOscillator();o2.type='sawtooth';o2.frequency.value=mtof(ch[i]);o2.detune.value=(i-1)*7;
      var f2=AC.createBiquadFilter();f2.type='lowpass';f2.frequency.value=850;
      var g2=AC.createGain();var dur=SPB*4;
      g2.gain.setValueAtTime(.0001,t);g2.gain.linearRampToValueAtTime(.045,t+.5);
      g2.gain.setValueAtTime(.045,t+dur-.6);g2.gain.linearRampToValueAtTime(.0001,t+dur);
      o2.connect(f2);f2.connect(g2);g2.connect(musicG);o2.start(t);o2.stop(t+dur+.05);
    }
  }
  // arp 16ths
  var an=ch[[0,1,2,1][s16%4]]+12;
  var o3=AC.createOscillator();o3.type='square';o3.frequency.value=mtof(an);
  var g3=AC.createGain();g3.gain.setValueAtTime(.0001,t);g3.gain.exponentialRampToValueAtTime(.055,t+.01);
  g3.gain.exponentialRampToValueAtTime(.001,t+.11);
  o3.connect(g3);g3.connect(musicG);g3.connect(dly);o3.start(t);o3.stop(t+.13);
  if(s16%4===0){ // kick
    var k=AC.createOscillator();k.type='sine';k.frequency.setValueAtTime(130,t);
    k.frequency.exponentialRampToValueAtTime(38,t+.12);
    var kg=AC.createGain();kg.gain.setValueAtTime(.5,t);kg.gain.exponentialRampToValueAtTime(.001,t+.14);
    k.connect(kg);kg.connect(musicG);k.start(t);k.stop(t+.16);
  }
  if(s16%8===4){ // snare
    var sn=AC.createBufferSource();sn.buffer=noiseBuf;
    var sf=AC.createBiquadFilter();sf.type='bandpass';sf.frequency.value=1900;
    var sg=AC.createGain();sg.gain.setValueAtTime(.22,t);sg.gain.exponentialRampToValueAtTime(.001,t+.13);
    sn.connect(sf);sf.connect(sg);sg.connect(musicG);sn.start(t);sn.stop(t+.14);
  }
  if(s16%2===1){ // hat
    var h=AC.createBufferSource();h.buffer=noiseBuf;
    var hf=AC.createBiquadFilter();hf.type='highpass';hf.frequency.value=6500;
    var hg=AC.createGain();var hv=(s16%4===3)?.14:.08;
    hg.gain.setValueAtTime(hv,t);hg.gain.exponentialRampToValueAtTime(.001,t+.035);
    h.connect(hf);hf.connect(hg);hg.connect(musicG);h.start(t);h.stop(t+.04);
  }
}
function tone(f0,f1,d,type,v,at){
  if(!AC)return;var t=AC.currentTime+(at||0);
  var o=AC.createOscillator();o.type=type||'square';
  o.frequency.setValueAtTime(Math.max(1,f0),t);
  o.frequency.exponentialRampToValueAtTime(Math.max(1,f1),t+d);
  var g=AC.createGain();g.gain.setValueAtTime(.0001,t);
  g.gain.exponentialRampToValueAtTime(v,t+.008);g.gain.exponentialRampToValueAtTime(.0001,t+d);
  o.connect(g);g.connect(sfxG);o.start(t);o.stop(t+d+.02);
}
function noiseS(d,v,ft,f0,f1,at){
  if(!AC)return;var t=AC.currentTime+(at||0);
  var s=AC.createBufferSource();s.buffer=noiseBuf;s.loop=true;
  var f=AC.createBiquadFilter();f.type=ft;f.frequency.setValueAtTime(f0,t);
  if(f1)f.frequency.exponentialRampToValueAtTime(Math.max(20,f1),t+d);
  var g=AC.createGain();g.gain.setValueAtTime(.0001,t);
  g.gain.exponentialRampToValueAtTime(v,t+.01);g.gain.exponentialRampToValueAtTime(.0001,t+d);
  s.connect(f);f.connect(g);g.connect(sfxG);s.start(t);s.stop(t+d+.02);
}
var sfx={
  shoot:function(){tone(880,300,.07,'square',.16);noiseS(.02,.08,'highpass',3000)},
  eshoot:function(){tone(300,130,.12,'sawtooth',.1)},
  hit:function(){noiseS(.05,.2,'bandpass',2500);tone(210,80,.05,'square',.12)},
  boom:function(){noiseS(.5,.5,'lowpass',3000,180);tone(110,28,.4,'sine',.5)},
  bigboom:function(){noiseS(.8,.6,'lowpass',4000,120);tone(90,22,.7,'sine',.6);noiseS(.4,.3,'bandpass',900,200,.1)},
  hurt:function(){tone(420,85,.25,'sawtooth',.3);noiseS(.15,.2,'lowpass',1200,300)},
  jump:function(){tone(300,640,.12,'square',.13)},
  djump:function(){tone(400,820,.12,'square',.13)},
  dash:function(){noiseS(.18,.24,'bandpass',700,2600)},
  chip:function(){tone(1250,1650,.05,'square',.1)},
  pick:function(){tone(660,660,.07,'square',.16);tone(990,990,.08,'square',.16,.08)},
  gate:function(){[523,659,784,1046].forEach(function(f,i){tone(f,f,.1,'square',.16,i*.09)})},
  clear:function(){[523,659,784,1046,1318,1568].forEach(function(f,i){tone(f,f,.12,'square',.16,i*.09)})},
  over:function(){tone(240,38,.9,'sawtooth',.32);noiseS(.6,.2,'lowpass',800,100)},
  win:function(){[523,659,784,1046,784,1046,1318,1568].forEach(function(f,i){tone(f,f,.13,'square',.16,i*.1)})},
  alert:function(){tone(950,950,.06,'square',.14);tone(950,950,.06,'square',.14,.1)},
  roar:function(){tone(95,55,.6,'sawtooth',.4);tone(97,52,.6,'sawtooth',.3);noiseS(.5,.25,'lowpass',500,120)},
  step:function(){noiseS(.02,.035,'highpass',3200)},
  land:function(){noiseS(.06,.11,'lowpass',520)},
  thunder:function(){noiseS(1.5,.5,'lowpass',180,60);tone(44,26,1.2,'sine',.3)},
  ui:function(){tone(720,720,.04,'square',.09)}
};
function setMuted(m){
  muted=m;LSset('nd_mute',m?'1':'0');
  if(AC&&master)master.gain.linearRampToValueAtTime(m?0:1,AC.currentTime+.15);
  var b=$('btnSound');b.dataset.muted=m?'1':'0';
  $('sndIc').textContent=m?'🔇':'🔊';$('sndTxt').textContent=m?'صامت':'الصوت';
}
function unlockAudio(){initAudio();if(AC&&AC.state==='suspended')AC.resume()}

/* ================= STATE ================= */
var DIFFS=[
  {name:'عادي',dmg:.7,fire:1.3,count:.8,bspd:150},
  {name:'صعب',dmg:1,fire:1,count:1,bspd:175},
  {name:'كابوس',dmg:1.45,fire:.75,count:1.25,bspd:205}
];
var S={state:'title',diff:clamp(parseInt(LSget('nd_diff','1'),10)||1,0,2),sector:0,score:0,sectorStart:0,
  kills:0,quota:6,chips:0,combo:0,comboT:0,time:0,best:parseInt(LSget('nd_best','0'),10)||0,
  bossDead:false,gateOpen:false,freeze:0,slowT:0,ts:1,shakeT:0,shakeM:0,hurtF:0,
  lightT:rnd(14,26),lightF:0,camX:0,camMin:0,gateMsgT:0,deadT:0,t:0,
  totals:{kills:0,chips:0,time:0}};
var P={x:60,y:GROUND,vx:0,vy:0,face:1,hp:100,en:100,ground:true,coyote:0,jbuf:0,jumps:0,
  shootCd:0,dashT:0,dashCd:0,if:0,anim:0,stepT:0,dead:false,muzzle:0,after:[]};
var Wd=null;
var input={left:false,right:false,jump:false,shoot:false,dash:false};
var coarse=window.matchMedia('(pointer:coarse)').matches||('ontouchstart'in window);
if(coarse)document.body.classList.add('coarse');
$('legendDesk').hidden=coarse;$('legendTouch').hidden=!coarse;

/* ================= PALETTES / SECTORS ================= */
var PALS=[
 {sky:['#0b0620','#2a0f45','#6b1d6e'],acc:'#ff2e88',acc2:'#22d3ee',acc3:'#ffb020',tower:'#1c0f33',win:'#ff5ea8',fog:'#ff2e88',name:'شارع المطر',sub:'RAIN STREET'},
 {sky:['#040b1e','#0a2350','#155e8a'],acc:'#22d3ee',acc2:'#3b82f6',acc3:'#ff2e88',tower:'#0a1730',win:'#5eead4',fog:'#22d3ee',name:'جسر النيون',sub:'NEON BRIDGE'},
 {sky:['#160408','#3d0a1c','#7a1f2b'],acc:'#ffb020',acc2:'#ff3b3b',acc3:'#ff2e88',tower:'#200a14',win:'#ffcf6e',fog:'#ff6a3b',name:'حيّ الحراسة',sub:'WARDEN BLOCK'}
];
var SECTOR_LEN=[2600,3200,3800];

/* ================= WORLD BUILD ================= */
function buildSector(idx){
  var pal=PALS[idx],len=SECTOR_LEN[idx],rng=mulberry(1234+idx*777);
  var w={idx:idx,pal:pal,len:len,platforms:[],enemies:[],chips:[],pickups:[],bolts:[],ebolts:[],
    parts:[],floats:[],triggers:[],gate:{x:len-60},boss:null,arenaStart:len-760,
    lamps:[],vents:[],flickers:[],antennas:[],traffic:[],poles:[],splashes:[],ripples:[],can:{}};
  // platforms
  var x=430;
  while(x<len-560){
    var h=24+((rng()*3)|0)*8;
    w.platforms.push({x:x,w:36,h:h,type:'crate'});
    if(rng()<.35)w.platforms.push({x:x+42,w:36,h:48,type:'crate'});
    for(var c=0;c<3;c++)w.chips.push({x:x+6+c*14,y:GROUND-h-16,taken:false,ph:rng()*6});
    x+=380+rng()*220;
  }
  w.platforms.push({x:len*.52,w:76,h:52,type:'awning'});
  // chip lines
  for(var cx=300;cx<len-300;cx+=520)for(var k=0;k<3;k++)w.chips.push({x:cx+k*16,y:200,taken:false,ph:rng()*6});
  // pickups
  w.pickups.push({x:len*.5,y:GROUND-8,type:'hp',taken:false,ph:0});
  w.pickups.push({x:len*.75,y:GROUND-8,type:'en',taken:false,ph:2});
  if(idx>0)w.pickups.push({x:len*.86,y:GROUND-8,type:'hp',taken:false,ph:4});
  // enemies
  var D=[[520,860,1240,1620,2050,2350],[480,820,1180,1560,1980,2380,2760],[500,900,1400,1900,2400]][idx];
  var Wk=[[700,1500,2150],[650,1350,2050,2650],[700,1600,2300]][idx];
  var T=[[1900],[1000,2250],[1200,2100]][idx];
  var dm=DIFFS[S.diff].count;
  D=D.slice(0,Math.max(3,Math.ceil(D.length*Math.min(1,dm))));
  Wk=Wk.slice(0,Math.max(2,Math.ceil(Wk.length*Math.min(1,dm))));
  if(S.diff===2){D.push(len*.55,len*.8);}
  D.forEach(function(ex){w.enemies.push(mkEnemy('drone',ex,176+rng()*26,rng))});
  Wk.forEach(function(ex){w.enemies.push(mkEnemy('walker',ex,GROUND,rng))});
  T.forEach(function(ex){w.enemies.push(mkEnemy('turret',ex,GROUND,rng))});
  w.quota=idx===0?6:idx===1?9:1;
  // ambush triggers
  if(idx>0){w.triggers.push({x:len*.4,done:false},{x:len*.72,done:false});}
  // boss
  if(idx===2){w.boss=mkBoss(len-260);}
  // lamps / vents / poles
  for(var lx=180;lx<len;lx+=260)w.lamps.push({x:lx,ph:rng()*9});
  for(var vx=500;vx<len;vx+=700)w.vents.push({x:vx,t:rnd(0,2)});
  for(var px=260;px<len*1.25;px+=460+rng()*180)w.poles.push({x:px});
  // traffic
  for(var ti=0;ti<6;ti++)w.traffic.push({x:rng()*len*.35,y:60+rng()*90,v:(20+rng()*40)*(rng()<.5?1:-1),
    col:[pal.acc,pal.acc2,pal.acc3][ti%3]});
  bakeWorld(w,rng);
  return w;
}
function mkEnemy(type,x,y,rng){
  var e={type:type,x:x,y:y,ax:x,ay:y,seed:rng()*9,flash:0,dead:false,aggro:false,
    fireT:rnd(.8,2),dir:rng()<.5?-1:1,vx:0,state:'patrol',stT:0,angle:Math.PI,burst:0,burstT:0};
  if(type==='drone'){e.hp=e.max=52;e.w=18;e.h=12;}
  if(type==='walker'){e.hp=e.max=90;e.w=18;e.h=24;e.b0=x-90;e.b1=x+90;}
  if(type==='turret'){e.hp=e.max=70;e.w=20;e.h=18;}
  return e;
}
function mkBoss(x){
  return {x:x,y:196,ax:x,hp:700,max:700,w:56,h:44,seed:1,flash:0,dead:false,phase:1,
    atkT:2.2,wind:0,pending:null,patIdx:0,state:'hover',stT:0,vx:0,spawnT:6,p2:false,p3:false};
}

/* ================= BAKE BACKGROUNDS ================= */
function bakeWorld(w,rng){
  var pal=w.pal,len=w.len,C=w.can;
  // SKY (static)
  var s=mkCanvas(VW,VH),sc=s[1];C.sky=s[0];
  var g=sc.createLinearGradient(0,0,0,VH);
  g.addColorStop(0,pal.sky[0]);g.addColorStop(.55,pal.sky[1]);g.addColorStop(1,pal.sky[2]);
  sc.fillStyle=g;sc.fillRect(0,0,VW,VH);
  for(var i=0;i<70;i++){sc.fillStyle=rgba(i%3?'#ffffff':pal.acc,rnd(.15,.7));
    sc.fillRect((rng()*VW)|0,(rng()*130)|0,1,1);}
  // giant holo ad ring
  sc.globalCompositeOperation='lighter';
  glow(sc,368,74,60,pal.acc,.16);
  sc.strokeStyle=rgba(pal.acc,.5);sc.lineWidth=2;sc.beginPath();sc.arc(368,74,34,0,7);sc.stroke();
  sc.strokeStyle=rgba(pal.acc2,.35);sc.beginPath();sc.arc(368,74,40,0,7);sc.stroke();
  sc.globalCompositeOperation='source-over';
  for(var hy=44;hy<106;hy+=4){sc.fillStyle=rgba(pal.sky[0],.35);sc.fillRect(326,hy,84,1);}
  pText(sc,'NEON',352,66,rgba(pal.acc,.9),1);pText(sc,'CITY',352,76,rgba(pal.acc2,.8),1);
  // SKYLINE f .15
  var sw=len*.15+VW,sk=mkCanvas(sw,VH),kc=sk[1];C.skyline=sk[0];
  var tx=0;
  while(tx<sw){
    var tw=26+rng()*46,th=70+rng()*130,ty=214-th;
    kc.fillStyle=pal.tower;kc.fillRect(tx,ty,tw,th);
    kc.fillStyle=rgba('#000000',.25);kc.fillRect(tx+tw-4,ty,4,th);
    for(var wy=ty+6;wy<208;wy+=7)for(var wx=tx+3;wx<tx+tw-4;wx+=6)
      if(rng()<.28){kc.fillStyle=rgba(pal.win,rnd(.25,.85));kc.fillRect(wx,wy,2,2);}
    if(rng()<.5){kc.fillStyle=pal.tower;kc.fillRect(tx+tw/2-1,ty-12,2,12);
      w.antennas.push({x:tx+tw/2,y:ty-13,ph:rng()*7});}
    tx+=tw+2+rng()*10;
  }
  // MID f .35
  var mw=len*.35+VW,md=mkCanvas(mw,VH),mc=md[1];C.mid=md[0];
  tx=0;
  while(tx<mw){
    var bw=40+rng()*60,bh=90+rng()*100,by=222-bh;
    mc.fillStyle=rgba('#0d0a1c',.95);mc.fillRect(tx,by,bw,bh);
    mc.fillStyle=rgba(pal.acc,.08);mc.fillRect(tx,by,bw,2);
    for(var my=by+8;my<214;my+=9)for(var mx=tx+4;mx<tx+bw-5;mx+=8)
      if(rng()<.4){mc.fillStyle=rng()<.75?rgba('#ffd9a0',rnd(.2,.6)):rgba(pal.acc2,rnd(.3,.7));
        mc.fillRect(mx,my,3,4);}
    if(rng()<.55){
      var scol=[pal.acc,pal.acc2,pal.acc3][(rng()*3)|0];
      var words=['BAR','HOTEL','24H','CLINIC','ARCADE'];var word=words[(rng()*words.length)|0];
      mc.globalCompositeOperation='lighter';glow(mc,tx+bw/2,by+16,20,scol,.35);mc.globalCompositeOperation='source-over';
      mc.fillStyle='#0a0714';mc.fillRect(tx+bw/2-pWidth(word,1)/2-3,by+10,pWidth(word,1)+6,11);
      mc.strokeStyle=rgba(scol,.8);mc.strokeRect(tx+bw/2-pWidth(word,1)/2-3.5,by+9.5,pWidth(word,1)+7,12);
      pText(mc,word,tx+bw/2-pWidth(word,1)/2,by+13,scol,1);
      w.flickers.push({x:tx+bw/2,y:by+16,col:scol,ph:rng()*9,f:.35});
    }
    tx+=bw+4+rng()*14;
  }
  // skybridge cables
  mc.strokeStyle=rgba('#000000',.5);mc.lineWidth=1;
  for(var cb=0;cb<mw;cb+=300){mc.beginPath();mc.moveTo(cb,90);mc.quadraticCurveTo(cb+150,130,cb+300,95);mc.stroke();}
  // NEAR f .7 — shop facades
  var nw=len*.7+VW,nr=mkCanvas(nw,VH),nc=nr[1];C.near=nr[0];
  var shops=['tattoo','store','clinic','ramen','bar','hotel'];var sx=0,si=0;
  nc.fillStyle='#160f26';nc.fillRect(0,118,nw,114);
  while(sx<nw){
    var sw2=90+rng()*70,kind=shops[si%shops.length];si++;
    drawShop(nc,sx,sw2,kind,pal,rng,w);
    sx+=sw2+2;
  }
  // STREET f 1
  var st=mkCanvas(len,VH),tc2=st[1];C.street=st[0];
  // sidewalk
  tc2.fillStyle='#241a2e';tc2.fillRect(0,GROUND,len,20);
  tc2.fillStyle='#3a2a4a';tc2.fillRect(0,GROUND,len,2);
  tc2.fillStyle=rgba(pal.acc,.25);tc2.fillRect(0,GROUND,len,1);
  for(var gx=0;gx<len;gx+=16){tc2.fillStyle=rgba('#000000',.3);tc2.fillRect(gx,GROUND+2,1,18);}
  // curb + road
  tc2.fillStyle='#0e0a18';tc2.fillRect(0,GROUND+20,len,2);
  var rg=tc2.createLinearGradient(0,GROUND+22,0,VH);
  rg.addColorStop(0,'#0b0716');rg.addColorStop(1,'#05030c');
  tc2.fillStyle=rg;tc2.fillRect(0,GROUND+22,len,VH-GROUND-22);
  // wet neon streaks on road
  for(var rx=40;rx<len;rx+=90+rng()*120){
    var rcol=[pal.acc,pal.acc2,pal.acc3][(rng()*3)|0];
    var rg2=tc2.createLinearGradient(0,GROUND+22,0,VH);
    rg2.addColorStop(0,rgba(rcol,.28));rg2.addColorStop(1,rgba(rcol,0));
    tc2.fillStyle=rg2;tc2.fillRect(rx,GROUND+22,3+rng()*4,VH-GROUND-22);
  }
  // railing (back edge)
  tc2.strokeStyle='#0e1220';tc2.lineWidth=2;
  for(var px2=10;px2<len;px2+=46){
    tc2.fillStyle='#141a2c';tc2.fillRect(px2,GROUND-18,3,18);
    tc2.fillStyle='#232c48';tc2.fillRect(px2,GROUND-18,3,2);
    tc2.beginPath();tc2.moveTo(px2+1,GROUND-14);tc2.quadraticCurveTo(px2+24,GROUND-8,px2+47,GROUND-14);tc2.stroke();
  }
  tc2.fillStyle='#1a2138';tc2.fillRect(0,GROUND-16,len,1);
  // lamps
  w.lamps.forEach(function(l){
    tc2.fillStyle='#101426';tc2.fillRect(l.x,GROUND-52,3,52);
    tc2.fillRect(l.x-6,GROUND-52,15,3);
    tc2.fillStyle='#ffd9a0';tc2.fillRect(l.x-5,GROUND-49,3,3);tc2.fillRect(l.x+6,GROUND-49,3,3);
    tc2.globalCompositeOperation='lighter';
    glow(tc2,l.x-4,GROUND-47,14,'#ffb020',.4);glow(tc2,l.x+7,GROUND-47,14,'#ffb020',.4);
    var cg=tc2.createLinearGradient(0,GROUND-46,0,GROUND);
    cg.addColorStop(0,rgba('#ffb020',.14));cg.addColorStop(1,rgba('#ffb020',0));
    tc2.fillStyle=cg;tc2.beginPath();tc2.moveTo(l.x-5,GROUND-46);tc2.lineTo(l.x-16,GROUND);
    tc2.lineTo(l.x+22,GROUND);tc2.lineTo(l.x+8,GROUND-46);tc2.closePath();tc2.fill();
    tc2.globalCompositeOperation='source-over';
  });
  // props: vending / hydrant / trash
  for(var vx2=260;vx2<len-200;vx2+=430+rng()*260){
    var kind2=rng();
    if(kind2<.4){ // vending machine
      tc2.fillStyle='#0d2b33';tc2.fillRect(vx2,GROUND-30,16,30);
      tc2.fillStyle=pal.acc2;tc2.fillRect(vx2+2,GROUND-27,12,16);
      tc2.fillStyle='#062a33';for(var cr=0;cr<3;cr++)for(var cc=0;cc<3;cc++)
        tc2.fillRect(vx2+3+cc*4,GROUND-25+cr*5,2,3);
      tc2.globalCompositeOperation='lighter';glow(tc2,vx2+8,GROUND-19,16,pal.acc2,.3);tc2.globalCompositeOperation='source-over';
      tc2.fillStyle='#083a44';tc2.fillRect(vx2+2,GROUND-8,12,4);
    }else if(kind2<.7){ // hydrant
      tc2.fillStyle=pal.acc;tc2.fillRect(vx2,GROUND-9,7,9);tc2.fillRect(vx2+1,GROUND-12,5,3);
      tc2.fillStyle=rgba('#ffffff',.3);tc2.fillRect(vx2+1,GROUND-9,1,8);
    }else{ // trash
      tc2.fillStyle='#131022';tc2.fillRect(vx2,GROUND-7,12,7);tc2.fillRect(vx2+9,GROUND-5,8,5);
      tc2.fillStyle='#1e1836';tc2.fillRect(vx2+2,GROUND-9,7,3);
    }
  }
}
function drawShop(c,x,w,kind,pal,rng,world){
  var y0=124,h=108;
  c.fillStyle=['#1b1230','#221436','#191026'][(rng()*3)|0];c.fillRect(x,y0,w,h);
  c.fillStyle=rgba('#000000',.3);c.fillRect(x+w-3,y0,3,h);
  // upper windows
  for(var wy=y0+8;wy<y0+40;wy+=14)for(var wx=x+6;wx<x+w-10;wx+=16){
    c.fillStyle='#0a0714';c.fillRect(wx,wy,10,10);
    if(rng()<.6){c.fillStyle=rgba(rng()<.7?'#ffd9a0':pal.acc2,rnd(.25,.7));c.fillRect(wx+1,wy+1,8,8);
      c.fillStyle=rgba('#000000',.4);c.fillRect(wx+4,wy+1,1,8);}
  }
  // AC unit + pipe
  c.fillStyle='#2a2440';c.fillRect(x+w-16,y0+44,12,8);c.fillStyle='#0e0a18';c.fillRect(x+w-14,y0+46,8,4);
  c.fillStyle='#241d3a';c.fillRect(x+2,y0+40,3,68);
  var cx=x+w/2;
  if(kind==='tattoo'){
    c.globalCompositeOperation='lighter';glow(c,x+12,y0+62,26,'#ff2e88',.5);c.globalCompositeOperation='source-over';
    c.fillStyle='#0a0714';c.fillRect(x+5,y0+42,15,58);
    c.strokeStyle='#ff2e88';c.lineWidth=1;c.strokeRect(x+5.5,y0+42.5,14,57);
    var word='TATTOO';for(var i=0;i<word.length;i++)pText(c,word[i],x+10,y0+46+i*9,'#ff5ea8',1);
    c.fillStyle='#0a0714';c.fillRect(x+26,y0+70,w-32,38);
    c.fillStyle=rgba('#ff2e88',.25);c.fillRect(x+28,y0+72,w-36,34);
    pText(c,'OPEN',x+30,y0+78,'#22d3ee',1);
    world.flickers.push({x:x+12,y:y0+62,col:'#ff2e88',ph:rng()*9,f:.7});
  }else if(kind==='store'){
    // awning stripes green/amber like reference
    for(var ax=x+4;ax<x+w-4;ax+=8){c.fillStyle=((ax/8)|0)%2?'#1f7a4d':'#d99a2b';c.fillRect(ax,y0+52,8,8);}
    c.fillStyle=rgba('#000000',.3);c.fillRect(x+4,y0+58,w-8,2);
    c.globalCompositeOperation='lighter';glow(c,cx,y0+48,24,'#ffb020',.5);c.globalCompositeOperation='source-over';
    c.fillStyle='#0a0714';c.fillRect(cx-24,y0+40,48,12);
    c.strokeStyle='#ffb020';c.strokeRect(cx-24.5,y0+39.5,49,13);
    pText(c,'TWELVE',cx-pWidth('TWELVE',1)/2,y0+43,'#ffd166',1);
    // warm window
    var wg=c.createLinearGradient(0,y0+62,0,y0+106);
    wg.addColorStop(0,'#ffe9c4');wg.addColorStop(1,'#e8a86a');
    c.fillStyle=wg;c.fillRect(x+8,y0+62,w-16,44);
    c.fillStyle=rgba('#7a4a20',.55);
    for(var sy=y0+70;sy<y0+102;sy+=10)c.fillRect(x+10,sy,w-20,2);
    for(var sx2=x+12;sx2<x+w-12;sx2+=9)c.fillRect(sx2,y0+66,2,38);
    c.fillStyle='#c0392b';c.fillRect(x+w-24,y0+66,14,8);pText(c,'SALE',x+w-23,y0+68,'#ffe9c4',1);
    world.flickers.push({x:cx,y:y0+46,col:'#ffb020',ph:rng()*9,f:.7});
  }else if(kind==='clinic'){
    c.fillStyle='#efe6d8';c.fillRect(cx-22,y0+42,44,26);
    c.strokeStyle='#b9a98f';c.strokeRect(cx-22.5,y0+41.5,45,27);
    pText(c,'ROBOT',cx-pWidth('ROBOT',1)/2,y0+46,'#7a4a20',1);
    pText(c,'CLINIC',cx-pWidth('CLINIC',1)/2,y0+56,'#7a4a20',1);
    c.fillStyle='#ff2e88';c.fillRect(cx+16,y0+44,4,10);c.fillRect(cx+13,y0+47,10,4);
    c.globalCompositeOperation='lighter';glow(c,cx,y0+54,22,'#ffd9a0',.35);c.globalCompositeOperation='source-over';
    c.fillStyle='#0a0714';c.fillRect(x+8,y0+74,w-16,32);
    c.fillStyle=rgba('#5eead4',.3);c.fillRect(x+10,y0+76,w-20,28);
    c.fillStyle='#22d3ee';c.fillRect(x+14,y0+82,8,14); // robot silhouette
    c.fillStyle='#0a0714';c.fillRect(x+16,y0+85,4,3);
  }else if(kind==='ramen'){
    c.globalCompositeOperation='lighter';glow(c,cx,y0+50,22,'#ff3b3b',.45);c.globalCompositeOperation='source-over';
    c.fillStyle='#0a0714';c.fillRect(cx-20,y0+42,40,12);
    c.strokeStyle='#ff3b3b';c.strokeRect(cx-20.5,y0+41.5,41,13);
    pText(c,'RAMEN',cx-pWidth('RAMEN',1)/2,y0+45,'#ff6b6b',1);
    c.fillStyle='#3d0a14';for(var nx=x+6;nx<x+w-6;nx+=10)c.fillRect(nx,y0+58,8,14);
    c.fillStyle=rgba('#ffd9a0',.5);c.fillRect(x+8,y0+76,w-16,28);
    world.flickers.push({x:cx,y:y0+48,col:'#ff3b3b',ph:rng()*9,f:.7});
  }else if(kind==='bar'){
    c.globalCompositeOperation='lighter';glow(c,cx,y0+50,22,'#3b82f6',.5);c.globalCompositeOperation='source-over';
    c.fillStyle='#0a0714';c.fillRect(cx-14,y0+42,28,12);
    c.strokeStyle='#3b82f6';c.strokeRect(cx-14.5,y0+41.5,29,13);
    pText(c,'BAR',cx-pWidth('BAR',1)/2,y0+45,'#7db4ff',1);
    c.fillStyle='#0a0714';c.fillRect(x+10,y0+62,w-20,44);
    c.fillStyle=rgba('#3b82f6',.22);c.fillRect(x+12,y0+64,w-24,40);
    pText(c,'OPEN',x+16,y0+70,'#22d3ee',1);
  }else{ // hotel
    c.globalCompositeOperation='lighter';glow(c,x+12,y0+60,24,'#7c3aed',.5);c.globalCompositeOperation='source-over';
    c.fillStyle='#0a0714';c.fillRect(x+5,y0+40,15,62);
    c.strokeStyle='#7c3aed';c.strokeRect(x+5.5,y0+39.5,14,63);
    var hw='HOTEL';for(var hi=0;hi<hw.length;hi++)pText(c,hw[hi],x+10,y0+44+hi*11,'#b794f6',1);
    c.fillStyle='#0a0714';c.fillRect(x+26,y0+70,w-32,36);
    c.fillStyle=rgba('#7c3aed',.25);c.fillRect(x+28,y0+72,w-36,32);
    world.flickers.push({x:x+12,y:y0+60,col:'#7c3aed',ph:rng()*9,f:.7});
  }
  // poster
  if(rng()<.6){c.fillStyle=[pal.acc,pal.acc2,pal.acc3][(rng()*3)|0];
    c.fillRect(x+w-12,y0+80,7,10);c.fillStyle=rgba('#000000',.4);c.fillRect(x+w-12,y0+86,7,1);}
}

/* ================= PARTICLES / FX ================= */
function addPart(p){if(Wd.parts.length>420)Wd.parts.shift();Wd.parts.push(p)}
function spark(x,y,col,n,spd){
  for(var i=0;i<n;i++){var a=rnd(0,6.28),v=rnd(30,spd||140);
    addPart({t:'sp',x:x,y:y,vx:Math.cos(a)*v,vy:Math.sin(a)*v-40,life:rnd(.2,.5),max:.5,col:col,sz:rnd(1,2.4)});}
}
function debris(x,y,col,n){
  for(var i=0;i<n;i++)addPart({t:'db',x:x,y:y,vx:rnd(-120,120),vy:rnd(-220,-60),life:rnd(.5,1),max:1,col:col,sz:rnd(1.5,3)});
}
function smoke(x,y,n){
  for(var i=0;i<n;i++)addPart({t:'sm',x:x+rnd(-4,4),y:y+rnd(-3,3),vx:rnd(-12,12),vy:rnd(-30,-12),life:rnd(.6,1.3),max:1.3,col:'#9b93c9',sz:rnd(2,5)});
}
function ring(x,y,col,r){addPart({t:'rg',x:x,y:y,life:.35,max:.35,col:col,sz:r||26})}
function boom(x,y,big,col){
  col=col||'#ffb020';
  spark(x,y,col,big?26:14,big?220:150);spark(x,y,'#ffffff',big?10:5,big?180:120);
  debris(x,y,'#2a2440',big?10:5);smoke(x,y,big?8:4);ring(x,y,col,big?40:24);
  addPart({t:'fl',x:x,y:y,life:.12,max:.12,col:col,sz:big?34:20});
  S.shakeT=Math.max(S.shakeT,big?.4:.22);S.shakeM=Math.max(S.shakeM,big?7:4);
}
function floatText(x,y,str,col){if(Wd.floats.length>28)Wd.floats.shift();
  Wd.floats.push({x:x,y:y,str:str,col:col||'#ffffff',life:.9,max:.9})}
function shake(t,m){S.shakeT=Math.max(S.shakeT,t);S.shakeM=Math.max(S.shakeM,m)}

/* ================= COMBAT ================= */
function playerBox(){return{x:P.x-6,y:P.y-30,w:12,h:30}}
function enemyBox(e){
  if(e.type==='drone')return{x:e.x-9,y:e.y-6,w:18,h:12};
  if(e.type==='walker')return{x:e.x-9,y:e.y-24,w:18,h:24};
  return{x:e.x-10,y:e.y-18,w:20,h:18};
}
function bossBox(b){return{x:b.x-28,y:b.y-22,w:56,h:44}}
function hitBox(b,x,y,r){return x+r>b.x&&x-r<b.x+b.w&&y+r>b.y&&y-r<b.y+b.h}
function comboMult(){return 1+Math.min(4,Math.floor(S.combo/2))}
function addScore(n,x,y){
  var m=comboMult(),v=n*m;S.score+=v;
  floatText(x,y,'+'+v+(m>1?' ×'+m:''),m>1?'#ff5ea8':'#ffd166');
}
function damageEnemy(e,dmg,dir){
  e.hp-=dmg;e.flash=.08;spark(e.x,e.y-(e.type==='drone'?0:12),'#22d3ee',4,90);
  sfx.hit();
  if(e.hp<=0&&!e.dead)killEnemy(e,dir);
}
function killEnemy(e,dir){
  e.dead=true;S.kills++;S.combo++;S.comboT=3;
  var pts=e.type==='drone'?100:e.type==='walker'?150:200;
  boom(e.x,e.y-(e.type==='drone'?0:12),false,e.type==='drone'?'#22d3ee':'#ff2e88');
  addScore(pts,e.x,e.y-24);sfx.boom();S.freeze=Math.max(S.freeze,.05);
  for(var i=0;i<2;i++)Wd.chips.push({x:e.x+rnd(-10,10),y:e.y-14,taken:false,ph:rnd(0,6),pop:true,vy:-90,vx:rnd(-40,40)});
  if(P.hp<60&&Math.random()<.14)Wd.pickups.push({x:e.x,y:GROUND-8,type:'hp',taken:false,ph:0});
  checkQuota();
}
function checkQuota(){
  if(Wd.idx<2&&S.kills>=Wd.quota&&!S.gateOpen){S.gateOpen=true;sfx.gate();toast('✔ البوابة مفتوحة! توجّه إلى EXIT','big');}
}
function damageBoss(dmg){
  var b=Wd.boss;if(!b||b.dead)return;
  b.hp-=dmg;b.flash=.06;spark(b.x+rnd(-20,20),b.y+rnd(-16,16),'#ff5ea8',3,90);sfx.hit();
  if(b.hp<=b.max*.66&&!b.p2){b.p2=true;b.phase=2;sfx.roar();toast('⚠ الحارسentered المرحلة الثانية!','warn');toast('⚠ الحارس غاضب — المرحلة 2!','warn');}
  if(b.hp<=b.max*.33&&!b.p3){b.p3=true;b.phase=3;sfx.roar();toast('🔥 المرحلة الأخيرة — احذر!','bad');shake(.4,6);}
  if(b.hp<=0){b.dead=true;bossDeath(b);}
}
function bossDeath(b){
  S.bossDead=true;S.gateOpen=true;S.kills++;S.combo++;S.comboT=3;
  addScore(2000,b.x,b.y-30);sfx.bigboom();sfx.gate();
  S.slowT=.9;S.ts=.3;shake(.7,9);
  for(var i=0;i<6;i++)(function(i){setTimeout(function(){if(Wd&&Wd.boss===b){boom(b.x+rnd(-26,26),b.y+rnd(-20,20),true,'#ff2e88');sfx.boom();}},i*130)})(i);
  toast('✔ الحارس سقط! البوابة مفتوحة','big');
  $('bossWrap').classList.remove('on');
}
function damagePlayer(dmg,dir){
  if(P.if>0||P.dead||P.dashT>0)return;
  dmg=Math.round(dmg*DIFFS[S.diff].dmg);
  P.hp-=dmg;P.if=.9;P.vx=dir*160;P.vy=-140;P.ground=false;
  S.hurtF=1;shake(.3,5);sfx.hurt();spark(P.x,P.y-16,'#ff3b3b',10,140);
  S.combo=0;S.comboT=0;
  if(P.hp<=0){P.hp=0;playerDeath();}
}
function playerDeath(){
  P.dead=true;S.deadT=1.15;S.slowT=.9;S.ts=.3;
  boom(P.x,P.y-16,true,'#ff2e88');debris(P.x,P.y-16,'#5a3b2e',12);sfx.bigboom();sfx.over();
}

/* ================= PLAYER UPDATE ================= */
function resetPlayer(){
  P.x=60;P.y=GROUND;P.vx=0;P.vy=0;P.face=1;P.hp=100;P.en=100;P.ground=true;
  P.coyote=0;P.jbuf=0;P.jumps=0;P.shootCd=0;P.dashT=0;P.dashCd=0;P.if=0;P.dead=false;
  P.muzzle=0;P.after.length=0;P.stepT=0;
}
function updatePlayer(dt){
  if(P.dead)return;
  P.if=Math.max(0,P.if-dt);P.shootCd-=dt;P.dashCd-=dt;P.muzzle=Math.max(0,P.muzzle-dt);
  P.en=Math.min(100,P.en+20*dt);
  var move=(input.right?1:0)-(input.left?1:0);
  if(move!==0)P.face=move;
  // dash
  if(input.dash&&P.dashCd<=0&&P.dashT<=0){
    if(P.en>=35){P.en-=35;P.dashT=.18;P.dashCd=.4;P.if=Math.max(P.if,.22);
      P.vx=P.face*520;P.vy=0;sfx.dash();spark(P.x,P.y-14,'#22d3ee',6,80);}
    else{$('enBar').classList.remove('deny');void $('enBar').offsetWidth;$('enBar').classList.add('deny');sfx.ui();}
    input.dash=false;
  }
  if(P.dashT>0){
    P.dashT-=dt;P.vx=P.face*520;P.vy=0;
    if(P.after.length>8)P.after.shift();
    P.after.push({x:P.x,y:P.y,face:P.face,life:.25});
  }else{
    var acc=1400,max=170;
    if(move!==0){P.vx+=move*acc*dt;P.vx=clamp(P.vx,-max,max);}
    else{var f=1200*dt;if(Math.abs(P.vx)<=f)P.vx=0;else P.vx-=Math.sign(P.vx)*f;}
    P.vy+=900*dt;if(P.vy>420)P.vy=420;
  }
  // jump buffer / coyote
  if(input.jump){P.jbuf=.12;input.jump=false;}
  P.jbuf-=dt;P.coyote-=dt;
  if(P.jbuf>0){
    if(P.ground||P.coyote>0){P.vy=-300;P.ground=false;P.coyote=0;P.jbuf=0;P.jumps=1;sfx.jump();smoke(P.x,P.y,3);}
    else if(P.jumps<2){P.vy=-265;P.jbuf=0;P.jumps=2;sfx.djump();ring(P.x,P.y-6,'#34d399',14);spark(P.x,P.y-4,'#34d399',5,70);}
  }
  var prevY=P.y;
  P.x+=P.vx*dt;P.y+=P.vy*dt;
  P.x=clamp(P.x,10,Wd.len-14);
  // ground
  var wasGround=P.ground;P.ground=false;
  if(P.y>=GROUND){P.y=GROUND;P.vy=0;P.ground=true;}
  // one-way platforms
  if(P.vy>=0)for(var i=0;i<Wd.platforms.length;i++){var pl=Wd.platforms[i];
    var top=GROUND-pl.h;
    if(P.x>pl.x-4&&P.x<pl.x+pl.w+4&&prevY<=top+2&&P.y>=top){P.y=top;P.vy=0;P.ground=true;}
  }
  if(P.ground){P.jumps=0;P.coyote=.1;
    if(!wasGround){sfx.land();smoke(P.x,P.y,2);}
    if(Math.abs(P.vx)>40){P.stepT-=dt;if(P.stepT<=0){P.stepT=.27;sfx.step();}}
  }
  P.anim+=dt*(Math.abs(P.vx)>20?10:3);
  // afterimages decay
  for(var a=P.after.length-1;a>=0;a--){P.after[a].life-=dt;if(P.after[a].life<=0)P.after.splice(a,1);}
  // shoot
  if(input.shoot&&P.shootCd<=0&&P.dashT<=0){
    P.shootCd=.16;P.muzzle=.06;
    var gx=P.x+P.face*10,gy=P.y-18;
    var dx=P.face,dy=0,best=null,bd=1e9;
    var targets=Wd.enemies.filter(function(e){return!e.dead});
    if(Wd.boss&&!Wd.boss.dead)targets.push(Wd.boss);
    for(var t=0;t<targets.length;t++){var e=targets[t];
      var ex=e.x-P.x,ey=(e.y-(e.type==='drone'?0:12))-gy;
      if(ex*P.face>0&&Math.abs(ex)<340){var ang=Math.abs(Math.atan2(ey,Math.abs(ex)));
        if(ang<.45){var d=Math.abs(ex);if(d<bd){bd=d;best=e;}}}
    }
    if(best){var tx=best.x-gx,ty=(best.y-(best.type==='drone'?0:12))-gy;
      var tl=Math.hypot(tx,ty)||1;dx=tx/tl;dy=ty/tl;}
    Wd.bolts.push({x:gx,y:gy,vx:dx*480,vy:dy*480,life:.9});
    sfx.shoot();P.vx-=P.face*14;
    addPart({t:'fl',x:gx+dx*4,y:gy,life:.05,max:.05,col:'#22d3ee',sz:9});
  }
}

/* ================= ENEMIES UPDATE ================= */
function updateEnemies(dt){
  var dm=DIFFS[S.diff];
  for(var i=0;i<Wd.enemies.length;i++){
    var e=Wd.enemies[i];if(e.dead)continue;
    e.flash=Math.max(0,e.flash-dt);
    var dx=P.x-e.x,dy=(P.y-16)-e.y,dist=Math.hypot(dx,dy);
    if(e.type==='drone'){
      e.y=e.ay+Math.sin(S.t*2+e.seed)*4;
      if(!P.dead&&dist<270){
        if(!e.aggro){e.aggro=true;floatText(e.x,e.y-14,'!','#ff3b3b');sfx.alert();}
        var want=dist>200?1:dist<130?-1:0;
        e.x+=Math.sign(dx)*want*55*dt;
        e.fireT-=dt;
        if(e.fireT<=0){e.fireT=rnd(1.5,2.2)*dm.fire;
          var l=Math.hypot(dx,dy)||1;
          Wd.ebolts.push({x:e.x,y:e.y+4,vx:dx/l*dm.bspd,vy:dy/l*dm.bspd,life:3,col:'#ff5e5e'});
          sfx.eshoot();addPart({t:'fl',x:e.x,y:e.y+4,life:.05,max:.05,col:'#ff3b3b',sz:7});}
      }else{e.x=lerp(e.x,e.ax,dt*.5);}
      e.x=clamp(e.x,20,Wd.len-30);
      if(!P.dead&&P.if<=0&&hitBox(enemyBox(e),P.x,P.y-15,7))damagePlayer(10,Math.sign(dx)||1);
    }
    else if(e.type==='walker'){
      if(e.state==='patrol'){
        e.x+=e.dir*40*dt;if(e.x<e.b0)e.dir=1;if(e.x>e.b1)e.dir=-1;
        if(!P.dead&&dist<220&&Math.abs(dy)<40){e.state='wind';e.stT=.4;e.dir=Math.sign(dx)||1;
          floatText(e.x,e.y-30,'!','#ffb020');sfx.alert();}
      }else if(e.state==='wind'){
        e.stT-=dt;if(e.stT<=0){e.state='charge';e.stT=1.1;}
      }else{
        e.x+=e.dir*125*dt;e.stT-=dt;
        if(Math.random()<.3)addPart({t:'sp',x:e.x-e.dir*8,y:e.y-2,vx:-e.dir*40,vy:-20,life:.2,max:.2,col:'#ffb020',sz:1.5});
        if(e.stT<=0||e.x<e.b0-40||e.x>e.b1+40){e.state='patrol';e.b0=Math.min(e.b0,e.x);e.b1=Math.max(e.b1,e.x);}
      }
      e.x=clamp(e.x,16,Wd.len-24);
      if(!P.dead&&P.if<=0&&hitBox(enemyBox(e),P.x,P.y-15,7))damagePlayer(15,Math.sign(dx)||1);
    }
    else{ // turret
      var ta=Math.atan2((P.y-18)-(e.y-14),P.x-e.x);
      e.angle=lerp(e.angle,ta,dt*4);
      if(!P.dead&&dist<300){
        e.fireT-=dt;
        if(e.fireT<=0){e.burst=3;e.burstT=0;e.fireT=2.6*dm.fire;}
      }
      if(e.burst>0){e.burstT-=dt;
        if(e.burstT<=0){e.burst--;e.burstT=.14;
          var bx=e.x+Math.cos(e.angle)*12,by=e.y-14+Math.sin(e.angle)*12;
          Wd.ebolts.push({x:bx,y:by,vx:Math.cos(e.angle)*dm.bspd,vy:Math.sin(e.angle)*dm.bspd,life:3,col:'#ffb020'});
          sfx.eshoot();}}
    }
  }
  // cleanup dead
  for(var d=Wd.enemies.length-1;d>=0;d--)if(Wd.enemies[d].dead&&Wd.enemies[d].flash<=0)Wd.enemies.splice(d,1);
}
function updateBoss(dt){
  var b=Wd.boss;if(!b||b.dead)return;
  b.flash=Math.max(0,b.flash-dt);
  var dm=DIFFS[S.diff];
  b.y=196+Math.sin(S.t*1.6)*6;
  if(b.state==='hover'){
    b.x=lerp(b.x,b.ax,dt*2);
    if(b.wind>0){b.wind-=dt;if(b.wind<=0)execBossPattern(b,dm);}
    else{b.atkT-=dt;
      if(b.atkT<=0&&!P.dead){
        var pats=b.phase===1?['aim3']:b.phase===2?['aim3','radial','charge']:['aim5','radial','charge'];
        b.pending=pats[b.patIdx%pats.length];b.patIdx++;
        b.wind=.38;b.atkT=(b.phase===3?1.3:b.phase===2?1.7:2.0)*dm.fire;
      }}
    if(b.phase===3){b.spawnT-=dt;
      if(b.spawnT<=0){b.spawnT=7;
        var alive=Wd.enemies.filter(function(e){return!e.dead&&e.type==='drone'}).length;
        if(alive<2){var e=mkEnemy('drone',b.x-60,180,Math.random);e.aggro=true;Wd.enemies.push(e);
          spark(e.x,e.y,'#ff2e88',8,100);sfx.alert();toast('⚠ الحارس استدعى درونات!','warn');}}}
  }else if(b.state==='charge'){
    b.stT-=dt;b.x+=b.vx*dt;
    addPart({t:'sp',x:b.x-Math.sign(b.vx)*26,y:b.y+rnd(-14,14),vx:-Math.sign(b.vx)*60,vy:rnd(-20,20),life:.25,max:.25,col:'#ff2e88',sz:2});
    if(b.stT<=0||b.x<Wd.arenaStart+70||b.x>Wd.len-90){b.state='hover';b.vx=0;}
  }
  b.x=clamp(b.x,Wd.arenaStart+60,Wd.len-80);
  if(!P.dead&&P.if<=0&&hitBox(bossBox(b),P.x,P.y-15,7))damagePlayer(25,Math.sign(P.x-b.x)||1);
  // jets
  if(Math.random()<.5)addPart({t:'sp',x:b.x+rnd(-18,18),y:b.y+22,vx:rnd(-10,10),vy:rnd(40,80),life:.2,max:.2,col:'#22d3ee',sz:1.5});
}
function execBossPattern(b,dm){
  var p=b.pending;b.pending=null;if(!p||P.dead)return;
  var dx=P.x-b.x,dy=(P.y-16)-b.y,l=Math.hypot(dx,dy)||1;
  if(p==='aim3'||p==='aim5'){
    var n=p==='aim3'?3:5,base=Math.atan2(dy,dx);
    for(var i=0;i<n;i++){var a=base+(i-(n-1)/2)*.16;
      Wd.ebolts.push({x:b.x,y:b.y,vx:Math.cos(a)*dm.bspd*1.1,vy:Math.sin(a)*dm.bspd*1.1,life:3.5,col:'#ff2e88'});}
    sfx.eshoot();addPart({t:'fl',x:b.x,y:b.y,life:.07,max:.07,col:'#ff2e88',sz:16});
  }else if(p==='radial'){
    for(var r=0;r<10;r++){var ra=r/10*6.283;
      Wd.ebolts.push({x:b.x,y:b.y,vx:Math.cos(ra)*dm.bspd*.8,vy:Math.sin(ra)*dm.bspd*.8,life:3.5,col:'#c026d3'});}
    sfx.roar();ring(b.x,b.y,'#c026d3',36);
  }else if(p==='charge'){
    b.state='charge';b.stT=.55;b.vx=Math.sign(dx)*270;sfx.dash();shake(.25,4);
  }
}

/* ================= BOLTS / CHIPS / PICKUPS ================= */
function updateBolts(dt){
  var i,b;
  for(i=Wd.bolts.length-1;i>=0;i--){b=Wd.bolts[i];
    b.x+=b.vx*dt;b.y+=b.vy*dt;b.life-=dt;var rm=b.life<=0||b.x<0||b.x>Wd.len||b.y<0||b.y>VH;
    if(!rm){
      for(var e=0;e<Wd.enemies.length;e++){var en=Wd.enemies[e];
        if(!en.dead&&hitBox(enemyBox(en),b.x,b.y,3)){damageEnemy(en,26,Math.sign(b.vx));rm=true;break;}}
      if(!rm&&Wd.boss&&!Wd.boss.dead&&hitBox(bossBox(Wd.boss),b.x,b.y,3)){damageBoss(26);rm=true;}
    }
    if(rm)Wd.bolts.splice(i,1);
  }
  for(i=Wd.ebolts.length-1;i>=0;i--){b=Wd.ebolts[i];
    b.x+=b.vx*dt;b.y+=b.vy*dt;b.life-=dt;var rm2=b.life<=0||b.x<0||b.x>Wd.len||b.y<0||b.y>VH;
    if(!rm2&&!P.dead&&P.if<=0&&hitBox(playerBox(),b.x,b.y,3)){damagePlayer(10,Math.sign(b.vx)||1);rm2=true;}
    if(rm2)Wd.ebolts.splice(i,1);
  }
}
function updateChips(dt){
  for(var i=0;i<Wd.chips.length;i++){var c=Wd.chips[i];if(c.taken)continue;
    if(c.pop){c.vy+=300*dt;c.y+=c.vy*dt;c.x+=(c.vx||0)*dt;if(c.y>GROUND-6){c.y=GROUND-6;c.pop=false;}}
    var dx=P.x-c.x,dy=(P.y-16)-c.y,d=Math.hypot(dx,dy);
    if(!P.dead&&d<44){c.x+=dx/d*180*dt;c.y+=dy/d*180*dt;}
    if(!P.dead&&d<12){c.taken=true;S.chips++;S.score+=25;P.en=Math.min(100,P.en+4);
      sfx.chip();spark(c.x,c.y,'#22d3ee',4,60);floatText(c.x,c.y-6,'+25','#5eead4');}
  }
  for(var p=0;p<Wd.pickups.length;p++){var pk=Wd.pickups[p];if(pk.taken)continue;
    if(!P.dead&&Math.abs(P.x-pk.x)<12&&Math.abs((P.y-14)-pk.y)<20){
      pk.taken=true;sfx.pick();ring(pk.x,pk.y,pk.type==='hp'?'#34d399':'#22d3ee',16);
      if(pk.type==='hp'){P.hp=Math.min(100,P.hp+30);floatText(pk.x,pk.y-10,'+30','#34d399');}
      else{P.en=Math.min(100,P.en+50);floatText(pk.x,pk.y-10,'+50','#22d3ee');}
    }
  }
}
function updateParts(dt){
  for(var i=Wd.parts.length-1;i>=0;i--){var p=Wd.parts[i];p.life-=dt;
    if(p.life<=0){Wd.parts.splice(i,1);continue;}
    if(p.t==='sp'||p.t==='db'){p.x+=p.vx*dt;p.y+=p.vy*dt;p.vy+=(p.t==='db'?500:200)*dt;
      if(p.t==='db'&&p.y>GROUND){p.y=GROUND;p.vy*=-.4;p.vx*=.7;}}
    else if(p.t==='sm'){p.x+=p.vx*dt;p.y+=p.vy*dt;p.sz+=4*dt;}
  }
  for(var f=Wd.floats.length-1;f>=0;f--){var ft=Wd.floats[f];ft.life-=dt;ft.y-=26*dt;
    if(ft.life<=0)Wd.floats.splice(f,1);}
}

/* ================= AMBIENT ================= */
var rainF=[],rainB=[];
for(var rf=0;rf<90;rf++)rainF.push({x:rnd(0,VW),y:rnd(0,VH),v:rnd(460,560)});
for(var rb=0;rb<70;rb++)rainB.push({x:rnd(0,VW),y:rnd(0,VH),v:rnd(330,400)});
function updateAmbient(dt){
  S.t+=dt;
  var i,d;
  for(i=0;i<rainF.length;i++){d=rainF[i];d.y+=d.v*dt;d.x-=60*dt;
    if(d.y>VH){d.y=-8;d.x=rnd(0,VW+40);
      if(Wd&&Math.random()<.3&&Wd.splashes.length<40)Wd.splashes.push({x:rnd(0,VW),y:GROUND+rnd(2,16),life:.18});}}
  for(i=0;i<rainB.length;i++){d=rainB[i];d.y+=d.v*dt;d.x-=40*dt;if(d.y>VH){d.y=-6;d.x=rnd(0,VW+30);}}
  if(Wd){
    for(i=Wd.splashes.length-1;i>=0;i--){Wd.splashes[i].life-=dt;if(Wd.splashes[i].life<=0)Wd.splashes.splice(i,1);}
    if(Math.random()<dt*.7&&Wd.ripples.length<8)Wd.ripples.push({x:rnd(20,VW-20),y:GROUND+rnd(24,34),r:1,life:.8});
    for(i=Wd.ripples.length-1;i>=0;i--){var rp=Wd.ripples[i];rp.r+=14*dt;rp.life-=dt;if(rp.life<=0)Wd.ripples.splice(i,1);}
    for(i=0;i<Wd.traffic.length;i++){var tr=Wd.traffic[i];tr.x+=tr.v*dt;
      var mw=Wd.len*.35+VW;if(tr.x<-20)tr.x=mw+10;if(tr.x>mw+20)tr.x=-10;}
    for(i=0;i<Wd.vents.length;i++){var v=Wd.vents[i];v.t-=dt;
      if(v.t<=0){v.t=rnd(.4,1.2);smoke(v.x,GROUND-2,1);}}
  }
  S.lightT-=dt;
  if(S.lightT<=0){S.lightT=rnd(16,32);S.lightF=.85;sfx.thunder();}
  S.lightF=Math.max(0,S.lightF-dt*2.2);
  S.hurtF=Math.max(0,S.hurtF-dt*2.5);
  S.shakeT=Math.max(0,S.shakeT-dt);
  if(S.shakeT<=0)S.shakeM=0;
}

/* ================= CAMERA / TRIGGERS / GATE ================= */
function updateCamera(dt){
  var target=P.x-200+P.face*40;
  var minCam=0,maxCam=Wd.len-VW;
  if(Wd.boss&&!Wd.boss.dead&&P.x>Wd.arenaStart)minCam=Wd.arenaStart;
  S.camX=clamp(lerp(S.camX,target,Math.min(1,dt*6)),minCam,maxCam);
}
function updateTriggers(){
  for(var i=0;i<Wd.triggers.length;i++){var tr=Wd.triggers[i];
    if(!tr.done&&P.x>tr.x){tr.done=true;sfx.alert();toast('⚠ كمين! درونات قادمة','warn');
      var n=S.diff===2?3:2;
      for(var k=0;k<n;k++){var e=mkEnemy('drone',Math.min(Wd.len-60,S.camX+VW+30+k*30),170+rnd(0,30),Math.random);
        e.ax=S.camX+340+k*50;e.aggro=true;Wd.enemies.push(e);spark(e.x,e.y,'#ff2e88',6,90);}
    }}
}
function updateGate(dt){
  S.gateMsgT-=dt;
  var g=Wd.gate;
  if(Math.abs(P.x-g.x)<16&&P.y>GROUND-40&&!P.dead){
    if(S.gateOpen){sectorClear();}
    else if(S.gateMsgT<=0){S.gateMsgT=1.6;sfx.ui();
      toast(Wd.idx===2?'⚠ دمّر الحارس WARDEN-09 أولاً!':'⚠ دمّر الأهداف المتبقية أولاً! ('+S.kills+'/'+Wd.quota+')','warn');}
  }
}

/* ================= RENDER ================= */
function render(){
  var cam=S.camX,shx=0,shy=0;
  if(S.shakeT>0){shx=rnd(-1,1)*S.shakeM*S.shakeT;shy=rnd(-1,1)*S.shakeM*S.shakeT;}
  ctx.setTransform(1,0,0,1,0,0);
  ctx.fillStyle='#05030c';ctx.fillRect(0,0,VW,VH);
  ctx.save();ctx.translate(Math.round(shx),Math.round(shy));
  var pal=Wd.pal;
  ctx.drawImage(Wd.can.sky,0,0);
  ctx.drawImage(Wd.can.skyline,Math.round(-cam*.15),0);
  // antenna blinks
  for(var a=0;a<Wd.antennas.length;a++){var an=Wd.antennas[a];var ax=an.x-cam*.15;
    if(ax>-4&&ax<VW+4&&Math.sin(S.t*2+an.ph)>.4){ctx.fillStyle='#ff3b3b';ctx.fillRect(ax,an.y,2,2);
      ctx.globalCompositeOperation='lighter';glow(ctx,ax+1,an.y+1,5,'#ff3b3b',.5);ctx.globalCompositeOperation='source-over';}}
  // traffic
  var moff=cam*.35;
  ctx.globalCompositeOperation='lighter';
  for(var t=0;t<Wd.traffic.length;t++){var tr=Wd.traffic[t];var tx=tr.x-moff;
    if(tx>-20&&tx<VW+20){var dir=Math.sign(tr.v);
      var tg=ctx.createLinearGradient(tx-dir*12,0,tx,0);
      tg.addColorStop(0,rgba(tr.col,0));tg.addColorStop(1,rgba(tr.col,.6));
      ctx.fillStyle=tg;ctx.fillRect(Math.min(tx,tx-dir*12),tr.y,12,1);
      ctx.fillStyle=rgba('#ffffff',.9);ctx.fillRect(tx,tr.y,2,1);
      glow(ctx,tx,tr.y,4,tr.col,.5);}}
  ctx.globalCompositeOperation='source-over';
  ctx.drawImage(Wd.can.mid,Math.round(-moff),0);
  // mid flickers
  ctx.globalCompositeOperation='lighter';
  for(var f=0;f<Wd.flickers.length;f++){var fl=Wd.flickers[f];
    var layer=fl.f===.35?moff:cam*.7;var fx=fl.x-layer;
    if(fx>-30&&fx<VW+30){var flick=Math.sin(S.t*7+fl.ph)*Math.sin(S.t*13+fl.ph*2);
      if(flick>-.2)glow(ctx,fx,fl.y,16,fl.col,.18+.12*flick);}}
  ctx.globalCompositeOperation='source-over';
  drawFog(0);
  ctx.drawImage(Wd.can.near,Math.round(-cam*.7),0);
  ctx.drawImage(Wd.can.street,Math.round(-cam),0);
  // lamp flicker bulbs
  ctx.globalCompositeOperation='lighter';
  for(var l=0;l<Wd.lamps.length;l++){var lp=Wd.lamps[l];var lx=lp.x-cam;
    if(lx>-20&&lx<VW+20){var lf=Math.sin(S.t*9+lp.ph)>.92?.1:.4;
      glow(ctx,lx-4,GROUND-47,10,'#ffb020',lf);glow(ctx,lx+7,GROUND-47,10,'#ffb020',lf);}}
  ctx.globalCompositeOperation='source-over';
  drawGate(cam);
  drawPlatforms(cam);
  drawChips(cam);drawPickups(cam);
  // entities
  for(var e=0;e<Wd.enemies.length;e++)drawEnemy(Wd.enemies[e],cam);
  if(Wd.boss&&!Wd.boss.dead)drawBoss(Wd.boss,cam);
  drawPlayer(cam);
  drawBolts(cam);
  drawParts(cam);
  drawFloats(cam);
  // foreground poles + cables
  var foff=cam*1.25;
  ctx.fillStyle='rgba(4,2,10,.92)';
  for(var p=0;p<Wd.poles.length;p++){var px=Wd.poles[p].x-foff;
    if(px>-30&&px<VW+30){ctx.fillRect(px,0,4,VH);ctx.fillRect(px-6,26,16,3);
      ctx.strokeStyle='rgba(4,2,10,.8)';ctx.lineWidth=1;
      ctx.beginPath();ctx.moveTo(px+2,28);ctx.quadraticCurveTo(px+120,66,px+240,30);ctx.stroke();}}
  drawFog(1);
  // rain back
  ctx.strokeStyle=rgba(pal.acc2,.22);ctx.lineWidth=1;ctx.beginPath();
  for(var rb2=0;rb2<rainB.length;rb2++){var d2=rainB[rb2];ctx.moveTo(d2.x,d2.y);ctx.lineTo(d2.x-1.5,d2.y+5);}
  ctx.stroke();
  // rain front
  ctx.strokeStyle='rgba(210,230,255,.45)';ctx.beginPath();
  for(var rf2=0;rf2<rainF.length;rf2++){var d3=rainF[rf2];ctx.moveTo(d3.x,d3.y);ctx.lineTo(d3.x-2,d3.y+7);}
  ctx.stroke();
  // splashes & ripples (screen space)
  ctx.fillStyle='rgba(210,230,255,.5)';
  for(var sp=0;sp<Wd.splashes.length;sp++){var s2=Wd.splashes[sp];
    ctx.fillRect(s2.x-1,s2.y,3,1);ctx.fillRect(s2.x,s2.y-1,1,1);}
  ctx.strokeStyle=rgba(pal.acc2,.18);
  for(var rp2=0;rp2<Wd.ripples.length;rp2++){var r2=Wd.ripples[rp2];
    ctx.beginPath();ctx.ellipse(r2.x,r2.y,r2.r,r2.r*.3,0,0,7);ctx.stroke();}
  ctx.restore();
  // lightning
  if(S.lightF>0){ctx.fillStyle=rgba('#cfe8ff',S.lightF*.35);ctx.fillRect(0,0,VW,VH);}
  // hurt flash
  if(S.hurtF>0){ctx.fillStyle=rgba('#ff3b3b',S.hurtF*.22);ctx.fillRect(0,0,VW,VH);}
}
function drawFog(layer){
  var pal=Wd.pal;
  var y=layer?190:120,amp=layer?8:14,sp=layer?18:9;
  var off=(S.t*sp)%VW;
  ctx.globalCompositeOperation='lighter';
  var g=ctx.createLinearGradient(0,y-24,0,y+30);
  g.addColorStop(0,rgba(pal.fog,0));g.addColorStop(.5,rgba(pal.fog,layer?.05:.07));g.addColorStop(1,rgba(pal.fog,0));
  ctx.fillStyle=g;
  ctx.beginPath();ctx.moveTo(0,y+30);
  for(var x=0;x<=VW;x+=16)ctx.lineTo(x,y+Math.sin((x+off)*.02)*amp);
  ctx.lineTo(VW,y+30);ctx.closePath();ctx.fill();
  ctx.globalCompositeOperation='source-over';
}
function drawGate(cam){
  var g=Wd.gate,x=g.x-cam;if(x<-60||x>VW+60)return;
  var col=S.gateOpen?'#34d399':'#ff3b3b';
  ctx.fillStyle='#141a2c';ctx.fillRect(x-16,GROUND-64,6,64);ctx.fillRect(x+12,GROUND-64,6,64);
  ctx.fillStyle='#232c48';ctx.fillRect(x-16,GROUND-64,6,3);ctx.fillRect(x+12,GROUND-64,6,3);
  ctx.fillStyle='#0a0714';ctx.fillRect(x-14,GROUND-72,30,10);
  ctx.strokeStyle=col;ctx.strokeRect(x-14.5,GROUND-72.5,31,11);
  pText(ctx,'EXIT',x-pWidth('EXIT',1)/2,GROUND-69,col,1);
  ctx.globalCompositeOperation='lighter';
  var pulse=.3+.2*Math.sin(S.t*4);
  var fg=ctx.createLinearGradient(x-10,0,x+12,0);
  fg.addColorStop(0,rgba(col,0));fg.addColorStop(.5,rgba(col,S.gateOpen?pulse+.15:pulse*.6));fg.addColorStop(1,rgba(col,0));
  ctx.fillStyle=fg;ctx.fillRect(x-10,GROUND-62,22,62);
  for(var sy=GROUND-62;sy<GROUND;sy+=6){var so=(S.t*30+sy)%6;
    ctx.fillStyle=rgba(col,.25);ctx.fillRect(x-8,sy+so,18,1);}
  glow(ctx,x+1,GROUND-32,26,col,S.gateOpen?.3:.15);
  ctx.globalCompositeOperation='source-over';
  if(S.gateOpen)pText(ctx,'>',x-2,GROUND-40+Math.sin(S.t*5)*2,'#ffffff',1);
}
function drawPlatforms(cam){
  for(var i=0;i<Wd.platforms.length;i++){var pl=Wd.platforms[i];var x=pl.x-cam;
    if(x<-90||x>VW+20)continue;var top=GROUND-pl.h;
    if(pl.type==='awning'){
      ctx.fillStyle='#101426';ctx.fillRect(x+4,top+8,3,pl.h-8);ctx.fillRect(x+pl.w-7,top+8,3,pl.h-8);
      for(var ax=0;ax<pl.w;ax+=8){ctx.fillStyle=((ax/8)|0)%2?'#1f7a4d':'#d99a2b';ctx.fillRect(x+ax,top,8,8);}
      ctx.fillStyle='rgba(0,0,0,.3)';ctx.fillRect(x,top+6,pl.w,2);
    }else{
      ctx.fillStyle='#2a2440';ctx.fillRect(x,top,pl.w,pl.h);
      ctx.fillStyle='#3a3158';ctx.fillRect(x,top,pl.w,3);
      ctx.fillStyle='rgba(0,0,0,.35)';ctx.fillRect(x+pl.w-4,top,4,pl.h);
      ctx.strokeStyle='rgba(155,147,201,.3)';ctx.strokeRect(x+.5,top+.5,pl.w-1,pl.h-1);
      ctx.beginPath();ctx.moveTo(x,top);ctx.lineTo(x+pl.w,top+pl.h);ctx.moveTo(x+pl.w,top);ctx.lineTo(x,top+pl.h);ctx.stroke();
      pText(ctx,'K7',x+4,top+4,'rgba(155,147,201,.5)',1);
    }
  }
}
function drawChips(cam){
  ctx.globalCompositeOperation='lighter';
  for(var i=0;i<Wd.chips.length;i++){var c=Wd.chips[i];if(c.taken)continue;
    var x=c.x-cam;if(x<-10||x>VW+10)continue;
    var bob=Math.sin(S.t*3+c.ph)*2,s=Math.abs(Math.sin(S.t*4+c.ph));
    glow(ctx,x,c.y+bob,7,'#22d3ee',.4);
    ctx.fillStyle='#5eead4';
    ctx.fillRect(x-2*s-1,c.y+bob-3,2*s+2,6);
    ctx.fillStyle='#ffffff';ctx.fillRect(x-1,c.y+bob-1,1,2);}
  ctx.globalCompositeOperation='source-over';
}
function drawPickups(cam){
  for(var i=0;i<Wd.pickups.length;i++){var pk=Wd.pickups[i];if(pk.taken)continue;
    var x=pk.x-cam;if(x<-14||x>VW+14)continue;
    var bob=Math.sin(S.t*2.5+pk.ph)*2,col=pk.type==='hp'?'#34d399':'#22d3ee';
    ctx.globalCompositeOperation='lighter';glow(ctx,x,pk.y+bob,11,col,.45);ctx.globalCompositeOperation='source-over';
    ctx.fillStyle='#0a0714';ctx.fillRect(x-6,pk.y+bob-6,12,12);
    ctx.strokeStyle=col;ctx.strokeRect(x-6.5,pk.y+bob-6.5,13,13);
    ctx.fillStyle=col;
    if(pk.type==='hp'){ctx.fillRect(x-1,pk.y+bob-4,2,8);ctx.fillRect(x-4,pk.y+bob-1,8,2);}
    else{ctx.fillRect(x-1,pk.y+bob-4,3,4);ctx.fillRect(x-2,pk.y+bob,3,4);}
  }
}
function drawPlayer(cam){
  // afterimages
  ctx.globalCompositeOperation='lighter';
  for(var a=0;a<P.after.length;a++){var ai=P.after[a];var ax=ai.x-cam;
    ctx.fillStyle=rgba('#22d3ee',ai.life*1.4);
    ctx.fillRect(ax-5,ai.y-28,10,20);ctx.fillRect(ax-4,ai.y-32,8,5);}
  ctx.globalCompositeOperation='source-over';
  if(P.dead)return;
  if(P.if>0&&Math.floor(S.t*20)%2===0)return; // hurt flicker
  var x=Math.round(P.x-cam),y=Math.round(P.y);
  ctx.save();ctx.translate(x,y);ctx.scale(P.face,1);
  var run=Math.abs(P.vx)>30&&P.ground,ph=Math.sin(P.anim*2),bob=P.ground?(run?Math.abs(ph)*1.5:Math.sin(S.t*2)*.7):0;
  var lean=P.dashT>0?3:run?1:0;
  ctx.translate(lean,-bob);
  // legs
  ctx.fillStyle='#1b1f2e';
  if(!P.ground){ctx.fillRect(-4,-9,3,7);ctx.fillRect(1,-7,3,6);}
  else if(run){ctx.fillRect(-4+ph*3,-8,3,8);ctx.fillRect(1-ph*3,-8,3,8);}
  else{ctx.fillRect(-4,-8,3,8);ctx.fillRect(1,-8,3,8);}
  ctx.fillStyle='#0e1220';ctx.fillRect(-5+(run?ph*3:0),-2,4,2);ctx.fillRect(0-(run?ph*3:0),-2,4,2);
  // coat
  ctx.fillStyle='#5a3b2e';ctx.fillRect(-5,-24,10,16);
  ctx.fillRect(-6,-10,12,3); // flare
  ctx.fillStyle='#6d4a3a';ctx.fillRect(-5,-24,2,15);
  ctx.fillStyle='#2a2333';ctx.fillRect(-5,-13,10,2); // belt
  ctx.fillStyle='#ff2e88';ctx.fillRect(-4,-25,8,2); // scarf
  ctx.fillRect(-6,-24+Math.sin(S.t*6)*1,2,4);
  // head + hat
  ctx.fillStyle='#e8b98a';ctx.fillRect(-3,-31,7,6);
  ctx.fillStyle='#22d3ee';ctx.fillRect(2,-29,2,1); // visor eye
  ctx.fillStyle='#2a2333';ctx.fillRect(-5,-33,11,2);ctx.fillRect(-3,-37,8,4);
  ctx.fillStyle='#3a3158';ctx.fillRect(-3,-34,8,1);
  // arm + gun
  var shooting=P.muzzle>0||input.shoot;
  ctx.fillStyle='#5a3b2e';
  if(shooting){ctx.fillRect(2,-20,7,3);
    ctx.fillStyle='#141a2c';ctx.fillRect(8,-21,6,3);
    ctx.fillStyle='#22d3ee';ctx.fillRect(13,-20,2,1);
    if(P.muzzle>0){ctx.globalCompositeOperation='lighter';glow(ctx,16,-19,10,'#22d3ee',.8);
      ctx.fillStyle='#ffffff';ctx.fillRect(14,-20,4,2);ctx.globalCompositeOperation='source-over';}
  }else{ctx.fillRect(2,-22,3,7);ctx.fillStyle='#141a2c';ctx.fillRect(2,-16,4,3);}
  ctx.restore();
}
function drawEnemy(e,cam){
  if(e.dead)return;
  var x=Math.round(e.x-cam);if(x<-40||x>VW+40)return;
  var flash=e.flash>0;
  if(e.type==='drone'){
    var y=Math.round(e.y);
    ctx.globalCompositeOperation='lighter';
    glow(ctx,x,y+8,8,e.aggro?'#ff3b3b':'#22d3ee',.3);
    ctx.globalCompositeOperation='source-over';
    ctx.fillStyle=flash?'#ffffff':'#2b3550';ctx.fillRect(x-8,y-5,16,10);
    ctx.fillStyle=flash?'#ffffff':'#3d4a6e';ctx.fillRect(x-8,y-5,16,2);
    ctx.fillStyle='#141a2c';ctx.fillRect(x-9,y-2,2,4);ctx.fillRect(x+7,y-2,2,4);
    // rotors blur
    var ro=Math.sin(S.t*40+e.seed)*.5+.5;
    ctx.fillStyle=rgba('#9b93c9',.3+ro*.3);ctx.fillRect(x-11,y-7,7,1);ctx.fillRect(x+4,y-7,7,1);
    // eye
    var ec=e.aggro?'#ff3b3b':'#22d3ee';
    ctx.fillStyle=ec;ctx.fillRect(x-2,y-2,4,3);
    ctx.globalCompositeOperation='lighter';glow(ctx,x,y,6,ec,.6);ctx.globalCompositeOperation='source-over';
    ctx.fillStyle='#141a2c';ctx.fillRect(x-1,y-8,1,3);
    ctx.fillStyle=Math.sin(S.t*6+e.seed)>0?'#ff3b3b':'#5a1a1a';ctx.fillRect(x-1,y-9,1,1);
  }else if(e.type==='walker'){
    var wy=Math.round(e.y),ph2=Math.sin(S.t*(e.state==='charge'?18:8)+e.seed);
    ctx.fillStyle=flash?'#ffffff':'#1b1f2e';
    ctx.fillRect(x-5+ph2*3,wy-8,3,8);ctx.fillRect(2-ph2*3,wy-8,3,8);
    ctx.fillStyle=flash?'#ffffff':'#3d4a6e';ctx.fillRect(x-8,wy-22,16,14);
    ctx.fillStyle=flash?'#ffffff':'#2b3550';ctx.fillRect(x-8,wy-22,16,3);
    ctx.fillStyle='#141a2c';ctx.fillRect(x-9,wy-20,2,8);ctx.fillRect(x+7,wy-20,2,8);
    var ec2=e.state==='charge'?'#ffb020':e.state==='wind'?'#ffffff':'#ff3b3b';
    ctx.fillStyle=ec2;ctx.fillRect(x-5+(e.dir>0?2:-2),wy-18,8,2);
    ctx.globalCompositeOperation='lighter';glow(ctx,x,wy-17,7,ec2,.5);ctx.globalCompositeOperation='source-over';
    ctx.fillStyle='#2b3550';ctx.fillRect(x-6,wy-25,12,3);
  }else{
    var ty=Math.round(e.y);
    ctx.fillStyle='#141a2c';ctx.fillRect(x-10,ty-8,20,8);
    ctx.fillStyle=flash?'#ffffff':'#2b3550';ctx.fillRect(x-7,ty-16,14,8);
    ctx.save();ctx.translate(x,ty-14);ctx.rotate(e.angle);
    ctx.fillStyle=flash?'#ffffff':'#3d4a6e';ctx.fillRect(0,-2,12,4);
    ctx.fillStyle='#ffb020';ctx.fillRect(10,-1,3,2);ctx.restore();
    ctx.fillStyle=e.burst>0?'#ffffff':'#ff3b3b';ctx.fillRect(x-2,ty-15,4,3);
    ctx.globalCompositeOperation='lighter';glow(ctx,x,ty-14,6,'#ffb020',.4);ctx.globalCompositeOperation='source-over';
  }
}
function drawBoss(b,cam){
  var x=Math.round(b.x-cam),y=Math.round(b.y);if(x<-80||x>VW+80)return;
  var flash=b.flash>0,wind=b.wind>0;
  var coreCol=b.phase===3?'#ff3b3b':b.phase===2?'#ff2e88':'#c026d3';
  ctx.globalCompositeOperation='lighter';glow(ctx,x,y+24,20,'#22d3ee',.25);ctx.globalCompositeOperation='source-over';
  // hull
  ctx.fillStyle=flash?'#ffffff':'#241d3a';ctx.fillRect(x-28,y-20,56,40);
  ctx.fillStyle=flash?'#ffffff':'#3a3158';ctx.fillRect(x-28,y-20,56,5);
  ctx.fillStyle='rgba(0,0,0,.35)';ctx.fillRect(x+22,y-20,6,40);
  ctx.fillStyle=flash?'#ffffff':'#2b3550';ctx.fillRect(x-24,y-24,48,5);
  // shoulder cannons
  ctx.fillStyle='#141a2c';ctx.fillRect(x-32,y-12,8,10);ctx.fillRect(x+24,y-12,8,10);
  ctx.fillStyle=coreCol;ctx.fillRect(x-31,y-8,3,3);ctx.fillRect(x+28,y-8,3,3);
  // eye strip
  var ec=wind?'#ffffff':coreCol;
  ctx.fillStyle=ec;ctx.fillRect(x-16,y-14,32,4);
  ctx.globalCompositeOperation='lighter';glow(ctx,x,y-12,wind?22:14,ec,wind?.8:.5);
  // core
  var cp=.5+.4*Math.sin(S.t*(b.phase===3?10:5));
  glow(ctx,x,y+6,16,coreCol,.5*cp+.2);ctx.globalCompositeOperation='source-over';
  ctx.fillStyle='#0a0714';ctx.fillRect(x-7,y-1,14,14);
  ctx.fillStyle=coreCol;ctx.fillRect(x-5,y+1,10,10);
  ctx.fillStyle='#ffffff';ctx.fillRect(x-2,y+4,4,4);
  // armor lines
  ctx.strokeStyle='rgba(155,147,201,.3)';ctx.strokeRect(x-28.5,y-20.5,57,41);
  ctx.beginPath();ctx.moveTo(x-28,y);ctx.lineTo(x-8,y);ctx.moveTo(x+8,y);ctx.lineTo(x+28,y);ctx.stroke();
  pText(ctx,'W-09',x-pWidth('W-09',1)/2,y+16,'rgba(155,147,201,.6)',1);
}
function drawBolts(cam){
  ctx.globalCompositeOperation='lighter';
  var i,b,x;
  for(i=0;i<Wd.bolts.length;i++){b=Wd.bolts[i];x=b.x-cam;
    var l=Math.hypot(b.vx,b.vy)||1,nx=b.vx/l,ny=b.vy/l;
    var g=ctx.createLinearGradient(x-nx*10,b.y-ny*10,x,b.y);
    g.addColorStop(0,rgba('#22d3ee',0));g.addColorStop(1,rgba('#22d3ee',.9));
    ctx.strokeStyle=g;ctx.lineWidth=2;ctx.beginPath();ctx.moveTo(x-nx*10,b.y-ny*10);ctx.lineTo(x,b.y);ctx.stroke();
    ctx.fillStyle='#ffffff';ctx.fillRect(x-1,b.y-1,2,2);glow(ctx,x,b.y,5,'#22d3ee',.6);}
  for(i=0;i<Wd.ebolts.length;i++){b=Wd.ebolts[i];x=b.x-cam;
    ctx.fillStyle=b.col;ctx.fillRect(x-2,b.y-2,4,4);
    ctx.fillStyle='#ffffff';ctx.fillRect(x-1,b.y-1,2,2);glow(ctx,x,b.y,6,b.col,.6);}
  ctx.globalCompositeOperation='source-over';ctx.lineWidth=1;
}
function drawParts(cam){
  for(var i=0;i<Wd.parts.length;i++){var p=Wd.parts[i];var x=p.x-cam;if(x<-50||x>VW+50)continue;
    var a=p.life/p.max;
    if(p.t==='sp'){ctx.globalCompositeOperation='lighter';ctx.fillStyle=rgba(p.col,a);
      ctx.fillRect(x-p.sz/2,p.y-p.sz/2,p.sz,p.sz);ctx.globalCompositeOperation='source-over';}
    else if(p.t==='db'){ctx.fillStyle=rgba(p.col,a);ctx.fillRect(x,p.y,p.sz,p.sz);}
    else if(p.t==='sm'){ctx.fillStyle=rgba(p.col,a*.22);
      ctx.beginPath();ctx.arc(x,p.y,p.sz,0,7);ctx.fill();}
    else if(p.t==='rg'){ctx.globalCompositeOperation='lighter';ctx.strokeStyle=rgba(p.col,a);
      ctx.lineWidth=2;ctx.beginPath();ctx.arc(x,p.y,p.sz*(1-a)+4,0,7);ctx.stroke();
      ctx.lineWidth=1;ctx.globalCompositeOperation='source-over';}
    else if(p.t==='fl'){ctx.globalCompositeOperation='lighter';glow(ctx,x,p.y,p.sz,p.col,a*.9);
      ctx.globalCompositeOperation='source-over';}
  }
}
function drawFloats(cam){
  for(var i=0;i<Wd.floats.length;i++){var f=Wd.floats[i];var x=f.x-cam;
    var a=Math.min(1,f.life/f.max*2);
    pText(ctx,f.str,Math.round(x-pWidth(f.str,1)/2),Math.round(f.y),rgba(f.col==='#ffffff'?'#ffffff':f.col,a),1);}
}

/* ================= HUD ================= */
var hudCache={};
function setTxt(id,v){if(hudCache[id]!==v){hudCache[id]=v;$(id).textContent=v}}
function updateHUD(){
  $('hpFill').style.transform='scaleX('+(P.hp/100)+')';
  $('enFill').style.transform='scaleX('+(P.en/100)+')';
  setTxt('hpNum',Math.ceil(P.hp));setTxt('enNum',Math.floor(P.en));
  setTxt('scoreVal',String(S.score));
  var cw=$('comboWrap');
  if(S.combo>=2){cw.classList.add('on');setTxt('comboVal','×'+comboMult());
    $('comboBar').firstElementChild.style.width=(S.comboT/3*100)+'%';}
  else cw.classList.remove('on');
  if(Wd.idx===2)setTxt('objVal',S.bossDead?'✔ اهروب عبر البوابة':'🎯 دمّر الحارس WARDEN-09');
  else setTxt('objVal',S.gateOpen?'✔ البوابة مفتوحة — EXIT':'الأهداف '+S.kills+'/'+Wd.quota);
  var pct=clamp(P.x/Wd.len*100,0,100);
  $('progFill').style.width=pct+'%';$('progMark').style.left='calc('+pct+'% - 1px)';
  $('progGate').style.background=S.gateOpen?'#34d399':'#ff3b3b';
  $('progGate').style.boxShadow='0 0 6px '+(S.gateOpen?'#34d399':'#ff3b3b');
  if(Wd.boss&&!Wd.boss.dead&&P.x>Wd.arenaStart-100){$('bossWrap').classList.add('on');
    $('bossFill').style.width=Math.max(0,Wd.boss.hp/Wd.boss.max*100)+'%';}
  else if(!Wd.boss||Wd.boss.dead)$('bossWrap').classList.remove('on');
}

/* ================= FLOW ================= */
var toastTimer=null;
function toast(msg,cls){
  var t=$('toast');t.textContent=msg;t.className=cls||'';
  void t.offsetWidth;t.classList.add('show');
  clearTimeout(toastTimer);toastTimer=setTimeout(function(){t.classList.remove('show')},2400);
}
function setState(st){
  S.state=st;document.body.dataset.state=st;
  $('ovTitle').hidden=st!=='title';$('ovPause').hidden=st!=='paused';
  $('ovClear').hidden=st!=='clear';$('ovOver').hidden=st!=='over';$('ovWin').hidden=st!=='win';
  $('btnPause').disabled=!(st==='playing'||st==='paused');
  $('pauseIc').textContent=st==='paused'?'▶':'⏸';
  if(AC&&musicG)musicG.gain.linearRampToValueAtTime(st==='paused'?.12:.5,AC.currentTime+.3);
}
function startSector(idx){
  S.sector=idx;Wd=buildSector(idx);resetPlayer();
  S.kills=0;S.chips=0;S.combo=0;S.comboT=0;S.time=0;S.sectorStart=S.score;
  S.bossDead=false;S.gateOpen=false;S.camX=0;S.deadT=0;S.freeze=0;S.slowT=0;S.ts=1;
  hudCache={};
  $('sectorTag').innerHTML='القطاع 0'+(idx+1)+' <em>// '+Wd.pal.name+'</em>';
  setState('playing');
  toast('القطاع 0'+(idx+1)+' — '+Wd.pal.name+' // '+Wd.pal.sub,'big');
  sfx.gate();
}
function startRun(){S.score=0;S.totals={kills:0,chips:0,time:0};startSector(0)}
function sectorClear(){
  if(S.state!=='playing')return;
  var bonus=Math.max(0,1200-Math.floor(S.time)*8);
  S.score+=bonus;S.totals.kills+=S.kills;S.totals.chips+=S.chips;S.totals.time+=S.time;
  sfx.clear();
  if(S.sector>=2){winGame();return;}
  $('clearName').textContent='تم تأمين '+Wd.pal.name;
  $('stTime').textContent=Math.floor(S.time)+'s';
  $('stKills').textContent=String(S.kills);
  $('stChips').textContent=String(S.chips);
  $('stBonus').textContent='+'+bonus;
  setState('clear');
}
function saveBest(){if(S.score>S.best){S.best=S.score;LSset('nd_best',String(S.best));}}
function gameOver(){
  saveBest();
  $('ovScore').textContent=String(S.score);$('ovBest').textContent=String(S.best);
  setState('over');
}
function winGame(){
  saveBest();
  var rank=S.score>=11000?'S':S.score>=8500?'A':S.score>=6000?'B':'C';
  $('rankGlyph').textContent=rank;
  $('rankGlyph').style.color=rank==='S'?'#ffb020':rank==='A'?'#ff2e88':rank==='B'?'#22d3ee':'#9b93c9';
  $('wScore').textContent=String(S.score);
  $('wTime').textContent=Math.floor(S.totals.time)+'s';
  $('wKills').textContent=String(S.totals.kills);
  $('wChips').textContent=String(S.totals.chips);
  setState('win');sfx.win();
  for(var i=0;i<40;i++)spark(P.x+rnd(-60,60),P.y-rnd(0,80),['#ff2e88','#22d3ee','#ffb020','#34d399'][i%4],1,160);
}
function retrySector(){S.score=S.sectorStart;startSector(S.sector)}
function toMenu(){
  saveBest();$('bestVal').textContent=String(S.best);
  Wd=buildSector(0);resetPlayer();S.camX=0;setState('title');
}
function pauseGame(){if(S.state==='playing'){setState('paused');sfx.ui();}}
function resumeGame(){if(S.state==='paused'){setState('playing');sfx.ui();}}

/* ================= MAIN LOOP ================= */
var last=performance.now(),acc=0,running=true;
function loop(now){
  requestAnimationFrame(loop);
  var dt=Math.min(.05,(now-last)/1000);last=now;
  // real-time timers
  if(S.freeze>0){S.freeze-=dt;}
  if(S.slowT>0){S.slowT-=dt;if(S.slowT<=0)S.ts=1;}
  var st=S.state;
  if(st==='playing'){
    if(S.freeze<=0){
      acc+=dt*S.ts;
      var guard=0;
      while(acc>=STEP&&guard<5){
        updatePlaying(STEP);acc-=STEP;guard++;
      }
      if(guard>=5)acc=0;
    }
    updateAmbient(dt*S.ts);
    updateHUD();
    if(P.dead){S.deadT-=dt;if(S.deadT<=0)gameOver();}
  }else if(st==='title'){
    updateAmbient(dt);
    S.camX=(Math.sin(S.t*.06)*.5+.5)*(Wd.len-VW)*.35;
  }else if(st==='clear'||st==='over'||st==='win'){
    updateAmbient(dt);updateParts(dt);
  }
  render();
}
function updatePlaying(dt){
  S.time+=dt;
  S.comboT-=dt;if(S.comboT<=0){S.combo=0;}
  updatePlayer(dt);
  updateEnemies(dt);
  updateBoss(dt);
  updateBolts(dt);
  updateChips(dt);
  updateParts(dt);
  updateTriggers();
  updateCamera(dt);
  if(!P.dead)updateGate(dt);
}

/* ================= INPUT ================= */
var KEYMAP={ArrowLeft:'left',KeyA:'left',ArrowRight:'right',KeyD:'right',
  ArrowUp:'jump',KeyW:'jump',Space:'jump',KeyJ:'shoot',KeyX:'shoot',KeyK:'dash',ShiftLeft:'dash',ShiftRight:'dash'};
window.addEventListener('keydown',function(e){
  unlockAudio();
  if(e.code==='KeyM'){setMuted(!muted);return;}
  if(e.code==='KeyP'||e.code==='Escape'){
    if(S.state==='playing')pauseGame();else if(S.state==='paused')resumeGame();return;}
  if(e.code==='Enter'){
    if(S.state==='title')startRun();
    else if(S.state==='clear')startSector(S.sector+1);
    else if(S.state==='over')retrySector();
    else if(S.state==='win')startRun();
    else if(S.state==='paused')resumeGame();
    return;}
  if(e.code==='KeyR'&&(S.state==='over'||S.state==='win')){startRun();return;}
  var k=KEYMAP[e.code];
  if(k){e.preventDefault();
    if(k==='jump'&&!input.jump)input.jump=true;
    else if(k==='dash'&&!input.dash)input.dash=true;
    else if(k!=='jump'&&k!=='dash')input[k]=true;
  }
  if(['Space','ArrowUp','ArrowDown','ArrowLeft','ArrowRight'].indexOf(e.code)>=0)e.preventDefault();
});
window.addEventListener('keyup',function(e){
  var k=KEYMAP[e.code];if(k&&k!=='jump'&&k!=='dash')input[k]=false;
});
// touch buttons
document.querySelectorAll('.tc').forEach(function(btn){
  var k=btn.dataset.k,ids={};
  function down(e){e.preventDefault();unlockAudio();
    ids[e.pointerId]=true;btn.classList.add('held');
    try{btn.setPointerCapture(e.pointerId)}catch(err){}
    if(k==='jump')input.jump=true;else if(k==='dash')input.dash=true;else input[k]=true;}
  function up(e){e.preventDefault();delete ids[e.pointerId];
    if(Object.keys(ids).length===0){btn.classList.remove('held');
      if(k==='left'||k==='right'||k==='shoot')input[k]=false;}}
  btn.addEventListener('pointerdown',down);
  btn.addEventListener('pointerup',up);
  btn.addEventListener('pointercancel',up);
  btn.addEventListener('lostpointercapture',up);
  btn.addEventListener('contextmenu',function(e){e.preventDefault()});
});
document.getElementById('frame').addEventListener('contextmenu',function(e){e.preventDefault()});
window.addEventListener('pointerdown',unlockAudio,{passive:true});
document.addEventListener('visibilitychange',function(){
  if(document.hidden&&S.state==='playing')pauseGame();
  if(!document.hidden&&AC&&AC.state==='suspended')AC.resume();
});
window.addEventListener('blur',function(){if(S.state==='playing')pauseGame()});

/* ================= UI WIRING ================= */
function bind(id,fn){$(id).addEventListener('click',function(e){unlockAudio();sfx.ui();fn();e.currentTarget.blur()})}
bind('btnStart',startRun);
bind('btnResume',resumeGame);
bind('btnRetryP',retrySector);
bind('btnMenuP',toMenu);
bind('btnNext',function(){startSector(S.sector+1)});
bind('btnRetry',retrySector);
bind('btnMenuO',toMenu);
bind('btnAgain',startRun);
bind('btnMenuW',toMenu);
bind('btnPause',function(){if(S.state==='playing')pauseGame();else if(S.state==='paused')resumeGame()});
$('btnSound').addEventListener('click',function(){unlockAudio();setMuted(!muted);sfx.ui();this.blur()});
$('btnFull').addEventListener('click',function(){
  unlockAudio();sfx.ui();
  var el=document.getElementById('app');
  if(!document.fullscreenElement){if(el.requestFullscreen)el.requestFullscreen().catch(function(){});}
  else if(document.exitFullscreen)document.exitFullscreen();
  this.blur();
});
document.querySelectorAll('#diffPills .pill').forEach(function(p){
  p.addEventListener('click',function(){
    unlockAudio();sfx.ui();S.diff=parseInt(p.dataset.d,10);LSset('nd_diff',String(S.diff));
    document.querySelectorAll('#diffPills .pill').forEach(function(q){q.setAttribute('aria-pressed',q===p?'true':'false')});
    p.blur();
  });
});
// init pills
document.querySelectorAll('#diffPills .pill').forEach(function(q){
  q.setAttribute('aria-pressed',parseInt(q.dataset.d,10)===S.diff?'true':'false')});
$('bestVal').textContent=String(S.best);
setMuted(muted);
if(coarse&&window.matchMedia('(orientation:portrait)').matches){
  setTimeout(function(){toast('💡 أدر الهاتف أفقياً لتجربة أفضل','',1)},1200);
}

/* ================= BOOT ================= */
Wd=buildSector(0);resetPlayer();setState('title');
requestAnimationFrame(function(t){last=t;requestAnimationFrame(loop)});
})();
</script>
</body>
</html>
