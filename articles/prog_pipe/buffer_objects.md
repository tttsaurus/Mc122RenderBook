There are many kinds of OpenGL buffer objects. Here's an intro.

| **Buffer**                       | **GL Constant**            | 
|----------------------------------|----------------------------|
| **Vertex Buffer Object (VBO)**   | `GL_ARRAY_BUFFER`          | 
| **Element Buffer Object (EBO)**  | `GL_ELEMENT_ARRAY_BUFFER`  | 
| **Pixel Pack Buffer (PBO)**      | `GL_PIXEL_PACK_BUFFER`     | 
| **Pixel Unpack Buffer (PBO)**    | `GL_PIXEL_UNPACK_BUFFER`   | 
| **Shader Storage Buffer (SSBO)** | `GL_SHADER_STORAGE_BUFFER` | 

## Typical Pairing
- VBO + EBO + VAO → Core rendering pipeline (vertices and indices)
- PBOs → Efficient texture or framebuffer data transfer
- SSBO → General GPU compute tasks and advanced rendering workflows

By reading through [Drawing Tris Using Vertices and Indices Via The Modern Pipeline](draw_vertices.md), 
you should already be pretty familiar with a typical VAO setup.

## Common Usage Patterns

OpenGL buffer objects are generic containers that store data in GPU memory. Their usage generally follows this lifecycle:

> **Notice**:<br>
> Buffers do not have an intrinsic or fixed type in GL.
> A buffer object is merely interpreted according to the binding
> target it is associated with after a `bind` call.
> However, buffers still don't gain a fixed type after `bind` but you should respect
> its first binding target type in most cases.
> You're still free to interpret a buffer in different types.

---

### Generate a Buffer ID

Each buffer object is identified by an unsigned integer.

```java
int id = GL15.glGenBuffers();
```

---

### Bind the Buffer to a Target

Before operating on the buffer, bind it to a specific target (e.g., vertex data, pixel unpacking).

```java
GL15.glBindBuffer(targetConstant, id);

// example: bind a PBO
GL15.glBindBuffer(GL21.GL_PIXEL_UNPACK_BUFFER, id);
```

To unbind (disable) the buffer:

```java
GL15.glBindBuffer(targetConstant, 0);
```

---

### Query Buffer Parameters

To retrieve metadata about the buffer:

```java
int result = GL15.glGetBufferParameteri(targetConstant, pname);

// example: get buffer size
int size = GL15.glGetBufferParameteri(GL21.GL_PIXEL_UNPACK_BUFFER, GL15.GL_BUFFER_SIZE);
```

---

### Query the Current Binding

To find which buffer is currently bound to a given target:

```java
int boundId = GL11.glGetInteger(bindingQuery);

// example: check bound PBO
int currentId = GL11.glGetInteger(GL21.GL_PIXEL_UNPACK_BUFFER_BINDING);
```

For each buffer target, there's a corresponding `*_BINDING` constant, e.g. `GL_ARRAY_BUFFER_BINDING`, `GL_SHADER_STORAGE_BUFFER_BINDING`.

---

### Allocate GPU Memory (Without Initial Data)

Allocate memory without uploading data:

```java
GL15.glBufferData(target, sizeInBytes, usageHint);

// example: reserve space for one 32-bit integer (4 bytes)
GL15.glBufferData(GL21.GL_PIXEL_UNPACK_BUFFER, 4, GL15.GL_STREAM_DRAW);
```

Usage hints:
- `GL_STATIC_DRAW`: rarely updated, frequently used
- `GL_DYNAMIC_DRAW`: frequently updated, frequently used
- `GL_STREAM_DRAW`: updated every frame or very frequently

---

### Upload Data to GPU (During Allocation)

To allocate and upload data in one call:

```java
GL15.glBufferData(target, byteBuffer, usageHint);

// example
ByteBuffer byteBuffer = ByteBuffer
    .allocateDirect(arr.length * Integer.BYTES)
    .order(ByteOrder.nativeOrder()); // critical for correct layout

IntBuffer intView = byteBuffer.asIntBuffer();
intView.put(arr);

byteBuffer.limit(intView.limit() * Integer.BYTES);
byteBuffer.position(0);

GL15.glBufferData(GL21.GL_PIXEL_UNPACK_BUFFER, byteBuffer, GL15.GL_STATIC_DRAW);
```

Use `ByteOrder.nativeOrder()` for proper layout when working with ints, floats, etc.

---

### Update a Subsection of the Buffer (Sub-Data)

Modify a portion of the buffer contents without reallocating:

```java
GL15.glBufferSubData(target, offsetInBytes, byteBuffer);

// example:
// initial buffer content: [4, 0, 0, 0]
// byteBuffer content: [1, 2, 3]
GL15.glBufferSubData(GL21.GL_PIXEL_UNPACK_BUFFER, 4, byteBuffer);
// resulting buffer: [4, 1, 2, 3]
```
