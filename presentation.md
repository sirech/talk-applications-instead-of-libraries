<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Applications instead of Libraries
## Micro Frontends with Module Federation

???

- the name of this talk is applications instead of libraries
- this is how I named the internal document that started this whole affair. I will explain what I mean throughout the talk. As a sneak preview, this is about leveraging micro frontends

---

## Mario Fernandez
 
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

???

- There are three themes I want to cover

- First I want to talk about our challenges distributing frontend apps, and how we arrived to the micro frontends approach
- Then I want to talk about module federation, the system we have picked to build our architecture
- Lastly, I want to talk about taking this approach past the MVP phase and bringing it to production

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# A Humble Beginning

---

# What Is This About?

---

## Partner Home @ Wayfair

???

- This talks is mainly based on the work I did while at Wayfair, although I no longer work there
- WF is an online furniture retailer

---

<!-- .slide: data-background-image="images/partner-home.png" data-background-size="auto 100%" -->

???

- This is Partner Home
- This is the portal that suppliers use in Wayfair to upload their wares, track inventory, logistics, and so on
- It's less visible than the store that most of our users experience. It's nevertheless really important for the health of the business
---

## Rough numbers

### ~100 flows
### ~30 experience teams

???

- It's a pretty significant piece of software, with a lot of teams involved

---

<!-- .slide: data-background-image="images/decoupling.png" data-background-size="90% auto" -->

???

- This application started, as it often does, as a monolith written in PHP. 
- Things have changed in the last two years, and it's moving to an architecture based on decoupled services

---

<!-- .slide: data-background-image="images/react.jpeg" data-background-size="50% auto" -->

???

- In terms of technology, it relies mostly on React, which a bunch of other techs involved
- This is relevant, as any solution we could think of could assume that everybody would be using React

---

# Facing a Challenge

???

- Enough about context. Let's talk about the problems

---

## Let's start with a question

---

## How to distribute front end applications?

???

- What does this even mean? Bear with me for a bit
- I just mentioned decoupled applications, so you can say: Distribute them individually

---

<!-- .slide: data-background-image="images/decoupling.png" data-background-size="90% auto" -->

---

## What about shared concerns?

???

- this makes even less sense than the question before. Let me show you a picture

---

<!-- .slide: data-background-image="images/navigation.png" data-background-size="100% auto" -->

???

- let's say, a navigation bar
- it's present in almost every page
- in a decoupled application architecture, you have to make sure that every individual app renders it

---

## Attempt 1️⃣
### Using an npm package

---

<!-- .slide: data-background-image="images/library.png" data-background-size="auto 100%" -->

---

## You can guess it didn't work

???

- I'm sitting here giving a talk about it, so it's a safe bet to assume this approach didn't work

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

- But why is that?

---

## Attempt 2️⃣
### Applications instead of libraries

???

- our approach is to treat these concerns as live applications, with all the consequences

---

## A different paradigm

### Independently deployed applications
### Integrated as a cohesive whole

???

- clear ownership
- the devil is in the details

---

<!-- .slide: data-background-image="images/verticals.png" data-background-size="80% auto" -->

???

- another way to see it is focusing on vertical domains
- a vertical domain split fits better the boundaries of the team I was on

---

## Plenty of ways to implement micro frontends

<span class="bottom-right">
The one right way to do microfrontends - two opinions
<a href="https://www.youtube.com/watch?v=Wly6KseqABE">youtube.com/watch?v=Wly6KseqABE</a>
</span>

???

- build time integration
- Server side integration
- runtime composition in the front end
- many options, worthy of its own talk. I did one some time way, in case you're curious

---

## Let's talk about _our_ implementation

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# What is module federation?

???

- 25%

---

## Load modules remotely at runtime

---

## New in Webpack 5

<span class="bottom-right"><a href="https://webpack.js.org/concepts/module-federation/">webpack.js.org/concepts/module-federation/</a></span>

---

## Why is that relevant?

---

## Low friction integration between independent applications

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Making an Application Remote

???

- thus far, we kept it high-level
- we're about to go on a deep dive on how to put this to practice

---

<!-- .slide: data-background-image="images/only-remote.png" data-background-size="auto 100%" -->

???

- what's our goal: Having an application, the remote, that can be integrated by other applications

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

???

- we do this through webpack. That means that you have get into the weeds with webpack configurations
- the good thing is that we don't have to change the way we build our application
- given that we use React, our quantum of sharing are components

---

## That's it? 🤔

???

- This is enough to expose a module, but there's more behind making an application remote

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

```js
const OtherComponent = React.lazy(() => import('remote/OtherComponent'));
```

