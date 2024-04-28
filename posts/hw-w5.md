---
title: Week 5 Homework - Glitch
published_at: 2024-04-10
snippet: Code comments for Glitch and Pixel Sort and Glitch Self-portrait
disable_html_sanitization: true
---

# 1. Explanatory Comments 

## Glitch

```html

//create a canvas element with the id name "glitch_self_portrait" 
<canvas id="glitch_self_portrait"></canvas>

<script type="module">

   //grab the canvas element with the specified id in the document
   //assign it to the variable cnv
   const cnv = document.getElementById (`glitch_self_portrait`) 

   //retrieves the width of the parent element of the canvas
   //considering the entire width of its content
   //and assigns it to the width of the canvas. 
   //dynamically adjusts the width of the canvas 
   //to match the width of its parent element
   //ensuring that the canvas fills its container horizontally. 
   //useful for responsive designs where the canvas 
   //needs to adapt to different screen sizes or layouts.
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
   const img = new Image ()

   //load an image from a URL specified by img.src. 
   //The onload event handler executes a function when the image has finished loading. 
   img.onload = () => {
   
      //resizes the canvas height to the aspect ratio of the image relative to the width of the canvas.
      cnv.height = cnv.width * (img.height / img.width)

      //call function draw to draw the image onto the canvas when image has loaded
      draw (img)

      //toDataURL() is used here to convert the content of the canvas into a data URL
      //representing the image in JPEG format
      //the resulting image data is stored as a string in the variable img_data.
      img_data = cnv.toDataURL ("image/jpeg")

      //call add_glitch function
      add_glitch ()
   }

   //source of the original image
   //give file path to image element, when it's done loading, then the above img.onload() runs
   img.src = `/240405/pfp_glasses.jpg`

   //define function rand_int with a max parameter to returns random variations of 
   //the largest rounded integer of data chunk size
   //that are removed from the image data during the glitching process. 
   const rand_int = max => Math.floor (Math.random () * max)

   //define glitchify function to introduce glitches into the image data
   //takes base64 encoded image data, a maximum chunk size, and the number of repeats as parameters. 
   const glitchify = (data, chunk_max, repeats) => {
      //determines the size of the chunk of data to be removed for the glitch effect.
      //chunk_max is a parameter passed to the function, 
      //representing the maximum size of the chunk of data to be removed during glitching.
      //rand_int (chunk_max / 4) generates a random position between 0 and chunk_max / 4
      //Dividing by 4 might be done to limit the maximum chunk size, 
      //ensuring the glitch effect doesn't remove too much data at once.
      //Multiplying by 4 ensures that the chunk_size is a multiple of 4, 
      //which is often necessary for data manipulation operations, 
      //particularly when dealing with binary data or image data.
      const chunk_size = rand_int (chunk_max / 4) * 4
      
      //random position in the data between 24 - chunk_size
      //first 24 positions are important so we want to protect them from the glitch
      //randomly removes chunks of data from the image data to simulate glitches.
      const i = rand_int (data.length - 24 - chunk_size) + 24
   
      //the front portion grabs positions before the random position, but not including it
      const front = data.slice (0, i)
      //the back portion starts where random position is plus the chunk size, 
      //to the end elements of the data string.
      const back = data.slice (i + chunk_size, data.length)
      //the result puts the pieces together, excluding the gap of the chunk_size
      const result = front + back
      
      //conditionla (ternary) operator to return result if repeats == 0
      //otherwise call itself again with repeats -1
      return repeats == 0 ? result : glitchify (result, chunk_max, repeats - 1)
   }

   //empty array to store glitched images
   const glitch_arr = []

   //define function that adds a glitched image
   //to the glitch_arr array 
   const add_glitch = () => {

      //creates a new image object
      const i = new Image ()

      //sets its onload event
      i.onload = () => {

         //add new image into the glitch_arr array
         glitch_arr.push (i)

         //call itself until there are 12 glitched images
         if (glitch_arr.length < 12) add_glitch ()

         //Once the array is populated, it calls the draw_frame function.
         else draw_frame ()
      }
      
      //give the new image some glitched image data, chunk_max 96, repeats 6 times
      //when the new glitched image is loaded, 
      //execute the i.onload() function above.
      i.src = glitchify (img_data, 96, 6)
   }

   //Two variables is_glitching and glitch_i are initialized to control the glitch effect.
   let is_glitching = false
   let glitch_i = 0

   //Define draw_frame function, 
   //which alternates between drawing the original image and glitched images 
   //based on a probability.
   const draw_frame = () => {

      //if the image is not glitching, 
      //draw the glitched images and put them into the glitch_arr array
      if (is_glitching) draw (glitch_arr[glitch_i])

       //if the image is glitching, draw the original image.
      else draw (img)

   //probability weightings for starting and stopping the glitch
      const prob = is_glitching ? 0.05 : 0.02

      //if random value is less than probability
      if (Math.random () < prob) {

         //choose a random glitched image index
         glitch_i = rand_int (glitch_arr.length)

         //flip the stage of is_glitching
         is_glitching = !is_glitching
      }

      // call the next animation frame
      requestAnimationFrame (draw_frame)
   }

</script>
```

