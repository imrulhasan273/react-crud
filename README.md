# TO RUN THE SERVER ALONG WITH API

Now run below command to run both server.

```cmd
~$ npm run start:dev
```

---

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

# Nav Active Link is not working

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

# Making 404 Page for non-existing page

---

`NotFound.js`

```js
import React from "react";

const NotFound = () => {
  return (
    <div className="not-found">
      <h1 className="display-1">Page Not Found</h1>
    </div>
  );
};

export default NotFound;
```

> Here is a class named `not-found`. It will be designed using `css`

Add below `Route` to `Switch` without `exact path`. Because it has no path.

```js
<Route component={NotFound} />
```

Add CSS Rule to `not-found` class.

```css
.not-found {
  position: fixed;
  top: 0;
  width: 100vw;
  height: 100vh;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

---

# **CRUD Operation Using API**

---

Install `json server` using npm

```cmd
~$ npm i json-server
```

---

# Create Database file

create `db.json` file in root folder.

https://www.npmjs.com/package/json-server

Add `dummy datas` from `https://jsonplaceholder.typicode.com/users`

Add below code to script

```js
"scripts": {
   "json-server": "json-server --watch db.json",
},
```

Use instead

```js
    "json-server": "json-server --watch db.json --port 3003",
```

> Now **npm** and **json server** both should be run side by side.

Run json server using following command

```cmd
~$ npm run json-server
```

---

# Run Multiple Server in one command

---

Install **concurrently** to run multiple command

```cmd
~$ npm i concurrently
```

```js
    "start:dev": "concurrently \"command1 arg\" \"command2 arg\"",
```

Change above with below script

```js
    "start:dev": "concurrently \"npm start\" \"npm run json-server\"",
```

Now run below command to run both server.

```cmd
~$ npm run start:dev
```

---

# Install Axios for API

---

```cmd
~$ npm install axios
```

---

# Data View in Home

---

`Home.js`

```js
import React, { useState, useEffect } from "react";
import axios from "axios";
import { Link } from "react-router-dom";

const Home = () => {
  const [users, setUser] = useState([]);
  useEffect(() => {
    loadUsers();
    // console.log("API");
  }, []);

  const loadUsers = async () => {
    const result = await axios.get("http://localhost:3003/users");
    // console.log(result);
    setUser(result.data);
  };

  return (
    <>
      <div className="container">
        <div className="py-4">
          <h1>Home Page</h1>
          <table className="table border shadow">
            <thead className="thead-dark">
              <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">User Name</th>
                <th scope="col">Email</th>
                <th scope="col">Action</th>
              </tr>
            </thead>
            <tbody>
              {users.map((user, index) => (
                <tr>
                  <th scope="row">{index + 1}</th>
                  <td>{user.name}</td>
                  <td>{user.username}</td>
                  <td>{user.email}</td>
                  <td>
                    <Link className="btn btn-primary mr-2">View</Link>
                    <Link className="btn btn-outline-success mr-2">Edit</Link>
                    <Link className="btn btn-outline-danger">Delete</Link>
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </div>
    </>
  );
};

export default Home;
```

---

# Add `add button` to Navbar

---

`Navbar.js`

```js
<button className="btn btn-outline-light">Add User</button>
```

# CRUD Operation

`Home.js`

