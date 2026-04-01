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
nav{position:sticky;top:0;background:#f5f5f5;padding:15px;display:flex;justify-content:space-between;align-items:center;}
nav a{margin:0 10px;cursor:pointer;}
.search{padding:6px;border-radius:6px;border:1px solid #ccc;}
.cart-count{background:#22c55e;color:#fff;padding:2px 8px;border-radius:10px;margin-left:5px;font-size:12px;}
.container{padding:20px;}
.title{font-size:1.8rem;margin:20px 0;}
.sub{font-size:1.3rem;margin:15px 0;color:#555;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:10px;}
.card{background:#f1f1f1;padding:10px;border-radius:10px;cursor:pointer;text-align:center;}
.card img{width:100%;height:120px;object-fit:cover;border-radius:8px;}
.product{background:#fff;border:1px solid #ddd;padding:10px;border-radius:10px;text-align:center;}
.product img{width:100%;height:120px;object-fit:cover;border-radius:8px;}
.btn{padding:6px 10px;background:#22c55e;color:#fff;border:none;border-radius:6px;cursor:pointer;margin-top:6px;}
</style>
</head>
<body>

<nav>
<div><b>FootballJerseyONE</b></div>
<div>
<input class="search" placeholder="Suche..." oninput="search(this.value)" />
<a onclick="go('national')">National</a>
<a onclick="go('clubs')">Vereine</a>
<a onclick="go('retro')">Retro</a>
🛒<span class="cart-count" id="cartCount">0</span>
</div>
</nav>

<div class="container" id="app"></div>

<script>
let cart = JSON.parse(localStorage.getItem('cart')||'[]');
function save(){localStorage.setItem('cart',JSON.stringify(cart));updateCartCount();}
function updateCartCount(){document.getElementById('cartCount').innerText=cart.length;}

const productsDB={
"Real Madrid":[
{name:"Real Madrid Heim",price:99,img:"https://images.unsplash.com/photo-1606813907291-d86efa9b94db"},
{name:"Real Madrid Auswärts",price:95,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"}
],
"Barcelona":[
{name:"Barcelona Heim",price:97,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},
{name:"Barcelona Auswärts",price:93,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"}
],
"Bayern":[
{name:"Bayern Heim",price:98,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
{name:"Bayern Auswärts",price:94,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"}
]
};

function go(page){location.hash=page;render();}
window.onhashchange=render;

function openTeam(name){location.hash='team-'+name;}

function addProduct(p){cart.push(p);save();alert('Hinzugefügt');}

function render(){
updateCartCount();
const app=document.getElementById('app');
const hash=location.hash.replace('#','')||'clubs';

if(hash==='clubs'){
app.innerHTML=`<div class='title'>Teams</div><div class='grid'>`+
Object.keys(productsDB).map(t=>`<div class='card' onclick="openTeam('${t}')">${t}</div>`).join('')+`</div>`;
}

else if(hash.startsWith('team-')){
let team=hash.replace('team-','');
let items=productsDB[team]||[];
app.innerHTML=`<div class='title'>${team}</div><div class='grid'>`+
items.map(p=>`<div class='product'>
<img src='${p.img}' />
${p.name}<br>${p.price}€
<br><button class='btn' onclick='addProduct(${JSON.stringify(p)})'>Kaufen</button>
</div>`).join('')+`</div>`;
}
}

function search(q){
q=q.toLowerCase();
const app=document.getElementById('app');
let results=[];
Object.values(productsDB).forEach(arr=>{
arr.forEach(p=>{
if(p.name.toLowerCase().includes(q)) results.push(p);
});
});

app.innerHTML=`<div class='title'>Suche</div><div class='grid'>`+
results.map(p=>`<div class='product'>
<img src='${p.img}' />
${p.name}<br>${p.price}€
<br><button class='btn' onclick='addProduct(${JSON.stringify(p)})'>Kaufen</button>
</div>`).join('')+`</div>`;
}

render();
</script>

</body>
</html>
