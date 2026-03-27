
Fetching GL states is essentially implicitly stalling the pipeline.
That is why `GL11.glGetInteger` causes overhead.

Some people follow a `enable/disable` pattern (not necessarily intended to avoid pipeline stalls; 
they most likely aren't aware of it), while some people follow a `change/restore` pattern, causing
stalls but much safer, correct, and predictable.

Instead of these two patterns, I'd like to introduce a possibility:
- We emulate a sense of state immutability with an underlying `enable/disable` pattern
- No `GL11.glGetInteger` is therefore needed

Say, we split a frame into `n` phases.
```
| Phase 1 -----| Phase 2 -----| Phase 3 -----| ...
```
And:
- For phase 1: `blend`=`true`, `depth`=`false`, ... (those states should be considered as immutable; 
  no one is allowed to change anything during phase _i_)
- For phase 2: ...
- For phase 3: ...

As a result, we avoided a performance bottleneck and achieved the pipeline predictability and correctness, _**but**_
there is no way to strictly guarantee that "no one changes anything during phase _i_"

> I hope it explains why mixin'ing render code is generally a bad idea

Nevertheless, the dilemma is:
- There is no well established "immutable pipeline state" system for Minecraft
- Restoring states is _good_ but also bad
- Following the `enable/disable` pattern is a compromise (could be a lack of understanding too)

It ultimately comes down to personal preferences and trade-offs.
However, aligning with conventions established by existing rendering ecosystems - 
even less mature or lesser-known ones - helps avoid the GL state dilemma.