## Pixel Sort

The glitch method applies a glitch effect to a specified region of the canvas, which calculates the index of each pixel in the specified region and retrieves the pixel data.

The code below creates a glitch effect on an image displayed on a canvas element by sorting pixel data based on brightness.

```html

//create a canvas with an id called pixel_sort
<canvas id="pixel_sort"></canvas>

<script type="module">

   //import the PixelSorter class 
   //from a JavaScript file located at "/scripts/pixel_sort.js".
   import { PixelSorter } from "/scripts/pixel_sort.js"

   //grab the canvas element with the specified id in the document
   //and assign it to the variable cnv
   const cnv  = document.getElementById (`pixel_sort`)

   //Sizing the canvas width to the width of its parent element
   cnv.width  = cnv.parentNode.scrollWidth

   //the canvas height is adjusted to maintain a 16:9 aspect ratio.
   cnv.height = cnv.width * 9 / 16   

   //getting canvas context
   const ctx = cnv.getContext (`2d`)

   //create a new pixel sorter context element
   //and assign it to the variable sorter
   const sorter = new PixelSorter (ctx)

   //create a new image element
   const img = new Image ()

   //define function to execute drawing the image upon loading the file
   img.onload = () => {

      //resize the canvas height to be
      //the aspect ratio of the image in relation to the canvas width
      cnv.height = cnv.width * (img.height / img.width)

      //draw the image onto the canvas
      //at top left corner, using canvas width and canvas height
      ctx.drawImage (img, 0, 0, cnv.width, cnv.height)

      //call function sorter.init() to initialize the PixelSorter instance.
      sorter.init ()

      //call function draw_frame() to continuously draw frames on the canvas.
      draw_frame ()
   }

   //give file path to the source image element
   //when this image is loaded, function img.onload() above will execute.
   img.src = `/240408/kornerpark.jpg`

   //assign initial value of 0 to variable frame_count
   let frame_count = 0

   //define function to draw image recursively
   const draw_frame = () => {

      //draw image onto the canvas context
      ctx.drawImage (img, 0, 0, cnv.width, cnv.height)

      //calculating a value (sig) that oscillates smoothly between -1 and 1 
      //based on the current frame count, 
      //with a frequency determined by the division by 500 
      //(divided by larger number means slower).
      //to modulate or control certain aspects of the animation or effect being created.
      let sig = Math.cos (frame_count * 2 * Math.PI / 500)

      //mid object represents midpoint coordinates of the canvas element
      //to establish a reference point from which other calculations, 
      //such as determining the position of the glitch effect, can be based. 
      const mid = {

         x: cnv.width / 2, //x-coordinate
         y: cnv.height / 2 //y-coordinate
      }

      //dim object 
      //contains the width and height of the glitched region on the canvas.
      //to control the size and shape of the glitch effect applied to the image.
      const dim = {

         //calculating the width, no decimal number
         //value of sig oscillates smoothly between -1 and 1 
         //sig + 3 ensures that it's shifted to be in the range of 2 to 4, 
         //effectively controlling the width of the glitched region.
         //cnv.width / 6 determines the scaling factor for the width based on the canvas width. 
         //adjusts the range of the glitch effect 
         //to be approximately one-sixth of the canvas width.
         //+ 1 guarantees that the glitched region will never be zero
         //always have a minimum size of 1 pixel in each dimension.
         x: Math.floor ((sig + 3) * (cnv.width / 6)) + 1,

         //calculating the height, no decimal number
         //sig + 1 controls the height of the glitched region,
         //cnv.height / 6 determines the scaling factor for the height based on the canvas height. 
         //adjusts the range of the glitch effect 
         //to be approximately one-sixth of the canvas height.
         y: Math.floor ((sig + 1) * (cnv.height / 6)) + 1
      }

      //glitch effect's position, no decimal
      const pos = {
         x: Math.floor (mid.x - (dim.x / 2)),
         y: Math.floor (mid.y - (dim.y / 2))
      }

      //call function sorter.glitch to apply the glitch effect to the image
      //based on frame count
      sorter.glitch (pos, dim)

      //increase frame_count by 1 each frame
      frame_count++

      //call the next animation frame
      requestAnimationFrame (draw_frame)
   }

</script>
```

