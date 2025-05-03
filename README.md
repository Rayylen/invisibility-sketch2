let t = 0;
let invisibilityPulse = 1;

function setup() {
  createCanvas(600, 600);
  noFill();
  textAlign(CENTER, CENTER);
  textSize(20);
}

function draw() {
  background(10, 10, 30, 80);
  drawGridBackground();

  translate(width / 2, height / 2);

  drawLabCoat();
  drawHair();
  drawGlassesGlare();
  drawInvisibilityOutline();
  drawTitleText();

  t += 0.01;
}

// Background grid to imply a high-tech lab
function drawGridBackground() {
  stroke(30, 30, 50);
  strokeWeight(0.5);
  for (let i = 0; i < width; i += 20) {
    line(i, 0, i, height);
    line(0, i, width, i);
  }
}

// Abstract triangle lab coat
function drawLabCoat() {
  stroke(255, 255, 255, 60);
  strokeWeight(2);
  triangle(-40, 60, 40, 60, 0, 180);
}

// Radiating red strands represent locs
function drawHair() {
  stroke(255, 0, 0, 180);
  strokeWeight(2);
  for (let i = 0; i < 40; i++) {
    let angle = TWO_PI * i / 40 + t * 2;
    let radius = 100 + noise(i * 0.1, t) * 30;
    let mx = map(mouseX, 0, width, -15, 15);  // Follow mouse subtly
    let my = map(mouseY, 0, height, -15, 15);
    let x = cos(angle) * radius + mx;
    let y = sin(angle) * radius + my;
    line(0, 0, x, y);
  }
}

// Animated glare on glasses
function drawGlassesGlare() {
  noFill();
  stroke(255, 255, 255, 220 + sin(t * 4) * 35);
  strokeWeight(3);
  ellipse(-40 + sin(t * 2) * 2, -20, 30 + sin(t * 3) * 5);
  ellipse(40 + sin(t * 2) * 2, -20, 30 + sin(t * 3) * 5);
}

// Wobbly outline symbolizing partial visibility
function drawInvisibilityOutline() {
  stroke(100, 255 - invisibilityPulse * 50, 255, 100 + sin(t * 2) * 60);
  strokeWeight(1.5 + invisibilityPulse * 0.5);
  beginShape();
  for (let a = 0; a < TWO_PI; a += 0.1) {
    let r = 60 + noise(t + a) * (20 * invisibilityPulse);
    let x = r * cos(a);
    let y = r * sin(a);
    vertex(x, y);
  }
  endShape(CLOSE);
}

// Power name label
function drawTitleText() {
  noStroke();
  fill(100, 200, 255, 180);
  text("INVISIBILITY", 0, 240);
}

// On mouse click, increase invisibility pulse temporarily
function mousePressed() {
  invisibilityPulse = (invisibilityPulse === 1) ? 2 : 1;
}
