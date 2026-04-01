[index.html.txt](https://github.com/user-attachments/files/26412724/index.html.txt)
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FootballJerseyONE - Online Shop</title>
  <style>
    * { margin:0; padding:0; box-sizing:border-box; font-family:Arial, Helvetica, sans-serif; }
    body { background:#0f172a; color:#fff; }

    header {
      background: linear-gradient(90deg,#111827,#1f2937);
      padding:15px 20px;
      display:flex;
      justify-content:space-between;
      align-items:center;
      position:sticky; top:0; z-index:1000;
    }

    header h1 { color:#22c55e; font-size:22px; }

    nav a { color:#fff; margin-left:12px; text-decoration:none; }
    nav a:hover { color:#22c55e; }

    .cart-btn { background:#22c55e; color:black; padding:8px 12px; border:none; border-radius:6px; cursor:pointer; }

    .hero {
      height:55vh;
      display:flex;
      justify-content:center;
      align-items:center;
      flex-direction:column;
      text-align:center;
      background:url('https://images.unsplash.com/photo-1518091043644-c1d4457512c6') center/cover;
      position:relative;
    }

    .hero::after { content:''; position:absolute; inset:0; background:rgba(0,0,0,0.6); }
    .hero h2,.hero p { position:relative; z-index:1; }
    .hero h2 { font-size:40px; color:#22c55e; }

    .container { padding:30px; max-width:1200px; margin:auto; }

    .section-title { font-size:24px; border-left:4px solid #22c55e; padding-left:10px; margin:20px 0; }

    .grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:20px; }

    .card { background:#1f2937; border-radius:10px; overflow:hidden; }
    .card img { width:100%; height:180px; object-fit:cover; }
    .card-content { padding:10px; }

    .price { color:#22c55e; font-weight:bold; }

    button { width:100%; margin-top:10px; padding:10px; border:none; background:#22c55e; cursor:pointer; font-weight:bold; }
    button:hover { background:#16a34a; }

    footer { text-align:center; padding:20px; margin-top:40px; background:#111827; }

    .modal {
      display:none;
      position:fixed;
      inset:0;
      background:rgba(0,0,0,0.7);
      justify-content:center;
      align-items:center;
    }

    .modal-content {
      background:#1f2937;
      padding:20px;
      width:90%;
      max-width:500px;
      border-radius:10px;
    }

    .close { float:right; cursor:pointer; color:red; }

    .login-box {
      background:#1f2937;
      padding:15px;
      margin:20px 0;
      border-radius:10px;
    }

    input {
      width:100%;
      padding:10px;
      margin:5px 0;
      border:none;
      border-radius:6px;
    }
  </style>
</head>
<body>

<header>
  <h1>FootballJerseyONE</h1>
  <nav>
    <a href="#national">National</a>
    <a href="#club">Vereine</a>
    <a href="#retro">Retro</a>
  </nav>
  <button class="cart-btn" onclick="openCart()">Warenkorb (<span id="cartCount">0</span>)</button>
</header>

<section class="hero">
  <h2>Dein Fußball Trikot Shop</h2>
  <p>National • Vereine • Retro</p>
</section>

<div class="container">

  <h2 class="section-title">Login (Admin / Demo User)</h2>
  <div class="login-box">
    <input id="user" placeholder="Benutzername" />
    <input id="pass" type="password" placeholder="Passwort" />
    <button onclick="login()">Login</button>
    <p id="loginStatus"></p>
  </div>

  <section id="national">
    <h2 class="section-title">Produkte</h2>
    <div class="grid" id="products"></div>
  </section>

</div>

<div class="modal" id="cartModal">
  <div class="modal-content">
    <span class="close" onclick="closeCart()">X</span>
    <h2>Warenkorb</h2>
    <div id="cartItems"></div>
    <h3 id="total"></h3>
    <button onclick="checkout()">Checkout (Stripe Demo)</button>
  </div>
</div>

<footer>
  <p>© 2026 FootballJerseyONE - Alle Rechte vorbehalten</p>
</footer>

<script>
let cart = JSON.parse(localStorage.getItem('cart')) || [];

const products = [
  {name:"Deutschland Trikot", price:89.99},
  {name:"Frankreich Trikot", price:89.99},
  {name:"Real Madrid Trikot", price:94.99},
  {name:"FC Barcelona Trikot", price:94.99},
  {name:"Brasil Retro Trikot", price:109.99}
];

function renderProducts(){
  const container = document.getElementById("products");
  container.innerHTML="";
  products.forEach((p,i)=>{
    container.innerHTML += `
      <div class='card'>
        <img src='https://images.unsplash.com/photo-1518091043644-c1d4457512c6'/>
        <div class='card-content'>
          <h3>${p.name}</h3>
          <p class='price'>${p.price}€</p>
          <button onclick='addToCart(${i})'>In den Warenkorb</button>
        </div>
      </div>
    `;
  });
}

function addToCart(i){
  cart.push(products[i]);
  localStorage.setItem('cart', JSON.stringify(cart));
  updateCart();
}

function updateCart(){
  document.getElementById("cartCount").innerText = cart.length;
}

function openCart(){
  document.getElementById("cartModal").style.display="flex";
  renderCart();
}

function closeCart(){
  document.getElementById("cartModal").style.display="none";
}

function renderCart(){
  let html="";
  let total=0;
  cart.forEach(c=>{
    html+=`<p>${c.name} - ${c.price}€</p>`;
    total+=c.price;
  });
  document.getElementById("cartItems").innerHTML=html;
  document.getElementById("total").innerText="Total: "+total.toFixed(2)+"€";
}

function checkout(){
  alert("Stripe Demo Checkout - hier würde echte Zahlung stattfinden");
  cart=[];
  localStorage.removeItem('cart');
  updateCart();
  closeCart();
}

function login(){
  let u=document.getElementById("user").value;
  let p=document.getElementById("pass").value;

  if(u==="admin" && p==="1234"){
    document.getElementById("loginStatus").innerText="Admin Login erfolgreich";
  } else {
    document.getElementById("loginStatus").innerText="Login als Gast";
  }
}

updateCart();
renderProducts();
</script>

</body>
</html>
