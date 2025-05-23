
Prereq: Create Copilot account - we must specify this at the beginning of the workshop

Links:
[Reference](https://p5js.org/reference/)
[p5.js Web Editor](https://editor.p5js.org/)

# 1st Part - Fundamentals
Goal: Understand basic concepts and how the environment work

## Create a canvas

```javascript
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(93, 226, 231);
}
```

Informatii: 
- modificam culoarea cu functia background, specificam valoarea pixelilor
- 255 -- valoarea maxima -> pixelii sunt 100% porniti => alb
- 0 - valoarea minima => negru
- punctul din stanga sus (0,0) dreapta jos (400,400)
- Putem examplifica asta cu niste cercuri 

# Create circles, squares..

```javascript
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(93, 226, 231);
  
  fill(255,255,255);
  circle(200,100,50);
  circle(200,175,100);
  circle(200,300,150);
  
  fill(0,0,0);
  circle(190,100,10);
  circle(210,100,10);
}
```

- circle(pozitia x, pozitia y si raza)
-   fill(0,0,0); pentru culoare
- aici observam ca e importanta ordinea. De ce? Pentru ca functia setup se executa o singura data la inceputul programului iar draw se executa o data cu fiecare frame

## Exemplificam functia draw

```javascript 
function setup() {
  createCanvas(400, 400);
  background(93, 226, 231);

}

function draw() {
  
  fill(239, 195, 202);
  noStroke();
  circle(mouseX,mouseY,100);
  
}
```
 
- mutam background din draw in setup() - ce credeti ca se intampla

```javascript
function setup() {
  createCanvas(400, 400);
  background(93, 226, 231);

}

function draw() {
  if(mouseIsPressed == true)
    {
      fill(239, 195, 202);
    }
  else
    {
      fill(195, 242, 199);
    }
  
  noStroke();
  circle(mouseX,mouseY,100);
  
}
```


## Simple definition of class

```javascript 
let frogs = [];

function setup() {
  createCanvas(400, 400);
  background(93, 226, 231);
  
  for(let i = 0; i<=40; i++)
    {
      x = random(0,400);
      y = random(0,400);
      size = random(50,100);
      
      frogs.push(new Frog(x,y,size));

    }

}

function draw() {
  
  frameRate(5);
  background(93, 226, 231);

  for(let i = 0; i<=40; i++)
  {
    frogs[i].show();
    frogs[i].hop();
  }


}


class Frog{
  constructor(x,y,size)
  {
    this.x = x;
    this.y = y;
    this.size = size;
  }
  
  show()
  {
    textAlign(CENTER,CENTER);
    textSize(this.size);
    text('🐸', this.x, this.y);
  }
  
  hop()
  {
    this.x += random(-10,10);
    this.y += random(-10,10);
  }
}
```

- WIN + . for emoji



# 2nd Part - Game Time

## Defining the Goals

Goal: Create a functioning game using only prompts
RPG Game should have
- a character that moves around a map
- it can interact with objects
- it can talk to NPCc
- has a Menu where we can view our items
- has a Journal to define the current Tasks
- it can fight with enemies and gain XP

The Game should be created where?
- in p5.js
- in Web Editor
- I want to be able to just copy the code and play the game
- Alternatively we can ask Copilot to tell us what we can use for this

## First try with copilot

💡Rembember to use ThinkDeeper !

Let's try different images

```
I want to create a game RPG like using p5.js. The outcome should like the following picture. Please generate the control including collision and controls for me to paste into editor and execute it
```

```
let pacman;
let ghosts = [];
let walls = [];
let dots = [];
let score = 0;
let lives = 3;

function setup() {
  createCanvas(400, 400);
  pacman = new PacMan(200, 300);

  // Create ghosts in the upper part of the canvas.
  ghosts.push(new Ghost(200, 100, 'red'));
  ghosts.push(new Ghost(220, 100, 'pink'));
  ghosts.push(new Ghost(240, 100, 'cyan'));
  ghosts.push(new Ghost(260, 100, 'orange'));

  createMaze();
}

function draw() {
  background(0);
  
  // Update and display Pac-Man.
  pacman.update();
  pacman.display();
  
  // Display maze walls.
  for (let wall of walls) {
    wall.display();
  }
  
  // Display dots and check if Pac-Man eats them.
  for (let i = dots.length - 1; i >= 0; i--) {
    let dot = dots[i];
    dot.display();
    if (pacman.collidesWith(dot)) {
      score += 10;
      // Remove the dot once eaten.
      dots.splice(i, 1);
    }
  }
  
  // Update and show ghosts.
  for (let ghost of ghosts) {
    ghost.update();
    ghost.display();
    // If a ghost catches Pac-Man, reduce a life and reset Pac-Man.
    if (pacman.collidesWith(ghost)) {
      lives--;
      pacman.reset();
      if (lives <= 0) {
        noLoop();
        textSize(32);
        fill(255);
        text("Game Over", width / 2 - 80, height / 2);
      }
    }
  }
  
  displayScore();
  displayLives();
}

// Create a simple maze layout.
function createMaze() {
  // Example walls (x, y, width, height).
  walls.push(new Wall(100, 100, 200, 20));
  walls.push(new Wall(100, 200, 200, 20));
  
  // Create a grid of dots at an offset (so they aren’t drawn on top of the walls).
  for (let i = 10; i < width; i += 20) {
    for (let j = 10; j < height; j += 20) {
      dots.push(new Dot(i, j));
    }
  }
}

// Display the current score in the upper left.
function displayScore() {
  textSize(16);
  fill(255);
  text("Score: " + score, 10, 20);
}

// Display the remaining lives.
function displayLives() {
  textSize(16);
  fill(255);
  text("Lives: " + lives, 10, 40);
}

// Collision detection between a circle and a rectangle.
// It finds the closest point on the rectangle to the circle center and compares the distance.
function circleRectCollide(cx, cy, radius, rx, ry, rw, rh) {
  let closestX = constrain(cx, rx, rx + rw);
  let closestY = constrain(cy, ry, ry + rh);
  let distanceX = cx - closestX;
  let distanceY = cy - closestY;
  return (distanceX * distanceX + distanceY * distanceY) < (radius * radius);
}

// ---------------------------
// Class Definitions
// ---------------------------

class PacMan {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.size = 20;
    this.speed = 2;
  }

  update() {
    // Save previous position to be able to revert if needed.
    let prevX = this.x;
    let prevY = this.y;
    
    // Move using arrow keys.
    if (keyIsDown(LEFT_ARROW)) {
      this.x -= this.speed;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      this.x += this.speed;
    }
    if (keyIsDown(UP_ARROW)) {
      this.y -= this.speed;
    }
    if (keyIsDown(DOWN_ARROW)) {
      this.y += this.speed;
    }
    
    // Check collision with walls using the circle-rectangle collision function.
    for (let wall of walls) {
      if (circleRectCollide(this.x, this.y, this.size / 2, wall.x, wall.y, wall.w, wall.h)) {
        // If colliding, revert to the previous position.
        this.x = prevX;
        this.y = prevY;
        break;
      }
    }
    
    // Constrain Pac-Man within the canvas boundaries.
    this.x = constrain(this.x, this.size / 2, width - this.size / 2);
    this.y = constrain(this.y, this.size / 2, height - this.size / 2);
  }

  display() {
    fill(255, 255, 0);
    ellipse(this.x, this.y, this.size, this.size);
  }

  // Check collision with another circular object (dots or ghosts).
  collidesWith(object) {
    let d = dist(this.x, this.y, object.x, object.y);
    return d < (this.size / 2 + object.size / 2);
  }
  
  reset() {
    this.x = 200;
    this.y = 300;
  }
}

class Ghost {
  constructor(x, y, col) {
    this.x = x;
    this.y = y;
    this.size = 20;
    this.col = col;
    this.speed = 1.5;
    this.direction = int(random(4));
    this.changeDirectionInterval = 30; // Change direction every 30 frames.
  }

  update() {
    if (frameCount % this.changeDirectionInterval === 0) {
      this.direction = int(random(4));
    }
    // Move in one of four directions based on the randomly chosen direction.
    switch (this.direction) {
      case 0:
        this.x += this.speed;
        break;
      case 1:
        this.x -= this.speed;
        break;
      case 2:
        this.y += this.speed;
        break;
      case 3:
        this.y -= this.speed;
        break;
    }
    // Keep the ghost within canvas boundaries.
    this.x = constrain(this.x, this.size / 2, width - this.size / 2);
    this.y = constrain(this.y, this.size / 2, height - this.size / 2);
  }

  display() {
    fill(this.col);
    ellipse(this.x, this.y, this.size, this.size);
  }
}

class Dot {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.size = 5;
  }

  display() {
    fill(255);
    ellipse(this.x, this.y, this.size, this.size);
  }
}

class Wall {
  constructor(x, y, w, h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
  }

  display() {
    fill(0, 0, 255);
    rect(this.x, this.y, this.w, this.h);
  }
}

```


Code generated - 1st Iteration
```javascript
let player;
let obstacles = [];
let playerSpeed = 3;

function setup() {
  createCanvas(800, 600);
  // Create the player at the canvas center; a 30×30 square.
  player = new Player(width / 2, height / 2, 30, 30);
  
  // Add some obstacles to the scene.
  obstacles.push(new Obstacle(150, 200, 100, 100));
  obstacles.push(new Obstacle(400, 100, 150, 50));
  obstacles.push(new Obstacle(500, 350, 50, 150));
}

function draw() {
  background(60, 179, 113); // A greenish background reminiscent of grassy terrain
  
  // Update and show the player.
  player.update();
  player.display();
  
  // Display all obstacles.
  for (let obs of obstacles) {
    obs.display();
  }
}

// The Player class manages movement and collision.
class Player {
  constructor(x, y, w, h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
  }
  
  update() {
    let moveX = 0;
    let moveY = 0;
    
    // Check arrow key inputs.
    if (keyIsDown(LEFT_ARROW)) {
      moveX = -playerSpeed;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      moveX = playerSpeed;
    }
    if (keyIsDown(UP_ARROW)) {
      moveY = -playerSpeed;
    }
    if (keyIsDown(DOWN_ARROW)) {
      moveY = playerSpeed;
    }
    
    // Handle collisions axis by axis (which allows for sliding along walls).
    if (!this.checkCollision(this.x + moveX, this.y)) {
      this.x += moveX;
    }
    if (!this.checkCollision(this.x, this.y + moveY)) {
      this.y += moveY;
    }
  }
  
  // Checks for collisions with any obstacle at the tentative (newX, newY) position.
  checkCollision(newX, newY) {
    for (let obs of obstacles) {
      if (this.collidesWith(obs, newX, newY)) {
        return true;
      }
    }
    return false;
  }
  
  // Simple axis-aligned bounding box (AABB) collision detection.
  collidesWith(obs, x, y) {
    return (x < obs.x + obs.w &&
            x + this.w > obs.x &&
            y < obs.y + obs.h &&
            y + this.h > obs.y);
  }
  
  display() {
    fill(255, 0, 0);
    rect(this.x, this.y, this.w, this.h);
  }
}

// Simple class for an obstacle.
class Obstacle {
  constructor(x, y, w, h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
  }
  
  display() {
    fill(100);
    rect(this.x, this.y, this.w, this.h);
  }
}

```

- let's look at how the player class is constructed
	- it has a `(x,y)` position and also a `Speed`
	- `Speed` is modified when pressing the arrows
- We can also look at collisions
	- it checks the position of the Player with the position of the obstacles (bounding box)



```
Let's add sprite images for the elements. For now we have the Player. and We will have two other types of obsctacles - Tree and House, each one with a spite mage. Also, background should be an image as well
```


💡 This is also good moment to create the sprite images
```
Generate a image with transparent background of fantasy house in pixel style, for a RPG game
```

### Variation on the code with image Sprites
```javascript
let player;
let obstacles = [];
let playerSpeed = 3;

// Sprite images
let bgImg, playerImg, treeImg, houseImg;

function preload() {
  // Load images - ensure these file names are correct and images are in your project folder
  bgImg = loadImage("background.png");
  playerImg = loadImage("player.png");
  treeImg = loadImage("tree.png");
  houseImg = loadImage("house.png");
}

function setup() {
  createCanvas(800, 600);
  
  // Create the player; setting up default dimensions that match your sprite scale
  player = new Player(width / 2, height / 2, 50, 50);
  
  // Create obstacles: trees and houses at different positions
  obstacles.push(new Tree(150, 200));
  obstacles.push(new House(400, 100));
  obstacles.push(new Tree(500, 350));
}

function draw() {
  // Draw the background image scaled to fill the canvas.
  image(bgImg, 0, 0, width, height);

  // Update the player's position and display it
  player.update();
  player.display();
  
  // Display all obstacles
  for (let obs of obstacles) {
    obs.display();
  }
}

// ------------------------
// Player class with sprite
// ------------------------
class Player {
  constructor(x, y, w, h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
  }
  
  update() {
    let moveX = 0;
    let moveY = 0;
    
    // Check arrow keys for movement
    if (keyIsDown(LEFT_ARROW)) {
      moveX = -playerSpeed;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      moveX = playerSpeed;
    }
    if (keyIsDown(UP_ARROW)) {
      moveY = -playerSpeed;
    }
    if (keyIsDown(DOWN_ARROW)) {
      moveY = playerSpeed;
    }
    
    // Handle collisions on each axis separately (allows sliding along obstacles)
    if (!this.checkCollision(this.x + moveX, this.y)) {
      this.x += moveX;
    }
    if (!this.checkCollision(this.x, this.y + moveY)) {
      this.y += moveY;
    }
  }
  
  // Checks the new position against all obstacles
  checkCollision(newX, newY) {
    for (let obs of obstacles) {
      if (this.collidesWith(obs, newX, newY)) {
        return true;
      }
    }
    return false;
  }
  
  // Simple axis-aligned bounding box (AABB) collision detection.
  collidesWith(obs, x, y) {
    return !(
      x + this.w < obs.x ||
      x > obs.x + obs.w ||
      y + this.h < obs.y ||
      y > obs.y + obs.h
    );
  }
  
  // Draw the player using its sprite image
  display() {
    image(playerImg, this.x, this.y, this.w, this.h);
  }
}

// -----------------------------------
// Base class for sprite obstacles
// -----------------------------------
class Obstacle {
  constructor(x, y, w, h, img) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.img = img;
  }
  
  display() {
    image(this.img, this.x, this.y, this.w, this.h);
  }
}

// Tree obstacle; using a preset size and the tree sprite
class Tree extends Obstacle {
  constructor(x, y) {
    // Here 50x80 are assumed dimensions; adjust as necessary
    super(x, y, 50, 80, treeImg);
  }
}

// House obstacle; using a preset size and the house sprite
class House extends Obstacle {
  constructor(x, y) {
    // Here 100x100 are assumed dimensions; adjust as needed
    super(x, y, 100, 100, houseImg);
  }
}

```

Code with update sizes:
```javascript
let player;
let obstacles = [];
let playerSpeed = 3;

// Sprite images
let bgImg, playerImg, treeImg, houseImg;

function preload() {
  // Load images - ensure these filenames are correct and images are in your project folder
  bgImg = loadImage("background.png");
  playerImg = loadImage("player.png");
  treeImg = loadImage("tree.png");
  houseImg = loadImage("house.png");
}

function setup() {
  createCanvas(1000, 1000);
  
  // Create the player at the center of the canvas with size 100 x 100
  player = new Player(width / 2 - 10, height / 2 - 10, 100, 100);
  
  // Create obstacles: trees and houses positioned on the scene
  obstacles.push(new Tree(150, 200));
  obstacles.push(new House(400, 100));
  obstacles.push(new Tree(500, 350));
}

function draw() {
  // Draw the background image scaled to fill 1000x1000 canvas.
  image(bgImg, 0, 0, width, height);

  // Update and show the player.
  player.update();
  player.display();
  
  // Display all obstacles.
  for (let obs of obstacles) {
    obs.display();
  }
}

// ------------------------
// Player class with sprite
// ------------------------
class Player {
  constructor(x, y, w, h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
  }
  
  update() {
    let moveX = 0;
    let moveY = 0;
    
    // Key-based movement
    if (keyIsDown(LEFT_ARROW)) {
      moveX = -playerSpeed;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      moveX = playerSpeed;
    }
    if (keyIsDown(UP_ARROW)) {
      moveY = -playerSpeed;
    }
    if (keyIsDown(DOWN_ARROW)) {
      moveY = playerSpeed;
    }
    
    // Axis-by-axis collision checking allows sliding along obstacles.
    if (!this.checkCollision(this.x + moveX, this.y)) {
      this.x += moveX;
    }
    if (!this.checkCollision(this.x, this.y + moveY)) {
      this.y += moveY;
    }
  }
  
  // Check collision with all obstacles at the new position.
  checkCollision(newX, newY) {
    for (let obs of obstacles) {
      if (this.collidesWith(obs, newX, newY)) {
        return true;
      }
    }
    return false;
  }
  
  // Axis-aligned bounding box (AABB) collision detection.
  collidesWith(obs, x, y) {
    return !(
      x + this.w < obs.x ||
      x > obs.x + obs.w ||
      y + this.h < obs.y ||
      y > obs.y + obs.h
    );
  }
  
  // Draw the player's sprite.
  display() {
    image(playerImg, this.x, this.y, this.w, this.h);
  }
}

// ----------------------------------
// Base class for sprite obstacles
// ----------------------------------
class Obstacle {
  constructor(x, y, w, h, img) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.img = img;
  }
  
  display() {
    image(this.img, this.x, this.y, this.w, this.h);
  }
}

// ----------------------------------
// Tree obstacle (150 x 200)
// ----------------------------------
class Tree extends Obstacle {
  constructor(x, y) {
    super(x, y, 150, 200, treeImg);
  }
}

// ----------------------------------
// House obstacle (250 x 250)
// ----------------------------------
class House extends Obstacle {
  constructor(x, y) {
    super(x, y, 250, 250, houseImg);
  }
}

```


Let's add a NPC that can talk:

Prompt:
```

```



```
// Global variables and settings
let player;
let obstacles = [];
let npc;
let playerSpeed = 3;

// Sprite images
let bgImg, playerImg, treeImg, houseImg, npcImg;

// Dialogue variables
let npcDialogue = "";
let dialogueExpiration = 0;

function preload() {
  // Load images (ensure that these filenames match your project files)
  bgImg = loadImage("background.png");
  playerImg = loadImage("player.png");
  treeImg = loadImage("tree.png");
  houseImg = loadImage("house.png");
  npcImg = loadImage("npc.png");
}

function setup() {
  createCanvas(1000, 1000);
  
  // Random starting position for the Player (ensuring the full 100x100 fits)
  let startX = random(0, width - 100);
  let startY = random(0, height - 100);
  player = new Player(startX, startY, 100, 100);
  
  // Add obstacles (trees and houses) that block movement
  obstacles.push(new Tree(150, 200));
  obstacles.push(new House(400, 100));
  obstacles.push(new Tree(500, 350));
  
  // Create the NPC with a random position
  let npcX = random(0, width - 100);
  let npcY = random(0, height - 100);
  npc = new NPC(npcX, npcY, 100, 100, npcImg);
}

function draw() {
  // Draw the background image scaled to fill the canvas.
  image(bgImg, 0, 0, width, height);
  
  // Update player's movement and check collisions against obstacles.
  player.update();
  
  // Display obstacles (which block the player).
  for (let obs of obstacles) {
    obs.display();
  }
  
  // Display the NPC.
  npc.display();
  
  // Display the player on top of obstacles & NPC.
  player.display();
  
  // Check if the player is near the NPC and display an interaction hint.
  let d = dist(
    player.x + player.w / 2,
    player.y + player.h / 2,
    npc.x + npc.w / 2,
    npc.y + npc.h / 2
  );
  if (d < 150) {
    fill(255);
    textSize(20);
    text("Press E to talk", npc.x, npc.y - 10);
  }
  
  // Display the NPC dialogue if currently active.
  if (millis() < dialogueExpiration) {
    fill(255);
    textSize(32);
    textAlign(CENTER, CENTER);
    text(npcDialogue, width / 2, 50);
  }
}

// ------------------------
// Player class with sprite and collision
// ------------------------
class Player {
  constructor(x, y, w, h) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
  }
  
  update() {
    let moveX = 0;
    let moveY = 0;
    
    if (keyIsDown(LEFT_ARROW)) {
      moveX = -playerSpeed;
    }
    if (keyIsDown(RIGHT_ARROW)) {
      moveX = playerSpeed;
    }
    if (keyIsDown(UP_ARROW)) {
      moveY = -playerSpeed;
    }
    if (keyIsDown(DOWN_ARROW)) {
      moveY = playerSpeed;
    }
    
    // Check collision separately for X and Y movement.
    if (!this.checkCollision(this.x + moveX, this.y)) {
      this.x += moveX;
    }
    if (!this.checkCollision(this.x, this.y + moveY)) {
      this.y += moveY;
    }
  }
  
  // Checks for collisions with all obstacles.
  checkCollision(newX, newY) {
    for (let obs of obstacles) {
      if (this.collidesWith(obs, newX, newY)) {
        return true;
      }
    }
    return false;
  }
  
  // Axis-aligned bounding box collision detection.
  collidesWith(obs, x, y) {
    return !(
      x + this.w < obs.x ||
      x > obs.x + obs.w ||
      y + this.h < obs.y ||
      y > obs.y + obs.h
    );
  }
  
  display() {
    image(playerImg, this.x, this.y, this.w, this.h);
  }
}

// ----------------------------------
// Base class for obstacles (Tree and House)
// ----------------------------------
class Obstacle {
  constructor(x, y, w, h, img) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.img = img;
  }
  
  display() {
    image(this.img, this.x, this.y, this.w, this.h);
  }
}

// Tree obstacle (150 x 200)
class Tree extends Obstacle {
  constructor(x, y) {
    super(x, y, 150, 200, treeImg);
  }
}

// House obstacle (250 x 250)
class House extends Obstacle {
  constructor(x, y) {
    super(x, y, 250, 250, houseImg);
  }
}

// ----------------------------------
// NPC class (interactive, not blocking the player)
// ----------------------------------
class NPC {
  constructor(x, y, w, h, img) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.img = img;
  }
  
  display() {
    image(this.img, this.x, this.y, this.w, this.h);
  }
}

// ------------------------
// Handle key presses for NPC interaction
// ------------------------
function keyPressed() {
  if (key === 'e' || key === 'E') {
    // Check if the player is near the NPC.
    let d = dist(
      player.x + player.w / 2,
      player.y + player.h / 2,
      npc.x + npc.w / 2,
      npc.y + npc.h / 2
    );
    if (d < 150) {
      npcDialogue = "NPC: Hello, traveler!";
      dialogueExpiration = millis() + 3000; // Dialogue stays for 3 seconds.
    }
  }
}

```



