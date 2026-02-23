---
title: Home
layout: default
---

<div markdown="1" style="display:flex;gap:1.25rem;align-items:center;flex-wrap:wrap;">
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

<p>
  <strong>Connect</strong>
  <span style="margin-left:.5rem;display:inline-flex;gap:.5rem;align-items:center">
    <a href="https://www.linkedin.com/in/matt-borowski-780aab87/" aria-label="LinkedIn" style="color:inherit;text-decoration:none"> 
      <!-- LinkedIn -->
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="24" height="24" rx="4" fill="#0A66C2"/><path d="M6.94 9.25H9.06V18H6.94zM8 5.5a1.26 1.26 0 110 2.52 1.26 1.26 0 010-2.52zM11.5 9.25h2.03v1.18h.03c.28-.53.96-1.09 1.98-1.09 2.12 0 2.51 1.4 2.51 3.22V18H17v-4.02c0-.96-.02-2.2-1.34-2.2-1.34 0-1.55 1.05-1.55 2.13V18h-2.64z" fill="#fff"/></svg>
    </a>
    <a href="https://github.com/MattJBorowski1991" aria-label="GitHub" style="color:inherit;text-decoration:none">
      <!-- GitHub -->
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="24" height="24" rx="4" fill="#333"/><path d="M12 2C7.58 2 4 5.58 4 10c0 3.54 2.29 6.54 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.69-.01-1.35-2.23.49-2.7-1.07-2.7-1.07-.36-.92-.88-1.17-.88-1.17-.72-.49.05-.48.05-.48.8.06 1.22.82 1.22.82.71 1.21 1.86.86 2.32.66.07-.52.28-.86.5-1.06-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.01.08-2.11 0 0 .67-.22 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.91.08 2.11.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.94-.01 2.2 0 .21.15.46.55.38C17.71 16.54 20 13.54 20 10c0-4.42-3.58-8-8-8z" fill="#fff"/></svg>
    </a>
    <a href="https://x.com/MattJBorowski" aria-label="X (Twitter)" style="color:inherit;text-decoration:none">
      <!-- X / Twitter -->
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="24" height="24" rx="4" fill="#1DA1F2"/><path d="M19 7.5c-.55.25-1.14.41-1.76.48.63-.38 1.1-.98 1.32-1.7-.59.35-1.25.6-1.95.74A3.44 3.44 0 0012.5 9c0 .28.03.56.09.83-2.86-.14-5.4-1.51-7.1-3.6-.3.52-.47 1.12-.47 1.76 0 1.21.62 2.28 1.57 2.9-.5-.02-.97-.16-1.38-.38v.04c0 1.7 1.21 3.12 2.82 3.45-.29.08-.6.12-.92.12-.22 0-.44-.02-.65-.06.44 1.38 1.71 2.39 3.22 2.42A6.9 6.9 0 015 18.54c1.99 1.28 4.35 2.03 6.89 2.03 8.27 0 12.8-6.86 12.8-12.81 0-.2 0-.41-.01-.61A9.18 9.18 0 0020 8.54c.39-.28.72-.62 1-1.01-.36.16-.74.27-1.14.32z" fill="#fff"/></svg>
    </a>
    <a href="mailto:mattjborowski@gmail.com" aria-label="Email" style="color:inherit;text-decoration:none">
      <!-- Email -->
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="24" height="24" rx="4" fill="#6b7280"/><path d="M4 7.5v9A2.5 2.5 0 006.5 19h11a2.5 2.5 0 002.5-2.5v-9A2.5 2.5 0 0017.5 5h-11A2.5 2.5 0 004 7.5zm2.2-.2L12 12.1l5.8-4.8" stroke="#fff" stroke-width="1.2" fill="none" stroke-linecap="round" stroke-linejoin="round"/></svg>
    </a>
  </span>
</p>

## Posts

<ul>
{% for post in site.posts %}
  <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a> — {{ post.date | date: "%b %d, %Y" }}</li>
{% endfor %}
</ul>
