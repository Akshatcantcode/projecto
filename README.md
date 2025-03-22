# projecto
// server/server.js
const express = require('express');
const app = express();
const path = require('path');

let adRevenue = 0; // Simulating revenue tracking
let donations = 0; // Simulating donations

app.use(express.static(path.join(__dirname, '../public')));

app.get('/get-revenue', (req, res) => {
  res.json({ adRevenue });
});

app.post('/donate', (req, res) => {
  let donation = parseInt(req.query.amount, 10) || 0;
  donations += donation;
  adRevenue -= donation;
  res.json({ message: 'Donation successful', donations, adRevenue });
});

// Simulate ad revenue generation
app.get('/simulate-ad-revenue', (req, res) => {
  adRevenue += Math.floor(Math.random() * 10) + 1; // Random revenue between $1 and $10
  res.json({ adRevenue });
});

app.listen(3000, () => {
  console.log('Server is running on http://localhost:3000');
});
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sustainability Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="game-container">
        <div id="game-area">
            <h1>Sustainability Game</h1>
            <p>Click to plant trees and help the environment!</p>
            <p id="tree-count">Trees Planted: 0</p>
            <p id="ad-revenue">Ad Revenue: $0</p>
            <p id="donations">Donations: $0</p>
            <div id="game-canvas" onclick="plantTree(event)"></div>
        </div>

        <div class="ads" id="ads">
            <div class="ad-placeholder">Ad Placeholder 1</div>
            <div class="ad-placeholder">Ad Placeholder 2</div>
            <div class="ad-placeholder">Ad Placeholder 3</div>
        </div>
    </div>

    <script src="game.js"></script>
</body>
</html>
/* public/style.css */
body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
    margin: 0;
}

.game-container {
    display: flex;
    justify-content: space-between;
    width: 80%;
    max-width: 1200px;
}

#game-area {
    width: 70%;
    background-color: #e0f7fa;
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.ads {
    width: 25%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.ad-placeholder {
    height: 100px;
    background-color: #cccccc;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-size: 16px;
    border-radius: 8px;
}

#game-canvas {
    background-color: #c8e6c9;
    border: 2px dashed green;
    width: 100%;
    height: 300px;
    position: relative;
}

#tree-count, #ad-revenue, #donations {
    font-size: 18px;
}
// public/game.js

let treeCount = 0;
let adRevenue = 0;
let donations = 0;

const treeCountDisplay = document.getElementById('tree-count');
const adRevenueDisplay = document.getElementById('ad-revenue');
const donationDisplay = document.getElementById('donations');

// Fetch initial revenue
async function fetchRevenue() {
    const response = await fetch('/get-revenue');
    const data = await response.json();
    adRevenue = data.adRevenue;
    adRevenueDisplay.textContent = `Ad Revenue: $${adRevenue}`;
}

// Plant a tree when the user clicks
function plantTree(event) {
    treeCount++;
    treeCountDisplay.textContent = `Trees Planted: ${treeCount}`;
    simulateAdRevenue();
}

// Simulate ad revenue generation
async function simulateAdRevenue() {
    const response = await fetch('/simulate-ad-revenue');
    const data = await response.json();
    adRevenue = data.adRevenue;
    adRevenueDisplay.textContent = `Ad Revenue: $${adRevenue}`;
}

// Handle donation
async function makeDonation(amount) {
    const response = await fetch(`/donate?amount=${amount}`, {
        method: 'POST',
    });
    const data = await response.json();
    donations = data.donations;
    adRevenue = data.adRevenue;
    donationDisplay.textContent = `Donations: $${donations}`;
    adRevenueDisplay.textContent = `Ad Revenue: $${adRevenue}`;
}

// Initial revenue fetch
fetchRevenue();
npm install express
/sustainability-game
  /public
    /index.html
    /style.css
    /game.js
  /server
    /server.js
  /node_modules
  /package.json
