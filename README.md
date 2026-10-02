<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="theme-color" content="#111827">
<title>บัญชีร้าน</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:system-ui,-apple-system,"Segoe UI",sans-serif;background:#f3f4f6;color:#1f2937}
button,input,select{font:inherit}
button{cursor:pointer;border:0}
.app{max-width:760px;margin:auto;padding-bottom:80px}
header{background:#111827;color:white;padding:18px 16px 20px;position:sticky;top:0;z-index:10}
header h1{margin:0;font-size:21px}
header p{margin:4px 0 0;color:#cbd5e1;font-size:13px}
.tabs{display:flex;gap:7px;overflow-x:auto;background:white;padding:9px;position:sticky;top:76px;z-index:9;border-bottom:1px solid #e5e7eb}
.tabs button{white-space:nowrap;background:#f1f5f9;color:#475569;padding:9px 13px;border-radius:9px}
.tabs button.active{background:#111827;color:white}
.page{display:none;padding:12px}.page.active{display:block}
.cards{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
.card{background:white;border-radius:13px;padding:14px;box-shadow:0 1px 4px #00000010}
.card .label{font-size:12px;color:#64748b}.card .value{font-size:21px;font-weight:700;margin-top:5px}
.green{color:#059669}.red{color:#dc2626}.blue{color:#2563eb}.orange{color:#ea580c}
.section{background:white;border-radius:13px;margin-top:12px;padding:14px;box-shadow:0 1px 4px #00000010}
.section h2{font-size:16px;margin:0 0 12px}
.form{display:grid;gap:9px}
.row{display:grid;grid-template-columns:1fr 1fr;gap:9px}
label{font-size:12px;color:#64748b}
input,select{width:100%;padding:11px;border:1px solid #d1d5db;border-radius:9px;background:white;outline:none}
input:focus,select:focus{border-color:#64748b}
.btn{padding:11px 14px;border-radius:9px;font-weight:600}
.btn-primary{background:#111827;color:white}.btn-green{background:#059669;color:white}
.btn-red{background:#dc2626;color:white}.btn-blue{background:#2563eb;color:white}
.btn-gray{background:#e5e7eb;color:#374151}
.actions{display:flex;gap:7px;flex-wrap:wrap}
.table-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:13px}
th,td{text-align:left;padding:9px 6px;border-bottom:1px solid #e5e7eb;white-space:nowrap}
th{color:#64748b;font-weight:600}
.empty{text-align:center;color:#94a3b8;padding:25px 10px}
.item{display:flex;justify-content:space-between;gap:10px;padding:11px 0;border-bottom:1px solid #eee}
.item:last-child{border-bottom:0}
.item small{display:block;color:#64748b;margin-top:3px}
.badge{display:inline-block;padding:3px 7px;border-radius:20px;font-size:11px;background:#e5e7eb}
.badge.in{background:#dcfce7;color:#166534}.badge.out{background:#fee2e2;color:#991b1b}
.stock-low{color:#dc2626;font-weight:700}
.search{margin-bottom:9px}
.total-box{display:flex;justify-content:space-between;background:#f8fafc;padding:11px;border-radius:9px;margin-top:10px}
footer{position:fixed;bottom:0;left:0;right:0;background:white;border-top:1px solid #ddd;padding:8px;text-align:center;font-size:11px;color:#64748b}
@media(min-width:700px){.cards{grid-template-columns:repeat(4,1fr)}}
</style>
</head>
<body>
<div class="app">
<header>
  <h1>🧾 บัญชีร้านขายของชำ</h1>
  <p>รายรับ • รายจ่าย • สินค้าเข้า • สินค้าออก • สต๊อก</p>
</header>

<nav class="tabs">
<button class="active" onclick="showPage('home',this)">ภาพรวม</button>
<button onclick="showPage('income',this)">รายรับ</button>
<button onclick="showPage('expense',this)">รายจ่าย</button>
<button onclick="showPage('stock',this)">สต๊อก</button>
<button onclick="showPage('products',this)">สินค้า</button>
<button onclick="showPage('backup',this)">สำรองข้อมูล</button>
</nav>

<main>
<section id="home" class="page active">
  <div class="cards">
    <div class="card"><div class="label">รายรับวันนี้</div><div id="todayIncome" class="value green">฿0.00</div></div>
    <div class="card"><div class="label">รายจ่ายวันนี้</div><div id="todayExpense" class="value red">฿0.00</div></div>
    <div class="card"><div class="label">กำไรวันนี้*</div><div id="todayProfit" class="value blue">฿0.00</div></div>
    <div class="card"><div class="label">มูลค่าสต๊อก</div><div id="stockValue" class="value orange">฿0.00</div></div>
  </div>

  <div class="section">
    <h2>📅 สรุปเดือนนี้</h2>
    <div class="total-box"><span>รายรับ</span><b id="monthIncome" class="green">฿0.00</b></div>
    <div class="total-box"><span>รายจ่าย</span><b id="monthExpense" class="red">฿0.00</b></div>
    <div class="total-box"><span>กำไร/ส่วนต่าง</span><b id="monthProfit">฿0.00</b></div>
  </div>

  <div class="section">
    <h2>⚠️ สินค้าใกล้หมด</h2>
    <div id="lowStock"></div>
  </div>

  <div class="section">
    <h2>🕘 รายการล่าสุด</h2>
    <div id="recent"></div>
  </div>
  <p style="font-size:11px;color:#94a3b8">* กำไรในหน้านี้ = รายรับ - รายจ่ายที่บันทึกไว้ ยังไม่หักต้นทุนสินค้าที่ขายโดยอัตโนมัติ</p>
</section>

<section id="income" class="page">
  <div class="section">
    <h2>➕ บันทึกรายรับ / ยอดขาย</h2>
    <div class="form">
      <div class="row">
        <div><label>วันที่</label><input id="incomeDate" type="date"></div>
        <div><label>จำนวนเงิน</label><input id="incomeAmount" type="number" step="0.01" inputmode="decimal" placeholder="0.00"></div>
      </div>
      <div><div><label>รายละเอียด</label><input id="incomeNote" placeholder="เช่น ยอดขายหน้าร้าน"></div></div>
      <button class="btn btn-green" onclick="addIncome()">บันทึกรายรับ</button>
    </div>
  </div>
  <div class="section"><h2>รายการรายรับ</h2><div id="incomeList"></div></div>
</section>

<section id="expense" class="page">
  <div class="section">
    <h2>➖ บันทึกรายจ่าย</h2>
    <div class="form">
      <div class="row">
        <div><label>วันที่</label><input id="expenseDate" type="date"></div>
        <div><label>จำนวนเงิน</label><input id="expenseAmount" type="number" step="0.01" inputmode="decimal" placeholder="0.00"></div>
      </div>
      <div><label>หมวดหมู่</label>
        <select id="expenseCategory">
          <option>ซื้อสินค้า</option><option>ค่าเช่า</option><option>ค่าน้ำ</option><option>ค่าไฟ</option>
          <option>ค่าส่ง</option><option>อุปกรณ์</option><option>อื่นๆ</option>
        </select>
      </div>
      <div><label>รายละเอียด</label><input id="expenseNote" placeholder="เช่น ซื้อเครื่องดื่มเข้าร้าน"></div>
      <button class="btn btn-red" onclick="addExpense()">บันทึกรายจ่าย</button>
    </div>
  </div>
  <div class="section"><h2>รายการรายจ่าย</h2><div id="expenseList"></div></div>
</section>

<section id="stock" class="page">
  <div class="section">
    <h2>📦 รับสินค้าเข้า</h2>
    <div class="form">
      <select id="inProduct"></select>
      <div class="row">
        <input id="inQty" type="number" min="0" step="0.01" inputmode="decimal" placeholder="จำนวน">
        <input id="inCost" type="number" min="0" step="0.01" inputmode="decimal" placeholder="ต้นทุน/หน่วย">
      </div>
      <input id="inDate" type="date">
      <button class="btn btn-blue" onclick="stockIn()">บันทึกสินค้าเข้า</button>
      <small style="color:#64748b">ระบบจะเพิ่มมูลค่าต้นทุนลงในรายจ่ายให้อัตโนมัติ</small>
    </div>
  </div>

  <div class="section">
    <h2>🛒 สินค้าออก / ขาย</h2>
    <div class="form">
      <select id="outProduct"></select>
      <div class="row">
        <input id="outQty" type="number" min="0" step="0.01" inputmode="decimal" placeholder="จำนวน">
        <input id="outPrice" type="number" min="0" step="0.01" inputmode="decimal" placeholder="ราคาขาย/หน่วย">
      </div>
      <input id="outDate" type="date">
      <button class="btn btn-green" onclick="stockOut()">บันทึกสินค้าออก</button>
      <small style="color:#64748b">ระบบจะเพิ่มยอดขายลงในรายรับให้อัตโนมัติ</small>
    </div>
  </div>

  <div class="section"><h2>📊 สต๊อกปัจจุบัน</h2><div id="stockTable"></div></div>
</section>

<section id="products" class="page">
  <div class="section">
    <h2>➕ เพิ่มสินค้า</h2>
    <div class="form">
      <input id="productName" placeholder="ชื่อสินค้า เช่น น้ำดื่ม 600ml">
      <div class="row">
        <input id="productUnit" placeholder="หน่วย เช่น ขวด">
        <input id="productMin" type="number" min="0" step="0.01" placeholder="แจ้งเตือนเมื่อเหลือ">
      </div>
      <button class="btn btn-primary" onclick="addProduct()">เพิ่มสินค้า</button>
    </div>
  </div>
  <div class="section"><h2>รายการสินค้า</h2><div id="productList"></div></div>
</section>

<section id="backup" class="page">
  <div class="section">
    <h2>💾 สำรอง / กู้คืนข้อมูล</h2>
    <p style="font-size:13px;color:#64748b">ข้อมูลทั้งหมดเก็บอยู่ในเครื่องนี้ หากล้างข้อมูลเบราว์เซอร์ ข้อมูลอาจหาย ควรสำรองเป็นไฟล์ JSON ไว้</p>
    <div class="actions">
      <button class="btn btn-blue" onclick="exportData()">⬇️ สำรองข้อมูล</button>
      <button class="btn btn-gray" onclick="document.getElementById('importFile').click()">⬆️ กู้คืนข้อมูล</button>
      <input id="importFile" type="file" accept=".json" style="display:none" onchange="importData(event)">
    </div>
  </div>
  <div class="section">
    <h2>📄 ส่งออกบัญชีเป็น CSV</h2>
    <button class="btn btn-green" onclick="exportCSV()">ดาวน์โหลดรายการบัญชี</button>
  </div>
  <div class="section">
    <h2>⚠️ ล้างข้อมูลทั้งหมด</h2>
    <button class="btn btn-red" onclick="clearAll()">ล้างข้อมูลทั้งหมด</button>
  </div>
</section>
</main>
<footer>บัญชีร้าน • ข้อมูลเก็บในเครื่อง</footer>
</div>

<script>
const KEY='grocery_account_v1';
let db=JSON.parse(localStorage.getItem(KEY)||'null')||{
  products:[],
  income:[],
  expense:[],
  stockMoves:[]
};

function save(){localStorage.setItem(KEY,JSON.stringify(db));renderAll()}
function money(n){return '฿'+Number(n||0).toLocaleString('th-TH',{minimumFractionDigits:2,maximumFractionDigits:2})}
function today(){return new Date().toISOString().slice(0,10)}
function month(){return today().slice(0,7)}
function esc(s){return String(s??'').replace(/[&<>"']/g,m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'}[m]))}

function showPage(id,btn){
 document.querySelectorAll('.page').forEach(x=>x.classList.remove('active'));
 document.getElementById(id).classList.add('active');
 document.querySelectorAll('.tabs button').forEach(x=>x.classList.remove('active'));
 btn.classList.add('active');
 renderAll();
}
function setDates(){
 ['incomeDate','expenseDate','inDate','outDate'].forEach(id=>{if(!document.getElementById(id).value)document.getElementById(id).value=today()});
}
function addIncome(){
 const amount=+document.getElementById('incomeAmount').value;
 if(!(amount>0))return alert('กรุณาใส่จำนวนเงิน');
 db.income.unshift({id:Date.now(),date:document.getElementById('incomeDate').value,amount,note:document.getElementById('incomeNote').value||'รายรับ'});
 document.getElementById('incomeAmount').value='';document.getElementById('incomeNote').value='';
 save();
}
function addExpense(){
 const amount=+document.getElementById('expenseAmount').value;
 if(!(amount>0))return alert('กรุณาใส่จำนวนเงิน');
 db.expense.unshift({id:Date.now(),date:document.getElementById('expenseDate').value,amount,category:document.getElementById('expenseCategory').value,note:document.getElementById('expenseNote').value||'รายจ่าย'});
 document.getElementById('expenseAmount').value='';document.getElementById('expenseNote').value='';
 save();
}
function addProduct(){
 const name=document.getElementById('productName').value.trim();
 if(!name)return alert('กรุณาใส่ชื่อสินค้า');
 db.products.push({id:Date.now(),name,unit:document.getElementById('productUnit').value.trim()||'ชิ้น',min:+document.getElementById('productMin').value||0});
 document.getElementById('productName').value='';document.getElementById('productUnit').value='';document.getElementById('productMin').value='';
 save();
}
function stockQty(pid){
 return db.stockMoves.filter(x=>x.productId===pid).reduce((s,x)=>s+(x.type==='in'?x.qty:-x.qty),0)
}
function stockIn(){
 const pid=+document.getElementById('inProduct').value,qty=+document.getElementById('inQty').value,cost=+document.getElementById('inCost').value;
 if(!pid||!(qty>0)||cost<0)return alert('กรุณากรอกข้อมูลให้ครบ');
 const p=db.products.find(x=>x.id===pid);
 db.stockMoves.unshift({id:Date.now(),productId:pid,type:'in',qty,cost,date:document.getElementById('inDate').value});
 db.expense.unshift({id:Date.now()+1,date:document.getElementById('inDate').value,amount:qty*cost,category:'ซื้อสินค้า',note:'สินค้าเข้า: '+p.name+' x '+qty});
 document.getElementById('inQty').value='';document.getElementById('inCost').value='';save();
}
function stockOut(){
 const pid=+document.getElementById('outProduct').value,qty=+document.getElementById('outQty').value,price=+document.getElementById('outPrice').value;
 if(!pid||!(qty>0)||price<0)return alert('กรุณากรอกข้อมูลให้ครบ');
 const p=db.products.find(x=>x.id===pid),q=stockQty(pid);
 if(qty>q)return alert('สต๊อกไม่พอ มีเหลือ '+q+' '+p.unit);
 db.stockMoves.unshift({id:Date.now(),productId:pid,type:'out',qty,price,date:document.getElementById('outDate').value});
 db.income.unshift({id:Date.now()+1,date:document.getElementById('outDate').value,amount:qty*price,note:'ขาย: '+p.name+' x '+qty});
 document.getElementById('outQty').value='';document.getElementById('outPrice').value='';save();
}
function removeProduct(id){
 if(stockQty(id)!==0)return alert('ไม่สามารถลบสินค้าที่มีสต๊อกคงเหลือได้');
 if(confirm('ลบสินค้านี้?')){db.products=db.products.filter(p=>p.id!==id);save()}
}
function delIncome(id){if(confirm('ลบรายการรายรับนี้?')){db.income=db.income.filter(x=>x.id!==id);save()}}
function delExpense(id){if(confirm('ลบรายการรายจ่ายนี้?')){db.expense=db.expense.filter(x=>x.id!==id);save()}}

function renderSelects(){
 const opts='<option value="">-- เลือกสินค้า --</option>'+db.products.map(p=>`<option value="${p.id}">${esc(p.name)} (${stockQty(p.id)} ${esc(p.unit)})</option>`).join('');
 document.getElementById('inProduct').innerHTML=opts;
 document.getElementById('outProduct').innerHTML=opts;
}
function renderLists(){
 document.getElementById('incomeList').innerHTML=db.income.length?db.income.map(x=>`
 <div class="item"><div><b>${esc(x.note)}</b><small>${x.date}</small></div><div><b class="green">${money(x.amount)}</b><br><button class="badge" onclick="delIncome(${x.id})">ลบ</button></div></div>`).join(''):'<div class="empty">ยังไม่มีรายรับ</div>';
 document.getElementById('expenseList').innerHTML=db.expense.length?db.expense.map(x=>`
 <div class="item"><div><b>${esc(x.note)}</b><small>${x.date} • ${esc(x.category)}</small></div><div><b class="red">${money(x.amount)}</b><br><button class="badge" onclick="delExpense(${x.id})">ลบ</button></div></div>`).join(''):'<div class="empty">ยังไม่มีรายจ่าย</div>';
 document.getElementById('productList').innerHTML=db.products.length?db.products.map(p=>`
 <div class="item"><div><b>${esc(p.name)}</b><small>หน่วย: ${esc(p.unit)} • แจ้งเตือน: ${p.min||0}</small></div><div><b>${stockQty(p.id)} ${esc(p.unit)}</b><br><button class="badge" onclick="removeProduct(${p.id})">ลบ</button></div></div>`).join(''):'<div class="empty">ยังไม่มีสินค้า</div>';
}
function renderStock(){
 let rows=db.products.map(p=>{
  const q=stockQty(p.id),moves=db.stockMoves.filter(x=>x.productId===p.id),lastCost=[...moves].reverse().find(x=>x.type==='in')?.cost||0;
  return `<tr><td>${esc(p.name)}</td><td class="${q<=p.min?'stock-low':''}">${q} ${esc(p.unit)}</td><td>${money(q*lastCost)}</td></tr>`
 }).join('');
 document.getElementById('stockTable').innerHTML=rows?`<div class="table-wrap"><table><thead><tr><th>สินค้า</th><th>คงเหลือ</th><th>มูลค่าโดยประมาณ</th></tr></thead><tbody>${rows}</tbody></table></div>`:'<div class="empty">ยังไม่มีสินค้า</div>';
}
function renderDashboard(){
 const ti=db.income.filter(x=>x.date===today()).reduce((s,x)=>s+x.amount,0);
 const te=db.expense.filter(x=>x.date===today()).reduce((s,x)=>s+x.amount,0);
 const mi=db.income.filter(x=>x.date.startsWith(month())).reduce((s,x)=>s+x.amount,0);
 const me=db.expense.filter(x=>x.date.startsWith(month())).reduce((s,x)=>s+x.amount,0);
 const sv=db.products.reduce((s,p)=>{const q=stockQty(p.id),cost=[...db.stockMoves].reverse().find(x=>x.productId===p.id&&x.type==='in')?.cost||0;return s+q*cost},0);
 document.getElementById('todayIncome').textContent=money(ti);document.getElementById('todayExpense').textContent=money(te);document.getElementById('todayProfit').textContent=money(ti-te);document.getElementById('stockValue').textContent=money(sv);
 document.getElementById('monthIncome').textContent=money(mi);document.getElementById('monthExpense').textContent=money(me);document.getElementById('monthProfit').textContent=money(mi-me);
 const low=db.products.filter(p=>stockQty(p.id)<=p.min);
 document.getElementById('lowStock').innerHTML=low.length?low.map(p=>`<div class="item"><div>${esc(p.name)}</div><b class="stock-low">${stockQty(p.id)} ${esc(p.unit)}</b></div>`).join(''):'<div class="empty">ไม่มีสินค้าใกล้หมด 🎉</div>';
 const all=[...db.income.map(x=>({...x,t:'รายรับ'})),...db.expense.map(x=>({...x,t:'รายจ่าย'}))].sort((a,b)=>b.id-a.id).slice(0,8);
 document.getElementById('recent').innerHTML=all.length?all.map(x=>`<div class="item"><div><b>${esc(x.note)}</b><small>${x.date} • ${x.t}</small></div><b class="${x.t==='รายรับ'?'green':'red'}">${x.t==='รายรับ'?'+':'-'}${money(x.amount)}</b></div>`).join(''):'<div class="empty">ยังไม่มีรายการ</div>';
}
function renderAll(){renderSelects();renderLists();renderStock();renderDashboard()}
function exportData(){
 const blob=new Blob([JSON.stringify(db,null,2)],{type:'application/json'});
 downloadBlob(blob,'backup-grocery-'+today()+'.json');
}
function importData(e){
 const file=e.target.files[0];if(!file)return;
 const r=new FileReader();r.onload=()=>{try{const d=JSON.parse(r.result);if(!d.products||!d.income||!d.expense||!d.stockMoves)throw 0;if(confirm('กู้คืนข้อมูลจากไฟล์นี้? ข้อมูลปัจจุบันจะถูกแทนที่')){db=d;save();alert('กู้คืนข้อมูลเรียบร้อย')}}catch{alert('ไฟล์ไม่ถูกต้อง')}};r.readAsText(file);e.target.value='';
}
function csvCell(v){return '"'+String(v??'').replace(/"/g,'""')+'"'}
function exportCSV(){
 let rows=[['ประเภท','วันที่','รายละเอียด','หมวดหมู่','จำนวนเงิน']];
 db.income.forEach(x=>rows.push(['รายรับ',x.date,x.note,'',x.amount]));
 db.expense.forEach(x=>rows.push(['รายจ่าย',x.date,x.note,x.category,x.amount]));
 const csv='\ufeff'+rows.map(r=>r.map(csvCell).join(',')).join('\n');
 downloadBlob(new Blob([csv],{type:'text/csv;charset=utf-8'}),'บัญชีร้าน-'+today()+'.csv');
}
function downloadBlob(blob,name){
 const a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download=name;a.click();setTimeout(()=>URL.revokeObjectURL(a.href),1000);
}
function clearAll(){
 if(confirm('ต้องการล้างข้อมูลทั้งหมดจริงหรือไม่? แนะนำให้สำรองข้อมูลก่อน')){localStorage.removeItem(KEY);location.reload()}
}
setDates();renderAll();
</script>
</body>
</html>
