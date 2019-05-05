# ts-functionaltypes
Provides fully typed types for pipe and compose

# Included
```Typescript
/* The type for the pipe function */
export type Pipe = <F extends Unary[]>(...funcs: Pipable<F>) => (i: ParameterUnary<F[0]>) => ReturnType<F[PrevN<F["length"]>]>

/* The type for the compose function */
export type Compose = <F extends Unary[]>(...funcs: Composable<F>) => (i: ParameterUnary<F[PrevN<F["length"]>]>) => ReturnType<F[0]>

```
# Usage
```Typescript
const pipe: Pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const compose: Compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const builder = pipe(
   (i: {a:0}) => ({a:0, b:0}),
   (i: {a:0, b:0}) => ({a:0, b:0, c:0}),
   (i: {a:0, b:0, c:0}) => ({a:0, b:0, c:0, d:0}),
   (i: { a: 0, b: 0, c: 0, d: 0 }) => ({ a: 0, b: 0, c: 0, d: 0, e: 0 })
)

const builder = compose(
   (i: {a:0, b:0, c:0, d:0}) => ({a:0, b:0, c:0, d:0, e:0}),
   (i: {a:0, b:0, c:0}) => ({a:0, b:0, c:0, d:0}),
   (i: {a:0, b:0}) => ({a:0, b:0, c:0}),
   (i: {a:0}) => ({a:0, b:0}),
)

// Fails (correctly)
const composefail = compose(
   (s: string) => 4,
   (n: number) => "hello",
   (n: string) => 5,
   (n: number) => 4,
)

// Fails (correctly)
const composefail = compose(
   (s: string) => 4,
   (n: number) => "hello",
   (n: number) => 5,
   (s: string) => 4
)

```
# Install
[https://www.npmjs.com/package/ts-strictargs](https://www.npmjs.com/package/ts-strictargs)
```
$ npm install --save-dev ts-strictargs
```

# Github
[https://github.com/Kotarski/ts-strictargs](https://github.com/Kotarski/ts-strictargs)
