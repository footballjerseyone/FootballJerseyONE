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
nav{background:#fff;border-bottom:1px solid #e5e7eb;padding:12px 15px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;position:sticky;top:0;z-index:1000;}
nav a{color:#333;cursor:pointer}
nav a:hover{color:#22c55e}
.search{background:#f1f5f9;color:#111}
.container{padding:15px;max-width:1200px;margin:auto}
.title{font-size:2rem;margin:20px 0;display:flex;align-items:center;gap:10px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:12px}
.card{transition:.2s;cursor:pointer;background:#fff;padding:10px;border-radius:10px}
.card:hover{transform:scale(1.05);box-shadow:0 10px 25px rgba(0,0,0,.15)}
.card img{width:100%;height:180px;object-fit:contain}
.btn{width:100%;padding:12px;background:#111;border:none;border-radius:10px;color:#fff;font-weight:600;margin-top:10px;cursor:pointer}
input,select{width:100%;padding:10px;margin-top:8px;border-radius:8px;border:1px solid #ddd}
.sub{font-size:1.4rem;margin:20px 0 10px;font-weight:600}
</style>
</head>

<body>

<nav>
<div><b>⚽ FootballJerseyONE</b></div>
<div>
<input class="search" placeholder="Suche..." oninput="searchAll(this.value)">
<a onclick="go('clubs')">Vereine</a>
<span onclick="openCart()">🛒 <span id="cartCount">0</span></span>
</div>
</nav>

<div class="container" id="app"></div>

<div id="popup" style="position:fixed;bottom:20px;left:20px;background:#000;color:#fff;padding:10px;border-radius:10px;display:none"></div>

<script>

let cart=[];

function save(){
localStorage.setItem('cart',JSON.stringify(cart));
document.getElementById('cartCount').innerText=cart.length;
}

function add(n,p){
cart.push({n,p});
save();
showPopup("🛒 Hinzugefügt");
}

function showPopup(text){
const p=document.getElementById("popup");
p.innerText=text;
p.style.display="block";
setTimeout(()=>p.style.display="none",2000);
}

function go(p){location.hash=p;render();}
window.onhashchange=render;

function searchAll(q){
q=q.toLowerCase();
const teams=["Real Madrid","Barcelona","Bayern München","PSG"];
const app=document.getElementById('app');
app.innerHTML=teams.filter(t=>t.toLowerCase().includes(q)).map(t=>
`<div class='card' onclick="openTeam('${t}')">${t}</div>`
).join('');
}

function openTeam(n){
location.hash='team-'+n;
}

function render(){
const app=document.getElementById('app');
const h=location.hash.replace('#','')||'home';

if(h==='home'){
app.innerHTML=`
<h1>🔥 Bestseller</h1>
<div class='grid'>
<div class='card' onclick="openTeam('Real Madrid')">Real Madrid</div>
<div class='card' onclick="openTeam('Barcelona')">Barcelona</div>
</div>`;
return;
}

if(h.startsWith('team-')){
let name=h.replace('team-','');
app.innerHTML=`
<h2>${name}</h2>
<input id="qty" type="number" value="1">
<button class="btn" onclick="add('${name}',11.99)">Kaufen</button>
`;
}
}

render();

</script>

</body>
</html>
