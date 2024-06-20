---
title: Week 12 Homework - AT3 Journey 
published_at: 2024-05-29
snippet: Documentation of AT3
disable_html_sanitization: true
---

Script for my A3

## My Plan

Inspired by Lauren Lee McCarthy's work, I want to create a website that users can participate and respond to. Since both Poon, my Thai roommate and I are international students living in the same Scape community of practice, we understand the struggles of living abroad away from our families, so having a comforting and interactive digital platform to remind ourselves to practice self-affirmation is the domain of our community of practice. 

I planned to broker creative coding knowledge from the p5js community to our community of practice so that Poon may find creative coding therapeutic, while I can also learn from her feedback to enrich my coding skill.

In short, my p5 sketch will display heart shapes with random self-affirmation messages when the mouse double-clicks on the canvas. If the mouse is dragged across the canvas, the cursor will show a trail of floating hearts. There are also options of the background images from the drop-down menu, and a button to play or pause the soothing background music.


## What actually happened

I learned to understand Patt Vira's tutorial about dragging a falling hearts cursor on YouTube, then I searched about 'colours that help reduce anxiety' on Google and tested colours in Adobe Illustrator to get the final palette hex codes. 

After that, I found another video of hers about exploding hearts using particle system, in which when the mouse is pressed, the ellipses form a heart shape then expand and disappear like a firework. However in my sketch, instead of falling down after the expansion, the particles float in random direction to feel more dreamy. I also included three random positive texts inside the heart particles. Since I wanted to add a human touch to the texts, I chose the handwriting font called Better Together by Katsia Jazwinska.

Realising the background was empty, I uploaded a sunset image taken when I was by the beach, which recalled a relaxing moment of the day for most people. To give my user more options of background images, I created a drop-down menu. Originally I want to have more than two, but the pictures were stretched badly so I only kept the best two, which are the sunset and the sky outside the airplane window.

For the audio, I found the Love Yourself track by Serge Quadrado on Freesound website very clean in quality and effective to evoke a tranquil feeling. Then I make a button to toggle between play and pause music when the user's mouse clicks once on it and set the default volume to a subtle level so that it isn't too overwhelming when the users read the on-screen messages.

With these setup of the website, I shared it to my roommate Poon, my older sister and one other Vietnamese friend called Mai. Although my roommate is the primary tester, I also want to know how I can improve the sketch from a third person's point of view. From the initial feedback, although they find the the visuals lovely and the music healing and easy to concentrate on, it was clear that they were confused about where to click the mouse, so I added a brief instruction on the web to guide the experience. Both my roomate and sister also suggested that there should be more than three positive quotes to arouse users' curiosity, so I updated more after that. 


## What was interesting about the experience

In the first draft, when Poon clicks on the play/pause music button once, the exploding hearts with messages appear beneath the button, which is a bug so later, to trigger the exploding hearts effect, I changed single clicking to double clicking. Interestingly, the double-click act mimics heartbeats. 

Moreover, my friend asked a brilliant question that why weren't the hearts of the cursor going up instead of falling down, since in our figurative saying, the heart blossoms when one's happy. Therefore in the final version, I changed the hearts' direction to embed that positive meaning.

In conclusion, the whole process of doing this project encapsulates the concept of mycelial praxis since I reached out to a member of a new community outside my university course, gradually accummulated p5js knowledge from other creators and inputs from my users to enhance get build the website. This work transformed my roommate Poon to be more fulfilled and empowered, at the same time, it also transformed me. I realised applying creative coding to the therapy area can be beneficial. By interacting with my website, I hope the users can take away a part of the project's DNA, which is the commitment to be caring, expressive, communicative, and tech-savvy.





One more thing Poon mentioned is to have the option to adjust music and Mai also said that she would love the music to loop since the original track only plays up to 2 minutes. I considered these suggestions but firstly it took me a long time to dive into the sound collection and pick out a satisfying track, secondly I attempted to make a loop button but there was an error which I could not fix yet, so for now to deliever the assignment on time, I will make a note of these issues and come back to update them when I have more time.

I also ponder between whether the messages should disappear along with the particles or remain on screen, because Mai brought up that the newer texts stamped on top of the older ones. There was definitely a way to erase the texts but I want the positive messages to imprint on the canvas as a reminder, albeit maybe harder to read.





