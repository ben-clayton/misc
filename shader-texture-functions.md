# Shader Texture Function Comparison

This document was created from the information found in:

* [Metal Shading Language Specification](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf) (MSL) \
  All functions added after Metal 1.0 have been omitted.
* [Direct3D HLSL Shader Model 4 Texture Object](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-to-type) \
  All functions added after HLSL Shader Model 5.1 have been omitted.
* [SPIR-V Specification 1.0](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html)

## Sampling functions

### Sample - 1D Texture

| Backend | Function                                                                                                                                                        | Comments                                                                                                                           |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Tv Texture1D.Sample(sampler, float coord[, int offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture1d-sample)                       |                                                                                                                                    |
| MSL     | `Tv texture1d.sample(sampler, float coord)`                                                                                                                     |                                                                                                                                    |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Requires the [`Sampled1D` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Sample - 1D Texture Array

| Backend | Function                                                                                                                                                        | Comments                                                                                                                                                         |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Tv Texture1DArray.Sample(sampler, float2 coord[, int offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture1darray-sample)            | Array index in `coord.y`.                                                                                                                                        |
| MSL     | `Tv texture1d_array.sample(sampler, float coord, uint array)`                                                                                                   |                                                                                                                                                                  |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[1]`.<br>Requires the [`Sampled1D` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Sample - 2D Texture

#### Sample - 2D Texture - Basic

| Backend | Function                                                                                                                                                        | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture2D.Sample(sampler, float2 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture-sample-overload)              |          |
| MSL     | `Tv texture2d.sample(sampler s, float2 coord[, int2 offset])`                                                                                                   |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - 2D Texture - Bias

| Backend | Function                                                                                                                                                                    | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture2D.SampleBias(sampler, float2 coord, float bias[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplebias)             |          |
| MSL     | `Tv texture2d.sample(sampler s, float2 coord, bias(float)[, int2 offset])`                                                                                                  |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - 2D Texture - Level

| Backend | Function                                                                                                                                                                  | Comments |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture2D.SampleLevel(sampler, float2 coord, float LOD[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplelevel)          |          |
| MSL     | `Tv texture2d.sample(sampler s, float2 coord, level(float)[, int2 offset])`                                                                                               |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

#### Sample - 2D Texture - Gradient

| Backend | Function                                                                                                                                                                       | Comments |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture2D.SampleGrad(sampler, float2 coord, float2 ddx, float2 ddy[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplegrad)    |          |
| MSL     | `Tv texture2d.sample(sampler s, float2 coord, gradient2d(float2 dx, float2 dy)[, int2 offset])`                                                                                |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

### Sample - 2D Texture Array

#### Sample - 2D Texture Array - Basic

| Backend | Function                                                                                                                                                        | Comments                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Tv Texture2DArray.Sample(sampler, float3 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-sample)           | Array index in `coord.z`.  |
| MSL     | `Tv texture2d_array.sample(sampler s, float2 coord, uint array[, int2 offset])`                                                                                 |                            |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[2]`. |

#### Sample - 2D Texture Array - Bias

| Backend | Function                                                                                                                                                                    | Comments                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Tv Texture2DArray.SampleBias(sampler, float3 coord, float bias[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplebias)   | Array index in `coord.z`.  |
| MSL     | `Tv texture2d_array.sample(sampler s, float2 coord, uint array, bias(float)[, int2 offset])`                                                                                |                            |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[2]`. |

#### Sample - 2D Texture Array - Level

| Backend | Function                                                                                                                                                                   | Comments                   |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Tv Texture2DArray.SampleLevel(sampler, float3 coord, float LOD[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplelevel) | Array index in `coord.z`.  |
| MSL     | `Tv texture2d_array.sample(sampler s, float2 coord, uint array, level(float)[, int2 offset])`                                                                              |                            |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod)  | Array index in `coord[2]`. |

#### Sample - 2D Texture Array - Gradient

| Backend | Function                                                                                                                                                                              | Comments                   |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Tv Texture2DArray.SampleGrad(sampler, float3 coord, float2 ddx, float2 ddy[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplegrad) | Array index in `coord.z`.  |
| MSL     | `Tv texture2d_array.sample(sampler s, float2 coord, uint array, gradient2d(float2 dx, float2 dy)[, int2 offset])`                                                                     |                            |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod)        | Array index in `coord[2]`. |

### Sample - 3D Texture

#### Sample - 3D Texture - Basic

| Backend | Function                                                                                                                                                        | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture3D.Sample(sampler, float3 coord[, int3 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture3d-sample)                     |          |
| MSL     | `Tv texture3d.sample(sampler s, float3 coord[, int3 offset])`                                                                                                   |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - 3D Texture - Bias

| Backend | Function                                                                                                                                                                    | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture3D.SampleBias(sampler, float3 coord, float bias[, int3 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture3d-samplebias)             |          |
| MSL     | `Tv texture3d.sample(sampler s, float3 coord, bias(float)[, int3 offset])`                                                                                                  |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - 3D Texture - Level

| Backend | Function                                                                                                                                                                  | Comments |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture3D.SampleLevel(sampler, float3 coord, float LOD[, int3 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture3d-samplelevel)          |          |
| MSL     | `Tv texture3d.sample(sampler s, float3 coord, level(float)[, int3 offset])`                                                                                               |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

#### Sample - 3D Texture - Gradient

| Backend | Function                                                                                                                                                                       | Comments |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture3D.SampleGrad(sampler, float3 coord, float3 ddx, float3 ddy[, int3 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture3d-samplegrad)    |          |
| MSL     | `Tv texture3d.sample(sampler s, float3 coord, gradient3d(float3 dx, float3 dy)[, int3 offset])`                                                                                |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

### Sample - Cube Texture

#### Sample - Cube Texture - Basic

| Backend | Function                                                                                                                                                                    | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv TextureCube.Sample(sampler, float3 coord)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-sample)                                            |          |
| MSL     | `Tv texturecube.sample(sampler s, float3 coord)`                                                                                                                            |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - Cube Texture - Bias

| Backend | Function                                                                                                                                                                    | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv TextureCube.SampleBias(sampler, float3 coord, float bias)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplebias)                        |          |
| MSL     | `Tv texturecube.sample(sampler s, float3 coord, bias(float))`                                                                                                               |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - Cube Texture - Level

| Backend | Function                                                                                                                                                                  | Comments                                          |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| HLSL    | [`Tv TextureCube.SampleLevel(sampler, float3 coord, float LOD)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplelevel)                     |                                                   |
| MSL     | `Tv texturecube.sample(sampler s, float3 coord, level(float))`                                                                                                            | Not entirely sure if this exists - spec is vague. |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |                                                   |

#### Sample - Cube Texture - Gradient

| Backend | Function                                                                                                                                                                       | Comments |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv TextureCube.SampleGrad(sampler, float3 coord, float3 ddx, float3 ddy)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplegrad)               |          |
| MSL     | `Tv texturecube.sample(sampler s, float3 coord, gradientcube(float3 dx, float3 dy))`                                                                                           |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

### Sample - Cube Texture Array

#### Sample - Cube Texture Array - Basic

| Backend | Function                                                                                                                                                        | Comments                                                                                                                                                              |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Tv TextureCubeArray.Sample(sampler, float4 coord)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-sample)                      | Array index in `coord.w`.                                                                                                                                             |
| MSL     | `Tv texturecube_array.sample(sampler s, float3 coord, uint array)`                                                                                              |                                                                                                                                                                       |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[3]`.<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Sample - Cube Texture Array - Bias

| Backend | Function                                                                                                                                                                    | Comments                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Tv TextureCubeArray.SampleBias(sampler, float4 coord, float bias)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-samplebias)              | Array index in `coord.w`.  |
| MSL     | `Tv texturecube_array.sample(sampler s, float3 coord, uint array, bias(float))`                                                                                             |                            |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[3]`. |

#### Sample - Cube Texture Array - Level

| Backend | Function                                                                                                                                                                  | Comments                                          |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| HLSL    | [`Tv TextureCubeArray.SampleLevel(sampler, float4 coord, float LOD)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-samplelevel)           | Array index in `coord.w`.                         |
| MSL     | `Tv texturecube_array.sample(sampler s, float3 coord, uint array, level(float))`                                                                                          | Not entirely sure if this exists - spec is vague. |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[3]`.                        |

#### Sample - Cube Texture Array - Gradient

| Backend | Function                                                                                                                                                                       | Comments                                                                                                                                                              |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Tv TextureCubeArray.SampleGrad(sampler, float4 coord, float3 ddx, float3 ddy)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-samplegrad)     | Array index in `coord.w`.                                                                                                                                             |
| MSL     | `Tv texturecube_array.sample(sampler s, float3 coord, uint array, gradientcube(float3 dx, float3 dy))`                                                                         |                                                                                                                                                                       |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[3]`.<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Sample - 2D Depth Texture

#### Sample - 2D Depth Texture - Basic

| Backend | Function                                                                                                                                                        | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T Texture2D.Sample(sampler, float2 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture-sample-overload)               |          |
| MSL     | `T depth2d.sample(sampler s, float2 coord[, int2 offset])`                                                                                                      |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - 2D Depth Texture - Bias

| Backend | Function                                                                                                                                                                    | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T Texture2D.SampleBias(sampler, float2 coord, float bias[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplebias)              |          |
| MSL     | `T depth2d.sample(sampler s, float2 coord, bias(float)[, int2 offset])`                                                                                                     |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - 2D Depth Texture - Level

| Backend | Function                                                                                                                                                                  | Comments |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T Texture2D.SampleLevel(sampler, float2 coord, float LOD[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplelevel)           |          |
| MSL     | `T depth2d.sample(sampler s, float2 coord, level(float)[, int2 offset])`                                                                                                  |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

#### Sample - 2D Depth Texture - Gradient

| Backend | Function                                                                                                                                                                       | Comments |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T Texture2D.SampleGrad(sampler, float2 coord, float2 ddx, float2 ddy[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplegrad)     |          |
| MSL     | `T depth2d.sample(sampler s, float2 coord, gradient2d(float2 dx, float2 dy)[, int2 offset])`                                                                                   |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

### Sample - 2D Depth Texture Array

#### Sample - 2D Depth Texture Array - Basic

| Backend | Function                                                                                                                                                        | Comments                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T Texture2DArray.Sample(sampler, float3 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-sample)            | Array index in `coord.z`.  |
| MSL     | `T depth2d_array.sample(sampler s, float2 coord, uint array[, int2 offset])`                                                                                    |                            |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[2]`. |

#### Sample - 2D Depth Texture Array - Bias

| Backend | Function                                                                                                                                                                    | Comments                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T Texture2DArray.SampleBias(sampler, float3 coord, float bias[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplebias)    | Array index in `coord.z`.  |
| MSL     | `T depth2d_array.sample(sampler s, float2 coord, uint array, bias(float)[, int2 offset])`                                                                                   |                            |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[2]`. |

#### Sample - 2D Depth Texture Array - Level

| Backend | Function                                                                                                                                                                  | Comments                   |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T Texture2DArray.SampleLevel(sampler, float3 coord, float LOD[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplelevel) | Array index in `coord.z`.  |
| MSL     | `T depth2d_array.sample(sampler s, float2 coord, uint array, level(float)[, int2 offset])`                                                                                |                            |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) | Array index in `coord[2]`. |

#### Sample - 2D Depth Texture Array - Gradient

| Backend | Function                                                                                                                                                                             | Comments                   |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T Texture2DArray.SampleGrad(sampler, float3 coord, float2 ddx, float2 ddy[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplegrad) | Array index in `coord.z`.  |
| MSL     | `T depth2d_array.sample(sampler s, float2 coord, uint array, gradient2d(float2 dx, float2 dy)[, int2 offset])`                                                                       |                            |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod)       | Array index in `coord[2]`. |

### Sample - Cube Depth Texture

#### Sample - Cube Depth Texture - Basic

| Backend | Function                                                                                                                                                                    | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T TextureCube.Sample(sampler, float3 coord)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-sample)                                             |          |
| MSL     | `T depthcube.sample(sampler s, float3 coord)`                                                                                                                               |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - Cube Depth Texture - Bias

| Backend | Function                                                                                                                                                                    | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T TextureCube.SampleBias(sampler, float3 coord, float bias)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplebias)                         |          |
| MSL     | `T depthcube.sample(sampler s, float3 coord, bias(float))`                                                                                                                  |          |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) |          |

#### Sample - Cube Depth Texture - Level

| Backend | Function                                                                                                                                                                  | Comments |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T TextureCube.SampleLevel(sampler, float3 coord, float LOD)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplelevel)                      |          |
| MSL     | `T depthcube.sample(sampler s, float3 coord, level(float))`                                                                                                               |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

#### Sample - Cube Depth Texture - Gradient

| Backend | Function                                                                                                                                                                       | Comments |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T TextureCube.SampleGrad(sampler, float3 coord, float3 ddx, float3 ddy)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplegrad)                |          |
| MSL     | `T depthcube.sample(sampler s, float3 coord, gradientcube(float3 dx, float3 dy))`                                                                                              |          |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleExplicitLod) |          |

### Sample - Cube Depth Texture Array

#### Sample - Cube Depth Texture Array - Basic

| Backend | Function                                                                                                                                                        | Comments                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T TextureCubeArray.Sample(sampler, float4 coord)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-sample)                       | Array index in `coord.w`.  |
| MSL     | `T depthcube_array.sample(sampler s, float3 coord, uint array)`                                                                                                 |                            |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[3]`. |

#### Sample - Cube Depth Texture Array - Bias

| Backend | Function                                                                                                                                                                    | Comments                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T TextureCubeArray.SampleBias(sampler, float4 coord, float bias)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-samplebias)               | Array index in `coord.w`.  |
| MSL     | `T depthcube_array.sample(sampler s, float3 coord, uint array, bias(float))`                                                                                                |                            |
| SPIR-V  | [`OpImageSampleImplicitLod <sampled-image> <coord> Bias <bias> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[3]`. |

#### Sample - Cube Depth Texture Array - Level

| Backend | Function                                                                                                                                                                  | Comments                   |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T TextureCubeArray.SampleLevel(sampler, float4 coord, float LOD)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-samplelevel)            | Array index in `coord.w`.  |
| MSL     | `T depthcube_array.sample(sampler s, float3 coord, uint array, level(float))`                                                                                             |                            |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[3]`. |

#### Sample - Cube Depth Texture Array - Gradient

| Backend | Function                                                                                                                                                                       | Comments                   |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T TextureCubeArray.SampleGrad(sampler, float4 coord, float3 ddx, float3 ddy)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-samplegrad)      | Array index in `coord.w`.  |
| MSL     | `T depthcube_array.sample(sampler s, float3 coord, uint array, gradientcube(float3 dx, float3 dy))`                                                                            |                            |
| SPIR-V  | [`OpImageSampleExplicitLod <sampled-image> <coord> Grad <dx> <dy> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleImplicitLod) | Array index in `coord[3]`. |

