---
title: "SH2:EE Master Volume feature"
excerpt: "Master Volume slider for Silent Hill 2: Enhanced Edition."
header:
  image: /assets/images/volume_header.png
  teaser: assets/images/volume_teaser.png
sidebar:
  - title: "Role"
    #image: http://placehold.it/350x250
    image_alt: "logo"
    text: "Developer"
  - title: "Responsibilities"
    text: "Reverse engineering, developing"
gallery:
  - url: /assets/images/volume1.png
    image_path: assets/images/volume1.png
    alt: "The original sh2 options menu"
  - url: /assets/images/volume2.png
    image_path: assets/images/volume2.png
    alt: "Analyzing polygons in PIX"
  - url: /assets/images/mastv.png
    image_path: assets/images/mastv.png
    alt: "The completed feature"
---

The Speaker Config option in Silent Hill 2 became obsolete after Windows XP, since the OS doesn’t let third party apps change that audio configuration any more.
We changed that option to emulate the other volume sliders, adding a new “Master Volume” option.

{% include gallery caption="Screenshots taken while developing the feature." %}

This involved reverse engineering vertex data from the game, drawing those graphics at the right time, handling resolution changes by scaling/translating the vertices and setting the color based on changed/unchanged option.

The right arrow that took input from the mouse was moved to the right, and the function that checks for cursor hitboxes was modified to accommodate the new arrow position. Keyboard interaction was detected by redirecting execution where the game already checked for them.
The game’s own check for changed options was used, swapping addresses for my new variables, as was the code to confirm/discard changes. These last features were handled directly in ASM.