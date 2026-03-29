<!DOCTYPE html>
<html>
<head>
  <title>Amazon Premium Clone</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">

<style>

/* RESET */
body { margin:0; font-family:Arial; background:#eaeded; }

/* NAVBAR */
.navbar {
  display:flex;
  align-items:center;
  justify-content:space-between;
  background:#131921;
  color:white;
  padding:10px 15px;
}

.logo { font-size:20px; font-weight:bold; }

.search-box {
  width:40%;
  padding:8px;
  border:none;
  border-radius:4px;
}

.nav-icons span {
  margin-left:15px;
  cursor:pointer;
}

/* BANNER */
.banner {
  background:url("https://via.placeholder.com/1200x300") no-repeat center;
  background-size:cover;
  height:250px;
}

/* PRODUCTS */
.products {
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
  gap:15px;
  padding:20px;
}

.card {
  background:white;
  padding:10px;
  border-radius:10px;
  transition:0.3s;
}

.card:hover { transform:scale(1.05); }

.card img {
  width:100%;
  height:180px;
  object-fit:cover;
}

.price { color:green; font-weight:bold; }

.rating { color:orange; }

/* BUTTON */
button {
  background:#ffd814;
  border:none;
  padding:8px;
  margin-top:5px;
  cursor:pointer;
  border-radius:5px;
}

/* CART DRAWER */
.cart {
  position:fixed;
  right:-300px;
  top:0;
  width:300px;
  height:100%;
  background:white;
  padding:15px;
  transition:0.3s;
  overflow:auto;
}

.cart.open { right:0; }

/* MOBILE */
@media(max-width:600px){
  .search-box { width:60%; }
}

</style>
</head>

<body>

<!-- NAVBAR -->
<div class="navbar">
  <div class="logo">🛒 Amazon</div>
  <input class="search-box" id="search" placeholder="Search products">
  <div class="nav-icons">
    <span onclick="toggleCart()">🛍️ (<span id="count">0</span>)</span>
  </div>
</div>

<!-- BANNER -->
<div class="banner"></div>

<!-- PRODUCTS -->
<div class="products" id="products"></div>

<!-- CART -->
<div class="cart" id="cartBox">
  <h3>Your Cart</h3>
  <div id="cartItems"></div>
  <h4>Total ₹<span id="total"></span></h4>
</div>

<script>

// PRODUCTS DATA
const products = [
 {id:1,name:"iPhone",price:70000,img:"https://via.placeholder.com/200",rating:"⭐⭐⭐⭐"},
 {id:2,name:"Shoes",price:1500,img:"https://via.placeholder.com/200",rating:"⭐⭐⭐"},
 {id:3,name:"Watch",price:2500,img:"https://via.placeholder.com/200",rating:"⭐⭐⭐⭐"},
 {id:4,name:"Laptop",price:55000,img:"https://via.placeholder.com/200",rating:"⭐⭐⭐⭐⭐"},
];

// STORAGE
let cart = JSON.parse(localStorage.getItem("cart")) || [];

// DISPLAY PRODUCTS
function showProducts(list=products){
 let html="";
 list.forEach(p=>{
  html+=`
  <div class="card">
    <img src="${p.img}">
    <h3>${p.name}</h3>
    <p class="price">₹${p.price}</p>
    <div class="rating">${p.rating}</div>
    <button onclick="addCart(${p.id})">Add to Cart</button>
  </div>`;
 });
 document.getElementById("products").innerHTML=html;
}

// ADD CART
function addCart(id){
 let item=products.find(p=>p.id==id);
 let exist=cart.find(c=>c.id==id);
 if(exist) exist.qty++;
 else cart.push({...item,qty:1});
 save();
}

// TOGGLE CART
function toggleCart(){
 document.getElementById("cartBox").classList.toggle("open");
 showCart();
}

// SHOW CART
function showCart(){
 let html="", total=0;
 cart.forEach((c,i)=>{
  total+=c.price*c.qty;
  html+=`
  <p>${c.name} x${c.qty} = ₹${c.price*c.qty}</p>`;
 });
 document.getElementById("cartItems").innerHTML=html;
 document.getElementById("total").innerText=total;
}

// SEARCH
document.getElementById("search").oninput=function(){
 let val=this.value.toLowerCase();
 let f=products.filter(p=>p.name.toLowerCase().includes(val));
 showProducts(f);
}

// SAVE
function save(){
 localStorage.setItem("cart",JSON.stringify(cart));
 document.getElementById("count").innerText=cart.length;
}

// INIT
showProducts();
save();

</script>

</body>
</html>
