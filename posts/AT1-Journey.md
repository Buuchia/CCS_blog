---
title: AT1 Journey
published_at: 2024-03-13
snippet: My documentation of responding to Rafael Rozendaal's return reverse website
disable_html_sanitization: true
---

## Result
<iframe id="hypnotic_flower" src="https://editor.p5js.org/Buuchia/full/j90PdpOUjC"></iframe>

<script type="module">
 const iframe = document.getElementById(`hypnotic_flower`)
 iframe.width = iframe.parentNode.scrollWidth
 iframe.height = iframe.parentNode.scrollWidth + 42
</script>

Note: Since the flowers are generated randomly, sometimes the flowers' upper layers look more pale than the lower ones, this is a flaw that I will address in more detail in the **Problem** section. I am sorry, but please refresh the page until you find a flower you may want to explore.

## AT1 Journey
I chose to respond to Rafael Rozendaal's [*return reverse*](https://www.returnreverse.com/) website.

The circles in the work slowly expand from the center of the website, as if they invite viewers to come closer towards them like being in a tunnel, having the quality of intimacy that belongs to the cute aesthetic category. Although the work makes me feel that I am in a meditative trance at first, the longer I look at it, the more drained I feel. It demands my attention, triggering me to anticipate the next circle or next color coming out from its origin, so it's similar to what Ngai mentioned about the influence of cute on consumption. Additionally this work gradually becomes more difficult to consume, almost resisting to let viewer absorb at their own pace, so it also has that kind of avant-garde tactic associated with Ngai's cute aesthetic.

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

So I will communicate with Rafael Rozendaa's *return universe* through the motion of a hypnotizing flower's pattern.

![Sunflower's disk floret](/hw_w3/sunflower.jpg)
[Image of a sunflower](https://www.freepik.com/free-photo/closeup-shot-beautiful-sunflower-with-bee-it_14256372.htm#fromView=search&page=1&position=1&uuid=4e1b5ad3-c2cf-4601-81b6-7d9d362faa03)

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
1. When I tried to make one flower rotates, instead of rotating in a 2-dimensional perspective, my flower rotates at a skew angle. Turned out I forgot to add the variable `rotate` in the equation of the y location.

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

2. In the console, there are 5 flowers in the array but because they all have the same radius, it looks like there is only 1 flower on the canvas.

![5 flowers stack on top each other](/a1_process/flowers-5-into-1.png)

To solve this, I modified the radius parameter so that each new flower created will be smaller than the previous one.

![flowers with different radius](/a1_process/flowers-smaller-radius.png)

3. When I apply the `blendMode(DIFFERENCE)`, the flower has a strobe light effect.

![strobing flower](/a1_process/flowers-strobe.png)

To solve this, I had to add `push()` before the blendMode(DIFFERENCE) and `pop()` after the endShape(CLOSE).

4. Since I want each flower to have different color, I tried fill with random but the methods were wrong.

First, this one flashes colors too quick and too chaotic, unlike my purpose of creating meditative feel.

![flowers fill - lerp is too fast](/a1_process/fill-lerp-toofast.png)

Secondly, I thought maybe assigning value to the flowers' fill in `setup()` function will avoid the chaotic hue change. It did give each flower a different hue and looked pretty cute actually, but it stopped the flowers' rotation too, so this failed as well.

![flowers fill - setup no rotation](/a1_process/flower-fill-color-setup-no-rot.png)

5. Testing `scale()` function

Since I want to let user interact with my flower, when mouse is pressed, the scale of the flowers is multiply by the number inside the bracket of the function. I played around with it and produced some weird results.

![scale - mouseX moves little](/a1_process/scale-mouseX-little.png)
This happened when the scale is affected by mouseX when the position is not too far away on the left side of canvas. And the shapes' edges are jaggy, too.

![scale - mouseX moves too much](/a1_process/scale-mouseX-much.png)
When mouseX is too far away, it is a complete chaos.

![scale - mouseY](/a1_process/scale-mouseY.png)
Weirder thing when I added in mouseY just for fun.

6. Trying to make radial gradient for background

I tried creating linear gradient for background with this tutorial and it worked but I noticed it was not the look I was looking for and it overpowers the flowers in centre, so I wanted to make the radial gradient background like in the original website.

I think I watched the wrong tutorial about radial gradient, first one created conic gradient actually and my flowers were blocked completely, but it looked pretty cool on its own.

![conic gradient ellipse blocks flowers](/a1_process/error-3-conic-gradient-lose-flower.png)

After that, I reckon the background should be simple, just random hue changes slowly each frame, since the main focus is the flowers, which has a lot of hues already.

5. Pale flowers' fill colours and disastrous solutions

Since I want the layers of flowers to have some transparency look, I use the `lerpColor()` and `random()` functions in the Flowers class. 

However, although I guess it's because of the lerpColor() that the top flowers' fill often look more muddy than the flowers below it, causing loss of details.
These are some examples when they are generated randomly:

![muddy flower 1](/a1_process/bad-1.png)

![muddy flower 2](/a1_process/bad-4.png)

Sometimes the random results are nicer with clearer details of the flowers, such as these following results:

![good flower 1](/a1_process/good-1.png)

![good flower 2](/a1_process/good-2.png)

I am very sad about this and I tried to tweaks flowers' properties, which I think improved the paleness little bit. But I would like to know how I can address this issue properly please, since whatever I tweak in random() and lerpColor() did not solve the problem.

I guess that I should use the reverse method, but since I have not understood it well from the *falling falling* example, the code produced the following strange results.

![wrong reverse 1](/a1_process/error-1.png)

![wrong reverse 2](/a1_process/error-2.png)


