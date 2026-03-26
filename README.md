# Food-License
Management Dashboard with an Automated Notification System
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FoodLicensePro — License Management Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:wght@300;400;500&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0c10;
    --surface: #111318;
    --surface2: #181c24;
    --border: #232834;
    --border2: #2e3547;
    --accent: #f5a623;
    --accent2: #f7c96a;
    --green: #22c55e;
    --red: #ef4444;
    --yellow: #f59e0b;
    --blue: #3b82f6;
    --text: #e8ecf2;
    --text2: #8b95a8;
    --text3: #4e5668;
    --font-display: 'Syne', sans-serif;
    --font-mono: 'DM Mono', monospace;
    --font-body: 'DM Sans', sans-serif;
    --radius: 10px;
    --shadow: 0 4px 24px rgba(0,0,0,0.5);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--font-body);
    font-size: 14px;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* SIDEBAR */
  .sidebar {
    position: fixed;
    left: 0; top: 0; bottom: 0;
    width: 240px;
    background: var(--surface);
    border-right: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    z-index: 100;
    transition: transform 0.3s ease;
  }
  .sidebar-logo {
    padding: 24px 20px 20px;
    border-bottom: 1px solid var(--border);
  }
  .logo-mark {
    display: flex; align-items: center; gap: 10px;
  }
  .logo-icon {
    width: 36px; height: 36px;
    background: var(--accent);
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
  }
  .logo-text {
    font-family: var(--font-display);
    font-weight: 800;
    font-size: 16px;
    line-height: 1.1;
    color: var(--text);
  }
  .logo-text span { color: var(--accent); }
  .logo-sub {
    font-size: 10px;
    color: var(--text3);
    font-family: var(--font-mono);
    letter-spacing: 0.08em;
    margin-top: 2px;
  }

  .sidebar-nav {
    flex: 1;
    padding: 16px 12px;
    overflow-y: auto;
  }
  .nav-section-label {
    font-family: var(--font-mono);
    font-size: 9px;
    letter-spacing: 0.12em;
    color: var(--text3);
    text-transform: uppercase;
    padding: 8px 8px 6px;
    margin-top: 8px;
  }
  .nav-item {
    display: flex; align-items: center; gap: 10px;
    padding: 9px 10px;
    border-radius: 8px;
    cursor: pointer;
    color: var(--text2);
    font-size: 13.5px;
    font-weight: 500;
    transition: all 0.15s;
    margin-bottom: 2px;
    position: relative;
  }
  .nav-item:hover { background: var(--surface2); color: var(--text); }
  .nav-item.active { background: rgba(245,166,35,0.12); color: var(--accent); }
  .nav-item.active::before {
    content: '';
    position: absolute;
    left: 0; top: 50%; transform: translateY(-50%);
    width: 3px; height: 60%; background: var(--accent);
    border-radius: 0 3px 3px 0;
  }
  .nav-icon { font-size: 16px; width: 20px; text-align: center; }
  .nav-badge {
    margin-left: auto;
    background: var(--red);
    color: #fff;
    font-size: 10px;
    font-family: var(--font-mono);
    font-weight: 500;
    padding: 1px 6px;
    border-radius: 20px;
  }
  .nav-badge.yellow { background: var(--yellow); color: #000; }

  .sidebar-footer {
    padding: 16px 12px;
    border-top: 1px solid var(--border);
  }
  .user-card {
    display: flex; align-items: center; gap: 10px;
    padding: 10px;
    border-radius: 8px;
    background: var(--surface2);
    cursor: pointer;
  }
  .user-avatar {
    width: 32px; height: 32px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--accent), #e07b00);
    display: flex; align-items: center; justify-content: center;
    font-weight: 700; font-size: 13px; color: #000;
    flex-shrink: 0;
  }
  .user-info { flex: 1; min-width: 0; }
  .user-name { font-weight: 600; font-size: 13px; color: var(--text); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .user-role { font-size: 11px; color: var(--text3); font-family: var(--font-mono); }

  /* MAIN */
  .main {
    margin-left: 240px;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }

  /* TOPBAR */
  .topbar {
    position: sticky; top: 0; z-index: 50;
    background: rgba(10,12,16,0.85);
    backdrop-filter: blur(16px);
    border-bottom: 1px solid var(--border);
    padding: 0 28px;
    height: 60px;
    display: flex; align-items: center; gap: 16px;
  }
  .topbar-title {
    font-family: var(--font-display);
    font-weight: 700;
    font-size: 18px;
  }
  .topbar-title span { color: var(--text2); font-weight: 400; font-size: 14px; font-family: var(--font-body); margin-left: 8px; }
  .topbar-spacer { flex: 1; }
  .search-box {
    display: flex; align-items: center; gap: 8px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 7px 12px;
    width: 220px;
  }
  .search-box input {
    background: none; border: none; outline: none;
    color: var(--text); font-family: var(--font-body); font-size: 13px;
    width: 100%;
  }
  .search-box input::placeholder { color: var(--text3); }
  .topbar-btn {
    display: flex; align-items: center; gap: 6px;
    padding: 7px 14px;
    background: var(--accent);
    color: #000;
    border: none; border-radius: 8px;
    cursor: pointer;
    font-family: var(--font-body);
    font-weight: 600; font-size: 13px;
    transition: opacity 0.15s;
  }
  .topbar-btn:hover { opacity: 0.88; }
  .notif-btn {
    position: relative;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    width: 36px; height: 36px;
    display: flex; align-items: center; justify-content: center;
    cursor: pointer; font-size: 16px;
    transition: border-color 0.15s;
  }
  .notif-btn:hover { border-color: var(--border2); }
  .notif-dot {
    position: absolute; top: 6px; right: 6px;
    width: 7px; height: 7px;
    background: var(--red); border-radius: 50%;
    border: 2px solid var(--bg);
  }

  /* CONTENT */
  .content { padding: 28px; flex: 1; }

  /* STATS ROW */
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
    margin-bottom: 28px;
  }
  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
    position: relative;
    overflow: hidden;
    transition: border-color 0.2s, transform 0.2s;
  }
  .stat-card:hover { border-color: var(--border2); transform: translateY(-1px); }
  .stat-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0; height: 3px;
  }
  .stat-card.total::before { background: var(--blue); }
  .stat-card.active::before { background: var(--green); }
  .stat-card.expiring::before { background: var(--yellow); }
  .stat-card.expired::before { background: var(--red); }
  .stat-label {
    font-family: var(--font-mono); font-size: 10px;
    letter-spacing: 0.1em; text-transform: uppercase; color: var(--text3);
    margin-bottom: 12px;
  }
  .stat-value {
    font-family: var(--font-display); font-size: 36px; font-weight: 800;
    line-height: 1; margin-bottom: 6px;
  }
  .stat-change { font-size: 12px; color: var(--text2); }
  .stat-change .up { color: var(--green); }
  .stat-change .down { color: var(--red); }
  .stat-icon {
    position: absolute; right: 20px; top: 50%;
    transform: translateY(-50%);
    font-size: 36px; opacity: 0.08;
  }

  /* MAIN GRID */
  .main-grid {
    display: grid;
    grid-template-columns: 1fr 340px;
    gap: 20px;
    margin-bottom: 28px;
  }

  /* TABLE CARD */
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    overflow: hidden;
  }
  .card-header {
    padding: 18px 20px;
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: 12px;
  }
  .card-title {
    font-family: var(--font-display); font-weight: 700; font-size: 15px;
  }
  .card-subtitle { font-size: 12px; color: var(--text2); margin-top: 2px; }
  .card-actions { margin-left: auto; display: flex; gap: 8px; }
  .btn-sm {
    padding: 5px 12px; border-radius: 6px;
    font-size: 12px; font-weight: 500;
    cursor: pointer; transition: all 0.15s;
    border: 1px solid var(--border2);
    background: var(--surface2); color: var(--text2);
  }
  .btn-sm:hover { color: var(--text); border-color: var(--border2); background: #232834; }
  .btn-sm.active { background: rgba(245,166,35,0.1); border-color: var(--accent); color: var(--accent); }

  /* TABLE */
  .table-wrap { overflow-x: auto; }
  table { width: 100%; border-collapse: collapse; }
  thead th {
    padding: 10px 16px;
    font-family: var(--font-mono); font-size: 10px;
    letter-spacing: 0.1em; text-transform: uppercase;
    color: var(--text3); text-align: left;
    border-bottom: 1px solid var(--border);
    white-space: nowrap;
  }
  tbody tr {
    border-bottom: 1px solid var(--border);
    transition: background 0.12s;
    cursor: pointer;
  }
  tbody tr:last-child { border-bottom: none; }
  tbody tr:hover { background: var(--surface2); }
  tbody td {
    padding: 12px 16px;
    font-size: 13px; color: var(--text);
    vertical-align: middle;
  }
  .td-name { font-weight: 600; }
  .td-id { font-family: var(--font-mono); font-size: 11px; color: var(--text3); }
  .td-type { font-size: 12px; color: var(--text2); }

  .badge {
    display: inline-flex; align-items: center; gap: 4px;
    padding: 3px 9px; border-radius: 20px;
    font-size: 11px; font-weight: 600; font-family: var(--font-mono);
    text-transform: uppercase; letter-spacing: 0.04em;
  }
  .badge-active { background: rgba(34,197,94,0.12); color: var(--green); }
  .badge-expiring { background: rgba(245,158,11,0.15); color: var(--yellow); }
  .badge-expired { background: rgba(239,68,68,0.12); color: var(--red); }
  .badge-pending { background: rgba(59,130,246,0.12); color: var(--blue); }
  .badge::before { content: '•'; font-size: 14px; }

  .days-left {
    font-family: var(--font-mono); font-size: 12px;
    font-weight: 500;
  }
  .days-left.ok { color: var(--green); }
  .days-left.warn { color: var(--yellow); }
  .days-left.danger { color: var(--red); }

  .action-btn {
    background: none; border: 1px solid var(--border);
    color: var(--text2); padding: 4px 10px;
    border-radius: 6px; font-size: 11px; cursor: pointer;
    transition: all 0.15s; white-space: nowrap;
  }
  .action-btn:hover { border-color: var(--accent); color: var(--accent); }

  /* NOTIFICATION PANEL */
  .notif-panel { display: flex; flex-direction: column; }
  .notif-list { padding: 8px; overflow-y: auto; max-height: 420px; }
  .notif-item {
    display: flex; gap: 12px; align-items: flex-start;
    padding: 12px; border-radius: 8px;
    margin-bottom: 4px;
    transition: background 0.12s; cursor: pointer;
    border: 1px solid transparent;
  }
  .notif-item:hover { background: var(--surface2); border-color: var(--border); }
  .notif-item.unread { background: rgba(245,166,35,0.04); border-color: rgba(245,166,35,0.15); }
  .notif-dot2 {
    width: 8px; height: 8px; border-radius: 50%;
    flex-shrink: 0; margin-top: 5px;
  }
  .notif-dot2.red { background: var(--red); }
  .notif-dot2.yellow { background: var(--yellow); }
  .notif-dot2.green { background: var(--green); }
  .notif-dot2.blue { background: var(--blue); }
  .notif-content { flex: 1; }
  .notif-title { font-size: 13px; font-weight: 600; margin-bottom: 2px; }
  .notif-msg { font-size: 12px; color: var(--text2); line-height: 1.4; }
  .notif-time { font-size: 10px; color: var(--text3); font-family: var(--font-mono); margin-top: 4px; }
  .notif-unread-dot {
    width: 6px; height: 6px; background: var(--accent);
    border-radius: 50%; flex-shrink: 0; margin-top: 7px;
  }

  /* AUTOMATION SECTION */
  .auto-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    margin-bottom: 28px;
  }
  .auto-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 20px;
    transition: border-color 0.2s;
  }
  .auto-card:hover { border-color: var(--border2); }
  .auto-header { display: flex; align-items: center; gap: 10px; margin-bottom: 14px; }
  .auto-icon {
    width: 36px; height: 36px; border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px; flex-shrink: 0;
  }
  .auto-icon.orange { background: rgba(245,166,35,0.15); }
  .auto-icon.green { background: rgba(34,197,94,0.12); }
  .auto-icon.blue { background: rgba(59,130,246,0.12); }
  .auto-title { font-weight: 700; font-size: 14px; font-family: var(--font-display); }
  .auto-desc { font-size: 12px; color: var(--text2); margin-bottom: 14px; line-height: 1.5; }
  .auto-status {
    display: flex; align-items: center; gap: 6px;
    font-size: 12px; font-family: var(--font-mono);
  }
  .pulse-dot {
    width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0;
    background: var(--green);
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(0.85); }
  }
  .auto-next { color: var(--text3); font-size: 11px; margin-top: 4px; font-family: var(--font-mono); }

  /* TOGGLE */
  .toggle {
    position: relative; width: 36px; height: 20px;
    background: var(--border2); border-radius: 20px;
    cursor: pointer; transition: background 0.2s; flex-shrink: 0;
  }
  .toggle.on { background: var(--green); }
  .toggle::after {
    content: '';
    position: absolute;
    left: 3px; top: 3px;
    width: 14px; height: 14px;
    background: #fff; border-radius: 50%;
    transition: left 0.2s;
  }
  .toggle.on::after { left: 19px; }

  /* MODAL OVERLAY */
  .modal-overlay {
    display: none;
    position: fixed; inset: 0; z-index: 200;
    background: rgba(0,0,0,0.7);
    backdrop-filter: blur(4px);
    align-items: center; justify-content: center;
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: var(--surface);
    border: 1px solid var(--border2);
    border-radius: 14px;
    width: 560px; max-width: 95vw;
    max-height: 90vh; overflow-y: auto;
    padding: 28px;
    animation: modalIn 0.2s ease;
  }
  @keyframes modalIn {
    from { opacity: 0; transform: translateY(12px) scale(0.98); }
    to { opacity: 1; transform: none; }
  }
  .modal-title {
    font-family: var(--font-display); font-weight: 800; font-size: 20px;
    margin-bottom: 6px;
  }
  .modal-sub { font-size: 13px; color: var(--text2); margin-bottom: 24px; }
  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-bottom: 14px; }
  .form-row.full { grid-template-columns: 1fr; }
  .form-group { display: flex; flex-direction: column; gap: 6px; }
  .form-label { font-size: 12px; font-family: var(--font-mono); color: var(--text2); letter-spacing: 0.05em; }
  .form-input {
    background: var(--surface2);
    border: 1px solid var(--border2);
    border-radius: 8px; padding: 9px 12px;
    color: var(--text); font-family: var(--font-body); font-size: 13px;
    outline: none; transition: border-color 0.15s;
  }
  .form-input:focus { border-color: var(--accent); }
  .form-input option { background: var(--surface2); }
  .modal-footer { display: flex; gap: 10px; justify-content: flex-end; margin-top: 20px; }
  .btn-cancel {
    padding: 9px 18px; border-radius: 8px;
    background: var(--surface2); border: 1px solid var(--border2);
    color: var(--text2); cursor: pointer; font-family: var(--font-body);
    font-size: 13px; font-weight: 500; transition: all 0.15s;
  }
  .btn-cancel:hover { color: var(--text); }
  .btn-primary {
    padding: 9px 20px; border-radius: 8px;
    background: var(--accent); border: none;
    color: #000; cursor: pointer; font-family: var(--font-body);
    font-size: 13px; font-weight: 700; transition: opacity 0.15s;
  }
  .btn-primary:hover { opacity: 0.88; }

  /* TOAST */
  .toast-container {
    position: fixed; top: 20px; right: 20px; z-index: 999;
    display: flex; flex-direction: column; gap: 8px;
    pointer-events: none;
  }
  .toast {
    background: var(--surface);
    border: 1px solid var(--border2);
    border-radius: 10px; padding: 12px 16px;
    display: flex; align-items: center; gap: 10px;
    font-size: 13px; pointer-events: all;
    animation: toastIn 0.3s ease;
    min-width: 280px; max-width: 340px;
    box-shadow: var(--shadow);
  }
  .toast.success { border-left: 3px solid var(--green); }
  .toast.warning { border-left: 3px solid var(--yellow); }
  .toast.error { border-left: 3px solid var(--red); }
  @keyframes toastIn {
    from { opacity: 0; transform: translateX(20px); }
    to { opacity: 1; transform: none; }
  }

  /* CHART BAR */
  .chart-bar-row {
    display: flex; align-items: center; gap: 10px;
    margin-bottom: 10px;
  }
  .chart-bar-label { font-size: 12px; color: var(--text2); width: 80px; flex-shrink: 0; }
  .chart-bar-track { flex: 1; background: var(--surface2); border-radius: 4px; height: 8px; overflow: hidden; }
  .chart-bar-fill { height: 100%; border-radius: 4px; transition: width 1s ease; }
  .chart-bar-val { font-size: 12px; color: var(--text2); font-family: var(--font-mono); width: 28px; text-align: right; }

  /* SCHEDULER */
  .sched-row {
    display: flex; align-items: center; gap: 12px;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
  }
  .sched-row:last-child { border-bottom: none; }
  .sched-time {
    font-family: var(--font-mono); font-size: 12px;
    color: var(--accent); width: 60px;
  }
  .sched-name { font-size: 13px; font-weight: 500; flex: 1; }
  .sched-status { font-size: 11px; font-family: var(--font-mono); color: var(--text3); }

  /* HAMBURGER */
  .hamburger {
    display: none;
    background: var(--surface); border: 1px solid var(--border);
    border-radius: 8px; width: 36px; height: 36px;
    align-items: center; justify-content: center;
    cursor: pointer; font-size: 18px;
  }

  /* RESPONSIVE */
  @media (max-width: 1100px) {
    .stats-grid { grid-template-columns: repeat(2, 1fr); }
    .main-grid { grid-template-columns: 1fr; }
    .auto-grid { grid-template-columns: 1fr 1fr; }
  }
  @media (max-width: 780px) {
    .sidebar { transform: translateX(-100%); }
    .sidebar.open { transform: none; }
    .main { margin-left: 0; }
    .hamburger { display: flex; }
    .stats-grid { grid-template-columns: 1fr 1fr; }
    .auto-grid { grid-template-columns: 1fr; }
    .content { padding: 16px; }
    .form-row { grid-template-columns: 1fr; }
  }
  @media (max-width: 500px) {
    .stats-grid { grid-template-columns: 1fr; }
  }

  /* SCROLLBAR */
  ::-webkit-scrollbar { width: 5px; height: 5px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 10px; }

  /* TABS */
  .tabs { display: flex; gap: 2px; padding: 0 20px; border-bottom: 1px solid var(--border); }
  .tab {
    padding: 12px 16px; font-size: 13px; font-weight: 500; cursor: pointer;
    color: var(--text2); border-bottom: 2px solid transparent;
    transition: all 0.15s; margin-bottom: -1px;
  }
  .tab:hover { color: var(--text); }
  .tab.active { color: var(--accent); border-bottom-color: var(--accent); }

  /* EMPTY */
  .empty-state {
    text-align: center; padding: 48px 20px; color: var(--text3);
  }
  .empty-state .icon { font-size: 40px; margin-bottom: 12px; opacity: 0.4; }
  .empty-state p { font-size: 13px; }

  /* PAGE SECTIONS */
  .section { display: none; }
  .section.active { display: block; }

  /* DETAIL PANEL */
  .detail-row { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid var(--border); }
  .detail-row:last-child { border-bottom: none; }
  .detail-key { font-family: var(--font-mono); font-size: 11px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.08em; }
  .detail-val { font-size: 13px; font-weight: 500; }

  /* PROGRESS RING */
  .ring-wrap { position: relative; width: 80px; height: 80px; }
  .ring-wrap svg { transform: rotate(-90deg); }
  .ring-label {
    position: absolute; inset: 0;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    font-family: var(--font-display); font-weight: 800; font-size: 18px; line-height: 1;
  }
  .ring-sub { font-size: 9px; font-family: var(--font-mono); color: var(--text3); }
