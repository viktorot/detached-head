---
title: "Objective 1 - Uncover Santa's Gift List"
date: 2021-01-17T00:00:00Z
draft: false
hide_on_homepage: false
---

The story start as we are thrown near the 7A highway exit. We're greeted by an elf named `Jingle Ringford`. He tells us that we can use the gondola next to him to reach Santa's castle. Before leaving he points out a billboard next to the highway that has an image, part of which is distorted.

# Objective
Our task it to try and read the distorted part of the billboard image and find out what `Josh Wright` is getting for the holidays.

# Solution
The original image looks like this:

[![Original](/kringle20/billboard.png)](/kringle20/billboard.png)

With a little bit of googling of how to `un-twirl` or `un-swirl` an image, we can find a [couple](https://cybercop-training.ch/?p=457) of [articles](https://matzjb.se/2015/07/26/deconstructing-swirl-face/) that tell us that this type of distortion or obfuscation can be reversed.<br>

Next step is to find a tool that can help us with the de-obfuscation. I ultimately used [this](https://www.photopea.com/) tool. With a little playing around with `lasso` and the `swirl` tools, I managed to undo the obfuscation. 

[![Solution](/kringle20/billboard-unswirled.png)](/kringle20/billboard-unswirled.png)

Tho it's not perfect, it's enough to read the solution for the objective. We can see that `Josh Wright` is getting a `proxmark`.<br>

------------------

You can find the solutions to the other challenges [here](/post/kringle20/intro/).

