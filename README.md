# Sais_ExamHub
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SAIS ExamHub</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

<div id="login">
  <h1>SAIS ExamHub</h1>

  <input id="name" placeholder="Full Name">

  <select id="grade">
    <option>Grade 11</option>
    <option>Grade 12</option>
  </select>

  <button onclick="login()">Login</button>
</div>

<div id="app" class="hidden">

  <h2 id="user"></h2>

  <div id="menu">

    <h3>Select CET Exam</h3>

    <button onclick="startExam('UPCAT')">UPCAT</button>
    <button onclick="startExam('USTET')">USTET</button>
    <button onclick="startExam('ACET')">ACET</button>
    <button onclick="startExam('DCAT')">DCAT</button>
    <button onclick="startExam('PUPCET')">PUPCET</button>
    <button onclick="startExam('DOSTSEI')">DOST-SEI</button>

    <hr>

    <button onclick="startExam('BUCET')">BUCET</button>
    <button onclick="startExam('CSPCAT')">CSPCAT</button>
    <button onclick="startExam('LICOM')">LiCom</button>
    <button onclick="startExam('HTCE')">HTCE</button>
    <button onclick="startExam('PCC')">PCC</button>

  </div>

  <div id="quiz" class="hidden">
    <div class="top">
      <p id="timer">Time: 900</p>
      <p id="score">Score: 0</p>
    </div>

    <h2 id="question"></h2>
    <div id="choices"></div>

    <button onclick="nextQuestion()">Next</button>
  </div>

  <div id="result" class="hidden">
    <h2>Results</h2>
    <p id="final"></p>
    <p id="analysis"></p>
  </div>

</div>

<script src="questions.js"></script>
<script src="script.js"></script>

</body>
</html>
body {
  font-family: Arial;
  background: #8B0000;
  color: white;
  text-align: center;
  margin: 0;
  padding: 20px;
}

input, select, button {
  padding: 10px;
  margin: 5px;
  width: 80%;
}

button {
  background: white;
  color: darkred;
  border: none;
  border-radius: 5px;
  font-weight: bold;
}

.hidden {
  display: none;
}

#quiz {
  background: white;
  color: black;
  padding: 20px;
  border-radius: 10px;
}