</style>
</head>
<body>

<!-- SIDEBAR -->
<aside class="sidebar" id="sidebar">
  <div class="sidebar-logo">
    <div class="logo-mark">
      <div class="logo-icon">🍽️</div>
      <div>
        <div class="logo-text">Food<span>License</span>Pro</div>
        <div class="logo-sub">MANAGEMENT SYSTEM v2.4</div>
      </div>
    </div>
  </div>
  <nav class="sidebar-nav">
    <div class="nav-section-label">Overview</div>
    <div class="nav-item active" onclick="navigate('dashboard', this)">
      <span class="nav-icon">⊞</span> Dashboard
    </div>
    <div class="nav-item" onclick="navigate('licenses', this)">
      <span class="nav-icon">📋</span> All Licenses
      <span class="nav-badge">142</span>
    </div>
    <div class="nav-section-label">Alerts</div>
    <div class="nav-item" onclick="navigate('expiring', this)">
      <span class="nav-icon">⏰</span> Expiring Soon
      <span class="nav-badge yellow">18</span>
    </div>
    <div class="nav-item" onclick="navigate('expired', this)">
      <span class="nav-icon">🚫</span> Expired
      <span class="nav-badge">7</span>
    </div>
    <div class="nav-section-label">Automation</div>
    <div class="nav-item" onclick="navigate('notifications', this)">
      <span class="nav-icon">🔔</span> Notifications
    </div>
    <div class="nav-item" onclick="navigate('scheduler', this)">
      <span class="nav-icon">⚙️</span> Scheduler
    </div>
    <div class="nav-section-label">Data</div>
    <div class="nav-item" onclick="navigate('analytics', this)">
      <span class="nav-icon">📊</span> Analytics
    </div>
    <div class="nav-item" onclick="navigate('clients', this)">
      <span class="nav-icon">👥</span> Clients
    </div>
    <div class="nav-item" onclick="navigate('settings', this)">
      <span class="nav-icon">⚙️</span> Settings
    </div>
  </nav>
  <div class="sidebar-footer">
    <div class="user-card">
      <div class="user-avatar">A</div>
      <div class="user-info">
        <div class="user-name">Admin User</div>
        <div class="user-role">SUPER ADMIN</div>
      </div>
      <span style="color:var(--text3);font-size:12px">⋯</span>
    </div>
  </div>
