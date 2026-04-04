<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FootballJerseyONE</title>

<script src="https://www.paypal.com/sdk/js?client-id=AVmx5KkGcuZr3ye1nLyNzvB5RhZXyrpnU8t71ZpFZyLyTO9Xt14jSULuXKjmPBNZw1kWigduGZTYXhii&currency=EUR"></script>


<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
  font-family:system-ui;
}

body{
  background:#ffffff;
  color:#111;
}

/* NAVBAR */
nav{
  background:#ffffff;
  border-bottom:1px solid #e5e7eb;
  padding:12px 15px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  flex-wrap:wrap;
  position:sticky;
  top:0;
  z-index:1000;
}

nav a{
  color:#333;
}

nav a:hover{
  color:#22c55e;
}
.search{
  background:#f1f5f9;
  color:#111;
}

/* CONTAINER */
.container{
  padding:15px;
  max-width:1200px;
  margin:auto;
}

/* TITLES */
.title{
  font-size:2rem;
  margin:20px 0;
  display:flex;
  align-items:center;
  gap:10px;
}

.title img{
  width:40px;
  height:40px;
  object-fit:contain;
}

/* GRID */
.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
  gap:12px;
}
/* CARD */



.card{
  transition:0.2s;
  cursor:pointer;
}

.card:hover{
  transform:scale(1.05);
  box-shadow:0 10px 25px rgba(0,0,0,0.15);
}

.card img{
  width:100%;
  height:180px;
  object-fit:contain;
}

/* BUTTON */
.btn{
  width:100%;
  padding:12px;
  background:#111;
  border:none;
  border-radius:10px;
  color:#fff;
  font-weight:600;
  margin-top:10px;
  cursor:pointer;
  transition:0.2s;
}
.btn:hover{
  background:#333;
}

/* INPUTS */
input, select{
  width:100%;
  padding:10px;
  margin-top:8px;
  border-radius:8px;
  border:1px solid #ddd;
  background:#fff;
  color:#111;
}

/* CART */
.cartModal{
  position:fixed;
  top:0;
  right:0;
  width:100%;
  max-width:400px;
  height:100%;
  display:none;
  flex-direction:column;
  z-index:9999;
}

.cartHeader{
  display:flex;
  justify-content:space-between;
  padding:15px;
  border-bottom:1px solid #1e293b;
}

.cartItem{
  background:#ffffff;
  border:1px solid #e5e7eb;
}

.cartImg{
  width:120px;
  height:120px;
  object-fit:contain;
}

.qtyBox{
  display:flex;
  align-items:center;
  gap:8px;
}

.qtyBox button{
  padding:6px 10px;
  border:none;
  background:#22c55e;
  color:#fff;
  border-radius:6px;
  cursor:pointer;
}

.checkoutBox{
  padding:15px;
  border-top:1px solid #1e293b;
}

  @media(max-width:600px){

.title{
  font-size:1.5rem;
}

.card img{
  height:140px;
}

nav{
  flex-direction:column;
  align-items:flex-start;
  gap:10px;
}

.search{
  width:100%;
}

}
  
</style>
</head>

<body>
<script>
const data = [
  {name:"Max", city:"Berlin", country:"Deutschland"},
  {name:"Luca", city:"Mailand", country:"Italien"},
  {name:"Noah", city:"Paris", country:"Frankreich"},
  {name:"Oliver", city:"London", country:"England"},
  {name:"Mateo", city:"Madrid", country:"Spanien"},
  {name:"Elias", city:"Wien", country:"Österreich"},
  {name:"Jonas", city:"Zürich", country:"Schweiz"},
  {name:"Lucas", city:"Amsterdam", country:"Niederlande"},
  {name:"Milan", city:"Prag", country:"Tschechien"},
  {name:"Ivan", city:"Zagreb", country:"Kroatien"},
  {name:"Leo", city:"Buenos Aires", country:"Argentinien"},
  {name:"João", city:"Rio de Janeiro", country:"Brasilien"},
  {name:"Ali", city:"Istanbul", country:"Türkei"},
  {name:"Omar", city:"Dubai", country:"VAE"},
  {name:"Kenji", city:"Tokio", country:"Japan"},
  {name:"Ethan", city:"New York", country:"USA"}
];

