<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>3D Happy Birthday With Fireworks</title>

<style>
  :root{
    --bg1: #0f172a;
    --text: #e6eef8;
    --muted: rgba(230,238,248,0.65);
    --accent: #6ee7b7;
    --accent2: #7dd3fc;
  }

  *{box-sizing:border-box;font-family:Inter,ui-sans-serif,system-ui}
  html,body{
    height:100%;margin:0;overflow:hidden;
    background: var(--bg1);
    color:var(--text);
  }

  /* 🎆 FIREWORKS CANVAS */
  #fireworks{
    position:fixed;
    top:0;left:0;
    width:100vw;
    height:100vh;
    z-index:0;
    pointer-events:none;
  }

  .wrap{
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:40px;
    position:relative;
    z-index:5;
  }

  .card{
    width:420px;max-width:96vw;
    border-radius:18px;padding:26px;
    background: rgba(255,255,255,0.04);
    box-shadow: 0 30px 60px rgba(2,6,23,0.6);
    border:1px solid rgba(255,255,255,0.04);
  }

  form{display:grid;gap:12px}
  label{font-size:12px;color:var(--muted);margin-bottom:6px}
  input[type="text"]{
    padding:12px 14px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);
    background: rgba(255,255,255,0.03);
    color:var(--text);font-size:14px;
  }

  .btn{
    padding:11px 16px;border-radius:12px;border:0;
    background:linear-gradient(90deg,var(--accent),var(--accent2));
    color:#04263b;font-weight:700;cursor:pointer;font-size:14px;
  }

  .welcome{text-align:center;padding:24px;display:none}

  .cake-wrap{margin-top:18px;position:relative;display:flex;justify-content:center}

  /* 🔥 Candle Flame */
  .flame{
    position:absolute;
    top:-12px;
    width:22px;height:40px;
    background: radial-gradient(circle, #fff7c2, #ffcf4d, #ff7b00);
    border-radius:50% 50% 40% 40%;
    filter: drop-shadow(0 0 18px #ff9b00);
    animation: flicker .2s infinite alternate;
  }
  @keyframes flicker{
    from{transform:scale(1) translateY(0)}
    to{transform:scale(1.1) translateY(-4px)}
  }
  .off{
    opacity:0;
    filter:none;
    transition:1.2s;
  }

  .cake{width:220px;filter: drop-shadow(0 18px 30px rgba(2,6,23,0.5))}
</style>
</head>

<body>

<!-- 🎆 FIREWORKS BACKGROUND -->
<canvas id="fireworks"></canvas>

<div class="wrap">
  <div class="card">
    <form id="nameForm">
      <label>Enter your name:</label>
      <input type="text" id="username" placeholder="Your Name" required>
      <button type="submit" class="btn">Show Birthday 🎉</button>
    </form>

    <div id="welcomePanel" class="welcome">
      <h1 id="welcomeName">Happy Birthday!</h1>
      <p style="color:var(--muted)">From Jani 🎉</p>

      <div class="cake-wrap">
        <div id="flame" class="flame"></div>

        <svg class="cake" viewBox="0 0 512 512">
          <defs>
            <linearGradient id="g1" x1="0" x2="1">
              <stop offset="0" stop-color="#ffd9b3"/>
              <stop offset="1" stop-color="#ffb4a2"/>
            </linearGradient>
          </defs>

          <ellipse cx="256" cy="460" rx="180" ry="22" fill="#f3f6fb" opacity="0.6"/>
          <rect x="86" y="240" rx="24" ry="24" width="340" height="160" fill="url(#g1)"/>
          <g>
            <rect x="236" y="140" width="10" height="70" rx="5" fill="#7dd3fc"/>
            <path d="M241 128c4-10 14-16 14-26 0 10 10 16 14 26 0 10-14 20-14 20s-14-10-14-20z" fill="#ffb86b"/>
          </g>
        </svg>
      </div>
    </div>
  </div>
</div>

<script>
/* 🎆 FIREWORKS ENGINE */
const canvas = document.getElementById("fireworks");
const ctx = canvas.getContext("2d");
canvas.width = innerWidth;
canvas.height = innerHeight;

class Particle{
  constructor(x,y,color){
    this.x=x;this.y=y;
    this.color=color;
    this.radius=Math.random()*3+2;
    this.speed=Math.random()*5+2;
    this.angle=Math.random()*2*Math.PI;
    this.alpha=1;
  }
  update(){
    this.x+=Math.cos(this.angle)*this.speed;
    this.y+=Math.sin(this.angle)*this.speed;
    this.alpha-=0.02;
  }
  draw(){
    ctx.globalAlpha=this.alpha;
    ctx.beginPath();
    ctx.arc(this.x,this.y,this.radius,0,Math.PI*2);
    ctx.fillStyle=this.color;
    ctx.fill();
  }
}

let particles=[];

function fireworks(){
  requestAnimationFrame(fireworks);
  ctx.fillStyle="rgba(0,0,0,0.15)";
  ctx.fillRect(0,0,canvas.width,canvas.height);

  if(Math.random()<0.05){
    const x=Math.random()*canvas.width;
    const y=Math.random()*canvas.height*0.4;
    const colors=["#ff3c00","#ffdd00","#7dd3fc","#6ee7b7","#ff4d9d"];
    const color=colors[Math.floor(Math.random()*colors.length)];
    for(let i=0;i<30;i++) particles.push(new Particle(x,y,color));
  }

  particles.forEach((p,i)=>{
    p.update();
    p.draw();
    if(p.alpha<=0) particles.splice(i,1);
  });
}
fireworks();

/* 🎂 NAME + FLAME */
const nameForm=document.getElementById("nameForm");
const username=document.getElementById("username");
const welcomePanel=document.getElementById("welcomePanel");
const welcomeName=document.getElementById("welcomeName");
const flame=document.getElementById("flame");

nameForm.addEventListener("submit",e=>{
  e.preventDefault();
  const name=username.value.trim();
  if(!name) return;

  welcomeName.textContent=`Happy Birthday, ${name}!`;
  nameForm.style.display="none";
  welcomePanel.style.display="block";

  setTimeout(()=>{ flame.classList.add("off"); },5000);
});
</script>

</body>
</html>
