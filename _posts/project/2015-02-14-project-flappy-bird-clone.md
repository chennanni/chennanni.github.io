---
layout: post
title:  "Flappy Bird Clone"
date:   2015-02-14
categories: [project, game]
comments: true
---

## 1. Inspiration

First of all, I’m a big fan of playing games and it’s one of the reasons why I want to be a software developer. 
This game is the outcome of my initial learning phrase of programming. 
I took a few days to figure all things out. 
My inspiration mainly comes from two places.

### One

The first is [Beginning Game Programming for Teens with Python](https://www.raywenderlich.com/24252/beginning-game-programming-for-teens-with-python). 
It’s a great article by Julian Meyer, good for beginner to learn both Python and Game design. 

### Two

Another good source is [How to make a Flappy Bird clone by lessmilk](http://www.lessmilk.com/tutorial/flappy-bird-phaser-1). 
The site publishes a new game in HTML at a weekly basis.(Fabulous Game and Tutorial!)

![flappy-bird-1](/source/img/flappy-bird-1.png)

So after I read these two GREAT blogs(I highly recommend you to read them yourself), 
I decided to make a Flappy Bird Clone in Python which is a combination of what I learned from these two articles.

In the process of development, my friends told me a cool website: code.org. 
It’s a good website to introduce programming to the public. I found Flappy Bird there too.

Next I’ll talk about details of how I made this game.

## 2. Flap the Bird

### Set up environment & recourses

To get started, you need to install Python in your computer.

After that, you are suggested to install Sublime text 2, which is what I used to edit Python code. 
It’s not necessary but it’ll make things a lot easier.

After that, you also need to download PyGame. 
It’s a library for Python which is used to make games. Make sure you download the right version of PyGame for Python.

> “To verify that you have PyGame installed properly, open IDLE or run Python via the Terminal and type in import pygame at the Python prompt. 
If this doesn’t result in any output, then you’re good.”

### Get Started

Great! Now let’s talk about some real codes.

~~~ python
#1-Import library
import pygame
from pygame.locals import *

#2-Initialization
pygame.init()
BOUND_W = 500
BOUND_H = 600
width, height = BOUND_W, BOUND_H
screen = pygame.display.set_mode((width, height))
bird_x = 100
bird_y = 200
START_V = 0.1
START_G = 0.001
velocity = START_V
gravity = START_G
key_firstPressed = False
key = False
key_previous = False

#3-Load images
bird = pygame.image.load("resources/image/bird.png")
pipe = pygame.image.load("resources/image/pipe.png")
The first part import Pygame library.
~~~

The second part is initialization. 

- `pygame.init()` means “Initialize all imported Pygame modules”.
- `BOUND` is the boundary of the game. I write it in Capital so that I know it’s a constant value in runtime and it’s also easy for me to change it if needed.
- `bird_x` and `bird_y` are the location of the bird in the screen.
- `START_V` and `START_G` are initial velocity and gravity of the game. We can change these two variables to make the birds fly in a different way.(You can choose two values to make the bird flap in a way that it’s not easy to crash! It’s exactly what I want when playing the real Flappy Bird game!)
- `key`, `key_firstPressed`, `key_previous` are used in key detection. I’ll talk about it when we get that.
- `screen = pygame.display.set_mode((width, height))` decide the size of the frame of the game
- `bird = pygame.image.load("resources/image/bird.png")` tell Pygame to load image and map this image to bird

### Display the bird

Next, we want to display these image in the screen. 
Put these codes right after the previous codes. Make sure the indent is correct. 
If you have an err, look into the sample code of this part.

~~~ python
#4-Main Function
while 1:
    #clear screen and fill the background color
    screen.fill((0,255,255))
    #draw elements
    screen.blit(bird,(bird_x,bird_y))
    #update the screen
    pygame.display.flip()
    #add gravity
    velocity += gravity
    bird_y+=velocity
    #set boundary
    if bird_y <= -60:
        #no change
        bird_y-=velocity
    elif bird_y >= BOUND_H:
        #bird die, stop the game, restart
        bird_y = 200
        velocity = START_V
        gravity = START_G
~~~

First, we fill the background with blue color. 
In a real game instead of setting color, we may want to set some real background image. 
The mechanism is the same. Then we draw bird! 
After that we add gravity to the game so that the bird will fall without flapping.

### Key detection–Flap

If you run the above code, what you’ll see is the bird keeps falling from the center of the screen to the bottom of the screen and then starts over. 
But we don’t want to bird to keep falling, we want to hit a key and make the bird flap. 
Add these code at the end of `#4`. Again, care the indent!

~~~ python
    #Flap bird
    if key_firstPressed == True:
        velocity -= START_V*10
        #set the max velocity change a flap can do
        if velocity < -START_V*5:
            velocity = -START_V*5
        key_firstPressed = False
    #add key detection
    #when the bird is upping, do not allow another flap
    keydetection()
~~~

It’s easy to understand: if you first press a key, then the velocity of the bird will change which means action “flap”. 
Of course we don’t want the bird to flap too high so we need to set the max flap a bird can do. 
Also, when the bird is going up, we do not allow another flap.

Notice: here’s how we want the bird to behave when flap. My first implementation is slightly different from my later edition, but the mechanism is similar. 
I also encourage you to write your own flap function, create a unique bird!

We still have one thing missing which is `keydetection()`. Add these code between `#3` and `#4`.

~~~ python
#Key detection
def keydetection () :
    # print "keydetection"
    global key_firstPressed
    global key
    global key_previous
    #update events
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            pygame.quit()
            exit(0)
    #detect key: space
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                #detect if it's the first time pressed the key
                if key_previous == True:
                    key_firstPressed = False
                else:
                    key_firstPressed = True
                #anyway, set key value true
                key = True
        if event.type == pygame.KEYUP:
            if event.key==pygame.K_SPACE:
                key = False
        #exit the game
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                pygame.quit()
                exit(0)
    #update key
    key_previous = key
~~~

This function does two things: 

- first, detect if someone want to close the game. 
- second detect if someone first press the “SPACE” key. Here first press means the whole process of press and release is treated as one action. In other words, even if you keep pressing “SPACE”, the bird won’t keep flapping.

## 3. Go Pipes

OK, now let’s add some pipes!

~~~ python
pipe_x = 300
pipe_y = 500
pipes = []
gap = 100
newpipe = False
Add these codes in the #2-Initialization.
~~~

`pipe_x` & `pipe_y` is the position of pipes. 
pipes is the container for pipe image and pipe’s position. 
gap is the gap between pipes. 
newpipe is the flag for adding a new pipe at the end of pipes.

Next, add these code before `#Key detection` or `#4-Main Function` (It’s the same.)

~~~ python
#Initialize pipes
i=1
for _ in range(5):
    pipes.append([pipe,(pipe_x+gap*i),pipe_y])
    i+=1
~~~

What it does is initialize pipes. 
In this game we define a pipe with three key elements: its image, its x position and its y position. 
In this section, the images of pipes are the same. But in real game, the pipes’ lengths are different. 
So we use different length pipe images to represent different length pipes. 
(We can also implement this function in another way: using length, x position, y position to define a pipe and create different length pipe images dynamically.)

Next, we draw pipes! Add these codes between `#draw elements` and `#update the screen` in `#4-Main Function`.

~~~ python
#pipe moving
    for onepipe in pipes:
        index = 0
        screen.blit(onepipe[0],(onepipe[1],onepipe[2]))
        onepipe[1]-=0.5
        if onepipe[1] < 100:
            pipes.pop(index)
            #set append new pipe flag
            newpipe = True
        index+=1
        endpipe = onepipe[1]
    #append a new pipe to the end of pipes
    if newpipe:
        pipes.append([pipe,endpipe+gap,pipe_y])
        newpipe = False
~~~

In this loop, all pipes are render to the screen. 
If one pipe’s x potion is too small(meaning it’s moving out of the left screen), 
pipes will kill this pipe and add a new pipe to the end of the list. 
Notice that pipes.pop is an action of queue instead of stack.

You may notice that the pipes are not real pipes. 
They are just some squares. We need to change these squares to pipes. 
There are a lot of different ways to do this as I have mentioned before. 
The way I did was to define a series of pipes with different length and use these pipes in random.

Here’s some code that you should add or change based on the previous code.

~~~ python
#Add
#1-Import library
import random

#Add & Change
#2-Initialization
GAP = 300
newpipe_l = False
newpipe_h = False

#Add
#3-Load images
pipe_2 = pygame.image.load("resources/image/pipe_2.png")
pipe_3 = pygame.image.load("resources/image/pipe_3.png")
pipe_4 = pygame.image.load("resources/image/pipe_4.png")
pipe_5 = pygame.image.load("resources/image/pipe_5.png")
pipe_6 = pygame.image.load("resources/image/pipe_6.png")

#Add
#Select pipe
def selectpipe(a):
    if a == 1:
        return pipe
    elif a == 2:
        return pipe_2
    elif a == 3:
        return pipe_3
    elif a == 4:
        return pipe_4
    elif a == 5:
        return pipe_5
    elif a == 6:
        return pipe_6
    else:
        return pipe

#Change
#Initialize pipes
i=1
for _ in range(5):
    pipe_l = random.randint(1,6)
    pipe_h = 7-pipe_l
    gap = random.randint(GAP,GAP*1.5)
    pipes_l.append([selectpipe(pipe_l),
           (pipe_x+gap*i),pipe_y_l-50*pipe_l])
    pipes_h.append([selectpipe(pipe_h),
           (pipe_x+gap*i),pipe_y_h])
    i+=1

#Change
    #pipe moving
    #the lower pipes
    for onepipe in pipes_l:
        index = 0
        screen.blit(onepipe[0],(onepipe[1],onepipe[2]))
        onepipe[1]-=0.5
        if onepipe[1] < 100:
            pipes_l.pop(index)
            #set append new pipe flag
            newpipe_l = True
        index+=1
        endpipe = onepipe[1]
    #append a new pipe to the end of pipes
    if newpipe_l:
        pipe_l = random.randint(1,6)
        pipe_h = 7-pipe_l
        gap = random.randint(GAP,GAP*1.5)
        pipes_l.append([selectpipe(pipe_l),
                endpipe+gap,pipe_y_l-50*pipe_l])
        newpipe_l = False
    #the higher pipes
    for onepipe in pipes_h:
        index = 0
        screen.blit(onepipe[0],(onepipe[1],onepipe[2]))
        onepipe[1]-=0.5
        if onepipe[1] < 100:
            pipes_h.pop(index)
            #set append new pipe flag
            newpipe_h = True
        index+=1
        endpipe = onepipe[1]
    #append a new pipe to the end of pipes
    if newpipe_h:
        pipes_h.append([selectpipe(pipe_h),endpipe+gap,pipe_y_h])
        newpipe_h = False
FlappyBirdCloneRandomPipes
~~~

If you run the code you’ll find that sometimes the pipes behavior is weird, 
two pipes will show together which is totally wrong. 
Try to fix it yourself or look at the final code which already fix this problem.

## 4. Collision

It’s very easy to add collision. 
We have a bird object, and several pipe objects. 
In every loop, just detect if bird run into pipes. 

~~~ python
#Collision
        #define bird's rectangle
        birdrect = pygame.Rect(bird.get_rect())
        birdrect.left = BIRD_X
        birdrect.top = bird_y
        #define pipes' rectangle
        for p in pipes_l:
            prect= pygame.Rect(p[0].get_rect())
            prect.left = p[1]
            prect.top = p[2]
            if prect.colliderect(birdrect):
                #stop the game
                pass
                running=0
                exitcode=1
            if (prect.left+50) < BIRD_X:
                score += 1
        for p in pipes_h:
            prect= pygame.Rect(p[0].get_rect())
            prect.left = p[1]
            prect.top = p[2]
            if prect.colliderect(birdrect):
                pass
                running=0
                exitcode=1
~~~

## 5. Count Scores

First, define score elements

~~~ python
#score
score = 0 #existing pipe's score count
previousscore = 0 #past pipe's score count

#set font
font = pygame.font.Font(None, 24)
scoretext = font.render(str(score+previousscore), True, (0,0,0))
textRect = scoretext.get_rect()
textRect.topleft=[20,20]
~~~

I set two parts of score: `previous score + current score = final score`. 
score automatically +1 for every past existing pipes. 
previous score counts how many “dead” pipes there are. 
The following codes are the changes relating to score.

~~~ python
#6-Initialize
    global previousscore

    #reset score
    previousscore = 0
        #append a new pipe to the end of pipes
            #count the score for the old pipe
            previousscore +=1
        #draw score
        scoretext = font.render(str(score+previousscore), True, (0,0,0))
        screen.blit(scoretext, textRect)
        #every loop reset score because score will +1 for
        #every past existing pipes every loop
        score = 0
        #define pipes' rectangle
            if (prect.left+50) < BIRD_X:
                score += 1
    #one time exitcode strategy
    if exitcode:
        exitfont = pygame.font.Font(None, 50)
        restartfont = pygame.font.Font(None, 30)
        exitText = exitfont.render(str(score+previousscore), True, (255,0,0))
~~~

## 6. Add Sound effects

The game is almost done. Last thing we need to do is adding some sound effect. There are several steps to do this:

- Load sound file
- Set sound volume
- Add play sound function at specific point

~~~python
#Step 1
flapSound = pygame.mixer.Sound("resources/audio/jump.wav")
#Step 2
flapSound.set_volume(0.1)
#Step 3
        #flap
        if key_firstPressed == True:
            flapSound.play()
~~~

## 7. .py to .exe

I found two ways to translate a `.py` file to `.exe` file. 
The first one is PyInstaller. 
Unfortunately I didn’t set up right to make it functional. 
Then I tried the second one: py2exe. I succeed in change a helloworld.py to a helloworld.exe. 
But I had trouble in compiling my pygame app to a standalone windows application. 
I got the err message like

> “can’t find module font.”

Then after some search, I found pygame2exe based on both py2exe and pygame. 
I implemented it but I got a new err message:

> “Microsoft Visual C++ Runtime Library Runtime Error! … This application has requested the Runtime to terminate in an unusual way. Please contact the application’s support team for more information.”

I did some other search and finally found a good solution to my problem: 
<http://stackoverflow.com/questions/12826093/pygame2exe-errors-that-i-cant-fix>

Instead set the font to None, i change it to

```
font = pygame.font.SysFont(“Arial”, 20)
```

But then there’s anothr err message like

> “can’t find resource/image/bird.png”

I thought the `.exe` should include everything (it’s 5MB!). 
But I seems to be wrong again. So I copy/parse the resourse folder in the same dir with `.exe` file.

Finally it worked!

:)

## 8. Source

<https://github.com/chennanni/flappy-bird-clone>
