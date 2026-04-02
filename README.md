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
.search{padding:6px;border:1px solid #ccc;border-radius:6px;}
.container{padding:20px;}
.title{font-size:1.8rem;margin:15px 0;display:flex;align-items:center;gap:10px;flex-wrap:wrap;}
.back{cursor:pointer;padding:6px 12px;border-radius:8px;background:#111;color:#fff;font-size:13px;}
.sub{font-size:1.2rem;margin:10px 0;color:#555;font-weight:600;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:12px;}
.card{background:#f3f3f3;padding:10px;border-radius:10px;text-align:center;cursor:pointer;transition:.2s;}
.card:hover{transform:scale(1.03);}
.card img{
  width:100%;
  height:160px;
  object-fit:contain;
  background:#f9f9f9;
  padding:5px;
}
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
<div style="display:flex;align-items:center;gap:8px;"><span>⚽</span><b>FootballJerseyONE</b></div>
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

<input id="cust-name" placeholder="Dein Name" style="width:100%;margin:5px 0;">
<input id="cust-email" placeholder="E-Mail" style="width:100%;margin:5px 0;">
<input id="cust-address" placeholder="Adresse" style="width:100%;margin:5px 0;">

<div id="paypal-button-container"></div>
</div>
</div>

<script>
let cart=JSON.parse(localStorage.getItem('cart')||'[]');
const SHIPPING=4.99;

function save(){localStorage.setItem('cart',JSON.stringify(cart));document.getElementById('cartCount').innerText=cart.length;}
function add(n,p,img,size,qty,player,number){
cart.push({n,p,img,size,qty,player,number});
save();
}
function remove(i){cart.splice(i,1);save();renderCart();}
function calc(){
return cart.reduce((a,b)=>a+(b.p * b.qty),0)+(cart.length?SHIPPING:0);
}
let paypalLoaded = false;

function openCart(){
document.getElementById('cartModal').style.display='flex';
renderCart();

if(!paypalLoaded){
paypalLoaded = true;

paypal.Buttons({
  createOrder: function(data, actions) {
    return actions.order.create({
      purchase_units: [{
        amount: {
          value: calc().toFixed(2)
        }
      }]
    });
  },

  onApprove: function(data, actions) {
    return actions.order.capture().then(function(details) {
     let customer = {
  name: document.getElementById('cust-name').value,
  email: document.getElementById('cust-email').value,
  address: document.getElementById('cust-address').value
};

console.log("BESTELLUNG:", {
  customer: customer,
  cart: cart,
  total: calc()
});

alert("Danke für deine Bestellung, " + customer.name + "!");
      cart = [];
      save();
      renderCart();
      closeCart();
    });
  }

}).render('#paypal-button-container');

}
}  
function closeCart(){document.getElementById('cartModal').style.display='none';}

function renderCart(){
let b=document.getElementById('cartBody');
if(!cart.length){b.innerHTML="Leer";return;}
b.innerHTML=cart.map((c,i)=>`
<div class='cartItem'>
<img src='${c.img}' width='50'>
<div style='flex:1;margin-left:10px'>${c.n}<br>
Größe: ${c.size}<br>
Menge: ${c.qty}<br>
${c.player ? 'Name: ' + c.player + '<br>' : ''}
${c.number ? 'Nr: ' + c.number + '<br>' : ''}
${(c.p * c.qty).toFixed(2)}€</div>
<button class='btn' onclick='remove(${i})'>X</button>
</div>`).join('');

let subtotal = cart.reduce((a,b)=>a+(b.p * b.qty),0);
let shipping = cart.length ? SHIPPING : 0;
let total = subtotal + shipping;

document.getElementById('total').innerHTML = `
Zwischensumme: ${subtotal.toFixed(2)}€ <br>
Versand: ${shipping.toFixed(2)}€ <br>
<b>Gesamt: ${total.toFixed(2)}€</b>
`;

}

// 🌍 COUNTRIES EXPANDED
const countries={
de:"Deutschland",fr:"Frankreich",es:"Spanien",gb:"England",it:"Italien",pt:"Portugal",nl:"Niederlande",be:"Belgien",ch:"Schweiz",at:"Österreich",
dk:"Dänemark",se:"Schweden",no:"Norwegen",pl:"Polen",cz:"Tschechien",hr:"Kroatien",rs:"Serbien",gr:"Griechenland",
br:"Brasilien",ar:"Argentinien",uy:"Uruguay",co:"Kolumbien",cl:"Chile",pe:"Peru",
ng:"Nigeria",ma:"Marokko",sn:"Senegal",gh:"Ghana",dz:"Algerien",cm:"Kamerun",
jp:"Japan",kr:"Südkorea",cn:"China",sa:"Saudi Arabien",ae:"UAE",qa:"Qatar",
us:"USA",mx:"Mexiko",ca:"Kanada",
au:"Australien",nz:"Neuseeland"
};

// 🌍 CONTINENTS ADDED
const continents={
"Europa":["de","fr","es","gb","it","pt","nl","be","ch","at","dk","se","no","pl","cz","hr","rs","gr"],
"Südamerika":["br","ar","uy","co","cl","pe"],
"Afrika":["ng","ma","sn","gh","dz","cm"],
"Asien":["jp","kr","cn","sa","ae","qa"],
"Nordamerika":["us","mx","ca"],
"Ozeanien":["au","nz"]
};

// 🏟 CLUBS EXPANDED
const leagues={
"Premier League":["Manchester United","Manchester City","Liverpool","Chelsea","Arsenal","Tottenham","Newcastle","Aston Villa","West Ham","Brighton"],
"La Liga":["Real Madrid","Barcelona","Atletico Madrid","Sevilla","Valencia","Betis","Villarreal","Real Sociedad","Athletic Bilbao","Girona"],
"Bundesliga":["Bayern München","Dortmund","Leipzig","Leverkusen","Frankfurt","Stuttgart","Freiburg","Wolfsburg","Gladbach","Hoffenheim"],
"Serie A":["Juventus","Inter","AC Milan","Napoli","Roma","Lazio","Atalanta","Fiorentina","Torino","Bologna"],
"Ligue 1":["PSG","Marseille","Lyon","Monaco","Lille","Nice","Rennes","Lens"],
"Primeira Liga":["Benfica","Porto","Sporting CP","Braga","Boavista","Guimaraes","Famalicao","Rio Ave"],
"Eredivisie":["Ajax","PSV","Feyenoord","AZ Alkmaar","Twente","Utrecht"],
"MLS":["Inter Miami","LA Galaxy","LAFC","Seattle Sounders","NYCFC"],
"Brasileirão":["Flamengo","Palmeiras","Corinthians","Fluminense","Sao Paulo"],
"Saudi Pro League":["Al Hilal","Al Nassr","Al Ittihad","Al Ahli"],
"Süper Lig":["Galatasaray","Fenerbahçe","Besiktas","Trabzonspor"]
};

const retro=["Deutschland 1990","Brasilien 2002","Frankreich 1998","Italien 2006"];

// 🖼️ BILDER FÜR ALLE TEAMS (HIER NUR LINKS ÄNDERN)
const teamImages = {

  // Premier League
  "Manchester United": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/341ec36583354390854e7ad4fd2a02dd_9366/Manchester_United_25-26_Home_Jersey_Red_JI7428_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/e1a2021d272842b0b7e9cdc61055e9ed_9366/Manchester_United_25-26_Away_Jersey_White_JI7423_01_laydown.jpg" },
  "Manchester City": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/780338/01/fnd/EEA/fmt/png/Manchester-City-25/26-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/780350/02/fnd/EEA/fmt/png/Manchester-City-25/26-Ausw%C3%A4rtstrikot-Herren" },
  "Liverpool": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/f54aaf314c3045d29ab6ad15ff1f4e20_9366/Liverpool_FC_25-26_Home_Jersey_Red_JV6423_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/2d296b7db27a4d5a80401ee6f8a00dd3_9366/Liverpool_FC_25-26_Away_Jersey_White_JV6487_01_laydown.jpg" },
  "Chelsea": { home: "https://images.footballfanatics.com/chelsea/chelsea-nike-home-stadium-shirt-2025-26_ss5_p-202492465+pv-5+u-2gcvqxc9vlx6povideuj+v-dwxcvwlvczc98nrqywdw.jpg?_hv=2&w=1018", away: "https://images.footballfanatics.com/chelsea/chelsea-nike-away-stadium-shirt-2025-26_ss5_p-202492460+pv-5+u-pwqn6ak93ykkjzakvmbr+v-sv2shizcmlelwwfegvos.jpg?_hv=2&w=1018" },
  "Arsenal": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/7126b710e8e94a6faea7b0ba98ca531f_9366/Arsenal_25-26_Home_Authentic_Jersey_Red_JI9516_HM30.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/c28d4a1486c746d796d529710d5c137b_9366/Arsenal_25-26_Away_Jersey_Blue_JI9511_01_laydown.jpg" },
  "Tottenham": { home: "https://img01.ztat.net/article/spp-media-p1/8e82f22d94524b089cf5ea96bcd74ebd/d79d07dc63234bf2a400fe712d6942d7.jpg?imwidth=1800&filter=packshot", away: "https://img01.ztat.net/article/spp-media-p1/416b34f0e5cd44178f5ccc76a1984a73/d8515306131e45a28455d4e15adaa3c7.jpg?imwidth=1800&filter=packshot" },
  "Newcastle": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/ac07205a9e7e4d05bb74d637ee420363_9366/Newcastle_United_FC_25-26_Away_Jersey_Green_JJ2245_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/e244bc740678489e9ab6fddbbd71436e_9366/Newcastle_United_FC_25-26_Home_Authentic_Jersey_Black_JI7391_01_laydown.jpg" },
  "Aston Villa": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/88d945d6114842118a2ddbdec6ccc165_9366/Aston_Villa_FC_25-26_Heimtrikot_Weinrot_JN8061_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/f88c00ab1d11444a9b8d8e8a639995a8_9366/Aston_Villa_25-26_Ausweichtrikot_Weiss_KA0859_01_laydown.jpg" },
  "West Ham": { home: "https://shop.whufc.com/siteimg/extrapics/6482.jpg?v=1757676622", away: "https://shop.whufc.com/siteimg/productimages/13803-1200.jpg?v=1753849308" },
  "Brighton": { home: "https://shop.brightonandhovealbion.com/siteimg/productimages/6699-505.jpg?v=1751358438", away: "https://shop.brightonandhovealbion.com/siteimg/prodhires/5767-515.jpg?v=1722350586" },

  // La Liga
  "Real Madrid": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/68f433dd2a1141ffba47a66cfc82d1ff_9366/Real_Madrid_25-26_Home_Authentic_Jersey_White_JV5918_HM30.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/55312dc2debc4ba98b3c931a8caaf002_9366/Real_Madrid_25-26_Away_Authentic_Jersey_Blue_JV5920_HM30.jpg" },
  "Barcelona": { home: "https://thumblr.uniid.it/product/393279/78e31df309fe.jpg?width=1920&format=webp&q=75", away: "https://thumblr.uniid.it/product/423995/dea545f7525e.jpg?width=1920&format=webp&q=75" },
  "Atletico Madrid": { home: "https://thumblr.uniid.it/product/393231/1518d0bbcb9d.jpg?width=1920&format=webp&q=75", away: "https://thumblr.uniid.it/product/393159/b5dc30163c68.jpg?width=1920&format=webp&q=75" },
  "Sevilla": { home: "https://static.wikia.nocookie.net/the-football-database/images/2/26/Sevilla_2025-26_home.png/revision/latest?cb=20250908055250", away: "https://www.futbolemotion.com/imagesarticulos/308471/750/camiseta-adidas-sevilla-fc-segunda-equipacion-2025-2026-rojo-1.webp" },
  "Valencia": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/948275/01/fnd/EEA/fmt/png/Valencia-CF-25/26-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/948281/05/fnd/EEA/fmt/png/Valencia-CF-25/26-Away-Jersey-Men" },
  "Betis": { home: "https://www.hummelsport.de/on/demandware.static/-/Sites-hummel-master-catalog/default/dw2fca122a/images/packshot/231191-6143_2.png", away: "https://thumblr.uniid.it/product/388552/3ac3c672c583.jpg?width=1920&format=webp&q=75" },
  "Villarreal": { home: "https://www.joma-sport.com/dw/image/v2/BFRV_PRD/on/demandware.static/-/Sites-joma-masterCatalog/default/dwfb2e14b8/images/medium/AI10601C0201_1.jpg?sw=900&sh=900&sm=fit", away: "https://www.joma-sport.com/dw/image/v2/BFRV_PRD/on/demandware.static/-/Sites-joma-masterCatalog/default/dwb77d182e/images/medium/AI10601C0202_1.jpg?sw=900&sh=900&sm=fit" },
  "Real Sociedad": { home: "https://thumblr.uniid.it/product/407919/4a4801f8cd14.jpg?width=1920&format=webp&q=75", away: "https://mediarsstore.realsociedad.eus/4016-large_default/kamiseta-away-2526.jpg" },
  "Athletic Bilbao": { home: "https://shop.athletic-club.eus/cdn/shop/files/TM11087-071-RACINGRED-BRILLIANTWHITE-01_430x.jpg?v=1751691502", away: "https://shop.athletic-club.eus/cdn/shop/files/TM11097EU-023-BARITONEBLUE-BLUEATOLL-01_430x.jpg?v=1761874698" },
  "Girona": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/783022/01/fnd/EEA/fmt/png/Girona-FC-25/26-Home-Jersey-Women", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/783025/03/fnd/EEA/fmt/png/Girona-FC-25/26-Away-Jersey-Men" },

  // Bundesliga
  "Bayern München": { home: "", away: "" },
  "Dortmund": { home: "", away: "" },
  "Leipzig": { home: "", away: "" },
  "Leverkusen": { home: "", away: "" },
  "Frankfurt": { home: "", away: "" },
  "Stuttgart": { home: "", away: "" },
  "Freiburg": { home: "", away: "" },
  "Wolfsburg": { home: "", away: "" },
  "Gladbach": { home: "", away: "" },
  "Hoffenheim": { home: "", away: "" },

  // Serie A
  "Juventus": { home: "", away: "" },
  "Inter": { home: "", away: "" },
  "AC Milan": { home: "", away: "" },
  "Napoli": { home: "", away: "" },
  "Roma": { home: "", away: "" },
  "Lazio": { home: "", away: "" },
  "Atalanta": { home: "", away: "" },
  "Fiorentina": { home: "", away: "" },
  "Torino": { home: "", away: "" },
  "Bologna": { home: "", away: "" },

  // Ligue 1
  "PSG": { home: "", away: "" },
  "Marseille": { home: "", away: "" },
  "Lyon": { home: "", away: "" },
  "Monaco": { home: "", away: "" },
  "Lille": { home: "", away: "" },
  "Nice": { home: "", away: "" },
  "Rennes": { home: "", away: "" },
  "Lens": { home: "", away: "" },

  // Portugal
  "Benfica": { home: "", away: "" },
  "Porto": { home: "", away: "" },
  "Sporting CP": { home: "", away: "" },
  "Braga": { home: "", away: "" },
  "Boavista": { home: "", away: "" },
  "Guimaraes": { home: "", away: "" },
  "Famalicao": { home: "", away: "" },
  "Rio Ave": { home: "", away: "" },

  // Niederlande
  "Ajax": { home: "", away: "" },
  "PSV": { home: "", away: "" },
  "Feyenoord": { home: "", away: "" },
  "AZ Alkmaar": { home: "", away: "" },
  "Twente": { home: "", away: "" },
  "Utrecht": { home: "", away: "" },

  // MLS
  "Inter Miami": { home: "", away: "" },
  "LA Galaxy": { home: "", away: "" },
  "LAFC": { home: "", away: "" },
  "Seattle Sounders": { home: "", away: "" },
  "NYCFC": { home: "", away: "" },

  // Brasilien
  "Flamengo": { home: "", away: "" },
  "Palmeiras": { home: "", away: "" },
  "Corinthians": { home: "", away: "" },
  "Fluminense": { home: "", away: "" },
  "Sao Paulo": { home: "", away: "" },

  // Saudi
  "Al Hilal": { home: "", away: "" },
  "Al Nassr": { home: "", away: "" },
  "Al Ittihad": { home: "", away: "" },
  "Al Ahli": { home: "", away: "" },

  // Türkei
  "Galatasaray": { home: "", away: "" },
  "Fenerbahce": { home: "", away: "" },
  "Besiktas": { home: "", away: "" },
  "Trabzonspor": { home: "", away: "" }

};
  
function go(p){location.hash=p;render();}
window.onhashchange=render;
function back(){history.back();}
function openTeam(n){location.hash='team-'+encodeURIComponent(n);}

function searchAll(q){q=q.toLowerCase();if(!q){render();return;}let r=[];
Object.values(leagues).flat().forEach(t=>{if(t.toLowerCase().includes(q))r.push({n:t,t:"Club"});});
Object.values(countries).forEach(c=>{if(c.toLowerCase().includes(q))r.push({n:c,t:"Nation"});});
retro.forEach(x=>{if(x.toLowerCase().includes(q))r.push({n:x,t:"Retro"});});
const app=document.getElementById('app');
app.innerHTML=r.map(x=>`<div class='card' onclick="openTeam('${x.n}')">${x.n}<br><small>${x.t}</small></div>`).join('');
}

function render(){
document.getElementById('cartCount').innerText=cart.length;
const app=document.getElementById('app');
const h=location.hash.replace('#','')||'clubs';

if(h==='national'){
app.innerHTML=Object.entries(continents).map(([c,list])=>`
<div class='sub'>${c}</div>
<div class='grid'>${list.map(code=>`
<div class='card' onclick="openTeam('${countries[code]}')">
<img class='flag' src='https://flagcdn.com/w80/${code}.png'/>
${countries[code]}
</div>`).join('')}</div>`).join('');
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
return;
}

if(h.startsWith('team-')){
let name=decodeURIComponent(h.replace('team-',''));

let homeImg = teamImages[name]?.home || `https://source.unsplash.com/400x300/?${encodeURIComponent(name+' football jersey home')}`;
let awayImg = teamImages[name]?.away || `https://source.unsplash.com/400x300/?${encodeURIComponent(name+' football jersey away')}`;

app.innerHTML=`
<div class='title'>
<span class='back' onclick='back()'>⬅ Zurück</span>
${name}
</div>

<div class='grid'>

<div class='card'>
<img src="${homeImg}" />
<b>${name} Heimtrikot</b>
<div>29.99€</div>

<select id="size-home">
<option>S</option><option>M</option><option>L</option><option>XL</option>
</select>

<input id="qty-home" type="number" value="1" min="1">

<input id="player-home" placeholder="Name">
<input id="number-home" type="number" placeholder="Nr.">

<button class='btn' onclick="
add(
'${name} Heimtrikot',
29.99,
'${homeImg}',
document.getElementById('size-home').value,
parseInt(document.getElementById('qty-home').value),
document.getElementById('player-home').value,
document.getElementById('number-home').value
)">
In den Warenkorb
</button>
</div>

<div class='card'>
<img src="${awayImg}" />
<b>${name} Auswärtstrikot</b>
<div>29.99€</div>

<select id="size-away">
<option>S</option><option>M</option><option>L</option><option>XL</option>
</select>

<input id="qty-away" type="number" value="1" min="1">

<input id="player-away" placeholder="Name">
<input id="number-away" type="number" placeholder="Nr.">

<button class='btn' onclick="
add(
'${name} Auswärtstrikot',
29.99,
'${awayImg}',
document.getElementById('size-away').value,
parseInt(document.getElementById('qty-away').value),
document.getElementById('player-away').value,
document.getElementById('number-away').value
)">
In den Warenkorb
</button>
</div>

</div>
`;
}
}

render();
</script>
</body>
</html>
