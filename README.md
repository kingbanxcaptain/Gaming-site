# Gaming-site
All about ur score 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>High Scores</title>
<style>
  :root{
    --bg: #10101a;
    --panel: #1a1a28;
    --panel-2: #21213380;
    --line: #33334a;
    --text: #eceaf7;
    --muted: #8888a3;
    --gold: #f2b705;
    --silver: #c9ccd8;
    --bronze: #d97a3f;
    --accent: #7c5cff;
    --danger: #ff5c7a;
  }
  *{ box-sizing: border-box; }
  html,body{ margin:0; padding:0; }
  body{
    background:
      radial-gradient(600px 300px at 15% -10%, #251e4a55, transparent),
      radial-gradient(500px 260px at 100% 0%, #3a234f55, transparent),
      var(--bg);
    color: var(--text);
    font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
    min-height: 100vh;
    padding: 28px 16px 60px;
  }
  .wrap{ max-width: 560px; margin: 0 auto; }

  .title-row{
    display:flex;
    align-items: baseline;
    justify-content: space-between;
    margin-bottom: 22px;
  }
  h1{
    font-size: 28px;
    letter-spacing: 0.5px;
    margin: 0;
    font-weight: 800;
  }
  h1 span{ color: var(--accent); }
  .subtitle{
    color: var(--muted);
    font-size: 13px;
    margin-top: 4px;
  }

  .podium{
    display:grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 10px;
    margin-bottom: 26px;
    align-items: end;
  }
  .podium-slot{
    background: var(--panel);
    border: 1px solid var(--line);
    border-radius: 10px;
    padding: 14px 8px 12px;
    text-align: center;
    position: relative;
  }
  .podium-slot.p1{ order:2; padding-top: 20px; border-color: #f2b70555; }
  .podium-slot.p2{ order:1; }
  .podium-slot.p3{ order:3; }
  .rank-badge{
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.5px;
    padding: 2px 8px;
    border-radius: 20px;
    display: inline-block;
    margin-bottom: 8px;
  }
  .p1 .rank-badge{ background: #f2b70522; color: var(--gold); }
  .p2 .rank-badge{ background: #c9ccd822; color: var(--silver); }
  .p3 .rank-badge{ background: #d97a3f22; color: var(--bronze); }
  .podium-name{
    font-weight: 700;
    font-size: 14px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .podium-score{
    font-variant-numeric: tabular-nums;
    font-size: 18px;
    font-weight: 800;
    margin-top: 4px;
  }
  .p1 .podium-score{ color: var(--gold); }
  .p2 .podium-score{ color: var(--silver); }
  .p3 .podium-score{ color: var(--bronze); }
  .empty-slot{ color: var(--muted); font-size: 13px; padding: 10px 0; }

  .board{
    background: var(--panel);
    border: 1px solid var(--line);
    border-radius: 12px;
    overflow: hidden;
    margin-bottom: 22px;
  }
  .row{
    display:flex;
    align-items:center;
    gap: 12px;
    padding: 12px 14px;
    border-bottom: 1px solid var(--line);
  }
  .row:last-child{ border-bottom:none; }
  .rank{
    width: 26px;
    text-align:center;
    font-weight: 800;
    color: var(--muted);
    font-variant-numeric: tabular-nums;
  }
  .rank.top{ color: var(--gold); }
  .name{
    flex:1;
    font-weight: 600;
    font-size: 15px;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap;
  }
  .score{
    font-weight: 800;
    font-variant-numeric: tabular-nums;
    font-size: 15px;
  }
  .del{
    background:none;
    border:none;
    color: var(--muted);
    font-size: 16px;
    cursor:pointer;
    padding: 4px 6px;
    line-height:1;
    border-radius: 6px;
  }
  .del:hover{ color: var(--danger); background: #ff5c7a15; }

  .empty-board{
    padding: 30px 16px;
    text-align:center;
    color: var(--muted);
    font-size: 14px;
  }

  form{
    display:flex;
    gap: 8px;
    flex-wrap: wrap;
  }
  input{
    background: var(--panel);
    border: 1px solid var(--line);
    color: var(--text);
    border-radius: 8px;
    padding: 12px 12px;
    font-size: 15px;
    outline: none;
  }
  input:focus{ border-color: var(--accent); }
  input[name="name"]{ flex: 1 1 140px; min-width: 0; }
  input[name="score"]{ width: 110px; }
  button.submit{
    background: var(--accent);
    color: white;
    border: none;
    border-radius: 8px;
    padding: 12px 18px;
    font-weight: 700;
    font-size: 15px;
    cursor: pointer;
    flex: 1 1 100%;
  }
  button.submit:active{ transform: translateY(1px); }

  .status{
    text-align:center;
    color: var(--muted);
    font-size: 12px;
    margin-top: 16px;
  }
</style>
</head>
<body>
  <div class="wrap">
    <div class="title-row">
      <div>
        <h1>High<span>Scores</span></h1>
        <div class="subtitle">Shared leaderboard — visible to everyone</div>
      </div>
    </div>

    <div class="podium" id="podium"></div>
    <div class="board" id="board"></div>

    <form id="scoreForm">
      <input type="text" name="name" placeholder="Player name" maxlength="20" required />
      <input type="number" name="score" placeholder="Score" required />
      <button type="submit" class="submit">Add score</button>
    </form>

    <div class="status" id="status">Loading leaderboard…</div>
  </div>

<script>
const STORAGE_KEY = "leaderboard-entries";
let entries = [];

const podiumEl = document.getElementById('podium');
const boardEl = document.getElementById('board');
const statusEl = document.getElementById('status');
const form = document.getElementById('scoreForm');

function medalLabel(i){
  return ["1ST","2ND","3RD"][i] || "";
}

function render(){
  const sorted = [...entries].sort((a,b) => b.score - a.score);

  // Podium (top 3)
  podiumEl.innerHTML = "";
  const order = [1,0,2]; // display order: 2nd, 1st, 3rd
  order.forEach((idx, visualPos) => {
    const e = sorted[idx];
    const slot = document.createElement('div');
    slot.className = 'podium-slot p' + (idx+1);
    if(e){
      slot.innerHTML = `
        <div class="rank-badge">${medalLabel(idx)}</div>
        <div class="podium-name">${escapeHtml(e.name)}</div>
        <div class="podium-score">${e.score}</div>
      `;
    } else {
      slot.innerHTML = `<div class="empty-slot">—</div>`;
    }
    podiumEl.appendChild(slot);
  });

  // Full board
  if(sorted.length === 0){
    boardEl.innerHTML = `<div class="empty-board">No scores yet. Be the first to play.</div>`;
  } else {
    boardEl.innerHTML = sorted.map((e, i) => `
      <div class="row">
        <div class="rank ${i < 3 ? 'top' : ''}">${i+1}</div>
        <div class="name">${escapeHtml(e.name)}</div>
        <div class="score">${e.score}</div>
        <button class="del" data-id="${e.id}" title="Remove">✕</button>
      </div>
    `).join("");

    boardEl.querySelectorAll('.del').forEach(btn => {
      btn.addEventListener('click', () => removeEntry(btn.dataset.id));
    });
  }
}

function escapeHtml(str){
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

async function loadEntries(){
  try{
    const result = await window.storage.get(STORAGE_KEY, true);
    entries = result ? JSON.parse(result.value) : [];
    statusEl.textContent = entries.length ? "Synced" : "No scores yet — add one below";
  } catch (err){
    entries = [];
    statusEl.textContent = "Starting a fresh leaderboard";
  }
  render();
}

async function saveEntries(){
  try{
    const result = await window.storage.set(STORAGE_KEY, JSON.stringify(entries), true);
    if(!result){
      statusEl.textContent = "Couldn't save — try again";
      return false;
    }
    statusEl.textContent = "Synced";
    return true;
  } catch (err){
    statusEl.textContent = "Couldn't save — try again";
    return false;
  }
}

async function removeEntry(id){
  entries = entries.filter(e => e.id !== id);
  render();
  await saveEntries();
}

form.addEventListener('submit', async (ev) => {
  ev.preventDefault();
  const data = new FormData(form);
  const name = data.get('name').trim();
  const score = Number(data.get('score'));
  if(!name || Number.isNaN(score)) return;

  entries.push({ id: Date.now() + "-" + Math.random().toString(36).slice(2,7), name, score });
  render();
  form.reset();
  statusEl.textContent = "Saving…";
  await saveEntries();
});

loadEntries();
</script>
</body>
</html>
