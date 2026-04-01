<!-- =========================
FILE: index.html (STARTSEITE)
========================= -->
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>FootballJerseyONE</title>
<style>
body{margin:0;font-family:Arial;background:#0f172a;color:white;}
header{padding:60px;text-align:center;background:radial-gradient(circle,#1e293b,#0f172a);}
.logo{font-size:50px;background:white;color:#0f172a;width:100px;height:100px;border-radius:50%;display:flex;align-items:center;justify-content:center;margin:0 auto 20px;}
.btn{padding:12px 18px;background:#22c55e;border:none;color:white;border-radius:10px;cursor:pointer;}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:20px;padding:20px;}
.card{background:#111827;padding:15px;border-radius:15px;cursor:pointer;}
.card img{width:100%;height:180px;object-fit:cover;border-radius:10px;}
.price{font-weight:bold;margin-top:10px;}
nav{position:sticky;top:0;background:#0b1220;padding:15px;display:flex;justify-content:space-between;}
</style>
</head>
<body>

<header>
  <div class="logo">⚽</div>
  <h1>FootballJerseyONE</h1>
  <p>National • Vereine • Retro</p>
  <button class="btn" onclick="document.getElementById('shop').scrollIntoView({behavior:'smooth'})">Shop</button>
</header>

<nav>
  <div><b>FootballJerseyONE</b></div>
  <div>🛒 <a href="checkout.html" style="color:white">Warenkorb</a></div>
</nav>

<div id="shop">
  <h2 style="padding:20px">Produkte</h2>
  <div class="grid" id="grid"></div>
</div>

<script>
const products=[
  {id:1,name:"Deutschland Heim",price:89,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
  {id:2,name:"Brasilien Heim",price:89,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},
  {id:3,name:"Real Madrid",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
  {id:4,name:"FC Barcelona",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},
  {id:5,name:"Retro 1990",price:120,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"}
];

function addToCart(id){
  let cart=JSON.parse(localStorage.getItem("cart")||"[]");
  cart.push(products.find(p=>p.id===id));
  localStorage.setItem("cart",JSON.stringify(cart));
  alert("Hinzugefügt!");
}

function openProduct(id){
  window.location.href="product.html?id="+id;
}

document.getElementById("grid").innerHTML=products.map(p=>`
  <div class='card' onclick='openProduct(${p.id})'>
    <img src='${p.img}' />
    <h3>${p.name}</h3>
    <div class='price'>${p.price}€</div>
    <button class='btn' onclick='event.stopPropagation();addToCart(${p.id})'>In den Warenkorb</button>
  </div>
`).join('');
</script>

</body>
</html>


<!-- =========================
FILE: product.html (PRODUKTSEITE)
========================= -->
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Produkt</title>
<style>
body{font-family:Arial;background:#0f172a;color:white;text-align:center;}
img{width:300px;border-radius:15px;margin-top:40px;}
.btn{padding:12px;background:#22c55e;border:none;color:white;border-radius:10px;cursor:pointer;}
</style>
</head>
<body>

<h1 id="name"></h1>
<img id="img" />
<h2 id="price"></h2>

<button class="btn" onclick="add()">In den Warenkorb</button>
<br><br>
<a href="index.html" style="color:white">Zurück</a>

<script>
const products=[
  {id:1,name:"Deutschland Heim",price:89,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"},
  {id:2,name:"Brasilien Heim",price:89,img:"https://images.unsplash.com/photo-1518091043644-c1d4457512c6"},
  {id:3,name:"Real Madrid",price:94,img:"https://images.unsplash.com/photo-1517649763962-0c623066013b"},
  {id:4,name:"FC Barcelona",price:94,img:"https://images.unsplash.com/photo-1508098682722-e99c43a406b2"},
  {id:5,name:"Retro 1990",price:120,img:"https://images.unsplash.com/photo-1521412644187-c49fa049e84d"}
];

const id=new URLSearchParams(window.location.search).get("id");
const p=products.find(x=>x.id==id);

document.getElementById("name").innerText=p.name;
document.getElementById("img").src=p.img;
document.getElementById("price").innerText=p.price+"€";

function add(){
  let cart=JSON.parse(localStorage.getItem("cart")||"[]");
  cart.push(p);
  localStorage.setItem("cart",JSON.stringify(cart));
  alert("Hinzugefügt!");
}
</script>

</body>
</html>


<!-- =========================
FILE: checkout.html (WARENKORB + PAYPAL)
========================= -->
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Checkout</title>
<script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=EUR"></script>
<style>
body{font-family:Arial;background:#0f172a;color:white;padding:20px;}
.item{background:#111827;padding:10px;margin:10px;border-radius:10px;}
.btn{padding:10px;background:red;border:none;color:white;border-radius:10px;cursor:pointer;}
</style>
</head>
<body>

<h1>Warenkorb</h1>
<div id="cart"></div>
<h2 id="total"></h2>

<div id="paypal"></div>

<script>
let cart=JSON.parse(localStorage.getItem("cart")||"[]");

function render(){
  let total=0;
  document.getElementById("cart").innerHTML=cart.map((i,index)=>{
    total+=i.price;
    return `<div class='item'>${i.name} - ${i.price}€ <button class='btn' onclick='remove(${index})'>X</button></div>`;
  }).join('');

  document.getElementById("total").innerText="Total: "+total+"€";

  if(total>0){
    paypal.Buttons({
      createOrder:(d,a)=>a.order.create({purchase_units:[{amount:{value:total.toFixed(2)}}]}),
      onApprove:(d,a)=>a.order.capture().then(()=>{
        alert("Bezahlt!");
        localStorage.removeItem("cart");
        location.reload();
      })
    }).render('#paypal');
  }
}

function remove(i){
  cart.splice(i,1);
  localStorage.setItem("cart",JSON.stringify(cart));
  location.reload();
}

render();
</script>

</body>
</html>
