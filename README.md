<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mohamad — Android Developer</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;600;800&family=Space+Grotesk:wght@300;400;700&display=swap" rel="stylesheet">
<style>
  :root {
    --green: #3DDC84;
    --green-dim: #1a5c3a;
    --green-glow: rgba(61,220,132,0.15);
    --bg: #0d0d0d;
    --bg2: #0a1628;
    --white: #ffffff;
    --gray: #8892a4;
    --card: rgba(255,255,255,0.03);
    --border: rgba(61,220,132,0.2);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--white);
    font-family: 'JetBrains Mono', monospace;
    overflow-x: hidden;
    min-height: 100vh;
  }

  /* ── PARTICLES ── */
  #particles {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 0;
    overflow: hidden;
  }
  .particle {
    position: absolute;
    width: 2px;
    height: 2px;
    background: var(--green);
    border-radius: 50%;
    opacity: 0;
    animation: float-particle linear infinite;
  }

  @keyframes float-particle {
    0%   { transform: translateY(100vh) scale(0); opacity: 0; }
    10%  { opacity: 0.6; }
    90%  { opacity: 0.3; }
    100% { transform: translateY(-10px) scale(1.5); opacity: 0; }
  }

  /* ── SCANLINE OVERLAY ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 1;
  }

  /* ── GRID BACKGROUND ── */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(61,220,132,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(61,220,132,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  .container {
    max-width: 860px;
    margin: 0 auto;
    padding: 40px 20px 80px;
    position: relative;
    z-index: 2;
  }

  /* ── HERO TERMINAL ── */
  .hero {
    padding: 48px 0 50px;
    position: relative;
  }

  /* terminal window */
  .terminal {
    background: #0a0a0a;
    border: 1px solid rgba(61,220,132,0.25);
    border-radius: 14px;
    overflow: hidden;
    box-shadow:
      0 0 0 1px rgba(0,0,0,0.6),
      0 30px 80px rgba(0,0,0,0.7),
      0 0 60px rgba(61,220,132,0.06);
    opacity: 0;
    animation: fade-up 0.9s 0.1s ease forwards;
  }

  /* title bar */
  .term-bar {
    display: flex;
    align-items: center;
    gap: 7px;
    padding: 12px 18px;
    background: #111;
    border-bottom: 1px solid rgba(255,255,255,0.06);
    position: relative;
  }
  .term-btn {
    width: 12px; height: 12px;
    border-radius: 50%;
    flex-shrink: 0;
  }
  .term-btn.r { background: #ff5f57; box-shadow: 0 0 6px #ff5f5766; }
  .term-btn.y { background: #febc2e; box-shadow: 0 0 6px #febc2e66; }
  .term-btn.g { background: #28c840; box-shadow: 0 0 6px #28c84066; }
  .term-title {
    position: absolute;
    left: 50%; transform: translateX(-50%);
    font-size: 11px;
    color: #555;
    letter-spacing: 1px;
  }
  .term-badge {
    margin-left: auto;
    font-size: 10px;
    color: var(--green);
    opacity: 0.6;
    letter-spacing: 2px;
  }

  /* terminal body */
  .term-body {
    padding: 28px 28px 32px;
    font-size: 14px;
    line-height: 1;
  }

  /* each line */
  .tline {
    display: flex;
    align-items: flex-start;
    gap: 0;
    margin-bottom: 10px;
    opacity: 0;
    animation: fade-in 0.01s ease forwards;
  }

  /* prompt pieces */
  .p-root  { color: var(--green); font-weight: 800; }
  .p-at    { color: #555; }
  .p-host  { color: #79c0ff; font-weight: 600; }
  .p-colon { color: #555; }
  .p-path  { color: #cc99ff; }
  .p-hash  { color: var(--green); font-weight: 800; margin: 0 6px 0 2px; }
  .p-cmd   { color: var(--white); }
  .p-flag  { color: #febc2e; }
  .p-str   { color: #f97316; }
  .p-comment { color: #3a3a3a; font-style: italic; }

  /* output lines */
  .tout {
    padding-left: 0;
    margin-bottom: 6px;
    opacity: 0;
    animation: fade-in 0.01s ease forwards;
  }

  /* blank spacer */
  .tblank { margin-bottom: 18px; }

  /* big name render */
  .ascii-name {
    font-size: clamp(36px, 8vw, 72px);
    font-weight: 800;
    color: var(--green);
    letter-spacing: -1px;
    line-height: 1;
    text-shadow:
      0 0 30px rgba(61,220,132,0.5),
      0 0 70px rgba(61,220,132,0.2);
    margin: 6px 0 4px 0;
    display: block;
    position: relative;
  }
  .ascii-name::before,
  .ascii-name::after {
    content: attr(data-text);
    position: absolute;
    top: 0; left: 0;
    width: 100%;
  }
  .ascii-name::before {
    color: #00ffcc;
    animation: glitch-1 6s 2s infinite;
    clip-path: polygon(0 0, 100% 0, 100% 38%, 0 38%);
  }
  .ascii-name::after {
    color: #3DDC84;
    animation: glitch-2 6s 2.1s infinite;
    clip-path: polygon(0 65%, 100% 65%, 100% 100%, 0 100%);
  }
  @keyframes glitch-1 {
    0%,92%,100% { transform: none; opacity: 0; }
    93% { transform: translate(-4px, 1px); opacity: 0.7; }
    94% { transform: translate(4px, -2px); opacity: 0.7; }
    95% { transform: translate(-2px, 3px); opacity: 0.7; }
    96% { transform: none; opacity: 0; }
  }
  @keyframes glitch-2 {
    0%,92%,100% { transform: none; opacity: 0; }
    93% { transform: translate(4px, -1px); opacity: 0.5; }
    94% { transform: translate(-4px, 2px); opacity: 0.5; }
    95% { transform: translate(2px, -3px); opacity: 0.5; }
    96% { transform: none; opacity: 0; }
  }

  .nick-tag {
    display: inline-block;
    font-size: 13px;
    color: #79c0ff;
    letter-spacing: 4px;
    margin-bottom: 18px;
  }
  .nick-tag .at-sym { color: var(--green); }

  /* role badge */
  .role-line {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-top: 4px;
  }
  .role-pill {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 6px 16px;
    background: rgba(61,220,132,0.08);
    border: 1px solid rgba(61,220,132,0.3);
    border-radius: 6px;
    font-size: 12px;
    color: var(--green);
    letter-spacing: 2px;
  }

  /* blinking block cursor */
  .block-cursor {
    display: inline-block;
    width: 10px; height: 18px;
    background: var(--green);
    margin-left: 2px;
    vertical-align: middle;
    animation: blink-block 1s step-end infinite;
    box-shadow: 0 0 8px var(--green);
  }
  @keyframes blink-block {
    0%,100% { opacity: 1; } 50% { opacity: 0; }
  }

  .dot {
    width: 8px; height: 8px;
    background: var(--green);
    border-radius: 50%;
    animation: blink 1.4s ease-in-out infinite;
    box-shadow: 0 0 8px var(--green);
    display: inline-block;
  }
  @keyframes blink {
    0%,100% { opacity: 1; } 50% { opacity: 0.2; }
  }

  /* ── DIVIDER ── */
  .divider {
    display: flex;
    align-items: center;
    gap: 16px;
    margin: 48px 0;
    opacity: 0;
    animation: fade-in 0.8s 1s ease forwards;
  }
  .divider-line {
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--green-dim), transparent);
  }
  .divider-icon {
    color: var(--green);
    font-size: 12px;
    letter-spacing: 3px;
  }

  @keyframes fade-up {
    from { opacity: 0; transform: translateY(24px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes fade-in {
    from { opacity: 0; } to { opacity: 1; }
  }

  /* ── SECTION TITLES ── */
  .section-title {
    font-size: 11px;
    letter-spacing: 5px;
    color: var(--green);
    text-transform: uppercase;
    margin-bottom: 24px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  /* ── CODE BLOCK ── */
  .code-block {
    background: rgba(0,0,0,0.5);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 28px 32px;
    font-size: 13px;
    line-height: 1.9;
    position: relative;
    overflow: hidden;
    backdrop-filter: blur(12px);
    box-shadow: 0 0 40px rgba(61,220,132,0.04), inset 0 1px 0 rgba(255,255,255,0.05);
  }
  .code-block::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--green), transparent);
    animation: scan-line 4s linear infinite;
    opacity: 0.5;
  }
  @keyframes scan-line {
    0%   { transform: translateX(-100%); }
    100% { transform: translateX(100%); }
  }

  .code-bar {
    display: flex;
    align-items: center;
    gap: 6px;
    margin-bottom: 20px;
  }
  .code-dot {
    width: 10px; height: 10px;
    border-radius: 50%;
  }
  .code-dot.red    { background: #ff5f57; }
  .code-dot.yellow { background: #febc2e; }
  .code-dot.green  { background: #28c840; }
  .code-filename {
    margin-left: 10px;
    font-size: 11px;
    color: var(--gray);
    letter-spacing: 1px;
  }

  .k { color: #cc99ff; }   /* keyword */
  .s { color: #f9c74f; }   /* string */
  .v { color: var(--green); } /* value/green */
  .p { color: #8892a4; }   /* punctuation */
  .c { color: #666; font-style: italic; } /* comment */
  .n { color: #79c0ff; }   /* name/class */
  .e { color: #ff9f43; }   /* emoji/special */

  /* ── GRID ── */
  .grid-2 {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    margin-bottom: 20px;
  }
  @media (max-width: 600px) {
    .grid-2 { grid-template-columns: 1fr; }
  }

  /* ── STACK PILLS ── */
  .stack-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 12px;
    margin-bottom: 48px;
  }

  .stack-pill {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 14px 18px;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 10px;
    font-size: 12px;
    transition: all 0.3s ease;
    cursor: default;
    position: relative;
    overflow: hidden;
  }
  .stack-pill::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--green-glow);
    opacity: 0;
    transition: opacity 0.3s;
  }
  .stack-pill:hover::before { opacity: 1; }
  .stack-pill:hover {
    border-color: var(--green);
    transform: translateY(-2px);
    box-shadow: 0 8px 30px rgba(61,220,132,0.12);
  }
  .pill-name { color: var(--white); font-weight: 600; font-size: 11px; letter-spacing: 1px; }
  .pill-stars { color: var(--green); letter-spacing: 2px; font-size: 10px; }

  /* ── PHILOSOPHY BOX ── */
  .philosophy {
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 32px;
    background: rgba(61,220,132,0.02);
    position: relative;
    overflow: hidden;
    margin-bottom: 48px;
  }
  .philosophy::before {
    content: '"';
    position: absolute;
    top: -10px; left: 20px;
    font-size: 120px;
    color: var(--green);
    opacity: 0.06;
    font-family: Georgia, serif;
    line-height: 1;
  }
  .philosophy-rules {
    list-style: none;
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    gap: 12px;
  }
  .philosophy-rules li {
    display: flex;
    align-items: flex-start;
    gap: 10px;
    font-size: 13px;
    color: var(--gray);
    line-height: 1.6;
  }
  .philosophy-rules li::before {
    content: '✦';
    color: var(--green);
    flex-shrink: 0;
    margin-top: 1px;
  }
  .philosophy-quote {
    font-size: 15px;
    color: var(--white);
    font-weight: 600;
    font-style: italic;
  }

  /* ── PROGRESS ── */
  .progress-section {
    margin-bottom: 48px;
  }
  .progress-title {
    font-size: 12px;
    color: var(--gray);
    margin-bottom: 6px;
    display: flex;
    justify-content: space-between;
  }
  .progress-bar-wrap {
    height: 6px;
    background: rgba(255,255,255,0.06);
    border-radius: 999px;
    overflow: hidden;
    margin-bottom: 16px;
  }
  .progress-bar-fill {
    height: 100%;
    border-radius: 999px;
    background: linear-gradient(90deg, #1a5c3a, var(--green));
    box-shadow: 0 0 10px rgba(61,220,132,0.4);
    width: 0%;
    transition: width 1.5s cubic-bezier(0.16, 1, 0.3, 1);
  }

  .current-build {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 24px 28px;
    margin-bottom: 48px;
  }
  .build-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px 0;
    border-bottom: 1px solid rgba(255,255,255,0.04);
    font-size: 12px;
    color: var(--gray);
    transition: color 0.3s;
  }
  .build-item:last-child { border-bottom: none; }
  .build-item:hover { color: var(--white); }
  .build-dot-filled { color: var(--green); }
  .build-dot-empty  { color: var(--gray); opacity: 0.4; }

  /* ── FOOTER ── */
  .footer {
    text-align: center;
    padding-top: 48px;
    border-top: 1px solid var(--border);
  }
  .footer-text {
    font-size: 11px;
    color: var(--gray);
    letter-spacing: 3px;
    text-transform: uppercase;
    animation: fade-in 1s 1.5s ease both;
  }
  .footer-glow {
    margin-top: 20px;
    font-size: 20px;
    color: var(--green);
    animation: float 3s ease-in-out infinite;
  }
  @keyframes float {
    0%,100% { transform: translateY(0); }
    50% { transform: translateY(-6px); }
  }

  /* ── SCROLL ANIMATIONS ── */
  .reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: none;
  }

  /* ── TYPING CURSOR ── */
  .cursor {
    display: inline-block;
    width: 2px;
    height: 1em;
    background: var(--green);
    margin-left: 2px;
    animation: blink 1s step-end infinite;
    vertical-align: middle;
  }
</style>
</head>
<body>

<div id="particles"></div>

<div class="container">

  <!-- HERO TERMINAL -->
  <div class="hero">
    <div class="terminal">

      <!-- title bar -->
      <div class="term-bar">
        <div class="term-btn r"></div>
        <div class="term-btn y"></div>
        <div class="term-btn g"></div>
        <span class="term-title">bash — zsh — 80×24</span>
        <span class="term-badge">SSH ●</span>
      </div>

      <!-- terminal body -->
      <div class="term-body">

        <!-- line 1: whoami -->
        <div class="tline" id="l1">
          <span class="p-root">root</span><span class="p-at">@</span><span class="p-host">github</span><span class="p-colon">:</span><span class="p-path">~/profile</span><span class="p-hash">#</span>
          <span class="p-cmd">whoami <span class="p-flag">--verbose</span></span>
        </div>

        <!-- output block -->
        <div class="tout tblank" id="out1" style="padding-left:0; margin-top:20px;">
          <span class="ascii-name" data-text="MOHAMAD">MOHAMAD</span>
          <span class="nick-tag"><span class="at-sym">@</span>M_One</span>
          <div class="role-line">
            <div class="role-pill">
              <span class="dot"></span>
              Android Developer
            </div>
          </div>
        </div>

        <!-- line 2: cat identity -->
        <div class="tline" id="l2" style="margin-top:24px;">
          <span class="p-root">root</span><span class="p-at">@</span><span class="p-host">github</span><span class="p-colon">:</span><span class="p-path">~/profile</span><span class="p-hash">#</span>
          <span class="p-cmd">cat <span class="p-str">identity.json</span></span>
        </div>

        <!-- json output -->
        <div class="tout" id="out2" style="margin-top:10px; padding-left:0; color:#8892a4; font-size:13px; line-height:2;">
<span style="color:#555">{</span><br>
&nbsp;&nbsp;<span style="color:#79c0ff">"name"</span><span style="color:#555">:</span> <span style="color:#f97316">"Mohamad"</span><span style="color:#555">,</span><br>
&nbsp;&nbsp;<span style="color:#79c0ff">"alias"</span><span style="color:#555">:</span> <span style="color:#f97316">"M_One"</span><span style="color:#555">,</span><br>
&nbsp;&nbsp;<span style="color:#79c0ff">"role"</span><span style="color:#555">:</span> <span style="color:#f97316">"Android Developer"</span><span style="color:#555">,</span><br>
&nbsp;&nbsp;<span style="color:#79c0ff">"stack"</span><span style="color:#555">:</span> <span style="color:#555">[</span><span style="color:#f97316">"Kotlin"</span><span style="color:#555">,</span> <span style="color:#f97316">"Jetpack Compose"</span><span style="color:#555">,</span> <span style="color:#f97316">"Coroutines"</span><span style="color:#555">],</span><br>
&nbsp;&nbsp;<span style="color:#79c0ff">"status"</span><span style="color:#555">:</span> <span style="color:var(--green)">"🟢 Building in production"</span><br>
<span style="color:#555">}</span>
        </div>

        <!-- line 3: prompt waiting -->
        <div class="tline" id="l3" style="margin-top:20px;">
          <span class="p-root">root</span><span class="p-at">@</span><span class="p-host">github</span><span class="p-colon">:</span><span class="p-path">~/profile</span><span class="p-hash">#</span>
          <span class="block-cursor"></span>
        </div>

      </div><!-- /term-body -->
    </div><!-- /terminal -->
  </div>

  <!-- DIVIDER -->
  <div class="divider">
    <div class="divider-line"></div>
    <div class="divider-icon">◈ whoami ◈</div>
    <div class="divider-line"></div>
  </div>

  <!-- IDENTITY CODE BLOCK -->
  <div class="reveal code-block" style="margin-bottom:20px;">
    <div class="code-bar">
      <div class="code-dot red"></div>
      <div class="code-dot yellow"></div>
      <div class="code-dot green"></div>
      <span class="code-filename">identity.kt</span>
    </div>
    <pre><span class="k">data class</span> <span class="n">Developer</span><span class="p">(</span>
  <span class="k">val</span> <span class="v">name</span>   <span class="p">:</span> <span class="n">String</span> <span class="p">=</span> <span class="s">"Mohamad"</span><span class="p">,</span>
  <span class="k">val</span> <span class="v">role</span>   <span class="p">:</span> <span class="n">String</span> <span class="p">=</span> <span class="s">"Android Developer"</span><span class="p">,</span>
  <span class="k">val</span> <span class="v">focus</span>  <span class="p">:</span> <span class="n">String</span> <span class="p">=</span> <span class="s">"Native · Kotlin · Compose"</span><span class="p">,</span>
  <span class="k">val</span> <span class="v">motto</span>  <span class="p">:</span> <span class="n">String</span> <span class="p">= </span><span class="s">"""
    Write code that survives
    the test of time.
  """</span><span class="p">.</span><span class="v">trimIndent()</span><span class="p">,</span>
  <span class="k">val</span> <span class="v">status</span> <span class="p">:</span> <span class="n">String</span> <span class="p">=</span> <span class="s"><span class="e">🟢</span> "Building in production"</span>
<span class="p">)</span></pre>
  </div>

  <!-- DIVIDER -->
  <div class="divider reveal">
    <div class="divider-line"></div>
    <div class="divider-icon">◈ stack.kt ◈</div>
    <div class="divider-line"></div>
  </div>

  <!-- TECH STACK -->
  <div class="section-title reveal">// Tech Stack</div>
  <div class="stack-grid reveal">
    <div class="stack-pill"><span class="pill-name">Kotlin</span><span class="pill-stars">★★★★★</span></div>
    <div class="stack-pill"><span class="pill-name">Jetpack Compose</span><span class="pill-stars">★★★★★</span></div>
    <div class="stack-pill"><span class="pill-name">Coroutines + Flow</span><span class="pill-stars">★★★★★</span></div>
    <div class="stack-pill"><span class="pill-name">Clean Architecture</span><span class="pill-stars">★★★★★</span></div>
    <div class="stack-pill"><span class="pill-name">Room / DataStore</span><span class="pill-stars">★★★★☆</span></div>
    <div class="stack-pill"><span class="pill-name">Hilt / Koin</span><span class="pill-stars">★★★★☆</span></div>
  </div>

  <!-- DIVIDER -->
  <div class="divider reveal">
    <div class="divider-line"></div>
    <div class="divider-icon">◈ philosophy ◈</div>
    <div class="divider-line"></div>
  </div>

  <!-- PHILOSOPHY -->
  <div class="philosophy reveal">
    <p class="philosophy-quote">"Clean Architecture isn't a template — it's a discipline."</p>
    <ul class="philosophy-rules">
      <li>UI layer stays dumb. Logic lives in ViewModels.</li>
      <li>Repositories own the data, not Activities.</li>
      <li>Every coroutine has a scope. Every scope has a reason.</li>
      <li>If it's hard to test, it's hard to trust.</li>
    </ul>
  </div>

  <!-- DIVIDER -->
  <div class="divider reveal">
    <div class="divider-line"></div>
    <div class="divider-icon">◈ shipping ◈</div>
    <div class="divider-line"></div>
  </div>

  <!-- BUILD IN PROGRESS -->
  <div class="section-title reveal">// What I'm Shipping</div>

  <div class="progress-section reveal">
    <div class="progress-title"><span>BUILD IN PROGRESS</span><span id="pct">0%</span></div>
    <div class="progress-bar-wrap">
      <div class="progress-bar-fill" id="progressBar"></div>
    </div>
  </div>

  <div class="current-build reveal">
    <div class="build-item"><span class="build-dot-filled">◉</span> Scalable Android apps with offline-first architecture</div>
    <div class="build-item"><span class="build-dot-filled">◉</span> Compose UI systems that feel alive and responsive</div>
    <div class="build-item"><span class="build-dot-filled">◉</span> Kotlin Multiplatform exploration</div>
    <div class="build-item"><span class="build-dot-filled">◉</span> Performance profiling & memory leak hunting</div>
    <div class="build-item"><span class="build-dot-empty">◎</span> Next: Contributing to open source Android tooling</div>
  </div>

  <!-- FOOTER -->
  <div class="footer reveal">
    <p class="footer-text">Thanks for visiting &nbsp;·&nbsp; Star something if it helped &nbsp;·&nbsp; Always open to collaborate</p>
    <div class="footer-glow">⚡</div>
  </div>

</div>

<script>
  /* ── Terminal typing reveal ── */
  const delays = { l1: 300, out1: 700, l2: 1600, out2: 2000, l3: 2800 };
  Object.entries(delays).forEach(([id, ms]) => {
    const el = document.getElementById(id);
    if (el) setTimeout(() => { el.style.opacity = '1'; }, ms);
  });

  /* ── Particles ── */
  const container = document.getElementById('particles');
  for (let i = 0; i < 40; i++) {
    const p = document.createElement('div');
    p.className = 'particle';
    p.style.cssText = `
      left: ${Math.random() * 100}%;
      width: ${Math.random() > 0.7 ? 3 : 2}px;
      height: ${Math.random() > 0.7 ? 3 : 2}px;
      animation-duration: ${6 + Math.random() * 12}s;
      animation-delay: ${Math.random() * 10}s;
      opacity: ${0.3 + Math.random() * 0.4};
    `;
    container.appendChild(p);
  }

  /* ── Scroll Reveal ── */
  const observer = new IntersectionObserver((entries) => {
    entries.forEach((e, i) => {
      if (e.isIntersecting) {
        setTimeout(() => e.target.classList.add('visible'), i * 80);
        observer.unobserve(e.target);
      }
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.reveal').forEach(el => observer.observe(el));

  /* ── Progress Bar ── */
  const progressObs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting) {
        const bar = document.getElementById('progressBar');
        const pct = document.getElementById('pct');
        const target = 87;
        bar.style.width = target + '%';
        let current = 0;
        const timer = setInterval(() => {
          current++;
          pct.textContent = current + '%';
          if (current >= target) clearInterval(timer);
        }, 1500 / target);
        progressObs.unobserve(e.target);
      }
    });
  }, { threshold: 0.5 });
  progressObs.observe(document.getElementById('progressBar'));
</script>
</body>
</html>
