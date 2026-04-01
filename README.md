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
.card img{width:100%;height:110px;object-fit:cover;border-radius:8px;margin-bottom:6px;}

.btn{padding:6px 10px;background:#22c55e;color:#fff;border:none;border-radius:6px;cursor:pointer;margin-top:6px;}

.cartModal{position:fixed;top:0;left:0;width:100%;height:100%;background:#fff;display:none;flex-direction:column;z-index:9999;}
.cartHeader{display:flex;justify-content:space-between;padding:15px;background:#f5f5f5;}
.cartBody{padding:20px;flex:1;overflow:auto;}
.cartItem{display:flex;justify-content:space-between;padding:10px;border-bottom:1px solid #ddd;align-items:center;}
.checkoutBox{padding:15px;border-top:1px solid #ddd;}
.flag{width:24px;height:16px;object-fit:cover;border-radius:3px;margin-right:6px;vertical-align:middle;}
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

function add(name,price=99,img="https://placehold.co/300x300?text=Jersey"){
cart.push({name,price,img});saveCart();}

function removeItem(i){cart.splice(i,1);saveCart();renderCart();}

function calcTotal(){let sum=cart.reduce((a,b)=>a+b.price,0);return sum+(cart.length?SHIPPING:0);}

function openCart(){document.getElementById('cartModal').style.display='flex';renderCart();}
function closeCart(){document.getElementById('cartModal').style.display='none';}

function renderCart(){
let b=document.getElementById('cartBody');
if(cart.length===0){b.innerHTML="Leer";return;}

b.innerHTML=cart.map((c,i)=>`
<div class='cartItem'>
<img src='${c.img}' width='50' height='50' style='border-radius:6px'>
<div style='flex:1;margin-left:10px'>${c.name}<br>${c.price}€</div>
<button class='btn' onclick='removeItem(${i})'>X</button>
</div>`).join('');

document.getElementById('total').innerHTML="Gesamt: "+calcTotal().toFixed(2)+"€";
renderPayPal();
}

function renderPayPal(){
if(!cart.length)return;

document.getElementById('paypal-button-container').innerHTML="";
paypal.Buttons({
createOrder:(d,a)=>a.order.create({purchase_units:[{amount:{value:calcTotal().toFixed(2)}}]}),
onApprove:(d,a)=>a.order.capture().then(()=>{alert('Bestellung erfolgreich');cart=[];saveCart();renderCart();})
}).render('#paypal-button-container');
}

const countries={
de:"Deutschland",fr:"Frankreich",es:"Spanien",gb:"England",it:"Italien",pt:"Portugal",nl:"Niederlande",be:"Belgien",ch:"Schweiz",at:"Österreich",
br:"Brasilien",ar:"Argentinien",uy:"Uruguay",co:"Kolumbien",cl:"Chile",
ng:"Nigeria",ma:"Marokko",sn:"Senegal",gh:"Ghana",ci:"Elfenbeinküste",dz:"Algerien",eg:"Ägypten",
jp:"Japan",kr:"Südkorea",cn:"China",qa:"Qatar",sa:"Saudi-Arabien",ae:"UAE",
us:"USA",mx:"Mexiko",ca:"Kanada",
au:"Australien",nz:"Neuseeland"
};

const continents={
"Europa":["de","fr","es","gb","it","pt","nl","be","ch","at","dk","se","no","pl","cz","gr","ua"],
"Südamerika":["br","ar","uy","co","cl"],
"Afrika":["ng","ma","sn","gh","ci","dz","eg"],
"Asien":["jp","kr","cn","qa","sa","ae"],
"Nordamerika":["us","mx","ca"],
"Ozeanien":["au","nz"]
};

const leagues={
"Premier League|gb":["Manchester United","Manchester City","Liverpool","Chelsea","Arsenal","Tottenham","Newcastle","Aston Villa","West Ham","Brighton"],
"La Liga|es":["Real Madrid","Barcelona","Atletico Madrid","Sevilla","Valencia","Betis","Villarreal","Bilbao"],
"Bundesliga|de":["Bayern München","Dortmund","Leipzig","Leverkusen","Frankfurt","Stuttgart","Freiburg","Wolfsburg"],
"Serie A|it":["Juventus","Inter","AC Milan","Napoli","Roma","Lazio","Atalanta","Fiorentina"],
"Ligue 1|fr":["PSG","Marseille","Lyon","Monaco","Lille","Nice"],
"Eredivisie|nl":["Ajax","PSV","Feyenoord","AZ Alkmaar"],
"Liga Portugal|pt":["Benfica","Porto","Sporting","Braga"],
"Brasileirao|br":["Flamengo","Palmeiras","Sao Paulo","Corinthians"],
"Argentina|ar":["Boca Juniors","River Plate","Racing"],
"MLS|us":["Inter Miami","LA Galaxy","LAFC","Seattle Sounders"]
};

const retro=[
{name:"Deutschland 1990",price:120,img:"https://placehold.co/300x300?text=Germany+1990"},
{name:"Brasilien 2002",price:130,img:"https://placehold.co/300x300?text=Brazil+2002"}
];

function go(p){location.hash=p;render();}
window.onhashchange=render;
function back(){history.back();}
function openTeam(name){location.hash='team-'+encodeURIComponent(name);}

function search(q){q=q.toLowerCase();if(!q){render();return;}let results=[];Object.values(leagues).forEach(a=>a.forEach(t=>{if(t.toLowerCase().includes(q))results.push(t);}));const app=document.getElementById('app');app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Suche</div>`+results.map(r=>`<div class='card' onclick="openTeam('${r}')"><img src='https://placehold.co/300x300?text=Kit'/>${r}</div>`).join('');}

function render(){
updateCart();
const app=document.getElementById('app');
const h=location.hash.replace('#','')||'clubs';

if(h.startsWith('team-')){
const name=decodeURIComponent(h.replace('team-',''));
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span>${name}</div>
<div class='card'>
<img src='https://placehold.co/400x400?text=Official+Kit+${name}'/>
<h3>${name}</h3>
<p>Offizielles Fan Trikot</p>
<button class='btn' onclick="add('${name} Heim')">Heim</button>
<button class='btn' onclick="add('${name} Auswärts')">Auswärts</button>
</div>`;
return;
}

if(h==='national'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Nationalteams</div>`+
Object.entries(continents).map(([cont,codes])=>`
<div class='sub'>${cont}</div>
<div class='grid'>${codes.map(code=>`
<div class='card' onclick="openTeam('${countries[code]}')">
<img class='flag' src='https://flagcdn.com/w80/${code}.png'/>
<div>${countries[code]}</div>
</div>`).join('')}</div>`).join('');
}

else if(h==='clubs'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Vereine</div>
<div style='margin-bottom:10px'>
<input class='filter' placeholder='Filter Team...' oninput='filterTeams(this.value)'>
</div>`+
Object.entries(leagues).map(([l,teams])=>{
let [title,code]=l.split('|');
return `<div class='sub'>${title}</div>
<div class='grid'>${teams.map(t=>`
<div class='card' onclick="openTeam('${t}')">
<img src='https://placehold.co/300x300?text=Kit+${t}'/>
${t}
</div>`).join('')}</div>`;
}).join('');
}

else if(h==='retro'){
app.innerHTML=`<div class='title'><span class='back' onclick='back()'>⬅ Zurück</span> Retro</div>`+
retro.map(r=>`
<div class='card'><img src='${r.img}'/>${r.name}<br><button class='btn' onclick="add('${r.name}',${r.price})">Kaufen</button></div>`).join('');
}
}

function filterTeams(v){
v=v.toLowerCase();
document.querySelectorAll('.card').forEach(c=>{
c.style.display=c.innerText.toLowerCase().includes(v)?'block':'none';
});
}

render();
</script>

</body>
</html>
