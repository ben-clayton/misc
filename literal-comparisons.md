# Literal / constant-expression behavior comparisons

## Unsuffixed FP quantization

```ts
var f : f32 = 16777216.0 + 1.0 - 1.0;
```

| Language                                                   | Value of `f`                                                                           | Notes                                                                                                                                                                    |
|------------------------------------------------------------|----------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL (DXC)                                                 | [`16777216.0`](https://shader-playground.timjones.io/3bdf50518ca9849c4b0c2525598ca05b) | [Arithmetic performed in `f64`, then converted to `f32`](https://github.com/gpuweb/gpuweb/pull/2306#issuecomment-988251893)                                              |
| HLSL (FXC)                                                 | [`16777216.0`](https://shader-playground.timjones.io/c02b5955516dcebeadb30da17e823e9e) | Arithmetic performed in `f64`?                                                                                                                                           |
| MSL 2.0                                                    | [`16777215.0`](https://shader-playground.timjones.io/99174b74edf52f0a87c8e8885902fb3b) | Arithmetic performed in `f32` as MSL does not support `f64`.<br>See: [2.1 Scalar Data Types](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf) |
| C++                                                        | [`16777216.0`](https://godbolt.org/z/9ozjqGba5)                                        | Unsuffixed float literals are `double`                                                                                                                                   |
| Go                                                         | [`16777216.0`](https://go.dev/play/p/9-reyz1kx2H)                                      | Arithmetic performed in high-precision, then converted to `f32`                                                                                                          |
| JavaScript                                                 | [`16777216.0`](http://jsfiddle.net/dofrzb6L/)                                          | Arithmetic performed in `f64`, no other number type.                                                                                                                     |
| WGSL [#2200](https://github.com/gpuweb/gpuweb/issues/2200) | `16777216.0`                                                                           | Arithmetic performed in `f64`, then converted to `f32`                                                                                                                   |
| WGSL [#2306](https://github.com/gpuweb/gpuweb/pull/2306)   | `16777215.0`                                                                           | `f`'s `f32` type propagates to all literals before evaluation                                                                                                            |

---

## `.f` Suffixed FP quantization


```ts
var f : f32 = 16777216.f + 1.f - 1.f;
```

| Language                                                   | Value of `f`                                                                           | Notes                                                                     |
|------------------------------------------------------------|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| HLSL (DXC)                                                 | [`16777215.0`](https://shader-playground.timjones.io/5bc7e830f56d2481222c6ab5a5847fde) | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |
| HLSL (FXC)                                                 | [`16777216.0`](https://shader-playground.timjones.io/4be4158715d1cc7e96cd435251703e03) | `.f` suffix does not appear to change type to `f32`                       |                                                                                                                                                          |
| MSL 2.0                                                    | [`16777215.0`](https://shader-playground.timjones.io/27cf6f73a80eea4b15ed0f954bf19fc0) | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |
| C++                                                        | [`16777215.0`](https://godbolt.org/z/hsxdTesze)                                        | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |
| Go                                                         | -                                                                                      | No `f` suffix                                                             |
| JavaScript                                                 | -                                                                                      | No `f` suffix                                                             |
| WGSL [#2200](https://github.com/gpuweb/gpuweb/issues/2200) | `16777215.0`                                                                           | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |
| WGSL [#2306](https://github.com/gpuweb/gpuweb/pull/2306)   | `16777215.0`                                                                           | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |

---

## Unsuffixed FP quantization via inferred-type `const`

```ts
const C = 16777216.0 + 1.0;
var f : f32 = C - 1.0;
```

| Language                                                   | Value of `f`                                                                           | Notes
|------------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL (DXC)                                                 | -                                                                                      | No way to declare a variable with inferred type                                                                                                                             |
| HLSL (FXC)                                                 | -                                                                                      | No way to declare a variable with inferred type                                                                                                                             |
| MSL 2.0                                                    | [`16777215.0`](https://shader-playground.timjones.io/06494219e828ba4ec076e419666d4b24) | Arithmetic performed in `f32` as MSL does not support `f64`.<br>See: [2.1 Scalar Data Types](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf)    |
| C++                                                        | [`16777216.0`](https://godbolt.org/z/xoTM5vzGf)                                        | Unsuffixed float literals are `double`, `auto` infers to this.                                                                                                              |
| Go                                                         | [`16777216.0`](https://go.dev/play/p/EXL1jGYHzS3)                                      | Arithmetic performed in high-precision, then converted to `f32`                                                                                                             |
| JavaScript                                                 | [`16777216.0`](http://jsfiddle.net/205nq487/)                                          | Arithmetic performed in `f64`, no other number type.                                                                                                                        |
| WGSL [#2200](https://github.com/gpuweb/gpuweb/issues/2200) | `16777216.0`                                                                           | Arithmetic performed in `f64`, then converted to `f32`                                                                                                                      |
| WGSL [#2306](https://github.com/gpuweb/gpuweb/pull/2306)   | `16777215.0`                                                                           | If `const` has fixed type, then the default is `f32`.<br>If `const` has a `literal type`, then the leaf-expressions are still `f32` by propagation from `f`.                |

---

## Suffixed FP quantization via inferred-type `const`

```ts
const C = 16777216.f + 1.f;
var f : f32 = C - 1.f;
```

| Language                                                   | Value of `f`                                                                           | Notes
|------------------------------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------
| HLSL (DXC)                                                 | -                                                                                      | No way to declare a variable with inferred type                           |
| HLSL (FXC)                                                 | -                                                                                      | No way to declare a variable with inferred type                           |
| MSL 2.0                                                    | [`16777215.0`](https://shader-playground.timjones.io/7ba7dbb87732d20ed3be271fce5c05cc) | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |
| C++                                                        | [`16777215.0`](https://godbolt.org/z/vzxc787cj)                                        | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |
| Go                                                         | -                                                                                      | No `f` suffix                                                             |
| JavaScript                                                 | -                                                                                      | No `f` suffix                                                             |
| WGSL [#2200](https://github.com/gpuweb/gpuweb/issues/2200) | `16777215.0`                                                                           | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |
| WGSL [#2306](https://github.com/gpuweb/gpuweb/pull/2306)   | `16777215.0`                                                                           | Expected result for quantizing `16777216.f + 1.f` to `f32` before `- 1.f` |

---

## i32 overflow

```ts
var i : i32 = 0x70000000 * 2 / 2;
```

| Language                                                   | Value of `i`                                                                                                      | Notes                                                                                                                       |
|------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| HLSL (DXC)                                                 | [`0x70000000`](https://shader-playground.timjones.io/2ae731b28c24d56c9122ad5eeff5caf7)                            | [Arithmetic performed in `i64`, then converted to `i32`](https://github.com/gpuweb/gpuweb/pull/2306#issuecomment-988251893) |
| HLSL (FXC)                                                 | [`158456325028528680000000000000.0`](https://shader-playground.timjones.io/e7d1388c66711f1ab63df2c29edd46c3) (?!) | No idea                                                                                                                     |
| MSL 2.0                                                    | [Undefined Behavior](https://shader-playground.timjones.io/f337d2a63f31595b91511440da2f4d8e)                      | `i32` overflow is UB. Interestingly no warnings or errors                                                                   |
| C++                                                        | [Undefined Behavior](https://godbolt.org/z/EKPWMP8rE)                                                             | `i32` overflow is UB. Clang warns.                                                                                          |
| Go                                                         | [`0x70000000`](https://go.dev/play/p/av43785A-76)                                                                 | Arithmetic performed in high-precision, then converted to `i32`                                                             |
| JavaScript                                                 | [`0x70000000`](http://jsfiddle.net/205nq487/1/)                                                                   | Arithmetic performed in `f64`, then converted to `i32`                                                                      |
| WGSL [#2200](https://github.com/gpuweb/gpuweb/issues/2200) | `0x70000000`                                                                                                      | Arithmetic performed in `i64`, then converted to `i32`                                                                      |
| WGSL [#2306](https://github.com/gpuweb/gpuweb/pull/2306)   | Error                                                                                                             |                                                                                                                             |

---

## i32 overflow via inferred-type `const`

```ts
const C = 0x70000000 * 2;
var i : i32 = C / 2;
```

| Language                                                   | Value of `i`                                                                      | Notes                                                           |
|------------------------------------------------------------|-----------------------------------------------------------------------------------|-----------------------------------------------------------------|
| HLSL (DXC)                                                 | -                                                                                 | No way to declare a variable with inferred type                 |
| HLSL (FXC)                                                 | -                                                                                 | No way to declare a variable with inferred type                 |
| MSL 2.0                                                    | [`Error`](https://shader-playground.timjones.io/de529530b2f048219ea77a78c30faa86) | `i32` overflow is UB. Compiler errors.                          |
| C++                                                        | [`Error`](https://godbolt.org/z/bT9sf9x71)                                        | `i32` overflow is UB. Clang errors.                             |
| Go                                                         | [`0x70000000`](https://go.dev/play/p/phlUO-PxxPA)                                 | Arithmetic performed in high-precision, then converted to `i32` |
| JavaScript                                                 | [`0x70000000`](http://jsfiddle.net/205nq487/1/)                                   | Arithmetic performed in `f64`, then converted to `i32`          |
| WGSL [#2200](https://github.com/gpuweb/gpuweb/issues/2200) | `0x70000000`                                                                      | Arithmetic performed in `i64`, then converted to `i32`          |
| WGSL [#2306](https://github.com/gpuweb/gpuweb/pull/2306)   | Error                                                                             |                                                                 |

---

## Large number arithmetic to `f32`

```ts
var f : f32 = 1.0e+40 / 1.0e+35;
```

| Language                                                   | Value of `f`                                                                           | Notes
|------------------------------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------
| HLSL (DXC)                                                 | [`100000.0`](https://shader-playground.timjones.io/aa819b1b1127722336fec7b2a5a770bd)   | [Arithmetic performed in `f64`, then converted to `f32`](https://github.com/gpuweb/gpuweb/pull/2306#issuecomment-988251893)             |
| HLSL (FXC)                                                 | [`100000.0`](https://shader-playground.timjones.io/57dd5a6ae4d00e1314b0a46617ecc31f)   | Arithmetic performed in `f64`?                                                                                                          |
| MSL 2.0                                                    | -                                                                                      | MSL does not support `f64`.<br>See: [2.1 Scalar Data Types](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf) |
| C++                                                        | [`100000.0`](https://godbolt.org/z/7be6dTx3a)                                          | Unsuffixed float literals are `double`                                                                                                  |
| Go                                                         | [`100000.0`](https://go.dev/play/p/ZSrQYsCEyte)                                        | Arithmetic performed in high-precision, then converted to `f32`                                                                         |
| JavaScript                                                 | [`100000.0`](http://jsfiddle.net/tcubLmjx/)                                            | Arithmetic performed in `f64`, no other number type.                                                                                    |
| WGSL [#2200](https://github.com/gpuweb/gpuweb/issues/2200) | `100000.0`                                                                             | Arithmetic performed in `f64`, then converted to `f32`                                                                                  |
| WGSL [#2306](https://github.com/gpuweb/gpuweb/pull/2306)   | Error                                                                                  | `f` is `f32`, so this will be applied to both literals, but these cannot be represented as a `f32`                                      |

---

## Large number in inferred-type `const`

```ts
const C = 1.0e+40;
var f : f64 = C / 1.0e+35;
```

| Language                                                   | Value of `f`                                                                           | Notes
|------------------------------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------
| HLSL (DXC)                                                 | -                                                                                      | No way to declare a variable with inferred type                                                                                         |
| HLSL (FXC)                                                 | -                                                                                      | No way to declare a variable with inferred type                                                                                         |
| MSL 2.0                                                    | -                                                                                      | MSL does not support `f64`.<br>See: [2.1 Scalar Data Types](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf) |
| C++                                                        | [`100000.0`](https://godbolt.org/z/oGhv4vsG1)                                          | Unsuffixed float literals are `double`                                                                                                  |
| Go                                                         | [`100000.0`](https://go.dev/play/p/2DWJ3yaicR_S)                                       | Arithmetic performed in high-precision, then converted to `f32`                                                                         |
| JavaScript                                                 | [`100000.0`](http://jsfiddle.net/rq657y3x/)                                            | Arithmetic performed in `f64`, no other number type.                                                                                    |
| WGSL [#2200](https://github.com/gpuweb/gpuweb/issues/2200) | `100000.0`                                                                             | Arithmetic performed in `f64`, then converted to `f32`                                                                                  |
| WGSL [#2306](https://github.com/gpuweb/gpuweb/pull/2306)   | `100000.0` or error                                                                    | If `const` has fixed type, then errors as `C` would be `f32`, and number does not fit.                                                  |

---

Hex ⟷ `f32` / decimal reference:
| hex                 | float        |
|---------------------|--------------|
|`0x4b800000`         | `16777216.0` |
|`0x416fffffe0000000` | `16777215.0` |
|`0x4b7fffff`         | `16777215.0` |
|`0x4170000000000000` | `16777216.0` |
|`0x47c35000`         | `10000000.0` |
|`0x40f86a0000000000` | `10000000.0` |

| hex          | dec          |
|--------------|--------------|
| `0x70000000` | `1879048192` |
| `0xf0000000` | `4026531840` |
| `0x10000000` | `268435456`  |

