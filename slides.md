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
fonts:
  provider: 'none'
  sans: 'Satoshi'
---

# JS On the "Edge"
or maybe the "Edge" is JS?

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->


---
layout: center
---
# What is the edge?
Serverless Edge Computing

Running your code on someone else's computer and only for the amount of time it needs to and as close as possible to users

---
layout: center
---

# "Traditional" serverless computing 
<v-clicks>

- AWS Lambda Edge. Uses Lambda and Cloudfront CDN
  
- GCP Cloud Run

- running your code on distributed docker containers or vms

**They all suffer from one common problem**
</v-clicks>

<!--
for the past couple years if you wanted to do Serverless you'd usually create a container and ship it to some provider. Or you could send providers functions that are ran in their runtimes.

They all come with one caveat though.
-->

---
layout: fact
---

# Cold Starts
The time spent to start your app.

---
layout: center
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

1 and 2 is where there's a lot of overhead from containers. You could improve the cold start times by foregoing containers, but we like containers!

How can this be improved? How can we keep the benefits of containers?

</v-clicks>

<!--
Cold starts are an inherent problem with Serverless. You wanna stop something to save resources but then you have to turn it back on. Turning it on takes time.

Cold starts can last anywhere from about 500ms to 2 seconds. Depending on your runtime.

Of course you don't *have* to use containers, but they're just super useful. It tends to be quicker to run directly in a runtime without a container.
-->

---
layout: statement
---
Introducing...

<v-clicks>

# Javascript Containers
v8 Isolates and Web Assembly

</v-clicks>

---
layout: statement
---

# v8 Isolates
This is how v8 "sandboxes" javascript.

---
layout: center
---

# Whats special about v8 Isolates?

<v-clicks>

- A runtime can run thousands of Isolates at once
- fast startup
- Each Isolate's memory is isolated which protects it from other isolates on the runtime
- Rather than paying the overhead of starting a container every time, you pay overhead *once* when starting v8
- v8 runs continuously

<div class="mt-6">

> Any given isolate can start around a hundred times faster than a Node process on a container or virtual machine. Notably, on startup isolates consume an order of magnitude less memory. — [**Cloudflare Workers Documentation**](https://developers.cloudflare.com/workers/learning/how-workers-works/#isolates)

</div>

</v-clicks>
<!--
using v8 Isolates you can drastically reduce the overhead from spinning up the runtime environment.
-->


---
layout: center
---

Lets Review
<v-clicks>

# dockerd -> v8 runtime
v8 is starting and stopping our "containers"

# docker run my-container -> v8 isolates
our "containers" are v8 isolates

</v-clicks>

<!--
I may be getting something totally wrong here, but based on how I understand docker and use it these are the comparisons I'm making.
-->

---
layout: statement
---
<h1> docker build -> <v-click> <span> Web Assembly </span> </v-click> </h1>

<v-clicks>

our "images" are Web Assembly

</v-clicks>


---
layout: statement
---

# Web Assembly
compile target that can run in a web environment (v8)

<!--
Web Assembly is a common compile target. Instead of building images that bundle all of their application's dependencies, they get compiled down to web assembly that can be run by v8
-->

---
layout: center
---
# Whats special about web assembly?
- Allows you to run programs written in Rust, Go, C++, pretty much anything that can compile to WASM, in the web
- Runs at near native speeds
- Also runs in sandboxes

---
layout: fact
---
# How do I start?

---
layout: center
---
# The Big Players
## Cloudflare Workers
- powers Vercel Edge Functions
- runs on cloudflare's network
- more popular (or at least has more marketing)
## Deno Deploy
- powers Netlify Edge Functions, Supabase Edge Functions
- Absurdly fast deployment (if you don't have a build step)
- better free tier

<!-- Shopify Oxygen is also doing using javascript containers. Though I'm not sure if they're using their own thing or not -->

---
layout: center
---
# Limitations
The smaller the better
## Cloudflare Workers
- Maximum app size of 1 MB
- Nodejs APIS not all available. Cloudflare specific runtime (basically stripped down Node)
- max 128MiB memory usage
## Deno Deploy
- Maximum app size of 20 MB
- max 512MiB memory usage
- NodeJS APIs not available (cuz its deno...)

<v-click>

[WinterCG](https://wintercg.org/)

</v-click>

<!-- The runtimes are kinda crazy right now. Though the WinterCG is helping to standardize these -->

---
layout: statement
---
# Your Databases should also be distributed!
Your database is still your bottleneck


<!-- You can distribute your business logic, but if you do that then maybe you should distribute data. -->

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
- Next.js (react)
- Astro (react, vue, svelte, solid)