## Sampling & Compare functions

> Metal Shading Language: `sample_compare` performs a comparison of the
`compare_value` value against the pixel value (`1.0` if the comparison passes
and `0.0` if it fails). These comparison result values per-pixel are then
blended together as in normal texture filtering and the resulting value between
`0.0` and `1.0` is returned.

> Metal Shading Language, 6.10 Texture Functions:
On macOS, for `sample_compare` functions, `bias` and `gradient*` are not supported, and `lod`
must be a zero constant.

### Sample & Compare - 2D Depth Texture

#### Sample & Compare - 2D Depth Texture - Basic

| Backend | Function                                                                                                                                                                                | Comments                  |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| HLSL    | [`T Texture2D.SampleCmp(SamplerComparisonState, float2 coord, float compare_value[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplecmp)    |                           |
| MSL     | `T depth2d.sample_compare(sampler s, float2 coord, float compare_value[, int2 offset])`                                                                                                 | `T` must be a float type. |
| SPIR-V  | [`OpImageSampleDrefImplicitLod <sampled-image> <coord> <compare_value> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleDrefImplicitLod) |                           |

#### Sample & Compare - 2D Depth Texture - Level

| Backend | Function                                                                                                                                                                                          | Comments                                                                                                                                                                                |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`T Texture2D.SampleCmp(SamplerComparisonState, float2 coord, float compare_value[, int2 offset], float clamp)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplecmp) | `clamp` is used as a `max()` on the mip level selected.<br>See also: [`SampleCmpLevelZero()`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-samplecmplevelzero) |
| MSL     | `T depth2d.sample_compare(sampler s, float2 coord, float compare_value, level(float lod)[, int2 offset])`                                                                                         | `T` must be a float type.<br>`lod` **must** be a zero constant on macOS.                                                                                                                |
| SPIR-V  | [`OpImageSampleDrefExplicitLod <sampled-image> <coord> <compare_value> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleDrefExplicitLod) |                                                                                                                                                                                         |

