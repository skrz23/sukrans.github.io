<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kuis Fisika PRO</title>

<style>
body {
    margin:0;
    font-family:'Segoe UI',sans-serif;
    background:linear-gradient(135deg,#0f172a,#1e293b);
    color:white;
    display:flex;
    justify-content:center;
    align-items:center;
    height:100vh;
}

/* CARD */
.quiz-box {
    width:95%;
    max-width:550px;
    background:rgba(255,255,255,0.05);
    backdrop-filter:blur(15px);
    padding:25px;
    border-radius:20px;
    box-shadow:0 15px 40px rgba(0,0,0,0.4);
}

/* PROGRESS */
.progress {
    height:8px;
    background:#334155;
    border-radius:10px;
    overflow:hidden;
    margin-bottom:10px;
}

.progress-bar {
    height:100%;
    width:0%;
    background:linear-gradient(90deg,#6366f1,#22c55e);
    transition:0.4s;
}

/* TIMER */
.timer {
    text-align:right;
    font-size:14px;
    margin-bottom:10px;
}

/* QUESTION */
.question {
    font-size:18px;
    margin-bottom:15px;
}

/* OPTIONS */
.option {
    background:#1e293b;
    padding:12px;
    margin:8px 0;
    border-radius:10px;
    cursor:pointer;
    transition:0.3s;
}

.option:hover {
    background:#334155;
}

.correct { background:#16a34a !important; }
.wrong { background:#dc2626 !important; }

/* EXPLANATION */
.explain {
    margin-top:10px;
    padding:10px;
    background:#020617;
    border-left:4px solid #6366f1;
    border-radius:8px;
    display:none;
}

/* BUTTON */
button {
    margin-top:15px;
    padding:10px 15px;
    border:none;
    border-radius:10px;
    background:#6366f1;
    color:white;
    cursor:pointer;
}

button:hover { background:#4f46e5; }

/* SCORE */
.score {
    text-align:center;
}

.grade {
    font-size:24px;
    margin:10px 0;
}
</style>
</head>

<body>

<div class="quiz-box">

<div class="progress">
<div class="progress-bar" id="progress"></div>
</div>

<div class="timer">⏱ <span id="time">15</span>s</div>

<div id="quiz"></div>

</div>

<audio id="correctSound" src="https://www.soundjay.com/buttons/sounds/button-3.mp3"></audio>
<audio id="wrongSound" src="https://www.soundjay.com/buttons/sounds/button-10.mp3"></audio>

<script>
const quizData = [
{
q:"Kecepatan 100m dalam 10s?",
options:["5","10","20","15"],
answer:"10",
explain:"Gunakan v = s/t → 100/10 = 10 m/s"
},
{
q:"Rumus GLBB?",
options:["v=s/t","F=ma","v=v0+at","p=mv"],
answer:"v=v0+at",
explain:"Rumus kecepatan GLBB adalah v = v0 + at"
},
{
q:"Gaya (m=5, a=4)?",
options:["10","20","25","30"],
answer:"20",
explain:"F = m×a → 5×4 = 20 N"
},
{
q:"Momentum (2kg, 4m/s)?",
options:["6","8","10","12"],
answer:"8",
explain:"p = m×v → 2×4 = 8"
},
{
q:"Impuls (50N, 0.2s)?",
options:["5","10","15","20"],
answer:"10",
explain:"I = F×t → 50×0.2 = 10 Ns"
}
];

let current=0;
let score=0;
let time=15;
let timer;

function startTimer(){
time=15;
document.getElementById("time").innerText=time;

timer=setInterval(()=>{
time--;
document.getElementById("time").innerText=time;

if(time<=0){
clearInterval(timer);
nextQuestion();
}
},1000);
}

function loadQuiz(){
const data=quizData[current];
const quiz=document.getElementById("quiz");

quiz.innerHTML=`
<div class="question">${data.q}</div>
${data.options.map(o=>`<div class="option">${o}</div>`).join("")}
<div class="explain" id="explain"></div>
`;

document.querySelectorAll(".option").forEach(el=>{
el.onclick=()=>selectAnswer(el,data);
});

updateProgress();
startTimer();
}

function selectAnswer(el,data){
clearInterval(timer);
document.querySelectorAll(".option").forEach(o=>o.onclick=null);

if(el.innerText==data.answer){
el.classList.add("correct");
score++;
document.getElementById("correctSound").play();
}else{
el.classList.add("wrong");
document.getElementById("wrongSound").play();
}

const explain=document.getElementById("explain");
explain.style.display="block";
explain.innerText=data.explain;

setTimeout(nextQuestion,2000);
}

function nextQuestion(){
current++;
if(current<quizData.length){
loadQuiz();
}else{
showScore();
}
}

function updateProgress(){
document.getElementById("progress").style.width =
(current/quizData.length)*100+"%";
}

function getGrade(score,total){
let percent=(score/total)*100;
if(percent>=90) return "A 🏆";
if(percent>=75) return "B 👍";
if(percent>=60) return "C 🙂";
return "D 😅";
}

function showScore(){
localStorage.setItem("lastScore",score);

document.getElementById("quiz").innerHTML=`
<div class="score">
<h2>🎉 Hasil Kamu</h2>
<p>Skor: <b>${score}/${quizData.length}</b></p>
<div class="grade">${getGrade(score,quizData.length)}</div>
<button onclick="location.reload()">Ulangi</button>
</div>
`;

document.getElementById("progress").style.width="100%";
}

loadQuiz();
</script>

</body>
</html>
