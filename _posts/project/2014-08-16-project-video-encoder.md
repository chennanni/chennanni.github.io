---
layout: post
title:  "Video Encoder"
date:   2014-08-16
categories: [project, image]
comments: true
---

## Goal

We have implemented an encoder of images into video which contains both command line and GUI versions. 
It can encoded up to 100 JPEG image files into one file with given quality (0~1) and three interesting video effects are provided during the viewing.

![video-encoder-requirement](/source/img/video-encoder-requirement.png)

## Features

- **Ability to encode up to 100 JPEG images (of the same dimensions) into one encoded file in under 5 minutes.** The file size should never be more than the sum of the individual files.
  - Compress 100 JPEG images into smaller ones with acceptable quality by com.sun.image.codec.jpeg.JPEGImageEncoder.
  - Delete half of total frames.
  - Encode remaning frames into one file.
- **Ability to play back the images from the encoded file at least 10 images per second.**
  - A clickable green process bar under the video which indicates the playing.
  - The video player will go back to where the user clicks on the process bar and continue playing from that frame.
- **Provide a Command-line User Interface for encoding and viewing.**
- **Provide a Graphic User Interface for encoding and viewing.**: Two main menu: Encoder Video & Play Video.
- **Three “interesting” real-time video effects: Inverse, ToGray, Multi-size.**
  - Inverse: Set the RGB value of each pixel from x to 255-x.
  - ToGray: Set the color space from RGB to Gray of each JPEG image by java.awt.image.ColorConvertOp.filter.
  - Multi-size: Set the video played in its original images size or the player’s default size.
- **Adjust parameters on movie**: JPEGImageEncoder provides a quality parameter to compress JPEG images which includes “high quality”, “medium quality”, “low quality”, “very low quality”.

## Design

![video-encoder-design](/source/img/video-encoder-design.png)

## Analysis

Asymptotic Complexity

- Read Images O(n), n is the num of images
- Delete frames O(nm), m=ab, a&b is the height and width of the image
- Compress images, O(nm)
- Add frames, O(nm)
- Caculate Video effect, O(n)
- Finally: O(nm)

Data Structure

- Array
- ArrayList<BufferedImage>

## Results

Command line Interface

![video-encoder-cmd](/source/img/video-encoder-cmd.png)

Graphic User Interface

![video-encoder-gui](/source/img/video-encoder-gui.png)

Playing Encoded Video with a Gray Filter

![video-encoder-play](/source/img/video-encoder-play.png)

## Running Instruction

To run the application successfully:

- Import the code package ec504, including sub-packages ec504.util, ec504.GUI, ec504.video and ec504.image.
- Run the code in the console.
- Follow the commandline user interface for encoding and playing videos.

## Project Structure

- default package:
  - `VideoEncoder`: the main entrance of the application, default control with command line
- GUI package:
  - `ImageCovertorMenu`: the image covert menu, used to encode video
  - `VideoPlayerMenu`: the image player menu, used to play video
  - `MainMenu`: the main menu of GUI
- image package:
  - `ImageCompressor`: image compressor interface
  - `ImgCmp`: image compressor
  - `ImgCvt`: change image RGB parameter
  - `InnerCmp`: inner frame deletion and addition
- util package:
  - `DecodeException`: decode exception
  - `EncodeException`:encode exception
  - `Helper`: implement function like ‘loadImages’, ‘mergeImages’, ‘sizeImages’, ‘writeFile’, ‘readFile’, ‘encodeFile’, ‘decodeFile’, ‘renameImages’
- video package:
  - `Board`: the painting board to display images
  - `Images2Video`: Render images to Screen using interface VideoPlayer
  - `ResizeImages`: Used to Resize the images to fit the screen
  - `VideoPlayer`: Interface for Images2Video
  - `YesBar`: Process bar
  
## Library

- JRE7.0
- Note: The API com.sun.image.codec.jpeg is restricted by eclipse. Find “Forbidden references(access rules)” in “Windows”-“Preferences”-“Java”-“Compiler”-“Errors/Warnings”-“Deprecated and restricted API” and set it to “Warning” to run this program.

## References

- JPEG (ISO/IEC JTC l), Digital Compression and Coding of Continuous-tone Still Images, ISO/IEC DIS 10918-1, Feb. 1992.
- MPEG (ISO/IEC JTCl/SC29/WGI l), Coding of Moving Pictures and Associated Audio for Digital Storage Media at up to about 1.5 Mbit/s, IS0 CD 11 172, Nov. 1991.
- J.L. Wu, S.J. Huang, Y.M. Huang, C.T. Hsu, and J.Shiu, “An efficient JPEG to MPEG-1 transcoding algorithm”, IEEE Trans. Consumer Electron., Vol.42, Issue: 3, pp.447-457, Aug. 1996.
- Gregory K. Wallace, The JPEG still picture compression standard, Communications of the ACM, v.34 n.4, p.30-44, April 1991.
- Java The Complete Reference, 8th Edition
- [JPEGImageEncoder Interface](<http://courses.cs.washington.edu/courses/cse341/98au/java/jdk1.2beta4/docs/api/com/sun/image/codec/jpeg/JPEGImageEncoder.html>)

## Resouces

- <http://jcodec.org>
- <http://www.macs.hw.ac.uk/cs/java-swing-guidebook>
- <http://docs.oracle.com/javase/tutorial/uiswing/components/filechooser.html>
- <http://docs.oracle.com/javase/tutorial/uiswing/components/frame.html>
- <http://www.randelshofer.ch/monte>
- <http://www.randelshofer.ch/blog/2008/08/writing-avi-videos-in-pure-java>
- <http://javagraphics.blogspot.com/2008/06/movies-writing-mov-files-without.html>