### Sample & Compare - 2D Depth Texture Array

#### Sample & Compare - 2D Depth Texture Array - Basic

| Backend | Function                                                                                                                                                                                       | Comments                   |
|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T Texture2DArray.SampleCmp(SamplerComparisonState, float3 coord, float compare_value[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplecmp) | Array index in `coord.z`.  |
| MSL     | `T depth2d_array.sample_compare(sampler s, float2 coord, uint array, float compare_value[, int2 offset])`                                                                                      | `T` must be a float type.  |
| SPIR-V  | [`OpImageSampleDrefImplicitLod <sampled-image> <coord> <compare_value> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleDrefImplicitLod)        | Array index in `coord[2]`. |

#### Sample & Compare - 2D Depth Texture Array - Level

| Backend | Function                                                                                                                                                                                                    | Comments                                                                                                                                                                                                                  |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`T Texture2DArray.SampleCmp(SamplerComparisonState, float3 coord, float compare_value[, int2 offset], float clamp)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplecmp) | Array index in `coord.z`.<br>`clamp` is used as a `max()` on the mip level selected.<br>See also: [`SampleCmpLevelZero()`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-samplecmplevelzero) |
| MSL     | `T depth2d_array.sample_compare(sampler s, float2 coord, uint array, float compare_value, level(float lod)[, int2 offset])`                                                                                 | `T` must be a float type.<br>`lod` **must** be a zero constant on macOS.                                                                                                                                                  |
| SPIR-V  | [`OpImageSampleDrefExplicitLod <sampled-image> <coord> <compare_value> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleDrefExplicitLod)           | Array index in `coord[2]`.                                                                                                                                                                                                |

### Sample & Compare - Cube Depth Texture

#### Sample & Compare - Cube Depth Texture - Basic

| Backend | Function                                                                                                                                                                                | Comments                  |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------|
| HLSL    | [`T TextureCube.SampleCmp(SamplerComparisonState, float3 coord, float compare_value)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplecmp)               |                           |
| MSL     | `T depthcube.sample_compare(sampler s, float3 coord, float compare_value)`                                                                                                              | `T` must be a float type. |
| SPIR-V  | [`OpImageSampleDrefImplicitLod <sampled-image> <coord> <compare_value> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleDrefImplicitLod) |                           |

#### Sample & Compare - Cube Depth Texture - Level

| Backend | Function                                                                                                                                                                                          | Comments                                                                                                                                                                                  |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`T TextureCube.SampleCmp(SamplerComparisonState, float3 coord, float compare_value, float clamp)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplecmp)            | `clamp` is used as a `max()` on the mip level selected.<br>See also: [`SampleCmpLevelZero()`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplecmplevelzero) |
| MSL     | `T depthcube.sample_compare(sampler s, float3 coord, float compare_value, level(float lod))`                                                                                                      | `T` must be a float type.<br>`lod` **must** be a zero constant on macOS.                                                                                                                  |
| SPIR-V  | [`OpImageSampleDrefExplicitLod <sampled-image> <coord> <compare_value> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleDrefExplicitLod) |                                                                                                                                                                                           |

### Sample & Compare - Cube Depth Texture Array

#### Sample & Compare - Cube Depth Texture Array - Basic

| Backend | Function                                                                                                                                                                                | Comments                   |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T TextureCubeArray.SampleCmp(SamplerComparisonState, float4 coord, float compare_value)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-samplecmp)     | Array index in `coord.w`.  |
| MSL     | `T depthcube_array.sample_compare(sampler s, float3 coord, uint array, float compare_value)`                                                                                            | `T` must be a float type.  |
| SPIR-V  | [`OpImageSampleDrefImplicitLod <sampled-image> <coord> <compare_value> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleDrefImplicitLod) | Array index in `coord[3]`. |

#### Sample & Compare - Cube Depth Texture Array - Level

| Backend | Function                                                                                                                                                                                          | Comments                                                                                                                                                                                                               |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`T TextureCubeArray.SampleCmp(SamplerComparisonState, float4 coord, float compare_value, float clamp)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-samplecmp)  | Array index in `coord.w`.<br>`clamp` is used as a `max()` on the mip level selected.<br>See also: [`SampleCmpLevelZero()`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-samplecmplevelzero) |
| MSL     | `T depthcube_array.sample_compare(sampler s, float3 coord, uint array, float compare_value, level(float lod))`                                                                                    | `T` must be a float type.<br>`lod` **must** be a zero constant on macOS.                                                                                                                                               |
| SPIR-V  | [`OpImageSampleDrefExplicitLod <sampled-image> <coord> <compare_value> Lod <lod> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageSampleDrefExplicitLod) | Array index in `coord[3]`.                                                                                                                                                                                             |

