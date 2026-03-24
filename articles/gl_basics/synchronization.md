## 1. `glMemoryBarrier`
It ensures memory writes performed by earlier GPU commands become visible to later GPU commands.

e.g.
```java
glDispatchCompute();
glMemoryBarrier(GL_SHADER_STORAGE_BARRIER_BIT);
glDrawArrays(...); 
```

```java
glDispatchCompute();
glMemoryBarrier(GL_SHADER_STORAGE_BARRIER_BIT);
glDispatchCompute(); 
```

The barrier guarantees memory visibility but not the execution completion,
so future GPU commands _must_ see your memory writes.

`glMemoryBarrier` doesn't:
- Wait for GPU to finish
- Sync CPU and GPU
- Block CPU
- Guarantee completion

You don't need barrier for:
- CPU2GPU writes (like `glBufferSubData`)
- GPU2CPU reads (use fences instead)

## 2. `glFenceSync`

e.g.
```java
glDispatchCompute();
long fence = glFenceSync(GL_SYNC_GPU_COMMANDS_COMPLETE, 0);
```

Treat it like "let me know when the GPU reaches this point."
What you are doing is like pulling the GPU status.

A fence doesn't:
- Block CPU / wait

A fence is only about the completion but not visibility.

## 3. `glClientWaitSync`
It allows the CPU to wait until the GPU reaches a fence.

e.g.
```java
result = glClientWaitSync(
    fence,
    GL_SYNC_FLUSH_COMMANDS_BIT,
    timeout
);
```

A wait sync may:
- Block CPU
- Return immediately
- Flush pending GPU commands

e.g. usage
```java
glDispatchCompute();
fence = glFenceSync(...);

glClientWaitSync(fence, GL_SYNC_FLUSH_COMMANDS_BIT, timeout);
// you might need a barrier here; depends on the context
glGetBufferSubData(...); // now safe; read result
```

## 4. `glFinish`
This is a global and heavyweight synchronization point.

It blocks CPU, waits for everything, destroys parallelism, etc.

e.g.
```java
// don't use in production
```

## 5. `glFlush`
It ensures that previously issued commands are submitted to the GPU.

`glFlush` doesn't:
- Block CPU / wait
- Guarantee completion
- Guarantee visibility

***

"I didn't do any of those but the program runs correctly and everything works fine"

Because drivers are written carefully to provide fallback solutions, which may cause performance issues and subtle bugs.

> **Note**:<br>
> GL synchronization is multi-dimensional.
> No single command covers all correctness requirements.

***

## Examples

**Context**:
- Compute shader writes to an image
- CPU reads the image afterward

```java
GL43.glDispatchCompute(1, 1, 1);
GL42.glMemoryBarrier(GL42.GL_SHADER_IMAGE_ACCESS_BARRIER_BIT);

long fence = GL32C.glFenceSync(GL32C.GL_SYNC_GPU_COMMANDS_COMPLETE, 0);
int waitReturn = GL32C.glClientWaitSync(fence, GL32.GL_SYNC_FLUSH_COMMANDS_BIT, 1_000_000L);
           
// get image
// notice: get image will stall the pipeline, which makes wait sync unnecessary here,
//         but it's still better to not rely on implicit side effects
```
