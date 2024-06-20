---
title: Week 12 Homework - AT3 Journey 
published_at: 2024-05-29
snippet: Documentation of AT3
disable_html_sanitization: true
---

Script for my A3

## My Plan

Inspired by Lauren Lee McCarthy's I heard TALKING IS DANGEROUS, I want to create a website that users can participate and respond to for my A3 Creative Coding assignment. Since both Poon, my Thai roommate and I are international students living in the same Scape community of practice, we both understand the struggles of living abroad away from our family and hometowns, such as experiencing self-doubt and low self-esteem, so we need a platform where we can get emotional support to take care of our mindfulness and mental well-being. Therefore, having a safe, comforting, and interactive digital space, which is the website that reminds my roommate to practice self-affirmation is the domain of our community of practice. 

To make this work, I am going to broker creative coding knowledge from the p5js community, which I have been a part of since the start of this semester, to our community of practice. In doing so, I hope Poon may find creative coding therapeutic, while I can also learn from her feedback to enrich my coding skill as she had some experience in play-testing websites from her previous jobs.

In short, my p5 sketch will display heart shapes with random self-affirmation messages when Poon's mouse double-clicks on the canvas. If she drags the mouse across the canvas, the cursor will show a trail of floating hearts. She can also choose the background images from the drop-down menu, and play or pause the soothing background music.


## What actually happened

I remember that I saw a tutorial about dragging a falling hearts cursor on Patt Vira's YouTube channel and it would fit really well with my concept so I learned to understand her walk-through. Then I searched about 'colours that help reduce anxiety' on Google, but the results weren't really helpful because the sites listed almost every hue, so I picked and tested colours in Adobe Illustrator to get the final palette. 

Once I finished that tutorial, I found another video of hers about exploding hearts using particle system, in which when the mouse is pressed, they form a heart shape then expand and disappear like a firework. However in my sketch, instead of falling down after the expansion, the particles float in random direction, since I find it having a more dreamy effect. I also included some codes to generate three random positive texts inside the heart particles. Since I want to add a human touch to the texts, I chose the handwriting font called Better Together by Katsia Jazwinska.

Then I realised the background was a little empty, so I wanted to do something interesting with it. I loaded a sunset image that I took when I was by the beach, which is a pleasant moment of the day that most people feel relaxed with.

For the audio, I went to Freesound website and looked up some meditative soundtracks. Fortunately, I found the Love Yourself track by Serge Quadrado very clean in quality and effective in evoking a tranquil feeling. Then I make a button to toggle between play and pause when the user's mouse clicks on it and set the volume to a subtle level so that it isn't too overwhelming when the users read the on-screen messages.

With these basic setup of the website, I shared it to my roommate Poon, my older sister and one other Vietnamese friend named Mai. Although my roommate is the primary tester, I also want to know how I can improve the sketch from a third person's point of view. From the initial feedback, although they find the the visuals lovely, and the music healing and easy to concentrate on, it was clear that they were confused about how the mouse were going to interact with the canvas, so having a brief instruction on the web is necessary to guide the experience. Both my roomate and sister also suggested that there should be more than three positive quotes, so I updated more after that. 

One more thing Poon mentioned is to have the option to adjust music and Mai also said that she would love the music to loop since the original track only plays up to 2 minutes. I considered these suggestions but firstly it took me a long time to dive into the sound collection and pick out a satisfying track, secondly I attempted to make a loop button but there was an error which I could not fix yet, so for now to deliever the assignment on time, I will make a note of these issues and come back to update them when I have more time.

To compensate that, I created a drop-down menu to provide options of background images. Originally, I want to have at least 3 pictures, one of which shows my cat, because kittens are perfect therapy companions! But the images were so stretched in full-screen viewport so I only kept the best two, the sunset and the sky.


## What was interesting about the experience

In the first draft, when Poon clicks on the play/pause button once, the exploding hearts with messages appear beneath the button, which is a bug so later I changed single clicking to double clicking to trigger the exploding hearts effect. Besides, I think the double-click act mimics the onomatopoeia of heartbeats, which sounds like "thump thump". 

Interestingly, my friend asked that why weren't the hearts of the cursor going up instead of falling down, since in our figurative saying, the heart blossoms when one's happy. At first I thought the falling hearts is like showering the user with love rain, but I agree with my friend that the hearts floating upward embeds a more positive meaning, so I changed the hearts' direction. 

I also ponder between whether the messages should disappear along with the particles or remain on screen, because Mai brought up that the newer texts stamped on top of the older ones. There was definitely a way to erase the texts but I want the positive messages to imprint on the canvas as a reminder, albeit maybe harder to read.

In conclusion, the whole process of doing this project encapsulates the concept of mycelial praxis since I reached out to a member of a new community outside my university course to learn how I can use creative coding to provide a solution, and gradually accummulated p5js knowledge, music and font sources from other creators, together with inputs from my users to enhance the outcome. This work transformed my roommate Poon to be more fulfilled and empowered, at the same time, it also transformed me. I realised applying creative coding to the therapy area can be beneficial and I also felt good about myself the more I read the positive quotes during the tests. By interacting with my website, I hope the users can take away a part of the project's DNA, which is the commitment to be caring, expressive, communicative, and tech-savvy.










