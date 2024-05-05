---
title: Week 8 Homework - AT2 Documentation
published_at: 2024-05-04
snippet: Assignment 2's Creative Process
disable_html_sanitization: true
---
Link to my AT2 Net Art is [here](https://buuchimachs-a2-project-68.deno.dev/).
1. Artists / Works inspiration
- Glitche aesthetic, hacker culture
- Rosa Menkman's blog post
- Sabato Visconti's Newer Cachemash Works - trails
- Pointer interaction

2. Zany techniques
- Matrix rain 
- Rotation of canvas
- Layers of canvas - using z-index
- Brush leaving trails

What I'm thinking to create is a net art with layers of 
- matrix rain in front
- glitched 'zany' word
- user interaction, draw trail of the zany word behind the rain
- Object oriented Programming - wrap functions and variables in objects

## PROCESS
Following the [Matrix Rain](https://www.youtube.com/watch?v=f5ZswIE_SgY&t=229s) tutorial by Franks laboratory and using the concept of Object-oriented Programming, I achieved a linear gradient matrix rain.

![linear gradient matrix rain](/a2_matrix_rain/linear_gradient_matrix_rain.png).

```html

//The Matrix Rain code is heavily inspired by Franks laboratory

//setting up the style for the body of document
document.body.style.margin   = 0
document.body.style.overflow = `hidden`
document.body.style.backgroundColor = 'hsl(' + Math.random() * 360 + ', 100%, 30%)'

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

## TESTS

1. Keeping the original fill style and the rest of the code and adding rotation of the canvas like in the code below:

```html
ctx.rotate(Math.PI / 4)
ctx.fillStyle = 'rgba(0, 0, 0, 0.05)'
```
the characters also rotate when falling down, but I don't like the edges of the rotated canvas. It takes them some time to become darker and darker. 

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
so the background changes colour randomly, which looks chaotic but it decreases the complexity of the matrix rain. Though the characters are still falling, and the fading of the character strings is harder to see. The result is a bit plain so it doesn't work.

![random background colour](/a2_matrix_rain/random_bg_color.png)

## TUTORIALS
[Matrix Rain Experiments in JavaScript (tutorial)](https://www.youtube.com/watch?v=f5ZswIE_SgY&list=LL)
[Text and Images on HTML Canvas](https://www.youtube.com/watch?v=u923wu6Oa2A)
[Synthesis, in Web Audio API](https://blog.science.family/240320_web_audio_api_synths)