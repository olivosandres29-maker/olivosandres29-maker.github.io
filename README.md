<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>¿Qué dijo la IA? / What did the AI say?</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d0d0d;
    --surface: #1a1a1a;
    --surface2: #252525;
    --accent: #f5c518;
    --accent2: #e05c00;
    --correct: #22c55e;
    --wrong: #ef4444;
    --text: #f0f0f0;
    --muted: #888;
    --border: #333;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* HEADER */
  header {
    width: 100%;
    max-width: 700px;
    padding: 24px 20px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .logo {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 2rem;
    letter-spacing: 2px;
    color: var(--accent);
    line-height: 1;
  }
  .logo span { color: var(--accent2); }
  .lang-toggle {
    display: flex;
    gap: 6px;
  }
  .lang-btn {
    background: var(--surface2);
    border: 1px solid var(--border);
    color: var(--muted);
    padding: 6px 14px;
    border-radius: 20px;
    cursor: pointer;
    font-size: 0.8rem;
    font-family: 'DM Sans', sans-serif;
    transition: all 0.2s;
  }
  .lang-btn.active {
    background: var(--accent);
    color: #000;
    border-color: var(--accent);
    font-weight: 700;
  }

  /* MAIN CONTENT */
  main {
    width: 100%;
    max-width: 700px;
    padding: 20px;
    flex: 1;
  }

  /* SCREEN: MENU */
  #screen-menu { animation: fadeIn 0.4s ease; }
  .menu-title {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 3.5rem;
    line-height: 1;
    margin-bottom: 8px;
  }
  .menu-sub {
    color: var(--muted);
    margin-bottom: 36px;
    font-size: 1rem;
    line-height: 1.5;
  }
  .diff-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 14px;
    margin-bottom: 28px;
  }
  .diff-card {
    background: var(--surface);
    border: 2px solid var(--border);
    border-radius: 14px;
    padding: 22px 16px;
    cursor: pointer;
    transition: all 0.2s;
    text-align: center;
  }
  .diff-card:hover, .diff-card.selected {
    border-color: var(--accent);
    background: #252000;
  }
  .diff-card .diff-icon { font-size: 2rem; margin-bottom: 8px; }
  .diff-card .diff-name {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.4rem;
    letter-spacing: 1px;
    color: var(--accent);
  }
  .diff-card .diff-words { font-size: 0.8rem; color: var(--muted); margin-top: 4px; }
  .diff-card .diff-tries { font-size: 0.75rem; color: var(--accent2); margin-top: 2px; }

  .cat-title {
    font-size: 0.85rem;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 12px;
  }
  .cat-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-bottom: 32px;
  }
  .cat-btn {
    background: var(--surface);
    border: 2px solid var(--border);
    color: var(--text);
    padding: 8px 18px;
    border-radius: 20px;
    cursor: pointer;
    font-family: 'DM Sans', sans-serif;
    font-size: 0.9rem;
    transition: all 0.2s;
  }
  .cat-btn.selected {
    border-color: var(--accent2);
    background: #2a1000;
    color: var(--accent);
  }
  .start-btn {
    width: 100%;
    background: var(--accent);
    color: #000;
    border: none;
    padding: 16px;
    border-radius: 12px;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.4rem;
    letter-spacing: 2px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .start-btn:hover { background: #ffd740; transform: translateY(-1px); }
  .start-btn:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }

  /* SCREEN: GAME */
  #screen-game { animation: fadeIn 0.4s ease; }
  .game-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
  .celebrity-card {
    background: linear-gradient(135deg, #1e1a00, #2a1e00);
    border: 2px solid var(--accent);
    border-radius: 16px;
    padding: 24px;
    text-align: center;
    margin-bottom: 24px;
    position: relative;
    overflow: hidden;
  }
  .celebrity-card::before {
    content: '';
    position: absolute;
    top: -40px; right: -40px;
    width: 120px; height: 120px;
    background: var(--accent);
    opacity: 0.06;
    border-radius: 50%;
  }
  .celeb-category {
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 3px;
    color: var(--accent2);
    margin-bottom: 6px;
  }
  .celeb-name {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 3rem;
    color: var(--accent);
    letter-spacing: 3px;
    line-height: 1;
  }
  .celeb-context {
    font-size: 0.85rem;
    color: var(--muted);
    margin-top: 6px;
  }

  /* PROGRESS */
  .progress-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }
  .progress-label { font-size: 0.85rem; color: var(--muted); }
  .tries-display {
    display: flex;
    gap: 6px;
  }
  .try-dot {
    width: 10px; height: 10px;
    border-radius: 50%;
    background: var(--accent2);
    transition: all 0.3s;
  }
  .try-dot.used { background: var(--border); }

  /* SLOTS */
  .slots-grid {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 20px;
  }
  .slot {
    background: var(--surface);
    border: 2px solid var(--border);
    border-radius: 12px;
    padding: 14px 18px;
    display: flex;
    align-items: center;
    gap: 14px;
    transition: all 0.3s;
    min-height: 56px;
  }
  .slot.correct {
    background: #0d2a0d;
    border-color: var(--correct);
  }
  .slot.revealed {
    background: #1a1a0d;
    border-color: #888600;
  }
  .slot-num {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.3rem;
    color: var(--muted);
    min-width: 28px;
    text-align: center;
  }
  .slot.correct .slot-num, .slot.revealed .slot-num { color: var(--accent); }
  .slot-word {
    font-size: 1.05rem;
    font-weight: 700;
    letter-spacing: 1px;
    flex: 1;
  }
  .slot-blank {
    display: flex;
    gap: 4px;
    flex: 1;
  }
  .slot-dash {
    width: 10px; height: 3px;
    background: var(--border);
    border-radius: 2px;
  }
  .slot-pct {
    font-size: 0.8rem;
    color: var(--muted);
    min-width: 44px;
    text-align: right;
  }
  .slot-bar {
    width: 70px; height: 6px;
    background: var(--border);
    border-radius: 3px;
    overflow: hidden;
  }
  .slot-bar-fill {
    height: 100%;
    background: var(--accent);
    border-radius: 3px;
    transition: width 0.6s ease;
  }

  /* INPUT */
  .input-area {
    display: flex;
    gap: 10px;
    margin-bottom: 16px;
  }
  .word-input {
    flex: 1;
    background: var(--surface2);
    border: 2px solid var(--border);
    border-radius: 12px;
    padding: 14px 18px;
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 1rem;
    outline: none;
    transition: border-color 0.2s;
  }
  .word-input:focus { border-color: var(--accent); }
  .word-input:disabled { opacity: 0.5; }
  .submit-btn {
    background: var(--accent);
    color: #000;
    border: none;
    padding: 14px 22px;
    border-radius: 12px;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.1rem;
    letter-spacing: 1px;
    cursor: pointer;
    transition: all 0.2s;
    white-space: nowrap;
  }
  .submit-btn:hover { background: #ffd740; }
  .submit-btn:disabled { opacity: 0.4; cursor: not-allowed; }

  /* FEEDBACK */
  .feedback {
    min-height: 40px;
    text-align: center;
    font-size: 0.95rem;
    padding: 10px;
    border-radius: 10px;
    margin-bottom: 14px;
    transition: all 0.3s;
  }
  .feedback.thinking {
    background: #1a1a2e;
    color: #8888ff;
  }
  .feedback.correct-fb {
    background: #0d2a0d;
    color: var(--correct);
    font-weight: 700;
  }
  .feedback.wrong-fb {
    background: #2a0d0d;
    color: var(--wrong);
  }
  .feedback.similar-fb {
    background: #2a1e00;
    color: var(--accent);
  }

  /* LOADING STATE */
  .loading-overlay {
    text-align: center;
    padding: 60px 20px;
  }
  .spinner {
    width: 48px; height: 48px;
    border: 4px solid var(--border);
    border-top-color: var(--accent);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    margin: 0 auto 20px;
  }
  .loading-text {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.5rem;
    letter-spacing: 2px;
    color: var(--muted);
  }

  /* RESULT SCREEN */
  #screen-result { animation: fadeIn 0.5s ease; text-align: center; padding: 20px 0; }
  .result-icon { font-size: 4rem; margin-bottom: 16px; }
  .result-title {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 3rem;
    letter-spacing: 3px;
    margin-bottom: 8px;
  }
  .result-title.win { color: var(--correct); }
  .result-title.lose { color: var(--wrong); }
  .result-sub { color: var(--muted); margin-bottom: 28px; font-size: 1rem; }
  .result-score {
    background: var(--surface);
    border-radius: 16px;
    padding: 20px;
    margin-bottom: 24px;
    text-align: left;
  }
  .score-row {
    display: flex;
    justify-content: space-between;
    padding: 8px 0;
    border-bottom: 1px solid var(--border);
    font-size: 0.9rem;
  }
  .score-row:last-child { border-bottom: none; }
  .score-val { font-weight: 700; color: var(--accent); }
  .result-btns { display: flex; gap: 12px; }
  .result-btn {
    flex: 1;
    padding: 14px;
    border-radius: 12px;
    border: none;
    font-family: 'Bebas Neue', sans-serif;
    font-size: 1.1rem;
    letter-spacing: 1px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .result-btn.primary { background: var(--accent); color: #000; }
  .result-btn.primary:hover { background: #ffd740; }
  .result-btn.secondary { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
  .result-btn.secondary:hover { border-color: var(--accent); }

  /* ANIMATIONS */
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
  @keyframes spin { to { transform: rotate(360deg); } }
  @keyframes pop {
    0% { transform: scale(1); }
    50% { transform: scale(1.08); }
    100% { transform: scale(1); }
  }
  .pop { animation: pop 0.3s ease; }

  .screen { display: none; }
  .screen.active { display: block; }
</style>
</head>
<body>

<header>
  <div class="logo">¿Qué dijo <span>la IA</span>?</div>
  <div class="lang-toggle">
    <button class="lang-btn active" onclick="setLang('es')">ES</button>
    <button class="lang-btn" onclick="setLang('en')">EN</button>
  </div>
</header>

<main>
  <!-- MENU SCREEN -->
  <div id="screen-menu" class="screen active">
    <p class="menu-title" id="txt-title">ADIVINA LO QUE<br>DIJO LA IA</p>
    <p class="menu-sub" id="txt-sub">La IA describe a una celebridad. ¿Puedes adivinar sus características antes de quedarte sin intentos?</p>

    <p class="cat-title" id="txt-diff-title">NIVEL DE DIFICULTAD</p>
    <div class="diff-grid">
      <div class="diff-card selected" onclick="selectDiff(this,'easy')" data-diff="easy">
        <div class="diff-icon">🌱</div>
        <div class="diff-name" id="d-easy">FÁCIL</div>
        <div class="diff-words" id="d-easy-w">3 palabras</div>
        <div class="diff-tries" id="d-easy-t">5 intentos</div>
      </div>
      <div class="diff-card" onclick="selectDiff(this,'medium')" data-diff="medium">
        <div class="diff-icon">⚡</div>
        <div class="diff-name" id="d-med">MEDIO</div>
        <div class="diff-words" id="d-med-w">5 palabras</div>
        <div class="diff-tries" id="d-med-t">4 intentos</div>
      </div>
      <div class="diff-card" onclick="selectDiff(this,'hard')" data-diff="hard">
        <div class="diff-icon">🔥</div>
        <div class="diff-name" id="d-hard">DIFÍCIL</div>
        <div class="diff-words" id="d-hard-w">10 palabras</div>
        <div class="diff-tries" id="d-hard-t">3 intentos</div>
      </div>
    </div>

    <p class="cat-title" id="txt-cat-title">CATEGORÍAS</p>
    <div class="cat-grid" id="cat-grid">
      <button class="cat-btn selected" data-cat="sports" onclick="toggleCat(this)">⚽ <span id="c-sports">Deportistas</span></button>
      <button class="cat-btn selected" data-cat="entertainment" onclick="toggleCat(this)">🎬 <span id="c-ent">Actores / Cantantes</span></button>
      <button class="cat-btn selected" data-cat="politics" onclick="toggleCat(this)">🏛️ <span id="c-pol">Políticos</span></button>
      <button class="cat-btn selected" data-cat="influencers" onclick="toggleCat(this)">📱 <span id="c-inf">Influencers</span></button>
    </div>

    <button class="start-btn" id="start-btn" onclick="startGame()" id="txt-start">▶ COMENZAR JUEGO</button>
  </div>

  <!-- GAME SCREEN -->
  <div id="screen-game" class="screen">
    <div id="loading-state" class="loading-overlay">
      <div class="spinner"></div>
      <div class="loading-text" id="txt-loading">LA IA ESTÁ PENSANDO...</div>
    </div>
    <div id="game-state" style="display:none;">
      <div class="game-header">
        <div style="font-size:0.85rem; color:var(--muted);" id="txt-round">Ronda 1</div>
        <button onclick="goMenu()" style="background:none;border:1px solid var(--border);color:var(--muted);padding:6px 14px;border-radius:8px;cursor:pointer;font-family:'DM Sans',sans-serif;font-size:0.8rem;" id="txt-quit">✕ Salir</button>
      </div>

      <div class="celebrity-card">
        <div class="celeb-category" id="celeb-cat-label"></div>
        <div class="celeb-name" id="celeb-name-display"></div>
        <div class="celeb-context" id="celeb-context-display"></div>
      </div>

      <div class="progress-row">
        <div class="progress-label" id="txt-progress">Características encontradas: 0/3</div>
        <div class="tries-display" id="tries-dots"></div>
      </div>

      <div class="slots-grid" id="slots-grid"></div>

      <div class="feedback" id="feedback-box"></div>

      <div class="input-area">
        <input type="text" class="word-input" id="word-input" placeholder="Escribe un adjetivo..." onkeydown="handleKey(event)" autocomplete="off">
        <button class="submit-btn" id="submit-btn" onclick="submitWord()" id="txt-submit">ENVIAR</button>
      </div>
    </div>
  </div>

  <!-- RESULT SCREEN -->
  <div id="screen-result" class="screen">
    <div class="result-icon" id="result-icon">🏆</div>
    <div class="result-title" id="result-title">¡GANASTE!</div>
    <p class="result-sub" id="result-sub">Adivinaste todas las características</p>
    <div class="result-score" id="result-score-box"></div>
    <div class="result-btns">
      <button class="result-btn primary" onclick="startGame()" id="txt-play-again">JUGAR DE NUEVO</button>
      <button class="result-btn secondary" onclick="goMenu()" id="txt-menu">MENÚ</button>
    </div>
  </div>
</main>

<script>
// ============================================================
// STATE
// ============================================================
let lang = 'es';
let difficulty = 'easy';
let selectedCats = new Set(['sports','entertainment','politics','influencers']);
let gameData = null; // { celebrity, category, context, characteristics: [{word, pct}] }
let slots = []; // [{word, pct, found: bool}]
let triesLeft = 5;
let totalTries = 5;
let round = 1;
let score = 0;

const DIFF_CONFIG = {
  easy:   { words: 3, tries: 5 },
  medium: { words: 5, tries: 4 },
  hard:   { words: 10, tries: 3 }
};

// ============================================================
// TRANSLATIONS
// ============================================================
const T = {
  es: {
    logo: '¿Qué dijo <span>la IA</span>?',
    title: 'ADIVINA LO QUE<br>DIJO LA IA',
    sub: 'La IA describe a una celebridad. ¿Puedes adivinar sus características antes de quedarte sin intentos?',
    diffTitle: 'NIVEL DE DIFICULTAD',
    easy: 'FÁCIL', medium: 'MEDIO', hard: 'DIFÍCIL',
    easyW: '3 palabras', medW: '5 palabras', hardW: '10 palabras',
    easyT: '5 intentos', medT: '4 intentos', hardT: '3 intentos',
    catTitle: 'CATEGORÍAS',
    cSports: 'Deportistas', cEnt: 'Actores / Cantantes', cPol: 'Políticos', cInf: 'Influencers',
    start: '▶ COMENZAR JUEGO',
    loading: 'LA IA ESTÁ PENSANDO...',
    quit: '✕ Salir',
    progress: (f,t) => `Características encontradas: ${f}/${t}`,
    placeholder: 'Escribe un adjetivo...',
    submit: 'ENVIAR',
    thinking: '🤔 La IA está evaluando tu respuesta...',
    correct: (w) => `✅ ¡Correcto! "${w}" está en la lista`,
    similar: (w) => `✨ ¡Válido! La IA acepta "${w}" como respuesta`,
    wrong: (w) => `❌ "${w}" no está en la lista`,
    noTries: '💀 Sin más intentos',
    round: (r) => `Ronda ${r}`,
    winTitle: '¡GANASTE!',
    winSub: 'Adivinaste todas las características',
    loseTitleTime: '¡SE ACABARON LOS INTENTOS!',
    loseSub: 'Mejor suerte la próxima vez',
    found: 'Encontradas',
    triesUsed: 'Intentos usados',
    difficulty: 'Dificultad',
    playAgain: 'JUGAR DE NUEVO',
    menu: 'MENÚ',
    catSports: 'Deportes', catEnt: 'Entretenimiento', catPol: 'Política', catInf: 'Redes Sociales',
  },
  en: {
    logo: 'What did <span>the AI</span> say?',
    title: 'GUESS WHAT<br>THE AI SAID',
    sub: 'The AI describes a celebrity. Can you guess all their characteristics before running out of tries?',
    diffTitle: 'DIFFICULTY LEVEL',
    easy: 'EASY', medium: 'MEDIUM', hard: 'HARD',
    easyW: '3 words', medW: '5 words', hardW: '10 words',
    easyT: '5 tries', medT: '4 tries', hardT: '3 tries',
    catTitle: 'CATEGORIES',
    cSports: 'Athletes', cEnt: 'Actors / Singers', cPol: 'Politicians', cInf: 'Influencers',
    start: '▶ START GAME',
    loading: 'THE AI IS THINKING...',
    quit: '✕ Quit',
    progress: (f,t) => `Found: ${f}/${t}`,
    placeholder: 'Type an adjective...',
    submit: 'SEND',
    thinking: '🤔 The AI is evaluating your answer...',
    correct: (w) => `✅ Correct! "${w}" is on the list`,
    similar: (w) => `✨ Valid! The AI accepts "${w}" as an answer`,
    wrong: (w) => `❌ "${w}" is not on the list`,
    noTries: '💀 No more tries',
    round: (r) => `Round ${r}`,
    winTitle: 'YOU WIN!',
    winSub: 'You guessed all the characteristics',
    loseTitleTime: 'OUT OF TRIES!',
    loseSub: 'Better luck next time',
    found: 'Found',
    triesUsed: 'Tries used',
    difficulty: 'Difficulty',
    playAgain: 'PLAY AGAIN',
    menu: 'MENU',
    catSports: 'Sports', catEnt: 'Entertainment', catPol: 'Politics', catInf: 'Social Media',
  }
};

// ============================================================
// LANG
// ============================================================
function setLang(l) {
  lang = l;
  document.querySelectorAll('.lang-btn').forEach(b => b.classList.toggle('active', b.textContent === l.toUpperCase()));
  document.querySelector('.logo').innerHTML = T[l].logo;
  document.getElementById('txt-title').innerHTML = T[l].title;
  document.getElementById('txt-sub').textContent = T[l].sub;
  document.getElementById('txt-diff-title').textContent = T[l].diffTitle;
  document.getElementById('d-easy').textContent = T[l].easy;
  document.getElementById('d-med').textContent = T[l].medium;
  document.getElementById('d-hard').textContent = T[l].hard;
  document.getElementById('d-easy-w').textContent = T[l].easyW;
  document.getElementById('d-med-w').textContent = T[l].medW;
  document.getElementById('d-hard-w').textContent = T[l].hardW;
  document.getElementById('d-easy-t').textContent = T[l].easyT;
  document.getElementById('d-med-t').textContent = T[l].medT;
  document.getElementById('d-hard-t').textContent = T[l].hardT;
  document.getElementById('txt-cat-title').textContent = T[l].catTitle;
  document.getElementById('c-sports').textContent = T[l].cSports;
  document.getElementById('c-ent').textContent = T[l].cEnt;
  document.getElementById('c-pol').textContent = T[l].cPol;
  document.getElementById('c-inf').textContent = T[l].cInf;
  document.getElementById('start-btn').textContent = T[l].start;
}

// ============================================================
// MENU CONTROLS
// ============================================================
function selectDiff(el, diff) {
  document.querySelectorAll('.diff-card').forEach(c => c.classList.remove('selected'));
  el.classList.add('selected');
  difficulty = diff;
}

function toggleCat(el) {
  const cat = el.dataset.cat;
  if (selectedCats.has(cat)) {
    if (selectedCats.size === 1) return; // at least one
    selectedCats.delete(cat);
    el.classList.remove('selected');
  } else {
    selectedCats.add(cat);
    el.classList.add('selected');
  }
}

function goMenu() {
  showScreen('screen-menu');
}

// ============================================================
// SCREEN MANAGEMENT
// ============================================================
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

// ============================================================
// GAME START
// ============================================================
async function startGame() {
  showScreen('screen-game');
  document.getElementById('loading-state').style.display = 'block';
  document.getElementById('game-state').style.display = 'none';
  document.getElementById('txt-loading').textContent = T[lang].loading;

  const cfg = DIFF_CONFIG[difficulty];
  triesLeft = cfg.tries;
  totalTries = cfg.tries;

  const catList = [...selectedCats];
  const catNames = {
    sports: lang === 'es' ? 'Deportes' : 'Sports',
    entertainment: lang === 'es' ? 'Entretenimiento' : 'Entertainment',
    politics: lang === 'es' ? 'Política' : 'Politics',
    influencers: lang === 'es' ? 'Redes Sociales / Influencers' : 'Social Media / Influencers'
  };
  const selectedCatNames = catList.map(c => catNames[c]).join(', ');

  const systemPrompt = lang === 'es'
    ? `Eres el motor de un juego estilo "100 personas dijeron" pero con IA. Tu tarea es generar datos de celebridades y evaluar respuestas. Responde ÚNICAMENTE con JSON válido, sin markdown, sin explicaciones.`
    : `You are the engine of a "100 people surveyed" style game but with AI. Your task is to generate celebrity data and evaluate answers. Respond ONLY with valid JSON, no markdown, no explanations.`;

  const userPrompt = lang === 'es'
    ? `Genera una celebridad aleatoria de las categorías: ${selectedCatNames}.
Devuelve exactamente este JSON:
{
  "name": "Nombre de la celebridad",
  "category": "Categoría en español",
  "context": "Una frase corta de contexto (ej: Futbolista portugués, 5 Balones de Oro)",
  "characteristics": [
    {"word": "adjetivo1", "pct": 95},
    {"word": "adjetivo2", "pct": 87},
    ...
  ]
}
Genera exactamente ${cfg.words} características. Son adjetivos o sustantivos cortos que la gente asocia con esa persona (ej: atlético, ganador, carismático, polémico). Los porcentajes representan cuánto % de personas lo diría (de mayor a menor).`
    : `Generate a random celebrity from categories: ${selectedCatNames}.
Return exactly this JSON:
{
  "name": "Celebrity name",
  "category": "Category in English",
  "context": "Short context phrase (e.g. Portuguese footballer, 5 Ballon d'Or)",
  "characteristics": [
    {"word": "adjective1", "pct": 95},
    {"word": "adjective2", "pct": 87},
    ...
  ]
}
Generate exactly ${cfg.words} characteristics. They are short adjectives or nouns people associate with this person (e.g. athletic, winner, charismatic, controversial). Percentages represent what % of people would say it (highest to lowest).`;

  try {
    const resp = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "claude-sonnet-4-20250514",
        max_tokens: 1000,
        system: systemPrompt,
        messages: [{ role: "user", content: userPrompt }]
      })
    });
    const data = await resp.json();
    const text = data.content.map(i => i.text || '').join('');
    const clean = text.replace(/```json|```/g, '').trim();
    gameData = JSON.parse(clean);

    slots = gameData.characteristics.map(c => ({ ...c, found: false }));
    renderGame();

  } catch(e) {
    document.getElementById('txt-loading').textContent = lang === 'es'
      ? '❌ Error al conectar con la IA. Intenta de nuevo.'
      : '❌ Error connecting to AI. Try again.';
    setTimeout(goMenu, 2500);
  }
}

// ============================================================
// RENDER GAME
// ============================================================
function renderGame() {
  document.getElementById('loading-state').style.display = 'none';
  document.getElementById('game-state').style.display = 'block';

  document.getElementById('celeb-cat-label').textContent = gameData.category;
  document.getElementById('celeb-name-display').textContent = gameData.name;
  document.getElementById('celeb-context-display').textContent = gameData.context;
  document.getElementById('txt-round').textContent = T[lang].round(round);
  document.getElementById('txt-quit').textContent = T[lang].quit;
  document.getElementById('word-input').placeholder = T[lang].placeholder;
  document.getElementById('submit-btn').textContent = T[lang].submit;
  document.getElementById('feedback-box').textContent = '';
  document.getElementById('feedback-box').className = 'feedback';

  renderSlots();
  renderTries();
  updateProgress();
  document.getElementById('word-input').focus();
}

function renderSlots() {
  const grid = document.getElementById('slots-grid');
  grid.innerHTML = '';
  slots.forEach((s, i) => {
    const div = document.createElement('div');
    div.className = 'slot' + (s.found ? ' correct' : '');
    div.id = `slot-${i}`;
    if (s.found) {
      div.innerHTML = `
        <div class="slot-num">${i+1}</div>
        <div class="slot-word">${s.word.toUpperCase()}</div>
        <div class="slot-bar"><div class="slot-bar-fill" style="width:${s.pct}%"></div></div>
        <div class="slot-pct">${s.pct}%</div>
      `;
    } else {
      const dashes = Array(Math.min(s.word.length, 8)).fill('<div class="slot-dash"></div>').join('');
      div.innerHTML = `
        <div class="slot-num">${i+1}</div>
        <div class="slot-blank">${dashes}</div>
        <div class="slot-bar"><div class="slot-bar-fill" style="width:0%"></div></div>
        <div class="slot-pct">?%</div>
      `;
    }
    grid.appendChild(div);
  });
}

function renderTries() {
  const dots = document.getElementById('tries-dots');
  dots.innerHTML = '';
  for (let i = 0; i < totalTries; i++) {
    const d = document.createElement('div');
    d.className = 'try-dot' + (i >= triesLeft ? ' used' : '');
    dots.appendChild(d);
  }
}

function updateProgress() {
  const found = slots.filter(s => s.found).length;
  document.getElementById('txt-progress').textContent = T[lang].progress(found, slots.length);
}

// ============================================================
// SUBMIT
// ============================================================
function handleKey(e) {
  if (e.key === 'Enter') submitWord();
}

async function submitWord() {
  const input = document.getElementById('word-input');
  const word = input.value.trim();
  if (!word || triesLeft <= 0) return;

  input.disabled = true;
  document.getElementById('submit-btn').disabled = true;

  setFeedback(T[lang].thinking, 'thinking');

  // Check if any slot matches using AI
  const unFound = slots.map((s, i) => ({ word: s.word, idx: i })).filter((_, i) => !slots[i].found);

  const systemPrompt = lang === 'es'
    ? `Eres el juez de un juego de palabras. Evalúa si una palabra del jugador coincide con alguna de las palabras objetivo (pueden ser sinónimos, variantes o la misma palabra). Responde SOLO con JSON válido.`
    : `You are the judge of a word game. Evaluate if a player's word matches any of the target words (synonyms, variants, or exact match count). Respond ONLY with valid JSON.`;

  const targetWords = unFound.map(u => u.word).join(', ');
  const userPrompt = lang === 'es'
    ? `Palabra del jugador: "${word}"
Palabras objetivo (no encontradas): ${targetWords}

¿La palabra del jugador es igual, sinónimo muy cercano, o variante de alguna palabra objetivo?
Responde con este JSON:
{"match": true/false, "matchedWord": "palabra_objetivo_que_coincide_o_null", "type": "exact/similar/none"}`
    : `Player's word: "${word}"
Target words (not found yet): ${targetWords}

Is the player's word equal to, a very close synonym, or a variant of any target word?
Respond with this JSON:
{"match": true/false, "matchedWord": "matched_target_word_or_null", "type": "exact/similar/none"}`;

  try {
    const resp = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "claude-sonnet-4-20250514",
        max_tokens: 200,
        system: systemPrompt,
        messages: [{ role: "user", content: userPrompt }]
      })
    });
    const data = await resp.json();
    const text = data.content.map(i => i.text || '').join('');
    const clean = text.replace(/```json|```/g, '').trim();
    const result = JSON.parse(clean);

    if (result.match && result.matchedWord) {
      // Find the slot index
      const idx = slots.findIndex(s => s.word.toLowerCase() === result.matchedWord.toLowerCase());
      if (idx !== -1 && !slots[idx].found) {
        slots[idx].found = true;
        renderSlots();
        updateProgress();
        const type = result.type === 'exact' ? 'correct' : 'similar';
        setFeedback(type === 'correct' ? T[lang].correct(word) : T[lang].similar(word), type + '-fb');
        // Animate slot
        setTimeout(() => {
          const el = document.getElementById(`slot-${idx}`);
          if (el) el.classList.add('pop');
        }, 100);
        score += 10 + (triesLeft * 2);

        // Check win
        if (slots.every(s => s.found)) {
          setTimeout(() => endGame(true), 1200);
          return;
        }
      }
    } else {
      triesLeft--;
      renderTries();
      setFeedback(T[lang].wrong(word), 'wrong-fb');
      if (triesLeft <= 0) {
        setTimeout(() => endGame(false), 1200);
        return;
      }
    }
  } catch(e) {
    triesLeft--;
    renderTries();
    setFeedback(T[lang].wrong(word), 'wrong-fb');
    if (triesLeft <= 0) {
      setTimeout(() => endGame(false), 1200);
      return;
    }
  }

  input.value = '';
  input.disabled = false;
  document.getElementById('submit-btn').disabled = false;
  input.focus();
}

