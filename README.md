

<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Quick Answer</title>
<style>
:root{--bg:#faf7f2;--card:#fff;--text:#1f1b16;--muted:#7a7268;--accent:#e4572e;--line:#e6dfd4;color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root{--bg:#17140f;--card:#241f18;--text:#f4eee4;--muted:#a69d90;--accent:#ff7a4d;--line:#3a3329;color-scheme:dark}}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--text);font:16px/1.4 system-ui,-apple-system,"Segoe UI",sans-serif}
main{max-width:640px;margin:0 auto;padding:20px 16px 40px}
h1{font-size:28px;margin:8px 0 4px}
.sub{color:var(--muted);margin:0 0 20px}
.card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:16px;margin-bottom:16px}
label{display:block;font-weight:600;margin-bottom:8px}
input{width:100%;font:inherit;color:var(--text);background:var(--bg);border:1px solid var(--line);border-radius:10px;padding:12px}
input:focus{outline:2px solid var(--accent);outline-offset:1px}
button{font:inherit;font-weight:600;border:0;border-radius:10px;padding:12px 18px;cursor:pointer;background:var(--accent);color:#fff}
button.ghost{background:transparent;color:var(--text);border:1px solid var(--line)}
button:disabled{opacity:.4;cursor:not-allowed}
.row{display:flex;gap:8px;align-items:center}
.row>*{flex:1}
.hidden{display:none}
.err{color:var(--accent);margin:8px 0 0;min-height:1.2em}
.code{display:inline-block;font:800 30px/1 ui-monospace,Menlo,monospace;letter-spacing:.2em;background:var(--card);border:1px dashed var(--accent);border-radius:10px;padding:8px 14px}
.chips{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px}
.chip{background:var(--bg);border:1px solid var(--line);border-radius:99px;padding:6px 12px}
.chip.me{border-color:var(--accent)}
.plist{max-height:220px;overflow:auto;margin-top:12px}
.plist .chip{cursor:pointer;font:inherit;color:var(--text);text-align:left;font-weight:400}
.prompt{font-size:26px;font-weight:700;text-align:center;margin:8px 0 16px}
.timer{font-size:56px;font-weight:800;text-align:center;font-variant-numeric:tabular-nums;color:var(--accent)}
.bar{height:8px;background:var(--line);border-radius:99px;overflow:hidden;margin:6px 0 20px}
.bar i{display:block;height:100%;width:100%;background:var(--accent);transition:width .25s linear}
.big{font-size:20px}
.muted{color:var(--muted)}
.ans{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:14px 16px;margin-bottom:12px;opacity:0;transform:translateY(12px);animation:in .5s forwards}
.ans small{color:var(--muted);font-weight:600;text-transform:uppercase;letter-spacing:.05em}
.ans div{font-size:22px;font-weight:700;margin-top:2px;word-break:break-word}
.ans .none{color:var(--muted);font-weight:400;font-style:italic}
@keyframes in{to{opacity:1;transform:none}}
</style>
</head>
<body>
<main>
<h1>Dont say the same as someone else</h1>
<p class="sub">Thinkin outside the box might help. When the timer ends, all answers are revealed.</p>

<section id="join">
  <div class="card">
    <label for="name">Your name</label>
    <input id="name" maxlength="20" placeholder="e.g. Sam">
  </div>
  <div class="card">
    <div class="row"><button id="create">Create a room</button></div>
    <p class="muted" style="text-align:center;margin:12px 0">or join your friends</p>
    <div class="row"><input id="code" maxlength="4" placeholder="Room code" style="text-transform:uppercase;letter-spacing:.2em"><button id="joinbtn" class="ghost">Join</button></div>
    <div class="err" id="err"></div>
  </div>
</section>

<section id="room" class="hidden">
  <div style="text-align:center;margin-bottom:16px">
    <span class="muted">Room code</span><br><span class="code" id="roomcode"></span><br>
    <button class="ghost" id="copy" style="margin-top:10px;padding:8px 14px">Copy invite link</button>
  </div>

  <div id="v-nohost" class="card hidden">
    <p class="big" id="nohostmsg" style="margin:0 0 12px">Connecting…</p>
    <button class="ghost" onclick="location.reload()">Back to start</button>
  </div>

  <div id="v-lobby" class="hidden">
    <div class="card">
      <label>Players (<span id="pcount">0</span>)</label>
      <div class="chips" id="players"></div>
    </div>
    <div class="card hidden" id="hostctl">
      <label>Category</label>
      <div class="row" style="margin-bottom:14px"><button id="catGen">🌍 General knowledge</button><button id="catMc" class="ghost">⛏️ Minecraft</button></div>
      <label for="prompt">Prompt</label>
      <input id="prompt" maxlength="100" placeholder="e.g. Name an animal">
      <div class="row" style="margin-top:10px">
        <button class="ghost" id="rand">🎲 Random prompt</button>
        <button class="ghost" id="browse">Browse all</button>
      </div>
      <div class="plist chips hidden" id="plist"></div>
      <label for="dur" style="margin-top:14px">Timer (seconds)</label>
      <input id="dur" type="number" inputmode="numeric" min="5" max="300" value="20">
      <div class="row" style="margin-top:12px"><button id="start" disabled>Start round</button></div>
    </div>
    <div class="card hidden" id="guestwait"><p class="big" style="margin:0" id="waitmsg">Waiting for the host to pick a prompt…</p></div>
  </div>

  <div id="v-play" class="hidden">
    <div class="prompt" id="pshow"></div>
    <div class="timer" id="time">20</div>
    <div class="bar"><i id="fill"></i></div>
    <div class="card">
      <label for="ans">Your answer</label>
      <input id="ans" maxlength="60" autocomplete="off" placeholder="Type your answer…">
      <p class="muted" id="sent" style="margin:10px 0 0"></p>
    </div>
  </div>

  <div id="v-reveal" class="hidden">
    <div class="prompt" id="rshow"></div>
    <div id="results"></div>
    <div class="row hidden" id="nextrow"><button id="next">Next prompt</button></div>
  </div>
</section>
</main>

<script src="https://cdn.jsdelivr.net/npm/peerjs@1.5.4/dist/peerjs.min.js"></script>
<script>
const DUR = 20;
const PREFIX = "quickanswer-online-";
const PROMPTS = [
"Name an animal","Name a country","Name a country flag with 2 colours or less","Name a typically favourite food","Name a form of transportation","Name a planet",
"Name a musical instrument","Name a fruit","Name a vegetable","Name a colour","Name a sport","Name a famous city","Name a car brand","Name a body part","Name a school subject",
"Name a profession","Name a movie everyone knows","Name an ice cream flavour","Name a pizza topping","Name a famous river","Name a mountain","Name a bird","Name a sea creature",
"Name a dog breed","Name a board game","Name a superhero","Name a cartoon character","Name a drink","Name a breakfast food","Name a type of weather","Name a month",
"Name a kitchen item","Name a piece of furniture","Name a piece of clothing","Name a tool","Name a flower","Name a tree","Name a gemstone","Name a metal","Name a chemical element",
"Name a shape","Name an insect","Name a reptile","Name a farm animal","Name a European capital city","Name a language","Name a currency","Name a famous landmark","Name an island",
"Name a candy or sweet","Name a social media app","Name a video game","Name something you find at the beach","Name something you take on holiday","Name a famous scientist",
"Name something yellow","Name something you find in a bathroom","Name a dessert","Name a country in Africa","Name an animal that lives in the ocean"
];
const MC_PROMPTS = [
"Name a passive mob","Name a hostile mob","Name a colour of wool","Name something that can kill you ingame","Name a biome","Name an ore","Name a tool","Name a food item","Name a type of wood","Name a potion effect",
"Name an enchantment","Name a boss mob","Name a block found in the Nether","Name a block found in the End","Name a piece of armour","Name a redstone component","Name a neutral mob","Name a mob that can fly","Name a crop you can farm","Name a block affected by gravity",
"Name something you can craft with sticks","Name a villager profession","Name a dimension","Name a structure you can find","Name something you can smelt in a furnace","Name something you can enchant","Name a mob that spawns at night","Name something flammable","Name a block you can’t break in survival","Name something you can ride",
"Name a dye colour","Name a flower","Name something you can find in a chest","Name a mob you can tame","Name an item that a mob drops","Name a Nether mob","Name a block that gives off light","Name something that restores hunger","Name a type of fish","Name a stone-type block",
"Name something that explodes","Name something you can brew a potion with","Name a tool material","Name something you can trade with a villager","Name something you find in a village","Name something that comes in 16 colours","Name an item you should bring into a cave"
];

const $ = id => document.getElementById(id);
let peer = null, conn = null, isHost = false, me = "", myId = "", roomCode = "";
let G = { phase: "lobby", round: 0, prompt: "", dur: DUR };
let S = null;                       // latest game state (host builds it, players receive it)
const hostPlayers = new Map();      // host only: peerId -> {name, conn, skip}
const hostAnswers = new Map();      // host only: id -> answer for the current round
let localRound = 0, endAt = 0, submitted = false, shownRound = 0, revealStart = 0;
let rows = new Map(), usedPrompts = new Set(), cat = "gen", timerStarted = false;

function err(t){ $("err").textContent = t; }
function newCode(){ const a = "ABCDEFGHJKLMNPQRSTUVWXYZ"; let s = ""; for (let i = 0; i < 4; i++) s += a[Math.floor(Math.random() * a.length)]; return s; }
function cleanName(){ return $("name").value.trim().slice(0, 20); }

/* ---------- joining / creating ---------- */
function checkReady(){
  me = cleanName();
  if (!me) { err("Please enter your name first."); return false; }
  if (!window.Peer) { err("Couldn’t load the connection library. Check your internet and reload."); return false; }
  err(""); return true;
}
function enterRoom(code){
  roomCode = code;
  $("roomcode").textContent = code;
  $("join").classList.add("hidden"); $("room").classList.remove("hidden");
  if (!timerStarted) { timerStarted = true; setInterval(tick, 250); }
  render();
}

function createRoom(tries){
  if (tries === undefined) { if (!checkReady()) return; tries = 0; }
  const code = newCode();
  let opened = false;
  peer = new Peer(PREFIX + code.toLowerCase());
  peer.on("open", id => {
    opened = true; isHost = true; myId = "host";
    peer.on("connection", c => {
      c.on("data", m => hostGets(c, m));
      c.on("close", () => dropPlayer(c.peer));
      c.on("error", () => dropPlayer(c.peer));
    });
    enterRoom(code); publish();
  });
  peer.on("error", e => {
    if (!opened) {
      if (e.type === "unavailable-id" && tries < 5) { peer.destroy(); createRoom(tries + 1); }
      else err("Couldn’t create a room (" + e.type + "). Please try again.");
    }
  });
}

function joinRoom(code){
  if (!checkReady()) return;
  if (!/^[A-Za-z]{4}$/.test(code)) return err("Room codes have 4 letters.");
  code = code.toUpperCase();
  let done = false;
  const fail = t => { if (!done) { done = true; err(t); try { peer.destroy(); } catch (e) {} } };
  peer = new Peer();
  const timeout = setTimeout(() => fail("Couldn’t reach that room. Check the code and try again."), 12000);
  peer.on("open", id => {
    myId = id;
    conn = peer.connect(PREFIX + code.toLowerCase(), { reliable: true });
    conn.on("open", () => {
      if (done) return;
      done = true; clearTimeout(timeout);
      conn.send({ t: "join", name: me });
      enterRoom(code);
    });
    conn.on("data", m => { if (m && m.t === "state" && m.s) { S = m.s; render(); } });
    conn.on("close", hostGone);
    conn.on("error", hostGone);
  });
  peer.on("error", e => {
    clearTimeout(timeout);
    if (!done) fail(e.type === "peer-unavailable" ? "Room not found. Check the code." : "Connection problem (" + e.type + ").");
  });
}
function hostGone(){
  if (isHost) return;
  S = null; $("nohostmsg").textContent = "The host left the game."; render();
}
$("create").onclick = () => createRoom();
$("joinbtn").onclick = () => joinRoom($("code").value.trim());
$("code").onkeydown = e => { if (e.key === "Enter") $("joinbtn").click(); };
$("copy").onclick = async () => {
  const link = location.href.split("#")[0] + "#" + roomCode;
  try { await navigator.clipboard.writeText(link); $("copy").textContent = "Copied!"; }
  catch (e) { $("copy").textContent = link; }
  setTimeout(() => { $("copy").textContent = "Copy invite link"; }, 2500);
};
if (/^#[A-Za-z]{4}$/.test(location.hash)) $("code").value = location.hash.slice(1).toUpperCase();

/* ---------- host logic ---------- */
function hostGets(c, m){
  if (!m || typeof m !== "object") return;
  if (m.t === "join" && typeof m.name === "string") {
    hostPlayers.set(c.peer, { name: m.name.trim().slice(0, 20) || "Player", conn: c, skip: G.phase !== "lobby" ? G.round : 0 });
    publish();
  } else if (m.t === "ans" && hostPlayers.has(c.peer) && m.round === G.round && typeof m.ans === "string") {
    hostAnswers.set(c.peer, m.ans.slice(0, 60));
    if (G.phase === "reveal") publish();
  }
}
function dropPlayer(id){ if (hostPlayers.delete(id)) publish(); }
function publish(){
  const list = [{ id: "host", name: me, host: true, skip: 0 }];
  hostPlayers.forEach((p, id) => list.push({ id, name: p.name, host: false, skip: p.skip }));
  S = { phase: G.phase, round: G.round, prompt: G.prompt, dur: G.dur, players: list,
        answers: G.phase === "reveal" ? Object.fromEntries(hostAnswers) : {} };
  hostPlayers.forEach(p => { try { if (p.conn.open) p.conn.send({ t: "state", s: S }); } catch (e) {} });
  render();
}

/* ---------- rendering ---------- */
function showView(v){ ["v-nohost","v-lobby","v-play","v-reveal"].forEach(x => $(x).classList.toggle("hidden", x !== v)); }

function render(){
  if (!roomCode) return;
  if (!S || !Array.isArray(S.players)) { showView("v-nohost"); return; }
  const mine = S.players.find(p => p.id === myId);
  const skipped = !!mine && S.round > 0 && mine.skip === S.round;

  if (S.phase === "play" && S.round !== localRound && !skipped) startLocalRound();

  if (S.phase === "lobby" || (S.phase === "play" && skipped)) {
    showView("v-lobby");
    $("pcount").textContent = S.players.length;
    $("players").innerHTML = "";
    S.players.forEach(p => {
      const c = document.createElement("span");
      c.className = "chip" + (p.id === myId ? " me" : "");
      c.textContent = (p.host ? "★ " : "") + String(p.name) + (p.id === myId ? " (you)" : "");
      $("players").append(c);
    });
    $("hostctl").classList.toggle("hidden", !isHost || S.phase !== "lobby");
    $("guestwait").classList.toggle("hidden", isHost && S.phase === "lobby");
    $("waitmsg").textContent = S.phase === "play" ? "A round is in progress. You’ll join the next one." : "Waiting for the host to pick a prompt…";
    $("start").disabled = !$("prompt").value.trim();
  } else if (S.phase === "play") {
    showView("v-play");
    $("pshow").textContent = String(S.prompt || "");
  } else if (S.phase === "reveal") {
    showView("v-reveal");
    if (shownRound !== S.round) {
      shownRound = S.round; revealStart = Date.now(); rows = new Map(); $("results").innerHTML = "";
      $("rshow").textContent = String(S.prompt || "");
      submitNow(S.round);
    }
    $("nextrow").classList.toggle("hidden", !isHost);
    renderResults();
  }
}

function renderResults(){
  if (!S || S.phase !== "reveal") return;
  const waitOver = Date.now() - revealStart > 4000;
  const answers = S.answers || {};
  S.players.filter(p => p.skip !== S.round).forEach(p => {
    let r = rows.get(p.id);
    if (!r) {
      const el = document.createElement("div"); el.className = "ans"; el.style.animationDelay = (rows.size * 0.3) + "s";
      const s = document.createElement("small"); const d = document.createElement("div");
      el.append(s, d); $("results").append(el); r = { s, d }; rows.set(p.id, r);
    }
    r.s.textContent = String(p.name) + (p.id === myId ? " (you)" : "");
    const has = Object.prototype.hasOwnProperty.call(answers, p.id);
    const v = has ? String(answers[p.id]) : "";
    if (has && v) { r.d.className = ""; r.d.textContent = v; }
    else if (has || waitOver) { r.d.className = "none"; r.d.textContent = "(no answer)"; }
    else { r.d.className = "none"; r.d.textContent = "…"; }
  });
}

/* ---------- round flow ---------- */
function startLocalRound(){
  localRound = S.round; endAt = Date.now() + (S.dur || DUR) * 1000; submitted = false;
  $("ans").value = ""; $("sent").textContent = ""; $("time").textContent = S.dur || DUR;
  $("fill").style.transition = "none"; $("fill").style.width = "100%";
  setTimeout(() => { $("fill").style.transition = ""; $("ans").focus(); }, 50);
}
function submitNow(round){
  if (submitted || localRound !== round) return;
  submitted = true;
  const a = $("ans").value.trim().slice(0, 60);
  if (isHost) { hostAnswers.set("host", a); if (G.phase === "reveal") publish(); }
  else if (conn && conn.open) conn.send({ t: "ans", round, ans: a });
  $("sent").textContent = "Answer sent!";
}
function tick(){
  if (!S) return;
  if (S.phase === "play" && localRound === S.round) {
    if (!submitted) {
      const ms = Math.max(0, endAt - Date.now());
      $("time").textContent = Math.ceil(ms / 1000);
      $("fill").style.width = (ms / ((S.dur || DUR) * 1000) * 100) + "%";
      if (ms <= 0) submitNow(S.round);
    } else $("time").textContent = 0;
    if (isHost && G.phase === "play" && Date.now() >= endAt + 1500) { G.phase = "reveal"; publish(); }
  }
  if (S.phase === "reveal" && Date.now() - revealStart < 5000) renderResults();
}

/* ---------- host controls ---------- */
function current(){ return cat === "mc" ? MC_PROMPTS : PROMPTS; }
function buildList(){
  $("plist").innerHTML = "";
  current().forEach(p => {
    const b = document.createElement("button"); b.className = "chip"; b.textContent = p;
    b.onclick = () => { $("prompt").value = p; $("start").disabled = false; };
    $("plist").append(b);
  });
}
function setCat(c){
  cat = c;
  $("catGen").className = c === "gen" ? "" : "ghost";
  $("catMc").className = c === "mc" ? "" : "ghost";
  buildList();
}
$("catGen").onclick = () => setCat("gen");
$("catMc").onclick = () => setCat("mc");
setCat("gen");

$("prompt").oninput = () => { $("start").disabled = !$("prompt").value.trim(); };
$("prompt").onkeydown = e => { if (e.key === "Enter" && !$("start").disabled) $("start").click(); };
$("rand").onclick = () => {
  const all = current();
  let pool = all.filter(p => !usedPrompts.has(p));
  if (!pool.length) { all.forEach(p => usedPrompts.delete(p)); pool = all; }
  $("prompt").value = pool[Math.floor(Math.random() * pool.length)];
  $("start").disabled = false;
};
$("browse").onclick = () => $("plist").classList.toggle("hidden");
$("start").onclick = () => {
  const p = $("prompt").value.trim(); if (!p) return;
  let d = Math.round(Number($("dur").value)) || DUR;
  d = Math.min(300, Math.max(5, d)); $("dur").value = d;
  usedPrompts.add(p);
  hostAnswers.clear();
  G = { phase: "play", round: G.round + 1, prompt: p, dur: d };
  publish();
};
$("next").onclick = () => { $("prompt").value = ""; G.phase = "lobby"; G.prompt = ""; publish(); };
</script>
</body>
</html>
