---
title: Week 8 Homework - AT2 Documentation
published_at: 2024-05-04
snippet: Assignment 2's Creative Process
disable_html_sanitization: true
---
**Link to my AT2 Net Art is [here](https://buuchimachs-a2-project-68.deno.dev/).**

1. My submission is inspired by [Rosa Menkman's blog](https://rosa-menkman.blogspot.com/) and [Sabato Visconti's Newer Cachmash Works](https://www.sabatobox.com/newer-cachemash-works-2019).
Both artists' works belonged to the glitch aesthetic.

In Rosa Menkman's work, I am fascinated with the animation running in the background, which reminds me of the matrix rain of hackers. I also like that she let the layers of the canvas rotate when the mouse interacts with it, an element of surprise. 

In Sabato Visconti's works, I like that each work has a specific glitched word, which redraws itself and leaving a trail.

2. To achieve the zany/chaotic aesthetic:

- I created a falling character rain in the background using the concept of Object-oriented Programming which wraps functions and variables in objects, and recursion to animate the rain every frame.

- In the foreground, the zany word is glitching because the recursive function redraws it at high framerate.

- To add more chaos into my composition, I incorporated a simple synthesiser to the canvas, which is triggered when the user's mouse clicks on it.

- Trials and errors are also a big component of the glitch aesthetic, so I also embrace some accidental discoveries and trying to see where they could fit into my zany/chaotic composition.

## PROCESS
Following the [Matrix Rain](https://www.youtube.com/watch?v=f5ZswIE_SgY&t=229s) tutorial by Franks laboratory and using the concept of Object-oriented Programming, I achieved a linear gradient matrix rain.

![linear gradient matrix rain](/a2_matrix_rain/linear_gradient_matrix_rain.png).

```html

//The Matrix Rain code is heavily inspired by Franks laboratory

//setting up the style for the body of document
document.body.style.margin   = 0
document.body.style.overflow = `hidden`
document.body.style.backgroundColor = 'hsl(' + Math.random() * 360 + ', 100%, 30%)'
document.body.style.mixBlendMode = 'screen'

//creating a canvas element
const cnv = document.getElementById (`cnv_element`)

//getting a good size of canvas
cnv.width = window.innerWidth
cnv.height = window.innerHeight

//getting canvas context
const ctx = cnv.getContext (`2d`)

//direction of the linear gradient
//createLinearGradient (startX, startY, endX, endY) syntax
let gradient = ctx.createLinearGradient(0, 0, cnv.width, cnv.height)

//addColorStop(offset, color), offset is the value between 0 - 1
gradient.addColorStop(0, 'red') //at 0%
gradient.addColorStop(0.2, 'cyan')
gradient.addColorStop(0.4, 'yellow')
gradient.addColorStop(0.6, 'magenta')
gradient.addColorStop(0.8, 'tomato')
gradient.addColorStop(1, 'green') //at 100%

//class Symbol creates and draws individual symbol objects
//that make up the rain effect
class Symbol {
    constructor(x, y, fontSize, canvasHeight) {
        this.chars = 'ポヴッンWELCOMETOMYZANYWORLDグズブヅプアァカサタナボ'
        this.x = x
        this.y = y
        this.fontSize = fontSize;
        this.text = ''
        this.canvasHeight = canvasHeight
    }

    //define a function to randomize current character 
    //and draw it to the canvas at a specific location
    //every time the method is called, it will choose a random symbol
    draw(context) {

        //charAt() can be called on string data type,
        //which takes a single 'index' argument and returns a new string 
        //containing only that one character 
        //located at that specific offset of the string
        //in charAt bracket is a random rounded number between 0 - this.chars.length
        //so this.text will have a random character from the this.chars string
        this.text = this.chars.charAt(Math.floor (Math.random() * this.chars.length))

        //to reduce the number of calls, I move this down to function animate()
        // context.fillStyle = 'green'

        //make character sit next to and below each other
        context.fillText(this.text, this.x * this.fontSize, this.y * this.fontSize)

        //when character reaches bottom of canvas height
        if (this.y * this.fontSize > this.canvasHeight && Math.random() > 0.9) {

            //reset its vertical position back to the top
            this.y = 0

        } else {
            //otherwise, increase it by 1
            //the next symbol will be drawn below depending on font size 
            //with no overlapping
            this.y += 1
        }
    }
}

//Main wrapper for entire rain effect
class Effect {
    constructor(canvasWidth, canvasHeight) {
        this.canvasWidth = canvasWidth
        this.canvasHeight = canvasHeight
        this.fontSize = 25
        this.columns = this.canvasWidth / this.fontSize
        this.symbols = [] //empty array to store symbol objects
        this.#init() //calling private method from inside constructor
        console.log (this.symbols)
    }

    //define an initialise function to fill the symbols array with symbol objects
    //using the Symbol class above
    //the number of symbols depends on the number of columns 
    //inside class Effect's constructor
    //making the init method private by starting with #
    //to ensure it is not affected by external interaction

    #init(){

        //for loop will run once for each column
        //each time it will fill that index in symbols array 
        //with an instance of Symbol class
        for (let i = 0; i < this.columns; i++){

            //the rain starts failling from top
            //so initial vertical coordinate y is set to 0
            this.symbols[i] = new Symbol (i, 0, this.fontSize, this.canvasHeight)
        }
    }

    //public resize method so the properties can be updated from outside
    resize(width, height) {
        this.canvasWidth = width
        this.canvasHeight = height
        this.columns = this.canvasWidth / this.fontSize
        this.symbols = []
        this.#init()
    }
}

//declaring effect variable
const effect = new Effect (cnv.width, cnv.height)

//assign 0 to lastTime variable
//this variable stores timestamp from the previous frame
let lastTime = 0

//assigning number of frames per second to variable fps
const fps = 30

//the amount of millisecond we wait until we trigger and draw the next frame
const nextFrame = 1000/fps

//this variable accumulates delta time
//when it reaches threshold defined in next frame
//it will animate next frame, reset itself to 0
//and start counting again
let timer = 0

//define a custom function to draw the rain effect
//60 times per second
const animate = (timeStamp) => {

    //delta time is the difference in milliseconds between 
    //the current animation frame and previous animation frame
    //this variable lets us know how many miliseconds it takes 
    //for our computer to serve next frame of animation
    const deltaTime = timeStamp - lastTime

    //assign new timeStamp to lastTime so it can be used for next loop
    lastTime = timeStamp

    //use time stamp and delta time to control framerate
    if (timer > nextFrame) {

        //making semi-transparent rectangles to make old symbols fade away
        ctx.fillStyle = 'rgba(0, 0, 0, 0.05)'
        
        //characters have different horizontal alignment
        //so align center to make all of them align evenly
        ctx.textAlign = 'center'
        
        //drawing a rectangle every frame
        ctx.fillRect(0, 0, cnv.width, cnv.height)

        //declaring fill style once for all the symbols in the array per frame
        //the gradient style applies to the canvas background, not the characters 
        //so characters take different colors depending on their position on canvas
        ctx.fillStyle = gradient 

        //font property specifies the current text style
        //monospace fonts have characters 
        //that occupy the same amount of horizontal space
        ctx.font = effect.fontSize + 'px monospace'

        //draw symbol to the canvas
        effect.symbols.forEach(symbol => symbol.draw(ctx))

        //restart timer to 0 so it can start countdown to the next frame again
        timer = 0

    } else {

        //otherwise increase timer by delta time
        //we don't animate anything and just wait until timer is high enough
        timer += deltaTime
        
    }

    //call the next animation frame
    //this function automatically passes a timestamp argument to the function it calls
    //so function animate() has access to the auto-generated timeStamp argument above
    //but we only have timestamp argument for second loop
    requestAnimationFrame(animate)
}

//first loop of animation is called here
//so there is no auto-generated timestamp
//so we need to pass it a value, such as 0
animate(0)

//define function to make the effects responsive to the canvas dimension
//when user resizes the window viewport
window.onresize = () => {
   cnv.width = window.innerWidth
   cnv.height = window.innerHeight   
   effect.resize(cnv.width, cnv.height)
   gradient = ctx.createLinearGradient(0, 0, cnv.width, cnv.height)
   gradient.addColorStop(0, 'red') //at 0%
   gradient.addColorStop(0.2, 'cyan')
   gradient.addColorStop(0.4, 'yellow')
   gradient.addColorStop(0.6, 'magenta')
   gradient.addColorStop(0.8, 'tomato')
   gradient.addColorStop(1, 'green') //at 100%
}

```

Next, I try creating a text on top of this matrix background. Ideally, I want to add glitch effect to this text to intensify the chaos.

What I have got so far is a custom function to draw the text and the text changes colour randomly every frame in an unpleasant speed to the viewer's eyes, then I call this function under line `timer = 0` inside the `animate()` function.

```html 

//define a function to draw the Zany text
const drawText = () => {
    ctx.font = 'bold 500px Roboto' //font name and size
    ctx.textBaseline = "middle"
    ctx.textAlign = "center"
    ctx.fillStyle = 'hsl(' + Math.random() * 360 + ', 100%, 50%)'
    ctx.fillText("ZANY", cnv.width /2 , cnv.height /2)
}

```

Result: 
![random zany colour](/a2_matrix_rain/zany_random_colour.png)

I also change the font in `animate()` function from 'px monospace' to the code below:

```html

//font property specifies the current text style
//add wingdings fonts to have more interesting symbols
ctx.font = effect.fontSize + 'px Wingdings'

```

Then I try adding a synthesiser to the canvas with code from the lecture and some adjustments to make the work more chaotic. Even though I see this warning in the console log, the audio doesn't start until the mouse clicks on the canvas and the audio still works, so that is a bit strange.

![audio warning](/a2_matrix_rain/audio_warning.png)

I want to the sound to remind of old video games, so I change the oscilliator type and the value of midi note, referring to [this midi sheet.](https://newt.phys.unsw.edu.au/jw/notes.html)

```html

// make it a square wave this time
osc.type            = 'square'

// set the value using the equation 
// for midi note to Hz
osc.frequency.value = 261.63 * 2 ** ((note - 40) / 20)

```

Another problem is that I struggle with incorporating the glitched pixels effect from the lecture's code to the word 'ZANY', because it is not an image that the function can get the data from. 

I also want to retain the rotation of canvas like in Rosa Menkman's blog post, so I try doing that in the `drawText()` function.

```html

//define a function to draw the Zany text
const drawText = () => {

    ctx.rotate((45 * Math.PI) / 180) //rotate the canvas by 45 degrees
    // ctx.clearRect(0, 0, cnv.width / 2, cnv.height / 3)

    ctx.font = 'bold 500px Roboto' //font name and size
    ctx.textBaseline = "middle"
    ctx.textAlign = "center"
    ctx.fillStyle = 'hsl(' + Math.random() * 360 + ', 100%, 50%)'
    ctx.fillText("ZANY", cnv.width /2 , cnv.height /2)
}
```

Between enabling and disabling line `ctx.clearRect(0, 0, cnv.width / 2, cnv.height / 3)`, I prefer the result without it because the rectangles are distracting yet does nothing much. 

Without `clearRect()`:
![text function disables clearRect()](/a2_matrix_rain/text_without_clearRect.png)

With `clearRect()`:
![text function enables clearRect()](/a2_matrix_rain/text_with_clearRect.png)

Lastly, I add blend mode to the body of document:

```html

document.body.style.mixBlendMode = 'screen'

```

I find that with this blend mode, I can see the random background colour more clearly while the fading of the previous characters are still kept, so the background won't turn black gradually all the time, which adds variety every time the page reloads. I was considering between `color-dodge` mode and `sscreen` mode, but I go with `screen` for now because it sacrifies the gradient effect less than the other.

![screen blend mode](/a2_matrix_rain/screen_blendmode.png)

## TESTS

1. Keeping the original fill style and the rest of the code and adding rotation of the canvas like in the code below:

```html
ctx.rotate(Math.PI / 4)
ctx.fillStyle = 'rgba(0, 0, 0, 0.05)'
```
The characters rotate when falling down. It takes the edges of the rotated canvas a while to become darker and darker. I am not sure if I like this or not, but it impacts the original rain in a new and interesting way. Instead of falling neatly in each columns, which is predictable and maybe boring in a long run, the characters appears more randomly.

Before:
![before](/a2_matrix_rain/black_rec_rotate_pi4_before.png)

After:
![after](/a2_matrix_rain/black_rec_rotate_pi4_after.png)

2. I change the fill style of context from this:

```html
ctx.fillStyle = 'rgba(0, 0, 0, 0.05)'
```
to this:

```html
ctx.fillStyle = 'hsl(' + Math.random() * 360 + ', 100%, 30%)'
```
so the background changes colour randomly, which looks chaotic but it decreases the complexity of the matrix rain. Though the characters are still falling, and the fading of the character strings is harder to see. The result is a bit plain so I don't think this works well.

![random background colour](/a2_matrix_rain/random_bg_color.png)

3. In this test, I try splitting the canvas into smaller portions, playing with `ctx.fillRect` and `ctx.clearRect`, but I don't think I would go with this composition because they are so still.

![split screen](/a2_matrix_rain/split_screen.png)

## REFLECTION

I am not 100% satisfied with the current result, but I try my best in responding to the artworks. Combining different ideas in one javascript file is really difficult.

Besides, even though I suspect that the layers in Rosa Menkman's blog has something to do with z-index, but since I only use one canvas, the functionality of z-index is still ambiguous. 

And I haven't figured out how to do the glitched brush trail in Visconti's work so my glitch version is unfortunately compromised for now.

About effective complexity, the non-randomness element lies in the falling symbols all coming from the same character set, yet randomizing each character in each index of the symbols array achieves the non-redundancy quality. The synthesiser also boosts this structure, because all the notes have same oscilliator (coherence), but with different math resulting in different values of frequency and different positions on the canvas (spiciness), they make different notes. Thus, although the composition is chaotic, it still contains some effective complexity.

## TUTORIALS
[Matrix Rain Experiments in JavaScript (tutorial)](https://www.youtube.com/watch?v=f5ZswIE_SgY&list=LL)

[Text and Images on HTML Canvas](https://www.youtube.com/watch?v=u923wu6Oa2A)

[Synthesis, in Web Audio API](https://blog.science.family/240320_web_audio_api_synths)