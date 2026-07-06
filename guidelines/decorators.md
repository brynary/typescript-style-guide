# Decorators

## Rule

Do not use decorators.

## Why

Decorators are non-erasable runtime syntax that the type-stripping baseline in [tsconfig-baseline.md](tsconfig-baseline.md) rules out, and they hide behavior behind an annotation. Plain functions and explicit wiring are clearer and greppable.

## Do

- Wrap behavior with a higher-order function that takes and returns the value it augments.
- Register handlers, routes, and metadata with explicit function calls or plain data.
- Compose cross-cutting concerns (logging, timing, retries) as functions that call through to the wrapped function.

## Avoid

- `@decorator` syntax on classes, methods, fields, or parameters.
- Reflection-metadata based dependency injection that relies on decorators.
- Reaching for a library whose primary API is decorator-driven when a function-based alternative exists.

## Example

```ts
interface Handler {
  (request: Request): Response;
}

function withLogging(handler: Handler): Handler {
  return (request) => {
    const response = handler(request);
    console.log(`${request.method} ${request.url} -> ${response.status}`);
    return response;
  };
}

function handleHealth(): Response {
  return new Response("ok");
}

const loggedHealth = withLogging(handleHealth);
```

## Exceptions

- If a framework in scope mechanically requires decorators, isolate them to the thin adapter that the framework instantiates and keep all other code decorator-free.
