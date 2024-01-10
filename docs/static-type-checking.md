# Static Type Checking

In JavaScript-land we often find ourselves stuck in errors that languages like Java, C and all other statically typed languages find before the application even compiles. The wonderful team working on Typescript has already solved this issue by providing a compilation step for JavaScript and new syntax to provide static typing inside JavaScript code.

Learning and converting to Typescript can be a challenge for teams and Typescript knew it, so it's team found the perfect solution. The Typescript tooling that VS Code and by extension BAS provides, allows us to have real time type checking. This feature is enabled by writing in the appropriate file `// @ts-check` and just like magic the tooling will start checking types and raising errors.

> This feature won't stop build and deploy attempts of a codebase with errors.
## Handles common issues

By default `// ts-check` handles the two most important errors in our codebases.
- Non-existent variables and object keys.
- Redeclaration of variables with mismatching types.
- Expressions between differing types.

This means that typos, and nonexistent values are always visible to you as errors.
```js
let person = 'Luke';
console.log(pesron); // Cannot find name 'pesron'. Did you mean 'person'?
```

Confusion over data types and structures are handled gracefully.
```js
let definitelyAString = true;
definitelyAString = 'string'; // Type 'string' is not assignable to type 'boolean'.
```

Checks get less error prone.
```js
let num = 0;
let str = 'string';
num === str // This comparison appears to be unintentional because the types 'number' and 'string' have no overlap.
```

There are plenty of examples to see about how this can help us write more maintainable and easy to use code but we first should check how to use this exactly.
## How is it enabled?
Enabling this feature is handled primarily by using the following comment.

```js
// @ts-check
```

Inserting it at the beginning of the file, enables real-time type checking in the current file allowing you to do all the cool stuff we'll talk about. You can also enable it at specific lines of code only, by writing it before the line of code that type checking should be enabled.

But what if you want to disable it because a line is using global scope objects that you don't have types for? Is that even possible? Just add the following line before the line you want to disable type checking.

```js
// @ts-ignore
```

There are many ways of enabling type checking throughout your application by default, but until types for the common globals are provided we'll need to ignore to make this feature function. 

You can still read more about these two comments and two more in the [Learn More](#learn-more) section.
## Usage
Once you enable this feature some errors will be found by default. This is great, because with minimal effort we can still make use of this feature. But with a little bit of tinkering we can perform really cool type checking inside our code that has the chance to save us a lot of time and braincells. From this point onward assume **`// @ts-check` is enabled in every code block** unless otherwise stated.
### Inferring types
When we write a variable in JavaScript, VS Code attempts to identify it's type. For example if we have initialize a variable with a string it's type will always be `string`. If we then attempt to insert anything but a `string` in this variable the code will be highlighted as an error.
```js
let shortStory = 'Once upon a time...';
shortStory = true; // Error: Type 'string' is not assignable to type 'boolean'.
```
This enforces the standard in JavaScript that every variable is of one type. Even in objects where we might get confused and use the same variable for storing objects with completely different properties.

Inferring the type of an item doesn't always go as expected though. As this code only infers in declaration, the following code fails to identify the type of the object.
```js
let shortStory;
shortStory = 'Once upon a time...';
```
Hovering over the variable you'll see that it is of type `any` meaning it can take any value possible and won't be tracked for errors.

How do we fix these issues? By introducing JS Doc. 
## Defining types
**JS Doc a special commenting format that describes our code**. With it we can do cool things like define the type of a variable.
```js
/**
 * @type {string}
 */
let shortStory;
shortStory = 'Once upon a time...';
```
Hovering over the variable now displays the type `string` and enables correct tracking from our editor's tooling. We can specify any literal, datatype as well as custom types within the brackets.

Combining types can be achieved with a pipe `|` making more complex and descriptive types. The pipe symbol can be though as an or statement and can be combined as many times as we want.
```js
/**
 * @type {'down' | 'running'}
 */
let serverStatus;
serverStatus = 'running';

// Specifying any string other than the ones defined above will result in an error
serverStatus = 'starting'; // Error: Type '"starting"' is not assignable to type '"down" | "running"'.
```

Inferring the type of an object can lead to unexpected issues. Although all the properties of the object are inferred, we aren't prohibited from adding new properties. This can cause issues later on as the new properties aren't tracked and the tooling won't check for types.
```js
let person = {
	name: 'John',
	surname: 'Doe'
};

// The following code doesn't raise an error
person.age = 35;
```

If we define it's type we'll be stopped from creating new properties.
```js
/*
 * @type {{name: string, surname: string}}
 */
let person = {
	name: 'John',
	surname: 'Doe'
};

person.age = 35; // Error: Property 'age' does not exist on type '{ name: string; surname: string; }'.
```

It's a good practice to name complex types to make them reusable.
```js
/**
 * @typedef {'down' | 'running'} ServerStatus
 */

/**
 * @type {ServerStatus}
 */
let serverStatus = 'running';

// and for objects we can either use the afforementioned syntax

/**
 * @typedef {{name: string, surname: string}} Person
 */

// or we can use the long format
  
/**
 * @typedef {object} Person
 * @property {string} name
 * @property {string} surname
 */
  
/**
 * @type {Person}
*/
let person = {
    name: 'John',
    surname: 'Doe'
};
```

Functions definitions can automatically infer the return type if it's possible. Parameters though don't indulge in such luxuries. We might see that defining the types of functions helps us identify and use them quicker, especially when the codebase grows drastically.
```js
function serverStatus() {
	return true;
}

let status = serverStatus();
```
Hovering on the variable shows the type `boolean`.

```js
/**
 * @parameter {number} num1
 * @parameter {number} num2
 * @returns {number}
 */
function add(num1, num2) {
	return num1 + num2;
}
```
*Ideally if the return type can be inferred don't override it. This example doesn't need the `@returns` tag*
## Limitations
Although all these features sound great. Some issues still need to be considered and possibly addressed.
- Code with errors isn't stopped from getting deployed.
- There need to be type declarations for global objects.
- UI5 needs some restructuring for this technique to be used effectively.
## Learn More
[Global Typesetting within VS Code](https://code.visualstudio.com/docs/nodejs/working-with-javascript) \
[Typescript documentation about // @ts-check](https://www.typescriptlang.org/docs/handbook/intro-to-js-ts.html) \
[Official JSDoc Reference](https://jsdoc.app/) \
[Typescript JSDoc Documentation](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html) \
[JSDoc cheat sheet](https://docs.joshuatz.com/cheatsheets/js/jsdoc/)