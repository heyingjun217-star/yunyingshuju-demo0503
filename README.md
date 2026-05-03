[index.html](https://github.com/user-attachments/files/27315834/index.html)
# yunyingshuju-demo0503
运营数据看板 Demo
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>运营数据看板 · Demo</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", "Microsoft YaHei", sans-serif;
  font-size: 13px;
  line-height: 1.6;
  color: #2C2C2A;
  background: #F8F7F3;
  -webkit-font-smoothing: antialiased;
}
:root {
  --bg-primary: #FFFFFF;
  --bg-secondary: #F1EFE8;
  --bg-tertiary: #F8F7F3;
  --bg-info: #E6F1FB;
  --bg-success: #E1F5EE;
  --bg-warn: #FAEEDA;
  --bg-danger: #FCEBEB;
  --bg-purple: #EEEDFE;
  --text-primary: #2C2C2A;
  --text-secondary: #5F5E5A;
  --text-tertiary: #888780;
  --text-info: #185FA5;
  --text-success: #0F6E56;
  --text-warn: #BA7517;
  --text-danger: #A32D2D;
  --text-purple: #534AB7;
  --border-tertiary: #E1DEDB;
  --border-secondary: #D3D1C7;
  --border-info: #B5D4F4;
  --border-success: #9FE1CB;
  --border-warn: #FAC775;
  --border-danger: #F7C1C1;
  --radius-md: 8px;
  --radius-lg: 12px;
}
.app { max-width: 1400px; margin: 0 auto; padding: 20px; }
header.app-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 20px;
  background: white;
  border-radius: var(--radius-lg);
  border: 0.5px solid var(--border-tertiary);
  margin-bottom: 16px;
  flex-wrap: wrap;
  gap: 12px;
}
.brand { display: flex; align-items: baseline; gap: 12px; }
.brand-name { font-size: 16px; font-weight: 500; }
.brand-tag { font-size: 11px; color: var(--text-tertiary); }
.role-switch { display: flex; background: var(--bg-secondary); border-radius: var(--radius-md); padding: 3px; gap: 2px; }
.role-switch button {
  background: transparent; border: none; padding: 6px 14px;
  font-size: 12px; cursor: pointer; color: var(--text-secondary);
  border-radius: 6px; font-family: inherit; transition: all .15s;
}
.role-switch button.active { background: var(--text-info); color: white; }
nav.tabs {
  display: flex; gap: 4px; flex-wrap: wrap; padding: 8px 4px;
  border-bottom: 0.5px solid var(--border-tertiary); margin-bottom: 16px;
}
nav.tabs button {
  background: transparent; border: none; padding: 9px 14px;
  font-size: 13px; cursor: pointer; color: var(--text-secondary);
  border-radius: var(--radius-md) var(--radius-md) 0 0;
  font-family: inherit; transition: all .15s;
  border-bottom: 2px solid transparent;
}
nav.tabs button:hover:not(.active) { color: var(--text-primary); background: var(--bg-secondary); }
nav.tabs button.active { color: var(--text-info); border-bottom-color: var(--text-info); font-weight: 500; }
nav.tabs .badge {
  display: inline-block; background: var(--bg-secondary); color: var(--text-tertiary);
  font-size: 10px; padding: 1px 6px; border-radius: 8px; margin-left: 4px; font-weight: 400;
}
.crumb { font-size: 12px; color: var(--text-tertiary); margin-bottom: 8px; }
.crumb a { color: var(--text-info); cursor: pointer; }
.fbar {
  display: flex; align-items: center; gap: 8px; flex-wrap: wrap;
  padding: 10px 14px; background: white; border-radius: var(--radius-md);
  margin-bottom: 14px; border: 0.5px solid var(--border-tertiary);
}
.fbar-lbl { font-size: 12px; color: var(--text-secondary); font-weight: 500; }
.fbar select, .fbar button, .fbar input {
  height: 30px; font-size: 12px; padding: 0 10px;
  border: 0.5px solid var(--border-secondary); border-radius: var(--radius-md);
  background: white; font-family: inherit; color: var(--text-primary);
}
.fbar input { width: 160px; }
.fbar button { cursor: pointer; }
.fbar button:hover { background: var(--bg-secondary); }
.sp { flex: 1; }
.btn-primary { background: var(--text-info) !important; color: white !important; border: none !important; }

.page-title { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 14px; flex-wrap: wrap; gap: 8px; }
.page-title h1 { font-size: 18px; font-weight: 500; }
.page-subtitle { font-size: 12px; color: var(--text-tertiary); }

.kpi-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-bottom: 14px; }
.kpi {
  background: white; border-radius: var(--radius-lg); padding: 14px 14px 12px;
  position: relative; overflow: hidden; border: 0.5px solid var(--border-tertiary);
}
.kpi::before { content: ''; position: absolute; top: 0; left: 0; right: 0; height: 3px; }
.kpi.tone-info::before { background: var(--text-info); }
.kpi.tone-warn::before { background: var(--text-warn); }
.kpi.tone-danger::before { background: var(--text-danger); }
.kpi.tone-success::before { background: var(--text-success); }
.kpi.tone-purple::before { background: var(--text-purple); }
.kpi.tone-neutral::before { background: var(--text-tertiary); }
.kpi-label {
  font-size: 12px; color: var(--text-secondary); margin-bottom: 6px;
  display: flex; justify-content: space-between; align-items: baseline;
}
.kpi-label-pct { font-size: 11px; color: var(--text-tertiary); font-variant-numeric: tabular-nums; }
.kpi-value { font-size: 22px; font-weight: 500; line-height: 1.1; margin-bottom: 4px; font-variant-numeric: tabular-nums; }
.kpi-sub { font-size: 11px; color: var(--text-tertiary); line-height: 1.4; }
.tone-info .kpi-value { color: var(--text-info); }
.tone-success .kpi-value { color: var(--text-success); }
.tone-warn .kpi-value { color: var(--text-warn); }
.tone-danger .kpi-value { color: var(--text-danger); }
.tone-purple .kpi-value { color: var(--text-purple); }

.cc {
  background: white; border-radius: var(--radius-lg); padding: 14px 16px;
  margin-bottom: 14px; border: 0.5px solid var(--border-tertiary);
}
.sec {
  font-size: 13px; font-weight: 500; margin: 0 0 10px;
  padding-left: 8px; border-left: 3px solid var(--text-info);
}
.split { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 14px; }

table.dt { width: 100%; border-collapse: collapse; font-size: 12px; }
table.dt th {
  text-align: left; padding: 8px 10px; font-weight: 500;
  color: var(--text-secondary); border-bottom: 0.5px solid var(--border-secondary);
  font-size: 11px;
}
table.dt td { padding: 7px 10px; border-bottom: 0.5px solid var(--border-tertiary); }
table.dt tr.row-clickable { cursor: pointer; transition: background .15s; }
table.dt tr.row-clickable:hover { background: var(--bg-info); }
.tnum { font-variant-numeric: tabular-nums; text-align: right; }
.tnum-zero { color: var(--border-secondary); }

