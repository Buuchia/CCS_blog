---
title: Week 5 Homework - Glitch Explain
published_at: 2024-04-17
snippet: Thomas' explanation
disable_html_sanitization: true
---

<canvas id="glitch_self_portrait"></canvas>

<script type="module">

//getting canvas element
   const cnv = document.getElementById (`glitch_self_portrait`)

   //sizing to be good size
   cnv.width = cnv.parentNode.scrollWidth
   cnv.height = cnv.width * 9 / 16

   //setting background color
   cnv.style.backgroundColor = `deeppink`

//getting canvas context
   const ctx = cnv.getContext (`2d`)

//initiating variable for the image data, blank now
   let img_data

//arrow syntax --> function
//takes argument i
//draw the image to the canvas context
   const draw = i => ctx.drawImage (i, 0, 0, cnv.width, cnv.height)

//create a new image element
   const img = new Image ()

   //define function to execute upon loading file
   img.onload = () => {
    //resizing the height of the canvas
    //to be same aspect ratop as image
      cnv.height = cnv.width * (img.height / img.width)

      //draw the image to canvas
      draw (img)

      //storing image data as string in img_data
      img_data = cnv.toDataURL ("image/jpeg")

      //call the glitch function
      add_glitch ()
   }

//change path to my image here
//give file ath to image element, when it's done loading, then img.onload () runs
   img.src = `/example_hw_5/IMG_7764.JPG`

//definine a function that returns a random value between 0 - max, not including the max, no decimal
   const rand_int = max => Math.floor (Math.random () * max)

//define a recursive function
//take data as string
   const glitchify = (data, chunk_max, repeats) => {

    //random multiple of 4 between 0 - chunk_max
      const chunk_size = rand_int (chunk_max / 4) * 4

      //random position in the data between 24 - chunk_size
      //first 24 positions are important so we want to protect them from the glitch
      const i = rand_int (data.length - 24 - chunk_size) + 24

      //grabbing all the data before the random position
      const front = data.slice (0, i)

      //leaving a gap the size of chunk_size
      //grabbing the rest of the data
      const back = data.slice (i + chunk_size, data.length)

      //putting the two pieces back together
      //leaving out a chunk
      const result = front + back

      //
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
```