# LASUASA-election-
For voting 
with open('/tmp/lasu_b64.txt') as f: lasu = f.read().strip()
with open('/tmp/asa_b64.txt') as f: asa = f.read().strip()

html = f"""<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LASUASA — IEC Voting Portal</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700;900&family=IBM+Plex+Mono:wght@400;500&family=IBM+Plex+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {{
    --dark-green: #0d3320;
    --mid-green: #1a5c35;
    --light-green: #2e7d50;
    --gold: #c9a84c;
    --gold-light: #e8c97a;
    --cream: #fdf8ee;
    --white: #ffffff;
    --text: #1a1a1a;
    --mid: #5a6a5e;
    --border: #c9a84c44;
  }}

  * {{ margin: 0; padding: 0; box-sizing: border-box; }}

  body {{
    font-family: 'IBM Plex Sans', sans-serif;
    min-height: 100vh;
    background: #0a2818;
    background-image:
      radial-gradient(ellipse at top left, #1a5c3522 0%, transparent 60%),
      radial-gradient(ellipse at bottom right, #c9a84c11 0%, transparent 60%),
      repeating-linear-gradient(
        45deg,
        transparent,
        transparent 40px,
        rgba(201,168,76,0.03) 40px,
        rgba(201,168,76,0.03) 41px
      );
    color: var(--text);
    overflow-x: hidden;
  }}

  /* ── HEADER ── */
  header {{
    background: linear-gradient(135deg, #0a2818 0%, #0d3320 50%, #0a2818 100%);
    border-bottom: 2px solid var(--gold);
    padding: 0.9rem 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: 0 2px 20px rgba(0,0,0,0.5);
  }}

  .header-logos {{
    display: flex;
    align-items: center;
    gap: 1rem;
  }}

  .header-logos img {{
    height: 52px;
    width: 52px;
    object-fit: contain;
    filter: drop-shadow(0 2px 4px rgba(0,0,0,0.4));
  }}

  .header-title {{
    text-align: center;
    flex: 1;
  }}

  .header-title .org {{
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.62rem;
    letter-spacing: 0.18em;
    color: var(--gold-light);
    text-transform: uppercase;
    margin-bottom: 2px;
  }}

  .header-title .portal-name {{
    font-family: 'Playfair Display', serif;
    font-size: 1.25rem;
    font-weight: 900;
    color: var(--white);
    letter-spacing: 0.04em;
  }}

  .header-title .portal-name span {{ color: var(--gold); }}

  .live-badge {{
    display: inline-flex;
    align-items: center;
    gap: 0.4rem;
    background: #c0392b;
    color: white;
    padding: 3px 10px;
    border-radius: 2px;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.6rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }}

  .live-dot {{
    width: 6px; height: 6px;
    background: white;
    border-radius: 50%;
    animation: blink 1.2s infinite;
  }}

  @keyframes blink {{ 0%,100% {{ opacity:1 }} 50% {{ opacity:0 }} }}

  /* ── HERO BANNER ── */
  .hero {{
    background: linear-gradient(180deg, #0d3320 0%, #0a2818 100%);
    border-bottom: 3px solid var(--gold);
    padding: 3rem 2rem 2.5rem;
    text-align: center;
    position: relative;
    overflow: hidden;
  }}

  .hero::before {{
    content: '';
    position: absolute;
    inset: 0;
    background:
      radial-gradient(circle at 20% 50%, rgba(201,168,76,0.06) 0%, transparent 50%),
      radial-gradient(circle at 80% 50%, rgba(46,125,80,0.08) 0%, transparent 50%);
    pointer-events: none;
  }}

  .hero-logos {{
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 2rem;
    margin-bottom: 1.5rem;
  }}

  .hero-logos img {{
    height: 90px;
    width: 90px;
    object-fit: contain;
    filter: drop-shadow(0 4px 12px rgba(0,0,0,0.5));
    transition: transform 0.3s;
  }}

  .hero-logos img:hover {{ transform: scale(1.05); }}

  .hero-logos .divider {{
    width: 1px;
    height: 70px;
    background: linear-gradient(180deg, transparent, var(--gold), transparent);
  }}

  .hero-tag {{
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.68rem;
    letter-spacing: 0.22em;
    color: var(--gold);
    text-transform: uppercase;
    margin-bottom: 0.7rem;
  }}

  .hero h1 {{
    font-family: 'Playfair Display', serif;
    font-size: clamp(1.6rem, 4vw, 2.8rem);
    font-weight: 900;
    color: var(--white);
    line-height: 1.15;
    margin-bottom: 0.5rem;
  }}

  .hero h1 span {{ color: var(--gold); }}

  .hero-sub {{
    font-size: 0.88rem;
    color: #8aaa96;
    max-width: 500px;
    margin: 0 auto;
    line-height: 1.6;
  }}

  .motto {{
    margin-top: 1.2rem;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.72rem;
    color: var(--gold-light);
    letter-spacing: 0.12em;
    font-style: italic;
  }}

  /* ── MAIN ── */
  main {{
    max-width: 780px;
    margin: 0 auto;
    padding: 2.5rem 1.5rem;
  }}

  .section-label {{
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 0.18em;
    text-transform: uppercase;
    color: var(--gold);
    border-left: 3px solid var(--gold);
    padding-left: 0.7rem;
    margin-bottom: 1.2rem;
  }}

  /* ── VOTER CARD ── */
  .voter-card {{
    background: linear-gradient(135deg, #ffffff 0%, #f8fdf9 100%);
    border: 1px solid var(--gold);
    border-top: 4px solid var(--dark-green);
    padding: 2rem;
    margin-bottom: 2.5rem;
    box-shadow: 0 8px 32px rgba(0,0,0,0.25);
    border-radius: 2px;
  }}

  .voter-card-header {{
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 0.4rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid #e0ead4;
  }}

  .voter-card-header img {{
    height: 42px;
    width: 42px;
    object-fit: contain;
  }}

  .voter-card-header .card-titles h2 {{
    font-family: 'Playfair Display', serif;
    font-size: 1.2rem;
    color: var(--dark-green);
    font-weight: 700;
  }}

  .voter-card-header .card-titles p {{
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.62rem;
    color: var(--mid);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-top: 2px;
  }}

  .form-grid {{
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.2rem;
    margin-top: 1.2rem;
  }}

  @media (max-width: 560px) {{ .form-grid {{ grid-template-columns: 1fr; }} }}

  .field label {{
    display: block;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.62rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--mid-green);
    margin-bottom: 0.35rem;
    font-weight: 500;
  }}

  .field input, .field select {{
    width: 100%;
    padding: 0.6rem 0.8rem;
    border: 1px solid #b8d4c0;
    background: #f4faf6;
    font-family: 'IBM Plex Sans', sans-serif;
    font-size: 0.9rem;
    color: var(--text);
    outline: none;
    transition: border 0.2s, background 0.2s;
    border-radius: 2px;
    appearance: none;
  }}

  .field input:focus, .field select:focus {{
    border-color: var(--mid-green);
    background: white;
    box-shadow: 0 0 0 3px rgba(26,92,53,0.08);
  }}

  .field-full {{ grid-column: 1 / -1; }}

  .verify-btn {{
    grid-column: 1 / -1;
    background: linear-gradient(135deg, var(--dark-green), var(--mid-green));
    color: var(--cream);
    border: none;
    padding: 0.85rem 2rem;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.78rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    cursor: pointer;
    transition: all 0.2s;
    width: 100%;
    border-radius: 2px;
    border-bottom: 2px solid var(--gold);
    margin-top: 0.5rem;
  }}

  .verify-btn:hover {{ background: linear-gradient(135deg, var(--mid-green), var(--light-green)); transform: translateY(-1px); box-shadow: 0 4px 12px rgba(0,0,0,0.2); }}
  .verify-btn:active {{ transform: translateY(0); }}

  /* ── BALLOT ── */
  #ballot-section {{ display: none; }}
  #ballot-section.visible {{ display: block; }}

  .race-block {{
    background: white;
    border: 1px solid #d0e4d8;
    margin-bottom: 1.5rem;
    border-radius: 2px;
    overflow: hidden;
    box-shadow: 0 4px 16px rgba(0,0,0,0.15);
    animation: slideUp 0.4s ease both;
  }}

  @keyframes slideUp {{ from {{ opacity:0; transform:translateY(16px) }} to {{ opacity:1; transform:translateY(0) }} }}

  .race-header {{
    background: linear-gradient(135deg, var(--dark-green), var(--mid-green));
    color: white;
    padding: 0.9rem 1.4rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 2px solid var(--gold);
  }}

  .race-title {{
    font-family: 'Playfair Display', serif;
    font-size: 1rem;
    font-weight: 700;
  }}

  .race-meta {{
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.6rem;
    color: var(--gold-light);
    letter-spacing: 0.1em;
  }}

  .candidates {{ padding: 0.8rem 1.2rem; display: flex; flex-direction: column; gap: 0.6rem; }}

  .candidate-option {{
    display: flex;
    align-items: center;
    gap: 0.9rem;
    padding: 0.8rem 0.9rem;
    border: 1px solid #ddeee4;
    cursor: pointer;
    transition: all 0.18s;
    position: relative;
    border-radius: 2px;
    background: #fafffe;
  }}

  .candidate-option:hover {{ border-color: var(--mid-green); background: #f0faf4; }}
  .candidate-option.selected {{ border-color: var(--mid-green); background: #eef8f2; }}
  .candidate-option.selected::before {{
    content: '';
    position: absolute;
    left: 0; top: 0; bottom: 0;
    width: 3px;
    background: var(--mid-green);
    border-radius: 2px 0 0 2px;
  }}

  .candidate-option input[type="radio"] {{ display: none; }}

  .candidate-avatar {{
    width: 40px; height: 40px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Playfair Display', serif;
    font-size: 0.95rem;
    font-weight: 700;
    color: white;
    flex-shrink: 0;
    border: 2px solid rgba(255,255,255,0.3);
  }}

  .candidate-info {{ flex: 1; }}
  .candidate-name {{ font-family: 'Playfair Display', serif; font-size: 0.95rem; font-weight: 700; color: var(--dark-green); margin-bottom: 1px; }}
  .candidate-party {{ font-family: 'IBM Plex Mono', monospace; font-size: 0.6rem; color: var(--mid); letter-spacing: 0.08em; text-transform: uppercase; }}

  .check-icon {{
    width: 20px; height: 20px;
    border-radius: 50%;
    border: 2px solid #ccc;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.7rem;
    transition: all 0.18s;
    flex-shrink: 0;
  }}

  .candidate-option.selected .check-icon {{
    background: var(--mid-green);
    border-color: var(--mid-green);
    color: white;
  }}

  /* ── SUBMIT ── */
  .submit-zone {{
    background: linear-gradient(135deg, var(--dark-green), #0a2818);
    color: white;
    padding: 2rem;
    text-align: center;
    display: none;
    border-radius: 2px;
    border: 1px solid var(--gold);
    box-shadow: 0 8px 24px rgba(0,0,0,0.3);
    animation: slideUp 0.5s ease;
  }}

  .submit-zone.visible {{ display: block; }}
  .submit-zone h3 {{ font-family: 'Playfair Display', serif; font-size: 1.4rem; margin-bottom: 0.4rem; }}
  .submit-zone p {{ color: #8aaa96; font-size: 0.85rem; margin-bottom: 1.2rem; }}

  .submit-btn {{
    background: linear-gradient(135deg, #b8860b, var(--gold));
    color: var(--dark-green);
    border: none;
    padding: 0.85rem 2.5rem;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.8rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    cursor: pointer;
    font-weight: 700;
    transition: all 0.2s;
    border-radius: 2px;
  }}

  .submit-btn:hover {{ background: linear-gradient(135deg, var(--gold), var(--gold-light)); transform: translateY(-1px); }}

  /* ── SUCCESS ── */
  .success-screen {{ display: none; text-align: center; padding: 4rem 2rem; animation: fadeIn 0.6s ease; }}
  .success-screen.visible {{ display: block; }}
  @keyframes fadeIn {{ from {{ opacity:0 }} to {{ opacity:1 }} }}

  .success-seal {{
    width: 90px; height: 90px;
    background: linear-gradient(135deg, var(--dark-green), var(--mid-green));
    border-radius: 50%;
    margin: 0 auto 1.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2.2rem;
    border: 3px solid var(--gold);
    animation: popIn 0.5s cubic-bezier(0.175,0.885,0.32,1.275);
    color: white;
  }}

  @keyframes popIn {{ from {{ transform:scale(0) }} to {{ transform:scale(1) }} }}

  .success-screen h2 {{ font-family: 'Playfair Display', serif; font-size: 2rem; font-weight: 900; color: white; margin-bottom: 0.4rem; }}
  .success-screen p {{ color: #8aaa96; margin-bottom: 0.3rem; font-size: 0.88rem; }}

  .confirmation-code {{
    font-family: 'IBM Plex Mono', monospace;
    font-size: 1rem;
    color: var(--gold);
    background: rgba(201,168,76,0.1);
    display: inline-block;
    padding: 0.5rem 1.5rem;
    border: 1px dashed var(--gold);
    margin-top: 1rem;
    letter-spacing: 0.2em;
    border-radius: 2px;
  }}

  /* ── FOOTER ── */
  footer {{
    background: #060f09;
    border-top: 2px solid var(--gold);
    padding: 1.5rem 2rem;
    text-align: center;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.65rem;
    color: #3a6a4a;
    letter-spacing: 0.08em;
    line-height: 2;
    margin-top: 2rem;
  }}

  footer img {{ height: 28px; opacity: 0.6; vertical-align: middle; margin: 0 6px; }}

  /* ── NOTIF ── */
  .notif {{
    position: fixed;
    bottom: 1.5rem; right: 1.5rem;
    background: var(--dark-green);
    color: white;
    padding: 0.75rem 1.1rem;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 0.72rem;
    border-left: 3px solid var(--gold);
    transform: translateX(200%);
    transition: transform 0.4s ease;
    z-index: 200;
    box-shadow: 0 4px 16px rgba(0,0,0,0.4);
    border-radius: 0 2px 2px 0;
    max-width: 280px;
  }}

  .notif.show {{ transform: translateX(0); }}
</style>
</head>
<body>

<!-- HEADER -->
<header>
  <div class="header-logos">
    <img src="data:image/png;base64,{lasu}" alt="LASU Logo">
    <img src="data:image/png;base64,{asa}" alt="ASA Logo">
  </div>
  <div class="header-title">
    <div class="org">LASUASA — Independent Electoral Committee</div>
    <div class="portal-name">THEMIS <span>VANGUARD</span></div>
  </div>
  <div class="live-badge"><span class="live-dot"></span> Polls Open</div>
</header>

<!-- HERO -->
<div class="hero">
  <div class="hero-logos">
    <img src="data:image/png;base64,{lasu}" alt="Lagos State University">
    <div class="divider"></div>
    <img src="data:image/png;base64,{asa}" alt="Architecture Students Association">
  </div>
  <div class="hero-tag">Lagos State University · Architecture Students' Association</div>
  <h1>LASUASA <span>Elections</span><br>2026</h1>
  <p class="hero-sub">Cast your official ballot securely. Your vote is confidential and encrypted.</p>
  <div class="motto">"Integrity, Fairness, Transparency" &nbsp;·&nbsp; #LASUASADecides</div>
</div>

<main>

  <!-- VERIFICATION -->
  <div id="verify-section">
    <div class="section-label">Step 1 — Voter Verification</div>
    <div class="voter-card">
      <div class="voter-card-header">
        <img src="data:image/png;base64,{asa}" alt="ASA">
        <div class="card-titles">
          <h2>Verify Your Identity</h2>
          <p>LASUASA-IEC · Secure Voter Portal</p>
        </div>
      </div>
      <div class="form-grid">
        <div class="field field-full">
          <label>Full Name</label>
          <input type="text" id="voter-name" placeholder="As on department list">
        </div>
        <div class="field">
          <label>Matric Number</label>
          <input type="text" id="voter-matric" placeholder="e.g. 241811022">
        </div>
        <div class="field">
          <label>Level</label>
          <select id="voter-level">
            <option value="">Select level</option>
            <option>100</option>
            <option>200</option>
            <option>300</option>
            <option>400</option>
          </select>
        </div>
        <div class="field field-full">
          <label>Unique Access Code</label>
          <input type="text" id="voter-code" placeholder="e.g. AR617" oninput="this.value=this.value.toUpperCase()">
        </div>
        <button class="verify-btn" onclick="verifyVoter()">Verify &amp; Access Ballot →</button>
      </div>
    </div>
  </div>

  <!-- BALLOT -->
  <div id="ballot-section">
    <div class="section-label">Step 2 — Cast Your Ballot</div>

    <div class="race-block" style="animation-delay:0.1s">
      <div class="race-header">
        <span class="race-title">President of the United States</span>
        <span class="race-meta">Select One</span>
      </div>
      <div class="candidates" id="race-president">
        <label class="candidate-option" onclick="selectCandidate(this,'president')">
          <input type="radio" name="president">
          <div class="candidate-avatar" style="background:#1a5c35">JH</div>
          <div class="candidate-info"><div class="candidate-name">James Hartwell</div><div class="candidate-party">Democratic Party</div></div>
          <div class="check-icon">✓</div>
        </label>
        <label class="candidate-option" onclick="selectCandidate(this,'president')">
          <input type="radio" name="president">
          <div class="candidate-avatar" style="background:#8b1a1a">MR</div>
          <div class="candidate-info"><div class="candidate-name">Margaret Raines</div><div class="candidate-party">Republican Party</div></div>
          <div class="check-icon">✓</div>
        </label>
        <label class="candidate-option" onclick="selectCandidate(this,'president')">
          <input type="radio" name="president">
          <div class="candidate-avatar" style="background:#5d7a3e">TC</div>
          <div class="candidate-info"><div class="candidate-name">Thomas Chen</div><div class="candidate-party">Green Independent Party</div></div>
          <div class="check-icon">✓</div>
        </label>
      </div>
    </div>

    <div class="race-block" style="animation-delay:0.2s">
      <div class="race-header">
        <span class="race-title">U.S. Senate</span>
        <span class="race-meta">Select One</span>
      </div>
      <div class="candidates" id="race-senate">
        <label class="candidate-option" onclick="selectCandidate(this,'senate')">
          <input type="radio" name="senate">
          <div class="candidate-avatar" style="background:#1a5c35">DA</div>
          <div class="candidate-info"><div class="candidate-name">Diana Alvarez</div><div class="candidate-party">Democratic Party</div></div>
          <div class="check-icon">✓</div>
        </label>
        <label class="candidate-option" onclick="selectCandidate(this,'senate')">
          <input type="radio" name="senate">
          <div class="candidate-avatar" style="background:#8b1a1a">RB</div>
          <div class="candidate-info"><div class="candidate-name">Robert Blackwell</div><div class="candidate-party">Republican Party</div></div>
          <div class="check-icon">✓</div>
        </label>
      </div>
    </div>

    <div class="race-block" style="animation-delay:0.3s">
      <div class="race-header">
        <span class="race-title">Governor</span>
        <span class="race-meta">Select One</span>
      </div>
      <div class="candidates" id="race-governor">
        <label class="candidate-option" onclick="selectCandidate(this,'governor')">
          <input type="radio" name="governor">
          <div class="candidate-avatar" style="background:#7b5ea7">SW</div>
          <div class="candidate-info"><div class="candidate-name">Sarah Westbrook</div><div class="candidate-party">Progressive Alliance</div></div>
          <div class="check-icon">✓</div>
        </label>
        <label class="candidate-option" onclick="selectCandidate(this,'governor')">
          <input type="radio" name="governor">
          <div class="candidate-avatar" style="background:#8b1a1a">PM</div>
          <div class="candidate-info"><div class="candidate-name">Patrick McGrath</div><div class="candidate-party">Republican Party</div></div>
          <div class="check-icon">✓</div>
        </label>
        <label class="candidate-option" onclick="selectCandidate(this,'governor')">
          <input type="radio" name="governor">
          <div class="candidate-avatar" style="background:#1a5c35">LN</div>
          <div class="candidate-info"><div class="candidate-name">Linda Nakamura</div><div class="candidate-party">Democratic Party</div></div>
          <div class="check-icon">✓</div>
        </label>
      </div>
    </div>

    <div class="race-block" style="animation-delay:0.4s">
      <div class="race-header">
        <span class="race-title">Proposition 47 — Clean Energy Act</span>
        <span class="race-meta">Ballot Measure</span>
      </div>
      <div class="candidates" id="race-prop47">
        <label class="candidate-option" onclick="selectCandidate(this,'prop47')">
          <input type="radio" name="prop47">
          <div class="candidate-avatar" style="background:#2e7d32;font-size:0.8rem">YES</div>
          <div class="candidate-info"><div class="candidate-name">Yes — Support</div><div class="candidate-party">Requires 100% renewable energy by 2040</div></div>
          <div class="check-icon">✓</div>
        </label>
        <label class="candidate-option" onclick="selectCandidate(this,'prop47')">
          <input type="radio" name="prop47">
          <div class="candidate-avatar" style="background:#8b1a1a;font-size:0.8rem">NO</div>
          <div class="candidate-info"><div class="candidate-name">No — Oppose</div><div class="candidate-party">Reject the proposed mandate</div></div>
          <div class="check-icon">✓</div>
        </label>
      </div>
    </div>

    <div class="submit-zone" id="submit-zone">
      <h3>Review &amp; Submit Your Ballot</h3>
      <p>Once submitted, your vote cannot be changed. Please confirm all your selections.</p>
      <button class="submit-btn" onclick="submitBallot()">Submit Ballot ★</button>
    </div>
  </div>

  <!-- SUCCESS -->
  <div class="success-screen" id="success-screen">
    <div class="success-seal">★</div>
    <h2>Ballot Received</h2>
    <p>Thank you for participating in the LASUASA election.</p>
    <p>Your vote has been securely recorded.</p>
    <div class="confirmation-code" id="conf-code">CONF-XXXX-XXXX</div>
    <br><br>
    <a href="admin-dashboard.html" style="font-family:'IBM Plex Mono',monospace;font-size:0.72rem;color:var(--gold);letter-spacing:0.1em;text-decoration:none;border-bottom:1px solid var(--gold);padding-bottom:2px;">View Admin Results Dashboard →</a>
  </div>

</main>

<footer>
  <img src="data:image/png;base64,{lasu}" alt="LASU">
  <img src="data:image/png;base64,{asa}" alt="ASA">
  <br>
  LASUASA Independent Electoral Committee &nbsp;·&nbsp; #LASUASADecides<br>
  "Integrity, Fairness, Transparency" &nbsp;·&nbsp; All votes are encrypted and confidential.
</footer>

<div class="notif" id="notif"></div>

<script>
  const votes = {{}};
  const races = ['president','senate','governor','prop47'];

  function showNotif(msg) {{
    const n = document.getElementById('notif');
    n.textContent = msg;
    n.classList.add('show');
    setTimeout(() => n.classList.remove('show'), 2800);
  }}

  const REGISTERED_VOTERS = [
    {{ name: 'adekunle oluwawibe jacob', matric: '241811001', level: '100', code: 'AR754' }},
    {{ name: 'adekunle tomilola grace', matric: '241811002', level: '100', code: 'AR214' }},
    {{ name: 'adekunle-george oghenefejiro oluwadarasimi', matric: '241811003', level: '100', code: 'AR125' }},
    {{ name: 'adewusi samuel adedayo', matric: '241811004', level: '100', code: 'AR859' }},
    {{ name: 'akapo oluwadarasimi david', matric: '241811005', level: '100', code: 'AR381' }},
    {{ name: 'ashimi shakirat funmilayo', matric: '241811006', level: '100', code: 'AR350' }},
    {{ name: 'kehinde olamide oluwatobi', matric: '241811007', level: '100', code: 'AR328' }},
    {{ name: 'ogbonna chidiebube favour', matric: '241811008', level: '100', code: 'AR242' }},
    {{ name: 'ogunnoiki david oluwademilade', matric: '241811009', level: '100', code: 'AR854' }},
    {{ name: 'oladeji olatunbosun david', matric: '241811010', level: '100', code: 'AR204' }},
    {{ name: 'oladele segun temidun', matric: '241811011', level: '100', code: 'AR792' }},
    {{ name: 'olaribigbe emmanuel oluwaseyi', matric: '241811012', level: '100', code: 'AR858' }},
    {{ name: 'olayanju muhammed oluwapelumi', matric: '241811013', level: '100', code: 'AR658' }},
    {{ name: 'owolana peace toluwalase', matric: '241811014', level: '100', code: 'AR189' }},
    {{ name: 'rotimi toluwanimi susanna', matric: '241811015', level: '100', code: 'AR704' }},
    {{ name: 'tunde-gafar babaloye malik', matric: '241811016', level: '100', code: 'AR532' }},
    {{ name: 'aluko dabiolu samuel', matric: '241811017', level: '100', code: 'AR132' }},
    {{ name: 'awoleye emmanuel oluwatimisola', matric: '241811018', level: '100', code: 'AR130' }},
    {{ name: 'jimoh habibah modupeola', matric: '241811019', level: '100', code: 'AR195' }},
    {{ name: 'olaniran oluwatobi emmanuel', matric: '241811020', level: '100', code: 'AR323' }},
    {{ name: 'shadare emmanuel oluwatofunmi', matric: '241811021', level: '100', code: 'AR338' }},
    {{ name: 'shambakiu ramadan olamilekan', matric: '241811022', level: '100', code: 'AR617' }},
    {{ name: 'akinwande dideiyanuoluwa amarachukwu', matric: '241811023', level: '100', code: 'AR716' }},
    {{ name: 'akinyetun demilade emmanuel', matric: '241811024', level: '100', code: 'AR127' }},
    {{ name: 'bello tomiwa ayomide', matric: '241811025', level: '100', code: 'AR674' }},
    {{ name: 'kayode mathias olasunkanmi', matric: '241811026', level: '100', code: 'AR303' }},
    {{ name: 'lawrence oluwanifemi oreoluwa', matric: '241811027', level: '100', code: 'AR833' }},
    {{ name: 'odusola ademola isaiah', matric: '241811028', level: '100', code: 'AR765' }},
    {{ name: 'olaiya ayobola ezekiel', matric: '241811029', level: '100', code: 'AR818' }},
    {{ name: 'olasupo emmanuel ayomide', matric: '241811030', level: '100', code: 'AR529' }},
    {{ name: 'olawale oluwatobi abiodun', matric: '241811031', level: '100', code: 'AR325' }},
    {{ name: 'osayiwu daniel efosa', matric: '241811032', level: '100', code: 'AR559' }},
    {{ name: 'subair abdulrahmon pelumi', matric: '241811033', level: '100', code: 'AR703' }},
    {{ name: 'wusu olusegun samuel', matric: '241811034', level: '100', code: 'AR384' }},
    {{ name: 'yusuf abdulqudus opeyemi', matric: '241811035', level: '100', code: 'AR928' }},
    {{ name: 'olagunju faith fiyinfoluwa', matric: '241811036', level: '100', code: 'AR990' }},
    {{ name: 'afolabi ezekiel boluwatife', matric: '241811037', level: '100', code: 'AR106' }},
    {{ name: 'idowu abdullah olamilekan', matric: '241811038', level: '100', code: 'AR877' }},
    {{ name: 'nworu doreen chiwendu', matric: '241811039', level: '100', code: 'AR925' }},
    {{ name: 'okolie israel chukwudiebube', matric: '241811040', level: '100', code: 'AR263' }},
    {{ name: 'salaudeen rukayat oluwafunmilayo', matric: '241811041', level: '100', code: 'AR814' }},
    {{ name: 'warri esther tonye', matric: '241811042', level: '100', code: 'AR448' }},
    {{ name: 'buraimoh habeeb abiola', matric: '241811043', level: '100', code: 'AR259' }},
    {{ name: 'odutola matthew ayorinde', matric: '241811044', level: '100', code: 'AR320' }},
    {{ name: 'solomon eloroghene prayer', matric: '241811045', level: '100', code: 'AR881' }},
    {{ name: 'adesanya hamjad ademola', matric: '241811047', level: '100', code: 'AR444' }},
    {{ name: 'rufai fawwaz niniola', matric: '231811047', level: '100', code: 'AR194' }},
    {{ name: 'shambakiu ramadan olamilekan', matric: '241811022', level: '300', code: 'AR617' }},
  ];

  let currentVoterName = '', currentVoterMatric = '', currentVoterLevel = '';
  const selectedCandidates = {{}};

  function verifyVoter() {{
    const name   = document.getElementById('voter-name').value.trim().toLowerCase();
    const matric = document.getElementById('voter-matric').value.trim();
    const level  = document.getElementById('voter-level').value;
    const code   = document.getElementById('voter-code').value.trim().toUpperCase();

    if (!name || !matric || !level || !code) {{
      showNotif('⚠ Please complete all fields.');
      return;
    }}

    const match = REGISTERED_VOTERS.find(v =>
      v.name === name && v.matric === matric && v.level === level && v.code === code
    );

    if (!match) {{
      showNotif('✗ Verification failed. Details not found.');
      return;
    }}

    currentVoterName   = document.getElementById('voter-name').value.trim();
    currentVoterMatric = matric;
    currentVoterLevel  = level;

    document.getElementById('verify-section').style.display = 'none';
    const ballot = document.getElementById('ballot-section');
    ballot.classList.add('visible');
    showNotif('✓ Identity verified. Ballot loaded.');
    ballot.scrollIntoView({{ behavior: 'smooth', block: 'start' }});
  }}

  function selectCandidate(el, race) {{
    document.getElementById('race-' + race).querySelectorAll('.candidate-option').forEach(o => o.classList.remove('selected'));
    el.classList.add('selected');
    votes[race] = true;
    selectedCandidates[race] = el.querySelector('.candidate-name').textContent;
    checkAllVoted();
  }}

  function checkAllVoted() {{
    const done = races.every(r => votes[r]);
    const zone = document.getElementById('submit-zone');
    if (done && !zone.classList.contains('visible')) {{
      zone.classList.add('visible');
      zone.scrollIntoView({{ behavior: 'smooth', block: 'nearest' }});
      showNotif('✓ All races selected. Ready to submit.');
    }}
  }}

  function submitBallot() {{
    if (!races.every(r => votes[r])) {{ showNotif('⚠ Please vote in all races first.'); return; }}
    const code = 'CONF-' + Math.random().toString(36).substring(2,6).toUpperCase() + '-' + Math.random().toString(36).substring(2,6).toUpperCase();
    const record = {{ voter: currentVoterName, matric: currentVoterMatric, level: currentVoterLevel, timestamp: new Date().toISOString(), confirmationCode: code, votes: {{...selectedCandidates}} }};
    const existing = JSON.parse(localStorage.getItem('electvote_records') || '[]');
    existing.push(record);
    localStorage.setItem('electvote_records', JSON.stringify(existing));
    document.getElementById('ballot-section').style.display = 'none';
    document.getElementById('conf-code').textContent = code;
    document.getElementById('success-screen').classList.add('visible');
    document.getElementById('success-screen').scrollIntoView({{ behavior: 'smooth' }});
  }}
</script>
</body>
</html>"""

with open('/mnt/user-data/outputs/election-voting.html', 'w') as f:
    f.write(html)

print("Done. File size:", len(html), "chars")
PYEOF
