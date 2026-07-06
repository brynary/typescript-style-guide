# Function Signatures

## Rule

Annotate an explicit return type on every exported function, and let internal functions infer their return type.

## Why

Explicit return types make a package's public contract stable and reviewable, and they catch accidental changes at the boundary. Inference keeps private code terse where the contract is local.

## Do

- Write the return type on every exported or otherwise public function and method.
- Let inference handle return types of non-exported, local functions.
- Prefer a union parameter or an options object over function overloads.
- Group parameters into a single options object once a function takes more than three, or takes two or more of the same type.
- Destructure the options object in the parameter list and give optional fields defaults there.

## Avoid

- Omitting the return type on an exported function.
- Overload signatures where a union or an options object expresses the same intent.
- Long positional parameter lists, especially several booleans or same-typed arguments in a row.
- Mutating a passed-in options object.

## Example

```ts
interface CreateUserOptions {
  readonly email: string;
  readonly displayName: string;
  readonly isAdmin?: boolean;
}

interface User {
  readonly email: string;
  readonly displayName: string;
  readonly isAdmin: boolean;
}

export function createUser(options: CreateUserOptions): User {
  const { email, displayName, isAdmin = false } = options;
  return { email, displayName, isAdmin };
}

function normalizeEmail(email: string) {
  return email.trim().toLowerCase();
}
```

## Exceptions

- Keep overloads only when a union return type would force callers to narrow a result they already know statically, and no options object removes the ambiguity.
