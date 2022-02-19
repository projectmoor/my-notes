## Overview
Context can be used for state management across components.

Context is not optimized for high frequency changes (multiple changes per second). For that purpose, we use Redux.
- Authentication is not frequent, thus we can use Context for that.

Bad Example: using context in the button component to control the onClick handler function. It's not wise because doing this, the button will only be a button with this specific functionality.

Thus use props for configuration (like button configuration), use context for state management across the entire app.

-----
## How to enable context
1. Context component: define what will be provided in Context
2. Add Context Provider Wrapper to the parent component
	1. define value object (what will be provided to child components)
3. Add Context Consumer Wrapper to the child component or use useContext hook

------
## Steps
1. Set up a context file - Context component
```js
import React from 'react';

  
const AuthContext = React.createContext({

	isLoggedIn: false

});
  

export default AuthContext;
```

2. Wrap around parent component with Context Provider
```jsx
import AuthContext from './store/auth-context';

function App() {
	return (
		<AuthContext.Provider
			value={{
				isLoggedIn: isLoggedIn,
			}}	
		>
			<MainHeader onLogout={logoutHandler} />
			<main>
				{!isLoggedIn && <Login onLogin={loginHandler} />}
				{isLoggedIn && <Home onLogout={logoutHandler} />}
			</main>
		</AuthContext.Provider>
	)
}
```
*if we only have default props in Context, we don't need Context Provider*

3. MainHeader has child component Navigation, we want to access the isLoggedIn property in Navigation component

3a. One way is to use Context Consumer wrapper and access the property thru function (a function receives context object and returns JSX component)
```jsx
import AuthContext from '../../store/auth-context';

const Navigation = (props) => {
	return (
		<AuthContext.Consumer>
			{(ctx) => {
				return (
					<nav className={classes.nav}>
						<ul>
							{ctx.isLoggedIn && (
								<li>
									<a href="/">Users</a>
								</li>
							)}
						</ul>
					</nav>
				);
			}}
		</AuthContext.Consumer>
	);
};

export default Navigation;
```

3b. Another Way: useContext hook
```jsx
import React, { useContext } from 'react';
import AuthContext from '../../store/auth-context';

const Navigation = (props) => {
	const ctx = useContext(AuthContext);

	return (
		<nav className={classes.nav}>
			<ul>
				{ctx.isLoggedIn && (
					<li>
						<a href="/">Users</a>
					</li>
				)}
			</ul>
		</nav>
	);
};

export default Navigation;
```

---
Custom Context Provider

Forward props via `value` object in Context Provider

Here, in the App.js file, we need to pass `onLogout` to the `MainHeader` , where `logoutHandler` is a function defined in the App.js file.
If we custom Context Provider, we won't need to pass the `onLogout` manually.
```jsx
import AuthContext from './store/auth-context';

function App() {
	return (
		<AuthContext.Provider
			value={{
				isLoggedIn: isLoggedIn,
			}}	
		>
			<MainHeader onLogout={logoutHandler} />
			<main>
				{!isLoggedIn && <Login onLogin={loginHandler} />}
				{isLoggedIn && <Home onLogout={logoutHandler} />}
			</main>
		</AuthContext.Provider>
	)
}
```
Would be:
```jsx
import AuthContext from './store/auth-context';

function App() {
	return (
		<AuthContext.Provider
			value={{
				isLoggedIn: isLoggedIn,
				onLogout: logoutHandler
			}}	
		>
			<MainHeader />
			<main>
				{!isLoggedIn && <Login onLogin={loginHandler} />}
				{isLoggedIn && <Home onLogout={logoutHandler} />}
			</main>
		</AuthContext.Provider>
	)
}
```

-----
## Refactor the Context Provider
- place Provider inside the Context component. 
- Handle state variable inside the Context component
```jsx
import React, { useState } from 'react';

  
const AuthContext = React.createContext({

	isLoggedIn: false,
	onLogout: () => {},
	onLogin: (email, password) => {}

});

const AuthContextProvider = () => {
	const [isLoggedIn, setIsloggedIn] = useState(false);
	const logoutHandler = () => {
		setIsLoggedIn(false);
	}
	const loginHandler = () => {
		setIsLoggedIn(true);
	}

	return <AuthContext.Provider
				value={{
					isLoggedIn: isLoggedIn,
					onLogout: logoutHandler,
					onLogin: loginHandler,
				}}
	
			>{props.childern}</AuthContext.Provider>

};
  

export default AuthContext;
```

Now we can wrap App component with Provider, in index.js file where App component is used:
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App'; 
import { AuthContextProvider } from './store/auth-context';

ReactDOM.render(
	<AuthContextProvider>
		<App />
	</AuthContextProvider>, document.getElementById('root'));
```

In the App.js, we can access those props thru `useContext` 
And in other components, we can also access those props thru `useContext`

```jsx
import React, { useConext } from 'react';
import AuthContext from '...';

const Home = (props) => {
	const authCtx = useContext(AuthContext);
}
```


----
