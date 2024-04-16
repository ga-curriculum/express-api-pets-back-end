# ![Express REST API - Pets - Index Route](./assets/hero.png)

**Learning objective:** By the end of this lesson, students can create an index route to find all pets by interacting with the database and send a JSON response to the client.

## READ - Index Route

In this section, we will create a new route to find all pets. This route will be a `GET` request to `/pets`, and will return a JSON response with all the pets in the database.

We will be following these specs when building the route:

- **CRUD Action:** READ
- **Method:** `GET`
- **Path:** `/pets`
- **Response:** JSON
- **Success Status Code:** 200
- **Success Response Body:** An array of all the pets in the database named `pets`. The array will be empty if there are no pets in the database.
- **Error Status Code:** 500
- **Error Response Body:** A JSON object with an `error` key and a message describing the error.

Let's get started! 🎉

## Create a GET Route - Index

Inside `controllers/pets.js`, create a new route to find all pets. The **method** for this route is `GET`, and the **path** is `/pets`. Use the `.get()` method on the `router` object to create the route and pass in the path and a callback function.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
  // Setting up for our code
});
```

### Test the Route

We must test the route before we can query the database to ensure it works. We can do this by sending a response with a message to the client.

```js
// controllers/pets.js

// READ - GET - /pets
router.get('/', async (req, res) => {
  // Add a message to test the route
  res.json({ message: 'Index Route' });
});
```

Let's use Postman to test the route. Open Postman and send a `GET` request to `http://localhost:3000/pets`. You should receive a JSON response with the message `Index Route`. If you do not receive this message, debug before moving on.

### Try - Find All Pets

We have tested the route, so let's query the database and find all pets.

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

First, we set the status to `500` to indicate that an error has occurred. We need to change the status code from `200` to a meaningful status code if there is an error. Here, we can reach for the `500` internal server error. We are choosing `500` because the errors that would happen here are internal or errors that occur with no input from the client. Since this endpoint is not expecting any input from the client, we can assume that any errors that happen are internal.

## 🎓 You do: Test the Index Route with Postman

Now that we have finished the route let's test it with Postman. Send a `GET` request to `http://localhost:3000/pets`. Test both **success** and **error** messages.

- **Success:** You should receive a JSON response with all the pets in the database.
- **Error:** You should receive a JSON response with an error message. This message should describe the error that has occurred.

Since we have yet to add the delete functionality, you might not be able to see an empty array response. This is okay; check the response's structure to ensure it is correct. When we add the delete functionality, you can test for the empty array response.

> 🧠 If you are unsure how to test the failure message here, you can simulate an error by creating one yourself. You can add a `throw` statement on the first line of the `try` block. For example, `throw new Error('This is an error message')`. This will throw an error, triggering the `catch` block. This is only for testing purposes and should be removed once you have tested the error message.
