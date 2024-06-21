---
title: Week 12 Homework - AT3 Journey 
published_at: 2024-06-05
snippet: Documentation of AT3
disable_html_sanitization: true
---

### Extra User Feedbacks 

My roommate Poon expressed that she would like to have more music options and my Vietnamese friend Mai said that she would love the current music to loop since the original track only plays up to 2 minutes. I considered these suggestions but firstly it took me a long time to dive into the sound collection and pick out a satisfying track, secondly I attempted to make a loop button but there was an error which I could not fix yet, so for now to deliever the assignment on time, I will make a note of these issues and come back to update them when I have more time.

I also ponder between whether the messages should disappear along with the particles or remain on screen, because Mai brought up that the newer texts stamped on top of the older ones. There was definitely a way to erase the texts, since I noticed that if I made the exploding heart particles fall down like in the original tutorial, the text will disappear with it. However, I want the positive messages to imprint on the canvas as a reminder, albeit maybe harder to read. Another way is to write codes that delete random remaining quotes at random times to help relieve the congestion of two many quotes on the page, but I haven't figured out how to do that yet.

Additionally, since my sister brought up the idea that the exploding heart particles may turn into flowers, I tried replacing the ellipses with a flower PNG, referencing [this tutorial](https://www.youtube.com/watch?v=i2C1hrJMwz0) where Daniel Shiffman turns the Bubble objects into images of kittens. My plan was that when the mouse clicks on the quotes remained on the screen, each quote would turn into an image of flower or kitten. However, what I got was that the size of the image was too large (and it wasn't even a real PNG!) and the heart shape of the particle system was gone. I tried with my kitten jpg as well, and it was a failure too. Since I work with three classes in one sketch, it is challenging for me to make sure they combine well.

Example 1: Failed flower image
![fake flower png replaces ellipse particles](/hw_12/flower_fakepng.png)

Example 2: Failed kitten image
![cat jpg replaces ellipse particles](/hw_12/cat_failed.png)

### Other Attempts

When I tried making the music loop button, I got an error saying that `song.unLoop()` was not a valid function. I could click to change unloop to loop, but cannot toggle back to unloop. To avoid causing confusion for user, I decided to skip this button at this stage.

Inside `setup()`:
```
//Creating loop button
  button_loop = createButton("loop music")
  button_loop.position(100, 30)
  button_loop.size(70, 70)
  button_loop.mousePressed(toggleLooping)
```

Defining a function to toggle between loop and unloop music:
```
function toggleLooping() {
  if (!song.isLooping()) {
    song.loop()
     button_loop.html("unloop music")
   } else {
     song.unLoop()
     button_loop.html("loop music")
   }
 }
```

Besides, I also thought about adding `p5.capture` script to my sketch so that users can record their experience and keep it as a memento of the self-affirmation therapy session, since that extra library will create files to be downloaded to their local devices if they press the on-screen record button. However, the movement in the videos were very slow even though I tested different fps values, particularly delaying the floating heart trail of the cursor, so I disabled it in the index.html file.

About the positive quotes, although I managed to keep them fit inside the heart particles, the issue is that the alignment wasn't accurately in the centre, and longer text will go beyond the heart's edge. I tried `rectMode(CENTER)` but could not see any obvious change, so I wonder if there was any way to control the bounding box of the texts to be definitely stay inside the heart.


### Video Transcript

**My Plan**

Inspired by Lauren Lee McCarthy's work, I want to create a website that users can participate and respond to. Since both Poon, my Thai roommate and I are residents of the same student accomodation, we formed a community of practice that understand the struggles of living abroad away from our families, so having a comforting and interactive platform to remind ourselves to practice self-affirmation is our domain. 

I planned to broker creative coding knowledge from the p5js community to our community of practice so that Poon may find creative coding therapeutic, while I can also learn from her feedback to enrich my coding skill.

In short, my p5 sketch will display heart shapes with random self-affirmation messages when the mouse double-clicks on the canvas. If the mouse is dragged across the screen, the cursor will show a trail of floating hearts. There are also options of the background images from the drop-down menu, and a button to play or pause the soothing background music.


**What actually happened**

I learned to understand Patt Vira's tutorial about dragging a falling hearts cursor on YouTube, then I searched about *colours that help reduce anxiety* and tested colours in Adobe Illustrator to get the final palette hex codes. 

After that, I found another video of hers about exploding hearts using particle system, in which when the mouse is pressed, the ellipses form a heart shape then expand and disappear like a firework. However in my sketch, instead of falling down after the expansion, the particles float in random direction to feel more dreamy. I also included three random positive texts inside the heart particles. Since I wanted to add a human touch to the texts, I chose the handwriting font called Better Together by Katsia Jazwinska.

Realising the background was empty, I uploaded a sunset image taken when I was by the beach, which recalled a relaxing moment of the day for most people. To give my user more options of background images, I created a drop-down menu. Originally I wanted to have more than two pictures, but they were stretched badly so I only kept the best two.

For the audio, I found the Love Yourself track by Serge Quadrado on Freesound website very clean in quality and effective to evoke a tranquil feeling. Then I made a button to toggle between play and pause music when the user's mouse clicks once on it and set the default volume to a subtle level so that it isn't too overwhelming when the users read the messages.

With these setup of the website, I shared it to my roommate Poon, my older sister and one other Vietnamese friend called Mai. Although my roommate is the primary tester, I also wanted to know how I could improve the sketch from a third person's point of view. From the initial feedback, although they found the the visuals lovely and the music healing and easy to concentrate on, it was clear that they were confused about where to click the mouse, so I added a brief instruction to guide the experience. Both my roomate and sister also suggested that there should be more than three positive quotes to arouse users' curiosity, so I updated more after that. 


**What was interesting about the experience**

In the first draft, when Poon clicks on the play/pause music button once, the exploding hearts with messages appear beneath the button, which is a bug so later, to trigger the exploding hearts effect and avoid the confusion, I changed single clicking to double clicking. Interestingly, the double-click act mimics heartbeats. 

Moreover, my friend asked a brilliant question that why weren't the hearts of the cursor going up instead of falling down, since in our figurative saying, the heart blossoms when one's happy. Therefore in the final version, I changed the hearts' direction to embed that positive meaning.

In conclusion, the whole process of doing this project encapsulates the concept of mycelial praxis since I collaborated with someone outside my university course, gradually accummulated p5js knowledge from other creators and inputs from my users to build the website. This work transformed my roommate Poon to be more fulfilled and empowered, at the same time, it also transformed me. I realised applying creative coding to the therapy area can be beneficial. By interacting with my website, I hope the users can take away a part of the project's DNA, which is the commitment to be caring, expressive, communicative, and tech-savvy.






