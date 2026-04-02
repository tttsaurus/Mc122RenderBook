
## WAR Hazard From GPU's POV

Consider the scenario
```java
bufferData(data);
draw();
```

This is generally considered as safe, and we've talked about it in "_GL Execution Model_".

```java
bufferData(data1);
draw();
bufferData(data2);
draw();
```

However, this is less elegant and more fragile. `Draw 1` may be running (or not even started) while `bufferData(data2)` triggers.
Passing data to a slice of memory that is _currently_/_to be_ accessed by a draw command is known as a write-after-read hazard.

Nevertheless, drivers will most likely fix this issue for you (drivers guarantee correctness but not performance). Still:
- Driver behavior is implicit and unpredictable
- You may experience different behaviors on different platforms/drivers
- You may experience fluctuations

Most importantly, the consequence of a WAR hazard is that read action will be waited by write action, breaking parallelism.
Or in short:
- Stalling the pipeline
- Slowing down

### Orphaning
One strategy is to orphan the old underlying storage that is "locked" by the draw command and allocate
a new underlying storage, but we still use the same buffer object namely (GL/CPU side).

```java
bufferData(data1);
draw();
bufferData(null);
bufferSubData(data2);
draw();
```

`bufferData(null)` will most likely trigger orphaning, but it's not guaranteed. 
Orphaning is widely used but still relies on driver heuristics.

**Level of predictability**:
- No guard : LOW
- Orphaning : ABOVE AVERAGE ~ HIGH

> **Notice**:<br>
> An intuitive way to visualize WAR hazard is to imagine a lock.
> ```java
> draw() { lock.tryLock(); }
> ```
> ```java
> bufferData() { lock.tryLock(); }
> ```
> But `draw` right after `upload` is totally fine, causes no stall, and is recommended.
> Even though `upload` is not an immediate action too.<br>
> A lock is not 100% accurate here since the actual reason behind the stall is related
> to the GPU command queue.

### Buffering
Another strategy is to have multiple buffers explicitly, so we don't have to rely on
so-called underlying behaviors.

```java
b1.bufferData(data1);
draw();
b2.bufferData(data2);
draw();
```

Having two buffer objects totally fix the hazard for a two-draw-call scenario, but we don't 
want _n_ buffers for _n_ draw calls anyway.

```java
b1.bufferData(data1);
draw();
f1 = fence();

b2.bufferData(data2);
draw();
f2 = fence();

when f1 finishes:
  reuse b1

when f2 finishes:
  reuse b2
```

Let's consider the worst case first, during the next update, both `f1`, `f2` aren't finished, so we
got nowhere to upload our `data3`, resulting in a small latency.
To be more specific, this is the capacity of double buffering.

If fences finish in a regular way, you can then expect a `Draw b1 - Upload b2 - Draw b2 - Upload b1` pattern.

Moreover, as you can see, the pseudocode can't really demonstrate the situation since
we must introduce timing/lifecycle as a new dimension.

## RAW Hazard From GPU's POV
Consider the scenario
```java
compute();
drawUsingComputeResult();
```

This is a typical RAW hazard setup since compute is an async process.
The way to fix it is using fences and buffering like we mentioned above.
