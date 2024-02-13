# ![[Express REST API] - Index Route](./assets/tktk-hero.png)

**Learning objective:** By the end of this lesson, students will be able to create an index route to find all pets, by interacting with the database, and send a JSON response to the client.

## READ - Index Route

In this section, we will create a new route to find all pets. This route will be a `GET` request to `/pets`, and will return a JSON response with all of the pets in the database.

We will be following these specs when building the route:

- **CRUD Action:** READ
- **Method:** `GET`
- **Path:** `/pets`
- **Response:** JSON
- **Success Status Code:** 200
- **Success Response Body:** An array of all the pets in the database named `pets`. If there are no pets in the database, the array will be empty.
- **Error Status Code:** 500
- **Error Response Body:** A JSON object with an `error` key and a message describing the error.

Let's get started! 🎉

## Create a GET Route - Index

Inside of `controllers/pets.js`, create a new route to find all pets. The **method** for this route is `GET`, and the **path** is `/pets`. Use the `.get()` method on the `router` object to create the route and pass in the path and a callback function.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
    // Seting up for our code
});
```

We are using `async` at the start of the callback function to allow us to use `await` inside of the function. This will allow us to use `await` when we query the database for all pets.

> 🧠 Notice how we have declared this path. We have just used `/` as the path. This is because we are already in the `/pets` route. This is a shorthand way of declaring the path. Check out `server.js` to see how we have declared the `/pets` route.

### Test the Route

Before we can query the database it's important to test the route to make sure it's working. We can do this by sending a response with a message to the client.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
    // Add a message to test the route
    res.json({ message: 'Index Route' });
});
```

Let's use Postman to test the route. Open Postman and send a `GET` request to `http://localhost:3000/pets`. You should receive a JSON response with the message `Index Route`. If you do not receive this message, debug before moving on.

> 📚 If you are having trouble, check the terminal for any error messages. Sometimes it's as simple as the server was not running. 😀

### Try - Find All Pets

We have tested the route so let's query the database and find all pets. We can do this by using the `.find()` method on the `Pet` model. We will use `await` to wait for the database to return all pets. However, we need to wrap this in a `try...catch` block to handle any errors that may occur.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
    try {
        const foundPets = await Pet.find();
        res.json({ pets: foundPets });
    } catch (error) {
        // Setup for error handling
    }
});
```

Inside of our `res.json` method, we are sending a JSON response with the key `pets` and the value of `foundPets`. This will send an array of all pets in the database to the client.

### Catch - Errors

Errors can occur in serveral places when querying the database. We need to handle for these errors and send a response to the client that is easy to read and understand. Luckily for us, we have already set up a `catch` block in the `try...catch` statement. We can use this to catch, or sense, any errors that might happen.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
    try {
        const foundPets = await Pet.find();
        res.json({ pets: foundPets });
    } catch (error) {
        // Add error handling code
        res.status(500).json({ error: error.message });
    }
});
```

First we set the status to `500` to indicate that an error has occurred. We need to change the status code from `200` to a meaninging status code if there is an error. Here we can reach for the `500` internal server error. We are choosing `500` because the errors that would happen here are internal errors, or errors that happen with no input from the client. Since this endpoint is not expecting any input from the client, we can assume that any errors that happen are internal.

Then, inside of the error we are being handed, there is a property called `message`. This is a string that describes the error that has occurred. We can use this to send a response to the client.

> 📚 The `error` object is an instance of the built in Error class in JavaScript. It has many useful properties one of them being `message`. For a full list of Error capiblities, check out the [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error).

## 🎓 You do: Test the Index Route with Postman

Now that we have finished the route, let's test it with Postman. Send a `GET` request to `http://localhost:3000/pets`. Test both **success** and **error** messages.

- **Success:** You should receive a JSON response with all of the pets in the database.
- **Error:** You should receive a JSON response with an error message. This message should describe the error that has occurred.

Since we have not added the delete functionality yet, you might not be able to see an empty array response. This is okay, check the structure of the response to make sure it is correct. You will be able to test for the empty array response when we add the delete functionality.

> 🧠 If you are unsure of how to test the failue message here you can simulate an error by creating one yourself. You can do this by adding a `throw` statement on the first line of the `try` block. For example, `throw new Error('This is an error message')`. This will throw an error and the `catch` block will be triggered. This is only for testing purposes and should be removed once you have tested the error message.