function showPopup(){
  const random = data[Math.floor(Math.random()*data.length)];
  const minutes = Math.floor(Math.random()*5)+1;

const flags = {
  "Deutschland": "🇩🇪",
  "Italien": "🇮🇹",
  "Frankreich": "🇫🇷",
  "England": "🏴",
  "Spanien": "🇪🇸",
  "Österreich": "🇦🇹",
  "Schweiz": "🇨🇭",
  "Niederlande": "🇳🇱",
  "Tschechien": "🇨🇿",
  "Kroatien": "🇭🇷",
  "Argentinien": "🇦🇷",
  "Brasilien": "🇧🇷",
  "Türkei": "🇹🇷",
  "VAE": "🇦🇪",
  "Japan": "🇯🇵",
  "USA": "🇺🇸"
};

  const teams = ["Real Madrid","Barcelona","Bayern München","PSG","Liverpool","Juventus"];
  const randomTeam = teams[Math.floor(Math.random()*teams.length)];

  const text = `${flags[random.country]} ${random.name} aus ${random.city} hat ${randomTeam} Trikot vor ${minutes} Min gekauft`;

  const popup = document.getElementById("popup");
  popup.innerText = text;
  popup.style.display = "block";

  setTimeout(()=>{
    popup.style.opacity = "1";
    popup.style.transform = "translateY(0)";
  }, 10);

  setTimeout(()=>{
    popup.style.opacity = "0";
    popup.style.transform = "translateY(20px)";
    setTimeout(()=>{
      popup.style.display = "none";
    }, 400);
  }, 4000);
}

function loopPopup(){
  showPopup();
  setTimeout(loopPopup, Math.random()*8000 + 5000);
}
loopPopup();
<nav>

<div style="display:flex;align-items:center;gap:8px;">
  <span>⚽</span>
  <b>FootballJerseyONE</b>
</div>

<div style="display:flex;align-items:center;gap:10px;flex-wrap:wrap;">
  <input class="search" placeholder="Suche..." oninput="searchAll(this.value)" style="padding:6px 10px;border-radius:8px;border:1px solid #ddd;" />

  <a onclick="go('national')">National</a>
<a onclick="go('clubs')">Vereine</a>
<a onclick="go('retro')">Retro</a>
  <span onclick="openCart()" style="cursor:pointer">
    🛒 <span id="cartCount">0</span>
  </span>
</div>

</nav>

<div class="container" id="app"></div>

<div class="cartModal" id="cartModal">
<div class="cartHeader"><h2>Warenkorb</h2><span onclick="closeCart()" class="back">✖</span></div>
<div class="cartBody" id="cartBody"></div>
<div class="checkoutBox">
<div id="total"></div>

<input id="cust-name" placeholder="Dein Name" style="width:100%;margin:5px 0;">
<input id="cust-email" required> placeholder="E-Mail" style="width:100%;margin:5px 0;">
<input id="cust-address" placeholder="Adresse" style="width:100%;margin:5px 0;">

<div id="paypal-button-container"></div>
</div>
</div>



<script>
let cart=JSON.parse(localStorage.getItem('cart')||'[]');
const SHIPPING=4.99;



function changeQty(i, change){
  cart[i].qty += change;

  if(cart[i].qty <= 0){
    remove(i);
    return;
  }

  save();
  renderCart();
}
  
function save(){
  localStorage.setItem('cart', JSON.stringify(cart));

  const count = cart.reduce((a,b)=>a + b.qty, 0);
  document.getElementById('cartCount').innerText = count;
}
function add(n,p,img,size,qty,player,number){

// ❌ VALIDIERUNG
if(!size){
  alert("Bitte Größe auswählen");
  return;
}

if(!qty || qty < 1){
  alert("Ungültige Menge");
  return;
}

// optional: Name nur wenn Nummer gesetzt
if(number && !player){
  alert("Bitte Namen zum Trikot eingeben");
  return;
}

// ✅ Produkt zusammenführen
let existing = cart.find(item =>
  item.n === n &&
  item.size === size &&
  item.player === player &&
  item.number === number
);

if(existing){
  existing.qty += qty;
}else{
  cart.push({n,p,img,size,qty,player,number});
}

save();
renderCart();
openCart();
  alert("Zum Warenkorb hinzugefügt!");
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

let name = document.getElementById('cust-name').value.trim();
let email = document.getElementById('cust-email').value.trim();
let address = document.getElementById('cust-address').value.trim();

if(!name || !email || !address){
  alert("Bitte alle Felder ausfüllen!");
  return;
}

return actions.order.capture().then(function(details) {

document.getElementById('cartBody').innerHTML = `
<div style="padding:30px;text-align:center">
<h2>🎉 Bestellung erfolgreich!</h2>
<p>Danke ${name} ❤️</p>
</div>
`;

cart = [];
save();

});
}

}).render('#paypal-button-container');

}
}  
function closeCart(){document.getElementById('cartModal').style.display='none';}

