<!DOCTYPE html>
<html>
<head>
<title>AI Gold Price Prediction System</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>

body{
font-family:Arial;
margin:0;
background:#f4f6f9;
}

header{
background:#1e3a8a;
color:white;
padding:15px;
text-align:center;
font-size:28px;
}

.container{
width:90%;
margin:auto;
}

.card{
background:white;
padding:20px;
margin:20px 0;
border-radius:10px;
box-shadow:0 0 10px rgba(0,0,0,0.1);
}

input,button,textarea{
width:100%;
padding:10px;
margin-top:10px;
}

button{
background:#1e3a8a;
color:white;
border:none;
cursor:pointer;
}

button:hover{
background:#162d6b;
}

.chatbox{
height:220px;
overflow-y:auto;
border:1px solid #ccc;
padding:10px;
border-radius:5px;
}

.msg{
margin:5px 0;
}

.user{
color:blue;
}

.bot{
color:green;
}

/* NEWS TICKER */

.news-wrapper{
overflow:hidden;
background:#fff8dc;
padding:10px;
border-radius:10px;
}

.news-track{
display:flex;
width:max-content;
animation:scrollNews 25s linear infinite;
}

.news-item{

min-width:350px;
margin-right:20px;
padding:15px;

background:white;

border-left:5px solid orange;

border-radius:5px;

font-weight:bold;

box-shadow:0 0 5px rgba(0,0,0,0.1);

}

@keyframes scrollNews{

0%{
transform:translateX(0);
}

100%{
transform:translateX(-50%);
}

}

</style>

</head>

<body>

<header>
🏆 AI Gold Price Prediction & Analysis System
</header>

<div class="container">

<!-- DATASET -->
<div class="card">

<h2>📂 Upload Gold Dataset</h2>

<input type="file" id="fileInput">

<p>
Upload CSV / TXT file with one gold price per line
</p>

</div>

<!-- GRAPH -->
<div class="card">

<h2>📊 Gold Price Graph</h2>

<canvas id="chart"></canvas>

</div>

<!-- ANALYSIS -->
<div class="card">

<h2>📈 Dataset Analysis</h2>

<p id="total"></p>
<p id="avg"></p>
<p id="max"></p>
<p id="min"></p>
<p id="trend"></p>
<p id="risk"></p>
<p id="suggestion"></p>

</div>

<!-- ALERT -->
<div class="card">

<h2>🚨 Gold Price Alert</h2>

<input type="number" id="alertPrice" placeholder="Enter alert price">

<button onclick="setAlert()">
Set Alert
</button>

<p id="alertMsg"></p>

</div>

<!-- EMI -->
<div class="card">

<h2>💰 EMI Calculator</h2>

<input type="number" id="goldAmount" placeholder="Gold Amount">

<input type="number" id="months" placeholder="Months">

<input type="number" id="interest" placeholder="Interest %">

<button onclick="calculateEMI()">
Calculate EMI
</button>

<p id="emiResult"></p>

</div>

<!-- LINEAR REGRESSION -->
<div class="card">

<h2>🤖 Linear Regression Prediction</h2>

<input type="number" id="days" placeholder="Predict future days">

<button onclick="predictLR()">
Predict
</button>

<p id="result"></p>

</div>

<!-- CHATBOT -->
<div class="card">

<h2>🤖 AI Chatbot Assistant</h2>

<div class="chatbox" id="chatbox"></div>

<input type="text" id="userInput" placeholder="Ask about gold price">

<button onclick="sendMessage()">
Send
</button>

</div>

<!-- GOLD SHOPS -->
<div class="card">

<h2>📍 Nearby Gold Shops</h2>

<input type="text" id="locationInput" placeholder="Enter location">

<button onclick="findShops()">
Find Shops
</button>

<ul id="shopList"></ul>

</div>

<!-- NEWS -->
<div class="card">

<h2>📰 Latest Gold Market News</h2>

<div class="news-wrapper">

<div class="news-track">

<div class="news-item">
📈 Gold prices rise due to global demand
</div>

<div class="news-item">
💰 Investors buying more gold amid inflation
</div>

<div class="news-item">
🌍 International market affects gold prices
</div>

<div class="news-item">
🏦 Central banks increasing gold reserves
</div>

<div class="news-item">
📊 Analysts predict stable gold growth
</div>

<div class="news-item">
🔥 Gold demand increases in India market
</div>

<!-- DUPLICATE -->

<div class="news-item">
📈 Gold prices rise due to global demand
</div>

<div class="news-item">
💰 Investors buying more gold amid inflation
</div>

<div class="news-item">
🌍 International market affects gold prices
</div>

<div class="news-item">
🏦 Central banks increasing gold reserves
</div>

<div class="news-item">
📊 Analysts predict stable gold growth
</div>

<div class="news-item">
🔥 Gold demand increases in India market
</div>

</div>

</div>

</div>

</div>

<script>

let goldData = [];
let chart;
let alertValue = null;

// FILE LOAD
document.getElementById("fileInput")
.addEventListener("change", function(e){

let file = e.target.files[0];

let reader = new FileReader();

reader.onload = function(event){

let data = event.target.result.split("\n");

goldData = data
.map(x => parseFloat(x))
.filter(x => !isNaN(x));

if(goldData.length < 2){
alert("Dataset needs minimum 2 values");
return;
}

analyzeData();
drawChart();

};

reader.readAsText(file);

});

