# Conventions and Rules

## Intro
This text is a general document that will combine all of the small and big rules and conventions
that I personally think should be in place in every js project written with this starter.
The goal behind this starter and this document is to make js development sipmler, more predictible
and let developers focus on development. The goal is to remove all of the noise,
all of the unnecessary variables from js development making them constants in this document.

## Entry Point
The entry point is the file from which nodejs start programm or server execution.
It case of this starter and all servers written with it this file located under `./main.js`

## No OOP, Functions is the Unit
OOP is a paradigm of the past. It makes code complex, hurts maintainability, hurts readability and reasoning becomes hard.
That's why I decided to use functions as a main unit in this starter. Each file except `./main.js` contains one function
and exports it like so `module.exports = { functionName }`. You shouldn't declare anything except that function in scope of that file.
All the variables should be contained inside a main function.

## Folder Structure
Because our main unit is a function we have a top level folder `./functions` in which all of the logic, code, i.e. functions are located.
This is mandatory. Inside this folder there are two folders: impure and pure. They are also mandatory to have and nothing but them inside.
Folder structure inside those two folders can be anything you wish. You can divide functions into folder as seems reasonable for your proejct.

## Impure folder
Here goes anything that interacts with a real world like internet requests(http, tcp, udp, smtp), file reads/writes,
getting value of some global like process even. This is done to separate code with side effects from pure logic.
It will simplify reasoning, maintanance and testing.

## Pure folder
Here goes all the functions that contain business logic. Functions here has some limitations.
1. You are prohibited of importing anything but files under `./functions/pure` here
2. Impure functions are injected into function argument like `{ impureFunctions }`
3. `impureFunctions` should be flat object with all fields being impure functions
4. Each function should return an object with the following structure: `{ errors: [any], value: any }`
In this document functions from this folder will be called pure functions, don't confuse with general definition.

## Client Server Interaction
This starter was written with srpc-framework. It follow all this rules and ideology.
All the clients would interact with server using srpc protocol. Srpc can work above http.
The idea of srpc is that you have functions on your server that client can execute with any arguments
and return value are returned to client.
I will call those functions `roots` from now on.

## Error handing
I recognize two types of errors. Ones that should be caught and returned to client which generaly called `validation errors`.
And the ones in which we don't expected the problem, but it happened which generaly called `internal server errors`.
The first type should be handled by checking if a function we called returned an error array inside it's response object.
If so we pass this error array to a parent function until it reaches `roots` and returned to client.
Only pure functions can return an error response of first type. Impure functions can only throw errors of second type.
Errors of second type should be logged by your logging mechanism and further investigated
while client gets a generic `Internal Server Error` message.

## Naming
All the images should be named using hyphen-case. All the js files should be named using camelCase and after the one function they export.

## Extensions of this Document
This document can be extended any further for some more specific cases, but this rules are the basis
