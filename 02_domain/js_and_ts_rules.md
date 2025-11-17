# General Coding Style Rules

Keep your explanations brief and to the point.

## Code Simplicity and Readability

The primary goal is to write code that is easy to understand and maintain. Code is read far more often than it is written. Prioritize clarity for your future self and for your teammates over cleverness or premature optimization.

### Prefer Simplicity Over "Clever" Code
Avoid "clever" one-liners or overly complex expressions that require a reader to pause and decipher them. If a piece of logic is complex, it's better to write it out in a more verbose but straightforward way. A developer should be able to understand the intent of a piece of code within seconds.

**Instead of this (clever but dense):**
```ts
const activeAdmins = users.filter(u => u.active && u.role === 'admin').map(u => ({...u, name: u.name.toUpperCase()})).sort((a, b) => b.posts - a.posts);
```

**Prefer this (clear and sequential):**
```ts
const activeAdmins = users.filter(user => user.active && user.role === 'admin');

const capitalizedAdmins = activeAdmins.map(user => ({
  ...user,
  name: user.name.toUpperCase(),
}));

const sortedAdmins = capitalizedAdmins.sort((a, b) => b.posts - a.posts);
```

### Embrace "WET" Programming Before Abstracting
Counter to the well-known "DRY" (Don't Repeat Yourself) principle, it's often better to "Write Everything Twice" (or even three times). Rushing to create an abstraction for the first instance of code duplication can lead to the *wrong* abstraction, which is more costly to fix than duplicated code.

By allowing a little duplication, the true, underlying pattern will emerge more clearly. Once you've written similar logic for the third time, you'll have a much better understanding of the requirements and can create a more robust and accurate abstraction. At that point, you should prefer iteration and modularization over continued code duplication.

### Keep Abstractions Shallow
When you do abstract complex logic into helper functions, avoid creating deep chains of function calls. A function that calls a helper, which calls another helper, which calls *another* helper, becomes fragmented and difficult to trace. This can hide the core logic and make debugging painful.

For most applications, a single layer of abstraction is sufficient. If a helper function becomes so complex that it needs its own helpers, consider if the initial function can be simplified or if the helper is trying to do too much. The goal is to create small, pure functions with descriptive names and few parameters, but not to create a maze of them.

**Avoid this (deeply nested abstractions):**
```ts
// Hard to follow the flow of data and logic
function mainOperation(data) {
  const processedData = processStepOne(data);
  return finalize(processedData);
}

function processStepOne(data) {
  const validated = validate(data);
  return transform(validated);
}

function validate(data) {
  // ...validation logic...
}

function transform(data) {
  // ...transformation logic...
}

function finalize(data) {
  // ...final steps...
}
```

**Prefer this (shallow, focused helpers):**
```ts
function mainOperation(data) {
  // Core logic is clear and easy to read
  if (!isValid(data)) {
    throw new Error("Invalid data");
  }

  const transformedData = transformData(data);
  const result = performFinalCalculation(transformedData);

  return result;
}

// Helper for a genuinely complex, self-contained task
function transformData(data) {
  // ...transformation logic is isolated here...
  return transformed;
}

// Another helper for a distinct step
function performFinalCalculation(data) {
  // ...calculation logic...
  return calculation;
}
```

## General Rules

- Use descriptive variable and function names
- Use functional and declarative programming patterns; avoid unnecessary classes except for state machines
- Use TypeScript for all code; prefer types over interfaces
- Implement proper lazy loading for images and other assets
- Wherever appropriate, opt for early returns as opposed to multiple or nested if/else conditions or switch statements

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



