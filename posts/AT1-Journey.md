---
title: AT1 Journey
published_at: 2024-03-13
snippet: My documentation of responding to Rafael Rozendaal's return reverse website
disable_html_sanitization: true
---

## AT1 Journey
I chose to respond to Rafael Rozendaal's [*return reverse*](https://www.returnreverse.com/) website.

The circles in the work slowly expand from the center of the website, as if they invite viewers to come closer, having the quality of intimacy that belongs to the cute aesthetic category. Although the work makes me feel that I am in a meditative trance at first, the longer I look at it, the more drained I feel. It demands my attention, triggering me to anticipate the next circle or next color coming out from its origin, so it's similar to what Ngai mentioned about the influence of cute on consumption. Additionally this work gradually becomes more difficult to consume, almost resisting to let viewer absorb at their own pace, so it also has that kind of avant-garde tactic associated with Ngai's cute aesthetic.

The artist employed effective complexity by generating iterations of simple circles with rotating hues of the colour wheel. These shapes have the same mechanic: born, expand, a new born drawn on top, and loop forever, but because each shape's color is varied and blended naturally together, the work achieves the balance between cohenrence (refering to shapes and their behaviour) and randomness (refering to the color).

### Atributes of the original work I would like to retain
- Shape drawn in the centre of the canvas.
- Meditative/Hypnotizing feel.
- Slow-motion approach of the shapes, or when they transform.
- Random color of the petals or 

### Aspects I would like to improve/change
- Change the circles to flower shape.
- Randomize color of the background in time with the flower transformation, instead of randomizing color of petals.

### The way my work in dialogue with Rafael Rozendaal's *return reverse* is in:
The title *return reverse* and the mechanical aspect the geometric shapes of the website inspire me to think about a loop of being born, disappearing, and being born again, which leads me to deep thoughts about life and how the universe works, where everything begins, ends, and a new beginning ensues. 

When one thinks of universe, it could be a starry sky or planets, but I came across a project called [Cosmic Bloom](https://outland.art/cosmic-bloom/) by Leo Villareal that also achieved effective complexity. The artist draws from the inspirations of 'organic and biological structures, stellar phenomena, and atomic patterns', layering and multiplying geometric forms, and using algorithms to create random moving patterns, which resembles flowers, hence its name. 

Using the imagery of a flower to represent the universe is not entirely new, but the fact that something so vast like the universe can be reduced to a small thing like flower is really cute and organic. I really like the idea that digital works can be natural.

So I will communicate with Rafael Rozendaa's *return universe* through the motion of a flower's disk floret pattern, perhaps like that of a [sunflower](https://www.freepik.com/free-photo/closeup-shot-beautiful-sunflower-with-bee-it_14256372.htm#fromView=search&page=1&position=1&uuid=4e1b5ad3-c2cf-4601-81b6-7d9d362faa03).

![Sunflower's disk floret](/hw_w3/sunflower.jpg)

### How JavaScript Techniques and Design Concepts I use contribute to the aesthetic of my response?
**Design Concept** 
1. I will create a hypnotic flower. 
2. The hypnotizing effect is needed to evoke the feeling of meditation then gradually turns into somewhat trippy if the viewer maintains the gaze with it after a long time. 

**Javascript Technique**
- Flower stays in the center of the canvas.
- Each flower shape has some transparency, so not solid fill.
- Hypnotizing effect may be achieved with slow motion, when the flower layers transform, when they are bigger and approaching.
- Rotating hue.
- Blend mode.
- for loop, since I need to make iterations of the flower layers.
- Make a flower object in a class.
- Some conditional statements.
- Random with map function (for background).
- Array of petals (?).
- Vertex because I will need to draw the petals.

**Tutorials**

[Patt Vira's Hyonotic Flowers](https://www.youtube.com/watch?v=o5t7PxRJSXk)

### Process

`vertex()` function draws points, but alone it doesn't show anything on screen, so we need additional functions `beginShape()` and `endShape()`.

The origin is still at the top left corner of the canvas.

![points at original origin](/a1_process/point-01.png)

So using `translate()` function to move the origin to the middle of the canvas.

```javascript
//Make 2 arrays to store the x and y location of each of the points
let x = []
let y = []
let pts = 10 //the number of the points making up our circle
let r = 100 //the radius of the circle

function setup() {
  createCanvas(innerWidth, innerHeight); //full-screen
}

function draw() {
  background(220);
  translate(width/2, height/2)
  beginShape()
  for (let i = 0; i < pts; i++){ // instead of drawing function, using for loop 
    let angle = i/pts * 360 //set up angle, evenly space the points
    x[i] = r*cos(angle) //concept of trigonometry to convert (r,theta) to (x,y)
    y[i] = r*sin(angle) 
    vertex(x[i], y[i])
  }
  endShape()
}
```

![shapes in middle](/a1_process/middle-shape-01.png)

The shape is not a circle yet because the default angle mode is in radians, so I have to set it to degrees, using `angleMode(DEGREES)`.

First and last point of the shape is not connected so I use `endShape(CLOSE)` to bridge them. 

The shape is still not fully a circle because it needs more points, so change `pts = 100` makes the edge round.

To make the flower, we don't want its radius to be 100 throughout the circle, so we add the flower's radius, declaring this variable `f.radius`, to the circle's radius `r`.

```javascript
x[i] = (r + f_radius) *cos(angle)
y[i] = (r + f_radius) *sin(angle)

```

To make the wavy pattern, this equation outputs the number between negative and positive of f_amp, and period is the number of times we get the value between negative and positive of f_amp.

```javascript

f_radius = f_amp * cos(angle * period)

```

For example: if f_amp = 20; period = 5 <br>
f_radius = 20 * cos(angle * 5) <br>
The output is 5 times we get the values between -20 and 20.

![flower shape f_amp = 20; period = 5](/a1_process/flower-amp20-p5.png)



### Problems
When I tried to make one flower rotates, instead of rotating in a 2-dimensional perspective, my flower rotates at a skew angle. Turned out I forgot to add the variable `rotate` in the equation of the y location.

![flower - skew rotation](/a1_process/flower-skew-angle.png)

Wrong code:
```javascript
x[i] = (r + f_radius) *cos(angle + rotate) 
y[i] = (r + f_radius) *sin(angle)
```

Correct code:
```javascript
x[i] = (r + f_radius) *cos(angle + rotate) 
y[i] = (r + f_radius) *sin(angle + rotate) 
```
