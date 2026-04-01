<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FootballJerseyONE</title>

  <!-- PayPal SDK (Client-ID später ersetzen) -->
  <script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=EUR"></script>

  <style>
    *{margin:0;padding:0;box-sizing:border-box;font-family:Arial, Helvetica, sans-serif;}
    body{background:#0f172a;color:white;}

    header{
      height:100vh;
      display:flex;
      flex-direction:column;
      justify-content:center;
      align-items:center;
      text-align:center;
      background:radial-gradient(circle at top,#1e293b,#0f172a);
    }

    .logo{
      width:120px;
      height:120px;
      border-radius:50%;
      background:white;
      display:flex;
      align-items:center;
      justify-content:center;
      margin-bottom:20px;
      font-size:40px;
      color:#0f172a;
      font-weight:bold;
    }

    h1{font-size:3rem;margin-bottom:10px;}
    p{opacity:.8;margin-bottom:20px;}

    .btn{
      padding:12px 20px;
      background:#22c55e;
      border:none;
      border-radius:10px;
      color:white;
      cursor:pointer;
      font-size:1rem;
      transition:.2s;
    }

    .btn:hover{background:#16a34a;}

    nav{
      position:sticky;
      top:0;
      background:#0b1220;
      display:flex;
      justify-content:space-between;
      padding:15px 20px;
      align-items:center;
      z-index:1000;
    }

    nav a{color:white;margin:0 10px;text-decoration:none;}

    .container{padding:40px 20px;}
    .section-title{font-size:2rem;margin:30px 0 20px;}

    .grid{
      display:grid;
      grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
      gap:20px;
    }

    .card{
      background:#111827;
      border-radius:15px;
      padding:15px;
      text-align:center;
      transition:.2s;
    }

    .card:hover{transform:translateY(-5px);}

    .card img{
      width:100%;
      border-radius:10px;
      height:200px;
      object-fit:cover;
    }

    .price{margin:10px 0;font-weight:bold;}

    .cart-btn{
      position:fixed;
      right:20px;
      bottom:20px;
      background:#22c55e;
      padding:15px;
      border-radius:50px;
      cursor:pointer;
      font-size:18px;
    }

    .cart-panel{
      position:fixed;
      right:0;
      top:0;
      width:320px;
      height:100%;
      background:#0b1220;
      padding:20px;
      overflow-y:auto;
      display:none;
    }

    .paypal-container{margin-top:20px;}

    .remove{color:red;cursor:pointer;font-size:12px;}
  </style>
</head>
<body>

<header>
  <div class="logo">⚽</div>
  <h1>FootballJerseyONE</h1>
  <p>Top Vereine weltweit • Nationalteams • Retro</p>
  <button class="btn" onclick="document.getElementById('shop').scrollIntoView({behavior:'smooth'})">Shop öffnen</button>
</header>

<nav>
  <div><strong>FootballJerseyONE</strong></div>
  <div>
    <a href="#national">Nationalteams</a>
    <a href="#clubs">Vereine</a>
    <a href="#retro">Retro</a>
  </div>
</nav>

<div class="container" id="shop">

  <h2 class="section-title" id="national">Nationalmannschaften</h2>
  <div class="grid" id="nationalGrid"></div>

  <h2 class="section-title" id="clubs">Vereinsmannschaften (Top 10 Ligen)</h2>
  <div class="grid" id="clubGrid"></div>

  <h2 class="section-title" id="retro">Retro Trikots</h2>
  <div class="grid" id="retroGrid"></div>

</div>

<div class="cart-btn" onclick="toggleCart()">🛒</div>

<div class="cart-panel" id="cartPanel">
  <h2>Warenkorb</h2>
  <div id="cartItems"></div>
  <h3 id="total">Total: 0€</h3>
  <div class="paypal-container" id="paypal-button-container"></div>
</div>

<script>
let cart = [];

const products = {
  national:[
    {name:"Deutschland Heim",price:89,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
    {name:"Brasilien Heim",price:89,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"}
  ],

  clubs:[
    // Premier League
    {name:"Manchester City",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},
    {name:"Liverpool FC",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
    {name:"Arsenal FC",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},

    // La Liga
    {name:"Real Madrid",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
    {name:"FC Barcelona",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},
    {name:"Atletico Madrid",price:94,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},

    // Serie A
    {name:"Juventus",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},
    {name:"Inter Mailand",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
    {name:"AC Milan",price:94,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},

    // Bundesliga
    {name:"Bayern München",price:94,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
    {name:"Borussia Dortmund",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},

    // Ligue 1
    {name:"Paris Saint-Germain",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},

    // Eredivisie
    {name:"Ajax Amsterdam",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},

    // Primeira Liga
    {name:"Benfica",price:94,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},

    // Brasil
    {name:"Flamengo",price:94,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},

    // MLS
    {name:"Inter Miami",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},

    // Türkei
    {name:"Galatasaray",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"}
  ],

  retro:[
    {name:"Deutschland 1990",price:120,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
    {name:"Brasilien 2002",price:120,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"}
  ]
};

function renderGrid(id,items){
  const el=document.getElementById(id);
  el.innerHTML="";
  items.forEach(p=>{
    el.innerHTML+=`
      <div class='card'>
        <img src='${p.img}'/>
        <h3>${p.name}</h3>
        <div class='price'>${p.price}€</div>
        <button class='btn' onclick='addToCart("${p.name}",${p.price})'>In den Warenkorb</button>
      </div>
    `;
  });
}

function addToCart(name,price){
  cart.push({name,price});
  updateCart();
}

function removeItem(index){
  cart.splice(index,1);
  updateCart();
}

function updateCart(){
  const el=document.getElementById("cartItems");
  el.innerHTML="";
  let total=0;

  cart.forEach((item,i)=>{
    total+=item.price;
    el.innerHTML+=`<p>${item.name} - ${item.price}€ <span class='remove' onclick='removeItem(${i})'>X</span></p>`;
  });

  document.getElementById("total").innerText="Total: "+total+"€";

  renderPayPal(total);
}

function toggleCart(){
  const panel=document.getElementById("cartPanel");
  panel.style.display=panel.style.display==="block"?"none":"block";
}

function renderPayPal(total){
  const container=document.getElementById("paypal-button-container");
  container.innerHTML="";

  if(total<=0) return;

  paypal.Buttons({
    createOrder:function(data,actions){
      return actions.order.create({
        purchase_units:[{amount:{value:total.toFixed(2)}}]
      });
    },
    onApprove:function(data,actions){
      return actions.order.capture().then(function(details){
        alert('Zahlung erfolgreich von '+details.payer.name.given_name);
        cart=[];
        updateCart();
      });
    }
  }).render('#paypal-button-container');
}

renderGrid("nationalGrid",products.national);
renderGrid("clubGrid",products.clubs);
renderGrid("retroGrid",products.retro);
</script>

</body>
</html>
