---
theme: default
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
css: unocss
fonts:
  provider: none
  sans: Satoshi
title: JS On the "Edge"
---

# JS On the "Edge"
or maybe the "Edge" is JS?

<!--
I'll be talking about What is the "Edge" and what does JS have to do with it?
-->

---
layout: center
---

# What is the edge?
Serverless Edge Computing

Running your code on someone else's computer and only for the amount of time it needs to and **as close as possible** to users

## Edge Rendering
Rendering HTML on the edge. Basically just regular rendering but in a different location, but with a few limitations. I'll explain later

<!--
How does the "Edge" differ from typical rendering? How does it differ from typical Serverless Computing?

I'll be talking more about edge computing here
-->

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

<!--
This is where Javascript Comes in

or more specifically, v8 and Web Assembly
-->

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

How is this like containers?
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

<!--
Web Assembly can be ran in v8, which means you can run any compiled language in it. I wanted to show a demo of this but didn't have time :(
You don't have to use WASM, you can use plain JS and be just fine too. This just enables you to use an existing tech stack and get the performance gains from v8.
I feel like WASM isn't quite there yet as far as ease of use goes, but I haven't played around with it much.
-->

---
layout: quote
---

<quote />

[twitter](https://twitter.com/solomonstre/status/1111004913222324225?lang=en)

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
The smaller the app, the better
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
# Frameworks
Popular Frameworks that make it easy to take advantage of this
- Fresh (deno)
  - ssr and islands architecture. No build step (kinda). Really nice DX compared to node in my opinion
- Nuxt 3 (vue)
  - Super quick to write vue SSR apps, kinda magical. Can directly call your api endpoint code on the server side. 
- Remix
  - React SSR, also has a new way of calling backend code. You write your backend logic in each "page" file. (vue support planned)
- SvelteKit
  - Svelte SSR, static generate specific routes, specify 0 JS pages,
- Next.js (react)
  - React SSR, static generate specific routes, can specify specific routes to go "serverless", awesome support by vercel
- Astro (react, vue, svelte, solid)
  -  Islands Arch. I'd say is geared more towards static applications, but can do SSR. Haven't looked much into the SSR part though.

---
layout: center
---
# Your typical flow is like this
1. write your code
2. build with an "adapter"
3. push build to the platform you "adapted" for

The adapter will usually take all the files and create a payload that works with whatever platform.

---
layout: center
---
# Differences between usual SSR (with node)
- The framework will probably have you use Web Standard apis. ex: Fetch, Request, Response, etc
- You have to take into account what runtime your app will be in. ie: if your app works when hosted as a node server, its not guaranteed to work in a Cloudflare Worker or on Deno Deploy

---
layout: statement
---
# Closing Thoughts

<!-- This way of running serverless applications being pushed by Cloudflare Workers and Deno Deploy is still really new, but its getting much more popular.
There's also improvements being made in the "Traditional" space with docker/vms. Fly.io is a pretty cool example with their distributed micro vm/docker infrastructure. There's also things like Lightstream that can help with distributed databases. Its all very cool and I can't wait to see it evolve more. -->

---
layout: center
---
# Resources
[Cloudflare Workers](https://workers.cloudflare.com/)

[How Workers Work](https://developers.cloudflare.com/workers/learning/how-workers-works/)

[Deno Deploy](https://deno.com/deploy)

[JS Party Podcast - What Cloudflare is up to](https://changelog.com/jsparty/209)

## Runtimes
[Cloudflare Workers](https://developers.cloudflare.com/workers/runtime-apis/)

[Deno Deploy](https://deno.com/deploy/docs/runtime-api)