</aside>

<!-- MAIN -->
<main class="main">
  <!-- TOPBAR -->
  <header class="topbar">
    <div class="hamburger" id="hamburger" onclick="toggleSidebar()">☰</div>
    <div class="topbar-title" id="topbar-title">Dashboard <span>Overview</span></div>
    <div class="topbar-spacer"></div>
    <div class="search-box">
      <span style="color:var(--text3);font-size:14px">🔍</span>
      <input type="text" placeholder="Search licenses, clients..." id="search-input" oninput="handleSearch(this.value)">
    </div>
    <div class="notif-btn" onclick="navigate('notifications', document.querySelector('.nav-item:nth-child(9)'))">
      🔔<span class="notif-dot"></span>
    </div>
    <button class="topbar-btn" onclick="openModal()">+ Add License</button>
  </header>

  <!-- CONTENT -->
  <div class="content">

    <!-- DASHBOARD SECTION -->
    <div class="section active" id="section-dashboard">
      <!-- Stats -->
      <div class="stats-grid">
        <div class="stat-card total">
          <div class="stat-label">Total Licenses</div>
          <div class="stat-value" style="color:var(--blue)">142</div>
          <div class="stat-change">+<span class="up">12</span> this month</div>
          <div class="stat-icon">📋</div>
        </div>
        <div class="stat-card active">
          <div class="stat-label">Active</div>
          <div class="stat-value" style="color:var(--green)">117</div>
          <div class="stat-change"><span class="up">82.3%</span> of total</div>
          <div class="stat-icon">✅</div>
        </div>
        <div class="stat-card expiring">
          <div class="stat-label">Expiring in 30 Days</div>
          <div class="stat-value" style="color:var(--yellow)">18</div>
          <div class="stat-change"><span class="down">+5</span> from last month</div>
          <div class="stat-icon">⏳</div>
        </div>
        <div class="stat-card expired">
          <div class="stat-label">Expired</div>
          <div class="stat-value" style="color:var(--red)">7</div>
          <div class="stat-change"><span class="down">Needs renewal</span></div>
          <div class="stat-icon">🚫</div>
        </div>
      </div>

      <!-- Main grid -->
      <div class="main-grid">
        <!-- License Table -->
        <div class="card">
          <div class="card-header">
            <div>
              <div class="card-title">Recent Licenses</div>
              <div class="card-subtitle">Sorted by expiration date</div>
            </div>
            <div class="card-actions">
              <button class="btn-sm active" onclick="filterTable('all', this)">All</button>
              <button class="btn-sm" onclick="filterTable('expiring', this)">Expiring</button>
              <button class="btn-sm" onclick="filterTable('expired', this)">Expired</button>
            </div>
          </div>
          <div class="table-wrap">
            <table id="main-table">
              <thead>
                <tr>
                  <th>Client</th>
                  <th>License #</th>
                  <th>Type</th>
                  <th>Expiry</th>
                  <th>Days Left</th>
                  <th>Status</th>
                  <th></th>
                </tr>
              </thead>
              <tbody id="main-tbody"></tbody>
            </table>
          </div>
        </div>

        <!-- Notification Panel -->
        <div class="card notif-panel">
          <div class="card-header">
            <div>
              <div class="card-title">🔔 Notifications</div>
              <div class="card-subtitle">Automated alerts</div>
            </div>
            <div class="card-actions">
              <button class="btn-sm" onclick="markAllRead()">Mark all read</button>
            </div>
          </div>
          <div class="notif-list" id="notif-list"></div>
        </div>
      </div>

      <!-- Automation cards -->
      <div class="auto-grid">
        <div class="auto-card">
          <div class="auto-header">
            <div class="auto-icon orange">⏰</div>
            <div>
              <div class="auto-title">30-Day Reminder</div>
            </div>
            <div class="toggle on" id="toggle-30" onclick="toggleSwitch(this,'30-day reminder')" style="margin-left:auto"></div>
          </div>
          <div class="auto-desc">Automatically sends email & SMS reminders to clients 30 days before license expiration.</div>
          <div class="auto-status"><div class="pulse-dot"></div> Running</div>
          <div class="auto-next">Next run: Today at 09:00 AM</div>
        </div>
        <div class="auto-card">
          <div class="auto-header">
            <div class="auto-icon orange">📅</div>
            <div>
              <div class="auto-title">7-Day Final Alert</div>
            </div>
            <div class="toggle on" id="toggle-7" onclick="toggleSwitch(this,'7-day alert')" style="margin-left:auto"></div>
          </div>
          <div class="auto-desc">Escalated final reminder sent 7 days before expiry with urgent action required message.</div>
          <div class="auto-status"><div class="pulse-dot"></div> Running</div>
          <div class="auto-next">Next run: Today at 09:00 AM</div>
        </div>
        <div class="auto-card">
          <div class="auto-header">
            <div class="auto-icon green">🚫</div>
            <div>
              <div class="auto-title">Expiry Notice</div>
            </div>
            <div class="toggle on" id="toggle-exp" onclick="toggleSwitch(this,'expiry notice')" style="margin-left:auto"></div>
          </div>
          <div class="auto-desc">Immediate alert sent on the day of expiration with renewal instructions and deadlines.</div>
          <div class="auto-status"><div class="pulse-dot"></div> Running</div>
          <div class="auto-next">Next run: Today at 09:00 AM</div>
        </div>
      </div>
    </div>

    <!-- ALL LICENSES SECTION -->
    <div class="section" id="section-licenses">
      <div class="card">
        <div class="card-header">
          <div>
            <div class="card-title">All Licenses</div>
            <div class="card-subtitle" id="license-count-label">142 records</div>
          </div>
          <div class="card-actions">
            <button class="btn-sm" onclick="exportCSV()">⬇ Export CSV</button>
            <button class="topbar-btn" onclick="openModal()" style="padding:5px 12px;font-size:12px">+ Add</button>
          </div>
        </div>
        <div class="tabs">
          <div class="tab active" onclick="filterSection('all', this)">All (142)</div>
          <div class="tab" onclick="filterSection('active', this)">Active (117)</div>
          <div class="tab" onclick="filterSection('expiring', this)">Expiring (18)</div>
          <div class="tab" onclick="filterSection('expired', this)">Expired (7)</div>
        </div>
        <div class="table-wrap">
          <table id="all-table">
            <thead>
              <tr>
                <th>Client Name</th>
                <th>License #</th>
                <th>Category</th>
                <th>Issued</th>
                <th>Expires</th>
                <th>Days</th>
                <th>Status</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody id="all-tbody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- EXPIRING SECTION -->
    <div class="section" id="section-expiring">
      <div class="card">
        <div class="card-header">
          <div>
            <div class="card-title">⏰ Expiring Soon</div>
            <div class="card-subtitle">Licenses expiring within 30 days — requires attention</div>
          </div>
          <div class="card-actions">
            <button class="topbar-btn" onclick="sendBulkReminder()" style="padding:6px 14px;font-size:12px">📨 Send All Reminders</button>
          </div>
        </div>
        <div class="table-wrap">
          <table>
            <thead>
              <tr>
                <th>Client</th>
                <th>License #</th>
                <th>Type</th>
                <th>Expiry Date</th>
                <th>Days Remaining</th>
                <th>Last Notified</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody id="expiring-tbody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- EXPIRED SECTION -->
    <div class="section" id="section-expired">
      <div class="card">
        <div class="card-header">
          <div>
            <div class="card-title">🚫 Expired Licenses</div>
            <div class="card-subtitle">These licenses have passed their expiry date</div>
          </div>
        </div>
        <div class="table-wrap">
          <table>
            <thead>
              <tr>
                <th>Client</th>
                <th>License #</th>
                <th>Type</th>
                <th>Expired On</th>
                <th>Days Overdue</th>
                <th>Status</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody id="expired-tbody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- NOTIFICATIONS SECTION -->
    <div class="section" id="section-notifications">
      <div style="display:grid;grid-template-columns:1fr 320px;gap:20px">
        <div class="card">
          <div class="card-header">
            <div>
              <div class="card-title">Notification Log</div>
              <div class="card-subtitle">All automated and manual notifications</div>
            </div>
            <div class="card-actions">
              <button class="btn-sm" onclick="markAllRead()">Mark all read</button>
            </div>
          </div>
          <div id="full-notif-list" style="padding:8px;max-height:600px;overflow-y:auto"></div>
        </div>
        <div class="card" style="align-self:start">
          <div class="card-header"><div class="card-title">📨 Templates</div></div>
          <div style="padding:16px;display:flex;flex-direction:column;gap:10px">
            <div style="background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:14px">
              <div style="font-weight:600;font-size:13px;margin-bottom:6px;color:var(--yellow)">⏰ 30-Day Reminder</div>
              <div style="font-size:12px;color:var(--text2);line-height:1.5">"Your food license #{LIC_NO} is expiring on {DATE}. Please renew to avoid disruption."</div>
            </div>
            <div style="background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:14px">
              <div style="font-weight:600;font-size:13px;margin-bottom:6px;color:var(--red)">🚨 7-Day Urgent</div>
              <div style="font-size:12px;color:var(--text2);line-height:1.5">"URGENT: Your license #{LIC_NO} expires in 7 days. Immediate renewal required."</div>
            </div>
            <div style="background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:14px">
              <div style="font-weight:600;font-size:13px;margin-bottom:6px;color:var(--red)">🚫 Expired Notice</div>
              <div style="font-size:12px;color:var(--text2);line-height:1.5">"Your license #{LIC_NO} has EXPIRED. Operating without a valid license may result in penalties."</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- SCHEDULER SECTION -->
    <div class="section" id="section-scheduler">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px">
        <div class="card">
          <div class="card-header">
            <div class="card-title">⚙️ Scheduled Tasks</div>
          </div>
          <div style="padding:8px 16px">
            <div class="sched-row"><div class="sched-time">00:00</div><div class="sched-name">Database Backup</div><div class="sched-status">Daily ✓</div></div>
            <div class="sched-row"><div class="sched-time">06:00</div><div class="sched-name">License Expiry Scan</div><div class="sched-status">Daily ✓</div></div>
            <div class="sched-row"><div class="sched-time">09:00</div><div class="sched-name">Send 30-Day Reminders</div><div class="sched-status">Daily ✓</div></div>
            <div class="sched-row"><div class="sched-time">09:05</div><div class="sched-name">Send 7-Day Alerts</div><div class="sched-status">Daily ✓</div></div>
            <div class="sched-row"><div class="sched-time">09:10</div><div class="sched-name">Expired License Notices</div><div class="sched-status">Daily ✓</div></div>
            <div class="sched-row"><div class="sched-time">12:00</div><div class="sched-name">Midday Status Check</div><div class="sched-status">Daily ✓</div></div>
            <div class="sched-row"><div class="sched-time">18:00</div><div class="sched-name">Evening Report Email</div><div class="sched-status">Daily ✓</div></div>
            <div class="sched-row"><div class="sched-time">Sun 08:00</div><div class="sched-name">Weekly Analytics Report</div><div class="sched-status">Weekly ✓</div></div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">📊 Automation Stats</div></div>
          <div style="padding:20px">
            <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:20px">
              <div style="background:var(--surface2);border-radius:8px;padding:14px;text-align:center">
                <div style="font-family:var(--font-display);font-size:28px;font-weight:800;color:var(--green)">1,842</div>
                <div style="font-size:11px;color:var(--text3);font-family:var(--font-mono);margin-top:4px">EMAILS SENT</div>
              </div>
              <div style="background:var(--surface2);border-radius:8px;padding:14px;text-align:center">
                <div style="font-family:var(--font-display);font-size:28px;font-weight:800;color:var(--blue)">624</div>
                <div style="font-size:11px;color:var(--text3);font-family:var(--font-mono);margin-top:4px">SMS SENT</div>
              </div>
              <div style="background:var(--surface2);border-radius:8px;padding:14px;text-align:center">
                <div style="font-family:var(--font-display);font-size:28px;font-weight:800;color:var(--yellow)">97.4%</div>
                <div style="font-size:11px;color:var(--text3);font-family:var(--font-mono);margin-top:4px">UPTIME</div>
              </div>
              <div style="background:var(--surface2);border-radius:8px;padding:14px;text-align:center">
                <div style="font-family:var(--font-display);font-size:28px;font-weight:800;color:var(--accent)">84%</div>
                <div style="font-size:11px;color:var(--text3);font-family:var(--font-mono);margin-top:4px">RENEWAL RATE</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ANALYTICS SECTION -->
    <div class="section" id="section-analytics">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px">
        <div class="card">
          <div class="card-header"><div class="card-title">License Type Distribution</div></div>
          <div style="padding:20px" id="analytics-chart"></div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">Monthly Renewals (2024)</div></div>
          <div style="padding:20px" id="renewals-chart"></div>
        </div>
      </div>
    </div>

    <!-- CLIENTS SECTION -->
    <div class="section" id="section-clients">
      <div class="card">
        <div class="card-header">
          <div class="card-title">Clients</div>
          <div class="card-actions"><button class="topbar-btn" onclick="openModal()" style="padding:5px 12px;font-size:12px">+ Add Client</button></div>
        </div>
        <div class="table-wrap">
          <table>
            <thead>
              <tr><th>Name</th><th>Business</th><th>Email</th><th>Phone</th><th>Licenses</th><th>Actions</th></tr>
            </thead>
            <tbody id="clients-tbody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- SETTINGS SECTION -->
    <div class="section" id="section-settings">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px">
        <div class="card">
          <div class="card-header"><div class="card-title">Notification Settings</div></div>
          <div style="padding:20px;display:flex;flex-direction:column;gap:16px">
            <div style="display:flex;justify-content:space-between;align-items:center;padding:12px;background:var(--surface2);border-radius:8px">
              <div><div style="font-weight:600;font-size:13px">Email Notifications</div><div style="font-size:12px;color:var(--text2)">Send via SMTP</div></div>
              <div class="toggle on" onclick="toggleSwitch(this,'Email notifications')"></div>
            </div>
            <div style="display:flex;justify-content:space-between;align-items:center;padding:12px;background:var(--surface2);border-radius:8px">
              <div><div style="font-weight:600;font-size:13px">SMS Notifications</div><div style="font-size:12px;color:var(--text2)">via Twilio</div></div>
              <div class="toggle on" onclick="toggleSwitch(this,'SMS notifications')"></div>
            </div>
            <div style="display:flex;justify-content:space-between;align-items:center;padding:12px;background:var(--surface2);border-radius:8px">
              <div><div style="font-weight:600;font-size:13px">30-day Advance Reminder</div><div style="font-size:12px;color:var(--text2)">First warning</div></div>
              <div class="toggle on" onclick="toggleSwitch(this,'30-day reminder')"></div>
            </div>
            <div style="display:flex;justify-content:space-between;align-items:center;padding:12px;background:var(--surface2);border-radius:8px">
              <div><div style="font-weight:600;font-size:13px">7-day Urgent Alert</div><div style="font-size:12px;color:var(--text2)">Escalated reminder</div></div>
              <div class="toggle on" onclick="toggleSwitch(this,'7-day alert')"></div>
            </div>
            <div style="display:flex;justify-content:space-between;align-items:center;padding:12px;background:var(--surface2);border-radius:8px">
              <div><div style="font-weight:600;font-size:13px">On-Expiry Notice</div><div style="font-size:12px;color:var(--text2)">Day-of alert</div></div>
              <div class="toggle on" onclick="toggleSwitch(this,'expiry notice')"></div>
            </div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><div class="card-title">System Configuration</div></div>
          <div style="padding:20px;display:flex;flex-direction:column;gap:14px">
            <div class="form-group">
              <label class="form-label">Admin Email</label>
              <input class="form-input" type="email" value="admin@foodlicensepro.com">
            </div>
            <div class="form-group">
              <label class="form-label">SMTP Server</label>
              <input class="form-input" type="text" value="smtp.sendgrid.net">
            </div>
            <div class="form-group">
              <label class="form-label">Database Provider</label>
              <select class="form-input">
                <option>PostgreSQL (Cloud)</option>
                <option>MySQL</option>
                <option>MongoDB Atlas</option>
              </select>
            </div>
            <div class="form-group">
              <label class="form-label">Timezone</label>
              <select class="form-input">
                <option>Asia/Kolkata (IST)</option>
                <option>UTC</option>
                <option>America/New_York</option>
              </select>
            </div>
            <button class="btn-primary" style="align-self:flex-start;margin-top:4px" onclick="showToast('Settings saved successfully.','success')">Save Configuration</button>
          </div>
        </div>
      </div>
    </div>

  </div><!-- end content -->
