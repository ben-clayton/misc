# [WGSL] Struct Alignment Proposal

## Design principles

1. Provides a single **default layout** for any host-shareable datatype (primitive, structure, array).

    The layout is always valid for storage buffers. \
    The rules are practical - not too wasteful. \
    The rules are forward-looking - std140 / “extended” is considered legacy.

2. Structure layout is defined in terms of “size” and “layout” with no additional rules. The offset of any field is always exactly:

	<code>roundUp(alignment <sub>field</sub>, offset <sub>previous field</sub> + size <sub>previous field</sub>)</code>.

3. User defined data types and individual structure fields may diverge from the default layout, by using annotations to override the default alignment, size, and array stride values.

    `[[align(n), size(n)]]` can be decorated on user-defined types and structure fields. \
    `[[stride(n)]]` on array types.

    In most cases, this is required to make a datatype conform to the restrictions of uniform buffers.
    It can also be used to specify layouts denser than the default, e.g. to use with future optional capabilities (e.g. “scalar” layout).

4. _Rules_ for valid uniform/storage buffer layouts are defined separately from the _algorithm_ that defines the default layout

    This proposal intentionally does define the layout rules for the various storage classes, but does demonstrate how more restrictive storage class layouts are supported.

## Proposal

### Default alignments and sizes

Every data type has a default alignment and size value. When `[[size(n)]]` or `[[align(n)]]` decorations aren’t specified for the type, they’re the same as the `Align(S, storage)` and `Extent(V, storage)` in the current WGSL draft spec.

Default alignment and size for each WGSL data type:

| Type                              | Alignment (bytes)       | Size (bytes)                                                     |
|-----------------------------------|-------------------------|------------------------------------------------------------------|
| `f32`                             | 4                       | 4                                                                |
| `i32`                             | 4                       | 4                                                                |
| `u32`                             | 4                       | 4                                                                |
| `vec2`                            | 8                       | 8                                                                |
| `vec3`                            | 16                      | 12                                                               |
| `vec4`                            | 16                      | 16                                                               |
| `[[stride(S)]]`<br> `array<T, N>` | `alignof(T)`            | `N * S`                                                          |
| `[[stride(S)]]`<br> `array<T>`    | `alignof(T)`            | undefined                                                        |
| `matXxY` (col-major)              | `alignof(vecY)`         | `sizeof(array<vecY, X>)`                                         |
| `mat2x2`                          | 8                       | 16                                                               |
| `mat3x2`                          | 8                       | 24                                                               |
| `mat4x2`                          | 8                       | 32                                                               |
| `mat2x3`                          | 16                      | 32                                                               |
| `mat3x3`                          | 16                      | 48                                                               |
| `mat4x3`                          | 16                      | 64                                                               |
| `mat2x4`                          | 16                      | 32                                                               |
| `mat3x4`                          | 16                      | 48                                                               |
| `mat4x4`                          | 16                      | 64                                                               |
| `struct S`                        | max of field alignments | `roundUp(alignof(S), offsetof(last field) + sizeof(last field))` |

> Note: A matrix is laid out so that a program can form a reference to each of its column vectors, and that reference (pointer) is like a reference to a regular column vector.
In particular, the column vector must be aligned.

Structures and type aliases may have their defaults overridden by using the `[[size(n)]]` or `[[align(n)]]` decorations on their declaration:

Example:

```rust
// Override default size and alignment
[[size(16), align(32)]]
struct S {
  u32 i;
};

// Override default size, keep default alignment
[[size(16)]]
type RGBX = vec3<f32>;
```

### Default array stride

| Type          | Default stride (bytes)           |
|---------------|----------------------------------|
| `array<T, N>` | `roundUp(alignof(T), sizeof(T))` |
| `array<T>`    | `roundUp(alignof(T), sizeof(T))` |


### Structure Layouts

The rules for aligning a structure field:

* A field uses the alignment and size of it's type, unless the field is explicitly annotated with `[[align(n)]]` and / or `[[size(n)]]` decorations, in which case these field decorations take precedence.
* The first field is always at offset 0 of the structure.
* Subsequent fields have an offset that has the byte offset: <code>roundUp(alignment <sub>field</sub>, offset <sub>previous field</sub> + size <sub>previous field</sub>)</code>.
* The default alignment of a structure is the same as the field with the largest alignment.
* The default size of a structure is equal to: `roundUp(alignof(S), offsetof(last field) + sizeof(last field))`
* Structures nested in other structures behave the same as any other field type.

### Default Structure Layout Example

