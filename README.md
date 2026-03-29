# shortsai
<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ShortsAI — Viral Shorts Platform</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=Syne:wght@700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #13131a;
    --surface2: #1c1c26;
    --border: rgba(255,255,255,0.08);
    --accent: #7c5cfc;
    --accent2: #fc5c7d;
    --green: #22c55e;
    --amber: #f59e0b;
    --text: #f0f0f8;
    --muted: #6b6b80;
    --radius: 14px;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: 'Space Grotesk', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; }

  /* SIDEBAR */
  .sidebar {
    position: fixed; left: 0; top: 0; bottom: 0; width: 220px;
    background: var(--surface); border-right: 1px solid var(--border);
    display: flex; flex-direction: column; padding: 24px 16px; z-index: 100;
  }
  .logo { font-family: 'Syne', sans-serif; font-size: 22px; font-weight: 800;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    margin-bottom: 32px; padding-left: 4px;
  }
  .nav-item {
    display: flex; align-items: center; gap: 10px; padding: 10px 12px;
    border-radius: 10px; cursor: pointer; font-size: 14px; color: var(--muted);
    transition: all 0.2s; margin-bottom: 4px; border: none; background: none; width: 100%; text-align: left;
  }
  .nav-item:hover { background: var(--surface2); color: var(--text); }
  .nav-item.active { background: rgba(124,92,252,0.15); color: var(--accent); }
  .nav-icon { font-size: 16px; width: 20px; text-align: center; }
  .sidebar-footer { margin-top: auto; }
  .user-pill {
    display: flex; align-items: center; gap: 10px; padding: 10px 12px;
    background: var(--surface2); border-radius: 10px; font-size: 13px;
  }
  .avatar { width: 32px; height: 32px; border-radius: 50%; background: linear-gradient(135deg, var(--accent), var(--accent2)); display: flex; align-items: center; justify-content: center; font-size: 13px; font-weight: 700; flex-shrink: 0; }

  /* MAIN */
  .main { margin-left: 220px; min-height: 100vh; }
  .page { display: none; padding: 32px; }
  .page.active { display: block; }
  .page-title { font-family: 'Syne', sans-serif; font-size: 26px; font-weight: 800; margin-bottom: 6px; }
  .page-sub { font-size: 14px; color: var(--muted); margin-bottom: 28px; }

  /* CARDS */
  .card {
    background: var(--surface); border: 1px solid var(--border);
    border-radius: var(--radius); padding: 20px;
  }
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-bottom: 20px; }
  .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 16px; margin-bottom: 20px; }
  .grid-4 { display: grid; grid-template-columns: repeat(4,1fr); gap: 14px; margin-bottom: 24px; }

  /* METRICS */
  .metric-card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 18px; }
  .metric-label { font-size: 12px; color: var(--muted); margin-bottom: 6px; }
  .metric-val { font-size: 28px; font-weight: 700; line-height: 1; }
  .metric-change { font-size: 12px; color: var(--green); margin-top: 4px; }

  /* BADGE */
  .badge { font-size: 11px; padding: 3px 10px; border-radius: 20px; font-weight: 600; display: inline-flex; align-items: center; gap: 4px; }
  .badge-purple { background: rgba(124,92,252,0.15); color: #a78bfa; }
  .badge-green { background: rgba(34,197,94,0.12); color: #4ade80; }
  .badge-amber { background: rgba(245,158,11,0.12); color: #fbbf24; }
  .badge-red { background: rgba(252,92,125,0.12); color: #fc5c7d; }
  .badge-blue { background: rgba(59,130,246,0.12); color: #60a5fa; }

  /* BUTTONS */
  .btn { padding: 10px 20px; border-radius: 10px; font-size: 13px; font-weight: 600; cursor: pointer; border: none; transition: all 0.2s; font-family: 'Space Grotesk', sans-serif; }
  .btn-primary { background: var(--accent); color: white; }
  .btn-primary:hover { background: #6b4de8; transform: translateY(-1px); }
  .btn-ghost { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
  .btn-ghost:hover { border-color: var(--accent); color: var(--accent); }
  .btn-danger { background: rgba(252,92,125,0.15); color: var(--accent2); border: 1px solid rgba(252,92,125,0.2); }

  /* PLATFORM CARDS */
  .platform-card {
    background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px;
    display: flex; flex-direction: column; gap: 14px; transition: all 0.2s;
  }
  .platform-card.connected { border-color: rgba(34,197,94,0.3); }
  .platform-top { display: flex; align-items: center; gap: 12px; }
  .platform-icon { width: 44px; height: 44px; border-radius: 12px; display: flex; align-items: center; justify-content: center; font-size: 22px; flex-shrink: 0; }
  .yt-icon { background: rgba(255,0,0,0.1); }
  .ig-icon { background: rgba(225,48,108,0.1); }
  .fb-icon { background: rgba(24,119,242,0.1); }
  .connect-btn {
    width: 100%; padding: 9px; border-radius: 8px; font-size: 13px; font-weight: 600;
    cursor: pointer; transition: all 0.2s; font-family: 'Space Grotesk', sans-serif; border: none;
  }
  .connect-btn.disconnected { background: var(--surface2); color: var(--text); border: 1px solid var(--border); }
  .connect-btn.connected { background: rgba(34,197,94,0.1); color: var(--green); border: 1px solid rgba(34,197,94,0.2); }
  .status-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
  .dot-green { background: var(--green); box-shadow: 0 0 6px var(--green); }
  .dot-gray { background: var(--muted); }

  /* SHORTS TABLE */
  .table { width: 100%; border-collapse: collapse; font-size: 13px; }
  .table th { text-align: left; padding: 10px 14px; color: var(--muted); font-weight: 500; font-size: 12px; border-bottom: 1px solid var(--border); }
  .table td { padding: 13px 14px; border-bottom: 1px solid var(--border); vertical-align: middle; }
  .table tr:last-child td { border-bottom: none; }
  .table tr:hover td { background: rgba(255,255,255,0.02); }
  .thumb { width: 52px; height: 34px; border-radius: 6px; object-fit: cover; background: var(--surface2); display: flex; align-items: center; justify-content: center; font-size: 16px; flex-shrink: 0; }

  /* AI CHAT */
  .chat-wrap { display: flex; flex-direction: column; height: calc(100vh - 120px); }
  .chat-messages { flex: 1; overflow-y: auto; padding: 20px; display: flex; flex-direction: column; gap: 14px; }
  .chat-messages::-webkit-scrollbar { width: 4px; }
  .chat-messages::-webkit-scrollbar-track { background: transparent; }
  .chat-messages::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }
  .msg { display: flex; gap: 10px; align-items: flex-start; max-width: 80%; }
  .msg.user { align-self: flex-end; flex-direction: row-reverse; }
  .msg-avatar { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 14px; flex-shrink: 0; }
  .msg-avatar.ai { background: linear-gradient(135deg, var(--accent), var(--accent2)); }
  .msg-avatar.user { background: var(--surface2); border: 1px solid var(--border); }
  .msg-bubble { padding: 12px 16px; border-radius: 14px; font-size: 14px; line-height: 1.65; }
  .msg.ai .msg-bubble { background: var(--surface); border: 1px solid var(--border); border-top-left-radius: 4px; }
  .msg.user .msg-bubble { background: var(--accent); color: white; border-top-right-radius: 4px; }
  .chat-input-wrap { padding: 16px 20px; border-top: 1px solid var(--border); display: flex; gap: 10px; background: var(--surface); }
  .chat-input {
    flex: 1; background: var(--surface2); border: 1px solid var(--border); border-radius: 12px;
    padding: 12px 16px; color: var(--text); font-size: 14px; font-family: 'Space Grotesk', sans-serif;
    outline: none; resize: none; height: 48px; transition: border-color 0.2s;
  }
  .chat-input:focus { border-color: var(--accent); }
  .send-btn { width: 48px; height: 48px; border-radius: 12px; background: var(--accent); border: none; cursor: pointer; font-size: 18px; flex-shrink: 0; transition: all 0.2s; display: flex; align-items: center; justify-content: center; }
  .send-btn:hover { background: #6b4de8; transform: translateY(-1px); }
  .send-btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }
  .typing { display: flex; gap: 4px; align-items: center; padding: 14px 16px; }
  .typing span { width: 7px; height: 7px; border-radius: 50%; background: var(--muted); animation: bounce 1.2s infinite; }
  .typing span:nth-child(2) { animation-delay: 0.2s; }
  .typing span:nth-child(3) { animation-delay: 0.4s; }
  @keyframes bounce { 0%,60%,100%{transform:translateY(0)} 30%{transform:translateY(-6px)} }

  /* API KEY INPUT */
  .api-setup { background: rgba(124,92,252,0.05); border: 1px solid rgba(124,92,252,0.2); border-radius: var(--radius); padding: 18px; margin-bottom: 16px; }
  .api-input { background: var(--surface2); border: 1px solid var(--border); border-radius: 10px; padding: 10px 14px; color: var(--text); font-size: 13px; width: 100%; font-family: 'Space Grotesk', sans-serif; outline: none; margin: 8px 0; }
  .api-input:focus { border-color: var(--accent); }

  /* TOGGLE */
  .toggle-wrap { display: flex; align-items: center; gap: 10px; }
  .toggle { position: relative; width: 44px; height: 24px; }
  .toggle input { opacity: 0; width: 0; height: 0; }
  .toggle-track { position: absolute; inset: 0; background: var(--surface2); border-radius: 12px; cursor: pointer; transition: 0.2s; border: 1px solid var(--border); }
  .toggle input:checked + .toggle-track { background: var(--accent); border-color: var(--accent); }
  .toggle-thumb { position: absolute; top: 3px; left: 3px; width: 16px; height: 16px; background: white; border-radius: 50%; transition: 0.2s; }
  .toggle input:checked ~ .toggle-thumb { left: 23px; }

  /* SCHEDULE ROW */
  .schedule-row { display: flex; align-items: center; justify-content: space-between; padding: 14px 0; border-bottom: 1px solid var(--border); }
  .schedule-row:last-child { border-bottom: none; }

  /* PROGRESS */
  .progress-bar { height: 6px; background: var(--surface2); border-radius: 3px; overflow: hidden; margin-top: 6px; }
  .progress-fill { height: 100%; border-radius: 3px; background: linear-gradient(90deg, var(--accent), var(--accent2)); transition: width 0.5s; }

  /* TRENDING TAGS */
  .tag { display: inline-block; padding: 4px 12px; border-radius: 20px; font-size: 12px; background: var(--surface2); border: 1px solid var(--border); color: var(--muted); margin: 3px; cursor: pointer; transition: all 0.2s; }
  .tag:hover, .tag.active { background: rgba(124,92,252,0.15); border-color: var(--accent); color: var(--accent); }

  /* MODAL */
  .modal-bg { position: fixed; inset: 0; background: rgba(0,0,0,0.7); z-index: 999; display: none; align-items: center; justify-content: center; }
  .modal-bg.open { display: flex; }
  .modal { background: var(--surface); border: 1px solid var(--border); border-radius: 20px; padding: 28px; width: 480px; max-width: 95vw; }

  /* NOTIFICATION */
  .notif { position: fixed; bottom: 24px; right: 24px; background: var(--surface); border: 1px solid var(--green); border-radius: 12px; padding: 14px 18px; font-size: 13px; color: var(--green); z-index: 9999; transform: translateY(100px); opacity: 0; transition: all 0.3s; }
  .notif.show { transform: translateY(0); opacity: 1; }

  /* ANIM */
  @keyframes fadeIn { from{opacity:0;transform:translateY(10px)} to{opacity:1;transform:translateY(0)} }
  .fade-in { animation: fadeIn 0.3s ease; }

  /* SELECT */
  select { background: var(--surface2); border: 1px solid var(--border); border-radius: 8px; padding: 8px 12px; color: var(--text); font-size: 13px; font-family: 'Space Grotesk', sans-serif; outline: none; cursor: pointer; }
  select:focus { border-color: var(--accent); }

  .sep { height: 1px; background: var(--border); margin: 20px 0; }
  .flex-between { display: flex; align-items: center; justify-content: space-between; }
  .flex-gap { display: flex; align-items: center; gap: 10px; }
</style>
</head>
<body>

<!-- SIDEBAR -->
<div class="sidebar">
  <div class="logo">ShortsAI</div>
  <nav>
    <button class="nav-item active" onclick="showPage('dashboard', this)">
      <span class="nav-icon">📊</span> Dashboard
    </button>
    <button class="nav-item" onclick="showPage('platforms', this)">
      <span class="nav-icon">🔗</span> Platforms
    </button>
    <button class="nav-item" onclick="showPage('shorts', this)">
      <span class="nav-icon">🎬</span> Mere Shorts
    </button>
    <button class="nav-item" onclick="showPage('schedule', this)">
      <span class="nav-icon">📅</span> Schedule
    </button>
    <button class="nav-item" onclick="showPage('aichat', this)">
      <span class="nav-icon">🤖</span> AI Assistant
    </button>
    <button class="nav-item" onclick="showPage('settings', this)">
      <span class="nav-icon">⚙️</span> Settings
    </button>
  </nav>
  <div class="sidebar-footer">
    <div class="user-pill">
      <div class="avatar">A</div>
      <div>
        <div style="font-size:13px;font-weight:600;">Aapka Account</div>
        <div style="font-size:11px;color:var(--muted);" id="plan-badge">Free Plan</div>
      </div>
    </div>
  </div>
</div>

<!-- MAIN -->
<div class="main">

  <!-- DASHBOARD -->
  <div class="page active" id="page-dashboard">
    <div class="page-title">Dashboard 👋</div>
    <div class="page-sub">Aapka viral shorts automation — sab kuch ek jagah</div>

    <div class="grid-4">
      <div class="metric-card">
        <div class="metric-label">Total Shorts</div>
        <div class="metric-val" style="color:var(--accent);">24</div>
        <div class="metric-change">↑ 3 is hafte</div>
      </div>
      <div class="metric-card">
        <div class="metric-label">Total Views</div>
        <div class="metric-val" style="color:var(--accent2);">1.2M</div>
        <div class="metric-change">↑ 18% is mahine</div>
      </div>
      <div class="metric-card">
        <div class="metric-label">Platforms Connected</div>
        <div class="metric-val" style="color:var(--green);" id="dash-platforms">0</div>
        <div class="metric-change">YouTube, Instagram, FB</div>
      </div>
      <div class="metric-card">
        <div class="metric-label">Next Upload</div>
        <div class="metric-val" style="font-size:20px;color:var(--amber);" id="next-upload">--:--:--</div>
        <div class="metric-change">Auto scheduled</div>
      </div>
    </div>

    <div class="grid-2">
      <div class="card">
        <div class="flex-between" style="margin-bottom:14px;">
          <span style="font-size:15px;font-weight:600;">Aaj ka Short</span>
          <span class="badge badge-green">● Ready</span>
        </div>
        <div style="background:var(--surface2);border-radius:10px;padding:14px;margin-bottom:12px;">
          <div style="font-size:13px;font-weight:600;margin-bottom:4px;" id="today-title">CarryMinati — Funniest Moments #24</div>
          <div style="font-size:12px;color:var(--muted);margin-bottom:8px;">Duration: 58s · 9MB · 1080p</div>
          <div style="font-size:12px;color:var(--muted);margin-bottom:6px;">SEO Score</div>
          <div class="progress-bar"><div class="progress-fill" style="width:94%;"></div></div>
          <div style="font-size:11px;color:var(--accent);margin-top:4px;">94/100 — Excellent</div>
        </div>
        <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:12px;">
          <span class="tag active">#CarryMinati</span>
          <span class="tag active">#Shorts</span>
          <span class="tag active">#Viral</span>
          <span class="tag active">#Comedy</span>
          <span class="tag active">#India</span>
        </div>
        <button class="btn btn-primary" style="width:100%;" onclick="showNotif('✅ Short upload ho gaya YouTube aur Instagram pe!')">🚀 Abhi Upload Karo</button>
      </div>

      <div class="card">
        <div style="font-size:15px;font-weight:600;margin-bottom:14px;">Trending Viral Topics</div>
        <div style="display:flex;flex-direction:column;gap:10px;">
          <div style="display:flex;align-items:center;gap:10px;padding:10px;background:var(--surface2);border-radius:10px;">
            <span style="font-size:18px;">🔥</span>
            <div style="flex:1;">
              <div style="font-size:13px;font-weight:600;">CarryMinati Roast 2025</div>
              <div style="font-size:11px;color:var(--muted);">12M views · Trending #1</div>
            </div>
            <button class="btn btn-ghost" style="padding:6px 12px;font-size:11px;" onclick="showNotif('AI short bana raha hai...')">Clip banao</button>
          </div>
          <div style="display:flex;align-items:center;gap:10px;padding:10px;background:var(--surface2);border-radius:10px;">
            <span style="font-size:18px;">⚡</span>
            <div style="flex:1;">
              <div style="font-size:13px;font-weight:600;">MrBeast India Challenge</div>
              <div style="font-size:11px;color:var(--muted);">8.4M views · Trending #3</div>
            </div>
            <button class="btn btn-ghost" style="padding:6px 12px;font-size:11px;" onclick="showNotif('AI short bana raha hai...')">Clip banao</button>
          </div>
          <div style="display:flex;align-items:center;gap:10px;padding:10px;background:var(--surface2);border-radius:10px;">
            <span style="font-size:18px;">📱</span>
            <div style="flex:1;">
              <div style="font-size:13px;font-weight:600;">Technical Guruji Review</div>
              <div style="font-size:11px;color:var(--muted);">5.1M views · Trending #7</div>
            </div>
            <button class="btn btn-ghost" style="padding:6px 12px;font-size:11px;" onclick="showNotif('AI short bana raha hai...')">Clip banao</button>
          </div>
        </div>
      </div>
    </div>

    <div class="card">
      <div class="flex-between" style="margin-bottom:14px;">
        <span style="font-size:15px;font-weight:600;">Recent Uploads</span>
        <button class="btn btn-ghost" style="padding:6px 14px;" onclick="showPage('shorts', document.querySelector('.nav-item:nth-child(3)'))">Sab dekho</button>
      </div>
      <table class="table">
        <thead>
          <tr>
            <th>Video</th><th>Platform</th><th>Views</th><th>SEO</th><th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td><div class="flex-gap"><div class="thumb">🎭</div><span>CarryMinati Best Moments</span></div></td>
            <td><span class="badge badge-red">▶ YT</span> <span class="badge badge-purple">📷 IG</span></td>
            <td style="color:var(--accent);font-weight:600;">24.5K</td>
            <td><span style="color:var(--green);font-weight:600;">96</span></td>
            <td><span class="badge badge-green">● Live</span></td>
          </tr>
          <tr>
            <td><div class="flex-gap"><div class="thumb">💰</div><span>MrBeast $1M Challenge</span></div></td>
            <td><span class="badge badge-red">▶ YT</span></td>
            <td style="color:var(--accent);font-weight:600;">18.2K</td>
            <td><span style="color:var(--green);font-weight:600;">91</span></td>
            <td><span class="badge badge-green">● Live</span></td>
          </tr>
          <tr>
            <td><div class="flex-gap"><div class="thumb">📱</div><span>Tech Guruji iPhone Review</span></div></td>
            <td><span class="badge badge-red">▶ YT</span> <span class="badge badge-purple">📷 IG</span></td>
            <td style="color:var(--accent);font-weight:600;">9.8K</td>
            <td><span style="color:var(--amber);font-weight:600;">88</span></td>
            <td><span class="badge badge-green">● Live</span></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <!-- PLATFORMS -->
  <div class="page" id="page-platforms">
    <div class="page-title">Platforms Connect Karo 🔗</div>
    <div class="page-sub">Ek baar connect karo — AI khud sab upload karega</div>

    <div class="grid-3">
      <div class="platform-card" id="yt-card">
        <div class="platform-top">
          <div class="platform-icon yt-icon">▶️</div>
          <div>
            <div style="font-size:15px;font-weight:600;">YouTube</div>
            <div style="font-size:12px;color:var(--muted);" id="yt-status">Connect nahi hai</div>
          </div>
          <div class="status-dot dot-gray" id="yt-dot" style="margin-left:auto;"></div>
        </div>
        <div style="font-size:12px;color:var(--muted);line-height:1.6;">
          Viral shorts, title, description, tags, SEO — sab AI bharta hai. Credit link bhi dalta hai.
        </div>
        <button class="connect-btn disconnected" id="yt-btn" onclick="connectPlatform('yt')">
          Connect YouTube
        </button>
      </div>

      <div class="platform-card" id="ig-card">
        <div class="platform-top">
          <div class="platform-icon ig-icon">📷</div>
          <div>
            <div style="font-size:15px;font-weight:600;">Instagram</div>
            <div style="font-size:12px;color:var(--muted);" id="ig-status">Connect nahi hai</div>
          </div>
          <div class="status-dot dot-gray" id="ig-dot" style="margin-left:auto;"></div>
        </div>
        <div style="font-size:12px;color:var(--muted);line-height:1.6;">
          Reels automatically upload hongi. Trending hashtags AI generate karega for maximum reach.
        </div>
        <button class="connect-btn disconnected" id="ig-btn" onclick="connectPlatform('ig')">
          Connect Instagram
        </button>
      </div>

      <div class="platform-card" id="fb-card">
        <div class="platform-top">
          <div class="platform-icon fb-icon">👍</div>
          <div>
            <div style="font-size:15px;font-weight:600;">Facebook</div>
            <div style="font-size:12px;color:var(--muted);" id="fb-status">Connect nahi hai</div>
          </div>
          <div class="status-dot dot-gray" id="fb-dot" style="margin-left:auto;"></div>
        </div>
        <div style="font-size:12px;color:var(--muted);line-height:1.6;">
          Facebook Reels aur Page pe shorts auto-post. Viral reach ke liye optimal posting time.
        </div>
        <button class="connect-btn disconnected" id="fb-btn" onclick="connectPlatform('fb')">
          Connect Facebook
        </button>
      </div>
    </div>

    <div class="card">
      <div style="font-size:15px;font-weight:600;margin-bottom:14px;">Auto-Upload Settings</div>
      <div class="schedule-row">
        <div>
          <div style="font-size:14px;font-weight:500;">Auto Title Generate karo</div>
          <div style="font-size:12px;color:var(--muted);">AI viral title aur description khud likhega</div>
        </div>
        <label class="toggle">
          <input type="checkbox" checked>
          <span class="toggle-track"></span>
          <span class="toggle-thumb"></span>
        </label>
      </div>
      <div class="schedule-row">
        <div>
          <div style="font-size:14px;font-weight:500;">Trending Tags Auto-Add</div>
          <div style="font-size:12px;color:var(--muted);">Real-time trending hashtags add karo</div>
        </div>
        <label class="toggle">
          <input type="checkbox" checked>
          <span class="toggle-track"></span>
          <span class="toggle-thumb"></span>
        </label>
      </div>
      <div class="schedule-row">
        <div>
          <div style="font-size:14px;font-weight:500;">Original Video Credit Link</div>
          <div style="font-size:12px;color:var(--muted);">Description mein original video ka link daalo</div>
        </div>
        <label class="toggle">
          <input type="checkbox" checked>
          <span class="toggle-track"></span>
          <span class="toggle-thumb"></span>
        </label>
      </div>
      <div class="schedule-row">
        <div>
          <div style="font-size:14px;font-weight:500;">SEO Optimization (100%)</div>
          <div style="font-size:12px;color:var(--muted);">Title, description, tags — sab SEO optimized</div>
        </div>
        <label class="toggle">
          <input type="checkbox" checked>
          <span class="toggle-track"></span>
          <span class="toggle-thumb"></span>
        </label>
      </div>
    </div>
  </div>

  <!-- SHORTS -->
  <div class="page" id="page-shorts">
    <div class="page-title">Mere Shorts 🎬</div>
    <div class="page-sub">Sare automatically bane aur upload hue shorts</div>

    <div style="display:flex;gap:10px;margin-bottom:20px;flex-wrap:wrap;">
      <select onchange="">
        <option>Sab Platforms</option>
        <option>YouTube</option>
        <option>Instagram</option>
        <option>Facebook</option>
      </select>
      <select>
        <option>Is Hafte</option>
        <option>Is Mahine</option>
        <option>Sab Time</option>
      </select>
      <button class="btn btn-primary" onclick="document.getElementById('new-modal').classList.add('open')">+ Naya Short Banao</button>
    </div>

    <div class="card">
      <table class="table">
        <thead>
          <tr><th>Short</th><th>YouTuber</th><th>Platform</th><th>Views</th><th>Likes</th><th>SEO</th><th>Status</th><th></th></tr>
        </thead>
        <tbody id="shorts-tbody">
          <tr>
            <td><div class="flex-gap"><div class="thumb">🎭</div><div><div style="font-size:13px;font-weight:500;">CarryMinati Best Roast</div><div style="font-size:11px;color:var(--muted);">Aaj · 58s</div></div></div></td>
            <td>CarryMinati</td>
            <td><span class="badge badge-red">▶</span> <span class="badge badge-purple">📷</span></td>
            <td style="font-weight:600;color:var(--accent);">24.5K</td>
            <td>1.2K</td>
            <td><span style="color:var(--green);font-weight:700;">96</span></td>
            <td><span class="badge badge-green">● Live</span></td>
            <td><button class="btn btn-ghost" style="padding:5px 10px;font-size:11px;">Dekho</button></td>
          </tr>
          <tr>
            <td><div class="flex-gap"><div class="thumb">💰</div><div><div style="font-size:13px;font-weight:500;">MrBeast $500K Win</div><div style="font-size:11px;color:var(--muted);">Kal · 60s</div></div></div></td>
            <td>MrBeast</td>
            <td><span class="badge badge-red">▶</span></td>
            <td style="font-weight:600;color:var(--accent);">18.2K</td>
            <td>890</td>
            <td><span style="color:var(--green);font-weight:700;">91</span></td>
            <td><span class="badge badge-green">● Live</span></td>
            <td><button class="btn btn-ghost" style="padding:5px 10px;font-size:11px;">Dekho</button></td>
          </tr>
          <tr>
            <td><div class="flex-gap"><div class="thumb">📱</div><div><div style="font-size:13px;font-weight:500;">Tech Guruji Review</div><div style="font-size:11px;color:var(--muted);">2 din · 45s</div></div></div></td>
            <td>Technical Guruji</td>
            <td><span class="badge badge-red">▶</span> <span class="badge badge-purple">📷</span></td>
            <td style="font-weight:600;color:var(--accent);">9.8K</td>
            <td>445</td>
            <td><span style="color:var(--amber);font-weight:700;">88</span></td>
            <td><span class="badge badge-green">● Live</span></td>
            <td><button class="btn btn-ghost" style="padding:5px 10px;font-size:11px;">Dekho</button></td>
          </tr>
          <tr>
            <td><div class="flex-gap"><div class="thumb">⏳</div><div><div style="font-size:13px;font-weight:500;">Kal ka Short</div><div style="font-size:11px;color:var(--muted);">Scheduled · 60s</div></div></div></td>
            <td>CarryMinati</td>
            <td><span class="badge badge-red">▶</span> <span class="badge badge-purple">📷</span> <span class="badge badge-blue">👍</span></td>
            <td style="color:var(--muted);">—</td>
            <td>—</td>
            <td><span style="color:var(--green);font-weight:700;">94</span></td>
            <td><span class="badge badge-amber">⏰ Scheduled</span></td>
            <td><button class="btn btn-ghost" style="padding:5px 10px;font-size:11px;">Edit</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <!-- SCHEDULE -->
  <div class="page" id="page-schedule">
    <div class="page-title">Auto Schedule 📅</div>
    <div class="page-sub">Daily automatic upload — set karo aur bhool jao</div>

    <div class="grid-2">
      <div class="card">
        <div style="font-size:15px;font-weight:600;margin-bottom:14px;">Upload Time Set Karo</div>
        <div class="schedule-row">
          <div>
            <div style="font-size:14px;font-weight:500;">▶ YouTube Shorts</div>
            <div style="font-size:12px;color:var(--muted);">Best time: Subah 9-11 AM</div>
          </div>
          <select><option>9:00 AM</option><option>10:00 AM</option><option>11:00 AM</option><option>12:00 PM</option><option>6:00 PM</option><option>8:00 PM</option></select>
        </div>
        <div class="schedule-row">
          <div>
            <div style="font-size:14px;font-weight:500;">📷 Instagram Reels</div>
            <div style="font-size:12px;color:var(--muted);">Best time: Shaam 6-8 PM</div>
          </div>
          <select><option>6:00 PM</option><option>7:00 PM</option><option>8:00 PM</option><option>9:00 PM</option></select>
        </div>
        <div class="schedule-row">
          <div>
            <div style="font-size:14px;font-weight:500;">👍 Facebook Reels</div>
            <div style="font-size:12px;color:var(--muted);">Best time: Raat 8-10 PM</div>
          </div>
          <select><option>8:00 PM</option><option>9:00 PM</option><option>10:00 PM</option></select>
        </div>
        <button class="btn btn-primary" style="width:100%;margin-top:16px;" onclick="showNotif('Schedule save ho gaya! AI roz apne aap upload karega.')">Schedule Save Karo</button>
      </div>

      <div class="card">
        <div style="font-size:15px;font-weight:600;margin-bottom:14px;">Channels Select Karo</div>
        <div style="font-size:13px;color:var(--muted);margin-bottom:12px;">Jinke videos se shorts banana hai:</div>
        <div style="display:flex;flex-wrap:wrap;gap:6px;">
          <span class="tag active">CarryMinati</span>
          <span class="tag active">MrBeast</span>
          <span class="tag active">Technical Guruji</span>
          <span class="tag">BB Ki Vines</span>
          <span class="tag">Fukra Insaan</span>
          <span class="tag">Round2Hell</span>
          <span class="tag">Triggered Insaan</span>
          <span class="tag">Ashish Chanchlani</span>
          <span class="tag">+ Add Custom</span>
        </div>
        <div class="sep"></div>
        <div style="font-size:14px;font-weight:500;margin-bottom:10px;">Shorts per din:</div>
        <div style="display:flex;gap:8px;">
          <button class="btn btn-ghost" id="count-1" onclick="setCount(1)" style="padding:8px 18px;">1</button>
          <button class="btn btn-primary" id="count-2" onclick="setCount(2)" style="padding:8px 18px;">2</button>
          <button class="btn btn-ghost" id="count-3" onclick="setCount(3)" style="padding:8px 18px;">3</button>
        </div>
      </div>
    </div>

    <div class="card">
      <div style="font-size:15px;font-weight:600;margin-bottom:14px;">Is Hafte Ka Schedule</div>
      <div style="display:grid;grid-template-columns:repeat(7,1fr);gap:8px;text-align:center;">
        <div style="padding:12px 6px;background:rgba(124,92,252,0.15);border-radius:10px;border:1px solid rgba(124,92,252,0.3);">
          <div style="font-size:11px;color:var(--muted);">Mon</div>
          <div style="font-size:18px;margin:4px 0;">✅</div>
          <div style="font-size:10px;color:var(--green);">Uploaded</div>
        </div>
        <div style="padding:12px 6px;background:rgba(124,92,252,0.15);border-radius:10px;border:1px solid rgba(124,92,252,0.3);">
          <div style="font-size:11px;color:var(--muted);">Tue</div>
          <div style="font-size:18px;margin:4px 0;">✅</div>
          <div style="font-size:10px;color:var(--green);">Uploaded</div>
        </div>
        <div style="padding:12px 6px;background:rgba(245,158,11,0.1);border-radius:10px;border:1px solid rgba(245,158,11,0.2);">
          <div style="font-size:11px;color:var(--muted);">Wed</div>
          <div style="font-size:18px;margin:4px 0;">⏰</div>
          <div style="font-size:10px;color:var(--amber);">9:00 AM</div>
        </div>
        <div style="padding:12px 6px;background:var(--surface2);border-radius:10px;">
          <div style="font-size:11px;color:var(--muted);">Thu</div>
          <div style="font-size:18px;margin:4px 0;">🎬</div>
          <div style="font-size:10px;color:var(--muted);">Ready</div>
        </div>
        <div style="padding:12px 6px;background:var(--surface2);border-radius:10px;">
          <div style="font-size:11px;color:var(--muted);">Fri</div>
          <div style="font-size:18px;margin:4px 0;">🎬</div>
          <div style="font-size:10px;color:var(--muted);">Ready</div>
        </div>
        <div style="padding:12px 6px;background:var(--surface2);border-radius:10px;">
          <div style="font-size:11px;color:var(--muted);">Sat</div>
          <div style="font-size:18px;margin:4px 0;">⚡</div>
          <div style="font-size:10px;color:var(--muted);">Auto</div>
        </div>
        <div style="padding:12px 6px;background:var(--surface2);border-radius:10px;">
          <div style="font-size:11px;color:var(--muted);">Sun</div>
          <div style="font-size:18px;margin:4px 0;">⚡</div>
          <div style="font-size:10px;color:var(--muted);">Auto</div>
        </div>
      </div>
    </div>
  </div>

  <!-- AI CHAT -->
  <div class="page" id="page-aichat">
    <div style="display:flex;flex-direction:column;height:calc(100vh - 80px);">
      <div style="padding:24px 32px 0;">
        <div class="page-title">AI Assistant 🤖</div>
        <div class="page-sub">Claude AI — channel growth, shorts, aur sab problems ka solution</div>

        <div class="api-setup" id="api-setup">
          <div class="flex-between">
            <div>
              <div style="font-size:13px;font-weight:600;color:var(--accent);">Claude API Key Setup</div>
              <div style="font-size:12px;color:var(--muted);margin-top:2px;">claude.ai se free API key lo — bilkul free!</div>
            </div>
            <button class="btn btn-ghost" style="padding:6px 12px;font-size:11px;" onclick="window.open('https://console.anthropic.com')">API Key Lo ↗</button>
          </div>
          <input class="api-input" type="password" id="api-key-input" placeholder="sk-ant-... (Anthropic API key yahan paste karo)">
          <button class="btn btn-primary" style="width:100%;" onclick="saveApiKey()">Save Karo → Chat Shuru</button>
        </div>
      </div>

      <div class="chat-messages" id="chat-messages">
        <div class="msg ai fade-in">
          <div class="msg-avatar ai">🤖</div>
          <div class="msg-bubble">
            Namaste! Main ShortsAI ka AI Assistant hoon — Claude se powered. 🎬<br><br>
            Main aapki help kar sakta hoon:<br>
            • Channel grow kaise karein<br>
            • Kaun si video ka short viral hoga<br>
            • Title aur description improve karna<br>
            • Upload schedule optimize karna<br>
            • Technical problems solve karna<br><br>
            Kuch bhi poochho — main hamesha available hoon! 😊
          </div>
        </div>
      </div>

      <div style="padding:0 32px;">
        <div class="chat-input-wrap" style="border-radius:var(--radius);border:1px solid var(--border);">
          <textarea class="chat-input" id="chat-input" placeholder="Kuch bhi poochho... (Enter dabao bhejna ke liye)" onkeydown="handleKey(event)" rows="1"></textarea>
          <button class="send-btn" id="send-btn" onclick="sendMessage()">➤</button>
        </div>
        <div style="font-size:11px;color:var(--muted);text-align:center;margin-top:6px;margin-bottom:16px;">Claude AI • Anthropic • Secure</div>
      </div>
    </div>
  </div>

  <!-- SETTINGS -->
  <div class="page" id="page-settings">
    <div class="page-title">Settings ⚙️</div>
    <div class="page-sub">Account aur platform settings</div>

    <div class="grid-2">
      <div class="card">
        <div style="font-size:15px;font-weight:600;margin-bottom:14px;">Aapka Plan</div>
        <div style="background:rgba(124,92,252,0.08);border:1px solid rgba(124,92,252,0.2);border-radius:12px;padding:16px;margin-bottom:14px;">
          <div class="flex-between">
            <div>
              <div style="font-size:14px;font-weight:700;color:var(--accent);">Free Plan</div>
              <div style="font-size:12px;color:var(--muted);margin-top:2px;">1 short har 5 din</div>
            </div>
            <div style="font-size:22px;font-weight:700;">$0</div>
          </div>
        </div>
        <div style="font-size:13px;color:var(--muted);margin-bottom:12px;">Daily shorts ke liye upgrade karo:</div>
        <div style="background:var(--surface2);border-radius:12px;padding:16px;border:1px solid rgba(124,92,252,0.3);">
          <div class="flex-between">
            <div>
              <div style="font-size:14px;font-weight:700;">Pro Plan</div>
              <div style="font-size:12px;color:var(--muted);">Daily auto shorts · Sab platforms</div>
            </div>
            <div style="font-size:20px;font-weight:700;color:var(--accent);">$5<span style="font-size:12px;color:var(--muted);">/mo</span></div>
          </div>
          <button class="btn btn-primary" style="width:100%;margin-top:12px;" onclick="showNotif('Payment page khul raha hai...')">Upgrade to Pro</button>
        </div>
      </div>

      <div class="card">
        <div style="font-size:15px;font-weight:600;margin-bottom:14px;">Notifications</div>
        <div class="schedule-row">
          <div style="font-size:14px;">Short upload hone pe email</div>
          <label class="toggle"><input type="checkbox" checked><span class="toggle-track"></span><span class="toggle-thumb"></span></label>
        </div>
        <div class="schedule-row">
          <div style="font-size:14px;">Viral trending alert</div>
          <label class="toggle"><input type="checkbox" checked><span class="toggle-track"></span><span class="toggle-thumb"></span></label>
        </div>
        <div class="schedule-row">
          <div style="font-size:14px;">Views milestone (10K, 100K)</div>
          <label class="toggle"><input type="checkbox" checked><span class="toggle-track"></span><span class="toggle-thumb"></span></label>
        </div>
        <div class="schedule-row">
          <div style="font-size:14px;">Weekly report</div>
          <label class="toggle"><input type="checkbox"><span class="toggle-track"></span><span class="toggle-thumb"></span></label>
        </div>
      </div>
    </div>

    <div class="card">
      <div style="font-size:15px;font-weight:600;margin-bottom:14px;">Watermark & Branding</div>
      <div class="schedule-row">
        <div>
          <div style="font-size:14px;font-weight:500;">Channel Watermark</div>
          <div style="font-size:12px;color:var(--muted);">Shorts pe apna logo lagao</div>
        </div>
        <button class="btn btn-ghost" style="padding:7px 14px;" onclick="showNotif('Watermark upload feature Pro plan mein milega')">Upload</button>
      </div>
      <div class="schedule-row">
        <div>
          <div style="font-size:14px;font-weight:500;">Custom Intro (3 sec)</div>
          <div style="font-size:12px;color:var(--muted);">Har short se pehle chhota intro</div>
        </div>
        <label class="toggle"><input type="checkbox"><span class="toggle-track"></span><span class="toggle-thumb"></span></label>
      </div>
    </div>
  </div>

</div>

<!-- NEW SHORT MODAL -->
<div class="modal-bg" id="new-modal">
  <div class="modal">
    <div class="flex-between" style="margin-bottom:18px;">
      <div style="font-family:'Syne',sans-serif;font-size:18px;font-weight:800;">Naya Short Banao</div>
      <button class="btn btn-ghost" style="padding:6px 12px;" onclick="document.getElementById('new-modal').classList.remove('open')">✕</button>
    </div>
    <div style="font-size:13px;color:var(--muted);margin-bottom:8px;">YouTuber ka naam ya channel URL:</div>
    <input class="api-input" placeholder="CarryMinati / Channel URL / Video URL" id="channel-input">
    <div style="font-size:13px;color:var(--muted);margin:12px 0 8px;">Upload kahan karna hai:</div>
    <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:16px;">
      <span class="tag active" id="t-yt" onclick="toggleTag('t-yt')">▶ YouTube</span>
      <span class="tag active" id="t-ig" onclick="toggleTag('t-ig')">📷 Instagram</span>
      <span class="tag" id="t-fb" onclick="toggleTag('t-fb')">👍 Facebook</span>
    </div>
    <button class="btn btn-primary" style="width:100%;" onclick="createShort()">🤖 AI se Short Banao + Upload Karo</button>
    <div style="font-size:11px;color:var(--muted);text-align:center;margin-top:8px;">AI khud best clip, title, description, tags sab select karega</div>
  </div>
</div>

<!-- NOTIFICATION -->
<div class="notif" id="notif"></div>

<script>
// Page navigation
function showPage(name, btn) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('active'));
  document.getElementById('page-' + name).classList.add('active');
  if (btn) btn.classList.add('active');
}

// Platform connection
const connections = { yt: false, ig: false, fb: false };
function connectPlatform(p) {
  const btn = document.getElementById(p + '-btn');
  const dot = document.getElementById(p + '-dot');
  const status = document.getElementById(p + '-status');
  const card = document.getElementById(p + '-card');
  const names = { yt: 'YouTube', ig: 'Instagram', fb: 'Facebook' };

  if (!connections[p]) {
    connections[p] = true;
    btn.textContent = '✓ Connected — ' + names[p];
    btn.className = 'connect-btn connected';
    dot.className = 'status-dot dot-green';
    status.textContent = 'Connected ✓';
    card.classList.add('connected');
    showNotif('✅ ' + names[p] + ' connect ho gaya! AI ab automatically upload karega.');
    const count = Object.values(connections).filter(Boolean).length;
    document.getElementById('dash-platforms').textContent = count;
  } else {
    connections[p] = false;
    btn.textContent = 'Connect ' + names[p];
    btn.className = 'connect-btn disconnected';
    dot.className = 'status-dot dot-gray';
    status.textContent = 'Connect nahi hai';
    card.classList.remove('connected');
    const count = Object.values(connections).filter(Boolean).length;
    document.getElementById('dash-platforms').textContent = count;
  }
}

// Notification
function showNotif(msg) {
  const n = document.getElementById('notif');
  n.textContent = msg;
  n.classList.add('show');
  setTimeout(() => n.classList.remove('show'), 3500);
}

// Timer
function updateTimer() {
  const now = new Date();
  const next = new Date();
  next.setHours(9, 0, 0, 0);
  if (next <= now) next.setDate(next.getDate() + 1);
  const diff = next - now;
  const h = String(Math.floor(diff/3600000)).padStart(2,'0');
  const m = String(Math.floor((diff%3600000)/60000)).padStart(2,'0');
  const s = String(Math.floor((diff%60000)/1000)).padStart(2,'0');
  document.getElementById('next-upload').textContent = h + ':' + m + ':' + s;
}
setInterval(updateTimer, 1000); updateTimer();

// Tags
function toggleTag(id) {
  const el = document.getElementById(id);
  el.classList.toggle('active');
}

// Count
function setCount(n) {
  [1,2,3].forEach(i => {
    document.getElementById('count-' + i).className = i === n ? 'btn btn-primary' : 'btn btn-ghost';
    document.getElementById('count-' + i).style.padding = '8px 18px';
  });
}

// Create short
function createShort() {
  const ch = document.getElementById('channel-input').value || 'CarryMinati';
  document.getElementById('new-modal').classList.remove('open');
  showNotif('🤖 AI ' + ch + ' ka viral short bana raha hai... upload hoga!');
  setTimeout(() => showNotif('🎉 Short ready aur upload ho gaya!'), 3000);
}

// AI CHAT
let chatHistory = [];
let apiKey = localStorage.getItem('claude_api_key') || '';

if (apiKey) {
  document.getElementById('api-setup').style.display = 'none';
}

function saveApiKey() {
  const key = document.getElementById('api-key-input').value.trim();
  if (!key.startsWith('sk-ant-')) {
    showNotif('❌ API key sahi nahi hai — sk-ant- se shuru honi chahiye');
    return;
  }
  apiKey = key;
  localStorage.setItem('claude_api_key', key);
  document.getElementById('api-setup').style.display = 'none';
  showNotif('✅ Claude API connected! Ab chat karo.');
}

function handleKey(e) {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); sendMessage(); }
}

async function sendMessage() {
  const input = document.getElementById('chat-input');
  const msg = input.value.trim();
  if (!msg) return;
  if (!apiKey) { showNotif('❌ Pehle API key save karo!'); return; }

  input.value = '';
  addMsg('user', msg);
  chatHistory.push({ role: 'user', content: msg });

  const btn = document.getElementById('send-btn');
  btn.disabled = true;

  const typingEl = addTyping();

  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey,
        'anthropic-version': '2023-06-01',
        'anthropic-dangerous-direct-browser-access': 'true'
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-6',
        max_tokens: 1000,
        system: `Tum ShortsAI platform ka AI assistant ho. Tum Hindi/Hinglish mein baat karte ho. Tum YouTube aur Instagram creators ki help karte ho viral shorts banana, channel grow karna, SEO optimize karna, aur viral trending topics find karna. Tum friendly, helpful aur concise ho. Har jawab mein emojis use karo. Agar user YouTube/Instagram/shorts ke baare mein pooche, to practical tips do. Platform ka naam "ShortsAI" hai.`,
        messages: chatHistory
      })
    });

    typingEl.remove();

    if (!res.ok) {
      const err = await res.json();
      throw new Error(err.error?.message || 'API error');
    }

    const data = await res.json();
    const reply = data.content[0].text;
    chatHistory.push({ role: 'assistant', content: reply });
    addMsg('ai', reply);

  } catch (err) {
    typingEl.remove();
    addMsg('ai', '❌ Error: ' + err.message + '\n\nAPI key check karo ya thodi der baad try karo.');
  }

  btn.disabled = false;
}

function addMsg(role, text) {
  const wrap = document.getElementById('chat-messages');
  const div = document.createElement('div');
  div.className = 'msg ' + role + ' fade-in';
  const avatar = document.createElement('div');
  avatar.className = 'msg-avatar ' + role;
  avatar.textContent = role === 'ai' ? '🤖' : '👤';
  const bubble = document.createElement('div');
  bubble.className = 'msg-bubble';
  bubble.style.whiteSpace = 'pre-wrap';
  bubble.textContent = text;
  div.appendChild(avatar);
  div.appendChild(bubble);
  wrap.appendChild(div);
  wrap.scrollTop = wrap.scrollHeight;
  return div;
}

function addTyping() {
  const wrap = document.getElementById('chat-messages');
  const div = document.createElement('div');
  div.className = 'msg ai fade-in';
  div.innerHTML = '<div class="msg-avatar ai">🤖</div><div class="msg-bubble"><div class="typing"><span></span><span></span><span></span></div></div>';
  wrap.appendChild(div);
  wrap.scrollTop = wrap.scrollHeight;
  return div;
}
</script>
</body>
</html>
