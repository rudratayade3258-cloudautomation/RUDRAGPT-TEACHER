<!--
Project: RUDRAGPT TEACHER
Files included below (copy into a repo):
- index.html
- css/style.css
- js/script.js
- api/openai.js        (serverless function for Vercel/Netlify)
- README.md (deployment + env instructions)

INSTRUCTIONS:
- For a fully working AI (paid), deploy the repo to Vercel (recommended) or Netlify and set OPENAI_API_KEY env var.
- If you want to host static-only (GitHub Pages), the chatbot will fall back to a simulated/local mode (limited).
-->

<!-- ==================== index.html ==================== -->
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>RUDRAGPT TEACHER</title>
  <link rel="stylesheet" href="/css/style.css" />
  <meta name="description" content="RUDRAGPT TEACHER — AI exam helper for JEE / NEET students" />
</head>
<body>
  <div class="app">
    <aside class="sidebar">
      <div class="brand">RUDRAGPT TEACHER</div>
      <nav>
        <button id="nav-chat" class="nav-btn active">AI Tutor</button>
        <button id="nav-quiz" class="nav-btn">Quick Quiz</button>
        <button id="nav-papers" class="nav-btn">Prev Year Qs</button>
        <button id="nav-settings" class="nav-btn">Settings</button>
      </nav>
      <div class="credits">Built for students • Advanced AI (via OpenAI)</div>
    </aside>

    <main class="main">
      <header class="top">
        <h1 id="page-title">AI Tutor</h1>
        <div class="model-indicator">Mode: <span id="mode">Auto</span></div>
      </header>

      <section id="view-chat" class="view">
        <div class="chat-panel">
          <div id="messages" class="messages"></div>
          <form id="chat-form" class="chat-form">
            <input id="chat-input" placeholder="Ask about thermodynamics, solve a problem or request practice questions..." autocomplete="off" />
            <select id="topic-select" aria-label="Select subject">
              <option value="general">General</option>
              <option value="physics">Physics</option>
              <option value="chemistry">Chemistry</option>
              <option value="math">Mathematics</option>
              <option value="biology">Biology</option>
            </select>
            <button type="submit">Send</button>
          </form>
        </div>
        <aside class="right-panel">
          <div class="panel-card">
            <h3>Quick Tips</h3>
            <ul>
              <li>Ask specific steps: e.g. "Solve integral step-by-step"</li>
              <li>Request practice problems: "Give 5 NEET-level MCQs on optics"</li>
              <li>Use the Quiz tab to attempt short timed tests</li>
            </ul>
          </div>

          <div class="panel-card">
            <h3>Session Controls</h3>
            <button id="clear-chat">Clear Chat</button>
            <button id="save-session">Export Chat</button>
          </div>
        </aside>
      </section>

      <section id="view-quiz" class="view hidden">
        <div class="panel-card large">
          <h2>Quick Quiz</h2>
          <div class="quiz-controls">
            <select id="quiz-subject">
              <option value="physics">Physics</option>
              <option value="chemistry">Chemistry</option>
              <option value="math">Math</option>
              <option value="biology">Biology</option>
            </select>
            <select id="quiz-level">
              <option value="easy">Easy</option>
              <option value="medium" selected>Medium</option>
              <option value="hard">Hard</option>
            </select>
            <button id="gen-quiz">Generate 5 Qs</button>
          </div>
          <div id="quiz-area"></div>
        </div>
      </section>

      <section id="view-papers" class="view hidden">
        <div class="panel-card large">
          <h2>Previous Year Questions</h2>
          <p>Sample curated problems (editable).</p>
          <div id="papers-list">
            <article class="paper">
              <h4>Physics — Projectile motion (JEE)</h4>
              <p>Question: A projectile is fired at ... (example)</p>
            </article>
            <article class="paper">
              <h4>Chemistry — Organic (NEET)</h4>
              <p>Question: Identify the major product of ...</p>
            </article>
          </div>
        </div>
      </section>

      <section id="view-settings" class="view hidden">
        <div class="panel-card">
          <h2>Settings</h2>
          <p><strong>AI Mode:</strong></p>
          <label><input type="radio" name="ai-mode" value="auto" checked /> Server (Recommended)</label>
          <label><input type="radio" name="ai-mode" value="local" /> Local Demo (Free)</label>

          <p style="margin-top:12px">Server settings (for OpenAI):</p>
          <pre class="code">Endpoint: /api/openai<br/>Env var: OPENAI_API_KEY</pre>

          <p style="color:#c5cdd9;font-size:13px">Tip: Deploy to Vercel and add environment variable in Project Settings.</p>
        </div>
      </section>

    </main>
  </div>

  <script src="/js/script.js"></script>
