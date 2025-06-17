<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Logical Reasoning & Set Operations Assistant</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --main-cyan: #00ffff;
      --main-dark: #181c24;
      --main-bg: linear-gradient(120deg, #181c24 0%, #1d2b3a 100%);
      --main-card: #232a34;
      --main-card2: #212c36;
      --main-shadow: 0 8px 32px #00ffff44;
      --main-radius: 22px;
      --main-radius-small: 14px;
      --main-font: 'Montserrat', 'Segoe UI', sans-serif;
    }
    html, body {
      margin: 0; padding: 0;
      min-height: 100vh;
      background: var(--main-bg);
      color: var(--main-cyan);
      font-family: var(--main-font);
      overflow-x: hidden;
    }
    body { position: relative; }
    
    #home-bg-video {
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      object-fit: cover;
      z-index: 0;
      filter: brightness(0.55) blur(0.5px);
      pointer-events: none;
      transition: opacity 0.8s;
    }
    #home-bg-video:not(.show) { opacity: 0; }
    #home-bg-video.show { opacity: 1; }
   
    #home.page {
      position: relative;
      z-index: 2;
      min-height: 100vh;
      width: 100vw;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      background: none;
    }
    .home-header {
      margin-top: 7vh;
      margin-bottom: 16px;
      text-align: center;
      color: var(--main-cyan);
      text-shadow: 0 4px 30px #00ffff99, 0 2px 8px #111;
      font-size: 2.3em;
      font-weight: bold;
      letter-spacing: 2px;
      line-height: 1.1;
      user-select: none;
      z-index: 2;
    }
    .home-cards {
      display: flex;
      flex-wrap: wrap;
      gap: 36px;
      align-items: flex-start;
      justify-content: center;
      margin: 40px 0 0 0;
      width: 100vw;
      max-width: 100vw;
      z-index: 2;
    }
    .module-card {
      width: 340px;
      min-height: 135px;
      background: rgba(33,44,54,0.82);
      border-radius: var(--main-radius);
      box-shadow: 0 8px 32px #00ffff33, 0 2px 8px #000a;
      display: flex;
      align-items: center;
      gap: 28px;
      padding: 28px 36px;
      cursor: pointer;
      position: relative;
      overflow: hidden;
      backdrop-filter: blur(7px) saturate(1.3);
      border: 1.5px solid #00ffff33;
      transition: transform 0.22s, box-shadow 0.22s, background 0.2s;
      animation: cardPop 0.7s cubic-bezier(.22,1,.36,1);
    }
    .module-card:hover {
      transform: scale(1.04) translateY(-8px) rotate(-1deg);
      box-shadow: 0 14px 40px #00ffff55;
      background: rgba(35,42,52,0.95);
      border: 1.5px solid #00ffff77;
    }
    .module-card .icon {
      font-size: 2.7em;
      margin-right: 5px;
      flex-shrink: 0;
      filter: drop-shadow(0 0 8px #00ffff33);
      transition: transform 0.2s;
    }
    .module-card:hover .icon {
      transform: scale(1.1) rotate(-6deg);
    }
    .module-card .card-content {
      flex: 1 1 auto;
    }
    .module-card .card-title {
      font-size: 1.25em;
      font-weight: 700;
      color: var(--main-cyan);
      margin-bottom: 4px;
      letter-spacing: 0.7px;
    }
    .module-card .card-desc {
      font-size: 1em;
      color: #aaffff;
      opacity: 0.85;
      line-height: 1.4;
    }
    @keyframes cardPop {
      from { transform: scale(0.94) translateY(40px); opacity: 0;}
      to { transform: scale(1) translateY(0); opacity: 1;}
    }
   
    .page {
      position: absolute;
      left: 0; top: 0;
      width: 100vw;
      min-height: 100vh;
      background: var(--main-bg);
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.6s, transform 0.6s;
      transform: translateX(100vw);
      z-index: 2;
    }
    .page.active {
      opacity: 1;
      pointer-events: all;
      transform: translateX(0);
      z-index: 10;
    }
    .page.slide-in {
      animation: slideIn 0.7s cubic-bezier(.22,1,.36,1) forwards;
    }
    .page.slide-out {
      animation: slideOut 0.7s cubic-bezier(.22,1,.36,1) forwards;
    }
    @keyframes slideIn {
      from { opacity: 0; transform: translateX(100vw);}
      to { opacity: 1; transform: translateX(0);}
    }
    @keyframes slideOut {
      from { opacity: 1; transform: translateX(0);}
      to { opacity: 0; transform: translateX(-100vw);}
    }
   
    .container {
      max-width: 650px;
      margin: 38px auto 40px auto;
      background: var(--main-card);
      border-radius: var(--main-radius);
      padding: 38px 20px 28px 20px;
      box-shadow: var(--main-shadow);
      min-height: 480px;
      position: relative;
      overflow: visible;
      transition: box-shadow 0.3s;
      margin-top: 70px;
    }
    .back-btn {
      background: none;
      border: none;
      color: var(--main-cyan);
      font-size: 1.1em;
      font-weight: 600;
      cursor: pointer;
      margin-bottom: 8px;
      padding: 0 0 0 0;
      transition: color 0.2s;
      letter-spacing: 0.5px;
      display: inline-block;
    }
    .back-btn:hover {
      color: #00b3b3;
      text-decoration: underline;
    }
    .form-card {
      background: var(--main-card2);
      border-radius: var(--main-radius-small);
      padding: 28px 18px 22px 18px;
      box-shadow: 0 3px 18px #00ffff11;
      margin-bottom: 14px;
      animation: fadeIn 0.8s;
      position: relative;
      z-index: 2;
    }
    label {
      color: #aaffff;
      font-size: 1.07em;
      margin-bottom: 3px;
      display: block;
      font-weight: 500;
      letter-spacing: 0.2px;
      transition: color 0.2s;
    }
    input, select, button, textarea {
      width: 100%;
      padding: 11px 12px;
      margin-bottom: 16px;
      border-radius: 8px;
      border: none;
      font-size: 1.05em;
      box-sizing: border-box;
      background: #1a222d;
      color: var(--main-cyan);
      transition: background 0.2s, box-shadow 0.2s;
      outline: none;
      font-family: inherit;
    }
    input:focus, textarea:focus, select:focus {
      background: #232a34;
      box-shadow: 0 0 0 2px #00ffff66;
    }
    textarea { resize: vertical; min-height: 42px; max-height: 120px; }
    button.action-btn {
      background: linear-gradient(90deg, #00ffff 60%, #00b3b3 100%);
      color: #181c24;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.2s, transform 0.13s;
      margin-bottom: 0;
      font-size: 1.13em;
      border-radius: 8px;
      box-shadow: 0 2px 10px #00ffff22;
    }
    button.action-btn:active {
      transform: scale(0.98);
    }
    button.action-btn:hover {
      background: linear-gradient(90deg, #00b3b3 60%, #00ffff 100%);
      color: #232a34;
    }
    .output, #output {
      margin-top: 18px;
      background: #1a222d;
      border-radius: 10px;
      padding: 17px 12px;
      min-height: 40px;
      font-size: 1.15em;
      word-break: break-word;
      position: relative;
      box-shadow: 0 2px 12px #00ffff11;
      animation: fadeIn 0.8s;
      transition: box-shadow 0.3s;
    }
    .output .history-title {
      color: #00ffff;
      font-size: 1em;
      margin-top: 12px;
      margin-bottom: 7px;
      font-weight: 600;
      letter-spacing: 0.2px;
    }
    .output .history-list {
      font-size: 0.97em;
      color: #aaffff;
      margin-bottom: 0;
      padding-left: 15px;
    }
    .output .history-list li {
      margin-bottom: 3px;
    }
    .success { color: #00ff99; }
    .error { color: #ff8383; }
    .progress-bar {
      width: 100%;
      height: 7px;
      background: #1a222d;
      border-radius: 8px;
      margin-bottom: 18px;
      overflow: hidden;
      position: relative;
    }
    .progress-bar-inner {
      height: 100%;
      width: 0;
      background: linear-gradient(90deg, #00ffff 0%, #00ffb3 100%);
      border-radius: 8px;
      animation: progressAnim 1s linear forwards;
    }
    @keyframes progressAnim {
      from { width: 0; }
      to { width: 100%; }
    }
    table.truth-table, table.matrix {
      width: 100%;
      border-collapse: collapse;
      margin-top: 8px;
      margin-bottom: 8px;
      font-size: 1em;
      border-radius: 8px;
      overflow: hidden;
      box-shadow: 0 2px 8px #00ffff22;
    }
    table.truth-table th, table.matrix th,
    table.matrix td {
      border: 1px solid #00b3b3;
      padding: 4px 6px;
      text-align: center;
      background: #232a34;
      color: #00ffff;
    }
    table.truth-table th, table.matrix th {
      background: #1a222d;
      color: #00ffff;
    }
    .copy-btn {
      background: #232a34;
      color: #00ffff;
      border: none;
      border-radius: 4px;
      padding: 3px 8px;
      font-size: 0.97em;
      cursor: pointer;
      opacity: 0.7;
      transition: opacity 0.2s, background 0.2s;
      margin-top: 5px;
      margin-bottom: 0;
      float: right;
    }
    .copy-btn:hover {
      opacity: 1;
      background: #00b3b3;
      color: #181c24;
    }
    .example-btn {
      background: #181c24;
      color: #00ffff;
      border: 1.5px solid #00ffff66;
      border-radius: 6px;
      padding: 3px 13px;
      font-size: 0.98em;
      cursor: pointer;
      margin-bottom: 12px;
      margin-top: -7px;
      float: right;
      transition: background 0.2s, color 0.2s;
    }
    .example-btn:hover {
      background: #00b3b3;
      color: #181c24;
    }
    .confetti {
      pointer-events: none;
      position: fixed;
      left: 0; top: 0; width: 100vw; height: 100vh;
      z-index: 10000;
      overflow: visible;
    }
    @media (max-width: 900px) {
      .home-header { font-size: 1.2em; margin-top: 3vh;}
      .module-card { width: 93vw; padding: 22px 10px;}
      .home-cards { gap: 20px;}
      .container { margin-top: 70px;}
    }
  </style>
</head>
<body>
 
  <video id="home-bg-video" class="show" autoplay loop muted playsinline>
    <source src="blue_flame_2.mov" type="video/mp4">
  </video>

 
  <div id="home" class="page active">
    <div class="home-header">Logical Reasoning & Set Operations<br>Personal Assistant System</div>
    <div class="home-cards">
      <div class="module-card" onclick="slideTo('truth')">
        <div class="icon">🧮</div>
        <div class="card-content">
          <div class="card-title">Truth Table Generator</div>
          <div class="card-desc">Generate truth tables for any propositional logic expression.</div>
        </div>
      </div>
      <div class="module-card" onclick="slideTo('sets')">
        <div class="icon">📚</div>
        <div class="card-content">
          <div class="card-title">Set Operations</div>
          <div class="card-desc">Perform union, intersection, difference, cartesian product, and power set.</div>
        </div>
      </div>
      <div class="module-card" onclick="slideTo('predicate')">
        <div class="icon">🔎</div>
        <div class="card-content">
          <div class="card-title">Predicate Logic</div>
          <div class="card-desc">Validate predicates across domains and find counterexamples.</div>
        </div>
      </div>
      <div class="module-card" onclick="slideTo('relation')">
        <div class="icon">🔗</div>
        <div class="card-content">
          <div class="card-title">Relation Properties</div>
          <div class="card-desc">Check if a relation is reflexive, symmetric, or transitive and see its matrix.</div>
        </div>
      </div>
      <div class="module-card" onclick="slideTo('logic')">
        <div class="icon">🤖</div>
        <div class="card-content">
          <div class="card-title">Propositional Logic Evaluation</div>
          <div class="card-desc">Evaluate logic expressions for given variable assignments.</div>
        </div>
      </div>
    </div>
  </div>

  <div id="truth" class="page">
    <div class="container">
      <button class="back-btn" onclick="slideTo('home')">&larr; Back</button>
      <h2>🧮 Truth Table Generator</h2>
      <div class="form-card">
        <button class="example-btn" onclick="fillExample('truth')">Random Example</button>
        <label>Logic Expression (e.g., (P && Q) || !R):</label>
        <input id="truthExpr" placeholder="Enter logic expression"/>
        <label>Variables (comma separated, e.g., P,Q,R):</label>
        <input id="truthVars" placeholder="P,Q,R"/>
        <div class="progress-bar" id="truthProgress" style="display:none;"><div class="progress-bar-inner"></div></div>
        <button class="action-btn" onclick="evaluateTruth()">Generate Table</button>
        <div id="truthOutput" class="output"></div>
      </div>
    </div>
  </div>
  
  <div id="sets" class="page">
    <div class="container">
      <button class="back-btn" onclick="slideTo('home')">&larr; Back</button>
      <h2>📚 Set Operations</h2>
      <div class="form-card">
        <button class="example-btn" onclick="fillExample('sets')">Random Example</button>
        <label>Set A (comma separated):</label>
        <input id="setA" placeholder="e.g., 1,2,3"/>
        <label>Set B (comma separated):</label>
        <input id="setB" placeholder="e.g., 2,3,4"/>
        <label>Operation:</label>
        <select id="setOp">
          <option value="union">Union</option>
          <option value="inter">Intersection</option>
          <option value="diff">A - B</option>
          <option value="cart">Cartesian Product</option>
          <option value="powerA">Power Set of A</option>
          <option value="powerB">Power Set of B</option>
        </select>
        <div class="progress-bar" id="setsProgress" style="display:none;"><div class="progress-bar-inner"></div></div>
        <button class="action-btn" onclick="evaluateSets()">Evaluate</button>
        <div id="setsOutput" class="output"></div>
      </div>
    </div>
  </div>
  
  <div id="predicate" class="page">
    <div class="container">
      <button class="back-btn" onclick="slideTo('home')">&larr; Back</button>
      <h2>🔎 Predicate Logic</h2>
      <div class="form-card">
        <button class="example-btn" onclick="fillExample('predicate')">Random Example</button>
        <label>Predicate (use x, e.g., x % 2 === 0):</label>
        <input id="predExpr" placeholder="e.g., x % 2 === 0"/>
        <label>Domain (comma separated):</label>
        <input id="predDom" placeholder="e.g., 1,2,3,4"/>
        <div class="progress-bar" id="predProgress" style="display:none;"><div class="progress-bar-inner"></div></div>
        <button class="action-btn" onclick="evaluatePredicate()">Evaluate</button>
        <div id="predOutput" class="output"></div>
      </div>
    </div>
  </div>
  
  <div id="relation" class="page">
    <div class="container">
      <button class="back-btn" onclick="slideTo('home')">&larr; Back</button>
      <h2>🔗 Relation Properties</h2>
      <div class="form-card">
        <button class="example-btn" onclick="fillExample('relation')">Random Example</button>
        <label>Relation pairs (e.g., (1,1),(2,2),(1,2)):</label>
        <input id="relPairs" placeholder="e.g., (1,1),(2,2),(1,2)"/>
        <label>Set of elements (comma separated):</label>
        <input id="relSet" placeholder="e.g., 1,2"/>
        <div class="progress-bar" id="relProgress" style="display:none;"><div class="progress-bar-inner"></div></div>
        <button class="action-btn" onclick="evaluateRelation()">Evaluate</button>
        <div id="relOutput" class="output"></div>
      </div>
    </div>
  </div>
  
  <div id="logic" class="page">
    <div class="container">
      <button class="back-btn" onclick="slideTo('home')">&larr; Back</button>
      <h2>🤖 Propositional Logic Evaluation</h2>
      <div class="form-card">
        <button class="example-btn" onclick="fillExample('logic')">Random Example</button>
        <label>Expression (e.g., (P && Q) || !R):</label>
        <input id="logicExpr" placeholder="e.g., (P && Q) || !R"/>
        <label>Values (comma separated, e.g., P=1,Q=0,R=1):</label>
        <input id="logicVals" placeholder="P=1,Q=0,R=1"/>
        <div class="progress-bar" id="logicProgress" style="display:none;"><div class="progress-bar-inner"></div></div>
        <button class="action-btn" onclick="evaluateLogic()">Evaluate</button>
        <div id="logicOutput" class="output"></div>
      </div>
    </div>
  </div>

  <canvas id="confetti" class="confetti" style="display:none;"></canvas>
  <script>
    function setHomeBg(show) {
      document.getElementById('home-bg-video').classList.toggle('show', show);
    }
    let currentPage = 'home';
    function slideTo(page) {
      if(page === currentPage) return;
      const curr = document.getElementById(currentPage);
      const next = document.getElementById(page);
      curr.classList.remove('active');
      curr.classList.add('slide-out');
      next.classList.add('slide-in');
      setTimeout(()=>{
        curr.classList.remove('slide-out');
        next.classList.remove('slide-in');
        next.classList.add('active');
        currentPage = page;
        setHomeBg(page==='home');
      }, 650);
    }
    function confettiBurst() {
      const canvas = document.getElementById('confetti');
      const ctx = canvas.getContext('2d');
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      canvas.style.display = '';
      let pieces = [];
      for(let i=0;i<60;i++) {
        pieces.push({
          x: Math.random()*canvas.width,
          y: -20,
          r: Math.random()*8+6,
          d: Math.random()*60,
          color: `hsl(${Math.floor(Math.random()*360)},100%,60%)`,
          tilt: Math.random()*10-10,
          tiltAngle: 0,
          tiltAngleIncremental: Math.random()*0.07+0.05
        });
      }
      let angle = 0;
      function draw() {
        ctx.clearRect(0,0,canvas.width,canvas.height);
        angle += 0.01;
        for(let i=0;i<pieces.length;i++) {
          let p = pieces[i];
          p.y += (Math.cos(angle+p.d)+3+p.r/3)/2;
          p.tiltAngle += p.tiltAngleIncremental;
          p.tilt = Math.sin(p.tiltAngle- (i%3)) * 15;
          ctx.beginPath();
          ctx.lineWidth = p.r;
          ctx.strokeStyle = p.color;
          ctx.moveTo(p.x+p.tilt+5, p.y);
          ctx.lineTo(p.x, p.y+p.tilt+10);
          ctx.stroke();
        }
      }
      let frame = 0;
      function animate() {
        draw();
        frame++;
        if(frame<80) requestAnimationFrame(animate);
        else canvas.style.display = 'none';
      }
      animate();
    }
    let histories = {
      truth: [],
      sets: [],
      predicate: [],
      relation: [],
      logic: []
    };
    function addHistory(module, input, output) {
      histories[module].unshift({input, output});
      if(histories[module].length > 5) histories[module].pop();
    }
    function renderHistory(module, outputId) {
      if(histories[module].length === 0) return '';
      let html = `<div class="history-title">Recent History:</div><ul class="history-list">`;
      for(const h of histories[module]) {
        html += `<li><b>Input:</b> ${h.input} <br/><b>Output:</b> ${h.output}</li>`;
      }
      html += '</ul>';
      document.getElementById(outputId).innerHTML += html;
    }
    function powerSet(arr) {
      const res = [[]];
      for (const el of arr) {
        const len = res.length;
        for (let i = 0; i < len; i++) {
          res.push(res[i].concat(el));
        }
      }
      return res;
    }
    function cartesianProduct(a, b) {
      const res = [];
      for (const x of a) for (const y of b) res.push([x, y]);
      return res;
    }
    function truthTable(vars, expr) {
      const n = vars.length;
      const rows = [];
      for (let i = 0; i < (1 << n); i++) {
        let vals = {};
        for (let j = 0; j < n; j++) {
          vals[vars[j]] = Boolean((i >> (n - 1 - j)) & 1);
        }
        let result;
        try {
          const fn = new Function(...vars, `return ${expr};`);
          result = fn(...vars.map(v => vals[v]));
        } catch {
          result = 'Err';
        }
        rows.push({ ...vals, result });
      }
      return rows;
    }
    function renderTruthTable(vars, rows) {
      let html = '<table class="truth-table"><tr>';
      for (const v of vars) html += `<th>${v}</th>`;
      html += '<th>Result</th></tr>';
      for (const row of rows) {
        html += '<tr>';
        for (const v of vars) html += `<td>${row[v] ? 1 : 0}</td>`;
        html += `<td>${row.result === true ? 1 : row.result === false ? 0 : row.result}</td>`;
        html += '</tr>';
      }
      html += '</table>';
      return html;
    }
    function renderMatrix(set, pairs) {
      let html = '<table class="matrix"><tr><th></th>';
      for (const y of set) html += `<th>${y}</th>`;
      html += '</tr>';
      for (const x of set) {
        html += `<tr><th>${x}</th>`;
        for (const y of set) {
          html += `<td>${pairs.some(([a, b]) => a === x && b === y) ? '1' : '0'}</td>`;
        }
        html += '</tr>';
      }
      html += '</table>';
      return html;
    }
    function copyResult(id) {
      const text = document.getElementById(id).innerText || document.getElementById(id).textContent;
      navigator.clipboard.writeText(text);
      let btn = document.getElementById(id + '_btn');
      btn.textContent = "Copied!";
      setTimeout(() => { btn.textContent = "Copy Result"; }, 1200);
    }
    const examples = {
      truth: [
        {expr: '(P && Q) || !R', vars: 'P,Q,R'},
        {expr: 'P || Q', vars: 'P,Q'},
        {expr: 'P && !Q', vars: 'P,Q'},
        {expr: '((P || Q) && R)', vars: 'P,Q,R'},
        {expr: '!(P && Q)', vars: 'P,Q'}
      ],
      sets: [
        {A: '1,2,3', B: '2,3,4', op: 'union'},
        {A: 'a,b,c', B: 'b,c,d', op: 'inter'},
        {A: 'x,y,z', B: 'y,z', op: 'diff'},
        {A: '1,2', B: 'a,b', op: 'cart'},
        {A: '1,2,3', B: '', op: 'powerA'}
      ],
      predicate: [
        {expr: 'x % 2 === 0', dom: '2,4,6,8'},
        {expr: 'x > 0', dom: '1,2,3,4'},
        {expr: 'x < 5', dom: '2,4,6,8'},
        {expr: 'x % 3 === 0', dom: '3,6,9,12'},
        {expr: 'x*x > 10', dom: '2,4,6'}
      ],
      relation: [
        {pairs: '(1,1),(2,2),(1,2),(2,1)', set: '1,2'},
        {pairs: '(a,a),(b,b),(a,b)', set: 'a,b'},
        {pairs: '(1,2),(2,3),(1,3)', set: '1,2,3'},
        {pairs: '(x,x),(y,y)', set: 'x,y'},
        {pairs: '(1,1),(2,2)', set: '1,2'}
      ],
      logic: [
        {expr: '(P && Q) || !R', vals: 'P=1,Q=0,R=1'},
        {expr: 'P || Q', vals: 'P=0,Q=1'},
        {expr: 'P && !Q', vals: 'P=1,Q=0'},
        {expr: '((P || Q) && R)', vals: 'P=1,Q=0,R=1'},
        {expr: '!(P && Q)', vals: 'P=1,Q=1'}
      ]
    };
    function fillExample(module) {
      const ex = examples[module][Math.floor(Math.random()*examples[module].length)];
      if(module==='truth') {
        document.getElementById('truthExpr').value = ex.expr;
        document.getElementById('truthVars').value = ex.vars;
      } else if(module==='sets') {
        document.getElementById('setA').value = ex.A;
        document.getElementById('setB').value = ex.B;
        document.getElementById('setOp').value = ex.op;
      } else if(module==='predicate') {
        document.getElementById('predExpr').value = ex.expr;
        document.getElementById('predDom').value = ex.dom;
      } else if(module==='relation') {
        document.getElementById('relPairs').value = ex.pairs;
        document.getElementById('relSet').value = ex.set;
      } else if(module==='logic') {
        document.getElementById('logicExpr').value = ex.expr;
        document.getElementById('logicVals').value = ex.vals;
      }
    }
    function showProgressBar(id, cb) {
      const bar = document.getElementById(id);
      bar.style.display = '';
      bar.firstElementChild.style.width = '0';
      setTimeout(()=> {
        bar.firstElementChild.style.width = '100%';
        setTimeout(()=>{ bar.style.display='none'; if(cb) cb(); }, 900);
      }, 10);
    }
    function evaluateTruth() {
      const expr = document.getElementById('truthExpr').value;
      const vars = document.getElementById('truthVars').value.split(',').map(x => x.trim()).filter(x=>x);
      const out = document.getElementById('truthOutput');
      if (!expr || vars.length === 0) { out.innerHTML = '<span class="error">Please provide expression and variables.</span>'; return; }
      showProgressBar('truthProgress', ()=>{
        const rows = truthTable(vars, expr);
        let tableHTML = renderTruthTable(vars, rows);
        out.innerHTML = tableHTML+
          `<button class="copy-btn" id="truthOutput_btn" onclick="copyResult('truthOutput')">Copy Result</button>`;
        addHistory('truth', `${expr} [${vars.join(',')}]`, 'See table');
        renderHistory('truth', 'truthOutput');
      });
    }
    function evaluateSets() {
      const setA = document.getElementById('setA').value.split(',').map(x => x.trim()).filter(x=>x);
      const setB = document.getElementById('setB').value.split(',').map(x => x.trim()).filter(x=>x);
      const op = document.getElementById('setOp').value;
      const out = document.getElementById('setsOutput');
      let result, label;
      showProgressBar('setsProgress', ()=>{
        try {
          if (op === 'union') {
            result = [...new Set([...setA, ...setB])];
            label = `A ∪ B = {${result.join(', ')}}`;
          } else if (op === 'inter') {
            result = setA.filter(x => setB.includes(x));
            label = `A ∩ B = {${result.join(', ')}}`;
          } else if (op === 'diff') {
            result = setA.filter(x => !setB.includes(x));
            label = `A - B = {${result.join(', ')}}`;
          } else if (op === 'cart') {
            result = cartesianProduct(setA, setB);
            label = `A × B = { ${result.map(pair => `(${pair[0]},${pair[1]})`).join(', ')} }`;
          } else if (op === 'powerA') {
            result = powerSet(setA);
            label = `Power Set of A = { ${result.map(s => `{${s.join(', ')}}`).join(', ')} }`;
          } else if (op === 'powerB') {
            result = powerSet(setB);
            label = `Power Set of B = { ${result.map(s => `{${s.join(', ')}}`).join(', ')} }`;
          }
          out.innerHTML = `<span class="success">${label}</span>
            <button class="copy-btn" id="setsOutput_btn" onclick="copyResult('setsOutput')">Copy Result</button>`;
          addHistory('sets', `A={${setA.join(',')}} B={${setB.join(',')}} [${op}]`, label);
          renderHistory('sets', 'setsOutput');
          confettiBurst();
        } catch {
          out.innerHTML = `<span class="error">Error: Invalid input.</span>`;
        }
      });
    }
    function evaluatePredicate() {
      const expr = document.getElementById('predExpr').value;
      const dom = document.getElementById('predDom').value.split(',').map(x => x.trim()).filter(x=>x);
      const out = document.getElementById('predOutput');
      showProgressBar('predProgress', ()=>{
        try {
          const fn = new Function('x', `return ${expr};`);
          const valid = dom.every(x => fn(Number(x)));
          let counter = dom.find(x => !fn(Number(x)));
          out.innerHTML = valid
            ? `<span class="success">Predicate holds for all elements.</span>
               <button class="copy-btn" id="predOutput_btn" onclick="copyResult('predOutput')">Copy Result</button>`
            : `<span class="error">Predicate does NOT hold for all elements.<br>
               Counterexample: <b>${counter}</b></span>
               <button class="copy-btn" id="predOutput_btn" onclick="copyResult('predOutput')">Copy Result</button>`;
          addHistory('predicate', `${expr} [${dom.join(',')}]`, valid ? 'Valid' : `Counter: ${counter}`);
          renderHistory('predicate', 'predOutput');
          if(valid) confettiBurst();
        } catch {
          out.innerHTML = `<span class="error">Error: Invalid input.</span>`;
        }
      });
    }
    function evaluateRelation() {
      const pairsRaw = document.getElementById('relPairs').value.match(/\(([^)]+)\)/g);
      const set = document.getElementById('relSet').value.split(',').map(x=>x.trim()).filter(x=>x);
      const out = document.getElementById('relOutput');
      showProgressBar('relProgress', ()=>{
        try {
          if (!pairsRaw) throw new Error("Invalid pairs format.");
          const pairs = pairsRaw.map(p => p.replace(/[()]/g, '').split(',').map(x=>x.trim()));
          let reflexive = set.every(a => pairs.some(([x, y]) => x === a && y === a));
          let symmetric = pairs.every(([a, b]) => pairs.some(([x, y]) => x === b && y === a));
          let transitive = pairs.every(([a, b]) => 
            pairs.filter(([x, y]) => x === b).every(([_, c]) => pairs.some(([x2, y2]) => x2 === a && y2 === c))
          );
          let matrixHTML = renderMatrix(set, pairs);
          out.innerHTML = `
            <span class="success">
              Reflexive: <b>${reflexive ? 'Yes' : 'No'}</b><br>
              Symmetric: <b>${symmetric ? 'Yes' : 'No'}</b><br>
              Transitive: <b>${transitive ? 'Yes' : 'No'}</b>
            </span>
            <br>
            <span style="color:#aaffff;">Relation Matrix:</span>
            ${matrixHTML}
            <button class="copy-btn" id="relOutput_btn" onclick="copyResult('relOutput')">Copy Result</button>
          `;
          addHistory('relation', `Pairs=[${pairsRaw}] Set={${set.join(',')}}`, `R:${reflexive?'Y':'N'} S:${symmetric?'Y':'N'} T:${transitive?'Y':'N'}`);
          renderHistory('relation', 'relOutput');
          if(reflexive && symmetric && transitive) confettiBurst();
        } catch {
          out.innerHTML = `<span class="error">Error: Invalid input or format.</span>`;
        }
      });
    }
    function evaluateLogic() {
      const expr = document.getElementById('logicExpr').value;
      const vals = document.getElementById('logicVals').value.split(',').reduce((acc, v) => {
        let [k, val] = v.split('=');
        acc[k?.trim()] = val?.trim() === '1';
        return acc;
      }, {});
      const out = document.getElementById('logicOutput');
      showProgressBar('logicProgress', ()=>{
        try {
          const fn = new Function(...Object.keys(vals), `return ${expr};`);
          const result = fn(...Object.values(vals));
          out.innerHTML = `<span class="success">Result: <b>${result ? 'True' : 'False'}</b></span>
            <button class="copy-btn" id="logicOutput_btn" onclick="copyResult('logicOutput')">Copy Result</button>`;
          addHistory('logic', `${expr} [${Object.entries(vals).map(([k,v])=>`${k}=${v?1:0}`).join(', ')}]`, result ? 'True' : 'False');
          renderHistory('logic', 'logicOutput');
          if(result) confettiBurst();
        } catch {
          out.innerHTML = `<span class="error">Error: Invalid input or expression.</span>`;
        }
      });
    }
    window.copyResult = copyResult;
    setHomeBg(true);
    slideTo('home');
  </script>
</body>
</html>