function setFeedback(msg, cls) {
  const fb = document.getElementById('feedback-box');
  fb.textContent = msg;
  fb.className = 'feedback ' + cls;
}

// ============================================================
// END GAME
// ============================================================
function endGame(win) {
  // Reveal all slots
  if (!win) {
    slots.forEach(s => { if (!s.found) s.revealed = true; });
    const grid = document.getElementById('slots-grid');
    grid.innerHTML = '';
    slots.forEach((s, i) => {
      const div = document.createElement('div');
      div.className = 'slot' + (s.found ? ' correct' : ' revealed');
      div.innerHTML = `
        <div class="slot-num">${i+1}</div>
        <div class="slot-word">${s.word.toUpperCase()}</div>
        <div class="slot-bar"><div class="slot-bar-fill" style="width:${s.pct}%"></div></div>
        <div class="slot-pct">${s.pct}%</div>
      `;
      grid.appendChild(div);
    });
    setTimeout(() => showResult(win), 1800);
  } else {
    showResult(win);
  }
}

function showResult(win) {
  showScreen('screen-result');
  document.getElementById('result-icon').textContent = win ? '🏆' : '💀';
  const titleEl = document.getElementById('result-title');
  titleEl.textContent = win ? T[lang].winTitle : T[lang].loseTitleTime;
  titleEl.className = 'result-title ' + (win ? 'win' : 'lose');
  document.getElementById('result-sub').textContent = win ? T[lang].winSub : T[lang].loseSub;

  const found = slots.filter(s => s.found).length;
  const diffNames = { easy: T[lang].easy, medium: T[lang].medium, hard: T[lang].hard };
  document.getElementById('result-score-box').innerHTML = `
    <div class="score-row"><span>${T[lang].found}</span><span class="score-val">${found}/${slots.length}</span></div>
    <div class="score-row"><span>${T[lang].triesUsed}</span><span class="score-val">${totalTries - triesLeft}/${totalTries}</span></div>
    <div class="score-row"><span>${T[lang].difficulty}</span><span class="score-val">${diffNames[difficulty]}</span></div>
    <div class="score-row"><span>Score</span><span class="score-val">${score}</span></div>
  `;
  document.getElementById('txt-play-again').textContent = T[lang].playAgain;
  document.getElementById('txt-menu').textContent = T[lang].menu;
  round++;
}
</script>
</body>
</html>
