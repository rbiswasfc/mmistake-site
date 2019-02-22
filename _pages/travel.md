---
title: "Travel"
layout: archive
permalink: /travel/
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
<img src="{{ site.url }}{{ site.baseurl }}/images/raja_3.JPG" alt="my caption">

Some math eq:



{% capture written_label %}'None'{% endcapture %}

{% for collection in site.collections %}
  {% unless collection.output == false or collection.label == "posts" %}
    {% capture label %}{{ collection.label }}{% endcapture %}
    {% if label != written_label %}
      <h2 id="{{ label | slugify }}" class="archive__subtitle">{{ label }}</h2>
      {% capture written_label %}{{ label }}{% endcapture %}
    {% endif %}
  {% endunless %}
  {% for post in collection.docs %}
    {% unless collection.output == false or collection.label == "posts" %}
      {% include archive-single.html %}
    {% endunless %}
  {% endfor %}
{% endfor %}