## Read

SPIR-V:  Use ImageFetch for sampled images, and ImageRead for storage images.

Issue: The listing below does not distinguish between sampled textures and storage textures. For now it only lists the sampled texture variants.

### Read - 1D Texture

| Backend | Function                                                                                                                      | Comments                                                                                                                         |
|---------|-------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Tv Texture1D.Load(int2 coord[, int offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture1d-load)   | Array index in `coord.y`.                                                                                                        |
| MSL     | `Tv texture1d.read(uint coord, uint lod = 0)`                                                                                 | Mipmaps are not supported for 1D textures.<br>`lod` **must** be `0`.                                                             |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch) | Requires the [`Image1D` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Read - 1D Texture Array

| Backend | Function                                                                                                                              | Comments                                                                                                                                                       |
|---------|---------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Tv Texture1DArray.Load(int3 coord[, int offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture1darray-load) | Array index in `coord.y`.                                                                                                                                      |
| MSL     | `Tv texture1d_array.read(uint coord, uint array, uint lod = 0)`                                                                       | Mipmaps are not supported for 1D textures.<br>`lod` **must** be `0`.                                                                                           |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch)         | Array index in `coord[1]`.<br>Requires the [`Image1D` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Read - 2D Texture

| Backend | Function                                                                                                                      | Comments |
|---------|-------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture2D.Load(int2 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-load)  |          |
| MSL     | `Tv texture2d.read(uint2 coord, uint lod = 0)`                                                                                |          |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch) |          |

### Read - 2D Texture Array

| Backend | Function                                                                                                                               | Comments                   |
|---------|----------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Tv Texture2DArray.Load(int3 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-load) | Array index in `coord.z`.  |
| MSL     | `Tv texture2d_array.read(uint2 coord, ushort array, uint lod = 0)`                                                                     |                            |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch)          | Array index in `coord[2]`. |

### Read - 3D Texture

| Backend | Function                                                                                                                      | Comments |
|---------|-------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Tv Texture3D.Load(int4 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture3d-load)  |          |
| MSL     | `Tv texture3d.read(uint3 coord, uint lod = 0)`                                                                                |          |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch) |          |

### Read - Cube Texture

| Backend | Function                                                                                                                      | Comments                                            |
|---------|-------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| HLSL    | \<not-supported\>                                                                                                             |                                                     |
| MSL     | `Tv texturecube.read(uint3 coord, uint face, uint lod = 0)`                                                                   | `face: [0: +X, 1: -X, 2: +Y, 3: -Y, 4: +Z, 5: -Z]`. |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch) |                                                     |

### Read - Cube Texture Array

| Backend | Function                                                                                                                      | Comments                                                                                                                                                              |
|---------|-------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | \<not-supported\>                                                                                                             |                                                                                                                                                                       |
| MSL     | `Tv texturecube_array.read(uint3 coord, uint face, uint array, uint lod = 0)`                                                 | `face: [0: +X, 1: -X, 2: +Y, 3: -Y, 4: +Z, 5: -Z]`.                                                                                                                   |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch) | Array index in `coord[4]`.<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Read - 2D Multisampled Texture

