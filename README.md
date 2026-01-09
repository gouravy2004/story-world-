# story-world-  

<html lang="en">
<head>
<meta charset="UTF-8">
<title>Gourav Yadav Story World</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
:root {
  --bg-color:#fff;
  --text-color:#111;
  --card-bg:#ffe6f0;
  --btn-bg:#ff4d6d;
  --btn-hover:#ff85b3;
}

body.dark{
  --bg-color:#111;
  --text-color:#fff;
  --card-bg:#111;
  --btn-bg:#ff4d6d;
  --btn-hover:#ff85b3;
}

body{
  margin:0;
  font-family: Arial, sans-serif;
  background: var(--bg-color);
  color: var(--text-color);
}

/* THEME TOGGLE BUTTON */
.theme-btn{
  position:fixed;
  top:10px; right:10px;
  padding:8px 15px;
  border:none;
  border-radius:20px;
  background:var(--btn-bg);
  color:#fff;
  cursor:pointer;
  z-index:10;
}

/* HEADER */
.header{
  text-align:center;
  padding:20px;
  font-size:24px;
  font-weight:bold;
  color:#ff4d6d;
}

/* CATEGORY BUTTONS */
.categories{
  display:flex;
  justify-content:center;
  gap:10px;
  margin:15px;
}

.categories button{
  padding:8px 16px;
  border:none;
  border-radius:20px;
  background:#ccc;
  color:#111;
  cursor:pointer;
  font-weight:bold;
  transition:0.3s;
}

.categories button.active{
  background:var(--btn-bg);
  color:#fff;
}

/* STORY BOX */
.story-container{
  max-width:400px;
  height:75vh;
  margin:auto;
  background: var(--card-bg);
  border-radius:20px;
  padding:20px;
  box-shadow:0 0 25px rgba(255,105,180,0.5);
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  text-align:center;
  position:relative;
  overflow:hidden;
  touch-action: pan-y;
}

/* STORY LINES */
.story-line{
  opacity:0;
  animation:fade 1s forwards;
  margin:8px 0;
  font-size:18px;
}
@keyframes fade{ to{opacity:1} }

/* PROGRESS BAR */
.progress{
  width:90%;
  height:5px;
  background:#ccc;
  border-radius:5px;
  overflow:hidden;
  margin-bottom:10px;
}
.progress-bar{
  height:100%;
  width:0%;
  background:var(--btn-bg);
  animation:load 7s linear forwards;
}
@keyframes load{ to{width:100%} }

/* DOTS */
.dots{
  display:flex;
  gap:5px;
  margin-top:10px;
}
.dot{
  width:10px; height:10px;
  background:#ccc;
  border-radius:50%;
  transition:0.3s;
}
.dot.active{
  background:var(--btn-bg);
}

/* ACTION BUTTONS */
.actions{
  display:flex;
  justify-content:center;
  gap:15px;
  margin-top:15px;
}
.actions button{
  padding:8px 16px;
  border:none;
  border-radius:20px;
  background:var(--btn-bg);
  color:#fff;
  font-weight:bold;
  cursor:pointer;
  transition:0.3s;
}
.actions button:hover{
  background:var(--btn-hover);
}

/* FOOTER */
.footer{
  text-align:center;
  padding:15px;
  font-size:16px;
  color:#ff4d6d;
}
</style>
</head>

<body class="dark">

<!-- HEADER -->
<div class="header">
  Welcome to Gourav Yadav's Story World ❤️
</div>

<button class="theme-btn" onclick="toggleTheme()">🌙 Theme</button>

<!-- CATEGORY BUTTONS -->
<div class="categories">
  <button class="active" onclick="changeCategory('love',this)">❤️ Love</button>
  <button onclick="changeCategory('sad',this)">💔 Sad</button>
  <button onclick="changeCategory('horror',this)">👻 Horror</button>
</div>

<!-- STORY BOX -->
<div class="story-container" id="storyBox">
  <div class="progress"><div class="progress-bar" id="bar"></div></div>
  <div id="storyText"></div>

  <!-- DOTS -->
  <div class="dots" id="dots"></div>

  <!-- AUDIO -->
  <audio id="voice"></audio>

  <!-- ACTIONS -->
  <div class="actions">
    <button onclick="likeStory()">❤️ Like</button>
    <button onclick="copyStory()">📋 Copy</button>
    <button onclick="toggleAudio()" id="audioBtn">▶️ Play Voice</button>
  </div>
</div>

<!-- FOOTER -->
<div class="footer">
  © 2026 Gourav Yadav. All Rights Reserved.
</div>

<script>
// STORIES DATA
const stories = {
  love:[
    ["उसकी मुस्कान में","मेरी पूरी दुनिया थी…","हर मुलाक़ात खास लगती थी…","और हर पल सिर्फ उसका…"],
    ["प्यार शब्दों से नहीं…","खामोशी से समझ आता है…","जब कोई बिना कहे","सब समझ जाए…"],
    ["तेरी आँखों में खो जाना","मेरी आदत बन गई…","हर दिन तुझसे मिलने की","उम्मीद रहती है…"],
    ["दिल ने कहा तुझे चाहूँगा…","दिमाग ने कहा संभल…","पर दिल की सुनना","हमेशा बेहतर होता है…"],
    ["तेरा हाथ पकड़े रहना","मुझे सुकून देता है…","हर तूफ़ान में तेरा साथ","मुझे मजबूत बनाता है…"],
    ["चाँदनी रात में तेरा साथ","हर सपना सच लगता है…","तेरी हँसी मेरी दुनिया है…","तेरी यादें हमेशा मेरे साथ हैं…"],
    ["हमने साथ जो सफ़र तय किया","हर मोड़ यादगार रहा…","तेरे बिना सब सूना है…","तेरी बातें दिल को छूती हैं…"],
    ["तू मेरी सोच, तू मेरी खुशियाँ…","तेरा होना मेरे लिए सब कुछ है…","हर पल तुझसे प्यार बढ़ता है…","तेरा हाथ हमेशा थामना चाहता हूँ…"]
  ],
  sad:[
    ["कुछ कहानियाँ","अधूरी रह जाती हैं…","क्योंकि वक़्त","साथ नहीं देता…"],
    ["हम हँसते बहुत हैं…","पर अंदर","सब बिखरा होता है…"],
    ["जुदाई ने सीखा दिया…","हर मुस्कान के पीछे","अकेलापन छुपा होता है…"],
    ["वो चला गया…","पर यादें अब भी साथ हैं…","हर रात बस उसका ख्याल…","संग रहती है…"],
    ["दिल टूटने का दर्द…","कभी शब्दों में नहीं आता…","बस महसूस किया जाता है…"],
    ["तन्हाई में हमने सीखा…","हर पल यादें और भी गहरी होती हैं…"],
    ["वक़्त ठहर नहीं सकता…","पर दिल में वो लम्हे हमेशा रह जाते हैं…"],
    ["कुछ रिश्ते…","बस याद बनकर रह जाते हैं…","मुलाक़ातें नहीं होती…"]
  ],
  horror:[
    ["रात के सन्नाटे में…","किसी ने मेरा नाम लिया…","पीछे मुड़ा…","लेकिन कोई नहीं था…"],
    ["आईना बोला…","वो अब यहीं है…","मैं अकेला नहीं था…","कभी था ही नहीं…"],
    ["अंधेरे कमरे में…","किसी ने धीरे से फुसफुसाया…","'तुमने देखा?'","पर कोई दिखाई नहीं दिया…"],
    ["पुराने हवेली की खिड़की…","स्वयं बंद हुई…","और एक ठंडी हवा चली…","जो सिर्फ मेरे लिए थी…"],
    ["सड़क पर अकेले चलते हुए…","कदमों की आवाजें…","पीछे से आती थीं…","पर कोई नहीं था…"],
    ["रात 12 बजे…","फोन की घंटी बजी…","लेकिन स्क्रीन खाली…","कोई कॉल नहीं आया था…"],
    ["काले पेड़ की छाया…","जैसे मेरे पीछे हिल रही थी…","मैं भागा…","पर रास्ता खत्म था…"],
    ["किसी ने कहा…","'मैं हमेशा तुम्हारे पास रहूँगा…'","और मैं अकेला था…","पर आवाज मेरे कानों में थी…"]
  ]
};

let currentCategory="love";
let index=0;

const storyText=document.getElementById("storyText");
const bar=document.getElementById("bar");
const dotsContainer=document.getElementById("dots");
const audio=document.getElementById("voice");
const audioBtn=document.getElementById("audioBtn");

// LOAD STORY
function loadStory(){
  storyText.innerHTML="";
  bar.style.animation="none";
  bar.offsetHeight;
  bar.style.animation="load 7s linear forwards";

  stories[currentCategory][index].forEach((line,i)=>{
    let div=document.createElement("div");
    div.className="story-line";
    div.style.animationDelay=`${i*0.6}s`;
    div.innerText=line;
    storyText.appendChild(div);
  });

  // Update dots
  dotsContainer.innerHTML="";
  for(let i=0;i<stories[currentCategory].length;i++){
    let d=document.createElement("div");
    d.className="dot";
    if(i===index)d.classList.add("active");
    dotsContainer.appendChild(d);
  }

  // Reset audio
  audio.pause();
  audio.src=""; // Add mp3 if available
  audioBtn.innerText="▶️ Play Voice";
}

// CATEGORY CHANGE
function changeCategory(cat,btn){
  currentCategory=cat;
  index=0;
  document.querySelectorAll(".categories button").forEach(b=>b.classList.remove("active"));
  btn.classList.add("active");
  loadStory();
}

// LIKE
function likeStory(){ alert("❤️ Story Liked!"); }

// COPY
function copyStory(){
  let text = stories[currentCategory][index].join("\n");
  navigator.clipboard.writeText(text);
  alert("📋 Story Copied!");
}

// AUDIO
function toggleAudio(){
  if(audio.paused){ audio.play(); audioBtn.innerText="⏸ Pause Voice"; }
  else{ audio.pause(); audioBtn.innerText="▶️ Play Voice"; }
}

// SWIPE (MOBILE)
let startX=0;
const storyBox=document.getElementById("storyBox");
storyBox.addEventListener("touchstart",e=>{ startX=e.touches[0].clientX; });
storyBox.addEventListener("touchmove",e=>{ e.preventDefault(); });
storyBox.addEventListener("touchend",e=>{
  let endX=e.changedTouches[0].clientX;
  let diff = startX - endX;
  if(Math.abs(diff) > 30){
    if(diff > 0) index=(index+1)%stories[currentCategory].length;
    else index=(index-1+stories[currentCategory].length)%stories[currentCategory].length;
    loadStory();
  }
});

// DESKTOP SWIPE (MOUSE)
let mouseDownX=0;
storyBox.addEventListener("mousedown", e=>{ mouseDownX = e.clientX; });
storyBox.addEventListener("mouseup", e=>{
  let diff = mouseDownX - e.clientX;
  if(Math.abs(diff)>30){
    if(diff>0) index=(index+1)%stories[currentCategory].length;
    else index=(index-1+stories[currentCategory].length)%stories[currentCategory].length;
    loadStory();
  }
});

// THEME TOGGLE
function toggleTheme(){ document.body.classList.toggle("dark"); }

// INITIAL LOAD
loadStory();
</script>
</body>
</html>
