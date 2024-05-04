---
title: Week 7 Homework - Combining Ideas
published_at: 2024-04-24
snippet: Combining c2js and ascii cam
disable_html_sanitization: true
---

<script src="/scripts/c2.min.js"></script>

<canvas id="c2"></canvas>

The ascii cam code is taken from the class' lecture and the c2js code example is from [Limited Voranoi 4](https://c2js.org/examples.html?name=LimitedVoronoi4.) created by Ren Yuan.

<div id="ascii_div"></div>

<script>
    const renderer = new c2.Renderer(document.getElementById('c2'));

    renderer.context.willReadFrequently = true 

    resize ()

    renderer.background('turquoise');
    let random = new c2.Random();


    class Agent extends c2.Cell{
    constructor() {
        let x = random.next(renderer.width);
        let y = random.next(renderer.height);
        let r = random.next(renderer.width / 40, renderer.width / 15);
        super(x, y, r);

        this.vx = random.next(-2, 2);
        this.vy = random.next(-2, 2);
        this.color = c2.Color.hsl(random.next(0, 30), random.next(30, 60), random.next(20, 100));
    }

    update(){
        this.p.x += this.vx;
        this.p.y += this.vy;

        if (this.p.x < 0) {
            this.p.x = 0;
            this.vx *= -1;
        } else if (this.p.x > renderer.width) {
            this.p.x = renderer.width;
            this.vx *= -1;
        }
        if (this.p.y < 0) {
            this.p.y = 0;
            this.vy *= -1;
        } else if (this.p.y > renderer.height) {
            this.p.y = renderer.height;
            this.vy *= -1;
        }
    }

    display(){
        if (this.state != 2) {
            renderer.stroke(c2.Color.rgb(0, .2));
            renderer.lineWidth(1);
            renderer.fill(this.color);
            renderer.polygon(this.polygon(4));

            renderer.stroke('#333333');
            renderer.lineWidth(5);
            renderer.point(this.p.x, this.p.y);
        }
    }
 }
 
let agents = new Array(15);
    for (let i = 0; i < agents.length; i++) {
    agents[i] = new Agent();
    }

const chars = "¶Ñ@%&∆∑∫#Wß¥$£√?!†§ºªµ¢çø∂æåπ*™≤≥≈∞~,.…_¬“‘˚`˙"

const div = document.getElementById (`ascii_div`)
   div.style.fontFamily = `monospace`
   div.style.textAlign = `center`

renderer.draw(() => {

    let voronoi = new c2.LimitedVoronoi();
    voronoi.compute(agents);

    renderer.stroke('#333333') 
    for (let i = 0; i < agents.length; i++) {
        agents[i].display();
        agents[i].update();
    }

const w = renderer.canvas.width
const h = renderer.canvas.height
const pixels = renderer.context.getImageData (0, 0, w, h).data

let ascii_img = ``

for (let y = 0; y < h; y += 22) {
    for (let x = 0; x < w; x += 10) {
        const i = (y * w + x) * 4
        const r = pixels[i]
        const g = pixels[i + 1]
        const b = pixels[i + 2]
        const br = (r * g * b / 16581376) ** 0.1
        const char_i = Math.floor (br * chars.length)
        ascii_img += chars[char_i]
        }
    ascii_img += `\n`
      }

    div.innerText = ascii_img

    });

   function resize () {
      let parent = renderer.canvas.parentElement
      renderer.size (parent.clientWidth, parent.clientWidth / 16 * 9)
   }

</script>


```html

<script src="/scripts/c2.min.js"></script>

<canvas id="c2"></canvas>
<div id="ascii_div"></div>

<script>

const renderer = new c2.Renderer(document.getElementById('c2'));

    renderer.context.willReadFrequently = true 

    resize ()

    renderer.background('turquoise');
    let random = new c2.Random();


    class Agent extends c2.Cell{
    constructor() {
        let x = random.next(renderer.width);
        let y = random.next(renderer.height);
        let r = random.next(renderer.width / 40, renderer.width / 15);
        super(x, y, r);

        this.vx = random.next(-2, 2);
        this.vy = random.next(-2, 2);
        this.color = c2.Color.hsl(random.next(0, 30), random.next(30, 60), random.next(20, 100));
    }

    update(){
        this.p.x += this.vx;
        this.p.y += this.vy;

        if (this.p.x < 0) {
            this.p.x = 0;
            this.vx *= -1;
        } else if (this.p.x > renderer.width) {
            this.p.x = renderer.width;
            this.vx *= -1;
        }
        if (this.p.y < 0) {
            this.p.y = 0;
            this.vy *= -1;
        } else if (this.p.y > renderer.height) {
            this.p.y = renderer.height;
            this.vy *= -1;
        }
    }

    display(){
        if (this.state != 2) {
            renderer.stroke(c2.Color.rgb(0, .2));
            renderer.lineWidth(1);
            renderer.fill(this.color);
            renderer.polygon(this.polygon(4));

            renderer.stroke('#333333');
            renderer.lineWidth(5);
            renderer.point(this.p.x, this.p.y);
        }
    }
 }
 
let agents = new Array(15);
    for (let i = 0; i < agents.length; i++) {
    agents[i] = new Agent();
    }

const chars = "¶Ñ@%&∆∑∫#Wß¥$£√?!†§ºªµ¢çø∂æåπ*™≤≥≈∞~,.…_¬“‘˚`˙"

const div = document.getElementById (`ascii_div`)
   div.style.fontFamily = `monospace`
   div.style.textAlign = `center`

renderer.draw(() => {

    let voronoi = new c2.LimitedVoronoi();
    voronoi.compute(agents);

    renderer.stroke('#333333') 
    for (let i = 0; i < agents.length; i++) {
        agents[i].display();
        agents[i].update();
    }

const w = renderer.canvas.width
const h = renderer.canvas.height
const pixels = renderer.context.getImageData (0, 0, w, h).data

let ascii_img = ``

for (let y = 0; y < h; y += 22) {
    for (let x = 0; x < w; x += 10) {
        const i = (y * w + x) * 4
        const r = pixels[i]
        const g = pixels[i + 1]
        const b = pixels[i + 2]
        const br = (r * g * b / 16581376) ** 0.1
        const char_i = Math.floor (br * chars.length)
        ascii_img += chars[char_i]
        }
    ascii_img += `\n`
      }

    div.innerText = ascii_img

    });

   function resize () {
      let parent = renderer.canvas.parentElement
      renderer.size (parent.clientWidth, parent.clientWidth / 16 * 9)
   }

</script>

```

In the console log, I receive this error `Canvas2D: Multiple readback operations using getImageData are faster with the willReadFrequently attribute set to true. See: https://html.spec.whatwg.org/multipage/canvas.html#concept-canvas-will-read-frequently`.

To solve this I added this code below:

```html
renderer.context.willReadFrequently = true 
```

Besides, nothing was shown on the context so unlike in the lecture's ascii cam + c2js example, I added the `display()` function and call it the the code.