
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Expense Tracker</title>
<style>
*{box-sizing:border-box}
body{margin:0;padding:25px;background:#101522;color:#f3f5ff;font-family:Arial}
main{max-width:800px;margin:auto}
.summary{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:12px}
.box,form,.history{background:#1a2234;border:1px solid #303b52;border-radius:14px;padding:18px;margin:14px 0}
input,select{padding:12px;width:100%;margin:7px 0 12px;background:#0e1524;color:white;border:1px solid #3b4964;border-radius:8px}
button{padding:11px 15px;border:0;border-radius:8px;background:#40e0b0;cursor:pointer;font-weight:bold}
.entry{display:flex;justify-content:space-between;align-items:center;gap:10px;padding:12px 0;border-bottom:1px solid #303b52;flex-wrap:wrap}
.income{color:#5ff0bc}.expense{color:#ff8494}
</style>
</head>
<body>
<main>
<h1>◈ Money Tracker</h1>
<p>Manage your money and understand your spending.</p>
<section class="summary">
<div class="box">Income<h2 id="income">₹0</h2></div>
<div class="box">Expenses<h2 id="expenses">₹0</h2></div>
<div class="box">Balance<h2 id="balance">₹0</h2></div>
</section>
<form id="form">
<h2>Add Transaction</h2>
<input id="title" placeholder="Description" required>
<input id="amount" type="number" min="0.01" step="0.01" placeholder="Amount ₹" required>
<select id="type"><option value="expense">Expense</option><option value="income">Income</option></select>
<button>Add Transaction</button>
</form>
<section class="history"><h2>History</h2><div id="list"></div></section>
</main>
<script>
const $=id=>document.getElementById(id);
let data=JSON.parse(localStorage.moneyItems||"[]");
const money=n=>"₹"+n.toLocaleString("en-IN",{maximumFractionDigits:2});
function esc(s){return String(s).replace(/[&<>"]/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]))}
function draw(){
let inc=data.filter(x=>x.type==="income").reduce((a,x)=>a+x.amount,0);
let exp=data.filter(x=>x.type==="expense").reduce((a,x)=>a+x.amount,0);
$("income").textContent=money(inc);
$("expenses").textContent=money(exp);
$("balance").textContent=money(inc-exp);
$("list").innerHTML=data.map((x,i)=>`
<div class="entry"><span>${esc(x.title)}<br><small>${x.type}</small></span>
<b class="${x.type}">${x.type==="income"?"+":"−"}${money(x.amount)}</b>
<button onclick="del(${i})">Delete</button></div>`).join("")||"No transactions yet.";
localStorage.moneyItems=JSON.stringify(data);
}
function del(i){data.splice(i,1);draw()}
$("form").onsubmit=e=>{
e.preventDefault();
data.unshift({title:$("title").value,amount:Number($("amount").value),type:$("type").value});
$("title").value="";$("amount").value="";draw();
};
draw();
</script>
</body>
</html>
# Expanse-Tracker