.tag { display: inline-block; padding: 1px 7px; border-radius: var(--radius-md); font-size: 11px; font-weight: 500; }
.tag-good { background: var(--bg-success); color: var(--text-success); }
.tag-warn { background: var(--bg-warn); color: var(--text-warn); }
.tag-bad { background: var(--bg-danger); color: var(--text-danger); }
.tag-info { background: var(--bg-info); color: var(--text-info); }
.tag-grade-a { background: var(--bg-info); color: var(--text-info); }
.tag-grade-am { background: var(--bg-info); color: var(--text-info); opacity: .75; }
.tag-grade-bp { background: #5DCAA5; color: #04342C; }
.tag-grade-b { background: var(--bg-secondary); color: var(--text-tertiary); }
.tag-grade-c { background: transparent; color: var(--text-tertiary); border: 0.5px dashed var(--border-tertiary); }
.tag-orphan { background: var(--bg-warn); color: var(--text-warn); }
.tag-down { background: var(--bg-danger); color: var(--text-danger); opacity: .85; }

.note { font-size: 11px; color: var(--text-tertiary); margin-top: 8px; line-height: 1.5; }
.note strong { color: var(--text-primary); }

.lg { display: flex; flex-wrap: wrap; gap: 10px; margin: 6px 0; font-size: 11px; color: var(--text-secondary); }
.lg span { display: flex; align-items: center; gap: 5px; }
.lg i { display: inline-block; width: 10px; height: 10px; border-radius: 2px; }

.cw { position: relative; height: 240px; }

.mini-bar { height: 5px; background: var(--border-tertiary); border-radius: 3px; margin-top: 6px; overflow: hidden; }
.mini-fill { height: 100%; border-radius: 3px; }

.kpi-arpu { font-size: 11px; color: var(--text-tertiary); margin-top: 4px; font-variant-numeric: tabular-nums; }

.banner {
  background: var(--bg-info); border: 0.5px solid var(--border-info);
  border-radius: var(--radius-md); padding: 10px 14px; font-size: 12px;
  color: var(--text-info); line-height: 1.6; margin-bottom: 14px;
  display: flex; gap: 10px; align-items: flex-start;
}
.banner-icon {
  flex-shrink: 0; width: 18px; height: 18px; border-radius: 50%;
  background: var(--text-info); color: #fff; display: flex; align-items: center;
  justify-content: center; font-weight: 500; font-size: 11px; margin-top: 1px;
}

.health-row {
  display: grid; grid-template-columns: 32px 1.5fr 0.8fr 0.8fr 0.8fr 1fr 0.8fr;
  gap: 10px; align-items: center; padding: 8px 10px;
  border-bottom: 0.5px solid var(--border-tertiary); font-size: 12px;
}
.health-row.head { font-size: 11px; color: var(--text-tertiary); font-weight: 500; }
.health-icon {
  width: 18px; height: 18px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-size: 11px; font-weight: 500; flex-shrink: 0;
}
.health-good { background: var(--bg-success); color: var(--text-success); }
.health-warn { background: var(--bg-warn); color: var(--text-warn); }
.health-bad { background: var(--bg-danger); color: var(--text-danger); }
.bar-wrap { height: 6px; background: var(--border-tertiary); border-radius: 3px; overflow: hidden; width: 100%; }
.bar-fill { height: 100%; border-radius: 3px; }

.exception-card {
  background: white; border: 0.5px solid var(--border-tertiary);
  border-radius: var(--radius-md); padding: 10px 12px; margin-bottom: 8px;
  display: grid; grid-template-columns: auto 1fr auto; gap: 12px; align-items: center;
}
.exc-icon {
  width: 28px; height: 28px; border-radius: 50%; display: flex; align-items: center;
  justify-content: center; font-weight: 500; font-size: 12px; flex-shrink: 0;
}
.exc-warn { background: var(--bg-warn); color: var(--text-warn); }
.exc-bad { background: var(--bg-danger); color: var(--text-danger); }
.exc-info { background: var(--bg-info); color: var(--text-info); }
.exc-title { font-size: 12px; font-weight: 500; margin-bottom: 2px; }
.exc-desc { font-size: 11px; color: var(--text-tertiary); }
.btn-sm {
  height: 26px; font-size: 11px; padding: 0 10px;
  background: white; border: 0.5px solid var(--border-secondary);
  border-radius: var(--radius-md); cursor: pointer; font-family: inherit;
}
.btn-sm:hover { background: var(--bg-secondary); }

/* Drawer */
.drawer-overlay {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(44, 44, 42, 0.45); z-index: 100;
  display: none; opacity: 0; transition: opacity .2s;
}
.drawer-overlay.show { display: block; opacity: 1; }
.drawer {
  position: fixed; top: 0; right: 0; bottom: 0; width: 540px; max-width: 90vw;
  background: white; z-index: 101; box-shadow: -4px 0 20px rgba(0,0,0,0.1);
  overflow-y: auto; transform: translateX(100%); transition: transform .25s;
}
.drawer.show { transform: translateX(0); }
.drawer-header {
  padding: 18px 20px; border-bottom: 0.5px solid var(--border-tertiary);
  display: flex; justify-content: space-between; align-items: flex-start; gap: 12px;
}
.drawer-title { font-size: 16px; font-weight: 500; margin-bottom: 4px; }
.drawer-meta { font-size: 11px; color: var(--text-tertiary); display: flex; gap: 8px; flex-wrap: wrap; }
.drawer-close {
  background: none; border: none; cursor: pointer; font-size: 20px;
  color: var(--text-tertiary); padding: 0; line-height: 1;
}
.drawer-body { padding: 16px 20px; }
.drawer-section { margin-bottom: 16px; }
.drawer-section h3 {
  font-size: 12px; font-weight: 500; color: var(--text-secondary);
  margin-bottom: 8px; text-transform: uppercase; letter-spacing: 0.5px;
}
.drawer-mini-kpis { display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px; }
.drawer-mini-kpi {
  background: var(--bg-secondary); border-radius: var(--radius-md);
  padding: 10px 12px;
}
.drawer-mini-kpi .lbl { font-size: 10px; color: var(--text-tertiary); margin-bottom: 4px; }
.drawer-mini-kpi .val { font-size: 16px; font-weight: 500; }
.drawer-meta-row {
  display: grid; grid-template-columns: 100px 1fr; gap: 10px;
  font-size: 12px; padding: 6px 0; border-bottom: 0.5px solid var(--border-tertiary);
}
.drawer-meta-row:last-child { border-bottom: none; }
.drawer-meta-row dt { color: var(--text-tertiary); }
.drawer-meta-row dd { margin: 0; }
.drawer-narrative {
  background: var(--bg-tertiary); border-radius: var(--radius-md);
  padding: 10px 12px; font-size: 12px; line-height: 1.6; margin-bottom: 8px;
}
.drawer-actions {
  display: flex; gap: 8px; padding: 14px 20px;
  border-top: 0.5px solid var(--border-tertiary);
  background: var(--bg-tertiary); position: sticky; bottom: 0;
}
.warning-box {
  background: var(--bg-danger); border: 0.5px solid var(--border-danger);
  color: var(--text-danger); padding: 10px 14px; border-radius: var(--radius-md);
  font-size: 12px; margin-bottom: 12px;
}

/* Pipeline */
.pipeline-row { display: grid; grid-template-columns: repeat(5, 1fr) auto repeat(2, 1fr); gap: 4px; align-items: center; margin-bottom: 14px; }
.pipeline-step {
  background: white; border: 0.5px solid var(--border-tertiary);
  border-radius: var(--radius-md); padding: 10px 12px; text-align: left;
}
.pipeline-step.active { border: 1px solid var(--text-info); background: var(--bg-info); }
.pipeline-step .num {
  display: inline-flex; align-items: center; justify-content: center;
  width: 18px; height: 18px; border-radius: 50%;
  background: var(--text-info); color: white; font-size: 11px; font-weight: 500; margin-right: 6px;
}
.pipeline-arrow { color: var(--text-tertiary); font-size: 14px; text-align: center; }
.pipeline-title { font-size: 12px; font-weight: 500; }
.pipeline-desc { font-size: 10px; color: var(--text-tertiary); margin-top: 4px; line-height: 1.4; }

/* Staff workspace */
.user-card {
  display: flex; align-items: center; gap: 12px;
  padding: 12px 16px; background: white; border-radius: var(--radius-lg);
  margin-bottom: 14px; border: 0.5px solid var(--border-tertiary);
}
.avatar {
  width: 40px; height: 40px; border-radius: 50%; background: var(--text-info);
  color: white; display: flex; align-items: center; justify-content: center;
  font-weight: 500; font-size: 16px;
}
.user-info { flex: 1; }
.user-name { font-size: 14px; font-weight: 500; }
.user-role { font-size: 11px; color: var(--text-tertiary); }
.scope-chip {
  display: inline-flex; align-items: center; gap: 5px;
  background: var(--bg-info); border: 0.5px solid var(--border-info);
  color: var(--text-info); padding: 3px 10px; border-radius: 14px;
  font-size: 11px; font-weight: 500; margin-left: 8px;
}
.todo-card {
  background: white; border: 0.5px solid var(--border-tertiary);
  border-radius: var(--radius-md); padding: 12px 14px; margin-bottom: 8px;
  display: grid; grid-template-columns: auto 1fr auto auto; gap: 12px; align-items: center;
}
.todo-priority { width: 4px; height: 36px; border-radius: 2px; }
.pri-high { background: var(--text-danger); }
.pri-mid { background: var(--text-warn); }
.pri-low { background: var(--text-tertiary); }
.todo-title { font-size: 12px; font-weight: 500; margin-bottom: 2px; }
.todo-desc { font-size: 11px; color: var(--text-tertiary); }
.todo-due { font-size: 11px; color: var(--text-tertiary); text-align: right; }
.todo-due strong { color: var(--text-danger); }

/* Hide pages */
.page { display: none; }
.page.active { display: block; }

/* Heatmap cell */
.heat-cell { position: relative; }

/* Footer */
footer.demo-footer {
  text-align: center; padding: 20px; font-size: 11px;
  color: var(--text-tertiary); margin-top: 20px;
}

@media (max-width: 1100px) {
  .kpi-grid { grid-template-columns: repeat(2, 1fr); }
  .split { grid-template-columns: 1fr; }
}
</style>
</head>
<body>

<div class="app">

<!-- ============ Header ============ -->
<header class="app-header">
  <div class="brand">
    <div class="brand-name">运营数据看板</div>
    <div class="brand-tag">Demo · 离线版本 · 6 页 + 2 抽屉 + 双角色</div>
  </div>
  <div class="role-switch">
    <button id="role-admin" class="active" onclick="switchRole('admin')">管理员视图</button>
    <button id="role-staff" onclick="switchRole('staff')">经办人视图</button>
  </div>
</header>

<!-- ============ Tabs ============ -->
<nav class="tabs" id="page-tabs">
  <button class="active" onclick="switchPage('cockpit')" data-admin>01 总览驾驶舱</button>
  <button onclick="switchPage('channel')" data-admin>02 渠道经营 <span class="badge">29</span></button>
  <button onclick="switchPage('library')" data-admin>03 节目库 <span class="badge">2,338</span></button>
  <button onclick="switchPage('revenue')" data-admin>04 节目收益 <span class="badge">3,015 万</span></button>
  <button onclick="switchPage('rights')" data-admin>05 版权洞察</button>
  <button onclick="switchPage('data')" data-admin>06 数据管理</button>
  <button onclick="switchPage('chdist')" data-admin>07 渠道分发 <span class="badge">29</span></button>
  <button onclick="switchPage('staff')" data-staff style="display:none">我的工作台 <span class="badge">87 部</span></button>
</nav>

<!-- ============ Page 1: Cockpit ============ -->
<section class="page active" id="page-cockpit">
  <div class="crumb">驾驶舱</div>
  <div class="page-title">
    <div>
      <h1>总览驾驶舱</h1>
      <div class="page-subtitle">CXO 入口 · 30 秒读懂经营状态 · 数据范围 26Q1 · 单位：万元</div>
    </div>
  </div>
  <div id="cockpit-kpis" class="kpi-grid"></div>

  <div class="cc">
    <div class="sec">月度收益趋势 + Q4 预估</div>
    <div class="cw"><canvas id="chart-cockpit-trend"></canvas></div>
  </div>

  <div class="split">
    <div class="cc" style="margin:0">
      <div class="sec">26Q1 缺口来源（247.78 万）</div>
      <div class="cw"><canvas id="chart-cockpit-gap"></canvas></div>
    </div>
    <div class="cc" style="margin:0">
      <div class="sec">29 渠道 5 状态分布</div>
      <table class="dt" id="cockpit-status-table"></table>
    </div>
  </div>
</section>

<!-- ============ Page 2: Channel ============ -->
<section class="page" id="page-channel">
  <div class="crumb"><a onclick="switchPage('cockpit')">驾驶舱</a> / 渠道经营</div>
  <div class="fbar">
    <span class="fbar-lbl">筛选</span>
    <select><option>全部年度</option><option>2026</option></select>
    <select><option>全部季度</option><option>Q1</option></select>
    <select><option>全部大类</option><option>OTT</option><option>IPTV</option><option>手机</option></select>
    <span class="sp"></span>
    <button>导出</button>
  </div>
  <div class="page-title">
    <div>
      <h1>渠道经营</h1>
      <div class="page-subtitle">29 渠道收益排行 · 达成率 · 同环比 · 卡点归因 · 26Q1</div>
    </div>
  </div>
  <div id="channel-kpis" class="kpi-grid"></div>

  <div class="cc">
    <div class="sec">渠道大类对比（指标 vs 实际）</div>
    <div class="cw"><canvas id="chart-channel-cat"></canvas></div>
  </div>

  <div class="cc">
    <div class="sec">TOP 10 渠道收益排行（点击渠道行查看抽屉）</div>
    <table class="dt" id="channel-top10-table"></table>
  </div>
</section>

<!-- ============ Page 3: Library ============ -->
<section class="page" id="page-library">
  <div class="crumb"><a onclick="switchPage('cockpit')">驾驶舱</a> / 节目库与上线状态</div>
  <div class="fbar">
    <span class="fbar-lbl">筛选</span>
    <select><option>全部等级</option><option>A</option><option>B+</option><option>B</option><option>C</option></select>
    <select><option>全部状态</option><option>已上线</option><option>未上线</option></select>
    <input placeholder="搜索节目..." />
    <span class="sp"></span>
    <button>导出</button>
  </div>
  <div class="page-title">
    <div>
      <h1>节目库与上线状态</h1>
      <div class="page-subtitle">主表 2,338 部 · 跨 29 渠道 56,676 条 · 95.3% 命名对齐</div>
    </div>
  </div>
  <div id="library-kpis" class="kpi-grid"></div>

  <div class="split">
    <div class="cc" style="margin:0">
      <div class="sec">主库等级金字塔（采购侧权威）</div>
      <div class="cw"><canvas id="chart-library-pyramid"></canvas></div>
    </div>
    <div class="cc" style="margin:0">
      <div class="sec">未上线 6 大归因</div>
      <div class="cw"><canvas id="chart-library-reasons"></canvas></div>
      <div class="note"><strong>剔除"结构性重复"后真实运营卡点 = 3,923 部次</strong></div>
    </div>
  </div>

  <div class="cc">
    <div class="sec">节目明细（点击行查看节目详情抽屉）</div>
    <table class="dt" id="library-programs-table"></table>
  </div>
</section>

<!-- ============ Page 4: Revenue ============ -->
<section class="page" id="page-revenue">
  <div class="crumb"><a onclick="switchPage('cockpit')">驾驶舱</a> / 节目收益分析</div>
  <div class="fbar">
    <span class="fbar-lbl">筛选</span>
    <select><option>全部年度</option><option>2024</option><option>2025</option></select>
    <select><option>全部季度</option><option>Q1</option><option>Q2</option><option>Q3</option><option>Q4</option></select>
    <select><option>全部渠道</option><option>小米</option><option>海信</option><option>酷开</option><option>TCL</option><option>康佳</option><option>华为</option></select>
    <select><option>全部等级</option><option>A</option><option>B+</option><option>B</option><option>C</option><option>已下线</option><option>孤儿</option></select>
    <span class="sp"></span>
    <button>导出</button>
  </div>
  <div class="page-title">
    <div>
      <h1>节目收益分析</h1>
      <div class="page-subtitle">3,015.7 万 · 6 渠道 · 7 季度 · 康佳已应用版权感知分配</div>
    </div>
  </div>
  <div id="revenue-kpis-row1" class="kpi-grid"></div>
  <div id="revenue-kpis-row2" class="kpi-grid"></div>

  <div class="cc">
    <div class="sec">A / B / C 级占比的逐季演变（结构性升级正在发生）</div>
    <div class="lg">
      <span><i style="background:#0F6E56"></i>A 系列</span>
      <span><i style="background:#534AB7"></i>B 系列</span>
      <span><i style="background:#9FE1CB"></i>C 级</span>
      <span><i style="background:#A32D2D"></i>已下线</span>
      <span><i style="background:#BA7517"></i>孤儿</span>
      <span style="margin-left:auto;color:var(--text-tertiary)">柱顶 = A 级占比</span>
    </div>
    <div class="cw"><canvas id="chart-revenue-trend"></canvas></div>
    <div class="note"><strong>核心发现：</strong>A 级占比从 13.8% 升到 44.2% · 翻 3 倍 · 长尾收缩 + 头部集中的结构性升级正在发生</div>
  </div>

  <div class="cc">
    <div class="sec" style="display:flex;justify-content:space-between;align-items:center">
      <span>TOP 25 节目收益矩阵</span>
      <div class="role-switch" style="margin-bottom:0">
        <button id="rev-tab-channel" class="active" onclick="switchRevTab('channel')" style="height:24px;font-size:11px">按渠道拆分</button>
        <button id="rev-tab-quarter" onclick="switchRevTab('quarter')" style="height:24px;font-size:11px">按季度拆分</button>
      </div>
    </div>
    <div style="overflow-x:auto">
      <table class="dt" id="revenue-matrix-table"></table>
    </div>
    <div class="note" id="revenue-matrix-note"></div>
  </div>
</section>

<!-- ============ Page 5: Rights ============ -->
<section class="page" id="page-rights">
  <div class="crumb"><a onclick="switchPage('cockpit')">驾驶舱</a> / 版权洞察</div>
  <div class="page-title">
    <div>
      <h1>版权洞察</h1>
      <div class="page-subtitle">主表 2,338 部版权字段汇总 · 战略层 · 集中度 / 到期 / 权利结构</div>
    </div>
  </div>
  <div id="rights-kpis" class="kpi-grid"></div>

  <div class="cc">
    <div class="sec">版权商集中度（TOP 10 + 长尾）</div>
    <div class="cw" style="height:300px"><canvas id="chart-rights-pareto"></canvas></div>
  </div>

  <div class="split">
    <div class="cc" style="margin:0">
      <div class="sec">采购模式分布</div>
      <div class="cw"><canvas id="chart-rights-license"></canvas></div>
    </div>
    <div class="cc" style="margin:0">
      <div class="sec">版权到期 5 桶预警</div>
      <table class="dt" id="rights-expiry-table"></table>
    </div>
  </div>
</section>

<!-- ============ Page 6: Data Management ============ -->
<section class="page" id="page-data">
  <div class="crumb"><a onclick="switchPage('cockpit')">驾驶舱</a> / 数据管理（底座）</div>
  <div class="page-title">
    <div>
      <h1>数据管理</h1>
      <div class="page-subtitle">7 张数据表 · 5 步流水线 · 命名 pipeline 自动应用 · 当前角色：管理员</div>
    </div>
    <button class="btn-primary" style="height:32px;padding:0 14px;border-radius:var(--radius-md);cursor:pointer;font-family:inherit">+ 上传新数据</button>
  </div>
  <div id="data-kpis" class="kpi-grid"></div>

  <div class="cc">
    <div class="sec">5 步上传流水线</div>
    <div class="pipeline-row">
      <div class="pipeline-step active">
        <div class="pipeline-title"><span class="num">1</span>选择文件</div>
        <div class="pipeline-desc">.xlsx / .csv / .zip · ≤50MB</div>
      </div>
      <div class="pipeline-arrow">→</div>
      <div class="pipeline-step">
        <div class="pipeline-title"><span class="num">2</span>字段映射</div>
        <div class="pipeline-desc">自动识别 · 别名词典记忆</div>
      </div>
      <div class="pipeline-arrow">→</div>
      <div class="pipeline-step">
        <div class="pipeline-title"><span class="num">3</span>清洗校验</div>
        <div class="pipeline-desc">14 类规则 · 异常单独列出</div>
      </div>
      <div class="pipeline-arrow">→</div>
      <div class="pipeline-step">
        <div class="pipeline-title"><span class="num">4</span>预览影响</div>
        <div class="pipeline-desc">差异 diff · 主表对齐</div>
      </div>
      <div class="pipeline-arrow">→</div>
      <div class="pipeline-step">
        <div class="pipeline-title"><span class="num">5</span>确认入库</div>
        <div class="pipeline-desc">版本化 · 自动刷新</div>
      </div>
    </div>
  </div>

  <div class="cc">
    <div class="sec">7 张数据表健康清单</div>
    <div id="data-tables-list"></div>
  </div>

  <div class="split">
    <div class="cc" style="margin:0">
      <div class="sec">异常处理队列</div>
      <div id="data-exceptions"></div>
    </div>
    <div class="cc" style="margin:0">
      <div class="sec">最近上传记录（支持版本回滚）</div>
      <table class="dt" id="data-uploads"></table>
    </div>
  </div>
</section>

<!-- ============ Page Staff: Personal Workspace ============ -->
<section class="page" id="page-staff">
  <div class="crumb">我的工作台</div>

  <div class="user-card">
    <div class="avatar" id="staff-avatar">严</div>
    <div class="user-info">
      <div class="user-name" id="staff-name">严俏城<span class="scope-chip">经办人视图</span></div>
      <div class="user-role" id="staff-role">采购部 · 负责 87 部节目 · 加入时间 2024-03</div>
    </div>
  </div>

  <div class="banner">
    <div class="banner-icon">i</div>
    <div><strong>视图说明：</strong>本工作台只显示你负责的 87 部节目数据。需要看全公司数据，请切换到「管理员视图」。所有跳转到的子页面会自动应用「我的节目」筛选。</div>
  </div>

  <div id="staff-kpis" class="kpi-grid"></div>

  <div class="cc">
    <div class="sec">我的待办（按优先级排序）</div>
    <div id="staff-todos"></div>
  </div>

  <div class="split">
    <div class="cc" style="margin:0">
      <div class="sec">快捷入口</div>
      <div id="staff-quick-actions"></div>
    </div>
    <div class="cc" style="margin:0">
      <div class="sec">我的 TOP 5 节目（本季度）</div>
      <table class="dt" id="staff-top5"></table>
    </div>
  </div>
</section>


<!-- ============ Page 7: Channel Distribution ============ -->
<section class="page" id="page-chdist">
  <div class="crumb"><a onclick="switchPage('cockpit')">驾驶舱</a> / 渠道节目分发分析</div>
  <div class="fbar">
    <span class="fbar-lbl">筛选</span>
    <select id="chdist-filter-cat"><option value="all">全部大类</option><option value="OTT">OTT</option><option value="IPTV">IPTV</option></select>
    <select id="chdist-sort"><option value="online_n">按在线节目数</option><option value="rate_auth">按授权率</option><option value="rate_full">按全表率</option><option value="grade_A">按 A 级数</option></select>
    <span class="sp"></span>
    <button>导出</button>
  </div>
  <div class="page-title">
    <div>
      <h1>渠道节目分发分析</h1>
      <div class="page-subtitle">29 渠道节目数 / 质量结构 / 上线率（双口径）/ 卡点归因 · 主表 2,338 部</div>
    </div>
  </div>
  <div id="chdist-kpis" class="kpi-grid"></div>

  <div class="banner">
    <div class="banner-icon">i</div>
    <div><strong>双口径说明：</strong>本页同时计算两种上线率 — <strong>「授权率」</strong>= 已上线 / 授权范围内节目（衡量真实分发执行力）；<strong>「全表率」</strong>= 已上线 / 主表全量（衡量分发广度）。两者并列呈现避免误读。</div>
  </div>

  <div class="cc">
    <div class="sec">29 渠道分发概况（按在线节目数排序 · 点击行查看渠道信息卡）</div>
    <div style="overflow-x:auto">
      <table class="dt" id="chdist-table"></table>
    </div>
    <div class="note" id="chdist-note"></div>
  </div>

  <div class="split">
    <div class="cc" style="margin:0">
      <div class="sec">渠道节目质量结构对比（TOP 12 在线渠道）</div>
      <div class="cw" style="height:280px"><canvas id="chart-chdist-quality"></canvas></div>
      <div class="note">每条柱按 ABC 等级堆叠 · 越偏左是头部内容占比越高</div>
    </div>
    <div class="cc" style="margin:0">
      <div class="sec">29 渠道未上线归因总览</div>
      <div class="cw" style="height:280px"><canvas id="chart-chdist-reasons"></canvas></div>
      <div class="note" id="chdist-reasons-note"></div>
    </div>
  </div>
</section>


<!-- ============ Channel Info Card Modal ============ -->
<div class="drawer-overlay" id="chcard-overlay" onclick="closeChannelCard()"></div>
<div id="chcard-modal" style="display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);width:90vw;max-width:780px;max-height:85vh;background:white;border-radius:var(--radius-lg);z-index:102;overflow:hidden;box-shadow:0 8px 32px rgba(0,0,0,0.2);">
  <div style="display:flex;justify-content:space-between;align-items:center;padding:16px 20px;border-bottom:0.5px solid var(--border-tertiary);background:var(--bg-secondary);">
    <div>
      <div id="chcard-title" style="font-size:16px;font-weight:500;">—</div>
      <div id="chcard-meta" style="font-size:11px;color:var(--text-tertiary);margin-top:2px;">—</div>
    </div>
    <button onclick="closeChannelCard()" style="background:none;border:none;cursor:pointer;font-size:22px;color:var(--text-tertiary);">×</button>
  </div>
  <div id="chcard-body" style="padding:18px 20px;overflow-y:auto;max-height:calc(85vh - 60px);"></div>
</div>

<!-- ============ Drawer ============ -->
<div class="drawer-overlay" id="drawer-overlay" onclick="closeDrawer()"></div>
<aside class="drawer" id="drawer">
  <div class="drawer-header">
    <div>
      <div class="drawer-title" id="drawer-title">—</div>
      <div class="drawer-meta" id="drawer-meta">—</div>
    </div>
    <button class="drawer-close" onclick="closeDrawer()">×</button>
  </div>
  <div class="drawer-body" id="drawer-body"></div>
  <div class="drawer-actions" id="drawer-actions"></div>
</aside>

<footer class="demo-footer">
运营数据看板 Demo · 静态演示版 · 数据来自历史聚合分析 · 真实部署需配合后端服务和实时数据源
</footer>

</div>

<script>
const DATA = {"cockpit": {"kpis": [{"label": "年度运营达成率", "value": "70.4%", "sub": "590.4 / 838.2 万 · 严重", "tone": "danger"}, {"label": "采销 ROI", "value": "待补", "sub": "需采购成本数据", "tone": "info"}, {"label": "A 级节目库存", "value": "39 部", "sub": "占库 1.7% · 跨渠道上线率 84%", "tone": "success"}, {"label": "12 月内版权到期", "value": "142 部", "sub": "6.1% · 含 5 部 A 级", "tone": "warn"}], "monthly_trend": [{"month": "26.01", "actual": 195.7, "cumActual": 195.7, "target": 279.4}, {"month": "26.02", "actual": 188.3, "cumActual": 384.0, "target": 558.8}, {"month": "26.03", "actual": 206.4, "cumActual": 590.4, "target": 838.2}, {"month": "26.04 预估", "actual": 210.0, "cumActual": 800.4, "target": 1117.5}], "gap": [{"cat": "IPTV", "target": 401.5, "done": 268.7, "gap": 132.8, "pct": 53.6}, {"cat": "OTT", "target": 380.7, "done": 268.4, "gap": 112.3, "pct": 45.3}, {"cat": "手机", "target": 56.0, "done": 53.3, "gap": 2.7, "pct": 1.1}], "status": [{"k": "达标 (≥100%)", "n": 1, "desc": "小米 OTT 单家超额完成", "tone": "success"}, {"k": "预警 (60-100%)", "n": 14, "desc": "完成度尚可但有缺口", "tone": "warn"}, {"k": "严重 (<60%)", "n": 9, "desc": "需重点关注 + 紧急介入", "tone": "danger"}, {"k": "0 出账", "n": 19, "desc": "本季度无开票流水", "tone": "danger"}, {"k": "未启动账期", "n": 5, "desc": "账期未到 · 不参与达成率算分", "tone": "neutral"}]}, "channel": {"kpis": [{"label": "Q1 总开票", "value": "590.4 万", "sub": "达成率 70.4% · 缺口 247.8 万", "tone": "info"}, {"label": "出账渠道率", "value": "34.5%", "sub": "10 / 29 渠道有流水", "tone": "warn"}, {"label": "TOP3 集中度", "value": "73%", "sub": "小米 + 未来电视 + 海信", "tone": "info"}, {"label": "高风险渠道数", "value": "9 个", "sub": "严重状态 · <60% 达成率", "tone": "danger"}], "top10": [{"rank": 1, "ch": "小米", "cat": "OTT", "target": 280.0, "actual": 287.92, "rate": 1.028, "yoy": 0.18, "qoq": 0.12, "status": "达标"}, {"rank": 2, "ch": "未来电视", "cat": "IPTV", "target": 100.0, "actual": 75.31, "rate": 0.753, "yoy": -0.05, "qoq": 0.02, "status": "预警"}, {"rank": 3, "ch": "海信", "cat": "OTT", "target": 60.0, "actual": 65.5, "rate": 1.092, "yoy": 0.21, "qoq": 0.18, "status": "达标"}, {"rank": 4, "ch": "酷开", "cat": "OTT", "target": 50.0, "actual": 38.4, "rate": 0.768, "yoy": -0.08, "qoq": -0.05, "status": "预警"}, {"rank": 5, "ch": "风行", "cat": "OTT", "target": 35.0, "actual": 28.3, "rate": 0.809, "yoy": 0.04, "qoq": 0.06, "status": "预警"}, {"rank": 6, "ch": "河南IPTV", "cat": "IPTV", "target": 40.0, "actual": 25.1, "rate": 0.628, "yoy": -0.15, "qoq": -0.1, "status": "预警"}, {"rank": 7, "ch": "康佳", "cat": "OTT", "target": 30.0, "actual": 22.4, "rate": 0.747, "yoy": -0.2, "qoq": -0.05, "status": "预警"}, {"rank": 8, "ch": "TCL", "cat": "OTT", "target": 25.0, "actual": 18.6, "rate": 0.744, "yoy": -0.25, "qoq": -0.18, "status": "预警"}, {"rank": 9, "ch": "甘肃IPTV", "cat": "IPTV", "target": 25.0, "actual": 14.2, "rate": 0.568, "yoy": -0.35, "qoq": -0.2, "status": "严重"}, {"rank": 10, "ch": "酷狗", "cat": "手机", "target": 30.0, "actual": 28.1, "rate": 0.937, "yoy": 0.02, "qoq": 0.04, "status": "预警"}], "category": [{"cat": "OTT", "n": 8, "target": 380.7, "actual": 268.4, "rate": 0.705}, {"cat": "IPTV", "n": 19, "target": 401.5, "actual": 268.7, "rate": 0.669}, {"cat": "手机", "n": 2, "target": 56.0, "actual": 53.3, "rate": 0.952}]}, "library": {"kpis": [{"label": "主库节目总量", "value": "2,338 部", "sub": "跨渠道记录 56,676 条", "tone": "info"}, {"label": "A 级头部战略", "value": "39 部", "sub": "占库 1.7% · 跨渠道上线率 84%", "tone": "success"}, {"label": "长尾 C 级", "value": "1,833 部", "sub": "占库 78.4% · 跨渠道上线率 24%", "tone": "warn"}, {"label": "真实运营卡点", "value": "3,894 部次", "sub": "剔除结构性重复后", "tone": "danger"}], "pyramid": [{"g": "A", "n": 39, "pct": 1.7, "rate": 84}, {"g": "A-", "n": 2, "pct": 0.1, "rate": 67}, {"g": "B+", "n": 47, "pct": 2.0, "rate": 44}, {"g": "B", "n": 362, "pct": 15.5, "rate": 51}, {"g": "C", "n": 1833, "pct": 78.4, "rate": 24}, {"g": "未分级", "n": 14, "pct": 0.6, "rate": 0}], "reasons": [{"k": "结构性重复", "n": 3945, "pct": 51.5, "cat": "行业结构", "isCard": false}, {"k": "平台未选 / 业务决策", "n": 1211, "pct": 15.8, "cat": "流程问题", "isCard": true}, {"k": "无权 / 权利不符", "n": 1106, "pct": 14.4, "cat": "权利问题", "isCard": true}, {"k": "无号 / 号在申请中", "n": 894, "pct": 11.7, "cat": "权利问题", "isCard": true}, {"k": "介质问题", "n": 683, "pct": 8.9, "cat": "流程问题", "isCard": true}, {"k": "流程中 / 等通知", "n": 29, "pct": 0.4, "cat": "流程问题", "isCard": true}], "sample_programs": [{"name": "超级飞侠 17 史前大救援", "grade": "A", "channels": 2, "rate": 86, "vendor": "广州奥飞", "revenue": 113.0}, {"name": "萌鸡小队 6", "grade": "A", "channels": 1, "rate": 71, "vendor": "北京小鸡彩虹", "revenue": 143.8}, {"name": "萌鸡小队", "grade": "B+", "channels": 2, "rate": 79, "vendor": "北京小鸡彩虹", "revenue": 189.2}, {"name": "帮帮龙探险先锋 2", "grade": "B", "channels": 6, "rate": 85, "vendor": "北京方块动漫", "revenue": 49.0}, {"name": "八爪鱼动画世界", "grade": "C", "channels": 2, "rate": 61, "vendor": "陕西聚量", "revenue": 31.0, "tag": "评级反例"}, {"name": "小公主戴安娜", "grade": null, "channels": 4, "rate": 28, "vendor": "上海乐普拉", "revenue": 84.5, "tag": "已下线"}, {"name": "帮帮龙出动 10 海洋救援队", "grade": "A", "channels": 6, "rate": 93, "vendor": "北京方块动漫", "revenue": 25.0}, {"name": "菲梦少女", "grade": "B", "channels": 2, "rate": 72, "vendor": "广州奥飞", "revenue": 47.7}]}, "revenue": {"kpis_row1": [{"label": "累计节目总收益", "value": "3,015.7 万", "sub": "1,203 部已识别 + 1,605 项孤儿", "pct": "100%", "tone": "info"}, {"label": "A 级总收益", "value": "716.5 万", "sub": "31 部 · ARPU 23.1 万/部", "pct": "23.8%", "tone": "success"}, {"label": "B 级总收益（B+B+）", "value": "992.8 万", "sub": "239 部 · ARPU 4.2 万/部", "pct": "32.9%", "tone": "purple"}, {"label": "C 级总收益", "value": "397.6 万", "sub": "902 部 · ARPU 0.4 万/部", "pct": "13.2%", "tone": "warn"}], "kpis_row2": [{"label": "头部贡献度（A+B 系列）", "value": "1,709.3 万", "sub": "270 部 · 占库 23% · 创收 57%", "pct": "56.7%", "tone": "success"}, {"label": "A 级占比（最新季度）", "value": "44.2%", "sub": "24Q1 13.8% → 25Q2 44.2%", "pct": "↑ +30.4pt", "tone": "success"}, {"label": "已下线节目收益", "value": "234.8 万", "sub": "26 部 · 主在 TCL/小米/酷开", "pct": "7.8%", "tone": "danger"}, {"label": "孤儿节目收益", "value": "674.0 万", "sub": "1,605 项 · 待主表补登", "pct": "22.4%", "tone": "warn"}], "quarterly_breakdown": [{"q": "2024 Q1", "A": 87.6, "B": 188.7, "C": 88.0, "down": 89.0, "orphan": 180.9, "total": 634.2, "aPct": 13.8}, {"q": "2024 Q2", "A": 84.0, "B": 187.1, "C": 78.5, "down": 70.7, "orphan": 146.8, "total": 567.1, "aPct": 14.8}, {"q": "2024 Q3", "A": 69.1, "B": 163.4, "C": 60.7, "down": 39.8, "orphan": 107.4, "total": 440.4, "aPct": 15.7}, {"q": "2024 Q4", "A": 105.6, "B": 174.5, "C": 52.5, "down": 30.6, "orphan": 102.1, "total": 465.3, "aPct": 22.7}, {"q": "2025 Q1", "A": 189.8, "B": 154.0, "C": 71.9, "down": 1.5, "orphan": 73.3, "total": 490.5, "aPct": 38.7}, {"q": "2025 Q2", "A": 170.4, "B": 114.5, "C": 37.4, "down": 1.5, "orphan": 62.0, "total": 385.8, "aPct": 44.2}], "top_programs": [{"rank": 1, "name": "萌鸡小队", "grade": "B+", "channels": [187.54, 0, 0, 0, 0, 1.69], "quarters": [42.61, 31.57, 21.4, 27.18, 36.32, 29.98, 0.18], "total": 189.23}, {"rank": 2, "name": "超级飞侠 全集", "grade": "孤儿", "channels": [149.26, 0, 0, 0, 0, 0], "quarters": [14.51, 10.59, 22.67, 37.51, 36.35, 27.64, 0], "total": 149.26}, {"rank": 3, "name": "萌鸡小队 6", "grade": "A", "channels": [143.75, 0, 0, 0, 0, 0], "quarters": [0, 0, 0, 0.92, 82.2, 60.63, 0], "total": 143.75}, {"rank": 4, "name": "超级飞侠 16 电能集结", "grade": "A", "channels": [117.06, 0, 0, 0, 0, 12.21], "quarters": [0, 32.17, 34.94, 24.91, 19.89, 16.61, 0.76], "total": 129.28}, {"rank": 5, "name": "超级飞侠 17 史前大救援", "grade": "A", "channels": [99.24, 0, 0, 0, 0, 13.78], "quarters": [0, 0, 0, 33.12, 48.57, 30.11, 1.22], "total": 113.02}, {"rank": 6, "name": "小公主戴安娜", "grade": "已下线", "channels": [32.28, 0, 8.11, 43.26, 0.86, 0], "quarters": [36.58, 26.58, 21.14, 0.22, 0, 0, 0], "total": 84.51}, {"rank": 7, "name": "超级飞侠 14", "grade": "A", "channels": [45.14, 0, 0, 0, 0, 6.14], "quarters": [21.63, 14.61, 4.99, 3.9, 3.2, 2.16, 0.79], "total": 51.29}, {"rank": 8, "name": "帮帮龙探险先锋 2", "grade": "B", "channels": [32.99, 4.91, 4.41, 0.94, 1.12, 4.62], "quarters": [1.42, 23.8, 13.17, 5.05, 2.74, 2.69, 0.12], "total": 48.99}, {"rank": 9, "name": "菲梦少女", "grade": "B", "channels": [45.44, 0, 0, 0, 0, 2.26], "quarters": [7.09, 5.55, 11.0, 6.91, 8.56, 8.26, 0.34], "total": 47.7}, {"rank": 10, "name": "超级飞侠 15", "grade": "A", "channels": [35.83, 0, 0, 0, 0, 9.99], "quarters": [26.52, 8.03, 3.51, 3.3, 2.64, 1.35, 0.46], "total": 45.81}, {"rank": 11, "name": "23 号牛乃唐第二季", "grade": "B+", "channels": [39.1, 1.08, 1.27, 0.86, 0, 0], "quarters": [3.99, 3.24, 6.12, 14.62, 8.2, 6.14, 0], "total": 42.31}, {"rank": 12, "name": "龙战士星源 第3季", "grade": "B", "channels": [29.81, 3.73, 1.93, 0, 0.93, 3.07], "quarters": [0, 0, 0, 22.22, 10.28, 6.53, 0.44], "total": 39.46}, {"rank": 13, "name": "萌鸡小队 3", "grade": "A", "channels": [35.13, 0, 0, 0, 0, 4.07], "quarters": [10.21, 7.93, 5.17, 5.05, 4.38, 5.86, 0.61], "total": 39.2}, {"rank": 14, "name": "搞怪兄弟弗拉德与尼基合集", "grade": "已下线", "channels": [0, 0, 4.07, 29.67, 2.53, 0], "quarters": [18.2, 15.37, 2.07, 0.63, 0, 0, 0], "total": 36.27}, {"rank": 15, "name": "汽车星球", "grade": "B", "channels": [23.07, 4.27, 4.38, 0.05, 0, 0.9], "quarters": [6.22, 3.21, 4.98, 11.16, 4.41, 2.62, 0.08], "total": 32.67}]}, "rights": {"kpis": [{"label": "合作版权商数", "value": "186 家", "sub": "TOP10 集中度 44%", "tone": "info"}, {"label": "TOP3 集中度", "value": "21%", "sub": "491 部 · 战略级供应商", "tone": "success"}, {"label": "12 月内到期", "value": "142 部", "sub": "6.1% · 续约重点池", "tone": "danger"}, {"label": "全网权利占比", "value": "34%", "sub": "795 部 · 跨平台分发能力", "tone": "success"}], "top_vendors": [{"rank": 1, "vendor": "陕西聚量", "n": 328, "pct": 14.0, "cum": 14.0, "tier": "战略级"}, {"rank": 2, "vendor": "北京爱升", "n": 87, "pct": 3.7, "cum": 17.7, "tier": "战略级"}, {"rank": 3, "vendor": "广州奥飞动漫", "n": 76, "pct": 3.3, "cum": 21.0, "tier": "战略级"}, {"rank": 4, "vendor": "北京小鸡彩虹", "n": 52, "pct": 2.2, "cum": 23.2, "tier": "观察"}, {"rank": 5, "vendor": "上海乐普拉", "n": 47, "pct": 2.0, "cum": 25.2, "tier": "观察"}, {"rank": 6, "vendor": "北京方块动漫", "n": 41, "pct": 1.8, "cum": 27.0, "tier": "观察"}, {"rank": 7, "vendor": "浙江中录文化", "n": 38, "pct": 1.6, "cum": 28.6, "tier": "观察"}, {"rank": 8, "vendor": "广东咏声动漫", "n": 35, "pct": 1.5, "cum": 30.1, "tier": "观察"}, {"rank": 9, "vendor": "江苏卡龙文化", "n": 32, "pct": 1.4, "cum": 31.5, "tier": "观察"}, {"rank": 10, "vendor": "湖南金鹰卡通", "n": 28, "pct": 1.2, "cum": 32.7, "tier": "观察"}], "expiry_buckets": [{"b": "已到期", "n": 18, "A": 1, "B": 3, "C": 14, "urgency": "已逾期", "tone": "danger"}, {"b": "0-3 月", "n": 35, "A": 2, "B": 9, "C": 24, "urgency": "紧急", "tone": "danger"}, {"b": "3-6 月", "n": 42, "A": 1, "B": 11, "C": 30, "urgency": "高", "tone": "warn"}, {"b": "6-12 月", "n": 47, "A": 1, "B": 12, "C": 34, "urgency": "中", "tone": "warn"}, {"b": "12-24 月", "n": 183, "A": 5, "B": 45, "C": 133, "urgency": "低", "tone": "neutral"}, {"b": "24+ 月", "n": 2013, "A": 29, "B": 329, "C": 1655, "urgency": "低", "tone": "neutral"}], "license_types": [{"k": "非独家", "n": 1456, "pct": 62.3}, {"k": "独家不含转授", "n": 453, "pct": 19.4}, {"k": "独家含转授", "n": 238, "pct": 10.2}, {"k": "非独不含转授", "n": 148, "pct": 6.3}, {"k": "其他", "n": 43, "pct": 1.8}]}, "data_mgmt": {"kpis": [{"label": "已接入数据表", "value": "7 / 8 计划", "sub": "采购成本表待接入", "tone": "info"}, {"label": "数据完整度", "value": "95.3%", "sub": "53,995 / 56,676 条对齐主表", "tone": "success"}, {"label": "待处理异常", "value": "1,777 项", "sub": "19 fuzzy review + 1,758 孤儿", "tone": "warn"}, {"label": "本月上传次数", "value": "23 次", "sub": "最近 2026.05.03 09:15", "tone": "info"}], "tables": [{"name": "主表全量片单", "rows": 2338, "completeness": 99.4, "updated": "2026.04.17", "status": "good", "desc": "所有页面的等级权威源"}, {"name": "26 渠道资料表", "rows": 17, "completeness": 88.0, "updated": "2026.04.10", "status": "good", "desc": "渠道详情抽屉数据源"}, {"name": "26Q1 渠道指标表", "rows": 29, "completeness": 100.0, "updated": "2026.05.03", "status": "good", "desc": "驾驶舱+渠道经营核心源"}, {"name": "28 渠道节目片单", "rows": 56676, "completeness": 95.3, "updated": "2026.04.16", "status": "good", "desc": "节目库页跨渠道数据"}, {"name": "渠道节目收益表", "rows": 61939, "completeness": 70.0, "updated": "2026.04.16", "status": "warn", "desc": "部分渠道数据不完整"}, {"name": "26 全年目标表", "rows": 29, "completeness": 100.0, "updated": "2026.01.10", "status": "good", "desc": "驾驶舱达成率算分依据"}, {"name": "采购成本表", "rows": 0, "completeness": 0, "updated": "—", "status": "bad", "desc": "未接入 · ROI 分析关键缺口"}], "exceptions": [{"k": "Fuzzy 拒绝匹配", "n": 19, "desc": "\"魔法泡泡美人鱼第 1 季中文版\" vs \"英文版\" 等 19 项", "action": "人工 review"}, {"k": "主表外孤儿节目", "n": 1758, "desc": "渠道有但主表无 · 占未匹配主体", "action": "导出清单"}, {"k": "渠道资料缺失", "n": 3, "desc": "芒果 OTT / 湖南 IPTV / 华数 未在线节目数据待补", "action": "通知运营"}, {"k": "收益数据不完整", "n": 3, "desc": "TCL 仅 2024 年 · 康佳仅年度汇总", "action": "查看推算"}], "upload_log": [{"time": "05.03 09:15", "table": "26Q1 渠道指标表", "rows": 29, "user": "管理员 张明", "version": "v12", "status": "已生效"}, {"time": "04.17 14:32", "table": "主表全量片单", "rows": 2338, "user": "管理员 张明", "version": "v8", "status": "已生效"}, {"time": "04.16 18:05", "table": "渠道节目收益", "rows": 61939, "user": "管理员 张明", "version": "v3", "status": "已生效"}, {"time": "04.16 16:42", "table": "28 渠道节目片单", "rows": 56676, "user": "管理员 张明", "version": "v3", "status": "已生效"}, {"time": "04.10 11:20", "table": "26 渠道资料表", "rows": 17, "user": "经办人 李芳", "version": "v5", "status": "已生效"}, {"time": "04.05 09:45", "table": "主表全量片单", "rows": 2341, "user": "管理员 张明", "version": "v7", "status": "已替换"}]}, "staff_workspace": {"user": {"name": "严俏城", "dept": "采购部", "programs": 87, "since": "2024-03"}, "kpis": [{"label": "我负责的节目数", "value": "87 部", "sub": "A 7 / B 18 / C 62 部", "tone": "info"}, {"label": "本季度收益（我）", "value": "142.3 万", "sub": "同比 +18% · 跨 6 渠道", "tone": "success"}, {"label": "待续约（90 天内）", "value": "5 部", "sub": "最近：超级飞侠 1 · 26 天后", "tone": "danger"}, {"label": "待我处理", "value": "8 项", "sub": "3 高优先 + 5 普通", "tone": "warn"}], "todos": [{"pri": "high", "title": "续约「超级飞侠 1」 · 广州奥飞", "desc": "A 级 · 6 渠道分发 · 影响 31.5 万 / 季度", "due": "2026.05.29", "action": "触发续约"}, {"pri": "high", "title": "续约「迟到大王」 · 浙江中录文化", "desc": "C 级 · 跨 23 渠道 · 上线率 48%", "due": "2026.06.15", "action": "触发续约"}, {"pri": "high", "title": "补登「萌鸡小队 6」主表元数据", "desc": "渠道侧已有数据但主表缺等级", "due": "本周", "action": "去补登"}, {"pri": "mid", "title": "Review fuzzy 匹配 · 5 项", "desc": "版本歧义需人工 review", "due": "本月内", "action": "去 review"}, {"pri": "mid", "title": "「八爪鱼动画世界」评级建议提级", "desc": "C 级反例 · 实际收入 29.9 万", "due": "本月内", "action": "提交建议"}, {"pri": "low", "title": "上传 26Q2 我的节目数据", "desc": "季末 5 个工作日内", "due": "7 月初", "action": "上传"}], "top5": [{"name": "超级飞侠 17", "g": "A", "rev": 30.1, "qoq": -38}, {"name": "超级飞侠 16", "g": "A", "rev": 16.6, "qoq": -17}, {"name": "23 号牛乃唐 2", "g": "B+", "rev": 6.1, "qoq": -25}, {"name": "迟到大王", "g": "C", "rev": 2.8, "qoq": -12}, {"name": "毛毛虫列车", "g": "C", "rev": 2.5, "qoq": -10}]}, "channel_dist": [{"channel": "银河", "category": "IPTV", "online_n": 656, "offline_n": 1685, "authorized_n": 2129, "rate_auth": 30.8, "rate_full": 28.1, "grade_A": 8, "grade_Am": 1, "grade_Bp": 16, "grade_B": 196, "grade_C": 435, "grade_unrated": 0, "top_reasons": [["未知", 1685]], "top_vendors": [["南京酷艺讯数字传媒有限公司", 50], ["天津图特网络科技有限公司", 46], ["北京爱升影视传媒有限公司", 38], ["北京时光飞鱼文化传媒有限公司", 32], ["央视动漫集团有限公司", 22]], "split_n": 746, "split_pct": 53.2, "total_online_with_split": 1402, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "华为", "category": "OTT", "online_n": 705, "offline_n": 1630, "authorized_n": 2186, "rate_auth": 32.3, "rate_full": 30.2, "grade_A": 26, "grade_Am": 0, "grade_Bp": 10, "grade_B": 140, "grade_C": 529, "grade_unrated": 0, "top_reasons": [["未知", 1630]], "top_vendors": [["陕西聚量文化传媒有限公司", 114], ["广州奥飞动漫文化传播有限公司", 67], ["天津图特网络科技有限公司", 37], ["南京酷艺讯数字传媒有限公司", 36], ["北京爱升影视传媒有限公司", 27]], "split_n": 472, "split_pct": 40.1, "total_online_with_split": 1177, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "未来电视", "category": "IPTV", "online_n": 726, "offline_n": 1612, "authorized_n": 2125, "rate_auth": 34.2, "rate_full": 31.1, "grade_A": 30, "grade_Am": 2, "grade_Bp": 20, "grade_B": 231, "grade_C": 443, "grade_unrated": 0, "top_reasons": [["未知", 1612]], "top_vendors": [["陕西聚量文化传媒有限公司", 119], ["广州奥飞动漫文化传播有限公司", 63], ["北京时光飞鱼文化传媒有限公司", 30], ["北京爱升影视传媒有限公司", 27], ["厦门孩子乐园网络科技有限公司", 25]], "split_n": 322, "split_pct": 30.7, "total_online_with_split": 1048, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "酷开", "category": "OTT", "online_n": 918, "offline_n": 1420, "authorized_n": 2101, "rate_auth": 39.5, "rate_full": 39.3, "grade_A": 13, "grade_Am": 2, "grade_Bp": 20, "grade_B": 202, "grade_C": 683, "grade_unrated": 0, "top_reasons": [["重复", 625], ["平台未选", 257], ["无权力", 179], ["无号或号在申请中无法上线", 133], ["介质不可用", 83], ["介质处理中", 41], ["题材平台不适合", 31], ["当时采购反馈先不要上了", 28]], "top_vendors": [["陕西聚量文化传媒有限公司", 141], ["天津图特网络科技有限公司", 92], ["南京酷艺讯数字传媒有限公司", 54], ["北京圆柱文化科技有限公司", 40], ["北京爱升影视传媒有限公司", 31]], "split_n": 0, "split_pct": 0, "total_online_with_split": 918, "data_source": "v3 28渠道节目片单"}, {"channel": "山东IPTV", "category": "IPTV", "online_n": 916, "offline_n": 630, "authorized_n": 2125, "rate_auth": 32.1, "rate_full": 39.2, "grade_A": 6, "grade_Am": 0, "grade_Bp": 11, "grade_B": 168, "grade_C": 733, "grade_unrated": 0, "top_reasons": [["未知", 630]], "top_vendors": [["陕西聚量文化传媒有限公司", 238], ["天津图特网络科技有限公司", 57], ["上海乐普拉文化传媒有限公司", 37], ["北京圆柱文化科技有限公司", 33], ["北京爱升影视传媒有限公司", 29]], "split_n": 0, "split_pct": 0, "total_online_with_split": 916, "data_source": "v3 28渠道节目片单"}, {"channel": "芒果OTT", "category": "OTT", "online_n": 655, "offline_n": 0, "authorized_n": 2080, "rate_auth": 31.5, "rate_full": 28.0, "grade_A": 3, "grade_Am": 0, "grade_Bp": 4, "grade_B": 124, "grade_C": 524, "grade_unrated": 0, "top_reasons": [], "top_vendors": [["陕西聚量文化传媒有限公司", 237], ["天津图特网络科技有限公司", 57], ["北京圆柱文化科技有限公司", 16], ["澳通（杭州）科技发展有限公司", 12], ["河南双曲线教育科技有限公司", 12]], "split_n": 169, "split_pct": 20.5, "total_online_with_split": 824, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "广西IPTV", "category": "IPTV", "online_n": 775, "offline_n": 1275, "authorized_n": 2125, "rate_auth": 32.5, "rate_full": 33.1, "grade_A": 12, "grade_Am": 0, "grade_Bp": 14, "grade_B": 142, "grade_C": 606, "grade_unrated": 1, "top_reasons": [["未知", 1281]], "top_vendors": [["陕西聚量文化传媒有限公司", 187], ["天津图特网络科技有限公司", 60], ["北京圆柱文化科技有限公司", 32], ["北京爱升影视传媒有限公司", 30], ["海宁云逸影视文化有限公司", 25]], "split_n": 0, "split_pct": 0, "total_online_with_split": 775, "data_source": "v3 28渠道节目片单"}, {"channel": "TCL", "category": "OTT", "online_n": 760, "offline_n": 1578, "authorized_n": 2095, "rate_auth": 31.5, "rate_full": 32.5, "grade_A": 20, "grade_Am": 2, "grade_Bp": 22, "grade_B": 213, "grade_C": 505, "grade_unrated": 0, "top_reasons": [["重复", 603], ["平台未选", 401], ["无权力", 178], ["无号或号在申请中无法上线", 149], ["介质不可用", 99], ["介质处理中", 41], ["当时采购反馈先不要上了", 33], ["题材平台不适合", 31]], "top_vendors": [["陕西聚量文化传媒有限公司", 102], ["南京酷艺讯数字传媒有限公司", 53], ["天津图特网络科技有限公司", 37], ["北京时光飞鱼文化传媒有限公司", 33], ["北京圆柱文化科技有限公司", 32]], "split_n": 0, "split_pct": 0, "total_online_with_split": 760, "data_source": "v3 28渠道节目片单"}, {"channel": "康佳", "category": "OTT", "online_n": 747, "offline_n": 1592, "authorized_n": 2101, "rate_auth": 32.0, "rate_full": 32.0, "grade_A": 11, "grade_Am": 2, "grade_Bp": 22, "grade_B": 228, "grade_C": 486, "grade_unrated": 0, "top_reasons": [["重复", 980], ["无号或号在申请中无法上线", 197], ["无权力", 167], ["介质不可用", 95], ["介质处理中", 41], ["当时采购反馈先不要上了", 33], ["平台未选", 31], ["题材平台不适合", 31]], "top_vendors": [["陕西聚量文化传媒有限公司", 113], ["天津图特网络科技有限公司", 47], ["北京时光飞鱼文化传媒有限公司", 33], ["北京爱升影视传媒有限公司", 28], ["央视动漫集团有限公司", 24]], "split_n": 0, "split_pct": 0, "total_online_with_split": 747, "data_source": "v3 28渠道节目片单"}, {"channel": "海信", "category": "OTT", "online_n": 676, "offline_n": 1652, "authorized_n": 2098, "rate_auth": 32.2, "rate_full": 28.9, "grade_A": 16, "grade_Am": 2, "grade_Bp": 19, "grade_B": 206, "grade_C": 433, "grade_unrated": 0, "top_reasons": [["未知", 1652]], "top_vendors": [["南京酷艺讯数字传媒有限公司", 51], ["哈尔滨抱抱熊动画设计有限公司", 50], ["北京时光飞鱼文化传媒有限公司", 31], ["宁波卡酷动画制作有限公司", 28], ["北京爱升影视传媒有限公司", 18]], "split_n": 73, "split_pct": 9.7, "total_online_with_split": 749, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "小米", "category": "OTT", "online_n": 745, "offline_n": 1593, "authorized_n": 2186, "rate_auth": 31.2, "rate_full": 31.9, "grade_A": 26, "grade_Am": 2, "grade_Bp": 16, "grade_B": 254, "grade_C": 447, "grade_unrated": 0, "top_reasons": [["重复", 987], ["无号或号在申请中无法上线", 160], ["无权力", 101], ["介质不可用无法上线", 95], ["平台未选", 88], ["介质处理中", 41], ["当时采购反馈先不要上了", 33], ["题材平台不适合", 31]], "top_vendors": [["南京酷艺讯数字传媒有限公司", 58], ["陕西聚量文化传媒有限公司", 47], ["广州奥飞动漫文化传播有限公司", 45], ["北京时光飞鱼文化传媒有限公司", 32], ["北京爱升影视传媒有限公司", 31]], "split_n": 0, "split_pct": 0, "total_online_with_split": 745, "data_source": "v3 28渠道节目片单"}, {"channel": "风行", "category": "OTT", "online_n": 677, "offline_n": 1661, "authorized_n": 2209, "rate_auth": 29.3, "rate_full": 29.0, "grade_A": 18, "grade_Am": 2, "grade_Bp": 19, "grade_B": 217, "grade_C": 421, "grade_unrated": 0, "top_reasons": [["重复", 750], ["无权力", 466], ["无号或号在申请中无法上线", 186], ["介质不可用", 91], ["平台未选", 54], ["介质处理中", 41], ["题材平台不适合", 32], ["当时采购反馈先不要上了", 26]], "top_vendors": [["天津图特网络科技有限公司", 58], ["南京酷艺讯数字传媒有限公司", 40], ["北京时光飞鱼文化传媒有限公司", 30], ["北京爱升影视传媒有限公司", 29], ["央视动漫集团有限公司", 25]], "split_n": 0, "split_pct": 0, "total_online_with_split": 677, "data_source": "v3 28渠道节目片单"}, {"channel": "重庆IPTV", "category": "IPTV", "online_n": 672, "offline_n": 1397, "authorized_n": 2125, "rate_auth": 25.1, "rate_full": 28.7, "grade_A": 45, "grade_Am": 1, "grade_Bp": 10, "grade_B": 128, "grade_C": 488, "grade_unrated": 0, "top_reasons": [["未知", 1403]], "top_vendors": [["陕西聚量文化传媒有限公司", 197], ["北京圆柱文化科技有限公司", 36], ["天津图特网络科技有限公司", 33], ["海宁云逸影视文化有限公司", 29], ["北京时光飞鱼文化传媒有限公司", 20]], "split_n": 0, "split_pct": 0, "total_online_with_split": 672, "data_source": "v3 28渠道节目片单"}, {"channel": "小度", "category": "OTT", "online_n": 613, "offline_n": 1750, "authorized_n": 2091, "rate_auth": 29.3, "rate_full": 26.2, "grade_A": 12, "grade_Am": 1, "grade_Bp": 11, "grade_B": 197, "grade_C": 392, "grade_unrated": 0, "top_reasons": [["未知", 1750]], "top_vendors": [["南京酷艺讯数字传媒有限公司", 39], ["哈尔滨抱抱熊动画设计有限公司", 36], ["北京时光飞鱼文化传媒有限公司", 32], ["天津图特网络科技有限公司", 29], ["杭州阿优文化科技有限公司", 18]], "split_n": 51, "split_pct": 7.7, "total_online_with_split": 664, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "云南IPTV", "category": "IPTV", "online_n": 608, "offline_n": 1156, "authorized_n": 2125, "rate_auth": 25.7, "rate_full": 26.0, "grade_A": 4, "grade_Am": 1, "grade_Bp": 17, "grade_B": 128, "grade_C": 456, "grade_unrated": 6, "top_reasons": [["未知", 1156]], "top_vendors": [["天津图特网络科技有限公司", 76], ["陕西聚量文化传媒有限公司", 71], ["北京爱升影视传媒有限公司", 30], ["海宁云逸影视文化有限公司", 26], ["北京时光飞鱼文化传媒有限公司", 23]], "split_n": 0, "split_pct": 0, "total_online_with_split": 608, "data_source": "v3 28渠道节目片单"}, {"channel": "吉林IPTV", "category": "IPTV", "online_n": 588, "offline_n": 1182, "authorized_n": 2125, "rate_auth": 20.8, "rate_full": 25.1, "grade_A": 16, "grade_Am": 1, "grade_Bp": 15, "grade_B": 154, "grade_C": 402, "grade_unrated": 0, "top_reasons": [["未知", 1182]], "top_vendors": [["南京酷艺讯数字传媒有限公司", 66], ["陕西聚量文化传媒有限公司", 58], ["天津图特网络科技有限公司", 46], ["北京时光飞鱼文化传媒有限公司", 29], ["北京爱升影视传媒有限公司", 22]], "split_n": 0, "split_pct": 0, "total_online_with_split": 588, "data_source": "v3 28渠道节目片单"}, {"channel": "河南IPTV", "category": "IPTV", "online_n": 565, "offline_n": 1519, "authorized_n": 2125, "rate_auth": 25.3, "rate_full": 24.2, "grade_A": 16, "grade_Am": 2, "grade_Bp": 15, "grade_B": 203, "grade_C": 329, "grade_unrated": 0, "top_reasons": [["未知", 1525]], "top_vendors": [["广州奥飞动漫文化传播有限公司", 37], ["央视动漫集团有限公司", 25], ["北京爱升影视传媒有限公司", 22], ["北京时光飞鱼文化传媒有限公司", 21], ["天津图特网络科技有限公司", 20]], "split_n": 0, "split_pct": 0, "total_online_with_split": 565, "data_source": "v3 28渠道节目片单"}, {"channel": "坚果", "category": "OTT", "online_n": 460, "offline_n": 1889, "authorized_n": 2094, "rate_auth": 22.0, "rate_full": 19.7, "grade_A": 12, "grade_Am": 1, "grade_Bp": 13, "grade_B": 176, "grade_C": 258, "grade_unrated": 0, "top_reasons": [["未知", 1889]], "top_vendors": [["北京时光飞鱼文化传媒有限公司", 32], ["上海今日动画影视文化有限公司", 29], ["北京爱升影视传媒有限公司", 26], ["杭州阿优文化科技有限公司", 17], ["上海淘米动画有限公司", 17]], "split_n": 66, "split_pct": 12.5, "total_online_with_split": 526, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "甘肃IPTV", "category": "IPTV", "online_n": 512, "offline_n": 1553, "authorized_n": 2125, "rate_auth": 20.3, "rate_full": 21.9, "grade_A": 20, "grade_Am": 1, "grade_Bp": 11, "grade_B": 133, "grade_C": 347, "grade_unrated": 0, "top_reasons": [["未知", 1560]], "top_vendors": [["上海乐普拉文化传媒有限公司", 45], ["海宁云逸影视文化有限公司", 43], ["澳通（杭州）科技发展有限公司", 28], ["深圳中鸿影视文化传媒有限公司", 15], ["北京时光飞鱼文化传媒有限公司", 15]], "split_n": 0, "split_pct": 0, "total_online_with_split": 512, "data_source": "v3 28渠道节目片单"}, {"channel": "湖南IPTV", "category": "IPTV", "online_n": 435, "offline_n": 0, "authorized_n": 2125, "rate_auth": 20.5, "rate_full": 18.6, "grade_A": 0, "grade_Am": 1, "grade_Bp": 8, "grade_B": 77, "grade_C": 349, "grade_unrated": 0, "top_reasons": [], "top_vendors": [["陕西聚量文化传媒有限公司", 163], ["天津图特网络科技有限公司", 49], ["北京圆柱文化科技有限公司", 23], ["美太芭比（上海）贸易有限公司", 19], ["深圳中鸿影视文化传媒有限公司", 11]], "split_n": 72, "split_pct": 14.2, "total_online_with_split": 507, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "江西IPTV", "category": "IPTV", "online_n": 467, "offline_n": 1607, "authorized_n": 2125, "rate_auth": 19.7, "rate_full": 20.0, "grade_A": 29, "grade_Am": 2, "grade_Bp": 16, "grade_B": 161, "grade_C": 263, "grade_unrated": 0, "top_reasons": [["未知", 1613]], "top_vendors": [["海宁云逸影视文化有限公司", 27], ["北京时光飞鱼文化传媒有限公司", 24], ["美太芭比（上海）贸易有限公司", 20], ["哈尔滨抱抱熊动画设计有限公司", 16], ["北京圆柱文化科技有限公司", 13]], "split_n": 0, "split_pct": 0, "total_online_with_split": 467, "data_source": "v3 28渠道节目片单"}, {"channel": "喜粤TV", "category": "IPTV", "online_n": 351, "offline_n": 1374, "authorized_n": 2125, "rate_auth": 13.2, "rate_full": 15.0, "grade_A": 3, "grade_Am": 0, "grade_Bp": 12, "grade_B": 104, "grade_C": 232, "grade_unrated": 0, "top_reasons": [["未知", 1374]], "top_vendors": [["陕西聚量文化传媒有限公司", 93], ["北京圆柱文化科技有限公司", 29], ["天津图特网络科技有限公司", 22], ["北京时光飞鱼文化传媒有限公司", 21], ["北京爱升影视传媒有限公司", 16]], "split_n": 0, "split_pct": 0, "total_online_with_split": 351, "data_source": "v3 28渠道节目片单"}, {"channel": "华数", "category": "IPTV", "online_n": 225, "offline_n": 0, "authorized_n": 2125, "rate_auth": 10.6, "rate_full": 9.6, "grade_A": 1, "grade_Am": 0, "grade_Bp": 0, "grade_B": 41, "grade_C": 183, "grade_unrated": 0, "top_reasons": [], "top_vendors": [["南京酷艺讯数字传媒有限公司", 39], ["哈尔滨抱抱熊动画设计有限公司", 25], ["澳通（杭州）科技发展有限公司", 12], ["广州灵动创想文化科技有限公司", 11], ["北京漫布星球文化传媒有限公司", 11]], "split_n": 90, "split_pct": 28.6, "total_online_with_split": 315, "data_source": "截止 2026.04.16 ZLY 上线片单"}, {"channel": "河北IPTV", "category": "IPTV", "online_n": 301, "offline_n": 1402, "authorized_n": 2125, "rate_auth": 8.6, "rate_full": 12.9, "grade_A": 4, "grade_Am": 2, "grade_Bp": 12, "grade_B": 80, "grade_C": 203, "grade_unrated": 0, "top_reasons": [["未知", 1402]], "top_vendors": [["陕西聚量文化传媒有限公司", 50], ["上海乐普拉文化传媒有限公司", 34], ["天津图特网络科技有限公司", 30], ["北京时光飞鱼文化传媒有限公司", 10], ["深圳中鸿影视文化传媒有限公司", 10]], "split_n": 0, "split_pct": 0, "total_online_with_split": 301, "data_source": "v3 28渠道节目片单"}, {"channel": "北京IPTV", "category": "IPTV", "online_n": 257, "offline_n": 1473, "authorized_n": 2125, "rate_auth": 8.2, "rate_full": 11.0, "grade_A": 14, "grade_Am": 1, "grade_Bp": 15, "grade_B": 133, "grade_C": 86, "grade_unrated": 8, "top_reasons": [["未知", 1473]], "top_vendors": [["北京时光飞鱼文化传媒有限公司", 27], ["哈尔滨抱抱熊动画设计有限公司", 14], ["广东咏声动漫股份有限公司", 13], ["央视动漫集团有限公司", 12], ["广东雷曦文化有限公司", 11]], "split_n": 0, "split_pct": 0, "total_online_with_split": 257, "data_source": "v3 28渠道节目片单"}, {"channel": "辽宁IPTV", "category": "IPTV", "online_n": 211, "offline_n": 1810, "authorized_n": 2125, "rate_auth": 9.7, "rate_full": 9.0, "grade_A": 10, "grade_Am": 2, "grade_Bp": 11, "grade_B": 76, "grade_C": 112, "grade_unrated": 0, "top_reasons": [["未知", 1813]], "top_vendors": [["央视动漫集团有限公司", 19], ["杭州漫视科技有限公司", 16], ["海宁云逸影视文化有限公司", 14], ["广东咏声动漫股份有限公司", 14], ["美太芭比（上海）贸易有限公司", 13]], "split_n": 0, "split_pct": 0, "total_online_with_split": 211, "data_source": "v3 28渠道节目片单"}, {"channel": "湖北IPTV", "category": "IPTV", "online_n": 190, "offline_n": 1129, "authorized_n": 2125, "rate_auth": 8.2, "rate_full": 8.1, "grade_A": 1, "grade_Am": 0, "grade_Bp": 6, "grade_B": 36, "grade_C": 138, "grade_unrated": 9, "top_reasons": [["未知", 1134]], "top_vendors": [["海宁云逸影视文化有限公司", 18], ["深圳中鸿影视文化传媒有限公司", 16], ["澳通（杭州）科技发展有限公司", 14], ["杭州漫视科技有限公司", 13], ["宁波智瞳文化传媒有限公司", 8]], "split_n": 0, "split_pct": 0, "total_online_with_split": 190, "data_source": "v3 28渠道节目片单"}, {"channel": "新疆IPTV", "category": "IPTV", "online_n": 133, "offline_n": 1403, "authorized_n": 2125, "rate_auth": 5.7, "rate_full": 5.7, "grade_A": 0, "grade_Am": 0, "grade_Bp": 6, "grade_B": 37, "grade_C": 90, "grade_unrated": 0, "top_reasons": [["未知", 1405]], "top_vendors": [["厦门孩子乐园网络科技有限公司", 31], ["央视动漫集团有限公司", 15], ["北京爱升影视传媒有限公司", 11], ["广东易腾动漫文化有限公司", 8], ["广东咏声动漫股份有限公司", 7]], "split_n": 0, "split_pct": 0, "total_online_with_split": 133, "data_source": "v3 28渠道节目片单"}, {"channel": "浙江IPTV", "category": "IPTV", "online_n": 42, "offline_n": 1965, "authorized_n": 2125, "rate_auth": 1.7, "rate_full": 1.8, "grade_A": 4, "grade_Am": 0, "grade_Bp": 11, "grade_B": 21, "grade_C": 5, "grade_unrated": 1, "top_reasons": [["未知", 1972]], "top_vendors": [["杭州友诺动漫有限公司", 6], ["美太芭比（上海）贸易有限公司", 4], ["深圳萌想文化传播有限公司", 4], ["广东雷曦文化有限公司", 3], ["广东咏声动漫股份有限公司", 3]], "split_n": 0, "split_pct": 0, "total_online_with_split": 42, "data_source": "v3 28渠道节目片单"}], "channel_dist_meta": {"kpis": [{"label": "29 渠道在线节目", "value": "15,586 部", "sub": "+ 拆条 2,061 · 未在线 38,937", "tone": "info"}, {"label": "最大分发渠道", "value": "酷开", "sub": "918 部 · 全表覆盖率 39.3%", "tone": "success"}, {"label": "A 级覆盖渠道", "value": "27 / 29", "sub": "多少渠道有 A 级节目分发", "tone": "success"}, {"label": "高拆条率渠道", "value": "5 个", "sub": "银河 / 华为 / 未来电视 · 拆条率 ≥20%", "tone": "warn"}], "reasons_total": [{"k": "未知（待归类）", "n": 31141, "pct": 80.0, "tone": "neutral", "desc": "数据治理债务 · 80% 未归因"}, {"k": "结构性重复", "n": 3945, "pct": 10.1, "tone": "info", "desc": "非独家+接入时序晚 · 非运营卡点"}, {"k": "无权 / 权利不符", "n": 1091, "pct": 2.8, "tone": "warn", "desc": "授权范围外 · 不应归类为运营卡点"}, {"k": "平台未选 / 业务决策", "n": 831, "pct": 2.1, "tone": "warn", "desc": "平台主动放弃"}, {"k": "无号 / 申请中", "n": 825, "pct": 2.1, "tone": "danger", "desc": "广电备案问题"}, {"k": "介质问题", "n": 657, "pct": 1.7, "tone": "danger", "desc": "介质处理 / 转码 / 标清"}, {"k": "其他卡点", "n": 447, "pct": 1.1, "tone": "danger", "desc": "题材不符 / 投诉 / 待安排"}]}, "channel_info_cards": {"浙江IPTV": {"contact": "运营：方玉琴", "category": "IPTV", "sub_categories": "电信、联通", "users": "2050 万", "current_status": "驻地方玉琴 · 三测用户约 2050 万 · 侧重电信 · 活动看奖品好坏 · 挑片看节目热度 · 浙江上片需要等排片 · 期望每周能上 2-3 部 · 目前动漫 13 部 / 少儿 20 部", "flow_status": "底量加跟播", "revenue": "1000 多万", "pain_points": "局方内存盘有限 · 上片困难", "cares_about": "好节目和大 IP · 活动看奖品好坏"}, "河北IPTV": {"contact": "驻地：牛雨菲", "category": "IPTV", "sub_categories": "三侧", "users": "1900 万", "current_status": "三测用户约 1900 万 · 电信单独运营 · 移动测收益较大 · 河北体量较大上线 225 内容 · 目前有其他渠道反馈好的节目", "flow_status": "跟播", "revenue": "1000 多万", "pain_points": "商务关系需要长期培养 · 局方对于跟 CP 的关系比较谨慎 · 崔老师负责推荐", "cares_about": "渠道的运用规则需要深入了解 · 客情关系需要长期培养"}, "重庆IPTV": {"contact": "驻地：孙玲玲", "category": "IPTV", "sub_categories": "三侧", "users": "1150 万", "current_status": "重点收入在电信侧 · 去年接入 · 少儿内容 370 部 · 五个产品包 · 移动少儿/移动教育/底量补量 · 专题的 PV 和 UV", "flow_status": "跟播", "revenue": "250 万左右", "pain_points": "目前大盘收益下滑严重 · 大盘缩减 · 内容热度不足 · 上线了超飞估计下个次结算能有好转", "cares_about": "大 IP · 在意内容好的"}, "湖南IPTV": {"contact": "郭婉颖", "category": "IPTV", "sub_categories": "/OTT", "users": "1860 万", "current_status": "三测用户约 1860 万 · 去年九月份刚接入 · 需要继续观察渠道 · 驻地是兼职 · 有很多节目在转码 · 上了 150 多部底量节目 · 驻地还没有进行上片 · 等完成审批后", "flow_status": "跟播", "revenue": "3500 万", "pain_points": "因为要跟芒果保持一致 · 导致上片排重的比较厉害", "cares_about": "在意新片"}, "辽宁IPTV": {"contact": "运营：刘禹彤", "category": "IPTV", "sub_categories": "电信、移动", "users": "980 万", "current_status": "三测用户约 980 万 · 现在电信、移动、底量加跟播 · 目前上线节目 230 部 · 局方每月瀑布流有专题推荐 · 大概每月中旬要上报争取推荐位 · 辽宁百视通家接了运营团", "flow_status": "跟播", "revenue": "200 万", "pain_points": "局方内存满了 · 上线比较慢一般需要一周时间", "cares_about": "在意好节目和大 IP · 需要给局方吸引的方案和设计图 · 由于 CP 比较多对于推荐位和活动需要主动跟老师沟通 · 谁推做谁家"}, "河南IPTV": {"contact": "夏荣佳", "category": "IPTV", "sub_categories": "电信/联通", "users": "2000 万", "current_status": "体量比较大 · 三测用户约 2000 万 · 目前驻地要兼职客服得工作", "flow_status": "跟播", "revenue": "电信一两百万 · 联通六七百万", "pain_points": "目前内存满了上片比较费劲 · 上专题比较费劲", "cares_about": "在意新片和大 IP"}, "江西IPTV": {"contact": "严琪", "category": "IPTV", "sub_categories": "电信主导", "users": "1500 万", "current_status": "三测用户约 1500 万 · 主要集中在电信 · 移动和联通加起来约 200 万用户 · 有驻地 · 去年 11 月入职之前在帕科 · 箐箐反馈是由于当地政策三册的推荐位要同步 · 电信的盘内存不", "flow_status": "跟播", "revenue": "4 千万", "pain_points": "上片量在减少", "cares_about": "在意新片和大 IP"}, "杭研": {"contact": "运营：菁菁", "category": "IPTV", "sub_categories": "江西/福建/四川 等", "users": "—", "current_status": "需要排重 · 移动办公沟通群里沟通 · 需要有转授权 · 审核流程比较麻烦 · 分省单独结算 · 沟通是没有什么问题", "flow_status": "跟播", "revenue": "渠道分散具体的要不到 · 但根据结算来看", "pain_points": "局方的审批流程比较麻烦 · 上片周期长", "cares_about": "大 IP · 在意内容好的"}, "山东IPTV": {"contact": "运营：郑旺", "category": "IPTV", "sub_categories": "电信/联通", "users": "2890 万", "current_status": "整体盘子比较大三测用户 2890 万 · 目前数据不温不火 · 也不是特别能争取好的位置 · 三侧分别结算 · 电信和联通测 · 因为银河在 · 一个月上一部节目", "flow_status": "跟播", "revenue": "4 千万", "pain_points": "推荐位一般没有", "cares_about": "大 IP · 在意内容好的"}, "湖北IPTV": {"contact": "代运营：李冰霞", "category": "IPTV", "sub_categories": "电信主导", "users": "1100 万", "current_status": "主要用户在电信侧约 900 万用户 · 移动联通侧加起约 200 万用户", "flow_status": "跟播", "revenue": "增值 3 侧加起来约 1500 万左右", "pain_points": "—", "cares_about": "—"}, "喜粤TV": {"contact": "驻地：崔文龙", "category": "IPTV", "sub_categories": "三侧", "users": "2800 万", "current_status": "三测加起来 2800 万用户 · 用户体量很大", "flow_status": "—", "revenue": "增值收入体量至少 1 个亿", "pain_points": "—", "cares_about": "—"}, "甘肃IPTV": {"contact": "—", "category": "IPTV", "sub_categories": "移动主导", "users": "780 万", "current_status": "主要用户在移动测约 780 万", "flow_status": "—", "revenue": "—", "pain_points": "—", "cares_about": "—"}, "新疆IPTV": {"contact": "—", "category": "IPTV", "sub_categories": "移动主导", "users": "1147 万", "current_status": "体量 1147 万 · 主要运营在移动测", "flow_status": "—", "revenue": "—", "pain_points": "—", "cares_about": "—"}, "广西IPTV": {"contact": "代运营", "category": "IPTV", "sub_categories": "电信/联通", "users": "1380 万", "current_status": "主要用户在电信和联通 · 合计约 1380 万用户", "flow_status": "—", "revenue": "—", "pain_points": "—", "cares_about": "—"}, "北京IPTV": {"contact": "驻地", "category": "IPTV", "sub_categories": "电信/联通", "users": "500 万", "current_status": "500 万用户 · 主要在电信及联通", "flow_status": "—", "revenue": "—", "pain_points": "—", "cares_about": "—"}}};
const COLORS = {
  info: '#185FA5', success: '#0F6E56', warn: '#BA7517', danger: '#A32D2D',
  purple: '#534AB7', neutral: '#888780',
  gradeA: '#0F6E56', gradeAm: '#1D9E75', gradeBp: '#5DCAA5', gradeB: '#9FE1CB', gradeC: '#E1F5EE',
  channels: ['#185FA5','#0F6E56','#7F77DD','#993C1D','#854F0B','#534AB7'],
  quarters: ['#0F6E56','#534AB7','#9FE1CB','#A32D2D','#BA7517']
};

let currentPage = 'cockpit';
let currentRole = 'admin';
let currentRevTab = 'channel';
const charts = {};

// =============== Page switching ===============
function switchPage(page) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.getElementById('page-' + page).classList.add('active');
  document.querySelectorAll('#page-tabs button').forEach(b => b.classList.remove('active'));
  const btn = Array.from(document.querySelectorAll('#page-tabs button')).find(b => b.getAttribute('onclick')?.includes("'" + page + "'"));
  if (btn) btn.classList.add('active');
  currentPage = page;
  setTimeout(() => renderPageCharts(page), 30);
}

