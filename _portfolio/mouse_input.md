---
title: "SH2:EE Mouse Input Overhaul"
excerpt: "Reworking the mouse input handling in Silent Hill 2: Enhanced Edition."
header:
  image: /assets/images/input_header.png
  teaser: assets/images/input_teaser.png
sidebar:
  - title: "Role"
    #image: http://placehold.it/350x250
    image_alt: "logo"
    text: "Developer"
  - title: "Responsibilities"
    text: "Reverse engineering, developing"
#gallery:
#  - url: /assets/images/overlay_poc.png
#    image_path: assets/images/overlay_poc.png
#    alt: "placeholder image 1"
#  - url: /assets/images/overlay_start.png
#    image_path: assets/images/overlay_start.png
#    alt: "placeholder image 2"
  #- url: /assets/images/unsplash-gallery-image-3.jpg
  #  image_path: assets/images/unsplash-gallery-image-3-th.jpg
  #  alt: "placeholder image 3"
---

Traditionally, Silent Hill 2’s utilization of the mouse was limited to a couple menus and the title screen, so I reverse engineered the game’s own input handling and translated DirectInput data coming from the mouse to emulate an analog stick. I’ve also implemented some QOL features, such as using mouse buttons to fire/aim, a toggle run feature and, in time, the keyboard input filtering feature came in handy to solve some bugs related to the 60 FPS feature.

This involved lots of testing and fixing behaviours all around the game.