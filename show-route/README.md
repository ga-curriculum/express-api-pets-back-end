# ![Express REST API - Index Route](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create a new route to find a single pet, by interacting with the database, and send a JSON response to the client.

## READ - Show Route

In this section, we will create a new route to find a single pet. This route will be a `GET` request to `/pets/:id`, and will return a JSON response with a single pet from the database.

We will be following these specs when building the route:

- **CRUD Action:** READ
- **Method:** `GET`
- **Path:** `/pets/:id`
- **Response:** JSON
- **Success Status Code:** 200
- **Success Response Body:** A JSON object with the pet that matches the `id` parameter.
- **Error Status Code:** 404
- **Error Response Body:** A JSON object with an `error` key and a message describing the error.

Let's get started! 🎉

## Create a GET Route - Show

Inside of `controllers/pets.js`, create a new route to find a single pet. The **method** for this route is `GET`, and the **path** is `/pets/:id`. Use the `.get()` method on the `router` object to create the route and pass in the path and a callback function.

```js
// controllers/pets.js

// READ - GET - /pets/:petId
router.get('/:petId', async (req, res) => {
    // Seting up for our code
});
```

Since we are already in the `/pets` route, we can use `/:petId` as the path. This will allow us to access the `petId` parameter from the `req.params` object.

### Test the Route

Before moving on we need to test this route to make sure it's working. We can do this by sending a response with a message to the client.

```js
// controllers/pets.js

// READ - GET - /pets/:petId
router.get('/:petId', async (req, res) => {
    // Add a message to test the route
    res.json({ message: `Show route with the param ${req.params.petId}` });
});
```

Using Postman to test the route. Open Postman and send a `GET` request to `http://localhost:3000/pets/1`. You should receive a JSON response with the message `Show Route with the param 1`. If you do not receive this message, debug before moving on.

### Try - Find a Single Pet

We have tested the route so let's query the database and find a single pet. We can do this by using the `.findById()` method on the `Pet` model. We will use `await` to wait for the database to return the pet. However, we need to wrap this in a `try...catch` block to handle any errors that may occur.

```js
// controllers/pets.js

// READ - GET - /pets/:petId
router.get('/:petId', async (req, res) => {
    try {
        // Add query to find a single pet
        const foundPet = await Pet.findById(req.params.petId);
        res.json({ pet: foundPet });
    } catch (error) {
       // Setup for error handling
    }
});
```

Just like in our test above we are using the `req.params.petId` to access the `petId` parameter from the `req.params` object. However, we have to handle for the case where the `petId` does not exist in the database. 

In Mongoose, if a document is not found, the `.findById()` method will return `null` but not throw an error. We need to handle this case and send a `404` status code with a message to the client.

We can do this by checking if `foundPet` is `null`, we set the status to `404` and throw a new error with the message `Pet not found`.

```js
// controllers/pets.js

// READ - GET - /pets/:petId
router.get('/:petId', async (req, res) => {
    try {
        const foundPet = await Pet.findById(req.params.petId);
        // Add error handling if a pet is not found
        if (!foundPet) {
            res.status(404);
            throw new Error('Pet not found.');
        }
        res.json({ pet: foundPet });
    } catch (error) {
        // Setup for error handling
    }
});
```

> tktk book emoji Since `res.status` just sets the status of the current response object, we need a way to stop the execution of the function. We can do this by throwing a new error with the message `Pet not found`. This will be caught in the `catch` block and we can send a response to the client.

## Catch - Errors

We have set up a `catch` block in the `try...catch` statement and we have coded for the case where a pet is not found. However we need to handle for any other errors that might occur. 

First we want to check to see if the error we are catching is a `404` status code. If this error occurs we have already set the status code so we just need to send a response to the client.

```js
// controllers/pets.js

// READ - GET - /pets/:petId
router.get('/:petId', async (req, res) => {
    try {
        const foundPet = await Pet.findById(req.params.petId);
        if (!foundPet) {
            res.status(404);
            throw new Error('Pet not found.');
        }
        res.json({ pet: foundPet });
    } catch (error) {
        // Add error handling code for 404 errors
        if (res.statusCode === 404) {
            res.json({ error: error.message });
        }
    }
});
```

Now we need to handle for any other errors that might occur. We can do this by setting the status code to `500` and sending a response to the client with a message describing the error.

```js
// controllers/pets.js

// READ - GET - /pets/:petId
router.get('/:petId', async (req, res) => {
    try {
        const foundPet = await Pet.findById(req.params.petId);
        if (!foundPet) {
            res.status(404);
            throw new Error('Pet not found.');
        }
        res.json({ pet: foundPet });
    } catch (error) {
        if (res.statusCode === 404) {
            res.json({ error: error.message });
        } else { // Add else statement to handle all other errors
        res.status(500).json({ error: error.message });
        }
    }
});
```

We do this because if we do not and let's say we have a database error, the client will receive a `200` status code with their error message. This is not good because the client can become confused and not know if they were successful or not.

## tktk hat emoji You do: Test the Index Route with Postman

Use Postman to test the new route. Open Postman and send a `GET` request to `http://localhost:3000/pets/<pet object id>`. Test both **success** and **error** cases.

- **Success:** You should receive a JSON response with the pet that matches the `petId` parameter.
- **Error**: You should receive a JSON response with an `error` key and a message describing the error.

To get a `petId` you can use the Index requet to find all pets and use the `id` of one of the pets.

> tktk brain emoji To test the `404` Not Found case, use a `petId` that does not exist in the database. You can do this by changing the last digit of the `petId` to a number that does not exist in the database.
