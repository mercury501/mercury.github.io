---
title: "SH2:EE Overlays"
excerpt: "Text Overlays for Silent Hill 2: Enhanced Edition."
header:
  image: /assets/images/overlay_header.png
  teaser: assets/images/ovl.png
sidebar:
  - title: "Role"
    #image: http://placehold.it/350x250
    image_alt: "logo"
    text: "Developer"
  - title: "Responsibilities"
    text: "Reverse engineering, developing"
gallery:
  - url: /assets/images/overlay_poc.png
    image_path: assets/images/overlay_poc.png
    alt: "Early POC for the feature"
  - url: /assets/images/overlay_start.png
    image_path: assets/images/overlay_start.png
    alt: "Testing the overlay"
  - url: /assets/images/overlays.png
    image_path: assets/images/overlays.png
    alt: "Both overlays"
---

{% include gallery caption="Screenshots taken while developing the feature." %}


The feature consisted in a debug overlay to show various information about the player, the game’s event indexes and our own added features, and also an information overlay to show relevant values towards 100% completion.
The implementation consisted of creating fonts using the DirectX8 API and drawing text on the screen, at the end of the frame drawing pipeline.