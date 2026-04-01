<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FootballJerseyONE</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:system-ui;}
body{background:#fff;color:#111;}
nav{position:sticky;top:0;background:#f5f5f5;padding:15px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;}
nav a{margin:0 8px;cursor:pointer;font-size:14px;}
.search{padding:6px;border:1px solid #ccc;border-radius:6px;}
.container{padding:20px;}
.title{font-size:1.8rem;margin:15px 0;}
.sub{font-size:1.2rem;margin:10px 0;color:#555;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;}
.card{background:#f3f3f3;padding:10px;border-radius:10px;cursor:pointer;text-align:center;}
.card img{width:100%;height:120px;object-fit:cover;border-radius:8px;}
.product{border:1px solid #ddd;border-radius:10px;padding:10px;text-align:center;background:#fff;}
.product img{width:100%;height:120px;object-fit:cover;border-radius:8px;}
.btn{padding:6px 10px;background:#22c55e;color:#fff;border:none;border-radius:6px;cursor:pointer;margin-top:6px;}
.cart{font-weight:bold;}
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
<span class="cart">🛒 <span id="cartCount">0</span></span>
</div>
</nav>

<div class="container" id="app"></div>

<script>
let cart = JSON.parse(localStorage.getItem('cart')||'[]');
function save(){localStorage.setItem('cart',JSON.stringify(cart));updateCart();}
function updateCart(){document.getElementById('cartCount').innerText=cart.length;}

// ---------------- DATA ----------------
const national = {
"Europa":["Deutschland","Frankreich","Spanien","England","Italien","Portugal","Niederlande"],
"Südamerika":["Brasilien","Argentinien"]
};

const retro=[
{name:"Deutschland 1990",price:120,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
{name:"Brasilien 2002",price:130,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},
{name:"Barcelona 2009 Retro",price:140,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"}
];

const leagues={
"Premier League":["Manchester United","Manchester City","Liverpool","Chelsea","Arsenal","Tottenham","Newcastle","Aston Villa","West Ham","Brighton"],
"La Liga":["Real Madrid","Barcelona","Atletico Madrid","Sevilla","Valencia","Real Sociedad","Villarreal","Athletic Bilbao","Betis","Girona"],
"Bundesliga":["Bayern München","Dortmund","Leipzig","Leverkusen","Frankfurt","Stuttgart","Freiburg","Wolfsburg","Gladbach","Hoffenheim"],
"Serie A":["Juventus","Inter","AC Milan","Napoli","Roma","Lazio","Atalanta","Fiorentina","Bologna","Torino"],
"Ligue 1":["PSG","Marseille","Lyon","Monaco","Lille","Nice","Rennes","Lens","Nantes","Toulouse"]
};

// ---------------- NAV ----------------
function go(p){location.hash=p;render();}
window.onhashchange=render;

function openLeague(l){location.hash='league-'+l;}
function openTeam(t){location.hash='team-'+t;}

function add(name){
cart.push({name:name,price:99,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"});
save();alert("Hinzugefügt");
}

// ---------------- RENDER ----------------
function render(){
updateCart();
const app=document.getElementById('app');
const h=location.hash.replace('#','')||'clubs';

// NATIONAL
if(h==='national'){
app.innerHTML=`<div class='title'>Nationalmannschaften</div>`+
Object.keys(national).map(c=>
`<div class='sub'>${c}</div><div class='grid'>`+
national[c].map(n=>`<div class='card' onclick="add('${n} Nationaltrikot')">${n}</div>`).join('')+`</div>`
).join('');
}

// CLUBS
else if(h==='clubs'){
app.innerHTML=`<div class='title'>Ligen</div><div class='grid'>`+
Object.keys(leagues).map(l=>`<div class='card' onclick="openLeague('${l}')">${l}</div>`).join('')+`</div>`;
}

// LEAGUE
else if(h.startsWith('league-')){
let l=h.replace('league-','');
let teams=leagues[l]||[];
app.innerHTML=`<div class='title'>${l}</div><div class='grid'>`+
teams.map(t=>`<div class='card' onclick="openTeam('${t}')">${t}</div>`).join('')+`</div>`;
}

// TEAM
else if(h.startsWith('team-')){
let t=h.replace('team-','');
app.innerHTML=`<div class='title'>${t}</div><button class='btn' onclick="add('${t} Trikot')">In Warenkorb</button>`;
}

// RETRO
else if(h==='retro'){
app.innerHTML=`<div class='title'>Retro Trikots</div><div class='grid'>`+
retro.map(p=>`
<div class='product'>
<img src='${p.img}'/>
${p.name}<br>${p.price}€<br>
<button class='btn' onclick="add('${p.name}')">Kaufen</button>
</div>`).join('')+`</div>`;
}
}

// SEARCH
function search(q){
q=q.toLowerCase();
let res=[];
Object.values(leagues).forEach(arr=>arr.forEach(t=>{if(t.toLowerCase().includes(q))res.push(t)}));
Object.values(national).forEach(arr=>arr.forEach(t=>{if(t.toLowerCase().includes(q))res.push(t)}));

const app=document.getElementById('app');
app.innerHTML=`<div class='title'>Suche</div><div class='grid'>`+
res.map(r=>`<div class='card'>${r}</div>`).join('')+`</div>`;
}

render();
</script>

</body>
</html>
