---
title: "SH2:EE Control Options Overhaul"
excerpt: "Replacing text for controller button descriptions with icons for Silent Hill 2: Enhanced Edition."
header:
  image: /assets/images/control_options_header.png
  teaser: assets/images/control_options_teaser.png
sidebar:
  - title: "Role"
    #image: http://placehold.it/350x250
    image_alt: "logo"
    text: "Developer"
  - title: "Responsibilities"
    text: "Reverse engineering, development"
gallery:
  - url: /assets/images/control_options1.png
    image_path: assets/images/control_options1.png
    alt: "The original sh2 options menu"
  - url: /assets/images/control_options2.png
    image_path: assets/images/control_options2.png
    alt: "The revamped control options menu, featuring icons"
  - url: /assets/images/control_options3.png
    image_path: assets/images/control_options3.png
    alt: "The icons change color, as the vanilla text did"
---

Silent Hill 2, like many games of its era, displayed controller buttons just by referring to their button number. This number means very little to players, so we replaced the text with icons, for clarity and aesthetics.

{% include gallery caption="Screenshots taken while developing the feature." %}

This involved replacing the text with a custom grid of quads, and mapping them to a texture using the right uv. While changing the button binding, or when selecting a binding, the color is altered through a simple shader.