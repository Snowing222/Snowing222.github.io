---
layout: post
title:      "JAVASCRIPT PROJECT: HI QTEE!"
date:       2020-12-24 19:57:35 +0000
permalink:  javascript_project_hi_qtee
---


For my JS/Rails project, I created a Tshirt Design App that allows users to create and customize a T-shirt. Users can select T-shirt color, add a font, apply different font styles, add shapes, as well as set freehand drawing mode.

It isn't a straight forward project, and I had no idea how those functionalities can be achieved and what tool to use in the beginning. I did a lot of research and figured out the canvas,  fabric.js might be the right route for me..but still I wasn't sure, and I did not have the confidence at all.  The hardest part of this project for me is, I was not able to layout the blueprint when I started the project. I wanted to see if I can pull it off, and how far I can push it.  However, this kind of mindset gives me some trouble after, when I wanted to add more functionalities and more styles. It shows me how important planing the project is.

Some chandelles in this project are:

1. When a user clicked the save canvas button, it is not the moment Ithat T-shirt info is sent to the backend. Tshirt info will not be saved until the user inputs their email. So I will have to store T-shirt info first and send a nested data object with both user info and T-shirt info to the backend and save them both at the same time.

2. learn how to use fabric.js library
It involved a lot of document reading and video watching. Once I got a hold of it. It was pretty straight forward. I'm always pretty intimidated by using a library. I think it's a good practice for me.

3. how to export the newly created image and saved it as img_src.
Once again, after research. I decided to use domtoImage. It turns an arbitrary DOM node into a vector (SVG) or raster (PNG or JPEG) image, written in JavaScript.

4. CSS styling
This is the project that we finally put frontend and backend together. Figuring out how data is communicated from front end JS to backend Rails made it all the more rewarding to see everything “come to life”. I feel the moving pieces we learn from the previous sector finally come together.