</main>

<!-- ADD LICENSE MODAL -->
<div class="modal-overlay" id="modal-overlay">
  <div class="modal">
    <div class="modal-title">Add New License</div>
    <div class="modal-sub">Fill in the details to register a new food license.</div>
    <div class="form-row">
      <div class="form-group">
        <label class="form-label">Client Name *</label>
        <input class="form-input" id="f-name" type="text" placeholder="e.g. Ramesh Kumar">
      </div>
      <div class="form-group">
        <label class="form-label">Business Name *</label>
        <input class="form-input" id="f-business" type="text" placeholder="e.g. Ramesh Foods Pvt Ltd">
      </div>
    </div>
    <div class="form-row">
      <div class="form-group">
        <label class="form-label">Email Address *</label>
        <input class="form-input" id="f-email" type="email" placeholder="client@email.com">
      </div>
      <div class="form-group">
        <label class="form-label">Phone Number</label>
        <input class="form-input" id="f-phone" type="tel" placeholder="+91 98765 43210">
      </div>
    </div>
    <div class="form-row">
      <div class="form-group">
        <label class="form-label">License Category *</label>
        <select class="form-input" id="f-type">
          <option value="">Select category</option>
          <option>Restaurant</option>
          <option>Food Processing</option>
          <option>Catering</option>
          <option>Street Food</option>
          <option>Bakery</option>
          <option>Dairy</option>
          <option>Meat Processing</option>
          <option>Import/Export</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">License Number *</label>
        <input class="form-input" id="f-licno" type="text" placeholder="e.g. FSSAI-2024-00142">
      </div>
    </div>
    <div class="form-row">
      <div class="form-group">
        <label class="form-label">Issue Date *</label>
        <input class="form-input" id="f-issued" type="date">
      </div>
      <div class="form-group">
        <label class="form-label">Expiry Date *</label>
        <input class="form-input" id="f-expiry" type="date">
      </div>
    </div>
    <div class="form-row full">
      <div class="form-group">
        <label class="form-label">Address</label>
        <input class="form-input" id="f-address" type="text" placeholder="Business address">
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn-cancel" onclick="closeModal()">Cancel</button>
      <button class="btn-primary" onclick="addLicense()">Register License</button>
    </div>
  </div>
