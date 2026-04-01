<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FootballJerseyONE</title>

<!-- PayPal -->
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
}

nav a{margin:0 6px;cursor:pointer;font-size:14px;}
.search{padding:6px;border:1px solid #ccc;border-radius:6px;}

.container{padding:20px;}
.title{font-size:1.8rem;margin:15px 0;display:flex;align-items:center;gap:10px;}
.back{cursor:pointer;font-size:14px;background:#eee;padding:5px 10px;border-radius:6px;}
.sub{font-size:1.2rem;margin:10px 0;color:#555;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;}

.card{background:#f3f3f3;padding:10px;border-radius:10px;cursor:pointer;text-align:center;transition:.2s;}
.card:hover{transform:scale(1.02);}
.card img{width:100%;height:110px;object-fit:cover;border-radius:8px;margin-bottom:6px;}

.product{border:1px solid #ddd;border-radius:10px;padding:10px;text-align:center;background:#fff;}
.product img{width:100%;height:120px;object-fit:cover;border-radius:8px;}

.btn{padding:6px 10px;background:#22c55e;color:#fff;border:none;border-radius:6px;cursor:pointer;margin-top:6px;}

.badge{background:#22c55e;color:#fff;padding:2px 8px;border-radius:10px;font-size:12px;}

/* CART + CHECKOUT */
.cartModal{
position:fixed;top:0;left:0;width:100%;height:100%;background:#fff;
display:none;flex-direction:column;z-index:9999;
}
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
<div class="checkoutBox">
<div id="total"></div>
<div id="paypal-button-container"></div>
</div>
</div>

<script>
let cart = JSON.parse(localStorage.getItem('cart') || '[]');
const SHIPPING = 4.99;

function saveCart(){
localStorage.setItem('cart', JSON.stringify(cart));
updateCart();
}

function updateCart(){
document.getElementById('cartCount').innerText = cart.length;
}

function add(name,price=99,img="https://images.unsplash.com/photo-1521412644187-c49fa049e84d"){
cart.push({name,price,img});
saveCart();
}

function removeItem(i){
cart.splice(i,1);
saveCart();
renderCart();
}

function calcTotal(){
let sum = cart.reduce((a,b)=>a+b.price,0);
return sum + (cart.length>0 ? SHIPPING : 0);
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
if(cart.length===0){b.innerHTML="Leer";return;}

b.innerHTML=cart.map((c,i)=>`
<div class='cartItem'>
<img src='${c.img}' width='50' height='50' style='border-radius:6px;object-fit:cover'>
<div style='flex:1;margin-left:10px'>${c.name}<br>${c.price}€</div>
<button class='btn' onclick='removeItem(${i})'>X</button>
</div>`).join('');

document.getElementById('total').innerHTML=
"Zwischensumme + Versand: " + calcTotal().toFixed(2) + "€";

renderPayPal();
}

function renderPayPal(){
document.getElementById('paypal-button-container').innerHTML="";
if(cart.length===0)return;

paypal.Buttons({
createOrder: function(data, actions){
return actions.order.create({
purchase_units: [{ amount: { value: calcTotal().toFixed(2) } }]
});
},
onApprove: function(data, actions){
return actions.order.capture().then(function(){
alert("Bestellung erfolgreich! 🎉");
cart=[];
saveCart();
renderCart();
});
}
}).render('#paypal-button-container');
}

// ---------------- DATA ----------------
const national={
"Europa":["Deutschland","Frankreich","Spanien","England","Italien","Portugal","Niederlande","Belgien","Schweiz","Österreich"],
"Südamerika":["Brasilien","Argentinien","Uruguay","Kolumbien","Chile","Peru"],
"Afrika":["Nigeria","Marokko","Senegal","Ghana","Ägypten","Algerien","Tunesien","Kamerun"],
"Asien":["Japan","Südkorea","Saudi Arabien","Iran","China","Qatar","Australien"],
"Nordamerika":["USA","Mexiko","Kanada","Costa Rica","Jamaika"],
"Ozeanien":["Australien","Neuseeland"]
};

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

const retro=[
{name:"Deutschland 1990",price:120,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
{name:"Brasilien 2002",price:130,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},
{name:"Barcelona 2009",price:140,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"}
];

// ---------------- NAV ----------------
function go(p){location.hash=p;render();}
window.onhashchange=render;

function back(){history.back();}

function openLeague(l){location.hash='league-'+l;}

function render(){
updateCart();
const app=document.getElementById('app');
const h=location.hash.replace('#','')||'clubs';

// NATIONAL
if(h==='national'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Nationalteams</div>`+
Object.keys(national).map(c=>`
<div class='sub'>${c}</div>
<div class='grid'>${national[c].map(n=>`
<div class='card'>
<img src='https://images.unsplash.com/photo-1521412644187-c49fa049e84d'/>
${n}<br>
<button class='btn' onclick="add('${n} Heim')">Heim</button>
<button class='btn' onclick="add('${n} Auswärts')">Auswärts</button>
</div>`).join('')}</div>`).join('');
}

// CLUBS
else if(h==='clubs'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Ligen</div>
<div class='grid'>`+
Object.keys(leagues).map(l=>`
<div class='card' onclick="openLeague('${l}')">
<img src='https://images.unsplash.com/photo-1508098682722-e99c43a406b2'/>
${l}
</div>`).join('')+'</div>';
}

// LEAGUE
else if(h.startsWith('league-')){
let l=h.replace('league-','');
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> ${l}</div><div class='grid'>`+
(leagues[l]||[]).map(t=>`
<div class='card'>
<img src='https://images.unsplash.com/photo-1521412644187-c49fa049e84d'/>
${t}<br>
<button class='btn' onclick="add('${t} Heim')">Heim</button>
<button class='btn' onclick="add('${t} Auswärts')">Auswärts</button>
</div>`).join('')+'</div>';
}

// RETRO
else if(h==='retro'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Retro</div><div class='grid'>`+
retro.map(r=>`
<div class='card'>
<img src='https://images.unsplash.com/photo-1518091043644-c1d4457512c6'/>
${r.name}<br>
<button class='btn' onclick="add('${r.name}',${r.price})">Kaufen</button>
</div>`).join('')+'</div>';
}
}

render();
</script>

</body>
</html>
