#processing

---
2021-10-27

# ブロック崩しゲームチュートリアル

すんごく分かりやすく教えてくれる。

https://www.greenowl5.com/gprogram/processing/processing140.html

![[Pasted image 20211027161904.png]]

```js
int gseq = 0;
int px = 200;
int py = 420;
int pw = 40;
int ph = 20;
float bx, by, spdx, spdy, lastx, lasty;
int bw = 7;
int bh = 7;
int blw = 78;
int blh = 30;
int[] blf = new int[25];
int score;
int bexist;
int mcnt;

void setup() {
  size(400,500);
  noStroke();
  colorMode(HSB, 100, 100, 100);
  gameInit();
}

void draw() {
  background(0);
  if(gseq == 0) {
    gameTitle();
  } else if(gseq == 1) {
    gamePlay();
  } else {
    gameOver();
  }
}

void gameInit() {
  gseq = 0;
  bx = 100;
  by = 250;
  spdx = 2;
  spdy = 2;
  score = 0;
  bexist = 1;
  mcnt = 0;
  for(int i=0; i<25; i++) blf[i] = 1;
}

void gameTitle() {
  playerMove();
  playerDisp();
  blockDisp();
  scoreDisp();
  textSize(60);
  fill(0, 0, 100);
  text("BREAK OUT", 30, 300);
  mcnt++;
  if( mcnt%60 < 40 ) {
    textSize(20);
    fill(20, 100, 100);
    text("Click to start!", 140, 360);
  }
}

void gamePlay() {
  playerMove();
  playerDisp();
  ballMove();
  ballDisp();
  blockDisp();
  scoreDisp();
}

void gameOver() {
  textSize(50);
  fill(0, 100, 100);
  text("GAME OVER", 60, 300);
  
  mcnt++;
  if( mcnt%60 < 40 ) {
    textSize(20);
    fill(20, 100, 100);
    text("Click to retry!", 140, 360);
  }
}


void playerMove(){
  px = mouseX;
  if( (px+pw) > width ) px = width - pw;
}
void playerDisp() {
  fill(0, 0, 100);
  rect(px, py, pw, ph, 5);
}
void ballMove() {
  lastx = bx;
  lasty = by;
  
  bx += spdx;
  by += spdy;
  if( 0 > by ) {
    spdy *= -1;
  }
  if( height-bh < by ) {
    //spdy *= -1;
    gseq = 2;
  }
  if( 0 > bx || width-bw < bx) spdx *= -1;
  if( bx > px && bx < px+pw && by+3 > py && by < py+ph){
    spdy = - Math.abs(spdy);
    if(bexist == 0) {
      for(int i=0; i<25; i++) blf[i] = 1;
    }
  }
}
void ballDisp() {
  imageMode(CENTER);
  fill(0, 0, 100);
  rect(bx, by, bw, bh);
  imageMode(CORNER);
}
void blockDisp() {  
  int xx, yy;
  bexist = 0;
  for(int i=0; i<=24; i++) {
    xx = (i%5) * (blw+2);
    yy = (i/5) * (blh+2) + 50;

    if(blf[i] == 1) {      
      blockHitCheck(i, xx, yy);
      fill((i/5)*15, 100, 100);
      rect(xx, yy, blw, blh);
      bexist = 1;
    }
  }
}
void blockHitCheck(int i, int xx, int yy) {
  if( !(bx > xx && bx < xx+blw &&  by > yy && by < yy+blh)) return;

  blf[i] = 0;
  score += 10;
  if( i < 10 ) score += 10;
  
  if(lastx > xx && lastx < xx+blw){
    spdy *= -1;
  } else if(lasty > yy && lasty < yy+blh) {
    spdx *= -1;
  } else {
    spdx *= -1;
    spdy *= -1;
  }  
}
void scoreDisp() {
  textSize(24);
  fill(0, 0, 100);
  text("score: " + score, 10, 25);
}
void mousePressed() {
  if(gseq == 0) {
    gseq = 1;
  }
  if(gseq == 2) {
    gameInit();
  }
}
```