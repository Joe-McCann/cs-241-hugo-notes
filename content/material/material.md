---
# Discussion on how this block works found here
# https://wowchemy.com/blocks/portfolio/
widget: portfolio
weight: 100
title: Topics
subtitle:
content:
  filters:
    folders:
      - course
    kinds:
      - section
    exclude_tags:
      - preface

  filter_button:
    - name: All Material
      tag: '.js-id-cs241, .js-id-previous'
    - name: CS 241
      tag: cs241

  filter_default: 0
  # Sort by page weight
  sort_by: 'Weight'
  sort_ascending: true
design:
  columns: '1'
  view: masonry
  flip_alt_rows: false
---