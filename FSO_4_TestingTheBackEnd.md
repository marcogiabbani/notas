# Test environment

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

### Tengo que hacer lo siguiente:

1. Modelo de notas
2. Controller de notas
3. Importar notasRouter a app.js
4. Prbar que funcione el alta, baja y consulta.
5. Elaborar el archivo de tests para las notras.
6. Refactorizar el archivo de tests para blogs


















<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


```shell

```
```json

```