var player, playerani;
var background1,backgroundimg;
var tree, treeimg, treegroup;
var stone,stoneimg, stonegroup;
var invisibleground;
var coins, coinsimg, coingroup;
var birds, birdsimg, birdsgroup;
var snake, snakeimg, snakegroup;
var score = 0;var coinscollect = 0;
var gameover, gameoverimg;
var diamonds, diamondimg;
var treasure, treasureimg;

function preload() {
  
  playerani = loadAnimation("https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/9f72b06a-17df-4f3e-a98e-21e54f297ff8.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/82177c00-cada-4060-baae-8157fb5f5b8f.png");
  
  backgroundimg = loadImage("https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/7eb862b7-b8f0-45ea-9f7b-dd813215c732.jpg");
  
  treeimg = loadImage("https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/52310eaf-a38c-4240-bb20-320a39d25a25.png");
  
  stoneimg = loadImage("https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/1d999673-a4f4-4251-bc13-ced74cc71423.png");
  
  gameoverimg = loadImage("https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/0f217b09-419e-4070-852b-8d2c5d5d8e71.png")
  
  coinsimg = loadAnimation("https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/850204a6-3046-42c9-8fc3-1a8c572d1128.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/02425bc6-3f77-491d-bb70-b5e1a31392de.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/a15ea0ed-49ad-4c42-a4c3-326c32868af3.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/62d660d9-521b-4d1a-ba75-0a9835003427.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/bbe08067-84ba-4353-b67c-691fc34bd4d9.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/5aca8460-8c55-45ab-b71c-8de6b06bb464.png");
  
  birdsimg = loadAnimation("https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/feefd90c-a267-496c-b5df-716e4802eb63.png", "https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/9e60d757-9975-49f8-9f29-56471d247c62.png", "https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/86c2521b-3079-4b44-87c3-349859b4ae2c.png");
  
  snakeimg=loadAnimation("https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/3206f435-25d5-476d-88e6-d662d19d0221.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/84adc2a0-7f93-4bca-be98-3954ee398242.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/33a42074-1d5a-4cc1-b783-1bb84becfd4f.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/0cab61bd-e58b-4743-8291-1279c23e4c08.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/3206f435-25d5-476d-88e6-d662d19d0221.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/99772206-13d5-4427-8594-c33041ad38ec.png","https://assets.editor.p5js.org/5f0d67398e8b0f0019e9adae/98cd8f2c-3910-407f-9fe4-6d2b4928ab23.png")
}

function setup() {
  
   background1 = createSprite(200,300,10,10);
   background1.addImage(backgroundimg);
   background1.velocityX = -(6 + 3*score/100);
  
   player = createSprite(35,310,10,10);
   player.addAnimation("playerani",playerani);
   player.scale = 0.4;
  
 
  
   invisibleground = createSprite(200,355,800,5);
  
   invisibleground.visible = false;
  
   treegroup = new Group();
   stonegroup = new Group();
   coingroup = new Group();
   birdsgroup = new Group();
   snakegroup = new Group();
  
   coinscollect = 0;
  
}

function draw() {
  background("blue");
  
  spawntree();
  spawnStones();
  spawncoins1();
  spawncoins2();
  spawnbirds();
  spawnsnake();
  
   player.collide(invisibleground);
  
  
    textSize(20);
    fill("white")
    text("Score: "+ score, 20,50);
    text("Coins Collected: "+ coinscollect,20,100);
  
    score = score + Math.round(getFrameRate()/60);
  
   if (background1.x<0 ){
       background1.x=background1.width/2 
   }
  
  if (player.isTouching(coingroup)) {
       coingroup.destroyEach();
    coinscollect = coinscollect + 1;
    score = score + 20;
  }
  
    if (keyDown("space") && player.y >=310 ) {
         player.velocityY = -23;
    }
  
  if (player.isTouching(stonegroup) ||player.isTouching(snakegroup))      {
       player.velocity = 0;
       gameover = createSprite(200,200,10,10);
       gameover.addImage(gameoverimg);
       gameover.scale = 2.7;
      }
  
  player.velocityY = player.velocityY + 2;
  
  drawSprites();
}

function spawntree() {
    if(frameCount % 400 === 0) {
     tree = createSprite(800,250,10,40);
     tree.addImage(treeimg);
     tree.velocityX = -(6 + 3*score/100);
     tree.scale = 0.3; 
     tree.lifetime = 220;

  treegroup.add(tree);
}
}

function spawnStones() {
  if(frameCount % 250 === 0) {
      stone = createSprite(800,330,10,40);
      stone.addImage(stoneimg);
      stone.velocityX = -(6 + 3*score/100);
      stone.scale = 0.1; 
      stone.lifetime = 220;
    
    //stone.debug = true;
    stone.setCollider("rectangle",0,0,30,30)

  stonegroup.add(stone);
}
}

function spawncoins1() {
    if(frameCount % 125 === 0) {
      coins = createSprite(800,230,10,40);
      coins.addAnimation("coinsimg",coinsimg);
      coins.velocityX = -(6 + 3*score/100);
      coins.scale = 0.1; 
      coins.lifetime = 220;

  coingroup.add(coins);
}
}

function spawncoins2() {
    if(frameCount % 135 === 0) {
      coins = createSprite(800,250,10,40);
      coins.addAnimation("coinsimg",coinsimg);
      coins.velocityX = -(6 + 3*score/100);
      coins.scale = 0.1; 
      coins.lifetime = 220;

  coingroup.add(coins);
}
}

function spawnbirds() {
      if(frameCount % 110 === 0) {
      birds = createSprite(800,200,10,40);
      birds.addAnimation("birdsimg",birdsimg);
      birds.velocityX = -(6 + 3*score/100);
      birds.scale = 0.250; 
      birds.lifetime = 220;

  birdsgroup.add(birds);
}  
}

function spawnsnake() {
    if(frameCount % 185 === 0) {
      snake = createSprite(800,320,10,40);
      snake.addAnimation("snakeimg",snakeimg);
      snake.velocityX = -(6 + 3*score/100);
      snake.scale = 0.250; 
      snake.lifetime = 220;
      
     // snake.debug = true;
      snake.setCollider("rectangle",0,0,20,20)

  snakegroup.add(snake);
}
}
