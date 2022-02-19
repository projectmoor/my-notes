### Layouts and Features      

\__app.tsx: our foundation 

AppLayout: user details, APIs, account. Handle transit from one feature to another feature.

Trigger animation in App level: app side bar stays, and content changes.

If within the same feature, keep sidebar unchanged, load the rest.

AppLayout also provides safety: never crash. 

Every feature also has a layout. Layout is responsible for providing context to the pages


There are Page specific props, Feature specific props, App wide specific props.


------
## Files at root directory

### env.local

NEXT_: send to clients

non-NEXT: only exposed to server.

JWT: on server only

  

### .eslintrc.js

  Try eslint —fix on save in VSCode

  

### .npmrc

Tell pacakge.json that we don’t want ^

Save exact version, don’t allow other version to be installed

Version “^2.2.0”: 2.2.x 

  
semvar: semantic version 

Major release.feature.bugs

  

### cypress.json
end to end testing

  

### Diagram.svg
show your basic code structure

  

### jest.config.js
Test specific functions - helper functions

  

### next.config.js

Plug into next build system, to do some custom things

Redirect user from ‘/‘ to ‘/login’ path

  

webpack(config) used for

For resolving import, we want lib to point to src/lib

Import ‘lib’ instead of import ‘src/lib’

  

In next version 12: beginning to switch from babel to SWC

Babel is very slow compared to other compiler

Next.js building a new Compiler in Rust: SWC

We use swc for styled component to increase speed:

styledComponents: true

  

### package.json

  

### pull_request_template.md

  

### tsconfig.json

“Path” must match what’s in webpakc(config) next.config.js

  
  
-------

## Redux vs Context
Stormy holds opposite opinion to redux: because redux makes things way too more complicated.

The Freedom App only uses React context to handle shared states.


-------

Random Stuff


IC manufacturing process

Ingot - features



Transition key in layout is the name of page 

  

Segment: capture user data and store in 

  

_app.js

User

Use theme

  
  
FeatureInit payload


  

  

