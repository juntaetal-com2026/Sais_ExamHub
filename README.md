# Sais_ExamHub
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SAIS ExamHub</title>
  <link rel="stylesheet" href="style.css">
</head>

<body>

<div id="loginPage">
  <h1>SAIS ExamHub</h1>

  <input id="name" placeholder="Full Name">
  
  <select id="grade">
    <option>Grade 11</option>
    <option>Grade 12</option>
  </select>

  <button onclick="login()">Login</button>
</div>

<div id="mainApp" class="hidden">

  <h2 id="userDisplay"></h2>

  <div id="menu">
    <h3>Select CET</h3>
    <button onclick="startExam('UPCAT')">UPCAT</button>
    <button onclick="startExam('USTET')">USTET</button>
    <button onclick="startExam('ACET')">ACET</button>
    <button onclick="startExam('DCAT')">DCAT</button>
    <button onclick="startExam('PUPCET')">PUPCET</button>
  </div>

  <div id="quiz" class="hidden">

    <div class="topBar">
      <p id="timer">Time: 0</p>
      <p id="score">Score: 0</p>
    </div>

    <h2 id="question"></h2>
    <div id="choices"></div>

    <button onclick="nextQuestion()">Next</button>
  </div>

  <div id="result" class="hidden">
    <h2>Results</h2>
    <p id="finalScore"></p>
    <p id="analytics"></p>
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
  border-radius: 5px;
  border: none;
}

button {
  background: white;
  color: darkred;
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
  margin-top: 10px;
}

.topBar {
  display: flex;
  justify-content: space-between;
}
const exams = {
  UPCAT: [
    {
      q: "If x + 10 = 25, what is x?",
      choices: ["10","15","20","25"],
      answer: 1,
      explanation: "25 - 10 = 15"
    },
    {
      q: "What is the capital of the Philippines?",
      choices: ["Cebu","Manila","Davao","Baguio"],
      answer: 1,
      explanation: "Manila is the capital."
    }
  ],

  USTET: [
    {
      q: "Synonym of 'rapid'?",
      choices: ["slow","fast","weak","late"],
      answer: 1,
      explanation: "Rapid means fast."
    }
  ],

  ACET: [
    {
      q: "Logical: All cats are animals. Some animals are pets. Cats are?",
      choices: ["plants","animals","cars","minerals"],
      answer: 1,
      explanation: "Cats are animals."
    }
  ],

  DCAT: [
    {
      q: "2x + 3 = 7. x = ?",
      choices: ["1","2","3","4"],
      answer: 1,
      explanation: "2(2)+3=7"
    }
  ],

  PUPCET: [
    {
      q: "Water formula?",
      choices: ["CO2","H2O","O2","NaCl"],
      answer: 1,
      explanation: "H2O is water."
    }
  ]
};
let user = {};
let currentExam = "";
let index = 0;
let score = 0;
let startTime;
let timer;
let seconds = 900; // 15 minutes per CET (recommended)

function login() {
  user.name = document.getElementById("name").value;
  user.grade = document.getElementById("grade").value;

  document.getElementById("loginPage").classList.add("hidden");
  document.getElementById("mainApp").classList.remove("hidden");

  document.getElementById("userDisplay").innerText =
    user.name + " (" + user.grade + ")";
}

function startExam(exam) {
  currentExam = exam;
  index = 0;
  score = 0;
  startTime = Date.now();
  seconds = 900;

  document.getElementById("menu").classList.add("hidden");
  document.getElementById("quiz").classList.remove("hidden");

  loadQuestion();
  startTimer();
}

function loadQuestion() {
  let q = exams[currentExam][index];

  document.getElementById("question").innerText = q.q;

  let container = document.getElementById("choices");
  container.innerHTML = "";

  q.choices.forEach((c, i) => {
    let btn = document.createElement("button");
    btn.innerText = c;
    btn.onclick = () => checkAnswer(i);
    container.appendChild(btn);
  });

  document.getElementById("score").innerText = "Score: " + score;
}

function checkAnswer(i) {
  let q = exams[currentExam][index];

  if (i === q.answer) score++;

  alert(q.explanation);
}

function nextQuestion() {
  index++;

  if (index < exams[currentExam].length) {
    loadQuestion();
  } else {
    finishExam();
  }
}

function finishExam() {
  clearInterval(timer);

  let timeUsed = 900 - seconds;
  let accuracy = Math.round((score / exams[currentExam].length) * 100);

  document.getElementById("quiz").classList.add("hidden");
  document.getElementById("result").classList.remove("hidden");

  let level =
    accuracy >= 80 ? "Advanced" :
    accuracy >= 50 ? "Developing" : "Needs Improvement";

  document.getElementById("finalScore").innerText =
    `Score: ${score}/${exams[currentExam].length}`;

  document.getElementById("analytics").innerText =
    `Accuracy: ${accuracy}% | Time: ${timeUsed}s | Proficiency: ${level}`;
}

function startTimer() {
  timer = setInterval(() => {
    seconds--;

    document.getElementById("timer").innerText =
      "Time: " + seconds;

    if (seconds <= 0) finishExam();
  }, 1000);
}