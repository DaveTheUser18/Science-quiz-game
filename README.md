# Science-quiz-game
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Science Quiz</title>

<style>
body{
font-family:Arial;
transition:0.4s;
text-align:center;
margin:0;
padding:10px;
}

/* Light / Dark Mode */
.light{ background:#f2f2f2; color:black; }
.dark{ background:#121212; color:white; }

.container{
max-width:500px;
width:95%;
margin:20px auto;
padding:20px;
border-radius:12px;
box-shadow:0 0 10px gray;
}

/* Inputs responsive */
input{
width:100%;
padding:10px;
margin:5px 0;
box-sizing:border-box;
}

/* Kahoot style buttons */
.answer{
width:100%;
padding:15px;
margin:8px 0;
border:none;
font-size:18px;
cursor:pointer;
color:white;
border-radius:8px;
}

.red{background:#e74c3c;}
.blue{background:#3498db;}
.green{background:#2ecc71;}
.yellow{background:#f1c40f; color:black;}

img.avatar{
width:90px;
max-width:40%;
border-radius:50%;
margin:10px;
}

/* 📱 MOBILE OPTIMIZATION */
@media (max-width:600px){

h1{
font-size:24px;
}

.container{
padding:15px;
}

.answer{
font-size:16px;
padding:12px;
}

img.avatar{
width:70px;
}

}

</style>
</head>

<body class="light">

<h1>🔬 Smart Science Quiz</h1>

<button onclick="toggleMode()">🌙 Toggle Dark Mode</button>

<div class="container">

<input id="name" placeholder="Your Name">
<input id="avatar" placeholder="Avatar Image URL (optional)">
<button onclick="startGame()">Start Quiz</button>

<hr>

<div id="profile"></div>

<h2 id="question"></h2>
<div id="answers"></div>

<h3 id="timer"></h3>
<h3 id="stats"></h3>
<h3 id="badges"></h3>

<hr>

<h3>🛠 Admin Panel</h3>
<input id="adminQ" placeholder="Question">
<input id="adminA" placeholder="Answer1,Answer2,Answer3">
<input id="adminC" placeholder="Correct Answer">
<button onclick="addAdminQuestion()">Add Question</button>

</div>

<script>

let questions=[
{q:"Red planet?",a:["Mars","Earth","Venus"],c:"Mars"},
{q:"Energy stored in food?",a:["Chemical","Thermal","Light"],c:"Chemical"}
];

function smartQuestion(){
const topics=["atoms","plants","gravity","energy"];
let t=topics[Math.floor(Math.random()*topics.length)];

return {
q:"Smart Question: Which field studies "+t+"?",
a:["Science","History","Sports"],
c:"Science"
};
}

let playerName="Player";
let playerAvatar="";
let score=0;
let correctAnswers=0;
let current=0;
let timer=10;
let interval;
let gameQuestions=[];

function startGame(){

playerName=document.getElementById("name").value || "Player";
playerAvatar=document.getElementById("avatar").value;

gameQuestions=[...questions];
gameQuestions.push(smartQuestion());
gameQuestions.sort(()=>Math.random()-0.5);

score=0;
correctAnswers=0;
current=0;

showProfile();
loadQuestion();
}

function showProfile(){
let html="<h3>"+playerName+"</h3>";
if(playerAvatar){
html+=`<img class="avatar" src="${playerAvatar}">`;
}
document.getElementById("profile").innerHTML=html;
}

function startTimer(){
timer=10;
document.getElementById("timer").textContent="Time: "+timer;

interval=setInterval(()=>{
timer--;
document.getElementById("timer").textContent="Time: "+timer;

if(timer<=0){
clearInterval(interval);
current++;
loadQuestion();
}
},1000);
}

function loadQuestion(){

if(current>=gameQuestions.length){
endGame();
return;
}

let q=gameQuestions[current];
document.getElementById("question").textContent=q.q;

let colors=["red","blue","green","yellow"];
let html="";

q.a.forEach((ans,i)=>{
html+=`<button class="answer ${colors[i]}" onclick="answer('${ans}')">${ans}</button>`;
});

document.getElementById("answers").innerHTML=html;

startTimer();
}

function answer(ans){

let q=gameQuestions[current];

if(ans===q.c){
score++;
correctAnswers++;
checkBadges();
}

clearInterval(interval);
updateStats();
current++;
loadQuestion();
}

function updateStats(){
document.getElementById("stats").textContent=
"Score: "+score+" | Correct Answers: "+correctAnswers;
}

function checkBadges(){
if(correctAnswers===2){
document.getElementById("badges").textContent="🏅 Science Star Badge Earned!";
}
}

function addAdminQuestion(){
let q=document.getElementById("adminQ").value;
let a=document.getElementById("adminA").value.split(",");
let c=document.getElementById("adminC").value;

questions.push({q,a,c});
alert("Question Added!");
}

function toggleMode(){
document.body.classList.toggle("dark");
document.body.classList.toggle("light");
}

function endGame(){
document.getElementById("question").textContent="Quiz Finished!";
document.getElementById("answers").innerHTML="";
updateStats();
}

</script>

</body>
</html>
