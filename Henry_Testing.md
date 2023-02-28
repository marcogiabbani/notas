# Testing 

## [JEST](https://jestjs.io/)

JEST es un framework de testing de fácil uso pero a la vez con muchisimas posibilidades. Viene con una librería de assertion integrada por lo que no tendremos que configurar ninguna.

### Configuración

Para comenzar a utilizar JEST basta con:

* Instalar la dependencia: `npm install --save-dev jest`
* Ejecutar `jest` o sino agregar script al package.json:

  ```js
  "scripts": {
    "test": "jest"
  }
  ```

Luego ejecutamos `npm test` y ya estaríamos corriendo los tests. El comando jest admite muchas opciones o flags entre los cuales vamos a mencionar los siguientes:

* Correr solo los archivos de tests que matcheen con determinado patron dentro de su nombre: `jest test-pattern`
* Correr un determinado archivo de test mediante su path: `jest path/to/test.js`
* Correr solo UN test mediante su nombre (Ya veremos como definir nombre para los tests): `jest -t name-spec`
* Correr en modo 'watcher': `jest --watch` o `jest --watchAll` (El primero solo correra los tests que fueron afectados por alguna modificación desde la última vez que hicimos cambio en el código)
* Agregar un resumen de cada archivo de test: `jest --verbose` (En el caso de ser un único archivo automáticamente lo hace sin necesidad del flag) 

### Ejemplo (Está dentro de la carpeta demo)

Crearemos un archivo `sum.js` y dentro de él una función `sum` que la exportaremos para poder utilizarla luego en el archivo de tests:

```js
function sum(a, b) {
  return a + b;
}

module.exports = sum;
```

Luego crearemos otro archivo, `sum.test.js` donde definiremos los tests que luego ejecutará JEST:

```js
const sum = require('./sum');

// it === test
it('should return 8 if adding 3 and 5', () => {
  expect(sum(3, 5)).toBe(8);
});

it('should return 15 if adding 7 and 8', () => {
  expect(sum(7, 8)).toBe(15);
});
```

Si ahora ejecutamos `npm test` (Configurar previamente el package.json como mostramos antes) debería ejecutarse los tests.

![Demo Test](/06-Testing/demo-test.png)

Si analizamos la estructura del ejemplo anterior usamos algunas palabras que hasta hoy no conociamos, como por ejemplo `it`, `expect` y `toBe`.

Entendamos para que sirve cada uno de ellas:

* `it` o `test`: nos permiten definir un nuevo test
* `expect`: función de JEST que va a devolver un "expectatio" object sobre el cual luego podremos invocar algunos `matchers`. Explicado más sencillo es lo que estamos ejecutando para probar, por ejemplo `sum(3,5)` arriba estariamos probando la función que creamos pasandole esos dos parámetros y sobre la respuesta vemos si se cumple la condición que queremos o no.
* `toBe`: es un matcher propio de JEST (no es el único de hecho ahora vamos a ver otros) que nos permite hacer una comparación exacta, en este ejemplo entre lo que devolvió la funcion `sum(3,5)` y el valor numérico 8. Si coinciden el test va a pasar y sino no.

### Matchers

JEST tiene distintos matchers para realizar distintas validaciones sobre las funcionalidades que queremos probar:

* `toBe`: igualdad exacta
* `toEqual`: verificación recursiva de cada propiedad del objeto o elemento del arreglo
* `toBeNull`: verifica que el valor sea null
* `toBeUndefined`: verifica que el valor sea undefined
* `toBeDefined`: verifica que el valor sea distinto de undefined
* `toBeTruthy`: verifica que el valor de veracidad sea verdadero sin necesariamente ser literalmente `true`
* `toBeFalse`: verifica que el valor de veracidad sea falso sin necesariamente ser literalmente `false`
* `toBeGreaterThan`: verifica que el valor sea mayor al de referencia
* `toBeGreaterThanOrEqual`: verifica que el valor sea mayor o igual al de referencia
* `toBeLessThan`: verifica que el valor sea menor al de referencia
* `toBeLessThanOrEqual`: verifica que el valor sea menor o igual al de referencia
* `toBeCloseTo`: verifica que el número este a pocos decimales de diferencia del valor de referencia
* `toMatch`: compara contra una expresión regular
* `toContain`: verifica si dentro de un arreglo existe determinado elemento
* `toThrow`: verifica si la función arroja un error