</div>

<!-- TOAST CONTAINER -->
<div class="toast-container" id="toast-container"></div>

<script>
// =============================================
// DATA STORE (simulates scalable DB)
// =============================================
const today = new Date();
function daysFromNow(d) {
  return Math.round((new Date(d) - today) / 86400000);
}

const licenses = [
  { id:1, client:"Priya Sharma", business:"Priya's Kitchen", email:"priya@kitchen.com", phone:"+91 98100 11234", type:"Restaurant", licNo:"FSSAI-2024-00101", issued:"2023-01-10", expiry: fmtDate(15), status:"expiring" },
  { id:2, client:"Arjun Mehta", business:"Mehta Catering Co.", email:"arjun@mehta.com", phone:"+91 87654 32109", type:"Catering", licNo:"FSSAI-2024-00102", issued:"2022-05-15", expiry: fmtDate(3), status:"expiring" },
  { id:3, client:"Sunita Devi", business:"Sunita Dairy Farm", email:"sunita@dairy.com", phone:"+91 76543 21098", type:"Dairy", licNo:"FSSAI-2024-00103", issued:"2022-08-20", expiry: fmtDate(-12), status:"expired" },
  { id:4, client:"Ramesh Kumar", business:"Kumar Bakery", email:"ramesh@bakery.com", phone:"+91 65432 10987", type:"Bakery", licNo:"FSSAI-2024-00104", issued:"2023-03-01", expiry: fmtDate(180), status:"active" },
  { id:5, client:"Ananya Singh", business:"Singh Foods Pvt Ltd", email:"ananya@singh.com", phone:"+91 54321 09876", type:"Food Processing", licNo:"FSSAI-2024-00105", issued:"2023-07-12", expiry: fmtDate(-5), status:"expired" },
  { id:6, client:"Vikram Patel", business:"Patel Street Eats", email:"vikram@patel.com", phone:"+91 43210 98765", type:"Street Food", licNo:"FSSAI-2024-00106", issued:"2023-11-01", expiry: fmtDate(8), status:"expiring" },
  { id:7, client:"Meena Joshi", business:"Joshi Exports", email:"meena@joshi.com", phone:"+91 32109 87654", type:"Import/Export", licNo:"FSSAI-2024-00107", issued:"2023-02-14", expiry: fmtDate(290), status:"active" },
  { id:8, client:"Ravi Nair", business:"Nair Meat House", email:"ravi@nair.com", phone:"+91 21098 76543", type:"Meat Processing", licNo:"FSSAI-2024-00108", issued:"2023-09-05", expiry: fmtDate(22), status:"expiring" },
  { id:9, client:"Kavita Rao", business:"Rao Restaurant", email:"kavita@rao.com", phone:"+91 10987 65432", type:"Restaurant", licNo:"FSSAI-2024-00109", issued:"2023-06-18", expiry: fmtDate(95), status:"active" },
  { id:10, client:"Suresh Gupta", business:"Gupta Sweets", email:"suresh@gupta.com", phone:"+91 90876 54321", type:"Bakery", licNo:"FSSAI-2024-00110", issued:"2023-04-22", expiry: fmtDate(-30), status:"expired" },
];

