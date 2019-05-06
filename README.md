# ts-functionaltypes
Provides fully typed types for pipe and compose

# Included
```Typescript
/* The type for the pipe function */
export type Pipe = <F extends Unary[]>(...funcs: UnariesToPiped<F>) => (i: ParameterUnary<F[0]>) => ReturnType<F[PrevN<F["length"]>]>

/* The type for the compose function */
export type Compose = <F extends Unary[]>(...funcs: UnariesToComposed<F>) => (i: ParameterUnary<F[PrevN<F["length"]>]>) => ReturnType<F[0]>

```
# Usage
```Typescript
const pipe: Pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

const compose: Compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const pipePass = pipe(
   (s: string) => 4,
   (n: number) => "hello",
   (n: string) => 5,
   (n: number) => 4
)

// Fails (correctly)
const pipeFail = compose(
   (s: string) => 4,
   (n: number) => "hello",
   (n: number) => 5, /* Fail: 
      Argument of type '(n: number) => number' is not assignable to parameter of type '(i: string) => number'.
      Types of parameters 'n' and 'i' are incompatible.
      Type 'string' is not assignable to type 'number'.*/
   (n: number) => 4
)


const composePass = compose(
   (s: string) => 4,
   (n: number) => "hello",
   (n: number) => 5,
   (s: string) => 4
)


// Fails (correctly)
const composefail = compose(
   (s: string) => 4,
   (n: string) => "hello",
   (n: number) => 5, /* Fail:
         Argument of type '(n: number) => number' is not assignable to parameter of type '(i: number) => string'.
         Type 'number' is not assignable to type 'string'.
      */
   (s: string) => 4
)

```
# Install
[https://www.npmjs.com/package/ts-functionaltypes](https://www.npmjs.com/package/ts-functionaltypes)
```
$ npm install --save-dev ts-functionaltypes
```

# Github
[https://github.com/Kotarski/ts-functionaltypes](https://github.com/Kotarski/ts-functionaltypes)
