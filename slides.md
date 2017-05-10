<a href='https://rd3.now.sh/#' target='demo'>
  <img class='logo' src='img/rd3-white.svg' width='150px' />
</a>

## React D3 Playground
Thibaut Tiberghien

Notes:
1. start with demo to engage audience
1. dashboard, chat, refresh when type text
1. stats viz, explore by hovering
1. barchart modes, piechart modes
1. user colors
1. back to slides > RD3, side project, React + D3
1. apologize for talk rather technically inclined
1. please interrupt me, interactive is better, like visualization

-----

## Data Experience Developer
---
Crafting data-driven apps with great UX & DX

Notes:
1. Why? 
  - received job offer
  - remind me of previous experience with dataviz
  - excited to mix in latest tech I've learned
  - good preparation for the position
1. goal: craft data-driven apps, mainly but not only dataviz
1. 2 focus: UX & DX
1. will explain now

---

### User Experience (UX)
- target: powerful data exploration
- interactive visualizations
- interconnected graphs with Reactive filtering
- requires high rendering performance

Notes:
1. dataviz much more powerful when interactive and allow deep exploration of data
1. hover on element filters data and update other graphs accordingly
1. need fast rendering of independent graphs that are interconnected through data

---

### render performance
- repainting the DOM is expensive (slow)
- minimize DOM manipulation with React
- components render to virtual DOM
- components lifecycle & reconciliation
- UI as pure function of data (DX)

Notes:
1. known issue: repainting the DOM is slow
1. what is the DOM?
  - simplified: the page in your browser, with structure and style
  - compute and manipulate the DOM is fast (your code)
  - but "repaint" by browser is slow (layout of DOM on the page with styling)
1. Facebook UI lib solves this: React
1. compose UI from components, from small button to full profile
1. components are "rendered" to a virtual DOM (representation of DOM but fast to compute)
1. when something changes, virtual DOM completely recalculated (fast)
1. reconciliation! find out what changed and send it to the DOM > minimal DOM changes
1. component lifecycle to avoid virtual DOM computation altogether
1. bonus: UI pure function of data, easier to code, only care about what it looks like
1. transition to DX

---

### Developer Experience (DX)
- target: fast prototyping
- efficient development tools & setup
- modular codebase, reusable components
- code architecture to ease debug & maintenance
- best practices, helping collaboration

Notes:
1. concept: great apps come from developers using tools they love
1. goal: increase DX to support fast prototyping team with the good tools and processes
1. how to make a developer or team perform at their best?
1. seamless experience when writing software
1. great tools, shared best practices
1. making code easier to write, debug and maintain

---

### DX with React
- composition of small focused components
- portability by co-locating component's code
- one-way dataflow, state management
- UI as pure function of props & state
- hot module reloading, dev tools, error reporting

Notes:
1. replace monolithic codebase by composition of small focused components
1. co-locate JSX (compiles to HTML), CSS-in-JS, JS
  - easy to reuse
  - reduce scope of search for bugs
1. UI pure function + one-way dataflow
  - code easy to reason about
  - enforce understanding and good practice for state management > less bugs, easy collaboration
1. devtools to inspect app, HMR & outstanding error reporting for shorter feedback loop > save time

-----

## Data visualization in React
---
React + D3.js

---

### D3.js
"Data-Driven Documents"
- most established web dataviz library
- data-driven DOM manipulation
- binding between data & DOM elements
- numerous mathematical helpers

Notes:
1. most established data visualization lib for the web
1. many other tools based on it
1. power is 2-fold:
  - data-driven DOM manipulation, especially data binding between data points and DOM elements (SVG) > example: scatterplot, one disc per datapoint, D3 keeps track of the binding for you
  - helper functions for array of problem: scales, colors, bar charts, maps, force-directed graphs, voronoi diagrams

---

### React + D3.js
- reusable charts
- render optimizations
- both are DOM manipulation libs
- don't play well together out of the box

Notes:
1. WHY?
  - reusable charts!!
  - React render performance
  - React ecosystem: tooling, state management, data selectors, SSR
  - lot of new possibilities of the modern web > bring it to D3 charts
1. HOW?
  - problem is both manipulating DOM separately
  - clash if no precaution
  - don't work out of the box
  - I studied the potential solutions, compare them now

---

### Idea 1
Split their scope of duty
- disable React in D3 land
- loose all powers of React in D3 code
- especially reconciliation part
- increase DOM repainting, hence slower

Notes:
1. techniques to shut off React in a part of the DOM
  - react renders empty div, D3 populates it
  - shouldComponentUpdate return false
