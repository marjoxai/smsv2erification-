[<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Private Account — Verification Numbers</title>
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #080b10;
      --surface: #0e1318;
      --border: #1e2730;
      --green: #00ff88;
      --green-dim: #00cc6a;
      --text: #e8edf2;
      --muted: #5a6a78;
      --card-bg: #0d1219;
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Syne', sans-serif;
      overflow-x: hidden;
      min-height: 100vh;
    }

    /* ── NOISE OVERLAY ── */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 512 512' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 0;
      opacity: 0.4;
    }

    /* ── GLOW BLOBS ── */
    .blob {
      position: fixed;
      border-radius: 50%;
      filter: blur(120px);
      pointer-events: none;
      z-index: 0;
      opacity: 0.12;
    }
    .blob-1 { width: 500px; height: 500px; background: var(--green); top: -150px; right: -100px; }
    .blob-2 { width: 400px; height: 400px; background: #0055ff; bottom: 100px; left: -120px; }

    /* ── NAV ── */
    nav {
      position: fixed;
      top: 0; left: 0; right: 0;
      z-index: 100;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 20px 40px;
      background: rgba(8,11,16,0.7);
      backdrop-filter: blur(16px);
      border-bottom: 1px solid var(--border);
    }

    .nav-logo {
      font-family: 'Space Mono', monospace;
      font-weight: 700;
      font-size: 1rem;
      letter-spacing: 0.08em;
      color: var(--green);
    }

    .nav-badge {
      font-family: 'Space Mono', monospace;
      font-size: 0.65rem;
      color: var(--muted);
      letter-spacing: 0.12em;
      text-transform: uppercase;
      border: 1px solid var(--border);
      padding: 4px 10px;
      border-radius: 20px;
    }

    /* ── HERO ── */
    .hero {
      position: relative;
      z-index: 1;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 120px 24px 80px;
    }

    .hero-tag {
      font-family: 'Space Mono', monospace;
      font-size: 0.7rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--green);
      background: rgba(0,255,136,0.08);
      border: 1px solid rgba(0,255,136,0.2);
      padding: 6px 16px;
      border-radius: 20px;
      display: inline-block;
      margin-bottom: 28px;
      animation: fadeUp 0.6s ease both;
    }

    .hero h1 {
      font-size: clamp(3rem, 10vw, 7.5rem);
      font-weight: 800;
      line-height: 0.95;
      letter-spacing: -0.03em;
      animation: fadeUp 0.6s 0.1s ease both;
    }

    .hero h1 .accent {
      color: var(--green);
      display: block;
    }

    .hero-sub {
      margin-top: 28px;
      max-width: 480px;
      font-size: 1.05rem;
      color: var(--muted);
      line-height: 1.65;
      animation: fadeUp 0.6s 0.2s ease both;
    }

    /* ── CTA BUTTON ── */
    .cta-wrap {
      margin-top: 48px;
      animation: fadeUp 0.6s 0.3s ease both;
    }

    .cta-btn {
      display: inline-flex;
      align-items: center;
      gap: 12px;
      background: var(--green);
      color: #050807;
      font-family: 'Space Mono', monospace;
      font-weight: 700;
      font-size: 0.9rem;
      letter-spacing: 0.05em;
      padding: 18px 36px;
      border-radius: 4px;
      text-decoration: none;
      border: none;
      cursor: pointer;
      transition: transform 0.2s, box-shadow 0.2s, background 0.2s;
      box-shadow: 0 0 40px rgba(0,255,136,0.25);
    }

    .cta-btn:hover {
      transform: translateY(-3px);
      box-shadow: 0 0 60px rgba(0,255,136,0.4);
      background: #1aff97;
    }

    .cta-btn:active { transform: translateY(0); }

    .cta-btn svg { width: 22px; height: 22px; flex-shrink: 0; }

    /* ── SCROLL HINT ── */
    .scroll-hint {
      margin-top: 72px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
      color: var(--muted);
      font-family: 'Space Mono', monospace;
      font-size: 0.6rem;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      animation: fadeUp 0.6s 0.5s ease both;
    }

    .scroll-line {
      width: 1px;
      height: 40px;
      background: linear-gradient(to bottom, var(--green), transparent);
      animation: scrollPulse 2s ease-in-out infinite;
    }

    /* ── FEATURES ── */
    .features {
      position: relative;
      z-index: 1;
      max-width: 1000px;
      margin: 0 auto;
      padding: 80px 24px 120px;
    }

    .section-label {
      font-family: 'Space Mono', monospace;
      font-size: 0.65rem;
      letter-spacing: 0.25em;
      text-transform: uppercase;
      color: var(--green);
      margin-bottom: 16px;
    }

    .section-title {
      font-size: clamp(1.8rem, 4vw, 2.8rem);
      font-weight: 800;
      letter-spacing: -0.02em;
      margin-bottom: 56px;
      color: var(--text);
    }

    .features-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
      gap: 2px;
    }

    .feature-card {
      background: var(--card-bg);
      border: 1px solid var(--border);
      padding: 36px 32px;
      transition: border-color 0.2s, background 0.2s;
      position: relative;
      overflow: hidden;
    }

    .feature-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 2px;
      background: var(--green);
      transform: scaleX(0);
      transform-origin: left;
      transition: transform 0.3s ease;
    }

    .feature-card:hover { background: #111820; border-color: #2a3845; }
    .feature-card:hover::before { transform: scaleX(1); }

    .feat-icon {
      font-size: 1.8rem;
      margin-bottom: 20px;
      display: block;
    }

    .feat-title {
      font-size: 1.1rem;
      font-weight: 700;
      margin-bottom: 10px;
      letter-spacing: -0.01em;
    }

    .feat-desc {
      font-size: 0.88rem;
      color: var(--muted);
      line-height: 1.65;
    }

    /* ── HOW IT WORKS ── */
    .how {
      position: relative;
      z-index: 1;
      background: var(--surface);
      border-top: 1px solid var(--border);
      border-bottom: 1px solid var(--border);
      padding: 100px 24px;
    }

    .how-inner {
      max-width: 900px;
      margin: 0 auto;
    }

    .steps {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 40px;
      margin-top: 56px;
    }

    .step {
      display: flex;
      flex-direction: column;
      gap: 14px;
    }

    .step-num {
      font-family: 'Space Mono', monospace;
      font-size: 2.5rem;
      font-weight: 700;
      color: var(--green);
      opacity: 0.25;
      line-height: 1;
    }

    .step-title {
      font-size: 1rem;
      font-weight: 700;
    }

    .step-desc {
      font-size: 0.85rem;
      color: var(--muted);
      line-height: 1.6;
    }

    /* ── BOTTOM CTA ── */
    .bottom-cta {
      position: relative;
      z-index: 1;
      text-align: center;
      padding: 120px 24px;
    }

    .bottom-cta h2 {
      font-size: clamp(2rem, 5vw, 3.5rem);
      font-weight: 800;
      letter-spacing: -0.03em;
      margin-bottom: 16px;
    }

    .bottom-cta p {
      color: var(--muted);
      font-size: 1rem;
      margin-bottom: 40px;
    }

    /* ── FOOTER ── */
    footer {
      position: relative;
      z-index: 1;
      border-top: 1px solid var(--border);
      padding: 28px 40px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 12px;
    }

    footer span {
      font-family: 'Space Mono', monospace;
      font-size: 0.7rem;
      color: var(--muted);
      letter-spacing: 0.08em;
    }

    .footer-tag {
      color: var(--green);
      opacity: 0.7;
    }

    /* ── ANIMATIONS ── */
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(24px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    @keyframes scrollPulse {
      0%, 100% { opacity: 0.3; transform: scaleY(1); }
      50%       { opacity: 1;   transform: scaleY(1.15); }
    }

    /* ── RESPONSIVE ── */
    @media (max-width: 600px) {
      nav { padding: 16px 20px; }
      .features, .bottom-cta { padding-left: 16px; padding-right: 16px; }
      footer { padding: 20px; }
    }
  </style>
</head>
<body>

  <div class="blob blob-1"></div>
  <div class="blob blob-2"></div>

  <!-- NAV -->
  <nav>
    <span class="nav-logo">PRIVATE_ACCOUNT</span>
    <span class="nav-badge">Verification Numbers</span>
  </nav>

  <!-- HERO -->
  <section class="hero">
    <span class="hero-tag">🔒 Instant · Secure · Verified</span>
    <h1>
      Real Numbers.<br>
      <span class="accent">Any Country.</span>
    </h1>
    <p class="hero-sub">
      Get virtual phone numbers from multiple countries for app verification — fast, discreet, and affordable.
    </p>
    <div class="cta-wrap">
      <a href="https://wa.me/2349169052481" target="_blank" rel="noopener" class="cta-btn">
        <!-- WhatsApp Icon -->
        <svg viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
          <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.886 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/>
        </svg>
        Chat on WhatsApp
      </a>
    </div>
    <div class="scroll-hint">
      <div class="scroll-line"></div>
      Scroll
    </div>
  </section>

  <!-- FEATURES -->
  <section class="features">
    <p class="section-label">// What we offer</p>
    <h2 class="section-title">Everything you need<br>to stay verified.</h2>
    <div class="features-grid">
      <div class="feature-card">
        <span class="feat-icon">🌍</span>
        <div class="feat-title">Multiple Countries</div>
        <p class="feat-desc">Numbers from a wide range of countries. Whatever app or platform you need to verify — we've got you covered.</p>
      </div>
      <div class="feature-card">
        <span class="feat-icon">⚡</span>
        <div class="feat-title">Instant Delivery</div>
        <p class="feat-desc">No waiting. Once you place an order, your number is delivered straight via WhatsApp in minutes.</p>
      </div>
      <div class="feature-card">
        <span class="feat-icon">🔐</span>
        <div class="feat-title">Private & Discreet</div>
        <p class="feat-desc">Your identity stays yours. We don't ask unnecessary questions — just fast, clean service.</p>
      </div>
      <div class="feature-card">
        <span class="feat-icon">💰</span>
        <div class="feat-title">Affordable Rates</div>
        <p class="feat-desc">Competitive pricing with no hidden charges. Pay per number or ask about bulk deals.</p>
      </div>
    </div>
  </section>

  <!-- HOW IT WORKS -->
  <section class="how">
    <div class="how-inner">
      <p class="section-label">// How it works</p>
      <h2 class="section-title">Three steps. Done.</h2>
      <div class="steps">
        <div class="step">
          <div class="step-num">01</div>
          <div class="step-title">Message Us</div>
          <p class="step-desc">Hit the WhatsApp button and tell us which country and platform you need a number for.</p>
        </div>
        <div class="step">
          <div class="step-num">02</div>
          <div class="step-title">Make Payment</div>
          <p class="step-desc">We'll confirm availability and share the price. Quick and easy payment process.</p>
        </div>
        <div class="step">
          <div class="step-num">03</div>
          <div class="step-title">Get Your Number</div>
          <p class="step-desc">Receive your working number instantly and complete your verification with ease.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- BOTTOM CTA -->
  <section class="bottom-cta">
    <h2>Ready to get started?</h2>
    <p>Tap below and we'll sort you out in minutes.</p>
    <a href="https://wa.me/2349169052481" target="_blank" rel="noopener" class="cta-btn">
      <svg viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg" style="width:22px;height:22px">
        <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.886 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/>
      </svg>
      Order via WhatsApp
    </a>
  </section>

  <!-- FOOTER -->
  <footer>
    <span>© 2026 <span class="footer-tag">PRIVATE_ACCOUNT</span></span>
    <span>+234 916 905 2481</span>
  </footer>

</body>
</html>
](https://github.com/marjoxai/smsv2erification-/commit/189ec1a52e80ee736f0f96bd1a9aa2fd09a464e7)
