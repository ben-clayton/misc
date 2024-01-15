# WGSL type info proposal

This proposal introduces two new fundamental changes to expressions:

- [Structure access expressions](https://www.w3.org/TR/WGSL/#struct-access-expr) can also be performed on [type expressions](https://www.w3.org/TR/WGSL/#type-expr). This dot-operator will be renamed to `Member access expression`.
- `Member access expression` may resolve to a value, type or [enumerant](https://www.w3.org/TR/WGSL/#enumeration-types).
- Builtin functions can return a types as well as values.

## New type members

### All types

All types will have the following members:

 Name                         | Description                        | Example
 -----------------------------|------------------------------------|---------------
 `const is_host_sharable : bool` | True if the type is host sharable | `if (T.is_host_sharable) {`

### Host sharable types

All host sharable types will have new members that reflect the type's memory size and alignment:

 Name                         | Description                        | Example
 -----------------------------|------------------------------------|---------------
 `const align : abstract-int` | The alignment in bytes of the type | `const c = vec3f.align;`
 `const size : abstract-int`  | The size in bytes of the type      | `const c = i32.size;`

### Array types

Both fixed-size and runtime-sized array types will have the following additional members:

| Name                          | Description                                                       | Example                        |
|-------------------------------|-------------------------------------------------------------------|--------------------------------|
| `const stride : abstract-int` | The number of bytes between two consecutive elements in the array | `const c = array<i32>.stride;` |
| `element : T`                 | The element type                                                  | `var v : array<i32>.element;`  |

Fixed-size array types will have an additional `length` member:

| Name                          | Description                         | Example                                         |
|-------------------------------|-------------------------------------|-------------------------------------------------|
| `const length : abstract-int` | The number of elements in the array | `for (var i = 0; i < arr.length; i++) { ... }`  |

### Vector types

Vector types will have the following additional members:

| Name                          | Description                            | Example                   |
|-------------------------------|----------------------------------------|---------------------------|
| `const length : abstract-int` | The number of components in the vector | `const c = vec4f.length;` |
| `element : T`                 | The vector component type              | `var v : vec3i.element;`  |

### Matrix types

Matrix types will the following additional members:

| Name                           | Description                                                               | Example                     |
|--------------------------------|---------------------------------------------------------------------------|-----------------------------|
| `column : C`                   | The matrix column vector type                                             | `var c : mat2x3.column;`    |
| `row : R`                      | The matrix row vector type                                                | `var r : mat2x3.row;`       |
| `element : T`                  | The matrix element type (equal to `M.row.element` and `M.column.element`) | `var v : vec3i.element;`    |
| `const columns : abstract-int` | The number of columns in the matrix (equal to `M.row.length`)             | `const c : mat2x3.columns;` |
| `const rows : abstract-int`    | The number of rows in the matrix (equal to `M.column.length`)             | `const r : mat2x3.rows;`    |

### Atomic types

Atomic types will have the following additional members:

| Name           | Description             | Example                         |
|----------------|-------------------------|---------------------------------|
| `element : T`  | The atomic integer type | `var v : atomic<i32>.element;`  |

### Structure types

Structure types will have a type member for each declared member of the structure. Each type member will have the following sub-members:

| Name                          | Description                      | Example (`struct S { a : i32, b : u32, c : f32 }`) |
|-------------------------------|----------------------------------|----------------------------------------------------|
| `const offset : abstract-int` | The byte offset in the structure | `const o = S.a.offset;`                            |
| `type : T`                    | The type of the member           | `let a : S.b.type = S().b;`                        |

### Sampled, Multisampled and Depth texture types

Sampled and Multisampled and Depth texture types will have the following additional members:

| Name                              | Description                                                                                  | Example (`alias T = texture_2d<i32>;`) |
|-----------------------------------|----------------------------------------------------------------------------------------------|----------------------------------------|
| `const dimensions : abstract-int` | The [dimensionality](https://www.w3.org/TR/WGSL/#texture-dimensionality) of the texture type | `const d = T.dimensions;`              |
| `const is_arrayed : bool`         | True if the texture is arrayed                                                               | `if (T.is_arrayed) { ... }`            |
| `texel : T`                       | The texel format type of the texture type                                                    | `var i : T.texel;`                     |

### Storage texture types

Storage texture types will have the following additional members:

| Name                              | Description                                                                                  | Example  (`alias T = texture_storage_2d<r32sint, read>;`) |
|-----------------------------------|----------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| `const dimensions : abstract-int` | The [dimensionality](https://www.w3.org/TR/WGSL/#texture-dimensionality) of the texture type | `const d = T.dimensions;`                                 |
| `format : FORMAT`                 | The [format](https://www.w3.org/TR/WGSL/#storage-texel-formats) of the texture type          | `alias T2 = texture_storage_2d<T.format, write>;`         |
| `access : ACCESS`                 | The [access](https://www.w3.org/TR/WGSL/#memory-access) of the texture type                  | `alias T2 = texture_storage_2d<r32float, T.access>;`      |

### Pointer types

Pointer types will have the following additional members:

| Name                    | Description                                                                        | Example  (`alias P = ptr<storage, i32, read_write>`) |
|-------------------------|------------------------------------------------------------------------------------|------------------------------------------------------|
| `element : T`           | The [store type](https://www.w3.org/TR/WGSL/#store-type) of the pointer type       | `var v : array<i32>.element;`                        |
| `space : ADDRESS_SPACE` | The [address space](https://www.w3.org/TR/WGSL/#memory-access) of the pointer type | `alias P2 = ptr<P.space, u32, read>;`                |
| `access : ACCESS`       | The [access](https://www.w3.org/TR/WGSL/#memory-access) of the pointer type        | `alias P2 = ptr<storage, u32, P.access>;`            |

## `typeof(E)`

`typeof(E)` is a new builtin function that returns the type of the value-expression `E`.

If `E` is of a reference type, then `typeof` returns the store-type of the reference.