// Generate more records for scalability demo
const names = ["Arun","Deepa","Harish","Lalita","Mohan","Neeta","Om","Poonam","Qadir","Rita","Sunil","Tara","Uday","Vidya","Wasim","Xenia","Yash","Zara"];
const bTypes = ["Restaurant","Catering","Bakery","Dairy","Food Processing","Street Food","Meat Processing","Import/Export"];
for(let i=11; i<=142; i++){
  const n = names[(i-11)%names.length];
  const d = Math.floor(Math.random()*400)-50;
  const s = d < 0 ? "expired" : d < 30 ? "expiring" : "active";
  licenses.push({
    id: i,
    client: n + " " + ["Kumar","Singh","Patel","Sharma","Mehta","Joshi","Rao","Nair","Gupta","Devi"][(i)%10],
    business: n + " " + bTypes[i%8],
    email: n.toLowerCase() + i + "@email.com",
    phone: "+91 9" + String(i*9999%100000000).padStart(8,'0'),
    type: bTypes[i%8],
    licNo: "FSSAI-2024-" + String(i).padStart(5,'0'),
    issued: fmtDate(-(Math.random()*500+100)),
    expiry: fmtDate(d),
    status: s
  });
}

function fmtDate(daysOffset) {
  const d = new Date(today);
  d.setDate(d.getDate() + Math.round(daysOffset));
  return d.toISOString().split('T')[0];
}

