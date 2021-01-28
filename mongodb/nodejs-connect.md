# Node.js connection to opeNode Mongodb addon

After creating a Mongodb addon with the default configurations, here is sample Node.js server connecting 
to the Mongodb addon server:

    const http = require("http");


    // MONGODB
    const MongoClient = require('mongodb').MongoClient;
    // Connection URL
    const url = 'mongodb://root:123456@mongodb:27017';

    // Database Name
    const dbName = 'mydb';

    // Use connect method to connect to the server

    MongoClient.connect(url, function(err, client) {
      if (!err) {
        console.log("Connected successfully to server");

        const db = client.db(dbName);

        const collection = db.collection('mydocuments');
        // Insert some documents
        collection.insertMany([
          {a : 1}, {a : 2}, {a : 3}
        ], function(err, result) {
          if (!err) {
            console.log("Inserted 3 documents into the collection");
          } else {
            console.log(err)
          }

        });

        client.close();

      } else {
        console.log("failed to connect")
        console.log(err);
        process.exit(1);
      }
    });

    const server = http.createServer(function (req, res) {
      if (req.url === '/status') {
        res.end(`Hello /status ${new Date()}`);

      }
      else {
        res.end(`!!Hello opeNode World`);
      }
    }).listen(process.env.PORT || 80, (err) => {
      if ( ! err) {
        console.log(`node server listening on port 80...`, new Date())
      }
    })

The following package.json can be used:

    {
      "name": "openode-hello-mongo",
      "version": "1.0.0",
      "description": "#",
      "main": "server.js",
      "dependencies": {
        "mongodb": "^3.6.2",
      },
      "devDependencies": {},
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "node server.js"
      },
      "author": "",
      "license": "ISC"
    }
