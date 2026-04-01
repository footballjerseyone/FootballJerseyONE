<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FootballJerseyONE</title>

  <script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=EUR"></script>

  <style>
    *{margin:0;padding:0;box-sizing:border-box;font-family:Arial;}
    body{background:#0f172a;color:white;}

    header{
      height:100vh;
      display:flex;
      flex-direction:column;
      justify-content:center;
      align-items:center;
      text-align:center;
      background:radial-gradient(circle,#1e293b,#0f172a);
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
      color:white;cursor:pointer;
    }

    nav{
      position:sticky;top:0;background:#0b1220;
      display:flex;justify-content:space-between;
      padding:15px 20px;z-index:1000;
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
      cursor:pointer;
      transition:.2s;
      text-align:center;
    }

    .card:hover{transform:scale(1.03);}

    .card img{width:100%;height:200px;object-fit:cover;border-radius:10px;}

    .price{margin:10px 0;font-weight:bold;}

    .cart-btn{
      position:fixed;right:20px;bottom:20px;
      background:#22c55e;padding:15px;border-radius:50px;
      cursor:pointer;
    }

    .cart-panel{
      position:fixed;right:0;top:0;
      width:350px;height:100%;background:#0b1220;
      padding:20px;overflow-y:auto;
      transform:translateX(100%);
      transition:.3s;
    }

    .cart-panel.open{transform:translateX(0);}

    .back-btn{
      margin-bottom:15px;
      background:#334155;
      padding:8px;
      border:none;
      color:white;
      border-radius:8px;
      cursor:pointer;
    }

    .detail-img{
      width:100%;border-radius:15px;margin-bottom:15px;
    }

  </style>
</head>
<body>

<header>
  <div class="logo">⚽</div>
  <h1>FootballJerseyONE</h1>
  <p>National • Vereine • Retro</p>
  <button class="btn" onclick="scrollShop()">Shop öffnen</button>
</header>

<nav>
  <div><strong>FootballJerseyONE</strong></div>
  <div>
    <a href="#shop">Shop</a>
  </div>
</nav>

<div class="container" id="shop">

  <div id="shopView">
    <h2 class="section-title">Nationalteams</h2>
    <div class="grid" id="nationalGrid"></div>

    <h2 class="section-title">Vereine</h2>
    <div class="grid" id="clubGrid"></div>

    <h2 class="section-title">Retro</h2>
    <div class="grid" id="retroGrid"></div>
  </div>

  <div id="productView" style="display:none;">
    <button class="back-btn" onclick="backToShop()">⬅ Zurück</button>
    <img id="pImg" class="detail-img" />
    <h2 id="pName"></h2>
    <h3 id="pPrice"></h3>
    <button class="btn" onclick="addCurrentToCart()">In den Warenkorb</button>
  </div>

</div>

<div class="cart-btn" onclick="toggleCart()">🛒</div>

<div class="cart-panel" id="cartPanel">
  <h2>Warenkorb</h2>
  <div id="cartItems"></div>
  <h3 id="total">Total: 0€</h3>
</div>

<script>
let cart=[];
let currentProduct=null;

const products={
  national:[
    {id:1,name:"Deutschland Heim",price:89,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
    {id:2,name:"Brasilien Heim",price:89,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"}
  ],
  clubs:[
    {id:3,name:"Real Madrid",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
    {id:4,name:"FC Barcelona",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"}
  ],
  retro:[
    {id:5,name:"Deutschland 1990",price:120,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
    {id:6,name:"Brasilien 2002",price:120,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"}
  ]
};

function allProducts(){
  return [...products.national,...products.clubs,...products.retro];
}

function renderGrid(list,el){
  el.innerHTML=list.map(p=>`
    <div class='card' onclick='openProduct(${p.id})'>
      <img src='${p.img}'/>
      <h3>${p.name}</h3>
      <div class='price'>${p.price}€</div>
    </div>
  `).join('');
}

function openProduct(id){
  const p=allProducts().find(x=>x.id===id);
  currentProduct=p;

  document.getElementById("shopView").style.display="none";
  document.getElementById("productView").style.display="block";

  document.getElementById("pImg").src=p.img;
  document.getElementById("pName").innerText=p.name;
  document.getElementById("pPrice").innerText=p.price+"€";
}

function backToShop(){
  document.getElementById("shopView").style.display="block";
  document.getElementById("productView").style.display="none";
}

function addCurrentToCart(){
  cart.push(currentProduct);
  updateCart();
  alert("Hinzugefügt!");
}

function toggleCart(){
  document.getElementById("cartPanel").classList.toggle("open");
}

function updateCart(){
  let el=document.getElementById("cartItems");
  let total=0;

  el.innerHTML=cart.map(i=>{
    total+=i.price;
    return `<p>${i.name} - ${i.price}€</p>`;
  }).join('');

  document.getElementById("total").innerText="Total: "+total+"€";
}

function scrollShop(){
  document.getElementById("shop").scrollIntoView({behavior:"smooth"});
}

renderGrid(products.national,document.getElementById("nationalGrid"));
renderGrid(products.clubs,document.getElementById("clubGrid"));
renderGrid(products.retro,document.getElementById("retroGrid"));
</script>

</body>
</html>
