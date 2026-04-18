<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>SAS4 Giveaway</title>

<style>
body {
  font-family: Arial;
  text-align: center;
  padding: 20px;
  background: #f4f4f4;
}

input, textarea {
  width: 80%;
  padding: 10px;
  margin: 8px;
}

button {
  padding: 12px 18px;
  margin: 8px;
  cursor: pointer;
}

#wheel {
  width: 220px;
  height: 220px;
  margin: 20px auto;
  border-radius: 50%;
  border: 8px solid #333;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  transition: transform 3s ease-out;
  background: white;
}

.container {
  background: white;
  padding: 15px;
  border-radius: 10px;
  max-width: 600px;
  margin: auto;
}
</style>
</head>

<body>

<div class="container">
<h1>SAS4 Giveaway</h1>

<input id="name" placeholder="Messenger username"><br>
<textarea id="comment" placeholder="Comment"></textarea><br>

<button onclick="addEntry()">Submit Entry</button>
<button onclick="pickWinner()">Spin & Pick Winner</button>

<div id="wheel">SPIN</div>

<h3>Entries</h3>
<ul id="entries"></ul>

<h3>Winners (max 15)</h3>
<ol id="winners"></ol>
</div>

<script>
let entries = JSON.parse(localStorage.getItem("entries")) || [];
let winners = JSON.parse(localStorage.getItem("winners")) || [];

function saveData(){
  localStorage.setItem("entries", JSON.stringify(entries));
  localStorage.setItem("winners", JSON.stringify(winners));
}

function addEntry(){
  let n = document.getElementById('name').value.trim();
  let c = document.getElementById('comment').value.trim();

  if(!n || !c) return alert("Fill both fields");

  if(entries.find(x => x.name.toLowerCase() === n.toLowerCase()))
    return alert("Already entered");

  entries.push({name:n, comment:c});
  saveData();
  render();

  document.getElementById('name').value = "";
  document.getElementById('comment').value = "";
}

function render(){
  document.getElementById('entries').innerHTML =
    entries
    .filter(e => !winners.some(w => w.name === e.name))
    .map(e => `<li><b>${e.name}</b> - ${e.comment}</li>`)
    .join("");

  document.getElementById('winners').innerHTML =
    winners.map(w => `<li><b>${w.name}</b> - ${w.comment}</li>`)
    .join("");
}

function pickWinner(){
  let pool = entries.filter(e => !winners.some(w => w.name === e.name));

  if(pool.length === 0) return alert("No entries left");
  if(winners.length >= 15) return alert("15 winners already selected");

  let wheel = document.getElementById("wheel");

  let spin = 1800 + Math.floor(Math.random() * 360);
  wheel.style.transform = `rotate(${spin}deg)`;

  setTimeout(() => {
    let pick = pool[Math.floor(Math.random() * pool.length)];
    winners.push(pick);

    wheel.innerHTML = "WINNER:<br>" + pick.name;

    saveData();
    render();
  }, 3000);
}

render();
</script>

</body>
</html>
