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

I started reverse engineering Silent Hill 2’s cursor because of an issue (link) that asked for a way to hide the cursor while using a controller.
Since I learned how to hide the cursor, after a few seconds of inactivity, it came natural to try and enable it in menus that lacked it, namely the pause screen and the memo list screen.

{% include gallery caption="Screenshots taken while developing the feature." %}

To handle mouse interaction, I wrote a custom solution accounting for changes in game resolution, and handled interactions with the game by identifying the addresses for various indexes and updating them accordingly.