</body>
</html>


/* ==================== css/style.css ==================== */
:root{--bg:#0f1724;--card:#0b1220;--muted:#94a3b8;--accent:#6ee7b7}
*{box-sizing:border-box;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,"Helvetica Neue",Arial}
html,body,#root{height:100%}
body{margin:0;background:linear-gradient(180deg,#071022 0%, #0a1220 100%);color:#e6eef8}
.app{display:flex;min-height:100vh}
.sidebar{width:260px;padding:28px;background:linear-gradient(180deg,#07102880,#071028);border-right:1px solid rgba(255,255,255,0.02)}
.brand{font-weight:800;font-size:20px;margin-bottom:18px}
.nav-btn{display:block;width:100%;padding:10px 12px;border:0;border-radius:10px;background:transparent;color:var(--muted);text-align:left;margin:6px 0;cursor:pointer}
.nav-btn.active{background:rgba(255,255,255,0.02);color:#fff}
.credits{font-size:12px;color:var(--muted);margin-top:22px}
.main{flex:1;padding:20px;display:flex;flex-direction:column}
.top{display:flex;justify-content:space-between;align-items:center}
.top h1{margin:0;font-size:20px}
.view{display:flex;gap:18px;margin-top:18px}
.hidden{display:none}
.chat-panel{flex:1;display:flex;flex-direction:column}
.messages{flex:1;padding:18px;border-radius:12px;background:linear-gradient(180deg,#071028,#071022);overflow:auto;border:1px solid rgba(255,255,255,0.03)}
.msg{margin-bottom:12px}
.msg.user{text-align:right}
.msg .bubble{display:inline-block;padding:10px 14px;border-radius:10px;max-width:78%;line-height:1.25}
.msg.user .bubble{background:linear-gradient(90deg,#0ea5a4,#06b6d4);color:#021;}
.msg.bot .bubble{background:rgba(255,255,255,0.03);color:#e6eef8}
.chat-form{display:flex;gap:8px;padding-top:12px}
.chat-form input{flex:1;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit}
.chat-form select{padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit}
.chat-form button{padding:9px 12px;border-radius:8px;border:none;background:var(--accent);color:#022;cursor:pointer}
.right-panel{width:320px}
.panel-card{background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));padding:12px;border-radius:10px;margin-bottom:12px;border:1px solid rgba(255,255,255,0.02)}
.panel-card.large{min-height:280px}
.quiz-controls{display:flex;gap:8px;align-items:center;margin-bottom:10px}
.quiz-item{padding:10px;border-radius:8px;background:rgba(255,255,255,0.02);margin-bottom:8px}
.code{background:#061221;padding:8px;border-radius:6px;font-size:13px}
#papers-list .paper{margin-bottom:8px}
button{font-weight:600}
@media(max-width:900px){.sidebar{display:none}.right-panel{display:none}.app{flex-direction:column}}


// ==================== js/script.js ====================
(function(){
  // Basic SPA nav
  const views = {chat:document.getElementById('view-chat'), quiz:document.getElementById('view-quiz'), papers:document.getElementById('view-papers'), settings:document.getElementById('view-settings')}
  const navBtns = document.querySelectorAll('.nav-btn');
  const setActive = (name)=>{
    document.getElementById('page-title').innerText = name==='chat'? 'AI Tutor' : name==='quiz'? 'Quick Quiz' : name==='papers'? 'Prev Year Qs' : 'Settings'
    Object.values(views).forEach(v=>v.classList.add('hidden'));
    if(name==='chat') views.chat.classList.remove('hidden');
    if(name==='quiz') views.quiz.classList.remove('hidden');
    if(name==='papers') views.papers.classList.remove('hidden');
    if(name==='settings') views.settings.classList.remove('hidden');
    navBtns.forEach(b=>b.classList.remove('active'));
    document.querySelector('#nav-'+name).classList.add('active');
  }
  document.getElementById('nav-chat').addEventListener('click',()=>setActive('chat'));
  document.getElementById('nav-quiz').addEventListener('click',()=>setActive('quiz'));
  document.getElementById('nav-papers').addEventListener('click',()=>setActive('papers'));
  document.getElementById('nav-settings').addEventListener('click',()=>setActive('settings'));

  // Chat logic
  const messagesEl = document.getElementById('messages');
  const chatForm = document.getElementById('chat-form');
  const chatInput = document.getElementById('chat-input');
  const topicSelect = document.getElementById('topic-select');
  const modeEl = document.getElementById('mode');

  let aiMode = localStorage.getItem('ai-mode') || 'auto';
  modeEl.innerText = aiMode==='auto'? 'Server (OpenAI)' : 'Local Demo';

  document.querySelectorAll('input[name="ai-mode"]').forEach(r=>r.addEventListener('change', e=>{
    aiMode = e.target.value; localStorage.setItem('ai-mode', aiMode); modeEl.innerText = aiMode==='auto'? 'Server (OpenAI)' : 'Local Demo';
  }));

  function appendMessage(text, who='bot'){
    const div = document.createElement('div'); div.className = 'msg '+who;
    const b = document.createElement('div'); b.className = 'bubble'; b.innerHTML = text.replace(/\n/g,'<br>');
    div.appendChild(b); messagesEl.appendChild(div); messagesEl.scrollTop = messagesEl.scrollHeight;
  }

  chatForm.addEventListener('submit', async (e)=>{
    e.preventDefault(); const q = chatInput.value.trim(); if(!q) return;
    appendMessage(q, 'user'); chatInput.value='';
    appendMessage('Thinking...', 'bot');
    const lastBot = messagesEl.querySelectorAll('.msg.bot .bubble');
    const placeholder = lastBot[lastBot.length-1];

    try{
      const response = await askAI(q, topicSelect.value);
      // replace placeholder
      placeholder.innerHTML = response;
    }catch(err){
      console.error(err);
      placeholder.innerHTML = 'Error: '+(err.message||err);
    }
  });

  async function askAI(question, topic){
    // If serverless endpoint exists, call /api/openai, else fallback to local simulated answers
    if(aiMode==='auto'){
      try{
        const res = await fetch('/api/openai',{
          method:'POST',headers:{'Content-Type':'application/json'},
          body: JSON.stringify({question, topic, max_tokens:600})
        });
        if(!res.ok){
          const t = await res.text(); throw new Error('Server error: '+t.slice(0,200));
        }
        const data = await res.json();
        // Expecting {answer: '...'}
        return data.answer || JSON.stringify(data).slice(0,1000);
      }catch(err){
        console.warn('Server call failed, falling back to local demo.',err);
        return localDemo(question, topic);
      }
    }else{
      return localDemo(question, topic);
    }
  }

  function localDemo(q, topic){
    // Simple simulated responses for offline demo (do not replace real AI)
    if(/solve|integral|differentiate|derivative|calculate|compute/i.test(q)){
      return 'Step 1: Identify variables. Step 2: Apply relevant formula. (This is a demo response. For full solutions enable server AI.)';
    }
    if(/practice|mcq|questions|quiz|give 5/i.test(q)){
      return '1) Sample MCQ on '+topic+' - (A) ... (B) ...\n2) Sample MCQ ...';
    }
    return 'RUDRAGPT (demo): I can help with '+topic+'. Ask specifics like "Solve this integral: \u222b x cos x dx" or "Give 5 NEET physics MCQs".';
  }

  // Clear & export
  document.getElementById('clear-chat').addEventListener('click', ()=>{messagesEl.innerHTML='';});
  document.getElementById('save-session').addEventListener('click', ()=>{
    const text = Array.from(messagesEl.querySelectorAll('.msg')).map(m=>m.innerText).join('\n---\n');
    const blob = new Blob([text],{type:'text/plain'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a'); a.href = url; a.download = 'rudragpt-session.txt'; a.click(); URL.revokeObjectURL(url);
  });

  // Quiz generation
  document.getElementById('gen-quiz').addEventListener('click', async ()=>{
    const subject = document.getElementById('quiz-subject').value;
    const level = document.getElementById('quiz-level').value;
    const area = document.getElementById('quiz-area'); area.innerHTML = '<p>Generating...</p>';
    // Try server first
    try{
      const res = await fetch('/api/openai',{method:'POST',headers:{'Content-Type':'application/json'},body: JSON.stringify({question:`Generate 5 ${level} ${subject} MCQs with answers in concise format`, topic:subject, max_tokens:400})});
      if(!res.ok) throw new Error('server failed');
      const d = await res.json(); area.innerHTML = '<pre class="code">'+(d.answer||JSON.stringify(d))+'</pre>';
    }catch(err){
      area.innerHTML = '<div class="quiz-item">(Demo) 1) Sample question on '+subject+' ...</div><div class="quiz-item">2) Another sample ...</div>';
    }
  });

  // Small UX: pre-populate welcome message
  appendMessage('Hello! I am RUDRAGPT — your AI exam helper. Ask me to solve problems, create practice quizzes, or explain concepts step-by-step. For full answers enable server AI (Settings).', 'bot');

})();


// ==================== api/openai.js (serverless) ====================
// Put this file under /api/openai.js for Vercel (or adapt for Netlify). Do NOT commit your OPENAI_API_KEY.
// Set the environment variable OPENAI_API_KEY in your deployment platform.

// Example Vercel-compatible handler (Node.js):

// Save as /api/openai.js

const openaiServerCode = `
// Node 18+ environment expected
import fetch from 'node-fetch';

export default async function handler(req, res) {
  if(req.method !== 'POST') return res.status(405).send('Method Not Allowed');
  const body = req.body || {};
  const question = body.question || 'Hello';
  const topic = body.topic || 'general';
  const max_tokens = body.max_tokens || 600;
  const OPENAI_KEY = process.env.OPENAI_API_KEY;
  const MODEL = process.env.OPENAI_MODEL || 'gpt-4o'; // change as needed
  if(!OPENAI_KEY) return res.status(500).json({error:'OPENAI_API_KEY not configured'});

  try{
    const response = await fetch('https://api.openai.com/v1/chat/completions',{method:'POST',headers:{
      'Content-Type':'application/json','Authorization':`Bearer ${OPENAI_KEY}`
    },body: JSON.stringify({
      model: MODEL,
      messages:[{role:'system',content:`You are RUDRAGPT, an expert teacher for JEE/NEET-level problems. Answer clearly, show steps for calculations, provide final answer and brief explanation.`},{role:'user',content:`Topic: ${topic}\nQuestion: ${question}`}],
      max_tokens: max_tokens
    })});
    const data = await response.json();
    // data.choices[0].message.content
    const ans = data?.choices?.[0]?.message?.content || JSON.stringify(data);
    return res.status(200).json({answer: ans});
  }catch(err){
    console.error(err); return res.status(500).json({error:err.message});
  }
}
`;

// ==================== README.md ====================
const readme = `# RUDRAGPT TEACHER\n\nA modern static frontend + optional serverless endpoint for OpenAI.\n\n## Files\n- index.html (main static site)\n- css/style.css\n- js/script.js\n- api/openai.js (serverless - Vercel/Netlify)\n\n## 1) Quick local test\n- Serve the folder with a static server (VSCode Live Server or: )\n  - Python: python -m http.server 8000  (then open http://localhost:8000)
\n## 2) To enable real OpenAI (paid): deploy to Vercel (recommended)\n- Create a Vercel project linked to this repo\n- Add Environment Variable: OPENAI_API_KEY = <your OpenAI key>\n- (Optional) OPENAI_MODEL = gpt-4o or whichever model you prefer\n- The serverless function is at /api/openai which the front-end calls.\n\n> Important: Do NOT put your API key in client JS or commit it. Use env vars in your host.\n\n## 3) If you must host on GitHub Pages (static only)\n- The chatbot will use a local demo fallback (limited).\n- For production AI, host the API on Vercel and point frontend to that endpoint OR create a small backend and enable CORS.\n\n## 4) Customization tips\n- Edit messages and system prompt in api/openai.js to bias answers towards step-by-step exam-style solutions.\n- Add more curated previous-year questions in index.html or load from a JSON file.\n\n## 5) Security Notes\n- Keep OPENAI_API_KEY secret. Use serverless env vars.\n- Limit requests and add basic auth or usage tracking if sharing public project.\n\nHappy building! - RUDRAGPT TEAM\n`;

// We'll export both strings so they can be copied into files by the developer/user.

// The canvas view should include these pieces for user to copy.

/*
Now the document contains:
- index.html content
- css/style.css content
- js/script.js content
- api/openai.js text (as string to copy)
- README content
*/

// Append helper text in the same doc so user can copy server code and README easily.


<!-- === Serverless & README (copy below into files) === -->

<!-- api/openai.js (copy the code between backticks into your server file) -->

<!-- README.md -->