```js [|9-11]
export default class LazyModule extends React.Component {
  constructor(props) {
    super(props)
    this.state = { error: null }
  }

  render() {
    return (
      <React.Suspense fallback={this.props.delayed ?? null}>
        <OtherComponent />
      </React.Suspense>
    )
  }
}
```

???

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

???

- If we're practicing CI, we'll have more than one environment. Being stuck with one limits our ability to test changes before they reach production

---

## More flexible loading

---

```js
const RemoteComponent = ({component, error, delayed, environment, ...props}) => {
  const RemoteComponentLoader = useMemo(
    () => loadRemoteComponent({ component, environment }),
    [component, environment]
  )

  const RemoteObject = useMemo(
    () => React.lazy(RemoteComponentLoader),
    [RemoteComponentLoader]
  )

  return (
    <LazyModule error={error} delayed={delayed}>
      <RemoteObject {...props} />
    </LazyModule>
  )
}

export default RemoteComponent
```

???

- we have built a remote component, an abstraction to load any component
- crucially, it's smart enough to load from different sources based on configuration
- This is a very simplified slide, as the whole thing is quite big. But, you build it once and you can reuse it

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Shared Dependencies

???

- 50%
- Shared dependencies are one of the classical issues when doing micro frontends. 
- Do you want to have complete independence for the different applications, or do you want to add coupling?

---

## You don't want to download React multiple times

???

- it's not just an efficiency thing
- certain libraries don't work well if you import multiple versions of them

---

## You don't want strong coupling, either

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

???

- The Context API works well in combination with smaller, remote applications
- I would 

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

<!-- .slide: data-background-image="images/shared-provider.png" data-background-size="auto 90%" -->

---

<h2 class="main">
X-Cutting concerns
</h2>

### Logging
### Monitoring
### i18n

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Production Ready

---

<!-- .slide: data-background-image="images/borat.png" data-background-size="90% auto" -->

---

# Error Handling

---

```js
export default class LazyModule extends React.Component {
  static getDerivedStateFromError(error) {
    return { error }
  }

  constructor(props) {
    super(props)
    this.state = { error: null }
  }

  componentDidCatch(_error, errorInfo) {
    // eslint-disable-next-line no-console
    console.error('LazyModule failed while loading remote module', errorInfo)

    if (this.props.logger) {
      this.props.logger.error({
        message: 'LazyModule failed while loading remote module',
        data: errorInfo,
      })
    }
  }
}
```

---

```js
export default class LazyModule extends React.Component {
  render() {
    if (this.state.error !== null) {
      const errorFallback = this.props.error

      if (React.isValidElement(errorFallback)) {
        return errorFallback
      } else if (typeof errorFallback === 'function') {
        return errorFallback({ error: this.state.error })
      } else {
        return null
      }
    }

    return (
      <React.Suspense fallback={this.props.delayed ?? null}>
        {this.props.children}
      </React.Suspense>
    )
  }
}
```

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

<!-- .slide: data-background-image="images/synthetic.png" data-background-size="80% auto" -->

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Scaling Usage

---

<!-- .slide: data-background-image="images/scaling-up.png" data-background-size="80% auto" -->

---

## What if you have multiple remote applications?

---

```js
const RemoteContainerProvider = ({
  environment,
  origins,
  children,
}: RemoteContainerProviderProps) => {
  const remoteContainerRegistry = useRemoteContainerRegistry({
    environment,
    origins,
  });

  return (
    <RemoteContainerContext.Provider value={remoteContainerRegistry}>
      {children}
    </RemoteContainerContext.Provider>
  );
};
```

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Encouraging Adoption

???

- 85%

---

## Should we build our new application as a micro frontend?

---

## It depends!

---

<!-- .slide: data-background-image="images/goldblum-meme.jpeg" data-background-size="auto 100%" -->

---

> Micro frontends are not about technology, they are about organizational structure

---

<!-- .slide: data-background-image="images/ddd.jpeg" data-background-size="auto 80%" -->

---

## Finding meaningful boundaries is **hard**!

---

<!-- .slide: data-background-color="var(--r-main-color)"  -->

# Lessons Learned

---

## Clear advantages
### Isolation
### Quick path to production

---

## There are challenges as well!

---

<!-- .slide: data-background-image="images/tweet.png" data-background-size="90% auto" -->

???

- by law, every time you mention micro frontends in a talk you have to show this tweet
- the point is that micro frontends are a useful tool, but they are not a silver bullet

---

#### medium.com/swlh/webpack-5-module-federation-a-game-changer-to-javascript-architecture-bcdd30e02669
#### aboutwayfair.com/careers/tech-blog/applications-instead-of-libraries-part-2
#### github.com/sirech/example-applications-instead-of-libraries

---

