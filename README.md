# habbit-tracker
<!DOCTYPE html><html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Habit Tracker</title>
<style>
body {font-family: Arial; background:#f4f4f4; margin:0;}
header {background:#2e7d32; color:white; padding:15px; text-align:center;}
.container {padding:20px;}
.day {background:white; margin:10px 0; padding:15px; border-radius:10px; box-shadow:0 2px 5px rgba(0,0,0,0.1);} 
.progress {font-size:18px; font-weight:bold; color:#2e7d32;}
input[type="text"] {width:70%; padding:5px;}
button {background:#2e7d32; color:white; border:none; padding:5px 10px; cursor:pointer; border-radius:5px;}
</style>
</head>
<body><header>
<h2>Habit Tracker</h2>
<p>Track your daily tasks and progress</p>
</header><div class="container" id="tracker"></div><script>
const days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
let data = JSON.parse(localStorage.getItem("habitData")) || {};

function saveData(){
  localStorage.setItem("habitData", JSON.stringify(data));
}

function render(){
  const container = document.getElementById("tracker");
  container.innerHTML = "";

  days.forEach(day => {
    if(!data[day]) data[day] = [];

    let completed = data[day].filter(t => t.done).length;
    let total = data[day].length;
    let percent = total ? Math.round((completed/total)*100) : 0;

    let div = document.createElement("div");
    div.className = "day";

    div.innerHTML = `
      <h3>${day}</h3>
      <div class="progress">Progress: ${percent}%</div>
      <div>
        <input type="text" id="input-${day}" placeholder="Add task">
        <button onclick="addTask('${day}')">Add</button>
      </div>
      <ul>
        ${data[day].map((t,i)=>`
          <li>
            <input type="checkbox" ${t.done?"checked":""} onchange="toggle('${day}',${i})">
            ${t.name}
          </li>
        `).join("")}
      </ul>
    `;

    container.appendChild(div);
  });
}

function addTask(day){
  let input = document.getElementById(`input-${day}`);
  if(input.value.trim()==="") return;
  data[day].push({name: input.value, done:false});
  input.value = "";
  saveData();
  render();
}

function toggle(day,index){
  data[day][index].done = !data[day][index].done;
  saveData();
  render();
}

render();
</script></body>
</html>
