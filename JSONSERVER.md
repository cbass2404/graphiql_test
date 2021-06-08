## Json-server

---

A quick easy way to create a server and use fake test data to test your app.

### Install and Use

---

1. In the terminal run the command:

```
$ npm install --save json-server
```

2. In your package.json scripts section add:

```json
"json:server": "json-server --watch db.json"
```

3. Create a db.json file and populate it with fake json style data:

```json
{
    "users": [
        { "id": "23", "firstName": "Bill", "age": 20 },
        { "id": "40", "firstName": "Alex", "age": 40 }
    ]
}
```

4. In your terminal run the command:

```
$ npm run json:server
```
