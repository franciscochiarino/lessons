# Middleware

Middleware functions are functions that have access to the **request** object, the **response** object, and the **next()** function in the application’s request-response cycle.

They are functions that run just before recieving the request. If there is something wrong, the middleware sends back the error, before it gets to the server. It is also used for authentification, and many other purposes.

|CLIENT| --->--->--->---> |MIDDLEWARE||SERVER|

Middleware functions can perform the following tasks:

- Execute any code.
- Make changes to the request and the response objects.
- End the request-response cycle.
- Call the next middleware function in the stack.

If the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging.

## Application-level Middleware

We assign an application-level middleware to an instance of the app object, by using `app.use(path, callback)`.

In the following example, we will write a middleware function. Since we are not going to pass a _path_ parameter, it is going to be executed every time the app recieves a request.
```
// First we assign express to app
const app = express();

// Middleware
app.use((req, res, next) => {
    // The code in here will run every time app recieves a request
    // Finally, we call our next function
    next();
});
```

In the next example, we are going to write a middleware function, that is going to be executed for any type of HTTP request on the `/user/:id` path.
```
app.use("/user/:id", (req, res, next) => {
    // The code in here will be executed for any request on "/user/:id"
    next();
});
```

It is also possible to assign the function to a variable, and then call it when we are using `app.use()`. Let's write a middleware function called _requestTime_ , and also add a requestTime property to the request object:
```
// Here we write the middleware function
const requestTime = (req, res, next) => {
    req.requestTime = Date.now();
    next();
}

// Here we use it
app.use(requestTime);
```

**TIP**

A very useful built-in middleware function in Express is `express.json()`. It parses incoming requests with JSON payloads. Now that we know how to _use_ middleware functions, lets try using `express.json()`:

```
app.use(express.json());
```

Sources:

Tutorials Point:
https://www.tutorialspoint.com/expressjs/expressjs_middleware.htm

Express API reference:
https://expressjs.com/en/4x/api.html

Express Guide:
https://expressjs.com/en/guide/writing-middleware.html
https://expressjs.com/en/guide/using-middleware.html

