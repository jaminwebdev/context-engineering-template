# Directory Purpose

This layer is meant to dictate specifics on how to approach programming, problem solving, code organization, and other programming-related philosophies. 

Here's a simplified example document (note: the example below isn't meant to be instructions, it's meant to serve as an example as to how to format documents in this directory):

````markdown
# General Coding Style Rules

Keep your explanations brief and to the point.

## General Rules

- Use descriptive variable and function names
- Use functional and declarative programming patterns; avoid unnecessary classes except for state machines
- Prefer iteration and modularization over code duplication
- Use TypeScript for all code; prefer types over interfaces
- Implement proper lazy loading for images and other assets
- Wherever appropriate, opt for early returns as opposed to multiple or nested if/else conditions or switch statements
- Prefer to abstract complicated logic into small pure functions with descriptive names and few parameters as necessary

### Accessibility

- Ensure proper semantic HTML structure
- Implement ARIA attributes where necessary
- Ensure keyboard navigation support for interactive elements

## Prefer to "early out" in functions rather than nesting statements

Instead of this:

```ts
let theResult = "";
if (someVariable == 42) {
  theResult = "the answer";
}
else if (someVariable == 69) {
  theResult = "nice";
}
else {
  theResult = "nope";
}
return theResult
```

You should write this:

```ts
if (someVariable == 42) 
  return "the answer";
if (someVariable == 69) 
  return "nice";
return "nope";
```

Another example. Instead of this:

```ts
const showAdminPanel = () => {
	if (wifi) {
		if (login) {
			if (admin) {
				seeAdminPanel();
			} else {
				console.log('must be an admin')
			}
		} else {
			console.log('must be logged in')
		}
	} else {
		console.log('must be connected to wifi')
	}
}
```

Do this:

```ts
const showAdminPanel = () => {
	if (!wifi) return handleNoWifi()
	
	if (!login) return handleNotLoggedIn()
	
	if (!admin) return handleNotAdmin()
	
	seeAdminPanel();
}
```

## Prefer to use the "object in object out" pattern when writing typescript functions

So instead of writing this:

```ts
function myFunction(firstArg: string, second: number, isSomething?: boolean) {
  // ...
}
```

You should write:

```ts
function myFunction({ firstArg, second, isSomething }: { firstArg: string, second: number, isSomething?: boolean }) {
  // ...
}
```

If the function needs to return multiple values then return an object:

```ts
function calculateSomething() {
  return {
    theAnswer: 42,
    reason: "the computer said so"
  }
}
```
````