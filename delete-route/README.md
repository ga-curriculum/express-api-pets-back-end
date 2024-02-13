# ![Express REST API - Delete Route](./assets/tktk-hero.png)

**Learning objective:** By the end of this lesson, students will be able to create a new route to delete a single pet by interacting with the database and send a JSON response to the client.

## DELETE - Delete Route

In this section, we will create a new route to delete a single pet. This route will be a `DELETE` request to `/pets/:petId`, returning a status of `204` with no content.

We will be following these specs when building the route:

- **CRUD Action:** DELETE
- **Method:** `DELETE`
- **Path:** `/pets/:petId`
- **Response:** No Content
- **Success Status Code:** 204
- **Error Status Code:** 404 || 500
- **Error Response Body:** A JSON object with an `error` key and a message describing the error.

## 🎓 You do: Create a DELETE Route

Create a new route to delete a single pet. Using the above specs, create a new route to delete a single pet from the database.

💡 Hints:

- Use the `req.params` object to access the `id` parameter from the path.
- Use the `.findByIdAndDelete()` method on the `Pet` model to delete a single pet from the database.
- You will need a `try...catch` block to handle any errors that may occur.
- You should handle a `404` status code if the pet is not in the database. Refer to the show route to see how to handle this.
- The method `res.end()` can be chained with methods like `res.status()` to send a status code without having to use `res.json()`. Example: `res.status(204).end();`.
- Since we are handling a `404` status code, we must handle a `500` status code. Refer to the show route to see how to handle this.

## Test the Delete Route with Postman

We have created the route, so let's test it to ensure it works. We can do this by sending a `DELETE` request to the route using Postman.

Let's create a new request in Postman to test the route. Open Postman and create a new request with the following details:

- Use the drop-down menu to select `DELETE` as the method.
- Enter the URL `http://localhost:3000/pets/<petId>` in the address bar. If you need to create some more pets, use the `Create Route` to create some pets.
- Click the `Send` button to send the request.

tktk Hunter - Can you take a screenshot of the Postman the above Postman request with highlights around where they need to change/fill in/click?

### Test for Success

After you have sent the request, you should receive a status code of `204` with no content in the response. This means the pet was successfully deleted from the database.

tktk Hunter - Can you take a screenshot of the Postman response with highlights around the status code?

### Test for Error

Let's test for an error. Send another `DELETE` request to the route, but this time use an `id` that does not exist in the database. Since we have just deleted a pet, we can use the same request again without changing anything to test for our `404` status code and error message.

tktk Hunter - Can you take a screenshot of the Postman response with highlights around the status code and error message?

We can test the `500` status code by throwing a new error in the route. Let's add a new line of code to throw a new error in the route and test the route again.

```js
// controllers/pets.js

// DELETE - DELETE - /pets/:petId
router.delete('/:petId', async (req, res) => {
    try {
        throw new Error('This is a test error'); // Add this line

        // Try to delete a single pet code below here
    } catch (error) {
        // Catch block code below here
    }
});
```

Send a `DELETE` request to the route, and you should receive a status code of `500` with an error message in the response.

tktk Hunter - Can you take a screenshot of the Postman response with highlights around the status code and error message?

If you have received something different in this testing process, debug before moving on. Remember to remove the line of code that throws the error before moving on.
