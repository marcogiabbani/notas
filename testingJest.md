adding to the packageJson
```json
  "jest": {
    "testEnvironment": "node"
  }
```
and to .eslintrc
```json
"env": {
        "browser": true,
        "commonjs": true,
        "es2021": true,
        "jest": true //for jest library
    },
```

Another way of running a single test (or describe block) is to specify the name of the test to be run with the -t flag:

```bash
npm test -- -t 'when list has only one blog, equals the likes of that'
```