```rust
/*             align(8)  size(24)  */ [[block]] struct A {
/* offset(0)   align(4)  size(4)   */     u : f32;
/* offset(4)   align(4)  size(4)   */     v : f32;
/* offset(8)   align(8)  size(8)   */     w : vec2<f32>;
/* offset(16)  align(4)  size(4)   */     x : f32;
/* offset(20)  align(1)  size(4)          -- implicit struct size padding -- */
                                      };

/*             align(16) size(160) */ [[block]] struct B {
/* offset(0)   align(8)  size(8)   */     a : vec2<f32>;
/* offset(8)   align(1)  size(8)          -- implicit field alignment padding -- */
/* offset(16)  align(16) size(12)  */     b : vec3<f32>;
/* offset(28)  align(4)  size(4)   */     c : f32;
/* offset(32)  align(4)  size(4)   */     d : f32;
/* offset(36)  align(1)  size(12)         -- implicit field alignment padding -- */
/* offset(40)  align(8)  size(24)  */     e : A;
/* offset(64)  align(16) size(12)  */     f : vec3<f32>;
/* offset(76)  align(1)  size(4)          -- implicit field alignment padding -- */
/* offset(80)  align(8)  size(24)  */     g : [[stride(24)]] array<A, 3>;
/* offset(152) align(4)  size(4)   */     h : 32;
/* offset(156) align(1)  size(4)          -- implicit struct size padding -- */
                                      };

[[group(0), binding(0)]]
var<storage> storage_buffer : B;
```

