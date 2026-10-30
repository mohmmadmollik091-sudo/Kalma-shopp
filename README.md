
<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>KOLOM Shopping</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#f2f6ff;color:#172b4d;
font-family:Arial,sans-serif}
header{background:#1264db;color:white;padding:20px;
border-radius:0 0 22px 22px}
.logo{font-size:27px;font-weight:bold}
header p{margin-bottom:0}
.search{width:100%;padding:14px;border:0;
border-radius:12px;margin-top:18px;font-size:16px}
h2{padding:0 15px}
.products{display:grid;grid-template-columns:1fr 1fr;
gap:12px;padding:12px}
.card{background:white;padding:12px;border-radius:15px;
box-shadow:0 3px 12px #dce5f5}
.pic{font-size:48px;text-align:center;
background:#edf4ff;padding:15px;border-radius:12px}
.price{color:#1264db;font-weight:bold}
button{border:0;background:#1264db;color:white;
padding:11px;border-radius:9px;font-size:14px;
cursor:pointer}
.card button{width:100%}
section{background:white;margin:15px;padding:15px;
border-radius:15px}
input,textarea{width:100%;padding:12px;margin:7px 0;
border:1px solid #ccd7e8;border-radius:9px;
font-size:15px}
.cartitem{display:flex;justify-content:space-between;
align-items:center;padding:9px 0;border-bottom:1px solid #eee}
footer{text-align:center;background:#1264db;
color:white;padding:20px;margin-top:20px}
</style>
</head>
<body>

<header>
<div class="logo">🍎 KOLOM</div>
<p>আপনার দৈনন্দিন কেনাকাটার বিশ্বস্ত ঠিকানা</p>
<input class="search" id="search"
placeholder="পণ্য খুঁজুন..." oninput="showProducts()">
</header>

<h2>🛒 আমাদের পণ্য</h2>
<div class="products" id="products"></div>

<section>
<h2>🛍️ আমার কার্ট</h2>
<div id="cart">এখনো কোনো পণ্য যোগ করা হয়নি</div>
<h3 id="total">মোট: ৳ ০</h3>
</section>

<section>
<h2>📦 অর্ডার করুন</h2>
<input id="name" placeholder="আপনার নাম">
<input id="phone" placeholder="মোবাইল নম্বর">
<textarea id="address" placeholder="সম্পূর্ণ ঠিকানা"></textarea>
<button style="width:100%" onclick="order()">
অর্ডার নিশ্চিত করুন
</button>
<p id="message"></p>
</section>

<footer>
🍎 KOLOM SHOPPING<br>
আপনার প্রতিদিনের প্রয়োজন, এক ঠিকানায়।
</footer>

<script>
const items=[
{name:"চাল",price:70,icon:"🍚"},
{name:"দুধ",price:90,icon:"🥛"},
{name:"আপেল",price:220,icon:"🍎"},
{name:"তেল",price:170,icon:"🫗"},
{name:"ডিম",price:150,icon:"🥚"},
{name:"আলু",price:40,icon:"🥔"},
{name:"পেঁয়াজ",price:60,icon:"🧅"},
{name:"বিস্কুট",price:30,icon:"🍪"},
{name:"সাবান",price:50,icon:"🧼"},
{name:"পানি",price:20,icon:"💧"}
];
let cart=[];

function showProducts(){
let q=document.getElementById("search").value;
document.getElementById("products").innerHTML=
items.filter(x=>x.name.includes(q)).map((x,i)=>`
<div class="card">
<div class="pic">${x.icon}</div>
<h3>${x.name}</h3>
<p class="price">৳ ${x.price}</p>
<button onclick="add('${x.name}')">কার্টে যোগ করুন</button>
</div>`).join("");
}

function add(name){
let p=cart.find(x=>x.name===name);
if(p)p.qty++;
else cart.push({...items.find(x=>x.name===name),qty:1});
showCart();
}

function change(name,n){
let p=cart.find(x=>x.name===name);
if(!p)return;
p.qty+=n;
cart=cart.filter(x=>x.qty>0);
showCart();
}

function showCart(){
let el=document.getElementById("cart");
if(cart.length===0){
el.innerHTML="এখনো কোনো পণ্য যোগ করা হয়নি";
}else{
el.innerHTML=cart.map(x=>`
<div class="cartitem">
<span>${x.icon} ${x.name}<br>
৳ ${x.price} × ${x.qty}</span>
<span>
<button onclick="change('${x.name}',-1)">−</button>
${x.qty}
<button onclick="change('${x.name}',1)">+</button>
</span>
</div>`).join("");
}
let total=cart.reduce((s,x)=>s+x.price*x.qty,0);
document.getElementById("total").innerText="মোট: ৳ "+total;
}

function order(){
let name=document.getElementById("name").value.trim();
let phone=document.getElementById("phone").value.trim();
let address=document.getElementById("address").value.trim();
if(!cart.length)return alert("আগে কার্টে পণ্য যোগ করুন");
if(!name||!phone||!address)
return alert("সব তথ্য পূরণ করুন");

let details=cart.map(x=>
x.name+" × "+x.qty+" = ৳"+(x.price*x.qty)
).join("\n");
let total=cart.reduce((s,x)=>s+x.price*x.qty,0);

let msg=`নতুন অর্ডার - KOLOM\nনাম: ${name}\nফোন: ${phone}\nঠিকানা: ${address}\n\n${details}\nমোট: ৳${total}`;

document.getElementById("message").innerText=
"অর্ডারের বিবরণ তৈরি হয়েছে। দোকানের সার্ভার সংযুক্ত না থাকায় অর্ডারটি এখনো পাঠানো হয়নি।";
alert(msg);
}

showProducts();
</script>
</body>
</html>
