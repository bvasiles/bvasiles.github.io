---
layout: page
title: Publications
---

## Journal articles
{% bibliography --query @article[pubcls=0] %}

## Conference papers (with peer-reviewed proceedings)
{% bibliography --query @inproceedings[pubcls=2 || pubcls=1] %}

## Others
{% bibliography --query @article[pubcls=3] @inproceedings[pubcls=3] %}

## Theses
{% bibliography --query @mastersthesis @phdthesis %}