.top {
  display: flex;
  justify-content: space-between;
}
const exams = {

UPCAT: {
  math: [
    { q:"If f(x)=x²-3x+2, f(5)-f(2)?", choices:["12","14","15","18"], answer:2, explanation:"Result = 15", topic:"Algebra", difficulty:"hard" },
    { q:"Area of circle r=7?", choices:["44π","49π","98π","154π"], answer:1, explanation:"πr²", topic:"Geometry", difficulty:"hard" },
    { q:"sin²θ + cos²θ =", choices:["0","1","2","-1"], answer:1, explanation:"Identity", topic:"Trigonometry", difficulty:"hard" },
    { q:"Mean of 2,4,6,8,10?", choices:["4","5","6","7"], answer:2, explanation:"Average", topic:"Statistics", difficulty:"hard" }
  ],

  science: [
    { q:"ATP is produced in?", choices:["Nucleus","Mitochondria","Ribosome","Cell wall"], answer:1, explanation:"Energy production", topic:"Biology", difficulty:"hard" },
    { q:"Earth liquid layer?", choices:["Crust","Mantle","Outer Core","Inner Core"], answer:2, explanation:"Outer core is liquid", topic:"Earth Science", difficulty:"hard" },
    { q:"Planet motion law?", choices:["Newton","Kepler","Ohm","Boyle"], answer:1, explanation:"Kepler laws", topic:"Astronomy", difficulty:"hard" }
  ]
},

DOSTSEI: {
  math: [
    { q:"∫2x dx =", choices:["x²","2x²","x²+C","2x+C"], answer:2, explanation:"Integral rule", topic:"Calculus", difficulty:"hard" },
    { q:"Standard deviation measures?", choices:["Center","Spread","Total","Ratio"], answer:1, explanation:"Data spread", topic:"Statistics", difficulty:"hard" }
  ],

  science: [
    { q:"Cell energy currency?", choices:["DNA","ATP","RNA","Glucose"], answer:1, explanation:"ATP", topic:"Biology", difficulty:"hard" },
    { q:"Largest planet?", choices:["Earth","Mars","Jupiter","Venus"], answer:2, explanation:"Jupiter", topic:"Astronomy", difficulty:"hard" }
  ]
},

ACET: {
  logic: [
    { q:"All A are B, no B are C. Conclusion?", choices:["Some A are C","No A are C","All C are A","Some B are C"], answer:1, explanation:"Logical exclusion", topic:"Logic", difficulty:"hard" }
  ]
},

USTET: {
  english: [
    { q:"Correct grammar:", choices:["Neither boys are","Neither boys is","Neither is boys","Neither are boy"], answer:1, explanation:"Subject singular", topic:"Grammar", difficulty:"hard" }
  ]
},

BUCET: {
  math: [
    { q:"Volume sphere r=3?", choices:["18π","27π","36π","12π"], answer:2, explanation:"4/3πr³", topic:"Geometry", difficulty:"hard" }
  ]
},

CSPCAT: {
  math: [
    { q:"Derivative of x²?", choices:["x","2x","x²","2"], answer:1, explanation:"Power rule", topic:"Calculus", difficulty:"hard" }
  ]
},

LICOM: {
  english: [
    { q:"Correct sentence:", choices:["He go","He goes","He going","He gone"], answer:1, explanation:"Grammar rule", topic:"Grammar", difficulty:"hard" }
  ]
},

HTCE: {
  science: [
    { q:"Earth rotation causes?", choices:["Seasons","Day/night","Tides","Storms"], answer:1, explanation:"Rotation effect", topic:"Earth Science", difficulty:"hard" }
  ]
},

PCC: {
  math: [
    { q:"log10(1000) =", choices:["2","3","4","5"], answer:1, explanation:"10³=1000", topic:"Logarithms", difficulty:"hard" }
  ]
}

};
let user = {};
let exam = "";
let index = 0;
let score = 0;
let timer;
let time = 900;

function login(){
  user.name = document.getElementById("name").value;
  user.grade = document.getElementById("grade").value;

  document.getElementById("login").classList.add("hidden");
  document.getElementById("app").classList.remove("hidden");

  document.getElementById("user").innerText =
    user.name + " - " + user.grade;
}

function startExam(e){
  exam = e;
  index = 0;
  score = 0;
  time = 900;

  document.getElementById("menu").classList.add("hidden");
  document.getElementById("quiz").classList.remove("hidden");

  load();
  startTimer();
}

function getSection(){
  return Object.keys(exams[exam])[0];
}

function load(){
  let section = getSection();
  let q = exams[exam][section][index];

  document.getElementById("question").innerText = q.q;

  let box = document.getElementById("choices");
  box.innerHTML = "";

  q.choices.forEach((c,i)=>{
    let b = document.createElement("button");
    b.innerText = c;
    b.onclick = ()=>check(i);
    box.appendChild(b);
  });

  document.getElementById("score").innerText = "Score: " + score;
}

function check(i){
  let section = getSection();
  let q = exams[exam][section][index];

  if(i === q.answer) score++;

  alert(q.explanation);
}

function nextQuestion(){
  index++;
  let section = getSection();

  if(index < exams[exam][section].length){
    load();
  } else {
    finish();
  }
}

function finish(){
  clearInterval(timer);

  let acc = Math.round((score / exams[exam][getSection()].length)*100);

  document.getElementById("quiz").classList.add("hidden");
  document.getElementById("result").classList.remove("hidden");

  document.getElementById("final").innerText =
    "Score: " + score;

  document.getElementById("analysis").innerText =
    "Accuracy: " + acc + "% | CET Simulation Complete";
}

function startTimer(){
  timer = setInterval(()=>{
    time--;
    document.getElementById("timer").innerText = "Time: " + time;
    if(time <= 0) finish();
  },1000);
}