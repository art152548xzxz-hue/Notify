<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>บันทึกออเดอร์สินค้า</title>
<style>
  :root{
    --bg:#f5f6f8; --card:#ffffff; --text:#1a1a1a; --sub:#666;
    --accent:#2563eb; --accent-text:#fff; --border:#e2e4e8;
    --done-bg:#e8f7ee; --done-text:#1b7a43; --wait-bg:#fff4e5; --wait-text:#b8570a;
    --paid-bg:#e8f7ee; --paid-text:#1b7a43; --unpaid-bg:#fdeaea; --unpaid-text:#c0392b;
  }
  @media (prefers-color-scheme: dark){
    :root:not([data-theme="light"]){
      --bg:#15161a; --card:#1f2126; --text:#eee; --sub:#9aa0aa;
      --accent:#3b82f6; --accent-text:#fff; --border:#2c2f36;
      --done-bg:#123322; --done-text:#5fd695; --wait-bg:#3a2a12; --wait-text:#f0a94e;
      --paid-bg:#123322; --paid-text:#5fd695; --unpaid-bg:#3a1414; --unpaid-text:#f28b8b;
    }
  }
  :root[data-theme="dark"]{
    --bg:#15161a; --card:#1f2126; --text:#eee; --sub:#9aa0aa;
    --accent:#3b82f6; --accent-text:#fff; --border:#2c2f36;
    --done-bg:#123322; --done-text:#5fd695; --wait-bg:#3a2a12; --wait-text:#f0a94e;
    --paid-bg:#123322; --paid-text:#5fd695; --unpaid-bg:#3a1414; --unpaid-text:#f28b8b;
  }
  *{box-sizing:border-box;}
  body{
    margin:0; background:var(--bg); color:var(--text);
    font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,sans-serif;
    padding:16px; max-width:520px; margin:0 auto;
  }
  h1{font-size:1.2rem; margin:12px 0 4px;}
  h2{font-size:1rem; margin:0 0 10px;}
  p.sub{color:var(--sub); font-size:0.88rem; margin:0 0 16px;}
  .card{background:var(--card); border:1px solid var(--border); border-radius:14px; padding:16px; margin-bottom:14px;}
  label{display:block; font-size:0.85rem; color:var(--sub); margin:10px 0 4px;}
  select, input, textarea{
    width:100%; padding:10px 12px; border-radius:10px; border:1px solid var(--border);
    background:var(--bg); color:var(--text); font-size:1rem; font-family:inherit;
  }
  textarea{resize:vertical; min-height:50px;}
  .info-box{
    background:var(--bg); border:1px dashed var(--border); border-radius:10px;
    padding:8px 10px; font-size:0.82rem; color:var(--sub); margin-top:6px;
  }
  .radio-row{display:flex; gap:10px; margin-top:6px;}
  .radio-opt{
    flex:1; display:flex; align-items:center; gap:6px; border:1px solid var(--border);
    border-radius:10px; padding:9px 10px; font-size:0.9rem; cursor:pointer;
  }
  .radio-opt input{width:auto;}
  button{font-family:inherit; font-size:0.95rem; border:none; border-radius:10px; padding:10px 14px; cursor:pointer;}
  .btn-primary{background:var(--accent); color:var(--accent-text); width:100%; margin-top:14px; font-weight:600; padding:12px;}
  .btn-primary:active{opacity:0.85;}
  .btn-secondary{background:var(--bg); color:var(--text); border:1px solid var(--border); width:100%; margin-top:6px; font-weight:600; padding:11px;}
  .row2{display:flex; gap:10px;}
  .row2 > div{flex:1;}
  .order, .product-item{border:1px solid var(--border); border-radius:12px; padding:12px; margin-bottom:10px; background:var(--card);}
  .order-top{display:flex; justify-content:space-between; align-items:flex-start; gap:8px;}
  .order-title{font-weight:600;}
  .order-meta{font-size:0.82rem; color:var(--sub); margin-top:2px;}
  .badges{display:flex; flex-direction:column; gap:4px; align-items:flex-end;}
  .badge{font-size:0.72rem; padding:3px 8px; border-radius:20px; white-space:nowrap;}
  .badge-wait{background:var(--wait-bg); color:var(--wait-text);}
  .badge-done{background:var(--done-bg); color:var(--done-text);}
  .badge-paid{background:var(--paid-bg); color:var(--paid-text);}
  .badge-unpaid{background:var(--unpaid-bg); color:var(--unpaid-text);}
  .order-actions{display:flex; gap:8px; margin-top:10px; flex-wrap:wrap;}
  .btn-small{background:var(--bg); color:var(--text); border:1px solid var(--border); font-size:0.82rem; padding:7px 10px;}
  .btn-danger{color:#d33; border-color:#d33;}
  .empty{color:var(--sub); text-align:center; padding:30px 0; font-size:0.9rem;}
  .toast{
    position:fixed; bottom:20px; left:50%; transform:translateX(-50%);
    background:var(--text); color:var(--bg); padding:10px 18px; border-radius:20px;
    font-size:0.85rem; opacity:0; transition:opacity 0.3s; pointer-events:none; max-width:90%;
  }
  .toast.show{opacity:0.95;}
  #historyWrap, #manageWrap{display:none;}
  hr{border:none; border-top:1px solid var(--border); margin:16px 0;}
</style>
</head>
<body>
  <h1>📦 บันทึกออเดอร์สินค้า</h1>
  <p class="sub">เลือกสินค้า ระบุจำนวนและสถานะการชำระเงิน เพื่อบันทึกและติดตามสถานะการรับของ</p>

  <div class="card">
    <label for="product">เลือกสินค้า</label>
    <select id="product" onchange="onProductChange()"></select>
    <div class="info-box" id="productInfo" style="display:none;"></div>

    <label for="qty" id="qtyLabel">จำนวน</label>
    <input id="qty" type="number" min="1" placeholder="ระบุจำนวน">

    <label>สถานะการชำระเงิน</label>
    <div class="radio-row">
      <label class="radio-opt"><input type="radio" name="paid" value="unpaid" checked> ยังไม่ได้ชำระ</label>
      <label class="radio-opt"><input type="radio" name="paid" value="paid"> ชำระแล้ว</label>
    </div>

    <label for="note">หมายเหตุ (ถ้ามี)</label>
    <textarea id="note" placeholder="เช่น เลขพัสดุ, ล็อตที่จัดส่ง"></textarea>

    <button class="btn-primary" onclick="addOrder()">➕ บันทึกออเดอร์</button>
  </div>

  <button class="btn-small" style="width:100%; margin-bottom:8px;" id="historyToggleBtn" onclick="toggleHistory()">📜 ดูประวัติการสั่ง</button>
  <button class="btn-small" style="width:100%; margin-bottom:14px;" id="manageToggleBtn" onclick="toggleManage()">🛠️ จัดการสินค้า / แหล่งซื้อ</button>

  <div id="historyWrap">
    <div id="list"></div>
  </div>

  <div id="manageWrap">
    <div class="card">
      <h2 id="manageFormTitle">➕ เพิ่มสินค้าใหม่</h2>
      <label for="pLabel">ชื่อสินค้า</label>
      <input id="pLabel" placeholder="เช่น นมพาสเจอร์ไรส์ 2000 ml">

      <div class="row2">
        <div>
          <label for="pUnit">หน่วยนับ</label>
          <input id="pUnit" placeholder="เช่น แกลลอน, ชุด">
        </div>
        <div>
          <label for="pDefaultQty">จำนวนเริ่มต้น</label>
          <input id="pDefaultQty" type="number" min="0" placeholder="ไม่บังคับ">
        </div>
      </div>

      <label for="pSupplier">แหล่งซื้อ / ผู้จำหน่าย</label>
      <input id="pSupplier" placeholder="เช่น บริษัท/ร้านค้า">

      <div class="row2">
        <div>
          <label for="pPrice">ราคาต่อหน่วย (บาท)</label>
          <input id="pPrice" type="number" min="0" step="0.01" placeholder="ไม่บังคับ">
        </div>
        <div>
          <label for="pUnitDetail">รายละเอียดหน่วย</label>
          <input id="pUnitDetail" placeholder="เช่น ชุดละ 52 แผ่น">
        </div>
      </div>

      <button class="btn-primary" id="pSaveBtn" onclick="saveProductFromForm()">➕ เพิ่มสินค้า</button>
      <button class="btn-secondary" id="pCancelBtn" style="display:none;" onclick="cancelEditProduct()">ยกเลิกการแก้ไข</button>
    </div>

    <hr>
    <div id="productList"></div>
  </div>

  <div class="toast" id="toast"></div>

<script>
const PRODUCTS_KEY = "products_v1";
const ORDERS_KEY = "partner_orders_v3";
let editingProductId = null;

const DEFAULT_PRODUCTS = [
  {
    id: "p_milk",
    label: "นมพาสเจอร์ไรส์ 2000 ml (รสจืด)",
    unit: "แกลลอน",
    supplier: "บริษัท ปโยสิน พันธมิตรร่วมค้า 999 จำกัด",
    defaultQty: 12,
    pricePerUnit: null,
    unitDetail: null
  },
  {
    id: "p_sticker",
    label: "Stickers Yoghurt Drink",
    unit: "ชุด",
    supplier: "ร้าน Dollaya Printing",
    defaultQty: 1,
    pricePerUnit: 95,
    unitDetail: "ชุดละ 52 แผ่น"
  }
];

// ---------- storage helpers ----------
function loadProducts(){
  try{
    const raw = localStorage.getItem(PRODUCTS_KEY);
    if(!raw){
      saveProducts(DEFAULT_PRODUCTS);
      return DEFAULT_PRODUCTS.slice();
    }
    return JSON.parse(raw);
  }catch(e){
    console.error("โหลดสินค้าไม่สำเร็จ", e);
    return DEFAULT_PRODUCTS.slice();
  }
}

function saveProducts(products){
  try{
    localStorage.setItem(PRODUCTS_KEY, JSON.stringify(products));
    return true;
  }catch(e){
    console.error("บันทึกสินค้าไม่สำเร็จ", e);
    return false;
  }
}

function loadOrders(){
  try{
    const raw = localStorage.getItem(ORDERS_KEY);
    return raw ? JSON.parse(raw) : [];
  }catch(e){
    console.error("โหลดออเดอร์ไม่สำเร็จ", e);
    return [];
  }
}

function saveOrders(orders){
  try{
    localStorage.setItem(ORDERS_KEY, JSON.stringify(orders));
    return true;
  }catch(e){
    console.error("บันทึกออเดอร์ไม่สำเร็จ", e);
    return false;
  }
}

// ---------- utils ----------
function showToast(msg){
  const t = document.getElementById("toast");
  t.textContent = msg;
  t.classList.add("show");
  setTimeout(()=> t.classList.remove("show"), 2200);
}

function formatDate(iso){
  const d = new Date(iso);
  return d.toLocaleDateString('th-TH', {day:'numeric', month:'short', year:'2-digit'}) +
         " " + d.toLocaleTimeString('th-TH', {hour:'2-digit', minute:'2-digit'});
}

function money(n){
  return n.toLocaleString('th-TH', {minimumFractionDigits:0, maximumFractionDigits:2});
}

function escapeHtml(str){
  const div = document.createElement("div");
  div.textContent = str == null ? "" : str;
  return div.innerHTML;
}

// ---------- product dropdown (order form) ----------
function renderProductDropdown(){
  const products = loadProducts();
  const select = document.getElementById("product");
  const current = select.value;
  if(products.length === 0){
    select.innerHTML = '<option value="">-- ยังไม่มีสินค้า กรุณาเพิ่มก่อน --</option>';
    document.getElementById("productInfo").style.display = "none";
    return;
  }
  select.innerHTML = '<option value="">-- เลือกสินค้า --</option>' +
    products.map(p => `<option value="${p.id}">${escapeHtml(p.label)}</option>`).join("");
  if(products.some(p => p.id === current)) select.value = current;
  onProductChange();
}

function onProductChange(){
  const products = loadProducts();
  const key = document.getElementById("product").value;
  const infoBox = document.getElementById("productInfo");
  const qtyLabel = document.getElementById("qtyLabel");
  const qtyInput = document.getElementById("qty");
  const p = products.find(x => x.id === key);
  if(!p){
    infoBox.style.display = "none";
    qtyLabel.textContent = "จำนวน";
    qtyInput.value = "";
    return;
  }
  let info = `หน่วย: ${p.unit}`;
  if(p.unitDetail) info += ` (${p.unitDetail})`;
  if(p.pricePerUnit != null) info += ` · ราคาต่อหน่วย ${money(p.pricePerUnit)} บาท`;
  info += ` · จาก ${p.supplier}`;
  if(p.lastOrderedAt) info += ` · สั่งล่าสุดเมื่อ ${formatDate(p.lastOrderedAt)}`;
  infoBox.style.display = "block";
  infoBox.textContent = info;
  qtyLabel.textContent = `จำนวน (${p.unit})`;
  // ใช้จำนวนล่าสุดที่เคยสั่งจริงเป็นค่าตั้งต้น ถ้ายังไม่เคยสั่งเลยจึงใช้จำนวนเริ่มต้นที่ตั้งไว้
  qtyInput.value = (p.lastQty != null ? p.lastQty : p.defaultQty) || "";
  const paidValue = p.lastPaid != null ? (p.lastPaid ? "paid" : "unpaid") : "unpaid";
  const radio = document.querySelector(`input[name="paid"][value="${paidValue}"]`);
  if(radio) radio.checked = true;
}

// ---------- order actions ----------
function buildMessage(order){
  let msg = `📦 สั่งของแล้วนะ!\nสินค้า: ${order.productLabel}\nจำนวน: ${order.qty} ${order.unit}`;
  if(order.unitDetail) msg += ` (${order.unitDetail})`;
  msg += `\nจาก: ${order.supplier}`;
  if(order.totalPrice != null) msg += `\nราคารวม: ${money(order.totalPrice)} บาท`;
  msg += `\nสถานะชำระเงิน: ${order.paid ? "ชำระแล้ว ✅" : "ยังไม่ได้ชำระ ⚠️"}`;
  if(order.note) msg += `\nหมายเหตุ: ${order.note}`;
  msg += `\nสั่งเมื่อ: ${formatDate(order.createdAt)}\n\nฝากไปรับที่ร้านด้วยนะ 🙏`;
  return msg;
}

async function copyOrder(id){
  const orders = loadOrders();
  const o = orders.find(x => x.id === id);
  if(!o) return;
  const message = buildMessage(o);
  try{
    await navigator.clipboard.writeText(message);
    showToast("คัดลอกข้อความสั่งแล้ว 📋");
  }catch(e){
    try{
      const ta = document.createElement("textarea");
      ta.value = message;
      ta.style.position = "fixed";
      ta.style.opacity = "0";
      document.body.appendChild(ta);
      ta.focus();
      ta.select();
      document.execCommand("copy");
      document.body.removeChild(ta);
      showToast("คัดลอกข้อความสั่งแล้ว 📋");
    }catch(e2){
      showToast("คัดลอกไม่สำเร็จ กรุณาคัดลอกด้วยตนเอง");
    }
  }
}

async function addOrder(){
  const products = loadProducts();
  const productId = document.getElementById("product").value;
  const qty = parseFloat(document.getElementById("qty").value);
  const paid = document.querySelector('input[name="paid"]:checked').value === "paid";
  const note = document.getElementById("note").value.trim();

  const p = products.find(x => x.id === productId);
  if(!p){
    showToast("กรุณาเลือกสินค้าก่อนนะ");
    return;
  }
  if(!qty || qty <= 0){
    showToast("กรุณาระบุจำนวนให้ถูกต้อง");
    return;
  }

  const totalPrice = p.pricePerUnit != null ? qty * p.pricePerUnit : null;

  const order = {
    id: Date.now().toString(),
    productId: p.id,
    productLabel: p.label,
    unit: p.unit,
    unitDetail: p.unitDetail || null,
    supplier: p.supplier,
    pricePerUnit: p.pricePerUnit,
    qty, note, paid, totalPrice,
    createdAt: new Date().toISOString(),
    picked: false
  };

  const orders = loadOrders();
  orders.unshift(order);
  saveOrders(orders);

  // จดจำค่าที่ใช้จริงล่าสุดไว้กับสินค้านี้ เพื่อใช้เป็นค่าตั้งต้นครั้งถัดไป (ไม่ย้อนกลับไปค่าตั้งต้นเดิม)
  p.lastQty = qty;
  p.lastPaid = paid;
  p.lastOrderedAt = order.createdAt;
  saveProducts(products);

  document.getElementById("product").value = "";
  onProductChange();
  document.getElementById("note").value = "";
  document.querySelector('input[name="paid"][value="unpaid"]').checked = true;

  renderOrders();
  await copyOrder(order.id);
  showToast("บันทึกออเดอร์แล้ว ✅ และคัดลอกข้อความสั่งแล้ว 📋");
}

function togglePicked(id){
  const orders = loadOrders();
  const o = orders.find(o => o.id === id);
  if(o){ o.picked = !o.picked; saveOrders(orders); renderOrders(); }
}

function togglePaid(id){
  const orders = loadOrders();
  const o = orders.find(o => o.id === id);
  if(o){ o.paid = !o.paid; saveOrders(orders); renderOrders(); }
}

function deleteOrder(id){
  if(!confirm("ลบรายการออเดอร์นี้ใช่ไหม?")) return;
  let orders = loadOrders();
  orders = orders.filter(o => o.id !== id);
  saveOrders(orders);
  renderOrders();
}

function renderOrders(){
  const orders = loadOrders();
  const list = document.getElementById("list");
  if(orders.length === 0){
    list.innerHTML = '<div class="empty">ยังไม่มีออเดอร์ที่บันทึกไว้</div>';
    return;
  }
  list.innerHTML = orders.map(o => `
    <div class="order">
      <div class="order-top">
        <div>
          <div class="order-title">${escapeHtml(o.productLabel)}</div>
          <div class="order-meta">${o.qty} ${escapeHtml(o.unit)}${o.totalPrice != null ? " · " + money(o.totalPrice) + " บาท" : ""}</div>
          <div class="order-meta">${escapeHtml(o.supplier)}</div>
          <div class="order-meta">${formatDate(o.createdAt)}</div>
          ${o.note ? `<div class="order-meta">📝 ${escapeHtml(o.note)}</div>` : ""}
        </div>
        <div class="badges">
          <span class="badge ${o.picked ? 'badge-done' : 'badge-wait'}">${o.picked ? "รับแล้ว" : "รอรับ"}</span>
          <span class="badge ${o.paid ? 'badge-paid' : 'badge-unpaid'}">${o.paid ? "ชำระแล้ว" : "ยังไม่ชำระ"}</span>
        </div>
      </div>
      <div class="order-actions">
        <button class="btn-small" onclick="copyOrder('${o.id}')">📋 คัดลอกข้อความ</button>
        <button class="btn-small" onclick="togglePicked('${o.id}')">${o.picked ? "↩️ ยังไม่ได้รับ" : "✅ รับแล้ว"}</button>
        <button class="btn-small" onclick="togglePaid('${o.id}')">${o.paid ? "↩️ ยังไม่ชำระ" : "💰 ชำระแล้ว"}</button>
        <button class="btn-small btn-danger" onclick="deleteOrder('${o.id}')">🗑️ ลบ</button>
      </div>
    </div>
  `).join("");
}

function toggleHistory(){
  const wrap = document.getElementById("historyWrap");
  const btn = document.getElementById("historyToggleBtn");
  const showing = wrap.style.display === "block";
  if(showing){
    wrap.style.display = "none";
    btn.textContent = "📜 ดูประวัติการสั่ง";
  }else{
    renderOrders();
    wrap.style.display = "block";
    btn.textContent = "🔼 ซ่อนประวัติการสั่ง";
  }
}

// ---------- product management ----------
function toggleManage(){
  const wrap = document.getElementById("manageWrap");
  const btn = document.getElementById("manageToggleBtn");
  const showing = wrap.style.display === "block";
  if(showing){
    wrap.style.display = "none";
    btn.textContent = "🛠️ จัดการสินค้า / แหล่งซื้อ";
  }else{
    cancelEditProduct();
    renderProductList();
    wrap.style.display = "block";
    btn.textContent = "🔼 ซ่อนการจัดการสินค้า";
  }
}

function renderProductList(){
  const products = loadProducts();
  const wrap = document.getElementById("productList");
  if(products.length === 0){
    wrap.innerHTML = '<div class="empty">ยังไม่มีสินค้า เพิ่มได้จากฟอร์มด้านบน</div>';
    return;
  }
  wrap.innerHTML = products.map(p => `
    <div class="product-item">
      <div class="order-title">${escapeHtml(p.label)}</div>
      <div class="order-meta">หน่วย: ${escapeHtml(p.unit)}${p.unitDetail ? " (" + escapeHtml(p.unitDetail) + ")" : ""}</div>
      <div class="order-meta">แหล่งซื้อ: ${escapeHtml(p.supplier)}</div>
      ${p.pricePerUnit != null ? `<div class="order-meta">ราคาต่อหน่วย: ${money(p.pricePerUnit)} บาท</div>` : ""}
      <div class="order-actions">
        <button class="btn-small" onclick="startEditProduct('${p.id}')">✏️ แก้ไข</button>
        <button class="btn-small btn-danger" onclick="deleteProduct('${p.id}')">🗑️ ลบสินค้านี้</button>
      </div>
    </div>
  `).join("");
}

function clearProductForm(){
  document.getElementById("pLabel").value = "";
  document.getElementById("pUnit").value = "";
  document.getElementById("pDefaultQty").value = "";
  document.getElementById("pSupplier").value = "";
  document.getElementById("pPrice").value = "";
  document.getElementById("pUnitDetail").value = "";
}

function startEditProduct(id){
  const products = loadProducts();
  const p = products.find(x => x.id === id);
  if(!p) return;
  editingProductId = id;
  document.getElementById("pLabel").value = p.label;
  document.getElementById("pUnit").value = p.unit;
  document.getElementById("pDefaultQty").value = p.defaultQty || "";
  document.getElementById("pSupplier").value = p.supplier;
  document.getElementById("pPrice").value = p.pricePerUnit != null ? p.pricePerUnit : "";
  document.getElementById("pUnitDetail").value = p.unitDetail || "";
  document.getElementById("manageFormTitle").textContent = "✏️ แก้ไขสินค้า";
  document.getElementById("pSaveBtn").textContent = "💾 บันทึกการแก้ไข";
  document.getElementById("pCancelBtn").style.display = "block";
  window.scrollTo({top:0, behavior:"smooth"});
}

function cancelEditProduct(){
  editingProductId = null;
  clearProductForm();
  document.getElementById("manageFormTitle").textContent = "➕ เพิ่มสินค้าใหม่";
  document.getElementById("pSaveBtn").textContent = "➕ เพิ่มสินค้า";
  document.getElementById("pCancelBtn").style.display = "none";
}

function saveProductFromForm(){
  const label = document.getElementById("pLabel").value.trim();
  const unit = document.getElementById("pUnit").value.trim();
  const supplier = document.getElementById("pSupplier").value.trim();
  const defaultQtyRaw = document.getElementById("pDefaultQty").value;
  const priceRaw = document.getElementById("pPrice").value;
  const unitDetail = document.getElementById("pUnitDetail").value.trim();

  if(!label || !unit || !supplier){
    showToast("กรุณากรอกชื่อสินค้า หน่วยนับ และแหล่งซื้อให้ครบ");
    return;
  }

  const products = loadProducts();

  if(editingProductId){
    const p = products.find(x => x.id === editingProductId);
    if(p){
      p.label = label;
      p.unit = unit;
      p.supplier = supplier;
      p.defaultQty = defaultQtyRaw ? parseFloat(defaultQtyRaw) : null;
      p.pricePerUnit = priceRaw !== "" ? parseFloat(priceRaw) : null;
      p.unitDetail = unitDetail || null;
    }
    showToast("แก้ไขสินค้าแล้ว ✅");
  }else{
    products.push({
      id: "p_" + Date.now().toString(),
      label, unit, supplier,
      defaultQty: defaultQtyRaw ? parseFloat(defaultQtyRaw) : null,
      pricePerUnit: priceRaw !== "" ? parseFloat(priceRaw) : null,
      unitDetail: unitDetail || null
    });
    showToast("เพิ่มสินค้าแล้ว ✅");
  }

  saveProducts(products);
  cancelEditProduct();
  renderProductList();
  renderProductDropdown();
}

function deleteProduct(id){
  if(!confirm("ลบสินค้านี้ใช่ไหม? (ประวัติการสั่งเดิมจะยังอยู่เหมือนเดิม)")) return;
  let products = loadProducts();
  products = products.filter(p => p.id !== id);
  saveProducts(products);
  if(editingProductId === id) cancelEditProduct();
  renderProductList();
  renderProductDropdown();
  showToast("ลบสินค้าแล้ว");
}

// ---------- init ----------
renderProductDropdown();
renderOrders();
</script>
</body>
</html>
