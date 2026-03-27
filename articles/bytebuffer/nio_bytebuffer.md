When it comes to OpenGL with Java, we use NIO `ByteBuffer` for off-heap memory a lot.

Here are some tips:
- `limit - position = remaining`. OpenGL functions (LWJGL ones) read from the **current position** up to **limit**, i.e., the `remaining()` bytes.
- So, set `position = offset`, `limit = byteCount + offset` before passing to GL. 
  Be cautious when using views — `intBuffer.limit()` is in _ints_, not _bytes_!
- Use **direct buffers** (`ByteBuffer.allocateDirect`) for OpenGL. Only direct buffers are compatible with native GPU memory access.
- Never allocate byte buffers in hotpaths (slow!)
- Therefore, only **allocate once**, typically in an init/setup method. Reusing buffers reduces GC pressure and avoids repeated native allocations.
- Don't forget to set order to `nativeOrder` (aka CPU endianness)
- `ByteBuffer.asFloatBuffer()`/`ByteBuffer.<any other interpretation>` points to the same underlying memory, but returns a new wrapper.
  These views have their **own** position/limit/capacity measured in **elements** (e.g., ints), not bytes. 
  Adding contents to alternative views doesn't grow the original `ByteBuffer` (i.e. grow `pos` etc.)
- `ByteBuffer.duplicate()` doesn't copy endianness but forces big endian. Don't forget to set order again.
