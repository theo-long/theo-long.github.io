---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

I'm a PhD student at Yale University in the Computer Science Department, advised by [Alex Lew](https://alexlew.net).

My interests are in designing tools and algorithms to improve, automate, and scale probabilistic reasoning. My research aims to develop ways to represent and compute with uncertainty in a principled way.

A non-exhaustive list of questions I'm thinking about:
- How can we detect 'good' and 'bad' MCMC chains?
- What's the best way to combine discrete and continuous variables in Bayesian inference?
- How can we parallelize inherently sequential inference algorithms?
- What are composable and intuitive priors for 2D and 3D shapes?

## Recent Posts
{% assign drafts = site.drafts | default: "" | split: "" %}
{% assign all_posts = site.posts | concat: drafts | sort: "date" | reverse %}

<ul>
  {% for post in all_posts limit:10 %}
    <li>
      <span style="color: #666;">{{ post.date | date: "%b %d, %Y" }}</span> 
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% if post.path contains '_drafts/' %}
        <strong style="color: #d9534f;"> [DRAFT]</strong>
      {% endif %}
    </li>
  {% endfor %}
</ul>