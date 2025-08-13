<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Water Sort Puzzle</title>
<style>
    body {
        background: #f5f5f5;
        font-family: Arial, sans-serif;
        text-align: center;
        margin: 0;
        padding: 0;
    }
    h1 {
        color: #333;
    }
    canvas {
        background: white;
        border: 2px solid #ccc;
        margin-top: 20px;
        cursor: pointer;
    }
</style>
</head>
<body>
<h1>Water Sort Puzzle</h1>
<canvas id="gameCanvas" width="600" height="400"></canvas>
<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

const tubeWidth = 50;
const tubeHeight = 150;
const tubeSpacing = 80;
const tubeBottom = 300;
const colors = ["red", "blue", "green", "orange", "purple", "yellow"];

let tubes = [];
let selectedTube = null;

function initGame() {
    tubes = [];
    let allColors = [];
    for (let c = 0; c < colors.length; c++) {
        for (let i = 0; i < 4; i++) {
            allColors.push(colors[c]);
        }
    }
    allColors.sort(() => Math.random() - 0.5);

    let tubeCount = 6;
    for (let i = 0; i < tubeCount; i++) {
        tubes.push(allColors.splice(0, 4));
    }
    tubes.push([]); // empty tube 1
    tubes.push([]); // empty tube 2
}

function drawTubes() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    tubes.forEach((tube, i) => {
        const x = 100 + i * tubeSpacing;
        ctx.strokeStyle = "black";
        ctx.strokeRect(x, tubeBottom - tubeHeight, tubeWidth, tubeHeight);
        for (let j = 0; j < tube.length; j++) {
            ctx.fillStyle = tube[j];
            ctx.fillRect(x, tubeBottom - (j + 1) * 35, tubeWidth, 35);
        }
        if (selectedTube === i) {
            ctx.strokeStyle = "gold";
            ctx.lineWidth = 3;
            ctx.strokeRect(x - 5, tubeBottom - tubeHeight - 5, tubeWidth + 10, tubeHeight + 10);
            ctx.lineWidth = 1;
        }
    });
}

canvas.addEventListener("click", (e) => {
    const rect = canvas.getBoundingClientRect();
    const clickX = e.clientX - rect.left;
    const tubeIndex = Math.floor((clickX - 100 + tubeSpacing / 2) / tubeSpacing);
    if (tubeIndex >= 0 && tubeIndex < tubes.length) {
        if (selectedTube === null) {
            selectedTube = tubeIndex;
        } else if (selectedTube === tubeIndex) {
            selectedTube = null;
        } else {
            moveWater(selectedTube, tubeIndex);
            selectedTube = null;
        }
    }
    drawTubes();
});

function moveWater(from, to) {
    if (tubes[from].length === 0) return;
    if (tubes[to].length >= 4) return;
    let colorToMove = tubes[from][tubes[from].length - 1];
    if (tubes[to].length === 0 || tubes[to][tubes[to].length - 1] === colorToMove) {
        tubes[to].push(tubes[from].pop());
    }
}

initGame();
drawTubes();
</script>
</body>
</html>