1. no more render performance (reconciliation) > slower
1. no more tooling, etc. > poor DX
1. basically, shut off modern web from D3 land

---

### Idea 2
D3 for Maths, React for DOM
- most widespread technique
- use D3 helpers to prepare data for rendering
- use React to render based on helpers output
- loose D3 transitions (`enter()`, `update()`, `exit()`)
- poor DX compared to vanilla D3
- React lock-in
- time-consuming, learning curve

Notes:
1. mentioned the helpers > use this only
1. data binding and DOM manipulation by React
1. most widespread technique, especially in charting libs
1. good performance

BUT:
1. loose D3 data binding and enter/update/exit paradigm
  - therefore loose D3 transitions and animations (big loss)
1. code is extremely different from vanilla D3
  - poor DX: need to draw SVG yourself in JSX + axis etc.
  - less portable since JSX tainted
  - longer to code from examples (always in D3) or port existing code
  - learning curve for D3 developers, unnecessary

---

### Idea 3
Feed D3 a fake DOM that renders to state
- vanilla D3 code with full API support
- rendering by React with optimizations
- inspectable by React DevTools
- supports D3 transitions & CSS animations
- portable viz code, tiny learning curve
- isomorphic charts with server-side rendering

<a href='https://rd3.now.sh/#' target='demo'>&#9673;</a>

Notes:
1. based on lib called react-faux-dom
1. feed fake dom to D3
  - JS object that behaves like a DOM from D3's perspective
  - render to React component state instead of DOM
  - React picks up state changes and render to DOM
1. full React: render performance, devtools, etc.
1. full D3 API compatibility: animations, transitions, events
1. minor changes to D3 code:
  - code from example, port existing code
  - can extract viz out of react
  - no learning curve
  - great DX
1. isomorphic charts: SSR, compute on server, send down and reconcile (great UX)
1. DEMO > generate new text > scatterplot enter/update/exit + other charts update

-----

## RD3 Dashboard
---
Advanced techniques

Notes:
1. play well together > see advanced techniques
1. I'll keep going back to demo to make it easier to grasp

---

### Sharing state
- move state up the component tree
- share state across charts / components
- charts can React to interaction in another chart
- enable deeper exploration of correlations in data
- example: *hover* is shared for filtering

<a href='https://rd3.now.sh/#' target='demo'>&#9673;</a>

Notes:
1. most apps require for state to be shared across components
1. in react: move state up the component tree and pass it down as props
1. in our dashboard:
  - interaction with a chart
  - other charts can respond to it
  - allow implicit filtering and exploration of data correlations
1. DEMO: hover
  - hover in one chart, hover info shared in dashboard, other chart react to it
  - barchart and scatterplot: set opacity to filter
  - pie chart: recompute data completely

---

### Enter Redux
- central repository for shared state of whole app
- components request part of state they need
- enforce outstanding architecture, great dev tools
- extend one-way dataflow to state changes
- new state pure function of state & action
- easy to reason about, always in sync
- real-time & mock data is straightforward

Notes:
1. lib to manage state in apps, not react specific but great fit for it
1. central repository of state, components request part they need
  - no need to keep track of state in each component or into a common parent to share it
  - everything in one place and easy to manage
1. biggest gain in DX here:
  - enforce good practices > code execution is easy to reason about, state always in sync across app with no code (huge)
  - next state is pure function of current state and action
  - great architecture for defining actions (source of change) and dispatching them
  - react has pure functions and 1-way dataflow for state-to-UI, redux extend it to state modification
  - overall easier to find bugs, architecture improves collaboration among developers
  - dev tools are outstanding and huge time saver
  - mock data to prototyping easy
  - real-time update pushed by server easy

---

### Styled Components
- CSS for the component age
- encapsulate CSS with component, class generated
- remove big stylesheet, class mapping, conflicts
- full CSS support + helpers + portability
- use ES6 tagged template literals &#187; adds JS power
- CSS changes based on props, themes, etc.
- no learning curve

<a href='https://rd3.now.sh/#' target='demo'>&#9673;</a>

Notes:
1. DEMO: redux also used to share text and colors > colors are used for styling
1. styling is generally a pain point in complex apps:
  - big stylesheets
  - mapping of classes and elements external to components
  - style conflicts
  - move a component, style changes due to context
1. styled-components is lib for styling in component age:
  - style co-located with component
  - class generated and unique
  - context has little impact, thus component portable / reusable
1. better than other CSS-in-JS because
  - full CSS support
  - write CSS
  - no learning curve
1. ES6 tagged template literals:
  - JS power to manipulate and write CSS
  - can adapt to props, dynamic styling
  - can do theming
