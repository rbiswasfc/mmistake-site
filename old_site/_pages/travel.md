---
title: "Travel"
layout: archive
permalink: /travel/
entries_layout: grid
mathjax: true
header:
    overlay_image: "/images/japan_autumn.jpg"
---

This page documents my experiences during my travel adventures. Places I have visited...

# H1 heading
## H2 heading
### H3 Heading


*italics text*
**bold**

Embed a link: [link](http://google.com)

List of elements
* Item 1
* item 2

Python code block:
```python
    import numpy as np
    Z = np.zeros(shape=(3,1))
    def f(x,y):
      return x+y
```

Here is an image:
<img src="{{ site.url }}{{ site.baseurl }}/images/japan/IMG_8622.jpg" alt="Japan Alps">

Some math eq: $$ x^2 = 4$$



{% capture written_label %}'None'{% endcapture %}
{% for collection in site.collections %}
  {% unless collection.output == false or collection.label == "posts" %}
    {% capture label %}{{ collection.label }}{% endcapture %}
    {% if label != written_label %}
      {% capture written_label %}{{ label }}{% endcapture %}
    {% endif %}
  {% endunless %}
  {% for post in collection.docs %}
    {% unless collection.output == false or collection.label == "posts" %}
      {% include archive-single.html %}
    {% endunless %}
  {% endfor %}
{% endfor %}

{% if page.mathjax %}
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
{% endif %}