| Backend | Function                                                                                                                                   | Comments                                                                   |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| HLSL    | [`Tv Texture2DMS.Load(int3 coord, int sample[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-load) |                                                                            |
| MSL     | `Tv texture2d_ms.read(uint2 coord, uint sample)`                                                                                           | `MTLRenderPassDescriptor.setSamplePositions` can specify sample positions. |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch)              |                                                                            |

### Read - 2D Depth Texture

| Backend | Function                                                                                                                      | Comments |
|---------|-------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T Texture2D.Load(int3 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-load)   |          |
| MSL     | `T depth2d.read(uint2 coord, uint lod = 0)`                                                                                   |          |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch) | Depth images are sampled images, so Sampeld=1, implying ImageFetch instead of ImageRead |

### Read - 2D Depth Texture Array

| Backend | Function                                                                                                                              | Comments                   |
|---------|---------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`T Texture2DArray.Load(int3 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-load) | Array index in `coord.z`.  |
| MSL     | `T depth2d_array.read(uint2 coord, uint array, uint lod = 0)`                                                                         |                            |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch)         | Array index in `coord[3]`. |

### Read - 2D Multisampled Depth Texture

| Backend | Function                                                                                                                                  | Comments |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`T Texture2DMS.Load(int3 coord, int sample[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-load) |          |
| MSL     | `T depth2d_ms.read(uint2 coord, uint sample)`                                                                                             |          |
| SPIR-V  | [`OpImageFetch <image> <coord> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageFetch)             |          |

### Read - Cube Depth Texture

| Backend | Function                                                 | Comments       |
|---------|----------------------------------------------------------|----------------|
| HLSL    | \<not-supported\>                                        |                |
| MSL     | `T depthcube.read(uint3 coord, uint face, uint lod = 0)` |                |
| SPIR-V  | \<not-supported\>                                        | TODO: Confirm. |

### Read - Cube Depth Texture Array

| Backend | Function                                                                   | Comments       |
|---------|----------------------------------------------------------------------------|----------------|
| HLSL    | \<not-supported\>                                                          |                |
| MSL     | `T depthcube_array.read(uint3 coord, uint face, uint array, uint lod = 0)` |                |
| SPIR-V  | \<not-supported\>                                                          | TODO: Confirm. |

## Write

HLSL texture writes require using `RWTexture`s - [note they do not support the same methods as regular `Texture`s](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-rwtexture1d):
> A `RWTexture1D` object cannot use methods from a `Texture1D` object, such as Sample.
However, because you can create multiple view types to the same resource, you
can declare multiple texture types as a single texture in multiple shaders.
For example, you can declare and use a RWTexture1D object as tex in a compute
shader and then declare and use a Texture1D object as tex in a pixel shader.

`RWTexture`s do not support mip maps.

### Write - 1D Texture

