<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FootballJerseyONE</title>

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
      width:120px;height:120px;border-radius:50%;
      background:white;display:flex;align-items:center;justify-content:center;
      margin-bottom:20px;font-size:40px;color:#0f172a;font-weight:bold;
    }

    h1{font-size:3rem;margin-bottom:10px;}
    p{opacity:.8;margin-bottom:20px;}

    .btn{
      padding:12px 20px;
      background:#22c55e;border:none;border-radius:10px;
      color:white;cursor:pointer;font-size:1rem;
    }

    nav{
      position:sticky;top:0;background:#0b1220;
      display:flex;justify-content:space-between;
      padding:15px 20px;align-items:center;z-index:1000;
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
    }

    .card img{width:100%;height:200px;object-fit:cover;border-radius:10px;}
    .price{margin:10px 0;font-weight:bold;}

    .cart-btn{
      position:fixed;right:20px;bottom:20px;
      background:#22c55e;padding:15px;border-radius:50px;
      cursor:pointer;font-size:18px;
    }

    .cart-panel{
      position:fixed;right:0;top:0;
      width:350px;height:100%;background:#0b1220;
      padding:20px;overflow-y:auto;
      transform:translateX(100%);
      transition:.3s;
    }

    .cart-panel.open{transform:translateX(0);}

    .remove{color:red;cursor:pointer;font-size:12px;margin-left:10px;}

    .qty-btn{margin:0 5px;cursor:pointer;color:#22c55e;}

    .paypal-container{margin-top:20px;}

    .clear{
      margin-top:10px;
      background:red;
      padding:8px;
      border:none;
      border-radius:8px;
      color:white;
      cursor:pointer;
      width:100%;
    }
  </style>
</head>
<body>

<header>
  <div class="logo">⚽</div>
  <h1>FootballJerseyONE</h1>
  <p>Nationalteams • Vereine • Retro Trikots</p>
  <button class="btn" onclick="document.getElementById('shop').scrollIntoView({behavior:'smooth'})">Shop öffnen</button>
</header>

<nav>
  <div><strong>FootballJerseyONE</strong></div>
  <div>
    <a href="#national">National</a>
    <a href="#clubs">Vereine</a>
    <a href="#retro">Retro</a>
  </div>
</nav>

<div class="container" id="shop">
  <h2 class="section-title">Nationalmannschaften</h2>
  <div class="grid" id="nationalGrid"></div>

  <h2 class="section-title">Vereine</h2>
  <div class="grid" id="clubGrid"></div>

  <h2 class="section-title">Retro</h2>
  <div class="grid" id="retroGrid"></div>
</div>

<div class="cart-btn" onclick="toggleCart()">🛒</div>

<div class="cart-panel" id="cartPanel">
  <h2>Warenkorb</h2>
  <div id="cartItems"></div>
  <h3 id="total">Total: 0€</h3>

  <button class="clear" onclick="clearCart()">Warenkorb leeren</button>

  <div class="paypal-container" id="paypal-button-container"></div>
</div>

<script>
let cart = JSON.parse(localStorage.getItem("cart")) || [];

const products = {
  national:[
    {name:"Deutschland Heim",price:89,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
    {name:"Brasilien Heim",price:89,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"}
  ],
  clubs:[
    {name:"Real Madrid",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
    {name:"FC Barcelona",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"}
  ],
  retro:[
    {name:"Deutschland 1990",price:120,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
    {name:"Brasilien 2002",price:120,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"}
  ]
};

function renderGrid(id,items){
  document.getElementById(id).innerHTML = items.map(p=>`
    <div class='card'>
      <img src='${p.img}'/>
      <h3>${p.name}</h3>
      <div class='price'>${p.price}€</div>
      <button class='btn' onclick='addToCart("${p.name}",${p.price})'>In den Warenkorb</button>
    </div>
  `).join('');
}

function addToCart(name,price){
  let item = cart.find(i=>i.name===name);
  if(item){item.qty++}
  else cart.push({name,price,qty:1});
  save();
}

function removeItem(name){
  cart = cart.filter(i=>i.name!==name);
  save();
}

function changeQty(name,delta){
  let item = cart.find(i=>i.name===name);
  if(!item) return;
  item.qty += delta;
  if(item.qty<=0) removeItem(name);
  save();
}

function clearCart(){
  cart=[];
  save();
}

function save(){
  localStorage.setItem("cart",JSON.stringify(cart));
  updateCart();
}

function updateCart(){
  let el=document.getElementById("cartItems");
  el.innerHTML="";
  let total=0;

  cart.forEach(i=>{
    total += i.price * i.qty;
    el.innerHTML += `
      <p>
        ${i.name} (${i.qty})
        <span class='qty-btn' onclick='changeQty("${i.name}",1)'>+</span>
        <span class='qty-btn' onclick='changeQty("${i.name}",-1)'>-</span>
        <span class='remove' onclick='removeItem("${i.name}")'>X</span>
      </p>`;
  });

  document.getElementById("total").innerText="Total: "+total.toFixed(2)+"€";
  renderPayPal(total);
}

function toggleCart(){
  document.getElementById("cartPanel").classList.toggle("open");
}

function renderPayPal(total){
  const c=document.getElementById("paypal-button-container");
  c.innerHTML="";
  if(total<=0) return;

  paypal.Buttons({
    createOrder:(d,a)=>a.order.create({purchase_units:[{amount:{value:total.toFixed(2)}}]}),
    onApprove:(d,a)=>a.order.capture().then(()=>{
      alert("Zahlung erfolgreich!");
      cart=[];
      save();
    })
  }).render("#paypal-button-container");
}

renderGrid("nationalGrid",products.national);
renderGrid("clubGrid",products.clubs);
renderGrid("retroGrid",products.retro);
updateCart();
</script>

</body>
</html>
