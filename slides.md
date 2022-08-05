---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS (experimental)
css: unocss
---

# JS On the "Edge"
What is it and how do I use it

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: statement
---
# Remember "Lambdas" or Serverless Functions?
Basically just that.

but its *cool* again
<!-- you're running code on someone else's computer, but they're gonna bill you on how long your code takes to run.
But they're just down the street from your customers! -->

---
layout: center
---
# Think of it as a CDN for your code
Instead of static HTML, you're running your code and delivering the output.

---
layout: center
---

## Cloudflare Workers
- v8 Isolates. No Cold Starts (not Node.js)
- powers Vercel Edge Functions
## Deno Deploy
- Deno and v8. Optimized for minimal cold starts
- powers Netlify Edge Functions, Supabase Edge Functions

## Docker MicroVMs
- Fly.io w/ Firecracker
- auto-scaling, minimum 1 instance to prevent cold starts

---
layout: statement
---
# Why the edge?

---
layout: center
---

# A Few Reasons
## SSR
- running your code closer to your customers will reduce loading times
- deploys almost like you're using a CDN (easy deployment)
## Cost
- Its cheaper! Kinda.
- Zero Maintenance. Kinda.

<!-- If your app works fine on a few ec2 instances then it may not be cheaper. But if you have customers
all over the world, then this might be good. There's even SQLite on the edge with Fly.io and lightstream, which  is
basically a sqlite database replicated across various S3 buckets -->

---
layout: center
---
# Limitations
Each platform is different. In general, the smaller the better.
## Cloudflare Workers
Vercel Edge
- Maximum app size of 1 MB
- Nodejs APIS not available. Cloudflare specific runtime
- max 128MiB memory usage
## Deno Deploy
Netlify Edge
- Maximum app size of 20 MB
- max 512MiB memory usage
- NodeJS APIs not available (cuz its deno...)
---
layout: center
---

# Workarounds and Tips
- Per Route Configuration. Only push some routes to the edge
- Lightweight Web Standards compliant Server (Nuxt/Nitro)

---
layout: center
---
# How do I get started?
Popular Frameworks
- Fresh (deno)
- Nuxt 3 (vue)
- Remix
- SvelteKit
- Next.js