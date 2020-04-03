# Route & Router

## Route

The `app.route(path)` method returns an instance of a single route, which you can then use to handle HTTP verbs with optional middleware. **We can use this method to avoid duplicating route names**.

Before we start, we are going to go through the 4 steps of creating a server. Then we'll handle 2 HTTP methods **with the same endpoint**: a GET and a POST.

> app.js
```
// 1. Import express module
const express = require("express");

// 2. Create server
const app = express();

// 3. Set a port
const port = process.env.PORT || 3000;

// 4. Listen
app.listen(port, () => console.log("server-started"));

// Functions
app.get("/electronics/computers", (req, res) => {
    // ...some code here...
});

app.post("/electronics/computers", (req, res) => {
    // ...some code here...
});
```

As we mentioned, in the above example, both of these function have the same path: _"/electronics/computers"_.

In order to simplify this process we can use `app.route()` and chain both functions together.

> app.js
```
app.route("/electronics/computers",)
    .get(req, res) => { // ...some code here... })
    .post(req, res) => { // ...some code here... });
```

Now the code look much more readable and simplifyed, but we can go even further, by using the Router object.

## Router

A router object is an isolated instance of middleware and routes. You can think of it as a “mini-application,” capable only of performing middleware and routing functions.

**A router behaves like middleware itself**, so you can use it as an argument to `app.use()` or as the argument to another router’s `use()` method.

As we've seen in the examples above, we have two functions for the "electronics" section of our website. What if we had complete website, with other product categories like "books" and "software"? It would get really messy if we write all our functions in one file.

The Router object is going to help us with that. It is going to make it easier for us to orginize our code. So our next step is to put everything that has to do with "electronics" in one file, **inside a folder called "routes"**.

> electronics.js
```
// Import express module
const express = require("express");

// Import router
let router = express.Router();

// Bring our functions from app.js
router
    .route("/computers",)
    .get(req, res) => { // ...some code here... })
    .post(req, res) => { // ...some code here... });

// Export router
module.exports = router;
```
As you might have noticed, we made some small modifications:
- Our functions are no longer applied to the **app** variable, which contains `express()`. They are applied to the **router** object, which contains `express.Router()`.

- The path is no longer "/electronics/computers" but just "/computers". This is because we are going to tell **app.js** to use the functions from **electronics.js** every time the path is "/electronics".

- We are exporting the router, to be able to use these functions in **app.js**.

The only thing we are missing now is to tell **app.js** to use **electronics.js** whenever the path is "/electronics". Remember when we said that a router behaves like a middleware? Well we are going to import it just like a middleware. Using `app.use()`.

> app.js
```
// First we import the file
const electronics = require("./routes/electronics");

// The we use it
app.use("/electronics", electronics);
```

Sources:

YouTube - Steve Griffith:
https://www.youtube.com/watch?v=iM_S4RczozU&list=PLyuRouwmQCjne87u8XUdOM5oCl7vI2vVL&index=8&t=0s

Express API reference:
https://expressjs.com/en/4x/api.html