const notifications = [
  { id:1, type:"red", title:"License Expired", msg:"Sunita Dairy Farm (FSSAI-2024-00103) expired 12 days ago.", time:"12 min ago", unread:true },
  { id:2, type:"yellow", title:"Expiring in 3 Days", msg:"Mehta Catering Co. (FSSAI-2024-00102) expires on " + fmtDate(3), time:"1 hr ago", unread:true },
  { id:3, type:"yellow", title:"Expiring in 8 Days", msg:"Nair Meat House (FSSAI-2024-00108) expires in 8 days.", time:"2 hrs ago", unread:true },
  { id:4, type:"yellow", title:"30-Day Reminder Sent", msg:"Reminder emailed to priya@kitchen.com for license FSSAI-2024-00101.", time:"9:00 AM", unread:false },
  { id:5, type:"green", title:"License Renewed", msg:"Kumar Bakery successfully renewed. Valid until " + fmtDate(180), time:"Yesterday", unread:false },
  { id:6, type:"blue", title:"New License Added", msg:"Joshi Exports registered: FSSAI-2024-00107.", time:"2 days ago", unread:false },
  { id:7, type:"red", title:"Urgent: Expired", msg:"Singh Foods Pvt Ltd license expired 5 days ago — action needed.", time:"5 days ago", unread:false },
];

// =============================================
// RENDER FUNCTIONS
// =============================================
function badgeHtml(status) {
  const map = { active:"badge-active", expiring:"badge-expiring", expired:"badge-expired", pending:"badge-pending" };
  return `<span class="badge ${map[status]||'badge-pending'}">${status}</span>`;
}
function daysHtml(expiry) {
  const d = daysFromNow(expiry);
  if(d < 0) return `<span class="days-left danger">${Math.abs(d)}d overdue</span>`;
  if(d <= 30) return `<span class="days-left warn">${d}d left</span>`;
  return `<span class="days-left ok">${d}d left</span>`;
}

function renderMainTable(filter='all') {
  const tbody = document.getElementById('main-tbody');
  const list = licenses.filter(l => filter==='all' || l.status===filter).slice(0,12);
  tbody.innerHTML = list.map(l => `
    <tr onclick="viewLicense(${l.id})">
      <td><div class="td-name">${l.client}</div><div class="td-type">${l.business}</div></td>
      <td><span class="td-id">${l.licNo}</span></td>
      <td>${l.type}</td>
      <td>${l.expiry}</td>
      <td>${daysHtml(l.expiry)}</td>
      <td>${badgeHtml(l.status)}</td>
      <td><button class="action-btn" onclick="event.stopPropagation();sendReminder(${l.id})">📨 Notify</button></td>
    </tr>
  `).join('');
}

function renderAllTable(filter='all') {
  const tbody = document.getElementById('all-tbody');
  const list = licenses.filter(l => filter==='all' || l.status===filter);
  document.getElementById('license-count-label').textContent = list.length + ' records';
  tbody.innerHTML = list.map(l => `
    <tr>
      <td><div class="td-name">${l.client}</div></td>
      <td><span class="td-id">${l.licNo}</span></td>
      <td>${l.type}</td>
      <td>${l.issued}</td>
      <td>${l.expiry}</td>
      <td>${daysHtml(l.expiry)}</td>
      <td>${badgeHtml(l.status)}</td>
      <td style="display:flex;gap:6px">
        <button class="action-btn" onclick="sendReminder(${l.id})">📨</button>
        <button class="action-btn" onclick="editLicense(${l.id})">✏️</button>
      </td>
    </tr>
  `).join('');
}

function renderExpiringTable() {
  const tbody = document.getElementById('expiring-tbody');
  const list = licenses.filter(l => l.status==='expiring');
  tbody.innerHTML = list.map(l => `
    <tr>
      <td><div class="td-name">${l.client}</div><div class="td-type">${l.business}</div></td>
      <td><span class="td-id">${l.licNo}</span></td>
      <td>${l.type}</td>
      <td>${l.expiry}</td>
      <td>${daysHtml(l.expiry)}</td>
      <td><span class="td-id" style="color:var(--text3)">Notified 2d ago</span></td>
      <td><button class="action-btn" onclick="sendReminder(${l.id})">📨 Send Reminder</button></td>
    </tr>
  `).join('');
}

function renderExpiredTable() {
  const tbody = document.getElementById('expired-tbody');
  const list = licenses.filter(l => l.status==='expired');
  tbody.innerHTML = list.map(l => `
    <tr>
      <td><div class="td-name">${l.client}</div><div class="td-type">${l.business}</div></td>
      <td><span class="td-id">${l.licNo}</span></td>
      <td>${l.type}</td>
      <td style="color:var(--red)">${l.expiry}</td>
      <td><span class="days-left danger">${Math.abs(daysFromNow(l.expiry))}d overdue</span></td>
      <td>${badgeHtml('expired')}</td>
      <td><button class="action-btn" onclick="renewLicense(${l.id})">🔄 Renew</button></td>
    </tr>
  `).join('');
}

function renderNotifications(containerId, list) {
  const el = document.getElementById(containerId);
  if(!el) return;
  el.innerHTML = list.map(n => `
    <div class="notif-item ${n.unread?'unread':''}" onclick="markRead(${n.id})">
      <div class="notif-dot2 ${n.type}"></div>
      <div class="notif-content">
        <div class="notif-title">${n.title}</div>
        <div class="notif-msg">${n.msg}</div>
        <div class="notif-time">${n.time}</div>
      </div>
      ${n.unread ? '<div class="notif-unread-dot"></div>' : ''}
    </div>
  `).join('');
}

function renderClientsTable() {
  const tbody = document.getElementById('clients-tbody');
  const uniqueClients = licenses.slice(0,20);
  tbody.innerHTML = uniqueClients.map(l => `
    <tr>
      <td class="td-name">${l.client}</td>
      <td>${l.business}</td>
      <td style="color:var(--text2)">${l.email}</td>
      <td style="color:var(--text2)">${l.phone}</td>
      <td><span class="badge badge-active">1 license</span></td>
      <td><button class="action-btn">View</button></td>
    </tr>
  `).join('');
}

function renderAnalytics() {
  const chart = document.getElementById('analytics-chart');
  const types = {};
  licenses.forEach(l => types[l.type] = (types[l.type]||0)+1);
  const max = Math.max(...Object.values(types));
  const colors = ['#f5a623','#22c55e','#3b82f6','#ef4444','#a855f7','#ec4899','#14b8a6','#f59e0b'];
  chart.innerHTML = Object.entries(types).map(([k,v],i) => `
    <div class="chart-bar-row">
      <div class="chart-bar-label">${k}</div>
      <div class="chart-bar-track">
        <div class="chart-bar-fill" style="width:${(v/max*100)}%;background:${colors[i%colors.length]}"></div>
      </div>
      <div class="chart-bar-val">${v}</div>
    </div>
  `).join('');

  const rc = document.getElementById('renewals-chart');
  const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const vals = [8,12,7,15,11,18,14,9,16,13,10,17];
  const mx = Math.max(...vals);
  rc.innerHTML = vals.map((v,i) => `
    <div class="chart-bar-row">
      <div class="chart-bar-label">${months[i]}</div>
      <div class="chart-bar-track">
        <div class="chart-bar-fill" style="width:${v/mx*100}%;background:#f5a623"></div>
      </div>
      <div class="chart-bar-val">${v}</div>
    </div>
  `).join('');
}

// =============================================
// NAVIGATION
// =============================================
const sectionTitles = {
  dashboard: 'Dashboard <span>Overview</span>',
  licenses: 'All Licenses <span>142 records</span>',
  expiring: 'Expiring Soon <span>18 licenses</span>',
  expired: 'Expired Licenses <span>7 licenses</span>',
  notifications: 'Notifications <span>Log</span>',
  scheduler: 'Scheduler <span>Automation</span>',
  analytics: 'Analytics <span>Insights</span>',
  clients: 'Clients <span>Directory</span>',
  settings: 'Settings <span>Configuration</span>',
};

