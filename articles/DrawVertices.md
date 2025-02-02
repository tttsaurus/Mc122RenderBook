## EG1
Draw a triangle using vertices and indices
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