```javascript
// pixel_sort.js

//define quicksort function to sort an array of objects 
//based on the value of their br property (brightness).
//parameter a is an array
const quicksort = a => {

   //if the a array is less than or equal 1, then return the a array
   if (a.length <= 1) return a

   //assign first element with index 0 of the a array to variable pivot
   let pivot = a[0]

   //instantiating an empty array for variable left and variable right
   let left = []
   let right = []

   //for loop
   //if the 
   for (let i = 1; i < a.length; i++) {

      //
      //push the element to the left array
      if (a[i].br < pivot.br) left.push (a[i])

      //otherwise, push the element to the right array
      else right.push (a[i])
   }

   //... means spread syntax
   //mdn: 'allows an iterable, such as an array or string, 
   //to be expanded in places where zero or more arguments (for function calls) 
   //or elements (for array literals) are expected.'
   //assign the sorted array to include all element 
   //from the left array, first element of the pivot array, 
   //and all elements of the right array
   const sorted = [ ...quicksort (left), pivot, ...quicksort (right) ]

   return sorted
}

//define class name
export class PixelSorter {

   //constructoraccepts a canvas 2D rendering context (ctx) as a parameter
   //and stores it in the this.ctx variable.
   constructor (ctx) {
      this.ctx = ctx
   }

   //define function init to initialize the PixelSorter instance 
   //by storing the canvas width and height
   //and get image data from the context
   init () {

      //resize the width and height of the initial pixel sorter object 
      //to be the width and height of the canvas
      this.width = this.ctx.canvas.width
      this.height = this.ctx.canvas.height

      //retrieving the image data from the canvas.
      this.img_data = this.ctx.getImageData (0, 0, this.width, this.height).data
   }

   //define glitch function applies a glitch effect to a specified region of the canvas
   //parameter position and dimension
   glitch (pos, dim) {

      //define function find_i to find position of the glitch
      const find_i = c => ((c.y * this.ctx.canvas.width) + c.x) * 4 

      //nested for loop
      for (let x_off = 0; x_off < dim.x; x_off++) {

         //instantiating an empty array to variable positions
         const positions = []
         
         //if y_pos of the pixel 
         for (let y_pos = pos.y; y_pos < pos.y + dim.y; y_pos++) {

            //push the new positions into the positions array
            positions.push (find_i ({ x: pos.x + x_off, y: y_pos }))
         }

         //instantiating an empty array to variable unsorted
         const unsorted = []

         //for each position element in the positions arrray
         positions.forEach (p => {

            //red channel
            const r = this.img_data[p]

            //green channel
            const g = this.img_data[p + 1]

            //blue channel
            const b = this.img_data[p + 2]

            //alpha channel
            const a = this.img_data[p + 3]

            //brightness
            const br = r * g * b

            //push the unsorted positions into the unsorted array
            unsorted.push ({ r, g, b, a, br })
         })

         //The pixel data is sorted based on brightness using the quicksort function.
         //reverese the order of the elements inside the unsorted array, 
         //when the array is passed into quicksort function
         //so, the pixel data is sorted based on br, a, b, g, r
         const sorted = quicksort (unsorted).reverse ()

         // //instantiating an empty array to variable rgba
         let rgba = []


         //for each sorted pixel, 
         //push its data into the rgba array
         sorted.forEach (e => {
            rgba.push (e.r)
            rgba.push (e.g)
            rgba.push (e.b)
            rgba.push (e.a)
         })

         //create a new array element that turns the colours of the positions into 8-bit style.
         //Uint8ClampedArray is an array of 8-bit unsigned integers clamped to 0–255. 
         //The contents are initialized to 0.
         rgba = new Uint8ClampedArray (rgba)

         //turning the rgba array into the 8-bit format and assign to the new_data variable
         const new_data = this.ctx.createImageData (1, dim.y)
         
         //set the new data format into the data
         new_data.data.set (rgba)

         //put the new data back onto the canvas context
         this.ctx.putImageData (new_data, pos.x + x_off, pos.y)
      }
   }
}


```

# 2. Glitch Self-portrait

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
   
   img.src = `/example_hw_5/Sophie_portrait.jpg`

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

      //set max chunk size 60, repeats 10 times
      i.src = glitchify (img_data, 60, 10)
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

I use lecture's glitch template but change the source image file path to my own image, reduce the max chunk size to 60 and set a repeat of 10 times.

# 3. Discussion

Referring to Ngai's aesthetic category, my self-portrait and the glitch style generally fit into the zany category, because the results have a quality of being hot and bothered. The aesthetic also critically disturbs the viewer's interaction with the artwork and this kind of experience came from the artist's 'affective labour', involving processes of manipulation, trials and errors, and re-formation. Thus, the glitch effect is closely related to production, a key characteristic of Ngai's zany aesthetic.

Glitch decreases the effective complexity, since it pushes re-dundancy out of balance in relation to elements of non-randomness. For instance, the glitch effect obscures my self portrait with repetitive and abstract patterns.

![Glitch decreases effective complexity](/example_hw_5/glitch_decrease_EF.png)
