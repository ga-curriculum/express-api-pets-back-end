# ![[tktk Module Name] - tktk Microlesson Name](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

This section should set up the Model with mongoose, parallel to the fruits crud app set up of the model.


```js
// models/pet.js

const mongoose = require('mongoose');

const petSchema = mongoose.Schema({
  name: {
    type: String,
    required: true,
  },
  age: {
    type: Number,
    min: 0,
  },
  breed: String,
});

const Pet = mongoose.model('Pet', petSchema);

module.exports = Pet;

```