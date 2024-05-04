---
title: Week 8 Homework - AT2 Documentation
published_at: 2024-05-04
snippet: Assignment 2's Creative Process
disable_html_sanitization: true
---

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

## TESTS

1. Keeping the original fill style and the rest of the code and adding rotation of the canvas like in the code below:

```html
ctx.rotate(Math.PI / 4)
ctx.fillStyle = 'rgba(0, 0, 0, 0.05)'
```
the characters also rotate when falling down, but I don't like the edges of the rotated canvas. It takes them some time to become darker and darker. 

![before](/a2_matrix_rain/black_rec_rotate_pi4_before.png)

![after](/a2_matrix_rain/black_rec_rotate_pi4_after.png)

2. I change the fill style of context from this:

```html
ctx.fillStyle = 'rgba(0, 0, 0, 0.05)'
```
to this:

```html
ctx.fillStyle = 'hsl(' + Math.random() * 360 + ', 100%, 30%)'
```
so the background changes colour randomly. Though the characters are still falling, and the fading of the character strings is harder to see.

![random background colour](/a2_matrix_rain/random_bg_color.png)


