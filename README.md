# 我的-
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#1E88E5">
<title>群索608 仓库管理</title>
<style>
* { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }
body {
  font-family: -apple-system, "Microsoft YaHei", sans-serif;
  background:#f0f2f5;
  font-size: 14px;
  padding-bottom: 60px;
  user-select:none;
}
.header {
  background: linear-gradient(135deg,#1E88E5,#1565C0);
  color:#fff; padding:12px 15px;
  position:sticky; top:0; z-index:100;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
.header h1 { font-size:18px; }
.header .sub { font-size:12px; opacity:0.9; margin-top:2px; }

.tabs {
  display:flex; background:#fff;
  position:sticky; top:60px; z-index:99;
  border-bottom:1px solid #eee;
}
.tabs .tab {
  flex:1; padding:12px 0; text-align:center;
  color:#666; font-size:13px;
  border-bottom:2px solid transparent;
}
.tabs .tab.active {
  color:#1E88E5; border-bottom-color:#1E88E5;
  font-weight:bold;
}

.page { display:none; padding:10px; }
.page.active { display:block; }

.card {
  background:#fff; border-radius:8px;
  padding:12px; margin-bottom:10px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.08);
}

.row { display:flex; gap:8px; margin-bottom:8px; }
.row > * { flex:1; }

input, select, textarea {
  width:100%; padding:10px;
  border:1px solid #ddd; border-radius:6px;
  font-size:14px; background:#fff;
}
input:focus, select:focus { border-color:#1E88E5; outline:none; }

.btn {
  padding:12px; border:none; border-radius:6px;
  font-size:15px; font-weight:bold;
  background:#1E88E5; color:#fff;
  cursor:pointer; width:100%;
}
.btn:active { opacity:0.8; }
.btn-green { background:#43A047; }
.btn-orange { background:#FB8C00; }
.btn-red { background:#E53935; }
.btn-gray { background:#757575; }
.btn-small { padding:6px 12px; font-size:12px; width:auto; }

.label { font-size:12px; color:#666; margin-bottom:4px; display:block; }

.list-item {
  background:#fff; padding:10px;
  border-bottom:1px solid #f0f0f0;
  display:flex; justify-content:space-between;
  align-items:center;
}
.list-item .info { flex:1; }
.list-item .name { font-weight:bold; }
.list-item .meta { font-size:12px; color:#888; margin-top:2px; }
.list-item .qty { font-size:18px; font-weight:bold; color:#1E88E5; }
.list-item .actions { display:flex; gap:5px; }

.scan-box {
  background:#fff; padding:15px; border-radius:8px;
  text-align:center; margin-bottom:10px;
}
.scan-box input {
  font-size:18px; text-align:center;
  font-weight:bold; letter-spacing:1px;
}

video, canvas { width:100%; border-radius:6px; }

.total-bar {
  position:fixed; bottom:0; left:0; right:0;
  background:#fff; padding:10px 15px;
  border-top:2px solid #1E88E5;
  display:flex; justify-content:space-between;
  align-items:center; z-index:90;
}
.total-bar .amount { font-size:18px; color:#E53935; font-weight:bold; }

.toast {
  position:fixed; top:50%; left:50%;
  transform:translate(-50%,-50%);
  background:rgba(0,0,0,0.8); color:#fff;
  padding:12px 20px; border-radius:6px;
  z-index:1000; display:none;
}

.modal-mask {
  position:fixed; inset:0;
  background:rgba(0,0,0,0.5); z-index:200;
  display:none; align-items:center;
  justify-content:center; padding:20px;
}
.modal {
  background:#fff; border-radius:8px;
  padding:15px; width:100%; max-width:400px;
  max-height:80vh; overflow-y:auto;
}
.modal h3 { margin-bottom:10px; color:#1E88E5; }

.tag {
  display:inline-block; padding:2px 6px;
  border-radius:4px; font-size:11px;
  background:#E3F2FD; color:#1976D2; margin-right:4px;
}
.tag-in { background:#E8F5E9; color:#43A047; }
.tag-out { background:#FFF3E0; color:#FB8C00; }

.empty {
  text-align:center; padding:40px 20px;
  color:#999; font-size:13px;
}

table { width:100%; border-collapse:collapse; font-size:12px; }
th, td { padding:6px; text-align:left; border-bottom:1px solid #eee; }
th { background:#f5f5f5; }

.stat-grid { display:grid; grid-template-columns:1fr 1fr; gap:8px; }
.stat-card {
  background:#fff; padding:15px;
  border-radius:8px; text-align:center;
}
.stat-card .num { font-size:24px; font-weight:bold; color:#1E88E5; }
.stat-card .label2 { font-size:12px; color:#666; margin-top:4px; }

.divider { height:1px; background:#eee; margin:10px 0; }

@media (max-width:400px) {
  body { font-size:13px; }
  input, select { padding:8px; }
}
</style>
</head>
<body>

<div class="header">
  <h1>📦 群索608 仓库管理</h1>
  <div class="sub" id="deviceInfo">设备: - | 操作员: 管理员</div>
</div>

<div class="tabs">
  <div class="tab active" data-page="inStock">入库</div>
  <div class="tab" data-page="outStock">出库</div>
  <div class="tab" data-page="stock">库存</div>
  <div class="tab" data-page="goods">商品</div>
  <div class="tab" data-page="bills">单据</div>
</div>

<!-- 入库页面 -->
<div class="page active" id="page-inStock">
  <div class="card">
    <div class="row">
      <div><span class="label">入库类型</span>
        <select id="inType">
          <option>采购入库</option><option>退货入库</option>
          <option>调拨入库</option><option>其他入库</option>
        </select>
      </div>
      <div><span class="label">仓库</span>
        <select id="inWH"><option>主仓库</option><option>分仓库</option></select>
      </div>
    </div>
    <span class="label">供应商(选填)</span>
    <input type="text" id="inSupplier" placeholder="供应商编码或名称">
  </div>

  <div class="card">
    <div class="scan-box">
      <span class="label">📷 扫描商品条码</span>
      <div class="row">
        <input type="text" id="inBarcode" placeholder="扫描或输入条码" autofocus>
        <button class="btn btn-small" onclick="openCamera('in')">📷</button>
      </div>
    </div>
    <div id="inGoodsInfo" style="display:none;">
      <div class="row">
        <div><span class="label">编码</span>
          <input type="text" id="inGoodsId" readonly></div>
        <div><span class="label">名称</span>
          <input type="text" id="inGoodsName" readonly></div>
      </div>
      <div class="row">
        <div><span class="label">规格</span>
          <input type="text" id="inSpec" readonly></div>
        <div><span class="label">单位</span>
          <input type="text" id="inUnit" readonly></div>
      </div>
      <div class="row">
        <div><span class="label">数量</span>
          <input type="number" id="inQty" placeholder="0" step="0.01"></div>
        <div><span class="label">单价</span>
          <input type="number" id="inPrice" placeholder="0.00" step="0.01"></div>
        <div><span class="label">批次</span>
          <input type="text" id="inBatch" placeholder="选填"></div>
      </div>
      <button class="btn btn-green" onclick="addInDetail()">➕ 添加明细</button>
    </div>
  </div>

  <div class="card">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
      <strong>入库明细 (<span id="inCount">0</span>)</strong>
      <button class="btn btn-small btn-red" onclick="clearInDetail()">清空</button>
    </div>
    <div id="inDetailList"></div>
  </div>

  <div class="total-bar">
    <div class="amount">合计:¥<span id="inTotal">0.00</span></div>
    <button class="btn" style="width:auto;padding:10px 20px;" onclick="saveInStock()">保存入库单</button>
  </div>
</div>

<!-- 出库页面 -->
<div class="page" id="page-outStock">
  <div class="card">
    <div class="row">
      <div><span class="label">出库类型</span>
        <select id="outType">
          <option>销售出库</option><option>领用出库</option>
          <option>调拨出库</option><option>报损出库</option>
        </select>
      </div>
      <div><span class="label">仓库</span>
        <select id="outWH"><option>主仓库</option><option>分仓库</option></select>
      </div>
    </div>
    <span class="label">领用部门/客户(选填)</span>
    <input type="text" id="outCustomer" placeholder="客户编码或部门">
  </div>

  <div class="card">
    <div class="scan-box">
      <span class="label">📷 扫描商品条码</span>
      <div class="row">
        <input type="text" id="outBarcode" placeholder="扫描或输入条码">
        <button class="btn btn-small" onclick="openCamera('out')">📷</button>
      </div>
    </div>
    <div id="outGoodsInfo" style="display:none;">
      <div class="row">
        <div><span class="label">编码</span>
          <input type="text" id="outGoodsId" readonly></div>
        <div><span class="label">名称</span>
          <input type="text" id="outGoodsName" readonly></div>
      </div>
      <div class="row">
        <div><span class="label">规格</span>
          <input type="text" id="outSpec" readonly></div>
        <div><span class="label">单位</span>
          <input type="text" id="outUnit" readonly></div>
        <div><span class="label">库存</span>
          <input type="text" id="outStock" readonly style="color:#1E88E5;font-weight:bold;"></div>
      </div>
      <div class="row">
        <div><span class="label">数量</span>
          <input type="number" id="outQty" placeholder="0" step="0.01"></div>
        <div><span class="label">单价</span>
          <input type="number" id="outPrice" placeholder="0.00" step="0.01"></div>
        <div><span class="label">批次</span>
          <input type="text" id="outBatch" placeholder="选填"></div>
      </div>
      <button class="btn btn-orange" onclick="addOutDetail()">➕ 添加明细</button>
    </div>
  </div>

  <div class="card">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
      <strong>出库明细 (<span id="outCount">0</span>)</strong>
      <button class="btn btn-small btn-red" onclick="clearOutDetail()">清空</button>
    </div>
    <div id="outDetailList"></div>
  </div>

  <div class="total-bar">
    <div class="amount">合计:¥<span id="outTotal">0.00</span></div>
    <button class="btn btn-orange" style="width:auto;padding:10px 20px;" onclick="saveOutStock()">保存出库单</button>
  </div>
</div>

<!-- 库存页面 -->
<div class="page" id="page-stock">
  <div class="stat-grid" style="margin-bottom:10px;">
    <div class="stat-card">
      <div class="num" id="statGoods">0</div>
      <div class="label2">商品种类</div>
    </div>
    <div class="stat-card">
      <div class="num" id="statQty">0</div>
      <div class="label2">库存总件数</div>
    </div>
  </div>
  <div class="card">
    <div class="row">
      <input type="text" id="stockSearch" placeholder="搜索商品编码/名称">
      <button class="btn" style="width:auto;padding:10px 20px;" onclick="loadStock()">查询</button>
    </div>
  </div>
  <div class="card">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px;">
      <strong>库存列表</strong>
      <div>
        <button class="btn btn-small btn-green" onclick="exportData()">� 导出</button>
        <button class="btn btn-small" onclick="document.getElementById('importFile').click()">📤 导入</button>
        <input type="file" id="importFile" accept=".json" style="display:none" onchange="importData(event)">
      </div>
    </div>
    <div id="stockList"></div>
  </div>
</div>

<!-- 商品页面 -->
<div class="page" id="page-goods">
  <div class="card">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;">
      <strong>商品资料</strong>
      <button class="btn btn-small btn-green" onclick="showGoodsEdit()">➕ 新增商品</button>
    </div>
    <input type="text" id="goodsSearch" placeholder="搜索商品" oninput="loadGoods()">
    <div id="goodsList" style="margin-top:10px;"></div>
  </div>
</div>

<!-- 单据页面 -->
<div class="page" id="page-bills">
  <div class="card">
    <div class="tabs" style="position:static;border:none;">
      <div class="tab active" data-billType="all" onclick="switchBillTab(this)">全部</div>
      <div class="tab" data-billType="in" onclick="switchBillTab(this)">入库单</div>
      <div class="tab" data-billType="out" onclick="switchBillTab(this)">出库单</div>
    </div>
    <div id="billsList" style="margin-top:10px;"></div>
  </div>
</div>

<!-- 商品编辑弹窗 -->
<div class="modal-mask" id="goodsEditModal">
  <div class="modal">
    <h3 id="goodsEditTitle">新增商品</h3>
    <input type="hidden" id="editGoodsId">
    <span class="label">商品编码 *</span>
    <input type="text" id="editGoodsCode" placeholder="如 G001">
    <span class="label" style="margin-top:8px;display:block;">条形码</span>
    <input type="text" id="editBarcode" placeholder="扫描或输入">
    <span class="label" style="margin-top:8px;display:block;">商品名称 *</span>
    <input type="text" id="editName" placeholder="商品名称">
    <div class="row" style="margin-top:8px;">
      <div><span class="label">规格</span>
        <input type="text" id="editSpec"></div>
      <div><span class="label">单位</span>
        <input type="text" id="editUnit"></div>
    </div>
    <div class="row">
      <div><span class="label">采购价</span>
        <input type="number" id="editPrice" step="0.01"></div>
      <div><span class="label">销售价</span>
        <input type="number" id="editSale" step="0.01"></div>
    </div>
    <div class="row" style="margin-top:15px;">
      <button class="btn btn-gray" onclick="closeGoodsEdit()">取消</button>
      <button class="btn btn-green" onclick="saveGoods()">保存</button>
    </div>
  </div>
</div>

<!-- 扫描弹窗 -->
<div class="modal-mask" id="scanModal">
  <div class="modal">
    <h3>📷 扫描条码</h3>
    <video id="scanVideo" autoplay playsinline style="max-height:300px;"></video>
    <p style="text-align:center;color:#666;font-size:12px;margin:10px 0;">
      将条码对准扫描框<br>(支持一维码、二维码)
    </p>
    <button class="btn btn-red" onclick="closeCamera()">关闭扫描</button>
  </div>
</div>

<!-- 单据详情弹窗 -->
<div class="modal-mask" id="billDetailModal">
  <div class="modal">
    <h3 id="billDetailTitle">单据详情</h3>
    <div id="billDetailContent"></div>
    <button class="btn btn-gray" style="margin-top:10px;" onclick="document.getElementById('billDetailModal').style.display='none'">关闭</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
/* ============ 数据存储 ============ */
const DB = {
  get(key) { try { return JSON.parse(localStorage.getItem('wms_' + key)) || []; } catch { return []; } },
  set(key, val) { localStorage.setItem('wms_' + key, JSON.stringify(val)); }
};

// 初始化示例数据
(function initData(){
  if (DB.get('goods').length === 0) {
    const sample = [
      {id:'G001', barcode:'6901234567890', name:'可口可乐', spec:'500ml', unit:'瓶', price:3.00, sale:4.00},
      {id:'G002', barcode:'6901234567891', name:'百事可乐', spec:'500ml', unit:'瓶', price:3.00, sale:4.00},
      {id:'G003', barcode:'6901234567892', name:'雪碧', spec:'500ml', unit:'瓶', price:3.00, sale:4.00},
      {id:'G004', barcode:'6901234567893', name:'农夫山泉', spec:'550ml', unit:'瓶', price:2.00, sale:3.00},
      {id:'G005', barcode:'6901234567894', name:'康师傅红烧牛肉面', spec:'桶装', unit:'桶', price:4.50, sale:6.00}
    ];
    DB.set('goods', sample);
  }
})();

let inDetails = [], outDetails = [], currentScanTarget = '', scanStream = null;

/* ============ 通用 ============ */
function toast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.style.display = 'block';
  setTimeout(() => t.style.display = 'none', 1500);
}

function genNo(prefix) {
  const d = new Date();
  const ds = d.getFullYear() +
    String(d.getMonth()+1).padStart(2,'0') +
    String(d.getDate()).padStart(2,'0');
  const list = DB.get(prefix === 'RK' ? 'inBills' : 'outBills');
  const seq = list.filter(b => b.no.startsWith(prefix + ds)).length + 1;
  return prefix + ds + String(seq).padStart(4,'0');
}

document.getElementById('deviceInfo').textContent = 
  '设备: ' + (navigator.userAgent.match(/Android[\/\s]\d+/) || ['Web']) + ' | 操作员: 管理员';

/* ============ Tab 切换 ============ */
document.querySelectorAll('.tab').forEach(t => {
  if (!t.dataset.billType) {
    t.addEventListener('click', () => {
      document.querySelectorAll('.tab').forEach(x => x.classList.remove('active'));
      t.classList.add('active');
      const page = t.dataset.page;
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.getElementById('page-' + page).classList.add('active');
      if (page === 'stock') loadStock();
      if (page === 'goods') loadGoods();
      if (page === 'bills') loadBills();
    });
  }
});

/* ============ 入库扫码 ============ */
document.getElementById('inBarcode').addEventListener('keypress', e => {
  if (e.key === 'Enter') lookupGoods('in', e.target.value.trim());
});
document.getElementById('outBarcode').addEventListener('keypress', e => {
  if (e.key === 'Enter') lookupGoods('out', e.target.value.trim());
});

function lookupGoods(type, code) {
  if (!code) return;
  const goods = DB.get('goods').find(g => g.barcode === code || g.id === code);
  if (!goods) {
    toast('❌ 未找到该条码商品');
    return;
  }
  document.getElementById(type + 'GoodsId').value = goods.id;
  document.getElementById(type + 'GoodsName').value = goods.name;
  document.getElementById(type + 'Spec').value = goods.spec || '';
  document.getElementById(type + 'Unit').value = goods.unit || '';
  document.getElementById(type + 'Price').value = goods.price || 0;
  document.getElementById(type + 'GoodsInfo').style.display = 'block';
  if (type === 'out') {
    const stock = getStock(goods.id, document.getElementById('outWH').value);
    document.getElementById('outStock').value = stock;
  }
  setTimeout(() => document.getElementById(type + 'Qty').focus(), 100);
}

/* ============ 添加入库明细 ============ */
function addInDetail() {
  const goodsId = document.getElementById('inGoodsId').value;
  const qty = parseFloat(document.getElementById('inQty').value);
  const price = parseFloat(document.getElementById('inPrice').value) || 0;
  if (!goodsId || !qty) { toast('请先扫描商品并输入数量'); return; }

  const goods = DB.get('goods').find(g => g.id === goodsId);
  inDetails.push({
    goodsId, name: goods.name, spec: goods.spec, unit: goods.unit,
    qty, price, total: qty * price,
    batch: document.getElementById('inBatch').value
  });
  renderInDetail();
  clearInInput();
  toast('✅ 已添加');
}

function renderInDetail() {
  let html = '';
  let total = 0;
  inDetails.forEach((d, i) => {
    total += d.total;
    html += `<div class="list-item">
      <div class="info">
        <div class="name">${d.name}</div>
        <div class="meta">${d.goodsId} | ${d.spec} | ${d.unit}</div>
        <div class="meta">${d.qty} × ¥${d.price.toFixed(2)} = ¥${d.total.toFixed(2)}</div>
        ${d.batch ? '<span class="tag">批次:'+d.batch+'</span>' : ''}
      </div>
      <button class="btn btn-small btn-red" onclick="delInDetail(${i})">删</button>
    </div>`;
  });
  document.getElementById('inDetailList').innerHTML = html || '<div class="empty">暂无明细,请扫描添加</div>';
  document.getElementById('inCount').textContent = inDetails.length;
  document.getElementById('inTotal').textContent = total.toFixed(2);
}

function delInDetail(i) { inDetails.splice(i, 1); renderInDetail(); }
function clearInDetail() { inDetails = []; renderInDetail(); }
function clearInInput() {
  ['inBarcode','inGoodsId','inGoodsName','inSpec','inUnit','inQty','inPrice','inBatch']
    .forEach(id => document.getElementById(id).value = '');
  document.getElementById('inGoodsInfo').style.display = 'none';
  document.getElementById('inBarcode').focus();
}

/* ============ 保存入库单 ============ */
function saveInStock() {
  if (inDetails.length === 0) { toast('请先添加明细'); return; }
  const wh = document.getElementById('inWH').value;
  const bill = {
    no: genNo('RK'),
    type: document.getElementById('inType').value,
    supplier: document.getElementById('inSupplier').value,
    warehouse: wh,
    date: new Date().toLocaleString('zh-CN'),
    operator: '管理员',
    details: [...inDetails],
    total: inDetails.reduce((s, d) => s + d.total, 0)
  };
  
  // 更新库存
  const stock = DB.get('stock');
  inDetails.forEach(d => {
    const key = d.goodsId + '_' + wh;
    const exist = stock.find(s => s.key === key);
    if (exist) exist.qty += d.qty;
    else stock.push({ key, goodsId: d.goodsId, warehouse: wh, qty: d.qty });
  });
  DB.set('stock', stock);
  
  // 保存单据
  const bills = DB.get('inBills');
  bills.unshift(bill);
  DB.set('inBills', bills);
  
  toast('✅ 入库单已保存: ' + bill.no);
  inDetails = [];
  renderInDetail();
  document.getElementById('inSupplier').value = '';
}

/* ============ 出库 ============ */
document.getElementById('outWH').addEventListener('change', () => {
  const gid = document.getElementById('outGoodsId').value;
  if (gid) {
    document.getElementById('outStock').value = getStock(gid, document.getElementById('outWH').value);
  }
});

function addOutDetail() {
  const goodsId = document.getElementById('outGoodsId').value;
  const qty = parseFloat(document.getElementById('outQty').value);
  const price = parseFloat(document.getElementById('outPrice').value) || 0;
  if (!goodsId || !qty) { toast('请先扫描商品并输入数量'); return; }
  
  const wh = document.getElementById('outWH').value;
  const stock = getStock(goodsId, wh);
  if (stock < qty) { toast('❌ 库存不足!当前库存:' + stock); return; }

  const goods = DB.get('goods').find(g => g.id === goodsId);
  outDetails.push({
    goodsId, name: goods.name, spec: goods.spec, unit: goods.unit,
    qty, price, total: qty * price,
    batch: document.getElementById('outBatch').value
  });
  renderOutDetail();
  clearOutInput();
  toast('✅ 已添加');
}

function renderOutDetail() {
  let html = '';
  let total = 0;
  outDetails.forEach((d, i) => {
    total += d.total;
    html += `<div class="list-item">
      <div class="info">
        <div class="name">${d.name}</div>
        <div class="meta">${d.goodsId} | ${d.spec} | ${d.unit}</div>
        <div class="meta">${d.qty} × ¥${d.price.toFixed(2)} = ¥${d.total.toFixed(2)}</div>
      </div>
      <button class="btn btn-small btn-red" onclick="delOutDetail(${i})">删</button>
    </div>`;
  });
  document.getElementById('outDetailList').innerHTML = html || '<div class="empty">暂无明细</div>';
  document.getElementById('outCount').textContent = outDetails.length;
  document.getElementById('outTotal').textContent = total.toFixed(2);
}

function delOutDetail(i) { outDetails.splice(i, 1); renderOutDetail(); }
function clearOutDetail() { outDetails = []; renderOutDetail(); }
function clearOutInput() {
  ['outBarcode','outGoodsId','outGoodsName','outSpec','outUnit','outStock','outQty','outPrice','outBatch']
    .forEach(id => document.getElementById(id).value = '');
  document.getElementById('outGoodsInfo').style.display = 'none';
  document.getElementById('outBarcode').focus();
}

function saveOutStock() {
  if (outDetails.length === 0) { toast('请先添加明细'); return; }
  const wh = document.getElementById('outWH').value;
  const bill = {
    no: genNo('CK'),
    type: document.getElementById('outType').value,
    customer: document.getElementById('outCustomer').value,
    warehouse: wh,
    date: new Date().toLocaleString('zh-CN'),
    operator: '管理员',
    details: [...outDetails],
    total: outDetails.reduce((s, d) => s + d.total, 0)
  };
  
  // 扣减库存
  const stock = DB.get('stock');
  outDetails.forEach(d => {
    const key = d.goodsId + '_' + wh;
    const exist = stock.find(s => s.key === key);
    if (exist) exist.qty -= d.qty;
  });
  DB.set('stock', stock);
  
  const bills = DB.get('outBills');
  bills.unshift(bill);
  DB.set('outBills', bills);
  
  toast('✅ 出库单已保存: ' + bill.no);
  outDetails = [];
  renderOutDetail();
  document.getElementById('outCustomer').value = '';
}

/* ============ 库存查询 ============ */
function getStock(goodsId, warehouse) {
  const s = DB.get('stock').find(x => x.key === goodsId + '_' + warehouse);
  return s ? s.qty : 0;
}

function loadStock() {
  const kw = (document.getElementById('stockSearch').value || '').toLowerCase();
  const stock = DB.get('stock');
  const goods = DB.get('goods');
  let totalQty = 0;
  let html = '';
  
  stock.forEach(s => {
    const g = goods.find(x => x.id === s.goodsId);
    if (!g) return;
    if (kw && !(g.id.toLowerCase().includes(kw) || g.name.toLowerCase().includes(kw))) return;
    totalQty += s.qty;
    html += `<div class="list-item">
      <div class="info">
        <div class="name">${g.name}</div>
        <div class="meta">${g.id} | ${g.spec || ''} | ${s.warehouse}</div>
      </div>
      <div class="qty">${s.qty.toFixed(0)} ${g.unit || ''}</div>
    </div>`;
  });
  
  document.getElementById('stockList').innerHTML = html || '<div class="empty">暂无库存数据</div>';
  document.getElementById('statGoods').textContent = goods.length;
  document.getElementById('statQty').textContent = totalQty.toFixed(0);
}

document.getElementById('stockSearch').addEventListener('input', loadStock);

/* ============ 商品管理 ============ */
function loadGoods() {
  const kw = (document.getElementById('goodsSearch').value || '').toLowerCase();
  const goods = DB.get('goods').filter(g =>
    !kw || g.id.toLowerCase().includes(kw) || g.name.toLowerCase().includes(kw)
  );
  let html = '';
  goods.forEach(g => {
    html += `<div class="list-item">
      <div class="info">
        <div class="name">${g.name}</div>
        <div class="meta">${g.id} | 条码:${g.barcode || '无'} | ${g.spec || ''} ${g.unit || ''}</div>
        <div class="meta">采购价:¥${(g.price||0).toFixed(2)} | 销售价:¥${(g.sale||0).toFixed(2)}</div>
      </div>
      <div class="actions">
        <button class="btn btn-small" onclick="editGoods('${g.id}')">编辑</button>
        <button class="btn btn-small btn-red" onclick="deleteGoods('${g.id}')">删</button>
      </div>
    </div>`;
  });
  document.getElementById('goodsList').innerHTML = html || '<div class="empty">暂无商品,请点击右上角"新增商品"</div>';
}

document.getElementById('goodsSearch').addEventListener('input', loadGoods);

function showGoodsEdit() {
  document.getElementById('goodsEditTitle').textContent = '新增商品';
  ['editGoodsId','editGoodsCode','editBarcode','editName','editSpec','editUnit','editPrice','editSale']
    .forEach(id => document.getElementById(id).value = '');
  document.getElementById('goodsEditModal').style.display = 'flex';
}

function editGoods(id) {
  const g = DB.get('goods').find(x => x.id === id);
  if (!g) return;
  document.getElementById('goodsEditTitle').textContent = '编辑商品';
  document.getElementById('editGoodsId').value = g.id;
  document.getElementById('editGoodsCode').value = g.id;
  document.getElementById('editBarcode').value = g.barcode || '';
  document.getElementById('editName').value = g.name;
  document.getElementById('editSpec').value = g.spec || '';
  document.getElementById('editUnit').value = g.unit || '';
  document.getElementById('editPrice').value = g.price || 0;
  document.getElementById('editSale').value = g.sale || 0;
  document.getElementById('goodsEditModal').style.display = 'flex';
}

function closeGoodsEdit() {
  document.getElementById('goodsEditModal').style.display = 'none';
}

function saveGoods() {
  const code = document.getElementById('editGoodsCode').value.trim();
  const name = document.getElementById('editName').value.trim();
  if (!code || !name) { toast('编码和名称必填'); return; }
  
  const goods = DB.get('goods');
  const editId = document.getElementById('editGoodsId').value;
  const obj = {
    id: code,
    barcode: document.getElementById('editBarcode').value.trim(),
    name,
    spec: document.getElementById('editSpec').value.trim(),
    unit: document.getElementById('editUnit').value.trim(),
    price: parseFloat(document.getElementById('editPrice').value) || 0,
    sale: parseFloat(document.getElementById('editSale').value) || 0
  };
  
  if (editId) {
    const i = goods.findIndex(g => g.id === editId);
    if (i >= 0) goods[i] = obj;
  } else {
    if (goods.find(g => g.id === code)) { toast('编码已存在'); return; }
    goods.push(obj);
  }
  DB.set('goods', goods);
  closeGoodsEdit();
  loadGoods();
  toast('✅ 保存成功');
}

function deleteGoods(id) {
  if (!confirm('确定删除商品 ' + id + '?')) return;
  DB.set('goods', DB.get('goods').filter(g => g.id !== id));
  loadGoods();
  toast('✅ 已删除');
}

/* ============ 单据管理 ============ */
function switchBillTab(el) {
  document.querySelectorAll('[data-billType]').forEach(x => x.classList.remove('active'));
  el.classList.add('active');
  loadBills();
}

function loadBills() {
  const filter = document.querySelector('[data-billType].active').dataset.billtype;
  let html = '';
  if (filter === 'all' || filter === 'in') {
    DB.get('inBills').forEach(b => {
      html += billItemHtml(b, 'in');
    });
  }
  if (filter === 'all' || filter === 'out') {
    DB.get('outBills').forEach(b => {
      html += billItemHtml(b, 'out');
    });
  }
  document.getElementById('billsList').innerHTML = html || '<div class="empty">暂无单据</div>';
}

function billItemHtml(b, type) {
  const cls = type === 'in' ? 'tag-in' : 'tag-out';
  const text = type === 'in' ? '入库' : '出库';
  return `<div class="list-item" onclick="showBillDetail('${type}','${b.no}')">
    <div class="info">
      <div class="name"><span class="tag ${cls}">${text}</span>${b.no}</div>
      <div class="meta">${b.type} | ${b.warehouse} | ${b.date}</div>
      <div class="meta">${b.details.length} 个明细 | 合计 ¥${b.total.toFixed(2)}</div>
    </div>
  </div>`;
}

function showBillDetail(type, no) {
  const key = type === 'in' ? 'inBills' : 'outBills';
  const b = DB.get(key).find(x => x.no === no);
  if (!b) return;
  
  let rows = '';
  b.details.forEach(d => {
    rows += `<tr>
      <td>${d.goodsId}</td><td>${d.name}</td>
      <td>${d.qty}</td><td>¥${d.total.toFixed(2)}</td>
    </tr>`;
  });
  
  document.getElementById('billDetailTitle').textContent = 
    (type === 'in' ? '入库单 ' : '出库单 ') + b.no;
  document.getElementById('billDetailContent').innerHTML = `
    <div class="meta" style="margin-bottom:10px;">
      类型:${b.type}<br>
      仓库:${b.warehouse}<br>
      ${type === 'in' ? '供应商:' + (b.supplier || '-') : '客户:' + (b.customer || '-')}<br>
      日期:${b.date}<br>
      操作员:${b.operator}
    </div>
    <table>
      <thead><tr><th>编码</th><th>名称</th><th>数量</th><th>金额</th></tr></thead>
      <tbody>${rows}</tbody>
    </table>
    <div style="text-align:right;margin-top:10px;font-weight:bold;color:#E53935;">
      合计:¥${b.total.toFixed(2)}
    </div>
  `;
  document.getElementById('billDetailModal').style.display = 'flex';
}

/* ============ 导出导入 ============ */
function exportData() {
  const data = {
    goods: DB.get('goods'),
    stock: DB.get('stock'),
    inBills: DB.get('inBills'),
    outBills: DB.get('outBills'),
    exportTime: new Date().toLocaleString('zh-CN')
  };
  const blob = new Blob([JSON.stringify(data, null, 2)], {type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'warehouse_' + Date.now() + '.json';
  a.click();
  URL.revokeObjectURL(url);
  toast('✅ 数据已导出');
}

function importData(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = ev => {
    try {
      const data = JSON.parse(ev.target.result);
      if (data.goods) DB.set('goods', data.goods);
      if (data.stock) DB.set('stock', data.stock);
      if (data.inBills) DB.set('inBills', data.inBills);
      if (data.outBills) DB.set('outBills', data.outBills);
      toast('✅ 数据导入成功');
      loadStock(); loadGoods(); loadBills();
    } catch { toast('❌ 文件格式错误'); }
  };
  reader.readAsText(file);
}

/* ============ 相机扫码 ============ */
async function openCamera(target) {
  currentScanTarget = target;
  const modal = document.getElementById('scanModal');
  const video = document.getElementById('scanVideo');
  modal.style.display = 'flex';
  try {
    scanStream = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: 'environment' }
    });
    video.srcObject = scanStream;
    
    // 简易条码识别(使用 BarcodeDetector API)
    if ('BarcodeDetector' in window) {
      const detector = new BarcodeDetector({
        formats: ['ean_13','ean_8','code_128','code_39','qr_code']
      });
      const tick = async () => {
        if (modal.style.display !== 'flex') return;
        try {
          const codes = await detector.detect(video);
          if (codes.length > 0) {
            const code = codes[0].rawValue;
            document.getElementById(target + 'Barcode').value = code;
            lookupGoods(target, code);
            closeCamera();
            return;
          }
        } catch {}
        requestAnimationFrame(tick);
      };
      tick();
    } else {
      toast('浏览器不支持扫码,请手动输入');
    }
  } catch (err) {
    toast('❌ 无法访问相机:' + err.message);
    closeCamera();
  }
}

function closeCamera() {
  const modal = document.getElementById('scanModal');
  modal.style.display = 'none';
  if (scanStream) {
    scanStream.getTracks().forEach(t => t.stop());
    scanStream = null;
  }
}

/* ============ 初始化 ============ */
loadStock();
loadGoods();
loadBills();
</script>

</body>
</html>
