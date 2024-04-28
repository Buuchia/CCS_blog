---
title: Week 4 Homework - Fractals
published_at: 2024-03-27
snippet: Process of my homework for week 4
disable_html_sanitization: true
---

## Process

Link to my fractals example is [here](https://buuchimach-wk4-hw-fractals.deno.dev/).

I started with a basic fractal pattern then added shadow to it. I did not use a css stylesheet like the tutorial but combined the style in the javasacript file. 

Besides, I defined a custom function to convert radians to degrees, using the conversion reversed from this source code from [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/tan).

```html
function getTanDeg(deg) {
  const rad = (deg * Math.PI) / 180;
  return Math.tan(rad);
}
```

Lastly, I also let the background get a random hue in each reload, instead of just the fractal in foreground.

![basic fractal](/hw_w4/fractal_basic.png)

![shadow fractal](/hw_w4/fractal_shadow.png)

The code is heavily inspired by [Franks Laboratory's tutorial](https://www.youtube.com/watch?v=dQKYao-daYw&list=PLYElE_rzEw_tN_lGjsx8uK85k-xZc4yrl).

```html
document.body.style.margin   = 0
document.body.style.overflow = `hidden`
document.body.style.backgroundColor = 'hsl(' + Math.random() * 360 + ', 100%, 30%)'

const cnv = this.document.getElementById('canvas1')

cnv.width = window.innerWidth
cnv.height = window.innerHeight

//creating a canvas element
const ctx = cnv.getContext('2d');

//define function to convert radians to degrees
radsToDegrees = rad => {
   const deg = (rad * 180) / Math.PI
   return deg
}

//setting line
ctx.lineWidth = 10;
ctx.lineCap = 'round'

// setting shadow and blur effect 
ctx.shadowColor = 'rgba(0,0,0,0.6)'
ctx.shadowOffsetX = 10
ctx.shadowOffsetY = 5
ctx.shadowBlur = 10

//effect settings
//size of main branch 
//ternary operator - condition ? run this if true : run this if false
let size = cnv.width < cnv.height ? cnv.width * 0.3 : cnv.height * 0.3

//number of branches distributed from centre point
sides = 5

//determines depth of the fractal
//how many branches split into smaller segments
//before fractal shape is finished
let maxLevel = 3

//determines the scale of the smaller branches
let scale = 0.5 //so next branch is half size of its parent branch

//the angle between the child branche and the parent branch
let spread = radsToDegrees(Math.PI / 4) // 45 degrees

//number of children branch growing from parent branch
let branches = 2

//randomize hue of fractal tree on each reload
let color = 'hsl(' + Math.random() * 360 + ', 100%, 50%)'

//define function to draw branches
drawBranch = level => {

   if (level > maxLevel) return

   ctx.beginPath()

   //starting coordinates of the line
   ctx.moveTo(0, 0)

   //ending coordinates of the line
   ctx.lineTo(size, 0);
   ctx.stroke()
   
   //for loop to make 2 more segments on each branch below
   for (let i = 0; i < branches; i++){
      //to make each branch to split into 2 small segments
   //first segment
   //add save and restore here so that the following transform 
   //only affect the drawBranch(level+1)
   ctx.save()
   //translates rotation's centre point along the branch
   //place next segment from x position of its parent branch
   //size - (size/branch) * 1 so that the child branch grows
   //from the end point of of its parent branch 
   ctx.translate(size - (size / branches) * i , 0) 
   ctx.rotate(spread)
   ctx.scale(scale, scale)
   drawBranch(level + 1)
   ctx.restore();

   //second segment
   ctx.save()
   ctx.translate(size - (size / branches) * i, 0) 

   //negative so that the segment is on the other side of parent branch
   ctx.rotate(-spread)
   ctx.scale(scale, scale)
   drawBranch(level + 1)
   ctx.restore()

   }

}

//to make circular fractal shape like snowflakes
drawFractal = () => {

   ctx.save()
   ctx.strokeStyle = color
   ctx.translate(cnv.width / 2, cnv.height / 2)

   for (let i = 0; i < sides; i++){

      ctx.rotate((Math.PI * 2) / sides)
      
      //call the function
      drawBranch(0)

   }

   ctx.restore()

   //draw the next frame
   requestAnimationFrame(drawFractal)
}

//call the function
drawFractal()
```

**Problems**

I haven't figured out the way to make the fractal move yet, and it seems to me that the shadow is drawn over and over, causing it to be darker than expected.

Also, more branches creating more complicated pattern, yet the browser takes longer time to load.

## Chaotic Potentiality for A2

I tried adjusting some values to see how far this fractal can be more chaotic. Through trials and errors, this may be used to increase chaos.

***Examples of what I think may work:**

1. Example 1

```html

//starting coordinates of the line
ctx.moveTo(0, size)

//ending coordinates of the line
ctx.lineTo(0, size - cnv.height);
ctx.stroke()

```

[chaotic fractal 1](/hw_w4/fractal_chaotic.png)

2. Example 2

[chaotic fractal 2](/hw_w4/fractal_chaotic_2.png)

```html

//ending coordinates of the line
ctx.lineTo(-size, 0);

```

3. Example 3

[chaotic fractal 3](/hw_w4/fractal_chaotic_3.png)

```html

//ending coordinates of the linefractal
ctx.lineTo(radsToDegrees(Math.PI / 4), 0);

```

4. Example 4: somehow I like that the shadow moves more visibly here

[chaotic fractal 4](/hw_w4/fractal_chaotic_4.png)

```html

//ending coordinates of the line
ctx.lineTo(size, cnv.height);

//rotate the canvas
ctx.rotate(radsToDegrees(Math.PI / 2), radsToDegrees(Math.PI / 2))

```

5. Example 5: more branches takes longer time to load, but the shadow becoming darker here kinda works.

[chaotic fractal 5](/hw_w4/fractal_chaotic_5.png)

```html

//number of children branch growing from parent branch
let branches = 7

//ending coordinates of the line
ctx.lineTo(branches, 0);

```



