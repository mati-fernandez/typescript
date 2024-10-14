[[React]]
[[Informática/Programación/Web/JavaScript|JavaScript]]
[Video 1](https://www.youtube.com/watch?v=fUgxxhI_bvc&list=PLUofhDIg_38pNCZe9ptj72exbFjzyePxT)
[Video 2](https://www.youtube.com/watch?v=L1ZSk-vPVKI&list=PLUofhDIg_38pNCZe9ptj72exbFjzyePxT&index=2)
[Playground](https://www.typescriptlang.org/play/)
# Introducción
TypeScript es un superset de JS. Osea tiene la sintaxis de JS como base y la extiende.

[[VS Code]] está en parte desarrollado en TS, y TypeScript mejora la experiencia de desarrollo. Por ejemplo, permite al IDE auto completar en situaciones como `name.` al sugerir los métodos aplicables a un string, asumiendo que `name` es de tipo string.

## cual es la gracia de TS si solo hace las validaciones en tiempo de desarrollo y no de compilación o de ejecución?

La "gracia" de TypeScript radica en que ayuda a prevenir errores antes de que ocurran, mejorando la calidad del código a largo plazo. Aunque las validaciones de tipos se hacen solo en tiempo de desarrollo y no en ejecución, hay varias ventajas clave:

1. Detección temprana de errores: TypeScript te avisa de errores en el código mientras lo estás escribiendo, lo que reduce la posibilidad de encontrarte con fallos inesperados al ejecutar tu programa.

2. Mejor autocompletado y documentación: Al tener tipos explícitos, el IDE puede sugerirte métodos y propiedades adecuados, lo que te ahorra tiempo y ayuda a escribir código más rápido y correcto.

3. Código más mantenible: Cuando trabajás en proyectos grandes o colaborativos, los tipos actúan como una especie de "contrato" que hace que el código sea más predecible y fácil de entender.

4. Prevención de bugs: Muchísimos bugs que ocurren en JavaScript son por no manejar correctamente tipos (por ejemplo, intentando operar con undefined o null). TypeScript reduce drásticamente estos problemas.

5. Refactorización más segura: Si cambias una parte de tu código, TS te ayuda a asegurarte de que el resto del programa sigue funcionando correctamente, lo cual es clave en proyectos grandes.

Aunque no haga las validaciones en tiempo de ejecución, TS ofrece una experiencia de desarrollo más robusta y confiable. Aún así, el manejo de validaciones de entrada y errores en tiempo de ejecución sigue siendo algo que debes manejar en tu lógica de JavaScript.

## entonces TS funciona en tiempo de desarrollo o de compilación? o son lo mismo?

TypeScript realiza sus verificaciones en tiempo de desarrollo y en tiempo de compilación, pero no durante la ejecución.

- Tiempo de desarrollo: Mientras escribís el código, el editor (como VS Code) te brinda retroalimentación inmediata, como autocompletado, sugerencias de tipos, y alertas sobre posibles errores de tipo.

- Tiempo de compilación: Cuando compilas el código TypeScript a JavaScript (con tsc o algún proceso de build), TypeScript hace una verificación más exhaustiva de los tipos. Si hay errores de tipos, te lo avisa antes de generar el JavaScript.

Ambos términos son casi intercambiables en este contexto, ya que durante el desarrollo también estás en un ciclo constante de compilación o verificación de tipos. Pero en resumen, no afecta en tiempo de ejecución (que es cuando el código ya está corriendo en el navegador o Node.js).
# Tipos
Los tipos de dato que se definen en TS, no se reflejan en tiempo de ejecución. Solo en tiempo de desarrollo y compilación.
Osea, no sirve para las validaciones de un input que le haríamos a un usuario por ejemplo.

## Sintaxis
Para definir un tipo se usa la sintaxis: `let name: string = 'Mike'`.
TypeScript también permite inferencia de tipos, por lo que `let name = 'Mike';` también sería tratado como string sin la necesidad de especificarlo.

## Any
Le estamos diciendo que ignore el tipado de TS.
En el ejemplo  `let anyValue: any = 'hola'`, le estamos diciendo que NO lo trate específicamente como un string.

## Unknown 
Cuando no sabemos de qué tipo es. A diferencia de `any`, TypeScript te obliga a hacer verificaciones de tipo antes de realizar operaciones sobre él. Por ejemplo, no te permitirá acceder a métodos o propiedades sin asegurarte primero de qué tipo es, lo que evita errores y te ayuda a prever comportamientos. A diferencia de `any`, no ofrece auto completado sin esas verificaciones.
# Funciones
Por defecto las funciones no tienen inferencia de tipos si no tienen contexto. 
```ts
function saludar(name) {
	console.log(`Hola ${name}`)
}
```
En este ejemplo, name al no asignarle un tipo, sería de tipo any. 

## Notación de tipos
Si queremos tipar los parámetros de las funciones que estén dentro de un objeto **(osea cuando estamos des-estructurando un objeto)**, nos encontramos con un problema de colisión de sintaxis con JS ya que JS admite la sintaxis `{name: nombre, age: edad}` para renombrar la propiedad de un objeto.
La forma correcta sería:
```ts
function saludar({name, age}: {name: string, age: number}) {
	console.log(`Hola ${name}, tienes ${age} años`)
}

saludar({name: 'Pepe', age: 2})
```
Otra forma:
```ts
function saludar(persona: {name: string, age: number}) {
	const {name, age} = persona
	console.log(`Hola ${name}, tienes ${age} años`)
}

saludar({name: 'Pepe', age: 2})
```
TS sí tiene inferencia del tipo que retorna:
```ts
function saludar(persona: {name: string, age: number}) {
	const {name, age} = persona
	console.log(`Hola ${name}, tienes ${age} años`)
	return age
}
```
Si ponemos el mouse sobre la función saludar, te muestra que esa función retorna un number.
También lo podemos indicar de forma explícita:
```ts
function saludar(persona: {name: string, age: number}): number { 
	const {name, age} = persona
	console.log(`Hola ${name}, tienes ${age} años`)
	return age
}
```
Se agregó `:number`.

## Call Backs

Para tipar una callback...
```ts
const sayHiFromFunction = (fn: (name: string) => void) => {
	fn('Miguel')
}

const sayHi = (name: string) => {
	console.log(`Hola ${name}`)
	return 3
}

sayHiFromFunction(sayHi)
```
Vemos en este caso que no importa lo que retorne porque indicamos void (incluso podría no devolver nada).

## Arrow Functions 
Es recomendable evitar tipar manualmente las funciones de flecha, ya que TypeScript puede inferir el tipo de los parámetros y el valor de retorno a menos que haya [[Ambigüedad en TypeScript|ambigüedad]]. Sin embargo, estas serían las dos formas de tipar una función de flecha de manera explícita:
Primera forma (tipando los parámetros y el valor de retorno):
```ts
const sumar = (a: number, b: number): number => {
  return a + b;
}
```
Segunda forma (tipando la variable que contiene la función):
```ts
const restar: (a: number, b: number) => number = (a, b) => {
  return a - b;
}
```
Ambas formas son válidas, pero la primera es más común y directa.

## Never 
El tipo never indica que una función nunca va a devolver nada. Esto ocurre cuando una función lanza un error o entra en un bucle infinito.
```ts
function throwError(message: string): never {
  throw new Error(message);
}
```
Este tipo se usa principalmente en situaciones donde el código no debería continuar ejecutándose, como cuando lanzamos excepciones o errores. TypeScript evita que se guarde el valor de una función never en una variable, ya que no hay retorno.

Aunque no se usa mucho en aplicaciones comunes, es útil para asegurarse de que no existan valores inesperados y para ayudar a TS a entender mejor el flujo del código.

## Inferencia funciones anónimas según el contexto
En este ejemplo, TypeScript logra inferir el tipo de los parámetros de la función anónima porque reconoce que el método `forEach` está recorriendo un array de strings. Por lo tanto, asume que `avenger` es de tipo `string`:
```ts
const avengers = ['Spidey', 'Hulk', 'Ironman']

avengers.forEach(function (avenger) {
	console.log(avenger.toUpperCase())
})
```
Gracias a la inferencia de tipos, no es necesario especificar que `avenger` es un `string`, ya que TypeScript deduce esto automáticamente.


# Objetos
Si queremos crear con notación de punto una propiedad nueva al objeto, TS tirará error ya que no logra inferir el tipo:
```ts
let hero = {
	name: 'thor',
	age: 1500
}
```
Si quisiéramos agregar `hero.power = 100`, acá cantaría el error.
Podemos crear nuestra instancia por otro lado con una función:
```ts
function createHero(name: string, age: number) {
	return {name, age}
}

const thor = createHero('Thor', 1500)
```
Pero no sabríamos si es del mismo tipo que el ejemplo anterior aunque en apariencia tengan las mismas propiedades.
Por eso TS tiene los `type alias` de la siguiente sección...
# Type Alias
[Video](https://youtu.be/fUgxxhI_bvc?si=NBEhlFiIdZcMHPM1&t=3211)
```ts
// Creamos el tipo de dato `Hero` (siempre se escribe en PascalCase).
type Hero = {
	name: string
	age: number
}
// Creamos una instancia `hero`.
let hero: Hero = {
	name: 'thor',
	age: 1500
}
// Ahora a la función le pasamos hero, del tipo Hero y que devuelve también el tipo Hero
function createHero(hero: Hero): Hero {
	const {name, age} = hero
	return {name, age}
}
// Acá modificamos para pasar un sólo parámetro en lugar de dos, metiéndolo en un objeto
const thor = createHero({name: 'Thor', age: 1500})
```
Esto lo iremos modificando para hacerlo más eficiente y legible en las siguientes secciones...

# Optional properties
```ts
// Agregamos la propiedad isActive pero con indicador de opcional
type Hero = {
	name: string
	age: number
	isActive?: boolean
}

let hero: Hero = {
	name: 'thor', 
	age: 1500
}

function createHero(hero: Hero): Hero {
	const {name, age} = hero
	return {name, age, isActive: true}
}

const thor = createHero({name: 'Thor', age: 1500})
console.log(thor.isActive) // --> true
```
Por defecto asumimos que al crear un nuevo héroe estará comenzando su carrera por lo que será activo por defecto.

# Optional chaining
Cuando hay una propiedad opcional, al llamarla podemos usar optional chaining:
```ts
type Hero = {
	// Agregamos propiedad opcional
	id?: number
	name: string
	age: number
	isActive?: boolean
}

let hero: Hero = {
	name: 'thor', 
	age: 1500
}

function createHero(hero: Hero): Hero {
	const {name, age} = hero
	return {name, age, isActive: true}
}

const thor = createHero({name: 'Thor', age: 1500})

// Ingresamos a la propiedad opcional de forma condicional
thor.id?.toString()
```

# Mutabilidad
```ts
type Hero = {
	// Agregamos solo lectura a la propiedad
	readonly id?: number
	name: string
	age: number
	isActive?: boolean
}

let hero: Hero = {
	name: 'thor', 
	age: 1500
}

function createHero(hero: Hero): Hero {
	const {name, age} = hero
	return {name, age, isActive: true}
}

const thor = createHero({name: 'Thor', age: 1500})

thor.id = 328423047023570034 // Mostrará error por ser readonly
```

**Nota:** Esto solo aplica en tiempo de desarrollo. No lo hace inmutable en tiempo de compilación. [Video](https://youtu.be/fUgxxhI_bvc?si=f_nV8djEm9O4sWVR&t=3673)

Si quisiéramos que realmente sea inmutable habría que usar `const thor = Object.freeze(createHero({name: 'Thor', age: 1500}))`

# Template union types
```ts // Creamos nuevo tipo HeroId
type HeroId = `${string}-${string}-${string}-${string}-${string}`

type Hero = { // Asignamos el tipo al id
	readonly id?: HeroId
	name: string
	age: number
	isActive?: boolean
}

let hero: Hero = {
	name: 'thor', 
	age: 1500
}

function createHero(hero: Hero): Hero {
	const {name, age} = hero
	return { // Crear la ID dentro de la función:
		id: crypto.randomUUID(), 
		name, 
		age, 
		isActive: true
	}
}

const thor = createHero({name: 'Thor', age: 1500})

thor.id = 328423047023570034 // Mostrará error por ser readonly
```
En este caso tiraría error el id a menos que ingresemos el formato por ej `123-123-123-123-123` que tengo entendido es el formato que genera `crypto.randomUUID()`.

## Otro ejemplo
```ts
type HexadecimalColor = `#${string}`

const color: HexadecimalColor = '0033ff' // Este daría error
const color2: HexadecimalColor = '#0033ff'
```

# Union types
Asignación de tipos específicos:
```ts
type HeroPowerScale = 'local' | 'planetary' | 'galactic' | 'universal' | 'multiversal'
// Agregamos una propiedad opcinal al tipo Hero que además sea del tipo HeroPowerScale:
type Hero = { // Asignamos el tipo al id
	readonly id?: HeroId
	name: string
	age: number
	isActive?: boolean
	powerScale?: HeroPowerScale
}
// Luego al querer asignar un valor a la propiedad escala de poder de por ejemplo thor (instancia de Hero), deberá cumplir con esos tipos:
thor.powerScale = 'universal' // universal y el resto de opciones aparecerán en el auto complete
```

# Intersection types
Para tener correctamente estructurados nuestros tipos y no tener propiedades demás, como por ejemplo en el tipo Hero del cual la función solo aprovecha el name y age de los parámetros que recibe, debemos usar intersection types:
```ts
type HeroId = `${string}-${string}-${string}-${string}-${string}`
type HeroPowerScale = 'local' | 'planetary' | 'galactic' | 'universal' | 'multiversal'

// Agregamos nuevo tipo de info básica
type HeroBasicInfo = {
	name: string,
	age: number
}

// Agregamos las demás props
type HeroProperties = {
	readonly id?: HeroId,
	isActive?: boolean,
	powerScale?: HeroPowerScale
}

// Agregamos la intersección:
type Hero = HeroBasicInfo & HeroProperties

let hero: Hero = {
	name: 'thor',
	age: 1500
}

function createHero(input: HeroBasicInfo): Hero {
	const {name, age} = input
	return {
		id: crypto.randomUUID(),
		name,
		age,
		isActive: true
	}
}

const thor = createHero({name: 'Thor', age: 1500})
thor.powerScale = 'planetary'
```
Luego veremos utility types que nos ayuda a mejorar esto.

# Type Indexing
Te permite extraer un tipo desde una propiedad existente: 
```ts
type HeroProperties = {
	isActive: boolean,
	address: {
		planet: string,
		city: string
	}
}

const addressHero: HeroProperties['address'] = {
	planet: 'Earth',
	city: 'Madrid'
}
```

# Type from value
En TS el typeof sirve para copiar el tipo:
```ts
const address = {
	planet: 'Earth',
	city: 'Madrid'
}

type Address = typeof address

const addressTwitch: Address = {
	planet: 'Mars',
	city: 'Twitch'
}
```

# Type from function return
En este caso estamos recuperando el tipo desde lo que devuelve la función:
```ts
function createAddress(){
	return {
		planet: 'Tierra',
		city: 'Barcelona'
	}
}

type Address = ReturnType<typeof createAddress>
```
No se suele usar mucho
# Type from class
[Video](https://youtu.be/L1ZSk-vPVKI?si=gIm-T2vVsTNIeStZ&t=4891)
```ts
class Avenger {
	name: string
	powerScore: number
	wonBattles: number = 0
	
	constructor(name: string, powerScore: number) {
	this.name = name
	this.powerScore = powerScore
	}

	get fullName() {
		return `${this.name}, de poder ${this.powerScore}`
	}

	set power(newPower: number) {
		if (newPower <= 100) {
			this.powerScore = newPower
		} else {
			throw new Error('Power score cannot be more than 100')
		}
	}
}

const avengers = new Avenger('Spidey', 80)

avengers.power // ahora saldría en el auto complete
```

```ts
avengers.name = 'Hulk' 
```
Vemos que permite mutar la clase por lo cual ahora Spidey se llama Hulk lo cual no tiene sentido. Para arreglar esto usamos readonly:
```ts
class Avenger {
	readonly name: string
	//.....
	//.....
}
```

## Propiedades privadas en JS
La forma oficial de JS para hacer privada una propiedad de una clase es agregar # al comienzo de la propiedad:
```ts
class Avenger {
	readonly name: string
	#powerScore: number
	//......
}
```
También debemos agregarlo al resto de `powerScore` que hay en el código.
Ahora esta propiedad es privada y solo accesible dentro de la misma clase.

## Propiedades privadas en TS
La forma de TS para que no funcione en Run Time, sería usar `private`:
```ts
class Avenger {
	readonly name: string
	private powerScore: number
	//......
}
```
ya no haría falta usar el hash en el resto de referencias a esta propiedad. Esto ahora funcionaría solo de forma estática. En tiempo de desarrollo.

También podés hacer que sea privado y solo lectura:
```ts
private readonly wonBattles: number = 0
```
Recordar que por defecto las props son public.

Por último tenemos el `protected`que es como el `private`pero podés acceder también a clases que heredan de esta clase mas no en instancias de la clase:
```ts
protected age: number = 0
```

## Interface en clases
[Video](https://youtu.be/L1ZSk-vPVKI?si=VEokCcSkCwNgmAxk&t=5174)
También podemos implementar las interfaces dentro de las clases:
```ts
interface Avenger {
	name: string
	powerScore: number
	wonBattles: number
	age: number
}

class Avenger implements Avenger {
	// readonly name: string SE BORRA
	// private powerScore: number SE BORRA
	// Ya no necesitariamos los tipos acá, se traen de la interface...
	...
	constructor(name: string, powerScore: number) {
	//...
	....
}
```
## Ejemplo de GPT
En TypeScript, puedes obtener el tipo a partir de una clase utilizando typeof y InstanceType.
Ejemplo básico de Type from Class:

```ts
class Hero {
  id: number;
  name: string;
  age: number;

  constructor(id: number, name: string, age: number) {
    this.id = id;
    this.name = name;
    this.age = age;
  }
}

// Obtenemos el tipo del constructor de la clase Hero
type HeroType = typeof Hero;

// Obtenemos el tipo de una instancia de la clase Hero
type HeroInstanceType = InstanceType<typeof Hero>;
```

- `typeof Hero` te da el tipo del constructor de la clase, es decir, su firma (como si fuera una función).

- `InstanceType<typeof Hero>` te da el tipo de una instancia de la clase, es decir, los valores y métodos disponibles en cada instancia de Hero.

Esto te permite manejar clases y sus instancias de manera más flexible, ya sea verificando tipos o pasando las clases como argumentos con tipado.
# Arrays
Primer caso:
```ts
const languages = []

languages.push('JavaScript')
```
Si lo dejáramos así, en TS sería un `never[]`, por lo cual estaríamos diciendo que queremos que nunca tenga datos, que sea siempre un array vacío.

Segundo caso:
```ts
const languages: string[] = []

languages.push('JavaScript')
```
En este caso sólo nos dejaría hacer push de strings.
En este caso también está la sintaxis alternativa:
```ts
const languages: Array<string> = []
```

Tercer caso:
```ts
const languages: (string | number)[] = []

languages.push('JS')
languages.push(3)
```
Ahora también admite number

También podríamos tener un array de Héroes:
```ts
type HeroId = `${string}-${string}-${string}-${string}-${string}`
type HeroPowerScale = 'local' | 'planetary' | 'galactic' | 'universal' | 'multiversal'

type HeroBasicInfo = {
	id?: number,
	name: string,
	age: number
}

const heros: HeroBasicInfo[] = []
```

# Matrices y tuplas
Simulando el tres en raya:
```ts
type CellValue = 'X' | 'O' | ''
type GameBoard = [ // tupla
	[CellValue, CellValue, CellValue],
	[CellValue, CellValue, CellValue],
	[CellValue, CellValue, CellValue]
]

const gameBoard: GameBoard = [
	['X', 'O', 'X'],
	['O', 'X',  'O'],
	['X', 'O',  'X']
]
```
En este ejemplo usamos una tupla, que es un array que tiene un límite fijado de longitud.

Otro ejemplo de tupla:
```ts
type State = [string, (newName: string) => void]
const [hero, setHero] = useState('thor')
```
Lo que devuelve un useState sería una tupla

También lo podemos ver en los códigos RGB:
```ts
type RGB = [number, number, number] // tupla
const RGB = [255, 255, 0]
```
Con esta tupla indicamos que sólo puede ser de tres posiciones y number el array.

## Destructive methods in tuple type
Si hacemos un push a una tupla, la rompemos. Es una issue pendiente de TS que no se "arregla" por retro compatibilidad hasta el día de hoy:
```ts
type RGB = [number, number, number] 

const black: RGB = [0, 0, 0]
const white: RGB = [255, 255, 255]

black.push(4) // Y SE LO COME CON FRITAS!!!
```
una forma de solucionarlo sería agregar solo lectura:
```ts
type RGB = readonly [number, number, number] 
```

# Enums
En JS sería algo así:
```js
const ERROR_TYPES = {
	NOT_FOUND: 'notFound',
	UNAUTHORIZED: 'unauthorized',
	FORBIDDEN: 'forbidden'
}

function mostrarMensaje (tipoDeError) {
	if (tipoDeError === ERROR_TYPES.NOT_FOUND) {
		console.log('No se encuentra el recurso')
	} else if (tipoDeError === ERROR_TYPES.UNAUTHORIZED) {
		console.log('No tienes permisos para acceder')
	} else if (tipoDeError === ERROR_TYPES.FORBIDDEN) {
		console.log('No tienes permisos para acceder')
	}
}
```
En TS, lo mejor sería que usemos Enums:
```ts
enum ERROR_TYPES {
	NOT_FOUND,
	UNAUTHORIZED,
	FORBIDDEN
}

// El resto del código sería igual, excepto porque le agregamos el tipado al parámetro:

function mostrarMensaje (tipoDeError: ERROR_TYPES) {
	if (tipoDeError === ERROR_TYPES.NOT_FOUND) {
		console.log('No se encuentra el recurso')
	} else if (tipoDeError === ERROR_TYPES.UNAUTHORIZED) {
		console.log('No tienes permisos para acceder')
	} else if (tipoDeError === ERROR_TYPES.FORBIDDEN) {
		console.log('No tienes permisos para acceder')
	}
}
```

## Uso de enums
[Video](https://youtu.be/L1ZSk-vPVKI?si=0mNcqRFTxkG3361J&t=1578)
Al ejemplo anterior podemos sumarle el const:
```ts
const enum ERROR_TYPES {
	NOT_FOUND,
	UNAUTHORIZED,
	FORBIDDEN
}
```
De esta forma al compilarse, genera menos código.
Es lo más común, que se use.
Pero si es una app que se va a consumir por un tercero, quizás es más interesante no usar const para darle más info al usuario.

# Aserciones de tipos
Si traemos el elemento por ejemplo canvas de HTML, TS al solo funcionar en tiempo de desarrollo y no de ejecución, no sabe realmente qué elemento estamos trayendo. Y cada elemento tiene sus propios métodos, atributos y propiedades. 
```ts
const canvas = document.getElementById('canvas')

// null si no lo encuentra
// HTMLElement si lo encuentra

if (canvas !== null) {
	const ctx = canvas.getContext('2d)
}
```
En este ejemplo, el `getContext` daría error "La propiedad `getContext` no existe en el tipo `HTMLElement`".

Lo que se podría hacer en TS es decirle que lo trate como el elemento en cuestión:
```ts
const canvas = document.getElementById('canvas') as HTMLCanvasElement
```
Pero esta forma no es robusta ya que el elemento podría no estar en el DOM y devolver null por ejemplo.

Así que la forma correcta sería SIN aserción de tipos:
```ts
const canvas = document.getElementById('span')

if (canvas instanceof HTMLCanvasElement) {
	const ctx = canvas.getContext('2d')
}
```
Con esta comprobación no tiraría error aunque le pasemos span al selector.

# Aserciones fetching
> [!warning] Para esta sección usaremos la extensión de archivo `.mts`que significa "Module TypeScript".

Cuando hacemos fetch en TS, pasa lo mismo que en la sección anterior. TS no sabe qué tipo de dato vendrá. Hay una forma manual de hacerlo pero no es la correcta, en este caso usaremos una herramienta que nos genera todos los tipos de datos de la API como [jsonformatter](https://jsonformatter.org/json-to-typescript) pero podría ser cualquiera que convierta JSON a TS.
[Video](https://youtu.be/L1ZSk-vPVKI?si=T4rwqrrPfXrwKNbZ&t=2734)
```ts
// Primero en esta línea pegaríamos el resultado que nos da la herramienta, osea la definición de tipos de TS

// Luego nuestro fetch:
const API_URL = "https://api.github.com/search/repositories?q=javascript"

const response = await fetch(API_URL)

if (!response.ok) {
	throw new Error('Request failed')
}

const data = await response.json() as GitHubAPIResponse

data.items.map(repo => {
	return {
		name: repo.name,
		id: repo.id,
		url: repo.link // link no existe
	}
})
```
En el caso de `repo.link`ahora TS sabe que no existe en la definición de tipos y nos da error.

## Aserción segura fetching
En el ejemplo anterior, `repo.name`podría devolver un valor no esperado (supongo porque la API puede cambiar luego de que hicimos la conversión a TS) por lo cual no es un método totalmente seguro.

Para solucionar esto, la herramienta [quicktype](https://app.quicktype.io/) por ejemplo, tiene también la opción `typescript zod`la cual además de convertir a tipos, usa la biblioteca zod que hace comprobaciones en tiempo de ejecución.
[Video](https://youtu.be/L1ZSk-vPVKI?si=kPhIOp-dI018U_7m&t=2944)

# Interface
En la gran mayoría de los casos es intercambiable con `type`, pero tienen sus ligeras diferencias.

```ts
interface Producto {
	id: number
	nombre: string
	precio: number
	quantity: number
}

// Usamos `extends`que es algo que no se puede hacer exactamente igual con los tipos:
interface Zapatilla extends Producto{
	talla: number
}

interface CarritoDeCompras {
	totalPrice: number
	// Las interfaces también pueden estar anidadas;
	// Además le decimos que puede ser del tipo producto o zapatilla el array
	productos: (Producto | Zapatilla)[]
}

const carrito: CarritoDeCompras = {
	totalPrice: 100,
	productos: [
		{
			id: 1,
			nombre: 'Producto 1',
			precio: 100,
			quantity: 1,
			talla: 5
		}
	]
}
```

Podríamos también definir las operaciones del carrito:
```ts
// Primer forma:
interface CarritoOps {
	add: (product: Producto) => void,
	remove: (id: number) => void,
	clear: () => void,
}

// Segunda forma:
interface CarritoOps {
	add(product: Producto): void
	remove(id: number): void
	clear(): void
}
```

Una de las diferencias que más destacan entre las interfaces y los tipos es la extensión implícita. En las interfaces podemos volver a llamarla y agregar lo que queramos más adelante en el código:
```ts
interface CarritoOps {
	add: (product: Producto) => void,
	remove: (id: number) => void,
}

/* Muchas líneas después...
.
.
.
.
*/

interface CarritoOps {
	clear: () => void,
}
```
Esta fusión de declaraciones no sería posible con los tipos.
Importante tener en cuenta ya que puede generar confusión o errores.

# Type guards y Narrowing
Narrowing y type guards están relacionados pero no son exactamente lo mismo. El narrowing es el proceso en el que TypeScript reduce un tipo a algo más específico dentro de un bloque de código. Por otro lado, los type guards son técnicas o verificaciones que ayudan a hacer ese narrowing.

En resumen:

Narrowing es el proceso de "afinar" un tipo en tu código.
Type guards son las herramientas (como typeof, instanceof, o incluso verificaciones personalizadas) que usás para hacer ese narrowing.

Ejemplo típico de narrowing con type guard:

```ts
function mostrarLongitud(objeto: number | string) {
  if (typeof objeto === 'string') {
    return objeto.length; // Narrowing: acá TS sabe que es string
  }
  return objeto.toString().length; // Acá TS sabe que es number
}
```

Si ponemos el mouse sobre objeto dentro de la condición, TS aún no sabe el tipo y muestra que puede ser string o number. Si lo hacemos en el objeto del return ya mostrará que es tipo string.
Narrowing es la técnica de ir filtrando los tipos y de esta forma ya TS puede pre ver los métodos en este caso del string para el auto complete.
[Video](https://youtu.be/L1ZSk-vPVKI?si=XEGteZUdcil_eENH&t=3924)
## Narrowing vs Type guards by GPT
La relación entre **narrowing** y **type guards** en TypeScript es bastante estrecha, pero no son exactamente lo mismo. Aquí te explico cómo se relacionan:

### **Narrowing**

Narrowing es el proceso de reducir el tipo de una variable a un tipo más específico. TypeScript utiliza el **narrowing** para inferir un tipo más específico de una variable en función de ciertas condiciones.

### **Type Guards**

Los **type guards** son técnicas que ayudan a TypeScript a determinar el tipo de una variable dentro de un bloque de código. Esto permite que TypeScript realice **narrowing** de forma más efectiva.

### **Relación entre ambos**

- **Los type guards son una herramienta que se utiliza para lograr narrowing.** Por ejemplo, puedes usar un type guard para verificar si una variable es de un tipo específico, lo que permite a TypeScript hacer narrowing en el contexto de tu código.

### **Ejemplo**

Aquí tienes un ejemplo que ilustra ambos conceptos:

```ts
type Pet = 
  | { type: 'dog'; bark(): void }
  | { type: 'cat'; meow(): void }

function handlePet(pet: Pet) {
  if (pet.type === 'dog') {
    // Aquí hacemos narrowing con el type guard
    pet.bark(); // pet es de tipo { type: 'dog'; bark(): void }
  } else {
    // Aquí pet es de tipo { type: 'cat'; meow(): void }
    pet.meow();
  }
}
```

### Resumen

- **Narrowing**: Proceso de reducir el tipo de una variable.
- **Type Guards**: Técnicas que facilitan el narrowing.

Así que, en resumen, los type guards son herramientas que utilizas para realizar narrowing en TypeScript.

# Tipos genéricos
Los genéricos son tipos que se le pasan como parámetros a las funciones lo cual definirá de qué tipo es la función.
```ts
function getArray<T>(items : T[]) : T[] {
	return new Array<T>().concat(items);
}

let numberArray = getArray([5,10,15,20]);
let stringArray = getArray(['Cats', 'Dogs', 'Birds'])

numberArray.push('hello')
numberArray.push({})

getArray(['string', 1, 2, 3])
```

La T que pusimos entre paréntesis angulares, representa un tipo que aún no está definido. Este se define al llamar la función y pasarle parámetros.
Cuando hacemos esos dos push, canta error ya que al haberlo llamado antes al numberArray pasándole solo numbers, se definión como array de números.
De esta forma TS lo infirió pero también se lo podemos decir de forma explícita o forzada:

```ts
let numberArray = getArray<string>([5,10,15,20]);
```
Acá por ej. marcaría como error los números que le estamos pasando al Array ya que le estamos diciendo que debe ser de strings.
[Video](https://youtu.be/g16brokfrQw?si=Xp5u1QlfFiuz9EqA&t=401)

## Uso de varias variables de tipo
Es igual pero pasamos varios tipos por parámetros:
```ts
function identity<T, U> (value: T, message: U) : T {
	console.log(message);
	return value
}

const value = identity<string, string>('hola, 'mensaje')
const value2 = identity<number, number>(1, 2)
```

## Uso de los métodos y las propiedades de un tipo genérico
```ts
function identity<T, U> (value: T, message: U) : T {
	let result: T = value + value;
	console.log(message);
	return result
}
```
En este caso `value + value` tira error ya que no se puede hacer una operación aritmética sobre un tipo `T` que aún no está definido.

# Uso de restricciones de tipos con genéricos
```ts
type ValidTypes = string | number

function identity<T extends ValidTypes, U> (value: T, message: U) {
	let result: ValidTypes = ''

	if (typeof value ==== 'number') {
		result = value + value // suma
	} else if (typeof value === 'string') {
		result = value + value // concatenación
	}

	console.log(message);
	return result
}

identity<number, string>(1, 'hola')
```
En este caso hacemos narrowing para que trate a la operación según el tipo que corresponda, usando tipos genéricos.

