# TypeScript
TypeScript is a strongly typed superset of JavaScript. It allows
better coding practices, easier debugging and development due to
static types, features that native JavaScript does not have like
Interfaces, better IDE support etc. Primitive types suported by 
JavaScript are number, string, boolean, bigint, null, undefined 
and symbol.

TypeScript is compiled into JavaScript code using the TypeScript
compiler (`tsc`). It creates workarounds for features that are
not natively supported by JavaScript.

## Table of Contents
1. [Inference](#inference)
1. [Objects](#objects)
1. [Arrays](#arrays)
1. [Tuples](#tuples)
1. [Enums](#enums)
1. [Any](#any)


## Inference of types

```typescript
const age = 10;         // number
const name = "Markus"   // string
const person = {
    name: name,
    age: age
}                       // {name: string; age: number}
```
TypeScript infers the types in the examples above thus, it's not
necessary to explicitly mention the types when declaring and 
initializing the variables at the same time. However it is 
necessary when only declaring the variables and not initializing 
them at the same time.


## Objects

Object is the type for the usual JavaScript key value pair object.
Although objects are a very generic type. More specific types can
be specified for JavaScript objects.
```typescript
const person: object = {
    firstname: "Akshat",
    age: 21
};
console.log(person.firstname);  // Error
console.log(person.lastname);   // Error

// Correct way of setting type of person (Redundant)
const person: {
    firstname: string;
    age: number;
} = {
    firstname: "Akshat",
    age: 21
};
```
In the code snippet above, TypeScript recognises that the object 
person does not have a key called `lastname` so it throws an error.
However since we did not allow TypeScript to implicitly infer the 
type of the person object and provided it with a generic object 
type, TypeScript does not know if the object has the property 
called `firstname` which the object does but TypeScript shows an 
error regardless because it doesn't know what properties does the 
object have.


## Arrays

These are any JavaScript arrays. The type of the arrays can be 
flexible or strict regarding the element types.
```typescript
const person = {
    name: "Akshat",
    age: 21, 
    hobbies: ["Music", "Cooking"]     // (property) hobbies: string[]
}

const someArray: string[] = ["Akshat", "Ayush"]

for (const hobby of person.hobbies) {
    console.log(hobby.toLowerCase()); // No error due to inference
}
```
Above are the ways to mention types of arrays of a particular type 
in TypeScript.


## Tuples

Tuples have a fixed type and length in TypeScript. This type is 
not natively supported in JavaScript.
```typescript
const person: {
    name: string;
    age: number;
    hobbies: string[];
    role: [number, string]; // Overriding the inferred type of role
} = {
    name: "Akshat",
    age: 21, 
    hobbies: ["Music", "Cooking"],
    role: [1, "admin"]
};

person.role.push("developer");          // Works
person.role = [1, "admin", "developer"] // Error
person.role[1] = 2;                     // Error
```
Note that the `push` operation works in tuples even though it will 
be violating the size restriction we specified.


## Enums
This type is also not natively supported by JavaScript. It 
provides automatically enumerated global constant identifiers. An 
enum is also a custom type created by us.
```typescript
enum Role {ADMIN, DEVELOPER};

const person = {
    name: "Akshat",
    age: 21, 
    hobbies: ["Music", "Cooking"],
    role: Role.ADMIN
};
```
We can also assign values to the fields insode the enum class to 
override the default behaviour of starting from 0. Other types can 
also be used for assignment.
```typescript
enum Role {ADMIN = 1, DEVELOPER, MODERATOR = 0}; // DEVELOPER = 2
```


## Any

As the name suggests, it can reperesent any data type. Thus, it 
should only be used in the cases when there is no way to know what 
kind of data will be stored in the variable. Otherwise, it wouldn't 
provide any benefits over regular JavaScript.


## Union

Used with variables that might be used for more than one type of 
data.
```typescript
function add(input1: number|string, input2: number|string) {
    let result;
    if(typeof input1 === 'number' && typeof input2 === 'number') {
        result = input1 + input2;
    } else {
        result = input1.toString() + input2.toString();
    }
    return result;
}
```
In the example above we are explicitly checking for number and 
strings inside the function because TypeSciript cannot determine 
if the union of two or more types will support arithmetic 
operations or not if we returned the sum of the inputs directly.


## Literals

Lietrals are used when we need to work with only a limited number 
of constants. In the example below, adding literals as type for 
the output type of the function, allows for `outType` to only be 
able to have 2 possible string values. This saves the developer 
from the extra work of remembering the constants used in the 
function.
```typescript
function add(input1: number|string, input2: number|string, 
            outType: 'as-number'|'as-string') {
    let result;
    if(typeof input1 === 'number' && typeof input2 === 'number') {
        result = input1 + input2;
    } else {
        result = input1.toString() + input2.toString();
    }
    if(outType === 'as-number') {
        return parseFloat(result);
    }
    return result.toString();
}
```


## Type aliases

```typescript
type Addable = number|string;
type outType = 'as-number'|'as-string';
```
The aliases can be used like typedef in C/C++


## Function return type

Function return types are inferred by TypeScript but can be 
explicitly mentioned below. It is better to let TypeScript infer 
the return type of the function unless we have an explicit reason 
to mention the return type.
```typescript
function add(n1: number, n2: number): number  {
    return n1 + n2;
}

function printResult(num: number): void {
    console.log("Result: " + num);
}

// undefined is a valid type in JavaScript
function printResult(num: number): undefined {
    console.log("Result: " + num);
    return; // returning an undefined value
}
```


## Function types

```typescript
function add(n1: number, n2: number): number  {
    return n1 + n2;
}
let combine: Function;
combine = add;
console.log(combine(10, 2));
```
In the above example, `combine` can be reassigned to any function 
because Function is the generic type representing all functions.
```typescript
function add(n1: number, n2: number): number  {
    return n1 + n2;
}
let combine: (a: number, b: number) => number;      // Function type
combine = add;
console.log(combine(10, 2));

function addHandler(a: number, b: number, 
                    callback: (a: number) => void) { // Function type
    const result = a + b;
    callback(result);
}

addHandler(10, 20, (result) => { // Type of result not required
    console.log(result);
}); 
/* 
Note: The passed callback function can return something
in the definition without raising an error even though
it's return type is declared as void. This still means
that whatever our function will return will be ignored in
the parent function. 
*/
```


## Unknown types

Unknown type works similar to the any type but with some 
restrictions that it cannot be assignned to a more stricter type 
variable. Any won't throw an error in this case. It is the most 
flexible type. Thus, unknown is a better choice.
```typescript
let userInput: unknown;
userInput = 5;

let userName: string;
userName = userInput;   // Error

if(typeof userInput === 'string') {
    userName = userInput;
}                       // No error
```


## Never type

For functions that never return, for example, while throwing 
exceptions, infinite loops etc.
```typescript
function generateError(message: string, code: number): never {
    throw { message: message, errorCode: code };
    while(true) {
        continue;
    }
}
```


## Compiling and watch node

TypeScript needs to be compiled into JavaScript and can be complied 
using the `tsc` compiler. A watch node can be added using the 
`--watch` or `-w` flag in the command to start watching the file 
for any changes. On saving changes the file is automatically 
recompiled.
```bash
tsc -w app.ts
```
To automatically compile all the files in the project and adding a 
watch on them we can let the compiler know the project folder and 
then run the command without pointing to any files.
```bash
tsc --init
tsc -w
```
This would create a `tsconfig.json` file that stores the 
configurations for the compiler. Some config options that do not 
affect the behavior of the compiler are given below.
```json
{
    "exclude": [        // exclude files from compiling
        "node_modules", // Automatically excluded
        "analytics.ts",
        "**/*.old.ts"   // excludes files with .old inside any folder
    ],
    "include": [        // Files not mentioned here won't be compiled
        "app.ts"
    ]
}
```