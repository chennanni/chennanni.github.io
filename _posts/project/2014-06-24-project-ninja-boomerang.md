---
layout: post
title:  "Ninja Boomerang"
date:   2014-06-24
categories: [blog, game]
comments: true
---

This is my first game made with Construct 2, hosting in my Dropbox Server.

It CAN be played in this [webpage](https://dl.dropboxusercontent.com/u/77067427/Boomerang2/index.html). Hope you enjoy it! 

![ninja-boomerang](/source/img/ninja-boomerang.png)

## Gameplay

- You are playing as a Ninja throwing boomerangs to enermies.
- You can move around with WASD and throw boomerang with SPACE.
- You have three boomerangs at your disposal. After throwing out, it will bouce back for you to catch.
- Boomerang kills every enermy it hits which adds to more points and remaining time.
- Try to score as many points as you can in a limited time.

## Development Log

- find resources
- add background
- add ninja and monster
- set w,a,s,d for movement in keyboard
- add sword, set up and down behavior
- set sword max number
- set ninja catching sword behavior
- monster re-spawn
- monster collision with ninja
- sword collision with monster
- count score
- start game and restart game
- add more monsters
- add time
- add sound

## Bugs

Due to limited time, there’s some bugs that I don’t attend to fix them.

- After you die, when you leave the game’s webpage and go back, a dying sound will be played
- Monsters overlap each other because they can have different speeds
- Sometimes, at the top of the screen, monsters will appear and then disappear (because when they are created, they overlap with others and I destroy them)

## Thoughts

- What I can do better
  - design the game framework first, then try to follow it
  - always try to reset variables at one places (just for construct 2))
  - pre-load all sounds at the beginning of the layout or it can’t play well (just for construct 2))
  - monsters only move inside the layout, it’s important to figure out the relationship between camera display and layout (just for construct 2))
- Better game creation process
  - find resources(images, sounds, videos)
  - define sprites(kinds, behaviors)
  - implementation
  - score system
  
## Resources

- Picture: <http://www.easyicon.net>
- Music: <http://tieba.baidu.com/p/2347705369>
- Sound: <https://www.freesound.org>
- Tiled background: <http://tiled-bg.blogspot.com>
- Tutorial
  - [Beginner’s guide to Construct 2](https://www.scirra.com/tutorials/37/beginners-guide-to-construct-2)
  - [How to embed a Construct 2 project into your website](https://www.scirra.com/tutorials/306/how-to-embed-a-construct-2-project-into-your-website)
  - [Upload your game to Dropbox](https://www.scirra.com/tutorials/42/upload-your-game-to-dropbox)
  - [Tips on publishing HTML5 games to the web](https://www.scirra.com/tutorials/655/tips-on-publishing-html5-games-to-the-web)
