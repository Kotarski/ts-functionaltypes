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

const pipeline = [
   (s: number) => 4,
   (n: number) => "hello",
   (n: string) => 5,
   (n: number) => 4
] as const  // As const will keep pipeline as a tuple

const pipeWithPipeline = pipe(
   ...pipeline
)

const pipeline = [
   (s: string) => 4,
   (n: number) => "hello",
   (n: string) => 5,
   (n: number) => 4
] as const

const pipeWithPipeline = pipe(
   ...pipeline
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

# Limitations
```Typescript

// Cannot detect that the input and output type of the sandwich filling should be the same
const pipeWithIdentity = pipe(
   (n: string) => "string",
   <T extends any>(i: T) => i, 
   (s: number) => 4 // Ideally would be error
)

const pipeWithIdentity2 = pipe(
   (n: string) => "string",
   <T extends number>(i: T) => i, // This does error
   (s: number) => 4
)

// Resulting pipe interface: (string => any) 
// Ideally would be: (string => number)
const pipeWithIdentity3 = pipe(
   (n: string) => "string",
   (s: number) => 4,
   <T extends any>(i: T) => i
)


```

# Install
[https://www.npmjs.com/package/ts-functionaltypes](https://www.npmjs.com/package/ts-functionaltypes)
```
$ npm install --save-dev ts-functionaltypes
```

# Github
[https://github.com/Kotarski/ts-functionaltypes](https://github.com/Kotarski/ts-functionaltypes)