| Backend | Function                                                                                                                                | Comments                                                                                                                         |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`RWTexture1D.operator[int] = color`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-rwtexture1d-operatorindex) |                                                                                                                                  |
| MSL     | `void texture1d.write(Tv color, uint coord, uint lod = 0)`                                                                              | Mipmaps are not supported for 1D textures.<br>`lod` **must** be `0`.                                                             |
| SPIR-V  | [`OpImageWrite <image> <coord> <texel> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageWrite) | Requires the [`Image1D` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Write - 1D Texture Array

| Backend | Function                                                                                                                                           | Comments                                                                                                                                                       |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`RWTexture1DArray.operator[int2] = color`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-rwtexture1darray-operatorindex) | Array index in index operand `y`.                                                                                                                              |
| MSL     | `void texture1d_array.write(Tv color, uint coord, uint lod = 0)`                                                                                   | Mipmaps are not supported for 1D textures.<br>`lod` **must** be `0`.                                                                                           |
| SPIR-V  | [`OpImageWrite <image> <coord> <texel> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageWrite)            | Array index in `coord[1]`.<br>Requires the [`Image1D` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Write - 2D Texture

| Backend | Function                                                                                                                                | Comments                        |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| HLSL    | [`RWTexture2D.operator[int2] = color`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-rwtexture2d)              |                                 |
| MSL     | `void texture2d.write(Tv color, uint2 coord, uint lod = 0)`                                                                             | `lod` **must** be `0` on macOS. |
| SPIR-V  | [`OpImageWrite <image> <coord> <texel> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageWrite) |                                 |

### Write - 2D Texture Array

| Backend | Function                                                                                                                                | Comments                          |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| HLSL    | [`RWTexture2DArray.operator[int3] = color`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-rwtexture2darray)    | Array index in index operand `z`. |
| MSL     | `void texture2d_array.write(Tv color, uint2 coord, uint array, uint lod = 0)`                                                           | `lod` **must** be `0` on macOS.   |
| SPIR-V  | [`OpImageWrite <image> <coord> <texel> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageWrite) | Array index in `coord[2]`.        |

### Write - 3D Texture

| Backend | Function                                                                                                                                 | Comments                        |
|---------|------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| HLSL    | [`RWTexture3D.operator[int3] = color`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-rwtexture3d-operatorindex) |                                 |
| MSL     | `void texture3d.write(Tv color, uint3 coord, uint lod = 0)`                                                                              | `lod` **must** be `0` on macOS. |
| SPIR-V  | [`OpImageWrite <image> <coord> <texel> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageWrite)  |                                 |

### Write - Cube Texture

| Backend | Function                                                                 | Comments                                                                               |
|---------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| HLSL    | \<not-supported\>                                                        |                                                                                        |
| MSL     | `void texturecube.write(Tv color, uint2 coord, uint face, uint lod = 0)` | `lod` **must** be `0` on macOS.<br>`face: [0: +X, 1: -X, 2: +Y, 3: -Y, 4: +Z, 5: -Z]`. |
| SPIR-V  | \<not-supported\>                                                        | TODO: Confirm.                                                                         |


### Write - Cube Texture Array

| Backend | Function                                                                                   | Comments                                                                               |
|---------|--------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| HLSL    | \<not-supported\>                                                                          |                                                                                        |
| MSL     | `void texturecube_array.write(Tv color, uint2 coord, uint face, uint array, uint lod = 0)` | `lod` **must** be `0` on macOS.<br>`face: [0: +X, 1: -X, 2: +Y, 3: -Y, 4: +Z, 5: -Z]`. |
| SPIR-V  | \<not-supported\>                                                                          | TODO: Confirm.                                                                         |

## Gather

A gather returns 4 samples of a specified channel (r,g,b,a) for bilinear interpolation.

### Gather - 2D Texture

| Backend | Function                                                                                                                                                            | Comments |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Texture2D.Gather[Red,Green,Blue,Alpha](sampler s, float2 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-gatherred) |          |
| MSL     | `Tv texture2d.gather(sampler s, float2 coord[, int2 offset], component c = component::x)`                                                                           |          |
| SPIR-V  | [`OpImageGather <sampled-image> <coord> <component> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageGather)               |          |

### Gather - 2D Texture Array

| Backend | Function                                                                                                                                                                      | Comments                   |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Texture2DArray.Gather[Red,Green,Blue,Alpha](sampler s, float3 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-gatherred) | Array index in `coord.z`.  |
| MSL     | `Tv texture2d_array.gather(sampler s, float2 coord, uint array[, int2 offset], component c = component::x)`                                                                   |                            |
| SPIR-V  | [`OpImageGather <sampled-image> <coord> <component> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageGather)                         | Array index in `coord[2]`. |

### Gather - Cube Texture

| Backend | Function                                                                                                                                                 | Comments |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`TextureCube.Gather[Red,Green,Blue,Alpha](sampler s, float3 coord)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-gatherred) |          |
| MSL     | `Tv texturecube.gather(sampler s, float3 coord, component c = component::x)`                                                                             |          |
| SPIR-V  | [`OpImageGather <sampled-image> <coord> <component> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageGather)    |          |

### Gather - Cube Texture Array

| Backend | Function                                                                                                                                                           | Comments                   |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`TextureCubeArray.Gather[Red,Green,Blue,Alpha](sampler s, float4 coord)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecubearray-gatherred) | Array index in `coord.w`.  |
| MSL     | `Tv texturecube_array.gather(sampler s, float3 coord, uint array, component c = component::x)`                                                                     |                            |
| SPIR-V  | [`OpImageGather <sampled-image> <coord> <component> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageGather)              | Array index in `coord[3]`. |

### Gather - 2D Depth Texture

| Backend | Function                                                                                                                                              | Comments |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Texture2D.Gather(sampler s, float2 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-gather)            |          |
| MSL     | `Tv depth2d.gather(sampler s, float2 coord[, int2 offset])`                                                                                           |          |
| SPIR-V  | [`OpImageGather <sampled-image> <coord> <component> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageGather) |          |

### Gather - 2D Depth Texture Array

| Backend | Function                                                                                                                                              | Comments                   |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Texture2DArray.Gather(sampler s, float3 coord[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-gather)  | Array index in `coord.z`.  |
| MSL     | `Tv depth2d_array.gather(sampler s, float2 coord, uint array[, int2 offset])`                                                                         |                            |
| SPIR-V  | [`OpImageGather <sampled-image> <coord> <component> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageGather) | Array index in `coord[2]`. |

### Gather - Cube Depth Texture

| Backend | Function                                                                                                                                              | Comments |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`TextureCube.Gather(sampler s, float3 coord)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-gather)                       |          |
| MSL     | `Tv depthcube.gather(sampler s, float3 coord)`                                                                                                        |          |
| SPIR-V  | [`OpImageGather <sampled-image> <coord> <component> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageGather) |          |

### Gather - Cube Depth Texture Array

| Backend | Function                                                                                                                                              | Comments                   |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`TextureCube.Gather(sampler s, float4 coord)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-gather)                       | Array index in `coord.w`.  |
| MSL     | `Tv depthcube_array.gather(sampler s, float3 coord, uint array)`                                                                                      |                            |
| SPIR-V  | [`OpImageGather <sampled-image> <coord> <component> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageGather) | Array index in `coord[3]`. |

## Gather Compare

### Gather Compare - 2D Depth Texture

| Backend | Function                                                                                                                                                              | Comments |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`Texture2D.GatherCmp(sampler s, float2 coord, float compare_value[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2d-gathercmp) |          |
| MSL     | `Tv depth2d.gather_compare(sampler s, float2 coord, float compare_value[, int2 offset])`                                                                              |          |
| SPIR-V  | [`OpImageDrefGather <sampled-image> <coord> <compare_value> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageDrefGather)     |          |

### Gather Compare - 2D Depth Texture Array

| Backend | Function                                                                                                                                                                        | Comments                   |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`Texture2DArray.GatherCmp(sampler s, float3 coord, float compare_value[, int2 offset])`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texture2darray-gathercmp) | Array index in `coord.z`.  |
| MSL     | `Tv depth2d_array.gather_compare(sampler s, float2 coord, uint array, float compare_value[, int2 offset])`                                                                      |                            |
| SPIR-V  | [`OpImageDrefGather <sampled-image> <coord> <compare_value> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageDrefGather)               | Array index in `coord[2]`. |

### Gather Compare - Cube Depth Texture

| Backend | Function                                                                                                                                                          | Comments |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| HLSL    | [`TextureCube.GatherCmp(sampler s, float3 coord, float compare_value)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-gathercmp)        |          |
| MSL     | `Tv depthcube.gather_compare(sampler s, float3 coord, float compare_value)`                                                                                       |          |
| SPIR-V  | [`OpImageDrefGather <sampled-image> <coord> <compare_value> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageDrefGather) |          |

### Gather Compare - Cube Depth Texture Array

| Backend | Function                                                                                                                                                          | Comments                   |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|
| HLSL    | [`TextureCubeArray.GatherCmp(sampler s, float4 coord, float compare_value)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/texturecube-gathercmp)   | Array index in `coord.w`.  |
| MSL     | `Tv depthcube_array.gather_compare(sampler s, float3 coord, uint array, float compare_value)`                                                                     |                            |
| SPIR-V  | [`OpImageDrefGather <sampled-image> <coord> <compare_value> [image-operands...]`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageDrefGather) | Array index in `coord[3]`. |

## Query

### Query - 1D Texture

#### Query - 1D Texture - Width

| Backend | Function                                                                                                                                                                    | Comments                                                                                                                            |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture1D.GetDimensions(in uint mip, out uint width, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture1d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture1d.get_width(uint lod = 0)`                                                                                                                                    | Mipmaps are not supported for 1D textures.<br>`lod` **must** be `0`.                                                                |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                     | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 1D Texture - Number of mips

| Backend | Function                                                                                                                                                                    | Comments                                                                                                                            |
|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture1D.GetDimensions(in uint mip, out uint width, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture1d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture1d.get_num_mip_levels()`                                                                                                                                       | Mipmaps are not supported for 1D textures<br>Always returns `0`.                                                                    |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                            | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - 1D Texture Array

#### Query - 1D Texture Array - Width

| Backend | Function                                                                                                                                                                                                 | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture1DArray.GetDimensions(in uint mip, out uint width, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture1darray-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture1d_array.get_width(uint lod = 0)`                                                                                                                                                           | Mipmaps are not supported for 1D textures.<br>`lod` **must** be `0`.                                                                |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                  | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 1D Texture Array - Number of array elements

| Backend | Function                                                                                                                                                                                                                               | Comments                                                                                                                                                                                              |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture1DArray.GetDimensions(in uint mip, out uint width, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture1darray-getdimensions)                               |                                                                                                                                                                                                       |
| MSL     | `uint texture1d_array.get_array_size()`                                                                                                                                                                                                |                                                                                                                                                                                                       |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)<br>[`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize) | Number of array elements in last component of returned vector.<br>Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 1D Texture Array - Number of mips

| Backend | Function                                                                                                                                                                                                 | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture1DArray.GetDimensions(in uint mip, out uint width, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture1darray-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture1d_array.get_num_mip_levels()`                                                                                                                                                              | Mipmaps are not supported for 1D textures<br>Always returns `0`.                                                                    |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                         | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - 2D Texture

#### Query - 2D Texture - Width

| Backend | Function                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2D.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d.get_width(uint lod = 0)`                                                                                                                                                     |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                      | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Texture - Height

| Backend | Function                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2D.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d.get_height(uint lod = 0)`                                                                                                                                                    |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                      | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Texture - Number of mips

| Backend | Function                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2D.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d.get_num_mip_levels()`                                                                                                                                                        |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                             | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - 2D Texture Array

#### Query - 2D Texture Array - Width

| Backend | Function                                                                                                                                                                                                                  | Comments                                                                                                                            |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d_array.get_width(uint lod = 0)`                                                                                                                                                                            |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                                   | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Texture Array - Height

| Backend | Function                                                                                                                                                                                                                  | Comments                                                                                                                            |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d_array.get_height(uint lod = 0)`                                                                                                                                                                           |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                                   | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Texture Array - Number of array elements

| Backend | Function                                                                                                                                                                                                                               | Comments                                                                                                                                                                                              |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray-getdimensions)              |                                                                                                                                                                                                       |
| MSL     | `uint texture2d_array.get_array_size()`                                                                                                                                                                                                |                                                                                                                                                                                                       |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)<br>[`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize) | Number of array elements in last component of returned vector.<br>Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Texture Array - Number of mips

| Backend | Function                                                                                                                                                                                                                  | Comments                                                                                                                            |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d_array.get_num_mip_levels()`                                                                                                                                                                               |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                                          | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - 3D Texture

#### Query - 3D Texture - Width

| Backend | Function                                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture3D.GetDimensions(in uint mip, out uint width, out uint height, out uint depth, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture3d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture3d.get_width(uint lod = 0)`                                                                                                                                                                     |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                      | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 3D Texture - Height

| Backend | Function                                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture3D.GetDimensions(in uint mip, out uint width, out uint height, out uint depth, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture3d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture3d.get_height(uint lod = 0)`                                                                                                                                                                    |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                      | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 3D Texture - Depth

| Backend | Function                                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture3D.GetDimensions(in uint mip, out uint width, out uint height, out uint depth, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture3d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture3d.get_depth(uint lod = 0)`                                                                                                                                                                     |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                      | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 3D Texture - Number of mips

| Backend | Function                                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture3D.GetDimensions(in uint mip, out uint width, out uint height, out uint depth, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture3d-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture3d.get_num_mip_levels()`                                                                                                                                                                        |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                             | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - Cube Texture

#### Query - Cube Texture - Width

| Backend | Function                                                                                                                | Comments                                                                                                                            |
|---------|-------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCube.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`                            | Missing from documentation, but DXC accepts it.                                                                                     |
| MSL     | `uint texturecube.get_width(uint lod = 0)`                                                                              |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Texture - Height

| Backend | Function                                                                                                                | Comments                                                                                                                            |
|---------|-------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCube.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`                            | Missing from documentation, but DXC accepts it.                                                                                     |
| MSL     | `uint texturecube.get_height(uint lod = 0)`                                                                             |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Texture - Number of mips

| Backend | Function                                                                                                         | Comments                                                                                                                            |
|---------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCube.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`                     | Missing from documentation, but DXC accepts it.                                                                                     |
| MSL     | `uint texturecube.get_num_mip_levels()`                                                                          |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - Cube Texture Array

#### Query - Cube Texture Array - Width

| Backend | Function                                                                                                                | Comments                                                                                                                                                                                                                                                                       |
|---------|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCubeArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`    | Missing from documentation, but DXC accepts it.                                                                                                                                                                                                                                |
| MSL     | `uint texturecube_array.get_width(uint lod = 0)`                                                                        |                                                                                                                                                                                                                                                                                |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability).<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Texture Array - Height

| Backend | Function                                                                                                                | Comments                                                                                                                                                                                                                                                                       |
|---------|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCubeArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`    | Missing from documentation, but DXC accepts it.                                                                                                                                                                                                                                |
| MSL     | `uint texturecube_array.get_height(uint lod = 0)`                                                                       |                                                                                                                                                                                                                                                                                |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability).<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Texture Array - Number of array elements

| Backend | Function                                                                                                                                                                                                                               | Comments                                                                                                                                                                                                                                                                                                                                         |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCubeArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`                                                                                                                   | Missing from documentation, but DXC accepts it.                                                                                                                                                                                                                                                                                                  |
| MSL     | `uint texturecube_array.get_array_size()`                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                                                                  |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)<br>[`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize) | Number of array elements in last component of returned vector.<br>Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability).<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Texture Array - Number of mips

| Backend | Function                                                                                                             | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCubeArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)` | Missing from documentation, but DXC accepts it.                                                                                     |
| MSL     | `uint texturecube_array.get_num_mip_levels()`                                                                        |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)     | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - 2D Multisampled Texture

#### Query - 2D Multisampled Texture - Width

| Backend | Function                                                                                                                                                                               | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DMS.GetDimensions(out uint width, out uint height, out uint num_samples)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2dms-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d_ms.get_width(uint lod = 0)`                                                                                                                                            |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize)                                                                            | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Multisampled Texture - Height

| Backend | Function                                                                                                                                                                               | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DMS.GetDimensions(out uint width, out uint height, out uint num_samples)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2dms-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d_ms.get_height(uint lod = 0)`                                                                                                                                           |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize)                                                                            | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Multisampled Texture - Number of samples

| Backend | Function                                                                                                                                                                               | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DMS.GetDimensions(out uint width, out uint height, out uint num_samples)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2dms-getdimensions) |                                                                                                                                     |
| MSL     | `uint texture2d_ms.get_num_samples()`                                                                                                                                                  |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySamples <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize)                                                                         | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - 2D Depth Texture

#### Query - 2D Depth Texture - Width

| Backend | Function                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2D.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d.get_width(uint lod = 0)`                                                                                                                                                       |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                      | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Depth Texture - Height

| Backend | Function                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2D.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d.get_height(uint lod = 0)`                                                                                                                                                      |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                      | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Depth Texture - Number of mips

| Backend | Function                                                                                                                                                                                     | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2D.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d.get_num_mip_levels()`                                                                                                                                                          |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                             | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - 2D Depth Texture Array

#### Query - 2D Depth Texture Array - Width

| Backend | Function                                                                                                                                                                                                                  | Comments                                                                                                                            |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d_array.get_width(uint lod = 0)`                                                                                                                                                                              |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                                   | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Depth Texture Array - Height

| Backend | Function                                                                                                                                                                                                                  | Comments                                                                                                                            |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d_array.get_height(uint lod = 0)`                                                                                                                                                                             |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                                   | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Depth Texture Array - Number of array elements

| Backend | Function                                                                                                                                                                                                                               | Comments                                                                                                                                                                                              |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray-getdimensions)              |                                                                                                                                                                                                       |
| MSL     | `uint depth2d_array.get_array_size()`                                                                                                                                                                                                  |                                                                                                                                                                                                       |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)<br>[`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize) | Number of array elements in last component of returned vector.<br>Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Depth Texture Array - Number of mips

| Backend | Function                                                                                                                                                                                                                  | Comments                                                                                                                            |
|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2darray-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d_array.get_num_mip_levels()`                                                                                                                                                                                 |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)                                                                                                          | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - 2D Multisampled Depth Texture

