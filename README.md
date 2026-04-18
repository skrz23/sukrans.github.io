<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kuis Fisika Lengkap</title>

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

.quiz-box {
    width:95%;
    max-width:650px;
    background:rgba(255,255,255,0.05);
    backdrop-filter:blur(15px);
    padding:25px;
    border-radius:20px;
}

.progress { height:8px; background:#334155; border-radius:10px; overflow:hidden; }
.progress-bar { height:100%; width:0%; background:#22c55e; transition:0.4s; }

.timer { text-align:right; margin:10px 0; }

.question { margin:15px 0; }

.option {
    background:#1e293b;
    padding:12px;
    margin:8px 0;
    border-radius:10px;
    cursor:pointer;
}

.correct { background:#16a34a !important; }
.wrong { background:#dc2626 !important; }

.explain {
    display:none;
    margin-top:10px;
    background:#020617;
    padding:10px;
    border-left:4px solid #6366f1;
}

button {
    margin-top:15px;
    padding:10px;
    border:none;
    border-radius:10px;
    background:#6366f1;
    color:white;
}
</style>
</head>

<body>

<div class="quiz-box">
<div class="progress"><div class="progress-bar" id="progress"></div></div>
<div class="timer">⏱ <span id="time">15</span>s</div>
<div id="quiz"></div>
</div>

<script>
const quizData = [

{q:"1. Kecepatan benda 100m dalam 10s?",options:["5","10","20","15"],answer:"10",explain:"v=s/t"},
{q:"2. v akhir (v0=5, a=2, t=4)?",options:["11","12","13","14"],answer:"13",explain:"v=v0+at"},
{q:"3. a jika F=20, m=5?",options:["2","3","4","5"],answer:"4",explain:"F=ma"},
{q:"4. Berat (m=10)?",options:["50","100","150","200"],answer:"100",explain:"W=mg"},
{q:"5. Gaya apung benda terapung?",options:["2","5","10","20"],answer:"5",explain:"= berat"},
{q:"6. k pegas?",options:["20","40","60","80"],answer:"40",explain:"F=kx"},
{q:"7. Momentum (2,4)?",options:["6","8","10","12"],answer:"8",explain:"p=mv"},
{q:"8. Impuls (50,0.2)?",options:["5","10","15","20"],answer:"10",explain:"I=Ft"},
{q:"9. Massa jenis?",options:["1000","1500","2000","2500"],answer:"2000",explain:"ρ=m/V"},

{q:"17. Gaya gesek (μ=0.2,m=20)?",options:["20","40","60","80"],answer:"40",explain:"f=μmg"},
{q:"18. Hidrolik?",options:["100","150","200","250"],answer:"200",explain:"F2=F1 A2/A1"},
{q:"19. Periode 20Hz?",options:["0.02","0.05","0.1","0.5"],answer:"0.05",explain:"T=1/f"},
{q:"20. Intensitas ambang?",options:["1e-10","1e-11","1e-12","1e-13"],answer:"1e-12",explain:"rumus desibel"},
{q:"21. Energi Einstein?",options:["1e16","4.5e16","9e16","3e16"],answer:"4.5e16",explain:"E=mc²"},
{q:"22. Trafo?",options:["22","44","66","88"],answer:"44",explain:"perbandingan lilitan"},
{q:"23. Energi jatuh?",options:["200","300","400","500"],answer:"400",explain:"mgh"},
{q:"24. Gaya apung bergantung?",options:["massa","volume","massa jenis","luas"],answer:"massa jenis",explain:"fluida"},
{q:"25. Pegas?",options:["3","4","5","6"],answer:"5",explain:"perbandingan"},
{q:"26. v melingkar?",options:["2","5","10","20"],answer:"5",explain:"v=ωr"},

{q:"35. Pegas seri?",options:["50","100","150","200"],answer:"100",explain:"1/k"},
{q:"36. Gaya rata-rata?",options:["-10","-20","-25","-30"],answer:"-25",explain:"F=ma"},
{q:"37. Bidang miring?",options:["3","5","7","10"],answer:"5",explain:"sin 30"},
{q:"38. Energi kinetik?",options:["36","72","100","144"],answer:"72",explain:"½mv²"},
{q:"39. Trafo lilitan?",options:["25","50","75","100"],answer:"50",explain:"rasio"},
{q:"40. Impuls tumbukan?",options:["-24","-36","-48","-60"],answer:"-48",explain:"Δp"},
{q:"41. Torricelli?",options:["10","15","20","25"],answer:"20",explain:"√2gh"},
{q:"42. Energi potensial?",options:["300","350","400","450"],answer:"350",explain:"mgh"},
{q:"43. Trafo?",options:["10","20","25","30"],answer:"25",explain:"rasio"}

];

let current=0, score=0, time=15, timer;

function startTimer(){
time=15;
document.getElementById("time").innerText=time;
timer=setInterval(()=>{
time--;
document.getElementById("time").innerText=time;
if(time<=0){ clearInterval(timer); nextQuestion(); }
},1000);
}

function loadQuiz(){
const d=quizData[current];
const quiz=document.getElementById("quiz");

quiz.innerHTML=`
<div class="question">${d.q}</div>
${d.options.map(o=>`<div class="option">${o}</div>`).join("")}
<div class="explain" id="explain"></div>
`;

document.querySelectorAll(".option").forEach(el=>{
el.onclick=()=>selectAnswer(el,d);
});

document.getElementById("progress").style.width=(current/quizData.length)*100+"%";
startTimer();
}

function selectAnswer(el,d){
clearInterval(timer);
document.querySelectorAll(".option").forEach(o=>o.onclick=null);

if(el.innerText==d.answer){ el.classList.add("correct"); score++; }
else el.classList.add("wrong");

const ex=document.getElementById("explain");
ex.style.display="block";
ex.innerText=d.explain;

setTimeout(nextQuestion,1500);
}

function nextQuestion(){
current++;
if(current<quizData.length) loadQuiz();
else showScore();
}

function showScore(){
document.getElementById("quiz").innerHTML=`
<h2>🎉 Skor: ${score}/${quizData.length}</h2>
<button onclick="location.reload()">Ulangi</button>
`;
document.getElementById("progress").style.width="100%";
}

loadQuiz();
</script>

</body>
</html>
