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
    <h3>Select Exam</h3>

    <!-- National CETs -->
    <button onclick="startExam('UPCAT')">UPCAT</button>
    <button onclick="startExam('USTET')">USTET</button>
    <button onclick="startExam('ACET')">ACET</button>
    <button onclick="startExam('DCAT')">DCAT</button>
    <button onclick="startExam('PUPCET')">PUPCET</button>
    <button onclick="startExam('DOSTSEI')">DOST-SEI</button>

    <hr>

    <!-- Local Bicol Institutions -->
    <button onclick="startExam('BUCET')">BUCET</button>
    <button onclick="startExam('CSPCAT')">CSPCAT</button>
    <button onclick="startExam('LICOM')">LiCom Entrance Test</button>
    <button onclick="startExam('HTCE')">Holy Trinity College</button>
    <button onclick="startExam('PCC')">Polangui Community College</button>
  </div>

  <div id="quiz" class="hidden">

    <div class="top">
      <p id="timer">Time</p>
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

  UPCAT: [{ q:"2+2=?", choices:["3","4","5","6"], answer:1, explanation:"Basic math" }],

  USTET: [{ q:"Synonym of Happy?", choices:["Sad","Joyful","Angry","Tired"], answer:1, explanation:"Happy = joyful" }],

  ACET: [{ q:"All cats are animals. Cats are?", choices:["Plants","Animals","Cars","Objects"], answer:1, explanation:"Cats are animals" }],

  DCAT: [{ q:"10-3=?", choices:["5","6","7","8"], answer:2, explanation:"10 minus 3 = 7" }],

  PUPCET: [{ q:"Water formula?", choices:["CO2","H2O","O2","NaCl"], answer:1, explanation:"H2O is water" }],

  DOSTSEI: [
    { q:"Speed = 60km in 1hr?", choices:["30","60","90","120"], answer:1, explanation:"60/1 = 60" }
  ],

  BUCET: [{ q:"25% of 200?", choices:["25","50","75","100"], answer:1, explanation:"25% = 50" }],

  CSPCAT: [{ q:"x+8=20, x=?", choices:["10","11","12","13"], answer:2, explanation:"x=12" }],

  LICOM: [{ q:"Correct grammar?", choices:["He go","He goes","He going","He gone"], answer:1, explanation:"He goes" }],

  HTCE: [{ q:"H2O is?", choices:["Air","Water","Fire","Soil"], answer:1, explanation:"Water" }],

  PCC: [{ q:"5+9=?", choices:["12","13","14","15"], answer:2, explanation:"14" }]
};
let user = {};
let exam = "";
let index = 0;
let score = 0;
let time = 900;
let timer;

function login() {
  user.name = document.getElementById("name").value;
  user.grade = document.getElementById("grade").value;

  document.getElementById("login").classList.add("hidden");
  document.getElementById("app").classList.remove("hidden");

  document.getElementById("user").innerText =
    user.name + " - " + user.grade;
}

function startExam(e) {
  exam = e;
  index = 0;
  score = 0;
  time = 900;

  document.getElementById("menu").classList.add("hidden");
  document.getElementById("quiz").classList.remove("hidden");

  load();
  startTimer();
}

function load() {
  let q = exams[exam][index];

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
  if(i === exams[exam][index].answer) score++;
  alert(exams[exam][index].explanation);
}

function nextQuestion(){
  index++;
  if(index < exams[exam].length){
    load();
  } else {
    finish();
  }
}

function finish(){
  clearInterval(timer);

  let acc = Math.round((score / exams[exam].length)*100);

  document.getElementById("quiz").classList.add("hidden");
  document.getElementById("result").classList.remove("hidden");

  document.getElementById("final").innerText =
    `Score: ${score}/${exams[exam].length}`;

  document.getElementById("analysis").innerText =
    `Accuracy: ${acc}% | ` +
    (acc>80?"Advanced":acc>50?"Developing":"Needs Improvement");
}

function startTimer(){
  timer = setInterval(()=>{
    time--;
    document.getElementById("timer").innerText = "Time: " + time;
    if(time<=0) finish();
  },1000);
}