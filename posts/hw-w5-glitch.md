---
title: Week 5 Homework - Glitch
published_at: 2024-04-10
snippet: Process of my homework for week 5
disable_html_sanitization: true
---

# 1. Explanatory Comments 

## Glitch

<canvas id="glitch_self_portrait"></canvas>

<script type="module">

   const cnv = document.getElementById (`glitch_self_portrait`)
   cnv.width = cnv.parentNode.scrollWidth
   cnv.height = cnv.width * 9 / 16
   cnv.style.backgroundColor = `deeppink`

   const ctx = cnv.getContext (`2d`)

   let img_data

   const draw = i => ctx.drawImage (i, 0, 0, cnv.width, cnv.height)

   const img = new Image ()
   img.onload = () => {
      cnv.height = cnv.width * (img.height / img.width)
      draw (img)
      img_data = cnv.toDataURL ("image/jpeg")
      add_glitch ()
   }
   
   img.src = `/240405/pfp_glasses.jpg`

   const rand_int = max => Math.floor (Math.random () * max)

   const glitchify = (data, chunk_max, repeats) => {
      const chunk_size = rand_int (chunk_max / 4) * 4
      const i = rand_int (data.length - 24 - chunk_size) + 24
      const front = data.slice (0, i)
      const back = data.slice (i + chunk_size, data.length)
      const result = front + back
      return repeats == 0 ? result : glitchify (result, chunk_max, repeats - 1)
   }

   const glitch_arr = []

   const add_glitch = () => {
      const i = new Image ()
      i.onload = () => {
         glitch_arr.push (i)
         if (glitch_arr.length < 12) add_glitch ()
         else draw_frame ()
      }
      i.src = glitchify (img_data, 96, 6)
   }

   let is_glitching = false
   let glitch_i = 0

   const draw_frame = () => {
      if (is_glitching) draw (glitch_arr[glitch_i])
      else draw (img)

      const prob = is_glitching ? 0.05 : 0.02
      if (Math.random () < prob) {
         glitch_i = rand_int (glitch_arr.length)
         is_glitching = !is_glitching
      }

      requestAnimationFrame (draw_frame)
   }

</script>

