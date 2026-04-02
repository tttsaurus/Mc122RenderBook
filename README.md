This repo aims to serve as a tutorial for Minecraft 1.12.2 rendering, _**but**_
you can also expect general OpenGL, LWJGL, engine design ideas to be explained.

> **Note**: You are free to copy any examples from this repo, but follow MIT license still.

## Intro To GL States
- [Font Renderer & Basic GL Calls](articles/state/fontrenderer_basic_gl_calls.md)
- [Common GL States](articles/state/gl_states.md)
- [Shape & State Experiments](articles/state/shapes_n_states.md)

## Rotation
- [Rotation & Quaternion](articles/rotation/rotation_n_quaternion.md)

## Alpha & Blending
- [Alpha Testing Explained](articles/alpha_blend/alpha_test.md)
- [Blending Explained](articles/alpha_blend/blend.md)

## Stencil
- [Stencil Experiments](articles/stencil/stencil.md)

***

> You are now expected to be pretty experienced with HUD/UI rendering after
> "_Intro To GL States_", "_Rotation_", "_Alpha & Blending_", "_Stencil_".<br>
> Time to test/experiment with some HUD/UI rendering thingy!

> Continue reading when you are confident with UI drawing and even fancy animations.

***

From now on, you can expect deeper and more _"serious"_ GL contents.

- [Drawing A 3D Object Via The Fixed-Func Pipeline](articles/common/fixed_func_3d_shapes.md)

The major issue with the fixed-func pipeline is not only how it's "fixed-func'd" but also how the pipeline is
non-scalable and fragile. A better alternative is the modern programmable pipeline (generally considered as GL30+),
where streamlining data and rendering is made possible by `VAO`, `Shader`, etc.

A programmable pipeline is not essentially needed because it's _faster_. Don't be lured by micro-optimizations.
We all care more about the structural wise modularity and scalability.

- [Drawing Tris Using Vertices and Indices Via The Modern Pipeline](articles/prog_pipe/draw_vertices.md)
- [Shader Loading & Shader Program](articles/prog_pipe/shaders.md)
- [VAO & Vertex Format](articles/prog_pipe/vao_n_vertex_format.md)
- [Common Buffer Objects](articles/prog_pipe/buffer_objects.md)
- [Typical Shader Setup](articles/prog_pipe/typical_shader_setup.md)

## NIO ByteBuffer
- [NIO ByteBuffer Tips](articles/bytebuffer/nio_bytebuffer.md)

## GL States Related Concerns
Here are some major [Concerns](articles/state/concerns.md)

## GL Execution Model
1. [Command Data Flow](articles/gl_basics/command_data_flow.md)
2. [Command Order](articles/gl_basics/order.md)
3. [Common Synchronizations](articles/gl_basics/synchronization.md)

> **Key point:**
> Now you've understood that OpenGL isn't merely about the correct command combinations;
> "Nothing" is necessarily guaranteed by CPU side method calls, and things work not necessarily
> because you've done the correct thing.

## Intro To RAW/WAR Hazards
> They are read-after-write and write-after-read hazards
- [Common Hazards](articles/hazard/raw_war.md)

***

> You are now expected to be _OK_ with shaders, draw calls, execution model, etc.
> It's recommended to design and implement your own pipeline that streamlines _something_ with
> the techniques you learned.

***

## Unit Test
It's recommended to set up an OpenGL test environment first so you'll be able to verify
any kinds of ideas, edge cases, etc.<br>
[JUnit OpenGL Extension](articles/unit_test/junit_gl_extension.md)

## Nsight Graphics
Minecraft 1.12.2 runs on the compat profile and Nsight works with the compat profile. You'll be able to
use RenderDoc only if an emulation (emulate fixed-func pipeline with modern programmable pipeline) layer is done,
but still... emulation emulates.<br>
- [Example: Nsight CLI + Gradle Task Integration](https://github.com/CleanroomMC/Cleanroom/blob/fdcfb19fc2fcb8d98e2738654f3d9f8d047c76c4/projects/cleanroom/build.gradle#L885-L928)
- [Nsight CLI Details](https://docs.nvidia.com/nsight-graphics/UserGuide/launch-application-overview.html#cli-arguments-details)


