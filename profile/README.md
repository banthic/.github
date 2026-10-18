<!DOCTYPE html>
<html lang="en" class="dark">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Banthic Labs — README</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Courier+Prime:wght@400;700&display=swap" rel="stylesheet" />
<style>
  @property --glow-angle { syntax: '<angle>'; initial-value: 0deg; inherits: false; }

  :root {
    --background: #000000;
    --foreground: #fafafa;
    --muted-foreground: #a1a1a1;
    --frame-border: rgba(255, 255, 255, 0.22);
    --cross: rgba(255, 255, 255, 0.3);
    --grid-line: rgba(255, 255, 255, 0.06);
    --font-sans: 'Inter', sans-serif;
    --font-typewriter: 'Courier Prime', ui-monospace, monospace;
    --font-mono: ui-monospace, 'SFMono-Regular', 'JetBrains Mono', 'Courier New', monospace;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { background: var(--background); }

  body {
    background: var(--background);
    color: var(--foreground);
    font-family: var(--font-sans);
    line-height: 1.5;
    -webkit-font-smoothing: antialiased;
    padding: 64px 24px 96px;
    position: relative;
  }

  .page {
    max-width: 760px;
    margin: 0 auto;
    position: relative;
  }

  /* ASCII noise field */
  .ascii-field {
    position: fixed;
    inset: 0;
    z-index: -1;
    overflow: hidden;
    pointer-events: none;
  }
  .ascii-field pre {
    font-family: var(--font-mono);
    font-size: 11px;
    line-height: 1.15;
    color: var(--foreground);
    opacity: 0.07;
    white-space: pre;
    word-break: break-all;
  }

  /* Frame + crosshairs */
  .frame {
    position: relative;
    border: 1px solid var(--frame-border);
    background: var(--background);
  }
  .cross-tl::before,
  .cross-br::after {
    content: '';
    position: absolute;
    width: 13px;
    height: 13px;
    background:
      linear-gradient(var(--cross) 0 0) center / 1px 100% no-repeat,
      linear-gradient(var(--cross) 0 0) center / 100% 1px no-repeat;
  }
  .cross-tl::before { top: -7px; left: -7px; }
  .cross-br::after { bottom: -7px; right: -7px; }

  /* Grid paper */
  .grid-lines {
    background-image:
      linear-gradient(var(--grid-line) 1px, transparent 1px),
      linear-gradient(90deg, var(--grid-line) 1px, transparent 1px);
    background-size: 48px 48px;
  }

  /* Glow */
  .glow-wrap { position: relative; isolation: isolate; }
  .glow-halo,
  .glow-grain {
    position: absolute;
    pointer-events: none;
  }

  /* Thin rotating spectrum edge halo */
  .glow-halo {
    inset: -2px;
    z-index: 0;
    filter: blur(10px);
    opacity: 0.9;
    background: conic-gradient(
      from 0deg,
      #ffffff,
      #ff3d3d 70deg,
      #d946ef 140deg,
      #5b6cff 210deg,
      #22d3ee 285deg,
      #ffffff
    );
    animation: glow-spin 8s linear infinite;
  }
  .glow-halo::after {
    content: '';
    position: absolute;
    inset: 0;
    background: conic-gradient(rgba(0,0,0,0.5) 0 25%, transparent 0 50%, rgba(0,0,0,0.5) 0 75%, transparent 0);
    background-size: 4px 4px;
  }
  .glow-grain {
    inset: -10px;
    z-index: 0;
    opacity: 0.45;
    mix-blend-mode: overlay;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 90 90' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3CfeComponentTransfer%3E%3CfeFuncR type='linear' slope='1.4' intercept='-0.2'/%3E%3CfeFuncG type='linear' slope='1.4' intercept='-0.2'/%3E%3CfeFuncB type='linear' slope='1.4' intercept='-0.2'/%3E%3C/feComponentTransfer%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size: 90px 90px;
  }

  /* Card sits above the glow layers */
  .glow-wrap > .frame {
    position: relative;
    z-index: 1;
  }

  @keyframes glow-spin {
    to { --glow-angle: 360deg; }
    from { background: conic-gradient(from var(--glow-angle), #ffffff, #ff3d3d 70deg, #d946ef 140deg, #5b6cff 210deg, #22d3ee 285deg, #ffffff); }
    to { background: conic-gradient(from var(--glow-angle), #ffffff, #ff3d3d 70deg, #d946ef 140deg, #5b6cff 210deg, #22d3ee 285deg, #ffffff); }
  }

  /* Typewriter eyebrow */
  .eyebrow {
    font-family: var(--font-typewriter);
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: 0.22em;
    color: var(--muted-foreground);
  }

  /* Title block */
  .hero {
    padding: 56px 40px;
    text-align: left;
  }
  .hero h1 {
    font-size: clamp(2.5rem, 6vw, 4.5rem);
    font-weight: 800;
    letter-spacing: -0.04em;
    line-height: 1.05;
    color: var(--foreground);
    margin-bottom: 20px;
  }
  .hero .tagline {
    font-size: 1.125rem;
    font-weight: 500;
    color: var(--foreground);
    margin-bottom: 20px;
    max-width: 52ch;
  }
  .hero p {
    font-size: 1rem;
    color: var(--muted-foreground);
    line-height: 1.65;
    max-width: 58ch;
    margin-bottom: 14px;
  }

  /* Sections */
  .section {
    margin-top: 48px;
    padding: 32px 40px;
  }
  .section > .eyebrow {
    display: block;
    margin-bottom: 12px;
  }
  .section h2 {
    font-size: 1.875rem;
    font-weight: 600;
    letter-spacing: -0.02em;
    margin-bottom: 20px;
  }
  .section h3 {
    font-size: 1.25rem;
    font-weight: 600;
    letter-spacing: -0.01em;
    margin-bottom: 6px;
  }
  .section p {
    font-size: 0.95rem;
    color: var(--muted-foreground);
    line-height: 1.65;
    margin-bottom: 16px;
    max-width: 62ch;
  }

  /* Exploring list */
  .explore-list { list-style: none; }
  .explore-list li {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 20px;
    padding: 18px 0;
    border-top: 1px solid rgba(250,250,250,0.1);
  }
  .explore-list li:last-child { border-bottom: 1px solid rgba(250,250,250,0.1); }
  .explore-list .idx {
    font-family: var(--font-typewriter);
    font-size: 12px;
    color: var(--muted-foreground);
    padding-top: 3px;
  }
  .explore-list strong {
    color: var(--foreground);
    font-weight: 600;
    display: block;
    margin-bottom: 2px;
    font-size: 1rem;
  }
  .explore-list span {
    color: var(--muted-foreground);
    font-size: 0.9rem;
  }

  /* Projects */
  .project {
    padding: 24px 0;
    border-top: 1px solid rgba(250,250,250,0.1);
  }
  .project:last-child { border-bottom: 1px solid rgba(250,250,250,0.1); }
  .project .eyebrow { font-size: 11px; margin-bottom: 4px; display: block; }

  /* Approach */
  .approach-item {
    padding: 24px 0;
    border-top: 1px solid rgba(250,250,250,0.1);
  }
  .approach-item:last-child { border-bottom: 1px solid rgba(250,250,250,0.1); }
  .approach-item h3 {
    color: var(--foreground);
    margin-bottom: 8px;
  }
  .approach-item p { margin-bottom: 0; }

  /* Get involved */
  .involve-list {
    list-style: none;
  }
  .involve-list li {
    padding: 10px 0;
    padding-left: 20px;
    position: relative;
    color: var(--muted-foreground);
    font-size: 0.95rem;
  }
  .involve-list li::before {
    content: '+';
    position: absolute;
    left: 0;
    top: 10px;
    font-family: var(--font-typewriter);
    color: var(--foreground);
    font-size: 12px;
  }

  /* Connect */
  .connect {
    margin-top: 48px;
    padding: 28px 40px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 24px;
  }
  .connect .eyebrow { margin-bottom: 4px; display: block; }
  .connect strong { font-size: 1.1rem; font-weight: 600; }
  .connect .tag {
    font-family: var(--font-typewriter);
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: 0.22em;
    padding: 6px 14px;
    border: 1px solid rgba(250,250,250,0.2);
    color: var(--foreground);
    white-space: nowrap;
  }

  /* Footer */
  .footer {
    margin-top: 40px;
    text-align: center;
    font-size: 0.8rem;
    color: var(--muted-foreground);
  }
  .footer span {
    background: linear-gradient(to bottom, var(--foreground), rgba(250,250,250,0.6));
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
  }

  @media (max-width: 600px) {
    body { padding: 32px 14px 64px; }
    .hero, .section, .connect { padding-left: 20px; padding-right: 20px; }
    .explore-list li { grid-template-columns: 1fr; gap: 4px; }
    .connect { flex-direction: column; align-items: flex-start; }
  }

  @media print {
    body { padding: 24px; }
    .glow-halo { display: none; }
    .frame { break-inside: avoid; }
  }
</style>
</head>
<body>

<div class="ascii-field" aria-hidden="true">
<pre id="ascii"></pre>
</div>

<main class="page">

  <!-- HERO -->
  <div class="glow-wrap">
    <div class="glow-halo"></div>
    <div class="glow-grain"></div>
    <section class="frame cross-tl cross-br grid-lines hero">
      <span class="eyebrow">Banthic Labs — Readme</span>
      <h1>BANTHIC&nbsp;LABS</h1>
      <p class="tagline">Building useful things around AI, open source and the people who use them.</p>
      <p>Banthic Labs is an independent space for building, experimenting with and exploring software around artificial intelligence.</p>
      <p>We believe the future of AI isn't just about bigger models. It's also about the tools, infrastructure and communities that make AI useful in the real world.</p>
      <p>We build practical projects, explore new ideas and share what we learn along the way.</p>
    </section>
  </div>

  <!-- WHAT WE'RE EXPLORING -->
  <section class="frame cross-tl cross-br section">
    <span class="eyebrow">01</span>
    <h2>What we're exploring</h2>
    <ul class="explore-list">
      <li>
        <span class="idx">01</span>
        <div><strong>AI tooling</strong><span>Making AI easier to build with, integrate and use.</span></div>
      </li>
      <li>
        <span class="idx">02</span>
        <div><strong>Agents &amp; workflows</strong><span>Creating practical foundations for AI agents that can use tools and take action.</span></div>
      </li>
      <li>
        <span class="idx">03</span>
        <div><strong>Open source</strong><span>Building reusable software that others can understand, adapt and extend.</span></div>
      </li>
      <li>
        <span class="idx">04</span>
        <div><strong>AI &amp; communities</strong><span>Exploring how AI can improve communication, knowledge sharing and collaboration.</span></div>
      </li>
    </ul>
    <p style="margin-top:20px;">Our interests evolve. These are areas we're exploring, not boundaries we're committing to.</p>
  </section>

  <!-- PROJECTS -->
  <section class="frame cross-tl cross-br section">
    <span class="eyebrow">02</span>
    <h2>Projects</h2>

    <div class="project">
      <span class="eyebrow">Discord</span>
      <h3>Nestor</h3>
      <p>An AI assistant for Discord communities, designed to make community knowledge more accessible and help people get answers in context.</p>
    </div>

    <div class="project">
      <span class="eyebrow">Agent</span>
      <h3>Zetelton-Agent</h3>
      <p>A simple, reproducible foundation for building AI agents. Focused on keeping the essentials understandable without unnecessary complexity.</p>
    </div>

    <div class="project">
      <span class="eyebrow">CLI</span>
      <h3>Zetelton-Init</h3>
      <p>A command-line tool designed to simplify getting started with Zetelton-Agent.</p>
    </div>
  </section>

  <!-- OUR APPROACH -->
  <section class="frame cross-tl cross-br section">
    <span class="eyebrow">03</span>
    <h2>Our approach</h2>

    <div class="approach-item">
      <h3>Keep it simple.</h3>
      <p>Useful software doesn't need unnecessary complexity. We aim to build tools that are easy to understand, configure and extend.</p>
    </div>
    <div class="approach-item">
      <h3>Stay open.</h3>
      <p>We value open source, interoperability and the freedom to choose the tools that work for you.</p>
    </div>
    <div class="approach-item">
      <h3>Build to learn.</h3>
      <p>Experiments, prototypes and small projects are part of the process. Not every idea needs to become a product.</p>
    </div>
    <div class="approach-item">
      <h3>Think beyond the model.</h3>
      <p>Good AI systems depend on more than the model itself. Tools, interfaces, infrastructure and the people using them all matter.</p>
    </div>
  </section>

  <!-- GET INVOLVED -->
  <section class="frame cross-tl cross-br section">
    <span class="eyebrow">04</span>
    <h2>Get involved</h2>
    <p>Banthic Labs is an evolving project, and we're interested in connecting with people who share a curiosity for AI, developer tooling and open source.</p>
    <ul class="involve-list">
      <li>Explore our repositories.</li>
      <li>Share feedback and ideas.</li>
      <li>Contribute code, documentation or experiments.</li>
      <li>Help us explore new directions.</li>
    </ul>
  </section>

  <!-- CONNECT -->
  <div class="frame cross-tl cross-br connect">
    <div>
      <span class="eyebrow">Connect</span>
      <strong>Discord: vzn.d3v</strong>
    </div>
    <span class="tag">Open source</span>
  </div>

  <p class="footer">
    <span>Independent by nature. Open by default. Built one experiment at a time.</span>
  </p>

</main>

<script>
  // ASCII background field
  const chars = '01#*+:-=.<>AB3F7E9C2';
  const el = document.getElementById('ascii');
  const cols = 120, rows = 90;
  let out = '';
  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      out += Math.random() < 0.3 ? ' ' : chars[Math.floor(Math.random() * chars.length)];
    }
    out += '\n';
  }
  el.textContent = out;
</script>

</body>
</html>
