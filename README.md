<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Jeu d'Amour pour Nestou 💖</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<style>
@import url('https://fonts.googleapis.com/css2?family=Pacifico&family=Comfortaa&family=Indie+Flower&family=Shadows+Into+Light&display=swap');
html, body {margin:0; padding:0; overflow:hidden; font-family:'Arial', sans-serif; height:100%; width:100%; background:#ffe6f0;}
.fond-element{position:absolute; font-size:40px; pointer-events:none; user-select:none; opacity:0.15;}
h1 {font-family:'Pacifico', cursive; font-size:36px; text-align:center; margin-top:20px; color:#ff1a3c; animation:heartbeat 1s infinite;}
@keyframes heartbeat{0%,100%{transform:scale(1);}50%{transform:scale(1.2);}}
button {background:#ff4d6d; color:white; border:none; padding:12px 20px; margin:10px; font-size:18px; border-radius:10px; cursor:pointer; box-shadow:0 5px #ff1a3c; transition:0.3s;}
button:hover {background:#ff1a3c; transform:translateY(-2px); box-shadow:0 3px #b30022;}
.game-container{text-align:center; margin-top:20px;}
#message{margin-top:20px; font-size:20px; line-height:1.6; text-align:center; color:#ff1a3c; padding:20px;}
.heart, .loveWord, .flowerDrag, .chat, .emoji, .ticCell{position:absolute; font-size:28px; cursor:pointer;}
#miniGame{display:none; margin-top:20px; position:relative; width:100%; height:60vh;}
#bouquetArea{border:2px dashed #ff4d6d; width:300px; height:300px; margin:20px auto; border-radius:20px; background:#fff0f5; touch-action:none; position:relative;}
#ticTacToe{display:flex; flex-wrap:wrap; width:250px; height:250px; margin:20px auto; position:relative;}
.ticCell{border:1px solid #ff4d6d; width:80px; height:80px; display:flex; align-items:center; justify-content:center; font-size:28px; cursor:pointer; position:relative;}

/* --- Fond spécial message final --- */
.final-bg{
  position:fixed;
  top:0; left:0;
  width:100%; height:100%;
  background:linear-gradient(135deg, #ffe6f0, #ffd6e8, #fff0f5);
  overflow:hidden;
  z-index:-1;
}
.final-bg span{
  position:absolute;
  font-size:32px;
  opacity:0.25;
  animation: floatFinal 18s linear infinite;
}
@keyframes floatFinal{
  from{transform:translateY(100vh) rotate(0deg);}
  to{transform:translateY(-120vh) rotate(360deg);}
}

/* --- Scroll page finale --- */
.final-scroll{
  max-height:100vh;
  overflow-y:auto;
  padding-bottom:60px;
}
</style>
</head>
<body>

<h1>💖 Jeu d'Amour pour Nestou 💖</h1>
<div class="game-container" id="game">
  <p>Salut Nestou ! Prête pour un voyage interactif rempli d'amour et de surprises ? 😘</p>
  <button id="startBtn">Oui, je suis prête ! 💕</button>
  <button onclick="endEarly()">Je suis timide 😅</button>
</div>

<div id="message"></div>
<div id="miniGame"></div>

<audio id="heartSound" src="https://www.soundjay.com/button/sounds/button-16.mp3"></audio>
<audio id="bgMusic" src="https://www.bensound.com/bensound-music/bensound-romantic.mp3" loop></audio>

<script>
// --- Fond doux avec fleurs, papillons et chats ---
const emojis=['🌸','🌹','💐','🦋','🐱'];
for(let i=0;i<100;i++){
  const e=document.createElement('div'); e.className='fond-element';
  e.innerHTML=emojis[Math.floor(Math.random()*emojis.length)];
  e.style.left=Math.random()*100+'%';
  e.style.top=Math.random()*100+'%';
  e.style.fontSize=(20+Math.random()*30)+'px';
  document.body.appendChild(e);
}

// --- Mots flottants centrés + 3 couleurs fixes ---
const fonts=['Pacifico','Comfortaa','Indie Flower','Shadows Into Light'];
const loveWords=["Je t'aime","Mon trésor","Ma Nestou","💕","❤️","Amour éternel"];
const wordColors=["#ff4d6d","#b266ff","#ff1a3c"];

function createLoveWord(){
  const w=document.createElement('div');
  w.className='loveWord';
  w.innerHTML=loveWords[Math.floor(Math.random()*loveWords.length)];
  w.style.position='absolute';
  w.style.left=Math.random()*100+'%';
  w.style.transform='translateX(-50%)';
  w.style.fontFamily=fonts[Math.floor(Math.random()*fonts.length)];
  w.style.color=wordColors[Math.floor(Math.random()*wordColors.length)];
  w.style.opacity='0.25';
  w.style.fontSize=(18+Math.random()*20)+'px';
  w.style.bottom='-50px';
  document.body.appendChild(w);
  let bottom=-50;
  const move=setInterval(()=>{
    bottom+=1;
    w.style.bottom=bottom+'px';
    if(bottom>window.innerHeight){
      w.remove();
      clearInterval(move);
    }
  },25);
}
setInterval(createLoveWord,2000);

// --- Variables ---
const game=document.getElementById('game');
const miniGame=document.getElementById('miniGame');
const heartSound=document.getElementById('heartSound');
const bgMusic=document.getElementById('bgMusic');
let answers={};
let score=0;

// --- Musique ---
function playMusic(){bgMusic.play().catch(()=>{});}

// --- Début ---
document.getElementById('startBtn').onclick=startGame;
function endEarly(){game.innerHTML='<p>Pas de problème 😘 mais je t\'aime quand même infiniment 💖</p>';}

function startGame(){
  playMusic();
  game.innerHTML='<p>Quel est ton moment préféré ensemble ? 😏</p>'+
    '<button onclick="nextQ(1,\'Écouter Miguel\')">Écouter Miguel 🎶</button>'+
    '<button onclick="nextQ(1,\'Couchers de soleil à Deshaies\')">Couchers de soleil à Deshaies 🌅</button>'+
    '<button onclick="nextQ(1,\'Voyage à St Barth\')">Voyage à St Barth ✈️</button>'+
    '<button onclick="nextQ(1,\'Regarder films\')">Regarder des films 🎬</button>';
}

function nextQ(step,ans){
  if(step==1){answers.moment=ans;
    if(ans==='Regarder films'){
      game.innerHTML='<p>Quel film préfères-tu ?</p>'+
        '<button onclick="nextQ(2,\'Malcom and Marie\')">Malcom and Marie 🎬</button>'+
        '<button onclick="nextQ(2,\'The Eternal Sunshine of the Spotless Mind\')">Eternal Sunshine ☀️</button>'+
        '<button onclick="nextQ(2,\'Waves\')">Waves 🌊</button>';
    } else {
      game.innerHTML='<p>Quelle activité te rend le plus heureuse avec moi ? 💕</p>'+
        '<button onclick="nextQ(2,\'Construire Lego\')">Construire des Lego 🧱</button>'+
        '<button onclick="nextQ(2,\'Plage\')">Plage et baignade 🏖️</button>'+
        '<button onclick="nextQ(2,\'Sushis\')">Déguster des sushis 🍣</button>'+
        '<button onclick="nextQ(2,\'Sport\')">Faire du sport ensemble 🏃‍♀️</button>';
    }
  }
  else if(step==2){
    answers.activity=ans;
    game.innerHTML='<p>Ton surnom préféré que je t\'appelle ? 😘</p>'+
      '<button onclick="nextQ(3,\'Ma Nestou\')">Ma Nestou 💖</button>'+
      '<button onclick="nextQ(3,\'Mon trésor\')">Mon trésor 💎</button>'+
      '<button onclick="nextQ(3,\'Mon amour\')">Mon amour ❤️</button>';
  }
  else if(step==3){
    answers.surnom=ans;
    startMiniGame1();
  }
}

// --- Mini-jeu 1 : Cliquer sur les cœurs ---
function startMiniGame1(){
  game.style.display='none';
  miniGame.style.display='block';
  miniGame.innerHTML='<p>🎯 Clique sur tous les cœurs ! 15 secondes 😘</p>';
  const interval=setInterval(()=>{
    const h=document.createElement('div');
    h.className='heart';
    h.innerHTML='💖';
    h.style.left=Math.random()*(window.innerWidth-30)+'px';
    h.style.top='-50px';
    miniGame.appendChild(h);
    let top=-50;
    const move=setInterval(()=>{
      top+=2;
      h.style.top=top+'px'; 
      if(top>window.innerHeight){
        h.remove();
        clearInterval(move);
      }
    },30);
    h.onclick=h.ontouchstart=()=>{
      heartSound.play();
      h.remove();
      score++;
    };
  },500);
  setTimeout(()=>{
    clearInterval(interval);
    miniGame.innerHTML='';
    startMiniGame2();
  },15000);
}

// --- Mini-jeu 2 : Bouquet ---
function startMiniGame2(){
  miniGame.innerHTML='<p>🌸 Compose ton bouquet ! Glisse les fleurs dans le panier 🌸</p>'+
    '<div id="bouquetArea"></div>';
  const bouquetArea=document.getElementById('bouquetArea');
  const flowers=['🌸','🌹','💐'];
  flowers.forEach((f,i)=>{
    const fl=document.createElement('div');
    fl.className='flowerDrag';
    fl.innerHTML=f;
    fl.style.top=(i*60)+'px';
    fl.style.left='10px';
    fl.ontouchstart=fl.onmousedown=(e)=>{dragFlower(e,fl,bouquetArea)};
    miniGame.appendChild(fl);
  });
}
function dragFlower(e,fl,bq){
  e.preventDefault();
  const startX=e.clientX||e.touches[0].clientX;
  const startY=e.clientY||e.touches[0].clientY;
  const origX=parseInt(fl.style.left);
  const origY=parseInt(fl.style.top);
  const moveFn=(ev)=>{
    const x=(ev.clientX||ev.touches[0].clientX)-startX+origX;
    const y=(ev.clientY||ev.touches[0].clientY)-startY+origY;
    fl.style.left=x+'px';
    fl.style.top=y+'px';
  };
  const upFn=()=>{
    const flRect=fl.getBoundingClientRect();
    const bqRect=bq.getBoundingClientRect();
    if(flRect.left>bqRect.left && flRect.right<bqRect.right && flRect.top>bqRect.top && flRect.bottom<bqRect.bottom){
      fl.style.left=(bqRect.width/2-20)+'px';
      fl.style.top=(bqRect.height/2-20)+'px';
      score+=5;
    } else {
      fl.style.left=origX+'px';
      fl.style.top=origY+'px';
    }
    document.removeEventListener('mousemove',moveFn);
    document.removeEventListener('mouseup',upFn);
    document.removeEventListener('touchmove',moveFn);
    document.removeEventListener('touchend',upFn);
    if([...document.querySelectorAll('.flowerDrag')].every(f=>parseInt(f.style.left)!==10)){
      setTimeout(startMiniGame3,1500);
    }
  };
  document.addEventListener('mousemove',moveFn);
  document.addEventListener('mouseup',upFn);
  document.addEventListener('touchmove',moveFn);
  document.addEventListener('touchend',upFn);
}

// --- Mini-jeu 3 : Chatons ---
function startMiniGame3(){
  miniGame.innerHTML='<p>🐱 Clique sur tous les chatons qui tombent !</p>';
  const interval=setInterval(()=>{
    const c=document.createElement('div');
    c.className='chat';
    c.innerHTML='🐱';
    c.style.left=Math.random()*(window.innerWidth-30)+'px';
    c.style.top='-50px';
    miniGame.appendChild(c);
    let top=-50;
    const move=setInterval(()=>{
      top+=3;
      c.style.top=top+'px';
      if(top>window.innerHeight){
        c.remove();
        clearInterval(move);
      }
    },30);
    c.onclick=c.ontouchstart=()=>{
      heartSound.play();
      c.remove();
      score++;
    };
  },600);
  setTimeout(()=>{
    clearInterval(interval);
    miniGame.innerHTML='';
    startMiniGame4();
  },15000);
}

// --- Mini-jeu 4 : Emojis ---
function startMiniGame4(){
  miniGame.innerHTML='<p>⚡ Clique sur les emojis !</p>';
  const ems=['🍓','🍪','🍩','🍭'];
  const interval=setInterval(()=>{
    const e=document.createElement('div');
    e.className='emoji';
    e.innerHTML=ems[Math.floor(Math.random()*ems.length)];
    e.style.left=Math.random()*(window.innerWidth-30)+'px';
    e.style.top=Math.random()*(miniGame.offsetHeight-30)+'px';
    miniGame.appendChild(e);
    e.onclick=e.ontouchstart=()=>{
      heartSound.play();
      e.remove();
      score++;
    };
    setTimeout(()=>{e.remove();},2000);
  },400);
  setTimeout(()=>{
    clearInterval(interval);
    miniGame.innerHTML='';
    startTicTacToe();
  },15000);
}

// --- Mini-jeu 5 : Tic-Tac-Toe ---
function startTicTacToe(){
  miniGame.innerHTML='<p>❌ Tic-Tac-Toe contre moi !</p><div id="ticTacToe"></div>';
  const board=[];
  const cells=[];
  const ttt=document.getElementById('ticTacToe');
  for(let i=0;i<9;i++){
    const c=document.createElement('div');
    c.className='ticCell';
    ttt.appendChild(c);
    cells.push(c);
    board.push('');
    c.onclick=()=>{
      if(board[i]===''){
        board[i]='X';
        c.innerHTML='❌';
        checkTTT();
        setTimeout(IA,400);
      }
    };
  }
  function IA(){
    const empty=board.map((v,i)=>v===''?i:-1).filter(v=>v!==-1);
    if(empty.length>0){
      const choice=empty[Math.floor(Math.random()*empty.length)];
      board[choice]='⭕';
      cells[choice].innerHTML='⭕';
      checkTTT();
    }
  }
  function checkTTT(){
    const lines=[[0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]];
    for(const l of lines){
      if(board[l[0]] && board[l[0]]===board[l[1]] && board[l[1]]===board[l[2]]){
        miniGame.innerHTML=`<p>🏆 'Moi 🤖' gagne !</p>`;
        setTimeout(showFinal,3000);
        return;
      }
    }
    if(board.every(v=>v!=='')){
      miniGame.innerHTML=`<p>🤝 Match nul !</p>`;
      score+=10;
      setTimeout(showFinal,3000);
    }
  }
}

// --- Message final ---
function showFinal(){
  finalMessage();
}
function finalMessage(){
  miniGame.style.display='none';
  game.style.display='none';

  const bg=document.createElement('div');
  bg.className='final-bg';
  document.body.appendChild(bg);
  const deco=['💖','🌹','🌸','🦋','🐱','💕','💐'];
  for(let i=0;i<30;i++){
    const s=document.createElement('span');
    s.innerHTML=deco[Math.floor(Math.random()*deco.length)];
    s.style.left=Math.random()*100+'%';
    s.style.animationDuration=(18+Math.random()*20)+'s';
    s.style.color=wordColors[Math.floor(Math.random()*wordColors.length)];
    bg.appendChild(s);
  }

  document.body.style.overflowY='auto';

  document.getElementById('message').className='final-scroll';
  document.getElementById('message').innerHTML=`
  <p style="font-size:26px;font-family:Pacifico;">🌹 Ma Nestou 🌹</p>
  <p>Je t’aime profondément et sincèrement. Chaque moment avec toi est précieux : nos Lego, nos plages, nos couchers de soleil, nos voyages, nos sushis, nos séries… tout est plus beau avec toi.</p>
  <p>Merci pour ton amour, ta patience et ta douceur. Tu es mon bonheur, mon refuge et mon cœur.</p>
  <p style="font-size:22px;font-family:Indie Flower;">💌 Joyeuse Saint-Valentin mon amour 😘❤️</p>
  <p style="font-size:20px;font-weight:bold;">🏆 Ton score final : ${score} points 💖</p>
  <p style="margin-top:40px;">⬇️ Tout ça de points ! T'es trop forte bébé ⬇️</p>
  `;
}
</script>
</body>
</html>
