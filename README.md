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

nav{
position:sticky;top:0;
background:#f5f5f5;
padding:12px 15px;
display:flex;
justify-content:space-between;
align-items:center;
flex-wrap:wrap;
gap:10px;
z-index:10;
}

nav a{margin:0 6px;cursor:pointer;font-size:14px;}
.search,.filter{padding:6px;border:1px solid #ccc;border-radius:6px;}

.container{padding:20px;}
.title{font-size:1.8rem;margin:15px 0;display:flex;align-items:center;gap:10px;flex-wrap:wrap;}

.back{
cursor:pointer;
padding:6px 12px;
border-radius:8px;
background:#111;
color:#fff;
font-size:13px;
transition:.2s;
}
.back:hover{opacity:0.8;transform:translateX(-2px);}

.sub{font-size:1.2rem;margin:10px 0;color:#555;display:flex;align-items:center;gap:8px;font-weight:600;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;}

.card{background:#f3f3f3;padding:10px;border-radius:10px;text-align:center;transition:.2s;cursor:pointer;}
.card:hover{transform:scale(1.03);}
.card img{width:100%;height:140px;object-fit:cover;border-radius:8px;margin-bottom:6px;}

.btn{padding:6px 10px;background:#22c55e;color:#fff;border:none;border-radius:6px;cursor:pointer;margin-top:6px;}

.flag{width:24px;height:16px;object-fit:cover;border-radius:3px;margin-right:6px;vertical-align:middle;}

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
<input class="search" placeholder="Suche alles..." oninput="searchAll(this.value)" />
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
let cart = JSON.parse(localStorage.getItem('cart') || '[]');
const SHIPPING = 4.99;

function saveCart(){localStorage.setItem('cart', JSON.stringify(cart));updateCart();}
function updateCart(){document.getElementById('cartCount').innerText = cart.length;}
function add(name,price=99,img="https://placehold.co/400x400?text=Kit"){cart.push({name,price,img});saveCart();}
function removeItem(i){cart.splice(i,1);saveCart();renderCart();}
function calcTotal(){let sum=cart.reduce((a,b)=>a+b.price,0);return sum+(cart.length?SHIPPING:0);}

function openCart(){document.getElementById('cartModal').style.display='flex';renderCart();}
function closeCart(){document.getElementById('cartModal').style.display='none';}

function renderCart(){
let b=document.getElementById('cartBody');
if(cart.length===0){b.innerHTML="Leer";return;}

b.innerHTML=cart.map((c,i)=>`
<div class='cartItem'>
<img src='${c.img}' width='50' height='50'>
<div style='flex:1;margin-left:10px'>${c.name}<br>${c.price}€</div>
<button class='btn' onclick='removeItem(${i})'>X</button>
</div>`).join('');

document.getElementById('total').innerHTML="Gesamt: "+calcTotal().toFixed(2)+"€";
renderPayPal();
}

function renderPayPal(){
if(!cart.length)return;
if(window.paypalRendered) return;
window.paypalRendered=true;

paypal.Buttons({
createOrder:(d,a)=>a.order.create({purchase_units:[{amount:{value:calcTotal().toFixed(2)}}]}),
onApprove:(d,a)=>a.order.capture().then(()=>{alert('Bestellung erfolgreich');cart=[];saveCart();renderCart();window.paypalRendered=false;})
}).render('#paypal-button-container');
}

// 🔥 REAL KIT API (Wikipedia fallback = echte Trikotbilder)
async function fetchKit(team){
try{
const res = await fetch(`https://en.wikipedia.org/api/rest_v1/page/media-list/${encodeURIComponent(team)}`);
const data = await res.json();
const file = data.items?.find(i=>i.original)?.original?.source;
return file || "https://placehold.co/400x400?text=Kit";
}catch(e){return "https://placehold.co/400x400?text=Kit";}
}

const countries={de:"Deutschland",fr:"Frankreich",es:"Spanien",gb:"England",it:"Italien",pt:"Portugal",nl:"Niederlande",be:"Belgien",ch:"Schweiz",at:"Österreich",br:"Brasilien",ar:"Argentinien",us:"USA",jp:"Japan"};

const leagues={
"Premier League":["Manchester United","Manchester City","Liverpool","Chelsea","Arsenal"],
"La Liga":["Real Madrid","Barcelona","Atletico Madrid"],
"Bundesliga":["Bayern München","Dortmund","Leipzig"],
"Serie A":["Juventus","Inter","AC Milan"],
"Ligue 1":["PSG","Marseille","Lyon"],
"MLS":["Inter Miami","LA Galaxy"]
};

const retro=["Deutschland 1990","Brasilien 2002"];

function go(p){location.hash=p;render();}
window.onhashchange=render;

function openTeam(name){location.hash='team-'+encodeURIComponent(name);}

// 🔥 GLOBAL SEARCH (alles)
function searchAll(q){
q=q.toLowerCase();if(!q){render();return;}
let results=[];

Object.values(leagues).forEach(a=>a.forEach(t=>{if(t.toLowerCase().includes(q))results.push({name:t,type:'club'});}));
Object.values(countries).forEach(c=>{if(c.toLowerCase().includes(q))results.push({name:c,type:'national'});});
retro.forEach(r=>{if(r.toLowerCase().includes(q))results.push({name:r,type:'retro'});});

const app=document.getElementById('app');
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Suche</div>`+
results.map(r=>`<div class='card' onclick="openTeam('${r.name}')">${r.name}<br><small>${r.type}</small></div>`).join('');
}

function back(){history.back();}

async function render(){
updateCart();
const app=document.getElementById('app');
const h=location.hash.replace('#','')||'clubs';

if(h.startsWith('team-')){
const name=decodeURIComponent(h.replace('team-',''));
const img=await fetchKit(name);
app.innerHTML=`
<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span>${name}</div>
<div class='card'>
<img src='${img}'/>
<h3>${name}</h3>
<p>2025/26 Fan Kit</p>
<button class='btn' onclick="add('${name} Heim')">Kaufen</button>
</div>`;
return;
}

if(h==='national'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Nationalteams</div>`+
Object.entries(countries).map(([c,n])=>`
<div class='card' onclick="openTeam('${n}')">
<img class='flag' src='https://flagcdn.com/w80/${c}.png'/>
${n}
</div>`).join('');
return;
}

if(h==='clubs'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Vereine</div>`+
Object.entries(leagues).map(([l,teams])=>`
<div class='sub'>${l}</div>
<div class='grid'>${teams.map(t=>`
<div class='card' onclick="openTeam('${t}')">${t}</div>`).join('')}</div>`).join('');
return;
}

if(h==='retro'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Retro</div>`+
retro.map(r=>`
<div class='card' onclick="openTeam('${r}')">${r}</div>`).join('');
}
}

render();
</script>

</body>
</html>