#### Query - 2D Multisampled Depth Texture - Width

| Backend | Function                                                                                                                                                                               | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DMS.GetDimensions(out uint width, out uint height, out uint num_samples)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2dms-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d_ms.get_width(uint lod = 0)`                                                                                                                                              |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize)                                                                            | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Multisampled Depth Texture - Height

| Backend | Function                                                                                                                                                                               | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DMS.GetDimensions(out uint width, out uint height, out uint num_samples)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2dms-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d_ms.get_height(uint lod = 0)`                                                                                                                                             |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize)                                                                            | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - 2D Multisampled Depth Texture - Number of samples

| Backend | Function                                                                                                                                                                               | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | [`Texture2DMS.GetDimensions(out uint width, out uint height, out uint num_samples)`](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2dms-getdimensions) |                                                                                                                                     |
| MSL     | `uint depth2d_ms.get_num_samples()`                                                                                                                                                    |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySamples <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize)                                                                         | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - Cube Depth Texture

#### Query - Cube Depth Texture - Width

| Backend | Function                                                                                                                | Comments                                                                                                                            |
|---------|-------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCube.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`                            | Missing from documentation, but DXC accepts it.                                                                                     |
| MSL     | `uint depthcube.get_width(uint lod = 0)`                                                                                |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Depth Texture - Height

| Backend | Function                                                                                                                | Comments                                                                                                                            |
|---------|-------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCube.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`                            | Missing from documentation, but DXC accepts it.                                                                                     |
| MSL     | `uint depthcube.get_height(uint lod = 0)`                                                                               |                                                                                                                                     |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Depth Texture - Number of mips

