---
toc: true
layout: post
author: 山拓
description:
categories: [processing]
title: 反応拡散方程式
---

## 概要
反応拡散方程式は動物の縞模様など発達生物学において非常に重要である。生物の縞模様がシミュレーションと一致することを示したのが、阪大の近藤先生であったりする。
- [こんどうしげるの「生命科学の明日はどっちだ？」](https://www.fbs-osaka-kondolabo.net/kondo-s-blog)

今回は、そんな反応拡散方程式をProcessingで実装してみた。左クリック、左ドラッグで描き、右ドラッグで長方形の範囲を消去できる。

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/processing.js/1.6.6/processing.min.js"></script>
<div style="text-align:center">
<script type="text/processing">
//Reaction Diffusion Simulation

float[][][] field;
float[][][] next_field;

float Da=1.0;
float Db=0.5;
float f=0.0545;
float k=0.062;
float dt=1;
int i,j;
int cx,cy,x,y,X,Y;

void setup(){
  size(200,200);
  colorMode(HSB);
  field=new float[width][height][2];
  next_field=new float[width][height][2];

  for(i=0;i<width;i++){
    for(j=0;j<height;j++){
      field[i][j][0]=1;
      field[i][j][1]=0;
      next_field[i][j][0]=1;
      next_field[i][j][1]=0;
    }
  }

  for(i=floor(width/2)-5;i<floor(height/2)+5;i++){
    for(j=floor(width/2)-5;j<floor(height/2)+5;j++){
      field[i][j][1]=1;
    }
  }
}


void draw(){
  calculate();
  swap();

  loadPixels();
  for(i=1;i<width-1;i++){
    for(j=1;j<height-1;j++){
      float col=((next_field[i][j][0]-next_field[i][j][1])) * 255;
      int pos = i + j  *  width;
      //pixels[pos] = color(col);
      pixels[pos] = color(col,255,255);
    }
  }
  updatePixels();
}

void calculate(){
  for(i=1;i<width-1;i++){
    for(j=1;j<height-1;j++){
      float A=field[i][j][0];
      float B=field[i][j][1];
      next_field[i][j][0]=A+(Da * Laplacian_A(i,j)-A * B * B+f * (1-A)) * dt;
      next_field[i][j][1]=B+(Db * Laplacian_B(i,j)+A * B * B-(k+f) * B) * dt;

      next_field[i][j][0]=constrain(next_field[i][j][0],0,1); //値を0～1に制限
      next_field[i][j][1]=constrain(next_field[i][j][1],0,1);
    }
  }
}

void swap(){
  float[][][] temp = field;
  field= next_field;
  next_field= temp;
}

float Laplacian_A(int i,int j){
  float sumA=0;
  sumA+=field[i][j][0] * (-1);
  sumA+=field[i-1][j][0] * 0.2;
  sumA+=field[i+1][j][0] * 0.2;
  sumA+=field[i][j-1][0] * 0.2;
  sumA+=field[i][j+1][0] * 0.2;
  sumA+=field[i-1][j-1][0] * 0.05;
  sumA+=field[i-1][j+1][0] * 0.05;
  sumA+=field[i+1][j-1][0] * 0.05;
  sumA+=field[i+1][j+1][0] * 0.05;
  return sumA;
}

float Laplacian_B(int i,int j){
  float sumB=0;
  sumB+=field[i][j][1] * (-1);
  sumB+=field[i-1][j][1] * 0.2;
  sumB+=field[i+1][j][1] * 0.2;
  sumB+=field[i][j-1][1] * 0.2;
  sumB+=field[i][j+1][1] * 0.2;
  sumB+=field[i-1][j-1][1] * 0.05;
  sumB+=field[i-1][j+1][1] * 0.05;
  sumB+=field[i+1][j-1][1] * 0.05;
  sumB+=field[i+1][j+1][1] * 0.05;
  return sumB;
}

void mouseClicked(){
  x=int(mouseX);
  y=int(mouseY);
  for(i=x-5;i<x+5;i++){
    for(j=y-5;j<y+5;j++){
      field[i][j][1]=1;
      next_field[i][j][1]=1;
    }
  }
}

void mousePressed(){
  cx=mouseX;
  cy=mouseY;
  loadPixels();
}

void mouseDragged(){
  if (mouseButton == RIGHT){
    updatePixels();
    X=mouseX;
    Y=mouseY;
     for(i=cx;i<X;i++){
      for(j=cy;j<Y;j++){
        field[i][j][1]=0;
        next_field[i][j][1]=0;
      }
    }
  }else if(mouseButton==LEFT){
    updatePixels();
    X=mouseX;
    Y=mouseY;
     for(i=cx;i<X;i++){
      for(j=cy;j<Y;j++){
        field[i][j][1]=1;
        next_field[i][j][1]=1;
      }
    }
  }
}
</script>
<canvas></canvas>
</div>

## 実装

```processing
//Reaction Diffusion Simulation

float[][][] field;
float[][][] next_field;

float Da=1.0;
float Db=0.5;
float f=0.0545;
float k=0.062;
float dt=1;
int i,j;
int cx,cy,x,y,X,Y;

void setup(){
  size(400,400);
  colorMode(HSB);
  field=new float[width][height][2];
  next_field=new float[width][height][2];

  for(i=0;i<width;i++){
    for(j=0;j<height;j++){
      field[i][j][0]=1;
      field[i][j][1]=0;
      next_field[i][j][0]=1;
      next_field[i][j][1]=0;
    }
  }

  for(i=floor(width/2)-5;i<floor(height/2)+5;i++){
    for(j=floor(width/2)-5;j<floor(height/2)+5;j++){
      field[i][j][1]=1;
    }
  }
}


void draw(){
  calculate();
  swap();

  loadPixels();
  for(i=1;i<width-1;i++){
    for(j=1;j<height-1;j++){
      float col=((next_field[i][j][0]-next_field[i][j][1])) * 255;
      int pos = i + j  *  width;
      //pixels[pos] = color(col);
      pixels[pos] = color(col,255,255);
    }
  }
  updatePixels();
}

void calculate(){
  for(i=1;i<width-1;i++){
    for(j=1;j<height-1;j++){
      float A=field[i][j][0];
      float B=field[i][j][1];
      next_field[i][j][0]=A+(Da * Laplacian_A(i,j)-A * B * B+f * (1-A)) * dt;
      next_field[i][j][1]=B+(Db * Laplacian_B(i,j)+A * B * B-(k+f) * B) * dt;

      next_field[i][j][0]=constrain(next_field[i][j][0],0,1); //値を0～1に制限
      next_field[i][j][1]=constrain(next_field[i][j][1],0,1);
    }
  }
}

void swap(){
  float[][][] temp = field;
  field= next_field;
  next_field= temp;
}

float Laplacian_A(int i,int j){
  float sumA=0;
  sumA+=field[i][j][0] * (-1);
  sumA+=field[i-1][j][0] * 0.2;
  sumA+=field[i+1][j][0] * 0.2;
  sumA+=field[i][j-1][0] * 0.2;
  sumA+=field[i][j+1][0] * 0.2;
  sumA+=field[i-1][j-1][0] * 0.05;
  sumA+=field[i-1][j+1][0] * 0.05;
  sumA+=field[i+1][j-1][0] * 0.05;
  sumA+=field[i+1][j+1][0] * 0.05;
  return sumA;
}

float Laplacian_B(int i,int j){
  float sumB=0;
  sumB+=field[i][j][1] * (-1);
  sumB+=field[i-1][j][1] * 0.2;
  sumB+=field[i+1][j][1] * 0.2;
  sumB+=field[i][j-1][1] * 0.2;
  sumB+=field[i][j+1][1] * 0.2;
  sumB+=field[i-1][j-1][1] * 0.05;
  sumB+=field[i-1][j+1][1] * 0.05;
  sumB+=field[i+1][j-1][1] * 0.05;
  sumB+=field[i+1][j+1][1] * 0.05;
  return sumB;
}

void mouseClicked(){
  x=int(mouseX);
  y=int(mouseY);
  for(i=x-5;i<x+5;i++){
    for(j=y-5;j<y+5;j++){
      field[i][j][1]=1;
      next_field[i][j][1]=1;
    }
  }
}

void mousePressed(){
  cx=mouseX;
  cy=mouseY;
  loadPixels();
}

void mouseDragged(){
  if (mouseButton == RIGHT){
    updatePixels();
    X=mouseX;
    Y=mouseY;
     for(i=cx;i<X;i++){
      for(j=cy;j<Y;j++){
        field[i][j][1]=0;
        next_field[i][j][1]=0;
      }
    }
  }else if(mouseButton==LEFT){
    updatePixels();
    X=mouseX;
    Y=mouseY;
     for(i=cx;i<X;i++){
      for(j=cy;j<Y;j++){
        field[i][j][1]=1;
        next_field[i][j][1]=1;
      }
    }
  }
}
```
