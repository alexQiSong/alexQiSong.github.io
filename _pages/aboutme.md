---
permalink: /aboutme/
defaults:
  # _pages
  - scope:
      path: ""
      type: pages
    values:
      layout: splash
      author_profile: true
feature_row:
  - image_path: assets/images/gbcb_logo1_cropped.jpg
    alt: "placeholder image 1"
    title: "GBCB logo version 1"
  - image_path: /assets/images/gbcb_logo3_cropped.jpg
    alt: "placeholder image 2"
    title: "GBCB logo version 2"
  - image_path: /assets/images/sketching.jpg
    alt: "placeholder image 3"
    title: "My sketching"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}