function switchRole(role) {
  currentRole = role;
  document.getElementById('role-admin').classList.toggle('active', role === 'admin');
  document.getElementById('role-staff').classList.toggle('active', role === 'staff');
  document.querySelectorAll('[data-admin]').forEach(b => b.style.display = role === 'admin' ? '' : 'none');
  document.querySelectorAll('[data-staff]').forEach(b => b.style.display = role === 'staff' ? '' : 'none');
  if (role === 'staff') switchPage('staff');
  else switchPage('cockpit');
}

// =============== KPI rendering ===============
function renderKpis(containerId, kpis) {
  const c = document.getElementById(containerId);
  if (!c) return;
  c.innerHTML = kpis.map(k => `
    <div class="kpi tone-${k.tone}">
      <div class="kpi-label">${k.label}${k.pct ? `<span class="kpi-label-pct">${k.pct}</span>` : ''}</div>
      <div class="kpi-value">${k.value}</div>
      ${k.pct && k.tone !== 'success' && k.tone !== 'danger' ? `<div class="mini-bar"><div class="mini-fill" style="width:${parseFloat(k.pct)||0}%;background:var(--text-${k.tone})"></div></div>` : ''}
      <div class="kpi-sub">${k.sub}</div>
    </div>
  `).join('');
}

// =============== Chart helpers ===============
Chart.defaults.font.family = "-apple-system, BlinkMacSystemFont, 'PingFang SC', 'Microsoft YaHei', sans-serif";
Chart.defaults.font.size = 11;
Chart.defaults.color = '#5F5E5A';

