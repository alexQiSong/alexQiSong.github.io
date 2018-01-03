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
excerpt: "Bacon ipsum dolor sit amet salami ham hock ham, hamburger corned beef short ribs kielbasa biltong t-bone drumstick tri-tip tail sirloin pork chop."
intro: 
  - excerpt: 'Nullam suscipit et nam, tellus velit pellentesque at malesuada, enim eaque. Quis nulla, netus tempor in diam gravida tincidunt, *proin faucibus* voluptate felis id sollicitudin. Centered with `type="center"`'
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}
