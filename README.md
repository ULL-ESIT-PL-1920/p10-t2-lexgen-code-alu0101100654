## P10-T2-LEXGEN-CODE-ALU0101100654
Este fichero describe los aspectos fundamentales del módulo ***p10-t2-lexgen-code-alu0101100654***.
![Package CI](https://github.com/ULL-ESIT-PL-1920/p10-t2-lexgen-testing-alu0101100654/workflows/Package%20CI/badge.svg)

---
## Introducción
***p10-t2-lexgen-code-alu0101100654*** es un módulo que está orientado al análisis léxico de cualquier lenguaje siempre y cuando se proveen una descripción del lenguaje adecuada en forma de **tokens**.

---
## Usage
### Requisitos previos
Para la instalación y utilización de este módulo, no es necesario que el usuario cumpla con ningún requisito.
### Funcionalidad principal
La funcionalidad principal del módulo radica en la única función que posee y exporta, `buildLexer`, la cual crea y devuelve una función que es capaz de realizar el análisis léxico del lenguaje especificado. La ejecución de esta función con un fragmento de código **válido**, generará una lista de tokens (en la pŕactica, un array de objetos) donde cada uno de ellos se autodefine como un par **tipo-valor**. 
### Ejemplo de uso.
1. Primero se crea la especificación del lenguaje. Esta debe ser un ***array de arrays***, donde cada array interno es un par **tipo-expresión regular**. La expresión es aquella que define a los elementos del lenguaje del tipo especificado en el par.
2. Se llama a la función `buildLexer`, pasándole como argumento en el array definido en el paso anterior. Se obtendrá como valor de retorno una función que realiza el **análisis léxico** del lenguaje especificado.
3. Utilice la función generada paśando como argumento una cadena de texto cuyo contenido es el código a analizar.

```js
const buildLexer = require('./@ULL-ESIT-PL-1920/p10-t2-lexgen-code-alu0101100654.js');
const SPACE = /(?<SPACE>\s+|\/\/.*)/;
const RESERVEDWORD = /(?<RESERVEDWORD>\b(const|let)\b)/;
const ID = /(?<ID>\b([a-z_]\w*))\b/;
const STRING = /(?<STRING>"([^\\"]|\\.")*")/;
const OP = /(?<OP>[+*\/=-])/;

const myTokens = [
  ['SPACE', SPACE], ['RESERVEDWORD', RESERVEDWORD], ['ID', ID],
  ['STRING', STRING], ['OP', OP]
];

const lexer = buildLexer.buildLexer(myTokens);

const str = 'const varName = "value"';

console.log(lexer(str)); /* [
  { type: 'RESERVEDWORD', value: 'const' },
  { type: 'ID', value: 'varName' },
  { type: 'OP', value: '=' },
  { type: 'STRING', value: '"value"' }
] */
```
Se ha puesto como un comentario, la salida esperada que generaría la llamada a `console.log`.

---
### Consideraciones
Existe ciertas condiciones que pueden darse durante la utilización del módulo y que se constituyen, en esencia, como comportamientos fuera del usual/esperado. Estas, son las siguientes:
* Cualquier token que se defina será considerado válido, a excepción de que se cree uno denominado ***ERROR***. Este token existe internamente en la función generada por `buildLexer` y es utilizado por la función que genera cuando detecta un elemento que no coincide con los especificados. Si en la especificación del lenguaje se pasa este tipo de elemento, `buildLexer` creará igualmente la función analizadora, pero esta no podrá ser nunca ejecutada de ninguna manera posible, puesto que se lanzará una excepción con cada ejecución.
* Si se detecta un elemento no definido durante la ejecución de la función analizadora, se creará un token **ERROR** cuyo valor será todo el código restante que no ha sido analizado, incluyendo el elemento que ocasionó el error.

---
## Documentación adicional.
Puede consultarse la documentación oficial del módulo a través de este [enlace](http://www.limni.net).