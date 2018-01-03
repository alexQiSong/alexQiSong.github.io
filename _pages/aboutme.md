---
permalink: /aboutme/
# page layout
defaults:
  # _pages
  - scope:
      path: ""
      type: pages
    values:
      layout: splash
      author_profile: true
# page content
feature_row:
  - image_path: assets/images/gbcb_logo1_cropped.jpg
    alt: "placeholder image 1"
    title: "Placeholder 1"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
  - image_path: /assets/images/gbcb_logo3_cropped.jpg
    alt: "placeholder image 2"
    title: "Placeholder 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting.
  - image_path: /assets/images/sketching.jpg
    alt: "placeholder image 3"
    title: "Placeholder 3"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}
