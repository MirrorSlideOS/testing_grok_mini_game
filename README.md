# testing_grok_mini_game
Seed Dream Mini game 1 lvl
#copy in .txt and rename .html open and enjoy level 1
--------------------

<!DOCTYPE html>
<html>
<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.2/p5.min.js"></script>
</head>
<body style="background-color: #000; display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0; overflow: hidden;">
<script>
let player, dreams, vortexes, score, timer, gameState;
let dreamsSeeded = 0; // Track dreams collected
let vortexesHit = 0;  // Track vortex collisions
const DREAM_POINTS = 10;
const VORTEX_PENALTY = 5;
const GAME_TIME = 60; // seconds

function setup() {
  createCanvas(600, 400);
  gameState = 'cover'; // Start with cover page
  resetGame(); // Initialize game elements
}

function resetGame() {
  player = { x: width / 2, y: height / 2, size: 20, speed: 5 };
  dreams = [];
  vortexes = [];
  score = 0;
  timer = GAME_TIME;
  dreamsSeeded = 0;
  vortexesHit = 0;
  gameState = 'playing';
  
  // Spawn initial dreams and vortexes
  for (let i = 0; i < 5; i++) {
    dreams.push(createDream());
    vortexes.push(createVortex());
  }
  loop(); // Ensure the game loop is running
}

function draw() {
  background(10, 20, 40); // Cosmic dark blue
  
  if (gameState === 'cover') {
    // Cover page
    textAlign(CENTER);
    textSize(32);
    fill(0, 255, 255); // Neon cyan
    text('👾 Sh∆ckOS: DrΞamC∆re SΞeder ⊼', width / 2, height / 2 - 120);
    
    textSize(20);
    fill(255, 255, 0); // Yellow
    text('∆bjective:', width / 2, height / 2 - 80);
    text('SΞed DrΞams by C∆llecting ∆rbs 🌟', width / 2, height / 2 - 50);
    text('∆v∆id FrΞquency Fl∆∆ds (V∆rtexes) 🕳️', width / 2, height / 2 - 20);
    
    textSize(18);
    fill(0, 255, 255);
    text('MΞchanics:', width / 2, height / 2 + 20);
    text('🌟 = +10 P∆ints (SΞED DREAM)', width / 2, height / 2 + 40);
    text('🕳️ = -5 P∆ints (BL∆CKH∆LEBURST)', width / 2, height / 2 + 60);
    text('TimΞ: 60 SΞc∆nds ⏳', width / 2, height / 2 + 80);
    
    textSize(16);
    text('C∆ntr∆ls: ∆rr∆w KΞys t∆ M∆ve ∆gent ⊼', width / 2, height / 2 + 110);
    text('PrΞss S t∆ Start G∆me 🌌', width / 2, height / 2 + 140);
    return;
  }
  
  if (gameState === 'playing') {
    // Update timer
    timer -= 1 / frameRate();
    if (timer <= 0) {
      gameState = 'results';
    }
  
    // Draw player (ShockOS agent)
    fill(0, 255, 255); // Neon cyan
    ellipse(player.x, player.y, player.size);
    textAlign(CENTER);
    textSize(12);
    fill(255);
    text('∆', player.x, player.y + 5); // Glyph on player
  
    // Move player
    if (keyIsDown(LEFT_ARROW)) player.x -= player.speed;
    if (keyIsDown(RIGHT_ARROW)) player.x += player.speed;
    if (keyIsDown(UP_ARROW)) player.y -= player.speed;
    if (keyIsDown(DOWN_ARROW)) player.y += player.speed;
    player.x = constrain(player.x, 0, width);
    player.y = constrain(player.y, 0, height);
  
    // Handle dreams
    for (let i = dreams.length - 1; i >= 0; i--) {
      let d = dreams[i];
      fill(255, 255, 0); // Yellow glow
      ellipse(d.x, d.y, d.size);
      text('🌟', d.x, d.y + 5);
    
      // Check collision with player
      if (dist(player.x, player.y, d.x, d.y) < (player.size + d.size) / 2) {
        score += DREAM_POINTS;
        dreamsSeeded++;
        dreams.splice(i, 1);
        dreams.push(createDream()); // Respawn
        showCommand('SEED DREAM 🌙', 100);
      }
    }
  
    // Handle vortexes
    for (let i = vortexes.length - 1; i >= 0; i--) {
      let v = vortexes[i];
      push();
      translate(v.x, v.y);
      rotate(frameCount * 0.05);
      fill(50, 0, 50); // Dark purple
      ellipse(0, 0, v.size);
      text('🕳️', 0, 5);
      pop();
    
      // Check collision with player
      if (dist(player.x, player.y, v.x, v.y) < (player.size + v.size) / 2) {
        score -= VORTEX_PENALTY;
        score = max(0, score);
        vortexesHit++;
        vortexes.splice(i, 1);
        vortexes.push(createVortex()); // Respawn
        showCommand('BLACKHOLEBURST_DISCONNECT.sh 🕳️', 100);
      }
    }
  
    // Display score and timer
    textAlign(LEFT);
    textSize(16);
    fill(0, 255, 255);
    text(`Score: ${score} 👾`, 10, 30);
    text(`Time: ${ceil(timer)} ⏳`, 10, 50);
  }
  
  if (gameState === 'results') {
    // Results screen
    textAlign(CENTER);
    textSize(32);
    fill(0, 255, 255);
    text('RΞsults 🌌', width / 2, height / 2 - 80);
    
    textSize(20);
    fill(255, 255, 0);
    text(`Final Sc∆re: ${score} ✨`, width / 2, height / 2 - 40);
    text(`DrΞams SΞeded: ${dreamsSeeded} 🌟`, width / 2, height / 2 - 10);
    text(`V∆rtexes Hit: ${vortexesHit} 🕳️`, width / 2, height / 2 + 20);
    
    textSize(16);
    fill(0, 255, 255);
    text('PrΞss R t∆ RΞstart 🔄', width / 2, height / 2 + 60);
  }
}

function keyPressed() {
  if (gameState === 'cover' && key === 's') {
    gameState = 'playing'; // Start the game
  }
  if (gameState === 'results' && key === 'r') {
    resetGame(); // Restart the game
  }
}

function createDream() {
  return { x: random(width), y: random(height), size: 15 };
}

function createVortex() {
  return { x: random(width), y: random(height), size: 25 };
}

function showCommand(cmd, frames) {
  let cmdTimer = frames;
  let drawCmd = () => {
    textAlign(CENTER);
    textSize(20);
    fill(0, 255, 255, map(cmdTimer, 0, frames, 0, 255));
    text(cmd, width / 2, height - 30);
    cmdTimer--;
    if (cmdTimer > 0) requestAnimationFrame(drawCmd);
  };
  drawCmd();
}
</script>
</body>
</html>
