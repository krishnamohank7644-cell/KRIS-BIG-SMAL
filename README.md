# KRIS-BIG-SMAL

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>KRISH Colour Game</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#101322;color:white;font-family:Arial;text-align:center}
header{padding:20px;background:#191e35;color:#ffd45c;font-size:27px;font-weight:bold}
main{max-width:440px;margin:auto;padding:18px}
.card{background:#1c223b;padding:20px;border-radius:18px;margin:15px 0}
#timer{font-size:38px;color:#ffd45c;font-weight:bold}
#round{color:#b9c5e8}
.buttons{display:flex;gap:10px;justify-content:center;flex-wrap:wrap}
button{border:0;border-radius:12px;padding:15px 18px;color:white;font-size:16px;font-weight:bold;cursor:pointer}
.red{background:#ed424e}.green{background:#16a875}.violet{background:#8847ed}
#points{font-size:25px;color:#65e6a1}
#message{min-height:25px;color:#ffd45c}
.history{padding:10px;border-bottom:1px solid #353c58}
small{color:#b9c5e8}
</style>
</head>
<body>
<header>👑 KRISH GAME</header>
<main>
  <div class="card">
    <small>DEMO MODE • VIRTUAL POINTS ONLY</small>
    <h3>Available Points</h3>
    <div id="points">1000</div>
  </div>

  <div class="card">
    <div id="round">Round 1</div>
    <div id="timer">15</div>
    <p>Choose a colour before the timer ends.</p>
    <div class="buttons">
      <button class="red" onclick="choose('Red')">RED</button>
      <button class="green" onclick="choose('Green')">GREEN</button>
      <button class="violet" onclick="choose('Violet')">VIOLET</button>
    </div>
    <p id="message">Choose your colour!</p>
    <small>Each choice costs 10 virtual points.</small>
  </div>

  <div class="card">
    <h3>📜 Result History</h3>
    <div id="history">No results yet</div>
  </div>
  <button onclick="resetGame()" style="background:#414967">Reset Demo</button>
</main>

<script>
let points = 1000;
let round = 1;
let timer = 15;
let choice = null;
let locked = false;
let history = [];

const colours = ["Red","Green","Violet"];

function render(){
  document.getElementById("points").textContent = points;
  document.getElementById("round").textContent = "Round " + round;
  document.getElementById("timer").textContent = timer;
}

function choose(colour){
  if(locked || choice) return;
  if(points < 10){
    document.getElementById("message").textContent =
      "Not enough demo points. Reset the demo.";
    return;
  }
  points -= 10;
  choice = colour;
  document.getElementById("message").textContent =
    "Your choice: " + colour;
  render();
}

function finishRound(){
  locked = true;
  const result = colours[Math.floor(Math.random()*colours.length)];
  let message = "Result: " + result + ". ";

  if(choice === result){
    points += 25;
    message += "You won 25 points!";
  }else if(choice){
    message += "Try again next round.";
  }else{
    message += "No colour selected.";
  }

  document.getElementById("message").textContent = message;
  history.unshift("Round " + round + " — " + result);
  history = history.slice(0,8);
  document.getElementById("history").innerHTML =
    history.map(item => '<div class="history">'+item+'</div>').join("");

  points = Math.max(0, points);
  render();

  setTimeout(()=>{
    round++;
    timer = 15;
    choice = null;
    locked = false;
    document.getElementById("message").textContent =
      "Choose your colour!";
    render();
  },2500);
}

setInterval(()=>{
  if(locked) return;
  timer--;
  if(timer <= 0){
    timer = 0;
    render();
    finishRound();
  }else{
    render();
  }
},1000);

function resetGame(){
  points = 1000;
  round = 1;
  timer = 15;
  choice = null;
  locked = false;
  history = [];
  document.getElementById("history").textContent = "No results yet";
  document.getElementById("message").textContent = "Choose your colour!";
  render();
}
render();
</script>
</body>
</html>
