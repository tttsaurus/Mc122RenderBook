## EG1
Draw a triangle using vertices and indices (requires GL30 support)
```java
int prevVao = GL11.glGetInteger(GL30.GL_VERTEX_ARRAY_BINDING);
int prevVbo = GL11.glGetInteger(GL15.GL_ARRAY_BUFFER_BINDING);
int prevEbo = GL11.glGetInteger(GL15.GL_ELEMENT_ARRAY_BUFFER_BINDING);

int vao;
int vbo;
int ebo;

float[] vertices = new float[]
{
      // positions         // texcoords   // normals
      -0.5f, -0.5f, 0.0f,  0.0f, 0.0f,    0.0f, 0.0f, 1.0f,  // bottom-left
      0.5f, -0.5f, 0.0f,   1.0f, 0.0f,    0.0f, 0.0f, 1.0f,  // bottom-right
      0.0f, 0.5f, 0.0f,    0.5f, 1.0f,    0.0f, 0.0f, 1.0f   // top
};

int[] indices = new int[]
{
      // triangle: bottom-left, bottom-right, top
      0, 1, 2
};

vao = GL30.glGenVertexArrays();
GL30.glBindVertexArray(vao);

FloatBuffer vertexBuffer = ByteBuffer.allocateDirect(vertices.length * Float.BYTES)
        .order(ByteOrder.nativeOrder()).asFloatBuffer();
vertexBuffer.put(vertices).flip();

IntBuffer indexBuffer = ByteBuffer.allocateDirect(indices.length * Integer.BYTES)
        .order(ByteOrder.nativeOrder()).asIntBuffer();
indexBuffer.put(indices).flip();

vbo = GL15.glGenBuffers();
GL15.glBindBuffer(GL15.GL_ARRAY_BUFFER, vbo);
GL15.glBufferData(GL15.GL_ARRAY_BUFFER, vertexBuffer, GL15.GL_STATIC_DRAW);

ebo = GL15.glGenBuffers();
GL15.glBindBuffer(GL15.GL_ELEMENT_ARRAY_BUFFER, ebo);
GL15.glBufferData(GL15.GL_ELEMENT_ARRAY_BUFFER, indexBuffer, GL15.GL_STATIC_DRAW);

// first 3 floats for position
GL20.glVertexAttribPointer(0, 3, GL11.GL_FLOAT, false, 8 * Float.BYTES, 0);
GL20.glEnableVertexAttribArray(0);

// next 2 floats for texCoord
GL20.glVertexAttribPointer(1, 2, GL11.GL_FLOAT, false, 8 * Float.BYTES, 3 * Float.BYTES);
GL20.glEnableVertexAttribArray(1);

// last 3 floats for normal
GL20.glVertexAttribPointer(2, 3, GL11.GL_FLOAT, false, 8 * Float.BYTES, 5 * Float.BYTES);
GL20.glEnableVertexAttribArray(2);

// trying to be safe so we unbind
GL30.glBindVertexArray(prevVao);
GL15.glBindBuffer(GL15.GL_ARRAY_BUFFER, prevVbo);
GL15.glBindBuffer(GL15.GL_ELEMENT_ARRAY_BUFFER, prevEbo);

// render part
useShader(); // we don't care what shaders are for now

GL30.glBindVertexArray(vao);
GL15.glBindBuffer(GL15.GL_ELEMENT_ARRAY_BUFFER, ebo);
GL11.glDrawElements(GL11.GL_TRIANGLES, indices.length, GL11.GL_UNSIGNED_INT, 0);
GL30.glBindVertexArray(0);

unuseShader(); // we don't care what shaders are for now
```
![Snipaste_2025-02-02_14-42-38](https://github.com/user-attachments/assets/28186c70-3288-4ebd-ab18-e5959ffb28f8)

In this case, we use 8 floats per vertex: `postion` (`layout (location = 0)`), `texcoord` (`layout (location = 1)`), and `normal` (`layout (location = 2)`).
While this might seem redundant, it's actually quite useful for handling complex models. Of course, you're free to define your own format but just make sure your shaders follow the same format.

## EG2
Draw two triangles using instancing
```java
int prevVao = GL11.glGetInteger(GL30.GL_VERTEX_ARRAY_BINDING);
int prevVbo = GL11.glGetInteger(GL15.GL_ARRAY_BUFFER_BINDING);
int prevEbo = GL11.glGetInteger(GL15.GL_ELEMENT_ARRAY_BUFFER_BINDING);

int vao;
int vbo;
int ebo;

int instanceVbo;

float[] vertices = new float[]
{
        // positions         // texcoords   // normals
        -0.5f, -0.5f, 0.0f,  0.0f, 0.0f,    0.0f, 0.0f, 1.0f,  // bottom-left
        0.5f, -0.5f, 0.0f,   1.0f, 0.0f,    0.0f, 0.0f, 1.0f,  // bottom-right
        0.0f, 0.5f, 0.0f,    0.5f, 1.0f,    0.0f, 0.0f, 1.0f   // top
};

int[] indices = new int[]
{
        // triangle: bottom-left, bottom-right, top
        0, 1, 2
};

float[] instanceOffsets = new float[]
{
        -0.1f, 0.0f, 0.0f,  // first instance: shift left
        0.1f, 0.0f, 0.0f    // second instance: shift right
};

vao = GL30.glGenVertexArrays();
GL30.glBindVertexArray(vao);

FloatBuffer vertexBuffer = ByteBuffer.allocateDirect(vertices.length * Float.BYTES)
        .order(ByteOrder.nativeOrder()).asFloatBuffer();
vertexBuffer.put(vertices).flip();

IntBuffer indexBuffer = ByteBuffer.allocateDirect(indices.length * Integer.BYTES)
        .order(ByteOrder.nativeOrder()).asIntBuffer();
indexBuffer.put(indices).flip();

vbo = GL15.glGenBuffers();
GL15.glBindBuffer(GL15.GL_ARRAY_BUFFER, vbo);
GL15.glBufferData(GL15.GL_ARRAY_BUFFER, vertexBuffer, GL15.GL_STATIC_DRAW);

ebo = GL15.glGenBuffers();
GL15.glBindBuffer(GL15.GL_ELEMENT_ARRAY_BUFFER, ebo);
GL15.glBufferData(GL15.GL_ELEMENT_ARRAY_BUFFER, indexBuffer, GL15.GL_STATIC_DRAW);

// first 3 floats for position
GL20.glVertexAttribPointer(0, 3, GL11.GL_FLOAT, false, 8 * Float.BYTES, 0);
GL20.glEnableVertexAttribArray(0);

// next 2 floats for texCoord
GL20.glVertexAttribPointer(1, 2, GL11.GL_FLOAT, false, 8 * Float.BYTES, 3 * Float.BYTES);
GL20.glEnableVertexAttribArray(1);

// last 3 floats for normal
GL20.glVertexAttribPointer(2, 3, GL11.GL_FLOAT, false, 8 * Float.BYTES, 5 * Float.BYTES);
GL20.glEnableVertexAttribArray(2);

// pass instancing data
instanceVbo = GL15.glGenBuffers();
GL15.glBindBuffer(GL15.GL_ARRAY_BUFFER, instanceVbo);
FloatBuffer instanceBuffer = ByteBuffer.allocateDirect(instanceOffsets.length * Float.BYTES)
        .order(ByteOrder.nativeOrder()).asFloatBuffer();
instanceBuffer.put(instanceOffsets).flip();
GL15.glBufferData(GL15.GL_ARRAY_BUFFER, instanceBuffer, GL15.GL_STATIC_DRAW);

// another 3 floats for instancing data
GL20.glVertexAttribPointer(3, 3, GL11.GL_FLOAT, false, 3 * Float.BYTES, 0);
GL20.glEnableVertexAttribArray(3);
GL33.glVertexAttribDivisor(3, 1);

// trying to be safe so we unbind
GL30.glBindVertexArray(prevVao);
GL15.glBindBuffer(GL15.GL_ARRAY_BUFFER, prevVbo);
GL15.glBindBuffer(GL15.GL_ELEMENT_ARRAY_BUFFER, prevEbo);

// render part
useShader(); // we don't care what shaders are for now

GL30.glBindVertexArray(vao);
GL15.glBindBuffer(GL15.GL_ELEMENT_ARRAY_BUFFER, ebo);

//GL11.glDrawElements(GL11.GL_TRIANGLES, indices.length, GL11.GL_UNSIGNED_INT, 0);
GL31.glDrawElementsInstanced(GL11.GL_TRIANGLES, indices.length, GL11.GL_UNSIGNED_INT, 0, 2);

GL30.glBindVertexArray(0);

unuseShader(); // we don't care what shaders are for now
```
![Snipaste_2025-02-21_18-52-07](https://github.com/user-attachments/assets/534919a6-0a3a-4ddc-b6af-314871758864)

Instance offset (`layout (location = 3)`) is being handled explicitly in vertex shader. By the way, we use instancing to reduce draw calls as `glDrawElementsInstanced` is the only draw call.