function destroyChart(id) {
  if (charts[id]) { charts[id].destroy(); delete charts[id]; }
}

// =============== Cockpit ===============
function renderCockpit() {
  const d = DATA.cockpit;
  renderKpis('cockpit-kpis', d.kpis);

  // Trend chart
  destroyChart('cockpit-trend');
  charts['cockpit-trend'] = new Chart(document.getElementById('chart-cockpit-trend'), {
    type: 'bar',
    data: {
      labels: d.monthly_trend.map(m => m.month),
      datasets: [
        { type:'bar', label:'当月开票', data: d.monthly_trend.map(m => m.actual), backgroundColor: '#185FA5', borderRadius: 3 },
        { type:'line', label:'累计', data: d.monthly_trend.map(m => m.cumActual), borderColor: '#0F6E56', backgroundColor: '#0F6E56', tension: 0.3, pointRadius: 4, borderWidth: 2 },
        { type:'line', label:'目标', data: d.monthly_trend.map(m => m.target), borderColor: '#BA7517', borderDash: [4,4], pointRadius: 0, borderWidth: 1.5 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: { legend: { position: 'bottom' } },
      scales: { y: { ticks: { callback: v => v + ' 万' } } }
    }
  });

  // Gap chart
  destroyChart('cockpit-gap');
  charts['cockpit-gap'] = new Chart(document.getElementById('chart-cockpit-gap'), {
    type: 'doughnut',
    data: {
      labels: d.gap.map(g => g.cat + ' (' + g.gap + ' 万)'),
      datasets: [{ data: d.gap.map(g => g.gap), backgroundColor: ['#A32D2D','#BA7517','#888780'], borderWidth: 0 }]
    },
    options: {
      responsive: true, maintainAspectRatio: false, cutout: '60%',
      plugins: { legend: { position: 'bottom' } }
    }
  });

  // Status table
  document.getElementById('cockpit-status-table').innerHTML = `
    <thead><tr><th>状态</th><th class="tnum">渠道数</th><th>说明</th></tr></thead>
    <tbody>
      ${d.status.map(s => `
        <tr>
          <td><span class="tag tag-${s.tone === 'success' ? 'good' : s.tone === 'danger' ? 'bad' : s.tone === 'warn' ? 'warn' : 'info'}">${s.k}</span></td>
          <td class="tnum">${s.n}</td>
          <td style="color:var(--text-tertiary);font-size:11px">${s.desc}</td>
        </tr>
      `).join('')}
    </tbody>
  `;
}

// =============== Channel ===============
function renderChannel() {
  const d = DATA.channel;
  renderKpis('channel-kpis', d.kpis);

  destroyChart('channel-cat');
  charts['channel-cat'] = new Chart(document.getElementById('chart-channel-cat'), {
    type: 'bar',
    data: {
      labels: d.category.map(c => c.cat),
      datasets: [
        { label: '指标（万）', data: d.category.map(c => c.target), backgroundColor: '#B5D4F4', borderRadius: 3 },
        { label: '实际（万）', data: d.category.map(c => c.actual), backgroundColor: '#185FA5', borderRadius: 3 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: { legend: { position: 'bottom' } },
      scales: { y: { ticks: { callback: v => v + ' 万' } } }
    }
  });

  // TOP10 table
  const statusTag = s => s === '达标' ? 'good' : s === '严重' ? 'bad' : 'warn';
  document.getElementById('channel-top10-table').innerHTML = `
    <thead><tr>
      <th>排名</th><th>渠道</th><th>大类</th>
      <th class="tnum">指标</th><th class="tnum">开票</th><th class="tnum">达成</th>
      <th class="tnum">同比</th><th class="tnum">环比</th><th>状态</th>
    </tr></thead>
    <tbody>
      ${d.top10.map(t => `
        <tr class="row-clickable" onclick="openChannelDrawer('${t.ch}')">
          <td>${t.rank}</td><td><strong>${t.ch}</strong></td><td>${t.cat}</td>
          <td class="tnum">${t.target.toFixed(1)}</td>
          <td class="tnum">${t.actual.toFixed(2)}</td>
          <td class="tnum">${(t.rate*100).toFixed(1)}%</td>
          <td class="tnum" style="color:${t.yoy >= 0 ? 'var(--text-success)' : 'var(--text-danger)'}">${(t.yoy*100).toFixed(0)}%</td>
          <td class="tnum" style="color:${t.qoq >= 0 ? 'var(--text-success)' : 'var(--text-danger)'}">${(t.qoq*100).toFixed(0)}%</td>
          <td><span class="tag tag-${statusTag(t.status)}">${t.status}</span></td>
        </tr>
      `).join('')}
    </tbody>
  `;
}

// =============== Library ===============
function renderLibrary() {
  const d = DATA.library;
  renderKpis('library-kpis', d.kpis);

  destroyChart('library-pyramid');
  charts['library-pyramid'] = new Chart(document.getElementById('chart-library-pyramid'), {
    type: 'bar',
    data: {
      labels: d.pyramid.map(p => p.g),
      datasets: [{
        label: '部数',
        data: d.pyramid.map(p => p.n),
        backgroundColor: ['#0F6E56','#1D9E75','#5DCAA5','#9FE1CB','#E1F5EE','#888780'],
        borderRadius: 3
      }]
    },
    options: {
      responsive: true, maintainAspectRatio: false, indexAxis: 'y',
      plugins: { legend: { display: false } },
      scales: { x: { ticks: { callback: v => v + ' 部' } } }
    }
  });

  destroyChart('library-reasons');
  charts['library-reasons'] = new Chart(document.getElementById('chart-library-reasons'), {
    type: 'doughnut',
    data: {
      labels: d.reasons.map(r => r.k),
      datasets: [{
        data: d.reasons.map(r => r.n),
        backgroundColor: ['#888780','#BA7517','#A32D2D','#993556','#534AB7','#185FA5'],
        borderWidth: 0
      }]
    },
    options: {
      responsive: true, maintainAspectRatio: false, cutout: '50%',
      plugins: { legend: { position: 'right', labels: { font: { size: 10 } } } }
    }
  });

  // Programs table
  const gradeTag = g => {
    if (!g) return '—';
    const map = { 'A':'tag-grade-a','A-':'tag-grade-am','B+':'tag-grade-bp','B':'tag-grade-b','C':'tag-grade-c' };
    return `<span class="tag ${map[g]||'tag-grade-c'}">${g}</span>`;
  };
  document.getElementById('library-programs-table').innerHTML = `
    <thead><tr>
      <th>节目名称</th><th>等级</th><th class="tnum">分布渠道</th><th class="tnum">上线率</th>
      <th>版权商</th><th class="tnum">关联收益</th>
    </tr></thead>
    <tbody>
      ${d.sample_programs.map(p => `
        <tr class="row-clickable" onclick="openProgramDrawer('${p.name}')">
          <td><strong>${p.name}</strong> ${p.tag ? `<span class="tag tag-${p.tag === '已下线' ? 'bad' : 'warn'}" style="margin-left:4px">${p.tag}</span>` : ''}</td>
          <td>${gradeTag(p.grade)}</td>
          <td class="tnum">${p.channels} / 29</td>
          <td class="tnum" style="color:${p.rate >= 70 ? 'var(--text-success)' : p.rate >= 40 ? 'var(--text-warn)' : 'var(--text-danger)'}">${p.rate}%</td>
          <td>${p.vendor}</td>
          <td class="tnum"><a style="color:var(--text-info);cursor:pointer" onclick="event.stopPropagation();switchPage('revenue')">${p.revenue.toFixed(1)} 万 →</a></td>
        </tr>
      `).join('')}
    </tbody>
  `;
}

// =============== Revenue ===============
function renderRevenue() {
  const d = DATA.revenue;
  renderKpis('revenue-kpis-row1', d.kpis_row1);
  renderKpis('revenue-kpis-row2', d.kpis_row2);

  destroyChart('revenue-trend');
  charts['revenue-trend'] = new Chart(document.getElementById('chart-revenue-trend'), {
    type: 'bar',
    data: {
      labels: d.quarterly_breakdown.map(q => q.q),
      datasets: [
        { label: 'A 系列', data: d.quarterly_breakdown.map(q => q.A), backgroundColor: '#0F6E56', stack:'s', borderRadius: 2 },
        { label: 'B 系列', data: d.quarterly_breakdown.map(q => q.B), backgroundColor: '#534AB7', stack:'s', borderRadius: 2 },
        { label: 'C 级', data: d.quarterly_breakdown.map(q => q.C), backgroundColor: '#9FE1CB', stack:'s', borderRadius: 2 },
        { label: '已下线', data: d.quarterly_breakdown.map(q => q.down), backgroundColor: '#A32D2D', stack:'s', borderRadius: 2 },
        { label: '孤儿', data: d.quarterly_breakdown.map(q => q.orphan), backgroundColor: '#BA7517', stack:'s', borderRadius: 2 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: { legend: { display: false } },
      scales: { x: { stacked: true }, y: { stacked: true, ticks: { callback: v => v + ' 万' } } }
    }
  });

  renderRevenueMatrix();
}

function switchRevTab(tab) {
  currentRevTab = tab;
  document.getElementById('rev-tab-channel').classList.toggle('active', tab === 'channel');
  document.getElementById('rev-tab-quarter').classList.toggle('active', tab === 'quarter');
  renderRevenueMatrix();
}

function renderRevenueMatrix() {
  const d = DATA.revenue;
  const channels = ['小米','海信','酷开','TCL','康佳','华为'];
  const quarters = ['24Q1','24Q2','24Q3','24Q4','25Q1','25Q2','25Q3'];
  const labels = currentRevTab === 'channel' ? channels : quarters;
  const note = currentRevTab === 'channel'
    ? '<strong>渠道视角洞察：</strong>「萌鸡小队」三家合计 372 万 · 几乎 100% 来自小米单点依赖严重 · 「八爪鱼动画世界」「小雪和小贝的童话王国」C 级反例需重新评级'
    : '<strong>季度视角洞察：</strong>「萌鸡小队 6」24Q4 启动 0.92 万 → 25Q1 爆量 82.20 万 → S 形启动曲线 · 「小公主戴安娜」从 24Q1 36.58 万断崖下滑到 24Q4 0.22 万 · 已下线节目衰退轨迹清晰';
  
  const heatBg = v => {
    if (v === 0) return 'transparent';
    const t = Math.min(v / 80, 1);
    const alpha = 0.05 + t * 0.70;
    return `rgba(15,110,86,${alpha.toFixed(3)})`;
  };
  const fmt = v => v === 0 ? '<span class="tnum-zero">—</span>' : v.toFixed(2);
  const gradeTag = g => {
    const map = { 'A':'tag-grade-a','A-':'tag-grade-am','B+':'tag-grade-bp','B':'tag-grade-b','C':'tag-grade-c','已下线':'tag-down','孤儿':'tag-orphan' };
    return `<span class="tag ${map[g]||'tag-grade-c'}">${g}</span>`;
  };

  let html = `<thead><tr><th style="width:30px">#</th><th>节目名称</th><th>等级</th>`;
  labels.forEach(l => { html += `<th class="tnum">${l}</th>`; });
  html += `<th class="tnum">合计</th></tr></thead><tbody>`;

  d.top_programs.forEach(p => {
    html += `<tr class="row-clickable" onclick="openProgramDrawer('${p.name}')"><td>${p.rank}</td><td>${p.name}</td><td>${gradeTag(p.grade)}</td>`;
    const values = currentRevTab === 'channel' ? p.channels : p.quarters;
    values.forEach(v => {
      if (currentRevTab === 'quarter' && v > 0) {
        html += `<td class="tnum heat-cell" style="background:${heatBg(v)}">${v.toFixed(2)}</td>`;
      } else {
        html += `<td class="tnum">${fmt(v)}</td>`;
      }
    });
    html += `<td class="tnum" style="font-weight:500">${p.total.toFixed(2)}</td></tr>`;
  });
  html += '</tbody>';
  document.getElementById('revenue-matrix-table').innerHTML = html;
  document.getElementById('revenue-matrix-note').innerHTML = note;
}

// =============== Rights ===============
function renderRights() {
  const d = DATA.rights;
  renderKpis('rights-kpis', d.kpis);

  destroyChart('rights-pareto');
  charts['rights-pareto'] = new Chart(document.getElementById('chart-rights-pareto'), {
    data: {
      labels: d.top_vendors.map(v => v.vendor),
      datasets: [
        { type:'bar', label:'节目数', data: d.top_vendors.map(v => v.n), backgroundColor: '#534AB7', yAxisID: 'y', borderRadius: 3 },
        { type:'line', label:'累计占比', data: d.top_vendors.map(v => v.cum), borderColor: '#BA7517', backgroundColor: '#BA7517', yAxisID: 'y1', tension: 0.2, pointRadius: 3, borderWidth: 2 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: { legend: { position: 'bottom' } },
      scales: {
        y: { position: 'left', ticks: { callback: v => v + ' 部' } },
        y1: { position: 'right', max: 100, grid: { display: false }, ticks: { callback: v => v + '%' } }
      }
    }
  });

  destroyChart('rights-license');
  charts['rights-license'] = new Chart(document.getElementById('chart-rights-license'), {
    type: 'doughnut',
    data: {
      labels: d.license_types.map(l => l.k),
      datasets: [{ data: d.license_types.map(l => l.n), backgroundColor: ['#534AB7','#185FA5','#0F6E56','#BA7517','#888780'], borderWidth: 0 }]
    },
    options: {
      responsive: true, maintainAspectRatio: false, cutout: '55%',
      plugins: { legend: { position: 'bottom', labels: { font: { size: 10 } } } }
    }
  });

  // Expiry table
  document.getElementById('rights-expiry-table').innerHTML = `
    <thead><tr><th>到期桶</th><th class="tnum">部数</th><th class="tnum">A 级</th><th class="tnum">B 系</th><th class="tnum">C 级</th><th>紧急度</th></tr></thead>
    <tbody>
      ${d.expiry_buckets.map(b => `
        <tr><td>${b.b}</td><td class="tnum">${b.n}</td><td class="tnum">${b.A}</td><td class="tnum">${b.B}</td><td class="tnum">${b.C}</td>
          <td><span class="tag tag-${b.tone === 'danger' ? 'bad' : b.tone === 'warn' ? 'warn' : 'info'}">${b.urgency}</span></td>
        </tr>
      `).join('')}
    </tbody>
  `;
}

// =============== Data Management ===============
function renderDataMgmt() {
  const d = DATA.data_mgmt;
  renderKpis('data-kpis', d.kpis);

  // 7 tables
  const list = document.getElementById('data-tables-list');
  list.innerHTML = `
    <div class="health-row head">
      <div></div><div>数据表</div><div>记录数</div><div>完整度</div><div>最近更新</div><div>说明</div><div>操作</div>
    </div>
    ${d.tables.map(t => `
      <div class="health-row">
        <div class="health-icon health-${t.status === 'good' ? 'good' : t.status === 'warn' ? 'warn' : 'bad'}">${t.status === 'good' ? '✓' : t.status === 'warn' ? '!' : '×'}</div>
        <div><strong>${t.name}</strong><br/><span style="color:var(--text-tertiary);font-size:11px">${t.desc}</span></div>
        <div class="tnum">${t.rows.toLocaleString()}</div>
        <div>
          <div class="bar-wrap"><div class="bar-fill" style="width:${t.completeness}%;background:${t.completeness >= 90 ? 'var(--text-success)' : t.completeness >= 60 ? 'var(--text-warn)' : 'var(--text-danger)'}"></div></div>
          <div class="tnum" style="font-size:10px;margin-top:2px">${t.completeness}%</div>
        </div>
        <div>${t.updated}</div>
        <div style="font-size:11px;color:var(--text-tertiary)">${t.desc}</div>
        <div><button class="btn-sm">查看</button></div>
      </div>
    `).join('')}
  `;

  // Exceptions
  const tones = { 19: 'warn', 1758: 'bad', 3: 'info' };
  document.getElementById('data-exceptions').innerHTML = d.exceptions.map(e => `
    <div class="exception-card">
      <div class="exc-icon exc-${e.n > 1000 ? 'bad' : e.n > 10 ? 'warn' : 'info'}">${e.n}</div>
      <div>
        <div class="exc-title">${e.k}</div>
        <div class="exc-desc">${e.desc}</div>
      </div>
      <button class="btn-sm">${e.action}</button>
    </div>
  `).join('');

  // Upload log
  document.getElementById('data-uploads').innerHTML = `
    <thead><tr><th>时间</th><th>数据表</th><th class="tnum">记录数</th><th>版本</th><th>状态</th></tr></thead>
    <tbody>
      ${d.upload_log.map(u => `
        <tr>
          <td style="color:var(--text-tertiary);font-size:11px">${u.time}</td>
          <td><strong>${u.table}</strong><br/><span style="color:var(--text-tertiary);font-size:11px">${u.user}</span></td>
          <td class="tnum">${u.rows.toLocaleString()}</td>
          <td><span class="tag tag-${u.status === '已生效' ? 'good' : 'info'}">${u.version}</span></td>
          <td>${u.status}</td>
        </tr>
      `).join('')}
    </tbody>
  `;
}

// =============== Staff Workspace ===============
function renderStaff() {
  const d = DATA.staff_workspace;
  renderKpis('staff-kpis', d.kpis);

  // Todos
  document.getElementById('staff-todos').innerHTML = d.todos.map(t => `
    <div class="todo-card">
      <div class="todo-priority pri-${t.pri}"></div>
      <div>
        <div class="todo-title">${t.title}</div>
        <div class="todo-desc">${t.desc}</div>
      </div>
      <div class="todo-due">${t.pri === 'high' ? '截止' : '建议'}<br/><strong>${t.due}</strong></div>
      <button class="btn-sm" style="background:${t.pri === 'high' ? 'var(--text-info)' : 'white'};color:${t.pri === 'high' ? 'white' : 'inherit'};border-color:${t.pri === 'high' ? 'var(--text-info)' : 'var(--border-secondary)'}">${t.action}</button>
    </div>
  `).join('');

  // Quick actions
  const actions = [
    { title: '查看我的节目库', desc: '87 部节目的等级 / 上线 / 版权状态', target: 'library' },
    { title: '查看我的节目收益', desc: '本季度 / 历史趋势 / TOP 节目矩阵', target: 'revenue' },
    { title: '上传我的节目数据', desc: '片单 / 收益 / 介质 · 简化的 5 步流水线', target: 'data' },
    { title: '续约工单中心', desc: '5 部节目待续约 · 已触发 / 待跟进', target: null },
  ];
  document.getElementById('staff-quick-actions').innerHTML = actions.map(a => `
    <div style="background:white;border:0.5px solid var(--border-tertiary);border-radius:var(--radius-md);padding:12px 14px;display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;cursor:pointer" onclick="${a.target ? `switchRole('admin');switchPage('${a.target}')` : ''}">
      <div>
        <div style="font-size:13px;font-weight:500;margin-bottom:2px">${a.title}</div>
        <div style="font-size:11px;color:var(--text-tertiary)">${a.desc}</div>
      </div>
      <div style="color:var(--text-info);font-size:14px">→</div>
    </div>
  `).join('');

  // TOP 5
  const gradeTag = g => {
    const map = { 'A':'tag-grade-a','A-':'tag-grade-am','B+':'tag-grade-bp','B':'tag-grade-b','C':'tag-grade-c' };
    return `<span class="tag ${map[g]||'tag-grade-c'}">${g}</span>`;
  };
  document.getElementById('staff-top5').innerHTML = `
    <thead><tr><th>节目</th><th>等级</th><th class="tnum">本季</th><th class="tnum">环比</th></tr></thead>
    <tbody>
      ${d.top5.map(t => `
        <tr><td>${t.name}</td><td>${gradeTag(t.g)}</td><td class="tnum">${t.rev.toFixed(1)}</td>
          <td class="tnum" style="color:${t.qoq >= 0 ? 'var(--text-success)' : 'var(--text-danger)'}">${t.qoq}%</td>
        </tr>
      `).join('')}
    </tbody>
  `;
}


// =============== Channel Distribution Page ===============
function renderChannelDist() {
  const dist = DATA.channel_dist;
  const meta = DATA.channel_dist_meta;
  
  renderKpis('chdist-kpis', meta.kpis);
  
  // Render table
  renderChdistTable();
  
  // Quality structure chart - top 12 by online_n
  destroyChart('chdist-quality');
  const top12 = [...dist].sort((a,b) => b.online_n - a.online_n).slice(0, 12);
  charts['chdist-quality'] = new Chart(document.getElementById('chart-chdist-quality'), {
    type: 'bar',
    data: {
      labels: top12.map(c => c.channel),
      datasets: [
        { label: 'A', data: top12.map(c => c.grade_A), backgroundColor: '#0F6E56', stack: 's' },
        { label: 'A-', data: top12.map(c => c.grade_Am), backgroundColor: '#1D9E75', stack: 's' },
        { label: 'B+', data: top12.map(c => c.grade_Bp), backgroundColor: '#5DCAA5', stack: 's' },
        { label: 'B', data: top12.map(c => c.grade_B), backgroundColor: '#9FE1CB', stack: 's' },
        { label: 'C', data: top12.map(c => c.grade_C), backgroundColor: '#E1F5EE', stack: 's' },
        { label: '未分级', data: top12.map(c => c.grade_unrated), backgroundColor: '#888780', stack: 's' },
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false, indexAxis: 'y',
      plugins: { legend: { position: 'bottom' } },
      scales: { x: { stacked: true, ticks: { callback: v => v + ' 部' } }, y: { stacked: true } }
    }
  });
  
  // Reasons chart  
  destroyChart('chdist-reasons');
  const reasons = meta.reasons_total;
  charts['chdist-reasons'] = new Chart(document.getElementById('chart-chdist-reasons'), {
    type: 'doughnut',
    data: {
      labels: reasons.map(r => r.k),
      datasets: [{
        data: reasons.map(r => r.n),
        backgroundColor: ['#888780','#185FA5','#BA7517','#993556','#A32D2D','#534AB7','#7F77DD'],
        borderWidth: 0
      }]
    },
    options: {
      responsive: true, maintainAspectRatio: false, cutout: '50%',
      plugins: { legend: { position: 'right', labels: { font: { size: 10 } } } }
    }
  });
  
  document.getElementById('chdist-reasons-note').innerHTML = '<strong>关键发现：</strong>80% 未上线节目归因为「未知」 — 这是数据治理债务 · 真实运营卡点（无号/介质/平台未选）只占 7%';
}

function renderChdistTable() {
  const dist = DATA.channel_dist;
  const sortBy = document.getElementById('chdist-sort')?.value || 'online_n';
  const filterCat = document.getElementById('chdist-filter-cat')?.value || 'all';
  
  let filtered = filterCat === 'all' ? dist : dist.filter(c => c.category === filterCat);
  filtered = [...filtered].sort((a, b) => b[sortBy] - a[sortBy]);
  
  let html = `<thead><tr>
    <th style="width:30px">#</th>
    <th>渠道</th>
    <th>大类</th>
    <th class="tnum">已匹配</th>
    <th class="tnum">拆条</th>
    <th class="tnum">未在线</th>
    <th class="tnum">A</th>
    <th class="tnum">B 系</th>
    <th class="tnum">C</th>
    <th class="tnum">授权率</th>
    <th class="tnum">全表率</th>
    <th>数据源</th>
  </tr></thead><tbody>`;
  
  filtered.forEach((c, i) => {
    const bSeries = c.grade_Bp + c.grade_B;
    const dataComplete = c.offline_n > 0;
    const rate1Color = c.rate_auth >= 30 ? 'var(--text-success)' : c.rate_auth >= 15 ? 'var(--text-warn)' : 'var(--text-danger)';
    const rate2Color = c.rate_full >= 30 ? 'var(--text-success)' : c.rate_full >= 15 ? 'var(--text-warn)' : 'var(--text-danger)';
    const splitN = c.split_n || 0;
    const splitPct = c.split_pct || 0;
    const splitColor = splitPct >= 30 ? 'var(--text-danger)' : splitPct >= 15 ? 'var(--text-warn)' : 'var(--text-tertiary)';
    const isZLY = (c.data_source || '').includes('ZLY');
    
    html += `<tr class="row-clickable" onclick="openChannelCard('${c.channel}')">
      <td>${i+1}</td>
      <td><strong>${c.channel}</strong></td>
      <td><span class="tag ${c.category === 'OTT' ? 'tag-info' : 'tag-good'}">${c.category}</span></td>
      <td class="tnum"><strong>${c.online_n.toLocaleString()}</strong></td>
      <td class="tnum" style="color:${splitColor}">${splitN > 0 ? splitN + ` <span style="font-size:10px">(${splitPct}%)</span>` : '—'}</td>
      <td class="tnum">${dataComplete ? c.offline_n.toLocaleString() : '<span style="color:var(--text-warn)">—</span>'}</td>
      <td class="tnum" style="${c.grade_A > 0 ? 'color:var(--text-success);font-weight:500' : ''}">${c.grade_A}</td>
      <td class="tnum">${bSeries}</td>
      <td class="tnum" style="color:var(--text-tertiary)">${c.grade_C}</td>
      <td class="tnum" style="color:${rate1Color};font-weight:500">${c.rate_auth}%</td>
      <td class="tnum" style="color:${rate2Color}">${c.rate_full}%</td>
      <td><span class="tag ${isZLY ? 'tag-info' : 'tag-good'}" style="font-size:10px">${isZLY ? '4.16 新' : 'v3'}</span></td>
    </tr>`;
  });
  html += '</tbody>';
  document.getElementById('chdist-table').innerHTML = html;
  
  document.getElementById('chdist-note').innerHTML = `
    <strong>4 个值得关注的洞察：</strong><br>
    • <strong>银河 IPTV 拆条率 53.2%（746 部）</strong> — 一半都是拆条节目（系列拆集独立上线），实际匹配主表的只有 656 部<br>
    • <strong>华为拆条率 40.1%（472 部）</strong> — 大量主表 #N/A 数据，需要采购侧补登记<br>
    • <strong>3 个数据不完整渠道：</strong>芒果 OTT / 湖南 IPTV / 华数 — 仅有在线数据，未上传未在线清单<br>
    • <strong>拆条节目不计入 ABC 等级统计</strong> — 表格中 A/B/C 列只统计已匹配主表的节目
  `;
}

// =============== Channel Info Card Modal ===============
function openChannelCard(channelName) {
  const c = DATA.channel_dist.find(x => x.channel === channelName);
  if (!c) return;
  
  document.getElementById('chcard-title').textContent = channelName + ' · 渠道信息卡';
  const splitN = c.split_n || 0;
  const splitInfo = splitN > 0 ? ` · 拆条 <strong style="color:var(--text-warn)">${splitN}</strong> 部 (${c.split_pct}%)` : '';
  document.getElementById('chcard-meta').innerHTML = `
    <span class="tag ${c.category === 'OTT' ? 'tag-info' : 'tag-good'}">${c.category}</span>
    已匹配 <strong style="color:var(--text-primary)">${c.online_n.toLocaleString()}</strong> 部${splitInfo} ·
    授权率 <strong style="color:var(--text-primary)">${c.rate_auth}%</strong>
  `;
  
  const bSeries = c.grade_Bp + c.grade_B;
  const total = c.grade_A + c.grade_Am + c.grade_Bp + c.grade_B + c.grade_C + c.grade_unrated;
  const reasonRows = c.top_reasons.slice(0, 6).map(([k, n]) => {
    const isStructural = k.includes('重复') || k === '未知';
    return `<tr>
      <td>${k}</td>
      <td class="tnum">${n.toLocaleString()}</td>
      <td>${isStructural ? '<span class="tag tag-info" style="font-size:10px">非卡点</span>' : '<span class="tag tag-warn" style="font-size:10px">运营卡点</span>'}</td>
    </tr>`;
  }).join('');
  
  const vendorRows = c.top_vendors.slice(0, 5).map(([v, n]) => `
    <tr><td>${v}</td><td class="tnum">${n} 部</td></tr>
  `).join('');
  
  document.getElementById('chcard-body').innerHTML = `
    <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:16px;">
      <div style="background:var(--bg-secondary);padding:10px 12px;border-radius:var(--radius-md)">
        <div style="font-size:10px;color:var(--text-tertiary);margin-bottom:3px">在线节目数</div>
        <div style="font-size:18px;font-weight:500">${c.online_n.toLocaleString()}</div>
      </div>
      <div style="background:var(--bg-secondary);padding:10px 12px;border-radius:var(--radius-md)">
        <div style="font-size:10px;color:var(--text-tertiary);margin-bottom:3px">授权率（内部分发执行力）</div>
        <div style="font-size:18px;font-weight:500;color:var(--text-success)">${c.rate_auth}%</div>
      </div>
      <div style="background:var(--bg-secondary);padding:10px 12px;border-radius:var(--radius-md)">
        <div style="font-size:10px;color:var(--text-tertiary);margin-bottom:3px">全表率（分发广度）</div>
        <div style="font-size:18px;font-weight:500;color:var(--text-info)">${c.rate_full}%</div>
      </div>
    </div>
    
    <h3 style="font-size:12px;color:var(--text-secondary);font-weight:500;margin-bottom:8px;text-transform:uppercase;letter-spacing:0.5px">节目质量结构</h3>
    <div style="background:var(--bg-tertiary);padding:12px;border-radius:var(--radius-md);margin-bottom:14px">
      <div style="display:flex;height:24px;border-radius:4px;overflow:hidden;margin-bottom:8px">
        ${c.grade_A > 0 ? `<div style="background:#0F6E56;flex:${c.grade_A};display:flex;align-items:center;justify-content:center;color:white;font-size:10px;font-weight:500" title="A: ${c.grade_A}">${c.grade_A > total * 0.05 ? 'A' : ''}</div>` : ''}
        ${c.grade_Am > 0 ? `<div style="background:#1D9E75;flex:${c.grade_Am}" title="A-: ${c.grade_Am}"></div>` : ''}
        ${c.grade_Bp > 0 ? `<div style="background:#5DCAA5;flex:${c.grade_Bp};display:flex;align-items:center;justify-content:center;color:#04342C;font-size:10px;font-weight:500" title="B+: ${c.grade_Bp}">${c.grade_Bp > total * 0.05 ? 'B+' : ''}</div>` : ''}
        ${c.grade_B > 0 ? `<div style="background:#9FE1CB;flex:${c.grade_B};display:flex;align-items:center;justify-content:center;color:#04342C;font-size:10px;font-weight:500" title="B: ${c.grade_B}">${c.grade_B > total * 0.05 ? 'B' : ''}</div>` : ''}
        ${c.grade_C > 0 ? `<div style="background:#E1F5EE;flex:${c.grade_C};display:flex;align-items:center;justify-content:center;color:#04342C;font-size:10px;font-weight:500" title="C: ${c.grade_C}">${c.grade_C > total * 0.05 ? 'C' : ''}</div>` : ''}
        ${c.grade_unrated > 0 ? `<div style="background:#888780;flex:${c.grade_unrated}" title="未分级: ${c.grade_unrated}"></div>` : ''}
      </div>
      <div style="display:flex;gap:14px;font-size:11px;flex-wrap:wrap">
        <span>A: <strong>${c.grade_A}</strong></span>
        <span>A-: ${c.grade_Am}</span>
        <span>B+: ${c.grade_Bp}</span>
        <span>B: ${c.grade_B}</span>
        <span style="color:var(--text-tertiary)">C: ${c.grade_C}</span>
        ${c.grade_unrated > 0 ? `<span style="color:var(--text-warn)">未分级: ${c.grade_unrated}</span>` : ''}
      </div>
    </div>
    
    ${DATA.channel_info_cards[channelName] ? `
    <h3 style="font-size:12px;color:var(--text-secondary);font-weight:500;margin-bottom:8px;text-transform:uppercase;letter-spacing:0.5px">渠道资料汇总</h3>
    <div style="background:var(--bg-info);border:0.5px solid var(--border-info);border-radius:var(--radius-md);padding:12px;margin-bottom:14px;font-size:11px;line-height:1.6">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px 14px;margin-bottom:8px">
        <div><span style="color:var(--text-tertiary)">对接人：</span><strong>${DATA.channel_info_cards[channelName].contact}</strong></div>
        <div><span style="color:var(--text-tertiary)">用户规模：</span><strong>${DATA.channel_info_cards[channelName].users}</strong></div>
        <div><span style="color:var(--text-tertiary)">流水状态：</span>${DATA.channel_info_cards[channelName].flow_status}</div>
        <div><span style="color:var(--text-tertiary)">流水规模：</span>${DATA.channel_info_cards[channelName].revenue}</div>
      </div>
      <div style="border-top:0.5px solid var(--border-info);padding-top:8px;margin-top:6px">
        <div style="margin-bottom:6px"><span style="color:var(--text-tertiary)">渠道现状：</span>${DATA.channel_info_cards[channelName].current_status}</div>
        <div style="margin-bottom:6px"><span style="color:var(--text-tertiary)">运营难点：</span>${DATA.channel_info_cards[channelName].pain_points}</div>
        <div><span style="color:var(--text-tertiary)">渠道在意：</span>${DATA.channel_info_cards[channelName].cares_about}</div>
      </div>
    </div>` : `<div style="background:var(--bg-secondary);color:var(--text-tertiary);padding:10px 12px;border-radius:var(--radius-md);font-size:11px;margin-bottom:14px;font-style:italic">⚠ 该渠道资料暂未录入${c.category === 'OTT' ? '（OTT 渠道资料后补）' : ''}</div>`}
    
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:14px">
      <div>
        <h3 style="font-size:12px;color:var(--text-secondary);font-weight:500;margin-bottom:8px;text-transform:uppercase;letter-spacing:0.5px">未上线归因 TOP 6</h3>
        ${c.offline_n === 0 ? '<div style="background:var(--bg-warn);color:var(--text-warn);padding:10px 12px;border-radius:var(--radius-md);font-size:11px">⚠ 该渠道仅有在线数据，未上传未在线清单 · 数据不完整</div>' : `
        <table class="dt" style="font-size:11px">
          <thead><tr><th>归因</th><th class="tnum">部数</th><th>类型</th></tr></thead>
          <tbody>${reasonRows}</tbody>
        </table>`}
      </div>
      <div>
        <h3 style="font-size:12px;color:var(--text-secondary);font-weight:500;margin-bottom:8px;text-transform:uppercase;letter-spacing:0.5px">主力版权商 TOP 5</h3>
        <table class="dt" style="font-size:11px">
          <thead><tr><th>版权商</th><th class="tnum">节目数</th></tr></thead>
          <tbody>${vendorRows || '<tr><td colspan="2" style="color:var(--text-tertiary)">暂无数据</td></tr>'}</tbody>
        </table>
      </div>
    </div>
    
    <div style="display:flex;gap:8px;padding-top:14px;border-top:0.5px solid var(--border-tertiary)">
      <button class="btn-sm" onclick="closeChannelCard();switchPage('channel')">查看渠道经营详情</button>
      <button class="btn-sm">导出该渠道节目清单</button>
      <button class="btn-sm" style="margin-left:auto" onclick="closeChannelCard()">关闭</button>
    </div>
  `;
  
  document.getElementById('chcard-overlay').classList.add('show');
  document.getElementById('chcard-modal').style.display = 'block';
  document.body.style.overflow = 'hidden';
}

function closeChannelCard() {
  document.getElementById('chcard-overlay').classList.remove('show');
  document.getElementById('chcard-modal').style.display = 'none';
  document.body.style.overflow = '';
}

// Wire up filter changes
document.addEventListener('DOMContentLoaded', () => {
  setTimeout(() => {
    const sortEl = document.getElementById('chdist-sort');
    const catEl = document.getElementById('chdist-filter-cat');
    if (sortEl) sortEl.addEventListener('change', renderChdistTable);
    if (catEl) catEl.addEventListener('change', renderChdistTable);
  }, 200);
});

// =============== Drawers ===============
function openChannelDrawer(channelName) {
  // Find channel data
  const ch = DATA.channel.top10.find(c => c.ch === channelName);
  if (!ch) return;
  document.getElementById('drawer-title').textContent = ch.ch + ' · 渠道详情';
  document.getElementById('drawer-meta').innerHTML = `<span class="tag tag-info">${ch.cat}</span> <span style="color:var(--text-tertiary)">账期：季度 · 入网 2022-08</span>`;
  document.getElementById('drawer-body').innerHTML = `
    <div class="drawer-section">
      <div class="drawer-mini-kpis">
        <div class="drawer-mini-kpi"><div class="lbl">Q1 指标</div><div class="val">${ch.target.toFixed(1)} 万</div></div>
        <div class="drawer-mini-kpi"><div class="lbl">Q1 开票</div><div class="val">${ch.actual.toFixed(2)} 万</div></div>
        <div class="drawer-mini-kpi"><div class="lbl">达成率</div><div class="val" style="color:${ch.rate >= 1 ? 'var(--text-success)' : ch.rate >= 0.6 ? 'var(--text-warn)' : 'var(--text-danger)'}">${(ch.rate*100).toFixed(1)}%</div></div>
        <div class="drawer-mini-kpi"><div class="lbl">同比 / 环比</div><div class="val" style="font-size:14px">${(ch.yoy*100).toFixed(0)}% / ${(ch.qoq*100).toFixed(0)}%</div></div>
      </div>
    </div>
    <div class="drawer-section">
      <h3>渠道现状</h3>
      <div class="drawer-narrative">用户规模 380 万 · 本季度 ARPU 4.6 元 · 节目库 1,548 部 · 5 状态：${ch.rate >= 1 ? '达标' : ch.rate >= 0.6 ? '预警' : '严重'}</div>
    </div>
    <div class="drawer-section">
      <h3>运营难点</h3>
      <div class="drawer-narrative">当地用户偏好竞品内容 · 我方 IP 推广通路较窄 · 需对接当地小学数学 K12 联运</div>
    </div>
    <div class="drawer-section">
      <h3>渠道在意</h3>
      <div class="drawer-narrative">续约 + 排播节奏 · 月度结算流程 · 介质质量 (要求 1080P+)</div>
    </div>
    <div class="drawer-section">
      <h3>联系人（驻地 + 局方 + 支撑方）</h3>
      <dl class="drawer-meta-row"><dt>驻地经理</dt><dd>李芳 · 渠道经理</dd></dl>
      <dl class="drawer-meta-row"><dt>局方</dt><dd>王某 · 内容部主管</dd></dl>
      <dl class="drawer-meta-row"><dt>支撑方</dt><dd>赵某 · 介质对接</dd></dl>
    </div>
    <div class="drawer-section">
      <h3>CP 情况 / 活动情况</h3>
      <div class="drawer-narrative"><strong>CP 合作：</strong>2026 Q1 新签陕西聚量 / 北京爱升各 1 个 IP 包 (+15% 节目供给)</div>
      <div class="drawer-narrative"><strong>近期活动：</strong>6 月儿童节联动 · 计划上线"超级飞侠 18"主题页 (+30 万收入预期)</div>
      <div class="drawer-narrative" style="background:var(--bg-warn);color:var(--text-warn)"><strong>风险点：</strong>7 月 1 部 A 级节目"超级飞侠 1"版权到期 · 需续约或下线</div>
    </div>
  `;
  document.getElementById('drawer-actions').innerHTML = `
    <button class="btn-sm">历史季度</button>
    <button class="btn-sm">节目表现</button>
    <button class="btn-sm">完整资料</button>
    <button class="btn-sm" style="margin-left:auto">编辑</button>
  `;
  showDrawer();
}

function openProgramDrawer(progName) {
  document.getElementById('drawer-title').textContent = progName + ' · 节目详情';
  const isExpiring = progName.includes('超级飞侠 1') && !progName.includes('1 ') && !progName.includes('17') && !progName.includes('16') && !progName.includes('14') && !progName.includes('15') && !progName.includes('18');
  // Simplify for demo
  document.getElementById('drawer-meta').innerHTML = `<span class="tag tag-grade-a">A 级</span> <span style="color:var(--text-tertiary)">2024 出品 · 26 集</span>`;
  
  let warningHtml = '';
  if (progName.includes('戴安娜') || progName.includes('搞怪兄弟') || progName.includes('熊出没')) {
    warningHtml = '<div class="warning-box">⚠ 该节目已下线 · 版权到期或合约结束 · 部分平台仍在产生延续期分账</div>';
  } else if (progName.includes('超级飞侠 1') && progName.length < 8) {
    warningHtml = '<div class="warning-box">⚠ 版权倒计时 26 天后到期（2026-07-29）· 立即与广州奥飞启动续约谈判</div>';
  }
  
  document.getElementById('drawer-body').innerHTML = `
    ${warningHtml}
    <div class="drawer-section">
      <div class="drawer-mini-kpis">
        <div class="drawer-mini-kpi"><div class="lbl">分布渠道</div><div class="val">11 / 29</div></div>
        <div class="drawer-mini-kpi"><div class="lbl">已上线</div><div class="val">3</div></div>
        <div class="drawer-mini-kpi"><div class="lbl">未在线</div><div class="val">8</div></div>
        <div class="drawer-mini-kpi"><div class="lbl">出品 / 集数</div><div class="val" style="font-size:13px">2024 · 26 集</div></div>
      </div>
    </div>
    <div class="drawer-section">
      <h3>节目元数据</h3>
      <dl class="drawer-meta-row"><dt>类型</dt><dd>冒险、认知</dd></dl>
      <dl class="drawer-meta-row"><dt>国别 / 语言</dt><dd>中国 · 国语</dd></dl>
      <dl class="drawer-meta-row"><dt>版权商</dt><dd>广州奥飞动漫</dd></dl>
      <dl class="drawer-meta-row"><dt>采购类型</dt><dd>采购</dd></dl>
    </div>
    <div class="drawer-section">
      <h3>版权信息（关键风险）</h3>
      <dl class="drawer-meta-row"><dt>授权方式</dt><dd>非独不含转授 · 指定平台</dd></dl>
      <dl class="drawer-meta-row"><dt>授权周期</dt><dd>2020-07-30 — 2026-07-29</dd></dl>
      <dl class="drawer-meta-row"><dt>授权范围</dt><dd>小米 / 华为 / 未来电视 / 上海 IPTV / 河南 IPTV / 重庆有线（6 平台）</dd></dl>
    </div>
    <div class="drawer-section">
      <h3>29 渠道状态</h3>
      <div class="drawer-narrative" style="background:var(--bg-success);color:var(--text-success)"><strong>已上线 (3)：</strong>小米 / 华为 / 未来电视</div>
      <div class="drawer-narrative" style="background:var(--bg-warn);color:var(--text-warn)"><strong>未在线 (8)：</strong>4 个无权 + 4 个待澄清</div>
      <div class="drawer-narrative" style="background:var(--bg-info);color:var(--text-info)"><strong>授权范围外 (18)：</strong>本节目仅授权 6 平台</div>
    </div>
    <div class="drawer-section">
      <h3>关键洞察 / 行动建议</h3>
      <div class="drawer-narrative" style="background:var(--bg-danger);color:var(--text-danger)"><strong>🔴 高优先级：</strong>立即与广州奥飞启动续约谈判 · 版权 26 天后到期</div>
      <div class="drawer-narrative" style="background:var(--bg-warn);color:var(--text-warn)"><strong>🟡 中优先级：</strong>推进剩余 3 平台上线（上海 / 河南 IPTV / 重庆）· 已授权但未上线 · 3/6 上线达成 50%</div>
      <div class="drawer-narrative" style="background:var(--bg-success);color:var(--text-success)"><strong>🟢 低优先级：</strong>续约时考虑升级为"全网独家"扩大可上线渠道</div>
    </div>
  `;
  document.getElementById('drawer-actions').innerHTML = `
    <button class="btn-sm">查看元数据</button>
    <button class="btn-sm">渠道日志</button>
    <button class="btn-sm" style="background:var(--text-danger);color:white;border-color:var(--text-danger)">触发续约工单</button>
    <button class="btn-sm" style="margin-left:auto">编辑</button>
  `;
  showDrawer();
}

function showDrawer() {
  document.getElementById('drawer-overlay').classList.add('show');
  document.getElementById('drawer').classList.add('show');
  document.body.style.overflow = 'hidden';
}
function closeDrawer() {
  document.getElementById('drawer-overlay').classList.remove('show');
  document.getElementById('drawer').classList.remove('show');
  document.body.style.overflow = '';
}

// =============== Page render dispatcher ===============
function renderPageCharts(page) {
  if (page === 'cockpit') renderCockpit();
  else if (page === 'channel') renderChannel();
  else if (page === 'library') renderLibrary();
  else if (page === 'revenue') renderRevenue();
  else if (page === 'rights') renderRights();
  else if (page === 'data') renderDataMgmt();
  else if (page === 'staff') renderStaff();
  else if (page === 'chdist') renderChannelDist();
}

// Initial render
window.addEventListener('load', () => {
  setTimeout(() => renderPageCharts('cockpit'), 100);
});
</script>

</body>
</html>
