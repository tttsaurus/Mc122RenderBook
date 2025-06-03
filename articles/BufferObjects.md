There are many kinds of OpenGL buffer objects. Here's an intro.

| **Buffer**                       | **GL Constant**            | **Main Purpose**                                                        | **Key Use Cases**                                 | **Special Features**                                              |
| -------------------------------- | -------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------- | ----------------------------------------------------------------- |
| **Vertex Buffer Object (VBO)**   | `GL_ARRAY_BUFFER`          | Stores vertex data: positions, UVs, normals, colors, etc.               | Used in rendering geometry                        | Used with `glVertexAttribPointer()` to feed shader inputs         |
| **Element Buffer Object (EBO)**  | `GL_ELEMENT_ARRAY_BUFFER`  | Stores vertex indices for indexed drawing (`glDrawElements`)            | Efficient rendering with shared vertices          | Bound to VAO; avoids redundant vertices                           |
| **Pixel Pack Buffer (PBO)**      | `GL_PIXEL_PACK_BUFFER`     | Reads pixel data from GPU (e.g. `glReadPixels`)                         | Async readback, screenshots, GPGPU read results   | CPU doesn't stall waiting for GPU to finish                       |
| **Pixel Unpack Buffer (PBO)**    | `GL_PIXEL_UNPACK_BUFFER`   | Uploads texture/image data to GPU (e.g. `glTexImage2D`)                 | Streaming textures, async uploads                 | Can be used with streaming video or live texture updates          |
| **Shader Storage Buffer (SSBO)** | `GL_SHADER_STORAGE_BUFFER` | General-purpose large data buffers accessible in shaders (read & write) | GPGPU tasks, particle systems, deferred rendering | Can hold large structured data; needs `layout(std430)` in shaders |

## Typical Pairing
- VBO + EBO (+ VAO) → Core rendering pipeline (vertices and indices).
- PBOs → Efficient texture or framebuffer data transfer.
- SSBO → General GPU compute tasks and advanced rendering workflows.

By reading through [Draw Triangles Using Vertices and Indices](https://github.com/tttsaurus/Mc122RenderBook/blob/main/articles/DrawVertices.md), you should already be pretty familiar with VAO + VBO + EBO.

## Common Usage Patterns

OpenGL buffer objects are generic containers that store data in GPU memory. Their usage generally follows this lifecycle:

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
