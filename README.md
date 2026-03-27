This repo aims to serve as a tutorial of Minecraft 1.12.2 rendering, _**but**_
you can also expect general OpenGL, LWJGL, Java ideas to be explained.

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

> You are expected to be pretty experienced with HUD/UI rendering after
> "_Intro To GL States_", "_Rotation_", "_Alpha & Blending_", "_Stencil_".<br>
> Time to test/experiment with some HUD/UI rendering thingy!

> Continue reading when you are confident with UI drawing and even fancy animations.

***

From now on, you can expect deeper and more _"serious"_ GL contents.

// todo: fixed-func 3d obj -> shader + vao -> programmable 3d obj -> common buffers

[//]: # (- [An Example of a 3D Object]&#40;articles/3DShapes.md&#41;)
[//]: # (- [Shader Loading & Shader Program]&#40;articles/ShaderLoading&ShaderProgram.md&#41;)
[//]: # (- [VAO & Vertex Attribute Pointer]&#40;articles/VaoAndVertexAttributePointer.md&#41;)
[//]: # (- [Draw Triangles Using Vertices and Indices]&#40;articles/DrawVertices.md&#41;)
[//]: # (- [Common Buffer Objects]&#40;articles/BufferObjects.md&#41;)

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

// todo: introduce "special" commands and RAW/WAR conditions

## NIO ByteBuffer
- [NIO ByteBuffer Tips](articles/bytebuffer/nio_bytebuffer.md)

// todo: allocate direct vs lwjgl allocate

[//]: # (- [Fixed-Function -> Programmable Pipeline]&#40;articles/ProgrammablePipeline.md&#41;)

[//]: # (- [Intro To GLSL]&#40;articles/IntroToGLSL.md&#41; WIP)

[//]: # (- [Clip Space, View Space, etc]&#40;articles/AllThatAboutViewClipEtc.md&#41; WIP)

[//]: # (- [Parsing Obj Model]&#40;articles/ParsingObjModel.md&#41; WIP)

[//]: # (- [Framebuffer]&#40;articles/Framebuffer.md&#41; WIP)
