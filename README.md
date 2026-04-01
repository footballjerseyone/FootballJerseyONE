<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FootballJerseyONE REAL MONEY</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Arial;}
body{background:#fff;color:#111;}
header{display:flex;justify-content:space-between;align-items:center;padding:14px 20px;border-bottom:1px solid #eee;position:sticky;top:0;background:#fff;z-index:10;}
.logo{font-weight:900;color:#111;display:flex;gap:10px;align-items:center;}
.logo-box{width:14px;height:14px;background:#16a34a;transform:rotate(45deg);}
nav a{margin:0 8px;text-decoration:none;color:#111;font-size:14px;}
nav a:hover{color:#16a34a;}
.btn{background:#16a34a;color:#fff;padding:8px 10px;border:none;border-radius:8px;cursor:pointer;font-weight:700;}
.hero{text-align:center;padding:45px;background:#f8fafc;}
.hero h1{font-size:38px;color:#16a34a;}
.hero p{color:#475569;}
.container{max-width:1100px;margin:auto;padding:20px;}
.tools{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:15px;}
input,select{padding:10px;border:1px solid #ddd;border-radius:8px;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:15px;}
.card{border:1px solid #eee;border-radius:12px;overflow:hidden;cursor:pointer;transition:0.2s;background:#fff;}
.card:hover{transform:translateY(-4px);}
.card img{width:100%;height:160px;object-fit:cover;}
.card-content{padding:10px;}
.price{color:#16a34a;font-weight:800;}
.modal{position:fixed;inset:0;background:rgba(0,0,0,0.6);display:none;justify-content:center;align-items:center;}
.modal-content{background:#fff;padding:20px;border-radius:12px;max-width:520px;width:92%;}
#paypal-button-container{margin-top:15px;}
</style>
</head>
<body>
<header>
<div class="logo"><div class="logo-box"></div>FootballJerseyONE</div>
<nav><a href="#">Home</a><a href="#">Shop</a></nav>
<button class="btn" onclick="openCart()">Warenkorb (<span id="cartCount">0</span>)</button>
</header>
<section class="hero">
<h1>REAL MONEY UPGRADE</h1>
<p>PayPal Live Payments • Orders System • GitHub Pages Ready</p>
</section>
<div class="container">
<div class="tools">
<input id="search" placeholder="Suche..." oninput="render()" />
<select id="filter" onchange="render()">
<option value="all">Alle</option>
<option value="national">National</option>
<option value="club">Vereine</option>
<option value="retro">Retro</option>
</select>
</div>
<div class="grid" id="grid"></div>
</div>

<div class="modal" id="cartModal">
<div class="modal-content">
<h2>Warenkorb</h2>
<div id="cartItems"></div>
<p id="shipping"></p>
<h3 id="total"></h3>

<div id="paypal-button-container"></div>

<button onclick="closeCart()" class="btn" style="margin-top:10px;">Schließen</button>
</div>
</div>

<script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=EUR"></script>

<script>
let products=[
{name:"Deutschland 2024",price:89.99,cat:"national"},
{name:"Frankreich Home",price:89.99,cat:"national"},
{name:"Real Madrid",price:94.99,cat:"club"},
{name:"Barcelona Retro",price:99.99,cat:"club"},
{name:"Brasil 2002",price:119.99,cat:"retro"}
];

let cart=JSON.parse(localStorage.getItem("cart"))||[];

function render(){
let grid=document.getElementById("grid");let s=document.getElementById("search").value.toLowerCase();let f=document.getElementById("filter").value;
grid.innerHTML="";
products.filter(p=>(f=="all"||p.cat==f)&&p.name.toLowerCase().includes(s)).forEach((p,i)=>{
let div=document.createElement("div");div.className="card";
div.innerHTML=`<img src='https://images.unsplash.com/photo-1521412644187-c49fa049e84d'><div class='card-content'><h3>${p.name}</h3><p class='price'>${p.price}€</p><button class='btn' onclick='addToCart(${i})'>In Warenkorb</button></div>`;
grid.appendChild(div);
});
}

function addToCart(i){
cart.push(products[i]);
localStorage.setItem("cart",JSON.stringify(cart));
document.getElementById("cartCount").innerText=cart.length;
}

function openCart(){
renderCart();
document.getElementById("cartModal").style.display="flex";
renderPayPal();
}

function closeCart(){
document.getElementById("cartModal").style.display="none";
}

function renderCart(){
let html="";let total=0;
cart.forEach(i=>{html+=`<p>${i.name} - ${i.price}€</p>`;total+=i.price;});
let shipping=cart.length?4.99:0;
document.getElementById("cartItems").innerHTML=html;
document.getElementById("shipping").innerText="Versand: "+shipping.toFixed(2)+"€";
document.getElementById("total").innerText="Gesamt: "+(total+shipping).toFixed(2)+"€";
}

function renderPayPal(){
let total=cart.reduce((a,b)=>a+b.price,0)+4.99;

document.getElementById("paypal-button-container").innerHTML="";

paypal.Buttons({
createOrder:function(data,actions){
return actions.order.create({
purchase_units:[{
amount:{value:total.toFixed(2)}
}]
});
},

onApprove:function(data,actions){
return actions.order.capture().then(function(details){
alert("Zahlung erfolgreich: "+details.payer.name.given_name);
localStorage.removeItem("cart");
cart=[];
document.getElementById("cartCount").innerText=0;
closeCart();
});
}
}).render('#paypal-button-container');
}

render();
document.getElementById("cartCount").innerText=cart.length;
</script>
</body>
</html>