function navigate(section, el) {
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById('section-'+section).classList.add('active');
  if(el) el.classList.add('active');
  document.getElementById('topbar-title').innerHTML = sectionTitles[section] || section;
  // Render on demand
  if(section==='licenses') renderAllTable();
  if(section==='expiring') renderExpiringTable();
  if(section==='expired') renderExpiredTable();
  if(section==='notifications') renderNotifications('full-notif-list', notifications);
  if(section==='analytics') setTimeout(renderAnalytics, 100);
  if(section==='clients') renderClientsTable();
  closeSidebar();
}

// =============================================
// FILTER TABLE
// =============================================
function filterTable(filter, el) {
  document.querySelectorAll('.card-actions .btn-sm').forEach(b => b.classList.remove('active'));
  el.classList.add('active');
  renderMainTable(filter);
}
function filterSection(filter, el) {
  document.querySelectorAll('.tabs .tab').forEach(t => t.classList.remove('active'));
  el.classList.add('active');
  renderAllTable(filter);
}

// =============================================
// SEARCH
// =============================================
function handleSearch(val) {
  if(!val) { renderMainTable(); return; }
  const q = val.toLowerCase();
  const tbody = document.getElementById('main-tbody');
  const list = licenses.filter(l =>
    l.client.toLowerCase().includes(q) ||
    l.licNo.toLowerCase().includes(q) ||
    l.business.toLowerCase().includes(q)
  ).slice(0,12);
  tbody.innerHTML = list.length ? list.map(l => `
    <tr>
      <td><div class="td-name">${l.client}</div><div class="td-type">${l.business}</div></td>
      <td><span class="td-id">${l.licNo}</span></td>
      <td>${l.type}</td>
      <td>${l.expiry}</td>
      <td>${daysHtml(l.expiry)}</td>
      <td>${badgeHtml(l.status)}</td>
      <td><button class="action-btn" onclick="sendReminder(${l.id})">📨 Notify</button></td>
    </tr>
  `).join('') : '<tr><td colspan="7"><div class="empty-state"><div class="icon">🔍</div><p>No results found</p></div></td></tr>';
}

// =============================================
// MODAL
// =============================================
function openModal() {
  document.getElementById('modal-overlay').classList.add('open');
  // Set today as default issued date
  document.getElementById('f-issued').value = today.toISOString().split('T')[0];
}
function closeModal() {
  document.getElementById('modal-overlay').classList.remove('open');
}
document.getElementById('modal-overlay').addEventListener('click', e => { if(e.target===e.currentTarget) closeModal(); });

function addLicense() {
  const name = document.getElementById('f-name').value.trim();
  const biz = document.getElementById('f-business').value.trim();
  const email = document.getElementById('f-email').value.trim();
  const type = document.getElementById('f-type').value;
  const licNo = document.getElementById('f-licno').value.trim();
  const issued = document.getElementById('f-issued').value;
  const expiry = document.getElementById('f-expiry').value;

  if(!name || !biz || !email || !type || !licNo || !issued || !expiry) {
    showToast('Please fill in all required fields.', 'error'); return;
  }

  const d = daysFromNow(expiry);
  const status = d < 0 ? 'expired' : d <= 30 ? 'expiring' : 'active';

  licenses.unshift({
    id: licenses.length + 1,
    client: name, business: biz, email, phone: document.getElementById('f-phone').value,
    type, licNo, issued, expiry, status
  });

  notifications.unshift({
    id: notifications.length + 1,
    type: 'blue',
    title: 'New License Registered',
    msg: `${biz} (${licNo}) has been added. Expiry: ${expiry}`,
    time: 'Just now',
    unread: true
  });

  closeModal();
  renderMainTable();
  renderNotifications('notif-list', notifications.slice(0,8));
  showToast(`License registered for ${name}.`, 'success');

  // Clear form
  ['f-name','f-business','f-email','f-phone','f-licno','f-expiry'].forEach(id => document.getElementById(id).value='');
  document.getElementById('f-type').value='';
}

// =============================================
// ACTIONS
// =============================================
function sendReminder(id) {
  const l = licenses.find(x=>x.id===id);
  if(!l) return;
  showToast(`📨 Reminder sent to ${l.client} (${l.email})`, 'success');
  notifications.unshift({
    id: Date.now(), type:'blue',
    title:'Reminder Sent',
    msg:`Manual reminder sent to ${l.client} for ${l.licNo}.`,
    time:'Just now', unread:true
  });
  renderNotifications('notif-list', notifications.slice(0,8));
}

function sendBulkReminder() {
  const count = licenses.filter(l=>l.status==='expiring').length;
  showToast(`📨 Reminders sent to all ${count} expiring clients.`, 'success');
}

function renewLicense(id) {
  const l = licenses.find(x=>x.id===id);
  if(!l) return;
  const newExpiry = fmtDate(365);
  l.expiry = newExpiry;
  l.status = 'active';
  renderExpiredTable();
  showToast(`✅ ${l.client}'s license renewed until ${newExpiry}`, 'success');
}

function editLicense(id) {
  showToast('Edit functionality — connect to your backend API.', 'warning');
}

function viewLicense(id) {
  // Could open a detail modal — simplified here
}

function markRead(id) {
  const n = notifications.find(x=>x.id===id);
  if(n) n.unread = false;
  renderNotifications('notif-list', notifications.slice(0,8));
  renderNotifications('full-notif-list', notifications);
}

function markAllRead() {
  notifications.forEach(n => n.unread = false);
  renderNotifications('notif-list', notifications.slice(0,8));
  renderNotifications('full-notif-list', notifications);
  showToast('All notifications marked as read.', 'success');
}

function toggleSwitch(el, name) {
  el.classList.toggle('on');
  const on = el.classList.contains('on');
  showToast(`${name.charAt(0).toUpperCase()+name.slice(1)} ${on?'enabled':'disabled'}.`, on?'success':'warning');
}

function exportCSV() {
  const headers = ['Client','Business','License No','Type','Issued','Expiry','Status'];
  const rows = licenses.map(l => [l.client,l.business,l.licNo,l.type,l.issued,l.expiry,l.status]);
  const csv = [headers,...rows].map(r=>r.join(',')).join('\n');
  const blob = new Blob([csv], {type:'text/csv'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'food_licenses.csv';
  a.click();
  showToast('CSV exported successfully.', 'success');
}

// =============================================
// TOAST
// =============================================
function showToast(msg, type='success') {
  const c = document.getElementById('toast-container');
  const t = document.createElement('div');
  const icons = { success:'✅', warning:'⚠️', error:'❌' };
  t.className = `toast ${type}`;
  t.innerHTML = `<span>${icons[type]||'ℹ️'}</span><span>${msg}</span>`;
  c.appendChild(t);
  setTimeout(() => t.remove(), 4000);
}

// =============================================
// SIDEBAR MOBILE
// =============================================
function toggleSidebar() {
  document.getElementById('sidebar').classList.toggle('open');
}
function closeSidebar() {
  document.getElementById('sidebar').classList.remove('open');
}

// =============================================
// AUTOMATED CHECK SIMULATION
// =============================================
function runAutomatedCheck() {
  const expiring = licenses.filter(l => l.status==='expiring' && daysFromNow(l.expiry) <= 7);
  const expired = licenses.filter(l => l.status==='expired');
  if(expiring.length) {
    expiring.slice(0,1).forEach(l => {
      notifications.unshift({
        id: Date.now(),
        type: 'yellow',
        title: `Auto: ${daysFromNow(l.expiry)}d remaining`,
        msg: `Scheduled check: ${l.client} (${l.licNo}) needs renewal.`,
        time: 'Just now',
        unread: true
      });
    });
    renderNotifications('notif-list', notifications.slice(0,8));
  }
}

// =============================================
// INIT
// =============================================
renderMainTable();
renderNotifications('notif-list', notifications.slice(0,8));

// Simulate automated check every 30 seconds
setInterval(runAutomatedCheck, 30000);
</script>
</body>
</html>