// ANALYSIS
function analyzeData(){

let total = goldData.length;

let sum = goldData.reduce((a,b)=>a+b,0);

let avg = sum / total;

let max = Math.max(...goldData);

let min = Math.min(...goldData);

// TREND
let trend = "";

if(goldData[goldData.length-1] > goldData[0]){
trend = "📈 Increasing";
}
else{
trend = "📉 Decreasing";
}

// VOLATILITY
let variance = 0;

for(let i=0;i<goldData.length;i++){

variance += Math.pow(goldData[i]-avg,2);

}

variance = variance / total;

let volatility = Math.sqrt(variance);

let risk = "";

if(volatility < 50){
risk = "Low Risk 🟢";
}
else if(volatility < 150){
risk = "Medium Risk 🟡";
}
else{
risk = "High Risk 🔴";
}

// SUGGESTION
let suggestion = "";

if(avg < 5700){
suggestion = "💰 BUY";
}
else if(avg > 6000){
suggestion = "📉 SELL";
}
else{
suggestion = "🤔 HOLD";
}

// DISPLAY
document.getElementById("total").innerText =
"Total Records : " + total;

document.getElementById("avg").innerText =
"Average Price : ₹ " + avg.toFixed(2);

document.getElementById("max").innerText =
"Highest Price : ₹ " + max;

document.getElementById("min").innerText =
"Lowest Price : ₹ " + min;

document.getElementById("trend").innerText =
"Market Trend : " + trend;

document.getElementById("risk").innerText =
"Risk Level : " + risk;

document.getElementById("suggestion").innerText =
"Investment Suggestion : " + suggestion;

}

// CHART
function drawChart(){

let labels = goldData.map((_,i)=>i+1);

if(chart){
chart.destroy();
}

chart = new Chart(document.getElementById("chart"),{

type:"line",

data:{

labels:labels,

datasets:[{

label:"Gold Price",

data:goldData,

borderColor:"blue",

fill:false

}]

}

});

}

// ALERT
function setAlert(){

alertValue =
document.getElementById("alertPrice").value;

document.getElementById("alertMsg").innerText =
"🚨 Alert set at ₹ " + alertValue;

}

// EMI
function calculateEMI(){

let P =
parseFloat(document.getElementById("goldAmount").value);

let N =
parseFloat(document.getElementById("months").value);

let R =
parseFloat(document.getElementById("interest").value)/100/12;

if(!P || !N || !R){
alert("Enter all fields");
return;
}

let EMI =
(P*R*Math.pow(1+R,N))/
(Math.pow(1+R,N)-1);

document.getElementById("emiResult").innerText =
"Monthly EMI : ₹ " + EMI.toFixed(2);

}

// LINEAR REGRESSION
function predictLR(){

let n = goldData.length;

if(n < 2){
alert("Upload dataset first");
return;
}

let xSum=0;
let ySum=0;
let xy=0;
let x2=0;

for(let i=0;i<n;i++){

xSum += i;
ySum += goldData[i];
xy += i*goldData[i];
x2 += i*i;

}

let m =
(n*xy - xSum*ySum)/
(n*x2 - xSum*xSum);

let c =
(ySum - m*xSum)/n;

let future =
parseInt(document.getElementById("days").value);

let predicted =
m*(n+future)+c;

document.getElementById("result").innerText =
"Predicted Future Price : ₹ " +
predicted.toFixed(2);

// ALERT
if(alertValue && predicted < alertValue){

alert("⚠ Predicted price below alert!");

}

}

// CHATBOT
function sendMessage(){

let input =
document.getElementById("userInput")
.value.toLowerCase();

let chatbox =
document.getElementById("chatbox");

chatbox.innerHTML +=
`<div class='msg user'>
You : ${input}
</div>`;

let reply =
"I didn't understand 😅";

if(input.includes("price")){
reply =
"Gold price changes based on market conditions.";
}

else if(input.includes("buy")){
reply =
"Current trend should be checked before buying.";
}

else if(input.includes("sell")){
reply =
"If prices are high, selling may be profitable.";
}

else if(input.includes("predict")){
reply =
"Use Linear Regression Prediction section.";
}

chatbox.innerHTML +=
`<div class='msg bot'>
Bot : ${reply}
</div>`;

document.getElementById("userInput").value = "";

chatbox.scrollTop =
chatbox.scrollHeight;

}

// LOCATION SHOPS
function findShops(){

let location =
document.getElementById("locationInput")
.value.toLowerCase();

let list =
document.getElementById("shopList");

list.innerHTML = "";

let shops = [];

if(location.includes("chennai")){

shops = [
"Tanishq - Chennai",
"Kalyan Jewellers - Chennai",
"Malabar Gold - Chennai"
];

}

else if(location.includes("madurai")){

shops = [
"Joyalukkas - Madurai",
"Tanishq - Madurai"
];

}

else if(location.includes("coimbatore")){

shops = [
"Malabar Gold - Coimbatore",
"Kalyan Jewellers - Coimbatore"
];

}

else{

shops = [
"Tanishq",
"Kalyan Jewellers",
"Malabar Gold"
];

}

shops.forEach(shop=>{

let li = document.createElement("li");

li.innerText = shop;

list.appendChild(li);

});

}

</script>

</body>
</html>
