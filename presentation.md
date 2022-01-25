<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Applications instead of Libraries

---

## Mario Fernandez
 
 Wayfair
 
???

---

<h2 class="main">
Themes
</h2>

<h3 class="fragment fade-up">
Distributing front end applications
</h3>

<h3 class="fragment fade-up">
Adopting module federation
</h3>

<h3 class="fragment fade-up">
Making it production-ready
</h3>

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# The Beginnings

---

# A Bit of  Context

---

<!-- .slide: data-background-image="images/partner-home.png" data-background-size="auto 100%" -->

---

picture of transition diagraam, with monolith, then decoupled applications

---

## Rough numbers

### ~100 applications
### ~30 experience teams

---

picture of technologies (React)

---

# Facing a Challenge

---

## Let's start with a question

---

## How to distribute front end applications?

---

<!-- .slide: data-background-image="images/navigation.png" data-background-size="100% auto" -->

---

## Attempt 1️⃣
### Using an npm package

---

picture of library being imported

---

## You can guess that it didn't quite work

---

<h2 class="main">
Challenges
</h2>

<h3 class="fragment fade-up">
Propagating updates
</h3>

<h3 class="fragment fade-up">
Consistency
</h3>

<h3 class="fragment fade-up">
Operability
</h3>

---

## Attempt 2️⃣
### Applications instead of libraries

---

picture of navigation as a micro front end

---

picture of different micro front end strategies

---

## Plenty of ways to implement micro frontends

<span class="bottom-right"><a href="https://www.youtube.com/watch?v=Wly6KseqABE">youtube.com/watch?v=Wly6KseqABE</a></span>

---

## Let's talk about our implementation

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# What is module federation?

---

## Load modules remotely

---

## New in Webpack 5

<span class="bottom-right"><a href="https://webpack.js.org/concepts/module-federation/">webpack.js.org/concepts/module-federation/</a></span>

---

## Why is that relevant?

---

## Low friction integration between independent applications

---

TODO: is there a picture missing here?

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Making an Application Remote

---

<!-- .slide: data-background-image="images/only-remote.png" data-background-size="auto 100%" -->

---

code snippet exporting

---

## That's it? 🤔

---

## With great power ...

TODO: what do I want to say here

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Consuming a Remote Application

---

<!-- .slide: data-background-image="images/remote-application.png" data-background-size="auto 100%" -->

---

code snippet import

---

## A problem
### Dynamic loading

---

snippet: focus url

---

## More flexible loading

---

code snippet: RemoteComponent

---

picture: loading single component

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Shared Dependencies

---

snippet shared dependencies

---

## You don't want to download React multiple times

---

## You don't want strong coupling, either

---

(maybe?) dependency resolution in module federation

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Beyond Single Components

---

## Keeping internal state private


---

code snippet: internal context

---

## Interactions between host and remote applications

---

code snippet: shared provider

---

<h2 class="main">
X-Cutting concerns
</h2>

### Logging
### Monitoring
### i18n

---

## A funky experiment
### Configurable remote applications

---

<!-- .slide: data-background-image="images/configurable-remote.png" data-background-size="auto 100%" -->

---

## 🧱🚧🏗️☢️🧯💥

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Production Ready

---

# Error Handling

---

code snippet boundaries

---

# Testing

---

<!-- .slide: data-background-image="images/trophy.jpeg" data-background-size="auto 100%" -->

---

## Testing remote components

---

code snippet: end 2 end tests

---

<!-- .slide: data-background-image="images/remote-application-local.webp" data-background-size="auto 100%" -->

---

## Contract Testing


<span class="bottom-right"><a href="https://matthias-kainer.de/blog/posts/contract-testing-in-the-frontend/">matthias-kainer.de/blog/posts/contract-testing-in-the-frontend/</a></span>

---

# Monitoring

---

## A micro frontend is a live application

---

<!-- .slide: data-background-image="images/synthetic.png" data-background-size="auto 100%" -->

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Scaling Usage

---

## What if you have multiple remote applications?

---

code snippet: registry

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Encouraging Adoption

---

## Should we build our new application as a micro frontend?

---

## It depends!

---

<!-- .slide: data-background-image="images/ddd.jpeg" data-background-size="auto 100%" -->

---

## Finding meaningful boundaries is **hard**!

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Lessons Learned

---

TODO

---

links

---



