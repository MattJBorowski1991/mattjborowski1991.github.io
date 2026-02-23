---
title: Home
layout: default
---

<div style="display:flex;gap:1.25rem;align-items:center;flex-wrap:wrap;">
  <div style="flex:0 0 160px;">
    <img src="/assets/profile.jpg" alt="Matt Borowski" style="width:160px;height:160px;object-fit:cover;border-radius:8px;" />
  </div>
  <div style="flex:1;min-width:220px;">

Hello visitor! Thanks for your interest.

I'm an MLE with focus on **CUDA** kernel profiling and optimization.

You can view my work below or on my [GitHub](https://github.com/MattJBorowski1991).

Beyond **CUDA** I have experience in machine learning as well as hyperscale data centers.

My Uni background is Applied Mathematics at [University of Oxford](https://www.maths.ox.ac.uk/) where I was advised by [Prof. Ben Hambly](https://www.maths.ox.ac.uk/).

Feel free to reach out!

  </div>
</div>

**Connect**: [LinkedIn](https://www.linkedin.com) • [GitHub](https://github.com/MattJBorowski1991) • [X](https://twitter.com) • <a href="mailto:mattjborowski@gmail.com">mattjborowski@gmail.com</a>

## Posts

<ul>
{% for post in site.posts %}
  <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a> — {{ post.date | date: "%b %d, %Y" }}</li>
{% endfor %}
</ul>
