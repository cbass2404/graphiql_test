## GraphQL

---

_To make a quick test server use [json-server](https://github.com/typicode/json-server)_

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

-   It can be used to make http requests to an outside api

## Install and Use

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

3. setup schema file:

```javascript
const graphql = require("graphql");
const { GraphQLObjectType, GraphQLString, GraphQLInt } = graphql;

const UserType = new GraphQLObjectType({
    name: "User",
    fields: {
        id: { type: GraphQLString },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt },
    },
});
```

-   name field tells graphql the name of the model
-   fields tells graphql the data structure of that model
    -   You must tell graphql what the type of the data is inside fields like above

4. Setup rootquery so graphql knows where to start at:

```javascript
const RootQuery = new GraphQLObjectType({
    name: "RootQueryType",
    fields: {
        user: {
            type: UserType,
            args: { id: { type: GraphQLString } },
            resolve(parentValue, args) {
                return _.find(users, { id: args.id });
            },
        },
    },
});
```

-   the most important part of the rootquery is the resolve function (parentValue rarely used)

5. Merge into graphql object and export the schema into main file:

```javascript
module.exports = new GraphQLSchema({
    query: RootQuery,
});
// index.js
const express = require("express");
const { graphqlHTTP } = require("express-graphql");
const schema = require("./schema/schema");

const app = express();
app.use("/graphql", graphqlHTTP({ schema, graphiql: true }));

app.listen(4000, () => {
    console.log("Listening");
});
```

6. At this point going to localhost:port#/graphql should pull up a user interface graphiql tool:

-   documentation explorer grows as you add more schema to the server
-   you can write queries in the interface to see how data is structured in graphql

```javascript
{
  user(id: "23"){
    id, firstName, age
  }
}
```

-   line 2 specifies the field object, with the argument to find it by id: "23"
-   line 3 tells it which fields you want returned from that object
-   you have to enter some kind of argument in queries or it will not work

7. To use an outside API:
