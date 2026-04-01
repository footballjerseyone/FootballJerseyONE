<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FootballJerseyONE</title>

<script src="https://www.paypal.com/sdk/js?client-id=AVmx5KkGcuZr3ye1nLyNzvB5RhZXyrpnU8t71ZpFZyLyTO9Xt14jSULuXKjmPBNZw1kWigduGZTYXhii&currency=EUR"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:system-ui;}
body{background:#0a0f1c;color:#fff;}
nav{position:sticky;top:0;background:#0b1220;padding:15px;display:flex;justify-content:space-between;z-index:1000;}
nav a{color:#fff;margin:0 10px;cursor:pointer;text-decoration:none;}
header{padding:80px 20px;text-align:center;background:radial-gradient(circle,#18243f,#0a0f1c);} 
.logo{width:100px;height:100px;border-radius:50%;background:#22c55e;display:flex;align-items:center;justify-content:center;font-size:40px;margin:0 auto 15px;}
.btn{padding:10px 15px;background:#22c55e;border:none;border-radius:10px;color:white;cursor:pointer;}
.container{padding:20px;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:15px;}
.card{background:#111827;padding:10px;border-radius:12px;cursor:pointer;}
.card img{width:100%;height:160px;object-fit:cover;border-radius:10px;}
.price{margin-top:8px;}
.cart-btn{position:fixed;right:20px;bottom:20px;background:#22c55e;padding:14px;border-radius:50px;cursor:pointer;z-index:2000;}

.cart-overlay{position:fixed;top:0;left:0;width:100%;height:100%;background:#0a0f1c;z-index:3000;display:none;padding:30px;}
.cart-header{display:flex;justify-content:space-between;margin-bottom:20px;}
.close-btn{background:red;border:none;color:white;padding:8px 12px;border-radius:8px;cursor:pointer;}
.item{background:#111827;padding:12px;margin:10px 0;border-radius:10px;display:flex;justify-content:space-between;align-items:center;}
.qty{display:flex;gap:8px;align-items:center;}
.qty button{background:#22c55e;border:none;padding:5px 10px;color:white;border-radius:6px;cursor:pointer;}
.remove{background:red;border:none;padding:5px 10px;color:white;border-radius:6px;cursor:pointer;}
.summary{margin-top:15px;}
</style>
</head>
<body>

<nav>
<div><b>FootballJerseyONE</b></div>
<div>
<a onclick="go('shop')">Shop</a>
<a onclick="openCart()">Warenkorb</a>
</div>
</nav>

<header>
<div class="logo">⚽</div>
<h1>FootballJerseyONE</h1>
<p>Premium Jerseys</p>
<button class="btn" onclick="go('shop')">Shop starten</button>
</header>

<div class="container" id="app"></div>

<div class="cart-btn" onclick="openCart()">🛒</div>

<div class="cart-overlay" id="cartOverlay">
  <div class="cart-header">
    <h2>Warenkorb</h2>
    <button class="close-btn" onclick="closeCart()">X</button>
  </div>

  <div id="cartItems"></div>

  <div class="summary">
    <p id="subtotal"></p>
    <p>Versand: 4.99€</p>
    <h3 id="total"></h3>
  </div>

  <h3>Versanddaten</h3>
  <input placeholder="Name" />
  <input placeholder="Adresse" />
  <input placeholder="PLZ" />

  <div id="paypal" style="margin-top:20px"></div>
</div>

<script>
let cart = JSON.parse(localStorage.getItem('cart')||'[]');

const products=[
{name:"Deutschland Heim",price:89,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
{name:"Brasilien Heim",price:89,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},
{name:"Real Madrid",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"}
];

function save(){localStorage.setItem('cart',JSON.stringify(cart));}

function go(page){location.hash=page;render();}
window.onhashchange=render;

function render(){
const app=document.getElementById('app');
const hash=location.hash.replace('#','')||'shop';

if(hash==='shop'){
app.innerHTML=`<div class='grid'>`+
products.map((p,i)=>`
<div class='card' onclick='openProduct(${i})'>
<img src='${p.img}'/>
<h3>${p.name}</h3>
<div class='price'>${p.price}€</div>
</div>`).join('')+`</div>`;
}

else if(hash.startsWith('product')){
const id=hash.split('-')[1];
const p=products[id];
app.innerHTML=`
<h2>${p.name}</h2>
<img src='${p.img}' style='width:300px;border-radius:10px'/>
<h3>${p.price}€</h3>
<button class='btn' onclick='add(${id})'>In Warenkorb</button>
`;
}
}

function openProduct(i){location.hash='product-'+i;}

function add(i){
let item = cart.find(x=>x.name===products[i].name);
if(item){item.qty++} else {cart.push({...products[i],qty:1});}
save();alert('Hinzugefügt');
}

function openCart(){
document.getElementById('cartOverlay').style.display='block';
renderCart();
}

function closeCart(){document.getElementById('cartOverlay').style.display='none';}

function changeQty(name,delta){
let item = cart.find(x=>x.name===name);
if(!item)return;
item.qty += delta;
if(item.qty<=0) cart = cart.filter(x=>x.name!==name);
save();renderCart();
}

function renderCart(){
let el=document.getElementById('cartItems');
let subtotal=0;
el.innerHTML='';

cart.forEach(p=>{
subtotal += p.price * p.qty;
el.innerHTML+=`
<div class='item'>
  <div>
    ${p.name}<br>${p.price}€ x ${p.qty}
  </div>
  <div class='qty'>
    <button onclick="changeQty('${p.name}',1)">+</button>
    <button onclick="changeQty('${p.name}',-1)">-</button>
    <button class='remove' onclick="changeQty('${p.name}',-999)">X</button>
  </div>
</div>`;
});

let shipping = cart.length>0 ? 4.99 : 0;
let total = subtotal + shipping;

document.getElementById('subtotal').innerText='Zwischensumme: '+subtotal.toFixed(2)+'€';
document.getElementById('total').innerText='Gesamt: '+total.toFixed(2)+'€';

const pay=document.getElementById('paypal');
pay.innerHTML='';

if(total<=0)return;

paypal.Buttons({
createOrder:(d,a)=>a.order.create({purchase_units:[{amount:{value:total.toFixed(2)}}]}),
onApprove:(d,a)=>a.order.capture().then(()=>{
alert('Zahlung erfolgreich');
cart=[];save();renderCart();
})
}).render('#paypal');
}

render();
</script>

</body>
</html>
