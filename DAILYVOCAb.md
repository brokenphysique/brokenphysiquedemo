<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Daily 10 Vocabulary</title>
<style>
  :root{
    --bg:#0f1724; --card:#0b1220; --accent:#60a5fa; --accent2:#7c3aed;
    --text:#e6eefc; --muted:#9aa7c7; --glass: rgba(255,255,255,0.03);
    --radius:12px;
  }
  *{box-sizing:border-box}
  body{margin:0;min-height:100vh;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,Helvetica,Arial;background: linear-gradient(180deg,#071023 0%, #082033 60%);color:var(--text);display:flex;align-items:center;justify-content:center;padding:28px;}
  .wrap{width:100%;max-width:980px;}
  header{display:flex;align-items:center;gap:14px;margin-bottom:18px}
  .logo{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--accent),var(--accent2));display:flex;align-items:center;justify-content:center;font-weight:900;color:#05212a}
  h1{margin:0;font-size:1.4rem}
  .subtitle{color:var(--muted);font-size:0.95rem;margin-top:4px}
  .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:14px;border-radius:var(--radius); border:1px solid rgba(255,255,255,0.04); box-shadow:0 8px 30px rgba(2,6,23,0.6)}
  .controls{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
  button{background:var(--accent);border:none;color:#05212a;font-weight:700;padding:8px 12px;border-radius:10px;cursor:pointer}
  button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--text)}
  .small{font-size:0.9rem;color:var(--muted)}
  .grid{display:grid;grid-template-columns:1fr 360px;gap:16px;margin-top:16px}
  @media (max-width:900px){ .grid{grid-template-columns:1fr} }
  .list{display:flex;flex-direction:column;gap:8px}
  .word-row{display:flex;gap:12px;align-items:flex-start;padding:12px;border-radius:10px;background:linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.005));border:1px solid rgba(255,255,255,0.02)}
  .word-left{width:220px}
  .word{font-weight:800;font-size:1.02rem}
  .pos{font-size:0.85rem;color:var(--muted);margin-left:6px;font-weight:600}
  .def{color:var(--muted);margin-top:6px}
  .example{margin-top:8px;color:#cfe8ff;font-size:0.95rem}
  .right-col{display:flex;flex-direction:column;gap:12px}
  .progress{display:flex;justify-content:space-between;align-items:center}
  .meter{height:12px;background:rgba(255,255,255,0.03);border-radius:999px;overflow:hidden;margin-top:8px}
  .meter > i{display:block;height:100%;background:linear-gradient(90deg,var(--accent),var(--accent2));width:0%}
  .actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:10px}
  .muted-block{color:var(--muted);font-size:0.9rem}
  footer{margin-top:18px;text-align:center;color:var(--muted);font-size:0.9rem}
  input[type="checkbox"]{transform:scale(1.12)}
  .badge{display:inline-block;padding:6px 8px;border-radius:999px;background:rgba(255,255,255,0.02);font-weight:700;color:var(--accent2)}
  .tiny{font-size:0.82rem;color:var(--muted)}
  .exportArea{width:100%;height:80px;padding:8px;border-radius:8px;background:rgba(0,0,0,0.05);border:1px dashed rgba(255,255,255,0.02);color:var(--muted);resize:none}
</style>
</head>
<body>
<div class="wrap">
<header>
  <div class="logo">V10</div>
  <div>
    <h1>Daily 10 Vocabulary</h1>
    <div class="subtitle">Learn 10 fresh words every day — mark known words and track progress.</div>
  </div>
</header>

<div class="grid">
  <section class="card">
    <div style="display:flex;justify-content:space-between;align-items:center">
      <div>
        <div class="small" id="dateLabel">Date</div>
        <div style="font-weight:800;font-size:1.1rem" id="setLabel">Today's words</div>
      </div>
      <div style="text-align:right">
        <div class="badge" id="setId">Set #</div>
        <div class="tiny" style="margin-top:6px" id="seedInfo"></div>
      </div>
    </div>

    <div style="margin-top:12px" class="controls">
      <button id="refreshBtn" class="ghost">Show today's set</button>
      <button id="randomNowBtn">Random 10 (not daily)</button>
      <button id="resetKnownBtn" class="ghost">Reset known</button>
      <div style="flex:1"></div>
      <div class="small">Auto-generated from built-in bank</div>
    </div>

    <div style="margin-top:12px" id="wordsList" class="list" aria-live="polite"></div>
  </section>

  <aside class="right-col">
    <div class="card">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div>
          <div class="small">Your progress</div>
          <div style="font-weight:800;font-size:1.1rem" id="knownCount">0 / 10 known</div>
        </div>
        <div style="text-align:right">
          <div class="small">Streak</div>
          <div style="font-weight:800" id="streakCount">0</div>
        </div>
      </div>

      <div style="margin-top:10px">
        <div class="meter"><i id="meterBar" style="width:0%"></i></div>
        <canvas id="streakChart" width="300" height="100" style="margin-top:12px; background: rgba(255,255,255,0.02); border-radius:8px;"></canvas>
        <div class="muted-block tiny" style="margin-top:8px">Check the box next to a word when you know it. Progress saved to your browser.</div>
      </div>

      <div class="actions">
        <button id="exportBtn" class="ghost">Export known</button>
        <button id="importBtn" class="ghost">Import JSON</button>
        <button id="copyBtn" class="ghost">Copy today's list</button>
      </div>

      <textarea id="exportArea" class="exportArea" placeholder='Exported JSON appears here (you can paste JSON here to import)'></textarea>
    </div>

    <div class="card">
      <div style="font-weight:800">How it works</div>
      <ol style="margin:8px 0 0;padding-left:18px;color:var(--muted)">
        <li>Every calendar day gets a deterministic set of 10 words (based on date seed).</li>
        <li>Click “Random 10” for a non-daily random set (useful for extra practice).</li>
        <li>Mark words as known — they persist in your browser storage.</li>
      </ol>
    </div>
  </aside>
</div>

<footer>
  Built for daily learning • Words bank: 120+ words • Local only (no servers)
</footer>
</div>

<script>
// …Insert all your JS here (the same script you wrote above, 
// but include the updated updateProgress() and renderChart() for streak + today vs yesterday)
</script>
</body>
</html>
