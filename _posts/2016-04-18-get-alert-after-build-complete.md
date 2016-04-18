---
layout: post
title:  "Get Alert After Build Complete With Ant & Batch Script"
date:   2016-04-18
categories: ant, batch script
comments: true
---

## Intro
In my daily work, building a large project take times. I don't want to sit there doing nothing but staring at the build window. Usually I use this time to take a break or just do something else. It would be nice to have an alert when the build is complete. I implement this idea with ant & batch script.

Requirement is simple: at the end of the build, pop out a window saying "build is complete" and play a nice "ding" sound.

## Preparation
**Add an `ant task` to the build file**
``` xml
<target name="alert">
  <exec executable="[path to your batch script]\alert.bat"></exec>
  <echo>Task Complete</echo>
</target>
```

**Create a batch script `alert.bat`**
``` batch
:: hide all output messages
@echo off
echo off

:: play sound
set "file=[path to your sound file]\ding.mp3"
( echo Set Sound = CreateObject("WMPlayer.OCX.7"^)
  echo Sound.URL = "%file%"
  echo Sound.Controls.play
  echo do while Sound.currentmedia.duration = 0
  echo wscript.sleep 100
  echo loop
  echo wscript.sleep (int(Sound.currentmedia.duration^)+1^)*1000
) > sound.vbs
start /min sound.vbs

:: show message box
cscript [path to your vbscript file]\MessageBox.vbs > nul

:: delete the sound scipt
del sound.vbs
```

**Create a vbscript file `MessageBox.vbs`**
```
messageText = "build is complete"
MsgBox messageText, vbSystemModal
```

Last but not least, don't forget to prepare the sound file.

## Usage
How to use it? Add the `alert` task at the end of the other tasks. For example: `ant compile alert`.

## Improvement
But this setup has a problem: if the build task fail, the program will exit and the alert will not show.

I want to get an alert whenever the build is success or fail. So I think of using `<trycatch>` block to wrap the build task

Add a new `ant task`
``` xml
<target name="compile-alert">
  <trycatch>
    <try>
      <antcall target="compile"/>
    </try>

    <catch>
      <fail message="Something is wrong!"/>
    </catch>

    <finally>
      <antcall target="alert"/>
    </finally>
  </trycatch>
</target>
```

## Resources
- https://ant.apache.org/manual/Tasks/exec.html
- http://stackoverflow.com/questions/774175/show-a-popup-message-box-from-a-windows-batch-file
- http://stackoverflow.com/questions/11394432/create-vbscript-messagebox-that-stays-on-top-and-blocks-other-windows
- http://stackoverflow.com/questions/23313709/play-invisible-music-with-batch-file
- http://stackoverflow.com/questions/2044882/how-to-hide-batch-output
