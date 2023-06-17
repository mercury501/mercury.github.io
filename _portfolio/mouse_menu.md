---
title: "SH2:EE Menu Cursor"
excerpt: "Enabled mouse functionality in menus for Silent Hill 2: Enhanced Edition."
header:
  image: /assets/images/cursor_header.png
  teaser: assets/images/cursor_teaser.png
sidebar:
  - title: "Role"
    #image: http://placehold.it/350x250
    image_alt: "logo"
    text: "Developer"
  - title: "Responsibilities"
    text: "Reverse engineering, developing"
gallery:
  - url: /assets/images/cursor1.png
    image_path: assets/images/cursor1.png
    alt: "Visualizing the hitboxes in game"
  - url: /assets/images/cursor2.png
    image_path: assets/images/cursor2.png
    alt: "Great concept design from Polymega"
  #- url: /assets/images/unsplash-gallery-image-3.jpg
  #  image_path: assets/images/unsplash-gallery-image-3-th.jpg
  #  alt: "placeholder image 3"
---

I started reverse engineering Silent Hill 2’s cursor because of an [issue](https://github.com/elishacloud/Silent-Hill-2-Enhancements/issues/750) that asked for a way to hide the cursor while using a controller.
Through research, I came up with a clever solution for this request, which was to auto-hide the mouse after a few seconds of inactivity. That way, both controller and keyboard users could benefit from this feature if they desired to use it. Using the knowledge and insight I gained from developing this auto-hide mouse feature, it came natural to also try and restore the use of the mouse in menus that inherently lacked it; namely the pause screen and the memo list screen.

{% include gallery caption="Screenshots taken while developing the feature." %}

To handle mouse interaction, I wrote a custom solution accounting for changes in game resolution, and handled interactions with the game by identifying the addresses for various indexes and updating them accordingly.