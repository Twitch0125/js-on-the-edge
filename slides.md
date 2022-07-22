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
layout: statement
---
# Why the edge?

---
layout: center
---
# A Few Reasons
## SSR
- running your code closer to your customers will reduce loading times
- deploys almost like deploying on a CDN (easy deployment)
## Cost
- Its cheaper! Kinda.

<!-- If your app works fine on a few ec2 instances then it may not be cheaper. But if you have customers
all over the world, then this might be good. There's even SQLite on the edge with Fly.io and lightstream, which  is
basically a sqlite database replicated across various S3 buckets -->

---
layout: quote
---
# "History doesn't repeat itself, but it rhymes"
someone smart

<!-- Back in the day everything was server rendered. Then SPAs were the new hotness and nothing was server side rendered. Now we're going back to server side rendering but on a bunch of really small servers. -->