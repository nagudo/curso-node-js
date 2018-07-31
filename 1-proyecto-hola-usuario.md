# Proyecto Hola Usuario

## Descripción

- Realizar un programa que escriba en un fichero (append) el texto
  
  ```bash
  Hola usuario, tienes 25 años
  ```

- Usuario se obtendrá de la variable de entorno (username)
- La edad se obtendrá de una variable


## Objetivos

- Entender el funcionamiento de los módulos
  - Utilizar modulos del núcleo (os, fs)
  - Crear un módulo sencillo
- Usar sintaxis de ES6 (*destructuring* y *template string*)


## Comenzar proyecto

```bash
mkdir holaUsuario
cd holaUsuario
touch app.js
code .
```

## Añadir texto a un fichero

- Función asíncrona:
  
  ```js
  console.log('Iniciando app');
  const fs = require('fs');
  // fs es un objeto con muchas funciones, ver api
  fs.appendFile('saludo.txt', 'Hola Usuario');
  ```


- Podemos recoger error o éxito con función de callback sería lo suyo para evitar warning:
    ```js
    (node:9493) [DEP0013] DeprecationWarning: Calling an asynchronous function without callback is deprecated.
    ```

- Podríamos utilizar también una función síncrona (no lo haremos)



## Obtener el nombre del usuario

- Utilizaremos el módulo OS para averiguar el nombre del usuario

```js
const os = require('os')
const user = os.userInfo();
console.log(user); // para ver que datos tiene userInfo()
```


### ES6: Destructuring

- Mapeamos una o varias partes de un objeto a una o varias variables:

```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }
```

## ES6: Template Strings

```js
var a = 5;
var b = 10;
console.log(`Fifteen is ${a + b} and not ${2 * a + b}.`);
// "Fifteen is 15 and not 20."
```

## ES6: Object Literal Property Value Shorthand

- Antes (ES5):

  ```js
  function createMonster(name, power) {
    return { type: 'Monster', name: name, power: power };
  }
  ```

- Ahora (ES6):

  ```js
  function createMonster(name, power) {
    return { type: 'Monster', name, power };
  }
  ```

# Solución

```js
const fs = require('fs')
const os = require('os')
const user = os.userInfo()

console.log('Iniciando app')
// fs.appendFile('saludo.txt', `Hola ${user.username}`)
```


## Solución ES6

```js
const fs = require('fs')
const os = require('os')
const { username } = os.userInfo()

console.log('Iniciando app')
fs.appendFile('saludo.txt', `Hola ${username}`)
```

## require
- *require* es un módulo que está en el objeto global
- No necesitamos hacer un require('require');
- Funciona de forma síncrona
  - Por eso se ponen al comienzo
  - Podríamos colocarlos más tarde y hacer *lazy loading*


## Uso de módulos

- Creamos el fichero user.js con el siguiente texto:

```js
console.log('Cargando módulo para el usuario');
```

- ¿Cómo lo cargamos dentro de nuestro app.js?
const user = require ('./user.js')

- Comprobamos la ejecución que muestra el texto del módulo requerido por consola.


## Uso de variables y funciones de otro módulo

- El objeto module tiene muchas propiedades, nos interesará **module.exports**

  ```js
  console.log(module)
  ```

- module.exports puede ser una función o un objeto.
- Será ahí donde tendremos que crear un objeto con el nombre del usuario y la edad.


## Solución proyecto

- Fichero app.js:
  
```js
const fs = require('fs')
const { username, edad } = require('./user')
fs.appendFile('saludo.txt', `Hola ${username}, tienes ${edad} años`)
```

- Módulo user.js:
  
```js
const { username } = require('os').userInfo()
const edad = 25;
module.exports = { username, edad }
```



## Ejercicio

- ¿Qué mostraría el siguiente programa?
- ¿Y si comentamos la primera línea de app.js?

app.js:
```js
require('./module1')
require('./module2')
console.log('Iniciando app')
```

module1.js:
```js
console.log('Ejecutando módulo 1');
```

module2.js:
```js
require('./module1')
console.log('Ejecutando módulo 2');
```

## Solución
- Ejecutando módulo 1 se muestra solo una vez
  - Ya está cargado previamente, se usa la caché y no se ejecuta
  - El texto *Inicializando app* sale después del console.log de los require ya que los require son síncronos.