```js
import React, { useState, useEffect } from "react";
import axios from "axios";
import { Link } from "react-router-dom";

const Home = () => {
  const [users, setUser] = useState([]);
  useEffect(() => {
    loadUsers();
  }, []);

  const loadUsers = async () => {
    const result = await axios.get("http://localhost:3003/users");
    setUser(result.data.reverse());
  };

  const deleteUser = async (id) => {
    await axios.delete(`http://localhost:3003/users/${id}`);
    loadUsers();
  };

  return (
    <>
      <div className="container">
        <div className="py-4">
          <h1>Home Page</h1>
          <table className="table border shadow">
            <thead className="thead-dark">
              <tr>
                <th scope="col">#</th>
                <th scope="col">Name</th>
                <th scope="col">User Name</th>
                <th scope="col">Email</th>
                <th scope="col">Action</th>
              </tr>
            </thead>
            <tbody>
              {users.map((user, index) => (
                <tr>
                  <th scope="row">{index + 1}</th>
                  <td>{user.name}</td>
                  <td>{user.username}</td>
                  <td>{user.email}</td>
                  <td>
                    <Link
                      className="btn btn-primary mr-2"
                      to={`/user/${user.id}`}
                    >
                      View
                    </Link>
                    <Link
                      className="btn btn-outline-success mr-2"
                      to={`/user/edit/${user.id}`}
                    >
                      Edit
                    </Link>
                    <Link
                      className="btn btn-outline-danger"
                      onClick={() => deleteUser(user.id)}
                    >
                      Delete
                    </Link>
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </div>
    </>
  );
};

export default Home;
```

`AddUser.js`

```js
import React, { useState } from "react";
import axios from "axios";
import { useHistory } from "react-router-dom";

const AddUser = () => {
  let history = useHistory();
  const [user, setUser] = useState({
    name: "",
    username: "",
    email: "",
    phone: "",
    website: "",
  });

  const { name, username, email, phone, website } = user;
  const onInputChange = (e) => {
    console.log(e.target.value);
    setUser({ ...user, [e.target.name]: e.target.value });
  };

  const onSubmit = async (e) => {
    e.preventDefault();
    await axios.post("http://localhost:3003/users", user);
    history.push("/");
  };

  return (
    <div className="container pt-4">
      <div className="card card-body">
        <h2 className="text-center mb-4">Add User</h2>
        <form onSubmit={(e) => onSubmit(e)}>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Name</label>
            <input
              type="text"
              className="form-control"
              placeholder="Enter Name"
              name="name"
              value={name}
              onChange={(e) => onInputChange(e)}
            />
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">User Name</label>
            <input
              type="text"
              className="form-control"
              placeholder="Enter Username"
              name="username"
              value={username}
              onChange={(e) => onInputChange(e)}
            />
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Email address</label>
            <input
              type="email"
              className="form-control"
              placeholder="Enter email"
              name="email"
              value={email}
              onChange={(e) => onInputChange(e)}
            />
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Phone</label>
            <input
              type="text"
              className="form-control"
              placeholder="Enter Phone"
              name="phone"
              value={phone}
              onChange={(e) => onInputChange(e)}
            />
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Website</label>
            <input
              type="text"
              className="form-control"
              placeholder="website url"
              name="website"
              value={website}
              onChange={(e) => onInputChange(e)}
            />
          </div>

          <button type="submit" className="btn btn-primary float-right">
            Add User
          </button>
        </form>
      </div>
    </div>
  );
};

export default AddUser;
```

`EditUser.js`

```js
import React, { useState, useEffect } from "react";
import axios from "axios";
import { useHistory, useParams } from "react-router-dom";

const EditUser = () => {
  let history = useHistory();
  const { id } = useParams();
  const [user, setUser] = useState({
    name: "",
    username: "",
    email: "",
    phone: "",
    website: "",
  });

  const { name, username, email, phone, website } = user;
  const onInputChange = (e) => {
    console.log(e.target.value);
    setUser({ ...user, [e.target.name]: e.target.value });
  };

  useEffect(() => {
    loadUser();
  }, []);

  const onSubmit = async (e) => {
    e.preventDefault();
    await axios.put(`http://localhost:3003/users/${id}`, user);
    history.push("/");
  };

  const loadUser = async () => {
    const result = await axios.get(`http://localhost:3003/users/${id}`);
    setUser(result.data);
  };

  return (
    <div className="container pt-4">
      <div className="card card-body">
        <h2 className="text-center mb-4">Edit User</h2>
        <form onSubmit={(e) => onSubmit(e)}>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Name</label>
            <input
              type="text"
              className="form-control"
              placeholder="Enter Name"
              name="name"
              value={name}
              onChange={(e) => onInputChange(e)}
            />
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">User Name</label>
            <input
              type="text"
              className="form-control"
              placeholder="Enter Username"
              name="username"
              value={username}
              onChange={(e) => onInputChange(e)}
            />
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Email address</label>
            <input
              type="email"
              className="form-control"
              placeholder="Enter email"
              name="email"
              value={email}
              onChange={(e) => onInputChange(e)}
            />
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Phone</label>
            <input
              type="text"
              className="form-control"
              placeholder="Enter Phone"
              name="phone"
              value={phone}
              onChange={(e) => onInputChange(e)}
            />
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Website</label>
            <input
              type="text"
              className="form-control"
              placeholder="website url"
              name="website"
              value={website}
              onChange={(e) => onInputChange(e)}
            />
          </div>

          <button type="submit" className="btn btn-success float-right">
            Update User
          </button>
        </form>
      </div>
    </div>
  );
};

export default EditUser;
```

`User.js`

```js
import React, { useState, useEffect } from "react";
import axios from "axios";
import { Link, useParams } from "react-router-dom";

const User = () => {
  const [user, setUser] = useState({
    name: "",
    username: "",
    email: "",
    phone: "",
    website: "",
  });
  const { id } = useParams();

  useEffect(() => {
    loadUser();
  }, []);

  const loadUser = async () => {
    const res = await axios.get(`http://localhost:3003/users/${id}`);
    setUser(res.data);
  };

  return (
    <div className="container py-4">
      <Link className="btn btn-primary" to="/">
        back to Home
      </Link>
      <h1 className="display-6">User Id: {id}</h1>
      <hr />
      <ul className="list-group w-50">
        <li className="list-group-item">name: {user.name}</li>
        <li className="list-group-item">user name: {user.username}</li>
        <li className="list-group-item">email: {user.email}</li>
        <li className="list-group-item">phone: {user.phone}</li>
        <li className="list-group-item">website: {user.website}</li>
      </ul>
    </div>
  );
};

export default User;
```

`Navbar.js`

```js
import React from "react";
import { Link, NavLink } from "react-router-dom";
const Navbar = () => {
  return (
    <>
      <nav className="navbar navbar-expand-lg navbar-dark bg-primary">
        <div className="container">
          <a className="navbar-brand" href="/">
            React CRUD
          </a>
          <button
            className="navbar-toggler"
            type="button"
            data-toggle="collapse"
            data-target="#navbarSupportedContent"
            aria-controls="navbarSupportedContent"
            aria-expanded="false"
            aria-label="Toggle navigation"
          >
            <span className="navbar-toggler-icon"></span>
          </button>

          <div className="collapse navbar-collapse">
            <ul className="navbar-nav mr-auto">
              <li className="nav-item">
                <NavLink className="nav-link" exact to="/">
                  Home
                </NavLink>
              </li>
              <li className="nav-item">
                <NavLink className="nav-link" exact to="/about">
                  About
                </NavLink>
              </li>
              <li className="nav-item">
                <NavLink className="nav-link" exact to="/contact">
                  Contact
                </NavLink>
              </li>
            </ul>
          </div>
          <Link className="btn btn-outline-light" to="/user/add">
            Add User
          </Link>
        </div>
      </nav>
    </>
  );
};

export default Navbar;
```

`App.js`

```js
import React from "react";
import logo from "./logo.svg";
import "./App.css";
import "../node_modules/bootstrap/dist/css/bootstrap.css";
import Home from "./components/pages/Home";
import About from "./components/pages/About";
import Contact from "./components/pages/Contact";
import Navbar from "./components/layouts/Navbar";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import NotFound from "./components/pages/NotFound";
import AddUser from "./components/users/AddUser";
import EditUser from "./components/users/EditUser";
import User from "./components/users/User";

function App() {
  return (
    <Router>
      <div className="App">
        <Navbar />
        <Switch>
          <Route exact path="/" component={Home} />
          <Route exact path="/about" component={About} />
          <Route exact path="/contact" component={Contact} />
          <Route exact path="/user/add" component={AddUser} />
          <Route exact path="/user/edit/:id" component={EditUser} />
          <Route exact path="/user/:id" component={User} />

          <Route component={NotFound} />
        </Switch>
      </div>
    </Router>
  );
}

export default App;
```
