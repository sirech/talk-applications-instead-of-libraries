<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Applications instead of Libraries

---

## Mario Fernandez
 
 Wayfair
 
???

---

## Themes of this Presentation

<h3 class="fragment fade-up">
How to distribute front end applications
</h3>

<h3 class="fragment fade-up">
Bringing module federation to production
</h3>

<h3 class="fragment fade-up">
Scaling our solution
</h3>

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# The Beginnings

---

# A Bit of  Context

---

picture of partner home

---

picture of transition diagraam, with monolith, then decoupled applications

---

## Rough numbers

### ~100 applications
### ~30 experience teams

---

picture of technologies (React)

---

# A Challenge

---

## Let's start with a question

---

## How to distribute front end applications?

---

picture of the navigation

---

## Attempt 1️⃣
### Using an npm package

---

picture of library being imported

---

## You can guess that it didn't quite work

---

## Challenges

<h3 class="fragment fade-up">
Consistency
</h3>

<h3 class="fragment fade-up">
Rolling updates
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

link to previous talk

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

picture remote application

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Making an Application Remote

---

code snippet exporting

---

## That's it? 🤔

---

## With great power ...

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Consuming a Remote Application

---

picture: application loading remote

---

code snippet import

---

## A problem
### Dynamic loading

---

snippet: focus url

---

## A more flexible loading

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

## X-Cutting concerns
### Logging
### Monitoring
### i18n

---

## A funky experiment
### Configurable remote applications

---

picture configurable remote

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

picture: the testing trophy

---

## Testing remote components

---

code snippet: end 2 end tests

---

picture: application host itselfs

---

## Contract Testing


<span class="bottom-right"><a href="https://matthias-kainer.de/blog/posts/contract-testing-in-the-frontend/">matthias-kainer.de/blog/posts/contract-testing-in-the-frontend/</a></span>

---

# Monitoring

---

## A micro frontend is a live application

---

picture: DD synthetic

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

picture: DDD

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



