# Express & Routing

## Installation

To install express module, run the following code in your project's terminal:
```
$ npm install express --save
```

To be able to use this module, we have to do the following:
```
// Import the module
const express = require("express");

//Create a server
const app = express();

// Set a port
const port = process.env.PORT || 3000;
```
If you are wondering what `process.env.PORT` is, it means the server will set whatever is in the environment variable "PORT". If there is nothing there, it will take port 3000.

Now that we have what we fundamentally need to run a server, we can start listening to requests using the following function:
```
app.listen(port, () => console.log("server-started"));
```

## Routing Basics

Routing determinates how an application responds to a client request to a particular endpoint. This is set by a path and a specific HTTP request method (GET, POST, etc.).

Each route can have one or more handler functions, which are executed when the route is matched.
The structure looks like this:
```
app.METHOD(PATH, HANDLER);
```
Where:
- **app** is an instance of express().
- **METHOD** is an HTTP request method.
- **PATH** is a path on the server.
- **HANDLER** is the function executed when the route is matched.

### Examples

Respond with "Hello World!" on the homepage (also called root route):
```
app.get("/", (request, response) => {
    response.send("Hello World!");
})
```

Respond to a POST request on the root route:
```
app.post("/", (request, response) => {
    response.send("Got a POST request");
})
```

## Dealing with GET

The GET method is used to request data from a specified resource.

As we've already seen, it is fairly simple to deal with routing using express. We have already used the `send(body)` method, which sends an HTTP response. The _body_ parameter can be a buffer object, a string, an object or an array.

We can also use the `sendFile(path)` method, where we can transfer the file at a given path. The _path_ argument must be an absolute path to the file.
```
app.get("/", (request, response) => {
    response.sendFile(__dirname + "/index.html");
})
```

To our disposal we have also the `json()` method, where we send a response where it's parameter is converted to a JSON sting.
```
app.get("/about", (request, response) => {
    response.json({
        user: "Mustermann",
        age: 47,
        address: {
            street: "Musterstrasse",
            number: "10",
            city: "Musterberg"
        }
    })
})
```

## Dealing with POST

The POST method is used to send data to a server to create/update a resource.

For us not having to convert incoming data to JSON every time, we can use a built-in middleware function (we will talk about middleware later in this guide). We enable this function by writing the following code:
```
app.use(express.json());
```

To be able to see what the user sends us, we will make use of `body`. This contains key-value pairs of data submitted in the request body (it's an object). Let's pretend we recieve some data from a user, and we want to push it to our database stored in a variable called _dataBase_:
```
app.post("/user", (request, response) => {
    const dataSentByUser = request.body;
    dataBase.push(dataSentByUser);
})
```

## Dealing with PUT and PATCH

**PUT**

Replaces an existing resource by a new one. Should be used when we are replacing an entire object.

To make good use of the PUT method, we can use `params`. This property is an object that contains properties mapped to the named route "parameters". For example, if your route is /user/`:id`, then the `id` property is available as `request.params.id`.

Now lets say we want to replace a user. In the following example, we are going to use `request.params` to find the user's `id`. Then we are going to use `request.body` to get the new user, and replace the complete object. 

```
app.put("/update/:id", (request, response) => {

    // Store the id into a variable
    const id = request.params.id;

    // Find the index of the user's object
    const index = dataBase.findIndex(user => user.id === id);

    // Replace the object
    dataBase.splice(index, 1, request.body);
});
```

**PATCH**

Partially updates the resource into the server mapped by the provided data. This is our go-to method when we want to update only a property of an onject.

Say we want to change just the description of a user. Then it would be wise to use PATCH:
```
app.patch("/update/:id", (request, response) => {

    // Store the user's id into a variable
    const id = request.params.id;

    // Find the user's object
    const foundUser = dataBase.find(user => user.id === id);

    // Change user's description
    foundUser.description = request.body.description;
});
```

Note: The `body` in these examples is a property of `request`. Not to be confused with the _body_ parameter of the `send()` method.

## Dealing with DELETE

As the name says, it is used to delete resources.

Imagine we want to delete a certain user from our dataBase. The code is pretty straight forward:
```
app.delete("/users/:id", (request, response) => {

    // Store the user's id into a variable
    const id = request.params.id;

    // Find the index of the user's object
    const index = dataBase.findIndex(user => user.id === id);

    // Delete the element
    dataBase.splice(index, 1);
})
```

Sources:

Express API reference: 
https://expressjs.com/en/4x/api.html

Quora: 
https://www.quora.com/What-is-the-difference-between-PUT-POST-and-PATCH-for-RESTful-APIs?share=1

YouTube - Fun Doo Testers:
https://www.youtube.com/watch?v=JZiEooQKxQQ