1. huge DX boost and time saving
1. DEMO: CSS is provided to all charts by dashboard component
  - centralized styling, charts have their style handled for them
  - charts only care about the element, faster coding

---

### Rendering performance tricks
- tooltips by React, not D3 &#187; cheaper renders
- dashboard share CSS changes &#187; avoid charts renders
- D3 re-render only on new data (or resize)
- reduce the need for React renders with:
  - immutable Redux store
  - reselect to compute props only on changes
  - shallow comparison is cheap

<a href='https://rd3.now.sh/#' target='demo'>&#9673;</a>

Notes:
1. D3 updates more expensive than react render
  - why? d3 recompute all helpers and react renders anyway
  - try to reduce need for D3 updates
  - DEMO: show render counts, D3 only render on new data or resize
1. tooltips very dynamic, extract them to JSX
  - DEMO: hover tooltip > render count
1. shared styling by dashboard means only css changes, not components
  - DEMO: hover opacity and user color > render count
1. further increase performance by reducing need for react renders
  - virtual DOM not recalculated, no reconciliation: huge performance improvement
  - components normally update each time parent updates (DEMO: tick)
1. techniques to avoid that (sorry very technical):
  - only render component if state/props change
  - comparison of state/props can be expensive: large amount of data in charts
  - cheap comparison in JS if object is the same reference (location in memory) (shallowCompare)
  - when state change, ref to non-changed values should be the same
    - immutable redux store
    - props > calculated from state: use memoized selectors, reuse reference if state same 
  

---

### Quick wins
- Business
  - can be embedded in any app
  - performance & DX suitable for production code
  - shorter prototype-to-production time
- Designers
  - component paradigm is natural for designers
  - resize & drag for fast UX exploration

<a href='https://rd3.now.sh/#' target='demo'>&#9673;</a>

Notes:
Business
1. react is a UI lib, not a framework
1. compatible with any framework or vanilla JS > easy to deploy in existing applications  
1. great performance and DX: code is usable for production
1. faster transition from prototype to client app

Design
1. component paradigm more suitable to work with designers
1. this is how they work already
1. tools like react-sketch-app to generate sketch content from react code (for design systems)
1. resize and drag for fast UX testing (almost no code, speed of render): DEMO!

-----

## Summary
- high performance apps with reusable charts
- enable powerful data exploration experience
- best DX enabling fast prototyping
- close to no learning curve for D3 programmers
- modern web stack with latest best practices
- shorter prototype-to-production time

Notes:
1. craft powerful data exploration experience using high performance rendering
1. great DX with best practices, powerful tooling, easy-to-debug modular code, reusable charts
1. D3 programmers get rendering performance at 0 cost, close to no learning curve
1. modern web stack and tools, cutting edge technology
1. lots of time saved in development, faster time to production: perfect for fast prototyping

---

## Q&A
https://rd3.now.sh

https://github.com/tibotiber/rd3

Notes:
1. React vs Angular2?
  - both are component based
  - performance is similar (for ng2, ng1 slower)
  - UI lib vs MVC, no model in React, make it seem unnecessary
  - MVC works great in backend with db and apis but UI can be simpler imo
  - typescript is great but a bit bloaty: jsdocs + vscode extension enough
  - developer satisfaction better in react
  - too much ng specific things to learn vs react is just javascript
  - JSX is better than ng templates imo
  - react devtools are outstanding
  - you get better as JS learning react, and learn some functional programming as well
  - I generally find Angular more complex and unnatural (personal preference)
1. React vs Vue?
  - HTML templates vs JSX:
    - JSX better debugging and testing
    - JS power to write
  - Vue simpler most of the time and faster as well
  - React has large tooling setup
  - React shines in larger projects:
    - component system easier to maintain, DRY
    - JSX debugging vs templates
  - advanced optimizations in React
    - shallow rendering
    - react-virtualized
  - larger community and battle-tested
  - react native
1. React vs Ember?
  - ember's chipmunk logo is more playful
  - ember much more opinionated and heavy
  - MVC vs UI lib, no model in React, make it seem unnecessary
  - MVC works great in backend with db and apis but UI can be simpler imo
  - ember has convention over configuration, sane default
    - faster to get started
    - no decision fatigue
    - but can be really cumbersome to get out of typical uses
  - handlebars templates vs JSX
    - JSX better debugging and testing
    - JS power to write
    - learn more fundamental JS + functional approach
  - styling components in ember has poor DX
  - 2-way data binding :(
  - ember cli to generate code
  - react more at the edge of performance and DX
  - devtools are better in react, HMR + redux devtools
  - innovation comes to react much earlier than ember