function renderCart(){
let b = document.getElementById('cartBody');

if(!cart.length){
  b.innerHTML = "<p style='padding:20px'>🛒 Dein Warenkorb ist leer</p>";
  document.getElementById('total').innerHTML = "";
  return;
}

b.innerHTML = cart.map((c,i)=>`
<div class='cartItem'>

  <img src='${c.img}' class='cartImg'>

  <div class='cartInfo'>
    <b>${c.n}</b><br>
    Größe: ${c.size}<br>
    ${c.player ? `Name: ${c.player}<br>` : ""}
    ${c.number ? `Nr: ${c.number}<br>` : ""}

    <div class='qtyBox'>
      <button onclick='changeQty(${i}, -1)'>-</button>
      <span>${c.qty}</span>
      <button onclick='changeQty(${i}, 1)'>+</button>
    </div>

    <b>${(c.p * c.qty).toFixed(2)}€</b>
  </div>

  <button onclick='remove(${i})'>✖</button>

</div>
`).join('');

let subtotal = cart.reduce((a,b)=>a+(b.p * b.qty),0);
let totalQty = cart.reduce((a,b)=>a + b.qty,0);
let shipping = totalQty ? SHIPPING : 0;
let total = subtotal + shipping;

document.getElementById('total').innerHTML = `
Artikel: ${totalQty}<br>
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
  "Bayern München": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/bbb4a070dea5429aa1756f460ea466d4_9366/FC_Bayern_Munchen_25-26_Heimtrikot_Rot_JJ2137_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/04f3367fcc874b80ada183ac69f9ca05_9366/FC_Bayern_Munchen_25-26_Auswartstrikot_Weiss_JJ2143_01_laydown.jpg" },
  "Dortmund": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/946437/01/fnd/EEA/fmt/png/Borussia-Dortmund-25/26-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/946424/02/fnd/EEA/fmt/png/Borussia-Dortmund-25/26-Ausw%C3%A4rtstrikot-Herren" },
  "Leipzig": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779876/01/fnd/EEA/fmt/png/RB-Leipzig-25/26-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779881/05/fnd/EEA/fmt/png/RB-Leipzig-25/26-Ausw%C3%A4rtstrikot-Herren" },
  "Leverkusen": { home: "https://intersport-de.imgdn.net/fsi/server?type=image&effects=Pad(cc,ffffff),Matte(FFFFFF)&width=600&height=600&source=marktplatz%2Fproduktiv%2F430%2FMT230541%2FMT230541_MT230541HME_BILD01_20250717.jpg", away: "https://thumblr.uniid.it/product/382358/8e9221cbf0b2.jpg?width=1920&format=webp&q=75" },
  "Frankfurt": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/adf36412900d4900b2ddbf7f569c8645_9366/Eintracht_Frankfurt_25-26_Home_Jersey_Black_JZ6359_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/b2c3dd04f5d74e46a4bc49be00f242ad_9366/Eintracht_Frankfurt_25-26_Away_Jersey_White_KK6438_01_laydown.jpg" },
  "Stuttgart": { home: "https://assets.vfb.de/webasset/0cc831e5-a4dd-581c-bdc7-ba4d8ea586e0?width=640&quality=80", away: "https://assets.vfb.de/webasset/d2c64a16-7dd6-572e-b9fa-ed6ecc17b929?width=640&quality=80" },
  "Freiburg": { home: "https://e591dcd21c.edge.storage/res/product_50/SCF-Heimtrikot-Badenova-2526-wei%C3%9F---376536bc-b878-4576-8ca4-14de6b9f006a.jpg", away: "https://e591dcd21c.edge.storage/res/product_50/SCF-Ausw%C3%A4rtstrikot-Badenova-2526-schwarz---ae2fe374-3167-48f8-a45f-177d70624150.jpg" },
  "Wolfsburg": { home: "https://static-shop6.vfl-wolfsburg.de/media/c3/19/0c/1763978323/2501010101-Heimtrikot202526-VfLWolfsburg-1-BCItem25010101011.png?ts=1763978323", away: "https://static-shop6.vfl-wolfsburg.de/media/82/fa/52/1770292971/2501010201-Auswrtstrikot202526-VfLWolfsburg-8-BCItem25010102018.png?ts=1770292971" },
  "Gladbach": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779757/01/fnd/EEA/fmt/png/Borussia-M%C3%B6nchengladbach-25/26-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779764/02/fnd/EEA/fmt/png/Borussia-M%C3%B6nchengladbach-25/26-Ausw%C3%A4rtstrikot-Herren" },
  "Hoffenheim": { home: "https://www.joma-sport.com/dw/image/v2/BFRV_PRD/on/demandware.static/-/Sites-joma-masterCatalog/default/dwb0cd4f66/images/medium/AX10601C0201_1.jpg?sw=900&sh=900&sm=fit", away: "https://www.joma-sport.com/dw/image/v2/BFRV_PRD/on/demandware.static/-/Sites-joma-masterCatalog/default/dwa9ec88c8/images/medium/AX10601C0202_1.jpg?sw=900&sh=900&sm=fit" },

  // Serie A
  "Juventus": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/cfa70dd2fd124535a7a09dc658314966_9366/Juventus_Turin_25-26_Heimtrikot_Weiss_JJ4320_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/ce72a303fb64409d9a455975a4937588_9366/Juventus_25-26_Auswarts_Jersey_Blau_JJ4323_01_laydown.jpg" },
  "Inter": { home: "https://thumblr.uniid.it/product/393142/b9f82b183f6f.jpg?width=1920&format=webp&q=75", away: "https://www.11teamsports.com/media/85/cb/56/1751981146/nike-inter-mailand-trikot-away-25-26-weiss-f497-hj4604-fan-shop-front.png" },
  "AC Milan": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779961/01/fnd/EEA/fmt/png/AC-Milan-25/26-Authentic-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779970/02/fnd/EEA/fmt/png/AC-Milan-25/26-Authentic-Ausw%C3%A4rtstrikot-Herren" },
  "Napoli": { home: "https://thumblr.uniid.it/product/429973/338a12ba8935.jpg?width=1920&format=webp&q=75", away: "https://thumblr.uniid.it/product/434612/bf237f7b394a.jpg?width=1920&format=webp&q=75" },
  "Roma": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/0a59ecf82f4a4e5a8b1f9a7dca3cec74_9366/AS_Rom_25-26_Heimtrikot_Weinrot_JP4184_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/9b0847991dbe418f8f487c89bbdb4519_9366/AS_Roma_25-26_Away_Jersey_Orange_JN9060_01_laydown.jpg" },
  "Lazio": { home: "https://emea.mizuno.com/dw/image/v2/BDBS_PRD/on/demandware.static/-/Sites-masterCatalog_Mizuno/default/dw5b358509/AW25/P2GACX07-23_1.jpg-1000x1000-s_i-c_t_White-f_png.png?sw=950&sh=950", away: "https://emea.mizuno.com/dw/image/v2/BDBS_PRD/on/demandware.static/-/Sites-masterCatalog_Mizuno/default/dw97503556/AW25/P2GACX08-07_1.jpg-1000x1000-s_i-c_t_White-f_png.png?sw=950&sh=950" },
  "Atalanta": { home: "https://store.atalanta.it/images/atalanta/products/small/AT25A01_11.webp", away: "https://store.atalanta.it/images/atalanta/products/small/AT25A02_11.webp" },
  "Fiorentina": { home: "https://www.fiorentinastore.com/images/fiorentina/products/small/FI25A07.webp", away: "https://www.fiorentinastore.com/images/fiorentina/products/small/FI25A08.webp" },
  "Torino": { home: "https://torinofcstore.com/308591-large_default/torino-fc-home-jersey-202425-kids.jpg", away: "https://torinofcstore.com/310518-large_default/torino-fc-away-jersey-202425.jpg" },
  "Bologna": { home: "https://www.bolognafcstore.com/images/bologna/products/small/BO25A01.webp", away: "https://www.bolognafcstore.com/images/bologna/products/small/BO25A02.webp" },

  // Ligue 1
  "PSG": { home: "https://thumblr.uniid.it/product/392883/cb63e25d201c.jpg?width=1920&format=webp&q=75", away: "https://thumblr.uniid.it/product/369617/6e9251292495.jpg?width=1920&format=webp&q=75" },
  "Marseille": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779801/01/fnd/EEA/fmt/png/Olympique-de-Marseille-25/26-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779853/03/fnd/EEA/fmt/png/Olympique-de-Marseille-25/26-Ausweichtrikot-Herren" },
  "Lyon": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/fea3eb8027844fadb513179dd23ed52b_9366/Olympique_Lyonnais_25-26_Heimtrikot_Weiss_JD1396_01_laydown.jpg", away: "https://thumblr.uniid.it/product/396171/294ba2f0bc0e.jpg?width=1920&format=webp&q=75" },
  "Monaco": { home: "https://thumblr.uniid.it/product/404636/4352e210fa00.jpg?width=1920&format=webp&q=75", away: "https://thumblr.uniid.it/product/404637/9a9b746e3831.jpg?width=1920&format=webp&q=75" },
  "Lille": { home: "https://thumblr.uniid.it/product/382344/1b7223baaca3.jpg?width=1920&format=webp&q=75", away: "https://www.11teamsports.com/cdn-cgi/image/format=webp,width=601/ch-de/media/dc/69/65/1751523310/new-balance-losc-lille-trikot-away-2025-2026-fawy-mt230455awy-fan-shop-front.png" },
  "Nice": { home: "https://boutique.ogcnice.com/media/catalog/product/cache/fb71270bb5973ecf596c8bfc818ac4da/m/a/maillot-ogc-nice-domicile-replica-2025-2026202602181453226995c452ab638.jpg?ts=1775152080", away: "https://boutique.ogcnice.com/media/catalog/product/cache/fb71270bb5973ecf596c8bfc818ac4da/m/a/maillot-ogc-nice-2025-2026-exterieur2025082517362268ac82f6d1999.jpg?ts=1775152105" },
  "Rennes": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_100,h_100/global/780219/01/fv/fnd/EEA/fmt/png", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_100,h_100/global/780222/02/fv/fnd/EEA/fmt/png" },
  "Lens": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_100,h_100/global/779728/01/fv/fnd/EEA/fmt/png", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_100,h_100/global/779733/03/fv/fnd/EEA/fmt/png" },

  // Portugal
  "Benfica": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/e51bd3f806cc423bace7cef1fa4d0bb0_9366/Benfica_25-26_Heimtrikot_Rot_JD1403_01_laydown.jpg", away: "https://media.slbenfica.pt/-/media/benficadp/shop/produtos-2020-2021/equipamentos/25-26/alternativa/homem/camisola_alternativa_branca_slbenfica_2025_2026.jpg?v=638842186810000000" },
  "Porto": { home: "https://nb.scene7.com/is/image/NB/mt230419hme_nb_40_i?$pdpflexf22x$&qlt=80&fmt=webp&wid=880&hei=880", away: "https://nb.scene7.com/is/image/NB/mt230420awy_nb_42_i?$pdpflexf22x$&qlt=80&fmt=webp&wid=880&hei=880" },
  "Sporting CP": { home: "https://1216451790.rsc.cdn77.org/temp/1747831722_7a35c1b47db198ece32c0097f26b168e.jpg", away: "https://1216451790.rsc.cdn77.org/temp/1753781579_f69fcf8ae4b7f88965e89cbae4c495ac.jpg" },
  "Braga": { home: "https://1216451790.rsc.cdn77.org/temp/1755857931_7ef1aa00d2196d1a67f957ef86c2edf0.jpg", away: "https://cdn.footballkitarchive.com/2025/07/19/n4gsG8Y2wamMjFs.jpg" },
  "Boavista": { home: "", away: "" },
  "Guimaraes": { home: "", away: "" },
  "Famalicao": { home: "", away: "" },
  "Rio Ave": { home: "", away: "" },

  // Niederlande
  "Ajax": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/1955d0b2a5d34ead9e6263b0c37fc7e4_9366/Ajax_25-26_Heimtrikot_Weiss_JP1448_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/33c16bb7fd5f4398aa33bb9319a145bd_9366/Ajax_Amsterdam_25-26_Ausweichtrikot_Beige_JP1446_01_laydown.jpg" },
  "PSV": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/780420/01/fnd/EEA/fmt/png/PSV-Eindhoven-25/26-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/780429/02/fnd/EEA/fmt/png/PSV-Eindhoven-25/26-Ausw%C3%A4rtstrikot-Herren" },
  "Feyenoord": { home: "https://de.castore.com/cdn/shop/files/TM12221-BRILLIANTWHITE-FIERYRED-01_1000x1000.jpg?v=1747912589", away: "https://de.castore.com/cdn/shop/files/TM12229_2_1000x1000.jpg?v=1754918780" },
  "AZ Alkmaar": { home: "https://cdn.footballkitarchive.com/2025/06/27/f2pycomNscCb240.jpg", away: "https://cdn.footballkitarchive.com/2025/08/19/CqfQmQvZpkzhtHk.jpg" },
  "Twente": { home: "https://cdn.footballkitarchive.com/2025/07/01/Y97yYsfNEzur10b.jpg", away: "https://cdn.footballkitarchive.com/2025/07/01/wxEZbHM8qTO6rIi.jpg" },
  "Utrecht": { home: "https://thumblr.uniid.it/product/407016/5bb42166ef79.jpg?width=1920&format=webp&q=75", away: "https://thumblr.uniid.it/product/407017/31ae8c08a7ab.jpg?width=1920&format=webp&q=75" },

  // MLS
  "Inter Miami": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/0e25cc23565d441c8c72d78240736721_9366/Inter_Miami_CF_25-26_Home_Authentic_Jersey_Pink_JJ1393_HM30.jpg", away: "https://thumblr.uniid.it/product/384968/e33998275c8f.jpg?width=1920&format=webp&q=75" },
  "LA Galaxy": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/8d6c8235a71145aca4b35ab218da3c72_9366/LA_Galaxy_26-27_Heimtrikot_Weiss_JL6810_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/2b4891208e134ba09388a9400d99d218_9366/LA_Galaxy_25-26_Away_Jersey_Blue_IV9853_01_laydown.jpg" },
  "LAFC": { home: "https://thumblr.uniid.it/product/458926/892bf75dc9d0.jpg?width=1920&format=webp&q=75", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/564820db34124a6f867ec25928f5c7a6_9366/LAFC_25-26_Away_Jersey_White_IV9857_01_laydown.jpg" },
  "Seattle Sounders": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/50b538e1bd6e40e28d1b6d09edf0eda0_9366/Seattle_Sounders_FC_24-25_Home_Jersey_Green_HZ6187_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/9d5e8c299c114deb8afeddd15ee0ca2c_9366/Seattle_Sounders_FC_25-26_Away_Jersey_Blau_IV9915_01_laydown.jpg" },
  "NYCFC": { home: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/503655323613403e93a7898909272c46_9366/New_York_City_FC_25-26_Heimtrikot_Blau_IV9926_01_laydown.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/6b6816373dc34ffeac17230292f34e4a_9366/New_York_City_FC_26-27_Away_Jersey_Blue_JL6826_01_laydown.jpg" },

  // Brasilien
  "Flamengo": { home: "https://cdn.footballkitarchive.com/2025/04/23/Lh9Kea9rhzJgDRs.jpg", away: "https://assets.adidas.com/images/h_2000,f_auto,q_auto,fl_lossy,c_fill,g_auto/1889d107c4c041c6ac146bf8d81a5a9f_9366/CR_Flamengo_25_Ausweichtrikot_Weiss_JI7303_01_laydown.jpg" },
  "Palmeiras": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_750,h_750/global/782426/01/fnd/GBR/fmt/png/SE-Palmeiras-Torcedor-25/26-Home-Jersey-Men", away: "https://sportiger.de/cdn/shop/files/se-palmeiras-sao-paulo-trikot-away-herren-25-26-8186708.png?v=1753467946&width=750" },
  "Corinthians": { home: "https://thumblr.uniid.it/product/393038/2fbb03fc31de.jpg?width=1920&format=webp&q=75", away: "https://thumblr.uniid.it/product/393039/38b20a3c5da4.jpg?width=1920&format=webp&q=75" },
  "Fluminense": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/788188/01/fnd/EEA/fmt/png/Fluminense-Torcedor-2026-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/788285/01/fnd/EEA/fmt/png/Fluminense-Torcedor-2026-Ausw%C3%A4rtstrikot-Herren" },
  "Sao Paulo": { home: "https://thumblr.uniid.it/product/430098/4ea64219ea19.jpg?width=1920&format=webp&q=75", away: "https://thumblr.uniid.it/product/430099/541f1133f0dd.jpg?width=1920&format=webp&q=75" },

  // Saudi
  "Al Hilal": { home: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779470/01/fnd/EEA/fmt/png/Al-Hilal-SFC-25/26-Heimtrikot-Herren", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779471/02/fnd/EEA/fmt/png/Al-Hilal-SFC-25/26-Ausw%C3%A4rtstrikot-Herren" },
  "Al Nassr": { home: "https://thumblr.uniid.it/product/396685/38b4b3d58a78.jpg?width=1920&format=webp&q=75", away: "https://assets.adidas.com/images/w_1880,f_auto,q_auto/68c2841fd06140bb92518a89ffa286fa_9366/KL0331_01_laydown.jpg" },
  "Al Ittihad": { home: "https://shop.ittihadclub.sa/cdn/shop/files/male_vapor_Front.jpg?v=1762104405&width=1946", away: "https://shop.ittihadclub.sa/cdn/shop/files/menMatchfront.jpg?v=1762249152&width=1946" },
  "Al Ahli": { home: "https://assets.adidas.com/images/w_1880,f_auto,q_auto/e2dc3a2f566740fbaa6a71c9c312aabe_9366/KL0334_01_laydown.jpg", away: "https://assets.adidas.com/images/w_1880,f_auto,q_auto/d6084c375b314d518ca7eeac8cd4b8c3_9366/KL0343_01_laydown.jpg" },

  // Türkei
  "Galatasaray": { home: "https://www.11teamsports.com/media/ea/79/83/1764328573/puma-galatasaray-istanbul-fan-trikot-h-25-26-k-f01-779828-fan-shop-front.png", away: "https://images.puma.com/image/upload/f_auto,q_auto,b_rgb:fafafa,w_550,h_550/global/779811/02/fnd/EEA/fmt/png/Galatasaray-SK-25/26-Ausw%C3%A4rtstrikot-Herren" },
  "Fenerbahce": { home: "https://intersport-de.imgdn.net/fsi/server?type=image&effects=Pad(cc,ffffff),Matte(FFFFFF)&width=1200&height=1200&source=marketplace2025%2Ftradebyte%2F3801%2F10815659-f7b3835000144.jpg", away: "https://intersport-de.imgdn.net/fsi/server?type=image&effects=Pad(cc,ffffff),Matte(FFFFFF)&width=1200&height=1200&source=marketplace2025%2Ftradebyte%2F3801%2F10815671-c826642450805.jpg" },
  "Besiktas": { home: "https://www.11teamsports.com/media/09/5b/f8/1757948177/adidas-besiktas-istanbul-trikot-home-25-26-weiss-jd1418-fan-shop-front.png", away: "https://www.11teamsports.com/media/cc/ae/79/1757947959/adidas-besiktas-istanbul-trikot-away-25-26-schwarz-jd1416-fan-shop-front.png" },
  "Trabzonspor": { home: "https://aad216.a-cdn.akinoncloud.com/products/2025/07/23/30424056/01655b97-1c5e-4bb5-a171-05def73cb15a_size2010x2010_cropCenter.jpg", away: "https://cdn.footballkitarchive.com/2025/08/25/ITWMca32Vd2e29j.jpg" }

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
save();
const app=document.getElementById('app');
const h=location.hash.replace('#','')||'home';

if(h==='home'){

const bestsellers = [
"PSG",
"Real Madrid",
"Frankreich",
"Deutschland",
"Liverpool",
"Barcelona",
"Bayern München",
"Juventus"
];

app.innerHTML = `

<!-- HERO -->
<div style="
background:linear-gradient(135deg,#ffffff,#f1f5f9);
color:#111;
padding:30px;
border-radius:20px;
margin-bottom:25px;
text-align:center;
">
<h1 style="font-size:2rem;">⚽ FootballJerseyONE</h1>
<p style="color:#555;margin-top:10px;">
Die besten Fußballtrikots 2026 🔥
</p>

<button class="btn" onclick="go('clubs')" style="max-width:200px;margin-top:15px;">
Jetzt shoppen
</button>
</div>

<!-- TRUST -->
<div style="display:flex;gap:15px;flex-wrap:wrap;margin-bottom:20px;">
<div class="card">🚚 Schneller Versand</div>
<div class="card">💳 Sicher bezahlen</div>
<div class="card">⭐ Top Qualität</div>
</div>

<!-- BESTSELLER -->
<div class='title'>🔥 Bestseller</div>

<div class='grid'>
${bestsellers.map(name => {

let img = teamImages[name]?.home || `https://source.unsplash.com/400x300/?${name} jersey`;

return `
<div class='card' onclick="openTeam(${JSON.stringify(name)})">

<div style="
position:absolute;
background:#ef4444;
padding:4px 8px;
border-radius:6px;
font-size:12px;">
Bestseller
</div>

<img src="${img}" />

<b>${name}</b>

<div style="color:#22c55e;font-size:13px;">✔ Auf Lager</div>
<div style="color:#ef4444;font-size:12px;">⚠ Nur noch wenige!</div>

<div style="font-weight:700;margin-top:5px;">11.99€</div>

<button class='btn'>Jetzt ansehen</button>

</div>
`;

}).join('')}
</div>

<!-- CTA -->
<div style="
margin-top:30px;
padding:20px;
background:#020617;
border-radius:15px;
text-align:center;
border:1px solid #1e293b;
">
<h2>🔥 Verpasse keine Angebote</h2>
<p style="color:#94a3b8;">Neue Trikots jede Woche</p>
</div>

`;

return;
}
  
if(h==='national'){
app.innerHTML=Object.entries(continents).map(([c,list])=>`
.sub{
  font-size:1.4rem;
  margin:20px 0 10px;
  font-weight:600;
}
<div class='grid'>${list.map(code=>`
<div class='card' onclick="openTeam('${countries[code]}')">
<img class='flag' src='https://flagcdn.com/w80/${code}.png'/>
${countries[code]}
</div>`).join('')}</div>`).join('');
return;
}

if(h==='clubs'){
app.innerHTML=Object.entries(leagues).map(([l,t])=>`
.sub{
  font-size:1.4rem;
  margin:20px 0 10px;
  font-weight:600;
}
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
<img src="https://www.textilwirtschaft.de/news/media/23/ADdias-Trikots-227790.jpeg">
${name}
</div>

<img src="https://media.gq-magazin.de/photos/690b73094fa0fed19422f786/3:2/w_2562,h_1708,c_limit/WC26_Product_Shoot_16x9_HighRes_Hero.jpg" style="
width:100%;
height:250px;
object-fit:cover;
border-radius:15px;
margin-bottom:20px;
">

<div class='grid'>

<div class='card'>
<img src="${homeImg}" />
<b style="font-size:14px;">${name} Heimtrikot 25/26</b>

<div style="font-size:18px;font-weight:700;">11.99€</div>

<div style="color:#22c55e;font-size:13px;">
✔ Auf Lager
</div>
<div style="color:#ef4444;font-size:13px;font-weight:600;">
⚠ Nur noch 3 verfügbar
</div></div>

<div style="font-size:12px;color:#666;">inkl. Versand ab 4.99€</div>

<select id="size-home">
<option>S</option><option>M</option><option>L</option><option>XL</option>
</select>

<input id="qty-home" type="number" value="1" min="1">

<input id="player-home" placeholder="Name">
<input id="number-home" type="number" placeholder="Nr.">

<button class='btn' onclick="
add(
'${name} Heimtrikot',
11.99,
'${homeImg}',
document.getElementById('size-home').value,
parseInt(document.getElementById('qty-home').value),
document.getElementById('player-home').value,
document.getElementById('number-home').value
)">
🛒 Jetzt kaufen
</button>
</div>

<div class='card'>
<img src="${awayImg}" />
<b style="font-size:14px;">${name} Auswärtstrikot 25/26</b>

<div style="font-size:18px;font-weight:700;">11.99€</div>

<div style="color:#16a34a;font-size:13px;"><div style="color:#22c55e;font-size:13px;">
✔ Auf Lager
</div>

<div style="color:#ef4444;font-size:13px;font-weight:600;">
⚠ Nur noch 3 verfügbar
</div></div>

<div style="font-size:12px;color:#666;">inkl. Versand ab 4.99€</div>

<select id="size-away">
<option>S</option><option>M</option><option>L</option><option>XL</option>
</select>

<input id="qty-away" type="number" value="1" min="1">

<input id="player-away" placeholder="Name">
<input id="number-away" type="number" placeholder="Nr.">

<button class='btn' onclick="
add(
'${name} Auswärtstrikot',
11.99,
'${awayImg}',
document.getElementById('size-away').value,
parseInt(document.getElementById('qty-away').value),
document.getElementById('player-away').value,
document.getElementById('number-away').value
)">
🛒 Jetzt kaufen
</button>
</div>

</div>
`;
}
}

render();
</script>
<div id="popup" style="
position:fixed;
bottom:20px;
left:20px;
background:#020617;
color:#fff;
padding:12px 16px;
border-radius:12px;
box-shadow:0 10px 30px rgba(0,0,0,0.6);
opacity:0;
transform:translateY(20px);
transition:0.4s;
z-index:99999;
font-size:14px;
"></div>
</body>
</html>