No son los únicos, existen más que pueden consultar en la [documentación oficial de JEST](https://jestjs.io/docs/expect).
Adicionalmente algunos de estos matchers mencionados arriba se encuentran en la demo `matchers.test.js` para que puedan ver como utilizarlos con ejemplos.

### Running Options

#### describe

Podemos también agrupar tests en "categorías" utilizando la palabra `describe`, por ejemplo siguiendo el ejemplo anterior de la suma podríamos tener casos con numeros enteros, otros con números decimales y otro con inputs inválidos:

![Describe Demo](/06-Testing/describe-demo.png)

Es posible armar también subcategorias poniendo describes dentro de otros describes.

#### Skip Tests

En el caso de que algunos tests no queramos que se ejecuten podemos saltearlos de forma individual colocando `xit` en vez de `it` cuando son definidos en el archivo de tests o sino podemos incluso saltear todo un `describe` completo colocando `xdescribe`.

Por ejemplo en el archivo `sum-describe.test.js` de la carpeta demo podríamos modificar alguns tests para que no se ejecuten:

```js
describe('Decimal numbers', () => {
  it('should return 8.33 if adding 3.32 and 5.01', () => {
    expect(sum(3.32, 5.01)).toBe(8.33);
  });
  
  xit('should return 15.82 if adding 7.72 and 8.1', () => {
    expect(sum(7.72, 8.1)).toBe(15.82);
  });
});

xdescribe('Invalid inputs', () => {
  it('should throw an TypeError if first parameter is not a number', () => {
    expect(() => sum('Franco', 5)).toThrow(TypeError);
  });
  
  it('should throw an TypeError if second parameter is not a number', () => {
    expect(() => sum(3, true)).toThrow(TypeError);
  });
});
```

Si observamos ahora la ejecución del comando `npm test sum-describe` veremos que el segundo test del describe de 'Decimal numbers' y todo el describe de 'Invalid inputs' no se van a ejecutar:

![Skip](/06-Testing/skip.png)

#### only

Hay casos en los que solamente queremos probar un tests para evitar que la consola se nos llene de código que no nos estaría interesando en ese momento, para esto JEST tien también una opcion `only` para ejecutar únicamente un test de toda la suite de tests. Volviendo al ejemplo anterior, si solo quisieramos ejecutar el test `should throw an TypeError if first parameter is not a number` podríamos colocarle `it.only` (Para probarlos en la demo saquen los `xit` y `xdescribe`).

```js
...

describe('Invalid inputs', () => {
  it.only('should throw an TypeError if first parameter is not a number', () => {
    expect(() => sum('Franco', 5)).toThrow(TypeError);
  });
  
  it('should throw an TypeError if second parameter is not a number', () => {
    expect(() => sum(3, true)).toThrow(TypeError);
  });
});
```

Ahora al ejecutar `npm test sum-describe` veremos que todo el resto de los tests fueron salteados:

![it only](/06-Testing/it-only.png)

Lo mismo se puede aplicar sobre los `describe` para ejecutar únicamente un grupo de tests.

```js
...

describe.only('Invalid inputs', () => {
  it('should throw an TypeError if first parameter is not a number', () => {
    expect(() => sum('Franco', 5)).toThrow(TypeError);
  });
  
  it('should throw an TypeError if second parameter is not a number', () => {
    expect(() => sum(3, true)).toThrow(TypeError);
  });
});
```

![describe only](/06-Testing/describe-only.png)






> falta asincronico y mocking de info