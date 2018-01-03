---
permalink: /aboutme/
defaults:
  # _pages
  - scope:
      path: ""
      type: pages
    values:
      layout: home
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
excerpt: "Bacon ipsum dolor sit amet salami ham hock ham, hamburger corned beef short ribs kielbasa biltong t-bone drumstick tri-tip tail sirloin pork chop."
intro: 
  - excerpt: "I am a Phd student enrolled in Genetics, Bioinformatics and Computational Biology (GBCB) program at Vriginia Tech."
---

{% include feature_row id="intro" type="right" %}

{% include feature_row %}
