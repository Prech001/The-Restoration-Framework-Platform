<html lang="en" dir="ltr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Structural Load — Restoration System</title>
<style>
  :root{
    --paper:#121110;
    --paper-dim:#1C1A17;
    --ink:#EFE7D8;
    --ink-soft:#A79E8E;
    --blueprint:#C6A15B;
    --blueprint-dim:#A8874A;
    --brass:#6E8F7C;
    --rust:#B4585A;
    --sage:#6E8F7C;
    --line:rgba(239,231,216,0.10);
    --surface:rgba(239,231,216,0.035);
    --surface-hover:rgba(239,231,216,0.06);
    --display: "Iowan Old Style", "Palatino Linotype", Georgia, "Times New Roman", serif;
    --body: -apple-system, "Segoe UI", "Helvetica Neue", Arial, sans-serif;
    --mono: "SFMono-Regular", Consolas, "Liberation Mono", Menlo, monospace;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--paper);
    color:var(--ink);
    font-family:var(--body);
    background-image:
      linear-gradient(var(--line) 1px, transparent 1px),
      linear-gradient(90deg, var(--line) 1px, transparent 1px);
    background-size:28px 28px;
    min-height:100vh;
  }
  a{color:var(--blueprint);}

  .shell{max-width:1180px;margin:0 auto;padding:0 24px 80px;}
  header.top{
    display:flex;justify-content:space-between;align-items:flex-end;
    padding:36px 0 18px;border-bottom:2px solid var(--ink);
    flex-wrap:wrap;gap:16px;
  }
  .brandmark{display:flex;flex-direction:column;gap:2px;}
  .eyebrow{font-family:var(--mono);font-size:11px;letter-spacing:.14em;text-transform:uppercase;color:var(--blueprint);}
  h1.title{font-family:var(--display);font-weight:600;font-size:36px;margin:2px 0 0;letter-spacing:-0.015em;color:var(--ink);}
  .io-controls{display:flex;gap:10px;align-items:center;flex-wrap:wrap;}
  button, .btn{
    font-family:var(--mono);font-size:12px;letter-spacing:.04em;text-transform:uppercase;background:transparent;
    border:1.5px solid var(--ink);color:var(--ink);padding:9px 14px;cursor:pointer;border-radius:2px;
    transition:background .15s, color .15s;
  }
  button.primary{background:var(--blueprint);border-color:var(--blueprint);color:#171512;font-weight:600;box-shadow:0 0 0 rgba(198,161,91,0);transition:box-shadow .2s, background .15s;}
  button.primary:hover{background:var(--blueprint-dim);box-shadow:0 0 18px rgba(198,161,91,0.25);}
  button:hover{background:var(--ink);color:var(--paper);}
  button.ghost{border-color:var(--line);color:var(--ink-soft);}
  button.btn-xs{padding:4px 8px;font-size:10px;border-width:1px;}
  button.danger{border-color:var(--rust);color:var(--rust);}
  button.danger:hover{background:var(--rust);color:var(--paper);}
  input[type=file]{display:none;}
  select#lang-select{
    font-family:var(--mono);font-size:12px;background:var(--paper-dim);color:var(--ink);
    border:1.5px solid var(--ink);padding:9px 10px;border-radius:2px;cursor:pointer;
  }

  nav.tabs{display:flex;gap:2px;margin:22px 0 0;flex-wrap:wrap;border-bottom:1px solid var(--line);}
  nav.tabs button.tab{border:none;border-bottom:3px solid transparent;background:none;padding:12px 14px;font-family:var(--mono);font-size:12px;letter-spacing:.06em;text-transform:uppercase;color:var(--ink-soft);}
  nav.tabs button.tab:hover{background:var(--paper-dim);color:var(--ink);}
  nav.tabs button.tab.active{color:var(--ink);border-bottom-color:var(--rust);font-weight:600;}

  section.panel{display:none;padding-top:34px;}
  section.panel.active{display:block;}

  .core-num{font-family:var(--mono);font-size:12px;color:var(--brass);letter-spacing:.1em;}
  h2.h{font-family:var(--display);font-size:26px;margin:4px 0 6px;}
  p.lede{color:var(--ink-soft);max-width:640px;line-height:1.55;margin:0 0 26px;}

  .hero-grid{display:grid;grid-template-columns:1.1fr .9fr;gap:36px;align-items:center;}
  @media(max-width:860px){.hero-grid{grid-template-columns:1fr;}}
  .hero-copy .hook{font-family:var(--display);font-size:28px;line-height:1.3;margin:8px 0 16px;}
  .hero-copy .hook .l2{font-style:normal;color:var(--rust);display:block;margin-top:4px;}
  .metric-row{display:flex;gap:26px;margin-top:22px;flex-wrap:wrap;}
  .metric{border-left:2px solid var(--brass);padding-left:10px;}
  [dir="rtl"] .metric{border-left:none;border-right:2px solid var(--brass);padding-left:0;padding-right:10px;}
  .metric .num{font-family:var(--mono);font-size:22px;}
  .metric .lbl{font-family:var(--mono);font-size:10px;color:var(--ink-soft);text-transform:uppercase;letter-spacing:.08em;}

  form.entry{display:grid;gap:12px;grid-template-columns:repeat(4,1fr);border:1px solid var(--line);padding:18px;background:var(--surface);margin-bottom:18px;}
  @media(max-width:760px){form.entry{grid-template-columns:1fr 1fr;}}
  form.entry label{font-family:var(--mono);font-size:10px;text-transform:uppercase;letter-spacing:.06em;color:var(--ink-soft);display:block;margin-bottom:4px;}
  form.entry input, form.entry select, form.entry textarea{
    width:100%;font-family:var(--body);font-size:13.5px;padding:8px;border:1px solid var(--line);
    background:var(--paper-dim);color:var(--ink);border-radius:2px;
  }
  form.entry .full{grid-column:1/-1;}
  form.entry .submit-row{grid-column:1/-1;display:flex;justify-content:flex-end;gap:8px;}

  .slider-field{grid-column:1/-1;border:1px solid var(--line);padding:12px 14px;border-radius:2px;background:rgba(198,161,91,0.03);}
  .slider-field .row-top{display:flex;justify-content:space-between;align-items:baseline;gap:10px;flex-wrap:wrap;}
  .slider-field label{margin-bottom:0;}
  .slider-readout{font-family:var(--mono);font-size:12px;color:var(--blueprint);}
  .slider-readout b{font-size:16px;}
  input[type=range]{width:100%;margin:8px 0 4px;accent-color:var(--blueprint);}
  .band-desc{font-size:12px;color:var(--ink-soft);line-height:1.4;min-height:16px;}
  details.legend{margin-top:6px;}
  details.legend summary{cursor:pointer;font-family:var(--mono);font-size:10px;color:var(--brass);letter-spacing:.04em;text-transform:uppercase;}
  .legend-table{margin-top:8px;font-size:11.5px;}
  .legend-table .lg-row{display:grid;grid-template-columns:52px 1fr;gap:8px;padding:4px 0;border-bottom:1px solid var(--line);}
  .legend-table .lg-row b{color:var(--blueprint);font-family:var(--mono);}
  .legend-table .lg-row span{color:var(--ink-soft);}

  .result-card{border:1px solid var(--blueprint);background:rgba(198,161,91,0.06);padding:18px;margin:0 0 26px;border-radius:2px;}
  .result-card .rc-head{font-family:var(--mono);font-size:10px;text-transform:uppercase;letter-spacing:.08em;color:var(--blueprint);margin-bottom:10px;}
  .result-card .rc-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:14px;}
  .result-card .rc-item .v{font-family:var(--mono);font-size:20px;}
  .result-card .rc-item .k{font-family:var(--mono);font-size:10px;color:var(--ink-soft);text-transform:uppercase;letter-spacing:.05em;}
  .result-card .rc-item .d{font-size:11.5px;color:var(--ink-soft);margin-top:2px;}
  .result-card .rc-delta{font-family:var(--mono);font-size:11px;margin-top:4px;}
  .delta-up{color:#8AA88C;}
  .delta-down{color:var(--rust);}

  table{width:100%;border-collapse:collapse;font-size:13px;}
  th{font-family:var(--mono);font-size:10px;text-transform:uppercase;letter-spacing:.06em;text-align:left;color:var(--ink-soft);border-bottom:1px solid var(--ink);padding:8px 6px;}
  [dir="rtl"] th{text-align:right;}
  td{padding:9px 6px;border-bottom:1px solid var(--line);vertical-align:top;}
  tr:hover td{background:var(--surface-hover);}
  .tag{font-family:var(--mono);font-size:10px;padding:2px 6px;border:1px solid var(--line);border-radius:10px;color:var(--ink-soft);}
  .empty{border:1.5px dashed var(--line);padding:30px;text-align:center;color:var(--ink-soft);font-family:var(--mono);font-size:12px;letter-spacing:.03em;}
  .row-actions{display:flex;gap:6px;flex-wrap:wrap;}
  .edit-input{width:100%;font-size:12px;padding:5px;background:var(--paper-dim);color:var(--ink);border:1px solid var(--line);border-radius:2px;}

  .doctrine{border-left:3px solid var(--blueprint);padding-left:18px;margin-bottom:26px;}
  [dir="rtl"] .doctrine{border-left:none;border-right:3px solid var(--blueprint);padding-left:0;padding-right:18px;}
  .doctrine h3{font-family:var(--display);font-size:18px;margin:0 0 6px;}
  .doctrine p{font-size:13.5px;line-height:1.6;color:var(--ink-soft);margin:0;}

  .week{display:flex;align-items:flex-start;gap:12px;padding:10px 0;border-bottom:1px solid var(--line);}
  .week input{margin-top:3px;}
  .week .wk{font-family:var(--mono);font-size:11px;color:var(--brass);min-width:58px;}
  .week .txt{font-size:13.5px;}
  .week .txt.done{text-decoration:line-through;color:var(--ink-soft);}
  .month-head{font-family:var(--display);font-size:17px;margin:26px 0 6px;color:var(--blueprint);}

  .gauge-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:18px;margin-bottom:30px;}
  @media(max-width:760px){.gauge-grid{grid-template-columns:1fr;}}
  .gauge{border:1px solid var(--line);padding:18px;text-align:center;background:var(--surface);}
  .gauge .val{font-family:var(--mono);font-size:34px;color:var(--blueprint);}
  .gauge .lbl{font-family:var(--mono);font-size:10px;text-transform:uppercase;letter-spacing:.07em;color:var(--ink-soft);margin-top:4px;}
  .gauge .bar{height:6px;background:var(--line);margin-top:10px;border-radius:3px;overflow:hidden;}
  .gauge .bar i{display:block;height:100%;background:var(--rust);}

  .client-select-bar{display:flex;gap:10px;align-items:center;margin-bottom:18px;flex-wrap:wrap;}
  .client-select-bar select{font-family:var(--body);font-size:13px;padding:8px;background:var(--paper-dim);color:var(--ink);border:1px solid var(--line);border-radius:2px;}
  .client-card{border:1px solid var(--line);background:var(--surface);padding:16px;margin-bottom:14px;border-radius:2px;}
  .client-card .ch{display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:10px;}
  .client-card h3{font-family:var(--display);font-size:18px;margin:0;}
  .client-card .meta{font-family:var(--mono);font-size:11px;color:var(--ink-soft);margin-top:4px;}
  .client-mini{display:flex;gap:18px;margin-top:10px;flex-wrap:wrap;}
  .client-mini .m{font-family:var(--mono);font-size:12px;}
  .client-mini .m .k{color:var(--ink-soft);font-size:10px;text-transform:uppercase;letter-spacing:.05em;display:block;}
  .detail{margin-top:14px;padding-top:14px;border-top:1px solid var(--line);display:none;}
  .detail.open{display:block;}
  .sub-h{font-family:var(--mono);font-size:10px;text-transform:uppercase;letter-spacing:.07em;color:var(--brass);margin:14px 0 6px;}

  footer.note{margin-top:60px;padding-top:16px;border-top:1px solid var(--line);font-family:var(--mono);font-size:11px;color:var(--ink-soft);}

  #print-report{display:none;}
  #print-report h1{font-family:var(--display);font-size:22px;margin:0 0 2px;}
  #print-report .p-sub{font-family:var(--mono);font-size:10px;color:#666;margin-bottom:18px;}
  #print-report .p-client{border:1px solid #ccc;padding:12px;margin-bottom:16px;page-break-inside:avoid;}
  #print-report .p-client h2{font-family:var(--display);font-size:16px;margin:0 0 4px;}
  #print-report .p-meta{font-family:var(--mono);font-size:10px;color:#555;margin-bottom:8px;}
  #print-report table{width:100%;border-collapse:collapse;font-size:11px;margin-bottom:8px;}
  #print-report th,#print-report td{border:1px solid #ddd;padding:5px;text-align:left;}
  #print-report th{background:#f2f2f2;}
  #print-report .p-section-title{font-family:var(--mono);font-size:10px;text-transform:uppercase;letter-spacing:.05em;color:#8C6A2F;margin:10px 0 4px;}
  @media print{
    body{background:#fff;color:#111;}
    .shell > *{display:none !important;}
    #print-report{display:block !important;}
  }
</style>
</head>
<body>
<div class="shell">

  <header class="top">
    <div class="brandmark">
      <span class="eyebrow" data-i18n="app_eyebrow"></span>
      <h1 class="title" data-i18n="app_title"></h1>
    </div>
    <div class="io-controls">
      <select id="lang-select" aria-label="Language"></select>
      <button class="ghost" id="btn-reset-lang" data-i18n="io_reset"></button>
      <button class="ghost" id="btn-export-csv" data-i18n="io_export_csv"></button>
      <button class="ghost" id="btn-export-pdf" data-i18n="io_export_pdf"></button>
      <label class="btn ghost" for="file-import" style="cursor:pointer;" data-i18n="io_import_csv"></label>
      <input type="file" id="file-import" accept=".csv,text/csv,.pdf,application/pdf">
    </div>
  </header>

  <nav class="tabs">
    <button class="tab active" data-panel="dashboard" data-i18n="nav_dashboard"></button>
    <button class="tab" data-panel="clients" data-i18n="nav_clients"></button>
    <button class="tab" data-panel="signal" data-i18n="nav_signal"></button>
    <button class="tab" data-panel="integration" data-i18n="nav_integration"></button>
    <button class="tab" data-panel="framework" data-i18n="nav_framework"></button>
    <button class="tab" data-panel="roadmap" data-i18n="nav_roadmap"></button>
    <button class="tab" data-panel="metrics" data-i18n="nav_metrics"></button>
  </nav>

  <!-- DASHBOARD -->
  <section class="panel active" id="panel-dashboard">
    <div class="hero-grid">
      <div class="hero-copy">
        <span class="eyebrow" data-i18n="dash_eyebrow"></span>
        <div class="hook"><span data-i18n="dash_hook1"></span><span class="l2" data-i18n="dash_hook2"></span></div>
        <p class="lede" data-i18n="dash_lede"></p>
        <div class="metric-row" id="dash-metrics"></div>
      </div>
      <svg viewBox="0 0 360 320" width="100%" style="max-width:420px;justify-self:center;">
        <defs><marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M0,0 L10,5 L0,10 z" fill="#B4585A"/></marker></defs>
        <text x="180" y="20" text-anchor="middle" font-family="SFMono-Regular,Consolas,monospace" font-size="10" fill="#C6A15B" letter-spacing="1" id="svg-crosssection"></text>
        <line x1="180" y1="30" x2="180" y2="60" stroke="#B4585A" stroke-width="2" marker-end="url(#arrow)"/>
        <text x="196" y="45" font-family="SFMono-Regular,monospace" font-size="9" fill="#B4585A" id="svg-demand"></text>
        <rect x="40" y="62" width="280" height="58" fill="none" stroke="#C6A15B" stroke-width="1.5"/>
        <text x="52" y="85" font-family="Georgia,serif" font-size="14" fill="#EFE7D8" id="svg-spirit"></text>
        <text x="52" y="102" font-family="SFMono-Regular,monospace" font-size="9" fill="#A79E8E" id="svg-spirit-sub"></text>
        <rect x="40" y="128" width="280" height="58" fill="none" stroke="#C6A15B" stroke-width="1.5"/>
        <text x="52" y="151" font-family="Georgia,serif" font-size="14" fill="#EFE7D8" id="svg-soul"></text>
        <text x="52" y="168" font-family="SFMono-Regular,monospace" font-size="9" fill="#A79E8E" id="svg-soul-sub"></text>
        <rect x="40" y="194" width="280" height="58" fill="none" stroke="#C6A15B" stroke-width="1.5"/>
        <text x="52" y="217" font-family="Georgia,serif" font-size="14" fill="#EFE7D8" id="svg-body"></text>
        <text x="52" y="234" font-family="SFMono-Regular,monospace" font-size="9" fill="#A79E8E" id="svg-body-sub"></text>
        <line x1="40" y1="252" x2="320" y2="252" stroke="#EFE7D8" stroke-width="2"/>
        <g font-family="SFMono-Regular,monospace" font-size="8" fill="#6E8F7C" text-anchor="middle">
          <line x1="70" y1="252" x2="70" y2="270" stroke="#6E8F7C"/><text x="70" y="282">01</text>
          <line x1="130" y1="252" x2="130" y2="270" stroke="#6E8F7C"/><text x="130" y="282">02</text>
          <line x1="180" y1="252" x2="180" y2="270" stroke="#6E8F7C"/><text x="180" y="282">03</text>
          <line x1="230" y1="252" x2="230" y2="270" stroke="#6E8F7C"/><text x="230" y="282">04</text>
          <line x1="290" y1="252" x2="290" y2="270" stroke="#6E8F7C"/><text x="290" y="282">05</text>
        </g>
        <text x="180" y="304" text-anchor="middle" font-family="SFMono-Regular,monospace" font-size="9" fill="#A79E8E" id="svg-support"></text>
      </svg>
    </div>
  </section>

  <!-- CLIENTS -->
  <section class="panel" id="panel-clients">
    <span class="core-num" data-i18n="clients_corenum"></span>
    <h2 class="h" data-i18n="clients_h2"></h2>
    <p class="lede" data-i18n="clients_lede"></p>

    <form class="entry" id="form-client">
      <div><label data-i18n="clients_f_name"></label><input type="text" id="c-name" required></div>
      <div><label data-i18n="clients_f_contact"></label><input type="text" id="c-contact"></div>
      <div><label data-i18n="clients_f_start"></label><input type="date" id="c-start" required></div>
      <div><label data-i18n="clients_f_status"></label>
        <select id="c-status">
          <option value="Active" data-i18n="status_active"></option>
          <option value="Paused" data-i18n="status_paused"></option>
          <option value="Completed" data-i18n="status_completed"></option>
        </select>
      </div>
      <div class="full"><label data-i18n="clients_f_note"></label><textarea id="c-note" rows="2" data-i18n-ph="clients_f_noteph"></textarea></div>
      <div class="submit-row"><button type="submit" class="primary" data-i18n="clients_submit"></button></div>
    </form>

    <div id="clients-wrap"></div>
  </section>

  <!-- SIGNAL LOG -->
  <section class="panel" id="panel-signal">
    <span class="core-num" data-i18n="signal_corenum"></span>
    <h2 class="h" data-i18n="signal_h2"></h2>
    <p class="lede" data-i18n="signal_lede"></p>

    <form class="entry" id="form-signal">
      <div><label data-i18n="label_client"></label><select id="s-client" required></select></div>
      <div><label data-i18n="label_date"></label><input type="date" id="s-date" required></div>
      <div><label data-i18n="signal_f_symptom"></label><input type="text" id="s-symptom" data-i18n-ph="signal_f_symptomph" required></div>
      <div><label data-i18n="signal_f_category"></label>
        <select id="s-cat">
          <option value="Fatigue" data-i18n="cat_fatigue"></option>
          <option value="Hormonal / Cycle" data-i18n="cat_hormonal"></option>
          <option value="Cognitive" data-i18n="cat_cognitive"></option>
          <option value="Spiritual drought" data-i18n="cat_spiritual"></option>
          <option value="Overcommitment" data-i18n="cat_overcommit"></option>
          <option value="Other" data-i18n="cat_other"></option>
        </select>
      </div>
      <div class="slider-field full">
        <div class="row-top"><label data-i18n="signal_f_load"></label><div class="slider-readout"><span data-i18n="value_label"></span> <b id="s-load-val">5</b>/10</div></div>
        <input type="range" id="s-load" min="1" max="10" value="5" step="1">
        <div class="band-desc" id="s-load-desc"></div>
        <details class="legend"><summary data-i18n="legend_toggle"></summary><div class="legend-table" id="s-load-legend"></div></details>
      </div>
      <div class="full"><label data-i18n="signal_f_note"></label><textarea id="s-note" rows="2"></textarea></div>
      <div class="submit-row">
        <button type="button" class="ghost" id="s-cancel-edit" style="display:none;" data-i18n="btn_cancel_edit"></button>
        <button type="submit" class="primary" id="s-submit-btn" data-i18n="btn_log_signal"></button>
      </div>
    </form>

    <div id="signal-result"></div>
    <div class="client-select-bar">
      <label style="font-family:var(--mono);font-size:10px;text-transform:uppercase;color:var(--ink-soft);" data-i18n="filter_by_client"></label>
      <select id="s-filter"></select>
    </div>
    <div id="signal-table-wrap"></div>
  </section>

  <!-- INTEGRATION -->
  <section class="panel" id="panel-integration">
    <span class="core-num" data-i18n="integration_corenum"></span>
    <h2 class="h" data-i18n="integration_h2"></h2>
    <p class="lede" data-i18n="integration_lede"></p>

    <form class="entry" id="form-integration">
      <div><label data-i18n="label_client"></label><select id="i-client" required></select></div>
      <div><label data-i18n="label_date"></label><input type="date" id="i-date" required></div>
      <div class="full"><label data-i18n="integration_f_note"></label><textarea id="i-note" rows="2" data-i18n-ph="integration_f_noteph"></textarea></div>

      <div class="slider-field full">
        <div class="row-top"><label data-i18n="label_spirit"></label><div class="slider-readout"><span data-i18n="value_label"></span> <b id="i-spirit-val">5</b>/10</div></div>
        <input type="range" id="i-spirit" min="1" max="10" value="5" step="1">
        <div class="band-desc" id="i-spirit-desc"></div>
        <details class="legend"><summary data-i18n="legend_toggle"></summary><div class="legend-table" id="i-spirit-legend"></div></details>
      </div>
      <div class="slider-field full">
        <div class="row-top"><label data-i18n="label_soul"></label><div class="slider-readout"><span data-i18n="value_label"></span> <b id="i-soul-val">5</b>/10</div></div>
        <input type="range" id="i-soul" min="1" max="10" value="5" step="1">
        <div class="band-desc" id="i-soul-desc"></div>
        <details class="legend"><summary data-i18n="legend_toggle"></summary><div class="legend-table" id="i-soul-legend"></div></details>
      </div>
      <div class="slider-field full">
        <div class="row-top"><label data-i18n="label_body"></label><div class="slider-readout"><span data-i18n="value_label"></span> <b id="i-body-val">5</b>/10</div></div>
        <input type="range" id="i-body" min="1" max="10" value="5" step="1">
        <div class="band-desc" id="i-body-desc"></div>
        <details class="legend"><summary data-i18n="legend_toggle"></summary><div class="legend-table" id="i-body-legend"></div></details>
      </div>

      <div class="submit-row">
        <button type="button" class="ghost" id="i-cancel-edit" style="display:none;" data-i18n="btn_cancel_edit"></button>
        <button type="submit" class="primary" id="i-submit-btn" data-i18n="btn_log_checkin"></button>
      </div>
    </form>

    <div id="integration-result"></div>
    <div class="client-select-bar">
      <label style="font-family:var(--mono);font-size:10px;text-transform:uppercase;color:var(--ink-soft);" data-i18n="filter_by_client"></label>
      <select id="i-filter"></select>
    </div>
    <div id="integration-table-wrap"></div>
  </section>

  <!-- FRAMEWORK -->
  <section class="panel" id="panel-framework">
    <span class="core-num" data-i18n="framework_corenum"></span>
    <h2 class="h" data-i18n="framework_h2"></h2>
    <p class="lede" data-i18n="framework_lede"></p>
    <div class="doctrine"><h3 data-i18n="f1_title"></h3><p data-i18n="f1_desc"></p></div>
    <div class="doctrine"><h3 data-i18n="f2_title"></h3><p data-i18n="f2_desc"></p></div>
    <div class="doctrine"><h3 data-i18n="f3_title"></h3><p data-i18n="f3_desc"></p></div>
    <div class="doctrine"><h3 data-i18n="f4_title"></h3><p data-i18n="f4_desc"></p></div>
    <div class="doctrine"><h3 data-i18n="f5_title"></h3><p data-i18n="f5_desc"></p></div>
  </section>

  <!-- ROADMAP -->
  <section class="panel" id="panel-roadmap">
    <span class="core-num" data-i18n="roadmap_corenum"></span>
    <h2 class="h" data-i18n="roadmap_h2"></h2>
    <p class="lede" data-i18n="roadmap_lede"></p>
    <div id="roadmap-wrap"></div>
  </section>

  <!-- METRICS -->
  <section class="panel" id="panel-metrics">
    <span class="core-num" data-i18n="metrics_corenum"></span>
    <h2 class="h" data-i18n="metrics_h2"></h2>
    <p class="lede" data-i18n="metrics_lede"></p>
    <div class="gauge-grid" id="gauge-wrap"></div>
    <div class="empty" id="metrics-empty" style="display:none;" data-i18n="metrics_empty"></div>
  </section>

  <footer class="note" data-i18n="footer_note"></footer>

  <div id="print-report"></div>
</div>

<script>
(function(){
  "use strict";

  const state = { clients: [], signals: [], integrations: [], roadmap: [] };
  let editingSignalId = null;
  let editingIntegrationId = null;
  let currentLang = 'en';
  const RTL_LANGS = ['ar'];
  const LANGS = [
    {code:'en',name:'English'},
    {code:'es',name:'Español'},
    {code:'fr',name:'Français'},
    {code:'pt',name:'Português'},
    {code:'de',name:'Deutsch'},
    {code:'ar',name:'العربية'},
    {code:'zh',name:'中文'},
    {code:'hi',name:'हिन्दी'},
    {code:'ru',name:'Русский'},
    {code:'ja',name:'日本語'}
  ];

  const uid = () => Date.now().toString(36)+Math.random().toString(36).slice(2,7);

  const T = {
en:{app_eyebrow:"Restoration Framework · Offline System",app_title:"Structural Load",io_export_csv:"Export CSV",io_export_pdf:"Export PDF",io_import_csv:"Import CSV",io_lang_label:"Language",io_reset:"Reset to English",
nav_dashboard:"Dashboard",nav_clients:"Clients",nav_signal:"Signal Log",nav_integration:"Integration",nav_framework:"Framework",nav_roadmap:"Program Roadmap",nav_metrics:"Structural Metrics",
dash_eyebrow:"Working thesis",dash_hook1:"Fatigue is not proof the body is broken.",dash_hook2:"It's proof the load exceeds the design.",dash_lede:"This system registers each woman as a client record, logs symptoms as structural signal against a defined scale, tracks spirit–soul–body as one system, and returns a result readout after every entry.",
dash_m_clients:"Clients registered",dash_m_signals:"Signals logged",dash_m_avgload:"Avg structural load",dash_m_avgspirit:"Avg spirit score",
svg_crosssection:"CROSS-SECTION · LOAD PATH",svg_demand:"demand",svg_spirit:"Spirit",svg_spirit_sub:"identity · design · purpose",svg_soul:"Soul",svg_soul_sub:"mind · emotion · will",svg_body:"Body",svg_body_sub:"hormones · cycle · nervous system",svg_support:"FIVE CORES = LOAD-BEARING SUPPORT",
clients_corenum:"CLIENT REGISTRY",clients_h2:"Register & Manage Clients",clients_lede:"Every woman attended to is registered once here, then selected from a dropdown when logging signals or check-ins. Records are fully editable — update details or correct history at any time.",
clients_f_name:"Full name",clients_f_contact:"Contact (email / phone)",clients_f_start:"Start date",clients_f_status:"Status",clients_f_note:"Intake note",clients_f_noteph:"Context at intake — what brought her in",clients_submit:"Register Client",
status_active:"Active",status_paused:"Paused",status_completed:"Completed",clients_nocontact:"no contact on file",clients_started:"Started",clients_edit:"Edit",clients_view:"View Records",clients_delete:"Delete",
mini_signals:"Signals",mini_checkins:"Check-ins",mini_avgload:"Avg Load",mini_avgspirit:"Avg Spirit",mini_avgsoul:"Avg Soul",mini_avgbody:"Avg Body",clients_intake_prefix:"Intake note:",save:"Save",cancel:"Cancel",
confirm_delete_client:"Delete this client and detach their logged entries? Entries remain but will show as unlinked.",clients_empty:"No clients registered yet. Use the form above to add the first record.",
detail_signal_history:"Signal History",detail_integration_history:"Integration History",detail_no_signals:"No signals logged for this client.",detail_no_integrations:"No check-ins logged for this client.",
row_edit:"Edit",row_del:"Del",confirm_delete_signal:"Delete this signal entry?",confirm_delete_integration:"Delete this check-in entry?",no_clients_registered:"No clients registered yet",
signal_corenum:"CORE 01 + 05",signal_h2:"Signal Log",signal_lede:"Every symptom entry is scored on structural load — how much this is the body signaling that demand exceeds design capacity, not simply malfunctioning.",
label_client:"Client",label_date:"Date",signal_f_symptom:"Symptom / signal",signal_f_symptomph:"e.g. brain fog, cycle irregularity",signal_f_category:"Category",
cat_fatigue:"Fatigue",cat_hormonal:"Hormonal / Cycle",cat_cognitive:"Cognitive",cat_spiritual:"Spiritual drought",cat_overcommit:"Overcommitment",cat_other:"Other",
signal_f_load:"Structural load",value_label:"Value:",legend_toggle:"What does each range mean?",signal_f_note:"Note (what was being carried that week)",
btn_cancel_edit:"Cancel Edit",btn_log_signal:"Log Signal",btn_update_signal:"Update Signal",filter_by_client:"Filter by client",filter_all_clients:"All clients",
result_head:"Result",result_structural_load:"Structural Load",result_delta_vs_avg:"vs. this client's average",result_baseline_first_signal:"First signal on record for this client — establishing baseline.",
table_client:"Client",table_date:"Date",table_signal:"Signal",table_category:"Category",table_load:"Load",signal_empty:"No signals logged yet.",alert_select_client:"Register and select a client first.",
integration_corenum:"CORE 02",integration_h2:"Integration Check-in",integration_lede:"Spirit, soul, and body scored together, against a defined scale each time — never separately, because the framework treats them as one system.",
integration_f_note:"Note",integration_f_noteph:"What moved, and what didn't",label_spirit:"Spirit",label_soul:"Soul",label_body:"Body",btn_log_checkin:"Log Check-in",btn_update_checkin:"Update Check-in",
result_baseline_entry:"Baseline entry",table_spirit:"Spirit",table_soul:"Soul",table_body:"Body",integration_empty:"No check-ins yet.",
framework_corenum:"CORE 03",framework_h2:"Framework Reference",framework_lede:"The fixed doctrine the rest of the system operates on.",
f1_title:"01 · Reframe of Diagnosis",f1_desc:"Symptoms are read as signal, not verdict. Fatigue, hormonal disruption, and brain fog indicate structural overload before they indicate biochemical failure.",
f2_title:"02 · Integration Principle",f2_desc:"Spirit, soul, and body are treated as one continuously interacting system. No intervention isolates a single layer.",
f3_title:"03 · Methodological Synthesis",f3_desc:"Hormone physiology and cycle literacy are combined with a biblical framing of identity and design. The combination is the mechanism of change.",
f4_title:"04 · Delivery Architecture",f4_desc:"Three coordinated formats: a four-month 1:1 program, a written doctrine (book), and collective formation through content and live events.",
f5_title:"05 · Structural / Societal Vision",f5_desc:"Widespread female exhaustion is treated as a structural phenomenon — demand exceeding design at scale — not an epidemic of individually broken hormones.",
roadmap_corenum:"CORE 04",roadmap_h2:"Program Roadmap",roadmap_lede:"Four months, sixteen weeks — the shared template underlying the 1:1 program.",
month1:"Month 1 — Assessment & Groundwork",month2:"Month 2 — Realignment",month3:"Month 3 — Integration in Practice",month4:"Month 4 — Consolidation",
m1w1:"Baseline structural load + cycle history intake",m1w2:"Spirit-soul-body baseline scoring",m1w3:"Map current commitments against actual capacity",m1w4:"Establish tracking rhythm (Signal Log + Integration)",
m2w1:"Identify top 3 structural overloads to remove or restructure",m2w2:"Introduce cycle-informed lifestyle adjustments",m2w3:"Begin identity/design study (spirit layer)",m2w4:"Mid-point load reassessment",
m3w1:"Apply adjustments under real-world load (work, family)",m3w2:"Track hormonal response to reduced structural load",m3w3:"Deepen spiritual practice tied to identity work",m3w4:"Address relapse points where old load creeps back",
m4w1:"Full spirit-soul-body reassessment vs. baseline",m4w2:"Build a sustainable, evergreen load ceiling",m4w3:"Define ongoing rhythm beyond the program",m4w4:"Exit review and forward plan",week_label:"Wk",
metrics_corenum:"CORE 05",metrics_h2:"Structural Metrics",metrics_lede:"Aggregated across every registered client — the structural, societal-scale readout.",
gauge_load:"Structural Load Index (÷10)",gauge_integration:"Spirit–Soul–Body Integration (÷10)",gauge_progress:"Program Progress",metrics_empty:"No data logged yet. Register a client and add entries to populate these metrics.",
footer_note:"Offline system. No network calls, no external fonts or scripts. Data lives in memory for this session only — use Export before closing, and Import to resume. Scale values are self-report reference points, not diagnostic instruments.",
print_title:"Structural Load — Client Report",print_generated:"Generated",print_noclients:"No clients registered yet.",print_signal_section:"Signal Log",print_integration_section:"Integration Check-ins",
print_no_signals:"No signals logged.",print_no_checkins:"No check-ins logged.",print_roadmap_section:"Program Roadmap Progress",print_roadmap_text:"of the 16-week roadmap complete (shared template).",
alert_csv_bad:"Could not read that file. Expecting a CSV exported from this system.",alert_pdf_import:"PDF files can't be imported. A PDF is a formatted report, not structured data — there is no reliable offline way to read client records back out of it. Use \u201cExport CSV\u201d / \u201cImport CSV\u201d to move data between sessions, and PDF only for printing or sharing a report.",
sc_load_0_t:"Minimal",sc_load_0_d:"Isolated occurrence, no real strain on capacity.",sc_load_1_t:"Mild",sc_load_1_d:"Noticeable but manageable; capacity slightly tested.",sc_load_2_t:"Moderate",sc_load_2_d:"Recurring; demand is clearly meeting or exceeding capacity.",sc_load_3_t:"High",sc_load_3_d:"Frequent and disruptive; load exceeds design capacity most days.",sc_load_4_t:"Severe",sc_load_4_d:"Constant and systemic; body is signaling urgent overload.",
sc_spirit_0_t:"Disconnected",sc_spirit_0_d:"No sense of God's presence; faith feels absent or distant.",sc_spirit_1_t:"Crowded out",sc_spirit_1_d:"Aware of the desire for God but no real space is being made.",sc_spirit_2_t:"Intermittent",sc_spirit_2_d:"Some connection, inconsistent practice, easily displaced.",sc_spirit_3_t:"Engaged",sc_spirit_3_d:"Regular practice; identity in God is becoming clearer.",sc_spirit_4_t:"Rooted",sc_spirit_4_d:"Steady communion; identity secure regardless of circumstance.",
sc_soul_0_t:"Depleted",sc_soul_0_d:"Overwhelmed, reactive, no emotional or mental bandwidth left.",sc_soul_1_t:"Strained",sc_soul_1_d:"Irritable, foggy, running on reserves.",sc_soul_2_t:"Coping",sc_soul_2_d:"Functional but tired; managing rather than thriving.",sc_soul_3_t:"Clear",sc_soul_3_d:"Emotionally regulated, mentally present, will engaged.",sc_soul_4_t:"Whole",sc_soul_4_d:"Calm, resilient, mind/emotion/will working in alignment.",
sc_body_0_t:"Crisis",sc_body_0_d:"Severe symptoms; body in active distress.",sc_body_1_t:"Struggling",sc_body_1_d:"Frequent symptoms, low energy, poor cycle/hormonal signs.",sc_body_2_t:"Fluctuating",sc_body_2_d:"Mixed days; some stability, some disruption.",sc_body_3_t:"Stabilizing",sc_body_3_d:"Mostly consistent; symptoms present but manageable.",sc_body_4_t:"Thriving",sc_body_4_d:"Strong energy, balanced cycle/hormones, minimal symptoms."
},
es:{app_eyebrow:"Marco de Restauración · Sistema Sin Conexión",app_title:"Carga Estructural",io_export_csv:"Exportar CSV",io_export_pdf:"Exportar PDF",io_import_csv:"Importar CSV",io_lang_label:"Idioma",io_reset:"Restablecer a Inglés",
nav_dashboard:"Panel",nav_clients:"Clientas",nav_signal:"Registro de Señales",nav_integration:"Integración",nav_framework:"Marco Teórico",nav_roadmap:"Hoja de Ruta",nav_metrics:"Métricas Estructurales",
dash_eyebrow:"Tesis central",dash_hook1:"La fatiga no es prueba de que el cuerpo esté roto.",dash_hook2:"Es prueba de que la carga supera el diseño.",dash_lede:"Este sistema registra a cada mujer como una ficha de clienta, anota los síntomas como señal estructural frente a una escala definida, sigue el espíritu, alma y cuerpo como un solo sistema, y devuelve un resultado tras cada entrada.",
dash_m_clients:"Clientas registradas",dash_m_signals:"Señales registradas",dash_m_avgload:"Carga estructural media",dash_m_avgspirit:"Puntuación media de espíritu",
svg_crosssection:"CORTE TRANSVERSAL · TRAYECTO DE CARGA",svg_demand:"demanda",svg_spirit:"Espíritu",svg_spirit_sub:"identidad · diseño · propósito",svg_soul:"Alma",svg_soul_sub:"mente · emoción · voluntad",svg_body:"Cuerpo",svg_body_sub:"hormonas · ciclo · sistema nervioso",svg_support:"CINCO PILARES = SOPORTE DE CARGA",
clients_corenum:"REGISTRO DE CLIENTAS",clients_h2:"Registrar y Gestionar Clientas",clients_lede:"Cada mujer atendida se registra una vez aquí y luego se selecciona desde un menú al registrar señales o controles. Los registros son totalmente editables — actualiza datos o corrige el historial en cualquier momento.",
clients_f_name:"Nombre completo",clients_f_contact:"Contacto (correo / teléfono)",clients_f_start:"Fecha de inicio",clients_f_status:"Estado",clients_f_note:"Nota de admisión",clients_f_noteph:"Contexto de admisión — qué la trajo aquí",clients_submit:"Registrar Clienta",
status_active:"Activa",status_paused:"En pausa",status_completed:"Completada",clients_nocontact:"sin contacto registrado",clients_started:"Inicio",clients_edit:"Editar",clients_view:"Ver Registros",clients_delete:"Eliminar",
mini_signals:"Señales",mini_checkins:"Controles",mini_avgload:"Carga Media",mini_avgspirit:"Espíritu Medio",mini_avgsoul:"Alma Media",mini_avgbody:"Cuerpo Medio",clients_intake_prefix:"Nota de admisión:",save:"Guardar",cancel:"Cancelar",
confirm_delete_client:"¿Eliminar esta clienta y desvincular sus entradas registradas? Las entradas permanecerán pero aparecerán sin vincular.",clients_empty:"Aún no hay clientas registradas. Usa el formulario de arriba para añadir el primer registro.",
detail_signal_history:"Historial de Señales",detail_integration_history:"Historial de Integración",detail_no_signals:"No hay señales registradas para esta clienta.",detail_no_integrations:"No hay controles registrados para esta clienta.",
row_edit:"Editar",row_del:"Elim.",confirm_delete_signal:"¿Eliminar esta señal?",confirm_delete_integration:"¿Eliminar este control?",no_clients_registered:"Aún no hay clientas registradas",
signal_corenum:"PILAR 01 + 05",signal_h2:"Registro de Señales",signal_lede:"Cada síntoma se puntúa según la carga estructural — cuánto indica que el cuerpo está señalando que la demanda supera la capacidad de diseño, y no simplemente un fallo.",
label_client:"Clienta",label_date:"Fecha",signal_f_symptom:"Síntoma / señal",signal_f_symptomph:"p. ej. niebla mental, ciclo irregular",signal_f_category:"Categoría",
cat_fatigue:"Fatiga",cat_hormonal:"Hormonal / Ciclo",cat_cognitive:"Cognitivo",cat_spiritual:"Sequía espiritual",cat_overcommit:"Sobrecarga de compromisos",cat_other:"Otro",
signal_f_load:"Carga estructural",value_label:"Valor:",legend_toggle:"¿Qué significa cada rango?",signal_f_note:"Nota (qué estaba cargando esa semana)",
btn_cancel_edit:"Cancelar Edición",btn_log_signal:"Registrar Señal",btn_update_signal:"Actualizar Señal",filter_by_client:"Filtrar por clienta",filter_all_clients:"Todas las clientas",
result_head:"Resultado",result_structural_load:"Carga Estructural",result_delta_vs_avg:"frente al promedio de esta clienta",result_baseline_first_signal:"Primera señal registrada para esta clienta — estableciendo línea base.",
table_client:"Clienta",table_date:"Fecha",table_signal:"Señal",table_category:"Categoría",table_load:"Carga",signal_empty:"Aún no hay señales registradas.",alert_select_client:"Registra y selecciona primero una clienta.",
integration_corenum:"PILAR 02",integration_h2:"Control de Integración",integration_lede:"Espíritu, alma y cuerpo se puntúan juntos, según una escala definida cada vez — nunca por separado, porque el marco los trata como un solo sistema.",
integration_f_note:"Nota",integration_f_noteph:"Qué cambió y qué no",label_spirit:"Espíritu",label_soul:"Alma",label_body:"Cuerpo",btn_log_checkin:"Registrar Control",btn_update_checkin:"Actualizar Control",
result_baseline_entry:"Entrada de línea base",table_spirit:"Espíritu",table_soul:"Alma",table_body:"Cuerpo",integration_empty:"Aún no hay controles registrados.",
framework_corenum:"PILAR 03",framework_h2:"Marco de Referencia",framework_lede:"La doctrina fija sobre la que opera el resto del sistema.",
f1_title:"01 · Redefinición del Diagnóstico",f1_desc:"Los síntomas se leen como señal, no como veredicto. La fatiga, la alteración hormonal y la niebla mental indican sobrecarga estructural antes que fallo bioquímico.",
f2_title:"02 · Principio de Integración",f2_desc:"Espíritu, alma y cuerpo se tratan como un solo sistema en interacción continua. Ninguna intervención aísla una sola capa.",
f3_title:"03 · Síntesis Metodológica",f3_desc:"El conocimiento hormonal y del ciclo se combina con una visión bíblica de la identidad y el diseño. Esa combinación es el mecanismo del cambio.",
f4_title:"04 · Arquitectura de Entrega",f4_desc:"Tres formatos coordinados: un programa individual de cuatro meses, una doctrina escrita (libro) y una formación colectiva mediante contenido y eventos en vivo.",
f5_title:"05 · Visión Estructural / Social",f5_desc:"El agotamiento femenino generalizado se trata como un fenómeno estructural — la demanda superando el diseño a escala — no como una epidemia de hormonas individualmente rotas.",
roadmap_corenum:"PILAR 04",roadmap_h2:"Hoja de Ruta del Programa",roadmap_lede:"Cuatro meses, dieciséis semanas — la plantilla compartida que sustenta el programa individual.",
month1:"Mes 1 — Evaluación y Bases",month2:"Mes 2 — Realineación",month3:"Mes 3 — Integración en la Práctica",month4:"Mes 4 — Consolidación",
m1w1:"Admisión de carga estructural base + historial de ciclo",m1w2:"Puntuación base de espíritu-alma-cuerpo",m1w3:"Mapear compromisos actuales frente a la capacidad real",m1w4:"Establecer ritmo de seguimiento (Registro de Señales + Integración)",
m2w1:"Identificar las 3 principales sobrecargas estructurales a eliminar o reestructurar",m2w2:"Introducir ajustes de estilo de vida según el ciclo",m2w3:"Comenzar el estudio de identidad/diseño (capa espiritual)",m2w4:"Reevaluación de carga a mitad de programa",
m3w1:"Aplicar ajustes bajo carga real (trabajo, familia)",m3w2:"Seguir la respuesta hormonal a la carga estructural reducida",m3w3:"Profundizar la práctica espiritual ligada al trabajo de identidad",m3w4:"Abordar los puntos de recaída donde vuelve la vieja carga",
m4w1:"Reevaluación completa de espíritu-alma-cuerpo frente a la línea base",m4w2:"Construir un techo de carga sostenible y perdurable",m4w3:"Definir el ritmo continuo más allá del programa",m4w4:"Revisión de salida y plan a futuro",week_label:"Sem",
metrics_corenum:"PILAR 05",metrics_h2:"Métricas Estructurales",metrics_lede:"Agregadas de todas las clientas registradas — el resultado a escala estructural y social.",
gauge_load:"Índice de Carga Estructural (÷10)",gauge_integration:"Integración Espíritu-Alma-Cuerpo (÷10)",gauge_progress:"Progreso del Programa",metrics_empty:"Aún no hay datos registrados. Registra una clienta y añade entradas para completar estas métricas.",
footer_note:"Sistema sin conexión. Sin llamadas de red, sin fuentes ni scripts externos. Los datos viven en memoria solo durante esta sesión — usa Exportar antes de cerrar, e Importar para continuar. Los valores de escala son puntos de autoevaluación, no instrumentos diagnósticos.",
print_title:"Carga Estructural — Informe de Clientas",print_generated:"Generado",print_noclients:"Aún no hay clientas registradas.",print_signal_section:"Registro de Señales",print_integration_section:"Controles de Integración",
print_no_signals:"No hay señales registradas.",print_no_checkins:"No hay controles registrados.",print_roadmap_section:"Progreso de la Hoja de Ruta",print_roadmap_text:"de la hoja de ruta de 16 semanas completado (plantilla compartida).",
alert_csv_bad:"No se pudo leer ese archivo. Se espera un CSV exportado desde este sistema.",alert_pdf_import:"Los archivos PDF no se pueden importar. Un PDF es un informe con formato, no datos estructurados — no hay forma fiable de extraer registros de clientas sin conexión. Usa \u201cExportar CSV\u201d / \u201cImportar CSV\u201d para mover datos entre sesiones, y el PDF solo para imprimir o compartir un informe.",
sc_load_0_t:"Mínima",sc_load_0_d:"Episodio aislado, sin presión real sobre la capacidad.",sc_load_1_t:"Leve",sc_load_1_d:"Perceptible pero manejable; capacidad ligeramente puesta a prueba.",sc_load_2_t:"Moderada",sc_load_2_d:"Recurrente; la demanda claramente iguala o supera la capacidad.",sc_load_3_t:"Alta",sc_load_3_d:"Frecuente y disruptiva; la carga supera la capacidad de diseño la mayoría de los días.",sc_load_4_t:"Severa",sc_load_4_d:"Constante y sistémica; el cuerpo señala sobrecarga urgente.",
sc_spirit_0_t:"Desconectada",sc_spirit_0_d:"Sin sentido de la presencia de Dios; la fe se siente ausente o distante.",sc_spirit_1_t:"Desplazada",sc_spirit_1_d:"Consciente del deseo de Dios pero sin espacio real para ello.",sc_spirit_2_t:"Intermitente",sc_spirit_2_d:"Algo de conexión, práctica inconsistente, fácilmente desplazada.",sc_spirit_3_t:"Comprometida",sc_spirit_3_d:"Práctica regular; la identidad en Dios se vuelve más clara.",sc_spirit_4_t:"Arraigada",sc_spirit_4_d:"Comunión estable; identidad segura sin importar las circunstancias.",
sc_soul_0_t:"Agotada",sc_soul_0_d:"Abrumada, reactiva, sin margen emocional ni mental.",sc_soul_1_t:"Tensionada",sc_soul_1_d:"Irritable, con niebla mental, funcionando con reservas.",sc_soul_2_t:"Sobrellevando",sc_soul_2_d:"Funcional pero cansada; gestionando más que prosperando.",sc_soul_3_t:"Clara",sc_soul_3_d:"Regulada emocionalmente, presente mentalmente, voluntad activa.",sc_soul_4_t:"Íntegra",sc_soul_4_d:"Calmada, resiliente, mente/emoción/voluntad en alineación.",
sc_body_0_t:"Crisis",sc_body_0_d:"Síntomas graves; cuerpo en angustia activa.",sc_body_1_t:"Luchando",sc_body_1_d:"Síntomas frecuentes, poca energía, señales hormonales/de ciclo deficientes.",sc_body_2_t:"Fluctuante",sc_body_2_d:"Días mixtos; algo de estabilidad, algo de alteración.",sc_body_3_t:"Estabilizando",sc_body_3_d:"Mayormente constante; síntomas presentes pero manejables.",sc_body_4_t:"Floreciente",sc_body_4_d:"Energía fuerte, ciclo/hormonas equilibradas, síntomas mínimos."
},
fr:{app_eyebrow:"Cadre de Restauration · Système Hors Ligne",app_title:"Charge Structurelle",io_export_csv:"Exporter CSV",io_export_pdf:"Exporter PDF",io_import_csv:"Importer CSV",io_lang_label:"Langue",io_reset:"Réinitialiser en Anglais",
nav_dashboard:"Tableau de Bord",nav_clients:"Clientes",nav_signal:"Journal des Signaux",nav_integration:"Intégration",nav_framework:"Cadre Théorique",nav_roadmap:"Feuille de Route",nav_metrics:"Métriques Structurelles",
dash_eyebrow:"Thèse centrale",dash_hook1:"La fatigue n'est pas la preuve que le corps est cassé.",dash_hook2:"C'est la preuve que la charge dépasse la conception.",dash_lede:"Ce système enregistre chaque femme comme une fiche cliente, consigne les symptômes comme signal structurel selon une échelle définie, suit l'esprit, l'âme et le corps comme un seul système, et restitue un résultat après chaque saisie.",
dash_m_clients:"Clientes enregistrées",dash_m_signals:"Signaux enregistrés",dash_m_avgload:"Charge structurelle moyenne",dash_m_avgspirit:"Score d'esprit moyen",
svg_crosssection:"COUPE TRANSVERSALE · TRAJET DE CHARGE",svg_demand:"demande",svg_spirit:"Esprit",svg_spirit_sub:"identité · dessein · but",svg_soul:"Âme",svg_soul_sub:"esprit · émotion · volonté",svg_body:"Corps",svg_body_sub:"hormones · cycle · système nerveux",svg_support:"CINQ PILIERS = SOUTIEN DE CHARGE",
clients_corenum:"REGISTRE DES CLIENTES",clients_h2:"Enregistrer et Gérer les Clientes",clients_lede:"Chaque femme suivie est enregistrée une fois ici, puis sélectionnée dans une liste déroulante lors de la saisie des signaux ou des points de contrôle. Les fiches sont entièrement modifiables — mettez à jour les données ou corrigez l'historique à tout moment.",
clients_f_name:"Nom complet",clients_f_contact:"Contact (e-mail / téléphone)",clients_f_start:"Date de début",clients_f_status:"Statut",clients_f_note:"Note d'admission",clients_f_noteph:"Contexte à l'admission — ce qui l'a amenée",clients_submit:"Enregistrer la Cliente",
status_active:"Active",status_paused:"En pause",status_completed:"Terminée",clients_nocontact:"aucun contact enregistré",clients_started:"Débuté",clients_edit:"Modifier",clients_view:"Voir les Dossiers",clients_delete:"Supprimer",
mini_signals:"Signaux",mini_checkins:"Points de contrôle",mini_avgload:"Charge Moy.",mini_avgspirit:"Esprit Moy.",mini_avgsoul:"Âme Moy.",mini_avgbody:"Corps Moy.",clients_intake_prefix:"Note d'admission :",save:"Enregistrer",cancel:"Annuler",
confirm_delete_client:"Supprimer cette cliente et détacher ses entrées enregistrées ? Les entrées resteront mais apparaîtront non liées.",clients_empty:"Aucune cliente enregistrée pour l'instant. Utilisez le formulaire ci-dessus pour ajouter la première fiche.",
detail_signal_history:"Historique des Signaux",detail_integration_history:"Historique d'Intégration",detail_no_signals:"Aucun signal enregistré pour cette cliente.",detail_no_integrations:"Aucun point de contrôle enregistré pour cette cliente.",
row_edit:"Modifier",row_del:"Suppr.",confirm_delete_signal:"Supprimer ce signal ?",confirm_delete_integration:"Supprimer ce point de contrôle ?",no_clients_registered:"Aucune cliente enregistrée pour l'instant",
signal_corenum:"PILIER 01 + 05",signal_h2:"Journal des Signaux",signal_lede:"Chaque symptôme est noté selon la charge structurelle — dans quelle mesure le corps signale que la demande dépasse la capacité de conception, plutôt qu'un simple dysfonctionnement.",
label_client:"Cliente",label_date:"Date",signal_f_symptom:"Symptôme / signal",signal_f_symptomph:"ex. brouillard mental, cycle irrégulier",signal_f_category:"Catégorie",
cat_fatigue:"Fatigue",cat_hormonal:"Hormonal / Cycle",cat_cognitive:"Cognitif",cat_spiritual:"Sécheresse spirituelle",cat_overcommit:"Surengagement",cat_other:"Autre",
signal_f_load:"Charge structurelle",value_label:"Valeur :",legend_toggle:"Que signifie chaque plage ?",signal_f_note:"Note (ce qui pesait cette semaine-là)",
btn_cancel_edit:"Annuler la Modification",btn_log_signal:"Enregistrer le Signal",btn_update_signal:"Mettre à Jour le Signal",filter_by_client:"Filtrer par cliente",filter_all_clients:"Toutes les clientes",
result_head:"Résultat",result_structural_load:"Charge Structurelle",result_delta_vs_avg:"par rapport à la moyenne de cette cliente",result_baseline_first_signal:"Premier signal enregistré pour cette cliente — établissement d'une base de référence.",
table_client:"Cliente",table_date:"Date",table_signal:"Signal",table_category:"Catégorie",table_load:"Charge",signal_empty:"Aucun signal enregistré pour l'instant.",alert_select_client:"Enregistrez et sélectionnez d'abord une cliente.",
integration_corenum:"PILIER 02",integration_h2:"Point de Contrôle d'Intégration",integration_lede:"Esprit, âme et corps sont notés ensemble, selon une échelle définie à chaque fois — jamais séparément, car le cadre les traite comme un seul système.",
integration_f_note:"Note",integration_f_noteph:"Ce qui a bougé, et ce qui n'a pas bougé",label_spirit:"Esprit",label_soul:"Âme",label_body:"Corps",btn_log_checkin:"Enregistrer le Point de Contrôle",btn_update_checkin:"Mettre à Jour le Point de Contrôle",
result_baseline_entry:"Entrée de référence",table_spirit:"Esprit",table_soul:"Âme",table_body:"Corps",integration_empty:"Aucun point de contrôle pour l'instant.",
framework_corenum:"PILIER 03",framework_h2:"Cadre de Référence",framework_lede:"La doctrine fixe sur laquelle repose le reste du système.",
f1_title:"01 · Redéfinition du Diagnostic",f1_desc:"Les symptômes sont lus comme un signal, non comme un verdict. La fatigue, le déséquilibre hormonal et le brouillard mental indiquent une surcharge structurelle avant une défaillance biochimique.",
f2_title:"02 · Principe d'Intégration",f2_desc:"L'esprit, l'âme et le corps sont traités comme un seul système en interaction continue. Aucune intervention n'isole une seule couche.",
f3_title:"03 · Synthèse Méthodologique",f3_desc:"La physiologie hormonale et la connaissance du cycle sont combinées à une lecture biblique de l'identité et du dessein. Cette combinaison est le mécanisme du changement.",
f4_title:"04 · Architecture de Diffusion",f4_desc:"Trois formats coordonnés : un programme individuel de quatre mois, une doctrine écrite (livre), et une formation collective via du contenu et des événements en direct.",
f5_title:"05 · Vision Structurelle / Sociétale",f5_desc:"L'épuisement féminin généralisé est traité comme un phénomène structurel — la demande dépassant la conception à grande échelle — non comme une épidémie d'hormones individuellement déréglées.",
roadmap_corenum:"PILIER 04",roadmap_h2:"Feuille de Route du Programme",roadmap_lede:"Quatre mois, seize semaines — le modèle commun sur lequel repose le programme individuel.",
month1:"Mois 1 — Évaluation et Bases",month2:"Mois 2 — Réalignement",month3:"Mois 3 — Intégration en Pratique",month4:"Mois 4 — Consolidation",
m1w1:"Admission de la charge structurelle initiale + historique du cycle",m1w2:"Notation initiale esprit-âme-corps",m1w3:"Cartographier les engagements actuels par rapport à la capacité réelle",m1w4:"Établir un rythme de suivi (Journal des Signaux + Intégration)",
m2w1:"Identifier les 3 principales surcharges structurelles à supprimer ou restructurer",m2w2:"Introduire des ajustements de mode de vie liés au cycle",m2w3:"Commencer l'étude identité/dessein (couche spirituelle)",m2w4:"Réévaluation de la charge à mi-parcours",
m3w1:"Appliquer les ajustements sous charge réelle (travail, famille)",m3w2:"Suivre la réponse hormonale à la charge structurelle réduite",m3w3:"Approfondir la pratique spirituelle liée au travail identitaire",m3w4:"Traiter les points de rechute où l'ancienne charge revient",
m4w1:"Réévaluation complète esprit-âme-corps par rapport à la base",m4w2:"Construire un plafond de charge durable et pérenne",m4w3:"Définir le rythme à tenir au-delà du programme",m4w4:"Bilan de sortie et plan d'avenir",week_label:"Sem.",
metrics_corenum:"PILIER 05",metrics_h2:"Métriques Structurelles",metrics_lede:"Agrégées sur l'ensemble des clientes enregistrées — le résultat à l'échelle structurelle et sociétale.",
gauge_load:"Indice de Charge Structurelle (÷10)",gauge_integration:"Intégration Esprit-Âme-Corps (÷10)",gauge_progress:"Progression du Programme",metrics_empty:"Aucune donnée enregistrée pour l'instant. Enregistrez une cliente et ajoutez des entrées pour alimenter ces métriques.",
footer_note:"Système hors ligne. Aucun appel réseau, aucune police ni script externe. Les données ne vivent en mémoire que pour cette session — utilisez Exporter avant de fermer, et Importer pour reprendre. Les valeurs d'échelle sont des repères d'auto-évaluation, non des instruments de diagnostic.",
print_title:"Charge Structurelle — Rapport Clientes",print_generated:"Généré",print_noclients:"Aucune cliente enregistrée pour l'instant.",print_signal_section:"Journal des Signaux",print_integration_section:"Points de Contrôle d'Intégration",
print_no_signals:"Aucun signal enregistré.",print_no_checkins:"Aucun point de contrôle enregistré.",print_roadmap_section:"Progression de la Feuille de Route",print_roadmap_text:"de la feuille de route de 16 semaines complétée (modèle commun).",
alert_csv_bad:"Impossible de lire ce fichier. Un CSV exporté depuis ce système est attendu.",alert_pdf_import:"Les fichiers PDF ne peuvent pas être importés. Un PDF est un rapport mis en forme, pas des données structurées — il n'existe aucun moyen fiable hors ligne d'en extraire les dossiers clientes. Utilisez \u00abExporter CSV\u00bb / \u00abImporter CSV\u00bb pour transférer des données entre sessions, et le PDF uniquement pour imprimer ou partager un rapport.",
sc_load_0_t:"Minimale",sc_load_0_d:"Occurrence isolée, aucune pression réelle sur la capacité.",sc_load_1_t:"Légère",sc_load_1_d:"Perceptible mais gérable ; capacité légèrement mise à l'épreuve.",sc_load_2_t:"Modérée",sc_load_2_d:"Récurrente ; la demande atteint ou dépasse clairement la capacité.",sc_load_3_t:"Élevée",sc_load_3_d:"Fréquente et perturbatrice ; la charge dépasse la capacité de conception la plupart des jours.",sc_load_4_t:"Sévère",sc_load_4_d:"Constante et systémique ; le corps signale une surcharge urgente.",
sc_spirit_0_t:"Déconnectée",sc_spirit_0_d:"Aucun sentiment de la présence de Dieu ; la foi semble absente ou lointaine.",sc_spirit_1_t:"Évincée",sc_spirit_1_d:"Consciente du désir de Dieu mais aucun espace réel n'est fait.",sc_spirit_2_t:"Intermittente",sc_spirit_2_d:"Un peu de connexion, pratique irrégulière, facilement supplantée.",sc_spirit_3_t:"Engagée",sc_spirit_3_d:"Pratique régulière ; l'identité en Dieu devient plus claire.",sc_spirit_4_t:"Enracinée",sc_spirit_4_d:"Communion stable ; identité sûre quelles que soient les circonstances.",
sc_soul_0_t:"Épuisée",sc_soul_0_d:"Débordée, réactive, sans marge émotionnelle ni mentale.",sc_soul_1_t:"Tendue",sc_soul_1_d:"Irritable, dans le brouillard, fonctionnant sur les réserves.",sc_soul_2_t:"Elle tient",sc_soul_2_d:"Fonctionnelle mais fatiguée ; elle gère plutôt qu'elle ne s'épanouit.",sc_soul_3_t:"Claire",sc_soul_3_d:"Régulée émotionnellement, présente mentalement, volonté engagée.",sc_soul_4_t:"Entière",sc_soul_4_d:"Calme, résiliente, esprit/émotion/volonté en harmonie.",
sc_body_0_t:"Crise",sc_body_0_d:"Symptômes sévères ; corps en détresse active.",sc_body_1_t:"En difficulté",sc_body_1_d:"Symptômes fréquents, faible énergie, signes hormonaux/de cycle défavorables.",sc_body_2_t:"Fluctuant",sc_body_2_d:"Jours variables ; un peu de stabilité, un peu de perturbation.",sc_body_3_t:"En stabilisation",sc_body_3_d:"Globalement constant ; symptômes présents mais gérables.",sc_body_4_t:"Épanoui",sc_body_4_d:"Énergie forte, cycle/hormones équilibrés, symptômes minimes."
},
pt:{app_eyebrow:"Estrutura de Restauração · Sistema Offline",app_title:"Carga Estrutural",io_export_csv:"Exportar CSV",io_export_pdf:"Exportar PDF",io_import_csv:"Importar CSV",io_lang_label:"Idioma",io_reset:"Redefinir para Inglês",
nav_dashboard:"Painel",nav_clients:"Clientes",nav_signal:"Registro de Sinais",nav_integration:"Integração",nav_framework:"Estrutura Teórica",nav_roadmap:"Roteiro do Programa",nav_metrics:"Métricas Estruturais",
dash_eyebrow:"Tese central",dash_hook1:"O cansaço não é prova de que o corpo está quebrado.",dash_hook2:"É prova de que a carga excede o design.",dash_lede:"Este sistema registra cada mulher como uma ficha de cliente, anota sintomas como sinal estrutural segundo uma escala definida, acompanha espírito, alma e corpo como um único sistema, e devolve um resultado após cada registro.",
dash_m_clients:"Clientes registradas",dash_m_signals:"Sinais registrados",dash_m_avgload:"Carga estrutural média",dash_m_avgspirit:"Pontuação média de espírito",
svg_crosssection:"CORTE TRANSVERSAL · CAMINHO DA CARGA",svg_demand:"demanda",svg_spirit:"Espírito",svg_spirit_sub:"identidade · design · propósito",svg_soul:"Alma",svg_soul_sub:"mente · emoção · vontade",svg_body:"Corpo",svg_body_sub:"hormônios · ciclo · sistema nervoso",svg_support:"CINCO PILARES = SUPORTE DE CARGA",
clients_corenum:"CADASTRO DE CLIENTES",clients_h2:"Cadastrar e Gerenciar Clientes",clients_lede:"Cada mulher atendida é cadastrada uma vez aqui, depois selecionada em um menu ao registrar sinais ou check-ins. Os registros são totalmente editáveis — atualize dados ou corrija o histórico a qualquer momento.",
clients_f_name:"Nome completo",clients_f_contact:"Contato (e-mail / telefone)",clients_f_start:"Data de início",clients_f_status:"Status",clients_f_note:"Nota de admissão",clients_f_noteph:"Contexto na admissão — o que a trouxe até aqui",clients_submit:"Cadastrar Cliente",
status_active:"Ativa",status_paused:"Pausada",status_completed:"Concluída",clients_nocontact:"sem contato registrado",clients_started:"Início",clients_edit:"Editar",clients_view:"Ver Registros",clients_delete:"Excluir",
mini_signals:"Sinais",mini_checkins:"Check-ins",mini_avgload:"Carga Média",mini_avgspirit:"Espírito Médio",mini_avgsoul:"Alma Média",mini_avgbody:"Corpo Médio",clients_intake_prefix:"Nota de admissão:",save:"Salvar",cancel:"Cancelar",
confirm_delete_client:"Excluir esta cliente e desvincular seus registros? Os registros permanecerão, mas aparecerão sem vínculo.",clients_empty:"Ainda não há clientes cadastradas. Use o formulário acima para adicionar o primeiro registro.",
detail_signal_history:"Histórico de Sinais",detail_integration_history:"Histórico de Integração",detail_no_signals:"Nenhum sinal registrado para esta cliente.",detail_no_integrations:"Nenhum check-in registrado para esta cliente.",
row_edit:"Editar",row_del:"Excl.",confirm_delete_signal:"Excluir este sinal?",confirm_delete_integration:"Excluir este check-in?",no_clients_registered:"Ainda não há clientes cadastradas",
signal_corenum:"PILAR 01 + 05",signal_h2:"Registro de Sinais",signal_lede:"Cada sintoma é pontuado pela carga estrutural — o quanto o corpo está sinalizando que a demanda excede a capacidade de design, e não simplesmente uma falha.",
label_client:"Cliente",label_date:"Data",signal_f_symptom:"Sintoma / sinal",signal_f_symptomph:"ex. neblina mental, ciclo irregular",signal_f_category:"Categoria",
cat_fatigue:"Fadiga",cat_hormonal:"Hormonal / Ciclo",cat_cognitive:"Cognitivo",cat_spiritual:"Seca espiritual",cat_overcommit:"Sobrecarga de compromissos",cat_other:"Outro",
signal_f_load:"Carga estrutural",value_label:"Valor:",legend_toggle:"O que cada faixa significa?",signal_f_note:"Nota (o que estava sendo carregado naquela semana)",
btn_cancel_edit:"Cancelar Edição",btn_log_signal:"Registrar Sinal",btn_update_signal:"Atualizar Sinal",filter_by_client:"Filtrar por cliente",filter_all_clients:"Todas as clientes",
result_head:"Resultado",result_structural_load:"Carga Estrutural",result_delta_vs_avg:"em relação à média desta cliente",result_baseline_first_signal:"Primeiro sinal registrado para esta cliente — estabelecendo linha de base.",
table_client:"Cliente",table_date:"Data",table_signal:"Sinal",table_category:"Categoria",table_load:"Carga",signal_empty:"Ainda não há sinais registrados.",alert_select_client:"Cadastre e selecione uma cliente primeiro.",
integration_corenum:"PILAR 02",integration_h2:"Check-in de Integração",integration_lede:"Espírito, alma e corpo são pontuados juntos, segundo uma escala definida a cada vez — nunca separadamente, pois a estrutura os trata como um único sistema.",
integration_f_note:"Nota",integration_f_noteph:"O que mudou, e o que não mudou",label_spirit:"Espírito",label_soul:"Alma",label_body:"Corpo",btn_log_checkin:"Registrar Check-in",btn_update_checkin:"Atualizar Check-in",
result_baseline_entry:"Registro de linha de base",table_spirit:"Espírito",table_soul:"Alma",table_body:"Corpo",integration_empty:"Ainda não há check-ins registrados.",
framework_corenum:"PILAR 03",framework_h2:"Referência da Estrutura",framework_lede:"A doutrina fixa sobre a qual o restante do sistema opera.",
f1_title:"01 · Reformulação do Diagnóstico",f1_desc:"Os sintomas são lidos como sinal, não veredito. Fadiga, desequilíbrio hormonal e neblina mental indicam sobrecarga estrutural antes de indicar falha bioquímica.",
f2_title:"02 · Princípio de Integração",f2_desc:"Espírito, alma e corpo são tratados como um único sistema em interação contínua. Nenhuma intervenção isola uma única camada.",
f3_title:"03 · Síntese Metodológica",f3_desc:"A fisiologia hormonal e a alfabetização do ciclo são combinadas com uma leitura bíblica da identidade e do design. Essa combinação é o mecanismo da mudança.",
f4_title:"04 · Arquitetura de Entrega",f4_desc:"Três formatos coordenados: um programa individual de quatro meses, uma doutrina escrita (livro) e uma formação coletiva por meio de conteúdo e eventos ao vivo.",
f5_title:"05 · Visão Estrutural / Social",f5_desc:"A exaustão feminina generalizada é tratada como um fenômeno estrutural — demanda excedendo o design em escala — não uma epidemia de hormônios individualmente quebrados.",
roadmap_corenum:"PILAR 04",roadmap_h2:"Roteiro do Programa",roadmap_lede:"Quatro meses, dezesseis semanas — o modelo compartilhado que sustenta o programa individual.",
month1:"Mês 1 — Avaliação e Fundamentos",month2:"Mês 2 — Realinhamento",month3:"Mês 3 — Integração na Prática",month4:"Mês 4 — Consolidação",
m1w1:"Admissão de carga estrutural base + histórico de ciclo",m1w2:"Pontuação base espírito-alma-corpo",m1w3:"Mapear compromissos atuais frente à capacidade real",m1w4:"Estabelecer ritmo de acompanhamento (Registro de Sinais + Integração)",
m2w1:"Identificar as 3 principais sobrecargas estruturais a remover ou reestruturar",m2w2:"Introduzir ajustes de estilo de vida conforme o ciclo",m2w3:"Iniciar estudo de identidade/design (camada espiritual)",m2w4:"Reavaliação de carga na metade do programa",
m3w1:"Aplicar ajustes sob carga real (trabalho, família)",m3w2:"Acompanhar a resposta hormonal à carga estrutural reduzida",m3w3:"Aprofundar a prática espiritual ligada ao trabalho de identidade",m3w4:"Tratar pontos de recaída onde a antiga carga retorna",
m4w1:"Reavaliação completa espírito-alma-corpo em relação à linha de base",m4w2:"Construir um teto de carga sustentável e duradouro",m4w3:"Definir o ritmo contínuo além do programa",m4w4:"Revisão de saída e plano futuro",week_label:"Sem",
metrics_corenum:"PILAR 05",metrics_h2:"Métricas Estruturais",metrics_lede:"Agregadas de todas as clientes cadastradas — o resultado em escala estrutural e social.",
gauge_load:"Índice de Carga Estrutural (÷10)",gauge_integration:"Integração Espírito-Alma-Corpo (÷10)",gauge_progress:"Progresso do Programa",metrics_empty:"Ainda não há dados registrados. Cadastre uma cliente e adicione registros para preencher essas métricas.",
footer_note:"Sistema offline. Sem chamadas de rede, sem fontes ou scripts externos. Os dados vivem em memória apenas durante esta sessão — use Exportar antes de fechar, e Importar para retomar. Os valores de escala são pontos de autoavaliação, não instrumentos diagnósticos.",
print_title:"Carga Estrutural — Relatório de Clientes",print_generated:"Gerado em",print_noclients:"Ainda não há clientes cadastradas.",print_signal_section:"Registro de Sinais",print_integration_section:"Check-ins de Integração",
print_no_signals:"Nenhum sinal registrado.",print_no_checkins:"Nenhum check-in registrado.",print_roadmap_section:"Progresso do Roteiro",print_roadmap_text:"do roteiro de 16 semanas concluído (modelo compartilhado).",
alert_csv_bad:"Não foi possível ler esse arquivo. Espera-se um CSV exportado deste sistema.",alert_pdf_import:"Arquivos PDF não podem ser importados. Um PDF é um relatório formatado, não dados estruturados — não há forma confiável offline de extrair registros de clientes dele. Use \u201cExportar CSV\u201d / \u201cImportar CSV\u201d para mover dados entre sessões, e o PDF apenas para imprimir ou compartilhar um relatório.",
sc_load_0_t:"Mínima",sc_load_0_d:"Ocorrência isolada, sem pressão real sobre a capacidade.",sc_load_1_t:"Leve",sc_load_1_d:"Perceptível mas gerenciável; capacidade levemente testada.",sc_load_2_t:"Moderada",sc_load_2_d:"Recorrente; a demanda claramente atinge ou excede a capacidade.",sc_load_3_t:"Alta",sc_load_3_d:"Frequente e disruptiva; a carga excede a capacidade de design na maioria dos dias.",sc_load_4_t:"Severa",sc_load_4_d:"Constante e sistêmica; o corpo sinaliza sobrecarga urgente.",
sc_spirit_0_t:"Desconectada",sc_spirit_0_d:"Nenhum senso da presença de Deus; a fé parece ausente ou distante.",sc_spirit_1_t:"Sufocada",sc_spirit_1_d:"Consciente do desejo de Deus, mas nenhum espaço real é aberto para isso.",sc_spirit_2_t:"Intermitente",sc_spirit_2_d:"Alguma conexão, prática inconsistente, facilmente deslocada.",sc_spirit_3_t:"Engajada",sc_spirit_3_d:"Prática regular; a identidade em Deus está ficando mais clara.",sc_spirit_4_t:"Enraizada",sc_spirit_4_d:"Comunhão estável; identidade segura independentemente das circunstâncias.",
sc_soul_0_t:"Esgotada",sc_soul_0_d:"Sobrecarregada, reativa, sem margem emocional ou mental.",sc_soul_1_t:"Tensa",sc_soul_1_d:"Irritada, com neblina mental, funcionando com reservas.",sc_soul_2_t:"Sobrevivendo",sc_soul_2_d:"Funcional mas cansada; administrando em vez de prosperar.",sc_soul_3_t:"Clara",sc_soul_3_d:"Regulada emocionalmente, presente mentalmente, vontade engajada.",sc_soul_4_t:"Íntegra",sc_soul_4_d:"Calma, resiliente, mente/emoção/vontade em alinhamento.",
sc_body_0_t:"Crise",sc_body_0_d:"Sintomas graves; corpo em sofrimento ativo.",sc_body_1_t:"Lutando",sc_body_1_d:"Sintomas frequentes, pouca energia, sinais hormonais/de ciclo ruins.",sc_body_2_t:"Flutuante",sc_body_2_d:"Dias variados; alguma estabilidade, alguma perturbação.",sc_body_3_t:"Estabilizando",sc_body_3_d:"Majoritariamente constante; sintomas presentes mas administráveis.",sc_body_4_t:"Florescendo",sc_body_4_d:"Energia forte, ciclo/hormônios equilibrados, sintomas mínimos."
},
de:{app_eyebrow:"Wiederherstellungsrahmen · Offline-System",app_title:"Strukturelle Last",io_export_csv:"CSV Exportieren",io_export_pdf:"PDF Exportieren",io_import_csv:"CSV Importieren",io_lang_label:"Sprache",io_reset:"Auf Englisch Zurücksetzen",
nav_dashboard:"Übersicht",nav_clients:"Klientinnen",nav_signal:"Signalprotokoll",nav_integration:"Integration",nav_framework:"Rahmenwerk",nav_roadmap:"Programm-Fahrplan",nav_metrics:"Strukturelle Metriken",
dash_eyebrow:"Kernthese",dash_hook1:"Erschöpfung ist kein Beweis dafür, dass der Körper kaputt ist.",dash_hook2:"Sie ist der Beweis, dass die Last das Design übersteigt.",dash_lede:"Dieses System erfasst jede Frau als Klientinnenakte, protokolliert Symptome als strukturelles Signal anhand einer definierten Skala, verfolgt Geist, Seele und Körper als ein System und liefert nach jeder Eingabe ein Ergebnis.",
dash_m_clients:"Registrierte Klientinnen",dash_m_signals:"Erfasste Signale",dash_m_avgload:"Durchschn. strukturelle Last",dash_m_avgspirit:"Durchschn. Geist-Wert",
svg_crosssection:"QUERSCHNITT · LASTPFAD",svg_demand:"Anforderung",svg_spirit:"Geist",svg_spirit_sub:"Identität · Bestimmung · Zweck",svg_soul:"Seele",svg_soul_sub:"Verstand · Emotion · Wille",svg_body:"Körper",svg_body_sub:"Hormone · Zyklus · Nervensystem",svg_support:"FÜNF SÄULEN = TRAGENDE STÜTZE",
clients_corenum:"KLIENTINNENVERZEICHNIS",clients_h2:"Klientinnen Registrieren & Verwalten",clients_lede:"Jede betreute Frau wird hier einmal registriert und danach über ein Dropdown beim Erfassen von Signalen oder Check-ins ausgewählt. Einträge sind vollständig bearbeitbar — Angaben jederzeit aktualisieren oder den Verlauf korrigieren.",
clients_f_name:"Vollständiger Name",clients_f_contact:"Kontakt (E-Mail / Telefon)",clients_f_start:"Startdatum",clients_f_status:"Status",clients_f_note:"Aufnahmenotiz",clients_f_noteph:"Kontext bei Aufnahme — was sie hergeführt hat",clients_submit:"Klientin Registrieren",
status_active:"Aktiv",status_paused:"Pausiert",status_completed:"Abgeschlossen",clients_nocontact:"kein Kontakt hinterlegt",clients_started:"Begonnen",clients_edit:"Bearbeiten",clients_view:"Einträge Ansehen",clients_delete:"Löschen",
mini_signals:"Signale",mini_checkins:"Check-ins",mini_avgload:"Ø Last",mini_avgspirit:"Ø Geist",mini_avgsoul:"Ø Seele",mini_avgbody:"Ø Körper",clients_intake_prefix:"Aufnahmenotiz:",save:"Speichern",cancel:"Abbrechen",
confirm_delete_client:"Diese Klientin löschen und ihre erfassten Einträge trennen? Einträge bleiben erhalten, erscheinen aber als nicht verknüpft.",clients_empty:"Noch keine Klientinnen registriert. Nutzen Sie das Formular oben, um den ersten Eintrag hinzuzufügen.",
detail_signal_history:"Signalverlauf",detail_integration_history:"Integrationsverlauf",detail_no_signals:"Keine Signale für diese Klientin erfasst.",detail_no_integrations:"Keine Check-ins für diese Klientin erfasst.",
row_edit:"Bearb.",row_del:"Lösch.",confirm_delete_signal:"Dieses Signal löschen?",confirm_delete_integration:"Diesen Check-in löschen?",no_clients_registered:"Noch keine Klientinnen registriert",
signal_corenum:"SÄULE 01 + 05",signal_h2:"Signalprotokoll",signal_lede:"Jedes Symptom wird nach struktureller Last bewertet — wie stark der Körper signalisiert, dass die Anforderung die Designkapazität übersteigt, statt einfach nur zu versagen.",
label_client:"Klientin",label_date:"Datum",signal_f_symptom:"Symptom / Signal",signal_f_symptomph:"z. B. Brain Fog, unregelmäßiger Zyklus",signal_f_category:"Kategorie",
cat_fatigue:"Erschöpfung",cat_hormonal:"Hormonell / Zyklus",cat_cognitive:"Kognitiv",cat_spiritual:"Geistliche Trockenheit",cat_overcommit:"Überlastung",cat_other:"Sonstiges",
signal_f_load:"Strukturelle Last",value_label:"Wert:",legend_toggle:"Was bedeutet jeder Bereich?",signal_f_note:"Notiz (was diese Woche getragen wurde)",
btn_cancel_edit:"Bearbeitung Abbrechen",btn_log_signal:"Signal Erfassen",btn_update_signal:"Signal Aktualisieren",filter_by_client:"Nach Klientin filtern",filter_all_clients:"Alle Klientinnen",
result_head:"Ergebnis",result_structural_load:"Strukturelle Last",result_delta_vs_avg:"gegenüber dem Durchschnitt dieser Klientin",result_baseline_first_signal:"Erstes erfasstes Signal für diese Klientin — Ausgangswert wird festgelegt.",
table_client:"Klientin",table_date:"Datum",table_signal:"Signal",table_category:"Kategorie",table_load:"Last",signal_empty:"Noch keine Signale erfasst.",alert_select_client:"Bitte zuerst eine Klientin registrieren und auswählen.",
integration_corenum:"SÄULE 02",integration_h2:"Integrations-Check-in",integration_lede:"Geist, Seele und Körper werden jedes Mal gemeinsam anhand einer definierten Skala bewertet — nie getrennt, da das Rahmenwerk sie als ein System behandelt.",
integration_f_note:"Notiz",integration_f_noteph:"Was sich verändert hat und was nicht",label_spirit:"Geist",label_soul:"Seele",label_body:"Körper",btn_log_checkin:"Check-in Erfassen",btn_update_checkin:"Check-in Aktualisieren",
result_baseline_entry:"Ausgangseintrag",table_spirit:"Geist",table_soul:"Seele",table_body:"Körper",integration_empty:"Noch keine Check-ins erfasst.",
framework_corenum:"SÄULE 03",framework_h2:"Rahmenwerk-Referenz",framework_lede:"Die feste Lehre, auf der der Rest des Systems basiert.",
f1_title:"01 · Neurahmung der Diagnose",f1_desc:"Symptome werden als Signal gelesen, nicht als Urteil. Erschöpfung, hormonelle Störung und Brain Fog deuten zuerst auf strukturelle Überlastung hin, nicht auf biochemisches Versagen.",
f2_title:"02 · Integrationsprinzip",f2_desc:"Geist, Seele und Körper werden als ein kontinuierlich zusammenwirkendes System behandelt. Keine Maßnahme isoliert eine einzelne Ebene.",
f3_title:"03 · Methodische Synthese",f3_desc:"Hormonphysiologie und Zykluswissen werden mit einer biblischen Deutung von Identität und Bestimmung verbunden. Diese Verbindung ist der Wirkmechanismus der Veränderung.",
f4_title:"04 · Vermittlungsstruktur",f4_desc:"Drei aufeinander abgestimmte Formate: ein vierwöchiges 1:1-Programm über vier Monate, eine schriftliche Lehre (Buch) und gemeinschaftliche Formung durch Inhalte und Live-Events.",
f5_title:"05 · Strukturelle / Gesellschaftliche Vision",f5_desc:"Weit verbreitete weibliche Erschöpfung wird als strukturelles Phänomen behandelt — Anforderung, die das Design im großen Maßstab übersteigt — nicht als Epidemie individuell gestörter Hormone.",
roadmap_corenum:"SÄULE 04",roadmap_h2:"Programm-Fahrplan",roadmap_lede:"Vier Monate, sechzehn Wochen — die gemeinsame Vorlage, auf der das 1:1-Programm aufbaut.",
month1:"Monat 1 — Bestandsaufnahme & Grundlagen",month2:"Monat 2 — Neuausrichtung",month3:"Monat 3 — Integration in die Praxis",month4:"Monat 4 — Konsolidierung",
m1w1:"Erfassung der strukturellen Ausgangslast + Zyklusverlauf",m1w2:"Ausgangsbewertung Geist-Seele-Körper",m1w3:"Aktuelle Verpflichtungen der tatsächlichen Kapazität gegenüberstellen",m1w4:"Nachverfolgungsrhythmus etablieren (Signalprotokoll + Integration)",
m2w1:"Die 3 größten strukturellen Überlastungen zum Abbau oder zur Umstrukturierung identifizieren",m2w2:"Zyklusbasierte Lebensstilanpassungen einführen",m2w3:"Identitäts-/Bestimmungsstudium beginnen (geistliche Ebene)",m2w4:"Lastbewertung zur Halbzeit",
m3w1:"Anpassungen unter realer Last anwenden (Arbeit, Familie)",m3w2:"Hormonelle Reaktion auf reduzierte strukturelle Last verfolgen",m3w3:"Geistliche Praxis vertiefen, verbunden mit Identitätsarbeit",m3w4:"Rückfallpunkte angehen, an denen die alte Last zurückkehrt",
m4w1:"Vollständige Geist-Seele-Körper-Neubewertung im Vergleich zum Ausgangswert",m4w2:"Eine nachhaltige, dauerhafte Lastobergrenze aufbauen",m4w3:"Fortlaufenden Rhythmus über das Programm hinaus festlegen",m4w4:"Abschlussüberprüfung und Zukunftsplan",week_label:"Wo.",
metrics_corenum:"SÄULE 05",metrics_h2:"Strukturelle Metriken",metrics_lede:"Aggregiert über alle registrierten Klientinnen — das Ergebnis auf struktureller, gesellschaftlicher Ebene.",
gauge_load:"Index Strukturelle Last (÷10)",gauge_integration:"Integration Geist-Seele-Körper (÷10)",gauge_progress:"Programmfortschritt",metrics_empty:"Noch keine Daten erfasst. Registrieren Sie eine Klientin und fügen Sie Einträge hinzu, um diese Metriken zu füllen.",
footer_note:"Offline-System. Keine Netzwerkaufrufe, keine externen Schriftarten oder Skripte. Daten liegen nur für diese Sitzung im Speicher — vor dem Schließen exportieren, zum Fortsetzen importieren. Skalenwerte sind Selbstauskunfts-Referenzpunkte, keine diagnostischen Instrumente.",
print_title:"Strukturelle Last — Klientinnenbericht",print_generated:"Erstellt am",print_noclients:"Noch keine Klientinnen registriert.",print_signal_section:"Signalprotokoll",print_integration_section:"Integrations-Check-ins",
print_no_signals:"Keine Signale erfasst.",print_no_checkins:"Keine Check-ins erfasst.",print_roadmap_section:"Fortschritt des Fahrplans",print_roadmap_text:"des 16-wöchigen Fahrplans abgeschlossen (gemeinsame Vorlage).",
alert_csv_bad:"Diese Datei konnte nicht gelesen werden. Erwartet wird eine aus diesem System exportierte CSV-Datei.",alert_pdf_import:"PDF-Dateien können nicht importiert werden. Ein PDF ist ein formatierter Bericht, keine strukturierten Daten — es gibt keine zuverlässige Offline-Methode, Klientinnendaten daraus wiederherzustellen. Nutzen Sie \u201eCSV Exportieren\u201c / \u201eCSV Importieren\u201c, um Daten zwischen Sitzungen zu übertragen, und PDF nur zum Drucken oder Teilen eines Berichts.",
sc_load_0_t:"Minimal",sc_load_0_d:"Vereinzeltes Vorkommnis, keine echte Belastung der Kapazität.",sc_load_1_t:"Leicht",sc_load_1_d:"Spürbar, aber handhabbar; Kapazität leicht auf die Probe gestellt.",sc_load_2_t:"Mäßig",sc_load_2_d:"Wiederkehrend; Anforderung erreicht oder übersteigt eindeutig die Kapazität.",sc_load_3_t:"Hoch",sc_load_3_d:"Häufig und störend; Last übersteigt an den meisten Tagen die Designkapazität.",sc_load_4_t:"Schwer",sc_load_4_d:"Konstant und systemisch; der Körper signalisiert dringende Überlastung.",
sc_spirit_0_t:"Getrennt",sc_spirit_0_d:"Kein Gespür für Gottes Gegenwart; der Glaube fühlt sich abwesend oder fern an.",sc_spirit_1_t:"Verdrängt",sc_spirit_1_d:"Sehnsucht nach Gott ist bewusst, aber kein echter Raum wird geschaffen.",sc_spirit_2_t:"Unregelmäßig",sc_spirit_2_d:"Etwas Verbindung, unregelmäßige Praxis, leicht verdrängt.",sc_spirit_3_t:"Engagiert",sc_spirit_3_d:"Regelmäßige Praxis; die Identität in Gott wird klarer.",sc_spirit_4_t:"Verwurzelt",sc_spirit_4_d:"Stete Gemeinschaft; Identität sicher, unabhängig von den Umständen.",
sc_soul_0_t:"Erschöpft",sc_soul_0_d:"Überwältigt, reaktiv, kein emotionaler oder mentaler Spielraum mehr.",sc_soul_1_t:"Angespannt",sc_soul_1_d:"Gereizt, benebelt, läuft auf Reserven.",sc_soul_2_t:"Bewältigend",sc_soul_2_d:"Funktionsfähig, aber müde; bewältigt eher, als dass sie aufblüht.",sc_soul_3_t:"Klar",sc_soul_3_d:"Emotional reguliert, mental präsent, Wille aktiv beteiligt.",sc_soul_4_t:"Ganz",sc_soul_4_d:"Ruhig, belastbar, Verstand/Emotion/Wille im Einklang.",
sc_body_0_t:"Krise",sc_body_0_d:"Schwere Symptome; Körper in akuter Not.",sc_body_1_t:"Kämpfend",sc_body_1_d:"Häufige Symptome, wenig Energie, schlechte Zyklus-/Hormonzeichen.",sc_body_2_t:"Schwankend",sc_body_2_d:"Gemischte Tage; etwas Stabilität, etwas Störung.",sc_body_3_t:"Stabilisierend",sc_body_3_d:"Größtenteils konstant; Symptome vorhanden, aber handhabbar.",sc_body_4_t:"Aufblühend",sc_body_4_d:"Starke Energie, ausgeglichener Zyklus/Hormone, minimale Symptome."
},
ar:{app_eyebrow:"إطار الاستعادة · نظام دون اتصال",app_title:"الحمل البنيوي",io_export_csv:"تصدير CSV",io_export_pdf:"تصدير PDF",io_import_csv:"استيراد CSV",io_lang_label:"اللغة",io_reset:"إعادة الضبط إلى الإنجليزية",
nav_dashboard:"لوحة القيادة",nav_clients:"العميلات",nav_signal:"سجل الإشارات",nav_integration:"التكامل",nav_framework:"الإطار المرجعي",nav_roadmap:"خارطة طريق البرنامج",nav_metrics:"المقاييس البنيوية",
dash_eyebrow:"الأطروحة الأساسية",dash_hook1:"الإرهاق ليس دليلاً على أن الجسد معطوب.",dash_hook2:"إنه دليل على أن الحمل يفوق التصميم.",dash_lede:"يسجّل هذا النظام كل امرأة كسجل عميلة، ويوثّق الأعراض كإشارة بنيوية وفق مقياس محدد، ويتابع الروح والنفس والجسد كنظام واحد، ويعرض نتيجة بعد كل إدخال.",
dash_m_clients:"العميلات المسجَّلات",dash_m_signals:"الإشارات المسجَّلة",dash_m_avgload:"متوسط الحمل البنيوي",dash_m_avgspirit:"متوسط درجة الروح",
svg_crosssection:"مقطع عرضي · مسار الحمل",svg_demand:"الطلب",svg_spirit:"الروح",svg_spirit_sub:"الهوية · التصميم · الغاية",svg_soul:"النفس",svg_soul_sub:"العقل · العاطفة · الإرادة",svg_body:"الجسد",svg_body_sub:"الهرمونات · الدورة · الجهاز العصبي",svg_support:"الركائز الخمس = الدعم الحامل",
clients_corenum:"سجل العميلات",clients_h2:"تسجيل وإدارة العميلات",clients_lede:"تُسجَّل كل امرأة تتم متابعتها هنا مرة واحدة، ثم تُختار من قائمة منسدلة عند تسجيل الإشارات أو المتابعات. السجلات قابلة للتعديل بالكامل — يمكن تحديث البيانات أو تصحيح السجل في أي وقت.",
clients_f_name:"الاسم الكامل",clients_f_contact:"وسيلة التواصل (بريد إلكتروني / هاتف)",clients_f_start:"تاريخ البدء",clients_f_status:"الحالة",clients_f_note:"ملاحظة الاستقبال",clients_f_noteph:"السياق عند الاستقبال — ما الذي دفعها للحضور",clients_submit:"تسجيل العميلة",
status_active:"نشطة",status_paused:"متوقفة مؤقتًا",status_completed:"مكتملة",clients_nocontact:"لا توجد وسيلة تواصل مسجَّلة",clients_started:"بدأت في",clients_edit:"تعديل",clients_view:"عرض السجلات",clients_delete:"حذف",
mini_signals:"الإشارات",mini_checkins:"المتابعات",mini_avgload:"متوسط الحمل",mini_avgspirit:"متوسط الروح",mini_avgsoul:"متوسط النفس",mini_avgbody:"متوسط الجسد",clients_intake_prefix:"ملاحظة الاستقبال:",save:"حفظ",cancel:"إلغاء",
confirm_delete_client:"هل تريدين حذف هذه العميلة وفصل إدخالاتها المسجَّلة؟ ستبقى الإدخالات موجودة لكنها ستظهر غير مرتبطة.",clients_empty:"لا توجد عميلات مسجَّلات بعد. استخدمي النموذج أعلاه لإضافة أول سجل.",
detail_signal_history:"سجل الإشارات",detail_integration_history:"سجل التكامل",detail_no_signals:"لا توجد إشارات مسجَّلة لهذه العميلة.",detail_no_integrations:"لا توجد متابعات مسجَّلة لهذه العميلة.",
row_edit:"تعديل",row_del:"حذف",confirm_delete_signal:"هل تريدين حذف هذه الإشارة؟",confirm_delete_integration:"هل تريدين حذف هذه المتابعة؟",no_clients_registered:"لا توجد عميلات مسجَّلات بعد",
signal_corenum:"الركيزة 01 + 05",signal_h2:"سجل الإشارات",signal_lede:"يُقيَّم كل عرض وفق الحمل البنيوي — إلى أي مدى يشير الجسد إلى أن الطلب يفوق طاقة التصميم، وليس مجرد خلل.",
label_client:"العميلة",label_date:"التاريخ",signal_f_symptom:"العرض / الإشارة",signal_f_symptomph:"مثال: ضبابية ذهنية، اضطراب الدورة",signal_f_category:"الفئة",
cat_fatigue:"إرهاق",cat_hormonal:"هرموني / الدورة",cat_cognitive:"إدراكي",cat_spiritual:"جفاف روحي",cat_overcommit:"إفراط في الالتزامات",cat_other:"أخرى",
signal_f_load:"الحمل البنيوي",value_label:"القيمة:",legend_toggle:"ماذا يعني كل نطاق؟",signal_f_note:"ملاحظة (ما الذي كانت تحمله تلك الأسبوع)",
btn_cancel_edit:"إلغاء التعديل",btn_log_signal:"تسجيل الإشارة",btn_update_signal:"تحديث الإشارة",filter_by_client:"تصفية حسب العميلة",filter_all_clients:"كل العميلات",
result_head:"النتيجة",result_structural_load:"الحمل البنيوي",result_delta_vs_avg:"مقارنةً بمتوسط هذه العميلة",result_baseline_first_signal:"أول إشارة مسجَّلة لهذه العميلة — يتم تحديد خط الأساس.",
table_client:"العميلة",table_date:"التاريخ",table_signal:"الإشارة",table_category:"الفئة",table_load:"الحمل",signal_empty:"لا توجد إشارات مسجَّلة بعد.",alert_select_client:"يرجى تسجيل واختيار عميلة أولاً.",
integration_corenum:"الركيزة 02",integration_h2:"متابعة التكامل",integration_lede:"تُقيَّم الروح والنفس والجسد معًا في كل مرة وفق مقياس محدد — أبدًا بشكل منفصل، لأن الإطار يعاملها كنظام واحد.",
integration_f_note:"ملاحظة",integration_f_noteph:"ما الذي تغيّر، وما الذي لم يتغيّر",label_spirit:"الروح",label_soul:"النفس",label_body:"الجسد",btn_log_checkin:"تسجيل المتابعة",btn_update_checkin:"تحديث المتابعة",
result_baseline_entry:"إدخال خط الأساس",table_spirit:"الروح",table_soul:"النفس",table_body:"الجسد",integration_empty:"لا توجد متابعات مسجَّلة بعد.",
framework_corenum:"الركيزة 03",framework_h2:"الإطار المرجعي",framework_lede:"العقيدة الثابتة التي يعمل عليها باقي النظام.",
f1_title:"01 · إعادة صياغة التشخيص",f1_desc:"تُقرأ الأعراض كإشارة، لا كحكم نهائي. الإرهاق واضطراب الهرمونات والضبابية الذهنية تدل على حمل بنيوي زائد قبل أن تدل على خلل كيميائي حيوي.",
f2_title:"02 · مبدأ التكامل",f2_desc:"تُعامَل الروح والنفس والجسد كنظام واحد متفاعل باستمرار. لا يعزل أي تدخل طبقة واحدة بمفردها.",
f3_title:"03 · التوليف المنهجي",f3_desc:"يُجمَع بين فيزيولوجيا الهرمونات ومعرفة الدورة وبين فهم كتابي للهوية والتصميم. هذا الجمع هو آلية التغيير.",
f4_title:"04 · بنية التقديم",f4_desc:"ثلاثة أشكال متناسقة: برنامج فردي مدته أربعة أشهر، وعقيدة مكتوبة (كتاب)، وتكوين جماعي عبر المحتوى والفعاليات المباشرة.",
f5_title:"05 · الرؤية البنيوية / المجتمعية",f5_desc:"يُعامَل الإرهاق النسائي الواسع الانتشار كظاهرة بنيوية — طلب يفوق التصميم على نطاق واسع — لا كوباء من هرمونات مختلة بشكل فردي.",
roadmap_corenum:"الركيزة 04",roadmap_h2:"خارطة طريق البرنامج",roadmap_lede:"أربعة أشهر، ستة عشر أسبوعًا — القالب المشترك الذي يقوم عليه البرنامج الفردي.",
month1:"الشهر 1 — التقييم والأساسيات",month2:"الشهر 2 — إعادة المواءمة",month3:"الشهر 3 — التكامل في الممارسة",month4:"الشهر 4 — الترسيخ",
m1w1:"تقييم الحمل البنيوي الأساسي + تاريخ الدورة",m1w2:"تقييم أساسي للروح-النفس-الجسد",m1w3:"مقارنة الالتزامات الحالية بالطاقة الفعلية",m1w4:"وضع إيقاع للمتابعة (سجل الإشارات + التكامل)",
m2w1:"تحديد أهم 3 أحمال بنيوية زائدة لإزالتها أو إعادة هيكلتها",m2w2:"إدخال تعديلات نمط حياة متوافقة مع الدورة",m2w3:"بدء دراسة الهوية/التصميم (طبقة الروح)",m2w4:"إعادة تقييم الحمل في منتصف المدة",
m3w1:"تطبيق التعديلات تحت الحمل الواقعي (العمل، الأسرة)",m3w2:"متابعة الاستجابة الهرمونية لانخفاض الحمل البنيوي",m3w3:"تعميق الممارسة الروحية المرتبطة بالعمل على الهوية",m3w4:"معالجة نقاط الانتكاس حيث يعود الحمل القديم",
m4w1:"إعادة تقييم كاملة للروح-النفس-الجسد مقارنة بخط الأساس",m4w2:"بناء سقف حمل مستدام ودائم",m4w3:"تحديد إيقاع مستمر بعد انتهاء البرنامج",m4w4:"مراجعة ختامية وخطة مستقبلية",week_label:"أسبوع",
metrics_corenum:"الركيزة 05",metrics_h2:"المقاييس البنيوية",metrics_lede:"مجمَّعة من جميع العميلات المسجَّلات — النتيجة على المستوى البنيوي والمجتمعي.",
gauge_load:"مؤشر الحمل البنيوي (÷10)",gauge_integration:"تكامل الروح-النفس-الجسد (÷10)",gauge_progress:"تقدم البرنامج",metrics_empty:"لا توجد بيانات مسجَّلة بعد. سجّلي عميلة وأضيفي إدخالات لملء هذه المقاييس.",
footer_note:"نظام دون اتصال. لا مكالمات شبكة، ولا خطوط أو نصوص برمجية خارجية. تبقى البيانات في الذاكرة لهذه الجلسة فقط — استخدمي التصدير قبل الإغلاق، والاستيراد لاستئناف العمل. قيم المقياس هي نقاط تقييم ذاتي، وليست أدوات تشخيصية.",
print_title:"الحمل البنيوي — تقرير العميلات",print_generated:"تاريخ الإنشاء",print_noclients:"لا توجد عميلات مسجَّلات بعد.",print_signal_section:"سجل الإشارات",print_integration_section:"متابعات التكامل",
print_no_signals:"لا توجد إشارات مسجَّلة.",print_no_checkins:"لا توجد متابعات مسجَّلة.",print_roadmap_section:"تقدم خارطة الطريق",print_roadmap_text:"من خارطة طريق الستة عشر أسبوعًا مكتمل (قالب مشترك).",
alert_csv_bad:"تعذّرت قراءة هذا الملف. يجب أن يكون ملف CSV مُصدَّرًا من هذا النظام.",alert_pdf_import:"لا يمكن استيراد ملفات PDF. ملف PDF هو تقرير منسّق، وليس بيانات بنيوية — لا توجد طريقة موثوقة دون اتصال لاستخراج سجلات العميلات منه. استخدمي «تصدير CSV» / «استيراد CSV» لنقل البيانات بين الجلسات، ولا تستخدمي PDF إلا للطباعة أو مشاركة تقرير.",
sc_load_0_t:"طفيف",sc_load_0_d:"حادثة معزولة، لا ضغط حقيقي على الطاقة.",sc_load_1_t:"خفيف",sc_load_1_d:"ملحوظ لكن يمكن التعامل معه؛ طاقة مختبَرة قليلاً.",sc_load_2_t:"متوسط",sc_load_2_d:"متكرر؛ الطلب يوازي أو يفوق الطاقة بوضوح.",sc_load_3_t:"مرتفع",sc_load_3_d:"متكرر ومعطِّل؛ الحمل يفوق طاقة التصميم في معظم الأيام.",sc_load_4_t:"شديد",sc_load_4_d:"مستمر وشامل؛ الجسد يشير إلى حمل زائد عاجل.",
sc_spirit_0_t:"منفصلة",sc_spirit_0_d:"لا إحساس بحضور الله؛ الإيمان يبدو غائبًا أو بعيدًا.",sc_spirit_1_t:"مزاحَمة",sc_spirit_1_d:"واعية بالرغبة في الله لكن لا مساحة حقيقية تُخصَّص لذلك.",sc_spirit_2_t:"متقطعة",sc_spirit_2_d:"بعض التواصل، ممارسة غير منتظمة، سهلة الإزاحة.",sc_spirit_3_t:"منخرطة",sc_spirit_3_d:"ممارسة منتظمة؛ الهوية في الله تزداد وضوحًا.",sc_spirit_4_t:"متجذرة",sc_spirit_4_d:"شركة ثابتة؛ هوية آمنة بغض النظر عن الظروف.",
sc_soul_0_t:"مستنزفة",sc_soul_0_d:"مثقَلة، ردّة فعل مفرطة، لا مساحة عاطفية أو ذهنية متبقية.",sc_soul_1_t:"متوترة",sc_soul_1_d:"سريعة الانفعال، ضبابية ذهنيًا، تعمل على الاحتياطي.",sc_soul_2_t:"صامدة",sc_soul_2_d:"قادرة على الأداء لكنها متعبة؛ تُدير الأمور أكثر مما تزدهر.",sc_soul_3_t:"واضحة",sc_soul_3_d:"منظّمة عاطفيًا، حاضرة ذهنيًا، إرادة فاعلة.",sc_soul_4_t:"متكاملة",sc_soul_4_d:"هادئة، مرنة، العقل/العاطفة/الإرادة في انسجام.",
sc_body_0_t:"أزمة",sc_body_0_d:"أعراض شديدة؛ الجسد في ضائقة فعلية.",sc_body_1_t:"تعاني",sc_body_1_d:"أعراض متكررة، طاقة منخفضة، مؤشرات هرمونية/دورية ضعيفة.",sc_body_2_t:"متذبذبة",sc_body_2_d:"أيام متفاوتة؛ بعض الاستقرار وبعض الاضطراب.",sc_body_3_t:"في طور الاستقرار",sc_body_3_d:"ثابتة في معظمها؛ الأعراض موجودة لكن يمكن التعامل معها.",sc_body_4_t:"مزدهرة",sc_body_4_d:"طاقة قوية، دورة/هرمونات متوازنة، أعراض ضئيلة."
},
zh:{app_eyebrow:"修复框架 · 离线系统",app_title:"结构负荷",io_export_csv:"导出 CSV",io_export_pdf:"导出 PDF",io_import_csv:"导入 CSV",io_lang_label:"语言",io_reset:"重置为英文",
nav_dashboard:"总览",nav_clients:"客户",nav_signal:"信号记录",nav_integration:"整合评估",nav_framework:"框架说明",nav_roadmap:"项目路线图",nav_metrics:"结构指标",
dash_eyebrow:"核心论点",dash_hook1:"疲惫并不能证明身体坏了。",dash_hook2:"它证明的是负荷已超出设计承载力。",dash_lede:"本系统为每位女性建立客户档案，依据既定量表将症状记录为结构性信号，将灵、魂、体作为一个整体系统追踪，并在每次记录后生成结果反馈。",
dash_m_clients:"已登记客户数",dash_m_signals:"已记录信号数",dash_m_avgload:"平均结构负荷",dash_m_avgspirit:"平均灵性得分",
svg_crosssection:"剖面图 · 负荷路径",svg_demand:"需求",svg_spirit:"灵",svg_spirit_sub:"身份 · 设计 · 目的",svg_soul:"魂",svg_soul_sub:"心思 · 情感 · 意志",svg_body:"体",svg_body_sub:"激素 · 周期 · 神经系统",svg_support:"五大支柱 = 承重支撑",
clients_corenum:"客户档案",clients_h2:"登记与管理客户",clients_lede:"每位接受辅导的女性只需在此登记一次，之后在记录信号或整合评估时从下拉菜单中选择即可。档案完全可编辑——可随时更新信息或修正历史记录。",
clients_f_name:"姓名",clients_f_contact:"联系方式（邮箱／电话）",clients_f_start:"开始日期",clients_f_status:"状态",clients_f_note:"接洽记录",clients_f_noteph:"接洽时的背景——是什么促使她前来",clients_submit:"登记客户",
status_active:"进行中",status_paused:"已暂停",status_completed:"已完成",clients_nocontact:"未留联系方式",clients_started:"开始于",clients_edit:"编辑",clients_view:"查看记录",clients_delete:"删除",
mini_signals:"信号数",mini_checkins:"评估次数",mini_avgload:"平均负荷",mini_avgspirit:"平均灵",mini_avgsoul:"平均魂",mini_avgbody:"平均体",clients_intake_prefix:"接洽记录：",save:"保存",cancel:"取消",
confirm_delete_client:"确定删除该客户并解除其已记录条目的关联吗？条目仍会保留，但将显示为未关联。",clients_empty:"尚未登记任何客户。请使用上方表单添加第一条记录。",
detail_signal_history:"信号历史",detail_integration_history:"整合评估历史",detail_no_signals:"该客户暂无信号记录。",detail_no_integrations:"该客户暂无评估记录。",
row_edit:"编辑",row_del:"删除",confirm_delete_signal:"删除该条信号记录？",confirm_delete_integration:"删除该条评估记录？",no_clients_registered:"尚未登记任何客户",
signal_corenum:"支柱 01 + 05",signal_h2:"信号记录",signal_lede:"每一条症状都会按结构负荷评分——衡量身体在多大程度上是在提示需求已超出设计承载力，而不仅仅是功能失调。",
label_client:"客户",label_date:"日期",signal_f_symptom:"症状／信号",signal_f_symptomph:"例如：脑雾、周期紊乱",signal_f_category:"类别",
cat_fatigue:"疲劳",cat_hormonal:"激素／周期",cat_cognitive:"认知",cat_spiritual:"灵性干涸",cat_overcommit:"过度承担",cat_other:"其他",
signal_f_load:"结构负荷",value_label:"数值：",legend_toggle:"每个区间代表什么？",signal_f_note:"备注（那一周承受了什么）",
btn_cancel_edit:"取消编辑",btn_log_signal:"记录信号",btn_update_signal:"更新信号",filter_by_client:"按客户筛选",filter_all_clients:"所有客户",
result_head:"结果",result_structural_load:"结构负荷",result_delta_vs_avg:"相较该客户平均值",result_baseline_first_signal:"该客户的首条信号记录——正在建立基线。",
table_client:"客户",table_date:"日期",table_signal:"信号",table_category:"类别",table_load:"负荷",signal_empty:"尚未记录任何信号。",alert_select_client:"请先登记并选择一位客户。",
integration_corenum:"支柱 02",integration_h2:"整合评估",integration_lede:"灵、魂、体每次都按同一量表一起评分——绝不分开评估，因为该框架将三者视为同一系统。",
integration_f_note:"备注",integration_f_noteph:"哪些方面有变化，哪些没有",label_spirit:"灵",label_soul:"魂",label_body:"体",btn_log_checkin:"记录评估",btn_update_checkin:"更新评估",
result_baseline_entry:"基线记录",table_spirit:"灵",table_soul:"魂",table_body:"体",integration_empty:"尚未记录任何评估。",
framework_corenum:"支柱 03",framework_h2:"框架参考",framework_lede:"系统其余部分运作所依据的固定原则。",
f1_title:"01 · 重新定义诊断",f1_desc:"症状被解读为信号，而非定论。疲劳、激素紊乱和脑雾首先指向结构性超负荷，而非生化功能衰竭。",
f2_title:"02 · 整合原则",f2_desc:"灵、魂、体被视为一个持续互动的整体系统，任何干预都不会孤立地作用于单一层面。",
f3_title:"03 · 方法论综合",f3_desc:"激素生理学与周期知识，与圣经中关于身份和设计的教导相结合。这种结合正是改变得以发生的机制。",
f4_title:"04 · 交付架构",f4_desc:"三种协同形式：为期四个月的一对一辅导项目、成文的理论著作（书籍），以及通过内容与线下活动进行的集体塑造。",
f5_title:"05 · 结构性／社会性愿景",f5_desc:"普遍存在的女性疲惫被视为一种结构性现象——大规模的需求超出设计承载力——而非个体激素失调的流行现象。",
roadmap_corenum:"支柱 04",roadmap_h2:"项目路线图",roadmap_lede:"四个月，十六周——支撑一对一项目的共用模板。",
month1:"第一月 — 评估与基础建设",month2:"第二月 — 重新调整",month3:"第三月 — 实践中的整合",month4:"第四月 — 巩固",
m1w1:"基线结构负荷评估 + 周期病史采集",m1w2:"灵魂体基线评分",m1w3:"梳理当前承诺与实际承载力的对比",m1w4:"建立追踪节奏（信号记录 + 整合评估）",
m2w1:"找出需要去除或重组的前三项结构性超负荷",m2w2:"引入与周期相适应的生活方式调整",m2w3:"开始身份／设计层面的学习（灵性层面）",m2w4:"中期负荷重新评估",
m3w1:"在真实生活负荷下应用调整方案（工作、家庭）",m3w2:"追踪结构负荷降低后的激素反应",m3w3:"深化与身份塑造相关的灵修实践",m3w4:"处理旧负荷回潮的复发节点",
m4w1:"相较基线的灵魂体全面重新评估",m4w2:"建立可持续、可长期维持的负荷上限",m4w3:"确立项目结束后的持续节奏",m4w4:"结项回顾与未来计划",week_label:"第",
metrics_corenum:"支柱 05",metrics_h2:"结构指标",metrics_lede:"汇总所有已登记客户的数据——呈现结构性、社会规模层面的结果。",
gauge_load:"结构负荷指数（÷10）",gauge_integration:"灵魂体整合指数（÷10）",gauge_progress:"项目进度",metrics_empty:"尚无数据记录。请登记一位客户并添加记录以生成这些指标。",
footer_note:"离线系统。无网络请求，无外部字体或脚本。数据仅在本次会话中保存于内存——关闭前请导出，恢复时请导入。量表数值为自我评估参考点，并非诊断工具。",
print_title:"结构负荷 — 客户报告",print_generated:"生成时间",print_noclients:"尚未登记任何客户。",print_signal_section:"信号记录",print_integration_section:"整合评估记录",
print_no_signals:"未记录信号。",print_no_checkins:"未记录评估。",print_roadmap_section:"项目路线图进度",print_roadmap_text:"已完成 16 周路线图中的进度（共用模板）。",
alert_csv_bad:"无法读取该文件。请提供由本系统导出的 CSV 文件。",alert_pdf_import:"无法导入 PDF 文件。PDF 是格式化报告，而非结构化数据——离线环境下无法可靠地从中提取客户记录。请使用“导出 CSV”／“导入 CSV”在不同会话间迁移数据，PDF 仅用于打印或分享报告。",
sc_load_0_t:"轻微",sc_load_0_d:"偶发个例，对承载力没有真正压力。",sc_load_1_t:"较轻",sc_load_1_d:"可察觉但可控；承载力受到轻微考验。",sc_load_2_t:"中等",sc_load_2_d:"反复出现；需求明显达到或超出承载力。",sc_load_3_t:"较高",sc_load_3_d:"频繁且影响明显；大多数日子负荷超出设计承载力。",sc_load_4_t:"严重",sc_load_4_d:"持续且系统性；身体正发出紧急超负荷信号。",
sc_spirit_0_t:"失联",sc_spirit_0_d:"感受不到神的同在；信仰似乎缺席或遥远。",sc_spirit_1_t:"被挤占",sc_spirit_1_d:"意识到渴望亲近神，却未真正腾出空间。",sc_spirit_2_t:"时断时续",sc_spirit_2_d:"有一些连接，操练不规律，容易被挤掉。",sc_spirit_3_t:"投入",sc_spirit_3_d:"规律操练；在神里面的身份认同日渐清晰。",sc_spirit_4_t:"扎根",sc_spirit_4_d:"稳定的相交；无论环境如何，身份认同都很稳固。",
sc_soul_0_t:"枯竭",sc_soul_0_d:"不堪重负，反应过激，情感和心智几乎没有余力。",sc_soul_1_t:"紧绷",sc_soul_1_d:"易怒、思维模糊，靠储备硬撑。",sc_soul_2_t:"勉力支撑",sc_soul_2_d:"能正常运作但疲惫；只是应付，谈不上兴旺。",sc_soul_3_t:"清明",sc_soul_3_d:"情绪调节良好，心智在场，意志投入。",sc_soul_4_t:"完整",sc_soul_4_d:"平静、有韧性，心思、情感与意志彼此协调。",
sc_body_0_t:"危机",sc_body_0_d:"症状严重；身体正处于急性困境中。",sc_body_1_t:"艰难挣扎",sc_body_1_d:"症状频繁，精力低下，周期／激素信号不佳。",sc_body_2_t:"波动起伏",sc_body_2_d:"状态时好时坏；有一定稳定性，也有波动。",sc_body_3_t:"趋于稳定",sc_body_3_d:"大体稳定；仍有症状但可控。",sc_body_4_t:"兴旺",sc_body_4_d:"精力充沛，周期／激素平衡，症状极少。"
},
hi:{app_eyebrow:"पुनर्स्थापना ढांचा · ऑफ़लाइन सिस्टम",app_title:"संरचनात्मक भार",io_export_csv:"CSV निर्यात करें",io_export_pdf:"PDF निर्यात करें",io_import_csv:"CSV आयात करें",io_lang_label:"भाषा",io_reset:"अंग्रेज़ी पर रीसेट करें",
nav_dashboard:"डैशबोर्ड",nav_clients:"क्लाइंट्स",nav_signal:"सिग्नल लॉग",nav_integration:"एकीकरण",nav_framework:"ढांचा",nav_roadmap:"कार्यक्रम रोडमैप",nav_metrics:"संरचनात्मक मेट्रिक्स",
dash_eyebrow:"मूल सिद्धांत",dash_hook1:"थकान इस बात का प्रमाण नहीं है कि शरीर टूट गया है।",dash_hook2:"यह प्रमाण है कि भार डिज़ाइन की क्षमता से अधिक है।",dash_lede:"यह सिस्टम प्रत्येक महिला को एक क्लाइंट रिकॉर्ड के रूप में दर्ज करता है, लक्षणों को एक निर्धारित पैमाने पर संरचनात्मक संकेत के रूप में लॉग करता है, आत्मा-मन-शरीर को एक ही प्रणाली मानकर ट्रैक करता है, और हर प्रविष्टि के बाद परिणाम दिखाता है।",
dash_m_clients:"पंजीकृत क्लाइंट्स",dash_m_signals:"दर्ज सिग्नल",dash_m_avgload:"औसत संरचनात्मक भार",dash_m_avgspirit:"औसत आत्मा स्कोर",
svg_crosssection:"क्रॉस-सेक्शन · भार पथ",svg_demand:"मांग",svg_spirit:"आत्मा",svg_spirit_sub:"पहचान · डिज़ाइन · उद्देश्य",svg_soul:"मन",svg_soul_sub:"बुद्धि · भावना · इच्छा",svg_body:"शरीर",svg_body_sub:"हार्मोन · चक्र · तंत्रिका तंत्र",svg_support:"पाँच स्तंभ = भार-वहन सहारा",
clients_corenum:"क्लाइंट रजिस्ट्री",clients_h2:"क्लाइंट पंजीकृत करें और प्रबंधित करें",clients_lede:"जिस भी महिला की सहायता की जाती है, उसे यहाँ एक बार पंजीकृत किया जाता है, फिर सिग्नल या चेक-इन दर्ज करते समय ड्रॉपडाउन से चुना जाता है। रिकॉर्ड पूरी तरह संपादन योग्य हैं — किसी भी समय विवरण अपडेट करें या इतिहास सुधारें।",
clients_f_name:"पूरा नाम",clients_f_contact:"संपर्क (ईमेल / फ़ोन)",clients_f_start:"आरंभ तिथि",clients_f_status:"स्थिति",clients_f_note:"इनटेक नोट",clients_f_noteph:"इनटेक के समय की पृष्ठभूमि — वह क्या लेकर आई",clients_submit:"क्लाइंट पंजीकृत करें",
status_active:"सक्रिय",status_paused:"रुकी हुई",status_completed:"पूर्ण",clients_nocontact:"कोई संपर्क दर्ज नहीं",clients_started:"आरंभ",clients_edit:"संपादित करें",clients_view:"रिकॉर्ड देखें",clients_delete:"हटाएं",
mini_signals:"सिग्नल",mini_checkins:"चेक-इन",mini_avgload:"औसत भार",mini_avgspirit:"औसत आत्मा",mini_avgsoul:"औसत मन",mini_avgbody:"औसत शरीर",clients_intake_prefix:"इनटेक नोट:",save:"सहेजें",cancel:"रद्द करें",
confirm_delete_client:"इस क्लाइंट को हटाएं और उनकी दर्ज प्रविष्टियों को अलग करें? प्रविष्टियाँ बनी रहेंगी लेकिन असंबद्ध दिखेंगी।",clients_empty:"अभी तक कोई क्लाइंट पंजीकृत नहीं है। पहला रिकॉर्ड जोड़ने के लिए ऊपर दिए गए फ़ॉर्म का उपयोग करें।",
detail_signal_history:"सिग्नल इतिहास",detail_integration_history:"एकीकरण इतिहास",detail_no_signals:"इस क्लाइंट के लिए कोई सिग्नल दर्ज नहीं है।",detail_no_integrations:"इस क्लाइंट के लिए कोई चेक-इन दर्ज नहीं है।",
row_edit:"संपादित",row_del:"हटाएं",confirm_delete_signal:"यह सिग्नल हटाएं?",confirm_delete_integration:"यह चेक-इन हटाएं?",no_clients_registered:"अभी तक कोई क्लाइंट पंजीकृत नहीं है",
signal_corenum:"स्तंभ 01 + 05",signal_h2:"सिग्नल लॉग",signal_lede:"प्रत्येक लक्षण को संरचनात्मक भार पर स्कोर किया जाता है — यह कि शरीर कितना संकेत दे रहा है कि मांग डिज़ाइन की क्षमता से अधिक है, न कि केवल खराबी।",
label_client:"क्लाइंट",label_date:"तिथि",signal_f_symptom:"लक्षण / संकेत",signal_f_symptomph:"जैसे: दिमागी धुंध, अनियमित चक्र",signal_f_category:"श्रेणी",
cat_fatigue:"थकान",cat_hormonal:"हार्मोनल / चक्र",cat_cognitive:"संज्ञानात्मक",cat_spiritual:"आध्यात्मिक सूखापन",cat_overcommit:"अत्यधिक प्रतिबद्धता",cat_other:"अन्य",
signal_f_load:"संरचनात्मक भार",value_label:"मान:",legend_toggle:"हर सीमा का क्या अर्थ है?",signal_f_note:"नोट (उस सप्ताह क्या ढोया जा रहा था)",
btn_cancel_edit:"संपादन रद्द करें",btn_log_signal:"सिग्नल दर्ज करें",btn_update_signal:"सिग्नल अपडेट करें",filter_by_client:"क्लाइंट के अनुसार फ़िल्टर करें",filter_all_clients:"सभी क्लाइंट्स",
result_head:"परिणाम",result_structural_load:"संरचनात्मक भार",result_delta_vs_avg:"इस क्लाइंट के औसत की तुलना में",result_baseline_first_signal:"इस क्लाइंट के लिए पहला दर्ज सिग्नल — आधार रेखा स्थापित हो रही है।",
table_client:"क्लाइंट",table_date:"तिथि",table_signal:"सिग्नल",table_category:"श्रेणी",table_load:"भार",signal_empty:"अभी तक कोई सिग्नल दर्ज नहीं है।",alert_select_client:"पहले एक क्लाइंट पंजीकृत करें और चुनें।",
integration_corenum:"स्तंभ 02",integration_h2:"एकीकरण चेक-इन",integration_lede:"आत्मा, मन और शरीर को हर बार एक ही निर्धारित पैमाने पर एक साथ स्कोर किया जाता है — कभी अलग-अलग नहीं, क्योंकि यह ढांचा उन्हें एक प्रणाली मानता है।",
integration_f_note:"नोट",integration_f_noteph:"क्या बदला, और क्या नहीं बदला",label_spirit:"आत्मा",label_soul:"मन",label_body:"शरीर",btn_log_checkin:"चेक-इन दर्ज करें",btn_update_checkin:"चेक-इन अपडेट करें",
result_baseline_entry:"आधार रेखा प्रविष्टि",table_spirit:"आत्मा",table_soul:"मन",table_body:"शरीर",integration_empty:"अभी तक कोई चेक-इन दर्ज नहीं है।",
framework_corenum:"स्तंभ 03",framework_h2:"ढांचा संदर्भ",framework_lede:"वह स्थायी सिद्धांत जिस पर बाकी सिस्टम कार्य करता है।",
f1_title:"01 · निदान का पुनर्निर्धारण",f1_desc:"लक्षणों को निर्णय नहीं, संकेत के रूप में पढ़ा जाता है। थकान, हार्मोनल गड़बड़ी और दिमागी धुंध जैव-रासायनिक विफलता से पहले संरचनात्मक अतिभार का संकेत देते हैं।",
f2_title:"02 · एकीकरण सिद्धांत",f2_desc:"आत्मा, मन और शरीर को एक निरंतर परस्पर क्रियाशील प्रणाली के रूप में माना जाता है। कोई भी हस्तक्षेप किसी एक परत को अलग नहीं करता।",
f3_title:"03 · पद्धतिगत संश्लेषण",f3_desc:"हार्मोन शरीरक्रिया और चक्र साक्षरता को पहचान और डिज़ाइन की बाइबिल आधारित समझ के साथ जोड़ा जाता है। यह संयोजन ही परिवर्तन का माध्यम है।",
f4_title:"04 · प्रस्तुति संरचना",f4_desc:"तीन समन्वित प्रारूप: चार महीने का व्यक्तिगत कार्यक्रम, एक लिखित सिद्धांत (पुस्तक), और सामग्री व लाइव आयोजनों के माध्यम से सामूहिक निर्माण।",
f5_title:"05 · संरचनात्मक / सामाजिक दृष्टिकोण",f5_desc:"व्यापक महिला थकावट को एक संरचनात्मक परिघटना माना जाता है — बड़े पैमाने पर मांग का डिज़ाइन से अधिक होना — न कि व्यक्तिगत रूप से बिगड़े हार्मोन की महामारी।",
roadmap_corenum:"स्तंभ 04",roadmap_h2:"कार्यक्रम रोडमैप",roadmap_lede:"चार महीने, सोलह सप्ताह — व्यक्तिगत कार्यक्रम का आधार बनने वाला साझा टेम्पलेट।",
month1:"महीना 1 — मूल्यांकन और आधार तैयारी",month2:"महीना 2 — पुनर्संरेखण",month3:"महीना 3 — व्यवहार में एकीकरण",month4:"महीना 4 — सुदृढ़ीकरण",
m1w1:"आधार संरचनात्मक भार + चक्र इतिहास दर्ज करना",m1w2:"आत्मा-मन-शरीर का आधार स्कोर",m1w3:"वर्तमान प्रतिबद्धताओं की वास्तविक क्षमता से तुलना",m1w4:"ट्रैकिंग की लय स्थापित करना (सिग्नल लॉग + एकीकरण)",
m2w1:"हटाने या पुनर्गठित करने योग्य शीर्ष 3 संरचनात्मक अतिभार पहचानना",m2w2:"चक्र-अनुरूप जीवनशैली समायोजन शुरू करना",m2w3:"पहचान/डिज़ाइन अध्ययन आरंभ करना (आत्मा स्तर)",m2w4:"मध्य-बिंदु भार पुनर्मूल्यांकन",
m3w1:"वास्तविक जीवन के भार में समायोजन लागू करना (काम, परिवार)",m3w2:"कम हुए संरचनात्मक भार पर हार्मोनल प्रतिक्रिया ट्रैक करना",m3w3:"पहचान-कार्य से जुड़े आध्यात्मिक अभ्यास को गहरा करना",m3w4:"उन बिंदुओं को संबोधित करना जहाँ पुराना भार लौट आता है",
m4w1:"आधार रेखा की तुलना में आत्मा-मन-शरीर का पूर्ण पुनर्मूल्यांकन",m4w2:"एक स्थायी, दीर्घकालिक भार सीमा बनाना",m4w3:"कार्यक्रम के बाद की निरंतर लय निर्धारित करना",m4w4:"समापन समीक्षा और आगे की योजना",week_label:"सप्ताह",
metrics_corenum:"स्तंभ 05",metrics_h2:"संरचनात्मक मेट्रिक्स",metrics_lede:"सभी पंजीकृत क्लाइंट्स से एकत्रित — संरचनात्मक, सामाजिक स्तर का परिणाम।",
gauge_load:"संरचनात्मक भार सूचकांक (÷10)",gauge_integration:"आत्मा-मन-शरीर एकीकरण (÷10)",gauge_progress:"कार्यक्रम प्रगति",metrics_empty:"अभी तक कोई डेटा दर्ज नहीं है। इन मेट्रिक्स को भरने के लिए एक क्लाइंट पंजीकृत करें और प्रविष्टियाँ जोड़ें।",
footer_note:"ऑफ़लाइन सिस्टम। कोई नेटवर्क कॉल नहीं, कोई बाहरी फ़ॉन्ट या स्क्रिप्ट नहीं। डेटा केवल इस सत्र के लिए मेमोरी में रहता है — बंद करने से पहले निर्यात करें, और फिर से शुरू करने के लिए आयात करें। पैमाने के मान स्व-मूल्यांकन संदर्भ बिंदु हैं, नैदानिक उपकरण नहीं।",
print_title:"संरचनात्मक भार — क्लाइंट रिपोर्ट",print_generated:"तैयार किया गया",print_noclients:"अभी तक कोई क्लाइंट पंजीकृत नहीं है।",print_signal_section:"सिग्नल लॉग",print_integration_section:"एकीकरण चेक-इन",
print_no_signals:"कोई सिग्नल दर्ज नहीं।",print_no_checkins:"कोई चेक-इन दर्ज नहीं।",print_roadmap_section:"रोडमैप प्रगति",print_roadmap_text:"16-सप्ताह के रोडमैप का हिस्सा पूर्ण (साझा टेम्पलेट)।",
alert_csv_bad:"वह फ़ाइल पढ़ी नहीं जा सकी। इस सिस्टम से निर्यात की गई CSV फ़ाइल अपेक्षित है।",alert_pdf_import:"PDF फ़ाइलें आयात नहीं की जा सकतीं। PDF एक फ़ॉर्मेट की गई रिपोर्ट है, संरचित डेटा नहीं — ऑफ़लाइन इससे क्लाइंट रिकॉर्ड वापस निकालने का कोई भरोसेमंद तरीका नहीं है। सत्रों के बीच डेटा स्थानांतरित करने के लिए \u201cCSV निर्यात करें\u201d / \u201cCSV आयात करें\u201d का उपयोग करें, और PDF केवल रिपोर्ट प्रिंट या साझा करने के लिए।",
sc_load_0_t:"न्यूनतम",sc_load_0_d:"अलग-थलग घटना, क्षमता पर कोई वास्तविक दबाव नहीं।",sc_load_1_t:"हल्का",sc_load_1_d:"ध्यान देने योग्य पर संभालने योग्य; क्षमता थोड़ी परखी गई।",sc_load_2_t:"मध्यम",sc_load_2_d:"बार-बार होने वाला; मांग स्पष्ट रूप से क्षमता के बराबर या उससे अधिक है।",sc_load_3_t:"उच्च",sc_load_3_d:"बार-बार और बाधक; अधिकांश दिनों में भार डिज़ाइन क्षमता से अधिक है।",sc_load_4_t:"गंभीर",sc_load_4_d:"निरंतर और प्रणालीगत; शरीर तत्काल अतिभार का संकेत दे रहा है।",
sc_spirit_0_t:"विच्छिन्न",sc_spirit_0_d:"ईश्वर की उपस्थिति का कोई एहसास नहीं; विश्वास अनुपस्थित या दूर महसूस होता है।",sc_spirit_1_t:"दबा हुआ",sc_spirit_1_d:"ईश्वर की चाह का एहसास है पर उसके लिए वास्तविक स्थान नहीं बनाया जा रहा।",sc_spirit_2_t:"रुक-रुक कर",sc_spirit_2_d:"कुछ जुड़ाव, असंगत अभ्यास, आसानी से हट जाने वाला।",sc_spirit_3_t:"संलग्न",sc_spirit_3_d:"नियमित अभ्यास; ईश्वर में पहचान स्पष्ट होती जा रही है।",sc_spirit_4_t:"जड़ जमाई हुई",sc_spirit_4_d:"स्थिर संगति; परिस्थितियों के बावजूद पहचान सुरक्षित।",
sc_soul_0_t:"थका हुआ",sc_soul_0_d:"अभिभूत, प्रतिक्रियात्मक, कोई भावनात्मक या मानसिक गुंजाइश शेष नहीं।",sc_soul_1_t:"तनावग्रस्त",sc_soul_1_d:"चिड़चिड़ा, धुंधला, भंडार पर चल रहा।",sc_soul_2_t:"संभाल रहा",sc_soul_2_d:"कार्यक्षम पर थका हुआ; फल-फूल नहीं रहा, बस संभाल रहा है।",sc_soul_3_t:"स्पष्ट",sc_soul_3_d:"भावनात्मक रूप से संतुलित, मानसिक रूप से उपस्थित, इच्छा संलग्न।",sc_soul_4_t:"संपूर्ण",sc_soul_4_d:"शांत, लचीला, मन/भावना/इच्छा तालमेल में।",
sc_body_0_t:"संकट",sc_body_0_d:"गंभीर लक्षण; शरीर सक्रिय संकट में।",sc_body_1_t:"संघर्षरत",sc_body_1_d:"बार-बार लक्षण, कम ऊर्जा, खराब चक्र/हार्मोनल संकेत।",sc_body_2_t:"उतार-चढ़ाव",sc_body_2_d:"मिश्रित दिन; कुछ स्थिरता, कुछ गड़बड़ी।",sc_body_3_t:"स्थिर हो रहा",sc_body_3_d:"अधिकांशतः स्थिर; लक्षण मौजूद पर संभालने योग्य।",sc_body_4_t:"फल-फूल रहा",sc_body_4_d:"मजबूत ऊर्जा, संतुलित चक्र/हार्मोन, न्यूनतम लक्षण."
},
ru:{app_eyebrow:"Программа Восстановления · Офлайн-Система",app_title:"Структурная Нагрузка",io_export_csv:"Экспорт CSV",io_export_pdf:"Экспорт PDF",io_import_csv:"Импорт CSV",io_lang_label:"Язык",io_reset:"Сбросить на Английский",
nav_dashboard:"Панель",nav_clients:"Клиентки",nav_signal:"Журнал Сигналов",nav_integration:"Интеграция",nav_framework:"Концепция",nav_roadmap:"План Программы",nav_metrics:"Структурные Метрики",
dash_eyebrow:"Основной тезис",dash_hook1:"Усталость — не доказательство того, что тело сломано.",dash_hook2:"Это доказательство того, что нагрузка превышает расчётную мощность.",dash_lede:"Эта система регистрирует каждую женщину как карточку клиентки, фиксирует симптомы как структурный сигнал по заданной шкале, отслеживает дух, душу и тело как единую систему и выдаёт результат после каждой записи.",
dash_m_clients:"Зарегистрировано клиенток",dash_m_signals:"Записано сигналов",dash_m_avgload:"Средняя структурная нагрузка",dash_m_avgspirit:"Средний показатель духа",
svg_crosssection:"ПОПЕРЕЧНОЕ СЕЧЕНИЕ · ПУТЬ НАГРУЗКИ",svg_demand:"нагрузка",svg_spirit:"Дух",svg_spirit_sub:"идентичность · замысел · предназначение",svg_soul:"Душа",svg_soul_sub:"разум · эмоции · воля",svg_body:"Тело",svg_body_sub:"гормоны · цикл · нервная система",svg_support:"ПЯТЬ ОПОР = НЕСУЩАЯ ОСНОВА",
clients_corenum:"РЕЕСТР КЛИЕНТОК",clients_h2:"Регистрация и Управление Клиентками",clients_lede:"Каждая женщина регистрируется здесь один раз, а затем выбирается из выпадающего списка при записи сигналов или проверок. Записи полностью редактируемы — данные можно обновить, а историю исправить в любой момент.",
clients_f_name:"Полное имя",clients_f_contact:"Контакт (эл. почта / телефон)",clients_f_start:"Дата начала",clients_f_status:"Статус",clients_f_note:"Заметка при приёме",clients_f_noteph:"Контекст при приёме — что её привело",clients_submit:"Зарегистрировать Клиентку",
status_active:"Активна",status_paused:"Приостановлена",status_completed:"Завершена",clients_nocontact:"контакт не указан",clients_started:"Начато",clients_edit:"Изменить",clients_view:"Просмотр Записей",clients_delete:"Удалить",
mini_signals:"Сигналы",mini_checkins:"Проверки",mini_avgload:"Ср. Нагрузка",mini_avgspirit:"Ср. Дух",mini_avgsoul:"Ср. Душа",mini_avgbody:"Ср. Тело",clients_intake_prefix:"Заметка при приёме:",save:"Сохранить",cancel:"Отмена",
confirm_delete_client:"Удалить эту клиентку и отвязать её записи? Записи останутся, но будут отображаться как непривязанные.",clients_empty:"Пока нет зарегистрированных клиенток. Используйте форму выше, чтобы добавить первую запись.",
detail_signal_history:"История Сигналов",detail_integration_history:"История Интеграции",detail_no_signals:"Для этой клиентки не записано сигналов.",detail_no_integrations:"Для этой клиентки не записано проверок.",
row_edit:"Изм.",row_del:"Удал.",confirm_delete_signal:"Удалить этот сигнал?",confirm_delete_integration:"Удалить эту проверку?",no_clients_registered:"Пока нет зарегистрированных клиенток",
signal_corenum:"ОПОРА 01 + 05",signal_h2:"Журнал Сигналов",signal_lede:"Каждый симптом оценивается по структурной нагрузке — насколько тело сигнализирует, что нагрузка превышает расчётную мощность, а не просто указывает на сбой.",
label_client:"Клиентка",label_date:"Дата",signal_f_symptom:"Симптом / сигнал",signal_f_symptomph:"напр., туман в голове, нерегулярный цикл",signal_f_category:"Категория",
cat_fatigue:"Усталость",cat_hormonal:"Гормональное / Цикл",cat_cognitive:"Когнитивное",cat_spiritual:"Духовная сухость",cat_overcommit:"Перегруженность обязательствами",cat_other:"Другое",
signal_f_load:"Структурная нагрузка",value_label:"Значение:",legend_toggle:"Что означает каждый диапазон?",signal_f_note:"Заметка (что было тяжестью на той неделе)",
btn_cancel_edit:"Отменить Изменение",btn_log_signal:"Записать Сигнал",btn_update_signal:"Обновить Сигнал",filter_by_client:"Фильтр по клиентке",filter_all_clients:"Все клиентки",
result_head:"Результат",result_structural_load:"Структурная Нагрузка",result_delta_vs_avg:"относительно среднего этой клиентки",result_baseline_first_signal:"Первый зарегистрированный сигнал для этой клиентки — устанавливается базовый уровень.",
table_client:"Клиентка",table_date:"Дата",table_signal:"Сигнал",table_category:"Категория",table_load:"Нагрузка",signal_empty:"Пока нет записанных сигналов.",alert_select_client:"Сначала зарегистрируйте и выберите клиентку.",
integration_corenum:"ОПОРА 02",integration_h2:"Проверка Интеграции",integration_lede:"Дух, душа и тело каждый раз оцениваются вместе по единой шкале — никогда по отдельности, поскольку концепция рассматривает их как одну систему.",
integration_f_note:"Заметка",integration_f_noteph:"Что изменилось, а что нет",label_spirit:"Дух",label_soul:"Душа",label_body:"Тело",btn_log_checkin:"Записать Проверку",btn_update_checkin:"Обновить Проверку",
result_baseline_entry:"Базовая запись",table_spirit:"Дух",table_soul:"Душа",table_body:"Тело",integration_empty:"Пока нет записанных проверок.",
framework_corenum:"ОПОРА 03",framework_h2:"Справочник Концепции",framework_lede:"Фиксированная доктрина, на которой строится остальная система.",
f1_title:"01 · Переосмысление Диагноза",f1_desc:"Симптомы читаются как сигнал, а не как вердикт. Усталость, гормональные нарушения и туман в голове указывают на структурную перегрузку раньше, чем на биохимический сбой.",
f2_title:"02 · Принцип Интеграции",f2_desc:"Дух, душа и тело рассматриваются как единая непрерывно взаимодействующая система. Ни одно вмешательство не изолирует отдельный уровень.",
f3_title:"03 · Методологический Синтез",f3_desc:"Физиология гормонов и знание цикла сочетаются с библейским пониманием идентичности и замысла. Именно это сочетание является механизмом изменения.",
f4_title:"04 · Архитектура Реализации",f4_desc:"Три согласованных формата: четырёхмесячная индивидуальная программа, письменная доктрина (книга) и коллективное формирование через контент и живые мероприятия.",
f5_title:"05 · Структурное / Общественное Видение",f5_desc:"Массовое женское истощение рассматривается как структурное явление — нагрузка, превышающая расчётную мощность в масштабе, — а не как эпидемия индивидуально нарушенных гормонов.",
roadmap_corenum:"ОПОРА 04",roadmap_h2:"План Программы",roadmap_lede:"Четыре месяца, шестнадцать недель — общий шаблон, лежащий в основе индивидуальной программы.",
month1:"Месяц 1 — Оценка и Основа",month2:"Месяц 2 — Перенастройка",month3:"Месяц 3 — Интеграция на Практике",month4:"Месяц 4 — Закрепление",
m1w1:"Оценка исходной структурной нагрузки + история цикла",m1w2:"Базовая оценка духа-души-тела",m1w3:"Сопоставление текущих обязательств с реальной мощностью",m1w4:"Установление ритма отслеживания (Журнал Сигналов + Интеграция)",
m2w1:"Определение трёх главных структурных перегрузок для устранения или перестройки",m2w2:"Внедрение корректировок образа жизни с учётом цикла",m2w3:"Начало изучения идентичности/замысла (уровень духа)",m2w4:"Промежуточная переоценка нагрузки",
m3w1:"Применение корректировок в реальной нагрузке (работа, семья)",m3w2:"Отслеживание гормональной реакции на сниженную структурную нагрузку",m3w3:"Углубление духовной практики, связанной с работой над идентичностью",m3w4:"Работа с точками рецидива, где возвращается прежняя нагрузка",
m4w1:"Полная переоценка духа-души-тела относительно исходного уровня",m4w2:"Построение устойчивого, долгосрочного предела нагрузки",m4w3:"Определение постоянного ритма после завершения программы",m4w4:"Итоговый обзор и план на будущее",week_label:"Нед.",
metrics_corenum:"ОПОРА 05",metrics_h2:"Структурные Метрики",metrics_lede:"Сводные данные по всем зарегистрированным клиенткам — результат в структурном, общественном масштабе.",
gauge_load:"Индекс Структурной Нагрузки (÷10)",gauge_integration:"Интеграция Духа-Души-Тела (÷10)",gauge_progress:"Прогресс Программы",metrics_empty:"Данные пока не записаны. Зарегистрируйте клиентку и добавьте записи, чтобы заполнить эти метрики.",
footer_note:"Офлайн-система. Никаких сетевых запросов, внешних шрифтов или скриптов. Данные хранятся в памяти только в течение этой сессии — используйте экспорт перед закрытием и импорт для продолжения работы. Значения шкалы — это ориентиры самооценки, а не диагностические инструменты.",
print_title:"Структурная Нагрузка — Отчёт по Клиенткам",print_generated:"Сформировано",print_noclients:"Пока нет зарегистрированных клиенток.",print_signal_section:"Журнал Сигналов",print_integration_section:"Проверки Интеграции",
print_no_signals:"Сигналы не записаны.",print_no_checkins:"Проверки не записаны.",print_roadmap_section:"Прогресс Плана Программы",print_roadmap_text:"из 16-недельного плана завершено (общий шаблон).",
alert_csv_bad:"Не удалось прочитать этот файл. Ожидается CSV-файл, экспортированный из этой системы.",alert_pdf_import:"Файлы PDF нельзя импортировать. PDF — это оформленный отчёт, а не структурированные данные — надёжного офлайн-способа извлечь из него записи клиенток не существует. Используйте «Экспорт CSV» / «Импорт CSV» для переноса данных между сессиями, а PDF — только для печати или передачи отчёта.",
sc_load_0_t:"Минимальная",sc_load_0_d:"Единичный случай, реальной нагрузки на мощность нет.",sc_load_1_t:"Лёгкая",sc_load_1_d:"Заметна, но управляема; мощность слегка испытывается.",sc_load_2_t:"Умеренная",sc_load_2_d:"Повторяющаяся; нагрузка явно достигает или превышает мощность.",sc_load_3_t:"Высокая",sc_load_3_d:"Частая и разрушительная; нагрузка превышает расчётную мощность большинство дней.",sc_load_4_t:"Тяжёлая",sc_load_4_d:"Постоянная и системная; тело сигнализирует о срочной перегрузке.",
sc_spirit_0_t:"Отключена",sc_spirit_0_d:"Нет ощущения присутствия Бога; вера кажется отсутствующей или далёкой.",sc_spirit_1_t:"Вытеснена",sc_spirit_1_d:"Осознаёт тягу к Богу, но реального пространства для этого не выделяется.",sc_spirit_2_t:"Периодическая",sc_spirit_2_d:"Есть некоторая связь, практика непостоянна, легко вытесняется.",sc_spirit_3_t:"Вовлечена",sc_spirit_3_d:"Регулярная практика; идентичность в Боге становится яснее.",sc_spirit_4_t:"Укоренена",sc_spirit_4_d:"Устойчивое общение; идентичность прочна независимо от обстоятельств.",
sc_soul_0_t:"Истощена",sc_soul_0_d:"Подавлена, реактивна, эмоциональных и умственных ресурсов не осталось.",sc_soul_1_t:"Напряжена",sc_soul_1_d:"Раздражительна, в тумане, работает на резервах.",sc_soul_2_t:"Справляется",sc_soul_2_d:"Функциональна, но устала; скорее справляется, чем процветает.",sc_soul_3_t:"Ясная",sc_soul_3_d:"Эмоционально уравновешена, умственно присутствует, воля задействована.",sc_soul_4_t:"Целостная",sc_soul_4_d:"Спокойна, устойчива; разум, эмоции и воля в согласии.",
sc_body_0_t:"Кризис",sc_body_0_d:"Тяжёлые симптомы; тело в остром бедствии.",sc_body_1_t:"Борется",sc_body_1_d:"Частые симптомы, низкая энергия, плохие признаки цикла/гормонов.",sc_body_2_t:"Колеблется",sc_body_2_d:"Дни разные; есть и стабильность, и сбои.",sc_body_3_t:"Стабилизируется",sc_body_3_d:"В основном стабильно; симптомы есть, но управляемы.",sc_body_4_t:"Процветает",sc_body_4_d:"Сильная энергия, сбалансированный цикл/гормоны, минимум симптомов."
},
ja:{app_eyebrow:"回復フレームワーク · オフラインシステム",app_title:"構造的負荷",io_export_csv:"CSVをエクスポート",io_export_pdf:"PDFをエクスポート",io_import_csv:"CSVをインポート",io_lang_label:"言語",io_reset:"英語にリセット",
nav_dashboard:"ダッシュボード",nav_clients:"クライアント",nav_signal:"シグナル記録",nav_integration:"統合チェック",nav_framework:"フレームワーク",nav_roadmap:"プログラム ロードマップ",nav_metrics:"構造指標",
dash_eyebrow:"中心となる考え方",dash_hook1:"疲労は、体が壊れている証拠ではない。",dash_hook2:"それは、負荷が設計を超えている証拠である。",dash_lede:"このシステムは、それぞれの女性をクライアント記録として登録し、症状を定義された尺度に基づく構造的シグナルとして記録し、霊・魂・体を一つのシステムとして追跡し、記録のたびに結果を表示します。",
dash_m_clients:"登録クライアント数",dash_m_signals:"記録済みシグナル数",dash_m_avgload:"平均構造的負荷",dash_m_avgspirit:"平均スピリット・スコア",
svg_crosssection:"断面図 · 負荷経路",svg_demand:"需要",svg_spirit:"霊",svg_spirit_sub:"アイデンティティ · 設計 · 目的",svg_soul:"魂",svg_soul_sub:"知性 · 感情 · 意志",svg_body:"体",svg_body_sub:"ホルモン · 周期 · 神経系",svg_support:"5つの柱＝耐荷重の支え",
clients_corenum:"クライアント登録簿",clients_h2:"クライアントの登録と管理",clients_lede:"対応する女性は一度ここで登録し、その後シグナルやチェックインを記録する際にドロップダウンから選択します。記録はいつでも完全に編集可能です — 詳細の更新や履歴の修正がいつでもできます。",
clients_f_name:"氏名",clients_f_contact:"連絡先（メール／電話）",clients_f_start:"開始日",clients_f_status:"状態",clients_f_note:"受付時のメモ",clients_f_noteph:"受付時の背景 — 何がきっかけで来られたか",clients_submit:"クライアントを登録",
status_active:"進行中",status_paused:"一時休止",status_completed:"完了",clients_nocontact:"連絡先未登録",clients_started:"開始日",clients_edit:"編集",clients_view:"記録を見る",clients_delete:"削除",
mini_signals:"シグナル数",mini_checkins:"チェックイン数",mini_avgload:"平均負荷",mini_avgspirit:"平均・霊",mini_avgsoul:"平均・魂",mini_avgbody:"平均・体",clients_intake_prefix:"受付時のメモ：",save:"保存",cancel:"キャンセル",
confirm_delete_client:"このクライアントを削除し、記録済みの項目の紐付けを解除しますか？記録は残りますが、紐付けなしとして表示されます。",clients_empty:"まだ登録されたクライアントはいません。上のフォームから最初の記録を追加してください。",
detail_signal_history:"シグナル履歴",detail_integration_history:"統合履歴",detail_no_signals:"このクライアントのシグナル記録はまだありません。",detail_no_integrations:"このクライアントのチェックイン記録はまだありません。",
row_edit:"編集",row_del:"削除",confirm_delete_signal:"このシグナルを削除しますか？",confirm_delete_integration:"このチェックインを削除しますか？",no_clients_registered:"まだ登録されたクライアントはいません",
signal_corenum:"柱 01 + 05",signal_h2:"シグナル記録",signal_lede:"各症状は構造的負荷で評価されます — 単なる不調ではなく、需要が設計上の許容量を超えていることを体がどれだけ知らせているかを示します。",
label_client:"クライアント",label_date:"日付",signal_f_symptom:"症状／シグナル",signal_f_symptomph:"例：頭がぼんやりする、周期の乱れ",signal_f_category:"カテゴリー",
cat_fatigue:"疲労",cat_hormonal:"ホルモン／周期",cat_cognitive:"認知",cat_spiritual:"霊的な渇き",cat_overcommit:"過度な負担",cat_other:"その他",
signal_f_load:"構造的負荷",value_label:"値：",legend_toggle:"各範囲は何を意味しますか？",signal_f_note:"メモ（その週に抱えていたもの）",
btn_cancel_edit:"編集をキャンセル",btn_log_signal:"シグナルを記録",btn_update_signal:"シグナルを更新",filter_by_client:"クライアントで絞り込み",filter_all_clients:"すべてのクライアント",
result_head:"結果",result_structural_load:"構造的負荷",result_delta_vs_avg:"このクライアントの平均との比較",result_baseline_first_signal:"このクライアントの最初のシグナル記録です — 基準値を設定しています。",
table_client:"クライアント",table_date:"日付",table_signal:"シグナル",table_category:"カテゴリー",table_load:"負荷",signal_empty:"まだシグナルは記録されていません。",alert_select_client:"先にクライアントを登録して選択してください。",
integration_corenum:"柱 02",integration_h2:"統合チェックイン",integration_lede:"霊・魂・体は毎回、定義された尺度に基づいて一緒に評価されます — このフレームワークはそれらを一つのシステムとして扱うため、決して個別には評価しません。",
integration_f_note:"メモ",integration_f_noteph:"変化したこと、変化しなかったこと",label_spirit:"霊",label_soul:"魂",label_body:"体",btn_log_checkin:"チェックインを記録",btn_update_checkin:"チェックインを更新",
result_baseline_entry:"基準記録",table_spirit:"霊",table_soul:"魂",table_body:"体",integration_empty:"まだチェックインは記録されていません。",
framework_corenum:"柱 03",framework_h2:"フレームワーク参照",framework_lede:"システムの残りの部分が拠って立つ、固定された考え方。",
f1_title:"01・診断の再定義",f1_desc:"症状は判定ではなくシグナルとして読み取られます。疲労、ホルモンの乱れ、頭のぼんやり感は、生化学的な機能不全である前に、構造的な過負荷を示します。",
f2_title:"02・統合の原則",f2_desc:"霊・魂・体は、絶えず相互に作用し合う一つのシステムとして扱われます。どの介入も、単一の層だけを切り離して扱うことはありません。",
f3_title:"03・方法論的統合",f3_desc:"ホルモン生理学と周期に関する知識は、アイデンティティと設計に関する聖書的な理解と組み合わされます。この組み合わせこそが変化の仕組みです。",
f4_title:"04・提供の構造",f4_desc:"3つの連携した形式：4か月間の個別プログラム、成文化された教え（書籍）、コンテンツやライブイベントを通じた集団的な形成。",
f5_title:"05・構造的／社会的ビジョン",f5_desc:"広く見られる女性の疲弊は、個々のホルモンの不調が流行しているのではなく、需要が設計を大規模に超えるという構造的な現象として扱われます。",
roadmap_corenum:"柱 04",roadmap_h2:"プログラム ロードマップ",roadmap_lede:"4か月、16週間 — 個別プログラムの基盤となる共通テンプレート。",
month1:"1か月目 — 評価と基盤づくり",month2:"2か月目 — 再調整",month3:"3か月目 — 実践における統合",month4:"4か月目 — 定着",
m1w1:"基準となる構造的負荷＋周期歴の受付",m1w2:"霊・魂・体の基準スコアリング",m1w3:"現在の負担を実際の許容量と照らし合わせる",m1w4:"追跡のリズムを確立する（シグナル記録＋統合）",
m2w1:"取り除くか再構築すべき上位3つの構造的過負荷を特定する",m2w2:"周期に合わせた生活習慣の調整を導入する",m2w3:"アイデンティティ／設計の学びを始める（霊の層）",m2w4:"中間地点での負荷の再評価",
m3w1:"現実の負荷（仕事、家庭）の中で調整を適用する",m3w2:"構造的負荷が軽減されたことへのホルモン反応を追跡する",m3w3:"アイデンティティの取り組みと結びついた霊的実践を深める",m3w4:"以前の負荷が戻ってくる再発ポイントに対処する",
m4w1:"基準値と比較した霊・魂・体の全面的な再評価",m4w2:"持続可能で長続きする負荷の上限を築く",m4w3:"プログラム終了後も続くリズムを定める",m4w4:"終了レビューと今後の計画",week_label:"週",
metrics_corenum:"柱 05",metrics_h2:"構造指標",metrics_lede:"登録されたすべてのクライアントを集計 — 構造的・社会的規模での結果。",
gauge_load:"構造的負荷指数（÷10）",gauge_integration:"霊・魂・体の統合度（÷10）",gauge_progress:"プログラム進捗",metrics_empty:"まだデータが記録されていません。クライアントを登録し、記録を追加するとこれらの指標が表示されます。",
footer_note:"オフラインシステムです。ネットワーク通信、外部フォント、外部スクリプトは一切使用しません。データはこのセッション中のみメモリに保持されます — 閉じる前にエクスポートし、再開時にインポートしてください。尺度の値は自己評価の目安であり、診断ツールではありません。",
print_title:"構造的負荷 — クライアントレポート",print_generated:"作成日時",print_noclients:"まだ登録されたクライアントはいません。",print_signal_section:"シグナル記録",print_integration_section:"統合チェックイン",
print_no_signals:"シグナルは記録されていません。",print_no_checkins:"チェックインは記録されていません。",print_roadmap_section:"ロードマップ進捗",print_roadmap_text:"16週間のロードマップのうち完了（共通テンプレート）。",
alert_csv_bad:"そのファイルを読み込めませんでした。このシステムからエクスポートされたCSVファイルが必要です。",alert_pdf_import:"PDFファイルはインポートできません。PDFは整形されたレポートであり、構造化データではありません — オフラインでクライアント記録を確実に抽出する方法はありません。セッション間でデータを移動するには「CSVをエクスポート」／「CSVをインポート」を使用し、PDFは印刷やレポートの共有のみに使用してください。",
sc_load_0_t:"最小",sc_load_0_d:"孤立した出来事で、許容量への実質的な負担はない。",sc_load_1_t:"軽度",sc_load_1_d:"気づく程度だが対応可能；許容量が軽く試されている。",sc_load_2_t:"中程度",sc_load_2_d:"繰り返し発生；需要が明らかに許容量に達するか超えている。",sc_load_3_t:"高い",sc_load_3_d:"頻繁で支障が大きい；ほとんどの日で負荷が設計上の許容量を超えている。",sc_load_4_t:"重度",sc_load_4_d:"継続的かつ全身的；体が緊急の過負荷を知らせている。",
sc_spirit_0_t:"断絶",sc_spirit_0_d:"神の臨在を感じられない；信仰が不在または遠く感じられる。",sc_spirit_1_t:"押しのけられている",sc_spirit_1_d:"神を求める思いはあるが、そのための実際の余地が作られていない。",sc_spirit_2_t:"断続的",sc_spirit_2_d:"つながりはあるが実践が一定せず、簡単に後回しにされる。",sc_spirit_3_t:"取り組めている",sc_spirit_3_d:"定期的な実践があり、神における自分らしさが明確になりつつある。",sc_spirit_4_t:"根づいている",sc_spirit_4_d:"安定した交わりがあり、状況に関わらずアイデンティティが確かである。",
sc_soul_0_t:"消耗",sc_soul_0_d:"圧倒され、反応的で、感情的・精神的な余力が残っていない。",sc_soul_1_t:"張り詰めている",sc_soul_1_d:"苛立ちやすく、頭がぼんやりし、蓄えで動いている。",sc_soul_2_t:"何とか対処",sc_soul_2_d:"機能はしているが疲れている；伸び伸びとではなく何とかこなしている。",sc_soul_3_t:"クリア",sc_soul_3_d:"感情が安定し、精神的に落ち着いており、意志も働いている。",sc_soul_4_t:"満たされている",sc_soul_4_d:"穏やかで回復力があり、知性・感情・意志が調和している。",
sc_body_0_t:"危機的",sc_body_0_d:"深刻な症状があり、体が急性の苦痛の中にある。",sc_body_1_t:"苦戦",sc_body_1_d:"症状が頻繁で、エネルギーが低く、周期・ホルモンの兆候も良くない。",sc_body_2_t:"変動あり",sc_body_2_d:"日によって波がある；安定している日もあれば乱れる日もある。",sc_body_3_t:"安定しつつある",sc_body_3_d:"おおむね一定しており、症状はあるが対応可能。",sc_body_4_t:"好調",sc_body_4_d:"エネルギーが高く、周期・ホルモンも整い、症状はごくわずか。"
}
  };

  function t(key){
    const dict = T[currentLang] || T.en;
    if(dict && dict[key]!==undefined) return dict[key];
    if(T.en[key]!==undefined) return T.en[key];
    return key;
  }

  const SCALE_RANGES = [[1,2],[3,4],[5,6],[7,8],[9,10]];
  function bandIndex(val){
    for(let i=0;i<SCALE_RANGES.length;i++){ if(val>=SCALE_RANGES[i][0] && val<=SCALE_RANGES[i][1]) return i; }
    return 0;
  }
  function bandTitle(scaleKey, val){ return t(`sc_${scaleKey}_${bandIndex(val)}_t`); }
  function bandDesc(scaleKey, val){ return t(`sc_${scaleKey}_${bandIndex(val)}_d`); }
  function legendHtml(scaleKey){
    return SCALE_RANGES.map((r,i)=>`<div class="lg-row"><b>${r[0]}\u2013${r[1]}</b><span><strong style="color:var(--ink);">${t(`sc_${scaleKey}_${i}_t`)}</strong> \u2014 ${t(`sc_${scaleKey}_${i}_d`)}</span></div>`).join('');
  }
  function wireSlider(inputId, valId, descId, legendId, scaleKey){
    const el = document.getElementById(inputId);
    function update(){
      const v = Number(el.value);
      document.getElementById(valId).textContent = v;
      document.getElementById(descId).innerHTML = `<strong style="color:var(--ink);">${bandTitle(scaleKey,v)}</strong> \u2014 ${bandDesc(scaleKey,v)}`;
      document.getElementById(legendId).innerHTML = legendHtml(scaleKey);
    }
    el.addEventListener('input', update);
    el._i18nUpdate = update;
    update();
  }
  wireSlider('s-load','s-load-val','s-load-desc','s-load-legend','load');
  wireSlider('i-spirit','i-spirit-val','i-spirit-desc','i-spirit-legend','spirit');
  wireSlider('i-soul','i-soul-val','i-soul-desc','i-soul-legend','soul');
  wireSlider('i-body','i-body-val','i-body-desc','i-body-legend','body');

  // ---------- Roadmap seed (keys, not literal text) ----------
  const ROADMAP_SEED = [
    [1,'month1',['m1w1','m1w2','m1w3','m1w4']],
    [2,'month2',['m2w1','m2w2','m2w3','m2w4']],
    [3,'month3',['m3w1','m3w2','m3w3','m3w4']],
    [4,'month4',['m4w1','m4w2','m4w3','m4w4']]
  ];
  (function seed(){
    let week=1;
    ROADMAP_SEED.forEach(([m,monthKey,itemKeys])=>{
      itemKeys.forEach((key,i)=>{ state.roadmap.push({id:`${m}-${i}`, month:m, monthKey, week:week++, textKey:key, done:false}); });
    });
  })();

  // ---------- Static text application ----------
  function applyStaticText(){
    document.documentElement.setAttribute('lang', currentLang);
    document.documentElement.setAttribute('dir', RTL_LANGS.includes(currentLang) ? 'rtl' : 'ltr');
    document.querySelectorAll('[data-i18n]').forEach(el=>{ el.textContent = t(el.getAttribute('data-i18n')); });
    document.querySelectorAll('[data-i18n-ph]').forEach(el=>{ el.setAttribute('placeholder', t(el.getAttribute('data-i18n-ph'))); });
    document.getElementById('lang-select').setAttribute('aria-label', t('io_lang_label'));
    ['s-load','i-spirit','i-soul','i-body'].forEach(id=>{ const el = document.getElementById(id); if(el && el._i18nUpdate) el._i18nUpdate(); });
  }

  function populateLangSelect(){
    const sel = document.getElementById('lang-select');
    sel.innerHTML = LANGS.map(l=>`<option value="${l.code}">${l.name}</option>`).join('');
    sel.value = currentLang;
  }

  function setLanguage(code){
    currentLang = LANGS.some(l=>l.code===code) ? code : 'en';
    document.getElementById('lang-select').value = currentLang;
    applyStaticText();
    document.getElementById('signal-result').innerHTML = '';
    document.getElementById('integration-result').innerHTML = '';
    refreshClientSelects();
    renderClients();
    renderSignals();
    renderIntegrations();
    renderRoadmap();
    renderDashboard();
    renderMetrics();
  }

  document.getElementById('lang-select').addEventListener('change', e=> setLanguage(e.target.value));
  document.getElementById('btn-reset-lang').addEventListener('click', ()=> setLanguage('en'));

  // ---------- Tabs ----------
  document.querySelectorAll('.tab').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
      document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
      btn.classList.add('active');
      document.getElementById('panel-'+btn.dataset.panel).classList.add('active');
      if(btn.dataset.panel==='metrics') renderMetrics();
      if(btn.dataset.panel==='dashboard') renderDashboard();
      if(btn.dataset.panel==='clients') renderClients();
    });
  });

  const fmtDate = d => d ? new Date(d+'T00:00:00').toLocaleDateString(currentLang, {month:'short',day:'numeric',year:'numeric'}) : '';
  const avg = arr => arr.length ? (arr.reduce((a,b)=>a+b,0)/arr.length) : 0;
  function escapeHtml(str){ return String(str||'').replace(/[&<>"']/g, m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }
  function clientName(id){ const c = state.clients.find(c=>c.id===id); return c ? c.name : t('no_clients_registered'); }
  function statusLabel(status){ return t('status_'+String(status||'active').toLowerCase()); }
  function catLabel(cat){
    const map = {'Fatigue':'cat_fatigue','Hormonal / Cycle':'cat_hormonal','Cognitive':'cat_cognitive','Spiritual drought':'cat_spiritual','Overcommitment':'cat_overcommit','Other':'cat_other'};
    return map[cat] ? t(map[cat]) : (cat || '');
  }

  // ---------- Clients ----------
  function refreshClientSelects(){
    const opts = state.clients.length
      ? state.clients.map(c=>`<option value="${c.id}">${escapeHtml(c.name)}</option>`).join('')
      : `<option value="">${t('no_clients_registered')}</option>`;
    ['s-client','i-client'].forEach(id=>{ document.getElementById(id).innerHTML = opts; });
    document.getElementById('s-filter').innerHTML = `<option value="">${t('filter_all_clients')}</option>` + opts;
    document.getElementById('i-filter').innerHTML = `<option value="">${t('filter_all_clients')}</option>` + opts;
  }

  document.getElementById('form-client').addEventListener('submit', e=>{
    e.preventDefault();
    state.clients.push({
      id: uid(),
      name: document.getElementById('c-name').value,
      contact: document.getElementById('c-contact').value,
      start: document.getElementById('c-start').value,
      status: document.getElementById('c-status').value,
      note: document.getElementById('c-note').value
    });
    e.target.reset();
    refreshClientSelects(); renderClients(); renderDashboard();
  });

  function clientStats(clientId){
    const sigs = state.signals.filter(s=>s.clientId===clientId);
    const ints = state.integrations.filter(i=>i.clientId===clientId);
    return {
      sigCount: sigs.length, intCount: ints.length,
      avgLoad: avg(sigs.map(s=>s.load)),
      avgSpirit: avg(ints.map(i=>i.spirit)), avgSoul: avg(ints.map(i=>i.soul)), avgBody: avg(ints.map(i=>i.body))
    };
  }

  function renderClients(){
    const wrap = document.getElementById('clients-wrap');
    if(!state.clients.length){ wrap.innerHTML = `<div class="empty">${t('clients_empty')}</div>`; return; }
    wrap.innerHTML = state.clients.map(c=>{
      const st = clientStats(c.id);
      return `<div class="client-card" data-client="${c.id}">
        <div class="ch">
          <div>
            <h3>${escapeHtml(c.name)}</h3>
            <div class="meta">${escapeHtml(c.contact||t('clients_nocontact'))} \u00b7 ${t('clients_started')} ${fmtDate(c.start)} \u00b7 <span class="tag">${statusLabel(c.status)}</span></div>
          </div>
          <div class="row-actions">
            <button class="btn-xs ghost" data-act="edit-client" data-id="${c.id}">${t('clients_edit')}</button>
            <button class="btn-xs ghost" data-act="toggle-detail" data-id="${c.id}">${t('clients_view')}</button>
            <button class="btn-xs danger" data-act="delete-client" data-id="${c.id}">${t('clients_delete')}</button>
          </div>
        </div>
        <div class="client-mini">
          <div class="m"><span class="k">${t('mini_signals')}</span>${st.sigCount}</div>
          <div class="m"><span class="k">${t('mini_checkins')}</span>${st.intCount}</div>
          <div class="m"><span class="k">${t('mini_avgload')}</span>${st.sigCount?st.avgLoad.toFixed(1):'\u2014'}</div>
          <div class="m"><span class="k">${t('mini_avgspirit')}</span>${st.intCount?st.avgSpirit.toFixed(1):'\u2014'}</div>
          <div class="m"><span class="k">${t('mini_avgsoul')}</span>${st.intCount?st.avgSoul.toFixed(1):'\u2014'}</div>
          <div class="m"><span class="k">${t('mini_avgbody')}</span>${st.intCount?st.avgBody.toFixed(1):'\u2014'}</div>
        </div>
        ${c.note?`<div class="meta" style="margin-top:8px;">${t('clients_intake_prefix')} ${escapeHtml(c.note)}</div>`:''}
        <div class="detail" id="detail-${c.id}"></div>
      </div>`;
    }).join('');
    wrap.querySelectorAll('[data-act="edit-client"]').forEach(b=>b.addEventListener('click', ()=>editClient(b.dataset.id)));
    wrap.querySelectorAll('[data-act="delete-client"]').forEach(b=>b.addEventListener('click', ()=>deleteClient(b.dataset.id)));
    wrap.querySelectorAll('[data-act="toggle-detail"]').forEach(b=>b.addEventListener('click', ()=>toggleDetail(b.dataset.id)));
  }

  function editClient(id){
    const c = state.clients.find(c=>c.id===id);
    const card = document.querySelector(`.client-card[data-client="${id}"] .ch`);
    card.innerHTML = `
      <div style="width:100%;display:grid;grid-template-columns:repeat(2,1fr);gap:8px;">
        <input class="edit-input" id="e-name-${id}" value="${escapeHtml(c.name)}" placeholder="${t('clients_f_name')}">
        <input class="edit-input" id="e-contact-${id}" value="${escapeHtml(c.contact||'')}" placeholder="${t('clients_f_contact')}">
        <input class="edit-input" type="date" id="e-start-${id}" value="${c.start}">
        <select class="edit-input" id="e-status-${id}">
          ${['Active','Paused','Completed'].map(s=>`<option value="${s}" ${s===c.status?'selected':''}>${statusLabel(s)}</option>`).join('')}
        </select>
        <textarea class="edit-input" id="e-note-${id}" style="grid-column:1/-1;" rows="2">${escapeHtml(c.note||'')}</textarea>
        <div class="row-actions" style="grid-column:1/-1;">
          <button class="btn-xs primary" data-act="save-client" data-id="${id}">${t('save')}</button>
          <button class="btn-xs ghost" data-act="cancel-client" data-id="${id}">${t('cancel')}</button>
        </div>
      </div>`;
    card.querySelector('[data-act="save-client"]').addEventListener('click', ()=>{
      c.name = document.getElementById(`e-name-${id}`).value || c.name;
      c.contact = document.getElementById(`e-contact-${id}`).value;
      c.start = document.getElementById(`e-start-${id}`).value;
      c.status = document.getElementById(`e-status-${id}`).value;
      c.note = document.getElementById(`e-note-${id}`).value;
      refreshClientSelects(); renderClients();
    });
    card.querySelector('[data-act="cancel-client"]').addEventListener('click', renderClients);
  }

  function deleteClient(id){
    if(!confirm(t('confirm_delete_client'))) return;
    state.clients = state.clients.filter(c=>c.id!==id);
    refreshClientSelects(); renderClients(); renderDashboard(); renderSignals(); renderIntegrations();
  }

  function toggleDetail(id){
    const el = document.getElementById(`detail-${id}`);
    const wasOpen = el.classList.contains('open');
    document.querySelectorAll('.detail.open').forEach(d=>d.classList.remove('open'));
    if(wasOpen) return;
    el.classList.add('open');
    const sigs = state.signals.filter(s=>s.clientId===id).sort((a,b)=>(b.date||'').localeCompare(a.date||''));
    const ints = state.integrations.filter(i=>i.clientId===id).sort((a,b)=>(b.date||'').localeCompare(a.date||''));
    let html = `<div class="sub-h">${t('detail_signal_history')}</div>`;
    html += sigs.length ? `<table><thead><tr><th>${t('table_date')}</th><th>${t('table_signal')}</th><th>${t('table_category')}</th><th>${t('table_load')}</th><th></th></tr></thead><tbody>` +
      sigs.map(s=>`<tr><td>${fmtDate(s.date)}</td><td>${escapeHtml(s.symptom)}</td><td><span class="tag">${catLabel(s.cat)}</span></td><td>${s.load}/10</td>
        <td class="row-actions"><button class="btn-xs ghost" data-edit-sig="${s.id}">${t('row_edit')}</button><button class="btn-xs danger" data-del-sig="${s.id}">${t('row_del')}</button></td></tr>`).join('') +
      `</tbody></table>` : `<div class="empty">${t('detail_no_signals')}</div>`;
    html += `<div class="sub-h">${t('detail_integration_history')}</div>`;
    html += ints.length ? `<table><thead><tr><th>${t('table_date')}</th><th>${t('table_spirit')}</th><th>${t('table_soul')}</th><th>${t('table_body')}</th><th></th></tr></thead><tbody>` +
      ints.map(i=>`<tr><td>${fmtDate(i.date)}</td><td>${i.spirit}</td><td>${i.soul}</td><td>${i.body}</td>
        <td class="row-actions"><button class="btn-xs ghost" data-edit-int="${i.id}">${t('row_edit')}</button><button class="btn-xs danger" data-del-int="${i.id}">${t('row_del')}</button></td></tr>`).join('') +
      `</tbody></table>` : `<div class="empty">${t('detail_no_integrations')}</div>`;
    el.innerHTML = html;
    el.querySelectorAll('[data-edit-sig]').forEach(b=>b.addEventListener('click', ()=>startEditSignal(b.dataset.editSig)));
    el.querySelectorAll('[data-del-sig]').forEach(b=>b.addEventListener('click', ()=>deleteSignal(b.dataset.delSig)));
    el.querySelectorAll('[data-edit-int]').forEach(b=>b.addEventListener('click', ()=>startEditIntegration(b.dataset.editInt)));
    el.querySelectorAll('[data-del-int]').forEach(b=>b.addEventListener('click', ()=>deleteIntegration(b.dataset.delInt)));
  }

  // ---------- Signal Log ----------
  document.getElementById('form-signal').addEventListener('submit', e=>{
    e.preventDefault();
    const clientId = document.getElementById('s-client').value;
    if(!clientId){ alert(t('alert_select_client')); return; }
    const payload = {
      clientId, date: document.getElementById('s-date').value, symptom: document.getElementById('s-symptom').value,
      load: Number(document.getElementById('s-load').value), cat: document.getElementById('s-cat').value, note: document.getElementById('s-note').value
    };
    let entry;
    if(editingSignalId){
      const idx = state.signals.findIndex(s=>s.id===editingSignalId);
      state.signals[idx] = Object.assign({id:editingSignalId}, payload);
      entry = state.signals[idx];
      editingSignalId = null;
      document.getElementById('s-submit-btn').textContent = t('btn_log_signal');
      document.getElementById('s-cancel-edit').style.display = 'none';
    } else {
      entry = Object.assign({id:uid()}, payload);
      state.signals.push(entry);
    }
    e.target.reset();
    document.getElementById('s-load').value = 5; document.getElementById('s-load').dispatchEvent(new Event('input'));
    showSignalResult(entry);
    renderSignals(); renderDashboard(); renderClients();
  });
  document.getElementById('s-cancel-edit').addEventListener('click', ()=>{
    editingSignalId = null;
    document.getElementById('form-signal').reset();
    document.getElementById('s-submit-btn').textContent = t('btn_log_signal');
    document.getElementById('s-cancel-edit').style.display = 'none';
  });

  function showSignalResult(entry){
    const priorAvg = avg(state.signals.filter(s=>s.clientId===entry.clientId && s.id!==entry.id).map(s=>s.load));
    const hasPrior = state.signals.filter(s=>s.clientId===entry.clientId && s.id!==entry.id).length > 0;
    let delta;
    if(hasPrior){
      const diff = (entry.load - priorAvg).toFixed(1);
      delta = `<div class="rc-delta ${diff>0?'delta-down':'delta-up'}">${diff>0?'\u25b2':'\u25bc'} ${Math.abs(diff)} ${t('result_delta_vs_avg')} (${priorAvg.toFixed(1)})</div>`;
    } else {
      delta = `<div class="rc-delta">${t('result_baseline_first_signal')}</div>`;
    }
    document.getElementById('signal-result').innerHTML = `
      <div class="result-card">
        <div class="rc-head">${t('result_head')} \u2014 ${escapeHtml(clientName(entry.clientId))} \u00b7 ${fmtDate(entry.date)}</div>
        <div class="rc-grid">
          <div class="rc-item"><div class="v">${entry.load}/10</div><div class="k">${t('result_structural_load')}</div><div class="d"><strong>${bandTitle('load',entry.load)}</strong> \u2014 ${bandDesc('load',entry.load)}</div>${delta}</div>
        </div>
      </div>`;
  }

  function startEditSignal(id){
    const s = state.signals.find(s=>s.id===id);
    if(!s) return;
    document.querySelector('.tab[data-panel="signal"]').click();
    document.getElementById('s-client').value = s.clientId;
    document.getElementById('s-date').value = s.date;
    document.getElementById('s-symptom').value = s.symptom;
    document.getElementById('s-cat').value = s.cat;
    document.getElementById('s-load').value = s.load;
    document.getElementById('s-load').dispatchEvent(new Event('input'));
    document.getElementById('s-note').value = s.note;
    editingSignalId = id;
    document.getElementById('s-submit-btn').textContent = t('btn_update_signal');
    document.getElementById('s-cancel-edit').style.display = 'inline-block';
  }
  function deleteSignal(id){
    if(!confirm(t('confirm_delete_signal'))) return;
    state.signals = state.signals.filter(s=>s.id!==id);
    renderSignals(); renderDashboard(); renderClients();
  }

  document.getElementById('s-filter').addEventListener('change', renderSignals);

  function renderSignals(){
    const wrap = document.getElementById('signal-table-wrap');
    const filter = document.getElementById('s-filter').value;
    let list = filter ? state.signals.filter(s=>s.clientId===filter) : state.signals;
    if(!list.length){ wrap.innerHTML = `<div class="empty">${t('signal_empty')}</div>`; return; }
    const rows = [...list].sort((a,b)=>(b.date||'').localeCompare(a.date||''))
      .map(s=>`<tr><td>${escapeHtml(clientName(s.clientId))}</td><td>${fmtDate(s.date)}</td><td>${escapeHtml(s.symptom)}</td><td><span class="tag">${catLabel(s.cat)}</span></td><td>${s.load}/10</td>
        <td class="row-actions"><button class="btn-xs ghost" data-e="${s.id}">${t('row_edit')}</button><button class="btn-xs danger" data-d="${s.id}">${t('row_del')}</button></td></tr>`).join('');
    wrap.innerHTML = `<table><thead><tr><th>${t('table_client')}</th><th>${t('table_date')}</th><th>${t('table_signal')}</th><th>${t('table_category')}</th><th>${t('table_load')}</th><th></th></tr></thead><tbody>${rows}</tbody></table>`;
    wrap.querySelectorAll('[data-e]').forEach(b=>b.addEventListener('click', ()=>startEditSignal(b.dataset.e)));
    wrap.querySelectorAll('[data-d]').forEach(b=>b.addEventListener('click', ()=>deleteSignal(b.dataset.d)));
  }

  // ---------- Integration ----------
  document.getElementById('form-integration').addEventListener('submit', e=>{
    e.preventDefault();
    const clientId = document.getElementById('i-client').value;
    if(!clientId){ alert(t('alert_select_client')); return; }
    const payload = {
      clientId, date: document.getElementById('i-date').value,
      spirit: Number(document.getElementById('i-spirit').value), soul: Number(document.getElementById('i-soul').value), body: Number(document.getElementById('i-body').value),
      note: document.getElementById('i-note').value
    };
    let entry;
    if(editingIntegrationId){
      const idx = state.integrations.findIndex(i=>i.id===editingIntegrationId);
      state.integrations[idx] = Object.assign({id:editingIntegrationId}, payload);
      entry = state.integrations[idx];
      editingIntegrationId = null;
      document.getElementById('i-submit-btn').textContent = t('btn_log_checkin');
      document.getElementById('i-cancel-edit').style.display = 'none';
    } else {
      entry = Object.assign({id:uid()}, payload);
      state.integrations.push(entry);
    }
    e.target.reset();
    ['i-spirit','i-soul','i-body'].forEach(id=>{ document.getElementById(id).value=5; document.getElementById(id).dispatchEvent(new Event('input')); });
    showIntegrationResult(entry);
    renderIntegrations(); renderDashboard(); renderClients();
  });
  document.getElementById('i-cancel-edit').addEventListener('click', ()=>{
    editingIntegrationId = null;
    document.getElementById('form-integration').reset();
    document.getElementById('i-submit-btn').textContent = t('btn_log_checkin');
    document.getElementById('i-cancel-edit').style.display = 'none';
  });

  function showIntegrationResult(entry){
    const others = state.integrations.filter(i=>i.clientId===entry.clientId && i.id!==entry.id);
    const hasPrior = others.length>0;
    function item(key,labelKey,scaleKey){
      const val = entry[key];
      let deltaHtml;
      if(hasPrior){
        const d = (val - avg(others.map(o=>o[key]))).toFixed(1);
        deltaHtml = `<div class="rc-delta ${d>0?'delta-up':'delta-down'}">${d>0?'\u25b2':'\u25bc'} ${Math.abs(d)} ${t('result_delta_vs_avg')}</div>`;
      } else {
        deltaHtml = `<div class="rc-delta">${t('result_baseline_entry')}</div>`;
      }
      return `<div class="rc-item"><div class="v">${val}/10</div><div class="k">${t(labelKey)}</div><div class="d"><strong>${bandTitle(scaleKey,val)}</strong> \u2014 ${bandDesc(scaleKey,val)}</div>${deltaHtml}</div>`;
    }
    document.getElementById('integration-result').innerHTML = `
      <div class="result-card">
        <div class="rc-head">${t('result_head')} \u2014 ${escapeHtml(clientName(entry.clientId))} \u00b7 ${fmtDate(entry.date)}</div>
        <div class="rc-grid">${item('spirit','label_spirit','spirit')}${item('soul','label_soul','soul')}${item('body','label_body','body')}</div>
      </div>`;
  }

  function startEditIntegration(id){
    const i = state.integrations.find(i=>i.id===id);
    if(!i) return;
    document.querySelector('.tab[data-panel="integration"]').click();
    document.getElementById('i-client').value = i.clientId;
    document.getElementById('i-date').value = i.date;
    document.getElementById('i-note').value = i.note;
    ['spirit','soul','body'].forEach(k=>{ document.getElementById('i-'+k).value = i[k]; document.getElementById('i-'+k).dispatchEvent(new Event('input')); });
    editingIntegrationId = id;
    document.getElementById('i-submit-btn').textContent = t('btn_update_checkin');
    document.getElementById('i-cancel-edit').style.display = 'inline-block';
  }
  function deleteIntegration(id){
    if(!confirm(t('confirm_delete_integration'))) return;
    state.integrations = state.integrations.filter(i=>i.id!==id);
    renderIntegrations(); renderDashboard(); renderClients();
  }

  document.getElementById('i-filter').addEventListener('change', renderIntegrations);

  function renderIntegrations(){
    const wrap = document.getElementById('integration-table-wrap');
    const filter = document.getElementById('i-filter').value;
    let list = filter ? state.integrations.filter(i=>i.clientId===filter) : state.integrations;
    if(!list.length){ wrap.innerHTML = `<div class="empty">${t('integration_empty')}</div>`; return; }
    const rows = [...list].sort((a,b)=>(b.date||'').localeCompare(a.date||''))
      .map(i=>`<tr><td>${escapeHtml(clientName(i.clientId))}</td><td>${fmtDate(i.date)}</td><td>${i.spirit}</td><td>${i.soul}</td><td>${i.body}</td>
        <td class="row-actions"><button class="btn-xs ghost" data-e="${i.id}">${t('row_edit')}</button><button class="btn-xs danger" data-d="${i.id}">${t('row_del')}</button></td></tr>`).join('');
    wrap.innerHTML = `<table><thead><tr><th>${t('table_client')}</th><th>${t('table_date')}</th><th>${t('table_spirit')}</th><th>${t('table_soul')}</th><th>${t('table_body')}</th><th></th></tr></thead><tbody>${rows}</tbody></table>`;
    wrap.querySelectorAll('[data-e]').forEach(b=>b.addEventListener('click', ()=>startEditIntegration(b.dataset.e)));
    wrap.querySelectorAll('[data-d]').forEach(b=>b.addEventListener('click', ()=>deleteIntegration(b.dataset.d)));
  }

  // ---------- Roadmap ----------
  function renderRoadmap(){
    const wrap = document.getElementById('roadmap-wrap');
    let html=''; let lastMonth=null;
    state.roadmap.forEach(item=>{
      const monthLabel = item.monthKey ? t(item.monthKey) : (item.monthLabel||'');
      const text = item.textKey ? t(item.textKey) : (item.text||'');
      if(item.month!==lastMonth){ html+=`<div class="month-head">${escapeHtml(monthLabel)}</div>`; lastMonth=item.month; }
      html+=`<div class="week"><input type="checkbox" data-id="${item.id}" ${item.done?'checked':''}><div class="wk">${t('week_label')} ${item.week}</div><div class="txt ${item.done?'done':''}">${escapeHtml(text)}</div></div>`;
    });
    wrap.innerHTML = html;
    wrap.querySelectorAll('input[type=checkbox]').forEach(cb=>{
      cb.addEventListener('change', ()=>{ state.roadmap.find(r=>r.id===cb.dataset.id).done = cb.checked; renderRoadmap(); renderDashboard(); });
    });
  }

  // ---------- Dashboard ----------
  function renderDashboard(){
    const avgLoad = avg(state.signals.map(s=>s.load)).toFixed(1);
    const avgSpirit = avg(state.integrations.map(i=>i.spirit)).toFixed(1);
    document.getElementById('dash-metrics').innerHTML = `
      <div class="metric"><div class="num">${state.clients.length}</div><div class="lbl">${t('dash_m_clients')}</div></div>
      <div class="metric"><div class="num">${state.signals.length}</div><div class="lbl">${t('dash_m_signals')}</div></div>
      <div class="metric"><div class="num">${state.signals.length?avgLoad:'\u2014'}</div><div class="lbl">${t('dash_m_avgload')}</div></div>
      <div class="metric"><div class="num">${state.integrations.length?avgSpirit:'\u2014'}</div><div class="lbl">${t('dash_m_avgspirit')}</div></div>
    `;
    ['crosssection','demand','spirit','spirit-sub','soul','soul-sub','body','body-sub','support'].forEach(k=>{
      const el = document.getElementById('svg-'+k);
      if(el) el.textContent = t('svg_'+k.replace('-','_'));
    });
  }

  // ---------- Metrics tab ----------
  function renderMetrics(){
    const empty = document.getElementById('metrics-empty');
    const wrap = document.getElementById('gauge-wrap');
    if(!state.signals.length && !state.integrations.length){ wrap.innerHTML=''; empty.style.display='block'; return; }
    empty.style.display='none';
    const avgLoad = avg(state.signals.map(s=>s.load));
    const avgSpirit = avg(state.integrations.map(i=>i.spirit));
    const avgSoul = avg(state.integrations.map(i=>i.soul));
    const avgBody = avg(state.integrations.map(i=>i.body));
    const integrationIndex = avg([avgSpirit,avgSoul,avgBody]);
    const roadmapPct = Math.round((state.roadmap.filter(r=>r.done).length/state.roadmap.length)*100);
    function gauge(label, value, max){
      const pct = Math.min(100, Math.round((value/max)*100));
      return `<div class="gauge"><div class="val">${value?value.toFixed(1):'0.0'}</div><div class="lbl">${label}</div><div class="bar"><i style="width:${pct}%"></i></div></div>`;
    }
    wrap.innerHTML = gauge(t('gauge_load'), avgLoad, 10) + gauge(t('gauge_integration'), integrationIndex, 10) +
      `<div class="gauge"><div class="val">${roadmapPct}%</div><div class="lbl">${t('gauge_progress')}</div><div class="bar"><i style="width:${roadmapPct}%"></i></div></div>`;
  }

  // ---------- CSV export / import ----------
  const CSV_HEADERS = ['type','id','clientId','name','contact','start','status','note','date','symptom','cat','load','spirit','soul','body','monthKey','week','textKey','text','done'];
  function csvEscape(v){ v = (v===undefined||v===null) ? '' : String(v); if(/[",\n]/.test(v)) v = '"' + v.replace(/"/g,'""') + '"'; return v; }
  function csvRow(obj){ return CSV_HEADERS.map(h => csvEscape(obj[h])).join(','); }

  function buildCSV(){
    const lines = [CSV_HEADERS.join(',')];
    state.clients.forEach(c => lines.push(csvRow({type:'client', id:c.id, name:c.name, contact:c.contact, start:c.start, status:c.status, note:c.note})));
    state.signals.forEach(s => lines.push(csvRow({type:'signal', id:s.id, clientId:s.clientId, date:s.date, symptom:s.symptom, cat:s.cat, load:s.load, note:s.note})));
    state.integrations.forEach(i => lines.push(csvRow({type:'integration', id:i.id, clientId:i.clientId, date:i.date, spirit:i.spirit, soul:i.soul, body:i.body, note:i.note})));
    state.roadmap.forEach(r => lines.push(csvRow({type:'roadmap', id:r.id, monthKey:r.monthKey, week:r.week, textKey:r.textKey||'', text:r.textKey?'':(r.text||''), done:r.done})));
    return lines.join('\r\n');
  }

  function parseCSV(text){
    const rows=[]; let row=[]; let field=''; let inQuotes=false;
    for(let i=0;i<text.length;i++){
      const c = text[i];
      if(inQuotes){
        if(c==='"'){ if(text[i+1]==='"'){ field+='"'; i++; } else { inQuotes=false; } }
        else field+=c;
      } else {
        if(c==='"') inQuotes=true;
        else if(c===',') { row.push(field); field=''; }
        else if(c==='\r'){ }
        else if(c==='\n'){ row.push(field); rows.push(row); row=[]; field=''; }
        else field+=c;
      }
    }
    if(field.length || row.length){ row.push(field); rows.push(row); }
    return rows.filter(r => r.length>1 || r[0]!=='');
  }

  document.getElementById('btn-export-csv').addEventListener('click', ()=>{
    const blob = new Blob([buildCSV()], {type:'text/csv;charset=utf-8;'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url; a.download = `structural-load-${new Date().toISOString().slice(0,10)}.csv`;
    a.click(); URL.revokeObjectURL(url);
  });

  document.getElementById('file-import').addEventListener('change', e=>{
    const file = e.target.files[0]; if(!file) return;
    const isPDF = /\.pdf$/i.test(file.name) || file.type === 'application/pdf';
    if(isPDF){ alert(t('alert_pdf_import')); e.target.value = ''; return; }
    const reader = new FileReader();
    reader.onload = ev=>{
      try{
        const rows = parseCSV(ev.target.result);
        if(!rows.length) throw new Error('empty');
        const header = rows[0];
        const idx = {}; header.forEach((h,i)=> idx[h]=i);
        const newClients=[], newSignals=[], newIntegrations=[], newRoadmap=[];
        for(let r=1;r<rows.length;r++){
          const row = rows[r];
          const get = h => (idx[h]!==undefined ? row[idx[h]] : '');
          const type = get('type');
          if(type==='client'){
            newClients.push({ id:get('id')||uid(), name:get('name'), contact:get('contact'), start:get('start'), status:get('status')||'Active', note:get('note') });
          } else if(type==='signal'){
            newSignals.push({ id:get('id')||uid(), clientId:get('clientId'), date:get('date'), symptom:get('symptom'), cat:get('cat'), load:Number(get('load'))||1, note:get('note') });
          } else if(type==='integration'){
            newIntegrations.push({ id:get('id')||uid(), clientId:get('clientId'), date:get('date'), spirit:Number(get('spirit'))||1, soul:Number(get('soul'))||1, body:Number(get('body'))||1, note:get('note') });
          } else if(type==='roadmap'){
            const mk = get('monthKey'); const tk = get('textKey');
            newRoadmap.push({ id:get('id'), month:Number((get('id')||'1-0').split('-')[0])||1, monthKey: mk||undefined, monthLabel: mk?undefined:get('monthKey'), week:Number(get('week'))||1, textKey: tk||undefined, text: tk?undefined:get('text'), done: get('done')==='true' });
          }
        }
        if(!newClients.length && !newSignals.length && !newIntegrations.length && !newRoadmap.length) throw new Error('no recognizable rows');
        state.clients = newClients;
        state.signals = newSignals;
        state.integrations = newIntegrations;
        if(newRoadmap.length) state.roadmap = newRoadmap;
        refreshClientSelects(); renderClients(); renderSignals(); renderIntegrations(); renderRoadmap(); renderDashboard(); renderMetrics();
      }catch(err){ alert(t('alert_csv_bad') + ' (' + CSV_HEADERS.join(', ') + ')'); }
      e.target.value = '';
    };
    reader.readAsText(file);
  });

  // ---------- PDF export (formatted report via browser print) ----------
  document.getElementById('btn-export-pdf').addEventListener('click', ()=>{
    const report = document.getElementById('print-report');
    let html = `<h1>${t('print_title')}</h1><div class="p-sub">${t('print_generated')} ${new Date().toLocaleString(currentLang)}</div>`;
    if(!state.clients.length){
      html += `<p>${t('print_noclients')}</p>`;
    } else {
      state.clients.forEach(c=>{
        const sigs = state.signals.filter(s=>s.clientId===c.id).sort((a,b)=>(a.date||'').localeCompare(b.date||''));
        const ints = state.integrations.filter(i=>i.clientId===c.id).sort((a,b)=>(a.date||'').localeCompare(b.date||''));
        const st = clientStats(c.id);
        html += `<div class="p-client">
          <h2>${escapeHtml(c.name)}</h2>
          <div class="p-meta">${escapeHtml(c.contact||t('clients_nocontact'))} \u00b7 ${t('clients_started')} ${fmtDate(c.start)} \u00b7 ${statusLabel(c.status)}
            ${st.sigCount?` \u00b7 ${t('mini_avgload')} ${st.avgLoad.toFixed(1)}/10`:''}${st.intCount?` \u00b7 ${avg([st.avgSpirit,st.avgSoul,st.avgBody]).toFixed(1)}/10`:''}</div>
          ${c.note?`<div class="p-meta">${t('clients_intake_prefix')} ${escapeHtml(c.note)}</div>`:''}`;
        html += `<div class="p-section-title">${t('print_signal_section')}</div>`;
        html += sigs.length ? `<table><thead><tr><th>${t('table_date')}</th><th>${t('table_signal')}</th><th>${t('table_category')}</th><th>${t('table_load')}</th><th>${t('signal_f_note')}</th></tr></thead><tbody>${
          sigs.map(s=>`<tr><td>${fmtDate(s.date)}</td><td>${escapeHtml(s.symptom)}</td><td>${catLabel(s.cat)}</td><td>${s.load}/10</td><td>${escapeHtml(s.note||'')}</td></tr>`).join('')
        }</tbody></table>` : `<p style="font-size:11px;color:#777;">${t('print_no_signals')}</p>`;
        html += `<div class="p-section-title">${t('print_integration_section')}</div>`;
        html += ints.length ? `<table><thead><tr><th>${t('table_date')}</th><th>${t('table_spirit')}</th><th>${t('table_soul')}</th><th>${t('table_body')}</th><th>${t('integration_f_note')}</th></tr></thead><tbody>${
          ints.map(i=>`<tr><td>${fmtDate(i.date)}</td><td>${i.spirit}</td><td>${i.soul}</td><td>${i.body}</td><td>${escapeHtml(i.note||'')}</td></tr>`).join('')
        }</tbody></table>` : `<p style="font-size:11px;color:#777;">${t('print_no_checkins')}</p>`;
        html += `</div>`;
      });
    }
    const donePct = Math.round((state.roadmap.filter(r=>r.done).length/state.roadmap.length)*100);
    html += `<div class="p-section-title">${t('print_roadmap_section')}</div><p style="font-size:12px;">${donePct}% ${t('print_roadmap_text')}</p>`;
    report.innerHTML = html;
    window.print();
  });

  // ---------- Init ----------
  populateLangSelect();
  applyStaticText();
  refreshClientSelects();
  renderClients();
  renderSignals();
  renderIntegrations();
  renderRoadmap();
  renderDashboard();
})();
</script>
</body>
</html>
