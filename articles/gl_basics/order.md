## What "Order" Actually Means

Consider command flow `A --> B`.

Even if commands are executed in order, you can still get wrong results.

Because order alone doesn't guarantee:
- That `A` has finished
- That `A`'s memory writes are visible
- That `A`'s results are usable by `B`

You can expect three different categories:
- Execution order
- Completion
- Memory visibility

and GL only guarantees the first one by default.

**Quick mental model check**:
- Code snippet:
  ```java
  GL.uploadData();
  GL.draw(); // based on the data we just uploaded
  ```
  Is it safe?
  > Yes.
  > This is a CPU-to-GPU memory copy which is implicitly protected by GL.
  > The data is visible to the draw command.
  > <br><br>**Note**:<br>
  > To illustrate,
  > CPU data is gathered at the `return` stage of `GL.uploadData()` but GPU side data copy completes later.
  > The draw command is guaranteed to run when everything is ready; the draw action, however, 
  > doesn't _wait_ for the data to upload, because a draw call isn't merely about the data and isn't
  > even an action completed in one shot.