<sub>[Example translated to GLSL](http://shader-playground.timjones.io/83be7e37fb33241d77fd0e467490c393).</sub>

Observe that:

* `struct A`'s size is padded to 24 bytes (`roundUp(20, 8)`)
* `struct A`'s alignment is equal to the field with the largest alignment (`w`)
* `c`, packs into end of the `vec3` of `b`.
* `g` has the alignment of the array element (`A`)

> Note: With the fixed type alignment and sizes of this initial proposal, scalars can be packed into the *end* of a vector, not the front.

The default layout rules will always produce layouts that are valid for storage buffers (without requiring explicit `[[align(n)]]` / `[[size(n)]]` decorations).
The same may not be true for non-storage buffers.

### Conforming to more constrained storage classes

Some storage classes are (at least initially) likely to impose tighter constraints on structure layouts than the default layout.

It is the developer's responsibility to ensure that structure layouts are valid for all storage class uses.
Field offsets and structure sizes that are invalid for a storage class that uses that structure will result in a compilation error.

For example, a uniform storage class might have a stricter set of layout rules to the default layout.
If we used the `struct B` definition from the above in a uniform buffer, we may encounter the following compilation errors:

```rust
/*             align(8)  size(24)  */ [[block]] struct A {
╭╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╮
╎                                     ^^^^^^^^^^^^^^^^^^^^                     ╎
╎ error: struct A size is 24 bytes.                                            ╎
╎ Uniform buffers require structure sizes to be a multiple of 16 bytes.        ╎
╰╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╯
/* offset(0)   align(4)  size(4)   */     u : f32;
/* offset(4)   align(4)  size(4)   */     v : f32;
/* offset(8)   align(8)  size(8)   */     w : vec2<f32>;
/* offset(16)  align(4)  size(4)   */     x : f32;
/* offset(20)  align(1)  size(4)          -- implicit struct size padding -- */
                                      };

/*             align(16) size(160) */ [[block]] struct B {
/* offset(0)   align(8)  size(8)   */     a : vec2<f32>;
/* offset(8)   align(1)  size(8)          -- implicit field alignment padding -- */
/* offset(16)  align(16) size(12)  */     b : vec3<f32>;
/* offset(28)  align(4)  size(4)   */     c : f32;
/* offset(32)  align(4)  size(4)   */     d : f32;
/* offset(36)  align(1)  size(12)         -- implicit field alignment padding -- */
/* offset(40)  align(8)  size(24)  */     e : A;
╭╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╮
╎                                         ^^^^^^                               ╎
╎ error: Structure field has byte offset 40.                                   ╎
╎ Uniform buffers require structures to be 16 byte aligned.                    ╎
╰╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╯
                                          ...
                                      };

[[group(0), binding(0)]]
var<uniform> uniform_buffer : B;
```

The developer can solve these compilation errors in a number of ways:

#### (1) Using `[[align]]` and `[[size]]` on struct

```rust
                                    ╭╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╮
                                    ╎ [[align(16), size(32)]] ╎
                                    ╰╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╯
/*             align(16) size(32)  */ [[block]] struct A {
/* offset(0)   align(4)  size(4)   */     u : f32;
/* offset(4)   align(4)  size(4)   */     v : f32;
/* offset(8)   align(8)  size(8)   */     w : vec2<f32>;
/* offset(16)  align(4)  size(4)   */     x : f32;
/* offset(20)  align(1)  size(12)         -- implicit struct size padding -- */
                                      };

                                    ╭╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╮
                                    ╎ [[align(16), size(208)]] ╎
                                    ╰╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╯
/*             align(16) size(208) */ [[block]] struct B {
/* offset(0)   align(8)  size(8)   */     a : vec2<f32>;
/* offset(8)   align(1)  size(8)          -- implicit field alignment padding -- */
/* offset(16)  align(16) size(12)  */     b : vec3<f32>;
/* offset(28)  align(4)  size(4)   */     c : f32;
/* offset(32)  align(4)  size(4)   */     d : f32;
/* offset(36)  align(1)  size(12)         -- implicit field alignment padding -- */
/* offset(48)  align(16) size(32)  */     e : A;
/* offset(80)  align(16) size(12)  */     f : vec3<f32>;
/* offset(92)  align(1)  size(4)          -- implicit field alignment padding -- */
/* offset(96)  align(16) size(96)  */     g : [[stride(32)]] array<A, 3>;
/* offset(192) align(4)  size(4)   */     h : 32;
/* offset(196) align(1)  size(12)         -- implicit struct size padding -- */
                                      };

[[group(0), binding(0)]]
var<storage> storage_buffer : B;

```

#### (2) Using `[[align]]` and `[[size]]` on fields

```rust
/*             align(8)  size(32)  */ [[block]] struct A {
/* offset(0)   align(4)  size(4)   */     u : f32;
/* offset(4)   align(4)  size(4)   */     v : f32;
/* offset(8)   align(8)  size(8)   */     w : vec2<f32>;
                                        ╭╌╌╌╌╌╌╌╌╌╌╌╌╌╌╮
/* offset(16)  align(4)  size(16)   */  ╎ [[size(16)]] ╎ x: f32;
                                        ╰╌╌╌╌╌╌╌╌╌╌╌╌╌╌╯
                                      };

/*             align(16) size(208) */ [[block]] struct B {
/* offset(0)   align(8)  size(8)   */     a : vec2<f32>;
/* offset(8)   align(1)  size(8)          -- implicit field alignment padding -- */
/* offset(16)  align(16) size(12)  */     b : vec3<f32>;
/* offset(28)  align(4)  size(4)   */     c : f32;
/* offset(32)  align(4)  size(4)   */     d : f32;
/* offset(36)  align(1)  size(12)         -- implicit field alignment padding -- */
                                        ╭╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╮
/* offset(48)  align(16) size(32)  */   ╎ [[align(16)]] ╎ e : A;
                                        ╰╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╯
/* offset(80)  align(16) size(12)  */     f : vec3<f32>;
/* offset(92)  align(1)  size(4)          -- implicit field alignment padding -- */
/* offset(96)  align(8)  size(96)  */     g : [[stride(32)]] array<A, 3>;
/* offset(192) align(4)  size(4)   */     h : 32;
/* offset(196) align(1)  size(12)         -- implicit struct size padding -- */
                                      };

[[group(0), binding(0)]]
var<storage> storage_buffer : B;
```

#### (3) Using padding fields

```rust
/*             align(8)  size(32)  */ [[block]] struct A {
/* offset(0)   align(4)  size(4)   */     u : f32;
/* offset(4)   align(4)  size(4)   */     v : f32;
/* offset(8)   align(8)  size(8)   */     w : vec2<f32>;
/* offset(16)  align(4)  size(4)   */     x : f32;
                                        ╭╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╮
/* offset(20)  align(4)  size(12)  */   ╎ _ : array<f32, 3>; ╎
                                        ╰╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╯
                                      };

/*             align(16) size(208) */ [[block]] struct B {
/* offset(0)   align(8)  size(8)   */     a : vec2<f32>;
/* offset(8)   align(1)  size(8)          -- implicit field alignment padding -- */
/* offset(16)  align(16) size(12)  */     b : vec3<f32>;
/* offset(28)  align(4)  size(4)   */     c : f32;
/* offset(32)  align(4)  size(4)   */     d : f32;
/* offset(36)  align(1)  size(12)         -- implicit field alignment padding -- */
/* offset(48)  align(16) size(32)  */     e : A;
/* offset(80)  align(16) size(12)  */     f : vec3<f32>;
/* offset(92)  align(1)  size(4)          -- implicit field alignment padding -- */
/* offset(96)  align(8)  size(96)  */     [[stride(32)]] array<A, 3>;
/* offset(192) align(4)  size(4)   */     h : 32;
/* offset(196) align(1)  size(12)         -- implicit struct size padding -- */
                                      };

[[group(0), binding(0)]]
var<storage> storage_buffer : B;
```
