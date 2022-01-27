<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Applications instead of Libraries

???

- the name of this talk is applications instead of libraries
- this is how I named the internal document that described our approach based on micro frontends. I'll explain what I mean throughout the talk

---

## Mario Fernandez
 
 Wayfair
 
???

- I'm an engineer at Wayfair

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

???

- There are three themes I want to cover

- First I want to talk about our challenges distributing frontend apps, and how we arrived to the micro frontends approach
- Then I want to talk about module federation, the system we have picked to build our architecture
- Lastly, I want to talk about taking this approach past the MVP phase and bringing it to production

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# A Humble Beginning

---

# What Are We Building?

---

<!-- .slide: data-background-image="images/partner-home.png" data-background-size="auto 100%" -->

???

- This is Partner Home
- This is the portal that suppliers use in Wayfair to upload their wares, track inventory, logistics, and so on
- It's less visible than the storefront, but it's nevertheless really important for the health of the business
---

## Rough numbers

### ~100 flows
### ~30 experience teams

???

- It's a pretty significant piece of software, with a lot of teams involved

---

<!-- .slide: data-background-image="images/decoupling.png" data-background-size="100% auto" -->

???

- This application started as a monolith written in PHP. Due to the lack of visibility, it has been historically a bit underinvested
- This has changed in the last two years, and we're moving to an architecture based on decoupled services

---

## Based on React

???

- In terms of technology, we rely mostly on React, which a bunch of other techs involved
- This is relevant, as any solution we could think of could assume that everybody would be using React

---

# Facing a Challenge

---

## Let's start with a question

---

## How to distribute front end applications?

???

- I just mentioned decoupled applications, so you can say: Distribute them individually

---

## What about shared concerns?

---

<!-- .slide: data-background-image="images/navigation.png" data-background-size="100% auto" -->

???

- let's say, a navigation bar
- it's present in almost every page

---

## Attempt 1️⃣
### Using an npm package

---

<!-- .slide: data-background-image="images/library.png" data-background-size="auto 100%" -->

---

## You can guess that it didn't quite work

???

- you can guess that the fact that I'm giving a talk about this implies that this approach didn't work

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

???

- But why?

---

## Attempt 2️⃣
### Applications instead of libraries

---

picture of navigation as a micro frontend

---

<!-- .slide: data-background-image="images/tweet.png" data-background-size="90% auto" -->

???

- by law, every time you mention micro frontends in a talk you have to show this tweet
- so yeah, micro frontends don't fix every issue, but they are really useful in the right context

---

picture of different micro frontend strategies

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

```js [|2|4-6]
new ModuleFederationPlugin({
  name: 'remote',
  filename: 'remoteEntry.js',
  exposes: {
    './Welcome': './src/Welcome',
  },
  shared: [
    {
      react: { requiredVersion: deps.react, singleton: true },
      'react-dom': { requiredVersion: deps['react-dom'], singleton: true },
      '@applications-instead-of-libraries/shared-library': {
        import: '@applications-instead-of-libraries/shared-library',
        requiredVersion: require('../shared-library/package.json').version,
      },
      '@material-ui/core': {
        requiredVersion: deps['@material-ui/core'],
        singleton: true,
      },
    },
  ],
})
```

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

```js [|3-5]
new ModuleFederationPlugin({
  name: 'host',
  remotes: {
    remote: 'remote@http://localhost:3002/remoteEntry.js',
  },
  shared: [
    {
      react: { requiredVersion: deps.react, singleton: true },
      'react-dom': { requiredVersion: deps['react-dom'], singleton: true },
      '@applications-instead-of-libraries/shared-library': {
        import: '@applications-instead-of-libraries/shared-library',
        requiredVersion: require('../shared-library/package.json').version,
      },
      '@material-ui/core': {
        requiredVersion: deps['@material-ui/core'],
        singleton: true,
      },
    },
  ],
})
```

---

## A problem
### Dynamic loading

---

```js
{
  remotes: {
    remote: 'remote@http://localhost:3002/remoteEntry.js',
  }
}
```

---

## More flexible loading

---

code snippet: RemoteComponent
TODO: can I do a snippet or maybe better a pic?

---

picture: loading single component

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Shared Dependencies

---

```js [|4]
{
  shared: [
    {
      react: { requiredVersion: deps.react, singleton: true },
      'react-dom': { requiredVersion: deps['react-dom'], singleton: true },
      '@applications-instead-of-libraries/shared-library': {
        import: '@applications-instead-of-libraries/shared-library',
        requiredVersion: require('../shared-library/package.json').version,
      },
      '@material-ui/core': {
        requiredVersion: deps['@material-ui/core'],
        singleton: true,
      },
    },
  ],
}
```

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

```js [|1-2|6]
const Context = createContext('')
export const useContext = () => React.useContext(Context)

const WelcomeFrame = () => {
  return (
    <Context.Provider value="[private]">
      <Card variant="outlined">
        <CardHeader title="WelcomeFrame"></CardHeader>
        <CardContent>
          <Welcome />
        </CardContent>
      </Card>
    </Context.Provider>
  )
}
```

---

## Interactions between host and remote applications

---

```jsx [|3]
const HostApplication = () => {
  return (
    <LanguageProvider value="de-DE">
      <Box p={1}>
        <RemoteComponent
          component="WelcomeFrame"
          delayed={<>Loading...</>}
        />
      </Box>
    </LanguageProvider>
  )
}
```

---

TODO: more context provider

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

<!-- .slide: data-background-image="images/trophy.jpeg" data-background-size="auto 80%" -->

---

## Testing remote components

---

<!-- .slide: data-background-image="images/remote-application-local.webp" data-background-size="auto 100%" -->

---

```js
context('Integrated Application', () => {
  beforeEach(() => {})

  it('shows the integrated remote component', () => {
    cy.visit('http://localhost:3001')

    cy.contains('Host Application').should('exist')
    cy.contains('The selected locale is de-DE').should('exist')
  })
})
```

---

## Contract Testing


<span class="bottom-right"><a href="https://matthias-kainer.de/blog/posts/contract-testing-in-the-frontend/">matthias-kainer.de/blog/posts/contract-testing-in-the-frontend/</a></span>

---

# Monitoring

---

## A micro frontend is a live application

<h3 class="fragment fade-up danger">
A micro frontend is a live application
</h3>

---

<!-- .slide: data-background-image="images/synthetic.png" data-background-size="90% auto" -->

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Scaling Usage

---

## What if you have multiple remote applications?

---

TODO: re-use abstractions

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

<!-- .slide: data-background-image="images/ddd.jpeg" data-background-size="auto 80%" -->

---

## Finding meaningful boundaries is **hard**!

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Lessons Learned

---

TODO

- There are clear advantages, such as isolation
- There are significant challenges

---

#### medium.com/swlh/webpack-5-module-federation-a-game-changer-to-javascript-architecture-bcdd30e02669
#### aboutwayfair.com/careers/tech-blog/applications-instead-of-libraries-part-2
#### github.com/sirech/example-applications-instead-of-libraries

---



