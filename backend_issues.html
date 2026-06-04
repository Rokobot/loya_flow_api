<!DOCTYPE html>
<html lang="az">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Backend API Issues</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap');

  *{box-sizing:border-box;margin:0;padding:0}

  @keyframes webDraw{from{stroke-dashoffset:600}to{stroke-dashoffset:0}}
  @keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-7px)}}
  @keyframes slideUp{from{opacity:0;transform:translateY(24px)}to{opacity:1;transform:translateY(0)}}
  @keyframes pulseRed{0%,100%{box-shadow:0 0 0 0 rgba(220,38,38,.4)}70%{box-shadow:0 0 0 8px rgba(220,38,38,0)}}
  @keyframes spin{to{transform:rotate(360deg)}}

  html{scroll-behavior:smooth}
  body{background:#07090f;color:#e2e8f0;font-family:'Inter',sans-serif;min-height:100vh;overflow-x:hidden}

  /* ── web bg ── */
  .web-bg{position:fixed;top:0;left:0;width:100%;height:100%;pointer-events:none;z-index:0;opacity:.07}
  .web-bg svg{width:100%;height:100%}

  /* ── layout ── */
  .container{position:relative;z-index:1;max-width:900px;margin:0 auto;padding:0 2rem 4rem}

  /* ── hero ── */
  .hero{text-align:center;padding:3.5rem 0 2.5rem}
  .spider-icon{font-size:52px;display:inline-block;animation:float 3s ease-in-out infinite;margin-bottom:1rem;filter:drop-shadow(0 0 20px rgba(220,38,38,.5))}
  .hero h1{font-size:36px;font-weight:600;color:#f1f5f9;letter-spacing:-1px;line-height:1.2}
  .hero h1 em{font-style:normal;color:#ef4444}
  .hero p{font-size:14px;color:#64748b;margin-top:10px}
  .hero-meta{display:flex;align-items:center;justify-content:center;gap:8px;margin-top:14px;flex-wrap:wrap}
  .meta-pill{font-size:12px;padding:4px 12px;border-radius:20px;border:0.5px solid #1e2a3a;color:#94a3b8;background:#0d1524}

  /* ── stats ── */
  .stats{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:2.5rem;animation:slideUp .5s ease both}
  .stat{background:#0d1524;border:0.5px solid #1e2a3a;border-radius:14px;padding:1.25rem;text-align:center}
  .stat-n{font-size:32px;font-weight:600}
  .stat-l{font-size:12px;color:#64748b;margin-top:4px}

  /* ── section label ── */
  .sec-label{display:flex;align-items:center;gap:10px;margin:2rem 0 1rem;font-size:11px;font-weight:600;letter-spacing:2px;text-transform:uppercase}
  .sec-label::after{content:'';flex:1;height:1px;background:linear-gradient(90deg,currentColor 0%,transparent 100%);opacity:.25}

  /* ── card ── */
  .card{background:#0d1524;border:0.5px solid #1e2a3a;border-radius:16px;margin-bottom:16px;overflow:hidden;animation:slideUp .5s ease both;transition:border-color .2s}
  .card:hover{border-color:#2d3f55}

  .card-head{display:flex;align-items:flex-start;gap:14px;padding:20px 22px;cursor:pointer;transition:background .2s;user-select:none}
  .card-head:hover{background:#111f35}
  .card-head-left{display:flex;align-items:flex-start;gap:14px;flex:1;min-width:0}

  .num{width:34px;height:34px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:600;flex-shrink:0;margin-top:1px}
  .num-bug{background:#3d1515;color:#ef4444;animation:pulseRed 2.5s infinite}
  .num-new{background:#0c2246;color:#60a5fa}

  .card-meta{flex:1;min-width:0}
  .card-top{display:flex;align-items:center;gap:10px;flex-wrap:wrap;margin-bottom:6px}
  .card-title{font-size:15px;font-weight:500;color:#f1f5f9;line-height:1.4}
  .card-endpoint{font-family:'JetBrains Mono',monospace;font-size:12px;color:#64748b;word-break:break-all}

  .badge{font-size:10px;font-weight:600;padding:3px 10px;border-radius:20px;letter-spacing:.3px;white-space:nowrap}
  .b-red{background:#3d1515;color:#f87171}
  .b-amber{background:#2d1f08;color:#fbbf24}
  .b-blue{background:#0c2246;color:#60a5fa}
  .b-green{background:#0a2310;color:#4ade80}

  .method{font-family:'JetBrains Mono',monospace;font-size:10px;font-weight:600;padding:3px 9px;border-radius:6px;flex-shrink:0}
  .GET{background:#0a2310;color:#4ade80}
  .POST{background:#0c2246;color:#60a5fa}
  .PUT{background:#2d1f08;color:#fbbf24}

  .chevron-btn{width:28px;height:28px;display:flex;align-items:center;justify-content:center;color:#4b5563;font-size:18px;flex-shrink:0;transition:transform .25s;margin-top:3px}
  .chevron-btn.open{transform:rotate(180deg)}

  /* ── body ── */
  .card-body{max-height:0;overflow:hidden;transition:max-height .4s cubic-bezier(.4,0,.2,1)}
  .card-body.open{max-height:1200px}
  .card-body-inner{padding:0 22px 22px}

  .divider{height:0.5px;background:#1e2a3a;margin-bottom:20px}

  /* ── 2col grid ── */
  .grid2{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:16px}
  .grid3{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:16px}
  .mini{background:#060d1a;border:0.5px solid #1e2a3a;border-radius:10px;padding:14px 16px}
  .mini-label{font-size:10px;font-weight:600;color:#475569;text-transform:uppercase;letter-spacing:1px;margin-bottom:8px}
  .mini-body{font-size:13px;color:#94a3b8;line-height:1.7}

  /* ── code block ── */
  .code{background:#04080f;border:0.5px solid #1a2535;border-radius:10px;padding:16px 18px;font-family:'JetBrains Mono',monospace;font-size:12.5px;color:#94a3b8;line-height:1.9;margin-bottom:14px;white-space:pre-wrap;word-break:break-all;position:relative}
  .code .ok{color:#4ade80}
  .code .err{color:#f87171}
  .code .dim{color:#334155}
  .code .key{color:#93c5fd}
  .code .val{color:#a3e635}
  .code .cmt{color:#334155;font-style:italic}

  /* ── fix box ── */
  .fix-box{background:#0a1220;border-left:3px solid #ef4444;border-radius:0 8px 8px 0;padding:14px 16px;font-size:13px;color:#94a3b8;line-height:1.8;margin-bottom:14px}
  .fix-box.blue{border-left-color:#3b82f6}
  .fix-box strong{color:#e2e8f0;font-weight:500}
  .fix-box code{font-family:'JetBrains Mono',monospace;font-size:11.5px;background:#111f35;padding:1px 6px;border-radius:4px;color:#93c5fd}

  /* ── params table ── */
  .params-wrap{margin-bottom:14px;border:0.5px solid #1e2a3a;border-radius:10px;overflow:hidden}
  table{width:100%;border-collapse:collapse;font-size:13px}
  th{text-align:left;font-size:10px;font-weight:600;color:#475569;padding:10px 14px;letter-spacing:.8px;text-transform:uppercase;background:#060d1a;border-bottom:0.5px solid #1e2a3a}
  td{padding:10px 14px;border-bottom:0.5px solid #0d1524;color:#94a3b8;vertical-align:top}
  tr:last-child td{border-bottom:none}
  td code{font-family:'JetBrains Mono',monospace;font-size:11.5px;background:#111f35;padding:1px 6px;border-radius:4px;color:#93c5fd}
  .req{color:#f87171;font-size:11px;font-weight:600}

  /* ── step indicator ── */
  .steps{display:flex;align-items:center;gap:8px;margin-bottom:14px;flex-wrap:wrap}
  .step{background:#0c2246;color:#60a5fa;font-size:11px;font-weight:600;padding:4px 12px;border-radius:20px;border:0.5px solid #1a3460}

  /* ── footer ── */
  .footer{text-align:center;padding:2rem 0;font-size:12px;color:#334155;border-top:0.5px solid #111827;margin-top:1rem}

  @media(max-width:600px){
    .stats{grid-template-columns:repeat(2,1fr)}
    .grid2,.grid3{grid-template-columns:1fr}
    .hero h1{font-size:26px}
    .card-head{padding:16px}
    .card-body-inner{padding:0 16px 16px}
  }
</style>
</head>
<body>

<div class="web-bg">
<svg viewBox="0 0 900 900" preserveAspectRatio="xMidYMid slice" xmlns="http://www.w3.org/2000/svg">
  <g stroke="#ef4444" stroke-width="1" fill="none">
    <line x1="450" y1="0" x2="0" y2="250"/><line x1="450" y1="0" x2="150" y2="250"/>
    <line x1="450" y1="0" x2="300" y2="250"/><line x1="450" y1="0" x2="450" y2="250"/>
    <line x1="450" y1="0" x2="600" y2="250"/><line x1="450" y1="0" x2="750" y2="250"/>
    <line x1="450" y1="0" x2="900" y2="250"/>
    <path d="M0,60 Q225,40 450,60 Q675,80 900,60"/>
    <path d="M0,130 Q225,110 450,130 Q675,150 900,130"/>
    <path d="M0,200 Q225,180 450,200 Q675,220 900,200"/>
    <path d="M0,270 Q225,250 450,270 Q675,290 900,270"/>
    <path d="M0,340 Q225,320 450,340 Q675,360 900,340"/>
    <path d="M0,410 Q225,390 450,410 Q675,430 900,410"/>
    <path d="M0,480 Q225,460 450,480 Q675,500 900,480"/>
    <path d="M0,550 Q225,530 450,550 Q675,570 900,550"/>
    <path d="M0,620 Q225,600 450,620 Q675,640 900,620"/>
    <path d="M0,690 Q225,670 450,690 Q675,710 900,690"/>
    <path d="M0,760 Q225,740 450,760 Q675,780 900,760"/>
    <path d="M0,830 Q225,810 450,830 Q675,850 900,830"/>
    <path d="M0,900 Q225,880 450,900 Q675,920 900,900"/>
  </g>
</svg>
</div>

<div class="container">

  <div class="hero">
    <div class="spider-icon">🕷️</div>
    <h1>Backend <em>API</em> Issue Log</h1>
    <p>Auth Service · Rafet → Backend Team</p>
    <div class="hero-meta">
      <span class="meta-pill">🕸️ Auth Service</span>
      <span class="meta-pill">📍 62.171.172.254:8083</span>
      <span class="meta-pill">📅 2025</span>
    </div>
  </div>

  <div class="stats">
    <div class="stat" style="animation-delay:.05s">
      <div class="stat-n" style="color:#ef4444">4</div>
      <div class="stat-l">Kritik Bug</div>
    </div>
    <div class="stat" style="animation-delay:.1s">
      <div class="stat-n" style="color:#60a5fa">4</div>
      <div class="stat-l">Yeni Endpoint</div>
    </div>
    <div class="stat" style="animation-delay:.15s">
      <div class="stat-n" style="color:#fbbf24">2</div>
      <div class="stat-l">Auth Fix</div>
    </div>
    <div class="stat" style="animation-delay:.2s">
      <div class="stat-n" style="color:#4ade80">2</div>
      <div class="stat-l">PIN API</div>
    </div>
  </div>

  <!-- ───── BUGS ───── -->
  <div class="sec-label" style="color:#ef4444">Bug-lar — düzəldilməli</div>

  <!-- #1 -->
  <div class="card" style="animation-delay:.08s">
    <div class="card-head" onclick="toggle(this)">
      <div class="card-head-left">
        <div class="num num-bug">1</div>
        <div class="card-meta">
          <div class="card-top">
            <span class="card-title">Login — yanlış credentials-də 500 qaytarır, 200 olmalıdır</span>
            <span class="badge b-red">Critical</span>
          </div>
          <div class="card-endpoint">POST /api/auth/login</div>
        </div>
      </div>
      <div class="chevron-btn">&#8964;</div>
    </div>
    <div class="card-body">
      <div class="card-body-inner">
        <div class="divider"></div>
        <div class="grid2">
          <div class="mini">
            <div class="mini-label">⚡ Mövcud davranış</div>
            <div class="mini-body">Telefon nömrəsi və ya şifrə yanlış olduqda server <span style="color:#f87171;font-weight:500">HTTP 500 Internal Server Error</span> qaytarır. Flutter tərəf <code style="font-family:monospace;font-size:12px;background:#111f35;padding:1px 5px;border-radius:4px;color:#f87171">DioExceptionType.badResponse</code> ilə crash edir.</div>
          </div>
          <div class="mini">
            <div class="mini-label">✅ Gözlənilən davranış</div>
            <div class="mini-body">Sorğu uğurla işlənməli, <span style="color:#4ade80;font-weight:500">HTTP 200 OK</span> + JSON body qaytarılmalıdır. <code style="font-family:monospace;font-size:12px;background:#111f35;padding:1px 5px;border-radius:4px;color:#4ade80">success: false</code> flag-i ilə istifadəçi dostu mesaj daxil olmalıdır.</div>
          </div>
        </div>

        <div class="mini" style="margin-bottom:14px">
          <div class="mini-label">📱 Flutter log (mövcud)</div>
          <div style="font-family:'JetBrains Mono',monospace;font-size:12px;color:#f87171;line-height:1.9;margin-top:4px">
            ╔╣ DioError ║ Status: 500 Internal Server Error ║ Time: 557 ms<br>
            ║ http://62.171.172.254:8083/api/auth/login<br>
            ╔ DioExceptionType.badResponse<br>
            ║ BadHttpRequestException: Telefon nömrəsi və ya şifrə yanlışdır.
          </div>
        </div>

        <div class="code"><span class="dim">// ❌ Mövcud response</span>
<span class="err">HTTP 500 Internal Server Error</span>
<span class="err">AuthService.Application.Exceptions.BadHttpRequestException:</span>
<span class="err">  Telefon nömrəsi və ya şifrə yanlışdır.</span>

<span class="dim">// ✅ Olmalıdır</span>
<span class="ok">HTTP 200 OK</span>
{
  <span class="key">"success"</span>: <span class="err">false</span>,
  <span class="key">"message"</span>: <span class="val">"Telefon nömrəsi və ya şifrə yanlışdır."</span>
}</div>

        <div class="fix-box">
          <strong>🔧 Fix:</strong> <code>BadHttpRequestException</code> tipini global exception handler-də tutun. Bu exception HTTP 500-ə çevrilməməli, <code>200 OK</code> + <code>success: false</code> + mesaj ilə qaytarılmalıdır. <code>ExceptionMiddleware</code> və ya <code>GlobalExceptionHandler</code>-də bu tipi ayrıca handle edin.
        </div>
      </div>
    </div>
  </div>

  <!-- #2 -->
  <div class="card" style="animation-delay:.12s">
    <div class="card-head" onclick="toggle(this)">
      <div class="card-head-left">
        <div class="num num-bug">2</div>
        <div class="card-meta">
          <div class="card-top">
            <span class="card-title">Login — ilk sorğuda 500, ikinci sorğuda success olur</span>
            <span class="badge b-red">High</span>
          </div>
          <div class="card-endpoint">POST /api/auth/login · Race condition / cold start</div>
        </div>
      </div>
      <div class="chevron-btn">&#8964;</div>
    </div>
    <div class="card-body">
      <div class="card-body-inner">
        <div class="divider"></div>
        <div class="mini" style="margin-bottom:14px">
          <div class="mini-label">📋 Təsvir</div>
          <div class="mini-body">Credentials tamamilə doğru olsa belə, ilk login sorğusu <span style="color:#f87171">500</span> qaytarır. Flutter tərəfdə catch blokunda ikinci try işə düşür və <span style="color:#4ade80">success</span> olur. Bu davranış server tərəfdəki bir initialization probleminə işarə edir.</div>
        </div>
        <div class="grid3">
          <div class="mini">
            <div class="mini-label">Səbəb 1</div>
            <div class="mini-body">DB connection pool ilk sorğuda hazır olmaya bilər (cold start).</div>
          </div>
          <div class="mini">
            <div class="mini-label">Səbəb 2</div>
            <div class="mini-body">Middleware sırası: token/session init middleware ilk sorğuda fail edə bilər.</div>
          </div>
          <div class="mini">
            <div class="mini-label">Səbəb 3</div>
            <div class="mini-body">Redis / cache / token store server start-da tam init olmadan sorğu gəlir.</div>
          </div>
        </div>
        <div class="fix-box">
          <strong>🔧 Fix tövsiyələri:</strong> Server başladıqda bütün servislərin (<code>DB</code>, <code>Redis</code>, <code>cache</code>, <code>token store</code>) hazır olduğunu yoxlamaq üçün health check endpoint əlavə edin. Middleware sırasını nəzərdən keçirin. Connection pool warmup tətbiq edin.
        </div>
      </div>
    </div>
  </div>

  <!-- #3 -->
  <div class="card" style="animation-delay:.16s">
    <div class="card-head" onclick="toggle(this)">
      <div class="card-head-left">
        <div class="num num-bug">3</div>
        <div class="card-meta">
          <div class="card-top">
            <span class="card-title">Eyni parol yenidən reset edilə bilir</span>
            <span class="badge b-amber">Medium</span>
          </div>
          <div class="card-endpoint">POST /api/auth/reset-password · Güvənlik problemi</div>
        </div>
      </div>
      <div class="chevron-btn">&#8964;</div>
    </div>
    <div class="card-body">
      <div class="card-body-inner">
        <div class="divider"></div>
        <div class="grid2" style="margin-bottom:14px">
          <div class="mini">
            <div class="mini-label">🚨 Problem</div>
            <div class="mini-body">Password reset endpoint-ində mövcud parol ilə eyni parolu yenidən set etmək mümkündür. Bu güvənlik riskidir — istifadəçi real mənada parolunu dəyişməmiş olur.</div>
          </div>
          <div class="mini">
            <div class="mini-label">✅ Gözlənilən</div>
            <div class="mini-body">Yeni parol köhnə parol ilə eyni olduqda <code style="font-family:monospace;font-size:12px;background:#111f35;padding:1px 5px;border-radius:4px;color:#fbbf24">400 Bad Request</code> + istifadəçi dostu xəta mesajı qaytarılmalıdır.</div>
          </div>
        </div>
        <div class="code"><span class="dim">// ❌ Mövcud — eyni parol qəbul edilir</span>
<span class="err">POST /api/auth/reset-password</span>
{ <span class="key">"new_password"</span>: <span class="val">"mövcud_parol"</span> }
<span class="err">→ HTTP 200 OK  (yanlışdır)</span>

<span class="dim">// ✅ Olmalıdır</span>
<span class="ok">HTTP 200 OK</span>
{
  <span class="key">"success"</span>: <span class="err">false</span>,
  <span class="key">"message"</span>: <span class="val">"Yeni parol köhnə parolla eyni ola bilməz."</span>
}</div>
        <div class="fix-box">
          <strong>🔧 Fix:</strong> Reset əməliyyatında yeni parolu hash-ləyib mövcud hash ilə <code>password_verify()</code> / <code>BCrypt.Verify()</code> ilə müqayisə edin. Eyni olduqda <code>success: false</code> + mesaj qaytarın. Bu yoxlama həm #7 PIN reset-ə də tətbiq edilməlidir.
        </div>
      </div>
    </div>
  </div>

  <!-- #4 -->
  <div class="card" style="animation-delay:.2s">
    <div class="card-head" onclick="toggle(this)">
      <div class="card-head-left">
        <div class="num num-bug">4</div>
        <div class="card-meta">
          <div class="card-top">
            <span class="card-title">Register — mövcud nömrə üçün 500 qaytarır</span>
            <span class="badge b-red">Critical</span>
          </div>
          <div class="card-endpoint">POST /api/auth/register</div>
        </div>
      </div>
      <div class="chevron-btn">&#8964;</div>
    </div>
    <div class="card-body">
      <div class="card-body-inner">
        <div class="divider"></div>
        <div class="mini" style="margin-bottom:14px">
          <div class="mini-label">📱 Flutter log (mövcud)</div>
          <div style="font-family:'JetBrains Mono',monospace;font-size:12px;color:#f87171;line-height:1.9;margin-top:4px">
            ║ AuthService.Application.Exceptions.BadHttpRequestException:<br>
            ║ Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib
          </div>
        </div>
        <div class="code"><span class="dim">// ❌ Mövcud</span>
<span class="err">HTTP 500 Internal Server Error</span>
<span class="err">BadHttpRequestException: Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib.</span>

<span class="dim">// ✅ Olmalıdır</span>
<span class="ok">HTTP 200 OK</span>
{
  <span class="key">"success"</span>: <span class="err">false</span>,
  <span class="key">"message"</span>: <span class="val">"Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib."</span>
}</div>
        <div class="fix-box">
          <strong>🔧 Fix:</strong> #1 ilə eyni kök problem — <code>BadHttpRequestException</code> global handler-də düzgün handle edilmir. Register controller-də nömrə mövcudluğunu əvvəlcədən DB-dən yoxlayın, exception-a düşmədən <code>200 + success: false</code> qaytarın.
        </div>
      </div>
    </div>
  </div>

  <!-- ───── NEW ENDPOINTS ───── -->
  <div class="sec-label" style="color:#3b82f6;margin-top:2.5rem">Yeni endpointlər — əlavə edilməli</div>

  <!-- #5 -->
  <div class="card" style="animation-delay:.24s">
    <div class="card-head" onclick="toggle(this)">
      <div class="card-head-left">
        <div class="num num-new">5</div>
        <div class="card-meta">
          <div class="card-top">
            <span class="method PUT">PUT</span>
            <span class="card-title">Müştəri nömrə dəyişmə API</span>
            <span class="badge b-blue">New Endpoint</span>
          </div>
          <div class="card-endpoint">/api/customer/change-phone · Auth required</div>
        </div>
      </div>
      <div class="chevron-btn">&#8964;</div>
    </div>
    <div class="card-body">
      <div class="card-body-inner">
        <div class="divider"></div>
        <div class="code"><span class="dim">PUT /api/customer/change-phone</span>
<span class="dim">Authorization: Bearer {token}</span></div>
        <div class="params-wrap">
          <table>
            <tr><th>Sahə</th><th>Tip</th><th>Tələb</th><th>Açıqlama</th></tr>
            <tr><td><code>current_phone</code></td><td>string</td><td><span class="req">✱ Məcburi</span></td><td>İstifadəçinin mövcud telefon nömrəsi</td></tr>
            <tr><td><code>new_phone</code></td><td>string</td><td><span class="req">✱ Məcburi</span></td><td>Yeni telefon nömrəsi (format: +994XXXXXXXXX)</td></tr>
            <tr><td><code>otp_code</code></td><td>string</td><td><span class="req">✱ Məcburi</span></td><td>Yeni nömrəyə göndərilən 6 rəqəmli OTP</td></tr>
          </table>
        </div>
        <div class="code"><span class="dim">// ✅ Success</span>
<span class="ok">HTTP 200 OK</span>
{ <span class="key">"success"</span>: true, <span class="key">"message"</span>: <span class="val">"Nömrə uğurla dəyişdirildi."</span> }

<span class="dim">// ❌ OTP xəta</span>
{ <span class="key">"success"</span>: false, <span class="key">"message"</span>: <span class="val">"OTP yanlışdır və ya müddəti bitib."</span> }

<span class="dim">// ❌ Nömrə artıq mövcuddur</span>
{ <span class="key">"success"</span>: false, <span class="key">"message"</span>: <span class="val">"Bu nömrə artıq istifadədədir."</span> }</div>
        <div class="fix-box blue">
          <strong>📝 Qeyd:</strong> OTP göndərmə üçün əvvəlcə <code>POST /api/auth/send-otp</code> sorğusu atılmalıdır. Nömrə dəyişdikdən sonra mövcud token yenilənməlidir (yeni nömrə ilə yeni token qaytarın).
        </div>
      </div>
    </div>
  </div>

  <!-- #6 -->
  <div class="card" style="animation-delay:.28s">
    <div class="card-head" onclick="toggle(this)">
      <div class="card-head-left">
        <div class="num num-new">6</div>
        <div class="card-meta">
          <div class="card-top">
            <span class="method GET">GET</span>
            <span class="card-title">İstifadəçi məlumatlarını alma API</span>
            <span class="badge b-blue">New Endpoint</span>
          </div>
          <div class="card-endpoint">/api/customer/me · Auth required</div>
        </div>
      </div>
      <div class="chevron-btn">&#8964;</div>
    </div>
    <div class="card-body">
      <div class="card-body-inner">
        <div class="divider"></div>
        <div class="code"><span class="dim">GET /api/customer/me</span>
<span class="dim">Authorization: Bearer {token}</span>
<span class="dim">Content-Type: application/json</span></div>
        <div class="code"><span class="dim">// ✅ Success response</span>
<span class="ok">HTTP 200 OK</span>
{
  <span class="key">"success"</span>: true,
  <span class="key">"data"</span>: {
    <span class="key">"id"</span>: 1,
    <span class="key">"full_name"</span>: <span class="val">"Əli Həsənov"</span>,
    <span class="key">"phone"</span>: <span class="val">"+994501234567"</span>,
    <span class="key">"email"</span>: <span class="val">"ali@example.com"</span>,
    <span class="key">"profile_image"</span>: <span class="val">"https://..."</span>,
    <span class="key">"created_at"</span>: <span class="val">"2024-01-01T00:00:00Z"</span>,
    <span class="key">"is_verified"</span>: true,
    <span class="key">"has_pin"</span>: true
  }
}

<span class="dim">// ❌ Token invalid / expired</span>
{ <span class="key">"success"</span>: false, <span class="key">"message"</span>: <span class="val">"Unauthorized."</span> }</div>
        <div class="fix-box blue">
          <strong>📝 Qeyd:</strong> <code>has_pin</code> sahəsi Flutter tərəfin PIN ekranına yönləndirmə qərarı üçün lazımdır. Token refresh lazım olduqda <code>401</code> qaytarın, <code>500</code> yox.
        </div>
      </div>
    </div>
  </div>

  <!-- #7 -->
  <div class="card" style="animation-delay:.32s">
    <div class="card-head" onclick="toggle(this)">
      <div class="card-head-left">
        <div class="num num-new">7</div>
        <div class="card-meta">
          <div class="card-top">
            <span class="method POST">POST</span>
            <span class="card-title">PIN doğrulama API</span>
            <span class="badge b-blue">New Endpoint</span>
          </div>
          <div class="card-endpoint">/api/auth/pin/verify · Brute-force qoruma daxil</div>
        </div>
      </div>
      <div class="chevron-btn">&#8964;</div>
    </div>
    <div class="card-body">
      <div class="card-body-inner">
        <div class="divider"></div>
        <div class="code"><span class="dim">POST /api/auth/pin/verify</span>
<span class="dim">Content-Type: application/json</span></div>
        <div class="params-wrap">
          <table>
            <tr><th>Sahə</th><th>Tip</th><th>Tələb</th><th>Açıqlama</th></tr>
            <tr><td><code>phone</code></td><td>string</td><td><span class="req">✱ Məcburi</span></td><td>İstifadəçi telefon nömrəsi</td></tr>
            <tr><td><code>pin</code></td><td>string</td><td><span class="req">✱ Məcburi</span></td><td>4–6 rəqəmli PIN kod</td></tr>
          </table>
        </div>
        <div class="code"><span class="dim">// ✅ Uğurlu doğrulama</span>
<span class="ok">HTTP 200 OK</span>
{
  <span class="key">"success"</span>: true,
  <span class="key">"token"</span>: <span class="val">"eyJhbGciOiJIUzI1NiIsInR5..."</span>,
  <span class="key">"user"</span>: { <span class="key">"id"</span>: 1, <span class="key">"full_name"</span>: <span class="val">"Əli Həsənov"</span> }
}

<span class="dim">// ❌ Yanlış PIN</span>
{ <span class="key">"success"</span>: false, <span class="key">"message"</span>: <span class="val">"PIN yanlışdır."</span>, <span class="key">"attempts_left"</span>: 4 }

<span class="dim">// ❌ Çox sayda yanlış cəhd</span>
{
  <span class="key">"success"</span>: false,
  <span class="key">"message"</span>: <span class="val">"Çox sayda yanlış cəhd. 5 dəqiqə sonra yenidən cəhd edin."</span>,
  <span class="key">"locked_until"</span>: <span class="val">"2024-01-01T12:05:00Z"</span>
}</div>
        <div class="fix-box blue">
          <strong>📝 Brute-force qoruma:</strong> Ardıcıl 5 yanlış cəhddən sonra hesab müvəqqəti kilidlənməlidir (<code>locked_until</code> qaytarın). <code>attempts_left</code> sahəsi Flutter UI üçün faydalıdır.
        </div>
      </div>
    </div>
  </div>

  <!-- #8 -->
  <div class="card" style="animation-delay:.36s">
    <div class="card-head" onclick="toggle(this)">
      <div class="card-head-left">
        <div class="num num-new">8</div>
        <div class="card-meta">
          <div class="card-top">
            <span class="method POST">POST</span>
            <span class="card-title">PIN sıfırlama — Forget PIN</span>
            <span class="badge b-blue">New Endpoint</span>
          </div>
          <div class="card-endpoint">/api/auth/pin/forgot + /api/auth/pin/reset · 2 addımlı flow</div>
        </div>
      </div>
      <div class="chevron-btn">&#8964;</div>
    </div>
    <div class="card-body">
      <div class="card-body-inner">
        <div class="divider"></div>
        <div class="steps">
          <span class="step">Addım 1 — OTP göndər</span>
          <span style="color:#334155;font-size:18px">→</span>
          <span class="step">Addım 2 — Yeni PIN set et</span>
        </div>

        <div class="code"><span class="dim">// ── Addım 1: OTP göndər ──────────────────────────</span>
<span class="dim">POST /api/auth/pin/forgot</span>
{ <span class="key">"phone"</span>: <span class="val">"+994501234567"</span> }

<span class="ok">→ HTTP 200 OK</span>
{ <span class="key">"success"</span>: true, <span class="key">"message"</span>: <span class="val">"OTP nömrənizə göndərildi."</span>, <span class="key">"expires_in"</span>: 300 }

<span class="dim">// ── Addım 2: Yeni PIN set et ─────────────────────</span>
<span class="dim">POST /api/auth/pin/reset</span>
{
  <span class="key">"phone"</span>: <span class="val">"+994501234567"</span>,
  <span class="key">"otp_code"</span>: <span class="val">"123456"</span>,
  <span class="key">"new_pin"</span>: <span class="val">"4321"</span>,
  <span class="key">"new_pin_confirmation"</span>: <span class="val">"4321"</span>
}

<span class="ok">→ HTTP 200 OK</span>
{ <span class="key">"success"</span>: true, <span class="key">"message"</span>: <span class="val">"PIN uğurla yeniləndi."</span> }

<span class="err">→ OTP yanlış / müddəti bitib:</span>
{ <span class="key">"success"</span>: false, <span class="key">"message"</span>: <span class="val">"OTP yanlışdır və ya müddəti bitib."</span> }

<span class="err">→ PIN uyğun deyil:</span>
{ <span class="key">"success"</span>: false, <span class="key">"message"</span>: <span class="val">"PIN-lər uyğun gəlmir."</span> }

<span class="err">→ Köhnə PIN ilə eyni:</span>
{ <span class="key">"success"</span>: false, <span class="key">"message"</span>: <span class="val">"Yeni PIN köhnə ilə eyni ola bilməz."</span> }</div>

        <div class="fix-box blue">
          <strong>📝 Qeydlər:</strong> OTP müddəti <code>5 dəqiqə (300 saniyə)</code>. <code>expires_in</code> sahəsi Flutter countdown timer üçün lazımdır. Yeni PIN köhnə ilə eyni ola bilməz (<code>#3</code> fix ilə uyğun). PIN minimum 4, maksimum 6 rəqəm olmalıdır.
        </div>
      </div>
    </div>
  </div>

  <div class="footer">
    🕸️ Auth Service · 4 Bug · 4 New Endpoint · Backend Team
  </div>

</div>

<script>
function toggle(head) {
  const body = head.nextElementSibling;
  const chev = head.querySelector('.chevron-btn');
  const isOpen = body.classList.contains('open');
  document.querySelectorAll('.card-body.open').forEach(b => b.classList.remove('open'));
  document.querySelectorAll('.chevron-btn.open').forEach(c => c.classList.remove('open'));
  if (!isOpen) { body.classList.add('open'); chev.classList.add('open'); }
}
</script>
</body>
</html>
