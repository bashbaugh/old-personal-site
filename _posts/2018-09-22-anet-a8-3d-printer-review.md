---
layout: post
section-type: post

updated: "October 14 2018"

title: "Anet A8 3d Printer Review"
image: "/img/post-thumbnails/anet-a8.thm"
slug: "anet-a8-3d-printer-review"
category: "3dp"
tags: [ 'review', '3d-printing', 'anet-a8' ]

titletag: "Anet A8 3d Printer Review"
description: "Anet A8 3d printer kit review: Is the Anet A8 3d printer any good? In this post, I talk about my experience of building and using the anet a8. The anet a8 is a cheap DIY 3d printer kit, priced at about $160."
keywords: "anet, a8, review, anet a8 2018, is the anet a8 good, anet a8 review 2018, best cheap 3d printer, 2018, $200 dollar 3d printer, how does a 3d printer work"
---

When the very first commercially available <a data-toggle="tooltip" title="Fused Deposition Modeling/Fused Filament Fabrication" data-placement="bottom">FDM/FFF</a> 3d printers were put on the market in the late 1990s, they cost thousands. Luckily, prices of 3d printers have dropped and dropped. In 2012, an article in a publication by TIME magazine stated that, "[3d printers are likely to be out of most people's budgets, good printers] are priced at well over $2000 dollars."  And now, you can get a good 3d printer for $600 or less.

One of the most famous and popular budget 3d printers is the _Anet A8_. The A8 is priced at an amazingly cheap $180, but it can usually be found on sale for as cheap as _$140._ But at this price, is the Anet A8 any good? This is a question most people have before ordering their Anet A8. But before I get into the review, let me explain how a 3d printer works, for those of you who don't completely know.

### How does a 3d printer work?

A FDM/FFF(The most common type of 3d printer, when someone says "3d printer," they are probably talking about a FFF/FDM printer) 3d printer works kind of like a regular inkjet printer, except it uses plastic instead of ink, and it layers the plastic up on top of itself until you have a 3d object. The part of the printer that dispenses the plastic is called the "extruder". Have you ever used a glue gun before? If you can imagine squeezing glue (like the plastic) out of the glue gun (like the extruder) and layering it up on top of itself, that's what's happening when a 3d printer prints.

![Picture of Glue Gun Extruding Glue](/img/3dp/glueGunExtruding.gif "A glue gun 'extruding'")

So here is what happens when the anet a8 printer prints:

1. First of all, you create a digital GCODE file that contains instructions for the printer on how to print. You send the file to the printer. I'll go over this in more detail later.
2. You press the print button to start the print. The first think the printer will do is heat up the print bed (if you enabled it). The print bed is the platform that the print sits on. Then the printer will heat up the "hot-end", which is the part of the extruder that heats up to melt the plastic.
3. Once the heating is done, the print will start. The extruder will lower itself until it is almost touching the print bed. Then, a motor in the extruder will push the plastic filament into the hot-end, causing it to melt and get squeezed out the nozzle (at the bottom of the hot-end) onto the build plate in a fine string of plastic.
4. While this is happening, two motors (one for the the x axis and one for the y axis) will move the _whole entire extruder_ around the build plate, causing the plastic to be layed down in just the right places.
5. After the layer of plastic is finished, two more motors will lift the extruder up just a tiny bit, so the printer can continue, this time laying plastic down on the prevoiusly printed layer. The second layer "melts" onto the first layer, so the two become fused together.
6. When the second layer finishes, the printer will start on the third, then the fourth, then the fifth, etc....
7. The printer will keep going until the print is finished.

![Picture of Anet A8 Extruding]() 


So... On to the review! First I'll talk about putting the printer together.

### The Build

The Anet A8 is a 3d printer kit - that means that it comes in a box full of parts and you have to assemble it on your own, using the instructions and many instructional videos available. For some people, that might be a con - but for me, personally, I think putting the printer together is one of the funnest parts!

In my opinion, putting the printer together wasn't that hard. Many youtubers claim that the pdf instructions that come with the printer are awful and unusable, but I didn't find them to be that bad. I mostly followed the pdf instructions, and used a video whenever I needed clarification (I think I used [this video](https://www.youtube.com/watch?v=nSdP-1c4WQI)).

The build was mostly straightforward; one of the biggest problems I encountered was related to the low quality of the motherboard (the central processing board that controls the printer): the extruder heater screw terminal had disconnected from the board and fallen off due to poor, low-quality soldering. 
![Image of the screw terminal that fell off](/img/3dp/aneta8/disconnected_screw_terminal.jpeg "The screw terminal that fell off")
(the screw terminal outlined in orange fell off). 
I own a soldering iron and was able to quickly re-solder the terminal back onto the board, but it might be a problem if you don't own a soldering iron. Luckily, I have never heard of this happening to anyone else.

One possible mistake I noticed in the instructions was the orienation of the build plate. The instructions direct you to install the build plate with the central metal bar facing upwards - the problem with this is that it forces the y belt (the belt that moves the build plate) to bend upwards to connect with the build plate. Normally, the belt _should_ be flat - so I don't know whether the Anet instructions are incorrect or the firmware that comes with the printer is adjusted to compensate for the slight warp in the belt. Either way, I'm pretty sure that it's better to have a flat y belt, so I just ended up flipping the plate over so the center bar is on the bottom. This mostly fixes the issue.

_That's the end for now! The next part is coming soon._



