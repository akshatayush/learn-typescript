# TypeScript
TypeScript is a strongly typed superset of JavaScript. It allows
better coding practices, easier debugging and development due to
static types, features that native JavaScript does not have like
Interfaces, better IDE support etc.

TypeScript is compiled into JavaScript code using the TypeScript
compiler (`tsc`). It creates workarounds for features that are
not natively supported by JavaScript.

*Primitive types suported by JavaScript are number, string,
boolean, bigint, null, undefined and symbol.*

```typescript
const age = 10; // number
const name = "Markus" // string
const person = {
    name: name,
    age: age
} // {name: string; age: number}
```
TypeScript infers the types in the examples above thus, it's not
necessary to explicitly mention the types when declaring and 
initializing the variables at the same time. However it is 
necessary when only declaring the variables and not initializing 
them at the same time.

---

## Objects

Object is the type for the usual JavaScript key value pair object.
Although objects are a very generic type. More specific types can
be specified for JavaScript objects.
```typescript
const person: object = {
    firstname: "Akshat",
    age: 21
};
console.log(person.firstname); // Error
console.log(person.lastname); // Error

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

---

## Arrays

These are any JavaScript arrays. The type of the arrays can be 
flexible or strict regarding the element types.
```typescript
const person = {
    name: "Akshat",
    age: 21, 
    hobbies: ["Music", "Cooking"] // (property) hobbies: string[]
}

const someArray: string[] = ["Akshat", "Ayush"]

for (const hobby of person.hobbies) {
    console.log(hobby.toLowerCase()); // No error due to inference
}
```
Above are the ways to mention types of arrays of a particular type in TypeScript.

---

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

person.role.push("developer"); // Works
person.role = [1, "admin", "developer"] // Error
person.role[1] = 2; // Error
```
Note that the `push` operation works in tuples even though it will 
be violating the size restriction we specified.

---

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
enum Role {ADMIN = 1, DEVELOPER, MODERATOR = 0} // DEVELOPER = 2
```