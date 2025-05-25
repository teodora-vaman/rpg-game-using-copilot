
Before starting this workshop: **Create Copilot account** 

Links:
[Reference](https://p5js.org/reference/)
[p5.js Web Editor](https://editor.p5js.org/)
[color picker - Search](https://www.bing.com/search?pglt=2211&q=color+picker&cvid=191ea46face54bf1a8a32518f4cde092&gs_lcrp=EgRlZGdlKgYIABBFGDsyBggAEEUYOzIGCAEQABhAMgYIAhAAGEAyBggDEC4YQDIGCAQQABhAMgYIBRAAGEAyBggGEAAYQDIGCAcQABhAMgYICBBFGDwyCAgJEOkHGPxV0gEIMTUzNWowajGoAgiwAgE&FORM=ANNAB1&PC=U531)

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

- Press `WIN + .` for emoji



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


## Writing Good Prompts 

1. **Be Clear and Specific**: When crafting prompts, clarity and specificity are crucial. Clearly state what you need Copilot to do, whether it's creating, summarizing, editing, or transforming content 
2. **Provide Context**: Include relevant context in your prompts to help Copilot understand the task better. For example, if you're asking for a summary, mention the specific document or topic, or provide a a image for context

3. **Iterative Development**: Use an iterative approach to refine your prompts. Start with a broad request and then narrow it down with follow-up questions to get more detailed and accurate responses 

4. **Use Examples and Constraints**: Incorporate specific examples and constraints in your prompts to guide Copilot's responses. This helps in generating more precise and relevant outcomes 

5. **Start Fresh for New Tasks**: When switching tasks, avoid interrupting the current conversation. Instead, type "new topic" to start fresh and provide clear instructions for the new task 

6. **Know Copilot's Limitations**: Understand that Copilot is limited to the current conversation. Providing detailed and specific prompts helps in getting better results 

## First try with copilot

💡Remember to use Think Deeper !

Let's try different images

![PacMan](https://github.com/user-attachments/assets/20175e7c-b213-4f69-937c-c8ce7142e33e)

```
I want to create a game like [Pac-Man] using p5.js. The outcome should look like the following picture. Please generate the control including collision and controls for me to paste into editor and execute it
```

```javascript
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


## Now let's try to create the RPG

Starting from this image:

![RPG_Start](https://github.com/user-attachments/assets/6ec6c35e-e417-454e-a4f3-82c0dd911e22)


Prompt:

```
I want to create a game RPG like using p5.js. The outcome should look like the following picture. Please generate the control including collision and controls for me to paste into editor and execute it
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



## Adding images to the game

```
Let's add sprite images for the elements. For now we have the Player. and We will have two other types of obsctacles - Tree and House, each one with a spite mage. Also, background should be an image as well
```

Prompt:
💡 This is also good moment to create the sprite images

📌Open a separate chat for creating the images so we don't mix the contexts
```
Generate a image with transparent background of fantasy house in pixel style, for a RPG game
```


Prompt:
```
Create a pixel art charcter for a 2d RPG game. Character is [a girl with brown hair and glasses](insert your own characteristics here). Make the background transparent.
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

Code with update sizes - better to use this one
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
Create a pixel art charcter for a 2d RPG game. Character is a knight with green armor. Make the background transparent.
```



```javascript
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



## Adding a Menu with Items and tasks

Prompt:
```
Let's add a Menu to the game. Pressing ESC will pause the game and open the Menu. It will be semi-transparent black background. On the menu we can see 3 things: Information about our Player, a button for Items and a button for Tasks. Information about the Player will contain: Name = Cappuccina, HP = 50, Gold = 0. Task will contain a list of tasks, intial task [Talk with the people around you]. Items is a list, initial item [sword]. Pressing ESC again will exit the menu and continuing the game
```


Game now:

```javascript

// Global game variables
let player;
let obstacles = [];
let npc;
let playerSpeed = 3;

// Menu Variables
let menuOpen = false;
let currentTab = "items"; // can be "items" or "tasks"
let gameItems = ["sword"];
let gameTasks = ["Talk with the people around you"];

// Sprite images
let bgImg, playerImg, treeImg, houseImg, npcImg;

// Dialogue variables for the NPC chat
let npcDialogue = "";
let dialogueExpiration = 0;

function preload() {
  // Load images - please ensure these files are in your project directory.
  bgImg    = loadImage("background.png");
  playerImg = loadImage("player.png");
  treeImg   = loadImage("tree.png");
  houseImg  = loadImage("house.png");
  npcImg    = loadImage("npc.png");
}

function setup() {
  createCanvas(1000, 1000);
  
  // Random starting position for the player (ensuring a full 100x100 sprite fits)
  let startX = random(0, width - 100);
  let startY = random(0, height - 100);
  player = new Player(startX, startY, 100, 100);
  
  // Add obstacles; these block movement in the game.
  obstacles.push(new Tree(150, 200));
  obstacles.push(new House(400, 100));
  obstacles.push(new Tree(500, 350));
  
  // Create an NPC at a random position (its sprite is also 100x100).
  let npcX = random(0, width - 100);
  let npcY = random(0, height - 100);
  npc = new NPC(npcX, npcY, 100, 100, npcImg);
}

function draw() {
  // Draw the background image scaled to fill the canvas.
  image(bgImg, 0, 0, width, height);
  
  // Update the player (and hence game objects) only if the menu is not open.
  if (!menuOpen) {
    player.update();
  }
  
  // Draw obstacles, NPC, and Player.
  for (let obs of obstacles) {
    obs.display();
  }
  npc.display();
  player.display();
  
  // If the player is near the NPC (within 150 pixels) display an interaction hint.
  let d = dist(
    player.x + player.w / 2,
    player.y + player.h / 2,
    npc.x + npc.w / 2,
    npc.y + npc.h / 2
  );
  if (d < 150 && !menuOpen) {
    fill(255);
    textSize(20);
    textAlign(CENTER, CENTER);
    text("Press E to talk", npc.x + npc.w / 2, npc.y - 10);
  }
  
  // Show the NPC dialogue (only when the game isn't paused).
  if (!menuOpen && millis() < dialogueExpiration) {
    fill(255);
    textSize(32);
    textAlign(CENTER, CENTER);
    text(npcDialogue, width / 2, 50);
  }
  
  // If the menu is open, draw the menu overlay.
  if (menuOpen) {
    drawMenu();
  }
}

function keyPressed() {
  // Toggle the menu when ESC is pressed.
  if (keyCode === ESCAPE) {
    menuOpen = !menuOpen;
    return;
  }
  
  // Only allow NPC interaction when the game is not paused.
  if (!menuOpen && (key === 'e' || key === 'E')) {
    // Check if the player is close enough to the NPC.
    let d = dist(
      player.x + player.w / 2,
      player.y + player.h / 2,
      npc.x + npc.w / 2,
      npc.y + npc.h / 2
    );
    if (d < 150) {
      npcDialogue = "NPC: Hello, traveler!";
      dialogueExpiration = millis() + 3000; // Display dialogue for 3 seconds.
    }
  }
}

function mousePressed() {
  // If the menu is open, check if one of the buttons was clicked.
  if (menuOpen) {
    // Button boundaries for Items button.
    let itemsButtonX = 50;
    let itemsButtonY = 170;
    let itemsButtonW = 120;
    let itemsButtonH = 50;
    
    // Button boundaries for Tasks button.
    let tasksButtonX = 200;
    let tasksButtonY = 170;
    let tasksButtonW = 120;
    let tasksButtonH = 50;
    
    // If the mouse is within the Items button, switch to the Items tab.
    if (
      mouseX >= itemsButtonX && mouseX <= itemsButtonX + itemsButtonW &&
      mouseY >= itemsButtonY && mouseY <= itemsButtonY + itemsButtonH
    ) {
      currentTab = "items";
    }
    
    // If the mouse is within the Tasks button, switch to the Tasks tab.
    if (
      mouseX >= tasksButtonX && mouseX <= tasksButtonX + tasksButtonW &&
      mouseY >= tasksButtonY && mouseY <= tasksButtonY + tasksButtonH
    ) {
      currentTab = "tasks";
    }
  }
}

// Function to draw the menu overlay.
function drawMenu() {
  // Semi-transparent black background.
  noStroke();
  fill(0, 0, 0, 150);
  rect(0, 0, width, height);
  
  // Draw the Player Information box.
  fill(255);
  rect(50, 50, 300, 100, 10);  // Rounded corners for a nicer look.
  fill(0);
  textSize(20);
  textAlign(LEFT, TOP);
  text("Player Info", 60, 60);
  text("Name: Cappuccina", 60, 90);
  text("HP: 50", 60, 120);
  text("Gold: 0", 60, 150);
  
  // Draw buttons for Items and Tasks.
  // Items button.
  let itemsButtonX = 50;
  let itemsButtonY = 170;
  let itemsButtonW = 120;
  let itemsButtonH = 50;
  if (currentTab === "items") {
    fill(180);
  } else {
    fill(100);
  }
  rect(itemsButtonX, itemsButtonY, itemsButtonW, itemsButtonH, 10);
  fill(255);
  textSize(20);
  textAlign(CENTER, CENTER);
  text("Items", itemsButtonX + itemsButtonW / 2, itemsButtonY + itemsButtonH / 2);
  
  // Tasks button.
  let tasksButtonX = 200;
  let tasksButtonY = 170;
  let tasksButtonW = 120;
  let tasksButtonH = 50;
  if (currentTab === "tasks") {
    fill(180);
  } else {
    fill(100);
  }
  rect(tasksButtonX, tasksButtonY, tasksButtonW, tasksButtonH, 10);
  fill(255);
  text("Tasks", tasksButtonX + tasksButtonW / 2, tasksButtonY + tasksButtonH / 2);
  
  // Draw the content list based on the active tab.
  textAlign(LEFT, TOP);
  fill(255);
  textSize(20);
  if (currentTab === "items") {
    text("Items List:", 50, 240);
    let yPos = 270;
    for (let item of gameItems) {
      text("- " + item, 50, yPos);
      yPos += 30;
    }
  } else if (currentTab === "tasks") {
    text("Task List:", 50, 240);
    let yPos = 270;
    for (let task of gameTasks) {
      text("- " + task, 50, yPos);
      yPos += 30;
    }
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
    
    // Player movement with arrow keys.
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
    
    // Check collisions axis-by-axis (allowing sliding along obstacles).
    if (!this.checkCollision(this.x + moveX, this.y)) {
      this.x += moveX;
    }
    if (!this.checkCollision(this.x, this.y + moveY)) {
      this.y += moveY;
    }
  }
  
  // Check for collisions with every obstacle.
  checkCollision(newX, newY) {
    for (let obs of obstacles) {
      if (this.collidesWith(obs, newX, newY)) {
        return true;
      }
    }
    return false;
  }
  
  // Axis-Aligned Bounding Box (AABB) collision detection.
  collidesWith(obs, x, y) {
    return !(
      x + this.w < obs.x ||
      x > obs.x + obs.w ||
      y + this.h < obs.y ||
      y > obs.y + obs.h
    );
  }
  
  // Render the player sprite.
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
// NPC class (interactive, does not block the player)
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

```


## Adding an Enemy and Battle Scene

💡Remember to upload enemy.png image

Prompts:

```
Add a new type of object: Enemy. The enemy will move randomly accross the scene. It will have an image as well. Touching the enemy will trigger Battle scene.
```

Battle scene prompt: - Paste the image as well
```
Let's create a battle scene similar to this image, just with two options Fight and Do Nothing. Fight will take 10 hp from the enemy. Pressing it will display a message on the screen "You attack viciously with your sword". After that is the Enemy's turn. It will to random damage between 0 and 10. Player has starting HP of 100, Enemy has HP of 50. If you win the fight the enemy dissapears HP is updated in the Menu and you win 10 Gold.
```


![battleScene](https://github.com/user-attachments/assets/e124ea8a-38cf-4f0f-aaac-b7ae6a371530)


Code in this point:
```javascript
// Global game variables
let player;
let obstacles = [];
let npc;
let enemy; // The enemy object; may be null after defeat.
let playerSpeed = 3;

// Menu Variables
let menuOpen = false;
let currentTab = "player"; // Options: "player", "items", "tasks"
let gameItems = ["sword"];
let gameTasks = ["Talk with the people around you"];

// Game state: whether a battle is ongoing.
let inBattle = false;

// Battle-related variables
let battleMessage = "";
let playerHP = 100;
let enemyHP = 50;
let playerGold = 0;

// Sprite images
let bgImg, playerImg, treeImg, houseImg, npcImg, enemyImg;

// Dialogue variables for NPC interaction
let npcDialogue = "";
let dialogueExpiration = 0;

function preload() {
  // Load images (ensure these filenames match your project files)
  bgImg    = loadImage("background.png");
  playerImg = loadImage("player.png");
  treeImg   = loadImage("tree.png");
  houseImg  = loadImage("house.png");
  npcImg    = loadImage("npc.png");
  enemyImg  = loadImage("enemy.png");
}

function setup() {
  createCanvas(1000, 1000);
  
  // Random starting position for the player (ensuring a 100x100 sprite fits)
  let startX = random(0, width - 100);
  let startY = random(0, height - 100);
  player = new Player(startX, startY, 100, 100);
  
  // Add obstacles (which block movement)
  obstacles.push(new Tree(150, 200));
  obstacles.push(new House(400, 100));
  obstacles.push(new Tree(500, 350));
  
  // Create an NPC at a random position (sprite sized 100x100)
  let npcX = random(0, width - 100);
  let npcY = random(0, height - 100);
  npc = new NPC(npcX, npcY, 100, 100, npcImg);
  
  // Create an Enemy that roams the scene randomly, also 100x100.
  // The enemy will have starting HP of 50.
  let enemyX = random(0, width - 100);
  let enemyY = random(0, height - 100);
  enemy = new Enemy(enemyX, enemyY, 100, 100, enemyImg);
}

function draw() {
  if (inBattle) {
    // When in battle, let the battle scene manage drawing.
    drawBattleScene();
    return;
  }
  
  // Exploration mode
  image(bgImg, 0, 0, width, height);
  
  // Update game objects only if the menu is closed.
  if (!menuOpen) {
    player.update();
    if (enemy) {
      enemy.update();
    }
  }
  
  // Draw obstacles.
  for (let obs of obstacles) {
    obs.display();
  }
  
  // Display NPC and enemy (enemy might be null after defeat).
  npc.display();
  if (enemy) {
    enemy.display();
  }
  
  // Display the player on top.
  player.display();
  
  // Check for NPC interaction hint (only if not paused).
  let dNPC = dist(
    player.x + player.w / 2,
    player.y + player.h / 2,
    npc.x + npc.w / 2,
    npc.y + npc.h / 2
  );
  if (dNPC < 150 && !menuOpen) {
    fill(255);
    textSize(20);
    textAlign(CENTER, CENTER);
    text("Press E to talk", npc.x + npc.w / 2, npc.y - 10);
  }
  
  // Show NPC dialogue if active.
  if (!menuOpen && millis() < dialogueExpiration) {
    fill(255);
    textSize(32);
    textAlign(CENTER, CENTER);
    text(npcDialogue, width / 2, 50);
  }
  
  // Check for enemy collision – touching the enemy triggers battle.
  if (!menuOpen && enemy && collisionRect(player, enemy)) {
    inBattle = true;
    // Reset battle variables.
    enemyHP = 50;
    battleMessage = "";
  }
  
  // Draw the menu overlay if the menu is open.
  if (menuOpen) {
    drawMenu();
  }
}

function keyPressed() {
  // In battle mode, we now use the two on-screen buttons.
  if (inBattle) {
    return; // Ignore key presses during battle.
  }
  
  // Toggle the pause menu when ESC is pressed.
  if (keyCode === ESCAPE) {
    menuOpen = !menuOpen;
    return;
  }
  
  // Allow NPC interaction in exploration mode.
  if (!menuOpen && (key === 'e' || key === 'E')) {
    let d = dist(
      player.x + player.w / 2,
      player.y + player.h / 2,
      npc.x + npc.w / 2,
      npc.y + npc.h / 2
    );
    if (d < 150) {
      npcDialogue = "NPC: Hello, traveler!";
      dialogueExpiration = millis() + 3000; // Dialogue lasts 3 seconds.
    }
  }
}

function mousePressed() {
  if (inBattle) {
    // Handle battle option clicks.
    let buttonW = 150;
    let buttonH = 50;
    let spacing = 50;
    let totalWidth = buttonW * 2 + spacing;
    let startX = (width - totalWidth) / 2;
    // Position the battle buttons near the bottom.
    let buttonY = height - 150;
    
    // "Fight" button.
    if (
      mouseX >= startX && mouseX <= startX + buttonW &&
      mouseY >= buttonY && mouseY <= buttonY + buttonH
    ) {
      processBattle('fight');
      return;
    }
    // "Do Nothing" button.
    if (
      mouseX >= startX + buttonW + spacing && mouseX <= startX + buttonW + spacing + buttonW &&
      mouseY >= buttonY && mouseY <= buttonY + buttonH
    ) {
      processBattle('nothing');
      return;
    }
  }
  
  // Handle menu button clicks if the menu is open.
  if (menuOpen) {
    let buttonW = 150;
    let buttonH = 50;
    let spacing = 20;
    let totalWidth = 3 * buttonW + 2 * spacing;
    let startX = (width - totalWidth) / 2;
    let centerY = (height - buttonH) / 2;
    
    // Player Info button.
    if (
      mouseX >= startX && mouseX <= startX + buttonW &&
      mouseY >= centerY && mouseY <= centerY + buttonH
    ) {
      currentTab = "player";
    }
    // Items button.
    else if (
      mouseX >= startX + buttonW + spacing &&
      mouseX <= startX + buttonW + spacing + buttonW &&
      mouseY >= centerY && mouseY <= centerY + buttonH
    ) {
      currentTab = "items";
    }
    // Tasks button.
    else if (
      mouseX >= startX + 2 * (buttonW + spacing) &&
      mouseX <= startX + 2 * (buttonW + spacing) + buttonW &&
      mouseY >= centerY && mouseY <= centerY + buttonH
    ) {
      currentTab = "tasks";
    }
  }
}

// AABB collision detection between two rectangular objects.
function collisionRect(a, b) {
  return !(
    a.x + a.w < b.x ||
    a.x > b.x + b.w ||
    a.y + a.h < b.y ||
    a.y > b.y + b.h
  );
}

//────────────────────────────────────────────
// Battle Scene Functions
//────────────────────────────────────────────

function drawBattleScene() {
  // Draw a dark battle background.
  background(80, 0, 0);
  
  // Battle title.
  fill(255);
  textSize(48);
  textAlign(CENTER, TOP);
  text("Battle!", width / 2, 20);
  
  // Draw player's sprite on the left.
  image(playerImg, 100, height / 2 - 100, 150, 150);
  textSize(24);
  textAlign(LEFT, TOP);
  text("Player HP: " + playerHP, 100, height / 2 + 60);
  
  // Draw enemy's sprite on the right if it exists.
  if (enemy) {
    image(enemyImg, width - 250, height / 2 - 100, 150, 150);
    textAlign(RIGHT, TOP);
    text("Enemy HP: " + enemyHP, width - 250, height / 2 + 60);
  }
  
  // Display the battle message.
  textSize(28);
  textAlign(CENTER, CENTER);
  text(battleMessage, width / 2, height / 2 - 200);
  
  // If enemy is still active, show the two battle option buttons.
  if (enemy) {
    let buttonW = 150;
    let buttonH = 50;
    let spacing = 50;
    let totalWidth = buttonW * 2 + spacing;
    let startX = (width - totalWidth) / 2;
    let buttonY = height - 150;
    
    // Fight button.
    fill(100);
    rect(startX, buttonY, buttonW, buttonH, 10);
    fill(255);
    textSize(20);
    textAlign(CENTER, CENTER);
    text("Fight", startX + buttonW / 2, buttonY + buttonH / 2);
    
    // Do Nothing button.
    fill(100);
    rect(startX + buttonW + spacing, buttonY, buttonW, buttonH, 10);
    fill(255);
    text("Do Nothing", startX + buttonW + spacing + buttonW / 2, buttonY + buttonH / 2);
  }
}

function processBattle(choice) {
  // Ensure enemy exists.
  if (!enemy) return;
  
  if (choice === 'fight') {
    battleMessage = "You attack viciously with your sword.";
    enemyHP -= 10;
    
    // Check for enemy defeat.
    if (enemyHP <= 0) {
      battleMessage += "\nYou defeated the enemy! You win 10 Gold.";
      playerGold += 10;
      enemy = null;  // Remove the enemy.
      // Exit battle scene after a short delay.
      setTimeout(() => {
        inBattle = false;
        battleMessage = "";
      }, 2000);
      return;
    }
  } else if (choice === 'nothing') {
    battleMessage = "You do nothing.";
  }
  
  // Enemy's turn: deal random damage between 0 and 10.
  let damage = Math.floor(random(0, 11));
  playerHP -= damage;
  battleMessage += "\nEnemy attacks and deals " + damage + " damage.";
  
  // Check if the player has been defeated.
  if (playerHP <= 0) {
    battleMessage += "\nYou have been defeated!";
    // For this demo, exit battle scene after a brief delay.
    setTimeout(() => {
      inBattle = false;
      battleMessage = "";
    }, 2000);
  }
}

//────────────────────────────────────────────
// Menu Drawing
//────────────────────────────────────────────

function drawMenu() {
  // Semi-transparent black overlay.
  noStroke();
  fill(0, 0, 0, 150);
  rect(0, 0, width, height);
  
  // Draw a Player Info box.
  fill(255);
  rect(50, 50, 300, 100, 10);
  fill(0);
  textSize(20);
  textAlign(LEFT, TOP);
  // Display updated HP and Gold.
  text("Player Info", 60, 60);
  text("Name: Cappuccina", 60, 90);
  text("HP: " + playerHP, 60, 120);
  text("Gold: " + playerGold, 60, 150);
  
  // Draw three centered menu buttons: Player Info, Items, Tasks.
  let buttonW = 150;
  let buttonH = 50;
  let spacing = 20;
  let totalWidth = 3 * buttonW + 2 * spacing;
  let startX = (width - totalWidth) / 2;
  let centerY = (height - buttonH) / 2;
  
  // Player Info button.
  fill(currentTab === "player" ? 180 : 100);
  rect(startX, centerY, buttonW, buttonH, 10);
  fill(255);
  textSize(20);
  textAlign(CENTER, CENTER);
  text("Player Info", startX + buttonW / 2, centerY + buttonH / 2);
  
  // Items button.
  let itemsButtonX = startX + buttonW + spacing;
  fill(currentTab === "items" ? 180 : 100);
  rect(itemsButtonX, centerY, buttonW, buttonH, 10);
  fill(255);
  text("Items", itemsButtonX + buttonW / 2, centerY + buttonH / 2);
  
  // Tasks button.
  let tasksButtonX = startX + 2 * (buttonW + spacing);
  fill(currentTab === "tasks" ? 180 : 100);
  rect(tasksButtonX, centerY, buttonW, buttonH, 10);
  fill(255);
  text("Tasks", tasksButtonX + buttonW / 2, centerY + buttonH / 2);
  
  // Display content below the buttons based on the active tab.
  fill(255);
  textSize(20);
  textAlign(CENTER, TOP);
  let contentY = centerY + buttonH + 30;
  if (currentTab === "player") {
    text("Name: Cappuccina\nHP: " + playerHP + "\nGold: " + playerGold, width / 2, contentY);
  } else if (currentTab === "items") {
    let itemsText = "Items List:\n";
    for (let item of gameItems) {
      itemsText += "- " + item + "\n";
    }
    text(itemsText, width / 2, contentY);
  } else if (currentTab === "tasks") {
    let tasksText = "Tasks List:\n";
    for (let task of gameTasks) {
      tasksText += "- " + task + "\n";
    }
    text(tasksText, width / 2, contentY);
  }
}

//────────────────────────────────────────────
// Game Object Classes
//────────────────────────────────────────────

// Player class manages movement, collision, and sprite display.
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
    
    // Axis-by-axis collision checking (allows sliding along obstacles).
    if (!this.checkCollision(this.x + moveX, this.y)) {
      this.x += moveX;
    }
    if (!this.checkCollision(this.x, this.y + moveY)) {
      this.y += moveY;
    }
  }
  
  checkCollision(newX, newY) {
    for (let obs of obstacles) {
      if (this.collidesWith(obs, newX, newY)) {
        return true;
      }
    }
    return false;
  }
  
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

// Base class for obstacles (Tree and House)
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

// NPC class (interactive, does not block the player)
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

// Enemy class: moves randomly; colliding triggers a battle.
class Enemy {
  constructor(x, y, w, h, img) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.img = img;
    this.vx = random(-2, 2);
    this.vy = random(-2, 2);
    this.changeTimer = int(random(30, 100)); // Frames until direction change.
  }
  
  update() {
    this.changeTimer--;
    if (this.changeTimer <= 0) {
      this.vx = random(-2, 2);
      this.vy = random(-2, 2);
      this.changeTimer = int(random(30, 100));
    }
    
    this.x += this.vx;
    this.y += this.vy;
    
    // Bounce off the edges.
    if (this.x < 0) {
      this.x = 0;
      this.vx *= -1;
    }
    if (this.x > width - this.w) {
      this.x = width - this.w;
      this.vx *= -1;
    }
    if (this.y < 0) {
      this.y = 0;
      this.vy *= -1;
    }
    if (this.y > height - this.h) {
      this.y = height - this.h;
      this.vy *= -1;
    }
  }
  
  display() {
    image(this.img, this.x, this.y, this.w, this.h);
  }
}

```
