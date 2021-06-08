## GraphQL

---

> Why does GraphQL exist?

### Rest-ful Routing:

---

    -   Given a collection of records on a server, there should be a uniform URL and HTTP request method used to utilize that collection of records
    -   CRUD functionality (Create, Read, Update, Destroy)(POST, GET, PUT/PATCH, DELETE)

Example:

```
URL         Method      Operation
/users      POST        Create a new user
/users      GET         Fetch a list of all users
/users/23   GET         Fetch details on user with ID 23
/users/23   PUT         Update details of user with ID 23
/users/23   DELETE      Delete user with ID 23
```

### Shortcomings of Rest-ful routing:

---

-   Deep nesting can cause issues when showing relational data between structures and multiple users
-   Getting around these issues severely breaks rest-ful conventions in some cases
-   Trying to get around these issues can cause a severe over serve of data to the client

> What is GraphQL?

-   It tries to solve the shortcomings of rest-ful routing
-   Attempts to only pull the data needed for the need instead of entire queries then only using part of the data

> How do we use GraphQL?

-   Normally express just takes a response and sends it back, however with graphql it asks if the request is asking for graphql and if it does it passes the request to graphql to handle before receiving it back to the client

## Install

---

1. Traverse to the file you want to put it in then run the command:

```
$ npm init
$ npm install --save express express-graphql graphql lodash
```

2. Set up the file to use graphQl:

```javascript
const express = require("express");
const { graphqlHTTP } = require("express-graphql");

const app = express();
app.use("/graphql", graphqlHTTP({ graphiql: true }));

app.listen(4000, () => {
    console.log("Listening");
});
```
