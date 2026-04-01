<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FootballJerseyONE</title>

<script src="https://www.paypal.com/sdk/js?client-id=AVmx5KkGcuZr3ye1nLyNzvB5RhZXyrpnU8t71ZpFZyLyTO9Xt14jSULuXKjmPBNZw1kWigduGZTYXhii&currency=EUR"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:system-ui;}
body{background:#fff;color:#111;}
nav{position:sticky;top:0;background:#f5f5f5;padding:12px 15px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:10px;z-index:10;}
nav a{margin:0 6px;cursor:pointer;font-size:14px;}
.search,.filter{padding:6px;border:1px solid #ccc;border-radius:6px;}
.container{padding:20px;}
.title{font-size:1.8rem;margin:15px 0;display:flex;align-items:center;gap:10px;flex-wrap:wrap;}
.back{cursor:pointer;padding:6px 12px;border-radius:8px;background:#111;color:#fff;font-size:13px;}
.sub{font-size:1.2rem;margin:10px 0;color:#555;font-weight:600;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;}
.card{background:#f3f3f3;padding:10px;border-radius:10px;text-align:center;cursor:pointer;transition:.2s;}
.card:hover{transform:scale(1.03);}
.card img{width:100%;height:140px;object-fit:cover;border-radius:8px;margin-bottom:6px;}
.btn{padding:6px 10px;background:#22c55e;color:#fff;border:none;border-radius:6px;cursor:pointer;margin-top:6px;}
.flag{width:24px;height:16px;object-fit:cover;border-radius:3px;margin-right:6px;vertical-align:middle;}
select,input{padding:6px;border-radius:6px;border:1px solid #ccc;margin-top:5px;}
.cartModal{position:fixed;top:0;left:0;width:100%;height:100%;background:#fff;display:none;flex-direction:column;z-index:9999;}
.cartHeader{display:flex;justify-content:space-between;padding:15px;background:#f5f5f5;}
.cartBody{padding:20px;flex:1;overflow:auto;}
.cartItem{display:flex;justify-content:space-between;padding:10px;border-bottom:1px solid #ddd;align-items:center;}
.checkoutBox{padding:15px;border-top:1px solid #ddd;}
</style>
</head>
<body>

<nav>
<div><b>FootballJerseyONE</b></div>
<div>
<input class="search" placeholder="Suche Teams, Länder, Retro..." oninput="searchAll(this.value)" />
<a onclick="go('national')">National</a>
<a onclick="go('clubs')">Vereine</a>
<a onclick="go('retro')">Retro</a>
<span onclick="openCart()" style="cursor:pointer">🛒 <span id="cartCount">0</span></span>
</div>
</nav>

<div class="container" id="app"></div>

<div class="cartModal" id="cartModal">
<div class="cartHeader"><h2>Warenkorb</h2><span onclick="closeCart()" class="back">✖</span></div>
<div class="cartBody" id="cartBody"></div>
<div class="checkoutBox">
<div id="total"></div>
<div id="paypal-button-container"></div>
</div>
</div>

<script>
let cart=JSON.parse(localStorage.getItem('cart')||'[]');
const SHIPPING=4.99;

function save(){localStorage.setItem('cart',JSON.stringify(cart));document.getElementById('cartCount').innerText=cart.length;}
function add(n,p,img,size){cart.push({n,p,img,size});save();}
function remove(i){cart.splice(i,1);save();renderCart();}
function calc(){return cart.reduce((a,b)=>a+b.p,0)+(cart.length?SHIPPING:0);}
function openCart(){document.getElementById('cartModal').style.display='flex';renderCart();}
function closeCart(){document.getElementById('cartModal').style.display='none';}

function renderCart(){
let b=document.getElementById('cartBody');
if(!cart.length){b.innerHTML="Leer";return;}
b.innerHTML=cart.map((c,i)=>`
<div class='cartItem'>
<img src='${c.img}' width='50'>
<div style='flex:1;margin-left:10px'>${c.n}<br>Größe: ${c.size || "-"}<br>${c.p}€</div>
<button class='btn' onclick='remove(${i})'>X</button>
</div>`).join('');
document.getElementById('total').innerHTML="Gesamt: "+calc().toFixed(2)+"€";
renderPayPal();
}

function renderPayPal(){
if(!cart.length)return;
if(window.paypalRendered) return;
window.paypalRendered=true;
paypal.Buttons({
createOrder:(d,a)=>a.order.create({purchase_units:[{amount:{value:calc().toFixed(2)}}]}),
onApprove:(d,a)=>a.order.capture().then(()=>{alert('Bestellung erfolgreich');cart=[];save();renderCart();window.paypalRendered=false;})
}).render('#paypal-button-container');
}

// 🌍 DATA
const countries={de:"Deutschland",fr:"Frankreich",es:"Spanien",gb:"England",it:"Italien",pt:"Portugal",nl:"Niederlande",be:"Belgien",ch:"Schweiz",at:"Österreich"};
const leagues={"Premier League":["Manchester United","Manchester City","Liverpool"],"Bundesliga":["Bayern München","Dortmund","Leipzig"],"La Liga":["Real Madrid","Barcelona","Atletico Madrid"],"Serie A":["Juventus","Inter","AC Milan"],"Primeira Liga":["Benfica","Porto","Sporting CP"]};
const retro=["Deutschland 1990","Brasilien 2002"];

function go(p){location.hash=p;render();}
window.onhashchange=render;
function back(){history.back();}
function openTeam(n){location.hash='team-'+encodeURIComponent(n);}

// 🔍 IMPROVED IMAGE (NO MORE BAD FALLBACK)
async function getImg(team,type){
try{
let res=await fetch(`https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(team+' jersey')}`);
let d=await res.json();
if(d.thumbnail?.source) return d.thumbnail.source;
res=await fetch(`https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(team)}`);
d=await res.json();
return d.thumbnail?.source || "https://placehold.co/400x400?text=Football";
}catch(e){return "https://placehold.co/400x400?text=Football";}
}

function searchAll(q){q=q.toLowerCase();if(!q){render();return;}let r=[];
Object.values(leagues).flat().forEach(t=>{if(t.toLowerCase().includes(q))r.push({n:t,t:"Club"});});
Object.values(countries).forEach(c=>{if(c.toLowerCase().includes(q))r.push({n:c,t:"Nation"});});
retro.forEach(x=>{if(x.toLowerCase().includes(q))r.push({n:x,t:"Retro"});});
const app=document.getElementById('app');
app.innerHTML=r.map(x=>`<div class='card' onclick="openTeam('${x.n}')">${x.n}<br><small>${x.t}</small></div>`).join('');
}

// 🧠 TEAM PAGE WITH SIZE + PRICE
async function render(){
document.getElementById('cartCount').innerText=cart.length;
const app=document.getElementById('app');
const h=location.hash.replace('#','')||'clubs';

if(h.startsWith('team-')){
let name=decodeURIComponent(h.replace('team-',''));
let img=await getImg(name);
app.innerHTML=`
<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span>${name}</div>
<div class='card'>
<img src='${img}'/>
<h3>${name}</h3>
<p>Preis: 89.99€</p>
<select id='size'>
<option>S</option>
<option>M</option>
<option>L</option>
<option>XL</option>
</select><br>
<button class='btn' onclick="add('${name}',89.99,'${img}',document.getElementById('size').value)">In Warenkorb</button>
</div>`;
return;
}

if(h==='national'){
app.innerHTML=Object.entries(countries).map(([k,v])=>`
<div class='card' onclick="openTeam('${v}')">${v}</div>`).join('');
return;
}

if(h==='clubs'){
app.innerHTML=Object.entries(leagues).map(([l,t])=>`
<div class='sub'>${l}</div>
<div class='grid'>${t.map(x=>`<div class='card' onclick="openTeam('${x}')">${x}</div>`).join('')}</div>`).join('');
return;
}

if(h==='retro'){
app.innerHTML=retro.map(x=>`<div class='card' onclick="openTeam('${x}')">${x}</div>`).join('');
}
}

render();
</script>

</body>
</html>
