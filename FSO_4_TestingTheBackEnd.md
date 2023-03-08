# Testing the backend

## Test environment

It is common practice to define separate modes for development and testing.
```json
{
  // ...
  "scripts": {
    "start": "NODE_ENV=production node index.js",
    "dev": "NODE_ENV=development nodemon index.js",
    "build:ui": "rm -rf build && cd ../frontend/ && npm run build && cp -r build ../backend",
    "deploy": "fly deploy",
    "deploy:full": "npm run build:ui && npm run deploy",
    "logs:prod": "fly logs",
    "lint": "eslint .",
    "test": "NODE_ENV=test jest --verbose --runInBand"  },
  // ...
}
```
We also added the runInBand option to the npm script that executes the tests. This option will prevent Jest from running tests in parallel.

There is a slight issue in the way that we have specified the mode of the application in our scripts: it will not work on Windows. We can correct this by installing the cross-env package as a development dependency with the command:

```bash
npm install --save-dev cross-env

```
We can then achieve cross-platform compatibility by using the cross-env library in our npm scripts defined in package.json:

```json
{
  // ...
  "scripts": {
    "start": "cross-env NODE_ENV=production node index.js",
    "dev": "cross-env NODE_ENV=development nodemon index.js",
    // ...
    "test": "cross-env NODE_ENV=test jest --verbose --runInBand",
  },
  // ...
}
```
NB: If you are deploying this application to Fly.io/Render, keep in mind that if cross-env is saved as a development dependency, it would cause an application error on your web server. To fix this, change cross-env to a production dependency by running this in the command line:

```
npm install cross-env
```
<br>

## Supertest

Let's use the supertest package to help us write our tests for testing the API.
```shell
npm install --save-dev supertest
```
> **Leí y copié todo el código se 'Supertest', pero no agregué nada a esta nota ni rellene la base de datos de prueba**

<br>


## Initializing the database before tests

Let's initialize the database before every test with the beforeEach function and modify the tests:

```js
  const mongoose = require('mongoose')
  const supertest = require('supertest')
  const app = require('../app')
  const api = supertest(app)
  const Note = require('../models/note')

  const initialNotes = [
    {    
      content: 'HTML is easy',
      important: false,  },
    {    
      content: 'Browser can execute only JavaScript',    
      important: true,  
    },
  ]

  beforeEach(async () => {  
    await Note.deleteMany({})  
    let noteObject = new Note(initialNotes[0])  
    await noteObject.save()  
    noteObject = new Note(initialNotes[1])  
    await noteObject.save()
  })

  test('all notes are returned', async () => {
    const response = await api.get('/api/notes')
    expect(response.body).toHaveLength(initialNotes.length)
  })

  test('a specific note is within the returned notes', async () => {
    const response = await api.get('/api/notes')
    const contents = response.body.map(r => r.content) 
    expect(contents).toContain('Browser can execute only JavaScript')
  })

  afterAll(async () => {
    await mongoose.connection.close()
  })  

```

<br>

## Running tests one by one
By folder, description or block:
```shell
$  # by file
$  npm test -- tests/note_api.test.js 
$  # by name or block description
$  npm test -- -t "a specific note is within the returned notes" 
$  # by partial name
$  npm test -- -t 'notes'

``` 
<br>

## <u> **Eliminate try-catch** </u>
 ```shell
npm install express-async-errors
``` 
Require it in app.js file
 ```js
const config = require('./utils/config')
const express = require('express')
require('express-async-errors')const
app = express()
const cors = require('cors')
const notesRouter = require('./controllers/notes')
const middleware = require('./utils/middleware')
const logger = require('./utils/logger')
const mongoose = require('mongoose')

// ...

module.exports = app
``` 

<u> **Now the magic happpens:** </u>

 ```js
notesRouter.delete('/:id', async (request, response, next) => {
  try {
    await Note.findByIdAndRemove(request.params.id)
    response.status(204).end()
  } catch (exception) {
    next(exception)
  }
})
```

&emsp;&emsp;&emsp;&emsp;&emsp;becomes:

 ```js
notesRouter.delete('/:id', async (request, response) => {
  await Note.findByIdAndRemove(request.params.id)
  response.status(204).end()
})
```

> Because of the library, we do not need the next(exception) call anymore. The library handles everything under the hood. If an exception occurs in an async route, the execution is automatically passed to the error handling middleware.

> ***What if I want some special thing happening in case of an error? may I over-write this library work with a simple next in mu route??***

<br>

## Promise-all

We want to optimize the data load to the database before running tests, but a `forEach()` function causes some trouble due to JS asynchronous nature. So we opt for `Promise.all` method:

```js
beforeEach(async () => {
  await Note.deleteMany({})

  const noteObjects = helper.initialNotes
    .map(note => new Note(note))
  const promiseArray = noteObjects.map(note => note.save())
  await Promise.all(promiseArray)
})
```

<br>

## Refactoring tests




<br>


# User Administration

<br>
<br>
<br>
<br>
<br>


```shell

```
```json

```