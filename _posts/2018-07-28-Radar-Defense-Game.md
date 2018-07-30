---
layout: post
section-type: post
title: Radar Defense Game
category: Projects
tags: [ 'arduino', 'python', 'game' ]
---
![Radar Defense v1.0-beta](/img/radar-defense/radar-defense-v1.0-beta.png "Radar Defense v1.0-beta")

As my first (actual) post on my blog, I decided to write about a game I made this week, which I named _Radar Defense_.

About a week ago I had found a picture of a cool game that was programmed in python with [pygame](https://www.pygame.org/docs/), a python library for making simple 2d games. What I thought was really cool about it was the guy had also built an external controller to control the on-screen game. When I saw it, I decided that I wanted to make something like that.

Since I just saw the picture and didn't read about it, I didn't know
how that guy's game actually worked. But I took inspiration from these
kind of radio wave things in the picture of his game and decided to
make some kind of defense game with a cool radar.

I started by making some very rough sketches of what I wanted the game
to look like, and how I wanted it to work. I then started programming
it.

Since I haven't used pygame much before, It took a little while to
settle on a game structure I liked, but eventually I settled on
something like this:

<pre><code data-trim class="python">
...
from gameObjects import *

class Game:
    ...
    gameObjects = [] # list of all objects that make up the game
    enemy = Enemy(self) # create new enemy
    gameObjects.append(enemy) # add the enemy to the list of objects
    ...
    self.initGame() # start the game
    ...
    for o in gameObjects:
        if o.enabled:
            o.update() # update each enabled object
    ...
</code></pre>

After I settled on a structure, I wrote some classes, like the radar class, the shield class, an energy class - by this point, I had mostly decided how the game would work. These are the opening lines in the instructions built into my game :grin::

>You are the director of a top secret base. 
>Unfortunately, your power generator broke down today, and some
>enemies have taken the opportunity to come and attack you with
>missiles. Without power, you cannot operate your shields, and 
>cannot block the missiles. Luckily, you have several backup supply
>batteries, but they will only last for so long. Your job is to
>survive long enough for your engineers to fix the power generator...

You could use your radar to scan the surrounding skies for missiles,
but having it on all the time would use way to much energy. So the
"energy" class manages the energy.

After I got to this point, I decided to start work on the controller.
I had already decided to use an [arduino](https://www.arduino.cc/) to
make it easy, but I had a problem. There aren't enough pins on the
arduino to manage all the buttons, switches, and knobs I was planning
to integrate into my controller. I had some [74HC595](https://www.arduino.cc/en/tutorial/ShiftOut) shift registers that I had ordered for a previous project, and I realized that I could use one of those to reduce the pins needed to operate a rotary switch from eight to four.

I would, using the shift register, individually turn each pin on the
rotary switch HIGH, read the common pin, and then turn it LOW
again. If the common pin was HIGH, it meant that the switch was switched to that pin, because voltage would flow into the pin and out the common pin only if the switch was rotated to that position.

After solving that problem, I added a an eight-switch DIP micro-switch
IC and two buttons, one to turn the radar on and one to activate a
feature I'll talk about later.

You can see the prototype controller here (I am planning to 3d print a case for it sometime in the future):
![prototype controller](/img/radar-defense/controller-prototype.JPG "the prototype for the controller")
You can see the eight-switch DIP on the right.

I wrote and uploaded (using a fantastic command line arduino-compatible program called [PlatformIO](https://platformio.org/)) 
some arduino code that would send an array that contained the state of every button, knob, and switch every 30 milliseconds to
the python program.

After I had the controller working, I programmed the missiles. Getting these working took a little more work, as I had to find a line-intersects-circle algorithm to detect when a missile smashed into a shield. I also programmed some other classes, like a game-over class, game-start class, etc. Finally, I had a working, playable prototype - if a missile hits your base, it explodes and you lose, if the counter in the upper right corner reached ten, you win.

I then decided to add an "anti-missile" system. This would consist of several "cameras" at the bottom of your screen which would let you
know if a missile got past your shields. If a missile got past, you
would then press a button, and then press the button again when the
camera the missile was seen in was highlighted. You would do this again with a crosshair on the camera - your goal would be to aim right at the missile, but you have to get the timing just right.

While writing this class, I had to calculate several cartesian distances to see if a missile was the right distance from the radar. This turned out to be super easy: You seperate the distances into the x and y planes, then use the Pythagorean theorom to calculate the hypotenuse:

    #psuedocode:
    xDist = x2 - x1
    yDist = y2 - y1
    
    Pythagorean theorom: a^2 + b^2 = c^2, so 
    
    xDist^2 + yDist^2 = hypotenuse^2, so
    dist = squareroot(xDist^2 + yDist^2)
    
I also had to calculate whether a point is within a circle, which obviously was as simple as using the above formula and checking to see if the distance between the point and the center of the circle was less than the radius of the circle.

Overall, I am happy with how the game (my first big pygame project) turned out - and it was really fun making it. Of course, the game still needs quite a bit of work, for example, better graphics (:grimacing:) would be nice, but I'll probably work on those sometime in the future.

If you would like to try out my game, go ahead! You can [download the source code here](https://github.com/scitronboy/radar-defense/archive/v1.1beta.zip "Radar Defense v1.1-beta.zip"), there are instructions in the [README](https://github.com/scitronboy/radar-defense/blob/master/README.md). If you like it, please star or fork it on github, and please leave any feedback or questions in the comment section below.

[View Radar Defense on Github.](https://github.com/scitronboy/radar-defense)
    




