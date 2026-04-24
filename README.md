<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Crypto Lite Platform</title>

<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js"></script>

<style>
body {margin:0;font-family:Arial;background:#0b1020;color:white;}
.box {width:320px;margin:80px auto;background:#141c33;padding:20px;border-radius:10px;}
input {width:92%;padding:10px;margin:6px;border-radius:5px;border:none;}
button {width:100%;padding:10px;margin-top:6px;background:#00ffcc;border:none;font-weight:bold;cursor:pointer;}

#app {display:none;padding:20px;}
.card {background:#141c33;padding:15px;margin:10px 0;border-radius:10px;}
.green {color:#00ffcc;}
</style>
</head>

<body>

<!-- LOGIN -->
<div class="box" id="authBox">
<h2>🔐 Crypto Login</h2>
<input id="email" placeholder="Email">
<input id="password" type="password" placeholder="Password">
<button onclick="signup()">Sign Up</button>
<button onclick="login()">Login</button>
<p id="msg"></p>
</div>

<!-- APP -->
<div id="app">

<h2>🚀 Crypto Dashboard</h2>

<div class="card">
<p>💰 Balance: <span class="green" id="balance">0 USDT</span></p>
</div>

<div class="card">
<p>💳 Deposit Address:</p>
<p id="wallet">YOUR_USDT_ADDRESS</p>
<button onclick="copy()">Copy</button>
</div>

<div class="card">
<h3>📊 Marketplace</h3>
<p>Buy / Sell Crypto & Digital Products</p>
</div>

<button onclick="logout()">Logout</button>

</div>

<script>

// 🔥 FIREBASE CONFIG (vendose ti)
const firebaseConfig = {
  apiKey: "VENDOS_KETU",
  authDomain: "VENDOS_KETU",
  projectId: "VENDOS_KETU"
};

firebase.initializeApp(firebaseConfig);

const auth = firebase.auth();
const db = firebase.firestore();

// 👉 USER STATE
auth.onAuthStateChanged(user => {
  if(user){
    document.getElementById("authBox").style.display="none";
    document.getElementById("app").style.display="block";
    loadData(user.uid);
  }
});

// SIGNUP
function signup(){
  let email=document.getElementById("email").value;
  let pass=document.getElementById("password").value;

  auth.createUserWithEmailAndPassword(email,pass)
  .then(u=>{
    db.collection("users").doc(u.user.uid).set({
      balance: 0
    });
  })
  .catch(e=>msg(e.message));
}

// LOGIN
function login(){
  let email=document.getElementById("email").value;
  let pass=document.getElementById("password").value;

  auth.signInWithEmailAndPassword(email,pass)
  .catch(e=>msg(e.message));
}

// LOAD DATA
function loadData(uid){
  db.collection("users").doc(uid).get().then(doc=>{
    document.getElementById("balance").innerText =
    (doc.data().balance || 0) + " USDT";
  });
}

// COPY ADDRESS
function copy(){
  navigator.clipboard.writeText("YOUR_USDT_ADDRESS");
  alert("Copied!");
}

// LOGOUT
function logout(){
  auth.signOut();
  location.reload();
}

function msg(t){
  document.getElementById("msg").innerText=t;
}

</script>

</body>
</html>