| Backend | Function                                                                                                         | Comments                                                                                                                            |
|---------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCube.GetDimensions(in uint mip, out uint width, out uint height, out uint num_mips)`                     | Missing from documentation, but DXC accepts it.                                                                                     |
| MSL     | `uint depthcube.get_num_mip_levels()`                                                                            |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

### Query - Cube Depth Texture Array

#### Query - Cube Depth Texture Array - Width

| Backend | Function                                                                                                                | Comments                                                                                                                                                                                                                                                                       |
|---------|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCubeArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`    | Missing from documentation, but DXC accepts it.                                                                                                                                                                                                                                |
| MSL     | `uint depthcube_array.get_width(uint lod = 0)`                                                                          |                                                                                                                                                                                                                                                                                |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability).<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Depth Texture Array - Height

| Backend | Function                                                                                                                | Comments                                                                                                                                                                                                                                                                       |
|---------|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCubeArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`    | Missing from documentation, but DXC accepts it.                                                                                                                                                                                                                                |
| MSL     | `uint depthcube_array.get_height(uint lod = 0)`                                                                         |                                                                                                                                                                                                                                                                                |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod) | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability).<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Depth Texture Array - Number of array elements

| Backend | Function                                                                                                                                                                                                                               | Comments                                                                                                                                                                                                                                                                                                                                         |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCubeArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)`                                                                                                                   | Missing from documentation, but DXC accepts it.                                                                                                                                                                                                                                                                                                  |
| MSL     | `uint depthcube_array.get_array_size()`                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                  |
| SPIR-V  | [`OpImageQuerySizeLod <image> <lod>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)<br>[`OpImageQuerySize <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySize) | Number of array elements in last component of returned vector.<br>Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability).<br>Requires the [`ImageCubeArray` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |

#### Query - Cube Depth Texture Array - Number of mips

| Backend | Function                                                                                                             | Comments                                                                                                                            |
|---------|----------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| HLSL    | `TextureCubeArray.GetDimensions(in uint mip, out uint width, out uint height, out uint elements, out uint num_mips)` | Missing from documentation, but DXC accepts it.                                                                                     |
| MSL     | `uint depthcube_array.get_num_mip_levels()`                                                                          |                                                                                                                                     |
| SPIR-V  | [`OpImageQueryLevels <image>`](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#OpImageQuerySizeLod)     | Requires the [`ImageQuery` capability](https://www.khronos.org/registry/spir-v/specs/1.0/SPIRV.html#_a_id_capability_a_capability). |
