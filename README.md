# Smart-Kisan-AI-Modified
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart Kisan</title>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',sans-serif}
body{background:#f5f7fa}

/* AUTH */
#authPage{height:100vh;display:flex;justify-content:center;align-items:center;background:linear-gradient(135deg,#1b5e20,#4caf50)}
.auth-box{background:#fff;padding:35px;border-radius:20px;width:320px;text-align:center;box-shadow:0 15px 35px rgba(0,0,0,0.25)}
.auth-box input{width:100%;padding:12px;margin:10px 0;border-radius:10px;border:1px solid #ccc}
.auth-box button{width:100%;padding:12px;background:#2e7d32;color:#fff;border:none;border-radius:10px;cursor:pointer}
.switch,.forgot{color:#1976d2;cursor:pointer;margin-top:10px;display:block}

/* DASHBOARD */
#dashboard{display:none;height:100vh;display:flex}

/* SIDEBAR */
.sidebar{width:240px;background:#1b5e20;color:white;padding:20px;display:flex;flex-direction:column}
.sidebar h2{margin-bottom:30px}
.sidebar button{background:none;border:none;color:white;text-align:left;padding:12px;font-size:16px;cursor:pointer;border-radius:8px}
.sidebar button:hover{background:#2e7d32}

/* MAIN */
.main-content{flex:1;display:flex;flex-direction:column}

.topbar{background:white;padding:15px 25px;display:flex;justify-content:space-between;align-items:center;box-shadow:0 5px 10px rgba(0,0,0,0.05)}

/* CENTER SCAN AREA */
.center-area{
 flex:1;
 display:flex;
 flex-direction:column;
 justify-content:center;
 align-items:center;
}

.scan-btn{
 padding:30px 60px;
 font-size:24px;
 border:none;
 border-radius:60px;
 background:linear-gradient(45deg,#ff9800,#ff5722);
 color:white;
 cursor:pointer;
 box-shadow:0 15px 30px rgba(0,0,0,0.2);
}

.icons-row{
 display:flex;
 gap:20px;
 margin-top:25px;
}

.icon-card{
 background:white;
 padding:15px 20px;
 border-radius:15px;
 box-shadow:0 5px 12px rgba(0,0,0,0.1);
 font-size:14px;
 cursor:pointer;
 text-align:center;
}

/* INFO CARDS */
.dashboard-grid{
 display:grid;
 grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
 gap:20px;
 padding:25px
}

.card{background:white;padding:20px;border-radius:15px;box-shadow:0 10px 20px rgba(0,0,0,0.08)}

footer{text-align:center;padding:15px;background:#1b5e20;color:white}
</style>
</head>

<body>

<!-- AUTH -->
<div id="authPage">
 <div class="auth-box">
  <h2 id="formTitle">Login</h2>
  <input type="text" id="email" placeholder="Email">
  <input type="password" id="password" placeholder="Password">
  <button onclick="handleAuth()">Login</button>
  <span class="forgot" onclick="forgotPassword()">Forgot Password?</span>
  <span class="switch" onclick="toggleForm()" id="toggleText">Don't have account? Sign Up</span>
 </div>
</div>

<!-- DASHBOARD -->
<div id="dashboard">

<div class="sidebar">
 <h2>🌿 Smart Kisan</h2>
 <button>🏠 Dashboard</button>
 <button>👤 Profile</button>
 <button>📊 History</button>
 <button onclick="logout()">🚪 Logout</button>
</div>

<div class="main-content">

<div class="topbar">
 <h3>Welcome Farmer 👋</h3>
 <div>
  📍 <span id="location">Loading...</span> |
  🌡 <span id="temp">--°C</span>
 </div>
</div>

<!-- CENTER SCAN -->
<div class="center-area">
 <button class="scan-btn" onclick="scanCrop()">📷 Scan Crop</button>
 <input type="file" id="fileInput" hidden onchange="processImage()">

 <div class="icons-row">
  <div class="icon-card">🌱 Health</div>
  <div class="icon-card">🌦 Weather</div>
  <div class="icon-card">⚠ Alerts</div>
  <div class="icon-card">📋 Tasks</div>
 </div>
</div>

<!-- DATA CARDS -->
<div class="dashboard-grid">
 <div class="card"><h3>Field Health</h3><p id="health">-</p></div>
 <div class="card"><h3>Soil & Weather</h3><p id="soil">-</p></div>
 <div class="card"><h3>Alerts</h3><p id="alerts">-</p></div>
 <div class="card"><h3>Tasks</h3><p id="tasks">-</p></div>
</div>

<footer>
 📞 +91 9876543210 | 📧 smartkisan@email.com
</footer>

</div>
</div>

<script>
let isLogin=true;
function toggleForm(){isLogin=!isLogin}

function handleAuth(){
 let email=document.getElementById("email").value.trim();
 let pass=document.getElementById("password").value.trim();
 if(!email||!pass){alert("Fill all fields");return}
 startApp();
}

function forgotPassword(){alert("Demo reset")}

function startApp(){
 document.getElementById("authPage").style.display="none";
 document.getElementById("dashboard").style.display="flex";
 getLocation();
}

function logout(){location.reload()}

function getLocation(){
 if(navigator.geolocation){
  navigator.geolocation.getCurrentPosition(pos=>{
   let lat=pos.coords.latitude;
   let lon=pos.coords.longitude;

   fetch(`https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true`)
   .then(r=>r.json())
   .then(d=>{document.getElementById("temp").innerText=d.current_weather.temperature+"°C"});

   fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${lat}&lon=${lon}`)
   .then(r=>r.json())
   .then(d=>{document.getElementById("location").innerText=(d.address.city||d.address.village||"Area")});
  });
 }
}

function scanCrop(){document.getElementById("fileInput").click()}

function processImage(){
 document.getElementById("health").innerText="Healthy (85%)";
 document.getElementById("soil").innerText="Moisture medium";
 document.getElementById("alerts").innerText="Pest risk detected";
 document.getElementById("tasks").innerText="Irrigation in 2 days";
}
</script>

</body>
</html>
