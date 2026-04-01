<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FootballJerseyONE</title>

  <!-- PayPal SDK (nur aktiv wenn Client-ID gesetzt wird) -->
  <script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=EUR"></script>

  <style>
    *{margin:0;padding:0;box-sizing:border-box;font-family:Arial, Helvetica, sans-serif;}
    body{background:#0f172a;color:white;}

    nav{
      position:sticky;top:0;
      background:#0b1220;
      display:flex;
      justify-content:space-between;
      padding:15px 20px;
      z-index:1000;
    }

    nav a{color:white;margin:0 10px;text-decoration:none;cursor:pointer;}

    header{
      padding:60px 20px;
      text-align:center;
      background:radial-gradient(circle at top,#1e293b,#0f172a);
    }

    .logo{
      width:100px;height:100px;border-radius:50%;
      background:white;color:#0f172a;
      display:flex;align-items:center;justify-content:center;
      font-size:40px;margin:0 auto 15px;
      font-weight:bold;
    }

    .btn{
      padding:10px 15px;
      background:#22c55e;
      border:none;border-radius:10px;
      color:white;cursor:pointer;
    }

    .container{padding:20px;}

    .grid{
      display:grid;
      grid-template-columns:repeat(auto-fit,minmax(200px,1fr));
      gap:15px;
    }

    .card{
      background:#111827;
      padding:10px;
      border-radius:12px;
      cursor:pointer;
      transition:.2s;
    }

    .card:hover{transform:scale(1.03);}

    .card img{width:100%;height:160px;object-fit:cover;border-radius:10px;}

    .price{margin:8px 0;font-weight:bold;}

    .cart{
      position:fixed;right:15px;bottom:15px;
      background:#22c55e;padding:12px;
      border-radius:50px;cursor:pointer;
    }

    .panel{
      position:fixed;right:0;top:0;width:320px;height:100%;
      background:#0b1220;padding:15px;
      display:none;overflow:auto;
    }
  </style>
</head>
<body>

<nav>
  <div><b>FootballJerseyONE</b></div>
  <div>
    <a onclick="go('shop')">Shop</a>
    <a onclick="go('checkout')">Warenkorb</a>
  </div>
</nav>

<header id="home">
  <div class="logo">⚽</div>
  <h1>FootballJerseyONE</h1>
  <p>National • Vereine • Retro</p>
  <button class="btn" onclick="go('shop')">Zum Shop</button>
</header>

<div class="container" id="app"></div>

<div class="cart" onclick="toggleCart()">🛒</div>
<div class="panel" id="cartPanel">
  <h2>Warenkorb</h2>
  <div id="cartItems"></div>
  <h3 id="total"></h3>
  <div id="paypal"></div>
</div>

<script>

let cart = JSON.parse(localStorage.getItem('cart')||'[]');

const products = [
  {name:"Deutschland Heim",price:89,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
  {name:"Brasilien Heim",price:89,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},
  {name:"Real Madrid",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},
  {name:"Barcelona",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
  {name:"Bayern München",price:94,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
  {name:"Retro 1990",price:120,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"}
];

function save(){
  localStorage.setItem('cart',JSON.stringify(cart));
}

function go(page){
  location.hash = page;
  render();
}

window.onhashchange = render;

function render(){
  const app=document.getElementById('app');
  const hash=location.hash.replace('#','')||'shop';

  if(hash==='shop'){
    app.innerHTML=`<h2>Shop</h2><div class='grid'>`+
    products.map((p,i)=>`
      <div class='card' onclick='openProduct(${i})'>
        <img src='${p.img}'/>
        <h3>${p.name}</h3>
        <div class='price'>${p.price}€</div>
      </div>
    `).join('')+`</div>`;
  }

  else if(hash.startsWith('product')){
    const id=hash.split('-')[1];
    const p=products[id];
    app.innerHTML=`
      <h2>${p.name}</h2>
      <img src='${p.img}' style='width:100%;max-width:400px;border-radius:10px'/>
      <h3>${p.price}€</h3>
      <button class='btn' onclick='add(${id})'>In Warenkorb</button>
    `;
  }

  else if(hash==='checkout'){
    renderCart();
  }
}

function openProduct(i){
  location.hash='product-'+i;
}

function add(i){
  cart.push(products[i]);
  save();
  alert('Hinzugefügt');
}

function toggleCart(){
  const p=document.getElementById('cartPanel');
  p.style.display=p.style.display==='block'?'none':'block';
  renderCart();
}

function renderCart(){
  const el=document.getElementById('cartItems');
  let total=0;
  el.innerHTML='';

  cart.forEach((p,i)=>{
    total+=p.price;
    el.innerHTML+=`<p>${p.name} ${p.price}€</p>`;
  });

  document.getElementById('total').innerText='Total: '+total+'€';

  const pay=document.getElementById('paypal');
  pay.innerHTML='';

  if(total<=0) return;

  if(typeof paypal!=='undefined'){
    paypal.Buttons({
      createOrder:(d,a)=>a.order.create({
        purchase_units:[{amount:{value:total.toFixed(2)}}]
      }),
      onApprove:(d,a)=>a.order.capture().then(()=>{
        alert('Zahlung erfolgreich');
        cart=[];
        save();
        renderCart();
      })
    }).render('#paypal');
  }
}

render();

</script>

</body>
</html>
