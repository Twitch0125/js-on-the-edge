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
or maybe the "Edge" is JS?

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: statement
---
# Remember "Lambdas" or Serverless Functions?
<v-click>
<h3> 
Its garbage now, get it out of here
</h3>
</v-click>

---
layout: center
---

## "Traditional" serverless computing 
<v-clicks>

- AWS Lambda Edge. Uses Lambda and Cloudfront CDN
  
- GCP Cloud Run

- running your code on distributed docker containers or vms

**They all suffer from one common problem**
</v-clicks>


---
layout: statement
---

# Cold Starts
The time spent to start your app.

<!--
Cold starts are kind of an inherent problem with Serverless. You wanna stop something to save resources but you also want it to act as if it was never off.
-->

---
layout: default
---
## Why are cold starts a thing?
<v-click>

the lifecycle for a container looks something like this
</v-click>

<v-clicks>

1. create your container <span style="opacity: 0.4"> adds cold start time </span>
2. app starts up in the container  <span style="opacity: 0.4"> adds cold start time </span>
3. app handles request or whatever until it stops receiving them
4. container stops after some time of inactivity 
</v-clicks>

<v-clicks>

You want these steps to be as short as possible. That creates a better user experience and is cheaper for you.

They can take anywhere from about 700 ms to a few seconds.

1 and 2 is where there's a lot of overhead from containers. You could improve the cold start times by foregoing containers, but we like containers!

How can this be improved?

</v-clicks>

---
layout: statement
---
# v8 Isolates and Web Assembly
Its been under our noses this whole time...
well, since 2017-ish

---
layout: default
---
# v8 Isolates
This is how v8 "sandboxes" javascript.

---
layout: default
---
# Web Assembly
compile target that can run in a web environment (v8)


---
layout: center
---
# The Big Players
## Cloudflare Workers
- powers Vercel Edge Functions
## Deno Deploy
- powers Netlify Edge Functions, Supabase Edge Functions

---
layout: center
---
# Limitations
The smaller the better
## Cloudflare Workers
- Maximum app size of 1 MB
- Nodejs APIS not available. Cloudflare specific runtime based on web standards
- max 128MiB memory usage
## Deno Deploy
- Maximum app size of 20 MB
- max 512MiB memory usage
- NodeJS APIs not available (cuz its deno...)

---
layout: center
---
# Your Databases should also be distributed!
Your database is still your bottleneck
---
layout: center
---

# Workarounds and Tips
- Per Route Configuration. Only push some routes to the edge (Next.js makes this easy)
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
- Astro


# Benchmarking
Nuxt 3 Vercel - 141ms avg response time
