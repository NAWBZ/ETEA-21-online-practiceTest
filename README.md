# ETEA-21-online-practiceTest
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ETEA Practice Test</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<style>
body{font-family:'Poppins',sans-serif;background:linear-gradient(135deg,#ff9a9e,#fecfef);margin:0}
.container{max-width:900px;margin:20px auto;background:#fff;padding:30px;border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,.2)}
h2{text-align:center}
.center{text-align:center}
input{width:100%;padding:12px;margin-bottom:15px;border-radius:10px;border:2px solid #ddd}
button{padding:12px;border:none;border-radius:10px;font-size:16px;font-weight:bold;cursor:pointer;margin:5px}
#startBtn{background:#2980b9;color:#fff;width:100%}
#timer{background:#e74c3c;color:#fff;padding:15px;text-align:center;border-radius:10px;margin-bottom:10px}
.stats-bar{display:flex;justify-content:space-between;background:#f8f9fa;padding:10px;border-radius:10px;margin-bottom:15px;font-weight:600;font-size:14px;border:1px solid #eee}
.option{border:2px solid #ddd;padding:15px;border-radius:10px;margin-bottom:12px;cursor:pointer}
.option.correct{background:#2980b9;color:#fff}
.option.wrong{background:#e74c3c;color:#fff}
.buttons{display:flex;justify-content:space-between}
#submitBtn{display:none;background:#27ae60;color:white;width:100%;margin-top:20px}
.subject-box{background:#f4f6f7;padding:12px;border-radius:10px;margin:8px 0}
.review-option.correct{background:#2980b9;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.review-option.wrong{background:#e74c3c;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.footer{text-align:center;color:#555;margin:20px 0;font-size:14px}
</style>
</head>
<body>

<div class="container center" id="loginDiv">
<h2>ETEA Practice Test</h2>
<input id="studentName" placeholder="Student Name">
<input id="rollNo" placeholder="Roll Number">
<button id="startBtn" onclick="startCountdown()">Start Test</button>
<div id="countdown" style="font-size:28px;color:red"></div>
</div>

<div class="container" id="quizDiv" style="display:none">
<div id="timer">Time Left: 40:00</div>
<div class="stats-bar">
    <span id="qCounter">Question: 1/100</span>
    <span id="attemptedCounter">Attempted: 0/100</span>
</div>
<div id="questionBox"></div>
<div id="optionsBox"></div>
<div class="buttons">
<button onclick="prevQ()">Previous</button>
<button onclick="skipQ()">Skip</button>
<button onclick="nextQ()" id="nextBtn" disabled>Next</button>
</div>
<button id="submitBtn" onclick="submitTest()">Submit Test</button>
</div>

<div class="container center" id="resultDiv" style="display:none">
<h2 id="resStatus"></h2>
<p id="resScore"></p>
<p id="resTime"></p>

<h3>Subject-Wise Result</h3>
<div id="subjectResult"></div>

<button onclick="showReview()">Review Test</button>
<button onclick="location.reload()">Restart</button>
</div>

<div class="container" id="reviewDiv" style="display:none">
<h2 class="center">Test Review</h2>
<div id="reviewContent"></div>
<button onclick="backToResult()">Back to Result</button>
</div>

<div class="footer">
Created by <b>MUHAMMAD FAIZAN NAWAB</b>
</div>

<script>
const questions = [
{subject:"Science", q:"Droplets formed by rain are:", o:["Solid","Liquid","Steam","All"], c:1},
{subject:"Science", q:"As we go up, temperature:", o:["Increases","No change","Drops","All possible"], c:2},
{subject:"Science", q:"Salt formation from ocean water occurs by process of:", o:["Compression","Condensation","Freezing","Evaporation"], c:3},
{subject:"Science", q:"Extraction of salt from seawater is done through:", o:["Evaporation","Condensation","Freezing","Melting"], c:0},
{subject:"Science", q:"Water droplets are formed from vapour through condensation when particles come:", o:["Farther","Closer","No change","Vibration"], c:1},
{subject:"Science", q:"Pencil is an example of:", o:["Gas","Solid","Liquid","Not a matter"], c:1},
{subject:"Science", q:"When liquid changes into gas by heat gain, it is called:", o:["Evaporation","Sublimation","Melting","Boiling"], c:0},
{subject:"Science", q:"When solid changes directly into gas by heat gain, it is called:", o:["Evaporation","Sublimation","Melting","Boiling"], c:1},
{subject:"Science", q:"Change in state of matter is called:", o:["Irreversible change","Reversible change","Both A and B","None"], c:1},
{subject:"Science", q:"Particles are brought closer and forced to remain fixed; this process is:", o:["Melting","Boiling","Sublimation","Freezing"], c:3},
{subject:"Science", q:"When ice is transformed into water, the process is called:", o:["Freezing","Boiling","Melting","All"], c:2},
{subject:"Science", q:"When water is transformed into steam, it is called:", o:["Condensation","Evaporation","Melting","All"], c:1},
{subject:"Science", q:"Water vapour changing into water droplets is called:", o:["Evaporation","Sublimation","Condensation","Freezing"], c:2},
{subject:"Science", q:"Percentage of saline water on Earth is:", o:["90%","98%","97%","100%"], c:2},
{subject:"Science", q:"Solid converting directly into gas is called:", o:["Sublimation","Evaporation","Condensation","All"], c:0},
{subject:"Science", q:"Change of state depends on heat being:", o:["Lost only","Gained only","Gained or lost","None"], c:2},
{subject:"Science", q:"Liquids do not have definite:", o:["Shape","Volume","Both A and B","None"], c:0},
{subject:"Science", q:"Mercury is a:", o:["Gas","Solid","Liquid","All"], c:2},
{subject:"Science", q:"Particles move continuously in which state:", o:["Gas","Liquid","Solid","None"], c:0},
{subject:"Science", q:"Gas does not have:", o:["Definite shape","Definite volume","Both A and B","None"], c:2},
{subject:"Science", q:"Atomic mass of helium is:", o:["5","2","4","6"], c:2},
{subject:"Science", q:"Balloon is made of material which is:", o:["Non-flexible","Flexible","Both A and B","None"], c:1},
{subject:"Science", q:"Continuous movement of particles in gas and liquid is called:", o:["Brownian motion","Only motion","Uniform motion","All"], c:0},
{subject:"Science", q:"Movement of pollen grains in water was observed by:", o:["Robert Hooke","Robert Koch","Darwin","Robert Brown"], c:3},
{subject:"Science", q:"Movement of particles from high concentration to low concentration is called:", o:["Diffusion","Sublimation","Evaporation","Compression"], c:0},
{subject:"Science", q:"Diffusion does not take place in:", o:["Gas","Liquid","Solid","All"], c:2},
{subject:"Science", q:"Solid converts into liquid by heat gain is called:", o:["Sublimation","Condensation","Melting","Evaporation"], c:2},
{subject:"Science", q:"Space occupied by an object is called:", o:["Mass","Volume","Area","Both A and B"], c:1},
{subject:"Science", q:"Metallic nail is an example of:", o:["Gas","Solid","Liquid","None"], c:1},
{subject:"Science", q:"The process of turning gas into liquid is:", o:["Melting","Evaporation","Condensation","Freezing"], c:2},
{subject:"English", q:"She usually ___ her work before sunset.", o:["finish","finishes","finished","finishing"], c:1},
{subject:"English", q:"The keys are ___ the drawer, not on the table.", o:["at","in","on","with"], c:1},
{subject:"English", q:"He ___ the office before I arrived.", o:["leave","left","had left","leaving"], c:2},
{subject:"English", q:"Which word functions as a noun in the sentence: Swimming is good for health.", o:["Good","Health","Swimming","For"], c:2},
{subject:"English", q:"We ___ waiting for the bus when it started raining.", o:["are","were","have been","had been"], c:1},
{subject:"English", q:"The teacher was angry ___ the students.", o:["at","with","on","in"], c:1},
{subject:"English", q:"Neither Ali nor his friends ___ ready for the test.", o:["is","was","are","be"], c:2},
{subject:"English", q:"Choose the correct question form:", o:["Does she plays tennis?","Did she played tennis?","Does she play tennis?","Did she plays tennis?"], c:2},
{subject:"English", q:"He has no interest ___ politics.", o:["on","at","in","with"], c:2},
{subject:"English", q:"The work ___ by the workers before evening.", o:["completes","completed","was completed","has completing"], c:2},
{subject:"English", q:"Which sentence is in present perfect tense?", o:["He writes a letter.","He wrote a letter.","He has written a letter.","He is writing a letter."], c:2},
{subject:"English", q:"Identify the adjective: She bought an expensive dress.", o:["Bought","Dress","Expensive","She"], c:2},
{subject:"English", q:"I will call you as soon as he ___ home.", o:["reach","reached","will reach","reaches"], c:3},
{subject:"English", q:"The boy was punished ___ breaking the rules.", o:["of","for","with","by"], c:1},
{subject:"English", q:"Hardly ___ the bell rang when the class ended.", o:["had I sat","I had sat","did I sit","I sat"], c:0},
{subject:"English", q:"Choose the correct passive voice: They are repairing the road.", o:["The road is repaired by them.","The road was being repaired.","The road is being repaired.","The road has repaired."], c:2},
{subject:"English", q:"She speaks ___ than her sister.", o:["more confidently","most confidently","confident","confidence"], c:0},
{subject:"English", q:"No sooner ___ the match started than it began to rain.", o:["did","has","had","was"], c:2},
{subject:"English", q:"He behaves ___ everyone politely.", o:["with","for","to","at"], c:2},
{subject:"English", q:"Identify the correct indirect speech: He said, “I am busy now.”", o:["He said that he is busy then.","He said that he was busy then.","He says that he was busy now.","He said that I was busy then."], c:1},
{subject:"English", q:"The train ___ have arrived by now.", o:["must","should","can","would"], c:0},
{subject:"English", q:"Which option is grammatically correct?", o:["She did not knew the answer.","She did not know the answer.","She does not knew the answer.","She did not knowing the answer."], c:1},
{subject:"English", q:"The man ___ wallet was stolen reported to police.", o:["who","which","whose","whom"], c:2},
{subject:"English", q:"He is ___ honest man.", o:["a","an","the","no article"], c:1},
{subject:"English", q:"Choose the correct sentence:", o:["He suggested me to go home.","He suggested that I go home.","He suggested me going home.","He suggested I to go home."], c:1},
{subject:"English", q:"She prefers tea ___ coffee.", o:["than","from","to","over"], c:2},
{subject:"English", q:"Which sentence is conditional (Type-1)?", o:["If he worked hard, he would pass.","If he works hard, he will pass.","If he had worked hard, he would have passed.","If he worked hard, he passed."], c:1},
{subject:"English", q:"The doctor advised him ___ smoking.", o:["stop","stopping","to stop","stopped"], c:2},
{subject:"English", q:"Identify the correct sentence:", o:["There is many problems.","There are much problems.","There are many problems.","There is much problem."], c:2},
{subject:"English", q:"Scarcely had she entered the room ___ everyone stood up.", o:["than","when","that","while"], c:1},
{subject:"Math", q:"Solve: 4x − 10 = 6. The value of x is:", o:["1","2","4","6"], c:1},
{subject:"Math", q:"Find x if: 5x = 40", o:["5","6","8","10"], c:2},
{subject:"Math", q:"Determine the solution of: x − 5 = 2x + 1", o:["−6","−4","4","6"], c:0},
{subject:"Math", q:"A linear equation contains a variable raised to the power of:", o:["0","1","2","3"], c:1},
{subject:"Math", q:"Which of the following represents a linear equation in one variable?", o:["x² + 3 = 0","x + 5 = 0","xy + 2 = 0","x² − 7"], c:1},
{subject:"Math", q:"If 9x − 9 = 0, then x equals:", o:["−1","0","1","9"], c:2},
{subject:"Math", q:"The solution of 2x + 3 = 11 is:", o:["2","3","4","5"], c:2},
{subject:"Math", q:"Find k if 12 = 60 ÷ k", o:["4","5","6","12"], c:1},
{subject:"Math", q:"Solve: ½x + 4 = x − 2", o:["4","8","12","16"], c:1},
{subject:"Math", q:"Determine x: 0.25x + 2 = x − 1", o:["2","3","4","5"], c:2},
{subject:"Math", q:"Five less than twice a number is 9. The number is:", o:["4","5","6","7"], c:3},
{subject:"Math", q:"If 4n − 8 = −12, then n is:", o:["−1","−2","2","4"], c:0},
{subject:"Math", q:"Solve: 5x = x + 12", o:["2","3","4","6"], c:2},
{subject:"Math", q:"The statement “x + 7 = 0” is called:", o:["Algebraic term","Algebraic expression","Algebraic equation","Algebra"], c:2},
{subject:"Math", q:"In an equation, both sides must be:", o:["unequal","balanced","random","variable"], c:1},
{subject:"Math", q:"An equation is formed when two expressions are joined by:", o:["+","−","×","="], c:3},
{subject:"Math", q:"Which has both left and right sides?", o:["Expression","Variable","Equation","Term"], c:2},
{subject:"Math", q:"“Three more than a number is 11” can be written as:", o:["x − 3 = 11","3x = 11","x + 3 = 11","11 − x = 3"], c:2},
{subject:"Math", q:"The greatest degree of a variable in a linear equation is:", o:["0","1","2","3"], c:1},
{subject:"Math", q:"Identify the algebraic equation:", o:["7x − 2","x² + 4","3a + 5 = 20","9p"], c:2},
{subject:"Math", q:"An equation is made by combining how many expressions?", o:["One","Two","Three","Four"], c:1},
{subject:"Math", q:"The symbol used to relate expressions in an equation is:", o:[">","<","=","~"], c:2},
{subject:"Math", q:"Solve: 3x − 6 = 0", o:["1","2","3","6"], c:1},
{subject:"Math", q:"Which statement must be true for an algebraic equation?", o:["It has no number","It has no variable","It contains at least one variable","It has two variables"], c:2},
{subject:"Math", q:"If twice a number equals 10, the number is:", o:["2","4","5","10"], c:2},
{subject:"Math", q:"Find the value of x: x + 9 = 3", o:["−12","−6","−3","6"], c:1},
{subject:"Math", q:"Which of the following is NOT linear?", o:["x + 4 = 7","2x − 3 = 0","x² + 5 = 0","5x = 20"], c:2},
{subject:"Math", q:"Solve: 6x = 18", o:["2","3","4","6"], c:1},
{subject:"Math", q:"The solution of an equation is the value which:", o:["changes variables","makes it incorrect","satisfies the equation","removes numbers"], c:2},
{subject:"Math", q:"If x − 4 = −9, then x equals:", o:["−13","−5","5","13"], c:1},
{subject:"Islamiat", q:"قرآنِ مجید میں سب سے پہلی وحی کس غار میں نازل ہوئی؟", o:["غارِ ثور","غارِ حرا","غارِ احد","غارِ نور"], c:1},
{subject:"Islamiat", q:"نماز میں سورۃ الفاتحہ پڑھنا فرض ہے:", o:["صرف فرض نماز میں","صرف نفل نماز میں","ہر رکعت میں","صرف پہلی رکعت میں"], c:2},
{subject:"Islamiat", q:"زکوٰۃ فرض ہونے کی بنیادی شرط کیا ہے؟", o:["عمر کا بالغ ہونا","مال کا نصاب کو پہنچنا","سال مکمل ہونا","صرف B اور C"], c:3},
{subject:"Islamiat", q:"ہجرتِ مدینہ کا مقصد کیا تھا؟", o:["تجارت","جنگ","دین کی حفاظت","سیر و سیاحت"], c:2},
{subject:"Islamiat", q:"قرآنِ مجید کو یاد کرنے والے کو کیا کہا جاتا ہے؟", o:["قاری","عالم","حافظ","فقیہ"], c:2},
{subject:"Urdu", q:"لفظ \"محنت\" کا درست متضاد کون سا ہے؟", o:["کوشش","سستی","ہمت","مشقت"], c:1},
{subject:"Urdu", q:"جملہ \"سچائی بہترین پالیسی ہے\" میں اسمِ مجرد کون سا ہے؟", o:["سچائی","بہترین","پالیسی","ہے"], c:0},
{subject:"Urdu", q:"لفظ \"لڑکیاں\" کی واحد صورت کیا ہے؟", o:["لڑکا","لڑکی","لڑکے","لڑکیوں"], c:1},
{subject:"Urdu", q:"\"وہ کھیل کر آیا\" میں \"کر\" کیا ہے؟", o:["اسم","فعل","حرف","صفت"], c:2},
{subject:"Urdu", q:"درج ذیل میں سے کون سا جملہ امر (حکم) ہے؟", o:["وہ اسکول جاتا ہے","تم کتاب پڑھ رہے ہو","کتاب کھولو","احمد نے کہا"], c:2},
];

let answers=new Array(questions.length).fill(null);
let index=0,time=2400,startTime,timerInt;

function startCountdown(){
if(!studentName.value||!rollNo.value){alert("Fill all fields");return;}
let c=5;countdown.innerText=c;
let i=setInterval(()=>{
c--;countdown.innerText=c;
if(c===0){
clearInterval(i);
loginDiv.style.display="none";
quizDiv.style.display="block";
startTime=Date.now();
loadQ();startTimer();
}},1000);
}

function updateStats() {
    let attempted = answers.filter(a => a !== null).length;
    document.getElementById("qCounter").innerText = `Question: ${index + 1}/${questions.length}`;
    document.getElementById("attemptedCounter").innerText = `Attempted: ${attempted}/${questions.length}`;
}

function loadQ(){
let q=questions[index];
questionBox.innerHTML=`<h3>${index+1}. (${q.subject}) ${q.q}</h3>`;
optionsBox.innerHTML="";
q.o.forEach((op,i)=>{
let d=document.createElement("div");
d.className="option";
if(answers[index] === i) d.classList.add(i===q.c?"correct":"wrong");
d.innerText=op;
d.onclick=()=>{
if(answers[index]!=null)return;
answers[index]=i;
d.classList.add(i===q.c?"correct":"wrong");
nextBtn.disabled=false;
updateStats();
};
optionsBox.appendChild(d);
});
nextBtn.disabled=answers[index]==null;
submitBtn.style.display=index===questions.length-1?"block":"none";
updateStats();
}

function nextQ(){if(index<questions.length-1){index++;loadQ();}}
function prevQ(){if(index>0){index--;loadQ();}}
function skipQ(){index=(index+1)%questions.length;loadQ();}

function startTimer(){
timerInt=setInterval(()=>{
let m=Math.floor(time/60),s=time%60;
timer.innerText=`Time Left: ${m}:${s<10?'0':''}${s}`;
if(time--<=0){clearInterval(timerInt);submitTest();}
},1000);
}

function submitTest(){
clearInterval(timerInt);
quizDiv.style.display="none";
resultDiv.style.display="block";
let score=0,subjectStats={};
questions.forEach((q,i)=>{
if(!subjectStats[q.subject]) subjectStats[q.subject]={total:0,correct:0};
subjectStats[q.subject].total++;
if(answers[i]===q.c){score++;subjectStats[q.subject].correct++;}
});
let pct=Math.round(score/questions.length*100);
resStatus.innerText=pct>=40?"PASSED":"FAILED";
resScore.innerText=`Overall Score: ${score}/${questions.length} (${pct}%)`;
resTime.innerText=`Time Taken: ${Math.floor((Date.now()-startTime)/60000)} minutes`;
subjectResult.innerHTML="";
for(let s in subjectStats){
let x=subjectStats[s];
subjectResult.innerHTML+=`<div class="subject-box"><b>${s}</b>: ${x.correct}/${x.total} → ${Math.round(x.correct/x.total*100)}%</div>`;
}
}

function showReview(){
resultDiv.style.display="none";
reviewDiv.style.display="block";
reviewContent.innerHTML="";
questions.forEach((q,i)=>{
let html=`<h4>${i+1}. ${q.q}</h4>`;
q.o.forEach((op,idx)=>{
let cls="review-option";
if(idx===q.c) cls+=" correct";
else if(answers[i]===idx) cls+=" wrong";
html+=`<div class="${cls}">${op}</div>`;
});
reviewContent.innerHTML+=html;
});
}

function backToResult(){
reviewDiv.style.display="none";
resultDiv.style.display="block";
}
</script>
</body>
</html>
