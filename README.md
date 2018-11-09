# mongolab-data-api-promise

`mongolab-data-api` is a node.js module designed to allow you to access [mLab's Data API](http://docs.mlab.com/data-api/#reference) with minimal overhead.

I designed mongolab-data-api-promise so the only documentation you need, in addition to the Data API specification, is how to install node, how to install the module, and how to make a call. Also, this module and this documentation is based off of [mongolab-data-api](https://github.com/gamontal/mongolab-data-api) by [gamontal](https://github.com/gamontal). Their version is no longer being maintained and sync oriented. If you need a synced version, be sure to check it out their version. 

## Installation

Using [npm](https://www.npmjs.com/):

    $ npm install mongolab-data-api-promise

If you don't have or don't want to use npm:

    $ cd ~/.node_modules
    $ git clone git://github.com/Electrox-Qui-Mortem/mongolab-data-api-promise.git

## Usage

To require the library and initialize it with your account API key:

```javascript
var mLab = require('mongolab-data-api-promise')('<Your Api Key Here>');
```

### Examples

**List databases**

```javascript
mLab.listDatabases()
    .then(databases => {
        console.log(data); // => [db1, db2, db3, ...]
    }, err => {
        console.log(err)
    })
;
```

**List collections**

```javascript
mLab.listCollections('exampledb')
    .then(collections => {
        console.log(collections); // => [coll1, coll2, ...]
    }, err => {
        console.log(err)
    });
```

**List documents**

```javascript
var opt = {
    database: 'exampledb',
    collectionName: 'examples',
    query: '{ "key": "value" }'
};

mLab.listDocuments(options, function (err, data) {
  console.log(data); //=> [ { _id: 1234, ...  } ]
});
```
### Methods

#### `listDatabases`

Get the databases linked to the authenticated account. Returns an array

`.lastDatabases(callback)`

#### `listCollections`

Get the collections in the specified database. Returns an array.

`.listCollections(database)`

***Parameters:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |

#### `listDocuments`

Get the documents in the specified collection. Returns an array.

`.listDocuments(options)`

***Options:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |
collectionName| MongoDB collection name | `String` | Yes |
query| restrict results by the specified JSON query | `String` | No |
resultCount| return the result count for this query | `Boolean` | No |
setOfFields| specify the set of fields to include or exclude in each document (1 - include; 0 - exclude) | `Object` | No |
findOne| return a single document from the result set (same as findOne() using the mongo shell) | `Boolean` | No |
sortOrder| specify the order in which to sort each specified field (1- ascending; -1 - descending) | `String` | No |
skipResults| number of documents to skip | `Number` | No |
limit| number of documents to return | `Number` | No |

#### `insertDocuments`

Create a new document in the specified collection. Returns nothing.

`.insertDocuments(options)`

***Options:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |
collectionName| MongoDB collection name | `String` | Yes |
documents| a document or array of documents to be inserted| `Object/Array` | Yes |

#### `updateDocuments`

Update one or more documents in the specified collection. Returns nothing

`.updateDocuments(options)`

***Options:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |
collectionName| MongoDB collection name | `String` | Yes |
data| replacement document or update modifiers | `Object` | Yes |
query| only update document(s) matching the specified JSON query | `String` | No |
allDocuments| update all documents collection or query (if specified). By default only one document is modified | `Boolean` | No |
upsert| insert the document defined in the request body if none match the specified query | `Boolean` | No |

#### `deleteDocuments`

Replace the contents of some or all documents of a collection. Returns nothing

`.deleteDocuments(options)`

***Options:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |
collectionName| MongoDB collection name | `String` | Yes |
query| only replace the document(s) matching the specified JSON query | `String` | No |

#### `viewDocument`

View a single document. Returns nothing

`.viewDocument(options)`

***Options:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |
collectionName| MongoDB collection name | `String` | Yes |
id| the document's id | - | Yes |

#### `updateDocument`

Update a single document. Returns nothing

`.updateDocument(options)`

***Options:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |
collectionName| MongoDB collection name | `String` | Yes |
id| the document's id | - | Yes |
updateObject| object sent as replacement | `Object` | Yes |

#### `deleteDocument`

Delete a single document. Returns nothing

`.deleteDocument(options)`

***Options:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |
collectionName| MongoDB collection name | `String` | Yes |
id| the document's id | - | Yes |

#### `runCommand`

Run a MongoDB database command. Returns nothing

`.runCommand(options)`

***Options:***

Name | Description | Type | Required |
-----|------------ |------|:----------:|
database| MongoDB database name | `String` | Yes |
commands| MongoDB database command | `Object` | Yes |

### Notes
- **Creating a new collection**
  - As soon as you POST your first document you should see the collection appear
- **runCommands**
  - Only certain MongoDB commands are exposed through the Data API
  - The available commands are:
    - getLastError
    - getPrevError
    - ping
    - profile
    - repairDatabase
    - resetError
    - whatsmyuri
    - convertToCapped
    - distinct
    - findAndModify
    - geoNear
    - reIndex
    - collStats
    - dbStats
    
## Requirements
- [mongolab-data-api-promise](https://github.com/Electrox-Qui-Mortem/mongolab-data-api-promise/)
- [mLab](https://mlab.com/) account w/API key.
- [node.js](https://nodejs.org/en/download/) v8.0+ (This module was made in this version)
- [request](https://www.npmjs.com/package/request) 2.88.0
- [requst-promise](https://www.npmjs.com/package/request-promise) 4.2.2

## Disclaimer

### [From the official mLab Data API documentation](http://docs.mlab.com/connecting/#methods):

> mLab databases can be accessed by your application code in two ways.
> The first method - the one we strongly recommend - is to connect using one of the MongoDB drivers (as described above). You do not need to use our API if you use the driver. In fact, using a driver provides better performance, better security, and more functionality.

> The second method is to connect via mLab’s RESTful Data API. Use this method only if you cannot connect using a MongoDB driver.

> ***Visit mLab's official documentation if you have any security concerns about using the Data API***

## Contributions

If you want anything added, you can send me an email  [here](mailto:quimortemking@gmail.com). Also, if you want to add anything yourself or see a problem with the module, you can create a pull request or open an issue. I'll try to fix it within a week. Maybe less!

## License

[MIT](https://github.com/Electrox-Qui-Mortem/mongolab-data-api-promise/blob/master/LICENSE.md) © Jermahl White