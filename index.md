---
title: Home
layout: default
---

<div style="display:flex;gap:1.25rem;align-items:center;flex-wrap:wrap;">
  <div style="flex:0 0 264px;">
    <img src="/assets/profile.jpg" alt="Matt Borowski" style="width:264px;height:264px;object-fit:cover;border-radius:8px;" />
  </div>
  <div style="flex:1;min-width:220px;">

<p>Hi there! Thanks for your interest.</p>

<p>My name is Matt and I'm an MLE with focus on <strong>CUDA</strong> kernel profiling and optimization.</p>

<p>You can view my work below or on my <a href="https://github.com/MattJBorowski1991">GitHub</a>.</p>

<p>Beyond CUDA I have experience in ML / deep learning and data centers.</p>

<p>My university background is Applied Mathematics at <a href="https://www.maths.ox.ac.uk/">University of Oxford</a> where I was advised by <a href="https://people.maths.ox.ac.uk/hambly/">Prof. Ben Hambly</a>.</p>

<p>Feel free to reach out!</p>

  </div>
</div>

<p>
  <span class="social-list" style="margin-left:.5rem;">
    <a class="social linkedin" href="https://www.linkedin.com/in/matt-borowski-780aab87/" aria-label="LinkedIn"> 
      <img src="https://www.linkedin.com/favicon.ico" alt="LinkedIn" />
    </a>
    <a class="social github" href="https://github.com/MattJBorowski1991" aria-label="GitHub">
      <img src="https://github.com/favicon.ico" alt="GitHub" />
    </a>
    <a class="social x" href="https://x.com/MattJBorowski" aria-label="X (Twitter)">
      <img src="https://x.com/favicon.ico" alt="X" />
    </a>
    <a class="social email" href="mailto:mattjborowski@gmail.com" aria-label="Email">
      <!-- Email -->
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="24" height="24" rx="4" fill="#6b7280"/><path d="M4 7.5v9A2.5 2.5 0 006.5 19h11a2.5 2.5 0 002.5-2.5v-9A2.5 2.5 0 0017.5 5h-11A2.5 2.5 0 004 7.5zm2.2-.2L12 12.1l5.8-4.8" stroke="#fff" stroke-width="1.2" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </a>
    <a class="social feed" href="/atom.xml" aria-label="Feed">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="24" height="24" rx="4" fill="#ff6600"/><path d="M6 17a1.5 1.5 0 100 3 1.5 1.5 0 000-3zM4 11c6 0 9 3 9 9" stroke="#fff" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </a>
  </span>
</p>

## Some of my work

{% for post in site.posts %}
<article style="margin-bottom:1.15rem;padding-bottom:1rem;border-bottom:1px solid #eee;">
  <h3 style="margin:.2rem 0 .35rem 0"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
  <div style="color:#6b7280;font-size:0.9rem;margin-bottom:.5rem">{{ post.date | date: "%b %d, %Y" }}</div>
  <p>{% if post.summary %}{{ post.summary }}{% else %}{{ post.excerpt | strip_html | truncatewords:40 }}{% endif %}</p>
  <p><a href="{{ post.url | relative_url }}">Read more →</a></p>
</article>
{% endfor %}
