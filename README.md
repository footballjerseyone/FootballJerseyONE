<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FootballJerseyONE</title>

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
}
nav a{margin:0 6px;cursor:pointer;font-size:14px;}
.search{padding:6px;border:1px solid #ccc;border-radius:6px;}
.container{padding:20px;}
.title{font-size:1.8rem;margin:15px 0;}
.sub{font-size:1.2rem;margin:10px 0;color:#555;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;}
.card{background:#f3f3f3;padding:10px;border-radius:10px;cursor:pointer;text-align:center;transition:.2s;}
.card:hover{transform:scale(1.02);}
.product{border:1px solid #ddd;border-radius:10px;padding:10px;text-align:center;background:#fff;}
.btn{padding:6px 10px;background:#22c55e;color:#fff;border:none;border-radius:6px;cursor:pointer;margin-top:6px;}

.cartModal{
position:fixed;top:0;left:0;width:100%;height:100%;background:#fff;
display:none;flex-direction:column;z-index:9999;
}
.cartHeader{display:flex;justify-content:space-between;padding:15px;background:#f5f5f5;}
.cartBody{padding:20px;flex:1;overflow:auto;}
.cartItem{display:flex;justify-content:space-between;padding:10px;border-bottom:1px solid #ddd;}
.badge{background:#22c55e;color:#fff;padding:2px 8px;border-radius:10px;font-size:12px;}
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
<span onclick="openCart()" style="cursor:pointer">🛒 <span class="badge" id="cartCount">0</span></span>
</div>
</nav>

<div class="container" id="app"></div>

<!-- CART -->
<div class="cartModal" id="cartModal">
<div class="cartHeader">
<h2>Warenkorb</h2>
<span onclick="closeCart()" style="cursor:pointer">✖</span>
</div>
<div class="cartBody" id="cartBody"></div>
</div>

<script>
// ---------------- CART (GitHub-safe persistent) ----------------
let cart = JSON.parse(localStorage.getItem('cart') || '[]');

function saveCart(){
localStorage.setItem('cart', JSON.stringify(cart));
updateCart();
}

function updateCart(){
document.getElementById('cartCount').innerText = cart.length;
}

function add(name){
cart.push({name});
saveCart();
}

function removeItem(i){
cart.splice(i,1);
saveCart();
renderCart();
}

function openCart(){
document.getElementById('cartModal').style.display='flex';
renderCart();
}

function closeCart(){
document.getElementById('cartModal').style.display='none';
}

function renderCart(){
let b=document.getElementById('cartBody');
if(cart.length===0){b.innerHTML="🛒 Leer";return;}
b.innerHTML=cart.map((c,i)=>`
<div class='cartItem'>
<div>${c.name}</div>
<button class='btn' onclick='removeItem(${i})'>X</button>
</div>`).join('');
}

// ---------------- DATA (expanded GitHub safe) ----------------
const national={
"Europa":["Deutschland","Frankreich","Spanien","England","Italien","Portugal","Niederlande","Belgien","Schweiz","Österreich"],
"Südamerika":["Brasilien","Argentinien","Uruguay","Kolumbien","Chile","Peru"],
"Afrika":["Nigeria","Marokko","Senegal","Ghana","Ägypten","Algerien","Tunesien","Kamerun"],
"Asien":["Japan","Südkorea","Saudi Arabien","Iran","China","Qatar","Australien"],
"Nordamerika":["USA","Mexiko","Kanada","Costa Rica","Jamaika"],
"Ozeanien":["Australien","Neuseeland"]
};

// 10 leagues + all clubs
const leagues={
"Premier League":["Manchester United","Manchester City","Liverpool","Chelsea","Arsenal","Tottenham","Newcastle","Aston Villa","West Ham","Brighton"],
"La Liga":["Real Madrid","Barcelona","Atletico Madrid","Sevilla","Valencia","Real Sociedad","Villarreal","Athletic Bilbao","Betis","Girona"],
"Bundesliga":["Bayern München","Dortmund","Leipzig","Leverkusen","Frankfurt","Stuttgart","Freiburg","Wolfsburg","Gladbach","Hoffenheim"],
"Serie A":["Juventus","Inter","AC Milan","Napoli","Roma","Lazio","Atalanta","Fiorentina","Bologna","Torino"],
"Ligue 1":["PSG","Marseille","Lyon","Monaco","Lille","Nice","Rennes","Lens","Nantes","Toulouse"],
"Eredivisie":["Ajax","PSV","Feyenoord","AZ Alkmaar","Utrecht","Twente","Sparta","Heerenveen","NEC","Willem II"],
"Liga Portugal":["Benfica","Porto","Sporting","Braga","Guimaraes","Boavista","Rio Ave","Famalicao","Casa Pia","Arouca"],
"Brasileirao":["Flamengo","Palmeiras","Sao Paulo","Corinthians","Fluminense","Botafogo","Gremio","Internacional","Atletico Mineiro","Santos"],
"Argentina":["Boca Juniors","River Plate","Racing","Independiente","San Lorenzo","Velez","Estudiantes","Newells","Rosario Central","Huracan"],
"MLS":["Inter Miami","LA Galaxy","LAFC","New York City","Seattle Sounders","Atlanta United","Chicago Fire","Toronto FC","Columbus Crew","Orlando City"]
};

const retro=["Deutschland 1990","Brasilien 2002","Frankreich 1998","Barcelona 2009","Inter 2010"];

// ---------------- NAV (GitHub-safe routing) ----------------
function go(page){
location.hash = encodeURIComponent(page);
render();
}

window.onhashchange=render;

function openLeague(l){
location.hash = 'league-' + encodeURIComponent(l);
}

function openTeam(t){
location.hash = 'team-' + encodeURIComponent(t);
}

// ---------------- RENDER ----------------
function render(){
updateCart();
const app=document.getElementById('app');
const raw = decodeURIComponent(location.hash.replace('#','')) || 'clubs';

// NATIONAL
if(raw==='national'){
app.innerHTML=`<div class='title'>Nationalteams</div>`+
Object.keys(national).map(c=>`
<div class='sub'>${c}</div>
<div class='grid'>${national[c].map(n=>`
<div class='card'>${n}<br>
<button class='btn' onclick="add('${n} Heim')">Heim</button>
<button class='btn' onclick="add('${n} Auswärts')">Auswärts</button>
</div>`).join('')}</div>`).join('');
}

// CLUBS
else if(raw==='clubs'){
app.innerHTML=`<div class='title'>Top 10 Ligen</div>
<div class='grid'>`+
Object.keys(leagues).map(l=>`
<div class='card' onclick="openLeague('${l}')">${l}</div>`).join('')+`</div>`;
}

// LEAGUE
else if(raw.startsWith('league-')){
let l=raw.replace('league-','');
app.innerHTML=`<div class='title'>${l}</div><div class='grid'>`+
(leagues[l]||[]).map(t=>`
<div class='card'>${t}<br>
<button class='btn' onclick="add('${t} Heim')">Heim</button>
<button class='btn' onclick="add('${t} Auswärts')">Auswärts</button>
</div>`).join('')+`</div>`;
}

// RETRO
else if(raw==='retro'){
app.innerHTML=`<div class='title'>Retro</div><div class='grid'>`+
retro.map(r=>`
<div class='card'>${r}<br>
<button class='btn' onclick="add('${r}')">Kaufen</button>
</div>`).join('')+`</div>`;
}
}

render();
</script>

</body>
</html>
