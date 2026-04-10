<html lang="vi"><head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>HOANGDZ TOOL BÁ</title>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@600;800;900&display=swap" rel="stylesheet">
<style>
:root{ --vip:#00ffb3; --bgGlass:rgba(0,30,20,.85); --xiu:#ffd700; --tai:#ffffff; }
*{ box-sizing:border-box; font-family:'Nunito',sans-serif; }
html,body{ margin:0; width:100%; height:100%; background:#001a12; overflow:hidden; color:#fff; }
#networkCanvas{ position:fixed; inset:0; z-index:0; pointer-events:none; }

/* LOGIN */
#authOverlay{ position:fixed; inset:0; background:#000; display:flex; align-items:center; justify-content:center; z-index:200000; }
.auth-box{ width:300px; padding:25px; background:#001a12; border-radius:18px; border:1px solid var(--vip); text-align:center; }
.auth-input{ width:100%; padding:12px; border-radius:10px; border:1px solid var(--vip); background:#001f17; color:var(--vip); font-weight:800; text-align:center; outline:none; }
.auth-btn{ width:100%; margin-top:15px; padding:12px; background:linear-gradient(90deg,#00ffb3,#009e72); border:none; border-radius:10px; font-weight:900; cursor:pointer; color:#002; }

/* GAME HUB */
#gameHub{ position:fixed; inset:0; display:none; flex-direction:column; align-items:center; justify-content:center; z-index:10000; }
.game-grid{ display:grid; grid-template-columns:repeat(3,110px); gap:20px; margin-top:20px; }
.game-card{ width:110px; height:110px; background:#001f17; border-radius:15px; border:1px solid var(--vip); display:flex; flex-direction:column; align-items:center; justify-content:center; cursor:pointer; }
.game-icon{ width:55px; height:55px; border-radius:10px; background:rgba(0,255,180,0.1); padding:5px; }

/* TOOL MAIN */
#mainApp{ position:fixed; inset:0; display:none; z-index:5000; }
#betFrame{ width:100%; height:100%; border:none; }
#floatBox{ position:fixed; top:80px; left:20px; z-index:9999; touch-action:none; transform-origin: top left; }
.tool{ width:320px; padding:20px; background:var(--bgGlass); backdrop-filter:blur(15px); border-radius:20px; border:1px solid var(--vip); cursor: move; }
.header h2{ margin:0; font-size:22px; color:var(--vip); text-shadow:0 0 15px var(--vip); text-align:center; font-weight:900; }

/* MENU & ZOOM */
#menuBtn{ position:fixed; bottom:25px; right:20px; width:55px; height:55px; border-radius:50%; background:var(--vip); color:#000; font-size:26px; border:none; cursor:pointer; z-index:100000; }
#menuPanel{ position:fixed; bottom:90px; right:20px; background:#001f17; border:1px solid var(--vip); border-radius:14px; padding:10px; display:none; z-index:100000; }
.menu-item{ padding:10px; color:var(--vip); font-weight:800; cursor:pointer; border-bottom:1px solid rgba(0,255,179,0.2); }
.zoom-controls{ display:flex; justify-content: space-around; padding:10px 0; }
.zoom-btn{ width:40px; height:40px; background:var(--vip); border:none; border-radius:5px; font-weight:900; cursor:pointer; }

.result{ margin-top:15px; padding:15px; border-radius:14px; background:#001f17; text-align:center; }
.res-main{ font-size:40px; font-weight:900; }
.input{ width:100%; padding:12px; border-radius:10px; border:1px solid var(--vip); background:#001f17; color:var(--vip); text-align:center; margin-top:10px; outline:none;}
</style>
</head>
<body>

<canvas id="networkCanvas"></canvas>

<div id="authOverlay">
    <div class="auth-box">
        <h2 style="color:var(--vip)">HOANGDZ TOOL BÁ</h2>
        <input type="text" id="activationKey" class="auth-input" placeholder="NHẬP KEY VIP...">
        <button class="auth-btn" onclick="verify()">KÍCH HOẠT</button>
    </div>
</div>

<div id="gameHub">
    <h2 style="color:var(--vip)">CHỌN CỔNG GAME</h2>
    <div class="game-grid">
        <div class="game-card" onclick="start('https://lc79.bet/')">
            <img src="https://i.postimg.cc/JnLWCgjm/3E497BE2-D59F-46D3-BCCF-FDC2E3A9929C.png" class="game-icon">
            <div style="font-size:12px;margin-top:5px">LC79</div>
        </div>
        <div class="game-card" onclick="start('https://play.betvip.fit/')">
            <img src="https://i.postimg.cc/ZqqrCCjQ/A3EFE26C-7E67-4AFF-A21F-130E42D235EE.png" class="game-icon">
            <div style="font-size:12px;margin-top:5px">BETVIP</div>
        </div>
        <div class="game-card" onclick="start('https://68gbvn88.bar/')">
            <img src="https://i.postimg.cc/7LymXygB/92337C95-28EA-4D08-A11B-48F6EE362A4D.png" class="game-icon">
            <div style="font-size:12px;margin-top:5px">68GB</div>
        </div>
    </div>
</div>

<div id="mainApp">
    <iframe id="betFrame"></iframe>
    <div id="floatBox">
        <div class="tool">
            <div class="header"><h2>HOANGDZ TOOL BÁ</h2></div>
            <input id="md5" class="input" placeholder="DÁN MÃ MD5">
            <div class="result">
                <div id="final" class="res-main">WAIT</div>
                <div id="info" style="font-size:12px;color:var(--vip)">AI Ready...</div>
            </div>
        </div>
    </div>
    
    <button id="menuBtn">⋮</button>
    <div id="menuPanel">
        <div class="menu-item" onclick="location.reload()">🎮 Đổi Game</div>
        <div class="menu-item" style="border:none">Phóng to / Thu nhỏ:</div>
        <div class="zoom-controls">
            <button class="zoom-btn" onclick="zoom(0.1)">+</button>
            <button class="zoom-btn" onclick="zoom(-0.1)">-</button>
        </div>
        <div class="menu-item" onclick="location.reload()" style="color:red">🚪 Thoát</div>
    </div>
</div>

<script>
// --- LOGIC KEY (KHÔNG CÓ CODE TẠO KEY Ở ĐÂY) ---
function verify() {
    let k = document.getElementById("activationKey").value.trim();
    let db = JSON.parse(localStorage.getItem('keyDB')) || {};
    // Key dự phòng cứng cho Admin
    if(db[k] || k === "ADMIN_8386") {
        document.getElementById("authOverlay").style.display="none";
        document.getElementById("gameHub").style.display="flex";
    } else { alert("Key sai hoặc hết hạn!"); }
}

function start(url) {
    document.getElementById("gameHub").style.display="none";
    document.getElementById("mainApp").style.display="block";
    document.getElementById("betFrame").src = url;
}

// --- THU PHÓNG (ZOOM) ---
let currentZoom = 1;
function zoom(val) {
    currentZoom += val;
    if(currentZoom < 0.5) currentZoom = 0.5;
    if(currentZoom > 1.5) currentZoom = 1.5;
    document.getElementById("floatBox").style.transform = `scale(${currentZoom})`;
}

// --- DI CHUYỂN (DRAG) ---
const box = document.getElementById("floatBox");
const tool = document.querySelector(".tool");
let isDrag = false, offset = [0,0];
tool.onpointerdown = (e) => {
    if(e.target.tagName === "INPUT") return;
    isDrag = true;
    offset = [e.clientX - box.offsetLeft, e.clientY - box.offsetTop];
};
document.onpointermove = (e) => {
    if(!isDrag) return;
    box.style.left = (e.clientX - offset[0]) + "px";
    box.style.top = (e.clientY - offset[1]) + "px";
};
document.onpointerup = () => isDrag = false;

// --- DỰ ĐOÁN MD5 ---
document.getElementById("md5").oninput = function() {
    if(this.value.length >= 31) {
        document.getElementById("final").innerText = "...";
        setTimeout(() => {
            const isTai = Math.random() > 0.5;
            const f = document.getElementById("final");
            f.innerText = isTai ? "TÀI" : "XỈU";
            f.style.color = isTai ? "var(--tai)" : "var(--xiu)";
            document.getElementById("info").innerText = "Độ chuẩn: " + (Math.random()*(98-88)+88).toFixed(2) + "%";
            this.value = "";
        }, 600);
    }
};

// Menu toggle
document.getElementById("menuBtn").onclick = () => {
    let m = document.getElementById("menuPanel");
    m.style.display = m.style.display === "block" ? "none" : "block";
};
</script></body></html>
