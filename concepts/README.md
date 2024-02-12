# ![[tktk Module Name] - Concepts](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

## REST APIs

An API (application programming interface) is an interface that facilitates communication between clients and resources. 

RESTful API is an interface that two computers use to exchange information over the internet. 

As you can tell, APIs have a pretty broad definition; as a result, they can also be configured in a multitude of different ways. That's why many developers adapt REST conventions when building their APIs. 

REST, short for REpresentational State Transfer, is a guiding architectural style for web applications, focusing on the client-server relationship. It's a set of principles that simplifies how web resources are defined and addressed. 

For this lesson, we'll be creating a RESTful API that handles [CRUD](https://developer.mozilla.org/en-US/docs/Glossary/CRUD) of a Pets resource. Using restful routing conventions, we can expect the following: 

| HTTP Method | Controller | Response | URI | Use Case |
| --- | --- | --- | --- | --- |
| POST | `create` | 200 | `/pets` | Create a pet |
| GET | `index` | 200 | `/pets` | List pets |
| GET | `show` | 200 | `/pets/:petId` | Get a single pet |
| PUT | `update` | 200 | `/pets/:petId` | Update a pet |
| DELETE | `deletePet` | 204 | `pets/:petId` | Delete a blog |