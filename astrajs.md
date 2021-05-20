## Connecting the Database

Let's briefly dive into the connection between our serverless functions and our Astra DB.
We are using the `@astrajs/collections` library to make the connection using the Document API provided by Stargate. To do so, we start by creating a 'client'.

``` javascript
const { createClient } = require("@astrajs/collections");

let astraClient = null;

const getAstraClient = async () => {
  if (astraClient === null) {
    astraClient = await createClient(
      {
        astraDatabaseId: process.env.ASTRA_DB_ID,
        astraDatabaseRegion: process.env.ASTRA_DB_REGION,
        applicationToken: process.env.ASTRA_DB_APPLICATION_TOKEN,
      },
      30000
    );
  }
  return astraClient;
};
```

Here we are defining a new method called `getAstraClient` that uses the `createClient` method from our `astrajs` library to create a connection to our database. We then provide it the needed database credentials we added to our environment varaiables earlier;

- `ASTRA_DB_ID`
- `ASTRA_DB_REGION`
- `ASTRA_DB_APPLICATION_TOKEN`

Then we return the `astraClient` we can then use in our API calls.

We also need to create a document collection to store our data.

``` javascript
const getCollection = async () => {
  const documentClient = await getAstraClient();
  return documentClient
    .namespace(process.env.ASTRA_DB_KEYSPACE)
    .collection("tktkposts");
};
```

In this method, we are using our previously created `getAstraClient` method to initialize the database connection, and then create a collection in the keyspace we defined in our environment variables;

- `ASTRA_DB_KEYSPACE`

We will call the collection "tktkposts".

So now, any time we want to perform operations on our data, we will reference this method `getCollection`, and use the Document API from Stargate to do so.

## Document API

For our TikTok app, we will not be dealing with the Document API directly. Instead `@astrajs/collections` does that for us, and provides us with easy to use methods.

If you want a comprehensive list of the capabilities of `@astrajs/collections`, check out this documentation: [AstraJS Collections](https://docs.datastax.com/en/astra/docs/astra-collection-client.html)

For now, let's go over the 3 methods we'll be using in this app:

- `create`
- `update`
- `find`

The `create` method is used when we want to add documents to our collection. For example, in `functions/add.js` we get our collection from the database using our `getCollection` method.

``` javascript
const users = await getCollection();
```

Then we use the `create` method to create a document, providing the collection id, and body of the document.

``` javascript
try {
    const user = await users.create(id, event.body);
    return {
      statusCode: 200,
      body: JSON.stringify(user),
    };
}
```

The `update` method is used to update portions of existing documents. Take a look at `functions/edit.js`. Again we use `getCollection()` to get our collection from the database, then we use the `update` method, provide it with an id for the document we want to edit, and the data that needs updating.

``` javascript
try {
    users.update(body.userId, body.data);
    return {
      statusCode: 200,
    };
  }
```

And finally the `find` method is used to retrieve documents. In `functions/posts.js` we are again using `getCollections()` and using the `find` method on the result.

``` javascript
try {
    const res = await users.find({});
    return {
      statusCode: 200,
      body: JSON.stringify(Object.keys(res).map((i) => res[i])),
    };
  }
```

In this case, we are passing an empty object to retrieve all documents. In a real-world scenario, we would pass a qualifier to get only the documents relevant to a specific user.