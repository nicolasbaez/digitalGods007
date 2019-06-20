# digitalGods007
Entramos al juego

![digitalGods](https://github.com/nicolasbaez/digitalGods007/blob/master/portada.png)

1. gameDev.htm
```html
<html>
    <script src="p5.js">
        //Descarga: https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.8.0/p5.js
    </script>
    <script src="p5.dom.js">
        //Descarga: https://raw.githubusercontent.com/lmccart/p5.js/master/lib/addons/p5.dom.js
    </script>
    <script src="digitalGods.js">
        //Nuestro juego de video
    </script>
    <body style="margin:0;">
    </body>
</html>
```
2. digitalGods.js
```javascript
//variables
var maxSpace=2;
var nEstrellas=32;
var nShips=32;
var nEdificios=128;
var minPisosEdificio=48;
var maxPisosEdificio=64;
var wEdificios=[];
var hEdificios=[];
var pEdificios=[];
var xStars=[];
var yStars=[];
var dStars=[];
var txLight=[];
var xShips=[];
var yShips=[];
var wShips=[];
var vShips=[];
function setup(){
	createCanvas(window.innerWidth,window.innerHeight);
	background(0);
	for(var i=0;i<nEstrellas;i++){
		xStars[i]=random(width);
		yStars[i]=random(height);
		dStars[i]=random(4);
	}
	for(var i=0;i<nShips;i++){
		xShips[i]=width;
		yShips[i]=random(height*0.5,height*0.75);
		wShips[i]=random(width*0.01,width*0.02);
		vShips[i]=random(width*0.01,width*0.02);
	}
	for(var i=0;i<nEdificios;i++){
		wEdificios[i]=random(width*0.04,width*0.06);
		hEdificios[i]=random(height*0.25,height*0.75);
		pEdificios[i]=random(minPisosEdificio,maxPisosEdificio);
	}
	for(var i=0;i<nEdificios;i++){
		txLight[i]=[];
		for(var j=0;j<pEdificios[i];j++){
			txLight[i][j]=random(192);
		}	
	}
}
function atardecer(){
	strokeWeight(1);
	for(var i=0;i<=height;i++){
		//rr,gg,bb
		stroke(
			map(i,0,height,map(mouseX,0,width,0,0),map(mouseX,0,width,255,255)),
			map(i,0,height,map(mouseX,0,width,153,0),map(mouseX,0,width,255,102)),
			map(i,0,height,map(mouseX,0,width,255,0),map(mouseX,0,width,255,0)),
		);
		line(0,i,width,i);
	}
}
function estrellas(){
	for(var i=0;i<nEstrellas;i++){
		fill(255,255,255,map(mouseX,0,width,0,255));
		noStroke();
		ellipse(xStars[i],yStars[i]-map(mouseX,0,width,0,16),dStars[i],dStars[i]);
	}
}
function edificios(){
	var dx=map(mouseX,0,width,0,-width*maxSpace);
	for(var i=0;i<nEdificios;i++){
		noStroke();
		fill(map(mouseX,0,width,32,0));
		rect(dx,height-hEdificios[i],wEdificios[i],hEdificios[i]);
		stroke(
			map(mouseX,0,width,255,255),
			map(mouseX,0,width,255,102),
			map(mouseX,0,width,255,0)
		);
		line(dx,height-hEdificios[i],dx+wEdificios[i],height-hEdificios[i]);
		var k=0;
		for(var j=height-hEdificios[i];j<=hEdificios[i];j+=hEdificios[i]/pEdificios[i]){
			stroke(map(mouseX,0,width,64,0));
			fill(0,255,255,map(mouseX,0,width,0,txLight[i][k]));
			rect(dx,j,wEdificios[i],hEdificios[i]/pEdificios[i]);
			k++;
		}
		dx+=width*maxSpace*0.02;
	}
}
function ships(){
	for(var i=0;i<nShips;i++){
		strokeWeight(2);
		stroke(map(mouseX,0,width,64,0));
		line(xShips[i],yShips[i],xShips[i]+wShips[i],yShips[i]);
		xShips[i]-=vShips[i];
		if(xShips[i]<0){
			xShips[i]=width;
			vShips[i]=random(width*0.01,width*0.02);
			wShips[i]=random(width*0.01,width*0.02);
			yShips[i]=random(height*0.5,height*0.75);
		}
	}
}
function draw(){
	atardecer();
	estrellas();
	edificios();
	ships();	
	strokeWeight(2);
	stroke(255);
	for(var i=0;i<nShips;i++){
		if(abs(mouseX-xShips[i])<=wShips[i]){
			if(abs(mouseY-yShips[i])<=wShips[i]){
				stroke(255,0,0);
			}
		}
	}
	line(mouseX,map(mouseY,0,height,height*0.5,height*0.75),mouseX+width*0.01,map(mouseY,0,height,height*0.5,height*0.75));
}
```
