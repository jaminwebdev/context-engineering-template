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

## Prefer if-statements with early returns over switch statements

Instead of this:

```ts
function doSomething(kind: MyKind) {
  switch (kind) {
    case "a":
      return "it was A";
    case "b":
      return "it was B";
    case "c":
      return "it was C";
  }
}
```

Prefer this:

```ts
function doSomething(kind: MyKind) {
  if (kind === "a") return "it was A";
  if (kind === "b") return "it was B";
  if (kind === "c") return "it was C";
}
```

Even better, use hashmaps/objects for what would equate to single evaluation conditions:

```ts
const kind = {
  'a': 'it was A',
  'b': 'it was B',
  'c': 'it was C'
}
```

## You should generally never use the non-null assertion operator to trick the typescript compiler

You should never do this:

```ts
function doSomething(myObj: { value: string } | null) {
  console.log(myObj!.value);
}
```

Instead do this:

```ts
function doSomething(myObj: { value: string } | null) {
  if (!myObj) throw new Error("myObj is null");
  console.log(myObj.value);
}
```

## Error Handling

- Prefer try/catch blocks for error handling, even multiple within the same function if it keeps the code clean
- Try to keep the number of async operations or requests within each try/catch block to a minimum. Separate them into their own try/catch blocks for maintainability and debugging reasons 
- Use explicit error checking rather than ignoring potential failures

Example:

```ts
async function processData({ input, backup }: { input: string, backup?: string }) {
  try {
    return await primaryProcessor(input);
  } catch (primaryError) {
    if (!backup) throw new Error("Primary processing failed and no backup provided");
    
    try {
      return await backupProcessor(backup);
    } catch (backupError) {
      throw new Error(`Both primary and backup processing failed: ${primaryError.message}, ${backupError.message}`);
    }
  }
}
```

## Type Definitions

- Prefer type unions over enums
- Use utility types like `Pick`, `Omit`, `Partial` when they improve code clarity

Instead of this:

```ts
enum Status {
  PENDING = "pending",
  COMPLETED = "completed", 
  FAILED = "failed"
}
```

Prefer this:

```ts
type Status = "pending" | "completed" | "failed";
```

Example using utility types:

```ts
type User = {
  id: string;
  name: string;
  email: string;
  role: "admin" | "user";
  createdAt: Date;
}

type CreateUser = Omit<User, "id" | "createdAt">;
type UpdateUser = Partial<Pick<User, "name" | "email">>;
```

## Import/Export Patterns

- Prefer named exports over default exports
- Follow generally accepted import ordering conventions