```html

//create a canvas element with the id name "glitch_self_portrait" 
<canvas id="glitch_self_portrait"></canvas>

<script type="module">

//assign to immutable variable cnv the newly created canvas element object in the document interface
   const cnv = document.getElementById (`glitch_self_portrait`) 

//retrieves the width of the parent element of the canvas
// considering the entire width of its content
// and assigns it to the width of the canvas. 
//dynamically adjusts the width of the canvas to match the width of its parent element
//ensuring that the canvas fills its container horizontally. 
//useful for responsive designs where the canvas needs to adapt to different screen sizes or layouts.
   cnv.width = cnv.parentNode.scrollWidth

//the canvas height maintains a 9:16 aspect ratio relative to the width of the canvas.
   cnv.height = cnv.width * 9 / 16

//set background colour of the canvas to deep pink
   cnv.style.backgroundColor = `deeppink`

//calling the getContext() method on the newly created canvas object
//return a CanvasRenderingContext2D object, which we interface with to draw to the canvas.
   const ctx = cnv.getContext (`2d`)

//declare variable to store the base64 encoded image data
   let img_data

//define function draw with an i parameter, i stands for image
//whatever image is passed into i, refer to the CanvasRenderingContext2D, 
//draw the image at the top left corner
//using the canvas width and height
   const draw = i => ctx.drawImage (i, 0, 0, cnv.width, cnv.height)

//creates a new instance of the Image object, 
//which serves as a placeholder for an image and 
//allows for asynchronous loading and manipulation of images in the browser environment.
   const img = new Image ()

   //load an image from a URL specified by img.src. 
   //The onload event handler executes a function when the image has finished loading. 
   //This asynchronous loading mechanism allows performing tasks after the image has been successfully loaded.
   img.onload = () => {
   
      //resizes the canvas height to the aspect ratio of the image relative to the width of the canvas.
      cnv.height = cnv.width * (img.height / img.width)

      //call function draw to draw the image onto the canvas when image has loaded
      draw (img)

      //toDataURL() is used here to convert the content of the canvas into a data URL 
      //representing the image in JPEG format
      //the resulting data URL is stored in the variable img_data.
      img_data = cnv.toDataURL ("image/jpeg")

      //call add_glitch function
      add_glitch ()
   }

   //source of the image
   img.src = `/240405/pfp_glasses.jpg`

//define function rand_int with a max parameter to returns random variations of the largest rounded integer of data chunk size
//that are removed from the image data during the glitching process. 
//Math.floor() static method rounds down and returns the largest integer less than or equal to a given number.
//Generating random integers for the chunk size allows the glitch effect to vary in intensity and size randomly.
//random integer function is also used to control the number of repeats in the glitch effect in the glitchify function.
   const rand_int = max => Math.floor (Math.random () * max)

//define glitchify function to introduce glitches into the image data
//takes base64 encoded image data, a maximum chunk size, and the number of repeats as parameters. 
//randomly removes chunks of data from the image data to simulate glitches.
//Each time the function is called, it selects a random position within the image data to apply the glitch effect, making the glitches appear more natural and unpredictable.
   const glitchify = (data, chunk_max, repeats) => {
      //determines the size of the chunk of data to be removed for the glitch effect.
      //chunk_max is a parameter passed to the function, representing the maximum size of the chunk of data to be removed during glitching.
      //rand_int(chunk_max / 4) generates a random integer between 0 and chunk_max / 4
      //Dividing by 4 might be done to limit the maximum chunk size, ensuring the glitch effect doesn't remove too much data at once.
      //Multiplying by 4 ensures that the chunk_size is a multiple of 4, which is often necessary for data manipulation operations, particularly when dealing with binary data or image data.
      const chunk_size = rand_int (chunk_max / 4) * 4
      
      const i = rand_int (data.length - 24 - chunk_size) + 24
      //the front variable will be an array that contains extracted data from the original data array
      //the front portion starts extraction from the first index of the data array up to i, but not including i
      const front = data.slice (0, i)
      //the back portion starts where i ends plus the chunk size to the end elements of the data array
      const back = data.slice (i + chunk_size, data.length)
      const result = front + back
      //The glitchify function is recursive, and each recursive call decrements the number of repeats until it reaches zero. 
      return repeats == 0 ? result : glitchify (result, chunk_max, repeats - 1)
   }

//empty array to store glitched images
   const glitch_arr = []

//The add_glitch function is defined 
//which creates a new image object, sets its onload event, pushes it into the glitch_arr, 
//and recursively calls itself until glitch_arr has 12 glitched images. 
//Once the array is populated, it calls the draw_frame function.
   const add_glitch = () => {
      const i = new Image ()
      i.onload = () => {
         glitch_arr.push (i)
         if (glitch_arr.length < 12) add_glitch ()
         else draw_frame ()
      }
      //source of the new glitch image will use the image data, chunk_max 96, repeats 6 times
      i.src = glitchify (img_data, 96, 6)
   }

//Two variables is_glitching and glitch_i are initialized to control the glitch effect.
   let is_glitching = false
   let glitch_i = 0

//Define draw_frame function, 
//which alternates between drawing the original image and glitched images 
//based on a probability.
   const draw_frame = () => {
      //if the image is not glitching, draw the glitch images and put them into the glitch_arr array
      if (is_glitching) draw (glitch_arr[glitch_i])
       //if the image is glitching, draw the original image.
      else draw (img)

      const prob = is_glitching ? 0.05 : 0.02
      if (Math.random () < prob) {
         glitch_i = rand_int (glitch_arr.length)
         is_glitching = !is_glitching
      }
// It uses requestAnimationFrame to continuously draw frames, creating an animation effect.
      requestAnimationFrame (draw_frame)
   }

</script>
```


# 2. Glitch Self-portrait

Link to my design code: 

# 3. Discussion

Draw from this weeks' readings to discuss in a blog post:
which of Ngai's aesthetic categories does your self-portrait (and glitch more generally) belong to, and why?
does glitch increase or decrease effective complexity, and why?