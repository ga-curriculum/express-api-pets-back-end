# ![Express REST API - Concepts](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will understand RESTful APIs.

## REST APIs

A web API (application programming interface) is an interface that facilitates communication between clients and resources. An API acts as an intermediary 

As you can tell, APIs have a pretty broad definition; as a result, they can also be configured in a multitude of different ways. As APIs have become increasingly prevalent, various API protocols have been established to help provide users with a set of defined rules. One popular API protocol is REST. 

REST, short for REpresentational State Transfer, is a guiding architectural style for web applications, focusing on the client-server relationship. It's a set of principles that simplifies how web resources are defined and addressed. 

For this lesson, we'll be creating a RESTful API that handles [CRUD](https://developer.mozilla.org/en-US/docs/Glossary/CRUD) of a Pets resource. Using restful routing conventions, we can expect the following: 

| HTTP Method | Controller | Response | URI | Use Case |
| --- | --- | --- | --- | --- |
| POST | `create` | 200 | `/pets` | Create a pet |
| GET | `index` | 200 | `/pets` | List pets |
| GET | `show` | 200 | `/pets/:petId` | Get a single pet |
| PUT | `update` | 200 | `/pets/:petId` | Update a pet |
| DELETE | `deletePet` | 204 | `pets/:petId` | Delete a pet |