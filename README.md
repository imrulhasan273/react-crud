# Create new React App

# Installing Bootstrap

```cmd
~$ npm i bootstrap
```

Import Bootstrap [App.js]

```js
import "../node_modules/bootstrap/dist/css/bootstrap.css";
```

---

I want to make a ajax Request on the Server.

To do so I will be writing some codes in order to make it happen.

---

# Page Navigation in React **SPA**

---

> But react have component. Then what is page navigation?

> React/Vua/Angular works in SPA.

> We need `router-dom`

Install the below package.

```cmd
~$ npm i react-router-dom
```

Import the `router` in `App.js` which is `root` component.

```js
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
```

`App.js`

```js
function App() {
  return (
    <Router>
      <div className="App">
        <Navbar />
        <Switch>
          <Route exact path="/" component={Home} />
          <Route exact path="/about" component={About} />
          <Route exact path="/contact" component={Contact} />
        </Switch>
      </div>
    </Router>
  );
}
```

> But the problem is page is still reloading.

> How to do routing without page refresh.

Add below line to `Navbar.js`

```js
import { Link } from "react-router-dom";
```

```js
<Link class="nav-link" to="/">
  Home
</Link>
```

> use `<Link>` instead of `<a>` tag.

> Use `to` instead of `href`.

Done.. Now we can navigate pages without page refresh.

---

# Nav Link is not working

- We can see that active link is not properly activated.

- Home is active for all links.

Import it

```js
import { Link, NavLink } from "react-router-dom";
```

Use below code `NavLink` instead of `Link`.

```js
<NavLink class="nav-link" to="/">
  Home
</NavLink>
```

- Now change all the attribute name `class` to `className` for JS Convention.

- and use `exact to` instead of `to` for linking.

```js
<NavLink className="nav-link" exact to="/">
  Home
</NavLink>
```

---

Done..... Now active class working properly....

---
