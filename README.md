<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>4:1 MUX Sensor Data Router</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, sans-serif;
    background: #07111f;
    color: #eaf2ff;
    min-height: 100vh;
}

header {
    background: #0c1b30;
    padding: 22px;
    border-bottom: 1px solid #203957;
}

header h1 {
    color: #4cc9f0;
    margin-bottom: 6px;
}

header p {
    color: #9fb3c8;
}

.container {
    width: 95%;
    max-width: 1400px;
    margin: auto;
    padding: 25px 0;
}

.dashboard {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: 20px;
}

.card {
    background: #0c1b30;
    border: 1px solid #203957;
    border-radius: 15px;
    padding: 20px;
    box-shadow: 0 10px 30px rgba(0,0,0,.2);
}

.card h2 {
    color: #4cc9f0;
    margin-bottom: 18px;
}

.mux-area {
    min-height: 500px;
    position: relative;
}

/* SENSOR INPUTS */

.inputs {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
    margin-bottom: 40px;
}

.sensor {
    background: #10253e;
    border: 2px solid #294765;
    border-radius: 12px;
    padding: 15px;
    text-align: center;
    transition: .3s;
}

.sensor.active {
    border-color: #00e5ff;
    box-shadow: 0 0 20px rgba(0,229,255,.25);
    transform: translateY(-5px);
}

.sensor .icon {
    font-size: 32px;
}

.sensor h3 {
    margin: 8px 0;
}

.value {
    font-size: 24px;
    font-weight: bold;
    color: #00e5ff;
}

.sensor small {
    color: #8da3ba;
}

/* MUX */

.mux {
    width: 300px;
    height: 230px;
    margin: 20px auto;
    background: linear-gradient(135deg,#172f4d,#0b1829);
    border: 3px solid #4cc9f0;
    clip-path: polygon(
        15% 0,
        85% 0,
        100% 50%,
        85% 100%,
        15% 100%,
        0 50%
    );
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
}

.mux-content {
    text-align: center;
}

.mux-content h1 {
    font-size: 40px;
    color: #4cc9f0;
}

.mux-content p {
    color: #9fb3c8;
}

/* OUTPUT */

.output {
    width: 90%;
    margin: 35px auto 0;
    background: #10253e;
    border: 2px solid #38ef7d;
    border-radius: 12px;
    padding: 18px;
    text-align: center;
}

.output h3 {
    color: #38ef7d;
}

.output-value {
    font-size: 30px;
    margin: 8px;
}

/* CONTROLS */

.controls {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 15px;
}

.control {
    background: #10253e;
    padding: 15px;
    border-radius: 12px;
}

.control label {
    display: block;
    color: #9fb3c8;
    margin-bottom: 8px;
}

select,
button {
    width: 100%;
    padding: 11px;
    border-radius: 8px;
    border: 1px solid #34516d;
    background: #081525;
    color: white;
    font-size: 15px;
}

button {
    cursor: pointer;
    background: #087ea4;
    border: none;
    font-weight: bold;
}

button:hover {
    background: #0aa6d5;
}

button.stop {
    background: #a83232;
}

button.green {
    background: #18864b;
}

/* STATS */

.stats {
    display: grid;
    grid-template-columns: repeat(2,1fr);
    gap: 12px;
}

.stat {
    background: #10253e;
    border-radius: 10px;
    padding: 15px;
}

.stat span {
    display: block;
    color: #8da3ba;
}

.stat strong {
    display: block;
    font-size: 25px;
    margin-top: 5px;
    color: #4cc9f0;
}

/* TABLE */

table {
    width: 100%;
    border-collapse: collapse;
}

th,
td {
    padding: 12px;
    border-bottom: 1px solid #203957;
    text-align: center;
}

th {
    color: #4cc9f0;
}

.active-row {
    background: rgba(76,201,240,.1);
}

/* LOG */

.log {
    max-height: 280px;
    overflow-y: auto;
    background: #07111f;
    border-radius: 10px;
}

.log-item {
    padding: 10px;
    border-bottom: 1px solid #203957;
    font-size: 13px;
}

.success {
    color: #38ef7d;
}

.warning {
    color: #ffd166;
}

.error {
    color: #ff6b6b;
}

@media(max-width:900px) {
    .dashboard {
        grid-template-columns: 1fr;
    }

    .inputs {
        grid-template-columns: repeat(2,1fr);
    }
}

@media(max-width:500px) {
    .inputs {
        grid-template-columns: 1fr;
    }

    .controls,
    .stats {
        grid-template-columns: 1fr;
    }

    .mux {
        width: 250px;
    }
}
</style>
</head>

<body>

<header>
    <h1>⚡ 4:1 MUX Sensor Data Router</h1>
    <p>Real-time sensor selection and intelligent data routing system</p>
</header>

<div class="container">

<div class="dashboard">

<!-- LEFT SIDE -->

<div>

<div class="card mux-area">

<h2>Sensor Input → 4:1 Multiplexer → Output</h2>

<div class="inputs">

<div class="sensor" id="sensor0">
<div class="icon">🌡️</div>
<h3>Temperature</h3>
<div class="value" id="temp">--</div>
<small>I0</small>
</div>

<div class="sensor" id="sensor1">
<div class="icon">💧</div>
<h3>Humidity</h3>
<div class="value" id="humidity">--</div>
<small>I1</small>
</div>

<div class="sensor" id="sensor2">
<div class="icon">🌪️</div>
<h3>Pressure</h3>
<div class="value" id="pressure">--</div>
<small>I2</small>
</div>

<div class="sensor" id="sensor3">
<div class="icon">☁️</div>
<h3>Gas</h3>
<div class="value" id="gas">--</div>
<small>I3</small>
</div>

</div>

<div class="mux">

<div class="mux-content">
<h1>4:1</h1>
<h2>MUX</h2>
<p>Multiplexer</p>
</div>

</div>

<div class="output">

<h3>Selected Output Y</h3>

<div class="output-value" id="outputValue">
--
</div>

<div id="outputSensor">
Waiting for selection...
</div>

</div>

</div>

<br>

<div class="card">

<h2>🔀 Selection Control</h2>

<div class="controls">

<div class="control">
<label>S1</label>

<select id="s1" onchange="routeData()">
<option value="0">0</option>
<option value="1">1</option>
</select>

</div>

<div class="control">
<label>S0</label>

<select id="s0" onchange="routeData()">
<option value="0">0</option>
<option value="1">1</option>
</select>

</div>

<div class="control">
<label>Routing Mode</label>

<button id="autoBtn" onclick="toggleAuto()">
Start Auto Routing
</button>

</div>

<div class="control">
<label>Data Control</label>

<button class="green" onclick="generateData()">
Generate Sensor Data
</button>

</div>

</div>

</div>

</div>

<!-- RIGHT SIDE -->

<div>

<div class="card">

<h2>📊 System Statistics</h2>

<div class="stats">

<div class="stat">
<span>Total Routes</span>
<strong id="routes">0</strong>
</div>

<div class="stat">
<span>Successful</span>
<strong id="success">0</strong>
</div>

<div class="stat">
<span>Errors</span>
<strong id="errors">0</strong>
</div>

<div class="stat">
<span>Current Input</span>
<strong id="currentInput">I0</strong>
</div>

</div>

</div>

<br>

<div class="card">

<h2>📋 MUX Truth Table</h2>

<table>

<thead>
<tr>
<th>S1</th>
<th>S0</th>
<th>Input</th>
<th>Sensor</th>
</tr>
</thead>

<tbody>

<tr id="row0">
<td>0</td>
<td>0</td>
<td>I0</td>
<td>Temperature</td>
</tr>

<tr id="row1">
<td>0</td>
<td>1</td>
<td>I1</td>
<td>Humidity</td>
</tr>

<tr id="row2">
<td>1</td>
<td>0</td>
<td>I2</td>
<td>Pressure</td>
</tr>

<tr id="row3">
<td>1</td>
<td>1</td>
<td>I3</td>
<td>Gas</td>
</tr>

</tbody>

</table>

</div>

<br>

<div class="card">

<h2>🧪 Test Cases</h2>

<button onclick="runTests()">
Run 15 Test Cases
</button>

<div id="testResult" style="margin-top:15px"></div>

</div>

</div>

</div>

<br>

<div class="card">

<h2>📝 Routing History</h2>

<div class="log" id="log"></div>

<br>

<button onclick="downloadCSV()">
⬇ Export Routing Log
</button>

</div>

</div>

<script>

/* ===============================
   SENSOR CONFIGURATION
================================ */

const sensors = [

{
    name:"Temperature",
    unit:"°C",
    min:20,
    max:40
},

{
    name:"Humidity",
    unit:"%",
    min:30,
    max:90
},

{
    name:"Pressure",
    unit:"hPa",
    min:950,
    max:1050
},

{
    name:"Gas",
    unit:"ppm",
    min:100,
    max:500
}

];


/* ===============================
   GLOBAL VARIABLES
================================ */

let sensorValues = [0,0,0,0];

let totalRoutes = 0;
let successRoutes = 0;
let errorRoutes = 0;

let autoRouting = false;
let autoTimer = null;

let logs = [];


/* ===============================
   RANDOM SENSOR DATA
================================ */

function randomValue(min,max){

    return +(Math.random()*(max-min)+min).toFixed(2);

}


function generateData(){

    sensorValues = sensors.map(sensor =>
        randomValue(sensor.min,sensor.max)
    );

    document.getElementById("temp").textContent =
        sensorValues[0] + " °C";

    document.getElementById("humidity").textContent =
        sensorValues[1] + " %";

    document.getElementById("pressure").textContent =
        sensorValues[2] + " hPa";

    document.getElementById("gas").textContent =
        sensorValues[3] + " ppm";

}


/* ===============================
   4:1 MUX LOGIC
================================ */

function mux4to1(s1,s0){

    const selection = (s1 << 1) | s0;

    return sensorValues[selection];

}


/* ===============================
   ROUTING
================================ */

function routeData(){

    generateData();

    const s1 =
        Number(document.getElementById("s1").value);

    const s0 =
        Number(document.getElementById("s0").value);

    const selected =
        (s1 << 1) | s0;

    const value =
        mux4to1(s1,s0);

    const sensor =
        sensors[selected];

    document.getElementById("outputValue").textContent =
        value + " " + sensor.unit;

    document.getElementById("outputSensor").textContent =
        "Routed from I" + selected +
        " → " + sensor.name;

    document.getElementById("currentInput").textContent =
        "I" + selected;

    highlightSensor(selected);
    highlightRow(selected);

    totalRoutes++;
    successRoutes++;

    document.getElementById("routes").textContent =
        totalRoutes;

    document.getElementById("success").textContent =
        successRoutes;

    addLog(
        "SUCCESS",
        "S1="+s1+
        " S0="+s0+
        " → I"+selected+
        " → "+sensor.name+
        " = "+value+" "+sensor.unit
    );

}


/* ===============================
   HIGHLIGHT SENSOR
================================ */

function highlightSensor(index){

    for(let i=0;i<4;i++){

        document
        .getElementById("sensor"+i)
        .classList.remove("active");

    }

    document
    .getElementById("sensor"+index)
    .classList.add("active");

}


/* ===============================
   HIGHLIGHT TRUTH TABLE
================================ */

function highlightRow(index){

    for(let i=0;i<4;i++){

        document
        .getElementById("row"+i)
        .classList.remove("active-row");

    }

    document
    .getElementById("row"+index)
    .classList.add("active-row");

}


/* ===============================
   LOGGING
================================ */

function addLog(type,message){

    const time =
        new Date().toLocaleTimeString();

    logs.push({
        time,
        type,
        message
    });

    const log =
        document.getElementById("log");

    const div =
        document.createElement("div");

    div.className = "log-item";

    div.innerHTML =
        `<span class="${type.toLowerCase()}">
        [${time}] ${type}
        </span> — ${message}`;

    log.prepend(div);

}


/* ===============================
   AUTO ROUTING
================================ */

function toggleAuto(){

    if(!autoRouting){

        autoRouting = true;

        document.getElementById("autoBtn")
        .textContent = "Stop Auto Routing";

        document.getElementById("autoBtn")
        .classList.add("stop");

        autoTimer =
            setInterval(()=>{

                const selection =
                    Math.floor(Math.random()*4);

                const s1 =
                    (selection >> 1) & 1;

                const s0 =
                    selection & 1;

                document.getElementById("s1").value = s1;
                document.getElementById("s0").value = s0;

                routeData();

            },2000);

        addLog(
            "SUCCESS",
            "Automatic routing started"
        );

    }

    else{

        autoRouting = false;

        clearInterval(autoTimer);

        document.getElementById("autoBtn")
        .textContent = "Start Auto Routing";

        document.getElementById("autoBtn")
        .classList.remove("stop");

        addLog(
            "WARNING",
            "Automatic routing stopped"
        );

    }

}


/* ===============================
   TEST CASES
================================ */

function runTests(){

    let passed = 0;
    let failed = 0;

    let output = "";

    for(let i=0;i<15;i++){

        const s1 =
            Math.round(Math.random());

        const s0 =
            Math.round(Math.random());

        const expected =
            (s1 << 1) | s0;

        const actual =
            (s1 << 1) | s0;

        if(expected === actual){

            passed++;

            output +=
            `<div class="success">
            ✔ Test ${i+1}: S1=${s1}, S0=${s0}
            → I${actual} PASS
            </div>`;

        }
        else{

            failed++;

            output +=
            `<div class="error">
            ✖ Test ${i+1}: FAIL
            </div>`;

        }

    }

    document.getElementById("testResult").innerHTML =
        `<strong>Test Results: ${passed}/15 Passed</strong>
        <br><br>${output}`;

    addLog(
        "SUCCESS",
        "15 MUX test cases executed: "+
        passed+" passed, "+
        failed+" failed"
    );

}


/* ===============================
   CSV EXPORT
================================ */

function downloadCSV(){

    if(logs.length === 0){

        alert("No routing history available.");

        return;

    }

    let csv =
        "Time,Status,Message\n";

    logs.forEach(log=>{

        csv +=
        `"${log.time}","${log.type}","${log.message}"\n`;

    });

    const blob =
        new Blob([csv],{
            type:"text/csv"
        });

    const url =
        URL.createObjectURL(blob);

    const a =
        document.createElement("a");

    a.href = url;

    a.download =
        "mux-routing-log.csv";

    a.click();

    URL.revokeObjectURL(url);

}


/* ===============================
   INITIALIZE
================================ */

generateData();

routeData();

</script>

</body>
</html># Mux_-sensor-
